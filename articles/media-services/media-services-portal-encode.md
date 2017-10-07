---
title: "a hello Azure-portálon keresztül Media Encoder Standard aaaEncode |} Microsoft Docs"
description: "Ez az oktatóanyag végigvezeti hello kódolási hello Azure-portálon a Media Encoder Standard keresztül."
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
ms.openlocfilehash: 0d118bbbe1fa9f4ba0bfa3ea3b10fb541d1d6379
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="encode-an-asset-using-media-encoder-standard-with-hello-azure-portal"></a>Egy eszköz használata a Media Encoder Standard hello Azure-portálon kódolása
> [!NOTE]
> toocomplete ebben az oktatóanyagban egy Azure-fiókra van szüksége. További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/). 
> 
> 

Ha az Azure Media Services egyik leggyakoribb forgatókönyve hello elkötelezett az adaptív sávszélességű streamelési tooyour ügyfelek működik. Media Services hello a következő adaptív sávszélességű streamelési technológiákat támogatja: HTTP Live Streaming (HLS), Smooth Streaming, MPEG DASH. tooprepare videók adaptív sávszélességű streamelés esetén meg kell tooencode a forrás videó többszörös sávszélességű fájlokba. Használjon hello **Media Encoder Standard** kódoló tooencode videók.  

A Media Services dinamikus becsomagolást is biztosít, amely lehetővé teszi a toodeliver közvetíteni többszörös sávszélességű MP4 a hello a következő formátumban: MPEG DASH, HLS, Smooth Streaming, anélkül, hogy toore-csomagját ezekbe a formátumokba. A dinamikus csomagolás csak akkor kell toostore és fizetési hello fájlokat egyetlen tárolási formátumban és a Media Services elkészíti és kiszolgálja az ügyféltől érkező kérésnek megfelelő választ hello.

tootake előny dinamikus becsomagolás kell tooencode a forrásfájlt (hello kódolás lépéseit egy későbbi részében) többszörös sávszélességű MP4-fájlokat alakítja.

tooscale media feldolgozása, lásd: [ez](media-services-portal-scale-media-processing.md) témakör.

## <a name="encode-with-hello-azure-portal"></a>Az Azure-portálon hello kódolása
Ez a szakasz ismerteti a hello lépéseket, amelyek tooencode a tartalmaknak a Media Encoder Standard.

1. A hello [Azure-portálon](https://portal.azure.com/), válassza ki az Azure Media Services-fiók.
2. A hello **beállítások** ablakban válassza ki **eszközök**.  
3. A hello **eszközök** ablakot, hogy szeretné-e tooencode válassza hello eszköz.
4. Nyomja le az hello **Encode** gombra.
5. A hello **kódolása egy eszköz** ablak, jelölje be hello "Media Encoder Standard" feldolgozóeszközt, és egy beállításkészletet. A beállításkészletekkel kapcsolatos információkért lásd [a sávszélességi skála automatikus létrehozását](media-services-autogen-bitrate-ladder-with-mes.md) és a [MES előre beállított feladatait](media-services-mes-presets-overview.md) ismertető részeket. Ha azt tervezi, hogy mely kódolási beállításkészlet használt toocontrol, ezt tartsa szem előtt: fontos tooselect hello készlet, amely a legjobban megfelelő a bemeneti videó. Például ha tudja, hogy a bemeneti videó felbontása 1920 x 1080 képpont rendelkezik, akkor használja hello "H264 Multiple Bitrate 1080p" beállításkészletet. Ha például alacsony felbontású (640 x 360) videót szeretne kódolni, ne válassza a „H264 Multiple Bitrate 1080p” beállításkészletet.
   
   Az egyszerűbb kezelés érdekében lehetősége van a szerkesztési hello hello kimeneti eszköz, és hello nevét hello feladat.
   
   ![Objektumok kódolása](./media/media-services-portal-vod-get-started/media-services-encode1.png)
6. Nyomja meg a **Create** (Létrehozás) gombot.

## <a name="next-step"></a>Következő lépés
Kódolási feladatok előrehaladásának hello Azure-portálon a figyelheti a [ez](media-services-portal-check-job-progress.md) cikk.  

## <a name="media-services-learning-paths"></a>Media Services képzési tervek
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

