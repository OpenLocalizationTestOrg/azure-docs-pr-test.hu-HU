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
# <a name="create-a-php-mysql-web-app-in-azure-app-service-and-deploy-using-git"></a>PHP-MySQL webes alkalmazás létrehozása az Azure App Service-ben és üzembe helyezése a Git használatával
Az oktatóanyag bemutatja, hogyan toocreate egy PHP-MySQL web app, és hogyan toodeploy azt túl[App Service](http://go.microsoft.com/fwlink/?LinkId=529714) Git használatával. Használandó [PHP][install-php], MySQL parancssori eszköz hello (része [MySQL][install-mysql]), és [Git] [ install-git] telepítve a számítógépre. hello az oktatóanyag utasításai követhetők bármelyik operációs rendszeren, beleértve a Windows, Mac és Linux. Ez az útmutató befejezése után fog egy PHP/MySQL az Azure-ban futó webalkalmazás.

Az oktatóanyagban érintett témák köre:

* Hogyan toocreate egy webalkalmazást és a MySQL-adatbázis hello segítségével [Azure Portal][management-portal]. Mivel a PHP nincs engedélyezve az [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714) alapértelmezés szerint a szükséges toorun semmi különleges a PHP-kódot.
* Hogyan toopublish, és újból közzéteszi a Git használatával alkalmazás tooAzure.
* Hogyan tooenable hello Composer bővítményt tooautomate szerkesztő feladatokat minden `git push`.

Az oktatóanyag utasításait követve egy egyszerű regisztrációs webalkalmazás PHP fog létrehozni. a webalkalmazásokban hello alkalmazás üzemel. A képernyőfelvétel a hello befejeződött alkalmazás alatt van:

![Az Azure PHP-webhely][running-app]

## <a name="set-up-hello-development-environment"></a>Hello fejlesztési környezet beállítása
Ez az oktatóanyag feltételezi, hogy rendelkezik [PHP][install-php], MySQL parancssori eszköz hello (része [MySQL][install-mysql]), és [Git] [ install-git] telepítve a számítógépre.

<a id="create-web-site-and-set-up-git"></a>

## <a name="create-a-web-app-and-set-up-git-publishing"></a>Webalkalmazás létrehozása és beállítása a Git-közzététel
A webes alkalmazás és MySQL-adatbázis, kövesse ezeket a lépéseket toocreate:

1. Bejelentkezési toohello [Azure Portal][management-portal].
2. Kattintson a hello **új** ikonra.
3. Kattintson a **tekintse meg az összes** következő túl**piactér**. 
4. Kattintson a **Web + mobil**, majd **webes alkalmazás + MySQL**. Ezt követően kattintson a **Create** (Létrehozás) gombra.
5. Adjon meg egy érvényes nevet az erőforráscsoport számára.
   
    ![Állítsa az erőforráscsoport neve][resource-group]
6. Adja meg az új webes alkalmazás értékeit.
   
    ![Webalkalmazás létrehozása][new-web-app]
7. Adja meg az értékeket az új adatbázisra, beleértve a elfogadja toohello jogi feltételeket.
   
    ![Új MySQL-adatbázis létrehozása][new-mysql-db]
8. Hello webalkalmazás létrehozásakor hello új webalkalmazás panelen jelenik meg.
9. A **beállítások** kattintson a **folyamatos üzembe helyezés**, majd kattintson a *kötelező beállítások konfigurálása*.
   
    ![Git-közzététel beállítása][setup-publishing]
10. Válassza ki **helyi Git-tárház** hello adatforrásra vonatkozóan.
    
     ![Git-tárház beállítása][setup-repository]
11. tooenable Git közzététele, meg kell adnia egy felhasználónevet és jelszót. Jegyezze fel a hello felhasználónevet és jelszót hoz létre. (Ha egy Git-tárház előtt beállítását követően ez a lépés kimarad.)
    
     ![Közzétételi hitelesítő adatok létrehozása][credentials]

## <a name="get-remote-mysql-connection-information"></a>Távoli MySQL kapcsolatadatok beolvasása
tooconnect toohello MySQL-adatbázis, amely futtatja a Web Apps, a fog kell hello kapcsolódási információt. tooget MySQL kapcsolati adatokat, kövesse az alábbi lépéseket:

1. Az erőforráscsoport kattintson hello adatbázis:
   
    ![Adatbázis kiválasztása][select-database]
2. Hello adatbázisból **beállítások**, jelölje be **tulajdonságok**.
   
    ![A tulajdonságok kiválasztása][select-properties]
3. Jegyezze fel a hello értékeinek `Database`, `Host`, `User Id`, és `Password`.
   
    ![Megjegyzés: Tulajdonságok][note-properties]

## <a name="build-and-test-your-app-locally"></a>És az alkalmazás helyi teszteléséhez
Most, hogy létrehozott egy webalkalmazást, az alkalmazás helyi fejleszthet, majd telepítheti azt a tesztelés után.

hello regisztrációs alkalmazása, amely lehetővé teszi a név és e-mail cím megadásával az esemény tooregister egyszerű PHP-alkalmazások. Előző igénylők kapcsolatos információkat a táblázatban jelennek meg. Regisztrációs adatokat a MySQL-adatbázisban tárolódik. hello alkalmazás áll egy fájl (másolás/beillesztés kód alatt érhető el):

* **index.php**: regisztráció és a bejegyzés adatokat tartalmazó táblát űrlap jeleníti meg.

toobuild és futtatási hello alkalmazás helyi, kövesse a hello lépéseket. Vegye figyelembe, hogy ezek a lépések feltételezik hello PHP és a MySQL parancssori eszköz (MySQL része) állítsa be a helyi számítógépen, és, hogy engedélyezte hello [MySQL OEM kiterjesztése][pdo-mysql].

1. Csatlakozás toohello távoli MySQL-kiszolgáló hello értéke `Data Source`, `User Id`, `Password`, és `Database` , amely korábban kapott:
   
        mysql -h{Data Source] -u[User Id] -p[Password] -D[Database]
2. hello MySQL parancssor jelenik meg:
   
        mysql>
3. Illessze be a következő hello `CREATE TABLE` parancs toocreate hello `registration_tbl` táblát az adatbázisban:
   
        CREATE TABLE registration_tbl(id INT NOT NULL AUTO_INCREMENT, PRIMARY KEY(id), name VARCHAR(30), email VARCHAR(30), date DATE);
4. Hozzon létre a helyi alkalmazások mappa gyökeréhez hello **index.php** fájlt.
5. Nyissa meg hello **index.php** fájlt egy szövegszerkesztőben, vagy IDE, és adja hozzá a hello a következő kódot, és teljes hello a szükséges módosításokat jelölésű `//TODO:` megjegyzések.

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

1. Az egy terminál lépjen tooyour alkalmazás mappa és a típus hello a következő parancsot:
   
       php -S localhost:8000

Most már megkeresheti túl**http://localhost:8000 /** tootest hello alkalmazás.

## <a name="publish-your-app"></a>Az alkalmazás közzététele
Miután ellenőrizte, hogy az alkalmazást helyileg, közzéteheti azt Git tooWeb alkalmazások. A rendszer a helyi Git-tárház inicializálása, és hello alkalmazás közzététele.

> [!NOTE]
> A rendszer hello azonos hello hello végén hello hozzon létre egy webalkalmazást az Azure portálon, és állítsa be Git-közzététel a fenti szakaszban bemutatott lépések.
> 
> 

1. (Választható)  Ha elfelejti vagy rossz helyen lévő a Git távoli repostitory URL-cím, keresse meg a toohello web app tulajdonságainak hello Azure portálon.
2. Nyissa meg a GitBash (vagy egy terminált, ha Git a `PATH`), módosítsa a könyvtárakat toohello az alkalmazás gyökérkönyvtárában, és futtassa a következő parancsok hello:
   
        git init
        git add .
        git commit -m "initial commit"
        git remote add azure [URL for remote repository]
        git push azure master
   
    A korábban létrehozott hello jelszó bekéri.
   
    ![Kezdeti leküldéses tooAzure Git keresztül][git-initial-push]
3. Keresse meg a túl**http://[site name].azurewebsites.net/index.php** toobegin hello alkalmazással (ezt az információt fogja tárolni az irányítópulton fiók):
   
    ![Az Azure PHP-webhely][running-app]

Miután közzétette az alkalmazást, módosításokat tooit állapotában, és Git toopublish használja őket.

## <a name="publish-changes-tooyour-app"></a>Módosítások tooyour alkalmazás közzététele
toopublish módosítások tooyour alkalmazást, kövesse az alábbi lépéseket:

1. Módosítások tooyour alkalmazás helyileg tétele.
2. Nyissa meg a GitBash (vagy a Terminálszolgáltatások informatikai Git van a `PATH`), módosítsa a könyvtárakat toohello az alkalmazás gyökérkönyvtárában, és futtassa a következő parancsok hello:
   
        git add .
        git commit -m "comment describing changes"
        git push azure master
   
    A korábban létrehozott hello jelszó bekéri.
   
    ![Előtte hely módosítások tooAzure Git keresztül][git-change-push]
3. Keresse meg a túl**http://[site name].azurewebsites.net/index.php** toosee az alkalmazás és az esetleg elvégzett módosításokat:
   
    ![Az Azure PHP-webhely][running-app]

> [!NOTE]
> Ha azt szeretné, hogy az az Azure-fiók regisztrálása előtt az Azure App Service lépései tooget, nyissa meg túl[App Service kipróbálása](https://azure.microsoft.com/try/app-service/), ahol azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben. Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.
> 
> 

<a name="composer"></a>

## <a name="enable-composer-automation-with-hello-composer-extension"></a>Szerkesztő automatizálása a hello Composer bővítményt engedélyezése
Alapértelmezés szerint hello git telepítési folyamatának az App Service-ben nem minden composer.json, ha nincs fiókja, a PHP-projektben. Engedélyezheti a feldolgozás során composer.json `git push` hello Composer bővítményt engedélyezésével.

1. A PHP a webalkalmazás-alkalmazás paneljén hello [Azure-portálon][management-portal], kattintson a **eszközök** > **bővítmények**.
   
    ![Composer bővítményt beállításai][composer-extension-settings]
2. Kattintson a **Hozzáadás**, majd kattintson a **szerkesztő**.
   
    ![Composer bővítményt hozzáadása][composer-extension-add]
3. Kattintson a **OK** tooaccept jogi feltételeket. Kattintson a **OK** újra tooadd hello bővítmény.
   
    Hello **bővítményeket telepített** panel mostantól megjeleníti hello Composer bővítményt.  
    ![Composer bővítményt megtekintése][composer-extension-view]
4. Most, hajtsa végre `git add`, `git commit`, és `git push` , például az előző szakaszban hello. Most láthatja, hogy szerkesztő telepíti a composer.json definiált függőségek.
   
    ![Composer bővítményt sikeres][composer-extension-success]

## <a name="next-steps"></a>Következő lépések
További információkért lásd: hello [PHP fejlesztői központ](/develop/php/).

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
