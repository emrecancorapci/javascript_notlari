# 1. Performansın Temelleri

## Performans Neden Önemli?

- Çünkü **Google** öyle diyor.
- Ne kadar **hızlı** olursa o kadar **iyi bir kullanıcı deneyimi** sunar. Ne kadar **iyi kullanıcı deneyimi** sunarsa o kadar **çok kullanıcıya** ulaşır. Ne kadar **çok kullanıcıya** ulaşırsa o kadar **çok para** kazanır.

## Beklemenin Psikolojisi

- Beklemek göreceli bir kavramdır. Bekleme süresi kullanıcıdan kullanıcıya değişir.
- Beklemeyi göze aldığımız zaman, beklentilerimiz ve beklerken ne yapabildiğimize göre değişir.

### İnsanlar bir an önce başlamak ister

Eğer erken bir başlangıç verirseniz bekleme süresinin kısa olduğunu düşünürler. Ama başlamak istedikleri halde onlara bir şey vermezseniz bekleme süresi yavaş hissettirir.

### Sıkıcı şeyler daha yavaş hissettirir

Eğer kullanıcının yapacak veya bakacak herhangi bir şeyi yoksa bekleme süresi normalden daha uzun hissedilir.

### Gerginlik daha yavaş hissettirir

Eğer bir test sonucunu bekliyorsanız, başvurduğunuz kredinin onaylanıp onaylanmadığını bekliyorsanız, bekleme süresi daha uzun hissettirir.

### Anlamsız bekleme daha yavaş hissettirir

Eğer neden beklediğinizi bilmiyorsanız veya kimse neden beklediğinizle ilgili bir cevap veremiyorsa bekleme süresi daha uzun hissettirir.

### Ne kadar bekleyeceğini bilmeme daha yavaş hissettirir

Eğer ne kadar bekleyeceğinizi bilmiyorsanız, yaptığınız işlemin ne kadar süreceği hakkında bir tahmininiz de yoksa bekleme süresi daha uzun hissettirir.

### İnsanlar değerli olan şeyler için beklemeye daha isteklidir

Eğer beklediğiniz şeyin değerli olduğunu düşünüyorsanız, beklemeye daha istekli olursunuz. Yeni ürün çıktığında Apple mağazalarında kuyruğa girilmesinin sebebi de budur. Kimse değer vermediği bir şey için beklemek istemez.

## Web Vitals

### Eski Metrikler

- Eskiden performansı ölçmek için tek bir metrik kullanılırdı: **Page Load Time**. Ama bu metrik, kullanıcı deneyimini tam olarak yansıtmıyordu. Çünkü sayfa yüklendikten sonra da kullanıcılar sayfa üzerinde etkileşimde bulunarak sayfayı değiştirebilirler.
- *React*, *Vue* gibi **Single Page Application**'lar sayfa yüklendikten sonra sayfa içerisindeki içerikleri dinamik olarak değiştirebilirler. Bunun yanında kullanıcıya ilk gönderilen HTML sayfası sadece boş beyaz bir sayfadan ibarettir. Bu yüzden **Page Load Time** metriği bu tür uygulamalarda kullanıcı deneyimini tam olarak yansıtamaz.
- Bu yüzden günümüzde bu metrik terk edilerek yerine **Google**'ın geliştirdiği **Web Vitals** metrikleri kullanılmaya başlandı.

### Web Vitals Metrikleri
