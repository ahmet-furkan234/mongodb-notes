
|Operatör|Ne yapar?|
|---|---|
|`$replaceOne`|**İlk eşleşen** parçayı değiştirir (ilk bulduğunu)|
|`$replaceAll`|**Tüm eşleşenleri** değiştirir|

---

## 📌 Ortak Söz Dizimi

```js
{
  $replaceOne: {
    input: <string>,
    find: <aranan>,
    replacement: <yeni_değer>
  }
}
```

`$replaceAll` için aynı yapı geçerlidir.

---

## 🧪 1. `$replaceOne` – İlk Eşleşeni Değiştir

```js
{
  $replaceOne: {
    input: "MongoDB öğren, MongoDB güçlüdür",
    find: "MongoDB",
    replacement: "🔥Mongo"
  }
}
```

📌 Çıktı: `"🔥Mongo öğren, MongoDB güçlüdür"`

---

## 🧪 2. `$replaceAll` – Tümünü Değiştir

```js
{
  $replaceAll: {
    input: "MongoDB öğren, MongoDB güçlüdür",
    find: "MongoDB",
    replacement: "🔥Mongo"
  }
}
```

📌 Çıktı: `"🔥Mongo öğren, 🔥Mongo güçlüdür"`

---

## 🧪 3. Alan Üzerinden Kullanım

```js
db.kullanicilar.aggregate([
  {
    $project: {
      temizTelefon: {
        $replaceAll: {
          input: "$telefon",
          find: "-",
          replacement: ""
        }
      }
    }
  }
])
```

📌 `"0555-123-4567"` → `"05551234567"`

---

## 🧪 4. Boşlukları Silme

```js
{
  $replaceAll: {
    input: " Ahmet   Yılmaz ",
    find: " ",
    replacement: ""
  }
}
```

📌 `"AhmetYılmaz"`

---

## 🧪 5. Noktalama Temizleme

```js
{
  $replaceAll: {
    input: "$adres",
    find: ",",
    replacement: ""
  }
}
```

📌 `"Ankara, Türkiye"` → `"Ankara Türkiye"`

---

## ⚠️ Dikkat Edilecek Noktalar

|Durum|Açıklama|
|---|---|
|`input` null ise|Sonuç da `null` olur|
|`find` boş string ise|Tüm karakter aralarına replacement ekler 😳|
|Harf büyük–küçük duyarlıdır|`"MongoDB"` ≠ `"mongodb"`|
|Regex desteklemez|`find` sadece düz metin olmalı (`$regex*` kullan regex için)|

---

## ✅ Özet

|Operatör|Açıklama|
|---|---|
|`$replaceOne`|İlk eşleşen kısmı değiştirir|
|`$replaceAll`|Tüm eşleşenleri değiştirir|
|Sadece düz string arar|Regex araması yapmaz|

---

Hazırsan devam edelim TkMatE! Sırada:

- 🔍 `$regexMatch`, `$regexFind`, `$regexFindAll` → Regex tabanlı eşleşmeler
    
- 📏 `$strLenCP`, `$indexOfCP` → String uzunluğu ve pozisyon
    
- 🔩 `$split` → Stringi parçalama
    
- 🔄 `$concat` + diğerleriyle birleşik kullanım örnekleri
    

Hangisini seçiyorsun kral?