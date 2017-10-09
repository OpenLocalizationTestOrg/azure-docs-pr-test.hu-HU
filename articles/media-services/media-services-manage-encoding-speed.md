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
#  <a name="manage-speed-and-concurrency-of-your-encoding"></a><span data-ttu-id="71095-103">Sebesség és a kódolási párhuzamossági kezelése</span><span class="sxs-lookup"><span data-stu-id="71095-103">Manage speed and concurrency of your encoding</span></span>

<span data-ttu-id="71095-104">Ez a cikk hogyan kezelheti a sebesség és a egyidejűségi beállítása pedig a kódolási feladatok feladatok rövid áttekintést nyújt.</span><span class="sxs-lookup"><span data-stu-id="71095-104">This article gives a brief overview of how you can manage speed and concurrency of your encoding jobs/tasks.</span></span>

## <a name="overview"></a><span data-ttu-id="71095-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="71095-105">Overview</span></span>

<span data-ttu-id="71095-106">A Media Services szolgáltatásban a **fenntartott egységtípus** hello sebesség, amellyel a feladatok feldolgozása media feldolgozásának határozza meg.</span><span class="sxs-lookup"><span data-stu-id="71095-106">In Media Services, a **Reserved Unit Type** determines hello speed with which your media processing tasks are processed.</span></span> <span data-ttu-id="71095-107">Hello következő közötti választhatja ki a szolgáltatás számára fenntartott egység: **S1**, **S2**, vagy **S3**.</span><span class="sxs-lookup"><span data-stu-id="71095-107">You can pick between hello following reserved unit types: **S1**, **S2**, or **S3**.</span></span> <span data-ttu-id="71095-108">Például ugyanazon kódolási feladat fut gyorsabban hello használatakor hello **S2** fenntartott egységnek típus összehasonlítása toohello **S1** típusa.</span><span class="sxs-lookup"><span data-stu-id="71095-108">For example, hello same encoding job runs faster when you use hello **S2** reserved unit type compare toohello **S1** type.</span></span> <span data-ttu-id="71095-109">Hello [kódolási egységek méretezése](media-services-scale-media-processing-overview.md) témakör, amely segít a döntést között különböző kódolási sebességű kiválasztásakor táblázatát jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="71095-109">hello [scaling encoding units](media-services-scale-media-processing-overview.md) topic shows a table that helps you make decision when choosing between different encoding speeds.</span></span>

<span data-ttu-id="71095-110">Ezenkívül toospecifying hello fenntartott egység típusát, akkor megadhatja tooprovision fiókját **fenntartott egységek**.</span><span class="sxs-lookup"><span data-stu-id="71095-110">In addition toospecifying hello reserved unit type, you can specify tooprovision your account with **Reserved Units**.</span></span> <span data-ttu-id="71095-111">kiépített fenntartott egységek számának hello hello egyidejűleg dolgozhatók fel egy adott fiókhoz media feladatok száma határozza meg.</span><span class="sxs-lookup"><span data-stu-id="71095-111">hello number of provisioned reserved units determines hello number of media tasks that can be processed concurrently in a given account.</span></span> <span data-ttu-id="71095-112">Például ha a fiók nem rendelkezik öt fenntartott egységek, majd öt media feladatokat futtatni egyidejűleg hosszú, amennyi a feladatok toobe feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="71095-112">For example, if your account has five reserved units, then five media tasks will be running concurrently as long as there are tasks toobe processed.</span></span> <span data-ttu-id="71095-113">hello feladataiban hello várólistában és egymás után feldolgozásához, amikor egy futó feladat befejezése után fog beolvasása felvételre.</span><span class="sxs-lookup"><span data-stu-id="71095-113">hello remaining tasks will wait in hello queue and will get picked up for processing sequentially when a running task finishes.</span></span> <span data-ttu-id="71095-114">Ha a fiók nem rendelkezik bármely fenntartott egységek kiosztása, majd feladatok fog felvételre egymás után.</span><span class="sxs-lookup"><span data-stu-id="71095-114">If an account does not have any reserved units provisioned, then tasks will be picked up sequentially.</span></span> <span data-ttu-id="71095-115">Ebben az esetben hello Várjon, amíg egy tevékenység befejezése között eltelt idő, és hello mellett egy kezdési függ erőforrások hello rendelkezésre állását hello rendszerben.</span><span class="sxs-lookup"><span data-stu-id="71095-115">In this case, hello wait time between one task finishing and hello next one starting will depend on hello availability of resources in hello system.</span></span>

<span data-ttu-id="71095-116">A részletes útmutatást és példákat, hogy hogyan tooscale kódolási egységek: megjelenítése [ez](media-services-scale-media-processing-overview.md) témakör.</span><span class="sxs-lookup"><span data-stu-id="71095-116">For detailed information and examples that show how tooscale encoding units, see [this](media-services-scale-media-processing-overview.md) topic.</span></span>

## <a name="next-step"></a><span data-ttu-id="71095-117">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="71095-117">Next step</span></span>

[<span data-ttu-id="71095-118">Kódolási méretezési egységek</span><span class="sxs-lookup"><span data-stu-id="71095-118">Scale encoding units</span></span>](media-services-scale-media-processing-overview.md)

## <a name="media-services-learning-paths"></a><span data-ttu-id="71095-119">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="71095-119">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="71095-120">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="71095-120">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

