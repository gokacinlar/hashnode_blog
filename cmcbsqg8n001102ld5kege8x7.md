---
title: "TypeScript'te Light DOM ve Shadow DOM kullanımı ve farkları"
seoTitle: "TypeScript'te Light DOM ve Shadow DOM kullanımı ve farkları"
seoDescription: "Document Object Model'in alt dallarını teşkil eden ve bileşenlerin uygulanmasıyla ilgili Light DOM ve Shadow DOM'un farklılıklarını tartışıyoruz."
datePublished: Wed Jun 25 2025 10:13:09 GMT+0000 (Coordinated Universal Time)
cuid: cmcbsqg8n001102ld5kege8x7
slug: typescriptte-light-dom-ve-shadow-dom-kullanimi-ve-farklari
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1750846314687/eac88c07-18b9-4f44-899a-14336342118c.png
tags: dom, typescript, shadowdom, domtree, lightdom

---

[Geçen yazımda](https://gokacinlar.hashnode.dev/typescript-ile-kutuphane-kullanmadan-class-based-component-sinif-tabanli-bilesen-olusturmak) TypeScript’te, React gibi kütüphaneler kullanmadan nasıl bileşen (component) oluşturabileceğimizi ve bunları web projelerimizde nasıl kullanabileceğimizi araştırmıştık. Şimdiki yazımızdaysa, bu bileşenleri tarayıcıya tanıtmada kullanabileceğimiz iki farklı yöntemi, yani *Light DOM* ve *Shadow DOM’u* tanıtıp tartışacağız.

## Light DOM (Aydınlık/Açık DOM) nedir?

Light DOM, Türkçesi ile “Aydınlık" veya daha tutarlı bir tabirle “Açık” DOM, web bileşenlerimizde kullanıldığı halde, yapısı itibariyle onları projede küresel halde erişilebilir kılan ve örneğin, *globals.css* gibi projemizde kullandığımız ana CSS dosyasından, bileşen içeriğimizde stilleme yapmadan tanımlanan herhangi bir kuralın aynen geçerli olduğunu betimleyen bir DOM implementation modelidir. Esasında web bileşenlerimizde, Shadow DOM kullanılmadığı takdirde varsayılan olarak kullanılan bir tekniktir. Yani Light DOM, sanki eski usul HTML’i aynen yazıyormuşsunuz gibi, tarayıcı tarafından yorumlanabilmesini sağlar ve herhangi ekstra bir konfigürasyon istemez. Bu şekilde, Light DOM haliyle tanımlanan bileşenimize, global bir erişim ve manipülasyon edilebilme yeteneği kazandırırız. Örneğin bunu, aldığı değeri yansıtan basit bir HTML *Heading (H1)* elementini bileşen şeklinde oluşturarak yazalım:

```typescript
// component.ts
class TitleHeading extends HTMLElement {
     // constructor metodu ile, bir sınıfı initialize etmiş oluruz
    constructor(title: string) {
        // super() metodu ile, sınıfımızın HTMLElement ile oluşturulmuş (extends'e dikkat edin)
        // yapısına erişiyoruz. Bu konuya "Inheritance" yazımda daha detaylı değineceğim
        super();
        // Yazdığımız son derece basit fonksiyonu constructor'da çağırarak initialize etmiş oluruz
        this.createTitle(title);
    }

    createTitle(title: string): string {
        // Değer olarak döndürdüğümüz başlık yazısını tanımlayalım
        return title;
    }
}
customElements.define("title-heading", TitleHeading);

// Bileşenimizi, uygulamamızın başka bir kısmında import ederek kullanabiliriz:
// app.ts
import {TitleHeading} from "component.ts";

new TitleHeading("Merhaba Dünya!");

// Örn: Template String içerisinde kullanılabilir:
public ornekTemplateString():string {
    return `
        <div>
            ${new TitleHeading("Merhaba Dünya!")}
            // bileşen içeriğinin gerisi
        </div>
    `;
}
```

### HTML’de gösterimi:

```xml
<div>
    <title-heading></title-heading>
</div>
```

Bahsetmem gerekir ki, bu haliyle bileşenimiz tamamıyla gereksizdir çünkü H1 elementiyle yazmak istediğimizi basitçe yazarız fakat bileşenleri, tabiri caizse modifiye etmeye başladıkça (H1’e dinamizm kazandırma, etkileşim ögeleri etkileme vs.), saf HTML ile yazılan artık angarya olarak gelmeye başlayacaktır. Burada tanımladığımız elemente bir `.baslık` class’ı atayarak stil verdiğimizde, bu element şekil alacaktır çünkü global olarak projemizdeki herhangi bir dosyadan erişilebilirdir. Tarayıcıya Light DOM ile bu şekilde tanıtıldığında hemen her yerden erişilebilir olacaktır. Bunu, Shadow DOM’da bahsedeceğimiz **Encapsulation** ve **Scope** kavramlarıyla uğraşmak yerine, özel bir uğraş gerektirmeyen bileşenlerinizde kullanabilirsiniz.

## Shadow DOM (Gölge DOM) Nedir?

[Shadow DOM](https://developer.mozilla.org/en-US/docs/Web/API/Web_components/Using_shadow_DOM), bir bileşenin iç yapısını — stilini, davranışını ve içeriğini — ana dokümandan ayırarak korumaya yarayan özel bir DOM modelidir. Bu sayede bileşeninize ait stil ve fonksiyonlar dışarıdan etkilenmez; **bileşen, dış dünyadan izole bir ortamda çalışır**. Aşağıdaki grafikte de gösterileceği üzere, dokümanın akışından koruyarak kapsül şekilde tutma **(encapsulate)** ve izole ederek ayrıştırmak üzerine oluşturulmuş bir yapıdır. Encapsulation, **kısaca web bileşenlerimizde tanımladığımız değişken ve verilerin başka kaynaktan değiştirilmesini engellemek üzerine kullandığımız bir tanımdır ve Shadow DOM ile birlikte gelir.** Temel amacı, web bileşenlerinizin iç yapısını, stilini ve davranışını esas dokümandan *(root)* koruyarak, bağımsız ve yeniden kullanılabilir birimler haline getirmektir:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750796627671/870456d1-3986-4dd9-8c4b-e5e795162e1a.png align="center")

Bu işte çoğu ama çoğu zaman, kullanacağımız Shadow DOM bileşeninin, projemizdeki diğer CSS stillerinin çakışmasından, fonksiyon ihtilaflarından veya düzeninden ayrı tutup engellemek için kullanıldığını bilmenizde yarar var çünkü, sanki bu bileşeni adı üzerinde gölgeliyormuşuz gibi, projemizdeki diğer elementler ve fonksiyonlarla olası uyuşmazlığından korumak için yaparız ve onu kapsülleyerek (encapsulation) ve projeden izole ederek (Isolation) belgeyi ve bileşenimizi aynı anda koruyarak, sanki kendi başına bir ögeymiş gibi esas projemizde apayrı bir element olarak kullanırız. Bunu, ya bu mantıkta çalışılan bir projede (örneğin TailwindCSS ile çalışılıyorsa mantıklı olabilir veya ) veya projenin ihtiyaçları doğrultusunda başvurulmasıyla kullanıldığı olur. Shadow DOM, kısaca bağımsız bir değişkendir. Şimdi yukarıdaki fonksiyonumuzu Shadow DOM kullanarak yazalım fakat önce, Shadow DOM için gereken birkaç parametreyi girmemiz gerekiyor:

1. `this.attachShadow` metodu ile, oluşturduğumuz bileşen içeriğini, dokümandaki hedef elemente eklemeliyiz.
    
2. `appendChild` metodu ile, oluşturduğumuz bileşeni, attachShadow ile yüklenecek yapıya eklememiz gerekiyor
    

### Örnek: Shadow DOM ile Aynı Bileşeni Oluşturmak

```typescript
class TitleHeading extends HTMLElement {
    // Private keyword ile bir değer tanımlayalım
    private shadow: ShadowRoot;

    constructor() {
        super();
        // Constructor'da, shadow DOM bileşenimizi initialize edelim.
        // Buradaki "open" parametresi, sayfamız yüklendiğinde bu shadow dom elementinin
        // otomatik olarak gözükeceği anlamına gelir. "closed" ise bunun tersidir.
        this.shadow = this.attachShadow({ mode: "open" });
    }

    // Element DOM'a eklendiğinde çalışmasını sağlayacak connectedCallback fonksiyonu içinde
    // kalan işlemleri yapacağız. Bu ve diğer life-cycle eventlere (yaşam döngüsü olayları)
    // bundan sonraki yazımda değineceğim.
    connectedCallback() {
        // Create heading element inside shadow root
        const heading = document.createElement("h1");
        heading.textContent = content;
    
        // NOT: Bu bileşenimiz global CSS kurallarından etkilenmeyeceğinden, bileşen
        // içinde tanımlama yapmamız gerekiyor. Onun için bir style elementi oluşturup
        // stillerimiz yazalım:
        const style = document.createElement("style");
        style.textContent = `
          h1 {
            color: #00000;
            font-size: 1.25rem;
            font-weight: bold;
          }
        `;
    
        // Oluşturduğumuz her iki ögeyi bileşene ekleyelim
        this.shadow.appendChild(style);
        this.shadow.appendChild(heading);
      }
    }


// Bileşeni taraycıya tanıtalım
customElements.define("title-heading", TitleHeading);
```

```typescript
<title-heading title="Gizli Başlık"></title-heading>
```

Bu bileşen, kendi Shadow DOM kökü içinde tanımlandığı için dışarıdan erişilemez ve dıştan gelen CSS kuralları etkili olmaz. Yani, örneğin `globals.css` dosyanızda `.baslik { color: red; }` gibi bir kural varsa, bu bileşen içinde **uygulanmaz**. Bileşen sadece **kendi içerisinde ona tanımlanan kurallara tabidir.**

Diğer yazımda **Inheritance (miras alma)** ve diğer alakalı birkaç konuya değineceğim. Görüşmek üzere!