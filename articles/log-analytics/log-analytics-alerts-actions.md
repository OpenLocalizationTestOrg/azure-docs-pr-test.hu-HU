---
title: "Riasztások az OMS szolgáltatáshoz válaszokat |} Microsoft Docs"
description: "Naplóelemzési riasztások határozza meg az OMS-adattárban lévő fontos adatokat és is proaktív értesítést küldenek, problémák vagy meghívása műveletek kijavításának őket.  Ez a cikk ismerteti a riasztási szabály és a részletek a különböző általuk végezhető műveletek létrehozása."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 6cfd2a46-b6a2-4f79-a67b-08ce488f9a91
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/28/2017
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b8731e1fe48b7d809b113eb5273e3962542b8f34
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="add-actions-to-alert-rules-in-log-analytics"></a><span data-ttu-id="8f359-104">Műveletek hozzáadni a Naplóelemzési riasztási szabályok</span><span class="sxs-lookup"><span data-stu-id="8f359-104">Add actions to alert rules in Log Analytics</span></span>
<span data-ttu-id="8f359-105">Ha egy [riasztást hoz létre a Naplóelemzési](log-analytics-alerts.md), lehetősége van a [a riasztási szabály konfigurálása](log-analytics-alerts.md) egy vagy több műveletek elvégzéséhez.</span><span class="sxs-lookup"><span data-stu-id="8f359-105">When an [alert is created in Log Analytics](log-analytics-alerts.md), you have the option of [configuring the alert rule](log-analytics-alerts.md) to perform one or more actions.</span></span>  <span data-ttu-id="8f359-106">Ez a cikk ismerteti a rendelkezésre álló különféle műveletek és a részletek konfigurálásával összes típusához.</span><span class="sxs-lookup"><span data-stu-id="8f359-106">This article describes the different actions that are available and details on configuring each kind.</span></span>

| <span data-ttu-id="8f359-107">Műveletek</span><span class="sxs-lookup"><span data-stu-id="8f359-107">Action</span></span> | <span data-ttu-id="8f359-108">Leírás</span><span class="sxs-lookup"><span data-stu-id="8f359-108">Description</span></span> |
|:--|:--|
| [<span data-ttu-id="8f359-109">E-mailek</span><span class="sxs-lookup"><span data-stu-id="8f359-109">Email</span></span>](#email-actions) | <span data-ttu-id="8f359-110">A riasztás részleteit e-mailt küldeni egy vagy több címzett.</span><span class="sxs-lookup"><span data-stu-id="8f359-110">Send an e-mail with the details of the alert to one or more recipients.</span></span> |
| [<span data-ttu-id="8f359-111">Webhook</span><span class="sxs-lookup"><span data-stu-id="8f359-111">Webhook</span></span>](#webhook-actions) | <span data-ttu-id="8f359-112">Egy külső folyamatban egy HTTP POST kérelemben keresztül meghívni.</span><span class="sxs-lookup"><span data-stu-id="8f359-112">Invoke an external process through a single HTTP POST request.</span></span> |
| [<span data-ttu-id="8f359-113">A Runbook</span><span class="sxs-lookup"><span data-stu-id="8f359-113">Runbook</span></span>](#runbook-actions) | <span data-ttu-id="8f359-114">Elindít egy forgatókönyvet az Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="8f359-114">Start a runbook in Azure Automation.</span></span> |


## <a name="email-actions"></a><span data-ttu-id="8f359-115">E-mailek műveletek</span><span class="sxs-lookup"><span data-stu-id="8f359-115">Email actions</span></span>
<span data-ttu-id="8f359-116">E-mailek műveletek a riasztás részleteit e-mail küldése egy vagy több címzett.</span><span class="sxs-lookup"><span data-stu-id="8f359-116">Email actions send an e-mail with the details of the alert to one or more recipients.</span></span>  <span data-ttu-id="8f359-117">Megadhatja, hogy az e-mail tárgya, de annak tartalma Naplóelemzési által összeállított szabványos formátumban.</span><span class="sxs-lookup"><span data-stu-id="8f359-117">You can specify the subject of the mail, but it's content is a standard format constructed by Log Analytics.</span></span>  <span data-ttu-id="8f359-118">Ez magában foglalja például a nevét a riasztás részleteit a napló keresés által visszaadott legfeljebb tíz rekordok mellett összegző információkat.</span><span class="sxs-lookup"><span data-stu-id="8f359-118">It includes summary information such as the name of the alert in addition to details of up to ten records returned by the log search.</span></span>  <span data-ttu-id="8f359-119">A Naplóelemzési, visszatér a rekordok teljes készletét, hogy a lekérdezés egy napló keresés hivatkozást is tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="8f359-119">It also includes a link to a log search in Log Analytics that will return the entire set of records from that query.</span></span>   <span data-ttu-id="8f359-120">Az üzenet küldője *a Microsoft Operations Management Suite Team &lt; noreply@oms.microsoft.com &gt;* .</span><span class="sxs-lookup"><span data-stu-id="8f359-120">The sender of the mail is *Microsoft Operations Management Suite Team &lt;noreply@oms.microsoft.com&gt;*.</span></span> 

<span data-ttu-id="8f359-121">E-mailek műveletek igényelnek a tulajdonságok az alábbi táblázatban.</span><span class="sxs-lookup"><span data-stu-id="8f359-121">Email actions require the properties in the following table.</span></span>

| <span data-ttu-id="8f359-122">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="8f359-122">Property</span></span> | <span data-ttu-id="8f359-123">Leírás</span><span class="sxs-lookup"><span data-stu-id="8f359-123">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="8f359-124">Tárgy</span><span class="sxs-lookup"><span data-stu-id="8f359-124">Subject</span></span> |<span data-ttu-id="8f359-125">A tulajdonos e-mailben.</span><span class="sxs-lookup"><span data-stu-id="8f359-125">Subject in the email.</span></span>  <span data-ttu-id="8f359-126">Az üzenet törzse nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="8f359-126">You cannot modify the body of the mail.</span></span> |
| <span data-ttu-id="8f359-127">Címzettek</span><span class="sxs-lookup"><span data-stu-id="8f359-127">Recipients</span></span> |<span data-ttu-id="8f359-128">Címzett e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="8f359-128">Addresses of all e-mail recipients.</span></span>  <span data-ttu-id="8f359-129">Ha több címet ad meg, majd külön a címeket pontosvesszővel (;).</span><span class="sxs-lookup"><span data-stu-id="8f359-129">If you specify more than one address, then separate the addresses with a semicolon (;).</span></span> |


## <a name="webhook-actions"></a><span data-ttu-id="8f359-130">Webhookműveletek</span><span class="sxs-lookup"><span data-stu-id="8f359-130">Webhook actions</span></span>

<span data-ttu-id="8f359-131">Webhookműveletek lehetővé teszi egy külső folyamatban egy HTTP POST kérelemben keresztül.</span><span class="sxs-lookup"><span data-stu-id="8f359-131">Webhook actions allow you to invoke an external process through a single HTTP POST request.</span></span>  <span data-ttu-id="8f359-132">A meghívott szolgáltatás kell webhookok támogatja, és határozza meg, hogyan fogja használni a tartalom kap.</span><span class="sxs-lookup"><span data-stu-id="8f359-132">The service being called should support webhooks and determine how it will use any payload it receives.</span></span>  <span data-ttu-id="8f359-133">Sikerült is meghívhatja a REST API-t nem támogató kifejezetten webhookokkal, amíg a kérelmet, amely együttműködik a API formátumban.</span><span class="sxs-lookup"><span data-stu-id="8f359-133">You could also call a REST API that doesn't specifically support webhooks as long as the request is in a format that the API understands.</span></span>  <span data-ttu-id="8f359-134">Példák a webhook riasztást válaszul egy üzenetet küldi [Slackhez](http://slack.com) , vagy hozzon létre egy incidenst a [PagerDuty](http://pagerduty.com/).</span><span class="sxs-lookup"><span data-stu-id="8f359-134">Examples of using a webhook in response to an alert are sending a message in [Slack](http://slack.com) or creating an incident in [PagerDuty](http://pagerduty.com/).</span></span>  <span data-ttu-id="8f359-135">Riasztási szabály létrehozása a Slackhez hívása olyan webhook részletes útmutatást érhető el: [Naplóelemzési riasztásokban Webhookok](log-analytics-alerts-webhooks.md).</span><span class="sxs-lookup"><span data-stu-id="8f359-135">A complete walkthrough of creating an alert rule with a webhook to call Slack is available at [Webhooks in Log Analytics alerts](log-analytics-alerts-webhooks.md).</span></span>

<span data-ttu-id="8f359-136">Webhookműveletek igényelnek a tulajdonságok az alábbi táblázatban.</span><span class="sxs-lookup"><span data-stu-id="8f359-136">Webhook actions require the properties in the following table.</span></span>

| <span data-ttu-id="8f359-137">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="8f359-137">Property</span></span> | <span data-ttu-id="8f359-138">Leírás</span><span class="sxs-lookup"><span data-stu-id="8f359-138">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="8f359-139">Webhook URL-CÍMÉT</span><span class="sxs-lookup"><span data-stu-id="8f359-139">Webhook URL</span></span> |<span data-ttu-id="8f359-140">A webhook URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="8f359-140">The URL of the webhook.</span></span> |
| <span data-ttu-id="8f359-141">Egyéni JSON-adattartalmat</span><span class="sxs-lookup"><span data-stu-id="8f359-141">Custom JSON payload</span></span> |<span data-ttu-id="8f359-142">A webhook küldendő egyéni hasznos.</span><span class="sxs-lookup"><span data-stu-id="8f359-142">Custom payload to send with the webhook.</span></span>  <span data-ttu-id="8f359-143">További információ alább olvasható.</span><span class="sxs-lookup"><span data-stu-id="8f359-143">See below for details.</span></span> |


<span data-ttu-id="8f359-144">Webhook URL-címet és a hasznos adatok között, amely a külső szolgáltatásnak továbbított adatok JSON formátumú, tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="8f359-144">Webhooks include a URL and a payload formatted in JSON that is the data sent to the external service.</span></span>  <span data-ttu-id="8f359-145">Alapértelmezés szerint a tartalom a következő táblázat tartalmazza az értékeket.</span><span class="sxs-lookup"><span data-stu-id="8f359-145">By default, the payload will include the values in the following table.</span></span>  <span data-ttu-id="8f359-146">Ha szeretné, ezek a hasznos adatok kicseréli a saját egyéni egy.</span><span class="sxs-lookup"><span data-stu-id="8f359-146">You can choose to replace this payload with a custom one of your own.</span></span>  <span data-ttu-id="8f359-147">Ebben az esetben használhatja a változók a táblázatban az egyes paraméterek egyéni adattartalmat értékük felvenni.</span><span class="sxs-lookup"><span data-stu-id="8f359-147">In that case you can use the variables in the table for each of the parameters to include their value in your custom payload.</span></span>

| <span data-ttu-id="8f359-148">Paraméter</span><span class="sxs-lookup"><span data-stu-id="8f359-148">Parameter</span></span> | <span data-ttu-id="8f359-149">Változó</span><span class="sxs-lookup"><span data-stu-id="8f359-149">Variable</span></span> | <span data-ttu-id="8f359-150">Leírás</span><span class="sxs-lookup"><span data-stu-id="8f359-150">Description</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="8f359-151">AlertRuleName</span><span class="sxs-lookup"><span data-stu-id="8f359-151">AlertRuleName</span></span> |<span data-ttu-id="8f359-152">#alertrulename</span><span class="sxs-lookup"><span data-stu-id="8f359-152">#alertrulename</span></span> |<span data-ttu-id="8f359-153">A riasztási szabály neve.</span><span class="sxs-lookup"><span data-stu-id="8f359-153">Name of the alert rule.</span></span> |
| <span data-ttu-id="8f359-154">AlertThresholdOperator</span><span class="sxs-lookup"><span data-stu-id="8f359-154">AlertThresholdOperator</span></span> |<span data-ttu-id="8f359-155">#thresholdoperator</span><span class="sxs-lookup"><span data-stu-id="8f359-155">#thresholdoperator</span></span> |<span data-ttu-id="8f359-156">A riasztási szabály operátor küszöbértéket.</span><span class="sxs-lookup"><span data-stu-id="8f359-156">Threshold operator for the alert rule.</span></span>  <span data-ttu-id="8f359-157">*Nagyobb, mint* vagy *kisebb, mint*.</span><span class="sxs-lookup"><span data-stu-id="8f359-157">*Greater than* or *Less than*.</span></span> |
| <span data-ttu-id="8f359-158">AlertThresholdValue</span><span class="sxs-lookup"><span data-stu-id="8f359-158">AlertThresholdValue</span></span> |<span data-ttu-id="8f359-159">#thresholdvalue</span><span class="sxs-lookup"><span data-stu-id="8f359-159">#thresholdvalue</span></span> |<span data-ttu-id="8f359-160">A riasztási szabály tartozó küszöbérték.</span><span class="sxs-lookup"><span data-stu-id="8f359-160">Threshold value for the alert rule.</span></span> |
| <span data-ttu-id="8f359-161">LinkToSearchResults</span><span class="sxs-lookup"><span data-stu-id="8f359-161">LinkToSearchResults</span></span> |<span data-ttu-id="8f359-162">#linktosearchresults</span><span class="sxs-lookup"><span data-stu-id="8f359-162">#linktosearchresults</span></span> |<span data-ttu-id="8f359-163">A lekérdezésből, amely a riasztás létrehozása a rekordok visszaadó Naplóelemzési napló keresési kapcsolódik.</span><span class="sxs-lookup"><span data-stu-id="8f359-163">Link to Log Analytics log search that returns the records from the query that created the alert.</span></span> |
| <span data-ttu-id="8f359-164">Attribútumhoz resultcount számlálót.</span><span class="sxs-lookup"><span data-stu-id="8f359-164">ResultCount</span></span> |<span data-ttu-id="8f359-165">#searchresultcount</span><span class="sxs-lookup"><span data-stu-id="8f359-165">#searchresultcount</span></span> |<span data-ttu-id="8f359-166">A keresési eredmények rekordok száma.</span><span class="sxs-lookup"><span data-stu-id="8f359-166">Number of records in the search results.</span></span> |
| <span data-ttu-id="8f359-167">SearchIntervalEndtimeUtc</span><span class="sxs-lookup"><span data-stu-id="8f359-167">SearchIntervalEndtimeUtc</span></span> |<span data-ttu-id="8f359-168">#searchintervalendtimeutc</span><span class="sxs-lookup"><span data-stu-id="8f359-168">#searchintervalendtimeutc</span></span> |<span data-ttu-id="8f359-169">Befejezési ideje UTC formátumban a lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="8f359-169">End time for the query in UTC format.</span></span> |
| <span data-ttu-id="8f359-170">SearchIntervalInSeconds</span><span class="sxs-lookup"><span data-stu-id="8f359-170">SearchIntervalInSeconds</span></span> |<span data-ttu-id="8f359-171">#searchinterval</span><span class="sxs-lookup"><span data-stu-id="8f359-171">#searchinterval</span></span> |<span data-ttu-id="8f359-172">A riasztási szabály időszak.</span><span class="sxs-lookup"><span data-stu-id="8f359-172">Time window for the alert rule.</span></span> |
| <span data-ttu-id="8f359-173">SearchIntervalStartTimeUtc</span><span class="sxs-lookup"><span data-stu-id="8f359-173">SearchIntervalStartTimeUtc</span></span> |<span data-ttu-id="8f359-174">#searchintervalstarttimeutc</span><span class="sxs-lookup"><span data-stu-id="8f359-174">#searchintervalstarttimeutc</span></span> |<span data-ttu-id="8f359-175">Indítsa el a lekérdezések ideje UTC formátumban.</span><span class="sxs-lookup"><span data-stu-id="8f359-175">Start time for the query in UTC format.</span></span> |
| <span data-ttu-id="8f359-176">SearchQuery</span><span class="sxs-lookup"><span data-stu-id="8f359-176">SearchQuery</span></span> |<span data-ttu-id="8f359-177">#searchquery</span><span class="sxs-lookup"><span data-stu-id="8f359-177">#searchquery</span></span> |<span data-ttu-id="8f359-178">Naplófájl-keresési lekérdezés a riasztási szabály által használt.</span><span class="sxs-lookup"><span data-stu-id="8f359-178">Log search query used by the alert rule.</span></span> |
| <span data-ttu-id="8f359-179">SearchResults</span><span class="sxs-lookup"><span data-stu-id="8f359-179">SearchResults</span></span> |<span data-ttu-id="8f359-180">Lásd az alábbi</span><span class="sxs-lookup"><span data-stu-id="8f359-180">See below</span></span> |<span data-ttu-id="8f359-181">A lekérdezés JSON formátumban rögzíti.</span><span class="sxs-lookup"><span data-stu-id="8f359-181">Records returned by the query in JSON format.</span></span>  <span data-ttu-id="8f359-182">Az első 5000 rekordok korlátozva.</span><span class="sxs-lookup"><span data-stu-id="8f359-182">Limited to the first 5,000 records.</span></span> |
| <span data-ttu-id="8f359-183">WorkspaceID</span><span class="sxs-lookup"><span data-stu-id="8f359-183">WorkspaceID</span></span> |<span data-ttu-id="8f359-184">#workspaceid</span><span class="sxs-lookup"><span data-stu-id="8f359-184">#workspaceid</span></span> |<span data-ttu-id="8f359-185">Az OMS-munkaterület azonosítója.</span><span class="sxs-lookup"><span data-stu-id="8f359-185">ID of your OMS workspace.</span></span> |

<span data-ttu-id="8f359-186">Például megadhatja a következő egyéni payload nevű egyetlen paramétert tartalmazó *szöveg*.</span><span class="sxs-lookup"><span data-stu-id="8f359-186">For example, you might specify the following custom payload that includes a single parameter called *text*.</span></span>  <span data-ttu-id="8f359-187">A szolgáltatás, amely behívja a webhook a ennek a paraméternek, akkor rendszer.</span><span class="sxs-lookup"><span data-stu-id="8f359-187">The service that this webhook calls would be expecting this parameter.</span></span>

    {
        "text":"#alertrulename fired with #searchresultcount over threshold of #thresholdvalue."
    }

<span data-ttu-id="8f359-188">Ez a példa hasznos volna oldja fel a következőhöz, ha a webhook.</span><span class="sxs-lookup"><span data-stu-id="8f359-188">This example payload would resolve to something like the following when sent to the webhook.</span></span>

    {
        "text":"My Alert Rule fired with 18 records over threshold of 10 ."
    }

<span data-ttu-id="8f359-189">A keresési eredmények belefoglalása az egyéni adattartalom, adja hozzá a következő sort a json-adattartalmat legfelső szintű tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="8f359-189">To include search results in a custom payload, add the following line as a top level property in the json payload.</span></span>  

    "IncludeSearchResults":true

<span data-ttu-id="8f359-190">Például egy egyéni adattartalom, amely tartalmazza a riasztás neve és a keresési eredmények létrehozásához használhatja a következő.</span><span class="sxs-lookup"><span data-stu-id="8f359-190">For example, to create a custom payload that includes just the alert name and the search results, you could use the following.</span></span> 

    {
       "alertname":"#alertrulename",
       "IncludeSearchResults":true
    }


<span data-ttu-id="8f359-191">Riasztási szabály létrehozása a webhook: egy külső szolgáltatás indítására átfogó példát keresztül végigvezetheti [riasztási webhook művelet létrehozása az OMS szolgáltatáshoz üzenetet küldeni a Slackhez](log-analytics-alerts-webhooks.md).</span><span class="sxs-lookup"><span data-stu-id="8f359-191">You can walk through a complete example of creating an alert rule with a webhook to start an external service at [Create an alert webhook action in OMS Log Analytics to send message to Slack](log-analytics-alerts-webhooks.md).</span></span>

## <a name="runbook-actions"></a><span data-ttu-id="8f359-192">Runbook-műveletek</span><span class="sxs-lookup"><span data-stu-id="8f359-192">Runbook actions</span></span>
<span data-ttu-id="8f359-193">Runbook műveletek az Azure Automationben runbook indítása.</span><span class="sxs-lookup"><span data-stu-id="8f359-193">Runbook actions start a runbook in Azure Automation.</span></span>  <span data-ttu-id="8f359-194">Ahhoz, hogy a művelet típusát, rendelkeznie kell a [Folyamatautomatizálási megoldása](log-analytics-add-solutions.md) telepítve, és az OMS-munkaterület konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="8f359-194">In order to use this type of action, you must have the [Automation solution](log-analytics-add-solutions.md) installed and configured in your OMS workspace.</span></span>  <span data-ttu-id="8f359-195">Az automation-fiókban az Automation-megoldás beállított runbookok közül választhat.</span><span class="sxs-lookup"><span data-stu-id="8f359-195">You can select from the runbooks in the automation account that you configured in the Automation solution.</span></span>

<span data-ttu-id="8f359-196">Runbook műveleteket igényelnek a tulajdonságok az alábbi táblázatban.</span><span class="sxs-lookup"><span data-stu-id="8f359-196">Runbook actions require the properties in the following table.</span></span>

| <span data-ttu-id="8f359-197">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="8f359-197">Property</span></span> | <span data-ttu-id="8f359-198">Leírás</span><span class="sxs-lookup"><span data-stu-id="8f359-198">Description</span></span> |
|:--- |:---|
| <span data-ttu-id="8f359-199">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="8f359-199">Runbook</span></span> | <span data-ttu-id="8f359-200">Ez a Runbook elindítja a riasztás létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="8f359-200">Runbook that you want to start when an alert is created.</span></span> |
| <span data-ttu-id="8f359-201">Futtassa a</span><span class="sxs-lookup"><span data-stu-id="8f359-201">Run on</span></span> | <span data-ttu-id="8f359-202">Adja meg **Azure** a runbook futtatásához a felhőben.</span><span class="sxs-lookup"><span data-stu-id="8f359-202">Specify **Azure** to run the runbook in the cloud.</span></span>  <span data-ttu-id="8f359-203">Adja meg **hibridfeldolgozó** a runbook futhat az ügynök [hibrid forgatókönyv-feldolgozó](../automation/automation-hybrid-runbook-worker.md ) telepítve.</span><span class="sxs-lookup"><span data-stu-id="8f359-203">Specify **Hybrid worker** to run the runbook on an agent with [Hybrid Runbook Worker](../automation/automation-hybrid-runbook-worker.md ) installed.</span></span>  |

<span data-ttu-id="8f359-204">Runbook műveletek indíthatja a runbook egy [webhook](../automation/automation-webhooks.md).</span><span class="sxs-lookup"><span data-stu-id="8f359-204">Runbook actions start the runbook using a [webhook](../automation/automation-webhooks.md).</span></span>  <span data-ttu-id="8f359-205">A riasztási szabály létrehozásakor az automatikusan létrehoz egy új webhook a runbook nevű **OMS riasztás szervizelési** GUID követ.</span><span class="sxs-lookup"><span data-stu-id="8f359-205">When you create the alert rule, it will automatically create a new webhook for the runbook with the name **OMS Alert Remediation** followed by a GUID.</span></span>  

<span data-ttu-id="8f359-206">Nem közvetlenül tölthető fel a runbook paramétereket, de a [$WebhookData paraméter](../automation/automation-webhooks.md) a riasztást, többek között az azt létrehozó napló keresési eredmények részleteit tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="8f359-206">You cannot directly populate any parameters of the runbook, but the [$WebhookData parameter](../automation/automation-webhooks.md) will include the details of the alert, including the results of the log search that created it.</span></span>  <span data-ttu-id="8f359-207">A runbook kell megadni **$WebhookData** úgy, hogy a riasztás tulajdonságai érhetők el a paramétert.</span><span class="sxs-lookup"><span data-stu-id="8f359-207">The runbook will need to define **$WebhookData** as a parameter for it to access the properties of the alert.</span></span>  <span data-ttu-id="8f359-208">A riasztási adatokat érhető el egyetlen tulajdonságot, json formátumban **SearchResults** a a **RequestBody** tulajdonsága **$WebhookData**.</span><span class="sxs-lookup"><span data-stu-id="8f359-208">The alert data is available in json format in a single property called **SearchResults** in the **RequestBody** property of **$WebhookData**.</span></span>  <span data-ttu-id="8f359-209">Ez az alábbi táblázatban a tulajdonságokkal fog rendelkezni.</span><span class="sxs-lookup"><span data-stu-id="8f359-209">This will have with the properties in the following table.</span></span>

| <span data-ttu-id="8f359-210">Csomópont</span><span class="sxs-lookup"><span data-stu-id="8f359-210">Node</span></span> | <span data-ttu-id="8f359-211">Leírás</span><span class="sxs-lookup"><span data-stu-id="8f359-211">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="8f359-212">id</span><span class="sxs-lookup"><span data-stu-id="8f359-212">id</span></span> |<span data-ttu-id="8f359-213">Elérési út és a keresés GUID.</span><span class="sxs-lookup"><span data-stu-id="8f359-213">Path and GUID of the search.</span></span> |
| <span data-ttu-id="8f359-214">__metadata</span><span class="sxs-lookup"><span data-stu-id="8f359-214">__metadata</span></span> |<span data-ttu-id="8f359-215">A riasztás rekordok száma és a keresési eredmények állapotát kapcsolatos információkat.</span><span class="sxs-lookup"><span data-stu-id="8f359-215">Information about the alert including the number of records and status of the search results.</span></span> |
| <span data-ttu-id="8f359-216">érték</span><span class="sxs-lookup"><span data-stu-id="8f359-216">value</span></span> |<span data-ttu-id="8f359-217">A keresési eredmények rekordokban külön bejegyzést.</span><span class="sxs-lookup"><span data-stu-id="8f359-217">Separate entry for each record in the search results.</span></span>  <span data-ttu-id="8f359-218">A bejegyzés a részleteket a tulajdonságok és értékek a rekord fog egyezni.</span><span class="sxs-lookup"><span data-stu-id="8f359-218">The details of the entry will match the properties and values of the record.</span></span> |

<span data-ttu-id="8f359-219">Például a következő runbook Ehhez bontsa ki a napló keresés által visszaadott rekordok, és rendelje hozzá minden egyes bejegyzés típusától függően különböző tulajdonságokat.</span><span class="sxs-lookup"><span data-stu-id="8f359-219">For example, the following runbook would extract the records returned by the log search  and assign different properties based on the type of each record.</span></span>  <span data-ttu-id="8f359-220">Vegye figyelembe, hogy a runbook elindítja átalakításával **RequestBody** JSON úgy, hogy az erőforrások az PowerShell objektumként.</span><span class="sxs-lookup"><span data-stu-id="8f359-220">Note that the runbook starts by converting **RequestBody** from json so that it can be worked with as an object in PowerShell.</span></span>

    param ( 
        [object]$WebhookData
    )

    $RequestBody = ConvertFrom-JSON -InputObject $WebhookData.RequestBody
    $Records     = $RequestBody.SearchResults.value

    foreach ($Record in $Records)
    {
        $Computer = $Record.Computer

        if ($Record.Type -eq 'Event')
        {
            $EventNo    = $Record.EventID
            $EventLevel = $Record.EventLevelName
            $EventData  = $Record.EventData
        }

        if ($Record.Type -eq 'Perf')
        {
            $Object    = $Record.ObjectName
            $Counter   = $Record.CounterName
            $Instance  = $Record.InstanceName
            $Value     = $Record.CounterValue
        }
    }


## <a name="next-steps"></a><span data-ttu-id="8f359-221">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8f359-221">Next steps</span></span>
- <span data-ttu-id="8f359-222">A forgatókönyv a [konfigurálása egy webook](log-analytics-alerts-webhooks.md) a riasztási szabályt.</span><span class="sxs-lookup"><span data-stu-id="8f359-222">Complete a walkthrough for [configuring a webook](log-analytics-alerts-webhooks.md) with an alert rule.</span></span>  
- <span data-ttu-id="8f359-223">Ismerje meg, hogyan írhat [az Azure Automation runbookjai](https://azure.microsoft.com/documentation/services/automation) riasztások által azonosított problémák megoldásáról.</span><span class="sxs-lookup"><span data-stu-id="8f359-223">Learn how to write [runbooks in Azure Automation](https://azure.microsoft.com/documentation/services/automation) to remediate problems identified by alerts.</span></span>