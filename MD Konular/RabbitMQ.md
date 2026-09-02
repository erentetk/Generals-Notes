Açık kaynaklı bir mesaj kuyruğu (message brok er) yazılımıdır. Uygulamaların birbirine doğrudan bağlanmak yerine, aralarında bir "postane" üzerinden mesaj alışverişi yapmasını sağlar.

*Temel Mantık*: Bir uygulama mesajı (produceer) mesajı RabbitMQ'ya bırakılır. RabbitMQ mesajı bir kuyrukta tutar, vaşka bir uygulama(consumer) hazır olduğunda kuyruktan alır ve işler. İki taraf aynı anda ayakta olmak zorunda değildir. 

##### Ne işe Yarar: 

- Asenkron çalışma : Uzun süren işlerde (rapor üretme , e-posta gönderme, görüntü işleme) arka plana atılır ve kullanıcı beklemez.
- Yün dengeleme : aynı kuyrugu idnleyen birden fazla consumer olduğunda işler, Yogunlukta consumer sayısını arttırmek yeterlidir.
- Tampon (buffer) görevi Ani yük patlamalarında istek kuyrukta birikir sşsten çökmek yerine sırayla işler.
- Servisleri birbirinden ayırma (Decoupleing): Servis A, servis B'nin adresini/durumunu bilmek zorunda kalmaz. B kapalıyken  bile mesajlar  kaybolmaz., B açılınca işler. 
- Yayın(pub-sub)  : Bir oalyı birden fazla servise aynı anda duyurabilirisn. örn  Fatura sersivi -->
