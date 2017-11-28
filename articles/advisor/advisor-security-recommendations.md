---
title: "Biztonsági javaslatokat biztosít aaaAzure |} Microsoft Docs"
description: "Használata Azure Advisor toohelp az Azure-környezetekhez hello biztonságának javítása."
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
ms.openlocfilehash: e01ac29eb6e02bff0b1e846e320e7c36f85c7343
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="advisor-security-recommendations"></a><span data-ttu-id="3413c-103">Biztonsági javaslatokat biztosít</span><span class="sxs-lookup"><span data-stu-id="3413c-103">Advisor Security recommendations</span></span>

<span data-ttu-id="3413c-104">Azure Advisor egy egységes, összevont nézetének összes Azure-erőforrások javaslatokat nyújt.</span><span class="sxs-lookup"><span data-stu-id="3413c-104">Azure Advisor provides you with a consistent, consolidated view of recommendations for all your Azure resources.</span></span> <span data-ttu-id="3413c-105">Az integrálható az Azure Security Center toobring meg biztonsági javaslatok.</span><span class="sxs-lookup"><span data-stu-id="3413c-105">It integrates with Azure Security Center toobring you security recommendations.</span></span> <span data-ttu-id="3413c-106">Biztonsági javaslatok beszerzése hello **biztonsági** hello Advisor irányítópult fület.</span><span class="sxs-lookup"><span data-stu-id="3413c-106">You can get security recommendations from hello **Security** tab on hello Advisor dashboard.</span></span>

![hello Advisor biztonsági gomb](./media/advisor-security-recommendations/advisor-security-tab.png)

<span data-ttu-id="3413c-108">A Security Center segítséget nyújt a megakadályozása, észlelésében és kezelésében toothreats láthatóság növelésével és az Azure-erőforrások hello biztonságát vezérelheti.</span><span class="sxs-lookup"><span data-stu-id="3413c-108">Security Center helps you prevent, detect, and respond toothreats with increased visibility into and control over hello security of your Azure resources.</span></span> <span data-ttu-id="3413c-109">Rendszeresen elemzi a hello az Azure-erőforrások biztonsági állapotát.</span><span class="sxs-lookup"><span data-stu-id="3413c-109">It periodically analyzes hello security state of your Azure resources.</span></span> <span data-ttu-id="3413c-110">A Security Center javaslatokat hoz létre, amikor lehetséges biztonsági réseket észlel.</span><span class="sxs-lookup"><span data-stu-id="3413c-110">When Security Center identifies potential security vulnerabilities, it creates recommendations.</span></span> <span data-ttu-id="3413c-111">hello javaslatok végigvezetik hello kell hello vezérlők konfigurálásának lépésein.</span><span class="sxs-lookup"><span data-stu-id="3413c-111">hello recommendations guide you through hello process of configuring hello controls you need.</span></span> 

![hello Advisor Biztonság lap](./media/advisor-security-recommendations/advisor-security-recommendations.png)

<span data-ttu-id="3413c-113">További információ a biztonsági javaslatok: [biztonsági javaslatok kezelése az Azure Security Center](https://azure.microsoft.com/en-us/documentation/articles/security-center-recommendations/).</span><span class="sxs-lookup"><span data-stu-id="3413c-113">For more information about security recommendations, see [Managing security recommendations in Azure Security Center](https://azure.microsoft.com/en-us/documentation/articles/security-center-recommendations/).</span></span>

## <a name="how-tooaccess-security-recommendations-in-azure-advisor"></a><span data-ttu-id="3413c-114">Hogyan tooaccess biztonsági javaslatok az Azure-tanácsadó</span><span class="sxs-lookup"><span data-stu-id="3413c-114">How tooaccess Security recommendations in Azure Advisor</span></span>

1. <span data-ttu-id="3413c-115">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="3413c-115">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="3413c-116">Hello bal oldali ablaktáblában kattintson **további szolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="3413c-116">In hello left pane, click **More services**.</span></span>

3. <span data-ttu-id="3413c-117">A hello szolgáltatás menü ablaktáblán, a **figyelés és felügyelet**, kattintson a **Azure Advisor**.</span><span class="sxs-lookup"><span data-stu-id="3413c-117">In hello service menu pane, under **Monitoring and Management**, click **Azure Advisor**.</span></span>  
 <span data-ttu-id="3413c-118">hello Advisor irányítópult jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="3413c-118">hello Advisor dashboard is displayed.</span></span>

4. <span data-ttu-id="3413c-119">Az hello Advisor irányítópultján kattintson a hello **biztonsági** fülre.</span><span class="sxs-lookup"><span data-stu-id="3413c-119">On hello Advisor dashboard, click hello **Security** tab.</span></span>

5. <span data-ttu-id="3413c-120">Válassza ki hello előfizetést, amelyhez szeretné, hogy tooreceive javaslatokat, és kattintson **javaslatok beszerzése**.</span><span class="sxs-lookup"><span data-stu-id="3413c-120">Select hello subscription for which you want tooreceive recommendations, and then click **Get recommendations**.</span></span>

> [!NOTE]
> <span data-ttu-id="3413c-121">Advisor-javaslatokra tooaccess, először *az előfizetés regisztrálása* az Advisor szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="3413c-121">tooaccess Advisor recommendations, you must first *register your subscription* with Advisor.</span></span> <span data-ttu-id="3413c-122">Egy előfizetés regisztrálva amikor egy *előfizetés tulajdonosának* elindítja az Advisor-irányítópult és az kattintással hello hello **javaslatok beszerzése** gombra.</span><span class="sxs-lookup"><span data-stu-id="3413c-122">A subscription is registered when a *subscription Owner* launches hello Advisor dashboard and clicks hello **Get recommendations** button.</span></span> <span data-ttu-id="3413c-123">Ez egy *egyszeri művelet*.</span><span class="sxs-lookup"><span data-stu-id="3413c-123">This is a *one-time operation*.</span></span> <span data-ttu-id="3413c-124">Hello előfizetés regisztrálása után érheti el, az Advisor-javaslatokra *tulajdonos*, *közreműködő*, vagy *olvasó* -előfizetéssel, egy erőforráscsoport vagy egy adott erőforrás.</span><span class="sxs-lookup"><span data-stu-id="3413c-124">After hello subscription is registered, you can access Advisor recommendations as *Owner*, *Contributor*, or *Reader* for a subscription, a resource group, or a specific resource.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3413c-125">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3413c-125">Next steps</span></span>

<span data-ttu-id="3413c-126">További információ az Advisor-javaslatokra toolearn lásd:</span><span class="sxs-lookup"><span data-stu-id="3413c-126">toolearn more about Advisor recommendations, see:</span></span>
* [<span data-ttu-id="3413c-127">Bevezetés tooAdvisor</span><span class="sxs-lookup"><span data-stu-id="3413c-127">Introduction tooAdvisor</span></span>](advisor-overview.md)
* [<span data-ttu-id="3413c-128">Bevezetés az Advisor használatába</span><span class="sxs-lookup"><span data-stu-id="3413c-128">Get started with Advisor</span></span>](advisor-get-started.md)
* [<span data-ttu-id="3413c-129">Költség javaslatokat biztosít</span><span class="sxs-lookup"><span data-stu-id="3413c-129">Advisor Cost recommendations</span></span>](advisor-performance-recommendations.md)
* [<span data-ttu-id="3413c-130">Teljesítmény javaslatokat biztosít</span><span class="sxs-lookup"><span data-stu-id="3413c-130">Advisor Performance recommendations</span></span>](advisor-performance-recommendations.md)
* [<span data-ttu-id="3413c-131">Magas rendelkezésre állású javaslatokat biztosít</span><span class="sxs-lookup"><span data-stu-id="3413c-131">Advisor High Availability recommendations</span></span>](advisor-high-availability-recommendations.md)


 
