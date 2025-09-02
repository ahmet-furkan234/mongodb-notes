
`$mergeObjects`, iki veya daha fazla **object (belge/alan)** birleştirmek için kullanılır. Aynı alan ismi varsa **sondaki** kazanır (overwrite eder).

---

## 📌 Söz Dizimi (Syntax):

```js
{ $mergeObjects: [ obj1, obj2, obj3, ... ] }
```

---

## 📦 Basit Örnek:

```js
db.getCollection("test").aggregate([
  {
    $project: {
      merged: {
        $mergeObjects: [
          { name: "TkMatE", age: 30 },
          { age: 32, city: "Ankara" }
        ]
      }
    }
  }
])
```

### 🧾 Çıktı:

```json
{
  "merged": {
    "name": "TkMatE",
    "age": 32,
    "city": "Ankara"
  }
}
```

> Dikkat: `age: 30` önce gelmesine rağmen, `age: 32` sonradan geldiği için değeri üzerine yazdı.

---

## 🏷️ Gerçek Veriyle Örnek (`etrade_rdms` → `USERS` + `ADDRESS`)

### Kullanım Senaryosu:

Bir kullanıcıya ait bilgilerle adres verilerini birleştirip tek nesne yapmak:

```js
db.getCollection("USERS").aggregate([
  {
    $lookup: {
      from: "ADDRESS",
      localField: "ID",
      foreignField: "USERID",
      as: "addr"
    }
  },
  {
    $unwind: "$addr"
  },
  {
    $project: {
      _id: 0,
      userWithAddress: {
        $mergeObjects: ["$$ROOT", "$addr"]
      }
    }
  }
])
```

### ✅ Ne yapar?

- Kullanıcı bilgisi (`USERS`)
    
- Adres bilgisi (`ADDRESS`)
    
- Hepsini **tek bir nesnede birleştirir.**
    

---

## 💡 Notlar:

- `null` veya olmayan alanlar geçerli nesne değilse yok sayılır.
    
- Diziler birleştirilmez — sadece nesneler birleştirilir.
    
- `$mergeObjects` içinde `$project`, `$replaceRoot`, `$addFields` gibi yerlerde kullanılabilir.
    

---

Hazırsan sırada `📘 $objectToArray ve $arrayToObject` var. Devam edelim mi?