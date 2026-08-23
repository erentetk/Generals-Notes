
# TryParse

if (int.TryParse("123", out sayi1))
{
    Console.WriteLine(sayi1);
}
```csharp
if (int.TryParse("123", out sayi1))
{
    Console.WriteLine(sayi1);
}
```

Burada amaç, string bir değeri dönüştürürken hem **kontrol mekanizması** kurmak hem de **atama işlemini güvenli şekilde yapmaktır**.  
Kontrol mekanizması, `try-catch` gibi bir hata fırlatıp programı durdurmaz.

İçine girilen string, `int.TryParse` ile `int`’e çevrilmeye çalışılır.  
Bu çevirme işlemi başarılı olursa fonksiyon `true`, başarısız olursa `false` döner.  
Eğer sonuç `true` ise, çevrilen değer `sayi1` değişkenine atanır.

# Implicit Conversion(otomatik dönüşüm) /Explicit Conversion (Manuel dönüşüm)

 - Aradaki temel fark şu ; 
	 Veri kaybı riski yoksa c#cogu zaman bunu kendi yapar. 
	 Veri kaybi riski varsa  c# bizden özellikle söylememizi bekler.


### Implicit Conversion
Bu dönüşüm güvenli kendim yaparım.

```csharp
int a = 5;  
long b = a;  
  
float x = 10;  
double y = x;  
  
char harf = 'A';  
int ascii = harf;
```


### Explicit Conversion 
Burada risk var. Eminsen kendin belirt.

```csharp
double sayi = 10.8;
int tamSayi = (int)sayi;
```

Burada veri kaybı olur tamSayinin degeri 10 olarak yazılır.


### Boxing, Unboxing 

**Boxing**, _value type_ bir değerin, tüm türlerin temel sınıfı olan `object` (veya bir interface) tipine dönüştürülmesidir.

**Unboxing** ise, `object` içinde tutulan bu değerin tekrar kendi _value type_ tipine **cast edilerek geri alınmasıdır**.