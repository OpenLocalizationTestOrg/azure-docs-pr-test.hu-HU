---
title: "aaaSecurely csatlakozás tooBackEnd erőforrásokhoz bárhonnan, az App Service-környezetek"
description: "További információk a hogyan toosecurely kapcsolódó toobackend erőforrások az App Service Environment-környezet."
services: app-service
documentationcenter: 
author: stefsch
manager: erikre
editor: 
ms.assetid: f82eb283-a6e7-4923-a00b-4b4ccf7c4b5b
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/04/2016
ms.author: stefsch
ms.openlocfilehash: 6311d3fc301512ea3c4ed8f14f268f75755aa415
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="securely-connecting-toobackend-resources-from-an-app-service-environment"></a>Biztonságosan csatlakozás tooBackend erőforrásokhoz bárhonnan, az App Service-környezetek
## <a name="overview"></a>Áttekintés
Mivel az App Service-környezetek mindig jön létre **vagy** az Azure Resource Manager virtuális hálózati **vagy** klasszikus telepítési modell [virtuális hálózati] [ virtualnetwork], az App Service Environment-környezet tooother háttér erőforrásokból kimenő kapcsolatok áramolhasson kizárólag hello virtuális hálózaton keresztül.  2016. június végzett legutóbbi módosítását ASEs is is telepíthető az nyilvános címtartományt, vagy RFC1918 címterek (azaz Magáncímeket) használó virtuális hálózatok.  

Előfordulhat például, a virtuális gépek zárolva 1433-as port a fürtön futó SQL Server.  lehet, hogy hello végpont tooonly egyéb erőforrásokhoz való hozzáférés engedélyezése a ACLd hello ugyanazt a virtuális hálózatot.  

Másik példaként, bizalmas végpontok a helyi, és akár keresztül csatlakoztatott tooAzure kell [pont-pont] [ SiteToSite] vagy [Azure ExpressRoute] [ ExpressRoute] kapcsolatok.  Ennek eredményeképpen csak azokat az erőforrásokat a virtuális hálózatok csatlakoztatva toohello pont-pont, vagy ExpressRoute-alagutak lesz képes tooaccess helyszíni végpontok.

Az összes forgatókönyvekben az App Service-környezetek futó alkalmazások kell képes toosecurely csatlakoztassa toohello különböző kiszolgálókon és erőforrásokat.  Egy App Service Environment-környezet tooprivate végpontok hello a futó alkalmazásokból kimenő forgalom ugyanazt a virtuális hálózatot (vagy toohello csatlakoztatva azonos virtuális hálózatban), csak a folyamat fog hello virtuális hálózaton keresztül.  Kimenő forgalom tooprivate végpontok nem halad keresztül hello a nyilvános internethez.

Egy szerint toooutbound forgalmat egy App Service Environment-környezet tooendpoints a virtuális hálózaton belül érvényes.  App Service Environment-környezetek nem érhető el a végpontok hello található virtuális gépek **azonos** alhálózat szerint hello App Service Environment-környezet.  Általában ne legyen hiba, amíg az App Service Environment-környezetek telepít egy alhálózatba csak hello App Service Environment-környezet által kizárólagos használatra fenntartva.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="outbound-connectivity-and-dns-requirements"></a>Kimenő kapcsolatok és a DNS-követelmények
Az egy App Service Environment-környezet toofunction megfelelően, akkor csak kimenő hozzáférést toovarious végpontok. Hello külső végpontok egy ASE által használt teljes listája megtalálható-e hello hello "A hálózati kapcsolat szükséges" szakasza [ExpressRoute tartozó fürthálózat-konfiguráció](app-service-app-service-environment-network-configuration-expressroute.md#required-network-connectivity) cikk.

App Service-környezetek hello virtuális hálózat egy érvényes DNS-infrastruktúra szükséges.  Ha bármilyen okból hello DNS-konfiguráció esetén megváltozik egy App Service Environment-környezet létrehozása után, a fejlesztők kényszerítheti az App Service Environment-környezet toopick hello új DNS-konfiguráció.  Hello "Restart" ikonnal hello App Service Environment-környezet hello tetején található működés közbeni környezet újraindítás kiváltó felügyelet panel hello portálon hatására hello környezet toopick hello új DNS-konfiguráció.

Ajánlott továbbá, hogy egyéni DNS-kiszolgálók hello a virtuális hálózaton kell előre idő előzetes toocreating az App Service-környezetek beállítása.  Ha egy virtuális hálózat DNS-konfiguráció megváltozott egy App Service Environment-környezet létrehozása közben, hello App Service Environment-környezet létrehozása folyamat sikertelen vezethet.  Egy hasonló tekintettel az egyéni DNS-kiszolgáló létezik a hello másik végén egy VPN gateway és hello DNS-kiszolgáló esetén nem érhető el vagy nem érhető el, hello App Service Environment-környezet létrehozása is sikertelen lesz.

## <a name="connecting-tooa-sql-server"></a>Kapcsolódás az SQL Server tooa
Egy közös SQL Server-konfigurációs az 1433-as portot figyelő végponttal rendelkezik:

![SQL Server-végpont][SqlServerEndpoint]

Kétféleképpen a forgalom toothis végpont korlátozása:

* [Hálózati hozzáférés-vezérlési listák] [ NetworkAccessControlLists] (hálózati hozzáférés-vezérlési listák)
* [Hálózati biztonsági csoportok][NetworkSecurityGroups]

## <a name="restricting-access-with-a-network-acl"></a>A hálózati hozzáférés-vezérlési lista a hozzáférés korlátozása
1433-as port a hálózati hozzáférés-vezérlési lista segítségével biztosítható.  whitelists ügyfél az alábbi hello példában címek egy virtuális hálózaton belül származó, és blokkolja hozzáférést tooall más ügyfelekkel.

![Hálózati hozzáférés vezérlő példa][NetworkAccessControlListExample]

A App Service Environment-környezetben futó alkalmazások hello ugyanazt a virtuális hálózatot, mert hello SQL Server lesz képes tooconnect toohello SQL Server-példány használatával hello **belső hálózatok** hello SQL Server virtuális gép IP-címet.  

hello példa kapcsolati karakterlánc hivatkozás alatti hello SQL Server Privát IP-címére.

    Server=tcp:10.0.1.6;Database=MyDatabase;User ID=MyUser;Password=PasswordHere;provider=System.Data.SqlClient

Bár a hello virtuális gépen van egy nyilvános végpontot, hello nyilvános IP-cím használatával történő kapcsolódási kísérletek elutasításra hello hálózati hozzáférés-vezérlési lista miatt. 

## <a name="restricting-access-with-a-network-security-group"></a>Hálózati biztonsági csoport a hozzáférés korlátozása
Hálózati biztonsági csoport van egy másik módjáról hozzáférés biztosítása érdekében.  Hálózati biztonsági csoportok alkalmazott tooindividual virtuális gépek vagy virtuális gépeket tartalmazó tooa alhálózati lehet.

Hálózati biztonsági csoport először létre toobe van szüksége:

    New-AzureNetworkSecurityGroup -Name "testNSGexample" -Location "South Central US" -Label "Example network security group for an app service environment"

Hozzáférés tooonly virtuális hálózat belső forgalom korlátozása a hálózati biztonsági csoport nagyon egyszerű.  hello alapértelmezett szabályok a hálózati biztonsági csoport csak engedélyezik a hozzáférést a többi hálózati ügyfelet hello ugyanazt a virtuális hálózatot.

Emiatt a hozzáférés tooSQL zárolása-kiszolgáló egy más dolga, mint az alapértelmezett szabályok tooeither hello futó virtuális gépek az SQL Server vagy hello virtuális gépeket tartalmazó hello alhálózatot a hálózati biztonsági csoport alkalmazása.

az alábbi hello minta hálózati biztonsági csoport toohello tartalmazó alhálózat vonatkozik:

    Get-AzureNetworkSecurityGroup -Name "testNSGExample" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-1'

hello végeredménynek olyan szabályokat, amelyek külső hozzáférés, miközben lehetővé teszi a virtuális hálózat belső hozzáférés letiltása:

![Alapértelmezett hálózati biztonsági szabályok][DefaultNetworkSecurityRules]

## <a name="getting-started"></a>Bevezetés
Összes cikket, és hogyan-a következőre az App Service Environment-környezetek érhetők el hello [alkalmazásszolgáltatási környezetek – fontos fájl](../app-service/app-service-app-service-environments-readme.md).

Lásd az App Service-környezetek lépései tooget [bemutatása tooApp Service-környezet][IntroToAppServiceEnvironment]

Ellenőrző bejövő forgalom tooyour App Service Environment-környezet körül, lásd: [szabályozása a bejövő forgalom tooan App Service Environment-környezet][ControlInboundASE]

Hello Azure App Service platformmal kapcsolatos további információkért lásd: [Azure App Service][AzureAppService].

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[ControlInboundTraffic]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[SiteToSite]: https://azure.microsoft.com/documentation/articles/vpn-gateway-site-to-site-create/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[NetworkAccessControlLists]: https://azure.microsoft.com/documentation/articles/virtual-networks-acl/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[IntroToAppServiceEnvironment]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/ 
[ControlInboundASE]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/ 

<!-- IMAGES -->
[SqlServerEndpoint]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/SqlServerEndpoint01.png
[NetworkAccessControlListExample]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/NetworkAcl01.png
[DefaultNetworkSecurityRules]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/DefaultNetworkSecurityRules01.png 
