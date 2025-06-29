---
title: "JavaScript'te "Inheritance" (Miras Alma) Kavramı Nedir ve Pratik Uygulaması Nasıl Olur? (Class ve Prototype Farkı, Prototype Chain vs.)"
seoTitle: "JavaScript Miras: Sınıflar ve Prototipler"
seoDescription: "JavaScript'in kullandığı Nesne Tabanlı Programlamanın (OOP) sınıf kavramlarından olan inheritance'ın (miras alma) ne olduğunu tartışıyoruz."
datePublished: Sun Jun 29 2025 03:42:46 GMT+0000 (Coordinated Universal Time)
cuid: cmch4jton001602lg39mz9jms
slug: javascriptte-inheritance-miras-alma-kavrami-nedir-ve-pratik-uygulamasi-nasil-olur-class-ve-prototype-farki-prototype-chain-vs
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1751167810121/e20d792c-2702-4812-b085-800baa6a6a0f.png
tags: oop, javascript, prototype, classes, inheritance

---

JavaScript nesne tabanlı programlamadan en etkili şekilde yararlanan dillerden biridir ve bu NTP’nin (nesne tabanlı programlamanın kısaltması olarak yazımda kullanacağım) sahip olduğu temel kavramlardan biri olan [*“Inheritance”*](https://developer.mozilla.org/en-US/docs/Learn_web_development/Extensions/Advanced_JavaScript_objects/Object-oriented_programming#inheritance), yani Türkçe’ye kusurlu da olsa (çünkü tam anlamını karşılamıyor) **miras alma** olarak çevrilebilecek tanım kısaca; Javascript’te yaratılan bir sınıfın, kendi elemanlarının, başka bir sınıftan erişilebilir olmasını sağlayan, kökünde kodun yeniden kullanılabilirliğini esas alan ve böylece yazdığımız koda modülerlik kazandıran, kod tekrarını azaltan ve yeniden kullanılabilir sınıfları oluşturarak esnek bir biçimde kodun başka yerlerde fonksiyonunu genişleten bir kavram olarak karşımıza çıkmaktadır. JavaScript’te *Inheritance* iki şekilde tanımlanır:

1. **Geleneksel Inheritance (Class-based Inheritance):** Bu genellikle tercih edilen bir yöntemdir. Sınıflar aracılığıyla nesneler arasında miras alma ilişkisi kurulur ve bu, özellikle büyük ölçekli projelerde kod yönetimini kolaylaştırır. Üst sınıftan (parent class) alt sınıflara (child classes) özellikler ve metotlar (yazıda değineceğiz) miras alınır, bu da kod tekrarını azaltır ve bakımını kolaylaştırır.
    
2. **Prototype Tabanlı Inheritance:** Bu yolda her nesne, [bir prototip nesnesine](https://developer.mozilla.org/en-US/docs/Learn_web_development/Extensions/Advanced_JavaScript_objects/Object_prototypes) bağlıdır ve bu prototip üzerinden paylaşılan özelliklere erişebilir. Prototype tabanlı miras alma, genellikle basit projelerde tercih edilir.
    

> Şunu anlamamız gerekir ki, ileri JavaScript’te nesneler arası miras alma, esasında **sınıflar üzerine değil, sınıfların da altyapısını oluşturan prototype üzerine bina edilmiştir.** JavaScript’te nesneler, diğer nesnelerden özelliğini, adına [**prototype chain**](https://developer.mozilla.org/en-US/docs/Learn_web_development/Extensions/Advanced_JavaScript_objects/Object_prototypes#the_prototype_chain) ismi verilen bir yöntemle alır ve ES6 ile tanıtılan “class” syntaxi, bunu kolaylaştırmak üzere hazırlanmıştır. **Yani biz, geleneksel inheritance kullanırken, esasında kaputun altında prototype inheritance’ı kullanıyoruzdur.**

## Geleneksel Inheritance (Class-based Inheritance)

Gelin, basit bir sınıf ve elemanlarını, bir metotla, yani sınıf içerisinde kullanılan fonksiyonlarla oluşturalım:

```javascript
"use strict"
class Araba {
    // Sınıfımızı constructor ile instantiate ediyoruz (somutlaştırıyor, tanıtıyoruz)
    constructor(arabaModeli, arabaYasi, arabaRengi) {
        // parametrelerimizi girelim
        this.arabaModeli = arabaModeli;
        this.arabaYasi = arabaYasi;
        this.arabaRengi = arabaRengi;
    }

    // Girdiğimiz değerlere göre mesaj gösteren bir metot (fonksiyon yazalım)
    mesaj(){
        console.log(`Arabanız: ${this.arabaModeli}, Yaşı: ${this.arabaYasi}, Rengi: ${this.arabaRengi}`);
    }
}

const araba = new Araba("BMW", 2, "Kırmızı").mesaj();
// 'Arabanız: BMW, Yaşı: 2, Rengi: Kırmızı'
```

Farzedelim ki, bu sınıfa ve elemanlarına başka bir yerde ihtiyaç duyduk. Tüm bunları tekrardan yazmak yerine, basitçe yazımızın temeli olan class inheritance ile bu `Araba` isimli sınıfın özelliklerini miras alarak başka bir sınıfta kullanabiliriz:

```javascript
"use strict"
class Araba {
    constructor(arabaModeli, arabaYasi, arabaRengi) {
        this.arabaModeli = arabaModeli;
        this.arabaYasi = arabaYasi;
        this.arabaRengi = arabaRengi;
    }

    mesaj() {
        console.log(`Arabanız: ${this.arabaModeli}, Yaşı: ${this.arabaYasi}, Rengi: ${this.arabaRengi}`);
    }
}

const araba = new Araba("BMW", 2, "Kırmızı");
araba.mesaj();
// 'Arabanız: BMW, Yaşı: 2, Rengi: Kırmızı'

// Yeni bir sınıf oluşturup Araba sınıfından miras alalım
// Bunu "extends" sözcüğü ile, Araba sınıfına uzanan ElektrikliAraba sınıfı ile yapıyoruz
class ElektrikliAraba extends Araba {
    constructor(arabaModeli, arabaYasi, arabaRengi, bataryaKapasitesi) {
        // Super() metodu ile Araba'nın özelliklerini alalım    
        super(arabaModeli, arabaYasi, arabaRengi);
        this.bataryaKapasitesi = bataryaKapasitesi;
    }

    // Şarj durumunu gösteren bir mesaj ekleyelim
    sarjDurumu() {
        console.log(`Batarya Kapasitesi: ${this.bataryaKapasitesi}`);
    }
}


// Örnek bir ElektrikliAraba nesnesi oluşturalım
// Bu, Araba'dan miras aldığı parametreler olan arabaModeli, arabaYasi, arabaRengi
// özelliklerini, super() ile alır ve bataryaKapasitesi özelliğini de ekler.
const elektrikliAraba = new ElektrikliAraba("Tesla Model 3", 1, "Beyaz", "75 kWh");
// mesaj() araba nesnesinindir ancak inheritance ile aldığımız için başka bir nesnede
// kullanabiliyoruz
elektrikliAraba.mesaj();
// 'Arabanız: Tesla Model 3, Yaşı: 1, Rengi: Beyaz'
elektrikliAraba.sarjDurumu();
// 'Batarya Kapasitesi: 75 kWh'
```

Bu örnekte, `ElektrikliAraba` sınıfı, `Araba` sınıfından miras alarak onun tüm özelliklerini ve metotlarını devralır. `super()` ile üst sınıfın `constructor'ını` çağırarak ortak özellikleri tanımlarız ve ardından `ElektrikliAraba` sınıfına özgü yeni özellikler (*bataryaKapasitesi*) ve metotlar (`sarjDurumu`) ekleriz.

Fakat, biz Araba sınıfındaki, örneğin mesaj metodunu değiştirmek istiyorsak, override dediğimiz geçersiz kılma ve yenisini ekleme operasyonunu gerçekleştirebiliriz. Böylece sınıf manipülasyonu yapabiliriz.

### Override Örneği

```javascript
"use strict"
class Araba {
    constructor(arabaModeli, arabaYasi, arabaRengi) {
        this.arabaModeli = arabaModeli;
        this.arabaYasi = arabaYasi;
        this.arabaRengi = arabaRengi;
    }

    mesaj() {
        console.log(`Arabanız: ${this.arabaModeli}, Yaşı: ${this.arabaYasi}, Rengi: ${this.arabaRengi}`);
    }
}

class ElektrikliAraba extends Araba {
    constructor(arabaModeli, arabaYasi, arabaRengi, bataryaKapasitesi) {
        super(arabaModeli, arabaYasi, arabaRengi);
        this.bataryaKapasitesi = bataryaKapasitesi;
    }

    // Bu sınıfta tekrardan aynı isimde bir mesaj() metodu tanımlayarak
    // override işlemimizi gerçekleştirdik
    mesaj() {
        console.log(`Arabanız: ${this.arabaModeli}, Yaşı: ${this.arabaYasi}, Rengi: ${this.arabaRengi}, Batarya Kapasitesi: ${this.bataryaKapasitesi}`);
    }

    sarjDurumu() {
        console.log(`Batarya Kapasitesi: ${this.bataryaKapasitesi}`);
    }
}

const elektrikliAraba = new ElektrikliAraba("Tesla Model 3", 1, "Beyaz", "75 kWh");
elektrikliAraba.mesaj();
// 'Arabanız: Tesla Model 3, Yaşı: 1, Rengi: Beyaz, Batarya Kapasitesi: 75 kWh'
elektrikliAraba.sarjDurumu();
// 'Batarya Kapasitesi: 75 kWh'
```

Şimdiyse ikinci yöntemimiz olan Prototype Inheritance kısmına bakalım.

## Prototype Inheritance

Prototype tabanlı miras alma yönteminde, sınıf (class) yapısı kullanmak yerine, doğrudan fonksiyon constructor'ları ve `prototype` özelliği üzerinden metotlar ekleriz. Bu yöntem, JavaScript'in temelinde yatan ve ES6 öncesinde ve sonrasında kullanılan ve kullanılmaya devam edilen miras alma yöntemidir. ES6 ile gelen class syntaxi çok daha fazla kullanılıyor ve popüler ancak üzerine bina edildiği prototype mantığını anlamak açısından hala elzem öneme sahiptir.

Şimdi, yukarıdaki Araba örneğini prototype inheritance ile yazalım:

```javascript
jsCopyEdit"use strict"

// Araba adlı constructor fonksiyonumuzu tanımlayalım
function Araba(arabaModeli, arabaYasi, arabaRengi) {
    this.arabaModeli = arabaModeli;
    this.arabaYasi = arabaYasi;
    this.arabaRengi = arabaRengi;
}

// Araba'nın prototype'ına mesaj metodunu ekleyelim
// Burada dot notation ile prototype nesnesini çağırarak sınıfın özelliklerine erişiriz
Araba.prototype.mesaj = function() {
    console.log(`Arabanız: ${this.arabaModeli}, Yaşı: ${this.arabaYasi}, Rengi: ${this.arabaRengi}`);
};

// ElektrikliAraba adlı yeni bir constructor fonksiyon tanımlayalım
function ElektrikliAraba(arabaModeli, arabaYasi, arabaRengi, bataryaKapasitesi) {
    // Araba constructor'unu çağırarak temel özellikleri miras alalım
    Araba.call(this, arabaModeli, arabaYasi, arabaRengi);
    this.bataryaKapasitesi = bataryaKapasitesi;
}

// ElektrikliAraba'nın prototype'ını Araba'dan türetelim
ElektrikliAraba.prototype = Object.create(Araba.prototype);
// constructor özelliğini doğru şekilde ayarlayalım
ElektrikliAraba.prototype.constructor = ElektrikliAraba;

// ElektrikliAraba'ya özgü sarjDurumu metodunu ekleyelim
ElektrikliAraba.prototype.sarjDurumu = function() {
    console.log(`Batarya Kapasitesi: ${this.bataryaKapasitesi}`);
};

// Araba'nın mesaj metodunu override (geçersiz kıl) edelim
ElektrikliAraba.prototype.mesaj = function() {
    console.log(`Arabanız: ${this.arabaModeli}, Yaşı: ${this.arabaYasi}, Rengi: ${this.arabaRengi}, Batarya Kapasitesi: ${this.bataryaKapasitesi}`);
};

// Örnek bir nesne oluşturalım
const elektrikliAraba = new ElektrikliAraba("Tesla Model S", 1, "Beyaz", "100 kWh");
elektrikliAraba.mesaj();
// 'Arabanız: Tesla Model S, Yaşı: 1, Rengi: Beyaz, Batarya Kapasitesi: 100 kWh'
elektrikliAraba.sarjDurumu();
// 'Batarya Kapasitesi: 100 kWh'
```

Burada, `Araba` constructor'ından temel özellikleri `call()` ile çağırdık ve `ElektrikliAraba` fonksiyonunun prototype'ını `Object.create()` yöntemiyle `Araba.prototype`'tan türettik. Bu sayede `ElektrikliAraba` nesnesi, hem `Araba`'dan gelen metotları hem de kendi eklediğimiz yeni metotları kullanabilir hale geldi. Son olarak, `ElektrikliAraba.prototype.mesaj` tanımlayarak üst sınıftaki `mesaj` metodunu override ettik. Şimdiyse, önemli kavramlardan sonuncusunu öğrenerek yazımızı tamamlayalım: Prototype Chain

### Prototype Chain Nedir?

JavaScript'te her nesne, bir başka nesneyi "prototip" olarak referans alır. Bir nesnede aradığımız bir özellik veya metot bulunamazsa, JavaScript **prototype zinciri** (prototype chain) boyunca yukarı doğru bakmaya devam eder. Bu zincirin en tepesinde `Object.prototype` vardır ve o da `null` referansı alır. Fakat daha basit bir şekilde anlamak için Lexical Scope’u öğrenmeniz daha faydalı olacaktır ve bununla alakalı öncelikle aşağıdaki yazıyı okumanızı tavsiye ederim:

%[https://www.freecodecamp.org/news/javascript-lexical-scope-tutorial/] 

Şimdiyse basit bir örnek yapalım:

```javascript
"use strict"
// Üst seviye bir obje oluşturalım
const hayvan = {
    yasiyor: true,
    hareketEt() {
        console.log("Hayvan hareket ediyor");
    }
};

// Bu objeyi temel alarak Object nesnesi ile bir köpek objesi oluşturalım
const kopek = Object.create(hayvan);
kopek.havla = function() {
    console.log("Hav hav!");
};

console.log(kopek.yasiyor); 
// true
kopek.hareketEt();          
// "Hayvan hareket ediyor"
kopek.havla();             
// "Hav hav!"
```

Fark ettiğiniz üzere köpek nesnesinde doğrudan `yasiyor` özelliği yoktur çünkü onu tanılmadık. Fakat JavaScript, prototype chain sırasına göre, önce köpek nesnesinin içinde bakıyor, bulamayınca köpek nesnesinin inheritance olarak aldığı prototipine **yani hayvan nesnesine bakıyor** ve orada buluyor. `hareketEt()` fonksiyonuna da aynı şekilde köpek içinde bakıyor, bulamazsa hayvan nesnesine gidiyor.

Prototype zincirinin işleyişi de bu şekildedir. Sonraki yazımda görüşmek üzere!

## Kaynaklar

%[https://javascript.info/class-inheritance] 

%[https://javascript.info/prototype-inheritance] 

%[https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Inheritance_and_the_prototype_chain] 

%[https://developer.mozilla.org/en-US/docs/Learn_web_development/Extensions/Advanced_JavaScript_objects/Object_prototypes]