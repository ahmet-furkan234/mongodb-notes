
➡ CAP Teoremi, 2000 yılında Eric Brewer tarafından ortaya atılmıştır.  
➡ **Dağıtık sistemlerde (ör. NoSQL) bir anda 3 temel özellikten en fazla 2’si sağlanabilir.**

---

### 🔑 **CAP’in Açılımı**

| Harf  | Anlamı                                  | Açıklama                                                                                      |
| ----- | --------------------------------------- | --------------------------------------------------------------------------------------------- |
| **C** | Consistency (Tutarlılık)                | Tüm nodelar aynı anda aynı veriyi görür. Bir işlem yapıldıysa herkes sonucu aynı anda görür.  |
| **A** | Availability (Erişilebilirlik)          | Her isteğe bir yanıt alınır (başarılı ya da hata bile olsa). Sistem asla tamamen susmaz.      |
| **P** | Partition Tolerance (Bölünme Toleransı) | Sistem node'lar arası bağlantı kopsa bile çalışmaya devam eder. Ağaç düşse bile ayakta kalır. |

---

## 📝 **CAP’in Ana Felsefesi**

🎯 Dağıtık bir sistemde **ağda problem (partition) varsa**, aynı anda:  
✅ Tutarlılığı %100  
✅ Erişilebilirliği %100  
**sağlayamazsın!**

➡ Ya **Consistency**'yi seçeceksin (doğru veri, bazen yanıt gelmez)  
➡ Ya **Availability**'yi seçeceksin (yanıt ver ama belki eski veri)

---

## ❗ **Neden Üçü Birden Olamaz?**

Bir örnekle tam oturtalım:

---

### 📌 Senaryo: 2 node’lu bir NoSQL veritabanı (ör. MongoDB cluster)

✅ Node A İstanbul'da  
✅ Node B Ankara’da

---

➡ Kullanıcı bir veri yazdı (ör. `balance = 500`)

➡ Tam o anda A ile B arasındaki ağ koptu (**Partition**)

---

Şimdi sistem karar vermek zorunda:

| Seçenek                         | Ne olur?                                                                                                                      |
| ------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| **Consistency'yi seç (C + P)**  | A, isteği alır ama B’ye iletemediği için kullanıcıya yanıt dönmez ya da hata verir. (Yanıt gecikir, ama tutarlılık bozulmaz.) |
| **Availability'yi seç (A + P)** | A isteği alır, yanıt döner. B bağlantı koptuğu için eski veriyi dönebilir. (Hızlı yanıt verir ama tutarlılık bozulabilir.)    |

---

💡 **Üçünü birden sağlayamama nedeni:**  
Çünkü ağ kopması (partition) olduğunda:  
👉 Hem doğru (consistent)  
👉 Hem hızlı (available)  
👉 Hem bağlantısız ortamda (partition tolerant)  
aynı anda mümkün değil.  
Birini feda etmek zorundasın!

---

## ⚡ **CAP Modelleri**

| Model  | Sağlanan                           | Feda Edilen         | Örnek Sistemler                                                                                |
| ------ | ---------------------------------- | ------------------- | ---------------------------------------------------------------------------------------------- |
| **CP** | Consistency + Partition Tolerance  | Availability        | HBase, MongoDB (strong consistency config), Redis Sentinel                                     |
| **AP** | Availability + Partition Tolerance | Consistency         | Cassandra, CouchDB, DynamoDB                                                                   |
| **CA** | Consistency + Availability         | Partition Tolerance | Tek node sistemlerde mümkün, ama gerçek dağıtık sistemlerde partition her zaman risk (teorik). |

---

## 🔑 **CA Gerçekte Yok!**

🎯 Dağıtık sistemde **partition olmaması imkansıza yakındır** (ağ her zaman kopabilir, gecikebilir).  
O yüzden gerçek hayatta **CA (hem tutarlı hem erişilebilir hem de bağlantı hep var)** olmaz.  
💡 **Her dağıtık sistem mutlaka P’yi hesaba katar.**

---

## 🌟 **Kısaca Özet**

✅ **Partition varsa → CAP 3’ü bir arada olmaz**  
✅ Gerçekte hep P vardır → Seçim C mi A mı olacak?  
✅ NoSQL sistemler bu tercihe göre tasarlanır:

- **CP sistemler:** Doğruluk daha önemli (ör. finans, kritik veriler)
- **AP sistemler:** Hız ve erişim daha önemli (ör. sosyal ağlar, cache)

---

## 💡 **Zihinde oturtacak anahtar cümle**

👉 **Dağıtık sistem = Partition riski = İkisini seç, birini bırak!**