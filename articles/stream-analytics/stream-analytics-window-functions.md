---
title: "Bevezetés a Stream Analytics ablak Funkciók |} Microsoft Docs"
description: "További tudnivalók a Stream Analytics (átfedésmentes, hopping, a késleltetett) ablakban funkcióit."
keywords: "átfedésmentes ablak késleltetett ablakban hopping ablak"
documentationcenter: 
services: stream-analytics
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 0d8d8717-5d23-43f0-b475-af078ab4627d
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: 09915ad747153210f655b152355c551046066a88
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="introduction-to-stream-analytics-window-functions"></a><span data-ttu-id="4470a-104">Bevezetés a Stream Analytics ablak Funkciók</span><span class="sxs-lookup"><span data-stu-id="4470a-104">Introduction to Stream Analytics Window functions</span></span>
<span data-ttu-id="4470a-105">Sok valós idejű adatfolyam-forgatókönyvek akkor kell végrehajtani a műveleteket csak időbeli windows szereplő adatokat.</span><span class="sxs-lookup"><span data-stu-id="4470a-105">In many real time streaming scenarios, it is necessary to perform operations only on the data contained in temporal windows.</span></span> <span data-ttu-id="4470a-106">Ablakozó függvény natív támogatása az Azure Stream Analytics, amely helyezi át a tű fejlesztést tesz lehetővé az összetett adatfolyam jelentésfeldolgozó feladatok szerzői alapfunkciója.</span><span class="sxs-lookup"><span data-stu-id="4470a-106">Native support for windowing functions is a key feature of Azure Stream Analytics that moves the needle on developer productivity in authoring complex stream processing jobs.</span></span> <span data-ttu-id="4470a-107">A Stream Analytics lehetővé teszi a fejlesztők használandó [ **Átfedésmentes**](https://msdn.microsoft.com/library/dn835055.aspx), [ **Hopping** ](https://msdn.microsoft.com/library/dn835041.aspx) és [ **csúszó** ](https://msdn.microsoft.com/library/dn835051.aspx) windows a streamelési adatok historikus műveletek végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="4470a-107">Stream Analytics enables developers to use [**Tumbling**](https://msdn.microsoft.com/library/dn835055.aspx), [**Hopping**](https://msdn.microsoft.com/library/dn835041.aspx) and [**Sliding**](https://msdn.microsoft.com/library/dn835051.aspx) windows to perform temporal operations on streaming data.</span></span> <span data-ttu-id="4470a-108">Érdemes megjegyezni, hogy minden [ablak](https://msdn.microsoft.com/library/dn835019.aspx) műveletek kimeneti eredményt a **end** ablak.</span><span class="sxs-lookup"><span data-stu-id="4470a-108">It is worth noting that all [Window](https://msdn.microsoft.com/library/dn835019.aspx) operations output results at the **end** of the window.</span></span> <span data-ttu-id="4470a-109">A kimenet az ablak lesz egyszeri esemény alapján az összesítő függvényt használja.</span><span class="sxs-lookup"><span data-stu-id="4470a-109">The output of the window will be single event based on the aggregate function used.</span></span> <span data-ttu-id="4470a-110">Az esemény lesz az időszak végének időbélyegzőjét, és az összes ablak függvényeket rögzített hosszúságú.</span><span class="sxs-lookup"><span data-stu-id="4470a-110">The event will have the time stamp of the end of the window and all Window functions are defined with a fixed length.</span></span> <span data-ttu-id="4470a-111">Végül fontos megjegyezni, hogy az összes ablak Funkciók használatos a [ **GROUP BY** ](https://msdn.microsoft.com/library/dn835023.aspx) záradékban.</span><span class="sxs-lookup"><span data-stu-id="4470a-111">Lastly it is important to note that all Window functions should be used in a [**GROUP BY**](https://msdn.microsoft.com/library/dn835023.aspx) clause.</span></span>

![Stream Analytics ablak Funkciók fogalmak](media/stream-analytics-window-functions/stream-analytics-window-functions-conceptual.png)

## <a name="tumbling-window"></a><span data-ttu-id="4470a-113">Átfedésmentes ablak</span><span class="sxs-lookup"><span data-stu-id="4470a-113">Tumbling Window</span></span>
<span data-ttu-id="4470a-114">Átfedésmentes ablak Funkciók adatfolyam szegmentálni történő különböző idő és a művelet végrehajtása előzetesen, például az alábbi példában használt.</span><span class="sxs-lookup"><span data-stu-id="4470a-114">Tumbling window functions are used to segment a data stream into distinct time segments and perform a function against them, such as the example below.</span></span> <span data-ttu-id="4470a-115">A kulcs differentiators Átfedésmentes ablak, hogy azok ismételje meg a, nem lehetnek átfedésben és az esemény nem tartozhat egynél több átfedésmentes ablak.</span><span class="sxs-lookup"><span data-stu-id="4470a-115">The key differentiators of a Tumbling window are that they repeat, do not overlap and an event cannot belong to more than one tumbling window.</span></span>

![Stream Analytics ablak Funkciók átfedésmentes – bevezetés](media/stream-analytics-window-functions/stream-analytics-window-functions-tumbling-intro.png)

## <a name="hopping-window"></a><span data-ttu-id="4470a-117">Ugróablak</span><span class="sxs-lookup"><span data-stu-id="4470a-117">Hopping Window</span></span>
<span data-ttu-id="4470a-118">Szórási ablak Funkciók Ugrás előre meghatározott időtartamon által.</span><span class="sxs-lookup"><span data-stu-id="4470a-118">Hopping window functions hop forward in time by a fixed period.</span></span> <span data-ttu-id="4470a-119">Elképzelhető, hogy könnyen gondolunk, hogy Átfedésmentes windows egymást átfedő is, mint, egynél több Hopping ablak eredménykészlet események is tartozhatnak.</span><span class="sxs-lookup"><span data-stu-id="4470a-119">It may be easy to think of them as Tumbling windows that can overlap, so events can belong to more than one Hopping window result set.</span></span> <span data-ttu-id="4470a-120">Ugyanaz, mint egy Átfedésmentes ablak Hopping annak az egy ablak egyszerűen kellene megadnia a ugrási méretének ablak méretének azonosnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="4470a-120">To make a Hopping window the same as a Tumbling window one would simply specify the hop size to be the same as the window size.</span></span> 

![Stream Analytics ablak működik szórási – bevezetés](media/stream-analytics-window-functions/stream-analytics-window-functions-hopping-intro.png)

## <a name="sliding-window"></a><span data-ttu-id="4470a-122">Csúszóablak</span><span class="sxs-lookup"><span data-stu-id="4470a-122">Sliding Window</span></span>
<span data-ttu-id="4470a-123">Mozgó ablak Funkciók Átfedésmentes és a windows, Hopping előállít egy kimeneti **csak** egy esemény bekövetkezésekor.</span><span class="sxs-lookup"><span data-stu-id="4470a-123">Sliding window functions, unlike Tumbling or Hopping windows, produce an output **only**  when an event occurs.</span></span> <span data-ttu-id="4470a-124">Minden ablak lesz legalább egy eseményt, és az ablak folyamatosan mozgatja előre egy "(epszilon).</span><span class="sxs-lookup"><span data-stu-id="4470a-124">Every window will have at least one event and the window continuously moves forward by an € (epsilon).</span></span> <span data-ttu-id="4470a-125">Például a Hopping Windows események több késleltetett ablak is tartozhatnak.</span><span class="sxs-lookup"><span data-stu-id="4470a-125">Like Hopping Windows, events can belong to more than one Sliding Window.</span></span>

![Bevezetés a késleltetett Stream Analytics ablak Funkciók](media/stream-analytics-window-functions/stream-analytics-window-functions-sliding-intro.png)

## <a name="getting-help-with-window-functions"></a><span data-ttu-id="4470a-127">Az ablak functions kapcsolatos segítség kérése</span><span class="sxs-lookup"><span data-stu-id="4470a-127">Getting help with Window functions</span></span>
<span data-ttu-id="4470a-128">További támogatásért keresse fel az [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span><span class="sxs-lookup"><span data-stu-id="4470a-128">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span></span>

## <a name="next-steps"></a><span data-ttu-id="4470a-129">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4470a-129">Next steps</span></span>
* [<span data-ttu-id="4470a-130">Az Azure Stream Analytics bemutatása</span><span class="sxs-lookup"><span data-stu-id="4470a-130">Introduction to Azure Stream Analytics</span></span>](stream-analytics-introduction.md)
* <span data-ttu-id="4470a-131">[Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md) (Bevezetés az Azure Stream Analytics használatába)</span><span class="sxs-lookup"><span data-stu-id="4470a-131">[Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)</span></span>
* <span data-ttu-id="4470a-132">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)</span><span class="sxs-lookup"><span data-stu-id="4470a-132">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md)</span></span>
* <span data-ttu-id="4470a-133">[Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referencia az Azure Stream Analytics lekérdezési nyelvhez)</span><span class="sxs-lookup"><span data-stu-id="4470a-133">[Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)</span></span>
* [<span data-ttu-id="4470a-134">Az Azure Stream Analytics felügyeleti REST API referenciája</span><span class="sxs-lookup"><span data-stu-id="4470a-134">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

