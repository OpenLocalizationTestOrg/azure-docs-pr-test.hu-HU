---
title: "Engedélyezze a hálózati biztonsági csoportok az Azure Security Centerben |} Microsoft Docs"
description: "Ez a dokumentum azt ismerteti, hogyan valósítja meg az Azure Security Center ajánlás ** engedélyezése hálózati biztonsági csoportok **."
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
ms.openlocfilehash: 1e034d59d8847f237fa0d4c772344d45cd618576
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="enable-network-security-groups-in-azure-security-center"></a><span data-ttu-id="f363b-103">Engedélyezze a hálózati biztonsági csoportok az Azure Security Centerben</span><span class="sxs-lookup"><span data-stu-id="f363b-103">Enable Network Security Groups in Azure Security Center</span></span>
<span data-ttu-id="f363b-104">Az Azure Security Center azt javasolja, hogy a hálózati biztonsági csoport (NSG) engedélyezése, ha egy már nincs engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="f363b-104">Azure Security Center recommends that you enable a network security group (NSG) if one is not already enabled.</span></span> <span data-ttu-id="f363b-105">NSG tartalmaz, amelyek engedélyezik vagy megtagadják a Virtuálisgép-példány egy virtuális hálózat hálózati forgalmának hozzáférés-vezérlési lista (ACL) szabályok listáját.</span><span class="sxs-lookup"><span data-stu-id="f363b-105">NSGs contain a list of Access Control List (ACL) rules that allow or deny network traffic to your VM instances in a Virtual Network.</span></span> <span data-ttu-id="f363b-106">Az NSG-ket alhálózatokhoz vagy az alhálózaton belüli virtuálisgép-példányokhoz lehet hozzárendelni.</span><span class="sxs-lookup"><span data-stu-id="f363b-106">NSGs can be associated with either subnets or individual VM instances within that subnet.</span></span> <span data-ttu-id="f363b-107">Ha az NSG-t hozzárendelik egy alhálózathoz, az ACL-szabályok érvényesek lesznek az alhálózatban lévő összes virtuálisgép-példányra.</span><span class="sxs-lookup"><span data-stu-id="f363b-107">When an NSG is associated with a subnet, the ACL rules apply to all the VM instances in that subnet.</span></span> <span data-ttu-id="f363b-108">Emellett az egyes virtuális gép is lehet korlátozni további korlátozásokat NGS társítása közvetlenül a virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="f363b-108">In addition, traffic to an individual VM can be restricted further by associating an NSG directly to that VM.</span></span> <span data-ttu-id="f363b-109">További részletek további [Mi az a hálózati biztonsági csoport (NSG)?](../virtual-network/virtual-networks-nsg.md)</span><span class="sxs-lookup"><span data-stu-id="f363b-109">To learn more see [What is a Network Security Group (NSG)?](../virtual-network/virtual-networks-nsg.md)</span></span>

<span data-ttu-id="f363b-110">Ha nem rendelkezik az NSG-k engedélyezve van, a Security Center ajánlásokat két Önnek: engedélyezése hálózati biztonsági csoportok alhálózatok és a hálózati biztonsági csoportok engedélyezése virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="f363b-110">If you do not have NSGs enabled, Security Center presents two recommendations to you: Enable Network Security Groups on subnets and Enable Network Security Groups on virtual machines.</span></span> <span data-ttu-id="f363b-111">Mely szint, az alhálózati vagy a virtuális Gépet, alkalmazza az NSG-k választja.</span><span class="sxs-lookup"><span data-stu-id="f363b-111">You choose which level, subnet or VM, to apply NSGs.</span></span>

> [!NOTE]
> <span data-ttu-id="f363b-112">Ez a dokumentum egy üzembe helyezést szemléltető példa segítségével mutatja be a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="f363b-112">This document introduces the service by using an example deployment.</span></span>  <span data-ttu-id="f363b-113">A dokumentum nem tartalmaz lépésenkénti útmutatót.</span><span class="sxs-lookup"><span data-stu-id="f363b-113">This is not a step-by-step guide.</span></span>
>
>

## <a name="implement-the-recommendation"></a><span data-ttu-id="f363b-114">A javaslat megvalósítása</span><span class="sxs-lookup"><span data-stu-id="f363b-114">Implement the recommendation</span></span>
1. <span data-ttu-id="f363b-115">Az a **javaslatok** panelen válassza **engedélyezze a hálózati biztonsági csoportok** alhálózatok vagy virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="f363b-115">In the **Recommendations** blade, select **Enable Network Security Groups** on subnets or on virtual machines.</span></span>
   <span data-ttu-id="f363b-116">![Hálózati biztonsági csoportok engedélyezése][1]</span><span class="sxs-lookup"><span data-stu-id="f363b-116">![Enable Network Security Groups][1]</span></span>
2. <span data-ttu-id="f363b-117">Ekkor megnyílik a panel **hiányzó hálózati biztonsági csoportok beállítása** alhálózatok vagy virtuális gépekhez, attól függően, hogy a kiválasztott javaslat.</span><span class="sxs-lookup"><span data-stu-id="f363b-117">This opens the blade **Configure Missing Network Security Groups** for subnets or for virtual machines, depending on the recommendation that you selected.</span></span> <span data-ttu-id="f363b-118">Válassza ki egy alhálózatot vagy egy virtuális gép konfigurálása egy NSG.</span><span class="sxs-lookup"><span data-stu-id="f363b-118">Select a subnet or a virtual machine to configure an NSG on.</span></span>

   ![NSG alhálózat konfigurálása][2]

   ![NSG a virtuális gép konfigurálása][3]
3. <span data-ttu-id="f363b-121">Az a **válassza a hálózati biztonsági csoport** panelen, jelöljön ki egy meglévő NSG vagy **hozzon létre új** létrehozni egy NSG.</span><span class="sxs-lookup"><span data-stu-id="f363b-121">On the **Choose network security group** blade, select an existing NSG or select **Create new** to create an NSG.</span></span>

   ![Hálózati biztonsági csoport választása][4]

<span data-ttu-id="f363b-123">Ha létrehoz egy NSG-t, kövesse a [kezelése az Azure portál használatával NSG-k](../virtual-network/virtual-networks-create-nsg-arm-pportal.md) hozzon létre egy NSG-t és a biztonsági szabályokat állíthat be.</span><span class="sxs-lookup"><span data-stu-id="f363b-123">If you create an NSG, follow the steps in [How to manage NSGs using the Azure portal](../virtual-network/virtual-networks-create-nsg-arm-pportal.md) to create an NSG and set security rules.</span></span>

## <a name="see-also"></a><span data-ttu-id="f363b-124">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="f363b-124">See also</span></span>
<span data-ttu-id="f363b-125">Ez a cikk bemutatta a Security Center ajánlott "A hálózati biztonsági csoportok engedélyezése" alhálózatok és virtuális gépek megvalósításához.</span><span class="sxs-lookup"><span data-stu-id="f363b-125">This article showed you how to implement the Security Center recommendation "Enable Network Security Groups" for subnets or virtual machines.</span></span> <span data-ttu-id="f363b-126">Az NSG-k engedélyezésére vonatkozó további tudnivalókért olvassa el a következőket:</span><span class="sxs-lookup"><span data-stu-id="f363b-126">To learn more about enabling NSGs, see the following:</span></span>

* [<span data-ttu-id="f363b-127">Mi az a hálózati biztonsági csoport (NSG)?</span><span class="sxs-lookup"><span data-stu-id="f363b-127">What is a Network Security Group (NSG)?</span></span>](../virtual-network/virtual-networks-nsg.md)
* [<span data-ttu-id="f363b-128">Az Azure portál használatával NSG-k kezelése</span><span class="sxs-lookup"><span data-stu-id="f363b-128">How to manage NSGs using the Azure portal</span></span>](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)

<span data-ttu-id="f363b-129">A Security Centerrel kapcsolatos további információkért olvassa el a következőket:</span><span class="sxs-lookup"><span data-stu-id="f363b-129">To learn more about Security Center, see the following:</span></span>

* <span data-ttu-id="f363b-130">[Biztonsági szabályzatok beállítása az Azure Security Centerben](security-center-policies.md) – Ez a cikk bemutatja, hogyan konfigurálhat biztonsági házirendeket Azure-előfizetései és -erőforráscsoportjai számára.</span><span class="sxs-lookup"><span data-stu-id="f363b-130">[Setting security policies in Azure Security Center](security-center-policies.md) -- Learn how to configure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="f363b-131">[Biztonsági javaslatok kezelése az Azure Security Center](security-center-recommendations.md) – megtudhatja, miként könnyítik meg a javaslatok az Azure-erőforrások védelme.</span><span class="sxs-lookup"><span data-stu-id="f363b-131">[Managing security recommendations in Azure Security Center](security-center-recommendations.md) -- Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="f363b-132">[Biztonsági állapotfigyelés az Azure Security Center](security-center-monitoring.md) – útmutató az Azure-erőforrások állapotának figyelésére.</span><span class="sxs-lookup"><span data-stu-id="f363b-132">[Security health monitoring in Azure Security Center](security-center-monitoring.md) -- Learn how to monitor the health of your Azure resources.</span></span>
* <span data-ttu-id="f363b-133">[Biztonsági riasztások kezelése és válaszadás a riasztásokra az Azure Security Centerben](security-center-managing-and-responding-alerts.md) – A biztonsági riasztások kezelése és az azokra való reagálás.</span><span class="sxs-lookup"><span data-stu-id="f363b-133">[Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) -- Learn how to manage and respond to security alerts.</span></span>
* <span data-ttu-id="f363b-134">[Partneri megoldások monitorozása az Azure Security Centerrel](security-center-partner-solutions.md) – Útmutató a partneri megoldások biztonsági állapotának monitorozásához.</span><span class="sxs-lookup"><span data-stu-id="f363b-134">[Monitoring partner solutions with Azure Security Center](security-center-partner-solutions.md) -- Learn how to monitor the health status of your partner solutions.</span></span>
* <span data-ttu-id="f363b-135">[Azure Security Center – gyakran ismételt kérdések](security-center-faq.md) – Gyakran ismételt kérdések a szolgáltatás használatával kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="f363b-135">[Azure Security Center FAQ](security-center-faq.md) -- Find frequently asked questions about using the service.</span></span>
* <span data-ttu-id="f363b-136">[Az Azure biztonsági blog](http://blogs.msdn.com/b/azuresecurity/) --az Azure biztonsági legfrissebb hírek és információ.</span><span class="sxs-lookup"><span data-stu-id="f363b-136">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) -- Get the latest Azure security news and information.</span></span>

<!--Image references-->
[1]: ./media/security-center-enable-nsg/enable-nsg.png
[2]:./media/security-center-enable-nsg/configure-nsg-for-subnet.png
[3]: ./media/security-center-enable-nsg/configure-nsg-for-vm.png
[4]: ./media/security-center-enable-nsg/choose-nsg.png
