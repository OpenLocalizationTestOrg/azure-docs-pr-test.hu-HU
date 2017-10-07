---
title: "Hogyan tooconfigure útválasztás (társviszony) az ExpressRoute-kapcsolatcsoportot: erőforrás-kezelő: PowerShell: Azure |} Microsoft Docs"
description: "Ez a cikk végigvezeti hello létrehozásához és a kiépítés hello saját, a nyilvános és a Microsoft társviszony-létesítést az ExpressRoute-kapcsolatcsoportot. Ez a cikk is bemutatja, hogyan toocheck hello állapotát, a frissítést, vagy a-kör társviszony törlése."
documentationcenter: na
services: expressroute
author: ganesr
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 0a036d51-77ae-4fee-9ddb-35f040fbdcdf
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: ganesr;cherylmc
ms.openlocfilehash: eb3ddf5c05a086ac3e22c64417e51381ef465921
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-modify-peering-for-an-expressroute-circuit-using-powershell"></a>Létrehozása és módosítása a powershellel ExpressRoute-kör társviszony

Ez a cikk segítséget nyújt a létrehozása és kezelése a PowerShell használatával hello Resource Manager üzembe helyezési modellel ExpressRoute-kapcsolatcsoportot útválasztási konfigurációja. Is hello állapota, update vagy delete ellenőrizze és kiosztásának megszüntetése ExpressRoute-kör társviszony. Ha egy másik módszer toowork toouse a kapcsolatcsoport rendelkező, jelölje be a cikk a következő lista hello:

> [!div class="op_single_selector"]
> * [Azure Portal](expressroute-howto-routing-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-routing-arm.md)
> * [Azure CLI](howto-routing-cli.md)
> * [Video - privát társviszony-létesítés](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-private-peering-for-your-expressroute-circuit)
> * [Video - nyilvános társviszony-létesítés](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-public-peering-for-your-expressroute-circuit)
> * [Videó – a Microsoft társviszony-létesítés](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-microsoft-peering-for-your-expressroute-circuit)
> * [PowerShell (klasszikus)](expressroute-howto-routing-classic.md)
> 

## <a name="configuration-prerequisites"></a>Konfigurációs előfeltételek

* Szüksége lesz Azure Resource Manager PowerShell-parancsmagok hello hello legújabb verzióját. További információkért lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview). 
* Győződjön meg arról, hogy átolvasta hello [Előfeltételek](expressroute-prerequisites.md) lap hello [útválasztási követelmények](expressroute-routing.md) lap és hello [munkafolyamatok](expressroute-workflows.md) lapon konfigurálás elkezdése előtt.
* Egy aktív ExpressRoute-kapcsolatcsoportra lesz szüksége. Útmutatás alapján hello túl[ExpressRoute-kapcsolatcsoportot létrehozni](expressroute-howto-circuit-arm.md) , és folytassa a kapcsolat szolgáltatójánál előtt által engedélyezett hello áramkör. hello ExpressRoute-kapcsolatcsoportot meg toobe képes toorun hello parancsmagok Ez a cikk a kiépített és engedélyezett állapotban kell lennie.

Ezek az utasítások csak a 2. rétegbeli kapcsolatot szolgáltatásokat nyújtó szolgáltatók létre toocircuits vonatkoznak. A szolgáltató által kezelt használata réteg (általában egy IPVPN, például az MPLS) 3 szolgáltatások, a kapcsolat szolgáltatójánál konfigurálása és kezelése az Ön útválasztást.

> [!IMPORTANT]
> A Microsoft jelenleg hirdetményt hello felügyeleti portálon keresztül szolgáltatók által konfigurált esetében. Dolgozunk azon, hogy hamarosan bevezethessük ezt a képességet. A BGP társviszony konfigurálása előtt ellenőrizze a szolgáltató.
> 
> 

Egy, két vagy akár mindhárom társviszony-létesítést (Azure privát, Azure nyilvános és Microsoft) is konfigurálhatja egy adott ExpressRoute-kapcsolatcsoportban. A társviszony-létesítéseket tetszőleges sorrendben konfigurálhatja. Azonban kell győződjön meg arról, hogy elvégezte-e minden társviszony-létesítési egyszerre csak egy hello konfigurációját. 

## <a name="azure-private-peering"></a>Azure privát társviszony-létesítés

Ez a szakasz segítséget nyújt a létrehozása, beolvasása, frissítése és törlése hello Azure privát társviszony-létesítési konfiguráció ExpressRoute-kapcsolatcsoportot.

### <a name="toocreate-azure-private-peering"></a>az Azure magánhálózati társviszony-létesítés toocreate

1. ExpressRoute hello PowerShell modul importálása.

  Telepítenie kell a hello legújabb PowerShell installer [PowerShell-galériában](http://www.powershellgallery.com/) és hello Azure Resource Manager modulok importálása hello PowerShell-munkamenetet a rendelés toostart hello ExpressRoute-parancsmagok használatával. Rendszergazdaként kell toorun PowerShell.

  ```powershell
  Install-Module AzureRM
  Install-AzureRM
  ```

  Importálja az összes hello AzureRM.* modulok hello ismert szemantikai verziója tartományon belül.

  ```powershell
  Import-AzureRM
  ```

  Csak is importálhatja egy select modul hello ismert szemantikai verziója tartományon belül.

  ```powershell
  Import-Module AzureRM.Network 
  ```

  Jelentkezzen be tooyour fiókjával.

  ```powershell
  Login-AzureRmAccount
  ```

  Válassza ki a kívánt toocreate ExpressRoute-kapcsolatcsoportot hello előfizetést.

  ```powershell
  Select-AzureRmSubscription -SubscriptionId "<subscription ID>"
  ```
2. Hozzon létre egy ExpressRoute-kapcsolatcsoportot.

  Kövesse az utasításokat toocreate hello egy [ExpressRoute-kapcsolatcsoportot](expressroute-howto-circuit-arm.md) , illetve hozta-e hello kapcsolat szolgáltatóját.

  Ha a kapcsolat szolgáltatójánál 3. rétegbeli felügyelt szolgáltatásokat kínál, a kapcsolat szolgáltató tooenable magánhálózati társviszony-létesítést az Ön Azure kérhet. Ebben az esetben nincs szükség hello következő szakaszokban szereplő toofollow utasításokat. Azonban ha a kapcsolat szolgáltatójánál nem kezeli az útválasztást, a kapcsolat létrehozása után továbbra is hello lépések a konfiguráció.
3. Ellenőrizze a hello ExpressRoute körön toomake arról, hogy van üzembe helyezve és is engedélyezve van. A következő példa hello használata:

  ```powershell
  Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
  ```

  a rendszer a következő példa hasonló toohello hello választ:

  ```
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
  ```
4. Az Azure magánhálózati társviszony-létesítés hello kör megadása Győződjön meg arról, hogy rendelkezik-e hello hello lépések folytatása előtt a következő elemek:

  * / 30-as alhálózat hello elsődleges kapcsolathoz. hello alhálózat nem virtuális hálózatok számára fenntartott címtartomány részének kell lennie.
  * / 30-as alhálózat hello másodlagos kapcsolathoz. hello alhálózat nem virtuális hálózatok számára fenntartott címtartomány részének kell lennie.
  * Egy érvényes VLAN-azonosító tooestablish a társviszony. Győződjön meg arról, hogy egyetlen másik társviszonya se a hello áramkör használ hello azonos VLAN-azonosítót.
  * Egy AS-szám a társviszony-létesítéshez. 2 és 4 bájtos AS-számokat is használhat. Ehhez a társviszony-létesítéshez használhat privát AS-számokat is. Ne használja a 65515 számot.
  * **Választható -** az MD5 kivonatoló, ha úgy dönt, hogy egy toouse.

  A következő példa tooconfigure magánhálózati társviszony-létesítés a kör Azure hello használata:

  ```powershell
  Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200

  Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

  Ha úgy dönt, toouse az MD5 kivonatoló, használja a következő példa hello:

  ```powershell
  Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200  -SharedKey "A1B2C3D4"
  ```

  > [!IMPORTANT]
  > Az AS-számot mindenképp társviszony-létesítési ASN-ként, és ne ügyfél ASN-ként adja meg.
  > 
  >

### <a name="tooview-azure-private-peering-details"></a>tooview Azure magánhálózati társviszony-létesítés részletei

Konfigurációs részletek kaphat a következő példa hello használata:

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -Circuit $ckt
```

### <a name="tooupdate-azure-private-peering-configuration"></a>tooupdate Azure magánhálózati társviszony-létesítési konfiguráció

Frissítheti, használja a következő példa hello hello konfiguráció bármely részeként. Ebben a példában a 100 too500 hello VLAN-Azonosítót a kapcsolatcsoport hello frissül.

```powershell
Set-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

### <a name="toodelete-azure-private-peering"></a>az Azure magánhálózati társviszony-létesítés toodelete

A társviszony-létesítési konfiguráció a következő példa hello futtatásával távolíthatja el:

> [!WARNING]
> Bizonyosodjon meg, hogy az összes virtuális hálózatot az ExpressRoute-kapcsolatcsoportot hello megszüntetni ebben a példában futtatása előtt. 
> 
> 

```powershell
Remove-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

## <a name="azure-public-peering"></a>Azure nyilvános társviszony-létesítés

Ez a szakasz segítséget nyújt a létrehozása, beolvasása, frissítése és hello Azure nyilvános társviszony-létesítési konfiguráció ExpressRoute-kapcsolatcsoportot törli.

### <a name="toocreate-azure-public-peering"></a>az Azure nyilvános társviszony toocreate

1. ExpressRoute hello PowerShell modul importálása.

  Telepítenie kell a hello legújabb PowerShell installer [PowerShell-galériában](http://www.powershellgallery.com/) és hello Azure Resource Manager modulok importálása hello PowerShell-munkamenetet a rendelés toostart hello ExpressRoute-parancsmagok használatával. Rendszergazdaként kell toorun PowerShell.

  ```powershell
  Install-Module AzureRM

  Install-AzureRM
```

  Importálja az összes hello AzureRM.* modulok hello ismert szemantikai verziója tartományon belül.

  ```powershell
  Import-AzureRM
  ```

  Csak is importálhatja egy select modul hello ismert szemantikai verziója tartományon belül.

  ```powershell
  Import-Module AzureRM.Network
```

  Jelentkezzen be tooyour fiókjával.

  ```powershell
  Login-AzureRmAccount
  ```

  Válassza ki a kívánt toocreate ExpressRoute-kapcsolatcsoportot hello előfizetést.

  ```powershell
  Select-AzureRmSubscription -SubscriptionId "<subscription ID>"
  ```
2. Hozzon létre egy ExpressRoute-kapcsolatcsoportot.

  Kövesse az utasításokat toocreate hello egy [ExpressRoute-kapcsolatcsoportot](expressroute-howto-circuit-arm.md) , illetve hozta-e hello kapcsolat szolgáltatóját.

  Ha a kapcsolat szolgáltatójánál 3. rétegbeli felügyelt szolgáltatásokat kínál, a kapcsolat szolgáltató tooenable magánhálózati társviszony-létesítést az Ön Azure kérhet. Ebben az esetben nincs szükség hello következő szakaszokban szereplő toofollow utasításokat. Azonban ha a kapcsolat szolgáltatójánál nem kezeli az útválasztást, a kapcsolat létrehozása után továbbra is hello lépések a konfiguráció.
3. Ellenőrizze a hello ExpressRoute körön tooensure van üzembe helyezve, és is engedélyezve van. A következő példa hello használata:

  ```powershell
  Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
  ```

  a rendszer a következő példa hasonló toohello hello választ:

  ```
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
  ```
4. Az Azure nyilvános társviszony-létesítés hello kör megadása Győződjön meg arról, hogy rendelkezik-e hello előtt végrehajtásának folytatásához a következő információkat.

  * / 30-as alhálózat hello elsődleges kapcsolathoz. Ennek egy érvényes nyilvános IPv4-előtagnak kell lennie.
  * / 30-as alhálózat hello másodlagos kapcsolathoz. Ennek egy érvényes nyilvános IPv4-előtagnak kell lennie.
  * Egy érvényes VLAN-azonosító tooestablish a társviszony. Győződjön meg arról, hogy egyetlen másik társviszonya se a hello áramkör használ hello azonos VLAN-azonosítót.
  * Egy AS-szám a társviszony-létesítéshez. 2 és 4 bájtos AS-számokat is használhat.
  * **Választható -** az MD5 kivonatoló, ha úgy dönt, hogy egy toouse.

  Futtassa a következő példa tooconfigure Azure nyilvános társviszony-létesítés a kör hello

  ```powershell
  Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt -PeeringType AzurePublicPeering -PeerASN 100 -PrimaryPeerAddressPrefix "12.0.0.0/30" -SecondaryPeerAddressPrefix "12.0.0.4/30" -VlanId 100

  Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

  Ha úgy dönt, toouse az MD5 kivonatoló, használja a következő példa hello:

  ```powershell
  Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt -PeeringType AzurePublicPeering -PeerASN 100 -PrimaryPeerAddressPrefix "12.0.0.0/30" -SecondaryPeerAddressPrefix "12.0.0.4/30" -VlanId 100  -SharedKey "A1B2C3D4"

  Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

  > [!IMPORTANT]
  > Az AS-számot mindenképp társviszony-létesítési ASN-ként, és ne ügyfél ASN-ként adja meg.
  > 
  >

### <a name="tooview-azure-public-peering-details"></a>tooview Azure nyilvános társviszony-létesítés részletei

Konfigurációs részletek hello a következő parancsmag használatával kaphat:

```powershell
  $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

  Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -Circuit $ckt
  ```

### <a name="tooupdate-azure-public-peering-configuration"></a>az Azure nyilvános társviszony-létesítési konfiguráció tooupdate

Frissítheti, használja a következő példa hello hello konfiguráció bármely részeként. Ebben a példában a 200 too600 hello VLAN-Azonosítót a kapcsolatcsoport hello frissül.

```powershell
Set-AzureRmExpressRouteCircuitPeeringConfig  -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt -PeeringType AzurePublicPeering -PeerASN 100 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 600

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

### <a name="toodelete-azure-public-peering"></a>az Azure nyilvános társviszony toodelete

A társviszony-létesítési konfiguráció a következő példa hello futtatásával távolíthatja el:

```powershell
Remove-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

## <a name="microsoft-peering"></a>Microsoft társviszony-létesítés

Ez a szakasz segítséget nyújt a létrehozása, beolvasása, frissítése és törlése hello Microsoft társviszony-létesítési konfiguráció ExpressRoute-kör.

> [!IMPORTANT]
> Előzetes tooAugust 1 Microsoft társviszony-létesítést az ExpressRoute-Kapcsolatcsoportok volt beállítva, akkor 2017 fog rendelkezni az összes szolgáltatás előtagok keresztül hello Microsoft társviszony-létesítést, meghirdetett, akkor is, ha nincsenek megadva útvonal szűrők. A Microsoft társviszony-létesítést az ExpressRoute-Kapcsolatcsoportok vannak konfigurálva, vagy azt követően 2017. augusztus 1. nem rendelkezik a előtagokat meghirdetett, amíg egy útvonal-szűrő nem csatlakoztatja toohello körön. További információkért lásd: [konfigurálása a Microsoft társviszony-létesítéshez útvonal szűrő](how-to-routefilter-powershell.md).
> 
> 

### <a name="toocreate-microsoft-peering"></a>toocreate Microsoft társviszony-létesítés

1. ExpressRoute hello PowerShell modul importálása.

  Telepítenie kell a hello legújabb PowerShell installer [PowerShell-galériában](http://www.powershellgallery.com/) és hello Azure Resource Manager modulok importálása hello PowerShell-munkamenetet a rendelés toostart hello ExpressRoute-parancsmagok használatával. Rendszergazdaként kell toorun PowerShell.

  ```powershell
  Install-Module AzureRM

  Install-AzureRM
  ```

  Importálja az összes hello AzureRM.* modulok hello ismert szemantikai verziója tartományon belül.

  ```powershell
  Import-AzureRM
  ```

  Csak is importálhatja egy select modul hello ismert szemantikai verziója tartományon belül.

  ```powershell
  Import-Module AzureRM.Network
  ```

  Jelentkezzen be tooyour fiókjával.

  ```powershell
  Login-AzureRmAccount
  ```

  Válassza ki a kívánt toocreate ExpressRoute-kapcsolatcsoportot hello előfizetést.

  ```powershell
Select-AzureRmSubscription -SubscriptionId "<subscription ID>"
  ```
2. Hozzon létre egy ExpressRoute-kapcsolatcsoportot.

  Kövesse az utasításokat toocreate hello egy [ExpressRoute-kapcsolatcsoportot](expressroute-howto-circuit-arm.md) , illetve hozta-e hello kapcsolat szolgáltatóját.

  Ha a kapcsolat szolgáltatójánál 3. rétegbeli felügyelt szolgáltatásokat kínál, a kapcsolat szolgáltató tooenable magánhálózati társviszony-létesítést az Ön Azure kérhet. Ebben az esetben nincs szükség hello következő szakaszokban szereplő toofollow utasításokat. Azonban ha a kapcsolat szolgáltatójánál nem kezeli az útválasztást, a kapcsolat létrehozása után továbbra is hello lépések a konfiguráció.
3. Ellenőrizze a hello ExpressRoute körön toomake arról, hogy van üzembe helyezve és is engedélyezve van. A következő példa hello használata:

  ```powershell
  Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
  ```

  a rendszer a következő példa hasonló toohello hello választ:

  ```
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
  ```
4. Konfigurálja a Microsoft hello-kör társviszony-létesítés. Győződjön meg arról, hogy rendelkezik-e folytatni a következő információk előtt hello.

  * / 30-as alhálózat hello elsődleges kapcsolathoz. Ennek egy érvényes nyilvános IPv4-előtagnak kell lennie, amely az Ön tulajdonában van, és regisztrálva van egy RIR-/IRR-jegyzékben.
  * / 30-as alhálózat hello másodlagos kapcsolathoz. Ennek egy érvényes nyilvános IPv4-előtagnak kell lennie, amely az Ön tulajdonában van, és regisztrálva van egy RIR-/IRR-jegyzékben.
  * Egy érvényes VLAN-azonosító tooestablish a társviszony. Győződjön meg arról, hogy egyetlen másik társviszonya se a hello áramkör használ hello azonos VLAN-azonosítót.
  * Egy AS-szám a társviszony-létesítéshez. 2 és 4 bájtos AS-számokat is használhat.
  * Meghirdetett előtagok: meg kell adnia a lista az összes előtagok hello BGP munkameneten keresztül tervezi tooadvertise. A rendszer kizárólag a nyilvános IP-cím-előtagokat fogadja el. Ha azt tervezi, toosend előtagok készlete, elküldheti a vesszővel tagolt listáját. Ezeket az előtagokat kell lenniük egy RIR-ben regisztrált tooyou vagy IRR-ben.
  * **Választható -** ügyfél ASN: Ha hirdetési-előtagok nem regisztrált toohello társviszony-létesítés SZÁMOT, megadhatja hello számú toowhich regisztrálva vannak.
  * Útválasztási beállításjegyzék-név: Hello RIR megadhat / BMR mely hello ellen, számát, és előtagok regisztrálva van.
  * **Választható -** az MD5 kivonatoló, ha úgy dönt, hogy egy toouse.

   A következő példa tooconfigure Microsoft társviszony-létesítés a kör hello használata:

  ```powershell
  Add-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt -PeeringType MicrosoftPeering -PeerASN 100 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 300 -MicrosoftConfigAdvertisedPublicPrefixes "123.1.0.0/24" -MicrosoftConfigCustomerAsn 23 -MicrosoftConfigRoutingRegistryName "ARIN"

  Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

### <a name="tooget-microsoft-peering-details"></a>tooget Microsoft társviszony-létesítési részletei

Kaphat a konfigurációs adatait a következő példa hello használata:

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

Get-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt
```

### <a name="tooupdate-microsoft-peering-configuration"></a>a Microsoft társviszony-létesítési konfiguráció tooupdate

Frissítheti, használja a következő példa hello hello konfiguráció bármely részeként:

```powershell
Set-AzureRmExpressRouteCircuitPeeringConfig  -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt -PeeringType MicrosoftPeering -PeerASN 100 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 300 -MicrosoftConfigAdvertisedPublicPrefixes "124.1.0.0/24" -MicrosoftConfigCustomerAsn 23 -MicrosoftConfigRoutingRegistryName "ARIN"

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

### <a name="toodelete-microsoft-peering"></a>toodelete Microsoft társviszony-létesítés

A társviszony-létesítési konfiguráció hello a következő parancsmag futtatásával távolíthatja el:

```powershell
Remove-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

## <a name="next-steps"></a>Következő lépések

Következő lépés, [csatolni a virtuális hálózat tooan ExpressRoute-kapcsolatcsoportot](expressroute-howto-linkvnet-arm.md).

* Az ExpressRoute-munkafolyamatokkal kapcsolatos további információkért lásd: [ExpressRoute workflows](expressroute-workflows.md) (ExpressRoute-munkafolyamatok).
* A kapcsolatcsoportok társviszony-létesítéseivel kapcsolatos további információkért lásd: [ExpressRoute circuits and routing domains](expressroute-circuit-peerings.md) (ExpressRoute-kapcsolatcsoportok és útválasztási tartományok).
* További információkért a virtuális hálózatok használatáról lásd: [Virtual network overview](../virtual-network/virtual-networks-overview.md) (Virtuális hálózatok áttekintése).
