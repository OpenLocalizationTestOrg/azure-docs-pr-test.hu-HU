---
title: "Node.js használata Azure SQL Database-adatbázis lekérdezéséhez | Microsoft Docs"
description: "Ez a témakör bemutatja, hogyan használhatja a Node.js-t egy Azure SQL Database-adatbázishoz csatlakozó program létrehozásához, és hogyan hajthat végre lekérdezést Transact-SQL-utasításokkal."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 53f70e37-5eb4-400d-972e-dd7ea0caacd4
ms.service: sql-database
ms.custom: mvc,develop apps
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: hero-article
ms.date: 07/05/2017
ms.author: carlrab
ms.openlocfilehash: 1907a95df9132c059d7985b6d5cd913536bf3403
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="use-nodejs-to-query-an-azure-sql-database"></a><span data-ttu-id="d67c4-103">Node.js használata Azure SQL Database-adatbázis lekérdezéséhez</span><span class="sxs-lookup"><span data-stu-id="d67c4-103">Use Node.js to query an Azure SQL database</span></span>

<span data-ttu-id="d67c4-104">Ez a gyors üzembehelyezési oktatóanyag ismerteti, hogyan használható a [Node.js](https://nodejs.org/en/) egy olyan program létrehozásához, amely Azure SQL Database-adatbázishoz csatlakozik, és hogyan lehet Transact-SQL-utasítások használatával adatokat lekérdezni.</span><span class="sxs-lookup"><span data-stu-id="d67c4-104">This quick start tutorial demonstrates how to use [Node.js](https://nodejs.org/en/) to create a program to connect to an Azure SQL database and use Transact-SQL statements to query data.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d67c4-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d67c4-105">Prerequisites</span></span>

<span data-ttu-id="d67c4-106">A gyors üzembehelyezési útmutató elvégzéséhez győződjön meg arról, hogy rendelkezik az alábbiakkal:</span><span class="sxs-lookup"><span data-stu-id="d67c4-106">To complete this quick start tutorial, make sure you have the following:</span></span>

- <span data-ttu-id="d67c4-107">Azure SQL Database-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="d67c4-107">An Azure SQL database.</span></span> <span data-ttu-id="d67c4-108">Ez a rövid útmutató az alábbi rövid útmutatók egyikében létrehozott erőforrásokat használja:</span><span class="sxs-lookup"><span data-stu-id="d67c4-108">This quick start uses the resources created in one of these quick starts:</span></span> 

   - [<span data-ttu-id="d67c4-109">DB létrehozása – portál</span><span class="sxs-lookup"><span data-stu-id="d67c4-109">Create DB - Portal</span></span>](sql-database-get-started-portal.md)
   - [<span data-ttu-id="d67c4-110">DB létrehozása – CLI</span><span class="sxs-lookup"><span data-stu-id="d67c4-110">Create DB - CLI</span></span>](sql-database-get-started-cli.md)
   - [<span data-ttu-id="d67c4-111">DB létrehozása – PowerShell</span><span class="sxs-lookup"><span data-stu-id="d67c4-111">Create DB - PowerShell</span></span>](sql-database-get-started-powershell.md)

- <span data-ttu-id="d67c4-112">A gyors üzembe helyezési útmutatóhoz használt számítógép nyilvános IP-címére vonatkozó [kiszolgálószintű tűzfalszabály](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).</span><span class="sxs-lookup"><span data-stu-id="d67c4-112">A [server-level firewall rule](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) for the public IP address of the computer you use for this quick start tutorial.</span></span>
- <span data-ttu-id="d67c4-113">Telepítette a Node.js-t és az operációs rendszerének megfelelő kapcsolódó szoftvereket.</span><span class="sxs-lookup"><span data-stu-id="d67c4-113">You have installed Node.js and related software for your operating system.</span></span>
    - <span data-ttu-id="d67c4-114">**MacOS**: Telepítse a Homebrew-t és a Node.js-t, majd telepítse az ODBC-illesztőt és az SQLCMD-t.</span><span class="sxs-lookup"><span data-stu-id="d67c4-114">**MacOS**: Install Homebrew and Node.js, and then install the ODBC driver and SQLCMD.</span></span> <span data-ttu-id="d67c4-115">Lásd az [1.2 és 1.3 lépést](https://www.microsoft.com/sql-server/developer-get-started/node/mac/).</span><span class="sxs-lookup"><span data-stu-id="d67c4-115">See [Step 1.2 and 1.3](https://www.microsoft.com/sql-server/developer-get-started/node/mac/).</span></span>
    - <span data-ttu-id="d67c4-116">**Ubuntu**: Telepítse a Node.js-t, majd telepítse az ODBC-illesztőt és az SQLCMD-t.</span><span class="sxs-lookup"><span data-stu-id="d67c4-116">**Ubuntu**: Install Node.js, and then install the ODBC driver and SQLCMD.</span></span> <span data-ttu-id="d67c4-117">Lásd az [1.2 és 1.3 lépést](https://www.microsoft.com/sql-server/developer-get-started/node/ubuntu/).</span><span class="sxs-lookup"><span data-stu-id="d67c4-117">See [Step 1.2 and 1.3](https://www.microsoft.com/sql-server/developer-get-started/node/ubuntu/) .</span></span>
    - <span data-ttu-id="d67c4-118">**Windows**: Telepítse a Chocolatey-t és a Node.js-t, majd telepítse az ODBC-illesztőt és az SQL CMD-t.</span><span class="sxs-lookup"><span data-stu-id="d67c4-118">**Windows**: Install Chocolatey and Node.js, and then install the ODBC driver and SQL CMD.</span></span> <span data-ttu-id="d67c4-119">Lásd az [1.2 és 1.3 lépést](https://www.microsoft.com/sql-server/developer-get-started/node/windows/).</span><span class="sxs-lookup"><span data-stu-id="d67c4-119">See [Step 1.2 and 1.3](https://www.microsoft.com/sql-server/developer-get-started/node/windows/).</span></span>

## <a name="sql-server-connection-information"></a><span data-ttu-id="d67c4-120">Az SQL-kiszolgáló kapcsolatadatai</span><span class="sxs-lookup"><span data-stu-id="d67c4-120">SQL server connection information</span></span>

<span data-ttu-id="d67c4-121">Kérje le az Azure SQL-adatbázishoz való csatlakozáshoz szükséges kapcsolatadatokat.</span><span class="sxs-lookup"><span data-stu-id="d67c4-121">Get the connection information needed to connect to the Azure SQL database.</span></span> <span data-ttu-id="d67c4-122">A későbbi eljárásokban szüksége lesz a teljes kiszolgálónévre, az adatbázis nevére és a bejelentkezési adatokra.</span><span class="sxs-lookup"><span data-stu-id="d67c4-122">You will need the fully qualified server name, database name, and login information in the next procedures.</span></span>

1. <span data-ttu-id="d67c4-123">Jelentkezzen be az [Azure portálra](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="d67c4-123">Log in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="d67c4-124">Válassza az **SQL-adatbázisok** elemet a bal oldali menüben, majd kattintson az új adatbázisra az **SQL-adatbázisok** oldalon.</span><span class="sxs-lookup"><span data-stu-id="d67c4-124">Select **SQL Databases** from the left-hand menu, and click your database on the **SQL databases** page.</span></span> 
3. <span data-ttu-id="d67c4-125">Az adatbázis **Áttekintés** oldalán tekintse meg a teljes kiszolgálónevet, amint az az alábbi képen látható.</span><span class="sxs-lookup"><span data-stu-id="d67c4-125">On the **Overview** page for your database, review the fully qualified server name as shown in the following image.</span></span> <span data-ttu-id="d67c4-126">Ha a mutatót a kiszolgáló neve fölé viszi, megjelenik a **Kattintson a másoláshoz** lehetőség.</span><span class="sxs-lookup"><span data-stu-id="d67c4-126">You can hover over the server name to bring up the **Click to copy** option.</span></span> 

   ![server-name](./media/sql-database-connect-query-dotnet/server-name.png) 

4. <span data-ttu-id="d67c4-128">Amennyiben elfelejtette Azure SQL Database-kiszolgálója bejelentkezési adatait, lépjen az SQL Database-kiszolgáló oldalára, és itt megtudhatja a kiszolgáló rendszergazdájának nevét, valamint szükség esetén visszaállíthatja a jelszót.</span><span class="sxs-lookup"><span data-stu-id="d67c4-128">If you have forgotten the login information for your Azure SQL Database server, navigate to the SQL Database server page to view the server admin name and, if necessary, reset the password.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d67c4-129">Rendelkeznie kell egy tűzfalszabállyal azon számítógép nyilvános IP-címéhez, amelyen ezt az oktatóanyagot elvégzi.</span><span class="sxs-lookup"><span data-stu-id="d67c4-129">You must have a firewall rule in place for the public IP address of the computer on which you perform this tutorial.</span></span> <span data-ttu-id="d67c4-130">Ha más számítógépet használ, vagy más nyilvános IP-címe van, hozzon létre egy [kiszolgálószintű tűzfalszabályt az Azure Portalon](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).</span><span class="sxs-lookup"><span data-stu-id="d67c4-130">If you are on a different computer or have a different public IP address, create a [server-level firewall rule using the Azure portal](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).</span></span> 

## <a name="create-a-nodejs-project"></a><span data-ttu-id="d67c4-131">Node.js-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="d67c4-131">Create a Node.js project</span></span>

<span data-ttu-id="d67c4-132">Nyisson meg egy parancssort, és hozzon létre egy *sqltest* nevű mappát.</span><span class="sxs-lookup"><span data-stu-id="d67c4-132">Open a command prompt and create a folder named *sqltest*.</span></span> <span data-ttu-id="d67c4-133">Keresse meg a létrehozott mappát, és futtassa a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="d67c4-133">Navigate to the folder you created and run the following command:</span></span>

    
    npm init -y
    npm install tedious
    npm install async
    

## <a name="insert-code-to-query-sql-database"></a><span data-ttu-id="d67c4-134">Kód beszúrása SQL-adatbázis lekérdezéséhez</span><span class="sxs-lookup"><span data-stu-id="d67c4-134">Insert code to query SQL database</span></span>

1. <span data-ttu-id="d67c4-135">A fejlesztői környezetben vagy egy tetszőleges szövegszerkesztőben hozzon létre egy **sqltest.js** nevű új fájlt.</span><span class="sxs-lookup"><span data-stu-id="d67c4-135">In your development environment or favorite text editor, create a new file, **sqltest.js**.</span></span>

2. <span data-ttu-id="d67c4-136">Cserélje le a tartalmat a következő kódra, és adja meg a kiszolgáló és az adatbázis megfelelő adatait, valamint a felhasználót és a jelszót.</span><span class="sxs-lookup"><span data-stu-id="d67c4-136">Replace the contents with the following code and add the appropriate values for your server, database, user, and password.</span></span>

   ```js
   var Connection = require('tedious').Connection;
   var Request = require('tedious').Request;

   // Create connection to database
   var config = 
      {
        userName: 'someuser', // update me
        password: 'somepassword', // update me
        server: 'edmacasqlserver.database.windows.net', // update me
        options: 
           {
              database: 'somedb' //update me
              , encrypt: true
           }
      }
   var connection = new Connection(config);

   // Attempt to connect and execute queries if connection goes through
   connection.on('connect', function(err) 
      {
        if (err) 
          {
             console.log(err)
          }
       else
          {
              queryDatabase()
          }
      }
    );

   function queryDatabase()
      { console.log('Reading rows from the Table...');

          // Read all rows from table
        request = new Request(
             "SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName FROM [SalesLT].[ProductCategory] pc JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid",
                function(err, rowCount, rows) 
                   {
                       console.log(rowCount + ' row(s) returned');
                       process.exit();
                   }
               );
    
        request.on('row', function(columns) {
           columns.forEach(function(column) {
               console.log("%s\t%s", column.metadata.colName, column.value);
            });
                });
        connection.execSql(request);
      }
```

## <a name="run-the-code"></a><span data-ttu-id="d67c4-137">A kód futtatása</span><span class="sxs-lookup"><span data-stu-id="d67c4-137">Run the code</span></span>

1. <span data-ttu-id="d67c4-138">Futtassa az alábbi parancsokat a parancssorban:</span><span class="sxs-lookup"><span data-stu-id="d67c4-138">At the command prompt, run the following commands:</span></span>

   ```js
   node sqltest.js
   ```

2. <span data-ttu-id="d67c4-139">Győződjön meg arról, hogy a parancssori felület visszaadta az első 20 sort, majd zárja be az alkalmazásablakot.</span><span class="sxs-lookup"><span data-stu-id="d67c4-139">Verify that the top 20 rows are returned and then close the application window.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d67c4-140">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d67c4-140">Next steps</span></span>

- <span data-ttu-id="d67c4-141">További információ az [SQL Serverhez készült Microsoft Node.js-illesztőről](https://docs.microsoft.com/sql/connect/node-js/node-js-driver-for-sql-server/)</span><span class="sxs-lookup"><span data-stu-id="d67c4-141">Learn about the [Microsoft Node.js Driver for SQL Server](https://docs.microsoft.com/sql/connect/node-js/node-js-driver-for-sql-server/)</span></span>
- <span data-ttu-id="d67c4-142">További információ [az Azure SQL Database-adatbázisokhoz való csatlakozásról és a .NET Core-ral való lekérdezésükről](sql-database-connect-query-dotnet-core.md) Windows/Linux/macOS rendszeren.</span><span class="sxs-lookup"><span data-stu-id="d67c4-142">Learn how to [connect and query an Azure SQL database using .NET core](sql-database-connect-query-dotnet-core.md) on Windows/Linux/macOS.</span></span>  
- <span data-ttu-id="d67c4-143">További információ [a .NET Core használatának első lépéseiről Windows/Linux/macOS rendszeren a parancssorral](/dotnet/core/tutorials/using-with-xplat-cli).</span><span class="sxs-lookup"><span data-stu-id="d67c4-143">Learn about [Getting started with .NET Core on Windows/Linux/macOS using the command line](/dotnet/core/tutorials/using-with-xplat-cli).</span></span>
- <span data-ttu-id="d67c4-144">További információ [az első Azure SQL Database-adatbázisának SSMS-sel való megtervezéséről](sql-database-design-first-database.md) és [az első Azure SQL Database-adatbázisának .NET-tel való megtervezéséről](sql-database-design-first-database-csharp.md).</span><span class="sxs-lookup"><span data-stu-id="d67c4-144">Learn how to [Design your first Azure SQL database using SSMS](sql-database-design-first-database.md) or [Design your first Azure SQL database using .NET](sql-database-design-first-database-csharp.md).</span></span>
- <span data-ttu-id="d67c4-145">További információ [az SSMS-hez való csatlakozásról és a lekérdezésről](sql-database-connect-query-ssms.md)</span><span class="sxs-lookup"><span data-stu-id="d67c4-145">Learn how to [Connect and query with SSMS](sql-database-connect-query-ssms.md)</span></span>
- <span data-ttu-id="d67c4-146">További információ [a Visual Studio Code-hoz való csatlakozásról és a lekérdezésről](sql-database-connect-query-vscode.md)</span><span class="sxs-lookup"><span data-stu-id="d67c4-146">Learn how to [Connect and query with Visual Studio Code](sql-database-connect-query-vscode.md).</span></span>


