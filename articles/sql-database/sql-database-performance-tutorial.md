---
title: "aaaTroubleshoot teljesítmény állít ki, és az adatbázis optimalizálása |} Microsoft Docs"
description: "Teljesítmény javaslatok tooyour SQL-adatbázis, valamint törlése hogyan toogain észrevételeket hello hello lekérdezések az adatbázis futtatott teljesítményének alkalmazása"
metakeywords: azure sql database performance monitoring recommendation
services: sql-database
documentationcenter: 
manager: jhubbard
author: jan-eng
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,monitor & tune
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2017
ms.author: janeng
ms.openlocfilehash: e948d30ac74eecf45420d5d77ef55e3c0b6f3f47
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-performance-issues-and-optimize-your-database"></a><span data-ttu-id="cdfe3-103">Optimalizálhatja az adatbázist, és a teljesítménnyel kapcsolatos problémák elhárítása</span><span class="sxs-lookup"><span data-stu-id="cdfe3-103">Troubleshoot performance issues and optimize your database</span></span>

<span data-ttu-id="cdfe3-104">A hiányzó indexek és rosszul optimalizált lekérdezések az adatbázis gyenge teljesítményének gyakori okai.</span><span class="sxs-lookup"><span data-stu-id="cdfe3-104">Missing indexes and poorly optimized queries are common reasons for poor database performance.</span></span> <span data-ttu-id="cdfe3-105">Ebben az oktatóanyagban elsajátíthatja, hogy:</span><span class="sxs-lookup"><span data-stu-id="cdfe3-105">In this tutorial you learn to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="cdfe3-106">Tekintse át, alkalmazza, és állítsa vissza a teljesítmény fokozása javaslatok</span><span class="sxs-lookup"><span data-stu-id="cdfe3-106">Review, apply and revert performance improvement recommendations</span></span>
> * <span data-ttu-id="cdfe3-107">Az magas erőforrás-használat lekérdezések keresése</span><span class="sxs-lookup"><span data-stu-id="cdfe3-107">Find queries with high resource utilization</span></span>
> * <span data-ttu-id="cdfe3-108">Hosszú ideig futó lekérdezések keresése</span><span class="sxs-lookup"><span data-stu-id="cdfe3-108">Find long running queries</span></span>

> <span data-ttu-id="cdfe3-109">A folyamatos munkaterhelés rendelkező adatbázison teljesítményproblémák – hiányzó index például tooreceive ajánlást van szüksége.</span><span class="sxs-lookup"><span data-stu-id="cdfe3-109">You need a continuous workload on a database with performance issues – missing an index for example tooreceive a recommendation.</span></span>
>

## <a name="log-in-toohello-azure-portal"></a><span data-ttu-id="cdfe3-110">Jelentkezzen be toohello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="cdfe3-110">Log in toohello Azure portal</span></span>

<span data-ttu-id="cdfe3-111">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="cdfe3-111">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>

## <a name="review-and-apply-a-recommendation"></a><span data-ttu-id="cdfe3-112">Tekintse át és alkalmaz egy javaslatot</span><span class="sxs-lookup"><span data-stu-id="cdfe3-112">Review and apply a recommendation</span></span>

<span data-ttu-id="cdfe3-113">Kövesse ezeket a lépéseket tooapply ajánlást hello rendszerből, az adatbázis számára:</span><span class="sxs-lookup"><span data-stu-id="cdfe3-113">Follow these steps tooapply a recommendation from hello system for your database:</span></span>

1. <span data-ttu-id="cdfe3-114">Kattintson a hello **teljesítmény javaslatok** menü hello adatbázis paneljén.</span><span class="sxs-lookup"><span data-stu-id="cdfe3-114">Click hello **Performance recommendations** menu in hello database blade.</span></span>

    ![teljesítmény javaslat](./media/sql-database-performance-tutorial/perf_recommendations.png)

2. <span data-ttu-id="cdfe3-116">Az ajánlások hello listájából válassza ki az aktív javaslat.</span><span class="sxs-lookup"><span data-stu-id="cdfe3-116">From hello list of recommendations, select an active recommendation.</span></span> <span data-ttu-id="cdfe3-117">A példában a Create Index.</span><span class="sxs-lookup"><span data-stu-id="cdfe3-117">In this example, Create Index.</span></span>

    ![Válassza ki a javaslat](./media/sql-database-performance-tutorial/create_index.png)

3. <span data-ttu-id="cdfe3-119">Hello javaslat alkalmazása hello kattintva **alkalmaz** gombra.</span><span class="sxs-lookup"><span data-stu-id="cdfe3-119">Apply hello recommendation by clicking hello **Apply** button.</span></span> <span data-ttu-id="cdfe3-120">Szükség esetén hello javaslat részletes leírását, és tekintse meg a hello T-SQL parancsfájl túl kattintva hajtható végre **parancsfájl megjelenítése** gombra.</span><span class="sxs-lookup"><span data-stu-id="cdfe3-120">Optionally, review hello recommendation details and see hello T-SQL script too be executed by clicking on **View Script** button.</span></span>

    ![A javaslat alkalmazása](./media/sql-database-performance-tutorial/apply.png)

4. <span data-ttu-id="cdfe3-122">[Választható] Engedélyezze a javaslatok toobe automatikusan alkalmazza az automatikus hangolása.</span><span class="sxs-lookup"><span data-stu-id="cdfe3-122">[Optional] Enable automatic tuning for recommendations toobe applied automatically.</span></span>

    ![automatikus hangolása](./media/sql-database-performance-tutorial/auto_tuning.png)

## <a name="revert-a-recommendation"></a><span data-ttu-id="cdfe3-124">Állítsa vissza a javaslat</span><span class="sxs-lookup"><span data-stu-id="cdfe3-124">Revert a recommendation</span></span>

<span data-ttu-id="cdfe3-125">Database Advisor hello minden javaslat megvalósított figyeli.</span><span class="sxs-lookup"><span data-stu-id="cdfe3-125">hello Database Advisor monitors every recommendation implemented.</span></span> <span data-ttu-id="cdfe3-126">Ha egy javaslatot nem javítása hello munkaterhelés automatikusan visszaáll.</span><span class="sxs-lookup"><span data-stu-id="cdfe3-126">If a recommendation doesn't improve hello workload it will be automatically reverted.</span></span> <span data-ttu-id="cdfe3-127">Lehetséges, de a legtöbb esetben nem szükséges manuálisan, akkor a gyermekrekordokra ajánlást.</span><span class="sxs-lookup"><span data-stu-id="cdfe3-127">Manually reverting a recommendation is possible, but not necessary in most cases.</span></span> <span data-ttu-id="cdfe3-128">a javaslat toorevert:</span><span class="sxs-lookup"><span data-stu-id="cdfe3-128">toorevert a recommendation:</span></span>

1. <span data-ttu-id="cdfe3-129">Nyissa meg toohello teljesítmény javaslatok menü, és válasszon ki egy alkalmazott hello javaslatokat.</span><span class="sxs-lookup"><span data-stu-id="cdfe3-129">Go toohello performance recommendations menu and select one of hello applied recommendations.</span></span>

    ![Válassza ki a javaslat](./media/sql-database-performance-tutorial/select.png)

2. <span data-ttu-id="cdfe3-131">Hello Részletek nézetben kattintson **visszaállítás**.</span><span class="sxs-lookup"><span data-stu-id="cdfe3-131">In hello details view, click **Revert**.</span></span>

    ![Állítsa vissza a javaslat](./media/sql-database-performance-tutorial/revert.png)

## <a name="find-hello-query-that-consumes-hello-most-resources"></a><span data-ttu-id="cdfe3-133">Hello lekérdezés, amely akkor hello legtöbb erőforrások keresése</span><span class="sxs-lookup"><span data-stu-id="cdfe3-133">Find hello query that consumes hello most resources</span></span>

<span data-ttu-id="cdfe3-134">Kövesse a lépéseket toofind hello lekérdezés hello fel legtöbb erőforrást:</span><span class="sxs-lookup"><span data-stu-id="cdfe3-134">Follow these steps toofind hello query consuming hello most resources:</span></span>

1. <span data-ttu-id="cdfe3-135">Kattintson a hello **lekérdezési Terheléselemző** menü hello adatbázis paneljén.</span><span class="sxs-lookup"><span data-stu-id="cdfe3-135">Click on hello **Query Performance Insight** menu in hello database blade.</span></span>

    ![lekérdezési](./media/sql-database-performance-tutorial/query_perf_insights.png)

2. <span data-ttu-id="cdfe3-137">Válasszon ki egy erőforrástípust.</span><span class="sxs-lookup"><span data-stu-id="cdfe3-137">Select a resource type.</span></span>

    ![lekérdezési](./media/sql-database-performance-tutorial/select_resource_type.png)

3. <span data-ttu-id="cdfe3-139">Válassza ki a hello első lekérdezés hello táblában.</span><span class="sxs-lookup"><span data-stu-id="cdfe3-139">Select hello first query in hello table.</span></span>

    ![lekérdezési](./media/sql-database-performance-tutorial/select_query.png)

4. <span data-ttu-id="cdfe3-141">Tekintse át a hello lekérdezés részletes adatait.</span><span class="sxs-lookup"><span data-stu-id="cdfe3-141">Review hello query details.</span></span>

    ![lekérdezési](./media/sql-database-performance-tutorial/query_details.png)

## <a name="find-hello-longest-running-query"></a><span data-ttu-id="cdfe3-143">Hello leghosszabb futó lekérdezés</span><span class="sxs-lookup"><span data-stu-id="cdfe3-143">Find hello longest running query</span></span>

1. <span data-ttu-id="cdfe3-144">Nyissa meg tooQuery teljesítmény elemzését, és válassza ki a hello **hosszú ideig futó lekérdezések** fülre.</span><span class="sxs-lookup"><span data-stu-id="cdfe3-144">Go tooQuery Performance Insight and select hello **Long running queries** tab.</span></span>

    ![lekérdezési](./media/sql-database-performance-tutorial/long_running.png)

3. <span data-ttu-id="cdfe3-146">Válassza ki a hello első lekérdezés hello táblában.</span><span class="sxs-lookup"><span data-stu-id="cdfe3-146">Select hello first query in hello table.</span></span>

    ![lekérdezési](./media/sql-database-performance-tutorial/select_first_query.png)

4. <span data-ttu-id="cdfe3-148">Tekintse át a hello lekérdezés részletes adatait.</span><span class="sxs-lookup"><span data-stu-id="cdfe3-148">Review hello query details.</span></span>

    ![lekérdezési](./media/sql-database-performance-tutorial/review_query_details.png)



## <a name="next-steps"></a><span data-ttu-id="cdfe3-150">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cdfe3-150">Next steps</span></span> 
<span data-ttu-id="cdfe3-151">A hiányzó indexek és rosszul optimalizált lekérdezések az adatbázis gyenge teljesítményének gyakori okai.</span><span class="sxs-lookup"><span data-stu-id="cdfe3-151">Missing indexes and poorly optimized queries are common reasons for poor database performance.</span></span> <span data-ttu-id="cdfe3-152">Ez az oktatóanyag megtanulta, hogy:</span><span class="sxs-lookup"><span data-stu-id="cdfe3-152">In this tutorial you learned to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="cdfe3-153">Tekintse át, alkalmazza, és állítsa vissza a teljesítmény fokozása javaslatok</span><span class="sxs-lookup"><span data-stu-id="cdfe3-153">Review, apply and revert performance improvement recommendations</span></span>
> * <span data-ttu-id="cdfe3-154">Az magas erőforrás-használat lekérdezések keresése</span><span class="sxs-lookup"><span data-stu-id="cdfe3-154">Find queries with high resource utilization</span></span>
> * <span data-ttu-id="cdfe3-155">Hosszú ideig futó lekérdezések keresése</span><span class="sxs-lookup"><span data-stu-id="cdfe3-155">Find long running queries</span></span>

[<span data-ttu-id="cdfe3-156">SQL-adatbázis teljesítményének hangolási tippek</span><span class="sxs-lookup"><span data-stu-id="cdfe3-156">SQL Database performance tuning tips</span></span>](https://docs.microsoft.com/azure/sql-database/sql-database-troubleshoot-performance)
