
MongoDB’de `db.createCollection(koleksiyonAdı, options)` ile kullanılabilen **opsiyonlar** şunlardır:

| Opsiyon                        | Tip       | Açıklama                                                                                            | Örnek                                               |
| ------------------------------ | --------- | --------------------------------------------------------------------------------------------------- | --------------------------------------------------- |
| `capped`                       | `Boolean` | Koleksiyon sabit boyutlu (capped) olsun mu? `true` ise yeni kayıtlar gelince eski kayıtlar silinir. | `capped: true`                                      |
| `size`                         | `Number`  | Capped koleksiyon için maksimum boyut (byte cinsinden)                                              | `size: 1048576` (1 MB)                              |
| `max`                          | `Number`  | Capped koleksiyon için maksimum doküman sayısı                                                      | `max: 1000`                                         |
| `validator`                    | `Object`  | Koleksiyon için şema doğrulama kuralları tanımlanır                                                 | `{ $jsonSchema: { ... } }`                          |
| `validationLevel`              | `String`  | Validation kontrol düzeyi: `strict`, `moderate`, `off`                                              | `validationLevel: "strict"`                         |
| `validationAction`             | `String`  | Validation hatasında ne yapılacak: `error` (engelle), `warn` (uyar ama ekle)                        | `validationAction: "error"`                         |
| `autoIndexId`                  | `Boolean` | (Eski sürümlerde) `_id` için otomatik index açılsın mı? (>=3.2 sürümünde kaldırıldı)                | `autoIndexId: true`                                 |
| `storageEngine`                | `Object`  | Koleksiyonun belirli bir storage engine ayarı varsa belirtmek için                                  | `{ wiredTiger: { ... } }`                           |
| `indexOptionDefaults`          | `Object`  | Koleksiyon için oluşturulacak indexlerin varsayılan ayarları                                        | `{ storageEngine: { ... } }`                        |
| `viewOn`                       | `String`  | Bir view (görünüm) oluşturuyorsan, hangi koleksiyonun görünümü olduğunu belirtir                    | `viewOn: "orders"`                                  |
| `pipeline`                     | `Array`   | View oluştururken kullanılacak aggregation pipeline                                                 | `pipeline: [{ $match: { aktif: true } }]`           |
| `collation`                    | `Object`  | Koleksiyon için varsayılan collation (karakter karşılaştırma, sıralama kuralı)                      | `{ locale: "tr", strength: 2 }`                     |
| `timeseries`                   | `Object`  | Bir time series collection oluşturmak için (MongoDB 5.0+)                                           | `{ timeField: "createdAt", metaField: "sensorId" }` |
| `expireAfterSeconds`           | `Number`  | Time series koleksiyon için TTL süresi                                                              | `expireAfterSeconds: 3600`                          |
| `encryptedFields`              | `Object`  | Field level encryption için yapı (MongoDB Enterprise / Atlas)                                       | `{ fields: [...] }`                                 |
| `changeStreamPreAndPostImages` | `Object`  | Change stream için eski ve yeni doküman görüntüsünü tut                                             | `{ enabled: true }`                                 |

---

## 🎯 **En sık kullanılanlar**

✅ `capped`, `size`, `max` → Capped collection ayarları  
✅ `validator`, `validationLevel`, `validationAction` → Şema doğrulama  
✅ `viewOn`, `pipeline` → View oluşturma  
✅ `timeseries`, `expireAfterSeconds` → Time series collection  
✅ `collation` → Dil ve sıralama kuralları  
✅ `changeStreamPreAndPostImages` → Change stream gelişmiş loglama

---

## ⚡ **Tam bir createCollection örneği (hemen hemen tüm yaygın opsiyonları içeren)**

```js
db.createCollection("sensordata", {
  capped: true,
  size: 10485760, // 10 MB
  max: 5000,
  validator: {
    $jsonSchema: {
      bsonType: "object",
      required: ["sensorId", "value"],
      properties: {
        sensorId: { bsonType: "string" },
        value: { bsonType: "double" }
      }
    }
  },
  validationLevel: "strict",
  validationAction: "error",
  collation: { locale: "tr", strength: 2 },
  timeseries: {
    timeField: "createdAt",
    metaField: "sensorId"
  },
  expireAfterSeconds: 3600,
  changeStreamPreAndPostImages: { enabled: true }
})
```

💡 **Bu örnek:**  
✅ 10 MB sabit boyutlu capped collection  
✅ Şema doğrulama var  
✅ Türkçe sıralama kuralları var  
✅ Time series + TTL ile veri 1 saat tutuluyor  
✅ Change stream ile veri değişimleri izlenebiliyor

---

## ⚠ **Dikkat Edilecekler**

- **`capped` true ise sonradan normal koleksiyona dönüştürülemez.**
- **`timeseries` ile `capped` birlikte kullanılmaz.**
- **Validation kuralları ile eklenen dokümanları sınırlandırırsın; ama `validationAction: "warn"` yaparsan kısıtlamayı zorunlu hale getirmez.**
- **`viewOn` ve `pipeline`, sadece view için kullanılır; normal koleksiyonda olmaz.**
