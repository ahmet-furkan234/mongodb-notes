
> `$cbrt`, verilen sayının **karekökü değil**, **küpkökünü** hesaplar.  
> Matematiksel olarak:  
> x3\sqrt[3]{x}

📌 Söz dizimi:

```js
{ $cbrt: <sayısal ifade> }
```

---

## 📁 Örnek Veri: `numbers`

```js
db.numbers.insertMany([
  { value: 8 },
  { value: 27 },
  { value: 64 },
  { value: 125 },
  { value: -27 }
])
```

---

## 📌 1. Küpkök alma

```js
db.numbers.aggregate([
  {
    $project: {
      value: 1,
      cubeRoot: { $cbrt: "$value" }
    }
  }
])
```

📤 Çıktı:

```json
{ value: 8, cubeRoot: 2 }
{ value: 27, cubeRoot: 3 }
{ value: 64, cubeRoot: 4 }
{ value: 125, cubeRoot: 5 }
{ value: -27, cubeRoot: -3 }
```

---

## 📌 2. Küpkök sonucu yuvarlama

```js
db.numbers.aggregate([
  {
    $project: {
      value: 1,
      cubeRootRounded: {
        $round: [
          { $cbrt: "$value" },
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
|Pozitif ve negatif sayı|Sorunsuz çalışır|
|`null` veya `string`|Hata verir|

---

## ✅ Özet

|Özellik|Desteklenir|
|---|---|
|Pozitif ve negatif sayının küpkökü|✅|
|Hesaplama sonrası yuvarlama|✅|
|Aggregation pipeline’de kullanılabilir|✅|
