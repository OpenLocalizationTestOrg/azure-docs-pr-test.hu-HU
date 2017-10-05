---
title: "Az Azure Advisor teljesítmény javaslatok |} Microsoft Docs"
description: "Az Advisor segítségével az Azure-környezetekhez teljesítményének optimalizálásához."
services: advisor
documentationcenter: NA
author: kumudd
manager: carmonm
editor: 
ms.assetid: 
ms.service: advisor
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/16/2016
ms.author: kumud
ms.openlocfilehash: 5fb86c60b2d1f258dde5636ff8854b6f30f7f1c8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="advisor-performance-recommendations"></a><span data-ttu-id="aa34c-103">Teljesítmény javaslatokat biztosít</span><span class="sxs-lookup"><span data-stu-id="aa34c-103">Advisor Performance recommendations</span></span>

<span data-ttu-id="aa34c-104">Az Azure teljesítménye javaslatokat biztosít a sebesség és az üzleti szempontból kritikus fontosságú alkalmazások válaszképességének javítása érdekében.</span><span class="sxs-lookup"><span data-stu-id="aa34c-104">Azure Advisor performance recommendations help improve the speed and responsiveness of your business-critical applications.</span></span> <span data-ttu-id="aa34c-105">Letölthető teljesítmény javaslatok Advisor a **teljesítmény** az Advisor irányítópult.</span><span class="sxs-lookup"><span data-stu-id="aa34c-105">You can get performance recommendations from Advisor on the **Performance** tab of the Advisor dashboard.</span></span>

![Az Advisor teljesítmény lap](./media/advisor-performance-recommendations/advisor-performance-tab.png)

## <a name="improve-database-performance-with-sql-db-advisor"></a><span data-ttu-id="aa34c-107">Az SQL DB Advisor adatbázis teljesítményének növelése</span><span class="sxs-lookup"><span data-stu-id="aa34c-107">Improve database performance with SQL DB Advisor</span></span>

<span data-ttu-id="aa34c-108">Az Advisor egy egységes, összevont nézetének összes Azure-erőforrások javaslatokat biztosít.</span><span class="sxs-lookup"><span data-stu-id="aa34c-108">Advisor provides you with a consistent, consolidated view of recommendations for all your Azure resources.</span></span> <span data-ttu-id="aa34c-109">SQL Database Advisor lapján kapcsolja a javaslatok az SQL Azure adatbázis teljesítményének növelése integrálható.</span><span class="sxs-lookup"><span data-stu-id="aa34c-109">It integrates with SQL Database Advisor to bring you recommendations for improving the performance of your SQL Azure database.</span></span> <span data-ttu-id="aa34c-110">SQL Database Advisor segédprogramot a használati előzmények elemzésével az SQL Azure-adatbázisok teljesítményét értékeli.</span><span class="sxs-lookup"><span data-stu-id="aa34c-110">SQL Database Advisor assesses the performance of your SQL Azure databases by analyzing your usage history.</span></span> <span data-ttu-id="aa34c-111">Lehetőséget kínál a javaslatok, amelyek az adatbázis átlagos munkaterhelésre fut a legalkalmasabb.</span><span class="sxs-lookup"><span data-stu-id="aa34c-111">It then offers recommendations that are best suited for running the database’s typical workload.</span></span> 

> [!NOTE]
> <span data-ttu-id="aa34c-112">Ahhoz, hogy a javaslatok, egy adatbázisnak rendelkeznie kell egy hét használati kapcsolatos, és a hét belül kell konzisztens tevékenységet észleltünk a fiókjában.</span><span class="sxs-lookup"><span data-stu-id="aa34c-112">To get recommendations, a database must have about a week of usage, and within that week there must be some consistent activity.</span></span> <span data-ttu-id="aa34c-113">SQL Database Advisor segédprogramot a lekérdezés konzisztens mintára mint a véletlenszerű felszakadásáig tevékenység könnyebben optimalizálható.</span><span class="sxs-lookup"><span data-stu-id="aa34c-113">SQL Database Advisor can optimize more easily for consistent query patterns than for random bursts of activity.</span></span>

<span data-ttu-id="aa34c-114">SQL Database Advisor kapcsolatos további információkért lásd: [SQL Database Advisor](https://azure.microsoft.com/en-us/documentation/articles/sql-database-advisor/).</span><span class="sxs-lookup"><span data-stu-id="aa34c-114">For more information about SQL Database Advisor, see [SQL Database Advisor](https://azure.microsoft.com/en-us/documentation/articles/sql-database-advisor/).</span></span>

![SQL-adatbázissal kapcsolatos javaslatok](./media/advisor-performance-recommendations/advisor-performance-sql.png)

## <a name="improve-redis-cache-performance-and-reliability"></a><span data-ttu-id="aa34c-116">Redis gyorsítótár teljesítményének és megbízhatóságának növelése</span><span class="sxs-lookup"><span data-stu-id="aa34c-116">Improve Redis Cache performance and reliability</span></span>

<span data-ttu-id="aa34c-117">Az Advisor azonosítja a Redis Cache példány ahol teljesítményt hátrányosan érintheti által magas memóriahasználat, a kiszolgáló terhelését, a hálózati sávszélesség vagy a nagyszámú ügyfél kapcsolatok.</span><span class="sxs-lookup"><span data-stu-id="aa34c-117">Advisor identifies Redis Cache instances where performance may be adversely affected by high memory usage, server load, network bandwidth, or a large number of client connections.</span></span> <span data-ttu-id="aa34c-118">Az Advisor is gyakorlati tanácsokat ajánlásokat lehetséges problémák elkerülése érdekében.</span><span class="sxs-lookup"><span data-stu-id="aa34c-118">Advisor also provides best practices recommendations to help you avoid potential issues.</span></span> <span data-ttu-id="aa34c-119">További információ a Redis Cache javaslatok: [Redis gyorsítótár Advisor](https://azure.microsoft.com/en-us/documentation/articles/cache-configure/#redis-cache-advisor).</span><span class="sxs-lookup"><span data-stu-id="aa34c-119">For more information about Redis Cache recommendations, see [Redis Cache Advisor](https://azure.microsoft.com/en-us/documentation/articles/cache-configure/#redis-cache-advisor).</span></span>


## <a name="improve-app-service-performance-and-reliability"></a><span data-ttu-id="aa34c-120">App Service-teljesítmény és megbízhatóság javítása</span><span class="sxs-lookup"><span data-stu-id="aa34c-120">Improve App Service performance and reliability</span></span>

<span data-ttu-id="aa34c-121">Azure Advisor integrálja a alkalmazásszolgáltatások felhasználói élmény javítása és felderítésére vonatkozó platform képességei ajánlott eljárásait.</span><span class="sxs-lookup"><span data-stu-id="aa34c-121">Azure Advisor integrates best practices recommendations for improving your App Services experience and discovering relevant platform capabilities.</span></span> <span data-ttu-id="aa34c-122">Például App Services ajánlások a következők:</span><span class="sxs-lookup"><span data-stu-id="aa34c-122">Examples of App Services recommendations are:</span></span>
* <span data-ttu-id="aa34c-123">Ahol memória vagy a Processzor-erőforrások elfogytak a megoldás beállítások app futtatókörnyezetek által példányok észlelése.</span><span class="sxs-lookup"><span data-stu-id="aa34c-123">Detection of instances where memory or CPU resources are exhausted by app runtimes with mitigation options.</span></span>
* <span data-ttu-id="aa34c-124">Ahol helymegosztást erőforrások – például webes alkalmazásokat és adatbázisokat példányok észlelésének javíthatja a teljesítményt és az alacsonyabb költségek.</span><span class="sxs-lookup"><span data-stu-id="aa34c-124">Detection of instances where collocating resources like web apps and databases can improve performance and lower cost.</span></span> 

<span data-ttu-id="aa34c-125">App Service szolgáltatások javaslatok kapcsolatos további információkért lásd: [ajánlott eljárások az Azure App Service](https://azure.microsoft.com/en-us/documentation/articles/app-service-best-practices/).</span><span class="sxs-lookup"><span data-stu-id="aa34c-125">For more information about App Services recommendations, see [Best Practices for Azure App Service](https://azure.microsoft.com/en-us/documentation/articles/app-service-best-practices/).</span></span>
<span data-ttu-id="aa34c-126">![App Services javaslatok](./media/advisor-performance-recommendations/advisor-performance-app-service.png)</span><span class="sxs-lookup"><span data-stu-id="aa34c-126">![App Services recommendations](./media/advisor-performance-recommendations/advisor-performance-app-service.png)</span></span>

## <a name="how-to-access-performance-recommendations-in-advisor"></a><span data-ttu-id="aa34c-127">Teljesítmény javaslatok az Advisor elérése</span><span class="sxs-lookup"><span data-stu-id="aa34c-127">How to access Performance recommendations in Advisor</span></span>

1. <span data-ttu-id="aa34c-128">Jelentkezzen be az [Azure Portalra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="aa34c-128">Sign in to the [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="aa34c-129">Kattintson a bal oldali ablaktáblában **további szolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="aa34c-129">In the left pane, click **More services**.</span></span>

3. <span data-ttu-id="aa34c-130">A szolgáltatás menü ablaktáblán alatt **figyelés és felügyelet**, kattintson a **Azure Advisor**.</span><span class="sxs-lookup"><span data-stu-id="aa34c-130">In the service menu pane, under **Monitoring and Management**, click **Azure Advisor**.</span></span>  
 <span data-ttu-id="aa34c-131">Az Advisor irányítópult jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="aa34c-131">The Advisor dashboard is displayed.</span></span>

4. <span data-ttu-id="aa34c-132">Az Advisor irányítópultján kattintson a **teljesítmény** fülre.</span><span class="sxs-lookup"><span data-stu-id="aa34c-132">On the Advisor dashboard, click the **Performance** tab.</span></span>

5. <span data-ttu-id="aa34c-133">Válassza ki az előfizetést, amely javaslatokat kap, és kattintson a kívánt **javaslatok beszerzése**.</span><span class="sxs-lookup"><span data-stu-id="aa34c-133">Select the subscription for which you want to receive recommendations, and then click **Get recommendations**.</span></span>

> [!NOTE]
> <span data-ttu-id="aa34c-134">Advisor-javaslatokra szeretne használni, először *az előfizetés regisztrálása* az Advisor szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="aa34c-134">To access Advisor recommendations, you must first *register your subscription* with Advisor.</span></span> <span data-ttu-id="aa34c-135">Egy előfizetés regisztrálva amikor egy *előfizetés tulajdonosának* elindítja az Advisor irányítópulton, és rákattint a **javaslatok beszerzése** gombra.</span><span class="sxs-lookup"><span data-stu-id="aa34c-135">A subscription is registered when a *subscription Owner* launches the Advisor dashboard and clicks the **Get recommendations** button.</span></span> <span data-ttu-id="aa34c-136">Ez egy *egyszeri művelet*.</span><span class="sxs-lookup"><span data-stu-id="aa34c-136">This is a *one-time operation*.</span></span> <span data-ttu-id="aa34c-137">Az előfizetés regisztrálása után érheti el, az Advisor-javaslatokra *tulajdonos*, *közreműködő*, vagy *olvasó* előfizetés, egy erőforráscsoport vagy egy adott erőforrás.</span><span class="sxs-lookup"><span data-stu-id="aa34c-137">After the subscription is registered, you can access Advisor recommendations as *Owner*, *Contributor*, or *Reader* for a subscription, a resource group, or a specific resource.</span></span>

## <a name="next-steps"></a><span data-ttu-id="aa34c-138">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="aa34c-138">Next steps</span></span>

<span data-ttu-id="aa34c-139">Az Advisor-javaslatokra kapcsolatos további információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="aa34c-139">To learn more about Advisor recommendations, see:</span></span>

* [<span data-ttu-id="aa34c-140">Az Advisor bemutatása</span><span class="sxs-lookup"><span data-stu-id="aa34c-140">Introduction to Advisor</span></span>](advisor-overview.md)
* [<span data-ttu-id="aa34c-141">Bevezetés az Advisor használatába</span><span class="sxs-lookup"><span data-stu-id="aa34c-141">Get started with Advisor</span></span>](advisor-get-started.md)
* [<span data-ttu-id="aa34c-142">Költség javaslatokat biztosít</span><span class="sxs-lookup"><span data-stu-id="aa34c-142">Advisor Cost recommendations</span></span>](advisor-performance-recommendations.md)
* [<span data-ttu-id="aa34c-143">Magas rendelkezésre állású javaslatokat biztosít</span><span class="sxs-lookup"><span data-stu-id="aa34c-143">Advisor High Availability recommendations</span></span>](advisor-high-availability-recommendations.md)
* [<span data-ttu-id="aa34c-144">Biztonsági javaslatokat biztosít</span><span class="sxs-lookup"><span data-stu-id="aa34c-144">Advisor Security recommendations</span></span>](advisor-security-recommendations.md)

