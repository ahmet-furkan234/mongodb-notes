
**`$isoDayOfWeek`**, bir `Date` değerinden haftanın kaçıncı günü olduğunu **ISO 8601 standardına göre** döner:

|Gün|Sayı|
|---|---|
|**Pazartesi**|1|
|Salı|2|
|Çarşamba|3|
|Perşembe|4|
|Cuma|5|
|Cumartesi|6|
|**Pazar**|7|

> Bu, klasik `$dayOfWeek`'ten farklıdır. `$dayOfWeek` Pazar'ı 1 kabul ederken, `$isoDayOfWeek` **Pazartesi’yi 1** kabul eder.

---

## 📌 Söz Dizimi

```js
{ $isoDayOfWeek: <tarih ifadesi> }
```

---

## 🧪 1. Temel Kullanım – ISO Gün Numarası Çekme

```js
db.loglar.aggregate([
  {
    $project: {
      mesaj: 1,
      gun: { $isoDayOfWeek: "$tarih" }
    }
  }
])
```

### Örnek veri:

```json
{ mesaj: "Giriş yapıldı", tarih: ISODate("2025-07-14T12:00:00Z") }
```

📌 Çıktı:

```json
{ mesaj: "Giriş yapıldı", gun: 1 }
```

✅ 14 Temmuz 2025 = Pazartesi → ISO günü: **1**

---

## 🧪 2. `new Date()` ile Bugünün ISO Günü

```js
db.now.aggregate([
  {
    $project: {
      bugun: new Date(),
      isoGun: { $isoDayOfWeek: new Date() }
    }
  }
])
```

---

## 🧪 3. ISO Günüyle Filtreleme – (örneğin Cuma)

```js
db.loglar.aggregate([
  {
    $match: {
      $expr: { $eq: [ { $isoDayOfWeek: "$tarih" }, 5 ] }
    }
  }
])
```

> Bu sorgu Cuma günü yapılan kayıtları getirir.

---

## 🧾 4. ISO Gününe Göre Gruplama – En Yoğun Gün

```js
db.loglar.aggregate([
  {
    $group: {
      _id: { isoGun: { $isoDayOfWeek: "$tarih" } },
      sayi: { $sum: 1 }
    }
  },
  { $sort: { sayi: -1 } }
])
```

📌 Pazartesi → 1, Pazar → 7

---

## 🔁 Karşılaştırma: `$dayOfWeek` vs `$isoDayOfWeek`

|Operatör|Pazartesi|Pazar|Açıklama|
|---|---|---|---|
|`$dayOfWeek`|2|1|Amerikan haftası başlar|
|`$isoDayOfWeek`|1|7|**ISO 8601 standardı** ✅|

> 📌 Avrupa sistemine veya iş haftası analizine uygunsa `isoDayOfWeek` tercih edilmelidir.

---

## ⚠️ Dikkat Edilecek Noktalar

- Tarih değeri mutlaka `Date` tipi olmalı.
    
- Saat dilimi UTC'ye göre yorumlanır.
    
- Pazartesi = 1, Pazar = 7
    

---

## ✅ Özet

|Özellik|Açıklama|
|---|---|
|`$isoDayOfWeek`|ISO 8601’e göre haftanın günü|
|Pazartesi = 1|✅|
|Pazar = 7|✅|
|Haftalık analizlerde idealdir|✅|
|`$dayOfWeek`’e alternatif|Avrupa standartlarında tercih edilir|

---

Hazırsan sıradaki tarih operatörüne geçebiliriz:

- 🔢 `$week` → yılın kaçıncı haftası
    
- ⏳ `$dateDiff` → iki tarih arası fark
    
- ➕ `$dateAdd` → tarihe ekleme yap
    
- ➖ `$dateSubtract` → tarihten çıkarma
    
- 🔠 `$dateToString` → tarihi yazıya çevirme
    
- 📆 `$dateTrunc` → tarihi örneğin haftaya/aya yuvarlama
    

Hangisinden devam edelim TkMatE?