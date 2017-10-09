---
title: "aaaUse PowerShell tooCreate, és konfigurálja a Naplóelemzési munkaterület |} Microsoft Docs"
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
ms.openlocfilehash: a6d66194204cc58de6aafb687a19fe9611e0c58e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-log-analytics-using-powershell"></a><span data-ttu-id="d6fb0-104">A Log Analytics felügyelete PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="d6fb0-104">Manage Log Analytics using PowerShell</span></span>
<span data-ttu-id="d6fb0-105">Használhatja a hello [napló Analytics PowerShell-parancsmagok](https://msdn.microsoft.com/library/mt188224\(v=azure.300\).aspx) tooperform különböző függvényeket a Naplóelemzési parancssori vagy parancsfájl részeként.</span><span class="sxs-lookup"><span data-stu-id="d6fb0-105">You can use hello [Log Analytics PowerShell cmdlets](https://msdn.microsoft.com/library/mt188224\(v=azure.300\).aspx) tooperform various functions in Log Analytics from a command line or as part of a script.</span></span>  <span data-ttu-id="d6fb0-106">A PowerShell használatával végezheti el hello feladatok közé:</span><span class="sxs-lookup"><span data-stu-id="d6fb0-106">Examples of hello tasks you can perform with PowerShell include:</span></span>

* <span data-ttu-id="d6fb0-107">Munkaterületek létrehozása</span><span class="sxs-lookup"><span data-stu-id="d6fb0-107">Create a workspace</span></span>
* <span data-ttu-id="d6fb0-108">Hozzáadni vagy eltávolítani egy megoldást</span><span class="sxs-lookup"><span data-stu-id="d6fb0-108">Add or remove a solution</span></span>
* <span data-ttu-id="d6fb0-109">Importálás és exportálás a mentett keresések</span><span class="sxs-lookup"><span data-stu-id="d6fb0-109">Import and export saved searches</span></span>
* <span data-ttu-id="d6fb0-110">Hozzon létre egy számítógépcsoportot</span><span class="sxs-lookup"><span data-stu-id="d6fb0-110">Create a computer group</span></span>
* <span data-ttu-id="d6fb0-111">Hello Windows-ügynök telepítve az IIS-naplók számítógépekről gyűjtésének engedélyezése</span><span class="sxs-lookup"><span data-stu-id="d6fb0-111">Enable collection of IIS logs from computers with hello Windows agent installed</span></span>
* <span data-ttu-id="d6fb0-112">A Linux és Windows számítógépekről a teljesítményszámlálók adatainak összegyűjtése</span><span class="sxs-lookup"><span data-stu-id="d6fb0-112">Collect performance counters from Linux and Windows computers</span></span>
* <span data-ttu-id="d6fb0-113">A Linux rendszerű számítógépeken syslog események gyűjtése</span><span class="sxs-lookup"><span data-stu-id="d6fb0-113">Collect events from syslog on Linux computers</span></span> 
* <span data-ttu-id="d6fb0-114">A Windows Eseménynapló eseményeinek gyűjtése</span><span class="sxs-lookup"><span data-stu-id="d6fb0-114">Collect events from Windows event logs</span></span>
* <span data-ttu-id="d6fb0-115">Egyéni eseménynaplók gyűjtése</span><span class="sxs-lookup"><span data-stu-id="d6fb0-115">Collect custom event logs</span></span>
* <span data-ttu-id="d6fb0-116">Hello napló analytics ügynök tooan Azure virtuális gépek hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d6fb0-116">Add hello log analytics agent tooan Azure virtual machine</span></span>
* <span data-ttu-id="d6fb0-117">Log analytics-tooindex adatok Azure diagnostics használatával gyűjt konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d6fb0-117">Configure log analytics tooindex data collected using Azure diagnostics</span></span>

<span data-ttu-id="d6fb0-118">Ez a cikk ismerteti, amelyek bemutatják a Powershellből a végrehajtható függvények hello két mintakódok.</span><span class="sxs-lookup"><span data-stu-id="d6fb0-118">This article provides two code samples that illustrate some of hello functions that you can perform from PowerShell.</span></span>  <span data-ttu-id="d6fb0-119">Olvassa el a toohello [napló Analytics PowerShell parancsmag-referencia](https://msdn.microsoft.com/library/mt188224\(v=azure.300\).aspx) az egyéb funkciót.</span><span class="sxs-lookup"><span data-stu-id="d6fb0-119">You can refer toohello [Log Analytics PowerShell cmdlet reference](https://msdn.microsoft.com/library/mt188224\(v=azure.300\).aspx) for other functions.</span></span>

> [!NOTE]
> <span data-ttu-id="d6fb0-120">Naplóelemzési korábban meghívták az Operational Insights, ezért a hello parancsmagokban használt hello név.</span><span class="sxs-lookup"><span data-stu-id="d6fb0-120">Log Analytics was previously called Operational Insights, which is why it is hello name used in hello cmdlets.</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="d6fb0-121">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d6fb0-121">Prerequisites</span></span>
<span data-ttu-id="d6fb0-122">Ezek a példák 2.3.0 verziójával vagy későbbi hello AzureRm.OperationalInsights modul működik.</span><span class="sxs-lookup"><span data-stu-id="d6fb0-122">These examples work with version 2.3.0 or later of hello AzureRm.OperationalInsights module.</span></span>


## <a name="create-and-configure-a-log-analytics-workspace"></a><span data-ttu-id="d6fb0-123">Hozza létre és konfigurálja a Naplóelemzési munkaterület</span><span class="sxs-lookup"><span data-stu-id="d6fb0-123">Create and configure a Log Analytics Workspace</span></span>
<span data-ttu-id="d6fb0-124">hello mintában parancsfájl bemutatja, hogyan:</span><span class="sxs-lookup"><span data-stu-id="d6fb0-124">hello following script sample illustrates how to:</span></span>

1. <span data-ttu-id="d6fb0-125">Munkaterületek létrehozása</span><span class="sxs-lookup"><span data-stu-id="d6fb0-125">Create a workspace</span></span>
2. <span data-ttu-id="d6fb0-126">Lista hello elérhető megoldások</span><span class="sxs-lookup"><span data-stu-id="d6fb0-126">List hello available solutions</span></span>
3. <span data-ttu-id="d6fb0-127">Adja hozzá a megoldások toohello munkaterület</span><span class="sxs-lookup"><span data-stu-id="d6fb0-127">Add solutions toohello workspace</span></span>
4. <span data-ttu-id="d6fb0-128">Importálás mentett keresések</span><span class="sxs-lookup"><span data-stu-id="d6fb0-128">Import saved searches</span></span>
5. <span data-ttu-id="d6fb0-129">Exportálás mentett keresések</span><span class="sxs-lookup"><span data-stu-id="d6fb0-129">Export saved searches</span></span>
6. <span data-ttu-id="d6fb0-130">Hozzon létre egy számítógépcsoportot</span><span class="sxs-lookup"><span data-stu-id="d6fb0-130">Create a computer group</span></span>
7. <span data-ttu-id="d6fb0-131">Hello Windows-ügynök telepítve az IIS-naplók számítógépekről gyűjtésének engedélyezése</span><span class="sxs-lookup"><span data-stu-id="d6fb0-131">Enable collection of IIS logs from computers with hello Windows agent installed</span></span>
8. <span data-ttu-id="d6fb0-132">Logikai lemez teljesítményszámlálói gyűjteni a Linux rendszerű számítógépek (% Inode-OK; Szabad hely MB-ban; Foglalt hely; % / Mp átvitelek; Lemezolvasások/mp; Lemezírás/mp)</span><span class="sxs-lookup"><span data-stu-id="d6fb0-132">Collect Logical Disk perf counters from Linux computers (% Used Inodes; Free Megabytes; % Used Space; Disk Transfers/sec; Disk Reads/sec; Disk Writes/sec)</span></span>
9. <span data-ttu-id="d6fb0-133">Syslog-események gyűjtése Linux számítógépekről</span><span class="sxs-lookup"><span data-stu-id="d6fb0-133">Collect syslog events from Linux computers</span></span>
10. <span data-ttu-id="d6fb0-134">Hiba és figyelmeztetési események gyűjtése hello alkalmazások eseménynaplójában keresse meg a Windows rendszerű számítógépek</span><span class="sxs-lookup"><span data-stu-id="d6fb0-134">Collect Error and Warning events from hello Application Event Log from Windows computers</span></span>
11. <span data-ttu-id="d6fb0-135">A Windows rendszerű számítógépek memória rendelkezésre álló memória (MB) teljesítményszámláló gyűjtése.</span><span class="sxs-lookup"><span data-stu-id="d6fb0-135">Collect Memory Available Mbytes performance counter from Windows computers</span></span>
12. <span data-ttu-id="d6fb0-136">Egy egyéni napló gyűjtése</span><span class="sxs-lookup"><span data-stu-id="d6fb0-136">Collect a custom log</span></span> 

```

$ResourceGroup = "oms-example"
$WorkspaceName = "log-analytics-" + (Get-Random -Maximum 99999) # workspace names need toobe unique - Get-Random helps with this for hello example code
$Location = "westeurope"

# List of solutions tooenable
$Solutions = "Security", "Updates", "SQLAssessment"

# Saved Searches tooimport
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

# Custom Log toocollect
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

# Create hello resource group if needed
try {
    Get-AzureRmResourceGroup -Name $ResourceGroup -ErrorAction Stop
} catch {
    New-AzureRmResourceGroup -Name $ResourceGroup -Location $Location
}

# Create hello workspace
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

# Create a computer group based on names (up too5000)
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

## <a name="configuring-log-analytics-tooindex-azure-diagnostics"></a><span data-ttu-id="d6fb0-137">A Naplóelemzési tooindex Azure diagnostics konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d6fb0-137">Configuring Log Analytics tooindex Azure diagnostics</span></span>
<span data-ttu-id="d6fb0-138">Az ügynök nélküli figyelés az Azure-erőforrások hello erőforrások toohave Azure diagnostics engedélyezett és konfigurált toowrite tooa Naplóelemzési munkaterület kell.</span><span class="sxs-lookup"><span data-stu-id="d6fb0-138">For agentless monitoring of Azure resources, hello resources need toohave Azure diagnostics enabled and configured toowrite tooa Log Analytics workspace.</span></span> <span data-ttu-id="d6fb0-139">Ez a megközelítés elküldi az adatokat közvetlenül tooLog elemzés, így nem szükséges tooa tárfiók írt adatok toobe.</span><span class="sxs-lookup"><span data-stu-id="d6fb0-139">This approach sends data directly tooLog Analytics and does not require data toobe written tooa storage account.</span></span> <span data-ttu-id="d6fb0-140">Támogatott erőforrások többek között:</span><span class="sxs-lookup"><span data-stu-id="d6fb0-140">Supported resources include:</span></span>

| <span data-ttu-id="d6fb0-141">Erőforrás típusa</span><span class="sxs-lookup"><span data-stu-id="d6fb0-141">Resource Type</span></span> | <span data-ttu-id="d6fb0-142">Logs</span><span class="sxs-lookup"><span data-stu-id="d6fb0-142">Logs</span></span> | <span data-ttu-id="d6fb0-143">Mérőszámok</span><span class="sxs-lookup"><span data-stu-id="d6fb0-143">Metrics</span></span> |
| --- | --- | --- |
| <span data-ttu-id="d6fb0-144">Application Gateway átjárók</span><span class="sxs-lookup"><span data-stu-id="d6fb0-144">Application Gateways</span></span>    | <span data-ttu-id="d6fb0-145">Igen</span><span class="sxs-lookup"><span data-stu-id="d6fb0-145">Yes</span></span> | <span data-ttu-id="d6fb0-146">Igen</span><span class="sxs-lookup"><span data-stu-id="d6fb0-146">Yes</span></span> |
| <span data-ttu-id="d6fb0-147">Automation-fiókok</span><span class="sxs-lookup"><span data-stu-id="d6fb0-147">Automation accounts</span></span>     | <span data-ttu-id="d6fb0-148">Igen</span><span class="sxs-lookup"><span data-stu-id="d6fb0-148">Yes</span></span> | |
| <span data-ttu-id="d6fb0-149">Batch-fiókok</span><span class="sxs-lookup"><span data-stu-id="d6fb0-149">Batch accounts</span></span>          | <span data-ttu-id="d6fb0-150">Igen</span><span class="sxs-lookup"><span data-stu-id="d6fb0-150">Yes</span></span> | <span data-ttu-id="d6fb0-151">Igen</span><span class="sxs-lookup"><span data-stu-id="d6fb0-151">Yes</span></span> |
| <span data-ttu-id="d6fb0-152">A Data Lake analytics</span><span class="sxs-lookup"><span data-stu-id="d6fb0-152">Data Lake analytics</span></span>     | <span data-ttu-id="d6fb0-153">Igen</span><span class="sxs-lookup"><span data-stu-id="d6fb0-153">Yes</span></span> | | 
| <span data-ttu-id="d6fb0-154">A Data Lake store</span><span class="sxs-lookup"><span data-stu-id="d6fb0-154">Data Lake store</span></span>         | <span data-ttu-id="d6fb0-155">Igen</span><span class="sxs-lookup"><span data-stu-id="d6fb0-155">Yes</span></span> | |
| <span data-ttu-id="d6fb0-156">A rugalmas SQL-készlet</span><span class="sxs-lookup"><span data-stu-id="d6fb0-156">Elastic SQL Pool</span></span>        |     | <span data-ttu-id="d6fb0-157">Igen</span><span class="sxs-lookup"><span data-stu-id="d6fb0-157">Yes</span></span> |
| <span data-ttu-id="d6fb0-158">Event Hub névtér</span><span class="sxs-lookup"><span data-stu-id="d6fb0-158">Event Hub namespace</span></span>     |     | <span data-ttu-id="d6fb0-159">Igen</span><span class="sxs-lookup"><span data-stu-id="d6fb0-159">Yes</span></span> |
| <span data-ttu-id="d6fb0-160">IoT-központok</span><span class="sxs-lookup"><span data-stu-id="d6fb0-160">IoT Hubs</span></span>                |     | <span data-ttu-id="d6fb0-161">Igen</span><span class="sxs-lookup"><span data-stu-id="d6fb0-161">Yes</span></span> |
| <span data-ttu-id="d6fb0-162">Key Vault</span><span class="sxs-lookup"><span data-stu-id="d6fb0-162">Key Vault</span></span>               | <span data-ttu-id="d6fb0-163">Igen</span><span class="sxs-lookup"><span data-stu-id="d6fb0-163">Yes</span></span> | |
| <span data-ttu-id="d6fb0-164">Terheléselosztók</span><span class="sxs-lookup"><span data-stu-id="d6fb0-164">Load Balancers</span></span>          | <span data-ttu-id="d6fb0-165">Igen</span><span class="sxs-lookup"><span data-stu-id="d6fb0-165">Yes</span></span> | |
| <span data-ttu-id="d6fb0-166">Logic Apps</span><span class="sxs-lookup"><span data-stu-id="d6fb0-166">Logic Apps</span></span>              | <span data-ttu-id="d6fb0-167">Igen</span><span class="sxs-lookup"><span data-stu-id="d6fb0-167">Yes</span></span> | <span data-ttu-id="d6fb0-168">Igen</span><span class="sxs-lookup"><span data-stu-id="d6fb0-168">Yes</span></span> |
| <span data-ttu-id="d6fb0-169">Network Security Groups (Hálózati biztonsági csoportok)</span><span class="sxs-lookup"><span data-stu-id="d6fb0-169">Network Security Groups</span></span> | <span data-ttu-id="d6fb0-170">Igen</span><span class="sxs-lookup"><span data-stu-id="d6fb0-170">Yes</span></span> | |
| <span data-ttu-id="d6fb0-171">Redis Cache</span><span class="sxs-lookup"><span data-stu-id="d6fb0-171">Redis Cache</span></span>             |     | <span data-ttu-id="d6fb0-172">Igen</span><span class="sxs-lookup"><span data-stu-id="d6fb0-172">Yes</span></span> |
| <span data-ttu-id="d6fb0-173">Szolgáltatások keresése</span><span class="sxs-lookup"><span data-stu-id="d6fb0-173">Search services</span></span>         | <span data-ttu-id="d6fb0-174">Igen</span><span class="sxs-lookup"><span data-stu-id="d6fb0-174">Yes</span></span> | <span data-ttu-id="d6fb0-175">Igen</span><span class="sxs-lookup"><span data-stu-id="d6fb0-175">Yes</span></span> |
| <span data-ttu-id="d6fb0-176">Service Bus-névtér</span><span class="sxs-lookup"><span data-stu-id="d6fb0-176">Service Bus namespace</span></span>   |     | <span data-ttu-id="d6fb0-177">Igen</span><span class="sxs-lookup"><span data-stu-id="d6fb0-177">Yes</span></span> |
| <span data-ttu-id="d6fb0-178">SQL (12-es verzió)</span><span class="sxs-lookup"><span data-stu-id="d6fb0-178">SQL (v12)</span></span>               |     | <span data-ttu-id="d6fb0-179">Igen</span><span class="sxs-lookup"><span data-stu-id="d6fb0-179">Yes</span></span> |
| <span data-ttu-id="d6fb0-180">Webhelyek</span><span class="sxs-lookup"><span data-stu-id="d6fb0-180">Web Sites</span></span>               |     | <span data-ttu-id="d6fb0-181">Igen</span><span class="sxs-lookup"><span data-stu-id="d6fb0-181">Yes</span></span> |
| <span data-ttu-id="d6fb0-182">Kiszolgáló-webfarmok</span><span class="sxs-lookup"><span data-stu-id="d6fb0-182">Web Server farms</span></span>        |     | <span data-ttu-id="d6fb0-183">Igen</span><span class="sxs-lookup"><span data-stu-id="d6fb0-183">Yes</span></span> |

<span data-ttu-id="d6fb0-184">Az elérhető mérőszámok hello hello részletekért lásd a túl[támogatott Azure-figyelő metrikák](../monitoring-and-diagnostics/monitoring-supported-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="d6fb0-184">For hello details of hello available metrics, refer too[supported metrics with Azure Monitor](../monitoring-and-diagnostics/monitoring-supported-metrics.md).</span></span>

<span data-ttu-id="d6fb0-185">Hello elérhető naplók hello részletekért lásd a túl[szolgáltatások és a séma támogatja a diagnosztikai naplók](../monitoring-and-diagnostics/monitoring-diagnostic-logs-schema.md).</span><span class="sxs-lookup"><span data-stu-id="d6fb0-185">For hello details of hello available logs, refer too[supported services and schema for diagnostic logs](../monitoring-and-diagnostics/monitoring-diagnostic-logs-schema.md).</span></span>

```
$workspaceId = "/subscriptions/d2e37fee-1234-40b2-5678-0b2199de3b50/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$resourceId = "/SUBSCRIPTIONS/ec11ca60-1234-491e-5678-0ea07feae25c/RESOURCEGROUPS/DEMO/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/DEMO" 

Set-AzureRmDiagnosticSetting -ResourceId $resourceId -WorkspaceId $workspaceId -Enabled $true
```

<span data-ttu-id="d6fb0-186">Használhatja az erőforrásokat, amelyek különböző előfizetésekhez parancsmag toocollect naplók megelőző hello is.</span><span class="sxs-lookup"><span data-stu-id="d6fb0-186">You can also use hello preceding cmdlet toocollect logs from resources that are in different subscriptions.</span></span> <span data-ttu-id="d6fb0-187">hello parancsmag is képes toowork előfizetések között, mivel mindkét hello erőforrás létrehozása a naplók hello azonosítója meg van adva, és hello munkaterület hello naplók küldött.</span><span class="sxs-lookup"><span data-stu-id="d6fb0-187">hello cmdlet is able toowork across subscriptions since you are providing hello id of both hello resource creating logs and hello workspace hello logs are sent to.</span></span>


## <a name="configuring-log-analytics-tooindex-azure-diagnostics-from-storage"></a><span data-ttu-id="d6fb0-188">A Naplóelemzési tooindex tárolóból az Azure diagnostics konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d6fb0-188">Configuring Log Analytics tooindex Azure diagnostics from storage</span></span>
<span data-ttu-id="d6fb0-189">toocollect naplóadatait belül egy klasszikus felhőalapú szolgáltatás, vagy a service fabric-fürt futó példányát, toofirst tooAzure írási hello adattárolás kell.</span><span class="sxs-lookup"><span data-stu-id="d6fb0-189">toocollect log data from within a running instance of a classic cloud service or a service fabric cluster, you need toofirst write hello data tooAzure storage.</span></span> <span data-ttu-id="d6fb0-190">Naplóelemzési majd van konfigurálva toocollect hello naplók hello tárfiókból.</span><span class="sxs-lookup"><span data-stu-id="d6fb0-190">Log Analytics is then configured toocollect hello logs from hello storage account.</span></span> <span data-ttu-id="d6fb0-191">Támogatott erőforrások többek között:</span><span class="sxs-lookup"><span data-stu-id="d6fb0-191">Supported resources include:</span></span>

* <span data-ttu-id="d6fb0-192">Klasszikus felhőszolgáltatások (webes és feldolgozói szerepkörök)</span><span class="sxs-lookup"><span data-stu-id="d6fb0-192">Classic cloud services (web and worker roles)</span></span>
* <span data-ttu-id="d6fb0-193">Service fabric-fürtök</span><span class="sxs-lookup"><span data-stu-id="d6fb0-193">Service fabric clusters</span></span>

<span data-ttu-id="d6fb0-194">a következő példa azt mutatja meg hogyan hello számára:</span><span class="sxs-lookup"><span data-stu-id="d6fb0-194">hello following example shows how to:</span></span>

1. <span data-ttu-id="d6fb0-195">Lista hello meglévő tárfiókok és Naplóelemzési indexeli az adatok helyek</span><span class="sxs-lookup"><span data-stu-id="d6fb0-195">List hello existing storage accounts and locations that Log Analytics will index data from</span></span>
2. <span data-ttu-id="d6fb0-196">Egy konfigurációs tooread a storage-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="d6fb0-196">Create a configuration tooread from a storage account</span></span>
3. <span data-ttu-id="d6fb0-197">Az újonnan létrehozott konfigurációs tooindex adatokat további helyekről hello frissítése</span><span class="sxs-lookup"><span data-stu-id="d6fb0-197">Update hello newly created configuration tooindex data from additional locations</span></span>
4. <span data-ttu-id="d6fb0-198">Az újonnan létrehozott hello konfiguráció törlése</span><span class="sxs-lookup"><span data-stu-id="d6fb0-198">Delete hello newly created configuration</span></span>

```
# validTables = "WADWindowsEventLogsTable", "LinuxsyslogVer2v0", "WADServiceFabric*EventTable", "WADETWEventTable" 
$workspace = (Get-AzureRmOperationalInsightsWorkspace).Where({$_.Name -eq "your workspace name"})

# Update these two lines with hello storage account resource ID and hello storage account key for hello storage account you want tooLog Analytics too 
$storageId = "/subscriptions/ec11ca60-1234-491e-5678-0ea07feae25c/resourceGroups/demo/providers/Microsoft.Storage/storageAccounts/wadv2storage"
$key = "abcd=="

# List existing insights
Get-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name

# Create a new insight
New-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" -StorageAccountResourceId $storageId -StorageAccountKey $key -Tables @("WADWindowsEventLogsTable") -Containers @("wad-iis-logfiles")

# Update existing insight
Set-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" -Tables @("WADWindowsEventLogsTable", "WADETWEventTable") -Containers @("wad-iis-logfiles")

# Remove hello insight
Remove-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" 

```

<span data-ttu-id="d6fb0-199">Parancsfájl toocollect naplók a tárfiók különböző előfizetésekhez megelőző hello is használható.</span><span class="sxs-lookup"><span data-stu-id="d6fb0-199">You can also use hello preceding script toocollect logs from storage accounts in different subscriptions.</span></span> <span data-ttu-id="d6fb0-200">hello parancsfájl is képes toowork előfizetések között, mivel a hello tárfiók erőforrás azonosítója és a megfelelő hozzáférési kulcs ad.</span><span class="sxs-lookup"><span data-stu-id="d6fb0-200">hello script is able toowork across subscriptions since you are providing hello storage account resource id and a corresponding access key.</span></span> <span data-ttu-id="d6fb0-201">Hello hozzáférési kulcs módosításakor kell tooupdate hello tárolási insight toohave hello új kulcsot.</span><span class="sxs-lookup"><span data-stu-id="d6fb0-201">When you change hello access key, you need tooupdate hello storage insight toohave hello new key.</span></span>


## <a name="next-steps"></a><span data-ttu-id="d6fb0-202">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d6fb0-202">Next steps</span></span>
* <span data-ttu-id="d6fb0-203">[Tekintse át a napló Analytics PowerShell-parancsmagok](https://msdn.microsoft.com/library/mt188224\(v=azure.300\).aspx) további információt a PowerShell használatával Log Analytics-konfigurációhoz.</span><span class="sxs-lookup"><span data-stu-id="d6fb0-203">[Review Log Analytics PowerShell cmdlets](https://msdn.microsoft.com/library/mt188224\(v=azure.300\).aspx) for additional information on using PowerShell for configuration of Log Analytics.</span></span>

