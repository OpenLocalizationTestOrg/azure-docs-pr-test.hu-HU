---
title: "Létrehozása és módosítása az Azure ExpressRoute-kapcsolatcsoportot: CLI |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan toocreate, kiépítéséhez, győződjön meg arról, frissítése, törlése és kiosztásának megszüntetése parancssori felület használatával ExpressRoute-kapcsolatcsoportot."
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/25/2017
ms.author: anzaman;cherylmc
ms.openlocfilehash: 396e325658a59afadb209bb525cbb59ac775ae6b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-modify-an-expressroute-circuit-using-cli"></a>Létrehozásához és módosításához az ExpressRoute-kapcsolatcsoportot parancssori felület használatával


Ez a cikk ismerteti, hogyan toocreate használatával Azure ExpressRoute-kapcsolatcsoportot hello parancssori felület (CLI). Ez a cikk is bemutatja, hogyan toocheck hello állapotát, frissítéséhez vagy törlése és expressroute-kapcsolatcsoporthoz kiosztásának megszüntetése. Ha azt szeretné, hogy egy másik módszer toowork az ExpressRoute-Kapcsolatcsoportok toouse, hello cikk választhat a következő lista hello:

> [!div class="op_single_selector"]
> * [Azure Portal](expressroute-howto-circuit-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-circuit-arm.md)
> * [Azure CLI](howto-circuit-cli.md)
> * [Video - Azure-portálon](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit)
> * [PowerShell (klasszikus)](expressroute-howto-circuit-classic.md)
> 

## <a name="before-you-begin"></a>Előkészületek

* Megkezdése előtt telepítse a legújabb verziójának hello hello parancssori felület parancsait (2.0-s vagy újabb). Hello parancssori felület parancsait telepítésével kapcsolatos információkért lásd: [Azure CLI 2.0 telepítése](/cli/azure/install-azure-cli) és [Ismerkedés az Azure CLI 2.0](/cli/azure/get-started-with-azure-cli).
* Felülvizsgálati hello [Előfeltételek](expressroute-prerequisites.md) és [munkafolyamatok](expressroute-workflows.md) konfigurálás elkezdése előtt.

## <a name="create-and-provision-an-expressroute-circuit"></a>Hozzon létre, és helyezze üzembe az ExpressRoute-kapcsolatcsoportot

### <a name="1-sign-in-tooyour-azure-account-and-select-your-subscription"></a>1. Jelentkezzen be Azure-fiók tooyour, és jelölje ki az előfizetését

toobegin a konfigurációt, jelentkezzen be Azure-fiók tooyour. A következő példák toohelp csatlakozás hello használata:

```azurecli
az login
```

Hello előfizetések hello fiók ellenőrzése.

```azurecli
az account list
```

Válassza ki a kívánt toocreate ExpressRoute-kapcsolatcsoportot hello előfizetést.

```azurecli
az account set --subscription "<subscription ID>"
```

### <a name="2-get-hello-list-of-supported-providers-locations-and-bandwidths"></a>2. Támogatott szolgáltatók, a helyek és a sávszélesség hello listájának beolvasása

ExpressRoute-kapcsolatcsoportot létrehozni, meg kell hello támogatott kapcsolat szolgáltatókat, a helyek és a sávszélesség-beállítások listája. hello CLI parancs "az hálózati expressroute lista--szolgáltatók" ezt az információt fogja használni a későbbi lépésekben adja vissza:

```azurecli
az network express-route list-service-providers
```

a rendszer a következő példa hasonló toohello hello választ:

```azurecli
[
  {
    "bandwidthsOffered": [
      {
        "offerName": "50Mbps",
        "valueInMbps": 50
      },
      {
        "offerName": "100Mbps",
        "valueInMbps": 100
      },
      {
        "offerName": "200Mbps",
        "valueInMbps": 200
      },
      {
        "offerName": "500Mbps",
        "valueInMbps": 500
      },
      {
        "offerName": "1Gbps",
        "valueInMbps": 1000
      },
      {
        "offerName": "2Gbps",
        "valueInMbps": 2000
      },
      {
        "offerName": "5Gbps",
        "valueInMbps": 5000
      },
      {
        "offerName": "10Gbps",
        "valueInMbps": 10000
      }
    ],
    "id": "/subscriptions//resourceGroups//providers/Microsoft.Network/expressRouteServiceProviders/",
    "location": null,
    "name": "AARNet",
    "peeringLocations": [
      "Melbourne",
      "Sydney"
    ],
    "provisioningState": "Succeeded",
    "resourceGroup": "",
    "tags": null,
    "type": "Microsoft.Network/expressRouteServiceProviders"
  },
```

Ellenőrizze a hello válasz toosee, ha a kapcsolat szolgáltatójánál szerepel. Jegyezze fel az adatokat, amelyeket a kapcsolat létrehozásakor kell a következő hello:

* Név
* PeeringLocations
* BandwidthsOffered

Most már készen áll a toocreate ExpressRoute-kapcsolatcsoportot.

### <a name="3-create-an-expressroute-circuit"></a>3. ExpressRoute-kapcsolatcsoport létrehozása

> [!IMPORTANT]
> Az ExpressRoute-kapcsolatcsoport ki egy szolgáltatási kulcs hello pillanattól lesz számlázva. Ehhez a művelethez készen tooprovision hello áramkör hello kapcsolat szolgáltató esetén.
> 
> 

Ha még nem rendelkezik egy erőforráscsoport, kell létrehoznia egyet az ExpressRoute-kapcsolatcsoportot létrehozása előtt. Létrehozhat egy erőforráscsoport hello a következő parancs futtatásával:

```azurecli
az group create -n ExpressRouteResourceGroup -l "West US"
```

hello a következő példa bemutatja, hogyan toocreate egy 200 MB/s ExpressRoute áramkör a szilícium Valley Equinix keresztül. Különböző szolgáltatók és más beállítások használata, amikor a kérést helyettesítse be ezt az információt. 

Győződjön meg arról, hogy megadja hello megfelelő SKU réteg és SKU-család:

* Termékváltozat szint határozza meg, egy ExpressRoute standard vagy ExpressRoute prémium bővítmény engedélyezve van-e. Megadhatja a "Standard" tooget hello standard Termékváltozat vagy "Prémium" hello prémium bővítménnyel.
* SKU-család hello számlázási típusa határozza meg. Megadhat "Metereddata" mért adatforgalmi és a "Unlimiteddata" egy adatforgalmi. Hello számlázási típusának "Metereddata"too'Unlimiteddata", de nem módosítható hello típusa"Unlimiteddata"too'Metereddata" módosíthatja.


Az ExpressRoute-kapcsolatcsoport ki egy szolgáltatási kulcs hello pillanattól lesz számlázva. a következő példa hello tulajdonképpen egy kérelem egy új szolgáltatás kulcs:

```azurecli
az network express-route create --bandwidth 200 -n MyCircuit --peering-location "Silicon Valley" -g ExpressRouteResourceGroup --provider "Equinix" -l "West US" --sku-family MeteredData --sku-tier Standard
```

hello válasz hello szolgáltatás kulcsot tartalmaz.

### <a name="4-list-all-expressroute-circuits"></a>4. A lista összes ExpressRoute-Kapcsolatcsoportok

minden hello ExpressRoute-Kapcsolatcsoportok létrehozott, listája tooget hello "az hálózati expressroute list" parancs futtatásával. Ez a parancs használatával ezt az információt bármikor kérheti le. toolist összes körök, paraméter nélküli hívás hello ellenőrizze.

```azurecli
az network express-route list
```

A szolgáltatás kulcs szerepel hello *ServiceKey* hello válasz mezőjében.

```azurecli
"allowClassicOperations": false,
"authorizations": [],
"circuitProvisioningState": "Enabled",
"etag": "W/\"1262c492-ffef-4a63-95a8-a6002736b8c4\"",
"gatewayManagerEtag": null,
"id": "/subscriptions/81ab786c-56eb-4a4d-bb5f-f60329772466/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/MyCircuit",
"location": "westus",
"name": "MyCircuit",
"peerings": [],
"provisioningState": "Succeeded",
"resourceGroup": "ExpressRouteResourceGroup",
"serviceKey": "1d05cf70-1db5-419f-ad86-1ca62c3c125b",
"serviceProviderNotes": null,
"serviceProviderProperties": {
  "bandwidthInMbps": 200,
  "peeringLocation": "Silicon Valley",
  "serviceProviderName": "Equinix"
},
"serviceProviderProvisioningState": "NotProvisioned",
"sku": {
  "family": "UnlimitedData",
  "name": "Standard_MeteredData",
  "tier": "Standard"
},
"tags": null,
"type": "Microsoft.Network/expressRouteCircuits]
```

Részletes leírását, az összes hello paraméterek kaphat futó hello parancs használatával hello "-h" paraméter.

```azurecli
az network express-route list -h
```

### <a name="5-send-hello-service-key-tooyour-connectivity-provider-for-provisioning"></a>5. Kapcsolat kulcs tooyour hello szolgáltató küldése történő üzembe helyezéséhez

"ServiceProviderProvisioningState" hello aktuális állapotának hello szolgáltatói oldalán kiépítési információkat biztosít. hello állapot hello állapot hello Microsoft ügyféloldali biztosít. További információkért lásd: hello [munkafolyamatok cikk](expressroute-workflows.md#expressroute-circuit-provisioning-states).

Amikor létrehoz egy új ExpressRoute-kapcsolatcsoportot, hello kapcsolatcsoport állapotát követő hello van:

```azurecli
"serviceProviderProvisioningState": "NotProvisioned"
"circuitProvisioningState": "Enabled"
```

hello áramkör módosítások toohello állapotát követő hello folyamatán, amely lehetővé teszi a hello kapcsolat hitelesítésszolgáltató esetén:

```azurecli
"serviceProviderProvisioningState": "Provisioning"
"circuitProvisioningState": "Enabled"
```

Az Ön toobe képes toouse ExpressRoute-kapcsolatcsoportot az állapot a következő hello kell lennie:

```azurecli
"serviceProviderProvisioningState": "Provisioned"
"circuitProvisioningState": "Enabled
```

### <a name="6-periodically-check-hello-status-and-hello-state-of-hello-circuit-key"></a>6. Rendszeres időközönként ellenőrizze a hello állapotát és hello áramkör kulcs hello állapota

Hello állapotát és hello áramkör kulcs hello állapotának ellenőrzése lehetővé teszi a szolgáltató a kapcsolatcsoport engedélyezésekor. Hello áramkör konfigurálása után "ServiceProviderProvisioningState" jelenik meg "Kiépítve", ahogy az alábbi példa hello:

```azurecli
az network express-route show --resource-group ExpressRouteResourceGroup --name MyCircuit
```

a rendszer a következő példa hasonló toohello hello választ:

```azurecli
"allowClassicOperations": false,
"authorizations": [],
"circuitProvisioningState": "Enabled",
"etag": "W/\"1262c492-ffef-4a63-95a8-a6002736b8c4\"",
"gatewayManagerEtag": null,
"id": "/subscriptions/81ab786c-56eb-4a4d-bb5f-f60329772466/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/MyCircuit",
"location": "westus",
"name": "MyCircuit",
"peerings": [],
"provisioningState": "Succeeded",
"resourceGroup": "ExpressRouteResourceGroup",
"serviceKey": "1d05cf70-1db5-419f-ad86-1ca62c3c125b",
"serviceProviderNotes": null,
"serviceProviderProperties": {
  "bandwidthInMbps": 200,
  "peeringLocation": "Silicon Valley",
  "serviceProviderName": "Equinix"
},
"serviceProviderProvisioningState": "NotProvisioned",
"sku": {
  "family": "UnlimitedData",
  "name": "Standard_MeteredData",
  "tier": "Standard"
},
"tags": null,
"type": "Microsoft.Network/expressRouteCircuits]
```

### <a name="7-create-your-routing-configuration"></a>7. Az útválasztó-konfiguráció létrehozása

Részletes útmutatásért lásd: hello [ExpressRoute-áramkör útválasztási konfigurációja](howto-routing-cli.md) toocreate a következő cikket, és módosítsa a kapcsolatcsoport esetében.

> [!IMPORTANT]
> Ezek az utasítások csak a szolgáltatók által biztosított réteg 2 internetkapcsolati szolgáltatás használatával létrehozott toocircuits vonatkoznak. A szolgáltató által kezelt használata réteg (általában az IP VPN, például az MPLS) 3 szolgáltatások, a kapcsolat szolgáltatójánál konfigurálja és kezeli az Ön útválasztást.
> 
> 

### <a name="8-link-a-virtual-network-tooan-expressroute-circuit"></a>8. Hivatkozásra egy virtuális hálózati tooan ExpressRoute-kapcsolatcsoportot

A következő hivatkozás egy virtuális hálózati tooyour ExpressRoute-kapcsolatcsoportot. Használjon hello [csatolása a virtuális hálózatok tooExpressRoute Kapcsolatcsoportok](howto-linkvnet-cli.md) cikk.

## <a name="modify"></a>Egy ExpressRoute-kapcsolatcsoportot módosítása

ExpressRoute-kapcsolatcsoportot egyes tulajdonságait módosíthatja kapcsolat befolyásolása nélkül. A következő módosításokat állásidő nélkül hello végezheti el:

* Engedélyezheti vagy ExpressRoute prémium bővítményeket tiltsa le az ExpressRoute-kapcsolatcsoport esetében.
* Feltéve, hogy kapacitás elérhető hello porton hello az ExpressRoute-kapcsolatcsoport sávszélessége növelhető. Alacsonyabb verziójúra változtatása hello sávszélesség, a kapcsolat azonban nem támogatott. 
* Díjköteles adatforgalom tooUnlimited adatok hello mérési terv módosítható. Korlátlan adatforgalom tooMetered adatok nem támogatott a terv mérési hello azonban módosítása.
* Engedélyezheti és letilthatja *klasszikus műveletek engedélyezése*.

További információ a korlátai és korlátozásai: hello [ExpressRoute – gyakori kérdések](expressroute-faqs.md).

### <a name="tooenable-hello-expressroute-premium-add-on"></a>tooenable hello ExpressRoute prémium szintű bővítmény

A meglévő kör hello ExpressRoute prémium szintű bővítmény engedélyezheti hello a következő parancs használatával:

```azurecli
az network express-route update -n MyCircuit -g ExpressRouteResourceGroup --sku-tier Premium
```

hello áramkör hello ExpressRoute prémium bővítmény engedélyezett szolgáltatások most már rendelkezik. Az első lépések számlázási meg hello prémium bővítmény képességhez, amint hello parancs sikeresen lefutott.

### <a name="toodisable-hello-expressroute-premium-add-on"></a>toodisable hello ExpressRoute prémium szintű bővítmény

> [!IMPORTANT]
> Ez a művelet sikertelen lehet erőforrásokat, amelyek nagyobbak, mint a megengedett hello szabványos kapcsolat használata.
> 
> 

Mielőtt hello ExpressRoute prémium bővítmény letiltása, a következő feltételek hello megismerése:

* Ahhoz, hogy megállapításában, a prémium szintű toostandard, győződjön meg arról, hogy rendelkezik-e legfeljebb 10 virtuális hálózatok csatolt toohello körön. Ha több mint 10, a frissítési kérelem sikertelen lesz, és a prémium ütemben számlázás meg.
* Minden virtuális hálózat más geopolitikai régiókban kell választható. Az összes virtuális hálózat nem választható, ha a frissítési kérelem sikertelen lesz, és a prémium ütemben számlázás meg.
* Az útvonaltábla a magánhálózati társviszony-létesítés kisebb, mint 4000 útvonalait kell lennie. Ha az útvonal tábla mérete nagyobb, mint 4000 útvonalakat, esik hello BGP-munkamenetet. hello munkamenet nem kell újra, amíg hirdetett előtagok hello száma nem éri el 4000 engedélyezve.

A következő példa hello segítségével letilthatja hello ExpressRoute prémium szintű bővítmény hello meglévő kapcsolat:

```azurecli
az network express-route update -n MyCircuit -g ExpressRouteResourceGroup --sku-tier Standard
```

### <a name="tooupdate-hello-expressroute-circuit-bandwidth"></a>tooupdate hello ExpressRoute-kapcsolatcsoport sávszélessége

Hello támogatott sávszélesség-beállítások a szolgáltató, ellenőrizze a hello [ExpressRoute – gyakori kérdések](expressroute-faqs.md). Kiválaszthatja a mérete nagyobb, mint a meglévő expressroute hello méretét.

> [!IMPORTANT]
> Ha nincs elég kapacitás hello meglévő porton, előfordulhat, hogy toorecreate hello ExpressRoute-kapcsolatcsoportot. Hello kapcsolatcsoport nem frissíthető, ha nincsenek további kapacitást érhető el az adott helyhez.
>
> Nem csökken a hello sávszélesség az ExpressRoute-kapcsolatcsoportot megszakítása nélkül. Alacsonyabb verziójúra változtatása sávszélesség meg toodeprovision hello ExpressRoute-kapcsolatcsoportot igényel, és majd építenie az új ExpressRoute-kapcsolatcsoportot.
>

Miután eldöntötte, hogy van szüksége, használja a következő parancs tooresize hello a kapcsolatcsoport hello mérete:

```azurecli
az network express-route update -n MyCircuit -g ExpressRouteResourceGroup --bandwidth 1000
```

A kör mérete hello Microsoft oldalán. Ezt követően vegye fel a kapcsolatot a kapcsolat szolgáltató tooupdate konfigurációit, az ügyféloldali toomatch ezt a módosítást. Miután elvégezte ezt az értesítést, az első lépések számlázást, a frissített hello sávszélesség beállítást.

### <a name="toomove-hello-sku-from-metered-toounlimited"></a>toomove hello SKU a mért toounlimited

A következő példa hello segítségével módosíthatja egy ExpressRoute-kapcsolatcsoport Termékváltozata hello:

```azurecli
az network express-route update -n MyCircuit -g ExpressRouteResourceGroup --sku-family UnlimitedData
```

### <a name="toocontrol-access-toohello-classic-and-resource-manager-environments"></a>toocontrol hozzáférés toohello klasszikus és Resource Manager-környezetben

Tekintse át a hello utasításait [áthelyezése ExpressRoute-Kapcsolatcsoportok hello klasszikus toohello Resource Manager üzembe helyezési modellben a](expressroute-howto-move-arm.md).

## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a>Megszüntetés és az ExpressRoute-kapcsolatcsoport törlése

toodeprovision és a delete, ExpressRoute-kapcsolatcsoportot tudja, hogy a következő feltételek hello:

* Az ExpressRoute-kapcsolatcsoportot hello összes virtuális hálózatot kell választható. Ha ez a művelet nem sikerül, ellenőrizze a toosee, ha a virtuális hálózatok kapcsolódó toohello áramkör.
* Ha hello ExpressRoute körön szolgáltató üzembe helyezési állapota **kiépítési** vagy **kiépítve**, együttműködve kell a service provider toodeprovision hello kapcsolatcsoport az oldalon. A Microsoft továbbra is tooreserve erőforrásokat, és kiszámlázni Önnek, amíg hello szolgáltató megszüntetési hello áramkör befejeződik, és értesítést küld nekünk.
* Ha hello szolgáltató rendelkezik platformelőfizetés hello kapcsolatcsoport törlése hello áramkör. Ha expressroute-kapcsolatcsoporthoz van platformelőfizetés, üzembe helyezési állapota hello szolgáltató túl van-e állítva**nincs kiépítve**. Ezzel leállítja a számlázási hello kör.

Az ExpressRoute-kapcsolatcsoport törlése hello a következő parancs futtatásával:

```azurecli
az network express-route delete  -n MyCircuit -g ExpressRouteResourceGroup
```

## <a name="next-steps"></a>Következő lépések

Miután létrehozta a kapcsolatcsoport, győződjön meg arról, hogy hello következő feladatokat:

* [Létrehozásához és módosításához az ExpressRoute-kapcsolatcsoport esetében routing](howto-routing-cli.md)
* [A virtuális hálózati tooyour ExpressRoute-kapcsolatcsoportot hivatkozás](howto-linkvnet-cli.md)
