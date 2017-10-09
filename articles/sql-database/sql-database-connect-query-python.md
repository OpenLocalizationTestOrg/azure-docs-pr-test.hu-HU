---
title: aaaUse Python tooquery Azure SQL Database |} Microsoft Docs
description: "Ez a témakör bemutatja, hogyan toouse Python toocreate egy programot, amely kapcsolatot tooan Azure SQL Database és a lekérdezés Transact-SQL utasítás használatával."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 452ad236-7a15-4f19-8ea7-df528052a3ad
ms.service: sql-database
ms.custom: mvc,develop apps
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: hero-article
ms.date: 08/08/2017
ms.author: carlrab
ms.openlocfilehash: 6c786e7a61f5ed3e7d2395da59acfeae008e90e3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-python-tooquery-an-azure-sql-database"></a><span data-ttu-id="9d3ec-103">Python tooquery Azure SQL-adatbázis használata</span><span class="sxs-lookup"><span data-stu-id="9d3ec-103">Use Python tooquery an Azure SQL database</span></span>

 <span data-ttu-id="9d3ec-104">A gyors üzembe helyezési bemutatja, hogyan toouse [Python](https://python.org) tooconnect tooan Azure SQL adatbázis- és a Transact-SQL utasítás tooquery adatok felhasználásával.</span><span class="sxs-lookup"><span data-stu-id="9d3ec-104">This quick start demonstrates how toouse [Python](https://python.org) tooconnect tooan Azure SQL database and use Transact-SQL statements tooquery data.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9d3ec-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="9d3ec-105">Prerequisites</span></span>

<span data-ttu-id="9d3ec-106">toocomplete a gyors üzembe helyezési útmutató, győződjön meg arról, hogy a következő hello:</span><span class="sxs-lookup"><span data-stu-id="9d3ec-106">toocomplete this quick start tutorial, make sure you have hello following:</span></span>

- <span data-ttu-id="9d3ec-107">Azure SQL Database-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="9d3ec-107">An Azure SQL database.</span></span> <span data-ttu-id="9d3ec-108">A gyors üzembe helyezési hoznak létre az egyik a gyors üzembe helyezések hello erőforrást használ:</span><span class="sxs-lookup"><span data-stu-id="9d3ec-108">This quick start uses hello resources created in one of these quick starts:</span></span> 

   - [<span data-ttu-id="9d3ec-109">DB létrehozása – portál</span><span class="sxs-lookup"><span data-stu-id="9d3ec-109">Create DB - Portal</span></span>](sql-database-get-started-portal.md)
   - [<span data-ttu-id="9d3ec-110">DB létrehozása – CLI</span><span class="sxs-lookup"><span data-stu-id="9d3ec-110">Create DB - CLI</span></span>](sql-database-get-started-cli.md)
   - [<span data-ttu-id="9d3ec-111">DB létrehozása – PowerShell</span><span class="sxs-lookup"><span data-stu-id="9d3ec-111">Create DB - PowerShell</span></span>](sql-database-get-started-powershell.md)

- <span data-ttu-id="9d3ec-112">A [kiszolgálószintű tűzfalszabály](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) hello nyilvános IP-cím hello számítógép használja a gyors üzembe helyezési útmutató.</span><span class="sxs-lookup"><span data-stu-id="9d3ec-112">A [server-level firewall rule](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) for hello public IP address of hello computer you use for this quick start tutorial.</span></span>

- <span data-ttu-id="9d3ec-113">Telepítette a Pythont és az operációs rendszerének megfelelő kapcsolódó szoftvereket.</span><span class="sxs-lookup"><span data-stu-id="9d3ec-113">You have installed Python and related software for your operating system.</span></span>

    - <span data-ttu-id="9d3ec-114">**MacOS**: Homebrew telepítése és a Python, hello ODBC-illesztőprogram és az Sqlcmd ESZKÖZRE telepíteni, és telepítse az SQL Server hello Python illesztőprogram.</span><span class="sxs-lookup"><span data-stu-id="9d3ec-114">**MacOS**: Install Homebrew and Python, install hello ODBC driver and SQLCMD, and then install hello Python Driver for SQL Server.</span></span> <span data-ttu-id="9d3ec-115">Lásd az [1.2, 1.3 és 2.1 lépést](https://www.microsoft.com/sql-server/developer-get-started/python/mac/).</span><span class="sxs-lookup"><span data-stu-id="9d3ec-115">See [Steps 1.2, 1.3, and 2.1](https://www.microsoft.com/sql-server/developer-get-started/python/mac/).</span></span>
    - <span data-ttu-id="9d3ec-116">**Ubuntu**: Python telepítése és más szükséges csomagokat, majd a telepítés hello Python illesztőprogram az SQL Server.</span><span class="sxs-lookup"><span data-stu-id="9d3ec-116">**Ubuntu**:  Install Python and other required packages, and then install hello Python Driver for SQL Server.</span></span> <span data-ttu-id="9d3ec-117">Lásd az [1.2, 1.3 és 2.1 lépést](https://www.microsoft.com/sql-server/developer-get-started/python/ubuntu/).</span><span class="sxs-lookup"><span data-stu-id="9d3ec-117">See [Steps 1.2, 1.3, and 2.1](https://www.microsoft.com/sql-server/developer-get-started/python/ubuntu/).</span></span>
    - <span data-ttu-id="9d3ec-118">**Windows**: hello (környezeti változó konfigurálta az Ön) Python legújabb verziójának telepítéséhez, telepítse az ODBC-illesztőprogram hello és SQLCMD és telepítse az SQL Server hello Python illesztőprogram.</span><span class="sxs-lookup"><span data-stu-id="9d3ec-118">**Windows**: Install hello newest version of Python (environment variable is now configured for you), install hello ODBC driver and SQLCMD, and then install hello Python Driver for SQL Server.</span></span> <span data-ttu-id="9d3ec-119">Lásd az [1.2, 1.3 és 2.1 lépést](https://www.microsoft.com/sql-server/developer-get-started/python/windows/).</span><span class="sxs-lookup"><span data-stu-id="9d3ec-119">See [Step 1.2, 1.3, and 2.1](https://www.microsoft.com/sql-server/developer-get-started/python/windows/).</span></span> 

## <a name="sql-server-connection-information"></a><span data-ttu-id="9d3ec-120">Az SQL-kiszolgáló kapcsolatadatai</span><span class="sxs-lookup"><span data-stu-id="9d3ec-120">SQL server connection information</span></span>

<span data-ttu-id="9d3ec-121">Hello kapcsolat szükséges információkat tooconnect toohello Azure SQL adatbázis beolvasása.</span><span class="sxs-lookup"><span data-stu-id="9d3ec-121">Get hello connection information needed tooconnect toohello Azure SQL database.</span></span> <span data-ttu-id="9d3ec-122">Hello teljes kiszolgálónév, az adatbázisnév és a bejelentkezési adatok a következő eljárások hello kell.</span><span class="sxs-lookup"><span data-stu-id="9d3ec-122">You will need hello fully qualified server name, database name, and login information in hello next procedures.</span></span>

1. <span data-ttu-id="9d3ec-123">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="9d3ec-123">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="9d3ec-124">Válassza ki **SQL-adatbázisok** hello bal oldali menüben kattintson a hello adatbázis **SQL-adatbázisok** lap.</span><span class="sxs-lookup"><span data-stu-id="9d3ec-124">Select **SQL Databases** from hello left-hand menu, and click your database on hello **SQL databases** page.</span></span> 
3. <span data-ttu-id="9d3ec-125">A hello **áttekintése** lap az adatbázishoz, tekintse át hello teljesen minősített kiszolgáló neve, ahogy az a következő kép hello.</span><span class="sxs-lookup"><span data-stu-id="9d3ec-125">On hello **Overview** page for your database, review hello fully qualified server name as shown in hello following image.</span></span> <span data-ttu-id="9d3ec-126">Hello server name toobring hello másolatot is mutat **toocopy kattintson** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="9d3ec-126">You can hover over hello server name toobring up hello **Click toocopy** option.</span></span>  

   ![server-name](./media/sql-database-connect-query-dotnet/server-name.png) 

4. <span data-ttu-id="9d3ec-128">Ha elfelejti a kiszolgálói bejelentkezési adatok, keresse meg a toohello SQL adatbázis-kiszolgáló lapon tooview hello server admin neve és, ha szükséges, állítsa vissza a hello jelszót.</span><span class="sxs-lookup"><span data-stu-id="9d3ec-128">If you forget your server login information, navigate toohello SQL Database server page tooview hello server admin name and, if necessary, reset hello password.</span></span>     
    
## <a name="insert-code-tooquery-sql-database"></a><span data-ttu-id="9d3ec-129">Helyezze be a kódját tooquery SQL-adatbázis</span><span class="sxs-lookup"><span data-stu-id="9d3ec-129">Insert code tooquery SQL database</span></span> 

1. <span data-ttu-id="9d3ec-130">Egy tetszőleges szövegszerkesztőben hozza létre a **sqltest.py** nevű új fájlt.</span><span class="sxs-lookup"><span data-stu-id="9d3ec-130">In your favorite text editor, create a new file, **sqltest.py**.</span></span>  

2. <span data-ttu-id="9d3ec-131">Cserélje ki hello tartalmát hello az alábbi kód, és adja hozzá a megfelelő értékeket hello a kiszolgáló, az adatbázis, a felhasználó és a jelszavát.</span><span class="sxs-lookup"><span data-stu-id="9d3ec-131">Replace hello contents with hello following code and add hello appropriate values for your server, database, user, and password.</span></span>

```Python
import pyodbc
server = 'your_server.database.windows.net'
database = 'your_database'
username = 'your_username'
password = 'your_password'
driver= '{ODBC Driver 13 for SQL Server}'
cnxn = pyodbc.connect('DRIVER='+driver+';PORT=1433;SERVER='+server+';PORT=1443;DATABASE='+database+';UID='+username+';PWD='+ password)
cursor = cnxn.cursor()
cursor.execute("SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName FROM [SalesLT].[ProductCategory] pc JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid")
row = cursor.fetchone()
while row:
    print (str(row[0]) + " " + str(row[1]))
    row = cursor.fetchone()
```

## <a name="run-hello-code"></a><span data-ttu-id="9d3ec-132">Hello kód futtatása</span><span class="sxs-lookup"><span data-stu-id="9d3ec-132">Run hello code</span></span>

1. <span data-ttu-id="9d3ec-133">Hello parancssorban futtassa a következő parancsok hello:</span><span class="sxs-lookup"><span data-stu-id="9d3ec-133">At hello command prompt, run hello following commands:</span></span>

   ```Python
   python sqltest.py
   ```

2. <span data-ttu-id="9d3ec-134">Győződjön meg arról, hogy hello felső 20 sorokat ad vissza, és zárja be hello alkalmazás ablak.</span><span class="sxs-lookup"><span data-stu-id="9d3ec-134">Verify that hello top 20 rows are returned and then close hello application window.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9d3ec-135">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9d3ec-135">Next steps</span></span>

- [<span data-ttu-id="9d3ec-136">Az első SQL Database-adatbázis megtervezése</span><span class="sxs-lookup"><span data-stu-id="9d3ec-136">Design your first Azure SQL database</span></span>](sql-database-design-first-database.md)
- [<span data-ttu-id="9d3ec-137">SQL Serverre készült Microsoft Python-illesztőprogramok</span><span class="sxs-lookup"><span data-stu-id="9d3ec-137">Microsoft Python Drivers for SQL Server</span></span>](https://docs.microsoft.com/sql/connect/python/python-driver-for-sql-server/)
- [<span data-ttu-id="9d3ec-138">Python fejlesztői központ</span><span class="sxs-lookup"><span data-stu-id="9d3ec-138">Python Developer Center</span></span>](https://azure.microsoft.com/develop/python/?v=17.23h)

