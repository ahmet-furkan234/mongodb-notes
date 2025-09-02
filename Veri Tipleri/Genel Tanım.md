	
MongoDB, **BSON (Binary JSON)** formatında verileri saklar. BSON, JSON’un binary bir uzantısıdır ve fazladan veri tiplerini destekler. İşte başlıca veri tipleri:

---

### 1️⃣ **String**

- 📌 Metin verilerini saklamak için kullanılır.
- UTF-8 formatında tutulur.

👉 **Örnek:**

```json
{ "ad": "Ahmet" }
```

---

### 2️⃣ **Number / Numeric Types**

MongoDB'de sayılar birkaç türde tutulabilir:

- `int32` → 32 bit tamsayı (ör. `{ "yas": 25 }`)
- `int64` → 64 bit tamsayı (ör. `{ "buyukYas": NumberLong(9223372036854775807) }`)
- `double` → Ondalıklı sayı (ör. `{ "puan": 99.5 }`)
- `decimal128` → Yüksek hassasiyetli ondalıklı sayı (ör. finansal işlemler)

👉 **Örnek:**

```json
{ "puan": 99.5 }
```

---

### 3️⃣ **Boolean**

- `true` veya `false` değerler için.

👉 **Örnek:**

```json
{ "aktif": true }
```

---

### 4️⃣ **Date**

- Tarih ve saat bilgisi (milisaniye cinsinden epoch time olarak tutulur).
- Otomatik olarak ISODate formatında gösterilir.

👉 **Örnek:**

```json
{ "olusturmaTarihi": ISODate("2025-07-05T20:00:00Z") }
```


---

### 5️⃣ **ObjectId**

- MongoDB tarafından her belgeye otomatik verilen 12 baytlık benzersiz kimlik.
- Zaman damgası ve rasgele baytları içerir.

👉 **Örnek:**

```json
{ "_id": ObjectId("60c5ba7f7f1f4d23e4d8b12c") }
```

---

### 6️⃣ **Array**

- Liste şeklinde veriler.

👉 **Örnek:**

```json
{ "hobiler": ["yüzme", "satranç", "kodlama"] }
```

---

### 7️⃣ **Embedded Document / Object**

- Belgelerin içinde başka belgeler.

👉 **Örnek:**

```json
{
  "adres": {
    "sehir": "Ankara",
    "postaKodu": "06000"
  }
}
```

---

### 8️⃣ **Null**

- Boş veya bilinmeyen değer.

👉 **Örnek:**

```json
{ "telefon": null }
```

---

### 9️⃣ **Binary Data**

- Dosyalar, resimler gibi binary veriler için.

👉 **Örnek:**

```json
{ "dosya": BinData(0,"SGVsbG8gd29ybGQ=") }
```

---

### 1️⃣0️⃣ **Regular Expression**

- Regex ile desen eşleme.

👉 **Örnek:**

```json
{ "ad": { "$regex": "^A" } }
```

---

### 1️⃣1️⃣ **Timestamp**

- Versiyonlama ve değişim zamanı için özel timestamp türü.

👉 **Örnek:**

```json
{ "guncelleme": Timestamp(1623532800, 1) }
```

---

### 1️⃣2️⃣ **MinKey / MaxKey**

- Sorgularda karşılaştırmalar için en küçük ve en büyük değerler.  

👉 **Örnek:**

```json
{ "deger": { "$gt": MinKey } }
```

```json
{ "deger": { "$lt": MaxKey } }
```

---

## 📌 **Özet Tablo**

|Tür|Açıklama|Örnek|
|---|---|---|
|String|Metin|`"ad": "Ali"`|
|Int32/Int64|Tamsayı|`"yas": 25`|
|Double|Ondalık sayı|`"puan": 88.5`|
|Decimal128|Hassas ondalık|`"bakiye": NumberDecimal("123.45")`|
|Boolean|Doğru/yanlış|`"aktif": true`|
|Date|Tarih-saat|`ISODate("2025-07-05T20:00:00Z")`|
|ObjectId|Benzersiz belge kimliği|`ObjectId(...)`|
|Array|Liste|`["a", "b", "c"]`|
|Embedded Doc|İç içe belge|`{ adres: {...} }`|
|Null|Boş değer|`null`|
|Binary|İkili veri|`BinData(...)`|
|Regex|Desen eşleme|`{ "$regex": ".*Ali.*" }`|
|Timestamp|Sistem timestamp|`Timestamp(...)`|
|MinKey / MaxKey|Karşılaştırma sınırı|`{ "$gt": MinKey }`|

---

## ⚡ **Dikkat Edilecekler**

✅ BSON sayesinde MongoDB, JSON’a göre daha fazla türü destekler (ör. `ObjectId`, `Decimal128`).  
✅ Tablolar arası ilişki yerine iç içe belgeler (embedded document) kullanılır.  
✅ MongoDB’de veri tipi esnek ama sorgularda doğru tip önemlidir.