
> `$ln`, verilen pozitif sayının **doğal logaritmasını** (logaritma tabanı _e_) hesaplar.  
> Matematiksel olarak:  
> ln⁡(x)=log⁡e(x)\ln(x) = \log_e(x)  
> Burada _e_ yaklaşık 2.71828’dir.

📌 Söz dizimi:

```js
{ $ln: <pozitif sayı veya ifade> }
```

---

## 📁 Örnek Veri: `values`

```js
db.values.insertMany([
  { number: 1 },
  { number: 2.718281828 },  // yaklaşık e
  { number: 10 },
  { number: 100 }
])
```

---

## 📌 1. Doğal logaritma hesaplama

```js
db.values.aggregate([
  {
    $project: {
      number: 1,
      lnValue: { $ln: "$number" }
    }
  }
])
```

📤 Çıktı:

```json
{ number: 1, lnValue: 0 }
{ number: 2.718281828, lnValue: 1 }
{ number: 10, lnValue: 2.302585 }
{ number: 100, lnValue: 4.605170 }
```

---

## 📌 2. Hesaplama sonrası yuvarlama (örneğin 3 basamağa)

```js
db.values.aggregate([
  {
    $project: {
      number: 1,
      lnRounded: {
        $round: [
          { $ln: "$number" },
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
|`number <= 0`|Hata verir (logaritma tanımsız)|
|`null` veya `string`|Hata verir|
|Pozitif sayı (0’dan büyük)|Sorunsuz çalışır|

---

## ✅ Özet

|Özellik|Desteklenir|
|---|---|
|Pozitif sayının doğal logaritması|✅|
|Hesaplama sonrası yuvarlama|✅|
|Aggregation pipeline içinde kullanılabilir|✅|

---

### 📚 Doğal Logaritma Nerelerde Kullanılır?

- Matematiksel analiz ve istatistikte
- Üstel büyüme/azalma hesaplamalarında
- Logaritmik skala dönüşümlerinde
- Makine öğrenmesi ve istatistiksel modellerde
