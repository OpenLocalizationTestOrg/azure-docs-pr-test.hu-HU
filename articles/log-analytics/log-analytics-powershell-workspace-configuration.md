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
# <a name="manage-log-analytics-using-powershell"></a>A Log Analytics felügyelete PowerShell használatával
Használhatja a hello [napló Analytics PowerShell-parancsmagok](https://msdn.microsoft.com/library/mt188224\(v=azure.300\).aspx) tooperform különböző függvényeket a Naplóelemzési parancssori vagy parancsfájl részeként.  A PowerShell használatával végezheti el hello feladatok közé:

* Munkaterületek létrehozása
* Hozzáadni vagy eltávolítani egy megoldást
* Importálás és exportálás a mentett keresések
* Hozzon létre egy számítógépcsoportot
* Hello Windows-ügynök telepítve az IIS-naplók számítógépekről gyűjtésének engedélyezése
* A Linux és Windows számítógépekről a teljesítményszámlálók adatainak összegyűjtése
* A Linux rendszerű számítógépeken syslog események gyűjtése 
* A Windows Eseménynapló eseményeinek gyűjtése
* Egyéni eseménynaplók gyűjtése
* Hello napló analytics ügynök tooan Azure virtuális gépek hozzáadása
* Log analytics-tooindex adatok Azure diagnostics használatával gyűjt konfigurálása

Ez a cikk ismerteti, amelyek bemutatják a Powershellből a végrehajtható függvények hello két mintakódok.  Olvassa el a toohello [napló Analytics PowerShell parancsmag-referencia](https://msdn.microsoft.com/library/mt188224\(v=azure.300\).aspx) az egyéb funkciót.

> [!NOTE]
> Naplóelemzési korábban meghívták az Operational Insights, ezért a hello parancsmagokban használt hello név.
> 
> 

## <a name="prerequisites"></a>Előfeltételek
Ezek a példák 2.3.0 verziójával vagy későbbi hello AzureRm.OperationalInsights modul működik.


## <a name="create-and-configure-a-log-analytics-workspace"></a>Hozza létre és konfigurálja a Naplóelemzési munkaterület
hello mintában parancsfájl bemutatja, hogyan:

1. Munkaterületek létrehozása
2. Lista hello elérhető megoldások
3. Adja hozzá a megoldások toohello munkaterület
4. Importálás mentett keresések
5. Exportálás mentett keresések
6. Hozzon létre egy számítógépcsoportot
7. Hello Windows-ügynök telepítve az IIS-naplók számítógépekről gyűjtésének engedélyezése
8. Logikai lemez teljesítményszámlálói gyűjteni a Linux rendszerű számítógépek (% Inode-OK; Szabad hely MB-ban; Foglalt hely; % / Mp átvitelek; Lemezolvasások/mp; Lemezírás/mp)
9. Syslog-események gyűjtése Linux számítógépekről
10. Hiba és figyelmeztetési események gyűjtése hello alkalmazások eseménynaplójában keresse meg a Windows rendszerű számítógépek
11. A Windows rendszerű számítógépek memória rendelkezésre álló memória (MB) teljesítményszámláló gyűjtése.
12. Egy egyéni napló gyűjtése 

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

## <a name="configuring-log-analytics-tooindex-azure-diagnostics"></a>A Naplóelemzési tooindex Azure diagnostics konfigurálása
Az ügynök nélküli figyelés az Azure-erőforrások hello erőforrások toohave Azure diagnostics engedélyezett és konfigurált toowrite tooa Naplóelemzési munkaterület kell. Ez a megközelítés elküldi az adatokat közvetlenül tooLog elemzés, így nem szükséges tooa tárfiók írt adatok toobe. Támogatott erőforrások többek között:

| Erőforrás típusa | Logs | Mérőszámok |
| --- | --- | --- |
| Application Gateway átjárók    | Igen | Igen |
| Automation-fiókok     | Igen | |
| Batch-fiókok          | Igen | Igen |
| A Data Lake analytics     | Igen | | 
| A Data Lake store         | Igen | |
| A rugalmas SQL-készlet        |     | Igen |
| Event Hub névtér     |     | Igen |
| IoT-központok                |     | Igen |
| Key Vault               | Igen | |
| Terheléselosztók          | Igen | |
| Logic Apps              | Igen | Igen |
| Network Security Groups (Hálózati biztonsági csoportok) | Igen | |
| Redis Cache             |     | Igen |
| Szolgáltatások keresése         | Igen | Igen |
| Service Bus-névtér   |     | Igen |
| SQL (12-es verzió)               |     | Igen |
| Webhelyek               |     | Igen |
| Kiszolgáló-webfarmok        |     | Igen |

Az elérhető mérőszámok hello hello részletekért lásd a túl[támogatott Azure-figyelő metrikák](../monitoring-and-diagnostics/monitoring-supported-metrics.md).

Hello elérhető naplók hello részletekért lásd a túl[szolgáltatások és a séma támogatja a diagnosztikai naplók](../monitoring-and-diagnostics/monitoring-diagnostic-logs-schema.md).

```
$workspaceId = "/subscriptions/d2e37fee-1234-40b2-5678-0b2199de3b50/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$resourceId = "/SUBSCRIPTIONS/ec11ca60-1234-491e-5678-0ea07feae25c/RESOURCEGROUPS/DEMO/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/DEMO" 

Set-AzureRmDiagnosticSetting -ResourceId $resourceId -WorkspaceId $workspaceId -Enabled $true
```

Használhatja az erőforrásokat, amelyek különböző előfizetésekhez parancsmag toocollect naplók megelőző hello is. hello parancsmag is képes toowork előfizetések között, mivel mindkét hello erőforrás létrehozása a naplók hello azonosítója meg van adva, és hello munkaterület hello naplók küldött.


## <a name="configuring-log-analytics-tooindex-azure-diagnostics-from-storage"></a>A Naplóelemzési tooindex tárolóból az Azure diagnostics konfigurálása
toocollect naplóadatait belül egy klasszikus felhőalapú szolgáltatás, vagy a service fabric-fürt futó példányát, toofirst tooAzure írási hello adattárolás kell. Naplóelemzési majd van konfigurálva toocollect hello naplók hello tárfiókból. Támogatott erőforrások többek között:

* Klasszikus felhőszolgáltatások (webes és feldolgozói szerepkörök)
* Service fabric-fürtök

a következő példa azt mutatja meg hogyan hello számára:

1. Lista hello meglévő tárfiókok és Naplóelemzési indexeli az adatok helyek
2. Egy konfigurációs tooread a storage-fiók létrehozása
3. Az újonnan létrehozott konfigurációs tooindex adatokat további helyekről hello frissítése
4. Az újonnan létrehozott hello konfiguráció törlése

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

Parancsfájl toocollect naplók a tárfiók különböző előfizetésekhez megelőző hello is használható. hello parancsfájl is képes toowork előfizetések között, mivel a hello tárfiók erőforrás azonosítója és a megfelelő hozzáférési kulcs ad. Hello hozzáférési kulcs módosításakor kell tooupdate hello tárolási insight toohave hello új kulcsot.


## <a name="next-steps"></a>Következő lépések
* [Tekintse át a napló Analytics PowerShell-parancsmagok](https://msdn.microsoft.com/library/mt188224\(v=azure.300\).aspx) további információt a PowerShell használatával Log Analytics-konfigurációhoz.

