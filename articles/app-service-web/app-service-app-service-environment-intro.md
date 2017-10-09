---
title: "aaaIntroduction tooApp Service-környezet 1-es verzió"
description: "Tudnivalók az App Service Environment-környezet v1 hello szolgáltatás, hogy a biztonságos, virtuális hálózatot csatlakoztatott, dedikált méretezési egységek az összes az alkalmazások futtatásához."
services: app-service
documentationcenter: 
author: stefsch
manager: erikre
editor: 
ms.assetid: 78e6d4f5-da46-4eb5-a632-b5fdc17d2394
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: ccompy
ms.openlocfilehash: 6e3cd1909b241887b5ec19412b9f7884d870cc3d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooapp-service-environment-v1"></a>Bevezetés tooApp Service-környezet 1-es verzió

> [!NOTE]
> Ez a cikk hello App Service Environment-környezet v1 tárgya.  App Service-környezet, amely könnyebben toouse, és nagyobb teljesítményű infrastruktúra futó hello újabb verziója van telepítve. több hello új verzióval kapcsolatos kezdődnie hello toolearn [bemutatása toohello App Service Environment-környezet](../app-service/app-service-environment/intro.md).
> 

## <a name="overview"></a>Áttekintés
Az App Service Environment-környezet egy [prémium] [ PremiumTier] terv lehetőségét, amely egy teljesen elkülönített és dedikált környezetben történő biztonságos futtatására, nagy méretekben Azure App Service-alkalmazásokhoz Azure App Service szolgáltatás beleértve [Web Apps][WebApps], [Mobile Apps][MobileApps], és [API-alkalmazások][APIApps].  

App Service-környezetek ideálisak igénylő alkalmazások és szolgáltatások:

* Nagyon nagy méretű
* Elkülönítés és a biztonságos hálózati hozzáférést

Az ügyfelek több App Service Environment-környezetek belül egyetlen Azure-régió, valamint több Azure-régiók közötti hozhatnak létre.  Így az App Service Environment-környezetek magas RPS munkaterhelések támogatásához állapot nélküli alkalmazások szintek vízszintesen méretezéshez ideális.

App Service-környezetek elkülönített toorunning csak egyetlen vevő alkalmazások, és mindig telepített virtuális hálózatba.  Ügyfelek mindkét bejövő és kimenő hálózati forgalom részletes szabályozhatják, és alkalmazások képes kapcsolatot létrehozni a nagy sebességű biztonságos virtuális hálózatok tooon helyszíni vállalati erőforrások keresztül.

Összes cikket, és hogyan-a következőre vonatkozó App Service Environment-környezetek érhetők el hello [alkalmazásszolgáltatási környezetek – fontos fájl](../app-service/app-service-app-service-environments-readme.md).

Megtudhatja, hogyan App Service Environment-környezetek engedélyezése a nagy méretű és a biztonságos hálózati hozzáférés, lásd: hello [AzureCon mélyreható] [ AzureConDeepDive] az App Service Environment-környezetek!

Egy részletesen a vízszintes méretezést több App Service Environment-környezetek cikke hello hogyan toosetup egy [földrajzilag elosztott alkalmazás erőforrásigényét][GeodistributedAppFootprint].

Hogyan hello biztonsági architektúrája látható hello AzureCon mélyreható lett konfigurálva, toosee cikke hello megvalósításáról a [biztonsági architektúrája réteges](app-service-app-service-environment-layered-security.md) az App Service Environment-környezetek.

App Service Environment-környezetek futó alkalmazásokra is hozzáférhet a saját felsőbb rétegbeli eszközök, például a webalkalmazási tűzfalak (WAF) által kezdeményezett.  hello foglalkozó [egy WAF konfigurálása az App Service Environment-környezetek](app-service-app-service-environment-web-application-firewall.md) ebben a forgatókönyvben ismerteti. 

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="dedicated-compute-resources"></a>Dedikált számítási erőforrásokat
Kizárólag tooa egyetlen előfizetés dedikált összes hello számítási erőforrások egy App Service Environment-környezetben, és az App Service-környezetek konfigurálható toofifty (50) számítási erőforrások kizárólagos használatra egy bizonyos alkalmazás.

Az App Service-környezetek előtér-számítási erőforráskészlet, valamint egy toothree feldolgozókészletek számítási erőforrás áll. 

hello előtér-készlet számítási erőforrásokat is automatikus terheléselosztást app kérelmek belül az App Service-környezetek, SSL-lezárást felelős tartalmaz. 

Minden egyes munkavégző készletét tartalmazza túl lefoglalt számítási erőforrásokat[App Service-csomagok][AppServicePlan], viszont tartalmazó egy vagy több Azure App Service-alkalmazásokhoz.  Lehet toothree különböző feldolgozókészletek fel egy App Service Environment-környezetben, mert rendelkezik hello rugalmasságot toochoose különböző számítási erőforrásokat az egyes munkavégző készletét.  

Például így toocreate egy feldolgozókészletek kevésbé hatékony számítási erőforrással az App Service-csomagok fejlesztési vagy tesztelési alkalmazásokhoz készült.  A második (vagy akár harmadik) feldolgozókészletek használhatja az App Service-csomagok futó termelési alkalmazások szánt nagyobb teljesítményű számítási erőforrásokat.

A számítási erőforrások elérhető toohello előtér- és feldolgozókészletek hello mennyisége további részletekért lásd: [hogyan tooConfigure az App Service-környezetek][HowToConfigureanAppServiceEnvironment].  

A rendelkezésre álló hello részleteinek számítási erőforrás mérete egy App Service Environment-környezet támogatja, olvassa el a hello [App Service szolgáltatás díjszabása] [ AppServicePricing] lapon, és tekintse át az App Service Environment-környezetek hello lehetőségek a hello prémium tarifacsomag.

## <a name="virtual-network-support"></a>Virtuális hálózati támogatása
Az App Service-környezetek hozhatók létre **vagy** az Azure Resource Manager virtuális hálózati **vagy** klasszikus telepítési modell virtuális hálózat ([további információ a virtuális hálózatok][MoreInfoOnVirtualNetworks]).  Mivel az App Service-környezetek mindig szerepel egy virtuális hálózatot, és pontosan egy virtuális hálózati alhálózat, belül kihasználható hello biztonsági a virtuális hálózatok toocontrol mindkét bejövő és kimenő hálózati kommunikáció.  

Az App Service Environment-környezet vagy nyilvános IP-címmel, vagy a belső csak Azure belső Load Balancer (ILB) címmel rendelkező irányuló internetes lehet.

Használhat [hálózati biztonsági csoportok] [ NetworkSecurityGroups] toorestrict bejövő hálózati kommunikációs toohello alhálózat egy App Service Environment-környezet tároló.  Ez lehetővé teszi toorun alkalmazások mögött fölérendelt eszközöket és szolgáltatásokat, például a webalkalmazási tűzfalak és a hálózati Szolgáltatottszoftver-szolgáltatók.

Alkalmazások is gyakran kell tooaccess vállalati erőforrások, például belső adatbázisok és a web services.  Általános gyakorlatként javasolt toomake ezek végpontok elérhető csak toointernal hálózati forgalom továbbítására egy Azure virtuális hálózaton belül van.  Amikor egy App Service Environment-környezet illesztett toohello hello belső szolgáltatások, hello környezetben futó alkalmazások megegyező virtuális hálózatban érhetik el azokat, beleértve a végpontok segítségével elérhető [pont-pont] [ SiteToSite]és [Azure ExpressRoute] [ ExpressRoute] kapcsolatok.

Az App Service Environment-környezetek virtuális hálózatok és a helyszíni hálózatokban működéséről további részleteket tekintse meg a következő cikkeket hello [hálózati architektúra][NetworkArchitectureOverview], [bejövő vezérlése Forgalom][ControllingInboundTraffic], és [biztonságos kapcsolódás tooBackends][SecurelyConnectingToBackends]. 

## <a name="getting-started"></a>Bevezetés
Lásd az App Service-környezetek lépései tooget [hogyan tooCreate egy App Service Environment-környezet][HowToCreateAnAppServiceEnvironment]

Összes cikket, és hogyan-a következőre az App Service Environment-környezetek érhetők el hello [alkalmazásszolgáltatási környezetek – fontos fájl](../app-service/app-service-app-service-environments-readme.md).

Hello Azure App Service platformmal kapcsolatos további információkért lásd: [Azure App Service][AzureAppService].

App Service Environment-környezet hálózati architektúra hello áttekintését lásd: hello [hálózati architektúra áttekintése] [ NetworkArchitectureOverview] cikk.

További információ az App Service-környezetek használatáról az expressroute-nál: következő cikket hello [Expressroute és az App Service Environment-környezetek][NetworkConfigDetailsForExpressRoute].

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[PremiumTier]: http://azure.microsoft.com/pricing/details/app-service/
[MoreInfoOnVirtualNetworks]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[AppServicePlan]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[HowToCreateAnAppServiceEnvironment]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[WebApps]: http://azure.microsoft.com/documentation/articles/app-service-web-overview/
[MobileApps]: http://azure.microsoft.com/documentation/articles/app-service-mobile-value-prop-preview/
[APIApps]: http://azure.microsoft.com/documentation/articles/app-service-api-apps-why-best-platform/
[LogicApps]: http://azure.microsoft.com/documentation/articles/app-service-logic-what-are-logic-apps/
[AzureConDeepDive]:  https://azure.microsoft.com/documentation/videos/azurecon-2015-deploying-highly-scalable-and-secure-web-and-mobile-apps/
[GeodistributedAppFootprint]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-geo-distributed-scale/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[SiteToSite]: https://azure.microsoft.com/documentation/articles/vpn-gateway-site-to-site-create/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[HowToConfigureanAppServiceEnvironment]:  http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/
[ControllingInboundTraffic]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[SecurelyConnectingToBackends]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-securely-connecting-to-backend-resources/
[NetworkArchitectureOverview]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-architecture-overview/
[NetworkConfigDetailsForExpressRoute]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-configuration-expressroute/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/ 

<!-- IMAGES -->


