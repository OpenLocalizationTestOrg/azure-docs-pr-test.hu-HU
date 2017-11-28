---
title: "Hárítsa el az endpoint protection állapotát riasztások az Azure Security Centerben |} Microsoft Docs"
description: "Ez a dokumentum azt ismerteti, hogyan valósítja meg az Azure Security Center ajánlás ** hárítsa el az Endpoint Protection állapotát riasztások **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 4050f453-98fc-4314-8438-d476469757fb
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/01/2016
ms.author: terrylan
ms.openlocfilehash: 5e6b136d6bd3b11fb82126d104fd0cb149255118
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="resolve-endpoint-protection-health-alerts-in-azure-security-center"></a><span data-ttu-id="535e2-103">Hárítsa el az endpoint protection állapotát riasztások az Azure Security Centerben</span><span class="sxs-lookup"><span data-stu-id="535e2-103">Resolve endpoint protection health alerts in Azure Security Center</span></span>
<span data-ttu-id="535e2-104">Azure Security Center javasolni fogja megoldani az észlelt endpoint protection állapotát riasztások.</span><span class="sxs-lookup"><span data-stu-id="535e2-104">Azure Security Center will recommend that you resolve detected endpoint protection health alerts.</span></span>  <span data-ttu-id="535e2-105">Biztonsági központ lehetővé teszi a virtuális gépek (VM), az endpoint protection hibáinak és hány hibák vannak.</span><span class="sxs-lookup"><span data-stu-id="535e2-105">Security Center enables you to see which virtual machines (VMs) have endpoint protection failures and how many failures.</span></span>

> [!NOTE]
> <span data-ttu-id="535e2-106">Ez a dokumentum egy üzembe helyezést szemléltető példa segítségével mutatja be a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="535e2-106">This document introduces the service by using an example deployment.</span></span> <span data-ttu-id="535e2-107">A dokumentum nem tartalmaz lépésenkénti útmutatót.</span><span class="sxs-lookup"><span data-stu-id="535e2-107">This is not a step-by-step guide.</span></span>
> 
> 

## <a name="implement-the-recommendation"></a><span data-ttu-id="535e2-108">A javaslat megvalósítása</span><span class="sxs-lookup"><span data-stu-id="535e2-108">Implement the recommendation</span></span>
1. <span data-ttu-id="535e2-109">Az a **javaslatok panel**, jelölje be **hárítsa el az Endpoint Protection állapotát riasztások**.</span><span class="sxs-lookup"><span data-stu-id="535e2-109">In the **Recommendations blade**, select **Resolve Endpoint Protection health alerts**.</span></span>
   <span data-ttu-id="535e2-110">![Endpoint Protection-állapotriasztások feloldása][1]</span><span class="sxs-lookup"><span data-stu-id="535e2-110">![Resolve endpoint protection health alerts][1]</span></span>
2. <span data-ttu-id="535e2-111">Ekkor megnyílik a panel **Endpoint Protection hiba** amely felsorolja azokat a virtuális gépek hibák és a hibák száma az egyes virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="535e2-111">This opens the blade **Endpoint Protection failure** which lists VMs with failures and the number of failures for each VM.</span></span> <span data-ttu-id="535e2-112">Jelöljön ki egy virtuális Gépet a listából.</span><span class="sxs-lookup"><span data-stu-id="535e2-112">Select a VM from the list.</span></span>
   <span data-ttu-id="535e2-113">![Az Endpoint protection hiba][2]</span><span class="sxs-lookup"><span data-stu-id="535e2-113">![Endpoint protection failure][2]</span></span>
3. <span data-ttu-id="535e2-114">A **hibák lista** ekkor megnyílik a kiválasztott virtuális géphez, hibák listáját megjelenítő panel.</span><span class="sxs-lookup"><span data-stu-id="535e2-114">A **Failures List** blade opens for the selected VM, displaying a list of failures.</span></span> <span data-ttu-id="535e2-115">Válassza ki a hibát további a listából.</span><span class="sxs-lookup"><span data-stu-id="535e2-115">Select a failure from the list to learn more.</span></span> <span data-ttu-id="535e2-116">Ekkor megnyílik egy panel a kijelölt hibával kapcsolatos információkat.</span><span class="sxs-lookup"><span data-stu-id="535e2-116">This opens a blade with information about the selected failure.</span></span>
   <span data-ttu-id="535e2-117">![Hibák lista][3]
   ![esemény][4]</span><span class="sxs-lookup"><span data-stu-id="535e2-117">![Failures list][3]
![Failure event][4]</span></span>

## <a name="see-also"></a><span data-ttu-id="535e2-118">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="535e2-118">See also</span></span>
<span data-ttu-id="535e2-119">A Security Centerrel kapcsolatos további információkért olvassa el a következőket:</span><span class="sxs-lookup"><span data-stu-id="535e2-119">To learn more about Security Center, see the following:</span></span>

* <span data-ttu-id="535e2-120">[Biztonsági házirendek beállítása az Azure Security Centerben](security-center-policies.md) – Annak bemutatása, hogy miként konfigurálhat biztonsági házirendeket Azure-előfizetéseihez és az erőforráscsoportokhoz.</span><span class="sxs-lookup"><span data-stu-id="535e2-120">[Setting security policies in Azure Security Center](security-center-policies.md)--Learn how to configure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="535e2-121">[Biztonsági javaslatok kezelése az Azure Security Centerben](security-center-recommendations.md) – Miként könnyítik meg a javaslatok az Azure-erőforrások védelmét?</span><span class="sxs-lookup"><span data-stu-id="535e2-121">[Managing security recommendations in Azure Security Center](security-center-recommendations.md)--Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="535e2-122">[Biztonsági állapotmonitorozás az Azure Security Centerben](security-center-monitoring.md) – Útmutató az Azure-erőforrások állapotának monitorozásához.</span><span class="sxs-lookup"><span data-stu-id="535e2-122">[Security health monitoring in Azure Security Center](security-center-monitoring.md)--Learn how to monitor the health of your Azure resources.</span></span>
* <span data-ttu-id="535e2-123">[Biztonsági riasztások kezelése és reagálás a riasztásokra az Azure Security Centerben](security-center-managing-and-responding-alerts.md) – A biztonsági riasztások kezelése és az azokra való reagálás.</span><span class="sxs-lookup"><span data-stu-id="535e2-123">[Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md)--Learn how to manage and respond to security alerts.</span></span>
* <span data-ttu-id="535e2-124">[Partneri megoldások monitorozása az Azure Security Centerrel](security-center-partner-solutions.md) – Útmutató a partneri megoldások biztonsági állapotának monitorozásához.</span><span class="sxs-lookup"><span data-stu-id="535e2-124">[Monitoring partner solutions with Azure Security Center](security-center-partner-solutions.md) -- Learn how to monitor the health status of your partner solutions.</span></span>
* <span data-ttu-id="535e2-125">[Azure Security Center – gyakran ismételt kérdések](security-center-faq.md) – Gyakran ismételt kérdések a szolgáltatás használatával kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="535e2-125">[Azure Security Center FAQ](security-center-faq.md)--Find frequently asked questions about using the service.</span></span>
* <span data-ttu-id="535e2-126">[Azure biztonsági blog](http://blogs.msdn.com/b/azuresecurity/) – Tájékozódás az Azure biztonságával kapcsolatos legfrissebb hírekről és információkról.</span><span class="sxs-lookup"><span data-stu-id="535e2-126">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/)--Get the latest Azure security news and information.</span></span>

<!--Image references-->
[1]: ./media/security-center-resolve-endpoint-protection/resolve-endpoint-protection.png
[2]: ./media/security-center-resolve-endpoint-protection/endpoint-protection-failure.png
[3]: ./media/security-center-resolve-endpoint-protection/failure-list.png
[4]: ./media/security-center-resolve-endpoint-protection/failure-event.png
