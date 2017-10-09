---
title: "aaaScaling Media feldolgozása áttekintése |} Microsoft Docs"
description: "Ez a témakör a méretezési Media feldolgozásának az Azure Media Services nyújt áttekintést."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 780ef5c2-3bd6-4261-8540-6dee77041387
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/04/2017
ms.author: juliako
ms.openlocfilehash: d17531f79d4c1e0d0fa544c4fb5c083684e706fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="scaling-media-processing-overview"></a>Méretezési Media feldolgozása – áttekintés
Ezen a lapon áttekintést útmutató és érvek tooscale media feldolgozása. 

## <a name="overview"></a>Áttekintés
Egy Media Services-fiók fenntartott egység típusú, amely megadja, hogy hello sebesség, amellyel a feladatok feldolgozása media feldolgozása hozzá rendelve. Hello következő közötti választhatja ki a szolgáltatás számára fenntartott egység: **S1**, **S2**, vagy **S3**. Például ugyanazon kódolási feladat fut gyorsabban hello használatakor hello **S2** fenntartott egységnek típus összehasonlítása toohello **S1** típusa. További információkért lásd: hello [fenntartott egység típusok](https://azure.microsoft.com/blog/high-speed-encoding-with-azure-media-services/).

Ezenkívül toospecifying hello fenntartott egység típusát, akkor megadhatja tooprovision fiókja fenntartott egységek használatával. kiépített fenntartott egységek számának hello hello egyidejűleg dolgozhatók fel egy adott fiókhoz media feladatok száma határozza meg. Például ha a fiók nem rendelkezik öt fenntartott egységek, majd öt media feladatokat futtatni egyidejűleg hosszú, amennyi a feladatok toobe feldolgozása. hello feladataiban hello várólistában és egymás után feldolgozásához, amikor egy futó feladat befejezése után fog beolvasása felvételre. Ha a fiók nem rendelkezik bármely fenntartott egységek kiosztása, majd feladatok fog felvételre egymás után. Ebben az esetben hello Várjon, amíg egy tevékenység befejezése között eltelt idő, és hello mellett egy kezdési függ erőforrások hello rendelkezésre állását hello rendszerben.

## <a name="choosing-between-different-reserved-unit-types"></a>Választás a különböző fenntartott egység
hello a következő táblázat segítséget nyújt a különböző kódolási sebességű közötti kiválasztásakor a döntést. Emellett néhány teljesítményteszt esetekben biztosít, és használhatja toodownload videók, amelyen elvégezhető SAS URL-érdemes saját teszteket végezni biztosít:

| Forgatókönyvek | **S1** | **S2** | **S3** |
| --- | --- | --- | --- |
| Tervezett használati eset |Egyszeres sávszélességű kódolását. <br/>Fájlok SD vagy az alatti megoldások, nem idő-és nagybetűket, az alacsony költségű. |Egyszeres sávszélességű, és több sávszélességű kódolását.<br/>Normál használati SD és a HD kódolására. |Egyszeres sávszélességű, és több sávszélességű kódolását.<br/>Teljes HD és 4K feloldási videók. Idő-és nagybetűket, gyorsabb esetenként kódolást. |
| Teljesítményteszt |[A bemeneti fájl: 5 perc hosszú 640x360p: 29,97 keretek/másodperc](https://wamspartners.blob.core.windows.net/for-long-term-share/Whistler_5min_360p30.mp4?sr=c&si=AzureDotComReadOnly&sig=OY0TZ%2BP2jLK7vmcQsCTAWl33GIVCu67I02pgarkCTNw%3D).<br/><br/>Kódolás tooa egyféle sávszélességű MP4-fájl, a hello azonos megoldás körülbelül 11 percet vesz igénybe. |[A bemeneti fájl: 5 perc hosszú 1280x720p: 29,97 keretek/másodperc](https://wamspartners.blob.core.windows.net/for-long-term-share/Whistler_5min_720p30.mp4?sr=c&si=AzureDotComReadOnly&sig=OY0TZ%2BP2jLK7vmcQsCTAWl33GIVCu67I02pgarkCTNw%3D)<br/><br/>"H264 egyféle sávszélességű 720p" kódolás előre beállított vesz körülbelül 5 percet.<br/><br/>Kódolás "H264 Multiple Bitrate 720p" beállításkészletet körülbelül 11,5 percet vesz igénybe. |[A bemeneti fájl: 5 perc hosszú 1920x1080p: 29,97 keretek/másodperc](https://wamspartners.blob.core.windows.net/for-long-term-share/Whistler_5min_1080p30.mp4?sr=c&si=AzureDotComReadOnly&sig=OY0TZ%2BP2jLK7vmcQsCTAWl33GIVCu67I02pgarkCTNw%3D). <br/><br/>"H264 egyetlen Bitrate 1080p" kódolás előre beállított vesz körülbelül 2.7 percet.<br/><br/>Kódolás "H264 Multiple Bitrate 1080p" beállításkészletet körülbelül 5.7 percet vesz igénybe. |

## <a name="considerations"></a>Megfontolandó szempontok
> [!IMPORTANT]
> Tekintse át a jelen szakaszban ismertetett szempontok.  
> 
> 

* Fenntartott egységek működnek az összes adathordozó feldolgozási, beleértve az Azure Media Indexer használó feladatok indexelő parallelizing.  De a kódolással ellentétben az indexelési feladatok feldolgozása nem lesz gyorsabb a gyorsabb Fenntartott egységekkel.
* Ha megosztott hello segítségével készlet, ez azt jelenti, hogy anélkül szolgáltatás számára fenntartott egység, akkor a encode feladatok kell hello ugyanazok, mint a S1 RUs. Azonban nincs felső korlátja toohello idő a feladatok képes költeni várakozó állapotban, és egy adott időpontban legfeljebb csak egy feladat fog futni.
* a következő adatközpontokban hello nem képes hello **S2** szolgáltatás számára fenntartott egység típusát: és a Dél-Brazília, Nyugat-India.
* hello következő adatközpont nem kínál hello **S3** szolgáltatás számára fenntartott egység típusát: Nyugat-India.

## <a name="billing"></a>Számlázás

A számlázás a Media szolgáltatás számára fenntartott egységek tényleges percalapú használatán alapul. Részletes leírását, című rész hello gyakran ismételt kérdések hello [Media Services díjszabása](https://azure.microsoft.com/pricing/details/media-services/) lap.   

## <a name="quotas-and-limitations"></a>Kvóták és korlátozások
További információ a kvóták és korlátozások, és hogyan tooopen egy támogatási jegy [kvóták és korlátozások](media-services-quotas-and-limitations.md).

## <a name="next-step"></a>Következő lépés
Ezek a technológiák egyike feladat feldolgozása media skálázás hello elérése: 

> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-encoding-units.md)
> * [Portál](media-services-portal-scale-media-processing.md)
> * [REST](https://docs.microsoft.com/rest/api/media/operations/encodingreservedunittype)
> * [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
> * [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
> 
> 

## <a name="media-services-learning-paths"></a>Media Services képzési tervek
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

