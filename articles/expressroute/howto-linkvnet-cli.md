---
title: "A virtuális hálózati tooan ExpressRoute-kapcsolatcsoportot hivatkozás: parancssori felület: Azure |} Microsoft Docs"
description: "Ez a dokumentum áttekintést hogyan toolink virtuális hálózatok hello Resource Manager üzembe helyezési modellben és a parancssori felület használatával (Vnetek) tooExpressRoute kapcsolatok."
services: expressroute
documentationcenter: na
author: cherylmc
manager: timlit
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/25/2017
ms.author: anzaman,cherylmc
ms.openlocfilehash: 1251f016d9b94d3fee81de1df164cb085cbe9d78
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-a-virtual-network-tooan-expressroute-circuit-using-cli"></a><span data-ttu-id="45a68-103">Csatlakozás a virtuális hálózati tooan ExpressRoute-kapcsolatcsoportot parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="45a68-103">Connect a virtual network tooan ExpressRoute circuit using CLI</span></span>

<span data-ttu-id="45a68-104">Ez a cikk segít csatolni a virtuális hálózatokon (Vnetek) ExpressRoute-Kapcsolatcsoportok tooAzure parancssori felület használatával.</span><span class="sxs-lookup"><span data-stu-id="45a68-104">This article helps you link virtual networks (VNets) tooAzure ExpressRoute circuits using CLI.</span></span> <span data-ttu-id="45a68-105">Azure parancssori felület használatával toolink hello virtuális hálózatok hello Resource Manager telepítési modell segítségével kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="45a68-105">toolink using Azure CLI, hello virtual networks must be created using hello Resource Manager deployment model.</span></span> <span data-ttu-id="45a68-106">Azok a hello lehet ugyanazt az előfizetést, vagy egy másik előfizetés részeként.</span><span class="sxs-lookup"><span data-stu-id="45a68-106">They can either be in hello same subscription, or part of another subscription.</span></span> <span data-ttu-id="45a68-107">Ha egy másik módszer tooconnect toouse a VNet tooan ExpressRoute-kapcsolatcsoportot, cikk választhat a következő lista hello:</span><span class="sxs-lookup"><span data-stu-id="45a68-107">If you want toouse a different method tooconnect your VNet tooan ExpressRoute circuit, you can select an article from hello following list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="45a68-108">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="45a68-108">Azure portal</span></span>](expressroute-howto-linkvnet-portal-resource-manager.md)
> * [<span data-ttu-id="45a68-109">PowerShell</span><span class="sxs-lookup"><span data-stu-id="45a68-109">PowerShell</span></span>](expressroute-howto-linkvnet-arm.md)
> * [<span data-ttu-id="45a68-110">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="45a68-110">Azure CLI</span></span>](howto-linkvnet-cli.md)
> * [<span data-ttu-id="45a68-111">Video - Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="45a68-111">Video - Azure portal</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit)
> * [<span data-ttu-id="45a68-112">PowerShell (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="45a68-112">PowerShell (classic)</span></span>](expressroute-howto-linkvnet-classic.md)
> 

## <a name="configuration-prerequisites"></a><span data-ttu-id="45a68-113">Konfigurációs előfeltételek</span><span class="sxs-lookup"><span data-stu-id="45a68-113">Configuration prerequisites</span></span>

* <span data-ttu-id="45a68-114">Hello hello parancssori felület (CLI) legújabb verziójára van szüksége.</span><span class="sxs-lookup"><span data-stu-id="45a68-114">You need hello latest version of hello command-line interface (CLI).</span></span> <span data-ttu-id="45a68-115">További információkért lásd: [Azure CLI 2.0 telepítése](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="45a68-115">For more information, see [Install Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli).</span></span>
* <span data-ttu-id="45a68-116">Tooreview hello kell [Előfeltételek](expressroute-prerequisites.md), [útválasztási követelmények](expressroute-routing.md), és [munkafolyamatok](expressroute-workflows.md) konfigurálás elkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="45a68-116">You need tooreview hello [prerequisites](expressroute-prerequisites.md), [routing requirements](expressroute-routing.md), and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>
* <span data-ttu-id="45a68-117">Egy aktív ExpressRoute-kapcsolatcsoportra lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="45a68-117">You must have an active ExpressRoute circuit.</span></span> 
  * <span data-ttu-id="45a68-118">Útmutatás alapján hello túl[ExpressRoute-kapcsolatcsoportot létrehozni](howto-circuit-cli.md) és hello áramkör szerint a kapcsolat szolgáltatójánál engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="45a68-118">Follow hello instructions too[create an ExpressRoute circuit](howto-circuit-cli.md) and have hello circuit enabled by your connectivity provider.</span></span> 
  * <span data-ttu-id="45a68-119">Győződjön meg arról, hogy rendelkezik az Azure magánhálózati társviszony-létesítés a kapcsolatcsoport konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="45a68-119">Ensure that you have Azure private peering configured for your circuit.</span></span> <span data-ttu-id="45a68-120">Lásd: hello [konfigurálja az útválasztást](howto-routing-cli.md) útválasztási miként.</span><span class="sxs-lookup"><span data-stu-id="45a68-120">See hello [configure routing](howto-routing-cli.md) article for routing instructions.</span></span> 
  * <span data-ttu-id="45a68-121">Győződjön meg arról, hogy konfigurálva van-e az Azure magánhálózati társviszony-létesítés.</span><span class="sxs-lookup"><span data-stu-id="45a68-121">Ensure that Azure private peering is configured.</span></span> <span data-ttu-id="45a68-122">hello BGP társviszony-létesítés a hálózat és a Microsoft között kell lennie be, hogy engedélyezheti a végpontok közötti kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="45a68-122">hello BGP peering between your network and Microsoft must be up so that you can enable end-to-end connectivity.</span></span>
  * <span data-ttu-id="45a68-123">Győződjön meg arról, hogy a virtuális hálózat és a virtuális hálózati átjáró létrehozása, és teljesen kiépítve.</span><span class="sxs-lookup"><span data-stu-id="45a68-123">Ensure that you have a virtual network and a virtual network gateway created and fully provisioned.</span></span> <span data-ttu-id="45a68-124">Útmutatás alapján hello túl[a virtuális hálózati átjáró konfigurálása ExpressRoute](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli).</span><span class="sxs-lookup"><span data-stu-id="45a68-124">Follow hello instructions too[Configure a virtual network gateway for ExpressRoute](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli).</span></span> <span data-ttu-id="45a68-125">Lehet, hogy toouse `--gateway-type ExpressRoute`.</span><span class="sxs-lookup"><span data-stu-id="45a68-125">Be sure toouse `--gateway-type ExpressRoute`.</span></span>

* <span data-ttu-id="45a68-126">Too10 virtuális hálózatok tooa szabványos ExpressRoute-kapcsolatcsoportot is csatolhatja.</span><span class="sxs-lookup"><span data-stu-id="45a68-126">You can link up too10 virtual networks tooa standard ExpressRoute circuit.</span></span> <span data-ttu-id="45a68-127">Az összes virtuális hálózatot kell hello geopolitikai régión szabványos ExpressRoute-kapcsolatcsoportot használatakor.</span><span class="sxs-lookup"><span data-stu-id="45a68-127">All virtual networks must be in hello same geopolitical region when using a standard ExpressRoute circuit.</span></span> 

* <span data-ttu-id="45a68-128">Ha hello ExpressRoute prémium bővítmény engedélyezéséhez egy virtuális hálózaton kívül hello geopolitikai területet hello ExpressRoute-kapcsolatcsoportot hivatkozásra, vagy csatlakoztassa a virtuális hálózatok tooyour ExpressRoute-kapcsolatcsoportot nagyobb számú.</span><span class="sxs-lookup"><span data-stu-id="45a68-128">If you enable hello ExpressRoute premium add-on, you can link a virtual network outside of hello geopolitical region of hello ExpressRoute circuit, or connect a larger number of virtual networks tooyour ExpressRoute circuit.</span></span> <span data-ttu-id="45a68-129">Prémium szintű bővítmény hello kapcsolatos további információkért lásd: hello [gyakran ismételt kérdések](expressroute-faqs.md).</span><span class="sxs-lookup"><span data-stu-id="45a68-129">For more information about hello premium add-on, see hello [FAQ](expressroute-faqs.md).</span></span>

## <a name="connect-a-virtual-network-in-hello-same-subscription-tooa-circuit"></a><span data-ttu-id="45a68-130">A virtuális hálózat a hello azonos előfizetés tooa áramkör</span><span class="sxs-lookup"><span data-stu-id="45a68-130">Connect a virtual network in hello same subscription tooa circuit</span></span>

<span data-ttu-id="45a68-131">A virtuális hálózati átjáró tooan ExpressRoute-kapcsolatcsoportot hello példa használatával képes kapcsolódni.</span><span class="sxs-lookup"><span data-stu-id="45a68-131">You can connect a virtual network gateway tooan ExpressRoute circuit by using hello example.</span></span> <span data-ttu-id="45a68-132">Győződjön meg arról, hogy hello virtuális hálózati átjáró jön létre, és készen áll a linking hello parancs futtatása előtt.</span><span class="sxs-lookup"><span data-stu-id="45a68-132">Make sure that hello virtual network gateway is created and is ready for linking before you run hello command.</span></span>

```azurecli
az network vpn-connection create --name ERConnection --resource-group ExpressRouteResourceGroup --vnet-gateway1 VNet1GW --express-route-circuit2 MyCircuit
```

## <a name="connect-a-virtual-network-in-a-different-subscription-tooa-circuit"></a><span data-ttu-id="45a68-133">Egy másik előfizetésben található tooa kapcsolat a virtuális hálózatot</span><span class="sxs-lookup"><span data-stu-id="45a68-133">Connect a virtual network in a different subscription tooa circuit</span></span>

<span data-ttu-id="45a68-134">Több előfizetés ExpressRoute-kapcsolatcsoportot lehet megosztani.</span><span class="sxs-lookup"><span data-stu-id="45a68-134">You can share an ExpressRoute circuit across multiple subscriptions.</span></span> <span data-ttu-id="45a68-135">hello az alábbi ábra egy egyszerű sematikus ExpressRoute-Kapcsolatcsoportok megosztási hogyan működik a több előfizetésekhez.</span><span class="sxs-lookup"><span data-stu-id="45a68-135">hello figure below shows a simple schematic of how sharing works for ExpressRoute circuits across multiple subscriptions.</span></span>

<span data-ttu-id="45a68-136">Hello kisebb felhők hello nagy felhőben egy szervezeten belül toodifferent részlegek tartozó használt toorepresent előfizetések.</span><span class="sxs-lookup"><span data-stu-id="45a68-136">Each of hello smaller clouds within hello large cloud is used toorepresent subscriptions that belong toodifferent departments within an organization.</span></span> <span data-ttu-id="45a68-137">Egyes hello részlegek hello szervezeten belül használhatja a saját előfizetés üzembe helyezéséhez, de a szolgáltatásból egy ExpressRoute körön tooconnect hátsó tooyour a helyszíni hálózaton megoszthatja.</span><span class="sxs-lookup"><span data-stu-id="45a68-137">Each of hello departments within hello organization can use their own subscription for deploying their services--but they can share a single ExpressRoute circuit tooconnect back tooyour on-premises network.</span></span> <span data-ttu-id="45a68-138">Egy részleghez (ebben a példában: informatikai) is saját hello ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="45a68-138">A single department (in this example: IT) can own hello ExpressRoute circuit.</span></span> <span data-ttu-id="45a68-139">Hello szervezeten belüli más előfizetésekkel használható hello ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="45a68-139">Other subscriptions within hello organization can use hello ExpressRoute circuit.</span></span>

> [!NOTE]
> <span data-ttu-id="45a68-140">Kapcsolat és a sávszélesség díjak dedikált hello kör lesz alkalmazott toohello ExpressRoute-kapcsolatcsoport tulajdonosát.</span><span class="sxs-lookup"><span data-stu-id="45a68-140">Connectivity and bandwidth charges for hello dedicated circuit will be applied toohello ExpressRoute Circuit Owner.</span></span> <span data-ttu-id="45a68-141">Az összes virtuális hálózatot megosztása hello azonos sávszélesség.</span><span class="sxs-lookup"><span data-stu-id="45a68-141">All virtual networks share hello same bandwidth.</span></span>
> 
> 

![Az előfizetések közötti kapcsolathoz](./media/expressroute-howto-linkvnet-classic/cross-subscription.png)

### <a name="administration---circuit-owners-and-circuit-users"></a><span data-ttu-id="45a68-143">Felügyeleti - áramkör tulajdonosa és a kapcsolatcsoport felhasználók</span><span class="sxs-lookup"><span data-stu-id="45a68-143">Administration - Circuit Owners and Circuit Users</span></span>

<span data-ttu-id="45a68-144">hello "Kapcsolatcsoport tulajdonosát" hello ExpressRoute-kapcsolatcsoport erőforrás Power jogosultsága.</span><span class="sxs-lookup"><span data-stu-id="45a68-144">hello 'Circuit Owner' is an authorized Power User of hello ExpressRoute circuit resource.</span></span> <span data-ttu-id="45a68-145">hello kapcsolatcsoport tulajdonosát hozhat létre, amely a "Kör felhasználók" is váltható engedélyek.</span><span class="sxs-lookup"><span data-stu-id="45a68-145">hello Circuit Owner can create authorizations that can be redeemed by 'Circuit Users'.</span></span> <span data-ttu-id="45a68-146">Kör felhasználók tulajdonosai virtuális hálózati átjárók, amelyek nincsenek belül hello ugyanahhoz az előfizetéshez, ExpressRoute-kapcsolatcsoportot hello.</span><span class="sxs-lookup"><span data-stu-id="45a68-146">Circuit Users are owners of virtual network gateways that are not within hello same subscription as hello ExpressRoute circuit.</span></span> <span data-ttu-id="45a68-147">Kör felhasználók is beválthatja engedélyek (virtuális hálózatonként egy engedélyezési).</span><span class="sxs-lookup"><span data-stu-id="45a68-147">Circuit Users can redeem authorizations (one authorization per virtual network).</span></span>

<span data-ttu-id="45a68-148">hello kapcsolatcsoport tulajdonosát hello power toomodify és visszavonni az engedélyeket bármikor rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="45a68-148">hello Circuit Owner has hello power toomodify and revoke authorizations at any time.</span></span> <span data-ttu-id="45a68-149">Amikor visszavon egy engedélyezési, minden kapcsolatoknál törlődnek hello előfizetésből, amelyek hozzáférését visszavonták.</span><span class="sxs-lookup"><span data-stu-id="45a68-149">When an authorization is revoked, all link connections are deleted from hello subscription whose access was revoked.</span></span>

### <a name="circuit-owner-operations"></a><span data-ttu-id="45a68-150">Kapcsolatcsoport-tulajdonos műveletek</span><span class="sxs-lookup"><span data-stu-id="45a68-150">Circuit Owner operations</span></span>

<span data-ttu-id="45a68-151">**az engedély toocreate**</span><span class="sxs-lookup"><span data-stu-id="45a68-151">**toocreate an authorization**</span></span>

<span data-ttu-id="45a68-152">hello kapcsolatcsoport tulajdonosát az engedélyt, amely hoz létre, amelyek lehetnek engedélykulcs hoz létre a virtuális hálózati átjárók toohello ExpressRoute-kapcsolatcsoportot egy kör felhasználói tooconnect használják.</span><span class="sxs-lookup"><span data-stu-id="45a68-152">hello Circuit Owner creates an authorization, which creates an authorization key that can be used by a Circuit User tooconnect their virtual network gateways toohello ExpressRoute circuit.</span></span> <span data-ttu-id="45a68-153">Az engedély csak egy kapcsolat érvénytelen.</span><span class="sxs-lookup"><span data-stu-id="45a68-153">An authorization is valid for only one connection.</span></span>

<span data-ttu-id="45a68-154">a következő példa azt mutatja meg hogyan hello toocreate engedélyezési:</span><span class="sxs-lookup"><span data-stu-id="45a68-154">hello following example shows how toocreate an authorization:</span></span>

```azurecli
az network express-route auth create --circuit-name MyCircuit -g ExpressRouteResourceGroup -n MyAuthorization
```

<span data-ttu-id="45a68-155">hello válasz tartalmazza, hello engedélyezési kulcsot és annak állapotát:</span><span class="sxs-lookup"><span data-stu-id="45a68-155">hello response contains hello authorization key and status:</span></span>

```azurecli
"authorizationKey": "0a7f3020-541f-4b4b-844a-5fb43472e3d7",
"authorizationUseStatus": "Available",
"etag": "W/\"010353d4-8955-4984-807a-585c21a22ae0\"",
"id": "/subscriptions/81ab786c-56eb-4a4d-bb5f-f60329772466/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/MyCircuit/authorizations/MyAuthorization1",
"name": "MyAuthorization1",
"provisioningState": "Succeeded",
"resourceGroup": "ExpressRouteResourceGroup"
```

<span data-ttu-id="45a68-156">**tooreview engedélyek**</span><span class="sxs-lookup"><span data-stu-id="45a68-156">**tooreview authorizations**</span></span>

<span data-ttu-id="45a68-157">hello kapcsolatcsoport tulajdonosát tekinthetők át egy adott körön fut a következő példa hello által kiállított összes engedélyek:</span><span class="sxs-lookup"><span data-stu-id="45a68-157">hello Circuit Owner can review all authorizations that are issued on a particular circuit by running hello following example:</span></span>

```azurecli
az network express-route auth list --circuit-name MyCircuit -g ExpressRouteResourceGroup
```

<span data-ttu-id="45a68-158">**tooadd engedélyek**</span><span class="sxs-lookup"><span data-stu-id="45a68-158">**tooadd authorizations**</span></span>

<span data-ttu-id="45a68-159">hello kapcsolatcsoport tulajdonosát a következő példa hello használatával adhat hozzá engedélyek:</span><span class="sxs-lookup"><span data-stu-id="45a68-159">hello Circuit Owner can add authorizations by using hello following example:</span></span>

```azurecli
az network express-route auth create --circuit-name MyCircuit -g ExpressRouteResourceGroup -n MyAuthorization1
```

<span data-ttu-id="45a68-160">**toodelete engedélyek**</span><span class="sxs-lookup"><span data-stu-id="45a68-160">**toodelete authorizations**</span></span>

<span data-ttu-id="45a68-161">hello kapcsolatcsoport tulajdonosát is visszavonni vagy törlése engedélyek toohello felhasználói hello például a következő futtatásával:</span><span class="sxs-lookup"><span data-stu-id="45a68-161">hello Circuit Owner can revoke/delete authorizations toohello user by running hello following example:</span></span>

```azurecli
az network express-route auth delete --circuit-name MyCircuit -g ExpressRouteResourceGroup -n MyAuthorization1
```

### <a name="circuit-user-operations"></a><span data-ttu-id="45a68-162">Kör felhasználói műveletek</span><span class="sxs-lookup"><span data-stu-id="45a68-162">Circuit User operations</span></span>

<span data-ttu-id="45a68-163">hello kapcsolatcsoport-felhasználó a hello társ-Azonosítóval és a kapcsolatcsoport tulajdonosát hello engedélykulcs kell.</span><span class="sxs-lookup"><span data-stu-id="45a68-163">hello Circuit User needs hello peer ID and an authorization key from hello Circuit Owner.</span></span> <span data-ttu-id="45a68-164">hello engedélyezési kulcsa egy GUID Azonosítót.</span><span class="sxs-lookup"><span data-stu-id="45a68-164">hello authorization key is a GUID.</span></span>

```azurecli
Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
```

<span data-ttu-id="45a68-165">**egy kapcsolat hitelesítési tooredeem**</span><span class="sxs-lookup"><span data-stu-id="45a68-165">**tooredeem a connection authorization**</span></span>

<span data-ttu-id="45a68-166">hello áramkör felhasználó futtathatja a következő példa tooredeem hello egy hivatkozás hitelesítési:</span><span class="sxs-lookup"><span data-stu-id="45a68-166">hello Circuit User can run hello following example tooredeem a link authorization:</span></span>

```azurecli
az network vpn-connection create --name ERConnection --resource-group ExpressRouteResourceGroup --vnet-gateway1 VNet1GW --express-route-circuit2 MyCircuit --authorization-key "^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^"
```

<span data-ttu-id="45a68-167">**egy kapcsolat hitelesítési toorelease**</span><span class="sxs-lookup"><span data-stu-id="45a68-167">**toorelease a connection authorization**</span></span>

<span data-ttu-id="45a68-168">Az engedély törölni kell, amely a hello ExpressRoute körön toohello virtuális hálózati kapcsolat hello is megjelenhetnek.</span><span class="sxs-lookup"><span data-stu-id="45a68-168">You can release an authorization by deleting hello connection that links hello ExpressRoute circuit toohello virtual network.</span></span>

## <a name="next-steps"></a><span data-ttu-id="45a68-169">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="45a68-169">Next steps</span></span>

<span data-ttu-id="45a68-170">ExpressRoute kapcsolatos további információkért lásd: hello [ExpressRoute – gyakori kérdések](expressroute-faqs.md).</span><span class="sxs-lookup"><span data-stu-id="45a68-170">For more information about ExpressRoute, see hello [ExpressRoute FAQ](expressroute-faqs.md).</span></span>
