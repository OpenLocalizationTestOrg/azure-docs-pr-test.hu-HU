---
title: "aaaIntroduction tooStream Analytics ablak Funkciók |} Microsoft Docs"
description: "Ismerje meg, függvények hello három ablakban a Stream Analytics (átfedésmentes, hopping, a késleltetett)."
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
ms.openlocfilehash: 7fc36eb9afb2b28e2791d925d26923145eb38074
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toostream-analytics-window-functions"></a><span data-ttu-id="77ed6-104">Bevezetés tooStream Analytics ablak Funkciók</span><span class="sxs-lookup"><span data-stu-id="77ed6-104">Introduction tooStream Analytics Window functions</span></span>
<span data-ttu-id="77ed6-105">Az adatfolyam-forgatókönyvek sok valós idejű szükséges tooperform műveleteket csak időbeli windows hello adatait is.</span><span class="sxs-lookup"><span data-stu-id="77ed6-105">In many real time streaming scenarios, it is necessary tooperform operations only on hello data contained in temporal windows.</span></span> <span data-ttu-id="77ed6-106">Ablakozó függvény natív támogatása, amely helyezi át hello tű fejlesztést tesz lehetővé az összetett adatfolyam jelentésfeldolgozó feladatok szerzői Azure Stream Analytics alapfunkciója.</span><span class="sxs-lookup"><span data-stu-id="77ed6-106">Native support for windowing functions is a key feature of Azure Stream Analytics that moves hello needle on developer productivity in authoring complex stream processing jobs.</span></span> <span data-ttu-id="77ed6-107">A Stream Analytics lehetővé teszi, hogy a fejlesztők toouse [ **Átfedésmentes**](https://msdn.microsoft.com/library/dn835055.aspx), [ **Hopping** ](https://msdn.microsoft.com/library/dn835041.aspx) és [ **csúszó** ](https://msdn.microsoft.com/library/dn835051.aspx) windows tooperform historikus műveleteket a streamelési adatok.</span><span class="sxs-lookup"><span data-stu-id="77ed6-107">Stream Analytics enables developers toouse [**Tumbling**](https://msdn.microsoft.com/library/dn835055.aspx), [**Hopping**](https://msdn.microsoft.com/library/dn835041.aspx) and [**Sliding**](https://msdn.microsoft.com/library/dn835051.aspx) windows tooperform temporal operations on streaming data.</span></span> <span data-ttu-id="77ed6-108">Érdemes megjegyezni, hogy minden [ablak](https://msdn.microsoft.com/library/dn835019.aspx) műveletek kimeneti hello eredményeket **end** hello ablak.</span><span class="sxs-lookup"><span data-stu-id="77ed6-108">It is worth noting that all [Window](https://msdn.microsoft.com/library/dn835019.aspx) operations output results at hello **end** of hello window.</span></span> <span data-ttu-id="77ed6-109">hello kimenet hello ablak lesz egyszeri esemény alapján hello aggregátumfüggvényt használja.</span><span class="sxs-lookup"><span data-stu-id="77ed6-109">hello output of hello window will be single event based on hello aggregate function used.</span></span> <span data-ttu-id="77ed6-110">hello esemény hello időbélyegzőjét hello ablak hello végéhez lesz, és az összes ablak függvényeket rögzített hosszúságú.</span><span class="sxs-lookup"><span data-stu-id="77ed6-110">hello event will have hello time stamp of hello end of hello window and all Window functions are defined with a fixed length.</span></span> <span data-ttu-id="77ed6-111">Végül, amely minden ablak függvényt kell használni a fontos toonote egy [ **GROUP BY** ](https://msdn.microsoft.com/library/dn835023.aspx) záradékban.</span><span class="sxs-lookup"><span data-stu-id="77ed6-111">Lastly it is important toonote that all Window functions should be used in a [**GROUP BY**](https://msdn.microsoft.com/library/dn835023.aspx) clause.</span></span>

![Stream Analytics ablak Funkciók fogalmak](media/stream-analytics-window-functions/stream-analytics-window-functions-conceptual.png)

## <a name="tumbling-window"></a><span data-ttu-id="77ed6-113">Átfedésmentes ablak</span><span class="sxs-lookup"><span data-stu-id="77ed6-113">Tumbling Window</span></span>
<span data-ttu-id="77ed6-114">Átfedésmentes ablak Funkciók használt toosegment történő különböző idő adatfolyam, és például az alábbi példa hello előzetesen, a művelet végrehajtására szolgál(nak).</span><span class="sxs-lookup"><span data-stu-id="77ed6-114">Tumbling window functions are used toosegment a data stream into distinct time segments and perform a function against them, such as hello example below.</span></span> <span data-ttu-id="77ed6-115">hello kulcs differentiators Átfedésmentes ablak, hogy azok ismételje meg a, nem lehetnek átfedésben és az esemény nem tartozhat toomore, mint egy átfedésmentes ablak.</span><span class="sxs-lookup"><span data-stu-id="77ed6-115">hello key differentiators of a Tumbling window are that they repeat, do not overlap and an event cannot belong toomore than one tumbling window.</span></span>

![Stream Analytics ablak Funkciók átfedésmentes – bevezetés](media/stream-analytics-window-functions/stream-analytics-window-functions-tumbling-intro.png)

## <a name="hopping-window"></a><span data-ttu-id="77ed6-117">Ugróablak</span><span class="sxs-lookup"><span data-stu-id="77ed6-117">Hopping Window</span></span>
<span data-ttu-id="77ed6-118">Szórási ablak Funkciók Ugrás előre meghatározott időtartamon által.</span><span class="sxs-lookup"><span data-stu-id="77ed6-118">Hopping window functions hop forward in time by a fixed period.</span></span> <span data-ttu-id="77ed6-119">Így események tartozhatnak toomore, mint egy Hopping ablak eredménykészlet Átfedésmentes windows egymást átfedő is, mint azok könnyen toothink lehet.</span><span class="sxs-lookup"><span data-stu-id="77ed6-119">It may be easy toothink of them as Tumbling windows that can overlap, so events can belong toomore than one Hopping window result set.</span></span> <span data-ttu-id="77ed6-120">toomake Hopping ablak hello ugyanaz, mint egy egyszerűen kellene megadnia Átfedésmentes ablak hello ugrási méretének toobe hello ugyanaz, mint hello ablakméret.</span><span class="sxs-lookup"><span data-stu-id="77ed6-120">toomake a Hopping window hello same as a Tumbling window one would simply specify hello hop size toobe hello same as hello window size.</span></span> 

![Stream Analytics ablak működik szórási – bevezetés](media/stream-analytics-window-functions/stream-analytics-window-functions-hopping-intro.png)

## <a name="sliding-window"></a><span data-ttu-id="77ed6-122">Csúszóablak</span><span class="sxs-lookup"><span data-stu-id="77ed6-122">Sliding Window</span></span>
<span data-ttu-id="77ed6-123">Mozgó ablak Funkciók Átfedésmentes és a windows, Hopping előállít egy kimeneti **csak** egy esemény bekövetkezésekor.</span><span class="sxs-lookup"><span data-stu-id="77ed6-123">Sliding window functions, unlike Tumbling or Hopping windows, produce an output **only**  when an event occurs.</span></span> <span data-ttu-id="77ed6-124">Minden időszak lesz legalább egy eseményt, és hello ablak folyamatosan mozgatja előre egy "(epszilon).</span><span class="sxs-lookup"><span data-stu-id="77ed6-124">Every window will have at least one event and hello window continuously moves forward by an € (epsilon).</span></span> <span data-ttu-id="77ed6-125">Például a Hopping Windows események toomore, mint egy késleltetett ablak is tartozhatnak.</span><span class="sxs-lookup"><span data-stu-id="77ed6-125">Like Hopping Windows, events can belong toomore than one Sliding Window.</span></span>

![Bevezetés a késleltetett Stream Analytics ablak Funkciók](media/stream-analytics-window-functions/stream-analytics-window-functions-sliding-intro.png)

## <a name="getting-help-with-window-functions"></a><span data-ttu-id="77ed6-127">Az ablak functions kapcsolatos segítség kérése</span><span class="sxs-lookup"><span data-stu-id="77ed6-127">Getting help with Window functions</span></span>
<span data-ttu-id="77ed6-128">További támogatásért keresse fel az [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span><span class="sxs-lookup"><span data-stu-id="77ed6-128">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span></span>

## <a name="next-steps"></a><span data-ttu-id="77ed6-129">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="77ed6-129">Next steps</span></span>
* [<span data-ttu-id="77ed6-130">A Stream Analytics bemutatása tooAzure</span><span class="sxs-lookup"><span data-stu-id="77ed6-130">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* <span data-ttu-id="77ed6-131">[Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md) (Bevezetés az Azure Stream Analytics használatába)</span><span class="sxs-lookup"><span data-stu-id="77ed6-131">[Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)</span></span>
* <span data-ttu-id="77ed6-132">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)</span><span class="sxs-lookup"><span data-stu-id="77ed6-132">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md)</span></span>
* <span data-ttu-id="77ed6-133">[Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referencia az Azure Stream Analytics lekérdezési nyelvhez)</span><span class="sxs-lookup"><span data-stu-id="77ed6-133">[Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)</span></span>
* [<span data-ttu-id="77ed6-134">Az Azure Stream Analytics felügyeleti REST API referenciája</span><span class="sxs-lookup"><span data-stu-id="77ed6-134">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

