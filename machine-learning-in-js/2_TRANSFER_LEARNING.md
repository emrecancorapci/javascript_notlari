# 2. Transfer Learning

## Transfer Learning Nedir?

Önceden eğitilmiş bir modeli yeni bir görevde yeniden kullanan bir tekniktir. Örneğin, eğer bir görüntü sınıflandırma modeli köpekleri tanıyacak şekilde eğitilmişse bu görevden elde edilen bilgi, balinaları tanımlamaya çalışmak için kullanılabilir.

Transfer öğrenme, genellikle daha az veri ile daha iyi sonuçlar elde etmek için kullanılır. Önceden eğitilmiş bir model, genellikle büyük veri setleri üzerinde eğitilmiş ve genel bir görevi yerine getirmek için kullanılabilir. Bu modeli, yeni bir görevde kullanarak, modelin öğrendiği özellikleri ve ağırlıkları kullanabiliriz.

[Teachable Machine](https://teachablemachine.withgoogle.com/) gibi araçlar, transfer öğrenme tekniklerini kullanarak, önceden eğitilmiş modelleri yeni görevlerde kullanmamıza olanak tanır.

## Terminoloji

### Epochs (Dönemler)

Yinelemeler veya iterasyon sayısı. Eğitim verilerinin makine öğrenimi algoritmasından tamamen geçirilmesi.

> #### Model Training (Model Eğitimi)
>
> Bir makine öğrenme modeli eğitilirken tüm veri seti tek seferde kullanılarak sonuç alınamaz. Makine öğrenmesi modellerinin birden çok katmanı vardır ve her bir katmanın ağırlıkları, veri seti üzerinde birden çok kez eğitilir.

### Activation Function (Aktivasyon Fonksiyonu)

Ağın verilerdeki karmaşık kalıpları öğrenmesine yardımcı olmak için yapay sinir ağına eklenen bir fonksiyon.

> Farklı tiplerde bir çok aktivasyon fonksiyonu var fakat hepsini bilmek gerekli değildir.

### Batch Size

Kullanılan eğitim örneklerinin sayısı. Büyük batch size'lar daha hızlı eğitim sürelerine yol açabilir fakat daha az doğruluk sağlayabilir. Küçük batch size'lar ise daha uzun eğitim sürelerine yol açabilir fakat daha iyi doğruluk sağlayabilir.

> #### Batch
>
> Eğitim veri seti çok büyük olduğunda, tüm veri setini aynı anda kullanmak yerine, veri setini küçük parçalara böleriz. Bu parçalara "batch" denir. Batch size, her bir eğitim adımında kullanılan örnek sayısını belirler.

### Learning Rate (Öğrenme Oranı)

Ağırlıklar her güncellendiğinde modelin ne kadar değişmesi gerektiğini kontrol eden hiperparametre.

> #### Hyperparameters (Hiperparametreler)
>
> Hiperparametreler, modelin eğitim sürecini kontrol eden parametrelerdir. Model eğitilirken, hiperparametrelerin değerleri değiştirilerek, model üzerinde farklı sonuçlar elde edilebilir.
>
> #### Hyperparameter Tuning (Hiperparametre Ayarı)
>
> Hiperparametrelerin sabit bir değeri yoktur. Modelden elde edilen sonuçlara göre hiperparametrelerde değişiklikler yapılarak model istenilen sonucu vermesi için yönlendirilir. ***Hiperparametre ayarı*** yapmak, modelin doğruluğunu ve performansını artırabilir.
>
> #### Weights (Ağırlıklar)
>
> Modelin doğruluğunu artırmak amacıyla veri kümesinin her bir özelliğine ne kadar önem verilmesi gerektiği. Bu ağırlıklar, model veriler üzerinde eğitildikçe ve doğruluk değiştikçe ayarlanır.

### Tensor

Fiziksel nicelikleri tanımlamak için kullanılan bir matematiksel nesne. Tensor, bir vektörün genelleştirilmiş bir hali olarak düşünülebilir. Spesifik olarak makine öğrenmesinde kullanılan bir veri tipi yada yapısı olarak düşünülebilir.

### Optimizer (İyileştirici)

Sinir ağının ağırlıklar ve öğrenme oranları gibi özelliklerini ayarlayan bir fonksiyon veya algoritma.
