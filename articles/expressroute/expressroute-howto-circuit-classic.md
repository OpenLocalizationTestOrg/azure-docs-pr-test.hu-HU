---
title: "Létrehozásához és módosításához ExpressRoute-kapcsolatcsoportot: PowerShell: klasszikus Azure portál |} Microsoft Docs"
description: "Ez a cikk végigvezeti hello létrehozásához és a kiépítés ExpressRoute-kapcsolatcsoportot. Ez a cikk is bemutatja, hogyan toocheck hello állapotát, frissítéséhez vagy törlése és a kapcsolatcsoport kiosztásának megszüntetése."
documentationcenter: na
services: expressroute
author: ganesr
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 0134d242-6459-4dec-a2f1-4657c3bc8b23
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/21/2017
ms.author: ganesr;cherylmc
ms.openlocfilehash: 9897c88776a2153ba22aa9ff328becb9f12b660b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-modify-an-expressroute-circuit-using-powershell-classic"></a>Létrehozása és módosítása a powershellel (klasszikus) ExpressRoute-kapcsolatcsoportot
> [!div class="op_single_selector"]
> * [Azure Portal](expressroute-howto-circuit-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-circuit-arm.md)
> * [Azure CLI](howto-circuit-cli.md)
> * [Video - Azure-portálon](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit)
> * [PowerShell (klasszikus)](expressroute-howto-circuit-classic.md)
>

Ez a cikk bemutatja, hogyan hello lépéseket toocreate Azure ExpressRoute-kapcsolatcsoportot PowerShell-parancsmagok és hello klasszikus telepítési modell használatával. Ez a cikk is bemutatja, hogyan toocheck hello állapota, frissítéséhez vagy törlése és kiosztásának megszüntetése ExpressRoute-kapcsolatcsoportot.

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]


**Tudnivalók az Azure üzembe helyezési modelljeiről**

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="before-you-begin"></a>Előkészületek
### <a name="step-1-review-hello-prerequisites-and-workflow-articles"></a>1. lépés Tekintse át a hello Előfeltételek és a munkafolyamat cikkek
Győződjön meg arról, hogy átolvasta hello [Előfeltételek](expressroute-prerequisites.md) és [munkafolyamatok](expressroute-workflows.md) konfigurálás elkezdése előtt.  

### <a name="step-2-install-hello-latest-versions-of-hello-azure-service-management-sm-powershell-modules"></a>2. lépés Hello legújabb verziói hello Azure Service Management (SM) PowerShell-modulok telepítése
Hello utasításait követve [Ismerkedés az Azure PowerShell-parancsmagok](/powershell/azure/overview) kapcsolatos részletes útmutatás tooconfigure a számítógép toouse hello Azure PowerShell-modulok.

### <a name="step-3-log-in-tooyour-azure-account-and-select-a-subscription"></a>3. lépés Jelentkezzen be Azure-fiók tooyour, és válasszon egy előfizetést
1. Nyissa meg a PowerShell-konzolt emelt szintű jogosultságokkal, és csatlakozzon a tooyour fiók. A következő példa toohelp csatlakozás hello használata:

        Login-AzureRmAccount

2. Hello előfizetések hello fiók ellenőrzése.

        Get-AzureRmSubscription

3. Ha egynél több előfizetéssel rendelkezik, válassza ki a megjeleníteni kívánt toouse hello előfizetést.

        Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

4. Ezután használhatja a következő parancsmag tooadd hello az Azure-előfizetés tooPowerShell hello klasszikus telepítési modell.

        Add-AzureAccount

## <a name="create-and-provision-an-expressroute-circuit"></a>Hozzon létre, és helyezze üzembe az ExpressRoute-kapcsolatcsoportot
### <a name="step-1-import-hello-powershell-modules-for-expressroute"></a>1. lépés Az ExpressRoute hello PowerShell modul importálása
 Ha még nem tette meg, importálnia kell hello Azure és az ExpressRoute modulok hello PowerShell-munkamenetet a rendelés toostart hello ExpressRoute-parancsmagok használatával. Hello modul importálása a hello helyre, amely voltak telepítve tooon helyi számítógépen. Hello módszertől függően tooinstall hello modulok használják, hello helye a következő példa azt mutatja meg hello eltérő lehet. Ha szükséges, módosítsa a hello példa.  

    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

### <a name="step-2-get-hello-list-of-supported-providers-locations-and-bandwidths"></a>2. lépés Támogatott szolgáltatók, a helyek és a sávszélesség hello listájának beolvasása
ExpressRoute-kapcsolatcsoportot létrehozni, meg kell hello támogatott kapcsolat szolgáltatókat, a helyek és a sávszélesség-beállítások listája.

PowerShell-parancsmag hello `Get-AzureDedicatedCircuitServiceProvider` adja vissza ezt az információt fogja használni a későbbi lépésekben:

    Get-AzureDedicatedCircuitServiceProvider

Ellenőrizze a toosee, ha a kapcsolat szolgáltatójánál van szó. Jegyezze fel a következő információ, mert azt később szüksége expressroute-kapcsolatcsoporthoz létrehozásakor hello:

* Név
* PeeringLocations
* BandwidthsOffered

Most már készen áll a toocreate ExpressRoute-kapcsolatcsoportot.         

### <a name="step-3-create-an-expressroute-circuit"></a>3. lépés ExpressRoute-kapcsolatcsoport létrehozása
hello a következő példa bemutatja, hogyan toocreate egy 200 MB/s ExpressRoute áramkör a szilícium Valley Equinix keresztül. Különböző szolgáltatók és más beállítások használata, amikor a kérést helyettesítse be ezt az információt.

> [!IMPORTANT]
> Az ExpressRoute-kapcsolatcsoportot jelenik meg a szolgáltatási kulcs hello pillanattól lesz terhelve. Győződjön meg arról, hogy készen áll a tooprovision hello áramkör hello kapcsolat szolgáltatójánál esetén elvégzi ezt a műveletet.
> 
> 

hello az alábbiakban látható egy példa egy kérelem egy új szolgáltatás kulcs:

    $Bandwidth = 200
    $CircuitName = "MyTestCircuit"
    $ServiceProvider = "Equinix"
    $Location = "Silicon Valley"

    New-AzureDedicatedCircuit -CircuitName $CircuitName -ServiceProviderName $ServiceProvider -Bandwidth $Bandwidth -Location $Location -sku Standard -BillingType MeteredData

Vagy, ha azt szeretné, hogy toocreate hello prémium bővítmény ExpressRoute-kapcsolatcsoportot, hello használata a következő példa. Tekintse meg a toohello [ExpressRoute – gyakori kérdések](expressroute-faqs.md) hello prémium szintű bővítmény kapcsolatos további részletekért.

    New-AzureDedicatedCircuit -CircuitName $CircuitName -ServiceProviderName $ServiceProvider -Bandwidth $Bandwidth -Location $Location -sku Premium - BillingType MeteredData


hello válasz hello szolgáltatás kulcsot fogja tartalmazni. Részletes leírását, az összes hello paraméterek kaphat hello következő futtatásával:

    get-help new-azurededicatedcircuit -detailed

### <a name="step-4-list-all-hello-expressroute-circuits"></a>4. lépés A lista összes hello ExpressRoute-Kapcsolatcsoportok
Hello futtatása `Get-AzureDedicatedCircuit` tooget az összes létrehozott ExpressRoute-Kapcsolatcsoportok hello parancsot:

    Get-AzureDedicatedCircuit

hello válasz valami hasonló toohello, például a következő lesz:

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : NotProvisioned
    Sku                              : Standard
    Status                           : Enabled

Ezt az információt bármikor hello használatával lekérhető `Get-AzureDedicatedCircuit` parancsmag. Összes hello áramkör paraméterek nélkül hívható hello így sorolja fel. A szolgáltatás kulcs megjelenik hello *ServiceKey* mező.

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : NotProvisioned
    Sku                              : Standard
    Status                           : Enabled

Részletes leírását, az összes hello paraméterek kaphat hello következő futtatásával:

    get-help get-azurededicatedcircuit -detailed

### <a name="step-5-send-hello-service-key-tooyour-connectivity-provider-for-provisioning"></a>5. lépés Kapcsolat kulcs tooyour hello szolgáltató küldése történő üzembe helyezéséhez
*ServiceProviderProvisioningState* információt nyújt a hello aktuális állapotának kiépítés hello szolgáltatói oldalán. *Állapot* hello Microsoft ügyféloldali hello állapot biztosít. Kiépítés állapotok áramkör kapcsolatos további információkért lásd: hello [munkafolyamatok](expressroute-workflows.md#expressroute-circuit-provisioning-states) cikk.

Amikor létrehoz egy új ExpressRoute-kapcsolatcsoportot, hello áramkör hello állapota a következő lehet:

    ServiceProviderProvisioningState : NotProvisioned
    Status                           : Enabled


hello áramkör kerül toohello állapotát követő hello folyamatán, amely lehetővé teszi a hello kapcsolat hitelesítésszolgáltató esetén:

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled

ExpressRoute-kapcsolatcsoportot kell hogy toobe képes toouse állapota a következő hello azt:

    ServiceProviderProvisioningState : Provisioned
    Status                           : Enabled


### <a name="step-6-periodically-check-hello-status-and-hello-state-of-hello-circuit-key"></a>6. lépés Rendszeres időközönként ellenőrizze a hello állapotát és hello áramkör kulcs hello állapota
Ez lehetővé teszi, hogy amikor a szolgáltató a kapcsolatcsoport engedélyezve van. Hello áramkör konfigurálása után *ServiceProviderProvisioningState* állapottal jelenik meg *kiépítve* a hello a következő példában látható módon:

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

### <a name="step-7-create-your-routing-configuration"></a>7. lépés Az útválasztó-konfiguráció létrehozása
Tekintse meg a toohello [ExpressRoute-áramkör útválasztási konfigurációja (létrehozásához és módosításához a kapcsolatcsoport esetében)](expressroute-howto-routing-classic.md) cikk lépéseit.

> [!IMPORTANT]
> Ezek az utasítások csak a szolgáltatók által biztosított réteg 2 internetkapcsolati szolgáltatás használatával létrehozott toocircuits vonatkoznak. A szolgáltató által kezelt használata réteg (általában az IP VPN, például az MPLS) 3 szolgáltatások, a kapcsolat szolgáltatójánál konfigurálása és kezelése az Ön útválasztást.
> 
> 

### <a name="step-8-link-a-virtual-network-tooan-expressroute-circuit"></a>8. lépés. Hivatkozásra egy virtuális hálózati tooan ExpressRoute-kapcsolatcsoportot
A következő hivatkozás egy virtuális hálózati tooyour ExpressRoute-kapcsolatcsoportot. Tekintse meg a túl[Linking ExpressRoute áramkörök toovirtual hálózatok](expressroute-howto-linkvnet-classic.md) részletes útmutatásait. Ha az ExpressRoute hello klasszikus üzembe helyezési modellt használó virtuális hálózatot kell toocreate, lásd: [hozzon létre egy virtuális hálózatot az ExpressRoute](expressroute-howto-vnet-portal-classic.md).

## <a name="getting-hello-status-of-an-expressroute-circuit"></a>Egy ExpressRoute-kapcsolatcsoportot hello állapotának beolvasása
Ezt az információt bármikor hello használatával lekérhető `Get-AzureCircuit` parancsmag. Összes hello áramkör paraméterek nélkül hívható hello így sorolja fel.

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

    Bandwidth                        : 1000
    CircuitName                      : MyAsiaCircuit
    Location                         : Singapore
    ServiceKey                       : #################################
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

Egy adott ExpressRoute-kapcsolatcsoportot tájékoztatást kaphat paraméter toohello hívásként hello szolgáltatás kulcs átadásával.

    Get-AzureDedicatedCircuit -ServiceKey "*********************************"

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled


Minden hello paraméterek részletes leírását a következő példa hello futtatásával kaphat:

    get-help get-azurededicatedcircuit -detailed

## <a name="modifying-an-expressroute-circuit"></a>Egy ExpressRoute-kapcsolatcsoportot módosítása
ExpressRoute-kapcsolatcsoportot egyes tulajdonságait módosíthatja kapcsolat befolyásolása nélkül.

Mindent hello állásidő nélkül a következő:

* Engedélyezi vagy letiltja az ExpressRoute-kapcsolatcsoportot prémium ExpressRoute bővítményt.
* Az ExpressRoute-kapcsolatcsoportot növekedése hello sávszélesség megadott érhető el kapacitás hello porton. Vegye figyelembe, hogy a alacsonyabb verziójúra változtatása hello sávszélesség, a kapcsolat nem támogatott. 
* Hello mérési adatok díjköteles tooUnlimited adatokat a terv módosítása Vegye figyelembe az adatok nem támogatott adatforgalmi tooMetered változó hello mérési terv.
* Engedélyezheti és letilthatja *klasszikus műveletek engedélyezése*.

Tekintse meg a toohello [ExpressRoute – gyakori kérdések](expressroute-faqs.md) korlátai és korlátozásai olvashat.

### <a name="tooenable-hello-expressroute-premium-add-on"></a>tooenable hello ExpressRoute prémium szintű bővítmény
Hello ExpressRoute prémium szintű bővítmény hello a következő PowerShell-parancsmag segítségével engedélyezheti a meglévő kapcsolat:

    Set-AzureDedicatedCircuitProperties -ServiceKey "*********************************" -Sku Premium

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Premium
    Status                           : Enabled

A kapcsolatcsoport most kell hello ExpressRoute prémium bővítmény szolgáltatások engedélyezve van. Vegye figyelembe, hogy azt megkezdődik, amint hello parancs sikeresen lefutott számlázási meg hello prémium bővítmény képességhez.

### <a name="toodisable-hello-expressroute-premium-add-on"></a>toodisable hello ExpressRoute prémium szintű bővítmény
> [!IMPORTANT]
> Ez a művelet sikertelen lehet erőforrásokat, amelyek nagyobbak, mint a megengedett hello szabványos kapcsolat használata.
> 
> 

#### <a name="considerations"></a>Megfontolandó szempontok

* Meg kell győződnie arról, hogy virtuális hálózatok csatolt toohello áramkör hello száma kisebb, mint 10, a prémium szintű toostandard visszaminősítését előtt. Ha nem így tesz, a frissítési kérelem sikertelen lesz, és számlázott hello díjait lesz.
* Minden virtuális hálózat más geopolitikai régiókban kell választható. Ha nem így tesz, a frissítési kérelem sikertelen lesz, és számlázott hello díjait lesz.
* Az útvonaltábla a magánhálózati társviszony-létesítés kisebb, mint 4000 útvonalait kell lennie. Ha az útvonal tábla mérete nagyobb, mint 4000 útvonalakat, hello BGP-munkamenetet eldobja, és nem fog újra engedélyezve, amíg a hirdetett hello számához nem 4000 éri el.

#### <a name="disable-hello-premium-add-on"></a>Prémium szintű hello-bővítmény letiltása
A meglévő kör hello ExpressRoute prémium szintű bővítmény letilthatja hello a következő PowerShell-parancsmag segítségével:

    Set-AzureDedicatedCircuitProperties -ServiceKey "*********************************" -Sku Standard

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled



### <a name="tooupdate-hello-expressroute-circuit-bandwidth"></a>tooupdate hello ExpressRoute-kapcsolatcsoport sávszélessége
Ellenőrizze a hello [ExpressRoute – gyakori kérdések](expressroute-faqs.md) támogatott sávszélesség-beállítások a szolgáltatóhoz. Bármely mérete nagyobb, mint a meglévő expressroute hello mérete, amíg hello tartozó fizikai port (amely a kapcsolatcsoport létre van hozva) lehetővé teszi, hogy ki tudja választani.

> [!IMPORTANT]
> Előfordulhat, hogy toorecreate hello ExpressRoute-kapcsolatcsoportot esetén nincs elég kapacitás hello meglévő porton. Hello kapcsolatcsoport nem frissíthető, ha nincsenek további kapacitást érhető el az adott helyhez.
>
> Nem csökken a hello sávszélesség az ExpressRoute-kapcsolatcsoportot megszakítása nélkül. Alacsonyabb verziójúra változtatása sávszélesség toodeprovision hello ExpressRoute-kapcsolatcsoportot meg, és majd építenie az új ExpressRoute-kapcsolatcsoportot.
> 
> 

#### <a name="resize-a-circuit"></a>Expressroute-kapcsolatcsoporthoz átméretezése

Miután eldöntötte, hogy milyen méretű van szüksége, használhatja a kapcsolatcsoport a következő parancs tooresize hello:

    Set-AzureDedicatedCircuitProperties -ServiceKey ********************************* -Bandwidth 1000

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

A kapcsolatcsoport fog rendelkezik lett méretű hello Microsoft oldalán. Vegye fel a kapcsolatot a kapcsolat szolgáltató tooupdate konfigurációit, az ügyféloldali toomatch ezt a módosítást. Vegye figyelembe, hogy azt indítása számlázást, a hello frissítve ettől a sávszélesség beállítást.

Ha megjelenik a következő a hiba, ha hello kapcsolatcsoport sávszélessége növelése hello, az azt jelenti nincs nincs elegendő sávszélesség hello fizikai port a meglévő expressroute létrehozási helyének bal. Toodelete a kapcsolatcsoport rendelkezik, és hozzon létre egy új kapcsolat hello méretű van szüksége. 

    Set-AzureDedicatedCircuitProperties : InvalidOperation : Insufficient bandwidth available tooperform this circuit
    update operation
    At line:1 char:1
    + Set-AzureDedicatedCircuitProperties -ServiceKey ********************* ...
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : CloseError: (:) [Set-AzureDedicatedCircuitProperties], CloudException
        + FullyQualifiedErrorId : Microsoft.WindowsAzure.Commands.ExpressRoute.SetAzureDedicatedCircuitPropertiesCommand


## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a>Megszüntetés és az ExpressRoute-kapcsolatcsoport törlése

### <a name="considerations"></a>Megfontolandó szempontok

* Ez a művelet toosucceed ExpressRoute-kapcsolatcsoportot hello minden virtuális hálózatokat kell választható. Ellenőrizze toosee, ha a virtuális hálózatok, amelyek kapcsolódó toohello áramkör, ha ez a művelet sikertelen.
* Ha hello ExpressRoute körön szolgáltató üzembe helyezési állapota **kiépítési** vagy **kiépítve** együttműködve kell a service provider toodeprovision hello kapcsolatcsoport az oldalon. Rendszer továbbra is tooreserve erőforrások és kiszámlázni Önnek, amíg hello szolgáltató megszüntetési hello áramkör befejeződik, és értesíti a számunkra.
* Ha hello szolgáltató rendelkezik platformelőfizetés hello áramkör (üzembe helyezési állapota hello szolgáltató értéke túl**nincs kiépítve**) hello áramkör törölhet. Ezzel leállítja a számlázási hello kör.

#### <a name="delete-a-circuit"></a>A kapcsolat törlése

Az ExpressRoute-kapcsolatcsoport törlése hello a következő parancs futtatásával:

    Remove-AzureDedicatedCircuit -ServiceKey "*********************************"



## <a name="next-steps"></a>Következő lépések
Miután létrehozta a kapcsolatcsoport, győződjön meg arról, hogy Ön hello a következő:

* [Létrehozásához és módosításához az ExpressRoute-kapcsolatcsoport esetében routing](expressroute-howto-routing-classic.md)
* [A virtuális hálózati tooyour ExpressRoute-kapcsolatcsoportot hivatkozás](expressroute-howto-linkvnet-classic.md)

