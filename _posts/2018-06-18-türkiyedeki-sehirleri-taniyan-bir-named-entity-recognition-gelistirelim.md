---
layout: post
title: "Türkiye'deki şehirleri tanıyan bir Named Entity Recognition geliştirelim"
published: true
date: 2018-06-17
tags : 
 - named entity recognition
 - stanford ner
image: named-entity-recognition.png
description: Stanford NER ile metinden Türkiye'deki şehirleri tanıyan bir Named Entity Recognition'ı geliştirme adımları
---


Yazının devamını daha iyi anlayabilmeniz için bazı kavramları açıklamakta fayda var.

Named Entity Recognition, metinlerden bilgi çıkarımı anlamına gelmektedir. Daha ayrıntılı bilgi için [bu bağlantıyı](https://towardsdatascience.com/named-entity-recognition-applications-and-use-cases-acdbf57d595e?gi=7e5f4cdf2720){:target="_blank"} inceleyebilirsiniz.

[Stanford NER](https://nlp.stanford.edu/software/CRF-NER.html){:target="_blank"}, Stanford Üniversitesinin NLP topluluğu tarafından geliştirilen [CRF](http://blog.echen.me/2012/01/03/introduction-to-conditional-random-fields/){:target="_blank"} tabanlı bir NER aracıdır ve Java'da yazılmıştır.

<center>
	<amp-img width="993" height="432" alt="Named Entity Recognition" layout="responsive" src="/assets/images/named-entity-recognition.png"></amp-img>
</center>

Bu yazıda Stanford NER kullanarak Ruby'de verilen metinden Türkiye'deki şehirleri çıkarabilen bir Named Entity Recognition'ı nasıl geliştirdiğimi anlatmaya çalışacağım.

Proje için gerekli kurulumlar;

- Java 1.8 veya daha yüksek bir sürüm
- Ruby

Stanford NER'i şu şekilde indirelim ve zip'ten çıkartalım. Sonrasında Ruby tarafında kullanacağımız gem'leri kuralım

<amp-gist data-gistid="2a8539d19640572b0624f8e84bbecbeb"
  layout="fixed-height"
  height="450">
</amp-gist>

Yapacağımız proje genel olarak iki aşamadan oluşuyor. İlk aşama NER için sınıflandırıcının oluşturulması. İkinci aşama ise oluşturduğumuz sınıflandırıcıyı bir metin ile test edilmesi.

İlk Aşama
-------
NER için sınıflandırıcıyı oluşturabilmemiz için gerekli etiketlemelerin yapıldığı bir veri seti gerekiyor. Bu veri setinin belli bir şablonu var. Bu şablon boşluk ile ayrılmış her kelimenden sonra tab karakteri gelir sonra da entity'nin ismi gelir. Bir sonraki kelime için de aynı şekilde bir alt satıra yazılır ve bu şekilde devam eder. 

<center>
	<amp-img width="458" height="469" alt="NER Corpus" src="/assets/images/ner-corpus.png"></amp-img>
</center>

Yukarıdaki görselde bu şablonu nasıl olduğunu görebilirsiniz. Veri setini bu şablona uygun olarak manuel olarak da hazırlayabilirsiniz fakat biz internetteki verilerden faydalanarak veri setini oluşturacağız. Bu aşamada Web Scraping olarak adlandırılan en basit tabirle Web üzerindeki verirlerin işlenmesi konusuna giriyoruz.

Web Scraping için [Nokogiri](https://github.com/sparklemotion/nokogiri){:target="_blank"} adlı gem’i kullanacağız.

İlk olarak bir web sayfasından bütün şehirlerin listesini alalım. Bunun için gerekli tanımlamaları yapıp ardından bu listenin yer aldığı web sayfasını open-uri ile erişiyoruz daha sonra nokogiri ile ayrıştırıyoruz. Daha sonra bu ayrıştırılmış web sayfasından CSS Selector tanımlamaları ile şehir isimlerinin yer aldığı bu cümleleri alıyoruz. Ardından birkaç string filtreleme işlemi ile şehir isimlerini bir diziye kaydediyoruz.

<amp-gist data-gistid="169acedde804f1b51a863ec2dd071b74"
  layout="fixed-height"
  height="450">
</amp-gist>

Bu aldığımız cümleleri ilk olarak kelimelere ayırıyoruz. Ardından kelimeleri şehirlerin isimlerinin bulunduğu dizi üzerinden bir karşılaştırma yaparak tsv uzantılı bir dosyaya kaydediyoruz. (Stanford NER'in veri seti şablonuna uygun olarak) Son olarak oluşturduğumuz veri seti ile sınıflandırıcıyı Stanford NER'in sağladığı CLI API'sini Ruby içerisinden çağırarak oluştururuz. java kodunu çalıştırırken kullanacağı bellek miktarı parametre olarak geçebiliyoruz. Bu proje için 1000 Mb fazlasıyla yeterli olacaktır ama daha büyük veri setleri için bu rakamı artırabilirsiniz. classifier.prop dosyasında ise oluşacak sınıflandırıcının ismi ve kullanılacak veri setinin ismi gibi gerekli ayarlamalar mevcut.

<amp-gist data-gistid="c3a760fdc41ca71d8cf142fd3663940a"
  layout="fixed-height"
  height="450">
</amp-gist>

Bu işlemin ardından city-classifier.ser.gz adlı sınıflandırıcımızı oluşturmuş olduk.


İkinci Aşama
-------
Oluşturduğumuz sınıflandırıcıyı test etmek için bir text dosyası oluşturduk. Ardından oluşurduğumuz sınıflandırıcıyı test ettik. Çıktı dosyanının formatı olarak slashTags , inlineXML , xml , tsv ve tabbedEntities seçeneklerinden birini seçebilirsiniz.

<amp-gist data-gistid="18ea5f3c7876f7dea79751ada2cb4b60"
  layout="fixed-height"
  height="450">
</amp-gist>

Bu işlemin ardından verdiğimiz input.txt'den output.txt dosyasını oluşturmuş olduk. Çıktı dosyasını incelersek Konya'yı başarılı bir şekilde bulduğunu görebilirsiniz. Veri setinizdeki örnekleri çoğaltarak ve farklı entity'ler ekleyerek daha kapsamlı Named Entity Recognition geliştirebilirsiniz. Eğer projenin tamamını incelemek isterseniz [şuradan](https://github.com/emredurukn/corenlp-ner) kodlara ulaşabilirsiniz.