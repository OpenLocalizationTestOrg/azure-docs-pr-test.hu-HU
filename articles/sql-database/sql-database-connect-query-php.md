---
title: aaaUse PHP tooquery Azure SQL Database |} Microsoft Docs
description: "Ez a témakör bemutatja, hogyan toouse PHP toocreate egy programot, amely kapcsolatot tooan Azure SQL Database és a lekérdezés Transact-SQL utasítás használatával."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 4e71db4a-a 22f-4f1c-83e5-4a34a036ecf3
ms.service: sql-database
ms.custom: mvc,develop apps
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: php
ms.topic: hero-article
ms.date: 08/08/2017
ms.author: carlrab
ms.openlocfilehash: 5fc49dcc42ab07cc1bec554be39bdf08dbd6f75e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-php-tooquery-an-azure-sql-database"></a><span data-ttu-id="b657c-103">A PHP tooquery Azure SQL-adatbázis használata</span><span class="sxs-lookup"><span data-stu-id="b657c-103">Use PHP tooquery an Azure SQL database</span></span>

<span data-ttu-id="b657c-104">Gyors üzembe helyezési oktatóanyag bemutatja, hogyan toouse [PHP](http://php.net/manual/en/intro-whatis.php) toocreate egy program tooconnect tooan Azure SQL adatbázis- és a Transact-SQL utasítás tooquery adatok felhasználásával.</span><span class="sxs-lookup"><span data-stu-id="b657c-104">This quick start tutorial demonstrates how toouse [PHP](http://php.net/manual/en/intro-whatis.php) toocreate a program tooconnect tooan Azure SQL database and use Transact-SQL statements tooquery data.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b657c-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="b657c-105">Prerequisites</span></span>

<span data-ttu-id="b657c-106">toocomplete a gyors üzembe helyezési útmutató, győződjön meg arról, hogy a következő hello:</span><span class="sxs-lookup"><span data-stu-id="b657c-106">toocomplete this quick start tutorial, make sure you have hello following:</span></span>

- <span data-ttu-id="b657c-107">Azure SQL Database-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="b657c-107">An Azure SQL database.</span></span> <span data-ttu-id="b657c-108">A gyors üzembe helyezési hoznak létre az egyik a gyors üzembe helyezések hello erőforrást használ:</span><span class="sxs-lookup"><span data-stu-id="b657c-108">This quick start uses hello resources created in one of these quick starts:</span></span> 

   - [<span data-ttu-id="b657c-109">DB létrehozása – portál</span><span class="sxs-lookup"><span data-stu-id="b657c-109">Create DB - Portal</span></span>](sql-database-get-started-portal.md)
   - [<span data-ttu-id="b657c-110">DB létrehozása – CLI</span><span class="sxs-lookup"><span data-stu-id="b657c-110">Create DB - CLI</span></span>](sql-database-get-started-cli.md)
   - [<span data-ttu-id="b657c-111">DB létrehozása – PowerShell</span><span class="sxs-lookup"><span data-stu-id="b657c-111">Create DB - PowerShell</span></span>](sql-database-get-started-powershell.md)

- <span data-ttu-id="b657c-112">A [kiszolgálószintű tűzfalszabály](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) hello nyilvános IP-cím hello számítógép használja a gyors üzembe helyezési útmutató.</span><span class="sxs-lookup"><span data-stu-id="b657c-112">A [server-level firewall rule](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) for hello public IP address of hello computer you use for this quick start tutorial.</span></span>

- <span data-ttu-id="b657c-113">Telepítette a PHP-t és az operációs rendszerének megfelelő kapcsolódó szoftvereket.</span><span class="sxs-lookup"><span data-stu-id="b657c-113">You have installed PHP and related software for your operating system.</span></span>

    - <span data-ttu-id="b657c-114">**MacOS**: Homebrew telepítéséhez és PHP-hello ODBC-illesztőprogram és az Sqlcmd ESZKÖZRE telepíteni, és telepítse az SQL Server hello PHP-illesztőprogramot.</span><span class="sxs-lookup"><span data-stu-id="b657c-114">**MacOS**: Install Homebrew and PHP, install hello ODBC driver and SQLCMD, and then install hello PHP Driver for SQL Server.</span></span> <span data-ttu-id="b657c-115">Lásd az [1.2, 1.3 és 2.1 lépést](https://www.microsoft.com/en-us/sql-server/developer-get-started/php/mac/).</span><span class="sxs-lookup"><span data-stu-id="b657c-115">See [Steps 1.2, 1.3, and 2.1](https://www.microsoft.com/en-us/sql-server/developer-get-started/php/mac/).</span></span>
    - <span data-ttu-id="b657c-116">**Ubuntu**: telepítse a PHP és más szükséges csomagokat, majd a telepítés hello PHP illesztőprogram az SQL Server.</span><span class="sxs-lookup"><span data-stu-id="b657c-116">**Ubuntu**:  Install PHP and other required packages, and then install hello PHP Driver for SQL Server.</span></span> <span data-ttu-id="b657c-117">Lásd az [1.2 és 2.1 lépést](https://www.microsoft.com/sql-server/developer-get-started/php/ubuntu/).</span><span class="sxs-lookup"><span data-stu-id="b657c-117">See [Steps 1.2 and 2.1](https://www.microsoft.com/sql-server/developer-get-started/php/ubuntu/).</span></span>
    - <span data-ttu-id="b657c-118">**Windows**: telepítés hello legújabb verzióját, a PHP az IIS Express, Microsoft SQL Server az IIS Express, Chocolatey, ODBC-illesztőprogram hello és SQLCMD Drivers hello legújabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="b657c-118">**Windows**: Install hello newest version of PHP for IIS Express, hello newest version of Microsoft Drivers for SQL Server in IIS Express, Chocolatey, hello ODBC driver, and SQLCMD.</span></span> <span data-ttu-id="b657c-119">Lásd az [1.2 és 1.3 lépést](https://www.microsoft.com/sql-server/developer-get-started/php/windows/).</span><span class="sxs-lookup"><span data-stu-id="b657c-119">See [Steps 1.2 and 1.3](https://www.microsoft.com/sql-server/developer-get-started/php/windows/).</span></span>    

## <a name="sql-server-connection-information"></a><span data-ttu-id="b657c-120">Az SQL-kiszolgáló kapcsolatadatai</span><span class="sxs-lookup"><span data-stu-id="b657c-120">SQL server connection information</span></span>

<span data-ttu-id="b657c-121">Hello kapcsolat szükséges információkat tooconnect toohello Azure SQL adatbázis beolvasása.</span><span class="sxs-lookup"><span data-stu-id="b657c-121">Get hello connection information needed tooconnect toohello Azure SQL database.</span></span> <span data-ttu-id="b657c-122">Hello teljes kiszolgálónév, az adatbázisnév és a bejelentkezési adatok a következő eljárások hello kell.</span><span class="sxs-lookup"><span data-stu-id="b657c-122">You will need hello fully qualified server name, database name, and login information in hello next procedures.</span></span>

1. <span data-ttu-id="b657c-123">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="b657c-123">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="b657c-124">Válassza ki **SQL-adatbázisok** hello bal oldali menüben kattintson a hello adatbázis **SQL-adatbázisok** lap.</span><span class="sxs-lookup"><span data-stu-id="b657c-124">Select **SQL Databases** from hello left-hand menu, and click your database on hello **SQL databases** page.</span></span> 
3. <span data-ttu-id="b657c-125">A hello **áttekintése** lap az adatbázishoz, tekintse át hello teljesen minősített kiszolgáló neve, ahogy az a következő kép hello.</span><span class="sxs-lookup"><span data-stu-id="b657c-125">On hello **Overview** page for your database, review hello fully qualified server name as shown in hello following image.</span></span> <span data-ttu-id="b657c-126">Hello server name toobring hello másolatot is mutat **toocopy kattintson** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="b657c-126">You can hover over hello server name toobring up hello **Click toocopy** option.</span></span>  

   ![server-name](./media/sql-database-connect-query-dotnet/server-name.png) 

4. <span data-ttu-id="b657c-128">Ha elfelejti a kiszolgálói bejelentkezési adatok, keresse meg a toohello SQL adatbázis-kiszolgáló lapon tooview hello server admin neve és, ha szükséges, állítsa vissza a hello jelszót.</span><span class="sxs-lookup"><span data-stu-id="b657c-128">If you forget your server login information, navigate toohello SQL Database server page tooview hello server admin name and, if necessary, reset hello password.</span></span>     
    
## <a name="insert-code-tooquery-sql-database"></a><span data-ttu-id="b657c-129">Helyezze be a kódját tooquery SQL-adatbázis</span><span class="sxs-lookup"><span data-stu-id="b657c-129">Insert code tooquery SQL database</span></span>

1. <span data-ttu-id="b657c-130">Egy tetszőleges szövegszerkesztőben hozza létre a **sqltest.php** nevű új fájlt.</span><span class="sxs-lookup"><span data-stu-id="b657c-130">In your favorite text editor, create a new file, **sqltest.php**.</span></span>  

2. <span data-ttu-id="b657c-131">Cserélje ki hello tartalmát hello az alábbi kód, és adja hozzá a megfelelő értékeket hello a kiszolgáló, az adatbázis, a felhasználó és a jelszavát.</span><span class="sxs-lookup"><span data-stu-id="b657c-131">Replace hello contents with hello following code and add hello appropriate values for your server, database, user, and password.</span></span>

   ```PHP
   <?php
   $serverName = "your_server.database.windows.net";
   $connectionOptions = array(
       "Database" => "your_database",
       "Uid" => "your_username",
       "PWD" => "your_password"
   );
   //Establishes hello connection
   $conn = sqlsrv_connect($serverName, $connectionOptions);
   $tsql= "SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
           FROM [SalesLT].[ProductCategory] pc
           JOIN [SalesLT].[Product] p
        ON pc.productcategoryid = p.productcategoryid";
   $getResults= sqlsrv_query($conn, $tsql);
   echo ("Reading data from table" . PHP_EOL);
   if ($getResults == FALSE)
       echo (sqlsrv_errors());
   while ($row = sqlsrv_fetch_array($getResults, SQLSRV_FETCH_ASSOC)) {
    echo ($row['CategoryName'] . " " . $row['ProductName'] . PHP_EOL);
   }
   sqlsrv_free_stmt($getResults);
   ?>
   ```

## <a name="run-hello-code"></a><span data-ttu-id="b657c-132">Hello kód futtatása</span><span class="sxs-lookup"><span data-stu-id="b657c-132">Run hello code</span></span>

1. <span data-ttu-id="b657c-133">Hello parancssorban futtassa a következő parancsok hello:</span><span class="sxs-lookup"><span data-stu-id="b657c-133">At hello command prompt, run hello following commands:</span></span>

   ```php
   php sqltest.php
   ```

2. <span data-ttu-id="b657c-134">Győződjön meg arról, hogy hello felső 20 sorokat ad vissza, és zárja be hello alkalmazás ablak.</span><span class="sxs-lookup"><span data-stu-id="b657c-134">Verify that hello top 20 rows are returned and then close hello application window.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b657c-135">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b657c-135">Next steps</span></span>
- [<span data-ttu-id="b657c-136">Az első SQL Database-adatbázis megtervezése</span><span class="sxs-lookup"><span data-stu-id="b657c-136">Design your first Azure SQL database</span></span>](sql-database-design-first-database.md)
- [<span data-ttu-id="b657c-137">SQL Serverre készült Microsoft PHP-illesztőprogramok</span><span class="sxs-lookup"><span data-stu-id="b657c-137">Microsoft PHP Drivers for SQL Server</span></span>](https://github.com/Microsoft/msphpsql/)
- [<span data-ttu-id="b657c-138">Problémák jelentése és kérdezés</span><span class="sxs-lookup"><span data-stu-id="b657c-138">Report issues or ask questions</span></span>](https://github.com/Microsoft/msphpsql/issues)
