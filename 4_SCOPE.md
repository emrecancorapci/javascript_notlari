# 4. Scope

## 4.1 Introduction

Scope, bir değişkenin tanımlandığı yerden yola çıkarak erişilebilirliğini belirler. Scope'lar fonksiyonlar ve bloklar aracılığıyla oluşturulur. Bu scope'lar `lexical scope` olarak adlandırılır.

### 4.1.1 Lexical Scope

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

## 4.1 Compiler Theory

- Compiler, Javascript kodunu `JavaScript Virtual Machine`'in anlayacağı dile çevirirken `lexical scope`'un da belirlenmesi gerçekleşir. Bu işlemi yapılırken kullandığı bir diğer terim ise `scope resolution`'dır. Bu terim, bir değişkenin hangi scope'da olduğunun belirlenmesi anlamına gelir.
- Bu Compile işleminden sorumlu iki terim bulunur: `Engine` ve `Scope Manager`. `Engine`, kodun çalıştırılmasından sorumlu olan kısımdır. `Scope Manager` ise `lexical scope`'un belirlenmesinden sorumludur.

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
