
## 1. Temel Metin Arama

### Index oluştur

```js
db.blogs.createIndex({ title: "text", body: "text" })
```

### Sorgu: Bir kelime arama

```js
db.blogs.find({ $text: { $search: "mongodb" } })
```

> `mongodb` geçen tüm dokümanlar gelir.

---

## 2. Birden Fazla Kelime (OR mantığı)

```js
db.blogs.find({ $text: { $search: "mongodb database" } })
```

> `mongodb` veya `database` geçenler döner.

---

## 3. Cümle Arama (Kelime grubunu exact aramak **mümkün değil**)

> `$text` tam cümle araması desteklemez, kelimeler **herhangi bir sırayla** aranır.

```js
db.blogs.find({ $text: { $search: "\"mongodb database\"" } })
```

> Tırnak içinde yazılan kelimeler birlikte aranır ancak sıralama veya bitişik olma garanti edilmez.

---

## 4. Negatif Arama (Bir kelimeyi hariç tutmak)

```js
db.blogs.find({ $text: { $search: "mongodb -database" } })
```

> `mongodb` geçen ama `database` içermeyen sonuçlar.

---

## 5. Büyük/Küçük Harf Duyarlılığı

```js
db.blogs.find({
  $text: {
    $search: "MongoDB",
    $caseSensitive: true
  }
})
```

> Sadece büyük-küçük harfe tam uyan sonuçlar gelir.

---

## 6. Aksan Duyarlılığı

```js
db.blogs.find({
  $text: {
    $search: "café",
    $diacriticSensitive: true
  }
})
```

> `café` kelimesi sadece aksanlı biçimde aranır, `cafe` farklı sonuçtur.

---

## 7. Sıralama: Skora Göre Sonuçları Sırala

```js
db.blogs.find(
  { $text: { $search: "mongodb database" } },
  { score: { $meta: "textScore" } }
).sort({ score: { $meta: "textScore" } })
```

---

## 8. Weighted Text Index (Ağırlıklandırma)

```js
db.blogs.createIndex(
  { title: "text", body: "text" },
  { weights: { title: 10, body: 2 } }
)
```

- `title` alanındaki kelimeler `body` alanına göre 5 kat daha önemli sayılır.
- Skor bu doğrultuda hesaplanır.

---

## 9. Text Index Sadece Bazı Belgelerde (Partial Index ile)

```js
db.blogs.createIndex(
  { title: "text", body: "text" },
  { partialFilterExpression: { published: true } }
)
```

- Sadece `published: true` olan belgelerde indeks oluşur.
- Yayımlanmamış içerikler arama dışı kalır.

---

## 10. Text Index Kısıtlamaları

- Koleksiyonda **yalnızca 1 tane** text index olabilir.
- Text index oluşturulacak alanların türü **string** olmalıdır.
- Text index, tüm dil köklerine göre arama yapar (stemming).
