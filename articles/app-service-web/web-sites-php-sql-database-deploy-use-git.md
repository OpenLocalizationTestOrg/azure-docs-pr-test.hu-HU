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
# <a name="create-a-php-sql-web-app-and-deploy-to-azure-app-service-using-git"></a>PHP-SQL-webalkalmazás létrehozása és telepítése az Azure App Service Git használatával
Ez az oktatóanyag bemutatja, hogyan hozzon létre egy PHP webalkalmazást a [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) , amely kapcsolódik az Azure SQL Database és a Git használatával telepítés módját. Ez az oktatóanyag feltételezi, hogy rendelkezik [PHP][install-php], [SQL Server Express][install-SQLExpress], a [Microsoft SQL Server php Drivers](http://www.microsoft.com/download/en/details.aspx?id=20098), és [Git] [ install-git] telepítve a számítógépre. Ez az útmutató befejezése után fog egy PHP-SQL Azure-ban futó webalkalmazás.

> [!NOTE]
> Telepítse és a PHP az SQL Server PHP, SQL Server Express és a Microsoft Drivers konfigurálása a [Microsoft Webplatform-telepítő](http://www.microsoft.com/web/downloads/platform.aspx).
> 
> 

Az oktatóanyagban érintett témák köre:

* Azure-webalkalmazás és egy SQL-adatbázis használata a [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715). A PHP alapértelmezés szerint engedélyezve van az App Service Web Apps, mert semmi különleges kell futnia a PHP-kódot.
* Hogyan közzététele az Azure Git használatával az alkalmazás közzétételét.

Az oktatóanyag utasításait követve egy egyszerű regisztrációs webalkalmazásából PHP fog létrehozni. Az alkalmazás üzemel az Azure-webhely. A kész alkalmazás képernyőfelvételének alatt van:

![Az Azure PHP-webhely](./media/web-sites-php-sql-database-deploy-use-git/running_app_3.png)

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

> [!NOTE]
> Ha az Azure App Service-t az Azure-fiók regisztrálása előtt szeretné kipróbálni, ugorjon [Az Azure App Service kipróbálása](https://azure.microsoft.com/try/app-service/) oldalra. Itt azonnal létrehozhat egy ideiglenes, kezdő szintű webalkalmazást az App Service szolgáltatásban. Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.
> 
> 

## <a name="create-an-azure-web-app-and-set-up-git-publishing"></a>Azure-webalkalmazás létrehozása és beállítása a Git-közzététel
Kövesse az alábbi lépéseket az Azure web app és az SQL-adatbázis létrehozásához:

1. Jelentkezzen be az [Azure Portal](https://portal.azure.com/).
2. Nyissa meg az Azure piactéren kattintva a **új** az irányítópult bal felső ikonra, kattintson a **kijelölése az összes** mellett a piactéren, és kiválasztja **Web + mobil**.
3. Válassza ki a piactér **Web + mobil**.
4. Kattintson a **webes alkalmazás + SQL** ikonra.
5. A webes alkalmazás + SQL alkalmazás leírásának elolvasása után válassza ki a **létrehozása**.
6. Kattintson az egyes komponenseihez (**erőforráscsoport**, **webalkalmazás**, **adatbázis**, és **előfizetés**), és adja meg vagy válassza ki a kötelező mezőket értékeit:
   
   * Adjon meg egy URL-címet a választott    
   * Adatbázis-kiszolgáló hitelesítő adatok beállítása
   * Válassza ki az Önhöz legközelebbi régiót
     
     ![az alkalmazás konfigurálása](./media/web-sites-php-sql-database-deploy-use-git/configure-db-settings.png)
7. Ha elkészült, a webes alkalmazás meghatározása, kattintson **létrehozása**.
   
    A webalkalmazás létrehozásakor a **értesítések** gomb villogjon, egy zöld **sikeres** és az erőforráscsoport panel nyissa meg a csoport a web app és az SQL-adatbázis egyaránt megjeleníthető.
8. Kattintson a webalkalmazás az erőforráscsoport panel megnyitása a webalkalmazása panelén.
   
    ![webes alkalmazás erőforráscsoport](./media/web-sites-php-sql-database-deploy-use-git/resource-group-blade.png)
9. A **beállítások** kattintson **folyamatos üzembe helyezés** > **kötelező beállítások konfigurálása**. Válassza ki **helyi Git-tárház** kattintson **OK**.
   
    ![hol található a Forráskód](./media/web-sites-php-sql-database-deploy-use-git/setup-local-git.png)
   
    Ha nem állított be egy Git-tárház előtt, meg kell adnia egy felhasználónevet és jelszót. Ehhez kattintson **beállítások** > **üzembe helyezési hitelesítő adatok** a webalkalmazás panelen.
   
    ![](./media/web-sites-php-sql-database-deploy-use-git/deployment-credentials.png)
10. A **beállítások** kattintson a **tulajdonságok** a Git távoli URL-címet kell használnia a PHP-alkalmazás telepítése később megjelenítéséhez.

## <a name="get-sql-database-connection-information"></a>SQL adatbázis-kapcsolat adatainak lekérése
Az SQL Database-példányt, amely csatolva van a web app alkalmazásban a fog csatlakozni kell a kapcsolati adatokat, az adatbázis létrehozásakor megadott. Az SQL adatbázis-kapcsolat adatainak lekéréséhez kövesse az alábbi lépéseket:

1. Az erőforráscsoport panelen kattintson az SQL-adatbázis ikonra.
2. Az SQL-adatbázis paneljén kattintson **beállítások** > **tulajdonságok**, majd kattintson a **adatbázis-kapcsolati karakterláncok megjelenítése**. 
   
    ![Adatbázis-tulajdonságok megtekintése](./media/web-sites-php-sql-database-deploy-use-git/view-database-properties.png)
3. Az a **PHP** szakaszban, a megjelenő párbeszédpanelen jegyezze fel a értékeit `Server`, `SQL Database`, és `User Name`. Ezeket az értékeket fogja használni később, amikor a PHP-webalkalmazás közzététele az Azure App Service.

## <a name="build-and-test-your-application-locally"></a>Az alkalmazás helyi összeállítása és tesztelése
A regisztrációs alkalmazás egy egyszerű PHP-alkalmazás, amely lehetővé teszi, hogy regisztrálja az esemény a név és e-mail cím megadásával. Előző igénylők kapcsolatos információkat a táblázatban jelennek meg. Regisztrációs adatai egy SQL Database-példányt. Az alkalmazás két fájlt (másolás/beillesztés kód alatt érhető el) áll:

* **index.php**: regisztráció és a bejegyzés adatokat tartalmazó táblát űrlap jeleníti meg.
* **createtable.php**: az SQL-adatbázis táblája az alkalmazás létrehozza. Ezt a fájlt csak egyszer használható.

Az alkalmazás helyileg történő futtatása, kövesse az alábbi lépéseket. Vegye figyelembe, hogy ezek a lépések feltételezik, PHP és az SQL Server Express állítsa be a helyi számítógépen, és engedélyezte a [OEM-bővítmény SQL Server][pdo-sqlsrv].

1. Hozzon létre egy SQL Server-adatbázis nevű `registration`. Az ehhez a `sqlcmd` parancssor a következő parancsokkal:
   
        >sqlcmd -S localhost\sqlexpress -U <local user name> -P <local password>
        1> create database registration
        2> GO    
2. Az alkalmazás gyökérkönyvtárában, hozzon létre két fájlokat - egy `createtable.php` és egy `index.php`.
3. Nyissa meg a `createtable.php` fájlt egy szövegszerkesztőben, vagy IDE, és adja hozzá az alábbi kódot. Ez a kód létrehozásához használható a `registration_tbl` a tábla a `registration` adatbázis.
   
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
   
    Vegye figyelembe, hogy értéket kell <code>$user</code> és <code>$pwd</code> a helyi SQL Server felhasználói nevét és jelszavát.
4. Parancsot egy terminálban az alkalmazás gyökérkönyvtárában, írja be a következő parancsot:
   
        php -S localhost:8000
5. Nyisson meg egy webböngészőt, és keresse meg a **http://localhost:8000/createtable.php**. Ekkor létrejön az `registration_tbl` tábla az adatbázisban.
6. Nyissa meg a **index.php** fájlt egy szövegszerkesztőben, vagy IDE, és adja hozzá az alapvető HTML- és CSS kódot (a PHP kód megkapja a későbbi lépésekben) lap.
   
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
7. A PHP címkék belül adja hozzá a PHP kódot az adatbázishoz való kapcsolódáshoz.
   
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
   
    Ebben az esetben szüksége lesz a értéket <code>$user</code> és <code>$pwd</code> a helyi MySQL-felhasználó nevét és jelszavát.
8. Követően az adatbázis-kapcsolat kód adja hozzá a kódot a regisztrációs adatokat beszúrni az adatbázisba.
   
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
9. Végezetül a fenti kódot követően adja hozzá a kódot az adatokat az adatbázisból.
   
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

Most tallózással megkereshet **http://localhost:8000/index.php** kell tesztelni az alkalmazást.

## <a name="publish-your-application"></a>Az alkalmazás közzététele
Az alkalmazás helyi tesztelése, után közzéteheti az App Service Web Apps Git használatával. Azonban először az alkalmazást az adatbázis-kapcsolat adatainak frissítése. Korábban beszerzett adatbázis kapcsolati információk használatával (a a **első SQL-adatbázis-kapcsolat adatainak** szakaszban), a következő információ frissítésére **mindkét** a `createdatabase.php` és `index.php` fájlokat a megfelelő értékekkel:

    // DB connection info
    $host = "tcp:<value of Server>";
    $user = "<value of User Name>";
    $pwd = "<your password>";
    $db = "<value of SQL Database>";

> [!NOTE]
> Az a <code>$host</code>, a kiszolgáló értéke kell $a, a <code>tcp:</code>.
> 
> 

Most már készen áll Git-közzététel beállítását, és tegye közzé az alkalmazást.

> [!NOTE]
> Ezek a végén az áttelepítés előtt feljegyzett ugyanazokat a lépéseket a **Azure-webalkalmazás létrehozása és beállítása a Git-közzététel** fenti szakaszban.
> 
> 

1. Nyissa meg a GitBash (vagy egy terminált, ha Git a `PATH`), módosítsa a könyvtárat az alkalmazás gyökérkönyvtárában (a **regisztrációs** directory), és futtassa a következő parancsokat:
   
        git init
        git add .
        git commit -m "initial commit"
        git remote add azure [URL for remote repository]
        git push azure master
   
    Fogja kérni fogja a korábban létrehozott jelszót.
2. Keresse meg a **http://[web app name].azurewebsites.net/createtable.php** az SQL-adatbázistáblában szereplő, az alkalmazás létrehozásához.
3. Keresse meg a **http://[web app name].azurewebsites.net/index.php** az alkalmazás használatának megkezdéséhez.

Miután közzétette az alkalmazást, annak módosításával kezdődik, és a Git segítségével közzéteszi. 

## <a name="publish-changes-to-your-application"></a>Alkalmazásmódosítások közzététele
Változások közzétételére alkalmazást, kövesse az alábbi lépéseket:

1. Az alkalmazás helyi módosíthatja.
2. Nyissa meg a GitBash (vagy a Terminálszolgáltatások informatikai Git van a `PATH`), módosítsa a könyvtárat az alkalmazás gyökérkönyvtárában, és futtassa a következő parancsokat:
   
        git add .
        git commit -m "comment describing changes"
        git push azure master
   
    Fogja kérni fogja a korábban létrehozott jelszót.
3. Keresse meg a **http://[web app name].azurewebsites.net/index.php** szeretné látni a változtatásokat.

## <a name="whats-changed"></a>A változások
* Információk a Websites szolgáltatásról az App Service-re való váltásról: [Az Azure App Service és a hatása a meglévő Azure-szolgáltatásokra](http://go.microsoft.com/fwlink/?LinkId=529714)

[install-php]: http://www.php.net/manual/en/install.php
[install-SQLExpress]: http://www.microsoft.com/download/details.aspx?id=29062
[install-Drivers]: http://www.microsoft.com/download/details.aspx?id=20098
[install-git]: http://git-scm.com/
[pdo-sqlsrv]: http://php.net/pdo_sqlsrv

