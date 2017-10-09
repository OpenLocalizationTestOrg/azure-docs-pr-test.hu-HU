---
title: aaaUse Java tooquery Azure SQL Database |} Microsoft Docs
description: "Ez a témakör bemutatja, hogyan toouse Java toocreate egy programot, amely kapcsolatot tooan Azure SQL Database és a lekérdezés Transact-SQL utasítás használatával."
services: sql-database
documentationcenter: 
author: ajlam
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,develop apps
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: hero-article
ms.date: 07/10/2017
ms.author: andrela
ms.openlocfilehash: f014edbe38ca0e7b6e43f4eb4d2e53d3561bf3e7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-java-tooquery-an-azure-sql-database"></a>Java tooquery Azure SQL-adatbázis használata

A gyors üzembe helyezési bemutatja, hogyan toouse [Java](https://docs.microsoft.com/sql/connect/jdbc/microsoft-jdbc-driver-for-sql-server) tooconnect tooan Azure SQL-adatbázis, majd a Transact-SQL utasítás tooquery adatokat.

## <a name="prerequisites"></a>Előfeltételek

toocomplete a gyors üzembe helyezési útmutató, győződjön meg arról, hogy a következő előfeltételek hello:

- Azure SQL Database-adatbázis. A gyors üzembe helyezési hoznak létre az egyik a gyors üzembe helyezések hello erőforrást használ: 

   - [DB létrehozása – portál](sql-database-get-started-portal.md)
   - [DB létrehozása – CLI](sql-database-get-started-cli.md)
   - [DB létrehozása – PowerShell](sql-database-get-started-powershell.md)

- A [kiszolgálószintű tűzfalszabály](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) hello nyilvános IP-cím hello számítógép használja a gyors üzembe helyezési útmutató.

- Telepítette a Javát és az operációs rendszerének megfelelő kapcsolódó szoftvereket.

    - **MacOS**: Telepítse a Homebrew-t és a Javát, majd telepítse a Mavent. Lásd az [1.2 és 1.3 lépést](https://www.microsoft.com/sql-server/developer-get-started/java/mac/).
    - **Ubuntu**: hello Java fejlesztői készlet telepítése, és telepítse a Maven. Lásd az [1.2, 1.3 és 1.4 lépést](https://www.microsoft.com/sql-server/developer-get-started/java/ubuntu/).
    - **Windows**: hello Java fejlesztői készlet és a Maven telepítése. Lásd az [1.2 és 1.3 lépést](https://www.microsoft.com/sql-server/developer-get-started/java/windows/).    

## <a name="sql-server-connection-information"></a>Az SQL-kiszolgáló kapcsolatadatai

Hello kapcsolat szükséges információkat tooconnect toohello Azure SQL adatbázis beolvasása. Hello teljes kiszolgálónév, az adatbázisnév és a bejelentkezési adatok a következő eljárások hello kell.

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).
2. Válassza ki **SQL-adatbázisok** hello bal oldali menüben kattintson a hello adatbázis **SQL-adatbázisok** lap. 
3. A hello **áttekintése** az adatbázis lapján tekintse át a hello teljes kiszolgálónév, ahogy az a következő kép hello: hello server name toobring hello másolatot is mutat **toocopy kattintson** lehetőséget.  

   ![server-name](./media/sql-database-connect-query-dotnet/server-name.png) 

4. Ha elfelejti a kiszolgálói bejelentkezési adatok, keresse meg a toohello SQL adatbázis-kiszolgáló lapon tooview hello server admin neve.  Ha szükséges, jelszó-átállítási hello.     

## <a name="create-maven-project-and-dependencies"></a>**Maven-projekt és függőségek létrehozása**
1. A Terminálszolgáltatások hello, hozzon létre egy új Maven project nevű **sqltest**. 

   ```bash
   mvn archetype:generate "-DgroupId=com.sqldbsamples" "-DartifactId=sqltest" "-DarchetypeArtifactId=maven-archetype-quickstart" "-Dversion=1.0.0"
   ```

2. Amikor a rendszer kéri, írja be az **Y** karaktert.
3. Módosítsa a könyvtárat túl**sqltest** , és nyissa meg ***pom.xml*** a kedvenc szövegszerkesztőjével.  Adja hozzá a hello **Microsoft SQL Server JDBC-illesztőt** használatával tooyour projekt függőségek hello következő kódot:

   ```xml
   <dependency>
       <groupId>com.microsoft.sqlserver</groupId>
       <artifactId>mssql-jdbc</artifactId>
       <version>6.2.1.jre8</version>
   </dependency>
   ```

4. Emellett a ***pom.xml***, adja hozzá a következő tulajdonságok tooyour projekt hello.  Ha még nem rendelkezik a Tulajdonságok szakaszát, hello függőségek után adhat hozzá.

   ```xml
   <properties>
       <maven.compiler.source>1.8</maven.compiler.source>
       <maven.compiler.target>1.8</maven.compiler.target>
   </properties>
   ```

5. Mentse és zárja be a ***pom.xml*** fájlt.

## <a name="insert-code-tooquery-sql-database"></a>Helyezze be a kódját tooquery SQL-adatbázis

1. A Maven-projektben már rendelkezik egy ***App.java*** nevű fájllal a következő helyen: ..\sqltest\src\main\java\com\sqlsamples\App.java

2. Nyissa meg a hello fájl- és csere hello következőre tartalmának code, és adja hozzá a hello megfelelő értékeket a kiszolgáló, az adatbázis, a felhasználó és a jelszavát.

   ```java
   package com.sqldbsamples;

   import java.sql.Connection;
   import java.sql.Statement;
   import java.sql.PreparedStatement;
   import java.sql.ResultSet;
   import java.sql.DriverManager;

   public class App {

    public static void main(String[] args) {
    
        // Connect toodatabase
           String hostName = "your_server.database.windows.net";
           String dbName = "your_database";
           String user = "your_username";
           String password = "your_password";
           String url = String.format("jdbc:sqlserver://%s:1433;database=%s;user=%s;password=%s;encrypt=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;", hostName, dbName, user, password);
           Connection connection = null;

           try {
                   connection = DriverManager.getConnection(url);
                   String schema = connection.getSchema();
                   System.out.println("Successful connection - Schema: " + schema);

                   System.out.println("Query data example:");
                   System.out.println("=========================================");

                   // Create and execute a SELECT SQL statement.
                   String selectSql = "SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName " 
                       + "FROM [SalesLT].[ProductCategory] pc "  
                       + "JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid";
                
                   try (Statement statement = connection.createStatement();
                       ResultSet resultSet = statement.executeQuery(selectSql)) {

                           // Print results from select statement
                           System.out.println("Top 20 categories:");
                           while (resultSet.next())
                           {
                               System.out.println(resultSet.getString(1) + " "
                                   + resultSet.getString(2));
                           }
                    connection.close();
                   }                   
           }
           catch (Exception e) {
                   e.printStackTrace();
           }
       }
   }
   ```

## <a name="run-hello-code"></a>Hello kód futtatása

1. Hello parancssorban futtassa a következő parancsok hello:

   ```bash
   mvn package
   mvn -q exec:java "-Dexec.mainClass=com.sqldbsamples.App"
   ```

2. Győződjön meg arról, hogy hello felső 20 sorokat ad vissza, és zárja be hello alkalmazás ablak.


## <a name="next-steps"></a>Következő lépések
- [Az első SQL Database-adatbázis megtervezése](sql-database-design-first-database.md)
- [Microsoft JDBC-illesztőprogram SQL Serverhez](https://github.com/microsoft/mssql-jdbc)
- [Problémák jelentése/kérdések](https://github.com/microsoft/mssql-jdbc/issues)

