# Web Performansının Temelleri

## 1. Performansın Temelleri

### Performans Neden Önemli?

- Çünkü **Google** öyle diyor.
- Ne kadar **hızlı** olursa o kadar iyi **kullanıcı deneyimi** sunar. Ne kadar iyi  **kullanıcı deneyimi** sunarsa o kadar çok **kullanıcıya** ulaşır. Ne kadar çok **kullanıcıya** ulaşırsa o kadar çok **para** kazanır.

### Beklemenin Psikolojisi

- Beklemek göreceli bir kavramdır ve bir çok faktöre bağlıdır.
- Beklemeyi göze aldığımız zaman, beklentilerimiz ve beklerken ne yapabildiğimize göre değişir.

#### İnsanlar bir an önce başlamak ister

Eğer erken bir başlangıç verirseniz bekleme süresinin kısa olduğunu düşünürler. Ama başlamak istedikleri halde onlara bir şey vermezseniz bekleme süresi yavaş hissettirir.

#### Sıkıcı şeyler daha yavaş hissettirir

Eğer kullanıcının yapacak veya bakacak herhangi bir şeyi yoksa bekleme süresi normalden daha uzun hissedilir.

#### Gerginlik daha yavaş hissettirir

Eğer bir test sonucunu bekliyorsanız, başvurduğunuz kredinin onaylanıp onaylanmadığını bekliyorsanız, bekleme süresi daha uzun hissettirir.

#### Anlamsız bekleme daha yavaş hissettirir

Eğer neden beklediğinizi bilmiyorsanız veya kimse neden beklediğinizle ilgili bir cevap veremiyorsa bekleme süresi daha uzun hissettirir.

#### Ne kadar bekleyeceğini bilmeme daha yavaş hissettirir

Eğer ne kadar bekleyeceğinizi bilmiyorsanız, yaptığınız işlemin ne kadar süreceği hakkında bir tahmininiz de yoksa bekleme süresi daha uzun hissettirir.

#### İnsanlar değerli olan şeyler için beklemeye daha isteklidir

Eğer beklediğiniz şeyin değerli olduğunu düşünüyorsanız, beklemeye daha istekli olursunuz. Yeni ürün çıktığında Apple mağazalarında kuyruğa girilmesinin sebebi de budur. Kimse değer vermediği bir şey için beklemek istemez.

### Web Vitals

#### Eski Metrikler

Eskiden performansı ölçmek için **Page Load Time** *-HTML dokümanının yüklenme süresi-* baz alınırdı. Ancak bu metrik, günümüzdeki web uygulamaları için yeterli değildir.

Günümüzde *React*, *Vue* gibi **Single Page Application** geliştirilebilmesini sağlayan kütüphaneler, kullanıcıya öncelikle boş bir HTML sayfası gönderir. Ardından javascript ile sayfa içeriği yüklenir. Bu yüzden **Page Load Time** metriği bu tür uygulamalarda kullanıcı deneyimini tam olarak yansıtamaz

Bu yüzden günümüzde bu metrik terk edilerek yerine **Google**'ın geliştirdiği **Web Vitals** metrikleri kullanılmaya başlandı.

#### Web Vitals Metrikleri

![Web Vitals](https://lh5.googleusercontent.com/HlrpevA1hZKx35h2SQdwOBdCO4FOC0YOqie9eMTiGDZx5MdSVTxY2VwPwdL58Pi68cuuG0ooeRTs3RJQEfU5woNRpgq1ZLV4SrWkzHIOH4kTnLS32i62Qf7UibEcz2xm8Gb4nT_e)

- **FCP (First Contentful Paint)**: Sayfa üzerindeki ***ilk içeriğin yüklenme süresini*** ölçer. Eğer sayfa üzerindeki ilk içerik geç yüklenirse bu metrik yüksek olur. Burada sayfada gerçekleşen ilk render işlemi kastedilir. Kullanıcının sayfanın yüklenmeye başladığını gördüğü anlamına gelir.
- **LCP (Largest Contentful Paint)**: Sayfa üzerindeki ***en büyük içeriğin yüklenme süresini*** ölçer. Eğer sayfa üzerindeki en büyük içerik geç yüklenirse bu metrik yüksek olur. Genellikle bu metrik, sayfa üzerindeki en büyük görsel veya yazı içeriğidir. Kullanıcının sayfa üzerindeki en büyük içeriği gördüğü anlamına gelir.
- **FID (First Input Delay)**: Kullanıcı ilk ***etkileşimde bulunabilene kadar geçen süreyi*** ölçer. Eğer sayfa üzerindeki içeriklerle etkileşime geçmek için uzun süre beklemek zorunda kalıyorsa bu metrik yüksek olur.
- **CLS (Cumulative Layout Shift)**: Sayfa üzerindeki ***içeriklerin sayfa var olduğu müddetçe ne kadar stabil olduğunu*** ölçer. Eğer sayfa üzerindeki içerikler sürekli olarak kayıyorsa bu metrik yüksek olur.

### Lighthouse

- **Lighthouse**, web uygulamalarının performansını ölçmek için kullanılan tarayıcının geliştirici konsolunda çalışan bir araçtır. **Google** tarafından geliştirilmiştir. **Lighthouse**'un sunduğu raporlar sayesinde web uygulamalarının performansını ölçebilir ve performansı artırmak için neler yapmanız gerektiğini öğrenebilirsiniz.
- Dikkat edilmesi gereken ilk şey Lighthouse çalıştırılırken geliştirici konsolunun ekran boyutunu küçültecek şekilde tarayıcıya bağlı olmamasıdır. Küçük ekranda çalıştırılan Lighthouse, o boyuttaki bir sayfanın performansını ölçer.
- Alınan raporlardaki metrikler kullanılan bilgisayar ve internet'in performansına göre değişiklik gösterebilir. Bu yüzden raporları alırken farklı cihazlarda ve farklı internet hızlarında da test etmek faydalı olacaktır.

## 2. Metrikler

### Performans Nerede Ölçülür?

#### Laboratuvar Ortamında

Yukarıda bahsedilen **Web Vitals** testleri aslında laboratuvar ortamında yapılan bir testtir. Bu testler, kullanıcıların gerçek dünyada yaşadığı deneyimi tam olarak yansıtmaz.

**Lighthouse**'un sunduğu raporlar da laboratuvar ortamında yapılan testlerin sonuçlarıdır.

#### Test Sunucuları Üzerinde

Gerçek kullanıcıya ihtiyaç duyulmadan yapılabilecek diğer bir test ise yapay veri (*synthetic data*) kullanılarak yapılan testlerdir.

Bu testlerde kullanıcıların gerçek dünyada yaşadığı deneyimi tam olarak yansıtmaz fakat laboratuvar ortamında yapılan testlerden daha gerçekçi sonuçlar verir. Bu testler ***APM (Application Performance Monitoring) Tool***'ları ile yapılabilir.

Örnek olarak **New Relic**, **Pingdom**, **Datadog** ve **Dynatrace** bu servisi sunmakta.

#### Gerçek Kullanıcıların Cihazlarında

Gerçek kullanıcılarla yapılan testler ise **Real User Monitoring (RUM)** ile yapılır. Bu testlerde kullanıcıların gerçek dünyada yaşadığı deneyim tam olarak yansıtılır. Bu testlerin sonuçları laboratuvar ortamında yapılan testlerden ve yapay verilerle yapılan testlerden daha gerçekçi sonuçlar verir.

Bu testlerde kullanıcılardan alınan verilerin konum, cihaz, tarayıcı, internet hızı gibi bilgileri dikkate alınarak sınıflandırılması gerekmektedir. Bu sınıflandırmalar hedef kitlenizi belirlemenizde ve kitlenize uygun çözümler sunabilmenizde size yardımcı olacaktır.

> **Google**'ın sağladığı [**web.dev**](https://web.dev/) adresinden, web performansı ve **Core Web Vitals** ile ilgili bir çok bilgiye ulaşabilirsiniz.

### Verileri Yorumlamak

Performans veri setinizi yorumlarken önemli olan kriter ortalamalar değil, dağılımlardır. Yani skorlarınızın ortalamasının iyi olması, kullanıcılarınızın genelinin deneyiminin iyi olduğu anlamına gelmez. Önemli olan, skorlarınızın dağılımının iyi olmasıdır.

> Örneğin kullanıcılarınızın çeyreği çok iyi bir performans gözlemlerken kalanı yeterli bir performans gözlemleyemiyorsa, ortalama değeriniz yüksek olduğu halde kullanıcılarınızın çoğunluğu iyi bir deneyim yaşamıyor demektir.  

Burada medyan devreye girer. **Medyan (p50)**, veri setindeki **orta değeri** ifade eder. Yani veri setinizdeki değerlerin yarısı medyan değerinden küçük, yarısı medyan değerinden büyüktür. Google'ın dikkat ettiği yer ise %75'lik dilimdir. Veri setinizin %75'lik dilimi iyi bir performans gösteriyorsa, kullanıcılarınızın çoğunluğunun iyi bir deneyim yaşadığını söyleyebilirsiniz.

> %95'lik dilimin dışında kalan veriler en kötü deneyimi yaşayan %5'lik dilimi temsil eder. Bu dilimdeki kullanıcılarınızın deneyimini iyileştirmek çok zordur. Bu dilimi hedef almak mantıklı bir strateji olmayabilir.

## 3. Performansı İyileştirmek

### Web İşletmesinin Hedefi

1. **Farkındalık**: Neler yaptığınız ve ürünlerinizin ne olduğu hakkında bilgi vermek.
2. **Muhafaza**: Kullanıcılarınızı yakında tutarak ürünlerinizin kullanımını artırmak.
3. **Dönüşüm**: Yeni kullanıcılar kazanmak ve mevcut etkileşimleri gelire dönüştürmek.
4. **Rekabet**: Rakiplerinizden iyi olmak.

> Ancak %20 oranında performans farkı kullanıcılar tarafından fark edilebilir. Yeterli performans sağlandıktan sonra daha fazla performans iyileştirmeleri yapmak yerine, başka alanlarda iyileştirmeler yapmak daha mantıklı bir karar olacaktır.

### Performance API

**Performance API**, tarayıcıların performans verilerine erişmek için kullanılan bir API'dir. Bu API sayesinde tarayıcıların performans verilerine erişebilir ve bu verileri kullanarak performansı ölçebilirsiniz.

> MDN'de [**Performance API**](https://developer.mozilla.org/en-US/docs/Web/API/Performance) hakkında daha fazla bilgi bulabilirsiniz.

```javascript
const performance = window.performance;

const navigationTiming = performance.getEntries();
console.log(navigationTiming);
```

```json
[
  ...
  {
    "connectEnd": 163.22,
    "connectStart": 75.35,
    "domComplete": 650.51,
    "domContentLoadedEventEnd": 531.47,
    "domContentLoadedEventStart": 529.13,
    "domInteractive": 497.93,
    "domainLookupEnd": 75.35,
    "domainLookupStart": 20.75,
    "duration": 650.51,
    "entryType": "navigation",
    "fetchStart": 4.37,
    "loadEventEnd": 650.51,
    "loadEventStart": 650.50,
    "name": "http://localhost:3000/",
    "redirectEnd": 0,
    "redirectStart": 0,
    "requestStart": 163.22,
    "responseEnd": 213.92,
    "responseStart": 211.62,
    "secureConnectionStart": 120.98,
    "type": "navigate"
  },
  ...
]
```

> **Performance API**'nin sunduğu veriler, tarayıcıların performans verileri olduğu için tarayıcıya bağlı olarak değişiklik gösterebilir.

### Bir HTTP Request Diyagramı

![Diagram of a HTTP Request](https://developer.mozilla.org/en-US/docs/Learn/Performance/Measuring_performance/navigationtimingapi.jpg)

## 4. Metriklerin İyileştirilmesi

### FCP (First Contentful Paint)

Bir web sayfası açıldığında sunucudan kullanıcıya gitmesi gereken bir takım dokümanlar var. FCP'nin ölçümü, bu dokümanların kullanıcıya ulaşma süresini ölçer. Bu sürenin kısa olması için neler gerekli?

- Sunucunun hızlı olması
- Sunucunun kullanıcıya yakın olması
- Gönderilen dokümanların boyutunun küçük olması

Bu maddeler nasıl sağlanır?

#### Sunucunun Hızlı Olması

- Sunucunun fiziksel özelliklerini *istisnai durumlara hazırlıklı olarak* web uygulamasının ihtiyacına göre yapılandırın.

> Pizza taşımak için tır, masa taşımak için bisiklet kullanmayın. İhtiyaçlarınıza uygun bir sunucu tercih edin.

- Sunucunuzda gereksiz işlemler gerçekleştirmeyin.

> Yapmanız için bir gereklilik yoksa basit bir `index.html` dosyası için veri tabanına istek atmak zorunda değilsiniz.

#### Küçük Dokümanlar

- Javascript ve CSS dosyalarınızın boyutlarını **CSS minify** ve **JS Minify** araçları kullanarak küçültün.

> **Rollup**, **Webpack**, **Turbopack** ve **TailwindCSS** gibi araçlar, bu işlemi otomatik olarak yapar.

- Dosyalarınızı **gzip** gibi sıkıştırma araçları kullanarak sıkıştırın.

> **Nginx** ve **Apache** gibi sunucuların ayarlarını configure ederek bu işlemi otomatik olarak yapılabilir.

- Resimlerinizi **WebP** formatına dönüştürün.

#### Sunucunun Kullanıcıya Yakın Olması

- Uygulamanın Dünya'nın farklı yerlerinden erişmeye çalışan kullanıcılarınız varsa, **CDN (Content Delivery Network)** kullanarak sunucunun kullanıcıya yakın olmasını sağlayın.

> **CDN**'ler, sunucunuzda bulunan dokümanları dünya üzerinde farklı noktalardaki sunuculara kopyalayarak, kullanıcının dokümanlara daha hızlı ulaşmasını sağlar.
> Örnek olarak **Cloudflare**, **Fastly** ve **Akamai** gibi servisler kullanılabilir.