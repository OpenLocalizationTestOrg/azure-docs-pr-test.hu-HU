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
# <a name="stream-azure-diagnostic-logs-tooan-event-hubs-namespace"></a>Az adatfolyam Azure diagnosztikai naplók tooan Event Hubs Namespace
**[Az Azure diagnosztikai naplók](monitoring-overview-of-diagnostic-logs.md)**  továbbítható a közel valós idejű tooany alkalmazások hello beépített "TooEvent hubok exportálása" lehetőséggel a portál hello, vagy engedélyezésével hello a Service Bus Szabályazonosító egy diagnosztikai beállítás hello Azure PowerShell használatával -Parancsmagjaival vagy Azure CLI-t.

## <a name="what-you-can-do-with-diagnostics-logs-and-event-hubs"></a>Mi mindent diagnosztikai naplók és az Event Hubs
Az alábbiakban néhány módokon használhatja a funkció a diagnosztikai naplók streaming hello:

* **Stream naplók too3rd naplózása és telemetriai rendszerekre** – adott idő alatt, az Event Hubs streaming fog vált hello mechanizmus toopipe toothird gyártótól a siem-ektől a diagnosztikai naplók és elemzési megoldásokat jelentkezzen.
* **Szolgáltatás állapotának megtekintéséhez a "gyors path" adatok tooPowerBI folyamatos** – Event Hubs használatával, a Stream Analytics és a Power BI, könnyen alakíthatja át az Azure-szolgáltatások a toonear valós idejű elemzése a diagnosztikai adatokat. [A dokumentációs cikket tartalmaz hogyan tooset fel az Event Hubs feldolgozni az adatokat az Stream Analytics használ, és kimenetként használata a Power bi kiváló áttekintést nyújt](../stream-analytics/stream-analytics-power-bi-dashboard.md). Az alábbiakban néhány tippek a diagnosztikai naplók első beállítása:
  
  * Az eseményközpontok a diagnosztikai naplók kategóriájú automatikusan létrejön hello portálon hello beállítást, vagy engedélyezze a PowerShell segítségével, tehát tooselect hello eseményközpont hello névtérben hello nevű kezdetű **insights-**.
  * a következő SQL-kódot hello egy minta Stream Analytics lekérdezési használható tooparse összes hello naplóadatok tooa Power bi táblában:

    ```sql
    SELECT
    records.ArrayValue.[Properties you want tootrack]
    INTO
    [OutputSourceName – hello PowerBI source]
    FROM
    [InputSourceName] AS e
    CROSS APPLY GetArrayElements(e.records) AS records
    ```

* **Egy egyéni telemetriai adatokat, illetve a naplózási platform** – Ha már rendelkezik egy egyedi telemetriai platform vagy a rendszer csak a gondolat egy kiválóan méretezhető hello közzétételi-feliratkozási épület jellegétől függően az Event Hubs lehetővé teszi a tooflexibly betöltési diagnosztikai naplózza. [Lásd: Dan Rosanova útmutató toousing Event Hubs egy globális méretű telemetriai platform Itt](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/).

## <a name="enable-streaming-of-diagnostic-logs"></a>Adatfolyamként való küldése a diagnosztikai naplók engedélyezése
Adatfolyamként való küldése a diagnosztikai naplók programozott módon, hello portálon vagy hello segítségével engedélyezheti [Azure figyelő REST API-k](https://docs.microsoft.com/rest/api/monitor/servicediagnosticsettings). Mindkét módszer esetén diagnosztikai beállítás létrehozása az Event Hubs névtér és a hello napló kategóriák és a metrikák azt szeretné, hogy toosend toohello névtér megadása. Az eseményközpontok engedélyezi napló kategóriákhoz tartozó hello névtér jön létre. A diagnosztika **napló kategória** a naplóban, amely egy erőforrás gyűjthet típusa.

> [!WARNING]
> Engedélyezése, valamint a folyamatos átviteli a számítási erőforrások (például a virtuális gépek vagy a Service Fabric) diagnosztikai naplók [meg kell adni egy másik lépések](../event-hubs/event-hubs-streaming-azure-diags-data.md).
> 
> 

a Service Bus hello vagy az Event Hubs névtér nincs toobe hello naplók kibocsátó mindaddig, amíg hello beállítás konfiguráló hello felhasználó rendelkezik-e megfelelő hozzáférési tooboth előfizetéseket a Szerepalapú hello erőforrás ugyanahhoz az előfizetéshez.

## <a name="stream-diagnostic-logs-using-hello-portal"></a>Az adatfolyam diagnosztikai naplók hello portál használatával
1. Hello portálon keresse meg a figyelő tooAzure, majd kattintson a **diagnosztikai beállítások**

    ![Figyelés szakaszban Azure-figyelő](media/monitoring-stream-diagnostic-logs-to-event-hubs/diagnostic-settings-blade.png)

2. Opcionálisan hello szűrőlistával erőforráscsoport és erőforrások típus szerint, majd a hello erőforrás diagnosztikai beállításának tooset milyen, amelynek.

3. Ha a beállítások nem található a kiválasztott hello erőforrás, felszólító toocreate beállítás áll. Kattintson a "Diagnosztika bekapcsolásához."

   ![Diagnosztikai beállítás - nincsenek meglévő beállítások hozzáadása](media/monitoring-stream-diagnostic-logs-to-event-hubs/diagnostic-settings-none.png)

   Ha meglévő beállítások hello erőforráson, látni fogja a már ehhez az erőforráshoz konfigurált beállítások listáját. A "Hozzáadás diagnosztikai beállításának."

   ![Diagnosztikai beállítás - meglévő beállítások hozzáadása](media/monitoring-stream-diagnostic-logs-to-event-hubs/diagnostic-settings-multiple.png)

3. Adjon a nevének beállítása és a hello jelölőnégyzetet **adatfolyam tooan eseményközpont**, majd válassza ki az Event Hubs névteret.
   
   ![Diagnosztikai beállítás - meglévő beállítások hozzáadása](media/monitoring-stream-diagnostic-logs-to-event-hubs/diagnostic-settings-configure.png)
    
   hello kijelölt névtér lesz hello eseményközpont létrehozása (Ha a folyamatos átviteli diagnosztikai naplók első alkalommal) vagy folyamatos átviteli túl rendelkező (ha van már erőforrásokat, amelyek a napló kategória toothis névtér vannak streaming), és hello házirend hello határozza meg engedélyek, amelyek hello adatfolyam mechanizmussal rendelkezik. Ma, streaming tooan eseményközpont kezelése, a Küldés és a figyelés engedély szükséges. Hozzon létre, vagy az Event Hubs névtér megosztott hozzáférési házirendek hello portálon hello konfigurálása lapon módosítsa a névtér. Ezek a diagnosztikai beállítások egyikét tooupdate, hello ügyfél engedéllyel kell rendelkeznie hello ListKey a hello Event Hubs engedélyezési szabályt.

4. Kattintson a **Save** (Mentés) gombra.

Néhány másodpercen belül hello új beállítás jelenik meg az ehhez az erőforráshoz beállítások listáját, és diagnosztikai naplók átvitt toothat tárfiókot, amint létrejön az új esemény-adatokat.

### <a name="via-powershell-cmdlets"></a>PowerShell-parancsmagok használatával
hello keresztül streaming tooenable [Azure PowerShell-parancsmagok](insights-powershell-samples.md), használhatja a hello `Set-AzureRmDiagnosticSetting` parancsmag ezekkel a paraméterekkel:

```powershell
Set-AzureRmDiagnosticSetting -ResourceId [your resource ID] -ServiceBusRuleId [your Service Bus rule ID] -Enabled $true
```

Service Bus Szabályazonosító hello egy ilyen formátumú karakterláncot: `{Service Bus resource ID}/authorizationrules/{key name}`, például `/subscriptions/{subscription ID}/resourceGroups/Default-ServiceBus-WestUS/providers/Microsoft.ServiceBus/namespaces/{Service Bus namespace}/authorizationrules/RootManageSharedAccessKey`.

### <a name="via-azure-cli"></a>Az Azure parancssori felület használatával
adatfolyam-keresztül hello tooenable [Azure CLI](insights-cli-samples.md), használhatja a hello `insights diagnostic set` ehhez hasonló parancsot:

```azurecli
azure insights diagnostic set --resourceId <resourceID> --serviceBusRuleId <serviceBusRuleID> --enabled true
```

Hello formátuma azonos a Service Bus Szabályazonosító a PowerShell-parancsmag hello leírtak szerint használja.

## <a name="how-do-i-consume-hello-log-data-from-event-hubs"></a>Hogyan használnak a hello adatainak naplózása az Event Hubs?
Minta kimenet Eseményközpontokból származó adatokat a következő:

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

| Elem neve | Leírás |
| --- | --- |
| rekordok |Ezek a hasznos adatok az összes naplóeseményeket tömbjét. |
| time |Az idő, amellyel hello esemény történt. |
| category |Ez az esemény napló kategóriát. |
| resourceId |Ezt az eseményt létrehozó hello erőforrás erőforrás-azonosító. |
| operationName |Hello művelet neve. |
| szint |Választható. Azt jelzi, hogy hello naplózási esemény szintet. |
| properties |Hello esemény tulajdonságai. |

Megtekintheti az összes erőforrás-szolgáltató, amely támogatja a folyamatos átviteli tooEvent hubok listáját [Itt](monitoring-overview-of-diagnostic-logs.md).

## <a name="stream-data-from-compute-resources"></a>Az adatfolyam adatok a számítási erőforrásokat
Adatfolyam formájában a diagnosztikai naplókat a számítási erőforrásokat hello Windows Azure diagnosztikai ügynök használatával is. [Ebben a cikkben találhat](../event-hubs/event-hubs-streaming-azure-diags-data.md) arról, hogyan tooset, hogy fel.

## <a name="next-steps"></a>Következő lépések
* [További tudnivalók az Azure diagnosztikai naplók](monitoring-overview-of-diagnostic-logs.md)
* [Bevezetés az Event Hubs használatába](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)

