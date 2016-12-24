---
layout: post
title: TensorFlow ile Derin Öğrenmeye Giriş
published: true
date: 2016-11-02
tags: tensorflow deep learning
image: tensorflow.jpg
description: TensorFlow kurulum adımları ve kısa bir giriş
---


[TensorFlow](https://www.tensorflow.org/), Google'ın açık kaynak kodlu makina öğrenmesi kütüphanesi ve özellikle derin öğrenme için kullanılıyor.

<center>
	<amp-img width="640" height="360" layout="responsive" src="/assets/images/tensorflow.jpg"></amp-img>
</center>


### Derin öğrenme nedir?

Derin öğrenme, sinir ağlarına dayanıyor. Kısacası, bu ağlara büyük miktarda veri veriyorsunuz, onlar da bir görevi gerçekleştirmeyi öğreniyorlar. Bir sürü kahvaltı, öğle yemeği ve akşam yemeği fotoğrafları verirseniz, bir öğünü ayırt etmeyi öğrenebilirler.

Sözlü kelimeler verirseniz, ne dediğinizi ayırt etmeyi öğrenebilirler. Eski filmlerden biraz diyalog verirseniz, bir sohbet sürdürmeyi öğrenebilirler. (Kusursuz bir sohbet değil ama yine de oldukça iyi bir sohbet.)

Derin öğrenme konusunda daha fazla bilgi için [bu siteye](http://www.derinogrenme.com/2015/07/21/derin-ogrenme-deep-learning-nedir/) bakabilirsiniz.


Bu yazımda Ubuntu 16.04 üzerinde TensorFlow'a kısa bir başlangıç yapmaya çalışacağım.

TensorFlow dahil birçok makine öğrenmesi kütüphanesi ile geliştirilen projelerde genelde Python tercih ediliyor. Neden Python diye sorarsak hızlı geliştirme olanağı ve topluluk desteği diyebiliriz. 


İlk olarak Python kurulumunu gerçekleştirelim.

```bash
$ sudo apt-get install python3-pip python3-dev -y
```

Daha sonra CPU versiyonunu kurmak için gerekli kodu girelim.

```bash
$ export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/linux/cpu/tensorflow-0.12.0-cp35-cp35m-linux_x86_64.whl
```

Son olarak TensorFlow kurulumunu başlatalım.

```bash
$ sudo pip3 install --upgrade $TF_BINARY_URL
```


### Hello world ile TensorFlow'a bir giriş yapalım.

```python
import tensorflow as tf

hello = tf.constant('Hello World')

with tf.Session() as sess:
  print(sess.run(hello)) 
```

TensorFlow'da bazı işlemlerde kullanılmak üzeri sabit olarak **constant** kullanılıyor. Çıktının gösterilmesi aşamasında ise **Session** yapısı kullanılıyor. 


### Şimdi de TensorFlow ile basit işlemler gerçekleştirelim.

```python
import tensorflow as tf

x1 = tf.constant(5)
x2 = tf.constant(6)

# Addition, Multiplication and Subtraction
add = tf.add(x1, x2)
sub = tf.sub(x1, x2)
mul = tf.mul(x1, x2)

# 1x2 Matrix
matrix1 = tf.constant(
	[
		[3., 4.]
	]
)

# 2x1 Matrix
matrix2 = tf.constant(
	[
		[2.],
		[2.]
	]
)

# Matrix Multiplication
product = tf.matmul(matrix1, matrix2)

# Linearly spaced vector
vector = tf.linspace(-3.0, 7.0, 6)

with tf.Session() as sess:
    print(sess.run(add))
    print(sess.run(sub))
    print(sess.run(mul))
	
    print(sess.run(product)) # 1x2 * 2x1 = 1x1 Matrix
    
    print(sess.run(vector)) 
```

 İlk olarak **x1** ve **x2** adında iki sabit tanımlayıp ardından toplama, çıkarma ve çarpma işlemleri için TensorFlow'da tanımlı olan fonksiyonları (**add**, **sub**, **mul**) kullandık.

 Daha sonra matris çarpımı için **matmul** fonksiyonunu, doğrusal aralıklı bir vektör dizisi oluşturmak için de **linspace** fonksiyonunu kullandık.

 Son olarak bu işlemlerin sonuçlarını atadığımız değişkenleri **Session** içerisinde **print** fonksiyonuyla ekrana yazdırdık.
