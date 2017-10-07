---
title: "aaaIntroduction tooAzure App Service-környezetek"
description: "Azure App Service-környezetek rövid áttekintése"
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: 3c7eaefa-1850-4643-8540-428e8982b7cb
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: ccompy
ms.openlocfilehash: 9261041333cf59374974a039edf89c4983c45cdd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooapp-service-environments"></a>Bevezetés tooApp Service-környezetek #
 
## <a name="overview"></a>Áttekintés ##

Az Azure App Service Environment-környezet az Azure App Service szolgáltatása, amely egy teljesen elkülönített és dedikált környezetben történő biztonságos futtatására, nagy méretekben App Service-alkalmazásokhoz. Ezzel a funkcióval rendelkezhet az [webalkalmazások][webapps], [mobilalkalmazások][mobileapps], [API-alkalmazások][APIapps], és [funkciók][Functions].

App Service-környezetek (ASEs) megfelelőek igénylő alkalmazások és szolgáltatások:

- Nagyon nagy méretű.
- Elkülönítés és a biztonságos hálózati hozzáférést.
- Magas memóriahasználat.

Az ügyfelek több ASEs egyetlen Azure régión belül vagy között több Azure-régiók hozhatnak létre. A rugalmasságnak köszönhetően az ASEs ideális vízszintesen skálázás állapot nélküli alkalmazások rétegek magas RPS munkaterhelések támogatásához.

ASEs elkülönített toorunning csak egyetlen vevő alkalmazások, és mindig telepített virtuális hálózatba. Az ügyfelek befolyásolni részletes bejövő és kimenő hálózati forgalmat. Alkalmazások képes kapcsolatot létrehozni a nagy sebességű biztonságos VPN tooon helyszíni vállalati erőforrások keresztül.

Minden cikkeket, és hogyan-tooinstructions ASEs kapcsolatos találhatók hello [App Service-környezetek – fontos fájl][ASEReadme]:

* ASEs engedélyezése a nagy méretű alkalmazást üzemeltető biztonságos hálózati hozzáférést. További információkért lásd: hello [AzureCon mélyreható](https://azure.microsoft.com/documentation/videos/azurecon-2015-deploying-highly-scalable-and-secure-web-and-mobile-apps/) ASEs meg.
* Több ASEs vízszintes lehet használt tooscale. További információkért lásd: [hogyan tooset be egy földrajzilag elosztott alkalmazás erőforrásigényét](https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-geo-distributed-scale/).
* ASEs használt tooconfigure biztonsági architektúra, lehet, ahogy az hello AzureCon részletes bemutatója. toosee hogyan hello biztonsági architektúrája látható AzureCon mélyreható hello konfigurálása, lásd: hello [cikkünket tooimplement egy többrétegű biztonsági architektúra](https://docs.microsoft.com/en-us/azure/app-service-web/app-service-app-service-environment-layered-security) az App Service-környezetek.
* ASEs futó alkalmazásokra is hozzáférhet a saját felsőbb rétegbeli eszközök, például a webalkalmazási tűzfalak (WAFs) által kezdeményezett. További információkért lásd: [egy WAF az App Service-környezetek konfigurálása](https://docs.microsoft.com/en-us/azure/app-service-web/app-service-app-service-environment-web-application-firewall).

## <a name="dedicated-environment"></a>Dedikált környezetben ##

Egy ASE van kizárólag tooa egyetlen előfizetés dedikált és tárolhatja, 100 példányok. hello tartományon is kiterjedhet 100 példányok egy egyetlen az alkalmazásszolgáltatási csomag too100 egypéldányos App Service-csomagok, és mindent az között.

Egy ASE előtér-webkiszolgálóinak és alkalmazottak tevődik össze. Előtér-webkiszolgálóinak HTTP/HTTPS-lezárást és automatikus terheléselosztást app kérelmek belül egy ASE felelősek. Előtér-webkiszolgálóinak automatikusan hozzá lesznek adva hello hello ASE az App Service-csomagokról is kiterjeszthető.

Munkavállalók alkalmazások vevői üzemeltető szerepkörök. Három rögzített méretű munkavállalók érhetők el:

* Egy mag/3.5 GB RAM
* Két alapvető/7 GB RAM
* Négy alapvető/14 GB RAM

Az ügyfelek nem kell toomanage előtér-webkiszolgálóinak és alkalmazottak. Az összes infrastruktúra automatikusan nincs felvéve az ügyfelek kibővítési az App Service-csomagokról. Az alkalmazásszolgáltatási csomagok létrehozása, illetve -környezetben méretezhető, mint a hello szükséges infrastruktúra hozzáadni vagy eltávolítani, mert a megfelelő.

Nincs egy egyszerű havi arány egy ASE, amely hello infrastruktúra fizet, és hello ASE hello mérete nem változik. Emellett nincs egy App Service-csomag core onkénti költséget. Minden alkalmazás-környezetben üzemeltetett hello vannak elszigetelt árképzési Termékváltozat. Egy ASE az árakkal kapcsolatos információkért lásd: hello [App Service díjszabás] [ Pricing] lapon, és tekintse át az elérhető beállítások hello ASEs.

## <a name="virtual-network-support"></a>Virtuális hálózati támogatása ##

Egy ASE csak az Azure Resource Manager virtuális hálózat is létrehozható. További információ az Azure virtuális hálózataihoz, toolearn, lásd: hello [Azure virtuális hálózatok – gyakori kérdések](https://azure.microsoft.com/documentation/articles/virtual-networks-faq/). Egy ASE mindig létezik-e virtuális hálózatban, és pontosabban, a virtuális hálózat egy alhálózaton belül. Bejövő és kimenő virtuális hálózatok toocontrol hello biztonsági funkcióit is használhatja a hálózati kommunikációt az alkalmazások.

Egy ASE lehet internetre irányuló nyilvános IP-cím vagy a belső hálózati rendelkező csak egy Azure belső terheléselosztó (ILB)-címén.

[Hálózati biztonsági csoportok] [ NSGs] bejövő hálózati kommunikációs toohello alhálózat egy ASE tartalmazó korlátozása. Használhatja az NSG-k toorun alkalmazások mögött fölérendelt eszközöket és szolgáltatásokat, például WAFs és hálózati Szolgáltatottszoftver-szolgáltatók.

Alkalmazások is gyakran kell tooaccess vállalati erőforrások, például belső adatbázisok és a web services. Ha egy virtuális hálózatban egy VPN-kapcsolat toohello a helyszíni hálózat hello ASE telepít, a hello ASE hello alkalmazások hello helyszíni erőforrásait is elérik. Ez a funkció értéke true, függetlenül attól, hogy van-e hello VPN egy [pont-pont](https://azure.microsoft.com/documentation/articles/vpn-gateway-site-to-site-create/) vagy [Azure ExpressRoute](http://azure.microsoft.com/services/expressroute/) VPN.

A virtuális hálózatok és a helyszíni hálózatokban ASEs működése további információkért lásd: [App Service Environment-környezet hálózati szempontok][ASENetwork].

## <a name="app-service-environment-v1"></a>App Service-környezet v1 ##

App Service Environment-környezet két verziója van: ASEv1 és ASEv2. információk megelőző hello ASEv2 alapján. Ez a szakasz jeleníti meg, akkor hello ASEv1 és ASEv2 közötti különbséget. 

ASEv1, toomanage kell manuálisan hello összes erőforrást. Amely tartalmazza a hello előtér-webkiszolgálóinak dolgozó munkatársak és az IP-alapú SSL-hez használt IP-címek. Mielőtt ki lehet terjeszteni az App Service-csomag, meg kell-e toofirst horizontális felskálázás toohost, ahová hello munkavégző készletét.

ASEv1 ASEv2 a különböző árképzési modellt használ. ASEv1 a minden lefoglalt core fizetnie. Az előtér-webkiszolgálóinak vagy bármilyen számítási feladatot nem futtató munkavállalók használt mag, amely tartalmazza. A ASEv1 hello alapértelmezett maximális méretű egy ASE mérete 55 összes állomás. Amely tartalmazza a dolgozók és első akkor ér véget. Egy előny tooASEv1, hogy központilag telepíthető a klasszikus virtuális hálózatot és egy erőforrás-kezelő virtuális hálózatot. toolearn ASEv1, kapcsolatos további információkért lásd: [App Service Environment-környezet v1 bemutatása][ASEv1Intro].

<!--Links-->
[Intro]: ./intro.md
[MakeExternalASE]: ./create-external-ase.md
[MakeASEfromTemplate]: ./create-from-template.md
[MakeILBASE]: ./create-ilb-ase.md
[ASENetwork]: ./network-info.md
[ASEReadme]: ./readme.md
[UsingASE]: ./using-an-ase.md
[UDRs]: ../../virtual-network/virtual-networks-udr-overview.md
[NSGs]: ../../virtual-network/virtual-networks-nsg.md
[ConfigureASEv1]: ../../app-service-web/app-service-web-configure-an-app-service-environment.md
[ASEv1Intro]: ../../app-service-web/app-service-app-service-environment-intro.md
[webapps]: ../../app-service-web/app-service-web-overview.md
[mobileapps]: ../../app-service-mobile/app-service-mobile-value-prop.md
[APIapps]: ../../app-service-api/app-service-api-apps-why-best-platform.md
[Functions]: ../../azure-functions/index.yml
[Pricing]: http://azure.microsoft.com/pricing/details/app-service/
[ARMOverview]: ../../azure-resource-manager/resource-group-overview.md
[ConfigureSSL]: ../../app-service-web/web-sites-purchase-ssl-web-site.md
[Kudu]: http://azure.microsoft.com/resources/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[AppDeploy]: ../../app-service-web/web-sites-deploy.md
[ASEWAF]: ../../app-service-web/app-service-app-service-environment-web-application-firewall.md
[AppGW]: ../../application-gateway/application-gateway-web-application-firewall-overview.md
