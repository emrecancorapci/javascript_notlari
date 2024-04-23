# Giriş

## Genel Bakış

### Makine Öğrenmesi Nedir?

Bilgisayarlara açıkça programlanmadan öğrenme yeteneği kazandırmak için algoritmalara ve verilere odaklanan Yapay Zeka dalı.

### ML vs AI

**Makine öğrenimi**, bir makineye belirli bir *görevi nasıl gerçekleştireceğini ve doğru sonuçlar üreteceğini* öğretmeye odaklanır.

**Yapay zeka**, *insan zekasını taklit etmeye odaklanan* daha geniş bir şemsiyedir. Yapay zekanın alt alanları:

- Makine öğrenme
- Derin öğrenme (yapay sinir ağlarını kullanan makine öğreniminin alt kümesi)
- Bilgisayar görüşü
- Doğal Dil İşleme

### Terminoloji

- Model: Bir girdi alan, hesaplamaları çalıştıran ve olasılıklarla birlikte bir çıktı üreten bir fonksiyon.
- Machine Learning Algorithms: Convolutional Neural Network (CNN), Naive Bayes, Long Short Term Memory (LSTM), ...
- Weights: Belirli bir görevi gerçekleştirmek için modelin doğruluğunu artırmak amacıyla veri kümesinin her bir özelliğine ne kadar önem verilmesi gerektiği. Bu ağırlıklar, model veriler üzerinde eğitildikçe ve doğruluk değiştikçe ayarlanır. Öğrenilen özellikler.
- Overfitting: Modelin eğitim veri setine **çok yakın uyması** ve dolayısıyla yeni verileri doğru bir şekilde tahmin edememesi.
- Classes or Labels: Tek bir veri parçasının tek kelimelik açıklaması. Örneğin evcil hayvan görselleri için "köpek", "kedi".

### Makine Öğrenme Türleri

#### Supervised learning (*Denetimli öğrenme*)

Denetimli öğrenmede bilgisayara etiketli bir veri seti verilir. Algoritma ise veriler ile etiketler arasında bir korelasyon bulmaya çalışır. Örneğin, bir resimdeki bir köpeği tanımlamak için bir algoritma eğitirken, resimler köpekler ve kediler olarak etiketlenmiş olarak verilir.

Algoritma, resimlerdeki köpeklerin ve kedilerin farklı özelliklerini öğrenir ve bu özellikleri kullanarak yeni resimlerdeki köpekleri ve kedileri tanımlamaya çalışır.

#### Unsupervised learning (*Denetimsiz öğrenme*)

Denetimsiz öğrenmede veriler etiketlenmeden makine öğrenmesi algoritmasına sunulur. Algoritma, veriler arasındaki ilişkileri ve yapıları keşfetmeye çalışır. Verileri gruplandırabilir, boyut azaltabilir, yoğunluk tahminleri yapabilir.

#### Semi-supervised learning (*Yarı denetimli öğrenme*)

Denetimli ve denetimsiz öğrenmeyi birleştirir. Verilerin bir kısmı etiketlenmiş, bir kısmı etiketlenmemiş. Faydaları? Daha yüksek doğruluk ve verimlilik elde edebilir.

#### Reinforcement learning (*Takviyeli öğrenme*)

Takviyeli öğrenmede eğitim yöntemi, istenilen davranışın ödüllendirilmesi, istenmeyen davranışın ise cezalandırılması esasına dayanır. Deneme yanılma yoluyla öğrenme. Bir ajanı eğittiğiniz bir ortamın olduğu ve bu ortamda aksiyon alınabildiği oyunlarda daha kullanışlıdır.

### Araçlar

- [TensorFlow.js](https://www.tensorflow.org/js) - Kullanılacak kütüphane
- [ML5.js](https://ml5js.org/) - Tensorflow.js üzerine inşa edilmiş daha kolay bir kütüphane
- [Brain.js](https://brain.js.org/)
- [ConvNetJS](http://cs.stanford.edu/people/karpathy/convnetjs/)
- [Keras.js](https://transcranial.github.io/keras-js/)

## Pre-trained Models

### Pre-trained Models Nedir?

Önceden eğitilmiş bir model, önceden eğitilmiş ve test edilmiş, uygulamanızda kullanılmaya hazır bir modeldir. Bu modeller, genellikle büyük veri setleri üzerinde eğitilmiş ve spesifik bir görevi yerine getirmek için kullanılabilir.

Bu modeller; image recognition (*görüntü tanıma*) modelleri, text classification (*metin sınıflandırma*) modelleri, sentiment analysis (*duygu analizi*) modelleri ve daha fazlası olabilir.

Bazı modeller Tensorflow.js'le uyumlu olabilir ve bu modelleri kullanarak kendi projelerinizi oluşturabilirsiniz. Bazı modeller ise Tensorflow.js ile uyumlu olmayabilir ve bu modelleri kullanarak kendi projelerinizi oluşturmak için Tensorflow.js'e dönüştürmeniz gerekebilir. Dönüşüm için çeşitli araçlar kullanılması hatta bazı durumlarda python yazılması gerekebilir.

### Nereden Bulunanabilir?

- [Kaggle](https://www.kaggle.com/models) hatta daha spesifik olarak [Tensorflow.js Modelleri](https://www.kaggle.com/search?q=tensorflow.js+in%3Amodels)
- [Github](https://github.com/tensorflow/tfjs-models)
- [Hugging Face](https://huggingface.co/models)
