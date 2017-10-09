---
title: "aaaError & kivételkezelést - Azure Logic Apps |} Microsoft Docs"
description: "Hiba és az Azure Logic Apps kivételkezelő minták"
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: e50ab2f2-1fdc-4d2a-be40-995a6cc5a0d4
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 10/18/2016
ms.author: LADocs; jehollan
ms.openlocfilehash: 326a252310c8dfb154e583f91c9421675e448d1f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="handle-errors-and-exceptions-in-azure-logic-apps"></a>Hibákat és kivételeket az Azure Logic Apps alkalmazásokat kezeléséhez

Az Azure Logic Apps hatékony eszközöket biztosít, és minták toohelp, győződjön meg arról a Integrációk hatékony és rugalmas-hibákkal szemben. Minden integrációs architektúra hello dokumentációkat az, hogy tooappropriately leíró állásidőt jelent, vagy a függő rendszerekről kapcsolatos problémákat. Logic Apps remek hibák kezelése a kiváló felhasználói élmény, felkínálva hello eszközökkel tooact kivételeket és a hibák esetén a munkafolyamatokban.

## <a name="retry-policies"></a>Ismételje meg a házirendek

Újrapróbálkozási házirendje egy kivétel és hibakezelés legalapvetőbb típusú hello. Ha az eredeti kérelem túllépte az időkorlátot, vagy sikertelen volt (a kérelmet, amely egy 429 eredményez vagy 5XX típusú válasz érkezett), ez a házirend határozza meg, hogy újra kell-e a hello művelet. Alapértelmezés szerint minden további 4 alkalommal keresztül 20 másodperces időközönként próbálkozzon újra. Igen, ha hello első kérést kap egy `500 Internal Server Error` választ, 20 másodperc szünet hello munkafolyamat-motor és kísérletek hello kérelem újra. Ha az összes újrapróbálkozás után hello művelet még mindig egy kivétel vagy sikertelen volt, hello munkafolyamat továbbra is fennáll, és jelek hello művelet állapotának `Failed`.

Újrapróbálkozási házirendeket konfigurálhat a hello **bemenetek** művelet esetén. Például konfigurálása egy újrapróbálkozási házirend tootry 4 alkalommal akár 1 órás időközönként keresztül Teljes bemeneti tulajdonságai, lásd: [munkafolyamat-műveleteket és eseményindítók][retryPolicyMSDN].

```json
"retryPolicy" : {
      "type": "<type-of-retry-policy>",
      "interval": <retry-interval>,
      "count": <number-of-retry-attempts>
    }
```

Ha a HTTP-művelet tooretry 4 alkalommal kívánta, és várjon 10 percet egyes kísérletek közötti, használja a következő definícióját hello:

```json
"HTTP": 
{
    "inputs": {
        "method": "GET",
        "uri": "http://myAPIendpoint/api/action",
        "retryPolicy" : {
            "type": "fixed",
            "interval": "PT10M",
            "count": 4
        }
    },
    "runAfter": {},
    "type": "Http"
}
```

A támogatott szintaxis további információkért lásd: hello [újrapróbálkozási-házirend a szakasz a munkafolyamat-műveleteket és eseményindítók][retryPolicyMSDN].

## <a name="catch-failures-with-hello-runafter-property"></a>A tényleges hibákat is hello RunAfter tulajdonság

Minden logikai alkalmazás művelet kijelenti, hogy milyen műveleteket csak befejezése előtt hello művelet, például a munkafolyamat hello lépéseket rendezés. Hello művelet definícióban, ebben a rendezése az úgynevezett hello `runAfter` tulajdonság. Ez a tulajdonság olyan objektum, amely leírja, milyen műveleteket és a művelet állapotok hello művelet hajtható végre. Alapértelmezés szerint hozzáadva hello Logic App Designer minden művelet túl van beállítva`runAfter` hello előző lépést, ha hello előző lépésben `Succeeded`. Azonban testre szabhatja a érték toofire műveletek során előző műveleteit `Failed`, `Skipped`, vagy egy lehetséges ezeket az értékeket. Ha tooadd egy elem tooa Service Bus-témakörbe kijelölt egy adott művelet után `Insert_Row` meghiúsul, használhatja a következő hello `runAfter` konfiguráció:

```json
"Send_message": {
    "inputs": {
        "body": {
            "ContentData": "@{encodeBase64(body('Insert_Row'))}",
            "ContentType": "{ \"content-type\" : \"application/json\" }"
        },
        "host": {
            "api": {
                "runtimeUrl": "https://logic-apis-westus.azure-apim.net/apim/servicebus"
            },
            "connection": {
                "name": "@parameters('$connections')['servicebus']['connectionId']"
            }
        },
        "method": "post",
        "path": "/@{encodeURIComponent('failures')}/messages"
    },
    "runAfter": {
        "Insert_Row": [
            "Failed"
        ]
    }
}
```

Értesítés hello `runAfter` tulajdonság értéke toofire, ha hello `Insert_Row` művelet `Failed`. Ha hello művelet állapota toorun hello művelet `Succeeded`, `Failed`, vagy `Skipped`, a következő szintaxist használja:

```json
"runAfter": {
        "Insert_Row": [
            "Failed", "Succeeded", "Skipped"
        ]
    }
```

> [!TIP]
> Műveletek futtatásához, és sikeresen befejeződik, miután a fenti művelet sikertelen volt, fel van tüntetve `Succeeded`. A viselkedés azt jelenti, hogy azt a munkafolyamat sikeresen általános hibák hello futtassa saját magát a jelölés szerint `Succeeded`.

## <a name="scopes-and-results-tooevaluate-actions"></a>Hatókörök és az eredmények tooevaluate műveletek

Egyes műveletek után futtathatja hasonló toohow, is csoportosíthatja műveletek belül egy [hatókör](../logic-apps/logic-apps-loops-and-scopes.md), amely összekötőként műveletek logikai csoportosítása. Hatókörök hasznosak a logic app műveletek sorolására, mind a hatókör hello állapot összesített értékelések végrehajtásához. hello hatókör magát egy állapot kap, a hatókör összes művelet után. hello hatókör határozza meg a hello ugyanazok a feltételek egy Futtatás mint. Ha hello végső egy végrehajtási fiókirodai művelete `Failed` vagy `Aborted`, hello állapota `Failed`.

az összes hiba történt a hello hatókörén belül toofire bizonyos műveleteket, használhatja `runAfter` van megjelölve hatókörrel rendelkező `Failed`. Ha *bármely* hello hatókörében művelet sikertelen, a hatókör sikertelen megadható, hogy fut létrehozni egy egyetlen művelettel toocatch hibák.

### <a name="getting-hello-context-of-failures-with-results"></a>Hibák az eredményekkel beolvasásakor hello környezetben

Bár a hatókörből hibák elfogja akkor hasznos, érdemes lehet környezet toohelp, hogy tudomásul veszi, hogy pontosan mely műveletek nem sikerült, és az esetleges hibákat vagy a visszaadott eredményobjektumokban tárolt állapotkódok. Hello `@result()` munkafolyamat-funkció biztosít a hatókör összes művelet eredménye hello kapcsolatos környezetben.

`@result()`egy egyetlen paramétert, a hatókör neve időt vesz igénybe, és minden hello művelet eredményeinek hatókörön belüli tömbjét adja vissza. E művelet objektumok például ugyanaz, mint hello attribútumok hello `@actions()` objektum, például művelet kezdési ideje, a művelet befejezése, a művelet állapota, a művelet bemeneti, a művelet közötti összefüggést szemléltető azonosítók és a művelet kimenetében. bármely művelet, amelyet nem sikerült egy hatókörön belüli toosend környezetében, hogy könnyen összepárosíthassa egy `@result()` működik egy `runAfter`.

tooexecute művelet *minden* művelet hatókörben, amely `Failed`, eredmények tooactions szűrő hello tömbje, amely nem sikerült, akkor párosítható `@result()` rendelkező egy  **[szűrő tömb](../connectors/connectors-native-query.md)**  műveletet és egy  **[ForEach](../logic-apps/logic-apps-loops-and-scopes.md)**  hurok. Hello szűrt eredmény tömb igénybe vehet, és egy műveletet hello használata minden egyes hibához tartozó **ForEach** hurok. Íme egy példa, majd részletesen ismerteti, hello adott válasz törzsének bármely művelet, amelyet nem sikerült a HTTP POST kérést küldő hello hatókörén belül `My_Scope`.

```json
"Filter_array": {
    "inputs": {
        "from": "@result('My_Scope')",
        "where": "@equals(item()['status'], 'Failed')"
    },
    "runAfter": {
        "My_Scope": [
            "Failed"
        ]
    },
    "type": "Query"
},
"For_each": {
    "actions": {
        "Log_Exception": {
            "inputs": {
                "body": "@item()['outputs']['body']",
                "method": "POST",
                "headers": {
                    "x-failed-action-name": "@item()['name']",
                    "x-failed-tracking-id": "@item()['clientTrackingId']"
                },
                "uri": "http://requestb.in/"
            },
            "runAfter": {},
            "type": "Http"
        }
    },
    "foreach": "@body('Filter_array')",
    "runAfter": {
        "Filter_array": [
            "Succeeded"
        ]
    },
    "type": "Foreach"
}
```

A részletes forgatókönyv toodescribe, mi történik, a következő:

1. belül minden művelet eredményét tooget hello `My_Scope`, hello **szűrő tömb** művelet szűrők `@result('My_Scope')`.

2. feltétel hello **szűrő tömb** tetszőleges `@result()` állapota egyenlő túl elem`Failed`. Ez a feltétel az összes művelet eredményeinek hello tömb szűrők `My_Scope` csak tooan tömb nem sikerült a művelet eredménye.

3. Hajtsa végre a **minden** hello művelete **szűrt tömb** kimenete. Ezt a lépést végrehajt egy műveletet *minden* korábban szűrt művelet eredménye nem sikerült.

    Ha hello hatókörében egyetlen művelet sikertelen volt, a hello műveletek hello `foreach` csak egyszer futnak le. 
    Sok sikertelen műveletek miatt egy művelet hibája /.

4. Küldjön egy HTTP POST hello `foreach` adott válasz törzsének, elem vagy `@item()['outputs']['body']`. Hello `@result()` elem alakzat van hello ugyanaz, mint hello `@actions()` alakul, és a program értelmezni tudja hello azonos módon.

5. Közé tartoznak a két egyéni fejlécek hello sikertelen művelet nevű `@item()['name']` hello sikertelen volt, mert a nyomkövetési azonosító futtatási ügyfél `@item()['clientTrackingId']`.

Referenciaként Íme egy példa `@result()` elemet ábrázoló hello `name`, `body`, és `clientTrackingId` hello előző példában szereplő elemzésének tulajdonságokhoz. Kívüli egy `foreach`, `@result()` ezek az objektumok tömbjét adja vissza.

```json
{
    "name": "Example_Action_That_Failed",
    "inputs": {
        "uri": "https://myfailedaction.azurewebsites.net",
        "method": "POST"
    },
    "outputs": {
        "statusCode": 404,
        "headers": {
            "Date": "Thu, 11 Aug 2016 03:18:18 GMT",
            "Server": "Microsoft-IIS/8.0",
            "X-Powered-By": "ASP.NET",
            "Content-Length": "68",
            "Content-Type": "application/json"
        },
        "body": {
            "code": "ResourceNotFound",
            "message": "/docs/folder-name/resource-name does not exist"
        }
    },
    "startTime": "2016-08-11T03:18:19.7755341Z",
    "endTime": "2016-08-11T03:18:20.2598835Z",
    "trackingId": "bdd82e28-ba2c-4160-a700-e3a8f1a38e22",
    "clientTrackingId": "08587307213861835591296330354",
    "code": "NotFound",
    "status": "Failed"
}
```

tooperform különböző kivételkezelést kombinációját, előzőleg bemutatott hello kifejezéseket is használhat. Előfordulhat, hogy válassza ki a tooexecute egyetlen kivételkezelő, amely fogadja a hibák szűrt tömb teljes hello hello hatókörén kívüli művelet, és távolítsa el a hello `foreach`. Hello más fontos tulajdonságait is használható `@result()` előzőleg bemutatott válasz.

## <a name="azure-diagnostics-and-telemetry"></a>Az Azure Diagnostics- és telemetriabevitelt

hello előző mintától kiváló módja toohandle hibákat és kivételeket futtató belül, de is azonosíthatja, és futtassa maga hello független tooerrors válaszol. 
[Az Azure Diagnostics](../logic-apps/logic-apps-monitor-your-logic-apps.md) biztosít egy egyszerű módon toosend összes munkafolyamat (beleértve az összes futtató és a művelet állapotának) események tooan Azure Storage-fiók vagy egy Azure Event hubs Eseményközpontot. tooevaluate állapotok futtassa, hello naplók és a metrikák figyelése, vagy közzététele inkább bármely felügyeleti eszközt. Egy lehetséges beállítás esetén toostream keresztül Azure Event hubs Eseményközpontot, az összes hello események [Stream Analytics](https://azure.microsoft.com/services/stream-analytics/). A Stream Analytics írhat élő lekérdezések ki bármely rendellenességek észlelését, átlagok vagy hibák hello a diagnosztikai naplókat. A Stream Analytics könnyen kimenetre küldheti például várólisták, témakörök, SQL, Azure Cosmos DB és a Power BI tooother adatforrások.

## <a name="next-steps"></a>Következő lépések

* [Tekintse meg, hogyan ügyfél buildek hiba történt az Azure Logic Apps kezelése](../logic-apps/logic-apps-scenario-error-and-exception-handling.md)
* [További Logic Apps példák és forgatókönyvek keresése](../logic-apps/logic-apps-examples-and-scenarios.md)
* [Ismerje meg, hogyan toocreate automatikus logic Apps alkalmazások központi telepítése](../logic-apps/logic-apps-create-deploy-template.md)
* [Logikai alkalmazások létrehozása és üzembe helyezése a Visual Studióval](logic-apps-deploy-from-vs.md)

<!-- References -->
[retryPolicyMSDN]: https://docs.microsoft.com/rest/api/logic/actions-and-triggers#Anchor_9
