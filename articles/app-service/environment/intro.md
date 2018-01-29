---
title: "Az Azure App Service Environment bemutatása"
description: "Az Azure App Service Environment rövid áttekintése"
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: 3c7eaefa-1850-4643-8540-428e8982b7cb
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 06/13/2017
ms.author: ccompy
ms.custom: mvc
ms.openlocfilehash: 803a1cde5387b549504b42346d1a2e6a5df04746
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 12/04/2017
---
# <a name="introduction-to-app-service-environments"></a>Az App Service Environment bemutatása #
 
## <a name="overview"></a>Áttekintés ##

Az Azure App Service Environment egy Azure App Service-funkció, amely teljesen elkülönített és dedikált környezetben futtatható az App Service-alkalmazások biztonságos, nagy léptékű futtatása érdekében. Ennek köszönhetően a szolgáltatás képes a webalkalmazások, [mobilalkalmazások][mobileapps], API-alkalmazások és [függvények][Functions] üzemeltetésére.

Az App Service Environment (ASE) a következő igényekkel rendelkező összes alkalmazási számítási feladat elvégzésére használható:

- Nagyon nagy skálázhatóság.
- Elkülönítés és biztonságos hálózati hozzáférés.
- Magas memóriakihasználtság.

Egy vagy több Azure-régión belül több ASE létrehozásának lehetősége az ügyfelek számára. Az ASE környezetek ennek a rugalmasságnak köszönhetik, hogy ideálisak az állapot nélküli, magas RPS-terhelésű alkalmazásszintek horizontális felskálázásához.

Elkülönítettségük révén az ASE-k környezetek egyetlen ügyfél alkalmazásait futtatják, és üzembe helyezésük mindig egy virtuális hálózaton belül történik. Az ügyfelek teljes mértékben szabályozhatják az alkalmazás bejövő és kimenő hálózati adatforgalmát. Az alkalmazások nagy sebességű, biztonságos VPN-kapcsolatokat létesíthetnek a helyszíni vállalati erőforrásokkal.

* Az ASE környezetek lehetővé teszik a biztonságos hálózati hozzáférésű, magas skálázhatóságú alkalmazásüzemeltetést. További információt az [AzureCon Deep Dive](https://azure.microsoft.com/documentation/videos/azurecon-2015-deploying-highly-scalable-and-secure-web-and-mobile-apps/) ASE környezetekről szóló részében talál.
* Több ASE is felhasználható a horizontális skálázásra. További információkért lásd a [földrajzilag elosztott alkalmazás beállítását](app-service-app-service-environment-geo-distributed-scale.md) ismertető részt.
* Az ASE környezetek használatával a biztonsági architektúra is konfigurálható, ahogyan azt az AzureCon Deep Dive is bemutatja. Az AzureCon Deep Dive-ban látható biztonsági architektúra konfigurálásáról a [rétegelt biztonsági architektúra App Service Environmenttel történő megvalósításáról szóló cikkben](app-service-app-service-environment-layered-security.md) találhat további információkat.
* Az ASE környezetekben futó alkalmazások hozzáférésében sorompós kapcsolatok alakíthatók ki alsóbb rétegbeli eszközök, például webalkalmazás-tűzfalak (WAF-ok) segítségével. További információt a [WAF App Service Environmenthez történő konfigurálását](app-service-app-service-environment-web-application-firewall.md) ismertető cikkben talál.

## <a name="dedicated-environment"></a>Dedikált környezet ##

Az ASE kizárólag egyetlen előfizetéshez van dedikálva, és akár 100 példány üzemeltetésére is képes. A tartomány akár 100 példányt is magában foglalhat egyetlen App Service-csomag keretében, vagy 100 egypéldányos App Service-csomagot, illetve ezek között bármennyit.

Egy ASE előtérrendszerekből és feldolgozókból áll. Az előtérrendszerek a HTTP/HTTPS-végződtetésért, valamint az alkalmazáskérések egy ASE-n belüli automatikus terheléselosztásáért felelősek. Az előtérrendszerek az ASE App Service-csomagjainak felskálázásakor automatikusan hozzáadódnak.

A feldolgozók az ügyfélalkalmazások üzemeltetéséért felelős szerepkörök. A feldolgozók három állandó méretben érhetők el:

* Egy vCPU/3,5 GB RAM
* Két vCPU/7 GB RAM
* Négy vCPU/14 GB RAM

Az ügyfeleknek nem kell foglalkozniuk az előtérrendszerek és a feldolgozók kezelésével. Amikor az ügyfelek horizontálisan felskálázzák az App Service-csomagjaikat, az infrastruktúra automatikusan hozzáadódik. Mivel az App Service-csomagok egy ASE-n belül vannak létrehozva vagy skálázva, ezért a szükséges infrastruktúra a helyzetnek megfelelően adható hozzá vagy távolítható el.

Az ASE átalányalapú havidíja fedezi az infrastruktúra költségét, és nem változik az ASE méretével. A további díjakat az App Service-csomagok vCPU-inak száma határozza meg. Egy ASE környezeten belül az összes üzemeltetett alkalmazás az elkülönített díjszabású termékváltozatba tartozik. Az ASE környezetek árképzéséről az [App Service díjszabása][Pricing] oldalon, az ASE környezetek elérhető díjszabási lehetőségei alatt találhat információkat.

## <a name="virtual-network-support"></a>Virtuális hálózatok támogatása ##

ASE környezetek kizárólag az Azure Resource Manager virtuális hálózatán hozhatók létre. Az Azure virtuális hálózatairól további információt az [Azure virtuális hálózatok GYIK](https://azure.microsoft.com/documentation/articles/virtual-networks-faq/) dokumentumában talál. Az ASE mindig egy virtuális hálózaton belül, pontosabban a virtuális hálózat alhálózatán működik. A virtuális hálózatok biztonsági funkciói segítségével szabályozhatja az alkalmazásai bejövő és kimenő hálózati kommunikációját.

Az ASE lehet internetre irányuló, nyilvános IP-címmel, vagy befelé irányuló, Azure belső terheléselosztási (ILB) címmel.

A [hálózati biztonsági csoportok][NSGs] korlátozzák az ASE alhálózatára beérkező hálózati kommunikációt. A hálózati biztonsági csoportok használatával futtathatja az alkalmazásait alsóbb rétegbeli eszközök és szolgáltatások, valamint WAF-ok és hálózati SaaS-szolgáltatók mögött.

Az alkalmazásoknak gyakran kell hozzáférniük vállalati erőforrásokhoz, például belső adatbázisokhoz vagy webes szolgáltatásokhoz. Ha olyan virtuális hálózatban telepíti az ASE környezetet, amely VPN-kapcsolatban van a helyszíni hálózattal, akkor az ASE környezeten belüli alkalmazások hozzáférhetnek a helyszíni erőforrásokhoz. Ez mind a [helyek közötti](https://azure.microsoft.com/documentation/articles/vpn-gateway-site-to-site-create/), mind az [Azure ExpressRoute](http://azure.microsoft.com/services/expressroute/) VPN-kapcsolatok esetében igaz.

Az ASE-k virtuális és helyszíni hálózatokkal történő használatáról az [App Service Environment hálózati szempontjai][ASENetwork] részben találhat további információkat.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-Application-Service-Environments-v2-Private-PaaS-Environments-in-the-Cloud/player]

## <a name="app-service-environment-v1"></a>App Service-környezet v1 ##

Kétféle verzió érhető el az App Service Environment szolgáltatáshoz: ASEv1 és ASEv2. A fenti információ az ASEv2 verzión alapul. Ebben a szakaszban az ASEv1 és ASEv2 különbségeiről olvashat. 

Az ASEv1 esetén minden erőforrást manuálisan kell felügyelnie. Ebbe beletartoznak az előtérrendszerek, a feldolgozók, valamint IP-alapú SSL esetén az IP-címek is. Mielőtt horizontálisan felskálázza az App Service-csomagot, először a feldolgozók készletét kell felskáláznia oda, ahol üzemeltetni szeretné azt.

Az ASEv1 díjszabása eltér az ASEv2-étől. Az ASEv1 esetében minden lefoglalt vCPU után fizet. Ez tartalmazza azokat az előtérrendszerekhez és a feldolgozókhoz tartozó vCPU-kat, amelyek nem üzemeltetnek számítási feladatokat. Az ASEv1 esetében egy ASE alapértelmezett maximális skálázhatósága összesen 55 gazdagép. Ez tartalmazza a feldolgozókat és az előtérrendszereket is. Az ASEv1 egyik előnye az, hogy klasszikus, illetve Resource Manager virtuális hálózatokban is üzembe helyezhető. Az ASEv1 verzióról további információt az [App Service Environment v1 bemutatása][ASEv1Intro] témakörben találhat.

<!--Links-->
[Intro]: ./intro.md
[MakeExternalASE]: ./create-external-ase.md
[MakeASEfromTemplate]: ./create-from-template.md
[MakeILBASE]: ./create-ilb-ase.md
[ASENetwork]: ./network-info.md
[UsingASE]: ./using-an-ase.md
[UDRs]: ../../virtual-network/virtual-networks-udr-overview.md
[NSGs]: ../../virtual-network/virtual-networks-nsg.md
[ConfigureASEv1]: app-service-web-configure-an-app-service-environment.md
[ASEv1Intro]: app-service-app-service-environment-intro.md
[webapps]: ../app-service-web-overview.md
[mobileapps]: ../../app-service-mobile/app-service-mobile-value-prop.md
[Functions]: ../../azure-functions/index.yml
[Pricing]: http://azure.microsoft.com/pricing/details/app-service/
[ARMOverview]: ../../azure-resource-manager/resource-group-overview.md
[ConfigureSSL]: ../web-sites-purchase-ssl-web-site.md
[Kudu]: http://azure.microsoft.com/resources/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[ASEWAF]: app-service-app-service-environment-web-application-firewall.md
[AppGW]: ../../application-gateway/application-gateway-web-application-firewall-overview.md
