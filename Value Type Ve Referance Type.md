# Value type
Veriyi doğrudan taşıyan ve taşıdıgı veriye kadar bellekte yer kaplar. Belleklte stack(yıgın) bölgesinde bulunur. Verilere burada görece hızlı erişilir.
Bir değer tipi başka bir değişkene atandığında veya bir metoda parametre olarak gönderildiğinde, **verinin kendisi kopyalanır**. Bu nedenle, kopya üzerinde yapılan bir değişiklik orijinal değişkeni etkilemez

`int`, `long`, `float`, `double`, `decimal`, `char`, `bool`, `byte`, `short`, `struct` ve `enum`

Bunalar ilkel (primitive ) veri tipleridir. Bunlar orogramlama dilleri tarafında önceden ayrılmış  temel veri türleridir. Nesne olara kabul edilmezler  ve veriyi doğrudan taşıma özelliğine sahiptir.

# Referance Type

Bir kopyalama veya atama işleminde verinin bellekteki adresi kopyalanır.  Ve her hangi  bir degişiklikte iki degişkeninde  baglı bulunduğu  bellekteki adres değişir. Bellekte yazıldıgı yer Heap tir. Heap'in erişimi görece yavaştır.
NULL işaretlendiğinde bellekte bir adres işaret etmez .

`string`, `object`, `class`, `interface`, `array` (diziler), `delegate` ve `pointer`



#Nullable türler, normalde `null` değeri alamayan değer tiplerinin (`int`, `bool`, `struct` vb.) hem kendi değerlerini hem de `null` değerini temsil etmesini sağlayan yapılardır.  
C#’ta bu yapı `System.Nullable<T>` ile sağlanır ve `T?` ya da `Nullable<T>` şeklinde yazılabilir.  
Örneğin `int?`, `int` türünün nullable halidir.  
Reference type’lar ise zaten temel olarak `null` alabilir; bu nedenle klasik anlamda `Nullable<T>` daha çok değer tipleri için kullanılır.