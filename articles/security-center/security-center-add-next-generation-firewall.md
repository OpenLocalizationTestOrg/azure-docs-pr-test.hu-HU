---
title: "az Azure Security Centerben új generációs tűzfal aaaAdd |} Microsoft Docs"
description: "Ez a dokumentum bemutatja, hogyan tooimplement hello Azure Security Center javaslatait ** adja hozzá a következő generációs tűzfal ** és ** útvonal traffice keresztül NGFW csak **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 48b99015-4db8-4ce8-85e4-b544c0fa203e
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/2017
ms.author: terrylan
ms.openlocfilehash: 9a80f12571ba08eadf3361728c6321388c863235
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-a-next-generation-firewall-in-azure-security-center"></a><span data-ttu-id="12103-103">Új generációs tűzfal hozzáadása az Azure Security Centerben</span><span class="sxs-lookup"><span data-stu-id="12103-103">Add a Next Generation Firewall in Azure Security Center</span></span>
<span data-ttu-id="12103-104">Az Azure Security Center is javasoljuk, hogy ad hozzá egy új generációs tűzfal (NGFW) az egy Microsoft partnert tooincrease a biztonsági védelmet.</span><span class="sxs-lookup"><span data-stu-id="12103-104">Azure Security Center may recommend that you add a next generation firewall (NGFW) from a Microsoft partner tooincrease your security protections.</span></span> <span data-ttu-id="12103-105">Ez a dokumentum útmutatást nyújt a példa bemutatja, hogyan toodo ez.</span><span class="sxs-lookup"><span data-stu-id="12103-105">This document walks you through an example of how toodo this.</span></span>

> [!NOTE]
> <span data-ttu-id="12103-106">Ez a dokumentum hello szolgáltatás telepítését bemutató példát használatával vezet be.</span><span class="sxs-lookup"><span data-stu-id="12103-106">This document introduces hello service by using an example deployment.</span></span>  <span data-ttu-id="12103-107">A dokumentum nem tartalmaz lépésenkénti útmutatót.</span><span class="sxs-lookup"><span data-stu-id="12103-107">This is not a step-by-step guide.</span></span>
>
>

## <a name="implement-hello-recommendation"></a><span data-ttu-id="12103-108">Hello javaslat megvalósítása</span><span class="sxs-lookup"><span data-stu-id="12103-108">Implement hello recommendation</span></span>
1. <span data-ttu-id="12103-109">A hello **javaslatok** panelen válassza **adja hozzá az új generációs tűzfal**.</span><span class="sxs-lookup"><span data-stu-id="12103-109">In hello **Recommendations** blade, select **Add a Next Generation Firewall**.</span></span>
   <span data-ttu-id="12103-110">![Újgenerációs tűzfal hozzáadása][1]</span><span class="sxs-lookup"><span data-stu-id="12103-110">![Add a Next Generation Firewall][1]</span></span>
2. <span data-ttu-id="12103-111">A hello **adja hozzá az új generációs tűzfal** panelen, jelöljön ki egy végpontot.</span><span class="sxs-lookup"><span data-stu-id="12103-111">In hello **Add a Next Generation Firewall** blade, select an endpoint.</span></span>
   <span data-ttu-id="12103-112">![Válasszon ki egy végpontot][2]</span><span class="sxs-lookup"><span data-stu-id="12103-112">![Select an endpoint][2]</span></span>
3. <span data-ttu-id="12103-113">Egy második **adja hozzá az új generációs tűzfal** panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="12103-113">A second **Add a Next Generation Firewall** blade opens.</span></span> <span data-ttu-id="12103-114">Dönthet úgy toouse egy meglévő megoldás ha rendelkezésre áll, vagy létrehozhat egy újat.</span><span class="sxs-lookup"><span data-stu-id="12103-114">You can choose toouse an existing solution if available or you can create a new one.</span></span> <span data-ttu-id="12103-115">Ebben a példában nincsenek nincs meglévő megoldások, létrehozhatunk egy NGFW.</span><span class="sxs-lookup"><span data-stu-id="12103-115">In this example, there are no existing solutions available so we create an NGFW.</span></span>
   <span data-ttu-id="12103-116">![Hozzon létre új generációs tűzfal][3]</span><span class="sxs-lookup"><span data-stu-id="12103-116">![Create Next Generation Firewall][3]</span></span>
4. <span data-ttu-id="12103-117">toocreate egy NGFW megoldás integrált partnereink hello listájából válassza ki.</span><span class="sxs-lookup"><span data-stu-id="12103-117">toocreate an NGFW, select a solution from hello list of integrated partners.</span></span> <span data-ttu-id="12103-118">Ez a példa azt válassza **Check Point**.</span><span class="sxs-lookup"><span data-stu-id="12103-118">In this example, we select **Check Point**.</span></span>
   <span data-ttu-id="12103-119">![Új generációs tűzfal-megoldás kiválasztása][4]</span><span class="sxs-lookup"><span data-stu-id="12103-119">![Select Next Generation Firewall solution][4]</span></span>
5. <span data-ttu-id="12103-120">Hello **Check Point** panel nyílik meg, és, hogy hello partneri megoldás információt nyújt.</span><span class="sxs-lookup"><span data-stu-id="12103-120">hello **Check Point** blade opens providing you information about hello partner solution.</span></span> <span data-ttu-id="12103-121">Válassza ki **létrehozása** hello információk panelen.</span><span class="sxs-lookup"><span data-stu-id="12103-121">Select **Create** in hello information blade.</span></span>
   <span data-ttu-id="12103-122">![Tűzfal információk panel][5]</span><span class="sxs-lookup"><span data-stu-id="12103-122">![Firewall information blade][5]</span></span>
6. <span data-ttu-id="12103-123">Hello **hozzon létre virtuális gépet** panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="12103-123">hello **Create virtual machine** blade opens.</span></span> <span data-ttu-id="12103-124">Itt adhatja meg a szükséges toospin futó virtuális gépek (VM) hello NGFW információkat.</span><span class="sxs-lookup"><span data-stu-id="12103-124">Here you can enter information required toospin up a virtual machine (VM) that runs hello NGFW.</span></span> <span data-ttu-id="12103-125">Hello lépésekkel, és adja meg a szükséges hello NGFW adatokat.</span><span class="sxs-lookup"><span data-stu-id="12103-125">Follow hello steps and provide hello NGFW information required.</span></span> <span data-ttu-id="12103-126">Válassza ki a OK tooapply.</span><span class="sxs-lookup"><span data-stu-id="12103-126">Select OK tooapply.</span></span>
   <span data-ttu-id="12103-127">![Virtuális gép toorun NGFW létrehozása][6]</span><span class="sxs-lookup"><span data-stu-id="12103-127">![Create virtual machine toorun NGFW][6]</span></span>

## <a name="route-traffic-through-ngfw-only"></a><span data-ttu-id="12103-128">Csak az újgenerációs tűzfalon keresztül haladjon a forgalom</span><span class="sxs-lookup"><span data-stu-id="12103-128">Route traffic through NGFW only</span></span>
<span data-ttu-id="12103-129">Térjen vissza a toohello **javaslatok** panelen.</span><span class="sxs-lookup"><span data-stu-id="12103-129">Return toohello **Recommendations** blade.</span></span> <span data-ttu-id="12103-130">Egy új bejegyzést jött létre, miután hozzáadta a Security Center nevű keresztül egy NGFW **keresztül NGFW forgalmat csak**.</span><span class="sxs-lookup"><span data-stu-id="12103-130">A new entry was generated after you added an NGFW via Security Center, called **Route traffic through NGFW only**.</span></span> <span data-ttu-id="12103-131">Ez a javaslat jön létre, csak akkor, ha telepítette a NGFW biztonsági központon keresztül történik.</span><span class="sxs-lookup"><span data-stu-id="12103-131">This recommendation is created only if you installed your NGFW through Security Center.</span></span> <span data-ttu-id="12103-132">Ha Internet felé néző végpontok, a Security Center azt javasolja, hogy a bejövő forgalom tooyour keresztül az NGFW VM kényszerítése a hálózati biztonsági csoport szabályok konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="12103-132">If you have Internet-facing endpoints, Security Center recommends that you configure Network Security Group rules that force inbound traffic tooyour VM through your NGFW.</span></span>

1. <span data-ttu-id="12103-133">A hello **javaslatok panel**, jelölje be **keresztül NGFW forgalmat csak**.</span><span class="sxs-lookup"><span data-stu-id="12103-133">In hello **Recommendations blade**, select **Route traffic through NGFW only**.</span></span>
   <span data-ttu-id="12103-134">![Csak az újgenerációs tűzfalon keresztül haladjon a forgalom][7]</span><span class="sxs-lookup"><span data-stu-id="12103-134">![Route traffic through NGFW only][7]</span></span>
2. <span data-ttu-id="12103-135">Hello paneljének megnyitása, ez **keresztül NGFW forgalmat csak**, amely felsorolja azokat a virtuális gépek, amelyek irányíthatja a forgalmat.</span><span class="sxs-lookup"><span data-stu-id="12103-135">This opens hello blade **Route traffic through NGFW only**, which lists VMs that you can route traffic to.</span></span> <span data-ttu-id="12103-136">Válassza ki a virtuális gépek hello listából.</span><span class="sxs-lookup"><span data-stu-id="12103-136">Select a VM from hello list.</span></span>
   <span data-ttu-id="12103-137">![Jelöljön ki egy virtuális Gépet][8]</span><span class="sxs-lookup"><span data-stu-id="12103-137">![Select a VM][8]</span></span>
3. <span data-ttu-id="12103-138">Egy hello panelen kiválasztott VM megnyílik, kapcsolódó bejövő szabályok megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="12103-138">A blade for hello selected VM opens, displaying related inbound rules.</span></span> <span data-ttu-id="12103-139">Olyan leírást nyújt további információt a következő lépést.</span><span class="sxs-lookup"><span data-stu-id="12103-139">A description provides you with more information on possible next steps.</span></span> <span data-ttu-id="12103-140">Válassza ki **bejövő szabályok szerkesztése** tooproceed rendelkező egy bejövő forgalomra vonatkozó szabály szerkesztése.</span><span class="sxs-lookup"><span data-stu-id="12103-140">Select **Edit inbound rules** tooproceed with editing an inbound rule.</span></span> <span data-ttu-id="12103-141">hello általános gyakorlat, hogy **forrás** értéke túl**bármely** a kapcsolt hello Internet felé néző végpontok hello NGFW.</span><span class="sxs-lookup"><span data-stu-id="12103-141">hello expectation is that **Source** is not set too**Any** for hello Internet-facing endpoints linked with hello NGFW.</span></span> <span data-ttu-id="12103-142">toolearn hello tulajdonságainak hello bejövő forgalomra vonatkozó szabály, bővebben lásd: [NSG-szabályok](../virtual-network/virtual-networks-nsg.md#nsg-rules).</span><span class="sxs-lookup"><span data-stu-id="12103-142">toolearn more about hello properties of hello inbound rule, see [NSG rules](../virtual-network/virtual-networks-nsg.md#nsg-rules).</span></span>
   <span data-ttu-id="12103-143">![Konfigurálja a szabályok toolimit hozzáférést][9]
   ![bejövő forgalomra vonatkozó szabály szerkesztése][10]</span><span class="sxs-lookup"><span data-stu-id="12103-143">![Configure rules toolimit access][9]
![Edit inbound rule][10]</span></span>

## <a name="see-also"></a><span data-ttu-id="12103-144">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="12103-144">See also</span></span>
<span data-ttu-id="12103-145">Ez a dokumentum bemutatta, hogyan tooimplement hello Security Center ajánlás "Vegyen fel új generációs tűzfal."</span><span class="sxs-lookup"><span data-stu-id="12103-145">This document showed you how tooimplement hello Security Center recommendation "Add a Next Generation Firewall."</span></span> <span data-ttu-id="12103-146">További információ az NGFWs és hello Check Point partneri megoldás, toolearn hello következő lásd:</span><span class="sxs-lookup"><span data-stu-id="12103-146">toolearn more about NGFWs and hello Check Point partner solution, see hello following:</span></span>

* [<span data-ttu-id="12103-147">Új generációs tűzfal</span><span class="sxs-lookup"><span data-stu-id="12103-147">Next-Generation Firewall</span></span>](https://en.wikipedia.org/wiki/Next-Generation_Firewall)
* [<span data-ttu-id="12103-148">Check Point vSEC</span><span class="sxs-lookup"><span data-stu-id="12103-148">Check Point vSEC</span></span>](https://azure.microsoft.com/marketplace/partners/checkpoint/check-point-r77-10/)

<span data-ttu-id="12103-149">További információ a Security Center toolearn hello következő lásd:</span><span class="sxs-lookup"><span data-stu-id="12103-149">toolearn more about Security Center, see hello following:</span></span>

* <span data-ttu-id="12103-150">[Biztonsági szabályzatok beállítása az Azure Security Center](security-center-policies.md) – megtudhatja, hogyan tooconfigure biztonsági házirendeket.</span><span class="sxs-lookup"><span data-stu-id="12103-150">[Setting security policies in Azure Security Center](security-center-policies.md) -- Learn how tooconfigure security policies.</span></span>
* <span data-ttu-id="12103-151">[Biztonsági javaslatok kezelése az Azure Security Center](security-center-recommendations.md) – megtudhatja, miként könnyítik meg a javaslatok az Azure-erőforrások védelme.</span><span class="sxs-lookup"><span data-stu-id="12103-151">[Managing security recommendations in Azure Security Center](security-center-recommendations.md) -- Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="12103-152">[Biztonsági állapotfigyelés az Azure Security Center](security-center-monitoring.md) – megtudhatja, hogyan toomonitor hello az Azure-erőforrások állapotát.</span><span class="sxs-lookup"><span data-stu-id="12103-152">[Security health monitoring in Azure Security Center](security-center-monitoring.md) -- Learn how toomonitor hello health of your Azure resources.</span></span>
* <span data-ttu-id="12103-153">[Az Azure Security Centerben riasztások kezelése és válaszol toosecurity](security-center-managing-and-responding-alerts.md) – megtudhatja, hogyan toomanage és válaszoljon toosecurity riasztásokat.</span><span class="sxs-lookup"><span data-stu-id="12103-153">[Managing and responding toosecurity alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) -- Learn how toomanage and respond toosecurity alerts.</span></span>
* <span data-ttu-id="12103-154">[Partnermegoldások figyelése az Azure Security Center](security-center-partner-solutions.md) – megtudhatja, hogyan toomonitor hello partneri megoldások biztonsági állapotát.</span><span class="sxs-lookup"><span data-stu-id="12103-154">[Monitoring partner solutions with Azure Security Center](security-center-partner-solutions.md) -- Learn how toomonitor hello health status of your partner solutions.</span></span>
* <span data-ttu-id="12103-155">[Azure Security Center: GYIK](security-center-faq.md) – gyakran ismételt kérdések hello szolgáltatás használatával kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="12103-155">[Azure Security Center FAQ](security-center-faq.md) -- Find frequently asked questions about using hello service.</span></span>
* <span data-ttu-id="12103-156">[Az Azure biztonsági blog](http://blogs.msdn.com/b/azuresecurity/) – blogbejegyzések az Azure biztonsági és megfelelőségi funkcióiról.</span><span class="sxs-lookup"><span data-stu-id="12103-156">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) -- Find blog posts about Azure security and compliance.</span></span>

<!--Image references-->
[1]: ./media/security-center-add-next-gen-firewall/add-next-gen-firewall.png
[2]: ./media/security-center-add-next-gen-firewall/select-an-endpoint.png
[3]: ./media/security-center-add-next-gen-firewall/create-new-next-gen-firewall.png
[4]: ./media/security-center-add-next-gen-firewall/select-next-gen-firewall.png
[5]: ./media/security-center-add-next-gen-firewall/firewall-solution-info-blade.png
[6]: ./media/security-center-add-next-gen-firewall/create-virtual-machine.png
[7]: ./media/security-center-add-next-gen-firewall/route-traffic-through-ngfw.png
[8]: ./media/security-center-add-next-gen-firewall/select-vm.png
[9]: ./media/security-center-add-next-gen-firewall/configure-rules-to-limit-access.png
[10]: ./media/security-center-add-next-gen-firewall/edit-inbound-rule.png
