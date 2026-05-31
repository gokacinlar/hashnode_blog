---
title: "Git Versiyon Kontrol & Takip Sisteminde "Tag"lerin (Etiket) Önemi ve Kullanımı"
seoTitle: "Git Versiyon Kontrol & Takip Sisteminde "Tag"ler (Etiket)"
seoDescription: "Git Versiyon Kontrol & Takip Sisteminde "Tag"lerin (Etiket) Önemi ve Kullanımı"
datePublished: 2026-05-31T18:02:12.461Z
cuid: cmpu37a7e00021sjnbwamcbc2
slug: git-versiyon-kontrol-takip-sisteminde-tag-lerin-etiket-nemi-ve-kullan-m
cover: https://cdn.hashnode.com/uploads/covers/684d7cffa7664b2f13d48092/a8ab62a1-cd36-4e97-b10f-a9b7b442b483.png
tags: git, tags

---

## Git'te "Tag" (Etiket) Nedir?

Git versiyon kontrol sisteminde etiketler, basitçe projenize ait yayınladığınız yazılım sürümlerini, git içerisinde takip edip referans vermek için kullanılır. Yani projenizde belirli bir aşamaya geldiğinizde çeşitli sürümlerle yayınladığınız (v1.2.3, v2, build2 vs.) programınızın o durumunu, bir etiket oluşturarak *commit*'leri ile birlikte sürümle ilişkilendirerek projenizi düzenlemeye yarar. Linux sisteminin bir parçası olan [*BTRFS*](https://en.wikipedia.org/wiki/Btrfs#Subvolumes_and_snapshots) dosya sisteminden aşina olacağınız şekilde, sisteminizin belirli bir görünümünü **snapshot** olarak almasıyla tıpatıp benzerdir. Etiketler hakkında bilmeniz gereken en önemli özellik, bir git etiketinin **immutable** olmasıdır**.** Yani bir branch'te olduğu gibi oluşturduğunuzda üzerine commit ekleyerek devam edemezsiniz. Geçmişe dönük, projenin belirli bir aşamasını (sürümle belirtilir genellikle) referans olarak olduğu gibi kalır.

İki türlü git etiket türü vardır:

1.  Kısa (lightweight) Etiket
    
2.  Açıklamalı (annotated) Etiket
    

## Kısa Git Etiket (*Lightweight Tag*)

Öncelikle projenizdeki etiketleri listeleyerek işe başlayabilirsiniz:

```shell
$ git tag --list

1.2.1
1.2.2
1.2.3
1.2.4
```

Bu benim projemde olan etiketlerin kendisidir. Eğer sizde yoksa ve yayınlamak istediğiniz bir sürüme gelmiş projeniz varsa, kısa bir git etiketi oluşturarak başlayalım:

```shell
$ git tag 2.0.0 # 2'nci sürümünde uygulamamıza bir etiket ekleyelim
```

Bu etiketi ekledikten sonra projemize eklemek için ise;

```shell
$ git push origin "tag ismi" # Buraya etiket ismini yazın
```

...diyerek ekleyebiliriz.

Silme işlemi için ise gerek yerelde gerek ise repo'muzda etiket silme işlemimizi yapabiliriz:

```shell
$ git tag --delete "tag ismi" # Yerelde silinir
$ git tag origin --delete  "tag ismi" # Repo'dan silinir
```

## Açıklamalı Git Etiket (*Annotated Tag*)

Açıklamalı git etiketi için ise basitçe iki parametre kullanırız:

```shell
$ git tag -a "etiket ismi" -m "etiket için mesaj"
```

Başka bir yazımda görüşmek üzere!