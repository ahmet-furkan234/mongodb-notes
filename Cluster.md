👉 **Cluster**, birden fazla MongoDB sunucusunun bir arada çalıştığı bir yapıdır.  
👉 Amaç:  
✅ Verileri birden fazla makineye yaymak (sharding)  
✅ Yüksek erişilebilirlik sağlamak (replication)  
✅ Büyük veri kümelerini ölçeklenebilir bir şekilde yönetmek

---

## 🔹 **MongoDB Cluster Türleri**

### 1️⃣ **Replica Set Cluster (Yüksek Erişilebilirlik için)**

- Aynı veriyi birden fazla sunucuda tutar (veri kopyaları).
- 1 primary (ana) + N secondary (yedekler)
- Primary’a yazılır, secondary’ler okuyabilir (ve gerektiğinde primary olur).

📌 Avantaj: Primary down olursa secondary devralır → **otomatik failover**

---

### 2️⃣ **Sharded Cluster (Yatay Ölçekleme için)**

- Veriyi parçalara ayırıp farklı sunuculara dağıtır.
- **Shard:** Veri parçası tutan sunucu.
- **Config Server:** Cluster yapı bilgisini tutar.
- **Mongos:** İstemci bağlantısını yöneten aracı (router).

📌 Avantaj: Büyük veri kümeleri çok sayıda sunucuya bölünür → **yük dengesi + performans**

---

## 🎨 **Cluster Mimarisi Görsel Olarak (metin şeması)**



```c
📌 Replica Set
  +------------------------+
  | PRIMARY (yazma + okuma) |
  +------------------------+
         /         \
+----------------+ +----------------+
| SECONDARY 1     | | SECONDARY 2     |
| (sadece okuma)  | | (sadece okuma)  |
+----------------+ +----------------+

Aynı veri hepsinde var.

------------------------------

📌 Sharded Cluster
          +-----------+
          |  mongos   | (router)
          +-----------+
              |
  +-----------------+-----------------+
  |                 |                 |
+------+        +------+          +------+
| shard1|        | shard2|        | shard3|
+------+        +------+          +------+
              (her biri veri parçası tutar)
              
(Config Server: cluster yapı bilgisi barındırır)
```

---

## ⚙ **Cluster Kullanım Senaryosu**

|Cluster Türü|Ne Zaman Kullanılır?|
|---|---|
|**Replica Set**|Yüksek erişilebilirlik isteniyorsa (ör: kritik uygulamalar, kesinti istemiyorsan)|
|**Sharded Cluster**|Çok büyük veri kümelerini ölçeklemek, milyonlarca belgeyi yatay dağıtmak gerekiyorsa|

---