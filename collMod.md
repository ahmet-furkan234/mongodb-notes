

`collMod`, bir koleksiyonun ayarlarını **sonradan değiştirmek** için kullanılır.

### ✨ En yaygın kullanım:

- **Validator eklemek/değiştirmek**
- **Doğrulama seviyesi (`validationLevel`) ayarlamak**
- **Doğrulama tepkisi (`validationAction`) değiştirmek**
- **TTL (expireAfterSeconds)** güncellemek
- **Index parametrelerini değiştirmek**

---

## 🧱 Temel Söz Dizimi

```js
db.runCommand({
  collMod: "koleksiyonAdi",
  validator: { ... },
  validationLevel: "strict",
  validationAction: "error"
})
```

---

## 🧪 Basit Örnek – Validator Ekleme

```js
db.runCommand({
  collMod: "kullanicilar",
  validator: {
    $jsonSchema: {
      bsonType: "object",
      required: ["email"],
      properties: {
        email: {
          bsonType: "string",
          pattern: "^.+@.+\\..+$"
        }
      }
    }
  },
  validationLevel: "strict",
  validationAction: "error"
})
```

> Bu komutla artık `kullanicilar` koleksiyonuna eklenecek her belge **doğru formatta e-mail** içermek zorundadır.

---

## 🔧 Diğer Ayarlarla Kullanım

### 🔹 Yalnızca `validator` Güncelle:

```js
db.runCommand({
  collMod: "kitaplar",
  validator: {
    $jsonSchema: {
      bsonType: "object",
      properties: {
        sayfa: { bsonType: "int", minimum: 1 }
      }
    }
  }
})
```

---

### 🔹 `validationLevel` Değiştir:

```js
db.runCommand({
  collMod: "kitaplar",
  validationLevel: "moderate"
})
```

---

### 🔹 `validationAction` Değiştir:

```js
db.runCommand({
  collMod: "kitaplar",
  validationAction: "warn"
})
```

> Kurallara uymayan belgeler yazılır ama MongoDB log’a uyarı yazar.

---

## 📌 Güncel Validator’a Bakmak İçin

Aşağıdaki komut mevcut şema kurallarını gösterir:

```js
db.getCollectionInfos({ name: "kullanicilar" })
```

veya

```js
db.kullanicilar.options()
```

---

## ❌ Hatalı Örnek – Gerekli Alan Eksikse

```js
db.kullanicilar.insertOne({ ad: "Ahmet" })
```

> Eğer validator `email` alanını istiyorsa, bu belge reddedilir.

---

## ⚠️ Uyarı:

- `collMod` ile sadece **mevcut koleksiyonlar** üzerinde işlem yapılır.
- `collMod`, oluşturulmuş koleksiyonlara sonradan **şema kuralları, TTL, indeks davranışları** eklemek için **tek yoldur**.

---

## 🔒 İleri Kullanım – TTL Güncelleme

```js
db.runCommand({
  collMod: "loglar",
  index: {
    keyPattern: { "timestamp": 1 },
    expireAfterSeconds: 3600
  }
})
```

> Bu örnek, `loglar` koleksiyonundaki `timestamp` alanına bağlı olan TTL indeksinin süresini 1 saat olarak günceller.

---

## 🎯 Özet Tablo

|Alan|Açıklama|
|---|---|
|`collMod`|Koleksiyona ait ayarları değiştirmek|
|`validator`|Şema kuralları ekleme veya değiştirme|
|`validationLevel`|Kuralların hangi koşulda çalışacağı|
|`validationAction`|Kural hatasında sistemin vereceği tepki|
|`index`|TTL ve index davranışını güncelleme|
