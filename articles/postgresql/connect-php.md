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
# <a name="azure-database-for-postgresql-use-php-tooconnect-and-query-data"></a><span data-ttu-id="4f326-103">Azure PostgreSQL-adatbázishoz: használja a PHP tooconnect és lekérdezési adatok</span><span class="sxs-lookup"><span data-stu-id="4f326-103">Azure Database for PostgreSQL: Use PHP tooconnect and query data</span></span>
<span data-ttu-id="4f326-104">A gyors üzembe helyezés bemutatja, hogyan tooconnect tooan Azure PostgreSQL az adatbázis egy [PHP](http://php.net/manual/intro-whatis.php) alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="4f326-104">This quickstart demonstrates how tooconnect tooan Azure Database for PostgreSQL using a [PHP](http://php.net/manual/intro-whatis.php) application.</span></span> <span data-ttu-id="4f326-105">Azt illusztrálja, hogyan toouse SQL utasítás tooquery beszúrási, frissítési és törlési hello adatbázis adatait.</span><span class="sxs-lookup"><span data-stu-id="4f326-105">It shows how toouse SQL statements tooquery, insert, update, and delete data in hello database.</span></span> <span data-ttu-id="4f326-106">Ez a cikk feltételezi, hogy jártas használata PHP-fejlesztési azonban, hogy új tooworking PostgreSQL az Azure-adatbázissal.</span><span class="sxs-lookup"><span data-stu-id="4f326-106">This article assumes you are familiar with development using PHP, but that you are new tooworking with Azure Database for PostgreSQL.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4f326-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="4f326-107">Prerequisites</span></span>
<span data-ttu-id="4f326-108">A gyors üzembe helyezés kiindulási pontként ezek az útmutatók valamelyikével létrehozott hello erőforrást használ:</span><span class="sxs-lookup"><span data-stu-id="4f326-108">This quickstart uses hello resources created in either of these guides as a starting point:</span></span>
- [<span data-ttu-id="4f326-109">DB létrehozása – portál</span><span class="sxs-lookup"><span data-stu-id="4f326-109">Create DB - Portal</span></span>](quickstart-create-server-database-portal.md)
- [<span data-ttu-id="4f326-110">DB létrehozása – Azure CLI</span><span class="sxs-lookup"><span data-stu-id="4f326-110">Create DB - Azure CLI</span></span>](quickstart-create-server-database-azure-cli.md)

## <a name="install-php"></a><span data-ttu-id="4f326-111">A PHP telepítése</span><span class="sxs-lookup"><span data-stu-id="4f326-111">Install PHP</span></span>
<span data-ttu-id="4f326-112">Telepítse a PHP-t a kiszolgálójára, vagy hozzon létre egy PHP-t tartalmazó Azure-[webalkalmazást](https://docs.microsoft.com/en-us/azure/app-service-web/app-service-web-overview).</span><span class="sxs-lookup"><span data-stu-id="4f326-112">Install PHP on your own server, or create an Azure [web app](https://docs.microsoft.com/en-us/azure/app-service-web/app-service-web-overview) that includes PHP.</span></span>

### <a name="windows"></a><span data-ttu-id="4f326-113">Windows</span><span class="sxs-lookup"><span data-stu-id="4f326-113">Windows</span></span>
- <span data-ttu-id="4f326-114">Töltse le a [PHP 7.1.4 non-thread safe (NTS) x64-es verzióját](http://windows.php.net/download#php-7.1)</span><span class="sxs-lookup"><span data-stu-id="4f326-114">Download [PHP 7.1.4 non-thread safe (x64) version](http://windows.php.net/download#php-7.1)</span></span>
- <span data-ttu-id="4f326-115">A PHP telepítése, majd tekintse át a toohello [PHP manuális](http://php.net/manual/install.windows.php) további konfiguráció</span><span class="sxs-lookup"><span data-stu-id="4f326-115">Install PHP and refer toohello [PHP manual](http://php.net/manual/install.windows.php) for further configuration</span></span>
- <span data-ttu-id="4f326-116">hello használ a hello **pgsql** osztály (ext/php_pgsql.dll) hello PHP-telepítés található.</span><span class="sxs-lookup"><span data-stu-id="4f326-116">hello code uses hello **pgsql** class (ext/php_pgsql.dll)  that is included in hello PHP installation.</span></span> 
- <span data-ttu-id="4f326-117">Engedélyezett hello **pgsql** hello php.ini konfigurációs fájlt, általában található bővítmény `C:\Program Files\PHP\v7.1\php.ini`.</span><span class="sxs-lookup"><span data-stu-id="4f326-117">Enabled hello **pgsql** extension by editing hello php.ini configuration file, typically located at `C:\Program Files\PHP\v7.1\php.ini`.</span></span> <span data-ttu-id="4f326-118">hello konfigurációs fájl tartalmaznia kell egy sor hello szövegre `extension=php_pgsql.so`.</span><span class="sxs-lookup"><span data-stu-id="4f326-118">hello configuration file should contain a line with hello text `extension=php_pgsql.so`.</span></span> <span data-ttu-id="4f326-119">Ha nem látható, hello szöveget, és mentse hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="4f326-119">If it is not shown, add hello text and save hello file.</span></span> <span data-ttu-id="4f326-120">Ha hello szöveg megtalálható, de megjegyzésként, pontosvesszővel válassza el előtaggal kezdődik állítsa vissza a hello szöveg hello pontosvessző eltávolításával.</span><span class="sxs-lookup"><span data-stu-id="4f326-120">If hello text is present, but commented with a semicolon prefix, uncomment hello text by removing hello semicolon.</span></span>

### <a name="linux-ubuntu"></a><span data-ttu-id="4f326-121">Linux (Ubuntu)</span><span class="sxs-lookup"><span data-stu-id="4f326-121">Linux (Ubuntu)</span></span>
- <span data-ttu-id="4f326-122">Töltse le a [PHP 7.1.4 non-thread safe (NTS) x64-es verzióját](http://php.net/downloads.php)</span><span class="sxs-lookup"><span data-stu-id="4f326-122">Download [PHP 7.1.4 non-thread safe (x64) version](http://php.net/downloads.php)</span></span> 
- <span data-ttu-id="4f326-123">A PHP telepítése, majd tekintse át a toohello [PHP manuális](http://php.net/manual/install.unix.php) további konfiguráció</span><span class="sxs-lookup"><span data-stu-id="4f326-123">Install PHP and refer toohello [PHP manual](http://php.net/manual/install.unix.php) for further configuration</span></span>
- <span data-ttu-id="4f326-124">hello használ a hello **pgsql** osztály (php_pgsql.so).</span><span class="sxs-lookup"><span data-stu-id="4f326-124">hello code uses hello **pgsql** class (php_pgsql.so).</span></span> <span data-ttu-id="4f326-125">Telepítse a `sudo apt-get install php-pgsql` futtatásával.</span><span class="sxs-lookup"><span data-stu-id="4f326-125">Install it by running `sudo apt-get install php-pgsql`.</span></span>
- <span data-ttu-id="4f326-126">Engedélyezett hello **pgsql** hello szerkesztésével bővítmény `/etc/php/7.0/mods-available/pgsql.ini` konfigurációs fájlt.</span><span class="sxs-lookup"><span data-stu-id="4f326-126">Enabled hello **pgsql** extension by editing hello `/etc/php/7.0/mods-available/pgsql.ini` configuration file.</span></span> <span data-ttu-id="4f326-127">hello konfigurációs fájl tartalmaznia kell egy sor hello szövegre `extension=php_pgsql.so`.</span><span class="sxs-lookup"><span data-stu-id="4f326-127">hello configuration file should contain a line with hello text `extension=php_pgsql.so`.</span></span> <span data-ttu-id="4f326-128">Ha nem látható, hello szöveget, és mentse hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="4f326-128">If it is not shown, add hello text and save hello file.</span></span> <span data-ttu-id="4f326-129">Ha hello szöveg megtalálható, de megjegyzésként, pontosvesszővel válassza el előtaggal kezdődik állítsa vissza a hello szöveg hello pontosvessző eltávolításával.</span><span class="sxs-lookup"><span data-stu-id="4f326-129">If hello text is present, but commented with a semicolon prefix, uncomment hello text by removing hello semicolon.</span></span>

### <a name="macos"></a><span data-ttu-id="4f326-130">MacOS</span><span class="sxs-lookup"><span data-stu-id="4f326-130">MacOS</span></span>
- <span data-ttu-id="4f326-131">Töltse le a [PHP 7.1.4-es verzióját](http://php.net/downloads.php)</span><span class="sxs-lookup"><span data-stu-id="4f326-131">Download [PHP 7.1.4 version](http://php.net/downloads.php)</span></span>
- <span data-ttu-id="4f326-132">A PHP telepítése, majd tekintse át a toohello [PHP manuális](http://php.net/manual/install.macosx.php) további konfiguráció</span><span class="sxs-lookup"><span data-stu-id="4f326-132">Install PHP and refer toohello [PHP manual](http://php.net/manual/install.macosx.php) for further configuration</span></span>

## <a name="get-connection-information"></a><span data-ttu-id="4f326-133">Kapcsolatadatok lekérése</span><span class="sxs-lookup"><span data-stu-id="4f326-133">Get connection information</span></span>
<span data-ttu-id="4f326-134">Hello kapcsolat szükséges információkat tooconnect toohello Azure adatbázis beolvasása PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="4f326-134">Get hello connection information needed tooconnect toohello Azure Database for PostgreSQL.</span></span> <span data-ttu-id="4f326-135">Teljesen minősített kiszolgáló nevét és a bejelentkezési hitelesítő adatokat hello van szüksége.</span><span class="sxs-lookup"><span data-stu-id="4f326-135">You need hello fully qualified server name and login credentials.</span></span>

1. <span data-ttu-id="4f326-136">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="4f326-136">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="4f326-137">A hello Azure-portálon a bal oldali menüből, kattintson az **összes erőforrás** , és keressen a létrehozott, például a hello server **mypgserver-20170401**.</span><span class="sxs-lookup"><span data-stu-id="4f326-137">From hello left-hand menu in Azure portal, click **All resources** and search for hello server you have created, such as **mypgserver-20170401**.</span></span>
3. <span data-ttu-id="4f326-138">Hello kiszolgáló nevére kattint **mypgserver-20170401**.</span><span class="sxs-lookup"><span data-stu-id="4f326-138">Click hello server name **mypgserver-20170401**.</span></span>
4. <span data-ttu-id="4f326-139">Jelölje be hello server **áttekintése** lap.</span><span class="sxs-lookup"><span data-stu-id="4f326-139">Select hello server's **Overview** page.</span></span> <span data-ttu-id="4f326-140">Jegyezze fel a hello **kiszolgálónév** és **kiszolgálói rendszergazda bejelentkezési név**.</span><span class="sxs-lookup"><span data-stu-id="4f326-140">Make a note of hello **Server name** and **Server admin login name**.</span></span>
 <span data-ttu-id="4f326-141">![PostgreSQL-hez készült Azure-adatbázis – Kiszolgáló-rendszergazdai bejelentkezés](./media/connect-php/1-connection-string.png)</span><span class="sxs-lookup"><span data-stu-id="4f326-141">![Azure Database for PostgreSQL - Server Admin Login](./media/connect-php/1-connection-string.png)</span></span>
5. <span data-ttu-id="4f326-142">Ha elfelejti a kiszolgálói bejelentkezési adatok, keresse meg a toohello **áttekintése** tooview hello kiszolgálói rendszergazda bejelentkezési név lapon, és ha szükséges, állítsa vissza a hello jelszót.</span><span class="sxs-lookup"><span data-stu-id="4f326-142">If you forget your server login information, navigate toohello **Overview** page tooview hello Server admin login name and, if necessary, reset hello password.</span></span>

## <a name="connect-and-create-a-table"></a><span data-ttu-id="4f326-143">Csatlakozás és tábla létrehozása</span><span class="sxs-lookup"><span data-stu-id="4f326-143">Connect and create a table</span></span>
<span data-ttu-id="4f326-144">Használjon hello alábbi code tooconnect, és hozzon létre egy táblát az **CREATE TABLE** SQL-utasítást, és **INSERT INTO** SQL utasítás tooadd sorok hello táblába.</span><span class="sxs-lookup"><span data-stu-id="4f326-144">Use hello following code tooconnect and create a table using **CREATE TABLE** SQL statement, followed by **INSERT INTO** SQL statements tooadd rows into hello table.</span></span>

<span data-ttu-id="4f326-145">kód telefonhívás hello [pg_connect()](http://php.net/manual/en/function.pg-connect.php) tooconnect tooAzure PostgreSQL-adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="4f326-145">hello code call method [pg_connect()](http://php.net/manual/en/function.pg-connect.php) tooconnect tooAzure Database for PostgreSQL.</span></span> <span data-ttu-id="4f326-146">Ezután a metódus meghívja [pg_query()](http://php.net/manual/en/function.pg-query.php) többször toorun több parancsokat, és [pg_last_error()](http://php.net/manual/en/function.pg-last-error.php) toocheck hello hiba esetén minden alkalommal, amikor adatokat.</span><span class="sxs-lookup"><span data-stu-id="4f326-146">Then it calls method [pg_query()](http://php.net/manual/en/function.pg-query.php) several times toorun several commands, and [pg_last_error()](http://php.net/manual/en/function.pg-last-error.php) toocheck hello details if an error occurred each time.</span></span> <span data-ttu-id="4f326-147">Ezután a metódus meghívja [pg_close()](http://php.net/manual/en/function.pg-close.php) tooclose hello kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="4f326-147">Then it calls method [pg_close()](http://php.net/manual/en/function.pg-close.php) tooclose hello connection.</span></span>

<span data-ttu-id="4f326-148">Cserélje le a hello `$host`, `$database`, `$user`, és `$password` paraméterek saját értékekkel.</span><span class="sxs-lookup"><span data-stu-id="4f326-148">Replace hello `$host`, `$database`, `$user`, and `$password` parameters with your own values.</span></span> 

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

## <a name="read-data"></a><span data-ttu-id="4f326-149">Adatok olvasása</span><span class="sxs-lookup"><span data-stu-id="4f326-149">Read data</span></span>
<span data-ttu-id="4f326-150">Használjon hello alábbi code tooconnect, és hello adatok segítségével olvassa a **kiválasztása** SQL-utasításban.</span><span class="sxs-lookup"><span data-stu-id="4f326-150">Use hello following code tooconnect and read hello data using a **SELECT** SQL statement.</span></span> 

 <span data-ttu-id="4f326-151">kód telefonhívás hello [pg_connect()](http://php.net/manual/en/function.pg-connect.php) tooconnect tooAzure PostgreSQL-adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="4f326-151">hello code call method [pg_connect()](http://php.net/manual/en/function.pg-connect.php) tooconnect tooAzure Database for PostgreSQL.</span></span> <span data-ttu-id="4f326-152">Ezután a metódus meghívja [pg_query()](http://php.net/manual/en/function.pg-query.php) toorun hello kijelölési parancs, hello eredmények megőrzi az eredményhalmazban, és [pg_last_error()](http://php.net/manual/en/function.pg-last-error.php) toocheck hello adatokat, ha hiba történt.</span><span class="sxs-lookup"><span data-stu-id="4f326-152">Then it calls method [pg_query()](http://php.net/manual/en/function.pg-query.php) toorun hello SELECT command, keeping hello results in a result set, and [pg_last_error()](http://php.net/manual/en/function.pg-last-error.php) toocheck hello details if an error occurred.</span></span>  <span data-ttu-id="4f326-153">tooread hello eredménykészlet, metódus [pg_fetch_row()](http://php.net/manual/en/function.pg-fetch-row.php) nevezik frissítették, miután minden sort, és hello sor adatokat beolvasni a tömbben `$row`, minden egyes tömb helyzetben oszloponként egy-egy értékkel.</span><span class="sxs-lookup"><span data-stu-id="4f326-153">tooread hello result set, method [pg_fetch_row()](http://php.net/manual/en/function.pg-fetch-row.php) is called in a loop, once per row, and hello row data is retrieved in an array `$row`, with one data value per column in each array position.</span></span>  <span data-ttu-id="4f326-154">toofree hello eredménykészlet, metódus [pg_free_result()](http://php.net/manual/en/function.pg-free-result.php) nevezik.</span><span class="sxs-lookup"><span data-stu-id="4f326-154">toofree hello result set, method [pg_free_result()](http://php.net/manual/en/function.pg-free-result.php) is called.</span></span> <span data-ttu-id="4f326-155">Ezután a metódus meghívja [pg_close()](http://php.net/manual/en/function.pg-close.php) tooclose hello kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="4f326-155">Then it calls method [pg_close()](http://php.net/manual/en/function.pg-close.php) tooclose hello connection.</span></span>

<span data-ttu-id="4f326-156">Cserélje le a hello `$host`, `$database`, `$user`, és `$password` paraméterek saját értékekkel.</span><span class="sxs-lookup"><span data-stu-id="4f326-156">Replace hello `$host`, `$database`, `$user`, and `$password` parameters with your own values.</span></span> 

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

## <a name="update-data"></a><span data-ttu-id="4f326-157">Adatok frissítése</span><span class="sxs-lookup"><span data-stu-id="4f326-157">Update data</span></span>
<span data-ttu-id="4f326-158">Használjon hello következő code tooconnect, és frissítse a hello adatok segítségével egy **frissítése** SQL-utasításban.</span><span class="sxs-lookup"><span data-stu-id="4f326-158">Use hello following code tooconnect and update hello data using a **UPDATE** SQL statement.</span></span>

<span data-ttu-id="4f326-159">kód telefonhívás hello [pg_connect()](http://php.net/manual/en/function.pg-connect.php) tooconnect tooAzure PostgreSQL-adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="4f326-159">hello code call method [pg_connect()](http://php.net/manual/en/function.pg-connect.php) tooconnect tooAzure Database for PostgreSQL.</span></span> <span data-ttu-id="4f326-160">Ezután a metódus meghívja [pg_query()](http://php.net/manual/en/function.pg-query.php) toorun egy parancsot, és [pg_last_error()](http://php.net/manual/en/function.pg-last-error.php) toocheck hello adatokat, ha hiba történt.</span><span class="sxs-lookup"><span data-stu-id="4f326-160">Then it calls method [pg_query()](http://php.net/manual/en/function.pg-query.php) toorun a command, and [pg_last_error()](http://php.net/manual/en/function.pg-last-error.php) toocheck hello details if an error occurred.</span></span> <span data-ttu-id="4f326-161">Ezután a metódus meghívja [pg_close()](http://php.net/manual/en/function.pg-close.php) tooclose hello kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="4f326-161">Then it calls method [pg_close()](http://php.net/manual/en/function.pg-close.php) tooclose hello connection.</span></span>

<span data-ttu-id="4f326-162">Cserélje le a hello `$host`, `$database`, `$user`, és `$password` paraméterek saját értékekkel.</span><span class="sxs-lookup"><span data-stu-id="4f326-162">Replace hello `$host`, `$database`, `$user`, and `$password` parameters with your own values.</span></span> 

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


## <a name="delete-data"></a><span data-ttu-id="4f326-163">Adat törlése</span><span class="sxs-lookup"><span data-stu-id="4f326-163">Delete data</span></span>
<span data-ttu-id="4f326-164">Használjon hello alábbi code tooconnect, és olvasott hello adatok egy **törlése** SQL-utasításban.</span><span class="sxs-lookup"><span data-stu-id="4f326-164">Use hello following code tooconnect and read hello data using a **DELETE** SQL statement.</span></span> 

 <span data-ttu-id="4f326-165">kód telefonhívás hello [pg_connect()](http://php.net/manual/en/function.pg-connect.php) tooconnect túl Azure a PostgreSQL-adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="4f326-165">hello code call method [pg_connect()](http://php.net/manual/en/function.pg-connect.php) tooconnect too Azure Database for PostgreSQL.</span></span> <span data-ttu-id="4f326-166">Ezután a metódus meghívja [pg_query()](http://php.net/manual/en/function.pg-query.php) toorun egy parancsot, és [pg_last_error()](http://php.net/manual/en/function.pg-last-error.php) toocheck hello adatokat, ha hiba történt.</span><span class="sxs-lookup"><span data-stu-id="4f326-166">Then it calls method [pg_query()](http://php.net/manual/en/function.pg-query.php) toorun a command, and [pg_last_error()](http://php.net/manual/en/function.pg-last-error.php) toocheck hello details if an error occurred.</span></span> <span data-ttu-id="4f326-167">Ezután a metódus meghívja [pg_close()](http://php.net/manual/en/function.pg-close.php) tooclose hello kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="4f326-167">Then it calls method [pg_close()](http://php.net/manual/en/function.pg-close.php) tooclose hello connection.</span></span>

<span data-ttu-id="4f326-168">Cserélje le a hello `$host`, `$database`, `$user`, és `$password` paraméterek saját értékekkel.</span><span class="sxs-lookup"><span data-stu-id="4f326-168">Replace hello `$host`, `$database`, `$user`, and `$password` parameters with your own values.</span></span> 

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

## <a name="next-steps"></a><span data-ttu-id="4f326-169">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4f326-169">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="4f326-170">Adatbázis migrálása exportálással és importálással</span><span class="sxs-lookup"><span data-stu-id="4f326-170">Migrate your database using Export and Import</span></span>](./howto-migrate-using-export-and-import.md)
