---
title: "aaaConnect alkalmazások tooAzure MySQL adatbázis |} Microsoft Docs"
description: "Ez a dokumentum az Azure Database alkalmazások tooconnect jelenleg támogatott hello kapcsolati karakterláncok MySQL, beleértve az ADO.NET (C#), JDBC, Node.js, ODBC, PHP, Python vagy Ruby sorolja fel."
services: mysql
author: mswutao
ms.author: wuta
editor: jasonwhowell
manager: jhubbard
ms.service: mysql-database
ms.topic: article
ms.date: 06/12/2017
ms.openlocfilehash: bbcb2c0ddb4f8e5c225ebef53781e073f5c7b1a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconnect-applications-tooazure-database-for-mysql"></a>Hogyan tooconnect alkalmazások tooAzure MySQL adatbázist
Ez a dokumentum hello karakterláncot kapcsolattípusokat által támogatott Azure-adatbázis a MySQL, sablonok és példákkal együtt sorolja fel. Lehetséges, hogy más paraméterekkel, és különböző beállításokat a kapcsolati karakterláncban.

- tooobtain hello tanúsítvány, lásd: [hogyan tooconfigure SSL](./howto-configure-ssl.md).
- {your_host} = <servername>. mysql.database.azure.com
- {your_user}@{servername} megfelelően = userID formátum a hitelesítéshez.  Csak hello userID használ, akkor a hello hitelesítési toofail.

## <a name="adonet"></a>ADO.NET
```ado.net
Server={your_host};Port={your_port};Database={your_database};Uid={username@servername};Pwd={your_password};[SslMode=Required;]
```

Ebben a példában hello kiszolgáló neve, `myserver4demo`, adatbázisnév `wpdb`, a felhasználó neve `WPAdmin`, és a jelszó `mypassword!2`. Ezt követően hello kapcsolati karakterláncot kell lennie:

```ado.net
Server= "myserver4demo.mysql.database.azure.com"; Port=3306; Database= "wpdb"; Uid= "WPAdmin@myserver4demo"; Pwd="mypassword!2"; SslMode=Required;
```

## <a name="jdbc"></a>JDBC
```jdbc
String url ="jdbc:mysql://%s:%s/%s[?verifyServerCertificate=true&useSSL=true&requireSSL=true]",{your_host},{your_port},{your_database}"; myDbConn = DriverManager.getConnection(url, {username@servername}, {your_password}";
```

## <a name="nodejs"></a>Node.js
```node.js
var conn = mysql.createConnection({host: {your_host}, user: {username@servername}, password: {your_password}, database: {your_database}, Port: {your_port}[, ssl:{ca:fs.readFileSync({ca-cert filename})}}]);
```

## <a name="odbc"></a>ODBC
```odbc
DRIVER={MySQL ODBC 5.3 UNICODE Driver};Server={your_host};Port={your_port};Database={your_database};Uid={username@servername};Pwd={your_password}; [sslca={ca-cert filename}; sslverify=1; Option=3;]
```

## <a name="php"></a>PHP
```php
$con=mysqli_init(); [mysqli_ssl_set($con, NULL, NULL, {ca-cert filename}, NULL, NULL);] mysqli_real_connect($con, {your_host}, {username@servername}, {your_password}, {your_database}, {your_port});
```

## <a name="python"></a>Python
```python
cnx = mysql.connector.connect(user={username@servername}, password={your_password}, host={your_host}, port={your_port}, database={your_database}[, ssl_ca={ca-cert filename}, ssl_verify_cert=true])
```

## <a name="ruby"></a>Ruby
```ruby
client = Mysql2::Client.new(username: {username@servername}, password: {your_password}, database: {your_database}, host: {your_host}, port: {your_port}[, sslca:{ca-cert filename}, sslverify:false, sslcipher:'AES256-SHA'])
```

## <a name="get-hello-connection-string-details-from-hello-azure-portal"></a>Ismerje meg hello kapcsolati karakterlánc részleteket hello Azure-portálon
A hello [Azure-portálon](https://portal.azure.com), nyissa meg az Azure Database tooyour MySQL-kiszolgáló, és kattintson a **kapcsolati karakterláncok** tooget a karakterlánc-példány listázása: ![hello kapcsolati karakterláncok ablaktábláján hello Azure-portálon](./media/howto-connection-strings/connection-strings-on-portal.png)

hello karakterlánc például hello illesztőprogram, a kiszolgáló és a más adatbázis paramétereit részletes adatokat biztosít. Módosítsa a példákat a saját paramétereivel, például az adatbázis nevét, jelszó és így tovább. A karakterlánc tooconnect toohello kiszolgáló a kódot és az alkalmazások használhatja.

## <a name="next-steps"></a>Következő lépések
- Kapcsolat szalagtárakkal kapcsolatos további információkért lásd: [fogalmak - adatkapcsolattárak](./concepts-connection-libraries.md).
