---
title: "aaaCreate egy WordPress webalkalmazást az Azure App Service szolgáltatásban |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy új Azure web app egy WordPress bloghoz hello Azure portál használatával."
services: app-service\web
documentationcenter: php
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 193ae094-0d7c-4749-a09b-ff4b1240149e
ms.service: app-service-web
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: 3a95fcb6732c15a8200921ce474b6dde2298dec3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-wordpress-web-app-in-azure-app-service"></a>WordPress-webalkalmazás létrehozása az Azure App Service szolgáltatásban
[!INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

Ez az oktatóanyag bemutatja, hogyan toodeploy egy WordPress-bloghoz az Azure piactér hello hely.

Amikor elkészült, a hello oktatóanyag összekapcsolta a saját WordPress-blogja és hello felhőben futó.

![WordPress-webhely](./media/web-sites-php-web-site-gallery/wpdashboard.png)

Az oktatóanyagból a következőket sajátíthatja el:

* Hogyan toofind egy JSON-sablon a hello Azure piactéren.
* Hogyan toocreate egy webalkalmazást az Azure App Service, amely hello sablon alapján.
* Hogyan új hello tooconfigure Azure App Service szolgáltatás beállításainak web app és az adatbázis.

hello Azure piactér elérhetővé teszi a Microsoft, külső vállalatok és nyílt forráskódú szoftver-kezdeményezések által fejlesztett népszerű webalkalmazások széles skáláját. hello webalkalmazások épülnek a legkülönbözőbb népszerű keretrendszerekre, például a [PHP](/develop/nodejs/) ebben a WordPress példában, [.NET](/develop/net/), [Node.js](/develop/nodejs/), [Java](/develop/java/), és [Python](/develop/python/), kevés a tooname. egy webalkalmazást az Azure piactéren csak a szükséges szoftver a hello használt böngésző hello hello hello toocreate [Azure Portal](https://portal.azure.com/). 

Ebben az oktatóanyagban üzembe helyezett WordPress webhely hello MySQL adatbázis hello használ. Ha a hello adatbázis SQL adatbázis tooinstead használata, lásd: [Project Nami](http://projectnami.org/). **A Project Nami** hello piactéren keresztül érhetők el.

> [!NOTE]
> toocomplete ebben az oktatóanyagban a Microsoft Azure-fiók szükséges. Ha nincs fiókja, [aktiválhatja a Visual Studio előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F), vagy [regisztráljon egy ingyenes próbaverzióra](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).
> 
> Ha azt szeretné, hogy az az Azure-fiók regisztrálása előtt az Azure App Service lépések tooget, nyissa meg túl[App Service kipróbálása](https://azure.microsoft.com/try/app-service/). Itt azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben. Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.
> 
> 

## <a name="select-wordpress-and-configure-for-azure-app-service"></a>Válassza ki a WordPresst, és konfigurálja azt az Azure App Service-hez
1. Jelentkezzen be toohello [Azure Portal](https://portal.azure.com/).
2. Kattintson az **Új** lehetőségre.
   
    ![Új létrehozása][5]
3. Keresse meg a **WordPress**-alkalmazást, majd kattintson a **WordPress** lehetőségre. Ha a MySQL helyett az SQL-adatbázis toouse, keressen **Project Nami**.
   
    ![A listában szereplő WordPress][7]
4. Hello hello WordPress alkalmazás leírásának elolvasása, után kattintson **létrehozása**.
   
    ![Létrehozás](./media/web-sites-php-web-site-gallery/create.png)
5. Adja meg egy nevet a webalkalmazás hello hello **webalkalmazás** mezőbe.
   
    A névnek kell lennie egyedi hello azurewebsites.net tartományban, mert hello hello webalkalmazás URL-címe {név} lesznek. azurewebsites.net. Ha hello név nem egyedi, hello szövegmezőben egy piros felkiáltójel jelenik meg.
6. Ha egynél több előfizetéssel, válassza ki a hello rendelkezik egy használni kívánt toouse. 
7. Válasszon egy **erőforráscsoportot**, vagy hozzon létre egy újat.
   
    További információ az erőforráscsoportokkal kapcsolatban: [Az Azure Resource Manager áttekintése](../azure-resource-manager/resource-group-overview.md).
8. Válasszon ki egy **App Service-csomagot/-helyet**, vagy hozzon létre egy újat.
   
    További információk az App Service-csomagokról: [Az Azure App Service-csomagok áttekintése](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)    
9. Kattintson a **adatbázis**, majd a hello **új MySQL-adatbázis** panelen adja meg a szükséges hello értékeket, a MySQL adatbázis konfigurálásához.
   
    a. Adjon meg egy új nevet, vagy hello alapértelmezett neve mező maradjon.
   
    b. Hagyja hello **adatbázistípus** túl beállítása**megosztott**.
   
    c. Válassza ki a hello ugyanarra a helyre, amelyiket Ön hello választott hello webalkalmazás.
   
    d. Válasszon egy tarifacsomagot. A Merkúr (ingyenes tarifacsomag, minimális megengedett kapcsolódással és lemezterülettel) ideális ehhez az oktatóanyaghoz.
10. A hello **új MySQL-adatbázis** panelen kattintson a **OK**. 
11. A hello **WordPress** panelen fogadja el a hello jogi feltételeket, és kattintson **létrehozása**. 
    
     ![A webalkalmazás konfigurálása](./media/web-sites-php-web-site-gallery/configure.png)
    
     Az Azure App Service általában kevesebb, mint egy percen belül hoz létre hello web app alkalmazásban. Hello portállapon hello tetején hello harang ikonra kattintva figyelemmel követheti hello folyamatban van.
    
     ![Folyamatjelző](./media/web-sites-php-web-site-gallery/progress.png)

## <a name="launch-and-manage-your-wordpress-web-app"></a>A WordPress-webalkalmazás elindítása és kezelése
1. Ha hello webalkalmazás létrehozása befejeződött, nyissa meg a hello Azure Portal toohello erőforráscsoportban, amelyben létrehozta hello alkalmazás, és megtekintheti hello web app és hello adatbázis.
   
    hello hello villanykörte ikonnal rendelkező további erőforrás van [Application Insights](/services/application-insights/), amely a webalkalmazás figyelési szolgáltatásokat biztosít.
2. A hello **erőforráscsoport** paneljén kattintson hello webalkalmazás sorra.
   
    ![A webalkalmazás konfigurálása](./media/web-sites-php-web-site-gallery/resourcegroup.png)
3. Hello webalkalmazás panelen kattintson **Tallózás**.
   
    ![webhely URL-címe][browse]
4. A hello WordPress **üdvözlő** lapon adja meg a WordPress által kért hello konfigurációs információkat, és kattintson a **WordPress telepítése**.
   
    ![A WordPress konfigurálása](./media/web-sites-php-web-site-gallery/wpconfigure.png)
5. Jelentkezzen be a hello létrehozott hello hitelesítő adatok használatával **üdvözlő** lap.  
6. Megnyílik a webhelye irányítópultja.    
   
    ![WordPress-webhely](./media/web-sites-php-web-site-gallery/wpdashboard.png)

## <a name="next-steps"></a>Következő lépések
Megtudhatta, hogyan toocreate és üzembe egy PHP webalkalmazást hello gyűjteményből. Az Azure-ban a PHP használatával kapcsolatos további információkért lásd: hello [PHP fejlesztői központ](/develop/php/).

További információ toowork az App Service Web Apps, tekintse meg a hello hivatkozásokat, a bal oldalán (széles böngészőablakok esetén) a hello található hello vagy hello tetején (keskeny böngészőablakok esetén) hello lapját. 

## <a name="whats-changed"></a>A változások
* A webhelyek tooApp szolgáltatás útmutató toohello módosítást, lásd: [Azure App Service és a hatása a meglévő Azure-szolgáltatások](http://go.microsoft.com/fwlink/?LinkId=529714).

[5]: ./media/web-sites-php-web-site-gallery/startmarketplace.png
[7]: ./media/web-sites-php-web-site-gallery/search-web-app.png
[browse]: ./media/web-sites-php-web-site-gallery/browse-web.png
