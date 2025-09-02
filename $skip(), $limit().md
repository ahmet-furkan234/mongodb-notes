
---

## ✅ 1. `limit(n)` → Belirli sayıda sonuç getirir

```js
db.collection.find().limit(5)
```

> En fazla 5 belge döner. Veritabanında ilk 5 kayıt.

---

## ✅ 2. `skip(n)` → Belirli sayıda sonucu atlar

```js
db.collection.find().skip(10)
```

> İlk 10 belgeyi atla, kalanları getir.

---

## 🧠 3. Birlikte Kullanım: Sayfalama

```js
db.collection.find().skip(10).limit(5)
```

> Sayfa başına 5 kayıt varsa, bu 3. sayfayı verir (`(3-1)×5 = 10 skip`).

---

## 📖 Uygulamalı Örnek

Koleksiyon: `users`

```js
db.users.find({}, { name: 1, age: 1, _id: 0 })
  .sort({ age: -1 })
  .skip(10)
  .limit(5)
```

### Bu ne yapar?

- Yaşça büyükten küçüğe sıralar
    
- İlk 10 kullanıcıyı atlar
    
- Sonraki 5 kişiyi getirir
    

> Yani 3. sayfayı döndürür (sayfa boyutu 5 ise)

---

## 🔄 Sayfa Mantığı İçin Formül

```js
const pageSize = 10;
const pageNumber = 4;

const skipAmount = (pageNumber - 1) * pageSize;
```

```js
db.collection.find()
  .skip(skipAmount)
  .limit(pageSize)
```

> Bu yapı, her yerde dinamik sayfalama için kullanılır.

---

## ⚠️ Dikkat Edilecek Noktalar

|Konu|Açıklama|
|---|---|
|`skip()` yavaşlar|Büyük `skip` değerlerinde performans düşebilir (1000+ gibi)|
|Sıralama şarttır|Sayfalama yaparken `sort()` ile birlikte kullanılmalı|
|`limit(0)`|Hiçbir belge döndürmez|
|`skip(0)`|Hiçbir belge atlanmaz (etkisi yok)|

---

## ✅ Gerçekçi Senaryo: Ürünleri 2. sayfada göster (8 ürün/sayfa)

```js
db.products.find({}, { name: 1, price: 1, _id: 0 })
  .sort({ price: -1 })
  .skip(8)     // (2-1)×8
  .limit(8)
```

> En pahalı ürünlerden başlayarak, ikinci sayfadaki 8 ürünü getirir.

---

## 🧪 Bonus: Toplam Sayfa Hesaplama

```js
const totalCount = db.products.countDocuments();
const totalPages = Math.ceil(totalCount / pageSize);
```

> Sayfalama arayüzlerinde toplam sayfa sayısını göstermek için.

---

## ✅ Özet

|MongoDB Fonksiyonu|Anlamı|SQL Karşılığı|
|---|---|---|
|`limit(n)`|En fazla n kayıt getir|`LIMIT n`|
|`skip(n)`|İlk n kaydı atla|`OFFSET n`|
|`skip().limit()`|Sayfalama yapar|`LIMIT x OFFSET y`|

---

İstersen bu yapıya bir `pagination` helper fonksiyonu yazabiliriz (örneğin JavaScript ya da C# için). Veya `aggregation + skip + limit` kombinasyonunu da anlatabilirim. Hangisiyle devam edelim TkMatE? 🚀