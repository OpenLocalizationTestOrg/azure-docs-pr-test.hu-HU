---
title: "aaaUpdate az operációs rendszer verziója, az Azure Security Centerben |} Microsoft Docs"
description: "Ez a cikk bemutatja, hogyan tooimplement hello Azure Security Center javaslat ** frissítés operációs rendszer verziója **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: aa372492-ecdb-4368-8fdd-d8ed31e216ee
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/01/2016
ms.author: terrylan
ms.openlocfilehash: f3497d7266da40d3a965f9cb8e84392d2aaa28f0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="update-os-version-in-azure-security-center"></a><span data-ttu-id="007f4-103">Frissítse az operációs rendszer verziója, az Azure Security Centerben</span><span class="sxs-lookup"><span data-stu-id="007f4-103">Update OS version in Azure Security Center</span></span>
<span data-ttu-id="007f4-104">A virtuális gépek (VM), a felhőalapú szolgáltatások Azure Security Center javasolni fogja, hogy hello operációs rendszer lesznek frissítve, ha van újabb verzió.</span><span class="sxs-lookup"><span data-stu-id="007f4-104">For virtual machines (VMs) in cloud services, Azure Security Center will recommend that hello operating system (OS) be updated if there is a more recent version available.</span></span>  <span data-ttu-id="007f4-105">Csak felhőalapú szolgáltatások üzembe helyezési ponti figyelt éles környezetben futó webes és feldolgozói szerepköröket.</span><span class="sxs-lookup"><span data-stu-id="007f4-105">Only cloud services web and worker roles running in production slots are monitored.</span></span>

> [!NOTE]
> <span data-ttu-id="007f4-106">Ez a dokumentum hello szolgáltatás telepítését bemutató példát használatával vezet be.</span><span class="sxs-lookup"><span data-stu-id="007f4-106">This document introduces hello service by using an example deployment.</span></span>  <span data-ttu-id="007f4-107">A dokumentum nem tartalmaz lépésenkénti útmutatót.</span><span class="sxs-lookup"><span data-stu-id="007f4-107">This is not a step-by-step guide.</span></span>
> 
> 

## <a name="implement-hello-recommendation"></a><span data-ttu-id="007f4-108">Hello javaslat megvalósítása</span><span class="sxs-lookup"><span data-stu-id="007f4-108">Implement hello recommendation</span></span>
1. <span data-ttu-id="007f4-109">A hello **javaslatok** panelen válassza **frissítés operációsrendszer-verzió**.</span><span class="sxs-lookup"><span data-stu-id="007f4-109">In hello **Recommendations** blade, select **Update OS version**.</span></span>
   <span data-ttu-id="007f4-110">![Operációs rendszer verziójának frissítése][1]</span><span class="sxs-lookup"><span data-stu-id="007f4-110">![Update OS version][1]</span></span>
2. <span data-ttu-id="007f4-111">Ekkor megnyílik hello panel **frissítés operációsrendszer-verzió**.</span><span class="sxs-lookup"><span data-stu-id="007f4-111">This opens hello blade **Update OS version**.</span></span> <span data-ttu-id="007f4-112">Hello kövesse a panel tooupdate hello operációs rendszer verziója.</span><span class="sxs-lookup"><span data-stu-id="007f4-112">Follow hello steps in this blade tooupdate hello OS version.</span></span>

## <a name="see-also"></a><span data-ttu-id="007f4-113">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="007f4-113">See also</span></span>
<span data-ttu-id="007f4-114">Ez a cikk bemutatta, hogyan tooimplement hello Security Center ajánlás "frissítés az operációs rendszer verzió."</span><span class="sxs-lookup"><span data-stu-id="007f4-114">This article showed you how tooimplement hello Security Center recommendation "Update OS version."</span></span> <span data-ttu-id="007f4-115">További információ a felhőalapú szolgáltatások és a frissítési hello operációs rendszerének verziója egy felhőalapú szolgáltatás toolearn lásd:</span><span class="sxs-lookup"><span data-stu-id="007f4-115">toolearn more about cloud services and updating hello OS version for a cloud service, see:</span></span>

* [<span data-ttu-id="007f4-116">A Cloud Services áttekintése</span><span class="sxs-lookup"><span data-stu-id="007f4-116">Cloud Services overview</span></span>](../cloud-services/cloud-services-choose-me.md)
* [<span data-ttu-id="007f4-117">Hogyan tooupdate egy felhőalapú szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="007f4-117">How tooupdate a cloud service</span></span>](../cloud-services/cloud-services-update-azure-service.md)
* [<span data-ttu-id="007f4-118">TooConfigure Cloud Services</span><span class="sxs-lookup"><span data-stu-id="007f4-118">How tooConfigure Cloud Services</span></span>](../cloud-services/cloud-services-how-to-configure-portal.md)

<span data-ttu-id="007f4-119">További információ a Security Center toolearn hello következő lásd:</span><span class="sxs-lookup"><span data-stu-id="007f4-119">toolearn more about Security Center, see hello following:</span></span>

* <span data-ttu-id="007f4-120">[Biztonsági szabályzatok beállítása az Azure Security Center](security-center-policies.md) – megtudhatja, hogyan tooconfigure biztonsági házirendek az Azure-előfizetések és az erőforráscsoportokat.</span><span class="sxs-lookup"><span data-stu-id="007f4-120">[Setting security policies in Azure Security Center](security-center-policies.md) -- Learn how tooconfigure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="007f4-121">[Biztonsági javaslatok kezelése az Azure Security Center](security-center-recommendations.md) – megtudhatja, miként könnyítik meg a javaslatok az Azure-erőforrások védelme.</span><span class="sxs-lookup"><span data-stu-id="007f4-121">[Managing security recommendations in Azure Security Center](security-center-recommendations.md) -- Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="007f4-122">[Biztonsági állapotfigyelés az Azure Security Center](security-center-monitoring.md) – megtudhatja, hogyan toomonitor hello az Azure-erőforrások állapotát.</span><span class="sxs-lookup"><span data-stu-id="007f4-122">[Security health monitoring in Azure Security Center](security-center-monitoring.md) -- Learn how toomonitor hello health of your Azure resources.</span></span>
* <span data-ttu-id="007f4-123">[Az Azure Security Centerben riasztások kezelése és válaszol toosecurity](security-center-managing-and-responding-alerts.md) – megtudhatja, hogyan toomanage és válaszoljon toosecurity riasztásokat.</span><span class="sxs-lookup"><span data-stu-id="007f4-123">[Managing and responding toosecurity alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) -- Learn how toomanage and respond toosecurity alerts.</span></span>
* <span data-ttu-id="007f4-124">[Partnermegoldások figyelése az Azure Security Center](security-center-partner-solutions.md) – megtudhatja, hogyan toomonitor hello partneri megoldások biztonsági állapotát.</span><span class="sxs-lookup"><span data-stu-id="007f4-124">[Monitoring partner solutions with Azure Security Center](security-center-partner-solutions.md) -- Learn how toomonitor hello health status of your partner solutions.</span></span>
* <span data-ttu-id="007f4-125">[Azure Security Center: GYIK](security-center-faq.md) – gyakran ismételt kérdések hello szolgáltatás használatával kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="007f4-125">[Azure Security Center FAQ](security-center-faq.md) -- Find frequently asked questions about using hello service.</span></span>
* <span data-ttu-id="007f4-126">[Az Azure biztonsági blog](http://blogs.msdn.com/b/azuresecurity/) – hello legújabb Azure biztonsági hírek és információ lekérése.</span><span class="sxs-lookup"><span data-stu-id="007f4-126">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) -- Get hello latest Azure security news and information.</span></span>

<!--Image references-->
[1]: ./media/security-center-update-os-version/update-os-version.png
