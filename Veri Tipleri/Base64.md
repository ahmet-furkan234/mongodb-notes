
👉 **Base64**, ikili (binary) veriyi (0 ve 1’lerden oluşan bit dizilerini) metin formatında (ASCII) temsil etmeye yarayan bir kodlama yöntemidir.

➡ Amaç:  
✅ Binary veriyi metin haline getirerek taşımak (ör. JSON, XML, e-posta gibi sadece metin kabul eden ortamlarda)  
✅ Binary içeriği bozulmadan iletmek

➡ Base64, her 3 byte (24 bit) binary veriyi 4 karakterlik ASCII dizesine dönüştürür.

---

# ⚡ **Nasıl Çalışır?**

### 🔹 Adım 1: Binary veriyi 6 bit’lik parçalara ayır

Her karakter Base64 tablosundan bir simgeye çevrilir.

```json
ör. "M"  → 01001101
```

3 byte = 24 bit → 24 bit / 6 = 4 Base64 karakteri

---

### 🔹 Adım 2: Base64 tablosundan karakter eşleştir

Base64 tablosu 64 farklı simge içerir:

```json
A-Z (26)
a-z (26)
0-9 (10)
+ /
```