---
title: aaaMedia Analytics hello Media Services platformon |} Microsoft Docs
description: "A Médiaelemzés beszéd- és számítógép stratégiai szolgáltatásának vállalati méretezés, a megfelelőségi, a biztonsági és a globális reach gyűjteménye public Preview – áttekintés"
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: c56e3781-8510-4f7f-b5ff-a218c1bb6f4c
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 06/29/2017
ms.author: milanga;juliako;johndeu
ms.openlocfilehash: 7545f0532d7618164ebe65e2f4232c5f63453cfd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="media-analytics-on-hello-media-services-platform"></a>Media Analytics hello Media Services platform
## <a name="overview"></a>Áttekintés
Több szervezet használja videó mint előnyben részesített közepes tootrain hello az alkalmazottak számára, azok az ügyfelek és a dokumentum üzleti funkciók kódolása. A felhőalapú megoldások tartalmaz egy módja toostore adatfolyamként, és nagy médiafájlokat eléréséhez. De videotartalom könyvtárának a vállalat növekedésével egyaránt hatékony elemzések hello tartalom kibontása eszközt kell. 

tooaddress egyre növekvő igénynek, Azure Media Services kínál az Azure Médiaelemzés használatával. Médiaelemzés beszéd- és vizuális összetevők gyűjteménye, amely egyszerűbbé teszi a szervezetek és vállalatok számára tooderive gyakorlatban használható elemzések készítsenek. Media Analytics hello alapösszetevőket Media Services platform használatával készültek, kezelni tud napon az egyik léptékű feldolgozás adathordozó.

Media Analytics fejlesztők is gyorsan vonja fejlett videó funkciói alkalmazások. Vállalati környezetben nyújtja a hello teljes méretezés, a megfelelőségi, a biztonsági és a globális reach szükséges nagy méretű szervezeteknek.

hello alábbi ábrán látható Médiaelemzés és egyéb hello Media Services platform fontosabb részei. 

![VoD-munkafolyamat](./media/media-services-analytics-overview/media-services-analytics-overview01.png)

A Médiaelemzés médiafeldolgozói MP4- vagy JSON-fájlokat hoznak létre. Egy media processzor MP4-fájlokat hoz létre, ha hello fájl fokozatosan lehet letölteni. Egy media processzor JSON-fájlt hoz létre, ha az Azure Blob storage letöltheti hello fájlt. 

## <a name="media-analytics-services"></a>Médiaelemzés-szolgáltatások

### <a name="indexer"></a>Indexelő
Az Azure Media Indexer hogy tartalom kereshető és készítése kódolt-feliratok követi nyomon. Összehasonlított toohello előző verzió, Azure Media Indexer 2 Preview rendelkezik gyorsabban indexelési és szélesebb körű nyelv támogatja. Támogatott nyelvek angol, német, francia, német, olasz, kínai, portugál és arab. Részletes útmutatást és példákat, lásd: [videók feldolgozása az Azure Media Indexer 2](media-services-process-content-with-indexer2.md).
### <a name="hyperlapse"></a>Hyperlapse
Microsoft Hyperlapse videó exportstabilizációt és gyorsított felvétel funkció toocreate gyors és fogyasztható a hosszú tartalomról videók egyesíti. Gyorsított felvétel videó létrehozása, mellett Hyperlapse toocreate stabil videók a mobiltelefonokat és kamerák rögzített shaky videók is használhatja. Részletes útmutatást és példákat, lásd: [Hyperlapse médiafájlok az Azure Media Hyperlapse](media-services-hyperlapse-content.md).
### <a name="motion-detector"></a>Mozgásérzékelő
Mozgásérzékelő toodetect mozgásérzékelési használhatja a helyhez kötött hátterek videó. Így a vakriasztások lehetséges toocheck mozgásérzékelési események által térfigyelő kamerák használatát észlelte. Részletes útmutatást és példákat, lásd: [Mozgásérzékelés az Azure Médiaelemzés használatával](media-services-motion-detection.md).
### <a name="face-detector"></a>Arcérzékelő
Arcfelismerési érzékelő használatával Népi lapokat és a érzelmek, beleértve a Boldogsága, sadness és jelzés nélküli észlelését. Számos hasznos iparági alkalmazások, későbbi, beleértve összesítésére és elemzésére esemény részt vevő személyek reakciók azt. Részletes útmutatást és példákat, lásd: [Arcfelismerés és érzelemfelismerési észlelési az Azure Médiaelemzés használatával](media-services-face-and-emotion-detection.md).
### <a name="video-summarization"></a>Videóösszegzés
Videóösszegzés segítségével létre videók kijelölésével automatikusan érdekes kódtöredékek hello forrás videó. Ez a lehetőség akkor hasznos, ha azt szeretné, hogy milyen hosszú videó tooexpect gyors áttekintést tooprovide. Részletes útmutatást és példákat, lásd: [használata Azure Media videó miniatűrök toocreate videóösszegzés](media-services-video-summarization.md).
### <a name="optical-character-recognition"></a>Optikai karakterfelismerés
Az Azure Media OCR (optikai karakter használata) átalakíthatja videofájlok a szöveges tartalom szerkeszthető, kereshető digitális szöveg. Automatizálható hello kivonása jelentéssel bíró metaadatok hello videó Signal médiafájlt.
### <a name="scalable-face-redaction"></a>Méretezhető arcfelismerési kivonási
Az Azure Media Redactor, amely méretezhető arcfelismerési kivonási hello felhőben nyújt Médiaelemzés media processzort. Arcfelismerési kivonási használatával módosíthatja a kijelölt személyek a videó tooblur felületei. Érdemes lehet toouse hello arcfelismerési kivonási szolgáltatás hírek media, vagy ha nyilvános biztonsági van szó. Több lapokat tartalmazó felvételei, néhány percet is igénybe vehet óra tooredact manuálisan, de ezzel a szolgáltatással arcfelismerési kivonási tart néhány egyszerű lépésben. További információkért lásd: hello [az Azure Media Analytics lapok kivonás](media-services-face-redaction.md) cikk.

## <a name="common-scenarios"></a>Gyakori forgatókönyvek
Media Analytics segítségével a szervezetek és vállalatok glean videó új információkat kaphat, és több hogyan kezelhet hatékonyan videotartalom nagy mennyiségű. Az alábbiakban néhány forgatókönyv:

* **Erőforrások hívás**. Akkor is közösségi hello megjelenésével ügyfél telefonos ügyfélszolgálatok továbbra is megkönnyíteni az ügyfélszolgálati tranzakciók nagy része. Ügyféladatok, amely nagy mennyiségű elemzett tooachieve magasabb ügyfelek elégedettségének a hangadatok kódolású van. Media Indexer használatával a szervezetek szöveg és keresési indexek és irányítópultokat hozhat létre. Majd kinyerhessék eszközintelligencia gyakran panaszkodnak, panaszokat források és egyéb kapcsolódó adatokat.
* **Felhasználó által létrehozott tartalom moderálás**. Hírek media kimeneteket toopolice részlegek, a számos szervezet rendelkezik nyilvánosan elérhető portálok fogadja el a felhasználók által létrehozott adathordozók, például videók és a képeket. hello mennyiségű tartalmat is lefoglalását toounexpected események miatt. Ezekben az esetekben nehéz tooconduct hatékony manuális értékelést, melyekkel tartalom. Az ügyfelek támaszkodhat hello tartalom-moderálás szolgáltatás toofocus a megfelelő tartalom.
* **Felügyeleti**. A hello növekedési használt IP-kamerák egy egyre bővülő leltár felügyeleti videó származnak. Manuálisan megtekintésével felügyeleti videó idő intenzív és hibalehetőségeket rejt magában toohuman hiba. Media Analytics például mozgásérzékelés arcfelismerési észlelési vagy Hyperlapse toomake hello folyamat áttekintése, kezelése és származékaik egyszerűbb létrehozását szolgáltatásokat biztosít.

## <a name="media-analytics-media-processors"></a>Media Analytics media processzor
Hogyan a szakasz a listák hello Médiaelemzés media processzorral és mutat be toouse .NET és REST tooget egy adathordozó processzor (MP) objektum.

### <a name="mp-names"></a>Felügyeleti csomag neve
* Az Azure Media Indexer 2 előzetes verzió
* Azure Media Indexer
* Azure Media Hyperlapse
* Azure Media Face Detector
* Azure Media Motion Detector
* Azure Media Video Thumbnails
* Azure Media OCR

### <a name="net"></a>.NET
a következő függvény vesz egy hello a hello megadott felügyeleti csomag neve és a felügyeleti csomag objektum beállítása/beolvasása.

    static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = _context.MediaProcessors
            .Where(p => p.Name == mediaProcessorName)
            .ToList()
            .OrderBy(p => new Version(p.Version))
            .LastOrDefault();

        if (processor == null)
            throw new ArgumentException(string.Format("Unknown media processor",
                                                       mediaProcessorName));

        return processor;
    }


### <a name="rest"></a>REST
A kérelem:

    GET https://media.windows.net/api/MediaProcessors()?$filter=Name%20eq%20'Azure%20Media%20OCR' HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer <token>
    x-ms-version: 2.12
    Host: media.windows.net

Válasz:

    . . .

    {  
       "odata.metadata":"https://media.windows.net/api/$metadata#MediaProcessors",
       "value":[  
          {  
             "Id":"nb:mpid:UUID:074c3899-d9fb-448f-9ae1-4ebcbe633056",
             "Description":"Azure Media OCR",
             "Name":"Azure Media OCR",
             "Sku":"",
             "Vendor":"Microsoft",
             "Version":"1.1"
          }
       ]
    }

## <a name="demos"></a>Bemutatók
Lásd: [Azure Médiaelemzés használatával bemutatók](http://azuremedialabs.azurewebsites.net/demos/Analytics.html).

## <a name="next-steps"></a>Következő lépések
Tekintse át a Media Services képzési terveket.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-articles"></a>Kapcsolódó cikkek
Lásd: [Médiaelemzés-szolgáltatások közlemény](https://azure.microsoft.com/blog/introducing-azure-media-analytics/).

<!-- Images -->

[overview]: ./media/media-services-video-on-demand-workflow/media-services-video-on-demand.png
