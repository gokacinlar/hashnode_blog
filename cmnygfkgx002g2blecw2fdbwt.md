---
title: "LM Studio + Web Search MCP ile Çevrimdışı Çalışan Local LLM (YZ) Kurulumu (Ubuntu 24.04 + Qwen 3.5)"
seoTitle: "LM Studio + Web Search MCP"
datePublished: 2026-04-14T10:04:14.053Z
cuid: cmnygfkgx002g2blecw2fdbwt
slug: lm-studio-web-search-mcp-ile-evrimd-al-an-local-llm-yz-kurulumu-ubuntu-24-04-qwen-3-5
cover: https://cdn.hashnode.com/uploads/covers/684d7cffa7664b2f13d48092/e05298ae-cba7-4589-8e42-348e012503c8.jpg
tags: llm, yapay-zeka, lmstudio, local-llm, qwen, mcp, qwen-35

---

## Giriş

Günümüzde yapay zeka modellerini, standart bir kullanıcı herhangi bir *mainstream* (ana akım) YZ (Yapay zeka) çözümleri satan OpenAI, xAI, Google veya Microsoft gibi platformlardan kullanıyor. Herhangi bir gerçek zamanlı verinin, araştırma maksatlı yorumlanmasından tutun da sıradan bir *meme* için mizah malzemesi olarak resim üretmeye kadar pek çok alanda yaygın kullanımları var. Kullanıcılar *Grok*'tan, *Gemini*'dan veya herhangi bir YZ sağlayıcısından bir şekilde bu isteklerini gideriyorlar ve onların, kabaca 2021'den itibaren alışık oldukları YZ kullanım metodu ve alışkanlıkları bu yönde.

Fakat bunları yaparlarken gözden kaçırdıkları şey, başta [**gizlilik ihlallerine gönüllü rıza gösterdiklerini dahi bilmemeleri**](https://news.northeastern.edu/2025/11/21/five-ways-llms-expose-your-personal-data/) ve kendilerinin bizatihi vermiş oldukları bilginin, yapay zeka modellerini eğitmek ve eğitildiği sürece de [size odaklı reklam (*targeted advertisement*)](https://insights.manageengine.com/artificial-intelligence/ads-in-llms-and-the-expansion-of-the-surveillance-economy/) veya [detaylı profilleme](https://news.stanford.edu/stories/2025/10/ai-chatbot-privacy-concerns-risks-research) ve [verinin parasallaştırılması (*data monetization*)](https://thebulletin.org/2025/08/how-ai-and-surveillance-capitalism-are-undermining-democracy/) ile kazanç maksatlı kullanımının endüstride neredeyse bir standart olduğundan veya bir kere geniş bir kullanıcı kitlesi edindiklerinde, sonradan farklı sebeplerle (genellikle keyfi kaprislere dayanır) kullanım şartlarını [kötü yönde değiştirdiklerinden](https://archive.is/P25wq) haberdar olmamalarıdır.

Tabii ki, bu son kullanıcılar için belki de "gözden çıkarılabilir" bir fedakarlık gibi görünebilir nihayetinde istediklerini elde ediyorlar fakat bu iş en nihayetinde bir kullanım ve **güç** meselesidir ve gereksiz yere *compute* yakmak veya oligopol derecesindeki şirketlere kendinizi metalaştırmak yerine; kendi makinenizde çalışan, **hızlı**, **güvenli** ve **gizli** bir yapay zeka çözümünü tercih etmek, üstelik gerçekten de işe yarayan kullanım senaryolarında bir standart olarak benimsemek oldukça mantıklı olabilir. İşte bu noktada, devreye **LOCAL AI** giriyor, yani *yerel yapay zeka.*

Bunu gerçekleştirmek için herhangi bir işletim sistemi (Linux, MacOS & Windows), [LM Studio](https://lmstudio.ai/), bu yazıda örnek olarak kullanacağımız [`Qwen 3.5`](https://lmstudio.ai/models/qwen/qwen3.5-9b) modelini ve açık kaynak bir `MCP` sunucusu olan [`web-search-mcp`](https://github.com/mrkrsl/web-search-mcp) yazılımını kullanacağız (LM Hub'dan edinilebilen [DuckDuckGo entegrasyonu](https://lmstudio.ai/danielsig/duckduckgo) da tercih edilebilir tabii ki de). `MCP` esasında sizin yapay zeka modellerinizin çeşitli veri kaynaklarına bağlanmasına izin veren bir standart. Bu standardın *over-engineered* olduğu, başka bir ekstra protokole gerek duyulmadığı veya *"API veya CLI dururken neden buna ihtiyacımız var?"* diye [pek çok eleştiri](https://www.forbes.com/councils/forbestechcouncil/2025/06/12/mcp-will-fail-and-heres-why/) alıyordu fakat günün sonunda işini gören bir teknoloji. Açık kaynak ve ücretsiz mi? Devam...

Kısaca ihtiyacımız olan gereksinimleri listeledim - işletim sistemi farklı olabilir:

*   Ubuntu 24.04
    
*   **Opsiyonel:** AMDGPU + ROCm
    
*   LM Studio
    
*   LM Studio'da çalışacak hafif (düşük sistemler için `LfM 1.2`) veya donanımızın izin verdiği ölçüde uygun bir YZ modeli (yazı için `Qwen 3.5` temel alındı).
    
*   [`Node.js (24)`](https://nodejs.org/tr/download)
    
*   `web-search-mcp`
    
*   15 dakika 😹
    

## Ubuntu 24.04 için AMDGPU - ROCm kurulumu (opsiyonel)

Esasında AMD'nin sürücü paketleri, Linux çekirdeğine gömülü halde gelmektedir çünkü AMD, kapalı kaynak sürücülerine *(Windows'larda kullanılan sürücüler)* ekstra açık kaynaklı sürücüleri de yayınlar ve geliştiriciler bunları [çekirdeğe ekler](https://news.ycombinator.com/item?id=43780660). Fakat bizim senaryomuzda maksimum verimi alabilmek için, NVIDIA'nın CUDA'sına eşdeğer (terimsel olarak, [teknoloji olarak değil](https://www.youtube.com/watch?v=Mr0rWJhv9jU)) [ROCM](https://en.wikipedia.org/wiki/ROCm) kurulumu da yapmamız faydalı olduğundan bunu yüklememiz bizim için işe yarayacaktır.

[Bu adresten](https://www.amd.com/en/support/download/linux-drivers.html) `amdgpu-install` isimli `.deb` dosyasını indirerek, gerek Ubuntu'nun kendi Uygulama Merkezi'nden veya `gdebi` gibi arayüzlerden yükleyerek `amdgpu-install` kurulumunu yapın. Ardından terminale kısaca `amdgpu-install` yazarak script'in otomatik olarak kurulumu yapmasını bekleyin.

> Kurulum aşamasında eğer secure boot aktifse, yazılım sizden bir [MOK anahtarı girmenizi isteyecek.](https://amdgpu-install.readthedocs.io/en/21.10/install-installing.html#secure-boot-support) Bu anahtarı aklınızda tutun çünkü sistemi yeniden başlattığınızda tekrardan bunu girmeniz gerekecek.

### ROCm Kurulumu

Ubuntu 26.04'te ROCm [standart bir paket olarak gelecek](https://www.phoronix.com/news/Ubuntu-26.04-ROCm-State) fakat 24.04'te bu böyle değil. ROCm'i yüklemek için kısaca `sudo apt install rocm` diyerek kurulumu yapabilirsiniz. Kurulumu onaylamak için bitiminde `rocm-smi` yazdığınızda terminal size çıktı veriyorsa yükleme tamamlanmıştır ve grafik kartlarınızı belirli özellikleriyle görebilirsiniz:

![](https://cdn.hashnode.com/uploads/covers/684d7cffa7664b2f13d48092/3deb8567-d4e7-4d8a-bbf8-3c450392b694.png align="middle")

## LM Studio ve `Qwen 3.5` (veya başka bir model) indirilmesi & kurulumu

%[https://lmstudio.ai/] 

Yukarıdaki adresten LM Studio'yu indirip makinenize kurun. Yazılım sizin donanımınıza uygun model önerecektir. İsterseniz onu kurabilirsiniz veya yazımızın temeli olan Qwen 3.5'i kurabilirsiniz. Bende zaten yüklü:

![](https://cdn.hashnode.com/uploads/covers/684d7cffa7664b2f13d48092/882d2e6c-3bb6-4a0e-baa1-fc4c8b82019c.png align="middle")

## `web-search-mcp` kurulumu

%[https://github.com/mrkrsl/web-search-mcp] 

GitHub repo'sundaki adımları izleyerek kurulumu gerçekleştirin. [TL;DR](https://github.com/mrkrsl/web-search-mcp?tab=readme-ov-file#installation-recommended).

## MCP konfigürasyonu ve birkaç ayar

LM Studio'nun sağ tarafında, *"Integrations"* kısmına gelerek mcp sunucusunu LM Studio'ya tanıtmamız gerekiyor.

![](https://cdn.hashnode.com/uploads/covers/684d7cffa7664b2f13d48092/7be75326-457b-401a-aa38-fc4c8ef785fd.png align="middle")

Girdiğimizde, dokümantasyonda olan şu kodu, `web-search-mcp`'yi kurduğunuz konumu kopyalayarak eklemeniz gerekiyor. Ben kullanıcı dizinine kurduğum için konum şu şekilde gözükecektir:

![](https://cdn.hashnode.com/uploads/covers/684d7cffa7664b2f13d48092/36d753da-4d39-424e-b0fa-52cedf15652b.png align="middle")

Eğer kurulum başarılı olduysa, mcp'yi etkinleştirdiğinizde şöyle çeşitli seçenekler ortaya çıkacaktır:

![](https://cdn.hashnode.com/uploads/covers/684d7cffa7664b2f13d48092/a087fdeb-10c4-4871-85af-27ae6b12668f.png align="middle")

Tüm kurulumları gerçekleştirdiğimize göre artık ufaktan bazı sorularımızı yapay zekaya sorup cevap alalım.

## Test Prompt'u

Prompt: *Hey, LLM'lerin bağlam penceresi ne demektir? İnternete bakabilir misin?*

Yapay Zeka, öncelikle LM Studio'ya entegre ettiğimiz MCP sunucusuna erişip, ardından bu mcp üzerinden arama motorlarına "*query*" göndererek içeriklere erişiyor ve yapay zekayı besliyor:

![](https://cdn.hashnode.com/uploads/covers/684d7cffa7664b2f13d48092/5c3daa72-ef51-4ed5-a5ca-4476e02e8aab.png align="middle")

İşte! Yerelde çalışan, tamamen bize ait olan, gerçekten de işe yarayan (en önemli nokta burası aslında) ve *AI-as-a-Service* tipi hizmetlerden arındırılmış ücretsiz & gizli Yapay Zeka çözümü!

## Not

Nisan 2026 itibariyle bazı siteler, AI botlarının acımasız [scraping](https://en.wikipedia.org/wiki/Web_scraping) taktikleri yüzünden sitelerini web erişimine kapatıyorlar. Bu sebeple bu özelliği kullanırken bazı sitelere erişiminiz `web-search-mcp`'nin `crawler`'ını [*(Chromium instance'inde çalışan arama motoru entegrasyonu)*](https://github.com/mrkrsl/web-search-mcp?tab=readme-ov-file#how-it-works) kullandığı için olmayabilir; nihayetinde pek bilinmese de yapay zeka dünyasında "veri hırsızlığı" oldukça tartışmalı bir konu ve tüm dünyanın bilgisini alıp, bir modele besleterek bunu parayla satmanın ne kadar etik olduğu [tartışılıyor](https://jskfellows.stanford.edu/theft-is-not-fair-use-474e11f0d063). YZ'yi, etik bir çerçevede değerlendirirken mutlaka David Bushell'in ["YZ Politikası" isimli sayfasından](https://dbushell.com/ai/) esinleniyorum. Neyin ne olduğunu görmeme oldukça yardımcı oluyor.

İyi kodlamalar!