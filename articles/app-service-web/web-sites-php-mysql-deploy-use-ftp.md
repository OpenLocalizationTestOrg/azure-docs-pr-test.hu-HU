---
title: "PHP-MySQL webes alkalmazás létrehozása az Azure App Service-ben és üzembe helyezése FTP használatával"
description: "Ez az oktatóanyag bemutatja, hogyan hozzon létre egy PHP webalkalmazást, amely tárolja az adatokat a MySQL és -felhasználási FTP telepítési az Azure-bA."
services: app-service\web
documentationcenter: php
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 6d9d1de5-5868-48fd-8bad-decb4979cd65
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: d428dffc6b810a692be0ec39a5f9cca05f5439e3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-php-mysql-web-app-in-azure-app-service-and-deploy-using-ftp"></a><span data-ttu-id="c7704-103">PHP-MySQL webes alkalmazás létrehozása az Azure App Service-ben és üzembe helyezése FTP használatával</span><span class="sxs-lookup"><span data-stu-id="c7704-103">Create a PHP-MySQL web app in Azure App Service and deploy using FTP</span></span>
<span data-ttu-id="c7704-104">Az oktatóanyag bemutatja, hogyan PHP-MySQL-webalkalmazás létrehozása és központi telepítése FTP használatával.</span><span class="sxs-lookup"><span data-stu-id="c7704-104">This tutorial shows you how to create a PHP-MySQL web app and how to deploy it using FTP.</span></span> <span data-ttu-id="c7704-105">Ez az oktatóanyag feltételezi, hogy rendelkezik [PHP][install-php], [MySQL][install-mysql], webkiszolgáló, és az FTP-ügyfél telepítve a számítógépre.</span><span class="sxs-lookup"><span data-stu-id="c7704-105">This tutorial assumes you have [PHP][install-php], [MySQL][install-mysql], a web server, and an FTP client installed on your computer.</span></span> <span data-ttu-id="c7704-106">Ez az oktatóanyag utasításai követhetők bármely operációs rendszeren, beleértve a Windows, Mac és Linux.</span><span class="sxs-lookup"><span data-stu-id="c7704-106">The instructions in this tutorial can be followed on any operating system, including Windows, Mac, and  Linux.</span></span> <span data-ttu-id="c7704-107">Ez az útmutató befejezése után fog egy PHP/MySQL az Azure-ban futó webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="c7704-107">Upon completing this guide, you will have a PHP/MySQL web app running in Azure.</span></span>

<span data-ttu-id="c7704-108">Az oktatóanyagban érintett témák köre:</span><span class="sxs-lookup"><span data-stu-id="c7704-108">You will learn:</span></span>

* <span data-ttu-id="c7704-109">Tudnivalók a webalkalmazások és az Azure portál használatával MySQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="c7704-109">How to create a web app and a MySQL database using the Azure Portal.</span></span> <span data-ttu-id="c7704-110">A PHP alapértelmezés szerint engedélyezve van a webalkalmazásokban, mert semmi különleges kell futnia a PHP-kódot.</span><span class="sxs-lookup"><span data-stu-id="c7704-110">Because PHP is enabled in Web Apps by default, nothing special is required to run your PHP code.</span></span>
* <span data-ttu-id="c7704-111">Hogyan lehet FTP használatával az alkalmazás közzétételére.</span><span class="sxs-lookup"><span data-stu-id="c7704-111">How to publish your application to Azure using FTP.</span></span>

<span data-ttu-id="c7704-112">Az oktatóanyag utasításait követve egy egyszerű regisztrációs webalkalmazás PHP fog létrehozni.</span><span class="sxs-lookup"><span data-stu-id="c7704-112">By following this tutorial, you will build a simple registration web app in PHP.</span></span> <span data-ttu-id="c7704-113">Az alkalmazás üzemel a webalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="c7704-113">The application will be hosted in a Web App.</span></span> <span data-ttu-id="c7704-114">A kész alkalmazás képernyőfelvételének alatt van:</span><span class="sxs-lookup"><span data-stu-id="c7704-114">A screenshot of the completed application is below:</span></span>

![Az Azure PHP-webhely][running-app]

> [!NOTE]
> <span data-ttu-id="c7704-116">Ha szeretne egy olyan fiók regisztrálása előtt Ismerkedés az Azure App Service, folytassa a [App Service kipróbálása](https://azure.microsoft.com/try/app-service/), ahol azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben.</span><span class="sxs-lookup"><span data-stu-id="c7704-116">If you want to get started with Azure App Service before signing up for an account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="c7704-117">Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.</span><span class="sxs-lookup"><span data-stu-id="c7704-117">No credit cards required, no commitments.</span></span> 
> 
> 

## <a name="create-a-web-app-and-set-up-ftp-publishing"></a><span data-ttu-id="c7704-118">Hozzon létre egy webalkalmazást, és állítsa be az FTP-közzététel</span><span class="sxs-lookup"><span data-stu-id="c7704-118">Create a web app and set up FTP publishing</span></span>
<span data-ttu-id="c7704-119">Kövesse az alábbi lépéseket a webes alkalmazás és MySQL-adatbázis létrehozásához:</span><span class="sxs-lookup"><span data-stu-id="c7704-119">Follow these steps to create a web app and a MySQL database:</span></span>

1. <span data-ttu-id="c7704-120">Jelentkezzen be a [Azure-portálon][management-portal].</span><span class="sxs-lookup"><span data-stu-id="c7704-120">Login to the [Azure Portal][management-portal].</span></span>
2. <span data-ttu-id="c7704-121">Kattintson a **+ új** az Azure portál bal felső ikonra.</span><span class="sxs-lookup"><span data-stu-id="c7704-121">Click the **+ New** icon on the top left of the Azure Portal.</span></span>
   
    ![Új Azure-webhely létrehozása][new-website]
3. <span data-ttu-id="c7704-123">A írja be a **webes alkalmazás + MySQL** , majd kattintson a **webes alkalmazás + MySQL**.</span><span class="sxs-lookup"><span data-stu-id="c7704-123">In the search type **Web app + MySQL** and click on **Web app + MySQL**.</span></span>
   
    ![Egyéni új webhely létrehozása][custom-create]
4. <span data-ttu-id="c7704-125">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="c7704-125">Click **Create**.</span></span> <span data-ttu-id="c7704-126">Adjon meg egy egyedi alkalmazás nevét, egy érvényes nevet az erőforráscsoportot és egy új service-csomagot.</span><span class="sxs-lookup"><span data-stu-id="c7704-126">Enter a unique app service name, a valid name for the resource group and a new service plan.</span></span>
   
    ![Állítsa az erőforráscsoport neve][resource-group]
5. <span data-ttu-id="c7704-128">Adja meg az új adatbázis, beleértve a jogi feltételek elfogadását jelző mezőt.</span><span class="sxs-lookup"><span data-stu-id="c7704-128">Enter values for your new database, including agreeing to the legal terms.</span></span>
   
    ![Új MySQL-adatbázis létrehozása][new-mysql-db]
6. <span data-ttu-id="c7704-130">A webalkalmazás létrehozásakor látni fogja az új szolgáltatás panelen.</span><span class="sxs-lookup"><span data-stu-id="c7704-130">When the web app has been created, you will see the new app service blade.</span></span>
7. <span data-ttu-id="c7704-131">Kattintson a **beállítások** > **üzembe helyezési hitelesítő adatok**.</span><span class="sxs-lookup"><span data-stu-id="c7704-131">Click on **Settings** > **Deployment credentials**.</span></span> 
   
    ![Telepítési hitelesítő adatok beállítása][set-deployment-credentials]
8. <span data-ttu-id="c7704-133">Az FTP-közzététel engedélyezéséhez meg kell adnia egy felhasználónevet és jelszót.</span><span class="sxs-lookup"><span data-stu-id="c7704-133">To enable FTP publishing, you must provide a user name and password.</span></span> <span data-ttu-id="c7704-134">A hitelesítő adatok mentése, és jegyezze fel a felhasználónevet és jelszót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="c7704-134">Save the credentials and make a note of the user name and password you create.</span></span>
   
    ![Közzétételi hitelesítő adatok létrehozása][portal-ftp-username-password]

## <a name="build-and-test-your-app-locally"></a><span data-ttu-id="c7704-136">És az alkalmazás helyi teszteléséhez</span><span class="sxs-lookup"><span data-stu-id="c7704-136">Build and test your app locally</span></span>
<span data-ttu-id="c7704-137">A regisztrációs alkalmazás egy egyszerű PHP-alkalmazás, amely lehetővé teszi, hogy regisztrálja az esemény a név és e-mail cím megadásával.</span><span class="sxs-lookup"><span data-stu-id="c7704-137">The Registration application is a simple PHP application that allows you to register for an event by providing your name and email address.</span></span> <span data-ttu-id="c7704-138">Előző igénylők kapcsolatos információkat a táblázatban jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="c7704-138">Information about previous registrants is displayed in a table.</span></span> <span data-ttu-id="c7704-139">Regisztrációs adatokat a MySQL-adatbázisban tárolódik.</span><span class="sxs-lookup"><span data-stu-id="c7704-139">Registration information is stored in a MySQL database.</span></span> <span data-ttu-id="c7704-140">Az alkalmazás két fájlból áll:</span><span class="sxs-lookup"><span data-stu-id="c7704-140">The app consists of two files:</span></span>

* <span data-ttu-id="c7704-141">**index.php**: regisztráció és a bejegyzés adatokat tartalmazó táblát űrlap jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="c7704-141">**index.php**: Displays a form for registration and a table containing registrant information.</span></span>
* <span data-ttu-id="c7704-142">**createtable.php**: az alkalmazás a MySQL táblát hoz létre.</span><span class="sxs-lookup"><span data-stu-id="c7704-142">**createtable.php**: Creates the MySQL table for the application.</span></span> <span data-ttu-id="c7704-143">Ezt a fájlt csak egyszer használható.</span><span class="sxs-lookup"><span data-stu-id="c7704-143">This file will only be used once.</span></span>

<span data-ttu-id="c7704-144">Felépítéséhez és az alkalmazás futtatása helyben, kövesse az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="c7704-144">To build and run the app locally, follow the steps below.</span></span> <span data-ttu-id="c7704-145">Vegye figyelembe, hogy ezek a lépések feltételezik, PHP, a MySQL és egy webkiszolgáló, állítsa be a helyi számítógépen, és engedélyezte a [MySQL OEM kiterjesztése][pdo-mysql].</span><span class="sxs-lookup"><span data-stu-id="c7704-145">Note that these steps assume you have PHP, MySQL, and a web server set up on your local machine, and that you have enabled the [PDO extension for MySQL][pdo-mysql].</span></span>

1. <span data-ttu-id="c7704-146">Hozzon létre egy MySQL-adatbázis nevű `registration`.</span><span class="sxs-lookup"><span data-stu-id="c7704-146">Create a MySQL database called `registration`.</span></span> <span data-ttu-id="c7704-147">Ez a MySQL-parancssorból, ezzel a paranccsal teheti meg:</span><span class="sxs-lookup"><span data-stu-id="c7704-147">You can do this from the MySQL command prompt with this command:</span></span>
   
        mysql> create database registration;
2. <span data-ttu-id="c7704-148">A webkiszolgáló gyökérkönyvtárában, hozzon létre egy nevű `registration` , és hozzon létre két fájlt az - egy `createtable.php` és egy `index.php`.</span><span class="sxs-lookup"><span data-stu-id="c7704-148">In your web server's root directory, create a folder called `registration` and create two files in it - one called `createtable.php` and one called `index.php`.</span></span>
3. <span data-ttu-id="c7704-149">Nyissa meg a `createtable.php` fájlt egy szövegszerkesztőben, vagy IDE, és adja hozzá az alábbi kódot.</span><span class="sxs-lookup"><span data-stu-id="c7704-149">Open the `createtable.php` file in a text editor or IDE and add the code below.</span></span> <span data-ttu-id="c7704-150">Ez a kód létrehozásához használható a `registration_tbl` a tábla a `registration` adatbázis.</span><span class="sxs-lookup"><span data-stu-id="c7704-150">This code will be used to create the `registration_tbl` table in the `registration` database.</span></span>
   
        <?php
        // DB connection info
        $host = "localhost";
        $user = "user name";
        $pwd = "password";
        $db = "registration";
        try{
            $conn = new PDO( "mysql:host=$host;dbname=$db", $user, $pwd);
            $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );
            $sql = "CREATE TABLE registration_tbl(
                        id INT NOT NULL AUTO_INCREMENT, 
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
   
   > [!NOTE]
   > <span data-ttu-id="c7704-151">Értéket kell <code>$user</code> és <code>$pwd</code> a helyi MySQL-felhasználó nevét és jelszavát.</span><span class="sxs-lookup"><span data-stu-id="c7704-151">You will need to update the values for <code>$user</code> and <code>$pwd</code> with your local MySQL user name and password.</span></span>
   > 
   > 
4. <span data-ttu-id="c7704-152">Nyisson meg egy webböngészőt, és keresse meg a [http://localhost/registration/createtable.php][localhost-createtable].</span><span class="sxs-lookup"><span data-stu-id="c7704-152">Open a web browser and browse to [http://localhost/registration/createtable.php][localhost-createtable].</span></span> <span data-ttu-id="c7704-153">Ekkor létrejön az `registration_tbl` tábla az adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="c7704-153">This will create the `registration_tbl` table in the database.</span></span>
5. <span data-ttu-id="c7704-154">Nyissa meg a **index.php** fájlt egy szövegszerkesztőben, vagy IDE, és adja hozzá az alapvető HTML- és CSS kódot (a PHP kód megkapja a későbbi lépésekben) lap.</span><span class="sxs-lookup"><span data-stu-id="c7704-154">Open the **index.php** file in a text editor or IDE and add the basic HTML and CSS code for the page (the PHP code will be added in later steps).</span></span>
   
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
6. <span data-ttu-id="c7704-155">A PHP címkék belül adja hozzá a PHP kódot az adatbázishoz való kapcsolódáshoz.</span><span class="sxs-lookup"><span data-stu-id="c7704-155">Within the PHP tags, add PHP code for connecting to the database.</span></span>
   
        // DB connection info
        $host = "localhost";
        $user = "user name";
        $pwd = "password";
        $db = "registration";
        // Connect to database.
        try {
            $conn = new PDO( "mysql:host=$host;dbname=$db", $user, $pwd);
            $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );
        }
        catch(Exception $e){
            die(var_dump($e));
        }
   
   > [!NOTE]
   > <span data-ttu-id="c7704-156">Ebben az esetben szüksége lesz a értéket <code>$user</code> és <code>$pwd</code> a helyi MySQL-felhasználó nevét és jelszavát.</span><span class="sxs-lookup"><span data-stu-id="c7704-156">Again, you will need to update the values for <code>$user</code> and <code>$pwd</code> with your local MySQL user name and password.</span></span>
   > 
   > 
7. <span data-ttu-id="c7704-157">Követően az adatbázis-kapcsolat kód adja hozzá a kódot a regisztrációs adatokat beszúrni az adatbázisba.</span><span class="sxs-lookup"><span data-stu-id="c7704-157">Following the database connection code, add code for inserting registration information into the database.</span></span>
   
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
8. <span data-ttu-id="c7704-158">Végezetül a fenti kódot követően adja hozzá a kódot az adatokat az adatbázisból.</span><span class="sxs-lookup"><span data-stu-id="c7704-158">Finally, following the code above, add code for retrieving data from the database.</span></span>
   
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

<span data-ttu-id="c7704-159">Most tallózással megkereshet [http://localhost/registration/index.php] [ localhost-index] az alkalmazás teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="c7704-159">You can now browse to [http://localhost/registration/index.php][localhost-index] to test the app.</span></span>

## <a name="get-mysql-and-ftp-connection-information"></a><span data-ttu-id="c7704-160">MySQL- és FTP-kapcsolati adatok</span><span class="sxs-lookup"><span data-stu-id="c7704-160">Get MySQL and FTP connection information</span></span>
<span data-ttu-id="c7704-161">A MySQL-adatbázis, amely futtatja a Web Apps, a fog csatlakozni kell a kapcsolat adatai.</span><span class="sxs-lookup"><span data-stu-id="c7704-161">To connect to the MySQL database that is running in Web Apps, your will need the connection information.</span></span> <span data-ttu-id="c7704-162">Ahhoz, hogy a MySQL-kapcsolati adatokat, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="c7704-162">To get MySQL connection information, follow these steps:</span></span>

1. <span data-ttu-id="c7704-163">Az app service webalkalmazás panelen kattintson az erőforrás-csoport hivatkozás:</span><span class="sxs-lookup"><span data-stu-id="c7704-163">From the app service web app blade click on the resource group link:</span></span>
   
    ![Válasszon erőforráscsoportot][select-resourcegroup]
2. <span data-ttu-id="c7704-165">Az erőforráscsoport kattintson az adatbázisban:</span><span class="sxs-lookup"><span data-stu-id="c7704-165">From your resource group, click the database:</span></span>
   
    ![Adatbázis kiválasztása][select-database]
3. <span data-ttu-id="c7704-167">Az adatbázisból összefoglaló, válassza ki a **beállítások** > **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="c7704-167">From the database summary, select **Settings** > **Properties**.</span></span>
   
    ![A tulajdonságok kiválasztása][select-properties]
4. <span data-ttu-id="c7704-169">Jegyezze fel az értékei `Database`, `Host`, `User Id`, és `Password`.</span><span class="sxs-lookup"><span data-stu-id="c7704-169">Make note of the values for `Database`, `Host`, `User Id`, and `Password`.</span></span>
   
    ![Megjegyzés: Tulajdonságok][note-properties]
5. <span data-ttu-id="c7704-171">A webalkalmazás, kattintson a **közzétételi profil letöltése** hivatkozásra, a jobb alsó sarkában az oldalon:</span><span class="sxs-lookup"><span data-stu-id="c7704-171">From your web app, click the **Download publish profile** link at the bottom right corner of the page:</span></span>
   
    ![Közzétételi profil letöltése][download-publish-profile]
6. <span data-ttu-id="c7704-173">Nyissa meg a `.publishsettings` fájlt egy XML-szerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="c7704-173">Open the `.publishsettings` file in an XML editor.</span></span> 
7. <span data-ttu-id="c7704-174">Keresés a `<publishProfile >` elem `publishMethod="FTP"` , amelyek a következőhöz hasonló:</span><span class="sxs-lookup"><span data-stu-id="c7704-174">Find the `<publishProfile >` element with `publishMethod="FTP"` that looks similar to this:</span></span>
   
        <publishProfile publishMethod="FTP" publishUrl="ftp://[mysite].azurewebsites.net/site/wwwroot" ftpPassiveMode="True" userName="[username]" userPWD="[password]" destinationAppUrl="http://[name].antdf0.antares-test.windows-int.net" 
            ...
        </publishProfile>

<span data-ttu-id="c7704-175">Jegyezze fel a `publishUrl`, `userName`, és `userPWD` attribútumok.</span><span class="sxs-lookup"><span data-stu-id="c7704-175">Make note of the `publishUrl`, `userName`, and `userPWD` attributes.</span></span>

## <a name="publish-your-app"></a><span data-ttu-id="c7704-176">Az alkalmazás közzététele</span><span class="sxs-lookup"><span data-stu-id="c7704-176">Publish your app</span></span>
<span data-ttu-id="c7704-177">Miután ellenőrizte, hogy az alkalmazást helyileg, közzéteheti a webalkalmazáshoz FTP használatával.</span><span class="sxs-lookup"><span data-stu-id="c7704-177">After you have tested your app locally, you can publish it to your web app using FTP.</span></span> <span data-ttu-id="c7704-178">Azonban először az alkalmazást az adatbázis-kapcsolat adatainak frissítése.</span><span class="sxs-lookup"><span data-stu-id="c7704-178">However, you first need to update the database connection information in the application.</span></span> <span data-ttu-id="c7704-179">Korábban beszerzett adatbázis kapcsolati információk használatával (a a **MySQL beszerzése és az FTP-kapcsolati adatokat** szakaszban), a következő információinak frissítése a **is** a `createdatabase.php` és `index.php` fájlokat a megfelelő értékekkel:</span><span class="sxs-lookup"><span data-stu-id="c7704-179">Using the database connection information you obtained earlier (in the **Get MySQL and FTP connection information** section), update the following information in **both** the `createdatabase.php` and `index.php` files with the appropriate values:</span></span>

    // DB connection info
    $host = "value of Data Source";
    $user = "value of User Id";
    $pwd = "value of Password";
    $db = "value of Database";

<span data-ttu-id="c7704-180">Most már készen áll az FTP használó alkalmazás közzététele.</span><span class="sxs-lookup"><span data-stu-id="c7704-180">Now you are ready to publish your app using FTP.</span></span>

1. <span data-ttu-id="c7704-181">Nyissa meg az FTP-ügyfél választott.</span><span class="sxs-lookup"><span data-stu-id="c7704-181">Open your FTP client of choice.</span></span>
2. <span data-ttu-id="c7704-182">Adja meg a *állomásnév* a a `publishUrl` az FTP-ügyfél a fent leírt attribútum.</span><span class="sxs-lookup"><span data-stu-id="c7704-182">Enter the *host name portion* from the `publishUrl` attribute you noted above into your FTP client.</span></span>
3. <span data-ttu-id="c7704-183">Adja meg a `userName` és `userPWD` fent leírt attribútumok be az FTP-ügyfél nem változik.</span><span class="sxs-lookup"><span data-stu-id="c7704-183">Enter the `userName` and `userPWD` attributes you noted above unchanged into your FTP client.</span></span>
4. <span data-ttu-id="c7704-184">A kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="c7704-184">Establish a connection.</span></span>

<span data-ttu-id="c7704-185">Kapcsolódás után lesz, a fájlok feltöltését és letöltését igény szerint.</span><span class="sxs-lookup"><span data-stu-id="c7704-185">After you have connected you will be able to upload and download files as needed.</span></span> <span data-ttu-id="c7704-186">Ne feledje, hogy feltölteni kívánt fájlokat a gyökérkönyvtárba, amely `/site/wwwroot`.</span><span class="sxs-lookup"><span data-stu-id="c7704-186">Be sure that you are uploading files to the root directory, which is `/site/wwwroot`.</span></span>

<span data-ttu-id="c7704-187">Mindkét feltöltése után `index.php` és `createtable.php`, keresse meg a **http://[site name].azurewebsites.net/createtable.php** MySQL tábla az alkalmazás létrehozása, majd keresse meg **http://[site name]. azurewebsites.NET/index.php** az alkalmazás használatának megkezdéséhez.</span><span class="sxs-lookup"><span data-stu-id="c7704-187">After uploading both `index.php` and `createtable.php`, browse to **http://[site name].azurewebsites.net/createtable.php** to create the MySQL table for the application, then browse to **http://[site name].azurewebsites.net/index.php** to begin using the application.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c7704-188">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c7704-188">Next steps</span></span>
<span data-ttu-id="c7704-189">További információkért lásd: a [PHP fejlesztői központ](/develop/php/).</span><span class="sxs-lookup"><span data-stu-id="c7704-189">For more information, see the [PHP Developer Center](/develop/php/).</span></span>

[install-php]: http://www.php.net/manual/en/install.php
[install-mysql]: http://dev.mysql.com/doc/refman/5.6/en/installing.html
[pdo-mysql]: http://www.php.net/manual/en/ref.pdo-mysql.php
[localhost-createtable]: http://localhost/tasklist/createtable.php
[localhost-index]: http://localhost/tasklist/index.php
[running-app]: ./media/web-sites-php-mysql-deploy-use-ftp/running_app_2.png
[new-website]: ./media/web-sites-php-mysql-deploy-use-ftp/new_website2.png
[custom-create]: ./media/web-sites-php-mysql-deploy-use-ftp/create_web_mysql.png
[website-details]: ./media/web-sites-php-web-site-mysql-deploy-use-ftp/website_details.jpg
[new-mysql-db]: ./media/web-sites-php-mysql-deploy-use-ftp/create_db.png
[go-to-webapp]: ./media/web-sites-php-mysql-deploy-use-ftp/select_webapp.png
[set-deployment-credentials]: ./media/web-sites-php-mysql-deploy-use-ftp/set_credentials.png
[portal-ftp-username-password]: ./media/web-sites-php-mysql-deploy-use-ftp/save_credentials.png
[resource-group]: ./media/web-sites-php-mysql-deploy-use-ftp/set_group.png
[new-web-app]: ./media/web-sites-php-mysql-deploy-use-ftp/create_wa.png
[select-database]: ./media/web-sites-php-mysql-deploy-use-ftp/select_database.png
[select-resourcegroup]: ./media/web-sites-php-mysql-deploy-use-ftp/select_resourcegroup.png
[select-properties]: ./media/web-sites-php-mysql-deploy-use-ftp/select_properties.png
[note-properties]: ./media/web-sites-php-mysql-deploy-use-ftp/note-properties.png

[connection-string-info]: ./media/web-sites-php-web-site-mysql-deploy-use-ftp/connection_string_info.png
[management-portal]: https://portal.azure.com
[download-publish-profile]: ./media/web-sites-php-mysql-deploy-use-ftp/download_publish_profile_3.png

