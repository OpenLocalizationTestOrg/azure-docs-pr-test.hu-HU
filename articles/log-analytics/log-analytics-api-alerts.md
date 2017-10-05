---
title: "OMS napló Analytics riasztási REST API használatával"
description: "A napló Analytics riasztási REST API-t kezelheti a riasztásokat az Operations Management Suite (OMS) része Naplóelemzési teszi lehetővé.  Ez a cikk ismerteti az API-t, és néhány példa a másik műveletet hajt végre."
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
ms.openlocfilehash: 5ce72ffef4394bf3bbe39fa420c4fcaa965ae35c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-and-manage-alert-rules-in-log-analytics-with-rest-api"></a><span data-ttu-id="0c532-104">Hozzon létre, és a REST API-val Naplóelemzési riasztási szabályok kezelése</span><span class="sxs-lookup"><span data-stu-id="0c532-104">Create and manage alert rules in Log Analytics with REST API</span></span>
<span data-ttu-id="0c532-105">A napló Analytics riasztási REST API lehetővé teszi, hogy hozhat létre és kezelheti a riasztásokat az Operations Management Suite (OMS).</span><span class="sxs-lookup"><span data-stu-id="0c532-105">The Log Analytics Alert REST API allows you to create and manage alerts in Operations Management Suite (OMS).</span></span>  <span data-ttu-id="0c532-106">Ez a cikk ismerteti az API-t, és néhány példa a másik műveletet hajt végre.</span><span class="sxs-lookup"><span data-stu-id="0c532-106">This article provides details of the API and several examples for performing different operations.</span></span>

<span data-ttu-id="0c532-107">A napló Analytics Search REST API RESTful, és az Azure Resource Manager REST API-n keresztül érhető el.</span><span class="sxs-lookup"><span data-stu-id="0c532-107">The Log Analytics Search REST API is RESTful and can be accessed via the Azure Resource Manager REST API.</span></span> <span data-ttu-id="0c532-108">Ebben a dokumentumban található példák az elérését az API-t a PowerShell parancssori használatával [ARMClient](https://github.com/projectkudu/ARMClient), egy nyílt forráskódú parancssori eszköz, amely leegyszerűsíti az Azure Resource Manager API meghívása.</span><span class="sxs-lookup"><span data-stu-id="0c532-108">In this document you will find examples where the API is accessed from a PowerShell command line using  [ARMClient](https://github.com/projectkudu/ARMClient), an open source command line tool that simplifies invoking the Azure Resource Manager API.</span></span> <span data-ttu-id="0c532-109">A ARMClient és a PowerShell használata a napló Analytics Search API eléréséhez számos lehetőség.</span><span class="sxs-lookup"><span data-stu-id="0c532-109">The use of ARMClient and PowerShell is one of many options to access the Log Analytics Search API.</span></span> <span data-ttu-id="0c532-110">Ezekkel az eszközökkel Azure Resource Manager RESTful API-hívások indítása az OMS-munkaterület, és rajtuk keresési parancsok végrehajtása használhatja.</span><span class="sxs-lookup"><span data-stu-id="0c532-110">With these tools you can utilize the RESTful Azure Resource Manager API to make calls to OMS workspaces and perform search commands within them.</span></span> <span data-ttu-id="0c532-111">Az API-t kimeneti fog keresési eredmények Önnek JSON formátumban, hogy lehetővé teszi a programozott módon használja a keresési eredmények között számos különböző módja.</span><span class="sxs-lookup"><span data-stu-id="0c532-111">The API will output search results to you in JSON format, allowing you to use the search results in many different ways programmatically.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0c532-112">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="0c532-112">Prerequisites</span></span>
<span data-ttu-id="0c532-113">Jelenleg riasztásokat csak hozhatja létre a Naplóelemzési mentett keresés.</span><span class="sxs-lookup"><span data-stu-id="0c532-113">Currently, alerts can only be created with a saved search in Log Analytics.</span></span>  <span data-ttu-id="0c532-114">Olvassa el a [napló Search REST API](log-analytics-log-search-api.md) további információt.</span><span class="sxs-lookup"><span data-stu-id="0c532-114">You can refer to the [Log Search REST API](log-analytics-log-search-api.md) for more information.</span></span>

## <a name="schedules"></a><span data-ttu-id="0c532-115">Ütemezések</span><span class="sxs-lookup"><span data-stu-id="0c532-115">Schedules</span></span>
<span data-ttu-id="0c532-116">A mentett kereséseket rendelkezhet egy vagy több ütemezés.</span><span class="sxs-lookup"><span data-stu-id="0c532-116">A saved search can have one or more schedules.</span></span> <span data-ttu-id="0c532-117">Az ütemezés határozza meg, hogy milyen gyakran a keresés futtatása és a időtartam alatt, amelyben a feltétel azonosítja.</span><span class="sxs-lookup"><span data-stu-id="0c532-117">The schedule defines how often the search is run and the time interval over which the criteria is identified.</span></span>
<span data-ttu-id="0c532-118">Ütemezések a jellemzőkkel rendelkezik, az alábbi táblázatban.</span><span class="sxs-lookup"><span data-stu-id="0c532-118">Schedules have the properties in the following table.</span></span>

| <span data-ttu-id="0c532-119">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="0c532-119">Property</span></span> | <span data-ttu-id="0c532-120">Leírás</span><span class="sxs-lookup"><span data-stu-id="0c532-120">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="0c532-121">időköz</span><span class="sxs-lookup"><span data-stu-id="0c532-121">Interval</span></span> |<span data-ttu-id="0c532-122">Milyen gyakran fut a keresést.</span><span class="sxs-lookup"><span data-stu-id="0c532-122">How often the search is run.</span></span> <span data-ttu-id="0c532-123">Percben értendő.</span><span class="sxs-lookup"><span data-stu-id="0c532-123">Measured in minutes.</span></span> |
| <span data-ttu-id="0c532-124">queryTimeSpan</span><span class="sxs-lookup"><span data-stu-id="0c532-124">QueryTimeSpan</span></span> |<span data-ttu-id="0c532-125">Az időtartam, amely a feltétel kiértékelése.</span><span class="sxs-lookup"><span data-stu-id="0c532-125">The time interval over which the criteria is evaluated.</span></span> <span data-ttu-id="0c532-126">Időköz nagyobbnak vagy azzal egyenlőnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="0c532-126">Must be equal to or greater than Interval.</span></span> <span data-ttu-id="0c532-127">Percben értendő.</span><span class="sxs-lookup"><span data-stu-id="0c532-127">Measured in minutes.</span></span> |
| <span data-ttu-id="0c532-128">Verzió</span><span class="sxs-lookup"><span data-stu-id="0c532-128">Version</span></span> |<span data-ttu-id="0c532-129">A használt API-verzió.</span><span class="sxs-lookup"><span data-stu-id="0c532-129">The API version being used.</span></span>  <span data-ttu-id="0c532-130">Jelenleg ez beállítása mindig 1.</span><span class="sxs-lookup"><span data-stu-id="0c532-130">Currently, this should always be set to 1.</span></span> |

<span data-ttu-id="0c532-131">Vegye figyelembe például egy esemény lekérdezés Timespan érték 30 perc és 15 perces időközönként.</span><span class="sxs-lookup"><span data-stu-id="0c532-131">For example, consider an event query with an Interval of 15 minutes and a Timespan of 30 minutes.</span></span> <span data-ttu-id="0c532-132">Ebben az esetben a lekérdezés futna 15 percenként, és egy riasztás akkor váltódik ki, ha a feltétel igaz over feloldani továbbra is a 30 perces span.</span><span class="sxs-lookup"><span data-stu-id="0c532-132">In this case, the query would be run every 15 minutes, and an alert would be triggered if the criteria continued to resolve to true over a 30 minute span.</span></span>

### <a name="retrieving-schedules"></a><span data-ttu-id="0c532-133">Ütemezés lekérése</span><span class="sxs-lookup"><span data-stu-id="0c532-133">Retrieving schedules</span></span>
<span data-ttu-id="0c532-134">A mentett keresések összes ütemezés a Get metódus használatával.</span><span class="sxs-lookup"><span data-stu-id="0c532-134">Use the Get method to retrieve all schedules for a saved search.</span></span>

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search  ID}/schedules?api-version=2015-03-20

<span data-ttu-id="0c532-135">A Get metódus használatával az ütemezés Azonosítójú egy meghatározott ütemezést, a mentett keresés.</span><span class="sxs-lookup"><span data-stu-id="0c532-135">Use the Get method with a schedule ID to retrieve a particular schedule for a saved search.</span></span>

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}?api-version=2015-03-20

<span data-ttu-id="0c532-136">Az alábbiakban az ütemezett mintát választ.</span><span class="sxs-lookup"><span data-stu-id="0c532-136">Following is a sample response for a schedule.</span></span>

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

### <a name="creating-a-schedule"></a><span data-ttu-id="0c532-137">Ütemezés létrehozása</span><span class="sxs-lookup"><span data-stu-id="0c532-137">Creating a schedule</span></span>
<span data-ttu-id="0c532-138">Az ütemezés egyedi Azonosítójú a Put metódust használatával hozzon létre egy új ütemezést.</span><span class="sxs-lookup"><span data-stu-id="0c532-138">Use the Put method with a unique schedule ID to create a new schedule.</span></span>  <span data-ttu-id="0c532-139">Ne feledje, hogy két ütemezések nem ugyanazzal az Azonosítóval akkor is, ha különböző társított mentett kereséseket.</span><span class="sxs-lookup"><span data-stu-id="0c532-139">Note that two schedules cannot have the same ID even if they are associated with different saved searches.</span></span>  <span data-ttu-id="0c532-140">Egy ütemezést az OMS-konzolon, GUID van hozza létre a ütemezés azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="0c532-140">When you create a schedule in the OMS console, a GUID is created for the schedule ID.</span></span>

> [!NOTE]
> <span data-ttu-id="0c532-141">A nevet minden mentett keresések, ütemezéseihez és napló Analytics API-val létrehozott kisbetűs kell lennie.</span><span class="sxs-lookup"><span data-stu-id="0c532-141">The name for all saved searches, schedules, and actions created with the Log Analytics API must be in lowercase.</span></span>

    $scheduleJson = "{'properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/mynewschedule?api-version=2015-03-20 $scheduleJson

### <a name="editing-a-schedule"></a><span data-ttu-id="0c532-142">Az ütemezés módosítása</span><span class="sxs-lookup"><span data-stu-id="0c532-142">Editing a schedule</span></span>
<span data-ttu-id="0c532-143">Használja a Put metódust egy meglévő ütemezés azonosítójú ugyanazon mentett keresés ütemezésnek.</span><span class="sxs-lookup"><span data-stu-id="0c532-143">Use the Put method with an existing schedule ID for the same saved search to modify that schedule.</span></span>  <span data-ttu-id="0c532-144">A kérelem törzse tartalmaznia kell az ütemezés etag.</span><span class="sxs-lookup"><span data-stu-id="0c532-144">The body of the request must include the etag of the schedule.</span></span>

      $scheduleJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A49.8074679Z'\""','properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' } }"
      armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/mynewschedule?api-version=2015-03-20 $scheduleJson


### <a name="deleting-schedules"></a><span data-ttu-id="0c532-145">Ütemezés törlése</span><span class="sxs-lookup"><span data-stu-id="0c532-145">Deleting schedules</span></span>
<span data-ttu-id="0c532-146">Ütemezés törléséhez használja a Delete metódus ütemezés-azonosítóval.</span><span class="sxs-lookup"><span data-stu-id="0c532-146">Use the Delete method with a schedule ID to delete a schedule.</span></span>

    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}?api-version=2015-03-20


## <a name="actions"></a><span data-ttu-id="0c532-147">Műveletek</span><span class="sxs-lookup"><span data-stu-id="0c532-147">Actions</span></span>
<span data-ttu-id="0c532-148">Ütemezés rendelkezhet több művelet.</span><span class="sxs-lookup"><span data-stu-id="0c532-148">A schedule can have multiple actions.</span></span> <span data-ttu-id="0c532-149">Művelet határozhatnak meg egy vagy több folyamat végrehajtásához, például egy levél küldése vagy a runbook indítása, vagy azt határozhatnak meg, amely azt határozza meg, ha a keresési eredmények bizonyos feltételeknek megfelelő küszöbértéket.</span><span class="sxs-lookup"><span data-stu-id="0c532-149">An action may define one or more processes to perform such as sending a mail or starting a runbook, or it may define a threshold that determines when the results of a search match some criteria.</span></span>  <span data-ttu-id="0c532-150">Bizonyos műveleteket határozza meg az is, hogy a folyamatok úgy hajtja végre, a küszöbérték elérése.</span><span class="sxs-lookup"><span data-stu-id="0c532-150">Some actions will define both so that the processes are performed when the threshold is met.</span></span>

<span data-ttu-id="0c532-151">Minden művelet a következő táblázat a jellemzőkkel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="0c532-151">All actions have the properties in the following table.</span></span>  <span data-ttu-id="0c532-152">Különböző típusú riasztások különböző további tulajdonságokat, amelyek az alábbiakban található rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="0c532-152">Different types of alerts have different additional properties which are described below.</span></span>

| <span data-ttu-id="0c532-153">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="0c532-153">Property</span></span> | <span data-ttu-id="0c532-154">Leírás</span><span class="sxs-lookup"><span data-stu-id="0c532-154">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="0c532-155">Típus</span><span class="sxs-lookup"><span data-stu-id="0c532-155">Type</span></span> |<span data-ttu-id="0c532-156">A művelet típusát.</span><span class="sxs-lookup"><span data-stu-id="0c532-156">Type of the action.</span></span>  <span data-ttu-id="0c532-157">Jelenleg a lehetséges értékei a riasztás és Webhook.</span><span class="sxs-lookup"><span data-stu-id="0c532-157">Currently the possible values are Alert and Webhook.</span></span> |
| <span data-ttu-id="0c532-158">Név</span><span class="sxs-lookup"><span data-stu-id="0c532-158">Name</span></span> |<span data-ttu-id="0c532-159">Megjelenítési nevet a riasztáshoz.</span><span class="sxs-lookup"><span data-stu-id="0c532-159">Display name for the alert.</span></span> |
| <span data-ttu-id="0c532-160">Verzió</span><span class="sxs-lookup"><span data-stu-id="0c532-160">Version</span></span> |<span data-ttu-id="0c532-161">A használt API-verzió.</span><span class="sxs-lookup"><span data-stu-id="0c532-161">The API version being used.</span></span>  <span data-ttu-id="0c532-162">Jelenleg ez beállítása mindig 1.</span><span class="sxs-lookup"><span data-stu-id="0c532-162">Currently, this should always be set to 1.</span></span> |

### <a name="retrieving-actions"></a><span data-ttu-id="0c532-163">Műveletek végrehajtása</span><span class="sxs-lookup"><span data-stu-id="0c532-163">Retrieving actions</span></span>
<span data-ttu-id="0c532-164">A Get metódus használatával ütemezett összes műveletet.</span><span class="sxs-lookup"><span data-stu-id="0c532-164">Use the Get method to retrieve all actions for a schedule.</span></span>

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search  ID}/schedules/{Schedule ID}/actions?api-version=2015-03-20

<span data-ttu-id="0c532-165">A Get metódus használatával a művelet azonosítójú ütemezett művelet.</span><span class="sxs-lookup"><span data-stu-id="0c532-165">Use the Get method with the action ID to retrieve a particular action for a schedule.</span></span>

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}/actions/{Action ID}?api-version=2015-03-20

### <a name="creating-or-editing-actions"></a><span data-ttu-id="0c532-166">Létrehozásával vagy szerkesztésével műveletek</span><span class="sxs-lookup"><span data-stu-id="0c532-166">Creating or editing actions</span></span>
<span data-ttu-id="0c532-167">A Put metódust használja az ütemezés hozzon létre egy új tevékenység egyedi azonosítójú művelet.</span><span class="sxs-lookup"><span data-stu-id="0c532-167">Use the Put method with an action ID that is unique to the schedule to create a new action.</span></span>  <span data-ttu-id="0c532-168">Egy műveletet hoz létre az OMS-konzolon, ha egy GUID van-e a művelet azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="0c532-168">When you create an action in the OMS console, a GUID is for the action ID.</span></span>

> [!NOTE]
> <span data-ttu-id="0c532-169">A nevet minden mentett keresések, ütemezéseihez és napló Analytics API-val létrehozott kisbetűs kell lennie.</span><span class="sxs-lookup"><span data-stu-id="0c532-169">The name for all saved searches, schedules, and actions created with the Log Analytics API must be in lowercase.</span></span>

<span data-ttu-id="0c532-170">Használja a Put metódust ugyanazon mentett keresés meglévő művelet azonosítója az ütemezésnek.</span><span class="sxs-lookup"><span data-stu-id="0c532-170">Use the Put method with an existing action ID for the same saved search to modify that schedule.</span></span>  <span data-ttu-id="0c532-171">A kérelem törzse tartalmaznia kell az ütemezés etag.</span><span class="sxs-lookup"><span data-stu-id="0c532-171">The body of the request must include the etag of the schedule.</span></span>

<span data-ttu-id="0c532-172">A kérelem formátuma új művelet létrehozásához művelettípus függ, ezek a példák az alábbi szakaszokban a.</span><span class="sxs-lookup"><span data-stu-id="0c532-172">The request format for creating a new action varies by action type so these examples are provided in the sections below.</span></span>

### <a name="deleting-actions"></a><span data-ttu-id="0c532-173">Műveletek törlése</span><span class="sxs-lookup"><span data-stu-id="0c532-173">Deleting actions</span></span>
<span data-ttu-id="0c532-174">A művelet azonosítójú a Delete metódus segítségével törölheti a műveletet.</span><span class="sxs-lookup"><span data-stu-id="0c532-174">Use the Delete method with the action ID to delete an action.</span></span>

    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}/Actions/{Action ID}?api-version=2015-03-20

### <a name="alert-actions"></a><span data-ttu-id="0c532-175">Értesítési műveletek</span><span class="sxs-lookup"><span data-stu-id="0c532-175">Alert Actions</span></span>
<span data-ttu-id="0c532-176">Ütemezés rendelkeznie kell egy műveletet.</span><span class="sxs-lookup"><span data-stu-id="0c532-176">A Schedule should have one and only one Alert action.</span></span>  <span data-ttu-id="0c532-177">Értesítési műveletek rendelkezik legalább egy, a következő táblázat a részeket.</span><span class="sxs-lookup"><span data-stu-id="0c532-177">Alert actions have one or more of the sections in the following table.</span></span>  <span data-ttu-id="0c532-178">A további részletek az alábbi leírása.</span><span class="sxs-lookup"><span data-stu-id="0c532-178">Each is described in further detail below.</span></span>

| <span data-ttu-id="0c532-179">A szakasz</span><span class="sxs-lookup"><span data-stu-id="0c532-179">Section</span></span> | <span data-ttu-id="0c532-180">Leírás</span><span class="sxs-lookup"><span data-stu-id="0c532-180">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="0c532-181">Küszöbérték</span><span class="sxs-lookup"><span data-stu-id="0c532-181">Threshold</span></span> |<span data-ttu-id="0c532-182">A művelet futtatásakor feltételeit.</span><span class="sxs-lookup"><span data-stu-id="0c532-182">Criteria for when the action is run.</span></span> |
| <span data-ttu-id="0c532-183">EmailNotification</span><span class="sxs-lookup"><span data-stu-id="0c532-183">EmailNotification</span></span> |<span data-ttu-id="0c532-184">E-mail küldés több címzettnek.</span><span class="sxs-lookup"><span data-stu-id="0c532-184">Send mail to multiple recipients.</span></span> |
| <span data-ttu-id="0c532-185">Szervizkiszolgáló</span><span class="sxs-lookup"><span data-stu-id="0c532-185">Remediation</span></span> |<span data-ttu-id="0c532-186">Elindít egy forgatókönyvet az Azure Automation sikertelen bejelentkezési kísérletet azonosított probléma elhárításához.</span><span class="sxs-lookup"><span data-stu-id="0c532-186">Start a runbook in Azure Automation to attempt to correct identified issue.</span></span> |

#### <a name="thresholds"></a><span data-ttu-id="0c532-187">Küszöbértékek</span><span class="sxs-lookup"><span data-stu-id="0c532-187">Thresholds</span></span>
<span data-ttu-id="0c532-188">Egy műveletet rendelkeznie kell egy küszöbértéket.</span><span class="sxs-lookup"><span data-stu-id="0c532-188">An Alert action should have one and only one threshold.</span></span>  <span data-ttu-id="0c532-189">A mentett keresés eredményei felel meg a küszöbérték, hogy a keresés társított művelet, amikor az adott művelet egyéb folyamatok futnak.</span><span class="sxs-lookup"><span data-stu-id="0c532-189">When the results of a saved search match the threshold in an action associated with that search, then any other processes in that action are run.</span></span>  <span data-ttu-id="0c532-190">Egy műveletet is tartalmazhat, csak a küszöbértéket, hogy más típusú, amelyek nem tartalmazzák a küszöbértékek műveletekhez használható.</span><span class="sxs-lookup"><span data-stu-id="0c532-190">An action can also contain only a threshold so that it can be used with actions of other types that don’t contain thresholds.</span></span>

<span data-ttu-id="0c532-191">Küszöbértékek az alábbi táblázatban a jellemzőkkel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="0c532-191">Thresholds have the properties in the following table.</span></span>

| <span data-ttu-id="0c532-192">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="0c532-192">Property</span></span> | <span data-ttu-id="0c532-193">Leírás</span><span class="sxs-lookup"><span data-stu-id="0c532-193">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="0c532-194">Operátor</span><span class="sxs-lookup"><span data-stu-id="0c532-194">Operator</span></span> |<span data-ttu-id="0c532-195">A küszöbérték összehasonlító operátort.</span><span class="sxs-lookup"><span data-stu-id="0c532-195">Operator for the threshold comparison.</span></span> <br> <span data-ttu-id="0c532-196">gt = nagyobb mint</span><span class="sxs-lookup"><span data-stu-id="0c532-196">gt = Greater Than</span></span> <br> <span data-ttu-id="0c532-197">lt = kisebb, mint</span><span class="sxs-lookup"><span data-stu-id="0c532-197">lt = Less Than</span></span> |
| <span data-ttu-id="0c532-198">Érték</span><span class="sxs-lookup"><span data-stu-id="0c532-198">Value</span></span> |<span data-ttu-id="0c532-199">Értéke a küszöbérték.</span><span class="sxs-lookup"><span data-stu-id="0c532-199">Value for the threshold.</span></span> |

<span data-ttu-id="0c532-200">Vegye figyelembe például 15 perc, 30 perc Timespan és egy nagyobb, mint 10 küszöbértéket időközzel eseménylekérdezési.</span><span class="sxs-lookup"><span data-stu-id="0c532-200">For example, consider an event query with an Interval of 15 minutes, a Timespan of 30 minutes, and a Threshold of greater than 10.</span></span> <span data-ttu-id="0c532-201">Ebben az esetben a lekérdezés futna 15 percenként, és a riasztás akkor váltódik ki, ha 10 keresztül a 30 perces span létrehozott események által visszaadott.</span><span class="sxs-lookup"><span data-stu-id="0c532-201">In this case, the query would be run every 15 minutes, and an alert would be triggered if it returned 10 events that were created over a 30 minute span.</span></span>

<span data-ttu-id="0c532-202">Az alábbiakban látható egy minta válasz csak a küszöbérték a művelet.</span><span class="sxs-lookup"><span data-stu-id="0c532-202">Following is a sample response for an action with only a threshold.</span></span>  

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

<span data-ttu-id="0c532-203">Az egyedi művelet Azonosítójú a Put metódust segítségével hozzon létre egy új küszöbérték műveletet ütemezés szerint.</span><span class="sxs-lookup"><span data-stu-id="0c532-203">Use the Put method with a unique action ID to create a new threshold action for a schedule.</span></span>  

    $thresholdJson = "{'properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdJson

<span data-ttu-id="0c532-204">Használja a Put metódust egy meglévő azonosítójú művelet egy ütemezés küszöbérték műveletet.</span><span class="sxs-lookup"><span data-stu-id="0c532-204">Use the Put method with an existing action ID to modify a threshold action for a schedule.</span></span>  <span data-ttu-id="0c532-205">A kérelem törzse tartalmaznia kell a művelet az etag.</span><span class="sxs-lookup"><span data-stu-id="0c532-205">The body of the request must include the etag of the action.</span></span>

    $thresholdJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdJson

#### <a name="email-notification"></a><span data-ttu-id="0c532-206">E-mail értesítések</span><span class="sxs-lookup"><span data-stu-id="0c532-206">Email Notification</span></span>
<span data-ttu-id="0c532-207">Értesítő e-mailt küldjön egy vagy több címzett.</span><span class="sxs-lookup"><span data-stu-id="0c532-207">Email Notifications send mail to one or more recipients.</span></span>  <span data-ttu-id="0c532-208">A tulajdonságok az alábbi táblázatban tartalmaznak.</span><span class="sxs-lookup"><span data-stu-id="0c532-208">They include the properties in the following table.</span></span>

| <span data-ttu-id="0c532-209">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="0c532-209">Property</span></span> | <span data-ttu-id="0c532-210">Leírás</span><span class="sxs-lookup"><span data-stu-id="0c532-210">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="0c532-211">Címzettek</span><span class="sxs-lookup"><span data-stu-id="0c532-211">Recipients</span></span> |<span data-ttu-id="0c532-212">E-mail címek listája.</span><span class="sxs-lookup"><span data-stu-id="0c532-212">List of mail addresses.</span></span> |
| <span data-ttu-id="0c532-213">Tárgy</span><span class="sxs-lookup"><span data-stu-id="0c532-213">Subject</span></span> |<span data-ttu-id="0c532-214">Az e-mail tárgyát.</span><span class="sxs-lookup"><span data-stu-id="0c532-214">The subject of the mail.</span></span> |
| <span data-ttu-id="0c532-215">Melléklet</span><span class="sxs-lookup"><span data-stu-id="0c532-215">Attachment</span></span> |<span data-ttu-id="0c532-216">Mellékletek jelenleg nem támogatottak, ezért ez a "None" értéke mindig lesz.</span><span class="sxs-lookup"><span data-stu-id="0c532-216">Attachments are not currently supported, so this will always have a value of “None”.</span></span> |

<span data-ttu-id="0c532-217">Az alábbiakban látható egy minta választ, a küszöbérték az e-mail értesítési művelet.</span><span class="sxs-lookup"><span data-stu-id="0c532-217">Following is a sample response for an email notification action with a threshold.</span></span>  

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
            "Subject": "This is the subject",
            "Attachment": "None"
        },
        "Version": 1
    }

<span data-ttu-id="0c532-218">Az egyedi művelet Azonosítójú a Put metódust segítségével hozzon létre egy új e-mail műveletet ütemezés szerint.</span><span class="sxs-lookup"><span data-stu-id="0c532-218">Use the Put method with a unique action ID to create a new e-mail action for a schedule.</span></span>  <span data-ttu-id="0c532-219">Az alábbi példa e-mailben értesítést a küszöbérték hoz létre, a levelezési küldi, ha a mentett keresési eredmények lépheti túl a küszöbértéket.</span><span class="sxs-lookup"><span data-stu-id="0c532-219">The following example creates an email notification with a threshold so the mail is sent when the results of the saved search exceed the threshold.</span></span>

    $emailJson = "{'properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is the subject', 'Attachment':'None'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myemailaction?api-version=2015-03-20 $emailJson

<span data-ttu-id="0c532-220">Használja a Put metódust egy meglévő azonosítójú művelet ütemezés e-mail művelet.</span><span class="sxs-lookup"><span data-stu-id="0c532-220">Use the Put method with an existing action ID to modify an e-mail action for a schedule.</span></span>  <span data-ttu-id="0c532-221">A kérelem törzse tartalmaznia kell a művelet az etag.</span><span class="sxs-lookup"><span data-stu-id="0c532-221">The body of the request must include the etag of the action.</span></span>

    $emailJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is the subject', 'Attachment':'None'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myemailaction?api-version=2015-03-20 $emailJson

#### <a name="remediation-actions"></a><span data-ttu-id="0c532-222">Szervizelési műveletek</span><span class="sxs-lookup"><span data-stu-id="0c532-222">Remediation actions</span></span>
<span data-ttu-id="0c532-223">Szervizelt próbál kijavítja a hibát, a riasztás által azonosított Azure Automation forgatókönyv indítása.</span><span class="sxs-lookup"><span data-stu-id="0c532-223">Remediations start a runbook in Azure Automation that attempts to correct the problem identified by the alert.</span></span>  <span data-ttu-id="0c532-224">A runbook egy szervizelési művelet szerepel a webhook létrehozása kell, és adja meg az URI a WebhookUri tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="0c532-224">You must create a webhook for the runbook used in a remediation action and then specify the URI in the WebhookUri property.</span></span>  <span data-ttu-id="0c532-225">Ez a művelet az OMS-konzollal létrehozásakor egy új webhook automatikusan létrejön a runbookhoz.</span><span class="sxs-lookup"><span data-stu-id="0c532-225">When you create this action using the OMS console, a new webhook is automatically created for the runbook.</span></span>

<span data-ttu-id="0c532-226">Szervizelt tulajdonságot tartalmazhatja az alábbi táblázatban.</span><span class="sxs-lookup"><span data-stu-id="0c532-226">Remediations include the properties in the following table.</span></span>

| <span data-ttu-id="0c532-227">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="0c532-227">Property</span></span> | <span data-ttu-id="0c532-228">Leírás</span><span class="sxs-lookup"><span data-stu-id="0c532-228">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="0c532-229">RunbookName</span><span class="sxs-lookup"><span data-stu-id="0c532-229">RunbookName</span></span> |<span data-ttu-id="0c532-230">A runbook neve.</span><span class="sxs-lookup"><span data-stu-id="0c532-230">Name of the runbook.</span></span> <span data-ttu-id="0c532-231">Ennek egyeznie kell a közzétett runbookok konfigurálva az Automation-megoldás az OMS-munkaterület az automation-fiókban.</span><span class="sxs-lookup"><span data-stu-id="0c532-231">This must match a published runbook in the automation account configured in the Automation Solution in your OMS workspace.</span></span> |
| <span data-ttu-id="0c532-232">WebhookUri</span><span class="sxs-lookup"><span data-stu-id="0c532-232">WebhookUri</span></span> |<span data-ttu-id="0c532-233">A webhook URI Azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="0c532-233">URI of the webhook.</span></span> |
| <span data-ttu-id="0c532-234">A lejárati</span><span class="sxs-lookup"><span data-stu-id="0c532-234">Expiry</span></span> |<span data-ttu-id="0c532-235">A lejárati dátum és idő, a webhook.</span><span class="sxs-lookup"><span data-stu-id="0c532-235">The expiration date and time of the webhook.</span></span>  <span data-ttu-id="0c532-236">Ha a webhook egy lejárati nem rendelkezik, majd ez lehet bármely érvényes jövőbeni dátum.</span><span class="sxs-lookup"><span data-stu-id="0c532-236">If the webhook doesn’t have an expiration, then this can be any valid future date.</span></span> |

<span data-ttu-id="0c532-237">Az alábbiakban látható egy mintaválasz szervizelési művelethez a küszöbértéket.</span><span class="sxs-lookup"><span data-stu-id="0c532-237">Following is a sample response for a remediation action with a threshold.</span></span>

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

<span data-ttu-id="0c532-238">Az egyedi művelet Azonosítójú a Put metódust segítségével hozzon létre egy új szervizelési művelet ütemezések.</span><span class="sxs-lookup"><span data-stu-id="0c532-238">Use the Put method with a unique action ID to create a new remediation action for a schedule.</span></span>  <span data-ttu-id="0c532-239">A következő példa egy szervizelés küszöbértéket hoz létre, a runbook indítását, amikor a mentett keresési eredmények lépheti túl a küszöbértéket.</span><span class="sxs-lookup"><span data-stu-id="0c532-239">The following example creates a remediation with a threshold so the runbook is started when the results of the saved search exceed the threshold.</span></span>

    $remediateJson = "{'properties': { 'Type':'Alert', 'Name': 'My Remediation Action', 'Version':'1', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'Remediation': {'RunbookName': 'My-Runbook', 'WebhookUri':'https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d', 'Expiry':'2018-02-25T18:27:20Z'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myremediationaction?api-version=2015-03-20 $remediateJson

<span data-ttu-id="0c532-240">Használja a Put metódust egy meglévő azonosítójú művelet egy ütemezés javítási műveletet.</span><span class="sxs-lookup"><span data-stu-id="0c532-240">Use the Put method with an existing action ID to modify a remediation action for a schedule.</span></span>  <span data-ttu-id="0c532-241">A kérelem törzse tartalmaznia kell a művelet az etag.</span><span class="sxs-lookup"><span data-stu-id="0c532-241">The body of the request must include the etag of the action.</span></span>

    $remediateJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Type':'Alert', 'Name': 'My Remediation Action', 'Version':'1', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'Remediation': {'RunbookName': 'My-Runbook', 'WebhookUri':'https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d', 'Expiry':'2018-02-25T18:27:20Z'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myremediationaction?api-version=2015-03-20 $remediateJson

#### <a name="example"></a><span data-ttu-id="0c532-242">Példa</span><span class="sxs-lookup"><span data-stu-id="0c532-242">Example</span></span>
<span data-ttu-id="0c532-243">Az alábbiakban látható egy teljes példa egy új e-mail-riasztások létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="0c532-243">Following is a complete example to create a new email alert.</span></span>  <span data-ttu-id="0c532-244">Ezzel létrehoz egy új ütemezést tartalmazó egy küszöbértéket és e-mailek művelet együtt.</span><span class="sxs-lookup"><span data-stu-id="0c532-244">This creates a new schedule along with an action containing a threshold and email.</span></span>

    $subscriptionId = "3d56705e-5b26-5bcc-9368-dbc8d2fafbfc"
    $resourceGroup  = "MyResourceGroup"    
    $workspaceName    = "MyWorkspace"
    $searchId       = "MySearch"
    $scheduleId     = "MySchedule"
    $thresholdId    = "MyThreshold"
    $actionId       = "MyEmailAction"

    $scheduleJson = "{'properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' }"
    armclient put /subscriptions/$subscriptionId/resourceGroups/$resourceGroup/providers/Microsoft.OperationalInsights/workspaces/$workspaceName/savedSearches/$searchId/schedules/$scheduleId/?api-version=2015-03-20 $scheduleJson

    $emailJson = "{'properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Severity':'Warning', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is the subject', 'Attachment':'None'} }"
    armclient put /subscriptions/$subscriptionId/resourceGroups/$resourceGroup/providers/Microsoft.OperationalInsights/workspaces/$workspaceName/savedSearches/$searchId/schedules/$scheduleId/actions/$actionId/?api-version=2015-03-20 $emailJson

### <a name="webhook-actions"></a><span data-ttu-id="0c532-245">Webhookműveletek</span><span class="sxs-lookup"><span data-stu-id="0c532-245">Webhook actions</span></span>
<span data-ttu-id="0c532-246">Webhookműveletek egy folyamat megkezdéséhez hívja az egy URL-cím és a nem kötelezően kell küldeni a hasznos adatok között.</span><span class="sxs-lookup"><span data-stu-id="0c532-246">Webhook actions start a process by calling a URL and optionally providing a payload to be sent.</span></span>  <span data-ttu-id="0c532-247">Ezek hasonlóak szervizelési műveletek kivételével ezek webhookokkal, amely az Azure Automation-runbook eltérő folyamatok indít el a célja.</span><span class="sxs-lookup"><span data-stu-id="0c532-247">They are similar to Remediation actions except they are meant for webhooks that may invoke processes other than Azure Automation runbooks.</span></span>  <span data-ttu-id="0c532-248">A további lehetőséget, hogy a hasznos adatok között a távoli folyamat küldendő is biztosítanak.</span><span class="sxs-lookup"><span data-stu-id="0c532-248">They also provide the additional option of providing a payload to be delivered to the remote process.</span></span>

<span data-ttu-id="0c532-249">Webhookműveletek nem rendelkezik a küszöbérték, de ehelyett hozzá kell adni egy ütemezést, amely egy riasztási műveletek a küszöbértéket.</span><span class="sxs-lookup"><span data-stu-id="0c532-249">Webhook actions do not have a threshold but instead should be added to a schedule that has an Alert action with a threshold.</span></span>  <span data-ttu-id="0c532-250">Összes futni fog, amikor a küszöbérték elérése több Webhook műveleteket adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="0c532-250">You can add multiple Webhook actions that will all be run when the threshold is met.</span></span>

<span data-ttu-id="0c532-251">Webhookműveletek tulajdonságot tartalmazhatja az alábbi táblázatban.</span><span class="sxs-lookup"><span data-stu-id="0c532-251">Webhook actions include the properties in the following table.</span></span>

| <span data-ttu-id="0c532-252">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="0c532-252">Property</span></span> | <span data-ttu-id="0c532-253">Leírás</span><span class="sxs-lookup"><span data-stu-id="0c532-253">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="0c532-254">WebhookUri</span><span class="sxs-lookup"><span data-stu-id="0c532-254">WebhookUri</span></span> |<span data-ttu-id="0c532-255">Az e-mail tárgyát.</span><span class="sxs-lookup"><span data-stu-id="0c532-255">The subject of the mail.</span></span> |
| <span data-ttu-id="0c532-256">customPayload</span><span class="sxs-lookup"><span data-stu-id="0c532-256">CustomPayload</span></span> |<span data-ttu-id="0c532-257">A webhook küldendő egyéni hasznos.</span><span class="sxs-lookup"><span data-stu-id="0c532-257">Custom payload to be sent to the webhook.</span></span>  <span data-ttu-id="0c532-258">A formátum a tartalma a webhook megfelelő függ.</span><span class="sxs-lookup"><span data-stu-id="0c532-258">The format will depend on what the webhook is expecting.</span></span> |

<span data-ttu-id="0c532-259">Az alábbiakban látható egy mintaválasz webhook műveletet és egy társított műveletet a küszöbértéket.</span><span class="sxs-lookup"><span data-stu-id="0c532-259">Following is a sample response for webhook action and an associated alert action with a threshold.</span></span>

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

#### <a name="create-or-edit-a-webhook-action"></a><span data-ttu-id="0c532-260">Hozzon létre vagy egy webhook művelet szerkesztése</span><span class="sxs-lookup"><span data-stu-id="0c532-260">Create or edit a webhook action</span></span>
<span data-ttu-id="0c532-261">Az egyedi művelet Azonosítójú a Put metódust segítségével hozzon létre egy új webhook műveletet ütemezés szerint.</span><span class="sxs-lookup"><span data-stu-id="0c532-261">Use the Put method with a unique action ID to create a new webhook action for a schedule.</span></span>  <span data-ttu-id="0c532-262">Az alábbi példa hoz létre egy Webhook műveletet és egy műveletet a küszöbértéket, hogy a webhook aktiválódik, amikor lépheti túl a küszöbértéket, a mentett keresés eredménye.</span><span class="sxs-lookup"><span data-stu-id="0c532-262">The following example creates a Webhook action and an Alert action with a threshold so that the webhook will be triggered when the results of the saved search exceed the threshold.</span></span>

    $thresholdAction = "{'properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdAction

    $webhookAction = "{'properties': {'Type': 'Webhook', 'Name': 'My Webhook", 'WebhookUri': 'https://oaaswebhookdf.cloudapp.net/webhooks?token=VrkYTKlhk%2fc%2bKBP', 'CustomPayload': '{\"field1\":\"value1\",\"field2\":\"value2\"}', 'Version': 1 }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mywebhookaction?api-version=2015-03-20 $webhookAction

<span data-ttu-id="0c532-263">A webhook művelet ütemezés módosításához használja egy meglévő azonosítójú művelet a Put metódust.</span><span class="sxs-lookup"><span data-stu-id="0c532-263">Use the Put method with an existing action ID to modify a webhook action for a schedule.</span></span>  <span data-ttu-id="0c532-264">A kérelem törzse tartalmaznia kell a művelet az etag.</span><span class="sxs-lookup"><span data-stu-id="0c532-264">The body of the request must include the etag of the action.</span></span>

    $webhookAction = "{'etag': 'W/\"datetime'2016-02-26T20%3A25%3A00.6862124Z'\"','properties': {'Type': 'Webhook', 'Name': 'My Webhook", 'WebhookUri': 'https://oaaswebhookdf.cloudapp.net/webhooks?token=VrkYTKlhk%2fc%2bKBP', 'CustomPayload': '{\"field1\":\"value1\",\"field2\":\"value2\"}', 'Version': 1 }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mywebhookaction?api-version=2015-03-20 $webhookAction

## <a name="next-steps"></a><span data-ttu-id="0c532-265">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0c532-265">Next steps</span></span>
* <span data-ttu-id="0c532-266">Használja a [REST API napló keresés](log-analytics-log-search-api.md) a Naplóelemzési.</span><span class="sxs-lookup"><span data-stu-id="0c532-266">Use the [REST API to perform log searches](log-analytics-log-search-api.md) in Log Analytics.</span></span>

