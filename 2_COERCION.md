# 2. Coercion

Coercion, bir tipten diğerine dönüşüm yapma işlemidir. Javascript'te bu işlem otomatik olarak yapıldığından dolayı bazen beklenmedik sonuçlar doğurabilir.

## 2.1 Implicit ve Explicit Coercion (Kapalı ve Açık Dönüşüm)

### Implicit coercion

Javascript'in otomatik olarak yaptığı dönüşüm işlemidir. Örneğin `1 + "2"` işleminde `1` sayısal bir değerken `"2"` string bir değerdir. Javascript bu durumu otomatik olarak `1 + Number("2")`'ye çevirir.

### Explicit coercion

Kullanıcının bilerek yaptığı dönüşüm işlemidir. Örneğin `Number("2")` işlemi kullanıcının bilerek string bir değeri sayıya çevirmesidir.

- İki tarafta farklı tipte değerler varsa `==`, `<`, `>`, `<=`, `>=` operatörleri implicit coercion işlemi gerçekleşir. Fakat `===` operatöründe bu durum gerçekleşmez. Dönüşümlerde sayıya dönüştürme işlemi önceliklidir. Eğer bir tarafta string bir tarafta sayı varsa string sayıya dönüştürülür.

## 2.2 Abstract Operations (Soyut İşlemler)

Javascript'te bazı işlemler soyut olarak tanımlanmıştır. Bu işlemler `ToPrimitive`, `ToBoolean`, `ToNumber`, `ToString`, `ToObject` ve `ToPropertyKey`'dir.

### ToPrimitive(input, hint)

Bir değeri primitive bir değere dönüştürmek için kullanılır. `ToPrimitive` işlemi arkada `valueOf` ve `toString` metodları ile yapılır. Eğer `valueOf` ve `toString` metodları yoksa Javascript `TypeError` hatası verir.

#### Hint: `number`

İlk olarak `valueOf` metodunu çağırır. Eğer primitive bir değer dönerse sonucu verir. Primitive değer dönmezse `toString` metodunu çağırır. Eğer primitive değer dönerse sonucu verir.

#### Hint: `string`

İlk olarak `toString` metodunu çağırır. Eğer primitive bir değer dönerse sonucu verir. Primitive değer dönmezse `valueOf` metodunu çağırır. Eğer primitive değer dönerse sonucu verir.

### Dönüşüm Tablosu

Dönüşüm tablosu için [tıklayınız](2_COERCION_TABLE.md).

- `null` ve `undefined` tipleri sayıya dönüştürülürken ilk önce string'e çevrileceklerinden dolayı boş string'e (`""`) daha sonra da `0`'a dönüşür.

## 2.3 Boxing

Javascript'te her şey obje gibi davranır fakat öyle değildir. İlkel tipler de tıpkı objeler gibi property ve fonksiyonlara sahiptir Fakat bu onları obje yapmaz. Bu durum boxing olarak adlandırılır.

## 2.4 İstisnalar

- `+` operatörü stringleri birleştirmek için de kullanılabildiğinden dolayı `"16" + 1` işlemi `161` sonucunu verebilir. Burada dikkat edilmesi gereken `""` sayıya dönüştürüldüğünde `0` döndürür.
- `-` operatörü sadece sayılar için kullanıldığından dolayı herhangi bir string kullanıldığında sayıya dönüştürülecektir.
- Aynı yerde birden fazla karşılaştırma operatörü kullanıldığında karşılaştırma işlemi soldan sağa doğru gerçekleşir. Örneğin `1 < 2 < 3` işleminde ilk olarak `1 < 2` işlemi `true` döner. `true` sayıya dönüştürülürerek `1 < 3` elde edilir ve sonucunda `true` döner.

## 2.5 Notlar

- Javascript'in implicit coercion işlemleri kodun okunabilirliğini arttırır. Fakat number olması gereken parametrenin string gelmesi gibi bazı beklenmedik sonuçlar doğurabilir. Bu gibi durumlarda tiplerin dönüşümlerini bilmek önem kazanır.
- Coercion işlemlerinde gerekli olmayan bir dönüşümün Implicit veya excplicit olacağının seçiminde yazılımcının kendisine şu soruyu sorması gerekir: "***Bu dönüşmü okuyucuya göstermek ona yardımcı mı olur yoksa dikkatini mi dağıtır?***"
