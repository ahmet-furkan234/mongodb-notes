
Bir array (dizi) alanının **tam olarak kaç eleman içerdiğini** kontrol eder.

> Not: `=`, yani eşitlik karşılaştırması yapar. `>`, `<` gibi karşılaştırmalar `$size` ile yapılamaz (aggregation gerekir).

---

## 🔹 1. **Temel Kullanım**

Diyelim ki aşağıdaki gibi `students` koleksiyonumuz var:

```json
{
  _id: 1,
  name: "Ali",
  skills: ["MongoDB", "Node.js", "Express"]
}
```

```js
db.students.find({
  skills: { $size: 3 }
})
```

> `skills` dizisi **tam olarak 3 elemanlı** olan belgeleri döner.

---

## 🔹 2. **Başka bir örnek veri:**

```json
{
  _id: 2,
  name: "Ayşe",
  skills: ["MongoDB", "Node.js"]
}
```

```json
{
  _id: 3,
  name: "Mehmet",
  skills: []
}
```

### 👇 Sorgu: skills dizisi boş olanları bul

```js
db.students.find({
  skills: { $size: 0 }
})
```

📌 Dönen: `Mehmet`

---

## 🔹 3. **Sadece eşitlik yapılabilir**

```js
db.students.find({
  skills: { $size: { $gt: 2 } }  // ❌ GEÇERSİZ
})
```

> `$size` sadece `number` kabul eder. Karşılaştırma operatörleriyle **çalışmaz**.

🛠 Bu tarz karşılaştırmalar için **aggregation** kullanmalısın:

```js
db.students.aggregate([
  {
    $project: {
      name: 1,
      skillsCount: { $size: "$skills" }
    }
  },
  {
    $match: {
      skillsCount: { $gt: 2 }
    }
  }
])
```

---

## 🔹 4. **Array alanı yoksa veya null ise**

Eğer belge şu şekildeyse:

```json
{
  name: "Zeynep"
  // skills alanı yok
}
```

ve şu sorguyu yaparsan:

```js
db.students.find({
  skills: { $size: 3 }
})
```

> Bu belge döndürülmez çünkü `skills` alanı **yok**.

---

## 🔹 5. **Array içi obje fark etmez**

```json
{
  name: "Ahmet",
  exams: [
    { subject: "Math", grade: 90 },
    { subject: "Physics", grade: 85 }
  ]
}
```

```js
db.students.find({
  exams: { $size: 2 }
})
```

> Array içinde string, obje, sayı fark etmez — `2` eleman varsa eşleşir.

---

## ✅ Özet Tablo

|Kullanım|Açıklama|
|---|---|
|`{ skills: { $size: 3 } }`|Dizi 3 elemanlı mı?|
|`{ tags: { $size: 0 } }`|Boş dizi mi?|
|`>`, `<`, `>=`|❌ Geçersiz|
|Büyük/küçük karşılaştırma|✅ Aggregation ile yapılır|

---

## 💡 İpucu:

Eğer array içinde eleman sayısı sınırıyla oynuyorsan (örneğin `max 5 etiket` gibi), `$size` ile validasyon veya filtreleme yapabilirsin.
