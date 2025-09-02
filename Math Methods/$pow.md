
> `$pow`, verilen bir sayının belirtilen kuvvetini (üsünü) hesaplar.  
> Matematiksel olarak:  
> result=baseexponent\text{result} = \text{base}^{exponent}

📌 Söz dizimi:

```js
{ $pow: [ <base>, <exponent> ] }
```

- `<base>`: Taban sayı (sayısal ifade)
- `<exponent>`: Üs (sayısal ifade)

---

## 📁 Örnek Veri: `numbers`

```js
db.numbers.insertMany([
  { value: 2 },
  { value: 3 },
  { value: 4 },
  { value: 5 }
])
```

---

## 📌 1. Basit üs alma — Kare alma (üs 2)

```js
db.numbers.aggregate([
  {
    $project: {
      value: 1,
      squared: { $pow: ["$value", 2] }
    }
  }
])
```

📤 Çıktı:

```json
{ value: 2, squared: 4 }
{ value: 3, squared: 9 }
{ value: 4, squared: 16 }
{ value: 5, squared: 25 }
```

---

## 📌 2. Küp alma (üs 3)

```js
db.numbers.aggregate([
  {
    $project: {
      value: 1,
      cubed: { $pow: ["$value", 3] }
    }
  }
])
```

---

## 📌 3. Üs olarak ondalıklı sayı kullanma

```js
db.numbers.aggregate([
  {
    $project: {
      value: 1,
      root: { $pow: ["$value", 0.5] }  // Karekök olarak da kullanılabilir
    }
  }
])
```

---

## 📌 4. Sabit taban ile değişken üs

Örneğin 2 üzeri değer hesaplama:

```js
db.numbers.aggregate([
  {
    $project: {
      value: 1,
      powOf2: { $pow: [2, "$value"] }
    }
  }
])
```

---

## 📌 5. Hesaplama sonrası yuvarlama

```js
db.numbers.aggregate([
  {
    $project: {
      value: 1,
      powRounded: {
        $round: [
          { $pow: ["$value", 2.5] },
          2
        ]
      }
    }
  }
])
```

---

## ⚠️ Dikkat Edilecekler

|Durum|Sonuç / Etki|
|---|---|
|Negatif taban, ondalıklı üs|Matematiksel olarak tanımsız olabilir, hata verebilir|
|Üs negatif olabilir|Desteklenir, örn: `2^-1 = 0.5`|
|Çok büyük üs|Performans etkilenebilir|
|`null` veya `string`|Hata verir|

---

## ✅ Özet

|Özellik|Desteklenir|
|---|---|
|Taban üzeri üs alma|✅|
|Ondalıklı üs ile hesaplama|✅|
|Negatif üs|✅|
|Hesap sonrası yuvarlama ile kombinasyon|✅|
|`$project`, `$match`, `$group` içinde kullanılabilir|✅|

---

Başka hangi operatörle veya örnekle devam edelim? Ya da istersen gerçek hayat problemlerinde nasıl kullanılır göstereyim.