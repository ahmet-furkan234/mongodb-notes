
`$split`, bir string ifadeyi **belirli bir ayırıcı karaktere göre bölerek bir diziye (array)** dönüştürür.  
Yani JavaScript’teki `"str".split(",")` fonksiyonunun aynısıdır.

---

## 📌 Söz Dizimi

```js
{
  $split: [ <string>, <ayırıcı> ]
}
```

|Parametre|Açıklama|
|---|---|
|`<string>`|Bölünecek metin|
|`<ayırıcı>`|Hangi karaktere göre bölüneceği|

---

## 🧪 1. Virgüle Göre Bölme

```js
{
  $split: ["Elma,Armut,Muz", ","]
}
```

📌 Çıktı: `["Elma", "Armut", "Muz"]`

---

## 🧪 2. Alan Üzerinde Bölme

```js
db.kullanicilar.aggregate([
  {
    $project: {
      isimParcalari: {
        $split: ["$tamIsim", " "]
      }
    }
  }
])
```

📌 `"Ahmet Yılmaz"` → `["Ahmet", "Yılmaz"]`

---

## 🧪 3. E-posta Bölme

```js
{
  $split: ["ahmet@gmail.com", "@"]
}
```

📌 `["ahmet", "gmail.com"]`

---

## 🧪 4. Nokta İle Bölme (örneğin IP adresi)

```js
{
  $split: ["192.168.1.10", "."]
}
```

📌 `["192", "168", "1", "10"]`

---

## 🧪 5. Tireli Telefon Numarası

```js
{
  $split: ["+90-532-000-0000", "-"]
}
```

📌 `["+90", "532", "000", "0000"]`

---

## ⚠️ Dikkat Edilecek Noktalar

|Konu|Açıklama|
|---|---|
|Ayırıcı bulunamazsa|Sonuç tek elemanlı dizi olur (girdi değişmeden kalır)|
|`""` (boş string) ile ayırırsan|Her harfi ayrı ayrı alır|
|`null` varsa|Sonuç da `null` olur|
|Sayılarla çalışmaz|Önce `$toString` kullanılır|

---

### 💥 İlginç Örnek – Harf Harf Bölme

```js
{
  $split: ["Merhaba", ""]
}
```

📌 `["M", "e", "r", "h", "a", "b", "a"]`

---

## ✅ Özet

|Özellik|Açıklama|
|---|---|
|`$split`|String ifadeyi **array**'e çevirir|
|Ayırıcı zorunlu|Karaktere göre bölme yapılır|
|Çok pratik|Ad-soyad ayırma, tag listesi, CSV bölme|
