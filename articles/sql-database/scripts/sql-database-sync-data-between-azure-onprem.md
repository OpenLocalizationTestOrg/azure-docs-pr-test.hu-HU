---
title: "SQL Database és SQL Server közötti PowerShell példa-szinkronizálás helyszíni |} Microsoft Docs"
description: "Az Azure PowerShell-példa parancsfájl egy Azure SQL-adatbázis és a helyszíni SQL Server-adatbázisok közötti szinkronizálása"
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
ms.openlocfilehash: 0fe16f87bb258f0ff991661277f1b0a9e7e5875d
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="use-powershell-to-sync-between-an-azure-sql-database-and-a-sql-server-on-premises-database"></a><span data-ttu-id="bc884-103">Egy Azure SQL-adatbázis és a helyszíni SQL Server-adatbázisok közötti szinkronizálása a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="bc884-103">Use PowerShell to sync between an Azure SQL Database and a SQL Server on-premises database</span></span>

<span data-ttu-id="bc884-104">A PowerShell-példa konfigurálja az adatszinkronizálás szinkronizálása az Azure SQL Database és a helyszíni SQL Server-adatbázis között.</span><span class="sxs-lookup"><span data-stu-id="bc884-104">This PowerShell example configures Data Sync to sync between an Azure SQL Database and a SQL Server on-premises database.</span></span> 

<span data-ttu-id="bc884-105">Ez a minta az Azure PowerShell 4.2 vagy újabb verziója szükséges.</span><span class="sxs-lookup"><span data-stu-id="bc884-105">This sample requires the Azure PowerShell module version 4.2 or later.</span></span> <span data-ttu-id="bc884-106">Futtatás `Get-Module -ListAvailable AzureRM` telepített verziója található.</span><span class="sxs-lookup"><span data-stu-id="bc884-106">Run `Get-Module -ListAvailable AzureRM` to find the installed version.</span></span> <span data-ttu-id="bc884-107">Ha telepíteni vagy frissíteni szeretne, olvassa el [az Azure PowerShell-modul telepítését](https://docs.microsoft.com/en-us/powershell/azure/install-azurerm-ps) ismertető cikket.</span><span class="sxs-lookup"><span data-stu-id="bc884-107">If you need to install or upgrade, see [Install Azure PowerShell module](https://docs.microsoft.com/en-us/powershell/azure/install-azurerm-ps).</span></span>
 
<span data-ttu-id="bc884-108">Futtatás `Login-AzureRmAccount` kapcsolat létrehozása az Azure-ral.</span><span class="sxs-lookup"><span data-stu-id="bc884-108">Run `Login-AzureRmAccount` to create a connection with Azure.</span></span> 

## <a name="sample-script"></a><span data-ttu-id="bc884-109">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="bc884-109">Sample script</span></span>

```powershell
# prerequisites: 
# 1. Create an Azure Database from AdventureWorksLT sample database as hub database
# 2. Create an Azure Database in the same region as sync database
# 3. Create an on premises SQL Server Database as member database
# 4. Update the parameters below before running the sample
#
using namespace Microsoft.Azure.Commands.Sql.DataSync.Model
using namespace System.Collections.Generic

# Hub database info
# Subscription id for hub database
$SubscriptionId = "subscription_guid"
# Resrouce group name for hub database
$ResourceGroupName = "ResourceGroupName"
# Server name for hub database
$ServerName = "ServerName"
# Database name for hub database
$DatabaseName = "TestHubDatabase"

# Sync database info
# Resource group name for sync database
$SyncDatabaseResourceGroupName = "ResourceGroupName"
# Server name for sync database
$SyncDatabaseServerName = "ServerName"
# Sync database name
$SyncDatabaseName = "SyncDatabaseName"

# Sync group info
# Sync group name
$SyncGroupName = "TestSyncGroup"
# Conflict resolution Policy. Value can be HubWin or MemberWin
$ConflictResolutionPolicy = "HubWin"
# Sync interval in seconds. Value must be no less than 300
$IntervalInSeconds = 300

# Member database info
# Member name
$SyncMemberName = "member"
# Member server name
$MemberServerName = "OnPremiseServer"
# Member database name
$MemberDatabaseName = "MemberDatabaseTest"
# Member database type. Value can be AzureSqlDatabase or SqlServerDatabase
$MemberDatabaseType = "SqlServerDatabase"
# Sync direction. Value can be Bidirectional, Onewaymembertohub, Onewayhubtomember
$SyncDirection = "Bidirectional"

#Sync Agent Info
$SyncAgentName = "TestSyncAgent"
$SyncAgentResourceGroupName = "ResourceGroupName"
$SyncAgentServerName = "ServerName"

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
# $PWord = ConvertTo-SecureString -String "password" -AsPlainText -Force
# $Credential = New-Object -TypeName "System.Management.Automation.PSCredential" -ArgumentList $User, $PWord

$Credential = $Host.ui.PromptForCredential("Need credential", 
              "Please enter your user name and password for server "+$ServerName+".database.windows.net", 
              "", 
              "")

 #Create a new Sync Agent
 Write-Host "Creating new Sync Agent "
 New-AzureRmSqlSyncAgent -ResourceGroupName $ResourceGroupName  `
                               -ServerName  $ServerName  `
                               -SyncDatabaseName $SyncDatabaseName `
                              -SyncAgentName $SyncAgentName


#Generate Agent Key
Write-Host "Generating Agent Key"
$AgentKey = New-AzureRmSqlSyncAgentKey -ResourceGroupName $ResourceGroupName   `
                              -ServerName  $ServerName  `
                              -SyncAgentName $SyncAgentName
 Write-Host "Use your agent key to configure the sync agent. Do this before proceeding"
$agentkey
                  
#DO THE FOLLOWING BEFORE THE NEXT STEP
#Install the on-premises sync agent on your machine and register the sync agent using the agent key generated above to bring the sync agent online.
#Add the SQL server database information including server name, database name, user name, password on the configuration tool within the sync agent.  


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

#Get Information from sync Agent 
#Confirm that your SQL Server was configured
#Note the Database ID, you will use this as SqlServerDatabaseID for the next step.
$SyncAgentInfo = Get-AzureRmSqlSyncAgentLinkedDatabase -ResourceGroupName $ResourceGroupName   `
                              -ServerName  $ServerName  `
                              -SyncAgentName $SyncAgentName

# Add a new sync member
Write-Host "Adding member"$SyncMemberName" to the sync group"



New-AzureRmSqlSyncMember -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName -SyncGroupName $SyncGroupName -Name $SyncMemberName -MemberDatabaseType $MemberDatabaseType -SyncAgentResourceGroupName SyncAgentResourceGroupName -SyncAgentServerName $SyncAgentServerName -SyncAgentName $SyncAgentName  -SyncDirection $SyncDirection -SqlServerDatabaseID  $SyncAgentInfo.DatabaseId

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
$IsSucceeded = "false"
While ($IsSucceeded -eq "False")
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


# Get the database schema 
Write-Host "Adding tables and columns to the sync schema"
$databaseSchema = Get-AzureRmSqlSyncSchema   -ResourceGroupName $ResourceGroupName `
                                             -ServerName $ServerName `
                                             -DatabaseName $DatabaseName `
                                             -SyncGroupName $SyncGroupName `

$databaseSchema | ConvertTo-Json -depth 5 -Compress | Out-File "C:\Users\OnPremiseServer\AppData\Local\Temp\syncSchema.json"     
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
                Write-Host $tableSchema.ErrorId -foregroundcolor "Red"
            }
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

$SyncLogStartTime = Get-Date

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
```

## <a name="clean-up-deployment"></a><span data-ttu-id="bc884-110">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="bc884-110">Clean up deployment</span></span>

<span data-ttu-id="bc884-111">A minta-parancsprogram futtatása után futtathatja a következő parancs futtatásával távolítsa el az erőforráscsoportot és a vele társított összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="bc884-111">After you run the sample script, you can run the following command to remove the resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="bc884-112">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="bc884-112">Script explanation</span></span>

<span data-ttu-id="bc884-113">A parancsfájl a következő parancsokat.</span><span class="sxs-lookup"><span data-stu-id="bc884-113">This script uses the following commands.</span></span> <span data-ttu-id="bc884-114">Minden egyes parancsa a tábla-parancs-specifikus dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="bc884-114">Each command in the table links to command-specific documentation.</span></span>

| <span data-ttu-id="bc884-115">Parancs</span><span class="sxs-lookup"><span data-stu-id="bc884-115">Command</span></span> | <span data-ttu-id="bc884-116">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="bc884-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="bc884-117">Új AzureRmSqlSyncAgent</span><span class="sxs-lookup"><span data-stu-id="bc884-117">New-AzureRmSqlSyncAgent</span></span>](/powershell/module/azurerm.sql/New-AzureRmSqlSyncAgent) |  <span data-ttu-id="bc884-118">Létrehoz egy új szinkronizálási ügynök</span><span class="sxs-lookup"><span data-stu-id="bc884-118">Creates a new Sync Agent</span></span> |
| [<span data-ttu-id="bc884-119">Új AzureRmSqlSyncAgentKey</span><span class="sxs-lookup"><span data-stu-id="bc884-119">New-AzureRmSqlSyncAgentKey</span></span>](/powershell/module/azurerm.sql/New-AzureRmSqlSyncAgentKey) |  <span data-ttu-id="bc884-120">A szinkronizálási ügynök tartozó ügynök kulcsot hoz létre</span><span class="sxs-lookup"><span data-stu-id="bc884-120">Generates the agent key associated with the Sync agent</span></span> |
| [<span data-ttu-id="bc884-121">Get-AzureRmSqlSyncAgentLinkedDatabase</span><span class="sxs-lookup"><span data-stu-id="bc884-121">Get-AzureRmSqlSyncAgentLinkedDatabase</span></span>](/powershell/module/azurerm.sql/Get-AzureRmSqlSyncAgentLinkedDatabase) |  <span data-ttu-id="bc884-122">A Sync-ügynök az összes adat beolvasása</span><span class="sxs-lookup"><span data-stu-id="bc884-122">Get all the information for the Sync Agent</span></span> |
| [<span data-ttu-id="bc884-123">Új AzureRmSqlSyncMember</span><span class="sxs-lookup"><span data-stu-id="bc884-123">New-AzureRmSqlSyncMember</span></span>](/powershell/module/azurerm.sql/New-AzureRmSqlSyncMember) |  <span data-ttu-id="bc884-124">Új tag hozzáadása a szinkronizálási csoport</span><span class="sxs-lookup"><span data-stu-id="bc884-124">Add a new member to the Sync Group</span></span> |
| [<span data-ttu-id="bc884-125">Frissítés-AzureRmSqlSyncSchema</span><span class="sxs-lookup"><span data-stu-id="bc884-125">Update-AzureRmSqlSyncSchema</span></span>](/powershell/module/azurerm.sql/Update-AzureRmSqlSyncSchema) |  <span data-ttu-id="bc884-126">Az adatbázis-séma információinak frissítése</span><span class="sxs-lookup"><span data-stu-id="bc884-126">Refreshes the database schema information</span></span> |
| [<span data-ttu-id="bc884-127">Get-AzureRmSqlSyncSchema</span><span class="sxs-lookup"><span data-stu-id="bc884-127">Get-AzureRmSqlSyncSchema</span></span>](/powershell/module/azurerm.sql/Get-AzureRmSqlSyncSchem) |  <span data-ttu-id="bc884-128">Az adatbázis lekérése</span><span class="sxs-lookup"><span data-stu-id="bc884-128">Get the database schema information</span></span> |
| [<span data-ttu-id="bc884-129">Frissítés-AzureRmSqlSyncGroup</span><span class="sxs-lookup"><span data-stu-id="bc884-129">Update-AzureRmSqlSyncGroup</span></span>](/powershell/module/azurerm.sql/Update-AzureRmSqlSyncGroup) |  <span data-ttu-id="bc884-130">A szinkronizálás csoport frissítése</span><span class="sxs-lookup"><span data-stu-id="bc884-130">Updates the Sync Group</span></span> |
| [<span data-ttu-id="bc884-131">Start-AzureRmSqlSyncGroupSync</span><span class="sxs-lookup"><span data-stu-id="bc884-131">Start-AzureRmSqlSyncGroupSync</span></span>](/powershell/module/azurerm.sql/Start-AzureRmSqlSyncGroupSync) | <span data-ttu-id="bc884-132">Elindítja a szinkronizálási</span><span class="sxs-lookup"><span data-stu-id="bc884-132">Triggers a Sync</span></span> |
| [<span data-ttu-id="bc884-133">Get-AzureRmSqlSyncGroupLog</span><span class="sxs-lookup"><span data-stu-id="bc884-133">Get-AzureRmSqlSyncGroupLog</span></span>](/powershell/module/azurerm.sql/Get-AzureRmSqlSyncGroupLog) |  <span data-ttu-id="bc884-134">Ellenőrzi a szinkronizálási napló</span><span class="sxs-lookup"><span data-stu-id="bc884-134">Checks the Sync Log</span></span> |
|||

## <a name="next-steps"></a><span data-ttu-id="bc884-135">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bc884-135">Next steps</span></span>

<span data-ttu-id="bc884-136">Azure PowerShell kapcsolatos további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="bc884-136">For more information about Azure PowerShell, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="bc884-137">További SQL Database PowerShell parancsfájl minták található [Azure SQL Database PowerShell-parancsfájlok](../sql-database-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="bc884-137">Additional SQL Database PowerShell script samples can be found in [Azure SQL Database PowerShell scripts](../sql-database-powershell-samples.md).</span></span>
