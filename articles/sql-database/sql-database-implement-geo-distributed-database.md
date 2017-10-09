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
# <a name="implement-a-geo-distributed-database"></a><span data-ttu-id="eda6d-103">Egy földrajzilag elosztott adatbázist megvalósítása</span><span class="sxs-lookup"><span data-stu-id="eda6d-103">Implement a geo-distributed database</span></span>

<span data-ttu-id="eda6d-104">Ebben az oktatóanyagban az Azure SQL-adatbázis és a feladatátvételi tooa távoli régió alkalmazás konfigurálása, és tesztelje a feladatátvételi tervet.</span><span class="sxs-lookup"><span data-stu-id="eda6d-104">In this tutorial, you configure an Azure SQL database and application for failover tooa remote region, and then test your failover plan.</span></span> <span data-ttu-id="eda6d-105">Az alábbiak végrehajtásának módját ismerheti meg:</span><span class="sxs-lookup"><span data-stu-id="eda6d-105">You learn how to:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="eda6d-106">Adatbázis-felhasználók létrehozása és engedélyekkel rendelkeznek</span><span class="sxs-lookup"><span data-stu-id="eda6d-106">Create database users and grant them permissions</span></span>
> * <span data-ttu-id="eda6d-107">Egy adatbázis-adatbázisszintű tűzfalszabály beállítása</span><span class="sxs-lookup"><span data-stu-id="eda6d-107">Set up a database-level firewall rule</span></span>
> * <span data-ttu-id="eda6d-108">Hozzon létre egy [georeplikáció feladatátvételi csoport](sql-database-geo-replication-overview.md)</span><span class="sxs-lookup"><span data-stu-id="eda6d-108">Create a [geo-replication failover group](sql-database-geo-replication-overview.md)</span></span>
> * <span data-ttu-id="eda6d-109">Hozzon létre és lefordítani a Java-alkalmazás tooquery Azure SQL-adatbázis</span><span class="sxs-lookup"><span data-stu-id="eda6d-109">Create and compile a Java application tooquery an Azure SQL database</span></span>
> * <span data-ttu-id="eda6d-110">Hajtsa végre a vész-helyreállítási részletezési</span><span class="sxs-lookup"><span data-stu-id="eda6d-110">Perform a disaster recovery drill</span></span>

<span data-ttu-id="eda6d-111">Ha nem rendelkezik Azure-előfizetéssel, [ingyenes fiók létrehozását](https://azure.microsoft.com/free/) megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="eda6d-111">If you don't have an Azure subscription, [create a free account](https://azure.microsoft.com/free/) before you begin.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="eda6d-112">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="eda6d-112">Prerequisites</span></span>

<span data-ttu-id="eda6d-113">Ebben az oktatóanyagban a következő előfeltételek győződjön meg arról, hogy hello elvégzése toocomplete:</span><span class="sxs-lookup"><span data-stu-id="eda6d-113">toocomplete this tutorial, make sure hello following prerequisites are completed:</span></span>

- <span data-ttu-id="eda6d-114">Legújabb telepített hello [Azure PowerShell](https://docs.microsoft.com/powershell/azureps-cmdlets-docs).</span><span class="sxs-lookup"><span data-stu-id="eda6d-114">Installed hello latest [Azure PowerShell](https://docs.microsoft.com/powershell/azureps-cmdlets-docs).</span></span> 
- <span data-ttu-id="eda6d-115">Azure SQL-adatbázis telepítve.</span><span class="sxs-lookup"><span data-stu-id="eda6d-115">Installed an Azure SQL database.</span></span> <span data-ttu-id="eda6d-116">Ez az oktatóanyag hello AdventureWorksLT példaadatbázisát használja nevű **mySampleDatabase** a gyors üzembe helyezések egyikét:</span><span class="sxs-lookup"><span data-stu-id="eda6d-116">This tutorial uses hello AdventureWorksLT sample database with a name of **mySampleDatabase** from one of these quick starts:</span></span>

   - [<span data-ttu-id="eda6d-117">DB létrehozása – portál</span><span class="sxs-lookup"><span data-stu-id="eda6d-117">Create DB - Portal</span></span>](sql-database-get-started-portal.md)
   - [<span data-ttu-id="eda6d-118">DB létrehozása – CLI</span><span class="sxs-lookup"><span data-stu-id="eda6d-118">Create DB - CLI</span></span>](sql-database-get-started-cli.md)
   - [<span data-ttu-id="eda6d-119">DB létrehozása – PowerShell</span><span class="sxs-lookup"><span data-stu-id="eda6d-119">Create DB - PowerShell</span></span>](sql-database-get-started-powershell.md)

- <span data-ttu-id="eda6d-120">Azonosította a metódus tooexecute SQL-szkriptek használatát az adatbázishoz, használható a következő lekérdezés eszközök hello egyikét:</span><span class="sxs-lookup"><span data-stu-id="eda6d-120">Have identified a method tooexecute SQL scripts against your database, you can use one of hello following query tools:</span></span>
   - <span data-ttu-id="eda6d-121">a hello hello Lekérdezésszerkesztő [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="eda6d-121">hello query editor in hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="eda6d-122">Hello Lekérdezésszerkesztő hello Azure-portál használatával további információkért lásd: [kapcsolódás és lekérdezés lekérdezés-szerkesztő segítségével](sql-database-get-started-portal.md#query-the-sql-database).</span><span class="sxs-lookup"><span data-stu-id="eda6d-122">For more information on using hello query editor in hello Azure portal, see [Connect and query using Query Editor](sql-database-get-started-portal.md#query-the-sql-database).</span></span>
   - <span data-ttu-id="eda6d-123">hello legújabb verziójának [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms), ez az egyetlen SQL infrastruktúra kezelése az SQL Server tooSQL adatbázis a Microsoft Windows integrált környezetet.</span><span class="sxs-lookup"><span data-stu-id="eda6d-123">hello newest version of [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms), which is an integrated environment for managing any SQL infrastructure, from SQL Server tooSQL Database for Microsoft Windows.</span></span>
   - <span data-ttu-id="eda6d-124">hello legújabb verziójának [Visual Studio Code](https://code.visualstudio.com/docs), amely egy grafikus kód szerkesztése Linux, macOS, és, amely támogatja a kiterjesztések, beleértve a Windows hello [mssql bővítmény](https://aka.ms/mssql-marketplace) Microsoft SQL Server lekérdezése , Az azure SQL Database és az SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="eda6d-124">hello newest version of [Visual Studio Code](https://code.visualstudio.com/docs), which is a graphical code editor for Linux, macOS, and Windows that supports extensions, including hello [mssql extension](https://aka.ms/mssql-marketplace) for querying Microsoft SQL Server, Azure SQL Database, and SQL Data Warehouse.</span></span> <span data-ttu-id="eda6d-125">Ezzel az eszközzel az Azure SQL Database további információkért lásd: [kapcsolódás és lekérdezés VS kóddal](sql-database-connect-query-vscode.md).</span><span class="sxs-lookup"><span data-stu-id="eda6d-125">For more information on using this tool with Azure SQL Database, see [Connect and query with VS Code](sql-database-connect-query-vscode.md).</span></span> 

## <a name="create-database-users-and-grant-permissions"></a><span data-ttu-id="eda6d-126">Hozzon létre az adatbázis-felhasználók és engedélyek</span><span class="sxs-lookup"><span data-stu-id="eda6d-126">Create database users and grant permissions</span></span>

<span data-ttu-id="eda6d-127">Csatlakozás tooyour adatbázis, és hozzon létre felhasználói fiókokat a következő lekérdezés eszközök hello egyikének használatával:</span><span class="sxs-lookup"><span data-stu-id="eda6d-127">Connect tooyour database and create user accounts using one of hello following query tools:</span></span>

- <span data-ttu-id="eda6d-128">hello Lekérdezésszerkesztő a hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="eda6d-128">hello Query editor in hello Azure portal</span></span>
- <span data-ttu-id="eda6d-129">SQL Server Management Studio</span><span class="sxs-lookup"><span data-stu-id="eda6d-129">SQL Server Management Studio</span></span>
- <span data-ttu-id="eda6d-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="eda6d-130">Visual Studio Code</span></span>

<span data-ttu-id="eda6d-131">Ezek a fiókok replikálása automatikusan tooyour másodlagos kiszolgáló (és szinkronban kell tartani).</span><span class="sxs-lookup"><span data-stu-id="eda6d-131">These user accounts replicate automatically tooyour secondary server (and be kept in sync).</span></span> <span data-ttu-id="eda6d-132">toouse SQL Server Management Studio vagy Visual Studio Code, szükség lehet egy tűzfalszabályt tooconfigure egy ügyfél, amelynek Ön még nincs konfigurálva a tűzfal IP-címen keresztül csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="eda6d-132">toouse SQL Server Management Studio or Visual Studio Code, you may need tooconfigure a firewall rule if you are connecting from a client at an IP address for which you have not yet configured a firewall.</span></span> <span data-ttu-id="eda6d-133">Részletes útmutató: [hozzon létre egy kiszolgálószintű tűzfalszabályt](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).</span><span class="sxs-lookup"><span data-stu-id="eda6d-133">For detailed steps, see [Create a server-level firewall rule](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).</span></span>

- <span data-ttu-id="eda6d-134">A lekérdezési ablakban hajtható végre a következő lekérdezés toocreate két felhasználói fiókokat az adatbázis hello.</span><span class="sxs-lookup"><span data-stu-id="eda6d-134">In a query window, execute hello following query toocreate two user accounts in your database.</span></span> <span data-ttu-id="eda6d-135">Ezt a parancsfájlt biztosít **db_owner** engedélyek toohello **app_admin** fiók és biztosít **kiválasztása** és **frissítés** engedélyek toohello **app_user** fiók.</span><span class="sxs-lookup"><span data-stu-id="eda6d-135">This script grants **db_owner** permissions toohello **app_admin** account and grants **SELECT** and **UPDATE** permissions toohello **app_user** account.</span></span> 

   ```sql
   CREATE USER app_admin WITH PASSWORD = 'ChangeYourPassword1';
   --Add SQL user toodb_owner role
   ALTER ROLE db_owner ADD MEMBER app_admin; 
   --Create additional SQL user
   CREATE USER app_user WITH PASSWORD = 'ChangeYourPassword1';
   --grant permission tooSalesLT schema
   GRANT SELECT, INSERT, DELETE, UPDATE ON SalesLT.Product tooapp_user;
   ```

## <a name="create-database-level-firewall"></a><span data-ttu-id="eda6d-136">Adatbázis-szintű tűzfal létrehozása</span><span class="sxs-lookup"><span data-stu-id="eda6d-136">Create database-level firewall</span></span>

<span data-ttu-id="eda6d-137">Hozzon létre egy [adatbázis-adatbázisszintű tűzfalszabály](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database) az SQL-adatbázis számára.</span><span class="sxs-lookup"><span data-stu-id="eda6d-137">Create a [database-level firewall rule](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database) for your SQL database.</span></span> <span data-ttu-id="eda6d-138">Az adatbázis-adatbázisszintű tűzfalszabály automatikusan replikálja az Ön által létrehozott ebben az oktatóanyagban toohello másodlagos kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="eda6d-138">This database-level firewall rule replicates automatically toohello secondary server that you create in this tutorial.</span></span> <span data-ttu-id="eda6d-139">Az egyszerűség érdekében (a jelen oktatóanyag), amelyen hajtja végre hello lépéseit az oktatóanyag hello számítógép hello nyilvános IP-címét használja.</span><span class="sxs-lookup"><span data-stu-id="eda6d-139">For simplicity (in this tutorial), use hello public IP address of hello computer on which you are performing hello steps in this tutorial.</span></span> <span data-ttu-id="eda6d-140">hello kiszolgálószintű tűzfalszabályt az aktuális számítógépen használt toodetermine hello IP-címmel lásd [hozzon létre egy kiszolgálószintű tűzfal](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).</span><span class="sxs-lookup"><span data-stu-id="eda6d-140">toodetermine hello IP address used for hello server-level firewall rule for your current computer, see [Create a server-level firewall](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).</span></span>  

- <span data-ttu-id="eda6d-141">Nyissa meg a lekérdezésablakban cserélje le hello előző lekérdezés hello lekérdezés a következő, a környezet hello megfelelő IP-címekkel rendelkező hello IP-címek cseréje.</span><span class="sxs-lookup"><span data-stu-id="eda6d-141">In your open query window, replace hello previous query with hello following query, replacing hello IP addresses with hello appropriate IP addresses for your environment.</span></span>  

   ```sql
   -- Create database-level firewall setting for your public IP address
   EXECUTE sp_set_database_firewall_rule @name = N'myGeoReplicationFirewallRule',@start_ip_address = '0.0.0.0', @end_ip_address = '0.0.0.0';
   ```

## <a name="create-an-active-geo-replication-auto-failover-group"></a><span data-ttu-id="eda6d-142">Aktív georeplikáció automatikus feladatátvételi csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="eda6d-142">Create an active geo-replication auto failover group</span></span> 

<span data-ttu-id="eda6d-143">Azure PowerShell használatával hozzon létre egy [aktív georeplikáció automatikus feladatátvételi csoport](sql-database-geo-replication-overview.md) között a meglévő Azure SQL server és az új hello Azure SQL server Azure-régióban üres, és adja hozzá a a minta adatbázis toohello feladatátvételi csoport.</span><span class="sxs-lookup"><span data-stu-id="eda6d-143">Using Azure PowerShell, create an [active geo-replication auto failover group](sql-database-geo-replication-overview.md) between your existing Azure SQL server and hello new empty Azure SQL server in an Azure region, and then add your sample database toohello failover group.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="eda6d-144">Ezeket a parancsmagokat az Azure PowerShell 4.0 szükséges.</span><span class="sxs-lookup"><span data-stu-id="eda6d-144">These cmdlets require Azure PowerShell 4.0.</span></span> [!INCLUDE [sample-powershell-install](../../includes/sample-powershell-install-no-ssh.md)]
>

1. <span data-ttu-id="eda6d-145">Feltöltése a PowerShell-parancsfájlok hello értékek használata a meglévő kiszolgáló és a mintaadatbázis változók, és adjon meg egy globális szinten egyedi érték feladatátvételi csoport neve.</span><span class="sxs-lookup"><span data-stu-id="eda6d-145">Populate variables for your PowerShell scripts using hello values for your existing server and sample database, and provide a globally unique value for failover group name.</span></span>

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

2. <span data-ttu-id="eda6d-146">Hozzon létre egy üres helykiszolgáló biztonsági mentése a feladatátvételi régióban.</span><span class="sxs-lookup"><span data-stu-id="eda6d-146">Create an empty backup server in your failover region.</span></span>

   ```powershell
   $mydrserver = New-AzureRmSqlServer -ResourceGroupName $myresourcegroupname `
      -ServerName $mydrservername `
      -Location $mydrlocation `
      -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $adminlogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))
   $mydrserver   
   ```

3. <span data-ttu-id="eda6d-147">Hozzon létre egy feladatátvevő hello két kiszolgáló között.</span><span class="sxs-lookup"><span data-stu-id="eda6d-147">Create a failover group between hello two servers.</span></span>

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

4. <span data-ttu-id="eda6d-148">Az adatbázis toohello feladatátvételi csoport hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="eda6d-148">Add your database toohello failover group.</span></span>

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

## <a name="install-java-software"></a><span data-ttu-id="eda6d-149">Java szoftver telepítése</span><span class="sxs-lookup"><span data-stu-id="eda6d-149">Install Java software</span></span>

<span data-ttu-id="eda6d-150">Ez a szakasz hello lépések azt feltételezik, hogy ismeri a Java használatával történő fejlesztéséhez, és az Azure SQL Database új tooworking.</span><span class="sxs-lookup"><span data-stu-id="eda6d-150">hello steps in this section assume that you are familiar with developing using Java and are new tooworking with Azure SQL Database.</span></span> 

### <a name="mac-os"></a><span data-ttu-id="eda6d-151">**Mac OS**</span><span class="sxs-lookup"><span data-stu-id="eda6d-151">**Mac OS**</span></span>
<span data-ttu-id="eda6d-152">Nyissa meg a terminált, és keresse meg a tooa könyvtárban, ahol létrehozását tervezi a Java-projektet.</span><span class="sxs-lookup"><span data-stu-id="eda6d-152">Open your terminal and navigate tooa directory where you plan on creating your Java project.</span></span> <span data-ttu-id="eda6d-153">Telepítés **brew** és **Maven** hello a következő parancsok beírásával:</span><span class="sxs-lookup"><span data-stu-id="eda6d-153">Install **brew** and **Maven** by entering hello following commands:</span></span> 

```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew update
brew install maven
```

<span data-ttu-id="eda6d-154">Részletes útmutatást telepítése és konfigurálása a Java és Maven környezet, lépjen a hello [hozza létre az alkalmazását, használja az SQL Server](https://www.microsoft.com/sql-server/developer-get-started/), jelölje be **Java**, jelölje be **MacOS**, és hajtsa végre hello részletes 1.3 és 1.2-es lépésben Java és Maven konfigurálására vonatkozó útmutatásokat.</span><span class="sxs-lookup"><span data-stu-id="eda6d-154">For detailed guidance on installing and configuring Java and Maven environment, go hello [Build an app using SQL Server](https://www.microsoft.com/sql-server/developer-get-started/), select **Java**, select **MacOS**, and then follow hello detailed instructions for configuring Java and Maven in step 1.2 and 1.3.</span></span>

### <a name="linux-ubuntu"></a><span data-ttu-id="eda6d-155">**Linux (Ubuntu)**</span><span class="sxs-lookup"><span data-stu-id="eda6d-155">**Linux (Ubuntu)**</span></span>
<span data-ttu-id="eda6d-156">Nyissa meg a terminált, és keresse meg a tooa könyvtárban, ahol létrehozását tervezi a Java-projektet.</span><span class="sxs-lookup"><span data-stu-id="eda6d-156">Open your terminal and navigate tooa directory where you plan on creating your Java project.</span></span> <span data-ttu-id="eda6d-157">Telepítés **Maven** hello a következő parancsok beírásával:</span><span class="sxs-lookup"><span data-stu-id="eda6d-157">Install **Maven** by entering hello following commands:</span></span>

```bash
sudo apt-get install maven
```

<span data-ttu-id="eda6d-158">Részletes útmutatást telepítése és konfigurálása a Java és Maven környezet, lépjen a hello [hozza létre az alkalmazását, használja az SQL Server](https://www.microsoft.com/sql-server/developer-get-started/), jelölje be **Java**, jelölje be **Ubuntu**, és hajtsa végre hello részletes 1.2-es, az 1.3 és az 1.4-es lépésben Java és Maven konfigurálására vonatkozó útmutatásokat.</span><span class="sxs-lookup"><span data-stu-id="eda6d-158">For detailed guidance on installing and configuring Java and Maven environment, go hello [Build an app using SQL Server](https://www.microsoft.com/sql-server/developer-get-started/), select **Java**, select **Ubuntu**, and then follow hello detailed instructions for configuring Java and Maven in step 1.2, 1.3, and 1.4.</span></span>

### <a name="windows"></a><span data-ttu-id="eda6d-159">**Windows**</span><span class="sxs-lookup"><span data-stu-id="eda6d-159">**Windows**</span></span>
<span data-ttu-id="eda6d-160">Telepítés [Maven](https://maven.apache.org/download.cgi) hello hivatalos installer használatával.</span><span class="sxs-lookup"><span data-stu-id="eda6d-160">Install [Maven](https://maven.apache.org/download.cgi) using hello official installer.</span></span> <span data-ttu-id="eda6d-161">Toohelp függőségeinek kezelése, létre, tesztelheti, és futtassa a Java-projektet Maven használata.</span><span class="sxs-lookup"><span data-stu-id="eda6d-161">Use Maven toohelp manage dependencies, build, test, and run your Java project.</span></span> <span data-ttu-id="eda6d-162">Részletes útmutatást telepítése és konfigurálása a Java és Maven környezet, lépjen a hello [hozza létre az SQL Server használatával alkalmazását](https://www.microsoft.com/sql-server/developer-get-started/), jelölje be **Java**, válassza a Windows, és hajtsa végre hello részletes utasítások a Konfigurálja a Java és Maven 1.3 és 1.2-es lépésben.</span><span class="sxs-lookup"><span data-stu-id="eda6d-162">For detailed guidance on installing and configuring Java and Maven environment, go hello [Build an app using SQL Server](https://www.microsoft.com/sql-server/developer-get-started/), select **Java**, select Windows, and then follow hello detailed instructions for configuring Java and Maven in step 1.2 and 1.3.</span></span>

## <a name="create-sqldbsample-project"></a><span data-ttu-id="eda6d-163">SqlDbSample projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="eda6d-163">Create SqlDbSample project</span></span>

1. <span data-ttu-id="eda6d-164">Hello parancskonzolról (például a Bash), a Maven-projekt létrehozása.</span><span class="sxs-lookup"><span data-stu-id="eda6d-164">In hello command console (such as Bash), create a Maven project.</span></span> 
   ```bash
   mvn archetype:generate "-DgroupId=com.sqldbsamples" "-DartifactId=SqlDbSample" "-DarchetypeArtifactId=maven-archetype-quickstart" "-Dversion=1.0.0"
   ```
2. <span data-ttu-id="eda6d-165">Típus **Y** kattintson **Enter**.</span><span class="sxs-lookup"><span data-stu-id="eda6d-165">Type **Y** and click **Enter**.</span></span>
3. <span data-ttu-id="eda6d-166">Az újonnan létrehozott projektben módosítsa a könyvtárat.</span><span class="sxs-lookup"><span data-stu-id="eda6d-166">Change directories into your newly created project.</span></span>

   ```bash
   cd SqlDbSamples
   ```

4. <span data-ttu-id="eda6d-167">A kedvenc szerkesztővel, nyissa meg a projektmappa hello pom.xml fájlt.</span><span class="sxs-lookup"><span data-stu-id="eda6d-167">Using your favorite editor, open hello pom.xml file in your project folder.</span></span> 

5. <span data-ttu-id="eda6d-168">Adja hozzá a hello JDBC-illesztőt Microsoft SQL Server függőségi tooyour Maven-projektek kedvenc szövegszerkesztőjével megnyitásával, majd a másolás és beillesztés hello a pom.xml fájlt az alábbi.</span><span class="sxs-lookup"><span data-stu-id="eda6d-168">Add hello Microsoft JDBC Driver for SQL Server dependency tooyour Maven project by opening your favorite text editor and copying and pasting hello following lines into your pom.xml file.</span></span> <span data-ttu-id="eda6d-169">Hello meglévő értékek előre kitölti az hello fájl felülírásának mellőzése.</span><span class="sxs-lookup"><span data-stu-id="eda6d-169">Do not overwrite hello existing values prepopulated in hello file.</span></span> <span data-ttu-id="eda6d-170">hello JDBC függőségi hello nagyobb "függőségei" szakasz () kell beilleszteni.</span><span class="sxs-lookup"><span data-stu-id="eda6d-170">hello JDBC dependency must be pasted within hello larger “dependencies” section ( ).</span></span>   

   ```xml
   <dependency>
     <groupId>com.microsoft.sqlserver</groupId>
     <artifactId>mssql-jdbc</artifactId>
    <version>6.1.0.jre8</version>
   </dependency>
   ```

6. <span data-ttu-id="eda6d-171">Adja meg a Java toocompile hello projekt elleni hello verzióját "Tulajdonságok" szakasz hello pom.xml fájlba a következő hello "függőségei" szakasz után hello hozzáadásával.</span><span class="sxs-lookup"><span data-stu-id="eda6d-171">Specify hello version of Java toocompile hello project against by adding hello following “properties” section into hello pom.xml file after hello "dependencies" section.</span></span> 

   ```xml
   <properties>
     <maven.compiler.source>1.8</maven.compiler.source>
     <maven.compiler.target>1.8</maven.compiler.target>
   </properties>
   ```
7. <span data-ttu-id="eda6d-172">Adja hozzá a hello következő "Szerkesztés" szakasz hello pom.xml fájlba után hello "Tulajdonságok" szakasz toosupport jegyzékfájlt a JAR-fájlok kivételével.</span><span class="sxs-lookup"><span data-stu-id="eda6d-172">Add hello following "build" section into hello pom.xml file after hello "properties" section toosupport manifest files in jars.</span></span>       

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
8. <span data-ttu-id="eda6d-173">Mentse és zárja be a hello pom.xml fájlt.</span><span class="sxs-lookup"><span data-stu-id="eda6d-173">Save and close hello pom.xml file.</span></span>
9. <span data-ttu-id="eda6d-174">Nyissa meg hello App.java fájlt (C:\apache-maven-3.5.0\SqlDbSample\src\main\java\com\sqldbsamples\App.java), és cserélje ki a következő tartalma hello hello tartalmát.</span><span class="sxs-lookup"><span data-stu-id="eda6d-174">Open hello App.java file (C:\apache-maven-3.5.0\SqlDbSample\src\main\java\com\sqldbsamples\App.java) and replace hello contents with hello following contents.</span></span> <span data-ttu-id="eda6d-175">Cserélje le a feladatátvételi csoport hello nevét hello feladatátvételi csoport nevét.</span><span class="sxs-lookup"><span data-stu-id="eda6d-175">Replace hello failover group name with hello name for your failover group.</span></span> <span data-ttu-id="eda6d-176">Ha módosította hello hello adatbázis neve, a felhasználó vagy a jelszó, ezeket az értékeket, valamint módosítása</span><span class="sxs-lookup"><span data-stu-id="eda6d-176">If you have changed hello values for hello database name, user, or password, change those values as well.</span></span>

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
6. <span data-ttu-id="eda6d-177">Mentse és zárja be hello App.java fájlt.</span><span class="sxs-lookup"><span data-stu-id="eda6d-177">Save and close hello App.java file.</span></span>

## <a name="compile-and-run-hello-sqldbsample-project"></a><span data-ttu-id="eda6d-178">Fordítsa le, és futtassa a hello SqlDbSample projekt</span><span class="sxs-lookup"><span data-stu-id="eda6d-178">Compile and run hello SqlDbSample project</span></span>

1. <span data-ttu-id="eda6d-179">Hello parancs konzolon toofollowing parancs hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="eda6d-179">In hello command console, execute toofollowing command.</span></span>

   ```bash
   mvn package
   ```
2. <span data-ttu-id="eda6d-180">Ha elkészült, hajtható végre a következő parancs toorun hello alkalmazás (futtatás esetén körülbelül 1 óra kivéve, ha manuálisan leállítása) hello:</span><span class="sxs-lookup"><span data-stu-id="eda6d-180">When finished, execute hello following command toorun hello application (it runs for about 1 hour unless you stop it manually):</span></span>

   ```bash
   mvn -q -e exec:java "-Dexec.mainClass=com.sqldbsamples.App"
   
   #######################################
   ## GEO DISTRIBUTED DATABASE TUTORIAL ##
   #######################################

   1. insert on primary successful, read from secondary successful
   2. insert on primary successful, read from secondary successful
   3. insert on primary successful, read from secondary successful
   ```

## <a name="perform-disaster-recovery-drill"></a><span data-ttu-id="eda6d-181">Hajtsa végre a vész-helyreállítási részletezési</span><span class="sxs-lookup"><span data-stu-id="eda6d-181">Perform disaster recovery drill</span></span>

1. <span data-ttu-id="eda6d-182">Hívja meg a feladatátvételi csoport kézi feladatátvételre.</span><span class="sxs-lookup"><span data-stu-id="eda6d-182">Call manual failover of failover group.</span></span> 

   ```powershell
   Switch-AzureRMSqlDatabaseFailoverGroup `
   -ResourceGroupName $myresourcegroupname  `
   -ServerName $mydrservername `
   -FailoverGroupName $myfailovergroupname
   ```

2. <span data-ttu-id="eda6d-183">Figyelje meg hello alkalmazás eredmények a feladatátvétel során.</span><span class="sxs-lookup"><span data-stu-id="eda6d-183">Observe hello application results during failover.</span></span> <span data-ttu-id="eda6d-184">Néhány beszúrása sikertelen hello DNS-gyorsítótár frissítése közben.</span><span class="sxs-lookup"><span data-stu-id="eda6d-184">Some inserts fail while hello DNS cache refreshes.</span></span>     

3. <span data-ttu-id="eda6d-185">Megtudhatja, milyen működik-e a vész-helyreállítási kiszolgálói szerepkört.</span><span class="sxs-lookup"><span data-stu-id="eda6d-185">Find out which role your disaster recovery server is performing.</span></span>

   ```powershell
   $mydrserver.ReplicationRole
   ```

4. <span data-ttu-id="eda6d-186">Feladat-visszavételre.</span><span class="sxs-lookup"><span data-stu-id="eda6d-186">Failback.</span></span>

   ```powershell
   Switch-AzureRMSqlDatabaseFailoverGroup `
   -ResourceGroupName $myresourcegroupname  `
   -ServerName $myservername `
   -FailoverGroupName $myfailovergroupname
   ```

5. <span data-ttu-id="eda6d-187">Figyelje meg hello alkalmazás eredmények feladat-visszavétel során.</span><span class="sxs-lookup"><span data-stu-id="eda6d-187">Observe hello application results during failback.</span></span> <span data-ttu-id="eda6d-188">Néhány beszúrása sikertelen hello DNS-gyorsítótár frissítése közben.</span><span class="sxs-lookup"><span data-stu-id="eda6d-188">Some inserts fail while hello DNS cache refreshes.</span></span>     

6. <span data-ttu-id="eda6d-189">Megtudhatja, milyen működik-e a vész-helyreállítási kiszolgálói szerepkört.</span><span class="sxs-lookup"><span data-stu-id="eda6d-189">Find out which role your disaster recovery server is performing.</span></span>

   ```powershell
   $fileovergroup = Get-AzureRMSqlDatabaseFailoverGroup `
      -FailoverGroupName $myfailovergroupname `
      -ResourceGroupName $myresourcegroupname `
      -ServerName $mydrservername
   $fileovergroup.ReplicationRole
   ```
## <a name="next-steps"></a><span data-ttu-id="eda6d-190">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="eda6d-190">Next steps</span></span> 

<span data-ttu-id="eda6d-191">További információkért lásd: [aktív georeplikáció és feladatátvételi csoportok](sql-database-geo-replication-overview.md).</span><span class="sxs-lookup"><span data-stu-id="eda6d-191">For more information, see [Active geo-replication and failover groups](sql-database-geo-replication-overview.md).</span></span>
