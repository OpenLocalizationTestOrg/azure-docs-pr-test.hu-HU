---
title: "Azure PowerShell: SQL-adatbázis létrehozása | Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy SQL Database logikai kiszolgálóhoz kiszolgálószintű tűzfalszabályt és adatbázisok hello Azure-portálon."
keywords: "oktatóanyag az SQL Database használatához, SQL-adatbázis létrehozása"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,DBs & servers
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: hero-article
ms.date: 04/17/2017
ms.author: carlrab
ms.openlocfilehash: e89f68b44083a3b64e61f95117dbbedfa6647ccb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-single-azure-sql-database-using-powershell"></a><span data-ttu-id="98738-104">Önálló Azure SQL-adatbázis létrehozása a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="98738-104">Create a single Azure SQL database using PowerShell</span></span>

<span data-ttu-id="98738-105">PowerShell használt toocreate és hello parancssorból vagy parancsfájlokban Azure-erőforrások kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="98738-105">PowerShell is used toocreate and manage Azure resources from hello command line or in scripts.</span></span> <span data-ttu-id="98738-106">Ez útmutató PowerShell toodeploy használatával részletei az Azure SQL-adatbázis egy [Azure erőforráscsoport](../azure-resource-manager/resource-group-overview.md) a egy [Azure SQL Database logikai kiszolgáló](sql-database-features.md).</span><span class="sxs-lookup"><span data-stu-id="98738-106">This guide details using PowerShell toodeploy an Azure SQL database in an [Azure resource group](../azure-resource-manager/resource-group-overview.md) in an [Azure SQL Database logical server](sql-database-features.md).</span></span>

<span data-ttu-id="98738-107">Ha nem rendelkezik Azure-előfizetéssel, első lépésként mindössze néhány perc alatt létrehozhat egy [ingyenes](https://azure.microsoft.com/free/) fiókot.</span><span class="sxs-lookup"><span data-stu-id="98738-107">If you don't have an Azure subscription, create a [free](https://azure.microsoft.com/free/) account before you begin.</span></span>

<span data-ttu-id="98738-108">Ez az oktatóanyag hello Azure PowerShell 4.0-s vagy újabb verziója szükséges.</span><span class="sxs-lookup"><span data-stu-id="98738-108">This tutorial requires hello Azure PowerShell module version 4.0 or later.</span></span> <span data-ttu-id="98738-109">Futtatás ` Get-Module -ListAvailable AzureRM` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="98738-109">Run ` Get-Module -ListAvailable AzureRM` toofind hello version.</span></span> <span data-ttu-id="98738-110">Ha tooinstall vagy frissítés van szüksége, tekintse meg [telepítése Azure PowerShell modul](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="98738-110">If you need tooinstall or upgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span> 

## <a name="log-in-tooazure"></a><span data-ttu-id="98738-111">Jelentkezzen be tooAzure</span><span class="sxs-lookup"><span data-stu-id="98738-111">Log in tooAzure</span></span>

<span data-ttu-id="98738-112">Jelentkezzen be tooyour hello használata Azure-előfizetés [Add-AzureRmAccount](/powershell/module/azurerm.profile/add-azurermaccount) parancsot, és kövesse hello képernyőn megjelenő utasításokat.</span><span class="sxs-lookup"><span data-stu-id="98738-112">Log in tooyour Azure subscription using hello [Add-AzureRmAccount](/powershell/module/azurerm.profile/add-azurermaccount) command and follow hello on-screen directions.</span></span>

```powershell
Add-AzureRmAccount
```

## <a name="create-variables"></a><span data-ttu-id="98738-113">Változók létrehozása</span><span class="sxs-lookup"><span data-stu-id="98738-113">Create variables</span></span>

<span data-ttu-id="98738-114">A gyors üzembe helyezési parancsfájlok hello változókat ad meg.</span><span class="sxs-lookup"><span data-stu-id="98738-114">Define variables for use in hello scripts in this quick start.</span></span>

```powershell
# hello data center and resource name for your resources
$resourcegroupname = "myResourceGroup"
$location = "WestEurope"
# hello logical server name: Use a random value or replace with your own value (do not capitalize)
$servername = "server-$(Get-Random)"
# Set an admin login and password for your database
# hello login information for hello server
$adminlogin = "ServerAdmin"
$password = "ChangeYourAdminPassword1"
# hello ip address range that you want tooallow tooaccess your server - change as appropriate
$startip = "0.0.0.0"
$endip = "0.0.0.0"
# hello database name
$databasename = "mySampleDatabase"
```

## <a name="create-a-resource-group"></a><span data-ttu-id="98738-115">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="98738-115">Create a resource group</span></span>

<span data-ttu-id="98738-116">Hozzon létre egy [Azure erőforráscsoport](../azure-resource-manager/resource-group-overview.md) hello segítségével [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) parancsot.</span><span class="sxs-lookup"><span data-stu-id="98738-116">Create an [Azure resource group](../azure-resource-manager/resource-group-overview.md) using hello [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) command.</span></span> <span data-ttu-id="98738-117">Az erőforráscsoport olyan logikai tároló, amelyben a rendszer üzembe helyezi és csoportként kezeli az Azure-erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="98738-117">A resource group is a logical container into which Azure resources are deployed and managed as a group.</span></span> <span data-ttu-id="98738-118">hello alábbi példa létrehoz egy erőforráscsoportot `myResourceGroup` a hello `westeurope` helyét.</span><span class="sxs-lookup"><span data-stu-id="98738-118">hello following example creates a resource group named `myResourceGroup` in hello `westeurope` location.</span></span>

```powershell
New-AzureRmResourceGroup -Name $resourcegroupname -Location $location
```
## <a name="create-a-logical-server"></a><span data-ttu-id="98738-119">Hozzon létre egy logikai kiszolgálót</span><span class="sxs-lookup"><span data-stu-id="98738-119">Create a logical server</span></span>

<span data-ttu-id="98738-120">Hozzon létre egy [Azure SQL Database logikai kiszolgáló](sql-database-features.md) hello segítségével [New-AzureRmSqlServer](/powershell/module/azurerm.sql/new-azurermsqlserver) parancsot.</span><span class="sxs-lookup"><span data-stu-id="98738-120">Create an [Azure SQL Database logical server](sql-database-features.md) using hello [New-AzureRmSqlServer](/powershell/module/azurerm.sql/new-azurermsqlserver) command.</span></span> <span data-ttu-id="98738-121">A logikai kiszolgálók adatbázisok egy csoportját tartalmazzák, amelyeket a rendszer egy csoportként kezel.</span><span class="sxs-lookup"><span data-stu-id="98738-121">A logical server contains a group of databases managed as a group.</span></span> <span data-ttu-id="98738-122">hello alábbi példa hoz létre egy véletlenszerűen előállított nevű kiszolgálót az erőforráscsoportban egy rendszergazdai bejelentkezés nevű `ServerAdmin` és egy jelszót `ChangeYourAdminPassword1`.</span><span class="sxs-lookup"><span data-stu-id="98738-122">hello following example creates a randomly named server in your resource group with an admin login named `ServerAdmin` and a password of `ChangeYourAdminPassword1`.</span></span> <span data-ttu-id="98738-123">Igény szerint cserélje le ezeket az előre meghatározott értékeket.</span><span class="sxs-lookup"><span data-stu-id="98738-123">Replace these pre-defined values as desired.</span></span>

```powershell
New-AzureRmSqlServer -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -Location $location `
    -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $adminlogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))
```

## <a name="configure-a-server-firewall-rule"></a><span data-ttu-id="98738-124">Konfiguráljon egy kiszolgálói tűzfalszabályt</span><span class="sxs-lookup"><span data-stu-id="98738-124">Configure a server firewall rule</span></span>

<span data-ttu-id="98738-125">Hozzon létre egy [Azure SQL Database kiszolgálószintű tűzfalszabály](sql-database-firewall-configure.md) hello segítségével [New-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule) parancsot.</span><span class="sxs-lookup"><span data-stu-id="98738-125">Create an [Azure SQL Database server-level firewall rule](sql-database-firewall-configure.md) using hello [New-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule) command.</span></span> <span data-ttu-id="98738-126">Egy kiszolgálószintű tűzfalszabályt lehetővé teszi, hogy a külső alkalmazás, például az SQL Server Management Studio vagy hello SQLCMD segédprogram tooconnect tooa SQL-adatbázis hello SQL Database szolgáltatás tűzfalon keresztül.</span><span class="sxs-lookup"><span data-stu-id="98738-126">A server-level firewall rule allows an external application, such as SQL Server Management Studio or hello SQLCMD utility tooconnect tooa SQL database through hello SQL Database service firewall.</span></span> <span data-ttu-id="98738-127">A következő példa hello hello tűzfal van csak megnyitva más Azure-erőforrások.</span><span class="sxs-lookup"><span data-stu-id="98738-127">In hello following example, hello firewall is only opened for other Azure resources.</span></span> <span data-ttu-id="98738-128">külső kapcsolatot tooenable, módosítás hello IP-cím tooan megfelelő címet a környezetben.</span><span class="sxs-lookup"><span data-stu-id="98738-128">tooenable external connectivity, change hello IP address tooan appropriate address for your environment.</span></span> <span data-ttu-id="98738-129">tooopen összes IP-cím, IP-cím és 255.255.255.255 indítása zárócíme hello hello 0.0.0.0 használják.</span><span class="sxs-lookup"><span data-stu-id="98738-129">tooopen all IP addresses, use 0.0.0.0 as hello starting IP address and 255.255.255.255 as hello ending address.</span></span>

```powershell
New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -FirewallRuleName "AllowSome" -StartIpAddress $startip -EndIpAddress $endip
```

> [!NOTE]
> <span data-ttu-id="98738-130">Az SQL Database az 1433-as porton kommunikál.</span><span class="sxs-lookup"><span data-stu-id="98738-130">SQL Database communicates over port 1433.</span></span> <span data-ttu-id="98738-131">Ha a vállalati hálózatból származó tooconnect, a hálózati tűzfal előfordulhat, hogy nem engedélyezett a 1433-as port kimenő forgalmát.</span><span class="sxs-lookup"><span data-stu-id="98738-131">If you are trying tooconnect from within a corporate network, outbound traffic over port 1433 may not be allowed by your network's firewall.</span></span> <span data-ttu-id="98738-132">Ha igen, csak akkor tudja tooconnect tooyour Azure SQL adatbázis-kiszolgáló kivéve, ha az IT-részleg megnyitja az 1433-as port.</span><span class="sxs-lookup"><span data-stu-id="98738-132">If so, you will not be able tooconnect tooyour Azure SQL Database server unless your IT department opens port 1433.</span></span>
>

## <a name="create-a-database-in-hello-server-with-sample-data"></a><span data-ttu-id="98738-133">Hozzon létre egy adatbázist mintaadatokkal hello kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="98738-133">Create a database in hello server with sample data</span></span>

<span data-ttu-id="98738-134">Hozzon létre egy adatbázist, egy [S0 teljesítményszint szükséges](sql-database-service-tiers.md) hello segítségével hello Server [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase) parancsot.</span><span class="sxs-lookup"><span data-stu-id="98738-134">Create a database with an [S0 performance level](sql-database-service-tiers.md) in hello server using hello [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase) command.</span></span> <span data-ttu-id="98738-135">hello alábbi példa létrehoz egy nevű adatbázist `mySampleDatabase` és terhelések hello AdventureWorksLT mintaadatokat az adatbázisba.</span><span class="sxs-lookup"><span data-stu-id="98738-135">hello following example creates a database called `mySampleDatabase` and loads hello AdventureWorksLT sample data into this database.</span></span> <span data-ttu-id="98738-136">Cserélje le ezeket az előre megadott értékek a kívánt módon (a gyűjtemény build a gyors üzembe helyezési hello értékek esetén más gyors üzembe helyezések).</span><span class="sxs-lookup"><span data-stu-id="98738-136">Replace these predefined values as desired (other quick starts in this collection build upon hello values in this quick start).</span></span>

```powershell
New-AzureRmSqlDatabase  -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -DatabaseName $databasename `
    -SampleName "AdventureWorksLT" `
    -RequestedServiceObjectiveName "S0"
```

## <a name="clean-up-resources"></a><span data-ttu-id="98738-137">Az erőforrások eltávolítása</span><span class="sxs-lookup"><span data-stu-id="98738-137">Clean up resources</span></span>

<span data-ttu-id="98738-138">Az ebben a gyűjteményben lévő többi rövid útmutató erre a rövid útmutatóra épít.</span><span class="sxs-lookup"><span data-stu-id="98738-138">Other quick starts in this collection build upon this quick start.</span></span> 

> [!TIP]
> <span data-ttu-id="98738-139">Ha az ezt követő gyors üzembe helyezések toowork toocontinue tervezi, karbantartása hello erőforrások létrehozása a gyors indításának mellőzése.</span><span class="sxs-lookup"><span data-stu-id="98738-139">If you plan toocontinue on toowork with subsequent quick starts, do not clean up hello resources created in this quick start.</span></span> <span data-ttu-id="98738-140">Ha nem tervezi toocontinue, használja a hello által a hello Azure-portálon a gyors üzembe helyezési lépéseket toodelete összes erőforrás létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="98738-140">If you do not plan toocontinue, use hello following steps toodelete all resources created by this quick start in hello Azure portal.</span></span>
>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName $resourcegroupname
```

## <a name="next-steps"></a><span data-ttu-id="98738-141">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="98738-141">Next steps</span></span>

<span data-ttu-id="98738-142">Most, hogy rendelkezik egy adatbázissal, csatlakoztathatja a kedvenc eszközeit, és lekérdezéseket hajthat végre velük.</span><span class="sxs-lookup"><span data-stu-id="98738-142">Now that you have a database, you can connect and query using your favorite tools.</span></span> <span data-ttu-id="98738-143">További információkért válassza ki az eszközt az alábbiak közül:</span><span class="sxs-lookup"><span data-stu-id="98738-143">Learn more by choosing your tool below:</span></span>

- [<span data-ttu-id="98738-144">SQL Server Management Studio</span><span class="sxs-lookup"><span data-stu-id="98738-144">SQL Server Management Studio</span></span>](sql-database-connect-query-ssms.md)
- [<span data-ttu-id="98738-145">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="98738-145">Visual Studio Code</span></span>](sql-database-connect-query-vscode.md)
- [<span data-ttu-id="98738-146">.NET</span><span class="sxs-lookup"><span data-stu-id="98738-146">.NET</span></span>](sql-database-connect-query-dotnet.md)
- [<span data-ttu-id="98738-147">PHP</span><span class="sxs-lookup"><span data-stu-id="98738-147">PHP</span></span>](sql-database-connect-query-php.md)
- [<span data-ttu-id="98738-148">Node.js</span><span class="sxs-lookup"><span data-stu-id="98738-148">Node.js</span></span>](sql-database-connect-query-nodejs.md)
- [<span data-ttu-id="98738-149">Java</span><span class="sxs-lookup"><span data-stu-id="98738-149">Java</span></span>](sql-database-connect-query-java.md)
- [<span data-ttu-id="98738-150">Python</span><span class="sxs-lookup"><span data-stu-id="98738-150">Python</span></span>](sql-database-connect-query-python.md)
- [<span data-ttu-id="98738-151">Ruby</span><span class="sxs-lookup"><span data-stu-id="98738-151">Ruby</span></span>](sql-database-connect-query-ruby.md)

