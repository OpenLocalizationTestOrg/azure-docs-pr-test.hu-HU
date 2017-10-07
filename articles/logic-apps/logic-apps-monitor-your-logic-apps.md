---
title: "aaaCheck állapotát, a naplózás beállítása és értesítéskérés - Azure Logic Apps |} Microsoft Docs"
description: "A figyelő állapotát és teljesítményét a logic apps diagnosztikai adatok naplózása, és értesítések beállítása"
author: jeffhollan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 5c1b1e15-3b6c-49dc-98a6-bdbe7cb75339
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 07/21/2017
ms.author: LADocs; jehollan
ms.openlocfilehash: 81f186e11a669b710f4c06089597eb5a76f7a44e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-status-set-up-diagnostics-logging-and-turn-on-alerts-for-azure-logic-apps"></a>Állapotának figyelésére, diagnosztikai naplózás beállítása és az Azure Logic Apps riasztás bekapcsolása

Miután [létrehozása és futtatása a logikai alkalmazás](../logic-apps/logic-apps-create-a-logic-app.md), ellenőrizheti a futtatása előzmények, eseményindító előzmények, állapotát és teljesítményét. A valós idejű esemény figyelése és részletesebb hibakeresés, állítson be [diagnosztikai naplózás](#azure-diagnostics) a logikai alkalmazásnak. Ily módon is [található, és tekintse meg az eseményeket](#find-events), például az eseményindító események, futtatási események és műveleti események. Emellett ezzel [diagnosztikai adatok más szolgáltatásokkal](#extend-diagnostic-data), például az Azure Storage és az Azure Event Hubs. 

hibák vagy egyéb lehetséges problémákat tooget értesítések beállítása [riasztások](#add-azure-alerts). Például létrehozhat egy riasztást, amelyek észlelik a ""több mint öt nem egy óra alatt. Is beállíthat figyelését, figyelemmel kíséri, és a naplózást programozott módon segítségével [Azure Diagnostics esemény beállításai és a Tulajdonságok](#diagnostic-event-properties).

## <a name="view-runs-and-trigger-history-for-your-logic-app"></a>Nézet futtatása és a Logic Apps alkalmazást eseményindító előzményei

1. toofind a Logic Apps alkalmazást a hello [Azure-portálon](https://portal.azure.com), a hello Azure főmenü, majd a **további szolgáltatások**. Hello a keresőmezőbe a "logic apps" található, és válassza a **a Logic apps**.

   ![A logikai alkalmazás keresése](./media/logic-apps-monitor-your-logic-apps/find-your-logic-app.png)

   hello Azure-portálon az Azure-előfizetéshez társított összes hello logikai alkalmazások jeleníti meg. 

2. Válassza ki a Logic Apps alkalmazást, majd válassza a **áttekintése**.

   hello Azure-portálon hello futtatása előzmények és a Logic Apps alkalmazást eseményindító előzményeit jeleníti meg. Példa:

   ![Logikai alkalmazás fut előzmények és eseményindító előzményei](media/logic-apps-monitor-your-logic-apps/overview.png)

   * **Futtatja az előzmények** jeleníti meg a Logic Apps alkalmazást minden hello tartoznak futtatások. 
   * **Indítás, előzmények** a Logic Apps alkalmazást minden hello eseményindító tevékenység műveleteit jeleníti meg.

   A leírások, lásd: [hibaelhárítása a Logic Apps alkalmazást](../logic-apps/logic-apps-diagnosing-failures.md).

   > [!TIP]
   > Ha nem találja a hello adatokat várt, hello eszköztáron válassza a **frissítése**.

3. egy adott futtatható, a lépések tooview hello **előzmények fut**, válassza ki a futtató. 

   hello figyelő nézet futtató egyes lépéseit mutatja. Példa:

   ![A megadott futtató műveletek](media/logic-apps-monitor-your-logic-apps/monitor-view-updated.png)

4. tooget részletesebben hello futtatja, válasszon **futtatása részletek**. Ezek az információk összesíti hello lépéseket, állapot, bemeneti adatokat, és kimeneti hello futtassa. 

   ![Válassza a "Futtatás részletei"](media/logic-apps-monitor-your-logic-apps/run-details.png)

   Például, hogy megkaphassa hello futtassa a **korrelációs azonosító**, amely szükség lehet a hello használatakor [REST API-t a Logic Apps](https://docs.microsoft.com/rest/api/logic).

5. egy adott lépésre tooget adatait válassza ki a lépés. Most adatait, például bemenetek, kimenetek és hibáit, és ismételje meg a lépés a tekintheti meg. Példa:

   ![Lépés részletei](media/logic-apps-monitor-your-logic-apps/monitor-view-details.png)
   
   > [!NOTE]
   > Az összes futásidejű adatokat és eseményeket hello Logic Apps szolgáltatás belül vannak titkosítva. Akkor lesznek visszafejtve, csak akkor, ha egy felhasználó kéri tooview adatokat. Azt is meghatározhatja, hozzáférési toothese eseményeket [átruházásához hozzáférés-vezérlés (RBAC)](../active-directory/role-based-access-control-what-is.md).

6. egy adott eseményindító esemény tooget adatait vissza toohello **áttekintése** ablaktáblán. A **előzmények indítás**, jelölje be hello indítási esemény. Most már megtekintheti adatait, például bemenetekhez és kimenetekhez, például:

   ![Indítási esemény kimeneti részletei](media/logic-apps-monitor-your-logic-apps/trigger-details.png)

<a name="azure-diagnostics"></a>

## <a name="turn-on-diagnostics-logging-for-your-logic-app"></a>Diagnosztika a logikai alkalmazás naplózásának engedélyezése

Gazdagabb feltárására futásidejű adatokat és eseményeket, állíthatja be a diagnosztikai naplózás [Azure Naplóelemzés](../log-analytics/log-analytics-overview.md). A Naplóelemzési rendszer szolgáltatása [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md) , amely figyeli a felhőben és a helyszíni környezetben toohelp azok rendelkezésre állását és teljesítményét karbantartása. 

Mielőtt elkezdené, toohave OMS-munkaterület szüksége. Ismerje meg, [hogyan toocreate OMS-munkaterület](../log-analytics/log-analytics-get-started.md).

1. A hello [Azure-portálon](https://portal.azure.com), keresse meg és jelölje meg a Logic Apps alkalmazást. 

2. Hello logic app panel menü alatti **figyelés**, válassza ki **diagnosztika** > **diagnosztikai beállítások**.

   ![Nyissa meg tooMonitoring, diagnosztikát, diagnosztikai beállítások](media/logic-apps-monitor-your-logic-apps/logic-app-diagnostics.png)

3. A **diagnosztikai beállítások**, válassza a **a**.

   ![Kapcsolja be a diagnosztikai naplók](media/logic-apps-monitor-your-logic-apps/turn-on-diagnostics-logic-app.png)

4. Most válasszon hello OMS munkaterületet, és az esemény naplózási kategóriát látható módon:

   1. Válassza ki **tooLog Analytics küldése**. 
   2. A **Naplóelemzési**, válassza a **konfigurálása**. 
   3. A **OMS-munkaterület**, jelölje be az OMS-munkaterület toouse hello a naplózás.
   4. A **napló**, jelölje be hello **WorkflowRuntime** kategóriát.
   5. Válassza ki a hello metrika időköz.
   6. Amikor elkészült, válassza ki a **mentése**.

   ![Válassza ki az OMS-munkaterület és a naplózási adatokat](media/logic-apps-monitor-your-logic-apps/send-diagnostics-data-log-analytics-workspace.png)

Most található események és egyéb adatok eseményindító események, eseményeket és a műveleti események futtatják.

<a name="find-events"></a>

## <a name="find-events-and-data-for-your-logic-app"></a>A Logic Apps alkalmazást talált események és adatok

a Logic Apps alkalmazást, eseményindító, futtassa az eseményeket, és műveleti események, például toofind és nézet események kövesse az alábbi lépéseket.

1. A hello [Azure-portálon](https://portal.azure.com), válassza a **több szolgáltatások**. Keresse meg a "naplóelemzési", és válassza a **Naplóelemzési** itt látható módon:

   ![Válassza ki a "Naplóelemzési"](media/logic-apps-monitor-your-logic-apps/browseloganalytics.png)

2. A **Naplóelemzési**, található, és válassza ki az OMS-munkaterület. 

   ![Az OMS-munkaterület kiválasztása](media/logic-apps-monitor-your-logic-apps/selectla.png)

3. A **felügyeleti**, válassza a **OMS-portálon**.

   ![Válassza ki a "OMS-portálon"](media/logic-apps-monitor-your-logic-apps/omsportalpage.png)

4. Az OMS kezdőlapján válassza **naplófájl-keresési**.

   ![Az OMS kezdőlapján válassza a "Naplófájl-keresési"](media/logic-apps-monitor-your-logic-apps/logsearch.png)

   – vagy –

   ![A hello OMS menüben válassza a "Naplófájl-keresési"](media/logic-apps-monitor-your-logic-apps/logsearch-2.png)

5. Hello keresési mezőbe, adjon meg egy mező, amelyet az toofind, és nyomja le az ENTER **Enter**. Amikor elkezdi beírni, OMS megjeleníti a lehetséges találatok és műveletek közül választhat. 

   Például toofind hello első 10 eseményeket, amelyek történtek, adja meg, és válassza ki a keresési lekérdezés: **kategória = WorkflowRuntime |} felső 10**

   ![Adja meg a keresési karakterláncot](media/logic-apps-monitor-your-logic-apps/oms-start-query.png)

   További információ [hogyan Naplóelemzési toofind adatok](../log-analytics/log-analytics-log-searches.md).

6. A hello eredmények lapon hello bal oldali sávon, válassza a hello időkeretet, amelyet az tooview.
toorefine adja hozzá egy szűrőt, a lekérdezés válasszon **+ Hozzáadás**.

   ![Válassza ki a lekérdezési eredmények időkeretre](media/logic-apps-monitor-your-logic-apps/query-results.png)

7. A **szűrők hozzáadása**, adja meg a hello szűrő nevét, így kívánt hello szűrő található. Válassza ki a hello szűrőt, és válassza a **+ Hozzáadás**.

   Ebben a példában használt hello word "állapot" toofind sikertelen volt az események **AzureDiagnostics**.
   Itt hello szűrőt **status_s** már be van jelölve.

   ![Válassza ki a szűrő](media/logic-apps-monitor-your-logic-apps/log-search-add-filter.png)

8. Hello bal oldali sávon, adja meg a hello szűrő értéket, hogy szeretné, hogy toouse, és válassza **alkalmaz**.

   ![Adja meg a szűrő értéket, válassza az "Alkalmaz"](media/logic-apps-monitor-your-logic-apps/log-search-apply-filter.png)

9. Térjen vissza van felépítése toohello lekérdezést. A lekérdezés frissül a kijelölt szűrő és az értéke. Az előző eredmények most túl szűrve.

   ![Térjen vissza a szűrt eredményekkel tooyour lekérdezés](media/logic-apps-monitor-your-logic-apps/log-search-query-filtered-results.png)

10. toosave későbbi használatra a lekérdezés válasszon **mentése**. Ismerje meg, [hogyan toosave a lekérdezés](../logic-apps/logic-apps-track-b2b-messages-omsportal-query-filter-control-number.md#save-oms-query).

<a name="extend-diagnostic-data"></a>

## <a name="extend-how-and-where-you-use-diagnostic-data-with-other-services"></a>Hogyan és hol diagnosztikai adatok használja más szolgáltatásokkal

Azure Naplóelemzés, valamint bővítheti, hogyan használhatja a Logic Apps alkalmazást diagnosztikai adatok más Azure-szolgáltatásokkal, például: 

* [Az Azure diagnosztikai naplók az Azure Storage archív](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md)
* [Az adatfolyam Azure diagnosztikai naplók tooAzure Event Hubs](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md) 

Ezután a get valós idejű figyelés telemetriai adatok és más szolgáltatások analytics segítségével például [Azure Stream Analytics](../stream-analytics/stream-analytics-introduction.md) és [Power BI](../log-analytics/log-analytics-powerbi.md). Példa:

* [Az Event Hubs tooStream Analytics adatfolyam adatait](../stream-analytics/stream-analytics-define-inputs.md)
* [A Stream Analytics adatfolyam-továbbítási adatok elemzése és a Power bi-ban a valós idejű elemzési irányítópult létrehozása](../stream-analytics/stream-analytics-power-bi-dashboard.md)

Alapján hello beállítása kívánt beállításokat, győződjön meg arról, hogy Ön első [az Azure storage-fiók létrehozása](../storage/common/storage-create-storage-account.md) vagy [hozzon létre egy Azure event hubs](../event-hubs/event-hubs-create.md). Ezután válasszon hello beállítások ahová toosend diagnosztikai adatokat:

![TooAzure tárolási fiók vagy esemény hub adatok küldése](./media/logic-apps-monitor-your-logic-apps/storage-account-event-hubs.png)

> [!NOTE]
> Megőrzési időtartamú csak válasszon toouse tárfiókot vonatkozik.

<a name="add-azure-alerts"></a>

## <a name="set-up-alerts-for-your-logic-app"></a>A logikai alkalmazás beállítása

adott mérőszámok toomonitor vagy a Logic Apps alkalmazást küszöbértékek túllépése beállítása [értesítések az Azure-ban](../monitoring-and-diagnostics/monitoring-overview-alerts.md). További tudnivalók [az Azure-ban mérőszámok](../monitoring-and-diagnostics/monitoring-overview-metrics.md). 

figyelmeztetéseket nélkül tooset [Azure Naplóelemzés](../log-analytics/log-analytics-overview.md), kövesse az alábbi lépéseket. A riasztások feltételek és a műveletek, speciális [Naplóelemzési beállítása](#azure-diagnostics) túl.

1. Hello logic app panel menü alatti **figyelés**, válassza ki **diagnosztika** > **riasztási szabályok** > **riasztáshozzáadása**itt látható módon:

   ![A Logic Apps alkalmazást értesítések hozzáadása](media/logic-apps-monitor-your-logic-apps/set-up-alerts.png)

2. A hello **riasztási szabály felvétele** panelen, a riasztás létrehozása, látható módon:

   1. A **erőforrás**, válassza ki a logikai alkalmazás, ha nincsenek használatban. 
   2. Adjon meg egy nevének és leírásának megadása a riasztás.
   3. Válassza ki a **metrika** vagy tootrack kívánt eseményt.
   4. Válassza ki a **feltétel**, adja meg egy **küszöbérték** hello metrika, és jelölje be hello **időszak** Ez a metrika figyelésre.
   5. Adja meg, hogy toosend levelezési hello riasztás. 
   6. Adjon meg más e-mail címek hello értesítés küldéséhez. 
   A webhook URL-CÍMÉT, ha azt szeretné, hogy toosend hello riasztás is megadható.

   Például ez a szabály küld riasztást, amikor öt vagy több futtatása sikertelen lesz, egy óra alatt:

   ![Metrika riasztási szabály létrehozása](media/logic-apps-monitor-your-logic-apps/create-alert-rule.png)

> [!TIP]
> logikai alkalmazás toorun riasztásból, megadhat hello [kérelem eseményindító](../connectors/connectors-native-reqres.md) a munkafolyamat, amellyel feladatokhoz a példák mint:
> 
> * [POST tooSlack](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app)
> * [A szöveg küldése](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app)
> * [Egy üzenetsor tooa hozzáadása](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app)

<a name="diagnostic-event-properties"></a>

## <a name="azure-diagnostics-event-settings-and-details"></a>Az Azure Diagnostics esemény beállításai és részletek

Minden egyes diagnosztikai esemény részleteit a Logic Apps alkalmazást és az esemény, például hello állapotát, a start idő, befejezési idő, és így tovább. figyelés, a nyomon követési és a naplózás beállítása tooprogrammatically, szervezeti egység használható ezen adatok hello [REST API-t az Azure Logic Apps](https://docs.microsoft.com/rest/api/logic) és hello [REST API-t az Azure Diagnostics](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftlogicworkflows).

Például hello `ActionCompleted` eseményhez tartozik hello `clientTrackingId` és `trackedProperties` nyomon követése és figyelésére használható tulajdonságokról:

``` json
{
    "time": "2016-07-09T17:09:54.4773148Z",
    "workflowId": "/SUBSCRIPTIONS/<subscription-ID>/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/MYLOGICAPP",
    "resourceId": "/SUBSCRIPTIONS/<subscription-ID>/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/MYLOGICAPP/RUNS/08587361146922712057/ACTIONS/HTTP",
    "category": "WorkflowRuntime",
    "level": "Information",
    "operationName": "Microsoft.Logic/workflows/workflowActionCompleted",
    "properties": {
        "$schema": "2016-06-01",
        "startTime": "2016-07-09T17:09:53.4336305Z",
        "endTime": "2016-07-09T17:09:53.5430281Z",
        "status": "Succeeded",
        "code": "OK",
        "resource": {
            "subscriptionId": "<subscription-ID>",
            "resourceGroupName": "MyResourceGroup",
            "workflowId": "cff00d5458f944d5a766f2f9ad142553",
            "workflowName": "MyLogicApp",
            "runId": "08587361146922712057",
            "location": "westus",
            "actionName": "Http"
        },
        "correlation": {
            "actionTrackingId": "e1931543-906d-4d1d-baed-dee72ddf1047",
            "clientTrackingId": "<my-custom-tracking-ID>"
        },
        "trackedProperties": {
            "myTrackedProperty": "<value>"
        }
    }
}
```

* `clientTrackingId`: Ha nincs megadott Azure automatikusan létrehozza ezt az Azonosítót, és korrelálja események futtatása logikai alkalmazás között, beleértve a beágyazott munkafolyamatok, amelyek nevezzük hello logic App. Ez az azonosító egy úgy, hogy manuálisan megadhatja egy `x-ms-client-tracking-id` fejléc a következő hello eseményindító kérelem az egyéni azonosító értéke. A kérelem eseményindító, a HTTP-eseményindítóval, illetve a webhook eseményindító is használhatja.

* `trackedProperties`: tootrack bemeneti vagy kimeneti diagnosztikai adatok, adhat hozzá a nyomon követett tulajdonságok tooactions a Logic Apps alkalmazást JSON-definícióból. Csak egyetlen művelettel bemenetekhez és kimenetekhez nyomon követheti nyomon követett tulajdonságok, de használhat hello `correlation` események toocorrelate futtató műveletek között tulajdonságait.

  tootrack egy vagy több tulajdonságának hozzáadása hello `trackedProperties` toohello művelet definition kívánt szakasz és hello tulajdonságokat. Tegyük fel, hogy a telemetriai adatok tootrack adatok, például egy "order ID" kívánt:

  ``` json
  "myAction": {
    "type": "http",
    "inputs": {
        "uri": "http://uri",
        "headers": {
            "Content-Type": "application/json"
        },
        "body": "@triggerBody()"
    },
    "trackedProperties": {
        "myActionHTTPStatusCode": "@action()['outputs']['statusCode']",
        "myActionHTTPValue": "@action()['outputs']['body']['<content>']",
        "transactionId": "@action()['inputs']['body']['<content>']"
    }
  }
  ```

## <a name="next-steps"></a>Következő lépések

* [A logic app központi telepítési sablonok létrehozása és a kiadáskezelés](../logic-apps/logic-apps-create-deploy-template.md)
* [A vállalati integrációs csomag B2B forgatókönyvek](../logic-apps/logic-apps-enterprise-integration-overview.md)
* [B2B üzenetek megfigyelése](../logic-apps/logic-apps-monitor-b2b-message.md)