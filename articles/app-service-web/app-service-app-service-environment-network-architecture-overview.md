---
title: "aaaNetwork architektúra áttekintése az App Service Environment-környezetek"
description: "Hálózati topológia ofApp Service-környezetek architektúra áttekintése."
services: app-service
documentationcenter: 
author: stefsch
manager: erikre
editor: 
ms.assetid: 13d03a37-1fe2-4e3e-9d57-46dfb330ba52
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/04/2016
ms.author: stefsch
ms.openlocfilehash: 3cbc86883f5687a9ada35a3ab2f577a450a3fa0b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="network-architecture-overview-of-app-service-environments"></a>Az App Service Environment-környezetek hálózati architektúrájának áttekintése
## <a name="introduction"></a>Bevezetés
App Service Environment-környezetek mindig létrejönnek az alhálózat egy [virtuális hálózati] [ virtualnetwork] -egy App Service Environment-környezetben futó alkalmazások képes kommunikálni a saját azonos virtuális hello belül található végpontok hálózati topológia.  Mivel az ügyfelek zárolását, előfordulhat, hogy a virtuális hálózati infrastruktúra részeit, is fontos toounderstand hello típusokat, amelyek egy App Service Environment-környezet a hálózati kommunikációs forgalom.

## <a name="general-network-flow"></a>Általános hálózati folyamata
Amikor egy App Service környezetben (ASE) alkalmazások a nyilvános virtuális IP-cím (VIP) használja, minden bejövő forgalom érkezik, a adott nyilvános VIP-e.  Ez magában foglalja az alkalmazások a HTTP és HTTPS-forgalom, valamint a típusú forgalom FTP, távoli hibakeresési szolgáltatás és az Azure felügyeleti műveleteket.  Hello teljes listáját adott portok (szükséges és választható) hello nyilvános VIP elérhető cikke hello a [bejövő forgalom vezérlése] [ controllinginboundtraffic] tooan App Service Environment-környezet. 

App Service Environment-környezetek is támogatja a futó alkalmazások kötött csak tooa virtuális hálózat belső cím, más néven tooas ILB (belső terheléselosztó) címet.  Az egy ILB engedélyezett ASE, HTTP és HTTPS-forgalmat az alkalmazások, valamint a távoli hibakeresési hívások, hello ILB cím ügyfélszámítógépekre érkeznek.  Leggyakoribb ILB-ASE konfigurációk FTP-/ FTPS-forgalom lesz is az ügyfélszámítógépekre érkeznek hello ILB cím.  Az Azure felügyeleti műveletek továbbra is erdőtől áramolnak a hello nyilvános VIP egy ILB a 454/455 tooports azonban engedélyezve van a ASE.

az alábbi ábrán hello áttekintését mutatja hello különböző bejövő és kimenő hálózati forgalom egy App Service Environment-környezet kötött tooa nyilvános virtuális IP-cím hello alkalmazások esetén:

![Általános hálózati forgalom][GeneralNetworkFlows]

Egy App Service Environment-környezet képes kommunikálni a saját felhasználói végpontok különböző.  Például az App Service Environment-környezet képes kapcsolódni az infrastruktúra-szolgáltatási virtuális gépeken futó toodatabase kiszolgáló(k) hello futó alkalmazások hello ugyanazon virtuális hálózati topológiától.

> [!IMPORTANT]
> Hello Hálódiagram megnézi, hello "egyéb számítási erőforrások" telepítése egy másik alhálózat a hello App Service Environment-környezet. A hello erőforrásokat üzembe helyezi ugyanazon az alhálózaton hello mértékéig blokkolja ASE toothose erőforrások (kivéve az adott helyen belüli-ASE Útválasztás) közötti kapcsolatot. Tooa telepítése másik alhálózat helyette (az ugyanazon virtuális hello). hello App Service Environment-környezet képes tooconnect lesz. Nincs szükség további konfigurációra.
> 
> 

App Service-környezetek is kommunikálni az Sql-adatbázis és az Azure Storage felügyeletével és üzemeltetésével az App Service-környezetek szükséges erőforrásokat.  Néhány hello Sql- és tárolási erőforrásokat, amelyek az App Service-környezetek kommunikál hello találhatók hello App Service Environment-környezet, míg mások távoli Azure-régiók találhatók és ugyanabban a régióban.  Ennek eredményeképpen kimenő kapcsolat toohello Internet mindig szükség egy App Service Environment-környezet toofunction megfelelően. 

Az App Service-környezetek alhálózat van telepítve, mert a hálózati biztonsági csoportok lehet-e a használt toocontrol bejövő forgalom toohello alhálózat.  Hogyan toocontrol bejövő forgalom tooan App Service Environment-környezet a részletekért lásd: hello következő [cikk][controllinginboundtraffic].

További részletekért tooallow kimenő internetkapcsolattal az App Service-környezetek, tekintse meg a következő cikket: használata hello [Express Route][ExpressRoute].  hello hello cikkben ismertetett ugyanezt a megközelítést akkor érvényes, ha a pont-pont kapcsolatban működik, és a kényszerített bújtatást.

## <a name="outbound-network-addresses"></a>Kimenő hálózati címek
Az App Service-környezetek lehetővé teszi a kimenő hívásokat, amikor egy IP-cím hozzá rendelve mindig hello kimenő hívások.  hello adott IP-cím használt függ, hogy meghívott hello végpont hello virtuális hálózati topológia belül vagy kívül hello virtuális hálózati topológia található.

Ha a hívott hello végpont **kívül** hello virtuális hálózati topológiáról, akkor hello kimenő (más néven hello kimenő NAT-cím) használt címe hello nyilvános VIP az App Service Environment-környezet hello.  Ez a cím hello portál felhasználói felületének hello App Service Environment-környezet a Tulajdonságok panelen található.

![Kimenő IP-cím][OutboundIPAddress]

Ezt a címet is meg lehet határozni a ASEs, amelyek csak egy nyilvános VIP létrehozásával egy alkalmazást az App Service Environment-környezet hello, és majd végrehajtása egy *nslookup* a hello webalkalmazás URL-címe. hello eredő IP-cím mind hello nyilvános VIP, valamint hello App Service Environment-kimenő NAT-cím.

Ha meghívott hello végpont **belül** hello virtuális hálózati topológiáról, hello kimenő cím hello hívó alkalmazás lesz hello egyes számítási erőforrás hello alkalmazást futtató hello belső IP-címét.  Azonban nincs a virtuális hálózat belső IP-címek tooapps állandó leképezéseket.  Alkalmazások mozgathatók különböző számítási erőforrások között, és az elérhető számítási erőforrások az App Service-környezetek hello készlet megváltoztathatják tooscaling műveletek miatt.

Azonban mivel az App Service-környezetek mindig egy alhálózaton belül található, akkor garantáltan fog hello belső IP-cím egy alkalmazást futtató számítási erőforrás mindig essen hello hello alhálózat CIDR-tartomány.  Ennek eredményeképpen részletes ACL-ek vagy a hálózati biztonsági csoportok használata toosecure tooother végpontok hello virtuális hálózaton belül hozzáférni, hello App Service Environment-környezet igényeinek toobe hozzáférést tartalmazó alhálózat tartományának hello.

hello alábbi ábrán látható ezekről a fogalmakról részletesebben:

![Kimenő hálózati címek][OutboundNetworkAddresses]

A fenti ábrán hello:

* Mivel nyilvános VIP hello az App Service Environment-környezet hello 192.23.1.2, ez hello hívása túl esetén használt kimenő IP-cím "Internet" végpontok.
* az App Service Environment-környezet hello alhálózatot tartalmazó hello CIDR-tartomány hello 10.0.1.0/26.  Más végpontok hello belül azonos virtuális hálózati infrastruktúra jelenik meg, az alkalmazásokból hívások as a címtartományán belül valahol származó.

## <a name="calls-between-app-service-environments"></a>App Service-környezetek közötti hívások
Egy összetettebb forgatókönyv akkor fordulhat elő, ha több App Service Environment-környezetek hello azonos virtuális hálózatban, és egy App Service Environment-környezet tooanother App Service Environment-környezet a kimenő hívásokat.  Az ilyen típusú kereszt-App Service Environment-környezet is hívások az "Internet" hívások tekinteni.

a következő diagram hello egy példán látható alkalmazásokkal architektúra egy App Service Environment-környezet (pl. "Bejárati ajtón" webalkalmazások) hívása a következő alkalmazásokat egy másik App Service-környezet (pl. belső háttér-API-alkalmazások nem feltétlenül érhető el a hello Internet toobe). 

![App Service-környezetek közötti hívások][CallsBetweenAppServiceEnvironments] 

App Service Environment-környezet "ASE egy" hello a fenti példában hello 192.23.1.2 kimenő IP-cím tartozik.  Ha egy alkalmazás az App Service-környezet futó küld egy másik App Service-környezet ("ASE két") található, azonos virtuális hálózat, hello hello futó kimenő kimenő hívás tooan alkalmazás hívás kezeli a program az "Internet" hívásként.  Emiatt hello hálózati forgalom hello érkező második App Service Environment-környezet megjeleníti a 192.23.1.2 származó (azaz nem hello alhálózati címtartományt a hello első App Service Environment-környezet).

Annak ellenére, hogy másik App Service Environment-környezetek közötti hívások tekintendők "Internet" hívások, ha mindkét App Service Environment-környezetek hello azonos Azure-régiót hello hálózati forgalom hello regionális Azure-hálózatot a marad, és fizikailag nem halad találhatók keresztül hello a nyilvános internethez.  Ennek következtében a hálózati biztonsági csoport használhatja a hello hello alhálózaton második App Service Environment-környezet tooonly lehetővé teszi a bejövő hívások hello első App Service Environment-környezet (amelynek kimenő IP-cím 192.23.1.2), biztosítva ezzel hello közötti biztonságos kommunikációhoz App Service-környezetek.

## <a name="additional-links-and-information"></a>További hivatkozások és információk
Összes cikket, és hogyan-a következőre az App Service Environment-környezetek érhetők el hello [alkalmazásszolgáltatási környezetek – fontos fájl](../app-service/app-service-app-service-environments-readme.md).

A részletek bejövő App Service Environment-környezetek által használt portok, és a hálózati biztonsági csoportok toocontrol használatával bejövő forgalom érhető el [Itt][controllinginboundtraffic].

Felhasználó által definiált útvonalak toogrant kimenő Internet access tooApp Service-környezetek használatával érhető el ezen [cikk][ExpressRoute]. 

<!-- LINKS -->
[virtualnetwork]: http://azure.microsoft.com/services/virtual-network/
[controllinginboundtraffic]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[ExpressRoute]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-configuration-expressroute/

<!-- IMAGES -->
[GeneralNetworkFlows]: ./media/app-service-app-service-environment-network-architecture-overview/NetworkOverview-1.png
[OutboundIPAddress]: ./media/app-service-app-service-environment-network-architecture-overview/OutboundIPAddress-1.png
[OutboundNetworkAddresses]: ./media/app-service-app-service-environment-network-architecture-overview/OutboundNetworkAddresses-1.png
[CallsBetweenAppServiceEnvironments]: ./media/app-service-app-service-environment-network-architecture-overview/CallsBetweenEnvironments-1.png

