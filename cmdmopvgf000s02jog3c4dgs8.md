---
title: "JavaScript'te map() fonksiyonu ile dinamik verilerle çalışmak (JSON, Diziler)"
seoTitle: "Dinamik JSON ve Dizi İşleme: map() Kullanımı"
seoDescription: "Discover how to use JavaScript's `map()` function for dynamic data manipulation with arrays and JSON for effective DOM interaction."
datePublished: Mon Jul 28 2025 05:45:54 GMT+0000 (Coordinated Universal Time)
cuid: cmdmopvgf000s02jog3c4dgs8
slug: javascriptte-map-fonksiyonu-ile-dinamik-verilerle-calismak-json-diziler
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1753681495097/d2de1925-ff99-40f0-a540-8127958b1439.jpeg
tags: javascript, json, arrays, map-in-js

---

JavaScript’te `map()` fonksiyonu, temel kullanımı itibariyle dizilerle çalışmamızı, dizilerdeki elemanların *iterable* olarak istenilen fonksiyonla farklı sonuçlar çıkartılarak zenginleştirilmesini ifade eder. Bunu, her elemana, yazılan *callback* fonksiyonu ile gerçekleştirir.

## map() & Arrays

`map()` fonksiyonunun dizilerle alakalı iki örneği hemen yazalım:

```javascript
"use strict"
// 5 rakamına kadar olan sayılarımızın bulunduğu bir dizi tanımladık.
const arrayOne = [1,2,3,4,5];
// SENARYO: Bunların üssünü alacak bir map fonksiyonu yazalım.
const arrayTwo = arrayOne.map((val) => {
    return val = val * val;
})
console.log(arrayTwo);
// [ 1, 4, 9, 16, 25 ]

// Eğer istenirse, Math.pow() fonksiyonu da kullanılarak yazılabilir tabii ki.
// const arrayTwo = arrayOne.map(val => Math.pow(val, 2));
```

Gördüğünüz üzere `map()` fonksiyonun parametresi olarak, *iterable* olan `arrayOne` isimli dizimizin elemanlarını temsil edecek **val** değerini girdik ve matematiksel işlemimizi yazdık.

`map()` fonksiyonunda dikkat edilecek en önemli şey, `map()` fonksiyonuyla elde edilen çıktının **yeni bir dizi (Array) olmasıdır**. `map()` fonksiyonu, aksi belirtilmedikçe **her zaman girdi olarak aldığı diziden yeni bir dizi çıkartır.** Ve **mutlaka bir** `return` **belirtilmesi gerekir**, aksi takdirde `undefined` elde ederiz. Gelin bir başka örnek daha yapalım:

```javascript
"use strict"
const arrayOne = ["Ali", "Veli", "Melisa"];
// SENARYO: İsimleri merhaba ile karşılayan bir map fonksiyonu yazalım
const arrayTwo = arrayOne.map((val)=> {
    return `Merhaba: ${val}`;
})

console.log(arrayTwo);
// [ 'Merhaba: Ali', 'Merhaba: Veli', 'Merhaba: Melisa' ]
```

Burada ise isimleri merhaba ile dolduran bir map() fonksiyonunu, [template literals](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals) yardımıyla yazmış olduk.

## map() & JSON

`map()` fonksiyonuyla pek çok Array manipulation örneği gerçekleştirebilir fakat bunu, nesneler ve JSON gibi ögelerle de gerçekleştirebiliriz. Hele bir de JSON’dan veri alarak dinamik bir şekilde DOM Manipulation örneği gerçekleştirmek istiyorsak, map() fonksiyonu işimize çok yarayabilir. Aşağıda fuzuli bir JSON verisi oluşturduk:

```javascript
const data = [
    {
        "id": 1,
        "name": "Alice",
        "age": 28
    },
    {
        "id": 2,
        "name": "Bob",
        "age": 34
    },
    {
        "id": 3,
        "name": "Charlie",
        "age": 22
    }
];
```

Şimdi bu JSON verisinden DOM için bir &lt;ul&gt; elementi, yani liste ve liste itemlerini oluşturalım:

```javascript
// Oluşturacağımız listeyi ekleyeceğimiz parent elementi seçelim
const container = document.getElementById("kullanıcıListesi");

// map() fonksiyonumuzla;
// 1) <ul> elementinin itemleri olan <li> elementini oluşturalım ve değer olarak
// 2) data verisinden, dot notation ile "id", "name" ve "yaş" isimli değerlerini alalım
const listItems = data.map(user => {
  const li = document.createElement("li");
  // Mümkün olduğunca innerHTML kullanmaktan kaçınalım
  li.textContent = `User ${user.id}: ${user.name}, Age: ${user.age}`;
  return li;
});

// <ul> elementi oluşturup 
const ul = document.createElement("ul");
listItems.forEach(li => ul.appendChild(li));

// Nihai ögemizi istediğimiz yere ekleyelim
container.appendChild(ul);
```

Şimdi de HTML’de bunu taşıyacak bir divi id’si ile oluşturalım:

```xml
<div id="kullanıcıListesi">
</div>
```

Nihai görüntümüz şuna benzeyecektir:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753680207921/1a3b6a9e-c4db-4902-8c44-681a73ba5d62.png align="center")

Daha sonraki yazımda görüşmek üzere!