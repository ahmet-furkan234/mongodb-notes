
Bir sorgunun sonucunda dönen belgeleri tek seferde değil, **aşamalı olarak** (batch batch) almamıza olanak tanır.

---

## 📦 Temel Özellikler:

|Özellik|Açıklama|
|---|---|
|Bellek dostu|Tüm veriyi tek seferde almaz, partiler halinde çeker.|
|Lazy Evaluation|Gerçekten ihtiyaç duyulana kadar veri getirmez.|
|Iterable|`.next()`, `.forEach()` gibi metotlarla gezilebilir.|
|Timeout'lıdır|Varsayılan olarak 10 dakika sonra kapanır.|
|Server-side'da tutulur|Çok büyük veri çekiyorsan dikkatli kullanılmalı.|

---

## 🔸 Basit Cursor Kullanımı

```js
const cursor = db.kullanicilar.find({ yas: { $gt: 18 } });

while (cursor.hasNext()) {
  printjson(cursor.next());
}
```

> `hasNext()` ve `next()` ile elle yönetim yapılır.

---

## 🔸 `.forEach()` ile Okuma

```js
db.kullanicilar.find().forEach(doc => {
  print(`Ad: ${doc.ad}, Yaş: ${doc.yas}`);
});
```

> Daha pratik bir kullanım. Özellikle script yazarken önerilir.

---

## 🔸 `.toArray()`

```js
const sonuc = db.kullanicilar.find().toArray();
printjson(sonuc);
```

> Cursor'daki tüm veriyi diziye çevirir. Dikkat! Büyük verilerde belleği zorlar.

---

## 🔸 `.limit()`, `.skip()`, `.sort()` gibi metodlar cursor üzerinde zincirleme çalışır:

```js
db.kullanicilar
  .find({ sehir: "Ankara" })
  .sort({ yas: -1 })
  .skip(10)
  .limit(5)
  .forEach(printjson);
```

---

## 🔸 Cursor Özelliklerini Görmek

```js
const cursor = db.kullanicilar.find();
printjson(cursor);
```

> Bu, cursor nesnesinin hangi parametrelerle oluşturulduğunu gösterir.

---

## 🔸 Cursor Timeout Özelliği

Varsayılan olarak, kullanılmayan cursor'lar **10 dakika sonra otomatik kapanır.** Bu sunucu kaynaklarını korumak içindir.

### Kapatmak için:

```js
db.kullanicilar.find().noCursorTimeout();
```

> ⚠️ Dikkat: Bu özellik dikkatli kullanılmalı, aksi takdirde sunucuda kaynak sızıntısına neden olabilir.

---

## 🔸 Batch Size Ayarlama

Cursor verileri varsayılan olarak batch halinde çeker. İstersen batch büyüklüğünü sen belirleyebilirsin:

```js
db.kullanicilar.find().batchSize(50);
```

> Her batch'te 50 belge gelir.

---

## 🔸 Cursor Methods Özeti

|Yöntem|Açıklama|
|---|---|
|`.next()`|Sonraki belgeyi döner|
|`.hasNext()`|Devam var mı kontrolü|
|`.forEach()`|Her belge için işlem yapar|
|`.toArray()`|Tüm sonuçları diziye çevirir|
|`.limit(n)`|En fazla `n` belge|
|`.skip(n)`|İlk `n` belgeyi atlar|
|`.sort()`|Sıralama yapar|
|`.batchSize(n)`|Batch büyüklüğü|
|`.noCursorTimeout()`|Cursor’ın kapanmasını engeller|

---

## 🔸 Node.js ile Cursor Kullanımı (Asenkron)

```js
const cursor = db.collection('kullanicilar').find({ yas: { $gte: 18 } });

for await (const doc of cursor) {
  console.log(doc);
}
```

> Modern `for await...of` yapısı, Node.js 10+ ile uyumludur.

---

## 🎯 Ne Zaman Cursor Kullanmalı?

|Durum|Kullanım|
|---|---|
|1000+ kayıt|Cursor kullan, `.toArray()` bellek tüketebilir|
|Kademeli gösterim (pagination)|`skip()` ve `limit()` ile|
|Arayüzde scroll veya infinite scroll varsa|Cursor idealdir|
|Büyük raporlar|`forEach()` veya `batchSize()` kullanarak oku|
