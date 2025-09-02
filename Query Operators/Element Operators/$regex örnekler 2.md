
## 🔹 BAŞLANGIÇ SEVİYESİ ÖRNEKLER

---

### 🎯 1. `ad` içinde "ah" geçen öğrenciler:

```js
db.ogrenciler.find({
  ad: { $regex: /ah/ }
})
```

---

### 🎯 2. `soyad` "oğlu" ile bitenler:

```js
db.ogrenciler.find({
  soyad: { $regex: /oğlu$/ }
})
```

---

### 🎯 3. `email` "gmail" ile başlayanlar:

```js
db.ogrenciler.find({
  email: { $regex: /^gmail/ }
})
```

---

### 🎯 4. `telefon` sadece rakamlardan oluşuyorsa:

```js
db.ogrenciler.find({
  telefon: { $regex: /^[0-9]+$/ }
})
```

---

### 🎯 5. `ad` sadece harf içeriyorsa:

```js
db.ogrenciler.find({
  ad: { $regex: /^[a-zA-Z]+$/ }
})
```

---

## 🔹 ORTA SEVİYE ÖRNEKLER

---

### 🎯 6. `ad` "a" ile başlıyor, 5 harfli:

```js
db.ogrenciler.find({
  ad: { $regex: /^a.{4}$/ }
})
```

> `.` → herhangi bir karakter  
> `{4}` → 4 karakter  
> `a + 4 karakter = 5 karakter`

---

### 🎯 7. `email` sonunda `.edu.tr` varsa:

```js
db.ogrenciler.find({
  email: { $regex: /\.edu\.tr$/ }
})
```

---

### 🎯 8. `ad` küçük harf ile başlıyor (içeriği önemli değil):

```js
db.ogrenciler.find({
  ad: { $regex: /^[a-z]/ }
})
```

---

### 🎯 9. `ad` başında 1 büyük harf, ardından 2 küçük harf:

```js
db.ogrenciler.find({
  ad: { $regex: /^[A-Z][a-z]{2}/ }
})
```

---

### 🎯 10. `email` alanı büyük/küçük harf fark etmeksizin "hotmail" içeriyor:

```js
db.ogrenciler.find({
  email: { $regex: /hotmail/i }
})
```

---

## 🔹 İLERİ SEVİYE – YAPILANDIRILMIŞ VERİ DESENLERİ

---

### 🎯 11. `tc` alanı tam olarak 11 rakamdan oluşuyorsa:

```js
db.ogrenciler.find({
  tc: { $regex: /^[0-9]{11}$/ }
})
```

---

### 🎯 12. `telefon` alanı “0XXX-XXX-XXXX” formatında ise:

```js
db.ogrenciler.find({
  telefon: { $regex: /^0[0-9]{3}-[0-9]{3}-[0-9]{4}$/ }
})
```

---

### 🎯 13. `ad` ya "Ali" ya da "Ayşe" olanlar:

```js
db.ogrenciler.find({
  ad: { $regex: /^(Ali|Ayşe)$/ }
})
```

---

### 🎯 14. `email` alanı @ işareti içeriyorsa (geçerli e-posta kontrolü için temel seviye):

```js
db.ogrenciler.find({
  email: { $regex: /@/ }
})
```

---

### 🎯 15. `adres` içinde "istanbul" kelimesi geçen (büyük/küçük harf duyarsız):

```js
db.ogrenciler.find({
  adres: { $regex: /istanbul/i }
})
```

---

## 🎯 Challenge: Regex Mantığını Kullanarak

Aşağıdakileri regex ile yapabilir misin?

1. 5 harfli, “m” ile başlayan adlar
    
2. Adı sadece harf ve boşluk içerenler
    
3. `.com`, `.net`, `.org` ile biten emailler
    
4. İçinde sayı bulunan adlar
    
5. Soyadı “-” içeriyorsa
    

---

Hazırsan birlikte regex challenge çözelim  
ya da sıradaki konu **`$mod`** gibi diğer evaluation operator olabilir.

Ne yapalım TkMatE?