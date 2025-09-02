

- **Amaç:** MongoDB veritabanındaki **bozulmuş veri yapıları, indeksler veya dosya sistemi sorunlarını onarmak**.
- `validate()` hatalı sonuç döndürürse veya MongoDB düzgün başlatılamıyorsa `repairDatabase()` çağrılabilir.

---

## 📌 Ne Yapar?

|İşlem Adımı|Açıklama|
|---|---|
|✅ Koleksiyonu ve indeksleri okur|Okunabilir verileri bulur|
|🔄 Geçici dosyalar oluşturur|Yeni sağlıklı kopyalar üretir|
|🧹 Bozuk verileri atar|Okunamayan veya bozuk kayıtlar atılır|
|💾 Veritabanını yeniden yazar|Yeni bir dosya seti oluşturur (`.wt` dosyaları)|

---

## ⚠️ Çok Kritik Uyarılar

|⚠️ Uyarı|Açıklama|
|---|---|
|❗ **Veri kaybı olabilir**|Bozuk belgeler geri alınamaz|
|❗ **Kullanıcı erişimi kısıtlanmalı**|`repairDatabase()` çalışırken veritabanına erişim olmamalı|
|❗ **Yedek alınmalı**|Komut çalıştırılmadan önce mutlaka backup alınmalı|
|🕐 İşlem uzun sürebilir|Büyük veri setlerinde çok yavaş olabilir|
|📁 Diskte iki katı boş alan olmalı|MongoDB yeni dosyaları oluşturmak için ek alan ister|

---

## 🔧 Kullanım Şekli

`repairDatabase()` bir **veritabanı seviyesinde** çalışır. Yani sadece aktif (`use database`) veritabanı üzerinde etkilidir.

### 💻 Shell'de çalıştırmak için:

```js
use mydatabase
db.repairDatabase()
```

---

## 🔒 Yalnızca Okuma / Yazma İzinleri

`repairDatabase()` çalıştırmak için kullanıcı şu yetkilere sahip olmalıdır:

```json
{ role: "dbAdmin", db: "mydatabase" }
```

---

## 💡 Gerçek Kullanım Senaryoları

### 1. `validate()` komutu `valid: false` döndürürse:

```js
db.users.validate()
// valid: false ise
db.repairDatabase()
```

### 2. MongoDB başlarken veri dosyaları bozuksa:

Terminal hatası:

```
Data file corrupted... can't recover...
```

Terminalden tamir:

```bash
mongod --repair --dbpath /var/lib/mongodb
```

> Bu komut MongoDB sunucusunu repair modunda başlatır.

---

## 🧠 Arkada Ne Olur?

1. MongoDB mevcut `.wt` (WiredTiger) dosyalarını okur.
2. Okunabilen tüm veriler yeni geçici koleksiyonlara aktarılır.
3. İndeksler yeniden oluşturulur.
4. Eski dosyalar silinir, yenileri yazılır.
5. `.ns` ve `.wt` dosyaları yeniden oluşturulur.

---

## 📝 Örnek Sonuç

```json
{
  "ok" : 1
}
```

Eğer bozuk belge veya indeks varsa:

```json
{
  "ok" : 0,
  "errmsg": "record corruption found...",
  "code": 136,
  "codeName": "DataCorruption"
}
```

---

## 📦 Alternatif: Dump ve Restore

Eğer `repairDatabase()` yeterli olmazsa veya veri çok kritikse:

```bash
mongodump --db=mydatabase
mongorestore --db=mydatabase_bak
```

> Bu işlem daha güvenli ve kontrollü bir onarma yöntemidir.

---

## 🧪 repairDatabase() vs validate()

|Özellik|`validate()`|`repairDatabase()`|
|---|---|---|
|Ne yapar?|Sadece kontrol eder|Veri yapısını düzeltir|
|Veri kaybı?|❌ Hayır|✅ Mümkün|
|Hızlı mı?|✅ Evet|❌ Yavaş olabilir|
|Üretimde çalıştırma|✅ Güvenli|⚠️ Dikkatli olunmalı|

---

## ✅ Özet

|Özellik|Açıklama|
|---|---|
|Komut|`db.repairDatabase()`|
|Etki Alanı|Aktif veritabanı|
|Kullanım Amacı|Bozuk veri yapısını onarmak|
|Risk|Veri kaybı olabilir|
|Alternatif|`mongodump + mongorestore`|
