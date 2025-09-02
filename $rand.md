
0 ile 1 arasında **rastgele bir ondalıklı sayı** üretir (her çağrıldığında farklı).

---

## 🧪 Basit Kullanım

```js
db.USERS.aggregate([
  {
    $project: {
      NAME: 1,
      randomScore: { $rand: {} }
    }
  }
])
```

### 🧾 Açıklama:

Her kullanıcıya `randomScore` adında bir alan ekler, bu alanda 0–1 arasında rastgele bir sayı olur.

---

## 🎯 Senaryo 1 — Rastgele Kullanıcı Seçimi

```js
db.USERS.aggregate([
  {
    $project: {
      NAME: 1,
      randomValue: { $rand: {} }
    }
  },
  { $sort: { randomValue: 1 } },
  { $limit: 1 }
])
```

### 📌 Açıklama:

Tüm kullanıcılar arasında rastgele 1 tanesini getirir. Her çalıştırıldığında farklı kullanıcı döner.

---

## 🎯 Senaryo 2 — 0.5’ten büyük olanları getir (şansa dayalı filtreleme)

```js
db.USERS.aggregate([
  {
    $project: {
      NAME: 1,
      randVal: { $rand: {} }
    }
  },
  {
    $match: {
      randVal: { $gt: 0.5 }
    }
  }
])
```

### 📌 Açıklama:

Yaklaşık yarısı filtrelenir — her çalıştırmada farklı sonuçlar oluşur.

---

## 🎯 Senaryo 3 — 1–100 Arası Tam Sayı Üret

MongoDB doğrudan `$rand` ile tam sayı vermez ama şöyle yapılabilir:

```js
db.USERS.aggregate([
  {
    $project: {
      NAME: 1,
      randomInt_1to100: {
        $toInt: { $multiply: [{ $rand: {} }, 100] }
      }
    }
  }
])
```

---

## 📚 Ek Not:

- `$rand` **her belge için yeniden çalışır**, yani her belgeye özel rastgele sayı üretir.
    
- `$sample` operatörüyle karıştırılmamalıdır. `$sample` tüm koleksiyondan rastgele seçim yapar ama `$rand` belge içi işlemlerde kullanılır.
    

---

Sıradaki operatörün `$indexOfArray`, devam edelim mi? Yoksa yukarıdaki örneklerde daha derine inmek ister misin?