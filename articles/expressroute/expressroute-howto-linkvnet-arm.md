---
title: "A virtuális hálózati tooan ExpressRoute-kapcsolatcsoportot hivatkozás: PowerShell: Azure |} Microsoft Docs"
description: "Ez a dokumentum áttekintést hogyan toolink virtuális hálózatok hello Resource Manager üzembe helyezési modellben és a PowerShell használatával (Vnetek) tooExpressRoute kapcsolatok."
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
ms.openlocfilehash: e75a9f6b42fa8e1a579e4f19882ec99b277b545f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-a-virtual-network-tooan-expressroute-circuit"></a><span data-ttu-id="98f84-103">Csatlakozás a virtuális hálózati tooan ExpressRoute-kapcsolatcsoportot</span><span class="sxs-lookup"><span data-stu-id="98f84-103">Connect a virtual network tooan ExpressRoute circuit</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="98f84-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="98f84-104">Azure portal</span></span>](expressroute-howto-linkvnet-portal-resource-manager.md)
> * [<span data-ttu-id="98f84-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="98f84-105">PowerShell</span></span>](expressroute-howto-linkvnet-arm.md)
> * [<span data-ttu-id="98f84-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="98f84-106">Azure CLI</span></span>](howto-linkvnet-cli.md)
> * [<span data-ttu-id="98f84-107">Video - Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="98f84-107">Video - Azure portal</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit)
> * [<span data-ttu-id="98f84-108">PowerShell (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="98f84-108">PowerShell (classic)</span></span>](expressroute-howto-linkvnet-classic.md)
>

<span data-ttu-id="98f84-109">Ez a cikk segítséget nyújt a virtuális hálózatokon (Vnetek) tooAzure ExpressRoute-Kapcsolatcsoportok hivatkozás hello Resource Manager üzembe helyezési modellben és a PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="98f84-109">This article helps you link virtual networks (VNets) tooAzure ExpressRoute circuits by using hello Resource Manager deployment model and PowerShell.</span></span> <span data-ttu-id="98f84-110">Virtuális hálózatok hello lehet azonos az előfizetés vagy egy másik előfizetés részeként.</span><span class="sxs-lookup"><span data-stu-id="98f84-110">Virtual networks can either be in hello same subscription or part of another subscription.</span></span> <span data-ttu-id="98f84-111">Ez a cikk azt is bemutatja, hogyan tooupdate virtuális hálózat hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="98f84-111">This article also shows you how tooupdate a virtual network link.</span></span> 

## <a name="before-you-begin"></a><span data-ttu-id="98f84-112">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="98f84-112">Before you begin</span></span>
* <span data-ttu-id="98f84-113">Hello hello Azure PowerShell-modulok legújabb verziójának telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="98f84-113">Install hello latest version of hello Azure PowerShell modules.</span></span> <span data-ttu-id="98f84-114">További információkért lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="98f84-114">For more information, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>
* <span data-ttu-id="98f84-115">Felülvizsgálati hello [Előfeltételek](expressroute-prerequisites.md), [útválasztási követelmények](expressroute-routing.md), és [munkafolyamatok](expressroute-workflows.md) konfigurálás elkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="98f84-115">Review hello [prerequisites](expressroute-prerequisites.md), [routing requirements](expressroute-routing.md), and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>
* <span data-ttu-id="98f84-116">Egy aktív ExpressRoute-kapcsolatcsoportra lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="98f84-116">You must have an active ExpressRoute circuit.</span></span> 
  * <span data-ttu-id="98f84-117">Útmutatás alapján hello túl[ExpressRoute-kapcsolatcsoportot létrehozni](expressroute-howto-circuit-arm.md) és hello áramkör szerint a kapcsolat szolgáltatójánál engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="98f84-117">Follow hello instructions too[create an ExpressRoute circuit](expressroute-howto-circuit-arm.md) and have hello circuit enabled by your connectivity provider.</span></span> 
  * <span data-ttu-id="98f84-118">Győződjön meg arról, hogy rendelkezik az Azure magánhálózati társviszony-létesítés a kapcsolatcsoport konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="98f84-118">Ensure that you have Azure private peering configured for your circuit.</span></span> <span data-ttu-id="98f84-119">Lásd: hello [konfigurálja az útválasztást](expressroute-howto-routing-arm.md) útválasztási miként.</span><span class="sxs-lookup"><span data-stu-id="98f84-119">See hello [configure routing](expressroute-howto-routing-arm.md) article for routing instructions.</span></span> 
  * <span data-ttu-id="98f84-120">Győződjön meg arról, hogy az Azure magánhálózati társviszony-létesítés van konfigurálva, és hello BGP társviszony-létesítés a hálózat és a Microsoft között működik-e, hogy engedélyezheti a végpontok közötti kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="98f84-120">Ensure that Azure private peering is configured and hello BGP peering between your network and Microsoft is up so that you can enable end-to-end connectivity.</span></span>
  * <span data-ttu-id="98f84-121">Győződjön meg arról, hogy a virtuális hálózat és a virtuális hálózati átjáró létrehozása, és teljesen kiépítve.</span><span class="sxs-lookup"><span data-stu-id="98f84-121">Ensure that you have a virtual network and a virtual network gateway created and fully provisioned.</span></span> <span data-ttu-id="98f84-122">Útmutatás alapján hello túl[hozzon létre egy virtuális hálózati átjáró ExpressRoute](expressroute-howto-add-gateway-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="98f84-122">Follow hello instructions too[create a virtual network gateway for ExpressRoute](expressroute-howto-add-gateway-resource-manager.md).</span></span> <span data-ttu-id="98f84-123">Az ExpressRoute a virtuális hálózati átjáró nem VPN GatewayType "ExpressRoute" hello használja.</span><span class="sxs-lookup"><span data-stu-id="98f84-123">A virtual network gateway for ExpressRoute uses hello GatewayType 'ExpressRoute', not VPN.</span></span>

* <span data-ttu-id="98f84-124">Too10 virtuális hálózatok tooa szabványos ExpressRoute-kapcsolatcsoportot is csatolhatja.</span><span class="sxs-lookup"><span data-stu-id="98f84-124">You can link up too10 virtual networks tooa standard ExpressRoute circuit.</span></span> <span data-ttu-id="98f84-125">Az összes virtuális hálózatot kell hello geopolitikai régión szabványos ExpressRoute-kapcsolatcsoportot használatakor.</span><span class="sxs-lookup"><span data-stu-id="98f84-125">All virtual networks must be in hello same geopolitical region when using a standard ExpressRoute circuit.</span></span> 

* <span data-ttu-id="98f84-126">Csatolni a virtuális hálózatok hello geopolitikai területet hello ExpressRoute-kapcsolatcsoportot kívül, vagy csatlakoztassa a virtuális hálózatok tooyour ExpressRoute-kapcsolatcsoportot nagyobb számú hello ExpressRoute prémium szintű bővítmény vannak engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="98f84-126">You can link a virtual networks outside of hello geopolitical region of hello ExpressRoute circuit, or connect a larger number of virtual networks tooyour ExpressRoute circuit if you enabled hello ExpressRoute premium add-on.</span></span> <span data-ttu-id="98f84-127">Ellenőrizze a hello [– gyakori kérdések](expressroute-faqs.md) hello prémium szintű bővítmény olvashat.</span><span class="sxs-lookup"><span data-stu-id="98f84-127">Check hello [FAQ](expressroute-faqs.md) for more details on hello premium add-on.</span></span>


## <a name="connect-a-virtual-network-in-hello-same-subscription-tooa-circuit"></a><span data-ttu-id="98f84-128">A virtuális hálózat a hello azonos előfizetés tooa áramkör</span><span class="sxs-lookup"><span data-stu-id="98f84-128">Connect a virtual network in hello same subscription tooa circuit</span></span>
<span data-ttu-id="98f84-129">A virtuális hálózati átjáró tooan ExpressRoute-kapcsolatcsoportot hello a következő parancsmag használatával képes kapcsolódni.</span><span class="sxs-lookup"><span data-stu-id="98f84-129">You can connect a virtual network gateway tooan ExpressRoute circuit by using hello following cmdlet.</span></span> <span data-ttu-id="98f84-130">Győződjön meg arról, hogy hello virtuális hálózati átjáró jön létre, és készen áll a linking hello parancsmag futtatása előtt:</span><span class="sxs-lookup"><span data-stu-id="98f84-130">Make sure that hello virtual network gateway is created and is ready for linking before you run hello cmdlet:</span></span>

```powershell
$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
$gw = Get-AzureRmVirtualNetworkGateway -Name "ExpressRouteGw" -ResourceGroupName "MyRG"
$connection = New-AzureRmVirtualNetworkGatewayConnection -Name "ERConnection" -ResourceGroupName "MyRG" -Location "East US" -VirtualNetworkGateway1 $gw -PeerId $circuit.Id -ConnectionType ExpressRoute
```

## <a name="connect-a-virtual-network-in-a-different-subscription-tooa-circuit"></a><span data-ttu-id="98f84-131">Egy másik előfizetésben található tooa kapcsolat a virtuális hálózatot</span><span class="sxs-lookup"><span data-stu-id="98f84-131">Connect a virtual network in a different subscription tooa circuit</span></span>
<span data-ttu-id="98f84-132">Több előfizetés ExpressRoute-kapcsolatcsoportot lehet megosztani.</span><span class="sxs-lookup"><span data-stu-id="98f84-132">You can share an ExpressRoute circuit across multiple subscriptions.</span></span> <span data-ttu-id="98f84-133">a következő ábra hello mutatja egy egyszerű sematikus ExpressRoute-Kapcsolatcsoportok megosztási hogyan működik a több előfizetésekhez.</span><span class="sxs-lookup"><span data-stu-id="98f84-133">hello following figure shows a simple schematic of how sharing works for ExpressRoute circuits across multiple subscriptions.</span></span>

<span data-ttu-id="98f84-134">Hello kisebb felhők hello nagy felhőben egy szervezeten belül toodifferent részlegek tartozó használt toorepresent előfizetések.</span><span class="sxs-lookup"><span data-stu-id="98f84-134">Each of hello smaller clouds within hello large cloud is used toorepresent subscriptions that belong toodifferent departments within an organization.</span></span> <span data-ttu-id="98f84-135">Egyes hello részlegek hello szervezeten belül használhatja a saját előfizetés üzembe helyezéséhez, de a szolgáltatásból egy ExpressRoute körön tooconnect hátsó tooyour a helyszíni hálózaton megoszthatja.</span><span class="sxs-lookup"><span data-stu-id="98f84-135">Each of hello departments within hello organization can use their own subscription for deploying their services--but they can share a single ExpressRoute circuit tooconnect back tooyour on-premises network.</span></span> <span data-ttu-id="98f84-136">Egy részleghez (ebben a példában: informatikai) is saját hello ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="98f84-136">A single department (in this example: IT) can own hello ExpressRoute circuit.</span></span> <span data-ttu-id="98f84-137">Hello szervezeten belüli más előfizetésekkel használható hello ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="98f84-137">Other subscriptions within hello organization can use hello ExpressRoute circuit.</span></span>

> [!NOTE]
> <span data-ttu-id="98f84-138">Hello ExpressRoute-kapcsolatcsoportot kapcsolat és a sávszélesség költségekkel alkalmazott toohello előfizetés tulajdonosa lesz.</span><span class="sxs-lookup"><span data-stu-id="98f84-138">Connectivity and bandwidth charges for hello ExpressRoute circuit will be applied toohello subscription owner.</span></span> <span data-ttu-id="98f84-139">Az összes virtuális hálózatot megosztása hello azonos sávszélesség.</span><span class="sxs-lookup"><span data-stu-id="98f84-139">All virtual networks share hello same bandwidth.</span></span>
> 
> 

![Az előfizetések közötti kapcsolathoz](./media/expressroute-howto-linkvnet-classic/cross-subscription.png)


### <a name="administration---circuit-owners-and-circuit-users"></a><span data-ttu-id="98f84-141">Felügyeleti - áramkör tulajdonosa és a kapcsolatcsoport felhasználók</span><span class="sxs-lookup"><span data-stu-id="98f84-141">Administration - circuit owners and circuit users</span></span>

<span data-ttu-id="98f84-142">hello "kapcsolatcsoport tulajdonosát" hello ExpressRoute-kapcsolatcsoport erőforrás Power jogosultsága.</span><span class="sxs-lookup"><span data-stu-id="98f84-142">hello 'circuit owner' is an authorized Power User of hello ExpressRoute circuit resource.</span></span> <span data-ttu-id="98f84-143">hello kapcsolatcsoport tulajdonosát hozhat létre, amely a "kör felhasználók" is váltható engedélyek.</span><span class="sxs-lookup"><span data-stu-id="98f84-143">hello circuit owner can create authorizations that can be redeemed by 'circuit users'.</span></span> <span data-ttu-id="98f84-144">Kör felhasználók tulajdonosai virtuális hálózati átjárók, amelyek nincsenek belül hello ugyanahhoz az előfizetéshez, ExpressRoute-kapcsolatcsoportot hello.</span><span class="sxs-lookup"><span data-stu-id="98f84-144">Circuit users are owners of virtual network gateways that are not within hello same subscription as hello ExpressRoute circuit.</span></span> <span data-ttu-id="98f84-145">Kör felhasználók is beválthatja engedélyek (virtuális hálózatonként egy engedélyezési).</span><span class="sxs-lookup"><span data-stu-id="98f84-145">Circuit users can redeem authorizations (one authorization per virtual network).</span></span>

<span data-ttu-id="98f84-146">hello kapcsolatcsoport tulajdonosát hello power toomodify és visszavonni az engedélyeket bármikor rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="98f84-146">hello circuit owner has hello power toomodify and revoke authorizations at any time.</span></span> <span data-ttu-id="98f84-147">Az összes hivatkozás kapcsolatok törlődnek, amelyek hozzáférését visszavonták hello előfizetésből engedélyezési eredményez visszavonása.</span><span class="sxs-lookup"><span data-stu-id="98f84-147">Revoking an authorization results in all link connections being deleted from hello subscription whose access was revoked.</span></span>

### <a name="circuit-owner-operations"></a><span data-ttu-id="98f84-148">Kapcsolatcsoport-tulajdonos műveletek</span><span class="sxs-lookup"><span data-stu-id="98f84-148">Circuit owner operations</span></span>

<span data-ttu-id="98f84-149">**az engedély toocreate**</span><span class="sxs-lookup"><span data-stu-id="98f84-149">**toocreate an authorization**</span></span>

<span data-ttu-id="98f84-150">hello kapcsolatcsoport tulajdonosát engedélyezési hoz létre.</span><span class="sxs-lookup"><span data-stu-id="98f84-150">hello circuit owner creates an authorization.</span></span> <span data-ttu-id="98f84-151">Az eredmény lehet engedélykulcs hello létrehozását a virtuális hálózati átjárók toohello ExpressRoute-kapcsolatcsoportot egy kör felhasználói tooconnect használják.</span><span class="sxs-lookup"><span data-stu-id="98f84-151">This results in hello creation of an authorization key that can be used by a circuit user tooconnect their virtual network gateways toohello ExpressRoute circuit.</span></span> <span data-ttu-id="98f84-152">Az engedély csak egy kapcsolat érvénytelen.</span><span class="sxs-lookup"><span data-stu-id="98f84-152">An authorization is valid for only one connection.</span></span>

<span data-ttu-id="98f84-153">a következő parancsmag kódrészletet mutat be hogyan hello toocreate engedélyezési:</span><span class="sxs-lookup"><span data-stu-id="98f84-153">hello following cmdlet snippet shows how toocreate an authorization:</span></span>

```powershell
$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
Add-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit -Name "MyAuthorization1"
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $circuit

$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
$auth1 = Get-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit -Name "MyAuthorization1"
```


<span data-ttu-id="98f84-154">hello válasz toothis hello hitelesítési kulcs és az állapot fogja tartalmazni:</span><span class="sxs-lookup"><span data-stu-id="98f84-154">hello response toothis will contain hello authorization key and status:</span></span>

    Name                   : MyAuthorization1
    Id                     : /subscriptions/&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&/resourceGroups/ERCrossSubTestRG/providers/Microsoft.Network/expressRouteCircuits/CrossSubTest/authorizations/MyAuthorization1
    Etag                   : &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&& 
    AuthorizationKey       : ####################################
    AuthorizationUseStatus : Available
    ProvisioningState      : Succeeded



<span data-ttu-id="98f84-155">**tooreview engedélyek**</span><span class="sxs-lookup"><span data-stu-id="98f84-155">**tooreview authorizations**</span></span>

<span data-ttu-id="98f84-156">hello kapcsolatcsoport tulajdonosát hello a következő parancsmag futtatásával egy adott körön kiállított összes engedélyek tekinthetők át:</span><span class="sxs-lookup"><span data-stu-id="98f84-156">hello circuit owner can review all authorizations that are issued on a particular circuit by running hello following cmdlet:</span></span>

```powershell
$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
$authorizations = Get-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit
```

<span data-ttu-id="98f84-157">**tooadd engedélyek**</span><span class="sxs-lookup"><span data-stu-id="98f84-157">**tooadd authorizations**</span></span>

<span data-ttu-id="98f84-158">hello kapcsolatcsoport tulajdonosát hello a következő parancsmag használatával adhat hozzá engedélyek:</span><span class="sxs-lookup"><span data-stu-id="98f84-158">hello circuit owner can add authorizations by using hello following cmdlet:</span></span>

```powershell
$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
Add-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit -Name "MyAuthorization2"
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $circuit

$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
$authorizations = Get-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit
```

<span data-ttu-id="98f84-159">**toodelete engedélyek**</span><span class="sxs-lookup"><span data-stu-id="98f84-159">**toodelete authorizations**</span></span>

<span data-ttu-id="98f84-160">hello kapcsolatcsoport tulajdonosát képes visszavonni vagy törlése engedélyek toohello felhasználói hello a következő parancsmag futtatásával:</span><span class="sxs-lookup"><span data-stu-id="98f84-160">hello circuit owner can revoke/delete authorizations toohello user by running hello following cmdlet:</span></span>

```powershell
Remove-AzureRmExpressRouteCircuitAuthorization -Name "MyAuthorization2" -ExpressRouteCircuit $circuit
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $circuit
```    

### <a name="circuit-user-operations"></a><span data-ttu-id="98f84-161">Kör felhasználói műveletek</span><span class="sxs-lookup"><span data-stu-id="98f84-161">Circuit user operations</span></span>

<span data-ttu-id="98f84-162">hello áramkör felhasználói hello társ-Azonosítóval és a hello kapcsolatcsoport tulajdonosát a engedélykulcs kell.</span><span class="sxs-lookup"><span data-stu-id="98f84-162">hello circuit user needs hello peer ID and an authorization key from hello circuit owner.</span></span> <span data-ttu-id="98f84-163">hello engedélyezési kulcsa egy GUID Azonosítót.</span><span class="sxs-lookup"><span data-stu-id="98f84-163">hello authorization key is a GUID.</span></span>

<span data-ttu-id="98f84-164">Társ azonosító ellenőrizhetők a hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="98f84-164">Peer ID can be checked from hello following command:</span></span>

```powershell
Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
```

<span data-ttu-id="98f84-165">**egy kapcsolat hitelesítési tooredeem**</span><span class="sxs-lookup"><span data-stu-id="98f84-165">**tooredeem a connection authorization**</span></span>

<span data-ttu-id="98f84-166">hello áramkör felhasználó futtathatja a következő parancsmag tooredeem hello egy hivatkozás hitelesítési:</span><span class="sxs-lookup"><span data-stu-id="98f84-166">hello circuit user can run hello following cmdlet tooredeem a link authorization:</span></span>

```powershell
$id = "/subscriptions/********************************/resourceGroups/ERCrossSubTestRG/providers/Microsoft.Network/expressRouteCircuits/MyCircuit"    
$gw = Get-AzureRmVirtualNetworkGateway -Name "ExpressRouteGw" -ResourceGroupName "MyRG"
$connection = New-AzureRmVirtualNetworkGatewayConnection -Name "ERConnection" -ResourceGroupName "RemoteResourceGroup" -Location "East US" -VirtualNetworkGateway1 $gw -PeerId $id -ConnectionType ExpressRoute -AuthorizationKey "^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^"
```

<span data-ttu-id="98f84-167">**egy kapcsolat hitelesítési toorelease**</span><span class="sxs-lookup"><span data-stu-id="98f84-167">**toorelease a connection authorization**</span></span>

<span data-ttu-id="98f84-168">Az engedély törölni kell, amely a hello ExpressRoute körön toohello virtuális hálózati kapcsolat hello is megjelenhetnek.</span><span class="sxs-lookup"><span data-stu-id="98f84-168">You can release an authorization by deleting hello connection that links hello ExpressRoute circuit toohello virtual network.</span></span>

## <a name="modify-a-virtual-network-connection"></a><span data-ttu-id="98f84-169">Módosítsa a virtuális hálózati kapcsolat</span><span class="sxs-lookup"><span data-stu-id="98f84-169">Modify a virtual network connection</span></span>
<span data-ttu-id="98f84-170">Frissítheti az egyes tulajdonságait a virtuális hálózati kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="98f84-170">You can update certain properties of a virtual network connection.</span></span> 

<span data-ttu-id="98f84-171">**tooupdate hello kapcsolat súlyozás**</span><span class="sxs-lookup"><span data-stu-id="98f84-171">**tooupdate hello connection weight**</span></span>

<span data-ttu-id="98f84-172">A virtuális hálózat csatlakoztatott toomultiple ExpressRoute-Kapcsolatcsoportok lehet.</span><span class="sxs-lookup"><span data-stu-id="98f84-172">Your virtual network can be connected toomultiple ExpressRoute circuits.</span></span> <span data-ttu-id="98f84-173">Egynél több ExpressRoute-kapcsolatcsoportot azonos előtag hello kaphat.</span><span class="sxs-lookup"><span data-stu-id="98f84-173">You may receive hello same prefix from more than one ExpressRoute circuit.</span></span> <span data-ttu-id="98f84-174">az előtag szánt mely kapcsolat toosend forgalom toochoose, módosíthatja *routingweight értékének* kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="98f84-174">toochoose which connection toosend traffic destined for this prefix, you can change *RoutingWeight* of a connection.</span></span> <span data-ttu-id="98f84-175">Forgalom küldi hello legmagasabb hello kapcsolatot *routingweight értékének*.</span><span class="sxs-lookup"><span data-stu-id="98f84-175">Traffic will be sent on hello connection with hello highest *RoutingWeight*.</span></span>

```powershell
$connection = Get-AzureRmVirtualNetworkGatewayConnection -Name "MyVirtualNetworkConnection" -ResourceGroupName "MyRG"
$connection.RoutingWeight = 100
Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection
```

<span data-ttu-id="98f84-176">számos hello *routingweight értékének* 0 too32000 van.</span><span class="sxs-lookup"><span data-stu-id="98f84-176">hello range of *RoutingWeight* is 0 too32000.</span></span> <span data-ttu-id="98f84-177">hello alapértelmezett értéke 0.</span><span class="sxs-lookup"><span data-stu-id="98f84-177">hello default value is 0.</span></span>

## <a name="next-steps"></a><span data-ttu-id="98f84-178">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="98f84-178">Next steps</span></span>
<span data-ttu-id="98f84-179">ExpressRoute kapcsolatos további információkért lásd: hello [ExpressRoute – gyakori kérdések](expressroute-faqs.md).</span><span class="sxs-lookup"><span data-stu-id="98f84-179">For more information about ExpressRoute, see hello [ExpressRoute FAQ](expressroute-faqs.md).</span></span>
