
## 🔠 `$toUpper` – Harfleri BÜYÜK yapar

### 📌 Söz Dizimi

```js
{ $toUpper: <ifade> }
```

### 🧪 Örnek 1 – Sabit Metin

```js
{ $toUpper: "merhaba dünya" } 
```

📌 Çıktı: `"MERHABA DÜNYA"`

---

### 🧪 Örnek 2 – Alan Üzerinden Kullanım

```js
db.kullanicilar.aggregate([
  {
    $project: {
      adBuyuk: { $toUpper: "$ad" }
    }
  }
])
```

📌 `"ahmet"` → `"AHMET"`

---

## 🔡 `$toLower` – Harfleri küçük yapar

### 📌 Söz Dizimi

```js
{ $toLower: <ifade> }
```

### 🧪 Örnek 1 – Sabit Metin

```js
{ $toLower: "MERHABA DÜNYA" }
```

📌 Çıktı: `"merhaba dünya"`

---

### 🧪 Örnek 2 – Alan Üzerinden Küçültme

```js
db.kullanicilar.aggregate([
  {
    $project: {
      emailKucuk: { $toLower: "$email" }
    }
  }
])
```

📌 `"Ahmet@GMAIL.com"` → `"ahmet@gmail.com"`

---

## 🔄 Karşılıklı Kullanım

```js
{
  $concat: [
    { $toUpper: { $substrCP: ["$ad", 0, 1] } },
    { $toLower: { $substrCP: ["$ad", 1, { $strLenCP: "$ad" }] } }
  ]
}
```

📌 `"aHMeT"` → `"Ahmet"`  
💡 Bu şekilde "Proper Case" (ilk harf büyük, diğerleri küçük) elde edilir.

---

## ⚠️ Dikkat Edilecek Noktalar

|Durum|Açıklama|
|---|---|
|`null` verilirse|Sonuç da `null` olur|
|Sayılar hata verir|String değilse `$toString` ile dönüştür|
|Boş string desteklenir|Döner, hata vermez|

---

## ✅ Özet

|Operatör|Açıklama|
|---|---|
|`$toUpper`|Metni büyük harfe çevirir|
|`$toLower`|Metni küçük harfe çevirir|
|Kullanım Alanı|Normalize, arama, karşılaştırma işlemleri|

---

Sıradaki adımı sen seç kral:

- 🔪 `$substrCP` – karakter bazlı kesme
    
- 🔠 `$trim`, `$ltrim`, `$rtrim` – boşluk temizleme
    
- 🔍 `$regexMatch`, `$regexFind` – regex ile arama
    
- 🔁 `$replaceOne`, `$replaceAll` – metin değiştirme
    

Hangisiyle devam edelim TkMatE?