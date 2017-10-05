---
title: "Azure Automation-feladat adatok továbbítására OMS szolgáltatáshoz |} Microsoft Docs"
description: "Ez a cikk bemutatja, hogyan küldhet feladat állapotát és a runbook feladat adatfolyamokat a Microsoft Operations Management Suite Log Analytics képes biztosítani a további betekintést és kezelése."
services: automation
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: tysonn
ms.assetid: c12724c6-01a9-4b55-80ae-d8b7b99bd436
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/02/2017
ms.author: magoedte
ms.openlocfilehash: 2c0ca7fc332963e5a5db3c20c400ed877ae0cc54
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="forward-job-status-and-job-streams-from-automation-to-log-analytics-oms"></a><span data-ttu-id="0c831-103">Továbbítsa feladat állapotát és a feladat adatfolyamok Automation való Naplóelemzés (OMS)</span><span class="sxs-lookup"><span data-stu-id="0c831-103">Forward job status and job streams from Automation to Log Analytics (OMS)</span></span>
<span data-ttu-id="0c831-104">Automatizálási küldhet runbook feladat állapotát és a feladat adatfolyamokat a Microsoft Operations Management Suite (OMS) Naplóelemzési munkaterületet.</span><span class="sxs-lookup"><span data-stu-id="0c831-104">Automation can send runbook job status and job streams to your Microsoft Operations Management Suite (OMS) Log Analytics workspace.</span></span>  <span data-ttu-id="0c831-105">Feladat naplózza, és a feladat adatfolyamok láthatók az Azure portálon, vagy a PowerShell használatával, az egyes feladatokat, és ez lehetővé teszi egyszerű vizsgálatok végrehajtását.</span><span class="sxs-lookup"><span data-stu-id="0c831-105">Job logs and job streams are visible in the Azure portal, or with PowerShell, for individual jobs and this allows you to perform simple investigations.</span></span> <span data-ttu-id="0c831-106">Most Log Analytics segítségével:</span><span class="sxs-lookup"><span data-stu-id="0c831-106">Now with Log Analytics you can:</span></span>

* <span data-ttu-id="0c831-107">Az automatizálási feladatok insight letöltése</span><span class="sxs-lookup"><span data-stu-id="0c831-107">Get insight on your Automation jobs</span></span>
* <span data-ttu-id="0c831-108">A runbook-feladat állapota (például felfüggesztett vagy sikertelen) alapuló riasztás vagy e-mail eseményindító</span><span class="sxs-lookup"><span data-stu-id="0c831-108">Trigger an email or alert based on your runbook job status (for example, failed or suspended)</span></span>
* <span data-ttu-id="0c831-109">Speciális lekérdezéseket írhat a feladat adatfolyamok között</span><span class="sxs-lookup"><span data-stu-id="0c831-109">Write advanced queries across your job streams</span></span>
* <span data-ttu-id="0c831-110">Feladatok összefüggéseket Automation-fiókok között</span><span class="sxs-lookup"><span data-stu-id="0c831-110">Correlate jobs across Automation accounts</span></span>
* <span data-ttu-id="0c831-111">A feladatelőzmények megjelenítheti az adott idő alatt</span><span class="sxs-lookup"><span data-stu-id="0c831-111">Visualize your job history over time</span></span>     

## <a name="prerequisites-and-deployment-considerations"></a><span data-ttu-id="0c831-112">Előfeltételek és központi telepítésével kapcsolatos megfontolások</span><span class="sxs-lookup"><span data-stu-id="0c831-112">Prerequisites and deployment considerations</span></span>
<span data-ttu-id="0c831-113">Az automatizálási naplók értesítésküldés Naplóelemzési, az alábbiak szükségesek:</span><span class="sxs-lookup"><span data-stu-id="0c831-113">To start sending your Automation logs to Log Analytics, you need:</span></span>

1. <span data-ttu-id="0c831-114">A November 2016 vagy újabb kiadása [Azure PowerShell](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/) (v2.3.0).</span><span class="sxs-lookup"><span data-stu-id="0c831-114">The November 2016 or later release of [Azure PowerShell](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/) (v2.3.0).</span></span>
2. <span data-ttu-id="0c831-115">A Naplóelemzési munkaterület.</span><span class="sxs-lookup"><span data-stu-id="0c831-115">A Log Analytics workspace.</span></span> <span data-ttu-id="0c831-116">További információkért lásd: [Ismerkedés a Naplóelemzési](../log-analytics/log-analytics-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="0c831-116">For more information, see [Get started with Log Analytics](../log-analytics/log-analytics-get-started.md).</span></span> 
3. <span data-ttu-id="0c831-117">Az erőforrás-azonosítója az Azure Automation-fiókhoz</span><span class="sxs-lookup"><span data-stu-id="0c831-117">The ResourceId for your Azure Automation account</span></span>

<span data-ttu-id="0c831-118">Az erőforrás-azonosítója az Azure Automation-fiók és a Naplóelemzési munkaterület megkereséséhez, futtassa a következő PowerShell:</span><span class="sxs-lookup"><span data-stu-id="0c831-118">To find the ResourceId for your Azure Automation account and Log Analytics workspace, run the following PowerShell:</span></span>

```powershell
# Find the ResourceId for the Automation Account
Find-AzureRmResource -ResourceType "Microsoft.Automation/automationAccounts"

# Find the ResourceId for the Log Analytics workspace
Find-AzureRmResource -ResourceType "Microsoft.OperationalInsights/workspaces"
```

<span data-ttu-id="0c831-119">Ha több Automation-fiókot, vagy a munkaterületek között, az előző parancs kimenetében található a *neve* meg kell konfigurálnia, és másolja a következő *ResourceId*.</span><span class="sxs-lookup"><span data-stu-id="0c831-119">If you have multiple Automation accounts, or workspaces, in the output of the preceding commands, find the *Name* you need to configure and copy the value for *ResourceId*.</span></span>

<span data-ttu-id="0c831-120">Ha meg szeretné tudni a *neve* az Automation-fiók, az Azure portálon válassza ki az Automation-fiók a a **Automation-fiók** panelhez, és válassza **összes beállítás**.</span><span class="sxs-lookup"><span data-stu-id="0c831-120">If you need to find the *Name* of your Automation account, in the Azure portal select your Automation account from the **Automation account** blade and select **All settings**.</span></span>  <span data-ttu-id="0c831-121">A **Minden beállítás** panel **Fiókbeállítások** részénél válassza a **Tulajdonságok** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="0c831-121">From the **All settings** blade, under **Account Settings** select **Properties**.</span></span>  <span data-ttu-id="0c831-122">A **Tulajdonságok** panelen megtalálja a keresett értékeket.</span><span class="sxs-lookup"><span data-stu-id="0c831-122">In the **Properties** blade, you can note these values.</span></span><br> <span data-ttu-id="0c831-123">![Automation-fiók tulajdonságai](media/automation-manage-send-joblogs-log-analytics/automation-account-properties.png).</span><span class="sxs-lookup"><span data-stu-id="0c831-123">![Automation Account properties](media/automation-manage-send-joblogs-log-analytics/automation-account-properties.png).</span></span>

## <a name="set-up-integration-with-log-analytics"></a><span data-ttu-id="0c831-124">Log Analytics-integráció beállítása</span><span class="sxs-lookup"><span data-stu-id="0c831-124">Set up integration with Log Analytics</span></span>
1. <span data-ttu-id="0c831-125">A számítógépen indítása **Windows PowerShell** a a **Start** képernyő.</span><span class="sxs-lookup"><span data-stu-id="0c831-125">On your computer, start **Windows PowerShell** from the **Start** screen.</span></span>  
2. <span data-ttu-id="0c831-126">Másolja és illessze be a következő PowerShell és az érték szerkesztése a `$workspaceId` és `$automationAccountId`.</span><span class="sxs-lookup"><span data-stu-id="0c831-126">Copy and paste the following PowerShell, and edit the value for the `$workspaceId` and `$automationAccountId`.</span></span>  <span data-ttu-id="0c831-127">Az a `-Environment` paraméter érvényes értékei a *AzureCloud* vagy *AzureUSGovernment* attól függően, hogy a felhő környezet használata.</span><span class="sxs-lookup"><span data-stu-id="0c831-127">For the `-Environment` parameter, valid values are *AzureCloud* or *AzureUSGovernment* depending on the cloud environment you are working in.</span></span>     

```powershell
[cmdletBinding()]
    Param
    (
        [Parameter(Mandatory=$True)]
        [ValidateSet("AzureCloud","AzureUSGovernment")]
        [string]$Environment="AzureCloud"
    )

#Check to see which cloud environment to sign into.
Switch ($Environment)
   {
       "AzureCloud" {Login-AzureRmAccount}
       "AzureUSGovernment" {Login-AzureRmAccount -EnvironmentName AzureUSGovernment} 
   }

# if you have one Log Analytics workspace you can use the following command to get the resource id of the workspace
$workspaceId = (Get-AzureRmOperationalInsightsWorkspace).ResourceId

$automationAccountId = "/SUBSCRIPTIONS/ec11ca60-1234-491e-5678-0ea07feae25c/RESOURCEGROUPS/DEMO/PROVIDERS/MICROSOFT.AUTOMATION/ACCOUNTS/DEMO" 

Set-AzureRmDiagnosticSetting -ResourceId $automationAccountId -WorkspaceId $workspaceId -Enabled $true

```

<span data-ttu-id="0c831-128">A parancsfájl futtatása után látni fogja az Log Analytics-rekordok új JobLogs vagy írása JobStreams 10 percen belül.</span><span class="sxs-lookup"><span data-stu-id="0c831-128">After running this script, you will see records in Log Analytics within 10 minutes of new JobLogs or JobStreams being written.</span></span>

<span data-ttu-id="0c831-129">A naplók megtekintéséhez futtassa a következő lekérdezés Naplóelemzési napló keresése:`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION"`</span><span class="sxs-lookup"><span data-stu-id="0c831-129">To see the logs, run the following query in Log Analytics log search: `Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION"`</span></span>

### <a name="verify-configuration"></a><span data-ttu-id="0c831-130">Konfiguráció ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="0c831-130">Verify configuration</span></span>
<span data-ttu-id="0c831-131">Annak ellenőrzéséhez, hogy az Automation-fiók naplókat küld a Naplóelemzési munkaterületet, ellenőrizze, hogy diagnosztika helyesen van-e beállítva a következő PowerShell-lel az Automation-fiókhoz:</span><span class="sxs-lookup"><span data-stu-id="0c831-131">To confirm that your Automation account is sending logs to your Log Analytics workspace, check that diagnostics are set correctly on the Automation account using the following PowerShell:</span></span>

```powershell
[cmdletBinding()]
    Param
    (
        [Parameter(Mandatory=$True)]
        [ValidateSet("AzureCloud","AzureUSGovernment")]
        [string]$Environment="AzureCloud"
    )

#Check to see which cloud environment to sign into.
Switch ($Environment)
   {
       "AzureCloud" {Login-AzureRmAccount}
       "AzureUSGovernment" {Login-AzureRmAccount -EnvironmentName AzureUSGovernment} 
   }
# if you have one Log Analytics workspace you can use the following command to get the resource id of the workspace
$workspaceId = (Get-AzureRmOperationalInsightsWorkspace).ResourceId

$automationAccountId = "/SUBSCRIPTIONS/ec11ca60-1234-491e-5678-0ea07feae25c/RESOURCEGROUPS/DEMO/PROVIDERS/MICROSOFT.AUTOMATION/ACCOUNTS/DEMO" 

Get-AzureRmDiagnosticSetting -ResourceId $automationAccountId
```

<span data-ttu-id="0c831-132">A kimenet győződjön meg arról, hogy:</span><span class="sxs-lookup"><span data-stu-id="0c831-132">In the output ensure that:</span></span>
+ <span data-ttu-id="0c831-133">A *naplók*, értéke *engedélyezve* van *igaz*</span><span class="sxs-lookup"><span data-stu-id="0c831-133">Under *Logs*, the value for *Enabled* is *True*</span></span>
+ <span data-ttu-id="0c831-134">Értékének *WorkspaceId* az erőforrás-azonosítója a Naplóelemzési munkaterület beállítása</span><span class="sxs-lookup"><span data-stu-id="0c831-134">The value of *WorkspaceId* is set to the ResourceId of your Log Analytics workspace</span></span>


## <a name="log-analytics-records"></a><span data-ttu-id="0c831-135">Log Analytics-rekordok</span><span class="sxs-lookup"><span data-stu-id="0c831-135">Log Analytics records</span></span>
<span data-ttu-id="0c831-136">Azure Automation diagnosztika kétféle típusú rekordok Naplóelemzési hoz létre, és címkével rendelkeznek, **típus = AzureDiagnostics**.</span><span class="sxs-lookup"><span data-stu-id="0c831-136">Diagnostics from Azure Automation creates two types of records in Log Analytics and are tagged as **Type=AzureDiagnostics**.</span></span>

### <a name="job-logs"></a><span data-ttu-id="0c831-137">Feladatnaplóit</span><span class="sxs-lookup"><span data-stu-id="0c831-137">Job Logs</span></span>
| <span data-ttu-id="0c831-138">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="0c831-138">Property</span></span> | <span data-ttu-id="0c831-139">Leírás</span><span class="sxs-lookup"><span data-stu-id="0c831-139">Description</span></span> |
| --- | --- |
| <span data-ttu-id="0c831-140">TimeGenerated</span><span class="sxs-lookup"><span data-stu-id="0c831-140">TimeGenerated</span></span> |<span data-ttu-id="0c831-141">A runbook-feladat végrehajtásának dátuma és időpontja.</span><span class="sxs-lookup"><span data-stu-id="0c831-141">Date and time when the runbook job executed.</span></span> |
| <span data-ttu-id="0c831-142">RunbookName_s</span><span class="sxs-lookup"><span data-stu-id="0c831-142">RunbookName_s</span></span> |<span data-ttu-id="0c831-143">A runbook neve.</span><span class="sxs-lookup"><span data-stu-id="0c831-143">The name of the runbook.</span></span> |
| <span data-ttu-id="0c831-144">Caller_s</span><span class="sxs-lookup"><span data-stu-id="0c831-144">Caller_s</span></span> |<span data-ttu-id="0c831-145">A művelet kezdeményezője.</span><span class="sxs-lookup"><span data-stu-id="0c831-145">Who initiated the operation.</span></span>  <span data-ttu-id="0c831-146">Lehetséges értékek: egy e-mail-cím vagy egy ütemezett feladatokat tartalmazó rendszer.</span><span class="sxs-lookup"><span data-stu-id="0c831-146">Possible values are either an email address or system for scheduled jobs.</span></span> |
| <span data-ttu-id="0c831-147">Tenant_g</span><span class="sxs-lookup"><span data-stu-id="0c831-147">Tenant_g</span></span> | <span data-ttu-id="0c831-148">A hívónak a bérlői azonosító GUID.</span><span class="sxs-lookup"><span data-stu-id="0c831-148">GUID that identifies the tenant for the Caller.</span></span> |
| <span data-ttu-id="0c831-149">JobId_g</span><span class="sxs-lookup"><span data-stu-id="0c831-149">JobId_g</span></span> |<span data-ttu-id="0c831-150">GUID, a runbook-feladat azonosítója.</span><span class="sxs-lookup"><span data-stu-id="0c831-150">GUID that is the Id of the runbook job.</span></span> |
| <span data-ttu-id="0c831-151">ResultType</span><span class="sxs-lookup"><span data-stu-id="0c831-151">ResultType</span></span> |<span data-ttu-id="0c831-152">A runbook-feladat állapota.</span><span class="sxs-lookup"><span data-stu-id="0c831-152">The status of the runbook job.</span></span>  <span data-ttu-id="0c831-153">Lehetséges értékek:</span><span class="sxs-lookup"><span data-stu-id="0c831-153">Possible values are:</span></span><br><span data-ttu-id="0c831-154">- Elindítva</span><span class="sxs-lookup"><span data-stu-id="0c831-154">- Started</span></span><br><span data-ttu-id="0c831-155">- Leállítva</span><span class="sxs-lookup"><span data-stu-id="0c831-155">- Stopped</span></span><br><span data-ttu-id="0c831-156">- Felfüggesztve</span><span class="sxs-lookup"><span data-stu-id="0c831-156">- Suspended</span></span><br><span data-ttu-id="0c831-157">- Sikertelen</span><span class="sxs-lookup"><span data-stu-id="0c831-157">- Failed</span></span><br><span data-ttu-id="0c831-158">-Befejeződött</span><span class="sxs-lookup"><span data-stu-id="0c831-158">- Completed</span></span> |
| <span data-ttu-id="0c831-159">Kategória</span><span class="sxs-lookup"><span data-stu-id="0c831-159">Category</span></span> | <span data-ttu-id="0c831-160">Az adattípus besorolása.</span><span class="sxs-lookup"><span data-stu-id="0c831-160">Classification of the type of data.</span></span>  <span data-ttu-id="0c831-161">Az Automation esetében az érték JobLogs.</span><span class="sxs-lookup"><span data-stu-id="0c831-161">For Automation, the value is JobLogs.</span></span> |
| <span data-ttu-id="0c831-162">OperationName</span><span class="sxs-lookup"><span data-stu-id="0c831-162">OperationName</span></span> | <span data-ttu-id="0c831-163">Meghatározza az Azure-ban végrehajtott művelet típusát.</span><span class="sxs-lookup"><span data-stu-id="0c831-163">Specifies the type of operation performed in Azure.</span></span>  <span data-ttu-id="0c831-164">Az automatizáláshoz értéke feladat.</span><span class="sxs-lookup"><span data-stu-id="0c831-164">For Automation, the value is Job.</span></span> |
| <span data-ttu-id="0c831-165">Erőforrás</span><span class="sxs-lookup"><span data-stu-id="0c831-165">Resource</span></span> | <span data-ttu-id="0c831-166">Az Automation-fiók neve</span><span class="sxs-lookup"><span data-stu-id="0c831-166">Name of the Automation account</span></span> |
| <span data-ttu-id="0c831-167">SourceSystem</span><span class="sxs-lookup"><span data-stu-id="0c831-167">SourceSystem</span></span> | <span data-ttu-id="0c831-168">Hogyan Naplóelemzési gyűjti az adatokat.</span><span class="sxs-lookup"><span data-stu-id="0c831-168">How Log Analytics collected the data.</span></span> <span data-ttu-id="0c831-169">Mindig *Azure* az Azure diagnostics.</span><span class="sxs-lookup"><span data-stu-id="0c831-169">Always *Azure* for Azure diagnostics.</span></span> |
| <span data-ttu-id="0c831-170">ResultDescription</span><span class="sxs-lookup"><span data-stu-id="0c831-170">ResultDescription</span></span> |<span data-ttu-id="0c831-171">Ismerteti a runbook-feladat eredményállapotát.</span><span class="sxs-lookup"><span data-stu-id="0c831-171">Describes the runbook job result state.</span></span>  <span data-ttu-id="0c831-172">Lehetséges értékek:</span><span class="sxs-lookup"><span data-stu-id="0c831-172">Possible values are:</span></span><br><span data-ttu-id="0c831-173">- A feladat elindult</span><span class="sxs-lookup"><span data-stu-id="0c831-173">- Job is started</span></span><br><span data-ttu-id="0c831-174">- A feladat nem sikerült</span><span class="sxs-lookup"><span data-stu-id="0c831-174">- Job Failed</span></span><br><span data-ttu-id="0c831-175">- A feladat befejeződött</span><span class="sxs-lookup"><span data-stu-id="0c831-175">- Job Completed</span></span> |
| <span data-ttu-id="0c831-176">CorrelationId</span><span class="sxs-lookup"><span data-stu-id="0c831-176">CorrelationId</span></span> |<span data-ttu-id="0c831-177">GUID, a runbook-feladat korrelációs azonosítója.</span><span class="sxs-lookup"><span data-stu-id="0c831-177">GUID that is the Correlation Id of the runbook job.</span></span> |
| <span data-ttu-id="0c831-178">ResourceId</span><span class="sxs-lookup"><span data-stu-id="0c831-178">ResourceId</span></span> |<span data-ttu-id="0c831-179">A runbook Azure Automation szolgáltatásbeli fiók erőforrás azonosítóját adja meg.</span><span class="sxs-lookup"><span data-stu-id="0c831-179">Specifies the Azure Automation account resource id of the runbook.</span></span> |
| <span data-ttu-id="0c831-180">SubscriptionId</span><span class="sxs-lookup"><span data-stu-id="0c831-180">SubscriptionId</span></span> | <span data-ttu-id="0c831-181">Az Azure-előfizetés azonosítója (GUID) az Automation-fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="0c831-181">The Azure subscription Id (GUID) for the Automation account.</span></span> |
| <span data-ttu-id="0c831-182">ResourceGroup</span><span class="sxs-lookup"><span data-stu-id="0c831-182">ResourceGroup</span></span> | <span data-ttu-id="0c831-183">Az erőforráscsoport neve az Automation-fiók.</span><span class="sxs-lookup"><span data-stu-id="0c831-183">Name of the resource group for the Automation account.</span></span> |
| <span data-ttu-id="0c831-184">ResourceProvider</span><span class="sxs-lookup"><span data-stu-id="0c831-184">ResourceProvider</span></span> | <span data-ttu-id="0c831-185">MICROSOFT. AUTOMATIZÁLÁS</span><span class="sxs-lookup"><span data-stu-id="0c831-185">MICROSOFT.AUTOMATION</span></span> |
| <span data-ttu-id="0c831-186">ResourceType</span><span class="sxs-lookup"><span data-stu-id="0c831-186">ResourceType</span></span> | <span data-ttu-id="0c831-187">AUTOMATIONACCOUNTS</span><span class="sxs-lookup"><span data-stu-id="0c831-187">AUTOMATIONACCOUNTS</span></span> |


### <a name="job-streams"></a><span data-ttu-id="0c831-188">Feladat adatfolyamok</span><span class="sxs-lookup"><span data-stu-id="0c831-188">Job Streams</span></span>
| <span data-ttu-id="0c831-189">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="0c831-189">Property</span></span> | <span data-ttu-id="0c831-190">Leírás</span><span class="sxs-lookup"><span data-stu-id="0c831-190">Description</span></span> |
| --- | --- |
| <span data-ttu-id="0c831-191">TimeGenerated</span><span class="sxs-lookup"><span data-stu-id="0c831-191">TimeGenerated</span></span> |<span data-ttu-id="0c831-192">A runbook-feladat végrehajtásának dátuma és időpontja.</span><span class="sxs-lookup"><span data-stu-id="0c831-192">Date and time when the runbook job executed.</span></span> |
| <span data-ttu-id="0c831-193">RunbookName_s</span><span class="sxs-lookup"><span data-stu-id="0c831-193">RunbookName_s</span></span> |<span data-ttu-id="0c831-194">A runbook neve.</span><span class="sxs-lookup"><span data-stu-id="0c831-194">The name of the runbook.</span></span> |
| <span data-ttu-id="0c831-195">Caller_s</span><span class="sxs-lookup"><span data-stu-id="0c831-195">Caller_s</span></span> |<span data-ttu-id="0c831-196">A művelet kezdeményezője.</span><span class="sxs-lookup"><span data-stu-id="0c831-196">Who initiated the operation.</span></span>  <span data-ttu-id="0c831-197">Lehetséges értékek: egy e-mail-cím vagy egy ütemezett feladatokat tartalmazó rendszer.</span><span class="sxs-lookup"><span data-stu-id="0c831-197">Possible values are either an email address or system for scheduled jobs.</span></span> |
| <span data-ttu-id="0c831-198">StreamType_s</span><span class="sxs-lookup"><span data-stu-id="0c831-198">StreamType_s</span></span> |<span data-ttu-id="0c831-199">A feladatstream típusa.</span><span class="sxs-lookup"><span data-stu-id="0c831-199">The type of job stream.</span></span> <span data-ttu-id="0c831-200">Lehetséges értékek:</span><span class="sxs-lookup"><span data-stu-id="0c831-200">Possible values are:</span></span><br><span data-ttu-id="0c831-201">- Folyamatban</span><span class="sxs-lookup"><span data-stu-id="0c831-201">-Progress</span></span><br><span data-ttu-id="0c831-202">- Kimenet</span><span class="sxs-lookup"><span data-stu-id="0c831-202">- Output</span></span><br><span data-ttu-id="0c831-203">- Figyelmeztetés</span><span class="sxs-lookup"><span data-stu-id="0c831-203">- Warning</span></span><br><span data-ttu-id="0c831-204">- Hiba</span><span class="sxs-lookup"><span data-stu-id="0c831-204">- Error</span></span><br><span data-ttu-id="0c831-205">- Hibakeresés</span><span class="sxs-lookup"><span data-stu-id="0c831-205">- Debug</span></span><br><span data-ttu-id="0c831-206">- Részletes</span><span class="sxs-lookup"><span data-stu-id="0c831-206">- Verbose</span></span> |
| <span data-ttu-id="0c831-207">Tenant_g</span><span class="sxs-lookup"><span data-stu-id="0c831-207">Tenant_g</span></span> | <span data-ttu-id="0c831-208">A hívónak a bérlői azonosító GUID.</span><span class="sxs-lookup"><span data-stu-id="0c831-208">GUID that identifies the tenant for the Caller.</span></span> |
| <span data-ttu-id="0c831-209">JobId_g</span><span class="sxs-lookup"><span data-stu-id="0c831-209">JobId_g</span></span> |<span data-ttu-id="0c831-210">GUID, a runbook-feladat azonosítója.</span><span class="sxs-lookup"><span data-stu-id="0c831-210">GUID that is the Id of the runbook job.</span></span> |
| <span data-ttu-id="0c831-211">ResultType</span><span class="sxs-lookup"><span data-stu-id="0c831-211">ResultType</span></span> |<span data-ttu-id="0c831-212">A runbook-feladat állapota.</span><span class="sxs-lookup"><span data-stu-id="0c831-212">The status of the runbook job.</span></span>  <span data-ttu-id="0c831-213">Lehetséges értékek:</span><span class="sxs-lookup"><span data-stu-id="0c831-213">Possible values are:</span></span><br><span data-ttu-id="0c831-214">-Folyamatban</span><span class="sxs-lookup"><span data-stu-id="0c831-214">- In Progress</span></span> |
| <span data-ttu-id="0c831-215">Kategória</span><span class="sxs-lookup"><span data-stu-id="0c831-215">Category</span></span> | <span data-ttu-id="0c831-216">Az adattípus besorolása.</span><span class="sxs-lookup"><span data-stu-id="0c831-216">Classification of the type of data.</span></span>  <span data-ttu-id="0c831-217">Az Automation esetében az érték JobStreams.</span><span class="sxs-lookup"><span data-stu-id="0c831-217">For Automation, the value is JobStreams.</span></span> |
| <span data-ttu-id="0c831-218">OperationName</span><span class="sxs-lookup"><span data-stu-id="0c831-218">OperationName</span></span> | <span data-ttu-id="0c831-219">Meghatározza az Azure-ban végrehajtott művelet típusát.</span><span class="sxs-lookup"><span data-stu-id="0c831-219">Specifies the type of operation performed in Azure.</span></span>  <span data-ttu-id="0c831-220">Az automatizáláshoz értéke feladat.</span><span class="sxs-lookup"><span data-stu-id="0c831-220">For Automation, the value is Job.</span></span> |
| <span data-ttu-id="0c831-221">Erőforrás</span><span class="sxs-lookup"><span data-stu-id="0c831-221">Resource</span></span> | <span data-ttu-id="0c831-222">Az Automation-fiók neve</span><span class="sxs-lookup"><span data-stu-id="0c831-222">Name of the Automation account</span></span> |
| <span data-ttu-id="0c831-223">SourceSystem</span><span class="sxs-lookup"><span data-stu-id="0c831-223">SourceSystem</span></span> | <span data-ttu-id="0c831-224">Hogyan Naplóelemzési gyűjti az adatokat.</span><span class="sxs-lookup"><span data-stu-id="0c831-224">How Log Analytics collected the data.</span></span> <span data-ttu-id="0c831-225">Mindig *Azure* az Azure diagnostics.</span><span class="sxs-lookup"><span data-stu-id="0c831-225">Always *Azure* for Azure diagnostics.</span></span> |
| <span data-ttu-id="0c831-226">ResultDescription</span><span class="sxs-lookup"><span data-stu-id="0c831-226">ResultDescription</span></span> |<span data-ttu-id="0c831-227">A runbook kimeneti streamjét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="0c831-227">Includes the output stream from the runbook.</span></span> |
| <span data-ttu-id="0c831-228">CorrelationId</span><span class="sxs-lookup"><span data-stu-id="0c831-228">CorrelationId</span></span> |<span data-ttu-id="0c831-229">GUID, a runbook-feladat korrelációs azonosítója.</span><span class="sxs-lookup"><span data-stu-id="0c831-229">GUID that is the Correlation Id of the runbook job.</span></span> |
| <span data-ttu-id="0c831-230">ResourceId</span><span class="sxs-lookup"><span data-stu-id="0c831-230">ResourceId</span></span> |<span data-ttu-id="0c831-231">A runbook Azure Automation szolgáltatásbeli fiók erőforrás azonosítóját adja meg.</span><span class="sxs-lookup"><span data-stu-id="0c831-231">Specifies the Azure Automation account resource id of the runbook.</span></span> |
| <span data-ttu-id="0c831-232">SubscriptionId</span><span class="sxs-lookup"><span data-stu-id="0c831-232">SubscriptionId</span></span> | <span data-ttu-id="0c831-233">Az Azure-előfizetés azonosítója (GUID) az Automation-fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="0c831-233">The Azure subscription Id (GUID) for the Automation account.</span></span> |
| <span data-ttu-id="0c831-234">ResourceGroup</span><span class="sxs-lookup"><span data-stu-id="0c831-234">ResourceGroup</span></span> | <span data-ttu-id="0c831-235">Az erőforráscsoport neve az Automation-fiók.</span><span class="sxs-lookup"><span data-stu-id="0c831-235">Name of the resource group for the Automation account.</span></span> |
| <span data-ttu-id="0c831-236">ResourceProvider</span><span class="sxs-lookup"><span data-stu-id="0c831-236">ResourceProvider</span></span> | <span data-ttu-id="0c831-237">MICROSOFT. AUTOMATIZÁLÁS</span><span class="sxs-lookup"><span data-stu-id="0c831-237">MICROSOFT.AUTOMATION</span></span> |
| <span data-ttu-id="0c831-238">ResourceType</span><span class="sxs-lookup"><span data-stu-id="0c831-238">ResourceType</span></span> | <span data-ttu-id="0c831-239">AUTOMATIONACCOUNTS</span><span class="sxs-lookup"><span data-stu-id="0c831-239">AUTOMATIONACCOUNTS</span></span> |

## <a name="viewing-automation-logs-in-log-analytics"></a><span data-ttu-id="0c831-240">A Naplóelemzési bejelentkezik Automation megtekintése</span><span class="sxs-lookup"><span data-stu-id="0c831-240">Viewing Automation Logs in Log Analytics</span></span>
<span data-ttu-id="0c831-241">Most, hogy használatba vette az Automation-feladat naplók küldése Naplóelemzési, nézzük meg, mi mindent, ezek a naplók Naplóelemzési belül.</span><span class="sxs-lookup"><span data-stu-id="0c831-241">Now that you have started sending your Automation job logs to Log Analytics, let’s see what you can do with these logs inside Log Analytics.</span></span>

<span data-ttu-id="0c831-242">A naplók megtekintéséhez futtassa a következő lekérdezést:`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION"`</span><span class="sxs-lookup"><span data-stu-id="0c831-242">To see the logs, run the following query: `Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION"`</span></span>

### <a name="send-an-email-when-a-runbook-job-fails-or-suspends"></a><span data-ttu-id="0c831-243">E-mailt küld, ha egy runbook-feladat sikertelen lesz, vagy felfüggesztése</span><span class="sxs-lookup"><span data-stu-id="0c831-243">Send an email when a runbook job fails or suspends</span></span>
<span data-ttu-id="0c831-244">Egy felső ügyfelünk kéri van arra, hogy a szöveg vagy egy e-mailt küldeni, ha probléma merül fel a runbook-feladatok.</span><span class="sxs-lookup"><span data-stu-id="0c831-244">One of our top customer asks is for the ability to send an email or a text when something goes wrong with a runbook job.</span></span>   

<span data-ttu-id="0c831-245">A riasztási szabályt létrehozni, akkor először hozzon létre egy napló keressen rá a runbook feladat azt jelzi, hogy a riasztás kell meghívnia.</span><span class="sxs-lookup"><span data-stu-id="0c831-245">To create an alert rule, you start by creating a log search for the runbook job records that should invoke the alert.</span></span>  <span data-ttu-id="0c831-246">Kattintson a **riasztás** gombra kattintva hozza létre és konfigurálja a riasztási szabályt.</span><span class="sxs-lookup"><span data-stu-id="0c831-246">Click the **Alert** button to create and configure the alert rule.</span></span>

1. <span data-ttu-id="0c831-247">A napló elemzés áttekintése lapon kattintson **naplófájl-keresési**.</span><span class="sxs-lookup"><span data-stu-id="0c831-247">From the Log Analytics Overview page, click **Log Search**.</span></span>
2. <span data-ttu-id="0c831-248">A riasztás napló keresési lekérdezés létrehozásához írja be a következő keresést a lekérdezés mezőbe: `Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobLogs (ResultType=Failed OR ResultType=Suspended)` szerint is csoportosíthatók a RunbookName által használatával:`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobLogs (ResultType=Failed OR ResultType=Suspended) | measure Count() by RunbookName_s`</span><span class="sxs-lookup"><span data-stu-id="0c831-248">Create a log search query for your alert by typing the following search into the query field:  `Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobLogs (ResultType=Failed OR ResultType=Suspended)`  You can also group by the RunbookName by using: `Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobLogs (ResultType=Failed OR ResultType=Suspended) | measure Count() by RunbookName_s`</span></span>   

   <span data-ttu-id="0c831-249">Ha állított be naplók egynél több Automation-fiók vagy előfizetés a munkaterületet, csoportosíthatók a a riasztások előfizetés és az Automation-fiók.</span><span class="sxs-lookup"><span data-stu-id="0c831-249">If you have set up logs from more than one Automation account or subscription to your workspace, you can group your alerts by subscription and Automation account.</span></span>  <span data-ttu-id="0c831-250">Automation-fiók nevét a JobLogs keresése erőforrás mezője származtatható.</span><span class="sxs-lookup"><span data-stu-id="0c831-250">Automation account name can be derived from the Resource field in the search of JobLogs.</span></span>  
3. <span data-ttu-id="0c831-251">Lehetőségre a **riasztási szabály hozzáadása** kattintson **riasztási** az oldal tetején.</span><span class="sxs-lookup"><span data-stu-id="0c831-251">To open the **Add Alert Rule** screen, click **Alert** at the top of the page.</span></span> <span data-ttu-id="0c831-252">További beállítások konfigurálása a riasztás további információkért lásd: [Naplóelemzési riasztások](../log-analytics/log-analytics-alerts.md#alert-rules).</span><span class="sxs-lookup"><span data-stu-id="0c831-252">For further details on the options to configure the alert, see [Alerts in Log Analytics](../log-analytics/log-analytics-alerts.md#alert-rules).</span></span>

### <a name="find-all-jobs-that-have-completed-with-errors"></a><span data-ttu-id="0c831-253">Található összes feladatot, amely hibákkal fejeződött be</span><span class="sxs-lookup"><span data-stu-id="0c831-253">Find all jobs that have completed with errors</span></span>
<span data-ttu-id="0c831-254">Mellett hibák riasztást küld, ha a runbook-feladatok nem okozó hibát tartalmaz található.</span><span class="sxs-lookup"><span data-stu-id="0c831-254">In addition to alerting on failures, you can find when a runbook job has a non-terminating error.</span></span> <span data-ttu-id="0c831-255">Ezekben az esetekben PowerShell egy hibafolyam eredményez, de a nem okozó hibákat nem indítják el a feladat felfüggesztése, vagy sikertelen.</span><span class="sxs-lookup"><span data-stu-id="0c831-255">In these cases PowerShell produces an error stream, but the non-terminating errors do not cause your job to suspend or fail.</span></span>    

1. <span data-ttu-id="0c831-256">A Naplóelemzési munkaterületet, kattintson **naplófájl-keresési**.</span><span class="sxs-lookup"><span data-stu-id="0c831-256">In your Log Analytics workspace, click **Log Search**.</span></span>
2. <span data-ttu-id="0c831-257">A lekérdezés mezőbe írja be a `Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobStreams StreamType_s=Error | measure count() by JobId_g` majd **keresési**.</span><span class="sxs-lookup"><span data-stu-id="0c831-257">In the query field, type `Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobStreams StreamType_s=Error | measure count() by JobId_g` and then click **Search**.</span></span>

### <a name="view-job-streams-for-a-job"></a><span data-ttu-id="0c831-258">Nézet feladat adatfolyamok feladat</span><span class="sxs-lookup"><span data-stu-id="0c831-258">View job streams for a job</span></span>
<span data-ttu-id="0c831-259">Ha egy feladat hibakeresés is érdemes lehet megismerhetők a feladat adatfolyamokat.</span><span class="sxs-lookup"><span data-stu-id="0c831-259">When you are debugging a job, you may also want to look into the job streams.</span></span>  <span data-ttu-id="0c831-260">A következő lekérdezés a GUID 2ebd22ea-e05e-4eb9 – 9d 76-d73cbd4356e0 egyetlen feladat az adatfolyamokat jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="0c831-260">The following query shows all the streams for a single job with GUID 2ebd22ea-e05e-4eb9-9d76-d73cbd4356e0:</span></span>   

`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobStreams JobId_g="2ebd22ea-e05e-4eb9-9d76-d73cbd4356e0" | sort TimeGenerated | select ResultDescription`

### <a name="view-historical-job-status"></a><span data-ttu-id="0c831-261">Korábbi feladat állapotának megtekintése</span><span class="sxs-lookup"><span data-stu-id="0c831-261">View historical job status</span></span>
<span data-ttu-id="0c831-262">Végül érdemes lehet a feladatelőzményekben megjelenítheti az adott idő alatt.</span><span class="sxs-lookup"><span data-stu-id="0c831-262">Finally, you may want to visualize your job history over time.</span></span>  <span data-ttu-id="0c831-263">Ez a lekérdezés segítségével keresse meg a feladatok állapotának adott idő alatt.</span><span class="sxs-lookup"><span data-stu-id="0c831-263">You can use this query to search for the status of your jobs over time.</span></span>

`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobLogs NOT(ResultType="started") | measure Count() by ResultType interval 1hour`  
<br> <span data-ttu-id="0c831-264">![OMS korábbi feladat állapota diagram](media/automation-manage-send-joblogs-log-analytics/historical-job-status-chart.png)</span><span class="sxs-lookup"><span data-stu-id="0c831-264">![OMS Historical Job Status Chart](media/automation-manage-send-joblogs-log-analytics/historical-job-status-chart.png)</span></span><br>

## <a name="summary"></a><span data-ttu-id="0c831-265">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="0c831-265">Summary</span></span>
<span data-ttu-id="0c831-266">Az Automation feladat állapotát és az adatfolyam adatokat küld a Naplóelemzési, által az automatizálási feladatok állapotának jobb betekintést kaphat:</span><span class="sxs-lookup"><span data-stu-id="0c831-266">By sending your Automation job status and stream data to Log Analytics, you can get better insight into the status of your Automation jobs by:</span></span>
+ <span data-ttu-id="0c831-267">Értesítést küldenek, ha probléma van a riasztások beállítása</span><span class="sxs-lookup"><span data-stu-id="0c831-267">Setting up alerts to notify you when there is an issue</span></span>
+ <span data-ttu-id="0c831-268">Egyéni nézetei és a keresési lekérdezések segítségével a runbook eredményeinek képi megjelenítése, runbook-feladat állapotát, és egyéb kapcsolódó fő mutatók vagy metrikákat.</span><span class="sxs-lookup"><span data-stu-id="0c831-268">Using custom views and search queries to visualize your runbook results, runbook job status, and other related key indicators or metrics.</span></span>  

<span data-ttu-id="0c831-269">A Naplóelemzési az Automation-feladat működési áttekinthetősége biztosít, és segít a cím incidensek gyorsabb.</span><span class="sxs-lookup"><span data-stu-id="0c831-269">Log Analytics provides greater operational visibility to your Automation jobs and can help address incidents quicker.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="0c831-270">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0c831-270">Next steps</span></span>
* <span data-ttu-id="0c831-271">A különböző keresési lekérdezések összeállításával és az Automation-feladatnaplók Log Analytics-szel történő megtekintésével kapcsolatos további tudnivalókat a [Naplókeresések a Log Analytics-ben](../log-analytics/log-analytics-log-searches.md) című rész tartalmazza</span><span class="sxs-lookup"><span data-stu-id="0c831-271">To learn more about how to construct different search queries and review the Automation job logs with Log Analytics, see [Log searches in Log Analytics](../log-analytics/log-analytics-log-searches.md)</span></span>
* <span data-ttu-id="0c831-272">Szeretné megtudni, hogyan hozhat létre és runbook-kimenet és a hiba üzeneteket beolvasni, lásd: [Runbook kimenet és üzenetek](automation-runbook-output-and-messages.md)</span><span class="sxs-lookup"><span data-stu-id="0c831-272">To understand how to create and retrieve output and error messages from runbooks, see [Runbook output and messages](automation-runbook-output-and-messages.md)</span></span>
* <span data-ttu-id="0c831-273">A runbook végrehajtásával, a runbook-feladatok figyelésével, illetve az egyéb technikai részletekkel kapcsolatos további tudnivalókat a [Runbook-feladatok nyomon követése](automation-runbook-execution.md) című rész tartalmazza</span><span class="sxs-lookup"><span data-stu-id="0c831-273">To learn more about runbook execution, how to monitor runbook jobs, and other technical details, see [Track a runbook job](automation-runbook-execution.md)</span></span>
* <span data-ttu-id="0c831-274">Az OMS Log Analytics használatával és adatgyűjtési forrásokkal kapcsolatos további tudnivalókat lásd: az [Azure-tárfiókbeli adatok Log Analytics-ben történő gyűjtésének az áttekintése](../log-analytics/log-analytics-azure-storage.md)</span><span class="sxs-lookup"><span data-stu-id="0c831-274">To learn more about OMS Log Analytics and data collection sources, see [Collecting Azure storage data in Log Analytics overview](../log-analytics/log-analytics-azure-storage.md)</span></span>
