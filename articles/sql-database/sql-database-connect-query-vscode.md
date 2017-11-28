---
title: "VS Code: Csatlakozás és adatok lekérdezése Azure SQL Database-adatbázisokból | Microsoft Docs"
description: "Ebből a cikkből megtudhatja, hogyan csatlakozhat az SQL Database-hez az Azure-ban a Visual Studio Code segítségével. Ezután futtasson Transact-SQL (T-SQL) utasításokat az adatok lekérdezéséhez és szerkesztéséhez."
metacanonical: 
keywords: "csatlakozás SQL Database-adatbázishoz"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 676bd799-a571-4bb8-848b-fb1720007866
ms.service: sql-database
ms.custom: mvc,DBs & servers
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 06/20/2017
ms.author: carlrab
ms.openlocfilehash: 4076b1e7ab3a70009217a1deff72da4bff0dc871
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-sql-database-use-visual-studio-code-to-connect-and-query-data"></a><span data-ttu-id="f9925-105">Azure SQL Database: Csatlakozás, majd adatok lekérdezése a Visual Studio Code használatával</span><span class="sxs-lookup"><span data-stu-id="f9925-105">Azure SQL Database: Use Visual Studio Code to connect and query data</span></span>

<span data-ttu-id="f9925-106">A [Visual Studio Code](https://code.visualstudio.com/docs) egy grafikus kódszerkesztő Linux, macOS és Windows rendszerekre, amely támogatja a bővítményeket, beleértve az [mssql bővítményt](https://aka.ms/mssql-marketplace) a Microsoft SQL Server, az Azure SQL Database és az SQL Data Warehouse lekérdezéséhez.</span><span class="sxs-lookup"><span data-stu-id="f9925-106">[Visual Studio Code](https://code.visualstudio.com/docs) is a graphical code editor for Linux, macOS, and Windows that supports extensions, including the [mssql extension](https://aka.ms/mssql-marketplace) for querying Microsoft SQL Server, Azure SQL Database, and SQL Data Warehouse.</span></span> <span data-ttu-id="f9925-107">Ez a gyors üzembehelyezési útmutató ismerteti, hogyan használható a Visual Studio Code egy Azure SQL Database-adatbázishoz való csatlakozáshoz, és hogyan lehet Transact-SQL-utasításokkal adatokat lekérdezni, beszúrni, frissíteni és törölni az adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="f9925-107">This quick start demonstrates how to use Visual Studio Code to connect to an Azure SQL database, and then use Transact-SQL statements to query, insert, update, and delete data in the database.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f9925-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="f9925-108">Prerequisites</span></span>

<span data-ttu-id="f9925-109">Ez a rövid útmutató az alábbi rövid útmutatók egyikében létrehozott erőforrásokat használja kiindulási pontnak:</span><span class="sxs-lookup"><span data-stu-id="f9925-109">This quick start uses as its starting point the resources created in one of these quick starts:</span></span>

- [<span data-ttu-id="f9925-110">DB létrehozása – portál</span><span class="sxs-lookup"><span data-stu-id="f9925-110">Create DB - Portal</span></span>](sql-database-get-started-portal.md)
- [<span data-ttu-id="f9925-111">DB létrehozása – CLI</span><span class="sxs-lookup"><span data-stu-id="f9925-111">Create DB - CLI</span></span>](sql-database-get-started-cli.md)
- [<span data-ttu-id="f9925-112">DB létrehozása – PowerShell</span><span class="sxs-lookup"><span data-stu-id="f9925-112">Create DB - PowerShell</span></span>](sql-database-get-started-powershell.md)

<span data-ttu-id="f9925-113">A kezdés előtt győződjön meg arról, hogy a [Visual Studio Code](https://code.visualstudio.com/Download) legújabb verziója van telepítve, és be van töltve az [mssql bővítmény](https://aka.ms/mssql-marketplace).</span><span class="sxs-lookup"><span data-stu-id="f9925-113">Before you start, make sure you have installed the newest version of [Visual Studio Code](https://code.visualstudio.com/Download) and loaded the [mssql extension](https://aka.ms/mssql-marketplace).</span></span> <span data-ttu-id="f9925-114">Az mssql bővítmény telepítési lépéseinek megismeréséhez olvassa el [a VS Code telepítését](https://docs.microsoft.com/sql/linux/sql-server-linux-develop-use-vscode#install-vs-code) és a [Visual Studio Code-hoz használható mssql-t](https://marketplace.visualstudio.com/items?itemName=ms-mssql.mssql) ismertető cikkeket.</span><span class="sxs-lookup"><span data-stu-id="f9925-114">For installation guidance for the mssql extension, see [Install VS Code](https://docs.microsoft.com/sql/linux/sql-server-linux-develop-use-vscode#install-vs-code) and see [mssql for Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-mssql.mssql).</span></span> 

## <a name="configure-vs-code"></a><span data-ttu-id="f9925-115">A VS Code konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f9925-115">Configure VS Code</span></span> 

### <a name="mac-os"></a><span data-ttu-id="f9925-116">**Mac OS**</span><span class="sxs-lookup"><span data-stu-id="f9925-116">**Mac OS**</span></span>
<span data-ttu-id="f9925-117">Mac OS esetén telepíteni kell az OpenSSL-t, amely az mssql bővítmény által használt DotNet Core előfeltétele.</span><span class="sxs-lookup"><span data-stu-id="f9925-117">For macOS, you need to install OpenSSL which is a prerequiste for DotNet Core that mssql extention uses.</span></span> <span data-ttu-id="f9925-118">Nyissa meg a terminált, és adja meg az alábbi parancsokat a **brew** és az **OpenSSL** telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="f9925-118">Open your terminal and enter the following commands to install **brew** and **OpenSSL**.</span></span> 

```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew update
brew install openssl
mkdir -p /usr/local/lib
ln -s /usr/local/opt/openssl/lib/libcrypto.1.0.0.dylib /usr/local/lib/
ln -s /usr/local/opt/openssl/lib/libssl.1.0.0.dylib /usr/local/lib/
```

### <a name="linux-ubuntu"></a><span data-ttu-id="f9925-119">**Linux (Ubuntu)**</span><span class="sxs-lookup"><span data-stu-id="f9925-119">**Linux (Ubuntu)**</span></span>

<span data-ttu-id="f9925-120">Nincs szükség különleges konfigurációra.</span><span class="sxs-lookup"><span data-stu-id="f9925-120">No special configuration needed.</span></span>

### <a name="windows"></a><span data-ttu-id="f9925-121">**Windows**</span><span class="sxs-lookup"><span data-stu-id="f9925-121">**Windows**</span></span>

<span data-ttu-id="f9925-122">Nincs szükség különleges konfigurációra.</span><span class="sxs-lookup"><span data-stu-id="f9925-122">No special configuration needed.</span></span>

## <a name="sql-server-connection-information"></a><span data-ttu-id="f9925-123">Az SQL-kiszolgáló kapcsolatadatai</span><span class="sxs-lookup"><span data-stu-id="f9925-123">SQL server connection information</span></span>

<span data-ttu-id="f9925-124">Kérje le az Azure SQL-adatbázishoz való csatlakozáshoz szükséges kapcsolatadatokat.</span><span class="sxs-lookup"><span data-stu-id="f9925-124">Get the connection information needed to connect to the Azure SQL database.</span></span> <span data-ttu-id="f9925-125">A későbbi eljárásokban szüksége lesz a teljes kiszolgálónévre, az adatbázis nevére és a bejelentkezési adatokra.</span><span class="sxs-lookup"><span data-stu-id="f9925-125">You will need the fully qualified server name, database name, and login information in the next procedures.</span></span>

1. <span data-ttu-id="f9925-126">Jelentkezzen be az [Azure portálra](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="f9925-126">Log in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="f9925-127">Válassza az **SQL-adatbázisok** elemet a bal oldali menüben, majd kattintson az új adatbázisra az **SQL-adatbázisok** oldalon.</span><span class="sxs-lookup"><span data-stu-id="f9925-127">Select **SQL Databases** from the left-hand menu, and click your database on the **SQL databases** page.</span></span> 
3. <span data-ttu-id="f9925-128">Az adatbázis **Áttekintés** oldalán tekintse meg a teljes kiszolgálónevet, amint az az alábbi képen látható.</span><span class="sxs-lookup"><span data-stu-id="f9925-128">On the **Overview** page for your database, review the fully qualified server name as shown in the following image.</span></span> <span data-ttu-id="f9925-129">Ha a mutatót a kiszolgáló neve fölé viszi, megjelenik a **Kattintson a másoláshoz** lehetőség.</span><span class="sxs-lookup"><span data-stu-id="f9925-129">You can hover over the server name to bring up the **Click to copy** option.</span></span>

   ![kapcsolatadatok](./media/sql-database-connect-query-dotnet/server-name.png) 

4. <span data-ttu-id="f9925-131">Amennyiben elfelejtette Azure SQL Database-kiszolgálója bejelentkezési adatait, lépjen az SQL Database-kiszolgáló oldalára, és itt megtudhatja a kiszolgáló rendszergazdájának nevét, valamint szükség esetén visszaállíthatja a jelszót.</span><span class="sxs-lookup"><span data-stu-id="f9925-131">If you have forgotten the login information for your Azure SQL Database server, navigate to the SQL Database server page to view the server admin name and, if necessary, reset the password.</span></span> 

## <a name="set-language-mode-to-sql"></a><span data-ttu-id="f9925-132">A nyelvmód SQL értékre állítása</span><span class="sxs-lookup"><span data-stu-id="f9925-132">Set language mode to SQL</span></span>

<span data-ttu-id="f9925-133">Az mssql-parancsok és a T-SQL IntelliSense engedélyezéséhez ügyeljen arra, hogy a nyelvmód beállítása **SQL** legyen a Visual Studio Code-ban.</span><span class="sxs-lookup"><span data-stu-id="f9925-133">Set the language mode is set to **SQL** in Visual Studio Code to enable mssql commands and T-SQL IntelliSense.</span></span>

1. <span data-ttu-id="f9925-134">Nyisson meg egy új Visual Studio Code-ablakot.</span><span class="sxs-lookup"><span data-stu-id="f9925-134">Open a new Visual Studio Code window.</span></span> 

2. <span data-ttu-id="f9925-135">Kattintson az **Egyszerű szöveg** elemre az állapotsor jobb alsó sarkában.</span><span class="sxs-lookup"><span data-stu-id="f9925-135">Click **Plain Text** in the lower right-hand corner of the status bar.</span></span>
3. <span data-ttu-id="f9925-136">A megnyíló **Nyelvmód kiválasztása** legördülő menübe írja be az **SQL** kifejezést, majd nyomja le az **ENTER** billentyűt a nyelvmód SQL értékre állításához.</span><span class="sxs-lookup"><span data-stu-id="f9925-136">In the **Select language mode** drop-down menu that opens, type **SQL**, and then press **ENTER** to set the language mode to SQL.</span></span> 

   ![SQL nyelvmód](./media/sql-database-connect-query-vscode/vscode-language-mode.png)

## <a name="connect-to-your-database"></a><span data-ttu-id="f9925-138">Csatlakozás az adatbázishoz</span><span class="sxs-lookup"><span data-stu-id="f9925-138">Connect to your database</span></span>

<span data-ttu-id="f9925-139">A Visual Studio Code segítségével kapcsolatot hozhat létre az Azure SQL Database-kiszolgálóval.</span><span class="sxs-lookup"><span data-stu-id="f9925-139">Use Visual Studio Code to establish a connection to your Azure SQL Database server.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f9925-140">A folytatás előtt győződjön meg arról, hogy rendelkezésére állnak a kiszolgálóval és az adatbázissal kapcsolatos, valamint a bejelentkezési adatok.</span><span class="sxs-lookup"><span data-stu-id="f9925-140">Before continuing, make sure that you have your server, database, and login information ready.</span></span> <span data-ttu-id="f9925-141">Ha elkezdi beírni a csatlakozási profil információit, és áthelyezi a fókuszt a Visual Studio Code-ról, újra kell kezdenie a csatlakozási profil létrehozását.</span><span class="sxs-lookup"><span data-stu-id="f9925-141">Once you begin entering the connection profile information, if you change your focus from Visual Studio Code, you have to restart creating the connection profile.</span></span>
>

1. <span data-ttu-id="f9925-142">A VS Code-ban nyomja le a **CTRL+SHIFT+P** billentyűkombinációt (vagy az **F1** billentyűt) a parancskatalógus megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="f9925-142">In VS Code, press **CTRL+SHIFT+P** (or **F1**) to open the Command Palette.</span></span>

2. <span data-ttu-id="f9925-143">Írja be az **sqlcon** parancsot, és nyomja le az **ENTER** billentyűt.</span><span class="sxs-lookup"><span data-stu-id="f9925-143">Type **sqlcon** and press **ENTER**.</span></span>

3. <span data-ttu-id="f9925-144">A **Kapcsolati profil létrehozása** kiválasztásához nyomja le az **ENTER** billentyűt.</span><span class="sxs-lookup"><span data-stu-id="f9925-144">Press **ENTER** to select **Create Connection Profile**.</span></span> <span data-ttu-id="f9925-145">Ezzel létrehoz egy kapcsolati profilt az SQL Server-példányhoz.</span><span class="sxs-lookup"><span data-stu-id="f9925-145">This creates a connection profile for your SQL Server instance.</span></span>

4. <span data-ttu-id="f9925-146">Az útmutatást követve adja meg az új kapcsolati profil kapcsolati tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="f9925-146">Follow the prompts to specify the connection properties for the new connection profile.</span></span> <span data-ttu-id="f9925-147">Az egyes értékek kiválasztása után a folytatáshoz nyomja le az **ENTER** billentyűt.</span><span class="sxs-lookup"><span data-stu-id="f9925-147">After specifying each value, press **ENTER** to continue.</span></span> 

   | <span data-ttu-id="f9925-148">Beállítás</span><span class="sxs-lookup"><span data-stu-id="f9925-148">Setting</span></span>       | <span data-ttu-id="f9925-149">Ajánlott érték</span><span class="sxs-lookup"><span data-stu-id="f9925-149">Suggested value</span></span> | <span data-ttu-id="f9925-150">Leírás</span><span class="sxs-lookup"><span data-stu-id="f9925-150">Description</span></span> |
   | ------------ | ------------------ | ------------------------------------------------- | 
   | <span data-ttu-id="f9925-151">**Kiszolgálónév</span><span class="sxs-lookup"><span data-stu-id="f9925-151">**Server name</span></span> | <span data-ttu-id="f9925-152">A teljes kiszolgálónév</span><span class="sxs-lookup"><span data-stu-id="f9925-152">The fully qualified server name</span></span> | <span data-ttu-id="f9925-153">A névnek valami ehhez hasonlónak kell lennie: **mynewserver20170313.database.windows.net**.</span><span class="sxs-lookup"><span data-stu-id="f9925-153">The name should be something like this: **mynewserver20170313.database.windows.net**.</span></span> |
   | <span data-ttu-id="f9925-154">**Adatbázis neve**</span><span class="sxs-lookup"><span data-stu-id="f9925-154">**Database name**</span></span> | <span data-ttu-id="f9925-155">mySampleDatabase</span><span class="sxs-lookup"><span data-stu-id="f9925-155">mySampleDatabase</span></span> | <span data-ttu-id="f9925-156">A kiszolgáló neve, amelyhez kapcsolódni szeretne.</span><span class="sxs-lookup"><span data-stu-id="f9925-156">The name of the database to which to connect.</span></span> |
   | <span data-ttu-id="f9925-157">**Hitelesítés**</span><span class="sxs-lookup"><span data-stu-id="f9925-157">**Authentication**</span></span> | <span data-ttu-id="f9925-158">SQL-bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="f9925-158">SQL Login</span></span>| <span data-ttu-id="f9925-159">Az SQL-hitelesítés az egyetlen hitelesítési típus, amelyet ebben az oktatóanyagban konfiguráltunk.</span><span class="sxs-lookup"><span data-stu-id="f9925-159">SQL Authentication is the only authentication type that we have configured in this tutorial.</span></span> |
   | <span data-ttu-id="f9925-160">**Felhasználónév**</span><span class="sxs-lookup"><span data-stu-id="f9925-160">**User name**</span></span> | <span data-ttu-id="f9925-161">A kiszolgálói rendszergazdai fiók</span><span class="sxs-lookup"><span data-stu-id="f9925-161">The server admin account</span></span> | <span data-ttu-id="f9925-162">Ez az a fiók, amely a kiszolgáló létrehozásakor lett megadva.</span><span class="sxs-lookup"><span data-stu-id="f9925-162">This is the account that you specified when you created the server.</span></span> |
   | <span data-ttu-id="f9925-163">**Jelszó (SQL-bejelentkezés)**</span><span class="sxs-lookup"><span data-stu-id="f9925-163">**Password (SQL Login)**</span></span> | <span data-ttu-id="f9925-164">A kiszolgálói rendszergazdai fiók jelszava</span><span class="sxs-lookup"><span data-stu-id="f9925-164">The password for your server admin account</span></span> | <span data-ttu-id="f9925-165">Ez az a jelszó, amely a kiszolgáló létrehozásakor lett megadva.</span><span class="sxs-lookup"><span data-stu-id="f9925-165">This is the password that you specified when you created the server.</span></span> |
   | <span data-ttu-id="f9925-166">**Menti a jelszót?**</span><span class="sxs-lookup"><span data-stu-id="f9925-166">**Save Password?**</span></span> | <span data-ttu-id="f9925-167">Igen vagy nem</span><span class="sxs-lookup"><span data-stu-id="f9925-167">Yes or No</span></span> | <span data-ttu-id="f9925-168">Válassza az Igen lehetőséget, ha nem szeretné minden alkalommal megadni a jelszót.</span><span class="sxs-lookup"><span data-stu-id="f9925-168">Select Yes if you do not want to enter the password each time.</span></span> |
   | <span data-ttu-id="f9925-169">**Adja meg a profil kívánt nevét**</span><span class="sxs-lookup"><span data-stu-id="f9925-169">**Enter a name for this profile**</span></span> | <span data-ttu-id="f9925-170">Egy profilnév, például: **mySampleDatabase**</span><span class="sxs-lookup"><span data-stu-id="f9925-170">A profile name, such as **mySampleDatabase**</span></span> | <span data-ttu-id="f9925-171">A mentett profilnév felgyorsítja a csatlakozást a későbbi bejelentkezések során.</span><span class="sxs-lookup"><span data-stu-id="f9925-171">A saved profile name speeds your connection on subsequent logins.</span></span> | 

5. <span data-ttu-id="f9925-172">Az **ESC** billentyűt lenyomva zárja be a profil létrehozásáról és csatlakoztatásáról tájékoztató üzenetet.</span><span class="sxs-lookup"><span data-stu-id="f9925-172">Press the **ESC** key to close the info message that informs you that the profile is created and connected.</span></span>

6. <span data-ttu-id="f9925-173">Ellenőrizze a kapcsolatot az állapotsávon.</span><span class="sxs-lookup"><span data-stu-id="f9925-173">Verify your connection in the status bar.</span></span>

   ![A kapcsolat állapota](./media/sql-database-connect-query-vscode/vscode-connection-status.png)

## <a name="query-data"></a><span data-ttu-id="f9925-175">Adatok lekérdezése</span><span class="sxs-lookup"><span data-stu-id="f9925-175">Query data</span></span>

<span data-ttu-id="f9925-176">A következő kód használatával lekérdezheti kategóriánként az első 20 terméket a [SELECT](https://msdn.microsoft.com/library/ms189499.aspx) Transact-SQL-utasítással.</span><span class="sxs-lookup"><span data-stu-id="f9925-176">Use the following code to query for the top 20 products by category using the [SELECT](https://msdn.microsoft.com/library/ms189499.aspx) Transact-SQL statement.</span></span>

1. <span data-ttu-id="f9925-177">A **szerkesztő** ablakában írja be a következő lekérdezést az üres lekérdezési ablakba:</span><span class="sxs-lookup"><span data-stu-id="f9925-177">In the **Editor** window, enter the following query in the empty query window:</span></span>

   ```sql
   SELECT pc.Name as CategoryName, p.name as ProductName
   FROM [SalesLT].[ProductCategory] pc
   JOIN [SalesLT].[Product] p
   ON pc.productcategoryid = p.productcategoryid;
   ```

2. <span data-ttu-id="f9925-178">A **CTRL+SHIFT+E** billentyűkombinációt lenyomva lekérheti az adatokat a Product és ProductCategory táblából.</span><span class="sxs-lookup"><span data-stu-id="f9925-178">Press **CTRL+SHIFT+E** to retrieve data from the Product and ProductCategory tables.</span></span>

    ![Lekérdezés](./media/sql-database-connect-query-vscode/query.png)

## <a name="insert-data"></a><span data-ttu-id="f9925-180">Adat beszúrása</span><span class="sxs-lookup"><span data-stu-id="f9925-180">Insert data</span></span>

<span data-ttu-id="f9925-181">A következő kód használatával beszúrhat egy új terméket a SalesLT.Product táblába az [INSERT](https://msdn.microsoft.com/library/ms174335.aspx) Transact-SQL-utasítással.</span><span class="sxs-lookup"><span data-stu-id="f9925-181">Use the following code to insert a new product into the SalesLT.Product table using the [INSERT](https://msdn.microsoft.com/library/ms174335.aspx) Transact-SQL statement.</span></span>

1. <span data-ttu-id="f9925-182">A **szerkesztő** ablakában törölje az előző lekérdezést, és adja meg a következő lekérdezést:</span><span class="sxs-lookup"><span data-stu-id="f9925-182">In the **Editor** window, delete the previous query and enter the following query:</span></span>

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

2. <span data-ttu-id="f9925-183">A **CTRL+SHFT+E** billentyűkombinációt lenyomva beszúrhat egy új sort a Product táblába.</span><span class="sxs-lookup"><span data-stu-id="f9925-183">Press **CTRL+SHIFT+E** to insert a new row in the Product table.</span></span>

## <a name="update-data"></a><span data-ttu-id="f9925-184">Adatok frissítése</span><span class="sxs-lookup"><span data-stu-id="f9925-184">Update data</span></span>

<span data-ttu-id="f9925-185">A következő kód használatával frissítheti az előzőleg hozzáadott új terméket az [UPDATE](https://msdn.microsoft.com/library/ms177523.aspx) Transact-SQL-utasítással.</span><span class="sxs-lookup"><span data-stu-id="f9925-185">Use the following code to update the new product that you previously added using the [UPDATE](https://msdn.microsoft.com/library/ms177523.aspx) Transact-SQL statement.</span></span>

1.  <span data-ttu-id="f9925-186">A **szerkesztő** ablakában törölje az előző lekérdezést, és adja meg a következő lekérdezést:</span><span class="sxs-lookup"><span data-stu-id="f9925-186">In the **Editor** window, delete the previous query and enter the following query:</span></span>

   ```sql
   UPDATE [SalesLT].[Product]
   SET [ListPrice] = 125
   WHERE Name = 'myNewProduct';
   ```

2. <span data-ttu-id="f9925-187">A **CTRL+SHFT+E** billentyűkombinációt lenyomva frissítheti a megadott sort a Product táblában.</span><span class="sxs-lookup"><span data-stu-id="f9925-187">Press **CTRL+SHIFT+E** to update the specified row in the Product table.</span></span>

## <a name="delete-data"></a><span data-ttu-id="f9925-188">Adat törlése</span><span class="sxs-lookup"><span data-stu-id="f9925-188">Delete data</span></span>

<span data-ttu-id="f9925-189">A következő kód használatával törölheti az előzőleg hozzáadott új terméket a [DELETE](https://msdn.microsoft.com/library/ms189835.aspx) Transact-SQL-utasítással.</span><span class="sxs-lookup"><span data-stu-id="f9925-189">Use the following code to delete the new product that you previously added using the [DELETE](https://msdn.microsoft.com/library/ms189835.aspx) Transact-SQL statement.</span></span>

1. <span data-ttu-id="f9925-190">A **szerkesztő** ablakában törölje az előző lekérdezést, és adja meg a következő lekérdezést:</span><span class="sxs-lookup"><span data-stu-id="f9925-190">In the **Editor** window, delete the previous query and enter the following query:</span></span>

   ```sql
   DELETE FROM [SalesLT].[Product]
   WHERE Name = 'myNewProduct';
   ```

2. <span data-ttu-id="f9925-191">A **CTRL+SHFT+E** billentyűkombinációt lenyomva törölheti a megadott sort a Product táblából.</span><span class="sxs-lookup"><span data-stu-id="f9925-191">Press **CTRL+SHIFT+E** to delete the specified row in the Product table.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f9925-192">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f9925-192">Next steps</span></span>

- <span data-ttu-id="f9925-193">Az SQL Server Management Studióval történő csatlakozáshoz és lekérdezéshez lásd [az SSMS segítségével történő csatlakozással és lekérdezéssel](sql-database-connect-query-ssms.md) foglalkozó témakört.</span><span class="sxs-lookup"><span data-stu-id="f9925-193">To connect and query using SQL Server Management Studio, see [Connect and query with SSMS](sql-database-connect-query-ssms.md).</span></span>
- <span data-ttu-id="f9925-194">Az MSDN magazin Visual Studio Code használatáról szóló cikkéhez lásd az [Adatbázis IDE létrehozása az MSSQL bővítménnyel blogbejegyzést](https://msdn.microsoft.com/magazine/mt809115).</span><span class="sxs-lookup"><span data-stu-id="f9925-194">For an MSDN magazine article on using Visual Studio Code, see [Create a database IDE with MSSQL extension blog post](https://msdn.microsoft.com/magazine/mt809115).</span></span>
