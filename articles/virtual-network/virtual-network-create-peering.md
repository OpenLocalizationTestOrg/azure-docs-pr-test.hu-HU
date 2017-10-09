---
title: "egy Azure virtuális hálózati társviszony - aaaCreate Resource Manager - ugyanahhoz az előfizetéshez |} Microsoft Docs"
description: "Ismerje meg, hogyan hozták létre toocreate virtuális hálózati társviszony-létesítés virtuális hálózatok közötti erőforrás-kezelővel, amely szerepel hello azonos Azure-előfizetés."
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
ms.openlocfilehash: c2d24fdc8103c09c3bfb8e59be12e301d9e9a55a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-network-peering---resource-manager-same-subscription"></a><span data-ttu-id="9708d-103">Hozzon létre egy virtuális hálózati társviszony - erőforrás-kezelő ugyanahhoz az előfizetéshez</span><span class="sxs-lookup"><span data-stu-id="9708d-103">Create a virtual network peering - Resource Manager, same subscription</span></span>

<span data-ttu-id="9708d-104">Ebben az oktatóanyagban elsajátíthatja toocreate egy virtuális hálózati társviszony-létesítés Resource Manager használatával létrehozott virtuális hálózatok között.</span><span class="sxs-lookup"><span data-stu-id="9708d-104">In this tutorial, you learn toocreate a virtual network peering between virtual networks created through Resource Manager.</span></span> <span data-ttu-id="9708d-105">Mindkét virtuális hálózat szerepel hello azonos előfizetéssel.</span><span class="sxs-lookup"><span data-stu-id="9708d-105">Both virtual networks exist in hello same subscription.</span></span> <span data-ttu-id="9708d-106">Társviszony-létesítési két virtuális hálózatok lehetővé teszi, hogy erőforrások egymással a különböző virtuális hálózatokon toocommunicate hello azonos sávszélesség és a késés, mintha hello erőforrások szerepeltek hello ugyanazt a virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="9708d-106">Peering two virtual networks enables resources in different virtual networks toocommunicate with each other with hello same bandwidth and latency as though hello resources were in hello same virtual network.</span></span> <span data-ttu-id="9708d-107">További információ [virtuális hálózati társviszony-létesítés](virtual-network-peering-overview.md).</span><span class="sxs-lookup"><span data-stu-id="9708d-107">Learn more about [Virtual network peering](virtual-network-peering-overview.md).</span></span> 

<span data-ttu-id="9708d-108">hello lépéseket toocreate virtuális hálózati társviszony-létesítés eltérőek, attól függően, hogy hello virtuális hálózatok vannak-e hello azonos vagy eltérő, előfizetések, és amely [Azure telepítési modell](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) hello virtuális hálózatok jönnek létre. keresztül.</span><span class="sxs-lookup"><span data-stu-id="9708d-108">hello steps toocreate a virtual network peering are different, depending on whether hello virtual networks are in hello same, or different, subscriptions, and which [Azure deployment model](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) hello virtual networks are created through.</span></span> <span data-ttu-id="9708d-109">Megtudhatja, hogyan toocreate a virtuális hálózati társviszony más esetekben hello forgatókönyv a következő táblázat hello kattintva:</span><span class="sxs-lookup"><span data-stu-id="9708d-109">Learn how toocreate a virtual network peering in other scenarios by clicking hello scenario from hello following table:</span></span>

|<span data-ttu-id="9708d-110">Azure üzembehelyezési modell</span><span class="sxs-lookup"><span data-stu-id="9708d-110">Azure deployment model</span></span>  | <span data-ttu-id="9708d-111">Azure-előfizetés</span><span class="sxs-lookup"><span data-stu-id="9708d-111">Azure subscription</span></span>  |
|--------- |---------|
|[<span data-ttu-id="9708d-112">Mindkét erőforrás-kezelő</span><span class="sxs-lookup"><span data-stu-id="9708d-112">Both Resource Manager</span></span>](create-peering-different-subscriptions.md) |<span data-ttu-id="9708d-113">Különböző</span><span class="sxs-lookup"><span data-stu-id="9708d-113">Different</span></span>|
|[<span data-ttu-id="9708d-114">Egy erőforrás-kezelő egy klasszikus</span><span class="sxs-lookup"><span data-stu-id="9708d-114">One Resource Manager, one classic</span></span>](create-peering-different-deployment-models.md) |<span data-ttu-id="9708d-115">Azonos</span><span class="sxs-lookup"><span data-stu-id="9708d-115">Same</span></span>|
|[<span data-ttu-id="9708d-116">Egy erőforrás-kezelő egy klasszikus</span><span class="sxs-lookup"><span data-stu-id="9708d-116">One Resource Manager, one classic</span></span>](create-peering-different-deployment-models-subscriptions.md) |<span data-ttu-id="9708d-117">Különböző</span><span class="sxs-lookup"><span data-stu-id="9708d-117">Different</span></span>|

<span data-ttu-id="9708d-118">Virtuális hálózati társviszony-létesítés nem hozható létre, hello klasszikus üzembe helyezési modellben telepített virtuális hálózatok között.</span><span class="sxs-lookup"><span data-stu-id="9708d-118">A virtual network peering cannot be created between two virtual networks deployed through hello classic deployment model.</span></span> <span data-ttu-id="9708d-119">Virtuális hálózati társviszony-létesítés csak hozhatók létre, amely szerepel hello azonos virtuális hálózatok között Azure-régiót.</span><span class="sxs-lookup"><span data-stu-id="9708d-119">A virtual network peering can only be created between two virtual networks that exist in hello same Azure region.</span></span> <span data-ttu-id="9708d-120">Ha tooconnect virtuális hálózatokat egyaránt létrehozott hello klasszikus telepítési modell használatával, vagy, amely létezik a különböző Azure-régiók van szüksége, használhatja az Azure [VPN-átjáró](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) tooconnect hello virtuális hálózatok.</span><span class="sxs-lookup"><span data-stu-id="9708d-120">If you need tooconnect virtual networks that were both created through hello classic deployment model, or that exist in different Azure regions, you can use an Azure [VPN Gateway](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) tooconnect hello virtual networks.</span></span> 

<span data-ttu-id="9708d-121">Használhatja a hello [Azure-portálon](#portal), hello Azure [parancssori felület](#cli) (CLI), Azure [PowerShell](#powershell), vagy egy [Azure Resource Manager sablon](#template) toocreate virtuális hálózati társviszony-létesítés.</span><span class="sxs-lookup"><span data-stu-id="9708d-121">You can use hello [Azure portal](#portal), hello Azure [command-line interface](#cli) (CLI), Azure [PowerShell](#powershell), or an [Azure Resource Manager template](#template) toocreate a virtual network peering.</span></span> <span data-ttu-id="9708d-122">Kattintson bármelyik hello előző eszköz hivatkozások toogo, közvetlen toohello lépések létrehozása a virtuális hálózati társviszony-létesítés a kiválasztott eszköz segítségével.</span><span class="sxs-lookup"><span data-stu-id="9708d-122">Click any of hello previous tool links toogo directly toohello steps for creating a virtual network peering using your tool of choice.</span></span>

## <span data-ttu-id="9708d-123"><a name="portal"></a>Hozzon létre a társviszony - Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="9708d-123"><a name="portal"></a>Create peering - Azure portal</span></span>

1. <span data-ttu-id="9708d-124">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="9708d-124">Log in toohello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="9708d-125">hello fiókkal jelentkezik be az hello szükséges engedélyek toocreate rendelkeznie kell egy virtuális hálózati társviszony-létesítés.</span><span class="sxs-lookup"><span data-stu-id="9708d-125">hello account you log in with must have hello necessary permissions toocreate a virtual network peering.</span></span> <span data-ttu-id="9708d-126">Lásd: hello [engedélyek](#permissions) jelen cikkben alább szakasza.</span><span class="sxs-lookup"><span data-stu-id="9708d-126">See hello [Permissions](#permissions) section of this article for details.</span></span>
2. <span data-ttu-id="9708d-127">Kattintson a **+ új**, kattintson a **hálózati**, majd kattintson a **virtuális hálózati**.</span><span class="sxs-lookup"><span data-stu-id="9708d-127">Click **+ New**, click **Networking**, then click **Virtual network**.</span></span>
3. <span data-ttu-id="9708d-128">A hello **virtuális hálózat létrehozása** panelen adja meg, vagy válassza ki a következő beállítások hello értékeit, majd kattintson **létrehozása**:</span><span class="sxs-lookup"><span data-stu-id="9708d-128">In hello **Create virtual network** blade, enter, or select values for hello following settings, then click **Create**:</span></span>
    - <span data-ttu-id="9708d-129">**Név**: *myVnet1*</span><span class="sxs-lookup"><span data-stu-id="9708d-129">**Name**: *myVnet1*</span></span>
    - <span data-ttu-id="9708d-130">**Címtér**: *10.0.0.0/16*</span><span class="sxs-lookup"><span data-stu-id="9708d-130">**Address space**: *10.0.0.0/16*</span></span>
    - <span data-ttu-id="9708d-131">**Alhálózati név**: *alapértelmezett*</span><span class="sxs-lookup"><span data-stu-id="9708d-131">**Subnet name**: *default*</span></span>
    - <span data-ttu-id="9708d-132">**Alhálózati címtartományt**: *10.0.0.0/24*</span><span class="sxs-lookup"><span data-stu-id="9708d-132">**Subnet address range**: *10.0.0.0/24*</span></span>
    - <span data-ttu-id="9708d-133">**Előfizetés**: Jelölje ki az előfizetését</span><span class="sxs-lookup"><span data-stu-id="9708d-133">**Subscription**: Select your subscription</span></span>
    - <span data-ttu-id="9708d-134">**Erőforráscsoport**: válasszon **hozzon létre új** , és írja be *myResourceGroup*</span><span class="sxs-lookup"><span data-stu-id="9708d-134">**Resource group**: Select **Create new** and enter *myResourceGroup*</span></span>
    - <span data-ttu-id="9708d-135">**Hely**: *USA keleti régiója*</span><span class="sxs-lookup"><span data-stu-id="9708d-135">**Location**: *East US*</span></span>
4. <span data-ttu-id="9708d-136">Hajtsa végre a lépéseket 2-3 újra megadása az értékek a 3. lépésben a következő hello:</span><span class="sxs-lookup"><span data-stu-id="9708d-136">Complete steps 2-3 again specifying hello following values in step 3:</span></span>
    - <span data-ttu-id="9708d-137">**Név**: *myVnet2*</span><span class="sxs-lookup"><span data-stu-id="9708d-137">**Name**: *myVnet2*</span></span>
    - <span data-ttu-id="9708d-138">**Címtér**: *10.1.0.0/16*</span><span class="sxs-lookup"><span data-stu-id="9708d-138">**Address space**: *10.1.0.0/16*</span></span>
    - <span data-ttu-id="9708d-139">**Alhálózati név**: *alapértelmezett*</span><span class="sxs-lookup"><span data-stu-id="9708d-139">**Subnet name**: *default*</span></span>
    - <span data-ttu-id="9708d-140">**Alhálózati címtartományt**: *10.1.0.0/24*</span><span class="sxs-lookup"><span data-stu-id="9708d-140">**Subnet address range**: *10.1.0.0/24*</span></span>
    - <span data-ttu-id="9708d-141">**Előfizetés**: Jelölje ki az előfizetését</span><span class="sxs-lookup"><span data-stu-id="9708d-141">**Subscription**: Select your subscription</span></span>
    - <span data-ttu-id="9708d-142">**Erőforráscsoport**: válasszon **meglévő** válassza *myResourceGroup*</span><span class="sxs-lookup"><span data-stu-id="9708d-142">**Resource group**: Select **Use existing** and select *myResourceGroup*</span></span>
    - <span data-ttu-id="9708d-143">**Hely**: *USA keleti régiója*</span><span class="sxs-lookup"><span data-stu-id="9708d-143">**Location**: *East US*</span></span>
5. <span data-ttu-id="9708d-144">A hello **keresési erőforrások** mezőt hello portálon típus hello tetején *myResourceGroup*.</span><span class="sxs-lookup"><span data-stu-id="9708d-144">In hello **Search resources** box at hello top of hello portal, type *myResourceGroup*.</span></span> <span data-ttu-id="9708d-145">Kattintson a **myResourceGroup** amikor megjelenik a hello keresési eredmények között.</span><span class="sxs-lookup"><span data-stu-id="9708d-145">Click **myResourceGroup** when it appears in hello search results.</span></span> <span data-ttu-id="9708d-146">A panel jelenik meg hello **myresourcegroup** erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="9708d-146">A blade appears for hello **myresourcegroup** resource group.</span></span> <span data-ttu-id="9708d-147">hello erőforráscsoport hello két létrehozott virtuális hálózatokat az előző lépéseket tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="9708d-147">hello resource group contains hello two virtual networks created in previous steps.</span></span>
6. <span data-ttu-id="9708d-148">Kattintson a **myVNet1**.</span><span class="sxs-lookup"><span data-stu-id="9708d-148">Click **myVNet1**.</span></span>
7. <span data-ttu-id="9708d-149">A hello **myVnet1** panel, amelyen megjelenik, kattintson a **Társviszony** hello függőleges bal oldalán található hello panel hello beállítások listája.</span><span class="sxs-lookup"><span data-stu-id="9708d-149">In hello **myVnet1** blade that appears, click **Peerings** from hello vertical list of options on hello left side of hello blade.</span></span>
8. <span data-ttu-id="9708d-150">A hello **myVnet1 - esetében** panel, amelyen jelent meg, kattintson a **+ Hozzáadás**</span><span class="sxs-lookup"><span data-stu-id="9708d-150">In hello **myVnet1 - Peerings** blade that appeared, click **+ Add**</span></span>
9. <span data-ttu-id="9708d-151">A hello **Hozzáadás társviszony-létesítés** panelen megjelenő, adja meg, vagy válassza ki az alábbi beállítások hello, majd kattintson **OK**:</span><span class="sxs-lookup"><span data-stu-id="9708d-151">In hello **Add peering** blade that appears, enter, or select hello following options, then click **OK**:</span></span>
     - <span data-ttu-id="9708d-152">**Név**: *myVnet1ToMyVnet2*</span><span class="sxs-lookup"><span data-stu-id="9708d-152">**Name**: *myVnet1ToMyVnet2*</span></span>
     - <span data-ttu-id="9708d-153">**Virtuális hálózat telepítési modell**: válasszon **erőforrás-kezelő**.</span><span class="sxs-lookup"><span data-stu-id="9708d-153">**Virtual network deployment model**:  Select **Resource Manager**.</span></span> 
     - <span data-ttu-id="9708d-154">**Előfizetés**: Jelölje ki az előfizetését</span><span class="sxs-lookup"><span data-stu-id="9708d-154">**Subscription**: Select your subscription</span></span>
     - <span data-ttu-id="9708d-155">**Virtuális hálózati**: kattintson a **virtuális hálózatot választ**, majd kattintson a **myVnet2**.</span><span class="sxs-lookup"><span data-stu-id="9708d-155">**Virtual network**:  Click **Choose a virtual network**, then click **myVnet2**.</span></span>
     - <span data-ttu-id="9708d-156">**Virtuális hálózati hozzáférés engedélyezése:** ügyeljen arra, hogy **engedélyezve** van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="9708d-156">**Allow virtual network access:** Ensure that **Enabled** is selected.</span></span>
    <span data-ttu-id="9708d-157">Ebben az oktatóanyagban nincs más beállítások használhatók.</span><span class="sxs-lookup"><span data-stu-id="9708d-157">No other settings are used in this tutorial.</span></span> <span data-ttu-id="9708d-158">toolearn összes társviszony-létesítési beállításról, olvasni [kezelheti a virtuális hálózati társviszony](virtual-network-manage-peering.md#create-a-peering).</span><span class="sxs-lookup"><span data-stu-id="9708d-158">toolearn about all peering settings, read [Manage virtual network peerings](virtual-network-manage-peering.md#create-a-peering).</span></span>
10. <span data-ttu-id="9708d-159">Kattintás után **OK** hello a korábbi lépésben hello **Hozzáadás társviszony-létesítés** panel bezárása után, és látni hello **myVnet1 - Társviszony** újra a panelt.</span><span class="sxs-lookup"><span data-stu-id="9708d-159">After clicking **OK** in hello previous step, hello **Add peering** blade closes and you see hello **myVnet1 - Peerings** blade again.</span></span> <span data-ttu-id="9708d-160">Néhány másodpercen belül létrehozott társviszony hello hello panel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="9708d-160">After a few seconds, hello peering you created appears in hello blade.</span></span> <span data-ttu-id="9708d-161">**Kezdeményezett** hello szerepel **társviszony-LÉTESÍTÉS állapot** hello oszlopában **myVnet1ToMyVnet2** társviszony-létesítés meg létrehozni.</span><span class="sxs-lookup"><span data-stu-id="9708d-161">**Initiated** is listed in hello **PEERING STATUS** column for hello **myVnet1ToMyVnet2** peering you created.</span></span> <span data-ttu-id="9708d-162">Már nincsenek társviszonyban Vnet1 tooVnet2, de most myVnet2 toomyVnet1 kell partnert.</span><span class="sxs-lookup"><span data-stu-id="9708d-162">You've peered Vnet1 tooVnet2, but now you must peer myVnet2 toomyVnet1.</span></span> <span data-ttu-id="9708d-163">hello társviszony-létesítés léteznie kell mindkét irányban hello virtuális hálózatok toocommunicate tooenable erőforrások egymással.</span><span class="sxs-lookup"><span data-stu-id="9708d-163">hello peering must be created in both directions tooenable resources in hello virtual networks toocommunicate with each other.</span></span>
11. <span data-ttu-id="9708d-164">Végezze el újra az myVnet2 5-10 lépéseket.</span><span class="sxs-lookup"><span data-stu-id="9708d-164">Complete steps 5-10 again for myVnet2.</span></span>  <span data-ttu-id="9708d-165">Név hello társviszony-létesítés *myVnet2ToMyVnet1*.</span><span class="sxs-lookup"><span data-stu-id="9708d-165">Name hello peering *myVnet2ToMyVnet1*.</span></span>
12. <span data-ttu-id="9708d-166">Néhány másodpercen belül kattintás után **OK** toocreate hello társviszony-létesítés MyVnet2, a hello **myVnet2ToMyVnet1** társviszony-létesítést az imént létrehozott szerepel **csatlakoztatva** a hello  **Társviszony-LÉTESÍTÉS állapot** oszlop.</span><span class="sxs-lookup"><span data-stu-id="9708d-166">A few seconds after clicking **OK** toocreate hello peering for MyVnet2, hello **myVnet2ToMyVnet1** peering you just created is listed with **Connected** in hello **PEERING STATUS** column.</span></span>
13. <span data-ttu-id="9708d-167">Újra 5-7 lépéseinek elvégzését MyVnet1.</span><span class="sxs-lookup"><span data-stu-id="9708d-167">Complete steps 5-7 again for MyVnet1.</span></span> <span data-ttu-id="9708d-168">Hello **társviszony-LÉTESÍTÉS állapot** a hello **myVnet1ToVNet2** társviszony-létesítés már is **csatlakoztatva**.</span><span class="sxs-lookup"><span data-stu-id="9708d-168">hello **PEERING STATUS** for hello **myVnet1ToVNet2** peering is now also **Connected**.</span></span> <span data-ttu-id="9708d-169">hello társviszony-létesítés sikeresen létrejött, miután látta, **csatlakoztatva** a hello **társviszony-LÉTESÍTÉS állapot** mindkét virtuális hálózat hello társviszony-létesítés oszlopában.</span><span class="sxs-lookup"><span data-stu-id="9708d-169">hello peering is successfully established after you see **Connected** in hello **PEERING STATUS** column for both virtual networks in hello peering.</span></span>
14. <span data-ttu-id="9708d-170">**Nem kötelező**: abban az esetben, ha ez az oktatóanyag nem vonatkozik a virtuális gépek létrehozását, hozzon létre egy virtuális gép minden egyes virtuális hálózati, és csatlakoztassa egy virtuális gép toohello más, toovalidate kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="9708d-170">**Optional**: Though creating virtual machines is not covered in this tutorial, you can create a virtual machine in each virtual network and connect from one virtual machine toohello other, toovalidate connectivity.</span></span>
15. <span data-ttu-id="9708d-171">**Nem kötelező**: Ebben az oktatóanyagban létrehozhat hello erőforrások toodelete, teljes hello szükséges lépések hello [törli az erőforrást](#delete-portal) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="9708d-171">**Optional**: toodelete hello resources that you create in this tutorial, complete hello steps in hello [Delete resources](#delete-portal) section of this article.</span></span>

<span data-ttu-id="9708d-172">Bármely Azure-hoz létre vagy virtuális hálózati erőforrások is képes toocommunicate egymás mellett az IP-címek használatával.</span><span class="sxs-lookup"><span data-stu-id="9708d-172">Any Azure resources you create in either virtual network are now able toocommunicate with each other through their IP addresses.</span></span> <span data-ttu-id="9708d-173">Alapértelmezett Azure névfeloldás hello virtuális hálózatok használata, hello erőforrások hello virtuális hálózatok a rendszer nem tudja tooresolve nevek hello virtuális hálózatok közötti.</span><span class="sxs-lookup"><span data-stu-id="9708d-173">If you're using default Azure name resolution for hello virtual networks, hello resources in hello virtual networks are not able tooresolve names across hello virtual networks.</span></span> <span data-ttu-id="9708d-174">Ha tooresolve nevek a társviszony-létesítés virtuális hálózatok között, létre kell hoznia a saját DNS-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="9708d-174">If you want tooresolve names across virtual networks in a peering, you must create your own DNS server.</span></span> <span data-ttu-id="9708d-175">Megtudhatja, hogyan mentése tooset [névfeloldáshoz a saját DNS-kiszolgáló](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).</span><span class="sxs-lookup"><span data-stu-id="9708d-175">Learn how tooset up [Name resolution using your own DNS server](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).</span></span>

## <span data-ttu-id="9708d-176"><a name="cli"></a>Társviszony - létrehozása az Azure parancssori felület</span><span class="sxs-lookup"><span data-stu-id="9708d-176"><a name="cli"></a>Create peering - Azure CLI</span></span>

<span data-ttu-id="9708d-177">a következő parancsfájl hello:</span><span class="sxs-lookup"><span data-stu-id="9708d-177">hello following script:</span></span>

- <span data-ttu-id="9708d-178">Szükséges hello Azure CLI 2.0.4 verzió vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="9708d-178">Requires hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="9708d-179">toofind hello verzió, futtassa a hello `az --version` parancsot.</span><span class="sxs-lookup"><span data-stu-id="9708d-179">toofind hello version, run hello `az --version` command.</span></span> <span data-ttu-id="9708d-180">Ha tooupgrade van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="9708d-180">If you need tooupgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json).</span></span>
- <span data-ttu-id="9708d-181">A rendszerhéjakba működik.</span><span class="sxs-lookup"><span data-stu-id="9708d-181">Works in a Bash shell.</span></span> <span data-ttu-id="9708d-182">Az Azure parancssori felület parancsfájlok futtatásához a Windows-ügyfelén beállítások, lásd: [a Windows hello Azure CLI-t futtató](../virtual-machines/windows/cli-options.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="9708d-182">For options on running Azure CLI scripts on Windows client, see [Running hello Azure CLI in Windows](../virtual-machines/windows/cli-options.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span></span> 

<span data-ttu-id="9708d-183">Hello CLI és függőségeinek telepítése, helyett hello Azure Cloud rendszerhéj is használhatja.</span><span class="sxs-lookup"><span data-stu-id="9708d-183">Instead of installing hello CLI and its dependencies, you can use hello Azure Cloud Shell.</span></span> <span data-ttu-id="9708d-184">hello Azure Cloud rendszerhéj a szabad rendszerhéjakba futtatható közvetlenül hello Azure-portálon belül.</span><span class="sxs-lookup"><span data-stu-id="9708d-184">hello Azure Cloud Shell is a free Bash shell that you can run directly within hello Azure portal.</span></span> <span data-ttu-id="9708d-185">Hello Azure CLI előtelepített és konfigurált toouse-fiókjához van.</span><span class="sxs-lookup"><span data-stu-id="9708d-185">It has hello Azure CLI preinstalled and configured toouse with your account.</span></span> <span data-ttu-id="9708d-186">Kattintson a hello **kipróbálás** hello parancsfájl, amely a következő, a felhő rendszerhéjat, amelyre bejelentkezik, amellyel bejelentkezhet tooyour gombra az Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="9708d-186">Click hello **Try it** button in hello script that follows, which invokes a Cloud Shell that logs you can log in tooyour Azure account with.</span></span> <span data-ttu-id="9708d-187">tooexecute hello parancsfájl, kattintson a hello **másolási** gombra, és hello tartalmának beillesztése a felhő rendszerhéjat.</span><span class="sxs-lookup"><span data-stu-id="9708d-187">tooexecute hello script, click hello **Copy** button and paste hello contents into your Cloud Shell.</span></span>

1. <span data-ttu-id="9708d-188">Hozzon létre egy erőforráscsoportot és két virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="9708d-188">Create a resource group and two virtual networks.</span></span>

    ```azurecli-interactive
    #!/bin/bash

    # Variables for common values used throughout hello script.
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

2. <span data-ttu-id="9708d-189">Hozzon létre egy virtuális hálózati társviszony-létesítés hello virtuális hálózatok között.</span><span class="sxs-lookup"><span data-stu-id="9708d-189">Create a virtual network peering between hello two virtual networks.</span></span>
 
    ```azurecli-interactive
    # Get hello id for VNet1.
    vnet1Id=$(az network vnet show \
      --resource-group $rgName \
      --name myVnet1 \
      --query id --out tsv)

    # Get hello id for VNet2.
    vnet2Id=$(az network vnet show \
      --resource-group $rgName \
      --name myVnet2 \
      --query id \
      --out tsv)

    # Peer VNet1 tooVNet2.
    az network vnet peering create \
      --name myVnet1ToMyVnet2 \
      --resource-group $rgName \
      --vnet-name myVnet1 \
      --remote-vnet-id $vnet2Id \
      --allow-vnet-access

    # Peer VNet2 tooVNet1.
    az network vnet peering create \
      --name myVnet2ToMyVnet1 \
      --resource-group $rgName \
      --vnet-name myVnet2 \
      --remote-vnet-id $vnet1Id \
      --allow-vnet-access
    ```

3. <span data-ttu-id="9708d-190">Miután hello parancsprogram végrehajtása során, tekintse át a hello esetében minden virtuális hálózathoz.</span><span class="sxs-lookup"><span data-stu-id="9708d-190">After hello script executes, review hello peerings for each virtual network.</span></span> 

    ```azurecli-interactive
    az network vnet peering list \
      --resource-group myResourceGroup \
      --vnet-name myVnet1 \
      --output table
    ```
    
    <span data-ttu-id="9708d-191">Újrafuttatása hello előző parancs, cseréje *myVnet1* rendelkező *myVnet2*.</span><span class="sxs-lookup"><span data-stu-id="9708d-191">Run hello previous command again, replacing *myVnet1* with *myVnet2*.</span></span> <span data-ttu-id="9708d-192">hello kimenetének mindkét parancsok látható **csatlakoztatva** a hello **PeeringState** oszlop.</span><span class="sxs-lookup"><span data-stu-id="9708d-192">hello output of both commands shows **Connected** in hello **PeeringState** column.</span></span>

     <span data-ttu-id="9708d-193">Bármely Azure-hoz létre vagy virtuális hálózati erőforrások is képes toocommunicate egymás mellett az IP-címek használatával.</span><span class="sxs-lookup"><span data-stu-id="9708d-193">Any Azure resources you create in either virtual network are now able toocommunicate with each other through their IP addresses.</span></span> <span data-ttu-id="9708d-194">Alapértelmezett Azure névfeloldás hello virtuális hálózatok használata, hello erőforrások hello virtuális hálózatok a rendszer nem tudja tooresolve nevek hello virtuális hálózatok közötti.</span><span class="sxs-lookup"><span data-stu-id="9708d-194">If you're using default Azure name resolution for hello virtual networks, hello resources in hello virtual networks are not able tooresolve names across hello virtual networks.</span></span> <span data-ttu-id="9708d-195">Ha tooresolve nevek a társviszony-létesítés virtuális hálózatok között, létre kell hoznia a saját DNS-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="9708d-195">If you want tooresolve names across virtual networks in a peering, you must create your own DNS server.</span></span> <span data-ttu-id="9708d-196">Megtudhatja, hogyan mentése tooset [névfeloldáshoz a saját DNS-kiszolgáló](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).</span><span class="sxs-lookup"><span data-stu-id="9708d-196">Learn how tooset up [Name resolution using your own DNS server](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).</span></span>

4. <span data-ttu-id="9708d-197">**Nem kötelező**: abban az esetben, ha ez az oktatóanyag nem vonatkozik a virtuális gépek létrehozását, hozzon létre egy virtuális gép minden egyes virtuális hálózati, és csatlakoztassa egy virtuális gép toohello más, toovalidate kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="9708d-197">**Optional**: Though creating virtual machines is not covered in this tutorial, you can create a virtual machine in each virtual network and connect from one virtual machine toohello other, toovalidate connectivity.</span></span>
5. <span data-ttu-id="9708d-198">**Nem kötelező**: Ebben az oktatóanyagban létrehozhat hello erőforrások toodelete, teljes hello szükséges lépések [törli az erőforrást](#delete-cli) ebben a cikkben.</span><span class="sxs-lookup"><span data-stu-id="9708d-198">**Optional**: toodelete hello resources that you create in this tutorial, complete hello steps in [Delete resources](#delete-cli) in this article.</span></span>


## <span data-ttu-id="9708d-199"><a name="powershell"></a>Hozzon létre a társviszony - PowerShell</span><span class="sxs-lookup"><span data-stu-id="9708d-199"><a name="powershell"></a>Create peering - PowerShell</span></span>

1. <span data-ttu-id="9708d-200">Hello hello PowerShell legújabb verziójának telepítéséhez [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) modul.</span><span class="sxs-lookup"><span data-stu-id="9708d-200">Install hello latest version of hello PowerShell [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) module.</span></span> <span data-ttu-id="9708d-201">Ha új tooAzure PowerShell, lásd: [Azure PowerShell áttekintése](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="9708d-201">If you're new tooAzure PowerShell, see [Azure PowerShell overview](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json).</span></span>
2. <span data-ttu-id="9708d-202">toostart a PowerShell-munkamenetet, nyissa meg túl**Start**, adja meg **powershell**, és kattintson a **PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="9708d-202">toostart a PowerShell session, go too**Start**, enter **powershell**, and then click **PowerShell**.</span></span>
3. <span data-ttu-id="9708d-203">A PowerShellben, jelentkezzen be tooAzure hello megadásával `login-azurermaccount` parancsot.</span><span class="sxs-lookup"><span data-stu-id="9708d-203">In PowerShell, log in tooAzure by entering hello `login-azurermaccount` command.</span></span> <span data-ttu-id="9708d-204">hello fiókkal jelentkezik be az hello szükséges engedélyek toocreate rendelkeznie kell egy virtuális hálózati társviszony-létesítés.</span><span class="sxs-lookup"><span data-stu-id="9708d-204">hello account you log in with must have hello necessary permissions toocreate a virtual network peering.</span></span> <span data-ttu-id="9708d-205">Lásd: hello [engedélyek](#permissions) jelen cikkben alább szakasza.</span><span class="sxs-lookup"><span data-stu-id="9708d-205">See hello [Permissions](#permissions) section of this article for details.</span></span>
4. <span data-ttu-id="9708d-206">Hozzon létre egy erőforráscsoportot és két virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="9708d-206">Create a resource group and two virtual networks.</span></span> <span data-ttu-id="9708d-207">tooexecute hello parancsfájl, másolása hello következő parancsfájlt, illessze be a PowerShell és nyomja le az `Enter` után utolsó sora hello hello képernyőn látható:</span><span class="sxs-lookup"><span data-stu-id="9708d-207">tooexecute hello script, copy hello following script, paste it into PowerShell, and then press `Enter` after hello last line appears on hello screen:</span></span>

    ```powershell
    # Variables for common values used throughout hello script.
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

5. <span data-ttu-id="9708d-208">Hozzon létre egy virtuális hálózati társviszony-létesítés hello virtuális hálózatok között.</span><span class="sxs-lookup"><span data-stu-id="9708d-208">Create a virtual network peering between hello two virtual networks.</span></span> <span data-ttu-id="9708d-209">Másolás hello következő parancsfájl-, illessze be a tooPowerShell, és nyomja le az `Enter` után utolsó sora hello hello képernyőn látható:</span><span class="sxs-lookup"><span data-stu-id="9708d-209">Copy hello following script, paste in tooPowerShell, and then press `Enter` after hello last line appears on hello screen:</span></span>
    ```powershell
    # Peer VNet1 tooVNet2.
    Add-AzureRmVirtualNetworkPeering `
      -Name 'myVnet1ToMyVnet2' `
      -VirtualNetwork $vnet1 `
      -RemoteVirtualNetworkId $vnet2.Id

    # Peer VNet2 tooVNet1.
    Add-AzureRmVirtualNetworkPeering `
      -Name 'myVnet2ToMyVnet1' `
      -VirtualNetwork $vnet2 `
      -RemoteVirtualNetworkId $vnet1.Id
    ```
6. <span data-ttu-id="9708d-210">hello alhálózatok tooreview hello virtuális hálózat, másolása hello következő parancsot, illessze be a tooPowerShell, és nyomja le az `Enter`:</span><span class="sxs-lookup"><span data-stu-id="9708d-210">tooreview hello subnets for hello virtual network, copy hello following command, paste in tooPowerShell, and then press `Enter`:</span></span>

    ```powershell
    Get-AzureRmVirtualNetworkPeering `
      -ResourceGroupName myResourceGroup `
      -VirtualNetworkName myVnet1 `
      | Format-Table VirtualNetworkName, PeeringState
    ```

    <span data-ttu-id="9708d-211">Újrafuttatása hello előző parancs, cseréje *myVnet1* rendelkező *myVnet2*.</span><span class="sxs-lookup"><span data-stu-id="9708d-211">Run hello previous command again, replacing *myVnet1* with *myVnet2*.</span></span> <span data-ttu-id="9708d-212">hello kimenetének mindkét parancsok látható **csatlakoztatva** a hello **PeeringState** oszlop.</span><span class="sxs-lookup"><span data-stu-id="9708d-212">hello output of both commands shows **Connected** in hello **PeeringState** column.</span></span>
7. <span data-ttu-id="9708d-213">**Nem kötelező**: abban az esetben, ha ez az oktatóanyag nem vonatkozik a virtuális gépek létrehozását, hozzon létre egy virtuális gép minden egyes virtuális hálózati, és csatlakoztassa egy virtuális gép toohello más, toovalidate kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="9708d-213">**Optional**: Though creating virtual machines is not covered in this tutorial, you can create a virtual machine in each virtual network and connect from one virtual machine toohello other, toovalidate connectivity.</span></span>
8. <span data-ttu-id="9708d-214">**Nem kötelező**: Ebben az oktatóanyagban létrehozhat hello erőforrások toodelete, teljes hello szükséges lépések [törli az erőforrást](#delete-powershell) ebben a cikkben.</span><span class="sxs-lookup"><span data-stu-id="9708d-214">**Optional**: toodelete hello resources that you create in this tutorial, complete hello steps in [Delete resources](#delete-powershell) in this article.</span></span>

<span data-ttu-id="9708d-215">Bármely Azure-hoz létre vagy virtuális hálózati erőforrások is képes toocommunicate egymás mellett az IP-címek használatával.</span><span class="sxs-lookup"><span data-stu-id="9708d-215">Any Azure resources you create in either virtual network are now able toocommunicate with each other through their IP addresses.</span></span> <span data-ttu-id="9708d-216">Alapértelmezett Azure névfeloldás hello virtuális hálózatok használata, hello erőforrások hello virtuális hálózatok a rendszer nem tudja tooresolve nevek hello virtuális hálózatok közötti.</span><span class="sxs-lookup"><span data-stu-id="9708d-216">If you're using default Azure name resolution for hello virtual networks, hello resources in hello virtual networks are not able tooresolve names across hello virtual networks.</span></span> <span data-ttu-id="9708d-217">Ha tooresolve nevek a társviszony-létesítés virtuális hálózatok között, létre kell hoznia a saját DNS-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="9708d-217">If you want tooresolve names across virtual networks in a peering, you must create your own DNS server.</span></span> <span data-ttu-id="9708d-218">Megtudhatja, hogyan mentése tooset [névfeloldáshoz a saját DNS-kiszolgáló](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).</span><span class="sxs-lookup"><span data-stu-id="9708d-218">Learn how tooset up [Name resolution using your own DNS server](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).</span></span>

## <span data-ttu-id="9708d-219"><a name="template"></a>Hozzon létre a társviszony - Resource Manager-sablon</span><span class="sxs-lookup"><span data-stu-id="9708d-219"><a name="template"></a>Create peering - Resource Manager template</span></span>

1. <span data-ttu-id="9708d-220">Hivatkozás [hozzon létre egy virtuális hálózati társviszony-létesítés](https://azure.microsoft.com/resources/templates/201-vnet-to-vnet-peering) Resource Manager-sablon.</span><span class="sxs-lookup"><span data-stu-id="9708d-220">Reference [Create a virtual network peering](https://azure.microsoft.com/resources/templates/201-vnet-to-vnet-peering) Resource Manager template.</span></span> <span data-ttu-id="9708d-221">Útmutatás a megadott hello sablonra telepítése hello sablon használatával hello Azure-portálon, a PowerShell vagy a hello Azure CLI el.</span><span class="sxs-lookup"><span data-stu-id="9708d-221">Instructions are provided with hello template for deploying hello template using hello Azure portal, PowerShell, or hello Azure CLI.</span></span> <span data-ttu-id="9708d-222">Napló toowhichever eszközben úgy dönt, hogy toodeploy hello sablont egy olyan fiókkal, amely rendelkezik a szükséges engedélyek toocreate hello, virtuális hálózati társviszony-létesítés.</span><span class="sxs-lookup"><span data-stu-id="9708d-222">Log in toowhichever tool you choose toodeploy hello template with using an account that has hello necessary permissions toocreate a virtual network peering.</span></span> <span data-ttu-id="9708d-223">Lásd: hello [engedélyek](#permissions) jelen cikkben alább szakasza.</span><span class="sxs-lookup"><span data-stu-id="9708d-223">See hello [Permissions](#permissions) section of this article for details.</span></span>
2. <span data-ttu-id="9708d-224">**Nem kötelező**: abban az esetben, ha ez az oktatóanyag nem vonatkozik a virtuális gépek létrehozását, hozzon létre egy virtuális gép minden egyes virtuális hálózati, és csatlakoztassa egy virtuális gép toohello más, toovalidate kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="9708d-224">**Optional**: Though creating virtual machines is not covered in this tutorial, you can create a virtual machine in each virtual network and connect from one virtual machine toohello other, toovalidate connectivity.</span></span>
3. <span data-ttu-id="9708d-225">**Nem kötelező**: Ebben az oktatóanyagban létrehozhat hello erőforrások toodelete, teljes hello szükséges lépések hello [törli az erőforrást](#delete) című szakaszát, használatával hello Azure-portálon, a PowerShell vagy a hello Azure parancssori felület.</span><span class="sxs-lookup"><span data-stu-id="9708d-225">**Optional**: toodelete hello resources that you create in this tutorial, complete hello steps in hello [Delete resources](#delete) section of this article, using either hello Azure portal, PowerShell, or hello Azure CLI.</span></span>

## <span data-ttu-id="9708d-226"><a name="permissions"></a>Engedélyek</span><span class="sxs-lookup"><span data-stu-id="9708d-226"><a name="permissions"></a>Permissions</span></span>

<span data-ttu-id="9708d-227">hello fiókjai virtuális hálózati társviszony-létesítés toocreate hello szükséges szerepkör- vagy hozzáférési engedélyekkel kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="9708d-227">hello accounts you use toocreate a virtual network peering must have hello necessary role or permissions.</span></span> <span data-ttu-id="9708d-228">Például, ha két virtuális hálózatok VNet1 és VNet2 nevű volt társviszony, a fiókjához társítva kell lenni a következő minimális szerepkör vagy minden egyes virtuális hálózati engedélyeinek hello:</span><span class="sxs-lookup"><span data-stu-id="9708d-228">For example, if you were peering two virtual networks named VNet1 and VNet2, your account must be assigned hello following minimum role or permissions for each virtual network:</span></span>
    
|<span data-ttu-id="9708d-229">Virtuális hálózat</span><span class="sxs-lookup"><span data-stu-id="9708d-229">Virtual network</span></span>|<span data-ttu-id="9708d-230">Szerepkör</span><span class="sxs-lookup"><span data-stu-id="9708d-230">Role</span></span>|<span data-ttu-id="9708d-231">Engedélyek</span><span class="sxs-lookup"><span data-stu-id="9708d-231">Permissions</span></span>|
|---|---|---|
|<span data-ttu-id="9708d-232">VNet1</span><span class="sxs-lookup"><span data-stu-id="9708d-232">VNet1</span></span>|[<span data-ttu-id="9708d-233">Hálózati közreműködő</span><span class="sxs-lookup"><span data-stu-id="9708d-233">Network Contributor</span></span>](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|<span data-ttu-id="9708d-234">Microsoft.Network/virtualNetworks/virtualNetworkPeerings/write</span><span class="sxs-lookup"><span data-stu-id="9708d-234">Microsoft.Network/virtualNetworks/virtualNetworkPeerings/write</span></span>|
|<span data-ttu-id="9708d-235">VNet2</span><span class="sxs-lookup"><span data-stu-id="9708d-235">VNet2</span></span>|[<span data-ttu-id="9708d-236">Hálózati közreműködő</span><span class="sxs-lookup"><span data-stu-id="9708d-236">Network Contributor</span></span>](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|<span data-ttu-id="9708d-237">Microsoft.Network/virtualNetworks/peer</span><span class="sxs-lookup"><span data-stu-id="9708d-237">Microsoft.Network/virtualNetworks/peer</span></span>|

<span data-ttu-id="9708d-238">További információ [beépített szerepkörök](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) és konkrét engedélyeket túl[egyéni szerepkörök](../active-directory/role-based-access-control-custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (csak Resource Manager).</span><span class="sxs-lookup"><span data-stu-id="9708d-238">Learn more about [built-in roles](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) and assigning specific permissions too[custom roles](../active-directory/role-based-access-control-custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (Resource Manager only).</span></span>

## <span data-ttu-id="9708d-239"><a name="delete"></a>Erőforrások törlése</span><span class="sxs-lookup"><span data-stu-id="9708d-239"><a name="delete"></a>Delete resources</span></span>
<span data-ttu-id="9708d-240">Ez az oktatóanyag befejezése után, érdemes lehet toodelete hello erőforrások hello oktatóanyag, így a használati költségek létrehozott.</span><span class="sxs-lookup"><span data-stu-id="9708d-240">When you've finished this tutorial, you might want toodelete hello resources you created in hello tutorial, so you don't incur usage charges.</span></span> <span data-ttu-id="9708d-241">Erőforráscsoport törlésekor a hello erőforráscsoportban található összes erőforrást is törlődnek.</span><span class="sxs-lookup"><span data-stu-id="9708d-241">Deleting a resource group also deletes all resources that are in hello resource group.</span></span>

### <span data-ttu-id="9708d-242"><a name="delete-portal"></a>Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="9708d-242"><a name="delete-portal"></a>Azure portal</span></span>

1. <span data-ttu-id="9708d-243">Hello portál keresési mezőbe, írja be a **myResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="9708d-243">In hello portal search box, enter **myResourceGroup**.</span></span> <span data-ttu-id="9708d-244">Hello keresési eredmények között kattintson **myResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="9708d-244">In hello search results, click **myResourceGroup**.</span></span>
2. <span data-ttu-id="9708d-245">A hello **myResourceGroup** panelen kattintson hello **törlése** ikonra.</span><span class="sxs-lookup"><span data-stu-id="9708d-245">On hello **myResourceGroup** blade, click hello **Delete** icon.</span></span>
3. <span data-ttu-id="9708d-246">tooconfirm hello törlésre, hello **típus hello ERŐFORRÁSCSOPORT-név** adja meg a **myResourceGroup**, és kattintson a **törlése**.</span><span class="sxs-lookup"><span data-stu-id="9708d-246">tooconfirm hello deletion, in hello **TYPE hello RESOURCE GROUP NAME** box, enter **myResourceGroup**, and then click **Delete**.</span></span>

### <span data-ttu-id="9708d-247"><a name="delete-cli"></a>Az Azure parancssori felület</span><span class="sxs-lookup"><span data-stu-id="9708d-247"><a name="delete-cli"></a>Azure CLI</span></span>

<span data-ttu-id="9708d-248">Adja meg a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="9708d-248">Enter hello following command:</span></span>

```azurecli-interactive
az group delete --name myResourceGroup --yes
```

### <span data-ttu-id="9708d-249"><a name="delete-powershell"></a>PowerShell</span><span class="sxs-lookup"><span data-stu-id="9708d-249"><a name="delete-powershell"></a>PowerShell</span></span>

<span data-ttu-id="9708d-250">Adja meg a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="9708d-250">Enter hello following command:</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -force
```

## <a name="next-steps"></a><span data-ttu-id="9708d-251">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9708d-251">Next steps</span></span>

- <span data-ttu-id="9708d-252">Alaposan olvassa el a fontos [virtuális hálózati társviszony-létesítési korlátozások és viselkedéshez](virtual-network-manage-peering.md#requirements-and-constraints) társviszony-létesítés üzemi virtuális hálózat létrehozása előtt használja.</span><span class="sxs-lookup"><span data-stu-id="9708d-252">Thoroughly familiarize yourself with important [virtual network peering constraints and behaviors](virtual-network-manage-peering.md#requirements-and-constraints) before creating a virtual network peering for production use.</span></span>
- <span data-ttu-id="9708d-253">További tudnivalók az összes [virtuális hálózati társviszony-létesítési beállítások](virtual-network-manage-peering.md#create-a-peering).</span><span class="sxs-lookup"><span data-stu-id="9708d-253">Learn about all [virtual network peering settings](virtual-network-manage-peering.md#create-a-peering).</span></span>
- <span data-ttu-id="9708d-254">Ismerje meg, hogyan túl[létrehoz egy központot, és a hálózati topológia irány](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) a virtuális hálózati társviszony-létesítés.</span><span class="sxs-lookup"><span data-stu-id="9708d-254">Learn how too[create a hub and spoke network topology](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) with virtual network peering.</span></span>
