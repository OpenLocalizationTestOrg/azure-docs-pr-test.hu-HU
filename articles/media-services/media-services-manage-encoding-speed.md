---
title: "AAA kezelése sebesség és az Azure Media Services kódolási párhuzamossági |} Microsoft Docs"
description: "Ez a cikk hogyan kezelheti a sebesség és a kódolási feladatok/feladatokat az Azure Media Services párhuzamossági rövid áttekintést nyújt."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 676313f8-a158-4e3a-a99b-2c29a341ecc9
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/10/2017
ms.author: juliako
ms.openlocfilehash: da52a6278a3d3b084dbf5a594db37df8447bb944
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
#  <a name="manage-speed-and-concurrency-of-your-encoding"></a>Sebesség és a kódolási párhuzamossági kezelése

Ez a cikk hogyan kezelheti a sebesség és a egyidejűségi beállítása pedig a kódolási feladatok feladatok rövid áttekintést nyújt.

## <a name="overview"></a>Áttekintés

A Media Services szolgáltatásban a **fenntartott egységtípus** hello sebesség, amellyel a feladatok feldolgozása media feldolgozásának határozza meg. Hello következő közötti választhatja ki a szolgáltatás számára fenntartott egység: **S1**, **S2**, vagy **S3**. Például ugyanazon kódolási feladat fut gyorsabban hello használatakor hello **S2** fenntartott egységnek típus összehasonlítása toohello **S1** típusa. Hello [kódolási egységek méretezése](media-services-scale-media-processing-overview.md) témakör, amely segít a döntést között különböző kódolási sebességű kiválasztásakor táblázatát jeleníti meg.

Ezenkívül toospecifying hello fenntartott egység típusát, akkor megadhatja tooprovision fiókját **fenntartott egységek**. kiépített fenntartott egységek számának hello hello egyidejűleg dolgozhatók fel egy adott fiókhoz media feladatok száma határozza meg. Például ha a fiók nem rendelkezik öt fenntartott egységek, majd öt media feladatokat futtatni egyidejűleg hosszú, amennyi a feladatok toobe feldolgozása. hello feladataiban hello várólistában és egymás után feldolgozásához, amikor egy futó feladat befejezése után fog beolvasása felvételre. Ha a fiók nem rendelkezik bármely fenntartott egységek kiosztása, majd feladatok fog felvételre egymás után. Ebben az esetben hello Várjon, amíg egy tevékenység befejezése között eltelt idő, és hello mellett egy kezdési függ erőforrások hello rendelkezésre állását hello rendszerben.

A részletes útmutatást és példákat, hogy hogyan tooscale kódolási egységek: megjelenítése [ez](media-services-scale-media-processing-overview.md) témakör.

## <a name="next-step"></a>Következő lépés

[Kódolási méretezési egységek](media-services-scale-media-processing-overview.md)

## <a name="media-services-learning-paths"></a>Media Services képzési tervek
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

