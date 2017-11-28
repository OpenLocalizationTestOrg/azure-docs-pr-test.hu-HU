---
title: az Azure SQL Database Ruby tooquery aaaUse |} Microsoft Docs
description: "Ez a témakör bemutatja, hogyan toouse Ruby toocreate egy programot, amely kapcsolatot tooan Azure SQL Database és a lekérdezés Transact-SQL utasítás használatával."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 94fec528-58ba-4352-ba0d-25ae4b273e90
ms.service: sql-database
ms.custom: mvc,develop apps
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: hero-article
ms.date: 07/14/2017
ms.author: carlrab
ms.openlocfilehash: 0d4b16b8aacb5e376ab80cbe37569130f2fd52b2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-ruby-tooquery-an-azure-sql-database"></a><span data-ttu-id="32da7-103">Ruby tooquery Azure SQL-adatbázis használata</span><span class="sxs-lookup"><span data-stu-id="32da7-103">Use Ruby tooquery an Azure SQL database</span></span>

<span data-ttu-id="32da7-104">Gyors üzembe helyezési oktatóanyag bemutatja, hogyan toouse [Ruby](https://www.ruby-lang.org) toocreate egy program tooconnect tooan Azure SQL adatbázis- és a Transact-SQL utasítás tooquery adatok felhasználásával.</span><span class="sxs-lookup"><span data-stu-id="32da7-104">This quick start tutorial demonstrates how toouse [Ruby](https://www.ruby-lang.org) toocreate a program tooconnect tooan Azure SQL database and use Transact-SQL statements tooquery data.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="32da7-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="32da7-105">Prerequisites</span></span>

<span data-ttu-id="32da7-106">toocomplete a gyors üzembe helyezési útmutató, győződjön meg arról, hogy a következő előfeltételek hello:</span><span class="sxs-lookup"><span data-stu-id="32da7-106">toocomplete this quick start tutorial, make sure you have hello following prerequisites:</span></span>

- <span data-ttu-id="32da7-107">Azure SQL Database-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="32da7-107">An Azure SQL database.</span></span> <span data-ttu-id="32da7-108">A gyors üzembe helyezési hoznak létre az egyik a gyors üzembe helyezések hello erőforrást használ:</span><span class="sxs-lookup"><span data-stu-id="32da7-108">This quick start uses hello resources created in one of these quick starts:</span></span> 

   - [<span data-ttu-id="32da7-109">DB létrehozása – portál</span><span class="sxs-lookup"><span data-stu-id="32da7-109">Create DB - Portal</span></span>](sql-database-get-started-portal.md)
   - [<span data-ttu-id="32da7-110">DB létrehozása – CLI</span><span class="sxs-lookup"><span data-stu-id="32da7-110">Create DB - CLI</span></span>](sql-database-get-started-cli.md)
   - [<span data-ttu-id="32da7-111">DB létrehozása – PowerShell</span><span class="sxs-lookup"><span data-stu-id="32da7-111">Create DB - PowerShell</span></span>](sql-database-get-started-powershell.md)

- <span data-ttu-id="32da7-112">A [kiszolgálószintű tűzfalszabály](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) hello nyilvános IP-cím hello számítógép használja a gyors üzembe helyezési útmutató.</span><span class="sxs-lookup"><span data-stu-id="32da7-112">A [server-level firewall rule](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) for hello public IP address of hello computer you use for this quick start tutorial.</span></span>
- <span data-ttu-id="32da7-113">Telepítette a Rubyt és az operációs rendszerének megfelelő kapcsolódó szoftvereket.</span><span class="sxs-lookup"><span data-stu-id="32da7-113">You have installed Ruby and related software for your operating system.</span></span>
    - <span data-ttu-id="32da7-114">**MacOS**: Telepítse a Homebrew-t, az rbenv-t és a ruby-buildet, a Rubyt, végül pedig a FreeTDS-t.</span><span class="sxs-lookup"><span data-stu-id="32da7-114">**MacOS**: Install Homebrew, install rbenv and ruby-build, install Ruby, and then install FreeTDS.</span></span> <span data-ttu-id="32da7-115">Lásd az [1.2, 1.3, 1.4 és 1.5 lépést](https://www.microsoft.com/sql-server/developer-get-started/ruby/mac/).</span><span class="sxs-lookup"><span data-stu-id="32da7-115">See [Step 1.2, 1.3, 1.4, and 1.5](https://www.microsoft.com/sql-server/developer-get-started/ruby/mac/).</span></span>
    - <span data-ttu-id="32da7-116">**Ubuntu**: Telepítse a Ruby előfeltételeit, az rbenv-t és a ruby-buildet, a Rubyt, végül pedig a FreeTDS-t.</span><span class="sxs-lookup"><span data-stu-id="32da7-116">**Ubuntu**: Install prerequisites for Ruby, install rbenv and ruby-build, install Ruby, and then install FreeTDS.</span></span> <span data-ttu-id="32da7-117">Lásd az [1.2, 1.3, 1.4 és 1.5 lépést](https://www.microsoft.com/sql-server/developer-get-started/ruby/ubuntu/).</span><span class="sxs-lookup"><span data-stu-id="32da7-117">See [Step 1.2, 1.3, 1.4, and 1.5](https://www.microsoft.com/sql-server/developer-get-started/ruby/ubuntu/).</span></span>

## <a name="sql-server-connection-information"></a><span data-ttu-id="32da7-118">Az SQL-kiszolgáló kapcsolatadatai</span><span class="sxs-lookup"><span data-stu-id="32da7-118">SQL server connection information</span></span>

<span data-ttu-id="32da7-119">Hello kapcsolat szükséges információkat tooconnect toohello Azure SQL adatbázis beolvasása.</span><span class="sxs-lookup"><span data-stu-id="32da7-119">Get hello connection information needed tooconnect toohello Azure SQL database.</span></span> <span data-ttu-id="32da7-120">Hello teljes kiszolgálónév, az adatbázisnév és a bejelentkezési adatok a következő eljárások hello kell.</span><span class="sxs-lookup"><span data-stu-id="32da7-120">You will need hello fully qualified server name, database name, and login information in hello next procedures.</span></span>

1. <span data-ttu-id="32da7-121">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="32da7-121">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="32da7-122">Válassza ki **SQL-adatbázisok** hello bal oldali menüben kattintson a hello adatbázis **SQL-adatbázisok** lap.</span><span class="sxs-lookup"><span data-stu-id="32da7-122">Select **SQL Databases** from hello left-hand menu, and click your database on hello **SQL databases** page.</span></span> 
3. <span data-ttu-id="32da7-123">A hello **áttekintése** az adatbázis lapján tekintse át a hello kiszolgáló teljesen minősített nevet.</span><span class="sxs-lookup"><span data-stu-id="32da7-123">On hello **Overview** page for your database, review hello fully qualified server name.</span></span> <span data-ttu-id="32da7-124">Hello server name toobring hello másolatot is mutat **toocopy kattintson** beállítás, ahogy az a következő kép hello:</span><span class="sxs-lookup"><span data-stu-id="32da7-124">You can hover over hello server name toobring up hello **Click toocopy** option, as shown in hello following image:</span></span>

   ![server-name](./media/sql-database-connect-query-dotnet/server-name.png) 

4. <span data-ttu-id="32da7-126">Ha elfelejtette a hello bejelentkezési adatok az Azure SQL Database-kiszolgáló, keresse meg a toohello SQL adatbázis-kiszolgáló lapon tooview hello server admin neve és, ha szükséges, állítsa vissza a hello jelszót.</span><span class="sxs-lookup"><span data-stu-id="32da7-126">If you have forgotten hello login information for your Azure SQL Database server, navigate toohello SQL Database server page tooview hello server admin name and, if necessary, reset hello password.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="32da7-127">Rendelkeznie kell egy tűzfalszabályt hello nyilvános IP-cím hello számítógép, amelyen ez az oktatóanyag végrehajtása helyen.</span><span class="sxs-lookup"><span data-stu-id="32da7-127">You must have a firewall rule in place for hello public IP address of hello computer on which you perform this tutorial.</span></span> <span data-ttu-id="32da7-128">Ha egy másik számítógépen vagy egy másik nyilvános IP-címet, hozzon létre egy [kiszolgálószintű tűzfal szabály használatával hello Azure-portálon](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).</span><span class="sxs-lookup"><span data-stu-id="32da7-128">If you are on a different computer or have a different public IP address, create a [server-level firewall rule using hello Azure portal](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).</span></span> 

## <a name="insert-code-tooquery-sql-database"></a><span data-ttu-id="32da7-129">Helyezze be a kódját tooquery SQL-adatbázis</span><span class="sxs-lookup"><span data-stu-id="32da7-129">Insert code tooquery SQL database</span></span>

1. <span data-ttu-id="32da7-130">Egy tetszőleges szövegszerkesztőben hozzon létre egy **sqltest.rb** nevű új fájlt</span><span class="sxs-lookup"><span data-stu-id="32da7-130">In your favorite text editor, create a new file, **sqltest.rb**</span></span>

2. <span data-ttu-id="32da7-131">Cserélje ki hello tartalmát hello az alábbi kód, és adja hozzá a megfelelő értékeket hello a kiszolgáló, az adatbázis, a felhasználó és a jelszavát.</span><span class="sxs-lookup"><span data-stu-id="32da7-131">Replace hello contents with hello following code and add hello appropriate values for your server, database, user, and password.</span></span>

```ruby
require 'tiny_tds'
server = 'your_server.database.windows.net'
database = 'your_database'
username = 'your_username'
password = 'your_password'
client = TinyTds::Client.new username: username, password: password, 
    host: server, port: 1433, database: database, azure: true

puts "Reading data from table"
tsql = "SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
        FROM [SalesLT].[ProductCategory] pc
        JOIN [SalesLT].[Product] p
        ON pc.productcategoryid = p.productcategoryid"
result = client.execute(tsql)
result.each do |row|
    puts row
end
```

## <a name="run-hello-code"></a><span data-ttu-id="32da7-132">Hello kód futtatása</span><span class="sxs-lookup"><span data-stu-id="32da7-132">Run hello code</span></span>

1. <span data-ttu-id="32da7-133">Hello parancssorban futtassa a következő parancsok hello:</span><span class="sxs-lookup"><span data-stu-id="32da7-133">At hello command prompt, run hello following commands:</span></span>

   ```bash
   ruby sqltest.rb
   ```

2. <span data-ttu-id="32da7-134">Győződjön meg arról, hogy hello felső 20 sorokat ad vissza, és zárja be hello alkalmazás ablak.</span><span class="sxs-lookup"><span data-stu-id="32da7-134">Verify that hello top 20 rows are returned and then close hello application window.</span></span>


## <a name="next-steps"></a><span data-ttu-id="32da7-135">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="32da7-135">Next Steps</span></span>
- [<span data-ttu-id="32da7-136">Az első SQL Database-adatbázis megtervezése</span><span class="sxs-lookup"><span data-stu-id="32da7-136">Design your first Azure SQL database</span></span>](sql-database-design-first-database.md)
- [<span data-ttu-id="32da7-137">GitHub-adattár a TinyTDS-hez</span><span class="sxs-lookup"><span data-stu-id="32da7-137">GitHub repository for TinyTDS</span></span>](https://github.com/rails-sqlserver/tiny_tds)
- [<span data-ttu-id="32da7-138">Problémák jelentése és kérdezés a TinyTDS-sel kapcsolatban</span><span class="sxs-lookup"><span data-stu-id="32da7-138">Report issues or ask questions about TinyTDS</span></span>](https://github.com/rails-sqlserver/tiny_tds/issues)
- [<span data-ttu-id="32da7-139">Ruby-illesztőprogramok az SQL Serverhez</span><span class="sxs-lookup"><span data-stu-id="32da7-139">Ruby Drivers for SQL Server</span></span>](https://docs.microsoft.com/sql/connect/ruby/ruby-driver-for-sql-server/)
