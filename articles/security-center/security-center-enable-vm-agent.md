---
title: "Engedélyezze a Virtuálisgép-ügynök az Azure Security Centerben |} Microsoft Docs"
description: "Ez a dokumentum azt ismerteti, hogyan valósítja meg az Azure Security Center ajánlás ** engedélyezése VM ügynök **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 5b431c25-4241-45b7-9556-cf2a1956f3da
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/2017
ms.author: terrylan
ms.openlocfilehash: 337a7adfd93c76882a749685702bea6d1524c96a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="enable-vm-agent-in-azure-security-center"></a><span data-ttu-id="5b58d-103">Az Azure Security Centerben Virtuálisgép-ügynök engedélyezése</span><span class="sxs-lookup"><span data-stu-id="5b58d-103">Enable VM Agent in Azure Security Center</span></span>
<span data-ttu-id="5b58d-104">A virtuális gép ügynököt telepíteni kell a virtuális gépek (VM) annak érdekében, hogy [az adatgyűjtést](security-center-enable-data-collection.md).</span><span class="sxs-lookup"><span data-stu-id="5b58d-104">The VM Agent must be installed on virtual machines (VMs) in order to [enable data collection](security-center-enable-data-collection.md).</span></span>  <span data-ttu-id="5b58d-105">Az Azure Security Center köszönhetően láthatja mely virtuális gépek a Virtuálisgép-ügynök igényelnek, és azt javasolja, hogy engedélyezi-e a Virtuálisgép-ügynököt a virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="5b58d-105">Azure Security Center enables you to see which VMs require the VM Agent and will recommend that you enable the VM Agent on those VMs.</span></span>

<span data-ttu-id="5b58d-106">Az Azure Marketplace-ről üzembe helyezett virtuális gépek esetében a virtuálisgép-ügynök alapértelmezés szerint telepítve van.</span><span class="sxs-lookup"><span data-stu-id="5b58d-106">The VM Agent is installed by default for VMs that are deployed from the Azure Marketplace.</span></span> <span data-ttu-id="5b58d-107">A virtuálisgép-ügynök telepítéséről a [Virtuális gép-ügynök és -bővítmények – 2. rész](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-2/) cikkben talál információkat.</span><span class="sxs-lookup"><span data-stu-id="5b58d-107">The article [VM Agent and Extensions – Part 2](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-2/) provides information on how to install the VM Agent.</span></span>

> [!NOTE]
> <span data-ttu-id="5b58d-108">Ez a dokumentum egy üzembe helyezést szemléltető példa segítségével mutatja be a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="5b58d-108">This document introduces the service by using an example deployment.</span></span> <span data-ttu-id="5b58d-109">A dokumentum nem tartalmaz lépésenkénti útmutatót.</span><span class="sxs-lookup"><span data-stu-id="5b58d-109">This is not a step-by-step guide.</span></span>
>
>

## <a name="implement-the-recommendation"></a><span data-ttu-id="5b58d-110">A javaslat megvalósítása</span><span class="sxs-lookup"><span data-stu-id="5b58d-110">Implement the recommendation</span></span>
1. <span data-ttu-id="5b58d-111">Az a **javaslatok panel**, jelölje be **Virtuálisgép-ügynök engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="5b58d-111">In the **Recommendations blade**, select **Enable VM Agent**.</span></span>
   <span data-ttu-id="5b58d-112">![Virtuálisgép-ügynök engedélyezése][1]</span><span class="sxs-lookup"><span data-stu-id="5b58d-112">![Enable VM Agent][1]</span></span>
2. <span data-ttu-id="5b58d-113">Ekkor megnyílik a panel **VM hiányzik vagy nem válaszol**.</span><span class="sxs-lookup"><span data-stu-id="5b58d-113">This opens the blade **VM Agent Is Missing Or Not Responding**.</span></span> <span data-ttu-id="5b58d-114">Ezen a panelen a virtuális gépeket, a Virtuálisgép-ügynök igénylő sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="5b58d-114">This blade lists the VMs that require the VM Agent.</span></span> <span data-ttu-id="5b58d-115">A panel a Virtuálisgép-ügynök telepítéséhez kövesse az utasításokat.</span><span class="sxs-lookup"><span data-stu-id="5b58d-115">Follow the instructions on the blade to install the VM agent.</span></span>
   <span data-ttu-id="5b58d-116">![Hiányzik a Virtuálisgép-ügynök][2]</span><span class="sxs-lookup"><span data-stu-id="5b58d-116">![VM Agent is missing][2]</span></span>

## <a name="see-also"></a><span data-ttu-id="5b58d-117">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="5b58d-117">See also</span></span>
<span data-ttu-id="5b58d-118">A Security Centerrel kapcsolatos további információkért olvassa el a következőket:</span><span class="sxs-lookup"><span data-stu-id="5b58d-118">To learn more about Security Center, see the following:</span></span>

* <span data-ttu-id="5b58d-119">[Biztonsági házirendek beállítása az Azure Security Centerben](security-center-policies.md) – Annak bemutatása, hogy miként konfigurálhat biztonsági házirendeket Azure-előfizetéseihez és az erőforráscsoportokhoz.</span><span class="sxs-lookup"><span data-stu-id="5b58d-119">[Setting security policies in Azure Security Center](security-center-policies.md)--Learn how to configure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="5b58d-120">[Biztonsági javaslatok kezelése az Azure Security Centerben](security-center-recommendations.md) – Miként könnyítik meg a javaslatok az Azure-erőforrások védelmét?</span><span class="sxs-lookup"><span data-stu-id="5b58d-120">[Managing security recommendations in Azure Security Center](security-center-recommendations.md)--Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="5b58d-121">[Biztonsági állapotmonitorozás az Azure Security Centerben](security-center-monitoring.md) – Útmutató az Azure-erőforrások állapotának monitorozásához.</span><span class="sxs-lookup"><span data-stu-id="5b58d-121">[Security health monitoring in Azure Security Center](security-center-monitoring.md)--Learn how to monitor the health of your Azure resources.</span></span>
* <span data-ttu-id="5b58d-122">[Biztonsági riasztások kezelése és reagálás a riasztásokra az Azure Security Centerben](security-center-managing-and-responding-alerts.md) – A biztonsági riasztások kezelése és az azokra való reagálás.</span><span class="sxs-lookup"><span data-stu-id="5b58d-122">[Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md)--Learn how to manage and respond to security alerts.</span></span>
* <span data-ttu-id="5b58d-123">[Partneri megoldások monitorozása az Azure Security Centerrel](security-center-partner-solutions.md) – Útmutató a partneri megoldások biztonsági állapotának monitorozásához.</span><span class="sxs-lookup"><span data-stu-id="5b58d-123">[Monitoring partner solutions with Azure Security Center](security-center-partner-solutions.md) -- Learn how to monitor the health status of your partner solutions.</span></span>
* <span data-ttu-id="5b58d-124">[Azure Security Center – gyakran ismételt kérdések](security-center-faq.md) – Gyakran ismételt kérdések a szolgáltatás használatával kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="5b58d-124">[Azure Security Center FAQ](security-center-faq.md)--Find frequently asked questions about using the service.</span></span>
* <span data-ttu-id="5b58d-125">[Azure biztonsági blog](http://blogs.msdn.com/b/azuresecurity/) – Tájékozódás az Azure biztonságával kapcsolatos legfrissebb hírekről és információkról.</span><span class="sxs-lookup"><span data-stu-id="5b58d-125">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/)--Get the latest Azure security news and information.</span></span>

<!--Image references-->
[1]: ./media/security-center-enable-vm-agent/enable-vm-agent.png
[2]: ./media/security-center-enable-vm-agent/vm-agent-is-missing.png
