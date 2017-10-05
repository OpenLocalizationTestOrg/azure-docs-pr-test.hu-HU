---
title: "PHP-SQL-webalkalmazás létrehozása és telepítése az Azure App Service Git használatával"
description: "Ez az oktatóanyag bemutatja, hogyan egy PHP webalkalmazást, amely tárolja az adatokat az Azure SQL-adatbázis létrehozása és használata az Azure App Service Git-telepítés."
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
ms.openlocfilehash: 0baa3eced3824fec0907ca937c594f127a2bdf8b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-php-sql-web-app-and-deploy-to-azure-app-service-using-git"></a><span data-ttu-id="6998f-103">PHP-SQL-webalkalmazás létrehozása és telepítése az Azure App Service Git használatával</span><span class="sxs-lookup"><span data-stu-id="6998f-103">Create a PHP-SQL web app and deploy to Azure App Service using Git</span></span>
<span data-ttu-id="6998f-104">Ez az oktatóanyag bemutatja, hogyan hozzon létre egy PHP webalkalmazást a [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) , amely kapcsolódik az Azure SQL Database és a Git használatával telepítés módját.</span><span class="sxs-lookup"><span data-stu-id="6998f-104">This tutorial shows you how to create a PHP web app in [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) that connects to Azure SQL Database and how to deploy it using Git.</span></span> <span data-ttu-id="6998f-105">Ez az oktatóanyag feltételezi, hogy rendelkezik [PHP][install-php], [SQL Server Express][install-SQLExpress], a [Microsoft SQL Server php Drivers](http://www.microsoft.com/download/en/details.aspx?id=20098), és [Git] [ install-git] telepítve a számítógépre.</span><span class="sxs-lookup"><span data-stu-id="6998f-105">This tutorial assumes you have [PHP][install-php], [SQL Server Express][install-SQLExpress], the [Microsoft Drivers for SQL Server for PHP](http://www.microsoft.com/download/en/details.aspx?id=20098), and [Git][install-git] installed on your computer.</span></span> <span data-ttu-id="6998f-106">Ez az útmutató befejezése után fog egy PHP-SQL Azure-ban futó webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="6998f-106">Upon completing this guide, you will have a PHP-SQL web app running in Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="6998f-107">Telepítse és a PHP az SQL Server PHP, SQL Server Express és a Microsoft Drivers konfigurálása a [Microsoft Webplatform-telepítő](http://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="6998f-107">You can install and configure PHP, SQL Server Express, and the Microsoft Drivers for SQL Server for PHP using the [Microsoft Web Platform Installer](http://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> 
> 

<span data-ttu-id="6998f-108">Az oktatóanyagban érintett témák köre:</span><span class="sxs-lookup"><span data-stu-id="6998f-108">You will learn:</span></span>

* <span data-ttu-id="6998f-109">Azure-webalkalmazás és egy SQL-adatbázis használata a [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715).</span><span class="sxs-lookup"><span data-stu-id="6998f-109">How to create an Azure web app and a SQL Database using the [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715).</span></span> <span data-ttu-id="6998f-110">A PHP alapértelmezés szerint engedélyezve van az App Service Web Apps, mert semmi különleges kell futnia a PHP-kódot.</span><span class="sxs-lookup"><span data-stu-id="6998f-110">Because PHP is enabled in App Service Web Apps by default, nothing special is required to run your PHP code.</span></span>
* <span data-ttu-id="6998f-111">Hogyan közzététele az Azure Git használatával az alkalmazás közzétételét.</span><span class="sxs-lookup"><span data-stu-id="6998f-111">How to publish and re-publish your application to Azure using Git.</span></span>

<span data-ttu-id="6998f-112">Az oktatóanyag utasításait követve egy egyszerű regisztrációs webalkalmazásából PHP fog létrehozni.</span><span class="sxs-lookup"><span data-stu-id="6998f-112">By following this tutorial, you will build a simple registration web application in PHP.</span></span> <span data-ttu-id="6998f-113">Az alkalmazás üzemel az Azure-webhely.</span><span class="sxs-lookup"><span data-stu-id="6998f-113">The application will be hosted in an Azure Website.</span></span> <span data-ttu-id="6998f-114">A kész alkalmazás képernyőfelvételének alatt van:</span><span class="sxs-lookup"><span data-stu-id="6998f-114">A screenshot of the completed application is below:</span></span>

![Az Azure PHP-webhely](./media/web-sites-php-sql-database-deploy-use-git/running_app_3.png)

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

> [!NOTE]
> <span data-ttu-id="6998f-116">Ha az Azure App Service-t az Azure-fiók regisztrálása előtt szeretné kipróbálni, ugorjon [Az Azure App Service kipróbálása](https://azure.microsoft.com/try/app-service/) oldalra. Itt azonnal létrehozhat egy ideiglenes, kezdő szintű webalkalmazást az App Service szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="6998f-116">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="6998f-117">Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.</span><span class="sxs-lookup"><span data-stu-id="6998f-117">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="create-an-azure-web-app-and-set-up-git-publishing"></a><span data-ttu-id="6998f-118">Azure-webalkalmazás létrehozása és beállítása a Git-közzététel</span><span class="sxs-lookup"><span data-stu-id="6998f-118">Create an Azure web app and set up Git publishing</span></span>
<span data-ttu-id="6998f-119">Kövesse az alábbi lépéseket az Azure web app és az SQL-adatbázis létrehozásához:</span><span class="sxs-lookup"><span data-stu-id="6998f-119">Follow these steps to create an Azure web app and a SQL Database:</span></span>

1. <span data-ttu-id="6998f-120">Jelentkezzen be az [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="6998f-120">Log in to the [Azure Portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="6998f-121">Nyissa meg az Azure piactéren kattintva a **új** az irányítópult bal felső ikonra, kattintson a **kijelölése az összes** mellett a piactéren, és kiválasztja **Web + mobil**.</span><span class="sxs-lookup"><span data-stu-id="6998f-121">Open the Azure Marketplace by clicking the **New** icon on the top left of the dashboard, click on **Select All** next to Marketplace and selecting **Web + Mobile**.</span></span>
3. <span data-ttu-id="6998f-122">Válassza ki a piactér **Web + mobil**.</span><span class="sxs-lookup"><span data-stu-id="6998f-122">In the Marketplace, select **Web + Mobile**.</span></span>
4. <span data-ttu-id="6998f-123">Kattintson a **webes alkalmazás + SQL** ikonra.</span><span class="sxs-lookup"><span data-stu-id="6998f-123">Click the **Web app + SQL** icon.</span></span>
5. <span data-ttu-id="6998f-124">A webes alkalmazás + SQL alkalmazás leírásának elolvasása után válassza ki a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="6998f-124">After reading the description of the Web app + SQL app, select **Create**.</span></span>
6. <span data-ttu-id="6998f-125">Kattintson az egyes komponenseihez (**erőforráscsoport**, **webalkalmazás**, **adatbázis**, és **előfizetés**), és adja meg vagy válassza ki a kötelező mezőket értékeit:</span><span class="sxs-lookup"><span data-stu-id="6998f-125">Click on each part (**Resource Group**, **Web App**, **Database**, and **Subscription**) and enter or select values for the required fields:</span></span>
   
   * <span data-ttu-id="6998f-126">Adjon meg egy URL-címet a választott</span><span class="sxs-lookup"><span data-stu-id="6998f-126">Enter a URL name of your choice</span></span>    
   * <span data-ttu-id="6998f-127">Adatbázis-kiszolgáló hitelesítő adatok beállítása</span><span class="sxs-lookup"><span data-stu-id="6998f-127">Configure database server credentials</span></span>
   * <span data-ttu-id="6998f-128">Válassza ki az Önhöz legközelebbi régiót</span><span class="sxs-lookup"><span data-stu-id="6998f-128">Select the region closest to you</span></span>
     
     ![az alkalmazás konfigurálása](./media/web-sites-php-sql-database-deploy-use-git/configure-db-settings.png)
7. <span data-ttu-id="6998f-130">Ha elkészült, a webes alkalmazás meghatározása, kattintson **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="6998f-130">When finished defining the web app, click **Create**.</span></span>
   
    <span data-ttu-id="6998f-131">A webalkalmazás létrehozásakor a **értesítések** gomb villogjon, egy zöld **sikeres** és az erőforráscsoport panel nyissa meg a csoport a web app és az SQL-adatbázis egyaránt megjeleníthető.</span><span class="sxs-lookup"><span data-stu-id="6998f-131">When the web app has been created, the **Notifications** button will flash a green **SUCCESS** and the resource group blade open to show both the web app and the SQL database in the group.</span></span>
8. <span data-ttu-id="6998f-132">Kattintson a webalkalmazás az erőforráscsoport panel megnyitása a webalkalmazása panelén.</span><span class="sxs-lookup"><span data-stu-id="6998f-132">Click the web app's icon in the resource group blade to open the web app's blade.</span></span>
   
    ![webes alkalmazás erőforráscsoport](./media/web-sites-php-sql-database-deploy-use-git/resource-group-blade.png)
9. <span data-ttu-id="6998f-134">A **beállítások** kattintson **folyamatos üzembe helyezés** > **kötelező beállítások konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="6998f-134">In **Settings** click **Continuous deployment** > **Configure required settings**.</span></span> <span data-ttu-id="6998f-135">Válassza ki **helyi Git-tárház** kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="6998f-135">Select **Local Git Repository** and click **OK**.</span></span>
   
    ![hol található a Forráskód](./media/web-sites-php-sql-database-deploy-use-git/setup-local-git.png)
   
    <span data-ttu-id="6998f-137">Ha nem állított be egy Git-tárház előtt, meg kell adnia egy felhasználónevet és jelszót.</span><span class="sxs-lookup"><span data-stu-id="6998f-137">If you have not set up a Git repository before, you must provide a user name and password.</span></span> <span data-ttu-id="6998f-138">Ehhez kattintson **beállítások** > **üzembe helyezési hitelesítő adatok** a webalkalmazás panelen.</span><span class="sxs-lookup"><span data-stu-id="6998f-138">To do this, click **Settings** > **Deployment credentials** in the web app's blade.</span></span>
   
    ![](./media/web-sites-php-sql-database-deploy-use-git/deployment-credentials.png)
10. <span data-ttu-id="6998f-139">A **beállítások** kattintson a **tulajdonságok** a Git távoli URL-címet kell használnia a PHP-alkalmazás telepítése később megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="6998f-139">In **Settings** click on **Properties** to see the Git remote URL you need to use to deploy your PHP app later.</span></span>

## <a name="get-sql-database-connection-information"></a><span data-ttu-id="6998f-140">SQL adatbázis-kapcsolat adatainak lekérése</span><span class="sxs-lookup"><span data-stu-id="6998f-140">Get SQL Database connection information</span></span>
<span data-ttu-id="6998f-141">Az SQL Database-példányt, amely csatolva van a web app alkalmazásban a fog csatlakozni kell a kapcsolati adatokat, az adatbázis létrehozásakor megadott.</span><span class="sxs-lookup"><span data-stu-id="6998f-141">To connect to the SQL Database instance that is linked to your web app, your will need the connection information, which you specified when you created the database.</span></span> <span data-ttu-id="6998f-142">Az SQL adatbázis-kapcsolat adatainak lekéréséhez kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="6998f-142">To get the SQL Database connection information, follow these steps:</span></span>

1. <span data-ttu-id="6998f-143">Az erőforráscsoport panelen kattintson az SQL-adatbázis ikonra.</span><span class="sxs-lookup"><span data-stu-id="6998f-143">Back in the resource group's blade, click the SQL database's icon.</span></span>
2. <span data-ttu-id="6998f-144">Az SQL-adatbázis paneljén kattintson **beállítások** > **tulajdonságok**, majd kattintson a **adatbázis-kapcsolati karakterláncok megjelenítése**.</span><span class="sxs-lookup"><span data-stu-id="6998f-144">In the SQL database's blade, click **Settings** > **Properties**, then click **Show database connection strings**.</span></span> 
   
    ![Adatbázis-tulajdonságok megtekintése](./media/web-sites-php-sql-database-deploy-use-git/view-database-properties.png)
3. <span data-ttu-id="6998f-146">Az a **PHP** szakaszban, a megjelenő párbeszédpanelen jegyezze fel a értékeit `Server`, `SQL Database`, és `User Name`.</span><span class="sxs-lookup"><span data-stu-id="6998f-146">From the **PHP** section of the resulting dialog, make note of the values for `Server`, `SQL Database`, and `User Name`.</span></span> <span data-ttu-id="6998f-147">Ezeket az értékeket fogja használni később, amikor a PHP-webalkalmazás közzététele az Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="6998f-147">You will use these values later when publishing your PHP web app to Azure App Service.</span></span>

## <a name="build-and-test-your-application-locally"></a><span data-ttu-id="6998f-148">Az alkalmazás helyi összeállítása és tesztelése</span><span class="sxs-lookup"><span data-stu-id="6998f-148">Build and test your application locally</span></span>
<span data-ttu-id="6998f-149">A regisztrációs alkalmazás egy egyszerű PHP-alkalmazás, amely lehetővé teszi, hogy regisztrálja az esemény a név és e-mail cím megadásával.</span><span class="sxs-lookup"><span data-stu-id="6998f-149">The Registration application is a simple PHP application that allows you to register for an event by providing your name and email address.</span></span> <span data-ttu-id="6998f-150">Előző igénylők kapcsolatos információkat a táblázatban jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="6998f-150">Information about previous registrants is displayed in a table.</span></span> <span data-ttu-id="6998f-151">Regisztrációs adatai egy SQL Database-példányt.</span><span class="sxs-lookup"><span data-stu-id="6998f-151">Registration information is stored in a SQL Database instance.</span></span> <span data-ttu-id="6998f-152">Az alkalmazás két fájlt (másolás/beillesztés kód alatt érhető el) áll:</span><span class="sxs-lookup"><span data-stu-id="6998f-152">The application consists of two files (copy/paste code available below):</span></span>

* <span data-ttu-id="6998f-153">**index.php**: regisztráció és a bejegyzés adatokat tartalmazó táblát űrlap jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="6998f-153">**index.php**: Displays a form for registration and a table containing registrant information.</span></span>
* <span data-ttu-id="6998f-154">**createtable.php**: az SQL-adatbázis táblája az alkalmazás létrehozza.</span><span class="sxs-lookup"><span data-stu-id="6998f-154">**createtable.php**: Creates the SQL Database table for the application.</span></span> <span data-ttu-id="6998f-155">Ezt a fájlt csak egyszer használható.</span><span class="sxs-lookup"><span data-stu-id="6998f-155">This file will only be used once.</span></span>

<span data-ttu-id="6998f-156">Az alkalmazás helyileg történő futtatása, kövesse az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="6998f-156">To run the application locally, follow the steps below.</span></span> <span data-ttu-id="6998f-157">Vegye figyelembe, hogy ezek a lépések feltételezik, PHP és az SQL Server Express állítsa be a helyi számítógépen, és engedélyezte a [OEM-bővítmény SQL Server][pdo-sqlsrv].</span><span class="sxs-lookup"><span data-stu-id="6998f-157">Note that these steps assume you have PHP and SQL Server Express set up on your local machine, and that you have enabled the [PDO extension for SQL Server][pdo-sqlsrv].</span></span>

1. <span data-ttu-id="6998f-158">Hozzon létre egy SQL Server-adatbázis nevű `registration`.</span><span class="sxs-lookup"><span data-stu-id="6998f-158">Create a SQL Server database called `registration`.</span></span> <span data-ttu-id="6998f-159">Az ehhez a `sqlcmd` parancssor a következő parancsokkal:</span><span class="sxs-lookup"><span data-stu-id="6998f-159">You can do this from the `sqlcmd` command prompt with these commands:</span></span>
   
        >sqlcmd -S localhost\sqlexpress -U <local user name> -P <local password>
        1> create database registration
        2> GO    
2. <span data-ttu-id="6998f-160">Az alkalmazás gyökérkönyvtárában, hozzon létre két fájlokat - egy `createtable.php` és egy `index.php`.</span><span class="sxs-lookup"><span data-stu-id="6998f-160">In your application root directory, create two files in it - one called `createtable.php` and one called `index.php`.</span></span>
3. <span data-ttu-id="6998f-161">Nyissa meg a `createtable.php` fájlt egy szövegszerkesztőben, vagy IDE, és adja hozzá az alábbi kódot.</span><span class="sxs-lookup"><span data-stu-id="6998f-161">Open the `createtable.php` file in a text editor or IDE and add the code below.</span></span> <span data-ttu-id="6998f-162">Ez a kód létrehozásához használható a `registration_tbl` a tábla a `registration` adatbázis.</span><span class="sxs-lookup"><span data-stu-id="6998f-162">This code will be used to create the `registration_tbl` table in the `registration` database.</span></span>
   
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
   
    <span data-ttu-id="6998f-163">Vegye figyelembe, hogy értéket kell <code>$user</code> és <code>$pwd</code> a helyi SQL Server felhasználói nevét és jelszavát.</span><span class="sxs-lookup"><span data-stu-id="6998f-163">Note that you will need to update the values for <code>$user</code> and <code>$pwd</code> with your local SQL Server user name and password.</span></span>
4. <span data-ttu-id="6998f-164">Parancsot egy terminálban az alkalmazás gyökérkönyvtárában, írja be a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="6998f-164">In a terminal at the root directory of the application type the following command:</span></span>
   
        php -S localhost:8000
5. <span data-ttu-id="6998f-165">Nyisson meg egy webböngészőt, és keresse meg a **http://localhost:8000/createtable.php**.</span><span class="sxs-lookup"><span data-stu-id="6998f-165">Open a web browser and browse to **http://localhost:8000/createtable.php**.</span></span> <span data-ttu-id="6998f-166">Ekkor létrejön az `registration_tbl` tábla az adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="6998f-166">This will create the `registration_tbl` table in the database.</span></span>
6. <span data-ttu-id="6998f-167">Nyissa meg a **index.php** fájlt egy szövegszerkesztőben, vagy IDE, és adja hozzá az alapvető HTML- és CSS kódot (a PHP kód megkapja a későbbi lépésekben) lap.</span><span class="sxs-lookup"><span data-stu-id="6998f-167">Open the **index.php** file in a text editor or IDE and add the basic HTML and CSS code for the page (the PHP code will be added in later steps).</span></span>
   
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
        <p>Fill in your name and email address, then click <strong>Submit</strong> to register.</p>
        <form method="post" action="index.php" enctype="multipart/form-data" >
              Name  <input type="text" name="name" id="name"/></br>
              Email <input type="text" name="email" id="email"/></br>
              <input type="submit" name="submit" value="Submit" />
        </form>
        <?php
   
        ?>
        </body>
        </html>
7. <span data-ttu-id="6998f-168">A PHP címkék belül adja hozzá a PHP kódot az adatbázishoz való kapcsolódáshoz.</span><span class="sxs-lookup"><span data-stu-id="6998f-168">Within the PHP tags, add PHP code for connecting to the database.</span></span>
   
        // DB connection info
        $host = "localhost\sqlexpress";
        $user = "user name";
        $pwd = "password";
        $db = "registration";
        // Connect to database.
        try {
            $conn = new PDO( "sqlsrv:Server= $host ; Database = $db ", $user, $pwd);
            $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );
        }
        catch(Exception $e){
            die(var_dump($e));
        }
   
    <span data-ttu-id="6998f-169">Ebben az esetben szüksége lesz a értéket <code>$user</code> és <code>$pwd</code> a helyi MySQL-felhasználó nevét és jelszavát.</span><span class="sxs-lookup"><span data-stu-id="6998f-169">Again, you will need to update the values for <code>$user</code> and <code>$pwd</code> with your local MySQL user name and password.</span></span>
8. <span data-ttu-id="6998f-170">Követően az adatbázis-kapcsolat kód adja hozzá a kódot a regisztrációs adatokat beszúrni az adatbázisba.</span><span class="sxs-lookup"><span data-stu-id="6998f-170">Following the database connection code, add code for inserting registration information into the database.</span></span>
   
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
9. <span data-ttu-id="6998f-171">Végezetül a fenti kódot követően adja hozzá a kódot az adatokat az adatbázisból.</span><span class="sxs-lookup"><span data-stu-id="6998f-171">Finally, following the code above, add code for retrieving data from the database.</span></span>
   
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

<span data-ttu-id="6998f-172">Most tallózással megkereshet **http://localhost:8000/index.php** kell tesztelni az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="6998f-172">You can now browse to **http://localhost:8000/index.php** to test the application.</span></span>

## <a name="publish-your-application"></a><span data-ttu-id="6998f-173">Az alkalmazás közzététele</span><span class="sxs-lookup"><span data-stu-id="6998f-173">Publish your application</span></span>
<span data-ttu-id="6998f-174">Az alkalmazás helyi tesztelése, után közzéteheti az App Service Web Apps Git használatával.</span><span class="sxs-lookup"><span data-stu-id="6998f-174">After you have tested your application locally, you can publish it to App Service Web Apps using Git.</span></span> <span data-ttu-id="6998f-175">Azonban először az alkalmazást az adatbázis-kapcsolat adatainak frissítése.</span><span class="sxs-lookup"><span data-stu-id="6998f-175">However, you first need to update the database connection information in the application.</span></span> <span data-ttu-id="6998f-176">Korábban beszerzett adatbázis kapcsolati információk használatával (a a **első SQL-adatbázis-kapcsolat adatainak** szakaszban), a következő információ frissítésére **mindkét** a `createdatabase.php` és `index.php` fájlokat a megfelelő értékekkel:</span><span class="sxs-lookup"><span data-stu-id="6998f-176">Using the database connection information you obtained earlier (in the **Get SQL Database connection information** section), update the following information in **both** the `createdatabase.php` and `index.php` files with the appropriate values:</span></span>

    // DB connection info
    $host = "tcp:<value of Server>";
    $user = "<value of User Name>";
    $pwd = "<your password>";
    $db = "<value of SQL Database>";

> [!NOTE]
> <span data-ttu-id="6998f-177">Az a <code>$host</code>, a kiszolgáló értéke kell $a, a <code>tcp:</code>.</span><span class="sxs-lookup"><span data-stu-id="6998f-177">In the <code>$host</code>, the value of Server must be prepended with <code>tcp:</code>.</span></span>
> 
> 

<span data-ttu-id="6998f-178">Most már készen áll Git-közzététel beállítását, és tegye közzé az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="6998f-178">Now, you are ready to set up Git publishing and publish the application.</span></span>

> [!NOTE]
> <span data-ttu-id="6998f-179">Ezek a végén az áttelepítés előtt feljegyzett ugyanazokat a lépéseket a **Azure-webalkalmazás létrehozása és beállítása a Git-közzététel** fenti szakaszban.</span><span class="sxs-lookup"><span data-stu-id="6998f-179">These are the same steps noted at the end of the **Create an Azure web app and set up Git publishing** section above.</span></span>
> 
> 

1. <span data-ttu-id="6998f-180">Nyissa meg a GitBash (vagy egy terminált, ha Git a `PATH`), módosítsa a könyvtárat az alkalmazás gyökérkönyvtárában (a **regisztrációs** directory), és futtassa a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="6998f-180">Open GitBash (or a terminal, if Git is in your `PATH`), change directories to the root directory of your application (the **registration** directory), and run the following commands:</span></span>
   
        git init
        git add .
        git commit -m "initial commit"
        git remote add azure [URL for remote repository]
        git push azure master
   
    <span data-ttu-id="6998f-181">Fogja kérni fogja a korábban létrehozott jelszót.</span><span class="sxs-lookup"><span data-stu-id="6998f-181">You will be prompted for the password you created earlier.</span></span>
2. <span data-ttu-id="6998f-182">Keresse meg a **http://[web app name].azurewebsites.net/createtable.php** az SQL-adatbázistáblában szereplő, az alkalmazás létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="6998f-182">Browse to **http://[web app name].azurewebsites.net/createtable.php** to create the SQL database table for the application.</span></span>
3. <span data-ttu-id="6998f-183">Keresse meg a **http://[web app name].azurewebsites.net/index.php** az alkalmazás használatának megkezdéséhez.</span><span class="sxs-lookup"><span data-stu-id="6998f-183">Browse to **http://[web app name].azurewebsites.net/index.php** to begin using the application.</span></span>

<span data-ttu-id="6998f-184">Miután közzétette az alkalmazást, annak módosításával kezdődik, és a Git segítségével közzéteszi.</span><span class="sxs-lookup"><span data-stu-id="6998f-184">After you have published your application, you can begin making changes to it and use Git to publish them.</span></span> 

## <a name="publish-changes-to-your-application"></a><span data-ttu-id="6998f-185">Alkalmazásmódosítások közzététele</span><span class="sxs-lookup"><span data-stu-id="6998f-185">Publish changes to your application</span></span>
<span data-ttu-id="6998f-186">Változások közzétételére alkalmazást, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="6998f-186">To publish changes to application, follow these steps:</span></span>

1. <span data-ttu-id="6998f-187">Az alkalmazás helyi módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="6998f-187">Make changes to your application locally.</span></span>
2. <span data-ttu-id="6998f-188">Nyissa meg a GitBash (vagy a Terminálszolgáltatások informatikai Git van a `PATH`), módosítsa a könyvtárat az alkalmazás gyökérkönyvtárában, és futtassa a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="6998f-188">Open GitBash (or a terminal, it Git is in your `PATH`), change directories to the root directory of your application, and run the following commands:</span></span>
   
        git add .
        git commit -m "comment describing changes"
        git push azure master
   
    <span data-ttu-id="6998f-189">Fogja kérni fogja a korábban létrehozott jelszót.</span><span class="sxs-lookup"><span data-stu-id="6998f-189">You will be prompted for the password you created earlier.</span></span>
3. <span data-ttu-id="6998f-190">Keresse meg a **http://[web app name].azurewebsites.net/index.php** szeretné látni a változtatásokat.</span><span class="sxs-lookup"><span data-stu-id="6998f-190">Browse to **http://[web app name].azurewebsites.net/index.php** to see your changes.</span></span>

## <a name="whats-changed"></a><span data-ttu-id="6998f-191">A változások</span><span class="sxs-lookup"><span data-stu-id="6998f-191">What's changed</span></span>
* <span data-ttu-id="6998f-192">Információk a Websites szolgáltatásról az App Service-re való váltásról: [Az Azure App Service és a hatása a meglévő Azure-szolgáltatásokra](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="6998f-192">For a guide to the change from Websites to App Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

[install-php]: http://www.php.net/manual/en/install.php
[install-SQLExpress]: http://www.microsoft.com/download/details.aspx?id=29062
[install-Drivers]: http://www.microsoft.com/download/details.aspx?id=20098
[install-git]: http://git-scm.com/
[pdo-sqlsrv]: http://php.net/pdo_sqlsrv

