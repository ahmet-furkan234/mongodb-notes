
👉 **Boolean**, bir alanın değerinin _true_ (**doğru**) veya _false_ (**yanlış**) olmasını sağlayan bir BSON veri tipidir.

➡ Sadece 2 geçerli değer alır:

```json
{
  "aktif": true
}
```

veya

```json
{
  "aktif": false
}
```

---

## 🌟 **Özellikler**

✅ Boolean değerler doğrudan `true` veya `false` olarak saklanır.  
✅ Sorgular, güncellemeler ve filtrelerde kolayca kullanılabilir.  
✅ JSON, BSON ve programlama dillerindeki boolean türleriyle doğrudan uyumludur.

---

## 📝 **Örnek Belge**

```json
{
  "ad": "Ahmet",
  "aktif": true,
  "abone": false
}
```

➡ `aktif` alanı doğru (true), `abone` alanı yanlış (false).

---

## ⚡ **Boolean ile Sorgu Örneği**

### 1️⃣ `true` olan belgeleri getir

```js
db.kisiler.find({ "aktif": true })
```

### 2️⃣ `false` olan belgeleri getir

```js
db.kisiler.find({ "abone": false })
```

### 3️⃣ Boolean değeri olan belgeleri getir

```js
db.kisiler.find({ "aktif": { $type: "bool" } })
```

➡ `aktif` alanı gerçekten boolean olan belgeler gelir.

---

## ⚠ **Dikkat!**

MongoDB’de _true_ ve _false_ açıkça boolean türünde tutulmalıdır.  
❌ `"true"` (string) ve `true` (boolean) aynı şey değildir!

```js
db.kisiler.find({ "aktif": "true" })  // eşleşmez, çünkü string
db.kisiler.find({ "aktif": true })    // doğru eşleşme
```

---

## 🌟 **SQL ile Kıyas**

|Özellik|MongoDB Boolean|SQL Boolean|
|---|---|---|
|Tip|`bool` (true / false)|`BOOLEAN`, `TINYINT(1)` (MySQL), `BIT`, `BOOL`|
|Değerler|true / false|1 / 0 veya true / false|
|Varsayılan atama|✔ Belgeye eklenirken belirlenir|✔ Kolon tipi belirlenir, default atanabilir|
|Kullanım|JSON uyumlu, doğrudan true / false|Genelde 1 / 0 olarak tutulur (özellikle MySQL)|

---

## 🌟 **Avantajlar**

✅ Basit ve net (sadece 2 değer: true / false)  
✅ JSON ve uygulama kodlarıyla doğrudan uyumlu  
✅ Filtre, index, aggregation’da kolay kullanım

---

## 🚩 **Dezavantajlar**

❌ Yanlışlıkla string `"true"` gibi bir değer koyarsan sorgular beklenmedik sonuç dönebilir  
❌ Boolean için ayrı bir index kullanmak genelde mantıklı olmaz (çok az farklı değer olduğu için index fayda sağlamaz)

---

## 🎯 **Ne zaman kullanılır?**

✅ Alanın sadece _var/yok_, _açık/kapalı_, _aktif/pasif_ gibi 2 değerli durumu varsa  
✅ Flag (bayrak) göstermek için (ör. `isDeleted`, `isActive`, `hasPremium`)

---

💬 **İstersen:**

- Boolean ile karmaşık sorgular (ör. true ve false birlikte filtreleme) örnekleyeyim
- SQL ve Mongo boolean kullanımını aynı örnekle yan yana yazayım
- Aggregation ile boolean alanları sayma örneği vereyim

👉 Nasıl devam edelim TkMatE? 🚀