## 1. Stack (Yığın)

Stack, "LIFO" (Last In, First Out - Son Giren İlk Çıkar) prensibiyle çalışan, boyutu nispeten küçük ama erişim hızı çok yüksek olan bir bellek bölgesidir.

- **Yönetim:** Tamamen CPU tarafından otomatik olarak yönetilir. Bir metod çağrıldığında o metoda ait değişkenler "push" edilir (eklenir), metod bittiğinde ise "pop" edilerek (silinerek) bellek anında boşaltılır.
    
- **Neler Tutulur:** * **Değer Tipleri (Value Types):** `int`, `bool`, `double`, `char`, `struct` gibi veriler.
    
    - **Pointerlar:** Heap bölgesindeki verilerin adreslerini tutan referanslar.
        
- **Özellikleri:**
    
    - Veriye erişim çok hızlıdır.
        
    - Bellek boyutu sınırlıdır (Stack Overflow hatası genellikle buranın dolmasıyla oluşur).
        
    - Değişkenlerin ömrü, bulundukları "scope" (kapsam) ile sınırlıdır.
        

---

## 2. Heap (Öbek)

Heap, programın çalışma zamanında (runtime) dinamik olarak kullanılan, Stack'e göre çok daha büyük ama yönetimi daha karmaşık bir bellek bölgesidir.

- **Yönetim:** Programcı veya çalışma zamanı motoru (örneğin .NET'teki **Garbage Collector**) tarafından yönetilir.
    
- **Neler Tutulur:** * **Referans Tipleri (Reference Types):** `class`, `string`, `array`, `object` gibi yapılar.
    
- **Özellikleri:**
    
    - Veriler bellekte rastgele yerlere dağılabilir.
        
    - Erişim hızı Stack'e göre daha yavaştır çünkü önce adresi bulmak (pointer takibi) gerekir.
        
    - Verilerin ömrü, onlara olan referans koptuğunda Garbage Collector'ın insiyatifine kalır.
        

---

## Stack ve Heap Arasındaki Temel Farklar

Aşağıdaki tablo, bu iki yapının çalışma mantığını özetlemektedir:

|**Özellik**|**Stack**|**Heap**|
|---|---|---|
|**Veri Tipi**|Değer tipleri ve referans adresleri|Nesneler ve referans tipleri|
|**Erişim Hızı**|Çok hızlı|Daha yavaş|
|**Yönetim**|Otomatik (LIFO)|Manuel veya Garbage Collector|
|**Boyut**|Sabit ve küçük|Dinamik ve büyük|
|**Hata Durumu**|Stack Overflow|Out of Memory|