---
title: "az App Service Environment-környezet v1 aaaHow tooCreate"
description: "Egy app service környezetben v1 leírását létrehozási folyamata"
services: app-service
documentationcenter: 
author: ccompy
manager: stefsch
editor: 
ms.assetid: 81bd32cf-7ae5-454b-a0d2-23b57b51af47
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/11/2017
ms.author: ccompy
ms.openlocfilehash: 95feb33854eee5bac02fa68b066e2fc10eb3fede
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-an-app-service-environment-v1"></a>Hogyan tooCreate egy App Service Environment-környezet 1-es verzió 

> [!NOTE]
> Ez a cikk hello App Service Environment-környezet v1 tárgya. App Service-környezet, amely könnyebben toouse, és nagyobb teljesítményű infrastruktúra futó hello újabb verziója van telepítve. több hello új verzióval kapcsolatos kezdődnie hello toolearn [bemutatása toohello App Service Environment-környezet](../app-service/app-service-environment/intro.md).
> 

### <a name="overview"></a>Áttekintés
App Service-környezet (ASE) hello beállítás Premium szolgáltatás az Azure App Service egy továbbfejlesztett konfigurációs funkció, amely nem érhető el a hello több-bérlős bélyegzők letölti. hello ASE funkció tulajdonképpen telepíti hello Azure App Service egy ügyfél virtuális hálózathoz. App Service Environment-környezetek megértése hello képességeket kínál toogain olvasási hello [Mi az az App Service-környezetek] [ WhatisASE] dokumentációját.

### <a name="before-you-create-your-ase"></a>A ASE létrehozása előtt
Fontos fontos toobe tudomást hello dolgot nem módosítható. Nem módosítható a ASE kapcsolatos létrehozásukat követően aspektusait a következők:

* Hely
* Előfizetés
* Erőforráscsoport
* A virtuális hálózaton használt
* Használt 
* Alhálózat mérete

Ha kiadási VNet és alhálózat, megadása ellenőrizzük, hogy van elég nagy tooaccomodate jövőbeli növekedésének. 

### <a name="creating-an-app-service-environment-v1"></a>Alkalmazás létrehozása szolgáltatás környezet 1-es verzió
toocreate egy App Service Environment-környezet v1 toosearch kell hello Azure piactérre ***App Service Environment-környezet v1***, vagy új átnézi -> Web + mobil -> App Service Environment-környezet. egy ASEv1 toocreate:

1. Adja meg a ASE hello nevét. hello neve hello ASE megadott hello ASE létrehozott hello alkalmazásokhoz használható. Ha hello ASE neve appsvcenvdemo hello altartománynév lenne. *appsvcenvdemo.p.azurewebsites.net*. Ha így hozott létre az alkalmazás neve *mytestapp* akkor megcímezhető: *mytestapp.appsvcenvdemo.p.azurewebsites.net*. A ASE hello neve szóközt nem használható. Nagybetűk hello nevét használja, ha a hello tartománynév hello teljes kis verziója ugyanez a neve lesz. Ha a egy ILB majd a ASE neve nem szerepel a altartomány de inkább explicit módon megadott ASE létrehozása során
   
    ![][1]
2. Válassza ki előfizetését. a ASE használt hello előfizetés esetében is hello egyet, amely az összes alkalmazást, hogy ASE jön létre. Nem helyezhető el a ASE egy Vnetet, amely egy másik előfizetésben található
3. Válassza ki vagy adja meg egy új erőforráscsoportot. a ASE használt hello erőforráscsoport kell lennie hello ugyanaz a virtuális hálózat használt. Ha egy már meglévő virtuális hálózatot választja, akkor hello erőforráscsoport kiválasztása a ASE a frissített tooreflect lesz, a virtuális hálózat.
   
    ![][2]
4. Adja meg a virtuális hálózat és a hely beállításokat. Válasszon toocreate új virtuális hálózatot, vagy válasszon egy már meglévő virtuális hálózat. Ha egy új virtuális hálózat ezután megadhatja a nevét és helyét. hello új virtuális hálózat lesz hello cím tartomány 192.168.250.0/23 és nevű alhálózat **alapértelmezett** , amely 192.168.250.0/24 típusúként van definiálva. Egy korábban létező klasszikus és Resource Manager virtuális hálózat is egyszerűen válassza. hello VIP választást határozza meg, ha a ASE közvetlenül elérhetők hello internet (külső) vagy egy belső Load Balancer (ILB) használ. További róluk, olvassa el toolearn [egy belső terheléselosztó használata az App Service-környezetek][ILBASE]. Ha egy külső VIP típusú jelölje be IP-címeket hány külső hello rendszere IPSSL célokra jön létre. Ha belső majd szüksége toospecify hello altartományt, amelyet a ASE fog használni. ASEs telepíthető a virtuális hálózatok, amelyek *vagy* nyilvános-címtartományokat, *vagy* RFC1918 címterek (azaz Magáncímeket). A sorrend toouse egy nyilvános-címtartománnyal rendelkező virtuális hálózat kell toocreate hello VNet időben. Amikor kiválaszt egy már meglévő virtuális hálózat szüksége lesz egy új alhálózatot toocreate ASE létrehozása során. **Nem használhat egy korábban létrehozott alhálózati hello portálon. Létrehozhat egy ASE már meglévő alhálózattal, a resource manager-sablon használatával ASE létrehozásakor.** egy ASE toocreate sablonból hello információkat itt használja [egy App Service Environment-környezet létrehozása sablonból] [ ILBAseTemplate] és itt [ILB App Service-környezetek létrehozása sablonból] [ASEfromTemplate].

### <a name="details"></a>Részletek
Egy ASE 2 első véget ér, és a 2 feldolgozónak hozza létre. hello első karakterlánccal végződik-e hello HTTP/HTTPS-végpontként személyekről, és a forgalom küldése toohello munkavállalók, amelyek az alkalmazások üzemeltető hello szerepkörök. Hello mennyiség beállítása ASE létrehozása után, és még akkor is létre lehet hozni automatikus skálázási szabályok ezeknek az erőforráskészleteknek. További részletekért manuális skálázás körül, kezelési és figyelési egy App Service Environment-környezet itt lépjen: [hogyan tooconfigure egy App Service Environment-környezet][ASEConfig] 

Csak hello egy ASE hello ASE által használt hello alhálózati létezhet. hello alhálózat nem használható a hello ASE csakis

### <a name="after-app-service-environment-v1-creation"></a>App Service Environment-környezet v1 létrehozása után
ASE létrehozása után módosíthatja:

* Előtér mennyisége (minimális: 2)
* A munkavállalók mennyiség (minimális: 2)
* Az IP-SSL elérhető IP-címek mennyisége
* Számítási erőforrás mérete hello előtér vagy dolgozók által használt (előtér minimális mérete P2)

További részletek manuális skálázás, felügyeleti és App Service Environment-környezetek figyelése itt érhetők el: [hogyan tooconfigure egy App Service Environment-környezet][ASEConfig] 

Információ az automatikus skálázás van egy útmutató itt: [hogyan tooconfigure automatikus skálázás egy App Service Environment-környezet][ASEAutoscale]

Nincsenek további függőségek, amelyek nincsenek például hello adatbázis és a tárolási testreszabását. Ezek az Azure-ban kezelt és hello rendszer kapható. hello rendszer storage támogatja too500 GB-ot használ fel hello teljes App Service Environment-környezet és hello adatbázis módosul az Azure-ban hello skála hello rendszer igény szerint.

## <a name="getting-started"></a>Bevezetés
Összes cikket, és hogyan-a következőre az App Service Environment-környezetek érhetők el hello [alkalmazásszolgáltatási környezetek – fontos fájl](../app-service/app-service-app-service-environments-readme.md).

Lásd az App Service Environment-környezet 1-es verzió használatába tooget [bemutatása toohello App Service Environment-környezet 1-es verzió][WhatisASE]

Hello Azure App Service platformmal kapcsolatos további információkért lásd: [Azure App Service][AzureAppService].

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!--Image references-->
[1]: ./media/app-service-web-how-to-create-an-app-service-environment/asecreate-basecreateblade.png
[2]: ./media/app-service-web-how-to-create-an-app-service-environment/asecreate-vnetcreation.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[ASEConfig]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/ 
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/ 
[ASEAutoscale]: http://azure.microsoft.com/documentation/articles/app-service-environment-auto-scale/
[ILBASE]: http://azure.microsoft.com/documentation/articles/app-service-environment-with-internal-load-balancer/
[ILBAseTemplate]: http://azure.microsoft.com/documentation/templates/201-web-app-ase-create/
[ASEfromTemplate]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-create-ilb-ase-resourcemanager/
