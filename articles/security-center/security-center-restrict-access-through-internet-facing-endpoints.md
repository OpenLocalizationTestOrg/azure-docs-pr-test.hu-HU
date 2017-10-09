---
title: "aaaRestrict hozzáférés a Internet felé néző végpontok az Azure Security Centerben |} Microsoft Docs"
description: "Ez a dokumentum bemutatja, hogyan tooimplement hello Azure Security Center javaslat ** internetre irányuló végpont **-en keresztüli hozzáférés korlátozása."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 727d88c9-163b-4ea0-a4ce-3be43686599f
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/03/2017
ms.author: terrylan
ms.openlocfilehash: ee72497088618d4db29b5ae4183f4fe77b498423
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="restrict-access-through-internet-facing-endpoints-in-azure-security-center"></a><span data-ttu-id="92d3e-103">Az Azure Security Centerben Internet felé néző végpontok-en keresztüli hozzáférés korlátozása</span><span class="sxs-lookup"><span data-stu-id="92d3e-103">Restrict access through Internet-facing endpoints in Azure Security Center</span></span>
<span data-ttu-id="92d3e-104">Azure Security Center javasolni fogja, az Internet felé néző végpontok-en keresztüli hozzáférés korlátozása, ha bármely, a hálózati biztonsági csoportok (NSG-k) egy vagy több bejövő szabályok, amelyek lehetővé teszik a hozzáférést a "bármely" forrás IP-cím.</span><span class="sxs-lookup"><span data-stu-id="92d3e-104">Azure Security Center will recommend that you restrict access through Internet-facing endpoints if any of your Network Security Groups (NSGs) has one or more inbound rules that allow access from “any” source IP address.</span></span> <span data-ttu-id="92d3e-105">Túl "any" megnyitása access lehetővé teheti a támadók tooaccess az erőforrások.</span><span class="sxs-lookup"><span data-stu-id="92d3e-105">Opening access too“any” may enable attackers tooaccess your resources.</span></span> <span data-ttu-id="92d3e-106">Security Center javasolni fogja, hogy szerkesztenie kell a bejövő szabályok toorestrict hozzáférés toosource IP-címek, amelyek ténylegesen hozzá kell férniük.</span><span class="sxs-lookup"><span data-stu-id="92d3e-106">Security Center will recommend that you edit these inbound rules toorestrict access toosource IP addresses that actually need access.</span></span>

<span data-ttu-id="92d3e-107">Ez a javaslat bármely nem webes porthoz, amely rendelkezik "a" forrás jön létre.</span><span class="sxs-lookup"><span data-stu-id="92d3e-107">This recommendation is generated for any non-web port that has "any" as source.</span></span>

> [!NOTE]
> <span data-ttu-id="92d3e-108">Ez a dokumentum hello szolgáltatás telepítését bemutató példát használatával vezet be.</span><span class="sxs-lookup"><span data-stu-id="92d3e-108">This document introduces hello service by using an example deployment.</span></span> <span data-ttu-id="92d3e-109">A dokumentum nem tartalmaz lépésenkénti útmutatót.</span><span class="sxs-lookup"><span data-stu-id="92d3e-109">This is not a step-by-step guide.</span></span>
>
>

## <a name="implement-hello-recommendation"></a><span data-ttu-id="92d3e-110">Hello javaslat megvalósítása</span><span class="sxs-lookup"><span data-stu-id="92d3e-110">Implement hello recommendation</span></span>
1. <span data-ttu-id="92d3e-111">A hello **javaslatok panel**, jelölje be **internetes végpont-en keresztüli hozzáférés korlátozása**.</span><span class="sxs-lookup"><span data-stu-id="92d3e-111">In hello **Recommendations blade**, select **Restrict access through Internet facing endpoint**.</span></span>

   ![Internetről elérhető végponton keresztüli hozzáférés korlátozása][1]
2. <span data-ttu-id="92d3e-113">Ekkor megnyílik hello panel **internetes végpont-en keresztüli hozzáférés korlátozása**.</span><span class="sxs-lookup"><span data-stu-id="92d3e-113">This opens hello blade **Restrict access through Internet facing endpoint**.</span></span> <span data-ttu-id="92d3e-114">Ezen a panelen hello virtuális gépek (VM) rendelkező bejövő szabályok létrehozása a potenciális biztonsági kockázatot listáját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="92d3e-114">This blade lists hello virtual machines (VMs) with inbound rules that create a potential security issue.</span></span> <span data-ttu-id="92d3e-115">Jelöljön ki egy virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="92d3e-115">Select a VM.</span></span>

   ![Jelöljön ki egy virtuális Gépet][2]
3. <span data-ttu-id="92d3e-117">Hello **NSG** panel információk jelennek meg a hálózati biztonsági csoport, kapcsolódó bejövő szabályok, és a hello kapcsolódó virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="92d3e-117">hello **NSG** blade displays Network Security Group information, related inbound rules, and hello associated VM.</span></span> <span data-ttu-id="92d3e-118">Válassza ki **bejövő szabályok szerkesztése** tooproceed rendelkező egy bejövő forgalomra vonatkozó szabály szerkesztése.</span><span class="sxs-lookup"><span data-stu-id="92d3e-118">Select **Edit inbound rules** tooproceed with editing an inbound rule.</span></span>

   ![Hálózati biztonsági csoport panel][3]
4. <span data-ttu-id="92d3e-120">A hello **bejövő biztonsági szabályok** panelen válassza ki a bejövő forgalomra vonatkozó szabály tooedit hello.</span><span class="sxs-lookup"><span data-stu-id="92d3e-120">On hello **Inbound security rules** blade select hello inbound rule tooedit.</span></span> <span data-ttu-id="92d3e-121">Ebben a példában most válasszon **AllowWeb**.</span><span class="sxs-lookup"><span data-stu-id="92d3e-121">In this example, let’s select **AllowWeb**.</span></span>

   ![Bejövő biztonsági szabályok][4]

   <span data-ttu-id="92d3e-123">Vegye figyelembe, igény szerint kiválaszthatja **alapértelmezett szabályok** toosee hello készletét, alapértelmezett szabályokat minden NSG-k által tartalmazott.</span><span class="sxs-lookup"><span data-stu-id="92d3e-123">Note, you can also select **Default rules** toosee hello set of default rules contained by all NSGs.</span></span> <span data-ttu-id="92d3e-124">hello alapértelmezett szabályokat nem lehet törölni, de hozzárendeli őket egy alacsonyabb prioritású virtuális gép, mert azok által létrehozott szabályok hello felülbírálható.</span><span class="sxs-lookup"><span data-stu-id="92d3e-124">hello default rules cannot be deleted but, because they are assigned a lower priority, they can be overridden by hello rules that you create.</span></span> <span data-ttu-id="92d3e-125">További információ [alapértelmezett szabályok](../virtual-network/virtual-networks-nsg.md#default-rules).</span><span class="sxs-lookup"><span data-stu-id="92d3e-125">Learn more about [default rules](../virtual-network/virtual-networks-nsg.md#default-rules).</span></span>

   ![Alapértelmezett szabályok][5]
5. <span data-ttu-id="92d3e-127">A hello **AllowWeb** panelen hello bejövő forgalomra vonatkozó szabály, amely hello hello tulajdonságainak szerkesztése **forrás** blokkot az IP-címek vagy IP-címet.</span><span class="sxs-lookup"><span data-stu-id="92d3e-127">On hello **AllowWeb** blade, edit hello properties of hello inbound rule so that hello **Source** is an IP address or block of IP addresses.</span></span> <span data-ttu-id="92d3e-128">toolearn hello tulajdonságainak hello bejövő forgalomra vonatkozó szabály, bővebben lásd: [NSG-szabályok](../virtual-network/virtual-networks-nsg.md#nsg-rules).</span><span class="sxs-lookup"><span data-stu-id="92d3e-128">toolearn more about hello properties of hello inbound rule, see [NSG rules](../virtual-network/virtual-networks-nsg.md#nsg-rules).</span></span>

   ![Bejövő forgalomra vonatkozó szabály szerkesztése][6]

## <a name="see-also"></a><span data-ttu-id="92d3e-130">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="92d3e-130">See also</span></span>
<span data-ttu-id="92d3e-131">Ez a cikk bemutatta, hogyan tooimplement hello Security Center ajánlás "hozzáférés korlátozása keresztül internetre irányuló végpont."</span><span class="sxs-lookup"><span data-stu-id="92d3e-131">This article showed you how tooimplement hello Security Center recommendation "Restrict access through Internet facing endpoint.”</span></span> <span data-ttu-id="92d3e-132">További információ az NSG-ket és a szabályok, toolearn hello következő lásd:</span><span class="sxs-lookup"><span data-stu-id="92d3e-132">toolearn more about enabling NSGs and rules, see hello following:</span></span>

* [<span data-ttu-id="92d3e-133">Mi az a hálózati biztonsági csoport (NSG)?</span><span class="sxs-lookup"><span data-stu-id="92d3e-133">What is a Network Security Group (NSG)?</span></span>](../virtual-network/virtual-networks-nsg.md)
* [<span data-ttu-id="92d3e-134">Hogyan toomanage NSG-k használatával hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="92d3e-134">How toomanage NSGs using hello Azure portal</span></span>](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)

<span data-ttu-id="92d3e-135">További információ a Security Center toolearn hello következő lásd:</span><span class="sxs-lookup"><span data-stu-id="92d3e-135">toolearn more about Security Center, see hello following:</span></span>

* <span data-ttu-id="92d3e-136">[Biztonsági szabályzatok beállítása az Azure Security Center](security-center-policies.md)– megtudhatja, hogyan tooconfigure biztonsági házirendek az Azure-előfizetések és az erőforráscsoportokat.</span><span class="sxs-lookup"><span data-stu-id="92d3e-136">[Setting security policies in Azure Security Center](security-center-policies.md)--Learn how tooconfigure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="92d3e-137">[Biztonsági javaslatok kezelése az Azure Security Centerben](security-center-recommendations.md) – Miként könnyítik meg a javaslatok az Azure-erőforrások védelmét?</span><span class="sxs-lookup"><span data-stu-id="92d3e-137">[Managing security recommendations in Azure Security Center](security-center-recommendations.md)--Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="92d3e-138">[Biztonsági állapotfigyelés az Azure Security Center](security-center-monitoring.md)– megtudhatja, hogyan toomonitor hello az Azure-erőforrások állapotát.</span><span class="sxs-lookup"><span data-stu-id="92d3e-138">[Security health monitoring in Azure Security Center](security-center-monitoring.md)--Learn how toomonitor hello health of your Azure resources.</span></span>
* <span data-ttu-id="92d3e-139">[Az Azure Security Centerben riasztások kezelése és válaszol toosecurity](security-center-managing-and-responding-alerts.md)– megtudhatja, hogyan toomanage és válaszoljon toosecurity riasztásokat.</span><span class="sxs-lookup"><span data-stu-id="92d3e-139">[Managing and responding toosecurity alerts in Azure Security Center](security-center-managing-and-responding-alerts.md)--Learn how toomanage and respond toosecurity alerts.</span></span>
* <span data-ttu-id="92d3e-140">[Partnermegoldások figyelése az Azure Security Center](security-center-partner-solutions.md) – megtudhatja, hogyan toomonitor hello partneri megoldások biztonsági állapotát.</span><span class="sxs-lookup"><span data-stu-id="92d3e-140">[Monitoring partner solutions with Azure Security Center](security-center-partner-solutions.md) -- Learn how toomonitor hello health status of your partner solutions.</span></span>
* <span data-ttu-id="92d3e-141">[Azure Security Center: GYIK](security-center-faq.md)– gyakran ismételt kérdések hello szolgáltatás használatával kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="92d3e-141">[Azure Security Center FAQ](security-center-faq.md)--Find frequently asked questions about using hello service.</span></span>
* <span data-ttu-id="92d3e-142">[Az Azure biztonsági blog](http://blogs.msdn.com/b/azuresecurity/)– hello legújabb Azure biztonsági hírek és információ lekérése.</span><span class="sxs-lookup"><span data-stu-id="92d3e-142">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/)--Get hello latest Azure security news and information.</span></span>

<!--Image references-->
[1]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/restrict-access-thru-internet-facing-endpoint.png
[2]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/select-a-vm.png
[3]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/network-security-group-blade.png
[4]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/inbound-security-rules.png
[5]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/default-rules.png
[6]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/edit-inbound-rule.png
