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
# <a name="connect-a-virtual-network-tooan-expressroute-circuit-using-cli"></a>Csatlakozás a virtuális hálózati tooan ExpressRoute-kapcsolatcsoportot parancssori felület használatával

Ez a cikk segít csatolni a virtuális hálózatokon (Vnetek) ExpressRoute-Kapcsolatcsoportok tooAzure parancssori felület használatával. Azure parancssori felület használatával toolink hello virtuális hálózatok hello Resource Manager telepítési modell segítségével kell létrehozni. Azok a hello lehet ugyanazt az előfizetést, vagy egy másik előfizetés részeként. Ha egy másik módszer tooconnect toouse a VNet tooan ExpressRoute-kapcsolatcsoportot, cikk választhat a következő lista hello:

> [!div class="op_single_selector"]
> * [Azure Portal](expressroute-howto-linkvnet-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-linkvnet-arm.md)
> * [Azure CLI](howto-linkvnet-cli.md)
> * [Video - Azure-portálon](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit)
> * [PowerShell (klasszikus)](expressroute-howto-linkvnet-classic.md)
> 

## <a name="configuration-prerequisites"></a>Konfigurációs előfeltételek

* Hello hello parancssori felület (CLI) legújabb verziójára van szüksége. További információkért lásd: [Azure CLI 2.0 telepítése](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli).
* Tooreview hello kell [Előfeltételek](expressroute-prerequisites.md), [útválasztási követelmények](expressroute-routing.md), és [munkafolyamatok](expressroute-workflows.md) konfigurálás elkezdése előtt.
* Egy aktív ExpressRoute-kapcsolatcsoportra lesz szüksége. 
  * Útmutatás alapján hello túl[ExpressRoute-kapcsolatcsoportot létrehozni](howto-circuit-cli.md) és hello áramkör szerint a kapcsolat szolgáltatójánál engedélyezve van. 
  * Győződjön meg arról, hogy rendelkezik az Azure magánhálózati társviszony-létesítés a kapcsolatcsoport konfigurálva. Lásd: hello [konfigurálja az útválasztást](howto-routing-cli.md) útválasztási miként. 
  * Győződjön meg arról, hogy konfigurálva van-e az Azure magánhálózati társviszony-létesítés. hello BGP társviszony-létesítés a hálózat és a Microsoft között kell lennie be, hogy engedélyezheti a végpontok közötti kapcsolat.
  * Győződjön meg arról, hogy a virtuális hálózat és a virtuális hálózati átjáró létrehozása, és teljesen kiépítve. Útmutatás alapján hello túl[a virtuális hálózati átjáró konfigurálása ExpressRoute](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli). Lehet, hogy toouse `--gateway-type ExpressRoute`.

* Too10 virtuális hálózatok tooa szabványos ExpressRoute-kapcsolatcsoportot is csatolhatja. Az összes virtuális hálózatot kell hello geopolitikai régión szabványos ExpressRoute-kapcsolatcsoportot használatakor. 

* Ha hello ExpressRoute prémium bővítmény engedélyezéséhez egy virtuális hálózaton kívül hello geopolitikai területet hello ExpressRoute-kapcsolatcsoportot hivatkozásra, vagy csatlakoztassa a virtuális hálózatok tooyour ExpressRoute-kapcsolatcsoportot nagyobb számú. Prémium szintű bővítmény hello kapcsolatos további információkért lásd: hello [gyakran ismételt kérdések](expressroute-faqs.md).

## <a name="connect-a-virtual-network-in-hello-same-subscription-tooa-circuit"></a>A virtuális hálózat a hello azonos előfizetés tooa áramkör

A virtuális hálózati átjáró tooan ExpressRoute-kapcsolatcsoportot hello példa használatával képes kapcsolódni. Győződjön meg arról, hogy hello virtuális hálózati átjáró jön létre, és készen áll a linking hello parancs futtatása előtt.

```azurecli
az network vpn-connection create --name ERConnection --resource-group ExpressRouteResourceGroup --vnet-gateway1 VNet1GW --express-route-circuit2 MyCircuit
```

## <a name="connect-a-virtual-network-in-a-different-subscription-tooa-circuit"></a>Egy másik előfizetésben található tooa kapcsolat a virtuális hálózatot

Több előfizetés ExpressRoute-kapcsolatcsoportot lehet megosztani. hello az alábbi ábra egy egyszerű sematikus ExpressRoute-Kapcsolatcsoportok megosztási hogyan működik a több előfizetésekhez.

Hello kisebb felhők hello nagy felhőben egy szervezeten belül toodifferent részlegek tartozó használt toorepresent előfizetések. Egyes hello részlegek hello szervezeten belül használhatja a saját előfizetés üzembe helyezéséhez, de a szolgáltatásból egy ExpressRoute körön tooconnect hátsó tooyour a helyszíni hálózaton megoszthatja. Egy részleghez (ebben a példában: informatikai) is saját hello ExpressRoute-kapcsolatcsoportot. Hello szervezeten belüli más előfizetésekkel használható hello ExpressRoute-kapcsolatcsoportot.

> [!NOTE]
> Kapcsolat és a sávszélesség díjak dedikált hello kör lesz alkalmazott toohello ExpressRoute-kapcsolatcsoport tulajdonosát. Az összes virtuális hálózatot megosztása hello azonos sávszélesség.
> 
> 

![Az előfizetések közötti kapcsolathoz](./media/expressroute-howto-linkvnet-classic/cross-subscription.png)

### <a name="administration---circuit-owners-and-circuit-users"></a>Felügyeleti - áramkör tulajdonosa és a kapcsolatcsoport felhasználók

hello "Kapcsolatcsoport tulajdonosát" hello ExpressRoute-kapcsolatcsoport erőforrás Power jogosultsága. hello kapcsolatcsoport tulajdonosát hozhat létre, amely a "Kör felhasználók" is váltható engedélyek. Kör felhasználók tulajdonosai virtuális hálózati átjárók, amelyek nincsenek belül hello ugyanahhoz az előfizetéshez, ExpressRoute-kapcsolatcsoportot hello. Kör felhasználók is beválthatja engedélyek (virtuális hálózatonként egy engedélyezési).

hello kapcsolatcsoport tulajdonosát hello power toomodify és visszavonni az engedélyeket bármikor rendelkezik. Amikor visszavon egy engedélyezési, minden kapcsolatoknál törlődnek hello előfizetésből, amelyek hozzáférését visszavonták.

### <a name="circuit-owner-operations"></a>Kapcsolatcsoport-tulajdonos műveletek

**az engedély toocreate**

hello kapcsolatcsoport tulajdonosát az engedélyt, amely hoz létre, amelyek lehetnek engedélykulcs hoz létre a virtuális hálózati átjárók toohello ExpressRoute-kapcsolatcsoportot egy kör felhasználói tooconnect használják. Az engedély csak egy kapcsolat érvénytelen.

a következő példa azt mutatja meg hogyan hello toocreate engedélyezési:

```azurecli
az network express-route auth create --circuit-name MyCircuit -g ExpressRouteResourceGroup -n MyAuthorization
```

hello válasz tartalmazza, hello engedélyezési kulcsot és annak állapotát:

```azurecli
"authorizationKey": "0a7f3020-541f-4b4b-844a-5fb43472e3d7",
"authorizationUseStatus": "Available",
"etag": "W/\"010353d4-8955-4984-807a-585c21a22ae0\"",
"id": "/subscriptions/81ab786c-56eb-4a4d-bb5f-f60329772466/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/MyCircuit/authorizations/MyAuthorization1",
"name": "MyAuthorization1",
"provisioningState": "Succeeded",
"resourceGroup": "ExpressRouteResourceGroup"
```

**tooreview engedélyek**

hello kapcsolatcsoport tulajdonosát tekinthetők át egy adott körön fut a következő példa hello által kiállított összes engedélyek:

```azurecli
az network express-route auth list --circuit-name MyCircuit -g ExpressRouteResourceGroup
```

**tooadd engedélyek**

hello kapcsolatcsoport tulajdonosát a következő példa hello használatával adhat hozzá engedélyek:

```azurecli
az network express-route auth create --circuit-name MyCircuit -g ExpressRouteResourceGroup -n MyAuthorization1
```

**toodelete engedélyek**

hello kapcsolatcsoport tulajdonosát is visszavonni vagy törlése engedélyek toohello felhasználói hello például a következő futtatásával:

```azurecli
az network express-route auth delete --circuit-name MyCircuit -g ExpressRouteResourceGroup -n MyAuthorization1
```

### <a name="circuit-user-operations"></a>Kör felhasználói műveletek

hello kapcsolatcsoport-felhasználó a hello társ-Azonosítóval és a kapcsolatcsoport tulajdonosát hello engedélykulcs kell. hello engedélyezési kulcsa egy GUID Azonosítót.

```azurecli
Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
```

**egy kapcsolat hitelesítési tooredeem**

hello áramkör felhasználó futtathatja a következő példa tooredeem hello egy hivatkozás hitelesítési:

```azurecli
az network vpn-connection create --name ERConnection --resource-group ExpressRouteResourceGroup --vnet-gateway1 VNet1GW --express-route-circuit2 MyCircuit --authorization-key "^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^"
```

**egy kapcsolat hitelesítési toorelease**

Az engedély törölni kell, amely a hello ExpressRoute körön toohello virtuális hálózati kapcsolat hello is megjelenhetnek.

## <a name="next-steps"></a>Következő lépések

ExpressRoute kapcsolatos további információkért lásd: hello [ExpressRoute – gyakori kérdések](expressroute-faqs.md).
