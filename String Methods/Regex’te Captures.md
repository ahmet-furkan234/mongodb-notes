

Regex desenlerinde parantez `( )` kullanıldığında, o parantez içindeki eşleşen alt bölümler **grup** olarak yakalanır ve ayrıca ayrı ayrı tutulur. Bu gruplar, genellikle metnin belirli parçalarını çıkarmak için kullanılır.

---

## Örnek

Regex: `(abc)(123)`

Metin: `"abc123def"`

- Tüm eşleşen: `"abc123"`
- İlk grup `(abc)`: `"abc"`
- İkinci grup `(123)`: `"123"`

---

## MongoDB’de `$regexFind` sonucu örneği

```json
{
  "match": "abc123",
  "idx": 0,
  "captures": ["abc", "123"]
}
```

- `"match"`: Tüm eşleşen ifade
- `"idx"`: Eşleşmenin başladığı pozisyon
- `"captures"`: Parantez içindeki her bir yakalanan grup (alt eşleşmeler)

---

## Neden Önemli?

- Örneğin, bir telefon numarasından sadece alan kodunu çıkarmak istiyorsan, grup yakalamalar çok işe yarar.
- Parçaları ayrı ayrı alıp işlem yapabilirsin.

---

## Özet

|Terim|Açıklama|
|---|---|
|Capture|Regex parantez içindeki grup yakalaması|
|captures|Yakalanan grup sonuçlarının dizisi|
|match|Tüm eşleşen string|
