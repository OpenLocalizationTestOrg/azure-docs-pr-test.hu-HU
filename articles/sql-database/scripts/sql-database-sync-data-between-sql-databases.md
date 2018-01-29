---
title: "PowerShell – példa-szinkronizálás több Azure SQL-adatbázisok közötti |} Microsoft Docs"
description: "Az Azure PowerShell-példa parancsfájl több Azure SQL-adatbázisok közötti szinkronizálása"
services: sql-database
documentationcenter: sql-database
author: jognanay
manager: craigg
editor: 
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: load & move data, mvc
ms.devlang: PowerShell
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 07/31/2017
ms.author: jognanay
ms.reviewer: douglasl
ms.openlocfilehash: fe52538c95619cd5bf69ff706ec266195194ace9
ms.sourcegitcommit: 4ac89872f4c86c612a71eb7ec30b755e7df89722
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 12/07/2017
---
# <a name="use-powershell-to-sync-between-multiple-sql-databases"></a>A PowerShell szolgáltatás használatával több SQL-adatbázisok közötti szinkronizálása
 
A PowerShell-példa adatszinkronizálás több Azure SQL-adatbázisok közötti szinkronizálására konfigurálja.

Ez a minta az Azure PowerShell 4.2 vagy újabb verziója szükséges. Futtatás `Get-Module -ListAvailable AzureRM` telepített verziója található. Ha telepíteni vagy frissíteni szeretne, olvassa el [az Azure PowerShell-modul telepítését](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) ismertető cikket.
 
Futtatás `Login-AzureRmAccount` kapcsolat létrehozása az Azure-ral.

SQL adatszinkronizálás áttekintését lásd: [adatok szinkronizálásának több felhőalapú és helyszíni adatbázisokat az Azure SQL adatszinkronizálás (előzetes verzió)](../sql-database-sync-data.md).

## <a name="sample-script"></a>Mintaparancsfájl

```powershell
# prerequisites: 
# 1. Create an Azure Database from AdventureWorksLT sample database as hub database
# 2. Create an Azure Database in the same region as sync database
# 3. Create an Azure Database as member database
# 4. Update the parameters below before running the sample
#
using namespace Microsoft.Azure.Commands.Sql.DataSync.Model
using namespace System.Collections.Generic

# Hub database info
# Subscription id for hub database
$SubscriptionId = "subscription_guid"
# Resource group name for hub database
$ResourceGroupName = "ResourceGroup"
# Server name for hub database
$ServerName = "Server"
# Database name for hub database
$DatabaseName = "AdventureWorks"

# Sync database info
# Resource group name for sync database
$SyncDatabaseResourceGroupName = "ResourceGroup"
# Server name for sync database
$SyncDatabaseServerName = "Server"
# Sync database name
$SyncDatabaseName = "SyncDatabase"

# Sync group info
# Sync group name
$SyncGroupName = "SampleSyncGroup1"
# Conflict resolution Policy. Value can be HubWin or MemberWin
$ConflictResolutionPolicy = "HubWin"
# Sync interval in seconds. Value must be no less than 300
$IntervalInSeconds = 300

# Member database info
# Member name
$SyncMemberName = "member"
# Member server name
$MemberServerName = "MemberServer"
# Member database name
$MemberDatabaseName = "SyncDatabase1"
# Member database type. Value can be AzureSqlDatabase or SqlServerDatabase
$MemberDatabaseType = "AzureSqlDatabase"
# Sync direction. Value can be Bidirectional, Onewaymembertohub, Onewayhubtomember
$SyncDirection = "Bidirectional"

# Other info
# Temp file to save the sync schema
$TempFile = $env:TEMP+"\syncSchema.json"

# List of included columns and tables in quoted name
$IncludedColumnsAndTables =  "[SalesLT].[Address].[AddressID]",
                             "[SalesLT].[Address].[AddressLine2]",
                             "[SalesLT].[Address].[rowguid]",
                             "[SalesLT].[Address].[PostalCode]",
                             "[SalesLT].[ProductDescription]"
$MetadataList = [System.Collections.ArrayList]::new($IncludedColumnsAndTables)


add-azurermaccount 
select-azurermsubscription -SubscriptionId $SubscriptionId

# Use this section if it is safe to show password in the script.
# Otherwise, use the PromptForCredential
# $User = "username"
# $PWord = ConvertTo-SecureString -String "Password" -AsPlainText -Force
# $Credential = New-Object -TypeName "System.Management.Automation.PSCredential" -ArgumentList $User, $PWord

$Credential = $Host.ui.PromptForCredential("Need credential", 
              "Please enter your user name and password for server "+$ServerName+".database.windows.net", 
              "", 
              "")

# Create a new sync group
Write-Host "Creating Sync Group"$SyncGroupName
New-AzureRmSqlSyncGroup   -ResourceGroupName $ResourceGroupName `
                            -ServerName $ServerName `
                            -DatabaseName $DatabaseName `
                            -Name $SyncGroupName `
                            -SyncDatabaseName $SyncDatabaseName `
                            -SyncDatabaseServerName $SyncDatabaseServerName `
                            -SyncDatabaseResourceGroupName $SyncDatabaseResourceGroupName `
                            -ConflictResolutionPolicy $ConflictResolutionPolicy `
                            -DatabaseCredential $Credential

# Use this section if it is safe to show password in the script.
#$User = "username"
#$Password = ConvertTo-SecureString -String "password" -AsPlainText -Force
#$Credential = New-Object -TypeName "System.Management.Automation.PSCredential" -ArgumentList $User, $Password

$Credential = $Host.ui.PromptForCredential("Need credential", 
              "Please enter your user name and password for server "+$MemberServerName, 
              "", 
              "")

# Add a new sync member
Write-Host "Adding member"$SyncMemberName" to the sync group"
New-AzureRmSqlSyncMember   -ResourceGroupName $ResourceGroupName `
                            -ServerName $ServerName `
                            -DatabaseName $DatabaseName `
                            -SyncGroupName $SyncGroupName `
                            -Name $SyncMemberName `
                            -MemberDatabaseCredential $Credential `
                            -MemberDatabaseName $MemberDatabaseName `
                            -MemberServerName ($MemberServerName + ".database.windows.net") `
                            -MemberDatabaseType $MemberDatabaseType `
                            -SyncDirection $SyncDirection

# Refresh database schema from hub database
# Specify the -SyncMemberName parameter if you want to refresh schema from the member database
Write-Host "Refreshing database schema from hub database"
$StartTime= Get-Date
Update-AzureRmSqlSyncSchema   -ResourceGroupName $ResourceGroupName `
                              -ServerName $ServerName `
                              -DatabaseName $DatabaseName `
                              -SyncGroupName $SyncGroupName


#Waiting for successful refresh

$StartTime=$StartTime.ToUniversalTime()
$timer=0
$timeout=90
# Check the log and see if refresh has gone through
Write-Host "Check for successful refresh"
$IsSucceeded = $false
While ($IsSucceeded -eq $false)
{
    Start-Sleep -s 10
    $timer=$timer+10
    $Details = Get-AzureRmSqlSyncSchema -SyncGroupName $SyncGroupName -ServerName $ServerName -DatabaseName $DatabaseName -ResourceGroupName $ResourceGroupName
    if ($Details.LastUpdateTime -gt $StartTime)
      {
        Write-Host "Refresh was successful"
        $IsSucceeded = $true
      }
    if ($timer -eq $timeout) 
      {
              Write-Host "Refresh timed out"
        break;
      }
}



# Get the database schema 
Write-Host "Adding tables and columns to the sync schema"
$databaseSchema = Get-AzureRmSqlSyncSchema   -ResourceGroupName $ResourceGroupName `
                                             -ServerName $ServerName `
                                             -DatabaseName $DatabaseName `
                                             -SyncGroupName $SyncGroupName `

$databaseSchema | ConvertTo-Json -depth 5 -Compress | Out-File "c:\tmp\databaseSchema"     
$newSchema = [AzureSqlSyncGroupSchemaModel]::new()
$newSchema.Tables = [List[AzureSqlSyncGroupSchemaTableModel]]::new();

# Add columns and tables to the sync schema
foreach ($tableSchema in $databaseSchema.Tables)
{
    $newTableSchema = [AzureSqlSyncGroupSchemaTableModel]::new()
    $newTableSchema.QuotedName = $tableSchema.QuotedName
    $newTableSchema.Columns = [List[AzureSqlSyncGroupSchemaColumnModel]]::new();
    $addAllColumns = $false
    if ($MetadataList.Contains($tableSchema.QuotedName))
    {
        if ($tableSchema.HasError)
        {
            $fullTableName = $tableSchema.QuotedName
            Write-Host "Can't add table $fullTableName to the sync schema" -foregroundcolor "Red"
            Write-Host $tableSchema.ErrorId -foregroundcolor "Red"
            continue;
        }
        else
        {
            $addAllColumns = $true
        }
    }
    foreach($columnSchema in $tableSchema.Columns)
    {
        $fullColumnName = $tableSchema.QuotedName + "." + $columnSchema.QuotedName
        if ($addAllColumns -or $MetadataList.Contains($fullColumnName))
        {
            if ((-not $addAllColumns) -and $tableSchema.HasError)
            {
                Write-Host "Can't add column $fullColumnName to the sync schema" -foregroundcolor "Red"
                Write-Host $tableSchema.ErrorId -foregroundcolor "Red"c            }
            elseif ((-not $addAllColumns) -and $columnSchema.HasError)
            {
                Write-Host "Can't add column $fullColumnName to the sync schema" -foregroundcolor "Red"
                Write-Host $columnSchema.ErrorId -foregroundcolor "Red"
            }
            else
            {
                Write-Host "Adding"$fullColumnName" to the sync schema"
                $newColumnSchema = [AzureSqlSyncGroupSchemaColumnModel]::new()
                $newColumnSchema.QuotedName = $columnSchema.QuotedName
                $newColumnSchema.DataSize = $columnSchema.DataSize
                $newColumnSchema.DataType = $columnSchema.DataType
                $newTableSchema.Columns.Add($newColumnSchema)
            }
        }
    }
    if ($newTableSchema.Columns.Count -gt 0)
    {
        $newSchema.Tables.Add($newTableSchema)
    }
}

# Convert sync schema to Json format
$schemaString = $newSchema | ConvertTo-Json -depth 5 -Compress

# workaround a powershell bug
$schemaString = $schemaString.Replace('"Tables"', '"tables"').Replace('"Columns"', '"columns"').Replace('"QuotedName"', '"quotedName"').Replace('"MasterSyncMemberName"','"masterSyncMemberName"')

# Save the sync schema to a temp file
$schemaString | Out-File $TempFile

# Update sync schema
Write-Host "Updating the sync schema"
Update-AzureRmSqlSyncGroup  -ResourceGroupName $ResourceGroupName `
                            -ServerName $ServerName `
                            -DatabaseName $DatabaseName `
                            -Name $SyncGroupName `
                            -Schema $TempFile

$SyncStartTime = Get-Date

# Trigger sync manually
Write-Host "Trigger sync manually"
Start-AzureRmSqlSyncGroupSync  -ResourceGroupName $ResourceGroupName `
                               -ServerName $ServerName `
                               -DatabaseName $DatabaseName `
                               -SyncGroupName $SyncGroupName

# Check the sync log and wait until the first sync succeeded
Write-Host "Check the sync log"
$IsSucceeded = $false
For ($i = 0; ($i -lt 300) -and (-not $IsSucceeded); $i = $i + 10)
{
    Start-Sleep -s 10
    $SyncLogEndTime = Get-Date
    $SyncLogList = Get-AzureRmSqlSyncGroupLog  -ResourceGroupName $ResourceGroupName `
                                           -ServerName $ServerName `
                                           -DatabaseName $DatabaseName `
                                           -SyncGroupName $SyncGroupName `
                                           -StartTime $SyncLogStartTime.ToUniversalTime() `
                                           -EndTime $SyncLogEndTime.ToUniversalTime()
    if ($SynclogList.Length -gt 0)
    {
        foreach ($SyncLog in $SyncLogList)
        {
            if ($SyncLog.Details.Contains("Sync completed successfully"))
            {
                Write-Host $SyncLog.TimeStamp : $SyncLog.Details
                $IsSucceeded = $true
            }
        }
    }
}

if ($IsSucceeded)
{
    # Enable scheduled sync
    Write-Host "Enable the scheduled sync with 300 seconds interval"
    Update-AzureRmSqlSyncGroup  -ResourceGroupName $ResourceGroupName `
                                -ServerName $ServerName `
                                -DatabaseName $DatabaseName `
                                -Name $SyncGroupName `
                                -IntervalInSeconds $IntervalInSeconds
}
else
{
    # Output all log if sync doesn't succeed in 300 seconds
    $SyncLogEndTime = Get-Date
    $SyncLogList = Get-AzureRmSqlSyncGroupLog  -ResourceGroupName $ResourceGroupName `
                                           -ServerName $ServerName `
                                           -DatabaseName $DatabaseName `
                                           -SyncGroupName $SyncGroupName `
                                           -StartTime $SyncLogStartTime.ToUniversalTime() `
                                           -EndTime $SyncLogEndTime.ToUniversalTime()
    if ($SynclogList.Length -gt 0)
    {
        foreach ($SyncLog in $SyncLogList)
        {
            Write-Host $SyncLog.TimeStamp : $SyncLog.Details
        }
    }
}

# Clean up deployment 
# Remove-AzureRmResourceGroup -ResourceGroupName $resourcegroupname
# Remove-AzureRmResourceGroup -ResourceGroupName $SyncDatabaseResourceGroupName

```

## <a name="clean-up-deployment"></a>Az üzemelő példány eltávolítása

A minta-parancsprogram futtatása után futtathatja a következő parancs futtatásával távolítsa el az erőforráscsoportot és a vele társított összes erőforrást.

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName $ResourceGroupName
Remove-AzureRmResourceGroup -ResourceGroupName $SyncDatabaseResourceGroupName
```

## <a name="script-explanation"></a>Parancsfájl ismertetése

A parancsfájl a következő parancsokat. Minden egyes parancsa a tábla-parancs-specifikus dokumentációjára mutató hivatkozásokat.

| Parancs | Megjegyzések |
|---|---|
| [Új AzureRmSqlSyncAgent](/powershell/module/azurerm.sql/New-AzureRmSqlSyncAgent) |  Létrehoz egy új szinkronizálási ügynök |
| [Új AzureRmSqlSyncAgentKey](/powershell/module/azurerm.sql/New-AzureRmSqlSyncAgentKey) |  A szinkronizálási ügynök tartozó ügynök kulcsot hoz létre |
| [Get-AzureRmSqlSyncAgentLinkedDatabase](/powershell/module/azurerm.sql/Get-AzureRmSqlSyncAgentLinkedDatabase) |  A Sync-ügynök az összes adat beolvasása |
| [Új AzureRmSqlSyncMember](/powershell/module/azurerm.sql/New-AzureRmSqlSyncMember) |  Új tag hozzáadása a szinkronizálási csoport |
| [Frissítés-AzureRmSqlSyncSchema](/powershell/module/azurerm.sql/Update-AzureRmSqlSyncSchema) |  Az adatbázis-séma információinak frissítése |
| [Get-AzureRmSqlSyncSchema](/powershell/module/azurerm.sql/Get-AzureRmSqlSyncSchem) |  Az adatbázis lekérése |
| [Frissítés-AzureRmSqlSyncGroup](/powershell/module/azurerm.sql/Update-AzureRmSqlSyncGroup) |  A szinkronizálás csoport frissítése |
| [Start-AzureRmSqlSyncGroupSync](/powershell/module/azurerm.sql/Start-AzureRmSqlSyncGroupSync) | Elindítja a szinkronizálási |
| [Get-AzureRmSqlSyncGroupLog](/powershell/module/azurerm.sql/Get-AzureRmSqlSyncGroupLog) |  Ellenőrzi a szinkronizálási napló |
|||

## <a name="next-steps"></a>Következő lépések

Azure PowerShell kapcsolatos további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).

További SQL Database PowerShell parancsfájl minták található [Azure SQL Database PowerShell-parancsfájlok](../sql-database-powershell-samples.md).

SQL adatszinkronizálás kapcsolatos további információkért lásd:

-   [Szinkronizálja az adatokat több felhőalapú és helyszíni adatbázisokat az Azure SQL adatszinkronizálás](../sql-database-sync-data.md)
-   [Azure SQL Data szinkronizálás beállítása](../sql-database-get-started-sql-data-sync.md)
-   [Ajánlott eljárások az Azure SQL-adatok szinkronizálása](../sql-database-best-practices-data-sync.md)
-   [A figyelő az Azure SQL adatszinkronizálás az OMS szolgáltatáshoz](../sql-database-sync-monitor-oms.md)
-   [Az Azure SQL adatszinkronizálás problémák elhárítása](../sql-database-troubleshoot-data-sync.md)

-   PowerShell-példák bemutatják, hogyan konfigurálja az SQL adatszinkronizálás befejezése:
    -   [Egy Azure SQL-adatbázis és a helyszíni SQL Server-adatbázisok közötti szinkronizálása a PowerShell használatával](sql-database-sync-data-between-azure-onprem.md)

-   [Töltse le az SQL Data szinkronizálási REST API dokumentációja](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_REST_API.pdf?raw=true)

SQL-adatbázis kapcsolatos további információkért lásd:

-   [SQL-adatbázis – áttekintés](../sql-database-technical-overview.md)
-   [Adatbázis életciklusának kezelésére](https://msdn.microsoft.com/library/jj907294.aspx)
