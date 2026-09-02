BUC, "Block Up Converter" — blok yukarı çevirici. LNB'nin tam tersi işi yapar.

LNB uydudan gelen sinyali alır, aşağı frekansa çevirir. BUC ise sizin göndereceğiniz sinyali alır, yukarı frekansa çevirir ve güçlendirip beslemeye verir. İkisi çift yönlü bir terminalde yan yana durur, aynı çanağı paylaşır.

**Zinciri baştan takip edelim**

Modem, gönderilecek veriyi 950–1450 MHz civarında (L bandı) bir sinyale çevirir ve koaksiyel kabloyla çanağa yollar. Kablo bu frekansı rahat taşır — LNB tarafındaki mantığın aynısı, sadece ters yönde.

Kablonun ucundaki BUC bu sinyali alır, uydunun beklediği frekansa çıkarır (Ku bandında ~14 GHz, Ka bandında ~30 GHz) ve gücünü yükseltir. Sonra besleme üzerinden çanağa, oradan uzaya gider.

**Dikkat gereken nokta**

BUC ciddi güç yayar. Çalışırken beslemenin önüne geçmek tehlikelidir; RF maruziyeti gerçek bir risktir. Ayrıca yanlış ayarlanmış bir BUC komşu uydulara parazit verebileceği için, gönderim yapan terminallerin operatör tarafından yetkilendirilip devreye alınması (line-up) gerekir.

**İnsanın Maruz kaldığı durumda** ; yanık riski var, ama şok edici bir olay değil. RF yanığı termal bir yaralanmadır — doku ısınır. Radyasyon hastalığı ya da iyonlaştırıcı hasar değil, X-ışını veya nükleer maruziyetle ilgisi yok. Ku bandındaki enerji dokuya birkaç milimetre kadar girer ve orayı ısıtır. Yeterince yakın ve yeterince güçlü bir kaynakta ısınma hissedilir hale gelir, uzun kalırsanız yanık oluşur. 