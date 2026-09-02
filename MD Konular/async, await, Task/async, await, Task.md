# Async / Await Notları

## Örnek

```csharp
public async Task CalistirAsync()
{
    Console.WriteLine("Veri isteniyor...");

    Task<string> kutu = VeriOkuAsync();   // Kutu hemen elimize geçer, ama içi henüz boş
    Console.WriteLine("Kutu elimde ama içi henüz dolmadı, başka iş yapabilirim.");

    string sonuc = await kutu;            // Burada kutu doluncaya kadar bekliyoruz (bloklamadan)

    Console.WriteLine("Kutunun içindeki değer: " + sonuc);
}
```

## `async` anahtar kelimesi ne işe yarar?

`async`, bir metodun başına yazılan bir işarettir ve şunu söyler: "Bu metodun içinde `await` kullanabilirsin." Yani `async` tek başına bir şeyi asenkron yapmaz — sadece o metoda `await` kullanma izni verir. Derleyici, `async` yazılan bir metodu arka planda tamamen farklı bir yapıya dönüştürür (buna "state machine" denir) ama siz bunu görmezsiniz, sadece normal kod yazıyormuş gibi hissedersiniz.

## `await` anahtar kelimesi ne yapar, tam olarak?

`await Task.Delay(2000)` satırına geldiğinde, üç şey olur:

1. Metot, `Task.Delay(2000)` işini başlatır (bu iş "2 saniye sonra bitecek bir Task" döndürür).
2. `await`, o Task'ın bitmediğini görünce, metodu burada durdurur ve thread'i serbest bırakır. Yani program bu noktada donmaz — çağıran taraf başka işler yapabilir, thread başka isteklere bakabilir.
3. 2 saniye dolup Task tamamlandığında, .NET otomatik olarak bu metodu kaldığı satırdan devam ettirir — sanki hiç ara vermemiş gibi, `string veri = ...` satırına geçer.

Yani `await`, "bekle ama bloklamadan bekle, iş bitince kaldığın yerden devam et" demenin yoludur. Fark şu: normal (senkron) bir bekleme thread'i tamamen kilitler, `await` ise thread'i bırakır ve iş bitince geri gelir.

## Metot neden `Task<string>` döndürüyor, `string` değil?

Çünkü metodun içinde `await` olduğu an, metot anında tamamlanamaz — 2 saniye beklemesi gerekiyor. Bu yüzden çağrıldığı anda size doğrudan `string` veremez, onun yerine "ileride bu string'i üretecek olan iş" anlamına gelen `Task<string>` kutusunu hemen döndürür. Siz bu kutuyu `await` ile açtığınızda gerçek `string` değerini alırsınız.

## Zincir nasıl işliyor (bir üst seviye)

```csharp
public async Task CalistirAsync()
{
    string sonuc = await VeriOkuAsync();
    Console.WriteLine(sonuc);
}
```

`CalistirAsync()` de `async` çünkü içinde `await VeriOkuAsync()` var. `VeriOkuAsync()` beklerken, `CalistirAsync()`'in thread'i de serbest kalır. Bu zincir böyle yukarı doğru devam eder — bu yüzden "async all the way" (baştan sona asenkron) deniyor. Zincirin bir yerinde birisi `.Result` veya `.GetAwaiter().GetResult()` ile bu zinciri kırıp senkrona çevirirse, deadlock riski ortaya çıkar.

