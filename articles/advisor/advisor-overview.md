---
title: "Bevezetés az Azure Advisor |} Microsoft Docs"
description: "Azure Advisor segítségével optimalizálhatja az Azure-környezetekhez."
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
ms.openlocfilehash: 35678142550f9f887562f311a5e7d9516495cf53
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="introduction-to-azure-advisor"></a><span data-ttu-id="bb4b6-103">Bevezetés az Azure Advisor</span><span class="sxs-lookup"><span data-stu-id="bb4b6-103">Introduction to Azure Advisor</span></span>

<span data-ttu-id="bb4b6-104">Tudnivalók az Azure Advisor és annak főbb funkcióiról, és gyakran feltett kérdésekre adott válaszok.</span><span class="sxs-lookup"><span data-stu-id="bb4b6-104">Learn about Azure Advisor and its key capabilities, and get answers to frequently asked questions.</span></span>

## <a name="what-is-advisor"></a><span data-ttu-id="bb4b6-105">Mi az az Advisor?</span><span class="sxs-lookup"><span data-stu-id="bb4b6-105">What is Advisor?</span></span>
<span data-ttu-id="bb4b6-106">Az Advisor egy személyre szabott felhő tanácsadó, amelynek segítségével hajtsa végre az ajánlott eljárások az Azure-környezetekhez optimalizálása érdekében.</span><span class="sxs-lookup"><span data-stu-id="bb4b6-106">Advisor is a personalized cloud consultant that helps you follow best practices to optimize your Azure deployments.</span></span> <span data-ttu-id="bb4b6-107">Az erőforrás-konfigurációhoz és használat telemetriai adatai elemzi, és az, amelyek javítják a költséghatékonyság, a teljesítmény, a magas rendelkezésre állású és az Azure-erőforrások biztonsági megoldások javasolja.</span><span class="sxs-lookup"><span data-stu-id="bb4b6-107">It analyzes your resource configuration and usage telemetry and then recommends solutions that can help you improve the cost effectiveness, performance, high availability, and security of your Azure resources.</span></span>

<span data-ttu-id="bb4b6-108">Az Advisor szolgáltatásban a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="bb4b6-108">With Advisor, you can:</span></span>
* <span data-ttu-id="bb4b6-109">Szerezze be a proaktív, hajtható végre, és személyre szabott ajánlott eljárások a.</span><span class="sxs-lookup"><span data-stu-id="bb4b6-109">Get proactive, actionable, and personalized best practices recommendations.</span></span> 
* <span data-ttu-id="bb4b6-110">A teljes Azure csökkentése érdekében lehetőségek meghatározása teljesítmény, biztonsági és az erőforrások magas rendelkezésre állás javítása televíziózással töltenek.</span><span class="sxs-lookup"><span data-stu-id="bb4b6-110">Improve the performance, security, and high availability of your resources, as you identify opportunities to reduce your overall Azure spend.</span></span>
* <span data-ttu-id="bb4b6-111">A javasolt műveletek beágyazott javaslatok beszerzése.</span><span class="sxs-lookup"><span data-stu-id="bb4b6-111">Get recommendations with proposed actions inline.</span></span>

<span data-ttu-id="bb4b6-112">Az Advisor keresztül érheti el a [Azure-portálon](https://aka.ms/azureadvisordashboard).</span><span class="sxs-lookup"><span data-stu-id="bb4b6-112">You can access Advisor through the [Azure portal](https://aka.ms/azureadvisordashboard).</span></span> <span data-ttu-id="bb4b6-113">Jelentkezzen be a [portal](https://portal.azure.com), jelölje be **Tallózás**, majd görgessen a **Azure Advisor**.</span><span class="sxs-lookup"><span data-stu-id="bb4b6-113">Sign in to the [portal](https://portal.azure.com), select **Browse**, and then scroll to **Azure Advisor**.</span></span> <span data-ttu-id="bb4b6-114">Az Advisor irányítópulton megjelenített személyre szabott javaslatok a kiválasztott előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="bb4b6-114">The Advisor dashboard displays personalized recommendations for a selected subscription.</span></span> 

<span data-ttu-id="bb4b6-115">A javaslatok négy kategóriába oszthatók:</span><span class="sxs-lookup"><span data-stu-id="bb4b6-115">The recommendations are divided into four categories:</span></span> 

* <span data-ttu-id="bb4b6-116">**Magas rendelkezésre állású**: Győződjön meg arról, az üzleti szempontból kritikus fontosságú alkalmazások folytonosságának javításához.</span><span class="sxs-lookup"><span data-stu-id="bb4b6-116">**High Availability**: To ensure and improve the continuity of your business-critical applications.</span></span> <span data-ttu-id="bb4b6-117">További információkért lásd: [Advisor magas rendelkezésre állású javaslatok](advisor-high-availability-recommendations.md).</span><span class="sxs-lookup"><span data-stu-id="bb4b6-117">For more information, see [Advisor High Availability recommendations](advisor-high-availability-recommendations.md).</span></span>

* <span data-ttu-id="bb4b6-118">**Biztonsági**: veszélyek és biztonsági problémák vezethet biztonsági rések észlelése.</span><span class="sxs-lookup"><span data-stu-id="bb4b6-118">**Security**: To detect threats and vulnerabilities that might lead to security breaches.</span></span> <span data-ttu-id="bb4b6-119">További információkért lásd: [Advisor biztonsági javaslatok](advisor-security-recommendations.md).</span><span class="sxs-lookup"><span data-stu-id="bb4b6-119">For more information, see [Advisor Security recommendations](advisor-security-recommendations.md).</span></span>

* <span data-ttu-id="bb4b6-120">**Teljesítmény**: az alkalmazások a sebesség növelése érdekében.</span><span class="sxs-lookup"><span data-stu-id="bb4b6-120">**Performance**: To improve the speed of your applications.</span></span> <span data-ttu-id="bb4b6-121">További információkért lásd: [Advisor teljesítmény javaslatok](advisor-performance-recommendations.md).</span><span class="sxs-lookup"><span data-stu-id="bb4b6-121">For more information, see [Advisor Performance recommendations](advisor-performance-recommendations.md).</span></span>

* <span data-ttu-id="bb4b6-122">**Költség**: optimalizálása és a teljes Azure televíziózással töltenek.</span><span class="sxs-lookup"><span data-stu-id="bb4b6-122">**Cost**: To optimize and reduce your overall Azure spend.</span></span> <span data-ttu-id="bb4b6-123">További információkért lásd: [Advisor költség javaslatok](advisor-cost-recommendations.md).</span><span class="sxs-lookup"><span data-stu-id="bb4b6-123">For more information, see [Advisor Cost recommendations](advisor-cost-recommendations.md).</span></span>

  ![Az Advisor javaslat típusok](./media/advisor-overview/advisor-all-tab-examples.png)

> [!NOTE]
> <span data-ttu-id="bb4b6-125">Advisor-javaslatokra szeretne használni, először *az előfizetés regisztrálása* az Advisor szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="bb4b6-125">To access Advisor recommendations, you must first *register your subscription* with Advisor.</span></span> <span data-ttu-id="bb4b6-126">Egy előfizetés regisztrálva amikor egy *előfizetés tulajdonosának* elindítja az Advisor irányítópulton, és rákattint a **javaslatok beszerzése** gombra.</span><span class="sxs-lookup"><span data-stu-id="bb4b6-126">A subscription is registered when a *subscription Owner* launches the Advisor dashboard and clicks the **Get recommendations** button.</span></span> <span data-ttu-id="bb4b6-127">Ez egy *egyszeri művelet*.</span><span class="sxs-lookup"><span data-stu-id="bb4b6-127">This is a *one-time operation*.</span></span> <span data-ttu-id="bb4b6-128">Az előfizetés regisztrálása után érheti el, az Advisor-javaslatokra *tulajdonos*, *közreműködő*, vagy *olvasó* előfizetés, egy erőforráscsoport vagy egy adott erőforrás.</span><span class="sxs-lookup"><span data-stu-id="bb4b6-128">After the subscription is registered, you can access Advisor recommendations as *Owner*, *Contributor*, or *Reader* for a subscription, a resource group, or a specific resource.</span></span>

<span data-ttu-id="bb4b6-129">Kattintson a javaslatra kattintva olvashat azokról bővebben.</span><span class="sxs-lookup"><span data-stu-id="bb4b6-129">You can click a recommendation to learn more about it.</span></span> <span data-ttu-id="bb4b6-130">Lehetőség előnyeit, vagy hárítsa el a problémát a végrehajtható műveletek is olvashat.</span><span class="sxs-lookup"><span data-stu-id="bb4b6-130">You can also learn about actions that you can perform to take advantage of an opportunity or resolve an issue.</span></span> 

<span data-ttu-id="bb4b6-131">Az Advisor ezekre vonatkozó javaslatokat beágyazott műveletek vagy dokumentáció hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="bb4b6-131">Advisor offers recommendations with inline actions or documentation links.</span></span> <span data-ttu-id="bb4b6-132">Egy beágyazott művelet végigvezeti Önt egy "az interaktív felhasználói út" végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="bb4b6-132">Clicking an inline action takes you through a “guided user journey” to implement it.</span></span> <span data-ttu-id="bb4b6-133">Dokumentáció hivatkozásra kattintva megtudhatja a dokumentáció, és ismerteti, hogyan lehet manuálisan valósítja meg a műveletet.</span><span class="sxs-lookup"><span data-stu-id="bb4b6-133">Clicking a documentation link points you to documentation that describes how to manually implement the action.</span></span> 

<span data-ttu-id="bb4b6-134">Az Advisor óránként frissíti a javaslatokat.</span><span class="sxs-lookup"><span data-stu-id="bb4b6-134">Advisor updates recommendations hourly.</span></span> <span data-ttu-id="bb4b6-135">Ha nem szeretné azonnal intézkedhet ajánlása, emlékeztet, hogy egy adott időszakra vonatkozóan, vagy zárja be azt.</span><span class="sxs-lookup"><span data-stu-id="bb4b6-135">If you don’t intend to take immediate action on a recommendation, you can snooze it for a specified time period or dismiss it.</span></span> 

## <a name="frequently-asked-questions"></a><span data-ttu-id="bb4b6-136">Gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="bb4b6-136">Frequently asked questions</span></span>

### <a name="how-do-i-access-advisor"></a><span data-ttu-id="bb4b6-137">Hogyan érhetem el az Advisor?</span><span class="sxs-lookup"><span data-stu-id="bb4b6-137">How do I access Advisor?</span></span>
<span data-ttu-id="bb4b6-138">Az Advisor keresztül érheti el a [Azure-portálon](https://aka.ms/azureadvisordashboard).</span><span class="sxs-lookup"><span data-stu-id="bb4b6-138">You can access Advisor through the [Azure portal](https://aka.ms/azureadvisordashboard).</span></span> <span data-ttu-id="bb4b6-139">Jelentkezzen be a [portal](https://portal.azure.com), jelölje be **Tallózás**, majd görgessen a **Azure Advisor**.</span><span class="sxs-lookup"><span data-stu-id="bb4b6-139">Sign in to the [portal](https://portal.azure.com), select **Browse**, and then scroll to **Azure Advisor**.</span></span> <span data-ttu-id="bb4b6-140">Az Advisor irányítópulton megjelenített személyre szabott javaslatok a kiválasztott előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="bb4b6-140">The Advisor dashboard displays personalized recommendations for a selected subscription.</span></span> 

<span data-ttu-id="bb4b6-141">Javaslatokat biztosít a virtuális gép erőforráspaneljének keresztül is megtekintheti.</span><span class="sxs-lookup"><span data-stu-id="bb4b6-141">You can also view Advisor recommendations through the virtual machine resource blade.</span></span> <span data-ttu-id="bb4b6-142">Válassza ki a virtuális gépet, és görgessen a menü javaslatokat biztosít.</span><span class="sxs-lookup"><span data-stu-id="bb4b6-142">Choose a virtual machine, and then scroll to Advisor recommendations in the menu.</span></span> 

### <a name="what-permissions-do-i-need-to-access-advisor"></a><span data-ttu-id="bb4b6-143">Milyen engedélyekkel Advisor elérésére van?</span><span class="sxs-lookup"><span data-stu-id="bb4b6-143">What permissions do I need to access Advisor?</span></span>

<span data-ttu-id="bb4b6-144">Advisor-javaslatokra szeretne használni, először *az előfizetés regisztrálása* az Advisor szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="bb4b6-144">To access Advisor recommendations, you must first *register your subscription* with Advisor.</span></span> <span data-ttu-id="bb4b6-145">Egy előfizetés regisztrálva amikor egy *előfizetés tulajdonosának* elindítja az Advisor irányítópulton, és rákattint a **javaslatok beszerzése** gombra.</span><span class="sxs-lookup"><span data-stu-id="bb4b6-145">A subscription is registered when a *subscription Owner* launches the Advisor dashboard and clicks the **Get recommendations** button.</span></span> <span data-ttu-id="bb4b6-146">Ez egy *egyszeri művelet*.</span><span class="sxs-lookup"><span data-stu-id="bb4b6-146">This is a *one-time operation*.</span></span> <span data-ttu-id="bb4b6-147">Az előfizetés regisztrálása után érheti el, az Advisor-javaslatokra *tulajdonos*, *közreműködő*, vagy *olvasó* előfizetés, egy erőforráscsoport vagy egy adott erőforrás.</span><span class="sxs-lookup"><span data-stu-id="bb4b6-147">After the subscription is registered, you can access Advisor recommendations as *Owner*, *Contributor*, or *Reader* for a subscription, a resource group, or a specific resource.</span></span>

### <a name="how-often-are-advisor-recommendations-updated"></a><span data-ttu-id="bb4b6-148">Milyen gyakran történik az Advisor-javaslatokra frissíteni?</span><span class="sxs-lookup"><span data-stu-id="bb4b6-148">How often are Advisor recommendations updated?</span></span>

<span data-ttu-id="bb4b6-149">Advisor-javaslatokra óránként frissíti.</span><span class="sxs-lookup"><span data-stu-id="bb4b6-149">Advisor recommendations are updated hourly.</span></span>

### <a name="what-resources-does-advisor-provide-recommendations-for"></a><span data-ttu-id="bb4b6-150">Milyen erőforrásokat Advisor nyújt javaslatokat?</span><span class="sxs-lookup"><span data-stu-id="bb4b6-150">What resources does Advisor provide recommendations for?</span></span>

<span data-ttu-id="bb4b6-151">Az Advisor virtuális gépek rendelkezésre állási készletek, alkalmazásátjárót, alkalmazásszolgáltatások, SQL Server-kiszolgálók, SQL-adatbázisok és Redis Cache vonatkozó javaslatokkal szolgál.</span><span class="sxs-lookup"><span data-stu-id="bb4b6-151">Advisor provides recommendations for virtual machines, availability sets, application gateways, App Services, SQL servers, SQL databases, and Redis Cache.</span></span>

### <a name="can-i-snooze-or-dismiss-a-recommendation"></a><span data-ttu-id="bb4b6-152">Emlékeztet vagy hagyja figyelmen kívül az ajánlás?</span><span class="sxs-lookup"><span data-stu-id="bb4b6-152">Can I snooze or dismiss a recommendation?</span></span>

<span data-ttu-id="bb4b6-153">Emlékeztet, vagy hagyja figyelmen kívül az ajánlás olyan környezetekben, kattintson a **emlékeztet** gombra vagy hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="bb4b6-153">To snooze or dismiss a recommendation, click the **Snooze** button or link.</span></span> <span data-ttu-id="bb4b6-154">Megadhat egy időtartamot időszak vagy select **soha** elvetni a javaslat.</span><span class="sxs-lookup"><span data-stu-id="bb4b6-154">You can specify a snooze time period or select **Never** to dismiss the recommendation.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bb4b6-155">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bb4b6-155">Next steps</span></span>

<span data-ttu-id="bb4b6-156">Az Advisor-javaslatokra kapcsolatos további információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="bb4b6-156">To learn more about Advisor recommendations, see:</span></span>

* [<span data-ttu-id="bb4b6-157">Bevezetés az Advisor használatába</span><span class="sxs-lookup"><span data-stu-id="bb4b6-157">Get started with Advisor</span></span>](advisor-get-started.md)
* [<span data-ttu-id="bb4b6-158">Magas rendelkezésre állású javaslatokat biztosít</span><span class="sxs-lookup"><span data-stu-id="bb4b6-158">Advisor High Availability recommendations</span></span>](advisor-high-availability-recommendations.md)
* [<span data-ttu-id="bb4b6-159">Biztonsági javaslatokat biztosít</span><span class="sxs-lookup"><span data-stu-id="bb4b6-159">Advisor Security recommendations</span></span>](advisor-security-recommendations.md)
* [<span data-ttu-id="bb4b6-160">Teljesítmény javaslatokat biztosít</span><span class="sxs-lookup"><span data-stu-id="bb4b6-160">Advisor Performance recommendations</span></span>](advisor-performance-recommendations.md)
* [<span data-ttu-id="bb4b6-161">Költség javaslatokat biztosít</span><span class="sxs-lookup"><span data-stu-id="bb4b6-161">Advisor Cost recommendations</span></span>](advisor-cost-recommendations.md)
