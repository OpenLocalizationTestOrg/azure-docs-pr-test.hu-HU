---
title: "Az Azure Security Center és a Windows virtuális gépek Azure-ban |} Microsoft Docs"
description: "További tudnivalók a Azure Windows virtuális gép az Azure Security Center biztonsági."
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/01/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: adb00e28b0b204858a763f83836ee2ac96f8f9e4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="monitor-virtual-machine-security-by-using-azure-security-center"></a><span data-ttu-id="9f83b-103">Virtuális gép biztonsági figyelje az Azure Security Centerben</span><span class="sxs-lookup"><span data-stu-id="9f83b-103">Monitor virtual machine security by using Azure Security Center</span></span>

<span data-ttu-id="9f83b-104">Az Azure Security Center segítségével, hogy lássák az Azure-erőforrás biztonsági eljárásokat.</span><span class="sxs-lookup"><span data-stu-id="9f83b-104">Azure Security Center can help you gain visibility into your Azure resource security practices.</span></span> <span data-ttu-id="9f83b-105">A Security Center kínál, integrált biztonsági figyelés.</span><span class="sxs-lookup"><span data-stu-id="9f83b-105">Security Center offers integrated security monitoring.</span></span> <span data-ttu-id="9f83b-106">Ellenkező esetben szabályzatkezelést fenyegetések azonosítására képes.</span><span class="sxs-lookup"><span data-stu-id="9f83b-106">It can detect threats that otherwise might go unnoticed.</span></span> <span data-ttu-id="9f83b-107">Ebben az oktatóanyagban további tudnivalók az Azure Security Center, és hogyan:</span><span class="sxs-lookup"><span data-stu-id="9f83b-107">In this tutorial, you learn about Azure Security Center, and how to:</span></span>
 
> [!div class="checklist"]
> * <span data-ttu-id="9f83b-108">Adatgyűjtés beállítása</span><span class="sxs-lookup"><span data-stu-id="9f83b-108">Set up data collection</span></span>
> * <span data-ttu-id="9f83b-109">Biztonsági házirendek beállítása</span><span class="sxs-lookup"><span data-stu-id="9f83b-109">Set up security policies</span></span>
> * <span data-ttu-id="9f83b-110">Megtekintheti és konfigurációs állapotát kapcsolatos problémák megoldása</span><span class="sxs-lookup"><span data-stu-id="9f83b-110">View and fix configuration health issues</span></span>
> * <span data-ttu-id="9f83b-111">Tekintse át a fenyegetést észlelt</span><span class="sxs-lookup"><span data-stu-id="9f83b-111">Review detected threats</span></span>  

## <a name="security-center-overview"></a><span data-ttu-id="9f83b-112">A Security Center áttekintése</span><span class="sxs-lookup"><span data-stu-id="9f83b-112">Security Center overview</span></span>

<span data-ttu-id="9f83b-113">A Security Center lehetséges a virtuális gép (VM) konfigurációs problémák azonosítja, és biztonsági fenyegetések megcélzott.</span><span class="sxs-lookup"><span data-stu-id="9f83b-113">Security Center identifies potential virtual machine (VM) configuration issues and targeted security threats.</span></span> <span data-ttu-id="9f83b-114">Ezek közé tartozik a virtuális gépek hálózati biztonsági csoportok, a nem titkosított lemezek és a találgatásos Remote Desktop Protocol (RDP) támadások hiányoznak.</span><span class="sxs-lookup"><span data-stu-id="9f83b-114">These might include VMs that are missing network security groups, unencrypted disks, and brute-force Remote Desktop Protocol (RDP) attacks.</span></span> <span data-ttu-id="9f83b-115">Az információ jelenik meg a Security Center irányítópultjának könnyen olvasható diagramokban.</span><span class="sxs-lookup"><span data-stu-id="9f83b-115">The information is shown on the Security Center dashboard in easy-to-read graphs.</span></span>

<span data-ttu-id="9f83b-116">Hozzáférni a Security Center irányítópultjának, az Azure portálon, a menüben válassza ki a **Security Center**.</span><span class="sxs-lookup"><span data-stu-id="9f83b-116">To access the Security Center dashboard, in the Azure portal, on the menu, select  **Security Center**.</span></span> <span data-ttu-id="9f83b-117">Az irányítópulton tekintse meg az Azure környezetben biztonsági állapotát, található az aktuális javaslatok számát, és fenyegetést riasztások aktuális állapotának megtekintése.</span><span class="sxs-lookup"><span data-stu-id="9f83b-117">On the dashboard, you can see the security health of your Azure environment, find a count of current recommendations, and view the current state of threat alerts.</span></span> <span data-ttu-id="9f83b-118">A részletek megtekintéséhez magas szintű diagramok bővítheti.</span><span class="sxs-lookup"><span data-stu-id="9f83b-118">You can expand each high-level chart to see more detail.</span></span>

![Security Center irányítópultjának](./media/tutorial-azure-security/asc-dash.png)

<span data-ttu-id="9f83b-120">A Security Center túllép adatok felderítés javaslatokkal szolgál, hogy az észlelt problémákat.</span><span class="sxs-lookup"><span data-stu-id="9f83b-120">Security Center goes beyond data discovery to provide recommendations for issues that it detects.</span></span> <span data-ttu-id="9f83b-121">Például ha egy virtuális Gépet egy hálózati biztonsági csoport nélkül lett telepítve, a Security Center ajánlás olyan környezetekben, a javítási lépéseket, amelyek jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="9f83b-121">For example, if a VM was deployed without an attached network security group, Security Center displays a recommendation, with remediation steps you can take.</span></span> <span data-ttu-id="9f83b-122">A Security Center kontextusában maradjanak automatikus javítási kap.</span><span class="sxs-lookup"><span data-stu-id="9f83b-122">You get automated remediation without leaving the context of Security Center.</span></span>  

![Javaslatok](./media/tutorial-azure-security/recommendations.png)

## <a name="set-up-data-collection"></a><span data-ttu-id="9f83b-124">Adatgyűjtés beállítása</span><span class="sxs-lookup"><span data-stu-id="9f83b-124">Set up data collection</span></span>

<span data-ttu-id="9f83b-125">Előtt a virtuális gép biztonsági beállításokat tud bejutni látható, akkor be kell állítania a Security Center adatgyűjtés.</span><span class="sxs-lookup"><span data-stu-id="9f83b-125">Before you can get visibility into VM security configurations, you need to set up Security Center data collection.</span></span> <span data-ttu-id="9f83b-126">Ebbe beletartozik az adatok gyűjtésének bekapcsolása és az összegyűjtött adatok tárolásához Azure storage-fiók létrehozása.</span><span class="sxs-lookup"><span data-stu-id="9f83b-126">This involves turning on data collection and creating an Azure storage account to hold collected data.</span></span> 

1. <span data-ttu-id="9f83b-127">A Security Center irányítópultján kattintson **biztonsági házirend**, majd válassza ki az előfizetését.</span><span class="sxs-lookup"><span data-stu-id="9f83b-127">On the Security Center dashboard, click **Security policy**, and then select your subscription.</span></span> 
2. <span data-ttu-id="9f83b-128">A **adatgyűjtés**, jelölje be **a**.</span><span class="sxs-lookup"><span data-stu-id="9f83b-128">For **Data collection**, select **On**.</span></span>
3. <span data-ttu-id="9f83b-129">A storage-fiók létrehozásához válassza **válasszon tárfiókot**.</span><span class="sxs-lookup"><span data-stu-id="9f83b-129">To create a storage account, select **Choose a storage account**.</span></span> <span data-ttu-id="9f83b-130">Ezt követően válassza **OK**.</span><span class="sxs-lookup"><span data-stu-id="9f83b-130">Then, select **OK**.</span></span>
4. <span data-ttu-id="9f83b-131">Az a **biztonsági házirend** panelen válassza **mentése**.</span><span class="sxs-lookup"><span data-stu-id="9f83b-131">On the **Security Policy** blade, select **Save**.</span></span> 

<span data-ttu-id="9f83b-132">A Security Center adatok szolgáltatások ügynököt telepíti az összes virtuális gépeken, és adatgyűjtés megkezdése.</span><span class="sxs-lookup"><span data-stu-id="9f83b-132">The Security Center data collection agent is then installed on all VMs, and data collection begins.</span></span> 

## <a name="set-up-a-security-policy"></a><span data-ttu-id="9f83b-133">A biztonsági házirendben</span><span class="sxs-lookup"><span data-stu-id="9f83b-133">Set up a security policy</span></span>

<span data-ttu-id="9f83b-134">Biztonsági házirendek határozzák meg a, amelynek a Security Center adatokat gyűjt, és ajánlásokat.</span><span class="sxs-lookup"><span data-stu-id="9f83b-134">Security policies are used to define the items for which Security Center collects data and makes recommendations.</span></span> <span data-ttu-id="9f83b-135">Az Azure-erőforrások más-más részhalmazához különböző biztonsági házirendeket is alkalmazhat.</span><span class="sxs-lookup"><span data-stu-id="9f83b-135">You can apply different security policies to different sets of Azure resources.</span></span> <span data-ttu-id="9f83b-136">Bár az alapértelmezés szerint minden házirendelemek szemben Azure-erőforrások értékelődnek ki, kikapcsolhatja az összes Azure-erőforrások vagy erőforráscsoport egyedi házirend elemek.</span><span class="sxs-lookup"><span data-stu-id="9f83b-136">Although by default Azure resources are evaluated against all policy items, you can turn off individual policy items for all Azure resources or for a resource group.</span></span> <span data-ttu-id="9f83b-137">A Security Center biztonsági házirendekkel kapcsolatos részletesebb információk: [biztonsági házirendek beállítása az Azure Security Centerben](../../security-center/security-center-policies.md).</span><span class="sxs-lookup"><span data-stu-id="9f83b-137">For in-depth information about Security Center security policies, see [Set security policies in Azure Security Center](../../security-center/security-center-policies.md).</span></span> 

<span data-ttu-id="9f83b-138">Az összes Azure-erőforrások biztonsági házirend beállítása:</span><span class="sxs-lookup"><span data-stu-id="9f83b-138">To set up a security policy for all Azure resources:</span></span>

1. <span data-ttu-id="9f83b-139">Válassza ki a Security Center irányítópultjának **biztonsági házirend**, majd válassza ki az előfizetését.</span><span class="sxs-lookup"><span data-stu-id="9f83b-139">On the Security Center dashboard, select **Security policy**, and then select your subscription.</span></span>
2. <span data-ttu-id="9f83b-140">Válassza ki **megakadályozási szabályzat**.</span><span class="sxs-lookup"><span data-stu-id="9f83b-140">Select **Prevention policy**.</span></span>
3. <span data-ttu-id="9f83b-141">Kapcsolja be, vagy kapcsolja ki az összes Azure-erőforrások alkalmazni kívánt házirend-elemeket.</span><span class="sxs-lookup"><span data-stu-id="9f83b-141">Turn on or turn off policy items that you want to apply to all Azure resources.</span></span>
4. <span data-ttu-id="9f83b-142">Amikor elkészült, válassza a beállítások, válassza ki a **OK**.</span><span class="sxs-lookup"><span data-stu-id="9f83b-142">When you're finished selecting your settings, select **OK**.</span></span>
5. <span data-ttu-id="9f83b-143">Az a **biztonsági házirend** panelen válassza **mentése**.</span><span class="sxs-lookup"><span data-stu-id="9f83b-143">On the **Security policy** blade, select **Save**.</span></span> 

<span data-ttu-id="9f83b-144">Az adott erőforráscsoport szabályzat beállítása:</span><span class="sxs-lookup"><span data-stu-id="9f83b-144">To set up a policy for a specific resource group:</span></span>

1. <span data-ttu-id="9f83b-145">Jelölje ki a Security Center irányítópultjának **biztonsági házirend**, majd válassza ki egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="9f83b-145">On the Security Center dashboard, select **Security policy**, and then select a resource group.</span></span>
2. <span data-ttu-id="9f83b-146">Válassza ki **megakadályozási szabályzat**.</span><span class="sxs-lookup"><span data-stu-id="9f83b-146">Select **Prevention policy**.</span></span>
3. <span data-ttu-id="9f83b-147">Kapcsolja be, vagy kapcsolja ki az erőforráscsoport alkalmazni kívánt házirend-elemeket.</span><span class="sxs-lookup"><span data-stu-id="9f83b-147">Turn on or turn off policy items that you want to apply to the resource group.</span></span>
4. <span data-ttu-id="9f83b-148">A **ÖRÖKLÉSI**, jelölje be **egyedi**.</span><span class="sxs-lookup"><span data-stu-id="9f83b-148">Under **INHERITANCE**, select **Unique**.</span></span>
5. <span data-ttu-id="9f83b-149">Amikor elkészült, válassza a beállítások, válassza ki a **OK**.</span><span class="sxs-lookup"><span data-stu-id="9f83b-149">When you're finished selecting your settings, select **OK**.</span></span>
6. <span data-ttu-id="9f83b-150">Az a **biztonsági házirend** panelen válassza **mentése**.</span><span class="sxs-lookup"><span data-stu-id="9f83b-150">On the **Security policy** blade, select **Save**.</span></span>  

<span data-ttu-id="9f83b-151">Ön is bármikor kikapcsolhatják az adatgyűjtést ezen a lapon megadott erőforráscsoport.</span><span class="sxs-lookup"><span data-stu-id="9f83b-151">You also can turn off data collection for a specific resource group on this page.</span></span>

<span data-ttu-id="9f83b-152">A következő példában egy egyedi házirend nevű erőforráscsoport létrejött *myResoureGroup*.</span><span class="sxs-lookup"><span data-stu-id="9f83b-152">In the following example, a unique policy has been created for a resource group named *myResoureGroup*.</span></span> <span data-ttu-id="9f83b-153">Ezt a házirendet, a lemez titkosítása és a webes alkalmazás tűzfal javaslatok ki vannak kapcsolva.</span><span class="sxs-lookup"><span data-stu-id="9f83b-153">In this policy, disk encryption and web application firewall recommendations are turned off.</span></span>

![Egyedi házirend](./media/tutorial-azure-security/unique-policy.png)

## <a name="view-vm-configuration-health"></a><span data-ttu-id="9f83b-155">Virtuális gép konfigurációs állapotának megtekintése</span><span class="sxs-lookup"><span data-stu-id="9f83b-155">View VM configuration health</span></span>

<span data-ttu-id="9f83b-156">Miután engedélyezve van a használatra vonatkozó adatok gyűjtésének és állíthat be a biztonsági házirendet, a Security Center veszi át a riasztások és javaslatok.</span><span class="sxs-lookup"><span data-stu-id="9f83b-156">After you've turned on data collection and set a security policy, Security Center begins to provide alerts and recommendations.</span></span> <span data-ttu-id="9f83b-157">Mivel a virtuális gépek vannak telepítve, az adatok gyűjtése ügynök telepítve van.</span><span class="sxs-lookup"><span data-stu-id="9f83b-157">As VMs are deployed, the data collection agent is installed.</span></span> <span data-ttu-id="9f83b-158">A Security Center majd az új virtuális gépek feltöltve adatokkal.</span><span class="sxs-lookup"><span data-stu-id="9f83b-158">Security Center is then populated with data for the new VMs.</span></span> <span data-ttu-id="9f83b-159">Részletes információ a virtuális gép konfigurációs állapotát: [a Security Center a virtuális gépek védelme](../../security-center/security-center-virtual-machine-recommendations.md).</span><span class="sxs-lookup"><span data-stu-id="9f83b-159">For in-depth information about VM configuration health, see [Protect your VMs in Security Center](../../security-center/security-center-virtual-machine-recommendations.md).</span></span> 

<span data-ttu-id="9f83b-160">Összegyűjtött adatok, az erőforrás állapota minden egyes virtuális gép és a kapcsolódó Azure-erőforrás összesíti.</span><span class="sxs-lookup"><span data-stu-id="9f83b-160">As data is collected, the resource health for each VM and related Azure resource is aggregated.</span></span> <span data-ttu-id="9f83b-161">Az információk könnyen olvasható diagramon látható.</span><span class="sxs-lookup"><span data-stu-id="9f83b-161">The information is shown in an easy-to-read chart.</span></span> 

<span data-ttu-id="9f83b-162">Erőforrás állapotának megtekintéséhez:</span><span class="sxs-lookup"><span data-stu-id="9f83b-162">To view resource health:</span></span>

1.  <span data-ttu-id="9f83b-163">A biztonsági Center irányítópultján a **erőforrás biztonsági állapota**, jelölje be **számítási**.</span><span class="sxs-lookup"><span data-stu-id="9f83b-163">On the Security Center dashboard, under **Resource security health**, select **Compute**.</span></span> 
2.  <span data-ttu-id="9f83b-164">Az a **számítási** panelen válassza **virtuális gépek**.</span><span class="sxs-lookup"><span data-stu-id="9f83b-164">On the **Compute** blade, select **Virtual machines**.</span></span> <span data-ttu-id="9f83b-165">Ez a nézet a konfigurációs állapotának összegzését tartalmazza a virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="9f83b-165">This view provides a summary of the configuration status for all your VMs.</span></span>

![Számítási állapota](./media/tutorial-azure-security/compute-health.png)

<span data-ttu-id="9f83b-167">A virtuális gépek összes javaslatok megtekintéséhez válasszon a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="9f83b-167">To see all recommendations for a VM, select the VM.</span></span> <span data-ttu-id="9f83b-168">Javaslatok, a javítási Ez az oktatóanyag következő szakasza részletesen ismertetnek.</span><span class="sxs-lookup"><span data-stu-id="9f83b-168">Recommendations and remediation are covered in more detail in the next section of this tutorial.</span></span>

## <a name="remediate-configuration-issues"></a><span data-ttu-id="9f83b-169">Konfigurációs problémák megoldásához</span><span class="sxs-lookup"><span data-stu-id="9f83b-169">Remediate configuration issues</span></span>

<span data-ttu-id="9f83b-170">A konfigurációs adatok feltöltése a Security Center elindítása után javaslatok alapján készülnek a biztonsági házirend beállítása.</span><span class="sxs-lookup"><span data-stu-id="9f83b-170">After Security Center begins to populate with configuration data, recommendations are made based on the security policy you set up.</span></span> <span data-ttu-id="9f83b-171">Például ha egy virtuális Gépet egy társított hálózati biztonsági csoport nélkül be lett állítva, a javaslatokkal kattintva létrehozhat egyet.</span><span class="sxs-lookup"><span data-stu-id="9f83b-171">For instance, if a VM was set up without an associated network security group, a recommendation is made to create one.</span></span> 

<span data-ttu-id="9f83b-172">Az összes javaslatok listájának megtekintéséhez:</span><span class="sxs-lookup"><span data-stu-id="9f83b-172">To see a list of all recommendations:</span></span> 

1. <span data-ttu-id="9f83b-173">Válassza ki a Security Center irányítópultjának **javaslatok**.</span><span class="sxs-lookup"><span data-stu-id="9f83b-173">On the Security Center dashboard, select **Recommendations**.</span></span>
2. <span data-ttu-id="9f83b-174">Válassza ki az adott javaslat.</span><span class="sxs-lookup"><span data-stu-id="9f83b-174">Select a specific recommendation.</span></span> <span data-ttu-id="9f83b-175">Megjelenik egy lista, amelynek erőforrásait a javaslat vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="9f83b-175">A list of all resources for which the recommendation applies appears.</span></span>
3. <span data-ttu-id="9f83b-176">Alkalmaz egy javaslatot, válassza ki az adott erőforrás.</span><span class="sxs-lookup"><span data-stu-id="9f83b-176">To apply a recommendation, select a specific resource.</span></span> 
4. <span data-ttu-id="9f83b-177">Kövesse az utasításokat a javítási lépéseket.</span><span class="sxs-lookup"><span data-stu-id="9f83b-177">Follow the instructions for remediation steps.</span></span> 

<span data-ttu-id="9f83b-178">Sok esetben a Security Center itt végrehajtandó lépések, anélkül, hogy a Security Center ajánlást megoldására.</span><span class="sxs-lookup"><span data-stu-id="9f83b-178">In many cases, Security Center provides actionable steps you can take to address a recommendation without leaving Security Center.</span></span> <span data-ttu-id="9f83b-179">A következő példában a Security Center észleli a hálózati biztonsági csoport, amely rendelkezik egy korlátlan bejövő szabályt.</span><span class="sxs-lookup"><span data-stu-id="9f83b-179">In the following example, Security Center detects a network security group that has an unrestricted inbound rule.</span></span> <span data-ttu-id="9f83b-180">A javaslat lapon kiválaszthatja a **bejövő szabályok szerkesztése** gombra.</span><span class="sxs-lookup"><span data-stu-id="9f83b-180">On the recommendation page, you can select the **Edit inbound rules** button.</span></span> <span data-ttu-id="9f83b-181">A felhasználói felület, ahhoz szükséges, hogy módosítsa a szabály akkor jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="9f83b-181">The UI that is needed to modify the rule appears.</span></span> 

![Javaslatok](./media/tutorial-azure-security/remediation.png)

<span data-ttu-id="9f83b-183">Javaslatok szervizelt vannak, mint megoldottként vannak beállítva.</span><span class="sxs-lookup"><span data-stu-id="9f83b-183">As recommendations are remediated, they are marked as resolved.</span></span> 

## <a name="view-detected-threats"></a><span data-ttu-id="9f83b-184">Észlelt fenyegetéseket megtekintése</span><span class="sxs-lookup"><span data-stu-id="9f83b-184">View detected threats</span></span>

<span data-ttu-id="9f83b-185">Mellett erőforrás konfigurációs javaslatait a Security Center figyelmeztetések jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="9f83b-185">In addition to resource configuration recommendations, Security Center displays threat detection alerts.</span></span> <span data-ttu-id="9f83b-186">A biztonsági riasztások szolgáltatás összesíti az egyes virtuális gép, Azure hálózati naplók és összekapcsolt partnermegoldások biztonsági fenyegetések ellen Azure-erőforrások észlelését gyűjtött adatokat.</span><span class="sxs-lookup"><span data-stu-id="9f83b-186">The security alerts feature aggregates data collected from each VM, Azure networking logs, and connected partner solutions to detect security threats against Azure resources.</span></span> <span data-ttu-id="9f83b-187">A Security Center threat detection képességeivel kapcsolatos részletesebb információk: [az Azure Security Center az észlelési képességek](../../security-center/security-center-detection-capabilities.md).</span><span class="sxs-lookup"><span data-stu-id="9f83b-187">For in-depth information about Security Center threat detection capabilities, see [Azure Security Center detection capabilities](../../security-center/security-center-detection-capabilities.md).</span></span>

<span data-ttu-id="9f83b-188">A biztonsági riasztások funkció használatához az IP-címek Security Center hogy *szabad* való *szabványos*.</span><span class="sxs-lookup"><span data-stu-id="9f83b-188">The security alerts feature requires the Security Center pricing tier to be increased from *Free* to *Standard*.</span></span> <span data-ttu-id="9f83b-189">Egy 30 napos **ingyenes próbaverzió** helyez át a magasabb szintű tarifacsomagban használható esetén érhető el.</span><span class="sxs-lookup"><span data-stu-id="9f83b-189">A 30-day **free trial** is available when you move to this higher pricing tier.</span></span> 

<span data-ttu-id="9f83b-190">Ez a tarifacsomag módosítása:</span><span class="sxs-lookup"><span data-stu-id="9f83b-190">To change the pricing tier:</span></span>  

1. <span data-ttu-id="9f83b-191">A Security Center irányítópultján kattintson **biztonsági házirend**, majd válassza ki az előfizetését.</span><span class="sxs-lookup"><span data-stu-id="9f83b-191">On the Security Center dashboard, click **Security policy**, and then select your subscription.</span></span>
2. <span data-ttu-id="9f83b-192">Válassza ki **tarifacsomag**.</span><span class="sxs-lookup"><span data-stu-id="9f83b-192">Select **Pricing tier**.</span></span>
3. <span data-ttu-id="9f83b-193">Az új réteget, majd válassza ki és **válasszon**.</span><span class="sxs-lookup"><span data-stu-id="9f83b-193">Select the new tier, and then select **Select**.</span></span>
4. <span data-ttu-id="9f83b-194">Az a **biztonsági házirend** panelen válassza **mentése**.</span><span class="sxs-lookup"><span data-stu-id="9f83b-194">On the **Security policy** blade, select **Save**.</span></span> 

<span data-ttu-id="9f83b-195">Miután megváltoztatta a tarifacsomagot, a biztonsági riasztások graph feltöltéséhez, mint a biztonsági fenyegetések észlelése kezdődik.</span><span class="sxs-lookup"><span data-stu-id="9f83b-195">After you've changed the pricing tier, the security alerts graph begins to populate as security threats are detected.</span></span>

![Biztonsági riasztások](./media/tutorial-azure-security/security-alerts.png)

<span data-ttu-id="9f83b-197">Válasszon ki egy riasztást, információk megtekintése.</span><span class="sxs-lookup"><span data-stu-id="9f83b-197">Select an alert to view information.</span></span> <span data-ttu-id="9f83b-198">Például láthatja a fenyegetés, az észlelés időpontja, az összes fenyegetés kísérletek és javasolt elhárítási műveletek leírását.</span><span class="sxs-lookup"><span data-stu-id="9f83b-198">For example, you can see a description of the threat, the detection time, all threat attempts, and the recommended remediation.</span></span> <span data-ttu-id="9f83b-199">A következő példában RDP találgatásos támadás volt észlelhető, 294 sikertelen RDP próbálnak.</span><span class="sxs-lookup"><span data-stu-id="9f83b-199">In the following example, an RDP brute-force attack was detected, with 294 failed RDP attempts.</span></span> <span data-ttu-id="9f83b-200">Javasolt megoldás valósul meg.</span><span class="sxs-lookup"><span data-stu-id="9f83b-200">A recommended resolution is provided.</span></span>

![RDP-támadás](./media/tutorial-azure-security/rdp-attack.png)

## <a name="next-steps"></a><span data-ttu-id="9f83b-202">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9f83b-202">Next steps</span></span>
<span data-ttu-id="9f83b-203">Ebben az oktatóanyagban az Azure Security Center beállítása, és ezután tekintse át a virtuális gépek a Security Center.</span><span class="sxs-lookup"><span data-stu-id="9f83b-203">In this tutorial, you set up Azure Security Center, and then reviewed VMs in Security Center.</span></span> <span data-ttu-id="9f83b-204">Megtudta, hogyan, hogy:</span><span class="sxs-lookup"><span data-stu-id="9f83b-204">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="9f83b-205">Adatgyűjtés beállítása</span><span class="sxs-lookup"><span data-stu-id="9f83b-205">Set up data collection</span></span>
> * <span data-ttu-id="9f83b-206">Biztonsági házirendek beállítása</span><span class="sxs-lookup"><span data-stu-id="9f83b-206">Set up security policies</span></span>
> * <span data-ttu-id="9f83b-207">Megtekintheti és konfigurációs állapotát kapcsolatos problémák megoldása</span><span class="sxs-lookup"><span data-stu-id="9f83b-207">View and fix configuration health issues</span></span>
> * <span data-ttu-id="9f83b-208">Tekintse át a fenyegetést észlelt</span><span class="sxs-lookup"><span data-stu-id="9f83b-208">Review detected threats</span></span>

<span data-ttu-id="9f83b-209">A következő oktatóanyag áttekintésével megismerheti, hogyan CI/CD folyamat létrehozása a Visual Studio Team Services és az IIS-t futtató Windows virtuális gép továbblépés.</span><span class="sxs-lookup"><span data-stu-id="9f83b-209">Advance to the next tutorial to learn how to create a CI/CD pipeline with Visual Studio Team Services and a Windows VM running IIS.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="9f83b-210">A Visual Studio Team Services CI/CD-folyamat</span><span class="sxs-lookup"><span data-stu-id="9f83b-210">Visual Studio Team Services CI/CD pipeline</span></span>](./tutorial-vsts-iis-cicd.md)
