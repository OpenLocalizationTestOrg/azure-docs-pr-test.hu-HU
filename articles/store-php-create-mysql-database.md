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
# <a name="create-and-connect-to-a-mysql-database-in-azure"></a><span data-ttu-id="e2e96-103">MySQL-adatbázis létrehozása és kapcsolódás hozzá az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="e2e96-103">Create and connect to a MySQL database in Azure</span></span>
<span data-ttu-id="e2e96-104">Ez az oktatóanyag bemutatja, hogyan a MySQL-adatbázis létrehozása a [Azure-portálon](https://portal.azure.com) (szolgáltató [ClearDB](http://www.cleardb.com/)) és egy PHP webalkalmazást futtatja a csatlakoztatása [Azure App Service](app-service/app-service-value-prop-what-is.md).</span><span class="sxs-lookup"><span data-stu-id="e2e96-104">This tutorial shows you how to create a MySQL database in the [Azure portal](https://portal.azure.com) (provider is [ClearDB](http://www.cleardb.com/)) and how to connect to it from a PHP web app running in [Azure App Service](app-service/app-service-value-prop-what-is.md).</span></span>

> [!NOTE]
> <span data-ttu-id="e2e96-105">MySQL-adatbázis részeként is létrehozhat egy [piactér app sablon](app-service-web/app-service-web-create-web-app-from-marketplace.md).</span><span class="sxs-lookup"><span data-stu-id="e2e96-105">You can also create a MySQL database as part of a [Marketplace app template](app-service-web/app-service-web-create-web-app-from-marketplace.md).</span></span>
>
>

## <a name="create-a-mysql-database-in-azure-portal"></a><span data-ttu-id="e2e96-106">MySQL-adatbázis létrehozása az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="e2e96-106">Create a MySQL database in Azure portal</span></span>
<span data-ttu-id="e2e96-107">MySQL-adatbázis létrehozása az Azure portálon, tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="e2e96-107">To create a MySQL database in the Azure portal, do the following:</span></span>

1. <span data-ttu-id="e2e96-108">Jelentkezzen be az [Azure portálra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="e2e96-108">Log in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="e2e96-109">Kattintson a bal oldali menü **új** > **adatok + tárolás** > **MySQL-adatbázis**.</span><span class="sxs-lookup"><span data-stu-id="e2e96-109">From the left menu, click **New** > **Data + Storage** > **MySQL Database**.</span></span>

    ![Azure - start MySQL-adatbázis létrehozása](./media/store-php-create-mysql-database/create-db-1-start.png)
3. <span data-ttu-id="e2e96-111">Az új MySQL-adatbázisban [panel](azure-portal-overview.md), az alábbiak szerint konfigurálja az új MySQL-adatbázis (*panel*: a portálon megjelenő vízszintesen):</span><span class="sxs-lookup"><span data-stu-id="e2e96-111">In the New MySQL Database [blade](azure-portal-overview.md), configure your new MySQL database as follows (*blade*: a portal page that opens horizontally):</span></span>

   * <span data-ttu-id="e2e96-112">**Adatbázis neve**: Adjon meg egy egyedi módon azonosítható nevet</span><span class="sxs-lookup"><span data-stu-id="e2e96-112">**Database Name**: Type a uniquely identifiable name</span></span>
   * <span data-ttu-id="e2e96-113">**Előfizetés**: válassza ki az előfizetés használatára</span><span class="sxs-lookup"><span data-stu-id="e2e96-113">**Subscription**: Choose the subscription to use</span></span>
   * <span data-ttu-id="e2e96-114">**Adatbázis-típus**: válasszon **megosztott** alacsony költségű vagy szabad rétege számára vagy **dedikált** dedikált erőforrások eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="e2e96-114">**Database Type**: Select **Shared** for low-cost or free tiers, or **Dedicated** to get dedicated resources.</span></span>
   * <span data-ttu-id="e2e96-115">**Erőforráscsoport**: a MySQL-adatbázis hozzáadása egy meglévő [erőforráscsoport](azure-resource-manager/resource-group-overview.md) vagy helyezze egy újat.</span><span class="sxs-lookup"><span data-stu-id="e2e96-115">**Resource group**: Add the MySQL database to an existing [resource group](azure-resource-manager/resource-group-overview.md) or put it in a new one.</span></span> <span data-ttu-id="e2e96-116">Erőforrások ugyanabban a csoportban is könnyen kezelhető együtt.</span><span class="sxs-lookup"><span data-stu-id="e2e96-116">Resources in the same group can be easily managed together.</span></span>
   * <span data-ttu-id="e2e96-117">**Hely**: Válasszon Önhöz legközelebb eső helyet.</span><span class="sxs-lookup"><span data-stu-id="e2e96-117">**Location**: Select a location close to you.</span></span> <span data-ttu-id="e2e96-118">Amikor ad hozzá egy meglévő erőforráscsoportot, akkor az erőforráscsoport helye van kapcsolva.</span><span class="sxs-lookup"><span data-stu-id="e2e96-118">When adding to an existing resource group, you're locked to the resource group's location.</span></span>
   * <span data-ttu-id="e2e96-119">**IP-címek**: kattintson a **Tarifacsomagot**, válassza ki a tarifacsomagot lehetőséget (**Merkúr** érvényben lévő korlát miatt szabad), és kattintson a **kiválasztása**.</span><span class="sxs-lookup"><span data-stu-id="e2e96-119">**Pricing Tier**: Click **Pricing Tier**, then select a pricing option (**Mercury** tier is free), and then click **Select**.</span></span>
   * <span data-ttu-id="e2e96-120">**Jogi feltételek**: kattintson a **jogi feltételeket**, tekintse át a vásárlás részletei, és kattintson a **beszerzési**.</span><span class="sxs-lookup"><span data-stu-id="e2e96-120">**Legal Terms**: Click **Legal Terms**, review the purchase details, and click **Purchase**.</span></span>
   * <span data-ttu-id="e2e96-121">**Rögzítés az irányítópulton**: akkor válassza, ha közvetlenül az irányítópultról eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="e2e96-121">**Pin to dashboard**: Select if you want to access it directly from the dashboard.</span></span> <span data-ttu-id="e2e96-122">Ez akkor hasznos, ha nem ismeri még portálnavigációjával.</span><span class="sxs-lookup"><span data-stu-id="e2e96-122">This is especially helpful if you aren't familiar with portal navigation yet.</span></span>

     <span data-ttu-id="e2e96-123">Az alábbi képernyőfelvételen értéke csak egy példa a MySQL adatbázis konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="e2e96-123">The following screenshot is just an example of how you can configure your MySQL database.</span></span>  
     ![MySQL-adatbázis létrehozása az Azure - konfigurálása](./media/store-php-create-mysql-database/create-db-2-configure.png)
4. <span data-ttu-id="e2e96-125">Ha elkészült konfigurálásával, kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="e2e96-125">When you're done configuring, click **Create**.</span></span>

    ![MySQL-adatbázis létrehozása az Azure-ban – létrehozása](./media/store-php-create-mysql-database/create-db-3-create.png)

    <span data-ttu-id="e2e96-127">Megjelenik egy előugró értesítések azzal kapcsolatban, akkor tudni, hogy az üzembe helyezés megkezdődött.</span><span class="sxs-lookup"><span data-stu-id="e2e96-127">You will see a pop-up notification letting you know that deployment has started.</span></span>

    ![Azure - MySQL-adatbázis létrehozása folyamatban](./media/store-php-create-mysql-database/create-db-4-started-status.png)

    <span data-ttu-id="e2e96-129">Elérhetővé válik egy másik előugró központi telepítés sikeres befejezése után.</span><span class="sxs-lookup"><span data-stu-id="e2e96-129">You will get another pop-up once deployment has succeeded.</span></span> <span data-ttu-id="e2e96-130">A portál is megnyílik a MySQL adatbázis panel automatikusan.</span><span class="sxs-lookup"><span data-stu-id="e2e96-130">The portal will also open your MySQL database blade automatically.</span></span>

<a name="connect"></a>

## <a name="connect-to-your-mysql-database"></a><span data-ttu-id="e2e96-131">Csatlakozás a MySQL-adatbázis</span><span class="sxs-lookup"><span data-stu-id="e2e96-131">Connect to your MySQL database</span></span>
<span data-ttu-id="e2e96-132">Új MySQL-adatbázis kapcsolati adatainak megtekintéséhez kattintson **tulajdonságok** a webalkalmazás panelen.</span><span class="sxs-lookup"><span data-stu-id="e2e96-132">To see the connection information for your new MySQL database, just click **Properties** in your web app's blade.</span></span>

![MySQL-adatbázis létrehozása az Azure - MySQL adatbázis paneljén](./media/store-php-create-mysql-database/create-db-5-finished-db-blade.png)

<span data-ttu-id="e2e96-134">Használhatja a kapcsolat adatait bármely webalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="e2e96-134">You can now use that connection information in any web app.</span></span> <span data-ttu-id="e2e96-135">Egy minta bemutatja, hogyan szeretné használni a kapcsolat egy egyszerű PHP-alkalmazásokban érhető el [Itt](https://github.com/WindowsAzure/azure-sdk-for-php-samples/tree/master/tasklist-mysql).</span><span class="sxs-lookup"><span data-stu-id="e2e96-135">A sample that shows how to use the connection information from a simple PHP app is available [here](https://github.com/WindowsAzure/azure-sdk-for-php-samples/tree/master/tasklist-mysql).</span></span>

## <a name="connect-a-laravel-web-app-from-the-php-get-started-tutorial"></a><span data-ttu-id="e2e96-136">(A a PHP get bemutató) Laravel webes alkalmazás csatlakoztatása</span><span class="sxs-lookup"><span data-stu-id="e2e96-136">Connect a Laravel web app (from the PHP get started tutorial)</span></span>
<span data-ttu-id="e2e96-137">Tegyük fel, hogy az imént befejeződött az oktatóanyag [létrehozása, konfigurálása, és a PHP-webalkalmazás telepítése az Azure](app-service-web/app-service-web-php-get-started.md) , és egy [Laravel](https://www.laravel.com/) az Azure-ban futó webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="e2e96-137">Suppose you just finished the tutorial [Create, configure, and deploy a PHP web app to Azure](app-service-web/app-service-web-php-get-started.md) and have a [Laravel](https://www.laravel.com/) web app running in Azure.</span></span> <span data-ttu-id="e2e96-138">Könnyen adatbázis-képességeket adhat a Laravel app.</span><span class="sxs-lookup"><span data-stu-id="e2e96-138">You can easily add database capabilities to your Laravel app.</span></span> <span data-ttu-id="e2e96-139">Kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="e2e96-139">Just follow the steps below:</span></span>

> [!NOTE]
> <span data-ttu-id="e2e96-140">A következő lépések azt feltételezik, hogy az oktatóanyag befejezése [létrehozása, konfigurálása, és a PHP-webalkalmazás telepítése az Azure](app-service-web/app-service-web-php-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="e2e96-140">The following steps assume that you have finished the tutorial [Create, configure, and deploy a PHP web app to Azure](app-service-web/app-service-web-php-get-started.md).</span></span>
>
>

1. <span data-ttu-id="e2e96-141">Laravel alkalmazás beállítása az a helyi fejlesztési környezetet a MySQL-adatbázis mutasson.</span><span class="sxs-lookup"><span data-stu-id="e2e96-141">Configure the Laravel app in your local development environment to point to the MySQL database.</span></span> <span data-ttu-id="e2e96-142">Ehhez nyissa meg a `.env` a Laravel alkalmazás gyökérkönyvtár, és a MySQL adatbázis-beállítások konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="e2e96-142">To do this, open `.env` from your Laravel app's root directory and configure the MySQL database options.</span></span>

        DB_CONNECTION=mysql
        DB_HOST=<HOSTNAME_from_properties_blade>
        DB_PORT=<PORT_from_properties_blade>
        DB_DATABASE=<see_note_below>
        DB_USERNAME=<USERNAME_from_properties_blade>
        DB_PASSWORD=<PASSWORD_from_properties_blade>

   > [!NOTE]
   > <span data-ttu-id="e2e96-143">Az a **tulajdonságok** panelen, előfordulhat, hogy a MySQL-adatbázis neve, vagy előfordulhat, hogy nem az egyik szerepelnek a **ADATBÁZISNÉV** mező.</span><span class="sxs-lookup"><span data-stu-id="e2e96-143">In the **Properties** blade, the name of your MySQL database may or may not be the one shown in the **DATABASE NAME** field.</span></span> <span data-ttu-id="e2e96-144">Érdemes ellenőrizze, hogy az adatbázis-paraméter szerepel a **KAPCSOLATI karakterlánc** mező.</span><span class="sxs-lookup"><span data-stu-id="e2e96-144">It's better to check the Database parameter in the **CONNECTION STRING** field.</span></span>    
   >
   > ![Azure - MySQL-adatbázis létrehozása folyamatban](./media/store-php-create-mysql-database/connect-db-1-database-name.png)
   >
   >
2. <span data-ttu-id="e2e96-146">A leggyorsabb módja MySQL hozzáférést most már rendelkezik [Laravel tartozó alapértelmezett hitelesítési állványok](https://laravel.com/docs/5.2/authentication#authentication-quickstart).</span><span class="sxs-lookup"><span data-stu-id="e2e96-146">The quickest way to verify that you have MySQL access now is to use [Laravel's default authentication scaffolding](https://laravel.com/docs/5.2/authentication#authentication-quickstart).</span></span>
   <span data-ttu-id="e2e96-147">A kívánt parancssori terminált futtassa az alábbi parancsokat a Laravel app gyökérkönyvtárából:</span><span class="sxs-lookup"><span data-stu-id="e2e96-147">In the command-line terminal, run the following commands from your Laravel app's root directory:</span></span>

         php artisan migrate
         php artisan make:auth

    <span data-ttu-id="e2e96-148">Az első parancs létrehozza a táblát alapján előre definiált áttelepítése az Azure-ban a `database/migrations` könyvtárra, és a második parancs scaffolds alapvető nézet és felhasználói regisztrációjának és hitelesítésének az útvonalakat.</span><span class="sxs-lookup"><span data-stu-id="e2e96-148">The first command creates the tables in Azure based on predefined migrations in the `database/migrations` directory, and the second  command scaffolds the basic views and routes for user registration and authentication.</span></span>
3. <span data-ttu-id="e2e96-149">A fejlesztési kiszolgáló futtatása:</span><span class="sxs-lookup"><span data-stu-id="e2e96-149">Run the development server now:</span></span>

        php artisan serve
4. <span data-ttu-id="e2e96-150">A böngészőben navigáljon a http://localhost:8000, és egy új felhasználó regisztrálása látható módon:</span><span class="sxs-lookup"><span data-stu-id="e2e96-150">In the browser, navigate to http://localhost:8000 and register a new user as shown:</span></span>

    ![Csatlakozzon az Azure-beli MySQL database – a felhasználó regisztrálása](./media/store-php-create-mysql-database/connect-db-2-development-server.png)

    <span data-ttu-id="e2e96-152">A teljes felhasználói felület felszólítást követve a regisztrációt.</span><span class="sxs-lookup"><span data-stu-id="e2e96-152">Follow the UI prompt complete the registration.</span></span> <span data-ttu-id="e2e96-153">Miután befejeződött a regisztráció, akkor vannak, naplózva lesznek.</span><span class="sxs-lookup"><span data-stu-id="e2e96-153">Once registration completes, you will be logged in.</span></span>

    ![Csatlakozzon az Azure-beli MySQL database – a felhasználó regisztrálása](./media/store-php-create-mysql-database/connect-db-3-registered-user.png)

    <span data-ttu-id="e2e96-155">Az alkalmazás a MySQL-adatbázis az Azure-ban most fejleszt.</span><span class="sxs-lookup"><span data-stu-id="e2e96-155">You are now developing your app against the MySQL database in Azure.</span></span>
5. <span data-ttu-id="e2e96-156">Mostantól ugyanúgy kell replikálni a `.env` beállításait az Azure-webalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="e2e96-156">Now, you just need to replicate your `.env` settings to your Azure web app.</span></span> <span data-ttu-id="e2e96-157">A következő Azure CLI-parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="e2e96-157">Run the following Azure CLI commands:</span></span>

        azure site appsetting add DB_CONNECTION=mysql
        azure site appsetting add DB_HOST=<HOSTNAME_from_properties_blade>
        azure site appsetting add DB_PORT=<PORT_from_properties_blade>
        azure site appsetting add DB_DATABASE=<Database_param_from_CONNECTION_INFO_from_properties_blade>
        azure site appsetting add DB_USERNAME=<USERNAME_from_properties_blade>
        azure site appsetting add DB_PASSWORD=<PASSWORD_from_properties_blade>

6. <span data-ttu-id="e2e96-158">Ezt követően véglegesítse, és az Azure küldje le a helyi módosítások korábbi futtatása során `php artisan make:auth`.</span><span class="sxs-lookup"><span data-stu-id="e2e96-158">Next, commit and push to Azure the local changes made earlier while running `php artisan make:auth`.</span></span>

        git add .
        git commit -m "scaffold auth views and routes"
        git push azure master
7. <span data-ttu-id="e2e96-159">Tallózással keresse meg az Azure-webalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="e2e96-159">Browse to the Azure web app.</span></span>

        azure site browse
8. <span data-ttu-id="e2e96-160">Jelentkezzen be a korábban létrehozott felhasználói hitelesítő adatok használatával.</span><span class="sxs-lookup"><span data-stu-id="e2e96-160">Log in using the user credentials you created earlier.</span></span>

    ![Csatlakozás az Azure web apphoz navigáljon az Azure - beli MySQL database](./media/store-php-create-mysql-database/connect-db-4-browse-azure-webapp.png)

    <span data-ttu-id="e2e96-162">Miután jelentkezik be, meg kell jelennie a rövid utáni bejelentkezési képernyőt.</span><span class="sxs-lookup"><span data-stu-id="e2e96-162">After you log in, you should see the friendly post-login screen.</span></span>

    ![Csatlakozás az Azure - bejelentkezett MySQL-adatbázis](./media/store-php-create-mysql-database/connect-db-5-logged-in.png)

    <span data-ttu-id="e2e96-164">Gratulálunk, a PHP webalkalmazást az Azure-ban most próbál adatokat a MySQL-adatbázisból.</span><span class="sxs-lookup"><span data-stu-id="e2e96-164">Congratulations, your PHP web app in Azure is now accessing data from your MySQL database.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e2e96-165">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e2e96-165">Next steps</span></span>
<span data-ttu-id="e2e96-166">További információkért lásd: a [PHP fejlesztői központ](/develop/php/).</span><span class="sxs-lookup"><span data-stu-id="e2e96-166">For more information, see the [PHP Developer Center](/develop/php/).</span></span>
