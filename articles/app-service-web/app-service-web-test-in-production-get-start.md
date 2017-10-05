---
title: "Ismerkedés a termelési környezetben végzett teszteléssel a Web Apps használatával"
description: "A teszt termelési (TiP) szolgáltatásban az Azure App Service Web Apps megismerése."
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
ms.openlocfilehash: 9f38b635140cacf0513c75385bce3c110a930969
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-test-in-production-for-web-apps"></a>Ismerkedés a termelési környezetben végzett teszteléssel a Web Apps használatával
Éles környezetben vizsgálat, vagy a webes alkalmazás használatával élő felhasználói forgalom, élő-vizsgálat a vizsgálat stratégia, amely alkalmazás a fejlesztők egyre integrálja a [gyors fejlesztési](https://en.wikipedia.org/wiki/Agile_software_development) módszert. Lehetővé teszi az alkalmazások élő felhasználói forgalom minőségének tesztelése az éles környezetben, egy tesztkörnyezetben szintetizált adatok szemben. Jelentkezik, mintha az új alkalmazás valódi felhasználók számára, akkor is tájékoztatni az alkalmazás esetleg szembesülhetnek, ha telepítve van a tényleges problémákat. A funkciót, a teljesítmény és az alkalmazások frissítése a kötet, a sebesség és a felhasználó forgalmat, amely akkor soha nem hozzávetőleges, tesztelési környezetben számos elleni értékének ellenőrizheti.

## <a name="traffic-routing-in-app-service-web-apps"></a>Forgalom-útválasztási az App Service Web Apps
A forgalom útválasztásához szolgáltatásával [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714), megadhatja, hogy egy vagy több élő felhasználói forgalom egy része [üzembe helyezési](web-sites-staged-publishing.md), és az alkalmazás a [Azure Application Insights](/services/application-insights/) vagy [Azure HDInsight](/services/hdinsight/), vagy egy külső eszköz, például [New Relic](/marketplace/partners/newrelic/newrelic/) a módosítás érvényesítéséhez. Az App Service például is valósítja meg az alábbi esetekben:

* Működési hibák fel, és a szűk keresztmetszetek a frissítések helyi szintű telepítését megelőzően a rögzítési ponthoz
* Hajtsa végre a "ellenőrzött teszt járatok" módosítás használhatóság metrikákat a béta alkalmazások szolgáltatásigényeit:
* Legfeljebb egy új frissítési fokozatosan hatékonyságának, és szabályosan biztonsági az aktuális verzióra hiba esetén 
* Az alkalmazás üzleti eredmények történő futtatásával optimalizálják [A / B tesztek](https://en.wikipedia.org/wiki/A/B_testing) vagy [multivariate tesztek](https://en.wikipedia.org/wiki/Multivariate_testing_in_marketing) több üzembe helyezési pontokon

### <a name="requirements-for-using-traffic-routing-in-web-apps"></a>A Web Apps a forgalom útválasztásához használatának követelményei
* A webalkalmazás futtatásához **szabványos** vagy **prémium** szintet, mert az több üzembe helyezési tárhely szükséges.
* Ahhoz, hogy a megfelelő működés érdekében a forgalom útválasztásához engedélyezni kell a felhasználó böngészőben a cookie-k van szükség. Forgalom-útválasztás rögzítése élettartama egy üzembe helyezési pont az ügyfél böngészője az ügyfél-munkamenet cookie-kat használ.
* Forgalom-útválasztás szituációkat speciális tipp Azure PowerShell-parancsmagokkal.

## <a name="route-traffic-segment-to-a-deployment-slot"></a>Útvonal forgalom szegmens egy üzembe helyezési tárhelyet
Minden tipp forgatókönyv alapvető szinten a működés közbeni forgalom előre definiált százalékos egy nem éles üzembe helyezési tárhelyet a továbbítani. Ehhez kövesse az alábbi lépéseket:

> [!NOTE]
> Az itt lépéseket feltételezi, hogy már rendelkezik egy [nem éles üzembe helyezési pont](web-sites-staged-publishing.md) és a kívánt webszolgáltatás alkalmazástartalom már [telepített](web-sites-deploy.md) rá.
> 
> 

1. Jelentkezzen be a [Azure-portálon](https://portal.azure.com/).
2. A webalkalmazás panelen kattintson **beállítások** > **a forgalom útválasztásához**.
   ![](./media/app-service-web-test-in-production/01-traffic-routing.png)
3. Válassza ki a tárolóhely irányíthatja a forgalmat és a teljes forgalom felügyelni, majd kattintson az kívánt **mentése**.
   
    ![](./media/app-service-web-test-in-production/02-select-slot.png)
4. Nyissa meg a telepített környezet tárolóhelye paneljét. Most látnia kell azt a csatlakoztatott élő forgalmat.
   
    ![](./media/app-service-web-test-in-production/03-traffic-routed.png)

Miután beállította a forgalom útválasztásához, megadott ügyfeleinek százalékos aránya véletlenszerűen továbbítja a nem éles tárolóhelyre. Fontos azonban vegye figyelembe, hogy amennyiben az ügyfél automatikusan történik egy adott helyre, akkor lesz "rögzítve" adott helyre, hogy ügyfél munkamenet során. Ez történik a cookie-k segítségével rögzítheti a felhasználói munkamenet. A HTTP-kérelmek vizsgálja meg, ha megtalálja a `TipMix` cookie-k minden későbbi kérés.

![](./media/app-service-web-test-in-production/04-tip-cookie.png)

## <a name="force-client-requests-to-a-specific-slot"></a>Egy adott helyre ügyfélkérelmek kényszerítése
Automatikus forgalom az útválasztáson kívül az App Service képes kérelmek adott helyre is. Ez akkor hasznos, ha azt szeretné, hogy a felhasználók tudja irányítani a vagy lemondja a béta-alkalmazás. Ehhez használja a `x-ms-routing-name` lekérdezési paraméter.

A felhasználók számára egy adott tárhely használatával átirányítása `x-ms-routing-name`, meg kell győződnie arról, hogy a tárolóhely már hozzá van adva a forgalom útválasztásához listájához. Szeretné továbbítani a tárhely explicit módon, mert nem számít, beállíthatja a tényleges útválasztási százalék. Ha azt szeretné, hogy összeállíthatnak egy "béta link", amellyel a felhasználók a béta alkalmazás eléréséhez.

![](./media/app-service-web-test-in-production/06-enable-x-ms-routing-name.png)

### <a name="opt-users-out-of-beta-app"></a>Részvétel a felhasználók beta alkalmazásból
Ahhoz, hogy a felhasználók tilthatják le a béta-alkalmazás, például helyezhet el ezt a hivatkozást a weblapon:

    <a href="<webappname>.azurewebsites.net/?x-ms-routing-name=self">Go back to production app</a>

A karakterlánc `x-ms-routing-name=self` határozza meg az éles webalkalmazásra. Ha az ügyfél böngészője a hivatkozás megnyitásához nem csak azt átirányítja az éles webalkalmazásra, de minden későbbi kérés fogja tartalmazni az `x-ms-routing-name=self` , amely rögzíti az éles tárolóhelyre munkamenet cookie-k.

![](./media/app-service-web-test-in-production/05-access-production-slot.png)

### <a name="opt-users-in-to-beta-app"></a>Részt vesz a béta-alkalmazás és a felhasználóknak
Így a felhasználók részt vevő a béta-alkalmazásba, állítsa be ugyanazon lekérdezési paraméter nem éles tárhely nevét:

        <webappname>.azurewebsites.net/?x-ms-routing-name=staging

## <a name="more-resources"></a>További erőforrások
* [Átmeneti környezet az Azure App Service web Apps beállítása](web-sites-staged-publishing.md)
* [Kiszámítható módon tudja az Azure-ban egy összetett alkalmazás központi telepítése](app-service-deploy-complex-application-predictably.md)
* [Agilis szoftverfejlesztői az Azure App Service](app-service-agile-software-development.md)
* [DevOps környezetek hatékony használata a webalkalmazások](app-service-web-staged-publishing-realworld-scenarios.md)

