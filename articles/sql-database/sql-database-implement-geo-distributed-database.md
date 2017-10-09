---
title: "földrajzilag elosztott Azure SQL adatbázis-megoldás aaaImplement |} Microsoft Docs"
description: "További tudnivalók az Azure SQL Database és a feladatátvételi tooa replikált alkalmazás adatbázisából, és a feladatátvételi teszt tooconfigure."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,business continuity
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 05/26/2017
ms.author: carlrab
ms.openlocfilehash: 9295d33c669405108a1a64ef1e7cb77f582773a1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="implement-a-geo-distributed-database"></a>Egy földrajzilag elosztott adatbázist megvalósítása

Ebben az oktatóanyagban az Azure SQL-adatbázis és a feladatátvételi tooa távoli régió alkalmazás konfigurálása, és tesztelje a feladatátvételi tervet. Az alábbiak végrehajtásának módját ismerheti meg: 

> [!div class="checklist"]
> * Adatbázis-felhasználók létrehozása és engedélyekkel rendelkeznek
> * Egy adatbázis-adatbázisszintű tűzfalszabály beállítása
> * Hozzon létre egy [georeplikáció feladatátvételi csoport](sql-database-geo-replication-overview.md)
> * Hozzon létre és lefordítani a Java-alkalmazás tooquery Azure SQL-adatbázis
> * Hajtsa végre a vész-helyreállítási részletezési

Ha nem rendelkezik Azure-előfizetéssel, [ingyenes fiók létrehozását](https://azure.microsoft.com/free/) megkezdése előtt.


## <a name="prerequisites"></a>Előfeltételek

Ebben az oktatóanyagban a következő előfeltételek győződjön meg arról, hogy hello elvégzése toocomplete:

- Legújabb telepített hello [Azure PowerShell](https://docs.microsoft.com/powershell/azureps-cmdlets-docs). 
- Azure SQL-adatbázis telepítve. Ez az oktatóanyag hello AdventureWorksLT példaadatbázisát használja nevű **mySampleDatabase** a gyors üzembe helyezések egyikét:

   - [DB létrehozása – portál](sql-database-get-started-portal.md)
   - [DB létrehozása – CLI](sql-database-get-started-cli.md)
   - [DB létrehozása – PowerShell](sql-database-get-started-powershell.md)

- Azonosította a metódus tooexecute SQL-szkriptek használatát az adatbázishoz, használható a következő lekérdezés eszközök hello egyikét:
   - a hello hello Lekérdezésszerkesztő [Azure-portálon](https://portal.azure.com). Hello Lekérdezésszerkesztő hello Azure-portál használatával további információkért lásd: [kapcsolódás és lekérdezés lekérdezés-szerkesztő segítségével](sql-database-get-started-portal.md#query-the-sql-database).
   - hello legújabb verziójának [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms), ez az egyetlen SQL infrastruktúra kezelése az SQL Server tooSQL adatbázis a Microsoft Windows integrált környezetet.
   - hello legújabb verziójának [Visual Studio Code](https://code.visualstudio.com/docs), amely egy grafikus kód szerkesztése Linux, macOS, és, amely támogatja a kiterjesztések, beleértve a Windows hello [mssql bővítmény](https://aka.ms/mssql-marketplace) Microsoft SQL Server lekérdezése , Az azure SQL Database és az SQL Data Warehouse. Ezzel az eszközzel az Azure SQL Database további információkért lásd: [kapcsolódás és lekérdezés VS kóddal](sql-database-connect-query-vscode.md). 

## <a name="create-database-users-and-grant-permissions"></a>Hozzon létre az adatbázis-felhasználók és engedélyek

Csatlakozás tooyour adatbázis, és hozzon létre felhasználói fiókokat a következő lekérdezés eszközök hello egyikének használatával:

- hello Lekérdezésszerkesztő a hello Azure-portálon
- SQL Server Management Studio
- Visual Studio Code

Ezek a fiókok replikálása automatikusan tooyour másodlagos kiszolgáló (és szinkronban kell tartani). toouse SQL Server Management Studio vagy Visual Studio Code, szükség lehet egy tűzfalszabályt tooconfigure egy ügyfél, amelynek Ön még nincs konfigurálva a tűzfal IP-címen keresztül csatlakozik. Részletes útmutató: [hozzon létre egy kiszolgálószintű tűzfalszabályt](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).

- A lekérdezési ablakban hajtható végre a következő lekérdezés toocreate két felhasználói fiókokat az adatbázis hello. Ezt a parancsfájlt biztosít **db_owner** engedélyek toohello **app_admin** fiók és biztosít **kiválasztása** és **frissítés** engedélyek toohello **app_user** fiók. 

   ```sql
   CREATE USER app_admin WITH PASSWORD = 'ChangeYourPassword1';
   --Add SQL user toodb_owner role
   ALTER ROLE db_owner ADD MEMBER app_admin; 
   --Create additional SQL user
   CREATE USER app_user WITH PASSWORD = 'ChangeYourPassword1';
   --grant permission tooSalesLT schema
   GRANT SELECT, INSERT, DELETE, UPDATE ON SalesLT.Product tooapp_user;
   ```

## <a name="create-database-level-firewall"></a>Adatbázis-szintű tűzfal létrehozása

Hozzon létre egy [adatbázis-adatbázisszintű tűzfalszabály](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database) az SQL-adatbázis számára. Az adatbázis-adatbázisszintű tűzfalszabály automatikusan replikálja az Ön által létrehozott ebben az oktatóanyagban toohello másodlagos kiszolgáló. Az egyszerűség érdekében (a jelen oktatóanyag), amelyen hajtja végre hello lépéseit az oktatóanyag hello számítógép hello nyilvános IP-címét használja. hello kiszolgálószintű tűzfalszabályt az aktuális számítógépen használt toodetermine hello IP-címmel lásd [hozzon létre egy kiszolgálószintű tűzfal](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).  

- Nyissa meg a lekérdezésablakban cserélje le hello előző lekérdezés hello lekérdezés a következő, a környezet hello megfelelő IP-címekkel rendelkező hello IP-címek cseréje.  

   ```sql
   -- Create database-level firewall setting for your public IP address
   EXECUTE sp_set_database_firewall_rule @name = N'myGeoReplicationFirewallRule',@start_ip_address = '0.0.0.0', @end_ip_address = '0.0.0.0';
   ```

## <a name="create-an-active-geo-replication-auto-failover-group"></a>Aktív georeplikáció automatikus feladatátvételi csoport létrehozása 

Azure PowerShell használatával hozzon létre egy [aktív georeplikáció automatikus feladatátvételi csoport](sql-database-geo-replication-overview.md) között a meglévő Azure SQL server és az új hello Azure SQL server Azure-régióban üres, és adja hozzá a a minta adatbázis toohello feladatátvételi csoport.

> [!IMPORTANT]
> Ezeket a parancsmagokat az Azure PowerShell 4.0 szükséges. [!INCLUDE [sample-powershell-install](../../includes/sample-powershell-install-no-ssh.md)]
>

1. Feltöltése a PowerShell-parancsfájlok hello értékek használata a meglévő kiszolgáló és a mintaadatbázis változók, és adjon meg egy globális szinten egyedi érték feladatátvételi csoport neve.

   ```powershell
   $adminlogin = "ServerAdmin"
   $password = "ChangeYourAdminPassword1"
   $myresourcegroupname = "<your resource group name>"
   $mylocation = "<your resource group location>"
   $myservername = "<your existing server name>"
   $mydatabasename = "mySampleDatabase"
   $mydrlocation = "<your disaster recovery location>"
   $mydrservername = "<your disaster recovery server name>"
   $myfailovergroupname = "<your unique failover group name>"
   ```

2. Hozzon létre egy üres helykiszolgáló biztonsági mentése a feladatátvételi régióban.

   ```powershell
   $mydrserver = New-AzureRmSqlServer -ResourceGroupName $myresourcegroupname `
      -ServerName $mydrservername `
      -Location $mydrlocation `
      -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $adminlogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))
   $mydrserver   
   ```

3. Hozzon létre egy feladatátvevő hello két kiszolgáló között.

   ```powershell
   $myfailovergroup = New-AzureRMSqlDatabaseFailoverGroup `
      –ResourceGroupName $myresourcegroupname `
      -ServerName $myservername `
      -PartnerServerName $mydrservername  `
      –FailoverGroupName $myfailovergroupname `
      –FailoverPolicy Automatic `
      -GracePeriodWithDataLossHours 2
   $myfailovergroup   
   ```

4. Az adatbázis toohello feladatátvételi csoport hozzáadása.

   ```powershell
   $myfailovergroup = Get-AzureRmSqlDatabase `
      -ResourceGroupName $myresourcegroupname `
      -ServerName $myservername `
      -DatabaseName $mydatabasename | `
    Add-AzureRmSqlDatabaseToFailoverGroup `
      -ResourceGroupName $myresourcegroupname ` `
      -ServerName $myservername `
      -FailoverGroupName $myfailovergroupname
   $myfailovergroup   
   ```

## <a name="install-java-software"></a>Java szoftver telepítése

Ez a szakasz hello lépések azt feltételezik, hogy ismeri a Java használatával történő fejlesztéséhez, és az Azure SQL Database új tooworking. 

### <a name="mac-os"></a>**Mac OS**
Nyissa meg a terminált, és keresse meg a tooa könyvtárban, ahol létrehozását tervezi a Java-projektet. Telepítés **brew** és **Maven** hello a következő parancsok beírásával: 

```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew update
brew install maven
```

Részletes útmutatást telepítése és konfigurálása a Java és Maven környezet, lépjen a hello [hozza létre az alkalmazását, használja az SQL Server](https://www.microsoft.com/sql-server/developer-get-started/), jelölje be **Java**, jelölje be **MacOS**, és hajtsa végre hello részletes 1.3 és 1.2-es lépésben Java és Maven konfigurálására vonatkozó útmutatásokat.

### <a name="linux-ubuntu"></a>**Linux (Ubuntu)**
Nyissa meg a terminált, és keresse meg a tooa könyvtárban, ahol létrehozását tervezi a Java-projektet. Telepítés **Maven** hello a következő parancsok beírásával:

```bash
sudo apt-get install maven
```

Részletes útmutatást telepítése és konfigurálása a Java és Maven környezet, lépjen a hello [hozza létre az alkalmazását, használja az SQL Server](https://www.microsoft.com/sql-server/developer-get-started/), jelölje be **Java**, jelölje be **Ubuntu**, és hajtsa végre hello részletes 1.2-es, az 1.3 és az 1.4-es lépésben Java és Maven konfigurálására vonatkozó útmutatásokat.

### <a name="windows"></a>**Windows**
Telepítés [Maven](https://maven.apache.org/download.cgi) hello hivatalos installer használatával. Toohelp függőségeinek kezelése, létre, tesztelheti, és futtassa a Java-projektet Maven használata. Részletes útmutatást telepítése és konfigurálása a Java és Maven környezet, lépjen a hello [hozza létre az SQL Server használatával alkalmazását](https://www.microsoft.com/sql-server/developer-get-started/), jelölje be **Java**, válassza a Windows, és hajtsa végre hello részletes utasítások a Konfigurálja a Java és Maven 1.3 és 1.2-es lépésben.

## <a name="create-sqldbsample-project"></a>SqlDbSample projekt létrehozása

1. Hello parancskonzolról (például a Bash), a Maven-projekt létrehozása. 
   ```bash
   mvn archetype:generate "-DgroupId=com.sqldbsamples" "-DartifactId=SqlDbSample" "-DarchetypeArtifactId=maven-archetype-quickstart" "-Dversion=1.0.0"
   ```
2. Típus **Y** kattintson **Enter**.
3. Az újonnan létrehozott projektben módosítsa a könyvtárat.

   ```bash
   cd SqlDbSamples
   ```

4. A kedvenc szerkesztővel, nyissa meg a projektmappa hello pom.xml fájlt. 

5. Adja hozzá a hello JDBC-illesztőt Microsoft SQL Server függőségi tooyour Maven-projektek kedvenc szövegszerkesztőjével megnyitásával, majd a másolás és beillesztés hello a pom.xml fájlt az alábbi. Hello meglévő értékek előre kitölti az hello fájl felülírásának mellőzése. hello JDBC függőségi hello nagyobb "függőségei" szakasz () kell beilleszteni.   

   ```xml
   <dependency>
     <groupId>com.microsoft.sqlserver</groupId>
     <artifactId>mssql-jdbc</artifactId>
    <version>6.1.0.jre8</version>
   </dependency>
   ```

6. Adja meg a Java toocompile hello projekt elleni hello verzióját "Tulajdonságok" szakasz hello pom.xml fájlba a következő hello "függőségei" szakasz után hello hozzáadásával. 

   ```xml
   <properties>
     <maven.compiler.source>1.8</maven.compiler.source>
     <maven.compiler.target>1.8</maven.compiler.target>
   </properties>
   ```
7. Adja hozzá a hello következő "Szerkesztés" szakasz hello pom.xml fájlba után hello "Tulajdonságok" szakasz toosupport jegyzékfájlt a JAR-fájlok kivételével.       

   ```xml
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
8. Mentse és zárja be a hello pom.xml fájlt.
9. Nyissa meg hello App.java fájlt (C:\apache-maven-3.5.0\SqlDbSample\src\main\java\com\sqldbsamples\App.java), és cserélje ki a következő tartalma hello hello tartalmát. Cserélje le a feladatátvételi csoport hello nevét hello feladatátvételi csoport nevét. Ha módosította hello hello adatbázis neve, a felhasználó vagy a jelszó, ezeket az értékeket, valamint módosítása

   ```java
   package com.sqldbsamples;

   import java.sql.Connection;
   import java.sql.Statement;
   import java.sql.PreparedStatement;
   import java.sql.ResultSet;
   import java.sql.Timestamp;
   import java.sql.DriverManager;
   import java.util.Date;
   import java.util.concurrent.TimeUnit;

   public class App {

      private static final String FAILOVER_GROUP_NAME = "myfailovergroupname";
  
      private static final String DB_NAME = "mySampleDatabase";
      private static final String USER = "app_user";
      private static final String PASSWORD = "ChangeYourPassword1";

      private static final String READ_WRITE_URL = String.format("jdbc:sqlserver://%s.database.windows.net:1433;database=%s;user=%s;password=%s;encrypt=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;", FAILOVER_GROUP_NAME, DB_NAME, USER, PASSWORD);
      private static final String READ_ONLY_URL = String.format("jdbc:sqlserver://%s.secondary.database.windows.net:1433;database=%s;user=%s;password=%s;encrypt=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;", FAILOVER_GROUP_NAME, DB_NAME, USER, PASSWORD);

      public static void main(String[] args) {
         System.out.println("#######################################");
         System.out.println("## GEO DISTRIBUTED DATABASE TUTORIAL ##");
         System.out.println("#######################################");
         System.out.println(""); 

         int highWaterMark = getHighWaterMarkId();

         try {
            for(int i = 1; i < 1000; i++) {
                //  loop will run for about 1 hour
                System.out.print(i + ": insert on primary " + (insertData((highWaterMark + i))?"successful":"failed"));
                TimeUnit.SECONDS.sleep(1);
                System.out.print(", read from secondary " + (selectData((highWaterMark + i))?"successful":"failed") + "\n");
                TimeUnit.SECONDS.sleep(3);
            }
         } catch(Exception e) {
            e.printStackTrace();
      }
   }

   private static boolean insertData(int id) {
      // Insert data into hello product table with a unique product name that we can use toofind hello product again later
      String sql = "INSERT INTO SalesLT.Product (Name, ProductNumber, Color, StandardCost, ListPrice, SellStartDate) VALUES (?,?,?,?,?,?);";

      try (Connection connection = DriverManager.getConnection(READ_WRITE_URL); 
              PreparedStatement pstmt = connection.prepareStatement(sql)) {
         pstmt.setString(1, "BrandNewProduct" + id);
         pstmt.setInt(2, 200989 + id + 10000);
         pstmt.setString(3, "Blue");
         pstmt.setDouble(4, 75.00);
         pstmt.setDouble(5, 89.99);
         pstmt.setTimestamp(6, new Timestamp(new Date().getTime()));
         return (1 == pstmt.executeUpdate());
      } catch (Exception e) {
         return false;
      }
   }

   private static boolean selectData(int id) {
      // Query hello data that was previously inserted into hello primary database from hello geo replicated database
      String sql = "SELECT Name, Color, ListPrice FROM SalesLT.Product WHERE Name = ?";

      try (Connection connection = DriverManager.getConnection(READ_ONLY_URL); 
              PreparedStatement pstmt = connection.prepareStatement(sql)) {
         pstmt.setString(1, "BrandNewProduct" + id);
         try (ResultSet resultSet = pstmt.executeQuery()) {
            return resultSet.next();
         }
      } catch (Exception e) {
         return false;
      }
   }

   private static int getHighWaterMarkId() {
      // Query hello high water mark id that is stored in hello table toobe able toomake unique inserts 
      String sql = "SELECT MAX(ProductId) FROM SalesLT.Product";
      int result = 1;
        
      try (Connection connection = DriverManager.getConnection(READ_WRITE_URL); 
              Statement stmt = connection.createStatement();
              ResultSet resultSet = stmt.executeQuery(sql)) {
         if (resultSet.next()) {
             result =  resultSet.getInt(1);
            }
         } catch (Exception e) {
          e.printStackTrace();
         }
         return result;
      }
   }
   ```
6. Mentse és zárja be hello App.java fájlt.

## <a name="compile-and-run-hello-sqldbsample-project"></a>Fordítsa le, és futtassa a hello SqlDbSample projekt

1. Hello parancs konzolon toofollowing parancs hajtható végre.

   ```bash
   mvn package
   ```
2. Ha elkészült, hajtható végre a következő parancs toorun hello alkalmazás (futtatás esetén körülbelül 1 óra kivéve, ha manuálisan leállítása) hello:

   ```bash
   mvn -q -e exec:java "-Dexec.mainClass=com.sqldbsamples.App"
   
   #######################################
   ## GEO DISTRIBUTED DATABASE TUTORIAL ##
   #######################################

   1. insert on primary successful, read from secondary successful
   2. insert on primary successful, read from secondary successful
   3. insert on primary successful, read from secondary successful
   ```

## <a name="perform-disaster-recovery-drill"></a>Hajtsa végre a vész-helyreállítási részletezési

1. Hívja meg a feladatátvételi csoport kézi feladatátvételre. 

   ```powershell
   Switch-AzureRMSqlDatabaseFailoverGroup `
   -ResourceGroupName $myresourcegroupname  `
   -ServerName $mydrservername `
   -FailoverGroupName $myfailovergroupname
   ```

2. Figyelje meg hello alkalmazás eredmények a feladatátvétel során. Néhány beszúrása sikertelen hello DNS-gyorsítótár frissítése közben.     

3. Megtudhatja, milyen működik-e a vész-helyreállítási kiszolgálói szerepkört.

   ```powershell
   $mydrserver.ReplicationRole
   ```

4. Feladat-visszavételre.

   ```powershell
   Switch-AzureRMSqlDatabaseFailoverGroup `
   -ResourceGroupName $myresourcegroupname  `
   -ServerName $myservername `
   -FailoverGroupName $myfailovergroupname
   ```

5. Figyelje meg hello alkalmazás eredmények feladat-visszavétel során. Néhány beszúrása sikertelen hello DNS-gyorsítótár frissítése közben.     

6. Megtudhatja, milyen működik-e a vész-helyreállítási kiszolgálói szerepkört.

   ```powershell
   $fileovergroup = Get-AzureRMSqlDatabaseFailoverGroup `
      -FailoverGroupName $myfailovergroupname `
      -ResourceGroupName $myresourcegroupname `
      -ServerName $mydrservername
   $fileovergroup.ReplicationRole
   ```
## <a name="next-steps"></a>Következő lépések 

További információkért lásd: [aktív georeplikáció és feladatátvételi csoportok](sql-database-geo-replication-overview.md).
