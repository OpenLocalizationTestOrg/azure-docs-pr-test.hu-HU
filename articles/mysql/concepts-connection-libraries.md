---
title: "MySQL az Azure-adatbázis adatkapcsolattárak |} Microsoft Docs"
description: "Ez a cikk sorolja fel, minden könyvtár vagy illesztőprogram, ügyfélprogramok MySQL az Azure-adatbázishoz való csatlakozáshoz használhat."
services: mysql
author: mswutao
ms.author: wutao
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 10/20/2017
ms.openlocfilehash: 759fa290cff94b04e29edd818b985b11267caab7
ms.sourcegitcommit: 4ed3fe11c138eeed19aef0315a4f470f447eac0c
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/23/2017
---
# <a name="connection-libraries-for-azure-database-for-mysql"></a>Adatkapcsolattárak MySQL az Azure-adatbázis
Ez a cikk sorolja fel, minden könyvtár vagy illesztőprogram, ügyfélprogramok MySQL az Azure-adatbázishoz való csatlakozáshoz használhat.

## <a name="client-interfaces"></a>Ügyfél felületek
MySQL adatbázis standard illesztőprogram kapcsolódási MySQL ipari szabványok ODBC és JDBC kompatibilis eszközök és az alkalmazások használatára vonatkozó lehetőséget. A rendszer, amely ODBC vagy JDBC MySQL használhatja.

| **Nyelv** | **Platform** | **További erőforrások** | **Letöltés** |
| :----------- | :------------| :-----------------------| :------------|
| PHP | Windows, Linux | [A PHP - mysqlnd natív MySQL-illesztőprogram](https://dev.mysql.com/downloads/connector/php-mysqlnd/) | [Letöltés](http://php.net/downloads.php) |
| ODBC | Windows, Linux, Mac OS X és Unix rendszereken | [MySQL-összekötő/ODBC fejlesztői útmutató](https://dev.mysql.com/doc/connector-odbc/en/) | [Letöltés](https://dev.mysql.com/downloads/connector/odbc/) |
| ADO.NET | Windows | [MySQL összekötő/Net fejlesztői útmutató](https://dev.mysql.com/doc/connector-net/en/) | [Letöltés](https://dev.mysql.com/downloads/connector/net/) |
| JDBC | Független platform | [MySQL-összekötő/J 5.1 – útmutató fejlesztőknek](https://dev.mysql.com/doc/connector-j/5.1/en/) | [Letöltés](https://dev.mysql.com/downloads/connector/j/) |
| Node.js | Windows, Linux, Mac OS X | [sidorares/csomópont-mysql2](https://github.com/sidorares/node-mysql2/tree/master/documentation) | [Letöltés](https://github.com/sidorares/node-mysql2) |
| Python | Windows, Linux, Mac OS X | [MySQL összekötő/Python fejlesztői útmutató](https://dev.mysql.com/doc/connector-python/en/) | [Letöltés](https://dev.mysql.com/downloads/connector/python/) |
| C++ | Windows, Linux, Mac OS X | [MySQL összekötő/C++ – útmutató fejlesztőknek](https://dev.mysql.com/doc/connector-cpp/en/) | [Letöltés](https://dev.mysql.com/downloads/connector/python/) |
| C# | Windows, Linux, Mac OS X | [MySQL összekötő/C – útmutató fejlesztőknek](https://dev.mysql.com/doc/connector-c/en/) | [Letöltés](https://dev.mysql.com/downloads/connector/c/)
| Perl | Windows, Linux, Mac OS X és Unix rendszereken | [DBD::MySQL](https://metacpan.org/pod/DBD::mysql) | [Letöltés](https://metacpan.org/pod/DBD::mysql) |


## <a name="next-steps"></a>Következő lépések
Olvassa el ezen quickstarts csatlakozni, és az Azure-adatbázis lekérdezése a MySQL a választott nyelv használatával:

[A PHP](./connect-php.md) | [Java](./connect-java.md) |  [.NET (C#)](./connect-csharp.md) | [Python](./connect-python.md) | [Node.JS](./connect-nodejs.md)  |  [Ruby](./connect-ruby.md) | [C++](connect-cpp.md) | [nyissa meg](./connect-go.md)

