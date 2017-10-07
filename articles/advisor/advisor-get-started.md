---
title: "aaaGet Azure Advisor használatába |} Microsoft Docs"
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
ms.openlocfilehash: 30fc8b8f3823f6f047e46cb9000189f3ccb3d514
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-advisor"></a><span data-ttu-id="bcb16-103">Ismerkedés az Azure Advisor szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="bcb16-103">Get started with Azure Advisor</span></span>

<span data-ttu-id="bcb16-104">Ismerje meg, hogyan tooaccess Advisor keresztül hello Azure-portálon get javaslatok ajánlások megvalósításával, keresse meg a javaslatok, illetve a frissítést.</span><span class="sxs-lookup"><span data-stu-id="bcb16-104">Learn how tooaccess Advisor through hello Azure portal, get recommendations, implement recommendations, search for recommendations, and refresh recommendations.</span></span>

## <a name="get-advisor-recommendations"></a><span data-ttu-id="bcb16-105">Javaslatok az Advisor használatával kapcsolatban</span><span class="sxs-lookup"><span data-stu-id="bcb16-105">Get Advisor recommendations</span></span>

1. <span data-ttu-id="bcb16-106">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="bcb16-106">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="bcb16-107">Hello bal oldali ablaktáblában kattintson **további szolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="bcb16-107">In hello left pane, click **More services**.</span></span>

3. <span data-ttu-id="bcb16-108">A hello szolgáltatás menü ablaktáblán, a **figyelés és felügyelet**, kattintson a **Azure Advisor**.</span><span class="sxs-lookup"><span data-stu-id="bcb16-108">In hello service menu pane, under **Monitoring and Management**, click **Azure Advisor**.</span></span>  
 <span data-ttu-id="bcb16-109">hello Advisor irányítópult jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="bcb16-109">hello Advisor dashboard is displayed.</span></span>

   ![Hozzáférés az Azure Advisor hello Azure-portál használatával](./media/advisor-overview/advisor-azure-portal-menu.png) 

4. <span data-ttu-id="bcb16-111">Hello Advisor irányítópultra válassza ki a kívánt tooreceive javaslatok hello előfizetést.</span><span class="sxs-lookup"><span data-stu-id="bcb16-111">On hello Advisor dashboard, select hello subscription for which you want tooreceive recommendations.</span></span>  
<span data-ttu-id="bcb16-112">hello Advisor irányítópulton megjelenített személyre szabott javaslatok a kiválasztott előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="bcb16-112">hello Advisor dashboard displays personalized recommendations for a selected subscription.</span></span> 

5. <span data-ttu-id="bcb16-113">egy adott kategória tooget javaslatok kattintson az hello lapfülek egyikére: **magas rendelkezésre állású**, **biztonsági**, **teljesítmény**, vagy **költség**.</span><span class="sxs-lookup"><span data-stu-id="bcb16-113">tooget recommendations for a particular category, click one of hello tabs: **High Availability**, **Security**, **Performance**, or **Cost**.</span></span>
 
> [!NOTE]
> <span data-ttu-id="bcb16-114">Advisor-javaslatokra tooaccess, először *az előfizetés regisztrálása* az Advisor szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="bcb16-114">tooaccess Advisor recommendations, you must first *register your subscription* with Advisor.</span></span> <span data-ttu-id="bcb16-115">Egy előfizetés regisztrálva amikor egy *előfizetés tulajdonosának* elindítja az Advisor-irányítópult és az kattintással hello hello **javaslatok beszerzése** gombra.</span><span class="sxs-lookup"><span data-stu-id="bcb16-115">A subscription is registered when a *subscription Owner* launches hello Advisor dashboard and clicks hello **Get recommendations** button.</span></span> <span data-ttu-id="bcb16-116">Ez egy *egyszeri művelet*.</span><span class="sxs-lookup"><span data-stu-id="bcb16-116">This is a *one-time operation*.</span></span> <span data-ttu-id="bcb16-117">Hello előfizetés regisztrálása után érheti el, az Advisor-javaslatokra *tulajdonos*, *közreműködő*, vagy *olvasó* -előfizetéssel, egy erőforráscsoport vagy egy adott erőforrás.</span><span class="sxs-lookup"><span data-stu-id="bcb16-117">After hello subscription is registered, you can access Advisor recommendations as *Owner*, *Contributor*, or *Reader* for a subscription, a resource group, or a specific resource.</span></span>

  ![Az Azure Advisor irányítópult](./media/advisor-overview/advisor-all-tab.png)

## <a name="get-advisor-recommendation-details-and-implement-a-solution"></a><span data-ttu-id="bcb16-119">Az Advisor javaslat részletes, és egy megoldás bevezetésére</span><span class="sxs-lookup"><span data-stu-id="bcb16-119">Get Advisor recommendation details, and implement a solution</span></span>

<span data-ttu-id="bcb16-120">Hello **javaslat** panel az Advisor hello javaslat további információt nyújt.</span><span class="sxs-lookup"><span data-stu-id="bcb16-120">hello **Recommendation** blade in Advisor offers additional information about hello recommendation.</span></span> 

1. <span data-ttu-id="bcb16-121">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com), majd indítsa el a [Azure Advisor](https://aka.ms/azureadvisordashboard).</span><span class="sxs-lookup"><span data-stu-id="bcb16-121">Sign in toohello [Azure portal](https://portal.azure.com), and then start [Azure Advisor](https://aka.ms/azureadvisordashboard).</span></span>

2. <span data-ttu-id="bcb16-122">A hello **Advisor-javaslatokra** irányítópultján kattintson **javaslatok beszerzése**.</span><span class="sxs-lookup"><span data-stu-id="bcb16-122">On hello **Advisor recommendations** dashboard, click **Get recommendations**.</span></span>

3. <span data-ttu-id="bcb16-123">A javaslatok hello listájában kattintson a azt ajánljuk, hogy szeretné-e tooreview részletesen.</span><span class="sxs-lookup"><span data-stu-id="bcb16-123">In hello list of recommendations, click a recommendation that you want tooreview in detail.</span></span>  
<span data-ttu-id="bcb16-124">Hello **javaslat** panel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="bcb16-124">hello **Recommendation** blade is displayed.</span></span>

4. <span data-ttu-id="bcb16-125">A hello **javaslatok** panelt, tekintse át adatokat azokról a műveletekről, hogy lehet elvégezni egy potenciális problémát tooresolve vagy egy költségkímélő lehetőség előnyeit.</span><span class="sxs-lookup"><span data-stu-id="bcb16-125">On hello **Recommendations** blade, review information about actions that you can perform tooresolve a potential issue, or take advantage of a cost-saving opportunity.</span></span> 
  
  ![hello Advisor indexjavaslat panelen](./media/advisor-overview/advisor-recommendation-action-example.png)

## <a name="search-for-advisor-recommendations"></a><span data-ttu-id="bcb16-127">Keresés az Advisor-javaslatokra</span><span class="sxs-lookup"><span data-stu-id="bcb16-127">Search for Advisor recommendations</span></span>

<span data-ttu-id="bcb16-128">Kereshet egy adott előfizetés vagy az erőforrás csoport javaslatok.</span><span class="sxs-lookup"><span data-stu-id="bcb16-128">You can search for recommendations for a particular subscription or resource group.</span></span> <span data-ttu-id="bcb16-129">Is kereshet javaslatok állapot szerint.</span><span class="sxs-lookup"><span data-stu-id="bcb16-129">You can also search for recommendations by status.</span></span>

1. <span data-ttu-id="bcb16-130">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com), majd indítsa el a [Azure Advisor](https://aka.ms/azureadvisordashboard).</span><span class="sxs-lookup"><span data-stu-id="bcb16-130">Sign in toohello [Azure portal](https://portal.azure.com), and then start [Azure Advisor](https://aka.ms/azureadvisordashboard).</span></span>

2. <span data-ttu-id="bcb16-131">Az előfizetések, erőforráscsoport-sablonok és javaslat állapot szűréssel javaslatokra keresési (**aktív** vagy **Snoozed**).</span><span class="sxs-lookup"><span data-stu-id="bcb16-131">Search for recommendations by filtering for subscriptions, resource groups, and recommendation status (**Active** or **Snoozed**).</span></span>

3. <span data-ttu-id="bcb16-132">toodisplay alapuló Advisor-javaslatokra listáját a keresés-szűrési feltételeket, kattintson a **javaslatok beszerzése**.</span><span class="sxs-lookup"><span data-stu-id="bcb16-132">toodisplay a list of Advisor recommendations that are based on your search-filter criteria, click **Get recommendations**.</span></span>

  ![Az Advisor keresési-szűrési feltételeket](./media/advisor-get-started/advisor-search.png)

## <a name="snooze-or-dismiss-advisor-recommendations"></a><span data-ttu-id="bcb16-134">Emlékeztet, vagy hagyja figyelmen kívül az Advisor-javaslatokra</span><span class="sxs-lookup"><span data-stu-id="bcb16-134">Snooze or dismiss Advisor recommendations</span></span>

1. <span data-ttu-id="bcb16-135">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com), majd indítsa el a [Azure Advisor](https://aka.ms/azureadvisordashboard).</span><span class="sxs-lookup"><span data-stu-id="bcb16-135">Sign in toohello [Azure portal](https://portal.azure.com), and then start [Azure Advisor](https://aka.ms/azureadvisordashboard).</span></span>

2. <span data-ttu-id="bcb16-136">Kattintson a **javaslatok beszerzése**, és javaslatok hello listában, kattintson egy javaslatot.</span><span class="sxs-lookup"><span data-stu-id="bcb16-136">Click **Get recommendations**, and then, in hello list of recommendations, click a recommendation.</span></span>

3. <span data-ttu-id="bcb16-137">A hello **javaslat** panelen kattintson a **emlékeztet**.</span><span class="sxs-lookup"><span data-stu-id="bcb16-137">On hello **Recommendation** blade, click **Snooze**.</span></span>  

   ![Az Advisor javasolt művelet – példa](./media/advisor-get-started/advisor-snooze.png)

4. <span data-ttu-id="bcb16-139">Adjon meg egy emlékeztető időszakra vonatkozóan, vagy válasszon **soha** toodismiss hello javaslat.</span><span class="sxs-lookup"><span data-stu-id="bcb16-139">Specify a snooze time period, or select **Never** toodismiss hello recommendation.</span></span>


## <a name="next-steps"></a><span data-ttu-id="bcb16-140">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bcb16-140">Next steps</span></span>

<span data-ttu-id="bcb16-141">toolearn Advisor kapcsolatos további információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="bcb16-141">toolearn more about Advisor, see:</span></span>
* [<span data-ttu-id="bcb16-142">Bevezetés tooAzure Advisor</span><span class="sxs-lookup"><span data-stu-id="bcb16-142">Introduction tooAzure Advisor</span></span>](advisor-overview.md)
* [<span data-ttu-id="bcb16-143">Magas rendelkezésre állású javaslatokat biztosít</span><span class="sxs-lookup"><span data-stu-id="bcb16-143">Advisor High Availability recommendations</span></span>](advisor-high-availability-recommendations.md)
* [<span data-ttu-id="bcb16-144">Biztonsági javaslatokat biztosít</span><span class="sxs-lookup"><span data-stu-id="bcb16-144">Advisor Security recommendations</span></span>](advisor-security-recommendations.md)
-  [<span data-ttu-id="bcb16-145">Teljesítmény javaslatokat biztosít</span><span class="sxs-lookup"><span data-stu-id="bcb16-145">Advisor Performance recommendations</span></span>](advisor-performance-recommendations.md)
* [<span data-ttu-id="bcb16-146">Költség javaslatokat biztosít</span><span class="sxs-lookup"><span data-stu-id="bcb16-146">Advisor Cost recommendations</span></span>](advisor-performance-recommendations.md)
