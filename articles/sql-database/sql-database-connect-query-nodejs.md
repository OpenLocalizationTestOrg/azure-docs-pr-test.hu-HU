---
title: aaaUse Node.js tooquery Azure SQL Database |} Microsoft Docs
description: "Ez a témakör bemutatja, hogyan toouse Node.js toocreate egy programot, amely kapcsolatot tooan Azure SQL Database és a lekérdezés Transact-SQL utasítás használatával."
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
ms.openlocfilehash: 3870130a486c218eafeb9cf792a4275de7fd6551
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-nodejs-tooquery-an-azure-sql-database"></a><span data-ttu-id="db934-103">Node.js tooquery Azure SQL-adatbázis használata</span><span class="sxs-lookup"><span data-stu-id="db934-103">Use Node.js tooquery an Azure SQL database</span></span>

<span data-ttu-id="db934-104">Gyors üzembe helyezési oktatóanyag bemutatja, hogyan toouse [Node.js](https://nodejs.org/en/) toocreate egy program tooconnect tooan Azure SQL adatbázis- és a Transact-SQL utasítás tooquery adatok felhasználásával.</span><span class="sxs-lookup"><span data-stu-id="db934-104">This quick start tutorial demonstrates how toouse [Node.js](https://nodejs.org/en/) toocreate a program tooconnect tooan Azure SQL database and use Transact-SQL statements tooquery data.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="db934-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="db934-105">Prerequisites</span></span>

<span data-ttu-id="db934-106">toocomplete a gyors üzembe helyezési útmutató, győződjön meg arról, hogy a következő hello:</span><span class="sxs-lookup"><span data-stu-id="db934-106">toocomplete this quick start tutorial, make sure you have hello following:</span></span>

- <span data-ttu-id="db934-107">Azure SQL Database-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="db934-107">An Azure SQL database.</span></span> <span data-ttu-id="db934-108">A gyors üzembe helyezési hoznak létre az egyik a gyors üzembe helyezések hello erőforrást használ:</span><span class="sxs-lookup"><span data-stu-id="db934-108">This quick start uses hello resources created in one of these quick starts:</span></span> 

   - [<span data-ttu-id="db934-109">DB létrehozása – portál</span><span class="sxs-lookup"><span data-stu-id="db934-109">Create DB - Portal</span></span>](sql-database-get-started-portal.md)
   - [<span data-ttu-id="db934-110">DB létrehozása – CLI</span><span class="sxs-lookup"><span data-stu-id="db934-110">Create DB - CLI</span></span>](sql-database-get-started-cli.md)
   - [<span data-ttu-id="db934-111">DB létrehozása – PowerShell</span><span class="sxs-lookup"><span data-stu-id="db934-111">Create DB - PowerShell</span></span>](sql-database-get-started-powershell.md)

- <span data-ttu-id="db934-112">A [kiszolgálószintű tűzfalszabály](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) hello nyilvános IP-cím hello számítógép használja a gyors üzembe helyezési útmutató.</span><span class="sxs-lookup"><span data-stu-id="db934-112">A [server-level firewall rule](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) for hello public IP address of hello computer you use for this quick start tutorial.</span></span>
- <span data-ttu-id="db934-113">Telepítette a Node.js-t és az operációs rendszerének megfelelő kapcsolódó szoftvereket.</span><span class="sxs-lookup"><span data-stu-id="db934-113">You have installed Node.js and related software for your operating system.</span></span>
    - <span data-ttu-id="db934-114">**MacOS**: Homebrew és a Node.js telepítése, és telepítse a hello ODBC-illesztőprogram és az Sqlcmd ESZKÖZRE.</span><span class="sxs-lookup"><span data-stu-id="db934-114">**MacOS**: Install Homebrew and Node.js, and then install hello ODBC driver and SQLCMD.</span></span> <span data-ttu-id="db934-115">Lásd az [1.2 és 1.3 lépést](https://www.microsoft.com/sql-server/developer-get-started/node/mac/).</span><span class="sxs-lookup"><span data-stu-id="db934-115">See [Step 1.2 and 1.3](https://www.microsoft.com/sql-server/developer-get-started/node/mac/).</span></span>
    - <span data-ttu-id="db934-116">**Ubuntu**: telepítse a Node.js, és telepítse a hello ODBC-illesztőprogram és az Sqlcmd ESZKÖZRE.</span><span class="sxs-lookup"><span data-stu-id="db934-116">**Ubuntu**: Install Node.js, and then install hello ODBC driver and SQLCMD.</span></span> <span data-ttu-id="db934-117">Lásd az [1.2 és 1.3 lépést](https://www.microsoft.com/sql-server/developer-get-started/node/ubuntu/).</span><span class="sxs-lookup"><span data-stu-id="db934-117">See [Step 1.2 and 1.3](https://www.microsoft.com/sql-server/developer-get-started/node/ubuntu/) .</span></span>
    - <span data-ttu-id="db934-118">**Windows**: Chocolatey és a Node.js telepítése, és telepítse a hello ODBC-illesztőprogram és az SQL cmd</span><span class="sxs-lookup"><span data-stu-id="db934-118">**Windows**: Install Chocolatey and Node.js, and then install hello ODBC driver and SQL CMD.</span></span> <span data-ttu-id="db934-119">Lásd az [1.2 és 1.3 lépést](https://www.microsoft.com/sql-server/developer-get-started/node/windows/).</span><span class="sxs-lookup"><span data-stu-id="db934-119">See [Step 1.2 and 1.3](https://www.microsoft.com/sql-server/developer-get-started/node/windows/).</span></span>

## <a name="sql-server-connection-information"></a><span data-ttu-id="db934-120">Az SQL-kiszolgáló kapcsolatadatai</span><span class="sxs-lookup"><span data-stu-id="db934-120">SQL server connection information</span></span>

<span data-ttu-id="db934-121">Hello kapcsolat szükséges információkat tooconnect toohello Azure SQL adatbázis beolvasása.</span><span class="sxs-lookup"><span data-stu-id="db934-121">Get hello connection information needed tooconnect toohello Azure SQL database.</span></span> <span data-ttu-id="db934-122">Hello teljes kiszolgálónév, az adatbázisnév és a bejelentkezési adatok a következő eljárások hello kell.</span><span class="sxs-lookup"><span data-stu-id="db934-122">You will need hello fully qualified server name, database name, and login information in hello next procedures.</span></span>

1. <span data-ttu-id="db934-123">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="db934-123">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="db934-124">Válassza ki **SQL-adatbázisok** hello bal oldali menüben kattintson a hello adatbázis **SQL-adatbázisok** lap.</span><span class="sxs-lookup"><span data-stu-id="db934-124">Select **SQL Databases** from hello left-hand menu, and click your database on hello **SQL databases** page.</span></span> 
3. <span data-ttu-id="db934-125">A hello **áttekintése** lap az adatbázishoz, tekintse át hello teljesen minősített kiszolgáló neve, ahogy az a következő kép hello.</span><span class="sxs-lookup"><span data-stu-id="db934-125">On hello **Overview** page for your database, review hello fully qualified server name as shown in hello following image.</span></span> <span data-ttu-id="db934-126">Hello server name toobring hello másolatot is mutat **toocopy kattintson** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="db934-126">You can hover over hello server name toobring up hello **Click toocopy** option.</span></span> 

   ![server-name](./media/sql-database-connect-query-dotnet/server-name.png) 

4. <span data-ttu-id="db934-128">Ha elfelejtette a hello bejelentkezési adatok az Azure SQL Database-kiszolgáló, keresse meg a toohello SQL adatbázis-kiszolgáló lapon tooview hello server admin neve és, ha szükséges, állítsa vissza a hello jelszót.</span><span class="sxs-lookup"><span data-stu-id="db934-128">If you have forgotten hello login information for your Azure SQL Database server, navigate toohello SQL Database server page tooview hello server admin name and, if necessary, reset hello password.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="db934-129">Rendelkeznie kell egy tűzfalszabályt hello nyilvános IP-cím hello számítógép, amelyen ez az oktatóanyag végrehajtása helyen.</span><span class="sxs-lookup"><span data-stu-id="db934-129">You must have a firewall rule in place for hello public IP address of hello computer on which you perform this tutorial.</span></span> <span data-ttu-id="db934-130">Ha egy másik számítógépen vagy egy másik nyilvános IP-címet, hozzon létre egy [kiszolgálószintű tűzfal szabály használatával hello Azure-portálon](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).</span><span class="sxs-lookup"><span data-stu-id="db934-130">If you are on a different computer or have a different public IP address, create a [server-level firewall rule using hello Azure portal](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).</span></span> 

## <a name="create-a-nodejs-project"></a><span data-ttu-id="db934-131">Node.js-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="db934-131">Create a Node.js project</span></span>

<span data-ttu-id="db934-132">Nyisson meg egy parancssort, és hozzon létre egy *sqltest* nevű mappát.</span><span class="sxs-lookup"><span data-stu-id="db934-132">Open a command prompt and create a folder named *sqltest*.</span></span> <span data-ttu-id="db934-133">Keresse meg a létrehozott, és futtassa a következő parancs hello toohello mappába:</span><span class="sxs-lookup"><span data-stu-id="db934-133">Navigate toohello folder you created and run hello following command:</span></span>

    
    npm init -y
    npm install tedious
    npm install async
    

## <a name="insert-code-tooquery-sql-database"></a><span data-ttu-id="db934-134">Helyezze be a kódját tooquery SQL-adatbázis</span><span class="sxs-lookup"><span data-stu-id="db934-134">Insert code tooquery SQL database</span></span>

1. <span data-ttu-id="db934-135">A fejlesztői környezetben vagy egy tetszőleges szövegszerkesztőben hozzon létre egy **sqltest.js** nevű új fájlt.</span><span class="sxs-lookup"><span data-stu-id="db934-135">In your development environment or favorite text editor, create a new file, **sqltest.js**.</span></span>

2. <span data-ttu-id="db934-136">Cserélje ki hello tartalmát hello az alábbi kód, és adja hozzá a megfelelő értékeket hello a kiszolgáló, az adatbázis, a felhasználó és a jelszavát.</span><span class="sxs-lookup"><span data-stu-id="db934-136">Replace hello contents with hello following code and add hello appropriate values for your server, database, user, and password.</span></span>

   ```js
   var Connection = require('tedious').Connection;
   var Request = require('tedious').Request;

   // Create connection toodatabase
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

   // Attempt tooconnect and execute queries if connection goes through
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
      { console.log('Reading rows from hello Table...');

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

## <a name="run-hello-code"></a><span data-ttu-id="db934-137">Hello kód futtatása</span><span class="sxs-lookup"><span data-stu-id="db934-137">Run hello code</span></span>

1. <span data-ttu-id="db934-138">Hello parancssorban futtassa a következő parancsok hello:</span><span class="sxs-lookup"><span data-stu-id="db934-138">At hello command prompt, run hello following commands:</span></span>

   ```js
   node sqltest.js
   ```

2. <span data-ttu-id="db934-139">Győződjön meg arról, hogy hello felső 20 sorokat ad vissza, és zárja be hello alkalmazás ablak.</span><span class="sxs-lookup"><span data-stu-id="db934-139">Verify that hello top 20 rows are returned and then close hello application window.</span></span>

## <a name="next-steps"></a><span data-ttu-id="db934-140">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="db934-140">Next steps</span></span>

- <span data-ttu-id="db934-141">További tudnivalók: hello [Microsoft Node.js Driver for SQL Server](https://docs.microsoft.com/sql/connect/node-js/node-js-driver-for-sql-server/)</span><span class="sxs-lookup"><span data-stu-id="db934-141">Learn about hello [Microsoft Node.js Driver for SQL Server](https://docs.microsoft.com/sql/connect/node-js/node-js-driver-for-sql-server/)</span></span>
- <span data-ttu-id="db934-142">Ismerje meg, hogyan túl[kapcsolódás és lekérdezés az Azure SQL-adatbázis használata a .NET core](sql-database-connect-query-dotnet-core.md) a Windows/Linux/macOS.</span><span class="sxs-lookup"><span data-stu-id="db934-142">Learn how too[connect and query an Azure SQL database using .NET core](sql-database-connect-query-dotnet-core.md) on Windows/Linux/macOS.</span></span>  
- <span data-ttu-id="db934-143">További tudnivalók [Ismerkedés a Windows/Linux/macOS hello parancssor használatával a .NET Core](/dotnet/core/tutorials/using-with-xplat-cli).</span><span class="sxs-lookup"><span data-stu-id="db934-143">Learn about [Getting started with .NET Core on Windows/Linux/macOS using hello command line](/dotnet/core/tutorials/using-with-xplat-cli).</span></span>
- <span data-ttu-id="db934-144">Ismerje meg, hogyan túl[tervezése az első Azure SQL-adatbázis SSMS használatával](sql-database-design-first-database.md) vagy [tervezése az első Azure SQL Database adatbázishoz .NET használatával](sql-database-design-first-database-csharp.md).</span><span class="sxs-lookup"><span data-stu-id="db934-144">Learn how too[Design your first Azure SQL database using SSMS](sql-database-design-first-database.md) or [Design your first Azure SQL database using .NET](sql-database-design-first-database-csharp.md).</span></span>
- <span data-ttu-id="db934-145">Ismerje meg, hogyan túl[kapcsolódás és lekérdezés ssms alkalmazásával](sql-database-connect-query-ssms.md)</span><span class="sxs-lookup"><span data-stu-id="db934-145">Learn how too[Connect and query with SSMS](sql-database-connect-query-ssms.md)</span></span>
- <span data-ttu-id="db934-146">Ismerje meg, hogyan túl[kapcsolódás és lekérdezés Visual Studio Code](sql-database-connect-query-vscode.md).</span><span class="sxs-lookup"><span data-stu-id="db934-146">Learn how too[Connect and query with Visual Studio Code](sql-database-connect-query-vscode.md).</span></span>


