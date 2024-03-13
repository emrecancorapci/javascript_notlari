# Functional JavaScript First Steps

## Fonksiyonel Programlama Nedir?

Bazı yazılım dilleri tarafından desteklenen bir kodlama paradigmasıdır.

> F#, Haskell, Scala, Clojure, Erlang, Elixir, Elm, Rust, Swift, Kotlin, JavaScript, TypeScript, vb. diller fonksiyonel programlamayı desteklemektedir.

## Neden Fonksiyonel Programlama?

- **Referential transparency**'ye sahip olduğu için **test**/**debug** edilmesi kolaydır.
- Kolayca **parallel programming**, **memoization** ve **caching**' yapılabilir.
- **Side effect** oluşturmadığı için kodun okunabilirliği ve güvenilirliği yüksektir.

## Fonksiyonel Programlama Prensipleri

- **Immutable** veri yapıları kullanılır.
- Değişkenler **const** ile tanımlanır.
- **Pure Function**'lar kullanılır.
- Loop yerine **map**, **filter**, **reduce** gibi fonksiyonlar kullanılır.

## Pure Function Nedir?

- Input aldığı şey haricinde dışarıdan bir şeylere bağımlı değildir.
- Side effect oluşturmaz. (Dosya okuma, yazma, network işlemleri, veritabanı işlemleri, vs.)
- **Referential transparency**'ye sahiptir. (Aynı input için her zaman aynı output'u verirler.)

> Dosya okuma, yazma, network işlemleri, veritabanı işlemleri, vs. gibi işlemler side-effect oluşturduğu için bir projenin tamamı pure function'dan oluşamaz. Bu işlemler genellikle projenin en dış katmanında yapılır ve iç katmanlarda pure function'lar kullanılır. Bu sayede projenin iç katmanları daha test edilebilir, daha okunabilir ve daha güvenilir olur.

`Not Pure Function`

```javascript
let a = 1;

function add(b) {
  return a + b;
}

add(2); // 3
a = 4;
add(2); // 6
```

`Pure Function`

```javascript
function add(a, b) {
  return a + b;
}

add(1, 2); // 3
add(4, 2); // 6
```

## Recursive Function

Kenar koşulunu sağlayan bir durum olana kadar **kendini tekrar eden fonksiyonlardır**. Recursive fonksiyonlar tekrardan çıkacakları bir **base case** ve kendilerini tekrar edecekleri bir **recursive case**'e sahip olmalıdır.

### Recursive Factorial

```javascript
function recursiveFactorial(n) {
  if (n === 0) return 1; // Base case
  return n * factorial(n - 1); // Recursive case
}
```

### Iterative Factorial

```javascript
function iterativeFactorial(n) {
  let product = 1;
  while (n > 0) {
    product *= n;
    n--;
  }
  return product;
}
```

## First-Class Function

- **First-Class Function**'lar, diğer veri tipleri ile aynı şekilde kullanılabilen fonksiyonlardır.
- **First-Class Function**'lar, fonksiyonları **değişkenlere atama**, **parametre olarak gönderme**, **return** olarak kullanma, **array**'lerde **tutma** gibi işlemleri yapabilir.

```javascript
const multiply = (a, b) => a * b;

const calculator = (operation, a, b) => operation(a, b);

calculator(multiply, 5, 3); // 15
```

## Higher-Order Function

- **Higher-Order Function**'lar, en az bir tane **First-Class Function** alıp, en az bir tane **First-Class Function** dönen fonksiyonlardır.
- **Higher-Order Function**'lar, **First-Class Function**'ları **parametre** olarak alabilir, **return** olarak kullanabilir, **array**'lerde **tutabilir**.

```javascript
const add = (a, b) => a + b;

const calculator = (operation) => (a, b) => operation(a, b);

const adder = calculator(add);

adder(5, 3); // 8
```

## Array Methods

- **map**: Array elemanlarını dönüştürmek için kullanılır.
- **filter**: Array elemanlarını filtrelemek için kullanılır.
- **reduce**: Array elemanlarını birleştirmek için kullanılır.

```javascript
const numbers = [1, 2, 3, 4, 5];

const doubled = numbers.map((number) => number * 2);
// [2, 4, 6, 8, 10]

const even = numbers.filter((number) => number % 2 === 0);
// [2, 4]

const sum = numbers.reduce((acc, number) => acc + number, 0);
// 15
```

## Currying

- **Currying**, bir fonksiyonun **tek parametre alan** bir fonksiyon dönmesi işlemidir. Bu sayede **partial application** yapılabilir.

```javascript
function curriedAdd(a) {
  return function add(b) {
    return a + b;
  }
}

const add5 = curriedAdd(5);
add5(3); // 8

const addAndMultiply = (a) => (b) => (c) => (a + b) * c;
addAndMultiply(1)(2)(3); // 9

const add2AndMultiply = addAndMultiply(2);
add2AndMultiply(3)(4); // 20

const add5AndMultiplyBy3 = (n) => addAndMultiply(5)(n)(3);
add5AndMultiplyBy3(4); // 27
```
