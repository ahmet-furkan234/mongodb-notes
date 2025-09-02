
## 🔍 `\d{3}` Nedir?

### 📌 Anlamı:

- `\d` → **digit** demektir yani **bir rakam** (0–9 arası)
- `{3}` → **tam olarak 3 tane** anlamına gelir

🔁 Yani `\d{3}` → **ardışık olarak tam 3 rakam** anlamına gelir.

---

## 🧪 Örnek: `\d{3}` eşleşir:

- ✅ `"123"`
- ✅ `"000"`
- ✅ `"987"`
- ❌ `"12"` → Yalnızca 2 rakam
- ❌ `"1234"` → 4 rakam, fazla

---

## 📦 Senin Örneğinde Kullanımı

```js
telefon: { $regex: /^\+90\s\(\d{3}\)\s\d{3}\s\d{4}$/ }
```

### Açılım:

|Bölüm|Anlamı|
|---|---|
|`^`|Satırın başı|
|`\+90`|"+90" (escape ile + işareti)|
|`\s`|1 adet boşluk|
|`\(` `\)`|Parantez aç/kapa|
|`\d{3}`|3 basamaklı alan kodu (örn: 532)|
|`\s`|boşluk|
|`\d{3}`|3 haneli numara (ilk bölüm)|
|`\s`|boşluk|
|`\d{4}`|4 haneli numara (son bölüm)|
|`$`|Satırın sonu|

---

### 📞 Eşleşen örnek:

```
+90 (532) 456 7890
```

### ❌ Eşleşmeyenler:

```
0532 456 7890       // çünkü başında +90 yok  
+905324567890       // boşluk ve parantez eksik  
+90(532)456-7890    // boşluk ve karakter eksik  
```

---

## 🔎 Daha Fazla `\d{}` Tabanlı Pattern

|Pattern|Anlamı|Eşleşen Örnek|
|---|---|---|
|`\d{2}`|Tam 2 rakam|`01`, `99`|
|`\d{4,}`|En az 4 rakam|`1234`, `99999`|
|`\d{1,3}`|1 ila 3 rakam|`5`, `12`, `321`|
|`^\d{11}$`|Sadece 11 rakamdan oluşan|`12345678901`|

---

İstersen şimdi `\d`, `\w`, `.` gibi diğer regex kısa yollarını da anlatabilirim  
veya `$mod`, `$expr` gibi yeni operatörlere geçebiliriz.

Ne dersin TkMatE?

Harika yakaladın TkMatE!  
Regex’teki `{2,}` ifadesi çoğu kişinin kafasını karıştırır — hemen detaylıca açıklayayım:

---

## 🔍 `{2,}` Ne Demek?

Bu bir **nicelik belirleyicidir** ve şu anlama gelir:

> **En az 2 tekrar**, üst sınır **yok**

---

### 📌 Açılım:

- `{n}`   ⇒ **Tam olarak n defa tekrar**
- `{n,}`  ⇒ **En az n defa tekrar**
- `{n,m}` ⇒ **En az n, en çok m defa tekrar**

---

## ✅ Senin Örneğinde:

```js
email: { $regex: /^[a-z0-9.-]+@[a-z0-9.-]+\.[a-z]{2,}$/ }
```

### 🔍 Sondaki `. + [a-z]{2, }` kısmı:

```js
\.[a-z]{2,}$
```

### Anlamı:

- `\.`   → Nokta (literal nokta)
- `[a-z]{2,}` → En az 2 harften oluşan uzantı
- `$`   → Satırın sonu

### 🔎 Eşleşen örnekler:

|Email|Eşleşir mi?|Neden|
|---|---|---|
|`ali@gmail.com`|✅ Evet|`.com` → 3 harf|
|`veli@firma.net`|✅ Evet|`.net` → 3 harf|
|`ayse@edu.tr`|✅ Evet|`.tr` → 2 harf|
|`mehmet@site.c`|❌ Hayır|`.c` → sadece 1 harf|
|`fatma@site.company`|✅ Evet|`.company` → 7 harf|

---

## 🧠 Ekstra Bilgi: Daha Sıkı Uzantı Kontrolü

Eğer **en fazla 4 harfli** bir domain uzantısı isteseydik:

```js
\.[a-z]{2,4}$
```

Bu sadece `.tr`, `.com`, `.info` gibi uzantılara izin verirdi; `.company` gibi uzun olanlar eşleşmezdi.

---

## 📌 Özet

|Regex|Açıklama|
|---|---|
|`{2}`|Tam olarak 2 tekrar|
|`{2,}`|En az 2 tekrar (sınırsız)|
|`{2,4}`|En az 2, en fazla 4 tekrar|
