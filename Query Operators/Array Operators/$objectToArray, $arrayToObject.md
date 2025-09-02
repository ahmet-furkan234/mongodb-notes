
**Nesne ↔ Dizi** dönüşümleri

Bu iki operatör, MongoDB’de nesne yapısını (object) anahtar-değer çifti içeren dizilere (array) dönüştürmek ve tam tersini yapmak için kullanılır.

---

## 🧩 `$objectToArray`

### 🔹 Tanım:

Bir object’i `{k: key, v: value}` yapısında bir diziye dönüştürür.

### 📌 Söz Dizimi:

```js
{ $objectToArray: "$alanAdı" }
```

---

### 📦 Örnek:

```js
db.getCollection("test").aggregate([
  {
    $project: {
      objToArr: {
        $objectToArray: {
          name: "TkMatE",
          age: 30,
          city: "İstanbul"
        }
      }
    }
  }
])
```

### 🧾 Çıktı:

```json
{
  "objToArr": [
    { "k": "name", "v": "TkMatE" },
    { "k": "age", "v": 30 },
    { "k": "city", "v": "İstanbul" }
  ]
}
```

---

## 🔁 `$arrayToObject`

### 🔹 Tanım:

`{k: key, v: value}` şeklindeki bir diziyi tekrar nesneye çevirir.

### 📌 Söz Dizimi:

```js
{ $arrayToObject: "$alanAdı" }
```

---

### 📦 Örnek:

```js
db.getCollection("test").aggregate([
  {
    $project: {
      arrToObj: {
        $arrayToObject: [
          { k: "name", v: "TkMatE" },
          { k: "age", v: 30 },
          { k: "city", v: "İstanbul" }
        ]
      }
    }
  }
])
```

### 🧾 Çıktı:

```json
{
  "arrToObj": {
    "name": "TkMatE",
    "age": 30,
    "city": "İstanbul"
  }
}
```

---

## 🏷️ Gerçek Kullanım (`USERS` tablosu):

Bazı durumlarda dinamik olarak gelen key’lerle işlem yapmak istersen (örneğin bilinmeyen alan adlarıyla çalışmak), bu operatörler çok işe yarar.

### 🔍 Örnek:

Bir kullanıcı nesnesini parçalayıp, `age` alanını bulup değiştirmek gibi:

```js
db.USERS.aggregate([
  {
    $project: {
      modifiedUser: {
        $arrayToObject: {
          $map: {
            input: { $objectToArray: "$$ROOT" },
            as: "item",
            in: {
              k: "$$item.k",
              v: {
                $cond: [
                  { $eq: ["$$item.k", "AGE"] },
                  { $add: ["$$item.v", 1] },
                  "$$item.v"
                ]
              }
            }
          }
        }
      }
    }
  }
])
```

> Bu örnekte tüm alanları koruyarak sadece `AGE` değerini +1 yapıyoruz.

---

Hazırsan sırada üçüncü operatörümüz **🎲 `$rand`** var. Devam edelim mi?