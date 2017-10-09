---
title: az SQL Data Warehouse aaaConnect tooAzure |} Microsoft Docs
description: "Hogyan toofind hello kiszolgáló nevét és a kapcsolati karakterlánc-az SQL Data Warehouse tooAzure"
services: sql-data-warehouse
documentationcenter: NA
author: antvgski
manager: jhubbard
editor: 
ms.assetid: e52872ca-ae74-4e25-9c56-d49c85c8d0f0
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: connect
ms.date: 10/31/2016
ms.author: anvang;barbkess
ms.openlocfilehash: f15e098026afb7c5efbbbfaf62b681e8cd7936bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-tooazure-sql-data-warehouse"></a>Csatlakozás az SQL Data Warehouse tooAzure
Ez a cikk segítséget nyújt a csatlakoztatott tooSQL adatraktár lekérése hello első alkalommal.

## <a name="find-your-server-name"></a>A kiszolgálónév lekérdezése
első lépés tooconnecting tooSQL adatraktár ismerete hello hogyan toofind a kiszolgáló nevét.  Például a hello kiszolgáló nevét a következő példa hello sample.database.windows.net. toofind hello teljes kiszolgálónév:

1. Nyissa meg toohello [Azure-portálon][Azure portal].
2. Kattintson az **SQL-adatbázisok** elemre 
3. Kattintson a kívánt való tooconnect hello adatbázisra.
4. Keresse meg a teljes kiszolgálónevet hello.
   
    ![Teljes kiszolgálónév][1]

## <a name="supported-drivers-and-connection-strings"></a>Támogatott illesztők és kapcsolati karakterláncok
Az Azure SQL Data Warehouse a következő illesztőprogramokat támogatja: [ADO.NET][ADO.NET], [ODBC][ODBC], [PHP][PHP] és [JDBC][JDBC]. Kattintson az egyik hello megelőző illesztőprogramok toofind hello legújabb verzióra, és a dokumentációt. tooautomatically hello kapcsolati karakterlánc az Ön által használt hello illesztőprogram generálása hello Azure portál, rákattinthat a hello **adatbázis-kapcsolati karakterláncok megjelenítése** az előző példa hello.  A következő néhány példa bemutatja, hogy néz ki a kapcsolati karakterlánc az egyes illesztők esetében.

> [!NOTE]
> Fontolja meg, hello kapcsolat időtúllépése too300 másodperc tooallow a kapcsolat toosurvive rövid időszakokra, elérhetetlensége beállítását.
> 
> 

### <a name="adonet-connection-string-example"></a>Minta ADO.NET kapcsolati karakterlánc
```C#
Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};User ID={your_user_name};Password={your_password_here};Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;
```

### <a name="odbc-connection-string-example"></a>Minta ODBC kapcsolati karakterlánc
```C#
Driver={SQL Server Native Client 11.0};Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};Uid={your_user_name};Pwd={your_password_here};Encrypt=yes;TrustServerCertificate=no;Connection Timeout=30;
```

### <a name="php-connection-string-example"></a>Minta PHP kapcsolati karakterlánc
```PHP
Server: {your_server}.database.windows.net,1433 \r\nSQL Database: {your_database}\r\nUser Name: {your_user_name}\r\n\r\nPHP Data Objects(PDO) Sample Code:\r\n\r\ntry {\r\n   $conn = new PDO ( \"sqlsrv:server = tcp:{your_server}.database.windows.net,1433; Database = {your_database}\", \"{your_user_name}\", \"{your_password_here}\");\r\n    $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );\r\n}\r\ncatch ( PDOException $e ) {\r\n   print( \"Error connecting tooSQL Server.\" );\r\n   die(print_r($e));\r\n}\r\n\rSQL Server Extension Sample Code:\r\n\r\n$connectionInfo = array(\"UID\" => \"{your_user_name}\", \"pwd\" => \"{your_password_here}\", \"Database\" => \"{your_database}\", \"LoginTimeout\" => 30, \"Encrypt\" => 1, \"TrustServerCertificate\" => 0);\r\n$serverName = \"tcp:{your_server}.database.windows.net,1433\";\r\n$conn = sqlsrv_connect($serverName, $connectionInfo);
```

### <a name="jdbc-connection-string-example"></a>Minta JDBC kapcsolati karakterlánc
```Java
jdbc:sqlserver://yourserver.database.windows.net:1433;database=yourdatabase;user={your_user_name};password={your_password_here};encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;
```

## <a name="connection-settings"></a>Kapcsolati beállítások
Az SQL Data Warehouse szabványosít néhány beállítást a csatlakozás és az objektumlétrehozás során. Ezeket a beállításokat nem lehet felülírni, és a következők lehetnek:

| Adatbázis-beállítások | Érték |
|:--- |:--- |
| [ANSI_NULLS][ANSI_NULLS] |ON |
| [QUOTED_IDENTIFIERS][QUOTED_IDENTIFIERS] |ON |
| [DATEFORMAT][DATEFORMAT] |hné |
| [DATEFIRST][DATEFIRST] |7 |

## <a name="next-steps"></a>Következő lépések
tooconnect és a Visual Studio lekérdezés [lekérdezése a Visual Studio][Query with Visual Studio]. További információk a hitelesítési beállítások toolearn lásd [hitelesítési tooAzure SQL Data Warehouse][Authentication tooAzure SQL Data Warehouse].

<!--Articles-->
[Query with Visual Studio]: ./sql-data-warehouse-query-visual-studio.md
[Authentication tooAzure SQL Data Warehouse]: ./sql-data-warehouse-authentication.md

<!--MSDN references-->
[ADO.NET]: https://msdn.microsoft.com/library/e80y5yhx(v=vs.110).aspx
[ODBC]: https://msdn.microsoft.com/library/jj730314.aspx
[PHP]: https://msdn.microsoft.com/library/cc296172.aspx?f=255&MSPPError=-2147217396
[JDBC]: https://msdn.microsoft.com/library/mt484311(v=sql.110).aspx
[ANSI_NULLS]: https://msdn.microsoft.com/library/ms188048.aspx
[QUOTED_IDENTIFIERS]: https://msdn.microsoft.com/library/ms174393.aspx
[DATEFORMAT]: https://msdn.microsoft.com/library/ms189491.aspx
[DATEFIRST]: https://msdn.microsoft.com/library/ms181598.aspx

<!--Other-->
[Azure portal]: https://portal.azure.com

<!--Image references-->
[1]: media/sql-data-warehouse-connect-overview/get-server-name.png


