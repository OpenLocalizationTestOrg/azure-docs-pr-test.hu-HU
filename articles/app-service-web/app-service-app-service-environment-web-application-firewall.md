---
title: "a webes alkalmazás tűzfalat (WAF) App Service Environment-környezet aaaConfiguring"
description: "Ismerje meg, hogyan tűzfal-tooconfigure egy webalkalmazást az App Service-környezet elé."
services: app-service\web
documentationcenter: 
author: naziml
manager: erikre
editor: jimbe
ms.assetid: a2101291-83ba-4169-98a2-2c0ed9a65e8d
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2016
ms.author: naziml
ms.openlocfilehash: 0fcf62aea871751c9d4f294d2d24df2186fc0e7e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-a-web-application-firewall-waf-for-app-service-environment"></a>Webalkalmazási tűzfal (WAF) App Service Environment-környezet konfigurálása
## <a name="overview"></a>Áttekintés
Webalkalmazás-hello például tűzfalak [Barracuda WAF az Azure-](https://www.barracuda.com/programs/azure) elérhető a hello [Azure piactér](https://azure.microsoft.com/marketplace/partners/barracudanetworks/waf-byol/) segítséget nyújt a webes alkalmazások biztonságos bejövő webes forgalom tooblock SQL vizsgálatával alkalommal, idegen, kártevő szoftverek feltöltések & DDoS alkalmazás és más támadásoknak. Azt is ellenőrzi hello háttér-webkiszolgálók hello válaszainak az adatok adatvesztés-megelőzési (DLP). Hello elkülönítési és az App Service Environment-környezetek által biztosított további skálázás együtt, a környezetet biztosít az ideális toohost üzleti kritikus webalkalmazások toowithstand kártékony kérelmek és nagy mennyiségű forgalom van szükség.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="setup"></a>Beállítás
A dokumentum azt konfigurálja az App Service-környezet több terhelés mögött, hogy csak hello WAF érkező forgalom hello App Service Environment-környezet képes elérni és nem lesz elérhető hello DMZ kiegyensúlyozott Barracuda WAF példányai. Azt is Azure Traffic Manager elé a Barracuda WAF példányok tooload egyenleg Azure-adatközpont és régiók között. Magas szintű diagramját, illetve hello beállítása néz alább láthatók.

![Architektúra][Architecture] 

> Megjegyzés: A hello bevezetése [ILB támogatja az App Service Environment-környezet](app-service-environment-with-internal-load-balancer.md), nem érhető el a hello DMZ hello ASE toobe konfigurálhatja, és csak akkor érhető el toohello magánhálózati. 
> 
> 

## <a name="configuring-your-app-service-environment"></a>Az App Service-környezet konfigurálása
az App Service-környezetek tooconfigure tekintse meg a túl[dokumentációban](app-service-web-how-to-create-an-app-service-environment.md) hello témáról. Miután egy App Service-környezet létrehozása, létrehozhat [webalkalmazások](app-service-web-overview.md), [API-alkalmazások](../app-service-api/app-service-api-apps-why-best-platform.md) és [Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md) ebben a környezetben, amely az összes védi mögött hello WAF azt a következő szakaszban hello konfigurálása.

## <a name="configuring-your-barracuda-waf-cloud-service"></a>A Barracuda WAF Felhőszolgáltatás konfigurálása
Barracuda rendelkezik egy [részletes cikk](https://campus.barracuda.com/product/webapplicationfirewall/article/WAF/DeployWAFInAzure) üzembe helyezni a WAF az Azure virtuális gépen. De azt szeretné, hogy redundancia, és a hibaérzékeny pontok kialakulását vezet be, mert szükség van-e legalább 2 toodeploy WAF példányának virtuális gépek hello azonos felhőalapú szolgáltatás, ha az alábbi utasításokat.

### <a name="adding-endpoints-toocloud-service"></a>Végpontok tooCloud szolgáltatás hozzáadása
Ha szükség van a 2 vagy több WAF Virtuálisgép-példányok a felhőszolgáltatásban hello azonnal használható [Azure-portálon](https://portal.azure.com/) tooadd HTTP és HTTPS végpontok az alábbi hello ábrán látható módon az alkalmazás által használt.

![Konfigurálja a végpontot][ConfigureEndpoint]

Az alkalmazások más végpontok használatát, győződjön meg arról, hogy tooadd e toothis listáját, valamint. 

### <a name="configuring-barracuda-waf-through-its-management-portal"></a>A felügyeleti portálon keresztül WAF Barracuda konfigurálása
Barracuda WAF TCP Port 8000 használja a konfiguráció a felügyeleti portálon keresztül. Mivel hello WAF virtuális gépek több példánya van szüksége lesz toorepeat hello itt lépéseket minden egyes Virtuálisgép-példány. 

> Megjegyzés: Ha elkészült, WAF konfigurációjával, távolítsa el hello TCP/8000 végpont a WAF virtuális gépek tookeep a biztonságos WAF.
> 
> 

Hello felügyeleti végpont hello ábrának megfelelően tooconfigure alatt a Barracuda WAF hozzáadása.

![Felügyeleti végpont hozzáadása][AddManagementEndpoint]

A böngésző toobrowse toohello felügyeleti végpont használatát a felhőalapú szolgáltatás. Ha a Felhőszolgáltatás neve test.cloudapp.net, ehhez a végponthoz toohttp://test.cloudapp.net:8000 tallózással elérésére. A bejelentkezési oldal megjelenik, például alatt is bejelentkezési hello WAF virtuális gép telepítési fázis a megadott hitelesítő adataival.

![Management bejelentkezési oldal][ManagementLoginPage]

Többször bejelentkezési, megjelenik egy irányítópultot, hello egy hello kép, amely alatt a jelent hello WAF védelmi alapvető statisztikája.

![Kezelési irányítópult][ManagementDashboard]

Hello szolgáltatások fülre kattintva így állíthatja be az általa védett szolgáltatásokhoz WAF. A Barracuda WAF konfigurálásával kapcsolatos további részletekért tanulmányozza [dokumentum](https://techlib.barracuda.com/waf/getstarted1). Hello példában alatt az Azure Web Apps HTTP és HTTPS-forgalmat szolgáltató nincs beállítva.

![Felügyeleti szolgáltatások hozzáadása][ManagementAddServices]

> Megjegyzés: Attól függően, hogy az alkalmazások hogyan vannak konfigurálva, és milyen funkciók használnak az App Service-környezet, szüksége lesz tooforward forgalom 80-as és 443-as TCP-portok például ha egy webalkalmazás IP SSL-beállítása. Az App Service Environment-környezetek használt hálózati portok listája, tekintse meg túl[vezérlő bejövő forgalom dokumentáció](app-service-app-service-environment-control-inbound-traffic.md) hálózati portok szakasz.
> 
> 

## <a name="configuring-microsoft-azure-traffic-manager-optional"></a>Microsoft Azure Traffic Managerben (nem kötelező)
Ha az alkalmazás érhető el több régióba, akkor érdemes választani tooload egyenleg őket mögött [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md). úgy adhat hozzá a végpont hello toodo [a klasszikus Azure portálon](https://manage.azure.com) hello felhőalapú szolgáltatás nevét a WAF használatát hello Traffic Manager-profilt, az alábbi hello ábrán látható módon. 

![Traffic Manager-végpontot][TrafficManagerEndpoint]

Ha az alkalmazás hitelesítést igényel, ellenőrizze, hogy néhány erőforrás, amely nem igényel a hitelesítés a Traffic Manager tooping hello rendelkezésre álláshoz az alkalmazás. Beállíthatja a konfigurálási csoportban hello hello URL hello [a klasszikus Azure portálon](https://manage.azure.com) alább látható módon.

![A Traffic Manager konfigurálása][ConfigureTrafficManager]

tooforward hello Traffic Manager ping-üzenetek az WAF tooyour alkalmazásról, kell toosetup webhely fordítások az Barracuda WAF tooforward forgalom tooyour alkalmazásra az alábbi hello példában látható módon.

![Webhely fordítások][WebsiteTranslations]

## <a name="securing-traffic-tooapp-service-environment-using-network-security-groups-nsg"></a>Forgalom tooApp szolgáltatási környezet használata hálózati biztonsági csoportok (NSG) védelmének biztosítása
Hajtsa végre a hello [vezérlő bejövő forgalom dokumentáció](app-service-app-service-environment-control-inbound-traffic.md) vonatkozó részletek korlátozása tooyour App Service Environment-környezet hello WAF a forgalmat, csak a felhőalapú szolgáltatás hello virtuális IP-cím használatával. Íme egy minta Powershell-parancsot a 80-as TCP-port a feladat végrehajtásához.

    Get-AzureNetworkSecurityGroup -Name "RestrictWestUSAppAccess" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP Barracuda" -Type Inbound -Priority 201 -Action Allow -SourceAddressPrefix '191.0.0.1'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP

Cserélje le hello SourceAddressPrefix hello Virtual IP Address (VIP) a WAF felhőalapú szolgáltatás.

> Megjegyzés: a felhőszolgáltatás VIP hello változik, törölje és hozza létre a felhőalapú szolgáltatás hello. Győződjön meg arról, hogy tooupdate hello IP-cím hello hálózati erőforráscsoporthoz tartozik, a telepítést követően. 
> 
> 

<!-- IMAGES -->
[Architecture]: ./media/app-service-app-service-environment-web-application-firewall/Architecture.png
[ConfigureEndpoint]: ./media/app-service-app-service-environment-web-application-firewall/ConfigureEndpoint.png
[AddManagementEndpoint]: ./media/app-service-app-service-environment-web-application-firewall/AddManagementEndpoint.png
[ManagementAddServices]: ./media/app-service-app-service-environment-web-application-firewall/ManagementAddServices.png
[ManagementDashboard]: ./media/app-service-app-service-environment-web-application-firewall/ManagementDashboard.png
[ManagementLoginPage]: ./media/app-service-app-service-environment-web-application-firewall/ManagementLoginPage.png
[TrafficManagerEndpoint]: ./media/app-service-app-service-environment-web-application-firewall/TrafficManagerEndpoint.png
[ConfigureTrafficManager]: ./media/app-service-app-service-environment-web-application-firewall/ConfigureTrafficManager.png
[WebsiteTranslations]: ./media/app-service-app-service-environment-web-application-firewall/WebsiteTranslations.png
