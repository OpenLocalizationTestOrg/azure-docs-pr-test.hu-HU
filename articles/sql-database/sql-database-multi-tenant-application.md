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
# <a name="implement-a-multi-tenant-saas-application-using-azure-sql-database"></a>Egy több-bérlős SaaS-alkalmazás használatával az Azure SQL Database megvalósítása

Egy több-bérlős alkalmazás üzemeltetett felhőalapú környezetek alkalmazás, és azonos beállítása szolgáltatások toohundreds vagy a bérlő nem megosztása, vagy másik adatok megtekintéséhez több ezer hello biztosít. Példa: egy SaaS-alkalmazás, amely a felhőben üzemeltetett környezetben a szolgáltatások tootenants biztosít. Ez a modell hello adatokat elkülöníti az egyes bérlők számára, és optimalizálja a hello terjesztési költség az erőforrások. 

Ez az oktatóanyag bemutatja, hogyan toocreate egy Azure SQL Database használatával több-bérlős SaaS-alkalmazáshoz.

Ebben az oktatóanyagban elsajátíthatja:
> [!div class="checklist"]
> * Egy több-bérlős SaaS-alkalmazáshoz, hello adatbázis / bérlői minta használatával egy adatbázis-környezet toosupport beállítása
> * Bérlő létrehozása
> * A bérlői adatbázis létesítéséhez, és regisztrálhatja azt hello bérlői katalógusban
> * Állítson be egy minta Java-alkalmazás 
> * Hozzáférés a bérlői adatbázisok egyszerű egy Java-Konzolalkalmazás
> * A bérlő törlése

Ha nem rendelkezik Azure-előfizetéssel, [ingyenes fiók létrehozását](https://azure.microsoft.com/free/) megkezdése előtt.

## <a name="prerequisites"></a>Előfeltételek

toocomplete ezen oktatóanyag, győződjön meg arról, hogy rendelkezik:

* Telepített hello legújabb verzióját, PowerShell és hello [legújabb Azure PowerShell SDK](http://azure.microsoft.com/downloads/)

* Telepített hello legújabb verziójának [SQL Server Management Studio](http://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms). SQL Server Management Studio telepítője hello SQLPackage, egy parancssori segédprogram, amely használt tooautomate adatbázis-fejlesztési feladatok számos legújabb verzióját is telepíti.

* Telepített hello [Java-futtatókörnyezet (JRE) 8](http://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html) és hello [legújabb JAVA fejlesztői készlet (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) van telepítve a számítógépre. 

* Telepített [Apache Maven](https://maven.apache.org/download.cgi). Maven használandó toohelp függőségek kezelése, létrehozása, tesztelése és hello minta Java-projektet futtatása

## <a name="set-up-data-environment"></a>Adatok környezet beállítása

Egy adatbázis bérlőnként fog kiépítése. hello adatbázis / bérlői modell biztosít a legmagasabb szintű hello kevés DevOps költség a bérlők elkülönítését. felhőalapú erőforrások toooptimize hello költségét, akkor lesz is kell hello bérlői adatbázisok kiépítése így toooptimize hello ár teljesítmény adatbázisok csoportja rugalmas készletbe. más adatbázis-modellek kiépítés toolearn [talál](sql-database-design-patterns-multi-tenancy-saas-applications.md#multi-tenant-data-models).

Hajtsa végre ezeket a lépéseket toocreate, egy SQL server és a bérlői adatbázisokat üzemeltető rugalmas készlethez. 

1. Hozzon létre változókat toostore értékeket, amelyeket a rendszer hello többi hello oktatóanyag. Győződjön meg arról, hogy toomodify hello IP cím változó tooinclude az IP-címe 
   
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
   
2. Bejelentkezési tooAzure és az SQL server és a rugalmas készlet létrehozása 
   
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
   
## <a name="create-tenant-catalog"></a>Bérlői katalógus létrehozása 

Egy több-bérlős SaaS-alkalmazáshoz hogy a rendszer egy bérlő adatokat tároló fontos tooknow. Ez általában a katalógus-adatbázis tárolja. hello katalógus adatbázisa használt toohold a bérlők és egy adott bérlő adatok van tároló adatbázis közötti leképezést.  Alapszintű hello mintát vonatkozik, hogy egy több-bérlős, vagy egy bérlői egyetlen adatbázist használja.

Kövesse a lépéseket toocreate hello minta SaaS-alkalmazás katalógus adatbázis.

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

## <a name="provision-database-for-tenant1-and-register-in-tenant-catalog"></a>A "tenant1" adatbázis létesítéséhez, és regisztrálja a bérlőt katalógus 
Powershell tooprovision egy adatbázist használ egy új bérlőt "tenant1", és regisztrálja ezt a bérlőt hello katalógusban. 

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

## <a name="provision-database-for-tenant2-and-register-in-tenant-catalog"></a>A "tenant2" adatbázis létesítéséhez, és regisztrálja a bérlőt katalógus
Powershell tooprovision egy adatbázist használ egy új bérlőt "tenant2", és regisztrálja ezt a bérlőt hello katalógusban. 

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

## <a name="set-up-sample-java-application"></a>A minta Java-alkalmazás beállítása 

1. Maven-projekt létrehozása. Írja be a hello következő parancsot a parancssorablakba:
   
   ```
   mvn archetype:generate -DgroupId=com.microsoft.sqlserver -DartifactId=mssql-jdbc -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
   ```
   
2. Adja hozzá a függőség, a nyelvi szintjének, és állítsa be a beállítást toosupport JAR-fájlok kivételével toohello pom.xml fájlt a fájlok:
   
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

3. Adja hozzá a hello következő hello App.java fájlba:

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

4. Hello fájl mentéséhez.

5. Nyissa meg toocommand konzolon, és végrehajtása

   ```bash
   mvn package
   ```

6. Ha elkészült, hajtható végre a következő toorun hello alkalmazás hello 
   
   ```
   mvn -q -e exec:java "-Dexec.mainClass=com.sqldbsamples.App"
   ```
   
hello kimenete a következőképpen jelenik meg, ha sikeresen lefutott:

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

## <a name="delete-first-tenant"></a>Első bérlői törlése 
Használja a PowerShell toodelete hello bérlői adatbázis és a katalógus bejegyzést hello első bérlő számára.

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

Próbáljon meg túl hello Java-alkalmazás "tenant1" használatával. Arról, hogy hello bérlő nem létezik hiba lép fel.

## <a name="next-steps"></a>Következő lépések 

Ebben az oktatóprogramban megismerte a:
> [!div class="checklist"]
> * Egy több-bérlős SaaS-alkalmazáshoz, hello adatbázis / bérlői minta használatával egy adatbázis-környezet toosupport beállítása
> * Bérlő létrehozása
> * A bérlői adatbázis létesítéséhez, és regisztrálhatja azt hello bérlői katalógusban
> * Állítson be egy minta Java-alkalmazás 
> * Hozzáférés a bérlői adatbázisok egyszerű egy Java-Konzolalkalmazás
> * A bérlő törlése

* PowerShell-példák gyakori feladatokat, lásd: [SQL Database PowerShell-példák](https://docs.microsoft.com/azure/sql-database/sql-database-powershell-samples)

* A kialakítási minták a több-bérlős SaaS-alkalmazások lásd [kialakítási minták](https://docs.microsoft.com/azure/sql-database/sql-database-design-patterns-multi-tenancy-saas-applications)

* Java-minták a leggyakoribb Azure feladatokat, lásd: [Java fejlesztői központ](https://azure.microsoft.com/develop/java/)



