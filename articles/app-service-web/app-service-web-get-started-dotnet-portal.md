---
title: "hello Azure-portálon öt perc múlva Umbraco webes alkalmazás aaaDeploy |} Microsoft Docs"
description: "Megtudhatja, milyen egyszerűen toorun webalkalmazásokkal az App Service-ben a minta az ASP.NET-alkalmazások üzembe helyezésével. Az eredmények azonnal láthatók."
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: b1e6bd58-48d1-4007-9d6c-53fd6db061e3
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2017
ms.author: cephalin
ms.openlocfilehash: 6da45f99b3043a3684e3d99c14e6443d597b5212
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-an-umbraco-web-app-in-hello-azure-portal-in-five-minutes"></a>Öt perc múlva a hello Azure-portálon Umbraco webes alkalmazás telepítése

Ez az oktatóanyag segít telepíteni n [Umbraco](https://our.umbraco.org/) webalkalmazás túl[Azure App Service](../app-service/app-service-value-prop-what-is.md) perc múlva.

![Umbraco-alkalmazás](./media/app-service-web-get-started-dotnet-portal/defaultpage.png)

## <a name="prerequisites"></a>Előfeltételek
Szüksége van egy Microsoft Azure-fiókra. Ha nincs fiókja, [regisztráljon egy ingyenes próbaverzióra](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F), vagy [aktiválhatja a Visual Studio előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

> [!NOTE]
> Az [App Service kipróbálása](https://azure.microsoft.com/try/app-service/) Azure-fiók nélkül is lehetséges. Hozzon létre egy alapszintű alkalmazást, és nincs szükség bankkártyára, nem jár kötelezettségekkel tooan óra--be azt a lejátszásához.
> 
> 

## <a name="deploy-hello-aspnet-app"></a>Hello ASP.NET alkalmazás üzembe helyezése
1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).

2. Nyissa meg a [https://portal.azure.com/#create/umbracoorg.UmbracoCMS](https://portal.azure.com/#create/umbracoorg.UmbracoCMS) webhelyet.

    Ez a hivatkozás egy helyi tooimmediately egy új Umbraco-alkalmazás beállítása az Azure-portálon hello.

3. Az **Alkalmazás neve** mezőben adja meg a webapp nevét. Látni fogja hello mezőben zöld pipa jelzi, ha hello név egyedi az hello `azurewebsites.net` tartomány.
   
5. A **erőforráscsoport**, kattintson a **hozzon létre új** új toocreate [erőforráscsoport](../azure-resource-manager/resource-group-overview.md), majd adjon neki egy nevet.

7. Kattintson az **App Service-csomag/Hely** > **Új létrehozása** lehetőségre. Hello konfigurálása [App Service-csomag](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) látható módon:

    - A **App Service-csomag**, hello kívánt neve.
    - A **hely**, egy hely toohost a csomag kiválasztása.
    - Kattintson a **Tarifacsomag** lehetőségre, majd válassza az **F1 – Ingyenes** elemet vagy egy másik megfelelő szintet, majd kattintson a **Kiválasztás** lehetőségre.
    - Kattintson az **OK** gombra.

    A Umbraco CMS konfigurációs mostantól a következő képernyőkép hello kell hasonlítania:

    ![A konfiguráció folyamatban – az első Umbraco-alkalmazás az Azure App Service-ben](./media/app-service-web-get-started-dotnet-portal/configure-in-progress.png)

12. Kattintson az **SQL Database** > **Új adatbázis létrehozása** lehetőségre. Konfigurálja a hello SQL-adatbázis látható módon:

    - A **Név** mezőben írjon be egy nevet, például a **myDB**-t.
    - Kattintson a **Tarifacsomag** lehetőségre, majd válassza az **F – Ingyenes** elemet vagy egy másik megfelelő szintet, majd kattintson a **Kiválasztás** lehetőségre.
    - Kattintson a **Célkiszogáló** > **Új kiszolgáló létrehozása** lehetőségre. Konfigurálja a hello adatbázis-kiszolgáló látható módon:

        - A **Kiszolgálónév** mezőben adjon meg egy kiszolgálónevet. Látni fogja hello mezőben zöld pipa jelzi, ha hello név egyedi az hello `.database.windows.net` tartomány.
        - A **kiszolgáló-rendszergazdai bejelentkezés**, típus hello rendszergazdán felhasználónév szükséges.
        - A **jelszó** és **jelszó megerősítése**, írja be a hello kívánt jelszót.
        - A hely kiválasztása hello hello webalkalmazás használhatja ugyanazon a helyen.
        - Győződjön meg arról, hogy **engedélyezése az azure szolgáltatások tooaccess server** van kiválasztva.
        - Kattintson a **Kiválasztás** gombra.
    
    - Kattintson a **Kiválasztás** gombra.

13. Kattintson a **webalkalmazás-beállításainak**, adja meg a hello felhasználónevet és jelszót, és kattintson a **OK**.

    A Umbraco CMS konfigurációs mostantól a következő képernyőkép hello kell hasonlítania:

    ![A konfiguráció kész – az első Umbraco-alkalmazás az Azure App Service-ben](./media/app-service-web-get-started-dotnet-portal/configure-complete.png)

14. Kattintson a  **Create** (Létrehozás) gombra.
    
    Az Azure ekkor létrehozza az Umbraco-alkalmazást a konfiguráció alapján. A **Központi telepítés elindítva...** értesítést kell látnia

    ![Az üzembe helyezés sikerült– az első Umbraco-alkalmazás az Azure App Service-ben](./media/app-service-web-get-started-dotnet-portal/deployment-started.png)
   
## <a name="launch-and-manage-your-umrbaco-web-app"></a>Az Umbraco-alkalmazás elindítása és kezelése

Ha az Azure befejezte az alkalmazás üzembe helyezését, megjelenik egy újabb értesítés.

![Az üzembe helyezés sikerült– az első Umbraco-alkalmazás az Azure App Service-ben](./media/app-service-web-get-started-dotnet-portal/deployment-succeeded.png)

1. Kattintson a hello értesítés. Ha, kihagyva, akkor mindig elérhető legyen az hello értesítési harang gombra kattintva (![értesítési alábbi – az Azure App Service első Umbraco](./media/app-service-web-get-started-dotnet-portal/notification.png)).

    Ekkor meg kell jelennie a webapp felügyeleti [paneljének](../azure-resource-manager/resource-group-portal.md#manage-resources) (*panel*: egy vízszintesen megnyíló portáloldal).

3. Hello – áttekintés oldalra hello felső részén kattintson **Tallózás**.
   
    ![Tallózás – az első Umbraco-alkalmazás az Azure App Service-ben](./media/app-service-web-get-started-dotnet-portal/browse.png)

    Most láthatja hello Umbraco **üdvözlő** lap. Hello Umbraco telepítésének konfigurálása, és azt lejátszásához!

    ![Az Umbraco konfigurálása – az első Umbraco-alkalmazás az Azure App Service-ben](./media/app-service-web-get-started-dotnet-portal/umbraco-config.png)
    
## <a name="next-steps"></a>Következő lépések
* [Az ASP.NET web app tooAzure App Service szolgáltatásban a Visual Studio használatával telepítheti](app-service-web-get-started-dotnet.md) -megtudhatja, hogyan toocreate egy új Azure-webalkalmazásban a Visual Studio eszközből hello egyikét sem tartalmazza alkalmazássablonok.
* [A kód tooAzure App Service telepítése](web-sites-deploy.md)-megtudhatja, hogyan szabályozza a toodeploy FTP vagy a forrás a tárházak találhatók.
* [Funkció tooyour első webalkalmazás hozzáadása](app-service-web-get-started-2.md) -érvénybe az Azure alkalmazás toohello következő szint. Hitelesítheti felhasználóit. Igény szerint méretezheti. Beállíthat a teljesítménnyel kapcsolatos riasztásokat. Mindezt csupán néhány kattintással.
