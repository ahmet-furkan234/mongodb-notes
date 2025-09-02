
👉 **Null**, bir alanın bilinçli olarak boş, değersiz veya "değeri yok" olduğunu belirtmek için kullanılan BSON veri türüdür (tip kodu: `0x0A`).  
➡ Yani alanın değeri **bilerek** `null` atanmıştır.

---

## 🌟 **Özellikler**

✅ Alan vardır ancak bir değer içermez (boş)  
✅ JSON ve JavaScript’teki `null` ile aynıdır  
✅ Sorgularda null değerli alanları kolayca bulabilirsin

---

## 📝 **Örnek Belge**

```json
{
  "ad": "Ahmet",
  "telefon": null
}
```

➡ `telefon` alanı var ama değeri bilinçli olarak null atanmış.

---

## ⚡ **Null ile İlgili Sorgular**

### Null olan alanları bulma

```js
db.kullanicilar.find({ telefon: null })
```

➡ `telefon` alanı null olanları getirir.

### Alan değeri olmayanları da dahil etmek için

```js
db.kullanicilar.find({ telefon: { $exists: false } })
```

➡ `telefon` alanı olmayanları getirir.

---

## ⚡ **Null ile Undefined farkı**

|Özellik|Null|Undefined|
|---|---|---|
|Alan var mı?|Evet, alan var ama değeri null|Alan yok ya da tanımsız|
|Anlam|Bilinçli boş değer|Tanımsız, değer atanmamış|
|Kullanım|Eksik veri, bilinçli boşluk|Alanın hiç konmaması tercih edilir|
|Sorgu|`{ alan: null }`|`{ alan: { $type: "undefined" } }`|

---

## 🌟 **SQL ile Kıyas**

|Özellik|MongoDB Null|SQL NULL|
|---|---|---|
|Anlam|Bilinçli boş değer|Aynı şekilde boş (NULL) değer|
|Kullanım|Eksik veya bilinçli boşluk|Eksik bilgi için kullanılır|
|Sorgu|`{ alan: null }`|`WHERE alan IS NULL`|

---

## 🌟 **Avantajları**

✅ Eksik veya bilinçli boş veriyi göstermek için ideal  
✅ Sorgular kolay, standart destekli  
✅ Veri bütünlüğü açısından anlamlı (alan var ama değersiz)

---

## 🚩 **Dezavantajları**

❌ `null` ile alanın olmaması farklıdır, dikkat edilmezse karışıklık olur  
❌ Uygulamada null ve undefined kullanımına dikkat etmek gerekir

---

## 🎯 **Ne zaman kullanılır?**

✅ Bilinçli olarak eksik veya geçici boş değer atamak istediğinde  
✅ Alanın boş olabileceği durumları açıkça göstermek için