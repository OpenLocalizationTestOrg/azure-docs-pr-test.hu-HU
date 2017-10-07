---
title: "aaaCreate speciális kódolás munkafolyamatok a munkafolyamat-tervezővel |} Microsoft Docs"
description: "További információk a hogyan toocreate speciális kódolási munkafolyamatok a munkafolyamat-tervezővel."
services: media-services
documentationcenter: 
author: anilmur
manager: cfowler
editor: 
ms.assetid: 004815f2-0761-4706-87a1-675ba36e0322
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: juliako;johndeu;anilmur
ms.openlocfilehash: 3744cde54c78bec7c7b586962ec1a8fe9529c1d2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-advanced-encoding-workflows-with-workflow-designer"></a>Speciális kódolási munkafolyamatok létrehozása a munkafolyamat-tervezővel
## <a name="overview"></a>Áttekintés
Hello **munkafolyamat-Tervező** a Windows asztali eszköz, amely használt toodesign és -buildek egyéni munkafolyamatokat kódolás **Media Encoder prémium munkafolyamat**.
Hello power hello munkafolyamat-Tervező eszköz használatával megtervezésének és létrehozásának fog futni a bonyolult munkafolyamatok **Media Encoder prémium**.  

Munkafolyamatok tartalmazhatnak ügyfél döntési programot, és ugorhat hello bemeneti forrás fájlok tulajdonságai alapján. Munkafolyamatok létrehozása felülbírálható tulajdonságok és dinamikus értékek toomake még akkor is, hello legösszetettebb kódolási feladatok könnyen toorepeat, és hello felhőben testreszabása.

Példa munkafolyamatokat hozhat létre a következők:

* Döntési alapú munkafolyamatok, amelyek vizsgálja meg a hello tartalmat a feloldásához, csak a szükséges hello kimeneti nyomon követi kódolása.  Ez az helfpul kihasználatlan hello nyomon követi, amelyek előállítják által hello forrás tartalom elírás upscaling kiküszöbölése révén.
* Több bemeneti fájl lehet használt toosupport feliratok, átfedések és való együttes tartalom. 

Ez az eszköz akkor is használt toomodify bármelyikét a [munkafolyamatok közzétett](media-services-workflow-designer.md#existing_workflows). 

> [!NOTE]
> tooget hello munkafolyamat-Tervező eszköz, a példány forduljon mepd@microsoft.com.
> 
> 

Egy munkafolyamat-fájl jön létre, ha eszközként fel kell tölteni, és ezután használható a médiafájlokat kódolási. Kapcsolatos információk a tooencode **Media Encoder prémium munkafolyamat** használatával **.NET**, lásd: [Media Encoder prémium munkafolyamat kódolás speciális](media-services-encode-with-premium-workflow.md).

## <a id="existing_workflows"></a>Módosítsa a meglévő munkafolyamatokat
alapértelmezett hello [munkafolyamatok közzétett](media-services-workflow-designer.md#existing_workflows) hello Tervező eszköz használatával módosítható. Kaphat hello alapértelmezett munkafolyamat-fájlok [Itt](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows). hello mappa ezeket a fájlokat hello leírását is tartalmazza.

a következő videók hello bemutatják, hogyan toouse hello Tervező.

### <a name="day-1--getting-started"></a>Naponta 1 – első lépések
Naponta 1 videó ismerteti:

* Tervező áttekintése
* Alapvető munkafolyamatok – a "Hello World"
* Több létrehozása kimeneti való használathoz az Azure Media Services streaming MP4-fájlok

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-Premium-Encoder-Workflow-Designer-Training-Videos-Day-1/player]
> 
> 

### <a name="day-2"></a>2 nap
Naponta 2 videó ismerteti:

* Forrás fájlelérési helyzetben változó – hang kezelése
* Speciális logikával munkafolyamatok
* Graph szakaszból

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-Premium-Encoder-Workflow-Designer-Training-Videos-Day-2/player]
> 
> 

### <a name="day-3"></a>Nap 3
Nap 3 videó ismerteti:

* Parancsfájl-kezelési munkafolyamatok/tervrajzokat belül
* A korlátozások hello aktuális kódoló
* A KÉRDÉSEK ÉS VÁLASZOK

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-Premium-Encoder-Workflow-Designer-Training-Videos-Day-3/player]
> 
> 

## <a name="next-step"></a>Következő lépés
Tekintse át a Media Services képzési terveket.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

Ha támogatja az kell, vagy egyéni munkafolyamatokat hello munkafolyamat-tervező eszközben létrehozásával kapcsolatos kérdése van, kérjük, küldjön e-mailek toomepd@microsoft.com.

## <a name="see-also"></a>Lásd még:
[Prémium szintű Azure kódoló munkafolyamat Tervező képzési videók](http://johndeutscher.com/2015/07/06/azure-premium-encoder-workflow-designer-training-videos/)

