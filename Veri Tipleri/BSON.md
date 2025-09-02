
**BSON (Binary JSON)**, MongoDB’nin veri saklama ve iletim formatıdır.  
➡ _Açılımı:_ **Binary Serialized Object Notation**  
➡ _Amacı:_ JSON’un esnekliğini korurken, hızlı ayrıştırılabilen ve çeşitli veri tiplerini destekleyen bir yapı sağlamaktır.

---

## 🌟 **BSON’un Temel Özellikleri**

- 🟢 **Binary (ikili)** formatta tutulur → bilgisayarların hızlı okuyup yazmasına uygundur.
- 🟢 **JSON’a benzer yapıdadır** ama JSON’dan daha zengin veri tiplerine sahiptir (ör. `ObjectId`, `Date`, `Binary`).
- 🟢 **Uzunluk bilgisi içerir** → bir belge okunduğunda boyut bilgisi baştan bilinir, hızlı ayrıştırılır.
- 🟢 **Kolay taşınabilir ve ağ üzerinden hızlı iletilebilir.**
- 🟢 **Veri tiplerini açıkça belirler**, örneğin bir sayının `int32` mi yoksa `double` mı olduğunu bilir.

---

## 🧩 **BSON’un Bileşenleri**

Bir BSON belgesi şunlardan oluşur:

|Alan|Açıklama|
|---|---|
|📏 **Toplam uzunluk**|Belgenin toplam byte cinsinden uzunluğu (4 byte).|
|🔑 **Anahtar-değer çiftleri**|Her alanın veri tipi, anahtar adı ve değeri.|
|🚩 **Sonlandırıcı**|`\x00` byte ile biten bir son işaretleyici.|

---

## 💡 **BSON Veri Türleri (Derinlemesine)**

| BSON Türü  | Kodu (hex) | Açıklama                       |
| ---------- | ---------- | ------------------------------ |
| Double     | `0x01`     | 64-bit IEEE 754 ondalıklı sayı |
| String     | `0x02`     | UTF-8 metin                    |
| Object     | `0x03`     | İç içe belge                   |
| Array      | `0x04`     | Sıralı değerler listesi        |
| Binary     | `0x05`     | Ham binary veri                |
| Undefined  | `0x06`     | (kullanılmıyor)                |
| ObjectId   | `0x07`     | 12 byte’lık benzersiz ID       |
| Boolean    | `0x08`     | true/false                     |
| DateTime   | `0x09`     | 64-bit tarih (epoch ms)        |
| Null       | `0x0A`     | Null değer                     |
| Regex      | `0x0B`     | Regular expression             |
| DBPointer  | `0x0C`     | (artık kullanılmıyor)          |
| JavaScript | `0x0D`     | JS kodu                        |
| Symbol     | `0x0E`     | (kullanılmıyor)                |
| JS + Scope | `0x0F`     | Kod + değişken alanı           |
| Int32      | `0x10`     | 32-bit tamsayı                 |
| Timestamp  | `0x11`     | Olay sıralama için             |
| Int64      | `0x12`     | 64-bit tamsayı                 |
| Decimal128 | `0x13`     | Yüksek hassasiyetli sayı       |
| MinKey     | `0xFF`     | En küçük değer                 |
| MaxKey     | `0x7F`     | En büyük değer                 |

---

## 📦 **BSON Veri Örneği (Görselleştirilmiş)**

```json
{
  "_id": ObjectId("60c5ba7f7f1f4d23e4d8b12c"),
  "ad": "Ahmet",
  "yas": 30,
  "aktif": true,
  "dogumTarihi": ISODate("1995-01-01T00:00:00Z"),
  "adres": {
    "sehir": "Ankara",
    "postaKodu": "06000"
  },
  "hobiler": ["yüzme", "satranç"]
}
```

➡ **BSON'da bu şöyle saklanır:**

```json
<belge uzunluğu>
0x07 "_id" <12 byte ObjectId>
0x02 "ad" <uzunluk + "Ahmet">
0x10 "yas" <4 byte int32: 30>
0x08 "aktif" <1 byte boolean: 1>
0x09 "dogumTarihi" <8 byte epoch ms>
0x03 "adres" <embedded document>
0x04 "hobiler" <array>
\x00 (belge sonu)
```

---

## ⚡ **BSON’un JSON’a Göre Avantajları**

|Özellik|BSON|JSON|
|---|---|---|
|📌 **Veri Tipi Desteği**|Çok zengin (ObjectId, Date, Binary, Decimal128, vb.)|Sadece string, number, boolean, null, array, object|
|📌 **Binary Format**|Evet, binary format → hızlı ayrıştırma|Hayır, düz metin|
|📌 **Uzunluk Bilgisi**|Var (başta toplam uzunluk)|Yok|
|📌 **Veri boyutu**|Genelde JSON’dan biraz daha büyük (tip bilgisi + uzunluk eklenir)|Daha küçük (metin formatı)|
|📌 **Hızlı ayrıştırma**|Yüksek performans|JSON’a göre daha yavaş (metin ayrıştırma gerekir)|

---

## 🚀 **JSON ve BSON Arasındaki Fark Örneği**

🔹 **JSON (metin formatı)**

```json
{
  "ad": "Ali",
  "yas": 25
}
```

Sadece `"ad"` ve `"yas"` için değerler var, tipi belirtilmez. Tarayıcı bunu ayrıştırıp anlamaya çalışır.

---

🔹 **BSON (binary format)**

```json
<belge uzunluğu>
0x02 "ad" <uzunluk> "Ali"
0x10 "yas" <4 byte int32: 25>
\x00
```

Tip, uzunluk ve değer açıkça bellidir. **MongoDB sorgu motoru doğrudan byte’ları okur.**

---

## ✅ **BSON Nerede Öne Çıkar?**

- **Performans kritik sistemler**
- **Büyük veri setleriyle çalışırken**
- **MongoDB gibi NoSQL sistemlerinde hızlı sorgu ve indeksleme için**

---

## 📌 **Sonuç**

|Kriter|BSON|JSON|
|---|---|---|
|Format|Binary|Metin (string)|
|Tip bilgisi içerir mi?|✔ Evet|❌ Hayır|
|Performans|✔ Daha hızlı ayrıştırma|⏳ Daha yavaş|
|Veri Tipleri|✔ Çok zengin|❌ Kısıtlı|
|Dosya boyutu|Biraz daha büyük|Daha küçük|