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
# <a name="create-a-single-azure-sql-database-using-powershell"></a>Önálló Azure SQL-adatbázis létrehozása a PowerShell használatával

PowerShell használt toocreate és hello parancssorból vagy parancsfájlokban Azure-erőforrások kezeléséhez. Ez útmutató PowerShell toodeploy használatával részletei az Azure SQL-adatbázis egy [Azure erőforráscsoport](../azure-resource-manager/resource-group-overview.md) a egy [Azure SQL Database logikai kiszolgáló](sql-database-features.md).

Ha nem rendelkezik Azure-előfizetéssel, első lépésként mindössze néhány perc alatt létrehozhat egy [ingyenes](https://azure.microsoft.com/free/) fiókot.

Ez az oktatóanyag hello Azure PowerShell 4.0-s vagy újabb verziója szükséges. Futtatás ` Get-Module -ListAvailable AzureRM` toofind hello verziója. Ha tooinstall vagy frissítés van szüksége, tekintse meg [telepítése Azure PowerShell modul](/powershell/azure/install-azurerm-ps). 

## <a name="log-in-tooazure"></a>Jelentkezzen be tooAzure

Jelentkezzen be tooyour hello használata Azure-előfizetés [Add-AzureRmAccount](/powershell/module/azurerm.profile/add-azurermaccount) parancsot, és kövesse hello képernyőn megjelenő utasításokat.

```powershell
Add-AzureRmAccount
```

## <a name="create-variables"></a>Változók létrehozása

A gyors üzembe helyezési parancsfájlok hello változókat ad meg.

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

## <a name="create-a-resource-group"></a>Hozzon létre egy erőforráscsoportot

Hozzon létre egy [Azure erőforráscsoport](../azure-resource-manager/resource-group-overview.md) hello segítségével [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) parancsot. Az erőforráscsoport olyan logikai tároló, amelyben a rendszer üzembe helyezi és csoportként kezeli az Azure-erőforrásokat. hello alábbi példa létrehoz egy erőforráscsoportot `myResourceGroup` a hello `westeurope` helyét.

```powershell
New-AzureRmResourceGroup -Name $resourcegroupname -Location $location
```
## <a name="create-a-logical-server"></a>Hozzon létre egy logikai kiszolgálót

Hozzon létre egy [Azure SQL Database logikai kiszolgáló](sql-database-features.md) hello segítségével [New-AzureRmSqlServer](/powershell/module/azurerm.sql/new-azurermsqlserver) parancsot. A logikai kiszolgálók adatbázisok egy csoportját tartalmazzák, amelyeket a rendszer egy csoportként kezel. hello alábbi példa hoz létre egy véletlenszerűen előállított nevű kiszolgálót az erőforráscsoportban egy rendszergazdai bejelentkezés nevű `ServerAdmin` és egy jelszót `ChangeYourAdminPassword1`. Igény szerint cserélje le ezeket az előre meghatározott értékeket.

```powershell
New-AzureRmSqlServer -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -Location $location `
    -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $adminlogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))
```

## <a name="configure-a-server-firewall-rule"></a>Konfiguráljon egy kiszolgálói tűzfalszabályt

Hozzon létre egy [Azure SQL Database kiszolgálószintű tűzfalszabály](sql-database-firewall-configure.md) hello segítségével [New-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule) parancsot. Egy kiszolgálószintű tűzfalszabályt lehetővé teszi, hogy a külső alkalmazás, például az SQL Server Management Studio vagy hello SQLCMD segédprogram tooconnect tooa SQL-adatbázis hello SQL Database szolgáltatás tűzfalon keresztül. A következő példa hello hello tűzfal van csak megnyitva más Azure-erőforrások. külső kapcsolatot tooenable, módosítás hello IP-cím tooan megfelelő címet a környezetben. tooopen összes IP-cím, IP-cím és 255.255.255.255 indítása zárócíme hello hello 0.0.0.0 használják.

```powershell
New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -FirewallRuleName "AllowSome" -StartIpAddress $startip -EndIpAddress $endip
```

> [!NOTE]
> Az SQL Database az 1433-as porton kommunikál. Ha a vállalati hálózatból származó tooconnect, a hálózati tűzfal előfordulhat, hogy nem engedélyezett a 1433-as port kimenő forgalmát. Ha igen, csak akkor tudja tooconnect tooyour Azure SQL adatbázis-kiszolgáló kivéve, ha az IT-részleg megnyitja az 1433-as port.
>

## <a name="create-a-database-in-hello-server-with-sample-data"></a>Hozzon létre egy adatbázist mintaadatokkal hello kiszolgáló

Hozzon létre egy adatbázist, egy [S0 teljesítményszint szükséges](sql-database-service-tiers.md) hello segítségével hello Server [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase) parancsot. hello alábbi példa létrehoz egy nevű adatbázist `mySampleDatabase` és terhelések hello AdventureWorksLT mintaadatokat az adatbázisba. Cserélje le ezeket az előre megadott értékek a kívánt módon (a gyűjtemény build a gyors üzembe helyezési hello értékek esetén más gyors üzembe helyezések).

```powershell
New-AzureRmSqlDatabase  -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -DatabaseName $databasename `
    -SampleName "AdventureWorksLT" `
    -RequestedServiceObjectiveName "S0"
```

## <a name="clean-up-resources"></a>Az erőforrások eltávolítása

Az ebben a gyűjteményben lévő többi rövid útmutató erre a rövid útmutatóra épít. 

> [!TIP]
> Ha az ezt követő gyors üzembe helyezések toowork toocontinue tervezi, karbantartása hello erőforrások létrehozása a gyors indításának mellőzése. Ha nem tervezi toocontinue, használja a hello által a hello Azure-portálon a gyors üzembe helyezési lépéseket toodelete összes erőforrás létrehozása után.
>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName $resourcegroupname
```

## <a name="next-steps"></a>Következő lépések

Most, hogy rendelkezik egy adatbázissal, csatlakoztathatja a kedvenc eszközeit, és lekérdezéseket hajthat végre velük. További információkért válassza ki az eszközt az alábbiak közül:

- [SQL Server Management Studio](sql-database-connect-query-ssms.md)
- [Visual Studio Code](sql-database-connect-query-vscode.md)
- [.NET](sql-database-connect-query-dotnet.md)
- [PHP](sql-database-connect-query-php.md)
- [Node.js](sql-database-connect-query-nodejs.md)
- [Java](sql-database-connect-query-java.md)
- [Python](sql-database-connect-query-python.md)
- [Ruby](sql-database-connect-query-ruby.md)

