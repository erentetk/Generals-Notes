
- Koddaki sınıfları sınıflandırmak için kullanılır. 
**Aynı namespace içinde aynı isimde iki class bulunamaz.**  
Ama **farklı namespace’lerde aynı isimde class bulunabilir.**

- Fiziksel bir dosyalama yapmaz sadece bir alan adıdır.
- Benzer yapıları aynı gurup altında toplamak 

```csharp
namespace Services  
{  
	public class BookManager  
	{  
	}  
}
```

```csharp

namespace Services  
{  
	public class BookManager  
	{  
	}  
}
```


```csharp
Services.BookManager  
Admin.BookManager
```

Bu şekilde  aynı isimde birden falza  sınıf tanımlayabiliriz.
