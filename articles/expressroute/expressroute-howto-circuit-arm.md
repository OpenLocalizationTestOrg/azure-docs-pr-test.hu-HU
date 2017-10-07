---
title: "Létrehozásához és módosításához ExpressRoute-kapcsolatcsoportot: PowerShell: Azure Resource Manager |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan toocreate, kiépítéséhez, győződjön meg arról, frissítése, törlése és kiosztásának megszüntetése ExpressRoute-kapcsolatcsoportot."
documentationcenter: na
services: expressroute
author: ganesr
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f997182e-9b25-4a7a-b079-b004221dadcc
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/12/2017
ms.author: ganesr;cherylmc
ms.openlocfilehash: 8d76c577a9cffdd393abac1b76cccc27d92e9e62
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-modify-an-expressroute-circuit-using-powershell"></a>Létrehozása és módosítása a powershellel ExpressRoute-kapcsolatcsoportot
> [!div class="op_single_selector"]
> * [Azure Portal](expressroute-howto-circuit-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-circuit-arm.md)
> * [Azure CLI](howto-circuit-cli.md)
> * [Video - Azure-portálon](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit)
> * [PowerShell (klasszikus)](expressroute-howto-circuit-classic.md)
>

Ez a cikk ismerteti, hogyan toocreate egy Azure ExpressRoute áramkör PowerShell-parancsmagok és hello Azure Resource Manager telepítési modell használatával. Ez a cikk hogyan hello kapcsolatcsoport állapotának toocheck hello a frissítést, vagy törölje és kiosztásának megszüntetése azt is bemutatja.

## <a name="before-you-begin"></a>Előkészületek
* Hello hello Azure Resource Manager PowerShell-parancsmagok legújabb verziójának telepítéséhez. További információkért lásd: [áttekintés az Azure PowerShell](/powershell/azure/overview).
* Felülvizsgálati hello [Előfeltételek](expressroute-prerequisites.md) és [munkafolyamatok](expressroute-workflows.md) konfigurálás elkezdése előtt.


## <a name="create-and-provision-an-expressroute-circuit"></a>Hozzon létre, és helyezze üzembe az ExpressRoute-kapcsolatcsoportot
### <a name="1-sign-in-tooyour-azure-account-and-select-your-subscription"></a>1. Jelentkezzen be Azure-fiók tooyour, és jelölje ki az előfizetését
toobegin a konfigurációt, jelentkezzen be Azure-fiók tooyour. A következő példák toohelp csatlakozás hello használata:

```powershell
Login-AzureRmAccount
```

Ellenőrizze a hello előfizetések hello fiók:

```powershell
Get-AzureRmSubscription
```

Válassza ki egy ExpressRoute-áramkör a toocreate hello előfizetést:

```powershell
Select-AzureRmSubscription -SubscriptionId "<subscription ID>"
```

### <a name="2-get-hello-list-of-supported-providers-locations-and-bandwidths"></a>2. Támogatott szolgáltatók, a helyek és a sávszélesség hello listájának beolvasása
ExpressRoute-kapcsolatcsoportot létrehozni, meg kell hello támogatott kapcsolat szolgáltatókat, a helyek és a sávszélesség-beállítások listája.

PowerShell-parancsmag hello **Get-AzureRmExpressRouteServiceProvider** adja vissza ezt az információt fogja használni a későbbi lépésekben:

```powershell
Get-AzureRmExpressRouteServiceProvider
```

Ellenőrizze a toosee, ha a kapcsolat szolgáltatójánál van szó. Jegyezze fel a következő információ hello. Azt később szüksége expressroute-kapcsolatcsoporthoz létrehozásakor.

* Név
* PeeringLocations
* BandwidthsOffered

Most már készen áll a toocreate ExpressRoute-kapcsolatcsoportot.   

### <a name="3-create-an-expressroute-circuit"></a>3. ExpressRoute-kapcsolatcsoport létrehozása
Ha még nem rendelkezik egy erőforráscsoport, kell létrehoznia egyet az ExpressRoute-kapcsolatcsoportot létrehozása előtt. Ezt hello a következő parancs futtatásával teheti meg:

```powershell
New-AzureRmResourceGroup -Name "ExpressRouteResourceGroup" -Location "West US"
```


hello a következő példa bemutatja, hogyan toocreate egy 200 MB/s ExpressRoute áramkör a szilícium Valley Equinix keresztül. Különböző szolgáltatók és más beállítások használata, amikor a kérést helyettesítse be ezt az információt. hello az alábbiakban látható egy példa egy kérelem egy új szolgáltatás kulcs:

```powershell
New-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup" -Location "West US" -SkuTier Standard -SkuFamily MeteredData -ServiceProviderName "Equinix" -PeeringLocation "Silicon Valley" -BandwidthInMbps 200
```

Győződjön meg arról, hogy megadja hello megfelelő SKU réteg és SKU-család:

* Termékváltozat szint határozza meg, egy ExpressRoute standard vagy ExpressRoute prémium bővítmény engedélyezve van-e. Megadhat *szabványos* tooget hello standard Termékváltozat vagy *prémium* hello prémium bővítménnyel.
* SKU-család hello számlázási típusa határozza meg. Megadhat *Metereddata* díjköteles adatforgalom tervhez és *Unlimiteddata* egy adatforgalmi a. Módosíthatja a számlázási típusának hello *Metereddata* túl*Unlimiteddata*, de nem módosíthatja a hello típusát *Unlimiteddata* túl*Metereddata *.

> [!IMPORTANT]
> Az ExpressRoute-kapcsolatcsoportot jelenik meg a szolgáltatási kulcs hello pillanattól lesz terhelve. Győződjön meg arról, hogy készen áll a tooprovision hello áramkör hello kapcsolat szolgáltatójánál esetén elvégzi ezt a műveletet.
> 
> 

hello válasz hello szolgáltatás kulcsot tartalmaz. Részletes leírását, az összes hello paraméterek kaphat hello a következő parancs futtatásával:

```powershell
get-help New-AzureRmExpressRouteCircuit -detailed
```


### <a name="4-list-all-expressroute-circuits"></a>4. A lista összes ExpressRoute-Kapcsolatcsoportok
az összes hello hozott létre, ExpressRoute-Kapcsolatcsoportok tooget futtatása hello **Get-AzureRmExpressRouteCircuit** parancs:

```powershell
Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
```

hello válasz a következő példa hasonló toohello fog kinézni:

    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                         "Tier": "Standard",
                                         "Family": "MeteredData"
                                          }
    CircuitProvisioningState          : Enabled
    ServiceProviderProvisioningState  : NotProvisioned
    ServiceProviderNotes              :
    ServiceProviderProperties         : {
                                          "ServiceProviderName": "Equinix",
                                          "PeeringLocation": "Silicon Valley",
                                          "BandwidthInMbps": 200
                                        }
    ServiceKey                        : **************************************
    Peerings                          : []

Ezt az információt bármikor hello használatával lekérhető `Get-AzureRmExpressRouteCircuit` parancsmag. Összes hello áramkör paraméter nélküli hívás hello így sorolja fel. A szolgáltatás kulcs megjelenik hello *ServiceKey* mező:

```powershell
Get-AzureRmExpressRouteCircuit
```


hello válasz a következő példa hasonló toohello fog kinézni:

    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                         "Tier": "Standard",
                                         "Family": "MeteredData"
                                          }
    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : NotProvisioned
    ServiceProviderNotes             :
    ServiceProviderProperties        : {
                                         "ServiceProviderName": "Equinix",
                                         "PeeringLocation": "Silicon Valley",
                                         "BandwidthInMbps": 200
                                          }
    ServiceKey                       : **************************************
    Peerings                         : []


Részletes leírását, az összes hello paraméterek kaphat hello a következő parancs futtatásával:

```powershell
get-help Get-AzureRmExpressRouteCircuit -detailed
```

### <a name="5-send-hello-service-key-tooyour-connectivity-provider-for-provisioning"></a>5. Kapcsolat kulcs tooyour hello szolgáltató küldése történő üzembe helyezéséhez
*ServiceProviderProvisioningState* hello aktuális állapotának hello szolgáltatói oldalán kiépítési információkat nyújt. Állapot hello állapot hello Microsoft ügyféloldali biztosít. Kiépítés állapotok áramkör kapcsolatos további információkért lásd: hello [munkafolyamatok](expressroute-workflows.md#expressroute-circuit-provisioning-states) cikk.

Amikor létrehoz egy új ExpressRoute-kapcsolatcsoportot, hello áramkör hello állapota a következő lehet:

    ServiceProviderProvisioningState : NotProvisioned
    CircuitProvisioningState         : Enabled



hello áramkör toohello hello folyamatán, amely lehetővé teszi a hello kapcsolat hitelesítésszolgáltató esetén a következő állapota változik:

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled

Az Ön toobe képes toouse ExpressRoute-kapcsolatcsoportot az állapot a következő hello kell lennie:

    ServiceProviderProvisioningState : Provisioned
    CircuitProvisioningState         : Enabled

### <a name="6-periodically-check-hello-status-and-hello-state-of-hello-circuit-key"></a>6. Rendszeres időközönként ellenőrizze a hello állapotát és hello áramkör kulcs hello állapota
Hello állapotát és hello áramkör kulcs hello állapotának ellenőrzése lehetővé teszi a szolgáltató a kapcsolatcsoport engedélyezésekor. Hello áramkör konfigurálása után *ServiceProviderProvisioningState* jelenik meg *kiépítve*, ahogy az alábbi példa hello:

```powershell
Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
```


hello válasz a következő példa hasonló toohello fog kinézni:

    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                         "Tier": "Standard",
                                         "Family": "MeteredData"
                                       }
    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : Provisioned
    ServiceProviderNotes             :
    ServiceProviderProperties        : {
                                         "ServiceProviderName": "Equinix",
                                         "PeeringLocation": "Silicon Valley",
                                         "BandwidthInMbps": 200
                                       }
    ServiceKey                       : **************************************
    Peerings                         : []

### <a name="7-create-your-routing-configuration"></a>7. Az útválasztó-konfiguráció létrehozása
Részletes útmutatásért lásd: hello [ExpressRoute-áramkör útválasztási konfigurációja](expressroute-howto-routing-arm.md) toocreate a következő cikket, és módosítsa a kapcsolatcsoport esetében.

> [!IMPORTANT]
> Ezek az utasítások csak a szolgáltatók által biztosított réteg 2 internetkapcsolati szolgáltatás használatával létrehozott toocircuits vonatkoznak. A szolgáltató által kezelt használata réteg (általában az IP VPN, például az MPLS) 3 szolgáltatások, a kapcsolat szolgáltatójánál konfigurálása és kezelése az Ön útválasztást.
> 
> 

### <a name="8-link-a-virtual-network-tooan-expressroute-circuit"></a>8. Hivatkozásra egy virtuális hálózati tooan ExpressRoute-kapcsolatcsoportot
A következő hivatkozás egy virtuális hálózati tooyour ExpressRoute-kapcsolatcsoportot. Használjon hello [csatolása a virtuális hálózatok tooExpressRoute Kapcsolatcsoportok](expressroute-howto-linkvnet-arm.md) hello Resource Manager üzembe helyezési modellben való munka során a következő cikket.

## <a name="getting-hello-status-of-an-expressroute-circuit"></a>Egy ExpressRoute-kapcsolatcsoportot hello állapotának beolvasása
Ezt az információt bármikor hello használatával lekérhető **Get-AzureRmExpressRouteCircuit** parancsmag. Összes hello áramkör paraméter nélküli hívás hello így sorolja fel.

```powershell
Get-AzureRmExpressRouteCircuit
```


hello válasz hasonló toohello, például a következő lesz:

    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                         "Tier": "Standard",
                                         "Family": "MeteredData"
                                       }
    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : Provisioned
    ServiceProviderNotes             :
    ServiceProviderProperties        : {
                                            "ServiceProviderName": "Equinix",
                                            "PeeringLocation": "Silicon Valley",
                                            "BandwidthInMbps": 200
                                          }
    ServiceKey                       : **************************************
    Peerings                         : []


Egy adott ExpressRoute-kapcsolatcsoportot tájékoztatást kaphat a paraméter toohello hívásként hello erőforráscsoport-név és a kapcsolat neve átadásával:

```powershell
Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
```


hello válasz a következő példa hasonló toohello fog kinézni:

    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                            "Tier": "Standard",
                                            "Family": "MeteredData"
                                          }
    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : Provisioned
    ServiceProviderNotes             :
    ServiceProviderProperties        : {
                                         "ServiceProviderName": "Equinix",
                                         "PeeringLocation": "Silicon Valley",
                                         "BandwidthInMbps": 200
                                          }
    ServiceKey                       : **************************************
    Peerings                         : []


Részletes leírását, az összes hello paraméterek kaphat hello a következő parancs futtatásával:

```powershell
get-help get-azurededicatedcircuit -detailed
```

## <a name="modify"></a>Egy ExpressRoute-kapcsolatcsoportot módosítása
ExpressRoute-kapcsolatcsoportot egyes tulajdonságait módosíthatja kapcsolat befolyásolása nélkül.

Mindent hello állásidő nélkül a következő:

* Engedélyezi vagy letiltja az ExpressRoute-kapcsolatcsoportot prémium ExpressRoute bővítményt.
* Az ExpressRoute-kapcsolatcsoportot növekedése hello sávszélesség megadott érhető el kapacitás hello porton. Alacsonyabb verziójúra változtatása hello sávszélesség, a kapcsolat nem támogatott. 
* Hello mérési adatok díjköteles tooUnlimited adatokat a terv módosítása Korlátlan adatforgalom tooMetered adatok nem támogatott a terv mérési hello módosítása.
* Engedélyezheti és letilthatja *klasszikus műveletek engedélyezése*.

További információ a korlátai és korlátozásai, tekintse meg a toohello [ExpressRoute – gyakori kérdések](expressroute-faqs.md).

### <a name="tooenable-hello-expressroute-premium-add-on"></a>tooenable hello ExpressRoute prémium szintű bővítmény
Hello ExpressRoute prémium szintű bővítmény a következő PowerShell-részlet hello segítségével engedélyezheti a meglévő kapcsolat:

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

$ckt.Sku.Tier = "Premium"
$ckt.sku.Name = "Premium_MeteredData"

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

hello áramkör most kell hello ExpressRoute prémium bővítmény szolgáltatások engedélyezve van. Kezdjük el számlázási meg hello prémium bővítmény képességhez, amint hello parancs sikeresen lefutott.

### <a name="toodisable-hello-expressroute-premium-add-on"></a>toodisable hello ExpressRoute prémium szintű bővítmény
> [!IMPORTANT]
> Ez a művelet sikertelen lehet erőforrásokat, amelyek nagyobbak, mint a megengedett hello szabványos kapcsolat használata.
> 
> 

Vegye figyelembe a következőket hello:

* Ahhoz, hogy megállapításában, a prémium szintű toostandard, győződjön meg arról, hogy kapcsolt virtuális hálózatok hello száma toohello áramkör érték kisebb, mint 10. Ha nem így tesz, a frissítési kérelem sikertelen lesz, és azt prémium ütemben számlázza meg.
* Minden virtuális hálózat más geopolitikai régiókban kell választható. Ha nem így tesz, a frissítési kérelem sikertelen lesz, és azt prémium ütemben számlázza meg.
* Az útvonaltábla a magánhálózati társviszony-létesítés kisebb, mint 4000 útvonalait kell lennie. Az útvonal tábla mérete nagyobb, mint 4000 útvonalakat, ha a hello BGP-munkamenetet esik, és nem fog újra engedélyezve, amíg a hirdetett hello számához nem 4000 éri el.

Letilthatja a hello ExpressRoute prémium szintű bővítmény hello meglévő kör hello a következő PowerShell-parancsmag segítségével:

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

$ckt.Sku.Tier = "Standard"
$ckt.sku.Name = "Standard_MeteredData"

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

### <a name="tooupdate-hello-expressroute-circuit-bandwidth"></a>tooupdate hello ExpressRoute-kapcsolatcsoport sávszélessége
A támogatott sávszélesség-beállításokat a szolgáltató, ellenőrizze a hello [ExpressRoute – gyakori kérdések](expressroute-faqs.md). Kiválaszthatja a mérete nagyobb, mint a meglévő expressroute hello méretét.

> [!IMPORTANT]
> Előfordulhat, hogy toorecreate hello ExpressRoute-kapcsolatcsoportot esetén nincs elég kapacitás hello meglévő porton. Hello kapcsolatcsoport nem frissíthető, ha nincsenek további kapacitást érhető el az adott helyhez.
>
> Nem csökken a hello sávszélesség az ExpressRoute-kapcsolatcsoportot megszakítása nélkül. Alacsonyabb verziójúra változtatása sávszélesség toodeprovision hello ExpressRoute-kapcsolatcsoportot meg, és majd építenie az új ExpressRoute-kapcsolatcsoportot.
> 

Miután eldöntötte, hogy milyen van szüksége, használja a következő parancs tooresize hello a kapcsolatcsoport mérete:

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

$ckt.ServiceProviderProperties.BandwidthInMbps = 1000

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```


A kapcsolatcsoport fog hello Microsoft oldalán kell méretezni. Ezután vegye fel a kapcsolatot a kapcsolat szolgáltató tooupdate konfigurációit, az ügyféloldali toomatch ezt a módosítást. Miután elvégezte ezt az értesítést, kezdjük el számlázást, a frissített hello sávszélesség beállítást.

### <a name="toomove-hello-sku-from-metered-toounlimited"></a>toomove hello SKU a mért toounlimited
A következő PowerShell-részlet hello segítségével módosíthatja egy ExpressRoute-kapcsolatcsoport Termékváltozata hello:

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

$ckt.Sku.Family = "UnlimitedData"
$ckt.sku.Name = "Premium_UnlimitedData"

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

### <a name="toocontrol-access-toohello-classic-and-resource-manager-environments"></a>toocontrol hozzáférés toohello klasszikus és Resource Manager-környezetben
Tekintse át a hello utasításait [áthelyezése ExpressRoute-Kapcsolatcsoportok hello klasszikus toohello Resource Manager üzembe helyezési modellben a](expressroute-howto-move-arm.md).  

## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a>Megszüntetés és az ExpressRoute-kapcsolatcsoport törlése
Vegye figyelembe a következőket hello:

* Az ExpressRoute-kapcsolatcsoportot hello összes virtuális hálózatot kell választható. Ha ez a művelet nem sikerül, ellenőrizze a toosee, ha a virtuális hálózatok kapcsolódó toohello áramkör.
* Ha hello ExpressRoute körön szolgáltató üzembe helyezési állapota **kiépítési** vagy **kiépítve** együttműködve kell a service provider toodeprovision hello kapcsolatcsoport az oldalon. Rendszer továbbra is tooreserve erőforrások és kiszámlázni Önnek, amíg hello szolgáltató megszüntetési hello áramkör befejeződik, és értesíti a számunkra.
* Ha hello szolgáltató rendelkezik platformelőfizetés hello áramkör (üzembe helyezési állapota hello szolgáltató értéke túl**nincs kiépítve**) hello áramkör törölhet. Ezzel leállítja a számlázási hello kör

Az ExpressRoute-kapcsolatcsoport törlése hello a következő parancs futtatásával:

```powershell
Remove-AzureRmExpressRouteCircuit -ResourceGroupName "ExpressRouteResourceGroup" -Name "ExpressRouteARMCircuit"
```

## <a name="next-steps"></a>Következő lépések

Miután létrehozta a kapcsolatcsoport, győződjön meg arról, hogy Ön hello a következő:

* [Létrehozásához és módosításához az ExpressRoute-kapcsolatcsoport esetében routing](expressroute-howto-routing-arm.md)
* [A virtuális hálózati tooyour ExpressRoute-kapcsolatcsoportot hivatkozás](expressroute-howto-linkvnet-arm.md)
