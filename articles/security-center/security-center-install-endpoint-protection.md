---
title: aaaInstall Endpoint Protection az Azure Security Centerben |} Microsoft Docs
description: "Ez a dokumentum bemutatja, hogyan tooimplement hello Azure Security Center javaslat ** telepíteni az Endpoint Protection **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 1599ad5f-d810-421d-aafc-892e831b403f
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: terrylan
ms.openlocfilehash: fea891810e042e4d4f6e55094c0cd4de013ba68a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="install-endpoint-protection-in-azure-security-center"></a><span data-ttu-id="7e494-103">Az Endpoint Protection telepítése az Azure Security Centerben</span><span class="sxs-lookup"><span data-stu-id="7e494-103">Install Endpoint Protection in Azure Security Center</span></span>
<span data-ttu-id="7e494-104">Az Azure Security Center azt javasolja, hogy az endpoint protection az Azure virtuális gépek (VM) Ha az endpoint protection nincs engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="7e494-104">Azure Security Center recommends that you install endpoint protection on your Azure virtual machines (VMs) if endpoint protection is not already enabled.</span></span> <span data-ttu-id="7e494-105">Ez a javaslat csak virtuális gépek tooWindows vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="7e494-105">This recommendation applies tooWindows VMs only.</span></span>

> [!NOTE]
> <span data-ttu-id="7e494-106">Telepítési példában a Microsoft Antimalware használja.</span><span class="sxs-lookup"><span data-stu-id="7e494-106">This example deployment uses Microsoft Antimalware.</span></span> <span data-ttu-id="7e494-107">Lásd: [Partner integrálása az Azure Security Center](security-center-partner-integration.md#partners-that-integrate-with-security-center) partnerek listáját a Security Center integrálva.</span><span class="sxs-lookup"><span data-stu-id="7e494-107">See [Partner Integration in Azure Security Center](security-center-partner-integration.md#partners-that-integrate-with-security-center) for a list of partners integrated with Security Center.</span></span>  
>
>

## <a name="implement-hello-recommendation"></a><span data-ttu-id="7e494-108">Hello javaslat megvalósítása</span><span class="sxs-lookup"><span data-stu-id="7e494-108">Implement hello recommendation</span></span>

> [!NOTE]
> <span data-ttu-id="7e494-109">Ez a dokumentum hello szolgáltatás telepítését bemutató példát használatával vezet be.</span><span class="sxs-lookup"><span data-stu-id="7e494-109">This document introduces hello service by using an example deployment.</span></span>  <span data-ttu-id="7e494-110">Ez a dokumentum nem tartalmaz lépésenkénti útmutatót.</span><span class="sxs-lookup"><span data-stu-id="7e494-110">This document is not a step-by-step guide.</span></span>
>
>

1. <span data-ttu-id="7e494-111">A hello **javaslatok** panelen válassza **Endpoint Protection telepítése**.</span><span class="sxs-lookup"><span data-stu-id="7e494-111">In hello **Recommendations** blade, select **Install Endpoint Protection**.</span></span>
   <span data-ttu-id="7e494-112">![Válassza ki az Endpoint Protection telepítése][1]</span><span class="sxs-lookup"><span data-stu-id="7e494-112">![Select Install Endpoint Protection][1]</span></span>
2. <span data-ttu-id="7e494-113">Hello **Endpoint Protection telepítése** panel nyílik meg, az endpoint protection nélkül a virtuális gépek listáját.</span><span class="sxs-lookup"><span data-stu-id="7e494-113">hello **Install Endpoint Protection** blade opens displaying a list of VMs without endpoint protection.</span></span> <span data-ttu-id="7e494-114">Kiválaszthatja, hello lista hello a kívánt tooinstall az endpoint protection, és kattintson a virtuális gépek **telepítse a virtuális gépek**.</span><span class="sxs-lookup"><span data-stu-id="7e494-114">Select from hello list hello VMs that you want tooinstall endpoint protection on and click **Install on VMs**.</span></span>
   <span data-ttu-id="7e494-115">![Válassza ki a virtuális gépek tooinstall Endpoint Protection a][2]</span><span class="sxs-lookup"><span data-stu-id="7e494-115">![Select VMs tooinstall Endpoint Protection on][2]</span></span>
3. <span data-ttu-id="7e494-116">Hello **válassza ki az Endpoint Protection** panel megnyitása tooallow meg tooselect hello végpontvédelmi megoldás toouse szeretné.</span><span class="sxs-lookup"><span data-stu-id="7e494-116">hello **Select Endpoint Protection** blade opens tooallow you tooselect hello endpoint protection solution you want toouse.</span></span> <span data-ttu-id="7e494-117">Ebben a példában most válasszon **Microsoft Antimalware**.</span><span class="sxs-lookup"><span data-stu-id="7e494-117">In this example, let's select **Microsoft Antimalware**.</span></span>
   <span data-ttu-id="7e494-118">![Válassza ki az Endpoint Protection][3]</span><span class="sxs-lookup"><span data-stu-id="7e494-118">![Select Endpoint Protection][3]</span></span>
4. <span data-ttu-id="7e494-119">Hello végpontvédelmi megoldás kapcsolatos további információk jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="7e494-119">Additional information about hello endpoint protection solution is displayed.</span></span> <span data-ttu-id="7e494-120">Kattintson a **Létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="7e494-120">Select **Create**.</span></span>
   <span data-ttu-id="7e494-121">![Kártevőirtó megoldás létrehozása][4]</span><span class="sxs-lookup"><span data-stu-id="7e494-121">![Create antimalware solution][4]</span></span>
5. <span data-ttu-id="7e494-122">Adja meg a szükséges hello konfigurációs beállításokat a hello **hozzáadása bővítmény** panelt, és válassza **OK**.</span><span class="sxs-lookup"><span data-stu-id="7e494-122">Enter hello required configuration settings on hello **Add Extension** blade, and then select **OK**.</span></span> <span data-ttu-id="7e494-123">toolearn hello konfigurációs beállításokkal, kapcsolatos további információkért lásd: [alapértelmezett és egyéni kártevőirtó-konfiguráció](../security/azure-security-antimalware.md#default-and-custom-antimalware-configuration).</span><span class="sxs-lookup"><span data-stu-id="7e494-123">toolearn more about hello configuration settings, see [Default and Custom Antimalware Configuration](../security/azure-security-antimalware.md#default-and-custom-antimalware-configuration).</span></span>

<span data-ttu-id="7e494-124">[A Microsoft Antimalware](../security/azure-security-antimalware.md) mostantól aktív hello a kijelölt virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="7e494-124">[Microsoft Antimalware](../security/azure-security-antimalware.md) is now active on hello selected VMs.</span></span>

## <a name="see-also"></a><span data-ttu-id="7e494-125">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="7e494-125">See also</span></span>
<span data-ttu-id="7e494-126">Ez a cikk bemutatta, hogyan tooimplement hello Security Center ajánlás "Az Endpoint Protection telepítése."</span><span class="sxs-lookup"><span data-stu-id="7e494-126">This article showed you how tooimplement hello Security Center recommendation "Install Endpoint Protection."</span></span> <span data-ttu-id="7e494-127">További információ az Azure, a Microsoft Antimalware engedélyezése toolearn lásd: a következő dokumentum hello:</span><span class="sxs-lookup"><span data-stu-id="7e494-127">toolearn more about enabling Microsoft Antimalware in Azure, see hello following document:</span></span>

* <span data-ttu-id="7e494-128">[A Microsoft Antimalware Felhőszolgáltatásokhoz és virtuális gépek](../security/azure-security-antimalware.md) – megtudhatja, hogyan toodeploy Microsoft Antimalware.</span><span class="sxs-lookup"><span data-stu-id="7e494-128">[Microsoft Antimalware for Cloud Services and Virtual Machines](../security/azure-security-antimalware.md) -- Learn how toodeploy Microsoft Antimalware.</span></span>

<span data-ttu-id="7e494-129">További információ a Security Center toolearn tekintse meg a következő dokumentumok hello:</span><span class="sxs-lookup"><span data-stu-id="7e494-129">toolearn more about Security Center, see hello following documents:</span></span>

* <span data-ttu-id="7e494-130">[Biztonsági szabályzatok beállítása az Azure Security Center](security-center-policies.md) – megtudhatja, hogyan tooconfigure biztonsági házirendeket.</span><span class="sxs-lookup"><span data-stu-id="7e494-130">[Setting security policies in Azure Security Center](security-center-policies.md) -- Learn how tooconfigure security policies.</span></span>
* <span data-ttu-id="7e494-131">[Biztonsági javaslatok kezelése az Azure Security Center](security-center-recommendations.md) – megtudhatja, miként könnyítik meg a javaslatok az Azure-erőforrások védelme.</span><span class="sxs-lookup"><span data-stu-id="7e494-131">[Managing security recommendations in Azure Security Center](security-center-recommendations.md) -- Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="7e494-132">[Biztonsági állapotfigyelés az Azure Security Center](security-center-monitoring.md) – megtudhatja, hogyan toomonitor hello az Azure-erőforrások állapotát.</span><span class="sxs-lookup"><span data-stu-id="7e494-132">[Security health monitoring in Azure Security Center](security-center-monitoring.md) -- Learn how toomonitor hello health of your Azure resources.</span></span>
* <span data-ttu-id="7e494-133">[Az Azure Security Centerben riasztások kezelése és válaszol toosecurity](security-center-managing-and-responding-alerts.md) – megtudhatja, hogyan toomanage és válaszoljon toosecurity riasztásokat.</span><span class="sxs-lookup"><span data-stu-id="7e494-133">[Managing and responding toosecurity alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) -- Learn how toomanage and respond toosecurity alerts.</span></span>
* <span data-ttu-id="7e494-134">[Partnermegoldások figyelése az Azure Security Center](security-center-partner-solutions.md) – megtudhatja, hogyan toomonitor hello partneri megoldások biztonsági állapotát.</span><span class="sxs-lookup"><span data-stu-id="7e494-134">[Monitoring partner solutions with Azure Security Center](security-center-partner-solutions.md) -- Learn how toomonitor hello health status of your partner solutions.</span></span>
* <span data-ttu-id="7e494-135">[Azure Security Center: GYIK](security-center-faq.md) – gyakran ismételt kérdések hello szolgáltatás használatával kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="7e494-135">[Azure Security Center FAQ](security-center-faq.md) -- Find frequently asked questions about using hello service.</span></span>
* <span data-ttu-id="7e494-136">[Az Azure biztonsági blog](http://blogs.msdn.com/b/azuresecurity/) – blogbejegyzések az Azure biztonsági és megfelelőségi funkcióiról.</span><span class="sxs-lookup"><span data-stu-id="7e494-136">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) -- Find blog posts about Azure security and compliance.</span></span>

<!--Image references-->
[1]:./media/security-center-install-endpoint-protection/select-install-endpoint-protection.png
[2]:./media/security-center-install-endpoint-protection/install-endpoint-protection-blade.png
[3]:./media/security-center-install-endpoint-protection/select-endpoint-protection.png
[4]:./media/security-center-install-endpoint-protection/create-antimalware-solution.png
