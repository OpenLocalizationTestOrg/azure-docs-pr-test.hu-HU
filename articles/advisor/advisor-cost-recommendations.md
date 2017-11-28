---
title: "aaaAzure Advisor költség javaslatok |} Microsoft Docs"
description: "Használja az Azure Advisor toooptimize hello költségét az Azure-környezetekhez."
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
ms.openlocfilehash: 50f70c33a17f550c8753795435cdfddd51e409f7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="advisor-cost-recommendations"></a><span data-ttu-id="19da4-103">Költség javaslatokat biztosít</span><span class="sxs-lookup"><span data-stu-id="19da4-103">Advisor Cost recommendations</span></span>

<span data-ttu-id="19da4-104">Az Advisor segítségével optimalizálhatja, és csökkentheti a teljes Azure töltött túl magas és üresjárati erőforrások azonosításával.</span><span class="sxs-lookup"><span data-stu-id="19da4-104">Advisor helps you optimize and reduce your overall Azure spend by identifying idle and underutilized resources.</span></span> <span data-ttu-id="19da4-105">Akkor is beolvasása költség hello javaslatokat **költség** hello Advisor irányítópult fület.</span><span class="sxs-lookup"><span data-stu-id="19da4-105">You can get cost recommendations from hello **Cost** tab on hello Advisor dashboard.</span></span>

![Az Advisor költség lap](./media/advisor-cost-recommendations/advisor-cost-tab2.png)

## <a name="optimize-virtual-machine-spend-by-resizing-underutilized-instances"></a><span data-ttu-id="19da4-107">Optimalizálhatja a túl példányok átméretezésével töltött virtuális gép</span><span class="sxs-lookup"><span data-stu-id="19da4-107">Optimize virtual machine spend by resizing underutilized instances</span></span> 
<span data-ttu-id="19da4-108">Bár egyes alkalmazás-forgatókönyvekre eredményezhet alacsony kihasználtságú úgy lett kialakítva, gyakran spórolhat a pénz hello méretét és számát a virtuális gépek kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="19da4-108">Although certain application scenarios can result in low utilization by design, you can often save money by managing hello size and number of your virtual machines.</span></span> <span data-ttu-id="19da4-109">Az Advisor figyeli a virtuális gépek használatának 14 napja, és majd azonosítja a kis-kihasználtságának virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="19da4-109">Advisor monitors your virtual machine usage for 14 days and then identifies low-utilization virtual machines.</span></span> <span data-ttu-id="19da4-110">Virtuális gépek, amelyek CPU-felhasználás csak 5 % vagy kevesebb és hálózati aktivitás a 7 MB vagy kisebb a négy vagy több napot minősülnek alacsony használt virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="19da4-110">Virtual machines whose CPU utilization is 5 percent or less and network usage is 7 MB or less for four or more days are considered low-utilization virtual machines.</span></span>

<span data-ttu-id="19da4-111">Az Advisor toorun a virtuális gépet, a Folytatás becsült költségét hello, így kiválaszthatja azt le, illetve átméretezheti tooshut jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="19da4-111">Advisor shows you hello estimated cost of continuing toorun your virtual machine, so that you can choose tooshut it down or resize it.</span></span>  

![Az Advisor költségeket, virtuális gépek átméretezésével kapcsolatos ajánlások](./media/advisor-cost-recommendations/advisor-cost-resizevms.png)

## <a name="use-a-cost-effective-solution-toomanage-performance-goals-of-multiple-sql-databases"></a><span data-ttu-id="19da4-113">Használjon egy költséghatékony megoldás toomanage teljesítményértékeket több SQL-adatbázisok</span><span class="sxs-lookup"><span data-stu-id="19da4-113">Use a cost effective solution toomanage performance goals of multiple SQL databases</span></span>
<span data-ttu-id="19da4-114">Az Advisor azonosítja az SQL server-példányokat, amelyek kihasználhatják a rugalmas adatbáziskészlet létrehozása.</span><span class="sxs-lookup"><span data-stu-id="19da4-114">Advisor identifies SQL server instances that can benefit from creating elastic database pools.</span></span> <span data-ttu-id="19da4-115">A rugalmas adatbáziskészletek egy egyszerű és költséghatékony megoldást toomanage hello teljesítményértékeket több adatbázisok esetén, amelyek különböző használati szokásokról, adja meg.</span><span class="sxs-lookup"><span data-stu-id="19da4-115">Elastic database pools provide a simple, cost-effective solution toomanage hello performance goals of multiple databases that have varying usage patterns.</span></span> <span data-ttu-id="19da4-116">Az Azure rugalmas készletek kapcsolatos további információkért lásd: [Mi az Azure rugalmas készletek?](https://azure.microsoft.com/en-us/documentation/articles/sql-database-elastic-pool/).</span><span class="sxs-lookup"><span data-stu-id="19da4-116">For more information about Azure elastic pools, see [What is an Azure Elastic pool?](https://azure.microsoft.com/en-us/documentation/articles/sql-database-elastic-pool/).</span></span>

![Az Advisor rugalmas adatbáziskészletek javaslatok költsége](./media/advisor-cost-recommendations/advisor-cost-elasticdbpools.png)

## <a name="how-tooaccess-cost-recommendations-in-azure-advisor"></a><span data-ttu-id="19da4-118">Hogyan tooaccess költség javaslatok az Azure-tanácsadó</span><span class="sxs-lookup"><span data-stu-id="19da4-118">How tooaccess cost recommendations in Azure Advisor</span></span>

1. <span data-ttu-id="19da4-119">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="19da4-119">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="19da4-120">Hello bal oldali ablaktáblában kattintson **további szolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="19da4-120">In hello left pane, click **More services**.</span></span>

3. <span data-ttu-id="19da4-121">A hello szolgáltatás menü ablaktáblán, a **figyelés és felügyelet**, kattintson a **Azure Advisor**.</span><span class="sxs-lookup"><span data-stu-id="19da4-121">In hello service menu pane, under **Monitoring and Management**, click **Azure Advisor**.</span></span>  
 <span data-ttu-id="19da4-122">hello Advisor irányítópult jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="19da4-122">hello Advisor dashboard is displayed.</span></span>

4. <span data-ttu-id="19da4-123">Az hello Advisor irányítópultján kattintson a hello **költség** fülre.</span><span class="sxs-lookup"><span data-stu-id="19da4-123">On hello Advisor dashboard, click hello **Cost** tab.</span></span>

5. <span data-ttu-id="19da4-124">Válassza ki hello előfizetést, amelyhez szeretné, hogy tooreceive javaslatokat, és kattintson **javaslatok beszerzése**.</span><span class="sxs-lookup"><span data-stu-id="19da4-124">Select hello subscription for which you want tooreceive recommendations, and then click **Get recommendations**.</span></span>

> [!NOTE]
> <span data-ttu-id="19da4-125">Advisor-javaslatokra tooaccess, először *az előfizetés regisztrálása* az Advisor szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="19da4-125">tooaccess Advisor recommendations, you must first *register your subscription* with Advisor.</span></span> <span data-ttu-id="19da4-126">Egy előfizetés regisztrálva amikor egy *előfizetés tulajdonosának* elindítja az Advisor-irányítópult és az kattintással hello hello **javaslatok beszerzése** gombra.</span><span class="sxs-lookup"><span data-stu-id="19da4-126">A subscription is registered when a *subscription Owner* launches hello Advisor dashboard and clicks hello **Get recommendations** button.</span></span> <span data-ttu-id="19da4-127">Ez egy *egyszeri művelet*.</span><span class="sxs-lookup"><span data-stu-id="19da4-127">This is a *one-time operation*.</span></span> <span data-ttu-id="19da4-128">Hello előfizetés regisztrálása után érheti el, az Advisor-javaslatokra *tulajdonos*, *közreműködő*, vagy *olvasó* -előfizetéssel, egy erőforráscsoport vagy egy adott erőforrás.</span><span class="sxs-lookup"><span data-stu-id="19da4-128">After hello subscription is registered, you can access Advisor recommendations as *Owner*, *Contributor*, or *Reader* for a subscription, a resource group, or a specific resource.</span></span>

## <a name="next-steps"></a><span data-ttu-id="19da4-129">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="19da4-129">Next steps</span></span>

<span data-ttu-id="19da4-130">További információ az Advisor-javaslatokra toolearn lásd:</span><span class="sxs-lookup"><span data-stu-id="19da4-130">toolearn more about Advisor recommendations, see:</span></span>
* [<span data-ttu-id="19da4-131">Bevezetés tooAdvisor</span><span class="sxs-lookup"><span data-stu-id="19da4-131">Introduction tooAdvisor</span></span>](advisor-overview.md)
* [<span data-ttu-id="19da4-132">Első lépések</span><span class="sxs-lookup"><span data-stu-id="19da4-132">Get Started</span></span>](advisor-get-started.md)
* [<span data-ttu-id="19da4-133">Teljesítmény javaslatokat biztosít</span><span class="sxs-lookup"><span data-stu-id="19da4-133">Advisor Performance recommendations</span></span>](advisor-cost-recommendations.md)
* [<span data-ttu-id="19da4-134">Magas rendelkezésre állású javaslatokat biztosít</span><span class="sxs-lookup"><span data-stu-id="19da4-134">Advisor High Availability recommendations</span></span>](advisor-cost-recommendations.md)
* [<span data-ttu-id="19da4-135">Biztonsági javaslatokat biztosít</span><span class="sxs-lookup"><span data-stu-id="19da4-135">Advisor Security recommendations</span></span>](advisor-cost-recommendations.md)
