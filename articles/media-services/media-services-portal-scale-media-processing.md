---
title: "aaaScale adathordozó használatával feldolgozása hello Azure portálon |} Microsoft Docs"
description: "Ez az oktatóanyag végigvezeti hello méretezési adathordozó feldolgozása hello Azure-portál használatával."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: e500f733-68aa-450c-b212-cf717c0d15da
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/04/2017
ms.author: juliako
ms.openlocfilehash: 89240c6f7579b8795e7b47f2b1c398b1d5477e20
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="change-hello-reserved-unit-type"></a>Hello szolgáltatás számára fenntartott egység típusának módosítása
> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-encoding-units.md)
> * [Portál](media-services-portal-scale-media-processing.md)
> * [REST](https://docs.microsoft.com/rest/api/media/operations/encodingreservedunittype)
> * [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
> * [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
> 
> 

## <a name="overview"></a>Áttekintés

Egy Media Services-fiók fenntartott egység típusú, amely megadja, hogy hello sebesség, amellyel a feladatok feldolgozása media feldolgozása hozzá rendelve. Hello következő közötti választhatja ki a szolgáltatás számára fenntartott egység: **S1**, **S2**, vagy **S3**. Például ugyanazon kódolási feladat fut gyorsabban hello használatakor hello **S2** fenntartott egységnek típus összehasonlítása toohello **S1** típusa.

Ezenkívül toospecifying hello fenntartott egység típusát, akkor megadhatja tooprovision fiókját **fenntartott egységek** (RUs). kiépített RUs hello száma határozza meg, egyidejűleg dolgozhatók fel egy adott fiókhoz media feladatok hello száma.

>[!NOTE]
>A Fenntartott egységek az összes médiafeldolgozás párhuzamossá tételéért felelősek, beleértve az Azure Media Indexerrel végzett indexelési feladatokat is. De a kódolással ellentétben az indexelési feladatok feldolgozása nem lesz gyorsabb a gyorsabb Fenntartott egységekkel.

> [!IMPORTANT]
> Győződjön meg arról, hogy tooreview hello [áttekintése](media-services-scale-media-processing-overview.md) témakör tooget témakör feldolgozása media méretezésével kapcsolatos további információk.
> 
> 

## <a name="scale-media-processing"></a>Méretezhető médiafeldolgozás
toochange hello szolgáltatás számára fenntartott egység típusa és hello fenntartott egységek számát, a következő hello:

1. A hello [Azure-portálon](https://portal.azure.com/), válassza ki az Azure Media Services-fiók.
2. A hello **beállítások** ablakban válassza ki **Media szolgáltatás számára fenntartott egység**.
   
    hello fenntartott egységek száma toochange hello fenntartott egységnek típust választotta, használja a hello **Media kiszolgált egységek** csúszkát.
   
    toochange hello **FENNTARTOTT EGYSÉGTÍPUS**, nyomja le az S1, S2 vagy S3.
   
    ![Feldolgozók lap](./media/media-services-portal-scale-media-processing/media-services-scale-media-processing.png)
3. Nyomja le az hello gomb toosave menti a módosításokat.
   
    új fenntartott egységek hello MENTÉS megnyomásakor foglal le.

## <a name="next-steps"></a>Következő lépések
Tekintse át a Media Services képzési terveket.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

