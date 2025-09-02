
`$isoWeekYear`, bir `Date` değerine karşılık gelen **ISO hafta yılı** bilgisini döner.  
Yani, **ISO haftaları hangi yıl içindeyse o yılın yıl bilgisini verir.**

---

### Neden önemli?

ISO haftaları bazen yılın ilk gününden farklı bir yıl içinde olabilir.  
Örneğin, 1 Ocak 2021 aslında ISO haftası açısından 2020 yılına ait olabilir.

---

## 📌 Söz Dizimi

```js
{ $isoWeekYear: <tarih ifadesi> }
```

---

## 🧪 1. Temel Kullanım

```js
db.loglar.aggregate([
  {
    $project: {
      tarih: 1,
      isoHaftaYili: { $isoWeekYear: "$tarih" }
    }
  }
])
```

---

## 🧪 2. `new Date()` ile Kullanım

```js
db.rapor.aggregate([
  {
    $project: {
      bugun: new Date(),
      isoHaftaYili: { $isoWeekYear: new Date() }
    }
  }
])
```

---

## 🧪 3. ISO Hafta Yılına Göre Filtreleme

Örnek: ISO hafta yılı 2024 olan kayıtlar

```js
db.loglar.aggregate([
  {
    $match: {
      $expr: { $eq: [ { $isoWeekYear: "$tarih" }, 2024 ] }
    }
  }
])
```

---

## 🧾 4. ISO Hafta Yılı ve Haftaya Göre Gruplama

```js
db.loglar.aggregate([
  {
    $group: {
      _id: {
        isoHaftaYili: { $isoWeekYear: "$tarih" },
        isoHafta: { $isoWeek: "$tarih" }
      },
      toplam: { $sum: 1 }
    }
  },
  { $sort: { "_id.isoHaftaYili": 1, "_id.isoHafta": 1 } }
])
```

---

## ⚠️ Dikkat Edilecek Noktalar

- Tarih tipi `Date` olmalı, değilse `$toDate` ile dönüştür.
    
- ISO hafta yılı, yılın ilk birkaç günü bazen önceki yıl olarak sayılabilir.
    

---

## ✅ Özet

|Özellik|Açıklama|
|---|---|
|`$isoWeekYear`|Tarihin ait olduğu ISO hafta yılını verir|
|ISO haftaları yıl sınırlarından farklı olabilir|Örn: 1 Ocak bazen önceki yıla ait olur|
|Haftalık bazlı yıl hesaplamalarında önemli|✅|

---

Sıradaki tarih fonksiyonlarına geçelim mi?

- ⏳ `$dateDiff`
    
- ➕ `$dateAdd`
    
- ➖ `$dateSubtract`
    
- 🔠 `$dateToString`
    
- 📆 `$dateTrunc`
    

Hangisini istersin TkMatE?