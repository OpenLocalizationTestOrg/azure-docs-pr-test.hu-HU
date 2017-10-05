---
title: "A webhook hívható meg Azure tevékenységnapló riasztások |} Microsoft Docs"
description: "Útvonal tevékenység alkalmazásnapló-események az egyéb szolgáltatásokkal egyéni műveletek. Például SMS küldése, jelentkezzen hibák, vagy egy csoport Csevegés/üzenetküldési szolgáltatáson keresztül értesíti."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 64d333d1-7f37-4a00-9d16-dda6e69a113b
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: johnkem
ms.openlocfilehash: 341ab32ad0ec691285fbf1537ee298ab30156a5d
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="call-a-webhook-on-azure-activity-log-alerts"></a><span data-ttu-id="1f0a3-104">A webhook hívható meg Azure tevékenységnapló riasztások</span><span class="sxs-lookup"><span data-stu-id="1f0a3-104">Call a webhook on Azure Activity Log alerts</span></span>
<span data-ttu-id="1f0a3-105">Webhook lehetővé teszik a más rendszerekkel utófeldolgozási vagy egyéni műveletek az Azure riasztási értesítések továbbításához.</span><span class="sxs-lookup"><span data-stu-id="1f0a3-105">Webhooks allow you to route an Azure alert notification to other systems for post-processing or custom actions.</span></span> <span data-ttu-id="1f0a3-106">A riasztás olyan webhook segítségével szolgáltatásokról, amelyek SMS küldése, hibák naplózása, értesítés keresztül Csevegés/üzenetküldési szolgáltatásokban csoport vagy más műveletek többféle irányítja.</span><span class="sxs-lookup"><span data-stu-id="1f0a3-106">You can use a webhook on an alert to route it to services that send SMS, log bugs, notify a team via chat/messaging services, or do any number of other actions.</span></span> <span data-ttu-id="1f0a3-107">A cikkből megtudhatja, hogyan kell beállítani a webhook kell meghívni, amikor egy Azure tevékenységnapló riasztási következik be.</span><span class="sxs-lookup"><span data-stu-id="1f0a3-107">This article describes how to set a webhook to be called when an Azure Activity Log alert fires.</span></span> <span data-ttu-id="1f0a3-108">Ezenfelül itt látható a hasznos a történő olyan webhook HTTP-KÖZZÉTÉTELLEL néz.</span><span class="sxs-lookup"><span data-stu-id="1f0a3-108">It also shows what the payload for the HTTP POST to a webhook looks like.</span></span> <span data-ttu-id="1f0a3-109">A telepítő és a séma Azure metrika riasztás [helyette látja ezt a lapot](insights-webhooks-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="1f0a3-109">For information on the setup and schema for an Azure metric alert, [see this page instead](insights-webhooks-alerts.md).</span></span> <span data-ttu-id="1f0a3-110">Is állíthatja be tevékenységnapló riasztást aktiválásakor e-mail üzenetek küldéséhez.</span><span class="sxs-lookup"><span data-stu-id="1f0a3-110">You can also set up an Activity Log alert to send email when activated.</span></span>

> [!NOTE]
> <span data-ttu-id="1f0a3-111">Ez a funkció jelenleg előzetes verzióban érhetők, és a jövőben bármikor törlődik.</span><span class="sxs-lookup"><span data-stu-id="1f0a3-111">This feature is currently in preview and will be removed at some point in the future.</span></span>
>
>

<span data-ttu-id="1f0a3-112">Egy tevékenység naplója riasztási használatával állíthat be a [Azure PowerShell-parancsmagok](insights-powershell-samples.md#create-metric-alerts), [platformfüggetlen parancssori felület](insights-cli-samples.md#work-with-alerts), vagy [Azure figyelő REST API](https://msdn.microsoft.com/library/azure/dn933805.aspx).</span><span class="sxs-lookup"><span data-stu-id="1f0a3-112">You can set up an Activity Log alert using the [Azure PowerShell Cmdlets](insights-powershell-samples.md#create-metric-alerts), [Cross-Platform CLI](insights-cli-samples.md#work-with-alerts), or [Azure Monitor REST API](https://msdn.microsoft.com/library/azure/dn933805.aspx).</span></span> <span data-ttu-id="1f0a3-113">Jelenleg nem állítható be az Azure portál segítségével egy.</span><span class="sxs-lookup"><span data-stu-id="1f0a3-113">Currently, you cannot set one up using the Azure portal.</span></span>

## <a name="authenticating-the-webhook"></a><span data-ttu-id="1f0a3-114">A webhook hitelesítése</span><span class="sxs-lookup"><span data-stu-id="1f0a3-114">Authenticating the webhook</span></span>
<span data-ttu-id="1f0a3-115">A webhook képes hitelesíteni az alábbi módszerek valamelyikével:</span><span class="sxs-lookup"><span data-stu-id="1f0a3-115">The webhook can authenticate using either of these methods:</span></span>

1. <span data-ttu-id="1f0a3-116">**Jogkivonat-alapú engedélyezési** -URI mentett jogkivonat-azonosítóval, például a webhook`https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`</span><span class="sxs-lookup"><span data-stu-id="1f0a3-116">**Token-based authorization** - The webhook URI is saved with a token ID, for example, `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`</span></span>
2. <span data-ttu-id="1f0a3-117">**Alapszintű engedélyezési** -URI mentik felhasználónévvel és jelszóval, például a webhook`https://userid:password@mysamplealert/webcallback?someparamater=somevalue&foo=bar`</span><span class="sxs-lookup"><span data-stu-id="1f0a3-117">**Basic authorization** - The webhook URI is saved with a username and password, for example, `https://userid:password@mysamplealert/webcallback?someparamater=somevalue&foo=bar`</span></span>

## <a name="payload-schema"></a><span data-ttu-id="1f0a3-118">Hasznos séma</span><span class="sxs-lookup"><span data-stu-id="1f0a3-118">Payload schema</span></span>
<span data-ttu-id="1f0a3-119">A POST műveletet tartalmaz, a következő JSON-adattartalmat és az összes tevékenységnapló-alapú értesítések séma.</span><span class="sxs-lookup"><span data-stu-id="1f0a3-119">The POST operation contains the following JSON payload and schema for all Activity Log-based alerts.</span></span> <span data-ttu-id="1f0a3-120">Ebben a sémában hasonlít a metrika-alapú értesítések használják.</span><span class="sxs-lookup"><span data-stu-id="1f0a3-120">This schema is similar to the one used by metric-based alerts.</span></span>

```
{
        "status": "Activated",
        "context": {
                "resourceProviderName": "Microsoft.Web",
                "event": {
                        "$type": "Microsoft.WindowsAzure.Management.Monitoring.Automation.Notifications.GenericNotifications.Datacontracts.InstanceEventContext, Microsoft.WindowsAzure.Management.Mon.Automation",
                        "authorization": {
                                "action": "Microsoft.Web/sites/start/action",
                                "scope": "/subscriptions/s1/resourcegroups/rg1/providers/Microsoft.Web/sites/leoalerttest"
                        },
                        "eventDataId": "327caaca-08d7-41b1-86d8-27d0a7adb92d",
                        "category": "Administrative",
                        "caller": "myname@mycompany.com",
                        "httpRequest": {
                                "clientRequestId": "f58cead8-c9ed-43af-8710-55e64def208d",
                                "clientIpAddress": "104.43.166.155",
                                "method": "POST"
                        },
                        "status": "Succeeded",
                        "subStatus": "OK",
                        "level": "Informational",
                        "correlationId": "4a40beaa-6a63-4d92-85c4-923a25abb590",
                        "eventDescription": "",
                        "operationName": "Microsoft.Web/sites/start/action",
                        "operationId": "4a40beaa-6a63-4d92-85c4-923a25abb590",
                        "properties": {
                                "$type": "Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage",
                                "statusCode": "OK",
                                "serviceRequestId": "f7716681-496a-4f5c-8d14-d564bcf54714"
                        }
                },
                "timestamp": "Friday, March 11, 2016 9:13:23 PM",
                "id": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/alertrules/alertonevent2",
                "name": "alertonevent2",
                "description": "test alert on event start",
                "conditionType": "Event",
                "subscriptionId": "s1",
                "resourceId": "/subscriptions/s1/resourcegroups/rg1/providers/Microsoft.Web/sites/leoalerttest",
                "resourceGroupName": "rg1"
        },
        "properties": {
                "key1": "value1",
                "key2": "value2"
        }
}
```

| <span data-ttu-id="1f0a3-121">Elem neve</span><span class="sxs-lookup"><span data-stu-id="1f0a3-121">Element Name</span></span> | <span data-ttu-id="1f0a3-122">Leírás</span><span class="sxs-lookup"><span data-stu-id="1f0a3-122">Description</span></span> |
| --- | --- |
| <span data-ttu-id="1f0a3-123">status</span><span class="sxs-lookup"><span data-stu-id="1f0a3-123">status</span></span> |<span data-ttu-id="1f0a3-124">A metrika riasztások használt.</span><span class="sxs-lookup"><span data-stu-id="1f0a3-124">Used for metric alerts.</span></span> <span data-ttu-id="1f0a3-125">Mindig megadni "aktív" tevékenységnapló riasztások esetén.</span><span class="sxs-lookup"><span data-stu-id="1f0a3-125">Always set to "activated" for Activity Log alerts.</span></span> |
| <span data-ttu-id="1f0a3-126">A környezetben</span><span class="sxs-lookup"><span data-stu-id="1f0a3-126">context</span></span> |<span data-ttu-id="1f0a3-127">Az esemény környezetében.</span><span class="sxs-lookup"><span data-stu-id="1f0a3-127">Context of the event.</span></span> |
| <span data-ttu-id="1f0a3-128">resourceProviderName</span><span class="sxs-lookup"><span data-stu-id="1f0a3-128">resourceProviderName</span></span> |<span data-ttu-id="1f0a3-129">Az erőforrás-szolgáltató az érintett erőforrás.</span><span class="sxs-lookup"><span data-stu-id="1f0a3-129">The resource provider of the impacted resource.</span></span> |
| <span data-ttu-id="1f0a3-130">conditionType</span><span class="sxs-lookup"><span data-stu-id="1f0a3-130">conditionType</span></span> |<span data-ttu-id="1f0a3-131">Mindig "Event".</span><span class="sxs-lookup"><span data-stu-id="1f0a3-131">Always "Event."</span></span> |
| <span data-ttu-id="1f0a3-132">név</span><span class="sxs-lookup"><span data-stu-id="1f0a3-132">name</span></span> |<span data-ttu-id="1f0a3-133">A riasztási szabály neve.</span><span class="sxs-lookup"><span data-stu-id="1f0a3-133">Name of the alert rule.</span></span> |
| <span data-ttu-id="1f0a3-134">id</span><span class="sxs-lookup"><span data-stu-id="1f0a3-134">id</span></span> |<span data-ttu-id="1f0a3-135">A riasztás erőforrás-azonosító.</span><span class="sxs-lookup"><span data-stu-id="1f0a3-135">Resource ID of the alert.</span></span> |
| <span data-ttu-id="1f0a3-136">Leírás</span><span class="sxs-lookup"><span data-stu-id="1f0a3-136">description</span></span> |<span data-ttu-id="1f0a3-137">A riasztás létrehozása során beállított riasztás leírása</span><span class="sxs-lookup"><span data-stu-id="1f0a3-137">Alert description as set during creation of the alert.</span></span> |
| <span data-ttu-id="1f0a3-138">subscriptionId</span><span class="sxs-lookup"><span data-stu-id="1f0a3-138">subscriptionId</span></span> |<span data-ttu-id="1f0a3-139">Az Azure előfizetés-azonosító.</span><span class="sxs-lookup"><span data-stu-id="1f0a3-139">Azure Subscription ID.</span></span> |
| <span data-ttu-id="1f0a3-140">időbélyeg</span><span class="sxs-lookup"><span data-stu-id="1f0a3-140">timestamp</span></span> |<span data-ttu-id="1f0a3-141">Az idő, amelyen az esemény váltotta az Azure-szolgáltatás, amely a kérelem feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="1f0a3-141">Time at which the event was generated by the Azure service that processed the request.</span></span> |
| <span data-ttu-id="1f0a3-142">resourceId</span><span class="sxs-lookup"><span data-stu-id="1f0a3-142">resourceId</span></span> |<span data-ttu-id="1f0a3-143">Erőforrás-azonosító az érintett erőforrás.</span><span class="sxs-lookup"><span data-stu-id="1f0a3-143">Resource ID of the impacted resource.</span></span> |
| <span data-ttu-id="1f0a3-144">erőforráscsoport-név</span><span class="sxs-lookup"><span data-stu-id="1f0a3-144">resourceGroupName</span></span> |<span data-ttu-id="1f0a3-145">Az érintett erőforrás az erőforráscsoport neve</span><span class="sxs-lookup"><span data-stu-id="1f0a3-145">Name of the resource group for the impacted resource</span></span> |
| <span data-ttu-id="1f0a3-146">properties</span><span class="sxs-lookup"><span data-stu-id="1f0a3-146">properties</span></span> |<span data-ttu-id="1f0a3-147">Állítsa be a `<Key, Value>` párok (azaz `Dictionary<String, String>`), amely tartalmazza az esemény részleteit.</span><span class="sxs-lookup"><span data-stu-id="1f0a3-147">Set of `<Key, Value>` pairs (i.e. `Dictionary<String, String>`) that includes details about the event.</span></span> |
| <span data-ttu-id="1f0a3-148">Esemény</span><span class="sxs-lookup"><span data-stu-id="1f0a3-148">event</span></span> |<span data-ttu-id="1f0a3-149">Az eseménnyel kapcsolatos metaadatokat tartalmazó elemnek.</span><span class="sxs-lookup"><span data-stu-id="1f0a3-149">Element containing metadata about the event.</span></span> |
| <span data-ttu-id="1f0a3-150">Engedélyezési</span><span class="sxs-lookup"><span data-stu-id="1f0a3-150">authorization</span></span> |<span data-ttu-id="1f0a3-151">Az esemény RBAC tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="1f0a3-151">The RBAC properties of the event.</span></span> <span data-ttu-id="1f0a3-152">Általában ezek közé tartozik, az "action", "szerepkör" és a "hatókörben."</span><span class="sxs-lookup"><span data-stu-id="1f0a3-152">These usually include the “action”, “role” and the “scope.”</span></span> |
| <span data-ttu-id="1f0a3-153">category</span><span class="sxs-lookup"><span data-stu-id="1f0a3-153">category</span></span> |<span data-ttu-id="1f0a3-154">Az esemény kategóriáját.</span><span class="sxs-lookup"><span data-stu-id="1f0a3-154">Category of the event.</span></span> <span data-ttu-id="1f0a3-155">Támogatott értékek a következők: felügyeleti, a riasztásra, biztonság, ServiceHealth, javaslat.</span><span class="sxs-lookup"><span data-stu-id="1f0a3-155">Supported values include: Administrative, Alert, Security, ServiceHealth, Recommendation.</span></span> |
| <span data-ttu-id="1f0a3-156">hívó</span><span class="sxs-lookup"><span data-stu-id="1f0a3-156">caller</span></span> |<span data-ttu-id="1f0a3-157">A művelet, a jogcím vagy a rendelkezésre állás alapján SPN jogcím végző felhasználó e-mail címe.</span><span class="sxs-lookup"><span data-stu-id="1f0a3-157">Email address of the user who performed the operation, UPN claim, or SPN claim based on availability.</span></span> <span data-ttu-id="1f0a3-158">Bizonyos rendszerhívások null értékű lehet.</span><span class="sxs-lookup"><span data-stu-id="1f0a3-158">Can be null for certain system calls.</span></span> |
| <span data-ttu-id="1f0a3-159">correlationId</span><span class="sxs-lookup"><span data-stu-id="1f0a3-159">correlationId</span></span> |<span data-ttu-id="1f0a3-160">Általában egy GUID karakterlánc-formátum.</span><span class="sxs-lookup"><span data-stu-id="1f0a3-160">Usually a GUID in string format.</span></span> <span data-ttu-id="1f0a3-161">A correlationId események ugyanaz a nagyobb művelet tartozik, és általában megosztani az egy correlationId.</span><span class="sxs-lookup"><span data-stu-id="1f0a3-161">Events with correlationId belong to the same larger action and usually share a correlationId.</span></span> |
| <span data-ttu-id="1f0a3-162">eventDescription</span><span class="sxs-lookup"><span data-stu-id="1f0a3-162">eventDescription</span></span> |<span data-ttu-id="1f0a3-163">Az esemény leírása statikus szöveg.</span><span class="sxs-lookup"><span data-stu-id="1f0a3-163">Static text description of the event.</span></span> |
| <span data-ttu-id="1f0a3-164">eventDataId</span><span class="sxs-lookup"><span data-stu-id="1f0a3-164">eventDataId</span></span> |<span data-ttu-id="1f0a3-165">Az esemény egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="1f0a3-165">Unique identifier for the event.</span></span> |
| <span data-ttu-id="1f0a3-166">Eseményforrás</span><span class="sxs-lookup"><span data-stu-id="1f0a3-166">eventSource</span></span> |<span data-ttu-id="1f0a3-167">Az Azure-szolgáltatás vagy az eseményt létrehozó infrastruktúra neve.</span><span class="sxs-lookup"><span data-stu-id="1f0a3-167">Name of the Azure service or infrastructure that generated the event.</span></span> |
| <span data-ttu-id="1f0a3-168">httpRequest</span><span class="sxs-lookup"><span data-stu-id="1f0a3-168">httpRequest</span></span> |<span data-ttu-id="1f0a3-169">Általában a "clientRequestId", "clientIpAddress" és "method" tartalmazza (pl. PUT HTTP-metódus).</span><span class="sxs-lookup"><span data-stu-id="1f0a3-169">Usually includes the “clientRequestId”, “clientIpAddress” and “method” (HTTP method e.g. PUT).</span></span> |
| <span data-ttu-id="1f0a3-170">szint</span><span class="sxs-lookup"><span data-stu-id="1f0a3-170">level</span></span> |<span data-ttu-id="1f0a3-171">A következő értékek egyikét: "Kritikus", "Error", "Figyelmeztetés", "Tájékoztató" és "Részletes."</span><span class="sxs-lookup"><span data-stu-id="1f0a3-171">One of the following values: “Critical”, “Error”, “Warning”, “Informational” and “Verbose.”</span></span> |
| <span data-ttu-id="1f0a3-172">OperationID azonosítójú</span><span class="sxs-lookup"><span data-stu-id="1f0a3-172">operationId</span></span> |<span data-ttu-id="1f0a3-173">Általában egy GUID azonosító az egyetlen műveletben megfelelő események között meg van osztva.</span><span class="sxs-lookup"><span data-stu-id="1f0a3-173">Usually a GUID shared among the events corresponding to single operation.</span></span> |
| <span data-ttu-id="1f0a3-174">operationName</span><span class="sxs-lookup"><span data-stu-id="1f0a3-174">operationName</span></span> |<span data-ttu-id="1f0a3-175">A művelet neve.</span><span class="sxs-lookup"><span data-stu-id="1f0a3-175">Name of the operation.</span></span> |
| <span data-ttu-id="1f0a3-176">properties</span><span class="sxs-lookup"><span data-stu-id="1f0a3-176">properties</span></span> |<span data-ttu-id="1f0a3-177">Az esemény tulajdonságai.</span><span class="sxs-lookup"><span data-stu-id="1f0a3-177">Properties of the event.</span></span> |
| <span data-ttu-id="1f0a3-178">status</span><span class="sxs-lookup"><span data-stu-id="1f0a3-178">status</span></span> |<span data-ttu-id="1f0a3-179">Karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="1f0a3-179">String.</span></span> <span data-ttu-id="1f0a3-180">A művelet állapotát.</span><span class="sxs-lookup"><span data-stu-id="1f0a3-180">Status of the operation.</span></span> <span data-ttu-id="1f0a3-181">Gyakori értékek a következők: "Started", "Folyamatban", "Sikeres", "Sikertelen", "Active", "Megoldott".</span><span class="sxs-lookup"><span data-stu-id="1f0a3-181">Common values include: "Started", "In Progress", "Succeeded", "Failed", "Active", "Resolved".</span></span> |
| <span data-ttu-id="1f0a3-182">a részállapot</span><span class="sxs-lookup"><span data-stu-id="1f0a3-182">subStatus</span></span> |<span data-ttu-id="1f0a3-183">Általában tartalmazza a megfelelő REST-hívást a HTTP-állapotkódot.</span><span class="sxs-lookup"><span data-stu-id="1f0a3-183">Usually includes the HTTP status code of the corresponding REST call.</span></span> <span data-ttu-id="1f0a3-184">A részállapot leíró más karakterláncok is tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="1f0a3-184">It might also include other strings describing a substatus.</span></span> <span data-ttu-id="1f0a3-185">Közös részállapot értékek a következők: OK (HTTP-állapotkód:: 200), létrehozott (HTTP-állapotkód:: 201-es), fogadja el (HTTP-állapotkód:: 202), nem a tartalom (HTTP-állapotkód: 204), hibás kérés (HTTP-állapotkód:: 400), nem található (HTTP-állapotkód:: 404-es), ütközés (HTTP-állapotkód:: 409), belső kiszolgálóhiba (HTTP-állapotkód:: 500), szolgáltatás nem érhető el (HTTP-állapotkód:: 503-as), átjáró időtúllépése (HTTP-állapotkód: 504)</span><span class="sxs-lookup"><span data-stu-id="1f0a3-185">Common substatus values include: OK (HTTP Status Code: 200), Created (HTTP Status Code: 201), Accepted (HTTP Status Code: 202), No Content (HTTP Status Code: 204), Bad Request (HTTP Status Code: 400), Not Found (HTTP Status Code: 404), Conflict (HTTP Status Code: 409), Internal Server Error (HTTP Status Code: 500), Service Unavailable (HTTP Status Code: 503), Gateway Timeout (HTTP Status Code: 504)</span></span> |

## <a name="next-steps"></a><span data-ttu-id="1f0a3-186">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1f0a3-186">Next steps</span></span>
* [<span data-ttu-id="1f0a3-187">További információ a műveletnapló</span><span class="sxs-lookup"><span data-stu-id="1f0a3-187">Learn more about the Activity Log</span></span>](monitoring-overview-activity-logs.md)
* [<span data-ttu-id="1f0a3-188">Azure Automation-parancsfájlok (Runbookok) végrehajtása Azure riasztások</span><span class="sxs-lookup"><span data-stu-id="1f0a3-188">Execute Azure Automation scripts (Runbooks) on Azure alerts</span></span>](http://go.microsoft.com/fwlink/?LinkId=627081)
* <span data-ttu-id="1f0a3-189">[Logikai alkalmazás segítségével Twilio keresztül SMS küldése az Azure-riasztás alapján](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app).</span><span class="sxs-lookup"><span data-stu-id="1f0a3-189">[Use Logic App to send an SMS via Twilio from an Azure alert](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app).</span></span> <span data-ttu-id="1f0a3-190">Ebben a példában a metrika riasztások, de használható tevékenységnapló riasztást sikerült módosítani.</span><span class="sxs-lookup"><span data-stu-id="1f0a3-190">This example is for metric alerts, but could be modified to work with an Activity Log alert.</span></span>
* <span data-ttu-id="1f0a3-191">[Használja a logikai alkalmazás közzététele a Slack üzenet küldése az Azure-riasztás alapján](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app).</span><span class="sxs-lookup"><span data-stu-id="1f0a3-191">[Use Logic App to send a Slack message from an Azure alert](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app).</span></span> <span data-ttu-id="1f0a3-192">Ebben a példában a metrika riasztások, de használható tevékenységnapló riasztást sikerült módosítani.</span><span class="sxs-lookup"><span data-stu-id="1f0a3-192">This example is for metric alerts, but could be modified to work with an Activity Log alert.</span></span>
* <span data-ttu-id="1f0a3-193">[Üzenet küldése az Azure Queue az Azure-riasztás alapján logikai alkalmazás segítségével](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app).</span><span class="sxs-lookup"><span data-stu-id="1f0a3-193">[Use Logic App to send a message to an Azure Queue from an Azure alert](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app).</span></span> <span data-ttu-id="1f0a3-194">Ebben a példában a metrika riasztások, de használható tevékenységnapló riasztást sikerült módosítani.</span><span class="sxs-lookup"><span data-stu-id="1f0a3-194">This example is for metric alerts, but could be modified to work with an Activity Log alert.</span></span>
