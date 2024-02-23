# 5. The Javascript Engine

Javascript kodu **Compile Time** ve **Run Time** diye adlandırılan iki aşamada gerçekleştirilir. Bu işleminden sorumlu olan **Engine** ve **Scope Manager** isimli iki araç bulunur. Öncelikle **Scope Manager**, kodun **lexical scope**'unu belirler. Ardından **Engine** bu bilgiler yardımıyla kodu çalıştırır.

## 5.1 Scope Manager

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

Yukarıdaki kodda **Scope Manager** sırayla **Compile Time**'da aşağıdaki adımları gerçekleştirir:

1. `meyve` değişkeni ***global*** *lexical environment*'a eklenir.
2. `muzSoy` fonksiyonunun *lexical environment*'ı oluşturulur ve referansı ***global*** *lexical environment*'a eklenir.
3. `meyve` değişkeni halihazırda ***global*** *lexical environment*'a var olduğu için bir eylem gerçekleştirilmez.
4. `soy` fonksiyonunun *lexical environment*'ı oluşturulur ve referansı ***global*** *lexical environment*'a eklenir.
5. `sebze` değişkeni `soy` fonksiyonunun *lexical environment*'ına eklenir.
6. `meyve` değişkeni `console.log(meyve, 'soyuldu!')` fonksiyonunda referans edildiğinden dolayı değişkenin **referansı** `soy` fonksiyonunun *lexical environment*`'ına eklenir.

## 5.2 The Javascript Engine

**Engine**, kodun çalıştırılmasından sorumlu olan kısımdır. **Engine**'in görevi **lexical scope**'u kullanarak değişkenlere erişmek ve bu değişkenlerin değerlerini referanslarına göndermek veya değişkeni değiştirmektir.

```javascript
var meyve = 'elma';
console.log(meyve);

function muzSoy() {
   var meyve = 'muz';
   console.log('Muz soyuldu!');
}

muzSoy();
```

- Yukarıdaki kodda ilk satırda `meyve` değişkeni **l-value (source)**, `elma` değeri ise **r-value (target)** olarak adlandırılır.
- `meyve`'nin *lexical environment*'ta olup olmadığı Engine tarafından kontrol edilir. Eğer mevcutsa `elma` değeri, `meyve` değişkenine atanır.
- İkinci satırda ise r-value olan `meyve` değişkeni l-value olan `console.log()` fonksiyonuna referans edildiği için *lexical environment*'a bakılır. `meyve` değişkeni bulunur ve `elma` değeri yazdırılır.
- L-value olan `muzSoy` fonksiyonu çalıştırıldığında *lexical environment*'a bakılır ve `muzSoy` fonksiyonunun *lexical environment*'ı bulunur. Environment'taki kod çalıştırılır ve `Muz soyuldu!` yazdırılır.

> Çağrılan fonksiyonlar neden *l-value* olur sorusuna verilebilecek en iyi cevap, kendisinin bir *target reference* **olmamasıdır**. Eğer bir *r-value* değilse, bir *l-value* olmalıdır çünkü elimizde iki seçenek vardır.

## 5.3 Memory and Call Stack

- *Scope Manager*'ın belirlediği *lexical environment*'ta bulunan tüm değişkenler ve fonksiyonlar **Memory**'de bir metin olarak saklanır. Bu metin **Call Stack**'e eklenir, çalıştırılır ve fonksiyon gerçekleştirildiğinde ise çıkarılır.

> **Stack**'in çalışma mantığı **LIFO** *(Last In First Out)* olarak adlandırılır. En son eklenen eleman, en önce çıkarılır. Somutlaştırmak gerekirse bir kitap yığını olarak düşünülebilir.

### [Önceki Sayfa](./4_SCOPE.md) | [Sonraki Sayfa](./6_OBJECTS.md) | [Ana Sayfa](./README.md) | [Yukarı Çık](#5-the-javascript-engine)
