
MongoDB’de **Binary Data**, ham ikili veriyi (binary data) doğrudan bir belge içinde saklamak için kullanılır.  
➡ BSON'un `0x05` tür koduyla temsil edilir.  
➡ Dosya parçaları, şifreli veri, imza, özel formatlı veri gibi her türlü ham binary bilgi saklanabilir.

---

## 🌟 **Özellikler**

✅ Herhangi bir formatta ikili veri (ör. resim, dosya, hash, şifreli bilgi) saklanabilir.  
✅ Binary data genelde **Base64** ya da hexadecimal formatta gösterilir (MongoDB bunu binary formatta tutar).  
✅ Binary alanı için maksimum belge boyutu sınırı geçerlidir (16MB).  
✅ Binary data için ayrıca `BinData(subtype, base64string)` biçimi kullanılır.

---

## 📝 **Örnek Belge**

```json
{
  "_id": ObjectId("..."),
  "dosyaAdi": "ornek.jpg",
  "icerik": BinData(0, "SGVsbG8gVEtNQVRF")  // "Hello TKMATE" base64 karşılığı
}
```

➡ `icerik` alanı ikili veriyi (örneğin bir dosyanın içeriğini) saklıyor.

---

## ⚡ **Binary Data Subtype'ları**

|Subtype|Açıklama|
|---|---|
|`0x00`|Generic binary data|
|`0x01`|Function|
|`0x02`|Binary (old)|
|`0x03`|UUID (old)|
|`0x04`|UUID (RFC 4122 standard)|
|`0x05`|MD5|
|`0x80` ve üstü|Kullanıcı tanımlı|

➡ **En yaygın subtype:** `0x00` (generic binary)

---

## ⚡ **Binary Data ile Ne Yapılır?**

✅ Şifreli tokenlar  
✅ Dijital imzalar  
✅ Dosya parçaları (resim, ses, video parçası)  
✅ UUID (benzersiz kimlikler)

---

## 💡 **Binary Data Nasıl Kullanılır?**

### 1️⃣ **Binary veri ekleme**

```js
db.dosyalar.insertOne({
  dosyaAdi: "ornek.jpg",
  icerik: new BinData(0, "SGVsbG8gVEtNQVRF")
})
```

---

### 2️⃣ **Binary veri sorgulama**

```js
db.dosyalar.find({ "icerik": new BinData(0, "SGVsbG8gVEtNQVRF") })
```

➡ Belirli ikili veriye sahip belgeleri getirir.

---

## ⚠️ **Dikkat Edilecek Noktalar**

- 🔹 16MB sınırı nedeniyle büyük dosyalar doğrudan belgede tutulmaz.
- 🔹 Büyük dosyalar için **GridFS** kullanılır (Mongo’nun dosya sistemi).
- 🔹 Binary data üzerinde sorgular sınırlıdır → doğrudan içerik araması yapmak pratik değildir.

---

## 🌟 **SQL ile Kıyas**

|Özellik|MongoDB Binary Data|SQL Karşılığı|
|---|---|---|
|Veri türü|BinData|BLOB (Binary Large Object), VARBINARY|
|Maks boyut|Belge başına 16MB|BLOB boyutu veritabanı ayarına bağlı (genelde büyük)|
|Kullanım|Binary içerik doğrudan belgede|Binary içerik doğrudan tabloda|
|Büyük veri|GridFS gerekir|BLOB alanı yeterli olabilir|
|Sorgu|Sınırlı (doğrudan içeriğe göre arama zor)|Sınırlı (genelde içeriğe göre sorgulanmaz)|

---

## 🌟 **Avantajlar**

✅ Binary veriyi doğrudan belgeyle birlikte tutabilirsin → okuma basittir  
✅ Küçük dosya parçaları veya şifreli veri için uygundur  
✅ BSON tip kodları ile veri ne tür binary olduğunu tanımlayabilirsin

---

## 🚩 **Dezavantajlar**

❌ Büyük dosyalar için uygun değildir (16MB sınırı)  
❌ Binary içerik üzerinde arama, filtreleme zordur  
❌ Binary alan belge boyutunu büyütür, performansa olumsuz etki edebilir

---

## 🎯 **Ne zaman kullanılır?**

✅ Küçük boyutlu binary veriler (ör. hash, imza, küçük dosya parçası)  
✅ Binary formatta özel işlenmiş veri saklanacaksa  
✅ Büyük dosya (resim, video, belge) için **GridFS** kullanılmalıdır