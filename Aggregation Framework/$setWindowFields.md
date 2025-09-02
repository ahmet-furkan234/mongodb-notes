
## 📌 Tanım:

`$setWindowFields`, MongoDB'nin **aggregation pipeline** aşamalarından biridir.  
Gruplar içinde **sıralama, numaralandırma, toplam alma, ortalama, fark hesaplama gibi işlemleri** yapar. SQL'deki `OVER(PARTITION BY ...)` fonksiyonuna benzer.

---

## 🧱 Söz Dizimi:

```js
{
  $setWindowFields: {
    partitionBy: "$<alan>",    // Gruplamak istediğin alan
    sortBy: { "<alan>": 1 },   // Gruplar içinde sıralama
    output: {
      <çıktıAdı>: { <fonksiyon>: {} } // Fonksiyon ve çıktısı
    }
  }
}
```

---

## 🧮 Fonksiyonlar:

|Fonksiyon|Açıklama|
|---|---|
|`$sum`|Toplam|
|`$avg`|Ortalama|
|`$min`, `$max`|Minimum ve maksimum değerler|
|`$count`|Grup içi belge sayısı|
|`$rank`|Aynı değerler aynı sırada, boşluk bırakır|
|`$denseRank`|Aynı değerler aynı sırada, boşluk bırakmaz|
|`$documentNumber`|Her belgeye 1’den başlayarak sıra numarası verir|
|`$shift`|Önceki ya da sonraki belgeyi alır|

---

## 📊 Temel Kullanım: Belgelere Sıra Numarası Verme

```js
db.movies.aggregate([
  {
    $setWindowFields: {
      partitionBy: "$year",
      sortBy: { "imdb.rating": -1 },
      output: {
        rank: { $documentNumber: {} }
      }
    }
  }
]);
```

📌 Açıklama:  
Her **yıl (`$year`) için**, IMDb puanına göre sıralanır ve **1’den başlayan sıra numarası (`rank`)** verilir.

---

## 🎯 Örnek 1: Her Yılın En İyi Filmi

```js
db.movies.aggregate([
  {
    $match: {
      "imdb.rating": { $type: "double" }
    }
  },
  {
    $setWindowFields: {
      partitionBy: "$year",
      sortBy: { "imdb.rating": -1 },
      output: {
        rank: { $documentNumber: {} }
      }
    }
  },
  {
    $match: {
      rank: 1
    }
  },
  {
    $project: {
      year: 1,
      title: 1,
      "imdb.rating": 1
    }
  }
]);
```

📌 Her yılın IMDb puanı **en yüksek** olan 1. filmi listelenir.

---

## 🎯 Örnek 2: Yönetmen Başına Ortalama IMDb Puanı

```js
db.movies.aggregate([
  { $unwind: "$directors" },
  {
    $match: {
      "imdb.rating": { $type: "double" }
    }
  },
  {
    $setWindowFields: {
      partitionBy: "$directors",
      output: {
        avgRating: { $avg: "$imdb.rating" }
      }
    }
  },
  {
    $project: {
      director: "$directors",
      title: 1,
      "imdb.rating": 1,
      avgRating: 1
    }
  }
]);
```

📌 Her filmde yönetmenin çektiği filmlerin **ortalama IMDb puanı** eklenmiş olur.

---

## 🧠 `$documentNumber` vs `$rank` vs `$denseRank`

|Fonksiyon|Açıklama|
|---|---|
|`$documentNumber`|Her belgeye sırayla 1, 2, 3, 4, ... verir|
|`$rank`|Aynı değerdekiler aynı sıraya sahip ama boşluk bırakır|
|`$denseRank`|Aynı değerdekiler aynı sıraya sahip ama boşluk bırakmaz|

### Örnek:

IMDb puanları: `9.0, 9.0, 8.5, 8.0`

|Film|$documentNumber|$rank|$denseRank|
|---|---|---|---|
|A (9.0)|1|1|1|
|B (9.0)|2|1|1|
|C (8.5)|3|3|2|
|D (8.0)|4|4|3|

---

## 🧩 Kullanım Senaryoları

|İhtiyaç|Kullanılır mı?|
|---|---|
|Her yılın en iyi 3 filmi|✅ Evet|
|Yönetmen başına IMDb ortalaması|✅ Evet|
|Her oyuncunun sıralaması|✅ Evet|
|Önceki filmin IMDb farkını bulmak (`$shift`)|✅ Evet|
|Grup içi toplam süre (`$sum`)|✅ Evet|

---

## 🔚 Özet:

`$setWindowFields` ile şunları yapabilirsin:

✅ Gruplar içinde sıralama  
✅ Belgelere sıra numarası verme  
✅ Ortalama, toplam, fark gibi istatistikler  
✅ SQL'deki `OVER(PARTITION BY ...)` yapısının aynısı

MongoDB'de **ileri seviye analiz** yapmak istiyorsan vazgeçilmezdir.

---

İstersen bu konuya özel mini testler veya daha fazla örnek hazırlayabilirim. Sıradaki soruya da geçebiliriz.