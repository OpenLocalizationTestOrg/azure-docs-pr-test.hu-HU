---
title: "aaaImplement több-bérlős SaaS-alkalmazás az Azure SQL Database |} Microsoft Docs"
description: "Több-bérlős SaaS-alkalmazás az Azure SQL Database megvalósításához."
services: sql-database
documentationcenter: 
author: AyoOlubeko
manager: jhubbard
editor: monicar
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,scale out apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 05/08/2017
ms.author: AyoOlubek
ms.openlocfilehash: b87b8f296e2c20a8f674b56375f43fdc92df76d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="implement-a-multi-tenant-saas-application-using-azure-sql-database"></a><span data-ttu-id="c5c5c-103">Egy több-bérlős SaaS-alkalmazás használatával az Azure SQL Database megvalósítása</span><span class="sxs-lookup"><span data-stu-id="c5c5c-103">Implement a multi-tenant SaaS application using Azure SQL Database</span></span>

<span data-ttu-id="c5c5c-104">Egy több-bérlős alkalmazás üzemeltetett felhőalapú környezetek alkalmazás, és azonos beállítása szolgáltatások toohundreds vagy a bérlő nem megosztása, vagy másik adatok megtekintéséhez több ezer hello biztosít.</span><span class="sxs-lookup"><span data-stu-id="c5c5c-104">A multi-tenant application is an application hosted in a cloud environment and that provides hello same set of services toohundreds or thousands of tenants who do not share or see each other’s data.</span></span> <span data-ttu-id="c5c5c-105">Példa: egy SaaS-alkalmazás, amely a felhőben üzemeltetett környezetben a szolgáltatások tootenants biztosít.</span><span class="sxs-lookup"><span data-stu-id="c5c5c-105">An example is an SaaS application that provides services tootenants in a cloud-hosted environment.</span></span> <span data-ttu-id="c5c5c-106">Ez a modell hello adatokat elkülöníti az egyes bérlők számára, és optimalizálja a hello terjesztési költség az erőforrások.</span><span class="sxs-lookup"><span data-stu-id="c5c5c-106">This model isolates hello data for each tenant and optimizes hello distribution of resources for cost.</span></span> 

<span data-ttu-id="c5c5c-107">Ez az oktatóanyag bemutatja, hogyan toocreate egy Azure SQL Database használatával több-bérlős SaaS-alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="c5c5c-107">This tutorial demonstrates how toocreate a multi-tenant SaaS application using Azure SQL Database.</span></span>

<span data-ttu-id="c5c5c-108">Ebben az oktatóanyagban elsajátíthatja:</span><span class="sxs-lookup"><span data-stu-id="c5c5c-108">In this tutorial, you will learn to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="c5c5c-109">Egy több-bérlős SaaS-alkalmazáshoz, hello adatbázis / bérlői minta használatával egy adatbázis-környezet toosupport beállítása</span><span class="sxs-lookup"><span data-stu-id="c5c5c-109">Set up a database environment toosupport a multi-tenant SaaS application, using hello Database-per-tenant pattern</span></span>
> * <span data-ttu-id="c5c5c-110">Bérlő létrehozása</span><span class="sxs-lookup"><span data-stu-id="c5c5c-110">Create a tenant catalog</span></span>
> * <span data-ttu-id="c5c5c-111">A bérlői adatbázis létesítéséhez, és regisztrálhatja azt hello bérlői katalógusban</span><span class="sxs-lookup"><span data-stu-id="c5c5c-111">Provision a tenant database and register it in hello tenant catalog</span></span>
> * <span data-ttu-id="c5c5c-112">Állítson be egy minta Java-alkalmazás</span><span class="sxs-lookup"><span data-stu-id="c5c5c-112">Set up a sample Java application</span></span> 
> * <span data-ttu-id="c5c5c-113">Hozzáférés a bérlői adatbázisok egyszerű egy Java-Konzolalkalmazás</span><span class="sxs-lookup"><span data-stu-id="c5c5c-113">Access tenant databases simple a Java console application</span></span>
> * <span data-ttu-id="c5c5c-114">A bérlő törlése</span><span class="sxs-lookup"><span data-stu-id="c5c5c-114">Delete a tenant</span></span>

<span data-ttu-id="c5c5c-115">Ha nem rendelkezik Azure-előfizetéssel, [ingyenes fiók létrehozását](https://azure.microsoft.com/free/) megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="c5c5c-115">If you don't have an Azure subscription, [create a free account](https://azure.microsoft.com/free/) before you begin.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c5c5c-116">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c5c5c-116">Prerequisites</span></span>

<span data-ttu-id="c5c5c-117">toocomplete ezen oktatóanyag, győződjön meg arról, hogy rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="c5c5c-117">toocomplete this tutorial, make sure you have:</span></span>

* <span data-ttu-id="c5c5c-118">Telepített hello legújabb verzióját, PowerShell és hello [legújabb Azure PowerShell SDK](http://azure.microsoft.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="c5c5c-118">Installed hello newest version of PowerShell and hello [latest Azure PowerShell SDK](http://azure.microsoft.com/downloads/)</span></span>

* <span data-ttu-id="c5c5c-119">Telepített hello legújabb verziójának [SQL Server Management Studio](http://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).</span><span class="sxs-lookup"><span data-stu-id="c5c5c-119">Installed hello latest version of [SQL Server Management Studio](http://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).</span></span> <span data-ttu-id="c5c5c-120">SQL Server Management Studio telepítője hello SQLPackage, egy parancssori segédprogram, amely használt tooautomate adatbázis-fejlesztési feladatok számos legújabb verzióját is telepíti.</span><span class="sxs-lookup"><span data-stu-id="c5c5c-120">Installing SQL Server Management Studio also installs hello latest version of SQLPackage, a command-line utility that can be used tooautomate a range of database development tasks.</span></span>

* <span data-ttu-id="c5c5c-121">Telepített hello [Java-futtatókörnyezet (JRE) 8](http://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html) és hello [legújabb JAVA fejlesztői készlet (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) van telepítve a számítógépre.</span><span class="sxs-lookup"><span data-stu-id="c5c5c-121">Installed hello [Java Runtime Environment (JRE) 8](http://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html) and hello [latest JAVA Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) installed on your machine.</span></span> 

* <span data-ttu-id="c5c5c-122">Telepített [Apache Maven](https://maven.apache.org/download.cgi).</span><span class="sxs-lookup"><span data-stu-id="c5c5c-122">Installed [Apache Maven](https://maven.apache.org/download.cgi).</span></span> <span data-ttu-id="c5c5c-123">Maven használandó toohelp függőségek kezelése, létrehozása, tesztelése és hello minta Java-projektet futtatása</span><span class="sxs-lookup"><span data-stu-id="c5c5c-123">Maven will be used toohelp manage dependencies, build, test and run hello sample Java project</span></span>

## <a name="set-up-data-environment"></a><span data-ttu-id="c5c5c-124">Adatok környezet beállítása</span><span class="sxs-lookup"><span data-stu-id="c5c5c-124">Set up data environment</span></span>

<span data-ttu-id="c5c5c-125">Egy adatbázis bérlőnként fog kiépítése.</span><span class="sxs-lookup"><span data-stu-id="c5c5c-125">You will be provisioning a database per tenant.</span></span> <span data-ttu-id="c5c5c-126">hello adatbázis / bérlői modell biztosít a legmagasabb szintű hello kevés DevOps költség a bérlők elkülönítését.</span><span class="sxs-lookup"><span data-stu-id="c5c5c-126">hello database-per-tenant model provides hello highest degree of isolation between tenants, with little DevOps cost.</span></span> <span data-ttu-id="c5c5c-127">felhőalapú erőforrások toooptimize hello költségét, akkor lesz is kell hello bérlői adatbázisok kiépítése így toooptimize hello ár teljesítmény adatbázisok csoportja rugalmas készletbe.</span><span class="sxs-lookup"><span data-stu-id="c5c5c-127">toooptimize hello cost of cloud resources, you will also be provisioning hello tenant databases into an elastic pool which allows you toooptimize hello price performance for a group of databases.</span></span> <span data-ttu-id="c5c5c-128">más adatbázis-modellek kiépítés toolearn [talál](sql-database-design-patterns-multi-tenancy-saas-applications.md#multi-tenant-data-models).</span><span class="sxs-lookup"><span data-stu-id="c5c5c-128">toolearn about other database provisioning models [see here](sql-database-design-patterns-multi-tenancy-saas-applications.md#multi-tenant-data-models).</span></span>

<span data-ttu-id="c5c5c-129">Hajtsa végre ezeket a lépéseket toocreate, egy SQL server és a bérlői adatbázisokat üzemeltető rugalmas készlethez.</span><span class="sxs-lookup"><span data-stu-id="c5c5c-129">Follow these steps toocreate a SQL server and an elastic pool that will host all your tenant databases.</span></span> 

1. <span data-ttu-id="c5c5c-130">Hozzon létre változókat toostore értékeket, amelyeket a rendszer hello többi hello oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="c5c5c-130">Create variables toostore values that will be used in hello rest of hello tutorial.</span></span> <span data-ttu-id="c5c5c-131">Győződjön meg arról, hogy toomodify hello IP cím változó tooinclude az IP-címe</span><span class="sxs-lookup"><span data-stu-id="c5c5c-131">Make sure toomodify hello IP address variable tooinclude your IP address</span></span> 
   
   ```PowerShell 
   # Set an admin login and password for your database
   $adminlogin = "ServerAdmin"
   $password = "ChangeYourAdminPassword1"
   
   # Create random unique names for logical server and tenants
   $servername = "server-$(Get-Random)"
   $tenant1 = "geolamice"
   $tenant2 = "ranplex"
   
   # Store current client IP address (modify tooinclude your IP address)
   $startIpAddress = 0.0.0.0 
   $endIpAddress = 0.0.0.0
   ```
   
2. <span data-ttu-id="c5c5c-132">Bejelentkezési tooAzure és az SQL server és a rugalmas készlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="c5c5c-132">Login tooAzure and create a SQL server and elastic pool</span></span> 
   
   ```PowerShell
   # Login tooAzure 
   Login-AzureRmAccount
   
   # Create resource group 
   New-AzureRmResourceGroup -Name "myResourceGroup" -Location "northcentralus"
   
   # Create logical SQL Server with firewall rules 
   New-AzureRmSqlServer -ResourceGroupName "myResourceGroup" `
       -ServerName $servername `
       -Location "northcentralus" `
       -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $adminlogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))
   
   New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourcegroupname `
       -ServerName $servername `
       -FirewallRuleName "singleAddress" -StartIpAddress $startIpAddress -EndIpAddress $endIpAddress
   
   # Create elastic pool 
   New-AzureRmSqlElasticPool -ResourceGroupName "myResourceGroup"
       -ServerName $servername `
       -ElasticPoolName "myElasticPool" `
       -Edition "Standard" `
       -Dtu 50 `
       -DatabaseDtuMin 10 `
       -DatabaseDtuMax 20
   ```
   
## <a name="create-tenant-catalog"></a><span data-ttu-id="c5c5c-133">Bérlői katalógus létrehozása</span><span class="sxs-lookup"><span data-stu-id="c5c5c-133">Create tenant catalog</span></span> 

<span data-ttu-id="c5c5c-134">Egy több-bérlős SaaS-alkalmazáshoz hogy a rendszer egy bérlő adatokat tároló fontos tooknow.</span><span class="sxs-lookup"><span data-stu-id="c5c5c-134">In a multi-tenant SaaS application, it’s important tooknow where information for a tenant is stored.</span></span> <span data-ttu-id="c5c5c-135">Ez általában a katalógus-adatbázis tárolja.</span><span class="sxs-lookup"><span data-stu-id="c5c5c-135">This is commonly stored in a catalog database.</span></span> <span data-ttu-id="c5c5c-136">hello katalógus adatbázisa használt toohold a bérlők és egy adott bérlő adatok van tároló adatbázis közötti leképezést.</span><span class="sxs-lookup"><span data-stu-id="c5c5c-136">hello catalog database is used toohold a mapping between a tenant and a database in which that tenant’s data is stored.</span></span>  <span data-ttu-id="c5c5c-137">Alapszintű hello mintát vonatkozik, hogy egy több-bérlős, vagy egy bérlői egyetlen adatbázist használja.</span><span class="sxs-lookup"><span data-stu-id="c5c5c-137">hello basic pattern applies whether a multi-tenant or a single-tenant database is used.</span></span>

<span data-ttu-id="c5c5c-138">Kövesse a lépéseket toocreate hello minta SaaS-alkalmazás katalógus adatbázis.</span><span class="sxs-lookup"><span data-stu-id="c5c5c-138">Follow these steps toocreate a catalog database for hello sample SaaS application.</span></span>

```PowerShell
# Create empty database in pool
New-AzureRmSqlDatabase  -ResourceGroupName "myResourceGroup" `
    -ServerName $servername `
    -DatabaseName "tenantCatalog" `
    -ElasticPoolName "myElasticPool"

# Create table tootrack mapping between tenants and their databases
$commandText = "
CREATE TABLE Tenants
(
   TenantId         INT IDENTITY PRIMARY KEY,
   TenantName       NVARCHAR(128) NOT NULL,
   TenantDatabase   NVARCHAR(128) NOT NULL
);

CREATE INDEX IX_TenantName ON Tenants (TenantName);"

Invoke-SqlCmd `
    -Username $adminlogin `
    -Password $password `
    -ServerInstance ($servername + ".database.windows.net") `
    -Database "tenantCatalog" `
    -ConnectionTimeout 30 `
    -Query $commandText `
    -EncryptConnection
```

## <a name="provision-database-for-tenant1-and-register-in-tenant-catalog"></a><span data-ttu-id="c5c5c-139">A "tenant1" adatbázis létesítéséhez, és regisztrálja a bérlőt katalógus</span><span class="sxs-lookup"><span data-stu-id="c5c5c-139">Provision database for 'tenant1' and register in tenant catalog</span></span> 
<span data-ttu-id="c5c5c-140">Powershell tooprovision egy adatbázist használ egy új bérlőt "tenant1", és regisztrálja ezt a bérlőt hello katalógusban.</span><span class="sxs-lookup"><span data-stu-id="c5c5c-140">Use Powershell tooprovision a database for a new tenant 'tenant1' and register this tenant in hello catalog.</span></span> 

```PowerShell
# Create empty database in pool for 'tenant1'
New-AzureRmSqlDatabase  -ResourceGroupName "myResourceGroup" `
    -ServerName $servername `
    -DatabaseName $tenant1 `
    -ElasticPoolName "myElasticPool"

# Create table WhoAmI and insert tenant name into hello table 
$commandText = "
CREATE TABLE WhoAmI (TenantName NVARCHAR(128) NOT NULL);
INSERT INTO WhoAmI VALUES ('Tenant $tenant1');"

Invoke-SqlCmd `
    -Username $adminlogin `
    -Password $password `
    -ServerInstance ($servername + ".database.windows.net") `
    -Database $tenant1 `
    -ConnectionTimeout 30 `
    -Query $commandText `
    -EncryptConnection

# Register 'tenant1' in hello tenant catalog 
$commandText = "
INSERT INTO Tenants VALUES ('$tenant1', '$tenant1');"
Invoke-SqlCmd `
    -Username $adminlogin `
    -Password $password `
    -ServerInstance ($servername + ".database.windows.net") `
    -Database "tenantCatalog" `
    -ConnectionTimeout 30 `
    -Query $commandText `
    -EncryptConnection
```

## <a name="provision-database-for-tenant2-and-register-in-tenant-catalog"></a><span data-ttu-id="c5c5c-141">A "tenant2" adatbázis létesítéséhez, és regisztrálja a bérlőt katalógus</span><span class="sxs-lookup"><span data-stu-id="c5c5c-141">Provision database for 'tenant2' and register in tenant catalog</span></span>
<span data-ttu-id="c5c5c-142">Powershell tooprovision egy adatbázist használ egy új bérlőt "tenant2", és regisztrálja ezt a bérlőt hello katalógusban.</span><span class="sxs-lookup"><span data-stu-id="c5c5c-142">Use Powershell tooprovision a database for a new tenant 'tenant2' and register this tenant in hello catalog.</span></span> 

```PowerShell
# Create empty database in pool for 'tenant2'
New-AzureRmSqlDatabase  -ResourceGroupName "myResourceGroup" `
    -ServerName $servername `
    -DatabaseName $tenant2 `
    -ElasticPoolName "myElasticPool"

# Create table WhoAmI and insert tenant name into hello table 
$commandText = "
CREATE TABLE WhoAmI (TenantName NVARCHAR(128) NOT NULL);
INSERT INTO WhoAmI VALUES ('Tenant $tenant2');"

Invoke-SqlCmd `
    -Username $adminlogin `
    -Password $password `
    -ServerInstance ($servername + ".database.windows.net") `
    -Database $tenant2 `
    -ConnectionTimeout 30 `
    -Query $commandText `
    -EncryptConnection

# Register tenant 'tenant2' in hello tenant catalog 
$commandText = "
INSERT INTO Tenants VALUES ('$tenant2', '$tenant2');"
Invoke-SqlCmd `
    -Username $adminlogin `
    -Password $password `
    -ServerInstance ($servername + ".database.windows.net") `
    -Database "tenantCatalog" `
    -ConnectionTimeout 30 `
    -Query $commandText `
    -EncryptConnection
```

## <a name="set-up-sample-java-application"></a><span data-ttu-id="c5c5c-143">A minta Java-alkalmazás beállítása</span><span class="sxs-lookup"><span data-stu-id="c5c5c-143">Set up sample Java application</span></span> 

1. <span data-ttu-id="c5c5c-144">Maven-projekt létrehozása.</span><span class="sxs-lookup"><span data-stu-id="c5c5c-144">Create a maven project.</span></span> <span data-ttu-id="c5c5c-145">Írja be a hello következő parancsot a parancssorablakba:</span><span class="sxs-lookup"><span data-stu-id="c5c5c-145">Type hello following in a command prompt window:</span></span>
   
   ```
   mvn archetype:generate -DgroupId=com.microsoft.sqlserver -DartifactId=mssql-jdbc -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
   ```
   
2. <span data-ttu-id="c5c5c-146">Adja hozzá a függőség, a nyelvi szintjének, és állítsa be a beállítást toosupport JAR-fájlok kivételével toohello pom.xml fájlt a fájlok:</span><span class="sxs-lookup"><span data-stu-id="c5c5c-146">Add this dependency, language level, and build option toosupport manifest files in jars toohello pom.xml file:</span></span>
   
   ```XML
   <dependency>
         <groupId>com.microsoft.sqlserver</groupId>
         <artifactId>mssql-jdbc</artifactId>
         <version>6.1.0.jre8</version>
   </dependency>
   
   <properties>
         <maven.compiler.source>1.8</maven.compiler.source>
         <maven.compiler.target>1.8</maven.compiler.target>
   </properties>
   
   <build>
        <plugins>
           <plugin>
              <groupId>org.apache.maven.plugins</groupId>
              <artifactId>maven-jar-plugin</artifactId>
              <version>3.0.0</version>
              <configuration>
                 <archive>
                    <manifest>
                       <mainClass>com.sqldbsamples.App</mainClass>
                    </manifest>
                 </archive>
              </configuration>
           </plugin>
        </plugins>
   </build>
   ```

3. <span data-ttu-id="c5c5c-147">Adja hozzá a hello következő hello App.java fájlba:</span><span class="sxs-lookup"><span data-stu-id="c5c5c-147">Add hello following into hello App.java file:</span></span>

   ```java 
   package com.sqldbsamples;
   
   import java.util.Map;
   
   import java.util.HashMap;
   
   import java.io.BufferedReader;
   
   import java.io.InputStreamReader;
   
   import java.sql.Connection;
   
   import java.sql.Statement;
   
   import java.sql.PreparedStatement;
   
   import java.sql.ResultSet;
   
   import java.sql.DriverManager;
   
   public class App {
   
   private static final String SERVER_NAME = "your-server-name";
   
   private static final String CATALOG_DB_NAME = "tenantCatalog";
   
   private static final String USER = "ServerAdmin";
   
   private static final String PASSWORD = "ChangeYourAdminPassword1";
   
   private static final String CATALOG_DB_URL = String.format("jdbc:sqlserver://%s.database.windows.net:1433;database=%s;user=%s;password=%s;encrypt=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;", SERVER_NAME, CATALOG_DB_NAME, USER, PASSWORD);
   
   private static final String CMD_LIST = "LIST";

   private static final String CMD_QUERY = "QUERY";

   private static final String CMD_QUIT = "QUIT";
   
   public static void main(String[] args) {
   
   System.out.println("\n############################");
   
   System.out.println("## SAAS DATABASE TUTORIAL ##");
   
   System.out.println("############################\n");
   
   System.out.println("OPTIONS");
   
   System.out.println(" " + CMD_LIST + " - list tenants");
   
   System.out.println(" " + CMD_QUERY + " <NAME> - connect and tenant query tenant <NAME>");
   
   System.out.println(" " + CMD_QUIT + " - quit hello application\n");
   
   try (BufferedReader br = new BufferedReader(new InputStreamReader(System.in))) {
   
   while(true) {
   
   String[] input = br.readLine().split("\\s");
   
   if (null != input && input.length > 0) {
   
   if (input[0].equalsIgnoreCase(CMD_LIST)) {
   
   listTenants();
   
   } else if (input[0].toLowerCase().startsWith(CMD_QUERY.toLowerCase()) && input.length == 2) {
   
   queryTenant(input[1].trim());
   
   } else if (input[0].equalsIgnoreCase(CMD_QUIT)) {
   
   break;
   
   } else {
   
   System.out.println(" -> Command not supported");
   
   }
   
   }
   
   System.out.println("");
   
   }
   
   } catch (Exception e) {
   
   System.out.println(e.getMessage());
   
   e.printStackTrace();
   
   }
   
   }
   
   private static void listTenants() {
   
   // List all tenants that currently exist in hello system
   
   String sql = "SELECT TenantName FROM Tenants";
   
   try (Connection connection = DriverManager.getConnection(CATALOG_DB_URL);
   
   Statement stmt = connection.createStatement();
   
   ResultSet resultSet = stmt.executeQuery(sql)) {
   
   while (resultSet.next()) {
   
   System.out.println(" -> " + resultSet.getString(1));
   
   }
   
   } catch (Exception e) {
   
   System.out.println(e.getMessage());
   
   e.printStackTrace();
   
   }
   
   }
   
   private static void queryTenant(String name) {
   
   // Query hello data that was previously inserted into hello primary database from hello geo replicated database
   
   String url = null;
   
   String sql = "SELECT TenantDatabase FROM Tenants WHERE TenantName = ?";
   
   try (Connection connection = DriverManager.getConnection(CATALOG_DB_URL);
   
   PreparedStatement pstmt = connection.prepareStatement(sql)) {
   
   pstmt.setString(1, name);
   
   try (ResultSet resultSet = pstmt.executeQuery()) {
   
   if (resultSet.next()) {
   
   url = String.format("jdbc:sqlserver://%s.database.windows.net:1433;database=%s;user=%s;password=%s;encrypt=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;", SERVER_NAME, resultSet.getString(1), USER, PASSWORD);
   
   }
   
   }
   
   } catch (Exception e) {
   
   System.out.println(e.getMessage());
   
   e.printStackTrace();
   
   }
   
   if (null != url) {
   
   String tenantSql = "SELECT TenantName FROM WhoAmI";
   
   try (Connection connection = DriverManager.getConnection(url);
   
   Statement stmt = connection.createStatement();

   ResultSet resultSet = stmt.executeQuery(tenantSql)) {
   
   while (resultSet.next()) {
   
   System.out.println(" -> Entry in table WhoAmI in tenant " + name + " is: " + resultSet.getString(1));
   
   }
   
   } catch (Exception e) {
   
   System.out.println(e.getMessage());
   
   e.printStackTrace();
   
   }
   
   } else {
   
   System.out.println(" -> Tenant " + name + " not found");
   
   }
   
   }
   
   }
   ```

4. <span data-ttu-id="c5c5c-148">Hello fájl mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="c5c5c-148">Save hello file.</span></span>

5. <span data-ttu-id="c5c5c-149">Nyissa meg toocommand konzolon, és végrehajtása</span><span class="sxs-lookup"><span data-stu-id="c5c5c-149">Go toocommand console and execute</span></span>

   ```bash
   mvn package
   ```

6. <span data-ttu-id="c5c5c-150">Ha elkészült, hajtható végre a következő toorun hello alkalmazás hello</span><span class="sxs-lookup"><span data-stu-id="c5c5c-150">When finished, execute hello following toorun hello application</span></span> 
   
   ```
   mvn -q -e exec:java "-Dexec.mainClass=com.sqldbsamples.App"
   ```
   
<span data-ttu-id="c5c5c-151">hello kimenete a következőképpen jelenik meg, ha sikeresen lefutott:</span><span class="sxs-lookup"><span data-stu-id="c5c5c-151">hello output will look like this if it runs successfully:</span></span>

```
############################

## SAAS DATABASE TUTORIAL ##

############################

OPTIONS

LIST - list tenants

QUERY <NAME> - connect and tenant query tenant <NAME>

QUIT - quit hello application

* List hello tenants

* Query tenants you created
```

## <a name="delete-first-tenant"></a><span data-ttu-id="c5c5c-152">Első bérlői törlése</span><span class="sxs-lookup"><span data-stu-id="c5c5c-152">Delete first tenant</span></span> 
<span data-ttu-id="c5c5c-153">Használja a PowerShell toodelete hello bérlői adatbázis és a katalógus bejegyzést hello első bérlő számára.</span><span class="sxs-lookup"><span data-stu-id="c5c5c-153">Use PowerShell toodelete hello tenant database and catalog entry for hello first tenant.</span></span>

```PowerShell
# Remove 'tenant1' from catalog 
$commandText = "DELETE FROM Tenants WHERE TenantName = '$tenant1';"
Invoke-SqlCmd `
    -Username $adminlogin `
    -Password $password `
    -ServerInstance ($servername + ".database.windows.net") `
    -Database "tenantCatalog" `
    -ConnectionTimeout 30 `
    -Query $commandText `
    -EncryptConnection

# Delete database 
Remove-AzureRmSqlDatabase -ResourceGroupName "myResourceGroup" `
    -ServerName $servername `
    -DatabaseName $tenant1
```

<span data-ttu-id="c5c5c-154">Próbáljon meg túl hello Java-alkalmazás "tenant1" használatával.</span><span class="sxs-lookup"><span data-stu-id="c5c5c-154">Try connecting too'tenant1' using hello Java application.</span></span> <span data-ttu-id="c5c5c-155">Arról, hogy hello bérlő nem létezik hiba lép fel.</span><span class="sxs-lookup"><span data-stu-id="c5c5c-155">You will get an error stating that hello tenant does not exist.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c5c5c-156">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c5c5c-156">Next steps</span></span> 

<span data-ttu-id="c5c5c-157">Ebben az oktatóprogramban megismerte a:</span><span class="sxs-lookup"><span data-stu-id="c5c5c-157">In this tutorial, you learned to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="c5c5c-158">Egy több-bérlős SaaS-alkalmazáshoz, hello adatbázis / bérlői minta használatával egy adatbázis-környezet toosupport beállítása</span><span class="sxs-lookup"><span data-stu-id="c5c5c-158">Set up a database environment toosupport a multi-tenant SaaS application, using hello Database-per-tenant pattern</span></span>
> * <span data-ttu-id="c5c5c-159">Bérlő létrehozása</span><span class="sxs-lookup"><span data-stu-id="c5c5c-159">Create a tenant catalog</span></span>
> * <span data-ttu-id="c5c5c-160">A bérlői adatbázis létesítéséhez, és regisztrálhatja azt hello bérlői katalógusban</span><span class="sxs-lookup"><span data-stu-id="c5c5c-160">Provision a tenant database and register it in hello tenant catalog</span></span>
> * <span data-ttu-id="c5c5c-161">Állítson be egy minta Java-alkalmazás</span><span class="sxs-lookup"><span data-stu-id="c5c5c-161">Set up a sample Java application</span></span> 
> * <span data-ttu-id="c5c5c-162">Hozzáférés a bérlői adatbázisok egyszerű egy Java-Konzolalkalmazás</span><span class="sxs-lookup"><span data-stu-id="c5c5c-162">Access tenant databases simple a Java console application</span></span>
> * <span data-ttu-id="c5c5c-163">A bérlő törlése</span><span class="sxs-lookup"><span data-stu-id="c5c5c-163">Delete a tenant</span></span>

* <span data-ttu-id="c5c5c-164">PowerShell-példák gyakori feladatokat, lásd: [SQL Database PowerShell-példák](https://docs.microsoft.com/azure/sql-database/sql-database-powershell-samples)</span><span class="sxs-lookup"><span data-stu-id="c5c5c-164">PowerShell samples for common tasks, see [SQL Database PowerShell samples](https://docs.microsoft.com/azure/sql-database/sql-database-powershell-samples)</span></span>

* <span data-ttu-id="c5c5c-165">A kialakítási minták a több-bérlős SaaS-alkalmazások lásd [kialakítási minták](https://docs.microsoft.com/azure/sql-database/sql-database-design-patterns-multi-tenancy-saas-applications)</span><span class="sxs-lookup"><span data-stu-id="c5c5c-165">Design patterns for multi-tenant SaaS applications see [Design patterns](https://docs.microsoft.com/azure/sql-database/sql-database-design-patterns-multi-tenancy-saas-applications)</span></span>

* <span data-ttu-id="c5c5c-166">Java-minták a leggyakoribb Azure feladatokat, lásd: [Java fejlesztői központ](https://azure.microsoft.com/develop/java/)</span><span class="sxs-lookup"><span data-stu-id="c5c5c-166">Java samples for common Azure tasks, see [Java Developer Center](https://azure.microsoft.com/develop/java/)</span></span>



