
👉 **Timestamp (BSON type code: `0x11`)**, MongoDB'de genelde iç sistem işlemleri için kullanılan özel bir veri türüdür.  
👉 64 bitlik bir sayıdır ve iki parçadan oluşur:

```
[ yüksek 32 bit: saniye cinsinden Unix zamanı | düşük 32 bit: artan sayaç ]
```

➡ Yani Timestamp:  
✅ **Unix epoch zamanı (saniye cinsinden)**  
✅ **Aynı saniye içindeki olayları ayırmak için sayaç**

---

## 🌟 **Neden kullanılır?**

✅ MongoDB replikasyonunda (oplog) değişikliklerin sıralı işlenmesi için  
✅ Sıralı işlem ID'leri oluşturmak için  
✅ Zaman damgası ve aynı saniyedeki olay ayırıcı olarak

---

## 📝 **Örnek Belge**

```json
{
  "olusturmaZamani": Timestamp(1625088000, 1)
}
```

➡ `1625088000` = Unix zamanı (ör. 2021-07-01 00:00:00 UTC)  
➡ `1` = O saniyedeki ilk işlem

---

## ⚡ **Nasıl atanır?**

Genelde **MongoDB kendi üretir** (ör. `oplog`, sistem alanları).  
Manuel atama mümkündür ama nadir kullanılır:

```js
db.loglar.insertOne({
  islem: "kayit",
  zaman: Timestamp( Math.floor(Date.now() / 1000), 1 )
})
```

➡ Burada `Date.now()` ile epoch saniyesi hesaplanır.

---

## 🌟 **SQL ile Kıyas**

|MongoDB Timestamp|SQL Karşılığı|
|---|---|
|Saniye + sayaç (64 bit)|TIMESTAMP + sıra kolonu|
|Zaman ve olay sırası birlikte|TIMESTAMP + SEQUENCE / AUTO_INCREMENT|

➡ SQL’de benzeri:

```sql
CREATE TABLE loglar (
  islem VARCHAR(50),
  zaman TIMESTAMP,
  sira INT
);
```

➡ MongoDB Timestamp → SQL’de ayrı iki sütun ile temsil edilir.

---

## 🌟 **Avantajları**

✅ Zaman ve sıralamayı tek değerle tutar  
✅ Replikasyon ve sistem loglarında güvenilir sıralama sağlar  
✅ Çoklu işlem yapılan sistemlerde aynı saniyedeki olayları ayırt eder

---

## 🚩 **Dezavantajları**

❌ Uygulama seviyesinde Timestamp kullanımı nadirdir (genelde sistem içindir)  
❌ Tarih sorgularında Date türü kadar esnek değildir  
❌ Aggregation ve tarih hesaplamalarında Date fonksiyonlarıyla uyumsuz olabilir

---

## ⚡ **Timestamp ve Date farkı**

|Özellik|Timestamp|Date|
|---|---|---|
|Amaç|Sistem sıralama + zaman damgası|Genel tarih/saat bilgisi|
|Format|Saniye + sayaç (64 bit)|Epoch milliseconds (64 bit)|
|Kullanım|Oplog, replikasyon|Her türlü tarih, saat|

---

## 🎯 **Ne zaman kullanılır?**

✅ Replikasyon ve sistem iç loglar (MongoDB kendisi üretir)  
✅ Özel sistemler arası değişiklik sıralama gerekiyorsa (çok özel kullanım)
