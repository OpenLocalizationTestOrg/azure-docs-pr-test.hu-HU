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
# <a name="create-and-connect-tooa-mysql-database-in-azure"></a><span data-ttu-id="cb777-103">Hozzon létre, és csatlakozzon az Azure-ban tooa MySQL-adatbázis</span><span class="sxs-lookup"><span data-stu-id="cb777-103">Create and connect tooa MySQL database in Azure</span></span>
<span data-ttu-id="cb777-104">Az oktatóanyag bemutatja, hogyan toocreate a MySQL-adatbázis használati ideje a hello [Azure-portálon](https://portal.azure.com) (szolgáltató [ClearDB](http://www.cleardb.com/)) és hogyan tooconnect tooit egy php-ből webes alkalmazás fusson az [Azure App Service](app-service/app-service-value-prop-what-is.md).</span><span class="sxs-lookup"><span data-stu-id="cb777-104">This tutorial shows you how toocreate a MySQL database in hello [Azure portal](https://portal.azure.com) (provider is [ClearDB](http://www.cleardb.com/)) and how tooconnect tooit from a PHP web app running in [Azure App Service](app-service/app-service-value-prop-what-is.md).</span></span>

> [!NOTE]
> <span data-ttu-id="cb777-105">MySQL-adatbázis részeként is létrehozhat egy [piactér app sablon](app-service-web/app-service-web-create-web-app-from-marketplace.md).</span><span class="sxs-lookup"><span data-stu-id="cb777-105">You can also create a MySQL database as part of a [Marketplace app template](app-service-web/app-service-web-create-web-app-from-marketplace.md).</span></span>
>
>

## <a name="create-a-mysql-database-in-azure-portal"></a><span data-ttu-id="cb777-106">MySQL-adatbázis létrehozása az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="cb777-106">Create a MySQL database in Azure portal</span></span>
<span data-ttu-id="cb777-107">hello Azure-portálon a MySQL-adatbázis toocreate hello a következő:</span><span class="sxs-lookup"><span data-stu-id="cb777-107">toocreate a MySQL database in hello Azure portal, do hello following:</span></span>

1. <span data-ttu-id="cb777-108">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="cb777-108">Log in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="cb777-109">Hello bal oldali menüben kattintson **új** > **adatok + tárolás** > **MySQL-adatbázis**.</span><span class="sxs-lookup"><span data-stu-id="cb777-109">From hello left menu, click **New** > **Data + Storage** > **MySQL Database**.</span></span>

    ![Azure - start MySQL-adatbázis létrehozása](./media/store-php-create-mysql-database/create-db-1-start.png)
3. <span data-ttu-id="cb777-111">A hello új MySQL-adatbázis [panel](azure-portal-overview.md), az alábbiak szerint konfigurálja az új MySQL-adatbázis (*panel*: a portálon megjelenő vízszintesen):</span><span class="sxs-lookup"><span data-stu-id="cb777-111">In hello New MySQL Database [blade](azure-portal-overview.md), configure your new MySQL database as follows (*blade*: a portal page that opens horizontally):</span></span>

   * <span data-ttu-id="cb777-112">**Adatbázis neve**: Adjon meg egy egyedi módon azonosítható nevet</span><span class="sxs-lookup"><span data-stu-id="cb777-112">**Database Name**: Type a uniquely identifiable name</span></span>
   * <span data-ttu-id="cb777-113">**Előfizetés**: hello előfizetés toouse kiválasztása</span><span class="sxs-lookup"><span data-stu-id="cb777-113">**Subscription**: Choose hello subscription toouse</span></span>
   * <span data-ttu-id="cb777-114">**Adatbázis-típus**: válasszon **megosztott** alacsony költségű vagy szabad rétege számára vagy **dedikált** tooget dedikált erőforrások.</span><span class="sxs-lookup"><span data-stu-id="cb777-114">**Database Type**: Select **Shared** for low-cost or free tiers, or **Dedicated** tooget dedicated resources.</span></span>
   * <span data-ttu-id="cb777-115">**Erőforráscsoport**: hello MySQL adatbázis tooan meglévő hozzáadása [erőforráscsoport](azure-resource-manager/resource-group-overview.md) vagy helyezze egy újat.</span><span class="sxs-lookup"><span data-stu-id="cb777-115">**Resource group**: Add hello MySQL database tooan existing [resource group](azure-resource-manager/resource-group-overview.md) or put it in a new one.</span></span> <span data-ttu-id="cb777-116">Ugyanabban a csoportban könnyen kezelhető együtt hello erőforrásokhoz.</span><span class="sxs-lookup"><span data-stu-id="cb777-116">Resources in hello same group can be easily managed together.</span></span>
   * <span data-ttu-id="cb777-117">**Hely**: válassza ki a hely Bezárás tooyou.</span><span class="sxs-lookup"><span data-stu-id="cb777-117">**Location**: Select a location close tooyou.</span></span> <span data-ttu-id="cb777-118">Meglévő erőforráscsoport tooan hozzáadásakor Ön zárolt toohello erőforrás csoport helyét.</span><span class="sxs-lookup"><span data-stu-id="cb777-118">When adding tooan existing resource group, you're locked toohello resource group's location.</span></span>
   * <span data-ttu-id="cb777-119">**IP-címek**: kattintson a **Tarifacsomagot**, válassza ki a tarifacsomagot lehetőséget (**Merkúr** érvényben lévő korlát miatt szabad), és kattintson a **kiválasztása**.</span><span class="sxs-lookup"><span data-stu-id="cb777-119">**Pricing Tier**: Click **Pricing Tier**, then select a pricing option (**Mercury** tier is free), and then click **Select**.</span></span>
   * <span data-ttu-id="cb777-120">**Jogi feltételek**: kattintson a **jogi feltételeket**, tekintse át a hello vásárlás részletei, és kattintson a **beszerzési**.</span><span class="sxs-lookup"><span data-stu-id="cb777-120">**Legal Terms**: Click **Legal Terms**, review hello purchase details, and click **Purchase**.</span></span>
   * <span data-ttu-id="cb777-121">**PIN-kód toodashboard**: akkor válassza, ha azt szeretné, hogy tooaccess azt közvetlenül hello irányítópultról.</span><span class="sxs-lookup"><span data-stu-id="cb777-121">**Pin toodashboard**: Select if you want tooaccess it directly from hello dashboard.</span></span> <span data-ttu-id="cb777-122">Ez akkor hasznos, ha nem ismeri még portálnavigációjával.</span><span class="sxs-lookup"><span data-stu-id="cb777-122">This is especially helpful if you aren't familiar with portal navigation yet.</span></span>

     <span data-ttu-id="cb777-123">a következő képernyőkép hello példája csak a MySQL adatbázis konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="cb777-123">hello following screenshot is just an example of how you can configure your MySQL database.</span></span>  
     ![MySQL-adatbázis létrehozása az Azure - konfigurálása](./media/store-php-create-mysql-database/create-db-2-configure.png)
4. <span data-ttu-id="cb777-125">Ha elkészült konfigurálásával, kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="cb777-125">When you're done configuring, click **Create**.</span></span>

    ![MySQL-adatbázis létrehozása az Azure-ban – létrehozása](./media/store-php-create-mysql-database/create-db-3-create.png)

    <span data-ttu-id="cb777-127">Megjelenik egy előugró értesítések azzal kapcsolatban, akkor tudni, hogy az üzembe helyezés megkezdődött.</span><span class="sxs-lookup"><span data-stu-id="cb777-127">You will see a pop-up notification letting you know that deployment has started.</span></span>

    ![Azure - MySQL-adatbázis létrehozása folyamatban](./media/store-php-create-mysql-database/create-db-4-started-status.png)

    <span data-ttu-id="cb777-129">Elérhetővé válik egy másik előugró központi telepítés sikeres befejezése után.</span><span class="sxs-lookup"><span data-stu-id="cb777-129">You will get another pop-up once deployment has succeeded.</span></span> <span data-ttu-id="cb777-130">automatikusan hello portálon is ekkor megnyílik a MySQL-adatbázis paneljén.</span><span class="sxs-lookup"><span data-stu-id="cb777-130">hello portal will also open your MySQL database blade automatically.</span></span>

<a name="connect"></a>

## <a name="connect-tooyour-mysql-database"></a><span data-ttu-id="cb777-131">Csatlakozás tooyour MySQL-adatbázis</span><span class="sxs-lookup"><span data-stu-id="cb777-131">Connect tooyour MySQL database</span></span>
<span data-ttu-id="cb777-132">Kattintson az új MySQL-adatbázis-toosee hello kapcsolódási információt **tulajdonságok** a webalkalmazás panelen.</span><span class="sxs-lookup"><span data-stu-id="cb777-132">toosee hello connection information for your new MySQL database, just click **Properties** in your web app's blade.</span></span>

![MySQL-adatbázis létrehozása az Azure - MySQL adatbázis paneljén](./media/store-php-create-mysql-database/create-db-5-finished-db-blade.png)

<span data-ttu-id="cb777-134">Használhatja a kapcsolat adatait bármely webalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="cb777-134">You can now use that connection information in any web app.</span></span> <span data-ttu-id="cb777-135">A példa bemutatja, hogyan érhető el egy egyszerű PHP-alkalmazás toouse hello kapcsolat adatainak [Itt](https://github.com/WindowsAzure/azure-sdk-for-php-samples/tree/master/tasklist-mysql).</span><span class="sxs-lookup"><span data-stu-id="cb777-135">A sample that shows how toouse hello connection information from a simple PHP app is available [here](https://github.com/WindowsAzure/azure-sdk-for-php-samples/tree/master/tasklist-mysql).</span></span>

## <a name="connect-a-laravel-web-app-from-hello-php-get-started-tutorial"></a><span data-ttu-id="cb777-136">(A hello PHP get bemutató) Laravel webes alkalmazás csatlakoztatása</span><span class="sxs-lookup"><span data-stu-id="cb777-136">Connect a Laravel web app (from hello PHP get started tutorial)</span></span>
<span data-ttu-id="cb777-137">Tegyük fel, hogy csak végzett hello oktatóanyag [létrehozása, konfigurálása, és a PHP webes alkalmazások tooAzure telepítése](app-service-web/app-service-web-php-get-started.md) , és egy [Laravel](https://www.laravel.com/) az Azure-ban futó webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="cb777-137">Suppose you just finished hello tutorial [Create, configure, and deploy a PHP web app tooAzure](app-service-web/app-service-web-php-get-started.md) and have a [Laravel](https://www.laravel.com/) web app running in Azure.</span></span> <span data-ttu-id="cb777-138">Egyszerűen hozzáadhatja adatbázis képességek tooyour Laravel alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="cb777-138">You can easily add database capabilities tooyour Laravel app.</span></span> <span data-ttu-id="cb777-139">Kövesse az alábbi hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="cb777-139">Just follow hello steps below:</span></span>

> [!NOTE]
> <span data-ttu-id="cb777-140">hello következő lépések azt feltételezik, hogy végzett a hello oktatóanyag [létrehozása, konfigurálása, és a PHP webes alkalmazások tooAzure telepítése](app-service-web/app-service-web-php-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="cb777-140">hello following steps assume that you have finished hello tutorial [Create, configure, and deploy a PHP web app tooAzure](app-service-web/app-service-web-php-get-started.md).</span></span>
>
>

1. <span data-ttu-id="cb777-141">Hello Laravel alkalmazás konfigurálása a helyi fejlesztési környezet toopoint toohello MySQL-adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="cb777-141">Configure hello Laravel app in your local development environment toopoint toohello MySQL database.</span></span> <span data-ttu-id="cb777-142">toodo a, nyissa meg `.env` a Laravel alkalmazás gyökérkönyvtár, és hello MySQL adatbázis-beállítások konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="cb777-142">toodo this, open `.env` from your Laravel app's root directory and configure hello MySQL database options.</span></span>

        DB_CONNECTION=mysql
        DB_HOST=<HOSTNAME_from_properties_blade>
        DB_PORT=<PORT_from_properties_blade>
        DB_DATABASE=<see_note_below>
        DB_USERNAME=<USERNAME_from_properties_blade>
        DB_PASSWORD=<PASSWORD_from_properties_blade>

   > [!NOTE]
   > <span data-ttu-id="cb777-143">A hello **tulajdonságok** panelen, előfordulhat, hogy a MySQL-adatbázis neve hello, vagy előfordulhat, hogy nem egy hello szerepelnek hello **ADATBÁZISNÉV** mező.</span><span class="sxs-lookup"><span data-stu-id="cb777-143">In hello **Properties** blade, hello name of your MySQL database may or may not be hello one shown in hello **DATABASE NAME** field.</span></span> <span data-ttu-id="cb777-144">Jobb toocheck hello adatbázis paramétere a hello **KAPCSOLATI karakterlánc** mező.</span><span class="sxs-lookup"><span data-stu-id="cb777-144">It's better toocheck hello Database parameter in hello **CONNECTION STRING** field.</span></span>    
   >
   > ![Azure - MySQL-adatbázis létrehozása folyamatban](./media/store-php-create-mysql-database/connect-db-1-database-name.png)
   >
   >
2. <span data-ttu-id="cb777-146">hello MySQL hozzáférést most már rendelkezik leggyorsabb módon tooverify toouse [Laravel tartozó alapértelmezett hitelesítési állványok](https://laravel.com/docs/5.2/authentication#authentication-quickstart).</span><span class="sxs-lookup"><span data-stu-id="cb777-146">hello quickest way tooverify that you have MySQL access now is toouse [Laravel's default authentication scaffolding](https://laravel.com/docs/5.2/authentication#authentication-quickstart).</span></span>
   <span data-ttu-id="cb777-147">Hello parancssori terminált, a futtassa a következő parancsokat a Laravel app gyökérkönyvtárából hello:</span><span class="sxs-lookup"><span data-stu-id="cb777-147">In hello command-line terminal, run hello following commands from your Laravel app's root directory:</span></span>

         php artisan migrate
         php artisan make:auth

    <span data-ttu-id="cb777-148">hello első parancs Azure hello az előre definiált áttelepítések alapján hoz létre hello táblák `database/migrations` könyvtárra, és a parancs második hello támasztók hello alapvető nézetek és felhasználói regisztrációjának és hitelesítésének az útvonalakat.</span><span class="sxs-lookup"><span data-stu-id="cb777-148">hello first command creates hello tables in Azure based on predefined migrations in hello `database/migrations` directory, and hello second  command scaffolds hello basic views and routes for user registration and authentication.</span></span>
3. <span data-ttu-id="cb777-149">Hello fejlesztési kiszolgáló futtatása:</span><span class="sxs-lookup"><span data-stu-id="cb777-149">Run hello development server now:</span></span>

        php artisan serve
4. <span data-ttu-id="cb777-150">Hello böngészőben nyissa meg a toohttp://localhost:8000, és regisztrálni egy új felhasználó látható:</span><span class="sxs-lookup"><span data-stu-id="cb777-150">In hello browser, navigate toohttp://localhost:8000 and register a new user as shown:</span></span>

    ![Csatlakozás az Azure-ban tooMySQL adatbázis - felhasználó regisztrálása](./media/store-php-create-mysql-database/connect-db-2-development-server.png)

    <span data-ttu-id="cb777-152">Hajtsa végre a hello felhasználói felület Rákérdezés teljes hello regisztrációs.</span><span class="sxs-lookup"><span data-stu-id="cb777-152">Follow hello UI prompt complete hello registration.</span></span> <span data-ttu-id="cb777-153">Miután befejeződött a regisztráció, akkor vannak, naplózva lesznek.</span><span class="sxs-lookup"><span data-stu-id="cb777-153">Once registration completes, you will be logged in.</span></span>

    ![Csatlakozás az Azure-ban tooMySQL adatbázis - felhasználó regisztrálása](./media/store-php-create-mysql-database/connect-db-3-registered-user.png)

    <span data-ttu-id="cb777-155">Az alkalmazás hello MySQL-adatbázis az Azure-ban most fejleszt.</span><span class="sxs-lookup"><span data-stu-id="cb777-155">You are now developing your app against hello MySQL database in Azure.</span></span>
5. <span data-ttu-id="cb777-156">Most, egyszerűen tooreplicate a `.env` beállítások tooyour Azure-webalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="cb777-156">Now, you just need tooreplicate your `.env` settings tooyour Azure web app.</span></span> <span data-ttu-id="cb777-157">Futtassa a következő Azure parancssori felület parancsait hello:</span><span class="sxs-lookup"><span data-stu-id="cb777-157">Run hello following Azure CLI commands:</span></span>

        azure site appsetting add DB_CONNECTION=mysql
        azure site appsetting add DB_HOST=<HOSTNAME_from_properties_blade>
        azure site appsetting add DB_PORT=<PORT_from_properties_blade>
        azure site appsetting add DB_DATABASE=<Database_param_from_CONNECTION_INFO_from_properties_blade>
        azure site appsetting add DB_USERNAME=<USERNAME_from_properties_blade>
        azure site appsetting add DB_PASSWORD=<PASSWORD_from_properties_blade>

6. <span data-ttu-id="cb777-158">Ezt követően véglegesítse és küldje le tooAzure hello helyi végrehajtott módosítások korábbi futtatása során `php artisan make:auth`.</span><span class="sxs-lookup"><span data-stu-id="cb777-158">Next, commit and push tooAzure hello local changes made earlier while running `php artisan make:auth`.</span></span>

        git add .
        git commit -m "scaffold auth views and routes"
        git push azure master
7. <span data-ttu-id="cb777-159">Keresse meg az Azure-webalkalmazásban toohello.</span><span class="sxs-lookup"><span data-stu-id="cb777-159">Browse toohello Azure web app.</span></span>

        azure site browse
8. <span data-ttu-id="cb777-160">Jelentkezzen be a korábban létrehozott hello felhasználó hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="cb777-160">Log in using hello user credentials you created earlier.</span></span>

    ![Csatlakozás az Azure-ban tooMySQL adatbázis - tooAzure webalkalmazás tallózása](./media/store-php-create-mysql-database/connect-db-4-browse-azure-webapp.png)

    <span data-ttu-id="cb777-162">Miután jelentkezik be, meg kell jelennie hello rövid utáni bejelentkezési képernyőt.</span><span class="sxs-lookup"><span data-stu-id="cb777-162">After you log in, you should see hello friendly post-login screen.</span></span>

    ![Csatlakozás az Azure - bejelentkezett tooMySQL adatbázis](./media/store-php-create-mysql-database/connect-db-5-logged-in.png)

    <span data-ttu-id="cb777-164">Gratulálunk, a PHP webalkalmazást az Azure-ban most próbál adatokat a MySQL-adatbázisból.</span><span class="sxs-lookup"><span data-stu-id="cb777-164">Congratulations, your PHP web app in Azure is now accessing data from your MySQL database.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cb777-165">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cb777-165">Next steps</span></span>
<span data-ttu-id="cb777-166">További információkért lásd: hello [PHP fejlesztői központ](/develop/php/).</span><span class="sxs-lookup"><span data-stu-id="cb777-166">For more information, see hello [PHP Developer Center](/develop/php/).</span></span>
