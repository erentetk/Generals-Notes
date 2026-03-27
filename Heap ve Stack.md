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

| **Özellik**                    | **Stack (Yığın)**                                                | **Heap (Öbek)**                                                     |
| ------------------------------ | ---------------------------------------------------------------- | ------------------------------------------------------------------- |
| **Temel Görev**                | Metod çağrılarını ve yerel verileri yönetir.                     | Dinamik nesneleri ve büyük verileri tutur.                          |
| **Veri Türü**                  | Değer tipleri (`int`, `double`, `struct`) ve referans adresleri. | Referans tipleri (`class`, `string`, `array`, `list`).              |
| **Yaşam Süresi**               | **Kısa ve Belirli:** Metod (Scope) bittiği an ölür.              | **Uzun ve Belirsiz:** Kimse kullanmayana dek yaşar.                 |
| **Erişim Hızı**                | **Çok Hızlı:** CPU'nun özel yazmaçları (Register) ile yönetilir. | **Yavaş:** Bellekte adres arama ve pointer takibi gerekir.          |
| **Bellek Yönetimi**            | **İşlemci (Hardware):** LIFO yapısıyla otomatik temizlenir.      | **Garbage Collector (Software):** Arka planda tarama yapılır.       |
| **Parçalanma (Fragmentation)** | **Yok:** Veriler her zaman bitişik ve sıralıdır.                 | **Var:** Nesneler silindikçe bellekte "delikler" oluşur.            |
| **Hata Türü**                  | **StackOverflow:** Çok derin recursive (özyinelemeli) çağrılar.  | **OutOfMemory:** RAM kapasitesinin tamamen dolması.                 |
| **Güvenlik (Thread Safety)**   | **Güvenli:** Her Thread'in kendi özel Stack'i vardır.            | **Riskli:** Tüm Thread'ler aynı Heap'i paylaşır (Lock gerekebilir). |
| **Boyut Sınırı**               | Genelde 1MB - 8MB gibi çok küçük limitleri vardır.               | İşletim sisteminin ve RAM'in izin verdiği kadar büyüktür.           |
