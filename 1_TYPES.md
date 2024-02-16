# 1. Tipler

- C++, C# ve Java gibi dillerde değişkenlerin tipleri vardır. Fakat javascript'te değerlerin tipleri vardır.
- Primitive (İlkel - Temel) Types: `undefined`, `string`, `number`, `boolean`, `symbol`, `null`, `bigint`
- Object Types: `object`, `function (callable objects)`, `array`

## 1.1 Hoisting

Javascript'te tanımlamalar en yukarıya çekilir. Bu sayede bir değişkenin tanımlanmadan önce kullanılması durumunda hata alınmaz. Fakat bu durum `let` ve `const` ile tanımlanan değişkenler için geçerli değildir. Bu durumda `Temporal Dead Zone` hatası alınır.

## 1.2 Emptiness

Javascript'te 3 tip boşluk vardır. Bunlar `uninitialized`, `undefined` ve `undeclared`'dır. Bu boşluklar farklı anlamlara gelmektedir.

### `Uninitialized` (TDZ) (Temporal Dead Zone)

Hiç initialize(başlatılmamış) edilmemiş değişken. `const` ve `let` ile oluşturulan değişkenler, tanımlanmadan önce kullanılmaya çalışıldığında `Temporal Dead Zone` (TDZ) hatası alınır. Javascript [hoisting](#11-hoisting) yaptığı için değişken oluşturulmuş fakat değişkenin kullanıldığı yerde henüz kendisi initialize edilmemiştir

TDZ hatası verecek kod örneği:

```javascript
console.log(user);

{...}

const user = "e"
```

### `undefined`

Değişken initialize edilmiş fakat bir değişkene bir değer tanımlanmamıştır. Aralarında en yaygın olanıdır.

### `Undeclared`

Değişken hiç tanımlanmamıştır. Bu durumda kod çalıştırıldığında `ReferenceError` hatası alınır. ESLint gibi statik analiz araçları bu durumu kontrol edebilir.

## 1.3 `NaN` (Not a Number)

Matematiksel işlem sonucu oluşan hata durumunu belirten `number` tipinde bir değerdir. IEEE 754 standardına göre kendisi de dahil herhangi bir değer ile yapılan bir işlemin sonucu `NaN` dönmektedir.

- `string` değeri `Number(value)` fonksiyonu ile çevrilmeye çalışıldığında `NaN` döner. Bu işlemlerde `Number.isNan(value)` fonksiyonu kullanılmalıdır.
- `NaN` yerine 0 kullanılamaz.

## 1.4 Negative Zero `-0` ve Positive Zero `0`

Javascript'te IEEE 754 standardında uygun olarak negative zero ve positive zero değerleri bulunur. Negative zero Javascript'e daha sonraki versiyonlarda dahil edildiğinden dolayı `0` ve `-0` değerleri `===` operatörü ile karşılaştırıldığında `true` dönecektir. Fakat `Object.is(value)` fonksiyonu sonradan eklendiği ve daha doğru çalıştığı için bu değerleri birbirinden ayrıt edebilir.

## Notlar

- `undefined` tipi bir değerin genel olarak tanımsız değil **o anda** tanımsız olduğunu gösterir.
- `typeof` operatörü değişkenin tipini `string` olarak döndürür.
- `null` tipi bir `object` tipi değerdir. ES1'de yapılan tanımlama yüzünden bu şekilde kalmıştır. Bu davranış bir bug olarak düşünülebilir.
- `Array` tipi `typeof` operatörü ile kontrol edildiğinde `Object` döndürecektir. Dolayısıyla bir değerin `Array` olup olmadığı `Array.isArray(value)` fonksiyonu ile kontrol edilir.

### [Sonraki Sayfa](./2_COERCION.md)
