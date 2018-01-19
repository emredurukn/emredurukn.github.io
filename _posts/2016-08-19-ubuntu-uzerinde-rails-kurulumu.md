---
layout: post
title: "Ubuntu üzerinde Rails kurulumu"
published: true
date: 2016-08-19
tags : 
 - rails
 - ubuntu
image: ruby-on-rails.jpg
description: Rails'in Ubuntu 16.04 üzerinde kurulum adımları
---

[Ruby on Rails](http://rubyonrails.org/){:target="_blank"}, Ruby dili ile yazılmış açık kaynak kodlu bir web framework'tür. [Ruby](https://www.ruby-lang.org/en/){:target="_blank"} ise nesneye yönelik bir programlama dilidir. Bu yazımda Rails'in Ubuntu 16.04 üzerinde kurulum adımlarını anlatmaya çalışacağım.

<center>
	<amp-img width="1080" height="329" alt="Ruby on Rails" layout="responsive" src="/assets/images/ruby-on-rails.jpg"></amp-img>
</center>


### 1) Ruby Kurulumu


Ruby ve Rails kurulumu için gerekli bağımlılıkları kuralım. Ardından Ruby'nin son sürümünü kurabilmek için ppa ekleyelim.

```bash
$ sudo apt-get install zlib1g-dev libsqlite3-dev libpq-dev curl software-properties-common -y

$ sudo add-apt-repository ppa:brightbox/ruby-ng
```

Paket arşivini güncelleyelim.

```bash
$ sudo apt-get update
```

Ruby kurulumunu gerçekleştirelim.

```bash
$ sudo apt-get install ruby2.4 ruby2.4-dev -y
```

Ruby versiyonunu kontrol edelim.

```bash
$ ruby -v
```


### 2) Geliştirme araçlarının kurulumu

Güncel versiyonu kurmak için bir depo ekleyelim.

```bash
$ sudo apt-add-repository ppa:git-core/ppa -y
```

Paket arşivini güncelleyelim.

```bash
$ sudo apt-get update
```

Git kurulumunu gerçekleştirelim.

```bash
$ sudo apt-get install git -y
```

Git versiyonunu kontrol edelim.

```bash
$ git --version
```


NodeJS kurulumunu gerçekleştirelim.

```bash
$ curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -

$ sudo apt-get install nodejs -y

$ nodejs -v
```


### 3) Rails Kurulumu

İlk olarak gemleri güncelleyelim.

```bash
$ sudo gem update --system

$ sudo gem update
```

Rails kurulumunu gerçekleştirelim.

```bash
$ sudo gem install rails
```

Rails versiyonunu kontrol edelim.

```bash
$ rails -v
```

Artık Rails ile uygulama geliştirmeye hazırız.