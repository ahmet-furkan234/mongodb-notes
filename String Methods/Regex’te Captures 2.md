
Regex içinde parantez `( )` koyduğunda, oradaki **eşleşen kısmı ayrı ayrı yakalıyorsun**. Yani:

- Parantez dışındaki tüm eşleşme → **match**
- Parantez içindeki her bölüm → **capture grup**

---

## Basit Örnek 1: Tek Grup Yakalama

Regex: `(abc)`

Metin: `"xxabcxx"`

- **match**: `"abc"`
- **captures**: `["abc"]`

---

## Basit Örnek 2: Birden Fazla Grup

Regex: `(a)(b)(c)`

Metin: `"abc"`

- **match**: `"abc"`
- **captures**: `["a", "b", "c"]`

---

## Örnek 3: Sayı ve Harf Grubu

Regex: `(\d+)([a-z]+)`

Metin: `"123abc"`

- **match**: `"123abc"`
- **captures**: `["123", "abc"]`

---

## Örnek 4: Tarih Parçalama

Regex: `(\d{4})-(\d{2})-(\d{2})`

Metin: `"2025-07-15"`

- **match**: `"2025-07-15"`
- **captures**: `["2025", "07", "15"]`

---

## Örnek 5: Telefon Numarası Parçalama

Regex: `(\+\d{2})-(\d{3})-(\d{3})-(\d{4})`

Metin: `"+90-532-123-4567"`

- **match**: `"+90-532-123-4567"`
- **captures**: `["+90", "532", "123", "4567"]`

---

## MongoDB `$regexFind` Örneği

```js
{
  $regexFind: {
    input: "+90-532-123-4567",
    regex: "(\\+\\d{2})-(\\d{3})-(\\d{3})-(\\d{4})"
  }
}
```

Sonuç:

```json
{
  "match": "+90-532-123-4567",
  "idx": 0,
  "captures": ["+90", "532", "123", "4567"]
}
```

---

## Nasıl Kullanılır?

Mesela sen:

- `"532"` kısmını almak istiyorsan, **captures[1]** (0’dan başlıyor)
- Alan kodunu almak istiyorsan **captures[0]** kullanırsın.

---

## Mantık

|Regex Parantezi|JSON'daki `captures` dizisindeki indeks|
|---|---|
|İlk parantez|captures[0]|
|İkinci parantez|captures[1]|
|...|...|

---

### Özet

|Terim|Ne Anlama Gelir|
|---|---|
|`match`|Regex’in tamamı ile eşleşen bölüm|
|`captures`|Parantez içine alınan alt gruplar|
