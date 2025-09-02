
MongoDB’de bir koleksiyon içindeki dokümanların sayısını öğrenmek için kullanılan methodlar vardır.  
Bunlar, veritabanındaki verilerin büyüklüğünü anlamak, istatistik almak veya şartlara uyan kaç kayıt olduğunu bulmak için kullanılır.

---

## 🔑 **Başlıca Counting Yöntemleri**

|Method|Açıklama|
|---|---|
|`countDocuments(filter, options)`|Belirli bir filtreye uyan doküman sayısını verir.|
|`estimatedDocumentCount()`|Koleksiyondaki toplam doküman sayısını hızlıca tahmin eder (filtre uygulanamaz).|
|`count()`|**Eski yöntem** (deprecated / önerilmez), yerine `countDocuments` ve `estimatedDocumentCount` kullanılır.|

---

## 1️⃣ **countDocuments()**

🔹 Bir koleksiyonda **filtreye uyan kaç doküman olduğunu tam olarak sayar**.  
🔹 Yavaş olabilir (özellikle büyük koleksiyonlarda), çünkü tarama yapar.

### 📌 Kullanım:

```js
db.users.countDocuments({ yas: { $gte: 18 } })
```

➡ 18 yaş ve üstü kullanıcıların sayısını döner.

### Örnek:

```js
db.orders.countDocuments({ durum: "tamamlandı" });
```

➡ Durumu “tamamlandı” olan siparişlerin sayısını verir.

### Opsiyonlar:

```js
db.users.countDocuments(
  { yas: { $gte: 18 } },
  { limit: 100 }
)
```

➡ 18 yaş ve üstü ilk 100 kullanıcıyı sayar.

---

## 2️⃣ **estimatedDocumentCount()**

🔹 Koleksiyondaki **tahmini toplam doküman sayısını hızlıca döner**.  
🔹 Filtre uygulanamaz.  
🔹 Koleksiyonun metadata’sından okur, çok hızlıdır.

### 📌 Kullanım:

```js
db.users.estimatedDocumentCount()
```

➡ `users` koleksiyonundaki toplam doküman sayısını verir.

---

## ⚠️ **count() (Eski Yöntem)**

`count()` eskiden yaygın kullanılırdı ama artık `countDocuments` ve `estimatedDocumentCount` ile değiştirilmiştir.  
Çünkü `count()` bazı durumlarda doğru olmayan sayılar verebilir ve performans sorunlarına yol açabilir.

### Örnek (kullanılması önerilmez):

```js
db.users.count({ yas: { $gte: 18 } })
```

---

## 🚀 **Karşılaştırma Tablosu**

|Yöntem|Filtre desteği|Hız|Kullanım amacı|
|---|---|---|---|
|`countDocuments`|✅|Daha yavaş|Tam doğru sonuç için|
|`estimatedDocumentCount`|❌|Çok hızlı|Hızlı toplam sayım (ör. istatistik için)|
|`count`|✅|Orta|❌ Önerilmez (deprecated)|

---

## 💡 **Gerçek bir örnek senaryosu**

```js
// Koleksiyonda kaç aktif kullanıcı var?
db.users.countDocuments({ aktif: true })

// Koleksiyondaki toplam kullanıcı sayısını hızlıca öğren
db.users.estimatedDocumentCount()
```

---

## ✨ **Sonuç**

✅ **`countDocuments`** → Doğru ve güvenilir sayım yapar, filtre uygulanabilir.  
✅ **`estimatedDocumentCount`** → Çok hızlıdır, ama tahmini toplam sayıyı verir, filtre uygulanamaz.  
❌ **`count`** → Artık kullanılmamalı.

