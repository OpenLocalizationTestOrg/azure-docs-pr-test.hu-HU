---
title: "PowerShell segítségével hozza létre és konfigurálja a Naplóelemzési munkaterület |} Microsoft Docs"
description: "A helyszíni kiszolgálók elemzés által használt adatok bejelentkezhet, illetve a felhőalapú infrastruktúra. Gép adatgyűjtést az Azure storage Azure diagnostics generálásakor."
services: log-analytics
documentationcenter: 
author: richrundmsft
manager: jochan
editor: 
ms.assetid: 3b9b7ade-3374-4596-afb1-51b695f481c2
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: powershell
ms.topic: article
ms.date: 11/21/2016
ms.author: richrund
ms.openlocfilehash: 6807ab67e3593da82c147669b29bfdae3b6c967c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="manage-log-analytics-using-powershell"></a><span data-ttu-id="1c865-104">A Log Analytics felügyelete PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="1c865-104">Manage Log Analytics using PowerShell</span></span>
<span data-ttu-id="1c865-105">Használhatja a [napló Analytics PowerShell-parancsmagok](https://msdn.microsoft.com/library/mt188224\(v=azure.300\).aspx) végzik el a különböző funkciókat a Naplóelemzési parancssori vagy parancsfájl részeként.</span><span class="sxs-lookup"><span data-stu-id="1c865-105">You can use the [Log Analytics PowerShell cmdlets](https://msdn.microsoft.com/library/mt188224\(v=azure.300\).aspx) to perform various functions in Log Analytics from a command line or as part of a script.</span></span>  <span data-ttu-id="1c865-106">A PowerShell használatával végezheti el a feladatok közé:</span><span class="sxs-lookup"><span data-stu-id="1c865-106">Examples of the tasks you can perform with PowerShell include:</span></span>

* <span data-ttu-id="1c865-107">Munkaterületek létrehozása</span><span class="sxs-lookup"><span data-stu-id="1c865-107">Create a workspace</span></span>
* <span data-ttu-id="1c865-108">Hozzáadni vagy eltávolítani egy megoldást</span><span class="sxs-lookup"><span data-stu-id="1c865-108">Add or remove a solution</span></span>
* <span data-ttu-id="1c865-109">Importálás és exportálás a mentett keresések</span><span class="sxs-lookup"><span data-stu-id="1c865-109">Import and export saved searches</span></span>
* <span data-ttu-id="1c865-110">Hozzon létre egy számítógépcsoportot</span><span class="sxs-lookup"><span data-stu-id="1c865-110">Create a computer group</span></span>
* <span data-ttu-id="1c865-111">A Windows-ügynök telepítve az IIS-naplók számítógépekről gyűjtésének engedélyezése</span><span class="sxs-lookup"><span data-stu-id="1c865-111">Enable collection of IIS logs from computers with the Windows agent installed</span></span>
* <span data-ttu-id="1c865-112">A Linux és Windows számítógépekről a teljesítményszámlálók adatainak összegyűjtése</span><span class="sxs-lookup"><span data-stu-id="1c865-112">Collect performance counters from Linux and Windows computers</span></span>
* <span data-ttu-id="1c865-113">A Linux rendszerű számítógépeken syslog események gyűjtése</span><span class="sxs-lookup"><span data-stu-id="1c865-113">Collect events from syslog on Linux computers</span></span> 
* <span data-ttu-id="1c865-114">A Windows Eseménynapló eseményeinek gyűjtése</span><span class="sxs-lookup"><span data-stu-id="1c865-114">Collect events from Windows event logs</span></span>
* <span data-ttu-id="1c865-115">Egyéni eseménynaplók gyűjtése</span><span class="sxs-lookup"><span data-stu-id="1c865-115">Collect custom event logs</span></span>
* <span data-ttu-id="1c865-116">A log analytics agent hozzáadása egy Azure virtuális gépen</span><span class="sxs-lookup"><span data-stu-id="1c865-116">Add the log analytics agent to an Azure virtual machine</span></span>
* <span data-ttu-id="1c865-117">Naplóelemzési Azure diagnostics használatával gyűjt index adatok konfigurálása</span><span class="sxs-lookup"><span data-stu-id="1c865-117">Configure log analytics to index data collected using Azure diagnostics</span></span>

<span data-ttu-id="1c865-118">Ez a cikk ismerteti, amelyek bemutatják a powershellből a végrehajtható függvények két mintakódok.</span><span class="sxs-lookup"><span data-stu-id="1c865-118">This article provides two code samples that illustrate some of the functions that you can perform from PowerShell.</span></span>  <span data-ttu-id="1c865-119">Olvassa el a [napló Analytics PowerShell parancsmag-referencia](https://msdn.microsoft.com/library/mt188224\(v=azure.300\).aspx) az egyéb funkciót.</span><span class="sxs-lookup"><span data-stu-id="1c865-119">You can refer to the [Log Analytics PowerShell cmdlet reference](https://msdn.microsoft.com/library/mt188224\(v=azure.300\).aspx) for other functions.</span></span>

> [!NOTE]
> <span data-ttu-id="1c865-120">Naplóelemzési korábban meghívták az Operational Insights, ezért a parancsmagokban használt a név.</span><span class="sxs-lookup"><span data-stu-id="1c865-120">Log Analytics was previously called Operational Insights, which is why it is the name used in the cmdlets.</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="1c865-121">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="1c865-121">Prerequisites</span></span>
<span data-ttu-id="1c865-122">Ezek a példák 2.3.0 verziójával vagy későbbi a AzureRm.OperationalInsights modul működik.</span><span class="sxs-lookup"><span data-stu-id="1c865-122">These examples work with version 2.3.0 or later of the AzureRm.OperationalInsights module.</span></span>


## <a name="create-and-configure-a-log-analytics-workspace"></a><span data-ttu-id="1c865-123">Hozza létre és konfigurálja a Naplóelemzési munkaterület</span><span class="sxs-lookup"><span data-stu-id="1c865-123">Create and configure a Log Analytics Workspace</span></span>
<span data-ttu-id="1c865-124">Az alábbi parancsfájl minta bemutatja, hogyan:</span><span class="sxs-lookup"><span data-stu-id="1c865-124">The following script sample illustrates how to:</span></span>

1. <span data-ttu-id="1c865-125">Munkaterületek létrehozása</span><span class="sxs-lookup"><span data-stu-id="1c865-125">Create a workspace</span></span>
2. <span data-ttu-id="1c865-126">A rendelkezésre álló megoldások felsorolása</span><span class="sxs-lookup"><span data-stu-id="1c865-126">List the available solutions</span></span>
3. <span data-ttu-id="1c865-127">A munkaterület megoldások hozzáadása</span><span class="sxs-lookup"><span data-stu-id="1c865-127">Add solutions to the workspace</span></span>
4. <span data-ttu-id="1c865-128">Importálás mentett keresések</span><span class="sxs-lookup"><span data-stu-id="1c865-128">Import saved searches</span></span>
5. <span data-ttu-id="1c865-129">Exportálás mentett keresések</span><span class="sxs-lookup"><span data-stu-id="1c865-129">Export saved searches</span></span>
6. <span data-ttu-id="1c865-130">Hozzon létre egy számítógépcsoportot</span><span class="sxs-lookup"><span data-stu-id="1c865-130">Create a computer group</span></span>
7. <span data-ttu-id="1c865-131">A Windows-ügynök telepítve az IIS-naplók számítógépekről gyűjtésének engedélyezése</span><span class="sxs-lookup"><span data-stu-id="1c865-131">Enable collection of IIS logs from computers with the Windows agent installed</span></span>
8. <span data-ttu-id="1c865-132">Logikai lemez teljesítményszámlálói gyűjteni a Linux rendszerű számítógépek (% Inode-OK; Szabad hely MB-ban; Foglalt hely; % / Mp átvitelek; Lemezolvasások/mp; Lemezírás/mp)</span><span class="sxs-lookup"><span data-stu-id="1c865-132">Collect Logical Disk perf counters from Linux computers (% Used Inodes; Free Megabytes; % Used Space; Disk Transfers/sec; Disk Reads/sec; Disk Writes/sec)</span></span>
9. <span data-ttu-id="1c865-133">Syslog-események gyűjtése Linux számítógépekről</span><span class="sxs-lookup"><span data-stu-id="1c865-133">Collect syslog events from Linux computers</span></span>
10. <span data-ttu-id="1c865-134">Hiba és figyelmeztetési események gyűjtése az alkalmazások eseménynaplójában a Windows rendszerű számítógépek</span><span class="sxs-lookup"><span data-stu-id="1c865-134">Collect Error and Warning events from the Application Event Log from Windows computers</span></span>
11. <span data-ttu-id="1c865-135">A Windows rendszerű számítógépek memória rendelkezésre álló memória (MB) teljesítményszámláló gyűjtése.</span><span class="sxs-lookup"><span data-stu-id="1c865-135">Collect Memory Available Mbytes performance counter from Windows computers</span></span>
12. <span data-ttu-id="1c865-136">Egy egyéni napló gyűjtése</span><span class="sxs-lookup"><span data-stu-id="1c865-136">Collect a custom log</span></span> 

```

$ResourceGroup = "oms-example"
$WorkspaceName = "log-analytics-" + (Get-Random -Maximum 99999) # workspace names need to be unique - Get-Random helps with this for the example code
$Location = "westeurope"

# List of solutions to enable
$Solutions = "Security", "Updates", "SQLAssessment"

# Saved Searches to import
$ExportedSearches = @"
[
    {
        "Category":  "My Saved Searches",
        "DisplayName":  "WAD Events (All)",
        "Query":  "Type=Event SourceSystem:AzureStorage ",
        "Version":  1
    },
    {        
        "Category":  "My Saved Searches",
        "DisplayName":  "Current Disk Queue Length",
        "Query":  "Type=Perf ObjectName=LogicalDisk InstanceName=\"C:\" CounterName=\"Current Disk Queue Length\"",
        "Version":  1
    }
]
"@ | ConvertFrom-Json

# Custom Log to collect
$CustomLog = @"
{
    "customLogName": "sampleCustomLog1", 
    "description": "Example custom log datasource", 
    "inputs": [
        { 
            "location": { 
            "fileSystemLocations": { 
                "windowsFileTypeLogPaths": [ "e:\\iis5\\*.log" ], 
                "linuxFileTypeLogPaths": [ "/var/logs" ] 
                } 
            }, 
        "recordDelimiter": { 
            "regexDelimiter": { 
                "pattern": "\\n", 
                "matchIndex": 0, 
                "matchIndexSpecified": true, 
                "numberedGroup": null 
                } 
            } 
        }
    ], 
    "extractions": [
        { 
            "extractionName": "TimeGenerated", 
            "extractionType": "DateTime", 
            "extractionProperties": { 
                "dateTimeExtraction": { 
                    "regex": null, 
                    "joinStringRegex": null 
                    } 
                } 
            }
        ] 
    }
"@

# Create the resource group if needed
try {
    Get-AzureRmResourceGroup -Name $ResourceGroup -ErrorAction Stop
} catch {
    New-AzureRmResourceGroup -Name $ResourceGroup -Location $Location
}

# Create the workspace
New-AzureRmOperationalInsightsWorkspace -Location $Location -Name $WorkspaceName -Sku Standard -ResourceGroupName $ResourceGroup

# List all solutions and their installation status
Get-AzureRmOperationalInsightsIntelligencePacks -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Add solutions
foreach ($solution in $Solutions) {
    Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -IntelligencePackName $solution -Enabled $true
}

#List enabled solutions
(Get-AzureRmOperationalInsightsIntelligencePacks -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName).Where({($_.enabled -eq $true)})

# Import Saved Searches
foreach ($search in $ExportedSearches) {
    $id = $search.Category + "|" + $search.DisplayName
    New-AzureRmOperationalInsightsSavedSearch -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -SavedSearchId $id -DisplayName $search.DisplayName -Category $search.Category -Query $search.Query -Version $search.Version
}

# Export Saved Searches
(Get-AzureRmOperationalInsightsSavedSearch -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName).Value.Properties | ConvertTo-Json 

# Create Computer Group based on a query
New-AzureRmOperationalInsightsComputerGroup -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -SavedSearchId "My Web Servers" -DisplayName "Web Servers" -Category "My Saved Searches" -Query "Computer=""web*"" | distinct Computer" -Version 1

# Create a computer group based on names (up to 5000)
$computerGroup = """servername1.contoso.com"",""servername2.contoso.com"",""servername3.contoso.com"",""servername4.contoso.com"""
New-AzureRmOperationalInsightsComputerGroup -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -SavedSearchId "My Named Servers" -DisplayName "Named Servers" -Category "My Saved Searches" -Query $computerGroup -Version 1

# Enable IIS Log Collection using agent
Enable-AzureRmOperationalInsightsIISLogCollection -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Linux Perf
New-AzureRmOperationalInsightsLinuxPerformanceObjectDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -ObjectName "Logical Disk" -InstanceName "*"  -CounterNames @("% Used Inodes", "Free Megabytes", "% Used Space", "Disk Transfers/sec", "Disk Reads/sec", "Disk Reads/sec", "Disk Writes/sec") -IntervalSeconds 20  -Name "Example Linux Disk Performance Counters"
Enable-AzureRmOperationalInsightsLinuxCustomLogCollection -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Linux Syslog
New-AzureRmOperationalInsightsLinuxSyslogDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -Facility "kern" -CollectEmergency -CollectAlert -CollectCritical -CollectError -CollectWarning -Name "Example kernal syslog collection"
Enable-AzureRmOperationalInsightsLinuxSyslogCollection -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Windows Event
New-AzureRmOperationalInsightsWindowsEventDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -EventLogName "Application" -CollectErrors -CollectWarnings -Name "Example Application Event Log"

# Windows Perf
New-AzureRmOperationalInsightsWindowsPerformanceCounterDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -ObjectName "Memory" -InstanceName "*" -CounterName "Available MBytes" -IntervalSeconds 20 -Name "Example Windows Performance Counter"

# Custom Logs
New-AzureRmOperationalInsightsCustomLogDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -CustomLogRawJson "$CustomLog" -Name "Example Custom Log Collection"

```

## <a name="configuring-log-analytics-to-index-azure-diagnostics"></a><span data-ttu-id="1c865-137">Az Azure diagnostics indexelésre Naplóelemzési konfigurálása</span><span class="sxs-lookup"><span data-stu-id="1c865-137">Configuring Log Analytics to index Azure diagnostics</span></span>
<span data-ttu-id="1c865-138">Az ügynök nélküli figyelés az Azure-erőforrások, az erőforrások kell rendelkeznie az Azure diagnostics engedélyezni és konfigurálni a Naplóelemzési munkaterület írni.</span><span class="sxs-lookup"><span data-stu-id="1c865-138">For agentless monitoring of Azure resources, the resources need to have Azure diagnostics enabled and configured to write to a Log Analytics workspace.</span></span> <span data-ttu-id="1c865-139">Ez a megközelítés adatokat közvetlenül küld a Naplóelemzési, és nem szükséges adatok tárfiókba kellene írni.</span><span class="sxs-lookup"><span data-stu-id="1c865-139">This approach sends data directly to Log Analytics and does not require data to be written to a storage account.</span></span> <span data-ttu-id="1c865-140">Támogatott erőforrások többek között:</span><span class="sxs-lookup"><span data-stu-id="1c865-140">Supported resources include:</span></span>

| <span data-ttu-id="1c865-141">Erőforrás típusa</span><span class="sxs-lookup"><span data-stu-id="1c865-141">Resource Type</span></span> | <span data-ttu-id="1c865-142">Logs</span><span class="sxs-lookup"><span data-stu-id="1c865-142">Logs</span></span> | <span data-ttu-id="1c865-143">Mérőszámok</span><span class="sxs-lookup"><span data-stu-id="1c865-143">Metrics</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1c865-144">Application Gateway átjárók</span><span class="sxs-lookup"><span data-stu-id="1c865-144">Application Gateways</span></span>    | <span data-ttu-id="1c865-145">Igen</span><span class="sxs-lookup"><span data-stu-id="1c865-145">Yes</span></span> | <span data-ttu-id="1c865-146">Igen</span><span class="sxs-lookup"><span data-stu-id="1c865-146">Yes</span></span> |
| <span data-ttu-id="1c865-147">Automation-fiókok</span><span class="sxs-lookup"><span data-stu-id="1c865-147">Automation accounts</span></span>     | <span data-ttu-id="1c865-148">Igen</span><span class="sxs-lookup"><span data-stu-id="1c865-148">Yes</span></span> | |
| <span data-ttu-id="1c865-149">Batch-fiókok</span><span class="sxs-lookup"><span data-stu-id="1c865-149">Batch accounts</span></span>          | <span data-ttu-id="1c865-150">Igen</span><span class="sxs-lookup"><span data-stu-id="1c865-150">Yes</span></span> | <span data-ttu-id="1c865-151">Igen</span><span class="sxs-lookup"><span data-stu-id="1c865-151">Yes</span></span> |
| <span data-ttu-id="1c865-152">A Data Lake analytics</span><span class="sxs-lookup"><span data-stu-id="1c865-152">Data Lake analytics</span></span>     | <span data-ttu-id="1c865-153">Igen</span><span class="sxs-lookup"><span data-stu-id="1c865-153">Yes</span></span> | | 
| <span data-ttu-id="1c865-154">A Data Lake store</span><span class="sxs-lookup"><span data-stu-id="1c865-154">Data Lake store</span></span>         | <span data-ttu-id="1c865-155">Igen</span><span class="sxs-lookup"><span data-stu-id="1c865-155">Yes</span></span> | |
| <span data-ttu-id="1c865-156">A rugalmas SQL-készlet</span><span class="sxs-lookup"><span data-stu-id="1c865-156">Elastic SQL Pool</span></span>        |     | <span data-ttu-id="1c865-157">Igen</span><span class="sxs-lookup"><span data-stu-id="1c865-157">Yes</span></span> |
| <span data-ttu-id="1c865-158">Event Hub névtér</span><span class="sxs-lookup"><span data-stu-id="1c865-158">Event Hub namespace</span></span>     |     | <span data-ttu-id="1c865-159">Igen</span><span class="sxs-lookup"><span data-stu-id="1c865-159">Yes</span></span> |
| <span data-ttu-id="1c865-160">IoT-központok</span><span class="sxs-lookup"><span data-stu-id="1c865-160">IoT Hubs</span></span>                |     | <span data-ttu-id="1c865-161">Igen</span><span class="sxs-lookup"><span data-stu-id="1c865-161">Yes</span></span> |
| <span data-ttu-id="1c865-162">Key Vault</span><span class="sxs-lookup"><span data-stu-id="1c865-162">Key Vault</span></span>               | <span data-ttu-id="1c865-163">Igen</span><span class="sxs-lookup"><span data-stu-id="1c865-163">Yes</span></span> | |
| <span data-ttu-id="1c865-164">Terheléselosztók</span><span class="sxs-lookup"><span data-stu-id="1c865-164">Load Balancers</span></span>          | <span data-ttu-id="1c865-165">Igen</span><span class="sxs-lookup"><span data-stu-id="1c865-165">Yes</span></span> | |
| <span data-ttu-id="1c865-166">Logic Apps</span><span class="sxs-lookup"><span data-stu-id="1c865-166">Logic Apps</span></span>              | <span data-ttu-id="1c865-167">Igen</span><span class="sxs-lookup"><span data-stu-id="1c865-167">Yes</span></span> | <span data-ttu-id="1c865-168">Igen</span><span class="sxs-lookup"><span data-stu-id="1c865-168">Yes</span></span> |
| <span data-ttu-id="1c865-169">Network Security Groups (Hálózati biztonsági csoportok)</span><span class="sxs-lookup"><span data-stu-id="1c865-169">Network Security Groups</span></span> | <span data-ttu-id="1c865-170">Igen</span><span class="sxs-lookup"><span data-stu-id="1c865-170">Yes</span></span> | |
| <span data-ttu-id="1c865-171">Redis Cache</span><span class="sxs-lookup"><span data-stu-id="1c865-171">Redis Cache</span></span>             |     | <span data-ttu-id="1c865-172">Igen</span><span class="sxs-lookup"><span data-stu-id="1c865-172">Yes</span></span> |
| <span data-ttu-id="1c865-173">Szolgáltatások keresése</span><span class="sxs-lookup"><span data-stu-id="1c865-173">Search services</span></span>         | <span data-ttu-id="1c865-174">Igen</span><span class="sxs-lookup"><span data-stu-id="1c865-174">Yes</span></span> | <span data-ttu-id="1c865-175">Igen</span><span class="sxs-lookup"><span data-stu-id="1c865-175">Yes</span></span> |
| <span data-ttu-id="1c865-176">Service Bus-névtér</span><span class="sxs-lookup"><span data-stu-id="1c865-176">Service Bus namespace</span></span>   |     | <span data-ttu-id="1c865-177">Igen</span><span class="sxs-lookup"><span data-stu-id="1c865-177">Yes</span></span> |
| <span data-ttu-id="1c865-178">SQL (12-es verzió)</span><span class="sxs-lookup"><span data-stu-id="1c865-178">SQL (v12)</span></span>               |     | <span data-ttu-id="1c865-179">Igen</span><span class="sxs-lookup"><span data-stu-id="1c865-179">Yes</span></span> |
| <span data-ttu-id="1c865-180">Webhelyek</span><span class="sxs-lookup"><span data-stu-id="1c865-180">Web Sites</span></span>               |     | <span data-ttu-id="1c865-181">Igen</span><span class="sxs-lookup"><span data-stu-id="1c865-181">Yes</span></span> |
| <span data-ttu-id="1c865-182">Kiszolgáló-webfarmok</span><span class="sxs-lookup"><span data-stu-id="1c865-182">Web Server farms</span></span>        |     | <span data-ttu-id="1c865-183">Igen</span><span class="sxs-lookup"><span data-stu-id="1c865-183">Yes</span></span> |

<span data-ttu-id="1c865-184">Az elérhető mérőszámok leírását, [támogatott Azure-figyelő metrikák](../monitoring-and-diagnostics/monitoring-supported-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="1c865-184">For the details of the available metrics, refer to [supported metrics with Azure Monitor](../monitoring-and-diagnostics/monitoring-supported-metrics.md).</span></span>

<span data-ttu-id="1c865-185">A naplók leírását, [szolgáltatások és a séma támogatja a diagnosztikai naplók](../monitoring-and-diagnostics/monitoring-diagnostic-logs-schema.md).</span><span class="sxs-lookup"><span data-stu-id="1c865-185">For the details of the available logs, refer to [supported services and schema for diagnostic logs](../monitoring-and-diagnostics/monitoring-diagnostic-logs-schema.md).</span></span>

```
$workspaceId = "/subscriptions/d2e37fee-1234-40b2-5678-0b2199de3b50/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$resourceId = "/SUBSCRIPTIONS/ec11ca60-1234-491e-5678-0ea07feae25c/RESOURCEGROUPS/DEMO/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/DEMO" 

Set-AzureRmDiagnosticSetting -ResourceId $resourceId -WorkspaceId $workspaceId -Enabled $true
```

<span data-ttu-id="1c865-186">A fenti parancsmag segítségével is naplóinak gyűjtése az erőforrásokat, amelyek különböző előfizetésekhez.</span><span class="sxs-lookup"><span data-stu-id="1c865-186">You can also use the preceding cmdlet to collect logs from resources that are in different subscriptions.</span></span> <span data-ttu-id="1c865-187">A parancsmag annak tud dolgozni előfizetések között, mert meg van adva mindkét az erőforrás létrehozása a naplókat, valamint a naplókat küld a munkaterület azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="1c865-187">The cmdlet is able to work across subscriptions since you are providing the id of both the resource creating logs and the workspace the logs are sent to.</span></span>


## <a name="configuring-log-analytics-to-index-azure-diagnostics-from-storage"></a><span data-ttu-id="1c865-188">Naplóelemzési tárolóból az Azure diagnostics indexelésre konfigurálása</span><span class="sxs-lookup"><span data-stu-id="1c865-188">Configuring Log Analytics to index Azure diagnostics from storage</span></span>
<span data-ttu-id="1c865-189">Napló amikből adatgyűjtést belül egy klasszikus felhőalapú szolgáltatás, vagy a service fabric-fürt futó példányát, először az adatok írása az Azure storage kell.</span><span class="sxs-lookup"><span data-stu-id="1c865-189">To collect log data from within a running instance of a classic cloud service or a service fabric cluster, you need to first write the data to Azure storage.</span></span> <span data-ttu-id="1c865-190">A Naplóelemzési majd gyűjteni a tárfiók van beállítva.</span><span class="sxs-lookup"><span data-stu-id="1c865-190">Log Analytics is then configured to collect the logs from the storage account.</span></span> <span data-ttu-id="1c865-191">Támogatott erőforrások többek között:</span><span class="sxs-lookup"><span data-stu-id="1c865-191">Supported resources include:</span></span>

* <span data-ttu-id="1c865-192">Klasszikus felhőszolgáltatások (webes és feldolgozói szerepkörök)</span><span class="sxs-lookup"><span data-stu-id="1c865-192">Classic cloud services (web and worker roles)</span></span>
* <span data-ttu-id="1c865-193">Service fabric-fürtök</span><span class="sxs-lookup"><span data-stu-id="1c865-193">Service fabric clusters</span></span>

<span data-ttu-id="1c865-194">Az alábbi példában látható hogyan:</span><span class="sxs-lookup"><span data-stu-id="1c865-194">The following example shows how to:</span></span>

1. <span data-ttu-id="1c865-195">A meglévő tárfiókok és a Naplóelemzési indexeli az adatok helyek felsorolása</span><span class="sxs-lookup"><span data-stu-id="1c865-195">List the existing storage accounts and locations that Log Analytics will index data from</span></span>
2. <span data-ttu-id="1c865-196">A tárfiók olvasni-konfiguráció létrehozása</span><span class="sxs-lookup"><span data-stu-id="1c865-196">Create a configuration to read from a storage account</span></span>
3. <span data-ttu-id="1c865-197">Az újonnan létrehozott konfigurációjának frissítése index adatokat további helyekről</span><span class="sxs-lookup"><span data-stu-id="1c865-197">Update the newly created configuration to index data from additional locations</span></span>
4. <span data-ttu-id="1c865-198">Az újonnan létrehozott konfiguráció törlése</span><span class="sxs-lookup"><span data-stu-id="1c865-198">Delete the newly created configuration</span></span>

```
# validTables = "WADWindowsEventLogsTable", "LinuxsyslogVer2v0", "WADServiceFabric*EventTable", "WADETWEventTable" 
$workspace = (Get-AzureRmOperationalInsightsWorkspace).Where({$_.Name -eq "your workspace name"})

# Update these two lines with the storage account resource ID and the storage account key for the storage account you want to Log Analytics to  
$storageId = "/subscriptions/ec11ca60-1234-491e-5678-0ea07feae25c/resourceGroups/demo/providers/Microsoft.Storage/storageAccounts/wadv2storage"
$key = "abcd=="

# List existing insights
Get-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name

# Create a new insight
New-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" -StorageAccountResourceId $storageId -StorageAccountKey $key -Tables @("WADWindowsEventLogsTable") -Containers @("wad-iis-logfiles")

# Update existing insight
Set-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" -Tables @("WADWindowsEventLogsTable", "WADETWEventTable") -Containers @("wad-iis-logfiles")

# Remove the insight
Remove-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" 

```

<span data-ttu-id="1c865-199">A fenti parancsfájl segítségével is gyűjteni a tárfiók különböző előfizetésekhez.</span><span class="sxs-lookup"><span data-stu-id="1c865-199">You can also use the preceding script to collect logs from storage accounts in different subscriptions.</span></span> <span data-ttu-id="1c865-200">A parancsfájl tud dolgozni előfizetések között, mert meg van adva, a tárfiók erőforrás azonosítója és a megfelelő hozzáférési kulcsot.</span><span class="sxs-lookup"><span data-stu-id="1c865-200">The script is able to work across subscriptions since you are providing the storage account resource id and a corresponding access key.</span></span> <span data-ttu-id="1c865-201">A hozzáférési kulcs módosításakor kell frissíteni az új kulcsot a tároló betekintést.</span><span class="sxs-lookup"><span data-stu-id="1c865-201">When you change the access key, you need to update the storage insight to have the new key.</span></span>


## <a name="next-steps"></a><span data-ttu-id="1c865-202">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1c865-202">Next steps</span></span>
* <span data-ttu-id="1c865-203">[Tekintse át a napló Analytics PowerShell-parancsmagok](https://msdn.microsoft.com/library/mt188224\(v=azure.300\).aspx) további információt a PowerShell használatával Log Analytics-konfigurációhoz.</span><span class="sxs-lookup"><span data-stu-id="1c865-203">[Review Log Analytics PowerShell cmdlets](https://msdn.microsoft.com/library/mt188224\(v=azure.300\).aspx) for additional information on using PowerShell for configuration of Log Analytics.</span></span>

