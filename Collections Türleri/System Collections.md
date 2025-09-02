
- MongoDB’nin **iç yönetim ve metadata** işlemleri için kullandığı özel koleksiyonlardır.
- Kullanıcıların veri saklamak için oluşturduğu koleksiyonlardan farklıdır.
- Sistem tarafından otomatik yaratılır ve yönetilir.
- Genellikle normal CRUD işlemlerine açık değildir.
- İç yapıyı ve MongoDB’nin çalışma durumunu yönetir.

---

## 🔍 Örnek System Collections

|Koleksiyon Adı|Açıklama|
|---|---|
|`system.indexes`|Koleksiyonlardaki indexlerin bilgisi|
|`system.profile`|Profilling (performans ölçüm) verisi|
|`system.users`|Veritabanı kullanıcılarının bilgisi|
|`system.version`|Veritabanı sürüm bilgisi|
|`system.js`|JavaScript fonksiyonlarının saklandığı koleksiyon (server-side)|
|`system.namespaces`|Veritabanındaki koleksiyonların isim bilgisi (eski sürümlerde)|

---

## ⚙️ Özellikleri

- Bu koleksiyonlar MongoDB tarafından **otomatik yönetilir**.
- Kullanıcı müdahalesi **genellikle önerilmez** ve çoğu zaman izin verilmez.
- Sistem koleksiyonları veritabanı iç yapısının ve konfigürasyonun takibi içindir.

---

## 📌 Kullanım Alanları

- Index yönetimi ve izleme.
- Performans profil bilgisi toplama.
- Kullanıcı yetkilendirme ve erişim kontrolü.
- Server tarafında JavaScript fonksiyonlarının saklanması ve çağrılması.

---

## 🧩 Örnek: Profilling Verileri

Profiling etkinleştirildiğinde `system.profile` koleksiyonu kullanılır:

```js
db.system.profile.find().pretty()
```

- Burada sorgu süreleri, kilitlenmeler gibi performans verileri tutulur.

---

## 🛠️ Sistem Koleksiyonları ile Çalışmak

- **Genellikle direk müdahale önerilmez.**
- Ancak bazı durumlarda:
    - `system.js` ile sunucu tarafı JavaScript fonksiyonları ekleyip çağırabilirsin.
    - Profiling verilerini okuyabilirsin.
- `system.users` koleksiyonu artık yeni MongoDB sürümlerinde **kullanıcılar** için doğrudan erişim yok, bunun yerine `usersInfo` komutu veya Atlas UI kullanılır.

---

## 📋 Özet Tablosu

|System Collection|İşlevi|Kullanım Durumu|
|---|---|---|
|`system.indexes`|Index bilgileri|Otomatik, nadiren kullanılır|
|`system.profile`|Performans izleme|Profiling aktifse kullanılır|
|`system.users`|Kullanıcı bilgileri|Yeni sürümlerde alternatif yöntem var|
|`system.js`|Server-side JavaScript fonksiyonları|Özel durumlarda kullanılır|
|Diğerleri|İç yönetim|Genelde erişim kapalı|
