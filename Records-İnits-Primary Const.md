# Init-Only Properties

C# 9.0 ile gelen `init`, bir property’ye **sadece nesne oluşturulurken** değer verilmesini sağlar.  
Nesne oluşturulduktan sonra artık değiştirilemez.
```csharp
public class Yemek  
{  
    public int Adet { get; init; } = 3;  
    public decimal Fiyat { get; }  
  
    public Yemek(decimal fiyat)  
    {  
        Fiyat = fiyat;  
    }  
}
```
Kullanımı:
```csharp
var yemek = new Yemek(25)  
{  
    Adet = 5  
};
```
Burada:

- `Fiyat` → constructor ile atanır
- `Adet` → object initializer ile atanır

İkisi de ilk oluşturulma anında değer alır, fakat yöntemleri farklıdır.

## Temel fark

### `get;`
```csharp
public decimal Fiyat { get; }
```
- Dışarıdan değer verilemez
- Genelde sadece constructor içinde atanır
- Değerin hangi yoldan geleceğini sınıf belirler

### `get; init;`
```csharp
public int Adet { get; init; }
```
- Nesne oluşturulurken dışarıdan değer verilebilir
- Sonradan değiştirilemez
```csharp
var yemek = new Yemek(25) { Adet = 5 };  
// yemek.Adet = 10; // hata
```
## Neden `init` geldi?

Eskiden iki seçenek vardı:

### 1. `set` kullanmak

Kolay yazılır ama nesne her zaman değiştirilebilir.

### 2. Sadece `get` + constructor kullanmak

Daha güvenlidir ama property sayısı arttıkça constructor kullanımı karmaşıklaşır.

`init`, bu iki yaklaşım arasında bir çözüm sundu:

- `object initializer` kadar okunaklı
- `set` kadar serbest değil
- nesne oluşturulduktan sonra değişikliği engeller

## Kısaca

`init` özelliği, **nesneyi oluştururken rahat değer vermeyi** sağlar;  
ama **oluştuktan sonra değiştirilmeyi engeller**.

Tabii. DTO’larda `init` niye rahatlık sağlıyor, en net **çok property’li örnekle** görünür.

## 1. Constructor ile kullanım
```csharp
public class CreateUserDto  
{  
    public string FirstName { get; }  
    public string LastName { get; }  
    public string Email { get; }  
    public string Phone { get; }  
    public int Age { get; }  
    public string City { get; }  
    public string Country { get; }  
    public string Department { get; }  
    public bool IsActive { get; }  
    public string Role { get; }  
  
    public CreateUserDto(  
        string firstName,  
        string lastName,  
        string email,  
        string phone,  
        int age,  
        string city,  
        string country,  
        string department,  
        bool isActive,  
        string role)  
    {  
        FirstName = firstName;  
        LastName = lastName;  
        Email = email;  
        Phone = phone;  
        Age = age;  
        City = city;  
        Country = country;  
        Department = department;  
        IsActive = isActive;  
        Role = role;  
    }  
}
```

Kullanımı:
```csharp
var userDto = new CreateUserDto(  
    "Ahmet",  
    "Yılmaz",  
    "ahmet@example.com",  
    "05551112233",  
    28,  
    "İstanbul",  
    "Türkiye",  
    "Yazılım",  
    true,  
    "Admin"  
);```
