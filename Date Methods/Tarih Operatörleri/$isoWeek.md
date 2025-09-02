
`$isoWeek`, bir `Date` değerinden **yılın ISO haftası numarasını** (1-53) döner.

- ISO haftaları Pazartesi başlar.
- Haftalar yılın ilk Perşembesi içeren haftası olarak sayılır.
- Haftalar 1-53 arası numaralandırılır.

---

## 📌 Söz Dizimi

```js
{ $isoWeek: <tarih ifadesi> }
```

---

## 🧪 1. Temel Kullanım – ISO Hafta Numarası Alma

```js
db.loglar.aggregate([
  {
    $project: {
      mesaj: 1,
      isoHafta: { $isoWeek: "$tarih" }
    }
  }
])
```

### Örnek veri:

```json
{ mesaj: "İşlem yapıldı", tarih: ISODate("2025-01-02T10:00:00Z") }
```

📌 Çıktı:

```json
{ mesaj: "İşlem yapıldı", isoHafta: 1 }
```

---

## 🧪 2. `new Date()` ile Şu Anki ISO Hafta Numarası

```js
db.rapor.aggregate([
  {
    $project: {
      bugun: new Date(),
      isoHafta: { $isoWeek: new Date() }
    }
  }
])
```

---

## 🧪 3. ISO Hafta Numarasına Göre Filtreleme

### Örnek: 2025 yılının 10. haftasındaki kayıtlar:

```js
db.loglar.aggregate([
  {
    $match: {
      $expr: { $eq: [ { $isoWeek: "$tarih" }, 10 ] }
    }
  }
])
```

---

## 🧾 4. ISO Hafta Bazlı Gruplama

```js
db.loglar.aggregate([
  {
    $group: {
      _id: { isoHafta: { $isoWeek: "$tarih" } },
      toplam: { $sum: 1 }
    }
  },
  { $sort: { "_id.isoHafta": 1 } }
])
```

---

## ⚠️ Dikkat Edilecek Noktalar

|Konu|Açıklama|
|---|---|
|Tarih tipi `Date` olmalı|Değilse `$toDate` ile dönüştür|
|ISO haftası Pazartesi başlar|Haftalar 1-53 arasıdır|
|Haftanın yıl içine düşme durumu|Bazı yıllar 53 hafta olabilir|

---

## ✅ Özet

|Özellik|Açıklama|
|---|---|
|`$isoWeek`|Tarihin ISO haftası numarasını verir|
|Pazartesi haftanın ilk günü|✅|
|Haftalar 1-53 arası|✅|
|Yıl bazlı haftalık raporlar için ideal|✅|
