
- `validate()` bir **koleksiyonun yapısını ve veri bütünlüğünü (integrity)** kontrol eden bir metottur.
- Genelde hata ayıklama (debug), bakım (maintenance) veya performans sorunları sırasında kullanılır.
- Veritabanı yöneticileri (DBA) tarafından, özellikle veri dosyaları bozulduğunda veya indeks hataları olduğunda çağrılır.

---

## 🧪 Kullanım Şekli:

```js
db.collection.validate()
```

🔧 Örnek:

```js
db.users.validate()
```

> Bu komut, `users` koleksiyonunun yapısal bütünlüğünü kontrol eder ve bir rapor döner.

---

## ⚙️ Gelişmiş Kullanım:

```js
db.collection.validate({ full: true })
```

- `full: true` parametresi, ek olarak **veri içeriğini ve her belgeyi detaylı** şekilde kontrol eder.
- Daha yavaş çalışır, ancak daha kapsamlı analiz sağlar.

---

## 🔄 Dönen Sonuç Yapısı:

`validate()` metodu aşağıdaki gibi bir sonuç döner:

```json
{
  "ns": "veritabaniAdi.koleksiyonAdi",
  "nrecords": 1500,
  "nIndexes": 3,
  "keysPerIndex": {
    "users._id_": 1500,
    "users.username_1": 1500
  },
  "valid": true,
  "errors": [],
  "warnings": [],
  "indexDetails": { ... }, // full: true ise gelir
  "ok": 1
}
```

### Açıklamaları:

|Alan|Açıklama|
|---|---|
|`ns`|Namespace → Hangi koleksiyon kontrol edildi|
|`nrecords`|Toplam belge sayısı|
|`nIndexes`|Toplam indeks sayısı|
|`keysPerIndex`|Her indeks için anahtar sayısı|
|`valid`|Koleksiyon tutarlı mı? (`true` veya `false`)|
|`errors`|Tespit edilen yapısal hatalar|
|`warnings`|Uyarılar (performans, tutarsızlık vb.)|
|`ok`|İşlem başarılı mı? (`1` başarılı, `0` hata)|

---

## 🛠 Ne Tür Hataları Gösterir?

- Bozuk indeks yapısı
- Veri ile indeks arasında tutarsızlık
- Eksik veya fazla anahtarlar (index key mismatch)
- Kötü dosya yapısı
- Yolsuz belge sayısı (corrupted record count)

---

## 🧠 Ne Zaman Kullanılır?

|Durum|Açıklama|
|---|---|
|📉 Performans düşüşü|Index bozuklukları performansı etkileyebilir.|
|❌ Sık sık sorgu hatası alınıyorsa|Veri bütünlüğü kontrol edilir.|
|🔄 Backup sonrası doğrulama|Yedekleme sonrası veri kontrolü yapılır.|
|🧪 Migration sonrası test|Veriler yeni bir sunucuya taşındıktan sonra kontrol.|

---

## 🧨 Not: `validate()` hiçbir belgeyi **silmez** veya **güncellemez**

- Bu bir analiz/denetim fonksiyonudur.
- Eğer sorun tespit edersen, **manuel müdahale** veya `repairDatabase()` gerekebilir.

---

## 🧪 `full: true` ile performans farkı

```js
db.kitaplar.validate({ full: true })
```

> Bu komut, sadece indeksleri değil, her belgeyi tek tek okuyarak iç tutarlılık kontrolü yapar. Büyük koleksiyonlarda **çok yavaş çalışabilir**.

---

## ⚠️ validate() ve Collection Types

- **Capped collections** (sabit boyutlu koleksiyonlar) üzerinde `validate()` çalışır ama özel uyarılar dönebilir.
- `timeseries` koleksiyonları için `validate()` farklı sonuçlar verir (dahili `buckets` yapısı kontrol edilir).

---

## ✅ Özet:

|Özellik|Açıklama|
|---|---|
|Amaç|Koleksiyon yapısal bütünlüğünü kontrol etmek|
|Değişiklik yapar mı?|❌ Hayır, sadece kontrol eder|
|Parametre|`full: true` ile daha detaylı doğrulama|
|Sonuç|`valid`, `errors`, `nrecords`, `nIndexes` vs.|

---

## 👨‍🔧 Bonus: Bütün koleksiyonları sırayla doğrulamak (script örneği)

```js
db.getCollectionNames().forEach(function(col) {
  const result = db.getCollection(col).validate();
  print(`Koleksiyon: ${col} - Geçerli mi? ${result.valid}`);
});
```
