
Eğer bir **update komutunda** doğrudan bir nesne verirsen (örneğin `$set`, `$inc`, `$push` gibi bir operatör kullanmadan) bu durumda **o belge tamamen verilen nesneyle değiştirilir**.

Yani:  
✅ Operatör kullandığında: Yalnızca belirttiğin alanlar güncellenir.  
⚠ **Operatör kullanmazsan:** Belge tamamen silinir ve senin verdiğin alanlarla baştan oluşturulur.

---

## 🔑 Örnek

### 1️⃣ **Operatörsüz update**

```js
db.users.updateOne(
  { username: "ali" },
  { username: "ali", city: "Ankara" }
)
```

📌 Bu durumda:

- `username: "ali"` olan belge bulundu.
- Belge tamamen şu hale gelir:

```json
{
  "_id": ObjectId("..."), 
  "username": "ali",
  "city": "Ankara"
}
```

👉 **Diğer tüm alanlar (ör. `age`, `tags`, `loginCount`) silinir.**

---

### 2️⃣ **Operatör ile update**

```js
db.users.updateOne(
  { username: "ali" },
  { $set: { city: "Ankara" } }
)
```

📌 Bu durumda:

- Sadece `city` alanı güncellenir veya eklenir.
- Diğer alanlara dokunulmaz.

---

## 💡 Neden dikkatli olmalıyız?

MongoDB'de `$set` ya da diğer update operatörleri **veriyi parça parça günceller**. Ama operatör kullanmadan update yazarsan, Mongo bunu **replace** gibi algılar → eski belgeyi silip, yeni verdiğin belgeyi baştan yazar.

---

## 📝 Kural:

👉 Eğer _belgenin sadece bazı alanlarını değiştirmek_ istiyorsan:  
✅ `$set`, `$inc`, `$push`, `$unset`, `$addToSet` gibi operatörler kullan!

👉 Eğer _belgeyi tamamen değiştirmek_ istiyorsan:  
✅ `replaceOne()` kullan (daha açık olur ve niyetini belli edersin).

---

## 📌 Hızlı karşılaştırma

|Yapı|Ne yapar?|
|---|---|
|`{ $set: { age: 30 } }`|Sadece `age` alanını 30 yapar.|
|`{ age: 30 }`|Belgeyi tamamen şu hale getirir: `{ age: 30 }` (diğer alanlar gider)|
