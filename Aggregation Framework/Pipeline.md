
> **Aggregation Pipeline**, verilerin **aşama aşama işlenmesini sağlayan** bir veri işleme hattıdır.

Her aşamada (stage) veri işlenir, filtrelenir, dönüştürülür ve bir sonraki aşamaya **aktarılır**.

---

## 🧱 Mantıksal Benzetme

Düşün ki bir su boru hattı var. Su ilk başta bir filtreden geçiyor, sonra ısıtılıyor, sonra temizleniyor vs.

MongoDB'de de:

- Veri ilk olarak `$match` filtresinden geçer.
- Sonra `$project` ile alanlar seçilir.
- Ardından `$group` ile özetlenir.
- En son `$sort`, `$limit`, vs. gibi işlemlerle düzenlenir.

Hepsi bir zincir şeklindedir.  
Bu zincire **aggregation pipeline** denir.

---

## 📦 Temel Yapı

```js
db.collection.aggregate([
  { $match: { status: "active" } },
  { $project: { name: 1, age: 1 } },
  { $sort: { age: -1 } },
  { $limit: 10 }
])
```

Bu örnekte:

1. `$match` → aktif olan belgeleri filtreler
2. `$project` → sadece ad ve yaşı seçer
3. `$sort` → yaşa göre sıralar
4. `$limit` → ilk 10 belgeyi verir

Her aşama, bir **pipeline stage**’dir.

---

## 🎯 Pipeline'ın Avantajları

✅ Veriyi **katmanlı ve kontrollü** işleyebilirsin 
✅ Daha az bellek kullanımı
✅ SQL’deki karmaşık JOIN + GROUP + WHERE + SELECT işlemlerine karşılık gelir
✅ Optimize edilmesi kolaydır (başta `$match` ile filtreleme yapabilirsin)

---

## 🎨 Pipeline Görsel Temsili

```
Koleksiyon
   ↓
[ $match ]     --> sadece aktif kullanıcılar
   ↓
[ $project ]   --> sadece ad ve yaş
   ↓
[ $group ]     --> yaş ortalaması
   ↓
[ $sort ]      --> en büyükten küçüğe
   ↓
[ $limit ]     --> sadece 5 kişi
```

---

## 🧠 Kısa Tanım:

> **Pipeline**, verileri adım adım işleyen bir veri analiz zinciridir.  
> Her aşama, veriyi dönüştürür ve bir sonrakine aktarır.
