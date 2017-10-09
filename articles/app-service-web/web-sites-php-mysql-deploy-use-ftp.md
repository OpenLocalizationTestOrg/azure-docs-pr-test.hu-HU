---
title: "aaaCreate egy PHP-beli MySQL az Azure App Service webalkalmazás, és a FTP telepítése"
description: "Ez az oktatóanyag bemutatja, hogyan toocreate a PHP webes alkalmazást, amely tárolja az adatokat a MySQL és -felhasználási FTP telepítési tooAzure."
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
ms.openlocfilehash: 4d3b56a8ac63d0eba0dc0aec1b62e6d12f601bf1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-php-mysql-web-app-in-azure-app-service-and-deploy-using-ftp"></a><span data-ttu-id="c34ac-103">PHP-MySQL webes alkalmazás létrehozása az Azure App Service-ben és üzembe helyezése FTP használatával</span><span class="sxs-lookup"><span data-stu-id="c34ac-103">Create a PHP-MySQL web app in Azure App Service and deploy using FTP</span></span>
<span data-ttu-id="c34ac-104">Az oktatóanyag bemutatja, hogyan toocreate egy PHP-MySQL web app, és hogyan toodeploy FTP használatával.</span><span class="sxs-lookup"><span data-stu-id="c34ac-104">This tutorial shows you how toocreate a PHP-MySQL web app and how toodeploy it using FTP.</span></span> <span data-ttu-id="c34ac-105">Ez az oktatóanyag feltételezi, hogy rendelkezik [PHP][install-php], [MySQL][install-mysql], webkiszolgáló, és az FTP-ügyfél telepítve a számítógépre.</span><span class="sxs-lookup"><span data-stu-id="c34ac-105">This tutorial assumes you have [PHP][install-php], [MySQL][install-mysql], a web server, and an FTP client installed on your computer.</span></span> <span data-ttu-id="c34ac-106">hello az oktatóanyag utasításai követhetők bármelyik operációs rendszeren, beleértve a Windows, Mac és Linux.</span><span class="sxs-lookup"><span data-stu-id="c34ac-106">hello instructions in this tutorial can be followed on any operating system, including Windows, Mac, and  Linux.</span></span> <span data-ttu-id="c34ac-107">Ez az útmutató befejezése után fog egy PHP/MySQL az Azure-ban futó webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="c34ac-107">Upon completing this guide, you will have a PHP/MySQL web app running in Azure.</span></span>

<span data-ttu-id="c34ac-108">Az oktatóanyagban érintett témák köre:</span><span class="sxs-lookup"><span data-stu-id="c34ac-108">You will learn:</span></span>

* <span data-ttu-id="c34ac-109">Hogyan toocreate egy webalkalmazást és a MySQL-adatbázis hello Azure portál használatával.</span><span class="sxs-lookup"><span data-stu-id="c34ac-109">How toocreate a web app and a MySQL database using hello Azure Portal.</span></span> <span data-ttu-id="c34ac-110">A PHP alapértelmezés szerint engedélyezve van a webalkalmazásokban, mert semmi különleges-e szükséges toorun a PHP-kódot.</span><span class="sxs-lookup"><span data-stu-id="c34ac-110">Because PHP is enabled in Web Apps by default, nothing special is required toorun your PHP code.</span></span>
* <span data-ttu-id="c34ac-111">Hogyan toopublish az alkalmazást tooAzure a FTP.</span><span class="sxs-lookup"><span data-stu-id="c34ac-111">How toopublish your application tooAzure using FTP.</span></span>

<span data-ttu-id="c34ac-112">Az oktatóanyag utasításait követve egy egyszerű regisztrációs webalkalmazás PHP fog létrehozni.</span><span class="sxs-lookup"><span data-stu-id="c34ac-112">By following this tutorial, you will build a simple registration web app in PHP.</span></span> <span data-ttu-id="c34ac-113">a webalkalmazásban hello alkalmazás üzemel.</span><span class="sxs-lookup"><span data-stu-id="c34ac-113">hello application will be hosted in a Web App.</span></span> <span data-ttu-id="c34ac-114">A képernyőfelvétel a hello befejeződött alkalmazás alatt van:</span><span class="sxs-lookup"><span data-stu-id="c34ac-114">A screenshot of hello completed application is below:</span></span>

![Az Azure PHP-webhely][running-app]

> [!NOTE]
> <span data-ttu-id="c34ac-116">Ha azt szeretné, hogy az Azure App Service egy olyan fiók regisztrálása előtt lépései tooget, nyissa meg túl[App Service kipróbálása](https://azure.microsoft.com/try/app-service/), ahol azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben.</span><span class="sxs-lookup"><span data-stu-id="c34ac-116">If you want tooget started with Azure App Service before signing up for an account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="c34ac-117">Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.</span><span class="sxs-lookup"><span data-stu-id="c34ac-117">No credit cards required, no commitments.</span></span> 
> 
> 

## <a name="create-a-web-app-and-set-up-ftp-publishing"></a><span data-ttu-id="c34ac-118">Hozzon létre egy webalkalmazást, és állítsa be az FTP-közzététel</span><span class="sxs-lookup"><span data-stu-id="c34ac-118">Create a web app and set up FTP publishing</span></span>
<span data-ttu-id="c34ac-119">A webes alkalmazás és MySQL-adatbázis, kövesse ezeket a lépéseket toocreate:</span><span class="sxs-lookup"><span data-stu-id="c34ac-119">Follow these steps toocreate a web app and a MySQL database:</span></span>

1. <span data-ttu-id="c34ac-120">Bejelentkezési toohello [Azure Portal][management-portal].</span><span class="sxs-lookup"><span data-stu-id="c34ac-120">Login toohello [Azure Portal][management-portal].</span></span>
2. <span data-ttu-id="c34ac-121">Kattintson a hello **+ új** hello felül ikon hello Azure-portálon a bal oldali.</span><span class="sxs-lookup"><span data-stu-id="c34ac-121">Click hello **+ New** icon on hello top left of hello Azure Portal.</span></span>
   
    ![Új Azure-webhely létrehozása][new-website]
3. <span data-ttu-id="c34ac-123">A hello írja be **webes alkalmazás + MySQL** , majd kattintson a **webes alkalmazás + MySQL**.</span><span class="sxs-lookup"><span data-stu-id="c34ac-123">In hello search type **Web app + MySQL** and click on **Web app + MySQL**.</span></span>
   
    ![Egyéni új webhely létrehozása][custom-create]
4. <span data-ttu-id="c34ac-125">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="c34ac-125">Click **Create**.</span></span> <span data-ttu-id="c34ac-126">Adjon meg egy egyedi alkalmazás nevét, egy érvényes nevet a hello erőforráscsoport és egy új service-csomagot.</span><span class="sxs-lookup"><span data-stu-id="c34ac-126">Enter a unique app service name, a valid name for hello resource group and a new service plan.</span></span>
   
    ![Állítsa az erőforráscsoport neve][resource-group]
5. <span data-ttu-id="c34ac-128">Adja meg az értékeket az új adatbázisra, beleértve a elfogadja toohello jogi feltételeket.</span><span class="sxs-lookup"><span data-stu-id="c34ac-128">Enter values for your new database, including agreeing toohello legal terms.</span></span>
   
    ![Új MySQL-adatbázis létrehozása][new-mysql-db]
6. <span data-ttu-id="c34ac-130">Hello webalkalmazás létrehozásakor hello új app service panel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="c34ac-130">When hello web app has been created, you will see hello new app service blade.</span></span>
7. <span data-ttu-id="c34ac-131">Kattintson a **beállítások** > **üzembe helyezési hitelesítő adatok**.</span><span class="sxs-lookup"><span data-stu-id="c34ac-131">Click on **Settings** > **Deployment credentials**.</span></span> 
   
    ![Telepítési hitelesítő adatok beállítása][set-deployment-credentials]
8. <span data-ttu-id="c34ac-133">tooenable FTP-közzététel, meg kell adnia egy felhasználónevet és jelszót.</span><span class="sxs-lookup"><span data-stu-id="c34ac-133">tooenable FTP publishing, you must provide a user name and password.</span></span> <span data-ttu-id="c34ac-134">Hello hitelesítő adatok mentése, és jegyezze fel a hello felhasználónevet és jelszót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="c34ac-134">Save hello credentials and make a note of hello user name and password you create.</span></span>
   
    ![Közzétételi hitelesítő adatok létrehozása][portal-ftp-username-password]

## <a name="build-and-test-your-app-locally"></a><span data-ttu-id="c34ac-136">És az alkalmazás helyi teszteléséhez</span><span class="sxs-lookup"><span data-stu-id="c34ac-136">Build and test your app locally</span></span>
<span data-ttu-id="c34ac-137">hello regisztrációs alkalmazása, amely lehetővé teszi a név és e-mail cím megadásával az esemény tooregister egyszerű PHP-alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="c34ac-137">hello Registration application is a simple PHP application that allows you tooregister for an event by providing your name and email address.</span></span> <span data-ttu-id="c34ac-138">Előző igénylők kapcsolatos információkat a táblázatban jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="c34ac-138">Information about previous registrants is displayed in a table.</span></span> <span data-ttu-id="c34ac-139">Regisztrációs adatokat a MySQL-adatbázisban tárolódik.</span><span class="sxs-lookup"><span data-stu-id="c34ac-139">Registration information is stored in a MySQL database.</span></span> <span data-ttu-id="c34ac-140">hello alkalmazás két fájlból áll:</span><span class="sxs-lookup"><span data-stu-id="c34ac-140">hello app consists of two files:</span></span>

* <span data-ttu-id="c34ac-141">**index.php**: regisztráció és a bejegyzés adatokat tartalmazó táblát űrlap jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="c34ac-141">**index.php**: Displays a form for registration and a table containing registrant information.</span></span>
* <span data-ttu-id="c34ac-142">**createtable.php**: hello alkalmazás hello MySQL táblát hoz létre.</span><span class="sxs-lookup"><span data-stu-id="c34ac-142">**createtable.php**: Creates hello MySQL table for hello application.</span></span> <span data-ttu-id="c34ac-143">Ezt a fájlt csak egyszer használható.</span><span class="sxs-lookup"><span data-stu-id="c34ac-143">This file will only be used once.</span></span>

<span data-ttu-id="c34ac-144">toobuild és futtatási hello alkalmazást helyileg, kövesse az alábbi hello lépéseket.</span><span class="sxs-lookup"><span data-stu-id="c34ac-144">toobuild and run hello app locally, follow hello steps below.</span></span> <span data-ttu-id="c34ac-145">Vegye figyelembe, hogy ezek a lépések feltételezik, hogy a PHP, a MySQL és egy webkiszolgáló, állítsa be a helyi számítógépen, és engedélyezte hello [MySQL OEM kiterjesztése][pdo-mysql].</span><span class="sxs-lookup"><span data-stu-id="c34ac-145">Note that these steps assume you have PHP, MySQL, and a web server set up on your local machine, and that you have enabled hello [PDO extension for MySQL][pdo-mysql].</span></span>

1. <span data-ttu-id="c34ac-146">Hozzon létre egy MySQL-adatbázis nevű `registration`.</span><span class="sxs-lookup"><span data-stu-id="c34ac-146">Create a MySQL database called `registration`.</span></span> <span data-ttu-id="c34ac-147">Ezt megteheti a parancssorból hello MySQL ezzel a paranccsal:</span><span class="sxs-lookup"><span data-stu-id="c34ac-147">You can do this from hello MySQL command prompt with this command:</span></span>
   
        mysql> create database registration;
2. <span data-ttu-id="c34ac-148">A webkiszolgáló gyökérkönyvtárában, hozzon létre egy nevű `registration` , és hozzon létre két fájlt az - egy `createtable.php` és egy `index.php`.</span><span class="sxs-lookup"><span data-stu-id="c34ac-148">In your web server's root directory, create a folder called `registration` and create two files in it - one called `createtable.php` and one called `index.php`.</span></span>
3. <span data-ttu-id="c34ac-149">Nyissa meg hello `createtable.php` fájlt egy szövegszerkesztőben, vagy IDE, és adja hozzá az alábbi hello kódot.</span><span class="sxs-lookup"><span data-stu-id="c34ac-149">Open hello `createtable.php` file in a text editor or IDE and add hello code below.</span></span> <span data-ttu-id="c34ac-150">Ez a kód lesz használt toocreate hello `registration_tbl` hello táblájában `registration` adatbázis.</span><span class="sxs-lookup"><span data-stu-id="c34ac-150">This code will be used toocreate hello `registration_tbl` table in hello `registration` database.</span></span>
   
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
   > <span data-ttu-id="c34ac-151">Szüksége lesz a tooupdate hello értékeinek <code>$user</code> és <code>$pwd</code> a helyi MySQL-felhasználó nevét és jelszavát.</span><span class="sxs-lookup"><span data-stu-id="c34ac-151">You will need tooupdate hello values for <code>$user</code> and <code>$pwd</code> with your local MySQL user name and password.</span></span>
   > 
   > 
4. <span data-ttu-id="c34ac-152">Nyisson meg egy webböngészőt, és keresse meg a túl[http://localhost/registration/createtable.php][localhost-createtable].</span><span class="sxs-lookup"><span data-stu-id="c34ac-152">Open a web browser and browse too[http://localhost/registration/createtable.php][localhost-createtable].</span></span> <span data-ttu-id="c34ac-153">Ezzel létrehoz hello `registration_tbl` hello adatbázis táblájában.</span><span class="sxs-lookup"><span data-stu-id="c34ac-153">This will create hello `registration_tbl` table in hello database.</span></span>
5. <span data-ttu-id="c34ac-154">Nyissa meg hello **index.php** fájlt egy szövegszerkesztőben, vagy IDE, és adja hozzá a hello alapvető HTML- és CSS kód hello lap (hello PHP kód megkapja a későbbi lépésekben).</span><span class="sxs-lookup"><span data-stu-id="c34ac-154">Open hello **index.php** file in a text editor or IDE and add hello basic HTML and CSS code for hello page (hello PHP code will be added in later steps).</span></span>
   
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
6. <span data-ttu-id="c34ac-155">Hello PHP címkék, belül adja hozzá a PHP kódot toohello adatbázis csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="c34ac-155">Within hello PHP tags, add PHP code for connecting toohello database.</span></span>
   
        // DB connection info
        $host = "localhost";
        $user = "user name";
        $pwd = "password";
        $db = "registration";
        // Connect toodatabase.
        try {
            $conn = new PDO( "mysql:host=$host;dbname=$db", $user, $pwd);
            $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );
        }
        catch(Exception $e){
            die(var_dump($e));
        }
   
   > [!NOTE]
   > <span data-ttu-id="c34ac-156">Ebben az esetben szüksége lesz tooupdate hello értékeinek <code>$user</code> és <code>$pwd</code> a helyi MySQL-felhasználó nevét és jelszavát.</span><span class="sxs-lookup"><span data-stu-id="c34ac-156">Again, you will need tooupdate hello values for <code>$user</code> and <code>$pwd</code> with your local MySQL user name and password.</span></span>
   > 
   > 
7. <span data-ttu-id="c34ac-157">Következő hello adatbázis kapcsolati kódja vegye fel a regisztrációs adatok beszúrása hello adatbázis kódot.</span><span class="sxs-lookup"><span data-stu-id="c34ac-157">Following hello database connection code, add code for inserting registration information into hello database.</span></span>
   
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
8. <span data-ttu-id="c34ac-158">Végül követően a fenti hello kódot, adja hozzá az adatok lekérdezése hello adatbázis kódját.</span><span class="sxs-lookup"><span data-stu-id="c34ac-158">Finally, following hello code above, add code for retrieving data from hello database.</span></span>
   
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

<span data-ttu-id="c34ac-159">Most már megkeresheti túl[http://localhost/registration/index.php] [ localhost-index] tootest hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="c34ac-159">You can now browse too[http://localhost/registration/index.php][localhost-index] tootest hello app.</span></span>

## <a name="get-mysql-and-ftp-connection-information"></a><span data-ttu-id="c34ac-160">MySQL- és FTP-kapcsolati adatok</span><span class="sxs-lookup"><span data-stu-id="c34ac-160">Get MySQL and FTP connection information</span></span>
<span data-ttu-id="c34ac-161">tooconnect toohello MySQL-adatbázis, amely futtatja a Web Apps, a fog kell hello kapcsolódási információt.</span><span class="sxs-lookup"><span data-stu-id="c34ac-161">tooconnect toohello MySQL database that is running in Web Apps, your will need hello connection information.</span></span> <span data-ttu-id="c34ac-162">tooget MySQL kapcsolati adatokat, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="c34ac-162">tooget MySQL connection information, follow these steps:</span></span>

1. <span data-ttu-id="c34ac-163">Hello app service webalkalmazás panelen kattintson az hello erőforrás csoportjának hivatkozásra:</span><span class="sxs-lookup"><span data-stu-id="c34ac-163">From hello app service web app blade click on hello resource group link:</span></span>
   
    ![Válasszon erőforráscsoportot][select-resourcegroup]
2. <span data-ttu-id="c34ac-165">Az erőforráscsoport kattintson hello adatbázis:</span><span class="sxs-lookup"><span data-stu-id="c34ac-165">From your resource group, click hello database:</span></span>
   
    ![Adatbázis kiválasztása][select-database]
3. <span data-ttu-id="c34ac-167">Hello adatbázis összegzése, válassza ki **beállítások** > **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="c34ac-167">From hello database summary, select **Settings** > **Properties**.</span></span>
   
    ![A tulajdonságok kiválasztása][select-properties]
4. <span data-ttu-id="c34ac-169">Jegyezze fel a hello értékeinek `Database`, `Host`, `User Id`, és `Password`.</span><span class="sxs-lookup"><span data-stu-id="c34ac-169">Make note of hello values for `Database`, `Host`, `User Id`, and `Password`.</span></span>
   
    ![Megjegyzés: Tulajdonságok][note-properties]
5. <span data-ttu-id="c34ac-171">Kattintson a webalkalmazás hello **közzétételi profil letöltése** hivatkozásra, hello jobb alsó sarkában hello lap:</span><span class="sxs-lookup"><span data-stu-id="c34ac-171">From your web app, click hello **Download publish profile** link at hello bottom right corner of hello page:</span></span>
   
    ![Közzétételi profil letöltése][download-publish-profile]
6. <span data-ttu-id="c34ac-173">Nyissa meg hello `.publishsettings` fájlt egy XML-szerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="c34ac-173">Open hello `.publishsettings` file in an XML editor.</span></span> 
7. <span data-ttu-id="c34ac-174">Hello található `<publishProfile >` elem `publishMethod="FTP"` , amelyek a következőhöz hasonló toothis:</span><span class="sxs-lookup"><span data-stu-id="c34ac-174">Find hello `<publishProfile >` element with `publishMethod="FTP"` that looks similar toothis:</span></span>
   
        <publishProfile publishMethod="FTP" publishUrl="ftp://[mysite].azurewebsites.net/site/wwwroot" ftpPassiveMode="True" userName="[username]" userPWD="[password]" destinationAppUrl="http://[name].antdf0.antares-test.windows-int.net" 
            ...
        </publishProfile>

<span data-ttu-id="c34ac-175">Jegyezze fel a hello `publishUrl`, `userName`, és `userPWD` attribútumok.</span><span class="sxs-lookup"><span data-stu-id="c34ac-175">Make note of hello `publishUrl`, `userName`, and `userPWD` attributes.</span></span>

## <a name="publish-your-app"></a><span data-ttu-id="c34ac-176">Az alkalmazás közzététele</span><span class="sxs-lookup"><span data-stu-id="c34ac-176">Publish your app</span></span>
<span data-ttu-id="c34ac-177">Miután ellenőrizte, hogy az alkalmazást helyileg, közzéteheti azt tooyour web app FTP használatával.</span><span class="sxs-lookup"><span data-stu-id="c34ac-177">After you have tested your app locally, you can publish it tooyour web app using FTP.</span></span> <span data-ttu-id="c34ac-178">Azonban először tooupdate hello adatbázis-kapcsolat adatainak hello alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="c34ac-178">However, you first need tooupdate hello database connection information in hello application.</span></span> <span data-ttu-id="c34ac-179">Korábban beszerzett hello adatbázis-kapcsolat adatainak használatával (a hello **MySQL beszerzése és az FTP-kapcsolati adatokat** szakaszban), a következő információkat a frissítés hello **is** hello `createdatabase.php` és `index.php` hello fájlokat a megfelelő értékeket:</span><span class="sxs-lookup"><span data-stu-id="c34ac-179">Using hello database connection information you obtained earlier (in hello **Get MySQL and FTP connection information** section), update hello following information in **both** hello `createdatabase.php` and `index.php` files with hello appropriate values:</span></span>

    // DB connection info
    $host = "value of Data Source";
    $user = "value of User Id";
    $pwd = "value of Password";
    $db = "value of Database";

<span data-ttu-id="c34ac-180">Most már áll toopublish az alkalmazás készen álljon a FTP.</span><span class="sxs-lookup"><span data-stu-id="c34ac-180">Now you are ready toopublish your app using FTP.</span></span>

1. <span data-ttu-id="c34ac-181">Nyissa meg az FTP-ügyfél választott.</span><span class="sxs-lookup"><span data-stu-id="c34ac-181">Open your FTP client of choice.</span></span>
2. <span data-ttu-id="c34ac-182">Adja meg a hello *állomásnév* a hello `publishUrl` az FTP-ügyfél a fent leírt attribútum.</span><span class="sxs-lookup"><span data-stu-id="c34ac-182">Enter hello *host name portion* from hello `publishUrl` attribute you noted above into your FTP client.</span></span>
3. <span data-ttu-id="c34ac-183">Adja meg a hello `userName` és `userPWD` fent leírt attribútumok be az FTP-ügyfél nem változik.</span><span class="sxs-lookup"><span data-stu-id="c34ac-183">Enter hello `userName` and `userPWD` attributes you noted above unchanged into your FTP client.</span></span>
4. <span data-ttu-id="c34ac-184">A kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="c34ac-184">Establish a connection.</span></span>

<span data-ttu-id="c34ac-185">Csatlakozás után tud tooupload kell, és szükség esetén töltse le a fájlokat.</span><span class="sxs-lookup"><span data-stu-id="c34ac-185">After you have connected you will be able tooupload and download files as needed.</span></span> <span data-ttu-id="c34ac-186">Ne feledje, hogy fájlokat toohello gyökérkönyvtár, amely feltölteni kívánt `/site/wwwroot`.</span><span class="sxs-lookup"><span data-stu-id="c34ac-186">Be sure that you are uploading files toohello root directory, which is `/site/wwwroot`.</span></span>

<span data-ttu-id="c34ac-187">Mindkét feltöltése után `index.php` és `createtable.php`, keresse meg a túl**http://[site name].azurewebsites.net/createtable.php** toocreate hello MySQL tábla hello alkalmazáshoz, majd keresse meg a túl**http://[site neve ].azurewebsites.net/index.php** toobegin hello alkalmazás segítségével.</span><span class="sxs-lookup"><span data-stu-id="c34ac-187">After uploading both `index.php` and `createtable.php`, browse too**http://[site name].azurewebsites.net/createtable.php** toocreate hello MySQL table for hello application, then browse too**http://[site name].azurewebsites.net/index.php** toobegin using hello application.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c34ac-188">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c34ac-188">Next steps</span></span>
<span data-ttu-id="c34ac-189">További információkért lásd: hello [PHP fejlesztői központ](/develop/php/).</span><span class="sxs-lookup"><span data-stu-id="c34ac-189">For more information, see hello [PHP Developer Center](/develop/php/).</span></span>

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

