# 1. Tipler

- C++, C# ve Java gibi dillerde değişkenlerin tipleri vardır. Fakat javascript'te değerlerin tipleri vardır.
- Temel (İlkel - Primitive) tipler: `undefined`, `string`, `number`, `boolean`, `symbol`, `null`, `bigint`
- Object tipleri: `object`, `function (callable objects)`, `array`

## 1.1 Emptiness (Boşluk)

Javascript'te 3 tip boşluk vardır.

### Uninitialized (TDZ) (Temporal Dead Zone)

Hiç initialize(başlatılmamış) edilmemiş değişken. const ve let ile oluşturulan değişkenler, tanımlanmadan önce kullanılmaya çalışıldığında `Temporal dead zone (TDZ)` hatası verir. Javascript hoisting (tüm tanımları en yukarıya çekmek) yaptığı için değişken oluşturulmuş fakat initilaze edilmemiştir.

Örnek bir tdz hatası:

```javascript
console.log(user);
const user = "e"
```

### Undefined

Değişken initialize edilmiş fakat bir değişkene bir değer tanımlanmamıştır.

### Undeclared

Değişken tanımlanmamıştır. Bu durumda `ReferenceError` hatası alınır.

## 1.2 NaN (Not a Number)

`NaN` bir number tipidir. `NaN` bir matematiksel işlem sonucu oluşan bir hata durumudur. IEEE 754 standardına göre `NaN`'ın kendisi de dahil herhangi bir değer ile yapılan bir işlemin sonucu `NaN` döner.

- `string` değeri `Number(value)` fonksiyonu ile çevrilmeye çalışıldığında `NaN` döner. Bu işlemlerde `Number.isNan(value)` fonksiyonu kullanılmalıdır.
- `NaN` yerine 0 kullanılamaz.

## 1.3 Negatif Sıfır ve Pozitif Sıfır

Javascript'te negatif sıfır ve pozitif sıfır vardır. Bu durum IEEE 754 standardından kaynaklanır. Bu durumda `0` ve `-0` değerleri birbirinden farklıdır. Fakat `===` operatörü ile karşılaştırıldığında aynı değer döner. Fakat `Object.is(value)` fonksiyonu ile karşılaştırıldığında farklı değerler döner.

## Notlar

- `undefined` tipi değerin tanımsız değil **şu anda** tanımsız olduğunu gösterir.
- `typeof` operatörü değişkenin tipini string olarak döndürür.
- `null` tipi `object` tipi değerdir. ES1'de yapılan tanımlama yüzünden bu şekilde kalmıştır. Bunu bir bug olarak düşünebiliriz.
- Arrayler de birer objedir. `typeof` ile kontrol edilemeyeceğinden dolayı bir değerin array olup olmadığı `Array.isArray(array)` fonksiyonu ile öğrenilir.
