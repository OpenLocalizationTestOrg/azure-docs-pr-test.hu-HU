---
title: "a PHP-SQL aaaCreate webalkalmazás, és tooAzure App Service telepítése Git használatával"
description: "Ez az oktatóanyag bemutatja, hogyan toocreate a PHP webes alkalmazást, amely tárolja az adatokat az Azure SQL Database, és használja a Git telepítési tooAzure App Service."
services: app-service\web, sql-database
documentationcenter: php
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 6b090bf6-31d8-4b74-81eb-050ef95929ca
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: aaacb2fe0787bbcdafa72871912e8d08792be29d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-php-sql-web-app-and-deploy-tooazure-app-service-using-git"></a><span data-ttu-id="b8434-103">PHP-SQL-webalkalmazás létrehozása és központi telepítése tooAzure App Service Git használatával</span><span class="sxs-lookup"><span data-stu-id="b8434-103">Create a PHP-SQL web app and deploy tooAzure App Service using Git</span></span>
<span data-ttu-id="b8434-104">Az oktatóanyag bemutatja, hogyan toocreate egy PHP webalkalmazást az app [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) tooAzure SQL-adatbázis csatlakozik, és hogyan toodeploy azt a Git használatával.</span><span class="sxs-lookup"><span data-stu-id="b8434-104">This tutorial shows you how toocreate a PHP web app in [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) that connects tooAzure SQL Database and how toodeploy it using Git.</span></span> <span data-ttu-id="b8434-105">Ez az oktatóanyag feltételezi, hogy rendelkezik [PHP][install-php], [SQL Server Express][install-SQLExpress], hello [Microsoft SQL Server php Drivers ](http://www.microsoft.com/download/en/details.aspx?id=20098), és [Git] [ install-git] telepítve a számítógépre.</span><span class="sxs-lookup"><span data-stu-id="b8434-105">This tutorial assumes you have [PHP][install-php], [SQL Server Express][install-SQLExpress], hello [Microsoft Drivers for SQL Server for PHP](http://www.microsoft.com/download/en/details.aspx?id=20098), and [Git][install-git] installed on your computer.</span></span> <span data-ttu-id="b8434-106">Ez az útmutató befejezése után fog egy PHP-SQL Azure-ban futó webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="b8434-106">Upon completing this guide, you will have a PHP-SQL web app running in Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="b8434-107">Telepítse, és hello használata php-hez tartozó SQL Server, SQL Server Express és a Microsoft Drivers hello konfigurálása [Microsoft Webplatform-telepítő](http://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="b8434-107">You can install and configure PHP, SQL Server Express, and hello Microsoft Drivers for SQL Server for PHP using hello [Microsoft Web Platform Installer](http://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> 
> 

<span data-ttu-id="b8434-108">Az oktatóanyagban érintett témák köre:</span><span class="sxs-lookup"><span data-stu-id="b8434-108">You will learn:</span></span>

* <span data-ttu-id="b8434-109">Hogyan toocreate az Azure web app és egy SQL-adatbázist hello [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715).</span><span class="sxs-lookup"><span data-stu-id="b8434-109">How toocreate an Azure web app and a SQL Database using hello [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715).</span></span> <span data-ttu-id="b8434-110">A PHP alapértelmezés szerint engedélyezve van az App Service Web Apps, mert semmi különleges-e szükséges toorun a PHP-kódot.</span><span class="sxs-lookup"><span data-stu-id="b8434-110">Because PHP is enabled in App Service Web Apps by default, nothing special is required toorun your PHP code.</span></span>
* <span data-ttu-id="b8434-111">Hogyan toopublish, és újból közzéteszi a Git használatával alkalmazás tooAzure.</span><span class="sxs-lookup"><span data-stu-id="b8434-111">How toopublish and re-publish your application tooAzure using Git.</span></span>

<span data-ttu-id="b8434-112">Az oktatóanyag utasításait követve egy egyszerű regisztrációs webalkalmazásából PHP fog létrehozni.</span><span class="sxs-lookup"><span data-stu-id="b8434-112">By following this tutorial, you will build a simple registration web application in PHP.</span></span> <span data-ttu-id="b8434-113">hello alkalmazás üzemel az Azure-webhely.</span><span class="sxs-lookup"><span data-stu-id="b8434-113">hello application will be hosted in an Azure Website.</span></span> <span data-ttu-id="b8434-114">A képernyőfelvétel a hello befejeződött alkalmazás alatt van:</span><span class="sxs-lookup"><span data-stu-id="b8434-114">A screenshot of hello completed application is below:</span></span>

![Az Azure PHP-webhely](./media/web-sites-php-sql-database-deploy-use-git/running_app_3.png)

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

> [!NOTE]
> <span data-ttu-id="b8434-116">Ha azt szeretné, hogy az az Azure-fiók regisztrálása előtt az Azure App Service lépései tooget, nyissa meg túl[App Service kipróbálása](https://azure.microsoft.com/try/app-service/), ahol azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben.</span><span class="sxs-lookup"><span data-stu-id="b8434-116">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="b8434-117">Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.</span><span class="sxs-lookup"><span data-stu-id="b8434-117">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="create-an-azure-web-app-and-set-up-git-publishing"></a><span data-ttu-id="b8434-118">Azure-webalkalmazás létrehozása és beállítása a Git-közzététel</span><span class="sxs-lookup"><span data-stu-id="b8434-118">Create an Azure web app and set up Git publishing</span></span>
<span data-ttu-id="b8434-119">Kövesse ezeket a lépéseket toocreate Azure-webalkalmazás és az SQL-adatbázisban:</span><span class="sxs-lookup"><span data-stu-id="b8434-119">Follow these steps toocreate an Azure web app and a SQL Database:</span></span>

1. <span data-ttu-id="b8434-120">Jelentkezzen be toohello [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="b8434-120">Log in toohello [Azure Portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="b8434-121">Hello kattintva nyissa meg hello Azure piactér **új** hello irányítópult balra hello felül ikonra, kattintson a **kijelölése az összes** következő tooMarketplace és kiválasztásával **Web + mobil**.</span><span class="sxs-lookup"><span data-stu-id="b8434-121">Open hello Azure Marketplace by clicking hello **New** icon on hello top left of hello dashboard, click on **Select All** next tooMarketplace and selecting **Web + Mobile**.</span></span>
3. <span data-ttu-id="b8434-122">Hello piactér, válassza ki **Web + mobil**.</span><span class="sxs-lookup"><span data-stu-id="b8434-122">In hello Marketplace, select **Web + Mobile**.</span></span>
4. <span data-ttu-id="b8434-123">Kattintson a hello **webes alkalmazás + SQL** ikonra.</span><span class="sxs-lookup"><span data-stu-id="b8434-123">Click hello **Web app + SQL** icon.</span></span>
5. <span data-ttu-id="b8434-124">Hello webes alkalmazás + SQL app hello leírása elolvasása, után válassza ki a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="b8434-124">After reading hello description of hello Web app + SQL app, select **Create**.</span></span>
6. <span data-ttu-id="b8434-125">Kattintson az egyes komponenseihez (**erőforráscsoport**, **webalkalmazás**, **adatbázis**, és **előfizetés**) és adja meg vagy válassza ki a szükséges hello értékeket mezők:</span><span class="sxs-lookup"><span data-stu-id="b8434-125">Click on each part (**Resource Group**, **Web App**, **Database**, and **Subscription**) and enter or select values for hello required fields:</span></span>
   
   * <span data-ttu-id="b8434-126">Adjon meg egy URL-címet a választott</span><span class="sxs-lookup"><span data-stu-id="b8434-126">Enter a URL name of your choice</span></span>    
   * <span data-ttu-id="b8434-127">Adatbázis-kiszolgáló hitelesítő adatok beállítása</span><span class="sxs-lookup"><span data-stu-id="b8434-127">Configure database server credentials</span></span>
   * <span data-ttu-id="b8434-128">Válassza ki a hello régió legközelebbi tooyou</span><span class="sxs-lookup"><span data-stu-id="b8434-128">Select hello region closest tooyou</span></span>
     
     ![az alkalmazás konfigurálása](./media/web-sites-php-sql-database-deploy-use-git/configure-db-settings.png)
7. <span data-ttu-id="b8434-130">Ha befejezte a hello webalkalmazás meghatározása, kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="b8434-130">When finished defining hello web app, click **Create**.</span></span>
   
    <span data-ttu-id="b8434-131">Hello webalkalmazás létrehozásakor hello **értesítések** gomb villogjon, egy zöld **sikeres** és erőforrás-csoport panel megnyitása tooshow hello mindkét hello web app és hello SQL-adatbázis hello csoportban.</span><span class="sxs-lookup"><span data-stu-id="b8434-131">When hello web app has been created, hello **Notifications** button will flash a green **SUCCESS** and hello resource group blade open tooshow both hello web app and hello SQL database in hello group.</span></span>
8. <span data-ttu-id="b8434-132">Kattintson a hello erőforrás csoport panel tooopen hello webalkalmazása panelén hello webes alkalmazás ikonra.</span><span class="sxs-lookup"><span data-stu-id="b8434-132">Click hello web app's icon in hello resource group blade tooopen hello web app's blade.</span></span>
   
    ![webes alkalmazás erőforráscsoport](./media/web-sites-php-sql-database-deploy-use-git/resource-group-blade.png)
9. <span data-ttu-id="b8434-134">A **beállítások** kattintson **folyamatos üzembe helyezés** > **kötelező beállítások konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="b8434-134">In **Settings** click **Continuous deployment** > **Configure required settings**.</span></span> <span data-ttu-id="b8434-135">Válassza ki **helyi Git-tárház** kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="b8434-135">Select **Local Git Repository** and click **OK**.</span></span>
   
    ![hol található a Forráskód](./media/web-sites-php-sql-database-deploy-use-git/setup-local-git.png)
   
    <span data-ttu-id="b8434-137">Ha nem állított be egy Git-tárház előtt, meg kell adnia egy felhasználónevet és jelszót.</span><span class="sxs-lookup"><span data-stu-id="b8434-137">If you have not set up a Git repository before, you must provide a user name and password.</span></span> <span data-ttu-id="b8434-138">toodo, kattintson **beállítások** > **üzembe helyezési hitelesítő adatok** a hello webalkalmazása panelén.</span><span class="sxs-lookup"><span data-stu-id="b8434-138">toodo this, click **Settings** > **Deployment credentials** in hello web app's blade.</span></span>
   
    ![](./media/web-sites-php-sql-database-deploy-use-git/deployment-credentials.png)
10. <span data-ttu-id="b8434-139">A **beállítások** kattintson a **tulajdonságok** toosee hello Git távoli URL-cím toouse toodeploy a PHP-alkalmazásokban később szüksége.</span><span class="sxs-lookup"><span data-stu-id="b8434-139">In **Settings** click on **Properties** toosee hello Git remote URL you need toouse toodeploy your PHP app later.</span></span>

## <a name="get-sql-database-connection-information"></a><span data-ttu-id="b8434-140">SQL adatbázis-kapcsolat adatainak lekérése</span><span class="sxs-lookup"><span data-stu-id="b8434-140">Get SQL Database connection information</span></span>
<span data-ttu-id="b8434-141">tooconnect toohello SQL Database-példányt, amely csatolva tooyour webalkalmazás, a fog kell hello hello adatbázis létrehozásakor megadott kapcsolati információt.</span><span class="sxs-lookup"><span data-stu-id="b8434-141">tooconnect toohello SQL Database instance that is linked tooyour web app, your will need hello connection information, which you specified when you created hello database.</span></span> <span data-ttu-id="b8434-142">tooget hello SQL adatbázis-kapcsolat adatainak, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="b8434-142">tooget hello SQL Database connection information, follow these steps:</span></span>

1. <span data-ttu-id="b8434-143">Hello erőforráscsoport panelen kattintson hello SQL adatbázis ikonjára.</span><span class="sxs-lookup"><span data-stu-id="b8434-143">Back in hello resource group's blade, click hello SQL database's icon.</span></span>
2. <span data-ttu-id="b8434-144">Hello SQL adatbázis paneljén kattintson **beállítások** > **tulajdonságok**, majd kattintson a **adatbázis-kapcsolati karakterláncok megjelenítése**.</span><span class="sxs-lookup"><span data-stu-id="b8434-144">In hello SQL database's blade, click **Settings** > **Properties**, then click **Show database connection strings**.</span></span> 
   
    ![Adatbázis-tulajdonságok megtekintése](./media/web-sites-php-sql-database-deploy-use-git/view-database-properties.png)
3. <span data-ttu-id="b8434-146">A hello **PHP** szakasz hello eredményül kapott párbeszédpanel, jegyezze fel a hello értékeinek `Server`, `SQL Database`, és `User Name`.</span><span class="sxs-lookup"><span data-stu-id="b8434-146">From hello **PHP** section of hello resulting dialog, make note of hello values for `Server`, `SQL Database`, and `User Name`.</span></span> <span data-ttu-id="b8434-147">Ha később közzététele a PHP webes alkalmazások tooAzure App Service ezeket az értékeket fogja használni.</span><span class="sxs-lookup"><span data-stu-id="b8434-147">You will use these values later when publishing your PHP web app tooAzure App Service.</span></span>

## <a name="build-and-test-your-application-locally"></a><span data-ttu-id="b8434-148">Az alkalmazás helyi összeállítása és tesztelése</span><span class="sxs-lookup"><span data-stu-id="b8434-148">Build and test your application locally</span></span>
<span data-ttu-id="b8434-149">hello regisztrációs alkalmazása, amely lehetővé teszi a név és e-mail cím megadásával az esemény tooregister egyszerű PHP-alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="b8434-149">hello Registration application is a simple PHP application that allows you tooregister for an event by providing your name and email address.</span></span> <span data-ttu-id="b8434-150">Előző igénylők kapcsolatos információkat a táblázatban jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="b8434-150">Information about previous registrants is displayed in a table.</span></span> <span data-ttu-id="b8434-151">Regisztrációs adatai egy SQL Database-példányt.</span><span class="sxs-lookup"><span data-stu-id="b8434-151">Registration information is stored in a SQL Database instance.</span></span> <span data-ttu-id="b8434-152">hello alkalmazás két fájlt (másolás/beillesztés kód alatt érhető el) áll:</span><span class="sxs-lookup"><span data-stu-id="b8434-152">hello application consists of two files (copy/paste code available below):</span></span>

* <span data-ttu-id="b8434-153">**index.php**: regisztráció és a bejegyzés adatokat tartalmazó táblát űrlap jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="b8434-153">**index.php**: Displays a form for registration and a table containing registrant information.</span></span>
* <span data-ttu-id="b8434-154">**createtable.php**: hello SQL-adatbázis táblája hello alkalmazáshoz hoz létre.</span><span class="sxs-lookup"><span data-stu-id="b8434-154">**createtable.php**: Creates hello SQL Database table for hello application.</span></span> <span data-ttu-id="b8434-155">Ezt a fájlt csak egyszer használható.</span><span class="sxs-lookup"><span data-stu-id="b8434-155">This file will only be used once.</span></span>

<span data-ttu-id="b8434-156">toorun hello alkalmazás helyi, kövesse a hello lépéseket.</span><span class="sxs-lookup"><span data-stu-id="b8434-156">toorun hello application locally, follow hello steps below.</span></span> <span data-ttu-id="b8434-157">Vegye figyelembe, hogy ezek a lépések feltételezik, PHP rendelkezik, és az SQL Server Express állítsa be a helyi számítógépen, és engedélyezte hello [OEM-bővítmény SQL Server][pdo-sqlsrv].</span><span class="sxs-lookup"><span data-stu-id="b8434-157">Note that these steps assume you have PHP and SQL Server Express set up on your local machine, and that you have enabled hello [PDO extension for SQL Server][pdo-sqlsrv].</span></span>

1. <span data-ttu-id="b8434-158">Hozzon létre egy SQL Server-adatbázis nevű `registration`.</span><span class="sxs-lookup"><span data-stu-id="b8434-158">Create a SQL Server database called `registration`.</span></span> <span data-ttu-id="b8434-159">Ehhez a hello `sqlcmd` parancssor a következő parancsokkal:</span><span class="sxs-lookup"><span data-stu-id="b8434-159">You can do this from hello `sqlcmd` command prompt with these commands:</span></span>
   
        >sqlcmd -S localhost\sqlexpress -U <local user name> -P <local password>
        1> create database registration
        2> GO    
2. <span data-ttu-id="b8434-160">Az alkalmazás gyökérkönyvtárában, hozzon létre két fájlokat - egy `createtable.php` és egy `index.php`.</span><span class="sxs-lookup"><span data-stu-id="b8434-160">In your application root directory, create two files in it - one called `createtable.php` and one called `index.php`.</span></span>
3. <span data-ttu-id="b8434-161">Nyissa meg hello `createtable.php` fájlt egy szövegszerkesztőben, vagy IDE, és adja hozzá az alábbi hello kódot.</span><span class="sxs-lookup"><span data-stu-id="b8434-161">Open hello `createtable.php` file in a text editor or IDE and add hello code below.</span></span> <span data-ttu-id="b8434-162">Ez a kód lesz használt toocreate hello `registration_tbl` hello táblájában `registration` adatbázis.</span><span class="sxs-lookup"><span data-stu-id="b8434-162">This code will be used toocreate hello `registration_tbl` table in hello `registration` database.</span></span>
   
        <?php
        // DB connection info
        $host = "localhost\sqlexpress";
        $user = "user name";
        $pwd = "password";
        $db = "registration";
        try{
            $conn = new PDO( "sqlsrv:Server= $host ; Database = $db ", $user, $pwd);
            $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );
            $sql = "CREATE TABLE registration_tbl(
            id INT NOT NULL IDENTITY(1,1) 
            PRIMARY KEY(id),
            name VARCHAR(30),
            email VARCHAR(30),
            date DATE)";
            $conn->query($sql);
        }
        catch(Exception $e){
            die(print_r($e));
        }
        echo "<h3>Table created.</h3>";
        ?>
   
    <span data-ttu-id="b8434-163">Vegye figyelembe, hogy szüksége lesz a tooupdate hello értékeinek <code>$user</code> és <code>$pwd</code> a helyi SQL Server felhasználói nevét és jelszavát.</span><span class="sxs-lookup"><span data-stu-id="b8434-163">Note that you will need tooupdate hello values for <code>$user</code> and <code>$pwd</code> with your local SQL Server user name and password.</span></span>
4. <span data-ttu-id="b8434-164">A terminálban hello annak gyökérkönyvtárában hello alkalmazás típusa hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="b8434-164">In a terminal at hello root directory of hello application type hello following command:</span></span>
   
        php -S localhost:8000
5. <span data-ttu-id="b8434-165">Nyisson meg egy webböngészőt, és keresse meg a túl**http://localhost:8000/createtable.php**.</span><span class="sxs-lookup"><span data-stu-id="b8434-165">Open a web browser and browse too**http://localhost:8000/createtable.php**.</span></span> <span data-ttu-id="b8434-166">Ezzel létrehoz hello `registration_tbl` hello adatbázis táblájában.</span><span class="sxs-lookup"><span data-stu-id="b8434-166">This will create hello `registration_tbl` table in hello database.</span></span>
6. <span data-ttu-id="b8434-167">Nyissa meg hello **index.php** fájlt egy szövegszerkesztőben, vagy IDE, és adja hozzá a hello alapvető HTML- és CSS kód hello lap (hello PHP kód megkapja a későbbi lépésekben).</span><span class="sxs-lookup"><span data-stu-id="b8434-167">Open hello **index.php** file in a text editor or IDE and add hello basic HTML and CSS code for hello page (hello PHP code will be added in later steps).</span></span>
   
        <html>
        <head>
        <Title>Registration Form</Title>
        <style type="text/css">
            body { background-color: #fff; border-top: solid 10px #000;
                color: #333; font-size: .85em; margin: 20; padding: 20;
                font-family: "Segoe UI", Verdana, Helvetica, Sans-Serif;
            }
            h1, h2, h3,{ color: #000; margin-bottom: 0; padding-bottom: 0; }
            h1 { font-size: 2em; }
            h2 { font-size: 1.75em; }
            h3 { font-size: 1.2em; }
            table { margin-top: 0.75em; }
            th { font-size: 1.2em; text-align: left; border: none; padding-left: 0; }
            td { padding: 0.25em 2em 0.25em 0em; border: 0 none; }
        </style>
        </head>
        <body>
        <h1>Register here!</h1>
        <p>Fill in your name and email address, then click <strong>Submit</strong> tooregister.</p>
        <form method="post" action="index.php" enctype="multipart/form-data" >
              Name  <input type="text" name="name" id="name"/></br>
              Email <input type="text" name="email" id="email"/></br>
              <input type="submit" name="submit" value="Submit" />
        </form>
        <?php
   
        ?>
        </body>
        </html>
7. <span data-ttu-id="b8434-168">Hello PHP címkék, belül adja hozzá a PHP kódot toohello adatbázis csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="b8434-168">Within hello PHP tags, add PHP code for connecting toohello database.</span></span>
   
        // DB connection info
        $host = "localhost\sqlexpress";
        $user = "user name";
        $pwd = "password";
        $db = "registration";
        // Connect toodatabase.
        try {
            $conn = new PDO( "sqlsrv:Server= $host ; Database = $db ", $user, $pwd);
            $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );
        }
        catch(Exception $e){
            die(var_dump($e));
        }
   
    <span data-ttu-id="b8434-169">Ebben az esetben szüksége lesz tooupdate hello értékeinek <code>$user</code> és <code>$pwd</code> a helyi MySQL-felhasználó nevét és jelszavát.</span><span class="sxs-lookup"><span data-stu-id="b8434-169">Again, you will need tooupdate hello values for <code>$user</code> and <code>$pwd</code> with your local MySQL user name and password.</span></span>
8. <span data-ttu-id="b8434-170">Következő hello adatbázis kapcsolati kódja vegye fel a regisztrációs adatok beszúrása hello adatbázis kódot.</span><span class="sxs-lookup"><span data-stu-id="b8434-170">Following hello database connection code, add code for inserting registration information into hello database.</span></span>
   
        if(!empty($_POST)) {
        try {
            $name = $_POST['name'];
            $email = $_POST['email'];
            $date = date("Y-m-d");
            // Insert data
            $sql_insert = "INSERT INTO registration_tbl (name, email, date) 
                           VALUES (?,?,?)";
            $stmt = $conn->prepare($sql_insert);
            $stmt->bindValue(1, $name);
            $stmt->bindValue(2, $email);
            $stmt->bindValue(3, $date);
            $stmt->execute();
        }
        catch(Exception $e) {
            die(var_dump($e));
        }
        echo "<h3>Your're registered!</h3>";
        }
9. <span data-ttu-id="b8434-171">Végül követően a fenti hello kódot, adja hozzá az adatok lekérdezése hello adatbázis kódját.</span><span class="sxs-lookup"><span data-stu-id="b8434-171">Finally, following hello code above, add code for retrieving data from hello database.</span></span>
   
        $sql_select = "SELECT * FROM registration_tbl";
        $stmt = $conn->query($sql_select);
        $registrants = $stmt->fetchAll(); 
        if(count($registrants) > 0) {
            echo "<h2>People who are registered:</h2>";
            echo "<table>";
            echo "<tr><th>Name</th>";
            echo "<th>Email</th>";
            echo "<th>Date</th></tr>";
            foreach($registrants as $registrant) {
                echo "<tr><td>".$registrant['name']."</td>";
                echo "<td>".$registrant['email']."</td>";
                echo "<td>".$registrant['date']."</td></tr>";
            }
             echo "</table>";
        } else {
            echo "<h3>No one is currently registered.</h3>";
        }

<span data-ttu-id="b8434-172">Most már megkeresheti túl**http://localhost:8000/index.php** tootest hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="b8434-172">You can now browse too**http://localhost:8000/index.php** tootest hello application.</span></span>

## <a name="publish-your-application"></a><span data-ttu-id="b8434-173">Az alkalmazás közzététele</span><span class="sxs-lookup"><span data-stu-id="b8434-173">Publish your application</span></span>
<span data-ttu-id="b8434-174">Az alkalmazás helyi tesztelése, után közzéteheti azt tooApp Service Web Apps Git használatával.</span><span class="sxs-lookup"><span data-stu-id="b8434-174">After you have tested your application locally, you can publish it tooApp Service Web Apps using Git.</span></span> <span data-ttu-id="b8434-175">Azonban először tooupdate hello adatbázis-kapcsolat adatainak hello alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="b8434-175">However, you first need tooupdate hello database connection information in hello application.</span></span> <span data-ttu-id="b8434-176">Korábban beszerzett hello adatbázis-kapcsolat adatainak használatával (a hello **első SQL-adatbázis-kapcsolat adatainak** szakaszban), a következő információkat a frissítés hello **mindkét** hello `createdatabase.php` és `index.php` hello fájlokat a megfelelő értékeket:</span><span class="sxs-lookup"><span data-stu-id="b8434-176">Using hello database connection information you obtained earlier (in hello **Get SQL Database connection information** section), update hello following information in **both** hello `createdatabase.php` and `index.php` files with hello appropriate values:</span></span>

    // DB connection info
    $host = "tcp:<value of Server>";
    $user = "<value of User Name>";
    $pwd = "<your password>";
    $db = "<value of SQL Database>";

> [!NOTE]
> <span data-ttu-id="b8434-177">A hello <code>$host</code>, kiszolgáló hello értéket kell $a, a <code>tcp:</code>.</span><span class="sxs-lookup"><span data-stu-id="b8434-177">In hello <code>$host</code>, hello value of Server must be prepended with <code>tcp:</code>.</span></span>
> 
> 

<span data-ttu-id="b8434-178">Most már készen áll a tooset be Git-közzététel és hello alkalmazás közzététele.</span><span class="sxs-lookup"><span data-stu-id="b8434-178">Now, you are ready tooset up Git publishing and publish hello application.</span></span>

> [!NOTE]
> <span data-ttu-id="b8434-179">Ezek a ugyanazokat a lépéseket az áttelepítés előtt feljegyzett hello hello végén hello **Azure-webalkalmazás létrehozása és beállítása a Git-közzététel** fenti szakaszban.</span><span class="sxs-lookup"><span data-stu-id="b8434-179">These are hello same steps noted at hello end of hello **Create an Azure web app and set up Git publishing** section above.</span></span>
> 
> 

1. <span data-ttu-id="b8434-180">Nyissa meg a GitBash (vagy egy terminált, ha Git a `PATH`), módosítsa a könyvtárakat toohello az alkalmazás gyökérkönyvtárában (hello **regisztrációs** directory), és futtatási hello a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="b8434-180">Open GitBash (or a terminal, if Git is in your `PATH`), change directories toohello root directory of your application (hello **registration** directory), and run hello following commands:</span></span>
   
        git init
        git add .
        git commit -m "initial commit"
        git remote add azure [URL for remote repository]
        git push azure master
   
    <span data-ttu-id="b8434-181">A korábban létrehozott hello jelszó bekéri.</span><span class="sxs-lookup"><span data-stu-id="b8434-181">You will be prompted for hello password you created earlier.</span></span>
2. <span data-ttu-id="b8434-182">Keresse meg a túl**http://[web app name].azurewebsites.net/createtable.php** toocreate hello SQL-adatbázistáblában szereplő hello alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="b8434-182">Browse too**http://[web app name].azurewebsites.net/createtable.php** toocreate hello SQL database table for hello application.</span></span>
3. <span data-ttu-id="b8434-183">Keresse meg a túl**http://[web app name].azurewebsites.net/index.php** toobegin hello alkalmazás segítségével.</span><span class="sxs-lookup"><span data-stu-id="b8434-183">Browse too**http://[web app name].azurewebsites.net/index.php** toobegin using hello application.</span></span>

<span data-ttu-id="b8434-184">Miután közzétette az alkalmazást, módosításokat tooit állapotában, és Git toopublish használja őket.</span><span class="sxs-lookup"><span data-stu-id="b8434-184">After you have published your application, you can begin making changes tooit and use Git toopublish them.</span></span> 

## <a name="publish-changes-tooyour-application"></a><span data-ttu-id="b8434-185">Módosítások tooyour alkalmazás közzététele</span><span class="sxs-lookup"><span data-stu-id="b8434-185">Publish changes tooyour application</span></span>
<span data-ttu-id="b8434-186">toopublish változik tooapplication, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="b8434-186">toopublish changes tooapplication, follow these steps:</span></span>

1. <span data-ttu-id="b8434-187">Módosításokat tooyour alkalmazás helyi.</span><span class="sxs-lookup"><span data-stu-id="b8434-187">Make changes tooyour application locally.</span></span>
2. <span data-ttu-id="b8434-188">Nyissa meg a GitBash (vagy a Terminálszolgáltatások informatikai Git van a `PATH`), módosítsa a könyvtárakat toohello az alkalmazás gyökérkönyvtárában, és futtassa a következő parancsok hello:</span><span class="sxs-lookup"><span data-stu-id="b8434-188">Open GitBash (or a terminal, it Git is in your `PATH`), change directories toohello root directory of your application, and run hello following commands:</span></span>
   
        git add .
        git commit -m "comment describing changes"
        git push azure master
   
    <span data-ttu-id="b8434-189">A korábban létrehozott hello jelszó bekéri.</span><span class="sxs-lookup"><span data-stu-id="b8434-189">You will be prompted for hello password you created earlier.</span></span>
3. <span data-ttu-id="b8434-190">Keresse meg a túl**http://[web app name].azurewebsites.net/index.php** toosee a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="b8434-190">Browse too**http://[web app name].azurewebsites.net/index.php** toosee your changes.</span></span>

## <a name="whats-changed"></a><span data-ttu-id="b8434-191">A változások</span><span class="sxs-lookup"><span data-stu-id="b8434-191">What's changed</span></span>
* <span data-ttu-id="b8434-192">Egy útmutató toohello webhelyek tooApp szolgáltatás változás lásd: [Azure App Service és a hatása a meglévő Azure-szolgáltatások](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="b8434-192">For a guide toohello change from Websites tooApp Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

[install-php]: http://www.php.net/manual/en/install.php
[install-SQLExpress]: http://www.microsoft.com/download/details.aspx?id=29062
[install-Drivers]: http://www.microsoft.com/download/details.aspx?id=20098
[install-git]: http://git-scm.com/
[pdo-sqlsrv]: http://php.net/pdo_sqlsrv

