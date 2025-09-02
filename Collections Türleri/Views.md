
- **View**, MongoDB’de gerçek veri içermeyen, başka koleksiyonlardan türetilmiş **sanal koleksiyonlardır**.
- SQL’deki **View** kavramına benzer.
- View, altında yatan koleksiyonlara yapılan sorgunun sonucunu döner.
- Sadece **okuma** amaçlıdır, doğrudan üzerinde veri ekleme/güncelleme/silme yapılamaz.
- View, oluşturulduğu anda verilen sorgu pipeline’ını her erişimde uygular.

---

## ⚙️ Özellikleri

|Özellik|Açıklama|
|---|---|
|Gerçek veri tutmaz|Altta yatan koleksiyondan veri getirir|
|Okuma amaçlıdır|Sadece sorgu ve okuma yapılabilir|
|Güncelleme yapılamaz|Veri ekleme, silme veya güncelleme yapılamaz|
|Performans|Karmaşık sorguları önceden tanımlamak için kullanılır|
|Pipeline destekler|Aggregation pipeline kullanarak tanımlanır|

---

## 🛠️ View Oluşturma

### Basit View

```js
db.createView(
  "activeUsers",
  "users",
  [
    { $match: { active: true } }
  ]
)
```

- `users` koleksiyonundaki sadece `active: true` olan kullanıcıları dönen bir view oluşturur.
- `activeUsers` adında yeni bir sanal koleksiyon oluşur.

---

## 🔍 View Kullanımı

```js
db.activeUsers.find()
```

- Altta yatan `users` koleksiyonunda `active: true` olan belgeler getirilir. 

---

## ⚠️ View Üzerinde Limitasyonlar

- View’lar üzerinde **insert, update, delete işlemi yapılamaz**.
- View’lar, karmaşık aggregation pipeline’larını tekrar tekrar yazmamak için kolaylık sağlar.
- Bazı aggregation aşamaları view içinde desteklenmeyebilir (örneğin `$out`).

---

## 🧩 Gelişmiş View Örneği

```js
db.createView(
  "recentOrders",
  "orders",
  [
    { $match: { orderDate: { $gte: new ISODate("2025-01-01") } } },
    { $project: { orderId: 1, customerId: 1, total: 1 } }
  ]
)
```

- `orders` koleksiyonundan sadece 2025 yılından sonraki siparişleri seçer.
- Sadece belli alanları döner.

---

## Performans Notları

- View sorguları altta yatan koleksiyona yönlendirilir, **view’ın kendisi bir veri depolamaz**.
- Karmaşık view’lar sorgu performansını etkileyebilir.
- İyi tasarlanmış indexler performansı artırır.

---

## Özet Tablosu

|Özellik|Açıklama|
|---|---|
|Veri Saklama|Yok, sadece sorgu sonucu döner|
|Güncelleme|Yok|
|Kullanım Amacı|Karmaşık sorguları tekrar kullanmak|
|Performans|Altta yatan koleksiyonun sorgusu hızına bağlı|
