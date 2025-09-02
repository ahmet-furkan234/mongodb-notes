
👉 **ObjectId**, MongoDB’de belgeler için benzersiz bir kimlik (ID) üretmek için kullanılan özel bir BSON veri türüdür.  
👉 Varsayılan olarak `_id` alanı için atanır.  
👉 12 byte (96 bit) uzunluğunda bir değerdir ve şu şekilde görünür:

```text
507f1f77bcf86cd799439011
```

---

## 🌟 **ObjectId Yapısı**

ObjectId 12 byte’tan oluşur:

|Parça|Byte|Anlam|
|---|---|---|
|Zaman damgası|4 byte|Oluşturulduğu zamanı saniye cinsinden temsil eder (Unix time)|
|Makine kimliği|5 byte|Makine + süreç ID + counter (benzersizlik için)|
|Sayaç|3 byte|Aynı saniyede üretilen ID'leri ayırmak için artan sayaç|

✅ Böylece aynı saniyede, aynı makinede bile üretilen ObjectId’ler benzersiz olur.

---

## 📝 **Örnek**

```json
{
  "_id": ObjectId("507f1f77bcf86cd799439011"),
  "ad": "Ahmet"
}
```

➡ Burada `_id` alanı ObjectId tipindedir.

---

## ⚡ **Nerede Kullanılır?**

✅ Her belgenin benzersiz kimliği olarak  
✅ Belgeler arası ilişki kurmak için (ör. foreign key yerine)  
✅ Sıralama ve zaman bazlı sorgular için (çünkü zaman damgası içerir)

---

## 🕒 **Zaman Damgası Kullanımı**

ObjectId'den oluşturulma tarihini çıkarabilirsin:

```js
ObjectId("507f1f77bcf86cd799439011").getTimestamp()
```

➡ Belgelerin oluşturulma zamanı doğrudan ObjectId'den okunabilir.

---

## ⚠️ **ObjectId Hakkında Bilinmesi Gerekenler**

- ObjectId benzersizdir, bu yüzden ek bir unique id oluşturmak zorunda kalmazsın.
- Zaman damgası içerdiği için sırayla artan değerler üretir (bu sıralama indekslemeyi hızlandırabilir).
- MongoDB `_id` alanını ObjectId olarak otomatik atar ama istersen kendin farklı bir ID de verebilirsin (ör. string, UUID).

---

## 🌟 **SQL ile Kıyas**

|Özellik|MongoDB ObjectId|SQL ID|
|---|---|---|
|Tip|12 byte binary (genelde hex gösterilir)|INT / BIGINT / UUID|
|Otomatik üretim|✔ Varsayılan|✔ (AUTO_INCREMENT / SERIAL / UUID)|
|Zaman bilgisi|✔ İçinde gömülü|❌ INT/UUID zaman içermez|
|Benzersizlik|✔ Global (zaman, makine, sayaç)|Lokal (tablo içinde unique)|

---

## 🌟 **Avantajları**

✅ Her ObjectId benzersiz → merkezi sistem gerekmez  
✅ Zaman damgası sayesinde sıralama yapılabilir  
✅ Büyük sistemlerde global benzersiz ID üretir (çakışma riski çok düşük)  
✅ JSON ile taşınabilir, görünür formu (hex) nettir

---

## 🚩 **Dezavantajları**

❌ 12 byte → INT’e göre daha büyük yer kaplar (ör. INT 4 byte)  
❌ İnsan tarafından okunması zordur  
❌ Zaman damgası içermesi nedeniyle ID’den veri sızabilir (ör. oluşturma zamanı)

---

## 💡 **Ne zaman ObjectId Kullanılır?**

✅ MongoDB’de varsayılan ID tipi olarak  
✅ Dağıtık sistemlerde global benzersiz ID gerekirken  
✅ Belgelerin yaklaşık oluşturulma zamanını da ID’den öğrenmek istiyorsan

---

## 🎯 **Sonuç**

👉 **ObjectId**, MongoDB’nin güçlü bir benzersiz kimlik mekanizmasıdır ve performans, benzersizlik ve zaman damgası avantajları sunar.