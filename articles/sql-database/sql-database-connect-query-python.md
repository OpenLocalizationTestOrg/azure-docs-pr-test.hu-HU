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
# <a name="use-python-tooquery-an-azure-sql-database"></a>Python tooquery Azure SQL-adatbázis használata

 A gyors üzembe helyezési bemutatja, hogyan toouse [Python](https://python.org) tooconnect tooan Azure SQL adatbázis- és a Transact-SQL utasítás tooquery adatok felhasználásával.

## <a name="prerequisites"></a>Előfeltételek

toocomplete a gyors üzembe helyezési útmutató, győződjön meg arról, hogy a következő hello:

- Azure SQL Database-adatbázis. A gyors üzembe helyezési hoznak létre az egyik a gyors üzembe helyezések hello erőforrást használ: 

   - [DB létrehozása – portál](sql-database-get-started-portal.md)
   - [DB létrehozása – CLI](sql-database-get-started-cli.md)
   - [DB létrehozása – PowerShell](sql-database-get-started-powershell.md)

- A [kiszolgálószintű tűzfalszabály](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) hello nyilvános IP-cím hello számítógép használja a gyors üzembe helyezési útmutató.

- Telepítette a Pythont és az operációs rendszerének megfelelő kapcsolódó szoftvereket.

    - **MacOS**: Homebrew telepítése és a Python, hello ODBC-illesztőprogram és az Sqlcmd ESZKÖZRE telepíteni, és telepítse az SQL Server hello Python illesztőprogram. Lásd az [1.2, 1.3 és 2.1 lépést](https://www.microsoft.com/sql-server/developer-get-started/python/mac/).
    - **Ubuntu**: Python telepítése és más szükséges csomagokat, majd a telepítés hello Python illesztőprogram az SQL Server. Lásd az [1.2, 1.3 és 2.1 lépést](https://www.microsoft.com/sql-server/developer-get-started/python/ubuntu/).
    - **Windows**: hello (környezeti változó konfigurálta az Ön) Python legújabb verziójának telepítéséhez, telepítse az ODBC-illesztőprogram hello és SQLCMD és telepítse az SQL Server hello Python illesztőprogram. Lásd az [1.2, 1.3 és 2.1 lépést](https://www.microsoft.com/sql-server/developer-get-started/python/windows/). 

## <a name="sql-server-connection-information"></a>Az SQL-kiszolgáló kapcsolatadatai

Hello kapcsolat szükséges információkat tooconnect toohello Azure SQL adatbázis beolvasása. Hello teljes kiszolgálónév, az adatbázisnév és a bejelentkezési adatok a következő eljárások hello kell.

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).
2. Válassza ki **SQL-adatbázisok** hello bal oldali menüben kattintson a hello adatbázis **SQL-adatbázisok** lap. 
3. A hello **áttekintése** lap az adatbázishoz, tekintse át hello teljesen minősített kiszolgáló neve, ahogy az a következő kép hello. Hello server name toobring hello másolatot is mutat **toocopy kattintson** lehetőséget.  

   ![server-name](./media/sql-database-connect-query-dotnet/server-name.png) 

4. Ha elfelejti a kiszolgálói bejelentkezési adatok, keresse meg a toohello SQL adatbázis-kiszolgáló lapon tooview hello server admin neve és, ha szükséges, állítsa vissza a hello jelszót.     
    
## <a name="insert-code-tooquery-sql-database"></a>Helyezze be a kódját tooquery SQL-adatbázis 

1. Egy tetszőleges szövegszerkesztőben hozza létre a **sqltest.py** nevű új fájlt.  

2. Cserélje ki hello tartalmát hello az alábbi kód, és adja hozzá a megfelelő értékeket hello a kiszolgáló, az adatbázis, a felhasználó és a jelszavát.

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

## <a name="run-hello-code"></a>Hello kód futtatása

1. Hello parancssorban futtassa a következő parancsok hello:

   ```Python
   python sqltest.py
   ```

2. Győződjön meg arról, hogy hello felső 20 sorokat ad vissza, és zárja be hello alkalmazás ablak.

## <a name="next-steps"></a>Következő lépések

- [Az első SQL Database-adatbázis megtervezése](sql-database-design-first-database.md)
- [SQL Serverre készült Microsoft Python-illesztőprogramok](https://docs.microsoft.com/sql/connect/python/python-driver-for-sql-server/)
- [Python fejlesztői központ](https://azure.microsoft.com/develop/python/?v=17.23h)

