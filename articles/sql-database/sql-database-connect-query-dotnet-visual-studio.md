---
title: "aaaUse Visual Studio és a .NET tooquery Azure SQL Database |} Microsoft Docs"
description: "Ez a témakör bemutatja, hogyan toouse Visual Studio toocreate egy programot, amely kapcsolatot tooan Azure SQL Database és a lekérdezés Transact-SQL utasítás használatával."
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
ms.openlocfilehash: 038cfb9c680217dfeea5a9996a0abed88cc80559
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-net-c-with-visual-studio-tooconnect-and-query-an-azure-sql-database"></a><span data-ttu-id="788dc-103">Visual Studio tooconnect .NET (C#) használjon, és egy Azure SQL-adatbázis lekérdezése</span><span class="sxs-lookup"><span data-stu-id="788dc-103">Use .NET (C#) with Visual Studio tooconnect and query an Azure SQL database</span></span>

<span data-ttu-id="788dc-104">Gyors üzembe helyezési oktatóanyag bemutatja, hogyan toouse hello [.NET-keretrendszer](https://www.microsoft.com/net/) toocreate egy C# Visual Studio tooconnect tooan Azure SQL Database programot, és a Transact-SQL utasítás tooquery adatok felhasználásával.</span><span class="sxs-lookup"><span data-stu-id="788dc-104">This quick start tutorial demonstrates how toouse hello [.NET framework](https://www.microsoft.com/net/) toocreate a C# program with Visual Studio tooconnect tooan Azure SQL database and use Transact-SQL statements tooquery data.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="788dc-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="788dc-105">Prerequisites</span></span>

<span data-ttu-id="788dc-106">toocomplete a gyors üzembe helyezési útmutató, győződjön meg arról, hogy a következő hello:</span><span class="sxs-lookup"><span data-stu-id="788dc-106">toocomplete this quick start tutorial, make sure you have hello following:</span></span>

- <span data-ttu-id="788dc-107">Azure SQL Database-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="788dc-107">An Azure SQL database.</span></span> <span data-ttu-id="788dc-108">A gyors üzembe helyezési hoznak létre az egyik a gyors üzembe helyezések hello erőforrást használ:</span><span class="sxs-lookup"><span data-stu-id="788dc-108">This quick start uses hello resources created in one of these quick starts:</span></span> 

   - [<span data-ttu-id="788dc-109">DB létrehozása – portál</span><span class="sxs-lookup"><span data-stu-id="788dc-109">Create DB - Portal</span></span>](sql-database-get-started-portal.md)
   - [<span data-ttu-id="788dc-110">DB létrehozása – CLI</span><span class="sxs-lookup"><span data-stu-id="788dc-110">Create DB - CLI</span></span>](sql-database-get-started-cli.md)
   - [<span data-ttu-id="788dc-111">DB létrehozása – PowerShell</span><span class="sxs-lookup"><span data-stu-id="788dc-111">Create DB - PowerShell</span></span>](sql-database-get-started-powershell.md)

- <span data-ttu-id="788dc-112">A [kiszolgálószintű tűzfalszabály](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) hello nyilvános IP-cím hello számítógép használja a gyors üzembe helyezési útmutató.</span><span class="sxs-lookup"><span data-stu-id="788dc-112">A [server-level firewall rule](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) for hello public IP address of hello computer you use for this quick start tutorial.</span></span>
- <span data-ttu-id="788dc-113">A [Visual Studio Community 2017, a Visual Studio Professional 2017 vagy a Visual Studio Enterprise 2017](https://www.visualstudio.com/downloads/) telepítése.</span><span class="sxs-lookup"><span data-stu-id="788dc-113">An installation of [Visual Studio Community 2017, Visual Studio Professional 2017, or Visual Studio Enterprise 2017](https://www.visualstudio.com/downloads/).</span></span>

## <a name="sql-server-connection-information"></a><span data-ttu-id="788dc-114">Az SQL-kiszolgáló kapcsolatadatai</span><span class="sxs-lookup"><span data-stu-id="788dc-114">SQL server connection information</span></span>

<span data-ttu-id="788dc-115">Hello kapcsolat szükséges információkat tooconnect toohello Azure SQL adatbázis beolvasása.</span><span class="sxs-lookup"><span data-stu-id="788dc-115">Get hello connection information needed tooconnect toohello Azure SQL database.</span></span> <span data-ttu-id="788dc-116">Hello teljes kiszolgálónév, az adatbázisnév és a bejelentkezési adatok a következő eljárások hello kell.</span><span class="sxs-lookup"><span data-stu-id="788dc-116">You will need hello fully qualified server name, database name, and login information in hello next procedures.</span></span>

1. <span data-ttu-id="788dc-117">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="788dc-117">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="788dc-118">Válassza ki **SQL-adatbázisok** hello bal oldali menüben kattintson a hello adatbázis **SQL-adatbázisok** lap.</span><span class="sxs-lookup"><span data-stu-id="788dc-118">Select **SQL Databases** from hello left-hand menu, and click your database on hello **SQL databases** page.</span></span> 
3. <span data-ttu-id="788dc-119">A hello **áttekintése** lap az adatbázishoz, tekintse át hello teljesen minősített kiszolgáló neve, ahogy az a következő kép hello.</span><span class="sxs-lookup"><span data-stu-id="788dc-119">On hello **Overview** page for your database, review hello fully qualified server name as shown in hello following image.</span></span> <span data-ttu-id="788dc-120">Hello server name toobring hello másolatot is mutat **toocopy kattintson** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="788dc-120">You can hover over hello server name toobring up hello **Click toocopy** option.</span></span> 

   ![server-name](./media/sql-database-connect-query-dotnet/server-name.png) 

4. <span data-ttu-id="788dc-122">Ha az Azure SQL Database kiszolgáló bejelentkezési adatok, keresse meg a toohello SQL adatbázis-kiszolgáló lapon tooview hello server admin neve.</span><span class="sxs-lookup"><span data-stu-id="788dc-122">If you forget your Azure SQL Database server login information, navigate toohello SQL Database server page tooview hello server admin name.</span></span> <span data-ttu-id="788dc-123">Szükség esetén alaphelyzetbe állíthatja a hello jelszót.</span><span class="sxs-lookup"><span data-stu-id="788dc-123">You can reset hello password if necessary.</span></span>

5. <span data-ttu-id="788dc-124">Kattintson az: **Adatbázis-kapcsolati karakterláncok megjelenítése** elemre.</span><span class="sxs-lookup"><span data-stu-id="788dc-124">Click **Show database connection strings**.</span></span>

6. <span data-ttu-id="788dc-125">Felülvizsgálati hello teljes **ADO.NET** kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="788dc-125">Review hello complete **ADO.NET** connection string.</span></span>

    ![ADO.NET kapcsolati karakterlánc](./media/sql-database-connect-query-dotnet/adonet-connection-string.png)

> [!IMPORTANT]
> <span data-ttu-id="788dc-127">Rendelkeznie kell egy tűzfalszabályt hello nyilvános IP-cím hello számítógép, amelyen ez az oktatóanyag végrehajtása helyen.</span><span class="sxs-lookup"><span data-stu-id="788dc-127">You must have a firewall rule in place for hello public IP address of hello computer on which you perform this tutorial.</span></span> <span data-ttu-id="788dc-128">Ha egy másik számítógépen vagy egy másik nyilvános IP-címet, hozzon létre egy [kiszolgálószintű tűzfal szabály használatával hello Azure-portálon](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).</span><span class="sxs-lookup"><span data-stu-id="788dc-128">If you are on a different computer or have a different public IP address, create a [server-level firewall rule using hello Azure portal](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).</span></span> 
>
  
## <a name="create-a-new-visual-studio-project"></a><span data-ttu-id="788dc-129">Új Visual Studio-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="788dc-129">Create a new Visual Studio project</span></span>

1. <span data-ttu-id="788dc-130">A Visual Studióban válassza a **Fájl**, **Új**, **Projekt** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="788dc-130">In Visual Studio, choose **File**, **New**, **Project**.</span></span> 
2. <span data-ttu-id="788dc-131">A hello **új projekt** párbeszédpanel, és bontsa ki a **Visual C#**.</span><span class="sxs-lookup"><span data-stu-id="788dc-131">In hello **New Project** dialog, and expand **Visual C#**.</span></span>
3. <span data-ttu-id="788dc-132">Válassza ki **Konzolalkalmazás** , és írja be *sqltest* hello projekt neve.</span><span class="sxs-lookup"><span data-stu-id="788dc-132">Select **Console App** and enter *sqltest* for hello project name.</span></span>
4. <span data-ttu-id="788dc-133">Kattintson a **OK** toocreate és a nyitott hello a Visual Studio új projekt</span><span class="sxs-lookup"><span data-stu-id="788dc-133">Click **OK** toocreate and open hello new project in Visual Studio</span></span>
4. <span data-ttu-id="788dc-134">A Megoldáskezelőben kattintson a jobb gombbal az **sqltest** elemre, majd kattintson a **Manage NuGet Packages** (NuGet-csomagok kezelése) parancsra.</span><span class="sxs-lookup"><span data-stu-id="788dc-134">In Solution Explorer, right-click **sqltest** and click **Manage NuGet Packages**.</span></span> 
5. <span data-ttu-id="788dc-135">A hello **Tallózás**, keressen ```System.Data.SqlClient``` és, ha található, válassza ki azt.</span><span class="sxs-lookup"><span data-stu-id="788dc-135">On hello **Browse**, search for ```System.Data.SqlClient``` and, when found, select it.</span></span>
6. <span data-ttu-id="788dc-136">A hello **System.Data.SqlClient** kattintson **telepítése**.</span><span class="sxs-lookup"><span data-stu-id="788dc-136">In hello **System.Data.SqlClient** page, click **Install**.</span></span>
7. <span data-ttu-id="788dc-137">Hello telepítés befejezése után tekintse át hello módosításokat, és kattintson a **OK** tooclose hello **előzetes** ablak.</span><span class="sxs-lookup"><span data-stu-id="788dc-137">When hello install completes, review hello changes and then click **OK** tooclose hello **Preview** window.</span></span> 
8. <span data-ttu-id="788dc-138">Ha megjelenik egy **License Acceptance** (Licenc elfogadása) ablak, kattintson az **I Accept** (Elfogadom) gombra.</span><span class="sxs-lookup"><span data-stu-id="788dc-138">If a **License Acceptance** window appears, click **I Accept**.</span></span>

## <a name="insert-code-tooquery-sql-database"></a><span data-ttu-id="788dc-139">Helyezze be a kódját tooquery SQL-adatbázis</span><span class="sxs-lookup"><span data-stu-id="788dc-139">Insert code tooquery SQL database</span></span>
1. <span data-ttu-id="788dc-140">Váltás túl (vagy ha szükséges) **Program.cs**</span><span class="sxs-lookup"><span data-stu-id="788dc-140">Switch too(or open if necessary) **Program.cs**</span></span>

2. <span data-ttu-id="788dc-141">Cserélje le a hello tartalmát **Program.cs** az alábbi kód, és adja hozzá a megfelelő értékeket hello a kiszolgálóhoz, az adatbázis, a felhasználó és a jelszó hello.</span><span class="sxs-lookup"><span data-stu-id="788dc-141">Replace hello contents of **Program.cs** with hello following code and add hello appropriate values for your server, database, user, and password.</span></span>

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

## <a name="run-hello-code"></a><span data-ttu-id="788dc-142">Hello kód futtatása</span><span class="sxs-lookup"><span data-stu-id="788dc-142">Run hello code</span></span>

1. <span data-ttu-id="788dc-143">Nyomja le az **F5** toorun hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="788dc-143">Press **F5** toorun hello application.</span></span>
2. <span data-ttu-id="788dc-144">Győződjön meg arról, hogy hello felső 20 sorokat ad vissza, és zárja be hello alkalmazás ablak.</span><span class="sxs-lookup"><span data-stu-id="788dc-144">Verify that hello top 20 rows are returned and then close hello application window.</span></span>

## <a name="next-steps"></a><span data-ttu-id="788dc-145">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="788dc-145">Next steps</span></span>

- <span data-ttu-id="788dc-146">Ismerje meg, hogyan túl[kapcsolódás és lekérdezés az Azure SQL-adatbázis használata a .NET core](sql-database-connect-query-dotnet-core.md) a Windows/Linux/macOS.</span><span class="sxs-lookup"><span data-stu-id="788dc-146">Learn how too[connect and query an Azure SQL database using .NET core](sql-database-connect-query-dotnet-core.md) on Windows/Linux/macOS.</span></span>  
- <span data-ttu-id="788dc-147">További tudnivalók [Ismerkedés a Windows/Linux/macOS hello parancssor használatával a .NET Core](/dotnet/core/tutorials/using-with-xplat-cli).</span><span class="sxs-lookup"><span data-stu-id="788dc-147">Learn about [Getting started with .NET Core on Windows/Linux/macOS using hello command line](/dotnet/core/tutorials/using-with-xplat-cli).</span></span>
- <span data-ttu-id="788dc-148">Ismerje meg, hogyan túl[tervezése az első Azure SQL-adatbázis SSMS használatával](sql-database-design-first-database.md) vagy [tervezése az első Azure SQL Database adatbázishoz .NET használatával](sql-database-design-first-database-csharp.md).</span><span class="sxs-lookup"><span data-stu-id="788dc-148">Learn how too[Design your first Azure SQL database using SSMS](sql-database-design-first-database.md) or [Design your first Azure SQL database using .NET](sql-database-design-first-database-csharp.md).</span></span>
- <span data-ttu-id="788dc-149">A .NET-ről a [.NET dokumentációjában](https://docs.microsoft.com/dotnet/) talál további információt.</span><span class="sxs-lookup"><span data-stu-id="788dc-149">For more information about .NET, see [.NET documentation](https://docs.microsoft.com/dotnet/).</span></span>
