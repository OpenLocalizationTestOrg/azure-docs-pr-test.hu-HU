---
title: "a napló tevékenységriasztásokat használt aaaUnderstand hello webhook séma |} Microsoft Docs"
description: "További információk a hello sémája hello JSON, amikor egy tevékenység napló riasztás aktiválja feladott tooa webhook URL-CÍMÉT."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: johnkem
ms.openlocfilehash: 75562e0589222d3e392ea73eacfd7414a422d115
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="webhooks-for-azure-activity-log-alerts"></a><span data-ttu-id="eb87d-103">Az Azure tevékenység napló riasztásokhoz Webhookok</span><span class="sxs-lookup"><span data-stu-id="eb87d-103">Webhooks for Azure activity log alerts</span></span>
<span data-ttu-id="eb87d-104">Hello definíciójának művelet csoport részeként webhook végpontok tooreceive tevékenység napló riasztási értesítéseket is konfigurálhat.</span><span class="sxs-lookup"><span data-stu-id="eb87d-104">As part of hello definition of an action group, you can configure webhook endpoints tooreceive activity log alert notifications.</span></span> <span data-ttu-id="eb87d-105">A webhook irányíthatja a utófeldolgozási vagy egyéni műveletek értesítések tooother rendszerek.</span><span class="sxs-lookup"><span data-stu-id="eb87d-105">With webhooks, you can route these notifications tooother systems for post-processing or custom actions.</span></span> <span data-ttu-id="eb87d-106">Ez a cikk bemutatja, milyen hello hasznos hello HTTP POST tooa webhook tűnik.</span><span class="sxs-lookup"><span data-stu-id="eb87d-106">This article shows what hello payload for hello HTTP POST tooa webhook looks like.</span></span>

<span data-ttu-id="eb87d-107">További információ a napló tevékenységriasztásokat: hogyan túl[Azure tevékenység létrehozása napló riasztások](monitoring-activity-log-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="eb87d-107">For more information on activity log alerts, see how too[create Azure activity log alerts](monitoring-activity-log-alerts.md).</span></span>

<span data-ttu-id="eb87d-108">Információk a művelet csoportokon: hogyan túl[művelet csoportok létrehozása a](monitoring-action-groups.md).</span><span class="sxs-lookup"><span data-stu-id="eb87d-108">For information on action groups, see how too[create action groups](monitoring-action-groups.md).</span></span>

## <a name="authenticate-hello-webhook"></a><span data-ttu-id="eb87d-109">Hello webhook hitelesítéséhez</span><span class="sxs-lookup"><span data-stu-id="eb87d-109">Authenticate hello webhook</span></span>
<span data-ttu-id="eb87d-110">hello webhook opcionálisan használhatja engedélyezési jogkivonat-alapú hitelesítésre.</span><span class="sxs-lookup"><span data-stu-id="eb87d-110">hello webhook can optionally use token-based authorization for authentication.</span></span> <span data-ttu-id="eb87d-111">a webhook URI mentett jogkivonat-azonosítóval, például hello `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`.</span><span class="sxs-lookup"><span data-stu-id="eb87d-111">hello webhook URI is saved with a token ID, for example, `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`.</span></span>

## <a name="payload-schema"></a><span data-ttu-id="eb87d-112">Hasznos séma</span><span class="sxs-lookup"><span data-stu-id="eb87d-112">Payload schema</span></span>
<span data-ttu-id="eb87d-113">hello JSON-adattartalmat hello FELADÁS egy vagy több művelet található helytől hello hasznos data.context.activityLog.eventSource mező.</span><span class="sxs-lookup"><span data-stu-id="eb87d-113">hello JSON payload contained in hello POST operation differs based on hello payload's data.context.activityLog.eventSource field.</span></span>

###<a name="common"></a><span data-ttu-id="eb87d-114">Közös</span><span class="sxs-lookup"><span data-stu-id="eb87d-114">Common</span></span>
```json
{
    "schemaId": "Microsoft.Insights/activityLogs",
    "data": {
        "status": "Activated",
        "context": {
            "activityLog": {
                "channels": "Operation",
                "correlationId": "6ac88262-43be-4adf-a11c-bd2179852898",
                "eventSource": "Administrative",
                "eventTimestamp": "2017-03-29T15:43:08.0019532+00:00",
                "eventDataId": "8195a56a-85de-4663-943e-1a2bf401ad94",
                "level": "Informational",
                "operationName": "Microsoft.Insights/actionGroups/write",
                "operationId": "6ac88262-43be-4adf-a11c-bd2179852898",
                "status": "Started",
                "subStatus": "",
                "subscriptionId": "52c65f65-0518-4d37-9719-7dbbfc68c57a",
                "submissionTimestamp": "2017-03-29T15:43:20.3863637+00:00",
                ...
            }
        },
        "properties": {}
    }
}
```
###<a name="administrative"></a><span data-ttu-id="eb87d-115">Felügyeleti</span><span class="sxs-lookup"><span data-stu-id="eb87d-115">Administrative</span></span>
```json
{
    "schemaId": "Microsoft.Insights/activityLogs",
    "data": {
        "status": "Activated",
        "context": {
            "activityLog": {
                "authorization": {
                    "action": "Microsoft.Insights/actionGroups/write",
                    "scope": "/subscriptions/52c65f65-0518-4d37-9719-7dbbfc68c57b/resourceGroups/CONTOSO-TEST/providers/Microsoft.Insights/actionGroups/IncidentActions"
                },
                "claims": "{...}",
                "caller": "me@contoso.com",
                "description": "",
                "httpRequest": "{...}",
                "resourceId": "/subscriptions/52c65f65-0518-4d37-9719-7dbbfc68c57b/resourceGroups/CONTOSO-TEST/providers/Microsoft.Insights/actionGroups/IncidentActions",
                "resourceGroupName": "CONTOSO-TEST",
                "resourceProviderName": "Microsoft.Insights",
                "resourceType": "Microsoft.Insights/actionGroups"
            }
        },
        "properties": {}
    }
}

```
###<a name="servicehealth"></a><span data-ttu-id="eb87d-116">ServiceHealth</span><span class="sxs-lookup"><span data-stu-id="eb87d-116">ServiceHealth</span></span>
```json
{
    "schemaId": "unknown",
    "data": {
        "status": "Activated",
        "context": {
            "activityLog": {
                "properties": {
                    "title": "...",
                    "service": "...",
                    "region": "...",
                    "communication": "...",
                    "incidentType": "Incident",
                    "trackingId": "...",
                    "groupId": "...",
                    "impactStartTime": "3/29/2017 3:43:21 PM",
                    "impactMitigationTime": "3/29/2017 3:43:21 PM",
                    "eventCreationTime": "3/29/2017 3:43:21 PM",
                    "impactedServices": "[{...}]",
                    "defaultLanguageTitle": "...",
                    "defaultLanguageContent": "...",
                    "stage": "Active",
                    "communicationId": "...",
                    "version": "0.1"
                }
            }
        },
        "properties": {}
    }
}
```

<span data-ttu-id="eb87d-117">A szolgáltatás állapotának értesítési tevékenység napló riasztások adott séma részletekért lásd: [szolgáltatás állapotával kapcsolatos értesítésekre](monitoring-service-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="eb87d-117">For specific schema details on service health notification activity log alerts, see [Service health notifications](monitoring-service-notifications.md).</span></span>

<span data-ttu-id="eb87d-118">Más tevékenység napló minden riasztásról adott séma részletekért lásd: [hello Azure tevékenységnapló áttekintése](monitoring-overview-activity-logs.md).</span><span class="sxs-lookup"><span data-stu-id="eb87d-118">For specific schema details on all other activity log alerts, see [Overview of hello Azure activity log](monitoring-overview-activity-logs.md).</span></span>

| <span data-ttu-id="eb87d-119">Elem neve</span><span class="sxs-lookup"><span data-stu-id="eb87d-119">Element name</span></span> | <span data-ttu-id="eb87d-120">Leírás</span><span class="sxs-lookup"><span data-stu-id="eb87d-120">Description</span></span> |
| --- | --- |
| <span data-ttu-id="eb87d-121">status</span><span class="sxs-lookup"><span data-stu-id="eb87d-121">status</span></span> |<span data-ttu-id="eb87d-122">A metrika riasztások használt.</span><span class="sxs-lookup"><span data-stu-id="eb87d-122">Used for metric alerts.</span></span> <span data-ttu-id="eb87d-123">Mindig beállította túl "aktivált" tevékenység napló riasztások esetén.</span><span class="sxs-lookup"><span data-stu-id="eb87d-123">Always set too"activated" for activity log alerts.</span></span> |
| <span data-ttu-id="eb87d-124">A környezetben</span><span class="sxs-lookup"><span data-stu-id="eb87d-124">context</span></span> |<span data-ttu-id="eb87d-125">Hello esemény környezetében.</span><span class="sxs-lookup"><span data-stu-id="eb87d-125">Context of hello event.</span></span> |
| <span data-ttu-id="eb87d-126">resourceProviderName</span><span class="sxs-lookup"><span data-stu-id="eb87d-126">resourceProviderName</span></span> |<span data-ttu-id="eb87d-127">erőforrás-szolgáltató hello hello az érintett erőforrás.</span><span class="sxs-lookup"><span data-stu-id="eb87d-127">hello resource provider of hello impacted resource.</span></span> |
| <span data-ttu-id="eb87d-128">conditionType</span><span class="sxs-lookup"><span data-stu-id="eb87d-128">conditionType</span></span> |<span data-ttu-id="eb87d-129">Mindig "Event".</span><span class="sxs-lookup"><span data-stu-id="eb87d-129">Always "Event."</span></span> |
| <span data-ttu-id="eb87d-130">név</span><span class="sxs-lookup"><span data-stu-id="eb87d-130">name</span></span> |<span data-ttu-id="eb87d-131">Hello riasztási szabály neve.</span><span class="sxs-lookup"><span data-stu-id="eb87d-131">Name of hello alert rule.</span></span> |
| <span data-ttu-id="eb87d-132">id</span><span class="sxs-lookup"><span data-stu-id="eb87d-132">id</span></span> |<span data-ttu-id="eb87d-133">Erőforrás-azonosító hello riasztás.</span><span class="sxs-lookup"><span data-stu-id="eb87d-133">Resource ID of hello alert.</span></span> |
| <span data-ttu-id="eb87d-134">leírás</span><span class="sxs-lookup"><span data-stu-id="eb87d-134">description</span></span> |<span data-ttu-id="eb87d-135">Ha hello riasztást hoz létre a riasztás leírásában.</span><span class="sxs-lookup"><span data-stu-id="eb87d-135">Alert description set when hello alert is created.</span></span> |
| <span data-ttu-id="eb87d-136">subscriptionId</span><span class="sxs-lookup"><span data-stu-id="eb87d-136">subscriptionId</span></span> |<span data-ttu-id="eb87d-137">Az Azure előfizetés-azonosító.</span><span class="sxs-lookup"><span data-stu-id="eb87d-137">Azure subscription ID.</span></span> |
| <span data-ttu-id="eb87d-138">időbélyeg</span><span class="sxs-lookup"><span data-stu-id="eb87d-138">timestamp</span></span> |<span data-ttu-id="eb87d-139">Mely hello esemény lett létrehozva, amikor hello Azure szolgáltatás, amelynek a feldolgozása hello kérelem ideje.</span><span class="sxs-lookup"><span data-stu-id="eb87d-139">Time at which hello event was generated by hello Azure service that processed hello request.</span></span> |
| <span data-ttu-id="eb87d-140">resourceId</span><span class="sxs-lookup"><span data-stu-id="eb87d-140">resourceId</span></span> |<span data-ttu-id="eb87d-141">Erőforrás-azonosítója, hello érintett erőforrás.</span><span class="sxs-lookup"><span data-stu-id="eb87d-141">Resource ID of hello impacted resource.</span></span> |
| <span data-ttu-id="eb87d-142">erőforráscsoport-név</span><span class="sxs-lookup"><span data-stu-id="eb87d-142">resourceGroupName</span></span> |<span data-ttu-id="eb87d-143">Hello hello erőforráscsoport nevét érintett erőforrás.</span><span class="sxs-lookup"><span data-stu-id="eb87d-143">Name of hello resource group for hello impacted resource.</span></span> |
| <span data-ttu-id="eb87d-144">properties</span><span class="sxs-lookup"><span data-stu-id="eb87d-144">properties</span></span> |<span data-ttu-id="eb87d-145">Állítsa be a `<Key, Value>` párok (Ez azt jelenti, hogy `Dictionary<String, String>`), amely hello esemény részleteit tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="eb87d-145">Set of `<Key, Value>` pairs (that is, `Dictionary<String, String>`) that includes details about hello event.</span></span> |
| <span data-ttu-id="eb87d-146">Esemény</span><span class="sxs-lookup"><span data-stu-id="eb87d-146">event</span></span> |<span data-ttu-id="eb87d-147">Hello eseménnyel kapcsolatos metaadatokat tartalmazó elemet.</span><span class="sxs-lookup"><span data-stu-id="eb87d-147">Element that contains metadata about hello event.</span></span> |
| <span data-ttu-id="eb87d-148">Engedélyezési</span><span class="sxs-lookup"><span data-stu-id="eb87d-148">authorization</span></span> |<span data-ttu-id="eb87d-149">Szerepköralapú hozzáférés-vezérlés tulajdonságai hello hello esemény.</span><span class="sxs-lookup"><span data-stu-id="eb87d-149">hello Role-Based Access Control properties of hello event.</span></span> <span data-ttu-id="eb87d-150">Ezek a Tulajdonságok általában például hello művelet hello szerepkör és hello hatókör.</span><span class="sxs-lookup"><span data-stu-id="eb87d-150">These properties usually include hello action, hello role, and hello scope.</span></span> |
| <span data-ttu-id="eb87d-151">category</span><span class="sxs-lookup"><span data-stu-id="eb87d-151">category</span></span> |<span data-ttu-id="eb87d-152">Hello esemény kategóriáját.</span><span class="sxs-lookup"><span data-stu-id="eb87d-152">Category of hello event.</span></span> <span data-ttu-id="eb87d-153">Támogatott értékek a következők: felügyeleti, figyelmeztetés, biztonsági, ServiceHealth és javaslat.</span><span class="sxs-lookup"><span data-stu-id="eb87d-153">Supported values include Administrative, Alert, Security, ServiceHealth, and Recommendation.</span></span> |
| <span data-ttu-id="eb87d-154">hívó</span><span class="sxs-lookup"><span data-stu-id="eb87d-154">caller</span></span> |<span data-ttu-id="eb87d-155">Hello művelet, a jogcím vagy a rendelkezésre állás alapján SPN jogcím hello felhasználó e-mail címe.</span><span class="sxs-lookup"><span data-stu-id="eb87d-155">Email address of hello user who performed hello operation, UPN claim, or SPN claim based on availability.</span></span> <span data-ttu-id="eb87d-156">Bizonyos rendszerhívások null értékű lehet.</span><span class="sxs-lookup"><span data-stu-id="eb87d-156">Can be null for certain system calls.</span></span> |
| <span data-ttu-id="eb87d-157">correlationId</span><span class="sxs-lookup"><span data-stu-id="eb87d-157">correlationId</span></span> |<span data-ttu-id="eb87d-158">Általában egy GUID karakterlánc-formátum.</span><span class="sxs-lookup"><span data-stu-id="eb87d-158">Usually a GUID in string format.</span></span> <span data-ttu-id="eb87d-159">A correlationId események tartozik toohello nagyobb műveletet és általában megosztani az egy correlationId.</span><span class="sxs-lookup"><span data-stu-id="eb87d-159">Events with correlationId belong toohello same larger action and usually share a correlationId.</span></span> |
| <span data-ttu-id="eb87d-160">eventDescription</span><span class="sxs-lookup"><span data-stu-id="eb87d-160">eventDescription</span></span> |<span data-ttu-id="eb87d-161">Statikus szöveg hello esemény leírását.</span><span class="sxs-lookup"><span data-stu-id="eb87d-161">Static text description of hello event.</span></span> |
| <span data-ttu-id="eb87d-162">eventDataId</span><span class="sxs-lookup"><span data-stu-id="eb87d-162">eventDataId</span></span> |<span data-ttu-id="eb87d-163">Hello esemény egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="eb87d-163">Unique identifier for hello event.</span></span> |
| <span data-ttu-id="eb87d-164">Eseményforrás</span><span class="sxs-lookup"><span data-stu-id="eb87d-164">eventSource</span></span> |<span data-ttu-id="eb87d-165">Neve hello Azure-szolgáltatásra vagy infrastruktúrát, hogy létrehozott hello esemény.</span><span class="sxs-lookup"><span data-stu-id="eb87d-165">Name of hello Azure service or infrastructure that generated hello event.</span></span> |
| <span data-ttu-id="eb87d-166">httpRequest</span><span class="sxs-lookup"><span data-stu-id="eb87d-166">httpRequest</span></span> |<span data-ttu-id="eb87d-167">hello kérelem általában tartalmazza hello clientRequestId clientIpAddress és HTTP-metódust (például PUT).</span><span class="sxs-lookup"><span data-stu-id="eb87d-167">hello request usually includes hello clientRequestId, clientIpAddress, and HTTP method (for example, PUT).</span></span> |
| <span data-ttu-id="eb87d-168">szint</span><span class="sxs-lookup"><span data-stu-id="eb87d-168">level</span></span> |<span data-ttu-id="eb87d-169">Hello a következő értékek egyikét: kritikus, hiba, figyelmeztetés, tájékoztatás és részletes.</span><span class="sxs-lookup"><span data-stu-id="eb87d-169">One of hello following values: Critical, Error, Warning, Informational, and Verbose.</span></span> |
| <span data-ttu-id="eb87d-170">OperationID azonosítójú</span><span class="sxs-lookup"><span data-stu-id="eb87d-170">operationId</span></span> |<span data-ttu-id="eb87d-171">Általában egy GUID azonosító toosingle művelet megfelelő hello események között meg van osztva.</span><span class="sxs-lookup"><span data-stu-id="eb87d-171">Usually a GUID shared among hello events corresponding toosingle operation.</span></span> |
| <span data-ttu-id="eb87d-172">operationName</span><span class="sxs-lookup"><span data-stu-id="eb87d-172">operationName</span></span> |<span data-ttu-id="eb87d-173">Hello művelet neve.</span><span class="sxs-lookup"><span data-stu-id="eb87d-173">Name of hello operation.</span></span> |
| <span data-ttu-id="eb87d-174">properties</span><span class="sxs-lookup"><span data-stu-id="eb87d-174">properties</span></span> |<span data-ttu-id="eb87d-175">Hello esemény tulajdonságai.</span><span class="sxs-lookup"><span data-stu-id="eb87d-175">Properties of hello event.</span></span> |
| <span data-ttu-id="eb87d-176">status</span><span class="sxs-lookup"><span data-stu-id="eb87d-176">status</span></span> |<span data-ttu-id="eb87d-177">Karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="eb87d-177">String.</span></span> <span data-ttu-id="eb87d-178">Hello művelet állapotát.</span><span class="sxs-lookup"><span data-stu-id="eb87d-178">Status of hello operation.</span></span> <span data-ttu-id="eb87d-179">A gyakori értékek a következők: elindítva, folyamatban lévő, sikeres, sikertelen, aktív és megoldva.</span><span class="sxs-lookup"><span data-stu-id="eb87d-179">Common values include Started, In Progress, Succeeded, Failed, Active, and Resolved.</span></span> |
| <span data-ttu-id="eb87d-180">a részállapot</span><span class="sxs-lookup"><span data-stu-id="eb87d-180">subStatus</span></span> |<span data-ttu-id="eb87d-181">Általában tartalmazza a megfelelő REST-hívást hello hello HTTP-állapotkódot.</span><span class="sxs-lookup"><span data-stu-id="eb87d-181">Usually includes hello HTTP status code of hello corresponding REST call.</span></span> <span data-ttu-id="eb87d-182">A részállapot más karakterláncokat is tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="eb87d-182">It might also include other strings that describe a substatus.</span></span> <span data-ttu-id="eb87d-183">Közös részállapot érték OK (HTTP-állapotkód:: 200), létrehozott (HTTP-állapotkód:: 201-es), fogadja el (HTTP-állapotkód:: 202), a tartalom nem (HTTP-állapotkód: 204), hibás kérés (HTTP-állapotkód:: 400), nem található (HTTP-állapotkód: 404-es), ütközés (HTTP-állapotkód:: 409 ), Belső kiszolgálóhiba (HTTP-állapotkód: 500), a Service nem érhető el (HTTP-állapotkód: 503-as), és az átjáró időtúllépése (HTTP-állapotkód: 504).</span><span class="sxs-lookup"><span data-stu-id="eb87d-183">Common substatus values include OK (HTTP Status Code: 200), Created (HTTP Status Code: 201), Accepted (HTTP Status Code: 202), No Content (HTTP Status Code: 204), Bad Request (HTTP Status Code: 400), Not Found (HTTP Status Code: 404), Conflict (HTTP Status Code: 409), Internal Server Error (HTTP Status Code: 500), Service Unavailable (HTTP Status Code: 503), and Gateway Timeout (HTTP Status Code: 504).</span></span> |

## <a name="next-steps"></a><span data-ttu-id="eb87d-184">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="eb87d-184">Next steps</span></span>
* <span data-ttu-id="eb87d-185">[További tudnivalók hello tevékenységnapló](monitoring-overview-activity-logs.md).</span><span class="sxs-lookup"><span data-stu-id="eb87d-185">[Learn more about hello activity log](monitoring-overview-activity-logs.md).</span></span>
* <span data-ttu-id="eb87d-186">[Hajtsa végre az Azure automation-parancsfájlok (Runbookok) Azure riasztások](http://go.microsoft.com/fwlink/?LinkId=627081).</span><span class="sxs-lookup"><span data-stu-id="eb87d-186">[Execute Azure automation scripts (Runbooks) on Azure alerts](http://go.microsoft.com/fwlink/?LinkId=627081).</span></span>
* <span data-ttu-id="eb87d-187">[Használja a logic app toosend keresztül Twilio SMS az Azure-riasztás alapján](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app).</span><span class="sxs-lookup"><span data-stu-id="eb87d-187">[Use a logic app toosend an SMS via Twilio from an Azure alert](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app).</span></span> <span data-ttu-id="eb87d-188">Ebben a példában a metrika riasztások, de egy tevékenység napló alert módosított toowork lehet.</span><span class="sxs-lookup"><span data-stu-id="eb87d-188">This example is for metric alerts, but it can be modified toowork with an activity log alert.</span></span>
* <span data-ttu-id="eb87d-189">[A logic app toosend Azure riasztást Slack üzenetét használja](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app).</span><span class="sxs-lookup"><span data-stu-id="eb87d-189">[Use a logic app toosend a Slack message from an Azure alert](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app).</span></span> <span data-ttu-id="eb87d-190">Ebben a példában a metrika riasztások, de egy tevékenység napló alert módosított toowork lehet.</span><span class="sxs-lookup"><span data-stu-id="eb87d-190">This example is for metric alerts, but it can be modified toowork with an activity log alert.</span></span>
* <span data-ttu-id="eb87d-191">[Használja a logic app toosend, egy üzenet tooan Azure feldolgozási sor az Azure-riasztás alapján](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app).</span><span class="sxs-lookup"><span data-stu-id="eb87d-191">[Use a logic app toosend a message tooan Azure queue from an Azure alert](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app).</span></span> <span data-ttu-id="eb87d-192">Ebben a példában a metrika riasztások, de egy tevékenység napló alert módosított toowork lehet.</span><span class="sxs-lookup"><span data-stu-id="eb87d-192">This example is for metric alerts, but it can be modified toowork with an activity log alert.</span></span>
