---
title: "Az Azure Stream Analytics lekérdezési tesztelése |} Microsoft Docs"
description: "Hogyan tesztelheti a lekérdezéseket a Stream Analytics-feladatok."
keywords: "lekérdezés tesztelése, a lekérdezés hibaelhárítása"
documentation center: 
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
ms.openlocfilehash: 16bb3f26ec3a69e5204162db9e54a186cf1ec6a6
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="test-azure-stream-analytics-queries-in-the-azure-portal"></a><span data-ttu-id="d8bb6-104">Azure Stream Analytics lekérdezések tesztelése az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="d8bb6-104">Test Azure Stream Analytics queries in the Azure portal</span></span>

<span data-ttu-id="d8bb6-105">Az Azure Stream Analytics lekérdezések tesztelheti az Azure portálon anélkül, hogy indítása vagy leállítása egy feladatot.</span><span class="sxs-lookup"><span data-stu-id="d8bb6-105">With Azure Stream Analytics, you can test queries in the Azure portal without needing to start or stop a job.</span></span>

## <a name="test-the-input"></a><span data-ttu-id="d8bb6-106">A bemeneti tesztelése</span><span class="sxs-lookup"><span data-stu-id="d8bb6-106">Test the input</span></span>

1. <span data-ttu-id="d8bb6-107">A minta bemeneti adatok teszteléséhez kattintson a jobb gombbal bármelyik bemenet, és válassza **tölthet fel fájlból adatot**.</span><span class="sxs-lookup"><span data-stu-id="d8bb6-107">To test with sample input data, right-click any of your inputs, and then select **Upload sample data from file**.</span></span>

    ![Stream analytics lekérdezési szerkesztő lekérdezés tesztelése](media/stream-analytics-test-query/stream-analytics-test-query-editor-upload.png)

2. <span data-ttu-id="d8bb6-109">A feltöltés befejeződése után kattintson **tesztelése** tesztelése a megadott mintaadatokat irányuló lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="d8bb6-109">After the upload is complete, click **Test** to test this query against the sample data you have provided.</span></span>

    ![Stream analytics lekérdezési szerkesztő teszt mintaadatokat](media/stream-analytics-test-query/stream-analytics-test-query-editor-test.png)

<span data-ttu-id="d8bb6-111">A lekérdezés kimenetét jelenik meg a böngészőben a letöltés eredményeit hivatkozással kell menteni szeretné a teszt eredménye későbbi használatra.</span><span class="sxs-lookup"><span data-stu-id="d8bb6-111">The output of your query is displayed in the browser, with Download results link should you want to save the test output for later use.</span></span> <span data-ttu-id="d8bb6-112">Mostantól egyszerűen és ismételt módosítsa a lekérdezést és tesztelik azt hogy ismételten hogyan a kimeneti változik.</span><span class="sxs-lookup"><span data-stu-id="d8bb6-112">You can now easily and iteratively modify your query and test it repeatedly to see how the output changes.</span></span>

![Stream Analytics query editor minta kimenet](media/stream-analytics-test-query/stream-analytics-test-query-editor-samples-output.png)

<span data-ttu-id="d8bb6-114">A lekérdezésben használt több kimenettel külön-külön tekintse meg mindkét kimenetek eredményét, és könnyen közötti váltáshoz.</span><span class="sxs-lookup"><span data-stu-id="d8bb6-114">With multiple outputs used in a query, you can see the results for both outputs separately and easily toggle between them.</span></span>

<span data-ttu-id="d8bb6-115">Miután elkészült a lekérdezés mentéséhez, indítsa el a feladatot, és lehetővé teszik a böngészőben megjelenített eredményekre feldolgozni az eseményeket hiba nélkül.</span><span class="sxs-lookup"><span data-stu-id="d8bb6-115">After you are satisfied with the results shown in the browser, you can save your query, start your job, and let it process events without error.</span></span>

## <a name="get-help"></a><span data-ttu-id="d8bb6-116">Segítségkérés</span><span class="sxs-lookup"><span data-stu-id="d8bb6-116">Get help</span></span>

<span data-ttu-id="d8bb6-117">Ha további segítségre van szüksége, próbálkozzon a [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="d8bb6-117">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>

## <a name="next-steps"></a><span data-ttu-id="d8bb6-118">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d8bb6-118">Next steps</span></span>

* [<span data-ttu-id="d8bb6-119">Az Azure Stream Analytics bemutatása</span><span class="sxs-lookup"><span data-stu-id="d8bb6-119">Introduction to Azure Stream Analytics</span></span>](stream-analytics-introduction.md)
* <span data-ttu-id="d8bb6-120">[Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md) (Bevezetés az Azure Stream Analytics használatába)</span><span class="sxs-lookup"><span data-stu-id="d8bb6-120">[Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)</span></span>
* <span data-ttu-id="d8bb6-121">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)</span><span class="sxs-lookup"><span data-stu-id="d8bb6-121">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md)</span></span>
* <span data-ttu-id="d8bb6-122">[Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referencia az Azure Stream Analytics lekérdezési nyelvhez)</span><span class="sxs-lookup"><span data-stu-id="d8bb6-122">[Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)</span></span>
* [<span data-ttu-id="d8bb6-123">Az Azure Stream Analytics felügyeleti REST API referenciája</span><span class="sxs-lookup"><span data-stu-id="d8bb6-123">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)
