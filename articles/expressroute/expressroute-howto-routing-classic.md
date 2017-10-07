---
title: "Hogyan tooconfigure útválasztás (társviszony) az ExpressRoute-kapcsolatcsoportot: Azure: klasszikus |} Microsoft Docs"
description: "Ez a cikk végigvezeti hello létrehozásához és a kiépítés hello saját, a nyilvános és a Microsoft társviszony-létesítést az ExpressRoute-kapcsolatcsoportot. Ez a cikk is bemutatja, hogyan toocheck hello állapotát, a frissítést, vagy a-kör társviszony törlése."
documentationcenter: na
services: expressroute
author: ganesr
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: a4bd39d2-373a-467a-8b06-36cfcc1027d2
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/21/2017
ms.author: ganesr;cherylmc
ms.openlocfilehash: dc5bcc4b86c3bc965a88281b6c2a660f3bc58eb1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-modify-peering-for-an-expressroute-circuit-classic"></a>Létrehozásához és módosításához (klasszikus) ExpressRoute-kör társviszony
> [!div class="op_single_selector"]
> * [Azure Portal](expressroute-howto-routing-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-routing-arm.md)
> * [Azure CLI](howto-routing-cli.md)
> * [Video - privát társviszony-létesítés](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-private-peering-for-your-expressroute-circuit)
> * [Video - nyilvános társviszony-létesítés](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-public-peering-for-your-expressroute-circuit)
> * [Videó – a Microsoft társviszony-létesítés](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-microsoft-peering-for-your-expressroute-circuit)
> * [PowerShell (klasszikus)](expressroute-howto-routing-classic.md)
> 

Ez a cikk bemutatja, hogyan hello lépéseket toocreate és kezelése a PowerShell és hello klasszikus üzembe helyezési modellel ExpressRoute-kapcsolatcsoportot útválasztási konfigurációja. az alábbi hello lépéseit is bemutatja, hogyan toocheck hello állapotát, a frissítés, vagy törölje, majd kiosztásának megszüntetése ExpressRoute-kör társviszony.

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

**Tudnivalók az Azure üzembe helyezési modelljeiről**

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]


## <a name="configuration-prerequisites"></a>Konfigurációs előfeltételek
* Szüksége lesz Azure Service Management (SM) PowerShell-parancsmagok hello hello legújabb verzióját. További információkért lásd: [Ismerkedés az Azure PowerShell-parancsmagok](/powershell/azure/overview).  
* Győződjön meg arról, hogy átolvasta hello [Előfeltételek](expressroute-prerequisites.md) lap hello [útválasztási követelmények](expressroute-routing.md) lap és hello [munkafolyamatok](expressroute-workflows.md) lapon konfigurálás elkezdése előtt.
* Egy aktív ExpressRoute-kapcsolatcsoportra lesz szüksége. Útmutatás alapján hello túl[ExpressRoute-kapcsolatcsoportot létrehozni](expressroute-howto-circuit-classic.md) , és folytassa a kapcsolat szolgáltatójánál előtt által engedélyezett hello áramkör. hello ExpressRoute-kapcsolatcsoportot toobe képes toorun hello parancsmagok az alábbiakban meg kiépített és engedélyezett állapotban kell lennie.

> [!IMPORTANT]
> Ezek az utasítások csak a 2. rétegbeli kapcsolatot szolgáltatásokat nyújtó szolgáltatók létre toocircuits vonatkoznak. Ha 3. rétegbeli szolgáltatásokat kínáló szolgáltatóval rendelkezik (tipikusan IPVPN, mint az MPLS), a kapcsolatszolgáltató konfigurálja és felügyeli az útválasztást Ön helyett.
> 
> 

Egy, két vagy akár mindhárom társviszony-létesítést (Azure privát, Azure nyilvános és Microsoft) is konfigurálhatja egy adott ExpressRoute-kapcsolatcsoportban. A társviszony-létesítéseket tetszőleges sorrendben konfigurálhatja. Azonban kell győződjön meg arról, hogy elvégezte-e minden társviszony-létesítési egyszerre csak egy hello konfigurációját.


### <a name="log-in-tooyour-azure-account-and-select-a-subscription"></a>Jelentkezzen be Azure-fiók tooyour, és válasszon egy előfizetést
1. Nyissa meg a PowerShell-konzolt emelt szintű jogosultságokkal, és csatlakozzon a tooyour fiók. A következő példa toohelp csatlakozás hello használata:

        Login-AzureRmAccount

2. Hello előfizetések hello fiók ellenőrzése.

        Get-AzureRmSubscription

3. Ha egynél több előfizetéssel rendelkezik, válassza ki a megjeleníteni kívánt toouse hello előfizetést.

        Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

4. Ezután használhatja a következő parancsmag tooadd hello az Azure-előfizetés tooPowerShell hello klasszikus telepítési modell.

        Add-AzureAccount


## <a name="azure-private-peering"></a>Azure privát társviszony-létesítés
Ez a szakasz útmutatás hogyan toocreate, beolvasása, frissítése és törlése hello Azure privát társviszony-létesítési konfiguráció ExpressRoute-kapcsolatcsoportot. 

### <a name="toocreate-azure-private-peering"></a>az Azure magánhálózati társviszony-létesítés toocreate
1. **ExpressRoute hello PowerShell modul importálása.**
   
    Hello Azure és az ExpressRoute modulok importálni kell az hello PowerShell-munkamenetet a rendelés toostart hello ExpressRoute-parancsmagok használatával. Futtatási hello következő tooimport hello Azure és az ExpressRoute-modulok parancsokat hello PowerShell-munkamenetbe.  
   
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'
2. **ExpressRoute-kapcsolatcsoportot létrehozni.**
   
    Kövesse az utasításokat toocreate hello egy [ExpressRoute-kapcsolatcsoportot](expressroute-howto-circuit-classic.md) , illetve hozta-e hello kapcsolat szolgáltatóját. Ha a kapcsolat szolgáltatójánál 3. rétegbeli felügyelt szolgáltatásokat kínál, a kapcsolat szolgáltató tooenable magánhálózati társviszony-létesítést az Ön Azure kérhet. Ebben az esetben nincs szükség hello következő szakaszokban szereplő toofollow utasításokat. Ha a kapcsolat szolgáltatójánál nem kezeli az útválasztást, a kapcsolat létrehozása után azonban kövesse az alábbi hello-utasításokat. 
3. **Ellenőrizze a hello ExpressRoute-kapcsolatcsoport tooensure ki van építve.**
   
    Ha ExpressRoute-kapcsolatcsoportot hello kiosztásakor és is engedélyezve van, először toosee ellenőriz. Tekintse meg az alábbi példa hello.
   
        PS C:\> Get-AzureDedicatedCircuit -ServiceKey "*********************************"
   
        Bandwidth                        : 200
        CircuitName                      : MyTestCircuit
        Location                         : Silicon Valley
        ServiceKey                       : *********************************
        ServiceProviderName              : equinix
        ServiceProviderProvisioningState : Provisioned
        Sku                              : Standard
        Status                           : Enabled
   
    Győződjön meg arról, hogy látható-e hello áramkör kiépítve és engedélyezve. Ha nem, dolgozni a kapcsolat szolgáltató tooget a kapcsolatcsoport szükséges toohello állapotát és állapotát.
   
        ServiceProviderProvisioningState : Provisioned
        Status                           : Enabled
4. **Az Azure magánhálózati társviszony-létesítés hello kör megadása**
   
    Győződjön meg arról, hogy rendelkezik-e hello hello lépések folytatása előtt a következő elemek:
   
   * / 30-as alhálózat hello elsődleges kapcsolathoz. Ez nem képezheti semmilyen, virtuális hálózatok számára lefoglalt címtér részét.
   * / 30-as alhálózat hello másodlagos kapcsolathoz. Ez nem képezheti semmilyen, virtuális hálózatok számára lefoglalt címtér részét.
   * Egy érvényes VLAN-azonosító tooestablish a társviszony. Győződjön meg arról, hogy egyetlen másik társviszonya se a hello áramkör használ hello azonos VLAN-azonosítót.
   * Egy AS-szám a társviszony-létesítéshez. 2 és 4 bájtos AS-számokat is használhat. Ehhez a társviszony-létesítéshez használhat privát AS-számokat is. Ne használja a 65515 számot.
   * Az MD5 kivonatoló, ha úgy dönt, hogy egy toouse. **Ez nem kötelező**.
     
    A következő parancsmag tooconfigure Azure magánhálózati társviszony-létesítés a kör hello is futtathatja.
     
        Új AzureBGPPeering - AccessType titkos - ServiceKey "x" - PrimaryPeerSubnet "10.0.0.0/30" - SecondaryPeerSubnet "10.0.0.4/30" - PeerAsn 1234 - VlanId 100
     
    Az alábbi hello parancsmag is használhatja, ha úgy dönt, toouse az MD5 kivonatoló.
     
        Új AzureBGPPeering - AccessType titkos - ServiceKey "x" - PrimaryPeerSubnet "10.0.0.0/30" - SecondaryPeerSubnet "10.0.0.4/30" - PeerAsn 1234 - VlanId 100 - SharedKey "A1B2C3D4"
     
     > [!IMPORTANT]
     > Az AS-számot mindenképp társviszony-létesítési ASN-ként, és ne ügyfél ASN-ként adja meg.
     > 
     > 

### <a name="tooview-azure-private-peering-details"></a>tooview Azure magánhálózati társviszony-létesítés részletei
Konfigurációs részletekért hello a következő parancsmag használatával

    Get-AzureBGPPeering -AccessType Private -ServiceKey "*********************************"

    AdvertisedPublicPrefixes       : 
    AdvertisedPublicPrefixesState  : Configured
    AzureAsn                       : 12076
    CustomerAutonomousSystemNumber : 
    PeerAsn                        : 1234
    PrimaryAzurePort               : 
    PrimaryPeerSubnet              : 10.0.0.0/30
    RoutingRegistryName            : 
    SecondaryAzurePort             : 
    SecondaryPeerSubnet            : 10.0.0.4/30
    State                          : Enabled
    VlanId                         : 100


### <a name="tooupdate-azure-private-peering-configuration"></a>tooupdate Azure magánhálózati társviszony-létesítési konfiguráció
Frissítheti, használja a következő parancsmag hello hello konfiguráció bármely részeként. Hello az alábbi példában a VLAN-Azonosítót a kapcsolatcsoport hello hello 100 too500 alatt módosul.

    Set-AzureBGPPeering -AccessType Private -ServiceKey "*********************************" -PrimaryPeerSubnet "10.0.0.0/30" -SecondaryPeerSubnet "10.0.0.4/30" -PeerAsn 1234 -VlanId 500 -SharedKey "A1B2C3D4"

### <a name="toodelete-azure-private-peering"></a>az Azure magánhálózati társviszony-létesítés toodelete
Eltávolíthatja a társviszony-létesítési konfiguráció hello a következő parancsmag futtatásával.

> [!WARNING]
> Bizonyosodjon meg, hogy az összes virtuális hálózatot az ExpressRoute-kapcsolatcsoportot hello megszüntetni a parancsmag futtatása előtt. 
> 
> 

    Remove-AzureBGPPeering -AccessType Private -ServiceKey "*********************************"


## <a name="azure-public-peering"></a>Azure nyilvános társviszony-létesítés
Ez a szakasz útmutatás hogyan toocreate, beolvasása, frissítése és törlése hello Azure nyilvános társviszony-létesítési konfiguráció ExpressRoute-kapcsolatcsoportot.

### <a name="toocreate-azure-public-peering"></a>az Azure nyilvános társviszony toocreate
1. **ExpressRoute hello PowerShell modul importálása.**
   
    Hello Azure és az ExpressRoute modulok importálni kell az hello PowerShell-munkamenetet a rendelés toostart hello ExpressRoute-parancsmagok használatával. Futtatási hello következő tooimport hello Azure és az ExpressRoute-modulok parancsokat hello PowerShell-munkamenetbe. 
   
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'
2. **ExpressRoute-kapcsolatcsoport létrehozása**
   
    Kövesse az utasításokat toocreate hello egy [ExpressRoute-kapcsolatcsoportot](expressroute-howto-circuit-classic.md) , illetve hozta-e hello kapcsolat szolgáltatóját. Ha a kapcsolat szolgáltatójánál 3. rétegbeli felügyelt szolgáltatásokat kínál, a kapcsolat szolgáltató tooenable Azure nyilvános társviszony-létesítés meg is igényelhet. Ebben az esetben nincs szükség hello következő szakaszokban szereplő toofollow utasításokat. Ha a kapcsolat szolgáltatójánál nem kezeli az útválasztást, a kapcsolat létrehozása után azonban kövesse az alábbi hello-utasításokat.
3. **Ellenőrizze a ExpressRoute-kapcsolatcsoport tooensure ki van építve**
   
    Ha ExpressRoute-kapcsolatcsoportot hello kiosztásakor és is engedélyezve van, először toosee ellenőriz. Tekintse meg az alábbi példa hello.
   
        PS C:\> Get-AzureDedicatedCircuit -ServiceKey "*********************************"
   
        Bandwidth                        : 200
        CircuitName                      : MyTestCircuit
        Location                         : Silicon Valley
        ServiceKey                       : *********************************
        ServiceProviderName              : equinix
        ServiceProviderProvisioningState : Provisioned
        Sku                              : Standard
        Status                           : Enabled
   
    Győződjön meg arról, hogy látható-e hello áramkör kiépítve és engedélyezve. Ha nem, dolgozni a kapcsolat szolgáltató tooget a kapcsolatcsoport szükséges toohello állapotát és állapotát.
   
        ServiceProviderProvisioningState : Provisioned
        Status                           : Enabled
4. **Az Azure nyilvános társviszony-létesítés hello kapcsolat konfigurálása**
   
    Győződjön meg arról, hogy rendelkezik-e hello előtt végrehajtásának folytatásához a következő információkat.
   
   * / 30-as alhálózat hello elsődleges kapcsolathoz. Ennek egy érvényes nyilvános IPv4-előtagnak kell lennie.
   * / 30-as alhálózat hello másodlagos kapcsolathoz. Ennek egy érvényes nyilvános IPv4-előtagnak kell lennie.
   * Egy érvényes VLAN-azonosító tooestablish a társviszony. Győződjön meg arról, hogy egyetlen másik társviszonya se a hello áramkör használ hello azonos VLAN-azonosítót.
   * Egy AS-szám a társviszony-létesítéshez. 2 és 4 bájtos AS-számokat is használhat.
   * Az MD5 kivonatoló, ha úgy dönt, hogy egy toouse. **Ez nem kötelező**.
     
    A következő parancsmag tooconfigure Azure nyilvános társviszony-létesítés a kör hello futtatása
     
        Új AzureBGPPeering - AccessType nyilvános - ServiceKey "x" - PrimaryPeerSubnet "131.107.0.0/30" - SecondaryPeerSubnet "131.107.0.4/30" - PeerAsn 1234 - VlanId 200
     
    Ha úgy dönt, toouse az MD5 kivonatoló hello parancsmag az alábbi használható
     
        Új AzureBGPPeering - AccessType nyilvános - ServiceKey "x" - PrimaryPeerSubnet "131.107.0.0/30" - SecondaryPeerSubnet "131.107.0.4/30" - PeerAsn 1234 - VlanId 200 - SharedKey "A1B2C3D4"
     
     > [!IMPORTANT]
     > Az AS-számot mindenképp társviszony-létesítési ASN-ként, és ne ügyfél ASN-ként adja meg.
     > 
     > 

### <a name="tooview-azure-public-peering-details"></a>tooview Azure nyilvános társviszony-létesítés részletei
Konfigurációs részletekért hello a következő parancsmag használatával

    Get-AzureBGPPeering -AccessType Public -ServiceKey "*********************************"

    AdvertisedPublicPrefixes       : 
    AdvertisedPublicPrefixesState  : Configured
    AzureAsn                       : 12076
    CustomerAutonomousSystemNumber : 
    PeerAsn                        : 1234
    PrimaryAzurePort               : 
    PrimaryPeerSubnet              : 131.107.0.0/30
    RoutingRegistryName            : 
    SecondaryAzurePort             : 
    SecondaryPeerSubnet            : 131.107.0.4/30
    State                          : Enabled
    VlanId                         : 200


### <a name="tooupdate-azure-public-peering-configuration"></a>az Azure nyilvános társviszony-létesítési konfiguráció tooupdate
Frissítheti a hello konfigurációját a következő parancsmag hello segítségével valamely része

    Set-AzureBGPPeering -AccessType Public -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -PeerAsn 1234 -VlanId 600 -SharedKey "A1B2C3D4"

a fenti példa hello 200 too600 hello VLAN-Azonosítót a kapcsolatcsoport hello alatt módosul.

### <a name="toodelete-azure-public-peering"></a>az Azure nyilvános társviszony toodelete
Eltávolíthatja a társviszony-létesítési konfiguráció hello a következő parancsmag futtatásával

    Remove-AzureBGPPeering -AccessType Public -ServiceKey "*********************************"

## <a name="microsoft-peering"></a>Microsoft társviszony-létesítés
Ez a szakasz útmutatás hogyan toocreate, beolvasása, frissítése és törlése hello Microsoft társviszony-létesítési konfiguráció az ExpressRoute-kapcsolatcsoportot. 

### <a name="toocreate-microsoft-peering"></a>toocreate Microsoft társviszony-létesítés
1. **ExpressRoute hello PowerShell modul importálása.**
   
    Hello Azure és az ExpressRoute modulok importálni kell az hello PowerShell-munkamenetet a rendelés toostart hello ExpressRoute-parancsmagok használatával. Futtatási hello következő tooimport hello Azure és az ExpressRoute-modulok parancsokat hello PowerShell-munkamenetbe.  
   
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'
2. **ExpressRoute-kapcsolatcsoport létrehozása**
   
    Kövesse az utasításokat toocreate hello egy [ExpressRoute-kapcsolatcsoportot](expressroute-howto-circuit-classic.md) , illetve hozta-e hello kapcsolat szolgáltatóját. Ha a kapcsolat szolgáltatójánál 3. rétegbeli felügyelt szolgáltatásokat kínál, a kapcsolat szolgáltató tooenable magánhálózati társviszony-létesítést az Ön Azure kérhet. Ebben az esetben nincs szükség hello következő szakaszokban szereplő toofollow utasításokat. Ha a kapcsolat szolgáltatójánál nem kezeli az útválasztást, a kapcsolat létrehozása után azonban kövesse az alábbi hello-utasításokat.
3. **Ellenőrizze a ExpressRoute-kapcsolatcsoport tooensure ki van építve**
   
    Ha ExpressRoute-kapcsolatcsoportot hello kiépítve és engedélyezett állapotban van, először ellenőrizze toosee.
   
        PS C:\> Get-AzureDedicatedCircuit -ServiceKey "*********************************"
   
        Bandwidth                        : 200
        CircuitName                      : MyTestCircuit
        Location                         : Silicon Valley
        ServiceKey                       : *********************************
        ServiceProviderName              : equinix
        ServiceProviderProvisioningState : Provisioned
        Sku                              : Standard
        Status                           : Enabled
   
    Győződjön meg arról, hogy látható-e hello áramkör kiépítve és engedélyezve. Ha nem, dolgozni a kapcsolat szolgáltató tooget a kapcsolatcsoport szükséges toohello állapotát és állapotát.
   
        ServiceProviderProvisioningState : Provisioned
        Status                           : Enabled
4. **A Microsoft hello-kör társviszony konfigurálása**
   
    Győződjön meg arról, hogy rendelkezik-e folytatni a következő információk előtt hello.
   
   * / 30-as alhálózat hello elsődleges kapcsolathoz. Ennek egy érvényes nyilvános IPv4-előtagnak kell lennie, amely az Ön tulajdonában van, és regisztrálva van egy RIR-/IRR-jegyzékben.
   * / 30-as alhálózat hello másodlagos kapcsolathoz. Ennek egy érvényes nyilvános IPv4-előtagnak kell lennie, amely az Ön tulajdonában van, és regisztrálva van egy RIR-/IRR-jegyzékben.
   * Egy érvényes VLAN-azonosító tooestablish a társviszony. Győződjön meg arról, hogy egyetlen másik társviszonya se a hello áramkör használ hello azonos VLAN-azonosítót.
   * Egy AS-szám a társviszony-létesítéshez. 2 és 4 bájtos AS-számokat is használhat.
   * Meghirdetett előtagok: meg kell adnia a lista az összes előtagok hello BGP munkameneten keresztül tervezi tooadvertise. A rendszer kizárólag a nyilvános IP-cím-előtagokat fogadja el. Ha azt tervezi, hogy toosend előtagok készlete küldhet egy vesszővel elválasztott listában. Ezeket az előtagokat kell lenniük egy RIR-ben regisztrált tooyou vagy IRR-ben.
   * Ügyfél ASN: Ha-előtagok nem regisztrált toohello társviszony-létesítés SZÁMOT vannak hirdetményt forgalmaz, megadhatja hello számú toowhich regisztrálva vannak. **Ez nem kötelező**.
   * Útválasztási beállításjegyzék-név: Hello RIR megadhat / BMR mely hello ellen, számát, és előtagok regisztrálva van.
   * Az MD5 kivonatoló, ha úgy dönt, hogy egy toouse. **Ez nem kötelező.**
     
    A következő parancsmag tooconfigure Microsoft pering a kapcsolatcsoport hello futtatása
     
        Új AzureBGPPeering - AccessType Microsoft - ServiceKey "x" - PrimaryPeerSubnet "131.107.0.0/30" - SecondaryPeerSubnet "131.107.0.4/30" - VlanId 300 - PeerAsn 1234 - CustomerAsn 2245 - AdvertisedPublicPrefixes " 123.0.0.0/30 "- RoutingRegistryName"ARIN"- SharedKey"A1B2C3D4"

### <a name="tooview-microsoft-peering-details"></a>tooview Microsoft társviszony-létesítési részletei
Konfigurációs részletek hello a következő parancsmag használatával kérheti le.

    Get-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************"

    AdvertisedPublicPrefixes       : 123.0.0.0/30
    AdvertisedPublicPrefixesState  : Configured
    AzureAsn                       : 12076
    CustomerAutonomousSystemNumber : 2245
    PeerAsn                        : 1234
    PrimaryAzurePort               : 
    PrimaryPeerSubnet              : 10.0.0.0/30
    RoutingRegistryName            : ARIN
    SecondaryAzurePort             : 
    SecondaryPeerSubnet            : 10.0.0.4/30
    State                          : Enabled
    VlanId                         : 300


### <a name="tooupdate-microsoft-peering-configuration"></a>a Microsoft társviszony-létesítési konfiguráció tooupdate
Frissítheti, használja a következő parancsmag hello hello konfiguráció bármely részeként.

    Set-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -VlanId 300 -PeerAsn 1234 -CustomerAsn 2245 -AdvertisedPublicPrefixes "123.0.0.0/30" -RoutingRegistryName "ARIN" -SharedKey "A1B2C3D4"

### <a name="toodelete-microsoft-peering"></a>toodelete Microsoft társviszony-létesítés
Eltávolíthatja a társviszony-létesítési konfiguráció hello a következő parancsmag futtatásával.

    Remove-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************"

## <a name="next-steps"></a>Következő lépések
Ezt követően [csatolni a virtuális hálózat tooan ExpressRoute-kapcsolatcsoportot](expressroute-howto-linkvnet-classic.md).

* Munkafolyamatokkal kapcsolatos további információkért lásd: [ExpressRoute-munkafolyamatokat](expressroute-workflows.md).
* A kapcsolatcsoportok társviszony-létesítéseivel kapcsolatos további információkért lásd: [ExpressRoute circuits and routing domains](expressroute-circuit-peerings.md) (ExpressRoute-kapcsolatcsoportok és útválasztási tartományok).

