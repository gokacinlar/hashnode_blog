---
title: "TypeScript + Headless WordPress + GraphQL ile Blog Önyüzü Geliştirmek"
seoTitle: "Developing a Blog Frontend with TypeScript"
seoDescription: "Önyüz geliştiricileri için TypeScript ve GraphQL ile Headless WordPress blog oluşturmanın adımları ve avantajları."
datePublished: Wed Oct 29 2025 19:24:28 GMT+0000 (Coordinated Universal Time)
cuid: cmhcdwsfz000002judlj7bmd4
slug: typescript-headless-wordpress-graphql-ile-blog-onyuzu-gelistirmek
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1761765847549/e01daef6-e32b-4fa0-a8a4-c31bb96956f0.jpeg
tags: typescript, graphql, headless, headless-wordpress, wpgraphql

---

Önyüz geliştiricileri olarak kariyerimizin mutlaka bir noktasında WordPress ile karşı karşıya gelmişizdir. WordPress, bir İYS (*İçerik Yönetim Sistemi - Content Management System*) olmakla birlikte, dünyadaki toplam websitelerin toplam **~43%** gibi bir oranını teşkil [ediyor](https://w3techs.com/technologies/details/cm-wordpress). WordPress’i bu kadar yaygın ve tercih edilir yapan unsurlar, elbette *legacy* bir sistem olarak yıllardır kullanılması, buna müteakip kendisi için geliştirilen tonlarca eklenti, çatı ve kütüphaneler ile belirgin bir standart haline gelmesindendir. Kısaca bir *no-brainer* çözümüdür.

Fakat bana kalırsa, WordPress, **korkunç bir geliştirici deneyimi sunuyor.** Temel mantığı [**WYSIWYG**](https://en.wikipedia.org/wiki/WYSIWYG) ile eşdeğer olan tema + eklenti (plugin) mantığında çalıştığı için ve elbette PHP ile çok sık çalışmadığım için bana her zaman kısıtlayıcı gelmiştir. Gerçekten kod yazarak belirli bir şema inşa ederek yazılımı oluşturmayı tercih ettiğimden WordPress’in bu ‘basitleştirici’ kullanım mantığı bana pek yatmıyor. Bunun için TypeScript/React/Next.js tarafını tercih ediyorum.

Fakat endüstri ve pek çok kişi bu yazılımı tercih etmeye devam ediyor ve bu yazının konusu olarak da, WordPress’i, sadece **headless** formunda, yani **ön-yüzden arındırılmış bir back-end şeklinde** kullanarak, ön-yüzü istediğimiz şekilde şekillendirebileceğiniz ve bunu yaparken de WordPress’in ortaya çıkış amaçlarından biri olan içerik yönetim sistemini, blog yazılarımız için graphQL ile kullanacağımız bir senaryo düşündüm. GraphQL, `REST API` kullanmadan, bizatihi istenen içeriğe *query* yapmamızı sağlayan bir *query language* olduğu için tipik bir senaryoda tercih edilmesi daha iyi oluyor.

Aslında bu senaryo zaten kullanılıyor **fakat ön-yüzünde esnekliğe önem veren ve aynı zamanda da WordPress’i kullanarak basit bir çözüm arayan herkes bu yazıdan faydalanabilir.** Şimdi gelin, ilk önce hazırlıklarımızı yaparak WordPress’i, Headless WordPress olarak ön-yüzümüzde nasıl kullanabiliriz, inceleyelim.

Proje boyunca kullanacağımı teknolojiler şunlar olacaktır:

* Typescript^5
    
* GraphQL^16
    
* WordPress^6
    
* Local veya WordPress Studio
    

## Aşama 1) Gerekli Yazılımları Kurmak

Sistemimizde her şeyden önce `Node.js` ve bir metin editörü/IDE yüklü olmalı; ben VSCode tercih ediyorum. Eğer yüklü değilse hemen [gidip yükleyin](https://nodejs.org/en/download). `Node.js` ile bir proje oluşturarak aşağıdaki komutları girdiğimizde, gerekli tüm `Typescript` projemizi oluşturmuş olacağız. Ben Windows ortamında geliştirme yaptığım için aşağıdaki komutları sırasıyla girerek yeni bir proje oluşturdum:

```powershell
Windows PowerShell
Copyright (C) Microsoft Corporation. All rights reserved.

Try the new cross-platform PowerShell https://aka.ms/pscore6

PS C:\Users\gokacinlar\Desktop> mkdir proje
PS C:\Users\gokacinlar\Desktop\proje> cd proje
PS C:\Users\gokacinlar\Desktop\proje> npm init -y
PS C:\Users\gokacinlar\Desktop\proje> npm i -D typescript ts-node eslint prettier @types/node
PS C:\Users\gokacinlar\Desktop\proje> npx tsc --init
PS C:\Users\gokacinlar\Desktop\proje> code . # Projemizi VSCode ile açalım
```

Şimdi ise WordPress kısmını halletmemiz gerekiyor. Yerel bilgisayarımızda bir sunucu oluşturarak WordPress’i çalıştırabilmemiz için pek çok yol mevcut. Bu yollar XAMPP, LAMP gibi yazılımlarla yapılabilir fakat işleri hızlandırmak için ya **Local** ya da **WordPress Studio** kullanmanız için çok daha avantajlı olacaktır. Ben tercihen **WordPress Studio kullanacağım.** Bu yazılımı kendi [websitesinden](https://developer.wordpress.com/studio/) veya Microsoft Store’dan [indirebilirsiniz](https://apps.microsoft.com/detail/9PNTBL35PZVS?hl=en-us&gl=TR&ocid=pdpshare):

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1761762611715/54f2827f-1e82-43af-8c8b-54bc56ef6186.png align="center")

Hemen ardından kurulumu gerçekleştiriyoruz:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1761762798659/bc9765ce-3f2f-4e78-bb42-dfed72ce913b.png align="center")

Kurulum tamamlandıktan sonra **WP-ADMIN** ile WordPress’imizin admin sayfasına gelerek eklentiler kısmından, "*Add Plugin”* seçeneğine tıklayarak **GraphQL** eklentisini aratıp kuruyoruz ve eklentiler sekmesinden aktif ediyoruz:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1761763083445/1cb3facc-ec06-4093-9cad-7b74660792e9.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1761763096654/6d911fd0-acb5-4b79-9e64-c0e0648eafc0.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1761763225756/bda95fc0-2941-4381-b80d-922d7152097b.png align="center")

Eğer tüm işlemler doğru yapıldıysa, eklentimiz başarıyla çalışmış ve bir **GraphQL** sekmesi belirmiş olacaktır:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1761763363245/14166a11-0b73-426c-b9d8-7b682bf88b4c.png align="center")

**Şimdiki aşama ise önemli**. GraphQL sekmesinden, *Settings* yani ayarlar sekmesine gidip GraphQL ile etkileşime gireceğimiz *domain endpoint’ini* bilmemiz gerekiyor çünkü ön-yüzümüzde gerçekleştireceğimiz işlemler, bu adres üzerinden GraphQL ile WordPress bağlantısı kurularak gerçekleştirilecektir. Benim bilgisayarımda bu şudur:

> http://localhost:8881/graphql

Bu endpoint üzerinden GraphQL ile WordPress’te olan blogumuzla ilgili işlemleri yapacağımızdan, WordPress Studio ile lokalde geliştirme yaparken **bu siteyi kapatmamaya özen gösterin**, yoksa [**5xx hatası alabilirsiniz!**](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes#5xx_server_errors)

## Aşama 2) Typescript Ayarlanması (Types & Interfaces)

Ciddi bir Typescript projesi, adı üzerinde verilerle ve nesne yönelimli programlamada olmazsa olmaz veri tanımlamaları gerektirir. Bu proje ilerledikçe karşılaşılacak baş ağrılarını azaltır. GraphQL sorguları için tip güvenliğini sağlamak amacıyla öncelikle gerekli *interface*'leri tanımlayalım ki, GraphQL ile gelecek verilerin metotlarımızca sorunsuz bir *type safety* ile çalışabilmesi için gerekli teminatı sağlayalım. Bu sayede TypeScript, kod yazarken bize yardımcı olacak ve hataları önceden yakalayacaktır.

```typescript
// WordPress GraphQL'den gelen yanıt tipleri
interface GraphQLAuthorNode {
    name: string;
}

interface GraphQLPostNode {
    id: string;
    title: string;
    author: {
        node: GraphQLAuthorNode;
    };
    content: string;
}

// Uygulamamızda kullanacağımız basitleştirilmiş tipler
// Bunlar GraphQL ile query yazarken alacağımız verilere atanan veri türleridir.
interface Author {
    name: string;
}

interface Post {
    id: string;
    title: string;
    author: Author;
    content: string;
}

interface PostPreview {
    id: string;
    title: string;
    author: Author;
}
```

Bu tip tanımlamaları, GraphQL'den gelen karmaşık veri yapısını, uygulamamızda kullanımı daha kolay olan düz bir yapıya dönüştürmemize yardımcı olacaktır.

## Aşama 3) GraphQL Sorguları Yazmak

GraphQL'in en güzel yanlarından biri, ihtiyacımız olan veriyi **tam olarak belirtebilmemizdir**. Bir blog düşündüğümüzde, öncelikle yazıları ve ardından yazıların içeriği olarak İki farklı sorgu yazacağız:

**1\. Blog Yazılarının Listesi İçin (Hafif Sorgu/Hızlı Listeleme İçin İdeal):**

```typescript
const QUERY_POST_PREVIEWS = `
    query GetPostPreviews($first: Int) {
        posts(first: $first) {
            nodes {
                id
                title
                author {
                    node {
                        name
                    }
                }
            }
        }
    }
`;
```

Bu sorgu sadece yazı başlıklarını ve yazarları getiriyor. İçeriği getirmediği için çok hızlı çalışır.

**2\. Tek Bir Yazının Detayı İçin (Tam Sorgu):**

Bu senaryoda ise, tıklanılan herhangi bir yazıyı okumak isteyen kullanıcı, tüm yazıları değil sadece tıklanan yazıyı indirecektir ve bu da bu sorguyla gerçekleştirilir. Dikkat ederseniz, **content**, WordPress tarafından barındırılan yazıların kendisidir ve bizim **RENDERED** olarak verdiğimiz değerle, HTML olarak karşımıza getirilir. [Dokümantasyonu](https://www.wpgraphql.com/docs/posts-and-pages#filtering-a-list-of-posts) buradan okuyabilirsiniz.

```typescript
const QUERY_SINGLE_POST = `
    query GetSinglePost($id: ID!) {
        post(id: $id, idType: DATABASE_ID) {
            id
            title
            content(format: RENDERED)
            author {
                node {
                    name
                }
            }
        }
    }
`;
```

Bu sorgu ise kullanıcı bir yazıya tıkladığında, o yazının tam içeriğini getirmek için kullanılır.

## Aşama 4) GraphQL ve Ön-yüz İstemciyi Yazmak

Şimdi asıl işi yapacak olan istemci sınıfımızı oluşturalım. Typescript’te NTP ile çalıştığım için temel bir sınıf oluşturarak kodumuzu yazalım. Bu sınıf, WordPress GraphQL API'sine sorgu gönderecek ve sonuçları işleyecektir:

```typescript
class WordPressClient {
    // GraphQL Endpointi belirtelim
    private static readonly GRAPHQL_ENDPOINT = "http://localhost:8881/graphql";

    // GraphQL sorgusunu çalıştıran asycn query yardımcı metodumuz
    private async query<T>(query: string, variables?: any): Promise<T> {
        const response = await fetch(WordPressClient.GRAPHQL_ENDPOINT, {
            method: "POST",
            mode: "cors", // Modu CORS olarak belirtmemiz faydalıdır
            headers: {
                "Content-Type": "application/json",
                "Accept": "application/json"
            },
            body: JSON.stringify({ query, variables }),
        });

        if (!response.ok) {
            throw new Error(`HTTP Hatası: ${response.status}`);
        }

        const result = await response.json();
        if (result.errors) {
            throw new Error(`GraphQL Hatası: ${result.errors[0].message}`);
        } else {
            return result.data;
        }
    }

    // Blog yazılarının önizlemelerini alalım
    // GraphQL ile varsayılan maksimum alınabilecek blog sayısını 10 olarak belirledik
    // Bu ileride pagination kullandığımızda gereksiz internet trafiğini önleyecektir
    public async fetchPostPreviews(count: number = 10): Promise<PostPreview[]> {
        const data = await this.query<{ posts: { nodes: GraphQLPostNode[] } }>(
            QUERY_POST_PREVIEWS,
            { first: count }
        );

        // GraphQL yapısını basitleştirilmiş yapıya dönüştürelim
        // Böylece sadece string olarak değil, JSON çıktı olarak veriyi daha 
        // kolay entegre edebiliriz
        return data.posts.nodes.map(node => ({
            id: node.id,
            title: node.title,
            author: {
                name: node.author.node.name
            }
        }));
    }

    // Tek bir yazının tam içeriğini getirmek için de ayrı bir metot yazalım
    public async fetchSinglePost(postId: string): Promise<Post> {
        const data = await this.query<{ post: GraphQLPostNode }>(
            QUERY_SINGLE_POST,
            { id: postId }
        );

        return {
            id: data.post.id,
            title: data.post.title,
            content: data.post.content,
            author: {
                name: data.post.author.node.name
            }
        };
    }
}
```

Artık Headless WordPress üzerinden blog yazılarımızı kendi geliştirdiğimiz front-end agnostic herhangi bir esnek çatı/kütüphane ile veya onlarsız yaptığımız sade bir arayüzle entegre edebileceğimiz sınıfımızı oluşturmuş olduk. Şimdi basit örnek olarak kullanımına bakalım.

## Aşama 5) GraphQL ve Ön-yüz İstemciyi Yazmak

```typescript
// Sınıfımızı instantiate edelim
const client = new WordPressClient();

// Blog yazılarının alalım ki kaç tane blog yazımız olduğunu listeleyebilelim
// SENARYO: 5 tane blog yazısı getirdik.
const previews = await client.fetchPostPreviews(5);
console.log(previews);
// Olası Çıktı: [{ id: "1", title: "İlk Yazım", author: { name: "Ahmet" } }, ...]

// Belirli bir yazının detayını alalım
const post = await client.fetchSinglePost("1");
console.log(post.title); // "İlk Yazım"
console.log(post.content); // "<p>Yazı içeriği...</p>"
// FORMATLANMIŞ HTML OLARAK GELECEKTİR.
```

Bu noktadan sonra bu altyapıyla ne yapacağınızı seçmek size kalmış. Oldukça eğlenceli ve kesin bir no-brainer çözümüydü. Bu metodu kendi sitemde kullanıyorum ve sonuçlarından kesinlikle çok memnunum. Kendi sitemin kaynak kodları GitHub’da mevcut ve sitemin kendisine de aşağıdaki bağlantıdan ulaşılabilir.

%[https://github.com/gokacinlar/portfolio] 

Bir sonraki yazımda görüşmek üzere!