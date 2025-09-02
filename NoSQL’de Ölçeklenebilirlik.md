

## 1️⃣ Ölçeklenebilirlik (Scalability) Nedir?

**Ölçeklenebilirlik**, bir sistemin artan yükü karşılayabilme kapasitesidir.  
İki türü vardır:

| Türü                                         | Açıklama                                           | Örnek                                |
| -------------------------------------------- | -------------------------------------------------- | ------------------------------------ |
| **Vertical Scaling (Dikey Ölçeklendirme)**   | Mevcut sunucunun donanımını artırmak (CPU, RAM vs) | Sunucuya daha güçlü işlemci takmak   |
| **Horizontal Scaling (Yatay Ölçeklendirme)** | Sisteme yeni sunucular eklemek, yükü dağıtmak      | Yeni node ekleyerek cluster büyütmek |

---


## 2️⃣ NoSQL ve Ölçeklenebilirlik

- NoSQL sistemleri genellikle **yatay ölçeklenebilir** olacak şekilde tasarlanır.
- Bu sayede büyük veri ve yüksek trafik durumlarında **daha fazla node ekleyerek performans artırılır**.
- Geleneksel ilişkisel veritabanları ise daha çok **dikey ölçeklenmeye** dayanır (donanım yükseltme).

---

## 3️⃣ Konsistensi (Consistency) ve Ölçeklenebilirlik İlişkisi

- Daha fazla node, **veri senkronizasyonu ve tutarlılık** sorunlarını artırır.
- Bu yüzden NoSQL sistemleri, ölçeklendikçe **konsistensi** için farklı modeller sunar:
    - **Strong Consistency (Güçlü Tutarlılık):** Tüm node’lar aynı anda aynı veriyi görür.
    - **Eventual Consistency (Sonunda Tutarlılık):** Veriler eninde sonunda tutarlı olur, kısa süreli farklılıklar olabilir.

---


## 4️⃣ CAP Teoremi ve Ölçeklenebilirlik

- **Partition Tolerance** ve **Horizontal Scaling** birbiriyle yakından bağlantılıdır.
- Ölçeklendikçe, ağ bölünme (partition) ihtimali artar, bu da CAP seçimlerini zorlaştırır.
- Bu yüzden NoSQL sistemleri genelde AP veya CP modellerinden birini tercih eder.

---

## 5️⃣ Örnek

|NoSQL Türü|Ölçeklenebilirlik Türü|Konsistensi Modeli|Açıklama|
|---|---|---|---|
|MongoDB|Yatay|Strong veya Eventual|Config’a göre değişir|
|Cassandra|Yatay|Eventual Consistency|Hız ve erişilebilirlik ön planda|
|Redis (Cluster)|Yatay|Strong Consistency|Hız için genelde CP tercih edilir|