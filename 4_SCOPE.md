# 4. Scope

Scope, bir değişkenin tanımlandığı yerden yola çıkarak erişilebilirliğini belirler. Scope'lar fonksiyonlar ve bloklar aracılığıyla oluşturulur. Bu scope'lar `lexical scope` olarak adlandırılır.

## 4.1 Lexical Scope

- `Lexical` terimi basitçe `kaynak kodu` veya başka bir deyişle `bir programın metniyle ilgili` anlamına gelir. `Lexical scope`'un bir diğer adı ise `static scope`'dur. Bu isim, bir değişkenin hangi scope'da olduğunun programın yazılım aşamasında belirlendiğini ifade eder.

- Bir fonksiyonun içindeki bir değişken önce fonksiyonun `local environment`'ında (yerel ortamında), daha sonra ise `lexical environment`'ında aranır.

- `Lexical environment` bir fonksiyon için o fonksiyonun kaynak kodundaki tanımını kapsayan ortamı ifade eder. Bu ortam fonksiyonun tanımlandığı yerdeki scope'lar ve bu scope'lar içerisindeki değişkenlerin birleşiminden oluşur.

- Aşağıdaki örnekte `lexical scope`'un nasıl çalıştığını görebiliriz:

```javascript
var a = 'global';

function f1() {
   console.log(a);
}

function f2() {
   var a = 'local';
   f1();
}

f2();
```

- Yukarıdaki örnekte `f1` fonksiyonu `f2` fonksiyonunun içerisinde çağrıldığı halde `a` değişkeninin değeri `global` olacaktır. Çünkü `f1` fonksiyonu `f2` fonksiyonunun tanımı içerisinde değil, global scope'da tanımlıdır. Fonksiyon öncelikle `a` değişkenini kendi scope'unda arayacak, bulamazsa `lexical environment`'a bakacaktır.

## 4.2 Compiler Theory

- `Compiler` terimi, bir programın kaynak kodunu `machine code`'a çeviren bir yazılımı ifade eder. `Machine code` ise bir bilgisayarın anlayabileceği dilde yazılmış kodları ifade eder.
- Javascript kodu `Compile Time` ve `Run Time` diye adlandırılan iki aşamada çalıştırılır.. `Compile Time`'da kodun `lexical scope`'u belirlenirken `Run Time`'da kodun çalıştırılması gerçekleşir.
- Compiler, Javascript kodu `JavaScript Virtual Machine`'in anlayacağı dile çevrilirken diğer yandan da `lexical scope`'un da belirlenmesi gerçekleşir. Bu işlemi yapılırken uygulanan bir diğer terim ise `scope resolution`'dır. Bu terim, bir değişkenin hangi scope'da olduğunun belirlenmesi anlamına gelir.
- Bu Compile işleminden sorumlu olan `Engine` ve `Scope Manager` isimli iki varlık bulunur. `Engine`, kodun çalıştırılmasından sorumlu olan kısımdır. `Scope Manager` ise `lexical scope`'un belirlenmesinden sorumludur.

### Scope Manager

```javascript
var meyve = 'elma';

function muzSoy() {
   var meyve = 'muz';
   console.log('Muz soyuldu!');
}

function soy() {
   var sebze = 'havuç';
   console.log(meyve, 'soyuldu!');
}

muzSoy(); // Muz soyuldu!
soy(); // elma soyuldu!
```

- Yukarıdaki kodda Scope Manager sırayla aşağıdaki adımları gerçekleştirir:
  1. `meyve` değişkeni `lexical environment`'a eklenir.
  2. `muzSoy` fonksiyonunun `lexical environment`'ı oluşturulur.
  3. `meyve` değişkeni `muzSoy` fonksiyonunun `lexical environment`'ına eklenir.
  4. `soy` fonksiyonunun `lexical environment`'ı oluşturulur.
  5. `sebze` değişkeni `soy` fonksiyonunun `lexical environment`'ına eklenir.
  6. `meyve` değişkeni `console.log(meyve, 'soyuldu!')` fonksiyonunda referans edildiğinden dolayı `soy` fonksiyonunun `lexical environment`'ına eklenir.
- Tüm bu işlemler `Compile Time`'da gerçekleşir.

### The Javascript Engine

- `Engine`, kodun çalıştırılmasından sorumlu olan kısımdır. `Engine`'un görevi `lexical scope`'u kullanarak değişkenlere erişmek ve bu değişkenlerin değerlerini almak veya değiştirmektir.

```javascript
var meyve = 'elma';
console.log(meyve);

function muzSoy() {
   var meyve = 'muz';
   console.log('Muz soyuldu!');
}

muzSoy();
```

- Yukarıdaki kodda ilk satırda `meyve` değişkeni `l-value(source)`, `elma` değeri ise `r-value(target)` olarak adlandırılır.
- `meyve`'nin `lexical environment`'ta olup olmadığı Engine tarafından kontrol edilir. Eğer mevcutsa `elma` değeri, `meyve` değişkenine atanır.
- İkinci satırda ise r-value olan `meyve` değişkeni l-value olan `console.log()` fonksiyonuna referans edildiği için `lexical environment`'a bakılır. `meyve` değişkeni bulunur ve `elma` değeri yazdırılır.
- L-value olan `muzSoy` fonksiyonu çalıştırıldığında `lexical environment`'a bakılır ve `muzSoy` fonksiyonunun `lexical environment`'ı bulunur. Environment'taki kod çalıştırılır ve `Muz soyuldu!` yazdırılır.

> Çağrılan fonksiyonlar neden l-value olur sorusuna verilebilecek en iyi cevap, kendisinin bir `target reference` olmamasıdır. Eğer bir `r-value` değilse, bir `l-value` olmalıdır çünkü elimizde iki seçenek vardır.

## 4.3 Nested Scopes

- `Nested scope`, bir scope'un başka bir scope'un içerisinde bulunması durumunu ifade eder. Bu durumda içteki scope, dıştaki scope'un değişkenlerine erişebilir.

```javascript
var eylem = 'kesildi!';

function elmaSoy() {
   var eylem = 'soyuldu!';

   function soy(meyve) {
      console.log(meyve, eylem);
   }

   soy('elma');
}

elmaSoy(); // elma soyuldu!
soy('muz'); // ReferenceError: soy is not defined
```

- Yukarıdaki örnekte `soy` fonksiyonu `elmaSoy` fonksiyonunun içerisinde tanımlıdır. Bu yüzden `soy` fonksiyonu `elmaSoy` fonksiyonunun `lexical environment`'ına yani `eylem` değişkenine erişebilir. Ancak `soy` fonksiyonu `elmaSoy` fonksiyonunun dışında tanımlı olmadığı için bu fonksiyona global scope'da erişilemez. Bu yüzden `soy('muz')` fonksiyonu global scope'da tanımlı olmadığı için fonksiyon çalıştırıldığında `ReferenceError` hatası verir.

## 4.4 IIFE Pattern

- `IIFE` (Immediately Invoked Function Expression) pattern, bir fonksiyonun tanımlandığı yerde hemen çağrılmasını sağlayan bir pattern'dir. Bu pattern, bir fonksiyonun `lexical scope`'unu kullanarak global scope'dan izole edilmesini sağlar.
- İçerdeki fonksiyon bir `function declaration` değil, `function expression` olarak nitelendirilir.

> `function declaration`, bir fonksiyonun adını ve gövdesini belirterek tanımlanmasını ifade eder. `function expression` ise bir fonksiyonun bir değişkene atanarak tanımlanmasını ifade eder.

```javascript
( function elmaSoy(meyve) {
   console.log(meyve, 'soyuldu!');
} )('elma');
```

- Bu pattern kullanılarak değişken şartlı atamaları tek seferde yapılabilir.

```javascript
var meyve = ( function() {
   try {
      return fetchMeyve(1);
   } catch (error) {
      return 'elma';
   }
} )();
```

## 4.5 var vs let vs const

- `var` anahtar kelimesi ile tanımlanan değişkenler `lexical scope`'a sahiptir. `let` ve `const` anahtar kelimeleri ile tanımlanan değişkenler ise `block scope`'a sahiptir.

> `block scope`, bir blok içerisinde tanımlanan değişkenlerin sadece o blok içerisinde erişilebilir olmasını ifade eder.

```javascript
function yiyecek() {
   { // a block
      var meyve = 'elma';
      let sebze = 'domates';
      const protein = 'tavuk';

      console.log(meyve); // elma
      console.log(sebze); // domates
      console.log(protein); // tavuk

      meyve = 'muz';
      sebze = 'havuç';
      protein = 'kırmızı et'; // TypeError: Assignment to constant variable.
   }

   console.log(meyve); // muz
   console.log(sebze); // ReferenceError: sebze is not defined
   console.log(protein); // ReferenceError: protein is not defined
}
```

- `const` anahtar kelimesi ile tanımlanan değişkenlerin değeri bir kez atanır ve daha sonra değiştirilemez. Fakat `const` ile tanımlanan bir array veya object'in içeriği değiştirilebilir.

```javascript
const peynir = ['kaşar', 'beyaz', 'tulum'];
console.log(peynir); // ['kaşar', 'beyaz', 'tulum']

peynir = ['cheddar', 'gouda', 'roquefort']; // TypeError: Assignment to constant variable.

peynir[0] = 'cheddar';
console.log(peynir); // ['cheddar', 'beyaz', 'tulum']
```

## 4.6 Hoisting

Genel kanının aksine Javascript'te tanımlamalar en yukarıya çekilmez. Öncelikle compile edildiği için tanımlamalar gerçekleştirildikten sonra kod çalıştırılır. `var` ile tanımlanan değişkenler ve fonksiyonlara tanımları yapılmadan önce erişilebildiği halde `let` ve `const` ile tanımlanan değişkenlere erişilemez. Erişilmeye çalışıldığında ise `TDZ (Temporal Dead Zone)` hatası alınır.

```javascript
var meyve = 'elma';

meyveSoy(): // Elma soyuldu!

function meyveSoy() {
   console.log(meyve, 'soyuldu!');
}
```
