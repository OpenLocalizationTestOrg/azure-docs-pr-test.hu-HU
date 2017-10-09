---
title: "aaaPowerShell példa-szinkronizálás több Azure SQL-adatbázisok közötti |} Microsoft Docs"
description: "Az Azure PowerShell példa parancsfájl toosync több Azure SQL-adatbázisok között"
services: sql-database
documentationcenter: sql-database
author: jognanay
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: load & move data
ms.devlang: PowerShell
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 07/31/2017
ms.author: douglasl
ms.openlocfilehash: 5bf92da25051bd53db536b295959b6f9af9625a7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-powershell-toosync-between-multiple-azure-sql-databases"></a><span data-ttu-id="ee7ec-103">PowerShell toosync több Azure SQL-adatbázist használja</span><span class="sxs-lookup"><span data-stu-id="ee7ec-103">Use PowerShell toosync between multiple Azure SQL databases</span></span>
 
<span data-ttu-id="ee7ec-104">A PowerShell-példa több Azure SQL-adatbázisok közötti adatszinkronizálás toosync konfigurálja.</span><span class="sxs-lookup"><span data-stu-id="ee7ec-104">This PowerShell example configures Data Sync toosync between multiple Azure SQL databases.</span></span>

<span data-ttu-id="ee7ec-105">Ez a minta hello Azure PowerShell 4.2 vagy újabb verziója szükséges.</span><span class="sxs-lookup"><span data-stu-id="ee7ec-105">This sample requires hello Azure PowerShell module version 4.2 or later.</span></span> <span data-ttu-id="ee7ec-106">Futtatás `Get-Module -ListAvailable AzureRM` toofind hello telepített verziója.</span><span class="sxs-lookup"><span data-stu-id="ee7ec-106">Run `Get-Module -ListAvailable AzureRM` toofind hello installed version.</span></span> <span data-ttu-id="ee7ec-107">Ha tooinstall vagy frissítés van szüksége, tekintse meg [telepítése Azure PowerShell modul](https://docs.microsoft.com/en-us/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="ee7ec-107">If you need tooinstall or upgrade, see [Install Azure PowerShell module](https://docs.microsoft.com/en-us/powershell/azure/install-azurerm-ps).</span></span>
 
<span data-ttu-id="ee7ec-108">Futtatás `Login-AzureRmAccount` toocreate Azure kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="ee7ec-108">Run `Login-AzureRmAccount` toocreate a connection with Azure.</span></span> 

## <a name="sample-script"></a><span data-ttu-id="ee7ec-109">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="ee7ec-109">Sample script</span></span>

```powershell
# prerequisites: 
# 1. Create an Azure Database from AdventureWorksLT sample database as hub database
# 2. Create an Azure Database in hello same region as sync database
# 3. Create an Azure Database as member database
# 4. Update hello parameters below before running hello sample
#
using namespace Microsoft.Azure.Commands.Sql.DataSync.Model
using namespace System.Collections.Generic

# Hub database info
# Subscription id for hub database
$SubscriptionId = "subscription_guid"
# Resrouce group name for hub database
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
# Temp file toosave hello sync schema
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

# Use this section if it is safe tooshow password in hello script.
# Otherwise, use hello PromptForCredential
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

# Use this section if it is safe tooshow password in hello script.
#$User = "username"
#$Password = ConvertTo-SecureString -String "password" -AsPlainText -Force
#$Credential = New-Object -TypeName "System.Management.Automation.PSCredential" -ArgumentList $User, $Password

$Credential = $Host.ui.PromptForCredential("Need credential", 
              "Please enter your user name and password for server "+$MemberServerName, 
              "", 
              "")

# Add a new sync member
Write-Host "Adding member"$SyncMemberName" toohello sync group"
New-AzureRmSqlSyncMember   -ResourceGroupName $ResourceGroupName `
                            -ServerName $ServerName `
                            -DatabaseName $DatabaseName `
                            -SyncGroupName $SyncGroupName `
                            -Name $SyncMemberName `
                            -MemberDatabaseCredential $Credential `
                            -MemberDatabaseName $MemberDatabaseName `
                            -MemberServerName ($MemberServerName + ".database.windows.net" `
                            -MemberDatabaseType $MemberDatabaseType `
                            -SyncDirection $SyncDirection

# Refresh database schema from hub database
# Specify hello -SyncMemberName parameter if you want toorefresh schema from hello member database
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
# Check hello log and see if refresh has gone through
Write-Host "Check for successful refresh"
$IsSucceeded = $false
While ($IsSucceeded -eq $false)
{
    Start-Sleep -s 10
    $timer=$timer+1
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



# Get hello database schema 
Write-Host "Adding tables and columns toohello sync schema"
$databaseSchema = Get-AzureRmSqlSyncSchema   -ResourceGroupName $ResourceGroupName `
                                             -ServerName $ServerName `
                                             -DatabaseName $DatabaseName `
                                             -SyncGroupName $SyncGroupName `

$databaseSchema | ConvertTo-Json -depth 5 -Compress | Out-File "c:\tmp\databaseSchema"     
$newSchema = [AzureSqlSyncGroupSchemaModel]::new()
$newSchema.Tables = [List[AzureSqlSyncGroupSchemaTableModel]]::new();

# Add columns and tables toohello sync schema
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
            Write-Host "Can't add table $fullTableName toohello sync schema" -foregroundcolor "Red"
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
                Write-Host "Can't add column $fullColumnName toohello sync schema" -foregroundcolor "Red"
                Write-Host $tableSchema.ErrorId -foregroundcolor "Red"c            }
            elseif ((-not $addAllColumns) -and $columnSchema.HasError)
            {
                Write-Host "Can't add column $fullColumnName toohello sync schema" -foregroundcolor "Red"
                Write-Host $columnSchema.ErrorId -foregroundcolor "Red"
            }
            else
            {
                Write-Host "Adding"$fullColumnName" toohello sync schema"
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

# Convert sync schema tooJson format
$schemaString = $newSchema | ConvertTo-Json -depth 5 -Compress

# workaround a powershell bug
$schemaString = $schemaString.Replace('"Tables"', '"tables"').Replace('"Columns"', '"columns"').Replace('"QuotedName"', '"quotedName"').Replace('"MasterSyncMemberName"','"masterSyncMemberName"')

# Save hello sync schema tooa temp file
$schemaString | Out-File $TempFile

# Update sync schema
Write-Host "Updating hello sync schema"
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

# Check hello sync log and wait until hello first sync succeeded
Write-Host "Check hello sync log"
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
    Write-Host "Enable hello scheduled sync with 300 seconds interval"
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
```

## <a name="clean-up-deployment"></a><span data-ttu-id="ee7ec-110">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="ee7ec-110">Clean up deployment</span></span>

<span data-ttu-id="ee7ec-111">Miután hello mintaparancsfájl, futtathatja a következő parancs tooremove hello erőforráscsoport hello, és a vele társított összes erőforrás.</span><span class="sxs-lookup"><span data-stu-id="ee7ec-111">After you run hello sample script, you can run hello following command tooremove hello resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="ee7ec-112">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="ee7ec-112">Script explanation</span></span>

<span data-ttu-id="ee7ec-113">A parancsfájl a következő parancsok hello.</span><span class="sxs-lookup"><span data-stu-id="ee7ec-113">This script uses hello following commands.</span></span> <span data-ttu-id="ee7ec-114">Minden egyes parancsa hello tábla hivatkozások toocommand vonatkozó dokumentációt.</span><span class="sxs-lookup"><span data-stu-id="ee7ec-114">Each command in hello table links toocommand-specific documentation.</span></span>

| <span data-ttu-id="ee7ec-115">Parancs</span><span class="sxs-lookup"><span data-stu-id="ee7ec-115">Command</span></span> | <span data-ttu-id="ee7ec-116">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="ee7ec-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="ee7ec-117">Új AzureRmSqlSyncAgent</span><span class="sxs-lookup"><span data-stu-id="ee7ec-117">New-AzureRmSqlSyncAgent</span></span>](/powershell/module/azurerm.sql/New-AzureRmSqlSyncAgent) |  <span data-ttu-id="ee7ec-118">Létrehoz egy új szinkronizálási ügynök</span><span class="sxs-lookup"><span data-stu-id="ee7ec-118">Creates a new Sync Agent</span></span> |
| [<span data-ttu-id="ee7ec-119">Új AzureRmSqlSyncAgentKey</span><span class="sxs-lookup"><span data-stu-id="ee7ec-119">New-AzureRmSqlSyncAgentKey</span></span>](/powershell/module/azurerm.sql/New-AzureRmSqlSyncAgentKey) |  <span data-ttu-id="ee7ec-120">Hello szinkronizálási ügynök társított hello ügynök kulcsot hoz létre</span><span class="sxs-lookup"><span data-stu-id="ee7ec-120">Generates hello agent key associated with hello Sync agent</span></span> |
| [<span data-ttu-id="ee7ec-121">Get-AzureRmSqlSyncAgentLinkedDatabase</span><span class="sxs-lookup"><span data-stu-id="ee7ec-121">Get-AzureRmSqlSyncAgentLinkedDatabase</span></span>](/powershell/module/azurerm.sql/Get-AzureRmSqlSyncAgentLinkedDatabase) |  <span data-ttu-id="ee7ec-122">A szinkronizálási ügynök hello összes hello adat beolvasása</span><span class="sxs-lookup"><span data-stu-id="ee7ec-122">Get all hello information for hello Sync Agent</span></span> |
| [<span data-ttu-id="ee7ec-123">Új AzureRmSqlSyncMember</span><span class="sxs-lookup"><span data-stu-id="ee7ec-123">New-AzureRmSqlSyncMember</span></span>](/powershell/module/azurerm.sql/New-AzureRmSqlSyncMember) |  <span data-ttu-id="ee7ec-124">Egy új tag toohello szinkronizálású csoport hozzáadása</span><span class="sxs-lookup"><span data-stu-id="ee7ec-124">Add a new member toohello Sync Group</span></span> |
| [<span data-ttu-id="ee7ec-125">Frissítés-AzureRmSqlSyncSchema</span><span class="sxs-lookup"><span data-stu-id="ee7ec-125">Update-AzureRmSqlSyncSchema</span></span>](/powershell/module/azurerm.sql/Update-AzureRmSqlSyncSchema) |  <span data-ttu-id="ee7ec-126">Hello adatbázis séma információinak frissítése</span><span class="sxs-lookup"><span data-stu-id="ee7ec-126">Refreshes hello database schema information</span></span> |
| [<span data-ttu-id="ee7ec-127">Get-AzureRmSqlSyncSchema</span><span class="sxs-lookup"><span data-stu-id="ee7ec-127">Get-AzureRmSqlSyncSchema</span></span>](/powershell/module/azurerm.sql/Get-AzureRmSqlSyncSchem) |  <span data-ttu-id="ee7ec-128">Lekérése hello adatbázis</span><span class="sxs-lookup"><span data-stu-id="ee7ec-128">Get hello database schema information</span></span> |
| [<span data-ttu-id="ee7ec-129">Frissítés-AzureRmSqlSyncGroup</span><span class="sxs-lookup"><span data-stu-id="ee7ec-129">Update-AzureRmSqlSyncGroup</span></span>](/powershell/module/azurerm.sql/Update-AzureRmSqlSyncGroup) |  <span data-ttu-id="ee7ec-130">Frissítések hello szinkronizálású csoport</span><span class="sxs-lookup"><span data-stu-id="ee7ec-130">Updates hello Sync Group</span></span> |
| [<span data-ttu-id="ee7ec-131">Start-AzureRmSqlSyncGroupSync</span><span class="sxs-lookup"><span data-stu-id="ee7ec-131">Start-AzureRmSqlSyncGroupSync</span></span>](/powershell/module/azurerm.sql/Start-AzureRmSqlSyncGroupSync) | <span data-ttu-id="ee7ec-132">Elindítja a szinkronizálási</span><span class="sxs-lookup"><span data-stu-id="ee7ec-132">Triggers a Sync</span></span> |
| [<span data-ttu-id="ee7ec-133">Get-AzureRmSqlSyncGroupLog</span><span class="sxs-lookup"><span data-stu-id="ee7ec-133">Get-AzureRmSqlSyncGroupLog</span></span>](/powershell/module/azurerm.sql/Get-AzureRmSqlSyncGroupLog) |  <span data-ttu-id="ee7ec-134">Ellenőrzi a szinkronizálási napló hello</span><span class="sxs-lookup"><span data-stu-id="ee7ec-134">Checks hello Sync Log</span></span> |
|||

## <a name="next-steps"></a><span data-ttu-id="ee7ec-135">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ee7ec-135">Next steps</span></span>

<span data-ttu-id="ee7ec-136">Azure PowerShell kapcsolatos további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="ee7ec-136">For more information about Azure PowerShell, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="ee7ec-137">További SQL Database PowerShell parancsfájl minták található [Azure SQL Database PowerShell-parancsfájlok](../sql-database-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="ee7ec-137">Additional SQL Database PowerShell script samples can be found in [Azure SQL Database PowerShell scripts](../sql-database-powershell-samples.md).</span></span>
