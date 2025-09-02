
NoSQL veritabanları farklı ihtiyaçlara göre tasarlanmış türlere ayrılır.  
Her türün veri modeli ve kullanım alanı farklıdır.

---
### 1️⃣ **Document-Oriented (Belge Tabanlı)**

➡ Veriler **JSON, BSON, XML** gibi belge formatında tutulur.  
➡ Her belge bir nesneyi temsil eder ve kendi içinde alanlar, diziler, iç içe belgeler barındırabilir.  
➡ **Şema zorunlu değil**, her belge farklı yapıda olabilir.

✅ **Örnek Kullanım:** Blog yazıları, ürün katalogları, kullanıcı profilleri  
✅ **Örnek Sistemler:** MongoDB, CouchDB, RavenDB

📝 **Anahtar Cümle:**  
👉 **Veri = Belge (ör. JSON nesnesi)**  
👉 Hızlı okuma/yazma + kolay genişleme

---

### 2️⃣ **Key-Value Store (Anahtar-Değer)**

➡ Veri **anahtar-değer çiftleri** şeklinde tutulur.  
➡ En basit NoSQL türüdür.  
➡ Anahtar ile doğrudan değere erişilir (hash table gibi).

✅ **Örnek Kullanım:** Önbellek (cache), oturum verisi, alışveriş sepeti  
✅ **Örnek Sistemler:** Redis, DynamoDB, Riak

📝 **Anahtar Cümle:**  
👉 **Veri = Anahtar -> Değer**  
👉 Aşırı hızlı okuma ve yazma

---

### 3️⃣ **Wide-Column Store (Kolon Tabanlı / Column Family)**

➡ Veriler satırlar ve kolon aileleri şeklinde tutulur (SQL gibi değil!).  
➡ Her satır farklı kolon setine sahip olabilir.  
➡ Büyük veri kümelerinde ve analizde güçlüdür.

✅ **Örnek Kullanım:** Zaman serisi verileri, IoT verileri, büyük veri analitiği  
✅ **Örnek Sistemler:** Cassandra, HBase, ScyllaDB

📝 **Anahtar Cümle:**  
👉 **Veri = Satır + Kolon Aileleri**  
👉 Yüksek yazma hızı + büyük veri için ölçeklenebilirlik

---

### 4️⃣ **Graph DB (Graf Veritabanı)**

➡ Veriler **düğümler (nodes)** ve **kenarlar (edges)** olarak tutulur.  
➡ İlişkiler doğrudan veri modelinin bir parçasıdır.  
➡ İlişki bazlı sorgularda çok hızlıdır (ör. sosyal ağlar).

✅ **Örnek Kullanım:** Sosyal ağlar, öneri sistemleri, fraud tespiti  
✅ **Örnek Sistemler:** Neo4j, ArangoDB, OrientDB

📝 **Anahtar Cümle:**  
👉 **Veri = Düğümler + İlişkiler**  
👉 Karmaşık ilişki sorgularında mükemmel performans

---

## 📌 **NoSQL Türleri Karşılaştırma Tablosu**

| Tür         | Yapı                 | Avantaj                        | Kullanım Alanı             |
| ----------- | -------------------- | ------------------------------ | -------------------------- |
| Document    | JSON/BSON belge      | Esnek yapı, şema zorunlu değil | CMS, kataloglar, profiller |
| Key-Value   | Anahtar + Değer      | Aşırı hızlı okuma/yazma        | Cache, session, sepet      |
| Wide-Column | Satır + kolon ailesi | Büyük veri + analitik          | IoT, log, analiz           |
| Graph       | Node + Edge          | Karmaşık ilişkilerde hızlı     | Sosyal ağ, fraud, öneri    |

---

## 🎯 **Özet Anahtarlar**

✅ **Document:** JSON belgeler, esnek şema  
✅ **Key-Value:** En hızlı, basit çiftler  
✅ **Wide-Column:** Büyük veri ve analitik  
✅ **Graph:** İlişki odaklı veri