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
# <a name="connect-tooazure-sql-data-warehouse"></a><span data-ttu-id="8face-103">Csatlakozás az SQL Data Warehouse tooAzure</span><span class="sxs-lookup"><span data-stu-id="8face-103">Connect tooAzure SQL Data Warehouse</span></span>
<span data-ttu-id="8face-104">Ez a cikk segítséget nyújt a csatlakoztatott tooSQL adatraktár lekérése hello első alkalommal.</span><span class="sxs-lookup"><span data-stu-id="8face-104">This article helps you get connected tooSQL Data Warehouse for hello first time.</span></span>

## <a name="find-your-server-name"></a><span data-ttu-id="8face-105">A kiszolgálónév lekérdezése</span><span class="sxs-lookup"><span data-stu-id="8face-105">Find your server name</span></span>
<span data-ttu-id="8face-106">első lépés tooconnecting tooSQL adatraktár ismerete hello hogyan toofind a kiszolgáló nevét.</span><span class="sxs-lookup"><span data-stu-id="8face-106">hello first step tooconnecting tooSQL Data Warehouse is knowing how toofind your server name.</span></span>  <span data-ttu-id="8face-107">Például a hello kiszolgáló nevét a következő példa hello sample.database.windows.net.</span><span class="sxs-lookup"><span data-stu-id="8face-107">For example, hello server name in hello following example is sample.database.windows.net.</span></span> <span data-ttu-id="8face-108">toofind hello teljes kiszolgálónév:</span><span class="sxs-lookup"><span data-stu-id="8face-108">toofind hello fully qualified server name:</span></span>

1. <span data-ttu-id="8face-109">Nyissa meg toohello [Azure-portálon][Azure portal].</span><span class="sxs-lookup"><span data-stu-id="8face-109">Go toohello [Azure portal][Azure portal].</span></span>
2. <span data-ttu-id="8face-110">Kattintson az **SQL-adatbázisok** elemre</span><span class="sxs-lookup"><span data-stu-id="8face-110">Click on **SQL databases**</span></span> 
3. <span data-ttu-id="8face-111">Kattintson a kívánt való tooconnect hello adatbázisra.</span><span class="sxs-lookup"><span data-stu-id="8face-111">Click on hello database you want tooconnect to.</span></span>
4. <span data-ttu-id="8face-112">Keresse meg a teljes kiszolgálónevet hello.</span><span class="sxs-lookup"><span data-stu-id="8face-112">Locate hello full server name.</span></span>
   
    ![Teljes kiszolgálónév][1]

## <a name="supported-drivers-and-connection-strings"></a><span data-ttu-id="8face-114">Támogatott illesztők és kapcsolati karakterláncok</span><span class="sxs-lookup"><span data-stu-id="8face-114">Supported drivers and connection strings</span></span>
<span data-ttu-id="8face-115">Az Azure SQL Data Warehouse a következő illesztőprogramokat támogatja: [ADO.NET][ADO.NET], [ODBC][ODBC], [PHP][PHP] és [JDBC][JDBC].</span><span class="sxs-lookup"><span data-stu-id="8face-115">Azure SQL Data Warehouse supports [ADO.NET][ADO.NET], [ODBC][ODBC], [PHP][PHP], and [JDBC][JDBC].</span></span> <span data-ttu-id="8face-116">Kattintson az egyik hello megelőző illesztőprogramok toofind hello legújabb verzióra, és a dokumentációt.</span><span class="sxs-lookup"><span data-stu-id="8face-116">Click on one of hello preceding drivers toofind hello latest version and documentation.</span></span> <span data-ttu-id="8face-117">tooautomatically hello kapcsolati karakterlánc az Ön által használt hello illesztőprogram generálása hello Azure portál, rákattinthat a hello **adatbázis-kapcsolati karakterláncok megjelenítése** az előző példa hello.</span><span class="sxs-lookup"><span data-stu-id="8face-117">tooautomatically generate hello connection string for hello driver that you are using from hello Azure portal, you can click on hello **Show database connection strings** from hello preceding example.</span></span>  <span data-ttu-id="8face-118">A következő néhány példa bemutatja, hogy néz ki a kapcsolati karakterlánc az egyes illesztők esetében.</span><span class="sxs-lookup"><span data-stu-id="8face-118">Following are also some examples of what a connection string looks like for each driver.</span></span>

> [!NOTE]
> <span data-ttu-id="8face-119">Fontolja meg, hello kapcsolat időtúllépése too300 másodperc tooallow a kapcsolat toosurvive rövid időszakokra, elérhetetlensége beállítását.</span><span class="sxs-lookup"><span data-stu-id="8face-119">Consider setting hello connection timeout too300 seconds tooallow your connection toosurvive short periods of unavailability.</span></span>
> 
> 

### <a name="adonet-connection-string-example"></a><span data-ttu-id="8face-120">Minta ADO.NET kapcsolati karakterlánc</span><span class="sxs-lookup"><span data-stu-id="8face-120">ADO.NET connection string example</span></span>
```C#
Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};User ID={your_user_name};Password={your_password_here};Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;
```

### <a name="odbc-connection-string-example"></a><span data-ttu-id="8face-121">Minta ODBC kapcsolati karakterlánc</span><span class="sxs-lookup"><span data-stu-id="8face-121">ODBC connection string example</span></span>
```C#
Driver={SQL Server Native Client 11.0};Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};Uid={your_user_name};Pwd={your_password_here};Encrypt=yes;TrustServerCertificate=no;Connection Timeout=30;
```

### <a name="php-connection-string-example"></a><span data-ttu-id="8face-122">Minta PHP kapcsolati karakterlánc</span><span class="sxs-lookup"><span data-stu-id="8face-122">PHP connection string example</span></span>
```PHP
Server: {your_server}.database.windows.net,1433 \r\nSQL Database: {your_database}\r\nUser Name: {your_user_name}\r\n\r\nPHP Data Objects(PDO) Sample Code:\r\n\r\ntry {\r\n   $conn = new PDO ( \"sqlsrv:server = tcp:{your_server}.database.windows.net,1433; Database = {your_database}\", \"{your_user_name}\", \"{your_password_here}\");\r\n    $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );\r\n}\r\ncatch ( PDOException $e ) {\r\n   print( \"Error connecting tooSQL Server.\" );\r\n   die(print_r($e));\r\n}\r\n\rSQL Server Extension Sample Code:\r\n\r\n$connectionInfo = array(\"UID\" => \"{your_user_name}\", \"pwd\" => \"{your_password_here}\", \"Database\" => \"{your_database}\", \"LoginTimeout\" => 30, \"Encrypt\" => 1, \"TrustServerCertificate\" => 0);\r\n$serverName = \"tcp:{your_server}.database.windows.net,1433\";\r\n$conn = sqlsrv_connect($serverName, $connectionInfo);
```

### <a name="jdbc-connection-string-example"></a><span data-ttu-id="8face-123">Minta JDBC kapcsolati karakterlánc</span><span class="sxs-lookup"><span data-stu-id="8face-123">JDBC connection string example</span></span>
```Java
jdbc:sqlserver://yourserver.database.windows.net:1433;database=yourdatabase;user={your_user_name};password={your_password_here};encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;
```

## <a name="connection-settings"></a><span data-ttu-id="8face-124">Kapcsolati beállítások</span><span class="sxs-lookup"><span data-stu-id="8face-124">Connection settings</span></span>
<span data-ttu-id="8face-125">Az SQL Data Warehouse szabványosít néhány beállítást a csatlakozás és az objektumlétrehozás során.</span><span class="sxs-lookup"><span data-stu-id="8face-125">SQL Data Warehouse standardizes some settings during connection and object creation.</span></span> <span data-ttu-id="8face-126">Ezeket a beállításokat nem lehet felülírni, és a következők lehetnek:</span><span class="sxs-lookup"><span data-stu-id="8face-126">These settings cannot be overridden and include:</span></span>

| <span data-ttu-id="8face-127">Adatbázis-beállítások</span><span class="sxs-lookup"><span data-stu-id="8face-127">Database Setting</span></span> | <span data-ttu-id="8face-128">Érték</span><span class="sxs-lookup"><span data-stu-id="8face-128">Value</span></span> |
|:--- |:--- |
| <span data-ttu-id="8face-129">[ANSI_NULLS][ANSI_NULLS]</span><span class="sxs-lookup"><span data-stu-id="8face-129">[ANSI_NULLS][ANSI_NULLS]</span></span> |<span data-ttu-id="8face-130">ON</span><span class="sxs-lookup"><span data-stu-id="8face-130">ON</span></span> |
| <span data-ttu-id="8face-131">[QUOTED_IDENTIFIERS][QUOTED_IDENTIFIERS]</span><span class="sxs-lookup"><span data-stu-id="8face-131">[QUOTED_IDENTIFIERS][QUOTED_IDENTIFIERS]</span></span> |<span data-ttu-id="8face-132">ON</span><span class="sxs-lookup"><span data-stu-id="8face-132">ON</span></span> |
| <span data-ttu-id="8face-133">[DATEFORMAT][DATEFORMAT]</span><span class="sxs-lookup"><span data-stu-id="8face-133">[DATEFORMAT][DATEFORMAT]</span></span> |<span data-ttu-id="8face-134">hné</span><span class="sxs-lookup"><span data-stu-id="8face-134">mdy</span></span> |
| <span data-ttu-id="8face-135">[DATEFIRST][DATEFIRST]</span><span class="sxs-lookup"><span data-stu-id="8face-135">[DATEFIRST][DATEFIRST]</span></span> |<span data-ttu-id="8face-136">7</span><span class="sxs-lookup"><span data-stu-id="8face-136">7</span></span> |

## <a name="next-steps"></a><span data-ttu-id="8face-137">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8face-137">Next steps</span></span>
<span data-ttu-id="8face-138">tooconnect és a Visual Studio lekérdezés [lekérdezése a Visual Studio][Query with Visual Studio].</span><span class="sxs-lookup"><span data-stu-id="8face-138">tooconnect and query with Visual Studio, see [Query with Visual Studio][Query with Visual Studio].</span></span> <span data-ttu-id="8face-139">További információk a hitelesítési beállítások toolearn lásd [hitelesítési tooAzure SQL Data Warehouse][Authentication tooAzure SQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="8face-139">toolearn more about authentication options, see [Authentication tooAzure SQL Data Warehouse][Authentication tooAzure SQL Data Warehouse].</span></span>

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


