---
title: "Sebesség és az Azure Media Services kódolási párhuzamossági kezelése |} Microsoft Docs"
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
ms.openlocfilehash: 0463904fd9bf1138587d0d214e572ddd38cc2184
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
#  <a name="manage-speed-and-concurrency-of-your-encoding"></a><span data-ttu-id="b79da-103">Sebesség és a kódolási párhuzamossági kezelése</span><span class="sxs-lookup"><span data-stu-id="b79da-103">Manage speed and concurrency of your encoding</span></span>

<span data-ttu-id="b79da-104">Ez a cikk hogyan kezelheti a sebesség és a egyidejűségi beállítása pedig a kódolási feladatok feladatok rövid áttekintést nyújt.</span><span class="sxs-lookup"><span data-stu-id="b79da-104">This article gives a brief overview of how you can manage speed and concurrency of your encoding jobs/tasks.</span></span>

## <a name="overview"></a><span data-ttu-id="b79da-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="b79da-105">Overview</span></span>

<span data-ttu-id="b79da-106">A Media Services szolgáltatásban a **fenntartott egységtípus** határozza meg a sebesség, amellyel a feladatok feldolgozása media feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="b79da-106">In Media Services, a **Reserved Unit Type** determines the speed with which your media processing tasks are processed.</span></span> <span data-ttu-id="b79da-107">A következő Fenntartott egység típusok közül választhat: **S1**, **S2** vagy **S3**.</span><span class="sxs-lookup"><span data-stu-id="b79da-107">You can pick between the following reserved unit types: **S1**, **S2**, or **S3**.</span></span> <span data-ttu-id="b79da-108">Ugyanaz a kódolási feladat például gyorsabban fut, amikor az **S2** Fenntartott egység típust használja az **S1** típus helyett.</span><span class="sxs-lookup"><span data-stu-id="b79da-108">For example, the same encoding job runs faster when you use the **S2** reserved unit type compare to the **S1** type.</span></span> <span data-ttu-id="b79da-109">A [kódolási egységek méretezése](media-services-scale-media-processing-overview.md) témakör, amely segít a döntést között különböző kódolási sebességű kiválasztásakor táblázatát jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="b79da-109">The [scaling encoding units](media-services-scale-media-processing-overview.md) topic shows a table that helps you make decision when choosing between different encoding speeds.</span></span>

<span data-ttu-id="b79da-110">Kívül fenntartott egységnek típusának megadásával, a fiók kiépítése megadhat **fenntartott egységek**.</span><span class="sxs-lookup"><span data-stu-id="b79da-110">In addition to specifying the reserved unit type, you can specify to provision your account with **Reserved Units**.</span></span> <span data-ttu-id="b79da-111">A megadott Fenntartott egységek száma határozza meg az egy adott fiókon egy időben feldolgozható médiafeladatok számát.</span><span class="sxs-lookup"><span data-stu-id="b79da-111">The number of provisioned reserved units determines the number of media tasks that can be processed concurrently in a given account.</span></span> <span data-ttu-id="b79da-112">Például ha a fiók nem rendelkezik öt fenntartott egységek, majd öt media feladatokat futtatni egyidejűleg hosszú, amennyi a feldolgozandó feladatok.</span><span class="sxs-lookup"><span data-stu-id="b79da-112">For example, if your account has five reserved units, then five media tasks will be running concurrently as long as there are tasks to be processed.</span></span> <span data-ttu-id="b79da-113">A hátralévő műveletekkel a várólistában, és egymás után feldolgozásához, amikor egy futó feladat befejezése után fog beolvasása felvételre.</span><span class="sxs-lookup"><span data-stu-id="b79da-113">The remaining tasks will wait in the queue and will get picked up for processing sequentially when a running task finishes.</span></span> <span data-ttu-id="b79da-114">Ha a fiók nem rendelkezik bármely fenntartott egységek kiosztása, majd feladatok fog felvételre egymás után.</span><span class="sxs-lookup"><span data-stu-id="b79da-114">If an account does not have any reserved units provisioned, then tasks will be picked up sequentially.</span></span> <span data-ttu-id="b79da-115">Ebben az esetben egy feladat befejeződik, és ezután egy kezdő között a várakozási idő függ a rendelkezésre álló erőforrások a rendszerben.</span><span class="sxs-lookup"><span data-stu-id="b79da-115">In this case, the wait time between one task finishing and the next one starting will depend on the availability of resources in the system.</span></span>

<span data-ttu-id="b79da-116">Részletes információkat és kódolási egységek méretezése példák: [ez](media-services-scale-media-processing-overview.md) témakör.</span><span class="sxs-lookup"><span data-stu-id="b79da-116">For detailed information and examples that show how to scale encoding units, see [this](media-services-scale-media-processing-overview.md) topic.</span></span>

## <a name="next-step"></a><span data-ttu-id="b79da-117">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="b79da-117">Next step</span></span>

[<span data-ttu-id="b79da-118">Kódolási méretezési egységek</span><span class="sxs-lookup"><span data-stu-id="b79da-118">Scale encoding units</span></span>](media-services-scale-media-processing-overview.md)

## <a name="media-services-learning-paths"></a><span data-ttu-id="b79da-119">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="b79da-119">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="b79da-120">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="b79da-120">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

