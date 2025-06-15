---
title: "Android Varsayılan Gizlilik Ayarlarını Düzenleme (Samsung & OneUI)"
seoTitle: "Android Varsayılan Gizlilik Ayarlarını Düzenleme (Samsung & OneUI)"
datePublished: Sun Jun 15 2025 15:16:54 GMT+0000 (Coordinated Universal Time)
cuid: cmbxt6kgd000a02l285cg8niz
slug: android-varsayilan-gizlilik-ayarlarini-duzenleme-samsung-and-oneui
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1750000133562/488466c8-7539-4c8e-ba2f-1d4c81e94604.jpeg
tags: privacy, android, samsung

---

---

Android'in varsayılan olarak, kullanıcı mahremiyetini radikal ölçüde ihlal eden pek çok ayarla [önyüklü olarak geldiğini](https://www.fastcompany.com/91174609/default-app-settings-user-privacy) biliyoruz -ayrıca genellikle reklam olarak alınan bazı uygulamaların da (Samsung için bunlar genellikle Facebook, TikTok gibi uygulamalardır) kurulum esnasında yüklenmesi cabasıdır- fakat gündelik yaşamın tesisi uğraşında, sadece makinesini alarak, onu son derece basit fakat gerekli amaçları için kullanmak isteyen insanlar, bu ayarlarla bırakın uğraşmayı, varlıklarından dahi haberleri olmuyor. Bunu yakınlarınıza, ebeveynlerinize de sorabilirsiniz ve alacağınız yanıtlar, *"bu ayarın var olduğunu bilmiyordum"* gibisinden onaylayıcı olacaktır.

Her ne kadar Android'den bu uygulamaların pek çoğu (hem size akıllı telefonunu satan şirketlerin önyüklü gelen uygulamaları hem de reklam uygulamaları) kaldırılabiliyor ve bu kullanıcı mahremiyetine minimum düzeyde saygı duyan özelliklerin pek çoğu kapatılabiliyorsa da, dediğimiz gibi, halihazırda bu özelliklerin var olduğunu bilen az sayıda insan olması yüzünden ve bilenlerin de bir şekilde öteki insanları bunlardan haberdar etmesinde çok da başarılı olmaması yüzünden (özellikle insanların bu konudaki vurdumduymazlığı da önemli bir faktördür, çünkü insanlar parasını vererek aldıkları şeyin genellikle işlerini görmesini isterler ve diğer şeylerle uğraşmazlar) bu ayarlar hiç değiştirilmeden kalıyor.

Bunun önüne geçmek kesinlikle mümkün ve dijital parmak izinizi mümkün olduğunca azaltmak için (çünkü tamamiyle yok etmek imkansızdır) belli başlı ayarlar yapılabilir. Bunu size, örnek olarak Samsung cihazdaki, ONEUI 6.1 arayüzü üzerindeki ayarlardan göstereceğim. Arayüzler, çeşitli Android cihaz üreten şirketlerin özel ROM'ları sebebiyle çeşitlilik gösterebilir fakat aşağı yukarı aynıdırlar. Siz de boş bir zamanınızda bu ayarları keşfederek düzenleyebilirsiniz.

## Google'a Ait Gizlilik Ayarları

Öncelikle telefonunuzdaki "Ayarlar" sekmesine gelerek, "Gizlilik ve Güvenlik" sekmesine gitmeniz gerekiyor. Ben telefonumu İngilizce ayarda kullanıyorum ancak simgeleri ve bulundukları arayüz konumu sebebiyle Türkçe'de de çok kolay bulunabilirler:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750000190172/8ca267a8-ca6e-4db9-8a82-8f099bb72c4f.jpeg align="center")

Bu sekmenin en alt kısmında, "Daha fazla gizlilik ayarı" anlamına gelen bir alt sekmeye açılan bir bölme göreceksiniz:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750000203166/01b14056-3cba-49db-906c-908ad6b12cee.jpeg align="center")

Bu sekmeye tıkladığınızda, asıl önemli işlerimizden ilki başlıyor ve benim şahsi olarak hepsini devre dışı bırakmanızı önerdiğim ayarlar listeleniyor:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750000217090/f6fc0e5b-4c41-4da4-bcfe-69165bae9484.jpeg align="center")

Buradaki ayarların ne olduğunu şöyle anlatabiliriz:

1. **Android Personalization Service:** Bu ayar, telefonunuzu kullanma biçiminizi öğrenerek size özel "kişiselleştirilmiş içerik" önerisi yapar. Yani sizin alışkanlıklarınızı öğrenerek, uygun yerlerde *identifiable*, yani tanımlanabilir kimliğinize yönelik reklamlar ve öneriler sunar.
    
2. **Data Sharing Updates for Location:** Bu ayar, uygulamaların ihtiyaç duyduğu konum bilgisini nasıl değiştirdiğini (kendi kullanım senaryosu için nasıl kullandığını) göstermek için vardır ve verilen izinler ölçüsünde sizin kesin konumunuz dahil olmak üzere, tahmin edilen konumunuzu alabilen uygulamaların, *kısaca konum verilerinize erişimi listeler.*
    
3. **Android System Intelligence:** Bu, belki de bu sekmedeki en önemli [ayarlardan](https://support.google.com/pixelphone/answer/12112173?hl=en) birisidir. Nitekim, cihaz kullanım senaryolarını öğrenerek size çeşitli yollardan öneriler sunar. Küçük çaplı bir yapay zeka gibi davranır ve amacı sizden olabildiğince çok öğrenip, olabildiğince fazla kişiselleştirilmiş öneri sunmaktır. Altta bütün ayarları kapatmanızı öneririm (Clear data, yani verileri temizleme butonuyla tüm verilerinizi silebilirsiniz):
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750000294097/85890aba-3d73-48ee-a5ea-b0f7f510a3df.jpeg align="center")

4. **Health Connect:** [Bu ayar](https://developer.android.com/health-and-fitness/guides), telefonun içinde gömülü olan sensörler vasıtasıyla, eğer varsa bilekliğiniz gibi harici donanımlarla sağlık bilgilerinizi yönetmenize yarar. Sorun, yine "kişiselleştirilebilir" verileri depolaması ve [işlemesidir](https://support.google.com/googleplay/android-developer/answer/12991134?hl=en#zippy=) ve bu bilgiler paylaşılabilir.
    
5. **Autofill Service from Google:** Bu servis, şifreleriniz, kullanıcı isimleriniz, e-postalarınız veya Google üzerinden depoladığınız bilgilerinizi, örneğin yeni bir siteye kayıt olurken veya Twitter/X gibi uygulamalarda oturum açarken tekrardan yazmak yerine hemen doldurmasıyla çalışır. Oldukça zaman kazandırıcı ve etkili bir yöntemdir, şahsen uzun süre kullanmıştım fakat isteyenler devredışı bırakabilir.
    
6. **Activity Controls:** Bu kısım, Google üzerindeki her bir gizlilik ayarını (YouTube, Google arama geçmişi, kişiselleştirilmiş reklamlar vs.) kontrol edebilmenize olanak tanır. Belki de en önemli özelliktir çünkü Google servislerinde bulunan gizlilik ayarlarını listeler ve bu ayarlar, Google gibi kazancını reklamlardan elde eden bir şirket için sizden öğrenebileceği bilgiler ile size reklam yönlendirmesi bakımından hiç olmazsa bir kontrol sağlar. Buradaki her bir ayarı kapatmanızı tavsiye ederim.
    
7. **Ads:** Bu kısım reklamlara aittir. Android ve onu geliştiren Google, diğer [yazımda](https://blog.dervisoksuzoglu.net/posts/linux-hakkinda-I-etik.html) da bahsettiğim gibi, size reklam göstererek gelir elde eder ve gösterdiği reklamların doğruluğu da beslendiği verilere bağlıdır. Sizden de çok çeşitli yollardan veri toplar ve bunu kontrol etmenize de bir ölçüde izin verir:
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750000336033/a7037665-3059-42d5-9363-9b9ca38a7831.jpeg align="center")

Burada dikkat etmeniz gereken yer, **"Ad Privacy"** başlığındaki tüm ayarları kapatmanız ve **"Advertising ID"** sekmesindeki, reklam kimliğiniz olan ve alışkanlıklarınızın özel, tanımlanabilir, tıpkı T.C. Kimlik numarası gibi biricik bir şekilde reklamlarla ilişkilendirilebileceği bir seçeneği, eğer varsa kaldırmanızdır. Bu, gösterilen reklamları azaltmaz ancak sizinle ilişkilendirilmesini büyük ölçüde azaltır:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750000367791/cc3df53f-aa1f-4036-a22e-0c9f0b3d7f8d.jpeg align="center")

8. **Usage & Statistics:** Bu ayar, cihazınızda bulunan kullanım senaryolarınızı Google'a göndererek, Android'in geliştirilmesine yardımcı olmanızı sağlar. Tıpkı 90'larda, geri bildirim için [insanların Microsoft'a gidip test yapması](https://youtu.be/i0F4nMeJ62I?t=640) gibi, siz de Google'a yardımcı olursunuz:
    

## Samsung'a Ait Gizlilik Ayarları

Google ve Android'e ait gizlilik ayarları olduğu gibi, elbette üreticinize göre değişebilen şirketlerin sunduğu hizmet ve uygulamaların da birer gizlilik ayarı vardır ve benim kullandığım telefon Samsung marka bir telefon olduğu için, Samsung'un uygulamalarına ve hizmetlerinin de birer gizlilik ayarı vardır. *Gizlilik Ayarları* sekmesindeki **"Hesap Gizliliği (Account Security)"** kısmından Samsung'un ve Google'ın ekstra gizlilik ayarlarına erişilebilir:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750000499283/904b13e1-82fd-4475-8871-91a30d74358a.jpeg align="center")

Samsung hesap gizliliği bölümüne girerek buradaki bütün ayarları kapatmanızı tavsiye ederim, nitekim cihaz üzerinde hayati bir engel teşkil etmiyorlar. Tabii ki de tercihinize göre size uygun gelenleri kapatmayabilirsiniz:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750000532131/57cae3cf-3181-44e6-bc19-f05aeb78a9d3.jpeg align="center")

[Samsung Gizlilik Websitesi](https://www.samsung.com/tr/info/privacy/) üzerinden, Samsung'un ne gibi veriler toplayıp bunu işlediğini inceleyebilirsiniz.