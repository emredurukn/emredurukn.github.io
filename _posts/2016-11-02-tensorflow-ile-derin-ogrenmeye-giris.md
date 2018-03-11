---
layout: post
title: TensorFlow ile Derin Öğrenmeye Giriş
published: true
date: 2016-11-02
tags: 
 - tensorflow
 - deep learning
image: tensorflow.jpg
description: TensorFlow kurulum adımları ve kısa bir giriş
---


[TensorFlow](https://www.tensorflow.org/){:target="_blank"}, Google'ın açık kaynak kodlu makina öğrenmesi kütüphanesi ve özellikle derin öğrenme için kullanılıyor.

<center>
	<amp-img width="640" height="360" alt="TensorFlow" layout="responsive" src="/assets/images/tensorflow.jpg"></amp-img>
</center>


### Derin öğrenme nedir?

Derin öğrenme, sinir ağlarına dayanıyor. Kısacası, bu ağlara büyük miktarda veri veriyorsunuz, onlar da bir görevi gerçekleştirmeyi öğreniyorlar. Bir sürü kahvaltı, öğle yemeği ve akşam yemeği fotoğrafları verirseniz, bir öğünü ayırt etmeyi öğrenebilirler.

Sözlü kelimeler verirseniz, ne dediğinizi ayırt etmeyi öğrenebilirler. Eski filmlerden biraz diyalog verirseniz, bir sohbet sürdürmeyi öğrenebilirler. (Kusursuz bir sohbet değil ama yine de oldukça iyi bir sohbet.)

Derin öğrenme konusunda daha fazla bilgi için [bu siteye](http://www.derinogrenme.com/2015/07/21/derin-ogrenme-deep-learning-nedir/){:target="_blank"} bakabilirsiniz.


Bu yazımda Ubuntu 16.04 üzerinde TensorFlow'a kısa bir başlangıç yapmaya çalışacağım.

TensorFlow dahil birçok makine öğrenmesi kütüphanesi ile geliştirilen projelerde genelde Python tercih ediliyor. Neden Python diye sorarsak hızlı geliştirme olanağı ve topluluk desteği diyebiliriz. 


İlk olarak Python kurulumunu gerçekleştirelim.

```bash
$ sudo apt-get install python3-pip python3-dev -y
```

Daha sonra pip için güncelleme yapalım.

```bash
$ sudo pip3 install --upgrade pip
```

Son olarak hangi versiyonu kullanacağınıza göre TensorFlow kurulumunu başlatalım.

> Eğer harici bir Nvidia ekran kartınız varsa GPU versiyonunu yoksa CPU versiyonunu tercih ediniz. Çünkü GPU versiyonu CPU versiyonuna kıyasla çok daha hızlıdır. Derin öğrenmede daha iyi sonuçlar almak için çok büyük veri setleri kullanıldığı için bu noktada performans çok önemlidir.


Harici ekran kartımız yok veya yeterli güçte değilse TensorFlow'un CPU versiyonunu kuralım.

```bash
$ sudo pip3 install tensorflow -y
```

GPU'ların CPU'nun yapacağı işleri yapması gözle görülür derecede hız artışı sağlar. Bu desteği Nvidia CUDA ismini verdiği GPU üzerinde çalışmasını sağlayan geliştirme araçları kümesi (Toolkit) sayesinde gerçekleştirir. CPU üzerinde gerçeklemesi zor olan büyük işlemlerde CUDA işlemi daha küçük parçalara ayırıp paralel olarak yaptığı için büyük avantaj sağlar. 

CUDA konusunda daha fazla bilgiyi [şuradan](http://www.nvidia.com.tr/object/cuda-parallel-computing-tr.html){:target="_blank"} edinebilirsiniz.

GPU'nuzun CUDA desteğini [şuradan](https://developer.nvidia.com/cuda-gpus){:target="_blank"} kontrol edebilirsiniz. TensorFlow'u CUDA desteği ile çalıştırabilmek için 3.0 ve üzeri bir Compute Capability değerine sahip bir ekran kartına sahip olmanız gerekmektedir.

CUDA kurulumuna geçmeden önce ekran kartınızın sürücüsünü (driver) kurmalısınız. Bu işlemi PPA ile yapabilirsiniz.

```bash
sudo add-apt-repository ppa:graphics-drivers/ppa -y
sudo apt-get update
```
Bu işlemden sonra Ubuntu'daki Additional Drivers kısmından sürücünüzü kolaylıkla kurabilirsiniz.

CUDA Toolkit 8.0'ı Ubuntu 16.04 sistemimize kurmak için ilk olarak [şuradan](https://developer.nvidia.com/compute/cuda/8.0/Prod2/local_installers/cuda-repo-ubuntu1604-8-0-local-ga2_8.0.61-1_amd64-deb){:target="_blank"} debian paketini (1.8 GB) indirmeliyiz. Daha sonra aşağıdaki şekilde kurulumumuzu gerçekleştirelim.

```bash
sudo dpkg -i cuda-repo-ubuntu1604-8-0-local-ga2_8.0.61-1_amd64.deb
sudo apt-get update
sudo apt-get install cuda
```

CUDA kurulumunun son aşaması olarak PATH sistem değişkenini tanımlayalım.

```bash
export PATH=/usr/local/cuda-8.0/bin${PATH:+:${PATH}}
```

cuDNN adında bir kütüphane daha mevcut. cuDNN kısaca CUDA'nın derin öğrenmeye göre optimize edilmiş bir versiyonudur. Çok katmanlı yapay sinir ağlarında büyük bir performans artışı sağlar. cuDNN'ı kullanabilmek için [şuradan](https://developer.nvidia.com/accelerated-computing-developer){:target="_blank"} Nvidia geliştirici hesabı oluşturmanız gerekiyor. Ardından cuDNN'ı [şuradan](https://developer.nvidia.com/compute/machine-learning/cudnn/secure/v5.1/prod_20161219/8.0/libcudnn5-dev_5.1.10-1%2Bcuda8.0_ppc64el-deb){:target="_blank"} indirebilirsiniz. İndirdiğimiz debian paketini GDebi ile kurabilirsiniz.

Opsiyonel olarak CUDA profilleri için kullanılan aracı şu şekilde kurabilirsiniz.

```bash
sudo apt-get install libcupti-dev -y
```

Gerekli kurulumları bitirdikten sonra son olarak TensorFlow'un GPU versiyonunu kuralım.

```bash
$ sudo pip3 install tensorflow-gpu -y
```

Yeni bir sürüm çıkığında kurduğumuz versiyonu son sürüme güncellemek için şu komutları kullanabiliriz;

```bash
sudo pip3 install --upgrade tensorflow

sudo pip3 install --upgrade tensorflow-gpu
```

### Hello world ile TensorFlow'a bir giriş yapalım.

~~~ python
import tensorflow as tf

hello = tf.constant('Hello World')

with tf.Session() as sess:
  print(sess.run(hello)) 
~~~

TensorFlow'da bazı işlemlerde kullanılmak üzeri sabit olarak **constant** kullanılıyor. Çıktının gösterilmesi aşamasında ise **Session** yapısı kullanılıyor. 


### Şimdi de TensorFlow ile basit işlemler gerçekleştirelim.

~~~ python
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
~~~

 İlk olarak **x1** ve **x2** adında iki sabit tanımlayıp ardından toplama, çıkarma ve çarpma işlemleri için TensorFlow'da tanımlı olan fonksiyonları (**add**, **sub**, **mul**) kullandık.

 Daha sonra matris çarpımı için **matmul** fonksiyonunu, doğrusal aralıklı bir vektör dizisi oluşturmak için de **linspace** fonksiyonunu kullandık.

 Son olarak bu işlemlerin sonuçlarını atadığımız değişkenleri **Session** içerisinde **print** fonksiyonuyla ekrana yazdırdık.

Bir blog yazısının daha sonuna geldik [TensorFlow ile MNIST Örneği](https://emredurukn.github.io/2016/11/09/tensorflow-ile-mnist-ornegi.html){:target="_blank"} adlı diğer blog yazıma da göz atabilirsiniz.

 <br>