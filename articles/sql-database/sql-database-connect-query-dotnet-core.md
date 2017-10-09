---
title: aaaUse .NET Core tooquery Azure SQL Database |} Microsoft Docs
description: "Ez a témakör bemutatja, hogyan toouse .NET Core toocreate egy programot, amely összeköti az Azure SQL Database tooan és lekérdezést Transact-SQL-utasítások segítségével."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,develop apps
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 07/05/2017
ms.author: carlrab
ms.openlocfilehash: 2d10c407f44f43b6baa3bf337cdd1173d9c9c35f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-net-core-c-tooquery-an-azure-sql-database"></a><span data-ttu-id="53bd1-103">A .NET Core (C#) tooquery Azure SQL-adatbázis használata</span><span class="sxs-lookup"><span data-stu-id="53bd1-103">Use .NET Core (C#) tooquery an Azure SQL database</span></span>

<span data-ttu-id="53bd1-104">Gyors üzembe helyezési oktatóanyag bemutatja, hogyan toouse [.NET Core](https://www.microsoft.com/net/) a Windows/Linux/macOS toocreate egy C# programban tooconnect tooan Azure SQL-adatbázis és a Transact-SQL utasítás tooquery adatok felhasználásával.</span><span class="sxs-lookup"><span data-stu-id="53bd1-104">This quick start tutorial demonstrates how toouse [.NET Core](https://www.microsoft.com/net/) on Windows/Linux/macOS toocreate a C# program tooconnect tooan Azure SQL database and use Transact-SQL statements tooquery data.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="53bd1-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="53bd1-105">Prerequisites</span></span>

<span data-ttu-id="53bd1-106">toocomplete a gyors üzembe helyezési útmutató, győződjön meg arról, hogy a következő hello:</span><span class="sxs-lookup"><span data-stu-id="53bd1-106">toocomplete this quick start tutorial, make sure you have hello following:</span></span>

- <span data-ttu-id="53bd1-107">Azure SQL Database-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="53bd1-107">An Azure SQL database.</span></span> <span data-ttu-id="53bd1-108">A gyors üzembe helyezési hoznak létre az egyik a gyors üzembe helyezések hello erőforrást használ:</span><span class="sxs-lookup"><span data-stu-id="53bd1-108">This quick start uses hello resources created in one of these quick starts:</span></span> 

   - [<span data-ttu-id="53bd1-109">DB létrehozása – portál</span><span class="sxs-lookup"><span data-stu-id="53bd1-109">Create DB - Portal</span></span>](sql-database-get-started-portal.md)
   - [<span data-ttu-id="53bd1-110">DB létrehozása – CLI</span><span class="sxs-lookup"><span data-stu-id="53bd1-110">Create DB - CLI</span></span>](sql-database-get-started-cli.md)
   - [<span data-ttu-id="53bd1-111">DB létrehozása – PowerShell</span><span class="sxs-lookup"><span data-stu-id="53bd1-111">Create DB - PowerShell</span></span>](sql-database-get-started-powershell.md)

- <span data-ttu-id="53bd1-112">A [kiszolgálószintű tűzfalszabály](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) hello nyilvános IP-cím hello számítógép használja a gyors üzembe helyezési útmutató.</span><span class="sxs-lookup"><span data-stu-id="53bd1-112">A [server-level firewall rule](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) for hello public IP address of hello computer you use for this quick start tutorial.</span></span>
- <span data-ttu-id="53bd1-113">Telepítette az [az operációs rendszerének megfelelő .NET Core-t](https://www.microsoft.com/net/core).</span><span class="sxs-lookup"><span data-stu-id="53bd1-113">You have installed [.NET Core for your operating system](https://www.microsoft.com/net/core).</span></span> 

## <a name="sql-server-connection-information"></a><span data-ttu-id="53bd1-114">Az SQL-kiszolgáló kapcsolatadatai</span><span class="sxs-lookup"><span data-stu-id="53bd1-114">SQL server connection information</span></span>

<span data-ttu-id="53bd1-115">Hello kapcsolat szükséges információkat tooconnect toohello Azure SQL adatbázis beolvasása.</span><span class="sxs-lookup"><span data-stu-id="53bd1-115">Get hello connection information needed tooconnect toohello Azure SQL database.</span></span> <span data-ttu-id="53bd1-116">Hello teljes kiszolgálónév, az adatbázisnév és a bejelentkezési adatok a következő eljárások hello kell.</span><span class="sxs-lookup"><span data-stu-id="53bd1-116">You will need hello fully qualified server name, database name, and login information in hello next procedures.</span></span>

1. <span data-ttu-id="53bd1-117">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="53bd1-117">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="53bd1-118">Válassza ki **SQL-adatbázisok** hello bal oldali menüben kattintson a hello adatbázis **SQL-adatbázisok** lap.</span><span class="sxs-lookup"><span data-stu-id="53bd1-118">Select **SQL Databases** from hello left-hand menu, and click your database on hello **SQL databases** page.</span></span> 
3. <span data-ttu-id="53bd1-119">A hello **áttekintése** lap az adatbázishoz, tekintse át hello teljesen minősített kiszolgáló neve, ahogy az a következő kép hello.</span><span class="sxs-lookup"><span data-stu-id="53bd1-119">On hello **Overview** page for your database, review hello fully qualified server name as shown in hello following image.</span></span> <span data-ttu-id="53bd1-120">Hello server name toobring hello másolatot is mutat **toocopy kattintson** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="53bd1-120">You can hover over hello server name toobring up hello **Click toocopy** option.</span></span> 

   ![server-name](./media/sql-database-connect-query-dotnet/server-name.png) 

4. <span data-ttu-id="53bd1-122">Ha az Azure SQL Database kiszolgáló bejelentkezési adatok, keresse meg a toohello SQL adatbázis-kiszolgáló lapon tooview hello server admin neve.</span><span class="sxs-lookup"><span data-stu-id="53bd1-122">If you forget your Azure SQL Database server login information, navigate toohello SQL Database server page tooview hello server admin name.</span></span> <span data-ttu-id="53bd1-123">Szükség esetén alaphelyzetbe állíthatja a hello jelszót.</span><span class="sxs-lookup"><span data-stu-id="53bd1-123">You can reset hello password if necessary.</span></span>

5. <span data-ttu-id="53bd1-124">Kattintson az: **Adatbázis-kapcsolati karakterláncok megjelenítése** elemre.</span><span class="sxs-lookup"><span data-stu-id="53bd1-124">Click **Show database connection strings**.</span></span>

6. <span data-ttu-id="53bd1-125">Felülvizsgálati hello teljes **ADO.NET** kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="53bd1-125">Review hello complete **ADO.NET** connection string.</span></span>

    ![ADO.NET kapcsolati karakterlánc](./media/sql-database-connect-query-dotnet/adonet-connection-string.png)

> [!IMPORTANT]
> <span data-ttu-id="53bd1-127">Rendelkeznie kell egy tűzfalszabályt hello nyilvános IP-cím hello számítógép, amelyen ez az oktatóanyag végrehajtása helyen.</span><span class="sxs-lookup"><span data-stu-id="53bd1-127">You must have a firewall rule in place for hello public IP address of hello computer on which you perform this tutorial.</span></span> <span data-ttu-id="53bd1-128">Ha egy másik számítógépen vagy egy másik nyilvános IP-címet, hozzon létre egy [kiszolgálószintű tűzfal szabály használatával hello Azure-portálon](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).</span><span class="sxs-lookup"><span data-stu-id="53bd1-128">If you are on a different computer or have a different public IP address, create a [server-level firewall rule using hello Azure portal](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).</span></span> 
>
  
## <a name="create-a-new-net-project"></a><span data-ttu-id="53bd1-129">Új .NET-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="53bd1-129">Create a new .NET project</span></span>

1. <span data-ttu-id="53bd1-130">Nyisson meg egy parancssort, és hozzon létre egy *sqltest* nevű mappát.</span><span class="sxs-lookup"><span data-stu-id="53bd1-130">Open a command prompt and create a folder named *sqltest*.</span></span> <span data-ttu-id="53bd1-131">Keresse meg a létrehozott, és futtassa a következő parancs hello toohello mappába:</span><span class="sxs-lookup"><span data-stu-id="53bd1-131">Navigate toohello folder you created and run hello following command:</span></span>

    ```
    dotnet new console
    ```

2. <span data-ttu-id="53bd1-132">Nyissa meg ***sqltest.csproj*** a kedvenc szövegszerkesztőjével, és adja hozzá a System.Data.SqlClient a függőség beállításához a következő kód hello használata:</span><span class="sxs-lookup"><span data-stu-id="53bd1-132">Open ***sqltest.csproj*** with your favorite text editor and add System.Data.SqlClient as a dependency using hello following code:</span></span>

    ```xml
    <ItemGroup>
        <PackageReference Include="System.Data.SqlClient" Version="4.3.0" />
    </ItemGroup>
    ```

## <a name="insert-code-tooquery-sql-database"></a><span data-ttu-id="53bd1-133">Helyezze be a kódját tooquery SQL-adatbázis</span><span class="sxs-lookup"><span data-stu-id="53bd1-133">Insert code tooquery SQL database</span></span>

1. <span data-ttu-id="53bd1-134">A fejlesztői környezetben vagy egy tetszőleges szövegszerkesztőben nyissa meg a **Program.cs** fájlt.</span><span class="sxs-lookup"><span data-stu-id="53bd1-134">In your development environment or favorite text editor open **Program.cs**</span></span>

2. <span data-ttu-id="53bd1-135">Cserélje ki hello tartalmát hello az alábbi kód, és adja hozzá a megfelelő értékeket hello a kiszolgáló, az adatbázis, a felhasználó és a jelszavát.</span><span class="sxs-lookup"><span data-stu-id="53bd1-135">Replace hello contents with hello following code and add hello appropriate values for your server, database, user, and password.</span></span>

```csharp
using System;
using System.Data.SqlClient;
using System.Text;

namespace sqltest
{
    class Program
    {
        static void Main(string[] args)
        {
            try 
            { 
                SqlConnectionStringBuilder builder = new SqlConnectionStringBuilder();
                builder.DataSource = "your_server.database.windows.net"; 
                builder.UserID = "your_user";            
                builder.Password = "your_password";     
                builder.InitialCatalog = "your_database";

                using (SqlConnection connection = new SqlConnection(builder.ConnectionString))
                {
                    Console.WriteLine("\nQuery data example:");
                    Console.WriteLine("=========================================\n");
                    
                    connection.Open();       
                    StringBuilder sb = new StringBuilder();
                    sb.Append("SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName ");
                    sb.Append("FROM [SalesLT].[ProductCategory] pc ");
                    sb.Append("JOIN [SalesLT].[Product] p ");
                    sb.Append("ON pc.productcategoryid = p.productcategoryid;");
                    String sql = sb.ToString();

                    using (SqlCommand command = new SqlCommand(sql, connection))
                    {
                        using (SqlDataReader reader = command.ExecuteReader())
                        {
                            while (reader.Read())
                            {
                                Console.WriteLine("{0} {1}", reader.GetString(0), reader.GetString(1));
                            }
                        }
                    }                    
                }
            }
            catch (SqlException e)
            {
                Console.WriteLine(e.ToString());
            }
            Console.ReadLine();
        }
    }
}
```

## <a name="run-hello-code"></a><span data-ttu-id="53bd1-136">Hello kód futtatása</span><span class="sxs-lookup"><span data-stu-id="53bd1-136">Run hello code</span></span>

1. <span data-ttu-id="53bd1-137">Hello parancssorban futtassa a következő parancsok hello:</span><span class="sxs-lookup"><span data-stu-id="53bd1-137">At hello command prompt, run hello following commands:</span></span>

   ```csharp
   dotnet restore
   dotnet run
   ```

2. <span data-ttu-id="53bd1-138">Győződjön meg arról, hogy hello felső 20 sorokat ad vissza, és zárja be hello alkalmazás ablak.</span><span class="sxs-lookup"><span data-stu-id="53bd1-138">Verify that hello top 20 rows are returned and then close hello application window.</span></span>


## <a name="next-steps"></a><span data-ttu-id="53bd1-139">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="53bd1-139">Next steps</span></span>

- <span data-ttu-id="53bd1-140">[Ismerkedés a Windows/Linux/macOS hello parancssor használatával a .NET Core](/dotnet/core/tutorials/using-with-xplat-cli).</span><span class="sxs-lookup"><span data-stu-id="53bd1-140">[Getting started with .NET Core on Windows/Linux/macOS using hello command line](/dotnet/core/tutorials/using-with-xplat-cli).</span></span>
- <span data-ttu-id="53bd1-141">Ismerje meg, hogyan túl[kapcsolódás és lekérdezés hello .NET-keretrendszer és a Visual Studio használatával Azure SQL-adatbázis](sql-database-connect-query-dotnet-visual-studio.md).</span><span class="sxs-lookup"><span data-stu-id="53bd1-141">Learn how too[connect and query an Azure SQL database using hello .NET framework and Visual Studio](sql-database-connect-query-dotnet-visual-studio.md).</span></span>  
- <span data-ttu-id="53bd1-142">Ismerje meg, hogyan túl[tervezése az első Azure SQL-adatbázis SSMS használatával](sql-database-design-first-database.md) vagy [tervezése az első Azure SQL Database adatbázishoz .NET használatával](sql-database-design-first-database-csharp.md).</span><span class="sxs-lookup"><span data-stu-id="53bd1-142">Learn how too[Design your first Azure SQL database using SSMS](sql-database-design-first-database.md) or [Design your first Azure SQL database using .NET](sql-database-design-first-database-csharp.md).</span></span>
- <span data-ttu-id="53bd1-143">A .NET-ről a [.NET dokumentációjában](https://docs.microsoft.com/dotnet/) talál további információt.</span><span class="sxs-lookup"><span data-stu-id="53bd1-143">For more information about .NET, see [.NET documentation](https://docs.microsoft.com/dotnet/).</span></span>
