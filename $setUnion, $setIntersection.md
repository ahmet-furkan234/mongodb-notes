
## 🔷 `$setUnion`

### ✅ Tanım:

İki diziyi **birleştirir** ve **tekrarlayan elemanları atar**.  
**Tüm benzersiz elemanları** döner.

### 📘 Söz dizimi:

```js
{ $setUnion: [ <array1>, <array2> ] }
```

---

### 🧪 Örnek:

```js
db.test.aggregate([
  {
    $project: {
      set1: [1, 2, 3],
      set2: [3, 4, 5],
      unionResult: { $setUnion: [[1, 2, 3], [3, 4, 5]] }
    }
  }
])
```

**Sonuç:**

```json
unionResult: [1, 2, 3, 4, 5]
```

---

## 🔷 `$setIntersection`

### ✅ Tanım:

İki dizi arasında **ortak olan** (kesişimdeki) elemanları döner.

### 📘 Söz dizimi:

```js
{ $setIntersection: [ <array1>, <array2> ] }
```

---

### 🧪 Örnek:

```js
db.test.aggregate([
  {
    $project: {
      set1: [1, 2, 3],
      set2: [3, 4, 5],
      intersectionResult: { $setIntersection: [[1, 2, 3], [3, 4, 5]] }
    }
  }
])
```

**Sonuç:**

```json
intersectionResult: [3]
```

---

## 🔰 Gerçek Hayat Örneği (ETRADE_RDMS)

### 🎯 Kullanıcıların Favori Kategorileri ile Sepet Kategorilerini Karşılaştır:

```js
db.USERS.aggregate([
  {
    $project: {
      name: "$NAMESURNAME",
      favorites: ["Elektronik", "Kitap", "Moda"],
      cartCategories: ["Moda", "Spor", "Elektronik"],
      
      ortaklar: {
        $setIntersection: [
          ["Elektronik", "Kitap", "Moda"],
          ["Moda", "Spor", "Elektronik"]
        ]
      },
      
      toplamIlgi: {
        $setUnion: [
          ["Elektronik", "Kitap", "Moda"],
          ["Moda", "Spor", "Elektronik"]
        ]
      }
    }
  }
])
```

---

## 🤔 Notlar:

|Özellik|`$setUnion`|`$setIntersection`|
|---|---|---|
|Çıktı Türü|Tekrarsız tüm elemanlar|Ortak olan elemanlar|
|Boş küme varsa|Diğer dizinin benzersizleri|Boş dizi `[]` döner|
|Dizi içeriği|Sıralı değil|Sıralı değil|
