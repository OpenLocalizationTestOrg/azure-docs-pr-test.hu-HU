---
title: "Csatlakozás a MySQL az Azure Database-alkalmazások |} Microsoft Docs"
description: "Ez a dokumentum a jelenleg támogatott kapcsolati karakterláncok az alkalmazások az Azure Database-MySQL, beleértve az ADO.NET (C#), JDBC, Node.js, ODBC, PHP, Python vagy Ruby kapcsolati sorolja fel."
services: mysql
author: mswutao
ms.author: wuta
editor: jasonwhowell
manager: jhubbard
ms.service: mysql-database
ms.topic: article
ms.date: 06/12/2017
ms.openlocfilehash: 2f40da41bcfda7e35f6fc63ead5d055246ab390c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-connect-applications-to-azure-database-for-mysql"></a><span data-ttu-id="bc488-103">A MySQL az Azure Database-alkalmazások összekapcsolása</span><span class="sxs-lookup"><span data-stu-id="bc488-103">How to connect applications to Azure Database for MySQL</span></span>
<span data-ttu-id="bc488-104">Ez a dokumentum által támogatott Azure-adatbázis a MySQL, sablonok és példákkal együtt karakterláncot kapcsolattípusokat sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="bc488-104">This document lists the connection string types that are supported by Azure Database for MySQL, together with templates and examples.</span></span> <span data-ttu-id="bc488-105">Lehetséges, hogy más paraméterekkel, és különböző beállításokat a kapcsolati karakterláncban.</span><span class="sxs-lookup"><span data-stu-id="bc488-105">You might have different parameters and different settings in your connection string.</span></span>

- <span data-ttu-id="bc488-106">A tanúsítvány beszerzéséről [SSL konfigurálása](./howto-configure-ssl.md).</span><span class="sxs-lookup"><span data-stu-id="bc488-106">To obtain the certificate, see [How to configure SSL](./howto-configure-ssl.md).</span></span>
- <span data-ttu-id="bc488-107">{your_host} = <servername>. mysql.database.azure.com</span><span class="sxs-lookup"><span data-stu-id="bc488-107">{your_host} = <servername>.mysql.database.azure.com</span></span>
- <span data-ttu-id="bc488-108">{your_user}@{servername} megfelelően = userID formátum a hitelesítéshez.</span><span class="sxs-lookup"><span data-stu-id="bc488-108">{your_user}@{servername} = userID format for authentication correctly.</span></span>  <span data-ttu-id="bc488-109">Csak a felhasználói azonosítóját használja, akkor a hitelesítés sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="bc488-109">Using just the userID will cause the authentication to fail.</span></span>

## <a name="adonet"></a><span data-ttu-id="bc488-110">ADO.NET</span><span class="sxs-lookup"><span data-stu-id="bc488-110">ADO.NET</span></span>
```ado.net
Server={your_host};Port={your_port};Database={your_database};Uid={username@servername};Pwd={your_password};[SslMode=Required;]
```

<span data-ttu-id="bc488-111">Ebben a példában a kiszolgáló neve, `myserver4demo`, adatbázisnév `wpdb`, a felhasználó neve `WPAdmin`, és a jelszó `mypassword!2`.</span><span class="sxs-lookup"><span data-stu-id="bc488-111">In this example, the server name is `myserver4demo`, database name is `wpdb`, user name is `WPAdmin`, and password is `mypassword!2`.</span></span> <span data-ttu-id="bc488-112">Ezt követően a kapcsolati karakterláncot kell lennie:</span><span class="sxs-lookup"><span data-stu-id="bc488-112">Then, the connection string should be:</span></span>

```ado.net
Server= "myserver4demo.mysql.database.azure.com"; Port=3306; Database= "wpdb"; Uid= "WPAdmin@myserver4demo"; Pwd="mypassword!2"; SslMode=Required;
```

## <a name="jdbc"></a><span data-ttu-id="bc488-113">JDBC</span><span class="sxs-lookup"><span data-stu-id="bc488-113">JDBC</span></span>
```jdbc
String url ="jdbc:mysql://%s:%s/%s[?verifyServerCertificate=true&useSSL=true&requireSSL=true]",{your_host},{your_port},{your_database}"; myDbConn = DriverManager.getConnection(url, {username@servername}, {your_password}";
```

## <a name="nodejs"></a><span data-ttu-id="bc488-114">Node.js</span><span class="sxs-lookup"><span data-stu-id="bc488-114">Node.js</span></span>
```node.js
var conn = mysql.createConnection({host: {your_host}, user: {username@servername}, password: {your_password}, database: {your_database}, Port: {your_port}[, ssl:{ca:fs.readFileSync({ca-cert filename})}}]);
```

## <a name="odbc"></a><span data-ttu-id="bc488-115">ODBC</span><span class="sxs-lookup"><span data-stu-id="bc488-115">ODBC</span></span>
```odbc
DRIVER={MySQL ODBC 5.3 UNICODE Driver};Server={your_host};Port={your_port};Database={your_database};Uid={username@servername};Pwd={your_password}; [sslca={ca-cert filename}; sslverify=1; Option=3;]
```

## <a name="php"></a><span data-ttu-id="bc488-116">PHP</span><span class="sxs-lookup"><span data-stu-id="bc488-116">PHP</span></span>
```php
$con=mysqli_init(); [mysqli_ssl_set($con, NULL, NULL, {ca-cert filename}, NULL, NULL);] mysqli_real_connect($con, {your_host}, {username@servername}, {your_password}, {your_database}, {your_port});
```

## <a name="python"></a><span data-ttu-id="bc488-117">Python</span><span class="sxs-lookup"><span data-stu-id="bc488-117">Python</span></span>
```python
cnx = mysql.connector.connect(user={username@servername}, password={your_password}, host={your_host}, port={your_port}, database={your_database}[, ssl_ca={ca-cert filename}, ssl_verify_cert=true])
```

## <a name="ruby"></a><span data-ttu-id="bc488-118">Ruby</span><span class="sxs-lookup"><span data-stu-id="bc488-118">Ruby</span></span>
```ruby
client = Mysql2::Client.new(username: {username@servername}, password: {your_password}, database: {your_database}, host: {your_host}, port: {your_port}[, sslca:{ca-cert filename}, sslverify:false, sslcipher:'AES256-SHA'])
```

## <a name="get-the-connection-string-details-from-the-azure-portal"></a><span data-ttu-id="bc488-119">Ismerje meg a kapcsolati karakterlánc részleteket az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="bc488-119">Get the connection string details from the Azure portal</span></span>
<span data-ttu-id="bc488-120">Az a [Azure-portálon](https://portal.azure.com), nyissa meg a MySQL-kiszolgáló Azure-adatbázishoz, és kattintson a **kapcsolati karakterláncok** a listában beolvasni a példány: ![a kapcsolati karakterláncok ablaktáblán az Azure-portálon](./media/howto-connection-strings/connection-strings-on-portal.png)</span><span class="sxs-lookup"><span data-stu-id="bc488-120">In the [Azure portal](https://portal.azure.com), go to your Azure Database for MySQL server, and then click **Connection strings** to get your string list for your instance: ![The Connection strings pane in the Azure portal](./media/howto-connection-strings/connection-strings-on-portal.png)</span></span>

<span data-ttu-id="bc488-121">A karakterlánc részletesen bemutatja a például az illesztőprogram, a kiszolgáló és a más adatbázis kapcsolat paramétereit.</span><span class="sxs-lookup"><span data-stu-id="bc488-121">The string provides details such as the driver, server, and other database connection parameters.</span></span> <span data-ttu-id="bc488-122">Módosítsa a példákat a saját paramétereivel, például az adatbázis nevét, jelszó és így tovább.</span><span class="sxs-lookup"><span data-stu-id="bc488-122">Modify these examples by using your own parameters, such as database name, password, and so on.</span></span> <span data-ttu-id="bc488-123">Ez a karakterlánc segítségével majd csatlakozni a kiszolgálóhoz, a kódot és az alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="bc488-123">You can then use this string to connect to the server from your code and applications.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bc488-124">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bc488-124">Next steps</span></span>
- <span data-ttu-id="bc488-125">Kapcsolat szalagtárakkal kapcsolatos további információkért lásd: [fogalmak - adatkapcsolattárak](./concepts-connection-libraries.md).</span><span class="sxs-lookup"><span data-stu-id="bc488-125">For more information about connection libraries, see [Concepts - Connection libraries](./concepts-connection-libraries.md).</span></span>
