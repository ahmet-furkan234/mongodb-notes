
MongoDB’de belgeleri silmek için iki temel method kullanılır:

|Metot|Ne Yapar?|
|---|---|
|`deleteOne()`|Eşleşen **ilk** belgeyi siler|
|`deleteMany()`|Eşleşen **tüm** belgeleri siler|

---

## 1️⃣ `deleteOne()`

- **Amaç:** Belirtilen filtreyle eşleşen **ilk belgeyi siler**.
- **Kullanımı:**

```js
db.collection.deleteOne(<filter>)
```

- **Örnek:**

```js
db.users.deleteOne({ username: "ahmet" })
```

> `username: "ahmet"` olan ilk belge silinir. Eğer birden fazla varsa sadece **biri** silinir.

---

## 2️⃣ `deleteMany()`

- **Amaç:** Belirtilen filtreyle eşleşen **tüm belgeleri siler**.
- **Kullanımı:**

```js
db.collection.deleteMany(<filter>)
```

- **Örnek:**

```js
db.users.deleteMany({ status: "inactive" })
```

> `status` değeri `"inactive"` olan **tüm kullanıcılar** silinir.

---

## 3️⃣ ❗ Filtre Kullanmadan Silmek

### `deleteMany({})` — Tüm Belgeleri Siler!

```js
db.users.deleteMany({})
```

> **Tüm belgeleri siler!**  
> Bu, koleksiyonu boşaltmak için kullanılır. Çok dikkatli kullanılmalıdır!

---

## 4️⃣ Silme Sonucu (`DeletedCount`)

Hem `deleteOne` hem de `deleteMany` işlemleri sonunda bir sonuç döner:

```js
{ "acknowledged" : true, "deletedCount" : 1 }
```

> `deletedCount`, kaç adet belgenin silindiğini belirtir.

---

## 5️⃣ `findOneAndDelete()`

- **Amaç:** Eşleşen ilk belgeyi siler ve **silen belgeyi geri döner**.
- **Kullanımı:**

```js
db.collection.findOneAndDelete(<filter>)
```

- **Örnek:**

```js
db.users.findOneAndDelete({ username: "mehmet" })
```

> `username: "mehmet"` olan belge silinir ve o belgenin içeriği geri döner.

---

# 📌 Farkların Tablosu

|Yöntem|Ne siler?|Geri dönen bilgi|
|---|---|---|
|`deleteOne()`|İlk eşleşen belge|Silinen belge sayısı|
|`deleteMany()`|Tüm eşleşen belgeler|Silinen belge sayısı|
|`findOneAndDelete()`|İlk eşleşen belge|Silinen belgenin içeriği|

---

## 🔐 Örnek Senaryolar

### 1. Kullanıcı sil

```js
db.users.deleteOne({ email: "a@b.com" })
```

### 2. 18 yaşından küçük tüm kullanıcıları sil

```js
db.users.deleteMany({ age: { $lt: 18 } })
```

### 3. Silmeden önce belgeyi görmek

```js
const silinen = db.users.findOneAndDelete({ username: "ayse" })
console.log(silinen)
```

---

## 🛑 Uyarı: Filtre Kullanmadan Silme

```js
db.users.deleteMany({})  // Tüm belgeleri siler!
```

Bunu kullanmadan önce emin ol. Alternatif olarak `drop()` ile koleksiyonu tamamen silebilirsin:

```js
db.users.drop()
```

> Bu, tabloyu (koleksiyonu) komple yok eder.

---

MongoDB’de bir collection içindeki **belirli bir alanı olmayan** dökümanları silmek için `deleteMany()` ile `$exists` operatörünü kullanabilirsin.

Örneğin:

```js
db.collection.deleteMany({   
	alanAdi: { $exists: false } 
});
```