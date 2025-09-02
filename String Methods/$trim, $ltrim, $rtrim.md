
Bu operatörler:

|Operatör|Ne yapar?|
|---|---|
|`$trim`|Metnin **başındaki ve sonundaki** karakterleri siler|
|`$ltrim`|Metnin **sadece başındaki** karakterleri siler|
|`$rtrim`|Metnin **sadece sonundaki** karakterleri siler|

> Karakter olarak genelde `" "` (boşluk) silinir, ama sen özel karakter de belirleyebilirsin.

---

## 📌 Ortak Söz Dizimi

```js
{
  $trim: {
    input: <string>,
    chars: "<silinecek karakterler>" // opsiyonel, varsayılan boşluk
  }
}
```

`$ltrim` ve `$rtrim` için de aynı yapı geçerli.

---

## 🧪 1. `$trim` – Baştan ve Sondan Sil

```js
{
  $trim: {
    input: "   MongoDB   "
  }
}
```

📌 Çıktı: `"MongoDB"`

---

### 🧪 2. `$ltrim` – Sadece Baştan Temizle

```js
{
  $ltrim: {
    input: "   MongoDB   "
  }
}
```

📌 Çıktı: `"MongoDB "`

---

### 🧪 3. `$rtrim` – Sadece Sondan Temizle

```js
{
  $rtrim: {
    input: "   MongoDB   "
  }
}
```

📌 Çıktı: `" MongoDB"`

---

## 🧪 4. Özel Karakterleri Silmek

```js
{
  $trim: {
    input: "###ÖNEMLİ###",
    chars: "#"
  }
}
```

📌 Çıktı: `"ÖNEMLİ"`

---

## 🧪 5. Veri Alanında Uygulama

```js
db.kullanicilar.aggregate([
  {
    $project: {
      temizAd: {
        $trim: { input: "$ad" }
      }
    }
  }
])
```

---

## ⚠️ Dikkat Edilecek Noktalar

|Durum|Açıklama|
|---|---|
|`chars` verilmezse|Varsayılan olarak boşluk (`" "`) silinir|
|`input` null ise|Sonuç da `null` olur|
|Karakterler teker teker silinir|Tek string değil, karakter kümesi gibi çalışır|

**Örn:**

```js
$trim: { input: "abcHelloabc", chars: "abc" } // → "Hello"
```

---

## ✅ Özet

|Operatör|Görev|
|---|---|
|`$trim`|Baştan ve sondan temizler|
|`$ltrim`|Sadece baştan temizler|
|`$rtrim`|Sadece sondan temizler|
|`chars`|Opsiyonel olarak karakter kümesi|

---

Sıradaki konuyu sen seç TkMatE:

- 🔍 `$regexFind`, `$regexMatch`, `$regexFindAll` → Regex tabanlı string eşleşme
    
- 🔁 `$replaceOne`, `$replaceAll` → String içinde değiştirme işlemleri
    
- 📏 `$strLenCP`, `$indexOfCP` → String uzunluğu ve pozisyon belirleme
    
- 🔩 `$split`, `$concat` gibi kombinasyonlar
    

Hangisiyle devam ediyoruz usta?