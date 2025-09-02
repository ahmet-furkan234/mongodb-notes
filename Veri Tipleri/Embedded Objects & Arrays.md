
### 📌 **Embedded Objects**

Bir belgenin içinde başka bir belge (alt nesne) tutmaktır.  
➡ Yani iç içe yapı (nested structure).

👉 Örnek:

```json
{
  "ad": "Ahmet",
  "adres": {
    "sehir": "Ankara",
    "postaKodu": "06000"
  }
}
```

---

### 📌 **Arrays**

Bir belgenin içinde liste (dizi) şeklinde veri tutmaktır.  
➡ Aynı anahtar altında çoklu değerler.

👉 Örnek:

```json
{
  "ad": "Ahmet",
  "hobiler": ["yüzme", "satranç", "kodlama"]
}
```

---

## ⚡ **SQL’de Karşılığı Nedir?**

|MongoDB Yapısı|SQL Karşılığı|
|---|---|
|Embedded Object|📌 **JOIN yapılacak ayrı bir tablo** (ör. `Adres` tablosu)|
|Array|📌 **1-N ilişkiyi gösteren ayrı tablo** (ör. `Hobiler` tablosu)|

---

### 📝 **SQL Örneği**

SQL’de bu yapı şöyle olurdu:

```sql
-- Kişi tablosu
id | ad
---|-------
1  | Ahmet

-- Adres tablosu
id | kisi_id | sehir  | posta_kodu
---|---------|--------|-----------
1  | 1       | Ankara | 06000

-- Hobi tablosu
id | kisi_id | hobi
---|---------|-------
1  | 1       | yüzme
2  | 1       | satranç
3  | 1       | kodlama
```


---

## ⚡ **Embedded Object & Array Avantaj ve Dezavantajları**

|Kriter|Embedded Object / Array (MongoDB)|JOIN + Ayrı Tablo (SQL)|
|---|---|---|
|**Okuma Performansı**|✔ Tüm veri tek belgede → hızlı okuma (tek disk erişimi)|❌ JOIN’ler gerektirir, daha yavaş okuma|
|**Yazma / Güncelleme**|✔ Tek belge değişir|❌ Birden çok tabloyu güncellemen gerekir|
|**Tekrarlayan Veri**|❌ Aynı alt belge/dizi tekrar eder (daha fazla disk alanı)|✔ Normalizasyon → veri tekrarını engeller|
|**Esneklik (Schema)**|✔ Esnek (her belge farklı alanlara sahip olabilir)|❌ Sabit şema (her kayıt aynı yapıda)|
|**Veri Bütünlüğü**|❌ Bütünlük kontrolü uygulaması geliştiriciye kalır|✔ Yabancı anahtar, constraint desteği var|
|**Veri Boyutu**|❌ Büyük belgelerde boyut sınırı (ör. 16MB)|✔ Tablo sınırı genelde daha esnektir|
|**Sorgulama Esnekliği**|✔ İç içe yapıya doğrudan erişim kolay|❌ JOIN yazmak gerekir|
|**İlişki Yönetimi**|❌ Çok karmaşık ilişkilere uygun değil|✔ Çok karmaşık ilişkilere uygun|

---

## 🎯 **Avantaj Özet (Embedded / Array için)**

✅ Tek belge ile birden çok veriyi bir arada almak kolay → **daha az sorgu, daha az disk erişimi**  
✅ JOIN maliyeti yok → **yüksek performans**  
✅ Şema zorunluluğu olmadan farklı yapılar tutulabilir

---

## 🚫 **Dezavantaj Özet (Embedded / Array için)**

❌ **Tekrarlayan veri** (ör. aynı şehir adı birden çok belgede tekrar eder)  
❌ **Bütünlük kontrolü geliştiriciye kalır** → ör. silinen bir kişinin alt verilerini elle temizlemen gerekir  
❌ **Belge boyut sınırı var** (16MB sınırı)

---

## 💡 **Ne Zaman Embedded Kullanılır?**

👉 **Veri doğrudan o belgeyle sıkı bağlıysa**

- Ör: Bir siparişin satırları (order + items)
    
- Ör: Bir kişinin adres bilgisi (kişi değişirse adresi de değişir)
    

👉 **Veri bağımsız değilse ayrı koleksiyon daha uygundur**

- Ör: Bir ürünün kategori bilgisi → ayrı koleksiyon + reference
    

---

## 🌟 **SQL ile MongoDB Embedded Karşılaştırma Tablosu**

|Özellik|MongoDB (Embedded / Array)|SQL (JOIN / Ayrı Tablolar)|
|---|---|---|
|Tasarım|Düz belge içinde her şey|Normalizasyon + ilişkili tablolar|
|Okuma|Daha hızlı (tek belge çekilir)|JOIN’li sorgular daha maliyetli|
|Güncelleme|Kolay (tek belge değişir)|JOIN'li verilerde dikkat ister|
|Bütünlük|Zayıf (elle kontrol)|Güçlü (foreign key, constraint)|
|Depolama|Fazla yer kaplayabilir|Daha verimli depolama|
|Karmaşık ilişki|Zor|Çok uygun|
|Esneklik|Çok yüksek|Düşük|

---

## 📌 **Özet**

➡ **MongoDB Embedded / Array** = JOIN'li SQL yapılarına göre okuma ve yazma performansı yüksek ama veri tekrarına ve bütünlük kontrolüne dikkat etmen gerekir.  
➡ **SQL JOIN yapıları** = Daha düzenli ve güvenli ilişkiler ama okuma/yazma karmaşık ve daha maliyetli.