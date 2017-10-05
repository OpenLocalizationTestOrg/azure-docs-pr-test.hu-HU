---
title: "Hozzon létre egy Azure virtuális hálózati társviszony - Resource Manager - ugyanahhoz az előfizetéshez |} Microsoft Docs"
description: "Megtudhatja, hogyan hozzon létre egy virtuális hálózati társviszony-létesítés erőforrás-kezelőt hozott létre, amely szerepel az azonos Azure-előfizetés virtuális hálózatok között."
services: virtual-network
documentationcenter: 
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 026bca75-2946-4c03-b4f6-9f3c5809c69a
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/17/2017
ms.author: jdial;narayan;annahar
ms.openlocfilehash: a32a6b33e04c603325ab3612f61e5852682eac7d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="create-a-virtual-network-peering---resource-manager-same-subscription"></a><span data-ttu-id="2528a-103">Hozzon létre egy virtuális hálózati társviszony - erőforrás-kezelő ugyanahhoz az előfizetéshez</span><span class="sxs-lookup"><span data-stu-id="2528a-103">Create a virtual network peering - Resource Manager, same subscription</span></span>

<span data-ttu-id="2528a-104">Ebben az oktatóanyagban elsajátíthatja társviszony-létesítés Resource Manager használatával létrehozott virtuális hálózatok közötti virtuális hálózat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="2528a-104">In this tutorial, you learn to create a virtual network peering between virtual networks created through Resource Manager.</span></span> <span data-ttu-id="2528a-105">Mindkét virtuális hálózat szerepel a ugyanahhoz az előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="2528a-105">Both virtual networks exist in the same subscription.</span></span> <span data-ttu-id="2528a-106">A különböző virtuális hálózatokon kommunikálni egymással sávszélesség és a késés, mintha az erőforrások volt az azonos virtuális hálózatban lévő két virtuális hálózatok lehetővé teszi, hogy erőforrásokat társviszony-létesítés.</span><span class="sxs-lookup"><span data-stu-id="2528a-106">Peering two virtual networks enables resources in different virtual networks to communicate with each other with the same bandwidth and latency as though the resources were in the same virtual network.</span></span> <span data-ttu-id="2528a-107">További információ [virtuális hálózati társviszony-létesítés](virtual-network-peering-overview.md).</span><span class="sxs-lookup"><span data-stu-id="2528a-107">Learn more about [Virtual network peering](virtual-network-peering-overview.md).</span></span> 

<span data-ttu-id="2528a-108">Virtuális hálózati társviszony-létesítés létrehozásának lépései eltérőek, attól függően, hogy a virtuális hálózatok ugyanazon, vagy másik, előfizetések, és amely [Azure telepítési modell](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) a virtuális hálózatok használatával jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="2528a-108">The steps to create a virtual network peering are different, depending on whether the virtual networks are in the same, or different, subscriptions, and which [Azure deployment model](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) the virtual networks are created through.</span></span> <span data-ttu-id="2528a-109">Megtudhatja, hogyan hozhat létre virtuális hálózatot gombra kattintva a következő táblázat a forgatókönyv más esetekben társviszony:</span><span class="sxs-lookup"><span data-stu-id="2528a-109">Learn how to create a virtual network peering in other scenarios by clicking the scenario from the following table:</span></span>

|<span data-ttu-id="2528a-110">Azure üzembehelyezési modell</span><span class="sxs-lookup"><span data-stu-id="2528a-110">Azure deployment model</span></span>  | <span data-ttu-id="2528a-111">Azure-előfizetés</span><span class="sxs-lookup"><span data-stu-id="2528a-111">Azure subscription</span></span>  |
|--------- |---------|
|[<span data-ttu-id="2528a-112">Mindkét erőforrás-kezelő</span><span class="sxs-lookup"><span data-stu-id="2528a-112">Both Resource Manager</span></span>](create-peering-different-subscriptions.md) |<span data-ttu-id="2528a-113">Különböző</span><span class="sxs-lookup"><span data-stu-id="2528a-113">Different</span></span>|
|[<span data-ttu-id="2528a-114">Egy erőforrás-kezelő egy klasszikus</span><span class="sxs-lookup"><span data-stu-id="2528a-114">One Resource Manager, one classic</span></span>](create-peering-different-deployment-models.md) |<span data-ttu-id="2528a-115">Azonos</span><span class="sxs-lookup"><span data-stu-id="2528a-115">Same</span></span>|
|[<span data-ttu-id="2528a-116">Egy erőforrás-kezelő egy klasszikus</span><span class="sxs-lookup"><span data-stu-id="2528a-116">One Resource Manager, one classic</span></span>](create-peering-different-deployment-models-subscriptions.md) |<span data-ttu-id="2528a-117">Különböző</span><span class="sxs-lookup"><span data-stu-id="2528a-117">Different</span></span>|

<span data-ttu-id="2528a-118">Virtuális hálózati társviszony-létesítés nem hozható létre, a klasszikus üzembe helyezési modellben telepített virtuális hálózatok között.</span><span class="sxs-lookup"><span data-stu-id="2528a-118">A virtual network peering cannot be created between two virtual networks deployed through the classic deployment model.</span></span> <span data-ttu-id="2528a-119">Virtuális hálózati társviszony-létesítés csak létrehozhatók, amelyek azonos Azure-régióban található virtuális hálózatok között.</span><span class="sxs-lookup"><span data-stu-id="2528a-119">A virtual network peering can only be created between two virtual networks that exist in the same Azure region.</span></span> <span data-ttu-id="2528a-120">Ha mindkét létrehozott a klasszikus üzembe helyezési modell használatával, és különböző Azure-régiók meglévő virtuális hálózatok csatlakoztatni kell, használhatja az Azure [VPN-átjáró](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) virtuális hálózathoz csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="2528a-120">If you need to connect virtual networks that were both created through the classic deployment model, or that exist in different Azure regions, you can use an Azure [VPN Gateway](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) to connect the virtual networks.</span></span> 

<span data-ttu-id="2528a-121">Használhatja a [Azure-portálon](#portal), az Azure [parancssori felület](#cli) (CLI), Azure [PowerShell](#powershell), vagy egy [Azure Resource Manager sablon](#template)létrehozni a virtuális hálózati társviszony-létesítés.</span><span class="sxs-lookup"><span data-stu-id="2528a-121">You can use the [Azure portal](#portal), the Azure [command-line interface](#cli) (CLI), Azure [PowerShell](#powershell), or an [Azure Resource Manager template](#template) to create a virtual network peering.</span></span> <span data-ttu-id="2528a-122">Kattintson az előző eszköz hivatkozásokra kattintva közvetlenül Ugrás a virtuális hálózati társviszony-létesítés a eszközzel választott létrehozásához szükséges lépésekről.</span><span class="sxs-lookup"><span data-stu-id="2528a-122">Click any of the previous tool links to go directly to the steps for creating a virtual network peering using your tool of choice.</span></span>

## <span data-ttu-id="2528a-123"><a name="portal"></a>Hozzon létre a társviszony - Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="2528a-123"><a name="portal"></a>Create peering - Azure portal</span></span>

1. <span data-ttu-id="2528a-124">Jelentkezzen be az [Azure portálra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="2528a-124">Log in to the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="2528a-125">A fiókkal jelentkezik be az virtuális hálózati társviszony-létesítés létrehozásához szükséges engedélyekkel kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="2528a-125">The account you log in with must have the necessary permissions to create a virtual network peering.</span></span> <span data-ttu-id="2528a-126">Tekintse meg a [engedélyek](#permissions) jelen cikkben alább szakasza.</span><span class="sxs-lookup"><span data-stu-id="2528a-126">See the [Permissions](#permissions) section of this article for details.</span></span>
2. <span data-ttu-id="2528a-127">Kattintson a **+ új**, kattintson a **hálózati**, majd kattintson a **virtuális hálózati**.</span><span class="sxs-lookup"><span data-stu-id="2528a-127">Click **+ New**, click **Networking**, then click **Virtual network**.</span></span>
3. <span data-ttu-id="2528a-128">Az a **virtuális hálózat létrehozása** panelen adja meg, vagy válassza ki a következő beállítások értékeit, majd kattintson **létrehozása**:</span><span class="sxs-lookup"><span data-stu-id="2528a-128">In the **Create virtual network** blade, enter, or select values for the following settings, then click **Create**:</span></span>
    - <span data-ttu-id="2528a-129">**Név**: *myVnet1*</span><span class="sxs-lookup"><span data-stu-id="2528a-129">**Name**: *myVnet1*</span></span>
    - <span data-ttu-id="2528a-130">**Címtér**: *10.0.0.0/16*</span><span class="sxs-lookup"><span data-stu-id="2528a-130">**Address space**: *10.0.0.0/16*</span></span>
    - <span data-ttu-id="2528a-131">**Alhálózati név**: *alapértelmezett*</span><span class="sxs-lookup"><span data-stu-id="2528a-131">**Subnet name**: *default*</span></span>
    - <span data-ttu-id="2528a-132">**Alhálózati címtartományt**: *10.0.0.0/24*</span><span class="sxs-lookup"><span data-stu-id="2528a-132">**Subnet address range**: *10.0.0.0/24*</span></span>
    - <span data-ttu-id="2528a-133">**Előfizetés**: Jelölje ki az előfizetését</span><span class="sxs-lookup"><span data-stu-id="2528a-133">**Subscription**: Select your subscription</span></span>
    - <span data-ttu-id="2528a-134">**Erőforráscsoport**: válasszon **hozzon létre új** , és írja be *myResourceGroup*</span><span class="sxs-lookup"><span data-stu-id="2528a-134">**Resource group**: Select **Create new** and enter *myResourceGroup*</span></span>
    - <span data-ttu-id="2528a-135">**Hely**: *USA keleti régiója*</span><span class="sxs-lookup"><span data-stu-id="2528a-135">**Location**: *East US*</span></span>
4. <span data-ttu-id="2528a-136">Végezze el újra a következő értékek megadása 3. lépés 2-3 lépéseket:</span><span class="sxs-lookup"><span data-stu-id="2528a-136">Complete steps 2-3 again specifying the following values in step 3:</span></span>
    - <span data-ttu-id="2528a-137">**Név**: *myVnet2*</span><span class="sxs-lookup"><span data-stu-id="2528a-137">**Name**: *myVnet2*</span></span>
    - <span data-ttu-id="2528a-138">**Címtér**: *10.1.0.0/16*</span><span class="sxs-lookup"><span data-stu-id="2528a-138">**Address space**: *10.1.0.0/16*</span></span>
    - <span data-ttu-id="2528a-139">**Alhálózati név**: *alapértelmezett*</span><span class="sxs-lookup"><span data-stu-id="2528a-139">**Subnet name**: *default*</span></span>
    - <span data-ttu-id="2528a-140">**Alhálózati címtartományt**: *10.1.0.0/24*</span><span class="sxs-lookup"><span data-stu-id="2528a-140">**Subnet address range**: *10.1.0.0/24*</span></span>
    - <span data-ttu-id="2528a-141">**Előfizetés**: Jelölje ki az előfizetését</span><span class="sxs-lookup"><span data-stu-id="2528a-141">**Subscription**: Select your subscription</span></span>
    - <span data-ttu-id="2528a-142">**Erőforráscsoport**: válasszon **meglévő** válassza *myResourceGroup*</span><span class="sxs-lookup"><span data-stu-id="2528a-142">**Resource group**: Select **Use existing** and select *myResourceGroup*</span></span>
    - <span data-ttu-id="2528a-143">**Hely**: *USA keleti régiója*</span><span class="sxs-lookup"><span data-stu-id="2528a-143">**Location**: *East US*</span></span>
5. <span data-ttu-id="2528a-144">Az a **keresési erőforrások** típus a portál felső részén *myResourceGroup*.</span><span class="sxs-lookup"><span data-stu-id="2528a-144">In the **Search resources** box at the top of the portal, type *myResourceGroup*.</span></span> <span data-ttu-id="2528a-145">Kattintson a **myResourceGroup** amikor megjelenik a keresési eredmények között.</span><span class="sxs-lookup"><span data-stu-id="2528a-145">Click **myResourceGroup** when it appears in the search results.</span></span> <span data-ttu-id="2528a-146">A panel jelenik meg a **myresourcegroup** erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="2528a-146">A blade appears for the **myresourcegroup** resource group.</span></span> <span data-ttu-id="2528a-147">Az erőforráscsoport az előző lépésekben létrehozott két virtuális hálózatot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="2528a-147">The resource group contains the two virtual networks created in previous steps.</span></span>
6. <span data-ttu-id="2528a-148">Kattintson a **myVNet1**.</span><span class="sxs-lookup"><span data-stu-id="2528a-148">Click **myVNet1**.</span></span>
7. <span data-ttu-id="2528a-149">Az a **myVnet1** panel, amelyen megjelenik, kattintson a **Társviszony** a függőleges a panel bal oldalán lehetőségek közül.</span><span class="sxs-lookup"><span data-stu-id="2528a-149">In the **myVnet1** blade that appears, click **Peerings** from the vertical list of options on the left side of the blade.</span></span>
8. <span data-ttu-id="2528a-150">Az a **myVnet1 - esetében** panel, amelyen jelent meg, kattintson a **+ Hozzáadás**</span><span class="sxs-lookup"><span data-stu-id="2528a-150">In the **myVnet1 - Peerings** blade that appeared, click **+ Add**</span></span>
9. <span data-ttu-id="2528a-151">Az a **Hozzáadás társviszony-létesítés** panelen megjelenő, adja meg, vagy válassza a következő beállításokat, majd kattintson **OK**:</span><span class="sxs-lookup"><span data-stu-id="2528a-151">In the **Add peering** blade that appears, enter, or select the following options, then click **OK**:</span></span>
     - <span data-ttu-id="2528a-152">**Név**: *myVnet1ToMyVnet2*</span><span class="sxs-lookup"><span data-stu-id="2528a-152">**Name**: *myVnet1ToMyVnet2*</span></span>
     - <span data-ttu-id="2528a-153">**Virtuális hálózat telepítési modell**: válasszon **erőforrás-kezelő**.</span><span class="sxs-lookup"><span data-stu-id="2528a-153">**Virtual network deployment model**:  Select **Resource Manager**.</span></span> 
     - <span data-ttu-id="2528a-154">**Előfizetés**: Jelölje ki az előfizetését</span><span class="sxs-lookup"><span data-stu-id="2528a-154">**Subscription**: Select your subscription</span></span>
     - <span data-ttu-id="2528a-155">**Virtuális hálózati**: kattintson a **virtuális hálózatot választ**, majd kattintson a **myVnet2**.</span><span class="sxs-lookup"><span data-stu-id="2528a-155">**Virtual network**:  Click **Choose a virtual network**, then click **myVnet2**.</span></span>
     - <span data-ttu-id="2528a-156">**Virtuális hálózati hozzáférés engedélyezése:** ügyeljen arra, hogy **engedélyezve** van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="2528a-156">**Allow virtual network access:** Ensure that **Enabled** is selected.</span></span>
    <span data-ttu-id="2528a-157">Ebben az oktatóanyagban nincs más beállítások használhatók.</span><span class="sxs-lookup"><span data-stu-id="2528a-157">No other settings are used in this tutorial.</span></span> <span data-ttu-id="2528a-158">Minden társviszony-létesítési beállításaival kapcsolatos további tudnivalókért olvassa el a [kezelheti a virtuális hálózati társviszony](virtual-network-manage-peering.md#create-a-peering).</span><span class="sxs-lookup"><span data-stu-id="2528a-158">To learn about all peering settings, read [Manage virtual network peerings](virtual-network-manage-peering.md#create-a-peering).</span></span>
10. <span data-ttu-id="2528a-159">Kattintás után **OK** az előző lépésben a **Hozzáadás társviszony-létesítés** panel bezárása után, és megjelenik a **myVnet1 - Társviszony** újra a panelt.</span><span class="sxs-lookup"><span data-stu-id="2528a-159">After clicking **OK** in the previous step, the **Add peering** blade closes and you see the **myVnet1 - Peerings** blade again.</span></span> <span data-ttu-id="2528a-160">Néhány másodpercen belül a társviszony-létesítés létrehozott jelenik meg a panelt.</span><span class="sxs-lookup"><span data-stu-id="2528a-160">After a few seconds, the peering you created appears in the blade.</span></span> <span data-ttu-id="2528a-161">**Kezdeményezett** szerepel a **társviszony-LÉTESÍTÉS állapot** oszlopában a **myVnet1ToMyVnet2** társviszony-létesítés meg létrehozni.</span><span class="sxs-lookup"><span data-stu-id="2528a-161">**Initiated** is listed in the **PEERING STATUS** column for the **myVnet1ToMyVnet2** peering you created.</span></span> <span data-ttu-id="2528a-162">Már nincsenek társviszonyban Vnet1 Vnet2 számára, de most a myVnet1 myVnet2 kell partnert.</span><span class="sxs-lookup"><span data-stu-id="2528a-162">You've peered Vnet1 to Vnet2, but now you must peer myVnet2 to myVnet1.</span></span> <span data-ttu-id="2528a-163">A társviszony-létesítést ahhoz, hogy a virtuális hálózatok kommunikálnak egymással erőforrások mindkét irányban kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="2528a-163">The peering must be created in both directions to enable resources in the virtual networks to communicate with each other.</span></span>
11. <span data-ttu-id="2528a-164">Végezze el újra az myVnet2 5-10 lépéseket.</span><span class="sxs-lookup"><span data-stu-id="2528a-164">Complete steps 5-10 again for myVnet2.</span></span>  <span data-ttu-id="2528a-165">A társviszony-létesítést name *myVnet2ToMyVnet1*.</span><span class="sxs-lookup"><span data-stu-id="2528a-165">Name the peering *myVnet2ToMyVnet1*.</span></span>
12. <span data-ttu-id="2528a-166">Néhány másodpercen belül kattintás után **OK** MyVnet2, a társviszony létrehozásához a **myVnet2ToMyVnet1** társviszony-létesítést az imént létrehozott szerepel **csatlakoztatva** a a  **Társviszony-LÉTESÍTÉS állapot** oszlop.</span><span class="sxs-lookup"><span data-stu-id="2528a-166">A few seconds after clicking **OK** to create the peering for MyVnet2, the **myVnet2ToMyVnet1** peering you just created is listed with **Connected** in the **PEERING STATUS** column.</span></span>
13. <span data-ttu-id="2528a-167">Újra 5-7 lépéseinek elvégzését MyVnet1.</span><span class="sxs-lookup"><span data-stu-id="2528a-167">Complete steps 5-7 again for MyVnet1.</span></span> <span data-ttu-id="2528a-168">A **társviszony-LÉTESÍTÉS állapot** a a **myVnet1ToVNet2** társviszony-létesítés már is **csatlakoztatva**.</span><span class="sxs-lookup"><span data-stu-id="2528a-168">The **PEERING STATUS** for the **myVnet1ToVNet2** peering is now also **Connected**.</span></span> <span data-ttu-id="2528a-169">A társviszony-létesítést sikeresen létrejött, miután látta, **csatlakoztatva** a a **társviszony-LÉTESÍTÉS állapot** oszlop társviszony-létesítés mindkét virtuális hálózat számára.</span><span class="sxs-lookup"><span data-stu-id="2528a-169">The peering is successfully established after you see **Connected** in the **PEERING STATUS** column for both virtual networks in the peering.</span></span>
14. <span data-ttu-id="2528a-170">**Nem kötelező**: abban az esetben, ha ez az oktatóanyag nem vonatkozik a virtuális gépek létrehozását, hozzon létre egy virtuális gép minden egyes virtuális hálózati, és csatlakoztassa egy virtuális gépről a másikra, való kapcsolat ellenőrzésére.</span><span class="sxs-lookup"><span data-stu-id="2528a-170">**Optional**: Though creating virtual machines is not covered in this tutorial, you can create a virtual machine in each virtual network and connect from one virtual machine to the other, to validate connectivity.</span></span>
15. <span data-ttu-id="2528a-171">**Nem kötelező**: az erőforrásokat, amelyek ebben az oktatóanyagban létrehozhat törléséhez lépéseinek végrehajtásához a [törli az erőforrást](#delete-portal) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="2528a-171">**Optional**: To delete the resources that you create in this tutorial, complete the steps in the [Delete resources](#delete-portal) section of this article.</span></span>

<span data-ttu-id="2528a-172">Bármely Azure-hoz létre vagy virtuális hálózati erőforrások is az IP-címek keresztül kommunikálnak egymással.</span><span class="sxs-lookup"><span data-stu-id="2528a-172">Any Azure resources you create in either virtual network are now able to communicate with each other through their IP addresses.</span></span> <span data-ttu-id="2528a-173">Alapértelmezett Azure névfeloldást használ a virtuális hálózatok, a virtuális hálózatok erőforrások esetén nem tudják feloldani a virtuális hálózatok közötti.</span><span class="sxs-lookup"><span data-stu-id="2528a-173">If you're using default Azure name resolution for the virtual networks, the resources in the virtual networks are not able to resolve names across the virtual networks.</span></span> <span data-ttu-id="2528a-174">Ha szeretné feloldani egy társviszony-létesítés virtuális hálózatok között, létre kell hoznia a saját DNS-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="2528a-174">If you want to resolve names across virtual networks in a peering, you must create your own DNS server.</span></span> <span data-ttu-id="2528a-175">Ismerje meg, hogyan állíthat be [névfeloldáshoz a saját DNS-kiszolgáló](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).</span><span class="sxs-lookup"><span data-stu-id="2528a-175">Learn how to set up [Name resolution using your own DNS server](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).</span></span>

## <span data-ttu-id="2528a-176"><a name="cli"></a>Társviszony - létrehozása az Azure parancssori felület</span><span class="sxs-lookup"><span data-stu-id="2528a-176"><a name="cli"></a>Create peering - Azure CLI</span></span>

<span data-ttu-id="2528a-177">A következő parancsfájlt:</span><span class="sxs-lookup"><span data-stu-id="2528a-177">The following script:</span></span>

- <span data-ttu-id="2528a-178">Van szükség az Azure parancssori felület 2.0.4 verzió vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="2528a-178">Requires the Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="2528a-179">A verzió található, futtassa a `az --version` parancsot.</span><span class="sxs-lookup"><span data-stu-id="2528a-179">To find the version, run the `az --version` command.</span></span> <span data-ttu-id="2528a-180">Ha frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="2528a-180">If you need to upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json).</span></span>
- <span data-ttu-id="2528a-181">A rendszerhéjakba működik.</span><span class="sxs-lookup"><span data-stu-id="2528a-181">Works in a Bash shell.</span></span> <span data-ttu-id="2528a-182">Az Azure parancssori felület parancsfájlok futtatásához a Windows-ügyfelén beállítások, lásd: [futtatása az Azure parancssori felület a Windows](../virtual-machines/windows/cli-options.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="2528a-182">For options on running Azure CLI scripts on Windows client, see [Running the Azure CLI in Windows](../virtual-machines/windows/cli-options.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span></span> 

<span data-ttu-id="2528a-183">A parancssori felület és függőségeinek telepítése, helyett az Azure-felhő rendszerhéj is használhatja.</span><span class="sxs-lookup"><span data-stu-id="2528a-183">Instead of installing the CLI and its dependencies, you can use the Azure Cloud Shell.</span></span> <span data-ttu-id="2528a-184">Az Azure Cloud Shell olyan ingyenes Bash-felület, amelyet közvetlenül futtathat az Azure Portalon.</span><span class="sxs-lookup"><span data-stu-id="2528a-184">The Azure Cloud Shell is a free Bash shell that you can run directly within the Azure portal.</span></span> <span data-ttu-id="2528a-185">A fiókjával való használat érdekében az Azure CLI már előre telepítve és konfigurálva van rajta.</span><span class="sxs-lookup"><span data-stu-id="2528a-185">It has the Azure CLI preinstalled and configured to use with your account.</span></span> <span data-ttu-id="2528a-186">Kattintson a **kipróbálás** gombra a parancsfájl, amely a következő, a felhő rendszerhéjat, amelyre bejelentkezik, amellyel bejelentkezhet az Azure-fiókjába.</span><span class="sxs-lookup"><span data-stu-id="2528a-186">Click the **Try it** button in the script that follows, which invokes a Cloud Shell that logs you can log in to your Azure account with.</span></span> <span data-ttu-id="2528a-187">A parancsfájl végrehajtására, kattintson a **másolási** gombra, majd a tartalom illessze be a felhő rendszerhéj.</span><span class="sxs-lookup"><span data-stu-id="2528a-187">To execute the script, click the **Copy** button and paste the contents into your Cloud Shell.</span></span>

1. <span data-ttu-id="2528a-188">Hozzon létre egy erőforráscsoportot és két virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="2528a-188">Create a resource group and two virtual networks.</span></span>

    ```azurecli-interactive
    #!/bin/bash

    # Variables for common values used throughout the script.
    rgName="myResourceGroup"
    location="eastus"

    # Create a resource group.
    az group create \
      --name $rgName \
      --location $location

    # Create virtual network 1.
    az network vnet create \
      --name myVnet1 \
      --resource-group $rgName \
      --location $location \
      --address-prefix 10.0.0.0/16

    # Create virtual network 2.
    az network vnet create \
      --name myVnet2 \
      --resource-group $rgName \
      --location $location \
      --address-prefix 10.1.0.0/16
    ```

2. <span data-ttu-id="2528a-189">Hozzon létre egy virtuális hálózati társviszony-létesítés a két virtuális hálózatok között.</span><span class="sxs-lookup"><span data-stu-id="2528a-189">Create a virtual network peering between the two virtual networks.</span></span>
 
    ```azurecli-interactive
    # Get the id for VNet1.
    vnet1Id=$(az network vnet show \
      --resource-group $rgName \
      --name myVnet1 \
      --query id --out tsv)

    # Get the id for VNet2.
    vnet2Id=$(az network vnet show \
      --resource-group $rgName \
      --name myVnet2 \
      --query id \
      --out tsv)

    # Peer VNet1 to VNet2.
    az network vnet peering create \
      --name myVnet1ToMyVnet2 \
      --resource-group $rgName \
      --vnet-name myVnet1 \
      --remote-vnet-id $vnet2Id \
      --allow-vnet-access

    # Peer VNet2 to VNet1.
    az network vnet peering create \
      --name myVnet2ToMyVnet1 \
      --resource-group $rgName \
      --vnet-name myVnet2 \
      --remote-vnet-id $vnet1Id \
      --allow-vnet-access
    ```

3. <span data-ttu-id="2528a-190">Miután a parancsfájl végrehajtása során, tekintse át az egyes virtuális hálózati társviszony.</span><span class="sxs-lookup"><span data-stu-id="2528a-190">After the script executes, review the peerings for each virtual network.</span></span> 

    ```azurecli-interactive
    az network vnet peering list \
      --resource-group myResourceGroup \
      --vnet-name myVnet1 \
      --output table
    ```
    
    <span data-ttu-id="2528a-191">Futtassa az előző parancsot újból, cseréje *myVnet1* rendelkező *myVnet2*.</span><span class="sxs-lookup"><span data-stu-id="2528a-191">Run the previous command again, replacing *myVnet1* with *myVnet2*.</span></span> <span data-ttu-id="2528a-192">Kimenetének mindkét parancsok látható **csatlakoztatva** a a **PeeringState** oszlop.</span><span class="sxs-lookup"><span data-stu-id="2528a-192">The output of both commands shows **Connected** in the **PeeringState** column.</span></span>

     <span data-ttu-id="2528a-193">Bármely Azure-hoz létre vagy virtuális hálózati erőforrások is az IP-címek keresztül kommunikálnak egymással.</span><span class="sxs-lookup"><span data-stu-id="2528a-193">Any Azure resources you create in either virtual network are now able to communicate with each other through their IP addresses.</span></span> <span data-ttu-id="2528a-194">Alapértelmezett Azure névfeloldást használ a virtuális hálózatok, a virtuális hálózatok erőforrások esetén nem tudják feloldani a virtuális hálózatok közötti.</span><span class="sxs-lookup"><span data-stu-id="2528a-194">If you're using default Azure name resolution for the virtual networks, the resources in the virtual networks are not able to resolve names across the virtual networks.</span></span> <span data-ttu-id="2528a-195">Ha szeretné feloldani egy társviszony-létesítés virtuális hálózatok között, létre kell hoznia a saját DNS-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="2528a-195">If you want to resolve names across virtual networks in a peering, you must create your own DNS server.</span></span> <span data-ttu-id="2528a-196">Ismerje meg, hogyan állíthat be [névfeloldáshoz a saját DNS-kiszolgáló](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).</span><span class="sxs-lookup"><span data-stu-id="2528a-196">Learn how to set up [Name resolution using your own DNS server](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).</span></span>

4. <span data-ttu-id="2528a-197">**Nem kötelező**: abban az esetben, ha ez az oktatóanyag nem vonatkozik a virtuális gépek létrehozását, hozzon létre egy virtuális gép minden egyes virtuális hálózati, és csatlakoztassa egy virtuális gépről a másikra, való kapcsolat ellenőrzésére.</span><span class="sxs-lookup"><span data-stu-id="2528a-197">**Optional**: Though creating virtual machines is not covered in this tutorial, you can create a virtual machine in each virtual network and connect from one virtual machine to the other, to validate connectivity.</span></span>
5. <span data-ttu-id="2528a-198">**Nem kötelező**: az erőforrásokat, amelyek ebben az oktatóanyagban létrehozhat törléséhez végrehajtásához a [törli az erőforrást](#delete-cli) ebben a cikkben.</span><span class="sxs-lookup"><span data-stu-id="2528a-198">**Optional**: To delete the resources that you create in this tutorial, complete the steps in [Delete resources](#delete-cli) in this article.</span></span>


## <span data-ttu-id="2528a-199"><a name="powershell"></a>Hozzon létre a társviszony - PowerShell</span><span class="sxs-lookup"><span data-stu-id="2528a-199"><a name="powershell"></a>Create peering - PowerShell</span></span>

1. <span data-ttu-id="2528a-200">Telepítse a PowerShell [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) moduljának legújabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="2528a-200">Install the latest version of the PowerShell [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) module.</span></span> <span data-ttu-id="2528a-201">Ha először használja a PowerShellt, olvassa el az [Azure PowerShell áttekintését](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="2528a-201">If you're new to Azure PowerShell, see [Azure PowerShell overview](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json).</span></span>
2. <span data-ttu-id="2528a-202">Egy PowerShell-munkamenet megkezdéséhez lépjen a **Start** menüre, írja be a **powershell** kifejezést, majd kattintson a **PowerShell** elemre.</span><span class="sxs-lookup"><span data-stu-id="2528a-202">To start a PowerShell session, go to **Start**, enter **powershell**, and then click **PowerShell**.</span></span>
3. <span data-ttu-id="2528a-203">A PowerShellben jelentkezzen be az Azure-ba a `login-azurermaccount` paranccsal.</span><span class="sxs-lookup"><span data-stu-id="2528a-203">In PowerShell, log in to Azure by entering the `login-azurermaccount` command.</span></span> <span data-ttu-id="2528a-204">A fiókkal jelentkezik be az virtuális hálózati társviszony-létesítés létrehozásához szükséges engedélyekkel kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="2528a-204">The account you log in with must have the necessary permissions to create a virtual network peering.</span></span> <span data-ttu-id="2528a-205">Tekintse meg a [engedélyek](#permissions) jelen cikkben alább szakasza.</span><span class="sxs-lookup"><span data-stu-id="2528a-205">See the [Permissions](#permissions) section of this article for details.</span></span>
4. <span data-ttu-id="2528a-206">Hozzon létre egy erőforráscsoportot és két virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="2528a-206">Create a resource group and two virtual networks.</span></span> <span data-ttu-id="2528a-207">A parancsfájl végrehajtása, másolja a következő, illessze be a PowerShell, és nyomja le az `Enter` után utolsó sora a képernyőn jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="2528a-207">To execute the script, copy the following script, paste it into PowerShell, and then press `Enter` after the last line appears on the screen:</span></span>

    ```powershell
    # Variables for common values used throughout the script.
    $rgName='myResourceGroup'
    $location='eastus'

    # Create a resource group.
    New-AzureRmResourceGroup `
      -Name $rgName `
      -Location $location

    # Create virtual network 1.
    $vnet1 = New-AzureRmVirtualNetwork `
      -ResourceGroupName $rgName `
      -Name 'myVnet1' `
      -AddressPrefix '10.0.0.0/16' `
      -Location $location

    # Create virtual network 2.
    $vnet2 = New-AzureRmVirtualNetwork `
      -ResourceGroupName $rgName `
      -Name 'myVnet2' `
      -AddressPrefix '10.1.0.0/16' `
      -Location $location
    ```

5. <span data-ttu-id="2528a-208">Hozzon létre egy virtuális hálózati társviszony-létesítés a két virtuális hálózatok között.</span><span class="sxs-lookup"><span data-stu-id="2528a-208">Create a virtual network peering between the two virtual networks.</span></span> <span data-ttu-id="2528a-209">Másolja a következő, illessze be a PowerShell és nyomja le az `Enter` után utolsó sora a képernyőn jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="2528a-209">Copy the following script, paste in to PowerShell, and then press `Enter` after the last line appears on the screen:</span></span>
    ```powershell
    # Peer VNet1 to VNet2.
    Add-AzureRmVirtualNetworkPeering `
      -Name 'myVnet1ToMyVnet2' `
      -VirtualNetwork $vnet1 `
      -RemoteVirtualNetworkId $vnet2.Id

    # Peer VNet2 to VNet1.
    Add-AzureRmVirtualNetworkPeering `
      -Name 'myVnet2ToMyVnet1' `
      -VirtualNetwork $vnet2 `
      -RemoteVirtualNetworkId $vnet1.Id
    ```
6. <span data-ttu-id="2528a-210">Tekintse át a virtuális hálózati alhálózat, másolja be a következő parancsot, illessze be a PowerShell, és nyomja le az `Enter`:</span><span class="sxs-lookup"><span data-stu-id="2528a-210">To review the subnets for the virtual network, copy the following command, paste in to PowerShell, and then press `Enter`:</span></span>

    ```powershell
    Get-AzureRmVirtualNetworkPeering `
      -ResourceGroupName myResourceGroup `
      -VirtualNetworkName myVnet1 `
      | Format-Table VirtualNetworkName, PeeringState
    ```

    <span data-ttu-id="2528a-211">Futtassa az előző parancsot újból, cseréje *myVnet1* rendelkező *myVnet2*.</span><span class="sxs-lookup"><span data-stu-id="2528a-211">Run the previous command again, replacing *myVnet1* with *myVnet2*.</span></span> <span data-ttu-id="2528a-212">Kimenetének mindkét parancsok látható **csatlakoztatva** a a **PeeringState** oszlop.</span><span class="sxs-lookup"><span data-stu-id="2528a-212">The output of both commands shows **Connected** in the **PeeringState** column.</span></span>
7. <span data-ttu-id="2528a-213">**Nem kötelező**: abban az esetben, ha ez az oktatóanyag nem vonatkozik a virtuális gépek létrehozását, hozzon létre egy virtuális gép minden egyes virtuális hálózati, és csatlakoztassa egy virtuális gépről a másikra, való kapcsolat ellenőrzésére.</span><span class="sxs-lookup"><span data-stu-id="2528a-213">**Optional**: Though creating virtual machines is not covered in this tutorial, you can create a virtual machine in each virtual network and connect from one virtual machine to the other, to validate connectivity.</span></span>
8. <span data-ttu-id="2528a-214">**Nem kötelező**: az erőforrásokat, amelyek ebben az oktatóanyagban létrehozhat törléséhez végrehajtásához a [törli az erőforrást](#delete-powershell) ebben a cikkben.</span><span class="sxs-lookup"><span data-stu-id="2528a-214">**Optional**: To delete the resources that you create in this tutorial, complete the steps in [Delete resources](#delete-powershell) in this article.</span></span>

<span data-ttu-id="2528a-215">Bármely Azure-hoz létre vagy virtuális hálózati erőforrások is az IP-címek keresztül kommunikálnak egymással.</span><span class="sxs-lookup"><span data-stu-id="2528a-215">Any Azure resources you create in either virtual network are now able to communicate with each other through their IP addresses.</span></span> <span data-ttu-id="2528a-216">Alapértelmezett Azure névfeloldást használ a virtuális hálózatok, a virtuális hálózatok erőforrások esetén nem tudják feloldani a virtuális hálózatok közötti.</span><span class="sxs-lookup"><span data-stu-id="2528a-216">If you're using default Azure name resolution for the virtual networks, the resources in the virtual networks are not able to resolve names across the virtual networks.</span></span> <span data-ttu-id="2528a-217">Ha szeretné feloldani egy társviszony-létesítés virtuális hálózatok között, létre kell hoznia a saját DNS-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="2528a-217">If you want to resolve names across virtual networks in a peering, you must create your own DNS server.</span></span> <span data-ttu-id="2528a-218">Ismerje meg, hogyan állíthat be [névfeloldáshoz a saját DNS-kiszolgáló](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).</span><span class="sxs-lookup"><span data-stu-id="2528a-218">Learn how to set up [Name resolution using your own DNS server](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).</span></span>

## <span data-ttu-id="2528a-219"><a name="template"></a>Hozzon létre a társviszony - Resource Manager-sablon</span><span class="sxs-lookup"><span data-stu-id="2528a-219"><a name="template"></a>Create peering - Resource Manager template</span></span>

1. <span data-ttu-id="2528a-220">Hivatkozás [hozzon létre egy virtuális hálózati társviszony-létesítés](https://azure.microsoft.com/resources/templates/201-vnet-to-vnet-peering) Resource Manager-sablon.</span><span class="sxs-lookup"><span data-stu-id="2528a-220">Reference [Create a virtual network peering](https://azure.microsoft.com/resources/templates/201-vnet-to-vnet-peering) Resource Manager template.</span></span> <span data-ttu-id="2528a-221">A sablonhoz utasítások is járnak, amelyek alapján üzembe helyezheti a sablont az Azure Portal, a PowerShell vagy az Azure CLI használatával.</span><span class="sxs-lookup"><span data-stu-id="2528a-221">Instructions are provided with the template for deploying the template using the Azure portal, PowerShell, or the Azure CLI.</span></span> <span data-ttu-id="2528a-222">Amelyik eszközzel való bejelentkezéshez válassza ki a sablon egy virtuális hálózati társviszony-létesítés létrehozásához szükséges engedélyekkel rendelkező fiók használatával történő telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="2528a-222">Log in to whichever tool you choose to deploy the template with using an account that has the necessary permissions to create a virtual network peering.</span></span> <span data-ttu-id="2528a-223">Tekintse meg a [engedélyek](#permissions) jelen cikkben alább szakasza.</span><span class="sxs-lookup"><span data-stu-id="2528a-223">See the [Permissions](#permissions) section of this article for details.</span></span>
2. <span data-ttu-id="2528a-224">**Nem kötelező**: abban az esetben, ha ez az oktatóanyag nem vonatkozik a virtuális gépek létrehozását, hozzon létre egy virtuális gép minden egyes virtuális hálózati, és csatlakoztassa egy virtuális gépről a másikra, való kapcsolat ellenőrzésére.</span><span class="sxs-lookup"><span data-stu-id="2528a-224">**Optional**: Though creating virtual machines is not covered in this tutorial, you can create a virtual machine in each virtual network and connect from one virtual machine to the other, to validate connectivity.</span></span>
3. <span data-ttu-id="2528a-225">**Nem kötelező**: az erőforrásokat, amelyek ebben az oktatóanyagban létrehozhat törléséhez lépéseinek végrehajtásához a [törli az erőforrást](#delete) című szakaszát, az Azure-portálon, a PowerShell vagy az Azure parancssori felület használatával.</span><span class="sxs-lookup"><span data-stu-id="2528a-225">**Optional**: To delete the resources that you create in this tutorial, complete the steps in the [Delete resources](#delete) section of this article, using either the Azure portal, PowerShell, or the Azure CLI.</span></span>

## <span data-ttu-id="2528a-226"><a name="permissions"></a>Engedélyek</span><span class="sxs-lookup"><span data-stu-id="2528a-226"><a name="permissions"></a>Permissions</span></span>

<span data-ttu-id="2528a-227">A fiókokat hozhat létre a virtuális hálózati társviszony-létesítés a szükséges szerepkör vagy az engedélyekkel kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="2528a-227">The accounts you use to create a virtual network peering must have the necessary role or permissions.</span></span> <span data-ttu-id="2528a-228">Például, ha két virtuális hálózatok VNet1 és VNet2 nevű volt társviszony, a fiókjához társítva kell lenni a következő minimális szerepkör vagy az engedélyekkel minden virtuális hálózathoz:</span><span class="sxs-lookup"><span data-stu-id="2528a-228">For example, if you were peering two virtual networks named VNet1 and VNet2, your account must be assigned the following minimum role or permissions for each virtual network:</span></span>
    
|<span data-ttu-id="2528a-229">Virtuális hálózat</span><span class="sxs-lookup"><span data-stu-id="2528a-229">Virtual network</span></span>|<span data-ttu-id="2528a-230">Szerepkör</span><span class="sxs-lookup"><span data-stu-id="2528a-230">Role</span></span>|<span data-ttu-id="2528a-231">Engedélyek</span><span class="sxs-lookup"><span data-stu-id="2528a-231">Permissions</span></span>|
|---|---|---|
|<span data-ttu-id="2528a-232">VNet1</span><span class="sxs-lookup"><span data-stu-id="2528a-232">VNet1</span></span>|[<span data-ttu-id="2528a-233">Hálózati közreműködő</span><span class="sxs-lookup"><span data-stu-id="2528a-233">Network Contributor</span></span>](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|<span data-ttu-id="2528a-234">Microsoft.Network/virtualNetworks/virtualNetworkPeerings/write</span><span class="sxs-lookup"><span data-stu-id="2528a-234">Microsoft.Network/virtualNetworks/virtualNetworkPeerings/write</span></span>|
|<span data-ttu-id="2528a-235">VNet2</span><span class="sxs-lookup"><span data-stu-id="2528a-235">VNet2</span></span>|[<span data-ttu-id="2528a-236">Hálózati közreműködő</span><span class="sxs-lookup"><span data-stu-id="2528a-236">Network Contributor</span></span>](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|<span data-ttu-id="2528a-237">Microsoft.Network/virtualNetworks/peer</span><span class="sxs-lookup"><span data-stu-id="2528a-237">Microsoft.Network/virtualNetworks/peer</span></span>|

<span data-ttu-id="2528a-238">További információ [beépített szerepkörök](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) és rendelje hozzá adott engedélyeket [egyéni szerepkörök](../active-directory/role-based-access-control-custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (csak Resource Manager).</span><span class="sxs-lookup"><span data-stu-id="2528a-238">Learn more about [built-in roles](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) and assigning specific permissions to [custom roles](../active-directory/role-based-access-control-custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (Resource Manager only).</span></span>

## <span data-ttu-id="2528a-239"><a name="delete"></a>Erőforrások törlése</span><span class="sxs-lookup"><span data-stu-id="2528a-239"><a name="delete"></a>Delete resources</span></span>
<span data-ttu-id="2528a-240">Ez az oktatóanyag befejezése után, előfordulhat, hogy törölni kívánja az erőforrások létrehozta az oktatóanyagban, használati költségek.</span><span class="sxs-lookup"><span data-stu-id="2528a-240">When you've finished this tutorial, you might want to delete the resources you created in the tutorial, so you don't incur usage charges.</span></span> <span data-ttu-id="2528a-241">Erőforráscsoport törlésével, az erőforráscsoport összes erőforrást is törli.</span><span class="sxs-lookup"><span data-stu-id="2528a-241">Deleting a resource group also deletes all resources that are in the resource group.</span></span>

### <span data-ttu-id="2528a-242"><a name="delete-portal"></a>Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="2528a-242"><a name="delete-portal"></a>Azure portal</span></span>

1. <span data-ttu-id="2528a-243">A portál keresési mezőbe, írja be a **myResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="2528a-243">In the portal search box, enter **myResourceGroup**.</span></span> <span data-ttu-id="2528a-244">A keresési eredmények között kattintson **myResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="2528a-244">In the search results, click **myResourceGroup**.</span></span>
2. <span data-ttu-id="2528a-245">Az a **myResourceGroup** panelen kattintson a **törlése** ikonra.</span><span class="sxs-lookup"><span data-stu-id="2528a-245">On the **myResourceGroup** blade, click the **Delete** icon.</span></span>
3. <span data-ttu-id="2528a-246">A törlés megerősítéséhez a a **típus az ERŐFORRÁSCSOPORT neve** adja meg a **myResourceGroup**, és kattintson a **törlése**.</span><span class="sxs-lookup"><span data-stu-id="2528a-246">To confirm the deletion, in the **TYPE THE RESOURCE GROUP NAME** box, enter **myResourceGroup**, and then click **Delete**.</span></span>

### <span data-ttu-id="2528a-247"><a name="delete-cli"></a>Az Azure parancssori felület</span><span class="sxs-lookup"><span data-stu-id="2528a-247"><a name="delete-cli"></a>Azure CLI</span></span>

<span data-ttu-id="2528a-248">Írja be a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="2528a-248">Enter the following command:</span></span>

```azurecli-interactive
az group delete --name myResourceGroup --yes
```

### <span data-ttu-id="2528a-249"><a name="delete-powershell"></a>PowerShell</span><span class="sxs-lookup"><span data-stu-id="2528a-249"><a name="delete-powershell"></a>PowerShell</span></span>

<span data-ttu-id="2528a-250">Írja be a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="2528a-250">Enter the following command:</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -force
```

## <a name="next-steps"></a><span data-ttu-id="2528a-251">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2528a-251">Next steps</span></span>

- <span data-ttu-id="2528a-252">Alaposan olvassa el a fontos [virtuális hálózati társviszony-létesítési korlátozások és viselkedéshez](virtual-network-manage-peering.md#requirements-and-constraints) társviszony-létesítés üzemi virtuális hálózat létrehozása előtt használja.</span><span class="sxs-lookup"><span data-stu-id="2528a-252">Thoroughly familiarize yourself with important [virtual network peering constraints and behaviors](virtual-network-manage-peering.md#requirements-and-constraints) before creating a virtual network peering for production use.</span></span>
- <span data-ttu-id="2528a-253">További tudnivalók az összes [virtuális hálózati társviszony-létesítési beállítások](virtual-network-manage-peering.md#create-a-peering).</span><span class="sxs-lookup"><span data-stu-id="2528a-253">Learn about all [virtual network peering settings](virtual-network-manage-peering.md#create-a-peering).</span></span>
- <span data-ttu-id="2528a-254">Megtudhatja, hogyan [létrehoz egy központot, és a hálózati topológia irány](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) a virtuális hálózati társviszony-létesítés.</span><span class="sxs-lookup"><span data-stu-id="2528a-254">Learn how to [create a hub and spoke network topology](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) with virtual network peering.</span></span>
