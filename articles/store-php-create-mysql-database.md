---
title: "MySQL-adatbázis létrehozása és kapcsolódás hozzá az Azure-ban"
description: "Ismerje meg, hogyan használható az Azure-portálon MySQL-adatbázis létrehozása, és csatlakoztassa azt egy PHP webalkalmazást az Azure-ban a."
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
ms.openlocfilehash: 66f4b7a5f8eb3f6f125c9420b40caffca3d43dd6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-and-connect-to-a-mysql-database-in-azure"></a>MySQL-adatbázis létrehozása és kapcsolódás hozzá az Azure-ban
Ez az oktatóanyag bemutatja, hogyan a MySQL-adatbázis létrehozása a [Azure-portálon](https://portal.azure.com) (szolgáltató [ClearDB](http://www.cleardb.com/)) és egy PHP webalkalmazást futtatja a csatlakoztatása [Azure App Service](app-service/app-service-value-prop-what-is.md).

> [!NOTE]
> MySQL-adatbázis részeként is létrehozhat egy [piactér app sablon](app-service-web/app-service-web-create-web-app-from-marketplace.md).
>
>

## <a name="create-a-mysql-database-in-azure-portal"></a>MySQL-adatbázis létrehozása az Azure-portálon
MySQL-adatbázis létrehozása az Azure portálon, tegye a következőket:

1. Jelentkezzen be az [Azure portálra](https://portal.azure.com).
2. Kattintson a bal oldali menü **új** > **adatok + tárolás** > **MySQL-adatbázis**.

    ![Azure - start MySQL-adatbázis létrehozása](./media/store-php-create-mysql-database/create-db-1-start.png)
3. Az új MySQL-adatbázisban [panel](azure-portal-overview.md), az alábbiak szerint konfigurálja az új MySQL-adatbázis (*panel*: a portálon megjelenő vízszintesen):

   * **Adatbázis neve**: Adjon meg egy egyedi módon azonosítható nevet
   * **Előfizetés**: válassza ki az előfizetés használatára
   * **Adatbázis-típus**: válasszon **megosztott** alacsony költségű vagy szabad rétege számára vagy **dedikált** dedikált erőforrások eléréséhez.
   * **Erőforráscsoport**: a MySQL-adatbázis hozzáadása egy meglévő [erőforráscsoport](azure-resource-manager/resource-group-overview.md) vagy helyezze egy újat. Erőforrások ugyanabban a csoportban is könnyen kezelhető együtt.
   * **Hely**: Válasszon Önhöz legközelebb eső helyet. Amikor ad hozzá egy meglévő erőforráscsoportot, akkor az erőforráscsoport helye van kapcsolva.
   * **IP-címek**: kattintson a **Tarifacsomagot**, válassza ki a tarifacsomagot lehetőséget (**Merkúr** érvényben lévő korlát miatt szabad), és kattintson a **kiválasztása**.
   * **Jogi feltételek**: kattintson a **jogi feltételeket**, tekintse át a vásárlás részletei, és kattintson a **beszerzési**.
   * **Rögzítés az irányítópulton**: akkor válassza, ha közvetlenül az irányítópultról eléréséhez. Ez akkor hasznos, ha nem ismeri még portálnavigációjával.

     Az alábbi képernyőfelvételen értéke csak egy példa a MySQL adatbázis konfigurálásához.  
     ![MySQL-adatbázis létrehozása az Azure - konfigurálása](./media/store-php-create-mysql-database/create-db-2-configure.png)
4. Ha elkészült konfigurálásával, kattintson a **létrehozása**.

    ![MySQL-adatbázis létrehozása az Azure-ban – létrehozása](./media/store-php-create-mysql-database/create-db-3-create.png)

    Megjelenik egy előugró értesítések azzal kapcsolatban, akkor tudni, hogy az üzembe helyezés megkezdődött.

    ![Azure - MySQL-adatbázis létrehozása folyamatban](./media/store-php-create-mysql-database/create-db-4-started-status.png)

    Elérhetővé válik egy másik előugró központi telepítés sikeres befejezése után. A portál is megnyílik a MySQL adatbázis panel automatikusan.

<a name="connect"></a>

## <a name="connect-to-your-mysql-database"></a>Csatlakozás a MySQL-adatbázis
Új MySQL-adatbázis kapcsolati adatainak megtekintéséhez kattintson **tulajdonságok** a webalkalmazás panelen.

![MySQL-adatbázis létrehozása az Azure - MySQL adatbázis paneljén](./media/store-php-create-mysql-database/create-db-5-finished-db-blade.png)

Használhatja a kapcsolat adatait bármely webalkalmazásban. Egy minta bemutatja, hogyan szeretné használni a kapcsolat egy egyszerű PHP-alkalmazásokban érhető el [Itt](https://github.com/WindowsAzure/azure-sdk-for-php-samples/tree/master/tasklist-mysql).

## <a name="connect-a-laravel-web-app-from-the-php-get-started-tutorial"></a>(A a PHP get bemutató) Laravel webes alkalmazás csatlakoztatása
Tegyük fel, hogy az imént befejeződött az oktatóanyag [létrehozása, konfigurálása, és a PHP-webalkalmazás telepítése az Azure](app-service-web/app-service-web-php-get-started.md) , és egy [Laravel](https://www.laravel.com/) az Azure-ban futó webalkalmazás. Könnyen adatbázis-képességeket adhat a Laravel app. Kövesse az alábbi lépéseket:

> [!NOTE]
> A következő lépések azt feltételezik, hogy az oktatóanyag befejezése [létrehozása, konfigurálása, és a PHP-webalkalmazás telepítése az Azure](app-service-web/app-service-web-php-get-started.md).
>
>

1. Laravel alkalmazás beállítása az a helyi fejlesztési környezetet a MySQL-adatbázis mutasson. Ehhez nyissa meg a `.env` a Laravel alkalmazás gyökérkönyvtár, és a MySQL adatbázis-beállítások konfigurálása.

        DB_CONNECTION=mysql
        DB_HOST=<HOSTNAME_from_properties_blade>
        DB_PORT=<PORT_from_properties_blade>
        DB_DATABASE=<see_note_below>
        DB_USERNAME=<USERNAME_from_properties_blade>
        DB_PASSWORD=<PASSWORD_from_properties_blade>

   > [!NOTE]
   > Az a **tulajdonságok** panelen, előfordulhat, hogy a MySQL-adatbázis neve, vagy előfordulhat, hogy nem az egyik szerepelnek a **ADATBÁZISNÉV** mező. Érdemes ellenőrizze, hogy az adatbázis-paraméter szerepel a **KAPCSOLATI karakterlánc** mező.    
   >
   > ![Azure - MySQL-adatbázis létrehozása folyamatban](./media/store-php-create-mysql-database/connect-db-1-database-name.png)
   >
   >
2. A leggyorsabb módja MySQL hozzáférést most már rendelkezik [Laravel tartozó alapértelmezett hitelesítési állványok](https://laravel.com/docs/5.2/authentication#authentication-quickstart).
   A kívánt parancssori terminált futtassa az alábbi parancsokat a Laravel app gyökérkönyvtárából:

         php artisan migrate
         php artisan make:auth

    Az első parancs létrehozza a táblát alapján előre definiált áttelepítése az Azure-ban a `database/migrations` könyvtárra, és a második parancs scaffolds alapvető nézet és felhasználói regisztrációjának és hitelesítésének az útvonalakat.
3. A fejlesztési kiszolgáló futtatása:

        php artisan serve
4. A böngészőben navigáljon a http://localhost:8000, és egy új felhasználó regisztrálása látható módon:

    ![Csatlakozzon az Azure-beli MySQL database – a felhasználó regisztrálása](./media/store-php-create-mysql-database/connect-db-2-development-server.png)

    A teljes felhasználói felület felszólítást követve a regisztrációt. Miután befejeződött a regisztráció, akkor vannak, naplózva lesznek.

    ![Csatlakozzon az Azure-beli MySQL database – a felhasználó regisztrálása](./media/store-php-create-mysql-database/connect-db-3-registered-user.png)

    Az alkalmazás a MySQL-adatbázis az Azure-ban most fejleszt.
5. Mostantól ugyanúgy kell replikálni a `.env` beállításait az Azure-webalkalmazásban. A következő Azure CLI-parancsok futtatásával:

        azure site appsetting add DB_CONNECTION=mysql
        azure site appsetting add DB_HOST=<HOSTNAME_from_properties_blade>
        azure site appsetting add DB_PORT=<PORT_from_properties_blade>
        azure site appsetting add DB_DATABASE=<Database_param_from_CONNECTION_INFO_from_properties_blade>
        azure site appsetting add DB_USERNAME=<USERNAME_from_properties_blade>
        azure site appsetting add DB_PASSWORD=<PASSWORD_from_properties_blade>

6. Ezt követően véglegesítse, és az Azure küldje le a helyi módosítások korábbi futtatása során `php artisan make:auth`.

        git add .
        git commit -m "scaffold auth views and routes"
        git push azure master
7. Tallózással keresse meg az Azure-webalkalmazásban.

        azure site browse
8. Jelentkezzen be a korábban létrehozott felhasználói hitelesítő adatok használatával.

    ![Csatlakozás az Azure web apphoz navigáljon az Azure - beli MySQL database](./media/store-php-create-mysql-database/connect-db-4-browse-azure-webapp.png)

    Miután jelentkezik be, meg kell jelennie a rövid utáni bejelentkezési képernyőt.

    ![Csatlakozás az Azure - bejelentkezett MySQL-adatbázis](./media/store-php-create-mysql-database/connect-db-5-logged-in.png)

    Gratulálunk, a PHP webalkalmazást az Azure-ban most próbál adatokat a MySQL-adatbázisból.

## <a name="next-steps"></a>Következő lépések
További információkért lásd: a [PHP fejlesztői központ](/develop/php/).
