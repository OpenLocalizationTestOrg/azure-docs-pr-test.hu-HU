---
title: "Az Azure Advisor költség javaslatok |} Microsoft Docs"
description: "Azure Advisor segítségével optimalizálhatja az Azure-környezetekhez költségét."
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
ms.openlocfilehash: 5eef2116f238b477fa8de46ce7b25728c393739c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="advisor-cost-recommendations"></a><span data-ttu-id="00598-103">Költség javaslatokat biztosít</span><span class="sxs-lookup"><span data-stu-id="00598-103">Advisor Cost recommendations</span></span>

<span data-ttu-id="00598-104">Az Advisor segítségével optimalizálhatja, és csökkentheti a teljes Azure töltött túl magas és üresjárati erőforrások azonosításával.</span><span class="sxs-lookup"><span data-stu-id="00598-104">Advisor helps you optimize and reduce your overall Azure spend by identifying idle and underutilized resources.</span></span> <span data-ttu-id="00598-105">Akkor is beolvasása költség javaslatokat a **költség** az Advisor irányítópult fület.</span><span class="sxs-lookup"><span data-stu-id="00598-105">You can get cost recommendations from the **Cost** tab on the Advisor dashboard.</span></span>

![Az Advisor költség lap](./media/advisor-cost-recommendations/advisor-cost-tab2.png)

## <a name="optimize-virtual-machine-spend-by-resizing-underutilized-instances"></a><span data-ttu-id="00598-107">Optimalizálhatja a túl példányok átméretezésével töltött virtuális gép</span><span class="sxs-lookup"><span data-stu-id="00598-107">Optimize virtual machine spend by resizing underutilized instances</span></span> 
<span data-ttu-id="00598-108">Bár egyes alkalmazás-forgatókönyvekre eredményezhet alacsony kihasználtságú úgy lett kialakítva, gyakran mentheti pénz való kezelésekor a a virtuális gépek száma és mérete.</span><span class="sxs-lookup"><span data-stu-id="00598-108">Although certain application scenarios can result in low utilization by design, you can often save money by managing the size and number of your virtual machines.</span></span> <span data-ttu-id="00598-109">Az Advisor figyeli a virtuális gépek használatának 14 napja, és majd azonosítja a kis-kihasználtságának virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="00598-109">Advisor monitors your virtual machine usage for 14 days and then identifies low-utilization virtual machines.</span></span> <span data-ttu-id="00598-110">Virtuális gépek, amelyek CPU-felhasználás csak 5 % vagy kevesebb és hálózati aktivitás a 7 MB vagy kisebb a négy vagy több napot minősülnek alacsony használt virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="00598-110">Virtual machines whose CPU utilization is 5 percent or less and network usage is 7 MB or less for four or more days are considered low-utilization virtual machines.</span></span>

<span data-ttu-id="00598-111">Az Advisor jeleníti meg, hogy ha szeretné leállítani, vagy méretezze át a virtuális gép futtatásához Folytatás becsült költségét.</span><span class="sxs-lookup"><span data-stu-id="00598-111">Advisor shows you the estimated cost of continuing to run your virtual machine, so that you can choose to shut it down or resize it.</span></span>  

![Az Advisor költségeket, virtuális gépek átméretezésével kapcsolatos ajánlások](./media/advisor-cost-recommendations/advisor-cost-resizevms.png)

## <a name="use-a-cost-effective-solution-to-manage-performance-goals-of-multiple-sql-databases"></a><span data-ttu-id="00598-113">Költséghatékony megoldás segítségével Teljesítménycélok több SQL-adatbázisok kezelése</span><span class="sxs-lookup"><span data-stu-id="00598-113">Use a cost effective solution to manage performance goals of multiple SQL databases</span></span>
<span data-ttu-id="00598-114">Az Advisor azonosítja az SQL server-példányokat, amelyek kihasználhatják a rugalmas adatbáziskészlet létrehozása.</span><span class="sxs-lookup"><span data-stu-id="00598-114">Advisor identifies SQL server instances that can benefit from creating elastic database pools.</span></span> <span data-ttu-id="00598-115">A rugalmas adatbáziskészletek adja meg, amely egyszerű és költséghatékony megoldást teljesítményértékeket rendelkező, különböző használati minták több adatbázis kezelésére.</span><span class="sxs-lookup"><span data-stu-id="00598-115">Elastic database pools provide a simple, cost-effective solution to manage the performance goals of multiple databases that have varying usage patterns.</span></span> <span data-ttu-id="00598-116">Az Azure rugalmas készletek kapcsolatos további információkért lásd: [Mi az Azure rugalmas készletek?](https://azure.microsoft.com/en-us/documentation/articles/sql-database-elastic-pool/).</span><span class="sxs-lookup"><span data-stu-id="00598-116">For more information about Azure elastic pools, see [What is an Azure Elastic pool?](https://azure.microsoft.com/en-us/documentation/articles/sql-database-elastic-pool/).</span></span>

![Az Advisor rugalmas adatbáziskészletek javaslatok költsége](./media/advisor-cost-recommendations/advisor-cost-elasticdbpools.png)

## <a name="how-to-access-cost-recommendations-in-azure-advisor"></a><span data-ttu-id="00598-118">Költség javaslatok az Azure Advisor elérése</span><span class="sxs-lookup"><span data-stu-id="00598-118">How to access cost recommendations in Azure Advisor</span></span>

1. <span data-ttu-id="00598-119">Jelentkezzen be az [Azure Portalra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="00598-119">Sign in to the [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="00598-120">Kattintson a bal oldali ablaktáblában **további szolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="00598-120">In the left pane, click **More services**.</span></span>

3. <span data-ttu-id="00598-121">A szolgáltatás menü ablaktáblán alatt **figyelés és felügyelet**, kattintson a **Azure Advisor**.</span><span class="sxs-lookup"><span data-stu-id="00598-121">In the service menu pane, under **Monitoring and Management**, click **Azure Advisor**.</span></span>  
 <span data-ttu-id="00598-122">Az Advisor irányítópult jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="00598-122">The Advisor dashboard is displayed.</span></span>

4. <span data-ttu-id="00598-123">Az Advisor irányítópultján kattintson a **költség** fülre.</span><span class="sxs-lookup"><span data-stu-id="00598-123">On the Advisor dashboard, click the **Cost** tab.</span></span>

5. <span data-ttu-id="00598-124">Válassza ki az előfizetést, amely javaslatokat kap, és kattintson a kívánt **javaslatok beszerzése**.</span><span class="sxs-lookup"><span data-stu-id="00598-124">Select the subscription for which you want to receive recommendations, and then click **Get recommendations**.</span></span>

> [!NOTE]
> <span data-ttu-id="00598-125">Advisor-javaslatokra szeretne használni, először *az előfizetés regisztrálása* az Advisor szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="00598-125">To access Advisor recommendations, you must first *register your subscription* with Advisor.</span></span> <span data-ttu-id="00598-126">Egy előfizetés regisztrálva amikor egy *előfizetés tulajdonosának* elindítja az Advisor irányítópulton, és rákattint a **javaslatok beszerzése** gombra.</span><span class="sxs-lookup"><span data-stu-id="00598-126">A subscription is registered when a *subscription Owner* launches the Advisor dashboard and clicks the **Get recommendations** button.</span></span> <span data-ttu-id="00598-127">Ez egy *egyszeri művelet*.</span><span class="sxs-lookup"><span data-stu-id="00598-127">This is a *one-time operation*.</span></span> <span data-ttu-id="00598-128">Az előfizetés regisztrálása után érheti el, az Advisor-javaslatokra *tulajdonos*, *közreműködő*, vagy *olvasó* előfizetés, egy erőforráscsoport vagy egy adott erőforrás.</span><span class="sxs-lookup"><span data-stu-id="00598-128">After the subscription is registered, you can access Advisor recommendations as *Owner*, *Contributor*, or *Reader* for a subscription, a resource group, or a specific resource.</span></span>

## <a name="next-steps"></a><span data-ttu-id="00598-129">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="00598-129">Next steps</span></span>

<span data-ttu-id="00598-130">Az Advisor-javaslatokra kapcsolatos további információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="00598-130">To learn more about Advisor recommendations, see:</span></span>
* [<span data-ttu-id="00598-131">Az Advisor bemutatása</span><span class="sxs-lookup"><span data-stu-id="00598-131">Introduction to Advisor</span></span>](advisor-overview.md)
* [<span data-ttu-id="00598-132">Első lépések</span><span class="sxs-lookup"><span data-stu-id="00598-132">Get Started</span></span>](advisor-get-started.md)
* [<span data-ttu-id="00598-133">Teljesítmény javaslatokat biztosít</span><span class="sxs-lookup"><span data-stu-id="00598-133">Advisor Performance recommendations</span></span>](advisor-cost-recommendations.md)
* [<span data-ttu-id="00598-134">Magas rendelkezésre állású javaslatokat biztosít</span><span class="sxs-lookup"><span data-stu-id="00598-134">Advisor High Availability recommendations</span></span>](advisor-cost-recommendations.md)
* [<span data-ttu-id="00598-135">Biztonsági javaslatokat biztosít</span><span class="sxs-lookup"><span data-stu-id="00598-135">Advisor Security recommendations</span></span>](advisor-cost-recommendations.md)
