---
title: "Az Azure tevékenységnapló archiválására |} Microsoft Docs"
description: "Útmutató az Azure tevékenységnapló a hosszú távú megőrzési tárfiókokban archiválja."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: d37d3fda-8ef1-477c-a360-a855b418de84
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/09/2016
ms.author: johnkem
ms.openlocfilehash: 0e3a5b84f57eac96249430fa1c2c4cc076c2926a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="archive-the-azure-activity-log"></a><span data-ttu-id="a5e39-103">Az Azure tevékenységnapló archiválása</span><span class="sxs-lookup"><span data-stu-id="a5e39-103">Archive the Azure Activity Log</span></span>
<span data-ttu-id="a5e39-104">Ebben a cikkben megmutatjuk használatát az Azure portálon, a PowerShell-parancsmagokkal vagy a platformfüggetlen parancssori felület archiválására a [ **Azure tevékenységnapló** ](monitoring-overview-activity-logs.md) tárfiókokban.</span><span class="sxs-lookup"><span data-stu-id="a5e39-104">In this article, we show how you can use the Azure portal, PowerShell Cmdlets, or Cross-Platform CLI to archive your [**Azure Activity Log**](monitoring-overview-activity-logs.md) in a storage account.</span></span> <span data-ttu-id="a5e39-105">Ez a beállítás akkor hasznos, ha azt szeretné, hogy megőrzi a naplózási, statikus elemzési vagy biztonsági mentése (a teljes hozzáféréssel az adatmegőrzési) 90 napnál hosszabb tevékenységnapló.</span><span class="sxs-lookup"><span data-stu-id="a5e39-105">This option is useful if you would like to retain your Activity Log longer than 90 days (with full control over the retention policy) for audit, static analysis, or backup.</span></span> <span data-ttu-id="a5e39-106">Ha csak szeretné megőrizni az események 90 napig, vagy kevesebb nem kell beállítása archiválási tárfiókba, mert tevékenységnapló események kerülnek be az Azure platformon 90 napig engedélyezése archiválás nélkül.</span><span class="sxs-lookup"><span data-stu-id="a5e39-106">If you only need to retain your events for 90 days or less you do not need to set up archival to a storage account, since Activity Log events are retained in the Azure platform for 90 days without enabling archival.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a5e39-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="a5e39-107">Prerequisites</span></span>
<span data-ttu-id="a5e39-108">Kezdés előtt kell [hozzon létre egy tárfiókot](../storage/common/storage-create-storage-account.md#create-a-storage-account) , amelyhez a tevékenységnapló archiválhatók.</span><span class="sxs-lookup"><span data-stu-id="a5e39-108">Before you begin, you need to [create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account) to which you can archive your Activity Log.</span></span> <span data-ttu-id="a5e39-109">Erősen ajánlott, hogy nem használja a benne tárolt, így jobban szabályozhatja a hozzáférést a figyelési adatok, amelyeket más, nem figyelési adatokat tartalmazó meglévő tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="a5e39-109">We highly recommend that you do not use an existing storage account that has other, non-monitoring data stored in it so that you can better control access to monitoring data.</span></span> <span data-ttu-id="a5e39-110">Azonban ha is archiválás diagnosztikai naplók és a metrikák egy tárfiókba, logikus összes figyelési adatot elhelyez egy központi helyen, hogy a tevékenységnapló tárfiók használatával.</span><span class="sxs-lookup"><span data-stu-id="a5e39-110">However, if you are also archiving Diagnostic Logs and metrics to a storage account, it may make sense to use that storage account for your Activity Log as well to keep all monitoring data in a central location.</span></span> <span data-ttu-id="a5e39-111">A storage-fiók használata egy általános célú tárfiókkal, nem a blob storage-fiók kell lennie.</span><span class="sxs-lookup"><span data-stu-id="a5e39-111">The storage account you use must be a general purpose storage account, not a blob storage account.</span></span> <span data-ttu-id="a5e39-112">A tárfiók nem kell ugyanazt az előfizetést, mint az előfizetés naplók kibocsátó mindaddig, amíg a beállítás konfigurálása felhasználó hozzáfér megfelelő RBAC mindkét előfizetéshez lehet.</span><span class="sxs-lookup"><span data-stu-id="a5e39-112">The storage account does not have to be in the same subscription as the subscription emitting logs as long as the user who configures the setting has appropriate RBAC access to both subscriptions.</span></span>

## <a name="log-profile"></a><span data-ttu-id="a5e39-113">Napló-profil</span><span class="sxs-lookup"><span data-stu-id="a5e39-113">Log Profile</span></span>
<span data-ttu-id="a5e39-114">Az alábbi módszerekkel történő tevékenységnapló archiválására, állítsa be a **napló profil** az előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="a5e39-114">To archive the Activity Log using any of the methods below, you set the **Log Profile** for a subscription.</span></span> <span data-ttu-id="a5e39-115">A napló profil határozza meg az eseményeket, amelyek tárolt vagy a folyamatos átviteli és a kimenetek – tárolási fiók és/vagy az event hub.</span><span class="sxs-lookup"><span data-stu-id="a5e39-115">The Log Profile defines the type of events that are stored or streamed and the outputs—storage account and/or event hub.</span></span> <span data-ttu-id="a5e39-116">A storage-fiókban tárolt eseményeket az adatmegőrzési (a megőrizni kívánt napok száma) is meghatároz.</span><span class="sxs-lookup"><span data-stu-id="a5e39-116">It also defines the retention policy (number of days to retain) for events stored in a storage account.</span></span> <span data-ttu-id="a5e39-117">Ha az adatmegőrzési nullára van beállítva, események határozatlan ideig tárolja.</span><span class="sxs-lookup"><span data-stu-id="a5e39-117">If the retention policy is set to zero, events are stored indefinitely.</span></span> <span data-ttu-id="a5e39-118">Ellenkező esetben ez állítható 1 és 2147483647 között bármilyen érték.</span><span class="sxs-lookup"><span data-stu-id="a5e39-118">Otherwise, this can be set to any value between 1 and 2147483647.</span></span> <span data-ttu-id="a5e39-119">Adatmegőrzési alkalmazott napi,, így napi (UTC) szerint naplókat, amelyik most már a megőrzési túl napjától végén házirend törlődni fog.</span><span class="sxs-lookup"><span data-stu-id="a5e39-119">Retention policies are applied per-day, so at the end of a day (UTC), logs from the day that is now beyond the retention policy will be deleted.</span></span> <span data-ttu-id="a5e39-120">Például ha egy nap adatmegőrzési, mai nap kezdetén a napló, a nap előtt tegnap törlése akkor történik meg.</span><span class="sxs-lookup"><span data-stu-id="a5e39-120">For example, if you had a retention policy of one day, at the beginning of the day today the logs from the day before yesterday would be deleted.</span></span> <span data-ttu-id="a5e39-121">[Részletesebb naplófájl kapcsolatos profilok Itt](monitoring-overview-activity-logs.md#export-the-activity-log-with-a-log-profile).</span><span class="sxs-lookup"><span data-stu-id="a5e39-121">[You can read more about log profiles here](monitoring-overview-activity-logs.md#export-the-activity-log-with-a-log-profile).</span></span> 

## <a name="archive-the-activity-log-using-the-portal"></a><span data-ttu-id="a5e39-122">A tevékenység naplót, a portál használatával</span><span class="sxs-lookup"><span data-stu-id="a5e39-122">Archive the Activity Log using the portal</span></span>
1. <span data-ttu-id="a5e39-123">A portálon kattintson a **tevékenységnapló** a bal oldali navigációs hivatkozásra kattintva.</span><span class="sxs-lookup"><span data-stu-id="a5e39-123">In the portal, click the **Activity Log** link on the left-side navigation.</span></span> <span data-ttu-id="a5e39-124">Ha a tevékenység napló hivatkozás nem látható, kattintson a **több szolgáltatások** először hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="a5e39-124">If you don’t see a link for the Activity Log, click the **More Services** link first.</span></span>
   
    ![Navigáljon a tevékenységnapló panel](media/monitoring-archive-activity-log/act-log-portal-navigate.png)
2. <span data-ttu-id="a5e39-126">Kattintson a panel tetején **exportálása**.</span><span class="sxs-lookup"><span data-stu-id="a5e39-126">At the top of the blade, click **Export**.</span></span>
   
    ![Kattintson az Exportálás gomb](media/monitoring-archive-activity-log/act-log-portal-export-button.png)
3. <span data-ttu-id="a5e39-128">A megjelenő panelen, jelölje be a **tárfiókba exportálása** , és válasszon egy tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="a5e39-128">In the blade that appears, check the box for **Export to a storage account** and select a storage account.</span></span>
   
    ![A storage-fiók beállítása](media/monitoring-archive-activity-log/act-log-portal-export-blade.png)
4. <span data-ttu-id="a5e39-130">A vagy a csúszka definiálja, amelynek tevékenységnapló események kell tartani a tárfiókban lévő napok számát.</span><span class="sxs-lookup"><span data-stu-id="a5e39-130">Using the slider or text box, define a number of days for which Activity Log events should be kept in your storage account.</span></span> <span data-ttu-id="a5e39-131">Ha szeretné adatait őrzi meg a tárfiók határozatlan ideig, ez az érték nullára.</span><span class="sxs-lookup"><span data-stu-id="a5e39-131">If you prefer to have your data persisted in the storage account indefinitely, set this number to zero.</span></span>
5. <span data-ttu-id="a5e39-132">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="a5e39-132">Click **Save**.</span></span>

## <a name="archive-the-activity-log-via-powershell"></a><span data-ttu-id="a5e39-133">A műveletnapló PowerShell archiválása</span><span class="sxs-lookup"><span data-stu-id="a5e39-133">Archive the Activity Log via PowerShell</span></span>
```
Add-AzureRmLogProfile -Name my_log_profile -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus -RetentionInDays 180 -Categories Write,Delete,Action
```

| <span data-ttu-id="a5e39-134">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="a5e39-134">Property</span></span> | <span data-ttu-id="a5e39-135">Szükséges</span><span class="sxs-lookup"><span data-stu-id="a5e39-135">Required</span></span> | <span data-ttu-id="a5e39-136">Leírás</span><span class="sxs-lookup"><span data-stu-id="a5e39-136">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a5e39-137">StorageAccountId</span><span class="sxs-lookup"><span data-stu-id="a5e39-137">StorageAccountId</span></span> |<span data-ttu-id="a5e39-138">Nem</span><span class="sxs-lookup"><span data-stu-id="a5e39-138">No</span></span> |<span data-ttu-id="a5e39-139">Erőforrás-azonosító a tárfiók tevékenységi naplóit mentésére.</span><span class="sxs-lookup"><span data-stu-id="a5e39-139">Resource ID of the Storage Account to which Activity Logs should be saved.</span></span> |
| <span data-ttu-id="a5e39-140">Helyek</span><span class="sxs-lookup"><span data-stu-id="a5e39-140">Locations</span></span> |<span data-ttu-id="a5e39-141">Igen</span><span class="sxs-lookup"><span data-stu-id="a5e39-141">Yes</span></span> |<span data-ttu-id="a5e39-142">Régiók, amelynek szeretné tevékenységnapló eseményeinek gyűjtése vesszővel tagolt listája.</span><span class="sxs-lookup"><span data-stu-id="a5e39-142">Comma-separated list of regions for which you would like to collect Activity Log events.</span></span> <span data-ttu-id="a5e39-143">Megtekintheti az összes régiók listáját [ezen a lapon felkeresésével](https://azure.microsoft.com/en-us/regions) vagy használatával [az Azure felügyeleti REST API](https://msdn.microsoft.com/library/azure/gg441293.aspx).</span><span class="sxs-lookup"><span data-stu-id="a5e39-143">You can view a list of all regions [by visiting this page](https://azure.microsoft.com/en-us/regions) or by using [the Azure Management REST API](https://msdn.microsoft.com/library/azure/gg441293.aspx).</span></span> |
| <span data-ttu-id="a5e39-144">retentionInDays</span><span class="sxs-lookup"><span data-stu-id="a5e39-144">RetentionInDays</span></span> |<span data-ttu-id="a5e39-145">Igen</span><span class="sxs-lookup"><span data-stu-id="a5e39-145">Yes</span></span> |<span data-ttu-id="a5e39-146">Az eseményeket meg kell őrizni, 1 és 2147483647 közötti napok számát.</span><span class="sxs-lookup"><span data-stu-id="a5e39-146">Number of days for which events should be retained, between 1 and 2147483647.</span></span> <span data-ttu-id="a5e39-147">A nulla érték a naplók határozatlan ideig tárolja (végtelen).</span><span class="sxs-lookup"><span data-stu-id="a5e39-147">A value of zero stores the logs indefinitely (forever).</span></span> |
| <span data-ttu-id="a5e39-148">Kategóriák</span><span class="sxs-lookup"><span data-stu-id="a5e39-148">Categories</span></span> |<span data-ttu-id="a5e39-149">Igen</span><span class="sxs-lookup"><span data-stu-id="a5e39-149">Yes</span></span> |<span data-ttu-id="a5e39-150">Be kell esemény kategóriák vesszővel tagolt listája.</span><span class="sxs-lookup"><span data-stu-id="a5e39-150">Comma-separated list of event categories that should be collected.</span></span> <span data-ttu-id="a5e39-151">Lehetséges értékek a következők: Olvasás, törlés és művelet.</span><span class="sxs-lookup"><span data-stu-id="a5e39-151">Possible values are Write, Delete, and Action.</span></span> |

## <a name="archive-the-activity-log-via-cli"></a><span data-ttu-id="a5e39-152">A tevékenység naplót parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="a5e39-152">Archive the Activity Log via CLI</span></span>
```
azure insights logprofile add --name my_log_profile --storageId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/my_storage --locations global,westus,eastus,northeurope --retentionInDays 180 –categories Write,Delete,Action
```

| <span data-ttu-id="a5e39-153">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="a5e39-153">Property</span></span> | <span data-ttu-id="a5e39-154">Szükséges</span><span class="sxs-lookup"><span data-stu-id="a5e39-154">Required</span></span> | <span data-ttu-id="a5e39-155">Leírás</span><span class="sxs-lookup"><span data-stu-id="a5e39-155">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a5e39-156">név</span><span class="sxs-lookup"><span data-stu-id="a5e39-156">name</span></span> |<span data-ttu-id="a5e39-157">Igen</span><span class="sxs-lookup"><span data-stu-id="a5e39-157">Yes</span></span> |<span data-ttu-id="a5e39-158">A napló profil neve.</span><span class="sxs-lookup"><span data-stu-id="a5e39-158">Name of your log profile.</span></span> |
| <span data-ttu-id="a5e39-159">storageId</span><span class="sxs-lookup"><span data-stu-id="a5e39-159">storageId</span></span> |<span data-ttu-id="a5e39-160">Nem</span><span class="sxs-lookup"><span data-stu-id="a5e39-160">No</span></span> |<span data-ttu-id="a5e39-161">Erőforrás-azonosító a tárfiók tevékenységi naplóit mentésére.</span><span class="sxs-lookup"><span data-stu-id="a5e39-161">Resource ID of the Storage Account to which Activity Logs should be saved.</span></span> |
| <span data-ttu-id="a5e39-162">Helyek</span><span class="sxs-lookup"><span data-stu-id="a5e39-162">locations</span></span> |<span data-ttu-id="a5e39-163">Igen</span><span class="sxs-lookup"><span data-stu-id="a5e39-163">Yes</span></span> |<span data-ttu-id="a5e39-164">Régiók, amelynek szeretné tevékenységnapló eseményeinek gyűjtése vesszővel tagolt listája.</span><span class="sxs-lookup"><span data-stu-id="a5e39-164">Comma-separated list of regions for which you would like to collect Activity Log events.</span></span> <span data-ttu-id="a5e39-165">Megtekintheti az összes régiók listáját [ezen a lapon felkeresésével](https://azure.microsoft.com/en-us/regions) vagy használatával [az Azure felügyeleti REST API](https://msdn.microsoft.com/library/azure/gg441293.aspx).</span><span class="sxs-lookup"><span data-stu-id="a5e39-165">You can view a list of all regions [by visiting this page](https://azure.microsoft.com/en-us/regions) or by using [the Azure Management REST API](https://msdn.microsoft.com/library/azure/gg441293.aspx).</span></span> |
| <span data-ttu-id="a5e39-166">retentionInDays</span><span class="sxs-lookup"><span data-stu-id="a5e39-166">retentionInDays</span></span> |<span data-ttu-id="a5e39-167">Igen</span><span class="sxs-lookup"><span data-stu-id="a5e39-167">Yes</span></span> |<span data-ttu-id="a5e39-168">Az eseményeket meg kell őrizni, 1 és 2147483647 közötti napok számát.</span><span class="sxs-lookup"><span data-stu-id="a5e39-168">Number of days for which events should be retained, between 1 and 2147483647.</span></span> <span data-ttu-id="a5e39-169">A nulla érték határozatlan ideig tárolja a naplók (végtelen).</span><span class="sxs-lookup"><span data-stu-id="a5e39-169">A value of zero will store the logs indefinitely (forever).</span></span> |
| <span data-ttu-id="a5e39-170">Kategóriák</span><span class="sxs-lookup"><span data-stu-id="a5e39-170">categories</span></span> |<span data-ttu-id="a5e39-171">Igen</span><span class="sxs-lookup"><span data-stu-id="a5e39-171">Yes</span></span> |<span data-ttu-id="a5e39-172">Be kell esemény kategóriák vesszővel tagolt listája.</span><span class="sxs-lookup"><span data-stu-id="a5e39-172">Comma-separated list of event categories that should be collected.</span></span> <span data-ttu-id="a5e39-173">Lehetséges értékek a következők: Olvasás, törlés és művelet.</span><span class="sxs-lookup"><span data-stu-id="a5e39-173">Possible values are Write, Delete, and Action.</span></span> |

## <a name="storage-schema-of-the-activity-log"></a><span data-ttu-id="a5e39-174">A műveletnapló tárolási séma</span><span class="sxs-lookup"><span data-stu-id="a5e39-174">Storage schema of the Activity Log</span></span>
<span data-ttu-id="a5e39-175">Miután állított be archiválási, tárolót létrejön a tárfiók, amint tevékenységnapló esemény következik be.</span><span class="sxs-lookup"><span data-stu-id="a5e39-175">Once you have set up archival, a storage container will be created in the storage account as soon as an Activity Log event occurs.</span></span> <span data-ttu-id="a5e39-176">A tárolóban található blobok kövesse ugyanazt a formátumot minden tevékenységnapló és diagnosztikai naplókat.</span><span class="sxs-lookup"><span data-stu-id="a5e39-176">The blobs within the container follow the same format across the Activity Log and Diagnostic Logs.</span></span> <span data-ttu-id="a5e39-177">A blobok szerkezete van:</span><span class="sxs-lookup"><span data-stu-id="a5e39-177">The structure of these blobs is:</span></span>

> <span data-ttu-id="a5e39-178">insights-működési-naplók/name = alapértelmezett/resourceId = / SUBSCRIPTIONS / {előfizetés-azonosító} / y = {négyjegyű numerikus year} / m = {kétjegyű numerikus month} / d {kétjegyű számozott napja} = / h = {kétjegyű 24 órás hour}/m=00/PT1H.json</span><span class="sxs-lookup"><span data-stu-id="a5e39-178">insights-operational-logs/name=default/resourceId=/SUBSCRIPTIONS/{subscription ID}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json</span></span>
> 
> 

<span data-ttu-id="a5e39-179">A blob neve lehet, például:</span><span class="sxs-lookup"><span data-stu-id="a5e39-179">For example, a blob name might be:</span></span>

> <span data-ttu-id="a5e39-180">insights-Operational-Logs/Name=default/resourceId=/Subscriptions/s1id1234-5679-0123-4567-890123456789/y=2016/m=08/d=22/h=18/m=00/PT1H.JSON</span><span class="sxs-lookup"><span data-stu-id="a5e39-180">insights-operational-logs/name=default/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/y=2016/m=08/d=22/h=18/m=00/PT1H.json</span></span>
> 
> 

<span data-ttu-id="a5e39-181">Minden egyes PT1H.json blob tartalmaz a blob URL-címben megadott órán belül előforduló eseményeket a JSON blob (pl. h = 12).</span><span class="sxs-lookup"><span data-stu-id="a5e39-181">Each PT1H.json blob contains a JSON blob of events that occurred within the hour specified in the blob URL (e.g. h=12).</span></span> <span data-ttu-id="a5e39-182">A jelenlegi órán belül események lesz hozzáfűzve a PT1H.json fájl azok bekövetkezésekor.</span><span class="sxs-lookup"><span data-stu-id="a5e39-182">During the present hour, events are appended to the PT1H.json file as they occur.</span></span> <span data-ttu-id="a5e39-183">A perc értéket (m = 00) mindig 00, mivel tevékenységnapló események óránként egyes blobok van felosztva.</span><span class="sxs-lookup"><span data-stu-id="a5e39-183">The minute value (m=00) is always 00, since Activity Log events are broken into individual blobs per hour.</span></span>

<span data-ttu-id="a5e39-184">PT1H.json fájlon belül mindegyik esemény tárolja a "rekordok" tömb, a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="a5e39-184">Within the PT1H.json file, each event is stored in the “records” array, following this format:</span></span>

```
{
    "records": [
        {
            "time": "2015-01-21T22:14:26.9792776Z",
            "resourceId": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841",
            "operationName": "microsoft.support/supporttickets/write",
            "category": "Write",
            "resultType": "Success",
            "resultSignature": "Succeeded.Created",
            "durationMs": 2826,
            "callerIpAddress": "111.111.111.11",
            "correlationId": "c776f9f4-36e5-4e0e-809b-c9b3c3fb62a8",
            "identity": {
                "authorization": {
                    "scope": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841",
                    "action": "microsoft.support/supporttickets/write",
                    "evidence": {
                        "role": "Subscription Admin"
                    }
                },
                "claims": {
                    "aud": "https://management.core.windows.net/",
                    "iss": "https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/",
                    "iat": "1421876371",
                    "nbf": "1421876371",
                    "exp": "1421880271",
                    "ver": "1.0",
                    "http://schemas.microsoft.com/identity/claims/tenantid": "1e8d8218-c5e7-4578-9acc-9abbd5d23315 ",
                    "http://schemas.microsoft.com/claims/authnmethodsreferences": "pwd",
                    "http://schemas.microsoft.com/identity/claims/objectidentifier": "2468adf0-8211-44e3-95xq-85137af64708",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "admin@contoso.com",
                    "puid": "20030000801A118C",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "9vckmEGF7zDKk1YzIY8k0t1_EAPaXoeHyPRn6f413zM",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "John",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "Smith",
                    "name": "John Smith",
                    "groups": "cacfe77c-e058-4712-83qw-f9b08849fd60,7f71d11d-4c41-4b23-99d2-d32ce7aa621c,31522864-0578-4ea0-9gdc-e66cc564d18c",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": " admin@contoso.com",
                    "appid": "c44b4083-3bq0-49c1-b47d-974e53cbdf3c",
                    "appidacr": "2",
                    "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
                    "http://schemas.microsoft.com/claims/authnclassreference": "1"
                }
            },
            "level": "Information",
            "location": "global",
            "properties": {
                "statusCode": "Created",
                "serviceRequestId": "50d5cddb-8ca0-47ad-9b80-6cde2207f97c"
            }
        }
    ]
}
```


| <span data-ttu-id="a5e39-185">Elem neve</span><span class="sxs-lookup"><span data-stu-id="a5e39-185">Element name</span></span> | <span data-ttu-id="a5e39-186">Leírás</span><span class="sxs-lookup"><span data-stu-id="a5e39-186">Description</span></span> |
| --- | --- |
| <span data-ttu-id="a5e39-187">time</span><span class="sxs-lookup"><span data-stu-id="a5e39-187">time</span></span> |<span data-ttu-id="a5e39-188">Az esemény az esemény megfelelő a kérés feldolgozása az Azure-szolgáltatás által kiváltott idejét jelző időbélyegző.</span><span class="sxs-lookup"><span data-stu-id="a5e39-188">Timestamp when the event was generated by the Azure service processing the request corresponding the event.</span></span> |
| <span data-ttu-id="a5e39-189">resourceId</span><span class="sxs-lookup"><span data-stu-id="a5e39-189">resourceId</span></span> |<span data-ttu-id="a5e39-190">Erőforrás-azonosító az érintett erőforrás.</span><span class="sxs-lookup"><span data-stu-id="a5e39-190">Resource ID of the impacted resource.</span></span> |
| <span data-ttu-id="a5e39-191">operationName</span><span class="sxs-lookup"><span data-stu-id="a5e39-191">operationName</span></span> |<span data-ttu-id="a5e39-192">A művelet neve.</span><span class="sxs-lookup"><span data-stu-id="a5e39-192">Name of the operation.</span></span> |
| <span data-ttu-id="a5e39-193">category</span><span class="sxs-lookup"><span data-stu-id="a5e39-193">category</span></span> |<span data-ttu-id="a5e39-194">A művelet kategória eg.</span><span class="sxs-lookup"><span data-stu-id="a5e39-194">Category of the action, eg.</span></span> <span data-ttu-id="a5e39-195">Írási, olvassa el, a műveletet.</span><span class="sxs-lookup"><span data-stu-id="a5e39-195">Write, Read, Action.</span></span> |
| <span data-ttu-id="a5e39-196">resultType</span><span class="sxs-lookup"><span data-stu-id="a5e39-196">resultType</span></span> |<span data-ttu-id="a5e39-197">Az eredmény típusú eg.</span><span class="sxs-lookup"><span data-stu-id="a5e39-197">The type of the result, eg.</span></span> <span data-ttu-id="a5e39-198">Sikeres, sikertelen, kezdő</span><span class="sxs-lookup"><span data-stu-id="a5e39-198">Success, Failure, Start</span></span> |
| <span data-ttu-id="a5e39-199">resultSignature</span><span class="sxs-lookup"><span data-stu-id="a5e39-199">resultSignature</span></span> |<span data-ttu-id="a5e39-200">Az erőforrástípus függ.</span><span class="sxs-lookup"><span data-stu-id="a5e39-200">Depends on the resource type.</span></span> |
| <span data-ttu-id="a5e39-201">durationMs</span><span class="sxs-lookup"><span data-stu-id="a5e39-201">durationMs</span></span> |<span data-ttu-id="a5e39-202">A művelet ezredmásodpercben időtartama</span><span class="sxs-lookup"><span data-stu-id="a5e39-202">Duration of the operation in milliseconds</span></span> |
| <span data-ttu-id="a5e39-203">callerIpAddress</span><span class="sxs-lookup"><span data-stu-id="a5e39-203">callerIpAddress</span></span> |<span data-ttu-id="a5e39-204">Felhasználó rendelkezik a művelet, a jogcím vagy a rendelkezésre állás alapján SPN jogcím IP-címe.</span><span class="sxs-lookup"><span data-stu-id="a5e39-204">IP address of the user who has performed the operation, UPN claim, or SPN claim based on availability.</span></span> |
| <span data-ttu-id="a5e39-205">correlationId</span><span class="sxs-lookup"><span data-stu-id="a5e39-205">correlationId</span></span> |<span data-ttu-id="a5e39-206">Általában egy GUID Azonosítót a karakterlánc formátuma.</span><span class="sxs-lookup"><span data-stu-id="a5e39-206">Usually a GUID in the string format.</span></span> <span data-ttu-id="a5e39-207">Az eseményeket, amelyek megosztása a correlationId ugyanazon uber művelet tartozik.</span><span class="sxs-lookup"><span data-stu-id="a5e39-207">Events that share a correlationId belong to the same uber action.</span></span> |
| <span data-ttu-id="a5e39-208">identity</span><span class="sxs-lookup"><span data-stu-id="a5e39-208">identity</span></span> |<span data-ttu-id="a5e39-209">Az engedélyezési és a jogcímek leíró JSON-blob.</span><span class="sxs-lookup"><span data-stu-id="a5e39-209">JSON blob describing the authorization and claims.</span></span> |
| <span data-ttu-id="a5e39-210">Engedélyezési</span><span class="sxs-lookup"><span data-stu-id="a5e39-210">authorization</span></span> |<span data-ttu-id="a5e39-211">Az esemény tulajdonságainak RBAC BLOB.</span><span class="sxs-lookup"><span data-stu-id="a5e39-211">Blob of RBAC properties of the event.</span></span> <span data-ttu-id="a5e39-212">Az "action", "szerepkör" és "hatókör" Tulajdonságok általában tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="a5e39-212">Usually includes the “action”, “role” and “scope” properties.</span></span> |
| <span data-ttu-id="a5e39-213">szint</span><span class="sxs-lookup"><span data-stu-id="a5e39-213">level</span></span> |<span data-ttu-id="a5e39-214">Az esemény szintje.</span><span class="sxs-lookup"><span data-stu-id="a5e39-214">Level of the event.</span></span> <span data-ttu-id="a5e39-215">A következő értékek egyikét: "Kritikus", "Error", "Figyelmeztetés", "Tájékoztató" és "Részletes"</span><span class="sxs-lookup"><span data-stu-id="a5e39-215">One of the following values: “Critical”, “Error”, “Warning”, “Informational” and “Verbose”</span></span> |
| <span data-ttu-id="a5e39-216">location</span><span class="sxs-lookup"><span data-stu-id="a5e39-216">location</span></span> |<span data-ttu-id="a5e39-217">A hely történt (vagy globális) régióban.</span><span class="sxs-lookup"><span data-stu-id="a5e39-217">Region in which the location occurred (or global).</span></span> |
| <span data-ttu-id="a5e39-218">properties</span><span class="sxs-lookup"><span data-stu-id="a5e39-218">properties</span></span> |<span data-ttu-id="a5e39-219">Állítsa be a `<Key, Value>` az esemény részleteit leíró párok (azaz szótárában).</span><span class="sxs-lookup"><span data-stu-id="a5e39-219">Set of `<Key, Value>` pairs (i.e. Dictionary) describing the details of the event.</span></span> |

> [!NOTE]
> <span data-ttu-id="a5e39-220">A tulajdonságok és ezek a tulajdonságok használatát eltérőek lehetnek attól függően, hogy az erőforrás.</span><span class="sxs-lookup"><span data-stu-id="a5e39-220">The properties and usage of those properties can vary depending on the resource.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="a5e39-221">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a5e39-221">Next steps</span></span>
* [<span data-ttu-id="a5e39-222">Elemzés blobok letöltése</span><span class="sxs-lookup"><span data-stu-id="a5e39-222">Download blobs for analysis</span></span>](../storage/blobs/storage-dotnet-how-to-use-blobs.md#download-blobs)
* [<span data-ttu-id="a5e39-223">Az Event hubs tevékenységnapló adatfolyam</span><span class="sxs-lookup"><span data-stu-id="a5e39-223">Stream the Activity Log to Event Hubs</span></span>](monitoring-stream-activity-logs-event-hubs.md)
* [<span data-ttu-id="a5e39-224">További tudnivalók a műveletnapló</span><span class="sxs-lookup"><span data-stu-id="a5e39-224">Read more about the Activity Log</span></span>](monitoring-overview-activity-logs.md)

