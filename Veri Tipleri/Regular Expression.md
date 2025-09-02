
👉 **Regular Expression (Regex)**, metin verilerinde desen eşleştirme ve arama yapmak için kullanılan güçlü bir araçtır.  
👉 MongoDB’de regex tipinde alan veya sorgu yaparak belirli desenlere uyan metinleri kolayca bulabilirsin.

---

## 🌟 **Özellikler**

✅ Metin içindeki belli karakter dizilerini, kalıpları aramak için kullanılır  
✅ Büyük/küçük harf duyarlılığı ayarlanabilir  
✅ Perl uyumlu (PCRE) düzenli ifadeler desteklenir  
✅ BSON’da özel bir tip olarak saklanmaz, sorgu ifadesinde regex olarak kullanılır

---

## 📝 **Regex Kullanımı**

### 1️⃣ **Belirli deseni sorgulama**

```js
db.kullanicilar.find({ ad: /Ahmet/ })
```

➡ `ad` alanında "Ahmet" geçen tüm kayıtları getirir.

---

### 2️⃣ **Büyük/küçük harf duyarsız arama**

```js
db.kullanicilar.find({ ad: /ahmet/i })
```

➡ "Ahmet", "ahmet", "AHMET" gibi hepsi bulunur (i = ignore case).

---

### 3️⃣ **Regex operatörü ile sorgu**

```js
db.kullanicilar.find({ ad: { $regex: "^A", $options: "i" } })
```


➡ `ad` alanı “A” ile başlayan kayıtlar (`^` = başlangıç).

---

## ⚡ **Regex ile String Sorgulama Avantajları**

- Esnek ve güçlü desen eşleştirme
- Çok çeşitli kalıplar kurulabilir (başlangıç, bitiş, içerik, alternatifler)
- Karmaşık aramalarda SQL LIKE’den çok daha etkili

---

## 🌟 **SQL ile Kıyas**

|Özellik|MongoDB Regex|SQL LIKE / Regex|
|---|---|---|
|Tip|Regex nesnesi / sorgu ifadesi|LIKE (basit), Regex (bazı DB'lerde)|
|Esneklik|Çok yüksek (PCRE uyumlu)|LIKE: sınırlı (yüzeysel), Regex destekleyenlerde benzer|
|Performans|Index destekliyorsa hızlı|LIKE genelde yavaştır, Regex destek değişir|
|Kullanım|Sorgu içinde regex objesi veya string|WHERE alan LIKE '%abc%' veya Regex fonksiyonu|

---

## 🚩 **Regex Dezavantajları**

❌ Büyük veri üzerinde regex sorgular yavaş olabilir  
❌ Index kullanımı sınırlıdır, sadece baştan eşleşen desenlerde hızlı  
❌ Karmaşık regex ifadesi yazımı zor olabilir, performans risklidir

---

## 🎯 **Ne zaman kullanılır?**

✅ Esnek metin aramaları gerektiğinde (isim, e-posta, kod, metin içeriği)  
✅ Tam metin arama yerine belirli kalıplara göre filtreleme yaparken  
✅ SQL LIKE yetersiz kaldığında

---

## 💡 **Regex Örnekleri**

|Regex|Açıklama|Örnek Sorgu|
|---|---|---|
|`/^A/`|A ile başlayan|`{ ad: /^A/ }`|
|`/abc$/`|abc ile biten|`{ kod: /abc$/ }`|
|`/a.c/`|a ile c arasında herhangi bir karakter|`{ ad: /a.c/ }`|
|`/\d+/`|Bir veya daha fazla rakam|`{ numara: /\d+/ }`|
|`/^.{5}$/`|Tam 5 karakter uzunluğunda|`{ parola: /^.{5}$/ }`|