# 2. Coercion

**Coercion**, bir tipten diğerine dönüşüm yapma işlemidir. Bu işlem bazı durumlarda otomatik olarak yapıldığından dolayı beklenmedik sonuçlar doğurabilir.

## 2.1. Implicit ve Explicit Coercion

### Implicit Coercion

Otomatik olarak yapılan dönüşüm işlemidir. Örneğin `1 + "2"` işleminde `+` operatörü `number` ve `string` tiplerini toplamaya çalıştığından dolayı işlemi  `1 + Number("2")` olarak dönüştürür ve hesaplar.

> ### Dikkat Edilmesi Gerekenler
>
> *Implicit coercion* işlemleri kodun okunabilirliğini arttırır. Fakat *number* olması gereken parametrenin *string* dönmesi gibi bazı beklenmedik sonuçlar doğurabilir. Bu gibi durumlarda tiplerin dönüşümlerini bilmek önem kazanır.

### Explicit Coercion

Kullanıcının yaptığı dönüşüm işlemidir. Örneğin `Number("2")` işlemi kullanıcının  string bir değeri sayıya çevirmesidir.

> ### `===` ve Diğer Karşılaştırma Operatörlerinde Coercion
>  
> İki farklı tipi karşılaştırılıyorsa `==`, `<`, `>`, `<=`, `>=` operatörleri implicit *coercion* işlemi gerçekleştirir. Fakat `===` operatöründe tiplerin farklı olması durumunda *coercion* işlemi gerçekleşmez ve `false` döndürür. Bu operatörlerin nasıl kullanılacağı hakkında daha fazla bilgi almak için bir sonraki bölüm olan [Equality](./3_EQUALITY.md) bölümüne bakabilirsiniz.
>
> ### Implicit ve Explicit Coercion Tercihi
>
> *Coercion* işlemlerinde gerekli olmayan bir dönüşümün *implicit* veya *explicit* olması gerektiğinin tercihini yaparken yazılımcının kendisine şu soruyu sorması gerekir: "***Bu dönüşümü açıkça göstermek okuyucuya yardımcı mı olur yoksa okuyucunun dikkatini mi dağıtır?***"

## 2.2. Abstract Operations

Javascript'te bazı işlemler abstract olarak tanımlanmıştır. Bu işlemler `ToPrimitive`, `ToBoolean`, `ToNumber`, `ToString`, `ToObject` ve `ToPropertyKey`'dir.

### ToPrimitive(input, hint)

Bir değeri primitive tipe dönüştürmek için kullanılır. `ToPrimitive` işlemi arkada `valueOf` ve `toString` metotları ile yapılır. Eğer `valueOf` ve `toString` metotları yoksa Javascript `TypeError` hatası verir.

#### Hint: `number`

İlk olarak `valueOf` metodunu çağırır. Sonucunda primitive tipinde bir değer dönmezse `toString` metodunu çağırır. Eğer primitive değer dönerse sonucu `number` tipine dönüştürür.

#### Hint: `string`

İlk olarak `toString` metodunu çağırır. Sonucunda primitive tipinde bir değer dönmezse `valueOf` metodunu çağırır. Eğer primitive değer dönerse sonucu `string` tipine dönüştürür.

### Dönüşüm Tablosu

Tiplerin detaylı dönüşüm tablosu için [tıklayınız](./assets/2_COERCION_TABLE.md).

> ### null ve undefined Neden `0` Döndürür?
>
> `null` ve `undefined` tipleri sayıya dönüştürülürken ilk önce `toString` metodu kullanılacağından dolayı bu değerler boş string'e (`""`) dönüştürülür. Elde edilen boş string sonrasında `number` tipine dönüştürülerek `0` döndürülür.

## 2.3. Boxing

*Primitive* tipler da tıpkı `object` tipi gibi property ve fonksiyonlara sahiptir Fakat obje gibi davranıyor olmaları onları öyle yapmaz. *Primitive* tiplerin de objeler gibi özelliklere sahip olması **boxing** olarak adlandırılır.

## 2.4. İstisnalar

- `+` operatörü **string** değerleri birleştirmek için de kullanıldığından dolayı `"16" + 1` işlemi `161` sonucunu verebilir. Burada dikkat edilmesi gereken boş *string* `""` sayıya dönüştürüldüğünde `0` döndürür.
- `-` operatörü sadece sayılar için kullanıldığından dolayı herhangi bir *string* kullanıldığında sayıya dönüştürülecektir.
- Aynı yerde birden fazla *karşılaştırma operatörü* kullanıldığında kontrol edilecek karşılaştırma soldan sağa doğru gerçekleşir. Örneğin `1 < 2 < 3` işleminde ilk olarak `1 < 2` işlemi `true` döner. Dönen `true` değeri sayıya dönüştürüldüğünde `1 < 3` işlemi elde edilir ve işlemin sonucunda `true` elde edilir.

### [Önceki Sayfa](./1_TYPES.md) | [Sonraki Sayfa](./3_EQUALITY.md) | [Ana Sayfa](./README.md) | [Yukarı Çık](#2-coercion)
