
Elbette TkMatE! Aşağıda `$elemMatch` kullanımını pekiştirmek için detaylı **örnek bir veri kümesi** ve onun üzerinden yapılan **gerçek MongoDB sorgularını** gösteriyorum.

---

## 🔸 Örnek Koleksiyon: `students`

```json
{
  "_id": 1,
  "name": "Ali",
  "exams": [
    { "subject": "Math", "grade": 90 },
    { "subject": "Physics", "grade": 75 }
  ]
}
```

```json
{
  "_id": 2,
  "name": "Ayşe",
  "exams": [
    { "subject": "Math", "grade": 80 },
    { "subject": "Physics", "grade": 85 }
  ]
}
```

```json
{
  "_id": 3,
  "name": "Mehmet",
  "exams": [
    { "subject": "Math", "grade": 60 },
    { "subject": "Physics", "grade": 90 }
  ]
}
```

---

## 🔹 1. **Math notu 85’ten büyük olan öğrencileri getir**

> Burada dikkat: `subject == "Math"` ve `grade > 85` **aynı objede olmalı**.

```js
db.students.find({
  exams: {
    $elemMatch: {
      subject: "Math",
      grade: { $gt: 85 }
    }
  }
})
```

📌 Dönen belge:

```json
{
  "_id": 1,
  "name": "Ali",
  "exams": [
    { "subject": "Math", "grade": 90 },
    { "subject": "Physics", "grade": 75 }
  ]
}
```

---

## 🔹 2. **Physics notu 80’den küçük olanları getir**

```js
db.students.find({
  exams: {
    $elemMatch: {
      subject: "Physics",
      grade: { $lt: 80 }
    }
  }
})
```

📌 Dönen belge:

```json
{
  "_id": 1,
  "name": "Ali",
  "exams": [
    { "subject": "Math", "grade": 90 },
    { "subject": "Physics", "grade": 75 }
  ]
}
```

---

## 🔹 3. **Math: 80 ve Physics: 85 olan öğrencileri getir**

Bu iki koşulu ayrı ayrı sağlayan iki farklı obje olmalı:

```js
db.students.find({
  exams: {
    $all: [
      { $elemMatch: { subject: "Math", grade: 80 } },
      { $elemMatch: { subject: "Physics", grade: 85 } }
    ]
  }
})
```

📌 Dönen belge:

```json
{
  "_id": 2,
  "name": "Ayşe",
  "exams": [
    { "subject": "Math", "grade": 80 },
    { "subject": "Physics", "grade": 85 }
  ]
}
```

---

## 🔹 4. **Herhangi bir sınavdan 85-95 arası not alanlar**

```js
db.students.find({
  exams: {
    $elemMatch: {
      grade: { $gte: 85, $lte: 95 }
    }
  }
})
```

📌 Dönen belgeler:

- Ali (90)
    
- Ayşe (85)
    
- Mehmet (90)
    

---

## 🔹 5. **Math sınavı notu kesinlikle 90 olanlar**

```js
db.students.find({
  exams: {
    $elemMatch: {
      subject: "Math",
      grade: 90
    }
  }
})
```

📌 Dönen belge:

- Sadece **Ali**
    

---

## 🔚 Sonuç

`$elemMatch`, şu gibi durumlar için **vazgeçilmezdir**:

- Array içindeki objede **çoklu alan** koşuluyla eşleşme
    
- Array içinde sayısal karşılaştırmalar
    
- `$all` ile birlikte birden fazla eşleşme kontrolü
    

---

İstersen şimdi bu veri kümesi üzerinden `$size`, `$in`, `$nin`, veya update (`$push`, `$pull`) gibi konuları da canlı örneklerle işleyebiliriz.

Hangi başlıkla devam edelim TkMatE?