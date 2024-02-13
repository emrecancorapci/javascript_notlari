# 1. Tipler

- C++, C# ve Java gibi dillerde değişkenlerin tipleri vardır. Fakat javascript'te değerlerin tipleri vardır.
- Temel (İlkel - Primitive) tipler: `undefined`, `string`, `number`, `boolean`, `symbol`, `null`, `bigint`
- Object tipleri: `object`, `function (callable objects)`, `array`

## 1.1 Hoisting

Javascript'te tanımlamalar en yukarıya çekilir. Bu sayede bir değişkenin tanımlanmadan önce kullanılması durumunda hata alınmaz. Fakat bu durum `let` ve `const` ile tanımlanan değişkenler için geçerli değildir. Bu durumda `Temporal Dead Zone` hatası alınır.

## 1.2 Emptiness

Javascript'te 3 tip boşluk vardır. Bunlar `uninitialized`, `undefined` ve `undeclared`'dır. Bu boşluklar farklı anlamlara gelmektedir.

### Uninitialized (TDZ) (Temporal Dead Zone)

Hiç initialize(başlatılmamış) edilmemiş değişken. const ve let ile oluşturulan değişkenler, tanımlanmadan önce kullanılmaya çalışıldığında `Temporal dead zone (TDZ)` hatası verir. Javascript hoisting yaptığı için  değişken oluşturulmuş fakat değişkenin kullanıldığı yerde henüz initilaze edilmemiştir


TDZ hatası verecek kod örneği:

```javascript
console.log(user);

...

const user = "e"
```

### Undefined

Değişken initialize edilmiş fakat bir değişkene bir değer tanımlanmamıştır. Aralarında en yaygın olanıdır.

### Undeclared

Değişken hiç tanımlanmamıştır. Bu durumda kod çalıştırıldığında `ReferenceError` hatası alınır. ESLint gibi statik analiz araçları bu durumu kontrol edebilir.

## 1.3 NaN (Not a Number)

`NaN` bir number tipidir. `NaN` bir matematiksel işlem sonucu oluşan bir hata durumudur. IEEE 754 standardına göre `NaN`'ın kendisi de dahil herhangi bir değer ile yapılan bir işlemin sonucu `NaN` döner.

- `string` değeri `Number(value)` fonksiyonu ile çevrilmeye çalışıldığında `NaN` döner. Bu işlemlerde `Number.isNan(value)` fonksiyonu kullanılmalıdır.
- `NaN` yerine 0 kullanılamaz.

## 1.4 Negative Zero `-0` ve Pozitive Zero `0`

Javascript'te negatif sıfır ve pozitif sıfır değerleri bulunur. İki farklı sıfır değerinin bulunmasının nedeni IEEE 754 standardında belirtilmiş olmasından kaynaklanır. Negatif sıfır spesifikasyonlara daha sonra dahil edildiğinden dolayı bu değerler `===` operatörü ile karşılaştırıldığında aralarındaki fark anlaşılamayack ve `true` dönecektir. Fakat `Object.is(value)` fonksiyonu sonradan spesifikasyonlara eklendiği için bu değerleri birbirinden ayrıt edebilir.

## Notlar

- `undefined` tipi değerin tanımsız değil **şu anda** tanımsız olduğunu gösterir.
- `typeof` operatörü değişkenin tipini string olarak döndürür.
- `null` tipi `object` tipi değerdir. ES1'de yapılan tanımlama yüzünden bu şekilde kalmıştır. Bunu bir bug olarak düşünebiliriz.
- Arrayler de birer objedir. `typeof` ile kontrol edilemeyeceğinden dolayı bir değerin array olup olmadığı `Array.isArray(array)` fonksiyonu ile öğrenilir.
