---
title: "aaaEnable hálózati biztonsági csoportok az Azure Security Centerben |} Microsoft Docs"
description: "Ez a dokumentum bemutatja, hogyan tooimplement hello Azure Security Center javaslat ** engedélyezése hálózati biztonsági csoportok **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: f53ed853-ffaf-4530-a019-1906ba6f341b
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/2017
ms.author: terrylan
ms.openlocfilehash: 2f70fe432aa452f833a5c322d13102ebbd6dbb69
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enable-network-security-groups-in-azure-security-center"></a><span data-ttu-id="50fb7-103">Engedélyezze a hálózati biztonsági csoportok az Azure Security Centerben</span><span class="sxs-lookup"><span data-stu-id="50fb7-103">Enable Network Security Groups in Azure Security Center</span></span>
<span data-ttu-id="50fb7-104">Az Azure Security Center azt javasolja, hogy a hálózati biztonsági csoport (NSG) engedélyezése, ha egy már nincs engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="50fb7-104">Azure Security Center recommends that you enable a network security group (NSG) if one is not already enabled.</span></span> <span data-ttu-id="50fb7-105">NSG tartalmaz, amelyek engedélyezik vagy megtagadják a hálózati forgalom tooyour Virtuálisgép-példány egy virtuális hálózati hozzáférés-vezérlési lista (ACL) szabályok listáját.</span><span class="sxs-lookup"><span data-stu-id="50fb7-105">NSGs contain a list of Access Control List (ACL) rules that allow or deny network traffic tooyour VM instances in a Virtual Network.</span></span> <span data-ttu-id="50fb7-106">Az NSG-ket alhálózatokhoz vagy az alhálózaton belüli virtuálisgép-példányokhoz lehet hozzárendelni.</span><span class="sxs-lookup"><span data-stu-id="50fb7-106">NSGs can be associated with either subnets or individual VM instances within that subnet.</span></span> <span data-ttu-id="50fb7-107">Amikor egy NSG-t hozzárendelnek egy alhálózathoz, hello ACL szabályok érvényessé válnak tooall hello Virtuálisgép-példány alhálózaton.</span><span class="sxs-lookup"><span data-stu-id="50fb7-107">When an NSG is associated with a subnet, hello ACL rules apply tooall hello VM instances in that subnet.</span></span> <span data-ttu-id="50fb7-108">Ezenkívül forgalom tooan adott virtuális Gépre is lehet korlátozni további korlátozásokat NGS társítása közvetlenül a virtuális gép toothat.</span><span class="sxs-lookup"><span data-stu-id="50fb7-108">In addition, traffic tooan individual VM can be restricted further by associating an NSG directly toothat VM.</span></span> <span data-ttu-id="50fb7-109">több lásd toolearn [Mi az a hálózati biztonsági csoport (NSG)?](../virtual-network/virtual-networks-nsg.md)</span><span class="sxs-lookup"><span data-stu-id="50fb7-109">toolearn more see [What is a Network Security Group (NSG)?](../virtual-network/virtual-networks-nsg.md)</span></span>

<span data-ttu-id="50fb7-110">Ha nem rendelkezik az NSG-k engedélyezve van, a Security Center mutatja be két javaslatok tooyou: engedélyezése hálózati biztonsági csoportok alhálózatok és a hálózati biztonsági csoportok engedélyezése virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="50fb7-110">If you do not have NSGs enabled, Security Center presents two recommendations tooyou: Enable Network Security Groups on subnets and Enable Network Security Groups on virtual machines.</span></span> <span data-ttu-id="50fb7-111">Mely szint, az alhálózati vagy a virtuális gép, tooapply NSG-k választja.</span><span class="sxs-lookup"><span data-stu-id="50fb7-111">You choose which level, subnet or VM, tooapply NSGs.</span></span>

> [!NOTE]
> <span data-ttu-id="50fb7-112">Ez a dokumentum hello szolgáltatás telepítését bemutató példát használatával vezet be.</span><span class="sxs-lookup"><span data-stu-id="50fb7-112">This document introduces hello service by using an example deployment.</span></span>  <span data-ttu-id="50fb7-113">A dokumentum nem tartalmaz lépésenkénti útmutatót.</span><span class="sxs-lookup"><span data-stu-id="50fb7-113">This is not a step-by-step guide.</span></span>
>
>

## <a name="implement-hello-recommendation"></a><span data-ttu-id="50fb7-114">Hello javaslat megvalósítása</span><span class="sxs-lookup"><span data-stu-id="50fb7-114">Implement hello recommendation</span></span>
1. <span data-ttu-id="50fb7-115">A hello **javaslatok** panelen válassza **engedélyezze a hálózati biztonsági csoportok** alhálózatok vagy virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="50fb7-115">In hello **Recommendations** blade, select **Enable Network Security Groups** on subnets or on virtual machines.</span></span>
   <span data-ttu-id="50fb7-116">![Hálózati biztonsági csoportok engedélyezése][1]</span><span class="sxs-lookup"><span data-stu-id="50fb7-116">![Enable Network Security Groups][1]</span></span>
2. <span data-ttu-id="50fb7-117">Ekkor megnyílik hello panel **hiányzó hálózati biztonsági csoportok beállítása** alhálózatok vagy virtuális gépekhez, attól függően, hogy a kiválasztott hello javaslat.</span><span class="sxs-lookup"><span data-stu-id="50fb7-117">This opens hello blade **Configure Missing Network Security Groups** for subnets or for virtual machines, depending on hello recommendation that you selected.</span></span> <span data-ttu-id="50fb7-118">Válassza ki egy alhálózatot vagy egy virtuális gép tooconfigure egy NSG-t a.</span><span class="sxs-lookup"><span data-stu-id="50fb7-118">Select a subnet or a virtual machine tooconfigure an NSG on.</span></span>

   ![NSG alhálózat konfigurálása][2]

   ![NSG a virtuális gép konfigurálása][3]
3. <span data-ttu-id="50fb7-121">A hello **válassza a hálózati biztonsági csoport** panelen, jelöljön ki egy meglévő NSG vagy **hozzon létre új** toocreate egy NSG.</span><span class="sxs-lookup"><span data-stu-id="50fb7-121">On hello **Choose network security group** blade, select an existing NSG or select **Create new** toocreate an NSG.</span></span>

   ![Hálózati biztonsági csoport választása][4]

<span data-ttu-id="50fb7-123">Ha létrehoz egy NSG-t, kövesse hello [hogyan toomanage NSG-k használatával hello Azure-portálon](../virtual-network/virtual-networks-create-nsg-arm-pportal.md) toocreate egy NSG-t és a biztonsági szabályokat állíthat be.</span><span class="sxs-lookup"><span data-stu-id="50fb7-123">If you create an NSG, follow hello steps in [How toomanage NSGs using hello Azure portal](../virtual-network/virtual-networks-create-nsg-arm-pportal.md) toocreate an NSG and set security rules.</span></span>

## <a name="see-also"></a><span data-ttu-id="50fb7-124">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="50fb7-124">See also</span></span>
<span data-ttu-id="50fb7-125">Ez a cikk bemutatta, hogyan tooimplement hello Security Center ajánlás "engedélyezése hálózati biztonsági csoportok" alhálózathoz vagy a virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="50fb7-125">This article showed you how tooimplement hello Security Center recommendation "Enable Network Security Groups" for subnets or virtual machines.</span></span> <span data-ttu-id="50fb7-126">További információ az NSG-ket, engedélyezése toolearn hello következő lásd:</span><span class="sxs-lookup"><span data-stu-id="50fb7-126">toolearn more about enabling NSGs, see hello following:</span></span>

* [<span data-ttu-id="50fb7-127">Mi az a hálózati biztonsági csoport (NSG)?</span><span class="sxs-lookup"><span data-stu-id="50fb7-127">What is a Network Security Group (NSG)?</span></span>](../virtual-network/virtual-networks-nsg.md)
* [<span data-ttu-id="50fb7-128">Hogyan toomanage NSG-k használatával hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="50fb7-128">How toomanage NSGs using hello Azure portal</span></span>](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)

<span data-ttu-id="50fb7-129">További információ a Security Center toolearn hello következő lásd:</span><span class="sxs-lookup"><span data-stu-id="50fb7-129">toolearn more about Security Center, see hello following:</span></span>

* <span data-ttu-id="50fb7-130">[Biztonsági szabályzatok beállítása az Azure Security Center](security-center-policies.md) – megtudhatja, hogyan tooconfigure biztonsági házirendek az Azure-előfizetések és az erőforráscsoportokat.</span><span class="sxs-lookup"><span data-stu-id="50fb7-130">[Setting security policies in Azure Security Center](security-center-policies.md) -- Learn how tooconfigure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="50fb7-131">[Biztonsági javaslatok kezelése az Azure Security Center](security-center-recommendations.md) – megtudhatja, miként könnyítik meg a javaslatok az Azure-erőforrások védelme.</span><span class="sxs-lookup"><span data-stu-id="50fb7-131">[Managing security recommendations in Azure Security Center](security-center-recommendations.md) -- Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="50fb7-132">[Biztonsági állapotfigyelés az Azure Security Center](security-center-monitoring.md) – megtudhatja, hogyan toomonitor hello az Azure-erőforrások állapotát.</span><span class="sxs-lookup"><span data-stu-id="50fb7-132">[Security health monitoring in Azure Security Center](security-center-monitoring.md) -- Learn how toomonitor hello health of your Azure resources.</span></span>
* <span data-ttu-id="50fb7-133">[Az Azure Security Centerben riasztások kezelése és válaszol toosecurity](security-center-managing-and-responding-alerts.md) – megtudhatja, hogyan toomanage és válaszoljon toosecurity riasztásokat.</span><span class="sxs-lookup"><span data-stu-id="50fb7-133">[Managing and responding toosecurity alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) -- Learn how toomanage and respond toosecurity alerts.</span></span>
* <span data-ttu-id="50fb7-134">[Partnermegoldások figyelése az Azure Security Center](security-center-partner-solutions.md) – megtudhatja, hogyan toomonitor hello partneri megoldások biztonsági állapotát.</span><span class="sxs-lookup"><span data-stu-id="50fb7-134">[Monitoring partner solutions with Azure Security Center](security-center-partner-solutions.md) -- Learn how toomonitor hello health status of your partner solutions.</span></span>
* <span data-ttu-id="50fb7-135">[Azure Security Center: GYIK](security-center-faq.md) – gyakran ismételt kérdések hello szolgáltatás használatával kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="50fb7-135">[Azure Security Center FAQ](security-center-faq.md) -- Find frequently asked questions about using hello service.</span></span>
* <span data-ttu-id="50fb7-136">[Az Azure biztonsági blog](http://blogs.msdn.com/b/azuresecurity/) – hello legújabb Azure biztonsági hírek és információ lekérése.</span><span class="sxs-lookup"><span data-stu-id="50fb7-136">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) -- Get hello latest Azure security news and information.</span></span>

<!--Image references-->
[1]: ./media/security-center-enable-nsg/enable-nsg.png
[2]:./media/security-center-enable-nsg/configure-nsg-for-subnet.png
[3]: ./media/security-center-enable-nsg/configure-nsg-for-vm.png
[4]: ./media/security-center-enable-nsg/choose-nsg.png
