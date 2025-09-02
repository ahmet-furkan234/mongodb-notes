
> **Tanım:**  
> `$ifNull`, bir alanın değeri `null` **veya yoksa**, yedek (varsayılan) bir değer döndürür.

### 📌 Söz Dizimi:

```js
{ $ifNull: [ <alan>, <null ise kullanılacak değer> ] }
```

---

## ✅ Temel Örnek

### 🧪 Örnek 1: `EMAIL` alanı yoksa `"E-posta girilmemiş"` yaz

```js
{
  $project: {
    name: 1,
    emailSafe: { $ifNull: ["$EMAIL", "E-posta girilmemiş"] }
  }
}
```

---

## 🔍 `$ifNull` Ne Zaman Kullanılır?

|Durum|Çözüm|
|---|---|
|Alan eksikse|Alternatif göster|
|Alan `null` ise|Varsayılan değer ver|
|Gösterilecek alanın olup olmadığını bilmediğinde|Güvenli görüntüleme sağla|

---

## 🛠️ Gerçek Hayat Senaryoları (Etrade için)

### 🧪 Örnek 2: `ORDERS.STATUS` alanı null ise "Bekliyor" yaz

```js
db.getCollection("ORDERS").aggregate([
  {
    $project: {
      ORDERID: 1,
      STATUS: { $ifNull: ["$STATUS", "Bekliyor"] }
    }
  }
])
```

---

### 🧪 Örnek 3: Kullanıcının `PHONE` alanı eksikse `"Belirtilmedi"` yaz

```js
db.getCollection("USERS").aggregate([
  {
    $project: {
      NAME: 1,
      TELNR: { $ifNull: ["$PHONE", "Belirtilmedi"] }
    }
  }
])
```

---

### 🧪 Örnek 4: Teslim tarihi (`DELIVEREDDATE`) olmayanlara `"Teslim edilmedi"` yaz

```js
db.getCollection("INVOICEDETAILS").aggregate([
  {
    $project: {
      ORDERID: 1,
      teslimDurumu: { $ifNull: ["$DELIVEREDDATE", "Teslim edilmedi"] }
    }
  }
])
```

---

## 🧠 Alternatif: `$coalesce` gibi çalışır mı?

> Evet! `$ifNull` sadece **ilk değer null** ise çalışır ama birden fazla alanı test etmek istersen:

```js
{ $ifNull: ["$email", "$backupEmail"] }
```

Ama daha da güçlü bir yöntem:

```js
{ $ifNull: [{ $ifNull: ["$email", "$backupEmail"] }, "Hiçbiri yok"] }
```

> ❗ Not: `$ifNull` sadece **birinci değerin null** olmasına bakar, bu yüzden zincirleme yapılabilir.

---

## 🔬 Bonus: `$ifNull` + `$toDate`

Veri dönüşümünde hata almamak için ideal:

```js
{
  $project: {
    dogumTarihi: {
      $toDate: { $ifNull: ["$BIRTHDATE", "01.01.2000"] }
    }
  }
}
```

---

## ✅ Ne Yapalım Şimdi?

Sana uygun mini görevler:

|Görev|Açıklama|
|---|---|
|Kullanıcının eksik `PHONE` alanını doldur|"Belirtilmedi" olarak göster|
|`STATUS` null olan siparişleri "Hazırlanıyor" yap|`$ifNull` kullan|
|`DELIVEREDDATE` null olanları "Bekliyor" olarak etiketle|`$ifNull` ile tarih kontrolü|

İstersen bir tanesini birlikte yazalım ya da `$ifNull` içeren ileri seviye örnek yapmak istersen `$toDate` ile birleştirebiliriz. Hangisini istersin?