---
title: "az event hubs érzékelőinek Azure Stream Analytics aaaDebug |} Microsoft Docs"
description: "A lekérdezés ajánlott eljárások az Event Hubs fogyasztói csoportok a Stream Analytics-feladatok figyelembe véve."
keywords: "Event hub határt, a fogyasztói csoportot"
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/20/2017
ms.author: jeffstok
ms.openlocfilehash: 89821e6273151de43b5e42d907e547c939e24824
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="debug-azure-stream-analytics-with-event-hub-receivers"></a><span data-ttu-id="26010-104">Azure Stream Analytics a hibakereséshez event hubs érzékelőinek száma</span><span class="sxs-lookup"><span data-stu-id="26010-104">Debug Azure Stream Analytics with event hub receivers</span></span>

<span data-ttu-id="26010-105">Használhatja az Azure Event Hubs az Azure Stream Analytics tooingest vagy a kimenetet a feladatból.</span><span class="sxs-lookup"><span data-stu-id="26010-105">You can use Azure Event Hubs in Azure Stream Analytics tooingest or output data from a job.</span></span> <span data-ttu-id="26010-106">Az Event Hubs használatára vonatkozó ajánlott toouse több fogyasztói csoportok, tooensure feladat méretezhetőséget.</span><span class="sxs-lookup"><span data-stu-id="26010-106">A best practice for using Event Hubs is toouse multiple consumer groups, tooensure job scalability.</span></span> <span data-ttu-id="26010-107">Egyik oka, hogy hello Stream Analytics-feladat egy adott bevitel az olvasók számát hello hatással lesz hello olvasók egyetlen felhasználói csoportokban.</span><span class="sxs-lookup"><span data-stu-id="26010-107">One reason is that hello number of readers in hello Stream Analytics job for a specific input affects hello number of readers in a single consumer group.</span></span> <span data-ttu-id="26010-108">hello pontos száma fogadók hello kibővített topológia logika belső részleteket alapul.</span><span class="sxs-lookup"><span data-stu-id="26010-108">hello precise number of receivers is based on internal implementation details for hello scale-out topology logic.</span></span> <span data-ttu-id="26010-109">hello száma fogadók kívülről nem lesz közzétéve.</span><span class="sxs-lookup"><span data-stu-id="26010-109">hello number of receivers is not exposed externally.</span></span> <span data-ttu-id="26010-110">hello olvasók számát hello feladat kezdési időpontban vagy feladat frissítéskor módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="26010-110">hello number of readers can change either at hello job start time or during job upgrades.</span></span>

> [!NOTE]
> <span data-ttu-id="26010-111">Hello olvasók számát egy feladat a frissítés során megváltozásakor átmeneti kapcsolatos figyelmeztetések írása tooaudit naplókat.</span><span class="sxs-lookup"><span data-stu-id="26010-111">When hello number of readers changes during a job upgrade, transient warnings are written tooaudit logs.</span></span> <span data-ttu-id="26010-112">Stream Analytics-feladatok automatikusan helyreállni ezek átmeneti probléma.</span><span class="sxs-lookup"><span data-stu-id="26010-112">Stream Analytics jobs automatically recover from these transient issues.</span></span>

## <a name="number-of-readers-per-partition-exceeds-event-hubs-limit-of-five"></a><span data-ttu-id="26010-113">Partíciónként Olvasók száma meghaladja az Event Hubs legfeljebb öt</span><span class="sxs-lookup"><span data-stu-id="26010-113">Number of readers per partition exceeds Event Hubs limit of five</span></span>

<span data-ttu-id="26010-114">Mely hello az olvasók partíciónként száma meghaladja a hello Event Hubs legfeljebb öt forgatókönyvek hello alábbiakat foglalja magába:</span><span class="sxs-lookup"><span data-stu-id="26010-114">Scenarios in which hello number of readers per partition exceeds hello Event Hubs limit of five include hello following:</span></span>

* <span data-ttu-id="26010-115">Több KIVÁLASZTÓ utasítást: Ha több KIVÁLASZTÓ utasítást túl hivatkozó**azonos** eseményközpont bemeneti, minden KIVÁLASZTÓ utasításhoz hatására a létrehozott új fogadó toobe.</span><span class="sxs-lookup"><span data-stu-id="26010-115">Multiple SELECT statements: If you use multiple SELECT statements that refer too**same** event hub input, each SELECT statement causes a new receiver toobe created.</span></span>
* <span data-ttu-id="26010-116">UNION: UNION használatához esetén lehetséges toohave toohello hivatkozó több bemeneti **azonos** event hub és fogyasztói csoportot.</span><span class="sxs-lookup"><span data-stu-id="26010-116">UNION: When you use a UNION, it's possible toohave multiple inputs that refer toohello **same** event hub and consumer group.</span></span>
* <span data-ttu-id="26010-117">ÖNILLESZTÉS: Automatikus csatlakozás művelet használatához esetén lehetséges toorefer toohello **azonos** eseményközpont több alkalommal.</span><span class="sxs-lookup"><span data-stu-id="26010-117">SELF JOIN: When you use a SELF JOIN operation, it's possible toorefer toohello **same** event hub multiple times.</span></span>

## <a name="solution"></a><span data-ttu-id="26010-118">Megoldás</span><span class="sxs-lookup"><span data-stu-id="26010-118">Solution</span></span>

<span data-ttu-id="26010-119">hello következő bevált gyakorlatát segíthet csökkenteni a mely hello partíciónként Olvasók száma meghaladja a hello Event Hubs legfeljebb öt forgatókönyvek.</span><span class="sxs-lookup"><span data-stu-id="26010-119">hello following best practices can help mitigate scenarios in which hello number of readers per partition exceeds hello Event Hubs limit of five.</span></span>

### <a name="split-your-query-into-multiple-steps-by-using-a-with-clause"></a><span data-ttu-id="26010-120">A lekérdezés felosztása több lépést a WITH záradék használatával</span><span class="sxs-lookup"><span data-stu-id="26010-120">Split your query into multiple steps by using a WITH clause</span></span>

<span data-ttu-id="26010-121">hello WITH záradékban ideiglenes elnevezett eredménykészletet, amely a FROM záradék a lekérdezés hello hivatkozhat.</span><span class="sxs-lookup"><span data-stu-id="26010-121">hello WITH clause specifies a temporary named result set that can be referenced by a FROM clause in hello query.</span></span> <span data-ttu-id="26010-122">Hello WITH záradékkal egy SELECT utasítás hello végrehajtási hatóköre határozza meg.</span><span class="sxs-lookup"><span data-stu-id="26010-122">You define hello WITH clause in hello execution scope of a single SELECT statement.</span></span>

<span data-ttu-id="26010-123">Például helyett ezt a lekérdezést:</span><span class="sxs-lookup"><span data-stu-id="26010-123">For example, instead of this query:</span></span>

```
SELECT foo 
INTO output1
FROM inputEventHub

SELECT bar
INTO output2
FROM inputEventHub 
…
```

<span data-ttu-id="26010-124">Használja ezt a lekérdezést:</span><span class="sxs-lookup"><span data-stu-id="26010-124">Use this query:</span></span>

```
WITH input (
   SELECT * FROM inputEventHub
) as data

SELECT foo
INTO output1
FROM data

SELECT bar
INTO output2
FROM data
…
```

### <a name="ensure-that-inputs-bind-toodifferent-consumer-groups"></a><span data-ttu-id="26010-125">Győződjön meg arról, hogy a bemeneti adatok kötési toodifferent fogyasztói csoportok</span><span class="sxs-lookup"><span data-stu-id="26010-125">Ensure that inputs bind toodifferent consumer groups</span></span>

<span data-ttu-id="26010-126">A lekérdezések, amelyekben három vagy több bemeneti adatok nem csatlakoztatott toohello ugyanazt az Event Hubs fogyasztói csoportjában külön felhasználói csoportok létrehozása.</span><span class="sxs-lookup"><span data-stu-id="26010-126">For queries in which three or more inputs are connected toohello same Event Hubs consumer group, create separate consumer groups.</span></span> <span data-ttu-id="26010-127">Ehhez szükséges további Stream Analytics bemenetek hello létrehozását.</span><span class="sxs-lookup"><span data-stu-id="26010-127">This requires hello creation of additional Stream Analytics inputs.</span></span>


## <a name="get-help"></a><span data-ttu-id="26010-128">Segítségkérés</span><span class="sxs-lookup"><span data-stu-id="26010-128">Get help</span></span>
<span data-ttu-id="26010-129">Ha további segítségre van szüksége, próbálkozzon a [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="26010-129">For additional assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>

## <a name="next-steps"></a><span data-ttu-id="26010-130">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="26010-130">Next steps</span></span>
* [<span data-ttu-id="26010-131">Bevezetés tooStream elemzés</span><span class="sxs-lookup"><span data-stu-id="26010-131">Introduction tooStream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="26010-132">A Stream Analytics használatába</span><span class="sxs-lookup"><span data-stu-id="26010-132">Get started with Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="26010-133">Stream Analytics-feladatok méretezése</span><span class="sxs-lookup"><span data-stu-id="26010-133">Scale Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="26010-134">Stream Analytics lekérdezési nyelvi referencia</span><span class="sxs-lookup"><span data-stu-id="26010-134">Stream Analytics query language reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="26010-135">A Stream Analytics felügyeleti REST API-referencia</span><span class="sxs-lookup"><span data-stu-id="26010-135">Stream Analytics management REST API reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)
