---
title: "aaaAzure Security Center és a Windows virtuális gépek Azure-ban |} Microsoft Docs"
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
ms.openlocfilehash: 238bf4e266a24a536d35dd647db6056ab39a1f1a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-virtual-machine-security-by-using-azure-security-center"></a><span data-ttu-id="b3ac0-103">Virtuális gép biztonsági figyelje az Azure Security Centerben</span><span class="sxs-lookup"><span data-stu-id="b3ac0-103">Monitor virtual machine security by using Azure Security Center</span></span>

<span data-ttu-id="b3ac0-104">Az Azure Security Center segítségével, hogy lássák az Azure-erőforrás biztonsági eljárásokat.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-104">Azure Security Center can help you gain visibility into your Azure resource security practices.</span></span> <span data-ttu-id="b3ac0-105">A Security Center kínál, integrált biztonsági figyelés.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-105">Security Center offers integrated security monitoring.</span></span> <span data-ttu-id="b3ac0-106">Ellenkező esetben szabályzatkezelést fenyegetések azonosítására képes.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-106">It can detect threats that otherwise might go unnoticed.</span></span> <span data-ttu-id="b3ac0-107">Ebben az oktatóanyagban további tudnivalók az Azure Security Center, és hogyan:</span><span class="sxs-lookup"><span data-stu-id="b3ac0-107">In this tutorial, you learn about Azure Security Center, and how to:</span></span>
 
> [!div class="checklist"]
> * <span data-ttu-id="b3ac0-108">Adatgyűjtés beállítása</span><span class="sxs-lookup"><span data-stu-id="b3ac0-108">Set up data collection</span></span>
> * <span data-ttu-id="b3ac0-109">Biztonsági házirendek beállítása</span><span class="sxs-lookup"><span data-stu-id="b3ac0-109">Set up security policies</span></span>
> * <span data-ttu-id="b3ac0-110">Megtekintheti és konfigurációs állapotát kapcsolatos problémák megoldása</span><span class="sxs-lookup"><span data-stu-id="b3ac0-110">View and fix configuration health issues</span></span>
> * <span data-ttu-id="b3ac0-111">Tekintse át a fenyegetést észlelt</span><span class="sxs-lookup"><span data-stu-id="b3ac0-111">Review detected threats</span></span>  

## <a name="security-center-overview"></a><span data-ttu-id="b3ac0-112">A Security Center áttekintése</span><span class="sxs-lookup"><span data-stu-id="b3ac0-112">Security Center overview</span></span>

<span data-ttu-id="b3ac0-113">A Security Center lehetséges a virtuális gép (VM) konfigurációs problémák azonosítja, és biztonsági fenyegetések megcélzott.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-113">Security Center identifies potential virtual machine (VM) configuration issues and targeted security threats.</span></span> <span data-ttu-id="b3ac0-114">Ezek közé tartozik a virtuális gépek hálózati biztonsági csoportok, a nem titkosított lemezek és a találgatásos Remote Desktop Protocol (RDP) támadások hiányoznak.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-114">These might include VMs that are missing network security groups, unencrypted disks, and brute-force Remote Desktop Protocol (RDP) attacks.</span></span> <span data-ttu-id="b3ac0-115">hello Security Center irányítópultjának könnyen olvasható diagramokban hello információ látható.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-115">hello information is shown on hello Security Center dashboard in easy-to-read graphs.</span></span>

<span data-ttu-id="b3ac0-116">tooaccess hello hello hello menüjében válassza az Azure-portálon a Security Center irányítópultján **Security Center**.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-116">tooaccess hello Security Center dashboard, in hello Azure portal, on hello menu, select  **Security Center**.</span></span> <span data-ttu-id="b3ac0-117">Hello irányítópulton tekintse meg az Azure környezetben hello biztonsági állapotát, található az aktuális javaslatok számát, és hello fenyegetés riasztások aktuális állapotának megtekintése.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-117">On hello dashboard, you can see hello security health of your Azure environment, find a count of current recommendations, and view hello current state of threat alerts.</span></span> <span data-ttu-id="b3ac0-118">Minden egyes magas szintű diagram toosee kibontásával további információkhoz juthat.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-118">You can expand each high-level chart toosee more detail.</span></span>

![Security Center irányítópultjának](./media/tutorial-azure-security/asc-dash.png)

<span data-ttu-id="b3ac0-120">A Security Center túllép adatok felderítési tooprovide javaslatok, hogy az észlelt problémákat.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-120">Security Center goes beyond data discovery tooprovide recommendations for issues that it detects.</span></span> <span data-ttu-id="b3ac0-121">Például ha egy virtuális Gépet egy hálózati biztonsági csoport nélkül lett telepítve, a Security Center ajánlás olyan környezetekben, a javítási lépéseket, amelyek jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-121">For example, if a VM was deployed without an attached network security group, Security Center displays a recommendation, with remediation steps you can take.</span></span> <span data-ttu-id="b3ac0-122">Automatikus javítási kap a Security Center hello kontextusában maradjanak.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-122">You get automated remediation without leaving hello context of Security Center.</span></span>  

![Javaslatok](./media/tutorial-azure-security/recommendations.png)

## <a name="set-up-data-collection"></a><span data-ttu-id="b3ac0-124">Adatgyűjtés beállítása</span><span class="sxs-lookup"><span data-stu-id="b3ac0-124">Set up data collection</span></span>

<span data-ttu-id="b3ac0-125">Virtuális gép biztonsági beállításokat tud bejutni látható, meg kell tooset fel a Security Center adatgyűjtés.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-125">Before you can get visibility into VM security configurations, you need tooset up Security Center data collection.</span></span> <span data-ttu-id="b3ac0-126">Ebbe beletartozik az adatok gyűjtésének bekapcsolása, és létre kell hozni egy az Azure storage-fiók toohold gyűjtött adatait.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-126">This involves turning on data collection and creating an Azure storage account toohold collected data.</span></span> 

1. <span data-ttu-id="b3ac0-127">Az hello Security Center irányítópultján kattintson **biztonsági házirend**, majd válassza ki az előfizetését.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-127">On hello Security Center dashboard, click **Security policy**, and then select your subscription.</span></span> 
2. <span data-ttu-id="b3ac0-128">A **adatgyűjtés**, jelölje be **a**.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-128">For **Data collection**, select **On**.</span></span>
3. <span data-ttu-id="b3ac0-129">toocreate a storage-fiók kiválasztása **válasszon tárfiókot**.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-129">toocreate a storage account, select **Choose a storage account**.</span></span> <span data-ttu-id="b3ac0-130">Ezt követően válassza **OK**.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-130">Then, select **OK**.</span></span>
4. <span data-ttu-id="b3ac0-131">A hello **biztonsági házirend** panelen válassza **mentése**.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-131">On hello **Security Policy** blade, select **Save**.</span></span> 

<span data-ttu-id="b3ac0-132">hello Security Center adatok gyűjtemény ügynök majd telepítve van az összes virtuális gépet, és adatgyűjtés megkezdése.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-132">hello Security Center data collection agent is then installed on all VMs, and data collection begins.</span></span> 

## <a name="set-up-a-security-policy"></a><span data-ttu-id="b3ac0-133">A biztonsági házirendben</span><span class="sxs-lookup"><span data-stu-id="b3ac0-133">Set up a security policy</span></span>

<span data-ttu-id="b3ac0-134">Biztonsági házirendek tartoznak használt toodefine hello elemek, amelynek a Security Center adatokat gyűjt, és ajánlásokat.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-134">Security policies are used toodefine hello items for which Security Center collects data and makes recommendations.</span></span> <span data-ttu-id="b3ac0-135">Különböző biztonsági házirendek toodifferent beállítása az Azure-erőforrások is alkalmazhatja.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-135">You can apply different security policies toodifferent sets of Azure resources.</span></span> <span data-ttu-id="b3ac0-136">Bár az alapértelmezés szerint minden házirendelemek szemben Azure-erőforrások értékelődnek ki, kikapcsolhatja az összes Azure-erőforrások vagy erőforráscsoport egyedi házirend elemek.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-136">Although by default Azure resources are evaluated against all policy items, you can turn off individual policy items for all Azure resources or for a resource group.</span></span> <span data-ttu-id="b3ac0-137">A Security Center biztonsági házirendekkel kapcsolatos részletesebb információk: [biztonsági házirendek beállítása az Azure Security Centerben](../../security-center/security-center-policies.md).</span><span class="sxs-lookup"><span data-stu-id="b3ac0-137">For in-depth information about Security Center security policies, see [Set security policies in Azure Security Center](../../security-center/security-center-policies.md).</span></span> 

<span data-ttu-id="b3ac0-138">tooset be egy biztonsági házirendet, az összes Azure-erőforrások:</span><span class="sxs-lookup"><span data-stu-id="b3ac0-138">tooset up a security policy for all Azure resources:</span></span>

1. <span data-ttu-id="b3ac0-139">A Security Center irányítópultjának hello, jelölje be **biztonsági házirend**, majd válassza ki az előfizetését.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-139">On hello Security Center dashboard, select **Security policy**, and then select your subscription.</span></span>
2. <span data-ttu-id="b3ac0-140">Válassza ki **megakadályozási szabályzat**.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-140">Select **Prevention policy**.</span></span>
3. <span data-ttu-id="b3ac0-141">Kapcsolja be, vagy kapcsolja ki a megjeleníteni kívánt tooapply tooall házirendelemek Azure-erőforrások.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-141">Turn on or turn off policy items that you want tooapply tooall Azure resources.</span></span>
4. <span data-ttu-id="b3ac0-142">Amikor elkészült, válassza a beállítások, válassza ki a **OK**.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-142">When you're finished selecting your settings, select **OK**.</span></span>
5. <span data-ttu-id="b3ac0-143">A hello **biztonsági házirend** panelen válassza **mentése**.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-143">On hello **Security policy** blade, select **Save**.</span></span> 

<span data-ttu-id="b3ac0-144">a megadott erőforráscsoport-házirend tooset:</span><span class="sxs-lookup"><span data-stu-id="b3ac0-144">tooset up a policy for a specific resource group:</span></span>

1. <span data-ttu-id="b3ac0-145">A Security Center irányítópultjának hello, jelölje be **biztonsági házirend**, majd válassza ki egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-145">On hello Security Center dashboard, select **Security policy**, and then select a resource group.</span></span>
2. <span data-ttu-id="b3ac0-146">Válassza ki **megakadályozási szabályzat**.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-146">Select **Prevention policy**.</span></span>
3. <span data-ttu-id="b3ac0-147">Kapcsolja be, vagy kapcsolja ki a házirend elemek megjeleníteni kívánt tooapply toohello erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-147">Turn on or turn off policy items that you want tooapply toohello resource group.</span></span>
4. <span data-ttu-id="b3ac0-148">A **ÖRÖKLÉSI**, jelölje be **egyedi**.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-148">Under **INHERITANCE**, select **Unique**.</span></span>
5. <span data-ttu-id="b3ac0-149">Amikor elkészült, válassza a beállítások, válassza ki a **OK**.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-149">When you're finished selecting your settings, select **OK**.</span></span>
6. <span data-ttu-id="b3ac0-150">A hello **biztonsági házirend** panelen válassza **mentése**.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-150">On hello **Security policy** blade, select **Save**.</span></span>  

<span data-ttu-id="b3ac0-151">Ön is bármikor kikapcsolhatják az adatgyűjtést ezen a lapon megadott erőforráscsoport.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-151">You also can turn off data collection for a specific resource group on this page.</span></span>

<span data-ttu-id="b3ac0-152">A következő példa hello, egyedi házirendet létrejött nevű erőforráscsoport *myResoureGroup*.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-152">In hello following example, a unique policy has been created for a resource group named *myResoureGroup*.</span></span> <span data-ttu-id="b3ac0-153">Ezt a házirendet, a lemez titkosítása és a webes alkalmazás tűzfal javaslatok ki vannak kapcsolva.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-153">In this policy, disk encryption and web application firewall recommendations are turned off.</span></span>

![Egyedi házirend](./media/tutorial-azure-security/unique-policy.png)

## <a name="view-vm-configuration-health"></a><span data-ttu-id="b3ac0-155">Virtuális gép konfigurációs állapotának megtekintése</span><span class="sxs-lookup"><span data-stu-id="b3ac0-155">View VM configuration health</span></span>

<span data-ttu-id="b3ac0-156">Miután engedélyezve van a használatra vonatkozó adatok gyűjtésének és állíthat be a biztonsági házirendet, a Security Center tooprovide riasztások és javaslatok kezdődik.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-156">After you've turned on data collection and set a security policy, Security Center begins tooprovide alerts and recommendations.</span></span> <span data-ttu-id="b3ac0-157">Virtuális gépek vannak telepítve, mint hello adatváltozások gyűjtési ügynök telepítve van.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-157">As VMs are deployed, hello data collection agent is installed.</span></span> <span data-ttu-id="b3ac0-158">A Security Center majd fel van töltve a hello adatokkal új virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-158">Security Center is then populated with data for hello new VMs.</span></span> <span data-ttu-id="b3ac0-159">Részletes információ a virtuális gép konfigurációs állapotát: [a Security Center a virtuális gépek védelme](../../security-center/security-center-virtual-machine-recommendations.md).</span><span class="sxs-lookup"><span data-stu-id="b3ac0-159">For in-depth information about VM configuration health, see [Protect your VMs in Security Center](../../security-center/security-center-virtual-machine-recommendations.md).</span></span> 

<span data-ttu-id="b3ac0-160">Mivel összegyűjtött adatok hello erőforrás állapota minden egyes virtuális gép és a kapcsolódó Azure-erőforrás összesíti.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-160">As data is collected, hello resource health for each VM and related Azure resource is aggregated.</span></span> <span data-ttu-id="b3ac0-161">hello információk könnyen olvasható diagramon látható.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-161">hello information is shown in an easy-to-read chart.</span></span> 

<span data-ttu-id="b3ac0-162">tooview erőforrás állapota:</span><span class="sxs-lookup"><span data-stu-id="b3ac0-162">tooview resource health:</span></span>

1.  <span data-ttu-id="b3ac0-163">A Security Center irányítópultján hello alatt **erőforrás biztonsági állapota**, jelölje be **számítási**.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-163">On hello Security Center dashboard, under **Resource security health**, select **Compute**.</span></span> 
2.  <span data-ttu-id="b3ac0-164">A hello **számítási** panelen válassza **virtuális gépek**.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-164">On hello **Compute** blade, select **Virtual machines**.</span></span> <span data-ttu-id="b3ac0-165">Ez a nézet a virtuális gépek hello konfigurációs állapotának összegzését tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-165">This view provides a summary of hello configuration status for all your VMs.</span></span>

![Számítási állapota](./media/tutorial-azure-security/compute-health.png)

<span data-ttu-id="b3ac0-167">toosee ajánlások a virtuális gépek, válassza ki a hello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-167">toosee all recommendations for a VM, select hello VM.</span></span> <span data-ttu-id="b3ac0-168">Javaslatok, a javítási hello Ez az oktatóanyag következő szakasza részletesen ismertetnek.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-168">Recommendations and remediation are covered in more detail in hello next section of this tutorial.</span></span>

## <a name="remediate-configuration-issues"></a><span data-ttu-id="b3ac0-169">Konfigurációs problémák megoldásához</span><span class="sxs-lookup"><span data-stu-id="b3ac0-169">Remediate configuration issues</span></span>

<span data-ttu-id="b3ac0-170">A Security Center toopopulate konfigurációs adataival megkezdése után javaslatok alapján készülnek hello biztonsági házirend beállítása.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-170">After Security Center begins toopopulate with configuration data, recommendations are made based on hello security policy you set up.</span></span> <span data-ttu-id="b3ac0-171">Például ha egy virtuális Gépet egy társított hálózati biztonsági csoport nélkül hozták létre, egy javaslatokkal egy toocreate.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-171">For instance, if a VM was set up without an associated network security group, a recommendation is made toocreate one.</span></span> 

<span data-ttu-id="b3ac0-172">toosee összes ajánlás listája:</span><span class="sxs-lookup"><span data-stu-id="b3ac0-172">toosee a list of all recommendations:</span></span> 

1. <span data-ttu-id="b3ac0-173">A Security Center irányítópultjának hello, jelölje be **javaslatok**.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-173">On hello Security Center dashboard, select **Recommendations**.</span></span>
2. <span data-ttu-id="b3ac0-174">Válassza ki az adott javaslat.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-174">Select a specific recommendation.</span></span> <span data-ttu-id="b3ac0-175">Megjelenik egy lista, amelynek erőforrásait hello ajánlás vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-175">A list of all resources for which hello recommendation applies appears.</span></span>
3. <span data-ttu-id="b3ac0-176">tooapply ajánlás olyan környezetekben, válassza ki az adott erőforrás.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-176">tooapply a recommendation, select a specific resource.</span></span> 
4. <span data-ttu-id="b3ac0-177">Hello követésével javítási lépéseket.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-177">Follow hello instructions for remediation steps.</span></span> 

<span data-ttu-id="b3ac0-178">Sok esetben a Security Center lépéseit végrehajthatóként készíthet tooaddress ajánlást Security Center maradjanak.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-178">In many cases, Security Center provides actionable steps you can take tooaddress a recommendation without leaving Security Center.</span></span> <span data-ttu-id="b3ac0-179">A következő példa hello a Security Center észleli a hálózati biztonsági csoport, amely rendelkezik egy korlátlan bejövő szabályt.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-179">In hello following example, Security Center detects a network security group that has an unrestricted inbound rule.</span></span> <span data-ttu-id="b3ac0-180">Hello javaslat oldalon kiválaszthatja hello **bejövő szabályok szerkesztése** gombra.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-180">On hello recommendation page, you can select hello **Edit inbound rules** button.</span></span> <span data-ttu-id="b3ac0-181">felhasználói felület, amely szükséges toomodify hello szabály hello jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-181">hello UI that is needed toomodify hello rule appears.</span></span> 

![Javaslatok](./media/tutorial-azure-security/remediation.png)

<span data-ttu-id="b3ac0-183">Javaslatok szervizelt vannak, mint megoldottként vannak beállítva.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-183">As recommendations are remediated, they are marked as resolved.</span></span> 

## <a name="view-detected-threats"></a><span data-ttu-id="b3ac0-184">Észlelt fenyegetéseket megtekintése</span><span class="sxs-lookup"><span data-stu-id="b3ac0-184">View detected threats</span></span>

<span data-ttu-id="b3ac0-185">Továbbá a tooresource konfigurációs javaslatait a Security Center figyelmeztetések jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-185">In addition tooresource configuration recommendations, Security Center displays threat detection alerts.</span></span> <span data-ttu-id="b3ac0-186">hello biztonsági riasztások szolgáltatás összesíti az egyes virtuális gép, Azure hálózati naplók és összekapcsolt partneri megoldások toodetect biztonsági fenyegetések ellen Azure-erőforrások gyűjtött adatokat.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-186">hello security alerts feature aggregates data collected from each VM, Azure networking logs, and connected partner solutions toodetect security threats against Azure resources.</span></span> <span data-ttu-id="b3ac0-187">A Security Center threat detection képességeivel kapcsolatos részletesebb információk: [az Azure Security Center az észlelési képességek](../../security-center/security-center-detection-capabilities.md).</span><span class="sxs-lookup"><span data-stu-id="b3ac0-187">For in-depth information about Security Center threat detection capabilities, see [Azure Security Center detection capabilities](../../security-center/security-center-detection-capabilities.md).</span></span>

<span data-ttu-id="b3ac0-188">hello biztonsági riasztások szolgáltatáshoz szükséges hello Security Center árképzési szint toobe közötti *szabad* túl*szabványos*.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-188">hello security alerts feature requires hello Security Center pricing tier toobe increased from *Free* too*Standard*.</span></span> <span data-ttu-id="b3ac0-189">Egy 30 napos **ingyenes próbaverzió** magasabb tarifacsomagra toothis áthelyezése esetén érhető el.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-189">A 30-day **free trial** is available when you move toothis higher pricing tier.</span></span> 

<span data-ttu-id="b3ac0-190">toochange hello árképzési szint:</span><span class="sxs-lookup"><span data-stu-id="b3ac0-190">toochange hello pricing tier:</span></span>  

1. <span data-ttu-id="b3ac0-191">Az hello Security Center irányítópultján kattintson **biztonsági házirend**, majd válassza ki az előfizetését.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-191">On hello Security Center dashboard, click **Security policy**, and then select your subscription.</span></span>
2. <span data-ttu-id="b3ac0-192">Válassza ki **tarifacsomag**.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-192">Select **Pricing tier**.</span></span>
3. <span data-ttu-id="b3ac0-193">Új réteg hello, majd válassza ki és **válasszon**.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-193">Select hello new tier, and then select **Select**.</span></span>
4. <span data-ttu-id="b3ac0-194">A hello **biztonsági házirend** panelen válassza **mentése**.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-194">On hello **Security policy** blade, select **Save**.</span></span> 

<span data-ttu-id="b3ac0-195">Miután megváltoztatta a hello IP-címek, hello biztonsági riasztások graph indul toopopulate biztonsági fenyegetések észlelése.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-195">After you've changed hello pricing tier, hello security alerts graph begins toopopulate as security threats are detected.</span></span>

![Biztonsági riasztások](./media/tutorial-azure-security/security-alerts.png)

<span data-ttu-id="b3ac0-197">Válassza ki a riasztási tooview információ.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-197">Select an alert tooview information.</span></span> <span data-ttu-id="b3ac0-198">Például hello fenyegetés, hello észlelés ideje, az összes fenyegetés kísérletek leírása, és ajánlott javítási hello.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-198">For example, you can see a description of hello threat, hello detection time, all threat attempts, and hello recommended remediation.</span></span> <span data-ttu-id="b3ac0-199">A következő példa hello RDP találgatásos támadás volt észlelhető, 294 sikertelen RDP próbálnak.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-199">In hello following example, an RDP brute-force attack was detected, with 294 failed RDP attempts.</span></span> <span data-ttu-id="b3ac0-200">Javasolt megoldás valósul meg.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-200">A recommended resolution is provided.</span></span>

![RDP-támadás](./media/tutorial-azure-security/rdp-attack.png)

## <a name="next-steps"></a><span data-ttu-id="b3ac0-202">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b3ac0-202">Next steps</span></span>
<span data-ttu-id="b3ac0-203">Ebben az oktatóanyagban az Azure Security Center beállítása, és ezután tekintse át a virtuális gépek a Security Center.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-203">In this tutorial, you set up Azure Security Center, and then reviewed VMs in Security Center.</span></span> <span data-ttu-id="b3ac0-204">Megismerte, hogyan végezheti el az alábbi műveleteket:</span><span class="sxs-lookup"><span data-stu-id="b3ac0-204">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b3ac0-205">Adatgyűjtés beállítása</span><span class="sxs-lookup"><span data-stu-id="b3ac0-205">Set up data collection</span></span>
> * <span data-ttu-id="b3ac0-206">Biztonsági házirendek beállítása</span><span class="sxs-lookup"><span data-stu-id="b3ac0-206">Set up security policies</span></span>
> * <span data-ttu-id="b3ac0-207">Megtekintheti és konfigurációs állapotát kapcsolatos problémák megoldása</span><span class="sxs-lookup"><span data-stu-id="b3ac0-207">View and fix configuration health issues</span></span>
> * <span data-ttu-id="b3ac0-208">Tekintse át a fenyegetést észlelt</span><span class="sxs-lookup"><span data-stu-id="b3ac0-208">Review detected threats</span></span>

<span data-ttu-id="b3ac0-209">Előzetes toohello következő útmutató toolearn toocreate CI/CD-ről a Visual Studio Team Services csővezeték- és IIS-t futtató Windows virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="b3ac0-209">Advance toohello next tutorial toolearn how toocreate a CI/CD pipeline with Visual Studio Team Services and a Windows VM running IIS.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="b3ac0-210">A Visual Studio Team Services CI/CD-folyamat</span><span class="sxs-lookup"><span data-stu-id="b3ac0-210">Visual Studio Team Services CI/CD pipeline</span></span>](./tutorial-vsts-iis-cicd.md)
