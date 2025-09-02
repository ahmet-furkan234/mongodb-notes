
## 📁 Kullanacağımız Koleksiyon: `USERS`

Örnek döküman (temsilî):

```json
{
  "_id": 1,
  "NAME": "Ahmet",
  "SURNAME": "Yılmaz",
  "EMAIL": "ahmet@example.com",
  "CITY": "Ankara",
  "AGE": 25
}
```

---

## 🔍 1. `$objectToArray` — Kullanıcı bilgilerini diziye dönüştürme

### 🎯 Amaç:

Her bir kullanıcıyı `{k: key, v: value}` formatında bir diziye çevirelim.

### 🧪 Sorgu:

```js
db.USERS.aggregate([
  {
    $project: {
      userAsArray: {
        $objectToArray: "$$ROOT"
      }
    }
  }
])
```

### 📤 Örnek çıktı:

```json
{
  "userAsArray": [
    { "k": "_id", "v": 1 },
    { "k": "NAME", "v": "Ahmet" },
    { "k": "SURNAME", "v": "Yılmaz" },
    { "k": "EMAIL", "v": "ahmet@example.com" },
    { "k": "CITY", "v": "Ankara" },
    { "k": "AGE", "v": 25 }
  ]
}
```

---

## 🔁 2. `$arrayToObject` — Dizi formatındaki kullanıcıyı tekrar nesneye çevirme

Bunu yapabilmek için ya önceki çıktıyı kullanırız ya da elimizde örnek dizi varsa onunla çalışırız.

### 🧪 Örnek:

```js
db.USERS.aggregate([
  {
    $project: {
      arrayToObj: {
        $arrayToObject: [
          { k: "NAME", v: "Zeynep" },
          { k: "AGE", v: 22 },
          { k: "CITY", v: "İzmir" }
        ]
      }
    }
  }
])
```

### 📤 Çıktı:

```json
{
  "arrayToObj": {
    "NAME": "Zeynep",
    "AGE": 22,
    "CITY": "İzmir"
  }
}
```

---

## 🔄 3. Gerçek Senaryo: Sadece `AGE` değerini güncelle, diğerlerini olduğu gibi koru

### 🎯 Amaç:

- Kullanıcı objesini diziye çevir
    
- Eğer `AGE` alanı varsa, 1 artır
    
- Sonra tekrar nesneye çevir
    

### 🧪 Sorgu:

```js
db.USERS.aggregate([
  {
    $project: {
      updatedUser: {
        $arrayToObject: {
          $map: {
            input: { $objectToArray: "$$ROOT" },
            as: "field",
            in: {
              k: "$$field.k",
              v: {
                $cond: [
                  { $eq: ["$$field.k", "AGE"] },
                  { $add: ["$$field.v", 1] },
                  "$$field.v"
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

### 📤 Çıktı (örnek):

```json
{
  "updatedUser": {
    "_id": 1,
    "NAME": "Ahmet",
    "SURNAME": "Yılmaz",
    "EMAIL": "ahmet@example.com",
    "CITY": "Ankara",
    "AGE": 26
  }
}
```

---

İstersen şimdi `$rand` operatörüyle devam edelim. O da çok ilginç ve az bilinir bir operatördür — özellikle test verisi üretmede harikadır. Devam edelim mi?