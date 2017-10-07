---
title: "a PHP használatával PostgreSQL adatbázis aaaConnect tooAzure |} Microsoft Docs"
description: "A gyors üzembe helyezés biztosít egy PHP kódminta PostgreSQL lekérdezése a Azure-adatbázis adatait és tooconnect használhatja."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.custom: mvc
ms.devlang: php
ms.topic: quickstart
ms.date: 06/29/2017
ms.openlocfilehash: 008505e837e37cb8c7fea3fc164b3446c3580e46
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-database-for-postgresql-use-php-tooconnect-and-query-data"></a>Azure PostgreSQL-adatbázishoz: használja a PHP tooconnect és lekérdezési adatok
A gyors üzembe helyezés bemutatja, hogyan tooconnect tooan Azure PostgreSQL az adatbázis egy [PHP](http://php.net/manual/intro-whatis.php) alkalmazás. Azt illusztrálja, hogyan toouse SQL utasítás tooquery beszúrási, frissítési és törlési hello adatbázis adatait. Ez a cikk feltételezi, hogy jártas használata PHP-fejlesztési azonban, hogy új tooworking PostgreSQL az Azure-adatbázissal.

## <a name="prerequisites"></a>Előfeltételek
A gyors üzembe helyezés kiindulási pontként ezek az útmutatók valamelyikével létrehozott hello erőforrást használ:
- [DB létrehozása – portál](quickstart-create-server-database-portal.md)
- [DB létrehozása – Azure CLI](quickstart-create-server-database-azure-cli.md)

## <a name="install-php"></a>A PHP telepítése
Telepítse a PHP-t a kiszolgálójára, vagy hozzon létre egy PHP-t tartalmazó Azure-[webalkalmazást](https://docs.microsoft.com/en-us/azure/app-service-web/app-service-web-overview).

### <a name="windows"></a>Windows
- Töltse le a [PHP 7.1.4 non-thread safe (NTS) x64-es verzióját](http://windows.php.net/download#php-7.1)
- A PHP telepítése, majd tekintse át a toohello [PHP manuális](http://php.net/manual/install.windows.php) további konfiguráció
- hello használ a hello **pgsql** osztály (ext/php_pgsql.dll) hello PHP-telepítés található. 
- Engedélyezett hello **pgsql** hello php.ini konfigurációs fájlt, általában található bővítmény `C:\Program Files\PHP\v7.1\php.ini`. hello konfigurációs fájl tartalmaznia kell egy sor hello szövegre `extension=php_pgsql.so`. Ha nem látható, hello szöveget, és mentse hello fájlt. Ha hello szöveg megtalálható, de megjegyzésként, pontosvesszővel válassza el előtaggal kezdődik állítsa vissza a hello szöveg hello pontosvessző eltávolításával.

### <a name="linux-ubuntu"></a>Linux (Ubuntu)
- Töltse le a [PHP 7.1.4 non-thread safe (NTS) x64-es verzióját](http://php.net/downloads.php) 
- A PHP telepítése, majd tekintse át a toohello [PHP manuális](http://php.net/manual/install.unix.php) további konfiguráció
- hello használ a hello **pgsql** osztály (php_pgsql.so). Telepítse a `sudo apt-get install php-pgsql` futtatásával.
- Engedélyezett hello **pgsql** hello szerkesztésével bővítmény `/etc/php/7.0/mods-available/pgsql.ini` konfigurációs fájlt. hello konfigurációs fájl tartalmaznia kell egy sor hello szövegre `extension=php_pgsql.so`. Ha nem látható, hello szöveget, és mentse hello fájlt. Ha hello szöveg megtalálható, de megjegyzésként, pontosvesszővel válassza el előtaggal kezdődik állítsa vissza a hello szöveg hello pontosvessző eltávolításával.

### <a name="macos"></a>MacOS
- Töltse le a [PHP 7.1.4-es verzióját](http://php.net/downloads.php)
- A PHP telepítése, majd tekintse át a toohello [PHP manuális](http://php.net/manual/install.macosx.php) további konfiguráció

## <a name="get-connection-information"></a>Kapcsolatadatok lekérése
Hello kapcsolat szükséges információkat tooconnect toohello Azure adatbázis beolvasása PostgreSQL. Teljesen minősített kiszolgáló nevét és a bejelentkezési hitelesítő adatokat hello van szüksége.

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).
2. A hello Azure-portálon a bal oldali menüből, kattintson az **összes erőforrás** , és keressen a létrehozott, például a hello server **mypgserver-20170401**.
3. Hello kiszolgáló nevére kattint **mypgserver-20170401**.
4. Jelölje be hello server **áttekintése** lap. Jegyezze fel a hello **kiszolgálónév** és **kiszolgálói rendszergazda bejelentkezési név**.
 ![PostgreSQL-hez készült Azure-adatbázis – Kiszolgáló-rendszergazdai bejelentkezés](./media/connect-php/1-connection-string.png)
5. Ha elfelejti a kiszolgálói bejelentkezési adatok, keresse meg a toohello **áttekintése** tooview hello kiszolgálói rendszergazda bejelentkezési név lapon, és ha szükséges, állítsa vissza a hello jelszót.

## <a name="connect-and-create-a-table"></a>Csatlakozás és tábla létrehozása
Használjon hello alábbi code tooconnect, és hozzon létre egy táblát az **CREATE TABLE** SQL-utasítást, és **INSERT INTO** SQL utasítás tooadd sorok hello táblába.

kód telefonhívás hello [pg_connect()](http://php.net/manual/en/function.pg-connect.php) tooconnect tooAzure PostgreSQL-adatbázisban. Ezután a metódus meghívja [pg_query()](http://php.net/manual/en/function.pg-query.php) többször toorun több parancsokat, és [pg_last_error()](http://php.net/manual/en/function.pg-last-error.php) toocheck hello hiba esetén minden alkalommal, amikor adatokat. Ezután a metódus meghívja [pg_close()](http://php.net/manual/en/function.pg-close.php) tooclose hello kapcsolat.

Cserélje le a hello `$host`, `$database`, `$user`, és `$password` paraméterek saját értékekkel. 

```php
<?php
    // Initialize connection variables.
    $host = "mypgserver-20170401.postgres.database.azure.com";
    $database = "mypgsqldb";
    $user = "mylogin@mypgserver-20170401";
    $password = "<server_admin_password>";

    // Initialize connection object.
    $connection = pg_connect("host=$host dbname=$database user=$user password=$password") 
        or die("Failed toocreate connection toodatabase: ". pg_last_error(). "<br/>");
    print "Successfully created connection toodatabase.<br/>";

    // Drop previous table of same name if one exists.
    $query = "DROP TABLE IF EXISTS inventory;";
    pg_query($connection, $query) 
        or die("Encountered an error when executing given sql statement: ". pg_last_error(). "<br/>");
    print "Finished dropping table (if existed).<br/>";

    // Create table.
    $query = "CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);";
    pg_query($connection, $query) 
        or die("Encountered an error when executing given sql statement: ". pg_last_error(). "<br/>");
    print "Finished creating table.<br/>";

    // Insert some data into table.
    $name = '\'banana\'';
    $quantity = 150;
    $query = "INSERT INTO inventory (name, quantity) VALUES ($1, $2);";
    pg_query($connection, $query) 
        or die("Encountered an error when executing given sql statement: ". pg_last_error(). "<br/>");

    $name = '\'orange\'';
    $quantity = 154;
    $query = "INSERT INTO inventory (name, quantity) VALUES ($name, $quantity);";
    pg_query($connection, $query) 
        or die("Encountered an error when executing given sql statement: ". pg_last_error(). "<br/>");

    $name = '\'apple\'';
    $quantity = 100;
    $query = "INSERT INTO inventory (name, quantity) VALUES ($name, $quantity);";
    pg_query($connection, $query) 
        or die("Encountered an error when executing given sql statement: ". pg_last_error()). "<br/>";

    print "Inserted 3 rows of data.<br/>";

    // Closing connection
    pg_close($connection);
?>
```

## <a name="read-data"></a>Adatok olvasása
Használjon hello alábbi code tooconnect, és hello adatok segítségével olvassa a **kiválasztása** SQL-utasításban. 

 kód telefonhívás hello [pg_connect()](http://php.net/manual/en/function.pg-connect.php) tooconnect tooAzure PostgreSQL-adatbázisban. Ezután a metódus meghívja [pg_query()](http://php.net/manual/en/function.pg-query.php) toorun hello kijelölési parancs, hello eredmények megőrzi az eredményhalmazban, és [pg_last_error()](http://php.net/manual/en/function.pg-last-error.php) toocheck hello adatokat, ha hiba történt.  tooread hello eredménykészlet, metódus [pg_fetch_row()](http://php.net/manual/en/function.pg-fetch-row.php) nevezik frissítették, miután minden sort, és hello sor adatokat beolvasni a tömbben `$row`, minden egyes tömb helyzetben oszloponként egy-egy értékkel.  toofree hello eredménykészlet, metódus [pg_free_result()](http://php.net/manual/en/function.pg-free-result.php) nevezik. Ezután a metódus meghívja [pg_close()](http://php.net/manual/en/function.pg-close.php) tooclose hello kapcsolat.

Cserélje le a hello `$host`, `$database`, `$user`, és `$password` paraméterek saját értékekkel. 

```php
<?php
    // Initialize connection variables.
    $host = "mypgserver-20170401.postgres.database.azure.com";
    $database = "mypgsqldb";
    $user = "mylogin@mypgserver-20170401";
    $password = "<server_admin_password>";
    
    // Initialize connection object.
    $connection = pg_connect("host=$host dbname=$database user=$user password=$password")
                or die("Failed toocreate connection toodatabase: ". pg_last_error(). "<br/>");

    print "Successfully created connection toodatabase. <br/>";

    // Perform some SQL queries over hello connection.
    $query = "SELECT * from inventory";
    $result_set = pg_query($connection, $query) 
        or die("Encountered an error when executing given sql statement: ". pg_last_error(). "<br/>");
    while ($row = pg_fetch_row($result_set))
    {
        print "Data row = ($row[0], $row[1], $row[2]). <br/>";
    }

    // Free result_set
    pg_free_result($result_set);

    // Closing connection
    pg_close($connection);
?>
```

## <a name="update-data"></a>Adatok frissítése
Használjon hello következő code tooconnect, és frissítse a hello adatok segítségével egy **frissítése** SQL-utasításban.

kód telefonhívás hello [pg_connect()](http://php.net/manual/en/function.pg-connect.php) tooconnect tooAzure PostgreSQL-adatbázisban. Ezután a metódus meghívja [pg_query()](http://php.net/manual/en/function.pg-query.php) toorun egy parancsot, és [pg_last_error()](http://php.net/manual/en/function.pg-last-error.php) toocheck hello adatokat, ha hiba történt. Ezután a metódus meghívja [pg_close()](http://php.net/manual/en/function.pg-close.php) tooclose hello kapcsolat.

Cserélje le a hello `$host`, `$database`, `$user`, és `$password` paraméterek saját értékekkel. 

```php
<?php
    // Initialize connection variables.
    $host = "mypgserver-20170401.postgres.database.azure.com";
    $database = "mypgsqldb";
    $user = "mylogin@mypgserver-20170401";
    $password = "<server_admin_password>";

    // Initialize connection object.
    $connection = pg_connect("host=$host dbname=$database user=$user password=$password")
                or die("Failed toocreate connection toodatabase: ". pg_last_error(). ".<br/>");

    print "Successfully created connection toodatabase. <br/>";

    // Modify some data in table.
    $new_quantity = 200;
    $name = '\'banana\'';
    $query = "UPDATE inventory SET quantity = $new_quantity WHERE name = $name;";
    pg_query($connection, $query) 
        or die("Encountered an error when executing given sql statement: ". pg_last_error(). ".<br/>");
    print "Updated 1 row of data. </br>";

    // Closing connection
    pg_close($connection);
?>
```


## <a name="delete-data"></a>Adat törlése
Használjon hello alábbi code tooconnect, és olvasott hello adatok egy **törlése** SQL-utasításban. 

 kód telefonhívás hello [pg_connect()](http://php.net/manual/en/function.pg-connect.php) tooconnect túl Azure a PostgreSQL-adatbázishoz. Ezután a metódus meghívja [pg_query()](http://php.net/manual/en/function.pg-query.php) toorun egy parancsot, és [pg_last_error()](http://php.net/manual/en/function.pg-last-error.php) toocheck hello adatokat, ha hiba történt. Ezután a metódus meghívja [pg_close()](http://php.net/manual/en/function.pg-close.php) tooclose hello kapcsolat.

Cserélje le a hello `$host`, `$database`, `$user`, és `$password` paraméterek saját értékekkel. 

```php
<?php
    // Initialize connection variables.
    $host = "mypgserver-20170401.postgres.database.azure.com";
    $database = "mypgsqldb";
    $user = "mylogin@mypgserver-20170401";
    $password = "<server_admin_password>";

    // Initialize connection object.
    $connection = pg_connect("host=$host dbname=$database user=$user password=$password")
            or die("Failed toocreate connection toodatabase: ". pg_last_error(). ". </br>");

    print "Successfully created connection toodatabase. <br/>";

    // Delete some data from table.
    $name = '\'orange\'';
    $query = "DELETE FROM inventory WHERE name = $name;";
    pg_query($connection, $query) 
        or die("Encountered an error when executing given sql statement: ". pg_last_error(). ". <br/>");
    print "Deleted 1 row of data. <br/>";

    // Closing connection
    pg_close($connection);
?>
```

## <a name="next-steps"></a>Következő lépések
> [!div class="nextstepaction"]
> [Adatbázis migrálása exportálással és importálással](./howto-migrate-using-export-and-import.md)
