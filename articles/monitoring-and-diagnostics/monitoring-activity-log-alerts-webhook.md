---
title: "A webhook séma napló tevékenységriasztásokat használt megértése |} Microsoft Docs"
description: "További tudnivalók az fel az a webhook URL-CÍMÉT egy figyelmeztetés a napló aktiválja JSON-séma."
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
ms.openlocfilehash: 75c71bcd16573d4f4dd3377c623aa9b414aa3906
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="webhooks-for-azure-activity-log-alerts"></a><span data-ttu-id="d2fab-103">Az Azure tevékenység napló riasztásokhoz Webhookok</span><span class="sxs-lookup"><span data-stu-id="d2fab-103">Webhooks for Azure activity log alerts</span></span>
<span data-ttu-id="d2fab-104">Egy művelet csoport definíciójának részeként tevékenység napló riasztási értesítések fogadásának webhook végpontok is konfigurálhat.</span><span class="sxs-lookup"><span data-stu-id="d2fab-104">As part of the definition of an action group, you can configure webhook endpoints to receive activity log alert notifications.</span></span> <span data-ttu-id="d2fab-105">A webhook a más rendszerekkel utófeldolgozási vagy egyéni műveletek irányíthatja a ezek az értesítések.</span><span class="sxs-lookup"><span data-stu-id="d2fab-105">With webhooks, you can route these notifications to other systems for post-processing or custom actions.</span></span> <span data-ttu-id="d2fab-106">Ez a cikk bemutatja, mi a hasznos a HTTP POST egy webhook a következőhöz hasonló.</span><span class="sxs-lookup"><span data-stu-id="d2fab-106">This article shows what the payload for the HTTP POST to a webhook looks like.</span></span>

<span data-ttu-id="d2fab-107">A napló tevékenységriasztásokat további információkért lásd: hogyan [Azure tevékenység létrehozása napló riasztások](monitoring-activity-log-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="d2fab-107">For more information on activity log alerts, see how to [create Azure activity log alerts](monitoring-activity-log-alerts.md).</span></span>

<span data-ttu-id="d2fab-108">A művelet csoportok információkért lásd: hogyan [művelet csoportok létrehozása a](monitoring-action-groups.md).</span><span class="sxs-lookup"><span data-stu-id="d2fab-108">For information on action groups, see how to [create action groups](monitoring-action-groups.md).</span></span>

## <a name="authenticate-the-webhook"></a><span data-ttu-id="d2fab-109">A webhook hitelesítéséhez</span><span class="sxs-lookup"><span data-stu-id="d2fab-109">Authenticate the webhook</span></span>
<span data-ttu-id="d2fab-110">A webhook opcionálisan használhatja engedélyezési jogkivonat-alapú hitelesítésre.</span><span class="sxs-lookup"><span data-stu-id="d2fab-110">The webhook can optionally use token-based authorization for authentication.</span></span> <span data-ttu-id="d2fab-111">A webhook URI mentett jogkivonat-azonosítóval, például `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`.</span><span class="sxs-lookup"><span data-stu-id="d2fab-111">The webhook URI is saved with a token ID, for example, `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`.</span></span>

## <a name="payload-schema"></a><span data-ttu-id="d2fab-112">Hasznos séma</span><span class="sxs-lookup"><span data-stu-id="d2fab-112">Payload schema</span></span>
<span data-ttu-id="d2fab-113">A JSON-adattartalmat a FELADÁS egy vagy több művelet található helytől a tartalom data.context.activityLog.eventSource mező.</span><span class="sxs-lookup"><span data-stu-id="d2fab-113">The JSON payload contained in the POST operation differs based on the payload's data.context.activityLog.eventSource field.</span></span>

###<a name="common"></a><span data-ttu-id="d2fab-114">Közös</span><span class="sxs-lookup"><span data-stu-id="d2fab-114">Common</span></span>
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
###<a name="administrative"></a><span data-ttu-id="d2fab-115">Felügyeleti</span><span class="sxs-lookup"><span data-stu-id="d2fab-115">Administrative</span></span>
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
###<a name="servicehealth"></a><span data-ttu-id="d2fab-116">ServiceHealth</span><span class="sxs-lookup"><span data-stu-id="d2fab-116">ServiceHealth</span></span>
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

<span data-ttu-id="d2fab-117">A szolgáltatás állapotának értesítési tevékenység napló riasztások adott séma részletekért lásd: [szolgáltatás állapotával kapcsolatos értesítésekre](monitoring-service-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="d2fab-117">For specific schema details on service health notification activity log alerts, see [Service health notifications](monitoring-service-notifications.md).</span></span>

<span data-ttu-id="d2fab-118">Más tevékenység napló minden riasztásról adott séma részletekért lásd: [az Azure tevékenységnapló áttekintése](monitoring-overview-activity-logs.md).</span><span class="sxs-lookup"><span data-stu-id="d2fab-118">For specific schema details on all other activity log alerts, see [Overview of the Azure activity log](monitoring-overview-activity-logs.md).</span></span>

| <span data-ttu-id="d2fab-119">Elem neve</span><span class="sxs-lookup"><span data-stu-id="d2fab-119">Element name</span></span> | <span data-ttu-id="d2fab-120">Leírás</span><span class="sxs-lookup"><span data-stu-id="d2fab-120">Description</span></span> |
| --- | --- |
| <span data-ttu-id="d2fab-121">status</span><span class="sxs-lookup"><span data-stu-id="d2fab-121">status</span></span> |<span data-ttu-id="d2fab-122">A metrika riasztások használt.</span><span class="sxs-lookup"><span data-stu-id="d2fab-122">Used for metric alerts.</span></span> <span data-ttu-id="d2fab-123">Beállítása mindig "aktivált" tevékenység napló riasztások esetén.</span><span class="sxs-lookup"><span data-stu-id="d2fab-123">Always set to "activated" for activity log alerts.</span></span> |
| <span data-ttu-id="d2fab-124">A környezetben</span><span class="sxs-lookup"><span data-stu-id="d2fab-124">context</span></span> |<span data-ttu-id="d2fab-125">Az esemény környezetében.</span><span class="sxs-lookup"><span data-stu-id="d2fab-125">Context of the event.</span></span> |
| <span data-ttu-id="d2fab-126">resourceProviderName</span><span class="sxs-lookup"><span data-stu-id="d2fab-126">resourceProviderName</span></span> |<span data-ttu-id="d2fab-127">Az erőforrás-szolgáltató az érintett erőforrás.</span><span class="sxs-lookup"><span data-stu-id="d2fab-127">The resource provider of the impacted resource.</span></span> |
| <span data-ttu-id="d2fab-128">conditionType</span><span class="sxs-lookup"><span data-stu-id="d2fab-128">conditionType</span></span> |<span data-ttu-id="d2fab-129">Mindig "Event".</span><span class="sxs-lookup"><span data-stu-id="d2fab-129">Always "Event."</span></span> |
| <span data-ttu-id="d2fab-130">név</span><span class="sxs-lookup"><span data-stu-id="d2fab-130">name</span></span> |<span data-ttu-id="d2fab-131">A riasztási szabály neve.</span><span class="sxs-lookup"><span data-stu-id="d2fab-131">Name of the alert rule.</span></span> |
| <span data-ttu-id="d2fab-132">id</span><span class="sxs-lookup"><span data-stu-id="d2fab-132">id</span></span> |<span data-ttu-id="d2fab-133">A riasztás erőforrás-azonosító.</span><span class="sxs-lookup"><span data-stu-id="d2fab-133">Resource ID of the alert.</span></span> |
| <span data-ttu-id="d2fab-134">Leírás</span><span class="sxs-lookup"><span data-stu-id="d2fab-134">description</span></span> |<span data-ttu-id="d2fab-135">Riasztás leírása állítható be, ha a riasztást hoz létre.</span><span class="sxs-lookup"><span data-stu-id="d2fab-135">Alert description set when the alert is created.</span></span> |
| <span data-ttu-id="d2fab-136">subscriptionId</span><span class="sxs-lookup"><span data-stu-id="d2fab-136">subscriptionId</span></span> |<span data-ttu-id="d2fab-137">Az Azure előfizetés-azonosító.</span><span class="sxs-lookup"><span data-stu-id="d2fab-137">Azure subscription ID.</span></span> |
| <span data-ttu-id="d2fab-138">időbélyeg</span><span class="sxs-lookup"><span data-stu-id="d2fab-138">timestamp</span></span> |<span data-ttu-id="d2fab-139">Az idő, amelyen az esemény váltotta az Azure-szolgáltatás, amely a kérelem feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="d2fab-139">Time at which the event was generated by the Azure service that processed the request.</span></span> |
| <span data-ttu-id="d2fab-140">resourceId</span><span class="sxs-lookup"><span data-stu-id="d2fab-140">resourceId</span></span> |<span data-ttu-id="d2fab-141">Erőforrás-azonosító az érintett erőforrás.</span><span class="sxs-lookup"><span data-stu-id="d2fab-141">Resource ID of the impacted resource.</span></span> |
| <span data-ttu-id="d2fab-142">erőforráscsoport-név</span><span class="sxs-lookup"><span data-stu-id="d2fab-142">resourceGroupName</span></span> |<span data-ttu-id="d2fab-143">Az érintett erőforrás az erőforráscsoport neve.</span><span class="sxs-lookup"><span data-stu-id="d2fab-143">Name of the resource group for the impacted resource.</span></span> |
| <span data-ttu-id="d2fab-144">properties</span><span class="sxs-lookup"><span data-stu-id="d2fab-144">properties</span></span> |<span data-ttu-id="d2fab-145">Állítsa be a `<Key, Value>` párok (Ez azt jelenti, hogy `Dictionary<String, String>`), amely tartalmazza az esemény részleteit.</span><span class="sxs-lookup"><span data-stu-id="d2fab-145">Set of `<Key, Value>` pairs (that is, `Dictionary<String, String>`) that includes details about the event.</span></span> |
| <span data-ttu-id="d2fab-146">Esemény</span><span class="sxs-lookup"><span data-stu-id="d2fab-146">event</span></span> |<span data-ttu-id="d2fab-147">Az eseménnyel kapcsolatos metaadatokat tartalmazó elemet.</span><span class="sxs-lookup"><span data-stu-id="d2fab-147">Element that contains metadata about the event.</span></span> |
| <span data-ttu-id="d2fab-148">Engedélyezési</span><span class="sxs-lookup"><span data-stu-id="d2fab-148">authorization</span></span> |<span data-ttu-id="d2fab-149">A szerepköralapú hozzáférés-vezérlés az esemény tulajdonságai.</span><span class="sxs-lookup"><span data-stu-id="d2fab-149">The Role-Based Access Control properties of the event.</span></span> <span data-ttu-id="d2fab-150">Ezek a Tulajdonságok általában például a művelet, a szerepkör és a hatókör.</span><span class="sxs-lookup"><span data-stu-id="d2fab-150">These properties usually include the action, the role, and the scope.</span></span> |
| <span data-ttu-id="d2fab-151">category</span><span class="sxs-lookup"><span data-stu-id="d2fab-151">category</span></span> |<span data-ttu-id="d2fab-152">Az esemény kategóriáját.</span><span class="sxs-lookup"><span data-stu-id="d2fab-152">Category of the event.</span></span> <span data-ttu-id="d2fab-153">Támogatott értékek a következők: felügyeleti, figyelmeztetés, biztonsági, ServiceHealth és javaslat.</span><span class="sxs-lookup"><span data-stu-id="d2fab-153">Supported values include Administrative, Alert, Security, ServiceHealth, and Recommendation.</span></span> |
| <span data-ttu-id="d2fab-154">hívó</span><span class="sxs-lookup"><span data-stu-id="d2fab-154">caller</span></span> |<span data-ttu-id="d2fab-155">A művelet, a jogcím vagy a rendelkezésre állás alapján SPN jogcím végző felhasználó e-mail címe.</span><span class="sxs-lookup"><span data-stu-id="d2fab-155">Email address of the user who performed the operation, UPN claim, or SPN claim based on availability.</span></span> <span data-ttu-id="d2fab-156">Bizonyos rendszerhívások null értékű lehet.</span><span class="sxs-lookup"><span data-stu-id="d2fab-156">Can be null for certain system calls.</span></span> |
| <span data-ttu-id="d2fab-157">correlationId</span><span class="sxs-lookup"><span data-stu-id="d2fab-157">correlationId</span></span> |<span data-ttu-id="d2fab-158">Általában egy GUID karakterlánc-formátum.</span><span class="sxs-lookup"><span data-stu-id="d2fab-158">Usually a GUID in string format.</span></span> <span data-ttu-id="d2fab-159">A correlationId események ugyanaz a nagyobb művelet tartozik, és általában megosztani az egy correlationId.</span><span class="sxs-lookup"><span data-stu-id="d2fab-159">Events with correlationId belong to the same larger action and usually share a correlationId.</span></span> |
| <span data-ttu-id="d2fab-160">eventDescription</span><span class="sxs-lookup"><span data-stu-id="d2fab-160">eventDescription</span></span> |<span data-ttu-id="d2fab-161">Az esemény leírása statikus szöveg.</span><span class="sxs-lookup"><span data-stu-id="d2fab-161">Static text description of the event.</span></span> |
| <span data-ttu-id="d2fab-162">eventDataId</span><span class="sxs-lookup"><span data-stu-id="d2fab-162">eventDataId</span></span> |<span data-ttu-id="d2fab-163">Az esemény egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="d2fab-163">Unique identifier for the event.</span></span> |
| <span data-ttu-id="d2fab-164">Eseményforrás</span><span class="sxs-lookup"><span data-stu-id="d2fab-164">eventSource</span></span> |<span data-ttu-id="d2fab-165">Az Azure-szolgáltatás vagy az eseményt létrehozó infrastruktúra neve.</span><span class="sxs-lookup"><span data-stu-id="d2fab-165">Name of the Azure service or infrastructure that generated the event.</span></span> |
| <span data-ttu-id="d2fab-166">httpRequest</span><span class="sxs-lookup"><span data-stu-id="d2fab-166">httpRequest</span></span> |<span data-ttu-id="d2fab-167">A kérelem általában tartalmazza a clientRequestId clientIpAddress és HTTP-metódust (például PUT).</span><span class="sxs-lookup"><span data-stu-id="d2fab-167">The request usually includes the clientRequestId, clientIpAddress, and HTTP method (for example, PUT).</span></span> |
| <span data-ttu-id="d2fab-168">szint</span><span class="sxs-lookup"><span data-stu-id="d2fab-168">level</span></span> |<span data-ttu-id="d2fab-169">A következő értékek egyikét: kritikus, hiba, figyelmeztetés, tájékoztatás és részletes.</span><span class="sxs-lookup"><span data-stu-id="d2fab-169">One of the following values: Critical, Error, Warning, Informational, and Verbose.</span></span> |
| <span data-ttu-id="d2fab-170">OperationID azonosítójú</span><span class="sxs-lookup"><span data-stu-id="d2fab-170">operationId</span></span> |<span data-ttu-id="d2fab-171">Általában egy GUID azonosító az egyetlen műveletben megfelelő események között meg van osztva.</span><span class="sxs-lookup"><span data-stu-id="d2fab-171">Usually a GUID shared among the events corresponding to single operation.</span></span> |
| <span data-ttu-id="d2fab-172">operationName</span><span class="sxs-lookup"><span data-stu-id="d2fab-172">operationName</span></span> |<span data-ttu-id="d2fab-173">A művelet neve.</span><span class="sxs-lookup"><span data-stu-id="d2fab-173">Name of the operation.</span></span> |
| <span data-ttu-id="d2fab-174">properties</span><span class="sxs-lookup"><span data-stu-id="d2fab-174">properties</span></span> |<span data-ttu-id="d2fab-175">Az esemény tulajdonságai.</span><span class="sxs-lookup"><span data-stu-id="d2fab-175">Properties of the event.</span></span> |
| <span data-ttu-id="d2fab-176">status</span><span class="sxs-lookup"><span data-stu-id="d2fab-176">status</span></span> |<span data-ttu-id="d2fab-177">Karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="d2fab-177">String.</span></span> <span data-ttu-id="d2fab-178">A művelet állapotát.</span><span class="sxs-lookup"><span data-stu-id="d2fab-178">Status of the operation.</span></span> <span data-ttu-id="d2fab-179">A gyakori értékek a következők: elindítva, folyamatban lévő, sikeres, sikertelen, aktív és megoldva.</span><span class="sxs-lookup"><span data-stu-id="d2fab-179">Common values include Started, In Progress, Succeeded, Failed, Active, and Resolved.</span></span> |
| <span data-ttu-id="d2fab-180">a részállapot</span><span class="sxs-lookup"><span data-stu-id="d2fab-180">subStatus</span></span> |<span data-ttu-id="d2fab-181">Általában tartalmazza a megfelelő REST-hívást a HTTP-állapotkódot.</span><span class="sxs-lookup"><span data-stu-id="d2fab-181">Usually includes the HTTP status code of the corresponding REST call.</span></span> <span data-ttu-id="d2fab-182">A részállapot más karakterláncokat is tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="d2fab-182">It might also include other strings that describe a substatus.</span></span> <span data-ttu-id="d2fab-183">Közös részállapot érték OK (HTTP-állapotkód:: 200), létrehozott (HTTP-állapotkód:: 201-es), fogadja el (HTTP-állapotkód:: 202), a tartalom nem (HTTP-állapotkód: 204), hibás kérés (HTTP-állapotkód:: 400), nem található (HTTP-állapotkód: 404-es), ütközés (HTTP-állapotkód:: 409 ), Belső kiszolgálóhiba (HTTP-állapotkód: 500), a Service nem érhető el (HTTP-állapotkód: 503-as), és az átjáró időtúllépése (HTTP-állapotkód: 504).</span><span class="sxs-lookup"><span data-stu-id="d2fab-183">Common substatus values include OK (HTTP Status Code: 200), Created (HTTP Status Code: 201), Accepted (HTTP Status Code: 202), No Content (HTTP Status Code: 204), Bad Request (HTTP Status Code: 400), Not Found (HTTP Status Code: 404), Conflict (HTTP Status Code: 409), Internal Server Error (HTTP Status Code: 500), Service Unavailable (HTTP Status Code: 503), and Gateway Timeout (HTTP Status Code: 504).</span></span> |

## <a name="next-steps"></a><span data-ttu-id="d2fab-184">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d2fab-184">Next steps</span></span>
* <span data-ttu-id="d2fab-185">[További információ a tevékenységnapló](monitoring-overview-activity-logs.md).</span><span class="sxs-lookup"><span data-stu-id="d2fab-185">[Learn more about the activity log](monitoring-overview-activity-logs.md).</span></span>
* <span data-ttu-id="d2fab-186">[Hajtsa végre az Azure automation-parancsfájlok (Runbookok) Azure riasztások](http://go.microsoft.com/fwlink/?LinkId=627081).</span><span class="sxs-lookup"><span data-stu-id="d2fab-186">[Execute Azure automation scripts (Runbooks) on Azure alerts](http://go.microsoft.com/fwlink/?LinkId=627081).</span></span>
* <span data-ttu-id="d2fab-187">[A logikai alkalmazás segítségével Twilio keresztül SMS küldése az Azure-riasztás alapján](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app).</span><span class="sxs-lookup"><span data-stu-id="d2fab-187">[Use a logic app to send an SMS via Twilio from an Azure alert](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app).</span></span> <span data-ttu-id="d2fab-188">Ebben a példában a metrika riasztások, de módosítható egy figyelmeztetés a napló együttműködni.</span><span class="sxs-lookup"><span data-stu-id="d2fab-188">This example is for metric alerts, but it can be modified to work with an activity log alert.</span></span>
* <span data-ttu-id="d2fab-189">[Használja a logikai alkalmazás közzététele a Slack üzenet küldése az Azure-riasztás alapján](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app).</span><span class="sxs-lookup"><span data-stu-id="d2fab-189">[Use a logic app to send a Slack message from an Azure alert](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app).</span></span> <span data-ttu-id="d2fab-190">Ebben a példában a metrika riasztások, de módosítható egy figyelmeztetés a napló együttműködni.</span><span class="sxs-lookup"><span data-stu-id="d2fab-190">This example is for metric alerts, but it can be modified to work with an activity log alert.</span></span>
* <span data-ttu-id="d2fab-191">[A logikai alkalmazás használja az Azure-riasztás alapján egy Azure-üzenetsorba való üzenetküldéshez](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app).</span><span class="sxs-lookup"><span data-stu-id="d2fab-191">[Use a logic app to send a message to an Azure queue from an Azure alert](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app).</span></span> <span data-ttu-id="d2fab-192">Ebben a példában a metrika riasztások, de módosítható egy figyelmeztetés a napló együttműködni.</span><span class="sxs-lookup"><span data-stu-id="d2fab-192">This example is for metric alerts, but it can be modified to work with an activity log alert.</span></span>
