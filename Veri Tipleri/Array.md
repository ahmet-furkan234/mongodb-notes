
## 📌 **Tanım**

MongoDB'de **Array**, bir alanın birden fazla değeri bir arada tutmasına olanak sağlar.  
➡ Bir belge içinde liste şeklinde verileri depolamak için kullanılır.  
➡ Array içeriği; string, sayı, obje, hatta başka array’ler olabilir.

---

## 🌟 **Özellikler**

✅ Array boyutu esnek → her belgede array boyu farklı olabilir.  
✅ Array elemanları farklı türlerden olabilir (ama genelde aynı tip tercih edilir).  
✅ Array’ler üzerinde sorgu ve index oluşturulabilir.

---

## 📝 **Örnek Belge**

```json
{
  "ad": "Ahmet",
  "hobiler": ["yüzme", "satranç", "kodlama"]
}
```

➡ `hobiler` alanı bir array’dir.

---

## ⚡ **Array Üzerinde Yapılabilecek İşlemler**

### 1️⃣ **Array içinde bir değer arama**

```js
db.kisiler.find({ "hobiler": "satranç" })
```

➡ `hobiler` array'inde "satranç" olan belgeleri getirir.

---

### 2️⃣ **Array içeriğinin belirli boyutta olması**

```js
db.kisiler.find({ "hobiler": { $size: 3 } })
```

➡ 3 elemanlı array’e sahip belgeleri getirir.

---

### 3️⃣ **Array içindeki belirli pozisyona erişim**

```js
db.kisiler.find({ "hobiler.0": "yüzme" })
```

➡ İlk hobi "yüzme" olan belgeleri getirir.

---

### 4️⃣ **Array ile index**

```js
db.kisiler.createIndex({ "hobiler": 1 })
```

➡ Array elemanları üzerinde arama hızlanır.

---

## ⚠️ **Dikkat Edilecek Noktalar**

- 🔹 Array çok büyükse belge boyutu hızla büyüyebilir (16MB sınırına dikkat!).
- 🔹 Çok karmaşık iç içe array’ler sorguları zorlaştırabilir.
- 🔹 Array eleman tipleri karışık olursa yönetim ve sorgu karmaşıklaşır.

---

## 🌟 **SQL ile Kıyas**

|Özellik|MongoDB Array|SQL Karşılığı|
|---|---|---|
|Çoklu değer|✔ Aynı belgede liste olarak tutulur|❌ Ayrı tablo + foreign key|
|Okuma|✔ Tek belgeyle gelir|❌ JOIN gerekir|
|Yazma|✔ Tek belge güncellenir|❌ Ana tablo + ilişki tablosu güncellenir|
|Veri bütünlüğü|❌ Zayıf (elle kontrol gerekir)|✔ Foreign key desteği|
|Sorgu|✔ Array içinde doğrudan arama|❌ JOIN ile arama|
|Performans|✔ Hızlı (tek belge erişimi)|❌ JOIN nedeniyle yavaş olabilir|
### 📝 **SQL Karşılığı**

SQL’de array gibi veri tutmak için genelde **1-N ilişkili tablo kullanılır.**

```sql
-- Ana tablo
id | ad
---|------
1  | Ahmet

-- Hobi tablosu
id | kisi_id | hobi
---|---------|-------
1  | 1       | yüzme
2  | 1       | satranç
3  | 1       | kodlama

```

➡ SQL'de ayrı tablo + JOIN gerekir.

---

## 🌟 **Avantaj (Mongo Array)**

✅ JOIN yok → tek belgeyle veri gelir → hızlı okuma  
✅ Dinamik yapı → her belge farklı sayıda array elemanına sahip olabilir  
✅ İç içe sorgularda esnek yapı

---

## 🚩 **Dezavantaj (Mongo Array)**

❌ Çok büyük array’ler belge boyutunu büyütür  
❌ Array elemanlarının tek tek güncellenmesi zor olabilir  
❌ Veri bütünlüğü geliştirici sorumluluğundadır