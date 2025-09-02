
- TTL index, MongoDB’de bir koleksiyon değil, **belirli bir alan için oluşturulan özel bir index türüdür.**
- Bu index sayesinde, koleksiyondaki belgeler belirlenen süreden (saniye cinsinden) sonra **otomatik olarak silinir.**
- Genellikle geçici veri, oturum (session), cache, log gibi kısa ömürlü veriler için kullanılır.

---

## ⚙️ Nasıl Çalışır?

1. **Bir tarih alanı (Date türünde)** belirtilir. Örneğin `createdAt` veya `lastAccessed`.
2. Bu alana göre index oluşturulur ve `expireAfterSeconds` parametresi ile zaman belirlenir.
3. MongoDB arka planda periyodik olarak bu alandaki tarih değerini kontrol eder.
4. Süresi dolan belgeler otomatik olarak **silinir.**

---

## 🛠️ TTL Index Oluşturma

Örnek: Belgeler 1 saat (3600 saniye) sonra otomatik silinsin.

```js
db.sessions.createIndex(
  { createdAt: 1 },
  { expireAfterSeconds: 3600 }
)
```

- `createdAt` alanı Date türünde olmalı.
- Bu index, `createdAt` alanındaki zamanın üzerinden 1 saat geçince belgeyi siler.

---

## 📋 Örnek Veri

```js
db.sessions.insertOne({
  userId: "user123",
  createdAt: new Date()
})
```

- Bu belge 1 saat sonra TTL index tarafından otomatik silinecek.

---

## ⏳ TTL Index ile İlgili Önemli Notlar

|Durum|Açıklama|
|---|---|
|Tarih Alanı Türü|Mutlaka **Date** olmalı|
|expireAfterSeconds|Silinme süresi saniye cinsinden|
|Süreyi geçmemiş belgeler kalabilir|TTL taraması her 60 saniyede bir çalışır|
|TTL sadece belge siler, güncellemez|Belgeyi otomatik siler, güncellemez|
|TTL Index tek alan içindir|Compound TTL index desteklenmez|
|TTL index ile capped collection uyumsuzdur|Capped koleksiyonlarda çalışmaz|

---

## ⚡ TTL Index Kullanım Senaryoları

- Oturum (session) yönetimi
- Cache verisi temizliği
- Geçici loglar
- Expire olması gereken token veya anahtarlar

---

## 🔧 TTL Index Sorgulama ve Silme

- Var olan indexleri listele:

```js
db.sessions.getIndexes()
```

- TTL index silmek için:

```js
db.sessions.dropIndex("createdAt_1")
```

---

## 🧠 TTL Index Performans

- MongoDB TTL index temizliği arka planda otomatik ve hafif çalışır.
- Çok yoğun veride veya çok kısa TTL’de performans sorunları yaşanabilir, buna göre süre ayarlanmalı.

---

# Özet

|Konu|Açıklama|
|---|---|
|TTL Index|Belge otomatik silinmesi için index türü|
|expireAfterSeconds|Silinme süresi (saniye)|
|Date Alanı|Tarih türünde alan kullanılır|
|Kullanım|Session, cache, log gibi geçici veriler|
