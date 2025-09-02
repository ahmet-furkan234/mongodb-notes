
👉 **Undefined**, MongoDB BSON formatında özel bir veri tipidir (type code `6`).  
➡ Bir alanın değeri _tanımsız_ olduğunda bu tip kullanılır.  
➡ JavaScript’teki `undefined` değişken kavramına karşılık gelir.

---

## 🌟 **Özellikler**

✅ Undefined, bir alanın **var ama değeri tanımlanmamış** olduğu durumu ifade eder.  
✅ Şema esnek olduğu için aslında Undefined kullanılmasına pek gerek kalmaz; alan hiç konmazsa zaten tanımsızdır.  
✅ Undefined bir değeri sorgulayabilirsin:

```js
db.ornek.find({ alan: { $type: "undefined" } })
```

✅ BSON'da `undefined` bir type kodu vardır ama **modern MongoDB sürümlerinde önerilmez** (deprecated sayılır).

---

## 📝 **Örnek Belge**

```json
{
  "_id": 1,
  "ad": undefined
}
```

👉 `ad` alanı var ama tanımlanmamış (Undefined).

> ⚠ Normalde MongoDB’de alanı koymazsan (`"ad"` hiç yazmazsan) aynı sonucu alırsın.

---

## ⚡ **Undefined ile Null farkı**

|Özellik|Undefined|Null|
|---|---|---|
|Alan var mı?|✔ Alan var ama değeri yok|✔ Alan var ve değeri null|
|Sorgu|`$type: "undefined"`|`$type: "null"`|
|Kullanım amacı|Tanımsız / boş bırakılmış|Bilinçli olarak null (boş) atanmış|
|Modern kullanımı|❌ Tavsiye edilmez, eski BSON özelliği|✔ Sık kullanılır|

---

## 🚩 **Neden kullanılmaz?**

✅ MongoDB şema esnek olduğu için alanı hiç koymamak aynı etkiyi sağlar.  
✅ Undefined, BSON standardında var ama pratikte modern uygulamalar bunu kullanmaz.  
✅ Yeni MongoDB sürümleri `undefined`’i desteklese de, alanı hiç koymamak tercih edilir.

---

## 🎯 **Undefined ne zaman karşına çıkar?**

👉 Eski sistemlerden veri taşırken (eski BSON verileri)  
👉 Yanlışlıkla eski API’lerle eklenmiş alanlarda  
👉 Bazen eski driver’larda `undefined` dönebilir

---

## 💡 **Modern Tavsiye**

✅ Alan değeri yoksa hiç koyma:

```json
{
  "_id": 1
}
```

✅ Bilinçli boş değer gerekiyorsa:

```json
{
  "_id": 1,
  "ad": null
}
```