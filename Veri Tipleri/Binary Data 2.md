
👉 **Binary Data (BinData)**, bir alanın değerini _ham ikili veri_ (0 ve 1’lerden oluşan bit dizileri) olarak saklamak için kullanılır.

➡ **Yani: Binary Data, bilgisayarın anlamlandırmadan, doğrudan bit dizisi olarak sakladığı veridir.**  
➡ İçeriği ne olursa olsun, MongoDB onu sadece ham veri olarak görür:

- bir resim dosyası olabilir
- bir ses dosyası olabilir
- bir şifreli anahtar olabilir
- bir dijital imza olabilir
- bir UUID olabilir
- bir uygulamanın özel binary formatı olabilir

---

## 💡 **Binary Data ne saklar?**

✅ **Ham dosya içeriği (ör. JPG, PDF, MP3)**  
✅ **Şifreli metinler, tokenlar**  
✅ **Dijital imzalar (ör. MD5, SHA çıktıları)**  
✅ **UUID değerleri (benzersiz kimlikler)**  
✅ **Herhangi bir uygulamaya özel binary format (ör. sıkıştırılmış veri)**

👉 Mongo için bu verinin anlamı yoktur; içerik senin uygulaman tarafından anlamlandırılır.

---

## 📝 **Örnek**

Şunu saklıyorsun diyelim:

```json
{
  "dosyaAdi": "ornek.jpg",
  "icerik": BinData(0, "SGVsbG8gVEtNQVRF")
}
```

➡ **SGVsbG8gVEtNQVRF** → bu Base64 ile kodlanmış binary veridir.  
➡ Gerçekte bu `"Hello TKMATE"` yazısının binary (bit dizisi) temsili.

---

## ⚠️ **Binary Data’nın kendisi şunu saklamaz:**

❌ Metin (anlamlandırılmış)  
❌ Sayı (anlamlandırılmış)  
❌ JSON yapısı

💡 **Binary Data sadece bit dizisini saklar; içeriğin ne olduğunu Mongo bilmez, sadece tutar.**

---

## 🌟 **Binary Data’yı neden kullanırsın?**

✅ Dosya parçası saklamak için (ör. bir resmin bir parçası)  
✅ Şifreli bilgiyi (ör. token) saklamak için  
✅ Hash değerlerini tutmak için (ör. parola hash’i)  
✅ Uygulamanın özel binary verilerini saklamak için

---

## ⚡ **Binary Data'nın yapısı**

```json
BinData(subtype, base64_encoded_data)
```

- subtype: Verinin türünü belirten bir kod (ör. generic = 0x00, UUID = 0x04)
- base64_encoded_data: Gerçek binary verinin Base64 ile ifade edilmiş hali (görünür hale getiriliyor)

🔑 Örnek Subtype Kullanımı

```json
{
  "token": BinData(0x00, "c29tZSByYW5kb20gYmluYXJ5IGRhdGE=")
}
```

👉 0x00 → generic binary  
👉 `"c29tZSByYW5kb20gYmluYXJ5IGRhdGE="` → `"some random binary data"` metninin base64 karşılığı

---

## 🎯 **Özetle Binary Data:**

➡ **Herhangi bir tür ikili (bit dizisi) veriyi doğrudan belgede saklamaya yarar.**  
➡ Mongo onun anlamını bilmez; sadece tutar ve gerektiğinde sana aynen geri verir.  
➡ Uygulama onu alıp anlamlandırır (ör. resmi açar, şifreyi çözer, sesi oynatır).