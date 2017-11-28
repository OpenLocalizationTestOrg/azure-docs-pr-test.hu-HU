---
title: "Virtuális hálózat csatolása ExpressRoute-kapcsolatcsoportot: PowerShell: Azure |} Microsoft Docs"
description: "Ez a dokumentum áttekintést csatolása a virtuális hálózatokon (Vnetek) az ExpressRoute-Kapcsolatcsoportok a Resource Manager üzembe helyezési modellben és a PowerShell használatával."
services: expressroute
documentationcenter: na
author: ganesr
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: daacb6e5-705a-456f-9a03-c4fc3f8c1f7e
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/05/2017
ms.author: ganesr
ms.openlocfilehash: 8c2f3036f754a98090ab860f95900416690ebf83
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="connect-a-virtual-network-to-an-expressroute-circuit"></a><span data-ttu-id="0071c-103">Csatlakozás a virtuális hálózati ExpressRoute-kapcsolatcsoportot</span><span class="sxs-lookup"><span data-stu-id="0071c-103">Connect a virtual network to an ExpressRoute circuit</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="0071c-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="0071c-104">Azure portal</span></span>](expressroute-howto-linkvnet-portal-resource-manager.md)
> * [<span data-ttu-id="0071c-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="0071c-105">PowerShell</span></span>](expressroute-howto-linkvnet-arm.md)
> * [<span data-ttu-id="0071c-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="0071c-106">Azure CLI</span></span>](howto-linkvnet-cli.md)
> * [<span data-ttu-id="0071c-107">Video - Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="0071c-107">Video - Azure portal</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit)
> * [<span data-ttu-id="0071c-108">PowerShell (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="0071c-108">PowerShell (classic)</span></span>](expressroute-howto-linkvnet-classic.md)
>

<span data-ttu-id="0071c-109">Ez a cikk segít virtuális hálózatokról (Vnetekről) hivatkozásra az Azure ExpressRoute-Kapcsolatcsoportok a Resource Manager üzembe helyezési modellben és a PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="0071c-109">This article helps you link virtual networks (VNets) to Azure ExpressRoute circuits by using the Resource Manager deployment model and PowerShell.</span></span> <span data-ttu-id="0071c-110">Virtuális hálózatok a ugyanahhoz az előfizetéshez vagy egy másik előfizetés része lehet.</span><span class="sxs-lookup"><span data-stu-id="0071c-110">Virtual networks can either be in the same subscription or part of another subscription.</span></span> <span data-ttu-id="0071c-111">Ez a cikk is bemutatja, hogyan frissítse a virtuális hálózati kapcsolaton.</span><span class="sxs-lookup"><span data-stu-id="0071c-111">This article also shows you how to update a virtual network link.</span></span> 

## <a name="before-you-begin"></a><span data-ttu-id="0071c-112">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="0071c-112">Before you begin</span></span>
* <span data-ttu-id="0071c-113">Telepítse a legújabb verzióját az Azure PowerShell-modulok.</span><span class="sxs-lookup"><span data-stu-id="0071c-113">Install the latest version of the Azure PowerShell modules.</span></span> <span data-ttu-id="0071c-114">További információt [az Azure PowerShell telepítésével és konfigurálásával](/powershell/azure/overview) foglalkozó témakörben talál.</span><span class="sxs-lookup"><span data-stu-id="0071c-114">For more information, see [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>
* <span data-ttu-id="0071c-115">Tekintse át a [Előfeltételek](expressroute-prerequisites.md), [útválasztási követelmények](expressroute-routing.md), és [munkafolyamatok](expressroute-workflows.md) konfigurálás elkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="0071c-115">Review the [prerequisites](expressroute-prerequisites.md), [routing requirements](expressroute-routing.md), and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>
* <span data-ttu-id="0071c-116">Egy aktív ExpressRoute-kapcsolatcsoportra lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="0071c-116">You must have an active ExpressRoute circuit.</span></span> 
  * <span data-ttu-id="0071c-117">Kövesse az utasításokat [ExpressRoute-kapcsolatcsoportot létrehozni](expressroute-howto-circuit-arm.md) és a kapcsolatcsoport szerint a kapcsolat szolgáltatójánál engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="0071c-117">Follow the instructions to [create an ExpressRoute circuit](expressroute-howto-circuit-arm.md) and have the circuit enabled by your connectivity provider.</span></span> 
  * <span data-ttu-id="0071c-118">Győződjön meg arról, hogy rendelkezik az Azure magánhálózati társviszony-létesítés a kapcsolatcsoport konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="0071c-118">Ensure that you have Azure private peering configured for your circuit.</span></span> <span data-ttu-id="0071c-119">Tekintse meg a [konfigurálja az útválasztást](expressroute-howto-routing-arm.md) útválasztási miként.</span><span class="sxs-lookup"><span data-stu-id="0071c-119">See the [configure routing](expressroute-howto-routing-arm.md) article for routing instructions.</span></span> 
  * <span data-ttu-id="0071c-120">Győződjön meg arról, hogy az Azure magánhálózati társviszony-létesítés konfigurálva legyen, és a BGP társviszony-létesítés a hálózat és a Microsoft között működik-e, hogy engedélyezheti a végpontok közötti kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="0071c-120">Ensure that Azure private peering is configured and the BGP peering between your network and Microsoft is up so that you can enable end-to-end connectivity.</span></span>
  * <span data-ttu-id="0071c-121">Győződjön meg arról, hogy a virtuális hálózat és a virtuális hálózati átjáró létrehozása, és teljesen kiépítve.</span><span class="sxs-lookup"><span data-stu-id="0071c-121">Ensure that you have a virtual network and a virtual network gateway created and fully provisioned.</span></span> <span data-ttu-id="0071c-122">Kövesse az utasításokat [hozzon létre egy virtuális hálózati átjáró ExpressRoute](expressroute-howto-add-gateway-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="0071c-122">Follow the instructions to [create a virtual network gateway for ExpressRoute](expressroute-howto-add-gateway-resource-manager.md).</span></span> <span data-ttu-id="0071c-123">A virtuális hálózati átjáró ExpressRoute az "ExpressRoute", nem VPN GatewayType használja.</span><span class="sxs-lookup"><span data-stu-id="0071c-123">A virtual network gateway for ExpressRoute uses the GatewayType 'ExpressRoute', not VPN.</span></span>

* <span data-ttu-id="0071c-124">Legfeljebb 10 virtuális hálózatok hozzákapcsolhatja egy szabványos ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="0071c-124">You can link up to 10 virtual networks to a standard ExpressRoute circuit.</span></span> <span data-ttu-id="0071c-125">Az összes virtuális hálózatot geopolitikai ugyanabban a régióban kell lennie, egy szabványos ExpressRoute-kapcsolatcsoportot használatakor.</span><span class="sxs-lookup"><span data-stu-id="0071c-125">All virtual networks must be in the same geopolitical region when using a standard ExpressRoute circuit.</span></span> 

* <span data-ttu-id="0071c-126">A virtuális hálózat kívül az ExpressRoute-kapcsolatcsoport geopolitikai régiója összekapcsolása, vagy virtuális hálózatok nagyobb számú csatlakozni az ExpressRoute-kapcsolatcsoportot, ha engedélyezte a prémium szintű ExpressRoute-bővítmény.</span><span class="sxs-lookup"><span data-stu-id="0071c-126">You can link a virtual networks outside of the geopolitical region of the ExpressRoute circuit, or connect a larger number of virtual networks to your ExpressRoute circuit if you enabled the ExpressRoute premium add-on.</span></span> <span data-ttu-id="0071c-127">Ellenőrizze a [gyakran ismételt kérdések](expressroute-faqs.md) a prémium szintű bővítmény olvashat.</span><span class="sxs-lookup"><span data-stu-id="0071c-127">Check the [FAQ](expressroute-faqs.md) for more details on the premium add-on.</span></span>


## <a name="connect-a-virtual-network-in-the-same-subscription-to-a-circuit"></a><span data-ttu-id="0071c-128">A virtuális hálózati ugyanahhoz az előfizetéshez csatlakozni expressroute-kapcsolatcsoporthoz</span><span class="sxs-lookup"><span data-stu-id="0071c-128">Connect a virtual network in the same subscription to a circuit</span></span>
<span data-ttu-id="0071c-129">A következő parancsmag használatával ExpressRoute-kapcsolatcsoportot virtuális hálózati átjáró képes kapcsolódni.</span><span class="sxs-lookup"><span data-stu-id="0071c-129">You can connect a virtual network gateway to an ExpressRoute circuit by using the following cmdlet.</span></span> <span data-ttu-id="0071c-130">Győződjön meg arról, hogy a virtuális hálózati átjáró jön létre, és készen áll a csatolás a parancsmag futtatása előtt:</span><span class="sxs-lookup"><span data-stu-id="0071c-130">Make sure that the virtual network gateway is created and is ready for linking before you run the cmdlet:</span></span>

```powershell
$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
$gw = Get-AzureRmVirtualNetworkGateway -Name "ExpressRouteGw" -ResourceGroupName "MyRG"
$connection = New-AzureRmVirtualNetworkGatewayConnection -Name "ERConnection" -ResourceGroupName "MyRG" -Location "East US" -VirtualNetworkGateway1 $gw -PeerId $circuit.Id -ConnectionType ExpressRoute
```

## <a name="connect-a-virtual-network-in-a-different-subscription-to-a-circuit"></a><span data-ttu-id="0071c-131">Egy másik előfizetéshez tartozó virtuális hálózat bevonása egy kapcsolatcsoportba</span><span class="sxs-lookup"><span data-stu-id="0071c-131">Connect a virtual network in a different subscription to a circuit</span></span>
<span data-ttu-id="0071c-132">Több előfizetés ExpressRoute-kapcsolatcsoportot lehet megosztani.</span><span class="sxs-lookup"><span data-stu-id="0071c-132">You can share an ExpressRoute circuit across multiple subscriptions.</span></span> <span data-ttu-id="0071c-133">Az alábbi ábrán egy egyszerű sematikus ExpressRoute-Kapcsolatcsoportok megosztási hogyan működik a több előfizetésekhez.</span><span class="sxs-lookup"><span data-stu-id="0071c-133">The following figure shows a simple schematic of how sharing works for ExpressRoute circuits across multiple subscriptions.</span></span>

<span data-ttu-id="0071c-134">A nagy felhőben kisebb felhők egyes szervezet különböző részlegei tartozó előfizetések megjelenítésére szolgál.</span><span class="sxs-lookup"><span data-stu-id="0071c-134">Each of the smaller clouds within the large cloud is used to represent subscriptions that belong to different departments within an organization.</span></span> <span data-ttu-id="0071c-135">A saját előfizetés üzembe helyezéséhez, a szolgáltatások –, de használhatja a szervezeten belüli osztályok mindegyikének oszthatnak meg csatlakozni a helyi hálózat a egyetlen ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="0071c-135">Each of the departments within the organization can use their own subscription for deploying their services--but they can share a single ExpressRoute circuit to connect back to your on-premises network.</span></span> <span data-ttu-id="0071c-136">Egy részleghez (ebben a példában: informatikai) az ExpressRoute-kapcsolatcsoport rendelkezhet.</span><span class="sxs-lookup"><span data-stu-id="0071c-136">A single department (in this example: IT) can own the ExpressRoute circuit.</span></span> <span data-ttu-id="0071c-137">A szervezeten belüli más előfizetések használhatja az ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="0071c-137">Other subscriptions within the organization can use the ExpressRoute circuit.</span></span>

> [!NOTE]
> <span data-ttu-id="0071c-138">Kapcsolat és a sávszélesség költségekkel az ExpressRoute-kapcsolatcsoport alkalmazandó az előfizetés tulajdonosa.</span><span class="sxs-lookup"><span data-stu-id="0071c-138">Connectivity and bandwidth charges for the ExpressRoute circuit will be applied to the subscription owner.</span></span> <span data-ttu-id="0071c-139">Az összes virtuális hálózatot megosztani a azonos sávszélességet.</span><span class="sxs-lookup"><span data-stu-id="0071c-139">All virtual networks share the same bandwidth.</span></span>
> 
> 

![Az előfizetések közötti kapcsolathoz](./media/expressroute-howto-linkvnet-classic/cross-subscription.png)


### <a name="administration---circuit-owners-and-circuit-users"></a><span data-ttu-id="0071c-141">Felügyeleti - áramkör tulajdonosa és a kapcsolatcsoport felhasználók</span><span class="sxs-lookup"><span data-stu-id="0071c-141">Administration - circuit owners and circuit users</span></span>

<span data-ttu-id="0071c-142">A kapcsolatcsoport tulajdonosát a ExpressRoute-kapcsolatcsoport erőforrás Power jogosultsága.</span><span class="sxs-lookup"><span data-stu-id="0071c-142">The 'circuit owner' is an authorized Power User of the ExpressRoute circuit resource.</span></span> <span data-ttu-id="0071c-143">A kapcsolatcsoport tulajdonosát hozhat létre, amely a "kör felhasználók" is váltható engedélyek.</span><span class="sxs-lookup"><span data-stu-id="0071c-143">The circuit owner can create authorizations that can be redeemed by 'circuit users'.</span></span> <span data-ttu-id="0071c-144">Kör felhasználók, amelyek nem tartoznak a ExpressRoute-kapcsolatcsoportot tárolóként ugyanazt az előfizetést virtuális hálózati átjárók tulajdonosai.</span><span class="sxs-lookup"><span data-stu-id="0071c-144">Circuit users are owners of virtual network gateways that are not within the same subscription as the ExpressRoute circuit.</span></span> <span data-ttu-id="0071c-145">Kör felhasználók is beválthatja engedélyek (virtuális hálózatonként egy engedélyezési).</span><span class="sxs-lookup"><span data-stu-id="0071c-145">Circuit users can redeem authorizations (one authorization per virtual network).</span></span>

<span data-ttu-id="0071c-146">A kapcsolatcsoport tulajdonosát a rendelkezik módosítja, és bármikor engedélyek visszavonása.</span><span class="sxs-lookup"><span data-stu-id="0071c-146">The circuit owner has the power to modify and revoke authorizations at any time.</span></span> <span data-ttu-id="0071c-147">Visszavonni egy engedélyezési eredményezi, az összes hivatkozás kapcsolatok törlődnek az előfizetésből, amelyek hozzáférését visszavonták.</span><span class="sxs-lookup"><span data-stu-id="0071c-147">Revoking an authorization results in all link connections being deleted from the subscription whose access was revoked.</span></span>

### <a name="circuit-owner-operations"></a><span data-ttu-id="0071c-148">Kapcsolatcsoport-tulajdonos műveletek</span><span class="sxs-lookup"><span data-stu-id="0071c-148">Circuit owner operations</span></span>

<span data-ttu-id="0071c-149">**Az engedély létrehozása**</span><span class="sxs-lookup"><span data-stu-id="0071c-149">**To create an authorization**</span></span>

<span data-ttu-id="0071c-150">A kapcsolatcsoport tulajdonosát engedélyezési hoz létre.</span><span class="sxs-lookup"><span data-stu-id="0071c-150">The circuit owner creates an authorization.</span></span> <span data-ttu-id="0071c-151">Az eredmény, használhatja a kapcsolatcsoport felhasználó való csatlakozáshoz a virtuális hálózati átjárók az ExpressRoute-kapcsolatcsoport engedélykulcs létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="0071c-151">This results in the creation of an authorization key that can be used by a circuit user to connect their virtual network gateways to the ExpressRoute circuit.</span></span> <span data-ttu-id="0071c-152">Az engedély csak egy kapcsolat érvénytelen.</span><span class="sxs-lookup"><span data-stu-id="0071c-152">An authorization is valid for only one connection.</span></span>

<span data-ttu-id="0071c-153">A következő parancsmag kódrészletben láthatja, az engedély létrehozása:</span><span class="sxs-lookup"><span data-stu-id="0071c-153">The following cmdlet snippet shows how to create an authorization:</span></span>

```powershell
$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
Add-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit -Name "MyAuthorization1"
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $circuit

$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
$auth1 = Get-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit -Name "MyAuthorization1"
```


<span data-ttu-id="0071c-154">Ez a válasz a hitelesítési kulcs és az állapot fogja tartalmazni:</span><span class="sxs-lookup"><span data-stu-id="0071c-154">The response to this will contain the authorization key and status:</span></span>

    Name                   : MyAuthorization1
    Id                     : /subscriptions/&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&/resourceGroups/ERCrossSubTestRG/providers/Microsoft.Network/expressRouteCircuits/CrossSubTest/authorizations/MyAuthorization1
    Etag                   : &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&& 
    AuthorizationKey       : ####################################
    AuthorizationUseStatus : Available
    ProvisioningState      : Succeeded



<span data-ttu-id="0071c-155">**Engedélyek áttekintése**</span><span class="sxs-lookup"><span data-stu-id="0071c-155">**To review authorizations**</span></span>

<span data-ttu-id="0071c-156">A kapcsolatcsoport tulajdonosát tekintse át a következő parancsmag futtatásával egy adott körön kiállított összes engedélyek:</span><span class="sxs-lookup"><span data-stu-id="0071c-156">The circuit owner can review all authorizations that are issued on a particular circuit by running the following cmdlet:</span></span>

```powershell
$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
$authorizations = Get-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit
```

<span data-ttu-id="0071c-157">**Engedélyek hozzáadása**</span><span class="sxs-lookup"><span data-stu-id="0071c-157">**To add authorizations**</span></span>

<span data-ttu-id="0071c-158">A kapcsolatcsoport tulajdonosát a következő parancsmag használatával adhat hozzá engedélyek:</span><span class="sxs-lookup"><span data-stu-id="0071c-158">The circuit owner can add authorizations by using the following cmdlet:</span></span>

```powershell
$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
Add-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit -Name "MyAuthorization2"
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $circuit

$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
$authorizations = Get-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit
```

<span data-ttu-id="0071c-159">**Engedélyek törlése**</span><span class="sxs-lookup"><span data-stu-id="0071c-159">**To delete authorizations**</span></span>

<span data-ttu-id="0071c-160">A kapcsolatcsoport tulajdonosát is revoke vagy törlése a felhasználó engedélyek a következő parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="0071c-160">The circuit owner can revoke/delete authorizations to the user by running the following cmdlet:</span></span>

```powershell
Remove-AzureRmExpressRouteCircuitAuthorization -Name "MyAuthorization2" -ExpressRouteCircuit $circuit
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $circuit
```    

### <a name="circuit-user-operations"></a><span data-ttu-id="0071c-161">Kör felhasználói műveletek</span><span class="sxs-lookup"><span data-stu-id="0071c-161">Circuit user operations</span></span>

<span data-ttu-id="0071c-162">A kapcsolatcsoport-felhasználó a társ-Azonosítóval és a engedélykulcs a kapcsolatcsoport tulajdonosát a kell.</span><span class="sxs-lookup"><span data-stu-id="0071c-162">The circuit user needs the peer ID and an authorization key from the circuit owner.</span></span> <span data-ttu-id="0071c-163">A hitelesítési kulcs egy GUID Azonosítót.</span><span class="sxs-lookup"><span data-stu-id="0071c-163">The authorization key is a GUID.</span></span>

<span data-ttu-id="0071c-164">Társ azonosítója a következő paranccsal ellenőrizhetők:</span><span class="sxs-lookup"><span data-stu-id="0071c-164">Peer ID can be checked from the following command:</span></span>

```powershell
Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
```

<span data-ttu-id="0071c-165">**Egy kapcsolat hitelesítési beváltani**</span><span class="sxs-lookup"><span data-stu-id="0071c-165">**To redeem a connection authorization**</span></span>

<span data-ttu-id="0071c-166">A kapcsolatcsoport felhasználó futtathatja egy hivatkozás hitelesítési beváltani a következő parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="0071c-166">The circuit user can run the following cmdlet to redeem a link authorization:</span></span>

```powershell
$id = "/subscriptions/********************************/resourceGroups/ERCrossSubTestRG/providers/Microsoft.Network/expressRouteCircuits/MyCircuit"    
$gw = Get-AzureRmVirtualNetworkGateway -Name "ExpressRouteGw" -ResourceGroupName "MyRG"
$connection = New-AzureRmVirtualNetworkGatewayConnection -Name "ERConnection" -ResourceGroupName "RemoteResourceGroup" -Location "East US" -VirtualNetworkGateway1 $gw -PeerId $id -ConnectionType ExpressRoute -AuthorizationKey "^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^"
```

<span data-ttu-id="0071c-167">**Egy kapcsolat hitelesítési felszabadítása**</span><span class="sxs-lookup"><span data-stu-id="0071c-167">**To release a connection authorization**</span></span>

<span data-ttu-id="0071c-168">Törölni kell a kapcsolat az ExpressRoute-kapcsolatcsoport a virtuális hálózatra hivatkozó engedélyezési is megjelenhetnek.</span><span class="sxs-lookup"><span data-stu-id="0071c-168">You can release an authorization by deleting the connection that links the ExpressRoute circuit to the virtual network.</span></span>

## <a name="modify-a-virtual-network-connection"></a><span data-ttu-id="0071c-169">Módosítsa a virtuális hálózati kapcsolat</span><span class="sxs-lookup"><span data-stu-id="0071c-169">Modify a virtual network connection</span></span>
<span data-ttu-id="0071c-170">Frissítheti az egyes tulajdonságait a virtuális hálózati kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="0071c-170">You can update certain properties of a virtual network connection.</span></span> 

<span data-ttu-id="0071c-171">**A kapcsolat súly frissítése**</span><span class="sxs-lookup"><span data-stu-id="0071c-171">**To update the connection weight**</span></span>

<span data-ttu-id="0071c-172">A virtuális hálózat több ExpressRoute-Kapcsolatcsoportok lehet csatlakoztatni.</span><span class="sxs-lookup"><span data-stu-id="0071c-172">Your virtual network can be connected to multiple ExpressRoute circuits.</span></span> <span data-ttu-id="0071c-173">Egynél több ExpressRoute-kapcsolatcsoportot ugyanazon előtaggal kaphat.</span><span class="sxs-lookup"><span data-stu-id="0071c-173">You may receive the same prefix from more than one ExpressRoute circuit.</span></span> <span data-ttu-id="0071c-174">Az előtag szánt forgalom küldése mely kapcsolat módosíthatja *routingweight értékének* kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="0071c-174">To choose which connection to send traffic destined for this prefix, you can change *RoutingWeight* of a connection.</span></span> <span data-ttu-id="0071c-175">Forgalom küldi a kapcsolatot a legmagasabb *routingweight értékének*.</span><span class="sxs-lookup"><span data-stu-id="0071c-175">Traffic will be sent on the connection with the highest *RoutingWeight*.</span></span>

```powershell
$connection = Get-AzureRmVirtualNetworkGatewayConnection -Name "MyVirtualNetworkConnection" -ResourceGroupName "MyRG"
$connection.RoutingWeight = 100
Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection
```

<span data-ttu-id="0071c-176">A számos *routingweight értékének* mely 0 és 32 000.</span><span class="sxs-lookup"><span data-stu-id="0071c-176">The range of *RoutingWeight* is 0 to 32000.</span></span> <span data-ttu-id="0071c-177">Az alapértelmezett értéke 0.</span><span class="sxs-lookup"><span data-stu-id="0071c-177">The default value is 0.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0071c-178">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0071c-178">Next steps</span></span>
<span data-ttu-id="0071c-179">További információ az ExpressRoute-tal kapcsolatban: [ExpressRoute – Gyakori kérdések](expressroute-faqs.md).</span><span class="sxs-lookup"><span data-stu-id="0071c-179">For more information about ExpressRoute, see the [ExpressRoute FAQ](expressroute-faqs.md).</span></span>
