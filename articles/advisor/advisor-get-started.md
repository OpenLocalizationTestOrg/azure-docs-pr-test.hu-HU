---
title: "Ismerkedés az Azure Advisor |} Microsoft Docs"
description: "Ismerkedés az Azure Advisor."
services: advisor
documentationcenter: NA
author: manbeenkohli
manager: carmonm
editor: 
ms.assetid: 
ms.service: advisor
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/10/2017
ms.author: makohli
ms.openlocfilehash: a662841bebda460d4225e080f16705b3f16fdc46
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-azure-advisor"></a><span data-ttu-id="e198d-103">Ismerkedés az Azure Advisor szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="e198d-103">Get started with Azure Advisor</span></span>

<span data-ttu-id="e198d-104">Útmutató az Azure-portálon keresztül férnek hozzá az Advisor, javaslatok beszerzése, ajánlások megvalósításával, keresse meg a javaslatok, illetve a frissítést.</span><span class="sxs-lookup"><span data-stu-id="e198d-104">Learn how to access Advisor through the Azure portal, get recommendations, implement recommendations, search for recommendations, and refresh recommendations.</span></span>

## <a name="get-advisor-recommendations"></a><span data-ttu-id="e198d-105">Javaslatok az Advisor használatával kapcsolatban</span><span class="sxs-lookup"><span data-stu-id="e198d-105">Get Advisor recommendations</span></span>

1. <span data-ttu-id="e198d-106">Jelentkezzen be az [Azure Portalra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="e198d-106">Sign in to the [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="e198d-107">Kattintson a bal oldali ablaktáblában **további szolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="e198d-107">In the left pane, click **More services**.</span></span>

3. <span data-ttu-id="e198d-108">A szolgáltatás menü ablaktáblán alatt **figyelés és felügyelet**, kattintson a **Azure Advisor**.</span><span class="sxs-lookup"><span data-stu-id="e198d-108">In the service menu pane, under **Monitoring and Management**, click **Azure Advisor**.</span></span>  
 <span data-ttu-id="e198d-109">Az Advisor irányítópult jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="e198d-109">The Advisor dashboard is displayed.</span></span>

   ![Hozzáférés az Azure Advisor az Azure portál használatával](./media/advisor-overview/advisor-azure-portal-menu.png) 

4. <span data-ttu-id="e198d-111">Az Advisor irányítópultra válassza ki a javaslatok fogadni kívánt előfizetést.</span><span class="sxs-lookup"><span data-stu-id="e198d-111">On the Advisor dashboard, select the subscription for which you want to receive recommendations.</span></span>  
<span data-ttu-id="e198d-112">Az Advisor irányítópulton megjelenített személyre szabott javaslatok a kiválasztott előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="e198d-112">The Advisor dashboard displays personalized recommendations for a selected subscription.</span></span> 

5. <span data-ttu-id="e198d-113">Ahhoz, hogy egy adott kategória javaslatok, kattintson a lapfülek egyikére: **magas rendelkezésre állású**, **biztonsági**, **teljesítmény**, vagy **költség**.</span><span class="sxs-lookup"><span data-stu-id="e198d-113">To get recommendations for a particular category, click one of the tabs: **High Availability**, **Security**, **Performance**, or **Cost**.</span></span>
 
> [!NOTE]
> <span data-ttu-id="e198d-114">Advisor-javaslatokra szeretne használni, először *az előfizetés regisztrálása* az Advisor szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="e198d-114">To access Advisor recommendations, you must first *register your subscription* with Advisor.</span></span> <span data-ttu-id="e198d-115">Egy előfizetés regisztrálva amikor egy *előfizetés tulajdonosának* elindítja az Advisor irányítópulton, és rákattint a **javaslatok beszerzése** gombra.</span><span class="sxs-lookup"><span data-stu-id="e198d-115">A subscription is registered when a *subscription Owner* launches the Advisor dashboard and clicks the **Get recommendations** button.</span></span> <span data-ttu-id="e198d-116">Ez egy *egyszeri művelet*.</span><span class="sxs-lookup"><span data-stu-id="e198d-116">This is a *one-time operation*.</span></span> <span data-ttu-id="e198d-117">Az előfizetés regisztrálása után érheti el, az Advisor-javaslatokra *tulajdonos*, *közreműködő*, vagy *olvasó* előfizetés, egy erőforráscsoport vagy egy adott erőforrás.</span><span class="sxs-lookup"><span data-stu-id="e198d-117">After the subscription is registered, you can access Advisor recommendations as *Owner*, *Contributor*, or *Reader* for a subscription, a resource group, or a specific resource.</span></span>

  ![Az Azure Advisor irányítópult](./media/advisor-overview/advisor-all-tab.png)

## <a name="get-advisor-recommendation-details-and-implement-a-solution"></a><span data-ttu-id="e198d-119">Az Advisor javaslat részletes, és egy megoldás bevezetésére</span><span class="sxs-lookup"><span data-stu-id="e198d-119">Get Advisor recommendation details, and implement a solution</span></span>

<span data-ttu-id="e198d-120">A **javaslat** panel az Advisor nyújt további információt a javaslat.</span><span class="sxs-lookup"><span data-stu-id="e198d-120">The **Recommendation** blade in Advisor offers additional information about the recommendation.</span></span> 

1. <span data-ttu-id="e198d-121">Jelentkezzen be a [Azure-portálon](https://portal.azure.com), majd indítsa el a [Azure Advisor](https://aka.ms/azureadvisordashboard).</span><span class="sxs-lookup"><span data-stu-id="e198d-121">Sign in to the [Azure portal](https://portal.azure.com), and then start [Azure Advisor](https://aka.ms/azureadvisordashboard).</span></span>

2. <span data-ttu-id="e198d-122">Az a **Advisor-javaslatokra** irányítópultján kattintson **javaslatok beszerzése**.</span><span class="sxs-lookup"><span data-stu-id="e198d-122">On the **Advisor recommendations** dashboard, click **Get recommendations**.</span></span>

3. <span data-ttu-id="e198d-123">A javaslatok listájának megtekintéséhez kattintson a azt ajánljuk, hogy meg szeretné tekinteni részletesen.</span><span class="sxs-lookup"><span data-stu-id="e198d-123">In the list of recommendations, click a recommendation that you want to review in detail.</span></span>  
<span data-ttu-id="e198d-124">A **javaslat** panel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="e198d-124">The **Recommendation** blade is displayed.</span></span>

4. <span data-ttu-id="e198d-125">Az a **javaslatok** panelt, tekintse át adatokat azokról a műveletekről, hogy akkor is lehetséges probléma megoldása érdekében hajtsa végre, vagy egy költségkímélő lehetőség előnyeit.</span><span class="sxs-lookup"><span data-stu-id="e198d-125">On the **Recommendations** blade, review information about actions that you can perform to resolve a potential issue, or take advantage of a cost-saving opportunity.</span></span> 
  
  ![Az Advisor indexjavaslat panelen](./media/advisor-overview/advisor-recommendation-action-example.png)

## <a name="search-for-advisor-recommendations"></a><span data-ttu-id="e198d-127">Keresés az Advisor-javaslatokra</span><span class="sxs-lookup"><span data-stu-id="e198d-127">Search for Advisor recommendations</span></span>

<span data-ttu-id="e198d-128">Kereshet egy adott előfizetés vagy az erőforrás csoport javaslatok.</span><span class="sxs-lookup"><span data-stu-id="e198d-128">You can search for recommendations for a particular subscription or resource group.</span></span> <span data-ttu-id="e198d-129">Is kereshet javaslatok állapot szerint.</span><span class="sxs-lookup"><span data-stu-id="e198d-129">You can also search for recommendations by status.</span></span>

1. <span data-ttu-id="e198d-130">Jelentkezzen be a [Azure-portálon](https://portal.azure.com), majd indítsa el a [Azure Advisor](https://aka.ms/azureadvisordashboard).</span><span class="sxs-lookup"><span data-stu-id="e198d-130">Sign in to the [Azure portal](https://portal.azure.com), and then start [Azure Advisor](https://aka.ms/azureadvisordashboard).</span></span>

2. <span data-ttu-id="e198d-131">Az előfizetések, erőforráscsoport-sablonok és javaslat állapot szűréssel javaslatokra keresési (**aktív** vagy **Snoozed**).</span><span class="sxs-lookup"><span data-stu-id="e198d-131">Search for recommendations by filtering for subscriptions, resource groups, and recommendation status (**Active** or **Snoozed**).</span></span>

3. <span data-ttu-id="e198d-132">A keresés-szűrési feltételeknek megfelelő Advisor-javaslatokra listájának megjelenítéséhez kattintson **javaslatok beszerzése**.</span><span class="sxs-lookup"><span data-stu-id="e198d-132">To display a list of Advisor recommendations that are based on your search-filter criteria, click **Get recommendations**.</span></span>

  ![Az Advisor keresési-szűrési feltételeket](./media/advisor-get-started/advisor-search.png)

## <a name="snooze-or-dismiss-advisor-recommendations"></a><span data-ttu-id="e198d-134">Emlékeztet, vagy hagyja figyelmen kívül az Advisor-javaslatokra</span><span class="sxs-lookup"><span data-stu-id="e198d-134">Snooze or dismiss Advisor recommendations</span></span>

1. <span data-ttu-id="e198d-135">Jelentkezzen be a [Azure-portálon](https://portal.azure.com), majd indítsa el a [Azure Advisor](https://aka.ms/azureadvisordashboard).</span><span class="sxs-lookup"><span data-stu-id="e198d-135">Sign in to the [Azure portal](https://portal.azure.com), and then start [Azure Advisor](https://aka.ms/azureadvisordashboard).</span></span>

2. <span data-ttu-id="e198d-136">Kattintson a **javaslatok beszerzése**, és a javaslatok listájának megtekintéséhez kattintson egy javaslat.</span><span class="sxs-lookup"><span data-stu-id="e198d-136">Click **Get recommendations**, and then, in the list of recommendations, click a recommendation.</span></span>

3. <span data-ttu-id="e198d-137">Az a **javaslat** panelen kattintson a **emlékeztet**.</span><span class="sxs-lookup"><span data-stu-id="e198d-137">On the **Recommendation** blade, click **Snooze**.</span></span>  

   ![Az Advisor javasolt művelet – példa](./media/advisor-get-started/advisor-snooze.png)

4. <span data-ttu-id="e198d-139">Adjon meg egy emlékeztető időszakra vonatkozóan, vagy válasszon **soha** elvetni a javaslat.</span><span class="sxs-lookup"><span data-stu-id="e198d-139">Specify a snooze time period, or select **Never** to dismiss the recommendation.</span></span>


## <a name="next-steps"></a><span data-ttu-id="e198d-140">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e198d-140">Next steps</span></span>

<span data-ttu-id="e198d-141">Az Advisor kapcsolatos további információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="e198d-141">To learn more about Advisor, see:</span></span>
* [<span data-ttu-id="e198d-142">Bevezetés az Azure Advisor</span><span class="sxs-lookup"><span data-stu-id="e198d-142">Introduction to Azure Advisor</span></span>](advisor-overview.md)
* [<span data-ttu-id="e198d-143">Magas rendelkezésre állású javaslatokat biztosít</span><span class="sxs-lookup"><span data-stu-id="e198d-143">Advisor High Availability recommendations</span></span>](advisor-high-availability-recommendations.md)
* [<span data-ttu-id="e198d-144">Biztonsági javaslatokat biztosít</span><span class="sxs-lookup"><span data-stu-id="e198d-144">Advisor Security recommendations</span></span>](advisor-security-recommendations.md)
-  [<span data-ttu-id="e198d-145">Teljesítmény javaslatokat biztosít</span><span class="sxs-lookup"><span data-stu-id="e198d-145">Advisor Performance recommendations</span></span>](advisor-performance-recommendations.md)
* [<span data-ttu-id="e198d-146">Költség javaslatokat biztosít</span><span class="sxs-lookup"><span data-stu-id="e198d-146">Advisor Cost recommendations</span></span>](advisor-performance-recommendations.md)
