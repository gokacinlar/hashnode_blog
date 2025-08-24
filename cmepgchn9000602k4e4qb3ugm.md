---
title: "JavaScript'te document.readyState ile load, DOMContentLoaded, window.onload & beforeunload farkları nelerdir?"
seoDescription: "JavaScript'te document.readyState, load, DOMContentLoaded, window.onload ve beforeunload nedir? Yüklenme durumlarına göre farkları öğrenin"
datePublished: Sun Aug 24 2025 08:54:33 GMT+0000 (Coordinated Universal Time)
cuid: cmepgchn9000602k4e4qb3ugm
slug: javascriptte-documentreadystate-ile-load-domcontentloaded-windowonload-and-beforeunload-farklari-nelerdir
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1755963213162/8ed36319-2bd2-4e74-9a17-599b1b7e356b.jpeg
tags: javascript, dom, document-object-model, window-object-in-javascript, lifecycle-methods, domcontentloaded

---

JavaScript ile, özellikle DOM ile çalışırken ihtiyacımız olan işlemlerden birisi, web sayfamızda gerçekleştireceğimiz dinamik arayüz işlevlerinin, web sayfasının yüklenme durumuna göre gerçekleşmesini sağlamaktır. Buna kısaca HTML’in yaşamdöngüsüne ait işler diyebiliriz ve genel olarak JavaScript’ten aşina olduğumuz *“eventListener”* mantığında çalışırlar. Daha önceki yazılarımdan [birinde](https://gokacinlar.hashnode.dev/typescriptte-light-dom-ve-shadow-dom-kullanimi-ve-farklari) belirttiğim gibi, eğer, örneğin web bileşenleri ile çalışıyorsak, gerçekleştirmek istediğimiz etkileşimlerin veya fonksiyonların, sayfanın yüklenme durumuna (yüklenirken, yüklenmeden önce & yüklendikten sonra) göre ayarlamamız gerekebilir. İşte bu durumlarda JavaScript, bir HTML sayfasının, kullanıcı tarayıcısında *render* edilme sürecinde neler yapılacağını belirten çeşitli yüklü fonksiyonlara sahiptir. Bu fonksiyonları, öncelikle bu sayfa yüklenme prensibini anladıktan sonra açacağız.

## document.ReadyState Nedir?

[document.ReadyState](https://developer.mozilla.org/en-US/docs/Web/API/Document/readyState), sayfanın yüklenme durumunu gösteren ve üç parametre alarak bu yüklenme aşamalarında işlem gerçekleştirmemize olanak tanıyan bir ögedir. Bu parametreler;

1. loading
    
2. interactive
    
3. complete
    

…şeklinde üçe ayrılır. **loading** kısmında sayfa, DOM ögeleri (&lt;h1&gt;, &lt;p&gt;, &lt;div&gt; vs.) gibi elementler ve içerikleriyle hala daha yüklenme aşamasındadır ve genellikle gördüğünüz boş beyaz sayfa şeklinde kendini belli eder. **interactive** kısımda ise bu DOM ögeleri yüklenmiş olmasına karşılık resim, video veya KB cinsinden yüksek olan dosyalar, paket paket gelip sizin tarayıcınızda indirildiği için daha yükleniyor olarak görülebilir fakat adı üzerinde, interactive olan bu kısımda sayfa ile bir şekilde etkileşiminiz mevcuttur. **complete** kısmında ise tüm website yüklenmiştir.

`document.readyState` değerini değişikliklerinde izlemek için `document.onreadystatechange` olayını kullanabilirsiniz; **her readyState değiştiğinde** bu olay tetiklenir:

```javascript
document.onreadystatechange = function () {
    if (document.readyState === "loading") {
        // DOM hâlâ yükleniyor
    } else if (document.readyState === "interactive") {
        // DOM yüklenmiş, kullanıcı ile etkileşim mümkün
    } else if (document.readyState === "complete") {
        // Tüm kaynaklar yüklenmiş
    }
};
```

JavaScript’teki sayfa yüklenme prensiplerini ufaktan anladığımıza göre, gelin `DOMContentLoaded` ile `window.onload` arasındaki farkları açıklayalım:

## DOMContentLoaded Nedir?

`DOMContentLoaded`, adından da anlaşılabileceği üzere, DOM’a ait elemanların, daha çok *scriptler* ve HTML’in kendisi şeklinde tarayıcıda **tamamen parse edildikten, yani indirilip hazır hale getirildikten sonra** çalıştırabileceğimiz bir ortam sunar ve uygulama mantığı olarak, yüklenmeye paralel olarak izleyecek olan fonksiyonları eklememize olanak sağlar. HTML’e dahil edilen *script* etiketlerinin aldığı **attribute’lardan** biri olan `defer` ve `type=”module”` şeklinde, özellikle bileşen tabanlı çalışılan ve modül olarak entegre edilen projelerde temel yüklenme biçiminde `DOMContentLoaded`, **bunların yüklenmesini bekledikten sonra çalışacaktır.** Ayrıca CSS dosyalarının, `link` olarak entegre edilmiş ögelerinin bitmesini de bekler. Fakat dikkat edilmesi gereken nokta, **bu olayın resim, video gibi dosyaların yüklenmesini beklememesidir.**

```javascript
"use strict"

document.addEventListener("DOMContentLoaded", ()=> {
    // DOM yüklendiğinde gerçekleştirilecek olaylar buraya yazılır
    console.log("DOM Yüklendi.");
});
```

Buraya konulacak herhangi bir fonksiyon, defer etiketi ile yüklenmesi bittikten sonra çalışacak script içine gömülü JavaScript fonksiyonlarından modül olarak belirtilen bileşenlere kadar DOM’u ilgilendiren her bir şey yüklendikten sonra çalışacak demektir.

## onload Nedir?

`DOMContentLoaded’in` aksine, onload event’i, bir sayfa, tüm özellikleriyle birlikte tamamen yüklendikten sonra çalışır. Sadece DOM değil; resim, video veya diğer ögelerin hepsi yüklendikten sonra çalışır. onload, [window nesnesine](https://developer.mozilla.org/en-US/docs/Web/API/Window) ait bir özelliktir ve window bildiğiniz üzere tarayıcının bir sekmesinde açık olan websitenin tamamını ihtiva eden kapsayıcı bir nesnedir ve pek çok metoduyla tarayıcı ile etkileşime geçmemize olanak verir.

```javascript
"use strict"

// İki farklı kullanımı vardır
// 1. Geleneksel kullanım:
window.onload = function() {
    console.log("Sayfa tamamen yükleni");
};

// 2. Modern kullanım:
window.addEventListener("load", function() {
    console.log("Sayfa tamamen yükleni");
});
```

## beforeunload Nedir?

Şu ana kadarki metotlarımızda sadece sayfanın yüklenme durumlarıyla ilgilenmiştik fakat JavaScript oldukça güçlü bir dil ve zengin bir kütüphaneye sahip olduğu için, kullanıcının bir websitesinden çıkışı durumunda da gerçekleştirebileceği fonksiyonlar vardır ve bunlardan birisi `beforeunload` metodudur. `beforeunload`, kullanıcı siteden çıkarken onu uyarmamızı ve gerekli işlemleri yerine getirmemizi, gerekirse tarayıcının kendi uyarı modalı’yla gerekirse de bizim kendi yapacağımız bir bileşenle sağlar. Bazı sitelerde (örn: Google Drive gibi) şu mesajla karşılaşmışsınızdır:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756025152282/83e6c57e-bbca-4f25-831b-0d9ece24cb0e.png align="center")

Bu mesajı tetikleyen şey, **beforeunload event’idir**. `beforeunload`, sayfadan ayrılış durumlarında tetiklenebilecek bir mesaj modal’ini göstermekle beraber istediğiniz farklı şeyleri de yerine getirir (kullanıcıyı anımsatmak, yüklenme & indirmenin sekteye uğrayacağını göstererek uyarı yapmak, kaydedilmemiş değişiklikleri göstermek veya 2000’lerden aşina olduğumuz kullanıcıyı irite eden şakalar yapmak gibi).

```javascript
window.addEventListener("beforeunload", (e) => {
    // beforeunload eventinin bir returnValue değeri de alabilmektedir
    // fakat modern tarayıcıların hepsi bu değeri görmezden gelir ve kendisininkini
    // gösterir.
    e.returnValue = "Siteden çıkıyor musun?";
});
```

Başka bir yazımda görüşmek dileğiyle!