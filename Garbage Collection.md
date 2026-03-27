**Çöp toplama** modern programla dillerinde (C# , java ,pyhon) gibi dillerde  bellek yönetimini  otomatize eden bir mekanizmadır.

Yazılımcının belleği manuel olarak tahsis etme ve serbest bırakma (C++'daki `malloc` ve `free` gibi) yükünü üzerinden alır.

Çöp toplama, bellepin **heap** bölgesindeki  nesneler ile ilgilenir. Temel amacı programın artık ihtiyaç duymadığı  nesneleri tespit edip silerek bellekte yer açmak ve ==Memory Leak== (Bellek Sızıntısı) oluşmasını engellemektir.

GC'nin Stack bölgesine  müdehale etmemesinin sebebi; Stackdeki yerel degişkenler ve metod cağrıları, kapsam (Scope) dışına cıktıklarında otomatik olarak silinirler. GC sadece Heap üzerindeki dinamik nesneleri yönetir.


