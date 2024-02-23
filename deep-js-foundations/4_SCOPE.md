# 4. Scope

Scope, bir değişkenin tanımlandığı yerden yola çıkarak erişilebilirliğini belirler. Scope'lar fonksiyonlar ve bloklar aracılığıyla oluşturulur. Bu scope'lar `lexical scope` olarak adlandırılır.

## 4.1. Lexical Scope

- `Lexical` terimi basitçe `kaynak kodu` veya başka bir deyişle `bir programın metniyle ilgili` anlamına gelir. `Lexical scope`'un bir diğer adı ise `static scope`'dur. Bu isim, bir değişkenin hangi scope'da olduğunun programın yazılım aşamasında belirlendiğini ifade eder.

> ### Değişken Arama Sırası
>  
> Bir fonksiyonun içindeki bir değişken önce fonksiyonun `local environment`'ında (yerel ortamında), daha sonra ise `lexical environment`'ında aranır.

- `Lexical environment` bir fonksiyon için o fonksiyonun kaynak kodundaki tanımını kapsayan ortamı ifade eder. Bu ortam fonksiyonun tanımlandığı yerdeki scope'lar ve bu scope'lar içerisindeki değişkenlerin birleşiminden oluşur.

- Aşağıdaki örnekte `f1` fonksiyonu `f2` fonksiyonunun içerisinde çağrıldığı halde `a` değişkeninin değeri `global` olacaktır. Çünkü `f1` fonksiyonu `f2` fonksiyonunun tanımı içerisinde değil, global scope'da tanımlıdır. Fonksiyon öncelikle `a` değişkenini kendi scope'unda arayacak, bulamazsa `lexical environment`'a bakacaktır.

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

## 4.2. Nested Scopes

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

## 4.3. IIFE Pattern

- `IIFE` (Immediately Invoked Function Expression) pattern, bir fonksiyonun tanımlandığı yerde hemen çağrılmasını sağlayan bir pattern'dir. Bu pattern, bir fonksiyonun `lexical scope`'unu kullanarak global scope'dan izole edilmesini sağlar.
- İçerdeki fonksiyon bir `function declaration` değil, `function expression` olarak nitelendirilir.

> ### Declaration ve Expression arasındaki fark
>  
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

## 4.4. var vs let vs const

- `var` anahtar kelimesi ile tanımlanan değişkenler `lexical scope`'a sahiptir. `let` ve `const` anahtar kelimeleri ile tanımlanan değişkenler ise `block scope`'a sahiptir.

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

> ### Block Scope Nedir?
>  
> `block scope`, bir blok içerisinde tanımlanan değişkenlerin sadece o blok içerisinde erişilebilir olmasını ifade eder.

- `const` anahtar kelimesi ile tanımlanan değişkenlerin değeri bir kez atanır ve daha sonra değiştirilemez. Fakat `const` ile tanımlanan bir array veya object'in içeriği değiştirilebilir.

   ```javascript
   const peynir = ['kaşar', 'beyaz', 'tulum'];
   console.log(peynir); // ['kaşar', 'beyaz', 'tulum']

   peynir = ['cheddar', 'gouda', 'roquefort']; // TypeError: Assignment to constant variable.

   peynir[0] = 'cheddar';
   console.log(peynir); // ['cheddar', 'beyaz', 'tulum']
   ```

> Objeler *scope* oluşturmaz.

## 4.5. Hoisting

- Genel kanının aksine Javascript'te tanımlamalar en yukarıya çekilmez. Öncelikle compile edildiği için tanımlamalar gerçekleştirildikten sonra kod çalıştırılır.
- `var` ile tanımlanan değişkenler ve fonksiyonlara tanımları yapılmadan önce erişilebildiği halde `let` ve `const` ile tanımlanan değişkenlere erişilemez. Erişilmeye çalışıldığında ise `TDZ (Temporal Dead Zone)` hatası alınır.

   ```javascript
   var meyve = 'elma';

   meyveSoy(): // Elma soyuldu!

   function meyveSoy() {
      console.log(meyve, 'soyuldu!');
   }
   ```

## 4.6. Closure

- Bir fonksiyonun mevcut scope'u dışındaki bir değişkene erişebilmesi durumuna `closure` denir. Bu durumda fonksiyon, kendi scope'u dışındaki bir değişkene erişebilir ve bu değişkeni kullanabilir.
- Aşağıdaki örnekte `soy` fonksiyonu `elmaSoy` fonksiyonunun içerisinde tanımlıdır. Bu durumda closure var olduğu için `soy` fonksiyonu `elmaSoy` fonksiyonunun `lexical environment`'ına erişebilir. Bu durumda `soy` fonksiyonu `eylem` değişkenine erişebilir ve bu değişkeni kullanabilir. Bu durumda `soy` fonksiyonu `closure`'a sahiptir.

   ```javascript
   function elmaSoy() {
      var eylem = 'soyuldu!';

      function soy(meyve) {
         console.log(meyve, eylem);
      }

      return soy;
   }

   var muzSoy = elmaSoy();
   muzSoy('muz'); // muz soyuldu!
   ```

> ### Closure Nerelerde Kullanılır?
>  
> Closure fonksiyonları `once` ve `memoize` gibi helper fonksiyonlar, `iterator` ve `generator` fonksiyonları, `Module Pattern` ve `Revealing Module Pattern` design pattern'ları, `Callback` ve `Promise` async operasyonları gibi birçok alanda kullanılır.

## 4.7. Module Pattern

- `Module Pattern`, bir fonksiyonun dışarıya sadece belirli fonksiyonları veya değişkenleri açmasını sağlayan bir pattern'dir. Bu pattern, `closure`'ı kullanarak bir fonksiyonun dışarıya sadece belirli fonksiyonları veya değişkenleri açmasını sağlar.

   ```javascript
   var module = ( function() {
      var privateVariable = 'private';

      function privateFunction() {
         console.log('private function');
      }

      return {
         publicVariable: 'public',
         publicFunction: function() {
            console.log('public function');
         }
      };
   } )();

   console.log(module.privateVariable); // undefined
   module.privateFunction(); // TypeError: module.privateFunction is not a function
   console.log(module.publicVariable); // public
   module.publicFunction(); // public function
   ```

### [Önceki Sayfa](./3_EQUALITY.md) | [Sonraki Sayfa](./5_ENGINE.md) | [Ana Sayfa](../README.md) | [Yukarı Çık](#4-scope)
