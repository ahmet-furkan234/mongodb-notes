
👉 **JavaScript (BSON type code: `0x0D`)**, MongoDB’nin BSON formatında bir belge içinde doğrudan JavaScript kodu (fonksiyon) saklamanı sağlar.  
👉 İsteğe bağlı olarak, fonksiyonla birlikte bir _scope_ (değişken ortamı) da saklanabilir (type code: `0x0F`).

➡ Yani bir alan değeri olarak JavaScript fonksiyonu barındırabilirsin.

---

## 🌟 **Nasıl görünür?**

```json
{
  "ad": "Ahmet",
  "kural": function(x) { return x > 5; }
}
```

👉 Bu belge içinde `kural` bir JavaScript fonksiyonudur.

---

## ⚡ **Nerede kullanılır?**

✅ MapReduce işlemlerinde  
✅ `$where` operatöründe (belgeyi sorgularken koşul yazmak için)  
✅ Shell'de karmaşık sorgular yazarken

---

## 📝 **Örnek Sorgu - `$where` ile JavaScript kodu kullanmak**

```
db.kullanicilar.find({
  $where: function() {
    return this.yas > 18;
  }
})
```

👉 JavaScript fonksiyonu ile her belgeyi kontrol eder.

---

## ⚡ **Scope ile JavaScript**

JavaScript kodu ile birlikte bir değişken ortamı (scope) da saklanabilir:

```json
{
  "kod": {
    "$code": "function(x) { return x + y; }",
    "$scope": { "y": 10 }
  }
}
```

👉 Burada `y` fonksiyonun çalışacağı ortamda 10 olarak belirlenmiş.

---

## 🌟 **Avantajları**

✅ Karmaşık koşulları doğrudan belge içinde tanımlayabilirsin  
✅ MapReduce ve $where ile esnek hesaplamalar yapabilirsin  
✅ Dinamik sorgular ve hesaplamalar mümkün

---

## 🚩 **Dezavantajları**

❌ Performansı düşürür (her belgeye fonksiyon çalıştırmak maliyetlidir)  
❌ Index kullanılamaz (JavaScript ile yapılan sorgular indekslerden yararlanamaz)  
❌ Güvenlik riski (shell veya istemci tarafında zararlı kod çalıştırılabilir)  
❌ Modern uygulamalarda önerilmez; aggregation ve built-in operatörler tercih edilir

---

## 🌟 **SQL ile Kıyas**

SQL’de belge içinde kod tutmak mümkün değildir. Ama:

|MongoDB JavaScript|SQL Karşılığı|
|---|---|
|Belge içinde JS fonksiyonu|SQL fonksiyon (stored procedure)|
|Sorguda JS kodu çalıştırma ($where)|WHERE içinde SQL fonksiyonu / stored proc çağrısı|
|MapReduce’da JS|SQL aggregate fonksiyonları|

---

## 🎯 **Ne zaman kullanılır?**

✅ Çok özel hesaplamalar gerektiğinde  
✅ MapReduce gibi toplu işlemlerde  
✅ Prototip aşamasında esnek sorgular denemek için

---

## ⚠ **Modern Tavsiye**

✅ **$where ve JS kullanımı minimize edilmelidir.**  
✅ Daha hızlı ve güvenli olan _aggregation framework_ ve _native sorgular_ tercih edilmelidir.