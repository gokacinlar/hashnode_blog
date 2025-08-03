---
title: "JavaScript'te Number() ve parseInt() metotlarının farkları ve performansı"
seoTitle: "Number vs parseInt: Farklar ve Performans"
seoDescription: "JavaScript'te parseInt() ve Number() metotlarının farklarını, kullanım alanlarını ve performanslarını öğrenin. İşte detaylar ve örnekler!"
datePublished: Sun Aug 03 2025 16:55:46 GMT+0000 (Coordinated Universal Time)
cuid: cmdvxafom000502l1ddan40g9
slug: javascriptte-number-ve-parseint-metotlarinin-farklari-ve-performansi
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1754240102437/3c0608a3-1470-48b0-9633-d79c5d23cacc.png
tags: javascript, number, parseint

---

## “parseInt()” Nedir?

Temelinde herkesin aşina olduğu `parseInt()` fonksiyonu, verilen `string`’in inputunu tarayarak herhangi bir **ilk rakamı geri döndürerek**, adı üzerinde **parse yaparak** istediğimiz veriyi, JavaScript’in *primitive type*’larından olan Number’a ait Integer, yani ondalık sayı türünde geri döndürür. Hemen bir örnekle pratikleştirelim:

```javascript
"use strict"

// İçinde rakam olan herhangi bir string
const string = "56734123enABCDqwerty678";
const result = parseInt(string);
console.log(result);
// 56734123
```

Dikkat edeceğiniz üzere, `parseInt()` fonksiyonu, bir string içerisinde rakamları ararken, *type* olarak ilk denk geldiği *string* elemanında return yapar ve string’imizin sonundaki 678 rakamını konsola vermez. **Bundaki tek istisna**, ***hexademical* (onaltılık) sayı biriminde olan rakamları da parse yapabilmesidir.**

```javascript
"use strict"

const string = "7en2";
for(let i = 0; i < str.length; i++) {
  console.log(parseInt(str[i]));
  // 7 NaN NaN 2
}
```

Gördüğümüz gibi, string’in elemanları içerisinde iterate ettiğimiz bir for döngüsü içerisinde denk gelinen string karakterler, `parseInt()` tarafından *NaN (Not a Number)* şeklinde tanımlanmaktadır.

parseInt() her zaman integer bir değer döndürür, sayı olmayan bir değer ile karşılaştığında kendini sonlandırır ve kullanım alanları, tahmin edebileceğiniz üzere string verilerden sayı çıkartmak veya ikinci parametresi olan `radix` (opsiyoneldir) değeri ile belirtilen aşağıdaki sayı tiplerinden birine göre dönüştürmek için kullanılır:

* **10:** Onluk sistem (Decimal)
    
* **16:** Onaltılık sistem (Hexademical)
    
* **2:** İkilik sistem
    

```javascript
"use strict"

const decimal = "567";  // Ondalık
const binary = "1011";  // İkilik
 
// Ondalıktan -> Onaltılığa
const hex = parseInt(decimal, 10).toString(16);  // Onluk sayıyı onaltılığa çeviriyoruz
console.log(hex);  
// "237"

// Binary'den -> Ondalıklığa
const result = parseInt(binary, 2);  // Binary'den ondalığa dönüşüm
console.log(result);  
// 11
```

## “Number()” Nedir?

`Number()` ise JavaScript’te global olarak erişilebilen bir nesnedir ve harika birkaç (isFinite, isInteger, isNan) metodu bulunur ve bunlar, özellikle JavaScript’te cehennem olan `Date` ile çalışanlar için işe yarar (zaman aralıklarıyla çalışmak, tarih *timestapler* veya zaman aralıklarını hesaplamak gibi). `Number()`, *primitive* bir sayı tipi döndürür; `parseInt()` metodunun aksine geriye *integer* değil, verilen input’taki herhangi bir “sayısal değeri” döndürür. Yani `Number()` fonksiyonu, verilen değeri sayıya dönüştürmeye çalışır ve dönüşüm başarılı olursa o değeri primitive sayı olarak döndürür. Eğer dönüşüm yapılamazsa, **NaN (Not a Number) döndürür.** Hemen örneklerle pratikleştirelim:

```javascript
"use strict"

// Sayıya dönüştürülebilecek değer adayımız olan string'e bakalım, Number() dönüştürebilecek mi?
const string = "123.45";
const num = Number(string);
console.log(num);  
// 123.45 (DÖNÜŞTÜRÜYORMUŞ)
// Ayrıca metotlarıyla da bir bakalım bu sayı NaN'mıymış?
const isNan = Number.isNaN(num);
console.log(isNan);
// false
```

## İki metotun hız farklarını karşılaştırmak

JavaScript’teki `Number` metodu, aslında bir [primitive wrapper’ıdır.](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number#constructor) Bu yüzden `parseInt()` gibi metotlardan daha yavaştır. Bu ikisi arasındaki hızı, hem V8, hem SpiderMonkey, hem de Node.js JavaScript motorları üzerindeki testleriyle görebilirsiniz. Bunun için aşağıdaki testi kullandım:

```javascript
"use strict";

// Test edilecek sayılar
const testData = [
  "12345",
  "67890",
  "9876543210",
  "1.234567", // Float
  "5.67890" // Float
];

const iterationCycle = 1000000;
function testPerformance(func, time) {
  console.time(time);
  for (let i = 0; i < iterationCycle; i++) {
    for (let j = 0; j < testData.length; j++) {
      func(testData[j]);
    }
  }
  console.timeEnd(time);
}

// Number()
testPerformance(Number, "Number() Sonucu");

// parseInt()
testPerformance((input) => parseInt(input, 10), "parseInt() Sonucu");
```

Bu testing JavaScript motorlarına göre karşılığı aşağıdaki gibidir:

| **JavaScript Motorları** | **Hız (MS cinsinden)** |
| --- | --- |
| **\- V8 (Chrome)** | Number: 119.189ms parseInt(): 115.985ms |
| \- **SpiderMonkey (Firefox)** | Number: 105ms parseInt(): 94ms |
| \- **Node.js (22+)** | Number: 256.721ms parseInt(): 190.071ms |

Her zaman genç kalmanızı ve webde harikalar yaratmanızı dilerim!