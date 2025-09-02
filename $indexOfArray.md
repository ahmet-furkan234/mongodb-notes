
Belirli bir öğenin, bir dizideki **indeksini (sırasını)** döner.

```js
{ $indexOfArray: [<array>, <searchElement>, <start>, <end>] }
```

|Parametre|Açıklama|
|---|---|
|`<array>`|İçinde aranacak dizi|
|`<searchElement>`|Aranacak değer|
|`<start>`|(Opsiyonel) Aramaya başlanacak index|
|`<end>`|(Opsiyonel) Aramanın duracağı index (hariç)|

> **Not:** Eleman bulunamazsa `-1` döner.

---

## 🧪 Basit Örnek

```js
db.LISTS.aggregate([
  {
    $project: {
      values: 1,
      indexOf42: { $indexOfArray: ["$values", 42] }
    }
  }
])
```

### 📌 Açıklama:

- Her belgedeki `values` dizisi içinde `42` değerinin indeksini bulur.
- Eğer varsa 0, 1, 2 gibi döner; yoksa `-1`.

---

## 📘 Veri Örneği

```json
{
  "_id": 1,
  "values": [10, 20, 42, 50]
}
```

```json
Sonuç: {
  "_id": 1,
  "indexOf42": 2
}
```

---

## 🎯 Senaryo 1 — Şehir Listesinde "İzmir" Kaçıncı Sırada?

```js
db.USERS.aggregate([
  {
    $project: {
      NAMESURNAME: 1,
      visitedCities: 1,
      izmirIndex: { $indexOfArray: ["$visitedCities", "İzmir"] }
    }
  }
])
```

---

## 🎯 Senaryo 2 — Belirli Konumdan Aramaya Başla

```js
db.LISTS.aggregate([
  {
    $project: {
      values: 1,
      indexAfter2nd: { $indexOfArray: ["$values", 42, 2] }
    }
  }
])
```

> Bu örnek, 2. indeksten sonra `42` varsa onun indeksini döner.

---

## 🎯 Senaryo 3 — Aralıkta Arama

```js
db.LISTS.aggregate([
  {
    $project: {
      values: 1,
      rangeIndex: { $indexOfArray: ["$values", 30, 1, 3] }
    }
  }
])
```

> Sadece index 1 ve 2 arası kontrol edilir. Varsa indeks döner, yoksa -1.

---

## 🎓 Bilgi:

- `null` veya `undefined` elemanlar da kontrol edilir.
- Sadece ilk eşleşme döner (diğerlerini almak istersen `$filter` gerekir).
