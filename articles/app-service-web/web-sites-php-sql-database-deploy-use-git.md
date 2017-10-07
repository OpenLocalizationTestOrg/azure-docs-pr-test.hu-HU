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
# <a name="create-a-php-sql-web-app-and-deploy-tooazure-app-service-using-git"></a>PHP-SQL-webalkalmazás létrehozása és központi telepítése tooAzure App Service Git használatával
Az oktatóanyag bemutatja, hogyan toocreate egy PHP webalkalmazást az app [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) tooAzure SQL-adatbázis csatlakozik, és hogyan toodeploy azt a Git használatával. Ez az oktatóanyag feltételezi, hogy rendelkezik [PHP][install-php], [SQL Server Express][install-SQLExpress], hello [Microsoft SQL Server php Drivers ](http://www.microsoft.com/download/en/details.aspx?id=20098), és [Git] [ install-git] telepítve a számítógépre. Ez az útmutató befejezése után fog egy PHP-SQL Azure-ban futó webalkalmazás.

> [!NOTE]
> Telepítse, és hello használata php-hez tartozó SQL Server, SQL Server Express és a Microsoft Drivers hello konfigurálása [Microsoft Webplatform-telepítő](http://www.microsoft.com/web/downloads/platform.aspx).
> 
> 

Az oktatóanyagban érintett témák köre:

* Hogyan toocreate az Azure web app és egy SQL-adatbázist hello [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715). A PHP alapértelmezés szerint engedélyezve van az App Service Web Apps, mert semmi különleges-e szükséges toorun a PHP-kódot.
* Hogyan toopublish, és újból közzéteszi a Git használatával alkalmazás tooAzure.

Az oktatóanyag utasításait követve egy egyszerű regisztrációs webalkalmazásából PHP fog létrehozni. hello alkalmazás üzemel az Azure-webhely. A képernyőfelvétel a hello befejeződött alkalmazás alatt van:

![Az Azure PHP-webhely](./media/web-sites-php-sql-database-deploy-use-git/running_app_3.png)

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

> [!NOTE]
> Ha azt szeretné, hogy az az Azure-fiók regisztrálása előtt az Azure App Service lépései tooget, nyissa meg túl[App Service kipróbálása](https://azure.microsoft.com/try/app-service/), ahol azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben. Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.
> 
> 

## <a name="create-an-azure-web-app-and-set-up-git-publishing"></a>Azure-webalkalmazás létrehozása és beállítása a Git-közzététel
Kövesse ezeket a lépéseket toocreate Azure-webalkalmazás és az SQL-adatbázisban:

1. Jelentkezzen be toohello [Azure Portal](https://portal.azure.com/).
2. Hello kattintva nyissa meg hello Azure piactér **új** hello irányítópult balra hello felül ikonra, kattintson a **kijelölése az összes** következő tooMarketplace és kiválasztásával **Web + mobil**.
3. Hello piactér, válassza ki **Web + mobil**.
4. Kattintson a hello **webes alkalmazás + SQL** ikonra.
5. Hello webes alkalmazás + SQL app hello leírása elolvasása, után válassza ki a **létrehozása**.
6. Kattintson az egyes komponenseihez (**erőforráscsoport**, **webalkalmazás**, **adatbázis**, és **előfizetés**) és adja meg vagy válassza ki a szükséges hello értékeket mezők:
   
   * Adjon meg egy URL-címet a választott    
   * Adatbázis-kiszolgáló hitelesítő adatok beállítása
   * Válassza ki a hello régió legközelebbi tooyou
     
     ![az alkalmazás konfigurálása](./media/web-sites-php-sql-database-deploy-use-git/configure-db-settings.png)
7. Ha befejezte a hello webalkalmazás meghatározása, kattintson a **létrehozása**.
   
    Hello webalkalmazás létrehozásakor hello **értesítések** gomb villogjon, egy zöld **sikeres** és erőforrás-csoport panel megnyitása tooshow hello mindkét hello web app és hello SQL-adatbázis hello csoportban.
8. Kattintson a hello erőforrás csoport panel tooopen hello webalkalmazása panelén hello webes alkalmazás ikonra.
   
    ![webes alkalmazás erőforráscsoport](./media/web-sites-php-sql-database-deploy-use-git/resource-group-blade.png)
9. A **beállítások** kattintson **folyamatos üzembe helyezés** > **kötelező beállítások konfigurálása**. Válassza ki **helyi Git-tárház** kattintson **OK**.
   
    ![hol található a Forráskód](./media/web-sites-php-sql-database-deploy-use-git/setup-local-git.png)
   
    Ha nem állított be egy Git-tárház előtt, meg kell adnia egy felhasználónevet és jelszót. toodo, kattintson **beállítások** > **üzembe helyezési hitelesítő adatok** a hello webalkalmazása panelén.
   
    ![](./media/web-sites-php-sql-database-deploy-use-git/deployment-credentials.png)
10. A **beállítások** kattintson a **tulajdonságok** toosee hello Git távoli URL-cím toouse toodeploy a PHP-alkalmazásokban később szüksége.

## <a name="get-sql-database-connection-information"></a>SQL adatbázis-kapcsolat adatainak lekérése
tooconnect toohello SQL Database-példányt, amely csatolva tooyour webalkalmazás, a fog kell hello hello adatbázis létrehozásakor megadott kapcsolati információt. tooget hello SQL adatbázis-kapcsolat adatainak, kövesse az alábbi lépéseket:

1. Hello erőforráscsoport panelen kattintson hello SQL adatbázis ikonjára.
2. Hello SQL adatbázis paneljén kattintson **beállítások** > **tulajdonságok**, majd kattintson a **adatbázis-kapcsolati karakterláncok megjelenítése**. 
   
    ![Adatbázis-tulajdonságok megtekintése](./media/web-sites-php-sql-database-deploy-use-git/view-database-properties.png)
3. A hello **PHP** szakasz hello eredményül kapott párbeszédpanel, jegyezze fel a hello értékeinek `Server`, `SQL Database`, és `User Name`. Ha később közzététele a PHP webes alkalmazások tooAzure App Service ezeket az értékeket fogja használni.

## <a name="build-and-test-your-application-locally"></a>Az alkalmazás helyi összeállítása és tesztelése
hello regisztrációs alkalmazása, amely lehetővé teszi a név és e-mail cím megadásával az esemény tooregister egyszerű PHP-alkalmazások. Előző igénylők kapcsolatos információkat a táblázatban jelennek meg. Regisztrációs adatai egy SQL Database-példányt. hello alkalmazás két fájlt (másolás/beillesztés kód alatt érhető el) áll:

* **index.php**: regisztráció és a bejegyzés adatokat tartalmazó táblát űrlap jeleníti meg.
* **createtable.php**: hello SQL-adatbázis táblája hello alkalmazáshoz hoz létre. Ezt a fájlt csak egyszer használható.

toorun hello alkalmazás helyi, kövesse a hello lépéseket. Vegye figyelembe, hogy ezek a lépések feltételezik, PHP rendelkezik, és az SQL Server Express állítsa be a helyi számítógépen, és engedélyezte hello [OEM-bővítmény SQL Server][pdo-sqlsrv].

1. Hozzon létre egy SQL Server-adatbázis nevű `registration`. Ehhez a hello `sqlcmd` parancssor a következő parancsokkal:
   
        >sqlcmd -S localhost\sqlexpress -U <local user name> -P <local password>
        1> create database registration
        2> GO    
2. Az alkalmazás gyökérkönyvtárában, hozzon létre két fájlokat - egy `createtable.php` és egy `index.php`.
3. Nyissa meg hello `createtable.php` fájlt egy szövegszerkesztőben, vagy IDE, és adja hozzá az alábbi hello kódot. Ez a kód lesz használt toocreate hello `registration_tbl` hello táblájában `registration` adatbázis.
   
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
   
    Vegye figyelembe, hogy szüksége lesz a tooupdate hello értékeinek <code>$user</code> és <code>$pwd</code> a helyi SQL Server felhasználói nevét és jelszavát.
4. A terminálban hello annak gyökérkönyvtárában hello alkalmazás típusa hello a következő parancsot:
   
        php -S localhost:8000
5. Nyisson meg egy webböngészőt, és keresse meg a túl**http://localhost:8000/createtable.php**. Ezzel létrehoz hello `registration_tbl` hello adatbázis táblájában.
6. Nyissa meg hello **index.php** fájlt egy szövegszerkesztőben, vagy IDE, és adja hozzá a hello alapvető HTML- és CSS kód hello lap (hello PHP kód megkapja a későbbi lépésekben).
   
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
7. Hello PHP címkék, belül adja hozzá a PHP kódot toohello adatbázis csatlakozni.
   
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
   
    Ebben az esetben szüksége lesz tooupdate hello értékeinek <code>$user</code> és <code>$pwd</code> a helyi MySQL-felhasználó nevét és jelszavát.
8. Következő hello adatbázis kapcsolati kódja vegye fel a regisztrációs adatok beszúrása hello adatbázis kódot.
   
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
9. Végül követően a fenti hello kódot, adja hozzá az adatok lekérdezése hello adatbázis kódját.
   
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

Most már megkeresheti túl**http://localhost:8000/index.php** tootest hello alkalmazás.

## <a name="publish-your-application"></a>Az alkalmazás közzététele
Az alkalmazás helyi tesztelése, után közzéteheti azt tooApp Service Web Apps Git használatával. Azonban először tooupdate hello adatbázis-kapcsolat adatainak hello alkalmazásban. Korábban beszerzett hello adatbázis-kapcsolat adatainak használatával (a hello **első SQL-adatbázis-kapcsolat adatainak** szakaszban), a következő információkat a frissítés hello **mindkét** hello `createdatabase.php` és `index.php` hello fájlokat a megfelelő értékeket:

    // DB connection info
    $host = "tcp:<value of Server>";
    $user = "<value of User Name>";
    $pwd = "<your password>";
    $db = "<value of SQL Database>";

> [!NOTE]
> A hello <code>$host</code>, kiszolgáló hello értéket kell $a, a <code>tcp:</code>.
> 
> 

Most már készen áll a tooset be Git-közzététel és hello alkalmazás közzététele.

> [!NOTE]
> Ezek a ugyanazokat a lépéseket az áttelepítés előtt feljegyzett hello hello végén hello **Azure-webalkalmazás létrehozása és beállítása a Git-közzététel** fenti szakaszban.
> 
> 

1. Nyissa meg a GitBash (vagy egy terminált, ha Git a `PATH`), módosítsa a könyvtárakat toohello az alkalmazás gyökérkönyvtárában (hello **regisztrációs** directory), és futtatási hello a következő parancsokat:
   
        git init
        git add .
        git commit -m "initial commit"
        git remote add azure [URL for remote repository]
        git push azure master
   
    A korábban létrehozott hello jelszó bekéri.
2. Keresse meg a túl**http://[web app name].azurewebsites.net/createtable.php** toocreate hello SQL-adatbázistáblában szereplő hello alkalmazáshoz.
3. Keresse meg a túl**http://[web app name].azurewebsites.net/index.php** toobegin hello alkalmazás segítségével.

Miután közzétette az alkalmazást, módosításokat tooit állapotában, és Git toopublish használja őket. 

## <a name="publish-changes-tooyour-application"></a>Módosítások tooyour alkalmazás közzététele
toopublish változik tooapplication, kövesse az alábbi lépéseket:

1. Módosításokat tooyour alkalmazás helyi.
2. Nyissa meg a GitBash (vagy a Terminálszolgáltatások informatikai Git van a `PATH`), módosítsa a könyvtárakat toohello az alkalmazás gyökérkönyvtárában, és futtassa a következő parancsok hello:
   
        git add .
        git commit -m "comment describing changes"
        git push azure master
   
    A korábban létrehozott hello jelszó bekéri.
3. Keresse meg a túl**http://[web app name].azurewebsites.net/index.php** toosee a módosításokat.

## <a name="whats-changed"></a>A változások
* Egy útmutató toohello webhelyek tooApp szolgáltatás változás lásd: [Azure App Service és a hatása a meglévő Azure-szolgáltatások](http://go.microsoft.com/fwlink/?LinkId=529714)

[install-php]: http://www.php.net/manual/en/install.php
[install-SQLExpress]: http://www.microsoft.com/download/details.aspx?id=29062
[install-Drivers]: http://www.microsoft.com/download/details.aspx?id=20098
[install-git]: http://git-scm.com/
[pdo-sqlsrv]: http://php.net/pdo_sqlsrv

