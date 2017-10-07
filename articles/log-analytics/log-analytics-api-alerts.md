---
title: "aaaUsing OMS napló Analytics riasztási REST API"
description: "hello napló Analytics riasztási REST API lehetővé teszi a toocreate és riasztások kezelése a alkotó Operations Management Suite (OMS) szolgáltatáshoz.  Ez a cikk részletesen hello API és néhány példa a másik műveletet hajt végre."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 628ad256-7181-4a0d-9e68-4ed60c0f3f04
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/12/2017
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 418dc7eb71d6151c6380b8925f1f147a0e13b178
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-alert-rules-in-log-analytics-with-rest-api"></a><span data-ttu-id="a41d5-104">Hozzon létre, és a REST API-val Naplóelemzési riasztási szabályok kezelése</span><span class="sxs-lookup"><span data-stu-id="a41d5-104">Create and manage alert rules in Log Analytics with REST API</span></span>
<span data-ttu-id="a41d5-105">hello napló Analytics riasztási REST API lehetővé teszi a toocreate, és kezelheti a riasztásokat az Operations Management Suite (OMS).</span><span class="sxs-lookup"><span data-stu-id="a41d5-105">hello Log Analytics Alert REST API allows you toocreate and manage alerts in Operations Management Suite (OMS).</span></span>  <span data-ttu-id="a41d5-106">Ez a cikk részletesen hello API és néhány példa a másik műveletet hajt végre.</span><span class="sxs-lookup"><span data-stu-id="a41d5-106">This article provides details of hello API and several examples for performing different operations.</span></span>

<span data-ttu-id="a41d5-107">hello napló Analytics Search REST API RESTful és hello Azure Resource Manager REST API-n keresztül érhető el.</span><span class="sxs-lookup"><span data-stu-id="a41d5-107">hello Log Analytics Search REST API is RESTful and can be accessed via hello Azure Resource Manager REST API.</span></span> <span data-ttu-id="a41d5-108">Ebben a dokumentumban található példák az hello API elérését egy PowerShell parancssori használatával [ARMClient](https://github.com/projectkudu/ARMClient), egy nyílt forráskódú parancssori eszköz, amely leegyszerűsíti a meghívása hello Azure Resource Manager API-val.</span><span class="sxs-lookup"><span data-stu-id="a41d5-108">In this document you will find examples where hello API is accessed from a PowerShell command line using  [ARMClient](https://github.com/projectkudu/ARMClient), an open source command line tool that simplifies invoking hello Azure Resource Manager API.</span></span> <span data-ttu-id="a41d5-109">hello ARMClient és a PowerShell használata sok beállítások tooaccess hello napló Analytics Search API közül.</span><span class="sxs-lookup"><span data-stu-id="a41d5-109">hello use of ARMClient and PowerShell is one of many options tooaccess hello Log Analytics Search API.</span></span> <span data-ttu-id="a41d5-110">Ezekkel az eszközökkel hello Azure Resource Manager RESTful API toomake hívások tooOMS munkaterületek használják, és hajtsa végre a keresési parancsok rajtuk.</span><span class="sxs-lookup"><span data-stu-id="a41d5-110">With these tools you can utilize hello RESTful Azure Resource Manager API toomake calls tooOMS workspaces and perform search commands within them.</span></span> <span data-ttu-id="a41d5-111">hello API kimeneteként keresési eredmények tooyou JSON formátumban, így toouse hello keresési eredmények között számos különböző módon programozott módon.</span><span class="sxs-lookup"><span data-stu-id="a41d5-111">hello API will output search results tooyou in JSON format, allowing you toouse hello search results in many different ways programmatically.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a41d5-112">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="a41d5-112">Prerequisites</span></span>
<span data-ttu-id="a41d5-113">Jelenleg riasztásokat csak hozhatja létre a Naplóelemzési mentett keresés.</span><span class="sxs-lookup"><span data-stu-id="a41d5-113">Currently, alerts can only be created with a saved search in Log Analytics.</span></span>  <span data-ttu-id="a41d5-114">Olvassa el a toohello [napló Search REST API](log-analytics-log-search-api.md) további információt.</span><span class="sxs-lookup"><span data-stu-id="a41d5-114">You can refer toohello [Log Search REST API](log-analytics-log-search-api.md) for more information.</span></span>

## <a name="schedules"></a><span data-ttu-id="a41d5-115">Ütemezések</span><span class="sxs-lookup"><span data-stu-id="a41d5-115">Schedules</span></span>
<span data-ttu-id="a41d5-116">A mentett kereséseket rendelkezhet egy vagy több ütemezés.</span><span class="sxs-lookup"><span data-stu-id="a41d5-116">A saved search can have one or more schedules.</span></span> <span data-ttu-id="a41d5-117">hello ütemezés meghatározása milyen gyakran hello keresési fut, és hello időtartam, mely hello feltételek keresztül azonosítja.</span><span class="sxs-lookup"><span data-stu-id="a41d5-117">hello schedule defines how often hello search is run and hello time interval over which hello criteria is identified.</span></span>
<span data-ttu-id="a41d5-118">Ütemezés a következő táblázat hello hello tulajdonságokkal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="a41d5-118">Schedules have hello properties in hello following table.</span></span>

| <span data-ttu-id="a41d5-119">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="a41d5-119">Property</span></span> | <span data-ttu-id="a41d5-120">Leírás</span><span class="sxs-lookup"><span data-stu-id="a41d5-120">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="a41d5-121">időköz</span><span class="sxs-lookup"><span data-stu-id="a41d5-121">Interval</span></span> |<span data-ttu-id="a41d5-122">Milyen gyakran hello keresési fut.</span><span class="sxs-lookup"><span data-stu-id="a41d5-122">How often hello search is run.</span></span> <span data-ttu-id="a41d5-123">Percben értendő.</span><span class="sxs-lookup"><span data-stu-id="a41d5-123">Measured in minutes.</span></span> |
| <span data-ttu-id="a41d5-124">queryTimeSpan</span><span class="sxs-lookup"><span data-stu-id="a41d5-124">QueryTimeSpan</span></span> |<span data-ttu-id="a41d5-125">mely hello feltételek kiértékelése hello időintervallumban.</span><span class="sxs-lookup"><span data-stu-id="a41d5-125">hello time interval over which hello criteria is evaluated.</span></span> <span data-ttu-id="a41d5-126">Nagyobbnak kell lennie mint tooor időköz.</span><span class="sxs-lookup"><span data-stu-id="a41d5-126">Must be equal tooor greater than Interval.</span></span> <span data-ttu-id="a41d5-127">Percben értendő.</span><span class="sxs-lookup"><span data-stu-id="a41d5-127">Measured in minutes.</span></span> |
| <span data-ttu-id="a41d5-128">Verzió</span><span class="sxs-lookup"><span data-stu-id="a41d5-128">Version</span></span> |<span data-ttu-id="a41d5-129">hello API-verziót használja.</span><span class="sxs-lookup"><span data-stu-id="a41d5-129">hello API version being used.</span></span>  <span data-ttu-id="a41d5-130">Jelenleg ez mindig meg kell too1.</span><span class="sxs-lookup"><span data-stu-id="a41d5-130">Currently, this should always be set too1.</span></span> |

<span data-ttu-id="a41d5-131">Vegye figyelembe például egy esemény lekérdezés Timespan érték 30 perc és 15 perces időközönként.</span><span class="sxs-lookup"><span data-stu-id="a41d5-131">For example, consider an event query with an Interval of 15 minutes and a Timespan of 30 minutes.</span></span> <span data-ttu-id="a41d5-132">Ebben az esetben hello lekérdezés futna 15 percenként, és a riasztás akkor váltódik ki, ha hello feltételek keresztül a 30 perces span tooresolve tootrue továbbra is.</span><span class="sxs-lookup"><span data-stu-id="a41d5-132">In this case, hello query would be run every 15 minutes, and an alert would be triggered if hello criteria continued tooresolve tootrue over a 30 minute span.</span></span>

### <a name="retrieving-schedules"></a><span data-ttu-id="a41d5-133">Ütemezés lekérése</span><span class="sxs-lookup"><span data-stu-id="a41d5-133">Retrieving schedules</span></span>
<span data-ttu-id="a41d5-134">Használjon hello Get metódus tooretrieve összes ütemezés a mentett keresés.</span><span class="sxs-lookup"><span data-stu-id="a41d5-134">Use hello Get method tooretrieve all schedules for a saved search.</span></span>

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search  ID}/schedules?api-version=2015-03-20

<span data-ttu-id="a41d5-135">Használjon hello lekérése egy ütemezés azonosító tooretrieve metódust egy meghatározott ütemezést, a mentett keresés.</span><span class="sxs-lookup"><span data-stu-id="a41d5-135">Use hello Get method with a schedule ID tooretrieve a particular schedule for a saved search.</span></span>

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}?api-version=2015-03-20

<span data-ttu-id="a41d5-136">Az alábbiakban az ütemezett mintát választ.</span><span class="sxs-lookup"><span data-stu-id="a41d5-136">Following is a sample response for a schedule.</span></span>

```json
{
    "value": [{
        "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/MyWorkspace/savedSearches/0f0f4853-17f8-4ed1-9a03-8e888b0d16ec/schedules/a17b53ef-bd70-4ca4-9ead-83b00f2024a8",
        "etag": "W/\"datetime'2016-02-25T20%3A54%3A49.8074679Z'\"",
        "properties": {
            "Interval": 15,
            "QueryTimeSpan": 15
        }
    }]
}
```

### <a name="creating-a-schedule"></a><span data-ttu-id="a41d5-137">Ütemezés létrehozása</span><span class="sxs-lookup"><span data-stu-id="a41d5-137">Creating a schedule</span></span>
<span data-ttu-id="a41d5-138">Hello Put metódust használ egy egyedi ütemezés azonosító toocreate új ütemezést.</span><span class="sxs-lookup"><span data-stu-id="a41d5-138">Use hello Put method with a unique schedule ID toocreate a new schedule.</span></span>  <span data-ttu-id="a41d5-139">Vegye figyelembe, hogy két ütemezések nem rendelkezik hello ugyanazzal az Azonosítóval akkor is, ha különböző társított mentett kereséseket.</span><span class="sxs-lookup"><span data-stu-id="a41d5-139">Note that two schedules cannot have hello same ID even if they are associated with different saved searches.</span></span>  <span data-ttu-id="a41d5-140">Amikor hello OMS konzolban hozna létre egy ütemezést, GUID jön létre hello ütemezési azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="a41d5-140">When you create a schedule in hello OMS console, a GUID is created for hello schedule ID.</span></span>

> [!NOTE]
> <span data-ttu-id="a41d5-141">minden mentett keresések, ütemezéseihez és hello napló Analytics API létre műveletek hello nevét kisbetűs kell lennie.</span><span class="sxs-lookup"><span data-stu-id="a41d5-141">hello name for all saved searches, schedules, and actions created with hello Log Analytics API must be in lowercase.</span></span>

    $scheduleJson = "{'properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/mynewschedule?api-version=2015-03-20 $scheduleJson

### <a name="editing-a-schedule"></a><span data-ttu-id="a41d5-142">Az ütemezés módosítása</span><span class="sxs-lookup"><span data-stu-id="a41d5-142">Editing a schedule</span></span>
<span data-ttu-id="a41d5-143">Meglévő ütemezés azonosítója megegyezik a mentett hello keresni, amelyek ütemezése toomodify hello Put metódust használja.</span><span class="sxs-lookup"><span data-stu-id="a41d5-143">Use hello Put method with an existing schedule ID for hello same saved search toomodify that schedule.</span></span>  <span data-ttu-id="a41d5-144">hello kérelem törzse hello hello etag hello ütemterv tartalmaznia kell.</span><span class="sxs-lookup"><span data-stu-id="a41d5-144">hello body of hello request must include hello etag of hello schedule.</span></span>

      $scheduleJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A49.8074679Z'\""','properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' } }"
      armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/mynewschedule?api-version=2015-03-20 $scheduleJson


### <a name="deleting-schedules"></a><span data-ttu-id="a41d5-145">Ütemezés törlése</span><span class="sxs-lookup"><span data-stu-id="a41d5-145">Deleting schedules</span></span>
<span data-ttu-id="a41d5-146">Egy ütemezés azonosító toodelete ütemezés hello Delete metódust használja.</span><span class="sxs-lookup"><span data-stu-id="a41d5-146">Use hello Delete method with a schedule ID toodelete a schedule.</span></span>

    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}?api-version=2015-03-20


## <a name="actions"></a><span data-ttu-id="a41d5-147">Műveletek</span><span class="sxs-lookup"><span data-stu-id="a41d5-147">Actions</span></span>
<span data-ttu-id="a41d5-148">Ütemezés rendelkezhet több művelet.</span><span class="sxs-lookup"><span data-stu-id="a41d5-148">A schedule can have multiple actions.</span></span> <span data-ttu-id="a41d5-149">Egy műveletet határozhatnak meg például a levél küldése vagy a runbook indítása egy vagy több folyamatok tooperform, vagy azt határozhatnak meg a küszöbérték, amely azt határozza meg, ha a keresési eredmények hello bizonyos feltételeknek megfelelő.</span><span class="sxs-lookup"><span data-stu-id="a41d5-149">An action may define one or more processes tooperform such as sending a mail or starting a runbook, or it may define a threshold that determines when hello results of a search match some criteria.</span></span>  <span data-ttu-id="a41d5-150">Bizonyos műveleteket határozza meg az is, hogy hello folyamatok végrehajtása közben hello küszöbérték elérése.</span><span class="sxs-lookup"><span data-stu-id="a41d5-150">Some actions will define both so that hello processes are performed when hello threshold is met.</span></span>

<span data-ttu-id="a41d5-151">Minden művelet a következő táblázat hello hello jellemzőkkel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="a41d5-151">All actions have hello properties in hello following table.</span></span>  <span data-ttu-id="a41d5-152">Különböző típusú riasztások különböző további tulajdonságokat, amelyek az alábbiakban található rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="a41d5-152">Different types of alerts have different additional properties which are described below.</span></span>

| <span data-ttu-id="a41d5-153">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="a41d5-153">Property</span></span> | <span data-ttu-id="a41d5-154">Leírás</span><span class="sxs-lookup"><span data-stu-id="a41d5-154">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="a41d5-155">Típus</span><span class="sxs-lookup"><span data-stu-id="a41d5-155">Type</span></span> |<span data-ttu-id="a41d5-156">Hello művelet típusa.</span><span class="sxs-lookup"><span data-stu-id="a41d5-156">Type of hello action.</span></span>  <span data-ttu-id="a41d5-157">Jelenleg hello lehetséges értékei a riasztás és Webhook.</span><span class="sxs-lookup"><span data-stu-id="a41d5-157">Currently hello possible values are Alert and Webhook.</span></span> |
| <span data-ttu-id="a41d5-158">Név</span><span class="sxs-lookup"><span data-stu-id="a41d5-158">Name</span></span> |<span data-ttu-id="a41d5-159">Hello riasztás megjelenített neve.</span><span class="sxs-lookup"><span data-stu-id="a41d5-159">Display name for hello alert.</span></span> |
| <span data-ttu-id="a41d5-160">Verzió</span><span class="sxs-lookup"><span data-stu-id="a41d5-160">Version</span></span> |<span data-ttu-id="a41d5-161">hello API-verziót használja.</span><span class="sxs-lookup"><span data-stu-id="a41d5-161">hello API version being used.</span></span>  <span data-ttu-id="a41d5-162">Jelenleg ez mindig meg kell too1.</span><span class="sxs-lookup"><span data-stu-id="a41d5-162">Currently, this should always be set too1.</span></span> |

### <a name="retrieving-actions"></a><span data-ttu-id="a41d5-163">Műveletek végrehajtása</span><span class="sxs-lookup"><span data-stu-id="a41d5-163">Retrieving actions</span></span>
<span data-ttu-id="a41d5-164">Használata hello Get metódus tooretrieve ütemezett összes műveletet.</span><span class="sxs-lookup"><span data-stu-id="a41d5-164">Use hello Get method tooretrieve all actions for a schedule.</span></span>

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search  ID}/schedules/{Schedule ID}/actions?api-version=2015-03-20

<span data-ttu-id="a41d5-165">Használjon hello beolvasása hello művelet azonosítója tooretrieve metódust egy adott művelet egy ütemezéshez.</span><span class="sxs-lookup"><span data-stu-id="a41d5-165">Use hello Get method with hello action ID tooretrieve a particular action for a schedule.</span></span>

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}/actions/{Action ID}?api-version=2015-03-20

### <a name="creating-or-editing-actions"></a><span data-ttu-id="a41d5-166">Létrehozásával vagy szerkesztésével műveletek</span><span class="sxs-lookup"><span data-stu-id="a41d5-166">Creating or editing actions</span></span>
<span data-ttu-id="a41d5-167">Hello Put metódust használja, amely egyedi toohello ütemezés toocreate új művelet azonosítójú művelet.</span><span class="sxs-lookup"><span data-stu-id="a41d5-167">Use hello Put method with an action ID that is unique toohello schedule toocreate a new action.</span></span>  <span data-ttu-id="a41d5-168">Hello OMS konzol egy műveletet hoz létre, amikor egy GUID van hello művelet azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="a41d5-168">When you create an action in hello OMS console, a GUID is for hello action ID.</span></span>

> [!NOTE]
> <span data-ttu-id="a41d5-169">minden mentett keresések, ütemezéseihez és hello napló Analytics API létre műveletek hello nevét kisbetűs kell lennie.</span><span class="sxs-lookup"><span data-stu-id="a41d5-169">hello name for all saved searches, schedules, and actions created with hello Log Analytics API must be in lowercase.</span></span>

<span data-ttu-id="a41d5-170">Egy meglévő művelet azonosítója megegyezik a mentett hello keresni, amelyek ütemezése toomodify hello Put metódust használja.</span><span class="sxs-lookup"><span data-stu-id="a41d5-170">Use hello Put method with an existing action ID for hello same saved search toomodify that schedule.</span></span>  <span data-ttu-id="a41d5-171">hello kérelem törzse hello hello etag hello ütemterv tartalmaznia kell.</span><span class="sxs-lookup"><span data-stu-id="a41d5-171">hello body of hello request must include hello etag of hello schedule.</span></span>

<span data-ttu-id="a41d5-172">hello kérés formátuma új művelet létrehozásához művelettípus függ, ezek a példák hello lentebbi szerepelnek a.</span><span class="sxs-lookup"><span data-stu-id="a41d5-172">hello request format for creating a new action varies by action type so these examples are provided in hello sections below.</span></span>

### <a name="deleting-actions"></a><span data-ttu-id="a41d5-173">Műveletek törlése</span><span class="sxs-lookup"><span data-stu-id="a41d5-173">Deleting actions</span></span>
<span data-ttu-id="a41d5-174">Hello művelet azonosítója toodelete művelet hello Delete metódust használja.</span><span class="sxs-lookup"><span data-stu-id="a41d5-174">Use hello Delete method with hello action ID toodelete an action.</span></span>

    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}/Actions/{Action ID}?api-version=2015-03-20

### <a name="alert-actions"></a><span data-ttu-id="a41d5-175">Értesítési műveletek</span><span class="sxs-lookup"><span data-stu-id="a41d5-175">Alert Actions</span></span>
<span data-ttu-id="a41d5-176">Ütemezés rendelkeznie kell egy műveletet.</span><span class="sxs-lookup"><span data-stu-id="a41d5-176">A Schedule should have one and only one Alert action.</span></span>  <span data-ttu-id="a41d5-177">Értesítési műveletek legalább a következő táblázat hello hello részeiből.</span><span class="sxs-lookup"><span data-stu-id="a41d5-177">Alert actions have one or more of hello sections in hello following table.</span></span>  <span data-ttu-id="a41d5-178">A további részletek az alábbi leírása.</span><span class="sxs-lookup"><span data-stu-id="a41d5-178">Each is described in further detail below.</span></span>

| <span data-ttu-id="a41d5-179">A szakasz</span><span class="sxs-lookup"><span data-stu-id="a41d5-179">Section</span></span> | <span data-ttu-id="a41d5-180">Leírás</span><span class="sxs-lookup"><span data-stu-id="a41d5-180">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="a41d5-181">Küszöbérték</span><span class="sxs-lookup"><span data-stu-id="a41d5-181">Threshold</span></span> |<span data-ttu-id="a41d5-182">Feltételek hello művelet futási idejét.</span><span class="sxs-lookup"><span data-stu-id="a41d5-182">Criteria for when hello action is run.</span></span> |
| <span data-ttu-id="a41d5-183">EmailNotification</span><span class="sxs-lookup"><span data-stu-id="a41d5-183">EmailNotification</span></span> |<span data-ttu-id="a41d5-184">Levélküldés toomultiple címzetteknek.</span><span class="sxs-lookup"><span data-stu-id="a41d5-184">Send mail toomultiple recipients.</span></span> |
| <span data-ttu-id="a41d5-185">Szervizkiszolgáló</span><span class="sxs-lookup"><span data-stu-id="a41d5-185">Remediation</span></span> |<span data-ttu-id="a41d5-186">Elindít egy forgatókönyvet az Azure Automation tooattempt toocorrect azonosított problémát.</span><span class="sxs-lookup"><span data-stu-id="a41d5-186">Start a runbook in Azure Automation tooattempt toocorrect identified issue.</span></span> |

#### <a name="thresholds"></a><span data-ttu-id="a41d5-187">Küszöbértékek</span><span class="sxs-lookup"><span data-stu-id="a41d5-187">Thresholds</span></span>
<span data-ttu-id="a41d5-188">Egy műveletet rendelkeznie kell egy küszöbértéket.</span><span class="sxs-lookup"><span data-stu-id="a41d5-188">An Alert action should have one and only one threshold.</span></span>  <span data-ttu-id="a41d5-189">Amikor hello mentett keresés eredményeit, hogy a keresés társított művelet hello küszöbértéke egyeznek, a művelet az egyéb folyamatok futnak.</span><span class="sxs-lookup"><span data-stu-id="a41d5-189">When hello results of a saved search match hello threshold in an action associated with that search, then any other processes in that action are run.</span></span>  <span data-ttu-id="a41d5-190">Egy műveletet is tartalmazhat, csak a küszöbértéket, hogy más típusú, amelyek nem tartalmazzák a küszöbértékek műveletekhez használható.</span><span class="sxs-lookup"><span data-stu-id="a41d5-190">An action can also contain only a threshold so that it can be used with actions of other types that don’t contain thresholds.</span></span>

<span data-ttu-id="a41d5-191">Küszöbértékek a következő táblázat hello hello tulajdonságokkal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="a41d5-191">Thresholds have hello properties in hello following table.</span></span>

| <span data-ttu-id="a41d5-192">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="a41d5-192">Property</span></span> | <span data-ttu-id="a41d5-193">Leírás</span><span class="sxs-lookup"><span data-stu-id="a41d5-193">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="a41d5-194">Operátor</span><span class="sxs-lookup"><span data-stu-id="a41d5-194">Operator</span></span> |<span data-ttu-id="a41d5-195">Hello küszöbérték összehasonlító operátort.</span><span class="sxs-lookup"><span data-stu-id="a41d5-195">Operator for hello threshold comparison.</span></span> <br> <span data-ttu-id="a41d5-196">gt = nagyobb mint</span><span class="sxs-lookup"><span data-stu-id="a41d5-196">gt = Greater Than</span></span> <br> <span data-ttu-id="a41d5-197">lt = kisebb, mint</span><span class="sxs-lookup"><span data-stu-id="a41d5-197">lt = Less Than</span></span> |
| <span data-ttu-id="a41d5-198">Érték</span><span class="sxs-lookup"><span data-stu-id="a41d5-198">Value</span></span> |<span data-ttu-id="a41d5-199">Hello küszöb értékét.</span><span class="sxs-lookup"><span data-stu-id="a41d5-199">Value for hello threshold.</span></span> |

<span data-ttu-id="a41d5-200">Vegye figyelembe például 15 perc, 30 perc Timespan és egy nagyobb, mint 10 küszöbértéket időközzel eseménylekérdezési.</span><span class="sxs-lookup"><span data-stu-id="a41d5-200">For example, consider an event query with an Interval of 15 minutes, a Timespan of 30 minutes, and a Threshold of greater than 10.</span></span> <span data-ttu-id="a41d5-201">Ebben az esetben hello lekérdezés futna 15 percenként, és a riasztás akkor váltódik ki, ha 10 keresztül a 30 perces span létrehozott események által visszaadott.</span><span class="sxs-lookup"><span data-stu-id="a41d5-201">In this case, hello query would be run every 15 minutes, and an alert would be triggered if it returned 10 events that were created over a 30 minute span.</span></span>

<span data-ttu-id="a41d5-202">Az alábbiakban látható egy minta válasz csak a küszöbérték a művelet.</span><span class="sxs-lookup"><span data-stu-id="a41d5-202">Following is a sample response for an action with only a threshold.</span></span>  

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My threshold action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "Version": 1
    }

<span data-ttu-id="a41d5-203">Egy egyedi művelet azonosítója toocreate új küszöbérték művelet ütemezett hello Put metódust használata.</span><span class="sxs-lookup"><span data-stu-id="a41d5-203">Use hello Put method with a unique action ID toocreate a new threshold action for a schedule.</span></span>  

    $thresholdJson = "{'properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdJson

<span data-ttu-id="a41d5-204">Egy meglévő művelet azonosítója toomodify egy küszöbértéket művelet ütemezett hello Put metódust használata.</span><span class="sxs-lookup"><span data-stu-id="a41d5-204">Use hello Put method with an existing action ID toomodify a threshold action for a schedule.</span></span>  <span data-ttu-id="a41d5-205">hello kérelem törzse hello hello etag hello művelet tartalmaznia kell.</span><span class="sxs-lookup"><span data-stu-id="a41d5-205">hello body of hello request must include hello etag of hello action.</span></span>

    $thresholdJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdJson

#### <a name="email-notification"></a><span data-ttu-id="a41d5-206">E-mail értesítések</span><span class="sxs-lookup"><span data-stu-id="a41d5-206">Email Notification</span></span>
<span data-ttu-id="a41d5-207">Értesítő e-mailek küldése mail tooone vagy a címzetteket.</span><span class="sxs-lookup"><span data-stu-id="a41d5-207">Email Notifications send mail tooone or more recipients.</span></span>  <span data-ttu-id="a41d5-208">A következő táblázat hello tartoznak hello tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="a41d5-208">They include hello properties in hello following table.</span></span>

| <span data-ttu-id="a41d5-209">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="a41d5-209">Property</span></span> | <span data-ttu-id="a41d5-210">Leírás</span><span class="sxs-lookup"><span data-stu-id="a41d5-210">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="a41d5-211">Címzettek</span><span class="sxs-lookup"><span data-stu-id="a41d5-211">Recipients</span></span> |<span data-ttu-id="a41d5-212">E-mail címek listája.</span><span class="sxs-lookup"><span data-stu-id="a41d5-212">List of mail addresses.</span></span> |
| <span data-ttu-id="a41d5-213">Tárgy</span><span class="sxs-lookup"><span data-stu-id="a41d5-213">Subject</span></span> |<span data-ttu-id="a41d5-214">hello hello e-mail tárgyát.</span><span class="sxs-lookup"><span data-stu-id="a41d5-214">hello subject of hello mail.</span></span> |
| <span data-ttu-id="a41d5-215">Melléklet</span><span class="sxs-lookup"><span data-stu-id="a41d5-215">Attachment</span></span> |<span data-ttu-id="a41d5-216">Mellékletek jelenleg nem támogatottak, ezért ez a "None" értéke mindig lesz.</span><span class="sxs-lookup"><span data-stu-id="a41d5-216">Attachments are not currently supported, so this will always have a value of “None”.</span></span> |

<span data-ttu-id="a41d5-217">Az alábbiakban látható egy minta választ, a küszöbérték az e-mail értesítési művelet.</span><span class="sxs-lookup"><span data-stu-id="a41d5-217">Following is a sample response for an email notification action with a threshold.</span></span>  

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My email action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "EmailNotification": {
            "Recipients": [
                "recipient1@contoso.com",
                "recipient2@contoso.com"
            ],
            "Subject": "This is hello subject",
            "Attachment": "None"
        },
        "Version": 1
    }

<span data-ttu-id="a41d5-218">Egy egyedi művelet azonosítója toocreate egy új e-mail művelet ütemezett hello Put metódust használata.</span><span class="sxs-lookup"><span data-stu-id="a41d5-218">Use hello Put method with a unique action ID toocreate a new e-mail action for a schedule.</span></span>  <span data-ttu-id="a41d5-219">hello alábbi példa hoz létre a e-mailben értesítést a küszöbérték, üdvözlő levelek küldi, ha a mentett keresés hello hello eredményeit meghaladnia hello küszöbértéket.</span><span class="sxs-lookup"><span data-stu-id="a41d5-219">hello following example creates an email notification with a threshold so hello mail is sent when hello results of hello saved search exceed hello threshold.</span></span>

    $emailJson = "{'properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is hello subject', 'Attachment':'None'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myemailaction?api-version=2015-03-20 $emailJson

<span data-ttu-id="a41d5-220">Egy meglévő művelet azonosítója toomodify e-mail művelet ütemezett hello Put metódust használata.</span><span class="sxs-lookup"><span data-stu-id="a41d5-220">Use hello Put method with an existing action ID toomodify an e-mail action for a schedule.</span></span>  <span data-ttu-id="a41d5-221">hello kérelem törzse hello hello etag hello művelet tartalmaznia kell.</span><span class="sxs-lookup"><span data-stu-id="a41d5-221">hello body of hello request must include hello etag of hello action.</span></span>

    $emailJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is hello subject', 'Attachment':'None'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myemailaction?api-version=2015-03-20 $emailJson

#### <a name="remediation-actions"></a><span data-ttu-id="a41d5-222">Szervizelési műveletek</span><span class="sxs-lookup"><span data-stu-id="a41d5-222">Remediation actions</span></span>
<span data-ttu-id="a41d5-223">Szervizelt érintő toocorrect hello probléma hello riasztás által azonosított Azure Automation forgatókönyv indítása.</span><span class="sxs-lookup"><span data-stu-id="a41d5-223">Remediations start a runbook in Azure Automation that attempts toocorrect hello problem identified by hello alert.</span></span>  <span data-ttu-id="a41d5-224">Hello runbook egy szervizelési művelet szerepel a webhook létrehozása kell, és adja meg a hello WebhookUri tulajdonság hello URI.</span><span class="sxs-lookup"><span data-stu-id="a41d5-224">You must create a webhook for hello runbook used in a remediation action and then specify hello URI in hello WebhookUri property.</span></span>  <span data-ttu-id="a41d5-225">Ez a művelet hello OMS-konzollal létrehozásakor egy új webhook hello runbook automatikusan létrejön.</span><span class="sxs-lookup"><span data-stu-id="a41d5-225">When you create this action using hello OMS console, a new webhook is automatically created for hello runbook.</span></span>

<span data-ttu-id="a41d5-226">Szervizelt hello a következő táblázat tartalmazza az hello tulajdonságokat.</span><span class="sxs-lookup"><span data-stu-id="a41d5-226">Remediations include hello properties in hello following table.</span></span>

| <span data-ttu-id="a41d5-227">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="a41d5-227">Property</span></span> | <span data-ttu-id="a41d5-228">Leírás</span><span class="sxs-lookup"><span data-stu-id="a41d5-228">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="a41d5-229">RunbookName</span><span class="sxs-lookup"><span data-stu-id="a41d5-229">RunbookName</span></span> |<span data-ttu-id="a41d5-230">Hello runbook neve.</span><span class="sxs-lookup"><span data-stu-id="a41d5-230">Name of hello runbook.</span></span> <span data-ttu-id="a41d5-231">Ennek egyeznie kell a közzétett runbookok hello Automation-megoldás az OMS-munkaterület a konfigurált hello automation-fiókban.</span><span class="sxs-lookup"><span data-stu-id="a41d5-231">This must match a published runbook in hello automation account configured in hello Automation Solution in your OMS workspace.</span></span> |
| <span data-ttu-id="a41d5-232">WebhookUri</span><span class="sxs-lookup"><span data-stu-id="a41d5-232">WebhookUri</span></span> |<span data-ttu-id="a41d5-233">Hello webhook URI Azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="a41d5-233">URI of hello webhook.</span></span> |
| <span data-ttu-id="a41d5-234">A lejárati</span><span class="sxs-lookup"><span data-stu-id="a41d5-234">Expiry</span></span> |<span data-ttu-id="a41d5-235">hello lejárati dátuma és időpontja hello webhook.</span><span class="sxs-lookup"><span data-stu-id="a41d5-235">hello expiration date and time of hello webhook.</span></span>  <span data-ttu-id="a41d5-236">Ha hello webhook egy lejárati nem rendelkezik, majd ez lehet bármely érvényes jövőbeni dátum.</span><span class="sxs-lookup"><span data-stu-id="a41d5-236">If hello webhook doesn’t have an expiration, then this can be any valid future date.</span></span> |

<span data-ttu-id="a41d5-237">Az alábbiakban látható egy mintaválasz szervizelési művelethez a küszöbértéket.</span><span class="sxs-lookup"><span data-stu-id="a41d5-237">Following is a sample response for a remediation action with a threshold.</span></span>

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My remediation action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "Remediation": {
            "RunbookName": "My-Runbook",
            "WebhookUri": "https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d",
            "Expiry": "2018-02-25T18:27:20"
            },
        "Version": 1
    }

<span data-ttu-id="a41d5-238">Egy egyedi művelet azonosítója toocreate egy új szervizelési művelet ütemezett hello Put metódust használata.</span><span class="sxs-lookup"><span data-stu-id="a41d5-238">Use hello Put method with a unique action ID toocreate a new remediation action for a schedule.</span></span>  <span data-ttu-id="a41d5-239">hello alábbi példa hoz létre egy szervizelés a küszöbérték, a mentett keresés hello hello eredményeit-nál nagyobb hello küszöbérték hello runbook indítását.</span><span class="sxs-lookup"><span data-stu-id="a41d5-239">hello following example creates a remediation with a threshold so hello runbook is started when hello results of hello saved search exceed hello threshold.</span></span>

    $remediateJson = "{'properties': { 'Type':'Alert', 'Name': 'My Remediation Action', 'Version':'1', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'Remediation': {'RunbookName': 'My-Runbook', 'WebhookUri':'https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d', 'Expiry':'2018-02-25T18:27:20Z'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myremediationaction?api-version=2015-03-20 $remediateJson

<span data-ttu-id="a41d5-240">Egy meglévő művelet azonosítója toomodify a szervizelési művelet ütemezett hello Put metódust használata.</span><span class="sxs-lookup"><span data-stu-id="a41d5-240">Use hello Put method with an existing action ID toomodify a remediation action for a schedule.</span></span>  <span data-ttu-id="a41d5-241">hello kérelem törzse hello hello etag hello művelet tartalmaznia kell.</span><span class="sxs-lookup"><span data-stu-id="a41d5-241">hello body of hello request must include hello etag of hello action.</span></span>

    $remediateJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Type':'Alert', 'Name': 'My Remediation Action', 'Version':'1', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'Remediation': {'RunbookName': 'My-Runbook', 'WebhookUri':'https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d', 'Expiry':'2018-02-25T18:27:20Z'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myremediationaction?api-version=2015-03-20 $remediateJson

#### <a name="example"></a><span data-ttu-id="a41d5-242">Példa</span><span class="sxs-lookup"><span data-stu-id="a41d5-242">Example</span></span>
<span data-ttu-id="a41d5-243">Az alábbiakban látható egy teljes példa toocreate egy új e-mail-riasztások.</span><span class="sxs-lookup"><span data-stu-id="a41d5-243">Following is a complete example toocreate a new email alert.</span></span>  <span data-ttu-id="a41d5-244">Ezzel létrehoz egy új ütemezést tartalmazó egy küszöbértéket és e-mailek művelet együtt.</span><span class="sxs-lookup"><span data-stu-id="a41d5-244">This creates a new schedule along with an action containing a threshold and email.</span></span>

    $subscriptionId = "3d56705e-5b26-5bcc-9368-dbc8d2fafbfc"
    $resourceGroup  = "MyResourceGroup"    
    $workspaceName    = "MyWorkspace"
    $searchId       = "MySearch"
    $scheduleId     = "MySchedule"
    $thresholdId    = "MyThreshold"
    $actionId       = "MyEmailAction"

    $scheduleJson = "{'properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' }"
    armclient put /subscriptions/$subscriptionId/resourceGroups/$resourceGroup/providers/Microsoft.OperationalInsights/workspaces/$workspaceName/savedSearches/$searchId/schedules/$scheduleId/?api-version=2015-03-20 $scheduleJson

    $emailJson = "{'properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Severity':'Warning', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is hello subject', 'Attachment':'None'} }"
    armclient put /subscriptions/$subscriptionId/resourceGroups/$resourceGroup/providers/Microsoft.OperationalInsights/workspaces/$workspaceName/savedSearches/$searchId/schedules/$scheduleId/actions/$actionId/?api-version=2015-03-20 $emailJson

### <a name="webhook-actions"></a><span data-ttu-id="a41d5-245">Webhookműveletek</span><span class="sxs-lookup"><span data-stu-id="a41d5-245">Webhook actions</span></span>
<span data-ttu-id="a41d5-246">Webhookműveletek egy folyamat megkezdéséhez hívja az egy URL-cím és a nem kötelezően egy hasznos toobe küldött.</span><span class="sxs-lookup"><span data-stu-id="a41d5-246">Webhook actions start a process by calling a URL and optionally providing a payload toobe sent.</span></span>  <span data-ttu-id="a41d5-247">Hasonló tooRemediation műveletek kivételével ezek célja a webhookokkal, előfordulhat, hogy aktiválják az Azure Automation-runbook eltérő folyamatok.</span><span class="sxs-lookup"><span data-stu-id="a41d5-247">They are similar tooRemediation actions except they are meant for webhooks that may invoke processes other than Azure Automation runbooks.</span></span>  <span data-ttu-id="a41d5-248">Hello további lehetőség a tartalom céleszközökre toobe toohello távoli folyamattal is biztosítanak.</span><span class="sxs-lookup"><span data-stu-id="a41d5-248">They also provide hello additional option of providing a payload toobe delivered toohello remote process.</span></span>

<span data-ttu-id="a41d5-249">Webhookműveletek nem rendelkezik a küszöbérték, de ehelyett hozzá kell adni egy riasztási küszöbértéket műveletek tooa ütemezés.</span><span class="sxs-lookup"><span data-stu-id="a41d5-249">Webhook actions do not have a threshold but instead should be added tooa schedule that has an Alert action with a threshold.</span></span>  <span data-ttu-id="a41d5-250">Több webhookműveletek összes futtatni kívánt hello küszöbérték teljesülésekor adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="a41d5-250">You can add multiple Webhook actions that will all be run when hello threshold is met.</span></span>

<span data-ttu-id="a41d5-251">Webhookműveletek hello a következő táblázat tartalmazza az hello tulajdonságokat.</span><span class="sxs-lookup"><span data-stu-id="a41d5-251">Webhook actions include hello properties in hello following table.</span></span>

| <span data-ttu-id="a41d5-252">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="a41d5-252">Property</span></span> | <span data-ttu-id="a41d5-253">Leírás</span><span class="sxs-lookup"><span data-stu-id="a41d5-253">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="a41d5-254">WebhookUri</span><span class="sxs-lookup"><span data-stu-id="a41d5-254">WebhookUri</span></span> |<span data-ttu-id="a41d5-255">hello hello e-mail tárgyát.</span><span class="sxs-lookup"><span data-stu-id="a41d5-255">hello subject of hello mail.</span></span> |
| <span data-ttu-id="a41d5-256">customPayload</span><span class="sxs-lookup"><span data-stu-id="a41d5-256">CustomPayload</span></span> |<span data-ttu-id="a41d5-257">Egyéni adattartalom küldött toobe toohello webhook.</span><span class="sxs-lookup"><span data-stu-id="a41d5-257">Custom payload toobe sent toohello webhook.</span></span>  <span data-ttu-id="a41d5-258">hello formátum függvényében adható meg, milyen hello webhook által várt paraméterekkel.</span><span class="sxs-lookup"><span data-stu-id="a41d5-258">hello format will depend on what hello webhook is expecting.</span></span> |

<span data-ttu-id="a41d5-259">Az alábbiakban látható egy mintaválasz webhook műveletet és egy társított műveletet a küszöbértéket.</span><span class="sxs-lookup"><span data-stu-id="a41d5-259">Following is a sample response for webhook action and an associated alert action with a threshold.</span></span>

    {
        "__metadata": {},
        "value": [
            {
                "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/bwren/savedSearches/2d1b30fb-7f48-4de5-9614-79ee244b52de/schedules/b80f5621-7217-4007-b32d-165d14377093/Actions/72884702-acf9-4653-bb67-f42436b342b4",
                "etag": "W/\"datetime'2016-02-26T20%3A25%3A00.6862124Z'\"",
                "properties": {
                    "Type": "Webhook",
                    "Name": "My Webhook Action",
                    "WebhookUri": "https://oaaswebhookdf.cloudapp.net/webhooks?token=VfkYTIlpk%2fc%2bJBP",
                    "CustomPayload": "{\"fielld1\":\"value1\",\"field2\":\"value2\"}",
                    "Version": 1
                }
            },
            {
                "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/bwren/savedSearches/2d1b30fb-7f48-4de5-9614-79ee244b52de/schedules/b80f5621-7217-4007-b32d-165d14377093/Actions/90a27cf8-71b7-4df2-b04f-54ed01f1e4b6",
                "etag": "W/\"datetime'2016-02-26T20%3A25%3A00.565204Z'\"",
                "properties": {
                    "Type": "Alert",
                    "Name": "Threshold for my webhook action",
                    "Threshold": {
                        "Operator": "gt",
                        "Value": 10
                    },
                    "Version": 1
                }
            }
        ]
    }

#### <a name="create-or-edit-a-webhook-action"></a><span data-ttu-id="a41d5-260">Hozzon létre vagy egy webhook művelet szerkesztése</span><span class="sxs-lookup"><span data-stu-id="a41d5-260">Create or edit a webhook action</span></span>
<span data-ttu-id="a41d5-261">Egy egyedi művelet azonosítója toocreate új webhook művelet ütemezett hello Put metódust használata.</span><span class="sxs-lookup"><span data-stu-id="a41d5-261">Use hello Put method with a unique action ID toocreate a new webhook action for a schedule.</span></span>  <span data-ttu-id="a41d5-262">hello alábbi példa létrehoz egy Webhook műveletet és egy műveletet a küszöbértéket, hello webhook váltja hello mentett keresés eredményei hello meghaladnia hello küszöbértéket.</span><span class="sxs-lookup"><span data-stu-id="a41d5-262">hello following example creates a Webhook action and an Alert action with a threshold so that hello webhook will be triggered when hello results of hello saved search exceed hello threshold.</span></span>

    $thresholdAction = "{'properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdAction

    $webhookAction = "{'properties': {'Type': 'Webhook', 'Name': 'My Webhook", 'WebhookUri': 'https://oaaswebhookdf.cloudapp.net/webhooks?token=VrkYTKlhk%2fc%2bKBP', 'CustomPayload': '{\"field1\":\"value1\",\"field2\":\"value2\"}', 'Version': 1 }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mywebhookaction?api-version=2015-03-20 $webhookAction

<span data-ttu-id="a41d5-263">Egy meglévő művelet azonosítója toomodify egy webhook művelet ütemezett hello Put metódust használata.</span><span class="sxs-lookup"><span data-stu-id="a41d5-263">Use hello Put method with an existing action ID toomodify a webhook action for a schedule.</span></span>  <span data-ttu-id="a41d5-264">hello kérelem törzse hello hello etag hello művelet tartalmaznia kell.</span><span class="sxs-lookup"><span data-stu-id="a41d5-264">hello body of hello request must include hello etag of hello action.</span></span>

    $webhookAction = "{'etag': 'W/\"datetime'2016-02-26T20%3A25%3A00.6862124Z'\"','properties': {'Type': 'Webhook', 'Name': 'My Webhook", 'WebhookUri': 'https://oaaswebhookdf.cloudapp.net/webhooks?token=VrkYTKlhk%2fc%2bKBP', 'CustomPayload': '{\"field1\":\"value1\",\"field2\":\"value2\"}', 'Version': 1 }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mywebhookaction?api-version=2015-03-20 $webhookAction

## <a name="next-steps"></a><span data-ttu-id="a41d5-265">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a41d5-265">Next steps</span></span>
* <span data-ttu-id="a41d5-266">Használjon hello [REST API tooperform napló keresések](log-analytics-log-search-api.md) a Naplóelemzési.</span><span class="sxs-lookup"><span data-stu-id="a41d5-266">Use hello [REST API tooperform log searches](log-analytics-log-search-api.md) in Log Analytics.</span></span>

