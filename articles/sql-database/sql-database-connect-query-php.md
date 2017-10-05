---
title: "PHP használata Azure SQL Database-adatbázis lekérdezéséhez | Microsoft Docs"
description: "Ez a témakör bemutatja, hogyan használhatja a PHP-t egy Azure SQL Database-adatbázishoz csatlakozó program létrehozásához, és hogyan hajthat végre lekérdezést Transact-SQL-utasításokkal."
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
ms.openlocfilehash: 3a43472ad2be4a0fd6f7126f72433acd8b5f25fd
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="use-php-to-query-an-azure-sql-database"></a><span data-ttu-id="cd3db-103">PHP használata Azure SQL Database-adatbázis lekérdezéséhez</span><span class="sxs-lookup"><span data-stu-id="cd3db-103">Use PHP to query an Azure SQL database</span></span>

<span data-ttu-id="cd3db-104">Ez a gyors üzembehelyezési útmutató ismerteti, hogyan használható a [PHP](http://php.net/manual/en/intro-whatis.php) egy olyan program létrehozásához, amely egy Azure SQL Database-adatbázishoz csatlakozik, és hogyan lehet Transact-SQL-utasítások használatával adatokat lekérdezni.</span><span class="sxs-lookup"><span data-stu-id="cd3db-104">This quick start tutorial demonstrates how to use [PHP](http://php.net/manual/en/intro-whatis.php) to create a program to connect to an Azure SQL database and use Transact-SQL statements to query data.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cd3db-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="cd3db-105">Prerequisites</span></span>

<span data-ttu-id="cd3db-106">A gyors üzembehelyezési útmutató elvégzéséhez győződjön meg arról, hogy rendelkezik az alábbiakkal:</span><span class="sxs-lookup"><span data-stu-id="cd3db-106">To complete this quick start tutorial, make sure you have the following:</span></span>

- <span data-ttu-id="cd3db-107">Azure SQL Database-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="cd3db-107">An Azure SQL database.</span></span> <span data-ttu-id="cd3db-108">Ez a rövid útmutató az alábbi rövid útmutatók egyikében létrehozott erőforrásokat használja:</span><span class="sxs-lookup"><span data-stu-id="cd3db-108">This quick start uses the resources created in one of these quick starts:</span></span> 

   - [<span data-ttu-id="cd3db-109">DB létrehozása – portál</span><span class="sxs-lookup"><span data-stu-id="cd3db-109">Create DB - Portal</span></span>](sql-database-get-started-portal.md)
   - [<span data-ttu-id="cd3db-110">DB létrehozása – CLI</span><span class="sxs-lookup"><span data-stu-id="cd3db-110">Create DB - CLI</span></span>](sql-database-get-started-cli.md)
   - [<span data-ttu-id="cd3db-111">DB létrehozása – PowerShell</span><span class="sxs-lookup"><span data-stu-id="cd3db-111">Create DB - PowerShell</span></span>](sql-database-get-started-powershell.md)

- <span data-ttu-id="cd3db-112">A gyors üzembe helyezési útmutatóhoz használt számítógép nyilvános IP-címére vonatkozó [kiszolgálószintű tűzfalszabály](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).</span><span class="sxs-lookup"><span data-stu-id="cd3db-112">A [server-level firewall rule](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) for the public IP address of the computer you use for this quick start tutorial.</span></span>

- <span data-ttu-id="cd3db-113">Telepítette a PHP-t és az operációs rendszerének megfelelő kapcsolódó szoftvereket.</span><span class="sxs-lookup"><span data-stu-id="cd3db-113">You have installed PHP and related software for your operating system.</span></span>

    - <span data-ttu-id="cd3db-114">**MacOS**: Telepítse a Homebrew-t és a PHP-t, telepítse az ODBC-illesztőt és az SQLCMD-t, majd telepítse az SQL Serverhez készült PHP-illesztőt.</span><span class="sxs-lookup"><span data-stu-id="cd3db-114">**MacOS**: Install Homebrew and PHP, install the ODBC driver and SQLCMD, and then install the PHP Driver for SQL Server.</span></span> <span data-ttu-id="cd3db-115">Lásd az [1.2, 1.3 és 2.1 lépést](https://www.microsoft.com/en-us/sql-server/developer-get-started/php/mac/).</span><span class="sxs-lookup"><span data-stu-id="cd3db-115">See [Steps 1.2, 1.3, and 2.1](https://www.microsoft.com/en-us/sql-server/developer-get-started/php/mac/).</span></span>
    - <span data-ttu-id="cd3db-116">**Ubuntu**: Telepítse a PHP-t és a többi szükséges csomagot, majd telepítse az SQL Serverhez készült PHP-illesztőt.</span><span class="sxs-lookup"><span data-stu-id="cd3db-116">**Ubuntu**:  Install PHP and other required packages, and then install the PHP Driver for SQL Server.</span></span> <span data-ttu-id="cd3db-117">Lásd az [1.2 és 2.1 lépést](https://www.microsoft.com/sql-server/developer-get-started/php/ubuntu/).</span><span class="sxs-lookup"><span data-stu-id="cd3db-117">See [Steps 1.2 and 2.1](https://www.microsoft.com/sql-server/developer-get-started/php/ubuntu/).</span></span>
    - <span data-ttu-id="cd3db-118">**Windows**: Telepítse az IIS Expresshez készült PHP legújabb verzióját, az SQL Server Microsoft-illesztőinek legújabb verzióját az IIS Expressben, a Chocolatey-t, az ODBC-illesztőt és az SQLCMD-t.</span><span class="sxs-lookup"><span data-stu-id="cd3db-118">**Windows**: Install the newest version of PHP for IIS Express, the newest version of Microsoft Drivers for SQL Server in IIS Express, Chocolatey, the ODBC driver, and SQLCMD.</span></span> <span data-ttu-id="cd3db-119">Lásd az [1.2 és 1.3 lépést](https://www.microsoft.com/sql-server/developer-get-started/php/windows/).</span><span class="sxs-lookup"><span data-stu-id="cd3db-119">See [Steps 1.2 and 1.3](https://www.microsoft.com/sql-server/developer-get-started/php/windows/).</span></span>    

## <a name="sql-server-connection-information"></a><span data-ttu-id="cd3db-120">Az SQL-kiszolgáló kapcsolatadatai</span><span class="sxs-lookup"><span data-stu-id="cd3db-120">SQL server connection information</span></span>

<span data-ttu-id="cd3db-121">Kérje le az Azure SQL-adatbázishoz való csatlakozáshoz szükséges kapcsolatadatokat.</span><span class="sxs-lookup"><span data-stu-id="cd3db-121">Get the connection information needed to connect to the Azure SQL database.</span></span> <span data-ttu-id="cd3db-122">A későbbi eljárásokban szüksége lesz a teljes kiszolgálónévre, az adatbázis nevére és a bejelentkezési adatokra.</span><span class="sxs-lookup"><span data-stu-id="cd3db-122">You will need the fully qualified server name, database name, and login information in the next procedures.</span></span>

1. <span data-ttu-id="cd3db-123">Jelentkezzen be az [Azure portálra](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="cd3db-123">Log in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="cd3db-124">Válassza az **SQL-adatbázisok** elemet a bal oldali menüben, majd kattintson az új adatbázisra az **SQL-adatbázisok** oldalon.</span><span class="sxs-lookup"><span data-stu-id="cd3db-124">Select **SQL Databases** from the left-hand menu, and click your database on the **SQL databases** page.</span></span> 
3. <span data-ttu-id="cd3db-125">Az adatbázis **Áttekintés** oldalán tekintse meg a teljes kiszolgálónevet, amint az az alábbi képen látható.</span><span class="sxs-lookup"><span data-stu-id="cd3db-125">On the **Overview** page for your database, review the fully qualified server name as shown in the following image.</span></span> <span data-ttu-id="cd3db-126">Ha a mutatót a kiszolgáló neve fölé viszi, megjelenik a **Kattintson a másoláshoz** lehetőség.</span><span class="sxs-lookup"><span data-stu-id="cd3db-126">You can hover over the server name to bring up the **Click to copy** option.</span></span>  

   ![server-name](./media/sql-database-connect-query-dotnet/server-name.png) 

4. <span data-ttu-id="cd3db-128">Amennyiben elfelejtette a kiszolgálója bejelentkezési adatait, lépjen az SQL Database-kiszolgáló oldalára, és itt megtudhatja a kiszolgáló rendszergazdájának nevét, valamint szükség esetén új jelszót kérhet.</span><span class="sxs-lookup"><span data-stu-id="cd3db-128">If you forget your server login information, navigate to the SQL Database server page to view the server admin name and, if necessary, reset the password.</span></span>     
    
## <a name="insert-code-to-query-sql-database"></a><span data-ttu-id="cd3db-129">Kód beszúrása SQL-adatbázis lekérdezéséhez</span><span class="sxs-lookup"><span data-stu-id="cd3db-129">Insert code to query SQL database</span></span>

1. <span data-ttu-id="cd3db-130">Egy tetszőleges szövegszerkesztőben hozza létre a **sqltest.php** nevű új fájlt.</span><span class="sxs-lookup"><span data-stu-id="cd3db-130">In your favorite text editor, create a new file, **sqltest.php**.</span></span>  

2. <span data-ttu-id="cd3db-131">Cserélje le a tartalmat a következő kódra, és adja meg a kiszolgáló és az adatbázis megfelelő adatait, valamint a felhasználót és a jelszót.</span><span class="sxs-lookup"><span data-stu-id="cd3db-131">Replace the contents with the following code and add the appropriate values for your server, database, user, and password.</span></span>

   ```PHP
   <?php
   $serverName = "your_server.database.windows.net";
   $connectionOptions = array(
       "Database" => "your_database",
       "Uid" => "your_username",
       "PWD" => "your_password"
   );
   //Establishes the connection
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

## <a name="run-the-code"></a><span data-ttu-id="cd3db-132">A kód futtatása</span><span class="sxs-lookup"><span data-stu-id="cd3db-132">Run the code</span></span>

1. <span data-ttu-id="cd3db-133">Futtassa az alábbi parancsokat a parancssorban:</span><span class="sxs-lookup"><span data-stu-id="cd3db-133">At the command prompt, run the following commands:</span></span>

   ```php
   php sqltest.php
   ```

2. <span data-ttu-id="cd3db-134">Győződjön meg arról, hogy a parancssori felület visszaadta az első 20 sort, majd zárja be az alkalmazásablakot.</span><span class="sxs-lookup"><span data-stu-id="cd3db-134">Verify that the top 20 rows are returned and then close the application window.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cd3db-135">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cd3db-135">Next steps</span></span>
- [<span data-ttu-id="cd3db-136">Az első SQL Database-adatbázis megtervezése</span><span class="sxs-lookup"><span data-stu-id="cd3db-136">Design your first Azure SQL database</span></span>](sql-database-design-first-database.md)
- [<span data-ttu-id="cd3db-137">SQL Serverre készült Microsoft PHP-illesztőprogramok</span><span class="sxs-lookup"><span data-stu-id="cd3db-137">Microsoft PHP Drivers for SQL Server</span></span>](https://github.com/Microsoft/msphpsql/)
- [<span data-ttu-id="cd3db-138">Problémák jelentése és kérdezés</span><span class="sxs-lookup"><span data-stu-id="cd3db-138">Report issues or ask questions</span></span>](https://github.com/Microsoft/msphpsql/issues)
