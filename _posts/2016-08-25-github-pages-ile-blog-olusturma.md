---
layout: post
published: true
title: "GitHub Pages ile blog oluşturma"
date: 2016-08-25
tags: [github-pages, jekyll]
image: github.pages.jpg
description: GitHub Pages ile Jekyll kullanarak adım adım blog kurulumu
---


<center>
	<amp-img width="500" height="300" layout="responsive" src="/assets/images/github.pages.jpg"></amp-img>
</center>


GitHub Pages, GitHub tarafından sağlanan projeleriniz için web sayfası oluşturabileceğiniz ve projenizi tanıtabileceğiniz bir hosting hizmetidir. Bu statik site hosting hizmeti ücretsizdir ve default olarak Jekyll desteği vardır.


Jekyll ise Ruby ile yazılmış bir statik site oluşturma aracıdır.

Bu yazımda Ubuntu 16.04 üzerinde GitHub Pages ile Jekyll kullanarak blog açmayı anlatmaya çalışacağım.


İlk olarak GitHub kullanıcı adımızı kullanarak kullanıcıadı.github.io adında bir depo (repository) oluşturuyoruz. Daha önceden aynı isimle oluşturduğum için uyarı verdi ama sizde bu uyarı olmayacaktır.

<center>
	<amp-img width="902" height="660" layout="responsive" src="/assets/images/repository-olusturma.png"></amp-img>
</center>


Şimdi [şu adresten](http://jekyllthemes.org/) bir tema seçelim. Temaların sayfalarındaki Homepage bağlantısı temanın bulunduğu deponun (repository) linkini vermektedir.

Ben bu seferlik [şu adreste](https://github.com/kronik3r/daktilo) bulunan temayı seçtim ama siz farklı temalara aynı işlemi uygulayabilirsiniz. Temayı aşağıda gösterilen alandan Download ZIP'e tıklayarak indirelim.

<center>
	<amp-img width="1138" height="575" layout="responsive" src="/assets/images/download-repository.png"></amp-img>
</center>


İndirdiğimiz zip dosyasını herhangi bir dizine çıkartalım.


Jekyll, Ruby ile yazıldığı için ilk olarak [bu yazımda](http://emredurukan.com/2016/08/19/ubuntu-uzerinde-rails-kurulumu/) açıkladığım şekilde Ruby kurulumunu gerçekleştirelim ve sonrasında Jekyll'i kuralım. 

```bash
$ sudo gem install jekyll
```

Jekyll'nin yanında sitemizde kullandığımız gem'leri kurmak için bundler adlı gem'i kuralım. 

```bash
$ sudo gem install bundler
```

Daha sonra zip dosyasını çıkartığımız klasöre geçip web sitemizin gem'lerini kuralım ve nasıl gözüktüğünü gözlemleyelim.

```bash
$ cd daktilo-master

$ bundle install

$ bundle exec jekyll serve
```

Artık web tarayıcımızı açıp **localhost:4000** adresinden sitemizin nasıl gözüktüğünü gözlemleyebiliriz. 

<center>
	<amp-img width="718" height="381" layout="responsive" src="/assets/images/local-jekyll.png"></amp-img>
</center>


GitHub oluşturduğunuz depodaki kodu saniyeler içerisinde Jekyll ile build edip yayına alıyor. Tek yapmanız gereken sitenizi düzenlemek ve deponuza düzenlediğiniz kodu push'lamak.

Ctrl + C ile terminalde işlemi yarıda keselim ve sitemizi artık GitHub'a push'layalım.


İlk olarak sitemizin bulunduğu dizinde yeni bir depo(repository) oluşturmak için şu komutu girelim.  

```bash
$ git init
```

Ardından dizindeki tüm dosyaları git'in izlemesi için şu komutu girelim.

```bash
$ git add .
```

Yaptığımız değişikleri bildirmek için şu komutu girelim.

```bash
$ git commit -m "First commit"
```

Bu adımda eğer hatayla karşılaşırsanız mail ve GitHub kullanıcı adı bilgilerinizi sisteme girin ve commit işlemini tekrar deneyin.

```bash
$ git config --global user.email "mail-adresiniz"

$ git config --global user.name "kullanıcı-adınız"
```

GitHub depomuzu (repository) blog adında bir uzak bağlantı olarak git'e tanımlayalım.

```bash
$ git remote add blog https://github.com/emredurukan/emredurukn.github.io.git
```

Son olarak web sitemizi GitHub'a push'layalım.

```bash
$ git push -u blog master
```

GitHub kullanıcı adımızı ve şifremizi girdikten sonra Enter'a basıp onaylıyoruz ve artık web siteniz hazır ve kullanıcı_adınız.github.io (emredurukn.github.io) adresinde yayında.


Bu işlemler ile kurulum aşamasını bitirmiş olduk sırada web sitenizi Jekyll'nin işleyişine uygun bir şekilde düzenlemek ve blogunuza yeni yazılar eklemek var.
