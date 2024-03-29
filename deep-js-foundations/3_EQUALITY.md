# 3. Equality

Genel kanının aksine `===` operatörü `==` operatörünün eksikliklerini gidermez. Nasıl çalıştıkları bilindiği sürece iki operatör de aynı projede hatta aynı fonksiyonda bile kullanılabilir.

`==` operatörü ve `===` operatörü arasındaki tek fark `==` operatörünün [implicit coercion](/2_COERCION.md/#21-implicit-ve-explicit-coercion) işlemi yapmasıdır. Bu karşılaştırma yapılırken her iki tarafın da aynı tipte olduğu görülür ise operatör `===` gibi davranır. Bu yüzden her iki tarafın aynı tipe ait olduğu biliniyorsa -*ki farklı tipleri karşılaştırırken istisnaları dikkate almak gerekli*- `==` operatörü kullanılabilir.

Karşılaştırılan değerlerin tiplerinin bilinmediği durumlarda `===` operatörü kullanmak daha güvenli bir seçenektir. Fakat bu seçeneğin tercih edilmesi, tercih nedeninin absürtlüğünü de göz önüne serer. Karşılaştırılan değerlerin tiplerinin bilinmiyor olması, *tehlikeli* bir karşılaştırmadan çok daha büyük bir sorundur. *Bir yazılım geliştirici yazdığı koddaki değerlerin tiplerini bilmiyorsa kodu da tam anlamıyla anlamıyor demektir.*

## 3.1. Abstract Equality Comparison (==)

- `==` operatörü farklı tipler arasında karşılaştırma yaparken bir değerin `number` olması durumunda diğer değeri de `number` tipine dönüştürecektir. Fakat değerlerden biri `object` ise bu değer, `ToPrimitive()` fonksiyonuyla karşılaştırma yapılabilecek bir tipe dönüştürülecektir.
- `undefined` ve `null` karşılaştırılırken `==` operatörü kullanılması `===` operatörünün aksine `true` döndürür. Bu durumun sebebi `null` ve `undefined`'ın birbirine eşit fakat farklı tiplerde olmasıdır.

> ### Objeler Eşitliği
>  
> Hiçbir obje birbirine eşit değildir. Sadece iki obje de aynı referansa sahip olursa eşitlik kabul edilir.

### Aşağıdaki durumlarda `==` operatörü kullanmaktan kaçının

- `0`, `""` ve `[]` gibi değerlerin karşılaştırılması

> *Implicit coercion* işlemi yapılacağından dolayı beklenmedik sonuçlar doğurabilir.

- *non-primitive* tiplerin karşılaştırılması

> ### Non-Primitive Tiplerde Karşılaştırma
>  
> *Non-primitive* tiplerde karşılaştırma işlemleri **referansları** aracılığıyla yapılır. İki farklı obje değerler bakımından aynı olsa da farklı referanslara sahip olduklarından dolayı karşılaştırma operatörü iki objeyi farklı görecektir. Bu gibi işlemlerde sağlıklı bir karşılaştırma yapılabilmesi için objeler özgün değerlere sahip olmalı ve bu değerler karşılaştırılmalıdır.

- `x == true` ve `x == false` gibi karşılaştırmalar

> `x`'in `true` veya `false` olup olmadığını kontrol etmek için `x`'in tipinin bilinmesi gerekmektedir. Bu durumda `===` operatörü kullanılmalıdır. Alternatif olarak `ToBoolean()` fonksiyonu da kullanılabilir.

### [Önceki Sayfa](./2_COERCION.md) | [Sonraki Sayfa](./4_SCOPE.md) | [Ana Sayfa](./README.md) | [Yukarı Çık](#3-equality)
