---
title: "A virtuális hálózati átjáró törlése: PowerShell: Azure Resource Manager |} Microsoft Docs"
description: "A PowerShell használatával a Resource Manager üzembe helyezési modellel virtuális hálózati átjáró törlése."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: 
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/20/2017
ms.author: cherylmc
ms.openlocfilehash: 4d0f085423d5bd60b24d88649ee1d77bcd1d009f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="delete-a-virtual-network-gateway-using-powershell"></a><span data-ttu-id="a4330-103">A PowerShell használatával virtuális hálózati átjáró törlése</span><span class="sxs-lookup"><span data-stu-id="a4330-103">Delete a virtual network gateway using PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="a4330-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="a4330-104">Azure portal</span></span>](vpn-gateway-delete-vnet-gateway-portal.md)
> * [<span data-ttu-id="a4330-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="a4330-105">PowerShell</span></span>](vpn-gateway-delete-vnet-gateway-powershell.md)
> * [<span data-ttu-id="a4330-106">PowerShell (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="a4330-106">PowerShell (classic)</span></span>](vpn-gateway-delete-vnet-gateway-classic-powershell.md)
>
>

<span data-ttu-id="a4330-107">Többféle különböző megközelítés közül választhat, ha törli a virtuális hálózati átjáró VPN gateway-konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="a4330-107">There are a couple of different approaches you can take when you want to delete a virtual network gateway for a VPN gateway configuration.</span></span>

- <span data-ttu-id="a4330-108">Ha teljes tartalmának törlése, és kezdje újra a folyamatot, ahogy a gyorsítás esetében is egy tesztkörnyezetben, törölheti az erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="a4330-108">If you want to delete everything and start over, as in the case of a test environment, you can delete the resource group.</span></span> <span data-ttu-id="a4330-109">Ha töröl egy erőforráscsoport, a csoportban lévő összes erőforrást törli.</span><span class="sxs-lookup"><span data-stu-id="a4330-109">When you delete a resource group, it deletes all the resources within the group.</span></span> <span data-ttu-id="a4330-110">Ez a módszer csak akkor javasolt, ha nem szeretné megtartani az erőforrások az erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="a4330-110">This is method is only recommended if you don't want to keep any of the resources in the resource group.</span></span> <span data-ttu-id="a4330-111">Ezzel a megközelítéssel csak néhány erőforrásokat külön-külön nem törölhető.</span><span class="sxs-lookup"><span data-stu-id="a4330-111">You can't selectively delete only a few resources using this approach.</span></span>

- <span data-ttu-id="a4330-112">Ha meg szeretné tartani a erőforrások az erőforráscsoportban, a virtuális hálózati átjáró törlése válik kicsit bonyolultabb.</span><span class="sxs-lookup"><span data-stu-id="a4330-112">If you want to keep some of the resources in your resource group, deleting a virtual network gateway becomes slightly more complicated.</span></span> <span data-ttu-id="a4330-113">A virtuális hálózati átjáró törlése előtt először törölnie kell az átjáró függő erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="a4330-113">Before you can delete the virtual network gateway, you must first delete any resources that are dependent on the gateway.</span></span> <span data-ttu-id="a4330-114">A szükséges lépések attól függ, a létrehozott kapcsolatok és a tőle függő erőforrások, minden egyes kapcsolathoz.</span><span class="sxs-lookup"><span data-stu-id="a4330-114">The steps you follow depend on the type of connections that you created and the dependent resources for each connection.</span></span>

## <a name="before-beginning"></a><span data-ttu-id="a4330-115">Mielőtt hozzálát</span><span class="sxs-lookup"><span data-stu-id="a4330-115">Before beginning</span></span>

### <a name="1-download-the-latest-azure-resource-manager-powershell-cmdlets"></a><span data-ttu-id="a4330-116">1. Töltse le a legújabb Azure Resource Manager PowerShell-parancsmagokkal.</span><span class="sxs-lookup"><span data-stu-id="a4330-116">1. Download the latest Azure Resource Manager PowerShell cmdlets.</span></span>

<span data-ttu-id="a4330-117">Töltse le és telepítse a legújabb verziót az Azure Resource Manager PowerShell-parancsmagokat.</span><span class="sxs-lookup"><span data-stu-id="a4330-117">Download and install the latest version of the Azure Resource Manager PowerShell cmdlets.</span></span> <span data-ttu-id="a4330-118">Letölti és telepíti a PowerShell-parancsmagokkal kapcsolatos további információkért lásd: [telepítése és konfigurálása az Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="a4330-118">For more information about downloading and installing PowerShell cmdlets, see [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

### <a name="2-connect-to-your-azure-account"></a><span data-ttu-id="a4330-119">2. Csatlakozás az Azure-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="a4330-119">2. Connect to your Azure account.</span></span>

<span data-ttu-id="a4330-120">Nyissa meg a PowerShell konzolt, és csatlakozzon a fiókjához.</span><span class="sxs-lookup"><span data-stu-id="a4330-120">Open your PowerShell console and connect to your account.</span></span> <span data-ttu-id="a4330-121">A következő példa segít a kapcsolódásban:</span><span class="sxs-lookup"><span data-stu-id="a4330-121">Use the following example to help you connect:</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="a4330-122">Keresse meg a fiókot az előfizetésekben.</span><span class="sxs-lookup"><span data-stu-id="a4330-122">Check the subscriptions for the account.</span></span>

```powershell
Get-AzureRmSubscription
```

<span data-ttu-id="a4330-123">Ha egynél több előfizetéssel rendelkezik, adja meg a használni kívánt előfizetést.</span><span class="sxs-lookup"><span data-stu-id="a4330-123">If you have more than one subscription, specify the subscription that you want to use.</span></span>

```powershell
Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
```

## <span data-ttu-id="a4330-124"><a name="S2S"></a>Telephelyek közötti VPN-átjáró törlése</span><span class="sxs-lookup"><span data-stu-id="a4330-124"><a name="S2S"></a>Delete a Site-to-Site VPN gateway</span></span>

<span data-ttu-id="a4330-125">Törölje a virtuális hálózati átjáró S2S-konfigurációt, először törölnie kell az egyes erőforrások, amelyek a virtuális hálózati átjáró vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="a4330-125">To delete a virtual network gateway for a S2S configuration, you must first delete each resource that pertains to the virtual network gateway.</span></span> <span data-ttu-id="a4330-126">Erőforrások miatt függőségek meghatározott sorrendben kell törölni.</span><span class="sxs-lookup"><span data-stu-id="a4330-126">Resources must be deleted in a certain order due to dependencies.</span></span> <span data-ttu-id="a4330-127">Az alábbi néhány példa az használatakor az értékeket meg kell adni, miközben más értékek egy kimeneti eredmények.</span><span class="sxs-lookup"><span data-stu-id="a4330-127">When working with the examples below, some of the values must be specified, while other values are an output result.</span></span> <span data-ttu-id="a4330-128">A következő megadott értékek a példákban, bemutatási célokra használjuk:</span><span class="sxs-lookup"><span data-stu-id="a4330-128">We use the following specific values in the examples for demonstration purposes:</span></span>

<span data-ttu-id="a4330-129">VNet-name: VNet1</span><span class="sxs-lookup"><span data-stu-id="a4330-129">VNet name: VNet1</span></span><br>
<span data-ttu-id="a4330-130">Erőforráscsoport neve: RG1</span><span class="sxs-lookup"><span data-stu-id="a4330-130">Resource Group name: RG1</span></span><br>
<span data-ttu-id="a4330-131">Virtuális hálózati átjárójához megadott név: GW1</span><span class="sxs-lookup"><span data-stu-id="a4330-131">Virtual network gateway name: GW1</span></span><br>

<span data-ttu-id="a4330-132">Az alábbi lépéseket a Resource Manager üzembe helyezési modellel vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="a4330-132">The following steps apply to the Resource Manager deployment model.</span></span>

### <a name="1-get-the-virtual-network-gateway-that-you-want-to-delete"></a><span data-ttu-id="a4330-133">1. A virtuális hálózati átjáró, amely a törölni kívánt beolvasása.</span><span class="sxs-lookup"><span data-stu-id="a4330-133">1. Get the virtual network gateway that you want to delete.</span></span>

```powershell
$Gateway=get-azurermvirtualnetworkgateway -Name "GW1" -ResourceGroupName "RG1"
```

### <a name="2-check-to-see-if-the-virtual-network-gateway-has-any-connections"></a><span data-ttu-id="a4330-134">2. Ellenőrizze, hogy a virtuális hálózati átjáró rendelkezik-e a kapcsolatokat.</span><span class="sxs-lookup"><span data-stu-id="a4330-134">2. Check to see if the virtual network gateway has any connections.</span></span>

```powershell
get-azurermvirtualnetworkgatewayconnection -ResourceGroupName "RG1" | where-object {$_.VirtualNetworkGateway1.Id -eq $GW.Id}
$Conns=get-azurermvirtualnetworkgatewayconnection -ResourceGroupName "RG1" | where-object {$_.VirtualNetworkGateway1.Id -eq $GW.Id}
```

### <a name="3-delete-all-connections"></a><span data-ttu-id="a4330-135">3. Törölje az összes kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="a4330-135">3. Delete all connections.</span></span>

<span data-ttu-id="a4330-136">A kapcsolatok mindegyikének a törlés megerősítéséhez kérheti.</span><span class="sxs-lookup"><span data-stu-id="a4330-136">You may be prompted to confirm the deletion of each of the connections.</span></span>

```powershell
$Conns | ForEach-Object {Remove-AzureRmVirtualNetworkGatewayConnection -Name $_.name -ResourceGroupName $_.ResourceGroupName}
```

### <a name="4-delete-the-virtual-network-gateway"></a><span data-ttu-id="a4330-137">4. A virtuális hálózati átjáró törlése.</span><span class="sxs-lookup"><span data-stu-id="a4330-137">4. Delete the virtual network gateway.</span></span>

<span data-ttu-id="a4330-138">Az átjáró a törlés megerősítéséhez kérheti.</span><span class="sxs-lookup"><span data-stu-id="a4330-138">You may be prompted to confirm the deletion of the gateway.</span></span> <span data-ttu-id="a4330-139">Ha egy P2S-konfigurációt a vneten mellett S2S-konfigurációjáról, a virtuális hálózati átjáró törlése folyamatban lesz automatikusan ügyfelek leválasztása a összes P2S figyelmeztetés nélkül.</span><span class="sxs-lookup"><span data-stu-id="a4330-139">If you have a P2S configuration to this VNet in addition to your S2S configuration, deleting the virtual network gateway will automatically disconnect all P2S clients without warning.</span></span>


```powershell
Remove-AzureRmVirtualNetworkGateway -Name "GW1" -ResourceGroupName "RG1"
```

<span data-ttu-id="a4330-140">Ezen a ponton a virtuális hálózati átjáró törölve lett.</span><span class="sxs-lookup"><span data-stu-id="a4330-140">At this point, your virtual network gateway has been deleted.</span></span> <span data-ttu-id="a4330-141">A következő lépések segítségével törölje minden olyan erőforrásnál, amely már nincsenek használatban.</span><span class="sxs-lookup"><span data-stu-id="a4330-141">You can use the next steps to delete any resources that are no longer being used.</span></span>

### <a name="5-delete-the-local-network-gateways"></a><span data-ttu-id="a4330-142">5 helyi hálózati átjáró törlése.</span><span class="sxs-lookup"><span data-stu-id="a4330-142">5 Delete the local network gateways.</span></span>

<span data-ttu-id="a4330-143">A megfelelő helyi hálózati átjáró listájának lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="a4330-143">Get the list of the corresponding local network gateways.</span></span>

```powershell
$LNG=Get-AzureRmLocalNetworkGateway -ResourceGroupName "RG1" | where-object {$_.Id -In $Conns.LocalNetworkGateway2.Id}
```

<span data-ttu-id="a4330-144">Helyi hálózati átjáró törlése.</span><span class="sxs-lookup"><span data-stu-id="a4330-144">Delete the local network gateways.</span></span> <span data-ttu-id="a4330-145">Előfordulhat, hogy kérni fogja a helyi hálózati átjáró mindegyikének a törlés jóváhagyásához.</span><span class="sxs-lookup"><span data-stu-id="a4330-145">You may be prompted to confirm the deletion of each of the local network gateway.</span></span>

```powershell
$LNG | ForEach-Object {Remove-AzureRmLocalNetworkGateway -Name $_.Name -ResourceGroupName $_.ResourceGroupName}
```

### <a name="6-delete-the-public-ip-address-resources"></a><span data-ttu-id="a4330-146">6. A nyilvános IP-cím erőforrás törlése.</span><span class="sxs-lookup"><span data-stu-id="a4330-146">6. Delete the Public IP address resources.</span></span>

<span data-ttu-id="a4330-147">A virtuális hálózati átjáró IP-konfigurációk beolvasása.</span><span class="sxs-lookup"><span data-stu-id="a4330-147">Get the IP configurations of the virtual network gateway.</span></span>

```powershell
$GWIpConfigs = $Gateway.IpConfigurations
```

<span data-ttu-id="a4330-148">A nyilvános IP-cím erőforrások ehhez a virtuális hálózati átjáróhoz használt listájának lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="a4330-148">Get the list of Public IP address resources used for this virtual network gateway.</span></span> <span data-ttu-id="a4330-149">Ha a virtuális hálózati átjáró aktív-aktív volt, két nyilvános IP-cím jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="a4330-149">If the virtual network gateway was active-active, you will see two Public IP addresses.</span></span>

```powershell
$PubIP=Get-AzureRmPublicIpAddress | where-object {$_.Id -In $GWIpConfigs.PublicIpAddress.Id}
```

<span data-ttu-id="a4330-150">A nyilvános IP-erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="a4330-150">Delete the Public IP resources.</span></span>

```powershell
$PubIP | foreach-object {remove-azurermpublicIpAddress -Name $_.Name -ResourceGroupName "RG1"}
```

### <a name="7-delete-the-gateway-subnet-and-set-the-configuration"></a><span data-ttu-id="a4330-151">7. Az átjáró-alhálózat törléséhez és a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="a4330-151">7. Delete the gateway subnet and set the configuration.</span></span>

```powershell
$GWSub = Get-AzureRmVirtualNetwork -ResourceGroupName "RG1" -Name "VNet1" | Remove-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet"
Set-AzureRmVirtualNetwork -VirtualNetwork $GWSub
```

## <span data-ttu-id="a4330-152"><a name="v2v"></a>VNet – VNet VPN-átjáró törlése</span><span class="sxs-lookup"><span data-stu-id="a4330-152"><a name="v2v"></a>Delete a VNet-to-VNet VPN gateway</span></span>

<span data-ttu-id="a4330-153">V2V konfiguráció esetén a virtuális hálózati átjáró törléséhez először törölnie kell az egyes erőforrások, amelyek a virtuális hálózati átjáró vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="a4330-153">To delete a virtual network gateway for a V2V configuration, you must first delete each resource that pertains to the virtual network gateway.</span></span> <span data-ttu-id="a4330-154">Erőforrások miatt függőségek meghatározott sorrendben kell törölni.</span><span class="sxs-lookup"><span data-stu-id="a4330-154">Resources must be deleted in a certain order due to dependencies.</span></span> <span data-ttu-id="a4330-155">Az alábbi néhány példa az használatakor az értékeket meg kell adni, miközben más értékek egy kimeneti eredmények.</span><span class="sxs-lookup"><span data-stu-id="a4330-155">When working with the examples below, some of the values must be specified, while other values are an output result.</span></span> <span data-ttu-id="a4330-156">A következő megadott értékek a példákban, bemutatási célokra használjuk:</span><span class="sxs-lookup"><span data-stu-id="a4330-156">We use the following specific values in the examples for demonstration purposes:</span></span>

<span data-ttu-id="a4330-157">VNet-name: VNet1</span><span class="sxs-lookup"><span data-stu-id="a4330-157">VNet name: VNet1</span></span><br>
<span data-ttu-id="a4330-158">Erőforráscsoport neve: RG1</span><span class="sxs-lookup"><span data-stu-id="a4330-158">Resource Group name: RG1</span></span><br>
<span data-ttu-id="a4330-159">Virtuális hálózati átjárójához megadott név: GW1</span><span class="sxs-lookup"><span data-stu-id="a4330-159">Virtual network gateway name: GW1</span></span><br>

<span data-ttu-id="a4330-160">Az alábbi lépéseket a Resource Manager üzembe helyezési modellel vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="a4330-160">The following steps apply to the Resource Manager deployment model.</span></span>

### <a name="1-get-the-virtual-network-gateway-that-you-want-to-delete"></a><span data-ttu-id="a4330-161">1. A virtuális hálózati átjáró, amely a törölni kívánt beolvasása.</span><span class="sxs-lookup"><span data-stu-id="a4330-161">1. Get the virtual network gateway that you want to delete.</span></span>

```powershell
$Gateway=get-azurermvirtualnetworkgateway -Name "GW1" -ResourceGroupName "RG1"
```

### <a name="2-check-to-see-if-the-virtual-network-gateway-has-any-connections"></a><span data-ttu-id="a4330-162">2. Ellenőrizze, hogy a virtuális hálózati átjáró rendelkezik-e a kapcsolatokat.</span><span class="sxs-lookup"><span data-stu-id="a4330-162">2. Check to see if the virtual network gateway has any connections.</span></span>

```powershell
get-azurermvirtualnetworkgatewayconnection -ResourceGroupName "RG1" | where-object {$_.VirtualNetworkGateway1.Id -eq $GW.Id}
```
 
<span data-ttu-id="a4330-163">Előfordulhat, hogy a virtuális hálózati átjáró egyéb kapcsolatokat, amelyek egy másik erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="a4330-163">There may be other connections to the virtual network gateway that are part of a different resource group.</span></span> <span data-ttu-id="a4330-164">Ellenőrizze, hogy minden további erőforráscsoportban további kapcsolatokat.</span><span class="sxs-lookup"><span data-stu-id="a4330-164">Check for additional connections in each additional resource group.</span></span> <span data-ttu-id="a4330-165">Ebben a példában keresése RG2 érkező kapcsolatok.</span><span class="sxs-lookup"><span data-stu-id="a4330-165">In this example, we are checking for connections from RG2.</span></span> <span data-ttu-id="a4330-166">Futtassa ezt minden erőforráscsoportban, hogy rendelkezik, amely lehet kapcsolódni a virtuális hálózati átjáró.</span><span class="sxs-lookup"><span data-stu-id="a4330-166">Run this for each resource group that you have which may have a connection to the virtual network gateway.</span></span>

```powershell
get-azurermvirtualnetworkgatewayconnection -ResourceGroupName "RG2" | where-object {$_.VirtualNetworkGateway2.Id -eq $GW.Id}
```

### <a name="3-get-the-list-of-connections-in-both-directions"></a><span data-ttu-id="a4330-167">3. A kapcsolatok listájának lekérdezése mindkét irányban.</span><span class="sxs-lookup"><span data-stu-id="a4330-167">3. Get the list of connections in both directions.</span></span>

<span data-ttu-id="a4330-168">Mivel ez egy VNet – VNet konfigurációs, kell mindkét irányban kapcsolatok listáját.</span><span class="sxs-lookup"><span data-stu-id="a4330-168">Because this is a VNet-to-VNet configuration, you need the list of connections in both directions.</span></span>

```powershell
$ConnsL=get-azurermvirtualnetworkgatewayconnection -ResourceGroupName "RG1" | where-object {$_.VirtualNetworkGateway1.Id -eq $GW.Id}
```
 
<span data-ttu-id="a4330-169">Ebben a példában keresése RG2 érkező kapcsolatok.</span><span class="sxs-lookup"><span data-stu-id="a4330-169">In this example, we are checking for connections from RG2.</span></span> <span data-ttu-id="a4330-170">Futtassa ezt minden erőforráscsoportban, hogy rendelkezik, amely lehet kapcsolódni a virtuális hálózati átjáró.</span><span class="sxs-lookup"><span data-stu-id="a4330-170">Run this for each resource group that you have which may have a connection to the virtual network gateway.</span></span>

```powershell
 $ConnsR=get-azurermvirtualnetworkgatewayconnection -ResourceGroupName "<NameOfResourceGroup2>" | where-object {$_.VirtualNetworkGateway2.Id -eq $GW.Id}
 ```

### <a name="4-delete-all-connections"></a><span data-ttu-id="a4330-171">4. Törölje az összes kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="a4330-171">4. Delete all connections.</span></span>

<span data-ttu-id="a4330-172">A kapcsolatok mindegyikének a törlés megerősítéséhez kérheti.</span><span class="sxs-lookup"><span data-stu-id="a4330-172">You may be prompted to confirm the deletion of each of the connections.</span></span>

```powershell
$ConnsL | ForEach-Object {Remove-AzureRmVirtualNetworkGatewayConnection -Name $_.name -ResourceGroupName $_.ResourceGroupName}
$ConnsR | ForEach-Object {Remove-AzureRmVirtualNetworkGatewayConnection -Name $_.name -ResourceGroupName $_.ResourceGroupName}
```

### <a name="5-delete-the-virtual-network-gateway"></a><span data-ttu-id="a4330-173">5. A virtuális hálózati átjáró törlése.</span><span class="sxs-lookup"><span data-stu-id="a4330-173">5. Delete the virtual network gateway.</span></span>

<span data-ttu-id="a4330-174">A virtuális hálózati átjáró törlésének megerősítéséhez kérheti.</span><span class="sxs-lookup"><span data-stu-id="a4330-174">You may be prompted to confirm the deletion of the virtual network gateway.</span></span> <span data-ttu-id="a4330-175">A Vnetek P2S konfigurációk kell a V2V konfigurációjában, ha a virtuális hálózati átjáró törlése lesz automatikusan ügyfelek leválasztása a összes P2S figyelmeztetés nélkül.</span><span class="sxs-lookup"><span data-stu-id="a4330-175">If you have P2S configurations to your VNets in addition to your V2V configuration, deleting the virtual network gateways will automatically disconnect all P2S clients without warning.</span></span>

```powershell
Remove-AzureRmVirtualNetworkGateway -Name "GW1" -ResourceGroupName "RG1"
```

<span data-ttu-id="a4330-176">Ezen a ponton a virtuális hálózati átjáró törölve lett.</span><span class="sxs-lookup"><span data-stu-id="a4330-176">At this point, your virtual network gateway has been deleted.</span></span> <span data-ttu-id="a4330-177">A következő lépések segítségével törölje minden olyan erőforrásnál, amely már nincsenek használatban.</span><span class="sxs-lookup"><span data-stu-id="a4330-177">You can use the next steps to delete any resources that are no longer being used.</span></span>

### <a name="6-delete-the-public-ip-address-resources"></a><span data-ttu-id="a4330-178">6. A nyilvános IP-cím erőforrás törlése</span><span class="sxs-lookup"><span data-stu-id="a4330-178">6. Delete the Public IP address resources</span></span>

<span data-ttu-id="a4330-179">A virtuális hálózati átjáró IP-konfigurációk beolvasása.</span><span class="sxs-lookup"><span data-stu-id="a4330-179">Get the IP configurations of the virtual network gateway.</span></span>

```powershell
$GWIpConfigs = $Gateway.IpConfigurations
```

<span data-ttu-id="a4330-180">A nyilvános IP-cím erőforrások ehhez a virtuális hálózati átjáróhoz használt listájának lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="a4330-180">Get the list of Public IP address resources used for this virtual network gateway.</span></span> <span data-ttu-id="a4330-181">Ha a virtuális hálózati átjáró aktív-aktív volt, két nyilvános IP-cím jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="a4330-181">If the virtual network gateway was active-active, you will see two Public IP addresses.</span></span>

```powershell
$PubIP=Get-AzureRmPublicIpAddress | where-object {$_.Id -In $GWIpConfigs.PublicIpAddress.Id}
```

<span data-ttu-id="a4330-182">A nyilvános IP-erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="a4330-182">Delete the Public IP resources.</span></span> <span data-ttu-id="a4330-183">A nyilvános IP-cím törlésének megerősítéséhez kérheti.</span><span class="sxs-lookup"><span data-stu-id="a4330-183">You may be prompted to confirm the deletion of the Public IP.</span></span>

```powershell
$PubIP | foreach-object {remove-azurermpublicIpAddress -Name $_.Name -ResourceGroupName "<NameOfResourceGroup1>"}
```

### <a name="7-delete-the-gateway-subnet-and-set-the-configuration"></a><span data-ttu-id="a4330-184">7. Az átjáró-alhálózat törléséhez és a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="a4330-184">7. Delete the gateway subnet and set the configuration.</span></span>

```powershell
$GWSub = Get-AzureRmVirtualNetwork -ResourceGroupName "RG1" -Name "VNet1" | Remove-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet"
Set-AzureRmVirtualNetwork -VirtualNetwork $GWSub
```

## <span data-ttu-id="a4330-185"><a name="deletep2s"></a>Egy pont – hely típusú VPN-átjáró törlése</span><span class="sxs-lookup"><span data-stu-id="a4330-185"><a name="deletep2s"></a>Delete a Point-to-Site VPN gateway</span></span>

<span data-ttu-id="a4330-186">Törölje a virtuális hálózati átjáró P2S-konfigurációt, először törölnie kell az egyes erőforrások, amelyek a virtuális hálózati átjáró vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="a4330-186">To delete a virtual network gateway for a P2S configuration, you must first delete each resource that pertains to the virtual network gateway.</span></span> <span data-ttu-id="a4330-187">Erőforrások miatt függőségek meghatározott sorrendben kell törölni.</span><span class="sxs-lookup"><span data-stu-id="a4330-187">Resources must be deleted in a certain order due to dependencies.</span></span> <span data-ttu-id="a4330-188">Az alábbi néhány példa az használatakor az értékeket meg kell adni, miközben más értékek egy kimeneti eredmények.</span><span class="sxs-lookup"><span data-stu-id="a4330-188">When working with the examples below, some of the values must be specified, while other values are an output result.</span></span> <span data-ttu-id="a4330-189">A következő megadott értékek a példákban, bemutatási célokra használjuk:</span><span class="sxs-lookup"><span data-stu-id="a4330-189">We use the following specific values in the examples for demonstration purposes:</span></span>

<span data-ttu-id="a4330-190">VNet-name: VNet1</span><span class="sxs-lookup"><span data-stu-id="a4330-190">VNet name: VNet1</span></span><br>
<span data-ttu-id="a4330-191">Erőforráscsoport neve: RG1</span><span class="sxs-lookup"><span data-stu-id="a4330-191">Resource Group name: RG1</span></span><br>
<span data-ttu-id="a4330-192">Virtuális hálózati átjárójához megadott név: GW1</span><span class="sxs-lookup"><span data-stu-id="a4330-192">Virtual network gateway name: GW1</span></span><br>

<span data-ttu-id="a4330-193">Az alábbi lépéseket a Resource Manager üzembe helyezési modellel vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="a4330-193">The following steps apply to the Resource Manager deployment model.</span></span>


>[!NOTE]
> <span data-ttu-id="a4330-194">Ha törli a VPN-átjárót, az összes kapcsolódó ügyfelek a VNet figyelmeztetés nélkül megszakad.</span><span class="sxs-lookup"><span data-stu-id="a4330-194">When you delete the VPN gateway, all connected clients will be disconnected from the VNet without warning.</span></span>
>
>

### <a name="1-get-the-virtual-network-gateway-that-you-want-to-delete"></a><span data-ttu-id="a4330-195">1. A virtuális hálózati átjáró, amely a törölni kívánt beolvasása.</span><span class="sxs-lookup"><span data-stu-id="a4330-195">1. Get the virtual network gateway that you want to delete.</span></span>

```powershell
$Gateway=get-azurermvirtualnetworkgateway -Name "GW1" -ResourceGroupName "RG1"
```

### <a name="2-delete-the-virtual-network-gateway"></a><span data-ttu-id="a4330-196">2. A virtuális hálózati átjáró törlése.</span><span class="sxs-lookup"><span data-stu-id="a4330-196">2. Delete the virtual network gateway.</span></span>

<span data-ttu-id="a4330-197">A virtuális hálózati átjáró törlésének megerősítéséhez kérheti.</span><span class="sxs-lookup"><span data-stu-id="a4330-197">You may be prompted to confirm the deletion of the virtual network gateway.</span></span>

```powershell
Remove-AzureRmVirtualNetworkGateway -Name "GW1" -ResourceGroupName "RG1"
```

<span data-ttu-id="a4330-198">Ezen a ponton a virtuális hálózati átjáró törölve lett.</span><span class="sxs-lookup"><span data-stu-id="a4330-198">At this point, your virtual network gateway has been deleted.</span></span> <span data-ttu-id="a4330-199">A következő lépések segítségével törölje minden olyan erőforrásnál, amely már nincsenek használatban.</span><span class="sxs-lookup"><span data-stu-id="a4330-199">You can use the next steps to delete any resources that are no longer being used.</span></span>

### <a name="3-delete-the-public-ip-address-resources"></a><span data-ttu-id="a4330-200">3. A nyilvános IP-cím erőforrás törlése</span><span class="sxs-lookup"><span data-stu-id="a4330-200">3. Delete the Public IP address resources</span></span>

<span data-ttu-id="a4330-201">A virtuális hálózati átjáró IP-konfigurációk beolvasása.</span><span class="sxs-lookup"><span data-stu-id="a4330-201">Get the IP configurations of the virtual network gateway.</span></span>

```powershell
$GWIpConfigs = $Gateway.IpConfigurations
```

<span data-ttu-id="a4330-202">Használja a virtuális hálózati átjáró nyilvános IP-címek listájának.</span><span class="sxs-lookup"><span data-stu-id="a4330-202">Get the list of Public IP addresses used for this virtual network gateway.</span></span> <span data-ttu-id="a4330-203">Ha a virtuális hálózati átjáró aktív-aktív volt, két nyilvános IP-cím jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="a4330-203">If the virtual network gateway was active-active, you will see two Public IP addresses.</span></span>

```powershell
$PubIP=Get-AzureRmPublicIpAddress | where-object {$_.Id -In $GWIpConfigs.PublicIpAddress.Id}
```

<span data-ttu-id="a4330-204">A nyilvános IP-cím törlése.</span><span class="sxs-lookup"><span data-stu-id="a4330-204">Delete the Public IPs.</span></span> <span data-ttu-id="a4330-205">A nyilvános IP-cím törlésének megerősítéséhez kérheti.</span><span class="sxs-lookup"><span data-stu-id="a4330-205">You may be prompted to confirm the deletion of the Public IP.</span></span>

```powershell
$PubIP | foreach-object {remove-azurermpublicIpAddress -Name $_.Name -ResourceGroupName "<NameOfResourceGroup1>"}
```

### <a name="4-delete-the-gateway-subnet-and-set-the-configuration"></a><span data-ttu-id="a4330-206">4. Az átjáró-alhálózat törléséhez és a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="a4330-206">4. Delete the gateway subnet and set the configuration.</span></span>

```powershell
$GWSub = Get-AzureRmVirtualNetwork -ResourceGroupName "RG1" -Name "VNet1" | Remove-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet"
Set-AzureRmVirtualNetwork -VirtualNetwork $GWSub
```

## <span data-ttu-id="a4330-207"><a name="delete"></a>Az erőforráscsoport törlésével VPN-átjáró törlése</span><span class="sxs-lookup"><span data-stu-id="a4330-207"><a name="delete"></a>Delete a VPN gateway by deleting the resource group</span></span>

<span data-ttu-id="a4330-208">Ha nem aggódik megőrzi az erőforrásokat az erőforráscsoport található, és szeretné elölről, törölheti a teljes erőforráscsoport.</span><span class="sxs-lookup"><span data-stu-id="a4330-208">If you are not concerned about keeping any of your resources in the resource group and you just want to start over, you can delete an entire resource group.</span></span> <span data-ttu-id="a4330-209">Ez az gyorsan távolítson el minden.</span><span class="sxs-lookup"><span data-stu-id="a4330-209">This is a quick way to remove everything.</span></span> <span data-ttu-id="a4330-210">Az alábbi lépéseket csak a Resource Manager üzembe helyezési modellel vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="a4330-210">The following steps apply only to the Resource Manager deployment model.</span></span>

### <a name="1-get-a-list-of-all-the-resource-groups-in-your-subscription"></a><span data-ttu-id="a4330-211">1. Az erőforráscsoportok listájának lekérése az előfizetéséhez.</span><span class="sxs-lookup"><span data-stu-id="a4330-211">1. Get a list of all the resource groups in your subscription.</span></span>

```powershell
Get-AzureRmResourceGroup
```

### <a name="2-locate-the-resource-group-that-you-want-to-delete"></a><span data-ttu-id="a4330-212">2. Keresse meg a törölni kívánt erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="a4330-212">2. Locate the resource group that you want to delete.</span></span>

<span data-ttu-id="a4330-213">Keresse meg az erőforráscsoport, amelyet szeretne törölni, és az erőforráscsoport erőforrások listájának megtekintése.</span><span class="sxs-lookup"><span data-stu-id="a4330-213">Locate the resource group that you want to delete and view the list of resources in that resource group.</span></span> <span data-ttu-id="a4330-214">A példában az erőforráscsoport neve nem RG1.</span><span class="sxs-lookup"><span data-stu-id="a4330-214">In the example, the name of the resource group is RG1.</span></span> <span data-ttu-id="a4330-215">Módosítsa a példa az erőforrások listájának beolvasása.</span><span class="sxs-lookup"><span data-stu-id="a4330-215">Modify the example to retrieve a list of all the resources.</span></span>

```powershell
Find-AzureRmResource -ResourceGroupNameContains RG1
```

### <a name="3-verify-the-resources-in-the-list"></a><span data-ttu-id="a4330-216">3. Ellenőrizze az erőforrások listájában.</span><span class="sxs-lookup"><span data-stu-id="a4330-216">3. Verify the resources in the list.</span></span>

<span data-ttu-id="a4330-217">Ha a listát ad vissza, át, és győződjön meg arról, hogy valóban törli az erőforráscsoportot, valamint az erőforráscsoportot, maga az összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="a4330-217">When the list is returned, review it to verify that you want to delete all the resources in the resource group, as well as the resource group itself.</span></span> <span data-ttu-id="a4330-218">Ha meg szeretné tartani a erőforrások az erőforráscsoportban, a lépések segítségével a cikk korábbi szakaszaiban ismertetett törölje az átjárót.</span><span class="sxs-lookup"><span data-stu-id="a4330-218">If you want to keep some of the resources in the resource group, use the steps in the earlier sections of this article to delete your gateway.</span></span>

### <a name="4-delete-the-resource-group-and-resources"></a><span data-ttu-id="a4330-219">4. Az erőforráscsoport és erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="a4330-219">4. Delete the resource group and resources.</span></span>

<span data-ttu-id="a4330-220">Az erőforráscsoport és az erőforrás-csoportban lévő összes erőforrás törlése, módosítása a példa, és futtassa.</span><span class="sxs-lookup"><span data-stu-id="a4330-220">To delete the resource group and all the resource contained in the resource group, modify the example and run.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name RG1
```

### <a name="5-check-the-status"></a><span data-ttu-id="a4330-221">5. Ellenőrizze az állapotát.</span><span class="sxs-lookup"><span data-stu-id="a4330-221">5. Check the status.</span></span>

<span data-ttu-id="a4330-222">Az Azure-erőforrások törlése némi időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="a4330-222">It takes some time for Azure to delete all the resources.</span></span> <span data-ttu-id="a4330-223">Ez a parancsmag használatával ellenőrizheti az erőforráscsoport állapota.</span><span class="sxs-lookup"><span data-stu-id="a4330-223">You can check the status of your resource group by using this cmdlet.</span></span>

```powershell
Get-AzureRmResourceGroup -ResourceGroupName RG1
```

<span data-ttu-id="a4330-224">A visszaadott eredmény jeleníti meg a "Sikeres".</span><span class="sxs-lookup"><span data-stu-id="a4330-224">The result that is returned shows 'Succeeded'.</span></span>

```
ResourceGroupName : RG1
Location          : eastus
ProvisioningState : Succeeded
```