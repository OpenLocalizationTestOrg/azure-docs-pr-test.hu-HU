---
title: "Az Azure portál használatával NSG-k kezelése |} Microsoft Docs"
description: "Megtudhatja, hogyan kezelheti a meglévő NSG-ket az Azure portál használatával."
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
ms.openlocfilehash: e9bcf8a893ff209337f6a5763b631a22f8514e20
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="manage-nsgs-using-the-portal"></a><span data-ttu-id="14ab0-103">A portál használatával NSG-k kezelése</span><span class="sxs-lookup"><span data-stu-id="14ab0-103">Manage NSGs using the portal</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="14ab0-104">Portal</span><span class="sxs-lookup"><span data-stu-id="14ab0-104">Portal</span></span>](virtual-network-manage-nsg-arm-portal.md)
> * [<span data-ttu-id="14ab0-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="14ab0-105">PowerShell</span></span>](virtual-network-manage-nsg-arm-ps.md)
> * [<span data-ttu-id="14ab0-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="14ab0-106">Azure CLI</span></span>](virtual-network-manage-nsg-arm-cli.md)
>

[!INCLUDE [virtual-network-manage-nsg-intro-include.md](../../includes/virtual-network-manage-nsg-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="14ab0-107">Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="14ab0-107">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="14ab0-108">Ez a cikk a Microsoft azt javasolja, hogy a klasszikus üzembe helyezési modellel helyett az új telepítések esetén a Resource Manager telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="14ab0-108">This article covers using the Resource Manager deployment model, which Microsoft recommends for most new deployments instead of the classic deployment model.</span></span>
>

[!INCLUDE [virtual-network-manage-nsg-arm-scenario-include.md](../../includes/virtual-network-manage-nsg-arm-scenario-include.md)]

## <a name="retrieve-information"></a><span data-ttu-id="14ab0-109">Információk beolvasása</span><span class="sxs-lookup"><span data-stu-id="14ab0-109">Retrieve Information</span></span>
<span data-ttu-id="14ab0-110">Megtekintheti a meglévő NSG-ket, szabályok lekérdezni egy meglévő NSG-t, és megtudhatja, milyen erőforrásokat egy NSG társítva.</span><span class="sxs-lookup"><span data-stu-id="14ab0-110">You can view your existing NSGs, retrieve rules for an existing NSG, and find out what resources an NSG is associated to.</span></span>

### <a name="view-existing-nsgs"></a><span data-ttu-id="14ab0-111">Meglévő NSG-k megtekintése</span><span class="sxs-lookup"><span data-stu-id="14ab0-111">View existing NSGs</span></span>

<span data-ttu-id="14ab0-112">Előfizetés az összes meglévő NSG-ket megtekintéséhez kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="14ab0-112">To view all existing NSGs in a subscription, complete the following steps:</span></span>

1. <span data-ttu-id="14ab0-113">Egy böngészőből keresse fel a http://portal.azure.com címet, majd jelentkezzen be az Azure-fiókjával, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="14ab0-113">From a browser, navigate to http://portal.azure.com and, if necessary, sign in with your Azure account.</span></span>

2. <span data-ttu-id="14ab0-114">Kattintson a **Tallózás >** > **hálózati biztonsági csoportok**.</span><span class="sxs-lookup"><span data-stu-id="14ab0-114">Click **Browse >** > **Network security groups**.</span></span>

    ![Azure portál – NSG-k](./media/virtual-network-manage-nsg-arm-portal/figure1.png)

3. <span data-ttu-id="14ab0-116">Az NSG-ket listáját a **hálózati biztonsági csoportok** panelen.</span><span class="sxs-lookup"><span data-stu-id="14ab0-116">Check the list of NSGs in the **Network security groups** blade.</span></span>

    ![Azure portál – NSG-k](./media/virtual-network-manage-nsg-arm-portal/figure2.png)

### <a name="view-nsgs-in-a-resource-group"></a><span data-ttu-id="14ab0-118">Az erőforráscsoport nézet NSG-k</span><span class="sxs-lookup"><span data-stu-id="14ab0-118">View NSGs in a resource group</span></span>

<span data-ttu-id="14ab0-119">Az NSG-k listájának megtekintéséhez a **RG-NSG** erőforrás csoportjában hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="14ab0-119">To view the list of NSGs in the **RG-NSG** resource group, complete the following steps:</span></span>

1. <span data-ttu-id="14ab0-120">Kattintson a **erőforráscsoportok >** > **RG-NSG** > **...** .</span><span class="sxs-lookup"><span data-stu-id="14ab0-120">Click **Resource groups >** > **RG-NSG** > **...**.</span></span>

    ![Azure portál – NSG-k](./media/virtual-network-manage-nsg-arm-portal/figure3.png)

2. <span data-ttu-id="14ab0-122">Az erőforrások listájához, keresse meg elemek megjelenítése a NSG ikonra, ahogy az a **erőforrások** az alábbi panelen.</span><span class="sxs-lookup"><span data-stu-id="14ab0-122">In the list of resources, look for items displaying the NSG icon, as shown in the **Resources** blade below.</span></span>

    ![Azure portál – NSG-k](./media/virtual-network-manage-nsg-arm-portal/figure4.png)

### <a name="list-all-rules-for-an-nsg"></a><span data-ttu-id="14ab0-124">A szabályok egy NSG listázása</span><span class="sxs-lookup"><span data-stu-id="14ab0-124">List all rules for an NSG</span></span>

<span data-ttu-id="14ab0-125">Az NSG nevű szabályainak megtekintéséhez **NSG-előtérbeli**, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="14ab0-125">To view the rules of an NSG named **NSG-FrontEnd**, complete the following steps:</span></span>

1. <span data-ttu-id="14ab0-126">Az a **hálózati biztonsági csoportok** panelen, vagy a **erőforrások** panelen látható a fenti kattintson **NSG-előtér**.</span><span class="sxs-lookup"><span data-stu-id="14ab0-126">From the **Network security groups** blade, or the **Resources** blade shown above, click **NSG-FrontEnd**.</span></span>

2. <span data-ttu-id="14ab0-127">Az a **beállítások** lapra, majd **bejövő biztonsági szabályok**.</span><span class="sxs-lookup"><span data-stu-id="14ab0-127">In the **Settings** tab, click **Inbound security rules**.</span></span>

    ![Azure portál – NSG-k](./media/virtual-network-manage-nsg-arm-portal/figure5.png)

3. <span data-ttu-id="14ab0-129">A **bejövő biztonsági szabályok** panel alább látható módon jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="14ab0-129">The **Inbound security rules** blade is displayed as shown below.</span></span>

    ![Azure portál – NSG-k](./media/virtual-network-manage-nsg-arm-portal/figure6.png)

4. <span data-ttu-id="14ab0-131">Az a **beállítások** lapra, majd **kimenő biztonsági szabályok** a kimenő szabályok megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="14ab0-131">In the **Settings** tab, click **Outbound security rules** to see the outbound rules.</span></span>

    > [!NOTE]
    > <span data-ttu-id="14ab0-132">Alapértelmezett szabályok megtekintéséhez kattintson a **alapértelmezett szabályok** ikon, amely azokat a szabályokat jeleníti meg a panel tetején.</span><span class="sxs-lookup"><span data-stu-id="14ab0-132">To view default rules, click the **Default rules** icon at the top of the blade that displays the rules.</span></span>
    >

### <a name="view-nsgs-associations"></a><span data-ttu-id="14ab0-133">Az NSG-társításának megtekintéséhez</span><span class="sxs-lookup"><span data-stu-id="14ab0-133">View NSGs associations</span></span>

<span data-ttu-id="14ab0-134">Milyen erőforrások megtekintése a **NSG-előtérbeli** NSG társítása a, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="14ab0-134">To view what resources the **NSG-FrontEnd** NSG is associate with, complete the following steps:</span></span>

1. <span data-ttu-id="14ab0-135">Az a **hálózati biztonsági csoportok** panelen, vagy a **erőforrások** panelen látható a fenti kattintson **NSG-előtér**.</span><span class="sxs-lookup"><span data-stu-id="14ab0-135">From the **Network security groups** blade, or the **Resources** blade shown above, click **NSG-FrontEnd**.</span></span>

2. <span data-ttu-id="14ab0-136">Az a **beállítások** lapra, majd **alhálózatok** milyen alhálózatok társítva a NSG megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="14ab0-136">In the **Settings** tab, click **Subnets** to view what subnets are associated to the NSG.</span></span>

    ![Azure portál – NSG-k](./media/virtual-network-manage-nsg-arm-portal/figure7.png)

3. <span data-ttu-id="14ab0-138">Az a **beállítások** lapra, majd **hálózati illesztőt** megtekintéséhez a hálózati adapterek Mik az NSG társítva.</span><span class="sxs-lookup"><span data-stu-id="14ab0-138">In the **Settings** tab, click **Network interfaces** to view what NICs are associated to the NSG.</span></span>

## <a name="manage-rules"></a><span data-ttu-id="14ab0-139">Szabályok kezelése</span><span class="sxs-lookup"><span data-stu-id="14ab0-139">Manage rules</span></span>
<span data-ttu-id="14ab0-140">Szabályok hozzáadása egy meglévő NSG, szerkesztheti a meglévő szabályokat, és törölje a szabályokat.</span><span class="sxs-lookup"><span data-stu-id="14ab0-140">You can add rules to an existing NSG, edit existing rules, and remove rules.</span></span>

### <a name="add-a-rule"></a><span data-ttu-id="14ab0-141">Szabály hozzáadása</span><span class="sxs-lookup"><span data-stu-id="14ab0-141">Add a rule</span></span>
<span data-ttu-id="14ab0-142">Hozzáadása egy szabály, amely lehetővé teszi **bejövő** forgalmának portra **443-as** bármely számítógépről történő a **NSG-előtér** NSG, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="14ab0-142">To add a rule allowing **inbound** traffic to port **443** from any machine to the **NSG-FrontEnd** NSG, complete the following steps:</span></span>

1. <span data-ttu-id="14ab0-143">Az a **hálózati biztonsági csoportok** panelen, vagy a **erőforrások** panelen látható a fenti kattintson **NSG-előtér**.</span><span class="sxs-lookup"><span data-stu-id="14ab0-143">From the **Network security groups** blade, or the **Resources** blade shown above, click **NSG-FrontEnd**.</span></span>
2. <span data-ttu-id="14ab0-144">Az a **beállítások** lapra, majd **bejövő biztonsági szabályok**.</span><span class="sxs-lookup"><span data-stu-id="14ab0-144">In the **Settings** tab, click **Inbound security rules**.</span></span>
3. <span data-ttu-id="14ab0-145">Az a **bejövő biztonsági szabályok** panelen kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="14ab0-145">In the **Inbound security rules** blade, click **Add**.</span></span> <span data-ttu-id="14ab0-146">Ezt követően a a **Hozzáadás bejövő biztonsági szabály** panelen alább látható módon, töltse ki az értékeket, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="14ab0-146">Then, in the **Add inbound security rule** blade, fill the values as shown below, and then click **OK**.</span></span>

    ![Azure portál – NSG-k](./media/virtual-network-manage-nsg-arm-portal/figure8.png)

    <span data-ttu-id="14ab0-148">Néhány másodperc múlva figyelje meg, az új szabály az **bejövő biztonsági szabályok** panelen.</span><span class="sxs-lookup"><span data-stu-id="14ab0-148">After a few seconds, notice the new rule in the **Inbound security rules** blade.</span></span>

    ![Azure portál – NSG-k](./media/virtual-network-manage-nsg-arm-portal/figure9.png)

### <a name="change-a-rule"></a><span data-ttu-id="14ab0-150">Szabály módosítása</span><span class="sxs-lookup"><span data-stu-id="14ab0-150">Change a rule</span></span>
<span data-ttu-id="14ab0-151">A szabály a bejövő adatforgalom engedélyezésére a fenti létrehozott módosítása a **Internet** csak, hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="14ab0-151">To change the rule created above to allow inbound traffic from the **Internet** only, complete the following steps:</span></span>

1. <span data-ttu-id="14ab0-152">Az a **hálózati biztonsági csoportok** panelen, vagy a **erőforrások** panelen látható a fenti kattintson **NSG-előtér**.</span><span class="sxs-lookup"><span data-stu-id="14ab0-152">From the **Network security groups** blade, or the **Resources** blade shown above, click **NSG-FrontEnd**.</span></span>
2. <span data-ttu-id="14ab0-153">Az a **beállítások** fülre, kattintson a fenti létrehozott szabály.</span><span class="sxs-lookup"><span data-stu-id="14ab0-153">In the **Settings** tab, click the rule created above.</span></span>
3. <span data-ttu-id="14ab0-154">Az a **engedélyezése https** panelen, módosítsa a **forrás** tulajdonság alább látható módon, és kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="14ab0-154">In the **allow-https** blade, change the **Source** property as shown below, and then click **Save**.</span></span>

    ![Azure portál – NSG-k](./media/virtual-network-manage-nsg-arm-portal/figure10.png)

### <a name="delete-a-rule"></a><span data-ttu-id="14ab0-156">Szabály törlése</span><span class="sxs-lookup"><span data-stu-id="14ab0-156">Delete a rule</span></span>

<span data-ttu-id="14ab0-157">A fentiekben létrehozott szabály törléséhez kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="14ab0-157">To delete the rule created above, complete the following steps:</span></span>

1. <span data-ttu-id="14ab0-158">Az a **hálózati biztonsági csoportok** panelen, vagy a **erőforrások** panelen látható a fenti kattintson **NSG-előtér**.</span><span class="sxs-lookup"><span data-stu-id="14ab0-158">From the **Network security groups** blade, or the **Resources** blade shown above, click **NSG-FrontEnd**.</span></span>
2. <span data-ttu-id="14ab0-159">Az a **beállítások** fülre, kattintson a fenti létrehozott szabály.</span><span class="sxs-lookup"><span data-stu-id="14ab0-159">In the **Settings** tab, click the rule created above.</span></span>
3. <span data-ttu-id="14ab0-160">Az a **engedélyezése https** panelen kattintson a **törlése**, és kattintson a **Igen**.</span><span class="sxs-lookup"><span data-stu-id="14ab0-160">In the **allow-https** blade, click **Delete**, and then click **Yes**.</span></span>

    ![Azure portál – NSG-k](./media/virtual-network-manage-nsg-arm-portal/figure11.png)

## <a name="manage-associations"></a><span data-ttu-id="14ab0-162">Társítások kezelése</span><span class="sxs-lookup"><span data-stu-id="14ab0-162">Manage associations</span></span>
<span data-ttu-id="14ab0-163">Az NSG alhálózatokra és hálózati adapterek is hozzárendelhető.</span><span class="sxs-lookup"><span data-stu-id="14ab0-163">You can associate an NSG to subnets and NICs.</span></span> <span data-ttu-id="14ab0-164">Is is leválasztja az összes erőforrásból, amelyekhez társítva vannak a NSG.</span><span class="sxs-lookup"><span data-stu-id="14ab0-164">You can also dissociate an NSG from any resource it's associated to.</span></span>

### <a name="associate-an-nsg-to-a-nic"></a><span data-ttu-id="14ab0-165">Társít egy NSG egy hálózati adapter</span><span class="sxs-lookup"><span data-stu-id="14ab0-165">Associate an NSG to a NIC</span></span>
<span data-ttu-id="14ab0-166">Rendelje hozzá a a **NSG-előtérbeli** NSG a **TestNICWeb1** hálózati adapter, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="14ab0-166">To associate the **NSG-FrontEnd** NSG to the **TestNICWeb1** NIC, complete the following steps:</span></span>

1. <span data-ttu-id="14ab0-167">Az a **hálózati biztonsági csoportok** panelen, vagy a **erőforrások** panelen látható a fenti kattintson **NSG-előtér**.</span><span class="sxs-lookup"><span data-stu-id="14ab0-167">From the **Network security groups** blade, or the **Resources** blade shown above, click **NSG-FrontEnd**.</span></span>
2. <span data-ttu-id="14ab0-168">Az a **beállítások** lapra, majd **hálózati illesztőt** > **társítása** > **TestNICWeb1**.</span><span class="sxs-lookup"><span data-stu-id="14ab0-168">In the **Settings** tab, click **Network interfaces** > **Associate** > **TestNICWeb1**.</span></span>

    ![Azure portál – NSG-k](./media/virtual-network-manage-nsg-arm-portal/figure12.png)

### <a name="dissociate-an-nsg-from-a-nic"></a><span data-ttu-id="14ab0-170">A társítást egy NSG-t a hálózati Adapterhez</span><span class="sxs-lookup"><span data-stu-id="14ab0-170">Dissociate an NSG from a NIC</span></span>

<span data-ttu-id="14ab0-171">Leválasztja a **NSG-előtér** az NSG-t a **TestNICWeb1** hálózati adapter, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="14ab0-171">To dissociate the **NSG-FrontEnd** NSG from the **TestNICWeb1** NIC, complete the following steps:</span></span>

1. <span data-ttu-id="14ab0-172">Az Azure-portálon kattintson **erőforráscsoportok >** > **RG-NSG** > **...**   >  **TestNICWeb1**.</span><span class="sxs-lookup"><span data-stu-id="14ab0-172">From the Azure portal, click **Resource groups >** > **RG-NSG** > **...** > **TestNICWeb1**.</span></span>

2. <span data-ttu-id="14ab0-173">Az a **TestNICWeb1** paneljén kattintson **biztonsági módosítása...**   >  **Nincs**.</span><span class="sxs-lookup"><span data-stu-id="14ab0-173">In the **TestNICWeb1** blade, click **Change security...** > **None**.</span></span>

    ![Azure portál – NSG-k](./media/virtual-network-manage-nsg-arm-portal/figure13.png)

> [!NOTE]
> <span data-ttu-id="14ab0-175">A hálózati adapterről bármely létező NSG társítása használhatja ezt a panelt.</span><span class="sxs-lookup"><span data-stu-id="14ab0-175">You can also use this blade to associate the NIC to any existing NSG.</span></span>
>

### <a name="dissociate-an-nsg-from-a-subnet"></a><span data-ttu-id="14ab0-176">Az NSG alhálózatból származó leválasztani</span><span class="sxs-lookup"><span data-stu-id="14ab0-176">Dissociate an NSG from a subnet</span></span>

<span data-ttu-id="14ab0-177">Leválasztja a **NSG-előtér** az NSG-t a **előtér** alhálózati, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="14ab0-177">To dissociate the **NSG-FrontEnd** NSG from the **FrontEnd** subnet, complete the following steps:</span></span>

1. <span data-ttu-id="14ab0-178">Az Azure-portálon kattintson **erőforráscsoportok >** > **RG-NSG** > **...**   >  **TestVNet**.</span><span class="sxs-lookup"><span data-stu-id="14ab0-178">From the Azure portal, click **Resource groups >** > **RG-NSG** > **...** > **TestVNet**.</span></span>

2. <span data-ttu-id="14ab0-179">Az a **beállítások** panelen kattintson a **alhálózatok** > **előtér** > **hálózati biztonsági csoport**  >  **Nincs**.</span><span class="sxs-lookup"><span data-stu-id="14ab0-179">In the **Settings** blade, click **Subnets** > **FrontEnd** > **Network security group** > **None**.</span></span>

    ![Azure portál – NSG-k](./media/virtual-network-manage-nsg-arm-portal/figure14.png)

3. <span data-ttu-id="14ab0-181">Az a **előtér** panelen kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="14ab0-181">In the **FrontEnd** blade, click **Save**.</span></span>

    ![Azure portál – NSG-k](./media/virtual-network-manage-nsg-arm-portal/figure15.png)

### <a name="associate-an-nsg-to-a-subnet"></a><span data-ttu-id="14ab0-183">Társít egy NSG alhálózathoz</span><span class="sxs-lookup"><span data-stu-id="14ab0-183">Associate an NSG to a subnet</span></span>

<span data-ttu-id="14ab0-184">Rendelje hozzá a a **NSG-előtérbeli** NSG a **FronEnd** alhálózat ebben az esetben kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="14ab0-184">To associate the **NSG-FrontEnd** NSG to the **FronEnd** subnet again, complete the following steps:</span></span>

1. <span data-ttu-id="14ab0-185">Az Azure-portálon kattintson **erőforráscsoportok >** > **RG-NSG** > **...**   >  **TestVNet**.</span><span class="sxs-lookup"><span data-stu-id="14ab0-185">From the Azure portal, click **Resource groups >** > **RG-NSG** > **...** > **TestVNet**.</span></span>
2. <span data-ttu-id="14ab0-186">Az a **beállítások** panelen kattintson a **alhálózatok** > **előtér** > **hálózati biztonsági csoport** > **NSG-előtér**.</span><span class="sxs-lookup"><span data-stu-id="14ab0-186">In the **Settings** blade, click **Subnets** > **FrontEnd** > **Network security group** > **NSG-FrontEnd**.</span></span>
3. <span data-ttu-id="14ab0-187">Az a **előtér** panelen kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="14ab0-187">In the **FrontEnd** blade, click **Save**.</span></span>

> [!NOTE]
> <span data-ttu-id="14ab0-188">Hozzá lehet rendelni egy NSG alhálózathoz thh NSG-t a **beállítások** panelen.</span><span class="sxs-lookup"><span data-stu-id="14ab0-188">You can also associate an NSG to a subnet from thh NSG's **Settings** blade.</span></span>
>

## <a name="delete-an-nsg"></a><span data-ttu-id="14ab0-189">Az NSG törlése</span><span class="sxs-lookup"><span data-stu-id="14ab0-189">Delete an NSG</span></span>
<span data-ttu-id="14ab0-190">Az NSG csak törölheti, ha nem kapcsolódik semmilyen erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="14ab0-190">You can only delete an NSG if it's not associated to any resource.</span></span> <span data-ttu-id="14ab0-191">Ha törölni szeretne egy NSG-t, az alábbi lépésekkel:.</span><span class="sxs-lookup"><span data-stu-id="14ab0-191">To delete an NSG, complete the following steps:.</span></span>

1. <span data-ttu-id="14ab0-192">Az Azure-portálon kattintson **erőforráscsoportok >** > **RG-NSG** > **...**   >  **NSG-előtérbeli**.</span><span class="sxs-lookup"><span data-stu-id="14ab0-192">From the Azure portal, click **Resource groups >** > **RG-NSG** > **...** > **NSG-FrontEnd**.</span></span>
2. <span data-ttu-id="14ab0-193">Az a **beállítások** panelen kattintson a **hálózati illesztőt**.</span><span class="sxs-lookup"><span data-stu-id="14ab0-193">In the **Settings** blade, click **Network interfaces**.</span></span>
3. <span data-ttu-id="14ab0-194">Ha egyetlen hálózati adapterrel felsorolt, kattintson a hálózati adapter, és hajtsa végre a 2. lépés a [leválasztani a hálózati Adapterhez egy NSG](#Dissociate-an-NSG-from-a-NIC).</span><span class="sxs-lookup"><span data-stu-id="14ab0-194">If there are any NICs listed, click the NIC, and follow step 2 in [Dissociate an NSG from a NIC](#Dissociate-an-NSG-from-a-NIC).</span></span>
4. <span data-ttu-id="14ab0-195">Ismételje meg a 3. lépés az egyes hálózati adapterhez.</span><span class="sxs-lookup"><span data-stu-id="14ab0-195">Repeat step 3 for each NIC.</span></span>
5. <span data-ttu-id="14ab0-196">Az a **beállítások** panelen kattintson a **alhálózatok**.</span><span class="sxs-lookup"><span data-stu-id="14ab0-196">In the **Settings** blade, click **Subnets**.</span></span>
6. <span data-ttu-id="14ab0-197">Ha nincsenek felsorolva alhálózatok, kattintson az alhálózat, és hajtsa végre a 2. és 3 [leválasztani az NSG alhálózatból származó](#Dissociate-an-NSG-from-a-subnet).</span><span class="sxs-lookup"><span data-stu-id="14ab0-197">If there are any subnets listed, click the subnet and follow steps 2 and 3 in [Dissociate an NSG from a subnet](#Dissociate-an-NSG-from-a-subnet).</span></span>
7. <span data-ttu-id="14ab0-198">Bal-görget a **NSG-előtér** panelen kattintson a **törlése** > **Igen**.</span><span class="sxs-lookup"><span data-stu-id="14ab0-198">Scrolls left to the **NSG-FrontEnd** blade, then click **Delete** > **Yes**.</span></span>

    ![Azure portál – NSG-k](./media/virtual-network-manage-nsg-arm-portal/figure16.png)

## <a name="next-steps"></a><span data-ttu-id="14ab0-200">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="14ab0-200">Next steps</span></span>
* <span data-ttu-id="14ab0-201">[Naplózás engedélyezése](virtual-network-nsg-manage-log.md) az NSG-ket.</span><span class="sxs-lookup"><span data-stu-id="14ab0-201">[Enable logging](virtual-network-nsg-manage-log.md) for NSGs.</span></span>
