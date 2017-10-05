---
title: "Az Azure Security Centerben Internet felé néző végpontok-en keresztüli hozzáférés korlátozása |} Microsoft Docs"
description: "Ez a dokumentum azt ismerteti, hogyan valósítja meg az Azure Security Center ajánlás ** internetre irányuló végpont **-en keresztüli hozzáférés korlátozása."
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
ms.openlocfilehash: f7309c617f1705205e2c9f1b1b48d141391d45da
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="restrict-access-through-internet-facing-endpoints-in-azure-security-center"></a><span data-ttu-id="e931c-103">Az Azure Security Centerben Internet felé néző végpontok-en keresztüli hozzáférés korlátozása</span><span class="sxs-lookup"><span data-stu-id="e931c-103">Restrict access through Internet-facing endpoints in Azure Security Center</span></span>
<span data-ttu-id="e931c-104">Azure Security Center javasolni fogja, az Internet felé néző végpontok-en keresztüli hozzáférés korlátozása, ha bármely, a hálózati biztonsági csoportok (NSG-k) egy vagy több bejövő szabályok, amelyek lehetővé teszik a hozzáférést a "bármely" forrás IP-cím.</span><span class="sxs-lookup"><span data-stu-id="e931c-104">Azure Security Center will recommend that you restrict access through Internet-facing endpoints if any of your Network Security Groups (NSGs) has one or more inbound rules that allow access from “any” source IP address.</span></span> <span data-ttu-id="e931c-105">Megnyitása access "bármely" engedélyezheti, hogy a támadók a erőforrások eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="e931c-105">Opening access to “any” may enable attackers to access your resources.</span></span> <span data-ttu-id="e931c-106">Security Center javasolni fogja, hogy a forrás IP-címek, amelyek ténylegesen hozzá kell férniük való hozzáférés korlátozása a bejövő szabályok szerkesztése.</span><span class="sxs-lookup"><span data-stu-id="e931c-106">Security Center will recommend that you edit these inbound rules to restrict access to source IP addresses that actually need access.</span></span>

<span data-ttu-id="e931c-107">Ez a javaslat bármely nem webes porthoz, amely rendelkezik "a" forrás jön létre.</span><span class="sxs-lookup"><span data-stu-id="e931c-107">This recommendation is generated for any non-web port that has "any" as source.</span></span>

> [!NOTE]
> <span data-ttu-id="e931c-108">Ez a dokumentum egy üzembe helyezést szemléltető példa segítségével mutatja be a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="e931c-108">This document introduces the service by using an example deployment.</span></span> <span data-ttu-id="e931c-109">A dokumentum nem tartalmaz lépésenkénti útmutatót.</span><span class="sxs-lookup"><span data-stu-id="e931c-109">This is not a step-by-step guide.</span></span>
>
>

## <a name="implement-the-recommendation"></a><span data-ttu-id="e931c-110">A javaslat megvalósítása</span><span class="sxs-lookup"><span data-stu-id="e931c-110">Implement the recommendation</span></span>
1. <span data-ttu-id="e931c-111">Az a **javaslatok panel**, jelölje be **internetes végpont-en keresztüli hozzáférés korlátozása**.</span><span class="sxs-lookup"><span data-stu-id="e931c-111">In the **Recommendations blade**, select **Restrict access through Internet facing endpoint**.</span></span>

   ![Internetről elérhető végponton keresztüli hozzáférés korlátozása][1]
2. <span data-ttu-id="e931c-113">Ekkor megnyílik a panel **internetes végpont-en keresztüli hozzáférés korlátozása**.</span><span class="sxs-lookup"><span data-stu-id="e931c-113">This opens the blade **Restrict access through Internet facing endpoint**.</span></span> <span data-ttu-id="e931c-114">Ezen a panelen a virtuális gépek (VM) rendelkező bejövő szabályok létrehozása a potenciális biztonsági kockázatot listáját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="e931c-114">This blade lists the virtual machines (VMs) with inbound rules that create a potential security issue.</span></span> <span data-ttu-id="e931c-115">Jelöljön ki egy virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="e931c-115">Select a VM.</span></span>

   ![Jelöljön ki egy virtuális Gépet][2]
3. <span data-ttu-id="e931c-117">A **NSG** csempe megjeleníti a társított virtuális Gépet, a hálózati biztonsági csoport információkat és a kapcsolódó bejövő szabályok.</span><span class="sxs-lookup"><span data-stu-id="e931c-117">The **NSG** blade displays Network Security Group information, related inbound rules, and the associated VM.</span></span> <span data-ttu-id="e931c-118">Válassza ki **bejövő szabályok szerkesztése** egy bejövő forgalomra vonatkozó szabály szerkesztése gombra.</span><span class="sxs-lookup"><span data-stu-id="e931c-118">Select **Edit inbound rules** to proceed with editing an inbound rule.</span></span>

   ![Hálózati biztonsági csoport panel][3]
4. <span data-ttu-id="e931c-120">Az a **bejövő biztonsági szabályok** panelen válassza ki a bejövő forgalomra vonatkozó szabály szerkesztése.</span><span class="sxs-lookup"><span data-stu-id="e931c-120">On the **Inbound security rules** blade select the inbound rule to edit.</span></span> <span data-ttu-id="e931c-121">Ebben a példában most válasszon **AllowWeb**.</span><span class="sxs-lookup"><span data-stu-id="e931c-121">In this example, let’s select **AllowWeb**.</span></span>

   ![Bejövő biztonsági szabályok][4]

   <span data-ttu-id="e931c-123">Vegye figyelembe, igény szerint kiválaszthatja **alapértelmezett szabályok** című szakaszban láthat az alapértelmezett szabályokat minden NSG-k által tartalmazott.</span><span class="sxs-lookup"><span data-stu-id="e931c-123">Note, you can also select **Default rules** to see the set of default rules contained by all NSGs.</span></span> <span data-ttu-id="e931c-124">Az alapértelmezett szabályokat nem lehet törölni, de hozzárendeli őket egy alacsonyabb prioritású virtuális gép, mert azok az Ön által létrehozott szabályok felülbírálható.</span><span class="sxs-lookup"><span data-stu-id="e931c-124">The default rules cannot be deleted but, because they are assigned a lower priority, they can be overridden by the rules that you create.</span></span> <span data-ttu-id="e931c-125">További információ [alapértelmezett szabályok](../virtual-network/virtual-networks-nsg.md#default-rules).</span><span class="sxs-lookup"><span data-stu-id="e931c-125">Learn more about [default rules](../virtual-network/virtual-networks-nsg.md#default-rules).</span></span>

   ![Alapértelmezett szabályok][5]
5. <span data-ttu-id="e931c-127">A a **AllowWeb** panelen a bejövő forgalomra vonatkozó szabály tulajdonságait szerkesztheti, hogy a **forrás** blokkot az IP-címek vagy IP-cím.</span><span class="sxs-lookup"><span data-stu-id="e931c-127">On the **AllowWeb** blade, edit the properties of the inbound rule so that the **Source** is an IP address or block of IP addresses.</span></span> <span data-ttu-id="e931c-128">A bejövő szabály tulajdonságait kapcsolatos további információkért lásd: [NSG-szabályok](../virtual-network/virtual-networks-nsg.md#nsg-rules).</span><span class="sxs-lookup"><span data-stu-id="e931c-128">To learn more about the properties of the inbound rule, see [NSG rules](../virtual-network/virtual-networks-nsg.md#nsg-rules).</span></span>

   ![Bejövő forgalomra vonatkozó szabály szerkesztése][6]

## <a name="see-also"></a><span data-ttu-id="e931c-130">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="e931c-130">See also</span></span>
<span data-ttu-id="e931c-131">Ez a cikk bemutatta megvalósításához a Security Center javaslat "Hozzáférés korlátozása az Internet felé néző végpont keresztül."</span><span class="sxs-lookup"><span data-stu-id="e931c-131">This article showed you how to implement the Security Center recommendation "Restrict access through Internet facing endpoint.”</span></span> <span data-ttu-id="e931c-132">Az NSG-k és szabályok engedélyezésével kapcsolatos további tudnivalókért olvassa el a következőket:</span><span class="sxs-lookup"><span data-stu-id="e931c-132">To learn more about enabling NSGs and rules, see the following:</span></span>

* [<span data-ttu-id="e931c-133">Mi az a hálózati biztonsági csoport (NSG)?</span><span class="sxs-lookup"><span data-stu-id="e931c-133">What is a Network Security Group (NSG)?</span></span>](../virtual-network/virtual-networks-nsg.md)
* [<span data-ttu-id="e931c-134">Az Azure portál használatával NSG-k kezelése</span><span class="sxs-lookup"><span data-stu-id="e931c-134">How to manage NSGs using the Azure portal</span></span>](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)

<span data-ttu-id="e931c-135">A Security Centerrel kapcsolatos további információkért olvassa el a következőket:</span><span class="sxs-lookup"><span data-stu-id="e931c-135">To learn more about Security Center, see the following:</span></span>

* <span data-ttu-id="e931c-136">[Biztonsági házirendek beállítása az Azure Security Centerben](security-center-policies.md) – Annak bemutatása, hogy miként konfigurálhat biztonsági házirendeket Azure-előfizetéseihez és az erőforráscsoportokhoz.</span><span class="sxs-lookup"><span data-stu-id="e931c-136">[Setting security policies in Azure Security Center](security-center-policies.md)--Learn how to configure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="e931c-137">[Biztonsági javaslatok kezelése az Azure Security Centerben](security-center-recommendations.md) – Miként könnyítik meg a javaslatok az Azure-erőforrások védelmét?</span><span class="sxs-lookup"><span data-stu-id="e931c-137">[Managing security recommendations in Azure Security Center](security-center-recommendations.md)--Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="e931c-138">[Biztonsági állapotmonitorozás az Azure Security Centerben](security-center-monitoring.md) – Útmutató az Azure-erőforrások állapotának monitorozásához.</span><span class="sxs-lookup"><span data-stu-id="e931c-138">[Security health monitoring in Azure Security Center](security-center-monitoring.md)--Learn how to monitor the health of your Azure resources.</span></span>
* <span data-ttu-id="e931c-139">[Biztonsági riasztások kezelése és reagálás a riasztásokra az Azure Security Centerben](security-center-managing-and-responding-alerts.md) – A biztonsági riasztások kezelése és az azokra való reagálás.</span><span class="sxs-lookup"><span data-stu-id="e931c-139">[Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md)--Learn how to manage and respond to security alerts.</span></span>
* <span data-ttu-id="e931c-140">[Partneri megoldások monitorozása az Azure Security Centerrel](security-center-partner-solutions.md) – Útmutató a partneri megoldások biztonsági állapotának monitorozásához.</span><span class="sxs-lookup"><span data-stu-id="e931c-140">[Monitoring partner solutions with Azure Security Center](security-center-partner-solutions.md) -- Learn how to monitor the health status of your partner solutions.</span></span>
* <span data-ttu-id="e931c-141">[Azure Security Center – gyakran ismételt kérdések](security-center-faq.md) – Gyakran ismételt kérdések a szolgáltatás használatával kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="e931c-141">[Azure Security Center FAQ](security-center-faq.md)--Find frequently asked questions about using the service.</span></span>
* <span data-ttu-id="e931c-142">[Azure biztonsági blog](http://blogs.msdn.com/b/azuresecurity/) – Tájékozódás az Azure biztonságával kapcsolatos legfrissebb hírekről és információkról.</span><span class="sxs-lookup"><span data-stu-id="e931c-142">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/)--Get the latest Azure security news and information.</span></span>

<!--Image references-->
[1]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/restrict-access-thru-internet-facing-endpoint.png
[2]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/select-a-vm.png
[3]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/network-security-group-blade.png
[4]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/inbound-security-rules.png
[5]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/default-rules.png
[6]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/edit-inbound-rule.png
