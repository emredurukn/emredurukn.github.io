---
layout: post
title: "Ubuntu üzerinde Rails kurulumu"
published: true
date: 2016-08-19
tags : [rails, ubuntu]
image: ruby-on-rails.jpg
description: Rails'in Ubuntu 16.04 üzerinde kurulum adımları
---

[Ruby on Rails](http://rubyonrails.org/), Ruby dili ile yazılmış açık kaynak kodlu bir web framework'tür. [Ruby](https://www.ruby-lang.org/en/) ise nesneye yönelik bir programlama dilidir. Bu yazımda Rails'in Ubuntu 16.04 üzerinde kurulum adımlarını anlatmaya çalışacağım.

<center>
	<amp-img width="1080" height="329" layout="responsive" src="/assets/images/ruby-on-rails.jpg"></amp-img>
</center>


### 1) Ruby Kurulumu

Ruby kurulumuna başlamadan önce paket arşivini güncelleyelim.

```bash
$ sudo apt-get update
```

Ruby kurulumu için gerekli bağımlılıkları kuralım.

```bash
$ sudo apt-get install curl zlib1g-dev build-essential libssl-dev libreadline-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt1-dev libcurl4-openssl-dev python-software-properties libffi-dev -y
```

Ruby kurulumunu gerçekleştirelim.

```bash
$ sudo apt-get install ruby ruby-dev -y
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
$ curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -

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