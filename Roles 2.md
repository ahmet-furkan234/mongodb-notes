## 🔍 Nedir bu 3. parametre?

```js
db.createRole({
  role: "rol_adi",
  privileges: [ ... ],
  roles: [ ... ]  // 🔥 3. parametre: bu role dahil edilen diğer roller
})
```

Bu parametre ile:

- Başka rollerin **yetkilerini** bu role miras aldırabilirsin (inheritance).
- Böylece aynı yetkileri tek tek `privileges` olarak yazmak zorunda kalmazsın.

---

## ✅ Örnek: Mevcut rollerin kullanıldığı bir custom role

Senaryomuz:

- `store` veritabanındaki `orders` koleksiyonuna sadece okuma izni verilsin (custom)
- Artı olarak bu kullanıcı tüm `store` DB’sinde okuma-yazma yetkisine de sahip olsun

```js
db.createRole({
  role: "ordersReaderWithRW",
  privileges: [
    {
      resource: { db: "store", collection: "orders" },
      actions: [ "find" ]
    }
  ],
  roles: [
    { role: "readWrite", db: "store" }  // 🔥 diğer yetkileri buradan al
  ]
})
```

Bu ne demek?

- `ordersReaderWithRW` rolü hem kendi `orders` özel yetkisini (okuma)
- Hem de `readWrite` rolünün sağladığı tüm yetkileri kullanabilir.

---

## 🔁 Örnek: Bir custom rol başka bir custom rolü içerebilir mi?

Evet!

Önce bir rol tanımlayalım:

```js
db.createRole({
  role: "readProductsOnly",
  privileges: [
    { resource: { db: "shop", collection: "products" }, actions: ["find"] }
  ],
  roles: []
})
```

Sonra başka bir role bunu dahil edelim:

```js
db.createRole({
  role: "inventoryManager",
  privileges: [
    { resource: { db: "shop", collection: "stock" }, actions: ["insert", "update"] }
  ],
  roles: [
    { role: "readProductsOnly", db: "admin" } // ✅ başka bir custom rolü içeri aldı
  ]
})
```

---

## 🎯 Ne zaman kullanılır?

|Durum|`roles` parametresi avantajı|
|---|---|
|Tekrarlayan yetkiler varsa|Başka roller eklenerek tekrar önlenir|
|Modüler rol sistemi istiyorsan|Küçük roller birleştirilerek esneklik sağlanır|
|Yetkileri gruplamak istiyorsan|Özel yetkileri ve hazır rolleri tek rolde toplayabilirsin|

---

## 💡 Ekstra İpucu: Hem `roles` hem `privileges` aynı anda kullanılabilir

Custom role = özel izinler (`privileges`) + başka roller (`roles`) kombinasyonu olabilir.

---

## 🔚 Özet

```js
db.createRole({
  role: "rol_adi",
  privileges: [ özel izinler ],
  roles: [ başka roller ]  // ✅ 3. parametre burası
})
```

Bu parametreyi, tıpkı C#’taki "inherit" gibi düşünebilirsin: başka rollerin yetkilerini devralır.
