---
title: "aaaArchive hello Azure tevékenységnapló |} Microsoft Docs"
description: "Ismerje meg, hogyan tooarchive az Azure tevékenység keresse meg a hosszú távú megőrzési tárfiókokban."
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
ms.openlocfilehash: 58c6d3a3a31398287f66f76999d48f2942ab5109
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="archive-hello-azure-activity-log"></a><span data-ttu-id="49056-103">Archív hello Azure tevékenységnapló</span><span class="sxs-lookup"><span data-stu-id="49056-103">Archive hello Azure Activity Log</span></span>
<span data-ttu-id="49056-104">Ebben a cikkben megmutatjuk, hogyan használhatja a hello Azure-portálon, a PowerShell-parancsmagokkal vagy a platformfüggetlen parancssori felület tooarchive a [ **Azure tevékenységnapló** ](monitoring-overview-activity-logs.md) tárfiókokban.</span><span class="sxs-lookup"><span data-stu-id="49056-104">In this article, we show how you can use hello Azure portal, PowerShell Cmdlets, or Cross-Platform CLI tooarchive your [**Azure Activity Log**](monitoring-overview-activity-logs.md) in a storage account.</span></span> <span data-ttu-id="49056-105">Ez a beállítás akkor hasznos, ha azt szeretné, hogy a hosszabb, mint 90 napra (a teljes hozzáféréssel hello adatmegőrzési) tevékenységnapló naplózásához statikus elemzés, vagy biztonsági mentési tooretain.</span><span class="sxs-lookup"><span data-stu-id="49056-105">This option is useful if you would like tooretain your Activity Log longer than 90 days (with full control over hello retention policy) for audit, static analysis, or backup.</span></span> <span data-ttu-id="49056-106">Ha csak tooretain 90 napig, vagy kevesebb, mint az események nem kell tooset archiválási tooa storage-fiók mentése óta tevékenységnapló események kerülnek be az Azure platformon hello 90 napig engedélyezése archiválás nélkül.</span><span class="sxs-lookup"><span data-stu-id="49056-106">If you only need tooretain your events for 90 days or less you do not need tooset up archival tooa storage account, since Activity Log events are retained in hello Azure platform for 90 days without enabling archival.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="49056-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="49056-107">Prerequisites</span></span>
<span data-ttu-id="49056-108">Kezdés előtt kell túl[hozzon létre egy tárfiókot](../storage/common/storage-create-storage-account.md#create-a-storage-account) úgy archiválhatók a tevékenységnapló toowhich.</span><span class="sxs-lookup"><span data-stu-id="49056-108">Before you begin, you need too[create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account) toowhich you can archive your Activity Log.</span></span> <span data-ttu-id="49056-109">Erősen ajánlott, hogy nem használja a benne tárolt, így jobban szabályozhatja a hozzáférést toomonitoring adatok más, nem figyelési adatokat tartalmazó meglévő tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="49056-109">We highly recommend that you do not use an existing storage account that has other, non-monitoring data stored in it so that you can better control access toomonitoring data.</span></span> <span data-ttu-id="49056-110">Azonban is archiválás diagnosztikai naplók és a metrikák tooa tárfiókot, ha azt is ellenőrizze logika toouse, hogy a tárfiók a tevékenység-e jelentkezni, valamint tookeep összes figyelési adatok egy központi helyen.</span><span class="sxs-lookup"><span data-stu-id="49056-110">However, if you are also archiving Diagnostic Logs and metrics tooa storage account, it may make sense toouse that storage account for your Activity Log as well tookeep all monitoring data in a central location.</span></span> <span data-ttu-id="49056-111">hello tárfiókot használ egy általános célú tárfiókkal, nem a blob storage-fiók kell lennie.</span><span class="sxs-lookup"><span data-stu-id="49056-111">hello storage account you use must be a general purpose storage account, not a blob storage account.</span></span> <span data-ttu-id="49056-112">hello storage-fiók nem rendelkezik toobe hello hello előfizetési naplók kibocsátó mindaddig, amíg hello beállítás konfiguráló hello felhasználó rendelkezik-e megfelelő hozzáférési tooboth előfizetéseket a Szerepalapú tárolóként ugyanazt az előfizetést.</span><span class="sxs-lookup"><span data-stu-id="49056-112">hello storage account does not have toobe in hello same subscription as hello subscription emitting logs as long as hello user who configures hello setting has appropriate RBAC access tooboth subscriptions.</span></span>

## <a name="log-profile"></a><span data-ttu-id="49056-113">Napló-profil</span><span class="sxs-lookup"><span data-stu-id="49056-113">Log Profile</span></span>
<span data-ttu-id="49056-114">hello beállítása tooarchive hello hello módszereket az alábbi tevékenységnapló, **napló profil** az előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="49056-114">tooarchive hello Activity Log using any of hello methods below, you set hello **Log Profile** for a subscription.</span></span> <span data-ttu-id="49056-115">hello napló profil meghatározza az eseményeket, amelyek tárolt vagy a folyamatos átviteli hello típusát és kimenetek hello – tárolási fiók és/vagy az event hub.</span><span class="sxs-lookup"><span data-stu-id="49056-115">hello Log Profile defines hello type of events that are stored or streamed and hello outputs—storage account and/or event hub.</span></span> <span data-ttu-id="49056-116">A storage-fiókban tárolt események hello adatmegőrzési (tooretain napok száma) is meghatároz.</span><span class="sxs-lookup"><span data-stu-id="49056-116">It also defines hello retention policy (number of days tooretain) for events stored in a storage account.</span></span> <span data-ttu-id="49056-117">Ha hello adatmegőrzési toozero, események határozatlan ideig tárolja.</span><span class="sxs-lookup"><span data-stu-id="49056-117">If hello retention policy is set toozero, events are stored indefinitely.</span></span> <span data-ttu-id="49056-118">Ellenkező esetben ez állítható tooany értékének 1 és 2147483647 között.</span><span class="sxs-lookup"><span data-stu-id="49056-118">Otherwise, this can be set tooany value between 1 and 2147483647.</span></span> <span data-ttu-id="49056-119">Adatmegőrzési alkalmazott napi, így: hello napi (UTC) szerint végét naplózza, amelyik most már túl hello adatmegőrzési hello nap törlődni fog.</span><span class="sxs-lookup"><span data-stu-id="49056-119">Retention policies are applied per-day, so at hello end of a day (UTC), logs from hello day that is now beyond hello retention policy will be deleted.</span></span> <span data-ttu-id="49056-120">Például ha egy nap adatmegőrzési, ma hello nap hello elején hello hello nap előtt tegnapi naplók törlése akkor történik meg.</span><span class="sxs-lookup"><span data-stu-id="49056-120">For example, if you had a retention policy of one day, at hello beginning of hello day today hello logs from hello day before yesterday would be deleted.</span></span> <span data-ttu-id="49056-121">[Részletesebb naplófájl kapcsolatos profilok Itt](monitoring-overview-activity-logs.md#export-the-activity-log-with-a-log-profile).</span><span class="sxs-lookup"><span data-stu-id="49056-121">[You can read more about log profiles here](monitoring-overview-activity-logs.md#export-the-activity-log-with-a-log-profile).</span></span> 

## <a name="archive-hello-activity-log-using-hello-portal"></a><span data-ttu-id="49056-122">Archív hello tevékenységnapló hello portál használatával</span><span class="sxs-lookup"><span data-stu-id="49056-122">Archive hello Activity Log using hello portal</span></span>
1. <span data-ttu-id="49056-123">Hello portálon kattintson hello **tevékenységnapló** hello bal oldali navigációs hivatkozásra kattintva.</span><span class="sxs-lookup"><span data-stu-id="49056-123">In hello portal, click hello **Activity Log** link on hello left-side navigation.</span></span> <span data-ttu-id="49056-124">Ha egy hivatkozást a hello tevékenységnapló nem látható, kattintson a hello **több szolgáltatások** először hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="49056-124">If you don’t see a link for hello Activity Log, click hello **More Services** link first.</span></span>
   
    ![Keresse meg a tooActivity napló panel](media/monitoring-archive-activity-log/act-log-portal-navigate.png)
2. <span data-ttu-id="49056-126">Hello hello panel felső részén kattintson **exportálása**.</span><span class="sxs-lookup"><span data-stu-id="49056-126">At hello top of hello blade, click **Export**.</span></span>
   
    ![Hello Exportálás gombra.](media/monitoring-archive-activity-log/act-log-portal-export-button.png)
3. <span data-ttu-id="49056-128">Hello paneljén megjelenő jelölőnégyzetet hello a **tooa tárfiók exportálása** , és válasszon egy tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="49056-128">In hello blade that appears, check hello box for **Export tooa storage account** and select a storage account.</span></span>
   
    ![A storage-fiók beállítása](media/monitoring-archive-activity-log/act-log-portal-export-blade.png)
4. <span data-ttu-id="49056-130">A hello csúszkát vagy szövegmező definiálja, amelynek tevékenységnapló események kell tartani a tárfiókban lévő napok számát.</span><span class="sxs-lookup"><span data-stu-id="49056-130">Using hello slider or text box, define a number of days for which Activity Log events should be kept in your storage account.</span></span> <span data-ttu-id="49056-131">Az adatok határozatlan ideig őrzi hello tárfiók toohave tetszés szerint állítsa be az a szám toozero.</span><span class="sxs-lookup"><span data-stu-id="49056-131">If you prefer toohave your data persisted in hello storage account indefinitely, set this number toozero.</span></span>
5. <span data-ttu-id="49056-132">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="49056-132">Click **Save**.</span></span>

## <a name="archive-hello-activity-log-via-powershell"></a><span data-ttu-id="49056-133">Archív hello tevékenységnapló PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="49056-133">Archive hello Activity Log via PowerShell</span></span>
```
Add-AzureRmLogProfile -Name my_log_profile -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus -RetentionInDays 180 -Categories Write,Delete,Action
```

| <span data-ttu-id="49056-134">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="49056-134">Property</span></span> | <span data-ttu-id="49056-135">Szükséges</span><span class="sxs-lookup"><span data-stu-id="49056-135">Required</span></span> | <span data-ttu-id="49056-136">Leírás</span><span class="sxs-lookup"><span data-stu-id="49056-136">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="49056-137">StorageAccountId</span><span class="sxs-lookup"><span data-stu-id="49056-137">StorageAccountId</span></span> |<span data-ttu-id="49056-138">Nem</span><span class="sxs-lookup"><span data-stu-id="49056-138">No</span></span> |<span data-ttu-id="49056-139">Erőforrás-azonosítója hello Tárfiók toowhich tevékenységi naplóit, mentésére.</span><span class="sxs-lookup"><span data-stu-id="49056-139">Resource ID of hello Storage Account toowhich Activity Logs should be saved.</span></span> |
| <span data-ttu-id="49056-140">Helyek</span><span class="sxs-lookup"><span data-stu-id="49056-140">Locations</span></span> |<span data-ttu-id="49056-141">Igen</span><span class="sxs-lookup"><span data-stu-id="49056-141">Yes</span></span> |<span data-ttu-id="49056-142">Régiók, amelynek szeretné toocollect tevékenységnapló események vesszővel tagolt listája.</span><span class="sxs-lookup"><span data-stu-id="49056-142">Comma-separated list of regions for which you would like toocollect Activity Log events.</span></span> <span data-ttu-id="49056-143">Megtekintheti az összes régiók listáját [ezen a lapon felkeresésével](https://azure.microsoft.com/en-us/regions) vagy használatával [hello Azure szolgáltatásfelügyelet REST API](https://msdn.microsoft.com/library/azure/gg441293.aspx).</span><span class="sxs-lookup"><span data-stu-id="49056-143">You can view a list of all regions [by visiting this page](https://azure.microsoft.com/en-us/regions) or by using [hello Azure Management REST API](https://msdn.microsoft.com/library/azure/gg441293.aspx).</span></span> |
| <span data-ttu-id="49056-144">retentionInDays</span><span class="sxs-lookup"><span data-stu-id="49056-144">RetentionInDays</span></span> |<span data-ttu-id="49056-145">Igen</span><span class="sxs-lookup"><span data-stu-id="49056-145">Yes</span></span> |<span data-ttu-id="49056-146">Az eseményeket meg kell őrizni, 1 és 2147483647 közötti napok számát.</span><span class="sxs-lookup"><span data-stu-id="49056-146">Number of days for which events should be retained, between 1 and 2147483647.</span></span> <span data-ttu-id="49056-147">A nulla érték hello naplók határozatlan ideig tárolja (végtelen).</span><span class="sxs-lookup"><span data-stu-id="49056-147">A value of zero stores hello logs indefinitely (forever).</span></span> |
| <span data-ttu-id="49056-148">Kategóriák</span><span class="sxs-lookup"><span data-stu-id="49056-148">Categories</span></span> |<span data-ttu-id="49056-149">Igen</span><span class="sxs-lookup"><span data-stu-id="49056-149">Yes</span></span> |<span data-ttu-id="49056-150">Be kell esemény kategóriák vesszővel tagolt listája.</span><span class="sxs-lookup"><span data-stu-id="49056-150">Comma-separated list of event categories that should be collected.</span></span> <span data-ttu-id="49056-151">Lehetséges értékek a következők: Olvasás, törlés és művelet.</span><span class="sxs-lookup"><span data-stu-id="49056-151">Possible values are Write, Delete, and Action.</span></span> |

## <a name="archive-hello-activity-log-via-cli"></a><span data-ttu-id="49056-152">Archív hello tevékenységnapló parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="49056-152">Archive hello Activity Log via CLI</span></span>
```
azure insights logprofile add --name my_log_profile --storageId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/my_storage --locations global,westus,eastus,northeurope --retentionInDays 180 –categories Write,Delete,Action
```

| <span data-ttu-id="49056-153">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="49056-153">Property</span></span> | <span data-ttu-id="49056-154">Szükséges</span><span class="sxs-lookup"><span data-stu-id="49056-154">Required</span></span> | <span data-ttu-id="49056-155">Leírás</span><span class="sxs-lookup"><span data-stu-id="49056-155">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="49056-156">név</span><span class="sxs-lookup"><span data-stu-id="49056-156">name</span></span> |<span data-ttu-id="49056-157">Igen</span><span class="sxs-lookup"><span data-stu-id="49056-157">Yes</span></span> |<span data-ttu-id="49056-158">A napló profil neve.</span><span class="sxs-lookup"><span data-stu-id="49056-158">Name of your log profile.</span></span> |
| <span data-ttu-id="49056-159">storageId</span><span class="sxs-lookup"><span data-stu-id="49056-159">storageId</span></span> |<span data-ttu-id="49056-160">Nem</span><span class="sxs-lookup"><span data-stu-id="49056-160">No</span></span> |<span data-ttu-id="49056-161">Erőforrás-azonosítója hello Tárfiók toowhich tevékenységi naplóit, mentésére.</span><span class="sxs-lookup"><span data-stu-id="49056-161">Resource ID of hello Storage Account toowhich Activity Logs should be saved.</span></span> |
| <span data-ttu-id="49056-162">Helyek</span><span class="sxs-lookup"><span data-stu-id="49056-162">locations</span></span> |<span data-ttu-id="49056-163">Igen</span><span class="sxs-lookup"><span data-stu-id="49056-163">Yes</span></span> |<span data-ttu-id="49056-164">Régiók, amelynek szeretné toocollect tevékenységnapló események vesszővel tagolt listája.</span><span class="sxs-lookup"><span data-stu-id="49056-164">Comma-separated list of regions for which you would like toocollect Activity Log events.</span></span> <span data-ttu-id="49056-165">Megtekintheti az összes régiók listáját [ezen a lapon felkeresésével](https://azure.microsoft.com/en-us/regions) vagy használatával [hello Azure szolgáltatásfelügyelet REST API](https://msdn.microsoft.com/library/azure/gg441293.aspx).</span><span class="sxs-lookup"><span data-stu-id="49056-165">You can view a list of all regions [by visiting this page](https://azure.microsoft.com/en-us/regions) or by using [hello Azure Management REST API](https://msdn.microsoft.com/library/azure/gg441293.aspx).</span></span> |
| <span data-ttu-id="49056-166">retentionInDays</span><span class="sxs-lookup"><span data-stu-id="49056-166">retentionInDays</span></span> |<span data-ttu-id="49056-167">Igen</span><span class="sxs-lookup"><span data-stu-id="49056-167">Yes</span></span> |<span data-ttu-id="49056-168">Az eseményeket meg kell őrizni, 1 és 2147483647 közötti napok számát.</span><span class="sxs-lookup"><span data-stu-id="49056-168">Number of days for which events should be retained, between 1 and 2147483647.</span></span> <span data-ttu-id="49056-169">A nulla érték határozatlan ideig tárolandó hello naplók (végtelen).</span><span class="sxs-lookup"><span data-stu-id="49056-169">A value of zero will store hello logs indefinitely (forever).</span></span> |
| <span data-ttu-id="49056-170">Kategóriák</span><span class="sxs-lookup"><span data-stu-id="49056-170">categories</span></span> |<span data-ttu-id="49056-171">Igen</span><span class="sxs-lookup"><span data-stu-id="49056-171">Yes</span></span> |<span data-ttu-id="49056-172">Be kell esemény kategóriák vesszővel tagolt listája.</span><span class="sxs-lookup"><span data-stu-id="49056-172">Comma-separated list of event categories that should be collected.</span></span> <span data-ttu-id="49056-173">Lehetséges értékek a következők: Olvasás, törlés és művelet.</span><span class="sxs-lookup"><span data-stu-id="49056-173">Possible values are Write, Delete, and Action.</span></span> |

## <a name="storage-schema-of-hello-activity-log"></a><span data-ttu-id="49056-174">Hello tevékenységnapló tároló sémája</span><span class="sxs-lookup"><span data-stu-id="49056-174">Storage schema of hello Activity Log</span></span>
<span data-ttu-id="49056-175">Miután állított be archiválási, tárolót létrejön hello tárfiókot, amint tevékenységnapló esemény bekövetkezésekor.</span><span class="sxs-lookup"><span data-stu-id="49056-175">Once you have set up archival, a storage container will be created in hello storage account as soon as an Activity Log event occurs.</span></span> <span data-ttu-id="49056-176">hello blobok hello tárolóban kövesse hello hello tevékenységnapló és diagnosztikai naplók formátuma azonos.</span><span class="sxs-lookup"><span data-stu-id="49056-176">hello blobs within hello container follow hello same format across hello Activity Log and Diagnostic Logs.</span></span> <span data-ttu-id="49056-177">a blobok hello szerkezete van:</span><span class="sxs-lookup"><span data-stu-id="49056-177">hello structure of these blobs is:</span></span>

> <span data-ttu-id="49056-178">insights-működési-naplók/name = alapértelmezett/resourceId = / SUBSCRIPTIONS / {előfizetés-azonosító} / y = {négyjegyű numerikus year} / m = {kétjegyű numerikus month} / d {kétjegyű számozott napja} = / h = {kétjegyű 24 órás hour}/m=00/PT1H.json</span><span class="sxs-lookup"><span data-stu-id="49056-178">insights-operational-logs/name=default/resourceId=/SUBSCRIPTIONS/{subscription ID}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json</span></span>
> 
> 

<span data-ttu-id="49056-179">A blob neve lehet, például:</span><span class="sxs-lookup"><span data-stu-id="49056-179">For example, a blob name might be:</span></span>

> <span data-ttu-id="49056-180">insights-Operational-Logs/Name=default/resourceId=/Subscriptions/s1id1234-5679-0123-4567-890123456789/y=2016/m=08/d=22/h=18/m=00/PT1H.JSON</span><span class="sxs-lookup"><span data-stu-id="49056-180">insights-operational-logs/name=default/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/y=2016/m=08/d=22/h=18/m=00/PT1H.json</span></span>
> 
> 

<span data-ttu-id="49056-181">Minden egyes PT1H.json blob tartalmazza a hello blob URL-címben megadott hello órán belül előforduló eseményeket JSON blob (pl. h = 12).</span><span class="sxs-lookup"><span data-stu-id="49056-181">Each PT1H.json blob contains a JSON blob of events that occurred within hello hour specified in hello blob URL (e.g. h=12).</span></span> <span data-ttu-id="49056-182">Hello jelen óra, során eseményeket hozzáfűzött toohello PT1H.json fájlt is, azok bekövetkezésekor.</span><span class="sxs-lookup"><span data-stu-id="49056-182">During hello present hour, events are appended toohello PT1H.json file as they occur.</span></span> <span data-ttu-id="49056-183">hello perc (m = 00) mindig 00, mivel tevékenységnapló események óránként egyes blobok van felosztva.</span><span class="sxs-lookup"><span data-stu-id="49056-183">hello minute value (m=00) is always 00, since Activity Log events are broken into individual blobs per hour.</span></span>

<span data-ttu-id="49056-184">Hello PT1H.json fájlban tárolja minden esemény hello "rekordok" tömb, a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="49056-184">Within hello PT1H.json file, each event is stored in hello “records” array, following this format:</span></span>

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


| <span data-ttu-id="49056-185">Elem neve</span><span class="sxs-lookup"><span data-stu-id="49056-185">Element name</span></span> | <span data-ttu-id="49056-186">Leírás</span><span class="sxs-lookup"><span data-stu-id="49056-186">Description</span></span> |
| --- | --- |
| <span data-ttu-id="49056-187">time</span><span class="sxs-lookup"><span data-stu-id="49056-187">time</span></span> |<span data-ttu-id="49056-188">Hello esemény lett létrehozva, amikor hello Azure szolgáltatás feldolgozása hello idejét jelző időbélyegző megfelelő hello esemény kérelmet.</span><span class="sxs-lookup"><span data-stu-id="49056-188">Timestamp when hello event was generated by hello Azure service processing hello request corresponding hello event.</span></span> |
| <span data-ttu-id="49056-189">resourceId</span><span class="sxs-lookup"><span data-stu-id="49056-189">resourceId</span></span> |<span data-ttu-id="49056-190">Erőforrás-azonosítója, hello érintett erőforrás.</span><span class="sxs-lookup"><span data-stu-id="49056-190">Resource ID of hello impacted resource.</span></span> |
| <span data-ttu-id="49056-191">operationName</span><span class="sxs-lookup"><span data-stu-id="49056-191">operationName</span></span> |<span data-ttu-id="49056-192">Hello művelet neve.</span><span class="sxs-lookup"><span data-stu-id="49056-192">Name of hello operation.</span></span> |
| <span data-ttu-id="49056-193">category</span><span class="sxs-lookup"><span data-stu-id="49056-193">category</span></span> |<span data-ttu-id="49056-194">Kategória hello művelet, például.</span><span class="sxs-lookup"><span data-stu-id="49056-194">Category of hello action, eg.</span></span> <span data-ttu-id="49056-195">Írási, olvassa el, a műveletet.</span><span class="sxs-lookup"><span data-stu-id="49056-195">Write, Read, Action.</span></span> |
| <span data-ttu-id="49056-196">resultType</span><span class="sxs-lookup"><span data-stu-id="49056-196">resultType</span></span> |<span data-ttu-id="49056-197">eg hello hello eredményének típusa.</span><span class="sxs-lookup"><span data-stu-id="49056-197">hello type of hello result, eg.</span></span> <span data-ttu-id="49056-198">Sikeres, sikertelen, kezdő</span><span class="sxs-lookup"><span data-stu-id="49056-198">Success, Failure, Start</span></span> |
| <span data-ttu-id="49056-199">resultSignature</span><span class="sxs-lookup"><span data-stu-id="49056-199">resultSignature</span></span> |<span data-ttu-id="49056-200">Hello erőforrás típusától függ.</span><span class="sxs-lookup"><span data-stu-id="49056-200">Depends on hello resource type.</span></span> |
| <span data-ttu-id="49056-201">durationMs</span><span class="sxs-lookup"><span data-stu-id="49056-201">durationMs</span></span> |<span data-ttu-id="49056-202">Ezredmásodpercben hello művelet időtartama</span><span class="sxs-lookup"><span data-stu-id="49056-202">Duration of hello operation in milliseconds</span></span> |
| <span data-ttu-id="49056-203">callerIpAddress</span><span class="sxs-lookup"><span data-stu-id="49056-203">callerIpAddress</span></span> |<span data-ttu-id="49056-204">IP-cím hello felhasználó hello művelet, a jogcím vagy a rendelkezésre állás alapján SPN jogcím hajtott végre.</span><span class="sxs-lookup"><span data-stu-id="49056-204">IP address of hello user who has performed hello operation, UPN claim, or SPN claim based on availability.</span></span> |
| <span data-ttu-id="49056-205">correlationId</span><span class="sxs-lookup"><span data-stu-id="49056-205">correlationId</span></span> |<span data-ttu-id="49056-206">Általában egy GUID Azonosítót a hello karakterlánc-formátum.</span><span class="sxs-lookup"><span data-stu-id="49056-206">Usually a GUID in hello string format.</span></span> <span data-ttu-id="49056-207">Az eseményeket, amelyek megosztása a correlationId tartozik toohello uber műveletet.</span><span class="sxs-lookup"><span data-stu-id="49056-207">Events that share a correlationId belong toohello same uber action.</span></span> |
| <span data-ttu-id="49056-208">identity</span><span class="sxs-lookup"><span data-stu-id="49056-208">identity</span></span> |<span data-ttu-id="49056-209">JSON-blob: hello engedélyezési és a jogcímeket.</span><span class="sxs-lookup"><span data-stu-id="49056-209">JSON blob describing hello authorization and claims.</span></span> |
| <span data-ttu-id="49056-210">Engedélyezési</span><span class="sxs-lookup"><span data-stu-id="49056-210">authorization</span></span> |<span data-ttu-id="49056-211">A BLOB RBAC tulajdonságok hello esemény.</span><span class="sxs-lookup"><span data-stu-id="49056-211">Blob of RBAC properties of hello event.</span></span> <span data-ttu-id="49056-212">Hello "action", "szerepkör" és "hatókör" Tulajdonságok általában tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="49056-212">Usually includes hello “action”, “role” and “scope” properties.</span></span> |
| <span data-ttu-id="49056-213">szint</span><span class="sxs-lookup"><span data-stu-id="49056-213">level</span></span> |<span data-ttu-id="49056-214">Hello esemény szintje.</span><span class="sxs-lookup"><span data-stu-id="49056-214">Level of hello event.</span></span> <span data-ttu-id="49056-215">Hello a következő értékek egyikét: "Kritikus", "Error", "Figyelmeztetés", "Tájékoztató" és "Részletes"</span><span class="sxs-lookup"><span data-stu-id="49056-215">One of hello following values: “Critical”, “Error”, “Warning”, “Informational” and “Verbose”</span></span> |
| <span data-ttu-id="49056-216">location</span><span class="sxs-lookup"><span data-stu-id="49056-216">location</span></span> |<span data-ttu-id="49056-217">A régióban mely hello helyen történt (vagy globális).</span><span class="sxs-lookup"><span data-stu-id="49056-217">Region in which hello location occurred (or global).</span></span> |
| <span data-ttu-id="49056-218">properties</span><span class="sxs-lookup"><span data-stu-id="49056-218">properties</span></span> |<span data-ttu-id="49056-219">Állítsa be a `<Key, Value>` hello esemény részleteinek hello leíró párok (azaz szótárában).</span><span class="sxs-lookup"><span data-stu-id="49056-219">Set of `<Key, Value>` pairs (i.e. Dictionary) describing hello details of hello event.</span></span> |

> [!NOTE]
> <span data-ttu-id="49056-220">hello tulajdonságai és használatának azokat a tulajdonságokat a hello erőforrás függően változhat.</span><span class="sxs-lookup"><span data-stu-id="49056-220">hello properties and usage of those properties can vary depending on hello resource.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="49056-221">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="49056-221">Next steps</span></span>
* [<span data-ttu-id="49056-222">Elemzés blobok letöltése</span><span class="sxs-lookup"><span data-stu-id="49056-222">Download blobs for analysis</span></span>](../storage/blobs/storage-dotnet-how-to-use-blobs.md#download-blobs)
* [<span data-ttu-id="49056-223">Adatfolyam-hello tevékenységnapló tooEvent hubok</span><span class="sxs-lookup"><span data-stu-id="49056-223">Stream hello Activity Log tooEvent Hubs</span></span>](monitoring-stream-activity-logs-event-hubs.md)
* [<span data-ttu-id="49056-224">Tudjon meg többet a hello műveletnapló</span><span class="sxs-lookup"><span data-stu-id="49056-224">Read more about hello Activity Log</span></span>](monitoring-overview-activity-logs.md)

