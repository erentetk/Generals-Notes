Yazılım dünyasında bir dönem çok yaygın bir sorun vardı: aynı bilgisayarda çalışan iki farklı uygulama, ortak kullandıkları bir DLL dosyasının farklı sürümlerine ihtiyaç duyabiliyordu. Başlangıçta her şey sorunsuz görünürdü. İki uygulama da aynı şehirde, aynı sistemde yaşar; ortak kullandıkları DLL dosyasının 1.0 sürümüyle gayet güzel çalışırdı. Saat gibi işlerdi.

Sonra bir gün siz masumca sadece **B uygulamasını** güncellemeye karar verirdiniz. İşte felaket de tam burada başlardı. Çünkü bu güncelleme, iki uygulamanın da ortak kullandığı DLL dosyasını 2.0 sürümüne yükseltirdi. İşletim sistemi de “en güncel sürüm en iyisidir” mantığıyla hareket edip yeni dosyayı kullanmaya başlardı. Sonuç olarak B uygulaması yeni sürümle harika şekilde çalışmaya devam ederdi. Ancak A uygulaması, eski sürümün içindeki belirli bir fonksiyona sıkı sıkıya bağlıysa aniden çökerdi. Üstelik çoğu zaman ortada klasik anlamda bir yazılım hatası da olmazdı. Sistem aslında tam da tasarlandığı gibi çalışıyordu; yalnızca tasarımın kendisi sorunluydu.

İşin trajikomik yanı şuydu: bu bir bug değildi. İşletim sistemi gerçekten de “en yeni sürüm en iyisidir” mantığıyla hareket ediyor ve tüm programlara aynı en güncel dosyayı veriyordu. Yani sistem kendi kuralı yüzünden çöküş üretiyordu. İşte bu duruma yıllarca **DLL Hell** yani “DLL cehennemi” dendi.

Microsoft, bu sürüm çakışmalarının sürdürülebilir olmadığını fark etti ve .NET Framework ile birlikte radikal bir yaklaşım getirdi: **GAC**, yani **Global Assembly Cache**.

---

## GAC: Devlet Kasası Mantığı

Microsoft’un burada söylediği şey aslında şuydu:  
“Madem herkes aynı dosyayı ezip duruyor, ben de bu dosyanın farklı sürümlerini aynı bilgisayarda yan yana yaşatırım.”

GAC tam olarak bu felsefeyle ortaya çıktı. Basitçe düşünürsek GAC, sisteme kurulan paylaşımlı .NET assembly’leri için oluşturulmuş, son derece kontrollü ve güvenli bir merkezî depo gibiydi. Bunu bir tür **devlet kasası** gibi düşünebiliriz. Rastgele bir DLL dosyası alınıp bu kasaya konulamazdı. Buraya girebilmek için bir assembly’nin kimliğinin çok net biçimde tanımlanmış olması gerekiyordu.

Bir dosyanın bu kasaya girebilmesi için yalnızca adı yetmezdi. Çünkü sistem artık “sharedapp.dll” gibi belirsiz bir kavramla çalışmak istemiyordu. Bunun yerine her assembly için çok daha ayrıntılı bir kimlik bilgisi gerekiyordu. Bu kimlik şu dört temel bileşenden oluşurdu:

- Assembly adı
- Tam sürüm numarası
- Culture bilgisi
- Public Key Token

Yani bir uygulama artık gidip sadece “Bana sharedapp lazım” demezdi. Şöyle derdi:  
“Bana sharedapp’in **1.0.0.0** sürümü lazım, culture bilgisi **neutral** olsun ve üzerinde tam olarak şu **public key token** bulunan sürüm olsun.”

Sistem de gidip kasadaki doğru çekmeceden tam olarak o assembly’yi çıkarırdı. Böylece A uygulaması 1.0 sürümünü, B uygulaması ise 2.0 sürümünü kullanabilirdi. Teoride kimse kimsenin işine karışmazdı. DLL cehennemi bitmiş gibi görünüyordu.

---

## Strong Name: Kriptografik Pasaport

Buradaki en kritik nokta **strong naming**, yani güçlü adlandırmaydı. GAC’e girecek assembly’nin kriptografik olarak imzalanmış olması gerekiyordu. Bunu eski zamanlardaki kraliyet mektuplarının üzerindeki sıcak mum mühürleri gibi düşünebiliriz.

Örneğin Microsoft, yazdığı DLL’i kendi özel anahtarıyla dijital olarak imzalardı. Dosyanın üzerinde de buna karşılık gelen açık anahtar bilgisi bulunurdu. Böylece işletim sistemi şu iki şeyi doğrulayabilirdi:

1. Bu dosya gerçekten iddia ettiği üretici tarafından mı yayımlandı?
2. Yolda birisi tarafından değiştirilmiş veya sahte biçimde üretilmiş olabilir mi?

Eğer mühür kırılmışsa, yani imza geçersizse, sistem dosyayı anında reddederdi. Böylece artık sistemde belirsiz assembly kavramı kalmazdı. Her şeyin kimliği nettir, pasaportu bellidir ve sistem yalnızca istediği pasaporta sahip dosyayı verir.

---

## Teoride Kusursuz, Pratikte Kâbus

Kağıt üzerinde GAC çok iyi bir çözümdü. Ancak pratikte kendi bürokratik kâbusunu yarattı.

Çünkü o “devlet kasasına” bir şey koymak ya da oradan bir şey çıkarmak düşündüğünüz kadar kolay değildi. Basit bir DLL’i uygulamanızın klasörüne kopyalayıp çalıştırmak yerine, bir sürü kurala ve araca uyum göstermeniz gerekiyordu. Programatik tarafta çeşitli özel API’ler, güvenlik kontrolleri ve sistem izinleri devreye giriyordu. Manuel tarafta ise geliştiriciler `gacutil.exe` gibi araçlarla uğraşıyordu.

Yani bir dosyayı yüklemek veya kaldırmak için terminal açıyor, çeşitli komutları ezberliyor ve sistemin istediği prosedürleri yerine getiriyordunuz. Eskinin basit “kopyala-yapıştır” mantığı tamamen ortadan kalkmıştı.

Üstelik kasanın fiziksel yeri bile kullanıcı dostu değildi. .NET sürümüne göre GAC’in bulunduğu yol değişebiliyor, bazı klasörler normal dosya sistemi gibi görünmüyor, özel kabuk uzantıları nedeniyle içerik farklı şekilde sunuluyordu. Geliştiriciler adeta birbirlerine yol tarifi vererek bu yapıyı anlamaya çalışıyordu.

---

## Silmek Bile Dertti

Sorun sadece yüklemek değildi; kaldırmak da büyük problemdi.

Diyelim ki bir sunucu yöneticisisiniz. Sistemde kurulu hatalı bir assembly’yi silmek istiyorsunuz. Yetkiniz var, makinenin sahibisiniz, her şey size ait. Sağ tıklayıp silmek istiyorsunuz ama sistem size “Erişim reddedildi” diyor.

Neden? Çünkü GAC arkasında çok sıkı bir referans ve kurulum yönetimi mantığı vardı. Eğer o assembly sisteme bir **Windows Installer (MSI)** paketiyle kurulduysa, sistem onun kaldırılmasını da aynı mekanizma üzerinden bekliyordu. Yani dosyayı dosya gibi değil, kurulu bir sistem bileşeni gibi görüyordu. Sonuç olarak basitçe silme işlemi yapılamıyor; orijinal kurulum paketini bulup onun kaldırma prosedürünü işletmeniz gerekiyordu.

Bu da eski **xcopy deployment** mantığını fiilen öldürdü. Eskiden yazdığınız küçük bir uygulamayı USB belleğe atıp başka bir bilgisayara kopyalayabilir ve çalıştırabilirdiniz. Ancak GAC dünyasında işler artık böyle yürümüyordu. Uygulamanız için ciddi kurulum paketleri hazırlamanız gerekiyordu.

---

## Modern Dünyanın İhtiyacı Değişti

Tam bu sırada yazılım dünyası da değişmeye başlamıştı. Bulut sistemleri, konteyner teknolojileri, hızlı dağıtım süreçleri ve mikroservis mimarileri öne çıkıyordu. Geliştiriciler artık uygulamalarının sadece kendi klasörleri içinde yaşamasını, başka uygulamalardan etkilenmemesini ve sisteme mümkün olduğunca az bağımlı olmasını istiyordu.

Merkezî bir kasa güvenli görünüyordu ama her işlem için bürokrasi gerekiyordu. Bu da çevikliği öldürüyordu. Modern dünya daha hafif, daha izole ve daha taşınabilir uygulamalar istiyordu.

Microsoft da sonunda bu dönüşümü kabul etti. .NET Core ile başlayıp .NET 5 ve sonrası sürümlerde bu eski yaklaşım büyük ölçüde terk edildi. **GAC kavramı modern .NET tarafında fiilen ortadan kalktı.**

Hatta eski alışkanlıklarla GAC’i sorgulayan bazı API’ler modern .NET tarafında size doğrudan “false” döndürüyor ve bunun artık desteklenmediğini belirten uyarılar üretiyor. Yani sistem adeta “o eski dünya artık yok” diyor.

---

## Yeni Dünya: Herkes Kendi Sırt Çantasını Taşıyor

Bu dönüşümü anlamanın en kolay yolu şu metafordur:

Eskiden herkes eşyalarını ortak bir devlet kasasına koyuyordu.  
Şimdi ise herkesin kendi **sırt çantası** var.

Bu sırt çantası, uygulamanın çalışması için ihtiyaç duyduğu bağımlılıkları kendi içinde taşıması anlamına geliyor. Modern .NET ve NuGet ekosisteminde uygulamalar artık ihtiyaç duydukları kütüphaneleri kendi bağımlılık ağları içinde getiriyor. Böylece bir uygulamanın kullandığı paket diğerini doğrudan bozmaz hale geliyor.

Bir başka deyişle:

- Kimse ortak bir kasaya gitmiyor
- Kimse başkasının çantasına elini sokmuyor
- Herkes kendi ihtiyacı olan sürümü kendi uygulamasıyla birlikte taşıyor

Bu yüzden günümüzde “Bir programı güncelledim, öteki bozuldu” tarzı problemleri çok daha az yaşıyoruz. İzolasyon, modern yazılım dünyasının temel prensiplerinden biri haline geldi.

---

## Büyük Kurumsal Kütüphaneler ve NuGet’in Gerçek Sınavı

Elbette küçük bir hesap makinesi uygulamasında bu mantık çok rahat işler. Asıl soru, devasa kurumsal sistemlerde bu yaklaşımın nasıl ayakta kaldığıdır.

Özellikle Microsoft’un veri erişimi ve kurumsal bağlantılar için geliştirdiği istemci kütüphaneleri burada iyi bir örnektir. Bunlar sıradan DLL’ler değildir; büyük şirketlerin, bankaların ve kurumsal sistemlerin veri tabanlarıyla konuşmasını sağlayan kritik altyapı parçalarıdır.

Eskiden bu tür bileşenler çoğu zaman MSI paketleriyle sisteme kuruluyor ve GAC benzeri merkezî yapılara yaslanıyordu. Ancak zamanla bunların dağıtım biçimi de değişti. Modern yaklaşım bu bileşenlerin de NuGet paketleri üzerinden, uygulamayla birlikte ve izole biçimde taşınması yönünde oldu.

Bu değişimin güvenlik tarafında çok önemli etkileri vardı.

---

## Güvenlik Güncellemeleri Neden Merkezî Değil de İzole Oldu?

Örneğin güvenlik protokolleri zamanla değişti. Bir dönem eski TLS 1.0 ve 1.1 protokolleri artık güvensiz kabul edildi ve TLS 1.2 gibi daha modern protokoller zorunlu hale geldi. Benzer şekilde eski kimlik doğrulama kütüphaneleri yerini daha güncel çözümlere bıraktı.

Şimdi şu soruyu soralım:  
Eğer hâlâ eski GAC mantığında yaşasaydık, bu güncellemeler nasıl uygulanacaktı?

Diyelim ki aynı sunucuda 20 farklı şirketin 20 farklı uygulaması çalışıyor. Eğer ortak kasadaki merkezi bileşeni tek kalemde modern protokollere geçirirseniz, yeni uygulamalar bundan memnun olur. Ama yıllardır sessiz sedasız çalışan, yalnızca eski protokolleri anlayan eski bir insan kaynakları uygulaması aniden veri tabanına bağlanamaz hale gelebilir. Sonuç: bordro sistemi çöker.

İşte modern paket temelli modelin gücü burada ortaya çıkar. Yeni ve güvenli uygulama kendi sırt çantasında en güncel kütüphaneleri ve güvenlik standartlarını getirir. Eski uygulama ise kendi eski bağımlılıklarıyla yaşamaya devam edebilir. Aynı sunucu üzerinde, birbirlerini bozmadan çalışabilirler.

Bu yaklaşım özellikle **çoklu runtime**, bulut sistemleri, mikroservisler ve bağımsız dağıtım yapan ekipler için büyük avantaj sağladı. Artık kimse “Benim küçük güncellemem başka ekibin uygulamasını bozar mı?” korkusuyla haftalarca beklemek zorunda değil.

---

## Ama Yeni Dünya Kusursuz mu?

Burada çok önemli ve rahatsız edici bir soru ortaya çıkıyor.

Eski merkezî modelde herkes aynı dosyaya bağlıydı. Evet, bu büyük bir sürüm çakışması ve DLL cehennemi üretiyordu. Ama bir güvenlik açığı bulunduğunda teorik olarak tek bir merkezi dosyayı yamamanız yeterliydi.

Modern izolasyon modelinde ise durum farklı. Artık tek bir merkezî dosya yok. Aynı bağımlılığın binlerce kopyası, binlerce farklı uygulamanın kendi “sırt çantası” içinde yaşıyor. Eğer kritik bir sıfırıncı gün güvenlik açığı bulunursa, o zaman yapılması gereken şey tek bir merkezi güncelleme değil; ilgili tüm uygulamaların kendi bağımlılıklarını tek tek güncellemesidir.

Bu da şu soruları doğuruyor:

- DLL cehenneminden kurtulurken acaba bu kez bir **güvenlik yaması cehennemine** mi girdik?
- Herkes aynı bağımlılığın kopyasını taşıdığı için gereksiz **disk alanı israfı** mı oluşuyor?
- Yazılım dünyasında gerçekten nihai çözüm diye bir şey var mı?
- Yoksa her yeni çözüm, yönetmesi daha kolay olduğunu düşündüğümüz yeni sorunlarla bir takas mı?

Aslında modern izolasyon modeli büyük ölçüde başarılı oldu. Çünkü günümüz donanım kaynakları çok daha ucuz ve bol. Birkaç megabaytlık tekrar eden dosyalar, geçmişte olduğu kadar büyük problem değil. Buna karşılık uygulama bağımsızlığı, dağıtım kolaylığı, ekipler arası izolasyon ve güvenli güncelleme stratejileri çok daha değerli hale geldi.

Ama yine de gerçek şu: yazılım mühendisliğinde çoğu zaman “mükemmel çözüm” yoktur. Genellikle eski problemlerin yerine, daha yönetilebilir yeni problemler gelir.
## Sonuç

Bu yolculukta önce DLL cehennemini gördük: ortak dosya kullanımı kolay görünüyordu ama sürüm çakışmaları sistemleri kırıyordu. Sonra düzen getirmek için GAC ortaya çıktı: güçlü adlandırma, sürüm kontrolü ve merkezi kimlik doğrulamasıyla sorunları çözmeye çalıştı. Ancak bu kez de sistem ağır bir bürokrasiye dönüştü; yüklemesi, kaldırması, yönetmesi zorlaştı ve geliştirici çevikliğini öldürdü.

Ardından modern dünya geldi. Bulut, konteynerler, mikroservisler ve hızlı dağıtım süreçleri merkezi kasayı gereksiz hale getirdi. GAC’in yerini, uygulamanın kendi bağımlılıklarını kendi içinde taşıdığı NuGet temelli izolasyon modeli aldı. Herkes kendi sırt çantasını taşımaya başladı.

Bu yaklaşım sürüm çakışmalarını büyük ölçüde azalttı, ekiplerin birbirini bozmasını önledi ve modern yazılım mimarisinin temelini oluşturdu. Ancak karşılığında bağımlılıkların çoğalması, güncelleme yönetimi ve güvenlik yamalarının dağınık hale gelmesi gibi yeni sorumluluklar getirdi.