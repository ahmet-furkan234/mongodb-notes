

## 🧠 Temel Bilgi: Database User Nedir?

- Sadece **bir veritabanına bağlanmak** ve **veritabanı işlemleri** yapmak için tanımlanırlar.
- Komutlar genellikle `admin` veritabanı üzerinde çalışır.
- Kullanıcılar veritabanına özgü rollerle yetkilendirilir.

---

## 🔧 1. `db.createUser()` – Kullanıcı Oluşturma

```js
use admin

db.createUser({
  user: "myUser",
  pwd: "myStrongPassword123",
  roles: [
    { role: "readWrite", db: "myDatabase" }
  ]
})
```

### 🔑 Parametreler:

- `user`: Kullanıcı adı
- `pwd`: Şifre
- `roles`: Kullanıcının sahip olacağı yetkiler

---

## 🛠️ 2. `db.updateUser()` – Kullanıcı Güncelleme (roller, parola vs.)

```js
use admin

db.updateUser("myUser", {
  pwd: "newSecurePassword456",  // parola güncelle
  roles: [
    { role: "dbAdmin", db: "myDatabase" },
    { role: "read", db: "logsDb" }
  ]
})
```

---

## ❌ 3. `db.dropUser()` – Kullanıcı Silme

```js
use admin

db.dropUser("myUser")
```

---

## 📜 4. `db.getUsers()` – Tüm Kullanıcıları Listeleme

```js
use admin

db.getUsers()
```

> Her kullanıcı hakkında username, roles, db gibi bilgiler verir.

---

## 🔍 5. `db.getUser()` – Belirli Bir Kullanıcıyı Getir

```js
db.getUser("myUser")
```

---

## ✅ 6. `db.auth()` – Manuel Bağlantı (Authenticate)

Konsolda kullanıcı doğrulaması yapmak için:

```js
db.auth("myUser", "myStrongPassword123")
```

---

## 🧹 7. `db.dropAllUsers()` – Tüm Kullanıcıları Sil

⚠️ Tehlikeli bir komut, tüm kullanıcıları kaldırır.

```js
db.dropAllUsers()
```

---

## 🔐 8. `db.changeUserPassword()` – Sadece Şifre Değiştirme

```js
db.changeUserPassword("myUser", "newPass789")
```

---

## 🧭 9. Role Tanımlı Kullanıcı Oluşturma (Özel Rollerle)

```js
db.createUser({
  user: "reportUser",
  pwd: "pass123",
  roles: [
    { role: "read", db: "analytics" },
    { role: "readWrite", db: "temp" }
  ]
})
```

---

## 🔐 10. Özel Role ile Kullanıcı Oluşturma

Önce role oluşturulur:

```js
db.createRole({
  role: "onlyInsertProducts",
  privileges: [
    {
      resource: { db: "shop", collection: "products" },
      actions: ["insert"]
    }
  ],
  roles: []
})
```

Sonra kullanıcıya atanır:

```js
db.createUser({
  user: "insertUser",
  pwd: "onlyInsert123",
  roles: ["onlyInsertProducts"]
})
```

---

## 🎯 Kullanıcı Rollerinin Tam Listesi

|Rol|Açıklama|
|---|---|
|`read`|Sadece okuma|
|`readWrite`|Okuma + yazma|
|`dbAdmin`|DB yapı yönetimi (index, validate vb.)|
|`userAdmin`|Kullanıcı yönetimi|
|`clusterAdmin`|Sunucu/genel yönetim rolleri|
|`readAnyDatabase`|Tüm DB'lerde okuma|
|`root`|Tüm yetkiler|
|`dbOwner`|DB üzerinde tüm işlemler|
|`backup`, `restore`|Yedekleme işlemleri|

---

## 🧪 Kullanıcı ile Bağlanmak

Bağlantı komutu:

```bash
mongo "mongodb+srv://cluster0.mongodb.net/myDatabase" \
  --username myUser \
  --password myStrongPassword123
```

---

## 🧼 Öneri: Her işlemden sonra şu komutu kullan

```js
db.getUser("myUser")
```

👉 Kullanıcının gerçekten doğru rollerle atanıp atanmadığını görmek için.
