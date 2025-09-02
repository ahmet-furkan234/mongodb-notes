
Bir belgedeki belirli alanın **var olup olmadığını** sorgulamak için kullanılır.

---

## 🔹 Sözdizimi:

```js
{ alanAdi: { $exists: true } }    // alan varsa eşleşir
{ alanAdi: { $exists: false } }   // alan yoksa eşleşir
```

---

## 🧪 Örnekler:

### 🎯 1. `notlar` alanı olan tüm öğrencileri getir:

```js
db.ogrenciler.find({
  notlar: { $exists: true }
})
```

### 🎯 2. `puanlar` alanı olmayan öğrencileri getir:

```js
db.ogrenciler.find({
  puanlar: { $exists: false }
})
```

---

## 🔥 Gelişmiş Kullanım:

### 📝 `reviews` alanı hem var, hem de boş olmayan belgeler:

```js
db.listings.find({
  reviews: {
    $exists: true,
    $not: { $size: 0 }
  }
})
```

---

## 🧠 Kullanım İpuçları:

- `$exists` **yalnızca alanın varlığını kontrol eder**, içeriği önemli değildir.
- `false` değeri, alanın hiç tanımlı olmadığı belgeleri getirir.
- Veri modelini kontrol etmek ve eksik alanları tespit etmek için çok faydalıdır.

---

## 🎯 Challenge Görevleri:

1. `notlar` alanı var olanlar
2. `puanlar` alanı olmayanlar
3. `adres` alanı olmayanlar
4. `telefon` alanı olanlar
5. `email` alanı olmayanlar
