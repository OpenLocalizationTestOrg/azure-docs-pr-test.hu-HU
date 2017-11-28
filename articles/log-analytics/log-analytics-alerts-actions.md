---
title: "az OMS szolgáltatáshoz aaaResponses tooalerts |} Microsoft Docs"
description: "Log Analytics riasztások határozza meg az OMS-adattárban lévő fontos adatokat és is proaktív értesítést küldenek, problémák vagy meg kíván hívni műveletek tooattempt toocorrect őket.  Ez a cikk ismerteti, hogyan toocreate riasztási szabály és a részletek hello különféle műveletek tarthat."
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
ms.openlocfilehash: d24bb726a96e7143985f111c0599dc4e7898b4f0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-actions-tooalert-rules-in-log-analytics"></a><span data-ttu-id="e31d2-104">A Naplóelemzési műveletek tooalert szabályok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e31d2-104">Add actions tooalert rules in Log Analytics</span></span>
<span data-ttu-id="e31d2-105">Ha egy [riasztást hoz létre a Naplóelemzési](log-analytics-alerts.md), lehetősége van hello a [konfigurálása hello riasztási szabály](log-analytics-alerts.md) tooperform egy vagy több művelet.</span><span class="sxs-lookup"><span data-stu-id="e31d2-105">When an [alert is created in Log Analytics](log-analytics-alerts.md), you have hello option of [configuring hello alert rule](log-analytics-alerts.md) tooperform one or more actions.</span></span>  <span data-ttu-id="e31d2-106">Ez a cikk ismerteti az elérhető különböző műveleteket hello és a részletek konfigurálásával összes típusához.</span><span class="sxs-lookup"><span data-stu-id="e31d2-106">This article describes hello different actions that are available and details on configuring each kind.</span></span>

| <span data-ttu-id="e31d2-107">Műveletek</span><span class="sxs-lookup"><span data-stu-id="e31d2-107">Action</span></span> | <span data-ttu-id="e31d2-108">Leírás</span><span class="sxs-lookup"><span data-stu-id="e31d2-108">Description</span></span> |
|:--|:--|
| [<span data-ttu-id="e31d2-109">E-mailek</span><span class="sxs-lookup"><span data-stu-id="e31d2-109">Email</span></span>](#email-actions) | <span data-ttu-id="e31d2-110">Hello adatokkal hello riasztási tooone vagy további címzettek e-mail küldése.</span><span class="sxs-lookup"><span data-stu-id="e31d2-110">Send an e-mail with hello details of hello alert tooone or more recipients.</span></span> |
| [<span data-ttu-id="e31d2-111">Webhook</span><span class="sxs-lookup"><span data-stu-id="e31d2-111">Webhook</span></span>](#webhook-actions) | <span data-ttu-id="e31d2-112">Egy külső folyamatban egy HTTP POST kérelemben keresztül meghívni.</span><span class="sxs-lookup"><span data-stu-id="e31d2-112">Invoke an external process through a single HTTP POST request.</span></span> |
| [<span data-ttu-id="e31d2-113">A Runbook</span><span class="sxs-lookup"><span data-stu-id="e31d2-113">Runbook</span></span>](#runbook-actions) | <span data-ttu-id="e31d2-114">Elindít egy forgatókönyvet az Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="e31d2-114">Start a runbook in Azure Automation.</span></span> |


## <a name="email-actions"></a><span data-ttu-id="e31d2-115">E-mailek műveletek</span><span class="sxs-lookup"><span data-stu-id="e31d2-115">Email actions</span></span>
<span data-ttu-id="e31d2-116">E-mailek műveletek hello adatokkal hello riasztási tooone vagy további címzettek e-mail küldése.</span><span class="sxs-lookup"><span data-stu-id="e31d2-116">Email actions send an e-mail with hello details of hello alert tooone or more recipients.</span></span>  <span data-ttu-id="e31d2-117">Hello e-mail tárgya hello is megadhat, de annak tartalma Naplóelemzési által összeállított szabványos formátumban.</span><span class="sxs-lookup"><span data-stu-id="e31d2-117">You can specify hello subject of hello mail, but it's content is a standard format constructed by Log Analytics.</span></span>  <span data-ttu-id="e31d2-118">A hello napló keresés által visszaadott tooten rekordok, továbbá toodetails hello riasztás például hello nevét összefoglaló információkat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="e31d2-118">It includes summary information such as hello name of hello alert in addition toodetails of up tooten records returned by hello log search.</span></span>  <span data-ttu-id="e31d2-119">A Naplóelemzési, visszatér a rekordok hello teljes készletét, hogy a lekérdezés hivatkozás tooa napló keresés is tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="e31d2-119">It also includes a link tooa log search in Log Analytics that will return hello entire set of records from that query.</span></span>   <span data-ttu-id="e31d2-120">hello hello mail küldője *a Microsoft Operations Management Suite Team &lt; noreply@oms.microsoft.com &gt;* .</span><span class="sxs-lookup"><span data-stu-id="e31d2-120">hello sender of hello mail is *Microsoft Operations Management Suite Team &lt;noreply@oms.microsoft.com&gt;*.</span></span> 

<span data-ttu-id="e31d2-121">E-mailek műveletek a következő táblázat hello hello tulajdonságok szükséges.</span><span class="sxs-lookup"><span data-stu-id="e31d2-121">Email actions require hello properties in hello following table.</span></span>

| <span data-ttu-id="e31d2-122">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="e31d2-122">Property</span></span> | <span data-ttu-id="e31d2-123">Leírás</span><span class="sxs-lookup"><span data-stu-id="e31d2-123">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="e31d2-124">Tárgy</span><span class="sxs-lookup"><span data-stu-id="e31d2-124">Subject</span></span> |<span data-ttu-id="e31d2-125">Tulajdonos hello e-mailben.</span><span class="sxs-lookup"><span data-stu-id="e31d2-125">Subject in hello email.</span></span>  <span data-ttu-id="e31d2-126">Üdvözlő e-mail törzsét hello nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="e31d2-126">You cannot modify hello body of hello mail.</span></span> |
| <span data-ttu-id="e31d2-127">Címzettek</span><span class="sxs-lookup"><span data-stu-id="e31d2-127">Recipients</span></span> |<span data-ttu-id="e31d2-128">Címzett e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="e31d2-128">Addresses of all e-mail recipients.</span></span>  <span data-ttu-id="e31d2-129">Ha ad meg egynél több címet, majd külön hello címeket pontosvesszővel (;).</span><span class="sxs-lookup"><span data-stu-id="e31d2-129">If you specify more than one address, then separate hello addresses with a semicolon (;).</span></span> |


## <a name="webhook-actions"></a><span data-ttu-id="e31d2-130">Webhookműveletek</span><span class="sxs-lookup"><span data-stu-id="e31d2-130">Webhook actions</span></span>

<span data-ttu-id="e31d2-131">Webhookműveletek lehetővé teszik a tooinvoke keresztül egy HTTP POST kérelemben egy külső folyamatban.</span><span class="sxs-lookup"><span data-stu-id="e31d2-131">Webhook actions allow you tooinvoke an external process through a single HTTP POST request.</span></span>  <span data-ttu-id="e31d2-132">meghívott hello szolgáltatást kell webhookok támogatja, és határozza meg, hogyan fogja használni a tartalom kap.</span><span class="sxs-lookup"><span data-stu-id="e31d2-132">hello service being called should support webhooks and determine how it will use any payload it receives.</span></span>  <span data-ttu-id="e31d2-133">Emellett a REST API-t nem támogató kifejezetten webhookokkal, amíg hello kérést a formátumban kell megadni, hogy megértette a API hello hívhatja meg.</span><span class="sxs-lookup"><span data-stu-id="e31d2-133">You could also call a REST API that doesn't specifically support webhooks as long as hello request is in a format that hello API understands.</span></span>  <span data-ttu-id="e31d2-134">Példák a webhook válasz tooan figyelmeztető üzenet üzenetet küld [Slackhez](http://slack.com) , vagy hozzon létre egy incidenst a [PagerDuty](http://pagerduty.com/).</span><span class="sxs-lookup"><span data-stu-id="e31d2-134">Examples of using a webhook in response tooan alert are sending a message in [Slack](http://slack.com) or creating an incident in [PagerDuty](http://pagerduty.com/).</span></span>  <span data-ttu-id="e31d2-135">A webhook toocall Slackhez a riasztási szabály létrehozásának részletes útmutatást érhető el: [Naplóelemzési riasztásokban Webhookok](log-analytics-alerts-webhooks.md).</span><span class="sxs-lookup"><span data-stu-id="e31d2-135">A complete walkthrough of creating an alert rule with a webhook toocall Slack is available at [Webhooks in Log Analytics alerts](log-analytics-alerts-webhooks.md).</span></span>

<span data-ttu-id="e31d2-136">Webhookműveletek hello tulajdonságait a következő táblázat hello igényelnek.</span><span class="sxs-lookup"><span data-stu-id="e31d2-136">Webhook actions require hello properties in hello following table.</span></span>

| <span data-ttu-id="e31d2-137">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="e31d2-137">Property</span></span> | <span data-ttu-id="e31d2-138">Leírás</span><span class="sxs-lookup"><span data-stu-id="e31d2-138">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="e31d2-139">Webhook URL-CÍMÉT</span><span class="sxs-lookup"><span data-stu-id="e31d2-139">Webhook URL</span></span> |<span data-ttu-id="e31d2-140">hello hello webhook URL-címe</span><span class="sxs-lookup"><span data-stu-id="e31d2-140">hello URL of hello webhook.</span></span> |
| <span data-ttu-id="e31d2-141">Egyéni JSON-adattartalmat</span><span class="sxs-lookup"><span data-stu-id="e31d2-141">Custom JSON payload</span></span> |<span data-ttu-id="e31d2-142">Egyéni adattartalom toosend a hello webhook.</span><span class="sxs-lookup"><span data-stu-id="e31d2-142">Custom payload toosend with hello webhook.</span></span>  <span data-ttu-id="e31d2-143">További információ alább olvasható.</span><span class="sxs-lookup"><span data-stu-id="e31d2-143">See below for details.</span></span> |


<span data-ttu-id="e31d2-144">Webhook URL-címet tartalmazza, és a hasznos adatok között, amelyek hello JSON formátumú, toohello külső szolgáltatás küldött.</span><span class="sxs-lookup"><span data-stu-id="e31d2-144">Webhooks include a URL and a payload formatted in JSON that is hello data sent toohello external service.</span></span>  <span data-ttu-id="e31d2-145">Alapértelmezés szerint hello hasznos tartalmazza a következő táblázat hello hello értékeket.</span><span class="sxs-lookup"><span data-stu-id="e31d2-145">By default, hello payload will include hello values in hello following table.</span></span>  <span data-ttu-id="e31d2-146">Ez hasznos a saját egyéni egy választható tooreplace.</span><span class="sxs-lookup"><span data-stu-id="e31d2-146">You can choose tooreplace this payload with a custom one of your own.</span></span>  <span data-ttu-id="e31d2-147">Ebben az esetben használhatja hello változókat hello táblázatban az egyes hello paraméterek tooinclude azok előnyeit az egyéni tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="e31d2-147">In that case you can use hello variables in hello table for each of hello parameters tooinclude their value in your custom payload.</span></span>

| <span data-ttu-id="e31d2-148">Paraméter</span><span class="sxs-lookup"><span data-stu-id="e31d2-148">Parameter</span></span> | <span data-ttu-id="e31d2-149">Változó</span><span class="sxs-lookup"><span data-stu-id="e31d2-149">Variable</span></span> | <span data-ttu-id="e31d2-150">Leírás</span><span class="sxs-lookup"><span data-stu-id="e31d2-150">Description</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="e31d2-151">AlertRuleName</span><span class="sxs-lookup"><span data-stu-id="e31d2-151">AlertRuleName</span></span> |<span data-ttu-id="e31d2-152">#alertrulename</span><span class="sxs-lookup"><span data-stu-id="e31d2-152">#alertrulename</span></span> |<span data-ttu-id="e31d2-153">Hello riasztási szabály neve.</span><span class="sxs-lookup"><span data-stu-id="e31d2-153">Name of hello alert rule.</span></span> |
| <span data-ttu-id="e31d2-154">AlertThresholdOperator</span><span class="sxs-lookup"><span data-stu-id="e31d2-154">AlertThresholdOperator</span></span> |<span data-ttu-id="e31d2-155">#thresholdoperator</span><span class="sxs-lookup"><span data-stu-id="e31d2-155">#thresholdoperator</span></span> |<span data-ttu-id="e31d2-156">Hello riasztási szabály operátor küszöbértéket.</span><span class="sxs-lookup"><span data-stu-id="e31d2-156">Threshold operator for hello alert rule.</span></span>  <span data-ttu-id="e31d2-157">*Nagyobb, mint* vagy *kisebb, mint*.</span><span class="sxs-lookup"><span data-stu-id="e31d2-157">*Greater than* or *Less than*.</span></span> |
| <span data-ttu-id="e31d2-158">AlertThresholdValue</span><span class="sxs-lookup"><span data-stu-id="e31d2-158">AlertThresholdValue</span></span> |<span data-ttu-id="e31d2-159">#thresholdvalue</span><span class="sxs-lookup"><span data-stu-id="e31d2-159">#thresholdvalue</span></span> |<span data-ttu-id="e31d2-160">Riasztási szabály hello tartozó küszöbérték.</span><span class="sxs-lookup"><span data-stu-id="e31d2-160">Threshold value for hello alert rule.</span></span> |
| <span data-ttu-id="e31d2-161">LinkToSearchResults</span><span class="sxs-lookup"><span data-stu-id="e31d2-161">LinkToSearchResults</span></span> |<span data-ttu-id="e31d2-162">#linktosearchresults</span><span class="sxs-lookup"><span data-stu-id="e31d2-162">#linktosearchresults</span></span> |<span data-ttu-id="e31d2-163">Hivatkozás tooLog Analytics napló keresési visszaadó hello rekordok hello riasztást létrehozó hello lekérdezésből.</span><span class="sxs-lookup"><span data-stu-id="e31d2-163">Link tooLog Analytics log search that returns hello records from hello query that created hello alert.</span></span> |
| <span data-ttu-id="e31d2-164">Attribútumhoz resultcount számlálót.</span><span class="sxs-lookup"><span data-stu-id="e31d2-164">ResultCount</span></span> |<span data-ttu-id="e31d2-165">#searchresultcount</span><span class="sxs-lookup"><span data-stu-id="e31d2-165">#searchresultcount</span></span> |<span data-ttu-id="e31d2-166">A találatok között hello rekordok száma.</span><span class="sxs-lookup"><span data-stu-id="e31d2-166">Number of records in hello search results.</span></span> |
| <span data-ttu-id="e31d2-167">SearchIntervalEndtimeUtc</span><span class="sxs-lookup"><span data-stu-id="e31d2-167">SearchIntervalEndtimeUtc</span></span> |<span data-ttu-id="e31d2-168">#searchintervalendtimeutc</span><span class="sxs-lookup"><span data-stu-id="e31d2-168">#searchintervalendtimeutc</span></span> |<span data-ttu-id="e31d2-169">Befejezési ideje UTC formátumban hello lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="e31d2-169">End time for hello query in UTC format.</span></span> |
| <span data-ttu-id="e31d2-170">SearchIntervalInSeconds</span><span class="sxs-lookup"><span data-stu-id="e31d2-170">SearchIntervalInSeconds</span></span> |<span data-ttu-id="e31d2-171">#searchinterval</span><span class="sxs-lookup"><span data-stu-id="e31d2-171">#searchinterval</span></span> |<span data-ttu-id="e31d2-172">Riasztási szabály hello időszak.</span><span class="sxs-lookup"><span data-stu-id="e31d2-172">Time window for hello alert rule.</span></span> |
| <span data-ttu-id="e31d2-173">SearchIntervalStartTimeUtc</span><span class="sxs-lookup"><span data-stu-id="e31d2-173">SearchIntervalStartTimeUtc</span></span> |<span data-ttu-id="e31d2-174">#searchintervalstarttimeutc</span><span class="sxs-lookup"><span data-stu-id="e31d2-174">#searchintervalstarttimeutc</span></span> |<span data-ttu-id="e31d2-175">Indítsa el a hello lekérdezések ideje UTC formátumban.</span><span class="sxs-lookup"><span data-stu-id="e31d2-175">Start time for hello query in UTC format.</span></span> |
| <span data-ttu-id="e31d2-176">SearchQuery</span><span class="sxs-lookup"><span data-stu-id="e31d2-176">SearchQuery</span></span> |<span data-ttu-id="e31d2-177">#searchquery</span><span class="sxs-lookup"><span data-stu-id="e31d2-177">#searchquery</span></span> |<span data-ttu-id="e31d2-178">Naplófájl-keresési lekérdezés hello riasztási szabály által használt.</span><span class="sxs-lookup"><span data-stu-id="e31d2-178">Log search query used by hello alert rule.</span></span> |
| <span data-ttu-id="e31d2-179">SearchResults</span><span class="sxs-lookup"><span data-stu-id="e31d2-179">SearchResults</span></span> |<span data-ttu-id="e31d2-180">Lásd az alábbi</span><span class="sxs-lookup"><span data-stu-id="e31d2-180">See below</span></span> |<span data-ttu-id="e31d2-181">JSON formátumban hello lekérdezés által visszaadott rekordok.</span><span class="sxs-lookup"><span data-stu-id="e31d2-181">Records returned by hello query in JSON format.</span></span>  <span data-ttu-id="e31d2-182">Korlátozott toohello első 5000 rögzíti.</span><span class="sxs-lookup"><span data-stu-id="e31d2-182">Limited toohello first 5,000 records.</span></span> |
| <span data-ttu-id="e31d2-183">WorkspaceID</span><span class="sxs-lookup"><span data-stu-id="e31d2-183">WorkspaceID</span></span> |<span data-ttu-id="e31d2-184">#workspaceid</span><span class="sxs-lookup"><span data-stu-id="e31d2-184">#workspaceid</span></span> |<span data-ttu-id="e31d2-185">Az OMS-munkaterület azonosítója.</span><span class="sxs-lookup"><span data-stu-id="e31d2-185">ID of your OMS workspace.</span></span> |

<span data-ttu-id="e31d2-186">Például megadhatja a következő nevű egyetlen paramétert tartalmazó egyéni adattartalom hello *szöveg*.</span><span class="sxs-lookup"><span data-stu-id="e31d2-186">For example, you might specify hello following custom payload that includes a single parameter called *text*.</span></span>  <span data-ttu-id="e31d2-187">hello szolgáltatást, amely behívja a webhook a ennek a paraméternek, akkor rendszer.</span><span class="sxs-lookup"><span data-stu-id="e31d2-187">hello service that this webhook calls would be expecting this parameter.</span></span>

    {
        "text":"#alertrulename fired with #searchresultcount over threshold of #thresholdvalue."
    }

<span data-ttu-id="e31d2-188">A példa hasznos oldja meg a következő esetekben hello például toosomething küldött toohello webhook.</span><span class="sxs-lookup"><span data-stu-id="e31d2-188">This example payload would resolve toosomething like hello following when sent toohello webhook.</span></span>

    {
        "text":"My Alert Rule fired with 18 records over threshold of 10 ."
    }

<span data-ttu-id="e31d2-189">egyéni payloadban tooinclude találatok hello json-adattartalmat a legfelső szintű tulajdonságként a sorban következő hello hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="e31d2-189">tooinclude search results in a custom payload, add hello following line as a top level property in hello json payload.</span></span>  

    "IncludeSearchResults":true

<span data-ttu-id="e31d2-190">Például egy egyéni adattartalom, amely tartalmazza az ebben az esetben a hello riasztás neve és a keresési eredmények hello toocreate, használhatja a következő hello.</span><span class="sxs-lookup"><span data-stu-id="e31d2-190">For example, toocreate a custom payload that includes just hello alert name and hello search results, you could use hello following.</span></span> 

    {
       "alertname":"#alertrulename",
       "IncludeSearchResults":true
    }


<span data-ttu-id="e31d2-191">A webhook toostart egy riasztási szabály létrehozása egy külső szolgáltatás átfogó példát keresztül végigvezetheti [riasztási webhook művelet létrehozása az OMS szolgáltatáshoz toosend üzenet tooSlack](log-analytics-alerts-webhooks.md).</span><span class="sxs-lookup"><span data-stu-id="e31d2-191">You can walk through a complete example of creating an alert rule with a webhook toostart an external service at [Create an alert webhook action in OMS Log Analytics toosend message tooSlack](log-analytics-alerts-webhooks.md).</span></span>

## <a name="runbook-actions"></a><span data-ttu-id="e31d2-192">Runbook-műveletek</span><span class="sxs-lookup"><span data-stu-id="e31d2-192">Runbook actions</span></span>
<span data-ttu-id="e31d2-193">Runbook műveletek az Azure Automationben runbook indítása.</span><span class="sxs-lookup"><span data-stu-id="e31d2-193">Runbook actions start a runbook in Azure Automation.</span></span>  <span data-ttu-id="e31d2-194">A rendezés toouse a művelet típusát, akkor hello [Folyamatautomatizálási megoldása](log-analytics-add-solutions.md) telepítve, és az OMS-munkaterület konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="e31d2-194">In order toouse this type of action, you must have hello [Automation solution](log-analytics-add-solutions.md) installed and configured in your OMS workspace.</span></span>  <span data-ttu-id="e31d2-195">Választhat hello runbookokat hello Folyamatautomatizálási megoldása során konfigurált hello automation-fiókban.</span><span class="sxs-lookup"><span data-stu-id="e31d2-195">You can select from hello runbooks in hello automation account that you configured in hello Automation solution.</span></span>

<span data-ttu-id="e31d2-196">Runbook-műveletek a következő táblázat hello hello tulajdonságok szükséges.</span><span class="sxs-lookup"><span data-stu-id="e31d2-196">Runbook actions require hello properties in hello following table.</span></span>

| <span data-ttu-id="e31d2-197">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="e31d2-197">Property</span></span> | <span data-ttu-id="e31d2-198">Leírás</span><span class="sxs-lookup"><span data-stu-id="e31d2-198">Description</span></span> |
|:--- |:---|
| <span data-ttu-id="e31d2-199">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="e31d2-199">Runbook</span></span> | <span data-ttu-id="e31d2-200">A Runbook toostart szeretné, ha riasztás jön létre.</span><span class="sxs-lookup"><span data-stu-id="e31d2-200">Runbook that you want toostart when an alert is created.</span></span> |
| <span data-ttu-id="e31d2-201">Futtassa a</span><span class="sxs-lookup"><span data-stu-id="e31d2-201">Run on</span></span> | <span data-ttu-id="e31d2-202">Adja meg **Azure** toorun hello runbook hello felhőben.</span><span class="sxs-lookup"><span data-stu-id="e31d2-202">Specify **Azure** toorun hello runbook in hello cloud.</span></span>  <span data-ttu-id="e31d2-203">Adja meg **hibridfeldolgozó** toorun hello runbook az ügynökön [hibrid forgatókönyv-feldolgozó](../automation/automation-hybrid-runbook-worker.md ) telepítve.</span><span class="sxs-lookup"><span data-stu-id="e31d2-203">Specify **Hybrid worker** toorun hello runbook on an agent with [Hybrid Runbook Worker](../automation/automation-hybrid-runbook-worker.md ) installed.</span></span>  |

<span data-ttu-id="e31d2-204">Runbook műveletek indíthatja hello runbook egy [webhook](../automation/automation-webhooks.md).</span><span class="sxs-lookup"><span data-stu-id="e31d2-204">Runbook actions start hello runbook using a [webhook](../automation/automation-webhooks.md).</span></span>  <span data-ttu-id="e31d2-205">Hello riasztási szabályt hoz létre, ha az automatikusan létrehoz egy új webhook hello runbook hello nevű **OMS riasztás szervizelési** GUID követ.</span><span class="sxs-lookup"><span data-stu-id="e31d2-205">When you create hello alert rule, it will automatically create a new webhook for hello runbook with hello name **OMS Alert Remediation** followed by a GUID.</span></span>  

<span data-ttu-id="e31d2-206">Megadja az közvetlenül nem hello runbook paramétereket feltöltéséhez, de hello [$WebhookData paraméter](../automation/automation-webhooks.md) tartalmazzák a hello hello riasztás hello eredményeiben hello napló keresése, amely létrehozta.</span><span class="sxs-lookup"><span data-stu-id="e31d2-206">You cannot directly populate any parameters of hello runbook, but hello [$WebhookData parameter](../automation/automation-webhooks.md) will include hello details of hello alert, including hello results of hello log search that created it.</span></span>  <span data-ttu-id="e31d2-207">hello runbook kell toodefine **$WebhookData** az paraméterként tooaccess hello hello riasztás tulajdonságai.</span><span class="sxs-lookup"><span data-stu-id="e31d2-207">hello runbook will need toodefine **$WebhookData** as a parameter for it tooaccess hello properties of hello alert.</span></span>  <span data-ttu-id="e31d2-208">hello riasztási adatokat érhető el egyetlen tulajdonságot, json formátumban **SearchResults** a hello **RequestBody** tulajdonsága **$WebhookData**.</span><span class="sxs-lookup"><span data-stu-id="e31d2-208">hello alert data is available in json format in a single property called **SearchResults** in hello **RequestBody** property of **$WebhookData**.</span></span>  <span data-ttu-id="e31d2-209">Ez a következő táblázat hello hello tulajdonságokkal fog rendelkezni.</span><span class="sxs-lookup"><span data-stu-id="e31d2-209">This will have with hello properties in hello following table.</span></span>

| <span data-ttu-id="e31d2-210">Csomópont</span><span class="sxs-lookup"><span data-stu-id="e31d2-210">Node</span></span> | <span data-ttu-id="e31d2-211">Leírás</span><span class="sxs-lookup"><span data-stu-id="e31d2-211">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="e31d2-212">id</span><span class="sxs-lookup"><span data-stu-id="e31d2-212">id</span></span> |<span data-ttu-id="e31d2-213">Elérési út és hello keresési GUID Azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="e31d2-213">Path and GUID of hello search.</span></span> |
| <span data-ttu-id="e31d2-214">__metadata</span><span class="sxs-lookup"><span data-stu-id="e31d2-214">__metadata</span></span> |<span data-ttu-id="e31d2-215">Hello riasztási többek között a következőket hello rekordok száma és a keresési eredmények hello állapotával kapcsolatos adatokat.</span><span class="sxs-lookup"><span data-stu-id="e31d2-215">Information about hello alert including hello number of records and status of hello search results.</span></span> |
| <span data-ttu-id="e31d2-216">érték</span><span class="sxs-lookup"><span data-stu-id="e31d2-216">value</span></span> |<span data-ttu-id="e31d2-217">A találatok között hello rekordokban külön bejegyzést.</span><span class="sxs-lookup"><span data-stu-id="e31d2-217">Separate entry for each record in hello search results.</span></span>  <span data-ttu-id="e31d2-218">hello bejegyzés hello részleteit hello tulajdonságok és értékek hello rekord fog egyezni.</span><span class="sxs-lookup"><span data-stu-id="e31d2-218">hello details of hello entry will match hello properties and values of hello record.</span></span> |

<span data-ttu-id="e31d2-219">Például hello következő runbook volna kinyerése hello napló keresés által visszaadott hello rögzíti és rendelje hozzá mindegyik rekorddal hello típusától függően különböző tulajdonságokat.</span><span class="sxs-lookup"><span data-stu-id="e31d2-219">For example, hello following runbook would extract hello records returned by hello log search  and assign different properties based on hello type of each record.</span></span>  <span data-ttu-id="e31d2-220">Vegye figyelembe a hello runbook elindul átalakításával **RequestBody** JSON úgy, hogy az erőforrások az PowerShell objektumként.</span><span class="sxs-lookup"><span data-stu-id="e31d2-220">Note that hello runbook starts by converting **RequestBody** from json so that it can be worked with as an object in PowerShell.</span></span>

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


## <a name="next-steps"></a><span data-ttu-id="e31d2-221">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e31d2-221">Next steps</span></span>
- <span data-ttu-id="e31d2-222">A forgatókönyv a [konfigurálása egy webook](log-analytics-alerts-webhooks.md) a riasztási szabályt.</span><span class="sxs-lookup"><span data-stu-id="e31d2-222">Complete a walkthrough for [configuring a webook](log-analytics-alerts-webhooks.md) with an alert rule.</span></span>  
- <span data-ttu-id="e31d2-223">Megtudhatja, hogyan toowrite [az Azure Automation runbookjai](https://azure.microsoft.com/documentation/services/automation) riasztások által azonosított tooremediate problémák.</span><span class="sxs-lookup"><span data-stu-id="e31d2-223">Learn how toowrite [runbooks in Azure Automation](https://azure.microsoft.com/documentation/services/automation) tooremediate problems identified by alerts.</span></span>
