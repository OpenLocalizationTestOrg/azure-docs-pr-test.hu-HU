---
title: "Azure diagnosztikai naplók aaaArchive |} Microsoft Docs"
description: "Ismerje meg, hogyan tooarchive az Azure diagnosztikai naplók a hosszú távú megőrzési tárfiókokban."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 3a55c73f-2ef3-45f3-8956-bcf9c0cb7e05
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/21/2017
ms.author: johnkem
ms.openlocfilehash: bc9edbd3a649023a728b7fe77130dba2b6e6370d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="archive-azure-diagnostic-logs"></a><span data-ttu-id="5c625-103">Az Azure diagnosztikai naplók archiválása</span><span class="sxs-lookup"><span data-stu-id="5c625-103">Archive Azure Diagnostic Logs</span></span>
<span data-ttu-id="5c625-104">Ebben a cikkben megmutatjuk, hogyan használhatja a hello Azure-portálon PowerShell-parancsmagok, CLI-t, vagy REST-API tooarchive a [Azure diagnosztikai naplók](monitoring-overview-of-diagnostic-logs.md) tárfiókokban.</span><span class="sxs-lookup"><span data-stu-id="5c625-104">In this article, we show how you can use hello Azure portal, PowerShell Cmdlets, CLI, or REST API tooarchive your [Azure diagnostic logs](monitoring-overview-of-diagnostic-logs.md) in a storage account.</span></span> <span data-ttu-id="5c625-105">Ez a beállítás akkor hasznos, ha azt szeretné, hogy a diagnosztikai naplók naplózási, statikus elemzési és biztonsági mentés olyan opcionális megőrzési házirend tooretain.</span><span class="sxs-lookup"><span data-stu-id="5c625-105">This option is useful if you would like tooretain your diagnostic logs with an optional retention policy for audit, static analysis, or backup.</span></span> <span data-ttu-id="5c625-106">hello storage-fiók nem rendelkezik toobe hello naplók kibocsátó mindaddig, amíg hello beállítás konfiguráló hello felhasználó rendelkezik-e megfelelő hozzáférési tooboth előfizetéseket a Szerepalapú hello erőforrás ugyanahhoz az előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="5c625-106">hello storage account does not have toobe in hello same subscription as hello resource emitting logs as long as hello user who configures hello setting has appropriate RBAC access tooboth subscriptions.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5c625-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="5c625-107">Prerequisites</span></span>
<span data-ttu-id="5c625-108">Kezdés előtt kell túl[hozzon létre egy tárfiókot](../storage/storage-create-storage-account.md) toowhich úgy archiválhatók a diagnosztikai naplókat.</span><span class="sxs-lookup"><span data-stu-id="5c625-108">Before you begin, you need too[create a storage account](../storage/storage-create-storage-account.md) toowhich you can archive your diagnostic logs.</span></span> <span data-ttu-id="5c625-109">Erősen ajánlott, hogy nem használja a benne tárolt, így jobban szabályozhatja a hozzáférést toomonitoring adatok más, nem figyelési adatokat tartalmazó meglévő tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="5c625-109">We highly recommend that you do not use an existing storage account that has other, non-monitoring data stored in it so that you can better control access toomonitoring data.</span></span> <span data-ttu-id="5c625-110">Azonban is archiválása a tevékenységnapló és diagnosztikai metrikák tooa tárfiókot, ha azt is ellenőrizze logika toouse, hogy a tárfiók a diagnosztikai naplók, valamint tookeep összes figyelési adatok egy központi helyen.</span><span class="sxs-lookup"><span data-stu-id="5c625-110">However, if you are also archiving your Activity Log and diagnostic metrics tooa storage account, it may make sense toouse that storage account for your diagnostic logs as well tookeep all monitoring data in a central location.</span></span> <span data-ttu-id="5c625-111">hello tárfiókot használ egy általános célú tárfiókkal, nem a blob storage-fiók kell lennie.</span><span class="sxs-lookup"><span data-stu-id="5c625-111">hello storage account you use must be a general purpose storage account, not a blob storage account.</span></span>

## <a name="diagnostic-settings"></a><span data-ttu-id="5c625-112">Diagnosztikai beállítások</span><span class="sxs-lookup"><span data-stu-id="5c625-112">Diagnostic settings</span></span>
<span data-ttu-id="5c625-113">a diagnosztikai naplók hello módszereket az alábbi tooarchive, állítsa be a **diagnosztikai beállításának** egy adott erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="5c625-113">tooarchive your diagnostic logs using any of hello methods below, you set a **diagnostic setting** for a particular resource.</span></span> <span data-ttu-id="5c625-114">Egy erőforrás diagnosztikai beállításának hello kategóriák naplók határozza meg, és metrika adatküldés tooa cél (tárfiók, az Event Hubs névtér vagy Naplóelemzési).</span><span class="sxs-lookup"><span data-stu-id="5c625-114">A diagnostic setting for a resource defines hello categories of logs and metric data sent tooa destination (storage account, Event Hubs namespace, or Log Analytics).</span></span> <span data-ttu-id="5c625-115">Az események minden egyes napló kategória és a storage-fiókban tárolt metrikaadatokat hello adatmegőrzési (tooretain napok száma) is meghatároz.</span><span class="sxs-lookup"><span data-stu-id="5c625-115">It also defines hello retention policy (number of days tooretain) for events of each log category and metric data stored in a storage account.</span></span> <span data-ttu-id="5c625-116">Ha adatmegőrzési toozero, események naplózási kategóriát tárolják határozatlan ideig (az toosay, végtelen).</span><span class="sxs-lookup"><span data-stu-id="5c625-116">If a retention policy is set toozero, events for that log category are stored indefinitely (that is toosay, forever).</span></span> <span data-ttu-id="5c625-117">Adatmegőrzési ellenkező esetben lehet bármely 1 és 2147483647 között eltelt napok számát.</span><span class="sxs-lookup"><span data-stu-id="5c625-117">A retention policy can otherwise be any number of days between 1 and 2147483647.</span></span> <span data-ttu-id="5c625-118">[További diagnosztikai beállítások itt kapcsolatos](monitoring-overview-of-diagnostic-logs.md#resource-diagnostic-settings).</span><span class="sxs-lookup"><span data-stu-id="5c625-118">[You can read more about diagnostic settings here](monitoring-overview-of-diagnostic-logs.md#resource-diagnostic-settings).</span></span> <span data-ttu-id="5c625-119">Adatmegőrzési alkalmazott napi, így: hello napi (UTC) szerint végét naplózza, amelyik most már túl hello adatmegőrzési hello nap törlődni fog.</span><span class="sxs-lookup"><span data-stu-id="5c625-119">Retention policies are applied per-day, so at hello end of a day (UTC), logs from hello day that is now beyond hello retention policy will be deleted.</span></span> <span data-ttu-id="5c625-120">Például ha egy nap adatmegőrzési, ma hello nap hello elején hello hello nap előtt tegnapi naplók lennének törölhetők</span><span class="sxs-lookup"><span data-stu-id="5c625-120">For example, if you had a retention policy of one day, at hello beginning of hello day today hello logs from hello day before yesterday would be deleted</span></span>

## <a name="archive-diagnostic-logs-using-hello-portal"></a><span data-ttu-id="5c625-121">Diagnosztikai naplók archiválása hello portál használatával</span><span class="sxs-lookup"><span data-stu-id="5c625-121">Archive diagnostic logs using hello portal</span></span>
1. <span data-ttu-id="5c625-122">Hello portálon keresse meg a figyelő tooAzure, majd kattintson a **diagnosztikai beállítások**</span><span class="sxs-lookup"><span data-stu-id="5c625-122">In hello portal, navigate tooAzure Monitor and click on **Diagnostic Settings**</span></span>

    ![Figyelés szakaszban Azure-figyelő](media/monitoring-archive-diagnostic-logs/diagnostic-settings-blade.png)

2. <span data-ttu-id="5c625-124">Opcionálisan hello szűrőlistával erőforráscsoport és erőforrások típus szerint, majd a hello erőforrás diagnosztikai beállításának tooset milyen, amelynek.</span><span class="sxs-lookup"><span data-stu-id="5c625-124">Optionally filter hello list by resource group or resource type, then click on hello resource for which you would like tooset a diagnostic setting.</span></span>

3. <span data-ttu-id="5c625-125">Ha a beállítások nem található a kiválasztott hello erőforrás, felszólító toocreate beállítás áll.</span><span class="sxs-lookup"><span data-stu-id="5c625-125">If no settings exist on hello resource you have selected, you are prompted toocreate a setting.</span></span> <span data-ttu-id="5c625-126">Kattintson a "Diagnosztika bekapcsolásához."</span><span class="sxs-lookup"><span data-stu-id="5c625-126">Click "Turn on diagnostics."</span></span>

   ![Diagnosztikai beállítás - nincsenek meglévő beállítások hozzáadása](media/monitoring-archive-diagnostic-logs/diagnostic-settings-none.png)

   <span data-ttu-id="5c625-128">Ha meglévő beállítások hello erőforráson, látni fogja a már ehhez az erőforráshoz konfigurált beállítások listáját.</span><span class="sxs-lookup"><span data-stu-id="5c625-128">If there are existing settings on hello resource, you will see a list of settings already configured on this resource.</span></span> <span data-ttu-id="5c625-129">A "Hozzáadás diagnosztikai beállításának."</span><span class="sxs-lookup"><span data-stu-id="5c625-129">Click "Add diagnostic setting."</span></span>

   ![Diagnosztikai beállítás - meglévő beállítások hozzáadása](media/monitoring-archive-diagnostic-logs/diagnostic-settings-multiple.png)

3. <span data-ttu-id="5c625-131">Adja meg a nevének beállítása és hello jelölőnégyzetet a **tooStorage fiók exportálása**, majd válasszon egy tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="5c625-131">Give your setting a name and check hello box for **Export tooStorage Account**, then select a storage account.</span></span> <span data-ttu-id="5c625-132">Beállíthatja a nap tooretain számú ezek a naplók hello segítségével **megőrzés (nap)** csúszkák.</span><span class="sxs-lookup"><span data-stu-id="5c625-132">Optionally, set a number of days tooretain these logs by using hello **Retention (days)** sliders.</span></span> <span data-ttu-id="5c625-133">Egy nulla napos megőrzési hello naplók határozatlan ideig tárolja.</span><span class="sxs-lookup"><span data-stu-id="5c625-133">A retention of zero days stores hello logs indefinitely.</span></span>
   
   ![Diagnosztikai beállítás - meglévő beállítások hozzáadása](media/monitoring-archive-diagnostic-logs/diagnostic-settings-configure.png)
    
4. <span data-ttu-id="5c625-135">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="5c625-135">Click **Save**.</span></span>

<span data-ttu-id="5c625-136">Néhány másodpercen belül hello új beállítás jelenik meg az ehhez az erőforráshoz beállítások listáját, és diagnosztikai naplók archivált toothat tárolási fiók, amint létrejön az új esemény-adatokat.</span><span class="sxs-lookup"><span data-stu-id="5c625-136">After a few moments, hello new setting appears in your list of settings for this resource, and diagnostic logs are archived toothat storage account as soon as new event data is generated.</span></span>

## <a name="archive-diagnostic-logs-via-azure-powershell"></a><span data-ttu-id="5c625-137">Diagnosztikai naplók archiválása Azure PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="5c625-137">Archive diagnostic logs via Azure PowerShell</span></span>
```
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg -StorageAccountId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Categories networksecuritygroupevent,networksecuritygrouprulecounter -Enabled $true -RetentionEnabled $true -RetentionInDays 90
```

| <span data-ttu-id="5c625-138">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="5c625-138">Property</span></span> | <span data-ttu-id="5c625-139">Szükséges</span><span class="sxs-lookup"><span data-stu-id="5c625-139">Required</span></span> | <span data-ttu-id="5c625-140">Leírás</span><span class="sxs-lookup"><span data-stu-id="5c625-140">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5c625-141">ResourceId</span><span class="sxs-lookup"><span data-stu-id="5c625-141">ResourceId</span></span> |<span data-ttu-id="5c625-142">Igen</span><span class="sxs-lookup"><span data-stu-id="5c625-142">Yes</span></span> |<span data-ttu-id="5c625-143">Erőforrás-azonosító hello erőforrás diagnosztikai beállításának tooset kívánja.</span><span class="sxs-lookup"><span data-stu-id="5c625-143">Resource ID of hello resource on which you want tooset a diagnostic setting.</span></span> |
| <span data-ttu-id="5c625-144">StorageAccountId</span><span class="sxs-lookup"><span data-stu-id="5c625-144">StorageAccountId</span></span> |<span data-ttu-id="5c625-145">Nem</span><span class="sxs-lookup"><span data-stu-id="5c625-145">No</span></span> |<span data-ttu-id="5c625-146">Erőforrás-azonosítója, hello Tárfiók toowhich diagnosztikai naplók menteni kell.</span><span class="sxs-lookup"><span data-stu-id="5c625-146">Resource ID of hello Storage Account toowhich Diagnostic Logs should be saved.</span></span> |
| <span data-ttu-id="5c625-147">Kategóriák</span><span class="sxs-lookup"><span data-stu-id="5c625-147">Categories</span></span> |<span data-ttu-id="5c625-148">Nem</span><span class="sxs-lookup"><span data-stu-id="5c625-148">No</span></span> |<span data-ttu-id="5c625-149">Napló kategóriák tooenable vesszővel tagolt listája.</span><span class="sxs-lookup"><span data-stu-id="5c625-149">Comma-separated list of log categories tooenable.</span></span> |
| <span data-ttu-id="5c625-150">Engedélyezve</span><span class="sxs-lookup"><span data-stu-id="5c625-150">Enabled</span></span> |<span data-ttu-id="5c625-151">Igen</span><span class="sxs-lookup"><span data-stu-id="5c625-151">Yes</span></span> |<span data-ttu-id="5c625-152">Logikai érték, amely jelzi, hogy diagnosztika engedélyezett vagy letiltott ezen az erőforráson.</span><span class="sxs-lookup"><span data-stu-id="5c625-152">Boolean indicating whether diagnostics are enabled or disabled on this resource.</span></span> |
| <span data-ttu-id="5c625-153">RetentionEnabled</span><span class="sxs-lookup"><span data-stu-id="5c625-153">RetentionEnabled</span></span> |<span data-ttu-id="5c625-154">Nem</span><span class="sxs-lookup"><span data-stu-id="5c625-154">No</span></span> |<span data-ttu-id="5c625-155">Logikai érték, amely azt jelzi, hogy engedélyezve vannak-e adatmegőrzési ezen az erőforráson.</span><span class="sxs-lookup"><span data-stu-id="5c625-155">Boolean indicating if a retention policy are enabled on this resource.</span></span> |
| <span data-ttu-id="5c625-156">retentionInDays</span><span class="sxs-lookup"><span data-stu-id="5c625-156">RetentionInDays</span></span> |<span data-ttu-id="5c625-157">Nem</span><span class="sxs-lookup"><span data-stu-id="5c625-157">No</span></span> |<span data-ttu-id="5c625-158">Amely események kell megtartani 1 és 2147483647 közötti napok számát.</span><span class="sxs-lookup"><span data-stu-id="5c625-158">Number of days for which events should be retained between 1 and 2147483647.</span></span> <span data-ttu-id="5c625-159">A nulla érték hello naplók határozatlan ideig tárolja.</span><span class="sxs-lookup"><span data-stu-id="5c625-159">A value of zero stores hello logs indefinitely.</span></span> |

## <a name="archive-diagnostic-logs-via-hello-cross-platform-cli"></a><span data-ttu-id="5c625-160">Diagnosztikai naplók archiválása hello platformfüggetlen parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="5c625-160">Archive diagnostic logs via hello Cross-Platform CLI</span></span>
```
azure insights diagnostic set --resourceId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg --storageId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage –categories networksecuritygroupevent,networksecuritygrouprulecounter --enabled true
```

| <span data-ttu-id="5c625-161">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="5c625-161">Property</span></span> | <span data-ttu-id="5c625-162">Szükséges</span><span class="sxs-lookup"><span data-stu-id="5c625-162">Required</span></span> | <span data-ttu-id="5c625-163">Leírás</span><span class="sxs-lookup"><span data-stu-id="5c625-163">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5c625-164">resourceId</span><span class="sxs-lookup"><span data-stu-id="5c625-164">resourceId</span></span> |<span data-ttu-id="5c625-165">Igen</span><span class="sxs-lookup"><span data-stu-id="5c625-165">Yes</span></span> |<span data-ttu-id="5c625-166">Erőforrás-azonosító hello erőforrás diagnosztikai beállításának tooset kívánja.</span><span class="sxs-lookup"><span data-stu-id="5c625-166">Resource ID of hello resource on which you want tooset a diagnostic setting.</span></span> |
| <span data-ttu-id="5c625-167">storageId</span><span class="sxs-lookup"><span data-stu-id="5c625-167">storageId</span></span> |<span data-ttu-id="5c625-168">Nem</span><span class="sxs-lookup"><span data-stu-id="5c625-168">No</span></span> |<span data-ttu-id="5c625-169">Erőforrás-azonosító hello Tárfiók toowhich diagnosztikai naplók menteni kell.</span><span class="sxs-lookup"><span data-stu-id="5c625-169">Resource ID of hello Storage Account toowhich diagnostic logs should be saved.</span></span> |
| <span data-ttu-id="5c625-170">Kategóriák</span><span class="sxs-lookup"><span data-stu-id="5c625-170">categories</span></span> |<span data-ttu-id="5c625-171">Nem</span><span class="sxs-lookup"><span data-stu-id="5c625-171">No</span></span> |<span data-ttu-id="5c625-172">Napló kategóriák tooenable vesszővel tagolt listája.</span><span class="sxs-lookup"><span data-stu-id="5c625-172">Comma-separated list of log categories tooenable.</span></span> |
| <span data-ttu-id="5c625-173">engedélyezve</span><span class="sxs-lookup"><span data-stu-id="5c625-173">enabled</span></span> |<span data-ttu-id="5c625-174">Igen</span><span class="sxs-lookup"><span data-stu-id="5c625-174">Yes</span></span> |<span data-ttu-id="5c625-175">Logikai érték, amely jelzi, hogy diagnosztika engedélyezett vagy letiltott ezen az erőforráson.</span><span class="sxs-lookup"><span data-stu-id="5c625-175">Boolean indicating whether diagnostics are enabled or disabled on this resource.</span></span> |

## <a name="archive-diagnostic-logs-via-hello-rest-api"></a><span data-ttu-id="5c625-176">Diagnosztikai naplók archiválása hello REST API-n keresztül</span><span class="sxs-lookup"><span data-stu-id="5c625-176">Archive diagnostic logs via hello REST API</span></span>
<span data-ttu-id="5c625-177">[Lásd a jelen dokumentum](https://docs.microsoft.com/rest/api/monitor/servicediagnosticsettings) olvashat, hogyan állíthat be a diagnosztikai beállításának hello Azure figyelő REST API használatával.</span><span class="sxs-lookup"><span data-stu-id="5c625-177">[See this document](https://docs.microsoft.com/rest/api/monitor/servicediagnosticsettings) for information on how you can set up a diagnostic setting using hello Azure Monitor REST API.</span></span>

## <a name="schema-of-diagnostic-logs-in-hello-storage-account"></a><span data-ttu-id="5c625-178">Diagnosztikai naplók hello tárfiókban sémája</span><span class="sxs-lookup"><span data-stu-id="5c625-178">Schema of diagnostic logs in hello storage account</span></span>
<span data-ttu-id="5c625-179">Állított be archiválási, miután egy tároló jön létre hello tárfiók hello napló kategóriák engedélyezte előfordulásakor egy eseményt.</span><span class="sxs-lookup"><span data-stu-id="5c625-179">Once you have set up archival, a storage container is created in hello storage account as soon as an event occurs in one of hello log categories you have enabled.</span></span> <span data-ttu-id="5c625-180">hello tárolóban hello BLOB formátuma azonos a diagnosztikai naplók közötti hello és hello tevékenységnapló követése.</span><span class="sxs-lookup"><span data-stu-id="5c625-180">hello blobs within hello container follow hello same format across Diagnostic Logs and hello Activity Log.</span></span> <span data-ttu-id="5c625-181">a blobok hello szerkezete van:</span><span class="sxs-lookup"><span data-stu-id="5c625-181">hello structure of these blobs is:</span></span>

> <span data-ttu-id="5c625-182">insights - logs-{napló kategória neve} / resourceId = / ELŐFIZETÉSEK / {előfizetés-azonosító} /RESOURCEGROUPS/ {erőforráscsoport neve} /PROVIDERS/ {erőforrás-szolgáltató neve} / {resource type} / {erőforrás neve} / y = {négyjegyű numerikus year} / m = {kétjegyű numerikus month} / d {kétjegyű számozott napja} = / h = {kétjegyű 24 órás hour}/m=00/PT1H.json</span><span class="sxs-lookup"><span data-stu-id="5c625-182">insights-logs-{log category name}/resourceId=/SUBSCRIPTIONS/{subscription ID}/RESOURCEGROUPS/{resource group name}/PROVIDERS/{resource provider name}/{resource type}/{resource name}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json</span></span>
> 
> 

<span data-ttu-id="5c625-183">Vagy egyszerűbb,</span><span class="sxs-lookup"><span data-stu-id="5c625-183">Or, more simply,</span></span>

> <span data-ttu-id="5c625-184">insights - logs-{napló kategória neve} / resourceId = / {erőforrás-azonosítót} / y = {négyjegyű numerikus year} / m = {kétjegyű numerikus month} / d {kétjegyű számozott napja} = / h = {kétjegyű 24 órás hour}/m=00/PT1H.json</span><span class="sxs-lookup"><span data-stu-id="5c625-184">insights-logs-{log category name}/resourceId=/{resource Id}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json</span></span>
> 
> 

<span data-ttu-id="5c625-185">A blob neve lehet, például:</span><span class="sxs-lookup"><span data-stu-id="5c625-185">For example, a blob name might be:</span></span>

> <span data-ttu-id="5c625-186">insights-logs-networksecuritygrouprulecounter/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUP/TESTNSG/y=2016/m=08/d=22/h=18/m=00/PT1H.json</span><span class="sxs-lookup"><span data-stu-id="5c625-186">insights-logs-networksecuritygrouprulecounter/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUP/TESTNSG/y=2016/m=08/d=22/h=18/m=00/PT1H.json</span></span>
> 
> 

<span data-ttu-id="5c625-187">Minden egyes PT1H.json blob hello blob URL-címben megadott hello órán belül előforduló eseményeket a JSON blob tartalmaz (például h = 12).</span><span class="sxs-lookup"><span data-stu-id="5c625-187">Each PT1H.json blob contains a JSON blob of events that occurred within hello hour specified in hello blob URL (for example, h=12).</span></span> <span data-ttu-id="5c625-188">Hello jelen óra, során eseményeket hozzáfűzött toohello PT1H.json fájlt is, azok bekövetkezésekor.</span><span class="sxs-lookup"><span data-stu-id="5c625-188">During hello present hour, events are appended toohello PT1H.json file as they occur.</span></span> <span data-ttu-id="5c625-189">hello perc (m = 00) mindig 00, mivel diagnosztikai alkalmazásnapló-események az egyes blobok óránként van felosztva.</span><span class="sxs-lookup"><span data-stu-id="5c625-189">hello minute value (m=00) is always 00, since diagnostic log events are broken into individual blobs per hour.</span></span>

<span data-ttu-id="5c625-190">Hello PT1H.json fájlban tárolja minden esemény hello "rekordok" tömb, a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="5c625-190">Within hello PT1H.json file, each event is stored in hello “records” array, following this format:</span></span>

```
{
    "records": [
        {
            "time": "2016-07-01T00:00:37.2040000Z",
            "systemId": "46cdbb41-cb9c-4f3d-a5b4-1d458d827ff1",
            "category": "NetworkSecurityGroupRuleCounter",
            "resourceId": "/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/TESTNSG",
            "operationName": "NetworkSecurityGroupCounters",
            "properties": {
                "vnetResourceGuid": "{12345678-9012-3456-7890-123456789012}",
                "subnetPrefix": "10.3.0.0/24",
                "macAddress": "000123456789",
                "ruleName": "/subscriptions/ s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg/securityRules/default-allow-rdp",
                "direction": "In",
                "type": "allow",
                "matchedConnections": 1988
            }
        }
    ]
}
```

| <span data-ttu-id="5c625-191">Elem neve</span><span class="sxs-lookup"><span data-stu-id="5c625-191">Element name</span></span> | <span data-ttu-id="5c625-192">Leírás</span><span class="sxs-lookup"><span data-stu-id="5c625-192">Description</span></span> |
| --- | --- |
| <span data-ttu-id="5c625-193">time</span><span class="sxs-lookup"><span data-stu-id="5c625-193">time</span></span> |<span data-ttu-id="5c625-194">Hello esemény lett létrehozva, amikor hello Azure szolgáltatás feldolgozása hello idejét jelző időbélyegző megfelelő hello esemény kérelmet.</span><span class="sxs-lookup"><span data-stu-id="5c625-194">Timestamp when hello event was generated by hello Azure service processing hello request corresponding hello event.</span></span> |
| <span data-ttu-id="5c625-195">resourceId</span><span class="sxs-lookup"><span data-stu-id="5c625-195">resourceId</span></span> |<span data-ttu-id="5c625-196">Erőforrás-azonosítója, hello érintett erőforrás.</span><span class="sxs-lookup"><span data-stu-id="5c625-196">Resource ID of hello impacted resource.</span></span> |
| <span data-ttu-id="5c625-197">operationName</span><span class="sxs-lookup"><span data-stu-id="5c625-197">operationName</span></span> |<span data-ttu-id="5c625-198">Hello művelet neve.</span><span class="sxs-lookup"><span data-stu-id="5c625-198">Name of hello operation.</span></span> |
| <span data-ttu-id="5c625-199">category</span><span class="sxs-lookup"><span data-stu-id="5c625-199">category</span></span> |<span data-ttu-id="5c625-200">Napló kategória hello esemény.</span><span class="sxs-lookup"><span data-stu-id="5c625-200">Log category of hello event.</span></span> |
| <span data-ttu-id="5c625-201">properties</span><span class="sxs-lookup"><span data-stu-id="5c625-201">properties</span></span> |<span data-ttu-id="5c625-202">Állítsa be a `<Key, Value>` hello esemény részleteinek hello leíró párok (azaz szótárában).</span><span class="sxs-lookup"><span data-stu-id="5c625-202">Set of `<Key, Value>` pairs (i.e. Dictionary) describing hello details of hello event.</span></span> |

> [!NOTE]
> <span data-ttu-id="5c625-203">hello tulajdonságai és használatának azokat a tulajdonságokat a hello erőforrás függően változhat.</span><span class="sxs-lookup"><span data-stu-id="5c625-203">hello properties and usage of those properties can vary depending on hello resource.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="5c625-204">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5c625-204">Next steps</span></span>
* [<span data-ttu-id="5c625-205">Elemzés blobok letöltése</span><span class="sxs-lookup"><span data-stu-id="5c625-205">Download blobs for analysis</span></span>](../storage/storage-dotnet-how-to-use-blobs.md)
* [<span data-ttu-id="5c625-206">Adatfolyam-diagnosztikai naplók tooan Event Hubs névtér</span><span class="sxs-lookup"><span data-stu-id="5c625-206">Stream diagnostic logs tooan Event Hubs namespace</span></span>](monitoring-stream-diagnostic-logs-to-event-hubs.md)
* [<span data-ttu-id="5c625-207">Tudjon meg többet a diagnosztikai naplók</span><span class="sxs-lookup"><span data-stu-id="5c625-207">Read more about diagnostic logs</span></span>](monitoring-overview-of-diagnostic-logs.md)
