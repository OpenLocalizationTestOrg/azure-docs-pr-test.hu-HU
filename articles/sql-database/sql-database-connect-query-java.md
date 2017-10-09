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
# <a name="use-java-tooquery-an-azure-sql-database"></a><span data-ttu-id="49292-103">Java tooquery Azure SQL-adatbázis használata</span><span class="sxs-lookup"><span data-stu-id="49292-103">Use Java tooquery an Azure SQL database</span></span>

<span data-ttu-id="49292-104">A gyors üzembe helyezési bemutatja, hogyan toouse [Java](https://docs.microsoft.com/sql/connect/jdbc/microsoft-jdbc-driver-for-sql-server) tooconnect tooan Azure SQL-adatbázis, majd a Transact-SQL utasítás tooquery adatokat.</span><span class="sxs-lookup"><span data-stu-id="49292-104">This quick start demonstrates how toouse [Java](https://docs.microsoft.com/sql/connect/jdbc/microsoft-jdbc-driver-for-sql-server) tooconnect tooan Azure SQL database and then use Transact-SQL statements tooquery data.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="49292-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="49292-105">Prerequisites</span></span>

<span data-ttu-id="49292-106">toocomplete a gyors üzembe helyezési útmutató, győződjön meg arról, hogy a következő előfeltételek hello:</span><span class="sxs-lookup"><span data-stu-id="49292-106">toocomplete this quick start tutorial, make sure you have hello following prerequisites:</span></span>

- <span data-ttu-id="49292-107">Azure SQL Database-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="49292-107">An Azure SQL database.</span></span> <span data-ttu-id="49292-108">A gyors üzembe helyezési hoznak létre az egyik a gyors üzembe helyezések hello erőforrást használ:</span><span class="sxs-lookup"><span data-stu-id="49292-108">This quick start uses hello resources created in one of these quick starts:</span></span> 

   - [<span data-ttu-id="49292-109">DB létrehozása – portál</span><span class="sxs-lookup"><span data-stu-id="49292-109">Create DB - Portal</span></span>](sql-database-get-started-portal.md)
   - [<span data-ttu-id="49292-110">DB létrehozása – CLI</span><span class="sxs-lookup"><span data-stu-id="49292-110">Create DB - CLI</span></span>](sql-database-get-started-cli.md)
   - [<span data-ttu-id="49292-111">DB létrehozása – PowerShell</span><span class="sxs-lookup"><span data-stu-id="49292-111">Create DB - PowerShell</span></span>](sql-database-get-started-powershell.md)

- <span data-ttu-id="49292-112">A [kiszolgálószintű tűzfalszabály](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) hello nyilvános IP-cím hello számítógép használja a gyors üzembe helyezési útmutató.</span><span class="sxs-lookup"><span data-stu-id="49292-112">A [server-level firewall rule](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) for hello public IP address of hello computer you use for this quick start tutorial.</span></span>

- <span data-ttu-id="49292-113">Telepítette a Javát és az operációs rendszerének megfelelő kapcsolódó szoftvereket.</span><span class="sxs-lookup"><span data-stu-id="49292-113">You have installed Java and related software for your operating system.</span></span>

    - <span data-ttu-id="49292-114">**MacOS**: Telepítse a Homebrew-t és a Javát, majd telepítse a Mavent.</span><span class="sxs-lookup"><span data-stu-id="49292-114">**MacOS**: Install Homebrew and Java, and then install Maven.</span></span> <span data-ttu-id="49292-115">Lásd az [1.2 és 1.3 lépést](https://www.microsoft.com/sql-server/developer-get-started/java/mac/).</span><span class="sxs-lookup"><span data-stu-id="49292-115">See [Step 1.2 and 1.3](https://www.microsoft.com/sql-server/developer-get-started/java/mac/).</span></span>
    - <span data-ttu-id="49292-116">**Ubuntu**: hello Java fejlesztői készlet telepítése, és telepítse a Maven.</span><span class="sxs-lookup"><span data-stu-id="49292-116">**Ubuntu**:  Install hello Java Development Kit, and install Maven.</span></span> <span data-ttu-id="49292-117">Lásd az [1.2, 1.3 és 1.4 lépést](https://www.microsoft.com/sql-server/developer-get-started/java/ubuntu/).</span><span class="sxs-lookup"><span data-stu-id="49292-117">See [Step 1.2, 1.3, and 1.4](https://www.microsoft.com/sql-server/developer-get-started/java/ubuntu/).</span></span>
    - <span data-ttu-id="49292-118">**Windows**: hello Java fejlesztői készlet és a Maven telepítése.</span><span class="sxs-lookup"><span data-stu-id="49292-118">**Windows**: Install hello Java Development Kit, and Maven.</span></span> <span data-ttu-id="49292-119">Lásd az [1.2 és 1.3 lépést](https://www.microsoft.com/sql-server/developer-get-started/java/windows/).</span><span class="sxs-lookup"><span data-stu-id="49292-119">See [Step 1.2 and 1.3](https://www.microsoft.com/sql-server/developer-get-started/java/windows/).</span></span>    

## <a name="sql-server-connection-information"></a><span data-ttu-id="49292-120">Az SQL-kiszolgáló kapcsolatadatai</span><span class="sxs-lookup"><span data-stu-id="49292-120">SQL server connection information</span></span>

<span data-ttu-id="49292-121">Hello kapcsolat szükséges információkat tooconnect toohello Azure SQL adatbázis beolvasása.</span><span class="sxs-lookup"><span data-stu-id="49292-121">Get hello connection information needed tooconnect toohello Azure SQL database.</span></span> <span data-ttu-id="49292-122">Hello teljes kiszolgálónév, az adatbázisnév és a bejelentkezési adatok a következő eljárások hello kell.</span><span class="sxs-lookup"><span data-stu-id="49292-122">You will need hello fully qualified server name, database name, and login information in hello next procedures.</span></span>

1. <span data-ttu-id="49292-123">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="49292-123">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="49292-124">Válassza ki **SQL-adatbázisok** hello bal oldali menüben kattintson a hello adatbázis **SQL-adatbázisok** lap.</span><span class="sxs-lookup"><span data-stu-id="49292-124">Select **SQL Databases** from hello left-hand menu, and click your database on hello **SQL databases** page.</span></span> 
3. <span data-ttu-id="49292-125">A hello **áttekintése** az adatbázis lapján tekintse át a hello teljes kiszolgálónév, ahogy az a következő kép hello: hello server name toobring hello másolatot is mutat **toocopy kattintson** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="49292-125">On hello **Overview** page for your database, review hello fully qualified server name as shown in hello following image: You can hover over hello server name toobring up hello **Click toocopy** option.</span></span>  

   ![server-name](./media/sql-database-connect-query-dotnet/server-name.png) 

4. <span data-ttu-id="49292-127">Ha elfelejti a kiszolgálói bejelentkezési adatok, keresse meg a toohello SQL adatbázis-kiszolgáló lapon tooview hello server admin neve.</span><span class="sxs-lookup"><span data-stu-id="49292-127">If you forget your server login information, navigate toohello SQL Database server page tooview hello server admin name.</span></span>  <span data-ttu-id="49292-128">Ha szükséges, jelszó-átállítási hello.</span><span class="sxs-lookup"><span data-stu-id="49292-128">If necessary, reset hello password.</span></span>     

## <a name="create-maven-project-and-dependencies"></a><span data-ttu-id="49292-129">**Maven-projekt és függőségek létrehozása**</span><span class="sxs-lookup"><span data-stu-id="49292-129">**Create Maven project and dependencies**</span></span>
1. <span data-ttu-id="49292-130">A Terminálszolgáltatások hello, hozzon létre egy új Maven project nevű **sqltest**.</span><span class="sxs-lookup"><span data-stu-id="49292-130">From hello terminal, create a new Maven project called **sqltest**.</span></span> 

   ```bash
   mvn archetype:generate "-DgroupId=com.sqldbsamples" "-DartifactId=sqltest" "-DarchetypeArtifactId=maven-archetype-quickstart" "-Dversion=1.0.0"
   ```

2. <span data-ttu-id="49292-131">Amikor a rendszer kéri, írja be az **Y** karaktert.</span><span class="sxs-lookup"><span data-stu-id="49292-131">Enter **Y** when prompted.</span></span>
3. <span data-ttu-id="49292-132">Módosítsa a könyvtárat túl**sqltest** , és nyissa meg ***pom.xml*** a kedvenc szövegszerkesztőjével.</span><span class="sxs-lookup"><span data-stu-id="49292-132">Change directory too**sqltest** and open ***pom.xml*** with your favorite text editor.</span></span>  <span data-ttu-id="49292-133">Adja hozzá a hello **Microsoft SQL Server JDBC-illesztőt** használatával tooyour projekt függőségek hello következő kódot:</span><span class="sxs-lookup"><span data-stu-id="49292-133">Add hello **Microsoft JDBC Driver for SQL Server** tooyour project's dependencies using hello following code:</span></span>

   ```xml
   <dependency>
       <groupId>com.microsoft.sqlserver</groupId>
       <artifactId>mssql-jdbc</artifactId>
       <version>6.2.1.jre8</version>
   </dependency>
   ```

4. <span data-ttu-id="49292-134">Emellett a ***pom.xml***, adja hozzá a következő tulajdonságok tooyour projekt hello.</span><span class="sxs-lookup"><span data-stu-id="49292-134">Also in ***pom.xml***, add hello following properties tooyour project.</span></span>  <span data-ttu-id="49292-135">Ha még nem rendelkezik a Tulajdonságok szakaszát, hello függőségek után adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="49292-135">If you don't have a properties section, you can add it after hello dependencies.</span></span>

   ```xml
   <properties>
       <maven.compiler.source>1.8</maven.compiler.source>
       <maven.compiler.target>1.8</maven.compiler.target>
   </properties>
   ```

5. <span data-ttu-id="49292-136">Mentse és zárja be a ***pom.xml*** fájlt.</span><span class="sxs-lookup"><span data-stu-id="49292-136">Save and close ***pom.xml***.</span></span>

## <a name="insert-code-tooquery-sql-database"></a><span data-ttu-id="49292-137">Helyezze be a kódját tooquery SQL-adatbázis</span><span class="sxs-lookup"><span data-stu-id="49292-137">Insert code tooquery SQL database</span></span>

1. <span data-ttu-id="49292-138">A Maven-projektben már rendelkezik egy ***App.java*** nevű fájllal a következő helyen: ..\sqltest\src\main\java\com\sqlsamples\App.java</span><span class="sxs-lookup"><span data-stu-id="49292-138">You should already have a file called ***App.java*** in your Maven project located at:  ..\sqltest\src\main\java\com\sqlsamples\App.java</span></span>

2. <span data-ttu-id="49292-139">Nyissa meg a hello fájl- és csere hello következőre tartalmának code, és adja hozzá a hello megfelelő értékeket a kiszolgáló, az adatbázis, a felhasználó és a jelszavát.</span><span class="sxs-lookup"><span data-stu-id="49292-139">Open hello file and replace its contents with hello following code and add hello appropriate values for your server, database, user, and password.</span></span>

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

## <a name="run-hello-code"></a><span data-ttu-id="49292-140">Hello kód futtatása</span><span class="sxs-lookup"><span data-stu-id="49292-140">Run hello code</span></span>

1. <span data-ttu-id="49292-141">Hello parancssorban futtassa a következő parancsok hello:</span><span class="sxs-lookup"><span data-stu-id="49292-141">At hello command prompt, run hello following commands:</span></span>

   ```bash
   mvn package
   mvn -q exec:java "-Dexec.mainClass=com.sqldbsamples.App"
   ```

2. <span data-ttu-id="49292-142">Győződjön meg arról, hogy hello felső 20 sorokat ad vissza, és zárja be hello alkalmazás ablak.</span><span class="sxs-lookup"><span data-stu-id="49292-142">Verify that hello top 20 rows are returned and then close hello application window.</span></span>


## <a name="next-steps"></a><span data-ttu-id="49292-143">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="49292-143">Next steps</span></span>
- [<span data-ttu-id="49292-144">Az első SQL Database-adatbázis megtervezése</span><span class="sxs-lookup"><span data-stu-id="49292-144">Design your first Azure SQL database</span></span>](sql-database-design-first-database.md)
- [<span data-ttu-id="49292-145">Microsoft JDBC-illesztőprogram SQL Serverhez</span><span class="sxs-lookup"><span data-stu-id="49292-145">Microsoft JDBC Driver for SQL Server</span></span>](https://github.com/microsoft/mssql-jdbc)
- [<span data-ttu-id="49292-146">Problémák jelentése/kérdések</span><span class="sxs-lookup"><span data-stu-id="49292-146">Report issues/ask questions</span></span>](https://github.com/microsoft/mssql-jdbc/issues)

