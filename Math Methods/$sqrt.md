
> `$sqrt`, verilen sayının **karekökünü** hesaplar.  
> Matematiksel olarak:  
> x\sqrt{x}

📌 Söz dizimi:

```js
{ $sqrt: <sayısal ifade> }
```

---

## 📁 Örnek Veri: `numbers`

```js
db.numbers.insertMany([
  { value: 4 },
  { value: 9 },
  { value: 16 },
  { value: 25 }
])
```

---

## 📌 1. Karekök alma

```js
db.numbers.aggregate([
  {
    $project: {
      value: 1,
      sqrtValue: { $sqrt: "$value" }
    }
  }
])
```

📤 Çıktı:

```json
{ value: 4, sqrtValue: 2 }
{ value: 9, sqrtValue: 3 }
{ value: 16, sqrtValue: 4 }
{ value: 25, sqrtValue: 5 }
```

---

## 📌 2. Karekök sonucu yuvarlama

```js
db.numbers.aggregate([
  {
    $project: {
      value: 1,
      sqrtRounded: {
        $round: [
          { $sqrt: "$value" },
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
|Negatif sayı|Hata verir (tanımsız)|
|`null` veya `string`|Hata verir|
|Pozitif sayı|Sorunsuz çalışır|

---

## ✅ Özet

|Özellik|Desteklenir|
|---|---|
|Pozitif sayının karekökü|✅|
|Hesaplama sonrası yuvarlama|✅|
|Aggregation pipeline’de kullanılabilir|✅|

---

Başka hangi matematik operatörü veya MongoDB konusu ile devam edelim?