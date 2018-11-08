---
layout: post
title: "Ruby ile Web Scraping'e Giriş"
published: true
date: 2018-11-09
tags:
  - web scraping
  - ruby
  - web crawling
  - nokogiri
image: web-scraping.jpg
description: Ruby ile Web Scraping'e Giriş
amp-gist: true
---

Kısaca Web Crawling, web sitelerinden linklerin elde edilmesine denir. Web Scraping ise web sitelerinde yer alan verilerin toplanmasına denir.

<center>
	<amp-img width="350" height="189" alt="Web-Scraping" layout="responsive" src="/assets/images/web-scraping.jpg"></amp-img>
</center>

Bu yazımda Web Scraping yaparken dikkat edilmesi gereken durumlardan ve karşımıza çıkabilecek hatalar ve buna karşı uygulayabileceğimiz çözümlerden bahsedeceğim. Sonrasında örnek olarak Vikings dizisinin imdb [sayfasında](https://www.imdb.com/title/tt2306299/){:target="_blank"} yer alan en yüksek puan alan bölümlerinin bilgilerini toplayacağız.

Bu yazımızda [nokogiri](https://github.com/sparklemotion/nokogiri){:target="_blank"} adlı gem'i kullanarak web sayfasında aradığımız kısımlara erişip bu verileri kaydetmek için HTML içeriğini işleyeceğiz. (Parsing işlemi)

Bir siteye ait Web Scraping kodunu yazarken sitemap'i ve dolaylı olarak sitede yer alan url'leri bulmak için ilk olarak sitenin **robots.txt**'sine bakılır. Aynı zamanda siteye ait sitemap'in sitenin **/sitemap.xml** konumunda yer alma olasılığı yüksektir. Siteye ait sitemap'i bulamadığımız noktada site içerisinde gezinerek kullanacağımız url'leri toplamayı deneyebiliriz.

Bazı sitelerde robots.txt'de sitemap konumu dışında crawl işlemi konusunda uyulması istenen kurallar da yer alabilir. Bu kurallara örnek olarak crawling işleminin yapılması istenen saat aralığı ve her iki istek arasında beklememiz gereken **crawl-delay** değeri yer alabilir. Bu kurallara uymadığınız takdirde siteye olan erişiminiz engellenebilir.

Verileri toplayacağımız siteye çok fazla yük bindirmemek için siteye gönderdiğimiz istekler arasında **en az 2 saniye** bırakmalıyız. Bunun yanında web sayfasına her eriştiğimizde farklı user agent kullanmamız siteye gönderdiğimiz isteklerin normal kullanıcılardan geliyormuş gibi gözükmesine yardımcı olur. Çok fazla url ile işlem yapacaksanız sisteminizin maksimum dosya işlem limitini artırmalısınız aksi takdirde bu limite takılırsınız ve kodunuz hataya düşer. Bu limiti sisteminizde şu komut ile görüntüleyebilirsiniz. 

```bash
ulimit -n
```
Ubuntu için bu limitin default değeri 1024 bu sayıdan fazla url'e erişmeniz gereken durumlar için şu komut ile sisteminizin maksimum dosya limitini artırabilirsiniz. 

```bash
sudo bash -c "echo '* - nofile 250000' >> /etc/security/limits.conf"
```

Şimdi gelelim örnek kodumuzu yazmaya ilk olarak kullanacağımız gem'leri, değişkenleri ve fonksiyonları tanımlayalım.

<amp-gist data-gistid="6d180954470bc3221549fb4da1588b1e"
  layout="fixed-height"
  height="450">
</amp-gist>

Yukarıdaki kodu incelersek **url_valid?** fonksiyonunda erişeceğimiz url'in düzgün formatta olup olmadığını kontrol ediyor. **normalize_uri** fonksiyonunda ise url'de encoding'den kaynaklı sorunlar varsa çözüyor. 4 farklı user agent'ın yer aldığı bir dizi tanımlarız. Daha sonra en yüksek oy alan bölümlerin bilgilerini saklayacağımız **top_rated_episodes** adında bir dizi tanımlarız. Son olarak da erişeceğimiz url için bir değişken tanımlarız.

<amp-gist data-gistid="9d89662dfd71e38d1021aaf170d3f51f"
  layout="fixed-height"
  height="450">
</amp-gist>

Yukarıdaki kodu incelersek ilk olarak erişeceğimiz url'in formatını test ediyoruz eğer formatı düzgün ise open-uri ile random bir user-agent kullanarak siteye erişiyoruz. Eğer formatı geçerli değilse normalize_uri fonksiyonu ile encoding sorunlarını elimine etmeyi deniyoruz. Kod içerisinde **open-uri ile siteye erişdiğimiz kısımlarda** birçok nedenden dolayı hata alabiliriz. Bu hatalar eriştiğimiz siteden, internet bağlantısından ya da SSL'den doğrulamasından kaynaklı olabilir. Kodun SSL'den kaynaklı hataya düşme olasılığını göz önünde bulundururarak Ruby'deki begin rescue yapısıyla kodun hataya düştüğü durumlarda SSL doğrulamasını devre dışı bırakarak tekrar siteye erişmeyi deniyoruz. 

<amp-gist data-gistid="7168423b68847d608f4baa0e9df7ff28"
  layout="fixed-height"
  height="450">
</amp-gist>

Yukarıdaki kodu incelersek nokogiri gem'inin **css selector** özelliğini kullanarak html içeriğinde erişmeye çalıştığımız node'u seçiyoruz.

```ruby
pc.css("#top-rated-episodes-rhs .episode-container")
```

komutunda **id**'si **top-rated-episodes-rhs** olan node'un altında yer alan **class**'ı **episode-container** olan node'ları seçmemize yarıyor. Aradığımız bilginin hangi node'a karşılık geldiğini chrome dev tools aracılığıyla bulabiliriz. Bu işlemi web sayfasında toplayacağınız bilgiyi seçip sağ tıklayıp inspect (incele) yaparak bulabilirsiniz.

```ruby
episode.css("p:nth-child(2)")
```

komutunda ise node'un ikinci child node'unu seçeriz. Aynı zamanda bu node'un tipinin p olduğunu da belirtiyoruz. nokogiri ile css selector'un yanında xpath yapısıyla da node'lara erişebiliriz.

```ruby
File.open("top-rated-episodes.json","w") do |f|
    f.write(JSON.pretty_generate(top_rated_episodes))
end
```

komutunda ise en yüksek oy alan bölümlerin bilgilerini sakladığımız dizideki bilgileri bir json dosyasına yazdırırız. Son olarak örnek olarak yazdığım Web Scraping kodlarına [şuradan](https://github.com/emredurukn/web-scraping-examples){:target="_blank"} erişebilirsiniz.

