
# 1. ISS (Internet Information Services) 
IIS ; NGINX, Apache HTTP Server bunalr IIS ile aynı işi yapar.
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

# 2. Kestrel nedir?

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