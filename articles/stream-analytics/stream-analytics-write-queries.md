---
title: "a Stream Analytics aaaHow toowrite lekérdezések |} Microsoft Docs"
description: "A Stream Analytics és adatait kérdezi le a lekérdezéseket írhat |} tanulási elérésiút-szegmens."
keywords: "Hogyan toowrite a lekérdezések adatait, a lekérdezés, lekérdezések írásáról írása"
documentationcenter: 
services: stream-analytics
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 0e9cdadd-0ee0-4bee-b65b-4a06fb863c95
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: b943c34f10afd2b21789afbd341c471a5f168729
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toowrite-queries-in-stream-analytics"></a><span data-ttu-id="6c49d-104">Hogyan toowrite lekérdezi a Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="6c49d-104">How toowrite queries in Stream Analytics</span></span>
<span data-ttu-id="6c49d-105">A streamfeldolgozó logikákat, az Azure Stream Analytics lekérdezések írásáról valósul meg "állandó lekérdezés" előtt egy feladat elindul, és végre az adatok, mivel lehet spórolni a hello feladat van definiálva.</span><span class="sxs-lookup"><span data-stu-id="6c49d-105">Writing queries for stream processing logic in Azure Stream Analytics is implemented as a "standing query" that is defined before a job starts and executed on data as it reaches hello job.</span></span> <span data-ttu-id="6c49d-106">hello van adatátalakítást, az SQL-szerű lekérdezésnyelvet, amely része a nagy mértékben T-SQL egy hozzáadott nyelvi fájlkiterjesztéseket – például [Ablakozó](https://msdn.microsoft.com/library/azure/dn835019.aspx) tooexpress historikus szemantikáját használja.</span><span class="sxs-lookup"><span data-stu-id="6c49d-106">hello data transformation is expressed in a SQL-like query language, which is largely a subset of T-SQL with some added language extensions like [Windowing](https://msdn.microsoft.com/library/azure/dn835019.aspx) used tooexpress temporal semantics.</span></span>

## <a name="writing-queries"></a><span data-ttu-id="6c49d-107">Lekérdezések írásáról:</span><span class="sxs-lookup"><span data-stu-id="6c49d-107">Writing Queries:</span></span>
1. <span data-ttu-id="6c49d-108">A Stream Analytics-feladat hello Azure felügyeleti portálon, kattintson **lekérdezés**.</span><span class="sxs-lookup"><span data-stu-id="6c49d-108">In your Stream Analytics Job in hello Azure Management portal, click **Query**.</span></span>
   
    ![Válassza ki a lekérdezés](./media/stream-analytics-write-queries/1-stream-analytics-write-queries.png)  
   
    <span data-ttu-id="6c49d-110">Hello Azure portál, kattintson **lekérdezés**.</span><span class="sxs-lookup"><span data-stu-id="6c49d-110">In hello Azure Portal, click **Query**.</span></span>
   
    ![Válassza ki a lekérdezés előnézeti](./media/stream-analytics-write-queries/query-preview-portal.png)  
2. <span data-ttu-id="6c49d-112">Új feladatnak a kezdéshez lekérdezés sablon toohelp vannak.</span><span class="sxs-lookup"><span data-stu-id="6c49d-112">New jobs have a query template toohelp get you started.</span></span> <span data-ttu-id="6c49d-113">hello sablon hajtja végre a "csatlakoztatott" query adott projektek bemeneti események összes mezőjét hello kimeneti be.</span><span class="sxs-lookup"><span data-stu-id="6c49d-113">hello query template performs a "pass-through" query that projects all fields from input events into hello output.</span></span>  
   
   * <span data-ttu-id="6c49d-114">Ha legalább egy bemeneti és kimeneti meghatározta a feldolgozás, lecserélheti hello helyőrző "[YourOutputAlias]" és "[YourInputAlias]" mezőjének hello aliasok a hello bemeneti és kimeneti használata először kívánja.</span><span class="sxs-lookup"><span data-stu-id="6c49d-114">If you have defined at least one input and output for your job, you can replace hello placeholder "[YourOutputAlias]" and "[YourInputAlias]" fields with hello aliases of hello input and output that you wish use first.</span></span> <span data-ttu-id="6c49d-115">Ezenkívül is hozhatnak létre és a lekérdezés tesztelése a klasszikus Azure portál hello hello feladaton bemenetekhez és kimenetekhez definiálása nélkül.</span><span class="sxs-lookup"><span data-stu-id="6c49d-115">In addition, you can still author and test your query in hello Azure Classic Portal without defining inputs and outputs on hello job.</span></span>
   * <span data-ttu-id="6c49d-116">Ha a tooperform mint egyszerű csatlakoztatott további feldolgozás, szerkesztheti hello lekérdezésdefiníciója.</span><span class="sxs-lookup"><span data-stu-id="6c49d-116">If you wish tooperform more processing than a simple pass-through, you can edit hello query definition.</span></span> <span data-ttu-id="6c49d-117">tooget használatába lekérdezés készítése, tekintse meg néhány gyakori lekérdezési minták a rendszer rögzíti [Itt](stream-analytics-stream-analytics-query-patterns.md).</span><span class="sxs-lookup"><span data-stu-id="6c49d-117">tooget started with query authoring, take a look at some common query patterns are captured [here](stream-analytics-stream-analytics-query-patterns.md).</span></span>  
   
   ![Ablak adatait](./media/stream-analytics-write-queries/2-stream-analytics-write-queries.png)  

## <a name="toovalidate-query-data-is-working"></a><span data-ttu-id="6c49d-119">működik-e toovalidate adatait:</span><span class="sxs-lookup"><span data-stu-id="6c49d-119">toovalidate query data is working:</span></span>
<span data-ttu-id="6c49d-120">Tesztelheti, hogy a lekérdezés egy vagy több helyi JSON a fájlokat tartalmazó Tesztadatok hello böngészőben futó várt módon viselkedik-e.</span><span class="sxs-lookup"><span data-stu-id="6c49d-120">You can test that your query behaves as expected by running it in hello browser over one or more local JSON files containing test data.</span></span> <span data-ttu-id="6c49d-121">Ez nem fog elindulni hello feladat vagy számlázási hatással van.</span><span class="sxs-lookup"><span data-stu-id="6c49d-121">This will not start hello job or have any billing implications.</span></span>

> [!NOTE]
> <span data-ttu-id="6c49d-122">Jelenleg a böngészőben a lekérdezéstesztelés nem támogatott hello Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="6c49d-122">Currently in-browser query testing is not supported in hello Azure Portal.</span></span>  
> 
> 

1. <span data-ttu-id="6c49d-123">Győződjön meg arról, hogy nincsenek-e hibák hello lekérdezés (ellenkező esetben hello teszt gomb le lesz tiltva) majd hello tesztelése gombra.</span><span class="sxs-lookup"><span data-stu-id="6c49d-123">Make sure that there are no errors in hello query (otherwise hello Test button will be disabled) and then click hello Test button.</span></span>  
   
   ![Teszt adatait](./media/stream-analytics-write-queries/3-stream-analytics-write-queries.png)  
2. <span data-ttu-id="6c49d-125">Az egyes hello lekérdezésben hivatkozott hello bemenetek felszólító toospecify fájlok lesznek.</span><span class="sxs-lookup"><span data-stu-id="6c49d-125">You will be prompted toospecify files for each of hello inputs referenced in hello query.</span></span> <span data-ttu-id="6c49d-126">Ebben a példában hello sablon lekérdezés állapotban maradt-van, így a "yourinputalias" nevű bemeneti arra kéri a hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="6c49d-126">In this example, hello template query is left as-is, so hello dialog is prompting for an input named "yourinputalias".</span></span>  
   
   ![Adatok lekérdezés tesztelése](./media/stream-analytics-write-queries/4-stream-analytics-write-queries.png)  
3. <span data-ttu-id="6c49d-128">Keresse meg a fájl tooa tesztelése.</span><span class="sxs-lookup"><span data-stu-id="6c49d-128">Browse tooa test file.</span></span> <span data-ttu-id="6c49d-129">Több mintafájlt érhető el a [github](https://github.com/Azure/azure-stream-analytics/tree/master/Sample Data) és a saját adatok adatfolyam bemenetei hello mintaadatok függvény hello bemenetek lapon keresztül is lekérhetik mintaadatok.</span><span class="sxs-lookup"><span data-stu-id="6c49d-129">Several sample files are available on [github](https://github.com/Azure/azure-stream-analytics/tree/master/Sample Data) and you can also retrieve sample data from your own data stream inputs via hello Sample Data function on hello inputs tab.</span></span>  
   
   ![A bemeneti lekérdezés](./media/stream-analytics-write-queries/5-stream-analytics-write-queries.png)  
4. <span data-ttu-id="6c49d-131">Hello párbeszédpanel bezárása, után keresztül hello Tesztadatok futtatni szeretné a lekérdezést, és látni fogja a hello eredmények alján hello hello lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="6c49d-131">After closing hello dialog, your query will be run over hello test data and you will see hello results at hello bottom of hello Query page.</span></span>  
   
   ![Lekérdezés összegzése](./media/stream-analytics-write-queries/6-stream-analytics-write-queries.png)  

## <a name="get-help"></a><span data-ttu-id="6c49d-133">Segítségkérés</span><span class="sxs-lookup"><span data-stu-id="6c49d-133">Get help</span></span>
<span data-ttu-id="6c49d-134">További támogatásért keresse fel az [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span><span class="sxs-lookup"><span data-stu-id="6c49d-134">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span></span>

## <a name="next-steps"></a><span data-ttu-id="6c49d-135">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6c49d-135">Next steps</span></span>
* [<span data-ttu-id="6c49d-136">A Stream Analytics bemutatása tooAzure</span><span class="sxs-lookup"><span data-stu-id="6c49d-136">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* <span data-ttu-id="6c49d-137">[Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md) (Bevezetés az Azure Stream Analytics használatába)</span><span class="sxs-lookup"><span data-stu-id="6c49d-137">[Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)</span></span>
* <span data-ttu-id="6c49d-138">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)</span><span class="sxs-lookup"><span data-stu-id="6c49d-138">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md)</span></span>
* <span data-ttu-id="6c49d-139">[Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referencia az Azure Stream Analytics lekérdezési nyelvhez)</span><span class="sxs-lookup"><span data-stu-id="6c49d-139">[Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)</span></span>
* [<span data-ttu-id="6c49d-140">Az Azure Stream Analytics felügyeleti REST API referenciája</span><span class="sxs-lookup"><span data-stu-id="6c49d-140">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

