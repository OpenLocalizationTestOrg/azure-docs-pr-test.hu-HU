---
title: "aaaCreate, és csatlakozzon az Azure-ban tooa MySQL-adatbázis"
description: "Ismerje meg, hogyan toouse hello Azure portál toocreate MySQL-adatbázis, és ezután csatlakoztassa tooit egy PHP webalkalmazást az Azure-ban."
documentationcenter: php
services: app-service\web
author: cephalin
manager: erikre
editor: 
tags: mysql
ms.assetid: 55465a9a-7e65-4fd9-8a65-dd83ee41f3e5
ms.service: multiple
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm;cephalin
ms.openlocfilehash: 3abc02f8887432625666afd847e9dc0c0a4db2e4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-connect-tooa-mysql-database-in-azure"></a>Hozzon létre, és csatlakozzon az Azure-ban tooa MySQL-adatbázis
Az oktatóanyag bemutatja, hogyan toocreate a MySQL-adatbázis használati ideje a hello [Azure-portálon](https://portal.azure.com) (szolgáltató [ClearDB](http://www.cleardb.com/)) és hogyan tooconnect tooit egy php-ből webes alkalmazás fusson az [Azure App Service](app-service/app-service-value-prop-what-is.md).

> [!NOTE]
> MySQL-adatbázis részeként is létrehozhat egy [piactér app sablon](app-service-web/app-service-web-create-web-app-from-marketplace.md).
>
>

## <a name="create-a-mysql-database-in-azure-portal"></a>MySQL-adatbázis létrehozása az Azure-portálon
hello Azure-portálon a MySQL-adatbázis toocreate hello a következő:

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Hello bal oldali menüben kattintson **új** > **adatok + tárolás** > **MySQL-adatbázis**.

    ![Azure - start MySQL-adatbázis létrehozása](./media/store-php-create-mysql-database/create-db-1-start.png)
3. A hello új MySQL-adatbázis [panel](azure-portal-overview.md), az alábbiak szerint konfigurálja az új MySQL-adatbázis (*panel*: a portálon megjelenő vízszintesen):

   * **Adatbázis neve**: Adjon meg egy egyedi módon azonosítható nevet
   * **Előfizetés**: hello előfizetés toouse kiválasztása
   * **Adatbázis-típus**: válasszon **megosztott** alacsony költségű vagy szabad rétege számára vagy **dedikált** tooget dedikált erőforrások.
   * **Erőforráscsoport**: hello MySQL adatbázis tooan meglévő hozzáadása [erőforráscsoport](azure-resource-manager/resource-group-overview.md) vagy helyezze egy újat. Ugyanabban a csoportban könnyen kezelhető együtt hello erőforrásokhoz.
   * **Hely**: válassza ki a hely Bezárás tooyou. Meglévő erőforráscsoport tooan hozzáadásakor Ön zárolt toohello erőforrás csoport helyét.
   * **IP-címek**: kattintson a **Tarifacsomagot**, válassza ki a tarifacsomagot lehetőséget (**Merkúr** érvényben lévő korlát miatt szabad), és kattintson a **kiválasztása**.
   * **Jogi feltételek**: kattintson a **jogi feltételeket**, tekintse át a hello vásárlás részletei, és kattintson a **beszerzési**.
   * **PIN-kód toodashboard**: akkor válassza, ha azt szeretné, hogy tooaccess azt közvetlenül hello irányítópultról. Ez akkor hasznos, ha nem ismeri még portálnavigációjával.

     a következő képernyőkép hello példája csak a MySQL adatbázis konfigurálásához.  
     ![MySQL-adatbázis létrehozása az Azure - konfigurálása](./media/store-php-create-mysql-database/create-db-2-configure.png)
4. Ha elkészült konfigurálásával, kattintson a **létrehozása**.

    ![MySQL-adatbázis létrehozása az Azure-ban – létrehozása](./media/store-php-create-mysql-database/create-db-3-create.png)

    Megjelenik egy előugró értesítések azzal kapcsolatban, akkor tudni, hogy az üzembe helyezés megkezdődött.

    ![Azure - MySQL-adatbázis létrehozása folyamatban](./media/store-php-create-mysql-database/create-db-4-started-status.png)

    Elérhetővé válik egy másik előugró központi telepítés sikeres befejezése után. automatikusan hello portálon is ekkor megnyílik a MySQL-adatbázis paneljén.

<a name="connect"></a>

## <a name="connect-tooyour-mysql-database"></a>Csatlakozás tooyour MySQL-adatbázis
Kattintson az új MySQL-adatbázis-toosee hello kapcsolódási információt **tulajdonságok** a webalkalmazás panelen.

![MySQL-adatbázis létrehozása az Azure - MySQL adatbázis paneljén](./media/store-php-create-mysql-database/create-db-5-finished-db-blade.png)

Használhatja a kapcsolat adatait bármely webalkalmazásban. A példa bemutatja, hogyan érhető el egy egyszerű PHP-alkalmazás toouse hello kapcsolat adatainak [Itt](https://github.com/WindowsAzure/azure-sdk-for-php-samples/tree/master/tasklist-mysql).

## <a name="connect-a-laravel-web-app-from-hello-php-get-started-tutorial"></a>(A hello PHP get bemutató) Laravel webes alkalmazás csatlakoztatása
Tegyük fel, hogy csak végzett hello oktatóanyag [létrehozása, konfigurálása, és a PHP webes alkalmazások tooAzure telepítése](app-service-web/app-service-web-php-get-started.md) , és egy [Laravel](https://www.laravel.com/) az Azure-ban futó webalkalmazás. Egyszerűen hozzáadhatja adatbázis képességek tooyour Laravel alkalmazást. Kövesse az alábbi hello lépéseket:

> [!NOTE]
> hello következő lépések azt feltételezik, hogy végzett a hello oktatóanyag [létrehozása, konfigurálása, és a PHP webes alkalmazások tooAzure telepítése](app-service-web/app-service-web-php-get-started.md).
>
>

1. Hello Laravel alkalmazás konfigurálása a helyi fejlesztési környezet toopoint toohello MySQL-adatbázisban. toodo a, nyissa meg `.env` a Laravel alkalmazás gyökérkönyvtár, és hello MySQL adatbázis-beállítások konfigurálása.

        DB_CONNECTION=mysql
        DB_HOST=<HOSTNAME_from_properties_blade>
        DB_PORT=<PORT_from_properties_blade>
        DB_DATABASE=<see_note_below>
        DB_USERNAME=<USERNAME_from_properties_blade>
        DB_PASSWORD=<PASSWORD_from_properties_blade>

   > [!NOTE]
   > A hello **tulajdonságok** panelen, előfordulhat, hogy a MySQL-adatbázis neve hello, vagy előfordulhat, hogy nem egy hello szerepelnek hello **ADATBÁZISNÉV** mező. Jobb toocheck hello adatbázis paramétere a hello **KAPCSOLATI karakterlánc** mező.    
   >
   > ![Azure - MySQL-adatbázis létrehozása folyamatban](./media/store-php-create-mysql-database/connect-db-1-database-name.png)
   >
   >
2. hello MySQL hozzáférést most már rendelkezik leggyorsabb módon tooverify toouse [Laravel tartozó alapértelmezett hitelesítési állványok](https://laravel.com/docs/5.2/authentication#authentication-quickstart).
   Hello parancssori terminált, a futtassa a következő parancsokat a Laravel app gyökérkönyvtárából hello:

         php artisan migrate
         php artisan make:auth

    hello első parancs Azure hello az előre definiált áttelepítések alapján hoz létre hello táblák `database/migrations` könyvtárra, és a parancs második hello támasztók hello alapvető nézetek és felhasználói regisztrációjának és hitelesítésének az útvonalakat.
3. Hello fejlesztési kiszolgáló futtatása:

        php artisan serve
4. Hello böngészőben nyissa meg a toohttp://localhost:8000, és regisztrálni egy új felhasználó látható:

    ![Csatlakozás az Azure-ban tooMySQL adatbázis - felhasználó regisztrálása](./media/store-php-create-mysql-database/connect-db-2-development-server.png)

    Hajtsa végre a hello felhasználói felület Rákérdezés teljes hello regisztrációs. Miután befejeződött a regisztráció, akkor vannak, naplózva lesznek.

    ![Csatlakozás az Azure-ban tooMySQL adatbázis - felhasználó regisztrálása](./media/store-php-create-mysql-database/connect-db-3-registered-user.png)

    Az alkalmazás hello MySQL-adatbázis az Azure-ban most fejleszt.
5. Most, egyszerűen tooreplicate a `.env` beállítások tooyour Azure-webalkalmazásban. Futtassa a következő Azure parancssori felület parancsait hello:

        azure site appsetting add DB_CONNECTION=mysql
        azure site appsetting add DB_HOST=<HOSTNAME_from_properties_blade>
        azure site appsetting add DB_PORT=<PORT_from_properties_blade>
        azure site appsetting add DB_DATABASE=<Database_param_from_CONNECTION_INFO_from_properties_blade>
        azure site appsetting add DB_USERNAME=<USERNAME_from_properties_blade>
        azure site appsetting add DB_PASSWORD=<PASSWORD_from_properties_blade>

6. Ezt követően véglegesítse és küldje le tooAzure hello helyi végrehajtott módosítások korábbi futtatása során `php artisan make:auth`.

        git add .
        git commit -m "scaffold auth views and routes"
        git push azure master
7. Keresse meg az Azure-webalkalmazásban toohello.

        azure site browse
8. Jelentkezzen be a korábban létrehozott hello felhasználó hitelesítő adataival.

    ![Csatlakozás az Azure-ban tooMySQL adatbázis - tooAzure webalkalmazás tallózása](./media/store-php-create-mysql-database/connect-db-4-browse-azure-webapp.png)

    Miután jelentkezik be, meg kell jelennie hello rövid utáni bejelentkezési képernyőt.

    ![Csatlakozás az Azure - bejelentkezett tooMySQL adatbázis](./media/store-php-create-mysql-database/connect-db-5-logged-in.png)

    Gratulálunk, a PHP webalkalmazást az Azure-ban most próbál adatokat a MySQL-adatbázisból.

## <a name="next-steps"></a>Következő lépések
További információkért lásd: hello [PHP fejlesztői központ](/develop/php/).
