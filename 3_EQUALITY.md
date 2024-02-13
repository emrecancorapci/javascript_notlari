# 3 Equality

- `==` operatörü ve `===` operatörü arasındaki tek fark `==` operatörünün implicit coercion işlemi yapmasıdır. Bu karşılaştırma yapılırken her iki taraf da aynı tipte ise `===` gibi davranır. Bu yüzden her iki tarafın aynı tipe ait olduğu biliniyorsa -*ki farklı tipleri karşılaştırırken istisnaları dikkate almak gerekli*- `==` operatörü kullanılabilir. Buradan hareketle her iki tarafın tiplerinin bilinmediği durumlarda `===` operatörü kullanarak karşılaştırma yapmak daha güvenli bir seçenek olacaktır.

## 3.1 Abstract Equality Comparison (==)

- `==` operatörü farklı tipler arasında karşılaştırma yaparken bir değerin `number` olması durumunda diğer değeri de `number` tipine dönüştürecektir. Fakat değerlerden biri `object` ise obje, `ToPrimitive()` fonksiyonuyla dönüştürülecektir.
- `undefined` ve `null`'ı karşılaştırırken `==` operatörü kullanıldığında `===` operatörünün aksine `true` döner. Bu durumun sebebi `null` ve `undefined`'ın birbirine eşit fakat farklı tiplerde olmasıdır.
- Hiçbir obje birbirine eşit değildir. Sadece iki obje de aynı referansa sahip olursa eşit kabul edilir. Bu gibi işlemlerde objelere ait özgün bir değer bulundurulmalı ve bu değerler karşılaştırılmalıdır.
