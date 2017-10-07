---
title: "Azure Stream Analytics aaaDebug lekérdezi a SELECT INTO használatával |} Microsoft Docs"
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
ms.openlocfilehash: 27e41af1a6ea06b4509d07a3a67087490d0ec1fd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="debug-queries-by-using-select-into-statements"></a><span data-ttu-id="5590c-103">A SELECT INTO utasítás használatával lekérdezések hibakeresése</span><span class="sxs-lookup"><span data-stu-id="5590c-103">Debug queries by using SELECT INTO statements</span></span>

<span data-ttu-id="5590c-104">A valós idejű adatfeldolgozással ismerik, mely hello adatok tűnik hello hello közepén lekérdezés hasznos lehet.</span><span class="sxs-lookup"><span data-stu-id="5590c-104">In real-time data processing, knowing what hello data looks like in hello middle of hello query can be helpful.</span></span> <span data-ttu-id="5590c-105">Bemeneti adatok vagy a lépéseket az Azure Stream Analytics-feladat több alkalommal olvashatók, mert külön SELECT INTO utasítások is írhat.</span><span class="sxs-lookup"><span data-stu-id="5590c-105">Because inputs or steps of an Azure Stream Analytics job can be read multiple times, you can write extra SELECT INTO statements.</span></span> <span data-ttu-id="5590c-106">Ezzel köztes adatok kiírja a tárolóba, és lehetővé teszi, hogy vizsgálja meg a hello helyességét hello adatok, ahogy *változók bemutató* tegye amikor hibakeresése a program.</span><span class="sxs-lookup"><span data-stu-id="5590c-106">Doing so outputs intermediate data into storage and lets you inspect hello correctness of hello data, just as *watch variables* do when you debug a program.</span></span>

## <a name="use-select-into-toocheck-hello-data-stream"></a><span data-ttu-id="5590c-107">A SELECT INTO toocheck hello az adatfolyam használata</span><span class="sxs-lookup"><span data-stu-id="5590c-107">Use SELECT INTO toocheck hello data stream</span></span>

<span data-ttu-id="5590c-108">hello egy Azure Stream Analytics-feladat a következő példalekérdezés van egy adatfolyam-bemenet, két hivatkozás adatok bemeneti és egy kimeneti tooAzure Table Storage.</span><span class="sxs-lookup"><span data-stu-id="5590c-108">hello following example query in an Azure Stream Analytics job has one stream input, two reference data inputs, and an output tooAzure Table Storage.</span></span> <span data-ttu-id="5590c-109">hello lekérdezés adatokat hello eseményközpont és két blobok tooget hello nevét és kategóriáját referenciaadatai csatlakozik:</span><span class="sxs-lookup"><span data-stu-id="5590c-109">hello query joins data from hello event hub and two reference blobs tooget hello name and category information:</span></span>

![Példa a SELECT INTO lekérdezést](./media/stream-analytics-select-into/stream-analytics-select-into-query1.png)

<span data-ttu-id="5590c-111">Vegye figyelembe, hogy hello feladat fut, de nincsenek események előállítása hello kimenet alatt.</span><span class="sxs-lookup"><span data-stu-id="5590c-111">Note that hello job is running, but no events are being produced in hello output.</span></span> <span data-ttu-id="5590c-112">A hello **figyelés** csempe látható itt, láthatja, hogy hello bemeneti van olyan adatokat, de nem tudja, melyik hello lépését **csatlakozás** összes hello eldobott események toobe okozta.</span><span class="sxs-lookup"><span data-stu-id="5590c-112">On hello **Monitoring** tile, shown here, you can see that hello input is producing data, but you don’t know which step of hello **JOIN** caused all hello events toobe dropped.</span></span>

![hello figyelés csempe](./media/stream-analytics-select-into/stream-analytics-select-into-monitor.png)
 
<span data-ttu-id="5590c-114">Ebben az esetben is hozzáadhat, néhány további SELECT INTO utasítások túl "naplófájl" hello köztes ILLESZTÉS eredményei és hello hello bemeneti olvasható adatok.</span><span class="sxs-lookup"><span data-stu-id="5590c-114">In this situation, you can add a few extra SELECT INTO statements too“log” hello intermediate JOIN results and hello data that's read from hello input.</span></span>

<span data-ttu-id="5590c-115">Ebben a példában fel lett véve a két új "ideiglenes kimenetek."</span><span class="sxs-lookup"><span data-stu-id="5590c-115">In this example, we've added two new “temporary outputs.”</span></span> <span data-ttu-id="5590c-116">A fogadó tetszés el.</span><span class="sxs-lookup"><span data-stu-id="5590c-116">They can be any sink you like.</span></span> <span data-ttu-id="5590c-117">Azure Storage itt példaként használjuk:</span><span class="sxs-lookup"><span data-stu-id="5590c-117">Here we use Azure Storage as an example:</span></span>

![Extra SELECT INTO utasítások hozzáadása](./media/stream-analytics-select-into/stream-analytics-select-into-outputs.png)

<span data-ttu-id="5590c-119">Ehhez hasonló hello lekérdezés majd módosíthatja:</span><span class="sxs-lookup"><span data-stu-id="5590c-119">You can then rewrite hello query like this:</span></span>

![Egy átírt SELECT INTO lekérdezést](./media/stream-analytics-select-into/stream-analytics-select-into-query2.png)

<span data-ttu-id="5590c-121">Most indítsa újra a hello feladat, és hagyja, hogy néhány percig futtassa.</span><span class="sxs-lookup"><span data-stu-id="5590c-121">Now start hello job again, and let it run for a few minutes.</span></span> <span data-ttu-id="5590c-122">Ezután lekérdezés temp1, és a Visual Studio Cloud Explorer tooproduce hello a következő táblák temp2:</span><span class="sxs-lookup"><span data-stu-id="5590c-122">Then query temp1 and temp2 with Visual Studio Cloud Explorer tooproduce hello following tables:</span></span>

<span data-ttu-id="5590c-123">**temp1 tábla**
![SELECT INTO temp1 tábla](./media/stream-analytics-select-into/stream-analytics-select-into-temp-table-1.png)</span><span class="sxs-lookup"><span data-stu-id="5590c-123">**temp1 table**
![SELECT INTO temp1 table](./media/stream-analytics-select-into/stream-analytics-select-into-temp-table-1.png)</span></span>

<span data-ttu-id="5590c-124">**temp2 tábla**
![SELECT INTO temp2 tábla](./media/stream-analytics-select-into/stream-analytics-select-into-temp-table-2.png)</span><span class="sxs-lookup"><span data-stu-id="5590c-124">**temp2 table**
![SELECT INTO temp2 table](./media/stream-analytics-select-into/stream-analytics-select-into-temp-table-2.png)</span></span>

<span data-ttu-id="5590c-125">Ahogy látja, temp1 és temp2 mindkét és adatokat, és hello névoszlopa megfelelően van-e feltöltve temp2.</span><span class="sxs-lookup"><span data-stu-id="5590c-125">As you can see, temp1 and temp2 both have data, and hello name column is populated correctly in temp2.</span></span> <span data-ttu-id="5590c-126">Azonban mivel a még nincsenek adatok kimenet, valami probléma:</span><span class="sxs-lookup"><span data-stu-id="5590c-126">However, because there is still no data in output, something is wrong:</span></span>

![A SELECT INTO output1 tábla adatot nem tartalmazó](./media/stream-analytics-select-into/stream-analytics-select-into-out-table-1.png)

<span data-ttu-id="5590c-128">Hello az adatok mintavétele biztos lehet szinte bizonyos jelenti, hogy hello probléma hello ILLESZTÉSI második.</span><span class="sxs-lookup"><span data-stu-id="5590c-128">By sampling hello data, you can be almost certain that hello issue is with hello second JOIN.</span></span> <span data-ttu-id="5590c-129">Hello blob hello referenciaadatok letöltését, és tekintse meg:</span><span class="sxs-lookup"><span data-stu-id="5590c-129">You can download hello reference data from hello blob and take a look:</span></span>

![A SELECT INTO ref tábla](./media/stream-analytics-select-into/stream-analytics-select-into-ref-table-1.png)

<span data-ttu-id="5590c-131">Ahogy látja, a referenciaadatok a GUID hello hello formátuma eltér [a] oszlopában temp2 hello hello formátuma.</span><span class="sxs-lookup"><span data-stu-id="5590c-131">As you can see, hello format of hello GUID in this reference data is different from hello format of hello [from] column in temp2.</span></span> <span data-ttu-id="5590c-132">Ezért hello adatok nem érkeznek output1 a várt módon.</span><span class="sxs-lookup"><span data-stu-id="5590c-132">That’s why hello data didn’t arrive in output1 as expected.</span></span>

<span data-ttu-id="5590c-133">Javítsa ki a hello adatformátum, tooreference blob feltöltése, és próbálkozzon újra:</span><span class="sxs-lookup"><span data-stu-id="5590c-133">You can fix hello data format, upload it tooreference blob, and try again:</span></span>

![A SELECT INTO ideiglenes tábla](./media/stream-analytics-select-into/stream-analytics-select-into-ref-table-2.png)

<span data-ttu-id="5590c-135">Megadott idő hello adatok hello kimenet formázásakor és feltölti a várt módon.</span><span class="sxs-lookup"><span data-stu-id="5590c-135">This time, hello data in hello output is formatted and populated as expected.</span></span>

![A SELECT INTO végső tábla](./media/stream-analytics-select-into/stream-analytics-select-into-final-table.png)


## <a name="get-help"></a><span data-ttu-id="5590c-137">Segítségkérés</span><span class="sxs-lookup"><span data-stu-id="5590c-137">Get help</span></span>

<span data-ttu-id="5590c-138">Ha további segítségre van szüksége, próbálkozzon a [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="5590c-138">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>

## <a name="next-steps"></a><span data-ttu-id="5590c-139">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5590c-139">Next steps</span></span>

* [<span data-ttu-id="5590c-140">A Stream Analytics bemutatása tooAzure</span><span class="sxs-lookup"><span data-stu-id="5590c-140">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* <span data-ttu-id="5590c-141">[Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md) (Bevezetés az Azure Stream Analytics használatába)</span><span class="sxs-lookup"><span data-stu-id="5590c-141">[Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)</span></span>
* <span data-ttu-id="5590c-142">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)</span><span class="sxs-lookup"><span data-stu-id="5590c-142">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md)</span></span>
* <span data-ttu-id="5590c-143">[Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referencia az Azure Stream Analytics lekérdezési nyelvhez)</span><span class="sxs-lookup"><span data-stu-id="5590c-143">[Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)</span></span>
* [<span data-ttu-id="5590c-144">Az Azure Stream Analytics felügyeleti REST API referenciája</span><span class="sxs-lookup"><span data-stu-id="5590c-144">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

