---
title: "aaaOverview és összehasonlítása az Azure media kódolók igény |} Microsoft Docs"
description: "Ez a témakör minden áttekintése és az igény szerinti media kódolók Azure összehasonlítása."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: e6bfc068-fa46-4d68-b1ce-9092c8f3a3c9
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/10/2017
ms.author: juliako
ms.openlocfilehash: 24a3e0a16162b1bebfcde290b6baf2dd8dbfff17
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="overview-and-comparison-of-azure-on-demand-media-encoders"></a>Áttekintés és az igény szerinti media kódolók Azure összehasonlítása
## <a name="encoding-overview"></a>Kódolási áttekintése
Az Azure Media Services hello kódolás hello felhőben adathordozó több lehetőséget biztosít.

Amikor kezdte meg a Media Services, fontos toounderstand hello különbségének kodekeket és a fájl formátuma nem.
A kodekeket hello szoftver, amely hello tömörítési/kitömörítés algoritmusok, mivel fájlformátumok tárolói tömörített hello videó tartsa.

A Media Services dinamikus becsomagolást, amely lehetővé teszi az adaptív sávszélességű MP4 vagy Smooth Streaming-kódolású tartalmak (MPEG DASH, HLS, Smooth Streaming) Media Services által támogatott streamformátumok toodeliver biztosít anélkül, hogy toore-csomagját ezek adatfolyam-továbbítási formátumokba.

>[!NOTE]
>Az AMS-fiók létrehozásakor egy **alapértelmezett** adatfolyam-továbbítási végpontra tooyour fiók kerül hello **leállítva** állapotát. a dinamikus csomagolás és a dinamikus titkosítás tartalmat, és hajtsa végre a megfelelő előnyeit streaming toostart hello streamvégpontra, amelyből el kívánja toostream tartalom toobe rendelkezik hello **futtató** állapotát. tootake előnyeit [dinamikus becsomagolás](media-services-dynamic-packaging-overview.md), toodo hello következőkre lesz szüksége:
>
>Emellett kódolja a forrásfájlt adaptív sávszélességű MP4-vagy adaptív sávszélességű Smooth Streaming-fájlsorozattá (az oktatóanyag későbbi részében hello kódolás lépéseit egy).

A Media Services hello következő igény szerinti kódolók ebben a cikkben ismertetett támogatja:

* [Media Encoder Standard](media-services-encode-asset.md#media-encoder-standard)
* [Media Encoder Premium-munkafolyamat](media-services-encode-asset.md#media-encoder-premium-workflow)

Ez a cikk rövid áttekintést nyújt az igény szerinti media kódolók, és hivatkozások tooarticles, amely részletesebb információkat biztosít. hello is témakör hello kódolók összehasonlítása.

>[!NOTE]
>Alapértelmezés szerint minden Media Services-fiók lehet aktív kódolási feladat is egyszerre. Kódolási egység, amely lehetővé teszi toohave több kódolási feladat fut egyidejűleg, egy minden egyes megvásárolt kódolási fenntartott egységnek a foglalhat. További információ: [kódolási egységek méretezése](media-services-scale-media-processing-overview.md).

## <a name="media-encoder-standard"></a>Media Encoder Standard
### <a name="how-toouse"></a>Hogyan toouse
[Hogyan tooencode a Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md)

### <a name="formats"></a>Formázza
[Formátumok és kodekek](media-services-media-encoder-standard-formats.md)

### <a name="presets"></a>Készletek
Media Encoder Standard segítségével konfigurálható: az egyik hello kódoló készletek leírt [Itt](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).

### <a name="input-and-output-metadata"></a>Bemeneti és kimeneti metaadatok
hello kódolók bemeneti metaadatok leírását [Itt](media-services-input-metadata-schema.md).

hello kódolók kimeneti metaadatok leírását [Itt](media-services-output-metadata-schema.md).

### <a name="generate-thumbnails"></a>Indexképének létrehozására
További információ: [hogyan Media Encoder Standard használatával toogenerate miniatűrök](media-services-advanced-encoding-with-mes.md#thumbnails).

### <a name="trim-videos-clipping"></a>Trim (vágás) videók
További információ: [hogyan Media Encoder Standard használatával tootrim videók](media-services-advanced-encoding-with-mes.md#trim_video).

### <a name="create-overlays"></a>Átfedések létrehozása
További információ: [hogyan Media Encoder Standard használatával toocreate átfedések](media-services-advanced-encoding-with-mes.md#overlay).

### <a name="see-also"></a>Lásd még:
[hello Media Services blog](https://azure.microsoft.com/blog/2015/07/16/announcing-the-general-availability-of-media-encoder-standard/)

## <a name="media-encoder-premium-workflow"></a>Media Encoder Premium-munkafolyamat
### <a name="overview"></a>Áttekintés
[Prémium szintű kódolás az Azure Media Services bemutatása](https://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services/)

### <a name="how-toouse"></a>Hogyan toouse
Media Encoder prémium munkafolyamat bonyolult munkafolyamatok segítségével lett konfigurálva. Munkafolyamat-fájlok létrehozása sikerült, és a hello frissítve [munkafolyamat-Tervező](media-services-workflow-designer.md) eszköz.

[Hogyan prémium szintű Azure Media Services kódolási tooUse](https://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services/)

### <a name="known-issues"></a>Ismert problémák
Ha a bemeneti videó nem tartalmaz a kódolt, a hello kimeneti eszköz továbbra is egy üres TTML fájlt fogja tartalmazni.


## <a name="media-services-learning-paths"></a>Media Services képzési tervek
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-articles"></a>Kapcsolódó cikkek
* [Speciális feladatokra kódolási Media Encoder Standard készletek testreszabásával.](media-services-custom-mes-presets-with-dotnet.md)
* [Kvóták és korlátozások](media-services-quotas-and-limitations.md)

<!--Reference links in article-->
[1]: http://azure.microsoft.com/pricing/details/media-services/
