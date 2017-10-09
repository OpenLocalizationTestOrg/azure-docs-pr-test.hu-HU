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
# <a name="use-net-core-c-tooquery-an-azure-sql-database"></a>A .NET Core (C#) tooquery Azure SQL-adatbázis használata

Gyors üzembe helyezési oktatóanyag bemutatja, hogyan toouse [.NET Core](https://www.microsoft.com/net/) a Windows/Linux/macOS toocreate egy C# programban tooconnect tooan Azure SQL-adatbázis és a Transact-SQL utasítás tooquery adatok felhasználásával.

## <a name="prerequisites"></a>Előfeltételek

toocomplete a gyors üzembe helyezési útmutató, győződjön meg arról, hogy a következő hello:

- Azure SQL Database-adatbázis. A gyors üzembe helyezési hoznak létre az egyik a gyors üzembe helyezések hello erőforrást használ: 

   - [DB létrehozása – portál](sql-database-get-started-portal.md)
   - [DB létrehozása – CLI](sql-database-get-started-cli.md)
   - [DB létrehozása – PowerShell](sql-database-get-started-powershell.md)

- A [kiszolgálószintű tűzfalszabály](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) hello nyilvános IP-cím hello számítógép használja a gyors üzembe helyezési útmutató.
- Telepítette az [az operációs rendszerének megfelelő .NET Core-t](https://www.microsoft.com/net/core). 

## <a name="sql-server-connection-information"></a>Az SQL-kiszolgáló kapcsolatadatai

Hello kapcsolat szükséges információkat tooconnect toohello Azure SQL adatbázis beolvasása. Hello teljes kiszolgálónév, az adatbázisnév és a bejelentkezési adatok a következő eljárások hello kell.

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).
2. Válassza ki **SQL-adatbázisok** hello bal oldali menüben kattintson a hello adatbázis **SQL-adatbázisok** lap. 
3. A hello **áttekintése** lap az adatbázishoz, tekintse át hello teljesen minősített kiszolgáló neve, ahogy az a következő kép hello. Hello server name toobring hello másolatot is mutat **toocopy kattintson** lehetőséget. 

   ![server-name](./media/sql-database-connect-query-dotnet/server-name.png) 

4. Ha az Azure SQL Database kiszolgáló bejelentkezési adatok, keresse meg a toohello SQL adatbázis-kiszolgáló lapon tooview hello server admin neve. Szükség esetén alaphelyzetbe állíthatja a hello jelszót.

5. Kattintson az: **Adatbázis-kapcsolati karakterláncok megjelenítése** elemre.

6. Felülvizsgálati hello teljes **ADO.NET** kapcsolati karakterláncot.

    ![ADO.NET kapcsolati karakterlánc](./media/sql-database-connect-query-dotnet/adonet-connection-string.png)

> [!IMPORTANT]
> Rendelkeznie kell egy tűzfalszabályt hello nyilvános IP-cím hello számítógép, amelyen ez az oktatóanyag végrehajtása helyen. Ha egy másik számítógépen vagy egy másik nyilvános IP-címet, hozzon létre egy [kiszolgálószintű tűzfal szabály használatával hello Azure-portálon](sql-database-get-started-portal.md#create-a-server-level-firewall-rule). 
>
  
## <a name="create-a-new-net-project"></a>Új .NET-projekt létrehozása

1. Nyisson meg egy parancssort, és hozzon létre egy *sqltest* nevű mappát. Keresse meg a létrehozott, és futtassa a következő parancs hello toohello mappába:

    ```
    dotnet new console
    ```

2. Nyissa meg ***sqltest.csproj*** a kedvenc szövegszerkesztőjével, és adja hozzá a System.Data.SqlClient a függőség beállításához a következő kód hello használata:

    ```xml
    <ItemGroup>
        <PackageReference Include="System.Data.SqlClient" Version="4.3.0" />
    </ItemGroup>
    ```

## <a name="insert-code-tooquery-sql-database"></a>Helyezze be a kódját tooquery SQL-adatbázis

1. A fejlesztői környezetben vagy egy tetszőleges szövegszerkesztőben nyissa meg a **Program.cs** fájlt.

2. Cserélje ki hello tartalmát hello az alábbi kód, és adja hozzá a megfelelő értékeket hello a kiszolgáló, az adatbázis, a felhasználó és a jelszavát.

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

## <a name="run-hello-code"></a>Hello kód futtatása

1. Hello parancssorban futtassa a következő parancsok hello:

   ```csharp
   dotnet restore
   dotnet run
   ```

2. Győződjön meg arról, hogy hello felső 20 sorokat ad vissza, és zárja be hello alkalmazás ablak.


## <a name="next-steps"></a>Következő lépések

- [Ismerkedés a Windows/Linux/macOS hello parancssor használatával a .NET Core](/dotnet/core/tutorials/using-with-xplat-cli).
- Ismerje meg, hogyan túl[kapcsolódás és lekérdezés hello .NET-keretrendszer és a Visual Studio használatával Azure SQL-adatbázis](sql-database-connect-query-dotnet-visual-studio.md).  
- Ismerje meg, hogyan túl[tervezése az első Azure SQL-adatbázis SSMS használatával](sql-database-design-first-database.md) vagy [tervezése az első Azure SQL Database adatbázishoz .NET használatával](sql-database-design-first-database-csharp.md).
- A .NET-ről a [.NET dokumentációjában](https://docs.microsoft.com/dotnet/) talál további információt.
