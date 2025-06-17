---
title: "TypeScript ile Kütüphane Kullanmadan Class-based Component (Sınıf Tabanlı Bileşen) Oluşturmak"
seoTitle: "TypeScript ile Kütüphane Kullanmadan Class-based Component (Sınıf Taba"
seoDescription: "React gibi popüler kütüphaneleri kullanmaya gerek duymadan yeniden kullanılabilir (reusable) bileşenleri (components) oluşturuyoruz."
datePublished: Tue Jun 17 2025 17:13:00 GMT+0000 (Coordinated Universal Time)
cuid: cmc0s7kr4000e02l522kdh8ws
slug: typescript-ile-kutuphane-kullanmadan-class-based-component-sinif-tabanli-bilesen-olusturmak
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1750180357523/ce891bab-16b4-43e3-bcaa-eacbd14444b6.png
tags: typescript, web-components

---

Temelinde modern arayüzler geliştirmenin modern standartlarından biri olan **yeniden kullanılabilir bileşenlerin (reusable components)** amacı, bir kere yazılan ve genellikle tarayıcıda *render* edilen bir HTML elementini barındıran kodun, birden fazla, istenilen her yerde kullanılmasını ve dinamik olarak manipüle edilmesini sağlamak, yönetilmesini kolaylaştırmaktır. Bileşenleri kullanmak ve bileşen mantığında çalışmak, geliştiriciler için son derece kolaylık sağlar ve geniş çaplı projelerde çalışılmasını, projenin kullandığı yapı dolayısıyla daha verimli kılar. Bunu, günümüzün en popüler kütüphanelerinden biri olan [React’in sayfasında](https://tr.react.dev/learn#components) okuyoruz:

> React uygulamaları *bileşenlerden* oluşur. Bileşen kendi mantığına ve görünüşüne sahip **bir UI (kullanıcı arayüzü) parçasıdır.** Bir bileşen bir buton kadar küçük ya da tam bir sayfa kadar büyük olabilir.

Fakat, her zaman React gibi kapsamlı kütüphaneleri kullanmak istemeyebiliriz! Çünkü projemizin ihtiyaçları doğrultusunda kullanmak istediğimiz altyapı basitten orta zorluğa kadar çeşitlilik gösterebilir veya tamamen kişisel tercihlerimiz doğrultusunda herhangi ekstra bir kütüphane kullanmaktan kaçınabiliriz. İşte tam bu noktada, JavaScript’in güçlü bir ön işlemcisi *(preprocessor)* olan **TypeScript** bize yardıma koşuyor. TypeScript, yerel çözümler vasıtasıyla, React gibi kütüphanelere bağımlı kalmadan **sadece TypeScript kullanarak** kendi bileşenlerimizi yazabilmemize olanak tanır.

## TypeScript’te Bileşen Taslağı Oluşturmak

React veya Vue gibi bir framework kullanmadan, yalnızca tarayıcının yerel [**Web Components API**](https://developer.mozilla.org/en-US/docs/Web/API/Web_components)**'leri (Custom Elements ve Shadow DOM gibi)** ile TypeScript'in gücünü birleştirerek kendi yeniden kullanılabilir bileşenlerimizi oluşturabiliriz.

Bu yaklaşım, özellikle performansın kritik olduğu, *bundle* boyutunun minimumda tutulmak istendiği veya mevcut bir projenin parçası olarak bağımsız, hafif UI öğeleri eklenmesi gereken senaryolarda idealdir. TypeScript, bileşenlerimizin beklediği prop'ları (özellikleri) tanımlamamıza, bileşenler arası iletişimi güvenli bir şekilde yönetmemize ve olası hataları çalışma zamanından önce tespit etmemize olanak tanır.

Basit bir sınıf taslağını `mycomponent.ts` isimli bir dosya içinde oluşturarak başlayalım:

```typescript
// mycomponent.ts
class MyComponent extends HTMLElement {

}
```

Sınıfımızı tanımladıktan sonra, `extends` anahtar kelimesiyle sınıfımızın hangi temeli genişlettiğini açıkça belirtiriz. Bu örnekte, `HTMLElement` sınıfını genişletiyoruz. `HTMLElement`, tarayıcının tüm temel HTML elementlerinin (örneğin `<div>`, `<p>`, `<span>`) *root’u* yani kökü olan yerleşik bir JavaScript arayüzüdür. Kendi sınıfımızı `HTMLElement`'dan türeterek, ona bir HTML elementi gibi davranma yeteneği kazandırıyoruz ve daha sonra detaylıca değineceğimiz, **Web Components standardının** bize sunduğu **yaşam döngüsü metotlarına** (örneğin `connectedCallback`, `disconnectedCallback`, `attributeChangedCallback`) erişim sağlıyoruz. Bu sayede, tarayıcıda kendi **özel etiketimizi** (örneğin `<my-component>`) tanımlayabilir ve tıpkı yerleşik bir HTML elementi gibi kullanarak kendi özel bileşenimizi oluşturmuş oluruz.

Bu basit sınıf tanımı, özel bir HTML elementi oluşturmak için atılan ilk adımdır. Ancak bu elementi tarayıcının tanıması ve kullanabilmesi için henüz yeterli değildir. Sınıfımızı tanımladıktan sonra, onu tarayıcının **"özel elementler" kaydına** eklememiz gerekir. Bu kayıt işlemi, `customElements.define()` metodu ile gerçekleştirilir:

```typescript
// mycomponent.ts
class MyComponent extends HTMLElement {

}

customElements.define("my-component", MyComponent);
```

Böylece bileşenimizi tarayıcıya tanıtmış olduk. Şimdi ise modern **ES6 standardına** dayanan `import` anahtar kelimesi ile projemize entegre etmek için, eğer aynı dosyada birden fazla bileşeni dışa aktaracaksanız, `class` tanımının önüne `export` anahtar kelimesini ekleyebilirsiniz. Tek bir bileşeni dışa aktaracaksanız, `export default` kullanarak daha kolay bir tanımlama yapabilirsiniz:

```typescript
// Eğer birden fazla bileşen aynı dosyada mevcut ise tek tek export ile yapılabilir.
export class MyComponent extends HTMLElement {

}

// Eğer tek bileşen export edilecekse, daha kolay bir tanımlama yapılabilir:
export default MyComponent;
customElements.define("my-component", MyComponent);
```

Artık bileşenimiz hazır ve üzerinde çalışmak için uygun bir ortama sahibiz. Bunu, app.ts gibi uygulamamızın kök dosyasından şu şekilde import edebiliriz (genellikle [**Webpack**](https://webpack.js.org/configuration/resolve/) gibi bir bundler kullanıldığı varsayılmıştır):

```typescript
// App.ts
import {MyComponent} from "mycomponent";
// Eğer Webpack ile .ts veya .js uzantısını resolve edecek bir modül yönetim sistemi kullanmıyorsanız
import {MyComponent} from "mycomponent.ts";
```

Daha sonraki yazılarımda bileşenlerin **yaşam döngülerine**, **Light DOM ve Shadow DOM arasındaki farklara** ve diğer pek çok önemli detaya değineceğim.

Görüşmek üzere!