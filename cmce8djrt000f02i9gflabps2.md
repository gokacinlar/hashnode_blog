---
title: "Webpack, TypeScript & Node.js (npm) ile Client-Side Web Projesi Taslağı Oluşturmak (2025) (DETAYLI)"
seoTitle: "Webpack, TypeScript & Node.js Modern Bir Web Projesi Taslağı Oluşturma"
seoDescription: "Kapsamlı bir rehberle Webpack, TypeScript ve Node.js kullanarak modern client-side web projesi oluşturmayı öğrenin."
datePublished: Fri Jun 27 2025 03:06:33 GMT+0000 (Coordinated Universal Time)
cuid: cmce8djrt000f02i9gflabps2
slug: webpack-typescript-and-nodejs-npm-ile-client-side-web-projesi-taslagi-olusturmak-2025-detayli
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1750985313951/612fae9f-d51f-4d04-bdd6-a8436562deb7.jpeg
tags: web-development, nodejs, webpack, typescript, client-side-rendering, bundling, 2025

---

Modern web projelerinin, hem arka-yüz (*back-end* veya *server-side*) hem de ön-yüz (*front-end* veya *client-side*) geliştirmede tercih edilen yöntemlerinden en popüler ve belki de standart haline gelmiş kabul edilen yolu, **Node.js** yazılımı ile (*basitçe tarayıcıda değil de makinede çalışan JavaScript*) birlikte gelen `npm` isimli, web üzerine ve dolayısıyla JavaScript üzerine çalışan geliştiricilerin yayınladığı kütüphaneler ve paketleri barındıran bir depo (*repository*) vasıtasıyla projeye entegre etmek istediğimiz her şeyi entegre edebildiğimiz yöntemi kullanmaktır. `Npm` vasıtasıyla neredeyse her bir kütüphaneye erişimimiz olmakla birlikte, başlığımızda gerekli olan Webpack ile TypeScript’i de buradan projemize entegre edeceğiz. Daha sonraki yazımda *server-side* ile ilgili yazacağım fakat ilk önce *client-side* mantığını anlamak daha temelli bir fikir edinmemizi sağlayacaktır. Şimdi kurulum ve gereksinimlerimizle başlayalım.

## TL;DR

GitHub reposuna ulaşabilirsiniz.

%[https://github.com/gokacinlar/webpack-ts-boilerplate] 

## Kurulum ve Gereksinimler

Öncelikle Node.js’i indirmemiz gerekiyor. Ben LTS (Long-term Support Release) tercih ediyorum çünkü benim için son sürümlerden ziyade stabil bir çalışma ortamı daha iyi. Windows için `winget` ile kısaca;

```powershell
winget install OpenJS.NodeJS.LTS # LTS sürümünü indirir
winget install OpenJS.NodeJS # En son sürümünü indirir
```

İndirip kurduktan sonra Windows Terminal’den, PowerShell’den veya herhangi bir terminal emülatöründen (Alacritty gibi), node -v yazarak yüklenip yüklenmediğini kontrol edebilirsiniz:

```powershell
Windows PowerShell
Copyright (C) Microsoft Corporation. All rights reserved.

Try the new cross-platform PowerShell https://aka.ms/pscore6

PS C:\Windows\system32> node -v
v22.16.0 # Node sürümünü görüyoruz ve yüklendiğini anlıyoruz.
```

**Linux** için kısaca `sudo apt-get install nodejs` veya **Mac** için `brew install node` ile indirebilirsiniz. Tabii [VSCode’u](https://code.visualstudio.com/) da unutmayalım. Burada IDE olarak onu kullanacağız.

## NPM Projeyi başlatmak

İstediğiniz bir klasörde projeyi başlatmak için öncelikle girmemiz gereken komut aşağıdaki gibidir:

```powershell
PS C:\Users\gokacinlar\Desktop\ornek-proje> npm init -y # Projemizi oluşturduk
```

Projemizin klasöründe oluşan `package.json` çıktısı şuna benzeyecektir:

```json
// package.json
{
  "name": "ornek-proje",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "type": "commonjs"
}
```

`Package.json`, kısaca projemizde kullanacağımız paketleri ve kütüphaneleri listeleyen, projenin temel tanıtıcı özelliklerini ve “scripts” ile projemizi derleyip çalıştırmak için kullanacağımız kısayollar atamamızı sağlayan kullanışlı bir araçtır ve projenin temel dosyalarından biridir. Şimdi TypeScript’i ve TypeScript kullanmamıza yardımcı olacak diğer paketleri indirelim:

## TypeScript’i İndirmek

```powershell
PS C:\Users\gokacinlar\Desktop\ornek-proje> npm i --save-dev typescript eslint
# eslint, TypeScript & JavaScript kodunuzu analiz eden kod yazmayı kolaylaştırıcı bir kütüphanedir
```

`--save-dev` parametresi ile, bunun **development modu**nda kullanılacak bir şey olduğunu belirtmemiz gerekiyor çünkü **TypeScript, JavaScript’in bir preprocessor’u olduğundan**, **üretimde (production) kullanılmaz.** Projemiz şimdi oluşan `node_modules` *(Adı üzerinden anlaşıldığı ile, node paketlerini barındıran klasördür)* ile şöyle bir görünüm aldı:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750987531006/b3c0e769-c613-433a-8247-952ea1e46fab.png align="center")

Şimdi TypeScript’i yüklediğimize göre, TypeScript derleyicisine ayar verebilmemiz için bir tsconfig.json dosyasına ihtiyacımız var ki, çıktıları JavaScript’e istediğimiz şekilde oluşturabilsin. Bunun için kısaca:

```powershell
PS C:\Users\gokacinlar\Desktop\ornek-proje> npx tsc --init 
# npx, npm'in bir parçasıdır ve TypeScript'i çalıştırmamızı sağlar.
# --init parametresi, initialize, yani başlatma prosedürüdr.
```

Bununla bir *tsconfig.json* dosyası oluşturduk ve şimdi bir sürü parametreye sahip içeriğine, Webpack ile çalışacağımızdan uygun ayarları yapmamız gerekiyor. Bunun için sadeleştirilmiş bir *tsconfig.json* dosyası hazırladım:

```json
{
  "compilerOptions": {
    "target": "ES6",
    /* ECMAScript sürümünü belirliyoruz. Bu genelde her zaman ES6'tir. */
    "module": "ES2015",
    /* Derlenen JS kodunun modül sistemini belirtir. 
       ES2015, import/export yapısını destekleyen ES modül sistemidir. */
    "moduleResolution": "node",
    /* node_modules klasöründen kütüphaneleri kullanmamız için gereklidir. */
    "esModuleInterop": true,
    /* Daha eski commonjs importların require() gibi, modern ES6 ile "import"
       çalışması için bir uyumluluk katmanıdır. */
    "allowSyntheticDefaultImports": true,
    /* Varsayılan export’u olmayan modülleri bile import something from "module"
       şeklinde import etmeye izin verir.*/
    "strict": true,
    /* TypeScript'in temeli olan type-checking için elzem bir ayardır. */
    "forceConsistentCasingInFileNames": true,
    "skipLibCheck": true,
    "resolveJsonModule": true,
    /* Projeye .json dosyalarını modül gibi import etme yeteneği kazandırır. */
    "isolatedModules": true,
    /* Her .ts dosyasının tek başına derlenebilir olması gerektiğini belirtir. 
       Webpack ile çalışırken bundan çok yararlanacağız */
    "noUnusedLocals": false,
    "noUnusedParameters": true,
    "useDefineForClassFields": true,
    "baseUrl": "./src",
    /*** PROJEMİZİN TEMEL DİZİNİNİ /SRC OLARAK BELİRTİYORUZ ***/
    "allowJs": true,
    "checkJs": false,
    "removeComments": true
  },
  "include": [
    "src"
  ], /* src/ dosyamınzdaki her bir dosyanın dahil edilmesini sağlar. */
  "exclude": [
    "node_modules",
    "public"
  ] /* node_modules, her zaman kütüphane olarak kullanıldığı için derleme dışı bırakılmalıdır
       ve public ise, nihai çıktımızın olduğu yer için derleme kullanılması mantık dışıdır. */
}
```

Şimdiyse dosyamızda esas programımızı yazacağım kısım olarak bir `/src` klasorü oluşturalım ve içerisine bir `app.ts` dosyası koyalım:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750988524378/dbd77d38-5fc2-4c3d-943d-9251715be806.png align="center")

```typescript
console.log("Merhaba dünya!");
```

Bu app.ts dosyasında yazacağımız kodu node ile çalıştırdığımızda çıktıları alırız:

```powershell
PS C:\Users\gokacinlar\Desktop\ornek-proje\src> node app.ts
Merhaba dünya!
```

Güzel! Şimdi projemizin en temel gereksinimlerini karşıladık ve ileri konfigürason ve Webpack için hazırız. Fakat daha önce yapmamız gereken, projemizdeki dizinleri ayarlamak çünkü kullanacağımız statik dosyalardan olan resimler, videolar veya fontları ayrı bir yerde, .ts veya .js dosyalarını ayrı bir yerde tutmamız ve bir hiyerarşi oluşturmamız gerekiyor. Bunun için şöyle bir proje dizini oluşturabilirsiniz:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750991122245/77652e70-6d5e-4558-9dfd-41076afddd1a.jpeg align="center")

```plaintext
ornek-proje
├─ package-lock.json
├─ package.json
├─ public             <- WEBPACK ÇIKTISI BURADA OLACAKTIR
├─ src
│  ├─ index.html      <- TEMEL HTML TASLAĞIMIZ
│  ├─ app.ts          <- ESAS APP.TS DOSYAMIZ
│  ├─ assets          <- STATİK ÖGELER
│  │  ├─ css
│  │  ├─ font
│  │  ├─ image
│  │  └─ video
│  └─ components      <- BİLEŞENLERLE ÇALIŞACAKSAK BİLEŞEN DOSYAMIZ
│     └─ header.ts
├─ tsconfig.json
└─ webpack.config.js
```

> NOT:
> 
> Diğer makalelerde, `index.html` dosyasının, **/public** klasöründe bulunduğunu görebilirsiniz fakat biz, aşağıda yükleyeceğimiz `html-webpack-plugin` ile HTML dosyamıza, temel bir taslak verdikten sonra **otomatik olarak oluşturulacak temel** `.js` **dosyalarımızla birlikte oluşturmak istediğimizden**, `/src` klasöründe oluşturarak işi uzun vadede kolaylaştırıyoruz. Bunu Webpack konfigürasyonunda ayarlayacağız.

## Webpack Kurulumu

Webpack’i de yine NPM üzerinden indiriyoruz:

```powershell
PS C:\Users\gokacinlar\Desktop\ornek-proje> npm install --save-dev webpack webpack-cli webpack-dev-server ts-loader 
```

Buradaki paketlerin işlevi kısaca şöyledir:

* `webpack`: Webpack’in kendisi.
    
* `webpack-cli`: Komut satırında Webpack komutlarını kullanabilmemiz için gerekli arayüzü sağlar.
    
* `webpack-dev-server`: Geliştirme sırasında değişiklikleri anlık görmemizi sağlayan, hot-reload özelliğine sahip yerel sunucuyu *localhost* üzerinden çalıştırmamızı sağlayan bir serverdır.
    
* `ts-loader`: TypeScript dosyalarını Webpack üzerinden derleyebilmek için kullanılan yükleyicidir.
    

Normalde temel bir Webpack projesi için bu paketler yeterli olacaktır fakat ben, size projeniz genişledikçe kullanabileceğiniz ekstra paketleri de indirmenizi sağlayarak uzun vadede daha sağlam bir temel edinmenizi kolaylaştırmak için bu paketleri de yüklemenizi isteyeceğim:

```javascript
PS C:\Users\gokacinlar\Desktop\ornek-proje> npm install --save-dev copy-webpack-plugin mini-css-extract-plugin css-minimizer-webpack-plugin terser-webpack-plugin html-webpack-plugin
```

* `copy-webpack-plugin`: Webpack yalnızca içeri aktarılan (import edilen) dosyalarla çalıştığı için, harici statik dosyaları (*resimler, videolar, fontlar vs.*) **dahil etmek için** bu eklenti kullanılır.
    
* `mini-css-extract-plugin`: CSS dosyalarını JavaScript içinde gömmek yerine, **ayrı .css dosyaların oluşturarak onlardan** **çıkış almanızı sağlar**. Bu, özellikle üretim ortamında daha verimli (cache edilebilir, paralel yüklenebilir) olur.
    
* `css-minimizer-webpack-plugin`: CSS dosyalarınızı **sıkıştırır ve küçültür** (*minify*). Böylece CSS veya SCSS dosyalarınız daha küçük olur.
    
* `terser-webpack-plugin`: JavaScript dosyalarınızı **sıkıştırır ve küçültür.**
    
* `html-webpack-plugin`: HTML dosyanızı otomatik olarak oluşturur veya verdiğiniz şablona göre üretir. Derlenen JavaScript dosyalarınızın ismini ve kullanımını **otomatik olarak** `<script>` etiketiyle bu oluşturduğu *HTML’ye* ekler.
    

Daha sonra `css` ve `scss` dosyalarımızın da yüklenmesini sağlayacak paketleri yükleyelim:

```javascript
PS C:\Users\gokacinlar\Desktop\ornek-proje> npm install --save-dev style-loader postcss-loader sass-loader css-loader
```

Ardından dosyamızda, Webpack’i de konfigüre edebilmek için bir `webpack.config.js` dosyası oluşturuyoruz:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750990800710/1f98317d-2bc5-4862-ac29-a7ece95af89a.png align="center")

Bu dosya içerisine pek çok ayar eklenebilir fakat bizim ihtiyacımız olan ayarları, sadeleştirerek size basit fakat son derece güçlü bir webpack konfigürasyonu ayarladım:

```javascript
// Gerekli modülleri, node_modules'den yüklüyoruz
const path = require("path");
const CopyPlugin = require("copy-webpack-plugin");
const MiniCssExtractPlugin = require("mini-css-extract-plugin");
const CssMinimizerPlugin = require("css-minimizer-webpack-plugin");
const TerserWebpackPlugin = require("terser-webpack-plugin");
const HtmlWebpackPlugin = require("html-webpack-plugin");

// Temel Webpack kurallarını belirttiğimiz esas module konfig kuralları burada tanımlanır.
module.exports = {
    // Giriş noktası (ana TypeScript dosyamız)
    entry: {
        main: "./src/app.ts",
    },
    target: "web", // target, hedef ortamını belirtir. Bunu belirtmenize aslında gerek yok
                   // çünkü webpack zaten varsayılan olarak "web", yani client-side kullanır.
    // Dosyaların nasıl işleneceğini tanımlayan kurallar
    module: {
        rules: [
            {
                // .ts uzantılı dosyaları ts-loader ile yükler ve işler
                test: /\.ts$/,
                use: "ts-loader",
                include: [path.resolve(__dirname,
                    "src")
                ], // Sadece **SRC** klasörü içinden yüklenir.
                exclude: /node_modules/, // node_modules klasörünü hariç tut
            },
            {
                // .css, .scss, .sass uzantılı dosyalar için gerekli loader'lar
                test: /\.s?[ac]ss$/i,
                use: [
                    MiniCssExtractPlugin.loader,
                    "css-loader",
                    "sass-loader",
                ],
            },
            {
                // Görselleri (png, jpg, gif, svg, ico) statik birer dosya (asset) olarak işle
                test: /\.(png|jpe?g|gif|svg|ico)$/i,
                type: "asset/resource",
                generator: {
                    filename: "assets/images/[name][ext]", // Çıkış klasör yapısı
                },
            },
            {
                // Font dosyaları için benzer şekilde asset/resource tipi
                test: /\.woff($|\?)|\.woff2($|\?)|\.ttf($|\?)|\.eot($|\?)|\.svg($|\?)/i,
                type: "asset/resource",
                generator: {
                    filename: "assets/fonts/[name][ext]",
                },
            },
            {
                // Video dosyalarını da aynı şekilde işleyip assets klasörüne koy
                test: /\.(mp4|webm|ogg|mov)$/i,
                type: "asset/resource",
                generator: {
                    filename: "assets/videos/[name][ext]",
                },
            }
        ],
    },
    // Derleme sonucunda elde ettiğimiz dosyaların burada tanımlarız
    output: {
        publicPath: "/", // Projemizin kökü "/" ile, hepsi dahil edilir.
        filename: "js/[name].[contenthash].js", // JS dosyası adı (cache için content hash ile)
        chunkFilename: "js/[name].[contenthash].chunk.js", // Dinamik olarak yüklenen parçalar için
        path: path.resolve(__dirname,
            "public"), // Derleme çıktısı bu klasöre gider
        clean: {
            keep: "public/index.html", // Derlemede public klasörünü temizlerken index.html'yi koru
        },
    },
    // Geliştirme modu (production'da daha farklı ayarlar olacak)
    mode: "development",
    // Geliştirme sunucusu (webpack-dev-server) ayarları
    devServer: {
        static: {
            directory: path.join(__dirname,
                "public"), // Sunulacak dizin
        },
        compress: false, // Gzip sıkıştırması kapalı
        client: {
            logging: "warn",
            overlay: {
                errors: true, // Hataları sayfanın üstünde gösterir böylece her zaman konsola gitmenize gerek kalmaz 
                warnings: false,
            },
            progress: true, // Derleme ilerlemesini göster
        },
        port: 1234, // Sunucu portu. Bunu istediğiniz şekilde ayarlayabilirsiniz (örn: 4321)
        host: "0.0.0.0", // Tüm IP adreslerinden erişilebilir
        hot: true, // Hot Module Replacement (modül değişikliklerinde sayfa yenilenmez)
        watchFiles: [
            "src/**/*"
        ], // src klasöründeki tüm dosyaları izler
    },
    // Otomatik dosya izleme için ekstra ayarlar
    watchOptions: {
        poll: 1000, // Her 1 saniyede bir dosya değişikliği kontrolü
        ignored: /node_modules/, // İzlemeden node_modules göz ardı edilir.
    },
    // Dosya uzantılarını çözme sırası
    resolve: {
        extensions: [
            ".ts",
            ".js"
        ],
    },
    // Performans ve optimizasyon ayarları
    // Ortak bir bundle.js kullanmak yerine, "chunk"lar kullanılarak kod dizini ayrı
    // dosyalara bölünür ve böylece gerektiği takdirde yüklenerek sayfa ve SEO performansını
    // artırır.
    optimization: {
        splitChunks: {
            chunks: "all", // Ortak kodları ayır
            cacheGroups: {
                vendors: {
                    test: /[\\/]node_modules[\\/]/, // node_modules içindeki kodları ayır
                    name: "vendors",
                    chunks: "all",
                },
            },
        },
        minimize: true, // Kodları küçült
        minimizer: [
            // JS sıkıştırma
            new TerserWebpackPlugin({
                terserOptions: {
                    format: {
                        comments: false, // Açıklama satırlarını kaldır
                    },
                },
                extractComments: false, // Ayrı bir .LICENSE.txt oluşturmasın
            }),
            // CSS sıkıştırma
            new CssMinimizerPlugin()
        ],
    },
    // Webpack eklentileri
    plugins: [
        // Belirli klasörleri doğrudan public'e kopyalar.
        // Burada kopyalamak istediğiniz klasörleri belirtsiniz.
        // Eğer belirtmezseniz, Webpack bunları kopyalamayacaktır.
        new CopyPlugin({
            patterns: [
                {
                    from: path.resolve(__dirname,
                        "src/assets/images"), // src içindeki görselleri
                    to: "assets/images", // dist içindeki klasöre kopyala
                    noErrorOnMissing: true, // Dosyalar eksikse hata verme
                },
                {
                    from: path.resolve(__dirname,
                        "src/assets/videos"),
                    to: "assets/videos",
                    noErrorOnMissing: true,
                },
            ],
        }),
        // CSS dosyalarını ayrı olarak çıkartır.
        // Bu aynı yukarıdaki gibi kullandığımız CSS'i de parçalara bölerek performansı artırır.
        new MiniCssExtractPlugin({
            filename: "css/[name].[contenthash].css", // Ana CSS
            chunkFilename: "css/[name].[contenthash].chunk.css", // Dinamik CSS parçaları
        }),
        // HTML dosyasını oluşturur ve otomatik olarak <script> ve <link> etiketlerini ekler
        new HtmlWebpackPlugin({
            template: path.resolve(__dirname, "src/index.html"), // Şablon HTML
            inject: "body", // <script> tag'ini body'ye ekle
            minify: false,
            filename: "./index.html" // Çıktı dosyası
        }),
    ]
};
```

Epey uzun bir konfig değil mi? :D Ama projeniz genişledikçe, bu konfig bile yetersiz kalmaya başlayabileceğinden, şimdiden profesyonel kullanımı alışkalık edinmek son derece elzemdir. Şimdiye yapmamız gereken, `package.json` dosyamızda webpack’ı çalıştırmak için gerekli script komutlarını kullanarak işi tamamlamaktır:

```json
{
  "name": "ornek-proje",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "build": "webpack --config webpack.config.js --mode production",
    "server": "webpack serve --config webpack.config.js --mode development"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "type": "commonjs",
  "devDependencies": {
    "copy-webpack-plugin": "^13.0.0",
    "css-minimizer-webpack-plugin": "^7.0.2",
    "eslint": "^9.29.0",
    "html-webpack-plugin": "^5.6.3",
    "mini-css-extract-plugin": "^2.9.2",
    "terser-webpack-plugin": "^5.3.14",
    "ts-loader": "^9.5.2",
    "typescript": "^5.8.3",
    "webpack": "^5.99.9",
    "webpack-cli": "^6.0.1",
    "webpack-dev-server": "^5.2.2"
  }
}
```

“scripts” kısmına iki farklı script ekledik ki her zaman terminale:

```powershell
PS C:\Users\gokacinlar\Desktop\ornek-proje> webpack --config webpack.config.js --mode production
```

…gibi uzun uzadıya yazmamıza gerek kalmasın. Onun yerine kısaca:

```powershell
PS C:\Users\gokacinlar\Desktop\ornek-proje> npm run build
```

…diyerek webpacki çalıştırır, derler ve nihai output’umuzu alırız. Bu kodu çalıştırdığımızda, dosyamızda şöyle bir değişiklik fark edeceğiz:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750991879238/b103d627-fb3e-4da4-bc2f-777b2f5dbb00.jpeg align="center")

`/public` klasörüne, TypeScript’in derlediği, Webpack’in bundle ederek kopyaladığı **her şey oluşturulmuştur**. Bunu, projenizde istediğini her şekilde geliştirmek için bir boilerplate olarak kullanabilirsiniz.

Görüşmek üzere!