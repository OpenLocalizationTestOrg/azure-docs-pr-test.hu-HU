---
title: "a webhook Azure tevékenységnapló riasztások aaaCall |} Microsoft Docs"
description: "Útvonal tevékenység naplózási események tooother-szolgáltatások egyéni műveletek. Például SMS küldése, jelentkezzen hibák, vagy egy csoport Csevegés/üzenetküldési szolgáltatáson keresztül értesíti."
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
ms.openlocfilehash: 9017ff3e5165857ec7084a8f07f4123552e55f73
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="call-a-webhook-on-azure-activity-log-alerts"></a><span data-ttu-id="d3e9e-104">A webhook hívható meg Azure tevékenységnapló riasztások</span><span class="sxs-lookup"><span data-stu-id="d3e9e-104">Call a webhook on Azure Activity Log alerts</span></span>
<span data-ttu-id="d3e9e-105">Webhook lehetővé teszik az Azure tooroute riasztási értesítés tooother rendszerek utófeldolgozási vagy egyéni műveletek.</span><span class="sxs-lookup"><span data-stu-id="d3e9e-105">Webhooks allow you tooroute an Azure alert notification tooother systems for post-processing or custom actions.</span></span> <span data-ttu-id="d3e9e-106">A webhook egy riasztási tooroute használhatja azt, az SMS küldő tooservices hibák jelentkezzen, egy csapat Csevegés/üzenetküldési szolgáltatásokban keresztül értesíti, vagy más műveletek többféle.</span><span class="sxs-lookup"><span data-stu-id="d3e9e-106">You can use a webhook on an alert tooroute it tooservices that send SMS, log bugs, notify a team via chat/messaging services, or do any number of other actions.</span></span> <span data-ttu-id="d3e9e-107">Ez a cikk ismerteti, hogyan tooset egy webhook toobe meghívva, amikor egy Azure tevékenységnapló riasztási következik be.</span><span class="sxs-lookup"><span data-stu-id="d3e9e-107">This article describes how tooset a webhook toobe called when an Azure Activity Log alert fires.</span></span> <span data-ttu-id="d3e9e-108">Azt is bemutatja, milyen hello hasznos hello HTTP POST tooa webhook tűnik.</span><span class="sxs-lookup"><span data-stu-id="d3e9e-108">It also shows what hello payload for hello HTTP POST tooa webhook looks like.</span></span> <span data-ttu-id="d3e9e-109">Hello beállítása és a séma Azure metrika riasztás [helyette látja ezt a lapot](insights-webhooks-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="d3e9e-109">For information on hello setup and schema for an Azure metric alert, [see this page instead](insights-webhooks-alerts.md).</span></span> <span data-ttu-id="d3e9e-110">Tevékenységnapló riasztási toosend e-mailt aktiválásakor is állíthat be.</span><span class="sxs-lookup"><span data-stu-id="d3e9e-110">You can also set up an Activity Log alert toosend email when activated.</span></span>

> [!NOTE]
> <span data-ttu-id="d3e9e-111">Ez a funkció jelenleg előzetes verzióban érhetők, és a jövőbeli hello bármikor törlődni fog.</span><span class="sxs-lookup"><span data-stu-id="d3e9e-111">This feature is currently in preview and will be removed at some point in hello future.</span></span>
>
>

<span data-ttu-id="d3e9e-112">Hello segítségével tevékenységnapló riasztást állíthat be [Azure PowerShell-parancsmagok](insights-powershell-samples.md#create-metric-alerts), [platformfüggetlen parancssori felület](insights-cli-samples.md#work-with-alerts), vagy [Azure figyelő REST API](https://msdn.microsoft.com/library/azure/dn933805.aspx).</span><span class="sxs-lookup"><span data-stu-id="d3e9e-112">You can set up an Activity Log alert using hello [Azure PowerShell Cmdlets](insights-powershell-samples.md#create-metric-alerts), [Cross-Platform CLI](insights-cli-samples.md#work-with-alerts), or [Azure Monitor REST API](https://msdn.microsoft.com/library/azure/dn933805.aspx).</span></span> <span data-ttu-id="d3e9e-113">Jelenleg nem állítható be Azure-portálon hello segítségével egy.</span><span class="sxs-lookup"><span data-stu-id="d3e9e-113">Currently, you cannot set one up using hello Azure portal.</span></span>

## <a name="authenticating-hello-webhook"></a><span data-ttu-id="d3e9e-114">Hello webhook hitelesítése</span><span class="sxs-lookup"><span data-stu-id="d3e9e-114">Authenticating hello webhook</span></span>
<span data-ttu-id="d3e9e-115">hello webhook képes hitelesíteni az alábbi módszerek valamelyikével:</span><span class="sxs-lookup"><span data-stu-id="d3e9e-115">hello webhook can authenticate using either of these methods:</span></span>

1. <span data-ttu-id="d3e9e-116">**Jogkivonat-alapú engedélyezési** -webhook URI mentett jogkivonat-azonosítóval, például hello`https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`</span><span class="sxs-lookup"><span data-stu-id="d3e9e-116">**Token-based authorization** - hello webhook URI is saved with a token ID, for example, `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`</span></span>
2. <span data-ttu-id="d3e9e-117">**Alapszintű engedélyezési** -webhook URI mentik felhasználónévvel és jelszóval, például hello`https://userid:password@mysamplealert/webcallback?someparamater=somevalue&foo=bar`</span><span class="sxs-lookup"><span data-stu-id="d3e9e-117">**Basic authorization** - hello webhook URI is saved with a username and password, for example, `https://userid:password@mysamplealert/webcallback?someparamater=somevalue&foo=bar`</span></span>

## <a name="payload-schema"></a><span data-ttu-id="d3e9e-118">Hasznos séma</span><span class="sxs-lookup"><span data-stu-id="d3e9e-118">Payload schema</span></span>
<span data-ttu-id="d3e9e-119">POST művelet hello JSON-adattartalmat és az összes tevékenységnapló alapú értesítések séma a következő hello tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="d3e9e-119">hello POST operation contains hello following JSON payload and schema for all Activity Log-based alerts.</span></span> <span data-ttu-id="d3e9e-120">Ebben a sémában metrika-alapú értesítések által használt egyik hasonló toohello.</span><span class="sxs-lookup"><span data-stu-id="d3e9e-120">This schema is similar toohello one used by metric-based alerts.</span></span>

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

| <span data-ttu-id="d3e9e-121">Elem neve</span><span class="sxs-lookup"><span data-stu-id="d3e9e-121">Element Name</span></span> | <span data-ttu-id="d3e9e-122">Leírás</span><span class="sxs-lookup"><span data-stu-id="d3e9e-122">Description</span></span> |
| --- | --- |
| <span data-ttu-id="d3e9e-123">status</span><span class="sxs-lookup"><span data-stu-id="d3e9e-123">status</span></span> |<span data-ttu-id="d3e9e-124">A metrika riasztások használt.</span><span class="sxs-lookup"><span data-stu-id="d3e9e-124">Used for metric alerts.</span></span> <span data-ttu-id="d3e9e-125">Mindig beállította túl "aktivált" tevékenységnapló riasztások esetén.</span><span class="sxs-lookup"><span data-stu-id="d3e9e-125">Always set too"activated" for Activity Log alerts.</span></span> |
| <span data-ttu-id="d3e9e-126">A környezetben</span><span class="sxs-lookup"><span data-stu-id="d3e9e-126">context</span></span> |<span data-ttu-id="d3e9e-127">Hello esemény környezetében.</span><span class="sxs-lookup"><span data-stu-id="d3e9e-127">Context of hello event.</span></span> |
| <span data-ttu-id="d3e9e-128">resourceProviderName</span><span class="sxs-lookup"><span data-stu-id="d3e9e-128">resourceProviderName</span></span> |<span data-ttu-id="d3e9e-129">erőforrás-szolgáltató hello hello az érintett erőforrás.</span><span class="sxs-lookup"><span data-stu-id="d3e9e-129">hello resource provider of hello impacted resource.</span></span> |
| <span data-ttu-id="d3e9e-130">conditionType</span><span class="sxs-lookup"><span data-stu-id="d3e9e-130">conditionType</span></span> |<span data-ttu-id="d3e9e-131">Mindig "Event".</span><span class="sxs-lookup"><span data-stu-id="d3e9e-131">Always "Event."</span></span> |
| <span data-ttu-id="d3e9e-132">név</span><span class="sxs-lookup"><span data-stu-id="d3e9e-132">name</span></span> |<span data-ttu-id="d3e9e-133">Hello riasztási szabály neve.</span><span class="sxs-lookup"><span data-stu-id="d3e9e-133">Name of hello alert rule.</span></span> |
| <span data-ttu-id="d3e9e-134">id</span><span class="sxs-lookup"><span data-stu-id="d3e9e-134">id</span></span> |<span data-ttu-id="d3e9e-135">Erőforrás-azonosító hello riasztás.</span><span class="sxs-lookup"><span data-stu-id="d3e9e-135">Resource ID of hello alert.</span></span> |
| <span data-ttu-id="d3e9e-136">leírás</span><span class="sxs-lookup"><span data-stu-id="d3e9e-136">description</span></span> |<span data-ttu-id="d3e9e-137">Riasztás leírása az beállított a hello riasztás létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="d3e9e-137">Alert description as set during creation of hello alert.</span></span> |
| <span data-ttu-id="d3e9e-138">subscriptionId</span><span class="sxs-lookup"><span data-stu-id="d3e9e-138">subscriptionId</span></span> |<span data-ttu-id="d3e9e-139">Az Azure előfizetés-azonosító.</span><span class="sxs-lookup"><span data-stu-id="d3e9e-139">Azure Subscription ID.</span></span> |
| <span data-ttu-id="d3e9e-140">időbélyeg</span><span class="sxs-lookup"><span data-stu-id="d3e9e-140">timestamp</span></span> |<span data-ttu-id="d3e9e-141">Mely hello esemény lett létrehozva, amikor hello Azure szolgáltatás, amelynek a feldolgozása hello kérelem ideje.</span><span class="sxs-lookup"><span data-stu-id="d3e9e-141">Time at which hello event was generated by hello Azure service that processed hello request.</span></span> |
| <span data-ttu-id="d3e9e-142">resourceId</span><span class="sxs-lookup"><span data-stu-id="d3e9e-142">resourceId</span></span> |<span data-ttu-id="d3e9e-143">Erőforrás-azonosítója, hello érintett erőforrás.</span><span class="sxs-lookup"><span data-stu-id="d3e9e-143">Resource ID of hello impacted resource.</span></span> |
| <span data-ttu-id="d3e9e-144">erőforráscsoport-név</span><span class="sxs-lookup"><span data-stu-id="d3e9e-144">resourceGroupName</span></span> |<span data-ttu-id="d3e9e-145">Hello hello erőforráscsoport nevét érintett erőforrás</span><span class="sxs-lookup"><span data-stu-id="d3e9e-145">Name of hello resource group for hello impacted resource</span></span> |
| <span data-ttu-id="d3e9e-146">properties</span><span class="sxs-lookup"><span data-stu-id="d3e9e-146">properties</span></span> |<span data-ttu-id="d3e9e-147">Állítsa be a `<Key, Value>` párok (azaz `Dictionary<String, String>`), amely hello esemény részleteit tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="d3e9e-147">Set of `<Key, Value>` pairs (i.e. `Dictionary<String, String>`) that includes details about hello event.</span></span> |
| <span data-ttu-id="d3e9e-148">Esemény</span><span class="sxs-lookup"><span data-stu-id="d3e9e-148">event</span></span> |<span data-ttu-id="d3e9e-149">Hello eseménnyel kapcsolatos metaadatokat tartalmazó elemnek.</span><span class="sxs-lookup"><span data-stu-id="d3e9e-149">Element containing metadata about hello event.</span></span> |
| <span data-ttu-id="d3e9e-150">Engedélyezési</span><span class="sxs-lookup"><span data-stu-id="d3e9e-150">authorization</span></span> |<span data-ttu-id="d3e9e-151">hello RBAC hello esemény tulajdonságai.</span><span class="sxs-lookup"><span data-stu-id="d3e9e-151">hello RBAC properties of hello event.</span></span> <span data-ttu-id="d3e9e-152">Ezek általában például hello "action", "szerepkör" és hello "hatókörben."</span><span class="sxs-lookup"><span data-stu-id="d3e9e-152">These usually include hello “action”, “role” and hello “scope.”</span></span> |
| <span data-ttu-id="d3e9e-153">category</span><span class="sxs-lookup"><span data-stu-id="d3e9e-153">category</span></span> |<span data-ttu-id="d3e9e-154">Hello esemény kategóriáját.</span><span class="sxs-lookup"><span data-stu-id="d3e9e-154">Category of hello event.</span></span> <span data-ttu-id="d3e9e-155">Támogatott értékek a következők: felügyeleti, a riasztásra, biztonság, ServiceHealth, javaslat.</span><span class="sxs-lookup"><span data-stu-id="d3e9e-155">Supported values include: Administrative, Alert, Security, ServiceHealth, Recommendation.</span></span> |
| <span data-ttu-id="d3e9e-156">hívó</span><span class="sxs-lookup"><span data-stu-id="d3e9e-156">caller</span></span> |<span data-ttu-id="d3e9e-157">Hello művelet, a jogcím vagy a rendelkezésre állás alapján SPN jogcím hello felhasználó e-mail címe.</span><span class="sxs-lookup"><span data-stu-id="d3e9e-157">Email address of hello user who performed hello operation, UPN claim, or SPN claim based on availability.</span></span> <span data-ttu-id="d3e9e-158">Bizonyos rendszerhívások null értékű lehet.</span><span class="sxs-lookup"><span data-stu-id="d3e9e-158">Can be null for certain system calls.</span></span> |
| <span data-ttu-id="d3e9e-159">correlationId</span><span class="sxs-lookup"><span data-stu-id="d3e9e-159">correlationId</span></span> |<span data-ttu-id="d3e9e-160">Általában egy GUID karakterlánc-formátum.</span><span class="sxs-lookup"><span data-stu-id="d3e9e-160">Usually a GUID in string format.</span></span> <span data-ttu-id="d3e9e-161">A correlationId események tartozik toohello nagyobb műveletet és általában megosztani az egy correlationId.</span><span class="sxs-lookup"><span data-stu-id="d3e9e-161">Events with correlationId belong toohello same larger action and usually share a correlationId.</span></span> |
| <span data-ttu-id="d3e9e-162">eventDescription</span><span class="sxs-lookup"><span data-stu-id="d3e9e-162">eventDescription</span></span> |<span data-ttu-id="d3e9e-163">Statikus szöveg hello esemény leírását.</span><span class="sxs-lookup"><span data-stu-id="d3e9e-163">Static text description of hello event.</span></span> |
| <span data-ttu-id="d3e9e-164">eventDataId</span><span class="sxs-lookup"><span data-stu-id="d3e9e-164">eventDataId</span></span> |<span data-ttu-id="d3e9e-165">Hello esemény egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="d3e9e-165">Unique identifier for hello event.</span></span> |
| <span data-ttu-id="d3e9e-166">Eseményforrás</span><span class="sxs-lookup"><span data-stu-id="d3e9e-166">eventSource</span></span> |<span data-ttu-id="d3e9e-167">Neve hello Azure-szolgáltatásra vagy infrastruktúrát, hogy létrehozott hello esemény.</span><span class="sxs-lookup"><span data-stu-id="d3e9e-167">Name of hello Azure service or infrastructure that generated hello event.</span></span> |
| <span data-ttu-id="d3e9e-168">httpRequest</span><span class="sxs-lookup"><span data-stu-id="d3e9e-168">httpRequest</span></span> |<span data-ttu-id="d3e9e-169">Általában hello "clientRequestId", "clientIpAddress" és "method" tartalmazza (pl. PUT HTTP-metódus).</span><span class="sxs-lookup"><span data-stu-id="d3e9e-169">Usually includes hello “clientRequestId”, “clientIpAddress” and “method” (HTTP method e.g. PUT).</span></span> |
| <span data-ttu-id="d3e9e-170">szint</span><span class="sxs-lookup"><span data-stu-id="d3e9e-170">level</span></span> |<span data-ttu-id="d3e9e-171">Hello a következő értékek egyikét: "Kritikus", "Error", "Figyelmeztetés", "Tájékoztató" és "Részletes."</span><span class="sxs-lookup"><span data-stu-id="d3e9e-171">One of hello following values: “Critical”, “Error”, “Warning”, “Informational” and “Verbose.”</span></span> |
| <span data-ttu-id="d3e9e-172">OperationID azonosítójú</span><span class="sxs-lookup"><span data-stu-id="d3e9e-172">operationId</span></span> |<span data-ttu-id="d3e9e-173">Általában egy GUID azonosító toosingle művelet megfelelő hello események között meg van osztva.</span><span class="sxs-lookup"><span data-stu-id="d3e9e-173">Usually a GUID shared among hello events corresponding toosingle operation.</span></span> |
| <span data-ttu-id="d3e9e-174">operationName</span><span class="sxs-lookup"><span data-stu-id="d3e9e-174">operationName</span></span> |<span data-ttu-id="d3e9e-175">Hello művelet neve.</span><span class="sxs-lookup"><span data-stu-id="d3e9e-175">Name of hello operation.</span></span> |
| <span data-ttu-id="d3e9e-176">properties</span><span class="sxs-lookup"><span data-stu-id="d3e9e-176">properties</span></span> |<span data-ttu-id="d3e9e-177">Hello esemény tulajdonságai.</span><span class="sxs-lookup"><span data-stu-id="d3e9e-177">Properties of hello event.</span></span> |
| <span data-ttu-id="d3e9e-178">status</span><span class="sxs-lookup"><span data-stu-id="d3e9e-178">status</span></span> |<span data-ttu-id="d3e9e-179">Karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="d3e9e-179">String.</span></span> <span data-ttu-id="d3e9e-180">Hello művelet állapotát.</span><span class="sxs-lookup"><span data-stu-id="d3e9e-180">Status of hello operation.</span></span> <span data-ttu-id="d3e9e-181">Gyakori értékek a következők: "Started", "Folyamatban", "Sikeres", "Sikertelen", "Active", "Megoldott".</span><span class="sxs-lookup"><span data-stu-id="d3e9e-181">Common values include: "Started", "In Progress", "Succeeded", "Failed", "Active", "Resolved".</span></span> |
| <span data-ttu-id="d3e9e-182">a részállapot</span><span class="sxs-lookup"><span data-stu-id="d3e9e-182">subStatus</span></span> |<span data-ttu-id="d3e9e-183">Általában tartalmazza a megfelelő REST-hívást hello hello HTTP-állapotkódot.</span><span class="sxs-lookup"><span data-stu-id="d3e9e-183">Usually includes hello HTTP status code of hello corresponding REST call.</span></span> <span data-ttu-id="d3e9e-184">A részállapot leíró más karakterláncok is tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="d3e9e-184">It might also include other strings describing a substatus.</span></span> <span data-ttu-id="d3e9e-185">Közös részállapot értékek a következők: OK (HTTP-állapotkód:: 200), létrehozott (HTTP-állapotkód:: 201-es), fogadja el (HTTP-állapotkód:: 202), nem a tartalom (HTTP-állapotkód: 204), hibás kérés (HTTP-állapotkód:: 400), nem található (HTTP-állapotkód:: 404-es), ütközés (HTTP-állapotkód:: 409), belső kiszolgálóhiba (HTTP-állapotkód:: 500), szolgáltatás nem érhető el (HTTP-állapotkód:: 503-as), átjáró időtúllépése (HTTP-állapotkód: 504)</span><span class="sxs-lookup"><span data-stu-id="d3e9e-185">Common substatus values include: OK (HTTP Status Code: 200), Created (HTTP Status Code: 201), Accepted (HTTP Status Code: 202), No Content (HTTP Status Code: 204), Bad Request (HTTP Status Code: 400), Not Found (HTTP Status Code: 404), Conflict (HTTP Status Code: 409), Internal Server Error (HTTP Status Code: 500), Service Unavailable (HTTP Status Code: 503), Gateway Timeout (HTTP Status Code: 504)</span></span> |

## <a name="next-steps"></a><span data-ttu-id="d3e9e-186">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d3e9e-186">Next steps</span></span>
* [<span data-ttu-id="d3e9e-187">További tudnivalók hello műveletnapló</span><span class="sxs-lookup"><span data-stu-id="d3e9e-187">Learn more about hello Activity Log</span></span>](monitoring-overview-activity-logs.md)
* [<span data-ttu-id="d3e9e-188">Azure Automation-parancsfájlok (Runbookok) végrehajtása Azure riasztások</span><span class="sxs-lookup"><span data-stu-id="d3e9e-188">Execute Azure Automation scripts (Runbooks) on Azure alerts</span></span>](http://go.microsoft.com/fwlink/?LinkId=627081)
* <span data-ttu-id="d3e9e-189">[Használja a logikai alkalmazás toosend keresztül Twilio SMS az Azure-riasztás alapján](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app).</span><span class="sxs-lookup"><span data-stu-id="d3e9e-189">[Use Logic App toosend an SMS via Twilio from an Azure alert](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app).</span></span> <span data-ttu-id="d3e9e-190">Ebben a példában a metrika riasztások, de egy tevékenységnapló alert módosított toowork lehet.</span><span class="sxs-lookup"><span data-stu-id="d3e9e-190">This example is for metric alerts, but could be modified toowork with an Activity Log alert.</span></span>
* <span data-ttu-id="d3e9e-191">[Használja a logikai alkalmazás toosend Azure riasztást Slack üzenetét](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app).</span><span class="sxs-lookup"><span data-stu-id="d3e9e-191">[Use Logic App toosend a Slack message from an Azure alert](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app).</span></span> <span data-ttu-id="d3e9e-192">Ebben a példában a metrika riasztások, de egy tevékenységnapló alert módosított toowork lehet.</span><span class="sxs-lookup"><span data-stu-id="d3e9e-192">This example is for metric alerts, but could be modified toowork with an Activity Log alert.</span></span>
* <span data-ttu-id="d3e9e-193">[Használja az Azure-riasztás alapján üzenet tooan Azure Queue logikai alkalmazás toosend](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app).</span><span class="sxs-lookup"><span data-stu-id="d3e9e-193">[Use Logic App toosend a message tooan Azure Queue from an Azure alert](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app).</span></span> <span data-ttu-id="d3e9e-194">Ebben a példában a metrika riasztások, de egy tevékenységnapló alert módosított toowork lehet.</span><span class="sxs-lookup"><span data-stu-id="d3e9e-194">This example is for metric alerts, but could be modified toowork with an Activity Log alert.</span></span>
