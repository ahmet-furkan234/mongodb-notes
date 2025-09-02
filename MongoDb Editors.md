
MongoDB’de veriyi yönetmek, sorgulamak ve görselleştirmek için CLI dışında birçok araç vardır. Bunlar 2 ana gruptur:

---
### 🔹 1️⃣ **Görsel Arayüz (GUI) Editörler**

Bu araçlar MongoDB veritabanını görsel olarak yönetmeni sağlar.  
Avantajları:  
✅ Sorguları kolayca yazmadan veri görebilirsin.  
✅ Tablo/koleksiyon yapısını ağaç görünümüyle görürsün.  
✅ CRUD işlemlerini tıklayarak yapabilirsin.

#### ⚡ **Popüler MongoDB GUI’leri**

---

✅ **MongoDB Compass (resmi MongoDB aracı)**

- MongoDB’nin kendi GUI aracıdır.
- Verileri grafik olarak gösterir (ör: koleksiyon büyüklüğü, indeksler).
- Sorgu yazmadan veriyi inceleyebilirsin.
- Query Builder ile filtreleme yapabilirsin.

👉 [https://www.mongodb.com/try/download/compass](https://www.mongodb.com/try/download/compass)

---

✅ **NoSQLBooster**

- Hem GUI hem gelişmiş sorgu editörü.
- Otomatik tamamlama, kod parçacıkları (snippets) destekler.
- Export/import özellikleri güçlüdür.

👉 [https://nosqlbooster.com](https://nosqlbooster.com)

---

✅ **Studio 3T**

- Kurumsal özellikler sunar (ör: SQL'den Mongo’ya dönüşüm aracı).
- Visual Query Builder, aggregation pipeline editor içerir.
- Ücretli ama güçlüdür.

👉 [https://studio3t.com](https://studio3t.com)

---

### 🔹 2️⃣ **Kod Editörleri + Eklentiler**

CLI gibi metin temelli çalışırken, kod editörlerinin MongoDB için destek verdiği özelliklerden yararlanabilirsin.

#### ⚡ **Popüler editörler ve eklentileri**

---

✅ **Visual Studio Code + MongoDB for VS Code Extension**

- VSCode içinden doğrudan MongoDB’ye bağlan, sorgu yaz ve çalıştır.
- Sonuçlar JSON olarak görüntülenir.
- Kolay bağlantı yönetimi + connection string desteği.

👉 [VS Code eklenti linki](https://marketplace.visualstudio.com/items?itemName=mongodb.mongodb-vscode)

---

✅ **JetBrains DataGrip**

- MongoDB desteği eklentiyle gelir.
- IntelliSense, query history, görsel veri inceleme sağlar.

👉 [https://www.jetbrains.com/datagrip/](https://www.jetbrains.com/datagrip/)

---

### 🔑 **Hangi Aracı Ne Zaman Kullanmalı?**

|İhtiyaç|Önerilen Araç|
|---|---|
|Hızlı görsel kontrol, veri gözleme|MongoDB Compass|
|Sorgu yazarken destek ve gelişmiş editör|NoSQLBooster, Studio 3T|
|Kodla çalışıp entegre geliştirme|VSCode + MongoDB eklentisi|
|SQL’den Mongo geçiş / kurumsal işler|Studio 3T|

---

## 🎨 **Görsel örnek akış**

💡 **MongoDB Compass ile örnek işlem:**  
1️⃣ Compass’ı aç  
2️⃣ Bağlantı URI’sini gir (ör: `mongodb://localhost:27017`)  
3️⃣ Veritabanlarını ve koleksiyonları gör  
4️⃣ Koleksiyona tıkla → belgeleri gör  
5️⃣ Filtre yaz → veriyi filtrele