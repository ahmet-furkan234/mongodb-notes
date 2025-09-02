
MongoDB'de bir kullanıcıya **yetki** vermek için **rol (role)** tanımlanır.

### 🎯 Bir kullanıcıya:

- ya **hazır (built-in) bir rol**,
- ya da **özel olarak senin oluşturduğun bir rol** atanabilir.

---

## 🧱 1. MongoDB'deki Hazır Roller (Built-in Roles)

### 🟢 **Veritabanı Seviyesindeki Roller**

Bu roller bir veritabanına özel işler yapmanı sağlar.

|Rol Adı|Açıklama|
|---|---|
|`read`|Verileri okur ama yazamaz|
|`readWrite`|Okur ve yazar|
|`dbAdmin`|DB yapısını yönetir (index, validate, stats)|
|`userAdmin`|Kullanıcı ve rol yönetimi|
|`dbOwner`|DB içindeki tüm haklar (read, write, admin)|

---

### 🔵 **Sunucu Seviyesindeki Roller (admin DB'de tanımlanır)**

|Rol Adı|Açıklama|
|---|---|
|`readAnyDatabase`|Tüm veritabanlarını okur|
|`readWriteAnyDatabase`|Tüm veritabanlarında okur-yazar|
|`userAdminAnyDatabase`|Tüm DB'lerde kullanıcı yönetimi|
|`dbAdminAnyDatabase`|Tüm DB'lerde DB yönetimi|
|`root`|Tüm sistem üzerinde tam yetki|
|`clusterAdmin`|Sunucu yönetimi (replica set, backup, config)|

---

## 🔨 2. Özel Rol Oluşturma (Custom Role)

Örneğin: Sadece `products` koleksiyonuna `insert` yetkisi olan özel bir rol oluşturalım.

### 2.1. Role Tanımı

```js
use admin

db.createRole({
  role: "insertProductsOnly",
  privileges: [
    {
      resource: { db: "store", collection: "products" },
      actions: [ "insert" ]
    }
  ],
  roles: []
})
```

- `resource`: Hangi DB ve koleksiyona uygulanacağı
- `actions`: Bu kaynakta neye izin verileceği
- `roles`: Diğer rollerin dahil edilip edilmeyeceği (rol içinde rol)

---

### 2.2. Kullanıcıya Rol Atama

```js
db.createUser({
  user: "productInserter",
  pwd: "pass123",
  roles: ["insertProductsOnly"]
})
```

---

## 📘 3. Özel Rol Oluşturulabilecek `actions` Listesi

MongoDB'de özel roller için kullanılabilecek **actions** (yetkiler) şunlardır:

### 🎯 CRUD için:

- `"find"` → okuma (select)
- `"insert"` → ekleme
- `"update"` → güncelleme
- `"remove"` → silme

### 🧰 DB yönetimi için:

- `"createCollection"` → koleksiyon oluşturma
- `"createIndex"` → index oluşturma
- `"dropCollection"` → koleksiyon silme
- `"collStats"` → istatistik çekme
- `"validate"` → veri yapısı doğrulama

### 👤 Kullanıcı yönetimi için:

- `"createUser"`
- `"dropUser"`
- `"grantRole"`
- `"revokeRole"`

### 🚀 Diğer örnek yetkiler:

- `"listDatabases"`
- `"killCursors"`
- `"compact"`
- `"serverStatus"`

🔗 [Resmi action listesi (MongoDB Docs)](https://www.mongodb.com/docs/manual/reference/privilege-actions/)

---

## 🧪 4. Örnek: Bir rol tüm ürünleri okuyabilsin ama kullanıcı bilgilerine erişemesin

```js
db.createRole({
  role: "productReader",
  privileges: [
    {
      resource: { db: "store", collection: "products" },
      actions: ["find"]
    }
  ],
  roles: []
})
```

---

## 🔄 5. Role Güncelleme

```js
db.updateRole("productReader", {
  privileges: [
    {
      resource: { db: "store", collection: "products" },
      actions: ["find", "update"] // update yetkisi eklendi
    }
  ]
})
```

---

## 🧼 6. Role Silme

```js
db.dropRole("productReader")
```

---

## 📋 7. Tanımlı Roller Listesi

```js
db.getRoles({ showBuiltinRoles: false })
```

> `true` verirsen hazır rolleri de listeler.

---

## ✅ Özetle

|İşlem|Komut|
|---|---|
|Rol oluştur|`db.createRole({...})`|
|Role sahip kullanıcı oluştur|`db.createUser({... roles: [...] })`|
|Role yetki ekle|`db.updateRole(...)`|
|Rolü sil|`db.dropRole(...)`|
|Roller listesi|`db.getRoles()`|
