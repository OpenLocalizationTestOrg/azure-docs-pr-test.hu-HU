---
title: "aaaManage NSG-ket hello Azure-portál használatával |} Microsoft Docs"
description: "Megtudhatja, hogyan toomanage meglévő NSG-ket hello Azure-portál használatával."
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: 
tags: azure-resource-manager
ms.assetid: 5d55679d-57da-457c-97dc-1e1973909ee5
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/14/2016
ms.author: jdial
ms.openlocfilehash: ad9a4060bd81bae4597ad5a4f59622e10cd214cf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-nsgs-using-hello-portal"></a><span data-ttu-id="55b68-103">Hello portálon NSG-k kezelése</span><span class="sxs-lookup"><span data-stu-id="55b68-103">Manage NSGs using hello portal</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="55b68-104">Portál</span><span class="sxs-lookup"><span data-stu-id="55b68-104">Portal</span></span>](virtual-network-manage-nsg-arm-portal.md)
> * [<span data-ttu-id="55b68-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="55b68-105">PowerShell</span></span>](virtual-network-manage-nsg-arm-ps.md)
> * [<span data-ttu-id="55b68-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="55b68-106">Azure CLI</span></span>](virtual-network-manage-nsg-arm-cli.md)
>

[!INCLUDE [virtual-network-manage-nsg-intro-include.md](../../includes/virtual-network-manage-nsg-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="55b68-107">Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="55b68-107">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="55b68-108">Ez a cikk a Microsoft azt javasolja, a legtöbb új központi telepítés helyett hello klasszikus üzembe helyezési modellel hello Resource Manager telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="55b68-108">This article covers using hello Resource Manager deployment model, which Microsoft recommends for most new deployments instead of hello classic deployment model.</span></span>
>

[!INCLUDE [virtual-network-manage-nsg-arm-scenario-include.md](../../includes/virtual-network-manage-nsg-arm-scenario-include.md)]

## <a name="retrieve-information"></a><span data-ttu-id="55b68-109">Információk beolvasása</span><span class="sxs-lookup"><span data-stu-id="55b68-109">Retrieve Information</span></span>
<span data-ttu-id="55b68-110">Megtekintheti a meglévő NSG-ket, szabályok lekérdezni egy meglévő NSG-t, és megtudhatja, milyen erőforrásokat egy NSG társítva.</span><span class="sxs-lookup"><span data-stu-id="55b68-110">You can view your existing NSGs, retrieve rules for an existing NSG, and find out what resources an NSG is associated to.</span></span>

### <a name="view-existing-nsgs"></a><span data-ttu-id="55b68-111">Meglévő NSG-k megtekintése</span><span class="sxs-lookup"><span data-stu-id="55b68-111">View existing NSGs</span></span>

<span data-ttu-id="55b68-112">tooview minden NSG-k létezik az előfizetés, a következő lépéseket teljes hello:</span><span class="sxs-lookup"><span data-stu-id="55b68-112">tooview all existing NSGs in a subscription, complete hello following steps:</span></span>

1. <span data-ttu-id="55b68-113">Egy böngészőből keresse meg a toohttp://portal.azure.com, és ha szükséges, jelentkezzen be az Azure-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="55b68-113">From a browser, navigate toohttp://portal.azure.com and, if necessary, sign in with your Azure account.</span></span>

2. <span data-ttu-id="55b68-114">Kattintson a **Tallózás >** > **hálózati biztonsági csoportok**.</span><span class="sxs-lookup"><span data-stu-id="55b68-114">Click **Browse >** > **Network security groups**.</span></span>

    ![Azure portál – NSG-k](./media/virtual-network-manage-nsg-arm-portal/figure1.png)

3. <span data-ttu-id="55b68-116">Ellenőrizze a hello hello az NSG-k listája **hálózati biztonsági csoportok** panelen.</span><span class="sxs-lookup"><span data-stu-id="55b68-116">Check hello list of NSGs in hello **Network security groups** blade.</span></span>

    ![Azure portál – NSG-k](./media/virtual-network-manage-nsg-arm-portal/figure2.png)

### <a name="view-nsgs-in-a-resource-group"></a><span data-ttu-id="55b68-118">Az erőforráscsoport nézet NSG-k</span><span class="sxs-lookup"><span data-stu-id="55b68-118">View NSGs in a resource group</span></span>

<span data-ttu-id="55b68-119">tooview hello listája NSG-ket a hello **RG-NSG** erőforráscsoport, a teljes hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="55b68-119">tooview hello list of NSGs in hello **RG-NSG** resource group, complete hello following steps:</span></span>

1. <span data-ttu-id="55b68-120">Kattintson a **erőforráscsoportok >** > **RG-NSG** > **...** .</span><span class="sxs-lookup"><span data-stu-id="55b68-120">Click **Resource groups >** > **RG-NSG** > **...**.</span></span>

    ![Azure portál – NSG-k](./media/virtual-network-manage-nsg-arm-portal/figure3.png)

2. <span data-ttu-id="55b68-122">A hello az erőforrások listájához, keresse meg elemek megjelenítése hello NSG ikonra, ahogy az hello **erőforrások** az alábbi panelen.</span><span class="sxs-lookup"><span data-stu-id="55b68-122">In hello list of resources, look for items displaying hello NSG icon, as shown in hello **Resources** blade below.</span></span>

    ![Azure portál – NSG-k](./media/virtual-network-manage-nsg-arm-portal/figure4.png)

### <a name="list-all-rules-for-an-nsg"></a><span data-ttu-id="55b68-124">A szabályok egy NSG listázása</span><span class="sxs-lookup"><span data-stu-id="55b68-124">List all rules for an NSG</span></span>

<span data-ttu-id="55b68-125">az NSG nevű tooview hello szabályainak **NSG-előtér**, teljes hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="55b68-125">tooview hello rules of an NSG named **NSG-FrontEnd**, complete hello following steps:</span></span>

1. <span data-ttu-id="55b68-126">A hello **hálózati biztonsági csoportok** panel vagy hello **erőforrások** panelen látható a fenti kattintson **NSG-előtér**.</span><span class="sxs-lookup"><span data-stu-id="55b68-126">From hello **Network security groups** blade, or hello **Resources** blade shown above, click **NSG-FrontEnd**.</span></span>

2. <span data-ttu-id="55b68-127">A hello **beállítások** lapra, majd **bejövő biztonsági szabályok**.</span><span class="sxs-lookup"><span data-stu-id="55b68-127">In hello **Settings** tab, click **Inbound security rules**.</span></span>

    ![Azure portál – NSG-k](./media/virtual-network-manage-nsg-arm-portal/figure5.png)

3. <span data-ttu-id="55b68-129">Hello **bejövő biztonsági szabályok** panel alább látható módon jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="55b68-129">hello **Inbound security rules** blade is displayed as shown below.</span></span>

    ![Azure portál – NSG-k](./media/virtual-network-manage-nsg-arm-portal/figure6.png)

4. <span data-ttu-id="55b68-131">A hello **beállítások** lapra, majd **kimenő biztonsági szabályok** toosee hello kimenő szabályok.</span><span class="sxs-lookup"><span data-stu-id="55b68-131">In hello **Settings** tab, click **Outbound security rules** toosee hello outbound rules.</span></span>

    > [!NOTE]
    > <span data-ttu-id="55b68-132">alapértelmezett szabályok tooview, kattintson a hello **alapértelmezett szabályok** ikon hello szabályok megjelenítő panelen hello hello tetején.</span><span class="sxs-lookup"><span data-stu-id="55b68-132">tooview default rules, click hello **Default rules** icon at hello top of hello blade that displays hello rules.</span></span>
    >

### <a name="view-nsgs-associations"></a><span data-ttu-id="55b68-133">Az NSG-társításának megtekintéséhez</span><span class="sxs-lookup"><span data-stu-id="55b68-133">View NSGs associations</span></span>

<span data-ttu-id="55b68-134">tooview milyen erőforrásokat hello **NSG-előtérbeli** NSG a következő lépéseket a társítása, teljes hello:</span><span class="sxs-lookup"><span data-stu-id="55b68-134">tooview what resources hello **NSG-FrontEnd** NSG is associate with, complete hello following steps:</span></span>

1. <span data-ttu-id="55b68-135">A hello **hálózati biztonsági csoportok** panel vagy hello **erőforrások** panelen látható a fenti kattintson **NSG-előtér**.</span><span class="sxs-lookup"><span data-stu-id="55b68-135">From hello **Network security groups** blade, or hello **Resources** blade shown above, click **NSG-FrontEnd**.</span></span>

2. <span data-ttu-id="55b68-136">A hello **beállítások** lapra, majd **alhálózatok** tooview milyen alhálózat társított toohello NSG.</span><span class="sxs-lookup"><span data-stu-id="55b68-136">In hello **Settings** tab, click **Subnets** tooview what subnets are associated toohello NSG.</span></span>

    ![Azure portál – NSG-k](./media/virtual-network-manage-nsg-arm-portal/figure7.png)

3. <span data-ttu-id="55b68-138">A hello **beállítások** lapra, majd **hálózati illesztőt** tooview hálózati adapterek Mik társított toohello NSG.</span><span class="sxs-lookup"><span data-stu-id="55b68-138">In hello **Settings** tab, click **Network interfaces** tooview what NICs are associated toohello NSG.</span></span>

## <a name="manage-rules"></a><span data-ttu-id="55b68-139">Szabályok kezelése</span><span class="sxs-lookup"><span data-stu-id="55b68-139">Manage rules</span></span>
<span data-ttu-id="55b68-140">Adja hozzá a meglévő NSG szabályok tooan, szerkesztheti a meglévő szabályokat, és törölje a szabályokat.</span><span class="sxs-lookup"><span data-stu-id="55b68-140">You can add rules tooan existing NSG, edit existing rules, and remove rules.</span></span>

### <a name="add-a-rule"></a><span data-ttu-id="55b68-141">Szabály hozzáadása</span><span class="sxs-lookup"><span data-stu-id="55b68-141">Add a rule</span></span>
<span data-ttu-id="55b68-142">egy szabály, amely lehetővé teszi tooadd **bejövő** forgalom tooport **443-as** bármely gépen toohello a **NSG-előtér** NSG-t, a következő lépéseket teljes hello:</span><span class="sxs-lookup"><span data-stu-id="55b68-142">tooadd a rule allowing **inbound** traffic tooport **443** from any machine toohello **NSG-FrontEnd** NSG, complete hello following steps:</span></span>

1. <span data-ttu-id="55b68-143">A hello **hálózati biztonsági csoportok** panel vagy hello **erőforrások** panelen látható a fenti kattintson **NSG-előtér**.</span><span class="sxs-lookup"><span data-stu-id="55b68-143">From hello **Network security groups** blade, or hello **Resources** blade shown above, click **NSG-FrontEnd**.</span></span>
2. <span data-ttu-id="55b68-144">A hello **beállítások** lapra, majd **bejövő biztonsági szabályok**.</span><span class="sxs-lookup"><span data-stu-id="55b68-144">In hello **Settings** tab, click **Inbound security rules**.</span></span>
3. <span data-ttu-id="55b68-145">A hello **bejövő biztonsági szabályok** panelen kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="55b68-145">In hello **Inbound security rules** blade, click **Add**.</span></span> <span data-ttu-id="55b68-146">Ezt követően a hello **Hozzáadás bejövő biztonsági szabály** panelen, töltse ki a hello értékek alább látható módon, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="55b68-146">Then, in hello **Add inbound security rule** blade, fill hello values as shown below, and then click **OK**.</span></span>

    ![Azure portál – NSG-k](./media/virtual-network-manage-nsg-arm-portal/figure8.png)

    <span data-ttu-id="55b68-148">Néhány másodperc múlva figyelje meg az új szabályt hello hello **bejövő biztonsági szabályok** panelen.</span><span class="sxs-lookup"><span data-stu-id="55b68-148">After a few seconds, notice hello new rule in hello **Inbound security rules** blade.</span></span>

    ![Azure portál – NSG-k](./media/virtual-network-manage-nsg-arm-portal/figure9.png)

### <a name="change-a-rule"></a><span data-ttu-id="55b68-150">Szabály módosítása</span><span class="sxs-lookup"><span data-stu-id="55b68-150">Change a rule</span></span>
<span data-ttu-id="55b68-151">a fenti tooallow létrehozott toochange hello szabály hello érkező bejövő adatforgalmat **Internet** lépések csak, teljes hello:</span><span class="sxs-lookup"><span data-stu-id="55b68-151">toochange hello rule created above tooallow inbound traffic from hello **Internet** only, complete hello following steps:</span></span>

1. <span data-ttu-id="55b68-152">A hello **hálózati biztonsági csoportok** panel vagy hello **erőforrások** panelen látható a fenti kattintson **NSG-előtér**.</span><span class="sxs-lookup"><span data-stu-id="55b68-152">From hello **Network security groups** blade, or hello **Resources** blade shown above, click **NSG-FrontEnd**.</span></span>
2. <span data-ttu-id="55b68-153">A hello **beállítások** fülre, kattintson a fenti létrehozott hello szabály.</span><span class="sxs-lookup"><span data-stu-id="55b68-153">In hello **Settings** tab, click hello rule created above.</span></span>
3. <span data-ttu-id="55b68-154">A hello **engedélyezése https** panelen, a módosítás hello **forrás** tulajdonság alább látható módon, és kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="55b68-154">In hello **allow-https** blade, change hello **Source** property as shown below, and then click **Save**.</span></span>

    ![Azure portál – NSG-k](./media/virtual-network-manage-nsg-arm-portal/figure10.png)

### <a name="delete-a-rule"></a><span data-ttu-id="55b68-156">Szabály törlése</span><span class="sxs-lookup"><span data-stu-id="55b68-156">Delete a rule</span></span>

<span data-ttu-id="55b68-157">toodelete hello szabály létrehozott fent, teljes hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="55b68-157">toodelete hello rule created above, complete hello following steps:</span></span>

1. <span data-ttu-id="55b68-158">A hello **hálózati biztonsági csoportok** panel vagy hello **erőforrások** panelen látható a fenti kattintson **NSG-előtér**.</span><span class="sxs-lookup"><span data-stu-id="55b68-158">From hello **Network security groups** blade, or hello **Resources** blade shown above, click **NSG-FrontEnd**.</span></span>
2. <span data-ttu-id="55b68-159">A hello **beállítások** fülre, kattintson a fenti létrehozott hello szabály.</span><span class="sxs-lookup"><span data-stu-id="55b68-159">In hello **Settings** tab, click hello rule created above.</span></span>
3. <span data-ttu-id="55b68-160">A hello **engedélyezése https** panelen kattintson a **törlése**, és kattintson a **Igen**.</span><span class="sxs-lookup"><span data-stu-id="55b68-160">In hello **allow-https** blade, click **Delete**, and then click **Yes**.</span></span>

    ![Azure portál – NSG-k](./media/virtual-network-manage-nsg-arm-portal/figure11.png)

## <a name="manage-associations"></a><span data-ttu-id="55b68-162">Társítások kezelése</span><span class="sxs-lookup"><span data-stu-id="55b68-162">Manage associations</span></span>
<span data-ttu-id="55b68-163">Az NSG toosubnets és a hálózati adapterek is hozzárendelhető.</span><span class="sxs-lookup"><span data-stu-id="55b68-163">You can associate an NSG toosubnets and NICs.</span></span> <span data-ttu-id="55b68-164">Is is leválasztja az összes erőforrásból, amelyekhez társítva vannak a NSG.</span><span class="sxs-lookup"><span data-stu-id="55b68-164">You can also dissociate an NSG from any resource it's associated to.</span></span>

### <a name="associate-an-nsg-tooa-nic"></a><span data-ttu-id="55b68-165">Társítson egy NSG tooa hálózati adapter</span><span class="sxs-lookup"><span data-stu-id="55b68-165">Associate an NSG tooa NIC</span></span>
<span data-ttu-id="55b68-166">tooassociate hello **NSG-előtérbeli** NSG toohello **TestNICWeb1** a hálózati adapter teljes hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="55b68-166">tooassociate hello **NSG-FrontEnd** NSG toohello **TestNICWeb1** NIC, complete hello following steps:</span></span>

1. <span data-ttu-id="55b68-167">A hello **hálózati biztonsági csoportok** panel vagy hello **erőforrások** panelen látható a fenti kattintson **NSG-előtér**.</span><span class="sxs-lookup"><span data-stu-id="55b68-167">From hello **Network security groups** blade, or hello **Resources** blade shown above, click **NSG-FrontEnd**.</span></span>
2. <span data-ttu-id="55b68-168">A hello **beállítások** lapra, majd **hálózati illesztőt** > **társítása** > **TestNICWeb1**.</span><span class="sxs-lookup"><span data-stu-id="55b68-168">In hello **Settings** tab, click **Network interfaces** > **Associate** > **TestNICWeb1**.</span></span>

    ![Azure portál – NSG-k](./media/virtual-network-manage-nsg-arm-portal/figure12.png)

### <a name="dissociate-an-nsg-from-a-nic"></a><span data-ttu-id="55b68-170">A társítást egy NSG-t a hálózati Adapterhez</span><span class="sxs-lookup"><span data-stu-id="55b68-170">Dissociate an NSG from a NIC</span></span>

<span data-ttu-id="55b68-171">toodissociate hello **NSG-előtérbeli** hello az NSG **TestNICWeb1** a hálózati adapter teljes hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="55b68-171">toodissociate hello **NSG-FrontEnd** NSG from hello **TestNICWeb1** NIC, complete hello following steps:</span></span>

1. <span data-ttu-id="55b68-172">A hello Azure-portálon, kattintson az **erőforráscsoportok >** > **RG-NSG** > **...**   >  **TestNICWeb1**.</span><span class="sxs-lookup"><span data-stu-id="55b68-172">From hello Azure portal, click **Resource groups >** > **RG-NSG** > **...** > **TestNICWeb1**.</span></span>

2. <span data-ttu-id="55b68-173">A hello **TestNICWeb1** paneljén kattintson **biztonsági módosítása...**   >  **Nincs**.</span><span class="sxs-lookup"><span data-stu-id="55b68-173">In hello **TestNICWeb1** blade, click **Change security...** > **None**.</span></span>

    ![Azure portál – NSG-k](./media/virtual-network-manage-nsg-arm-portal/figure13.png)

> [!NOTE]
> <span data-ttu-id="55b68-175">A panel tooassociate hello NIC tooany meglévő NSG-t is használhatja.</span><span class="sxs-lookup"><span data-stu-id="55b68-175">You can also use this blade tooassociate hello NIC tooany existing NSG.</span></span>
>

### <a name="dissociate-an-nsg-from-a-subnet"></a><span data-ttu-id="55b68-176">Az NSG alhálózatból származó leválasztani</span><span class="sxs-lookup"><span data-stu-id="55b68-176">Dissociate an NSG from a subnet</span></span>

<span data-ttu-id="55b68-177">toodissociate hello **NSG-előtérbeli** hello az NSG **előtér** alhálózat, a teljes hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="55b68-177">toodissociate hello **NSG-FrontEnd** NSG from hello **FrontEnd** subnet, complete hello following steps:</span></span>

1. <span data-ttu-id="55b68-178">A hello Azure-portálon, kattintson az **erőforráscsoportok >** > **RG-NSG** > **...**   >  **TestVNet**.</span><span class="sxs-lookup"><span data-stu-id="55b68-178">From hello Azure portal, click **Resource groups >** > **RG-NSG** > **...** > **TestVNet**.</span></span>

2. <span data-ttu-id="55b68-179">A hello **beállítások** panelen kattintson a **alhálózatok** > **előtér** > **hálózati biztonsági csoport**  >  **Nincs**.</span><span class="sxs-lookup"><span data-stu-id="55b68-179">In hello **Settings** blade, click **Subnets** > **FrontEnd** > **Network security group** > **None**.</span></span>

    ![Azure portál – NSG-k](./media/virtual-network-manage-nsg-arm-portal/figure14.png)

3. <span data-ttu-id="55b68-181">A hello **előtér** panelen kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="55b68-181">In hello **FrontEnd** blade, click **Save**.</span></span>

    ![Azure portál – NSG-k](./media/virtual-network-manage-nsg-arm-portal/figure15.png)

### <a name="associate-an-nsg-tooa-subnet"></a><span data-ttu-id="55b68-183">Társítsa az NSG-tooa alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="55b68-183">Associate an NSG tooa subnet</span></span>

<span data-ttu-id="55b68-184">tooassociate hello **NSG-előtérbeli** NSG toohello **FronEnd** alhálózati újra, a teljes hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="55b68-184">tooassociate hello **NSG-FrontEnd** NSG toohello **FronEnd** subnet again, complete hello following steps:</span></span>

1. <span data-ttu-id="55b68-185">A hello Azure-portálon, kattintson az **erőforráscsoportok >** > **RG-NSG** > **...**   >  **TestVNet**.</span><span class="sxs-lookup"><span data-stu-id="55b68-185">From hello Azure portal, click **Resource groups >** > **RG-NSG** > **...** > **TestVNet**.</span></span>
2. <span data-ttu-id="55b68-186">A hello **beállítások** panelen kattintson a **alhálózatok** > **előtér** > **hálózati biztonsági csoport**  >  **NSG-előtérbeli**.</span><span class="sxs-lookup"><span data-stu-id="55b68-186">In hello **Settings** blade, click **Subnets** > **FrontEnd** > **Network security group** > **NSG-FrontEnd**.</span></span>
3. <span data-ttu-id="55b68-187">A hello **előtér** panelen kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="55b68-187">In hello **FrontEnd** blade, click **Save**.</span></span>

> [!NOTE]
> <span data-ttu-id="55b68-188">Egy NSG tooa alhálózathoz a thh NSG-t is rendelhet **beállítások** panelen.</span><span class="sxs-lookup"><span data-stu-id="55b68-188">You can also associate an NSG tooa subnet from thh NSG's **Settings** blade.</span></span>
>

## <a name="delete-an-nsg"></a><span data-ttu-id="55b68-189">Az NSG törlése</span><span class="sxs-lookup"><span data-stu-id="55b68-189">Delete an NSG</span></span>
<span data-ttu-id="55b68-190">Ha nem kapcsolódnak hozzá erőforrás tooany csak törlése egy NSG.</span><span class="sxs-lookup"><span data-stu-id="55b68-190">You can only delete an NSG if it's not associated tooany resource.</span></span> <span data-ttu-id="55b68-191">az NSG-t, a lépéseket követve teljes hello toodelete:.</span><span class="sxs-lookup"><span data-stu-id="55b68-191">toodelete an NSG, complete hello following steps:.</span></span>

1. <span data-ttu-id="55b68-192">A hello Azure-portálon, kattintson az **erőforráscsoportok >** > **RG-NSG** > **...**   >  **NSG-előtérbeli**.</span><span class="sxs-lookup"><span data-stu-id="55b68-192">From hello Azure portal, click **Resource groups >** > **RG-NSG** > **...** > **NSG-FrontEnd**.</span></span>
2. <span data-ttu-id="55b68-193">A hello **beállítások** panelen kattintson a **hálózati illesztőt**.</span><span class="sxs-lookup"><span data-stu-id="55b68-193">In hello **Settings** blade, click **Network interfaces**.</span></span>
3. <span data-ttu-id="55b68-194">Ha egyetlen hálózati adapterrel felsorolt, hello NIC kattintson, és hajtsa végre a 2. lépés [leválasztani a hálózati Adapterhez egy NSG](#Dissociate-an-NSG-from-a-NIC).</span><span class="sxs-lookup"><span data-stu-id="55b68-194">If there are any NICs listed, click hello NIC, and follow step 2 in [Dissociate an NSG from a NIC](#Dissociate-an-NSG-from-a-NIC).</span></span>
4. <span data-ttu-id="55b68-195">Ismételje meg a 3. lépés az egyes hálózati adapterhez.</span><span class="sxs-lookup"><span data-stu-id="55b68-195">Repeat step 3 for each NIC.</span></span>
5. <span data-ttu-id="55b68-196">A hello **beállítások** panelen kattintson a **alhálózatok**.</span><span class="sxs-lookup"><span data-stu-id="55b68-196">In hello **Settings** blade, click **Subnets**.</span></span>
6. <span data-ttu-id="55b68-197">Ha nincsenek felsorolva alhálózatok, kattintson a hello alhálózatot, és kövesse a 2. és 3 [leválasztani az NSG alhálózatból származó](#Dissociate-an-NSG-from-a-subnet).</span><span class="sxs-lookup"><span data-stu-id="55b68-197">If there are any subnets listed, click hello subnet and follow steps 2 and 3 in [Dissociate an NSG from a subnet](#Dissociate-an-NSG-from-a-subnet).</span></span>
7. <span data-ttu-id="55b68-198">Bal oldali toohello görget **NSG-előtér** panelen, majd kattintson a **törlése** > **Igen**.</span><span class="sxs-lookup"><span data-stu-id="55b68-198">Scrolls left toohello **NSG-FrontEnd** blade, then click **Delete** > **Yes**.</span></span>

    ![Azure portál – NSG-k](./media/virtual-network-manage-nsg-arm-portal/figure16.png)

## <a name="next-steps"></a><span data-ttu-id="55b68-200">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="55b68-200">Next steps</span></span>
* <span data-ttu-id="55b68-201">[Naplózás engedélyezése](virtual-network-nsg-manage-log.md) az NSG-ket.</span><span class="sxs-lookup"><span data-stu-id="55b68-201">[Enable logging](virtual-network-nsg-manage-log.md) for NSGs.</span></span>
