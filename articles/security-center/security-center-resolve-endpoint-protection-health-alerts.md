---
title: "aaaResolve endpoint protection állapotát riasztások az Azure Security Centerben |} Microsoft Docs"
description: "Ez a dokumentum bemutatja, hogyan tooimplement hello Azure Security Center javaslat ** hárítsa el az Endpoint Protection állapotát riasztások **."
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
ms.openlocfilehash: 9631d15aa1dfa9003d56332363ae7911061ed0b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="resolve-endpoint-protection-health-alerts-in-azure-security-center"></a><span data-ttu-id="d8c2e-103">Hárítsa el az endpoint protection állapotát riasztások az Azure Security Centerben</span><span class="sxs-lookup"><span data-stu-id="d8c2e-103">Resolve endpoint protection health alerts in Azure Security Center</span></span>
<span data-ttu-id="d8c2e-104">Azure Security Center javasolni fogja megoldani az észlelt endpoint protection állapotát riasztások.</span><span class="sxs-lookup"><span data-stu-id="d8c2e-104">Azure Security Center will recommend that you resolve detected endpoint protection health alerts.</span></span>  <span data-ttu-id="d8c2e-105">Biztonsági központ lehetővé teszi virtuális gépek (VM) rendelkezik, az endpoint protection hibáinak és hány sikertelen toosee.</span><span class="sxs-lookup"><span data-stu-id="d8c2e-105">Security Center enables you toosee which virtual machines (VMs) have endpoint protection failures and how many failures.</span></span>

> [!NOTE]
> <span data-ttu-id="d8c2e-106">Ez a dokumentum hello szolgáltatás telepítését bemutató példát használatával vezet be.</span><span class="sxs-lookup"><span data-stu-id="d8c2e-106">This document introduces hello service by using an example deployment.</span></span> <span data-ttu-id="d8c2e-107">A dokumentum nem tartalmaz lépésenkénti útmutatót.</span><span class="sxs-lookup"><span data-stu-id="d8c2e-107">This is not a step-by-step guide.</span></span>
> 
> 

## <a name="implement-hello-recommendation"></a><span data-ttu-id="d8c2e-108">Hello javaslat megvalósítása</span><span class="sxs-lookup"><span data-stu-id="d8c2e-108">Implement hello recommendation</span></span>
1. <span data-ttu-id="d8c2e-109">A hello **javaslatok panel**, jelölje be **hárítsa el az Endpoint Protection állapotát riasztások**.</span><span class="sxs-lookup"><span data-stu-id="d8c2e-109">In hello **Recommendations blade**, select **Resolve Endpoint Protection health alerts**.</span></span>
   <span data-ttu-id="d8c2e-110">![Endpoint Protection-állapotriasztások feloldása][1]</span><span class="sxs-lookup"><span data-stu-id="d8c2e-110">![Resolve endpoint protection health alerts][1]</span></span>
2. <span data-ttu-id="d8c2e-111">Ekkor megnyílik hello panel **Endpoint Protection hiba** amely felsorolja azokat a virtuális gépek hibák és hello hibák száma az egyes virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="d8c2e-111">This opens hello blade **Endpoint Protection failure** which lists VMs with failures and hello number of failures for each VM.</span></span> <span data-ttu-id="d8c2e-112">Válassza ki a virtuális gépek hello listából.</span><span class="sxs-lookup"><span data-stu-id="d8c2e-112">Select a VM from hello list.</span></span>
   <span data-ttu-id="d8c2e-113">![Az Endpoint protection hiba][2]</span><span class="sxs-lookup"><span data-stu-id="d8c2e-113">![Endpoint protection failure][2]</span></span>
3. <span data-ttu-id="d8c2e-114">A **hibák lista** megnyílik a hello kiválasztott VM hibák listáját megjelenítő panel.</span><span class="sxs-lookup"><span data-stu-id="d8c2e-114">A **Failures List** blade opens for hello selected VM, displaying a list of failures.</span></span> <span data-ttu-id="d8c2e-115">Válassza ki a hiba hello lista toolearn további.</span><span class="sxs-lookup"><span data-stu-id="d8c2e-115">Select a failure from hello list toolearn more.</span></span> <span data-ttu-id="d8c2e-116">Ekkor megnyílik egy panel kijelölt hello a hibával kapcsolatos információkat.</span><span class="sxs-lookup"><span data-stu-id="d8c2e-116">This opens a blade with information about hello selected failure.</span></span>
   <span data-ttu-id="d8c2e-117">![Hibák lista][3]
   ![esemény][4]</span><span class="sxs-lookup"><span data-stu-id="d8c2e-117">![Failures list][3]
![Failure event][4]</span></span>

## <a name="see-also"></a><span data-ttu-id="d8c2e-118">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="d8c2e-118">See also</span></span>
<span data-ttu-id="d8c2e-119">További információ a Security Center toolearn hello következő lásd:</span><span class="sxs-lookup"><span data-stu-id="d8c2e-119">toolearn more about Security Center, see hello following:</span></span>

* <span data-ttu-id="d8c2e-120">[Biztonsági szabályzatok beállítása az Azure Security Center](security-center-policies.md)– megtudhatja, hogyan tooconfigure biztonsági házirendek az Azure-előfizetések és az erőforráscsoportokat.</span><span class="sxs-lookup"><span data-stu-id="d8c2e-120">[Setting security policies in Azure Security Center](security-center-policies.md)--Learn how tooconfigure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="d8c2e-121">[Biztonsági javaslatok kezelése az Azure Security Centerben](security-center-recommendations.md) – Miként könnyítik meg a javaslatok az Azure-erőforrások védelmét?</span><span class="sxs-lookup"><span data-stu-id="d8c2e-121">[Managing security recommendations in Azure Security Center](security-center-recommendations.md)--Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="d8c2e-122">[Biztonsági állapotfigyelés az Azure Security Center](security-center-monitoring.md)– megtudhatja, hogyan toomonitor hello az Azure-erőforrások állapotát.</span><span class="sxs-lookup"><span data-stu-id="d8c2e-122">[Security health monitoring in Azure Security Center](security-center-monitoring.md)--Learn how toomonitor hello health of your Azure resources.</span></span>
* <span data-ttu-id="d8c2e-123">[Az Azure Security Centerben riasztások kezelése és válaszol toosecurity](security-center-managing-and-responding-alerts.md)– megtudhatja, hogyan toomanage és válaszoljon toosecurity riasztásokat.</span><span class="sxs-lookup"><span data-stu-id="d8c2e-123">[Managing and responding toosecurity alerts in Azure Security Center](security-center-managing-and-responding-alerts.md)--Learn how toomanage and respond toosecurity alerts.</span></span>
* <span data-ttu-id="d8c2e-124">[Partnermegoldások figyelése az Azure Security Center](security-center-partner-solutions.md) – megtudhatja, hogyan toomonitor hello partneri megoldások biztonsági állapotát.</span><span class="sxs-lookup"><span data-stu-id="d8c2e-124">[Monitoring partner solutions with Azure Security Center](security-center-partner-solutions.md) -- Learn how toomonitor hello health status of your partner solutions.</span></span>
* <span data-ttu-id="d8c2e-125">[Azure Security Center: GYIK](security-center-faq.md)– gyakran ismételt kérdések hello szolgáltatás használatával kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="d8c2e-125">[Azure Security Center FAQ](security-center-faq.md)--Find frequently asked questions about using hello service.</span></span>
* <span data-ttu-id="d8c2e-126">[Az Azure biztonsági blog](http://blogs.msdn.com/b/azuresecurity/)– hello legújabb Azure biztonsági hírek és információ lekérése.</span><span class="sxs-lookup"><span data-stu-id="d8c2e-126">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/)--Get hello latest Azure security news and information.</span></span>

<!--Image references-->
[1]: ./media/security-center-resolve-endpoint-protection/resolve-endpoint-protection.png
[2]: ./media/security-center-resolve-endpoint-protection/endpoint-protection-failure.png
[3]: ./media/security-center-resolve-endpoint-protection/failure-list.png
[4]: ./media/security-center-resolve-endpoint-protection/failure-event.png
