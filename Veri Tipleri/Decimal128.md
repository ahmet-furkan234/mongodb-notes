
👉 **Decimal128**, MongoDB’de yüksek hassasiyetli ondalıklı sayılar için kullanılan 128 bitlik bir sayısal veri türüdür (BSON type code: `0x13`).  
👉 IEEE 754-2008 standardına uygun **34 basamak hassasiyetinde** ondalıklı sayıları tutar.  
👉 Finans, muhasebe ve hassas hesaplamalar için idealdir.

---

## 🌟 **Neden gerekli?**

✅ JavaScript’in `Number` (double) türü yaklaşık 15-17 basamak hassasiyet sağlar ve yuvarlama hatalarına açıktır.  
✅ Decimal128 ise 34 basamak hassasiyetle tam sonucu korur.  
✅ Parasal işlemler, kur hesapları, faiz hesapları gibi alanlarda kullanılır.

---

## 🌟 **Decimal128 Aralığı**

➡ 10^−6143 ile 10^+6144 arasında değer tutabilir (çok geniş aralık)

---

## 📝 **Örnek Belge**

```json
{
  "_id": 1,
  "bakiye": NumberDecimal("12345.678901234567890123456789012345")
}
```

👉 Bu değer tam 34 basamak hassasiyetle saklanır.

---

## ⚡ **Decimal128 Kullanımı**

### Değer atama

```js
db.hesaplar.insertOne({
  ad: "Ahmet",
  bakiye: NumberDecimal("1000.75")
})
```

### Sorgu

```js
db.hesaplar.find({ bakiye: NumberDecimal("1000.75") })
```

---

## 🌟 **SQL ile Kıyas**

|MongoDB Decimal128|SQL Karşılığı|
|---|---|
|128 bit decimal (34 basamak)|DECIMAL, NUMERIC|
|Yüksek hassasiyet|Yüksek hassasiyet|
|Parasal işlemler için ideal|Parasal işlemler için ideal|

➡ SQL örneği:

```sql
CREATE TABLE hesaplar (
  bakiye DECIMAL(34, 10)
);
```

---

## 🌟 **Avantajları**

✅ 34 basamak hassasiyet → parasal ve bilimsel hesaplamalarda doğru sonuç  
✅ Yuvarlama hatası yok (double gibi değil)  
✅ Parasal işlemler için aggregation ve hesaplamalarda güvenli

---

## 🚩 **Dezavantajları**

❌ Double’a göre daha fazla yer kaplar (16 byte)  
❌ Hesaplama işlemleri (ör. toplama, çarpma) daha maliyetli (performans açısından)  
❌ Driver ve uygulama dilinde desteklenmesine dikkat gerekir (bazı client’larda ek iş gerekebilir)

---

## 🌟 **Decimal128 vs Double**

|Özellik|Decimal128|Double|
|---|---|---|
|Hassasiyet|34 basamak|~15-17 basamak|
|Amaç|Parasal ve hassas hesaplama|Genel amaçlı sayılar|
|Hata|Yuvarlama hatası yok|Yuvarlama hatası olabilir|
|Boyut|16 byte|8 byte|

---

## 🎯 **Ne zaman kullanılır?**

✅ Parasal işlemler (ör. bakiye, kur, faiz, muhasebe)  
✅ Çok hassas bilimsel hesaplamalar  
✅ Yuvarlama hatasının kabul edilemeyeceği tüm işlemler

---

💬 **İstersen:**

- Aggregation örneklerinde Decimal128 kullanımı yazayım
    
- Double ve Decimal128 karşılaştırmalı örnekler vereyim (ör. yuvarlama hatası göstereyim)
    
- Decimal128 alanlarda index performans ipuçlarını anlatayım
    

👉 Ne dersin TkMatE? 🚀