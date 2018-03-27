---
layout: post
title: TensorFlow ile MNIST Örneği
published: true
date: 2016-11-09
tags:
  - tensorflow
  - deep learning
  - mnist
image: tensorflow.jpg
description: TensorFlow ile Mnist örnek uygulaması
---


Bu yazıya başlamadan önce [TensorFlow ile Derin Öğrenmeye Giriş](https://emredurukn.github.io/2016/11/02/tensorflow-ile-derin-ogrenmeye-giris.html){:target="_blank"} adlı yazıma göz atarsanız bu yazıyı anlamanız daha kolay olacaktır.


Bu yazımda bir modeli rakamların olduğu resimlerden birine bakarak resimde hangi rakamın olduğunu tahmin edebilmesi için eğiteceğiz. Sonrasında ise ne oranda doğru tahminde bulunduğunu test edeceğiz. Bu giriş seviyesi bir model olucak bu yüzden çok yüksek doğruluk oranlarına ulaşamayacak ama işin temelini anlamak için çok iyi bir örnektir aynı zamanda. Modeli eğitmek için MNIST adında bir veri setini kullanacağız.


### MNIST nedir?

[MNIST](https://en.wikipedia.org/wiki/MNIST_database){:target="_blank"}, görüntüyü işlemek ve anlamlandırmak için kullanılan bir veri setidir. Bu veri seti el yazısıyla yazılmış rakamların resimlerinden oluşur.

<center>
	<amp-img width="640" height="360" alt="MNIST" layout="responsive" src="/assets/images/mnist-examples.png"></amp-img>
</center>

İlk olarak TensorFlow'u ekleyelim. Ardından input_data sınıfı ile MNIST veri setini ekleyelim. 

<amp-gist data-gistid="b45773fc81ff92d366d09d11e3038492"
  layout="fixed-height"
  height="450">
</amp-gist>

MNIST veri setindeki her bir resim 28x28 piksel olduğundan bu resimlerlerin içerisinde 28 x 28 = 784 piksel bulunmaktadır. MNIST veri setindeki resimleri giriş olarak almak için TensorFlow'da giriş için kullanılan **placeholder**'ı 784 pikseli ifade edecek şekilde vektörleştirerek tanımladık. Bu tanımda Matrisin satır sayısındaki **None** herhangi bir değer olabilir anlamındadır. Bunu kullanmamızın nedeni dinamik bir yapı oluşturmak.

Vektörleştirmek temel olarak birden fazla boyutlu matrisin (MNIST için 2 Boyutlu (28x28) tek boyuta indirgenmesi vektör şeklinde ifade edilmesi diyebiliriz.


Resimlerde bulunan rakamları tespit edebilmek için bir sınıflandırıcı tanımlamalıyız. Bu örnekte Lojistik Sınıflandırıcı (**Logistic Classifier**) kullanıcağız. Lojistik Sınıflandırıcı girişten aldığı resmi **Weights** ile çarpıp **Bias** ile toplayarak hangi sınıfa dahil olduğuna dair skorlar belirliyor. Tüm bu işlemler matris formunda gerçekleşiyor. ( [Nonex784] * [784x10] = [Nonex10])


Lojistik Sınıflandırıcı sonucu elde edilen skorlar Softmax fonksiyonu ile resimde hangi rakamın olduğuna dair ihtimallere dönüştürülüyor.

<amp-gist data-gistid="b3243e822df779fa8b4e024d596229d5"
  layout="fixed-height"
  height="450">
</amp-gist>

**Cost** modelin istenen sonuca ne kadar uzak olduğunu gösterir. Modelin Cost değerinin belirlenmesi için **Cross-entropy** adında bir fonksiyon kullanacağız. Cross-entropy birçok alanda kullanılan çok önemli bir kavram merak edenler [bu siteye](http://colah.github.io/posts/2015-09-Visual-Information/){:target="_blank"} göz atabilir.

Bir giriş değişkeni (**placeholder**) tanımladık ve Cross-entropy fonksiyonunu bu değişkene uyguladık.

Eğitim aşamasında daha iyi bir model ortaya çıkarmak için daha küçük hata payı elde etmeye çalışırız. Bu nedenle Cross-entropy fonksiyonundan elde ettiğimiz değeri optimize etmek için [Gradient Descent algoritmasını](https://en.wikipedia.org/wiki/Gradient_descent){:target="_blank"} kullanan **GradientDescentOptimizer** fonksiyonunu learning rate parametresini 0.5 vererek kullandık.

<amp-gist data-gistid="45d42a359c43c9666cd928f94647ff74"
  layout="fixed-height"
  height="450">
</amp-gist>

Oluşturduğumuz değişkenlere ilk değer ataması yapmak için bir işlem (init) tanımladık. Ardından **Session** tanımlayıp değişkenlere ilk değer atamasını gerçekleştirdik. Ardından öğrenme adımını for döngüsü ile 1000 defa gerçekleştirdik. 

<amp-gist data-gistid="2b7e3a3f5051ca85a850d13e5d6e7492"
  layout="fixed-height"
  height="450">
</amp-gist>

Sıra modelimizin hangi oranda doğru tahmin yaptığını belirlemeye geldi. **tf.argmax(y,1)** ifadesi modelimizin yaptığı tahminleri içeren katmanı ifade eder. **tf.argmax(y_,1)** ifadesi ise gerçek sonuçları içerien katmanı ifade eder. Bu iki katmanı karşılaştırmak için equal fonksiyonunu kullandık. Bu işlem sonucunda örnek olarak,

[True, False, True, True] şeklinde bir sonuç elde ettik.

Doğru tahmin oranını belirleyebilmek için equal fonskiyonu ile elde edilen boolean değerleri cast fonksiyonuyla float'a dönüştürtük. Bu işlem sonucunda ise örnek olarak,

[1, 0, 1, 1] şeklinde bir sonuç elde ettik.

Ardından reduce_mean fonskiyonuyla doğru tahmin oranını (accuracy) belirledik.

Son olarak elde ettiğimiz accuracy değerini print fonksiyonuyla ekrana yazdırdık.

<amp-gist data-gistid="64302a414decdaef265993f3eb7e404f"
  layout="fixed-height"
  height="450">
</amp-gist>