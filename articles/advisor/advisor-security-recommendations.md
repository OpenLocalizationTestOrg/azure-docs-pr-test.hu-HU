---
title: "Az Azure biztonsági javaslatokat biztosít |} Microsoft Docs"
description: "Azure Advisor használata az Azure-környezetekhez biztonságát tökéletesítése céljából."
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
ms.openlocfilehash: 53be05593c3733ccb27979a3a026414013125779
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="advisor-security-recommendations"></a><span data-ttu-id="30ae7-103">Biztonsági javaslatokat biztosít</span><span class="sxs-lookup"><span data-stu-id="30ae7-103">Advisor Security recommendations</span></span>

<span data-ttu-id="30ae7-104">Azure Advisor egy egységes, összevont nézetének összes Azure-erőforrások javaslatokat nyújt.</span><span class="sxs-lookup"><span data-stu-id="30ae7-104">Azure Advisor provides you with a consistent, consolidated view of recommendations for all your Azure resources.</span></span> <span data-ttu-id="30ae7-105">Az Azure Security Center biztonsági javaslatok növelheti integrálható.</span><span class="sxs-lookup"><span data-stu-id="30ae7-105">It integrates with Azure Security Center to bring you security recommendations.</span></span> <span data-ttu-id="30ae7-106">A biztonsági javaslatok kaphat a **biztonsági** az Advisor irányítópult fület.</span><span class="sxs-lookup"><span data-stu-id="30ae7-106">You can get security recommendations from the **Security** tab on the Advisor dashboard.</span></span>

![Az Advisor biztonsági gomb](./media/advisor-security-recommendations/advisor-security-tab.png)

<span data-ttu-id="30ae7-108">A Security Center az Azure-erőforrások biztonsági felügyeletének átláthatóbbá és szabályozhatóbbá tételével megkönnyíti a fenyegetések megelőzését, észlelését és elhárítását.</span><span class="sxs-lookup"><span data-stu-id="30ae7-108">Security Center helps you prevent, detect, and respond to threats with increased visibility into and control over the security of your Azure resources.</span></span> <span data-ttu-id="30ae7-109">Rendszeres időközönként megvizsgálja az Azure-erőforrások biztonsági állapotát.</span><span class="sxs-lookup"><span data-stu-id="30ae7-109">It periodically analyzes the security state of your Azure resources.</span></span> <span data-ttu-id="30ae7-110">A Security Center javaslatokat hoz létre, amikor lehetséges biztonsági réseket észlel.</span><span class="sxs-lookup"><span data-stu-id="30ae7-110">When Security Center identifies potential security vulnerabilities, it creates recommendations.</span></span> <span data-ttu-id="30ae7-111">A javaslatok végigvezeti Önt a szükséges vezérlők konfigurálásának lépésein.</span><span class="sxs-lookup"><span data-stu-id="30ae7-111">The recommendations guide you through the process of configuring the controls you need.</span></span> 

![Az Advisor Biztonság lap](./media/advisor-security-recommendations/advisor-security-recommendations.png)

<span data-ttu-id="30ae7-113">További információ a biztonsági javaslatok: [biztonsági javaslatok kezelése az Azure Security Center](https://azure.microsoft.com/en-us/documentation/articles/security-center-recommendations/).</span><span class="sxs-lookup"><span data-stu-id="30ae7-113">For more information about security recommendations, see [Managing security recommendations in Azure Security Center](https://azure.microsoft.com/en-us/documentation/articles/security-center-recommendations/).</span></span>

## <a name="how-to-access-security-recommendations-in-azure-advisor"></a><span data-ttu-id="30ae7-114">Biztonsági javaslatok az Azure Advisor elérése</span><span class="sxs-lookup"><span data-stu-id="30ae7-114">How to access Security recommendations in Azure Advisor</span></span>

1. <span data-ttu-id="30ae7-115">Jelentkezzen be az [Azure Portalra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="30ae7-115">Sign in to the [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="30ae7-116">Kattintson a bal oldali ablaktáblában **további szolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="30ae7-116">In the left pane, click **More services**.</span></span>

3. <span data-ttu-id="30ae7-117">A szolgáltatás menü ablaktáblán alatt **figyelés és felügyelet**, kattintson a **Azure Advisor**.</span><span class="sxs-lookup"><span data-stu-id="30ae7-117">In the service menu pane, under **Monitoring and Management**, click **Azure Advisor**.</span></span>  
 <span data-ttu-id="30ae7-118">Az Advisor irányítópult jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="30ae7-118">The Advisor dashboard is displayed.</span></span>

4. <span data-ttu-id="30ae7-119">Az Advisor irányítópultján kattintson a **biztonsági** fülre.</span><span class="sxs-lookup"><span data-stu-id="30ae7-119">On the Advisor dashboard, click the **Security** tab.</span></span>

5. <span data-ttu-id="30ae7-120">Válassza ki az előfizetést, amely javaslatokat kap, és kattintson a kívánt **javaslatok beszerzése**.</span><span class="sxs-lookup"><span data-stu-id="30ae7-120">Select the subscription for which you want to receive recommendations, and then click **Get recommendations**.</span></span>

> [!NOTE]
> <span data-ttu-id="30ae7-121">Advisor-javaslatokra szeretne használni, először *az előfizetés regisztrálása* az Advisor szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="30ae7-121">To access Advisor recommendations, you must first *register your subscription* with Advisor.</span></span> <span data-ttu-id="30ae7-122">Egy előfizetés regisztrálva amikor egy *előfizetés tulajdonosának* elindítja az Advisor irányítópulton, és rákattint a **javaslatok beszerzése** gombra.</span><span class="sxs-lookup"><span data-stu-id="30ae7-122">A subscription is registered when a *subscription Owner* launches the Advisor dashboard and clicks the **Get recommendations** button.</span></span> <span data-ttu-id="30ae7-123">Ez egy *egyszeri művelet*.</span><span class="sxs-lookup"><span data-stu-id="30ae7-123">This is a *one-time operation*.</span></span> <span data-ttu-id="30ae7-124">Az előfizetés regisztrálása után érheti el, az Advisor-javaslatokra *tulajdonos*, *közreműködő*, vagy *olvasó* előfizetés, egy erőforráscsoport vagy egy adott erőforrás.</span><span class="sxs-lookup"><span data-stu-id="30ae7-124">After the subscription is registered, you can access Advisor recommendations as *Owner*, *Contributor*, or *Reader* for a subscription, a resource group, or a specific resource.</span></span>

## <a name="next-steps"></a><span data-ttu-id="30ae7-125">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="30ae7-125">Next steps</span></span>

<span data-ttu-id="30ae7-126">Az Advisor-javaslatokra kapcsolatos további információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="30ae7-126">To learn more about Advisor recommendations, see:</span></span>
* [<span data-ttu-id="30ae7-127">Az Advisor bemutatása</span><span class="sxs-lookup"><span data-stu-id="30ae7-127">Introduction to Advisor</span></span>](advisor-overview.md)
* [<span data-ttu-id="30ae7-128">Bevezetés az Advisor használatába</span><span class="sxs-lookup"><span data-stu-id="30ae7-128">Get started with Advisor</span></span>](advisor-get-started.md)
* [<span data-ttu-id="30ae7-129">Költség javaslatokat biztosít</span><span class="sxs-lookup"><span data-stu-id="30ae7-129">Advisor Cost recommendations</span></span>](advisor-performance-recommendations.md)
* [<span data-ttu-id="30ae7-130">Teljesítmény javaslatokat biztosít</span><span class="sxs-lookup"><span data-stu-id="30ae7-130">Advisor Performance recommendations</span></span>](advisor-performance-recommendations.md)
* [<span data-ttu-id="30ae7-131">Magas rendelkezésre állású javaslatokat biztosít</span><span class="sxs-lookup"><span data-stu-id="30ae7-131">Advisor High Availability recommendations</span></span>](advisor-high-availability-recommendations.md)


 
