---
title: "Csatlakozás a MySQL-hez készült Azure Database-hez a PHP segítségével | Microsoft Docs"
description: "Ebben a gyors útmutatóban számos PHP-kódmintát biztosítunk a MySQL-hez készült Azure Database-ről való csatlakozáshoz és adatlekérdezéshez."
services: mysql
author: mswutao
ms.author: wuta
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.custom: mvc
ms.topic: hero-article
ms.date: 07/12/2017
ms.openlocfilehash: 59da1ab9e76685d7ed0c4415ef99578c982e956c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="azure-database-for-mysql-use-php-to-connect-and-query-data"></a><span data-ttu-id="c4901-103">A MySQL-hez készült Azure Database: Csatlakozás és adatlekérdezés a PHP használatával</span><span class="sxs-lookup"><span data-stu-id="c4901-103">Azure Database for MySQL: Use PHP to connect and query data</span></span>
<span data-ttu-id="c4901-104">Ebben a gyors útmutatóban azt szemléltetjük, hogy miként lehet [PHP](http://php.net/manual/intro-whatis.php)-alkalmazás használatával csatlakozni a MySQL-hez készült Azure Database-hez.</span><span class="sxs-lookup"><span data-stu-id="c4901-104">This quickstart demonstrates how to connect to an Azure Database for MySQL using a [PHP](http://php.net/manual/intro-whatis.php) application.</span></span> <span data-ttu-id="c4901-105">Bemutatjuk, hogy az SQL-utasítások használatával hogyan kérdezhetők le, illeszthetők be, frissíthetők és törölhetők az adatok az adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="c4901-105">It shows how to use SQL statements to query, insert, update, and delete data in the database.</span></span> <span data-ttu-id="c4901-106">A jelen cikk azt feltételezi, hogy Ön már rendelkezik fejlesztési tapasztalatokkal a PHP használatával kapcsolatosan, de a MySQL-hez készült Azure Database használatában pedig még járatlan.</span><span class="sxs-lookup"><span data-stu-id="c4901-106">This article assumes you are familiar with development using PHP, but that you are new to working with Azure Database for MySQL.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c4901-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c4901-107">Prerequisites</span></span>
<span data-ttu-id="c4901-108">Ebben a rövid útmutatóban a következő útmutatók valamelyikében létrehozott erőforrásokat használunk kiindulási pontként:</span><span class="sxs-lookup"><span data-stu-id="c4901-108">This quickstart uses the resources created in either of these guides as a starting point:</span></span>
- [<span data-ttu-id="c4901-109">Azure-adatbázis létrehozása MySQL-kiszolgálóhoz az Azure Portal használatával</span><span class="sxs-lookup"><span data-stu-id="c4901-109">Create an Azure Database for MySQL server using Azure portal</span></span>](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [<span data-ttu-id="c4901-110">Azure-adatbázis létrehozása MySQL-kiszolgálóhoz az Azure CLI használatával</span><span class="sxs-lookup"><span data-stu-id="c4901-110">Create an Azure Database for MySQL server using Azure CLI</span></span>](./quickstart-create-mysql-server-database-using-azure-cli.md)

## <a name="install-php"></a><span data-ttu-id="c4901-111">A PHP telepítése</span><span class="sxs-lookup"><span data-stu-id="c4901-111">Install PHP</span></span>
<span data-ttu-id="c4901-112">Telepítse a PHP-t saját kiszolgálón, vagy hozzon létre egy, a PHP-t tartalmazó Azure-[webalkalmazást](https://docs.microsoft.com/en-us/azure/app-service-web/app-service-web-overview).</span><span class="sxs-lookup"><span data-stu-id="c4901-112">Install PHP on your own server, or create an Azure [web app](https://docs.microsoft.com/en-us/azure/app-service-web/app-service-web-overview) that includes PHP.</span></span>

### <a name="macos"></a><span data-ttu-id="c4901-113">MacOS</span><span class="sxs-lookup"><span data-stu-id="c4901-113">MacOS</span></span>
- <span data-ttu-id="c4901-114">Töltse le a [PHP 7.1.4-es verzióját](http://php.net/downloads.php)</span><span class="sxs-lookup"><span data-stu-id="c4901-114">Download [PHP 7.1.4 version](http://php.net/downloads.php)</span></span>
- <span data-ttu-id="c4901-115">Telepítse a PHP-t, majd tekintse át a [PHP kézikönyvét](http://php.net/manual/install.macosx.php) a további konfiguráláshoz</span><span class="sxs-lookup"><span data-stu-id="c4901-115">Install PHP and refer to the [PHP manual](http://php.net/manual/install.macosx.php) for further configuration</span></span>

### <a name="linux-ubuntu"></a><span data-ttu-id="c4901-116">Linux (Ubuntu)</span><span class="sxs-lookup"><span data-stu-id="c4901-116">Linux (Ubuntu)</span></span>
- <span data-ttu-id="c4901-117">Töltse le a [PHP 7.1.4 non-thread safe (NTS) x64-es verzióját](http://php.net/downloads.php)</span><span class="sxs-lookup"><span data-stu-id="c4901-117">Download [PHP 7.1.4 non-thread safe (x64) version](http://php.net/downloads.php)</span></span>
- <span data-ttu-id="c4901-118">Telepítse a PHP-t, majd tekintse át a [PHP kézikönyvét](http://php.net/manual/install.unix.php) a további konfiguráláshoz</span><span class="sxs-lookup"><span data-stu-id="c4901-118">Install PHP and refer to the [PHP manual](http://php.net/manual/install.unix.php) for further configuration</span></span>

### <a name="windows"></a><span data-ttu-id="c4901-119">Windows</span><span class="sxs-lookup"><span data-stu-id="c4901-119">Windows</span></span>
- <span data-ttu-id="c4901-120">Töltse le a [PHP 7.1.4 non-thread safe (NTS) x64-es verzióját](http://windows.php.net/download#php-7.1)</span><span class="sxs-lookup"><span data-stu-id="c4901-120">Download [PHP 7.1.4 non-thread safe (x64) version](http://windows.php.net/download#php-7.1)</span></span>
- <span data-ttu-id="c4901-121">Telepítse a PHP-t, majd tekintse át a [PHP kézikönyvét](http://php.net/manual/install.windows.php) a további konfiguráláshoz</span><span class="sxs-lookup"><span data-stu-id="c4901-121">Install PHP and refer to the [PHP manual](http://php.net/manual/install.windows.php) for further configuration</span></span>

## <a name="get-connection-information"></a><span data-ttu-id="c4901-122">Kapcsolatadatok lekérése</span><span class="sxs-lookup"><span data-stu-id="c4901-122">Get connection information</span></span>
<span data-ttu-id="c4901-123">Kérje le a MySQL-hez készült Azure Database-hez való csatlakozáshoz szükséges kapcsolatadatokat.</span><span class="sxs-lookup"><span data-stu-id="c4901-123">Get the connection information needed to connect to the Azure Database for MySQL.</span></span> <span data-ttu-id="c4901-124">Ehhez szükség lesz a teljes kiszolgálónévre és bejelentkezési hitelesítő adatokra.</span><span class="sxs-lookup"><span data-stu-id="c4901-124">You need the fully qualified server name and login credentials.</span></span>

1. <span data-ttu-id="c4901-125">Jelentkezzen be az [Azure portálra](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="c4901-125">Log in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="c4901-126">A bal oldali ablaktáblán kattintson a **Minden erőforrás** lehetőségre, és keressen rá a létrehozott kiszolgálóra (például: **myserver4demo**).</span><span class="sxs-lookup"><span data-stu-id="c4901-126">In the left pane, click **All resources**, and then search for the server you have created (for example, **myserver4demo**).</span></span>
3. <span data-ttu-id="c4901-127">Kattintson a kiszolgálónévre.</span><span class="sxs-lookup"><span data-stu-id="c4901-127">Click the server name.</span></span>
4. <span data-ttu-id="c4901-128">Válassza a kiszolgáló **tulajdonságlapját**.</span><span class="sxs-lookup"><span data-stu-id="c4901-128">Select the server's **Properties** page.</span></span> <span data-ttu-id="c4901-129">Jegyezze fel a **Kiszolgálónevet** és a **Kiszolgáló-rendszergazdai bejelentkezési nevet**.</span><span class="sxs-lookup"><span data-stu-id="c4901-129">Make a note of the **Server name** and **Server admin login name**.</span></span>
 <span data-ttu-id="c4901-130">![A MySQL-hez készült Azure Database-kiszolgáló neve](./media/connect-php/1_server-properties-name-login.png)</span><span class="sxs-lookup"><span data-stu-id="c4901-130">![Azure Database for MySQL server name](./media/connect-php/1_server-properties-name-login.png)</span></span>
5. <span data-ttu-id="c4901-131">Amennyiben elfelejtette a kiszolgáló bejelentkezési adatait, lépjen az **Overview** (Áttekintés) oldalra, és itt megtudhatja a kiszolgáló rendszergazdájának bejelentkezési nevét, valamint szükség esetén visszaállíthatja a jelszót.</span><span class="sxs-lookup"><span data-stu-id="c4901-131">If you forget your server login information, navigate to the **Overview** page to view the Server admin login name and, if necessary, reset the password.</span></span>

## <a name="connect-and-create-a-table"></a><span data-ttu-id="c4901-132">Csatlakozás és tábla létrehozása</span><span class="sxs-lookup"><span data-stu-id="c4901-132">Connect and create a table</span></span>
<span data-ttu-id="c4901-133">A következő kód segítségével csatlakozzon, és hozzon létre egy táblát a **CREATE TABLE** SQL-utasítás használatával.</span><span class="sxs-lookup"><span data-stu-id="c4901-133">Use the following code to connect and create a table using **CREATE TABLE** SQL statement.</span></span> 

<span data-ttu-id="c4901-134">A kód a PHP beépített **MySQL Improved extension** (mysqli) osztályát használja.</span><span class="sxs-lookup"><span data-stu-id="c4901-134">The code uses the **MySQL Improved extension** (mysqli) class included in PHP.</span></span> <span data-ttu-id="c4901-135">A kód a következő metódusokat hívja a MySQL-hez való kapcsolódáshoz: [mysqli_init](http://php.net/manual/mysqli.init.php) és [mysqli_real_connect](http://php.net/manual/mysqli.real-connect.php).</span><span class="sxs-lookup"><span data-stu-id="c4901-135">The code call methods [mysqli_init](http://php.net/manual/mysqli.init.php) and [mysqli_real_connect](http://php.net/manual/mysqli.real-connect.php) to connect to MySQL.</span></span> <span data-ttu-id="c4901-136">Ezután a kód meghívja [mysqli_query](http://php.net/manual/mysqli.query.php) metódust a lekérdezés futtatásához.</span><span class="sxs-lookup"><span data-stu-id="c4901-136">Then it calls method [mysqli_query](http://php.net/manual/mysqli.query.php) to run the query.</span></span> <span data-ttu-id="c4901-137">Végül pedig a [mysqli_close](http://php.net/manual/mysqli.close.php) meghívásával bontja a kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="c4901-137">Then it calls method [mysqli_close](http://php.net/manual/mysqli.close.php) to close the connection.</span></span>

<span data-ttu-id="c4901-138">A host (gazdagép), a username (felhasználónév), a password (jelszó) és a db_name (adatbázisnév) paramétereket cserélje le a saját értékeire.</span><span class="sxs-lookup"><span data-stu-id="c4901-138">Replace the host, username, password, and db_name parameters with your own values.</span></span> 

```php
<?php
$host = 'myserver4demo.mysql.database.azure.com';
$username = 'myadmin@myserver4demo';
$password = 'your_password';
$db_name = 'your_database';

//Establishes the connection
$conn = mysqli_init();
mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306);
if (mysqli_connect_errno($conn)) {
die('Failed to connect to MySQL: '.mysqli_connect_error());
}

// Run the create table query
if (mysqli_query($conn, '
CREATE TABLE Products (
`Id` INT NOT NULL AUTO_INCREMENT ,
`ProductName` VARCHAR(200) NOT NULL ,
`Color` VARCHAR(50) NOT NULL ,
`Price` DOUBLE NOT NULL ,
PRIMARY KEY (`Id`)
);
')) {
printf("Table created\n");
}

//Close the connection
mysqli_close($conn);
?>
```

## <a name="insert-data"></a><span data-ttu-id="c4901-139">Adat beszúrása</span><span class="sxs-lookup"><span data-stu-id="c4901-139">Insert data</span></span>
<span data-ttu-id="c4901-140">Az alábbi kód használatával csatlakozhat, és adatokat illeszthet be egy **INSERT** SQL-utasítás segítségével.</span><span class="sxs-lookup"><span data-stu-id="c4901-140">Use the following code to connect and insert data using an **INSERT** SQL statement.</span></span>

<span data-ttu-id="c4901-141">A kód a PHP beépített **MySQL Improved extension** (mysqli) osztályát használja.</span><span class="sxs-lookup"><span data-stu-id="c4901-141">The code uses the **MySQL Improved extension** (mysqli) class included in PHP.</span></span> <span data-ttu-id="c4901-142">A kód a [mysqli_prepare](http://php.net/manual/mysqli.prepare.php) metódus használatával létrehoz egy előkészített beillesztési parancsot, majd a [mysqli_stmt_bind_param](http://php.net/manual/mysqli-stmt.bind-param.php) metódus használatával hozzáköti az egyes beillesztett oszlopokhoz a paramétereket.</span><span class="sxs-lookup"><span data-stu-id="c4901-142">The code uses method [mysqli_prepare](http://php.net/manual/mysqli.prepare.php) to create a prepared insert statement, then binds the parameters for each inserted column value using method [mysqli_stmt_bind_param](http://php.net/manual/mysqli-stmt.bind-param.php).</span></span> <span data-ttu-id="c4901-143">A kód a [mysqli_stmt_execute](http://php.net/manual/mysqli-stmt.execute.php) metódus használatával futtatja a parancsot, majd a [mysqli_stmt_close](http://php.net/manual/mysqli-stmt.close.php) metódussal lezárja az utasítást.</span><span class="sxs-lookup"><span data-stu-id="c4901-143">The code runs the statement using method [mysqli_stmt_execute](http://php.net/manual/mysqli-stmt.execute.php) and afterwards closes the statement using method [mysqli_stmt_close](http://php.net/manual/mysqli-stmt.close.php).</span></span>

<span data-ttu-id="c4901-144">A host (gazdagép), a username (felhasználónév), a password (jelszó) és a db_name (adatbázisnév) paramétereket cserélje le a saját értékeire.</span><span class="sxs-lookup"><span data-stu-id="c4901-144">Replace the host, username, password, and db_name parameters with your own values.</span></span> 

```php
<?php
$host = 'myserver4demo.mysql.database.azure.com';
$username = 'myadmin@myserver4demo';
$password = 'your_password';
$db_name = 'your_database';

//Establishes the connection
$conn = mysqli_init();
mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306);
if (mysqli_connect_errno($conn)) {
die('Failed to connect to MySQL: '.mysqli_connect_error());
}

//Create an Insert prepared statement and run it
$product_name = 'BrandNewProduct';
$product_color = 'Blue';
$product_price = 15.5;
if ($stmt = mysqli_prepare($conn, "INSERT INTO Products (ProductName, Color, Price) VALUES (?, ?, ?)")) {
mysqli_stmt_bind_param($stmt, 'ssd', $product_name, $product_color, $product_price);
mysqli_stmt_execute($stmt);
printf("Insert: Affected %d rows\n", mysqli_stmt_affected_rows($stmt));
mysqli_stmt_close($stmt);
}

// Close the connection
mysqli_close($conn);
?>
```

## <a name="read-data"></a><span data-ttu-id="c4901-145">Adatok olvasása</span><span class="sxs-lookup"><span data-stu-id="c4901-145">Read data</span></span>
<span data-ttu-id="c4901-146">Az alábbi kód használatával csatlakozhat és végezheti el az adatok olvasását az adott **SELECT** SQL-utasítás segítségével.</span><span class="sxs-lookup"><span data-stu-id="c4901-146">Use the following code to connect and read the data using a **SELECT** SQL statement.</span></span>  <span data-ttu-id="c4901-147">A kód a PHP beépített **MySQL Improved extension** (mysqli) osztályát használja.</span><span class="sxs-lookup"><span data-stu-id="c4901-147">The code uses the **MySQL Improved extension** (mysqli) class included in PHP.</span></span> <span data-ttu-id="c4901-148">A kód a [mysqli_query](http://php.net/manual/mysqli.query.php) metódus segítségével végrehajtja az SQL-lekérdezést, és a [mysqli_fetch_assoc](http://php.net/manual/mysqli-result.fetch-assoc.php) metódussal beolvassa az eredményül kapott sorokat.</span><span class="sxs-lookup"><span data-stu-id="c4901-148">The code uses method [mysqli_query](http://php.net/manual/mysqli.query.php) perform the sql query, and uses [mysqli_fetch_assoc](http://php.net/manual/mysqli-result.fetch-assoc.php) method to fetch the resulting rows.</span></span>

<span data-ttu-id="c4901-149">A host (gazdagép), a username (felhasználónév), a password (jelszó) és a db_name (adatbázisnév) paramétereket cserélje le a saját értékeire.</span><span class="sxs-lookup"><span data-stu-id="c4901-149">Replace the host, username, password, and db_name parameters with your own values.</span></span> 

```php
<?php
$host = 'myserver4demo.mysql.database.azure.com';
$username = 'myadmin@myserver4demo';
$password = 'your_password';
$db_name = 'your_database';

//Establishes the connection
$conn = mysqli_init();
mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306);
if (mysqli_connect_errno($conn)) {
die('Failed to connect to MySQL: '.mysqli_connect_error());
}

//Run the Select query
printf("Reading data from table: \n");
$res = mysqli_query($conn, 'SELECT * FROM Products');
while ($row = mysqli_fetch_assoc($res)) {
var_dump($row);
}

//Close the connection
mysqli_close($conn);
?>
```

## <a name="update-data"></a><span data-ttu-id="c4901-150">Adatok frissítése</span><span class="sxs-lookup"><span data-stu-id="c4901-150">Update data</span></span>
<span data-ttu-id="c4901-151">Az alábbi kód használatával csatlakozhat és végezheti el az adatok módosítását egy **UPDATE** SQL-utasítás segítségével.</span><span class="sxs-lookup"><span data-stu-id="c4901-151">Use the following code to connect and update the data using a **UPDATE** SQL statement.</span></span>

<span data-ttu-id="c4901-152">A kód a PHP beépített **MySQL Improved extension** (mysqli) osztályát használja.</span><span class="sxs-lookup"><span data-stu-id="c4901-152">The code uses the **MySQL Improved extension** (mysqli) class included in PHP.</span></span> <span data-ttu-id="c4901-153">A kód a [mysqli_prepare](http://php.net/manual/mysqli.prepare.php) metódus használatával létrehoz egy előkészített módosítási parancsot, majd a [mysqli_stmt_bind_param](http://php.net/manual/mysqli-stmt.bind-param.php) metódus használatával hozzáköti az egyes módosított oszlopokhoz a paramétereket.</span><span class="sxs-lookup"><span data-stu-id="c4901-153">The code uses method [mysqli_prepare](http://php.net/manual/mysqli.prepare.php) to create a prepared update statement, then binds the parameters for each updated column value using method [mysqli_stmt_bind_param](http://php.net/manual/mysqli-stmt.bind-param.php).</span></span> <span data-ttu-id="c4901-154">A kód a [mysqli_stmt_execute](http://php.net/manual/mysqli-stmt.execute.php) metódus használatával futtatja a parancsot, majd a [mysqli_stmt_close](http://php.net/manual/mysqli-stmt.close.php) metódussal lezárja az utasítást.</span><span class="sxs-lookup"><span data-stu-id="c4901-154">The code runs the statement using method [mysqli_stmt_execute](http://php.net/manual/mysqli-stmt.execute.php) and afterwards closes the statement using method [mysqli_stmt_close](http://php.net/manual/mysqli-stmt.close.php).</span></span>

<span data-ttu-id="c4901-155">A host (gazdagép), a username (felhasználónév), a password (jelszó) és a db_name (adatbázisnév) paramétereket cserélje le a saját értékeire.</span><span class="sxs-lookup"><span data-stu-id="c4901-155">Replace the host, username, password, and db_name parameters with your own values.</span></span> 

```php
<?php
$host = 'myserver4demo.mysql.database.azure.com';
$username = 'myadmin@myserver4demo';
$password = 'your_password';
$db_name = 'your_database';

//Establishes the connection
$conn = mysqli_init();
mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306);
if (mysqli_connect_errno($conn)) {
die('Failed to connect to MySQL: '.mysqli_connect_error());
}

//Run the Update statement
$product_name = 'BrandNewProduct';
$new_product_price = 15.1;
if ($stmt = mysqli_prepare($conn, "UPDATE Products SET Price = ? WHERE ProductName = ?")) {
mysqli_stmt_bind_param($stmt, 'ds', $new_product_price, $product_name);
mysqli_stmt_execute($stmt);
printf("Update: Affected %d rows\n", mysqli_stmt_affected_rows($stmt));

//Close the connection
mysqli_stmt_close($stmt);
}

mysqli_close($conn);
?>
```


## <a name="delete-data"></a><span data-ttu-id="c4901-156">Adat törlése</span><span class="sxs-lookup"><span data-stu-id="c4901-156">Delete data</span></span>
<span data-ttu-id="c4901-157">Az alábbi kód használatával csatlakozhat és végezheti el az adatok olvasását egy **DELETE** SQL-utasítás segítségével.</span><span class="sxs-lookup"><span data-stu-id="c4901-157">Use the following code to connect and read the data using a **DELETE** SQL statement.</span></span> 

<span data-ttu-id="c4901-158">A kód a PHP beépített **MySQL Improved extension** (mysqli) osztályát használja.</span><span class="sxs-lookup"><span data-stu-id="c4901-158">The code uses the **MySQL Improved extension** (mysqli) class included in PHP.</span></span> <span data-ttu-id="c4901-159">A kód a [mysqli_prepare](http://php.net/manual/mysqli.prepare.php) metódus használatával létrehoz egy előkészített törlési parancsot, majd hozzáköti a paramétereket a parancs where záradékához a [mysqli_stmt_bind_param](http://php.net/manual/mysqli-stmt.bind-param.php) metódus használatával.</span><span class="sxs-lookup"><span data-stu-id="c4901-159">The code uses method [mysqli_prepare](http://php.net/manual/mysqli.prepare.php) to create a prepared delete statement, then binds the parameters for the where clause in the statement using method [mysqli_stmt_bind_param](http://php.net/manual/mysqli-stmt.bind-param.php).</span></span> <span data-ttu-id="c4901-160">A kód a [mysqli_stmt_execute](http://php.net/manual/mysqli-stmt.execute.php) metódus használatával futtatja a parancsot, majd a [mysqli_stmt_close](http://php.net/manual/mysqli-stmt.close.php) metódussal lezárja az utasítást.</span><span class="sxs-lookup"><span data-stu-id="c4901-160">The code runs the statement using method [mysqli_stmt_execute](http://php.net/manual/mysqli-stmt.execute.php) and afterwards closes the statement using method [mysqli_stmt_close](http://php.net/manual/mysqli-stmt.close.php).</span></span>

<span data-ttu-id="c4901-161">A host (gazdagép), a username (felhasználónév), a password (jelszó) és a db_name (adatbázisnév) paramétereket cserélje le a saját értékeire.</span><span class="sxs-lookup"><span data-stu-id="c4901-161">Replace the host, username, password, and db_name parameters with your own values.</span></span> 

```php
<?php
$host = 'myserver4demo.mysql.database.azure.com';
$username = 'myadmin@myserver4demo';
$password = 'your_password';
$db_name = 'your_database';

//Establishes the connection
$conn = mysqli_init();
mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306);
if (mysqli_connect_errno($conn)) {
die('Failed to connect to MySQL: '.mysqli_connect_error());
}

//Run the Delete statement
$product_name = 'BrandNewProduct';
if ($stmt = mysqli_prepare($conn, "DELETE FROM Products WHERE ProductName = ?")) {
mysqli_stmt_bind_param($stmt, 's', $product_name);
mysqli_stmt_execute($stmt);
printf("Delete: Affected %d rows\n", mysqli_stmt_affected_rows($stmt));
mysqli_stmt_close($stmt);
}

//Close the connection
mysqli_close($conn);
?>
```

## <a name="next-steps"></a><span data-ttu-id="c4901-162">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c4901-162">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="c4901-163">PHP- és MySQL-webalkalmazás létrehozása az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="c4901-163">Build a PHP and MySQL web app in Azure</span></span>](../app-service-web/app-service-web-tutorial-php-mysql.md?toc=%2fazure%2fmysql%2ftoc.json)
