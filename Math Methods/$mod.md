
> `$mod`, bir sayının **diğer sayıya bölümünden kalanı** verir.  
> Söz dizimi:

```js
{ $mod: [ <bölünen>, <bölen> ] }
```

---

## 📁 Örnek Veri: `students`

```js
db.students.insertMany([
  { name: "Ali", score: 87 },
  { name: "Ayşe", score: 92 },
  { name: "Veli", score: 78 },
  { name: "Zeynep", score: 95 },
  { name: "Ahmet", score: 84 }
])
```

---

## 📌 1. Tek / Çift sayı kontrolü

> `score` değeri **tek olanları getir**

```js
db.students.find({
  $expr: {
    $eq: [ { $mod: ["$score", 2] }, 1 ]
  }
})
```

📤 Çıktı:

```json
{ name: "Ali", score: 87 }
{ name: "Zeynep", score: 95 }
```

---

## 📌 2. 5'e tam bölünen skorları bul

```js
db.students.find({
  $expr: {
    $eq: [ { $mod: ["$score", 5] }, 0 ]
  }
})
```

📤 Çıktı:

```json
{ name: "Zeynep", score: 95 }
```

---

## 📌 3. `$project` içinde kalan bilgisi göstermek

```js
db.students.aggregate([
  {
    $project: {
      name: 1,
      score: 1,
      kalan5eGore: { $mod: ["$score", 5] }
    }
  }
])
```

📤 Çıktı:

```json
{ name: "Ali", kalan5eGore: 2 }
{ name: "Ayşe", kalan5eGore: 2 }
{ name: "Veli", kalan5eGore: 3 }
...
```

---

## 📌 4. Günün saatine göre blok atama

> Diyelim ki bir sistemde saat bilgisinden **6 saatlik blokları** hesaplıyorsun (0–5, 6–11, 12–17, 18–23):

```js
db.logs.aggregate([
  {
    $project: {
      hour: { $hour: "$createdAt" },
      block: {
        $mod: [{ $hour: "$createdAt" }, 6]
      }
    }
  }
])
```

---

## 📌 5. `$cond` ile birlikte sınıflama

> Öğrencinin puanı 5'e tam bölünüyorsa "A", değilse "B":

```js
db.students.aggregate([
  {
    $project: {
      name: 1,
      score: 1,
      grade: {
        $cond: {
          if: { $eq: [ { $mod: ["$score", 5] }, 0 ] },
          then: "A",
          else: "B"
        }
      }
    }
  }
])
```

---

## ⚠️ Dikkat Edilecekler

|Durum|Sonuç|
|---|---|
|`null % sayı`|`null`|
|Sayı % `0`|❌ Hata (division by zero)|
|Negatif mod|Sonuç negatif olabilir|
|Float mod|Desteklenir (küsuratlı sonuç döner)|

---

## ✅ Özet

|Kullanım Durumu|Desteklenir|
|---|---|
|Tek / çift sayı tespiti|✅|
|Belirli aralıkta periyot belirleme|✅|
|Tarihsel saat, blok gruplama|✅|
|`mod` sonucu proje etme|✅|
|`mod` ile filtreleme|✅|
|`mod` + `$cond` ile sınıflama|✅|

---

Şimdi istersen devamında:

- 🔄 `$round` (yuvarlama),
    
- 🔃 `$floor`, `$ceil`,
    
- 🎯 `$abs`, `$trunc`,
    
- 📊 `$cond`, `$switch` (koşullu operatörler),
    
- ⏱ `$dateAdd`, `$dateSubtract`, `$dateDiff`
    

gibi operatörlerle ilerleyebiliriz.

Hangisiyle devam edelim TkMatE?