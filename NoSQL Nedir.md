
**NoSQL (Not Only SQL)**, ilişkisel olmayan (non-relational) bir veritabanı türüdür. Geleneksel SQL tabanlı sistemlerin (ör. SQL Server, MySQL, PostgreSQL) esnek olmayan şema yapısına karşı, **esnek veri modelleri** ve **yüksek ölçeklenebilirlik** sunar.

➡ NoSQL, büyük miktarda veriyi hızlıca depolamak ve işlemek için tasarlanmıştır.  
➡ Genellikle **dağıtık sistemler** ve **bulut tabanlı uygulamalarda** tercih edilir.

---

### 🔹 **NoSQL’in Ortaya Çıkış Nedeni**

İlişkisel veritabanları (SQL) 1970'lerden beri kullanılıyor. Ancak 2000'li yıllarda:  
✅ Web ve mobil uygulamalarda veri miktarı **devasa boyutlara** ulaştı.  
✅ Veriler daha **karmaşık ve yapısız** hale geldi (ör. JSON, XML, multimedia).  
✅ Uygulamalar **yüksek hızda ve esnek** veri depolama ihtiyacı duydu.

➡ Bu yüzden NoSQL sistemleri geliştirildi.

---

### 🔹 **NoSQL’in Temel Özellikleri**

| Özellik                      | Açıklama                                                           |
| ---------------------------- | ------------------------------------------------------------------ |
| **Esnek Şema**               | Önceden tablo ve kolon tanımlamadan veri ekleyebilirsiniz.         |
| **Yüksek Ölçeklenebilirlik** | Yatayda (sunucu ekleyerek) kolayca büyüyebilir.                    |
| **Büyük Veri Desteği**       | Yüksek hacimli ve hızlı akan veriler için uygundur.                |
| **Dağıtık Mimari**           | Veriler birden çok sunucuya yayılabilir (sharding).                |
| **Çeşitli Veri Modelleri**   | Document, Key-Value, Column, Graph gibi farklı modeller destekler. |

---

### 🔹 **NoSQL Türleri**

| Tür                   | Açıklama                                 | Örnek            |
| --------------------- | ---------------------------------------- | ---------------- |
| **Document Store**    | JSON benzeri belgeleri depolar.          | MongoDB, CouchDB |
| **Key-Value Store**   | Anahtar-değer çiftleri.                  | Redis, DynamoDB  |
| **Wide Column Store** | Kolon ailesi temelli yapı.               | Cassandra, HBase |
| **Graph DB**          | Düğümler ve kenarlarla ilişkileri tutar. | Neo4j, ArangoDB  |

---

### 🔹 **NoSQL vs SQL Karşılaştırması**

|Özellik|SQL (Relational)|NoSQL (Non-Relational)|
|---|---|---|
|**Şema**|Katı şema|Esnek şema|
|**İlişkiler**|Güçlü ilişkiler (JOIN)|İlişkiler zayıf veya yok|
|**Sorgulama**|SQL dili|API ve özel sorgu dilleri|
|**Ölçeklenme**|Dikey (daha güçlü sunucu)|Yatay (sunucu ekleyerek)|
|**Uygulama Tipi**|Kurumsal uygulamalar|Büyük veri, gerçek zamanlı uygulamalar|

---

### 🔹 **NoSQL Nerede Kullanılır?**

✅ Büyük veri uygulamaları (Big Data)  
✅ Gerçek zamanlı analitik sistemler  
✅ IoT ve sensör verileri  
✅ İçerik yönetim sistemleri  
✅ Sosyal ağlar (ör. mesajlar, beğeniler)

---