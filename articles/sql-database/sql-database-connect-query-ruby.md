---
title: "Ruby használata Azure SQL Database-adatbázis lekérdezéséhez | Microsoft Docs"
description: "Ez a témakör bemutatja, hogyan használhatja a Rubyt egy Azure SQL Database-adatbázishoz csatlakozó program létrehozásához, és hogyan hajthat végre lekérdezést Transact-SQL-utasításokkal."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 94fec528-58ba-4352-ba0d-25ae4b273e90
ms.service: sql-database
ms.custom: mvc,develop apps
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: quickstart
ms.date: 07/15/2017
ms.author: carlrab
ms.openlocfilehash: 3427d216540451bc10b968f866d0fce0f6df3c54
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 11/29/2017
---
# <a name="use-ruby-to-query-an-azure-sql-database"></a>Ruby használata Azure SQL Database-adatbázis lekérdezéséhez

Ez a rövid útmutató azt ismerteti, hogyan használható a [Ruby](https://www.ruby-lang.org) egy olyan program létrehozásához, amely egy Azure SQL Database-adatbázishoz csatlakozik, és hogyan lehet Transact-SQL-utasítások használatával adatokat lekérdezni.

## <a name="prerequisites"></a>Előfeltételek

A rövid útmutató végrehajtásához győződjön meg arról, hogy megfelel az alábbi előfeltételeknek:

[!INCLUDE [prerequisites-create-db](../../includes/sql-database-connect-query-prerequisites-create-db-includes.md)]

- A rövid útmutatóhoz használt számítógép nyilvános IP-címére vonatkozó [kiszolgálószintű tűzfalszabály](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).

- Telepítette a Rubyt és az operációs rendszerének megfelelő kapcsolódó szoftvereket.
    - **MacOS**: Telepítse a Homebrew-t, az rbenv-t és a ruby-buildet, a Rubyt, végül pedig a FreeTDS-t. Lásd az [1.2, 1.3, 1.4 és 1.5 lépést](https://www.microsoft.com/sql-server/developer-get-started/ruby/mac/).
    - **Ubuntu**: Telepítse a Ruby előfeltételeit, az rbenv-t és a ruby-buildet, a Rubyt, végül pedig a FreeTDS-t. Lásd az [1.2, 1.3, 1.4 és 1.5 lépést](https://www.microsoft.com/sql-server/developer-get-started/ruby/ubuntu/).

## <a name="sql-server-connection-information"></a>Az SQL-kiszolgáló kapcsolatadatai

[!INCLUDE [prerequisites-server-connection-info](../../includes/sql-database-connect-query-prerequisites-server-connection-info-includes.md)]

> [!IMPORTANT]
> Rendelkeznie kell egy tűzfalszabállyal azon számítógép nyilvános IP-címéhez, amelyen ezt az oktatóanyagot elvégzi. Ha más számítógépet használ, vagy más nyilvános IP-címe van, hozzon létre egy [kiszolgálószintű tűzfalszabályt az Azure Portalon](sql-database-get-started-portal.md#create-a-server-level-firewall-rule). 

## <a name="insert-code-to-query-sql-database"></a>Kód beszúrása SQL-adatbázis lekérdezéséhez

1. Egy tetszőleges szövegszerkesztőben hozzon létre egy **sqltest.rb** nevű új fájlt

2. Cserélje le a tartalmat a következő kódra, és adja meg a kiszolgáló és az adatbázis megfelelő adatait, valamint a felhasználót és a jelszót.

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

## <a name="run-the-code"></a>A kód futtatása

1. Futtassa az alábbi parancsokat a parancssorban:

   ```bash
   ruby sqltest.rb
   ```

2. Győződjön meg arról, hogy a parancssori felület visszaadta az első 20 sort, majd zárja be az alkalmazásablakot.


## <a name="next-steps"></a>Következő lépések
- [Az első SQL Database-adatbázis megtervezése](sql-database-design-first-database.md)
- [GitHub-adattár a TinyTDS-hez](https://github.com/rails-sqlserver/tiny_tds)
- [Problémák jelentése és kérdezés a TinyTDS-sel kapcsolatban](https://github.com/rails-sqlserver/tiny_tds/issues)
- [Ruby-illesztőprogramok az SQL Serverhez](https://docs.microsoft.com/sql/connect/ruby/ruby-driver-for-sql-server/)
