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
# <a name="connect-a-virtual-network-tooan-expressroute-circuit"></a>Csatlakozás a virtuális hálózati tooan ExpressRoute-kapcsolatcsoportot
> [!div class="op_single_selector"]
> * [Azure Portal](expressroute-howto-linkvnet-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-linkvnet-arm.md)
> * [Azure CLI](howto-linkvnet-cli.md)
> * [Video - Azure-portálon](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit)
> * [PowerShell (klasszikus)](expressroute-howto-linkvnet-classic.md)
>

Ez a cikk segítséget nyújt a virtuális hálózatokon (Vnetek) tooAzure ExpressRoute-Kapcsolatcsoportok hivatkozás hello Resource Manager üzembe helyezési modellben és a PowerShell használatával. Virtuális hálózatok hello lehet azonos az előfizetés vagy egy másik előfizetés részeként. Ez a cikk azt is bemutatja, hogyan tooupdate virtuális hálózat hivatkozásra. 

## <a name="before-you-begin"></a>Előkészületek
* Hello hello Azure PowerShell-modulok legújabb verziójának telepítéséhez. További információkért lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).
* Felülvizsgálati hello [Előfeltételek](expressroute-prerequisites.md), [útválasztási követelmények](expressroute-routing.md), és [munkafolyamatok](expressroute-workflows.md) konfigurálás elkezdése előtt.
* Egy aktív ExpressRoute-kapcsolatcsoportra lesz szüksége. 
  * Útmutatás alapján hello túl[ExpressRoute-kapcsolatcsoportot létrehozni](expressroute-howto-circuit-arm.md) és hello áramkör szerint a kapcsolat szolgáltatójánál engedélyezve van. 
  * Győződjön meg arról, hogy rendelkezik az Azure magánhálózati társviszony-létesítés a kapcsolatcsoport konfigurálva. Lásd: hello [konfigurálja az útválasztást](expressroute-howto-routing-arm.md) útválasztási miként. 
  * Győződjön meg arról, hogy az Azure magánhálózati társviszony-létesítés van konfigurálva, és hello BGP társviszony-létesítés a hálózat és a Microsoft között működik-e, hogy engedélyezheti a végpontok közötti kapcsolat.
  * Győződjön meg arról, hogy a virtuális hálózat és a virtuális hálózati átjáró létrehozása, és teljesen kiépítve. Útmutatás alapján hello túl[hozzon létre egy virtuális hálózati átjáró ExpressRoute](expressroute-howto-add-gateway-resource-manager.md). Az ExpressRoute a virtuális hálózati átjáró nem VPN GatewayType "ExpressRoute" hello használja.

* Too10 virtuális hálózatok tooa szabványos ExpressRoute-kapcsolatcsoportot is csatolhatja. Az összes virtuális hálózatot kell hello geopolitikai régión szabványos ExpressRoute-kapcsolatcsoportot használatakor. 

* Csatolni a virtuális hálózatok hello geopolitikai területet hello ExpressRoute-kapcsolatcsoportot kívül, vagy csatlakoztassa a virtuális hálózatok tooyour ExpressRoute-kapcsolatcsoportot nagyobb számú hello ExpressRoute prémium szintű bővítmény vannak engedélyezve. Ellenőrizze a hello [– gyakori kérdések](expressroute-faqs.md) hello prémium szintű bővítmény olvashat.


## <a name="connect-a-virtual-network-in-hello-same-subscription-tooa-circuit"></a>A virtuális hálózat a hello azonos előfizetés tooa áramkör
A virtuális hálózati átjáró tooan ExpressRoute-kapcsolatcsoportot hello a következő parancsmag használatával képes kapcsolódni. Győződjön meg arról, hogy hello virtuális hálózati átjáró jön létre, és készen áll a linking hello parancsmag futtatása előtt:

```powershell
$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
$gw = Get-AzureRmVirtualNetworkGateway -Name "ExpressRouteGw" -ResourceGroupName "MyRG"
$connection = New-AzureRmVirtualNetworkGatewayConnection -Name "ERConnection" -ResourceGroupName "MyRG" -Location "East US" -VirtualNetworkGateway1 $gw -PeerId $circuit.Id -ConnectionType ExpressRoute
```

## <a name="connect-a-virtual-network-in-a-different-subscription-tooa-circuit"></a>Egy másik előfizetésben található tooa kapcsolat a virtuális hálózatot
Több előfizetés ExpressRoute-kapcsolatcsoportot lehet megosztani. a következő ábra hello mutatja egy egyszerű sematikus ExpressRoute-Kapcsolatcsoportok megosztási hogyan működik a több előfizetésekhez.

Hello kisebb felhők hello nagy felhőben egy szervezeten belül toodifferent részlegek tartozó használt toorepresent előfizetések. Egyes hello részlegek hello szervezeten belül használhatja a saját előfizetés üzembe helyezéséhez, de a szolgáltatásból egy ExpressRoute körön tooconnect hátsó tooyour a helyszíni hálózaton megoszthatja. Egy részleghez (ebben a példában: informatikai) is saját hello ExpressRoute-kapcsolatcsoportot. Hello szervezeten belüli más előfizetésekkel használható hello ExpressRoute-kapcsolatcsoportot.

> [!NOTE]
> Hello ExpressRoute-kapcsolatcsoportot kapcsolat és a sávszélesség költségekkel alkalmazott toohello előfizetés tulajdonosa lesz. Az összes virtuális hálózatot megosztása hello azonos sávszélesség.
> 
> 

![Az előfizetések közötti kapcsolathoz](./media/expressroute-howto-linkvnet-classic/cross-subscription.png)


### <a name="administration---circuit-owners-and-circuit-users"></a>Felügyeleti - áramkör tulajdonosa és a kapcsolatcsoport felhasználók

hello "kapcsolatcsoport tulajdonosát" hello ExpressRoute-kapcsolatcsoport erőforrás Power jogosultsága. hello kapcsolatcsoport tulajdonosát hozhat létre, amely a "kör felhasználók" is váltható engedélyek. Kör felhasználók tulajdonosai virtuális hálózati átjárók, amelyek nincsenek belül hello ugyanahhoz az előfizetéshez, ExpressRoute-kapcsolatcsoportot hello. Kör felhasználók is beválthatja engedélyek (virtuális hálózatonként egy engedélyezési).

hello kapcsolatcsoport tulajdonosát hello power toomodify és visszavonni az engedélyeket bármikor rendelkezik. Az összes hivatkozás kapcsolatok törlődnek, amelyek hozzáférését visszavonták hello előfizetésből engedélyezési eredményez visszavonása.

### <a name="circuit-owner-operations"></a>Kapcsolatcsoport-tulajdonos műveletek

**az engedély toocreate**

hello kapcsolatcsoport tulajdonosát engedélyezési hoz létre. Az eredmény lehet engedélykulcs hello létrehozását a virtuális hálózati átjárók toohello ExpressRoute-kapcsolatcsoportot egy kör felhasználói tooconnect használják. Az engedély csak egy kapcsolat érvénytelen.

a következő parancsmag kódrészletet mutat be hogyan hello toocreate engedélyezési:

```powershell
$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
Add-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit -Name "MyAuthorization1"
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $circuit

$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
$auth1 = Get-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit -Name "MyAuthorization1"
```


hello válasz toothis hello hitelesítési kulcs és az állapot fogja tartalmazni:

    Name                   : MyAuthorization1
    Id                     : /subscriptions/&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&/resourceGroups/ERCrossSubTestRG/providers/Microsoft.Network/expressRouteCircuits/CrossSubTest/authorizations/MyAuthorization1
    Etag                   : &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&& 
    AuthorizationKey       : ####################################
    AuthorizationUseStatus : Available
    ProvisioningState      : Succeeded



**tooreview engedélyek**

hello kapcsolatcsoport tulajdonosát hello a következő parancsmag futtatásával egy adott körön kiállított összes engedélyek tekinthetők át:

```powershell
$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
$authorizations = Get-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit
```

**tooadd engedélyek**

hello kapcsolatcsoport tulajdonosát hello a következő parancsmag használatával adhat hozzá engedélyek:

```powershell
$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
Add-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit -Name "MyAuthorization2"
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $circuit

$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
$authorizations = Get-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit
```

**toodelete engedélyek**

hello kapcsolatcsoport tulajdonosát képes visszavonni vagy törlése engedélyek toohello felhasználói hello a következő parancsmag futtatásával:

```powershell
Remove-AzureRmExpressRouteCircuitAuthorization -Name "MyAuthorization2" -ExpressRouteCircuit $circuit
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $circuit
```    

### <a name="circuit-user-operations"></a>Kör felhasználói műveletek

hello áramkör felhasználói hello társ-Azonosítóval és a hello kapcsolatcsoport tulajdonosát a engedélykulcs kell. hello engedélyezési kulcsa egy GUID Azonosítót.

Társ azonosító ellenőrizhetők a hello a következő parancsot:

```powershell
Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
```

**egy kapcsolat hitelesítési tooredeem**

hello áramkör felhasználó futtathatja a következő parancsmag tooredeem hello egy hivatkozás hitelesítési:

```powershell
$id = "/subscriptions/********************************/resourceGroups/ERCrossSubTestRG/providers/Microsoft.Network/expressRouteCircuits/MyCircuit"    
$gw = Get-AzureRmVirtualNetworkGateway -Name "ExpressRouteGw" -ResourceGroupName "MyRG"
$connection = New-AzureRmVirtualNetworkGatewayConnection -Name "ERConnection" -ResourceGroupName "RemoteResourceGroup" -Location "East US" -VirtualNetworkGateway1 $gw -PeerId $id -ConnectionType ExpressRoute -AuthorizationKey "^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^"
```

**egy kapcsolat hitelesítési toorelease**

Az engedély törölni kell, amely a hello ExpressRoute körön toohello virtuális hálózati kapcsolat hello is megjelenhetnek.

## <a name="modify-a-virtual-network-connection"></a>Módosítsa a virtuális hálózati kapcsolat
Frissítheti az egyes tulajdonságait a virtuális hálózati kapcsolat. 

**tooupdate hello kapcsolat súlyozás**

A virtuális hálózat csatlakoztatott toomultiple ExpressRoute-Kapcsolatcsoportok lehet. Egynél több ExpressRoute-kapcsolatcsoportot azonos előtag hello kaphat. az előtag szánt mely kapcsolat toosend forgalom toochoose, módosíthatja *routingweight értékének* kapcsolat. Forgalom küldi hello legmagasabb hello kapcsolatot *routingweight értékének*.

```powershell
$connection = Get-AzureRmVirtualNetworkGatewayConnection -Name "MyVirtualNetworkConnection" -ResourceGroupName "MyRG"
$connection.RoutingWeight = 100
Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection
```

számos hello *routingweight értékének* 0 too32000 van. hello alapértelmezett értéke 0.

## <a name="next-steps"></a>Következő lépések
ExpressRoute kapcsolatos további információkért lásd: hello [ExpressRoute – gyakori kérdések](expressroute-faqs.md).
