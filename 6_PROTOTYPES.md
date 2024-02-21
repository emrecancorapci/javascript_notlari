# 6. Prototypes

## 6.1 Prototype

- JavaScript'te her obje bir *prototype*'a sahiptir. *Prototype*, bir objenin diğer objelerden özelliklerini miras almasını sağlar.
- Bir objenin *prototype*'ına, `__proto__` *property*'si ile erişilebilir. Bir *prototype*'ın *property*'lerine ise `prototype` *property*'si ile erişilebilir.

    ```javascript
    const meyve = {
    isim: 'Elma'
    };

    console.log(meyve.__proto__ === Object.prototype); // true
    ```

- Objeler `new` anahtar kelimesi ***constructor calls*** ile oluşturulur. ***Constructor call*** objeyi kendi *prototype*'ı ile birleştirir.

### Prototype Chain

- Bir objenin *prototype*'ı, bir başka objenin *prototype*'ı olabilir. Bu durumda bir ***prototype chain***  oluşur. Ve *prototype*'ın sahip olduğu özellikler, *prototype chain* ile birbirine bağlıdır.
- Tüm objelerin *prototype*'ları, `Object.prototype`'a kadar uzanır. Bu durumda bir objenin *prototype*'ı olmadığı zaman, `Object.prototype`'a bakılır.

    ```javascript
    function Meyve(isim) {
    this.isim = isim;
    }

    Meyve.prototype.soy = function() {
    console.log(this.isim, 'soyuldu!');
    };

    const elma = new Meyve('Elma');

    elma.soy(); // Elma soyuldu!
    console.log(elma.__proto__ === Meyve.prototype); // true
    console.log(Meyve.prototype.__proto__ === Object.prototype); // true
    ```

- `Object.create(context)` fonksiyonu veya `new` anahtar kelimesi ile oluşturulan bir *object*'te erişilmeye çalışılan değer bulunamazsa `__proto__` property'sine bakılır. Fonksiyondaki `context` parametresi, *object*'in `__proto__` *property*'sine atanır.

    ```javascript
    const meyve = {
      isim: 'Elma'
    };

    const muz = Object.create(meyve);
    console.log(muz.isim); // Elma
    console.log(muz.__proto__ === meyve); // true
    ```
