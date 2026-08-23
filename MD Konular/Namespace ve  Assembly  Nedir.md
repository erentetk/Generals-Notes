
# 1. Namespace 

- **Namespace**, C#’ta sınıfları, interface’leri, enum’ları ve benzeri yapıları **mantıksal olarak gruplamak** için kullanılan bir isim alanıdır.
**Aynı namespace içinde aynı isimde iki class bulunamaz.**  
Ama **farklı namespace’lerde aynı isimde class bulunabilir.**

- Fiziksel bir dosyalama yapmaz sadece bir alan adıdır.
- Benzer yapıları aynı gurup altında toplamak 

```csharp
namespace StoreApp.Entities
{
    public class Product
    {
        public int Id { get; set; }
        public string Name { get; set; }
    }
}

namespace StoreApp.DTOs
{
    public class Product
    {
        public int Id { get; set; }
        public string Name { get; set; }
        public decimal Price { get; set; }
    }
}
```
Burada:

- `StoreApp.Entities` içinde bir `Product` class’ı var
- `StoreApp.DTOs` içinde de yine bir `Product` class’ı var

Bu şekilde  aynı isimde birden falza  sınıf tanımlayabiliriz.


# 2.  Assembly 

Normalde Assembly denince akla düşük seviyeli bir  dil gelir. Zorluğu ve anlaşılması  zor olmasıyla bilinen bir dildir.

Bizim odaklandığımız konu 
## .NET de assembly nedir?

**.NET Assembly**, herhangi bir .NET dilinde (C#, VB.NET, F# gibi) yazılan kodun derlenmesi sonucunda oluşan **.dll** veya **.exe** uzantılı dosyalardır,,. Bir Assembly; uygulamanın çalışma mantığını, kendi tanımını ve gerekli kaynakları barındıran temel dağıtım, sürüm oluşturma ve güvenlik birimidir,.

.NET Assembly'nin Dört Ana Bileşeni

Bir Assembly yapısı fiziksel olarak dört temel bölümden oluşur,:

1. **Common Intermediate Language (CIL / MSIL):** .NET dilleriyle yazılan kaynak kodun derleyici tarafından dönüştürüldüğü, işlemci ve işletim sisteminden bağımsız olan ortak ara dildir,,. Bu kodlar makine kodu değildir; çalışma zamanında CLR (Common Language Runtime) tarafından JIT derleyicisi kullanılarak o anki işlemcinin anlayacağı dile çevrilir,.

2. **Metadata:** Assembly içindeki her bir tipin (sınıf, yapı, enum vb.) ayırt edici özelliklerini, üyelerini (metotlar, değişkenler, referanslar) ve dışarıdan referans verilen diğer kütüphanelerin bilgisini taşır,,. Visual Studio'nun sunduğu **IntelliSense önerileri bu bilgilerden faydalanarak çalışır,.**

3. **Manifest (Assembly Metadata):** Assembly'nin kimliğini tanımlayan bölümdür,. İçerisinde **Assembly adı**, **versiyon numarası**, kültür bilgisi, yasal bilgiler ve bağımlı olunan diğer Assembly listeleri gibi veriler tutulur,,. Bu yapı sürüm kontrolü (versioning) ve güvenlik açısından kritiktir.

4. **Resources (Kaynaklar):** Uygulamanın kullandığı resimler, ikonlar, ses dosyaları, HTML/XML dosyaları veya yerelleştirme (localization) dosyaları gibi kod dışı yardımcı varlıkların depolandığı bölümdür,,.

Assembly Dosya Türleri ve Dağıtım Biçimleri

- **Exe ve Dll Ayrımı:** Çalıştırılabilir dosyalar (.exe), programın giriş noktası olan bir **Main** metodu içerirken; kütüphane dosyaları (.dll) bu giriş noktasını içermez ve başka uygulamalar tarafından referans alınarak kullanılır.
- **Tekli ve Çoklu Dosya Yapısı:** Çoğu uygulama varsayılan olarak tüm bileşenlerin tek bir dosya içinde toplandığı **Single-File Assembly** yapısını kullanır. Ancak ihtiyaç duyulursa, manifest'in birincil modülde olduğu ve diğer kütüphanelerin ayrı modüller olarak yer aldığı **Multiple-File Assembly** yapısı da oluşturulabilir.
- **Özel ve Paylaşılan Assembly'ler:**
    - **Private (Özel):** Sadece belirli bir uygulama tarafından kullanılır ve uygulama dizininde bulunur,.
    - **Shared (Paylaşılan):** Aynı makinedeki birden fazla uygulama tarafından kullanılabilir,. Bu tür Assembly'lerin **[[Global Assembly Cache (GAC)]]** adı verilen özel bir dizine atılması ve benzersizliği garanti eden bir **Strong Name (Güçlü İsim)** alması gerekir,.
