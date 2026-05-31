---
title: "JavaScript'te "reduce()" fonksiyonu ile diziler üzerinde iterasyon/döngü yapmak"
datePublished: 2026-05-31T16:39:20.715Z
cuid: cmpu08pzb000b1spt3i90df49
slug: javascript-te-reduce-fonksiyonu-ile-diziler-zerinde-iterasyon-d-ng-yapmak
cover: https://cdn.hashnode.com/uploads/covers/684d7cffa7664b2f13d48092/7df81cde-e0c3-4b2b-ae34-949433b66236.png
tags: javascript, reduce

---

JavaScript'teki çok güçlü bir *Array* (dizi) döngüsü yaratmaya yarayan gömülü bir fonksiyon olarak `reduce()` (öyle ki, React'te *state management*'ta, derleyicilerde, *parser*'larde asenkron *pipeline* görevlerinde ve daha pek çok yerde sıkça kullanılıyor) ilk başta `map()` veya `filter()` gibi daha basit bir söz dizimi olan fonksiyonlar yerine daha korkutucu gibi gelse de, mantığını kavradığınızda oldukça etkili iterasyonlar/döngüler için kullanılabilir.

[MDN'in](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce) sayfasından da görebileceğiniz üzere `reduce()`:

> " ...bir dizi (Array) üzerinde; **soldan sağa sıralı olarak** bir *callback* fonksiyonunu çalıştırarak, her döngüde bir önceki sonucu (akümülatör) ile geçerli elemanı matematiksel hesaplamalara göre işler ve her bir elemandan yapılan sonuç değeri tek bir değer olarak indirgeyerek döndürür."

Bunu daha basitçe anlamak için aşağıdaki diyagrama bakabilirsiniz:

![](https://cdn.hashnode.com/uploads/covers/684d7cffa7664b2f13d48092/d2ee75d8-1aef-4a0a-9663-fea941948200.jpg align="center")

`reduce()` fonksiyonu, girdi olarak aldığı bir dizi yapısındaki her bir eleman üzerinde *indis* numarasına göre bir callback fonksiyonu (o eleman üzerinde işletilen fonksiyon) döngü yaparak her bir eleman üzerinde, ondan sonraki ile yapılacak herhangi bir matematiksel işlem için işleyerek geriye tek bir sayıya indirgenmiş çıktı döndürür.

Burada dikkat edilmesi gereken bazı belirli noktalar vardır:

1.  Çıktı olarak gösterebileceği değer bir dizi, sayı, metin ya da nesne olabilir.
    
2.  Varsayılan olarak bir başlangıç değeri alması tavsiye edilir. Eğer bu sağlanmazsa, girdi olarak verilen dizinin ilk elemanı esas alınır.
    

Gelin şimdi de bunu kod üzerinde detaylıca işleyelim:

## reduce() Basit Söz dizimi & Kullanımı

```javascript
// reduce() syntax'i

const arr = ["merhaba", "dünya", "burası", "mars"];

/**
* @param akumulator | Önceki işlemin sonucu
* @param simdikiDeger | İşlem yapılan geçerli değer
* @param indis | Geçerli elemanın index numarası
* @param dizi | Orijinal dizi (opsiyonel)
*/

arr.reduce((akumulator, simdikiDeger, indis)=> {
    console.log(`Eleman indisi: ${indis}: "${akumulator}" + "${simdikiDeger}"\n`);
    return akumulator + " " + simdikiDeger;
});

// Çalışan döngü:
'Eleman indisi: 1: "merhaba" + "dünya"
' 'Eleman indisi: 2: "merhaba dünya" + "burası"
' 'Eleman indisi: 3: "merhaba dünya burası" + "mars"
'
// Nihai çıktı:
"merhaba dünya burası mars" 
```

`reduce()` fonksiyonun en güzel yanı, `map()` gibi fonksiyonların aksine elemanları **bağımsız değerlendirmemesi**, işlem sonucunu bir diğer elemana aktarabilecek hatırlama yeteneğine sahip olmasıdır. Nihayetinde `reduce()`, iterasyona tabi tutarak dönüştürdüğü verileri, tahmin edilebilir bir **state** **akışı içinde toplayarak yönetir.**

## reduce() ile Pratik Örnekler

### Array flattening (Dizi düzleştirme)

Özellikle iç içe geçmiş dizilerde (nested array) değerleri düzleştirmek için kullanılabilir:

```javascript
const nestedArr = [[1, 2], [3, 4], [5, 6]];

const flatArr = nestedArr.reduce((acc, elemanlar) => {
    return acc.concat(elemanlar);
}, []); // Varsayılan olarak boş bir [] array belirttik, böylece sağladığımız nestedArr dizisinin ilk elemanını baz almayalım

console.log(flatArr);

// Çıktı: [1, 2, 3, 4, 5, 6]
```

### Nesne Dönüştürme (Array to Object)

*Array objects* (dizi nesneleri) olarak tanımlanan veri türünden, örneğin, *JSON* formatında okunup kaydedilebilecek verilere dönüştürülmesi için de oldukça kullanışlıdır.

```javascript
const kullanicilar = [
    { id: 1, ad: "Ali" },
    { id: 2, ad: "Ayşe" },
    { id: 3, ad: "Mehmet" }
];

const kullaniciNesnesi = kullanicilar.reduce((acc, kullanici) => {
    // akümülatöre dot notation ile istediğimiz özelliği ekleyelim
    acc[kullanici.id] = kullanici.ad;
    return acc;
}, {});

console.log(kullaniciNesnesi);

// Çıktı: { '1': 'Ali', '2': 'Ayşe', '3': 'Mehmet' }
```

### Dizi Elemanlarını Sayma (Frequency Count)

```javascript
const renkler = ["kırmızı", "mavi", "kırmızı", "yeşil", "mavi", "kırmızı"];

const renkSayisi = renkler.reduce((acc, renk) => {
    acc[renk] = (acc[renk] || 0) + 1; // Her iterasyonda rengin aynı olup olmadığını kontrol ederek aynı ise bir artırır ve kaydeder...
    return acc;
}, {});

console.log(renkSayisi);

// Çıktı: { kırmızı: 3, mavi: 2, yeşil: 1 }
```

### Gruplandırma (Grouping)

```javascript
const ogrenciler = [
    { ad: "Ali", sinif: "9A" },
    { ad: "Ayşe", sinif: "9B" },
    { ad: "Mehmet", sinif: "9A" },
    { ad: "Fatma", sinif: "9B" }
];

const ogrenciGruplari = ogrenciler.reduce((acc, ogrenci) => {
    const sinif = ogrenci.sinif;
    if (!acc[sinif]) {
        acc[sinif] = [];
    }
    acc[sinif].push(ogrenci.ad);
    return acc;
}, {});

console.log(ogrenciGruplari);

// Çıktı:
// {
//   '9A': ['Ali', 'Mehmet'],
//   '9B': ['Ayşe', 'Fatma']
// }
```

Başka bir yazımda görüşmek üzere!