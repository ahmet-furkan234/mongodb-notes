
|Metakarakter|Açıklama|Örnek|Açıklama|
|---|---|---|---|
|`\w`|Harf (a-z, A-Z), rakam (0-9) ve alt çizgi (_) karakteri|`\w+`|Bir veya daha fazla kelime karakteri (word character)|
|`\W`|`\w` dışındaki karakterler (büyük harf W)|`\W+`|Kelime karakteri olmayanlar|
|`\d`|Rakam (digit) 0-9|`\d{3}`|Üç tane rakam|
|`\D`|`\d` dışındaki karakterler|`\D+`|Rakam olmayan karakterler|
|`\s`|Boşluk karakterleri (space, tab, newline)|`\s+`|Bir veya daha fazla boşluk|
|`\S`|`\s` dışındaki karakterler|`\S+`|Boşluk olmayan karakterler|
|`.`|Yeni satır harici herhangi bir karakter|`a.b`|a ile b arasında herhangi bir karakter|

---

## Detaylı Açıklamalar

### 1. `\w` — Word Character (Kelime Karakteri)

- Harfler: a-z, A-Z
    
- Rakamlar: 0-9
    
- Alt çizgi: `_`
    
- **Türkçe karakterler veya diğer Unicode karakterler dahil değildir!**
    

Örnek: `"Ahmet_123"` içinde tüm karakterler `\w` kapsamındadır.

---

### 2. `\d` — Digit (Rakam)

- Sadece rakamları kapsar: 0,1,2,3,...9
    
- `"1234"` → `\d{4}` eşleşir
    
- `"12a4"` → `\d{4}` eşleşmez çünkü içinde harf var
    

---

### 3. `\s` — Whitespace (Boşluk Karakterleri)

- Space (boşluk)
    
- Tab
    
- Yeni satır (line break)
    
- Form feed gibi görünmez boşluklar
    

---

### 4. `.` — Any Character (Herhangi Karakter)

- Satır sonu hariç herhangi bir karakter ile eşleşir.
    

---

## Örnekler

|Regex|Metin|Açıklama|
|---|---|---|
|`\w+`|`"Ahmet_123"`|`"Ahmet_123"` tamamı eşleşir|
|`\d{2}`|`"45"`|2 rakam eşleşir|
|`\d{3}`|`"1234"`|İlk 3 rakamı eşleştirir|
|`\s+`|`"Merhaba Dünya"`|boşluklar eşleşir|
|`.`|`"a\nb"`|`"a"` ve `"b"` ile eşleşir, `\n` ile eşleşmez|

---

## Özet Tablo

|Metakarakter|Anlamı|Karakterler|
|---|---|---|
|`\w`|Kelime karakteri|a-z, A-Z, 0-9, _ (alt çizgi)|
|`\d`|Rakam|0-9|
|`\s`|Boşluk|Space, tab, newline vs.|
|`.`|Herhangi karakter (newline hariç)|Tüm karakterler (yeni satır hariç)|
|`\W`|Kelime olmayan karakterler|Tüm `\w` dışı karakterler|
|`\D`|Rakam olmayan karakterler|Tüm `\d` dışı karakterler|
|`\S`|Boşluk olmayan karakterler|Tüm `\s` dışı karakterler|

---

İstersen şimdi bu karakter sınıflarını kullanarak örnek regexler yapabiliriz ya da MongoDB’de nasıl kullandığını göstereyim. Nasıl istersen?