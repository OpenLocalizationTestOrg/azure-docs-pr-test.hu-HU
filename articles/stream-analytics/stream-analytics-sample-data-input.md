---
title: "az Azure Stream Analytics AAA mintavételi bemenetek |} Microsoft Docs"
description: "Meghatározhatja problémák elhárításakor Stream Analytics-feladatok."
keywords: "bemeneti, bemeneti mintavételi hibaelhárítása"
documentationcenter: 
services: stream-analytics
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
ms.openlocfilehash: 9637a8664de099eebb8f5654036d2957f4c6b7ce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-stream-analytics-input-stream-sampling"></a><span data-ttu-id="59467-104">Az Azure Stream Analytics bemenet-adatfolyam mintavétel</span><span class="sxs-lookup"><span data-stu-id="59467-104">Azure Stream Analytics input-stream sampling</span></span>

<span data-ttu-id="59467-105">Azure Stream Analytics segítségével bemeneti eseményeket, amelyek fájlból származnak és tesztelheti a lekérdezéseket hello portálon toostart anélkül vagy leállíthat feladatot is mintát.</span><span class="sxs-lookup"><span data-stu-id="59467-105">By using Azure Stream Analytics, you can sample input events that come from a file and test queries in hello portal without needing toostart or stop a job.</span></span>

## <a name="testing-your-query"></a><span data-ttu-id="59467-106">A lekérdezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="59467-106">Testing your query</span></span>

<span data-ttu-id="59467-107">Hello Stream Analytics feladat részletek ablaktábláján nyissa meg a hello **Lekérdezésszerkesztő** panel alatt hello lekérdezés nevére kattintva **lekérdezés**.</span><span class="sxs-lookup"><span data-stu-id="59467-107">In hello Stream Analytics job details pane, open hello **Query editor** blade by clicking hello query name under **Query**.</span></span> <span data-ttu-id="59467-108">(Nem a lekérdezés még jött létre, mert a példaforgatókönyvben, kattintson a hello **< >** helyőrző.)</span><span class="sxs-lookup"><span data-stu-id="59467-108">(In our example scenario, because no query has been created yet, click hello **< >** placeholder.)</span></span>

![hello Stream Analytics query editor](media/stream-analytics-sample-data-input/stream-analytics-query-editor.png)

<span data-ttu-id="59467-110">hello gazdag szerkesztő panelen a lekérdezés létrehozásához volt hello korábbi változatban jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="59467-110">hello rich editor blade for creating your query is displayed as it was in hello previous release.</span></span> <span data-ttu-id="59467-111">Most hello panel frissítve lett egy új, hogy látható hello be- és kimenetekkel, hello a lekérdezés és a feladat meghatározott bal oldali ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="59467-111">Now hello blade has been updated with a new left pane that shows hello inputs and outputs that are used by hello query and defined for this job.</span></span>

![hello Stream Analytics query editor a bemeneti és kimeneti listák](media/stream-analytics-sample-data-input/stream-analytics-query-editor-highlight.png)

<span data-ttu-id="59467-113">Egy további bemeneti és kimeneti, amelyek nem definiált is megjelennek a rendszer.</span><span class="sxs-lookup"><span data-stu-id="59467-113">Also shown are an additional input and output, which are not defined.</span></span> <span data-ttu-id="59467-114">Hello új lekérdezés sablont, amely a kiindulási pont származnak.</span><span class="sxs-lookup"><span data-stu-id="59467-114">They come from hello new query template that you start with.</span></span> <span data-ttu-id="59467-115">Módosíthatja, vagy akár eltűnnek a teljes sorozatnak így hello lekérdezés szerkesztése.</span><span class="sxs-lookup"><span data-stu-id="59467-115">They change, or even disappear altogether, as you edit hello query.</span></span> <span data-ttu-id="59467-116">Nyugodtan figyelmen kívül hagyhatja őket most.</span><span class="sxs-lookup"><span data-stu-id="59467-116">You can safely ignore them for now.</span></span>

<span data-ttu-id="59467-117">tootest minta bemeneti adatok, kattintson a jobb gombbal bármelyik bemenet, és válassza **tölthet fel fájlból adatot**.</span><span class="sxs-lookup"><span data-stu-id="59467-117">tootest with sample input data, right-click any of your inputs, and then select **Upload sample data from file**.</span></span>

![hello Stream Analytics lekérdezési szerkesztő feltöltés mintaadatok fájlból parancs](media/stream-analytics-sample-data-input/stream-analytics-query-editor-upload.png)

<span data-ttu-id="59467-119">Hello feltöltés befejeződése után kattintson **teszt** tootest a hello lekérdezése az imént megadott adatokat.</span><span class="sxs-lookup"><span data-stu-id="59467-119">After hello upload is complete, click **Test** tootest this query against hello sample data you have just provided.</span></span>

![hello Stream Analytics query editor teszt gombra](media/stream-analytics-sample-data-input/stream-analytics-query-editor-test.png)

<span data-ttu-id="59467-121">Ha toosave hello tesztkimenet későbbi használatra, a lekérdezés kimenetét hello egy hivatkozás toohello letöltés eredményeit hello böngésző jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="59467-121">If you want toosave hello test output for later use, hello output of your query is displayed in hello browser with a link toohello download results.</span></span> <span data-ttu-id="59467-122">Mostantól egyszerűen és ismételt módosítsa a lekérdezést és tesztelik azt ismételten toosee hogyan hello kimeneti változik.</span><span class="sxs-lookup"><span data-stu-id="59467-122">You can now easily and iteratively modify your query and test it repeatedly toosee how hello output changes.</span></span>

![Stream Analytics query editor minta kimenet](media/stream-analytics-sample-data-input/stream-analytics-query-editor-samples-output.png)

<span data-ttu-id="59467-124">A kép megelőző hello, egy második kimenet hozzáadva, úgynevezett **HighAvgTempOutput**.</span><span class="sxs-lookup"><span data-stu-id="59467-124">In hello preceding image, a second output has been added, called **HighAvgTempOutput**.</span></span>

<span data-ttu-id="59467-125">Ha több kimenet egy lekérdezésben, dokumentumkönyvtárából hello minden kimeneti külön-külön, és könnyen váltása.</span><span class="sxs-lookup"><span data-stu-id="59467-125">When you use multiple outputs in a query, you can see hello results for each output separately and easily toggle between them.</span></span>

<span data-ttu-id="59467-126">Után elégedett hello eredményeket, mentheti a lekérdezést, indítsa el a feladat, elhelyezkedik vissza, és tekintse meg a Stream Analytics hello magic.</span><span class="sxs-lookup"><span data-stu-id="59467-126">After you are satisfied with hello results, you can save your query, start your job, sit back and watch hello magic of Stream Analytics.</span></span>

## <a name="get-help"></a><span data-ttu-id="59467-127">Segítségkérés</span><span class="sxs-lookup"><span data-stu-id="59467-127">Get help</span></span>

<span data-ttu-id="59467-128">Ha további segítségre van szüksége, próbálkozzon a [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="59467-128">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>

## <a name="next-steps"></a><span data-ttu-id="59467-129">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="59467-129">Next steps</span></span>
* [<span data-ttu-id="59467-130">A Stream Analytics bemutatása tooAzure</span><span class="sxs-lookup"><span data-stu-id="59467-130">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* <span data-ttu-id="59467-131">[Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md) (Bevezetés az Azure Stream Analytics használatába)</span><span class="sxs-lookup"><span data-stu-id="59467-131">[Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)</span></span>
* <span data-ttu-id="59467-132">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)</span><span class="sxs-lookup"><span data-stu-id="59467-132">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md)</span></span>
* <span data-ttu-id="59467-133">[Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referencia az Azure Stream Analytics lekérdezési nyelvhez)</span><span class="sxs-lookup"><span data-stu-id="59467-133">[Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)</span></span>
* [<span data-ttu-id="59467-134">Az Azure Stream Analytics felügyeleti REST API referenciája</span><span class="sxs-lookup"><span data-stu-id="59467-134">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)
