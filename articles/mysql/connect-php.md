---
title: "Php-ből MySQL-adatbázis tooAzure kapcsolati |} Microsoft Docs"
description: "A gyors üzembe helyezés biztosít több PHP mintakódok tooconnect használhat és MySQL az Azure-adatbázis adatait lekérdezése."
services: mysql
author: mswutao
ms.author: wuta
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.custom: mvc
ms.topic: hero-article
ms.date: 07/12/2017
ms.openlocfilehash: b928748c473c1aef320ae2183f237b5b845e83f2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-database-for-mysql-use-php-tooconnect-and-query-data"></a><span data-ttu-id="839ee-103">MySQL az Azure-adatbázishoz: használja a PHP tooconnect és lekérdezési adatok</span><span class="sxs-lookup"><span data-stu-id="839ee-103">Azure Database for MySQL: Use PHP tooconnect and query data</span></span>
<span data-ttu-id="839ee-104">A gyors üzembe helyezés bemutatja, hogyan tooconnect tooan Azure adatbázis a MySQL használata egy [PHP](http://php.net/manual/intro-whatis.php) alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="839ee-104">This quickstart demonstrates how tooconnect tooan Azure Database for MySQL using a [PHP](http://php.net/manual/intro-whatis.php) application.</span></span> <span data-ttu-id="839ee-105">Azt illusztrálja, hogyan toouse SQL utasítás tooquery beszúrási, frissítési és törlési hello adatbázis adatait.</span><span class="sxs-lookup"><span data-stu-id="839ee-105">It shows how toouse SQL statements tooquery, insert, update, and delete data in hello database.</span></span> <span data-ttu-id="839ee-106">Ez a cikk feltételezi, hogy jártas használata PHP-fejlesztési azonban, hogy új tooworking MySQL az Azure-adatbázissal.</span><span class="sxs-lookup"><span data-stu-id="839ee-106">This article assumes you are familiar with development using PHP, but that you are new tooworking with Azure Database for MySQL.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="839ee-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="839ee-107">Prerequisites</span></span>
<span data-ttu-id="839ee-108">A gyors üzembe helyezés kiindulási pontként ezek az útmutatók valamelyikével létrehozott hello erőforrást használ:</span><span class="sxs-lookup"><span data-stu-id="839ee-108">This quickstart uses hello resources created in either of these guides as a starting point:</span></span>
- [<span data-ttu-id="839ee-109">Azure-adatbázis létrehozása MySQL-kiszolgálóhoz az Azure Portal használatával</span><span class="sxs-lookup"><span data-stu-id="839ee-109">Create an Azure Database for MySQL server using Azure portal</span></span>](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [<span data-ttu-id="839ee-110">Azure-adatbázis létrehozása MySQL-kiszolgálóhoz az Azure CLI használatával</span><span class="sxs-lookup"><span data-stu-id="839ee-110">Create an Azure Database for MySQL server using Azure CLI</span></span>](./quickstart-create-mysql-server-database-using-azure-cli.md)

## <a name="install-php"></a><span data-ttu-id="839ee-111">A PHP telepítése</span><span class="sxs-lookup"><span data-stu-id="839ee-111">Install PHP</span></span>
<span data-ttu-id="839ee-112">Telepítse a PHP-t saját kiszolgálón, vagy hozzon létre egy, a PHP-t tartalmazó Azure-[webalkalmazást](https://docs.microsoft.com/en-us/azure/app-service-web/app-service-web-overview).</span><span class="sxs-lookup"><span data-stu-id="839ee-112">Install PHP on your own server, or create an Azure [web app](https://docs.microsoft.com/en-us/azure/app-service-web/app-service-web-overview) that includes PHP.</span></span>

### <a name="macos"></a><span data-ttu-id="839ee-113">MacOS</span><span class="sxs-lookup"><span data-stu-id="839ee-113">MacOS</span></span>
- <span data-ttu-id="839ee-114">Töltse le a [PHP 7.1.4-es verzióját](http://php.net/downloads.php)</span><span class="sxs-lookup"><span data-stu-id="839ee-114">Download [PHP 7.1.4 version](http://php.net/downloads.php)</span></span>
- <span data-ttu-id="839ee-115">A PHP telepítése, majd tekintse át a toohello [PHP manuális](http://php.net/manual/install.macosx.php) további konfiguráció</span><span class="sxs-lookup"><span data-stu-id="839ee-115">Install PHP and refer toohello [PHP manual](http://php.net/manual/install.macosx.php) for further configuration</span></span>

### <a name="linux-ubuntu"></a><span data-ttu-id="839ee-116">Linux (Ubuntu)</span><span class="sxs-lookup"><span data-stu-id="839ee-116">Linux (Ubuntu)</span></span>
- <span data-ttu-id="839ee-117">Töltse le a [PHP 7.1.4 non-thread safe (NTS) x64-es verzióját](http://php.net/downloads.php)</span><span class="sxs-lookup"><span data-stu-id="839ee-117">Download [PHP 7.1.4 non-thread safe (x64) version](http://php.net/downloads.php)</span></span>
- <span data-ttu-id="839ee-118">A PHP telepítése, majd tekintse át a toohello [PHP manuális](http://php.net/manual/install.unix.php) további konfiguráció</span><span class="sxs-lookup"><span data-stu-id="839ee-118">Install PHP and refer toohello [PHP manual](http://php.net/manual/install.unix.php) for further configuration</span></span>

### <a name="windows"></a><span data-ttu-id="839ee-119">Windows</span><span class="sxs-lookup"><span data-stu-id="839ee-119">Windows</span></span>
- <span data-ttu-id="839ee-120">Töltse le a [PHP 7.1.4 non-thread safe (NTS) x64-es verzióját](http://windows.php.net/download#php-7.1)</span><span class="sxs-lookup"><span data-stu-id="839ee-120">Download [PHP 7.1.4 non-thread safe (x64) version](http://windows.php.net/download#php-7.1)</span></span>
- <span data-ttu-id="839ee-121">A PHP telepítése, majd tekintse át a toohello [PHP manuális](http://php.net/manual/install.windows.php) további konfiguráció</span><span class="sxs-lookup"><span data-stu-id="839ee-121">Install PHP and refer toohello [PHP manual](http://php.net/manual/install.windows.php) for further configuration</span></span>

## <a name="get-connection-information"></a><span data-ttu-id="839ee-122">Kapcsolatadatok lekérése</span><span class="sxs-lookup"><span data-stu-id="839ee-122">Get connection information</span></span>
<span data-ttu-id="839ee-123">MySQL hello kapcsolat szükséges információkat tooconnect toohello Azure adatbázis beolvasása.</span><span class="sxs-lookup"><span data-stu-id="839ee-123">Get hello connection information needed tooconnect toohello Azure Database for MySQL.</span></span> <span data-ttu-id="839ee-124">Teljesen minősített kiszolgáló nevét és a bejelentkezési hitelesítő adatokat hello van szüksége.</span><span class="sxs-lookup"><span data-stu-id="839ee-124">You need hello fully qualified server name and login credentials.</span></span>

1. <span data-ttu-id="839ee-125">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="839ee-125">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="839ee-126">Hello bal oldali ablaktáblában kattintson **összes erőforrás**, majd keresse meg a létrehozott hello server (például **myserver4demo**).</span><span class="sxs-lookup"><span data-stu-id="839ee-126">In hello left pane, click **All resources**, and then search for hello server you have created (for example, **myserver4demo**).</span></span>
3. <span data-ttu-id="839ee-127">Hello kiszolgáló nevére kattint.</span><span class="sxs-lookup"><span data-stu-id="839ee-127">Click hello server name.</span></span>
4. <span data-ttu-id="839ee-128">Jelölje be hello server **tulajdonságok** lap.</span><span class="sxs-lookup"><span data-stu-id="839ee-128">Select hello server's **Properties** page.</span></span> <span data-ttu-id="839ee-129">Jegyezze fel a hello **kiszolgálónév** és **kiszolgálói rendszergazda bejelentkezési név**.</span><span class="sxs-lookup"><span data-stu-id="839ee-129">Make a note of hello **Server name** and **Server admin login name**.</span></span>
 <span data-ttu-id="839ee-130">![A MySQL-hez készült Azure Database-kiszolgáló neve](./media/connect-php/1_server-properties-name-login.png)</span><span class="sxs-lookup"><span data-stu-id="839ee-130">![Azure Database for MySQL server name](./media/connect-php/1_server-properties-name-login.png)</span></span>
5. <span data-ttu-id="839ee-131">Ha elfelejti a kiszolgálói bejelentkezési adatok, keresse meg a toohello **áttekintése** tooview hello kiszolgálói rendszergazda bejelentkezési név lapon, és ha szükséges, állítsa vissza a hello jelszót.</span><span class="sxs-lookup"><span data-stu-id="839ee-131">If you forget your server login information, navigate toohello **Overview** page tooview hello Server admin login name and, if necessary, reset hello password.</span></span>

## <a name="connect-and-create-a-table"></a><span data-ttu-id="839ee-132">Csatlakozás és tábla létrehozása</span><span class="sxs-lookup"><span data-stu-id="839ee-132">Connect and create a table</span></span>
<span data-ttu-id="839ee-133">Használjon hello alábbi code tooconnect, és hozzon létre egy táblát az **CREATE TABLE** SQL-utasításban.</span><span class="sxs-lookup"><span data-stu-id="839ee-133">Use hello following code tooconnect and create a table using **CREATE TABLE** SQL statement.</span></span> 

<span data-ttu-id="839ee-134">hello használ a hello **MySQL továbbfejlesztett bővítmény** (mysqli) osztály PHP szerepel.</span><span class="sxs-lookup"><span data-stu-id="839ee-134">hello code uses hello **MySQL Improved extension** (mysqli) class included in PHP.</span></span> <span data-ttu-id="839ee-135">kód hívása módszerek hello [mysqli_init](http://php.net/manual/mysqli.init.php) és [mysqli_real_connect](http://php.net/manual/mysqli.real-connect.php) tooconnect tooMySQL.</span><span class="sxs-lookup"><span data-stu-id="839ee-135">hello code call methods [mysqli_init](http://php.net/manual/mysqli.init.php) and [mysqli_real_connect](http://php.net/manual/mysqli.real-connect.php) tooconnect tooMySQL.</span></span> <span data-ttu-id="839ee-136">Ezután a metódus meghívja [mysqli_query](http://php.net/manual/mysqli.query.php) toorun hello lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="839ee-136">Then it calls method [mysqli_query](http://php.net/manual/mysqli.query.php) toorun hello query.</span></span> <span data-ttu-id="839ee-137">Ezután a metódus meghívja [mysqli_close](http://php.net/manual/mysqli.close.php) tooclose hello kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="839ee-137">Then it calls method [mysqli_close](http://php.net/manual/mysqli.close.php) tooclose hello connection.</span></span>

<span data-ttu-id="839ee-138">Hello gazdagép, a felhasználónevet, jelszót és db_name paraméterek cserélje le a saját értékeit.</span><span class="sxs-lookup"><span data-stu-id="839ee-138">Replace hello host, username, password, and db_name parameters with your own values.</span></span> 

```php
<?php
$host = 'myserver4demo.mysql.database.azure.com';
$username = 'myadmin@myserver4demo';
$password = 'your_password';
$db_name = 'your_database';

//Establishes hello connection
$conn = mysqli_init();
mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306);
if (mysqli_connect_errno($conn)) {
die('Failed tooconnect tooMySQL: '.mysqli_connect_error());
}

// Run hello create table query
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

//Close hello connection
mysqli_close($conn);
?>
```

## <a name="insert-data"></a><span data-ttu-id="839ee-139">Adat beszúrása</span><span class="sxs-lookup"><span data-stu-id="839ee-139">Insert data</span></span>
<span data-ttu-id="839ee-140">Használjon hello alábbi code tooconnect, és adatokat beszúrni egy **BESZÚRÁSA** SQL-utasításban.</span><span class="sxs-lookup"><span data-stu-id="839ee-140">Use hello following code tooconnect and insert data using an **INSERT** SQL statement.</span></span>

<span data-ttu-id="839ee-141">hello használ a hello **MySQL továbbfejlesztett bővítmény** (mysqli) osztály PHP szerepel.</span><span class="sxs-lookup"><span data-stu-id="839ee-141">hello code uses hello **MySQL Improved extension** (mysqli) class included in PHP.</span></span> <span data-ttu-id="839ee-142">hello kód módszerrel [mysqli_prepare](http://php.net/manual/mysqli.prepare.php) toocreate egy előkészített utasítás, majd kötések hello minden beszúrt oszlopérték metódussal paraméterek [mysqli_stmt_bind_param](http://php.net/manual/mysqli-stmt.bind-param.php).</span><span class="sxs-lookup"><span data-stu-id="839ee-142">hello code uses method [mysqli_prepare](http://php.net/manual/mysqli.prepare.php) toocreate a prepared insert statement, then binds hello parameters for each inserted column value using method [mysqli_stmt_bind_param](http://php.net/manual/mysqli-stmt.bind-param.php).</span></span> <span data-ttu-id="839ee-143">hello kódjának futtatásához metódussal hello utasítás [mysqli_stmt_execute](http://php.net/manual/mysqli-stmt.execute.php) és ezek után bezárul hello metódussal utasítás [mysqli_stmt_close](http://php.net/manual/mysqli-stmt.close.php).</span><span class="sxs-lookup"><span data-stu-id="839ee-143">hello code runs hello statement using method [mysqli_stmt_execute](http://php.net/manual/mysqli-stmt.execute.php) and afterwards closes hello statement using method [mysqli_stmt_close](http://php.net/manual/mysqli-stmt.close.php).</span></span>

<span data-ttu-id="839ee-144">Hello gazdagép, a felhasználónevet, jelszót és db_name paraméterek cserélje le a saját értékeit.</span><span class="sxs-lookup"><span data-stu-id="839ee-144">Replace hello host, username, password, and db_name parameters with your own values.</span></span> 

```php
<?php
$host = 'myserver4demo.mysql.database.azure.com';
$username = 'myadmin@myserver4demo';
$password = 'your_password';
$db_name = 'your_database';

//Establishes hello connection
$conn = mysqli_init();
mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306);
if (mysqli_connect_errno($conn)) {
die('Failed tooconnect tooMySQL: '.mysqli_connect_error());
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

// Close hello connection
mysqli_close($conn);
?>
```

## <a name="read-data"></a><span data-ttu-id="839ee-145">Adatok olvasása</span><span class="sxs-lookup"><span data-stu-id="839ee-145">Read data</span></span>
<span data-ttu-id="839ee-146">Használjon hello alábbi code tooconnect, és hello adatok segítségével olvassa a **kiválasztása** SQL-utasításban.</span><span class="sxs-lookup"><span data-stu-id="839ee-146">Use hello following code tooconnect and read hello data using a **SELECT** SQL statement.</span></span>  <span data-ttu-id="839ee-147">hello használ a hello **MySQL továbbfejlesztett bővítmény** (mysqli) osztály PHP szerepel.</span><span class="sxs-lookup"><span data-stu-id="839ee-147">hello code uses hello **MySQL Improved extension** (mysqli) class included in PHP.</span></span> <span data-ttu-id="839ee-148">hello kód módszerrel [mysqli_query](http://php.net/manual/mysqli.query.php) hello sql-lekérdezés, illetve a felhasználási [mysqli_fetch_assoc](http://php.net/manual/mysqli-result.fetch-assoc.php) metódus toofetch hello utasítás.</span><span class="sxs-lookup"><span data-stu-id="839ee-148">hello code uses method [mysqli_query](http://php.net/manual/mysqli.query.php) perform hello sql query, and uses [mysqli_fetch_assoc](http://php.net/manual/mysqli-result.fetch-assoc.php) method toofetch hello resulting rows.</span></span>

<span data-ttu-id="839ee-149">Hello gazdagép, a felhasználónevet, jelszót és db_name paraméterek cserélje le a saját értékeit.</span><span class="sxs-lookup"><span data-stu-id="839ee-149">Replace hello host, username, password, and db_name parameters with your own values.</span></span> 

```php
<?php
$host = 'myserver4demo.mysql.database.azure.com';
$username = 'myadmin@myserver4demo';
$password = 'your_password';
$db_name = 'your_database';

//Establishes hello connection
$conn = mysqli_init();
mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306);
if (mysqli_connect_errno($conn)) {
die('Failed tooconnect tooMySQL: '.mysqli_connect_error());
}

//Run hello Select query
printf("Reading data from table: \n");
$res = mysqli_query($conn, 'SELECT * FROM Products');
while ($row = mysqli_fetch_assoc($res)) {
var_dump($row);
}

//Close hello connection
mysqli_close($conn);
?>
```

## <a name="update-data"></a><span data-ttu-id="839ee-150">Adatok frissítése</span><span class="sxs-lookup"><span data-stu-id="839ee-150">Update data</span></span>
<span data-ttu-id="839ee-151">Használjon hello következő code tooconnect, és frissítse a hello adatok segítségével egy **frissítése** SQL-utasításban.</span><span class="sxs-lookup"><span data-stu-id="839ee-151">Use hello following code tooconnect and update hello data using a **UPDATE** SQL statement.</span></span>

<span data-ttu-id="839ee-152">hello használ a hello **MySQL továbbfejlesztett bővítmény** (mysqli) osztály PHP szerepel.</span><span class="sxs-lookup"><span data-stu-id="839ee-152">hello code uses hello **MySQL Improved extension** (mysqli) class included in PHP.</span></span> <span data-ttu-id="839ee-153">hello kód módszerrel [mysqli_prepare](http://php.net/manual/mysqli.prepare.php) toocreate egy előkészített update utasítás, majd ennek minden frissített oszlopérték metódussal hello paramétereinek [mysqli_stmt_bind_param](http://php.net/manual/mysqli-stmt.bind-param.php).</span><span class="sxs-lookup"><span data-stu-id="839ee-153">hello code uses method [mysqli_prepare](http://php.net/manual/mysqli.prepare.php) toocreate a prepared update statement, then binds hello parameters for each updated column value using method [mysqli_stmt_bind_param](http://php.net/manual/mysqli-stmt.bind-param.php).</span></span> <span data-ttu-id="839ee-154">hello kódjának futtatásához metódussal hello utasítás [mysqli_stmt_execute](http://php.net/manual/mysqli-stmt.execute.php) és ezek után bezárul hello metódussal utasítás [mysqli_stmt_close](http://php.net/manual/mysqli-stmt.close.php).</span><span class="sxs-lookup"><span data-stu-id="839ee-154">hello code runs hello statement using method [mysqli_stmt_execute](http://php.net/manual/mysqli-stmt.execute.php) and afterwards closes hello statement using method [mysqli_stmt_close](http://php.net/manual/mysqli-stmt.close.php).</span></span>

<span data-ttu-id="839ee-155">Hello gazdagép, a felhasználónevet, jelszót és db_name paraméterek cserélje le a saját értékeit.</span><span class="sxs-lookup"><span data-stu-id="839ee-155">Replace hello host, username, password, and db_name parameters with your own values.</span></span> 

```php
<?php
$host = 'myserver4demo.mysql.database.azure.com';
$username = 'myadmin@myserver4demo';
$password = 'your_password';
$db_name = 'your_database';

//Establishes hello connection
$conn = mysqli_init();
mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306);
if (mysqli_connect_errno($conn)) {
die('Failed tooconnect tooMySQL: '.mysqli_connect_error());
}

//Run hello Update statement
$product_name = 'BrandNewProduct';
$new_product_price = 15.1;
if ($stmt = mysqli_prepare($conn, "UPDATE Products SET Price = ? WHERE ProductName = ?")) {
mysqli_stmt_bind_param($stmt, 'ds', $new_product_price, $product_name);
mysqli_stmt_execute($stmt);
printf("Update: Affected %d rows\n", mysqli_stmt_affected_rows($stmt));

//Close hello connection
mysqli_stmt_close($stmt);
}

mysqli_close($conn);
?>
```


## <a name="delete-data"></a><span data-ttu-id="839ee-156">Adat törlése</span><span class="sxs-lookup"><span data-stu-id="839ee-156">Delete data</span></span>
<span data-ttu-id="839ee-157">Használjon hello alábbi code tooconnect, és olvasott hello adatok egy **törlése** SQL-utasításban.</span><span class="sxs-lookup"><span data-stu-id="839ee-157">Use hello following code tooconnect and read hello data using a **DELETE** SQL statement.</span></span> 

<span data-ttu-id="839ee-158">hello használ a hello **MySQL továbbfejlesztett bővítmény** (mysqli) osztály PHP szerepel.</span><span class="sxs-lookup"><span data-stu-id="839ee-158">hello code uses hello **MySQL Improved extension** (mysqli) class included in PHP.</span></span> <span data-ttu-id="839ee-159">hello kód módszerrel [mysqli_prepare](http://php.net/manual/mysqli.prepare.php) egy előkészített toocreate törlési utasításnak, majd kötések hello hello paramétereinek ahol metódussal hello utasításban szereplő [mysqli_stmt_bind_param](http://php.net/manual/mysqli-stmt.bind-param.php).</span><span class="sxs-lookup"><span data-stu-id="839ee-159">hello code uses method [mysqli_prepare](http://php.net/manual/mysqli.prepare.php) toocreate a prepared delete statement, then binds hello parameters for hello where clause in hello statement using method [mysqli_stmt_bind_param](http://php.net/manual/mysqli-stmt.bind-param.php).</span></span> <span data-ttu-id="839ee-160">hello kódjának futtatásához metódussal hello utasítás [mysqli_stmt_execute](http://php.net/manual/mysqli-stmt.execute.php) és ezek után bezárul hello metódussal utasítás [mysqli_stmt_close](http://php.net/manual/mysqli-stmt.close.php).</span><span class="sxs-lookup"><span data-stu-id="839ee-160">hello code runs hello statement using method [mysqli_stmt_execute](http://php.net/manual/mysqli-stmt.execute.php) and afterwards closes hello statement using method [mysqli_stmt_close](http://php.net/manual/mysqli-stmt.close.php).</span></span>

<span data-ttu-id="839ee-161">Hello gazdagép, a felhasználónevet, jelszót és db_name paraméterek cserélje le a saját értékeit.</span><span class="sxs-lookup"><span data-stu-id="839ee-161">Replace hello host, username, password, and db_name parameters with your own values.</span></span> 

```php
<?php
$host = 'myserver4demo.mysql.database.azure.com';
$username = 'myadmin@myserver4demo';
$password = 'your_password';
$db_name = 'your_database';

//Establishes hello connection
$conn = mysqli_init();
mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306);
if (mysqli_connect_errno($conn)) {
die('Failed tooconnect tooMySQL: '.mysqli_connect_error());
}

//Run hello Delete statement
$product_name = 'BrandNewProduct';
if ($stmt = mysqli_prepare($conn, "DELETE FROM Products WHERE ProductName = ?")) {
mysqli_stmt_bind_param($stmt, 's', $product_name);
mysqli_stmt_execute($stmt);
printf("Delete: Affected %d rows\n", mysqli_stmt_affected_rows($stmt));
mysqli_stmt_close($stmt);
}

//Close hello connection
mysqli_close($conn);
?>
```

## <a name="next-steps"></a><span data-ttu-id="839ee-162">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="839ee-162">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="839ee-163">PHP- és MySQL-webalkalmazás létrehozása az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="839ee-163">Build a PHP and MySQL web app in Azure</span></span>](../app-service-web/app-service-web-tutorial-php-mysql.md?toc=%2fazure%2fmysql%2ftoc.json)
