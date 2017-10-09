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
# <a name="azure-database-for-mysql-use-php-tooconnect-and-query-data"></a>MySQL az Azure-adatbázishoz: használja a PHP tooconnect és lekérdezési adatok
A gyors üzembe helyezés bemutatja, hogyan tooconnect tooan Azure adatbázis a MySQL használata egy [PHP](http://php.net/manual/intro-whatis.php) alkalmazás. Azt illusztrálja, hogyan toouse SQL utasítás tooquery beszúrási, frissítési és törlési hello adatbázis adatait. Ez a cikk feltételezi, hogy jártas használata PHP-fejlesztési azonban, hogy új tooworking MySQL az Azure-adatbázissal.

## <a name="prerequisites"></a>Előfeltételek
A gyors üzembe helyezés kiindulási pontként ezek az útmutatók valamelyikével létrehozott hello erőforrást használ:
- [Azure-adatbázis létrehozása MySQL-kiszolgálóhoz az Azure Portal használatával](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [Azure-adatbázis létrehozása MySQL-kiszolgálóhoz az Azure CLI használatával](./quickstart-create-mysql-server-database-using-azure-cli.md)

## <a name="install-php"></a>A PHP telepítése
Telepítse a PHP-t saját kiszolgálón, vagy hozzon létre egy, a PHP-t tartalmazó Azure-[webalkalmazást](https://docs.microsoft.com/en-us/azure/app-service-web/app-service-web-overview).

### <a name="macos"></a>MacOS
- Töltse le a [PHP 7.1.4-es verzióját](http://php.net/downloads.php)
- A PHP telepítése, majd tekintse át a toohello [PHP manuális](http://php.net/manual/install.macosx.php) további konfiguráció

### <a name="linux-ubuntu"></a>Linux (Ubuntu)
- Töltse le a [PHP 7.1.4 non-thread safe (NTS) x64-es verzióját](http://php.net/downloads.php)
- A PHP telepítése, majd tekintse át a toohello [PHP manuális](http://php.net/manual/install.unix.php) további konfiguráció

### <a name="windows"></a>Windows
- Töltse le a [PHP 7.1.4 non-thread safe (NTS) x64-es verzióját](http://windows.php.net/download#php-7.1)
- A PHP telepítése, majd tekintse át a toohello [PHP manuális](http://php.net/manual/install.windows.php) további konfiguráció

## <a name="get-connection-information"></a>Kapcsolatadatok lekérése
MySQL hello kapcsolat szükséges információkat tooconnect toohello Azure adatbázis beolvasása. Teljesen minősített kiszolgáló nevét és a bejelentkezési hitelesítő adatokat hello van szüksége.

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).
2. Hello bal oldali ablaktáblában kattintson **összes erőforrás**, majd keresse meg a létrehozott hello server (például **myserver4demo**).
3. Hello kiszolgáló nevére kattint.
4. Jelölje be hello server **tulajdonságok** lap. Jegyezze fel a hello **kiszolgálónév** és **kiszolgálói rendszergazda bejelentkezési név**.
 ![A MySQL-hez készült Azure Database-kiszolgáló neve](./media/connect-php/1_server-properties-name-login.png)
5. Ha elfelejti a kiszolgálói bejelentkezési adatok, keresse meg a toohello **áttekintése** tooview hello kiszolgálói rendszergazda bejelentkezési név lapon, és ha szükséges, állítsa vissza a hello jelszót.

## <a name="connect-and-create-a-table"></a>Csatlakozás és tábla létrehozása
Használjon hello alábbi code tooconnect, és hozzon létre egy táblát az **CREATE TABLE** SQL-utasításban. 

hello használ a hello **MySQL továbbfejlesztett bővítmény** (mysqli) osztály PHP szerepel. kód hívása módszerek hello [mysqli_init](http://php.net/manual/mysqli.init.php) és [mysqli_real_connect](http://php.net/manual/mysqli.real-connect.php) tooconnect tooMySQL. Ezután a metódus meghívja [mysqli_query](http://php.net/manual/mysqli.query.php) toorun hello lekérdezés. Ezután a metódus meghívja [mysqli_close](http://php.net/manual/mysqli.close.php) tooclose hello kapcsolat.

Hello gazdagép, a felhasználónevet, jelszót és db_name paraméterek cserélje le a saját értékeit. 

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

## <a name="insert-data"></a>Adat beszúrása
Használjon hello alábbi code tooconnect, és adatokat beszúrni egy **BESZÚRÁSA** SQL-utasításban.

hello használ a hello **MySQL továbbfejlesztett bővítmény** (mysqli) osztály PHP szerepel. hello kód módszerrel [mysqli_prepare](http://php.net/manual/mysqli.prepare.php) toocreate egy előkészített utasítás, majd kötések hello minden beszúrt oszlopérték metódussal paraméterek [mysqli_stmt_bind_param](http://php.net/manual/mysqli-stmt.bind-param.php). hello kódjának futtatásához metódussal hello utasítás [mysqli_stmt_execute](http://php.net/manual/mysqli-stmt.execute.php) és ezek után bezárul hello metódussal utasítás [mysqli_stmt_close](http://php.net/manual/mysqli-stmt.close.php).

Hello gazdagép, a felhasználónevet, jelszót és db_name paraméterek cserélje le a saját értékeit. 

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

## <a name="read-data"></a>Adatok olvasása
Használjon hello alábbi code tooconnect, és hello adatok segítségével olvassa a **kiválasztása** SQL-utasításban.  hello használ a hello **MySQL továbbfejlesztett bővítmény** (mysqli) osztály PHP szerepel. hello kód módszerrel [mysqli_query](http://php.net/manual/mysqli.query.php) hello sql-lekérdezés, illetve a felhasználási [mysqli_fetch_assoc](http://php.net/manual/mysqli-result.fetch-assoc.php) metódus toofetch hello utasítás.

Hello gazdagép, a felhasználónevet, jelszót és db_name paraméterek cserélje le a saját értékeit. 

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

## <a name="update-data"></a>Adatok frissítése
Használjon hello következő code tooconnect, és frissítse a hello adatok segítségével egy **frissítése** SQL-utasításban.

hello használ a hello **MySQL továbbfejlesztett bővítmény** (mysqli) osztály PHP szerepel. hello kód módszerrel [mysqli_prepare](http://php.net/manual/mysqli.prepare.php) toocreate egy előkészített update utasítás, majd ennek minden frissített oszlopérték metódussal hello paramétereinek [mysqli_stmt_bind_param](http://php.net/manual/mysqli-stmt.bind-param.php). hello kódjának futtatásához metódussal hello utasítás [mysqli_stmt_execute](http://php.net/manual/mysqli-stmt.execute.php) és ezek után bezárul hello metódussal utasítás [mysqli_stmt_close](http://php.net/manual/mysqli-stmt.close.php).

Hello gazdagép, a felhasználónevet, jelszót és db_name paraméterek cserélje le a saját értékeit. 

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


## <a name="delete-data"></a>Adat törlése
Használjon hello alábbi code tooconnect, és olvasott hello adatok egy **törlése** SQL-utasításban. 

hello használ a hello **MySQL továbbfejlesztett bővítmény** (mysqli) osztály PHP szerepel. hello kód módszerrel [mysqli_prepare](http://php.net/manual/mysqli.prepare.php) egy előkészített toocreate törlési utasításnak, majd kötések hello hello paramétereinek ahol metódussal hello utasításban szereplő [mysqli_stmt_bind_param](http://php.net/manual/mysqli-stmt.bind-param.php). hello kódjának futtatásához metódussal hello utasítás [mysqli_stmt_execute](http://php.net/manual/mysqli-stmt.execute.php) és ezek után bezárul hello metódussal utasítás [mysqli_stmt_close](http://php.net/manual/mysqli-stmt.close.php).

Hello gazdagép, a felhasználónevet, jelszót és db_name paraméterek cserélje le a saját értékeit. 

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

## <a name="next-steps"></a>Következő lépések
> [!div class="nextstepaction"]
> [PHP- és MySQL-webalkalmazás létrehozása az Azure-ban](../app-service-web/app-service-web-tutorial-php-mysql.md?toc=%2fazure%2fmysql%2ftoc.json)
