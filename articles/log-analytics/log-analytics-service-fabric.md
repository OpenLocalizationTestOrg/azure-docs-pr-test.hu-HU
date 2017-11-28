---
title: "Service Fabric-alkalmazások az Azure Naplóelemzés PowerShell-lel aaaAssess |} Microsoft Docs"
description: "A PowerShell tooassess hello kockázat és a Service Fabric-alkalmazások, micro-szolgáltatások, csomópontokat és fürtöket állapotának Naplóelemzési hello Service Fabric megoldás is használhatja."
services: log-analytics
documentationcenter: 
author: niniikhena
manager: jochan
editor: 
ms.assetid: 2047b3fa-96b1-4230-af5d-a4c331d973ce
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2017
ms.author: nini
ms.openlocfilehash: 3f6d6c0df02d6d453b77e50b75b64bf7eb73bbbf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="assess-azure-service-fabric-applications-and-micro-services-with-powershell"></a><span data-ttu-id="411cc-103">Azure Service Fabric-alkalmazások és a PowerShell-lel micro-szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="411cc-103">Assess Azure Service Fabric applications and micro-services with PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="411cc-104">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="411cc-104">Resource Manager</span></span>](log-analytics-service-fabric-azure-resource-manager.md)
> * [<span data-ttu-id="411cc-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="411cc-105">PowerShell</span></span>](log-analytics-service-fabric.md)
>
>


![A Service Fabric szimbólum](./media/log-analytics-service-fabric/service-fabric-assessment-symbol.png)

<span data-ttu-id="411cc-107">Ez a cikk ismerteti, hogyan toouse hello Naplóelemzési toohelp megoldás a Service Fabric azonosítása és elhárítása a Service Fabric-fürt között.</span><span class="sxs-lookup"><span data-stu-id="411cc-107">This article describes how toouse hello Service Fabric solution in Log Analytics toohelp identify and troubleshoot issues across your Service Fabric cluster.</span></span> <span data-ttu-id="411cc-108">A program segít tekintse meg, hogyan működnek a Service Fabric-csomópontokon, és hogyan az alkalmazások és micro-szolgáltatások futnak.</span><span class="sxs-lookup"><span data-stu-id="411cc-108">It helps you see how your Service Fabric nodes are performing and how your applications and micro-services are running.</span></span>

<span data-ttu-id="411cc-109">hello Service Fabric megoldás Azure diagnosztikai adatokat a Service Fabric virtuális gépek által az adatok gyűjtése az Azure ÜVEGVATTA táblát használ.</span><span class="sxs-lookup"><span data-stu-id="411cc-109">hello Service Fabric solution uses Azure Diagnostics data from your Service Fabric VMs, by collecting this data from your Azure WAD tables.</span></span> <span data-ttu-id="411cc-110">A Naplóelemzési ezután beolvassa a Service Fabric keretrendszer események hello:</span><span class="sxs-lookup"><span data-stu-id="411cc-110">Log Analytics then reads hello following Service Fabric framework events:</span></span>

- <span data-ttu-id="411cc-111">**Megbízható eseményei**</span><span class="sxs-lookup"><span data-stu-id="411cc-111">**Reliable Service Events**</span></span>
- <span data-ttu-id="411cc-112">**Szereplő események**</span><span class="sxs-lookup"><span data-stu-id="411cc-112">**Actor Events**</span></span>
- <span data-ttu-id="411cc-113">**A működési események**</span><span class="sxs-lookup"><span data-stu-id="411cc-113">**Operational Events**</span></span>
- <span data-ttu-id="411cc-114">**Egyéni ETW-események**</span><span class="sxs-lookup"><span data-stu-id="411cc-114">**Custom ETW events**</span></span>

<span data-ttu-id="411cc-115">hello Service Fabric megoldás irányítópult jelentős problémák és kapcsolódó eseményeket jeleníti meg a Service Fabric-környezetben.</span><span class="sxs-lookup"><span data-stu-id="411cc-115">hello Service Fabric solution dashboard shows you notable issues and relevant events in your Service Fabric environment.</span></span>

## <a name="installing-and-configuring-hello-solution"></a><span data-ttu-id="411cc-116">Telepítése és konfigurálása hello megoldás</span><span class="sxs-lookup"><span data-stu-id="411cc-116">Installing and configuring hello solution</span></span>
<span data-ttu-id="411cc-117">Kövesse az alábbi három egyszerű lépését tooinstall és hello megoldás konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="411cc-117">Follow these three easy steps tooinstall and configure hello solution:</span></span>

1. <span data-ttu-id="411cc-118">Hello toocreate összes fürterőforrás, beleértve a storage-fiókok, a munkaterület meg használt Azure-előfizetést társíthat.</span><span class="sxs-lookup"><span data-stu-id="411cc-118">Associate hello Azure subscription that you used toocreate all cluster resources, including storage accounts, with your workspace.</span></span> <span data-ttu-id="411cc-119">Lásd: [Ismerkedés a Naplóelemzési](log-analytics-get-started.md) a Naplóelemzési munkaterület létrehozásával kapcsolatos információkat.</span><span class="sxs-lookup"><span data-stu-id="411cc-119">See [Get started with Log Analytics](log-analytics-get-started.md) for information about creating a Log Analytics workspace.</span></span>
2. <span data-ttu-id="411cc-120">A Naplóelemzési toocollect konfigurálása és a Service Fabric naplók megtekintése.</span><span class="sxs-lookup"><span data-stu-id="411cc-120">Configure Log Analytics toocollect and view Service Fabric logs.</span></span>
3. <span data-ttu-id="411cc-121">Engedélyezze a hello Service Fabric-megoldás a munkaterületen.</span><span class="sxs-lookup"><span data-stu-id="411cc-121">Enable hello Service Fabric solution in your workspace.</span></span>

## <a name="configure-log-analytics-toocollect-and-view-service-fabric-logs"></a><span data-ttu-id="411cc-122">A Naplóelemzési toocollect konfigurálása és a Service Fabric naplók megtekintése</span><span class="sxs-lookup"><span data-stu-id="411cc-122">Configure Log Analytics toocollect and view Service Fabric logs</span></span>
<span data-ttu-id="411cc-123">Ebben a szakaszban megismerheti, hogyan naplózza az tooconfigure Naplóelemzési tooretrieve Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="411cc-123">In this section, you learn how tooconfigure Log Analytics tooretrieve Service Fabric logs.</span></span> <span data-ttu-id="411cc-124">hello naplók tooview lehetővé teszik, elemzése és elhárítása a fürt vagy a fürt, hello OMS-portálon futó hello alkalmazások és szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="411cc-124">hello logs allow you tooview, analyze, and troubleshoot issues in your cluster or in hello applications and services running in that cluster, using hello OMS portal.</span></span>

> [!NOTE]
> <span data-ttu-id="411cc-125">Hello Azure Diagnostics bővítmény tooupload hello naplók tárolási táblák konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="411cc-125">Configure hello Azure Diagnostics extension tooupload hello logs for storage tables.</span></span> <span data-ttu-id="411cc-126">hello táblák mi Naplóelemzési keresi meg kell egyeznie.</span><span class="sxs-lookup"><span data-stu-id="411cc-126">hello tables must match what Log Analytics looks for.</span></span> <span data-ttu-id="411cc-127">További információkért lásd: [hogyan toocollect naplózza az Azure Diagnostics](../service-fabric/service-fabric-diagnostics-how-to-setup-wad.md).</span><span class="sxs-lookup"><span data-stu-id="411cc-127">For more information, see [How toocollect logs with Azure Diagnostics](../service-fabric/service-fabric-diagnostics-how-to-setup-wad.md).</span></span> <span data-ttu-id="411cc-128">hello konfigurációs beállításokat a cikkben szereplő példák megjelenítése milyen hello neveket hello tárhelyet kell szerepelnie.</span><span class="sxs-lookup"><span data-stu-id="411cc-128">hello configuration settings examples in this article show you what hello names of hello storage tables should be.</span></span> <span data-ttu-id="411cc-129">Miután diagnosztika hello fürtön be van állítva, és a naplók tooa tárfiók tölti fel, hello következő lépésre tooconfigure Naplóelemzési toocollect-e ezek a naplók.</span><span class="sxs-lookup"><span data-stu-id="411cc-129">Once Diagnostics is set up on hello cluster and is uploading logs tooa storage account, hello next step is tooconfigure Log Analytics toocollect these logs.</span></span>
>
>

<span data-ttu-id="411cc-130">Győződjön meg arról, hogy a hello frissíteni **EtwEventSourceProviderConfiguration** hello szakasz **template.json** tooadd bejegyzések hello frissítése új EventSources hello konfiguráció alkalmazása előtt a fájl futó **deploy.ps1**.</span><span class="sxs-lookup"><span data-stu-id="411cc-130">Ensure that you update hello **EtwEventSourceProviderConfiguration** section in hello **template.json** file tooadd entries for hello new EventSources before you apply hello configuration update by running **deploy.ps1**.</span></span> <span data-ttu-id="411cc-131">hello tábla a feltöltés hello azonos (ETWEventTable) szerint.</span><span class="sxs-lookup"><span data-stu-id="411cc-131">hello table for upload is hello same as (ETWEventTable).</span></span> <span data-ttu-id="411cc-132">Hello pillanatban Naplóelemzési is csak olvasható alkalmazás ETW-események hello *WADETWEventTable* tábla.</span><span class="sxs-lookup"><span data-stu-id="411cc-132">At hello moment, Log Analytics can only read application ETW events from hello *WADETWEventTable* table.</span></span>

<span data-ttu-id="411cc-133">hello következő eszközök használt tooperform hello műveleteket ebben a szakaszban:</span><span class="sxs-lookup"><span data-stu-id="411cc-133">hello following tools are used tooperform some of hello operations in this section:</span></span>

* <span data-ttu-id="411cc-134">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="411cc-134">Azure PowerShell</span></span>
* [<span data-ttu-id="411cc-135">Operations Management Suite</span><span class="sxs-lookup"><span data-stu-id="411cc-135">Operations Management Suite</span></span>](http://www.microsoft.com/oms)

### <a name="configure-a-log-analytics-workspace-tooshow-hello-cluster-logs"></a><span data-ttu-id="411cc-136">A Naplóelemzési munkaterület tooshow hello fürt jelentkezik konfigurálása</span><span class="sxs-lookup"><span data-stu-id="411cc-136">Configure a Log Analytics workspace tooshow hello cluster logs</span></span>

<span data-ttu-id="411cc-137">A Naplóelemzési munkaterület létrehozása után konfigurálja a hello munkaterület toopull naplók hello Azure tárolási táblákból.</span><span class="sxs-lookup"><span data-stu-id="411cc-137">After you create a Log Analytics workspace, configure hello workspace toopull logs from hello Azure storage tables.</span></span> <span data-ttu-id="411cc-138">Ezután futtassa a következő PowerShell-parancsfájl hello:</span><span class="sxs-lookup"><span data-stu-id="411cc-138">Then, run hello following PowerShell script:</span></span>

```
<#
    This script will configure an Operations Management Suite workspace (previously called an Operational Insights workspace) tooread Diagnostics from an Azure Storage account.
    It will enable all supported data types (currently Service Fabric Events, ETW Events and IIS Logs).
    It supports Resource Manager storage accounts.
    If you have more than one Azure Subscription, you will be prompted for hello subscription tooconfigure.
    If you have more than one Log Analytics workspace you will be prompted for hello workspace tooconfigure.
    It will then look through your Service Fabric clusters, and configure your Log Analytics workspace tooread Diagnostics from storage accounts that are connected toothat cluster and have diagnostics enabled.
#>

try
{
    Get-AzureRMContext
}
catch [System.Management.Automation.PSInvalidOperationException]
{
    Add-AzureRmAccount
}

$validTables = "WADServiceFabric*EventTable", "WADETWEventTable"
function Select-Subscription {
      $subscription = ""
      $allSubscriptions = Get-AzureRmSubscription
      switch ($allSubscriptions.Count) {
             0 {Write-Error "No Operations Management Suite workspaces found"}
             1 {return $allSubscriptions}
        default {
            $uiPrompt = "Enter hello number corresponding toohello Azure subscription you would like toowork with.`n"

            $count = 1
            foreach ($subscription in $allSubscriptions) {
                $uiPrompt += "$count. " + $subscription.Name + " (" + $subscription.Id + ")`n"
                $count++
            }
            $answer = (Read-Host -Prompt $uiPrompt) - 1
            $subscription = $allSubscriptions[$answer]
             Write-Host $subscription.Id
        }  
    }
    return $subscription
}

function Select-Workspace {
    $workspace = ""
    $allWorkspaces = Get-AzureRmOperationalInsightsWorkspace  

    switch ($allWorkspaces.Count) {
        0 {Write-Error "No Operations Management Suite workspaces found. `n"}
        1 {return $allWorkspaces}
        default {
            $uiPrompt = "Enter hello number corresponding toohello workspace you want tooconfigure.`n"
            $count = 1
            foreach ($workspace in $allWorkspaces) {
                $uiPrompt += "$count. " + $workspace.Name + " (" + $workspace.CustomerId + ")`n"
                $count++
            }
            $answer = (Read-Host -Prompt $uiPrompt) - 1
            $workspace = $allWorkspaces[$answer]
             Write-Host $workspace.WorkspaceName
        }  
    }
    return $workspace
}

function Check-ETWProviderLogging {
     param(
     [string]$id,
     [string]$provider,
     [string]$expectedTable,
     [string]$table
    )       
         Write-Debug ("ID: $id Provider: $provider ExpectedTable $expectedTable ActualTable $table")
         if ( ($table -eq $null) -or ($table -eq ""))  
         {
             Write-Warning ("$id No configuration found for $provider. Configure Azure diagnostics toowrite too$expectedTable.")
         }  
         elseif ( $table -ne $expectedTable )
         {
             Write-Warning ("$id $provider events are being written too$table instead of WAD$expectedTable. Events will not be collected by Log Analytics")
         }  
         else
         {
             Write-Verbose "$id $provider events are being written tooWAD$expectedTable (Correct configuration.)"
         }
 }

function Check-ServiceFabricScaleSetDiagnostics {
     param(
          [psobject]$scaleSetDiagnostics
   )
     $storageAccountsFound = @()
     Write-Verbose ("Checking " + $scaleSetDiagnostics)
     $sfReliableActorTable = $null
     $sfReliableServiceTable = $null
     $sfOperationalTable = $null

     Write-Debug $scaleSetDiagnostics
     $serviceFabricProviderList = ""
     $etwManifestProviderList = ""

     if ( $scaleSetDiagnostics.xmlCfg )  
      {
             Write-Debug ("Found XMLcfg")
             $xmlCfg = [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($scaleSetDiagnostics.xmlCfg))
             Write-Debug $xmlCfg
             $etwProviders = Select-Xml -Content $xmlCfg -XPath "//EtwProviders"                 
             $serviceFabricProviderList = $etwProviders.Node.EtwEventSourceProviderConfiguration
             $etwManifestProviderList = $etwProviders.Node.EtwManifestProviderConfiguration
      } elseif ($scaleSetDiagnostics.WadCfg )  
     {
         Write-Debug ("Found WADcfg")
         Write-Debug $scaleSetDiagnostics.WadCfg
         $serviceFabricProviderList = $scaleSetDiagnostics.WadCfg.DiagnosticMonitorConfiguration.EtwProviders.EtwEventSourceProviderConfiguration
         $etwManifestProviderList = $scaleSetDiagnostics.WadCfg.DiagnosticMonitorConfiguration.EtwProviders.EtwManifestProviderConfiguration
     } else
     {
         Write-Error "Unable tooparse Azure Diagnostics setting for $id"
             Write-Warning ("$id does not have diagnostics enabled")
     }
     foreach ($provider in $serviceFabricProviderList)  
     {
         Write-Debug ("Event Source Provider: " + $provider.Provider + " Destination: " + $provider.DefaultEvents.eventDestination)
         if ($provider.Provider -eq "Microsoft-ServiceFabric-Actors")
         {
             $sfReliableActorTable = $provider.DefaultEvents.eventDestination  
         } elseif ($provider.Provider -eq "Microsoft-ServiceFabric-Services")  
         {  
             $sfReliableServiceTable = $provider.DefaultEvents.eventDestination  
         } else  
         {
             Check-ETWProviderLogging $id $provider.Provider "ETWEventTable" $provider.DefaultEvents.eventDestination
         }
     }
     foreach ($provider in $etwManifestProviderList)
     {
         Write-Debug ("Manifest Provider: " + $provider.Provider + " Destination: " + $provider.DefaultEvents.eventDestination)
         if ($provider.Provider -eq "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8")
         {
             $sfOperationalTable = $provider.DefaultEvents.eventDestination  
         } else  
         {
             Check-ETWProviderLogging $id $provider.Provider "ETWEventTable" $provider.DefaultEvents.eventDestination
         }
     }

     Check-ETWProviderLogging $id "Microsoft-ServiceFabric-Actors" "ServiceFabricReliableActorEventTable" $sfReliableActorTable
     Check-ETWProviderLogging $id "Microsoft-ServiceFabric-Services" "ServiceFabricReliableServiceEventTable" $sfReliableServiceTable
     Check-ETWProviderLogging $id "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8 (System events)" "ServiceFabricSystemEventTable" $sfOperationalTable

     Write-Verbose ("StorageAccount: " + $scaleSetDiagnostics.StorageAccount)
     $storageAccountsFound += ($scaleSetDiagnostics.StorageAccount)
     return ($storageAccountsFound)
 }

function Select-StorageAccount {
    $allResources = Get-AzureRmResource #pulls in all resources
    $serviceFabricClusters = $allResources.Where({$_.ResourceType -eq "Microsoft.ServiceFabric/clusters"}) #pulls in all service fabric clusters in hello resource
    $storageAccountList = @()
    foreach($cluster in $serviceFabricClusters) {
        Write-Host("Checking cluster: " + $cluster.Name)
         $scaleSet = $allResources.Where({($_.ResourceType -eq "Microsoft.Compute/virtualMachineScaleSets") -and ($_.ResourceGroupName -eq $cluster.ResourceGroupName)})

         foreach($set in $scaleSet) {
             $resource = Get-AzureRmResource -ResourceId $set.ResourceId
             $extensions = $resource.Properties.VirtualMachineProfile.ExtensionProfile.Extensions

             foreach($ext in $extensions) {
                 if ($ext.Properties.Publisher -eq "Microsoft.Azure.Diagnostics" -and $ext.Properties.Type -eq "IaaSDiagnostics") {
                     $storageAccountList += (Check-ServiceFabricScaleSetDiagnostics $ext.Properties.Settings)
                 }
             }
          }

         $storageAccountsToCheck = $allResources.Where({($_.ResourceType -eq "Microsoft.Storage/storageAccounts") -and ($_.ResourceName -in $storageAccountList)})

         if ($storageAccountsToCheck.Count -eq "0") {
                Write-Error "No storage accounts found"
           }
           else {
                    foreach ($storageAccount in $storageAccountsToCheck) {
                        Write-Host("Checking Storage Account: " + $storageAccount.Name)
                        $insightsName = $storageAccount.Name + $workspace.Name
                        $existingConfig = ""
                        try
                            {
                                $existingConfig = Get-AzureRmOperationalInsightsStorageInsight -Workspace $workspace -Name $insightsName -ErrorAction Stop
                            }
                        catch
                            {
                                # HTTP Not Found is returned if hello storage insight doesn't exist
                            }
                        if ($existingConfig) {                         
                                  [array]$Tables = $existingConfig.Tables
                                   foreach($table in $validTables) {
                                         if($Tables -notcontains $table) {
                                               $Tables += $table
                                               $dirty = $true;
                                               Write-Host "Adding Table: $table";
                                         }
                                         else {
                                               Write-Host "$table is already configured.`n";
                                             }
                                      }
                                      # If any of hello tables from hello table list are not already monitored, then we add them
                                   if($dirty -eq $true) {
                                           Set-AzureRmOperationalInsightsStorageInsight -Workspace $workspace -Name $insightsName -Tables $Tables
                                           Write-Host "Updating Storage Insight. `n"
                                    }
                                    else {
                                           Write-Host "Storage Insight already updated."
                                  }
                          }
                     else {
                            $key = (Get-AzureRmStorageAccountKey -ResourceGroupName $storageAccount.ResourceGroupName -Name $storageAccount.Name)[0].Value
                           New-AzureRmOperationalInsightsStorageInsight -Workspace $workspace -Name $insightsName -StorageAccountResourceId $storageAccount.ResourceId -StorageAccountKey $key -Tables $validTables
                            Write-Host "New Azure Storage Insight Configured. `n"
                           }
                    }
             }
      }
      return
     }

$subscription = Select-Subscription
$subscriptionId = $subscription.SubscriptionId
$subscription = Select-AzureRmSubscription -SubscriptionId $subscriptionId
$workspace = Select-Workspace
$storageAccount = Select-StorageAccount
```

<span data-ttu-id="411cc-139">Hello Naplóelemzési munkaterület tooread hello Azure a beállítása után a tárfiókban lévő táblák jelentkezzen be Azure-portálon toohello.</span><span class="sxs-lookup"><span data-stu-id="411cc-139">After you've configured hello Log Analytics workspace tooread from hello Azure tables in your storage account, log in toohello Azure portal.</span></span> <span data-ttu-id="411cc-140">Válassza ki a Naplóelemzési munkaterület hello **összes erőforrás**.</span><span class="sxs-lookup"><span data-stu-id="411cc-140">Select hello Log Analytics workspace from **All Resources**.</span></span> <span data-ttu-id="411cc-141">tárolási fiók csatlakoztatva naplók toohello munkaterület hello száma jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="411cc-141">hello number of storage account logs connected toohello workspace is displayed.</span></span> <span data-ttu-id="411cc-142">Jelölje be hello **a tárolási fiók naplófájljai** csempére.</span><span class="sxs-lookup"><span data-stu-id="411cc-142">Select hello **Storage account logs** tile.</span></span> <span data-ttu-id="411cc-143">Tekintse át a tárolási fiók naplók tooverify, hogy a tárfiók-e a csatlakoztatott toohello megfelelő munkaterület hello listáját.</span><span class="sxs-lookup"><span data-stu-id="411cc-143">Review hello list of storage account logs tooverify that your storage account is connected toohello correct workspace.</span></span>

![A tárolási fiók naplófájljai](./media/log-analytics-service-fabric/sf1.png)

## <a name="enable-hello-service-fabric-solution"></a><span data-ttu-id="411cc-145">A Service Fabric-megoldás hello engedélyezése</span><span class="sxs-lookup"><span data-stu-id="411cc-145">Enable hello Service Fabric solution</span></span>
<span data-ttu-id="411cc-146">A következő parancsfájl tooadd hello megoldás tooyour Naplóelemzési munkaterület hello használata.</span><span class="sxs-lookup"><span data-stu-id="411cc-146">Use hello following script tooadd hello solution tooyour Log Analytics workspace.</span></span> <span data-ttu-id="411cc-147">A PowerShell használatával hello Azure-előfizetéssel társított hello Naplóelemzési munkaterület, amelyet szeretne tooenable hello Service Fabric megoldás hello parancsfájl futtatása.</span><span class="sxs-lookup"><span data-stu-id="411cc-147">Run hello script in PowerShell, using hello Azure subscription that is associated with hello Log Analytics workspace that you want tooenable hello Service Fabric solution in.</span></span>

```
function Select-Subscription {
      $subscription = ""
      $allSubscriptions = Get-AzureRmSubscription
      switch ($allSubscriptions.Count) {
             0 {Write-Error "No Operations Management Suite workspaces found"}
             1 {return $allSubscriptions}
        default {
            $uiPrompt = "Enter hello number corresponding toohello Azure subscription you would like toowork with.`n"
            $count = 1
            foreach ($subscription in $allSubscriptions) {
                $uiPrompt += "$count. " + $subscription.SubscriptionName + " (" + $subscription.SubscriptionId + ")`n"
                $count++
            }
            $answer = (Read-Host -Prompt $uiPrompt) - 1
            $subscription = $allSubscriptions[$answer]
             Write-Host $subscription.SubscriptionId
        }  
    }
    return $subscription
}

function Select-Workspace {
    $workspace = ""
    $allWorkspaces = Get-AzureRmOperationalInsightsWorkspace  
    switch ($allWorkspaces.Count) {
        0 {Write-Error "No Operations Management Suite workspaces found"}
        1 {return $allWorkspaces}
        default {
            $uiPrompt = "Enter hello number corresponding toohello workspace you want tooconfigure.`n"
            $count = 1
            foreach ($workspace in $allWorkspaces) {
                $uiPrompt += "$count. " + $workspace.Name + " (" + $workspace.CustomerId + ")`n"
                $count++
            }
            $answer = (Read-Host -Prompt $uiPrompt) - 1
            $workspace = $allWorkspaces[$answer]
                           Write-Host $workspace.WorkspaceName
        }  
    }
    return $workspace
}
$subscription = Select-Subscription
$subscriptionId = $subscription.Id
$subscription = Select-AzureRmSubscription -SubscriptionId $subscriptionId
$workspace = Select-Workspace
Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -IntelligencePackName "ServiceFabric" -Enabled $true
```

<span data-ttu-id="411cc-148">Miután engedélyezte a hello megoldás, hello Service Fabric csempe szerepel-e tooyour Naplóelemzési *áttekintése* lap.</span><span class="sxs-lookup"><span data-stu-id="411cc-148">After you enable hello solution, hello Service Fabric tile is added tooyour Log Analytics *Overview* page.</span></span> <span data-ttu-id="411cc-149">hello lapon jelentős problémák, például a runAsync hibákat és azokat az elmúlt 24 órában történt hello megszakításait nézete látható.</span><span class="sxs-lookup"><span data-stu-id="411cc-149">hello page shows a view of notable issues such as runAsync failures and cancellations that occurred in hello last 24 hours.</span></span>

![A Service Fabric csempe](./media/log-analytics-service-fabric/sf2.png)

### <a name="view-service-fabric-events"></a><span data-ttu-id="411cc-151">A Service Fabric-események megtekintése</span><span class="sxs-lookup"><span data-stu-id="411cc-151">View Service Fabric events</span></span>
<span data-ttu-id="411cc-152">Kattintson a hello **Service Fabric** csempe tooopen hello Service Fabric dashboard.</span><span class="sxs-lookup"><span data-stu-id="411cc-152">Click hello **Service Fabric** tile tooopen hello Service Fabric dashboard.</span></span> <span data-ttu-id="411cc-153">hello irányítópult hello oszlopok hello táblázatot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="411cc-153">hello dashboard includes hello columns in hello table that follows.</span></span> <span data-ttu-id="411cc-154">Egyes oszlopok hello felső 10 események száma a hozzá, hogy az oszlop feltételek hello megadott időtartomány sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="411cc-154">Each column lists hello top 10 events by count matching that column's criteria for hello specified time range.</span></span> <span data-ttu-id="411cc-155">Futtathatja a napló keresési kattintva hello teljes lista biztosító **láthatja az összes** hello jobb alsó oszlopok, vagy hello oszlop fejlécére kattint.</span><span class="sxs-lookup"><span data-stu-id="411cc-155">You can run a log search that provides hello entire list by clicking **See all** at hello right bottom of each column, or by clicking hello column header.</span></span>

| <span data-ttu-id="411cc-156">**A Service Fabric-esemény**</span><span class="sxs-lookup"><span data-stu-id="411cc-156">**Service Fabric event**</span></span> | <span data-ttu-id="411cc-157">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="411cc-157">**description**</span></span> |
| --- | --- |
| <span data-ttu-id="411cc-158">Jelentős problémák</span><span class="sxs-lookup"><span data-stu-id="411cc-158">Notable Issues</span></span> | <span data-ttu-id="411cc-159">Például RunAsyncFailures, RunAsynCancellations és csomópont időszakosan megszakadó problémákat jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="411cc-159">Displays issues including RunAsyncFailures, RunAsynCancellations, and Node Downs.</span></span> |
| <span data-ttu-id="411cc-160">A működési események</span><span class="sxs-lookup"><span data-stu-id="411cc-160">Operational Events</span></span> | <span data-ttu-id="411cc-161">Megjeleníti a figyelmet a jelentősebb működési eseményeit, beleértve az alkalmazásfrissítés és központi telepítések.</span><span class="sxs-lookup"><span data-stu-id="411cc-161">Displays notable operational events including application upgrade and deployments.</span></span> |
| <span data-ttu-id="411cc-162">Megbízható eseményei</span><span class="sxs-lookup"><span data-stu-id="411cc-162">Reliable Service Events</span></span> | <span data-ttu-id="411cc-163">Többek között a Runasyncinvocations figyelmet a jelentősebb megbízható szolgáltatás eseményeket jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="411cc-163">Displays notable reliable service events including  Runasyncinvocations.</span></span> |
| <span data-ttu-id="411cc-164">Szereplő események</span><span class="sxs-lookup"><span data-stu-id="411cc-164">Actor Events</span></span> | <span data-ttu-id="411cc-165">A micro-szolgáltatások által létrehozott figyelmet a jelentősebb szereplő eseményeket jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="411cc-165">Displays notable actor events generated by your micro-services.</span></span> <span data-ttu-id="411cc-166">Események közé tartozik egy aktormetódus, szereplő aktiválások és deactivations, és így tovább által okozott kivételeket.</span><span class="sxs-lookup"><span data-stu-id="411cc-166">Events include exceptions thrown by an actor method, actor activations and deactivations, and so on.</span></span> |
| <span data-ttu-id="411cc-167">Alkalmazás-események</span><span class="sxs-lookup"><span data-stu-id="411cc-167">Application Events</span></span> | <span data-ttu-id="411cc-168">Az alkalmazások által létrehozott összes egyéni ETW eseményeket jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="411cc-168">Displays all custom ETW events generated by your applications.</span></span> |

![A Service Fabric irányítópult](./media/log-analytics-service-fabric/sf3.png)

![A Service Fabric irányítópult](./media/log-analytics-service-fabric/sf4.png)

<span data-ttu-id="411cc-171">hello következő táblázatban adatgyűjtési módszerek és egyéb adatok gyűjtése hogyan Service Fabric részleteit:</span><span class="sxs-lookup"><span data-stu-id="411cc-171">hello following table shows data collection methods and other details about how data is collected for Service Fabric:</span></span>

| <span data-ttu-id="411cc-172">Platform</span><span class="sxs-lookup"><span data-stu-id="411cc-172">platform</span></span> | <span data-ttu-id="411cc-173">Közvetlen ügynök</span><span class="sxs-lookup"><span data-stu-id="411cc-173">Direct Agent</span></span> | <span data-ttu-id="411cc-174">Operations Manager-ügynök</span><span class="sxs-lookup"><span data-stu-id="411cc-174">Operations Manager agent</span></span> | <span data-ttu-id="411cc-175">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="411cc-175">Azure Storage</span></span> | <span data-ttu-id="411cc-176">Az Operations Manager szükséges?</span><span class="sxs-lookup"><span data-stu-id="411cc-176">Operations Manager required?</span></span> | <span data-ttu-id="411cc-177">Az Operations Manager ügynök adatait a felügyeleti csoport keresztül küldött</span><span class="sxs-lookup"><span data-stu-id="411cc-177">Operations Manager agent data sent via management group</span></span> | <span data-ttu-id="411cc-178">Gyűjtemény gyakorisága</span><span class="sxs-lookup"><span data-stu-id="411cc-178">collection frequency</span></span> |
| --- | --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="411cc-179">Windows</span><span class="sxs-lookup"><span data-stu-id="411cc-179">Windows</span></span> |  |  | <span data-ttu-id="411cc-180">&#8226;</span><span class="sxs-lookup"><span data-stu-id="411cc-180">&#8226;</span></span> |  |  |<span data-ttu-id="411cc-181">10 perc</span><span class="sxs-lookup"><span data-stu-id="411cc-181">10 minutes</span></span> |

> [!NOTE]
> <span data-ttu-id="411cc-182">Az események hello hatókörének módosítása **adatok alapján az elmúlt hét napban** hello irányítópult hello tetején.</span><span class="sxs-lookup"><span data-stu-id="411cc-182">Change hello scope of events with **Data based on last seven days** at hello top of hello dashboard.</span></span> <span data-ttu-id="411cc-183">Elmúlt hét napban, egy nap vagy hat órán belül hello generált események akkor is megjelenhet.</span><span class="sxs-lookup"><span data-stu-id="411cc-183">You can also show events generated within hello last seven days, one day, or six hours.</span></span> <span data-ttu-id="411cc-184">Vagy választhat **egyéni** toospecify egyéni dátumtartományt.</span><span class="sxs-lookup"><span data-stu-id="411cc-184">Or, you can select **Custom** toospecify a custom date range.</span></span>
>
>

## <a name="troubleshoot-your-service-fabric-and-log-analytics-configuration"></a><span data-ttu-id="411cc-185">A Service Fabric és Naplóelemzési konfiguráció hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="411cc-185">Troubleshoot your Service Fabric and Log Analytics configuration</span></span>
<span data-ttu-id="411cc-186">Ha kell tooverify a Naplóelemzési konfigurációs mert Ön nem tooview eseményadatok Naplóelemzési, használja a következő parancsfájl hello.</span><span class="sxs-lookup"><span data-stu-id="411cc-186">If you need tooverify your Log Analytics configuration because you are unable tooview event data in Log Analytics, use hello following script.</span></span> <span data-ttu-id="411cc-187">A következő műveletek hello segítségével végzi:</span><span class="sxs-lookup"><span data-stu-id="411cc-187">It performs hello following actions:</span></span>

1. <span data-ttu-id="411cc-188">Beolvassa a Service Fabric diagnosztikai konfigurációja</span><span class="sxs-lookup"><span data-stu-id="411cc-188">Reads your Service Fabric diagnostics configuration</span></span>
2. <span data-ttu-id="411cc-189">Hello táblákba írt adatok ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="411cc-189">Checks for data written into hello tables</span></span>
3. <span data-ttu-id="411cc-190">Ellenőrzi, hogy a Naplóelemzési konfigurált tooread hello táblából</span><span class="sxs-lookup"><span data-stu-id="411cc-190">Verifies that Log Analytics is configured tooread from hello tables</span></span>

```
<#
    Verify Service Fabric and Log Analytics configuration
    1. Read Service Fabric diagnostics configuration
    2. Check for data being written into hello tables
    3. Verify Log Analytics is configured tooread from hello tables

    Supported tables:
    WADServiceFabricReliableActorEventTable
    WADServiceFabricReliableServiceEventTable
    WADServiceFabricSystemEventTable
    WADETWEventTable

    Script will write a warning for every misconfiguration detected
    toosee items that are correctly configured set $VerbosePreference="Continue"
#>
Param
(
    [Parameter(Mandatory=$true,
    ValueFromPipeline=$true,
    Position=1)]
    [string]$workspaceName
)

$WADtables = @("WADServiceFabricReliableActorEventTable",
               "WADServiceFabricReliableServiceEventTable",
               "WADServiceFabricSystemEventTable",
               "WADETWEventTable"
               )

<#
    Check if OMS Log Analytics is configured tooindex service fabric events from hello specified table
#>

function Check-OMSLogAnalyticsConfiguration {
    param(
    [psobject]$workspace,
    [psobject]$storageAccount,
    [string]$id
    )

    $existingInsights = Get-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name

    if ($existingInsights)
    {
        $currentStorageAccountInsight = $existingInsights.Where({$_.StorageAccountResourceId -eq $storageAccount.ResourceId})

        if ("WADServiceFabric*EventTable" -in $currentStorageAccountInsight.Tables)
        {
            Write-Verbose ("OMS Log Analytics workspace " + $workspace.Name + " is configured tooindex service fabric actor, service and operational events from " + $storageAccount.Name)
        } else
        {
            Write-Warning ("OMS Log Analytics workspace " + $workspace.Name + " is not configured tooindex service fabric actor, service and operational events from " + $storageAccount.Name)
        }
        if ("WADETWEventTable" -in $currentStorageAccountInsight.Tables)
        {
            Write-Verbose ("OMS Log Analytics workspace " + $workspace.Name + " is configured tooindex service fabric application events from " + $storageAccount.Name)
        } else
        {
            Write-Warning ("OMS Log Analytics workspace " + $workspace.Name + " is not configured tooindex service fabric application events from " + $storageAccount.Name)
        }
    } else
    {
        Write-Warning ("OMS Log Analytics workspace " + $workspace.Name + "is not configured tooread service fabric events from " + $storageAccount.Name)
    }    
}

<#
    Check Azure table storage tooconfirm there is recent data written by Service Fabric
#>

function Check-TablesForData {
    param(
    [psobject]$storageAccount
    )

    $ctx = (Get-AzureRmStorageAccount -ResourceGroupName $storageAccount.ResourceGroupName -Name $storageAccount.ResourceName).Context

    $createdTables = Get-AzureStorageTable -Context $ctx

    $recently = Get-Date -Format s ((Get-Date).AddMinutes(-20).ToUniversalTime())
    $recently = $recently + "Z"

    foreach ($table in $WADtables)
    {
        if ($table -in $createdTables.Name)
        {
            $tbl = Get-AzureStorageTable -Name $table -Context $ctx
            $query = New-Object Microsoft.WindowsAzure.Storage.Table.TableQuery
            $list = New-Object System.Collections.Generic.List[string]
            $list.Add("RowKey")
            $list.Add("ProviderName")
            $list.Add("Timestamp")
            $query.FilterString = "Timestamp gt datetime'$recently'"
            $query.SelectColumns = $list
            $query.TakeCount = 20
            $entities = $tbl.CloudTable.ExecuteQuery($query)
            Write-Debug $entities
            if ($entities.Count -gt 0)
            {
                Write-Verbose ("Data was written too$table in " + $storageAccount.ResourceName + "after $recently")
            } else
            {
                Write-Warning ("No data after $recently is in  $table in " + $storageAccount.ResourceName)
            }
        } else
        {
            Write-Warning ("$table does not exist in storage account " + $storageAccount.ResourceName)
        }
    }
}

<#
    Check if ETW provider is configured toolog events toohello expected table storage
#>
function Check-ETWProviderLogging {
    param(
    [string]$id,
    [string]$provider,
    [string]$expectedTable,
    [string]$table
    )      
        Write-Debug ("ID: $id Provider: $provider ExpectedTable $expectedTable ActualTable $table")
        if ( ($table -eq $null) -or ($table -eq ""))
        {
            Write-Warning ("$id No configuration found for $provider. Configure Azure diagnostics toowrite too$expectedTable.")
        }
        elseif ( $table -ne $expectedTable )
        {
            Write-Warning ("$id $provider events are being written too$table instead of WAD$expectedTable. Events will not be collected by Log Analytics")
        }
        else
        {
            Write-Verbose "$id $provider events are being written tooWAD$expectedTable (Correct configuration.)"
        }
}

<#
    Check Azure Diagnostics Configuration for a Service Fabric cluster
#>
function Check-ServiceFabricScaleSetDiagnostics {
    param(
    [psobject]$scaleSetDiagnostics
    )

    $storageAccountsFound = @()
    Write-Verbose ("Checking " + $scaleSetDiagnostics)
    $sfReliableActorTable = $null
    $sfReliableServiceTable = $null
    $sfOperationalTable = $null
    Write-Debug $scaleSetDiagnostics
    $serviceFabricProviderList = ""
    $etwManifestProviderList = ""

    if ( $scaleSetDiagnostics.xmlCfg )
    {
        Write-Debug ("Found XMLcfg")
        $xmlCfg = [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($scaleSetDiagnostics.xmlCfg))
        Write-Debug $xmlCfg
        $etwProviders = Select-Xml -Content $xmlCfg -XPath "//EtwProviders"                
        $serviceFabricProviderList = $etwProviders.Node.EtwEventSourceProviderConfiguration
        $etwManifestProviderList = $etwProviders.Node.EtwManifestProviderConfiguration
    } elseif ($scaleSetDiagnostics.WadCfg )
    {
        Write-Debug ("Found WADcfg")
        Write-Debug $scaleSetDiagnostics.WadCfg
        $serviceFabricProviderList = $scaleSetDiagnostics.WadCfg.DiagnosticMonitorConfiguration.EtwProviders.EtwEventSourceProviderConfiguration
        $etwManifestProviderList = $scaleSetDiagnostics.WadCfg.DiagnosticMonitorConfiguration.EtwProviders.EtwManifestProviderConfiguration
    } else
    {
        Write-Error "Unable tooparse Azure Diagnostics setting for $id"
        Write-Warning ("$id does not have diagnostics enabled")
    }

    foreach ($provider in $serviceFabricProviderList)
    {
        Write-Debug ("Event Source Provider: " + $provider.Provider + " Destination: " + $provider.DefaultEvents.eventDestination)
        if ($provider.Provider -eq "Microsoft-ServiceFabric-Actors")
        {
            $sfReliableActorTable = $provider.DefaultEvents.eventDestination
        } elseif ($provider.Provider -eq "Microsoft-ServiceFabric-Services")
        {
            $sfReliableServiceTable = $provider.DefaultEvents.eventDestination
        } else
        {
            Check-ETWProviderLogging $id $provider.Provider "ETWEventTable" $provider.DefaultEvents.eventDestination
        }
    }
    foreach ($provider in $etwManifestProviderList)
    {
        Write-Debug ("Manifest Provider: " + $provider.Provider + " Destination: " + $provider.DefaultEvents.eventDestination)
        if ($provider.Provider -eq "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8")
        {
            $sfOperationalTable = $provider.DefaultEvents.eventDestination
        } else
        {
            Check-ETWProviderLogging $id $provider.Provider "ETWEventTable" $provider.DefaultEvents.eventDestination
        }
    }

    Check-ETWProviderLogging $id "Microsoft-ServiceFabric-Actors" "ServiceFabricReliableActorEventTable" $sfReliableActorTable
    Check-ETWProviderLogging $id "Microsoft-ServiceFabric-Services" "ServiceFabricReliableServiceEventTable" $sfReliableServiceTable
    Check-ETWProviderLogging $id "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8 (System events)" "ServiceFabricSystemEventTable" $sfOperationalTable

    Write-Verbose ("StorageAccount: " + $scaleSetDiagnostics.StorageAccount)

    $storageAccountsFound += ($scaleSetDiagnostics.StorageAccount)
    return ($storageAccountsFound)
}

# This script uses Get-AzureRmVMDiagnosticsExtension and needs a version where -Name is not a required parameter
Import-Module AzureRM.Compute -MinimumVersion 1.2.2

try
{
    Get-AzureRmContext
}
catch [System.Management.Automation.PSInvalidOperationException]
{
    Login-AzureRmAccount
}

$allResources = Get-AzureRmResource

$OMSworkspace = $allResources.Where({($_.ResourceType -eq "Microsoft.OperationalInsights/workspaces") -and ($_.ResourceName -eq $workspaceName)})

if ($OMSworkspace.Name -ne $workspaceName)
{
    Write-Error ("Unable toofind Log Analytics Workspace " + $workspaceName)
}

$serviceFabricClusters = $allResources.Where({$_.ResourceType -eq "Microsoft.ServiceFabric/clusters"})
$storageAccountList = @()
foreach($cluster in $serviceFabricClusters) {
    Write-Verbose ("Checking cluster: " + $cluster.Name)
    $scaleSet = ($allResources.Where({($_.ResourceType -eq "Microsoft.Compute/virtualMachineScaleSets") -and ($_.ResourceGroupName -eq $cluster.ResourceGroupName)}))

    foreach($set in $scaleSet) {
        $resource = Get-AzureRmResource -ResourceId $set.ResourceId
        $extensions = $resource.Properties.VirtualMachineProfile.ExtensionProfile.Extensions
        foreach($ext in $extensions) {
            if ($ext.Properties.Publisher -eq "Microsoft.Azure.Diagnostics" -and $ext.Properties.Type -eq "IaaSDiagnostics") {
                $storageAccountList += (Check-ServiceFabricScaleSetDiagnostics $ext.Properties.Settings)
            }
        }
    }
}

$storageAccountList = $storageAccountList | Sort-Object | Get-Unique
$storageAccountsToCheck = ($allResources.Where({($_.ResourceType -eq "Microsoft.Storage/storageAccounts") -and ($_.ResourceName -in $storageAccountList)}))

foreach($storageAccount in $storageAccountsToCheck)
{
    Check-TablesForData $storageAccount
    Check-OMSLogAnalyticsConfiguration $OMSworkspace $storageAccount
}
 ```


## <a name="next-steps"></a><span data-ttu-id="411cc-191">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="411cc-191">Next steps</span></span>
* <span data-ttu-id="411cc-192">Használjon [napló megkeresi a Naplóelemzési](log-analytics-log-searches.md) tooview részletes Service Fabric eseményadatok.</span><span class="sxs-lookup"><span data-stu-id="411cc-192">Use [Log Searches in Log Analytics](log-analytics-log-searches.md) tooview detailed Service Fabric event data.</span></span>
