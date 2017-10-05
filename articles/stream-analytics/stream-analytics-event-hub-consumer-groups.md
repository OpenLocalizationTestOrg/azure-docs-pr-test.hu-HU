---
title: "Az event hubs érzékelőinek Azure Stream Analytics Debug |} Microsoft Docs"
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
ms.openlocfilehash: 145981d0b5eff0c574c5012c85f43a6318ba4126
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="debug-azure-stream-analytics-with-event-hub-receivers"></a><span data-ttu-id="fc818-104">Azure Stream Analytics a hibakereséshez event hubs érzékelőinek száma</span><span class="sxs-lookup"><span data-stu-id="fc818-104">Debug Azure Stream Analytics with event hub receivers</span></span>

<span data-ttu-id="fc818-105">Használhatja az Azure Event Hubs az Azure Stream Analytics betöltési vagy kimeneti egy feladat adatait.</span><span class="sxs-lookup"><span data-stu-id="fc818-105">You can use Azure Event Hubs in Azure Stream Analytics to ingest or output data from a job.</span></span> <span data-ttu-id="fc818-106">Az Event Hubs használata esetén ajánlott eljárás, hogy több felhasználói csoport használja annak feladat méretezhetőség érdekében.</span><span class="sxs-lookup"><span data-stu-id="fc818-106">A best practice for using Event Hubs is to use multiple consumer groups, to ensure job scalability.</span></span> <span data-ttu-id="fc818-107">Egyik ok: az, hogy az adott bevitel Stream Analytics-feladat az olvasók számát hatással van az olvasók egyetlen felhasználói csoportokban.</span><span class="sxs-lookup"><span data-stu-id="fc818-107">One reason is that the number of readers in the Stream Analytics job for a specific input affects the number of readers in a single consumer group.</span></span> <span data-ttu-id="fc818-108">A fogadók pontos száma belső megvalósítás részletei kibővített topológia logika alapul.</span><span class="sxs-lookup"><span data-stu-id="fc818-108">The precise number of receivers is based on internal implementation details for the scale-out topology logic.</span></span> <span data-ttu-id="fc818-109">A fogadók száma kívülről nem lesz közzétéve.</span><span class="sxs-lookup"><span data-stu-id="fc818-109">The number of receivers is not exposed externally.</span></span> <span data-ttu-id="fc818-110">Az olvasók számát módosíthatja a feladat kezdési időpontban vagy feladat frissítéskor.</span><span class="sxs-lookup"><span data-stu-id="fc818-110">The number of readers can change either at the job start time or during job upgrades.</span></span>

> [!NOTE]
> <span data-ttu-id="fc818-111">Ha az olvasók számát egy feladat a frissítés során, átmeneti kapcsolatos figyelmeztetések írása a naplók.</span><span class="sxs-lookup"><span data-stu-id="fc818-111">When the number of readers changes during a job upgrade, transient warnings are written to audit logs.</span></span> <span data-ttu-id="fc818-112">Stream Analytics-feladatok automatikusan helyreállni ezek átmeneti probléma.</span><span class="sxs-lookup"><span data-stu-id="fc818-112">Stream Analytics jobs automatically recover from these transient issues.</span></span>

## <a name="number-of-readers-per-partition-exceeds-event-hubs-limit-of-five"></a><span data-ttu-id="fc818-113">Partíciónként Olvasók száma meghaladja az Event Hubs legfeljebb öt</span><span class="sxs-lookup"><span data-stu-id="fc818-113">Number of readers per partition exceeds Event Hubs limit of five</span></span>

<span data-ttu-id="fc818-114">Forgatókönyvek, amelyben az olvasók partíciónként száma meghaladja az Event Hubs legfeljebb öt közé tartoznak a következők:</span><span class="sxs-lookup"><span data-stu-id="fc818-114">Scenarios in which the number of readers per partition exceeds the Event Hubs limit of five include the following:</span></span>

* <span data-ttu-id="fc818-115">Több KIVÁLASZTÓ utasítást: Ha több KIVÁLASZTÓ utasítást hivatkozó **azonos** eseményközpont bemeneti, minden KIVÁLASZTÓ utasításhoz hatására létrejön egy új fogadó.</span><span class="sxs-lookup"><span data-stu-id="fc818-115">Multiple SELECT statements: If you use multiple SELECT statements that refer to **same** event hub input, each SELECT statement causes a new receiver to be created.</span></span>
* <span data-ttu-id="fc818-116">Unió: UNION használatához esetén előfordulhat, hogy több bemenet, amely hivatkozik a **azonos** event hub és fogyasztói csoportot.</span><span class="sxs-lookup"><span data-stu-id="fc818-116">UNION: When you use a UNION, it's possible to have multiple inputs that refer to the **same** event hub and consumer group.</span></span>
* <span data-ttu-id="fc818-117">ÖNILLESZTÉS: Automatikus csatlakozás művelet használatakor is lehet hivatkozni a **azonos** eseményközpont több alkalommal.</span><span class="sxs-lookup"><span data-stu-id="fc818-117">SELF JOIN: When you use a SELF JOIN operation, it's possible to refer to the **same** event hub multiple times.</span></span>

## <a name="solution"></a><span data-ttu-id="fc818-118">Megoldás</span><span class="sxs-lookup"><span data-stu-id="fc818-118">Solution</span></span>

<span data-ttu-id="fc818-119">A következő gyakorlati tanácsok segíthet csökkenteni az forgatókönyvek, amelyben az olvasók partíciónként száma meghaladja az Event Hubs legfeljebb öt.</span><span class="sxs-lookup"><span data-stu-id="fc818-119">The following best practices can help mitigate scenarios in which the number of readers per partition exceeds the Event Hubs limit of five.</span></span>

### <a name="split-your-query-into-multiple-steps-by-using-a-with-clause"></a><span data-ttu-id="fc818-120">A lekérdezés felosztása több lépést a WITH záradék használatával</span><span class="sxs-lookup"><span data-stu-id="fc818-120">Split your query into multiple steps by using a WITH clause</span></span>

<span data-ttu-id="fc818-121">A WITH záradékkal ideiglenes elnevezett eredménykészletet, amely a FROM záradék a lekérdezés hivatkozhat határozza meg.</span><span class="sxs-lookup"><span data-stu-id="fc818-121">The WITH clause specifies a temporary named result set that can be referenced by a FROM clause in the query.</span></span> <span data-ttu-id="fc818-122">A WITH záradékkal egyetlen SELECT utasítás a végrehajtás hatókörében határozza meg.</span><span class="sxs-lookup"><span data-stu-id="fc818-122">You define the WITH clause in the execution scope of a single SELECT statement.</span></span>

<span data-ttu-id="fc818-123">Például helyett ezt a lekérdezést:</span><span class="sxs-lookup"><span data-stu-id="fc818-123">For example, instead of this query:</span></span>

```
SELECT foo 
INTO output1
FROM inputEventHub

SELECT bar
INTO output2
FROM inputEventHub 
…
```

<span data-ttu-id="fc818-124">Használja ezt a lekérdezést:</span><span class="sxs-lookup"><span data-stu-id="fc818-124">Use this query:</span></span>

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

### <a name="ensure-that-inputs-bind-to-different-consumer-groups"></a><span data-ttu-id="fc818-125">Győződjön meg arról, hogy bemeneti kötése különböző felhasználói csoportokhoz</span><span class="sxs-lookup"><span data-stu-id="fc818-125">Ensure that inputs bind to different consumer groups</span></span>

<span data-ttu-id="fc818-126">A lekérdezések, amelyben legalább három bemeneti adatokat a ugyanazt az Event Hubs fogyasztói csoportot csatlakozik külön felhasználói csoportok létrehozása.</span><span class="sxs-lookup"><span data-stu-id="fc818-126">For queries in which three or more inputs are connected to the same Event Hubs consumer group, create separate consumer groups.</span></span> <span data-ttu-id="fc818-127">Ehhez szükséges további Stream Analytics bemenetek létrehozását.</span><span class="sxs-lookup"><span data-stu-id="fc818-127">This requires the creation of additional Stream Analytics inputs.</span></span>


## <a name="get-help"></a><span data-ttu-id="fc818-128">Segítségkérés</span><span class="sxs-lookup"><span data-stu-id="fc818-128">Get help</span></span>
<span data-ttu-id="fc818-129">Ha további segítségre van szüksége, próbálkozzon a [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="fc818-129">For additional assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>

## <a name="next-steps"></a><span data-ttu-id="fc818-130">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fc818-130">Next steps</span></span>
* [<span data-ttu-id="fc818-131">A Stream Analytics bemutatása</span><span class="sxs-lookup"><span data-stu-id="fc818-131">Introduction to Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="fc818-132">A Stream Analytics használatába</span><span class="sxs-lookup"><span data-stu-id="fc818-132">Get started with Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="fc818-133">Stream Analytics-feladatok méretezése</span><span class="sxs-lookup"><span data-stu-id="fc818-133">Scale Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="fc818-134">Stream Analytics lekérdezési nyelvi referencia</span><span class="sxs-lookup"><span data-stu-id="fc818-134">Stream Analytics query language reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="fc818-135">A Stream Analytics felügyeleti REST API-referencia</span><span class="sxs-lookup"><span data-stu-id="fc818-135">Stream Analytics management REST API reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)
