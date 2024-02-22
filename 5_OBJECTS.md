# 5. Objects

- Fonksiyonları çağırmanın birkaç yolu bulunmaktadır. Bu yollar `call`, `apply`, `bind` ve `new` fonksiyonlarıdır. Bu fonksiyonlar, fonksiyonların `this` anahtar kelimesinin değerini değiştirmek için kullanılır.

## 5.1 Implicit Binding

- `this` anahtar kelimesi, bir fonksiyonun çağrıldığı zaman, fonksiyonun çağrıldığı objeyi işaret eder. Bu işlem ***implicit binding*** olarak adlandırılır.
- Aşağıdaki örnekte `yap` fonksiyonu `meyve` objesi içeriği ile çağrıldığı için `this` anahtar kelimesi `meyve` objesini işaret eder.

    ```javascript
    const meyve = {
    isim: 'Elma',
    soy() {
        console.log(this.isim, 'soyuldu!');
    }
    };

    meyve.soy(); // Elma soyuldu!
    ```

- Aşağıdaki örnekte `soy` fonksiyonu `meyve` ve `sebze` objeleri içeriği ile çağrıldığı için `this` anahtar kelimesi sırasıyla `meyve` ve `sebze` objelerini işaret eder.

    ```javascript
    function soy() {
    console.log(this.isim, 'soyuldu!');
    }

    const meyve = {
    isim: 'Elma',
    soy: soy
    };

    const sebze = {
    isim: 'Domates',
    soy: soy
    };

    meyve.soy(); // Elma soyuldu!
    sebze.soy(); // Domates soyuldu!
    ```

## 5.2 Explicit Binding

- Fonksiyonları çağırmanın birkaç yolu bulunmaktadır. Bu yolların en yaygın olanlarından biri de `call` fonksiyonudur.

    ```javascript
    function mandalinaSoy() {
    console.log('Mandalina soyuldu!');
    }

    mandalinaSoy.call(); // Mandalina soyuldu!
    ```

- `call` fonksiyonu, fonksiyonu çağırırken `this` anahtar kelimesinin değerinin  ve fonksiyon parametrelerinin değiştirilebilmesini sağlar.

    ```javascript
    function eylem(ismi) {
    console.log(this.meyveİsmi, ismi);
    }

    const meyve = {
    meyveİsmi: 'Domates'
    };

    eylem.call(meyve, 'soy!'); // Domates soy! 
    ```

- Burada `call` fonksiyonu, `greet` fonksiyonunu `person` objesi içeriği ile çağırır. Bu sayede `this` anahtar kelimesi `person` objesini işaret eder. Bu işlem ***explicit binding*** olarak adlandırılır.

## 5.3 Hard Binding

- `call` fonksiyonu, fonksiyonu çağırırken `this` anahtar kelimesinin değerini değiştirebilir. Ancak, bu değişiklikler kalıcı olmaz. Yani, bir fonksiyonun `this` anahtar kelimesinin değerini bir kez değiştirdiğinizde, bu değişiklikler sadece o çağrı için geçerli olur.
- `bind` fonksiyonu, fonksiyonun `this` anahtar kelimesinin değerini kalıcı olarak değiştirmek için kullanılır.

    ```javascript
    function soy() {
    console.log(this.isim, 'soyuldu!');
    }

    const meyve = {
    isim: 'Portakal'
    };

    const portakalSoy = soy.bind(meyve);
    portakalSoy(); // Portakal soyuldu!
    ```

- `bind` fonksiyonu, `greet` fonksiyonunu `person` objesi içeriği ile bağlar. Bu sayede `this` anahtar kelimesi `person` objesini işaret eder. Bu işlem ***hard binding*** olarak adlandırılır.
- Aşağıdaki örnekte ilk `setTimeout` fonksiyonu *global*'e bağlanmıştır. `this` anahtar kelimesi *global*'de `isim` değişkeni bulunmadığından dolayı `this.isim` ifadesi `undefined` değerini dönmüştür.
  
    ```javascript
    const meyve = {
    isim: 'Armut',
    yap(isim) {
        console.log(this.isim, isim);
    }
    };

    setTimeout(meyve.yap, 1000, "yemek!"); // undefined yemek!
    setTimeout(meyve.yap.bind(meyve), 1000, "yemek!"); // Elma yemek!
    ```

- İkinci `setTimeout` fonksiyonunda ise `bind` fonksiyonu kullanılarak, `meyve.yap` fonksiyonunu `meyve` objesi içeriği ile bağlanmıştır. Bu sayede `this` anahtar kelimesi `meyve` objesini işaret eder.

### `new` Keyword

- `new` anahtar kelimesi, bir fonksiyonu bir obje olarak çağırmak için kullanılır. Bu sayede fonksiyondaki `this` anahtar kelimesi, yeni oluşturulan objeyi işaret eder. Bu işlem ***new binding*** olarak adlandırılır.

- `new` anahtar kelimesi kullanıldığında gerçekleşen işlemler aşağıdaki gibidir:

  1. Yeni bir boş obje oluşturulur.
  2. Bu obje başka bir objeye bağlanır.*
  3. Yeni nesneyi işaret eden `this` anahtar sözcüğüyle fonksiyonu çağırır.
  4. Fonksiyon bir obje döndürmezse, `this` değeri otomatik olarak döndürülür.

## 5.4 Default Binding

- `this` anahtar kelimesi, bir fonksiyonun çağrıldığı zaman, fonksiyonun çağrıldığı objeyi işaret eder. Eğer bir fonksiyonun çağrıldığı zaman, fonksiyonun çağrıldığı obje yoksa, `this` anahtar kelimesi *global* objeyi işaret eder. Bu işlem ***default binding*** olarak adlandırılır.
- Aşağıdaki örnekte `yap` fonksiyonu *global* obje içeriği ile çağrıldığı için `this` anahtar kelimesi *global* objeyi işaret eder.

    ```javascript
    function yap() {
    console.log(this.isim, 'yapıldı!');
    }

    const meyve = {
    isim: 'Elma',
    yap: yap
    };

    const yap = meyve.yap;
    yap(); // undefined yapıldı!
    ```

- Aşağıdaki örnekte `soy` fonksiyonu *strict mode* içerisinde `this` ile *global* objeyi işaret etmediğinden dolayı `TypeError` hatası almıştır.

    ```javascript
    var meyve = 'karpuz'; 

    function kes() {
    console.log(this.meyve, 'kesildi!');
    }

    function soy() {
        "use strict";
        console.log(this.meyve, 'soyuldu!');
    }

    kes(); // karpuz kesildi!
    soy(); // TypeError: Cannot read property 'meyve' of undefined
    ```

## 5.5 Arrow Functions

- Arrow fonksiyonlarda `this` anahtar kelimesi, fonksiyonun tanımlandığı *lexical scope*'a işaret eder. Bu durum `function` anahtar kelimesi ile tanımlanan fonksiyonlardan farklıdır.
- Aşağıdaki örnekte `yap` fonksiyonu *lexical scope*'a işaret ettiği için `this` anahtar kelimesi *global* objeyi işaret eder.

    ```javascript
    const meyve = {
    isim: 'Elma',
    yap: () => {
        console.log(this.isim, 'yapıldı!');
    }
    };

    meyve.yap(); // undefined yapıldı!
    meyve.yap.call(meyve); // undefined yapıldı!
    ```

> ### Objelerde `this` Anahtar Kelimesi
>  
> `meyve` objesi bir `lexical scope` oluşturmadığı için `call` fonksiyonu ile `yap` fonksiyonu çağrıldığında `this` anahtar kelimesi *global* objeyi işaret eder.

- Aşağıdaki örnekte `setTimeout` fonksiyonu *lexical scope*'a işaret ettiği için `this` anahtar kelimesi `meyve` objesini işaret eder.

    ```javascript
    const meyve = {
    isim: 'Elma',
    yap() {
        setTimeout(() => {
        console.log(this.isim, 'yapıldı!');
        }, 1000);
    }
    };

    meyve.yap(); // Elma yapıldı!
    ```

## 5.6 Notlar

- Aynı `class`'ta olsa bile `this` kullanarak *context*'ten bir *property* çağıran bir fonksiyonun aynı *scope*'taki başka bir fonksiyondan çağrılması durumunda `bind` **yapılmaması**, `this`'in *global* objeyi işaret etmesine sebep olur. Bu nedenle `this` kullanan fonksiyonlar başka bir fonksiyondan çağrılırken `bind(this)` kullanılarak objeye *context* verilmelidir.
