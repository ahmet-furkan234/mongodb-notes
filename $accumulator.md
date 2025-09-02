
`$group` aşamasında, **kendi toplama mantığını** yazabileceğin bir _custom accumulator function_ tanımlar.  
JavaScript benzeri bir yapıdadır ama Mongo'nun sınırlı işlemleriyle çalışır.

---

## 📘 Söz Dizimi:

```js
{
  $accumulator: {
    init: <function>,        // ilk değerleri ayarlar
    initArgs: <array>,       // init'e argüman gönderir (opsiyonel)
    accumulate: <function>,  // her belge için çalışır
    accumulateArgs: <array>, // belge verilerini gönderir
    merge: <function>,       // paralel işlemleri birleştirir
    finalize: <function>,    // son değeri döner
    lang: "js"               // her zaman "js"
  }
}
```

---

## 📌 Örnek 1: Karakter Toplamı ($accumulator ile karakter sayısı)

### Veri:

```json
[
  { "category": "A", "text": "hello" },
  { "category": "A", "text": "world" },
  { "category": "B", "text": "mongodb" }
]
```

### Sorgu:

```js
db.collection.aggregate([
  {
    $group: {
      _id: "$category",
      totalCharCount: {
        $accumulator: {
          init: function () {
            return 0;
          },
          accumulate: function (state, text) {
            return state + text.length;
          },
          accumulateArgs: ["$text"],
          merge: function (state1, state2) {
            return state1 + state2;
          },
          finalize: function (state) {
            return state;
          },
          lang: "js"
        }
      }
    }
  }
])
```

### Sonuç:

```json
[
  { "_id": "A", "totalCharCount": 10 },
  { "_id": "B", "totalCharCount": 7 }
]
```

---

## 📌 Örnek 2: Kendi Ortalamanı Hesapla

```js
db.collection.aggregate([
  {
    $group: {
      _id: "$category",
      customAvg: {
        $accumulator: {
          init: function () {
            return { sum: 0, count: 0 };
          },
          accumulate: function (state, val) {
            return {
              sum: state.sum + val,
              count: state.count + 1
            };
          },
          accumulateArgs: ["$value"],
          merge: function (state1, state2) {
            return {
              sum: state1.sum + state2.sum,
              count: state1.count + state2.count
            };
          },
          finalize: function (state) {
            return state.count === 0 ? 0 : state.sum / state.count;
          },
          lang: "js"
        }
      }
    }
  }
])
```

---

## 💡 Nerelerde Kullanılır?

- Kendi özel `reduce` işlemlerini tanımlamak (örn: standart sapma, ağırlıklı ortalama)
    
- Text veya array verilerde özel string manipulation
    
- Paralel çalışmada `merge` ve `finalize` işlemlerini kontrol etmek
    

---

İstersen sırada `$meta` operatörünü inceleyebiliriz.  
Bu operatör genelde **text search skorları** veya **index bilgileri** için kullanılır. Devam edelim mi?