---
title: "aaaGet teszt termelési környezetben, a Web Apps használatába"
description: "Hello teszt termelési (TiP) szolgáltatásban az Azure App Service Web Apps megismerése."
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: 4623468d-886e-4203-8012-8f86deb2790b
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/13/2016
ms.author: cephalin
ms.openlocfilehash: 2ddbd532ffe2a4f3e07fd386d9741a3fde3639ca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-test-in-production-for-web-apps"></a>Ismerkedés a termelési környezetben végzett teszteléssel a Web Apps használatával
Éles környezetben vizsgálat, vagy a webes alkalmazás használatával élő felhasználói forgalom, élő-vizsgálat a vizsgálat stratégia, amely alkalmazás a fejlesztők egyre integrálja a [gyors fejlesztési](https://en.wikipedia.org/wiki/Agile_software_development) módszert. Lehetővé teszi az alkalmazások élő felhasználói forgalomnak a termelési környezetben tootest hello minőségének megakadályozását toosynthesized adatként tesztkörnyezetben. Jelentkezik, mintha az új alkalmazás tooreal felhasználói, akkor is tájékoztatni hello valós problémák az alkalmazás esetleg szembesülhetnek, ha telepítve van. Hello funkciót, a teljesítmény és az alkalmazásfrissítések hello kötet, a sebesség és a felhasználó forgalmat, amely akkor soha nem hozzávetőleges, tesztelési környezetben számos elleni értékének ellenőrizheti.

## <a name="traffic-routing-in-app-service-web-apps"></a>Forgalom-útválasztási az App Service Web Apps
A forgalom útválasztásához hello szolgáltatás [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714), megadhatja, hogy egy élő felhasználói forgalom tooone vagy több része [üzembe helyezési](web-sites-staged-publishing.md), és az alkalmazás a [Azure-alkalmazás Elemzések](/services/application-insights/) vagy [Azure HDInsight](/services/hdinsight/), vagy egy külső eszköz, például [New Relic](/marketplace/partners/newrelic/newrelic/) toovalidate a módosítást. Például a következő forgatókönyvek az App Service hello valósíthatja meg:

* Működési hibák fel, és a frissítések előzetes toosite kiterjedő környezetben szűk keresztmetszetek rögzítési ponthoz
* Hajtsa végre a "ellenőrzött teszt járatok" módosítás használhatóság metrikák hello beta alkalmazások szolgáltatásigényeit:
* Fokozatosan hatékonyságának növeléséhez tooa új frissítést, és hiba esetén toohello verziószámának le szabályosan biztonsági 
* Az alkalmazás üzleti eredmények történő futtatásával optimalizálják [A / B tesztek](https://en.wikipedia.org/wiki/A/B_testing) vagy [multivariate tesztek](https://en.wikipedia.org/wiki/Multivariate_testing_in_marketing) több üzembe helyezési pontokon

### <a name="requirements-for-using-traffic-routing-in-web-apps"></a>A Web Apps a forgalom útválasztásához használatának követelményei
* A webalkalmazás futtatásához **szabványos** vagy **prémium** szintet, mert az több üzembe helyezési tárhely szükséges.
* A sorrend toowork megfelelően, forgalom útválasztásához szükség van a cookie-k toobe hello felhasználók böngészőben engedélyezve. Forgalom-útválasztás hello élettartama hello ügyfél munkamenet cookie-k toopin egy ügyfél böngésző tooa üzembe helyezési pont használja.
* Forgalom-útválasztás szituációkat speciális tipp Azure PowerShell-parancsmagokkal.

## <a name="route-traffic-segment-tooa-deployment-slot"></a>Útvonal forgalom szegmens tooa üzembe helyezési tárhelyet
Hello alapvető szinten minden tipp forgatókönyvben az élő forgalom tooa nem éles üzembe helyezési pont előre definiált százalékaként továbbítani. toodo ezt, hajtsa végre hello az alábbi lépéseket:

> [!NOTE]
> hello lépéseket itt feltételezi, hogy már rendelkezik egy [nem éles üzembe helyezési pont](web-sites-staged-publishing.md) és, hogy hello szükséges webszolgáltatás alkalmazástartalom már [telepített](web-sites-deploy.md) tooit.
> 
> 

1. Jelentkezzen be a hello [Azure Portal](https://portal.azure.com/).
2. A webalkalmazás panelen kattintson **beállítások** > **a forgalom útválasztásához**.
   ![](./media/app-service-web-test-in-production/01-traffic-routing.png)
3. Jelölje be hello tárolóhely tooroute forgalom tooand hello százalékos hello teljes forgalom felügyelni, majd kattintson a kívánt **mentése**.
   
    ![](./media/app-service-web-test-in-production/02-select-slot.png)
4. Nyissa meg toohello telepített környezet tárolóhelye panelen. Meg kell jelennie élő forgalom irányított tooit alatt.
   
    ![](./media/app-service-web-test-in-production/03-traffic-routed.png)

Ha a forgalom útválasztásához konfigurálva van, a hello megadott, ügyfeleinek százalékos aránya lesznek véletlenszerűen irányított tooyour nem éles tárolóhelyre. Azt azonban fontos, hogy egy ügyfél automatikusan irányított tooa adott tárolóhelye, állnak, hogy az ügyfélmunkamenethez hello élettartama "rögzített" toothat tárhely toonote. Ez kész toopin hello felhasználói munkamenet cookie-k használatával. Ha a hello HTTP-kérelmek vizsgálja meg, egy `TipMix` cookie-k minden későbbi kérés.

![](./media/app-service-web-test-in-production/04-tip-cookie.png)

## <a name="force-client-requests-tooa-specific-slot"></a>Ügyfél kérelmek tooa adott tárolóhely kényszerítése
Továbbá tooautomatic forgalmának irányítását, App Service képes tooroute kérelmek tooa adott bővítőhely. Ez akkor hasznos, ha azt szeretné, hogy a felhasználók toobe képes tooopt-be vagy lemondja a béta-alkalmazás. toodo, használhatja a hello `x-ms-routing-name` lekérdezési paraméter.

tooreroute felhasználók tooa adott tárhely használatával `x-ms-routing-name`, meg kell győződnie arról, hogy a hello bővítőhely már hozzá van adva toohello forgalmának irányítását a listában. Explicit módon szeretne tooroute tooa tárolóhely, mivel nem számít, hello tényleges útválasztási százalékos be. Ha azt szeretné, hogy a "béta link", amellyel a felhasználók összeállíthatnak tooaccess hello beta alkalmazást.

![](./media/app-service-web-test-in-production/06-enable-x-ms-routing-name.png)

### <a name="opt-users-out-of-beta-app"></a>Részvétel a felhasználók beta alkalmazásból
toolet felhasználók tilthatják le a béta-alkalmazás, például helyezhet el ezt a hivatkozást a weblapon:

    <a href="<webappname>.azurewebsites.net/?x-ms-routing-name=self">Go back tooproduction app</a>

karakterlánc hello `x-ms-routing-name=self` határozza meg a hello éles tárolóhelyre. Amint hello ügyfél böngésző hozzáférés hello hivatkozás, nem csak az átirányított toohello éles tárolóhelyre, de minden későbbi kérés fogja tartalmazni hello `x-ms-routing-name=self` cookie-k, amelyek PIN hello munkamenet toohello éles tárolóhelyre.

![](./media/app-service-web-test-in-production/05-access-production-slot.png)

### <a name="opt-users-in-toobeta-app"></a>Részvétel a felhasználók toobeta alkalmazásban
toolet felhasználók részt vevő tooyour beta alkalmazást, a set hello azonos toohello paraméternév hello nem éles bővítőhely, például lekérdezése:

        <webappname>.azurewebsites.net/?x-ms-routing-name=staging

## <a name="more-resources"></a>További erőforrások
* [Átmeneti környezet az Azure App Service web Apps beállítása](web-sites-staged-publishing.md)
* [Kiszámítható módon tudja az Azure-ban egy összetett alkalmazás központi telepítése](app-service-deploy-complex-application-predictably.md)
* [Agilis szoftverfejlesztői az Azure App Service](app-service-agile-software-development.md)
* [DevOps környezetek hatékony használata a webalkalmazások](app-service-web-staged-publishing-realworld-scenarios.md)

