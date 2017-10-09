---
title: "App Service Environment-környezet információs aaaAzure"
description: "Listák hello dokumentáció, és ismerteti az Azure App Service Environment-környezet"
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: 77452413-5193-4762-8b3d-5fa8e4edf1ca
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: ccompy
ms.openlocfilehash: 6edc74804ded7497e70c31c9e08252257add4415
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="app-service-environment-documentation"></a>App Service-környezet dokumentáció
 Az Azure App Service Environment-környezet az Azure App Service szolgáltatása, amely egy teljesen elkülönített és dedikált környezetben történő biztonságos futtatására, nagy méretekben App Service-alkalmazásokhoz. Ezzel a funkcióval rendelkezhet az [webalkalmazások][webapps], [mobilalkalmazások][mobileapps], [API-alkalmazások][APIApps], és [funkciók][Functions].

App Service-környezetek (ASEs) ideálisak igénylő alkalmazások és szolgáltatások:

* Nagyon nagy méretű.
* Elkülönítés és a biztonságos hálózati hozzáférést.

Az ügyfelek hozhatnak létre több ASEs egyetlen Azure régión belül és több Azure-régiók között. Ez sokoldalúsága teszi ASEs ideális vízszintesen skálázás állapot nélküli alkalmazások rétegek magas RPS munkaterhelések támogatásához.

ASEs elkülönített toorunning csak egyetlen vevő alkalmazások, és mindig egy Azure virtuális hálózatra telepített. Az ügyfelek szabályozhassák részletes bejövő és kimenő hálózati forgalmat a [hálózati biztonsági csoportok][NSGs]. Alkalmazások is is nagy sebességű biztonságos kapcsolat létrehozása a virtuális hálózatok tooon helyszíni vállalati erőforrások keresztül.

Alkalmazások gyakran kell tooaccess vállalati erőforrások, például belső adatbázisok és webszolgáltatásokat. ASEs a futó alkalmazások férjenek hozzá erőforrásokhoz keresztül [pont-pont] [ SiteToSite] VPN és [Azure ExpressRoute] [ ExpressRoute] kapcsolatok.

* [Mi az az App Service-környezet?][Intro]
* [App Service-környezet létrehozása][MakeExternalASE]
* [A belső terheléselosztók App Service Environment-környezet létrehozása][MakeILBASE]
* [App Service-környezet használata][UsingASE]
* [Hálózati szempontok és a hello App Service Environment-környezet][ASENetwork]
* [App Service-környezet létrehozása sablonból][MakeASEfromTemplate]


## <a name="videos"></a>Videók
Fő Modern PaaS a hello vállalati az Azure App Service
>[!VIDEO https://channel9.msdn.com/Events/Ignite/2016/BRK3205/player]

Hatékonyan skálázható és biztonságos alkalmazások üzembe helyezése
>[!VIDEO https://channel9.msdn.com/Events/Microsoft-Azure/AzureCon-2015/ACON325/player]

Nagyvállalati web- és mobilalkalmazások futtatása az Azure App Service-ben
>[!VIDEO https://channel9.msdn.com/Events/Ignite/2015/BRK3715/player]

## <a name="app-service-environment-v1"></a>App Service-környezet v1 ##
App Service Environment-környezet két verziója van: ASEv1 és ASEv2. A ASEv1 információkért lásd: [App Service Environment-környezet v1 dokumentáció][ASEv1README].


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
[PremiumTier]: http://azure.microsoft.com/pricing/details/app-service/
[ASEv1README]: ../app-service-app-service-environments-readme.md
[SiteToSite]: ../../vpn-gateway/vpn-gateway-site-to-site-create.md
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
