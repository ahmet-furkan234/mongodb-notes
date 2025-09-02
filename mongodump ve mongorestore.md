
- **`mongodump`** → MongoDB veritabanını veya koleksiyonu dışa aktarır (backup).
- **`mongorestore`** → Daha önce alınmış yedeği MongoDB’ye tekrar yükler.

---

## 📦 `mongodump` (Veri Yedekleme)

### ✅ Temel Kullanım:

```bash
mongodump --db=mydatabase
```

> `mydatabase` veritabanını yedekler ve bulunduğun klasöre `dump/` adında klasör oluşturur.

### 📁 Örnek Çıktı Yapısı:

```
dump/
└── mydatabase/
    ├── users.bson
    └── users.metadata.json
```

---

### 🧩 Sadece bir koleksiyonu yedeklemek:

```bash
mongodump --db=mydatabase --collection=users
```

---

### 🛣 Hedef klasörü belirlemek:

```bash
mongodump --db=mydatabase --out=/yedekler/mongo
```

> Yedeği istediğin klasöre alır.

---

### 🔑 Kullanıcı adı/şifre ile yedek:

```bash
mongodump --host=localhost --port=27017 --username=admin --password=123456 --authenticationDatabase=admin --db=mydatabase
```

---

## ♻️ `mongorestore` (Veriyi Geri Yükleme)

### ✅ Temel Kullanım:

```bash
mongorestore --db=mydatabase dump/mydatabase
```

> `dump/mydatabase` klasöründen verileri `mydatabase` isimli veritabanına geri yükler.

---

### 🧩 Sadece bir koleksiyonu geri yüklemek:

```bash
mongorestore --db=mydatabase --collection=users dump/mydatabase/users.bson
```

---

### 🔄 Var olan veriyi silmeden eklemek (varsayılan):

```bash
mongorestore --db=mydatabase --drop dump/mydatabase
```

> `--drop` eklenirse, eski veriler silinir ve yenisi yüklenir (çok dikkatli olunmalı!).

---

### 🔒 Yetkili kullanıcıyla restore:

```bash
mongorestore --username=admin --password=123456 --authenticationDatabase=admin --db=mydatabase dump/mydatabase
```

---

## 🧠 Dump ve Restore Senaryosu

### 1. Backup alma (yedekleme):

```bash
mongodump --db=okul --out=/yedekler/07-07-2025
```

### 2. Veri bozulursa veya başka sunucuya taşıma gerekirse:

```bash
mongorestore --db=okul --drop /yedekler/07-07-2025/okul
```

---

## 🧪 Dump ve Restore Test Etmek İçin Küçük Demo

### Adım 1: Bir veri oluştur

```js
use testDB
db.students.insertMany([
  { name: "Ali", age: 20 },
  { name: "Ayşe", age: 21 }
])
```

### Adım 2: Dump

```bash
mongodump --db=testDB
```

### Adım 3: Veriyi sil

```js
db.students.drop()
```

### Adım 4: Geri yükle

```bash
mongorestore --db=testDB dump/testDB
```

> 🎉 `students` koleksiyonu geri gelir.

---

## 🛡️ Dump Dosyası Nerede Saklanır?

Varsayılan olarak bulunduğun dizine `dump/` klasörü olarak oluşturulur. Bu klasör taşınabilir, sıkıştırılabilir veya başka sunucuya gönderilebilir.

---

## 🔁 Dump/Restore ile Taşıma Senaryosu

**Bir sunucudan diğerine veri taşımak:**

1. Kaynak sunucuda:

```bash
mongodump --db=urunler --out=./yedek
```

2. Dump dosyasını hedef sunucuya taşı:

```bash
scp -r ./yedek root@192.168.1.100:/yedek
```

3. Hedef sunucuda:

```bash
mongorestore --db=urunler /yedek/urunler
```

---

## ✅ Özet

|Komut|Görev|
|---|---|
|`mongodump`|Yedek alır (`.bson` formatında)|
|`mongorestore`|Yedeği geri yükler|
|`--db`|Hangi veritabanı|
|`--collection`|Belirli koleksiyon|
|`--drop`|Yüklemeden önce veriyi siler|
