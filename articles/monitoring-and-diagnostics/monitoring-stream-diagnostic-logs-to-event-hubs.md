---
title: "Az Event Hubs Namespace Azure diagnosztikai naplók adatfolyam |} Microsoft Docs"
description: "Útmutató az Azure diagnosztikai naplók az Event Hubs névtérhez adatfolyamként történő küldéséhez."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 42bc4845-c564-4568-b72d-0614591ebd80
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/21/2017
ms.author: johnkem
ms.openlocfilehash: 01ba8ddfcf90e1368ac147296fd180f99420d96f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="stream-azure-diagnostic-logs-to-an-event-hubs-namespace"></a><span data-ttu-id="66d5d-103">Az Event Hubs Namespace Azure diagnosztikai naplók adatfolyam</span><span class="sxs-lookup"><span data-stu-id="66d5d-103">Stream Azure Diagnostic Logs to an Event Hubs Namespace</span></span>
<span data-ttu-id="66d5d-104">**[Az Azure diagnosztikai naplók](monitoring-overview-of-diagnostic-logs.md)**  továbbítható bármely alkalmazás a beépített "Exportálás az Event Hubs" beállítás használatával, a portálon, vagy egy diagnosztikai beállítás, az Azure PowerShell-parancsmagok vagy Azure parancssori felület használatával a Service Bus Szabályazonosító engedélyezésével közel valós időben.</span><span class="sxs-lookup"><span data-stu-id="66d5d-104">**[Azure diagnostic logs](monitoring-overview-of-diagnostic-logs.md)** can be streamed in near real time to any application using the built-in “Export to Event Hubs” option in the Portal, or by enabling the Service Bus Rule ID in a diagnostic setting via the Azure PowerShell Cmdlets or Azure CLI.</span></span>

## <a name="what-you-can-do-with-diagnostics-logs-and-event-hubs"></a><span data-ttu-id="66d5d-105">Mi mindent diagnosztikai naplók és az Event Hubs</span><span class="sxs-lookup"><span data-stu-id="66d5d-105">What you can do with diagnostics logs and Event Hubs</span></span>
<span data-ttu-id="66d5d-106">Az alábbiakban néhány módszereket, akkor előfordulhat, hogy az adatfolyam-továbbítási képesség a diagnosztikai naplók:</span><span class="sxs-lookup"><span data-stu-id="66d5d-106">Here are just a few ways you might use the streaming capability for Diagnostic Logs:</span></span>

* <span data-ttu-id="66d5d-107">**Adatfolyam-bejegyzéseit, amelyek a 3. fél naplózása és telemetriai rendszerek** – adott idő alatt, az Event Hubs streaming lesz a mechanizmust, amely a diagnosztikai naplók külső siem-ektől a csövön keresztüli, valamint naplófájl-elemzési megoldásokat.</span><span class="sxs-lookup"><span data-stu-id="66d5d-107">**Stream logs to 3rd party logging and telemetry systems** – Over time, Event Hubs streaming will become the mechanism to pipe your diagnostic logs in to third-party SIEMs and log analytics solutions.</span></span>
* <span data-ttu-id="66d5d-108">**Szolgáltatás állapotának megtekintéséhez a Power bi szolgáltatásba "forró path" adatok folyamatos** – az Event Hubs használatával, a Stream Analytics és a Power BI, könnyen átalakíthatja a diagnosztikai adatokat a valós idejű elemzése az Azure-szolgáltatások a közelében.</span><span class="sxs-lookup"><span data-stu-id="66d5d-108">**View service health by streaming “hot path” data to PowerBI** – Using Event Hubs, Stream Analytics, and PowerBI, you can easily transform your diagnostics data in to near real-time insights on your Azure services.</span></span> <span data-ttu-id="66d5d-109">[A dokumentációs cikket tartalmaz az Event Hubs beállítása, a Stream Analytics adatok feldolgozása és kimenetként használata a Power bi kiváló áttekintést nyújt](../stream-analytics/stream-analytics-power-bi-dashboard.md).</span><span class="sxs-lookup"><span data-stu-id="66d5d-109">[This documentation article gives a great overview of how to set up Event Hubs, process data with Stream Analytics, and use PowerBI as an output](../stream-analytics/stream-analytics-power-bi-dashboard.md).</span></span> <span data-ttu-id="66d5d-110">Az alábbiakban néhány tippek a diagnosztikai naplók első beállítása:</span><span class="sxs-lookup"><span data-stu-id="66d5d-110">Here are a few tips for getting set up with diagnostic logs:</span></span>
  
  * <span data-ttu-id="66d5d-111">Az eseményközpontok a diagnosztikai naplók kategóriájú automatikusan jön létre, jelölje be a beállítást, a portálon, vagy engedélyezze a PowerShell segítségével, így szeretné kiválasztani az event hubs kezdetű névvel névtér **insights -**.</span><span class="sxs-lookup"><span data-stu-id="66d5d-111">An event hub for a category of diagnostic logs is created automatically when you check the option in the portal or enable it through PowerShell, so you want to select the event hub in the namespace with the name that starts with **insights-**.</span></span>
  * <span data-ttu-id="66d5d-112">A következő SQL-kódot a Stream Analytics mintalekérdezést használó összes a napló az adatok a Power bi táblára:</span><span class="sxs-lookup"><span data-stu-id="66d5d-112">The following SQL code is a sample Stream Analytics query that you can use to parse all the log data in to a PowerBI table:</span></span>

    ```sql
    SELECT
    records.ArrayValue.[Properties you want to track]
    INTO
    [OutputSourceName – the PowerBI source]
    FROM
    [InputSourceName] AS e
    CROSS APPLY GetArrayElements(e.records) AS records
    ```

* <span data-ttu-id="66d5d-113">**Létre egy egyéni telemetriai adatokat és a naplózás platform** – Ha már rendelkezik egy egyedi telemetriai platform vagy csak egy kiválóan méretezhető kiépítésével foglalkozó végezni azok jellegű közzétételi-feliratkozási az Event hubs lehetővé teszi a rugalmas tölti be a diagnosztikai naplókat.</span><span class="sxs-lookup"><span data-stu-id="66d5d-113">**Build a custom telemetry and logging platform** – If you already have a custom-built telemetry platform or are just thinking about building one, the highly scalable publish-subscribe nature of Event Hubs allows you to flexibly ingest diagnostic logs.</span></span> <span data-ttu-id="66d5d-114">[Az Event Hubs használatával egy globális méretű telemetriai platform itt Dan Rosanova útmutatójában](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/).</span><span class="sxs-lookup"><span data-stu-id="66d5d-114">[See Dan Rosanova’s guide to using Event Hubs in a global scale telemetry platform here](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/).</span></span>

## <a name="enable-streaming-of-diagnostic-logs"></a><span data-ttu-id="66d5d-115">Adatfolyamként való küldése a diagnosztikai naplók engedélyezése</span><span class="sxs-lookup"><span data-stu-id="66d5d-115">Enable streaming of diagnostic logs</span></span>
<span data-ttu-id="66d5d-116">Adatfolyamként való küldése a diagnosztikai naplók programozott módon, a portálon, vagy használatával engedélyezheti a [Azure figyelő REST API-k](https://docs.microsoft.com/rest/api/monitor/servicediagnosticsettings).</span><span class="sxs-lookup"><span data-stu-id="66d5d-116">You can enable streaming of diagnostic logs programmatically, via the portal, or using the [Azure Monitor REST APIs](https://docs.microsoft.com/rest/api/monitor/servicediagnosticsettings).</span></span> <span data-ttu-id="66d5d-117">Mindkét módszer esetén létrehozhat egy diagnosztikai beállítás található, amely akkor adja meg az Event Hubs-névteret és a napló kategóriák és a metrikák szeretne küldeni névtérhez.</span><span class="sxs-lookup"><span data-stu-id="66d5d-117">Either way, you create a diagnostic setting in which you specify an Event Hubs namespace and the log categories and metrics you want to send in to the namespace.</span></span> <span data-ttu-id="66d5d-118">Az eseményközpontok engedélyezi napló kategóriákhoz tartozó névtér jön létre.</span><span class="sxs-lookup"><span data-stu-id="66d5d-118">An event hub is created in the namespace for each log category you enable.</span></span> <span data-ttu-id="66d5d-119">A diagnosztika **napló kategória** a naplóban, amely egy erőforrás gyűjthet típusa.</span><span class="sxs-lookup"><span data-stu-id="66d5d-119">A diagnostic **log category** is a type of log that a resource may collect.</span></span>

> [!WARNING]
> <span data-ttu-id="66d5d-120">Engedélyezése, valamint a folyamatos átviteli a számítási erőforrások (például a virtuális gépek vagy a Service Fabric) diagnosztikai naplók [meg kell adni egy másik lépések](../event-hubs/event-hubs-streaming-azure-diags-data.md).</span><span class="sxs-lookup"><span data-stu-id="66d5d-120">Enabling and streaming diagnostic logs from Compute resources (for example, VMs or Service Fabric) [requires a different set of steps](../event-hubs/event-hubs-streaming-azure-diags-data.md).</span></span>
> 
> 

<span data-ttu-id="66d5d-121">A Service Bus vagy az Event Hubs névtér nem kell lennie a naplók kibocsátó mindaddig, amíg a beállítás konfigurálása felhasználó hozzáfér megfelelő RBAC mindkét előfizetéshez erőforrás ugyanahhoz az előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="66d5d-121">The Service Bus or Event Hubs namespace does not have to be in the same subscription as the resource emitting logs as long as the user who configures the setting has appropriate RBAC access to both subscriptions.</span></span>

## <a name="stream-diagnostic-logs-using-the-portal"></a><span data-ttu-id="66d5d-122">Az adatfolyam diagnosztikai naplókat a portálon</span><span class="sxs-lookup"><span data-stu-id="66d5d-122">Stream diagnostic logs using the portal</span></span>
1. <span data-ttu-id="66d5d-123">A portál Azure figyelő keresse meg és kattintson a **diagnosztikai beállítások**</span><span class="sxs-lookup"><span data-stu-id="66d5d-123">In the portal, navigate to Azure Monitor and click on **Diagnostic Settings**</span></span>

    ![Figyelés szakaszban Azure-figyelő](media/monitoring-stream-diagnostic-logs-to-event-hubs/diagnostic-settings-blade.png)

2. <span data-ttu-id="66d5d-125">Opcionálisan erőforráscsoport és erőforrások típus szerint a lista szűréséhez, majd kattintson az erőforráson, amelynek szeretné beállítani a diagnosztikai.</span><span class="sxs-lookup"><span data-stu-id="66d5d-125">Optionally filter the list by resource group or resource type, then click on the resource for which you would like to set a diagnostic setting.</span></span>

3. <span data-ttu-id="66d5d-126">Ha a beállítások nem található az erőforrás a választott, kéri beállítás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="66d5d-126">If no settings exist on the resource you have selected, you are prompted to create a setting.</span></span> <span data-ttu-id="66d5d-127">Kattintson a "Diagnosztika bekapcsolásához."</span><span class="sxs-lookup"><span data-stu-id="66d5d-127">Click "Turn on diagnostics."</span></span>

   ![Diagnosztikai beállítás - nincsenek meglévő beállítások hozzáadása](media/monitoring-stream-diagnostic-logs-to-event-hubs/diagnostic-settings-none.png)

   <span data-ttu-id="66d5d-129">Ha az erőforrás-meglévő beállítások, látni fogja már ehhez az erőforráshoz konfigurált beállítások listája.</span><span class="sxs-lookup"><span data-stu-id="66d5d-129">If there are existing settings on the resource, you will see a list of settings already configured on this resource.</span></span> <span data-ttu-id="66d5d-130">A "Hozzáadás diagnosztikai beállításának."</span><span class="sxs-lookup"><span data-stu-id="66d5d-130">Click "Add diagnostic setting."</span></span>

   ![Diagnosztikai beállítás - meglévő beállítások hozzáadása](media/monitoring-stream-diagnostic-logs-to-event-hubs/diagnostic-settings-multiple.png)

3. <span data-ttu-id="66d5d-132">Adjon a beállítás a neve, és jelölje be a **adatfolyam egy eseményközpontba**, majd válassza ki az Event Hubs névteret.</span><span class="sxs-lookup"><span data-stu-id="66d5d-132">Give your setting a name and check the box for **Stream to an event hub**, then select an Event Hubs namespace.</span></span>
   
   ![Diagnosztikai beállítás - meglévő beállítások hozzáadása](media/monitoring-stream-diagnostic-logs-to-event-hubs/diagnostic-settings-configure.png)
    
   <span data-ttu-id="66d5d-134">A kijelölt névtér lesz az eseményközpont létre (Ha ez az első alkalommal adatfolyam diagnosztikai naplók) vagy a folyamatos átviteli rendelkező (ha van már erőforrásokat, amelyek ehhez a névtérhez napló kategória vannak streaming), és a házirend határozza meg az adatfolyam-továbbítási mechanizmus jogosultságai.</span><span class="sxs-lookup"><span data-stu-id="66d5d-134">The namespace selected will be where the event hub is created (if this is your first time streaming diagnostic logs) or streamed to (if there are already resources that are streaming that log category to this namespace), and the policy defines the permissions that the streaming mechanism has.</span></span> <span data-ttu-id="66d5d-135">Napjainkban az eseményközpontba streaming engedélyekkel kell rendelkeznie kezelése, a Küldés és a figyelés.</span><span class="sxs-lookup"><span data-stu-id="66d5d-135">Today, streaming to an event hub requires Manage, Send, and Listen permissions.</span></span> <span data-ttu-id="66d5d-136">Hozzon létre, vagy az Event Hubs névtér megosztott hozzáférési házirendek a portálon az konfigurálása lapon módosítsa a névtér.</span><span class="sxs-lookup"><span data-stu-id="66d5d-136">You can create or modify Event Hubs namespace shared access policies in the portal under the Configure tab for your namespace.</span></span> <span data-ttu-id="66d5d-137">Ezek a diagnosztikai beállítások frissítéséhez az ügyfél a ListKey engedéllyel kell rendelkeznie az Event Hubs engedélyezési szabályt.</span><span class="sxs-lookup"><span data-stu-id="66d5d-137">To update one of these diagnostic settings, the client must have the ListKey permission on the Event Hubs authorization rule.</span></span>

4. <span data-ttu-id="66d5d-138">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="66d5d-138">Click **Save**.</span></span>

<span data-ttu-id="66d5d-139">Néhány másodpercen belül az új beállítás jelenik meg az ehhez az erőforráshoz beállítások listáját, és diagnosztikai naplók részére, hogy a tárfiókot, amint létrejön az új esemény-adatokat.</span><span class="sxs-lookup"><span data-stu-id="66d5d-139">After a few moments, the new setting appears in your list of settings for this resource, and diagnostic logs are streamed to that storage account as soon as new event data is generated.</span></span>

### <a name="via-powershell-cmdlets"></a><span data-ttu-id="66d5d-140">PowerShell-parancsmagok használatával</span><span class="sxs-lookup"><span data-stu-id="66d5d-140">Via PowerShell Cmdlets</span></span>
<span data-ttu-id="66d5d-141">Adatfolyamként keresztül a [Azure PowerShell-parancsmagok](insights-powershell-samples.md), használhatja a `Set-AzureRmDiagnosticSetting` parancsmag ezekkel a paraméterekkel:</span><span class="sxs-lookup"><span data-stu-id="66d5d-141">To enable streaming via the [Azure PowerShell Cmdlets](insights-powershell-samples.md), you can use the `Set-AzureRmDiagnosticSetting` cmdlet with these parameters:</span></span>

```powershell
Set-AzureRmDiagnosticSetting -ResourceId [your resource ID] -ServiceBusRuleId [your Service Bus rule ID] -Enabled $true
```

<span data-ttu-id="66d5d-142">A Service Bus Szabályazonosító: karakterlánc a következő formátumban: `{Service Bus resource ID}/authorizationrules/{key name}`, például `/subscriptions/{subscription ID}/resourceGroups/Default-ServiceBus-WestUS/providers/Microsoft.ServiceBus/namespaces/{Service Bus namespace}/authorizationrules/RootManageSharedAccessKey`.</span><span class="sxs-lookup"><span data-stu-id="66d5d-142">The Service Bus Rule ID is a string with this format: `{Service Bus resource ID}/authorizationrules/{key name}`, for example, `/subscriptions/{subscription ID}/resourceGroups/Default-ServiceBus-WestUS/providers/Microsoft.ServiceBus/namespaces/{Service Bus namespace}/authorizationrules/RootManageSharedAccessKey`.</span></span>

### <a name="via-azure-cli"></a><span data-ttu-id="66d5d-143">Az Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="66d5d-143">Via Azure CLI</span></span>
<span data-ttu-id="66d5d-144">Adatfolyamként keresztül a [Azure CLI](insights-cli-samples.md), használhatja a `insights diagnostic set` ehhez hasonló parancsot:</span><span class="sxs-lookup"><span data-stu-id="66d5d-144">To enable streaming via the [Azure CLI](insights-cli-samples.md), you can use the `insights diagnostic set` command like this:</span></span>

```azurecli
azure insights diagnostic set --resourceId <resourceID> --serviceBusRuleId <serviceBusRuleID> --enabled true
```

<span data-ttu-id="66d5d-145">A Service Bus Szabályazonosító ugyanazt a formátumot használja, a PowerShell-parancsmag leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="66d5d-145">Use the same format for Service Bus Rule ID as explained for the PowerShell Cmdlet.</span></span>

## <a name="how-do-i-consume-the-log-data-from-event-hubs"></a><span data-ttu-id="66d5d-146">Hogyan Eseményközpontokból származó napló adatokat felhasználó?</span><span class="sxs-lookup"><span data-stu-id="66d5d-146">How do I consume the log data from Event Hubs?</span></span>
<span data-ttu-id="66d5d-147">Minta kimenet Eseményközpontokból származó adatokat a következő:</span><span class="sxs-lookup"><span data-stu-id="66d5d-147">Here is sample output data from Event Hubs:</span></span>

```json
{
    "records": [
        {
            "time": "2016-07-15T18:00:22.6235064Z",
            "workflowId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA",
            "resourceId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA/RUNS/08587330013509921957/ACTIONS/SEND_EMAIL",
            "category": "WorkflowRuntime",
            "level": "Error",
            "operationName": "Microsoft.Logic/workflows/workflowActionCompleted",
            "properties": {
                "$schema": "2016-04-01-preview",
                "startTime": "2016-07-15T17:58:55.048482Z",
                "endTime": "2016-07-15T18:00:22.4109204Z",
                "status": "Failed",
                "code": "BadGateway",
                "resource": {
                    "subscriptionId": "df602c9c-7aa0-407d-a6fb-eb20c8bd1192",
                    "resourceGroupName": "JohnKemTest",
                    "workflowId": "243aac67fe904cf195d4a28297803785",
                    "workflowName": "JohnKemTestLA",
                    "runId": "08587330013509921957",
                    "location": "westus",
                    "actionName": "Send_email"
                },
                "correlation": {
                    "actionTrackingId": "29a9862f-969b-4c70-90c4-dfbdc814e413",
                    "clientTrackingId": "08587330013509921958"
                }
            }
        },
        {
            "time": "2016-07-15T18:01:15.7532989Z",
            "workflowId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA",
            "resourceId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA/RUNS/08587330012106702630/ACTIONS/SEND_EMAIL",
            "category": "WorkflowRuntime",
            "level": "Information",
            "operationName": "Microsoft.Logic/workflows/workflowActionStarted",
            "properties": {
                "$schema": "2016-04-01-preview",
                "startTime": "2016-07-15T18:01:15.5828115Z",
                "status": "Running",
                "resource": {
                    "subscriptionId": "df602c9c-7aa0-407d-a6fb-eb20c8bd1192",
                    "resourceGroupName": "JohnKemTest",
                    "workflowId": "243aac67fe904cf195d4a28297803785",
                    "workflowName": "JohnKemTestLA",
                    "runId": "08587330012106702630",
                    "location": "westus",
                    "actionName": "Send_email"
                },
                "correlation": {
                    "actionTrackingId": "042fb72c-7bd4-439e-89eb-3cf4409d429e",
                    "clientTrackingId": "08587330012106702632"
                }
            }
        }
    ]
}
```

| <span data-ttu-id="66d5d-148">Elem neve</span><span class="sxs-lookup"><span data-stu-id="66d5d-148">Element Name</span></span> | <span data-ttu-id="66d5d-149">Leírás</span><span class="sxs-lookup"><span data-stu-id="66d5d-149">Description</span></span> |
| --- | --- |
| <span data-ttu-id="66d5d-150">rekordok</span><span class="sxs-lookup"><span data-stu-id="66d5d-150">records</span></span> |<span data-ttu-id="66d5d-151">Ezek a hasznos adatok az összes naplóeseményeket tömbjét.</span><span class="sxs-lookup"><span data-stu-id="66d5d-151">An array of all log events in this payload.</span></span> |
| <span data-ttu-id="66d5d-152">time</span><span class="sxs-lookup"><span data-stu-id="66d5d-152">time</span></span> |<span data-ttu-id="66d5d-153">Az időt az esemény történt.</span><span class="sxs-lookup"><span data-stu-id="66d5d-153">Time at which the event occurred.</span></span> |
| <span data-ttu-id="66d5d-154">category</span><span class="sxs-lookup"><span data-stu-id="66d5d-154">category</span></span> |<span data-ttu-id="66d5d-155">Ez az esemény napló kategóriát.</span><span class="sxs-lookup"><span data-stu-id="66d5d-155">Log category for this event.</span></span> |
| <span data-ttu-id="66d5d-156">resourceId</span><span class="sxs-lookup"><span data-stu-id="66d5d-156">resourceId</span></span> |<span data-ttu-id="66d5d-157">Az erőforrás ezt az eseményt létrehozó erőforrás-azonosító.</span><span class="sxs-lookup"><span data-stu-id="66d5d-157">Resource ID of the resource that generated this event.</span></span> |
| <span data-ttu-id="66d5d-158">operationName</span><span class="sxs-lookup"><span data-stu-id="66d5d-158">operationName</span></span> |<span data-ttu-id="66d5d-159">A művelet neve.</span><span class="sxs-lookup"><span data-stu-id="66d5d-159">Name of the operation.</span></span> |
| <span data-ttu-id="66d5d-160">szint</span><span class="sxs-lookup"><span data-stu-id="66d5d-160">level</span></span> |<span data-ttu-id="66d5d-161">Választható.</span><span class="sxs-lookup"><span data-stu-id="66d5d-161">Optional.</span></span> <span data-ttu-id="66d5d-162">A naplózási esemény szintjét jelzi.</span><span class="sxs-lookup"><span data-stu-id="66d5d-162">Indicates the log event level.</span></span> |
| <span data-ttu-id="66d5d-163">properties</span><span class="sxs-lookup"><span data-stu-id="66d5d-163">properties</span></span> |<span data-ttu-id="66d5d-164">Az esemény tulajdonságai.</span><span class="sxs-lookup"><span data-stu-id="66d5d-164">Properties of the event.</span></span> |

<span data-ttu-id="66d5d-165">Megtekintheti az összes erőforrás-szolgáltató, amely támogatja az Event hubs streaming listáját [Itt](monitoring-overview-of-diagnostic-logs.md).</span><span class="sxs-lookup"><span data-stu-id="66d5d-165">You can view a list of all resource providers that support streaming to Event Hubs [here](monitoring-overview-of-diagnostic-logs.md).</span></span>

## <a name="stream-data-from-compute-resources"></a><span data-ttu-id="66d5d-166">Az adatfolyam adatok a számítási erőforrásokat</span><span class="sxs-lookup"><span data-stu-id="66d5d-166">Stream data from Compute resources</span></span>
<span data-ttu-id="66d5d-167">Adatfolyam formájában a diagnosztikai naplókat a számítási erőforrásokat, a Windows Azure diagnosztikai ügynök használatával is.</span><span class="sxs-lookup"><span data-stu-id="66d5d-167">You can also stream diagnostic logs from Compute resources using the Windows Azure Diagnostics agent.</span></span> <span data-ttu-id="66d5d-168">[Ebben a cikkben találhat](../event-hubs/event-hubs-streaming-azure-diags-data.md) beállítása, amely a.</span><span class="sxs-lookup"><span data-stu-id="66d5d-168">[See this article](../event-hubs/event-hubs-streaming-azure-diags-data.md) for how to set that up.</span></span>

## <a name="next-steps"></a><span data-ttu-id="66d5d-169">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="66d5d-169">Next steps</span></span>
* [<span data-ttu-id="66d5d-170">További tudnivalók az Azure diagnosztikai naplók</span><span class="sxs-lookup"><span data-stu-id="66d5d-170">Read more about Azure Diagnostic Logs</span></span>](monitoring-overview-of-diagnostic-logs.md)
* [<span data-ttu-id="66d5d-171">Bevezetés az Event Hubs használatába</span><span class="sxs-lookup"><span data-stu-id="66d5d-171">Get started with Event Hubs</span></span>](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)

