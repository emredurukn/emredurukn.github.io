---
layout: post
title: TensorFlow ile MNIST Örneği
published: true
date: 2016-11-09
tags:
  - tensorflow
  - deep learning
image: tensorflow.jpg
description: TensorFlow ile örnek uygulamalar
---


Bu yazıya başlamadan önce [TensorFlow ile Derin Öğrenmeye Giriş](https://emredurukn.github.io/2016/11/02/tensorflow-ile-derin-ogrenmeye-giris.html) adlı yazıma göz atarsanız bu yazıyı anlamanız daha kolay olacaktır.


Bu yazımda bir modeli rakamların olduğu resimlerden birine bakarak resimde hangi rakamın olduğunu tahmin edebilmesi için eğiticez. Sonrasında ise ne oranda doğru tahminde bulunduğunu test edicez. Bu giriş seviyesi bir model olucak bu yüzden çok yüksek doğruluk oranlarına ulaşamayacak ama işin temelini anlamak için çok iyi bir örnektir aynı zamanda. Modeli eğitmek için MNIST adında bir veri setini kullanacağız.


### MNIST nedir?

MNIST, görüntüyü işlemek ve anlamlandırmak için kullanılan bir veri setidir. Bu veri seti el yazısıyla yazılmış rakamların resimlerinden oluşur.

<center>
	<amp-img width="640" height="360" layout="responsive" src="/assets/images/mnist-examples.png"></amp-img>
</center>

İlk olarak TensorFlow'u ekleyelim. Ardından input_data sınıfı ile MNIST veri setini ekleyelim. 

```python
import tensorflow as tf
from tensorflow.examples.tutorials.mnist import input_data

mnist = input_data.read_data_sets("/tmp/data/", one_hot=True) 
```

MNIST veri setindeki her bir resim 28x28 piksel olduğundan bu resimlerlerin içerisinde 28 x 28 = 784 piksel bulunmaktadır. MNIST veri setindeki resimleri giriş olarak almak için TensorFlow'da giriş için kullanılan **placeholder**'ı 784 pikseli ifade edecek şekilde vektörleştirerek tanımladık. Bu tanımda Matrisin satır sayısındaki **None** herhangi bir değer olabilir anlamındadır. Bunu kullanmamızın nedeni dinamik bir yapı oluşturmak.

Vektörleştirmek temel olarak birden fazla boyutlu matrisin (MNIST için 2 Boyutlu (28x28) tek boyuta indirgenmesi vektör şeklinde ifade edilmesi diyebiliriz.


Resimlerde bulunan rakamları tespit edebilmek için bir sınıflandırıcı tanımlamalıyız. Bu örnekte Lojistik Sınıflandırıcı (**Logistic Classifier**) kullanıcağız. Lojistik Sınıflandırıcı girişten aldığı resmi **Weights** ile çarpıp **Bias** ile toplayarak hangi sınıfa dahil olduğuna dair skorlar belirliyor. Tüm bu işlemler matris formunda gerçekleşiyor. ( [Nonex784] * [784x10] = [Nonex10])


Lojistik Sınıflandırıcı sonucu elde edilen skorlar Softmax fonksiyonu ile resimde hangi rakamın olduğuna dair ihtimallere dönüştürülüyor.


```python
x = tf.placeholder(tf.float32, [None, 784])	# Input

W = tf.Variable(tf.zeros([784, 10]))		# Weight
b = tf.Variable(tf.zeros([10]))			# Bias

y = tf.nn.softmax(tf.matmul(x, W) + b)  # Softmax(Logistic Classifier)
```

**Cost** modelin istenen sonuca ne kadar uzak olduğunu gösterir. Modelin Cost değerinin belirlenmesi için **Cross-entropy** adında bir fonksiyon kullanacağız. Cross-entropy birçok alanda kullanılan çok önemli bir kavram merak edenler [bu siteye](http://colah.github.io/posts/2015-09-Visual-Information/) göz atabilir.

Bir giriş değişkeni (**placeholder**) tanımladık ve Cross-entropy fonksiyonunu bu değişkene uyguladık.

Eğitim aşamasında daha iyi bir model ortaya çıkarmak için daha küçük hata payı elde etmeye çalışırız. Bu nedenle Cross-entropy fonksiyonundan elde ettiğimiz değeri optimize etmek için [Gradient Descent algoritmasını](https://en.wikipedia.org/wiki/Gradient_descent) kullanan **GradientDescentOptimizer** fonksiyonunu learning rate parametresini 0.5 vererek kullandık.

```python
y_ = tf.placeholder(tf.float32, [None, 10])

cross_entropy = tf.reduce_mean(-tf.reduce_sum(y_ * tf.log(y), reduction_indices=[1]))

train_step = tf.train.GradientDescentOptimizer(0.5).minimize(cross_entropy)
```

Oluşturduğumuz değişkenlere ilk değer ataması yapmak için bir işlem (init) tanımladık. Ardından **Session** tanımlayıp değişkenlere ilk değer atamasını gerçekleştirdik. Ardından öğrenme adımını for döngüsü ile 1000 defa gerçekleştirdik. 

```python
init = tf.initialize_all_variables()

sess = tf.Session()
sess.run(init)

for i in range(1000):
  batch_xs, batch_ys = mnist.train.next_batch(100)
  sess.run(train_step, feed_dict={x: batch_xs, y_: batch_ys})
```

Sıra modelimizin hangi oranda doğru tahmin yaptığını belirlemeye geldi. **tf.argmax(y,1)** ifadesi modelimizin yaptığı tahminleri içeren katmanı ifade eder. **tf.argmax(y_,1)** ifadesi ise gerçek sonuçları içerien katmanı ifade eder. Bu iki katmanı karşılaştırmak için equal fonksiyonunu kullandık. Bu işlem sonucunda örnek olarak,

[True, False, True, True] şeklinde bir sonuç elde ettik.

Doğru tahmin oranını belirleyebilmek için equal fonskiyonu ile elde edilen boolean değerleri cast fonksiyonuyla float'a dönüştürtük. Bu işlem sonucunda ise örnek olarak,

[1, 0, 1, 1] şeklinde bir sonuç elde ettik.

Ardından reduce_mean fonskiyonuyla doğru tahmin oranını (accuracy) belirledik.

Son olarak elde ettiğimiz accuracy değerini print fonksiyonuyla ekrana yazdırdık.

```python
correct_prediction = tf.equal(tf.argmax(y,1), tf.argmax(y_,1))

accuracy = tf.reduce_mean(tf.cast(correct_prediction, tf.float32))

print(sess.run(accuracy, feed_dict={x: mnist.test.images, y_: mnist.test.labels}))
```
