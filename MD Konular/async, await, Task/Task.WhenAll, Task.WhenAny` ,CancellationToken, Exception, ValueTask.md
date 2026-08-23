# Async / Await İleri Konular

## 1. `Task.WhenAll` ve `Task.WhenAny` — birden fazla işi aynı anda yürütmek

Diyelim ki 3 tane SNMP simülatörünüz var ve hepsini aynı anda başlatmak istiyorsunuz. Yanlış (ama çok yaygın) yaklaşım şudur:

```csharp
await sim1.StartAsync();
await sim2.StartAsync();
await sim3.StartAsync();
```

Bu kod çalışır ama **sıralı** çalışır: `sim1` bitmeden `sim2` başlamaz. Eğer her biri 1 saniye sürüyorsa, toplamda 3 saniye beklersiniz — oysa hepsi birbirinden bağımsızsa aynı anda başlayıp aynı anda bitebilirlerdi.

Doğrusu:

```csharp
Task t1 = sim1.StartAsync();
Task t2 = sim2.StartAsync();
Task t3 = sim3.StartAsync();

await Task.WhenAll(t1, t2, t3);
```

Burada üç iş de hemen art arda başlatılır (henüz `await` edilmediği için hiçbiri sizi bloklamaz), sonra `Task.WhenAll` ile "hepsi bitene kadar bekle" denir. Toplam süre, en yavaş olanın süresi kadardır — 3 saniye değil, yaklaşık 1 saniye.

`Task.WhenAny` ise farklı bir amaç için: "hangisi önce biterse bitsin, ilk biteni bana ver." Örneğin birden fazla kaynaktan veri isteyip ilk cevap veren kaynağı kullanmak istediğinizde kullanılır.

Sizin projenizde birden fazla simülatör varsa (birden fazla port dinliyor gibi görünüyor), `StartAsync`/`StopAsync` çağrılarını tek tek `await` yerine `Task.WhenAll` ile toplu yapmak ciddi bir performans farkı yaratır.

## 2. `CancellationToken` — asenkron işi dışarıdan durdurabilmek

Bir asenkron işlem başladığında, bazen o işin bitmesini beklemeden "vazgeç, dur" demeniz gerekir. Örneğin `StopAsync()` metodu `_snmpCore.StopListen()` diye senkron bir durdurma yapıyor ama eğer bir dinleme döngüsü asenkron çalışıyorsa (`await` ile sürekli veri bekliyorsa), o döngüyü "artık durabilirsin" diye bilgilendirmenin standart yolu `CancellationToken`'dır.

```csharp
public async Task ListenAsync(CancellationToken cancellationToken)
{
    while (!cancellationToken.IsCancellationRequested)
    {
        var veri = await SoketOkuAsync(cancellationToken);
        // veriyi işle
    }
}
```

Dışarıdan durdurmak isteyen kod:

```csharp
CancellationTokenSource cts = new CancellationTokenSource();
Task dinlemeIsi = ListenAsync(cts.Token);

// ... bir süre sonra durdurmak istediğinizde:
cts.Cancel();
```

`cts.Cancel()` çağrıldığında, `cancellationToken.IsCancellationRequested` `true` olur ve döngü bir sonraki kontrolde kendini durdurur. Bazı metotlar (örneğin ağ okuma metotları) token'ı doğrudan kabul eder ve bekleme sırasında bile iptali fark edip anında bir `OperationCanceledException` fırlatabilir — böylece "sonsuza kadar bekleme" riskine karşı gerçek bir çıkış yolu olur.

Bu senaryoda bu, özellikle "dinleme sürekli açık, ama kullanıcı durdur dediğinde beklemeden hemen kapanmalı" durumları için önemlidir.

## 3. Exception (hata) davranışı — hata nerede yakalanır?

Normal senkron kodda, bir metot hata fırlattığı an, çağıran kod bunu hemen `try/catch` ile yakalar. Asenkron kodda ise durum biraz farklıdır: `async` bir metotta hata fırlarsa, bu hata hemen fırlamaz, `Task`'ın içine "saklanır." Hata ancak siz o `Task`'ı `await` ettiğinizde gerçekten fırlar.

```csharp
public async Task<string> VeriOkuAsync()
{
    await Task.Delay(1000);
    throw new InvalidOperationException("Bağlantı koptu");
}

public async Task CalistirAsync()
{
    Task<string> kutu = VeriOkuAsync();   // Hata burada fırlamaz, kutunun içine gizlenir
    Console.WriteLine("Buraya kadar geldi, hiçbir hata görünmedi");

    try
    {
        string sonuc = await kutu;         // Hata TAM OLARAK burada fırlar
    }
    catch (InvalidOperationException ex)
    {
        Console.WriteLine("Yakaladım: " + ex.Message);
    }
}
```

Bunu bilmemenin verdiği tipik kafa karışıklığı şu: "Metodumda `throw` var ama try-catch'ime düşmüyor" diyen biri genelde `Task`'ı hiç `await` etmemiştir — belki `.Result` kullanmıştır ya da Task'ı sonuna kadar hiç beklememiştir. Ayrıca `Task.WhenAll` ile birden fazla işte hata varsa, hepsi tek bir `AggregateException` içinde toplanır — bu da ayrıca dikkat edilmesi gereken bir noktadır.

## 4. `ValueTask` — ne zaman, neden kullanılır?

`Task`, bir sınıftır (class) — yani her `Task` oluşturulduğunda bellekte (heap'te) yeni bir nesne ayrılır. Çoğu durumda bu önemsizdir. Ama çok sık çağrılan (saniyede binlerce kez) ve çoğu zaman anında, senkron olarak bitebilen bir metotta, her seferinde gereksiz yere `Task` nesnesi oluşturmak performans kaybına yol açabilir.

`ValueTask`, bir struct'tır (class değil) — yani çoğu durumda ekstra bellek ayırmadan çalışabilir. Şöyle bir senaryoda işe yarar:

```csharp
public ValueTask<int> DegerAlAsync()
{
    if (cacheDoluysa)
        return new ValueTask<int>(cachedenGelenDeger); // Hiç Task oluşturmadan, anında dönüyor

    return new ValueTask<int>(GercekAsyncIslemAsync()); // Gerçekten beklemesi gerekiyorsa Task'a sarılır
}
```

Genel kural şudur: sıradan kodda `Task` kullanmaya devam edin, `ValueTask`'ı sadece performans ölçümüyle gerçekten darboğaz olduğunu tespit ettiğiniz, çok sık çağrılan yerlerde düşünün. Şimdilik sadece "kodda görürsem bu ne diye şaşırmayayım" diye bilmek yeterli — kendi kodunuzda aktif olarak kullanmanıza gerek yok.

