
`options`, koleksiyonun nasıl oluşturulacağını belirleyen isteğe bağlı ayarlardır.

## 🧩 Genel Söz Dizimi

```js
db.createCollection("koleksiyonAdi", {
  capped: <boolean>,
  size: <number>,
  max: <number>,
  validator: <object>,
  validationLevel: <"off" | "moderate" | "strict">,
  validationAction: <"warn" | "error">,
  autoIndexId: <boolean>,        // deprecated
  storageEngine: <object>,
  indexOptionDefaults: <object>,
  collation: <object>,
  timeseries: <object>,
  expireAfterSeconds: <number>
})
```

---

## 🔹 1. `capped: true | false`

Sabit boyutlu koleksiyon oluşturur (FIFO mantığı). Eski kayıtlar otomatik silinir.

```js
capped: true
```

---

## 🔹 2. `size: <byte>`

Capped koleksiyon için **maksimum boyut (byte cinsinden)**.

```js
size: 1048576   // 1 MB
```

> `capped: true` ile birlikte kullanılmalı.

---

## 🔹 3. `max: <sayı>`

Capped koleksiyon için maksimum belge sayısı.

```js
max: 5000
```

---

## 🔹 4. `validator: { $jsonSchema: {...} }`

Belge yapısını denetler. MongoDB 3.6+ ile geldi.

### Örnek:

```js
validator: {
  $jsonSchema: {
    bsonType: "object",
    required: ["ad", "yas"],
    properties: {
      ad: { bsonType: "string" },
      yas: { bsonType: "int", minimum: 18 }
    }
  }
}
```

---

## 🔹 5. `validationLevel: "off" | "moderate" | "strict"`

- `"off"` → Doğrulama kapalı
- `"moderate"` → Sadece yeni/updated belgeleri denetler
- `"strict"` → Tüm belgeleri denetler (varsayılan)

```js
validationLevel: "strict"
```

---

## 🔹 6. `validationAction: "warn" | "error"`

- `"error"` → Kurala uymayan belgeyi reddeder
- `"warn"` → Belgeler yazılır ama MongoDB uyarı verir

```js
validationAction: "error"
```

---

## 🔹 7. `storageEngine: {}`

Belirli storage engine’lere özgü yapılandırma. Genelde WiredTiger için özelleştirme yapılır.

```js
storageEngine: {
  wiredTiger: {
    configString: "block_compressor=zlib"
  }
}
```

> **İleri seviye** kullanım, çoğu projede gerekmez.

---

## 🔹 8. `indexOptionDefaults: { storageEngine: {...} }`

Otomatik oluşturulacak indekslerin storage ayarları.

```js
indexOptionDefaults: {
  storageEngine: {
    wiredTiger: {
      configString: "block_compressor=zlib"
    }
  }
}
```

---

## 🔹 9. `collation: {...}`

**Büyük/küçük harf duyarlılığı, dil kuralları** gibi metin karşılaştırmalarını belirler.

### Örnek: Türkçe duyarlı sıralama

```js
collation: {
  locale: "tr",
  strength: 1
}
```

| Seviye | Karşılaştırma Türü                |
| ------ | --------------------------------- |
| 1      | A ≠ a, İ ≠ i (en genel)           |
| 2      | A = a, ama aksanlı karakter farkı |
| 3      | A = a, İ = i (daha hassas)        |

---

## 🔹 10. `timeseries: {}`

MongoDB 5.0+ sürümünde zaman serisi veriler için özel olarak oluşturulan koleksiyon tipi.

```js
timeseries: {
  timeField: "timestamp",
  metaField: "sensorId",
  granularity: "minutes"
}
```

> Bu yapı IoT, loglama, vs. gibi zaman-temelli veriler için tasarlanmıştır.

---

## 🔹 11. `expireAfterSeconds: <saniye>`

Time-series koleksiyonlarında belgelerin ne kadar sonra otomatik silineceğini belirler.

```js
expireAfterSeconds: 86400  // 1 gün sonra belgeleri sil
```

> Sadece time-series ile birlikte kullanılabilir.

---

## 🔺 `autoIndexId` → Artık kullanılmıyor (deprecated)

Eskiden `_id` için otomatik indeksleme ayarıydı. MongoDB 4.0’dan itibaren varsayılan davranış haline geldi.

---

## 🔚 Tam Örnek: Karma Yapı

```js
db.createCollection("uyeler", {
  capped: false,
  validator: {
    $jsonSchema: {
      bsonType: "object",
      required: ["ad", "email"],
      properties: {
        ad: { bsonType: "string" },
        email: { bsonType: "string", pattern: "^.+@.+\\..+$" },
        yas: { bsonType: "int", minimum: 18 }
      }
    }
  },
  validationLevel: "strict",
  validationAction: "error",
  collation: {
    locale: "tr",
    strength: 1
  }
})
```

---

## 🎯 Özet Tablo

|Alan|Açıklama|
|---|---|
|`capped`|Sabit boyutlu koleksiyon (FIFO)|
|`size`|Byte cinsinden maksimum boyut (capped için)|
|`max`|Maksimum belge sayısı (capped için)|
|`validator`|Belge doğrulama kuralları (JSON Schema ile)|
|`validationLevel`|Doğrulama kapsamı: `off`, `moderate`, `strict`|
|`validationAction`|Uygunsuz veri için: `warn` veya `error`|
|`collation`|Dil ve karşılaştırma kuralları|
|`storageEngine`|Fiziksel motor ayarları (ileri düzey)|
|`timeseries`|Zaman serisi koleksiyonu için ayarlar|
|`expireAfterSeconds`|Time-series koleksiyonlarda belge süresi (otomatik silme)|

