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
# <a name="create-a-php-mysql-web-app-in-azure-app-service-and-deploy-using-ftp"></a>PHP-MySQL webes alkalmazás létrehozása az Azure App Service-ben és üzembe helyezése FTP használatával
Az oktatóanyag bemutatja, hogyan PHP-MySQL-webalkalmazás létrehozása és központi telepítése FTP használatával. Ez az oktatóanyag feltételezi, hogy rendelkezik [PHP][install-php], [MySQL][install-mysql], webkiszolgáló, és az FTP-ügyfél telepítve a számítógépre. Ez az oktatóanyag utasításai követhetők bármely operációs rendszeren, beleértve a Windows, Mac és Linux. Ez az útmutató befejezése után fog egy PHP/MySQL az Azure-ban futó webalkalmazás.

Az oktatóanyagban érintett témák köre:

* Tudnivalók a webalkalmazások és az Azure portál használatával MySQL-adatbázis. A PHP alapértelmezés szerint engedélyezve van a webalkalmazásokban, mert semmi különleges kell futnia a PHP-kódot.
* Hogyan lehet FTP használatával az alkalmazás közzétételére.

Az oktatóanyag utasításait követve egy egyszerű regisztrációs webalkalmazás PHP fog létrehozni. Az alkalmazás üzemel a webalkalmazásban. A kész alkalmazás képernyőfelvételének alatt van:

![Az Azure PHP-webhely][running-app]

> [!NOTE]
> Ha szeretne egy olyan fiók regisztrálása előtt Ismerkedés az Azure App Service, folytassa a [App Service kipróbálása](https://azure.microsoft.com/try/app-service/), ahol azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben. Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel. 
> 
> 

## <a name="create-a-web-app-and-set-up-ftp-publishing"></a>Hozzon létre egy webalkalmazást, és állítsa be az FTP-közzététel
Kövesse az alábbi lépéseket a webes alkalmazás és MySQL-adatbázis létrehozásához:

1. Jelentkezzen be a [Azure-portálon][management-portal].
2. Kattintson a **+ új** az Azure portál bal felső ikonra.
   
    ![Új Azure-webhely létrehozása][new-website]
3. A írja be a **webes alkalmazás + MySQL** , majd kattintson a **webes alkalmazás + MySQL**.
   
    ![Egyéni új webhely létrehozása][custom-create]
4. Kattintson a **Create** (Létrehozás) gombra. Adjon meg egy egyedi alkalmazás nevét, egy érvényes nevet az erőforráscsoportot és egy új service-csomagot.
   
    ![Állítsa az erőforráscsoport neve][resource-group]
5. Adja meg az új adatbázis, beleértve a jogi feltételek elfogadását jelző mezőt.
   
    ![Új MySQL-adatbázis létrehozása][new-mysql-db]
6. A webalkalmazás létrehozásakor látni fogja az új szolgáltatás panelen.
7. Kattintson a **beállítások** > **üzembe helyezési hitelesítő adatok**. 
   
    ![Telepítési hitelesítő adatok beállítása][set-deployment-credentials]
8. Az FTP-közzététel engedélyezéséhez meg kell adnia egy felhasználónevet és jelszót. A hitelesítő adatok mentése, és jegyezze fel a felhasználónevet és jelszót hoz létre.
   
    ![Közzétételi hitelesítő adatok létrehozása][portal-ftp-username-password]

## <a name="build-and-test-your-app-locally"></a>És az alkalmazás helyi teszteléséhez
A regisztrációs alkalmazás egy egyszerű PHP-alkalmazás, amely lehetővé teszi, hogy regisztrálja az esemény a név és e-mail cím megadásával. Előző igénylők kapcsolatos információkat a táblázatban jelennek meg. Regisztrációs adatokat a MySQL-adatbázisban tárolódik. Az alkalmazás két fájlból áll:

* **index.php**: regisztráció és a bejegyzés adatokat tartalmazó táblát űrlap jeleníti meg.
* **createtable.php**: az alkalmazás a MySQL táblát hoz létre. Ezt a fájlt csak egyszer használható.

Felépítéséhez és az alkalmazás futtatása helyben, kövesse az alábbi lépéseket. Vegye figyelembe, hogy ezek a lépések feltételezik, PHP, a MySQL és egy webkiszolgáló, állítsa be a helyi számítógépen, és engedélyezte a [MySQL OEM kiterjesztése][pdo-mysql].

1. Hozzon létre egy MySQL-adatbázis nevű `registration`. Ez a MySQL-parancssorból, ezzel a paranccsal teheti meg:
   
        mysql> create database registration;
2. A webkiszolgáló gyökérkönyvtárában, hozzon létre egy nevű `registration` , és hozzon létre két fájlt az - egy `createtable.php` és egy `index.php`.
3. Nyissa meg a `createtable.php` fájlt egy szövegszerkesztőben, vagy IDE, és adja hozzá az alábbi kódot. Ez a kód létrehozásához használható a `registration_tbl` a tábla a `registration` adatbázis.
   
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
   > Értéket kell <code>$user</code> és <code>$pwd</code> a helyi MySQL-felhasználó nevét és jelszavát.
   > 
   > 
4. Nyisson meg egy webböngészőt, és keresse meg a [http://localhost/registration/createtable.php][localhost-createtable]. Ekkor létrejön az `registration_tbl` tábla az adatbázisban.
5. Nyissa meg a **index.php** fájlt egy szövegszerkesztőben, vagy IDE, és adja hozzá az alapvető HTML- és CSS kódot (a PHP kód megkapja a későbbi lépésekben) lap.
   
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
6. A PHP címkék belül adja hozzá a PHP kódot az adatbázishoz való kapcsolódáshoz.
   
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
   > Ebben az esetben szüksége lesz a értéket <code>$user</code> és <code>$pwd</code> a helyi MySQL-felhasználó nevét és jelszavát.
   > 
   > 
7. Követően az adatbázis-kapcsolat kód adja hozzá a kódot a regisztrációs adatokat beszúrni az adatbázisba.
   
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
8. Végezetül a fenti kódot követően adja hozzá a kódot az adatokat az adatbázisból.
   
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

Most tallózással megkereshet [http://localhost/registration/index.php] [ localhost-index] az alkalmazás teszteléséhez.

## <a name="get-mysql-and-ftp-connection-information"></a>MySQL- és FTP-kapcsolati adatok
A MySQL-adatbázis, amely futtatja a Web Apps, a fog csatlakozni kell a kapcsolat adatai. Ahhoz, hogy a MySQL-kapcsolati adatokat, kövesse az alábbi lépéseket:

1. Az app service webalkalmazás panelen kattintson az erőforrás-csoport hivatkozás:
   
    ![Válasszon erőforráscsoportot][select-resourcegroup]
2. Az erőforráscsoport kattintson az adatbázisban:
   
    ![Adatbázis kiválasztása][select-database]
3. Az adatbázisból összefoglaló, válassza ki a **beállítások** > **tulajdonságok**.
   
    ![A tulajdonságok kiválasztása][select-properties]
4. Jegyezze fel az értékei `Database`, `Host`, `User Id`, és `Password`.
   
    ![Megjegyzés: Tulajdonságok][note-properties]
5. A webalkalmazás, kattintson a **közzétételi profil letöltése** hivatkozásra, a jobb alsó sarkában az oldalon:
   
    ![Közzétételi profil letöltése][download-publish-profile]
6. Nyissa meg a `.publishsettings` fájlt egy XML-szerkesztőben. 
7. Keresés a `<publishProfile >` elem `publishMethod="FTP"` , amelyek a következőhöz hasonló:
   
        <publishProfile publishMethod="FTP" publishUrl="ftp://[mysite].azurewebsites.net/site/wwwroot" ftpPassiveMode="True" userName="[username]" userPWD="[password]" destinationAppUrl="http://[name].antdf0.antares-test.windows-int.net" 
            ...
        </publishProfile>

Jegyezze fel a `publishUrl`, `userName`, és `userPWD` attribútumok.

## <a name="publish-your-app"></a>Az alkalmazás közzététele
Miután ellenőrizte, hogy az alkalmazást helyileg, közzéteheti a webalkalmazáshoz FTP használatával. Azonban először az alkalmazást az adatbázis-kapcsolat adatainak frissítése. Korábban beszerzett adatbázis kapcsolati információk használatával (a a **MySQL beszerzése és az FTP-kapcsolati adatokat** szakaszban), a következő információinak frissítése a **is** a `createdatabase.php` és `index.php` fájlokat a megfelelő értékekkel:

    // DB connection info
    $host = "value of Data Source";
    $user = "value of User Id";
    $pwd = "value of Password";
    $db = "value of Database";

Most már készen áll az FTP használó alkalmazás közzététele.

1. Nyissa meg az FTP-ügyfél választott.
2. Adja meg a *állomásnév* a a `publishUrl` az FTP-ügyfél a fent leírt attribútum.
3. Adja meg a `userName` és `userPWD` fent leírt attribútumok be az FTP-ügyfél nem változik.
4. A kapcsolatot.

Kapcsolódás után lesz, a fájlok feltöltését és letöltését igény szerint. Ne feledje, hogy feltölteni kívánt fájlokat a gyökérkönyvtárba, amely `/site/wwwroot`.

Mindkét feltöltése után `index.php` és `createtable.php`, keresse meg a **http://[site name].azurewebsites.net/createtable.php** MySQL tábla az alkalmazás létrehozása, majd keresse meg **http://[site name]. azurewebsites.NET/index.php** az alkalmazás használatának megkezdéséhez.

## <a name="next-steps"></a>Következő lépések
További információkért lásd: a [PHP fejlesztői központ](/develop/php/).

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

