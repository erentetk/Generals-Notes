LNB, çanağın önünde/altında duran o küçük silindirik parçadır. Açılımı "Low Noise Block" — düşük gürültülü blok çevirici.

Üç iş yapar:

**1. Toplanan sinyali yakalar.** Çanak sinyali bir noktada yoğunlaştırır, LNB'nin ucundaki huni (feed horn) tam o noktada durup sinyali içeri alır.

**2. Yükseltir.** Uydudan gelen sinyal inanılmaz zayıftır — 36.000 km yol gelmiştir. LNB bunu güçlendirir. "Düşük gürültülü" kısmı burada devreye girer: yükseltirken kendi elektroniğinin ürettiği paraziti mümkün olduğunca az katması gerekir, yoksa zaten zayıf olan sinyal gürültünün içinde kaybolur.

**3. Frekansı düşürür.** Bu en kritik kısım. Uydudan gelen sinyal ~11–12 GHz civarındadır. Bu frekans koaksiyel kabloda çok hızlı sönümlenir; birkaç metre kablonun sonunda elinizde hiçbir şey kalmaz. LNB sinyali 950–2150 MHz aralığına çevirir. Bu frekans kabloda rahat yol alır, 20–30 metre kablo çekebilirsiniz.

Sonra sinyal tek bir koaksiyel kabloyla evdeki uydu alıcısına gider.

Aynı kablo çift yönlü çalışır: alıcı, LNB'ye elektrik de gönderir. Gönderdiği voltajı değiştirerek (13V veya 18V) LNB'ye hangi polarizasyondan yayın alacağını söyler, 22 kHz'lik bir sinyal ekleyerek de alt/üst frekans bandını seçer. Yani ayrı bir kumanda kablosu yoktur, her şey o tek kablodan halledilir.

Çanak sinyali toplar, LNB onu kullanılabilir hale getirir. İkisi olmadan diğeri işe yaramaz.

## Ticari antenlerde ne fark var. 
Temel fizik aynı: parabolik yansıtıcı sinyali odakta toplar, oradaki besleme onu alır, düşük gürültülü bir yükselteç güçlendirir, sonra frekans düşürülür. Bu kısım 60 cm'lik balkon çanağında da 13 metrelik teleport antenlerinde de değişmez.
#### Değişen şeyler şunlar:  

**Çift yönlü çalışırlar.** Ev çanağı sadece dinler. VSAT terminalleri, haber araçları (SNG), teleportlar hem alır hem gönderir. Bu yüzden LNB'nin yanında bir de [[BUC (Block up Converter)]] veya güç yükselteci bulunur — gönderilecek sinyali yukarı frekansa çıkarıp güçlendirir. Aynı beslemeden hem alım hem gönderim geçtiği için araya OMT ve diplekser girer; bunlar iki yönü polarizasyona ve frekansa göre birbirinden ayırır.

**Gönderim yaptığınız anda regülasyon devreye girer.** Yayın yaptığınız için yan lobların (yani çanağın asıl hedefi dışına saçtığı enerjinin) komşu uyduları rahatsız etmemesi gerekir. ITU-R S.580 ve FCC 25.209 gibi standartlar buna sınır koyar. Ev çanağında böyle bir zorunluluk yok, çünkü hiçbir şey yaymıyor.

**Geometri büyüdükçe değişir.** Offset yapı yaklaşık 2–2.4 metreye kadar yaygındır. Daha büyüklerde genelde Cassegrain veya ring-focus tasarımı kullanılır: odakta ikinci bir küçük yansıtıcı (subreflector) durur, sinyal oradan çanağın merkezindeki deliğe yönlendirilir. Böylece ağır elektronik anten kolunun ucunda değil, çanağın arkasında, zemine yakın bir kabinde durur. Bakımı ve soğutması kolaylaşır.

**Frekans kararlılığı ciddi bir mesele.** Ev tipi LNB'ler DRO denilen basit bir osilatör kullanır; birkaç yüz kHz kayabilir, DVB alıcısı bunu tolere eder. Profesyonel tarafta PLL LNB kullanılır ve harici bir 10 MHz referans saatine kilitlenir. Dar bantlı taşıyıcılarda bu kaymaya tahammül yoktur.

**Yedeklilik ve takip.** Teleportlarda LNB'ler 1:1 veya 1:2 yedekli kurulur, biri arızalanınca anahtar devreye girer. Eğimli yörüngedeki uyduları izlemek için motorlu step-track sistemleri vardır. Buzlanmaya karşı ısıtıcı, rüzgâr yükü için ağır çelik yapı, bazen radome eklenir.
