
- **Sharding**, MongoDB’de **verilerin yatay olarak (horizontal) bölünerek farklı sunuculara dağıtılmasıdır.**
- Bu sayede çok büyük veri kümeleri, performans ve kapasite sorunları olmadan yönetilebilir.
- Kısaca, **veri parçalanarak farklı sunuculara (shard’lara) dağıtılır.**

---

## Neden Sharding?

|Durum|Açıklama|
|---|---|
|Çok büyük veri setleri|Tek bir sunucu kapasitesi yetersiz kalır|
|Yüksek trafik|Yük tek bir sunucuya binmez, dağıtılır|
|Performans sorunları|Veri parçalanarak sorgular hızlı hale gelir|

---

## Sharding Bileşenleri

|Bileşen|Görev|
|---|---|
|**Shard**|Verinin parçalarının saklandığı sunucu/grup|
|**Config Server**|Sharding ile ilgili metadata (yapı bilgisi) depolar|
|**Query Router (mongos)**|Uygulamadan gelen sorguları shard’lara yönlendirir|

---

## Nasıl Çalışır?

1. Veriler, belirlenen bir **shard key**’e göre parçalara bölünür.
2. Her parça (chunk), farklı shard’lara dağıtılır.
3. Sorgular, mongos tarafından ilgili shard’lara yönlendirilir.
4. Parçalanmış veri sayesinde paralel işlem yapılır, performans artar.

---

## Örnek:

- Diyelim kullanıcı verilerini saklıyorsun ve **userId** alanı shard key.
- UserId 1-1000 arası veriler Shard 1’de, 1001-2000 arası Shard 2’de tutulur.
- Böylece sorgular sadece ilgili shard’a gider ve hızlı sonuç döner.

---

## Özet

|Avantaj|Açıklama|
|---|---|
|Yatay Ölçeklenebilirlik|Yeni shard ekleyerek kapasite artırılır|
|Yük Dengeleme|Sorgular shard’lar arasında dengelenir|
|Büyük Veri Yönetimi|Çok büyük veri kümeleri kolayca yönetilir|