---
title: "aaaAzure Media Services dinamikus becsomagolás áttekintése |} Microsoft Docs"
description: "hello témakör által biztosított és a dinamikus becsomagolás áttekintése."
author: Juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: 0d9e4f54-5daa-45c1-bfaa-cf09ca89b812
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: juliako
ms.openlocfilehash: 970e24eba800e098774172c87f56629430b227a9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="dynamic-packaging"></a>Dinamikus csomagolás
## <a name="overview"></a>Áttekintés
A Microsoft Azure Media Services lehet használt toodeliver sok media forrás fájlformátumokat, media formátumban és a védett tartalom formátumok tooa számos ügyfél technológiák (például iOS, XBOX, a Silverlight, a Windows 8). Ezek az ügyfelek ismertetése különböző protokollok, például iOS igényel-e egy HTTP-Live Streaming (HLS) V4 formátum, és a Silverlight- és Xbox igényelnek, Smooth Streaming. Ha rendelkezik egy adaptív sávszélességű (többféle sávszélességű) készletét MP4 (ISO talál médiafájlok 14496-12) vagy egy adaptív sávszélességű Smooth Streaming-fájlsorozattá, hogy szeretné-e, ismernie MPEG DASH, HLS vagy Smooth Streaming tooserve tooclients, meg kell előnyeit adathordozó Services dinamikus becsomagolást.

A dinamikus csomagolás szüksége van egy eszköz, amely tartalmazza az adaptív sávszélességű MP4-vagy adaptív sávszélességű Smooth Streaming-fájlsorozattá toocreate. Majd hello hello jegyzékben megadott formátumnak alapján kérelem darabolható, vagy igény szerinti adatfolyam-kiszolgáló biztosítja a megjelenő hello adatfolyam a kiválasztott protokollal hello hello. Ennek eredményeképpen csak akkor kell toostore és fizetési hello fájlokat egyetlen tárolási formátumban és a Media Services szolgáltatás elkészíti és kiszolgálja az ügyféltől érkező kérésnek megfelelő választ hello.

hello alábbi ábrán látható hello hagyományos kódolás és statikus csomagolás munkafolyamat.

![Statikus kódolás](./media/media-services-dynamic-packaging-overview/media-services-static-packaging.png)

hello alábbi ábrán látható hello dinamikus becsomagolás munkafolyamat.

![Dinamikus kódolás](./media/media-services-dynamic-packaging-overview/media-services-dynamic-packaging.png)


## <a name="common-scenario"></a>Egy gyakori alaphelyzete
1. Töltse fel a bemeneti fájl (néven mezzazine-fájlt). Például H.264, MP4 vagy WMV (hello támogatott formátumok listája: [Media Encoder Standard hello által támogatott formátumok](media-services-media-encoder-standard-formats.md).
2. Kódolja a mezzanine fájl tooH.264 MP4 adaptív sávszélességű beállítása.
3. Tegye közzé hello eszköz, amely tartalmazza a hello adaptív sávszélességű MP4 típusú beállításkészlettel hello igény kereső létrehozásával.
4. Build a streaming URL-címek tooaccess hello és a tartalmak.

## <a name="preparing-assets-for-dynamic-streaming"></a>Dinamikus streaming eszközök előkészítése
tooprepare dinamikus streaming meg az eszköz két lehetőség közül választhat:

1. [Töltse fel a fő fájlt](media-services-dotnet-upload-files.md).
2. [Hello Media Encoder Standard encoder tooproduce H.264 MP4 adaptív sávszélességű készletek használata](media-services-dotnet-encode-with-media-encoder-standard.md).
3. [A tartalmak](media-services-deliver-content-overview.md).

## <a id="unsupported_formats"></a>Dinamikus becsomagolás által nem támogatott
hello következő forrás formátumok nem támogatja a dinamikus csomagolás.

* Dolby digitális mp4-fájlokat.
* Dolby digitális zökkenőmentes fájlokat.

## <a name="media-services-learning-paths"></a>Media Services képzési tervek
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

