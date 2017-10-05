---
title: "SSMS: Csatlakozás és adatok lekérdezése Azure SQL Database adatbázisokból | Microsoft Docs"
description: "Ebből a cikkből megtudhatja, hogyan csatlakozhat az SQL Database-hez az Azure-ban az SQL Server Management Studio (SSMS) használatával. Ezután futtasson Transact-SQL (T-SQL) utasításokat az adatok lekérdezéséhez és szerkesztéséhez."
metacanonical: 
keywords: "csatlakozás sql database-hez,sql server management studio"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 7cd2a114-c13c-4ace-9088-97bd9d68de12
ms.service: sql-database
ms.custom: mvc,DBs & servers
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 05/26/2017
ms.author: carlrab
ms.openlocfilehash: 2835a72fc90d1fd39af73c6907648908e5d9fdeb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-sql-database-use-sql-server-management-studio-to-connect-and-query-data"></a><span data-ttu-id="bb78a-105">Azure SQL Database: Az SQL Server Management Studio segítségével csatlakozhat és kérdezhet le adatokat</span><span class="sxs-lookup"><span data-stu-id="bb78a-105">Azure SQL Database: Use SQL Server Management Studio to connect and query data</span></span>

<span data-ttu-id="bb78a-106">Az [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx) (SSMS) bármely SQL-infrastruktúra kezelésére alkalmas integrált környezet az SQL Servertől az SQL Database for Microsoft Windowsig.</span><span class="sxs-lookup"><span data-stu-id="bb78a-106">[SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx) (SSMS) is an integrated environment for managing any SQL infrastructure, from SQL Server to SQL Database for Microsoft Windows.</span></span> <span data-ttu-id="bb78a-107">Ez a gyors üzembehelyezési útmutató ismerteti, hogyan használható az SSMS egy Azure SQL Database-adatbázishoz való csatlakozáshoz, majd hogyan lehet Transact-SQL-utasítások használatával adatokat lekérdezni, beszúrni, frissíteni és törölni az adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="bb78a-107">This quick start demonstrates how to use SSMS to connect to an Azure SQL database, and then use Transact-SQL statements to query, insert, update, and delete data in the database.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="bb78a-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="bb78a-108">Prerequisites</span></span>

<span data-ttu-id="bb78a-109">Ez a rövid útmutató az alábbi rövid útmutatók egyikében létrehozott erőforrásokat használja kiindulási pontnak:</span><span class="sxs-lookup"><span data-stu-id="bb78a-109">This quick start uses as its starting point the resources created in one of these quick starts:</span></span>

- [<span data-ttu-id="bb78a-110">DB létrehozása – portál</span><span class="sxs-lookup"><span data-stu-id="bb78a-110">Create DB - Portal</span></span>](sql-database-get-started-portal.md)
- [<span data-ttu-id="bb78a-111">DB létrehozása – CLI</span><span class="sxs-lookup"><span data-stu-id="bb78a-111">Create DB - CLI</span></span>](sql-database-get-started-cli.md)
- [<span data-ttu-id="bb78a-112">DB létrehozása – PowerShell</span><span class="sxs-lookup"><span data-stu-id="bb78a-112">Create DB - PowerShell</span></span>](sql-database-get-started-powershell.md)

<span data-ttu-id="bb78a-113">A kezdés előtt győződjön meg arról, hogy az [SSMS](https://msdn.microsoft.com/library/mt238290.aspx) legújabb verziója van telepítve.</span><span class="sxs-lookup"><span data-stu-id="bb78a-113">Before you start, make sure you have installed the newest version of [SSMS](https://msdn.microsoft.com/library/mt238290.aspx).</span></span> 

## <a name="sql-server-connection-information"></a><span data-ttu-id="bb78a-114">Az SQL-kiszolgáló kapcsolatadatai</span><span class="sxs-lookup"><span data-stu-id="bb78a-114">SQL server connection information</span></span>

<span data-ttu-id="bb78a-115">Kérje le az Azure SQL-adatbázishoz való csatlakozáshoz szükséges kapcsolatadatokat.</span><span class="sxs-lookup"><span data-stu-id="bb78a-115">Get the connection information needed to connect to the Azure SQL database.</span></span> <span data-ttu-id="bb78a-116">A későbbi eljárásokban szüksége lesz a teljes kiszolgálónévre, az adatbázis nevére és a bejelentkezési adatokra.</span><span class="sxs-lookup"><span data-stu-id="bb78a-116">You will need the fully qualified server name, database name, and login information in the next procedures.</span></span>

1. <span data-ttu-id="bb78a-117">Jelentkezzen be az [Azure portálra](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="bb78a-117">Log in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="bb78a-118">Válassza az **SQL-adatbázisok** elemet a bal oldali menüben, majd kattintson az új adatbázisra az **SQL-adatbázisok** oldalon.</span><span class="sxs-lookup"><span data-stu-id="bb78a-118">Select **SQL Databases** from the left-hand menu, and click your database on the **SQL databases** page.</span></span> 
3. <span data-ttu-id="bb78a-119">Az adatbázis **Áttekintés** lapján tekintse meg a teljes kiszolgálónevet, amint az alábbi képen is látható.</span><span class="sxs-lookup"><span data-stu-id="bb78a-119">On the **Overview** page for your database, review the fully qualified server name as shown in the image below.</span></span> <span data-ttu-id="bb78a-120">Ha a mutatót a kiszolgáló neve fölé viszi, megjelenik a **Kattintson a másoláshoz** lehetőség.</span><span class="sxs-lookup"><span data-stu-id="bb78a-120">You can hover over the server name to bring up the **Click to copy** option.</span></span>

   ![kapcsolatadatok](./media/sql-database-connect-query-dotnet/server-name.png) 

4. <span data-ttu-id="bb78a-122">Amennyiben elfelejtette Azure SQL Database-kiszolgálója bejelentkezési adatait, lépjen az SQL Database-kiszolgáló oldalára, és itt megtudhatja a kiszolgáló rendszergazdájának nevét, valamint szükség esetén visszaállíthatja a jelszót.</span><span class="sxs-lookup"><span data-stu-id="bb78a-122">If you have forgotten the login information for your Azure SQL Database server, navigate to the SQL Database server page to view the server admin name and, if necessary, reset the password.</span></span> 

## <a name="connect-to-your-database"></a><span data-ttu-id="bb78a-123">Csatlakozás az adatbázishoz</span><span class="sxs-lookup"><span data-stu-id="bb78a-123">Connect to your database</span></span>

<span data-ttu-id="bb78a-124">Az SQL Server Management Studióban építse fel a kapcsolatot az Azure SQL Database kiszolgálóval.</span><span class="sxs-lookup"><span data-stu-id="bb78a-124">Use SQL Server Management Studio to establish a connection to your Azure SQL Database server.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="bb78a-125">Egy Azure SQL Database logikai kiszolgáló figyel az 1433-as porton.</span><span class="sxs-lookup"><span data-stu-id="bb78a-125">An Azure SQL Database logical server listens on port 1433.</span></span> <span data-ttu-id="bb78a-126">Ha vállalati tűzfalon belülről szeretne csatlakozni egy Azure SQL Database logikai kiszolgálóhoz, ennek a portnak nyitva kell lennie a vállalati tűzfalon a sikeres csatlakozáshoz.</span><span class="sxs-lookup"><span data-stu-id="bb78a-126">If you are attempting to connect to an Azure SQL Database logical server from within a corporate firewall, this port must be open in the corporate firewall for you to successfully connect.</span></span>
>

1. <span data-ttu-id="bb78a-127">Nyissa meg az SQL Server Management Studiót.</span><span class="sxs-lookup"><span data-stu-id="bb78a-127">Open SQL Server Management Studio.</span></span>

2. <span data-ttu-id="bb78a-128">A **Connect to Server** (Kapcsolódás a kiszolgálóhoz) párbeszédpanelen adja meg a következő adatokat:</span><span class="sxs-lookup"><span data-stu-id="bb78a-128">In the **Connect to Server** dialog box, enter the following information:</span></span>

   | <span data-ttu-id="bb78a-129">Beállítás</span><span class="sxs-lookup"><span data-stu-id="bb78a-129">Setting</span></span>       | <span data-ttu-id="bb78a-130">Ajánlott érték</span><span class="sxs-lookup"><span data-stu-id="bb78a-130">Suggested value</span></span> | <span data-ttu-id="bb78a-131">Leírás</span><span class="sxs-lookup"><span data-stu-id="bb78a-131">Description</span></span> | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | <span data-ttu-id="bb78a-132">**Kiszolgáló típusa**</span><span class="sxs-lookup"><span data-stu-id="bb78a-132">**Server type**</span></span> | <span data-ttu-id="bb78a-133">Adatbázismotor</span><span class="sxs-lookup"><span data-stu-id="bb78a-133">Database engine</span></span> | <span data-ttu-id="bb78a-134">Kötelezően megadandó érték.</span><span class="sxs-lookup"><span data-stu-id="bb78a-134">This value is required.</span></span> |
   | <span data-ttu-id="bb78a-135">**Kiszolgálónév**</span><span class="sxs-lookup"><span data-stu-id="bb78a-135">**Server name**</span></span> | <span data-ttu-id="bb78a-136">A teljes kiszolgálónév</span><span class="sxs-lookup"><span data-stu-id="bb78a-136">The fully qualified server name</span></span> | <span data-ttu-id="bb78a-137">A névnek a következőhöz hasonlónak kell lennie: **mynewserver20170313.database.windows.net**.</span><span class="sxs-lookup"><span data-stu-id="bb78a-137">The name should be something like this: **mynewserver20170313.database.windows.net**.</span></span> |
   | <span data-ttu-id="bb78a-138">**Hitelesítés**</span><span class="sxs-lookup"><span data-stu-id="bb78a-138">**Authentication**</span></span> | <span data-ttu-id="bb78a-139">SQL Server-hitelesítés</span><span class="sxs-lookup"><span data-stu-id="bb78a-139">SQL Server Authentication</span></span> | <span data-ttu-id="bb78a-140">Ebben az oktatóanyagban az SQL-hitelesítésen kívül más hitelesítéstípus nincs konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="bb78a-140">SQL Authentication is the only authentication type that we have configured in this tutorial.</span></span> |
   | <span data-ttu-id="bb78a-141">**Bejelentkezés**</span><span class="sxs-lookup"><span data-stu-id="bb78a-141">**Login**</span></span> | <span data-ttu-id="bb78a-142">A kiszolgálói rendszergazdafiók</span><span class="sxs-lookup"><span data-stu-id="bb78a-142">The server admin account</span></span> | <span data-ttu-id="bb78a-143">Ezt a fiókot adta meg a kiszolgáló létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="bb78a-143">This is the account that you specified when you created the server.</span></span> |
   | <span data-ttu-id="bb78a-144">**Jelszó**</span><span class="sxs-lookup"><span data-stu-id="bb78a-144">**Password**</span></span> | <span data-ttu-id="bb78a-145">A kiszolgálói rendszergazdafiók jelszava</span><span class="sxs-lookup"><span data-stu-id="bb78a-145">The password for your server admin account</span></span> | <span data-ttu-id="bb78a-146">Ezt a jelszót adta meg a kiszolgáló létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="bb78a-146">This is the password that you specified when you created the server.</span></span> |

   ![kapcsolódás a kiszolgálóhoz](./media/sql-database-connect-query-ssms/connect.png)  

3. <span data-ttu-id="bb78a-148">A **Connect to server** (Kapcsolódás a kiszolgálóhoz) párbeszédpanelen kattintson az **Options** (Beállítások) elemre.</span><span class="sxs-lookup"><span data-stu-id="bb78a-148">Click **Options** in the **Connect to server** dialog box.</span></span> <span data-ttu-id="bb78a-149">A **Connect to database** (Csatlakozás az adatbázishoz) szakaszban adja meg a következőt: **mySampleDatabase** az adatbázishoz való csatlakozáshoz.</span><span class="sxs-lookup"><span data-stu-id="bb78a-149">In the **Connect to database** section, enter **mySampleDatabase** to connect to this database.</span></span>

   ![csatlakozás kiszolgálón található adatbázishoz](./media/sql-database-connect-query-ssms/options-connect-to-db.png)  

4. <span data-ttu-id="bb78a-151">Kattintson a **Connect** (Csatlakozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="bb78a-151">Click **Connect**.</span></span> <span data-ttu-id="bb78a-152">Megnyílik az Object Explorer ablak az SSMS-ben.</span><span class="sxs-lookup"><span data-stu-id="bb78a-152">The Object Explorer window opens in SSMS.</span></span> 

   ![csatlakoztatva a kiszolgálóhoz](./media/sql-database-connect-query-ssms/connected.png)  

5. <span data-ttu-id="bb78a-154">Az Object Explorerben bontsa ki a **Database** (Adatbázisok), majd a **mySampleDatabese** csomópontot a mintaadatbázisban található objektumok megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="bb78a-154">In Object Explorer, expand **Databases** and then expand **mySampleDatabase** to view the objects in the sample database.</span></span>

## <a name="query-data"></a><span data-ttu-id="bb78a-155">Adatok lekérdezése</span><span class="sxs-lookup"><span data-stu-id="bb78a-155">Query data</span></span>

<span data-ttu-id="bb78a-156">A következő kód használatával lekérdezheti kategóriánként az első 20 terméket a [SELECT](https://msdn.microsoft.com/library/ms189499.aspx) Transact-SQL-utasítással.</span><span class="sxs-lookup"><span data-stu-id="bb78a-156">Use the following code to query for the top 20 products by category using the [SELECT](https://msdn.microsoft.com/library/ms189499.aspx) Transact-SQL statement.</span></span>

1. <span data-ttu-id="bb78a-157">Az Object Explorerben kattintson a jobb gombbal a **mySampleDatabase** adatbázisra, majd kattintson a **New Query** (Új lekérdezés) elemre.</span><span class="sxs-lookup"><span data-stu-id="bb78a-157">In Object Explorer, right-click **mySampleDatabase** and click **New Query**.</span></span> <span data-ttu-id="bb78a-158">Megnyílik egy, az adatbázishoz csatlakoztatott üres lekérdezési ablak.</span><span class="sxs-lookup"><span data-stu-id="bb78a-158">A blank query window opens that is connected to your database.</span></span>
2. <span data-ttu-id="bb78a-159">A lekérdezési ablakban írja be a következő lekérdezést:</span><span class="sxs-lookup"><span data-stu-id="bb78a-159">In the query window, enter the following query:</span></span>

   ```sql
   SELECT pc.Name as CategoryName, p.name as ProductName
   FROM [SalesLT].[ProductCategory] pc
   JOIN [SalesLT].[Product] p
   ON pc.productcategoryid = p.productcategoryid;
   ```

3. <span data-ttu-id="bb78a-160">Az eszköztáron kattintson az **Execute** (Végrehajtás) elemre a Product és ProductCategory táblák adatainak lekérdezéséhez.</span><span class="sxs-lookup"><span data-stu-id="bb78a-160">On the toolbar, click **Execute** to retrieve data from the Product and ProductCategory tables.</span></span>

    ![lekérdezés](./media/sql-database-connect-query-ssms/query.png)

## <a name="insert-data"></a><span data-ttu-id="bb78a-162">Adat beszúrása</span><span class="sxs-lookup"><span data-stu-id="bb78a-162">Insert data</span></span>

<span data-ttu-id="bb78a-163">A következő kód használatával beszúrhat egy új terméket a SalesLT.Product táblába az [INSERT](https://msdn.microsoft.com/library/ms174335.aspx) Transact-SQL-utasítással.</span><span class="sxs-lookup"><span data-stu-id="bb78a-163">Use the following code to insert a new product into the SalesLT.Product table using the [INSERT](https://msdn.microsoft.com/library/ms174335.aspx) Transact-SQL statement.</span></span>

1. <span data-ttu-id="bb78a-164">A lekérdezési ablakban cserélje le az előző lekérdezést a következő lekérdezésre:</span><span class="sxs-lookup"><span data-stu-id="bb78a-164">In the query window, replace the previous query with the following query:</span></span>

   ```sql
   INSERT INTO [SalesLT].[Product]
           ( [Name]
           , [ProductNumber]
           , [Color]
           , [ProductCategoryID]
           , [StandardCost]
           , [ListPrice]
           , [SellStartDate]
           )
     VALUES
           ('myNewProduct'
           ,123456789
           ,'NewColor'
           ,1
           ,100
           ,100
           ,GETDATE() );
   ```

2. <span data-ttu-id="bb78a-165">Az eszköztáron kattintson az **Execute** (Végrehajtás) elemre az új sor Product táblába történő beszúrásához.</span><span class="sxs-lookup"><span data-stu-id="bb78a-165">On the toolbar, click **Execute**  to insert a new row in the Product table.</span></span>

    <img src="./media/sql-database-connect-query-ssms/insert.png" alt="insert" style="width: 780px;" />

## <a name="update-data"></a><span data-ttu-id="bb78a-166">Adatok frissítése</span><span class="sxs-lookup"><span data-stu-id="bb78a-166">Update data</span></span>

<span data-ttu-id="bb78a-167">A következő kód használatával frissítheti az előzőleg hozzáadott új terméket az [UPDATE](https://msdn.microsoft.com/library/ms177523.aspx) Transact-SQL-utasítással.</span><span class="sxs-lookup"><span data-stu-id="bb78a-167">Use the following code to update the new product that you previously added using the [UPDATE](https://msdn.microsoft.com/library/ms177523.aspx) Transact-SQL statement.</span></span>

1. <span data-ttu-id="bb78a-168">A lekérdezési ablakban cserélje le az előző lekérdezést a következő lekérdezésre:</span><span class="sxs-lookup"><span data-stu-id="bb78a-168">In the query window, replace the previous query with the following query:</span></span>

   ```sql
   UPDATE [SalesLT].[Product]
   SET [ListPrice] = 125
   WHERE Name = 'myNewProduct';
   ```

2. <span data-ttu-id="bb78a-169">Az eszköztáron kattintson az **Execute** (Végrehajtás) elemre a Product tábla egy megadott sorának frissítéséhez.</span><span class="sxs-lookup"><span data-stu-id="bb78a-169">On the toolbar, click **Execute** to update the specified row in the Product table.</span></span>

    <img src="./media/sql-database-connect-query-ssms/update.png" alt="update" style="width: 780px;" />

## <a name="delete-data"></a><span data-ttu-id="bb78a-170">Adat törlése</span><span class="sxs-lookup"><span data-stu-id="bb78a-170">Delete data</span></span>

<span data-ttu-id="bb78a-171">A következő kód használatával törölheti az előzőleg hozzáadott új terméket a [DELETE](https://msdn.microsoft.com/library/ms189835.aspx) Transact-SQL-utasítással.</span><span class="sxs-lookup"><span data-stu-id="bb78a-171">Use the following code to delete the new product that you previously added using the [DELETE](https://msdn.microsoft.com/library/ms189835.aspx) Transact-SQL statement.</span></span>

1. <span data-ttu-id="bb78a-172">A lekérdezési ablakban cserélje le az előző lekérdezést a következő lekérdezésre:</span><span class="sxs-lookup"><span data-stu-id="bb78a-172">In the query window, replace the previous query with the following query:</span></span>

   ```sql
   DELETE FROM [SalesLT].[Product]
   WHERE Name = 'myNewProduct';
   ```

2. <span data-ttu-id="bb78a-173">Az eszköztáron kattintson az **Execute** (Végrehajtás) elemre a Product tábla egy megadott sorának törléséhez.</span><span class="sxs-lookup"><span data-stu-id="bb78a-173">On the toolbar, click **Execute** to delete the specified row in the Product table.</span></span>

    <img src="./media/sql-database-connect-query-ssms/delete.png" alt="delete" style="width: 780px;" />

## <a name="next-steps"></a><span data-ttu-id="bb78a-174">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bb78a-174">Next steps</span></span>

- <span data-ttu-id="bb78a-175">Kiszolgálók és adatbázisok Transact-SQL-lel történő létrehozásáról és kezeléséről az [Azure SQL Database-kiszolgálók és -adatbázisok megismerése](sql-database-servers-databases.md) című cikkből tájékozódhat.</span><span class="sxs-lookup"><span data-stu-id="bb78a-175">To learn about creating and managing servers and databases with Transact-SQL, see [Learn about Azure SQL Database servers and databases](sql-database-servers-databases.md).</span></span>
- <span data-ttu-id="bb78a-176">Az SSMS eszközről további információt [az SQL Server Management Studio használatát ismertető cikkben talál](https://msdn.microsoft.com/library/ms174173.aspx).</span><span class="sxs-lookup"><span data-stu-id="bb78a-176">For information about SSMS, see [Use SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx).</span></span>
- <span data-ttu-id="bb78a-177">A Visual Studio Code használatával történő csatlakozásról és lekérdezésről lásd a [Visual Studio Code használatával végzett csatlakozásról és lekérdezésről](sql-database-connect-query-vscode.md) szóló témakört.</span><span class="sxs-lookup"><span data-stu-id="bb78a-177">To connect and query using Visual Studio Code, see [Connect and query with Visual Studio Code](sql-database-connect-query-vscode.md).</span></span>
- <span data-ttu-id="bb78a-178">A .NET használatával történő csatlakozásról és lekérdezésről lásd a [.NET használatával végzett csatlakozásról és lekérdezésről](sql-database-connect-query-dotnet.md) szóló témakört.</span><span class="sxs-lookup"><span data-stu-id="bb78a-178">To connect and query using .NET, see [Connect and query with .NET](sql-database-connect-query-dotnet.md).</span></span>
- <span data-ttu-id="bb78a-179">A PHP-vel történő csatlakozásról és lekérdezésről lásd a [PHP-vel végzett csatlakozásról és lekérdezésről](sql-database-connect-query-php.md) szóló témakört.</span><span class="sxs-lookup"><span data-stu-id="bb78a-179">To connect and query using PHP, see [Connect and query with PHP](sql-database-connect-query-php.md).</span></span>
- <span data-ttu-id="bb78a-180">A Node.js használatával történő csatlakozásról és lekérdezésről lásd a [Node.js használatával végzett csatlakozásról és lekérdezésről](sql-database-connect-query-nodejs.md) szóló témakört.</span><span class="sxs-lookup"><span data-stu-id="bb78a-180">To connect and query using Node.js, see [Connect and query with Node.js](sql-database-connect-query-nodejs.md).</span></span>
- <span data-ttu-id="bb78a-181">A Java használatával történő csatlakozásról és lekérdezésről lásd a [Java használatával végzett csatlakozásról és lekérdezésről](sql-database-connect-query-java.md) szóló témakört.</span><span class="sxs-lookup"><span data-stu-id="bb78a-181">To connect and query using Java, see [Connect and query with Java](sql-database-connect-query-java.md).</span></span>
- <span data-ttu-id="bb78a-182">A Python használatával történő csatlakozásról és lekérdezésről lásd a [Python használatával végzett csatlakozásról és lekérdezésről](sql-database-connect-query-python.md) szóló témakört.</span><span class="sxs-lookup"><span data-stu-id="bb78a-182">To connect and query using Python, see [Connect and query with Python](sql-database-connect-query-python.md).</span></span>
- <span data-ttu-id="bb78a-183">A Ruby használatával történő csatlakozásról és lekérdezésről lásd a [Ruby használatával végzett csatlakozásról és lekérdezésről](sql-database-connect-query-ruby.md) szóló témakört.</span><span class="sxs-lookup"><span data-stu-id="bb78a-183">To connect and query using Ruby, see [Connect and query with Ruby](sql-database-connect-query-ruby.md).</span></span>
