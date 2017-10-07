---
title: "Konfigurációs részletek aaaNetwork az Express Route használata"
description: "Hálózati konfiguráció részletei App Service Environment-környezetek futó virtuális hálózatok tooan ExpressRoute-Kapcsolatcsoportot csatlakoztatva."
services: app-service
documentationcenter: 
author: stefsch
manager: nirma
editor: 
ms.assetid: 34b49178-2595-4d32-9b41-110c96dde6bf
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/14/2016
ms.author: stefsch
ms.openlocfilehash: 85bbc45cfed619485957ee2a898aa0a7604173cb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="network-configuration-details-for-app-service-environments-with-expressroute"></a>Az ExpressRoute-kapcsolattal rendelkező App Service Environments-környezetek hálózati konfigurációjának részletei
## <a name="overview"></a>Áttekintés
Az ügyfelek kapcsolódhatnak egy [Azure ExpressRoute] [ ExpressRoute] áramkör tootheir virtuális hálózati infrastruktúra, így kiterjesztése a helyszíni hálózati tooAzure.  Az App Service-környezetek hozhatók létre az alhálózatot [virtuális hálózati] [ virtualnetwork] infrastruktúra.  App Service Environment-környezet hello futó alkalmazások majd létesíthet biztonságos kapcsolatok tooback-a befejezési erőforrások elérhető csak keresztül hello ExpressRoute-kapcsolatot.  

Az App Service-környezetek hozhatók létre **vagy** az Azure Resource Manager virtuális hálózati **vagy** klasszikus telepítési modell virtuális hálózatot.  2016. június végzett legutóbbi módosítását ASEs is most is telepíthető az nyilvános címtartományt, vagy RFC1918 címterek (azaz Magáncímeket) használó virtuális hálózatok. 

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="required-network-connectivity"></a>Szükséges hálózati kapcsolat
Nincsenek App Service Environment-környezetek, előfordulhat, hogy kezdetben jutott egy csatlakoztatott virtuális hálózati tooan ExpressRoute a hálózati kapcsolat követelményei.  App Service-környezetek követelje rendelés toofunction hello következő megfelelően:

* Kimenő hálózati kapcsolat tooAzure tárolási végpontok világszerte mindkét 80-as és 443-as porton.  Ez magában foglalja a hello található végpontok és hello App Service Environment-környezet ugyanabban a régióban, valamint tárolási végpontok található **más** Azure-régiók.  Azure Storage-végpontok oldja meg a következő DNS-tartományok hello: *table.core.windows.net*, *blob.core.windows.net*, *queue.core.windows.net* és *file.core.windows.net*.  
* Kimenő hálózati kapcsolat toohello Azure fájlok szolgáltatás 445-ös porton.
* Kimenő hálózati kapcsolat tooSql DB végpont található hello azonos régióban, az App Service Environment-környezet hello.  SQL-adatbázis a végpontok oldja meg a tartomány a következő hello: *database.windows.net*.  Ehhez a hozzáférési tooports 1433-as számú 11000-11999 és 14000-14999 megnyitása.  További információ: [Ez a cikk a Sql Database 12-es port használati](../sql-database/sql-database-develop-direct-route-ports-adonet-v12.md).
* Kimenő hálózati kapcsolat toohello Azure felügyeleti vezérlősík végpontok (ASM, mind az ARM-végpontok).  Ez magában foglalja a kimenő kapcsolat tooboth *management.core.windows.net* és *management.azure.com*. 
* Kimenő hálózati kapcsolatra túl*ocsp.msocsp.com*, *mscrl.microsoft.com* és *crl.microsoft.com*.  Ez a szükséges toosupport SSL funkciót.
* hello virtuális hálózat DNS-konfiguráció hello megoldásának hello végpontok képesnek kell lennie, és a tartományok említett hello korábbi pontok.  Ezeket a végpontokat nem oldható fel, ha az App Service Environment-környezet létrehozása kísérletek sikertelenek lesznek, és meglévő App Service Environment-környezetek sérültként lesz megjelölve.
* Kimenő hozzáférést 53-as porton szükség, DNS-kiszolgálókkal való kommunikációhoz.
* Ha egy egyéni DNS-kiszolgáló létezik a VPN-átjáró másik végén hello, hello DNS-kiszolgálót tartalmazó hello App Service Environment-környezet hello alhálózatból elérhetőnek kell lennie. 
* hello kimenő hálózati elérési út nem haladnak keresztül belső vállalati proxyk, sem annak kényszerített bújtatott tooon helyszíni.  Ennek során, a módosítások hello hello App Service Environment-környezet a kimenő hálózati forgalom hatékony NAT-cím.  Ha módosítja a hello NAT-cím egy App Service Environment kimenő hálózati forgalom, akkor a fent felsorolt hibák toomany hello végpontok kapcsolat.  Az eredmény App Service Environment-környezet létrehozása sikertelen kísérleteket, valamint a korábban kifogástalan App Service Environment-környezetek megfelelő állapotúként lett megjelölve.  
* Az App Service Environment-környezetek engedélyezni kell az ezzel a bejövő forgalmat kezelő hálózati hozzáférési toorequired portok [cikk][requiredports].

egy érvényes DNS-infrastruktúra hello virtuális hálózat megmarad és konfigurált biztosításával hello DNS-követelmények érheti el.  Ha bármilyen okból hello DNS-konfiguráció esetén megváltozik egy App Service Environment-környezet létrehozása után, a fejlesztők kényszerítheti az App Service Environment-környezet toopick hello új DNS-konfiguráció.  Működés közbeni környezet újraindítás hello App Service Environment-környezet felügyeleti panel az hello hello tetején található ikon "Restart" hello segítségével váltanak [Azure-portálon] [ NewPortal] hello környezet toopick, akkor másolatot hello új DNS-konfiguráció.

hello bejövő hálózati hozzáférési követelmények érheti el, úgy konfigurálja a [hálózati biztonsági csoport] [ NetworkSecurityGroups] hello App Service Environment alhálózati tooallow szükséges hello hozzáférést a leírtak[cikk][requiredports].

## <a name="enabling-outbound-network-connectivity-for-an-app-service-environment"></a>Az App Service Environment-környezet kimenő hálózati kapcsolat engedélyezése
Alapértelmezés szerint egy újonnan létrehozott ExpressRoute-kapcsolatcsoportot hirdeti egy alapértelmezett útvonalat, amely lehetővé teszi a kimenő internetkapcsolat.  Ez a konfiguráció az App Service Environment-környezet lesz képes tooconnect tooother Azure-végpontok.

Azonban egy közös felhasználói konfigurálása toodefine a saját alapértelmezett útvonalat (0.0.0.0/0), amely arra kényszeríti a kimenő Internet tooinstead adatforgalmat helyszíni.  A forgalom áramlását tüntetnek megsérti App Service Environment-környezetek, mert hello kimenő adatforgalmat a letiltott helyi, vagy NAT-d tooan felismerhetetlen címek, amelyek már nem használhatók a különböző Azure-végpontok készlete.

egy (vagy több) toodefine felhasználó által megadott útvonalakat (udr-EK), amely tartalmazza az App Service Environment-környezet hello hello alhálózaton hello megoldás.  Egy UDR hello alapértelmezett útvonal helyett szembeni szerződéses kötelezettségeket vonatkozó alhálózati útvonalakat határozza meg.

Ha lehetséges a következő konfigurációs toouse hello ajánlott:

* hello ExpressRoute konfigurációs hirdeti 0.0.0.0/0, és alapértelmezés szerint kényszerített bújtatja minden kimenő forgalom helyszíni.
* hello alkalmazott UDR toohello alhálózati hello App Service Environment-környezet tartalmazó 0.0.0.0/0 az interneten (például az az ebben a cikkben le megtalálhatók) a következő ugrás típusa határozza meg.

hello kombinált hatását, hogy ezeket a lépéseket az, hogy hello alhálózat-szintű UDR elsőbbséget élvez hello ExpressRoute kényszerített bújtatás, biztosítva ezzel az App Service Environment-környezet hello kimenő Internet-hozzáféréssel.

> [!IMPORTANT]
> egy UDR definiált útvonalak hello **kell** kell elég konkrétan fogalmaz ahhoz túl hello ExpressRoute-konfiguráció bármely útvonalak keresztül elsőbbséget hirdeti.  az alábbi példa hello hello széleskörű 0.0.0.0/0 címtartomány használ, és ilyen potenciálisan véletlenül felülbírálhatja útvonal-hirdetéseinek pontosabb címtartományai használatával.
> 
> App Service Environment-környezetek ExpressRoute-konfigurációk nem támogatottak, amelyek **határokon hirdetési hello nyilvános társviszony-létesítési elérési toohello magánhálózati társviszony-létesítési elérési útvonalak**.  ExpressRoute-konfigurációk, amelyek rendelkeznek a nyilvános társviszony konfigurálva, a Microsoft Azure IP-címtartományok számos útvonal-hirdetéseinek kap Microsoft.  Ha a cím tartományok közötti meghirdetett hello magánhálózati társviszony-létesítési elérési úton, hello end eredménye, hogy minden kimenő hálózati csomagokat a hello App Service Environment-alhálózatból lesz kényszerített bújtatott tooa az ügyfél a helyi hálózati infrastruktúra.  A hálózati folyamat jelenleg nem támogatott az App Service Environment-környezetek.  Az egyik megoldás toothis probléma toostop cross-hirdetési útvonalak hello nyilvános társviszony-létesítési elérési toohello magánhálózati társviszony-létesítési elérési útról.
> 
> 

Háttér-információkat a felhasználó által megadott útvonalakat érhető el ezen [áttekintése][UDROverview].  

Létrehozása és konfigurálása a felhasználó által megadott útvonalakat érhető el ezen [hogyan tooGuide][UDRHowTo].

## <a name="example-udr-configuration-for-an-app-service-environment"></a>Példa UDR konfiguráció egy App Service Environment-környezet
**Előfeltételek**

1. Telepítse az Azure Powershell hello [Azure letöltőoldala] [ AzureDownloads] (dátummal June 2015-ös vagy újabb).  A "Parancssori eszközök" a "Windows Powershell", amely telepíti majd hello legújabb Powershell-parancsmagok egy "A telepítés" kapcsolat van.
2. Javasoljuk, hogy egyedi alhálózatot kizárólagos használatra létrejön egy App Service Environment-környezet által.  Ez biztosítja, hogy hello udr-EK alkalmazása toohello alhálózaton csak nyissa meg az App Service Environment-környezet hello kimenő forgalmát.
3. **Fontos**: ne telepítsen App Service Environment-környezet csak hello **után** konfigurációs lépések hello követi.  Ez biztosítja, hogy kimenő hálózati kapcsolat érhető el az App Service-környezetek toodeploy megkísérlése előtt.

**1. lépés:, Hozzon létre egy elnevezett útválasztási táblázatot**

hello következő kódrészlettel útvonaltábla létrehozása "DirectInternetRouteTable" nevű hello nyugati Velünk Azure régióban:

    New-AzureRouteTable -Name 'DirectInternetRouteTable' -Location uswest

**2. lépés: Hello útválasztási táblázatot egy vagy több útvonal létrehozása**

Szüksége lesz egy tooadd vagy további útvonalakat toohello útvonaltábla a rendelés tooenable kimenő Internet-hozzáféréssel.  

hello ajánlott megközelítést alkalmazva konfigurálásához kimenő hozzáférést toohello Internet toodefine egy útvonalat a 0.0.0.0/0 az alábbi ábrán látható módon.

    Get-AzureRouteTable -Name 'DirectInternetRouteTable' | Set-AzureRoute -RouteName 'Direct Internet Range 0' -AddressPrefix 0.0.0.0/0 -NextHopType Internet

Ne feledje, hogy a 0.0.0.0/0 a széles körű címtartományt, és így felülbírálják pontosabb címtartományai hello ExpressRoute által hirdetett.  toore-hello többször korábbi ajánlás, egy UDR 0.0.0.0/0 útvonal, csak a 0.0.0.0/0 is hirdeti ExressRoute konfigurációjával együtt kell használni. 

Alternatív megoldásként letöltheti CIDR tartományok Azure használja az átfogó és frissített listáját.  hello összes hello Azure IP-címtartományokat tartalmazó XML-fájl megtalálható a hello [Microsoft Download Center][DownloadCenterAddressRanges].  

Vegye figyelembe, hogy ezek a tartományok változnak az idők, állásához rendszeres manuális toohello felhasználó által definiált útvonalak tookeep szinkronban.  Is, mert nincs olyan alapértelmezett felső korlát az egyetlen UDR 100 útvonalat, meg kell túl "azokat" hello Azure IP-cím címtartományok toofit belül hello 100 útvonal, szem előtt tartani, hogy UDR által definiált útvonalak kell toobe pontosabb, mint által hirdetett hello útvonalak a ExpressRoute.  

**3. lépés: Hello útvonal tábla toohello alhálózati tartalmazó hello App Service Environment-környezet társítása**

hello utolsó konfigurációs lépés tooassociate hello útvonal tábla toohello alhálózaton, hova szeretné telepíteni az App Service Environment-környezet hello.  hello következő parancs hozzárendeli hello "DirectInternetRouteTable" toohello "ASESubnet", amely egy App Service Environment-környezet végül fogja tartalmazni.

    Set-AzureSubnetRouteTable -VirtualNetworkName 'YourVirtualNetworkNameHere' -SubnetName 'ASESubnet' -RouteTableName 'DirectInternetRouteTable'


**4. lépés: Utolsó lépéseit**

Ha hello útvonaltábla kötött toohello alhálózati, az ajánlott toofirst teszt, és erősítse meg, hello szánt hatása.  Például egy virtuális gép üzembe helyezés hello alhálózatot, és ellenőrizze, hogy:

* Kimenő forgalom tooboth Azure és az Azure-végpontok említett korábban a Ez a cikk **nem** hello ExpressRoute-kapcsolatcsoportot lefelé halad.  Nagyon fontos tooverify mindig fogja ezt a viselkedést, mivel ha hello alhálózatból kimenő forgalom továbbra is kényszerített bújtatott a helyszíni App Service-környezet létrehozása sikertelen. 
* DNS-keresések hello végpontok a korábban említett, az összes megoldásán megfelelően. 

Miután megerősítik hello a fenti lépéseket, mert hello alhálózati toobe "üres" hello idő hello App Service Environment-környezet létrehozása a kell toodelete hello virtuális gép.

Ezután folytathatja az App Service-környezetek létrehozása a!

## <a name="getting-started"></a>Bevezetés
Összes cikket, és hogyan-a következőre az App Service Environment-környezetek érhetők el hello [alkalmazásszolgáltatási környezetek – fontos fájl](../app-service/app-service-app-service-environments-readme.md).

Lásd az App Service-környezetek lépései tooget [bemutatása tooApp Service-környezet][IntroToAppServiceEnvironment]

Hello Azure App Service platformmal kapcsolatos további információkért lásd: [Azure App Service][AzureAppService].

<!-- LINKS -->
[virtualnetwork]: http://azure.microsoft.com/services/virtual-network/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[requiredports]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[NetworkSecurityGroups]: http://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[UDROverview]: http://azure.microsoft.com/documentation/articles/virtual-networks-udr-overview/
[UDRHowTo]: http://azure.microsoft.com/documentation/articles/virtual-networks-udr-how-to/
[HowToCreateAnAppServiceEnvironment]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[AzureDownloads]: http://azure.microsoft.com/en-us/downloads/ 
[DownloadCenterAddressRanges]: http://www.microsoft.com/download/details.aspx?id=41653  
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[IntroToAppServiceEnvironment]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[NewPortal]:  https://portal.azure.com


<!-- IMAGES -->
