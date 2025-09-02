
## 📌 Nedir Compound Index?

> Compound Index, **birden fazla alan**ı kapsayan bir indekstir.  
> Alanlar **belirli bir sıraya göre** tanımlanır ve sorgu da bu sıraya uygun olmalıdır.

```js
db.collection.createIndex({ alan1: 1, alan2: -1 })
```

- `alan1`: Artan sıralama
- `alan2`: Azalan sıralama
- En fazla **32 alan** içerebilir.

---

## ✅ Örnek: Sipariş Tablosu

### Koleksiyon: `orders`

```json
{
  userId: 123,
  status: "completed",
  createdAt: ISODate("2025-07-12")
}
```

---

### 🔹 1. Index Oluşturma

```js
db.orders.createIndex({ userId: 1, createdAt: -1 })
```

> Bu index şu kombinasyonlar için **kullanılır**:

|Sorgu|Index Kullanılır mı?|
|---|---|
|`{ userId: 123 }`|✅ Evet|
|`{ userId: 123, createdAt: { $gt: ... } }`|✅ Evet|
|`{ createdAt: { $gt: ... } }`|❌ Hayır|

> ❗ **Sadece ilk alandan başlanarak** ilerlenebilir. İlk alan olmadan ikinciye gidemezsin!

---

### 🔹 2. Sıralama İçin Kullanım

```js
db.orders.find({ userId: 123 }).sort({ createdAt: -1 })
```

> Bu sorgu hem **filter** hem **sort** için index kullanır → **çok hızlı** çalışır.

---

## 🧠 Prefix Rule (Ön Ek Kuralı)

> MongoDB, compound index’in **başlangıç alanlarına göre sıralı prefix’lerini** kullanabilir.

```js
createIndex({ a: 1, b: 1, c: 1 })
```

Şunlar için index kullanılır:

- `{ a }`
- `{ a, b }`
- `{ a, b, c }`

Ama **şunlar için kullanılamaz**:

- `{ b }`
- `{ b, c }`
- `{ c }`

---

## 🧪 Kullanım Farklılıkları

| Sorgu                           | Index Kullanımı              |
| ------------------------------- | ---------------------------- |
| `find({ a: 1 })`                | ✅ Evet                       |
| `find({ a: 1 }).sort({ b: 1 })` | ✅ Evet                       |
| `find({ a: 1 }).sort({ c: 1 })` | ⚠️ Sıralama için kullanılmaz |
| `find({ b: 1 })`                | ❌ Hayır                      |

---

## ⚠️ Dikkat Edilmesi Gerekenler

| Uyarı                         | Açıklama                                |
| ----------------------------- | --------------------------------------- |
| Sıra önemlidir                | Index sıradaki alanlara göre işler      |
| Fazla alan performans düşürür | Gereksiz alan eklersen yazma hızı düşer |
| İlk alan şarttır              | İlk alan yoksa index kullanılmaz        |
| Filtre + sort birleşimi       | Doğru kurarsan çok performanslı olur    |

---

## 🔎 `explain()` ile kontrol:

```js
db.orders.find({ userId: 123 }).sort({ createdAt: -1 }).explain("executionStats")
```

```json
"stage": "IXSCAN"
```

Görüyorsan → **index doğru kullanılmış.**

---

## 💡 İpuçları

| Amaç                        | Index Önerisi                           |
| --------------------------- | --------------------------------------- |
| `find + sort`               | Filter alanı + sort alanı               |
| Raporlama sorguları         | En çok kullanılan alanları üstte sırala |
| Çok sık güncellenen alanlar | Sona bırak veya index'e ekleme          |

---

## 🧠 Compound vs Single Index

| Sorgu                            | Gerekli Index                  |
| -------------------------------- | ------------------------------ |
| `{ userId: 123 }`                | `{ userId: 1 }`                |
| `{ userId: 123, createdAt: -1 }` | `{ userId: 1, createdAt: -1 }` |

> ❗ Her kombinasyon için ayrı single index yaratmak yerine **tek compound index yeterli.**

---

## 🔚 Özet

| Özellik            | Açıklama                              |
| ------------------ | ------------------------------------- |
| Birden fazla alan  | Aynı anda index'lenir                 |
| Sıra çok önemlidir | Başlangıçtan tarar                    |
| Prefix mantığı     | İlk alanlardan başlanarak çalışır     |
| Performans         | Uygun filtre + sıralama ile çok hızlı |

---

İstersen sana özel `compound index` önerisi çıkarabilirim:  
Koleksiyon yapını ve örnek sorgularını verirsen, en doğru index yapısını birlikte analiz edebiliriz.

Devam edelim mi?