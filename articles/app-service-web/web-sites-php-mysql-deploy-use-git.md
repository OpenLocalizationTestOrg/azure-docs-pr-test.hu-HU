---
title: "aaaCreate egy PHP-MySQL webalkalmazás az Azure App Service-ben, és telepítse a Git használatával"
description: "Ez az oktatóanyag bemutatja, hogyan toocreate a PHP webes alkalmazást, amely tárolja az adatokat a MySQL és a Git telepítési tooAzure használja."
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
ms.openlocfilehash: 9c22946777598cc973cd9dfc8d2a258bd08cc39a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-php-mysql-web-app-in-azure-app-service-and-deploy-using-git"></a><span data-ttu-id="2ba54-103">PHP-MySQL webes alkalmazás létrehozása az Azure App Service-ben és üzembe helyezése a Git használatával</span><span class="sxs-lookup"><span data-stu-id="2ba54-103">Create a PHP-MySQL web app in Azure App Service and deploy using Git</span></span>
<span data-ttu-id="2ba54-104">Az oktatóanyag bemutatja, hogyan toocreate egy PHP-MySQL web app, és hogyan toodeploy azt túl[App Service](http://go.microsoft.com/fwlink/?LinkId=529714) Git használatával.</span><span class="sxs-lookup"><span data-stu-id="2ba54-104">This tutorial shows you how toocreate a PHP-MySQL web app and how toodeploy it too[App Service](http://go.microsoft.com/fwlink/?LinkId=529714) using Git.</span></span> <span data-ttu-id="2ba54-105">Használandó [PHP][install-php], MySQL parancssori eszköz hello (része [MySQL][install-mysql]), és [Git] [ install-git] telepítve a számítógépre.</span><span class="sxs-lookup"><span data-stu-id="2ba54-105">You will use [PHP][install-php], hello MySQL Command-Line Tool (part of [MySQL][install-mysql]), and [Git][install-git] installed on your computer.</span></span> <span data-ttu-id="2ba54-106">hello az oktatóanyag utasításai követhetők bármelyik operációs rendszeren, beleértve a Windows, Mac és Linux.</span><span class="sxs-lookup"><span data-stu-id="2ba54-106">hello instructions in this tutorial can be followed on any operating system, including Windows, Mac, and  Linux.</span></span> <span data-ttu-id="2ba54-107">Ez az útmutató befejezése után fog egy PHP/MySQL az Azure-ban futó webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="2ba54-107">Upon completing this guide, you will have a PHP/MySQL web app running in Azure.</span></span>

<span data-ttu-id="2ba54-108">Az oktatóanyagban érintett témák köre:</span><span class="sxs-lookup"><span data-stu-id="2ba54-108">You will learn:</span></span>

* <span data-ttu-id="2ba54-109">Hogyan toocreate egy webalkalmazást és a MySQL-adatbázis hello segítségével [Azure Portal][management-portal].</span><span class="sxs-lookup"><span data-stu-id="2ba54-109">How toocreate a web app and a MySQL database using hello [Azure Portal][management-portal].</span></span> <span data-ttu-id="2ba54-110">Mivel a PHP nincs engedélyezve az [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714) alapértelmezés szerint a szükséges toorun semmi különleges a PHP-kódot.</span><span class="sxs-lookup"><span data-stu-id="2ba54-110">Because PHP is enabled in [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714) by default, nothing special is required toorun your PHP code.</span></span>
* <span data-ttu-id="2ba54-111">Hogyan toopublish, és újból közzéteszi a Git használatával alkalmazás tooAzure.</span><span class="sxs-lookup"><span data-stu-id="2ba54-111">How toopublish and re-publish your application tooAzure using Git.</span></span>
* <span data-ttu-id="2ba54-112">Hogyan tooenable hello Composer bővítményt tooautomate szerkesztő feladatokat minden `git push`.</span><span class="sxs-lookup"><span data-stu-id="2ba54-112">How tooenable hello Composer extension tooautomate Composer tasks at every `git push`.</span></span>

<span data-ttu-id="2ba54-113">Az oktatóanyag utasításait követve egy egyszerű regisztrációs webalkalmazás PHP fog létrehozni.</span><span class="sxs-lookup"><span data-stu-id="2ba54-113">By following this tutorial, you will build a simple registration web app in PHP.</span></span> <span data-ttu-id="2ba54-114">a webalkalmazásokban hello alkalmazás üzemel.</span><span class="sxs-lookup"><span data-stu-id="2ba54-114">hello application will be hosted in Web Apps.</span></span> <span data-ttu-id="2ba54-115">A képernyőfelvétel a hello befejeződött alkalmazás alatt van:</span><span class="sxs-lookup"><span data-stu-id="2ba54-115">A screenshot of hello completed application is below:</span></span>

![Az Azure PHP-webhely][running-app]

## <a name="set-up-hello-development-environment"></a><span data-ttu-id="2ba54-117">Hello fejlesztési környezet beállítása</span><span class="sxs-lookup"><span data-stu-id="2ba54-117">Set up hello development environment</span></span>
<span data-ttu-id="2ba54-118">Ez az oktatóanyag feltételezi, hogy rendelkezik [PHP][install-php], MySQL parancssori eszköz hello (része [MySQL][install-mysql]), és [Git] [ install-git] telepítve a számítógépre.</span><span class="sxs-lookup"><span data-stu-id="2ba54-118">This tutorial assumes you have [PHP][install-php], hello MySQL Command-Line Tool (part of [MySQL][install-mysql]), and [Git][install-git] installed on your computer.</span></span>

<a id="create-web-site-and-set-up-git"></a>

## <a name="create-a-web-app-and-set-up-git-publishing"></a><span data-ttu-id="2ba54-119">Webalkalmazás létrehozása és beállítása a Git-közzététel</span><span class="sxs-lookup"><span data-stu-id="2ba54-119">Create a web app and set up Git publishing</span></span>
<span data-ttu-id="2ba54-120">A webes alkalmazás és MySQL-adatbázis, kövesse ezeket a lépéseket toocreate:</span><span class="sxs-lookup"><span data-stu-id="2ba54-120">Follow these steps toocreate a web app and a MySQL database:</span></span>

1. <span data-ttu-id="2ba54-121">Bejelentkezési toohello [Azure Portal][management-portal].</span><span class="sxs-lookup"><span data-stu-id="2ba54-121">Login toohello [Azure Portal][management-portal].</span></span>
2. <span data-ttu-id="2ba54-122">Kattintson a hello **új** ikonra.</span><span class="sxs-lookup"><span data-stu-id="2ba54-122">Click hello **New** icon.</span></span>
3. <span data-ttu-id="2ba54-123">Kattintson a **tekintse meg az összes** következő túl**piactér**.</span><span class="sxs-lookup"><span data-stu-id="2ba54-123">Click **See All** next too**Marketplace**.</span></span> 
4. <span data-ttu-id="2ba54-124">Kattintson a **Web + mobil**, majd **webes alkalmazás + MySQL**.</span><span class="sxs-lookup"><span data-stu-id="2ba54-124">Click **Web + Mobile**, then **Web app + MySQL**.</span></span> <span data-ttu-id="2ba54-125">Ezt követően kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="2ba54-125">Then, click **Create**.</span></span>
5. <span data-ttu-id="2ba54-126">Adjon meg egy érvényes nevet az erőforráscsoport számára.</span><span class="sxs-lookup"><span data-stu-id="2ba54-126">Enter a valid name for your resource group.</span></span>
   
    ![Állítsa az erőforráscsoport neve][resource-group]
6. <span data-ttu-id="2ba54-128">Adja meg az új webes alkalmazás értékeit.</span><span class="sxs-lookup"><span data-stu-id="2ba54-128">Enter values for your new web app.</span></span>
   
    ![Webalkalmazás létrehozása][new-web-app]
7. <span data-ttu-id="2ba54-130">Adja meg az értékeket az új adatbázisra, beleértve a elfogadja toohello jogi feltételeket.</span><span class="sxs-lookup"><span data-stu-id="2ba54-130">Enter values for your new database, including agreeing toohello legal terms.</span></span>
   
    ![Új MySQL-adatbázis létrehozása][new-mysql-db]
8. <span data-ttu-id="2ba54-132">Hello webalkalmazás létrehozásakor hello új webalkalmazás panelen jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="2ba54-132">When hello web app has been created, you will see hello new web app blade.</span></span>
9. <span data-ttu-id="2ba54-133">A **beállítások** kattintson a **folyamatos üzembe helyezés**, majd kattintson a *kötelező beállítások konfigurálása*.</span><span class="sxs-lookup"><span data-stu-id="2ba54-133">In **Settings** click on **Continuous Deployment**, then click on *Configure required settings*.</span></span>
   
    ![Git-közzététel beállítása][setup-publishing]
10. <span data-ttu-id="2ba54-135">Válassza ki **helyi Git-tárház** hello adatforrásra vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="2ba54-135">Select **Local Git Repository** for hello source.</span></span>
    
     ![Git-tárház beállítása][setup-repository]
11. <span data-ttu-id="2ba54-137">tooenable Git közzététele, meg kell adnia egy felhasználónevet és jelszót.</span><span class="sxs-lookup"><span data-stu-id="2ba54-137">tooenable Git publishing, you must provide a user name and password.</span></span> <span data-ttu-id="2ba54-138">Jegyezze fel a hello felhasználónevet és jelszót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="2ba54-138">Make a note of hello user name and password you create.</span></span> <span data-ttu-id="2ba54-139">(Ha egy Git-tárház előtt beállítását követően ez a lépés kimarad.)</span><span class="sxs-lookup"><span data-stu-id="2ba54-139">(If you have set up a Git repository before, this step will be skipped.)</span></span>
    
     ![Közzétételi hitelesítő adatok létrehozása][credentials]

## <a name="get-remote-mysql-connection-information"></a><span data-ttu-id="2ba54-141">Távoli MySQL kapcsolatadatok beolvasása</span><span class="sxs-lookup"><span data-stu-id="2ba54-141">Get remote MySQL connection information</span></span>
<span data-ttu-id="2ba54-142">tooconnect toohello MySQL-adatbázis, amely futtatja a Web Apps, a fog kell hello kapcsolódási információt.</span><span class="sxs-lookup"><span data-stu-id="2ba54-142">tooconnect toohello MySQL database that is running in Web Apps, your will need hello connection information.</span></span> <span data-ttu-id="2ba54-143">tooget MySQL kapcsolati adatokat, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="2ba54-143">tooget MySQL connection information, follow these steps:</span></span>

1. <span data-ttu-id="2ba54-144">Az erőforráscsoport kattintson hello adatbázis:</span><span class="sxs-lookup"><span data-stu-id="2ba54-144">From your resource group, click hello database:</span></span>
   
    ![Adatbázis kiválasztása][select-database]
2. <span data-ttu-id="2ba54-146">Hello adatbázisból **beállítások**, jelölje be **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="2ba54-146">From hello database **Settings**, select **Properties**.</span></span>
   
    ![A tulajdonságok kiválasztása][select-properties]
3. <span data-ttu-id="2ba54-148">Jegyezze fel a hello értékeinek `Database`, `Host`, `User Id`, és `Password`.</span><span class="sxs-lookup"><span data-stu-id="2ba54-148">Make note of hello values for `Database`, `Host`, `User Id`, and `Password`.</span></span>
   
    ![Megjegyzés: Tulajdonságok][note-properties]

## <a name="build-and-test-your-app-locally"></a><span data-ttu-id="2ba54-150">És az alkalmazás helyi teszteléséhez</span><span class="sxs-lookup"><span data-stu-id="2ba54-150">Build and test your app locally</span></span>
<span data-ttu-id="2ba54-151">Most, hogy létrehozott egy webalkalmazást, az alkalmazás helyi fejleszthet, majd telepítheti azt a tesztelés után.</span><span class="sxs-lookup"><span data-stu-id="2ba54-151">Now that you have created a web app, you can develop your application locally, then deploy it after testing.</span></span>

<span data-ttu-id="2ba54-152">hello regisztrációs alkalmazása, amely lehetővé teszi a név és e-mail cím megadásával az esemény tooregister egyszerű PHP-alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="2ba54-152">hello Registration application is a simple PHP application that allows you tooregister for an event by providing your name and email address.</span></span> <span data-ttu-id="2ba54-153">Előző igénylők kapcsolatos információkat a táblázatban jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="2ba54-153">Information about previous registrants is displayed in a table.</span></span> <span data-ttu-id="2ba54-154">Regisztrációs adatokat a MySQL-adatbázisban tárolódik.</span><span class="sxs-lookup"><span data-stu-id="2ba54-154">Registration information is stored in a MySQL database.</span></span> <span data-ttu-id="2ba54-155">hello alkalmazás áll egy fájl (másolás/beillesztés kód alatt érhető el):</span><span class="sxs-lookup"><span data-stu-id="2ba54-155">hello application consists of one file (copy/paste code available below):</span></span>

* <span data-ttu-id="2ba54-156">**index.php**: regisztráció és a bejegyzés adatokat tartalmazó táblát űrlap jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="2ba54-156">**index.php**: Displays a form for registration and a table containing registrant information.</span></span>

<span data-ttu-id="2ba54-157">toobuild és futtatási hello alkalmazás helyi, kövesse a hello lépéseket.</span><span class="sxs-lookup"><span data-stu-id="2ba54-157">toobuild and run hello application locally, follow hello steps below.</span></span> <span data-ttu-id="2ba54-158">Vegye figyelembe, hogy ezek a lépések feltételezik hello PHP és a MySQL parancssori eszköz (MySQL része) állítsa be a helyi számítógépen, és, hogy engedélyezte hello [MySQL OEM kiterjesztése][pdo-mysql].</span><span class="sxs-lookup"><span data-stu-id="2ba54-158">Note that these steps assume you have hello PHP and MySQL Command-Line Tool (part of MySQL) set up on your local machine, and that you have enabled hello [PDO extension for MySQL][pdo-mysql].</span></span>

1. <span data-ttu-id="2ba54-159">Csatlakozás toohello távoli MySQL-kiszolgáló hello értéke `Data Source`, `User Id`, `Password`, és `Database` , amely korábban kapott:</span><span class="sxs-lookup"><span data-stu-id="2ba54-159">Connect toohello remote MySQL server, using hello value for `Data Source`, `User Id`, `Password`, and `Database` that you retrieved earlier:</span></span>
   
        mysql -h{Data Source] -u[User Id] -p[Password] -D[Database]
2. <span data-ttu-id="2ba54-160">hello MySQL parancssor jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="2ba54-160">hello MySQL command prompt will appear:</span></span>
   
        mysql>
3. <span data-ttu-id="2ba54-161">Illessze be a következő hello `CREATE TABLE` parancs toocreate hello `registration_tbl` táblát az adatbázisban:</span><span class="sxs-lookup"><span data-stu-id="2ba54-161">Paste in hello following `CREATE TABLE` command toocreate hello `registration_tbl` table in your database:</span></span>
   
        CREATE TABLE registration_tbl(id INT NOT NULL AUTO_INCREMENT, PRIMARY KEY(id), name VARCHAR(30), email VARCHAR(30), date DATE);
4. <span data-ttu-id="2ba54-162">Hozzon létre a helyi alkalmazások mappa gyökeréhez hello **index.php** fájlt.</span><span class="sxs-lookup"><span data-stu-id="2ba54-162">In hello root of your local application folder create **index.php** file.</span></span>
5. <span data-ttu-id="2ba54-163">Nyissa meg hello **index.php** fájlt egy szövegszerkesztőben, vagy IDE, és adja hozzá a hello a következő kódot, és teljes hello a szükséges módosításokat jelölésű `//TODO:` megjegyzések.</span><span class="sxs-lookup"><span data-stu-id="2ba54-163">Open hello **index.php** file in a text editor or IDE and add hello following code, and complete hello necessary changes marked with `//TODO:` comments.</span></span>

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
            // DB connection info
            //TODO: Update hello values for $host, $user, $pwd, and $db
            //using hello values you retrieved earlier from hello Azure Portal.
            $host = "value of Data Source";
            $user = "value of User Id";
            $pwd = "value of Password";
            $db = "value of Database";
            // Connect toodatabase.
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

1. <span data-ttu-id="2ba54-164">Az egy terminál lépjen tooyour alkalmazás mappa és a típus hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="2ba54-164">In a terminal go tooyour application folder and type hello following command:</span></span>
   
       php -S localhost:8000

<span data-ttu-id="2ba54-165">Most már megkeresheti túl**http://localhost:8000 /** tootest hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="2ba54-165">You can now browse too**http://localhost:8000/** tootest hello application.</span></span>

## <a name="publish-your-app"></a><span data-ttu-id="2ba54-166">Az alkalmazás közzététele</span><span class="sxs-lookup"><span data-stu-id="2ba54-166">Publish your app</span></span>
<span data-ttu-id="2ba54-167">Miután ellenőrizte, hogy az alkalmazást helyileg, közzéteheti azt Git tooWeb alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="2ba54-167">After you have tested your app locally, you can publish it tooWeb Apps using Git.</span></span> <span data-ttu-id="2ba54-168">A rendszer a helyi Git-tárház inicializálása, és hello alkalmazás közzététele.</span><span class="sxs-lookup"><span data-stu-id="2ba54-168">You will initialize your local Git repository and publish hello application.</span></span>

> [!NOTE]
> <span data-ttu-id="2ba54-169">A rendszer hello azonos hello hello végén hello hozzon létre egy webalkalmazást az Azure portálon, és állítsa be Git-közzététel a fenti szakaszban bemutatott lépések.</span><span class="sxs-lookup"><span data-stu-id="2ba54-169">These are hello same steps shown in hello Azure Portal at hello end of hello Create a web app and Set up Git Publishing section above.</span></span>
> 
> 

1. <span data-ttu-id="2ba54-170">(Választható)  Ha elfelejti vagy rossz helyen lévő a Git távoli repostitory URL-cím, keresse meg a toohello web app tulajdonságainak hello Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="2ba54-170">(Optional)  If you've forgotten or misplaced your Git remote repostitory URL, navigate toohello web app properties on hello Azure Portal.</span></span>
2. <span data-ttu-id="2ba54-171">Nyissa meg a GitBash (vagy egy terminált, ha Git a `PATH`), módosítsa a könyvtárakat toohello az alkalmazás gyökérkönyvtárában, és futtassa a következő parancsok hello:</span><span class="sxs-lookup"><span data-stu-id="2ba54-171">Open GitBash (or a terminal, if Git is in your `PATH`), change directories toohello root directory of your application, and run hello following commands:</span></span>
   
        git init
        git add .
        git commit -m "initial commit"
        git remote add azure [URL for remote repository]
        git push azure master
   
    <span data-ttu-id="2ba54-172">A korábban létrehozott hello jelszó bekéri.</span><span class="sxs-lookup"><span data-stu-id="2ba54-172">You will be prompted for hello password you created earlier.</span></span>
   
    ![Kezdeti leküldéses tooAzure Git keresztül][git-initial-push]
3. <span data-ttu-id="2ba54-174">Keresse meg a túl**http://[site name].azurewebsites.net/index.php** toobegin hello alkalmazással (ezt az információt fogja tárolni az irányítópulton fiók):</span><span class="sxs-lookup"><span data-stu-id="2ba54-174">Browse too**http://[site name].azurewebsites.net/index.php** toobegin using hello application (this information will be stored on your account dashboard):</span></span>
   
    ![Az Azure PHP-webhely][running-app]

<span data-ttu-id="2ba54-176">Miután közzétette az alkalmazást, módosításokat tooit állapotában, és Git toopublish használja őket.</span><span class="sxs-lookup"><span data-stu-id="2ba54-176">After you have published your app, you can begin making changes tooit and use Git toopublish them.</span></span>

## <a name="publish-changes-tooyour-app"></a><span data-ttu-id="2ba54-177">Módosítások tooyour alkalmazás közzététele</span><span class="sxs-lookup"><span data-stu-id="2ba54-177">Publish changes tooyour app</span></span>
<span data-ttu-id="2ba54-178">toopublish módosítások tooyour alkalmazást, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="2ba54-178">toopublish changes tooyour app, follow these steps:</span></span>

1. <span data-ttu-id="2ba54-179">Módosítások tooyour alkalmazás helyileg tétele.</span><span class="sxs-lookup"><span data-stu-id="2ba54-179">Make changes tooyour app locally.</span></span>
2. <span data-ttu-id="2ba54-180">Nyissa meg a GitBash (vagy a Terminálszolgáltatások informatikai Git van a `PATH`), módosítsa a könyvtárakat toohello az alkalmazás gyökérkönyvtárában, és futtassa a következő parancsok hello:</span><span class="sxs-lookup"><span data-stu-id="2ba54-180">Open GitBash (or a terminal, it Git is in your `PATH`), change directories toohello root directory of your app, and run hello following commands:</span></span>
   
        git add .
        git commit -m "comment describing changes"
        git push azure master
   
    <span data-ttu-id="2ba54-181">A korábban létrehozott hello jelszó bekéri.</span><span class="sxs-lookup"><span data-stu-id="2ba54-181">You will be prompted for hello password you created earlier.</span></span>
   
    ![Előtte hely módosítások tooAzure Git keresztül][git-change-push]
3. <span data-ttu-id="2ba54-183">Keresse meg a túl**http://[site name].azurewebsites.net/index.php** toosee az alkalmazás és az esetleg elvégzett módosításokat:</span><span class="sxs-lookup"><span data-stu-id="2ba54-183">Browse too**http://[site name].azurewebsites.net/index.php** toosee your app and any changes you may have made:</span></span>
   
    ![Az Azure PHP-webhely][running-app]

> [!NOTE]
> <span data-ttu-id="2ba54-185">Ha azt szeretné, hogy az az Azure-fiók regisztrálása előtt az Azure App Service lépései tooget, nyissa meg túl[App Service kipróbálása](https://azure.microsoft.com/try/app-service/), ahol azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben.</span><span class="sxs-lookup"><span data-stu-id="2ba54-185">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="2ba54-186">Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.</span><span class="sxs-lookup"><span data-stu-id="2ba54-186">No credit cards required; no commitments.</span></span>
> 
> 

<a name="composer"></a>

## <a name="enable-composer-automation-with-hello-composer-extension"></a><span data-ttu-id="2ba54-187">Szerkesztő automatizálása a hello Composer bővítményt engedélyezése</span><span class="sxs-lookup"><span data-stu-id="2ba54-187">Enable Composer automation with hello Composer extension</span></span>
<span data-ttu-id="2ba54-188">Alapértelmezés szerint hello git telepítési folyamatának az App Service-ben nem minden composer.json, ha nincs fiókja, a PHP-projektben.</span><span class="sxs-lookup"><span data-stu-id="2ba54-188">By default, hello git deployment process in App Service doesn't do anything with composer.json, if you have one in your PHP project.</span></span> <span data-ttu-id="2ba54-189">Engedélyezheti a feldolgozás során composer.json `git push` hello Composer bővítményt engedélyezésével.</span><span class="sxs-lookup"><span data-stu-id="2ba54-189">You can enable composer.json processing during `git push` by enabling hello Composer extension.</span></span>

1. <span data-ttu-id="2ba54-190">A PHP a webalkalmazás-alkalmazás paneljén hello [Azure-portálon][management-portal], kattintson a **eszközök** > **bővítmények**.</span><span class="sxs-lookup"><span data-stu-id="2ba54-190">In your PHP web app's blade in hello [Azure portal][management-portal], click **Tools** > **Extensions**.</span></span>
   
    ![Composer bővítményt beállításai][composer-extension-settings]
2. <span data-ttu-id="2ba54-192">Kattintson a **Hozzáadás**, majd kattintson a **szerkesztő**.</span><span class="sxs-lookup"><span data-stu-id="2ba54-192">Click **Add**, then click **Composer**.</span></span>
   
    ![Composer bővítményt hozzáadása][composer-extension-add]
3. <span data-ttu-id="2ba54-194">Kattintson a **OK** tooaccept jogi feltételeket.</span><span class="sxs-lookup"><span data-stu-id="2ba54-194">Click **OK** tooaccept legal terms.</span></span> <span data-ttu-id="2ba54-195">Kattintson a **OK** újra tooadd hello bővítmény.</span><span class="sxs-lookup"><span data-stu-id="2ba54-195">Click **OK** again tooadd hello extension.</span></span>
   
    <span data-ttu-id="2ba54-196">Hello **bővítményeket telepített** panel mostantól megjeleníti hello Composer bővítményt.</span><span class="sxs-lookup"><span data-stu-id="2ba54-196">hello **Installed extensions** blade will now show hello Composer extension.</span></span>  
    <span data-ttu-id="2ba54-197">![Composer bővítményt megtekintése][composer-extension-view]</span><span class="sxs-lookup"><span data-stu-id="2ba54-197">![Composer Extension View][composer-extension-view]</span></span>
4. <span data-ttu-id="2ba54-198">Most, hajtsa végre `git add`, `git commit`, és `git push` , például az előző szakaszban hello.</span><span class="sxs-lookup"><span data-stu-id="2ba54-198">Now, perform `git add`, `git commit`, and `git push` like in hello previous section.</span></span> <span data-ttu-id="2ba54-199">Most láthatja, hogy szerkesztő telepíti a composer.json definiált függőségek.</span><span class="sxs-lookup"><span data-stu-id="2ba54-199">You'll now see that Composer is installing dependencies defined in composer.json.</span></span>
   
    ![Composer bővítményt sikeres][composer-extension-success]

## <a name="next-steps"></a><span data-ttu-id="2ba54-201">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2ba54-201">Next steps</span></span>
<span data-ttu-id="2ba54-202">További információkért lásd: hello [PHP fejlesztői központ](/develop/php/).</span><span class="sxs-lookup"><span data-stu-id="2ba54-202">For more information, see hello [PHP Developer Center](/develop/php/).</span></span>

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
