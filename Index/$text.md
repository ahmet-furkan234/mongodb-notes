
MongoDB, büyük metin alanlarında **tam metin arama (full-text search)** yapabilmek için `text index` kullanır ve sorgularda `$text` operatörünü sağlar.

---

### 1. Text Index Oluşturma

```js
db.articles.createIndex({ title: "text", content: "text" })
```

- `title` ve `content` alanları üzerinde tam metin araması için indeks oluşturulur.
- Bir koleksiyonda sadece **bir tane text index** olabilir.

---

### 2. `$text` Sorgusu

```js
db.articles.find({ $text: { $search: "mongodb index" } })
```

- `"mongodb"` veya `"index"` kelimelerini içeren belgeler döner.
- Kelimeler **veya (OR)** mantığıyla aranır.

---

### 3. Gelişmiş Arama Parametreleri

| Operatör              | Açıklama                                      |
| --------------------- | --------------------------------------------- |
| `$search`             | Arama yapılacak kelime veya kelime grubu      |
| `$language`           | Dil seçimi (default: "english")               |
| `$caseSensitive`      | Büyük/küçük harf duyarlılığı (default: false) |
| `$diacriticSensitive` | Aksan duyarlılığı (default: false)            |

---

### 4. Örnek: Dil ve Duyarlılık

```js
db.articles.find({
  $text: {
    $search: "mongodb",
    $language: "none",
    $caseSensitive: true,
    $diacriticSensitive: false
  }
})
```

---

### 5. Sonuçların Sıralanması (Score ile)

Sorgu sonucu, metin eşleşmesinin kalitesine göre **bir skor** döner. Bu skoru kullanarak sıralama yapabilirsin:

```js
db.articles.find(
  { $text: { $search: "mongodb index" } },
  { score: { $meta: "textScore" } }
).sort({ score: { $meta: "textScore" } })
```

---

### 6. Text Index Oluşturma İpuçları

- Tek bir text index olmalı.    
- Çok alanlı text index mümkün.
- Text index oluştururken **`weights`** ile alanlara ağırlık verebilirsin.

```js
db.articles.createIndex(
  { title: "text", content: "text" },
  { weights: { title: 10, content: 5 } }
)
```

---

### 7. Özet

| Konsept                                                | Açıklama                         |
| ------------------------------------------------------ | -------------------------------- |
| `text index`                                           | Metin arama için özel index türü |
| `$text` operatörü                                      | Metin araması yapar              |
| `$search`                                              | Aranacak kelimeler               |
| `score`                                                | Eşleşme kalitesi                 |
| Dil, büyük-küçük harf ve aksan duyarlılığı opsiyonları |                                  |
