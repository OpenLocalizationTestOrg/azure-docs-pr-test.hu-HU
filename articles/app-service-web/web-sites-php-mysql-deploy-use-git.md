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
# <a name="create-a-php-mysql-web-app-in-azure-app-service-and-deploy-using-git"></a>PHP-MySQL webes alkalmazás létrehozása az Azure App Service-ben és üzembe helyezése a Git használatával
Az oktatóanyag bemutatja, hogyan PHP-MySQL-webalkalmazás létrehozása és központi telepítése úgy, hogy [App Service](http://go.microsoft.com/fwlink/?LinkId=529714) Git használatával. Használandó [PHP][install-php], a MySQL parancssori eszköz (része [MySQL][install-mysql]), és [Git] [ install-git] telepítve a számítógépre. Ez az oktatóanyag utasításai követhetők bármely operációs rendszeren, beleértve a Windows, Mac és Linux. Ez az útmutató befejezése után fog egy PHP/MySQL az Azure-ban futó webalkalmazás.

Az oktatóanyagban érintett témák köre:

* A webes alkalmazás és MySQL adatbázis használatával létrehozása a [Azure Portal][management-portal]. Mivel a PHP nincs engedélyezve az [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714) alapértelmezés szerint nem különleges futtatásához szükséges a PHP-kódot.
* Hogyan közzététele az Azure Git használatával az alkalmazás közzétételét.
* Feladatok automatizálásához szerkesztő Composer bővítményt engedélyezése minden `git push`.

Az oktatóanyag utasításait követve egy egyszerű regisztrációs webalkalmazás PHP fog létrehozni. Az alkalmazás üzemel a webalkalmazásokban. A kész alkalmazás képernyőfelvételének alatt van:

![Az Azure PHP-webhely][running-app]

## <a name="set-up-the-development-environment"></a>A fejlesztési környezet kialakítása
Ez az oktatóanyag feltételezi, hogy rendelkezik [PHP][install-php], a MySQL parancssori eszköz (része [MySQL][install-mysql]), és [Git] [ install-git] telepítve a számítógépre.

<a id="create-web-site-and-set-up-git"></a>

## <a name="create-a-web-app-and-set-up-git-publishing"></a>Webalkalmazás létrehozása és beállítása a Git-közzététel
Kövesse az alábbi lépéseket a webes alkalmazás és MySQL-adatbázis létrehozásához:

1. Jelentkezzen be a [Azure-portálon][management-portal].
2. Kattintson a **új** ikonra.
3. Kattintson a **tekintse meg az összes** melletti **piactér**. 
4. Kattintson a **Web + mobil**, majd **webes alkalmazás + MySQL**. Ezt követően kattintson a **Create** (Létrehozás) gombra.
5. Adjon meg egy érvényes nevet az erőforráscsoport számára.
   
    ![Állítsa az erőforráscsoport neve][resource-group]
6. Adja meg az új webes alkalmazás értékeit.
   
    ![Webalkalmazás létrehozása][new-web-app]
7. Adja meg az új adatbázis, beleértve a jogi feltételek elfogadását jelző mezőt.
   
    ![Új MySQL-adatbázis létrehozása][new-mysql-db]
8. Ha a webalkalmazás létrejött, látni fogja az új webalkalmazás panelen.
9. A **beállítások** kattintson a **folyamatos üzembe helyezés**, majd kattintson a *kötelező beállítások konfigurálása*.
   
    ![Git-közzététel beállítása][setup-publishing]
10. Válassza ki **helyi Git-tárház** adatforrásra vonatkozóan.
    
     ![Git-tárház beállítása][setup-repository]
11. Git-közzététel engedélyezéséhez meg kell adnia egy felhasználónevet és jelszót. Jegyezze fel a felhasználónevet és jelszót hoz létre. (Ha egy Git-tárház előtt beállítását követően ez a lépés kimarad.)
    
     ![Közzétételi hitelesítő adatok létrehozása][credentials]

## <a name="get-remote-mysql-connection-information"></a>Távoli MySQL kapcsolatadatok beolvasása
A MySQL-adatbázis, amely futtatja a Web Apps, a fog csatlakozni kell a kapcsolat adatai. Ahhoz, hogy a MySQL-kapcsolati adatokat, kövesse az alábbi lépéseket:

1. Az erőforráscsoport kattintson az adatbázisban:
   
    ![Adatbázis kiválasztása][select-database]
2. Az adatbázisból **beállítások**, jelölje be **tulajdonságok**.
   
    ![A tulajdonságok kiválasztása][select-properties]
3. Jegyezze fel az értékei `Database`, `Host`, `User Id`, és `Password`.
   
    ![Megjegyzés: Tulajdonságok][note-properties]

## <a name="build-and-test-your-app-locally"></a>És az alkalmazás helyi teszteléséhez
Most, hogy létrehozott egy webalkalmazást, az alkalmazás helyi fejleszthet, majd telepítheti azt a tesztelés után.

A regisztrációs alkalmazás egy egyszerű PHP-alkalmazás, amely lehetővé teszi, hogy regisztrálja az esemény a név és e-mail cím megadásával. Előző igénylők kapcsolatos információkat a táblázatban jelennek meg. Regisztrációs adatokat a MySQL-adatbázisban tárolódik. Az alkalmazás tartalmaz egy fájl (másolás/beillesztés kód alatt érhető el):

* **index.php**: regisztráció és a bejegyzés adatokat tartalmazó táblát űrlap jeleníti meg.

Építsenek, és futtassa az alkalmazást helyileg, kövesse az alábbi lépéseket. Vegye figyelembe, hogy ezek a lépések feltételezik, a PHP és a MySQL parancssori eszköz (MySQL része) állítsa be a helyi számítógépen, és engedélyezte a [MySQL OEM kiterjesztése][pdo-mysql].

1. A távoli MySQL kiszolgálóhoz csatlakoznak, a értéke `Data Source`, `User Id`, `Password`, és `Database` , amely korábban kapott:
   
        mysql -h{Data Source] -u[User Id] -p[Password] -D[Database]
2. A MySQL parancssor jelenik meg:
   
        mysql>
3. Illessze be a következő `CREATE TABLE` parancs futtatásával hozzon létre a `registration_tbl` táblát az adatbázisban:
   
        CREATE TABLE registration_tbl(id INT NOT NULL AUTO_INCREMENT, PRIMARY KEY(id), name VARCHAR(30), email VARCHAR(30), date DATE);
4. Hozzon létre a helyi alkalmazások mappa gyökeréhez **index.php** fájlt.
5. Nyissa meg a **index.php** szöveg- vagy IDE fájlt, és adja hozzá a következő kódot, és végezze el a szükséges módosításokat jelölésű `//TODO:` megjegyzések.

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

1. A terminálban az alkalmazás mappájában navigáljon, és írja be a következő parancsot:
   
       php -S localhost:8000

Most tallózással megkereshet **http://localhost:8000 /** kell tesztelni az alkalmazást.

## <a name="publish-your-app"></a>Az alkalmazás közzététele
Miután ellenőrizte, hogy az alkalmazást helyileg, közzéteheti a Git használatával webalkalmazások. A rendszer a helyi Git-tárház inicializálása, és tegye közzé az alkalmazást.

> [!NOTE]
> Ezek a ugyanazokat a lépéseket az Azure portálon végén a webalkalmazás létrehozása és a készlet be Git-közzététel a fenti szakaszban látható.
> 
> 

1. (Választható)  Ha elfelejti vagy rossz helyen lévő a Git távoli repostitory URL-cím, keresse meg a webes alkalmazás tulajdonságainak az Azure portálon.
2. Nyissa meg a GitBash (vagy egy terminált, ha Git a `PATH`), módosítsa a könyvtárat az alkalmazás gyökérkönyvtárában, és futtassa a következő parancsokat:
   
        git init
        git add .
        git commit -m "initial commit"
        git remote add azure [URL for remote repository]
        git push azure master
   
    Fogja kérni fogja a korábban létrehozott jelszót.
   
    ![Az Azure Git keresztül kezdeti leküldéses][git-initial-push]
3. Keresse meg a **http://[site name].azurewebsites.net/index.php** (ezt az információt fogja tárolni a fiók irányítópult) az alkalmazás használatának megkezdéséhez:
   
    ![Az Azure PHP-webhely][running-app]

Miután közzétette az alkalmazást, annak módosításával kezdődik, és a Git segítségével közzéteszi.

## <a name="publish-changes-to-your-app"></a>Módosítások az alkalmazás közzététele
A módosítások az alkalmazásban való közzétételéhez kövesse az alábbi lépéseket:

1. Az alkalmazás helyileg módosítja.
2. Nyissa meg a GitBash (vagy a Terminálszolgáltatások informatikai Git van a `PATH`), módosítsa a könyvtárat az alkalmazás gyökérkönyvtárában, és futtassa a következő parancsokat:
   
        git add .
        git commit -m "comment describing changes"
        git push azure master
   
    Fogja kérni fogja a korábban létrehozott jelszót.
   
    ![Azure Git keresztül előtte webhely módosításai][git-change-push]
3. Keresse meg a **http://[site name].azurewebsites.net/index.php** , az alkalmazást, és esetleg elvégzett módosításokat:
   
    ![Az Azure PHP-webhely][running-app]

> [!NOTE]
> Ha az Azure App Service-t az Azure-fiók regisztrálása előtt szeretné kipróbálni, ugorjon [Az Azure App Service kipróbálása](https://azure.microsoft.com/try/app-service/) oldalra. Itt azonnal létrehozhat egy ideiglenes, kezdő szintű webalkalmazást az App Service szolgáltatásban. Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.
> 
> 

<a name="composer"></a>

## <a name="enable-composer-automation-with-the-composer-extension"></a>Szerkesztő automatizálása a Composer bővítményt engedélyezése
Alapértelmezés szerint az App Service-ben a git telepítési folyamat nem minden composer.json, ha nincs fiókja, a PHP-projektben. Engedélyezheti a feldolgozás során composer.json `git push` Composer bővítményt engedélyezésével.

1. A PHP a webalkalmazás-alkalmazás paneljén a [Azure-portálon][management-portal], kattintson a **eszközök** > **bővítmények**.
   
    ![Composer bővítményt beállításai][composer-extension-settings]
2. Kattintson a **Hozzáadás**, majd kattintson a **szerkesztő**.
   
    ![Composer bővítményt hozzáadása][composer-extension-add]
3. Kattintson a **OK** jogi feltételek elfogadásának. Kattintson a **OK** újra a adja hozzá a kiterjesztést.
   
    A **bővítményeket telepített** panel most megjelenik a Composer bővítményt.  
    ![Composer bővítményt megtekintése][composer-extension-view]
4. Most, hajtsa végre `git add`, `git commit`, és `git push` , például az előző szakaszban. Most láthatja, hogy szerkesztő telepíti a composer.json definiált függőségek.
   
    ![Composer bővítményt sikeres][composer-extension-success]

## <a name="next-steps"></a>Következő lépések
További információkért lásd: a [PHP fejlesztői központ](/develop/php/).

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
