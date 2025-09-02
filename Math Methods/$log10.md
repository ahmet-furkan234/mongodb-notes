
> `$log10`, verilen sayının **10 tabanındaki logaritmasını** hesaplar.  
> Matematikte sıkça kullanılan logaritma türüdür.

📌 Söz dizimi:

```js
{ $log10: <pozitif sayı veya ifade> }
```

---

## 📁 Örnek Veri: `values`

```js
db.values.insertMany([
  { number: 1 },
  { number: 10 },
  { number: 100 },
  { number: 1000 }
])
```

---

## 📌 1. Taban 10 logaritmasını hesapla

```js
db.values.aggregate([
  {
    $project: {
      number: 1,
      log10Value: { $log10: "$number" }
    }
  }
])
```

📤 Çıktı:

```json
{ number: 1, log10Value: 0 }
{ number: 10, log10Value: 1 }
{ number: 100, log10Value: 2 }
{ number: 1000, log10Value: 3 }
```

---

## 📌 2. Hesaplama sonrası yuvarlama

```js
db.values.aggregate([
  {
    $project: {
      number: 1,
      log10Rounded: {
        $round: [
          { $log10: "$number" },
          2
        ]
      }
    }
  }
])
```

---

## ⚠️ Dikkat Edilecekler

|Durum|Etki|
|---|---|
|`number <= 0`|Hata verir (logaritma tanımsız)|
|`null` veya `string`|Hata verir|
|Pozitif sayı|Sorunsuz çalışır|

---

## ✅ Özet

|Özellik|Desteklenir|
|---|---|
|10 tabanında logaritma hesaplama|✅|
|Hesaplama sonrası yuvarlama|✅|
|Aggregation pipeline’de kullanım|✅|

---

## 📚 Kullanım Alanları

- Bilim ve mühendislikte ölçüm dönüşümlerinde
- Ses yoğunluğu, pH ölçümü gibi logaritmik ölçeklerde
- Veri analizinde ölçeklendirme
