---
title: "Linux Hakkında I - Linux’u Kullanmak İçin “Etik” Gerekçelerin Değerlendirilmesi"
seoTitle: "Linux Hakkında I - Linux’u Kullanmak İçin “Etik” Gerekçelerin Değerlen"
seoDescription: "Linux, sahipli yazılımlara nazaran etik olarak kullanıcıya ne vaat ediyor?"
datePublished: Sun Jun 15 2025 15:02:00 GMT+0000 (Coordinated Universal Time)
cuid: cmbxsne30000202l9ayzu5x45
slug: linux-hakkinda-i-linuxu-kullanmak-icin-etik-gerekcelerin-degerlendirilmesi
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1749999555999/2dcf6407-6e98-4fc0-a9fd-6f68ac106829.webp
tags: linux

---

---

**Linux** ile, yakın bir arkadaşımın vasıtasıyla, Debian’ın adeta kendi başına bir IQ testi olan yükleme ve kurulumunu kolaylaştırmak (Linux’u yaratan adam olan Linus Torvalds bile Debian [çok zor olduğu için yükleyemediğini](https://www.youtube.com/watch?v=qHGTs1NSB1s) belirtmiştir :D) ve insanları Linux ile kolay bir şekilde etkileşime geçip bu çekirdeğe dayalı sistemi ve bu sistemin esas aldığı yazılım ve bir nevi hayat felsefesi sayılabilecek [**GNU**](https://www.gnu.org/home.tr.html) fikrini yaygınlaştırmayı misyon edinmiş [Ubuntu](https://ubuntu.com/)’nun 10.10 sürümüyle tanışmam sonrası pek çok dağıtımı deneme ve linux terminolojisi ise [“distro hopping”](https://embeddedinventor.com/distro-hopping-what-why-how-explained/) yapmaya fırsatım oldu. Genel anlamıyla Linux’u ben, gündelik işleri yapmak, GNU felsefesine göre yazılımsal olarak bu felsefenin yakındığı temel sorunlar olan gizlilik, sahipli yazılım ve oligopol haline gelmiş yazılım tekellerine olan bağımlılığı azaltmak maksadıyla ve elbette, kendi yazılım/web geliştirme işlerim için yıllardır kullanıyordum ve şu an ana makinelerimde de Ubuntu dağıtımını kullanıyorum. Pek çok insan da bu işletim sistemini denemek, hatta mümkünse Windows’tan veya MacOS’ten geçmek için bir alternatif olarak görüyor ve yeni gelenlere ise bu şekilde pazarlanıyor. Bu pazarlamanın dayandığı kollar, çok genel tabiriyle “etik” ve “teknik” olarak ikiye ayrılıyor. Peki bunlar nelerdir?

Eğer Microsoft’un üretip dağıtımını yaptığı Windows işletim sistemi ailesinin herhangi bir sürümünü kendi bilgisayarınıza kurup kullanmak isterseniz, [“EULA (End-User License Agreement)”](https://www.technopat.net/sosyal/konu/windows-10-final-tuerkce-kullanici-soezlesmesi-eula.230483/) denilen bir kullanıcı sözleşmesi imzalarsınız. Kullanıcıların %99.9’u bunun ne olduğuna bakıp, durup yüzeysel bir tarama, bir skimming bile yapmazlar ve bunun için çok mantıklı gerekçelere de sahiptirler – iş ve gündelik hayatta kimse zaten bir standart ve hatta zorunluluk olarak gördüğü ve “kullanmak zorunda olacağı” bir işletim sisteminin sayfalarca uzunluktaki legal dökümanı için zaman ayırmaz. Fakat ayıranlar, bu EULA’da şöyle bir ifadeye denk gelecektir (detaylı incelemesine [buradan](https://www.cagataycebi.com/free_articles/freedom/eula_facts.html) ulaşabilirsiniz):

> Yazılımın satışı yapılmamakta, lisansı verilmektedir. İlgili yasalar size daha fazla hak vermiyorsa, Microsoft; zımnen, önceden yapılan bir beyana dayanarak itiraz hakkı bulunmayan veya diğer bir biçimde işbu anlaşma kapsamında açıkça verilmemiş diğer tüm hakları saklı tutmaktadır. Yazılımdaki, onu yalnızca belirli şekilde kullanmanıza izin veren her türlü teknik sınırlamaya uymanız gerekir.

Bu teknik sınırlama içerisinde, Windows işletim sistemini ve bunun üzerine yazılmış Microsoft tarafından geliştirilen çoğu uygulamanın, “proprietary” denilen sahipli yazılımlarının, sizin, yani son kullanıcı tarafından kaynak kodlarının incelenememesine, değiştirilememesine ve tekrardan dağıtımının yapılamamasına kadar varan bir dizi kısıtlamaları ihtiva etmektedir. Bu kısıtlamaların temel olarak iki amacı vardır:

* Kodların ve yazılımın sahibi olan şirketin, bunlara olan talebi para kazanmak için değerlendirmek istemesi.
    
* Bill Gates’in meşhur bir mektubu olan, [“An Open Letter to Hobbyists”](https://genius.com/Bill-gates-an-open-letter-to-hobbyists-annotated)da açıkladığı gibi, “bu yazılımı ben yaptım ve bu yazılımı kimseye vermemek benim hakkımdır” şeklinde açıklanabilecek bir etik görüşünü savunmaktır.
    

GNU, işte tam da bu noktaya itiraz etmektedir. GNU felsefesine göre, sizin donanımınız olan makineye (mikroişlemciyle çalışan herhangi bir bilgisayar) yüklediğiniz yazılımlar üzerinde tam bir kontrol sahibi olmanızı, bu yazılımları gerekirse açıp inceleyebilmenizi ve üzerinde değişiklik yapabilmenizi, böylece sahipli olan yazılımların genellikle başını çektiği, onu dağıtıp size satan veya freeware olarak size kullandırtan şirketlerin/oluşumların kendi kararlarından etkilenmeden, özgürce makinenizle ve sizin kararlarınızla optimize bir şekilde çalışabilmesini öngören bir dizi kurallara gereksinim olduğunu iddia etmektedir. Bu kurallar kabaca dört madde şeklinde genellenmiştir:

1. Herhangi bir amaç için programı istediğiniz gibi çalıştırma özgürlüğü (özgürlük 0).
    
2. Programın nasıl çalıştığını inceleme özgürlüğü ve programı istediğiniz gibi yapın (özgürlük 1). Kaynak koduna erişim bunun için bir ön koşuldur.
    
3. Başkalarına yardım edebilmeniz için kopyaları yeniden dağıtma özgürlüğü(özgürlük 2).
    
4. Değiştirilmiş sürümlerin kopyalarını başkalarına dağıtma özgürlüğü (özgürlük 3).
    

Sahipli yazılımı tercih eden şirketlerin, start-up’ların veya herhangi bir yazılım geliştiricisinin ağırlıklı hedefi, bu yazılımı satarak para kazanabilmek ve kaynak kodlarını elinde bulundurduğu için, başkalarının bunlara erişmesini engelleyerek o yazılımın sunduğu hizmetin, üreticisine has olmasını sağlamaktır. Windows bunun en popüler örneklerinden biridir. Bunun haricinde, sahipli yazılımlar [ücretsiz (freeware)](https://tr.wikipedia.org/wiki/%C3%9Ccretsiz_yaz%C4%B1l%C4%B1m) de olabilmektedirler ve bu tür yazılımlar üzerinden kazanılan para, “eğer bir ürün için para ödemiyorsanız, ürün sizsinizdir” şeklindeki felsefeden gelir. Google gibi şirketlerin ürünleri genellikle ücretsizdir ve ücretli olan servislerinden, örneğin yıllık fiyatı 199.99TL (7$) olan [200GB Google Drive paketi](https://one.google.com/about/plans?hl=tr) gibi hizmetlerden kazandığı para komik seviyelerdedir ve Google gibi devasa bir oligopolun bilançosunu kurtarabilmesi için bundan çok daha fazlasına ihtiyacı vardır.

Google bunu, en kaba tabirle “reklamlar”la yapar ve bunu da, yazılımsal algoritmalarının, sizin ona sunduğunuz arama geçmişi, fotoğraflar, beğeniler, konum, belgeler, çerezler ve diğer dijital alışkanlıklarınızı, reklam partnerlerine uygun şekilde yönlendirerek yapar. Aşağıdaki diyagram bu süreci özetlemektedir:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1749999600206/9ab3ca1a-b48a-4ae8-adcc-34efd259f9ff.png align="center")

Bu uğraşın bir adı bile vardır: Reklam takip endüstrisi. Bu size ücretsiz, kullanımı kolay, arayüzü estetik ve yaygın kabul gören pek çok uygulamanın (Facebook, Whatsapp, Instagram vs.) can alıcı bir bedelle erişimini sağlar: Gizlilik ve mahremiyet. Çünkü veri sizsiniz.

Normatif yargıda bulunmadığımız zamanki göreceğimiz tabloda bu yaklaşım, aslında günümüz modern haberleşme veya sosyal paylaşım ağlarının var olabilmesi için gereken maddi kaynakları sağlamaktadır; böylece teknoloji ile harmanlanmış ve elektrik kadar vazgeçilmez hale gelmiş bazı alışkanlıkların devamı sağlanır. Fakat daha önce gizlilik ve mahremiyet hakkında nasıl bir tavır takındığını açıkladığımız GNU felsefesi, bunun doğru bir uygulama olmadığını söyleyecektir çünkü GNU, burada etik bir yargıda bulunur ve nasıl ki sokakta yanınızdan geçen rastgele bir yabancıya, dijital cihazınızdaki verilerinizi asla vermeyecek olmanıza rağmen, sizden bu verilerinizi “işlemek” üzere alan herhangi başka bir yabancıdan farksız olan şirketlere verilmesine karşı çıkar.

Bu, eninde sonunda bir güç ve kontrol çekişmesine yol açmaktadır: Bazı özgürlüklerinizi feda ederek kullanacağınız yaygın & kullanımı kolay bir program mı yoksa hiçbir şey feda etmeden kullanacağınız yaygın olmayan & genellikle kullanıcı tarafından yazılıma müdahale gerektiren (konfigürasyon, hataların kullanıcı tarafından çözülmesi gibi birkaç “ekstra” çabaya dayalı çalışma) bir program mı? GNU burada sahipli yazılımların, hele aşırı şekilde yaygınlaşıp kullanıcıların üzerinde söz söyleme haklarını ellerinden alacak ölçüde tekelleşmiş, bu sayede de ciddi bir rakibi olmayan ve bu yazılımı geliştiren şirketin istediği gibi at koşturabileceği keyfi bir güç hakkına karşı çıkar. Toparlayacak olursak, şu maddelere dikkat eden GNU felsefesinin neden Linux üzerinde ısrar ettiğini kolaylıkla anlayabilirsiniz:

1. Sahipli yazılım, onu geliştiren kişinin istekleri ve yaygın olma durumuna göre her zaman doğru yönde gelişmez ve genellikle yanlış yönelimlerle sahip olur.
    
2. Sahipli yazılımın tekel olduğu yerde kullanıcı mahremiyeti de kısıtlı olur; çünkü bu yazılımlar genellikle freeware olduğu için mevcudiyetlerinin devamı için gereken finansmanı kullanıcı verilerini işleyerek satmak zorundadırlar.
    
3. Sahipli yazılımı geliştiren kişilerin fikirlerine uygun olmayan karşıt fikirleri savunan kişilere karşılık, gücün aşırı kullanımına varan sansür, shadowban veya kendisini yazılımdan dışarı atan engelleme gibi önlemlere başvurulmasına oldukça sık şahit olunur ve bu yazılımlarda gerçek manada özgür düşünceden ve bu düşüncenin ifade edilmesinden bahsedilemez.