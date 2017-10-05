---
title: "A SELECT INTO Azure Stream Analytics lekérdezések Debug |} Microsoft Docs"
description: "Adatok közepes mintalekérdezés Stream Analytics a SELECT INTO utasítások használatával"
keywords: 
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 9952e2cf-b335-4a5c-8f45-8d3e1eda2e20
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/20/2017
ms.author: jeffstok
ms.openlocfilehash: b05222c6d6f4fc2c5b847dd75ff7e29352cd538c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="debug-queries-by-using-select-into-statements"></a><span data-ttu-id="c5b93-103">A SELECT INTO utasítás használatával lekérdezések hibakeresése</span><span class="sxs-lookup"><span data-stu-id="c5b93-103">Debug queries by using SELECT INTO statements</span></span>

<span data-ttu-id="c5b93-104">A valós idejű adatfeldolgozással az adatok néz közepén a lekérdezés ismerete hasznos lehet.</span><span class="sxs-lookup"><span data-stu-id="c5b93-104">In real-time data processing, knowing what the data looks like in the middle of the query can be helpful.</span></span> <span data-ttu-id="c5b93-105">Bemeneti adatok vagy a lépéseket az Azure Stream Analytics-feladat több alkalommal olvashatók, mert külön SELECT INTO utasítások is írhat.</span><span class="sxs-lookup"><span data-stu-id="c5b93-105">Because inputs or steps of an Azure Stream Analytics job can be read multiple times, you can write extra SELECT INTO statements.</span></span> <span data-ttu-id="c5b93-106">Ezzel a közbülső adatok kiírja a tárolóba, és lehetővé teszi, hogy ellenőrizze a helyességét az adatokat, ugyanúgy, mint *változók bemutató* tegye amikor hibakeresése a program.</span><span class="sxs-lookup"><span data-stu-id="c5b93-106">Doing so outputs intermediate data into storage and lets you inspect the correctness of the data, just as *watch variables* do when you debug a program.</span></span>

## <a name="use-select-into-to-check-the-data-stream"></a><span data-ttu-id="c5b93-107">A SELECT INTO használatával ellenőrizze az adatfolyam</span><span class="sxs-lookup"><span data-stu-id="c5b93-107">Use SELECT INTO to check the data stream</span></span>

<span data-ttu-id="c5b93-108">Egy Azure Stream Analytics-feladat a következő példa lekérdezést egy adatfolyam-bemenet, a két hivatkozás adatok bemeneti és a Azure Table Storage kimenettel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="c5b93-108">The following example query in an Azure Stream Analytics job has one stream input, two reference data inputs, and an output to Azure Table Storage.</span></span> <span data-ttu-id="c5b93-109">A lekérdezés az event hubs és a két hivatkozás blobok a nevét és kategóriáját adatainak megszerzése adatokból csatlakozik:</span><span class="sxs-lookup"><span data-stu-id="c5b93-109">The query joins data from the event hub and two reference blobs to get the name and category information:</span></span>

![Példa a SELECT INTO lekérdezést](./media/stream-analytics-select-into/stream-analytics-select-into-query1.png)

<span data-ttu-id="c5b93-111">Vegye figyelembe, hogy a feladat fut, de nincsenek események a kimenetben alatt előállítása.</span><span class="sxs-lookup"><span data-stu-id="c5b93-111">Note that the job is running, but no events are being produced in the output.</span></span> <span data-ttu-id="c5b93-112">A a **figyelés** csempe látható itt, láthatja, hogy a bemeneti van olyan adatokat, de nem tudja, melyik lépésében a **csatlakozás** elvet összes esemény miatt.</span><span class="sxs-lookup"><span data-stu-id="c5b93-112">On the **Monitoring** tile, shown here, you can see that the input is producing data, but you don’t know which step of the **JOIN** caused all the events to be dropped.</span></span>

![A figyelés csempe](./media/stream-analytics-select-into/stream-analytics-select-into-monitor.png)
 
<span data-ttu-id="c5b93-114">Ez a helyzet néhány további SELECT INTO utasítások a "" a köztes ILLESZTÉS eredményei és bemenetről beolvasott adatok adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="c5b93-114">In this situation, you can add a few extra SELECT INTO statements to “log” the intermediate JOIN results and the data that's read from the input.</span></span>

<span data-ttu-id="c5b93-115">Ebben a példában fel lett véve a két új "ideiglenes kimenetek."</span><span class="sxs-lookup"><span data-stu-id="c5b93-115">In this example, we've added two new “temporary outputs.”</span></span> <span data-ttu-id="c5b93-116">A fogadó tetszés el.</span><span class="sxs-lookup"><span data-stu-id="c5b93-116">They can be any sink you like.</span></span> <span data-ttu-id="c5b93-117">Azure Storage itt példaként használjuk:</span><span class="sxs-lookup"><span data-stu-id="c5b93-117">Here we use Azure Storage as an example:</span></span>

![Extra SELECT INTO utasítások hozzáadása](./media/stream-analytics-select-into/stream-analytics-select-into-outputs.png)

<span data-ttu-id="c5b93-119">Majd módosíthatja a lekérdezés ehhez hasonló:</span><span class="sxs-lookup"><span data-stu-id="c5b93-119">You can then rewrite the query like this:</span></span>

![Egy átírt SELECT INTO lekérdezést](./media/stream-analytics-select-into/stream-analytics-select-into-query2.png)

<span data-ttu-id="c5b93-121">Most indítsa el újra a feladatot, és azt a Futtatás néhány percig.</span><span class="sxs-lookup"><span data-stu-id="c5b93-121">Now start the job again, and let it run for a few minutes.</span></span> <span data-ttu-id="c5b93-122">Majd lekérdezés temp1 és a Visual Studio Cloud Explorer létrehozásához az alábbi táblázatok temp2:</span><span class="sxs-lookup"><span data-stu-id="c5b93-122">Then query temp1 and temp2 with Visual Studio Cloud Explorer to produce the following tables:</span></span>

<span data-ttu-id="c5b93-123">**temp1 tábla**
![SELECT INTO temp1 tábla](./media/stream-analytics-select-into/stream-analytics-select-into-temp-table-1.png)</span><span class="sxs-lookup"><span data-stu-id="c5b93-123">**temp1 table**
![SELECT INTO temp1 table](./media/stream-analytics-select-into/stream-analytics-select-into-temp-table-1.png)</span></span>

<span data-ttu-id="c5b93-124">**temp2 tábla**
![SELECT INTO temp2 tábla](./media/stream-analytics-select-into/stream-analytics-select-into-temp-table-2.png)</span><span class="sxs-lookup"><span data-stu-id="c5b93-124">**temp2 table**
![SELECT INTO temp2 table](./media/stream-analytics-select-into/stream-analytics-select-into-temp-table-2.png)</span></span>

<span data-ttu-id="c5b93-125">Ahogy látja, temp1 és temp2 mindkét és adatokat, és a Név oszlopban nem üres megfelelően temp2.</span><span class="sxs-lookup"><span data-stu-id="c5b93-125">As you can see, temp1 and temp2 both have data, and the name column is populated correctly in temp2.</span></span> <span data-ttu-id="c5b93-126">Azonban mivel a még nincsenek adatok kimenet, valami probléma:</span><span class="sxs-lookup"><span data-stu-id="c5b93-126">However, because there is still no data in output, something is wrong:</span></span>

![A SELECT INTO output1 tábla adatot nem tartalmazó](./media/stream-analytics-select-into/stream-analytics-select-into-out-table-1.png)

<span data-ttu-id="c5b93-128">Az adatok mintavétele, biztos lehet majdnem biztos, hogy a probléma van a második való CSATLAKOZÁST.</span><span class="sxs-lookup"><span data-stu-id="c5b93-128">By sampling the data, you can be almost certain that the issue is with the second JOIN.</span></span> <span data-ttu-id="c5b93-129">A referenciaadatok letöltését a blob, és tekintse meg:</span><span class="sxs-lookup"><span data-stu-id="c5b93-129">You can download the reference data from the blob and take a look:</span></span>

![A SELECT INTO ref tábla](./media/stream-analytics-select-into/stream-analytics-select-into-ref-table-1.png)

<span data-ttu-id="c5b93-131">Ahogy látja, a referenciaadatok a GUID formátuma formátuma eltér az [a] oszlopában temp2.</span><span class="sxs-lookup"><span data-stu-id="c5b93-131">As you can see, the format of the GUID in this reference data is different from the format of the [from] column in temp2.</span></span> <span data-ttu-id="c5b93-132">Ezért az adatok nem érkeznek output1 a várt módon.</span><span class="sxs-lookup"><span data-stu-id="c5b93-132">That’s why the data didn’t arrive in output1 as expected.</span></span>

<span data-ttu-id="c5b93-133">Hárítsa el a adatformátum, töltse fel az blob hivatkoznak, és próbálkozzon újra:</span><span class="sxs-lookup"><span data-stu-id="c5b93-133">You can fix the data format, upload it to reference blob, and try again:</span></span>

![A SELECT INTO ideiglenes tábla](./media/stream-analytics-select-into/stream-analytics-select-into-ref-table-2.png)

<span data-ttu-id="c5b93-135">Most, az adatok a kimenetben formázott és feltölti a várt módon.</span><span class="sxs-lookup"><span data-stu-id="c5b93-135">This time, the data in the output is formatted and populated as expected.</span></span>

![A SELECT INTO végső tábla](./media/stream-analytics-select-into/stream-analytics-select-into-final-table.png)


## <a name="get-help"></a><span data-ttu-id="c5b93-137">Segítségkérés</span><span class="sxs-lookup"><span data-stu-id="c5b93-137">Get help</span></span>

<span data-ttu-id="c5b93-138">Ha további segítségre van szüksége, próbálkozzon a [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="c5b93-138">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>

## <a name="next-steps"></a><span data-ttu-id="c5b93-139">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c5b93-139">Next steps</span></span>

* [<span data-ttu-id="c5b93-140">Az Azure Stream Analytics bemutatása</span><span class="sxs-lookup"><span data-stu-id="c5b93-140">Introduction to Azure Stream Analytics</span></span>](stream-analytics-introduction.md)
* <span data-ttu-id="c5b93-141">[Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md) (Bevezetés az Azure Stream Analytics használatába)</span><span class="sxs-lookup"><span data-stu-id="c5b93-141">[Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)</span></span>
* <span data-ttu-id="c5b93-142">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)</span><span class="sxs-lookup"><span data-stu-id="c5b93-142">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md)</span></span>
* <span data-ttu-id="c5b93-143">[Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referencia az Azure Stream Analytics lekérdezési nyelvhez)</span><span class="sxs-lookup"><span data-stu-id="c5b93-143">[Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)</span></span>
* [<span data-ttu-id="c5b93-144">Az Azure Stream Analytics felügyeleti REST API referenciája</span><span class="sxs-lookup"><span data-stu-id="c5b93-144">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

