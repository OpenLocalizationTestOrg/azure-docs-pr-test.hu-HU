---
title: "App Service Environment-környezet |} Microsoft Docs"
description: "Mi az az Azure App Service-környezetek? App Service Environment bemutatása."
keywords: "az Azure app service Environment-környezet, virtuális hálózati biztonságos hálózatkezelés"
services: app-service
documentationcenter: 
author: stefsch
manager: erikre
editor: 
ms.assetid: 1db5c057-3c56-4537-b580-cdd21fe3f3a7
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/01/2016
ms.author: stefsch
ms.openlocfilehash: 1de3f2c513f4f5ce6fd2ab2b57514122955a9a98
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="app-service-environment-documentation"></a>Az App Service-környezet dokumentáció
Az App Service Environment-környezet egy [prémium] [ PremiumTier] terv lehetőségét, amely egy teljesen elkülönített és dedikált környezetben történő biztonságos futtatására, nagy méretekben Azure App Service-alkalmazásokhoz Azure App Service szolgáltatás beleértve [Web Apps][WebApps], [Mobile Apps][MobileApps], és [API-alkalmazások][APIApps].  

App Service-környezetek ideálisak igénylő alkalmazások és szolgáltatások:

* Nagyon nagy méretű
* Elkülönítés és a biztonságos hálózati hozzáférést

Az ügyfelek több App Service Environment-környezetek belül egyetlen Azure-régió, valamint több Azure-régiók közötti hozhatnak létre.  Így az App Service Environment-környezetek magas RPS munkaterhelések támogatásához állapot nélküli alkalmazások szintek vízszintesen méretezéshez ideális.

App Service-környezetek csak az egyetlen ügyfél-alkalmazások futtatása érdekében elkülönített, és mindig telepített virtuális hálózatba.  Az ügyfelek szabályozhassák részletes mindkét bejövő és kimenő hálózati forgalom használó [hálózati biztonsági csoportok][NetworkSecurityGroups].  Alkalmazások is is nagy sebességű biztonságos kapcsolat létrehozása a virtuális hálózatokon keresztül kapcsolódjanak a helyszíni vállalati erőforrásokhoz.

Alkalmazások gyakran kell hozzáférhet a vállalati erőforrásokhoz, például a belső adatbázisok és webszolgáltatásokat.  App Service Environment-környezetek futó alkalmazások hozzáférhetnek a keresztül elérhető erőforrásokhoz [pont-pont] [ SiteToSite] VPN és [Azure ExpressRoute] [ ExpressRoute] kapcsolatok.

* [Mi az az App Service Environment?](../app-service-web/app-service-app-service-environment-intro.md)
* [App Service Environment környezetek létrehozása](../app-service-web/app-service-web-how-to-create-an-app-service-environment.md)
* [Alkalmazások létrehozása App Service Environment környezetben](../app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase.md)
* [Létrehozásával és a belső Terheléselosztók használatával az App Service-környezetek](../app-service-web/app-service-environment-with-internal-load-balancer.md)
* [Az App Service Environment konfigurálása](../app-service-web/app-service-web-configure-an-app-service-environment.md) 
* [Alkalmazások skálázása App Service Environmentben](../app-service-web/app-service-web-scale-a-web-app-in-an-app-service-environment.md)
* [Hálózatbiztonság és -architektúra](../app-service-web/app-service-app-service-environment-network-architecture-overview.md)

## <a name="how-tos"></a>Hogyan meg
[!INCLUDE [app-service-blueprint-app-service-environment](../../includes/app-service-blueprint-app-service-environment.md)]

## <a name="videos"></a>Videók
>[!VIDEO https://channel9.msdn.com/Events/Ignite/2016/BRK3205/player]

>[!VIDEO https://channel9.msdn.com/Events/Microsoft-Azure/AzureCon-2015/ACON325/player]

>[!VIDEO https://channel9.msdn.com/Events/Ignite/2015/BRK3715/player]



<!-- LINKS -->
[PremiumTier]: http://azure.microsoft.com/pricing/details/app-service/
[WebApps]: http://azure.microsoft.com/documentation/articles/app-service-web-overview/
[MobileApps]: http://azure.microsoft.com/documentation/articles/app-service-mobile-value-prop-preview/
[APIApps]: http://azure.microsoft.com/documentation/articles/app-service-api-apps-why-best-platform/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[SiteToSite]: https://azure.microsoft.com/documentation/articles/vpn-gateway-site-to-site-create/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
