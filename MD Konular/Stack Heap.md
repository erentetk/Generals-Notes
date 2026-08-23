
# 1. Saklanan Veri türleri
## Stack; 
Burada deger tipleri saklanır.(int, double bool, struct,) pointerlar ve nesnelerin bellekteki adresi saklanır. 
## Heap: 
Referans tiplerde (class nesneleri gibi) nesnenin asıl verisi heap’te tutulur. Bu nesneye erişimi sağlayan referans ise bulunduğu bağlama göre tutulur; yerel değişkenlerde çoğu zaman stack üzerinde düşünülür.

# 2. Erişim Hızı ve Performans
## Stack; 
Heap e göre daha hızlıdır. Veriler art arda sıralandıgı için verilere erişim kolaydır.
## Heap: 
Stack e kıyasla daha yavaştır. karmaşık veri yapıları ve nesneler burada depolandıgı için maliyet daha fazladır.

# 3. Yaşam Döngüsü

Scope içinde tanımlanan nesneler  scope  bitiminde bellekte temizlenirken. Heap te nesneler uygulama boyunca  ortak kullanılır ve bellek yönetimi  Garbage Collector taradından kontrol edilir.