# 1. Types

## 1.1. Value Types

- C++, C# ve Java gibi dillerde değişkenlerin tipleri vardır. Fakat javascript'te **değerlerin tipleri** vardır.
- Primitive (İlkel - Temel) Types: `undefined`, `string`, `number`, `boolean`, `symbol`, `null`, `bigint`
- Object Types: `object`, `function (callable objects)`, `array`

> ### `null` tipi
>  
> `null` tipi bir `object` tipi değerdir. **ES1**'de yapılan tanımlama yüzünden bu şekilde kalmıştır. Bu davranış bir bug olarak düşünülebilir.
>  
> ### Array Sorgulama
>  
> `Array` tipi `typeof` operatörü ile kontrol edildiğinde `Object` döndürecektir. Dolayısıyla bir değerin `Array` olup olmadığı `Array.isArray(value)` fonksiyonu ile kontrol edilebilir.

## 1.2. Emptiness

3 tip *empty value* vardır. Bunlar `uninitialized`, `undefined` ve `undeclared`'dır. Bu emptiness'lar farklı anlamlara gelmektedir.

### `Uninitialized` (TDZ) (Temporal Dead Zone)

Hiç initialize(başlatılmamış) edilmemiş değişken. `const` ve `let` ile oluşturulan değişkenler, tanımlanmadan önce kullanılmaya çalışıldığında `Temporal Dead Zone` (TDZ) hatası alınır. Javascript [hoisting](/4_SCOPE.md/#46-hoisting) yaptığı için değişken oluşturulmuş fakat değişkenin kullanıldığı yerde henüz kendisi initialize edilmemiştir

TDZ hatası verecek kod örneği:

```javascript
console.log(user);

const user = "e"
```

### `undefined`

Değişken initialize edilmiş fakat bir değişkene bir değer tanımlanmamıştır. Aralarında en yaygın olanıdır.

> ### Tanımsızlık Kapsamı
>  
> `undefined` tipi bir değerin genel olarak tanımsız değil **o anda** tanımsız olduğunu gösterir.

### `Undeclared`

Değişken **hiç** tanımlanmamıştır. Bu durumda kod çalıştırıldığında `ReferenceError` hatası alınır. *ESLint* gibi statik analiz araçları bu durumu kontrol edebilir.

## 1.3. `NaN` (Not a Number)

Matematiksel işlem sonucu oluşan hata durumunu belirten `number` tipinde bir değerdir. IEEE 754 standardına göre kendisi de dahil herhangi bir değer ile yapılan bir işlemin sonucu `NaN` dönmektedir.

- `string` değeri `Number(value)` fonksiyonu ile çevrilmeye çalışıldığında `NaN` döner. Bu işlemlerde `Number.isNan(value)` fonksiyonu kullanılmalıdır.
- `NaN` yerine 0 kullanılamaz.

## 1.4. Negative Zero `-0` ve Positive Zero `0`

**IEEE 754** standardında uygun olarak *negative zero* ve *positive zero* değerleri bulunur. *Negative zero* daha sonraki **ECMAScript** versiyonlarda dahil edildiğinden dolayı `0` ve `-0` değerleri `===` operatörü ile karşılaştırıldığında `true` dönecektir. Fakat sonradan eklenen ve daha doğru çalışan `Object.is(value)` fonksiyonu bu değerleri birbirinden ayrıt edebilir.

### [Ana Sayfa](./README.md) | [Sonraki Sayfa](./2_COERCION.md) | [Yukarı Çık](#1-types)
