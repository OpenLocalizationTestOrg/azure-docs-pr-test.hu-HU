---
title: "a WordPress alkalmazás hello Azure-portálon öt perc múlva aaaDeploy |} Microsoft Docs"
description: "Megtudhatja, milyen egyszerűen toorun webalkalmazásokkal az App Service-ben a WordPress alkalmazás telepítésével. Az eredmények azonnal láthatók."
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: 6feac128-c728-4491-8b79-962da9a40788
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/13/2017
ms.author: cephalin
ms.openlocfilehash: 4cd26bbacf89657997847ded1284e472288ddebe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-wordpress-app-in-hello-azure-portal-in-five-minutes"></a>Öt perc múlva hello Azure-portálon a WordPress alkalmazás telepítése

Az oktatóanyag bemutatja, hogyan toodeploy az első [WordPress](https://wordpress.org/) webalkalmazás túl[Azure App Service](../app-service/app-service-value-prop-what-is.md) perc múlva.

![WordPress-webhely](./media/app-service-web-get-started-php-portal/wpdashboard.png)

## <a name="prerequisites"></a>Előfeltételek
Szüksége van egy Microsoft Azure-fiókra. Ha nincs fiókja, [regisztráljon egy ingyenes próbaverzióra](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F), vagy [aktiválhatja a Visual Studio előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

> [!NOTE]
> Az [App Service kipróbálása](https://azure.microsoft.com/try/app-service/) Azure-fiók nélkül is lehetséges. Hozzon létre egy alapszintű alkalmazást, és nincs szükség bankkártyára, nem jár kötelezettségekkel tooan óra--be azt a lejátszásához.
> 
> 

## <a name="deploy-hello-wordpress-app"></a>Hello WordPress alkalmazás üzembe helyezése
1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).

2. Nyissa meg a [https://portal.azure.com/#create/WordPress.WordPress](https://portal.azure.com/#create/WordPress.WordPress) webhelyet.

    Ez a hivatkozás egy helyi tooimmediately egy új WordPress-alkalmazás beállítása az hello Azure-portálon.

3. Az **Alkalmazás neve** mezőben adja meg a webapp nevét. Látni fogja hello mezőben zöld pipa jelzi, ha hello név egyedi az hello `azurewebsites.net` tartomány.
   
5. A **erőforráscsoport**, kattintson a **hozzon létre új** új toocreate [erőforráscsoport](../azure-resource-manager/resource-group-overview.md), majd adjon neki egy nevet.

6. Az **Adatbázis-szolgáltató** területen válassza a **CleaDB** elemet.

7. Kattintson az **App Service-csomag/Hely** > **Új létrehozása** lehetőségre. Hello konfigurálása [App Service-csomag](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) látható módon:

    - A **App Service-csomag**, hello kívánt neve.
    - A **hely**, egy hely toohost a csomag kiválasztása.
    - Kattintson a **Tarifacsomag** lehetőségre, majd válassza az **F1 – Ingyenes** elemet vagy egy másik megfelelő szintet, majd kattintson a **Kiválasztás** lehetőségre.
    - Kattintson az **OK** gombra.

8. Kattintson az **Adatbázis** > **Új létrehozása** elemre. Konfigurálja a hello SQL-adatbázis látható módon:

    - Az **Adatbázis neve** mezőben adjon meg egy adatbázisnevet. 
    - A **hely**, válassza a hello hello App Service-csomag megegyező helyen.
    - Kattintson a **Tarifacsomag** lehetőségre, majd válassza a **Merkúr** elemet vagy egy másik megfelelő szintet, majd kattintson a **Kiválasztás** lehetőségre.
    - Válassza ki a **Jogi feltételek** elemet, és kattintson a **Vásárlás** lehetőségre.
    - Kattintson az **OK** gombra.

9. Kattintson a **Létrehozás** gombra.

    Az Azure ekkor létrehozza a WordPress-alkalmazást a konfiguráció alapján. A **Központi telepítés elindítva...** értesítést kell látnia

    ![Az üzembe helyezés megkezdődött – az első WordPress az Azure App Service-ben](./media/app-service-web-get-started-php-portal/deployment-started.png)
   
## <a name="launch-and-manage-your-wordpress-web-app"></a>A WordPress-webalkalmazás elindítása és kezelése

Ha az Azure befejezte az alkalmazás üzembe helyezését, megjelenik egy újabb értesítés.

![Az üzembe helyezés sikerült– az első WordPress az Azure App Service-ben](./media/app-service-web-get-started-php-portal/deployment-succeeded.png)

1. Kattintson a hello értesítés. Ha, kihagyva, akkor mindig elérhető legyen az hello értesítési harang gombra kattintva (![értesítési alábbi – az Azure App Service első WordPress](./media/app-service-web-get-started-dotnet-portal/notification.png)).

    Ekkor meg kell jelennie a webapp felügyeleti [paneljének](../azure-resource-manager/resource-group-portal.md#manage-resources) (*panel*: egy vízszintesen megnyíló portáloldal).

3. Hello – áttekintés oldalra hello felső részén kattintson **Tallózás**.
   
    ![Tallózás – az első WordPress az Azure App Service-ben](./media/app-service-web-get-started-php-portal/browse.png)

    Most megjelenik a hello WordPress **üdvözlő** lap. Hello WordPress telepítésének konfigurálása, és azt lejátszásához!

    ![A Wordpress konfigurálása – az első WordPress az Azure App Service-ben](./media/app-service-web-get-started-php-portal/wordpress-config.png)
    
## <a name="next-steps"></a>Következő lépések
* [Létrehozása, konfigurálása és telepítése a Laravel web app tooAzure](app-service-web-php-get-started.md) -további hello alapvető ismeretek kell toorun bármely PHP webalkalmazást az Azure, például:

    * Alkalmazások létrehozása és konfigurálása az Azure-ban Powershellből/Bashből.
    * A PHP-verzió beállítása.
    * A start-fájl, amely nincs hello alkalmazás gyökérkönyvtárában használható.
    * Composer-automatizálás engedélyezése.
    * Hozzáférés környezetspecifikus változókhoz.
    * Gyakori hibák elhárítása.

* [A kód tooAzure App Service telepítése](web-sites-deploy.md)-megtudhatja, hogyan szabályozza a toodeploy FTP vagy a forrás a tárházak találhatók.
* [Funkció tooyour első webalkalmazás hozzáadása](app-service-web-get-started-2.md) -érvénybe az Azure alkalmazás toohello következő szint. Hitelesítheti felhasználóit. Igény szerint méretezheti. Beállíthat a teljesítménnyel kapcsolatos riasztásokat. Mindezt csupán néhány kattintással.
