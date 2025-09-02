
> `$log`, verilen sayının belirtilen tabandaki logaritmasını hesaplar.  
> Matematiksel olarak:  
> log⁡base(x)\log_{base}(x)  
> Burada:
> - _x_ = logaritması alınacak pozitif sayı
> - _base_ = logaritma tabanı (pozitif ve 1’den farklı)

📌 Söz dizimi:

```js
{ $log: [ <pozitif sayı>, <pozitif taban> ] }
```

---

## 📁 Örnek Veri: `values`

```js
db.values.insertMany([
  { number: 8 },
  { number: 16 },
  { number: 32 },
  { number: 64 }
])
```

---

## 📌 1. Taban 2 logaritması (binary log)

```js
db.values.aggregate([
  {
    $project: {
      number: 1,
      logBase2: { $log: ["$number", 2] }
    }
  }
])
```

📤 Çıktı:

```json
{ number: 8, logBase2: 3 }
{ number: 16, logBase2: 4 }
{ number: 32, logBase2: 5 }
{ number: 64, logBase2: 6 }
```

---

## 📌 2. Taban 10 logaritması (log10 ile aynı sonucu verir)

```js
db.values.aggregate([
  {
    $project: {
      number: 1,
      logBase10: { $log: ["$number", 10] }
    }
  }
])
```

---

## 📌 3. Hesaplama sonrası yuvarlama

```js
db.values.aggregate([
  {
    $project: {
      number: 1,
      logRounded: {
        $round: [
          { $log: ["$number", 2] },
          3
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
|`number <= 0`|Hata verir (log tanımsız)|
|`base <= 0` veya `base == 1`|Hata verir (geçersiz taban)|
|`null` veya `string`|Hata verir|
|Pozitif sayı ve uygun taban|Sorunsuz çalışır|

---

## ✅ Özet

|Özellik|Desteklenir|
|---|---|
|İstenen tabanda logaritma|✅|
|Hesaplama sonrası yuvarlama|✅|
|Aggregation pipeline içinde kullanılabilir|✅|

---

### 📚 Nerelerde Kullanılır?

- Bilgisayar biliminde algoritma karmaşıklığı hesaplarında
- Veri ölçeklendirmede
- Finans ve mühendislikte logaritmik hesaplamalarda
