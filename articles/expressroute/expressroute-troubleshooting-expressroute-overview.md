---
title: "A kapcsolat ellenőrzése: Azure ExpressRoute-hibaelhárítási útmutatója |} Microsoft Docs"
description: "Ezen a lapon útmutatás hibaelhárítási és az ExpressRoute-kapcsolatcsoportot end tooend létesített kapcsolatok ellenőrzése."
documentationcenter: na
services: expressroute
author: rambk
manager: tracsman
editor: 
ms.assetid: 
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/01/2017
ms.author: cherylmc
ms.openlocfilehash: 713c39c7eafd77a4380b2a91902a9686f2ce1d85
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="verifying-expressroute-connectivity"></a>Az ExpressRoute-kapcsolat ellenőrzése
ExpressRoute, egy a helyszíni hálózat kibővítve hello Microsoft felhő be, hogy a kapcsolat szolgáltatójánál megkönnyíthető titkos kapcsolaton keresztül, a következő három különböző hálózati zónák hello foglal magában:

-   Ügyfél hálózati
-   Szolgáltató
-   A Microsoft Datacenter

hello a jelen dokumentum célja toohelp felhasználói tooidentify ahol (vagy ha a) a kapcsolódási problémát létezik, és belül melyik zónát, és ezáltal tooseek segítség a megfelelő csapat tooresolve hello probléma. Ha a Microsoft támogatási szolgálatához szükséges tooresolve problémát, a támogatási jegy megnyitása [Microsoft Support][Support].

> [!IMPORTANT]
> Ez a dokumentum tervezett toohelp felderítésére és egyszerű problémák elhárítására. Már nem tervezett toobe helyettesíti a Microsoft támogatási szolgálatához. A támogatási jegy megnyitása [Microsoft Support] [ Support] nem toosolve hello problémát hello útmutatást esetén.
>
>

## <a name="overview"></a>Áttekintés
hello alábbi ábrán látható hello logikai kapcsolat ExpressRoute használó ügyfél hálózati tooMicrosoft hálózat.
[![1]][1]

Diagram megelőző hello hello számok kulcs hálózati pontokat meg. hello hálózati pontokat gyakran ez a cikk keresztül által hivatkozott a hozzájuk társított számokat.

Attól függően, hogy hello ExpressRoute kapcsolat modell (felhőalapú Exchange történő együttes elhelyezés, Point-to-Point protokoll Ethernet-kapcsolat vagy bármely elem közöttiként (IPVPN)) hello hálózati 3. és 4 is lehetnek kapcsolók (2. rétegbeli eszközök). hello kulcs hálózati pontok mutatja a következők:

1.  Ügyfél számítási eszközt (például kiszolgáló vagy számítógép)
2.  Tanúsítványigénylési webszolgáltatás: Ügyfél peremhálózati útválasztók 
3.  PEs (CE hozzáférhető): szolgáltató peremhálózati útválasztók/kapcsolók, amelyek az ügyfél peremhálózati útválasztók használata során szembesülnek. Tooas PE-CEs ebben a dokumentumban említett.
4.  PEs (MSEE hozzáférhető): szolgáltató peremhálózati útválasztók/kapcsolók használata során szembesülnek MSEEs. Tooas PE-MSEEs ebben a dokumentumban említett.
5.  MSEEs: A Microsoft vállalati peremhálózati (MSEE) ExpressRoute útválasztók
6.  (VNet) virtuális hálózati átjáró
7.  Számítási hello Azure VNet-eszközön

Hello felhőalapú Exchange közös elhelyezés vagy Point-to-Point protokoll Ethernet-kapcsolat modellek használata esetén hello ügyfél peremhálózati útválasztója (2) a BGP társviszony-létesítés (5) MSEEs hozzák létre. 3. és 4 hálózati pontok volna továbbra is létezik, de lehet némileg átlátszó 2. rétegbeli eszközök.

Hello bármely elem közöttiként (IPVPN) kapcsolat modellt használja, ha hello PEs (MSEE hozzáférhető) (4) hozzák létre a BGP társviszony-létesítés MSEEs (5). Útvonalak majd továbbítja hátsó toohello ügyfél hálózati hello IPVPN szolgáltatás szolgáltató hálózaton keresztül.

>[!NOTE]
>Microsoft ExpressRoute magas rendelkezésre állás, a BGP-munkamenetek között MSEEs (5) és a PE-MSEEs (4) a redundáns pár van szükség. Hálózati elérési út redundáns két szintén javasolt felhasználói hálózat és a PE-CEs között. Bármely elem közöttiként (IPVPN) kapcsolat modellben, azonban egyetlen CE eszközt (2) lehet csatlakoztatott tooone vagy további PEs (3).
>
>

egy ExpressRoute-kapcsolatcsoportot, a következő lépéseket hello toovalidate (a hello hálózati pont kapcsolódó hello számot jelzi) tartoznak:
1. [Ellenőrizze a kiépítésével és állapota (5)](#validate-circuit-provisioning-and-state)
2. [Ellenőrizze a legalább egy ExpressRoute-társviszony létesítése – konfigurálva (5)](#validate-peering-configuration)
3. [ARP ellenőrzése a Microsoft és hello szolgáltató (4. és 5 közötti kapcsolat) között](#validate-arp-between-microsoft-and-the-service-provider)
4. [A BGP és útvonalak hello MSEE (BGP-4 too5, és 5 too6, ha csatlakozik egy VNet között) a ellenőrzése](#validate-bgp-and-routes-on-the-msee)
5. [Ellenőrizze a hello forgalom statisztika (5 áthaladó forgalom)](#check-the-traffic-statistics)

További ellenőrzések és ellenőrzések hozzáadódnak hello jövőbeli, próbálkozzon újra a havi!

##<a name="validate-circuit-provisioning-and-state"></a>Kiépítésével és állapotának ellenőrzése
Hello kapcsolat modell függetlenül ExpressRoute-kapcsolatcsoportot rendelkezik létrehozott toobe, és így a szolgáltatás hozza létre a kiépítésével kulcsot. A redundáns 2. rétegbeli kapcsolatot PE-MSEEs (4) és MSEEs (5) között ExpressRoute-kapcsolatcsoportot kiépítés hoz létre. Hogyan toocreate, módosítás, kiépíteni, és ellenőrizze a ExpressRoute-kapcsolatcsoportot további információkért lásd: hello cikk [létrehozása és módosítása az ExpressRoute-kapcsolatcsoportot][CreateCircuit].

>[!TIP]
>A szolgáltatás kulcs egyedileg azonosítja az ExpressRoute-kapcsolatcsoportot. A kulcs mindenképpen szükséges a legtöbb ebben a dokumentumban említett hello powershell-parancsokat. Emellett kell segítségre van szüksége Microsoft és az ExpressRoute-partner tootroubleshoot ExpressRoute problémát, adja meg a hello szolgáltatás fő tooreadily hello áramkör azonosításához.
>
>

###<a name="verification-via-hello-azure-portal"></a>Ellenőrzési hello Azure-portálon keresztül
Hello Azure-portálon, az ExpressRoute-kapcsolatcsoportot hello állapotának ellenőrizhetők kiválasztásával ![2][2] hello a bal oldali oldalsó sáv menüre, és jelölje be hello ExpressRoute-kapcsolatcsoportot. Egy ExpressRoute kiválasztása áramkör "Összes erőforrás" alatt szereplő hello ExpressRoute-kapcsolatcsoport panel megnyitása. A hello ![3][3] hello panelen hello essentials találhatók, ahogy az alábbi képernyőfelvétel a hello ExpressRoute szakaszában:

![4][4]    

Az ExpressRoute Essentials hello *állapot áramkör* hello kapcsolatnak az ügyféloldali Microsoft hello hello állapotát jelzi. *Szolgáltató állapota* azt jelzi, hogy lett-e hello áramkör *kiépítve/nem létesített* hello szolgáltatói oldalán. 

Toobe működési egy ExpressRoute körön, hello *állapot áramkör* kell *engedélyezve* és hello *szolgáltató állapota* kell *kiépítve*.

>[!NOTE]
>Ha hello *állapot áramkör* van nincs engedélyezve, forduljon a [Microsoft Support][Support]. Ha hello *szolgáltató állapota* van nincs telepítve, forduljon a szolgáltatójához.
>
>

###<a name="verification-via-powershell"></a>PowerShell ellenőrzése
toolist összes hello ExpressRoute-Kapcsolatcsoportok erőforráscsoportban, a következő parancs hello használata:

    Get-AzureRmExpressRouteCircuit -ResourceGroupName "Test-ER-RG"

>[!TIP]
>Az erőforráscsoport neve hello Azure-portálon keresztül érheti el. Lásd az hello előző ebben a dokumentumban, és vegye figyelembe, hogy hello erőforráscsoport-név szerepel hello példa képernyőképet.
>
>

egy adott ExpressRoute-kapcsolatcsoportot erőforráscsoportban, a következő parancs használata hello tooselect:

    Get-AzureRmExpressRouteCircuit -ResourceGroupName "Test-ER-RG" -Name "Test-ER-Ckt"

A rendszer egy minta választ:

    Name                             : Test-ER-Ckt
    ResourceGroupName                : Test-ER-RG
    Location                         : westus2
    Id                               : /subscriptions/***************************/resourceGroups/Test-ER-RG/providers/***********/expressRouteCircuits/Test-ER-Ckt
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                        "Name": "Standard_UnlimitedData",
                                        "Tier": "Standard",
                                        "Family": "UnlimitedData"
                                        }
    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : Provisioned
    ServiceProviderNotes             : 
    ServiceProviderProperties        : {
                                        "ServiceProviderName": "****",
                                        "PeeringLocation": "******",
                                        "BandwidthInMbps": 100
                                        }
    ServiceKey                       : **************************************
    Peerings                         : []
    Authorizations                   : []

tooconfirm ExpressRoute-kapcsolatcsoportot működik, ha fordítson különös figyelmet toohello a következő mezőket:

    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : Provisioned

>[!NOTE]
>Ha hello *CircuitProvisioningState* van nincs engedélyezve, forduljon a [Microsoft Support][Support]. Ha hello *ServiceProviderProvisioningState* van nincs telepítve, forduljon a szolgáltatójához.
>
>

###<a name="verification-via-powershell-classic"></a>Ellenőrzési PowerShell (klasszikus)
toolist összes hello ExpressRoute-Kapcsolatcsoportok adott előfizetésen belül, a következő parancs hello használata:

    Get-AzureDedicatedCircuit

egy adott ExpressRoute-kapcsolatcsoportot tooselect hello a következő parancsot használja:

    Get-AzureDedicatedCircuit -ServiceKey **************************************

A rendszer egy minta választ:

    andwidth                         : 100
    BillingType                      : UnlimitedData
    CircuitName                      : Test-ER-Ckt
    Location                         : westus2
    ServiceKey                       : **************************************
    ServiceProviderName              : ****
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

tooconfirm ExpressRoute-kapcsolatcsoportot működik, ha nagy a következő mezők különös figyelmet toohello: ServiceProviderProvisioningState: kiépített állapot: engedélyezve

>[!NOTE]
>Ha hello *állapot* van nincs engedélyezve, forduljon a [Microsoft Support][Support]. Ha hello *ServiceProviderProvisioningState* van nincs telepítve, forduljon a szolgáltatójához.
>
>

##<a name="validate-peering-configuration"></a>Társviszony-létesítési konfiguráció ellenőrzése
Hello szolgáltatónak befejezett hello hello ExpressRoute-kapcsolatcsoportot kiépítés, miután egy útválasztási konfigurációja hello ExpressRoute-kapcsolatcsoport között MSEE-PRs (4) és MSEEs (5) keresztül hozhatók létre. Minden egyes ExpressRoute-kapcsolatcsoportot rendelkezhet egy, kettő vagy három útválasztási környezetek engedélyezve: Azure magánhálózati társviszony-létesítés (forgalom tooprivate virtuális hálózatok az Azure-ban), az Azure nyilvános társviszony-létesítés (forgalom toopublic IP-címek az Azure-ban) és a Microsoft társviszony-létesítés (forgalom tooOffice 365 és Dynamics 365). További információt a toocreate és útválasztási konfigurációjának módosításához hello cikke [létrehozása és módosítása az ExpressRoute-kapcsolatcsoportot útválasztási][CreatePeering].

###<a name="verification-via-hello-azure-portal"></a>Ellenőrzési hello Azure-portálon keresztül
>[!IMPORTANT]
>Egy ismert hiba van hello Azure portál, amelynek ExpressRoute-társviszony vannak *nem* hello szolgáltató konfiguráltságát hello portálon is látható. ExpressRoute-társviszony hello portál vagy PowerShell hozzáadása *hello szolgáltatás Szolgáltatóbeállítások felülírja*. Ez a művelet megszakítja a hello hello ExpressRoute-kapcsolatcsoportot az Útválasztás és hello szolgáltatás szolgáltató toorestore hello beállítások hello támogatnia kell a, és helyreállítani a normál útválasztást. Hello ExpressRoute-társviszony csak akkor módosítsa, ha biztos, hogy hello szolgáltató biztosítja a csak a 2. réteg szolgáltatásokat!
>
>

<p/>
>[!NOTE]
>Ha 3 rétegbeli feltéve, hogy által hello szolgáltatás szolgáltató és hello esetében üres hello portálon, a PowerShell lehet használt toosee hello szolgáltatást konfigurált szolgáltató beállításai.
>
>

Hello Azure-portálon, az ExpressRoute-kapcsolatcsoportot állapotának ellenőrizhetők kiválasztásával ![2][2] hello a bal oldali oldalsó sáv menüre, és jelölje be hello ExpressRoute-kapcsolatcsoportot. Egy ExpressRoute kiválasztása "Összes erőforrás" alatt szereplő áramkör nyitna hello ExpressRoute-kapcsolatcsoport panelen. A hello ![3][3] hello panelen hello essentials kellene szerepelnie, ahogy az alábbi képernyőfelvétel a hello ExpressRoute szakaszában:

![5][5]

A fenti példa hello mint beállításértékeket Azure magánhálózati társviszony-létesítési útválasztási környezet engedélyezve van, mivel nyilvános Azure és a Microsoft társviszony-létesítési útválasztási környezetek nem engedélyezett. Sikeresen engedélyezve a társviszony-létesítési környezetben is kellene hello elsődleges és másodlagos Point-to-Point protokoll (BGP szükséges) alhálózat szerepel. hello/30-as alhálózat hello MSEEs hello illesztő IP-címét és a PE-MSEEs használ. 

>[!NOTE]
>Társviszony-létesítés nem engedélyezett, ha ellenőrizze, hogy egyeznek-e hozzárendelve hello elsődleges és másodlagos alhálózat PE-MSEEs hello konfigurációt. Ha nem, toochange hello konfigurációs MSEE útválasztók, tekintse meg a túl[létrehozása és módosítása az ExpressRoute-kapcsolatcsoportot útválasztási][CreatePeering]
>
>

###<a name="verification-via-powershell"></a>PowerShell ellenőrzése
tooget hello Azure titkos társviszony konfigurációs részletek, használja a következő parancsok hello:

    $ckt = Get-AzureRmExpressRouteCircuit -ResourceGroupName "Test-ER-RG" -Name "Test-ER-Ckt"
    Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -Circuit $ckt

A minta választ, egy sikeresen konfigurált magánhálózati társviszony-létesítés, a következő:

    Name                       : AzurePrivatePeering
    Id                         : /subscriptions/***************************/resourceGroups/Test-ER-RG/providers/***********/expressRouteCircuits/Test-ER-Ckt/peerings/AzurePrivatePeering
    Etag                       : W/"################################"
    PeeringType                : AzurePrivatePeering
    AzureASN                   : 12076
    PeerASN                    : ####
    PrimaryPeerAddressPrefix   : 172.16.0.0/30
    SecondaryPeerAddressPrefix : 172.16.0.4/30
    PrimaryAzurePort           : 
    SecondaryAzurePort         : 
    SharedKey                  : 
    VlanId                     : 300
    MicrosoftPeeringConfig     : null
    ProvisioningState          : Succeeded

 Sikeresen engedélyezve a társviszony-létesítési környezetben hello elsődleges és másodlagos címelőtagokat felsorolt kellene lennie. hello/30-as alhálózat hello MSEEs hello illesztő IP-címét és a PE-MSEEs használ.

tooget hello Azure nyilvános társviszony konfigurációs részletek, használja a következő parancsok hello:

    $ckt = Get-AzureRmExpressRouteCircuit -ResourceGroupName "Test-ER-RG" -Name "Test-ER-Ckt"
    Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -Circuit $ckt

tooget hello Microsoft társviszony-létesítési konfiguráció részletei, a következő parancsok hello használata:

    $ckt = Get-AzureRmExpressRouteCircuit -ResourceGroupName "Test-ER-RG" -Name "Test-ER-Ckt"
    Get-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -Circuit $ckt

Ha nincs konfigurálva a társviszony-létesítés, nem lenne egy hibaüzenetet. Mintát választ, ha hello közölt társviszony-létesítés (Azure nyilvános társviszony-létesítés ebben a példában) belül hello körhöz nincs konfigurálva:

    Get-AzureRmExpressRouteCircuitPeeringConfig : Sequence contains no matching element
    At line:1 char:1
        + Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering ...
        + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
            + CategoryInfo          : CloseError: (:) [Get-AzureRmExpr...itPeeringConfig], InvalidOperationException
            + FullyQualifiedErrorId : Microsoft.Azure.Commands.Network.GetAzureExpressRouteCircuitPeeringConfigCommand


<p/>
>[!NOTE]
>Ha nincs engedélyezve a társviszony-létesítés, ellenőrizze, ha hello hozzárendelt elsődleges és másodlagos alhálózat egyezés hello konfigurációs hello csatolt PE-MSEE. Ellenőrizze, hogy hello kijavíthatja is *VlanId*, *AzureASN*, és *PeerASN* MSEEs használja, és ha ezeket az értékeket leképezi a hello lévőktől toohello csatolt PE-MSEE. Az MD5 kivonatoló választása esetén hello megosztott kulcsot kell ugyanazon a MSEE és a PE-MSEE pár. toochange hello konfigurációs hello MSEE útválasztók, tekintse meg a túl [létrehozása és módosítása az ExpressRoute-kapcsolatcsoportot útválasztási] [CreatePeering].  
>
>

### <a name="verification-via-powershell-classic"></a>Ellenőrzési PowerShell (klasszikus)
tooget hello Azure titkos társviszony konfigurációs részletek hello a következő parancsot használja:

    Get-AzureBGPPeering -AccessType Private -ServiceKey "*********************************"

A rendszer egy minta választ, egy sikeresen konfigurált magánhálózati társviszony-létesítés esetében:

    AdvertisedPublicPrefixes       : 
    AdvertisedPublicPrefixesState  : Configured
    AzureAsn                       : 12076
    CustomerAutonomousSystemNumber : 
    PeerAsn                        : ####
    PrimaryAzurePort               : 
    PrimaryPeerSubnet              : 10.0.0.0/30
    RoutingRegistryName            : 
    SecondaryAzurePort             : 
    SecondaryPeerSubnet            : 10.0.0.4/30
    State                          : Enabled
    VlanId                         : 100

A sikeres legyen, engedélyezve társviszony-létesítési környezetben hello elsődleges és másodlagos társ alhálózatok felsorolt kellene lennie. hello/30-as alhálózat hello MSEEs hello illesztő IP-címét és a PE-MSEEs használ.

tooget hello Azure nyilvános társviszony konfigurációs részletek, használja a következő parancsok hello:

    Get-AzureBGPPeering -AccessType Public -ServiceKey "*********************************"

tooget hello Microsoft társviszony-létesítési konfiguráció részletei, a következő parancsok hello használata:

    Get-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************"

>[!IMPORTANT]
>3 rétegbeli társviszony hello szolgáltató által beállított, hello ExpressRoute-társviszony hello portál vagy PowerShell beállítás felülírja a hello szolgáltatás szolgáltató beállításai. Hello szolgáltató ügyféloldali társviszony-létesítési beállításainak visszaállításáról hello támogatás hello szolgáltató szükséges. Hello ExpressRoute-társviszony csak akkor módosítsa, ha biztos, hogy hello szolgáltató biztosítja a csak a 2. réteg szolgáltatásokat!
>
>

<p/>
>[!NOTE]
>Ha nincs engedélyezve a társviszony-létesítés, ellenőrizze, ha hello elsődleges és másodlagos társ rendelt alhálózatok egyezés hello konfigurációs hello csatolt PE-MSEE. Ellenőrizze, hogy hello kijavíthatja is *VlanId*, *AzureAsn*, és *PeerAsn* MSEEs használja, és ha ezeket az értékeket leképezi a hello lévőktől toohello csatolt PE-MSEE. toochange hello konfigurációs hello MSEE útválasztók, tekintse meg a túl [létrehozása és módosítása az ExpressRoute-kapcsolatcsoportot útválasztási] [CreatePeering].
>
>

## <a name="validate-arp-between-microsoft-and-hello-service-provider"></a>ARP ellenőrzése a Microsoft és hello szolgáltató között
Ez a szakasz (klasszikus) PowerShell-parancsokat használ. Ha használt PowerShell Azure Resource Manager parancsok, győződjön meg arról, hogy rendelkezik-e rendszergazdai vagy társadminisztrátori hozzáférés toohello előfizetésébe [a klasszikus Azure portálon][OldPortal]. A hibaelhárítás Azure Resource Manager segítségével parancsok tekintse meg az toohello [első ARP táblák hello Resource Manager üzembe helyezési modellel] [ ARP] dokumentum.

>[!NOTE]
>tooget ARP, hello Azure-portálon, mind az Azure Resource Manager PowerShell-parancsok használhatók. Ha hiba történik, hello Azure Resource Manager PowerShell-parancsokkal, klasszikus PowerShell-parancsok klasszikus PowerShell parancsokat is dolgozhat Azure Resource Manager ExpressRoute-Kapcsolatcsoportok használható.
>
>

tooget hello hello elsődleges MSEE útválasztó hello magánhálózati társviszony-létesítés ARP a táblázatot, a következő parancs hello használata:

    Get-AzureDedicatedCircuitPeeringArpInfo -AccessType Private -Path Primary -ServiceKey "*********************************"

Egy példa egy válasz hello parancs sikeres forgatókönyvben hello:

    ARP Info:

                 Age           Interface           IpAddress          MacAddress
                 113             On-Prem       10.0.0.1           e8ed.f335.4ca9
                   0           Microsoft       10.0.0.2           7c0e.ce85.4fc9

Hasonló módon ellenőrizheti a hello hello MSEE a hello ARP tábla *elsődleges*/*másodlagos* elérési útja, a *titkos* /  *Nyilvános*/*Microsoft* esetében.

hello alábbi példa azt mutatja be a társviszony-létesítés hello parancs válasz hello nem létezik.

    ARP Info:
       
>[!NOTE]
>Ha hello ARP-táblázat nem rendelkezik a hello felületek IP-címek hozzárendelt tooMAC címeket, felülvizsgálati hello a következő információkat:
>1. Ha hello hivatkozás közötti hozzárendelt első IP-cím hello hello/30-as alhálózat MSEE-PR hello és MSEE MSEE-PR. hello felületén szolgál Azure mindig MSEEs hello második IP-címet használja.
>2. Győződjön meg arról, ha hello ügyfél (C-címke) és VLAN-címkék szolgáltatás (S-címke) felel meg a MSEE-PR és MSEE pár mindkét.
>
>

## <a name="validate-bgp-and-routes-on-hello-msee"></a>Ellenőrizze a BGP és hello MSEE útvonalak
Ez a szakasz (klasszikus) PowerShell-parancsokat használ. Ha használt PowerShell Azure Resource Manager parancsok, győződjön meg arról, hogy rendelkezik-e rendszergazdai vagy társadminisztrátori hozzáférés toohello előfizetésébe [a klasszikus Azure portálon][OldPortal]

>[!NOTE]
>tooget BGP információ mindkét hello Azure-portál és az Azure Resource Manager PowerShell-parancsok is használhatók. Ha hiba történik, hello Azure Resource Manager PowerShell-parancsokkal, klasszikus PowerShell-parancsok klasszikus PowerShell parancsokat is dolgozhat Azure Resource Manager ExpressRoute-Kapcsolatcsoportok használható.
>
>

tooget hello egy adott útválasztási környezet összefoglaló útválasztási táblázathoz (BGP szomszéd), a következő parancs hello használata:

    Get-AzureDedicatedCircuitPeeringRouteTableSummary -AccessType Private -Path Primary -ServiceKey "*********************************"

Egy példa egy válasz a következő:

    Route Table Summary:

            Neighbor                   V                  AS              UpDown         StatePfxRcd
            10.0.0.1                   4                ####                8w4d                  50

Ahogy az előző példa hello, hello parancs a hello útválasztási környezet létrehozása mennyi ideig hasznos toodetermine. Azt is hello társviszony-létesítési útválasztó által hirdetett útvonal előtagok számát jelzi.

>[!NOTE]
>Ha hello állapota aktív vagy inaktív, ellenőrizze, ha hello elsődleges és másodlagos társ rendelt alhálózatok egyezés hello konfigurációs hello csatolt PE-MSEE. Ellenőrizze, hogy hello kijavíthatja is *VlanId*, *AzureAsn*, és *PeerAsn* MSEEs használja, és ha ezeket az értékeket leképezi a hello lévőktől toohello csatolt PE-MSEE. Az MD5 kivonatoló választása esetén hello megosztott kulcsot kell ugyanazon a MSEE és a PE-MSEE pár. toochange hello konfigurációs hello MSEE útválasztók, tekintse meg a túl[létrehozása és módosítása az ExpressRoute-kapcsolatcsoportot útválasztási][CreatePeering].
>
>

<p/>
>[!NOTE]
>Egyes célhoz keresztül adott társviszony-létesítés nem érhetők el, ha ellenőrizze hello útválasztási táblázatot az hello MSEEs tartozó toohello adott társviszony-létesítési környezetben. Ha egy egyező előtag (Ez lehet gelt IP) hello útválasztási táblázatban szerepel, akkor ellenőrizze Ha tűzfalak/NSG/hozzáférés-vezérlési listák hello elérési úton, és ha azok hello forgalom engedélyezése.
>
>

tooget hello teljes útválasztási táblázatot MSEE a hello *elsődleges* adott hello elérési útját *titkos* útválasztási környezetben, a következő parancs használata hello:

    Get-AzureDedicatedCircuitPeeringRouteTableInfo -AccessType Private -Path Primary -ServiceKey "*********************************"

Egy példa sikeres hello parancs eredménye:

    Route Table Info:

             Network             NextHop              LocPrf              Weight                Path
         10.1.0.0/16            10.0.0.1                                       0    #### ##### #####     
         10.2.0.0/16            10.0.0.1                                       0    #### ##### #####
    ...

Hasonlóképpen, ellenőrizheti a hello útválasztási táblázatot hello MSEE a hello *elsődleges*/*másodlagos* elérési útja, a *titkos* / *Nyilvános*/*Microsoft* társviszony-létesítési környezetben.

hello alábbi példa azt mutatja be a társviszony-létesítés hello parancs válasz hello nem létezik:

    Route Table Info:

##<a name="check-hello-traffic-statistics"></a>Ellenőrizze a hello forgalom statisztikák
tooget hello elsődleges és másodlagos elérési forgalom statisztika--bájt bejövő és kimenő adatforgalma – kombinált társviszony-létesítési környezetének, a következő parancs használata hello:

    Get-AzureDedicatedCircuitStats -ServiceKey 97f85950-01dd-4d30-a73c-bf683b3a6e5c -AccessType Private

Egy minta hello parancs kimenete:

    PrimaryBytesIn PrimaryBytesOut SecondaryBytesIn SecondaryBytesOut
    -------------- --------------- ---------------- -----------------
         240780020       239863857        240565035         239628474

A minta a nem létező társviszony-létesítés hello parancs kimenete:

    Get-AzureDedicatedCircuitStats : ResourceNotFound: Can not find any subinterface for peering type 'Public' for circuit '97f85950-01dd-4d30-a73c-bf683b3a6e5c' .
    At line:1 char:1
    + Get-AzureDedicatedCircuitStats -ServiceKey 97f85950-01dd-4d30-a73c-bf ...
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : CloseError: (:) [Get-AzureDedicatedCircuitStats], CloudException
        + FullyQualifiedErrorId : Microsoft.WindowsAzure.Commands.ExpressRoute.GetAzureDedicatedCircuitPeeringStatsCommand

## <a name="next-steps"></a>Következő lépések
További információt vagy a help tekintse meg a következő hivatkozások hello:

- [A Microsoft támogatás][Support]
- [Létrehozásához és módosításához ExpressRoute-kapcsolatcsoportot][CreateCircuit]
- [Létrehozásához és módosításához útválasztás az ExpressRoute-kapcsolatcsoportot][CreatePeering]

<!--Image References-->
[1]: ./media/expressroute-troubleshooting-expressroute-overview/expressroute-logical-diagram.png "Logikai Expressroute-kapcsolat"
[2]: ./media/expressroute-troubleshooting-expressroute-overview/portal-all-resources.png "Minden erőforrás ikon"
[3]: ./media/expressroute-troubleshooting-expressroute-overview/portal-overview.png "Áttekintés ikon"
[4]: ./media/expressroute-troubleshooting-expressroute-overview/portal-circuit-status.png "ExpressRoute Essentials minta képernyőképe"
[5]: ./media/expressroute-troubleshooting-expressroute-overview/portal-private-peering.png "ExpressRoute Essentials minta képernyőképe"

<!--Link References-->
[Support]: https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade
[CreateCircuit]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-circuit-portal-resource-manager 
[CreatePeering]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-routing-portal-resource-manager
[OldPortal]: https://manage.windowsazure.com
[ARP]: https://docs.microsoft.com/en-us/azure/expressroute/expressroute-troubleshooting-arp-resource-manager






