
### 🔹 1. **Query Düzeyi (doğrudan alan üzerinden)**

```js
{ CITY: { $in: ["Ankara", "İzmir", "İstanbul"] } }
```

> Bu yazım **sadece doğrudan sorgu (query)** içinde kullanılır — yani `find`, `$match` gibi yerlerde geçerlidir.

---

### 🔹 2. **Aggregation Düzeyi (alan değeri operatörün içinde)**

```js
{ $in: ["$CITY", ["Ankara", "İzmir", "İstanbul"]] }
```

> Bu yazım ise **aggregation işlemleri içinde**, yani `$project`, `$cond`, `$switch` gibi operatörlerde kullanılır.  
> `$in` burada bir ifade (expression) olarak çalışır.

---

## 🔍 Kıyaslama Tablosu

|Özellik|`CITY: { $in: [...] }`|`$in: ["$CITY", [...]]`|
|---|---|---|
|Nerede kullanılır?|`find`, `$match` gibi filtrelerde|`$project`, `$cond`, `$expr`, `$switch` içinde|
|Alan yazımı|Sol tarafta|Dizi içindeki ilk parametre|
|Fonksiyonel ifade mi?|❌ Hayır|✅ Evet|
|Değer sabit mi?|Evet|Hayır – başka alanlar da kullanılabilir|
|Nested kullanım?|Uygun değil|Uygun|

---

## 🔧 Örnekle Gösterelim:

### A. `CITY` alanı "Ankara", "İzmir", "İstanbul" olanları getirmek (query)

```js
db.getCollection("USERS").find({
  CITY: { $in: ["Ankara", "İzmir", "İstanbul"] }
})
```

---

### B. `CITY` alanına göre etiket oluşturmak (`$project` içinde)

```js
db.getCollection("USERS").aggregate([
  {
    $project: {
      name: 1,
      cityType: {
        $cond: {
          if: { $in: ["$CITY", ["Ankara", "İzmir", "İstanbul"]] },
          then: "Büyükşehir",
          else: "Diğer"
        }
      }
    }
  }
])
```

---

## 🎯 Sonuç:

|Yazım|Kullanım Alanı|Fonksiyon Gibi Mi?|
|---|---|---|
|`{ CITY: { $in: [...] } }`|`find`, `$match`|❌|
|`{ $in: ["$CITY", [...]] }`|`$project`, `$cond`, `$expr`, `$switch`|✅|
