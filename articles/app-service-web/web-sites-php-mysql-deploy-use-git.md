---
title: "PHP-MySQL webes alkalmazás létrehozása az Azure App Service-ben és üzembe helyezése a Git használatával"
description: "Ez az oktatóanyag bemutatja, hogyan kell, amely tárolja az adatokat a MySQL PHP-webalkalmazás létrehozása és használata az Azure Git-telepítés."
services: app-service\web
documentationcenter: php
author: rmcmurray
manager: erikre
editor: 
tags: mysql
ms.assetid: 7454475f-e275-4bc7-9f09-1ef07382e5da
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: aa845eb474dbd42ae2c31880690d4ced059eb448
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-php-mysql-web-app-in-azure-app-service-and-deploy-using-git"></a><span data-ttu-id="eeef7-103">PHP-MySQL webes alkalmazás létrehozása az Azure App Service-ben és üzembe helyezése a Git használatával</span><span class="sxs-lookup"><span data-stu-id="eeef7-103">Create a PHP-MySQL web app in Azure App Service and deploy using Git</span></span>
<span data-ttu-id="eeef7-104">Az oktatóanyag bemutatja, hogyan PHP-MySQL-webalkalmazás létrehozása és központi telepítése úgy, hogy [App Service](http://go.microsoft.com/fwlink/?LinkId=529714) Git használatával.</span><span class="sxs-lookup"><span data-stu-id="eeef7-104">This tutorial shows you how to create a PHP-MySQL web app and how to deploy it to [App Service](http://go.microsoft.com/fwlink/?LinkId=529714) using Git.</span></span> <span data-ttu-id="eeef7-105">Használandó [PHP][install-php], a MySQL parancssori eszköz (része [MySQL][install-mysql]), és [Git] [ install-git] telepítve a számítógépre.</span><span class="sxs-lookup"><span data-stu-id="eeef7-105">You will use [PHP][install-php], the MySQL Command-Line Tool (part of [MySQL][install-mysql]), and [Git][install-git] installed on your computer.</span></span> <span data-ttu-id="eeef7-106">Ez az oktatóanyag utasításai követhetők bármely operációs rendszeren, beleértve a Windows, Mac és Linux.</span><span class="sxs-lookup"><span data-stu-id="eeef7-106">The instructions in this tutorial can be followed on any operating system, including Windows, Mac, and  Linux.</span></span> <span data-ttu-id="eeef7-107">Ez az útmutató befejezése után fog egy PHP/MySQL az Azure-ban futó webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="eeef7-107">Upon completing this guide, you will have a PHP/MySQL web app running in Azure.</span></span>

<span data-ttu-id="eeef7-108">Az oktatóanyagban érintett témák köre:</span><span class="sxs-lookup"><span data-stu-id="eeef7-108">You will learn:</span></span>

* <span data-ttu-id="eeef7-109">A webes alkalmazás és MySQL adatbázis használatával létrehozása a [Azure Portal][management-portal].</span><span class="sxs-lookup"><span data-stu-id="eeef7-109">How to create a web app and a MySQL database using the [Azure Portal][management-portal].</span></span> <span data-ttu-id="eeef7-110">Mivel a PHP nincs engedélyezve az [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714) alapértelmezés szerint nem különleges futtatásához szükséges a PHP-kódot.</span><span class="sxs-lookup"><span data-stu-id="eeef7-110">Because PHP is enabled in [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714) by default, nothing special is required to run your PHP code.</span></span>
* <span data-ttu-id="eeef7-111">Hogyan közzététele az Azure Git használatával az alkalmazás közzétételét.</span><span class="sxs-lookup"><span data-stu-id="eeef7-111">How to publish and re-publish your application to Azure using Git.</span></span>
* <span data-ttu-id="eeef7-112">Feladatok automatizálásához szerkesztő Composer bővítményt engedélyezése minden `git push`.</span><span class="sxs-lookup"><span data-stu-id="eeef7-112">How to enable the Composer extension to automate Composer tasks at every `git push`.</span></span>

<span data-ttu-id="eeef7-113">Az oktatóanyag utasításait követve egy egyszerű regisztrációs webalkalmazás PHP fog létrehozni.</span><span class="sxs-lookup"><span data-stu-id="eeef7-113">By following this tutorial, you will build a simple registration web app in PHP.</span></span> <span data-ttu-id="eeef7-114">Az alkalmazás üzemel a webalkalmazásokban.</span><span class="sxs-lookup"><span data-stu-id="eeef7-114">The application will be hosted in Web Apps.</span></span> <span data-ttu-id="eeef7-115">A kész alkalmazás képernyőfelvételének alatt van:</span><span class="sxs-lookup"><span data-stu-id="eeef7-115">A screenshot of the completed application is below:</span></span>

![Az Azure PHP-webhely][running-app]

## <a name="set-up-the-development-environment"></a><span data-ttu-id="eeef7-117">A fejlesztési környezet kialakítása</span><span class="sxs-lookup"><span data-stu-id="eeef7-117">Set up the development environment</span></span>
<span data-ttu-id="eeef7-118">Ez az oktatóanyag feltételezi, hogy rendelkezik [PHP][install-php], a MySQL parancssori eszköz (része [MySQL][install-mysql]), és [Git] [ install-git] telepítve a számítógépre.</span><span class="sxs-lookup"><span data-stu-id="eeef7-118">This tutorial assumes you have [PHP][install-php], the MySQL Command-Line Tool (part of [MySQL][install-mysql]), and [Git][install-git] installed on your computer.</span></span>

<a id="create-web-site-and-set-up-git"></a>

## <a name="create-a-web-app-and-set-up-git-publishing"></a><span data-ttu-id="eeef7-119">Webalkalmazás létrehozása és beállítása a Git-közzététel</span><span class="sxs-lookup"><span data-stu-id="eeef7-119">Create a web app and set up Git publishing</span></span>
<span data-ttu-id="eeef7-120">Kövesse az alábbi lépéseket a webes alkalmazás és MySQL-adatbázis létrehozásához:</span><span class="sxs-lookup"><span data-stu-id="eeef7-120">Follow these steps to create a web app and a MySQL database:</span></span>

1. <span data-ttu-id="eeef7-121">Jelentkezzen be a [Azure-portálon][management-portal].</span><span class="sxs-lookup"><span data-stu-id="eeef7-121">Login to the [Azure Portal][management-portal].</span></span>
2. <span data-ttu-id="eeef7-122">Kattintson a **új** ikonra.</span><span class="sxs-lookup"><span data-stu-id="eeef7-122">Click the **New** icon.</span></span>
3. <span data-ttu-id="eeef7-123">Kattintson a **tekintse meg az összes** melletti **piactér**.</span><span class="sxs-lookup"><span data-stu-id="eeef7-123">Click **See All** next to **Marketplace**.</span></span> 
4. <span data-ttu-id="eeef7-124">Kattintson a **Web + mobil**, majd **webes alkalmazás + MySQL**.</span><span class="sxs-lookup"><span data-stu-id="eeef7-124">Click **Web + Mobile**, then **Web app + MySQL**.</span></span> <span data-ttu-id="eeef7-125">Ezt követően kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="eeef7-125">Then, click **Create**.</span></span>
5. <span data-ttu-id="eeef7-126">Adjon meg egy érvényes nevet az erőforráscsoport számára.</span><span class="sxs-lookup"><span data-stu-id="eeef7-126">Enter a valid name for your resource group.</span></span>
   
    ![Állítsa az erőforráscsoport neve][resource-group]
6. <span data-ttu-id="eeef7-128">Adja meg az új webes alkalmazás értékeit.</span><span class="sxs-lookup"><span data-stu-id="eeef7-128">Enter values for your new web app.</span></span>
   
    ![Webalkalmazás létrehozása][new-web-app]
7. <span data-ttu-id="eeef7-130">Adja meg az új adatbázis, beleértve a jogi feltételek elfogadását jelző mezőt.</span><span class="sxs-lookup"><span data-stu-id="eeef7-130">Enter values for your new database, including agreeing to the legal terms.</span></span>
   
    ![Új MySQL-adatbázis létrehozása][new-mysql-db]
8. <span data-ttu-id="eeef7-132">Ha a webalkalmazás létrejött, látni fogja az új webalkalmazás panelen.</span><span class="sxs-lookup"><span data-stu-id="eeef7-132">When the web app has been created, you will see the new web app blade.</span></span>
9. <span data-ttu-id="eeef7-133">A **beállítások** kattintson a **folyamatos üzembe helyezés**, majd kattintson a *kötelező beállítások konfigurálása*.</span><span class="sxs-lookup"><span data-stu-id="eeef7-133">In **Settings** click on **Continuous Deployment**, then click on *Configure required settings*.</span></span>
   
    ![Git-közzététel beállítása][setup-publishing]
10. <span data-ttu-id="eeef7-135">Válassza ki **helyi Git-tárház** adatforrásra vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="eeef7-135">Select **Local Git Repository** for the source.</span></span>
    
     ![Git-tárház beállítása][setup-repository]
11. <span data-ttu-id="eeef7-137">Git-közzététel engedélyezéséhez meg kell adnia egy felhasználónevet és jelszót.</span><span class="sxs-lookup"><span data-stu-id="eeef7-137">To enable Git publishing, you must provide a user name and password.</span></span> <span data-ttu-id="eeef7-138">Jegyezze fel a felhasználónevet és jelszót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="eeef7-138">Make a note of the user name and password you create.</span></span> <span data-ttu-id="eeef7-139">(Ha egy Git-tárház előtt beállítását követően ez a lépés kimarad.)</span><span class="sxs-lookup"><span data-stu-id="eeef7-139">(If you have set up a Git repository before, this step will be skipped.)</span></span>
    
     ![Közzétételi hitelesítő adatok létrehozása][credentials]

## <a name="get-remote-mysql-connection-information"></a><span data-ttu-id="eeef7-141">Távoli MySQL kapcsolatadatok beolvasása</span><span class="sxs-lookup"><span data-stu-id="eeef7-141">Get remote MySQL connection information</span></span>
<span data-ttu-id="eeef7-142">A MySQL-adatbázis, amely futtatja a Web Apps, a fog csatlakozni kell a kapcsolat adatai.</span><span class="sxs-lookup"><span data-stu-id="eeef7-142">To connect to the MySQL database that is running in Web Apps, your will need the connection information.</span></span> <span data-ttu-id="eeef7-143">Ahhoz, hogy a MySQL-kapcsolati adatokat, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="eeef7-143">To get MySQL connection information, follow these steps:</span></span>

1. <span data-ttu-id="eeef7-144">Az erőforráscsoport kattintson az adatbázisban:</span><span class="sxs-lookup"><span data-stu-id="eeef7-144">From your resource group, click the database:</span></span>
   
    ![Adatbázis kiválasztása][select-database]
2. <span data-ttu-id="eeef7-146">Az adatbázisból **beállítások**, jelölje be **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="eeef7-146">From the database **Settings**, select **Properties**.</span></span>
   
    ![A tulajdonságok kiválasztása][select-properties]
3. <span data-ttu-id="eeef7-148">Jegyezze fel az értékei `Database`, `Host`, `User Id`, és `Password`.</span><span class="sxs-lookup"><span data-stu-id="eeef7-148">Make note of the values for `Database`, `Host`, `User Id`, and `Password`.</span></span>
   
    ![Megjegyzés: Tulajdonságok][note-properties]

## <a name="build-and-test-your-app-locally"></a><span data-ttu-id="eeef7-150">És az alkalmazás helyi teszteléséhez</span><span class="sxs-lookup"><span data-stu-id="eeef7-150">Build and test your app locally</span></span>
<span data-ttu-id="eeef7-151">Most, hogy létrehozott egy webalkalmazást, az alkalmazás helyi fejleszthet, majd telepítheti azt a tesztelés után.</span><span class="sxs-lookup"><span data-stu-id="eeef7-151">Now that you have created a web app, you can develop your application locally, then deploy it after testing.</span></span>

<span data-ttu-id="eeef7-152">A regisztrációs alkalmazás egy egyszerű PHP-alkalmazás, amely lehetővé teszi, hogy regisztrálja az esemény a név és e-mail cím megadásával.</span><span class="sxs-lookup"><span data-stu-id="eeef7-152">The Registration application is a simple PHP application that allows you to register for an event by providing your name and email address.</span></span> <span data-ttu-id="eeef7-153">Előző igénylők kapcsolatos információkat a táblázatban jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="eeef7-153">Information about previous registrants is displayed in a table.</span></span> <span data-ttu-id="eeef7-154">Regisztrációs adatokat a MySQL-adatbázisban tárolódik.</span><span class="sxs-lookup"><span data-stu-id="eeef7-154">Registration information is stored in a MySQL database.</span></span> <span data-ttu-id="eeef7-155">Az alkalmazás tartalmaz egy fájl (másolás/beillesztés kód alatt érhető el):</span><span class="sxs-lookup"><span data-stu-id="eeef7-155">The application consists of one file (copy/paste code available below):</span></span>

* <span data-ttu-id="eeef7-156">**index.php**: regisztráció és a bejegyzés adatokat tartalmazó táblát űrlap jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="eeef7-156">**index.php**: Displays a form for registration and a table containing registrant information.</span></span>

<span data-ttu-id="eeef7-157">Építsenek, és futtassa az alkalmazást helyileg, kövesse az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="eeef7-157">To build and run the application locally, follow the steps below.</span></span> <span data-ttu-id="eeef7-158">Vegye figyelembe, hogy ezek a lépések feltételezik, a PHP és a MySQL parancssori eszköz (MySQL része) állítsa be a helyi számítógépen, és engedélyezte a [MySQL OEM kiterjesztése][pdo-mysql].</span><span class="sxs-lookup"><span data-stu-id="eeef7-158">Note that these steps assume you have the PHP and MySQL Command-Line Tool (part of MySQL) set up on your local machine, and that you have enabled the [PDO extension for MySQL][pdo-mysql].</span></span>

1. <span data-ttu-id="eeef7-159">A távoli MySQL kiszolgálóhoz csatlakoznak, a értéke `Data Source`, `User Id`, `Password`, és `Database` , amely korábban kapott:</span><span class="sxs-lookup"><span data-stu-id="eeef7-159">Connect to the remote MySQL server, using the value for `Data Source`, `User Id`, `Password`, and `Database` that you retrieved earlier:</span></span>
   
        mysql -h{Data Source] -u[User Id] -p[Password] -D[Database]
2. <span data-ttu-id="eeef7-160">A MySQL parancssor jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="eeef7-160">The MySQL command prompt will appear:</span></span>
   
        mysql>
3. <span data-ttu-id="eeef7-161">Illessze be a következő `CREATE TABLE` parancs futtatásával hozzon létre a `registration_tbl` táblát az adatbázisban:</span><span class="sxs-lookup"><span data-stu-id="eeef7-161">Paste in the following `CREATE TABLE` command to create the `registration_tbl` table in your database:</span></span>
   
        CREATE TABLE registration_tbl(id INT NOT NULL AUTO_INCREMENT, PRIMARY KEY(id), name VARCHAR(30), email VARCHAR(30), date DATE);
4. <span data-ttu-id="eeef7-162">Hozzon létre a helyi alkalmazások mappa gyökeréhez **index.php** fájlt.</span><span class="sxs-lookup"><span data-stu-id="eeef7-162">In the root of your local application folder create **index.php** file.</span></span>
5. <span data-ttu-id="eeef7-163">Nyissa meg a **index.php** szöveg- vagy IDE fájlt, és adja hozzá a következő kódot, és végezze el a szükséges módosításokat jelölésű `//TODO:` megjegyzések.</span><span class="sxs-lookup"><span data-stu-id="eeef7-163">Open the **index.php** file in a text editor or IDE and add the following code, and complete the necessary changes marked with `//TODO:` comments.</span></span>

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
            // DB connection info
            //TODO: Update the values for $host, $user, $pwd, and $db
            //using the values you retrieved earlier from the Azure Portal.
            $host = "value of Data Source";
            $user = "value of User Id";
            $pwd = "value of Password";
            $db = "value of Database";
            // Connect to database.
            try {
                $conn = new PDO( "mysql:host=$host;dbname=$db", $user, $pwd);
                $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );
            }
            catch(Exception $e){
                die(var_dump($e));
            }
            // Insert registration info
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
            // Retrieve data
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
        ?>
        </body>
        </html>

1. <span data-ttu-id="eeef7-164">A terminálban az alkalmazás mappájában navigáljon, és írja be a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="eeef7-164">In a terminal go to your application folder and type the following command:</span></span>
   
       php -S localhost:8000

<span data-ttu-id="eeef7-165">Most tallózással megkereshet **http://localhost:8000 /** kell tesztelni az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="eeef7-165">You can now browse to **http://localhost:8000/** to test the application.</span></span>

## <a name="publish-your-app"></a><span data-ttu-id="eeef7-166">Az alkalmazás közzététele</span><span class="sxs-lookup"><span data-stu-id="eeef7-166">Publish your app</span></span>
<span data-ttu-id="eeef7-167">Miután ellenőrizte, hogy az alkalmazást helyileg, közzéteheti a Git használatával webalkalmazások.</span><span class="sxs-lookup"><span data-stu-id="eeef7-167">After you have tested your app locally, you can publish it to Web Apps using Git.</span></span> <span data-ttu-id="eeef7-168">A rendszer a helyi Git-tárház inicializálása, és tegye közzé az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="eeef7-168">You will initialize your local Git repository and publish the application.</span></span>

> [!NOTE]
> <span data-ttu-id="eeef7-169">Ezek a ugyanazokat a lépéseket az Azure portálon végén a webalkalmazás létrehozása és a készlet be Git-közzététel a fenti szakaszban látható.</span><span class="sxs-lookup"><span data-stu-id="eeef7-169">These are the same steps shown in the Azure Portal at the end of the Create a web app and Set up Git Publishing section above.</span></span>
> 
> 

1. <span data-ttu-id="eeef7-170">(Választható)  Ha elfelejti vagy rossz helyen lévő a Git távoli repostitory URL-cím, keresse meg a webes alkalmazás tulajdonságainak az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="eeef7-170">(Optional)  If you've forgotten or misplaced your Git remote repostitory URL, navigate to the web app properties on the Azure Portal.</span></span>
2. <span data-ttu-id="eeef7-171">Nyissa meg a GitBash (vagy egy terminált, ha Git a `PATH`), módosítsa a könyvtárat az alkalmazás gyökérkönyvtárában, és futtassa a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="eeef7-171">Open GitBash (or a terminal, if Git is in your `PATH`), change directories to the root directory of your application, and run the following commands:</span></span>
   
        git init
        git add .
        git commit -m "initial commit"
        git remote add azure [URL for remote repository]
        git push azure master
   
    <span data-ttu-id="eeef7-172">Fogja kérni fogja a korábban létrehozott jelszót.</span><span class="sxs-lookup"><span data-stu-id="eeef7-172">You will be prompted for the password you created earlier.</span></span>
   
    ![Az Azure Git keresztül kezdeti leküldéses][git-initial-push]
3. <span data-ttu-id="eeef7-174">Keresse meg a **http://[site name].azurewebsites.net/index.php** (ezt az információt fogja tárolni a fiók irányítópult) az alkalmazás használatának megkezdéséhez:</span><span class="sxs-lookup"><span data-stu-id="eeef7-174">Browse to **http://[site name].azurewebsites.net/index.php** to begin using the application (this information will be stored on your account dashboard):</span></span>
   
    ![Az Azure PHP-webhely][running-app]

<span data-ttu-id="eeef7-176">Miután közzétette az alkalmazást, annak módosításával kezdődik, és a Git segítségével közzéteszi.</span><span class="sxs-lookup"><span data-stu-id="eeef7-176">After you have published your app, you can begin making changes to it and use Git to publish them.</span></span>

## <a name="publish-changes-to-your-app"></a><span data-ttu-id="eeef7-177">Módosítások az alkalmazás közzététele</span><span class="sxs-lookup"><span data-stu-id="eeef7-177">Publish changes to your app</span></span>
<span data-ttu-id="eeef7-178">A módosítások az alkalmazásban való közzétételéhez kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="eeef7-178">To publish changes to your app, follow these steps:</span></span>

1. <span data-ttu-id="eeef7-179">Az alkalmazás helyileg módosítja.</span><span class="sxs-lookup"><span data-stu-id="eeef7-179">Make changes to your app locally.</span></span>
2. <span data-ttu-id="eeef7-180">Nyissa meg a GitBash (vagy a Terminálszolgáltatások informatikai Git van a `PATH`), módosítsa a könyvtárat az alkalmazás gyökérkönyvtárában, és futtassa a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="eeef7-180">Open GitBash (or a terminal, it Git is in your `PATH`), change directories to the root directory of your app, and run the following commands:</span></span>
   
        git add .
        git commit -m "comment describing changes"
        git push azure master
   
    <span data-ttu-id="eeef7-181">Fogja kérni fogja a korábban létrehozott jelszót.</span><span class="sxs-lookup"><span data-stu-id="eeef7-181">You will be prompted for the password you created earlier.</span></span>
   
    ![Azure Git keresztül előtte webhely módosításai][git-change-push]
3. <span data-ttu-id="eeef7-183">Keresse meg a **http://[site name].azurewebsites.net/index.php** , az alkalmazást, és esetleg elvégzett módosításokat:</span><span class="sxs-lookup"><span data-stu-id="eeef7-183">Browse to **http://[site name].azurewebsites.net/index.php** to see your app and any changes you may have made:</span></span>
   
    ![Az Azure PHP-webhely][running-app]

> [!NOTE]
> <span data-ttu-id="eeef7-185">Ha az Azure App Service-t az Azure-fiók regisztrálása előtt szeretné kipróbálni, ugorjon [Az Azure App Service kipróbálása](https://azure.microsoft.com/try/app-service/) oldalra. Itt azonnal létrehozhat egy ideiglenes, kezdő szintű webalkalmazást az App Service szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="eeef7-185">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="eeef7-186">Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.</span><span class="sxs-lookup"><span data-stu-id="eeef7-186">No credit cards required; no commitments.</span></span>
> 
> 

<a name="composer"></a>

## <a name="enable-composer-automation-with-the-composer-extension"></a><span data-ttu-id="eeef7-187">Szerkesztő automatizálása a Composer bővítményt engedélyezése</span><span class="sxs-lookup"><span data-stu-id="eeef7-187">Enable Composer automation with the Composer extension</span></span>
<span data-ttu-id="eeef7-188">Alapértelmezés szerint az App Service-ben a git telepítési folyamat nem minden composer.json, ha nincs fiókja, a PHP-projektben.</span><span class="sxs-lookup"><span data-stu-id="eeef7-188">By default, the git deployment process in App Service doesn't do anything with composer.json, if you have one in your PHP project.</span></span> <span data-ttu-id="eeef7-189">Engedélyezheti a feldolgozás során composer.json `git push` Composer bővítményt engedélyezésével.</span><span class="sxs-lookup"><span data-stu-id="eeef7-189">You can enable composer.json processing during `git push` by enabling the Composer extension.</span></span>

1. <span data-ttu-id="eeef7-190">A PHP a webalkalmazás-alkalmazás paneljén a [Azure-portálon][management-portal], kattintson a **eszközök** > **bővítmények**.</span><span class="sxs-lookup"><span data-stu-id="eeef7-190">In your PHP web app's blade in the [Azure portal][management-portal], click **Tools** > **Extensions**.</span></span>
   
    ![Composer bővítményt beállításai][composer-extension-settings]
2. <span data-ttu-id="eeef7-192">Kattintson a **Hozzáadás**, majd kattintson a **szerkesztő**.</span><span class="sxs-lookup"><span data-stu-id="eeef7-192">Click **Add**, then click **Composer**.</span></span>
   
    ![Composer bővítményt hozzáadása][composer-extension-add]
3. <span data-ttu-id="eeef7-194">Kattintson a **OK** jogi feltételek elfogadásának.</span><span class="sxs-lookup"><span data-stu-id="eeef7-194">Click **OK** to accept legal terms.</span></span> <span data-ttu-id="eeef7-195">Kattintson a **OK** újra a adja hozzá a kiterjesztést.</span><span class="sxs-lookup"><span data-stu-id="eeef7-195">Click **OK** again to add the extension.</span></span>
   
    <span data-ttu-id="eeef7-196">A **bővítményeket telepített** panel most megjelenik a Composer bővítményt.</span><span class="sxs-lookup"><span data-stu-id="eeef7-196">The **Installed extensions** blade will now show the Composer extension.</span></span>  
    <span data-ttu-id="eeef7-197">![Composer bővítményt megtekintése][composer-extension-view]</span><span class="sxs-lookup"><span data-stu-id="eeef7-197">![Composer Extension View][composer-extension-view]</span></span>
4. <span data-ttu-id="eeef7-198">Most, hajtsa végre `git add`, `git commit`, és `git push` , például az előző szakaszban.</span><span class="sxs-lookup"><span data-stu-id="eeef7-198">Now, perform `git add`, `git commit`, and `git push` like in the previous section.</span></span> <span data-ttu-id="eeef7-199">Most láthatja, hogy szerkesztő telepíti a composer.json definiált függőségek.</span><span class="sxs-lookup"><span data-stu-id="eeef7-199">You'll now see that Composer is installing dependencies defined in composer.json.</span></span>
   
    ![Composer bővítményt sikeres][composer-extension-success]

## <a name="next-steps"></a><span data-ttu-id="eeef7-201">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="eeef7-201">Next steps</span></span>
<span data-ttu-id="eeef7-202">További információkért lásd: a [PHP fejlesztői központ](/develop/php/).</span><span class="sxs-lookup"><span data-stu-id="eeef7-202">For more information, see the [PHP Developer Center](/develop/php/).</span></span>

<!-- URL List -->

[install-php]: http://www.php.net/manual/en/install.php
[install-SQLExpress]: http://www.microsoft.com/download/details.aspx?id=29062
[install-Drivers]: http://www.microsoft.com/download/details.aspx?id=20098
[install-git]: http://git-scm.com/
[install-mysql]: http://dev.mysql.com/downloads/mysql/
[pdo-mysql]: http://www.php.net/manual/en/ref.pdo-mysql.php
[management-portal]: https://portal.azure.com
[sql-database-editions]: http://msdn.microsoft.com/library/windowsazure/ee621788.aspx

<!-- IMG List -->

[running-app]: ./media/web-sites-php-mysql-deploy-use-git/running_app_2.png
[new-website]: ./media/web-sites-php-mysql-deploy-use-git/new_website2.png
[custom-create]: ./media/web-sites-php-mysql-deploy-use-git/create_web_mysql.png
[new-mysql-db]: ./media/web-sites-php-mysql-deploy-use-git/create_db.png
[go-to-webapp]: ./media/web-sites-php-mysql-deploy-use-git/select_webapp.png
[setup-git-publishing]: ./media/web-sites-php-mysql-deploy-use-git/setup_git_publishing.png
[credentials]: ./media/web-sites-php-mysql-deploy-use-git/save_credentials.png
[resource-group]: ./media/web-sites-php-mysql-deploy-use-git/set_group.png
[new-web-app]: ./media/web-sites-php-mysql-deploy-use-git/create_wa.png
[setup-publishing]: ./media/web-sites-php-mysql-deploy-use-git/setup_deploy.png
[setup-repository]: ./media/web-sites-php-mysql-deploy-use-git/select_local_git.png
[select-database]: ./media/web-sites-php-mysql-deploy-use-git/select_database.png
[select-properties]: ./media/web-sites-php-mysql-deploy-use-git/select_properties.png
[note-properties]: ./media/web-sites-php-mysql-deploy-use-git/note-properties.png

[git-instructions]: ./media/web-sites-php-mysql-deploy-use-git/git-instructions.png
[git-change-push]: ./media/web-sites-php-mysql-deploy-use-git/php-git-change-push.png
[git-initial-push]: ./media/web-sites-php-mysql-deploy-use-git/php-git-initial-push.png

[composer-extension-settings]: ./media/web-sites-php-mysql-deploy-use-git/composer-extension-settings.png
[composer-extension-add]: ./media/web-sites-php-mysql-deploy-use-git/composer-extension-add.png
[composer-extension-view]: ./media/web-sites-php-mysql-deploy-use-git/composer-extension-view.png
[composer-extension-success]: ./media/web-sites-php-mysql-deploy-use-git/composer-extension-success.png
