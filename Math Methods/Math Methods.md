
| Operatör    | Açıklama                      | Örnek Kullanımı                         |
| ----------- | ----------------------------- | --------------------------------------- |
| `$abs`      | Mutlak değer                  | `{ $abs: "$field" }`                    |
| `$ceil`     | Yukarı yuvarlama              | `{ $ceil: "$field" }`                   |
| `$floor`    | Aşağı yuvarlama               | `{ $floor: "$field" }`                  |
| `$round`    | En yakın tam sayıya yuvarlama | `{ $round: ["$field", 2] }`             |
| `$trunc`    | Ondalık kısmı kırpma          | `{ $trunc: ["$field", 2] }`             |
| `$add`      | Toplama işlemi                | `{ $add: ["$field1", "$field2"] }`      |
| `$subtract` | Çıkarma işlemi                | `{ $subtract: ["$field1", "$field2"] }` |
| `$multiply` | Çarpma işlemi                 | `{ $multiply: ["$field1", 2] }`         |
| `$divide`   | Bölme işlemi                  | `{ $divide: ["$field1", 4] }`           |
| `$mod`      | Modulus (kalan)               | `{ $mod: ["$field", 2] }`               |
| `$exp`      | e üzeri x (üstel)             | `{ $exp: "$field" }`                    |
| `$ln`       | Doğal logaritma               | `{ $ln: "$field" }`                     |
| `$log`      | Logaritma (tabanı belirli)    | `{ $log: ["$field", 10] }`              |
| `$log10`    | Logaritma tabanı 10           | `{ $log10: "$field" }`                  |
| `$pow`      | Üs alma                       | `{ $pow: ["$field", 2] }`               |
| `$sqrt`     | Karekök                       | `{ $sqrt: "$field" }`                   |
| `$cbrt`     | Küpkök                        | `{ $cbrt: "$field" }`                   |

---

## Örnekler

### 1. Üs alma — `$pow`

```js
db.numbers.aggregate([
  {
    $project: {
      value: 1,
      squared: { $pow: ["$value", 2] }
    }
  }
])
```

### 2. Karekök — `$sqrt`

```js
db.numbers.aggregate([
  {
    $project: {
      value: 1,
      root: { $sqrt: "$value" }
    }
  }
])
```

### 3. Logaritma — `$log`

```js
db.numbers.aggregate([
  {
    $project: {
      value: 1,
      logBase10: { $log10: "$value" }
    }
  }
])
```

### 4. Üstel Fonksiyon — `$exp`

```js
db.numbers.aggregate([
  {
    $project: {
      value: 1,
      expValue: { $exp: "$value" }
    }
  }
])
```

---

## ⚠️ Dikkat

- Logaritma ve kök işlemlerinde negatif veya sıfır değerler hata verir.
- Üstel fonksiyon (`$exp`) çok büyük değerler üretip performansı etkileyebilir.
