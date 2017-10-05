---
title: "Az Azure portálon keresztül Media Encoder Standard kódolása |} Microsoft Docs"
description: "Ez az oktatóanyag végigvezeti a kódolást és az Azure portál Media Encoder Standard keresztül."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 107d9e9a-71e9-43e5-b17c-6e00983aceab
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/07/2017
ms.author: juliako
ms.openlocfilehash: a299245e285c4caa68988b184799cd6f4d13e080
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="encode-an-asset-using-media-encoder-standard-with-the-azure-portal"></a>Az Azure portálon keresztül Media Encoder Standard kódolása
> [!NOTE]
> Az oktatóanyag elvégzéséhez egy Azure-fiókra lesz szüksége. További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/). 
> 
> 

Az Azure Media Services egyik legnépszerűbb funkciója, amikor a portál használatával adaptív sávszélességű streamelést biztosítunk az ügyfelek számára. A Media Services a következő adaptív sávszélességű streamelési technológiákat támogatja: HTTP Live Streaming (HLS), Smooth Streaming és MPEG DASH. A videók adaptív sávszélességű streameléséhez többszörös sávszélességű fájlokká kell kódolnia a forrásvideókat. Javasoljuk, hogy a videók kódolásához használja a **Media Encoder Standard** kódolót.  

A Media Services is biztosít a dinamikus csomagolás, amely lehetővé teszi, hogy a következő formátumban közvetíteni többszörös sávszélességű MP4: MPEG DASH, HLS, Smooth Streaming, anélkül, hogy át kellene őket csomagolni ezekbe a streamformátumokba. A dinamikus becsomagolás révén elég egyetlen tárolási formátumban tárolni a fájlokat (és kifizetni a tárhelyüket), a Media Services elkészíti és kiszolgálja az ügyféltől érkező kérésnek megfelelő választ.

Annak érdekében, hogy kihasználhassa a dinamikus csomagolást, kódolja többszörös sávszélességű MP4-fájlokká a forrásfájlt (a kódolás lépéseit egy későbbi részben találja meg).

Media feldolgozási méretezési, lásd: [ez](media-services-portal-scale-media-processing.md) témakör.

## <a name="encode-with-the-azure-portal"></a>Az Azure portálon kódolása
Ebben a részben leírjuk, milyen lépéseket kell elvégeznie a tartalmaknak a Media Encoder Standard segítségével történő kódolásához.

1. Az [Azure-portálon](https://portal.azure.com/) válassza ki Azure Media Services-fiókját.
2. A **Settings** (Beállítások) ablakban válassza az **Assets** (Objektumok) lehetőséget.  
3. Az **Assets** (Objektumok) ablakban válassza ki a kódolni kívánt objektumot.
4. Nyomja meg az **Encode** (Kódolás) gombot.
5. Az **Encode an asset** (Objektum kódolása) ablakban válassza a „Media Encoder Standard” feldolgozóeszközt, és egy beállításkészletet. A beállításkészletekkel kapcsolatos információkért lásd [a sávszélességi skála automatikus létrehozását](media-services-autogen-bitrate-ladder-with-mes.md) és a [MES előre beállított feladatait](media-services-mes-presets-overview.md) ismertető részeket. Ha szeretné szabályozni, hogy a rendszer mely kódolási beállításkészletet használja, ne feledje: rendkívül fontos, hogy a bemeneti videóhoz leginkább megfelelő készletet válassza. Ha például tudja, hogy a bemeneti videó felbontása 1920 x 1080 képpont, akkor használja a „H264 Multiple Bitrate 1080p” beállításkészletet. Ha például alacsony felbontású (640 x 360) videót szeretne kódolni, ne válassza a „H264 Multiple Bitrate 1080p” beállításkészletet.
   
   Az egyszerűbb kezelés érdekében lehetősége van módosítani a kimeneti objektum nevét, illetve a feladat nevét.
   
   ![Objektumok kódolása](./media/media-services-portal-vod-get-started/media-services-encode1.png)
6. Nyomja meg a **Create** (Létrehozás) gombot.

## <a name="next-step"></a>Következő lépés
Kódolási feladatok előrehaladásának és az Azure portál, a figyelheti [ez](media-services-portal-check-job-progress.md) cikk.  

## <a name="media-services-learning-paths"></a>Media Services képzési tervek
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

