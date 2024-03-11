# Functional JavaScript First Steps

- Fonksiyonel programlamada **pure function**'lar kullanılır.

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

## Neden Pure Function Kullanmalıyız?

- **Referential transparency**'ye sahip olduğu için test edilmesi kolaydır.
- **Memoization** yapılabilir. (Referential transparency sayesinde, daha önce hesaplanmış bir sonucu tekrar hesaplamak yerine, daha önce hesaplanmış sonucu kullanabiliriz.)
- **Parallel programming** yapılabilir. (Referential transparency sayesinde, aynı input'lar için aynı anda birden fazla işlem yapılabilir.)
- **Cache**'lenebilir. (Memoization sayesinde, daha önce hesaplanmış sonuçları cache'leyebiliriz.)
- **Debug** etmesi kolaydır. (Deterministik olduğu için, hata ayıklaması kolaydır.)
