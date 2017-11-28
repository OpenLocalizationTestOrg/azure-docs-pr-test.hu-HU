---
title: "aaaEnable Virtuálisgép-ügynök az Azure Security Centerben |} Microsoft Docs"
description: "Ez a dokumentum bemutatja, hogyan tooimplement hello Azure Security Center javaslat ** engedélyezése VM ügynök **."
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
ms.openlocfilehash: 9bd71e638b020780537da25fd4cf7baf34d3e11a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enable-vm-agent-in-azure-security-center"></a><span data-ttu-id="fab2a-103">Az Azure Security Centerben Virtuálisgép-ügynök engedélyezése</span><span class="sxs-lookup"><span data-stu-id="fab2a-103">Enable VM Agent in Azure Security Center</span></span>
<span data-ttu-id="fab2a-104">hello VM ügynököt telepíteni kell a virtuális gépek (VM) sorrendben túl[az adatgyűjtést](security-center-enable-data-collection.md).</span><span class="sxs-lookup"><span data-stu-id="fab2a-104">hello VM Agent must be installed on virtual machines (VMs) in order too[enable data collection](security-center-enable-data-collection.md).</span></span>  <span data-ttu-id="fab2a-105">Az Azure Security Center lehetővé teszi, hogy Ön toosee virtuális gépek igénylő hello Virtuálisgép-ügynök, és azt javasolja, hogy engedélyezze a virtuális gépek Virtuálisgép-ügynök hello.</span><span class="sxs-lookup"><span data-stu-id="fab2a-105">Azure Security Center enables you toosee which VMs require hello VM Agent and will recommend that you enable hello VM Agent on those VMs.</span></span>

<span data-ttu-id="fab2a-106">Virtuálisgép-ügynök hello alapértelmezés szerint telepítve van a hello Azure Piactérről származó központilag telepített virtuális gépekhez.</span><span class="sxs-lookup"><span data-stu-id="fab2a-106">hello VM Agent is installed by default for VMs that are deployed from hello Azure Marketplace.</span></span> <span data-ttu-id="fab2a-107">hello cikk [ügynök és Virtuálisgép-bővítmények – 2. rész](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-2/) bemutatja, hogyan tooinstall hello Virtuálisgép-ügynök.</span><span class="sxs-lookup"><span data-stu-id="fab2a-107">hello article [VM Agent and Extensions – Part 2](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-2/) provides information on how tooinstall hello VM Agent.</span></span>

> [!NOTE]
> <span data-ttu-id="fab2a-108">Ez a dokumentum hello szolgáltatás telepítését bemutató példát használatával vezet be.</span><span class="sxs-lookup"><span data-stu-id="fab2a-108">This document introduces hello service by using an example deployment.</span></span> <span data-ttu-id="fab2a-109">A dokumentum nem tartalmaz lépésenkénti útmutatót.</span><span class="sxs-lookup"><span data-stu-id="fab2a-109">This is not a step-by-step guide.</span></span>
>
>

## <a name="implement-hello-recommendation"></a><span data-ttu-id="fab2a-110">Hello javaslat megvalósítása</span><span class="sxs-lookup"><span data-stu-id="fab2a-110">Implement hello recommendation</span></span>
1. <span data-ttu-id="fab2a-111">A hello **javaslatok panel**, jelölje be **Virtuálisgép-ügynök engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="fab2a-111">In hello **Recommendations blade**, select **Enable VM Agent**.</span></span>
   <span data-ttu-id="fab2a-112">![Virtuálisgép-ügynök engedélyezése][1]</span><span class="sxs-lookup"><span data-stu-id="fab2a-112">![Enable VM Agent][1]</span></span>
2. <span data-ttu-id="fab2a-113">Ekkor megnyílik hello panel **VM hiányzik vagy nem válaszol**.</span><span class="sxs-lookup"><span data-stu-id="fab2a-113">This opens hello blade **VM Agent Is Missing Or Not Responding**.</span></span> <span data-ttu-id="fab2a-114">Ezen a panelen hello hello Virtuálisgép-ügynök igénylő virtuális gépek sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="fab2a-114">This blade lists hello VMs that require hello VM Agent.</span></span> <span data-ttu-id="fab2a-115">Hello utasításokat kövesse hello panel tooinstall hello Virtuálisgép-ügynök.</span><span class="sxs-lookup"><span data-stu-id="fab2a-115">Follow hello instructions on hello blade tooinstall hello VM agent.</span></span>
   <span data-ttu-id="fab2a-116">![Hiányzik a Virtuálisgép-ügynök][2]</span><span class="sxs-lookup"><span data-stu-id="fab2a-116">![VM Agent is missing][2]</span></span>

## <a name="see-also"></a><span data-ttu-id="fab2a-117">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="fab2a-117">See also</span></span>
<span data-ttu-id="fab2a-118">További információ a Security Center toolearn hello következő lásd:</span><span class="sxs-lookup"><span data-stu-id="fab2a-118">toolearn more about Security Center, see hello following:</span></span>

* <span data-ttu-id="fab2a-119">[Biztonsági szabályzatok beállítása az Azure Security Center](security-center-policies.md)– megtudhatja, hogyan tooconfigure biztonsági házirendek az Azure-előfizetések és az erőforráscsoportokat.</span><span class="sxs-lookup"><span data-stu-id="fab2a-119">[Setting security policies in Azure Security Center](security-center-policies.md)--Learn how tooconfigure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="fab2a-120">[Biztonsági javaslatok kezelése az Azure Security Centerben](security-center-recommendations.md) – Miként könnyítik meg a javaslatok az Azure-erőforrások védelmét?</span><span class="sxs-lookup"><span data-stu-id="fab2a-120">[Managing security recommendations in Azure Security Center](security-center-recommendations.md)--Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="fab2a-121">[Biztonsági állapotfigyelés az Azure Security Center](security-center-monitoring.md)– megtudhatja, hogyan toomonitor hello az Azure-erőforrások állapotát.</span><span class="sxs-lookup"><span data-stu-id="fab2a-121">[Security health monitoring in Azure Security Center](security-center-monitoring.md)--Learn how toomonitor hello health of your Azure resources.</span></span>
* <span data-ttu-id="fab2a-122">[Az Azure Security Centerben riasztások kezelése és válaszol toosecurity](security-center-managing-and-responding-alerts.md)– megtudhatja, hogyan toomanage és válaszoljon toosecurity riasztásokat.</span><span class="sxs-lookup"><span data-stu-id="fab2a-122">[Managing and responding toosecurity alerts in Azure Security Center](security-center-managing-and-responding-alerts.md)--Learn how toomanage and respond toosecurity alerts.</span></span>
* <span data-ttu-id="fab2a-123">[Partnermegoldások figyelése az Azure Security Center](security-center-partner-solutions.md) – megtudhatja, hogyan toomonitor hello partneri megoldások biztonsági állapotát.</span><span class="sxs-lookup"><span data-stu-id="fab2a-123">[Monitoring partner solutions with Azure Security Center](security-center-partner-solutions.md) -- Learn how toomonitor hello health status of your partner solutions.</span></span>
* <span data-ttu-id="fab2a-124">[Azure Security Center: GYIK](security-center-faq.md)– gyakran ismételt kérdések hello szolgáltatás használatával kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="fab2a-124">[Azure Security Center FAQ](security-center-faq.md)--Find frequently asked questions about using hello service.</span></span>
* <span data-ttu-id="fab2a-125">[Az Azure biztonsági blog](http://blogs.msdn.com/b/azuresecurity/)– hello legújabb Azure biztonsági hírek és információ lekérése.</span><span class="sxs-lookup"><span data-stu-id="fab2a-125">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/)--Get hello latest Azure security news and information.</span></span>

<!--Image references-->
[1]: ./media/security-center-enable-vm-agent/enable-vm-agent.png
[2]: ./media/security-center-enable-vm-agent/vm-agent-is-missing.png
