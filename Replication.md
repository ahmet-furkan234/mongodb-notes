
Replication, MongoDB’de **verilerin birden fazla sunucuya (node) kopyalanması işlemidir.**  
Bu sayede:

- **Yüksek erişilebilirlik (high availability)** sağlanır.
- Bir sunucu arızalandığında (down olduğunda), diğer sunucular veri kaybı olmadan hizmet vermeye devam eder.
- Veri kaybı riski azaltılır.

---

## Temel Kavramlar

|Terim|Açıklama|
|---|---|
|**Primary Node**|Yazma işlemlerinin yapıldığı ana sunucu|
|**Secondary Node**|Primary’den veriyi kopyalayan ve sadece okuma yapabilen sunucular|
|**Replica Set**|Birbirine bağlı Primary ve Secondary node’lardan oluşan grup|

---

## Nasıl Çalışır?

1. **Primary Node**, uygulamadan gelen yazma işlemlerini alır.
2. Bu değişiklikler (oplog - operation log) **Secondary Node’lara aktarılır.**
3. Secondary Node’lar bu değişiklikleri alıp veritabanını günceller.
4. Eğer Primary down olursa, Replica Set içindeki diğer node’lar yeni Primary seçmek için **election** yapar ve sistem kesintisiz devam eder.

---

## Neden Önemlidir?

- **Kesintisiz hizmet:** Sunuculardan biri arızalansa bile sistem çalışmaya devam eder.
- **Yük dengeleme:** Okuma işlemleri Secondary node’lara da yönlendirilebilir.
- **Veri güvenliği:** Veri kaybı riski azalır.

---

## Özet

|Özellik|Açıklama|
|---|---|
|Yüksek erişilebilirlik|Primary down olsa bile hizmet devam eder|
|Veri çoğaltma|Veriler birden fazla sunucuya kopyalanır|
|Failover mekanizması|Otomatik Primary seçimi|