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
# <a name="create-a-php-mysql-web-app-in-azure-app-service-and-deploy-using-ftp"></a>PHP-MySQL webes alkalmazás létrehozása az Azure App Service-ben és üzembe helyezése FTP használatával
Az oktatóanyag bemutatja, hogyan toocreate egy PHP-MySQL web app, és hogyan toodeploy FTP használatával. Ez az oktatóanyag feltételezi, hogy rendelkezik [PHP][install-php], [MySQL][install-mysql], webkiszolgáló, és az FTP-ügyfél telepítve a számítógépre. hello az oktatóanyag utasításai követhetők bármelyik operációs rendszeren, beleértve a Windows, Mac és Linux. Ez az útmutató befejezése után fog egy PHP/MySQL az Azure-ban futó webalkalmazás.

Az oktatóanyagban érintett témák köre:

* Hogyan toocreate egy webalkalmazást és a MySQL-adatbázis hello Azure portál használatával. A PHP alapértelmezés szerint engedélyezve van a webalkalmazásokban, mert semmi különleges-e szükséges toorun a PHP-kódot.
* Hogyan toopublish az alkalmazást tooAzure a FTP.

Az oktatóanyag utasításait követve egy egyszerű regisztrációs webalkalmazás PHP fog létrehozni. a webalkalmazásban hello alkalmazás üzemel. A képernyőfelvétel a hello befejeződött alkalmazás alatt van:

![Az Azure PHP-webhely][running-app]

> [!NOTE]
> Ha azt szeretné, hogy az Azure App Service egy olyan fiók regisztrálása előtt lépései tooget, nyissa meg túl[App Service kipróbálása](https://azure.microsoft.com/try/app-service/), ahol azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben. Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel. 
> 
> 

## <a name="create-a-web-app-and-set-up-ftp-publishing"></a>Hozzon létre egy webalkalmazást, és állítsa be az FTP-közzététel
A webes alkalmazás és MySQL-adatbázis, kövesse ezeket a lépéseket toocreate:

1. Bejelentkezési toohello [Azure Portal][management-portal].
2. Kattintson a hello **+ új** hello felül ikon hello Azure-portálon a bal oldali.
   
    ![Új Azure-webhely létrehozása][new-website]
3. A hello írja be **webes alkalmazás + MySQL** , majd kattintson a **webes alkalmazás + MySQL**.
   
    ![Egyéni új webhely létrehozása][custom-create]
4. Kattintson a **Create** (Létrehozás) gombra. Adjon meg egy egyedi alkalmazás nevét, egy érvényes nevet a hello erőforráscsoport és egy új service-csomagot.
   
    ![Állítsa az erőforráscsoport neve][resource-group]
5. Adja meg az értékeket az új adatbázisra, beleértve a elfogadja toohello jogi feltételeket.
   
    ![Új MySQL-adatbázis létrehozása][new-mysql-db]
6. Hello webalkalmazás létrehozásakor hello új app service panel jelenik meg.
7. Kattintson a **beállítások** > **üzembe helyezési hitelesítő adatok**. 
   
    ![Telepítési hitelesítő adatok beállítása][set-deployment-credentials]
8. tooenable FTP-közzététel, meg kell adnia egy felhasználónevet és jelszót. Hello hitelesítő adatok mentése, és jegyezze fel a hello felhasználónevet és jelszót hoz létre.
   
    ![Közzétételi hitelesítő adatok létrehozása][portal-ftp-username-password]

## <a name="build-and-test-your-app-locally"></a>És az alkalmazás helyi teszteléséhez
hello regisztrációs alkalmazása, amely lehetővé teszi a név és e-mail cím megadásával az esemény tooregister egyszerű PHP-alkalmazások. Előző igénylők kapcsolatos információkat a táblázatban jelennek meg. Regisztrációs adatokat a MySQL-adatbázisban tárolódik. hello alkalmazás két fájlból áll:

* **index.php**: regisztráció és a bejegyzés adatokat tartalmazó táblát űrlap jeleníti meg.
* **createtable.php**: hello alkalmazás hello MySQL táblát hoz létre. Ezt a fájlt csak egyszer használható.

toobuild és futtatási hello alkalmazást helyileg, kövesse az alábbi hello lépéseket. Vegye figyelembe, hogy ezek a lépések feltételezik, hogy a PHP, a MySQL és egy webkiszolgáló, állítsa be a helyi számítógépen, és engedélyezte hello [MySQL OEM kiterjesztése][pdo-mysql].

1. Hozzon létre egy MySQL-adatbázis nevű `registration`. Ezt megteheti a parancssorból hello MySQL ezzel a paranccsal:
   
        mysql> create database registration;
2. A webkiszolgáló gyökérkönyvtárában, hozzon létre egy nevű `registration` , és hozzon létre két fájlt az - egy `createtable.php` és egy `index.php`.
3. Nyissa meg hello `createtable.php` fájlt egy szövegszerkesztőben, vagy IDE, és adja hozzá az alábbi hello kódot. Ez a kód lesz használt toocreate hello `registration_tbl` hello táblájában `registration` adatbázis.
   
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
   > Szüksége lesz a tooupdate hello értékeinek <code>$user</code> és <code>$pwd</code> a helyi MySQL-felhasználó nevét és jelszavát.
   > 
   > 
4. Nyisson meg egy webböngészőt, és keresse meg a túl[http://localhost/registration/createtable.php][localhost-createtable]. Ezzel létrehoz hello `registration_tbl` hello adatbázis táblájában.
5. Nyissa meg hello **index.php** fájlt egy szövegszerkesztőben, vagy IDE, és adja hozzá a hello alapvető HTML- és CSS kód hello lap (hello PHP kód megkapja a későbbi lépésekben).
   
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
6. Hello PHP címkék, belül adja hozzá a PHP kódot toohello adatbázis csatlakozni.
   
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
   > Ebben az esetben szüksége lesz tooupdate hello értékeinek <code>$user</code> és <code>$pwd</code> a helyi MySQL-felhasználó nevét és jelszavát.
   > 
   > 
7. Következő hello adatbázis kapcsolati kódja vegye fel a regisztrációs adatok beszúrása hello adatbázis kódot.
   
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
8. Végül követően a fenti hello kódot, adja hozzá az adatok lekérdezése hello adatbázis kódját.
   
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

Most már megkeresheti túl[http://localhost/registration/index.php] [ localhost-index] tootest hello alkalmazást.

## <a name="get-mysql-and-ftp-connection-information"></a>MySQL- és FTP-kapcsolati adatok
tooconnect toohello MySQL-adatbázis, amely futtatja a Web Apps, a fog kell hello kapcsolódási információt. tooget MySQL kapcsolati adatokat, kövesse az alábbi lépéseket:

1. Hello app service webalkalmazás panelen kattintson az hello erőforrás csoportjának hivatkozásra:
   
    ![Válasszon erőforráscsoportot][select-resourcegroup]
2. Az erőforráscsoport kattintson hello adatbázis:
   
    ![Adatbázis kiválasztása][select-database]
3. Hello adatbázis összegzése, válassza ki **beállítások** > **tulajdonságok**.
   
    ![A tulajdonságok kiválasztása][select-properties]
4. Jegyezze fel a hello értékeinek `Database`, `Host`, `User Id`, és `Password`.
   
    ![Megjegyzés: Tulajdonságok][note-properties]
5. Kattintson a webalkalmazás hello **közzétételi profil letöltése** hivatkozásra, hello jobb alsó sarkában hello lap:
   
    ![Közzétételi profil letöltése][download-publish-profile]
6. Nyissa meg hello `.publishsettings` fájlt egy XML-szerkesztőben. 
7. Hello található `<publishProfile >` elem `publishMethod="FTP"` , amelyek a következőhöz hasonló toothis:
   
        <publishProfile publishMethod="FTP" publishUrl="ftp://[mysite].azurewebsites.net/site/wwwroot" ftpPassiveMode="True" userName="[username]" userPWD="[password]" destinationAppUrl="http://[name].antdf0.antares-test.windows-int.net" 
            ...
        </publishProfile>

Jegyezze fel a hello `publishUrl`, `userName`, és `userPWD` attribútumok.

## <a name="publish-your-app"></a>Az alkalmazás közzététele
Miután ellenőrizte, hogy az alkalmazást helyileg, közzéteheti azt tooyour web app FTP használatával. Azonban először tooupdate hello adatbázis-kapcsolat adatainak hello alkalmazásban. Korábban beszerzett hello adatbázis-kapcsolat adatainak használatával (a hello **MySQL beszerzése és az FTP-kapcsolati adatokat** szakaszban), a következő információkat a frissítés hello **is** hello `createdatabase.php` és `index.php` hello fájlokat a megfelelő értékeket:

    // DB connection info
    $host = "value of Data Source";
    $user = "value of User Id";
    $pwd = "value of Password";
    $db = "value of Database";

Most már áll toopublish az alkalmazás készen álljon a FTP.

1. Nyissa meg az FTP-ügyfél választott.
2. Adja meg a hello *állomásnév* a hello `publishUrl` az FTP-ügyfél a fent leírt attribútum.
3. Adja meg a hello `userName` és `userPWD` fent leírt attribútumok be az FTP-ügyfél nem változik.
4. A kapcsolatot.

Csatlakozás után tud tooupload kell, és szükség esetén töltse le a fájlokat. Ne feledje, hogy fájlokat toohello gyökérkönyvtár, amely feltölteni kívánt `/site/wwwroot`.

Mindkét feltöltése után `index.php` és `createtable.php`, keresse meg a túl**http://[site name].azurewebsites.net/createtable.php** toocreate hello MySQL tábla hello alkalmazáshoz, majd keresse meg a túl**http://[site neve ].azurewebsites.net/index.php** toobegin hello alkalmazás segítségével.

## <a name="next-steps"></a>Következő lépések
További információkért lásd: hello [PHP fejlesztői központ](/develop/php/).

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

