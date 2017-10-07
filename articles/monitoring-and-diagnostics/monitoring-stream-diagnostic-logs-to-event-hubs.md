---
title: "aaaStream Azure diagnosztikai naplók tooan Event Hubs Namespace |} Microsoft Docs"
description: "Ismerje meg, hogy miként naplózza az Azure diagnosztikai toostream a tooan Event Hubs névtér."
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
ms.openlocfilehash: 00092ea8f3fe4fa1476e3a697bf1e8645dd21e6e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="stream-azure-diagnostic-logs-tooan-event-hubs-namespace"></a><span data-ttu-id="b7d3b-103">Az adatfolyam Azure diagnosztikai naplók tooan Event Hubs Namespace</span><span class="sxs-lookup"><span data-stu-id="b7d3b-103">Stream Azure Diagnostic Logs tooan Event Hubs Namespace</span></span>
<span data-ttu-id="b7d3b-104">**[Az Azure diagnosztikai naplók](monitoring-overview-of-diagnostic-logs.md)**  továbbítható a közel valós idejű tooany alkalmazások hello beépített "TooEvent hubok exportálása" lehetőséggel a portál hello, vagy engedélyezésével hello a Service Bus Szabályazonosító egy diagnosztikai beállítás hello Azure PowerShell használatával -Parancsmagjaival vagy Azure CLI-t.</span><span class="sxs-lookup"><span data-stu-id="b7d3b-104">**[Azure diagnostic logs](monitoring-overview-of-diagnostic-logs.md)** can be streamed in near real time tooany application using hello built-in “Export tooEvent Hubs” option in hello Portal, or by enabling hello Service Bus Rule ID in a diagnostic setting via hello Azure PowerShell Cmdlets or Azure CLI.</span></span>

## <a name="what-you-can-do-with-diagnostics-logs-and-event-hubs"></a><span data-ttu-id="b7d3b-105">Mi mindent diagnosztikai naplók és az Event Hubs</span><span class="sxs-lookup"><span data-stu-id="b7d3b-105">What you can do with diagnostics logs and Event Hubs</span></span>
<span data-ttu-id="b7d3b-106">Az alábbiakban néhány módokon használhatja a funkció a diagnosztikai naplók streaming hello:</span><span class="sxs-lookup"><span data-stu-id="b7d3b-106">Here are just a few ways you might use hello streaming capability for Diagnostic Logs:</span></span>

* <span data-ttu-id="b7d3b-107">**Stream naplók too3rd naplózása és telemetriai rendszerekre** – adott idő alatt, az Event Hubs streaming fog vált hello mechanizmus toopipe toothird gyártótól a siem-ektől a diagnosztikai naplók és elemzési megoldásokat jelentkezzen.</span><span class="sxs-lookup"><span data-stu-id="b7d3b-107">**Stream logs too3rd party logging and telemetry systems** – Over time, Event Hubs streaming will become hello mechanism toopipe your diagnostic logs in toothird-party SIEMs and log analytics solutions.</span></span>
* <span data-ttu-id="b7d3b-108">**Szolgáltatás állapotának megtekintéséhez a "gyors path" adatok tooPowerBI folyamatos** – Event Hubs használatával, a Stream Analytics és a Power BI, könnyen alakíthatja át az Azure-szolgáltatások a toonear valós idejű elemzése a diagnosztikai adatokat.</span><span class="sxs-lookup"><span data-stu-id="b7d3b-108">**View service health by streaming “hot path” data tooPowerBI** – Using Event Hubs, Stream Analytics, and PowerBI, you can easily transform your diagnostics data in toonear real-time insights on your Azure services.</span></span> <span data-ttu-id="b7d3b-109">[A dokumentációs cikket tartalmaz hogyan tooset fel az Event Hubs feldolgozni az adatokat az Stream Analytics használ, és kimenetként használata a Power bi kiváló áttekintést nyújt](../stream-analytics/stream-analytics-power-bi-dashboard.md).</span><span class="sxs-lookup"><span data-stu-id="b7d3b-109">[This documentation article gives a great overview of how tooset up Event Hubs, process data with Stream Analytics, and use PowerBI as an output](../stream-analytics/stream-analytics-power-bi-dashboard.md).</span></span> <span data-ttu-id="b7d3b-110">Az alábbiakban néhány tippek a diagnosztikai naplók első beállítása:</span><span class="sxs-lookup"><span data-stu-id="b7d3b-110">Here are a few tips for getting set up with diagnostic logs:</span></span>
  
  * <span data-ttu-id="b7d3b-111">Az eseményközpontok a diagnosztikai naplók kategóriájú automatikusan létrejön hello portálon hello beállítást, vagy engedélyezze a PowerShell segítségével, tehát tooselect hello eseményközpont hello névtérben hello nevű kezdetű **insights-**.</span><span class="sxs-lookup"><span data-stu-id="b7d3b-111">An event hub for a category of diagnostic logs is created automatically when you check hello option in hello portal or enable it through PowerShell, so you want tooselect hello event hub in hello namespace with hello name that starts with **insights-**.</span></span>
  * <span data-ttu-id="b7d3b-112">a következő SQL-kódot hello egy minta Stream Analytics lekérdezési használható tooparse összes hello naplóadatok tooa Power bi táblában:</span><span class="sxs-lookup"><span data-stu-id="b7d3b-112">hello following SQL code is a sample Stream Analytics query that you can use tooparse all hello log data in tooa PowerBI table:</span></span>

    ```sql
    SELECT
    records.ArrayValue.[Properties you want tootrack]
    INTO
    [OutputSourceName – hello PowerBI source]
    FROM
    [InputSourceName] AS e
    CROSS APPLY GetArrayElements(e.records) AS records
    ```

* <span data-ttu-id="b7d3b-113">**Egy egyéni telemetriai adatokat, illetve a naplózási platform** – Ha már rendelkezik egy egyedi telemetriai platform vagy a rendszer csak a gondolat egy kiválóan méretezhető hello közzétételi-feliratkozási épület jellegétől függően az Event Hubs lehetővé teszi a tooflexibly betöltési diagnosztikai naplózza.</span><span class="sxs-lookup"><span data-stu-id="b7d3b-113">**Build a custom telemetry and logging platform** – If you already have a custom-built telemetry platform or are just thinking about building one, hello highly scalable publish-subscribe nature of Event Hubs allows you tooflexibly ingest diagnostic logs.</span></span> <span data-ttu-id="b7d3b-114">[Lásd: Dan Rosanova útmutató toousing Event Hubs egy globális méretű telemetriai platform Itt](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/).</span><span class="sxs-lookup"><span data-stu-id="b7d3b-114">[See Dan Rosanova’s guide toousing Event Hubs in a global scale telemetry platform here](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/).</span></span>

## <a name="enable-streaming-of-diagnostic-logs"></a><span data-ttu-id="b7d3b-115">Adatfolyamként való küldése a diagnosztikai naplók engedélyezése</span><span class="sxs-lookup"><span data-stu-id="b7d3b-115">Enable streaming of diagnostic logs</span></span>
<span data-ttu-id="b7d3b-116">Adatfolyamként való küldése a diagnosztikai naplók programozott módon, hello portálon vagy hello segítségével engedélyezheti [Azure figyelő REST API-k](https://docs.microsoft.com/rest/api/monitor/servicediagnosticsettings).</span><span class="sxs-lookup"><span data-stu-id="b7d3b-116">You can enable streaming of diagnostic logs programmatically, via hello portal, or using hello [Azure Monitor REST APIs](https://docs.microsoft.com/rest/api/monitor/servicediagnosticsettings).</span></span> <span data-ttu-id="b7d3b-117">Mindkét módszer esetén diagnosztikai beállítás létrehozása az Event Hubs névtér és a hello napló kategóriák és a metrikák azt szeretné, hogy toosend toohello névtér megadása.</span><span class="sxs-lookup"><span data-stu-id="b7d3b-117">Either way, you create a diagnostic setting in which you specify an Event Hubs namespace and hello log categories and metrics you want toosend in toohello namespace.</span></span> <span data-ttu-id="b7d3b-118">Az eseményközpontok engedélyezi napló kategóriákhoz tartozó hello névtér jön létre.</span><span class="sxs-lookup"><span data-stu-id="b7d3b-118">An event hub is created in hello namespace for each log category you enable.</span></span> <span data-ttu-id="b7d3b-119">A diagnosztika **napló kategória** a naplóban, amely egy erőforrás gyűjthet típusa.</span><span class="sxs-lookup"><span data-stu-id="b7d3b-119">A diagnostic **log category** is a type of log that a resource may collect.</span></span>

> [!WARNING]
> <span data-ttu-id="b7d3b-120">Engedélyezése, valamint a folyamatos átviteli a számítási erőforrások (például a virtuális gépek vagy a Service Fabric) diagnosztikai naplók [meg kell adni egy másik lépések](../event-hubs/event-hubs-streaming-azure-diags-data.md).</span><span class="sxs-lookup"><span data-stu-id="b7d3b-120">Enabling and streaming diagnostic logs from Compute resources (for example, VMs or Service Fabric) [requires a different set of steps](../event-hubs/event-hubs-streaming-azure-diags-data.md).</span></span>
> 
> 

<span data-ttu-id="b7d3b-121">a Service Bus hello vagy az Event Hubs névtér nincs toobe hello naplók kibocsátó mindaddig, amíg hello beállítás konfiguráló hello felhasználó rendelkezik-e megfelelő hozzáférési tooboth előfizetéseket a Szerepalapú hello erőforrás ugyanahhoz az előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="b7d3b-121">hello Service Bus or Event Hubs namespace does not have toobe in hello same subscription as hello resource emitting logs as long as hello user who configures hello setting has appropriate RBAC access tooboth subscriptions.</span></span>

## <a name="stream-diagnostic-logs-using-hello-portal"></a><span data-ttu-id="b7d3b-122">Az adatfolyam diagnosztikai naplók hello portál használatával</span><span class="sxs-lookup"><span data-stu-id="b7d3b-122">Stream diagnostic logs using hello portal</span></span>
1. <span data-ttu-id="b7d3b-123">Hello portálon keresse meg a figyelő tooAzure, majd kattintson a **diagnosztikai beállítások**</span><span class="sxs-lookup"><span data-stu-id="b7d3b-123">In hello portal, navigate tooAzure Monitor and click on **Diagnostic Settings**</span></span>

    ![Figyelés szakaszban Azure-figyelő](media/monitoring-stream-diagnostic-logs-to-event-hubs/diagnostic-settings-blade.png)

2. <span data-ttu-id="b7d3b-125">Opcionálisan hello szűrőlistával erőforráscsoport és erőforrások típus szerint, majd a hello erőforrás diagnosztikai beállításának tooset milyen, amelynek.</span><span class="sxs-lookup"><span data-stu-id="b7d3b-125">Optionally filter hello list by resource group or resource type, then click on hello resource for which you would like tooset a diagnostic setting.</span></span>

3. <span data-ttu-id="b7d3b-126">Ha a beállítások nem található a kiválasztott hello erőforrás, felszólító toocreate beállítás áll.</span><span class="sxs-lookup"><span data-stu-id="b7d3b-126">If no settings exist on hello resource you have selected, you are prompted toocreate a setting.</span></span> <span data-ttu-id="b7d3b-127">Kattintson a "Diagnosztika bekapcsolásához."</span><span class="sxs-lookup"><span data-stu-id="b7d3b-127">Click "Turn on diagnostics."</span></span>

   ![Diagnosztikai beállítás - nincsenek meglévő beállítások hozzáadása](media/monitoring-stream-diagnostic-logs-to-event-hubs/diagnostic-settings-none.png)

   <span data-ttu-id="b7d3b-129">Ha meglévő beállítások hello erőforráson, látni fogja a már ehhez az erőforráshoz konfigurált beállítások listáját.</span><span class="sxs-lookup"><span data-stu-id="b7d3b-129">If there are existing settings on hello resource, you will see a list of settings already configured on this resource.</span></span> <span data-ttu-id="b7d3b-130">A "Hozzáadás diagnosztikai beállításának."</span><span class="sxs-lookup"><span data-stu-id="b7d3b-130">Click "Add diagnostic setting."</span></span>

   ![Diagnosztikai beállítás - meglévő beállítások hozzáadása](media/monitoring-stream-diagnostic-logs-to-event-hubs/diagnostic-settings-multiple.png)

3. <span data-ttu-id="b7d3b-132">Adjon a nevének beállítása és a hello jelölőnégyzetet **adatfolyam tooan eseményközpont**, majd válassza ki az Event Hubs névteret.</span><span class="sxs-lookup"><span data-stu-id="b7d3b-132">Give your setting a name and check hello box for **Stream tooan event hub**, then select an Event Hubs namespace.</span></span>
   
   ![Diagnosztikai beállítás - meglévő beállítások hozzáadása](media/monitoring-stream-diagnostic-logs-to-event-hubs/diagnostic-settings-configure.png)
    
   <span data-ttu-id="b7d3b-134">hello kijelölt névtér lesz hello eseményközpont létrehozása (Ha a folyamatos átviteli diagnosztikai naplók első alkalommal) vagy folyamatos átviteli túl rendelkező (ha van már erőforrásokat, amelyek a napló kategória toothis névtér vannak streaming), és hello házirend hello határozza meg engedélyek, amelyek hello adatfolyam mechanizmussal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="b7d3b-134">hello namespace selected will be where hello event hub is created (if this is your first time streaming diagnostic logs) or streamed too(if there are already resources that are streaming that log category toothis namespace), and hello policy defines hello permissions that hello streaming mechanism has.</span></span> <span data-ttu-id="b7d3b-135">Ma, streaming tooan eseményközpont kezelése, a Küldés és a figyelés engedély szükséges.</span><span class="sxs-lookup"><span data-stu-id="b7d3b-135">Today, streaming tooan event hub requires Manage, Send, and Listen permissions.</span></span> <span data-ttu-id="b7d3b-136">Hozzon létre, vagy az Event Hubs névtér megosztott hozzáférési házirendek hello portálon hello konfigurálása lapon módosítsa a névtér.</span><span class="sxs-lookup"><span data-stu-id="b7d3b-136">You can create or modify Event Hubs namespace shared access policies in hello portal under hello Configure tab for your namespace.</span></span> <span data-ttu-id="b7d3b-137">Ezek a diagnosztikai beállítások egyikét tooupdate, hello ügyfél engedéllyel kell rendelkeznie hello ListKey a hello Event Hubs engedélyezési szabályt.</span><span class="sxs-lookup"><span data-stu-id="b7d3b-137">tooupdate one of these diagnostic settings, hello client must have hello ListKey permission on hello Event Hubs authorization rule.</span></span>

4. <span data-ttu-id="b7d3b-138">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="b7d3b-138">Click **Save**.</span></span>

<span data-ttu-id="b7d3b-139">Néhány másodpercen belül hello új beállítás jelenik meg az ehhez az erőforráshoz beállítások listáját, és diagnosztikai naplók átvitt toothat tárfiókot, amint létrejön az új esemény-adatokat.</span><span class="sxs-lookup"><span data-stu-id="b7d3b-139">After a few moments, hello new setting appears in your list of settings for this resource, and diagnostic logs are streamed toothat storage account as soon as new event data is generated.</span></span>

### <a name="via-powershell-cmdlets"></a><span data-ttu-id="b7d3b-140">PowerShell-parancsmagok használatával</span><span class="sxs-lookup"><span data-stu-id="b7d3b-140">Via PowerShell Cmdlets</span></span>
<span data-ttu-id="b7d3b-141">hello keresztül streaming tooenable [Azure PowerShell-parancsmagok](insights-powershell-samples.md), használhatja a hello `Set-AzureRmDiagnosticSetting` parancsmag ezekkel a paraméterekkel:</span><span class="sxs-lookup"><span data-stu-id="b7d3b-141">tooenable streaming via hello [Azure PowerShell Cmdlets](insights-powershell-samples.md), you can use hello `Set-AzureRmDiagnosticSetting` cmdlet with these parameters:</span></span>

```powershell
Set-AzureRmDiagnosticSetting -ResourceId [your resource ID] -ServiceBusRuleId [your Service Bus rule ID] -Enabled $true
```

<span data-ttu-id="b7d3b-142">Service Bus Szabályazonosító hello egy ilyen formátumú karakterláncot: `{Service Bus resource ID}/authorizationrules/{key name}`, például `/subscriptions/{subscription ID}/resourceGroups/Default-ServiceBus-WestUS/providers/Microsoft.ServiceBus/namespaces/{Service Bus namespace}/authorizationrules/RootManageSharedAccessKey`.</span><span class="sxs-lookup"><span data-stu-id="b7d3b-142">hello Service Bus Rule ID is a string with this format: `{Service Bus resource ID}/authorizationrules/{key name}`, for example, `/subscriptions/{subscription ID}/resourceGroups/Default-ServiceBus-WestUS/providers/Microsoft.ServiceBus/namespaces/{Service Bus namespace}/authorizationrules/RootManageSharedAccessKey`.</span></span>

### <a name="via-azure-cli"></a><span data-ttu-id="b7d3b-143">Az Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="b7d3b-143">Via Azure CLI</span></span>
<span data-ttu-id="b7d3b-144">adatfolyam-keresztül hello tooenable [Azure CLI](insights-cli-samples.md), használhatja a hello `insights diagnostic set` ehhez hasonló parancsot:</span><span class="sxs-lookup"><span data-stu-id="b7d3b-144">tooenable streaming via hello [Azure CLI](insights-cli-samples.md), you can use hello `insights diagnostic set` command like this:</span></span>

```azurecli
azure insights diagnostic set --resourceId <resourceID> --serviceBusRuleId <serviceBusRuleID> --enabled true
```

<span data-ttu-id="b7d3b-145">Hello formátuma azonos a Service Bus Szabályazonosító a PowerShell-parancsmag hello leírtak szerint használja.</span><span class="sxs-lookup"><span data-stu-id="b7d3b-145">Use hello same format for Service Bus Rule ID as explained for hello PowerShell Cmdlet.</span></span>

## <a name="how-do-i-consume-hello-log-data-from-event-hubs"></a><span data-ttu-id="b7d3b-146">Hogyan használnak a hello adatainak naplózása az Event Hubs?</span><span class="sxs-lookup"><span data-stu-id="b7d3b-146">How do I consume hello log data from Event Hubs?</span></span>
<span data-ttu-id="b7d3b-147">Minta kimenet Eseményközpontokból származó adatokat a következő:</span><span class="sxs-lookup"><span data-stu-id="b7d3b-147">Here is sample output data from Event Hubs:</span></span>

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

| <span data-ttu-id="b7d3b-148">Elem neve</span><span class="sxs-lookup"><span data-stu-id="b7d3b-148">Element Name</span></span> | <span data-ttu-id="b7d3b-149">Leírás</span><span class="sxs-lookup"><span data-stu-id="b7d3b-149">Description</span></span> |
| --- | --- |
| <span data-ttu-id="b7d3b-150">rekordok</span><span class="sxs-lookup"><span data-stu-id="b7d3b-150">records</span></span> |<span data-ttu-id="b7d3b-151">Ezek a hasznos adatok az összes naplóeseményeket tömbjét.</span><span class="sxs-lookup"><span data-stu-id="b7d3b-151">An array of all log events in this payload.</span></span> |
| <span data-ttu-id="b7d3b-152">time</span><span class="sxs-lookup"><span data-stu-id="b7d3b-152">time</span></span> |<span data-ttu-id="b7d3b-153">Az idő, amellyel hello esemény történt.</span><span class="sxs-lookup"><span data-stu-id="b7d3b-153">Time at which hello event occurred.</span></span> |
| <span data-ttu-id="b7d3b-154">category</span><span class="sxs-lookup"><span data-stu-id="b7d3b-154">category</span></span> |<span data-ttu-id="b7d3b-155">Ez az esemény napló kategóriát.</span><span class="sxs-lookup"><span data-stu-id="b7d3b-155">Log category for this event.</span></span> |
| <span data-ttu-id="b7d3b-156">resourceId</span><span class="sxs-lookup"><span data-stu-id="b7d3b-156">resourceId</span></span> |<span data-ttu-id="b7d3b-157">Ezt az eseményt létrehozó hello erőforrás erőforrás-azonosító.</span><span class="sxs-lookup"><span data-stu-id="b7d3b-157">Resource ID of hello resource that generated this event.</span></span> |
| <span data-ttu-id="b7d3b-158">operationName</span><span class="sxs-lookup"><span data-stu-id="b7d3b-158">operationName</span></span> |<span data-ttu-id="b7d3b-159">Hello művelet neve.</span><span class="sxs-lookup"><span data-stu-id="b7d3b-159">Name of hello operation.</span></span> |
| <span data-ttu-id="b7d3b-160">szint</span><span class="sxs-lookup"><span data-stu-id="b7d3b-160">level</span></span> |<span data-ttu-id="b7d3b-161">Választható.</span><span class="sxs-lookup"><span data-stu-id="b7d3b-161">Optional.</span></span> <span data-ttu-id="b7d3b-162">Azt jelzi, hogy hello naplózási esemény szintet.</span><span class="sxs-lookup"><span data-stu-id="b7d3b-162">Indicates hello log event level.</span></span> |
| <span data-ttu-id="b7d3b-163">properties</span><span class="sxs-lookup"><span data-stu-id="b7d3b-163">properties</span></span> |<span data-ttu-id="b7d3b-164">Hello esemény tulajdonságai.</span><span class="sxs-lookup"><span data-stu-id="b7d3b-164">Properties of hello event.</span></span> |

<span data-ttu-id="b7d3b-165">Megtekintheti az összes erőforrás-szolgáltató, amely támogatja a folyamatos átviteli tooEvent hubok listáját [Itt](monitoring-overview-of-diagnostic-logs.md).</span><span class="sxs-lookup"><span data-stu-id="b7d3b-165">You can view a list of all resource providers that support streaming tooEvent Hubs [here](monitoring-overview-of-diagnostic-logs.md).</span></span>

## <a name="stream-data-from-compute-resources"></a><span data-ttu-id="b7d3b-166">Az adatfolyam adatok a számítási erőforrásokat</span><span class="sxs-lookup"><span data-stu-id="b7d3b-166">Stream data from Compute resources</span></span>
<span data-ttu-id="b7d3b-167">Adatfolyam formájában a diagnosztikai naplókat a számítási erőforrásokat hello Windows Azure diagnosztikai ügynök használatával is.</span><span class="sxs-lookup"><span data-stu-id="b7d3b-167">You can also stream diagnostic logs from Compute resources using hello Windows Azure Diagnostics agent.</span></span> <span data-ttu-id="b7d3b-168">[Ebben a cikkben találhat](../event-hubs/event-hubs-streaming-azure-diags-data.md) arról, hogyan tooset, hogy fel.</span><span class="sxs-lookup"><span data-stu-id="b7d3b-168">[See this article](../event-hubs/event-hubs-streaming-azure-diags-data.md) for how tooset that up.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b7d3b-169">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b7d3b-169">Next steps</span></span>
* [<span data-ttu-id="b7d3b-170">További tudnivalók az Azure diagnosztikai naplók</span><span class="sxs-lookup"><span data-stu-id="b7d3b-170">Read more about Azure Diagnostic Logs</span></span>](monitoring-overview-of-diagnostic-logs.md)
* [<span data-ttu-id="b7d3b-171">Bevezetés az Event Hubs használatába</span><span class="sxs-lookup"><span data-stu-id="b7d3b-171">Get started with Event Hubs</span></span>](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)

