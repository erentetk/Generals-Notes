### 1. IDE (Integrated Development Environment)

**IDE**, bir yazılımcının kod yazmak, hata ayıklamak (debug) ve projeyi çalıştırmak için ihtiyaç duyduğu tüm araçları tek bir ekranda sunan dev bir "atölye" yazılımıdır.

- **Ne işe yarar?** Kodunuzu renklendirir, hataları siz daha kodu çalıştırmadan fark eder ve veritabanı gibi dış araçlarla bağlantı kurmanızı kolaylaştırır. (Örnek: Visual Studio, JetBrains Rider).


### 2. Solution vs. Project Arasındaki Fark

Bu iki kavram genellikle birbirine karıştırılır, ancak aralarında bir "konteyner" ilişkisi vardır:

- **Project (Proje):** Bir uygulamanın bir bölümünü veya tamamını oluşturmak için gereken tüm kaynak dosyalarının (kod dosyaları, görseller, yapılandırma dosyaları) bir araya getirilmiş halidir. Bir proje genellikle bir **.dll** veya **.exe** çıktısı üretir.
    
- **Solution (Çözüm):** Bir veya birden fazla projeyi bir arada tutan en üst düzey yapıdır. Büyük bir uygulamanın hem bir mobil uygulaması (Proje 1) hem de bir Web API'si (Proje 2) olabilir. İşte **Solution**, bu projelerin hepsini tek bir çatı altında tutar.
    

> **Kısaca:** Bir Solution, içerisinde birden fazla Project barındırabilir.

![[Pasted image 20260327151255.png|376]]

Örnegin burada Solution 'bsStoreApp' (5 of 5 projects) en çatıdır. Altindakiler de birer projedir.


### 3. IntelliSense Nedir?

**IntelliSense**, bir IDE'nin önemli özelliklerinden  olan akıllı kod tamamlama  hızlı  tamamlama özelliğidir.

### 4. Breakpoint Nedir?

**Breakpoint (Kırılma Noktası)**, kodunuzun çalışırken geçici olarak durmasını istediğiniz satıra koyduğunuz bir "dur işareti"dir.

**Neden kullanılır?** Kodunuz çalışırken bir değişkenin değerini o anda görmek veya programın o satıra ulaşıp ulaşmadığını anlamak. program break pointe geldigiğinde sürükleyerek  kody  gri veya ileri taşımak, örnegin x degeri 5 den büyükse ilgili break point çalışsıın gibi   ek özellikler İDE lerde bulunmaktadır. 


### 5. NuGet Nedir?

**NuGet**, .NET ekosistemi için bir **Paket Yöneticisidir (Package Manager)**.

- Tekerleği her seferinde yeniden icat etmenize gerek kalmaz. Örneğin, projenizde bir JSON dosyasını okumak istiyorsanız, bu işlemi yapan dünyaca ünlü `Newtonsoft.Json` kütüphanesini NuGet üzerinden tek tıkla projenize dahil edebilirsiniz. Başkalarının yazdığı güvenilir kod paketlerini projenize eklemenizi ve yönetmenizi sağlar.


### 6. Debug ve Release Build Arasındaki Farklar

Kodunuzu derlerken (build) önünüzde iki ana seçenek vardır:

| ==**Özellik**==    | ==**Debug Build**==                              | ==**Release Build**==                          |
| ------------------ | ------------------------------------------------ | ---------------------------------------------- |
| **Kullanım Amacı** | Geliştirme aşaması ve hata ayıklama.             | Son kullanıcıya sunulacak bitmiş ürün.         |
| **Hız**            | Daha yavaş çalışır.                              | Çok hızlıdır (Optimizasyonlar yapılır).        |
| **Boyut**          | Daha büyüktür (Hata ayıklama sembolleri içerir). | Daha küçüktür (Gereksiz veriler temizlenir).   |
| **Hata Ayıklama**  | Breakpoint koyup satır satır izleyebilirsiniz.   | Hata ayıklama zordur, kod optimize edilmiştir. |

# 7. ISS (Internet Information Services) 
IIS ; Nginx
Microsoft tarafından geliştirilen Windows işletim sistemleri  üzerinden  web sayfaları yayınlamak ve web uygulamaları çalıştırmak için kullanılan esnek yapılı bir web sunucusudur
## IIS kestrelden ayrı ne yapar?

IIS, uygulamanın etrafındaki **hosting ve yönetim işlerini** yapar.

### 1. Site yayınlama

- domain bağlama
- port ayarlama
- aynı sunucuda birden fazla site çalıştırma

Örnek:

- `site1.com` -> uygulama 1
- `api.site1.com` -> uygulama 2

Bunları IIS kolay yönetir.

### 2. SSL / HTTPS yönetimi

- sertifika tanımlama
- 80’den 443’e yönlendirme
- HTTPS yayını düzenleme

### 3. Reverse proxy görevi

İstek önce IIS’e gelir, sonra IIS bunu Kestrel’e yollar.

Yani:  
**Client -> IIS -> Kestrel -> App**

### 4. Process / yaşam döngüsü yönetimi

- uygulamayı başlatma
- durdurma
- yeniden başlatma
- çökünce toparlama
- application pool yönetimi

### 5. Güvenlik ve erişim kontrolü

- Windows Authentication
- IP kısıtlama
- request filtering
- bazı saldırılara karşı koruma katmanları

### 6. Statik dosya sunma

- css
- js
- resim
- html

Bunları doğrudan kendisi servis edebilir.

### 7. Loglama ve yönetim paneli

- gelen istekleri loglama
- hata takibi
- IIS Manager üzerinden görsel yönetim

---

## En net fark

==**Kestrel uygulamayı çalıştırır.**==  
==**IIS uygulamayı yayın ortamında yönetir ve dış dünyaya sunar.**==

# 8. Kestrel nedir?

**Kestrel**, ASP.NET Core uygulamaları için kullanılan **hafif ve hızlı web sunucusudur**.

Yani bir ASP.NET Core API veya web uygulaması yazdığında, uygulaman genelde kendi içinde **Kestrel** ile ayağa kalkar.
### Kestrel ne işe yarar?

- ASP.NET Core uygulamasını çalıştırır
- HTTP isteklerini dinler
- gelen isteği uygulamadaki controller / endpoint’lere gönderir
- cevabı geri döner

Örneğin:

- `http://localhost:5000`
- `https://localhost:5001`

gibi portlarda uygulamanı ayağa kaldırabilir.

### ## Benzetme

Bir mağaza düşün:

- **Kestrel** = mağazadaki çalışan, siparişi hazırlıyor
- **IIS** = mağazanın dış kapısı, güvenliği, tabela sistemi, giriş kontrolü, şube yönetimi

Yani müşteriyle temas eden dış düzeni IIS sağlar.  
Asıl işi içeride yapan motor ise Kestrel’dir.

---

## Tek başına neden Kestrel yetmeyebilir?

Yetebilir. Ama IIS şunları ekler:

- daha düzenli hosting
- Windows entegrasyonu
- kolay SSL yönetimi
- çoklu site yönetimi
- merkezi kontrol
- güvenlik ve loglama kolaylığı