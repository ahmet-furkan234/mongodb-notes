
👉 **Symbol**, MongoDB BSON formatında bir veri türüdür (type code: `0x0E`).  
👉 Normal bir string (yazı) gibi görünür ama **sembolik anlamı olan, genelde programlama dillerinde sembol olarak işlenen değerler için** tasarlanmıştır.

➡ Symbol aslında bir _etiket_, _sabit ad_ veya _sözcük_ gibi davranmak için vardır.

✅ JavaScript ve bazı dillerde sembol/sabit adı olarak kullanılabilir.  
✅ MongoDB’de Symbol, String gibi işlenir ama BSON formatında ayrı bir type olarak tanımlanır.

---

## 🌟 **Örnek Belge**

```js
{
  "durum": Symbol("aktif")
}
```

👉 `durum` alanı bir Symbol değeri içeriyor.

---

## ⚡ **Symbol Kullanım Amacı**

- Daha çok eski sürüm MongoDB uygulamalarında **sabit değerler**, **durum etiketleri** için
- Programlama dilleriyle veri uyumluluğu sağlamak (ör. eski JS motorları)

---

## ⚠ **Modern MongoDB’de Durum**

✅ Symbol desteklenir ama artık önerilmez  
✅ Yerine normal **String** kullanılır, aynı işlevi görür  
✅ Symbol ve String sorgu ve index açısından aynı şekilde davranır

---

## 🌟 **Symbol ile String Karşılaştırması**

|Özellik|Symbol|String|
|---|---|---|
|BSON tipi|0x0E|0x02|
|Görünüş|Sembol adı (ör. Symbol("aktif"))|Yazı (ör. "aktif")|
|Kullanım|Programatik sembol|Genel metin|
|Tavsiye|Modern sistemlerde kullanılmaz|Tavsiye edilir|

---

## 🌟 **SQL ile Kıyas**

SQL'de Symbol gibi ayrı bir tip **yoktur**.  
Sabit değerler ve etiketler için **VARCHAR / TEXT / ENUM** kullanılır.

|MongoDB Symbol|SQL Karşılığı|
|---|---|
|Sabit sembol değeri|ENUM, VARCHAR sabit değer|

---

## 🌟 **Avantajları**

✅ Sembol ile programatik anlam içeren değerler saklanabilir (ama bu sadece eski sistemlerde anlamlıdır)

---

## 🚩 **Dezavantajları**

❌ Symbol ile String arasında pratik fark yok → gereksiz karmaşıklık  
❌ Modern uygulamalarda String yeterli ve daha uyumlu  
❌ Symbol kullanımı aggregation, sorgu, index açısından bir avantaj sağlamaz

---

## 🎯 **Modern Tavsiye**

👉 **Symbol kullanma, yerine String kullan.**  
👉 Symbol’ler yalnızca eski sistemlerden veri taşırken karşına çıkabilir.

---

## 💡 **Ek İpucu**

✅ Eğer bir Symbol alan varsa ve String ile sorgu yapmak istersen doğrudan yapabilirsin:

```js
db.koleksiyon.find({ durum: "aktif" })
```

➡ Symbol("aktif") olan alan da eşleşir.