
![[CLR Architecture.png]]
# 1. CIL (Common Intermediate Language - Ortak Ara Dil)

Sürecin ilk aşamasında, C# veya F# gibi yüksek seviyeli dillerle yazılan kodlar doğrudan makine koduna dönüştürülmez. Bunun yerine, dil derleyicileri tarafından **CIL** (veya MSIL/IL) adı verilen, **işlemciden bağımsız bir ara dile** çevrilir.

- **İlişkideki Rolü:** CIL, programın farklı işletim sistemlerinde ve donanımlarda çalışabilmesini sağlayan taşınabilir bir "ara ürün"dür.


# 2. CLR (Common Language Runtime - Ortak Dil Çalışma Zamanı)

**CLR**, .NET uygulamalarının üzerinde çalıştığı **yürütme motorudur** ve işletim sistemi ile program arasında bir ara birim görevi görür.

- **İlişkideki Rolü:** CIL kodlarını devralan ve bunların nasıl çalıştırılacağını yöneten ana platformdur. CLR sadece kod çalıştırmakla kalmaz; bellek yönetimi (Garbage Collection), güvenlik ve tür güvenliği gibi hayati hizmetleri de sağlar.

# 3. JIT (Just-In-Time - Tam Zamanında) Derleyici

**JIT derleyicisi**, CLR'ın bir parçasıdır. Program çalıştırıldığı anda devreye girer ve **CIL kodlarını, o an kullanılan işlemcinin anlayabileceği yerel makine koduna (binary code) dönüştürür**.

- **İlişkideki Rolü:** JIT, CIL ile makine arasındaki köprüdür. Bir metod ilk kez çağrıldığında JIT bu metodu derler ve sonuçları bellekte saklar; böylece aynı metod tekrar çağrıldığında derleme işlemi tekrarlanmaz, doğrudan makine kodu çalıştırılır.


# Özet Çalışma Mekanizması
Bu üç kavram arasındaki ilişki şu işlem sırasıyla gerçekleşir 
##### 1- **Derleme zamanı**: Yazdığımız kodlar bir derleyici tarafından  CIL(Ortak ara dil)' e dönüştürülür.
##### 2- **Yükleme** ;Uygulama çalıştırıldığında **CLR** devereye girer ve gerekli dosyaları rama yükler.
##### 3- **Çalışma Zamanı (Runtime)**: Kodun yürütülmesi gerektiğinde CLR içindeki **JIT derleyicisi** bu CIL kodlarını alır ve işlemciye özel makine koduna çevrir 

##### 4- **Yürütme**;  En sondaki makine kodu CPU tarafından CLR'ın gözetimi altında çalıştırılır.
