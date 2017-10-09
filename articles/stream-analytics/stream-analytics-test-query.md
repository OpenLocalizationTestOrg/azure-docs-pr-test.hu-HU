---
title: "aaaAzure Stream Analytics lekérdezési tesztelése |} Microsoft Docs"
description: "Hogyan tootest a lekérdezéseket a Stream Analytics-feladatok."
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
ms.openlocfilehash: 3b141d98332fdc170e696e181c8446796a86f78e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="test-azure-stream-analytics-queries-in-hello-azure-portal"></a><span data-ttu-id="4de1a-104">Azure Stream Analytics lekérdezések tesztelése hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="4de1a-104">Test Azure Stream Analytics queries in hello Azure portal</span></span>

<span data-ttu-id="4de1a-105">Az Azure Stream Analytics lekérdezések tesztelhet hello Azure-portálon a toostart anélkül, illetve leállíthat feladatot.</span><span class="sxs-lookup"><span data-stu-id="4de1a-105">With Azure Stream Analytics, you can test queries in hello Azure portal without needing toostart or stop a job.</span></span>

## <a name="test-hello-input"></a><span data-ttu-id="4de1a-106">Hello bemeneti tesztelése</span><span class="sxs-lookup"><span data-stu-id="4de1a-106">Test hello input</span></span>

1. <span data-ttu-id="4de1a-107">tootest minta bemeneti adatok, kattintson a jobb gombbal bármelyik bemenet, és válassza **tölthet fel fájlból adatot**.</span><span class="sxs-lookup"><span data-stu-id="4de1a-107">tootest with sample input data, right-click any of your inputs, and then select **Upload sample data from file**.</span></span>

    ![Stream analytics lekérdezési szerkesztő lekérdezés tesztelése](media/stream-analytics-test-query/stream-analytics-test-query-editor-upload.png)

2. <span data-ttu-id="4de1a-109">Hello feltöltés befejeződése után kattintson **teszt** tootest a hello lekérdezése az megadott adatokat.</span><span class="sxs-lookup"><span data-stu-id="4de1a-109">After hello upload is complete, click **Test** tootest this query against hello sample data you have provided.</span></span>

    ![Stream analytics lekérdezési szerkesztő teszt mintaadatokat](media/stream-analytics-test-query/stream-analytics-test-query-editor-test.png)

<span data-ttu-id="4de1a-111">a lekérdezés kimenetét hello megjelenő hello böngészőben a letöltés eredményeit hivatkozással azt szeretné, hogy toosave hello tesztkimenet későbbi használatra.</span><span class="sxs-lookup"><span data-stu-id="4de1a-111">hello output of your query is displayed in hello browser, with Download results link should you want toosave hello test output for later use.</span></span> <span data-ttu-id="4de1a-112">Mostantól egyszerűen és ismételt módosítsa a lekérdezést és tesztelik azt ismételten toosee hogyan hello kimeneti változik.</span><span class="sxs-lookup"><span data-stu-id="4de1a-112">You can now easily and iteratively modify your query and test it repeatedly toosee how hello output changes.</span></span>

![Stream Analytics query editor minta kimenet](media/stream-analytics-test-query/stream-analytics-test-query-editor-samples-output.png)

<span data-ttu-id="4de1a-114">Több kimenettel, a lekérdezésben használt lásd: külön-külön is kimenetek hello eredményeit, és könnyen váltása.</span><span class="sxs-lookup"><span data-stu-id="4de1a-114">With multiple outputs used in a query, you can see hello results for both outputs separately and easily toggle between them.</span></span>

<span data-ttu-id="4de1a-115">Miután elégedett a lekérdezés mentéséhez, indítsa el a feladatot, és lehetővé teszik hello böngészőben megjelenített hello eredményekkel feldolgozni az eseményeket hiba nélkül.</span><span class="sxs-lookup"><span data-stu-id="4de1a-115">After you are satisfied with hello results shown in hello browser, you can save your query, start your job, and let it process events without error.</span></span>

## <a name="get-help"></a><span data-ttu-id="4de1a-116">Segítségkérés</span><span class="sxs-lookup"><span data-stu-id="4de1a-116">Get help</span></span>

<span data-ttu-id="4de1a-117">Ha további segítségre van szüksége, próbálkozzon a [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="4de1a-117">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>

## <a name="next-steps"></a><span data-ttu-id="4de1a-118">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4de1a-118">Next steps</span></span>

* [<span data-ttu-id="4de1a-119">A Stream Analytics bemutatása tooAzure</span><span class="sxs-lookup"><span data-stu-id="4de1a-119">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* <span data-ttu-id="4de1a-120">[Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md) (Bevezetés az Azure Stream Analytics használatába)</span><span class="sxs-lookup"><span data-stu-id="4de1a-120">[Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)</span></span>
* <span data-ttu-id="4de1a-121">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)</span><span class="sxs-lookup"><span data-stu-id="4de1a-121">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md)</span></span>
* <span data-ttu-id="4de1a-122">[Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referencia az Azure Stream Analytics lekérdezési nyelvhez)</span><span class="sxs-lookup"><span data-stu-id="4de1a-122">[Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)</span></span>
* [<span data-ttu-id="4de1a-123">Az Azure Stream Analytics felügyeleti REST API referenciája</span><span class="sxs-lookup"><span data-stu-id="4de1a-123">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)
