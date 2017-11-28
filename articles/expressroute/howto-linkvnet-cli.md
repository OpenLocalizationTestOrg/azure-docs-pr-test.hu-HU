---
title: "Virtuális hálózat csatolása ExpressRoute-kapcsolatcsoportot: parancssori felület: Azure |} Microsoft Docs"
description: "Ez a dokumentum áttekintést csatolása a virtuális hálózatokon (Vnetek) az ExpressRoute-Kapcsolatcsoportok a Resource Manager üzembe helyezési modellben és a parancssori felület használatával."
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
ms.openlocfilehash: 0ea696e796ec3a943bc028f56da417978b728b82
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/29/2017
---
# <a name="connect-a-virtual-network-to-an-expressroute-circuit-using-cli"></a><span data-ttu-id="ea12c-103">Egy virtuális hálózathoz csatlakozni egy ExpressRoute-kapcsolatcsoportot parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="ea12c-103">Connect a virtual network to an ExpressRoute circuit using CLI</span></span>

<span data-ttu-id="ea12c-104">Ez a cikk segít virtuális hálózatokról (Vnetekről) hivatkozásra az Azure ExpressRoute-Kapcsolatcsoportok parancssori felület használatával.</span><span class="sxs-lookup"><span data-stu-id="ea12c-104">This article helps you link virtual networks (VNets) to Azure ExpressRoute circuits using CLI.</span></span> <span data-ttu-id="ea12c-105">Azure parancssori felület használatával csatolni a virtuális hálózatok segítségével kell létrehozni a Resource Manager üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="ea12c-105">To link using Azure CLI, the virtual networks must be created using the Resource Manager deployment model.</span></span> <span data-ttu-id="ea12c-106">Lehetnek ugyanazon előfizetés, vagy egy másik előfizetés részeként.</span><span class="sxs-lookup"><span data-stu-id="ea12c-106">They can either be in the same subscription, or part of another subscription.</span></span> <span data-ttu-id="ea12c-107">Ha szeretne egy másik módszer segítségével csatlakozzon a virtuális hálózat ExpressRoute-kapcsolatcsoportot, kiválaszthatja a cikk az alábbi listából:</span><span class="sxs-lookup"><span data-stu-id="ea12c-107">If you want to use a different method to connect your VNet to an ExpressRoute circuit, you can select an article from the following list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="ea12c-108">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="ea12c-108">Azure portal</span></span>](expressroute-howto-linkvnet-portal-resource-manager.md)
> * [<span data-ttu-id="ea12c-109">PowerShell</span><span class="sxs-lookup"><span data-stu-id="ea12c-109">PowerShell</span></span>](expressroute-howto-linkvnet-arm.md)
> * [<span data-ttu-id="ea12c-110">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="ea12c-110">Azure CLI</span></span>](howto-linkvnet-cli.md)
> * [<span data-ttu-id="ea12c-111">Video - Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="ea12c-111">Video - Azure portal</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit)
> * [<span data-ttu-id="ea12c-112">PowerShell (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="ea12c-112">PowerShell (classic)</span></span>](expressroute-howto-linkvnet-classic.md)
> 

## <a name="configuration-prerequisites"></a><span data-ttu-id="ea12c-113">Konfigurációs előfeltételek</span><span class="sxs-lookup"><span data-stu-id="ea12c-113">Configuration prerequisites</span></span>

* <span data-ttu-id="ea12c-114">A parancssori felület (CLI) legújabb verziójára van szüksége.</span><span class="sxs-lookup"><span data-stu-id="ea12c-114">You need the latest version of the command-line interface (CLI).</span></span> <span data-ttu-id="ea12c-115">További információkért lásd: [Azure CLI 2.0 telepítése](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="ea12c-115">For more information, see [Install Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli).</span></span>
* <span data-ttu-id="ea12c-116">Át kell tekintenie a [Előfeltételek](expressroute-prerequisites.md), [útválasztási követelmények](expressroute-routing.md), és [munkafolyamatok](expressroute-workflows.md) konfigurálás elkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="ea12c-116">You need to review the [prerequisites](expressroute-prerequisites.md), [routing requirements](expressroute-routing.md), and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>
* <span data-ttu-id="ea12c-117">Egy aktív ExpressRoute-kapcsolatcsoportra lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="ea12c-117">You must have an active ExpressRoute circuit.</span></span> 
  * <span data-ttu-id="ea12c-118">Kövesse az utasításokat [ExpressRoute-kapcsolatcsoportot létrehozni](howto-circuit-cli.md) és a kapcsolatcsoport szerint a kapcsolat szolgáltatójánál engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="ea12c-118">Follow the instructions to [create an ExpressRoute circuit](howto-circuit-cli.md) and have the circuit enabled by your connectivity provider.</span></span> 
  * <span data-ttu-id="ea12c-119">Győződjön meg arról, hogy rendelkezik az Azure magánhálózati társviszony-létesítés a kapcsolatcsoport konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="ea12c-119">Ensure that you have Azure private peering configured for your circuit.</span></span> <span data-ttu-id="ea12c-120">Tekintse meg a [konfigurálja az útválasztást](howto-routing-cli.md) útválasztási miként.</span><span class="sxs-lookup"><span data-stu-id="ea12c-120">See the [configure routing](howto-routing-cli.md) article for routing instructions.</span></span> 
  * <span data-ttu-id="ea12c-121">Győződjön meg arról, hogy konfigurálva van-e az Azure magánhálózati társviszony-létesítés.</span><span class="sxs-lookup"><span data-stu-id="ea12c-121">Ensure that Azure private peering is configured.</span></span> <span data-ttu-id="ea12c-122">A BGP társviszony-létesítés között a hálózati és a Microsoft be kell, hogy engedélyezheti a végpontok közötti kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="ea12c-122">The BGP peering between your network and Microsoft must be up so that you can enable end-to-end connectivity.</span></span>
  * <span data-ttu-id="ea12c-123">Győződjön meg arról, hogy a virtuális hálózat és a virtuális hálózati átjáró létrehozása, és teljesen kiépítve.</span><span class="sxs-lookup"><span data-stu-id="ea12c-123">Ensure that you have a virtual network and a virtual network gateway created and fully provisioned.</span></span> <span data-ttu-id="ea12c-124">Kövesse az utasításokat [a virtuális hálózati átjáró konfigurálása ExpressRoute](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli).</span><span class="sxs-lookup"><span data-stu-id="ea12c-124">Follow the instructions to [Configure a virtual network gateway for ExpressRoute](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli).</span></span> <span data-ttu-id="ea12c-125">Használjon `--gateway-type ExpressRoute`.</span><span class="sxs-lookup"><span data-stu-id="ea12c-125">Be sure to use `--gateway-type ExpressRoute`.</span></span>

* <span data-ttu-id="ea12c-126">Legfeljebb 10 virtuális hálózatok hozzákapcsolhatja egy szabványos ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="ea12c-126">You can link up to 10 virtual networks to a standard ExpressRoute circuit.</span></span> <span data-ttu-id="ea12c-127">Az összes virtuális hálózatot geopolitikai ugyanabban a régióban kell lennie, egy szabványos ExpressRoute-kapcsolatcsoportot használatakor.</span><span class="sxs-lookup"><span data-stu-id="ea12c-127">All virtual networks must be in the same geopolitical region when using a standard ExpressRoute circuit.</span></span> 

* <span data-ttu-id="ea12c-128">Ha engedélyezi a prémium szintű ExpressRoute-bővítmény, egy virtuális hálózaton kívül az ExpressRoute-kapcsolatcsoport geopolitikai régiója hivatkozásra, vagy virtuális hálózatok nagyobb számú kapcsolódni az ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="ea12c-128">If you enable the ExpressRoute premium add-on, you can link a virtual network outside of the geopolitical region of the ExpressRoute circuit, or connect a larger number of virtual networks to your ExpressRoute circuit.</span></span> <span data-ttu-id="ea12c-129">A prémium szintű bővítmény kapcsolatos további információkért tekintse meg a [gyakran ismételt kérdések](expressroute-faqs.md).</span><span class="sxs-lookup"><span data-stu-id="ea12c-129">For more information about the premium add-on, see the [FAQ](expressroute-faqs.md).</span></span>

## <a name="connect-a-virtual-network-in-the-same-subscription-to-a-circuit"></a><span data-ttu-id="ea12c-130">A virtuális hálózati ugyanahhoz az előfizetéshez csatlakozni expressroute-kapcsolatcsoporthoz</span><span class="sxs-lookup"><span data-stu-id="ea12c-130">Connect a virtual network in the same subscription to a circuit</span></span>

<span data-ttu-id="ea12c-131">A példa használatával ExpressRoute-kapcsolatcsoportot virtuális hálózati átjáró képes kapcsolódni.</span><span class="sxs-lookup"><span data-stu-id="ea12c-131">You can connect a virtual network gateway to an ExpressRoute circuit by using the example.</span></span> <span data-ttu-id="ea12c-132">Győződjön meg arról, hogy a virtuális hálózati átjáró jön létre, és készen áll a létrehozhatja, ha a parancs futtatása előtt.</span><span class="sxs-lookup"><span data-stu-id="ea12c-132">Make sure that the virtual network gateway is created and is ready for linking before you run the command.</span></span>

```azurecli
az network vpn-connection create --name ERConnection --resource-group ExpressRouteResourceGroup --vnet-gateway1 VNet1GW --express-route-circuit2 MyCircuit
```

## <a name="connect-a-virtual-network-in-a-different-subscription-to-a-circuit"></a><span data-ttu-id="ea12c-133">Egy másik előfizetéshez tartozó virtuális hálózat bevonása egy kapcsolatcsoportba</span><span class="sxs-lookup"><span data-stu-id="ea12c-133">Connect a virtual network in a different subscription to a circuit</span></span>

<span data-ttu-id="ea12c-134">Több előfizetés ExpressRoute-kapcsolatcsoportot lehet megosztani.</span><span class="sxs-lookup"><span data-stu-id="ea12c-134">You can share an ExpressRoute circuit across multiple subscriptions.</span></span> <span data-ttu-id="ea12c-135">Az alábbi ábra egy egyszerű sematikus ExpressRoute-Kapcsolatcsoportok megosztási hogyan működik a több előfizetésekhez.</span><span class="sxs-lookup"><span data-stu-id="ea12c-135">The figure below shows a simple schematic of how sharing works for ExpressRoute circuits across multiple subscriptions.</span></span>

<span data-ttu-id="ea12c-136">A nagy felhőben kisebb felhők egyes szervezet különböző részlegei tartozó előfizetések megjelenítésére szolgál.</span><span class="sxs-lookup"><span data-stu-id="ea12c-136">Each of the smaller clouds within the large cloud is used to represent subscriptions that belong to different departments within an organization.</span></span> <span data-ttu-id="ea12c-137">A saját előfizetés üzembe helyezéséhez, a szolgáltatások –, de használhatja a szervezeten belüli osztályok mindegyikének oszthatnak meg csatlakozni a helyi hálózat a egyetlen ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="ea12c-137">Each of the departments within the organization can use their own subscription for deploying their services--but they can share a single ExpressRoute circuit to connect back to your on-premises network.</span></span> <span data-ttu-id="ea12c-138">Egy részleghez (ebben a példában: informatikai) az ExpressRoute-kapcsolatcsoport rendelkezhet.</span><span class="sxs-lookup"><span data-stu-id="ea12c-138">A single department (in this example: IT) can own the ExpressRoute circuit.</span></span> <span data-ttu-id="ea12c-139">A szervezeten belüli más előfizetések használhatja az ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="ea12c-139">Other subscriptions within the organization can use the ExpressRoute circuit.</span></span>

> [!NOTE]
> <span data-ttu-id="ea12c-140">A dedikált kör kapcsolat és a sávszélesség költségek az ExpressRoute-kapcsolatcsoport tulajdonosát alkalmazandó.</span><span class="sxs-lookup"><span data-stu-id="ea12c-140">Connectivity and bandwidth charges for the dedicated circuit will be applied to the ExpressRoute Circuit Owner.</span></span> <span data-ttu-id="ea12c-141">Az összes virtuális hálózatot megosztani a azonos sávszélességet.</span><span class="sxs-lookup"><span data-stu-id="ea12c-141">All virtual networks share the same bandwidth.</span></span>
> 
> 

![Az előfizetések közötti kapcsolathoz](./media/expressroute-howto-linkvnet-classic/cross-subscription.png)

### <a name="administration---circuit-owners-and-circuit-users"></a><span data-ttu-id="ea12c-143">Felügyeleti - áramkör tulajdonosa és a kapcsolatcsoport felhasználók</span><span class="sxs-lookup"><span data-stu-id="ea12c-143">Administration - Circuit Owners and Circuit Users</span></span>

<span data-ttu-id="ea12c-144">A kapcsolatcsoport tulajdonosát a ExpressRoute-kapcsolatcsoport erőforrás Power jogosultsága.</span><span class="sxs-lookup"><span data-stu-id="ea12c-144">The 'Circuit Owner' is an authorized Power User of the ExpressRoute circuit resource.</span></span> <span data-ttu-id="ea12c-145">A kapcsolatcsoport tulajdonosát hozhat létre, amely a "Kör felhasználók" is váltható engedélyek.</span><span class="sxs-lookup"><span data-stu-id="ea12c-145">The Circuit Owner can create authorizations that can be redeemed by 'Circuit Users'.</span></span> <span data-ttu-id="ea12c-146">Kör felhasználók, amelyek nem tartoznak a ExpressRoute-kapcsolatcsoportot tárolóként ugyanazt az előfizetést virtuális hálózati átjárók tulajdonosai.</span><span class="sxs-lookup"><span data-stu-id="ea12c-146">Circuit Users are owners of virtual network gateways that are not within the same subscription as the ExpressRoute circuit.</span></span> <span data-ttu-id="ea12c-147">Kör felhasználók is beválthatja engedélyek (virtuális hálózatonként egy engedélyezési).</span><span class="sxs-lookup"><span data-stu-id="ea12c-147">Circuit Users can redeem authorizations (one authorization per virtual network).</span></span>

<span data-ttu-id="ea12c-148">A kapcsolatcsoport tulajdonosát a rendelkezik módosítja, és bármikor engedélyek visszavonása.</span><span class="sxs-lookup"><span data-stu-id="ea12c-148">The Circuit Owner has the power to modify and revoke authorizations at any time.</span></span> <span data-ttu-id="ea12c-149">Amikor visszavon egy engedélyezési, minden kapcsolatoknál törlődnek az előfizetésből, amelyek hozzáférését visszavonták.</span><span class="sxs-lookup"><span data-stu-id="ea12c-149">When an authorization is revoked, all link connections are deleted from the subscription whose access was revoked.</span></span>

### <a name="circuit-owner-operations"></a><span data-ttu-id="ea12c-150">Kapcsolatcsoport-tulajdonos műveletek</span><span class="sxs-lookup"><span data-stu-id="ea12c-150">Circuit Owner operations</span></span>

<span data-ttu-id="ea12c-151">**Az engedély létrehozása**</span><span class="sxs-lookup"><span data-stu-id="ea12c-151">**To create an authorization**</span></span>

<span data-ttu-id="ea12c-152">A kapcsolatcsoport tulajdonosát létrehoz egy engedélyezésre, amelynek segítségével a kapcsolatcsoport felhasználó csatlakozzon a virtuális hálózati átjárók az ExpressRoute-kapcsolatcsoport engedélykulcs hoz létre.</span><span class="sxs-lookup"><span data-stu-id="ea12c-152">The Circuit Owner creates an authorization, which creates an authorization key that can be used by a Circuit User to connect their virtual network gateways to the ExpressRoute circuit.</span></span> <span data-ttu-id="ea12c-153">Az engedély csak egy kapcsolat érvénytelen.</span><span class="sxs-lookup"><span data-stu-id="ea12c-153">An authorization is valid for only one connection.</span></span>

<span data-ttu-id="ea12c-154">A következő példa bemutatja, hogyan hozzon létre egy engedélyezési:</span><span class="sxs-lookup"><span data-stu-id="ea12c-154">The following example shows how to create an authorization:</span></span>

```azurecli
az network express-route auth create --circuit-name MyCircuit -g ExpressRouteResourceGroup -n MyAuthorization
```

<span data-ttu-id="ea12c-155">A válasz tartalmazza az engedélyezési kulcsot és annak állapotát:</span><span class="sxs-lookup"><span data-stu-id="ea12c-155">The response contains the authorization key and status:</span></span>

```azurecli
"authorizationKey": "0a7f3020-541f-4b4b-844a-5fb43472e3d7",
"authorizationUseStatus": "Available",
"etag": "W/\"010353d4-8955-4984-807a-585c21a22ae0\"",
"id": "/subscriptions/81ab786c-56eb-4a4d-bb5f-f60329772466/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/MyCircuit/authorizations/MyAuthorization1",
"name": "MyAuthorization1",
"provisioningState": "Succeeded",
"resourceGroup": "ExpressRouteResourceGroup"
```

<span data-ttu-id="ea12c-156">**Engedélyek áttekintése**</span><span class="sxs-lookup"><span data-stu-id="ea12c-156">**To review authorizations**</span></span>

<span data-ttu-id="ea12c-157">A kapcsolatcsoport tulajdonosát futtassa a következő példa egy adott körön kiállított összes engedélyek tekinthetők át:</span><span class="sxs-lookup"><span data-stu-id="ea12c-157">The Circuit Owner can review all authorizations that are issued on a particular circuit by running the following example:</span></span>

```azurecli
az network express-route auth list --circuit-name MyCircuit -g ExpressRouteResourceGroup
```

<span data-ttu-id="ea12c-158">**Engedélyek hozzáadása**</span><span class="sxs-lookup"><span data-stu-id="ea12c-158">**To add authorizations**</span></span>

<span data-ttu-id="ea12c-159">A kapcsolatcsoport tulajdonosát az alábbi példa használatával adhat hozzá engedélyek:</span><span class="sxs-lookup"><span data-stu-id="ea12c-159">The Circuit Owner can add authorizations by using the following example:</span></span>

```azurecli
az network express-route auth create --circuit-name MyCircuit -g ExpressRouteResourceGroup -n MyAuthorization1
```

<span data-ttu-id="ea12c-160">**Engedélyek törlése**</span><span class="sxs-lookup"><span data-stu-id="ea12c-160">**To delete authorizations**</span></span>

<span data-ttu-id="ea12c-161">A kapcsolatcsoport tulajdonosát is visszavonni vagy törlése a felhasználó engedélyek futtassa az alábbi példa:</span><span class="sxs-lookup"><span data-stu-id="ea12c-161">The Circuit Owner can revoke/delete authorizations to the user by running the following example:</span></span>

```azurecli
az network express-route auth delete --circuit-name MyCircuit -g ExpressRouteResourceGroup -n MyAuthorization1
```

### <a name="circuit-user-operations"></a><span data-ttu-id="ea12c-162">Kör felhasználói műveletek</span><span class="sxs-lookup"><span data-stu-id="ea12c-162">Circuit User operations</span></span>

<span data-ttu-id="ea12c-163">A kapcsolatcsoport-felhasználó a társ-Azonosítóval és a engedélykulcs a kapcsolatcsoport tulajdonosát a kell.</span><span class="sxs-lookup"><span data-stu-id="ea12c-163">The Circuit User needs the peer ID and an authorization key from the Circuit Owner.</span></span> <span data-ttu-id="ea12c-164">A hitelesítési kulcs egy GUID Azonosítót.</span><span class="sxs-lookup"><span data-stu-id="ea12c-164">The authorization key is a GUID.</span></span>

```azurecli
Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
```

<span data-ttu-id="ea12c-165">**Egy kapcsolat hitelesítési beváltani**</span><span class="sxs-lookup"><span data-stu-id="ea12c-165">**To redeem a connection authorization**</span></span>

<span data-ttu-id="ea12c-166">A kapcsolatcsoport felhasználó futtathatja a következő példa egy hivatkozás hitelesítési beváltani:</span><span class="sxs-lookup"><span data-stu-id="ea12c-166">The Circuit User can run the following example to redeem a link authorization:</span></span>

```azurecli
az network vpn-connection create --name ERConnection --resource-group ExpressRouteResourceGroup --vnet-gateway1 VNet1GW --express-route-circuit2 MyCircuit --authorization-key "^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^"
```

<span data-ttu-id="ea12c-167">**Egy kapcsolat hitelesítési felszabadítása**</span><span class="sxs-lookup"><span data-stu-id="ea12c-167">**To release a connection authorization**</span></span>

<span data-ttu-id="ea12c-168">Törölni kell a kapcsolat az ExpressRoute-kapcsolatcsoport a virtuális hálózatra hivatkozó engedélyezési is megjelenhetnek.</span><span class="sxs-lookup"><span data-stu-id="ea12c-168">You can release an authorization by deleting the connection that links the ExpressRoute circuit to the virtual network.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ea12c-169">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ea12c-169">Next steps</span></span>

<span data-ttu-id="ea12c-170">További információ az ExpressRoute-tal kapcsolatban: [ExpressRoute – Gyakori kérdések](expressroute-faqs.md).</span><span class="sxs-lookup"><span data-stu-id="ea12c-170">For more information about ExpressRoute, see the [ExpressRoute FAQ](expressroute-faqs.md).</span></span>