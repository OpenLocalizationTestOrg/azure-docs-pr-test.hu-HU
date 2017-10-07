---
title: "követés sémák B2B aaaCustom figyelési - Azure Logic Apps |} Microsoft Docs"
description: "Hozzon létre egyéni követési sémák toomonitor B2B üzenetek a tranzakciók az Azure-integráció fiókban."
author: padmavc
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 433ae852-a833-44d3-a3c3-14cca33403a2
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: LADocs; padmavc
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8cf26a43d89f0414a2a8c5ef59d804235afeb5d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enable-tracking-toomonitor-your-complete-workflow-end-to-end"></a>Engedélyezi a nyomkövetési toomonitor munkafolyamat, végpontok közötti
Nincs beépített nyomon követése, hogy engedélyezheti a vállalatok munkafolyamat, például követési AS2 vagy X12 egyes részeinek üzeneteket. Amikor hoz létre, amely tartalmazza a logikai alkalmazás, a BizTalk Serveren, SQL Server vagy bármely más réteg munkafolyamatok, majd engedélyezheti egyéni, amely eseményeket naplózza a munkafolyamat a hello elejére toohello végére. 

Ez a témakör olyan egyéni kód, amely kívül a logikai alkalmazás hello rétegek használhatja. 

## <a name="custom-tracking-schema"></a>Egyéni követési séma
````java

        {
            "sourceType": "",
            "source": {

            "workflow": {
                "systemId": ""
            },
            "runInstance": {
                "runId": ""
            },
            "operation": {
                "operationName": "",
                "repeatItemScopeName": "",
                "repeatItemIndex": "",
                "trackingId": "",
                "correlationId": "",
                "clientRequestId": ""
                }
            },
            "events": [
            {
                "eventLevel": "",
                "eventTime": "",
                "recordType": "",
                "record": {                
                }
            }
         ]
      }

````

| Tulajdonság | Típus | Leírás |
| --- | --- | --- |
| Forrástípus |   | Futtatás hello forrás típusa. Két érték engedélyezett **Microsoft.Logic/workflows** és **egyéni**. (Kötelező) |
| Forrás |   | Ha hello forrástípus **Microsoft.Logic/workflows**, hello adatforrásra vonatkozó információ kell toofollow ebben a sémában. Ha hello forrástípus **egyéni**, hello séma egy JToken. (Kötelező) |
| Rendszerazonosító | Karakterlánc | Logic app rendszer azonosítóját. (Kötelező) |
| futtatásazonosító | Karakterlánc | Logikai alkalmazás futtatásához azonosítóját. (Kötelező) |
| operationName | Karakterlánc | Hello művelet (például a művelet vagy az eseményindító) neve. (Kötelező) |
| repeatItemScopeName | Karakterlánc | Ismételje meg a elem neve, ha hello művelet belül egy `foreach` / `until` hurok. (Kötelező) |
| repeatItemIndex | Egész szám | E művelet hello belül van-e egy `foreach` / `until` hurok. Azt jelzi, hogy hello ismétlődő elem indexe. (Kötelező) |
| trackingId | Karakterlánc | Nyomkövetési azonosító, toocorrelate köszönőüzenetei. (Választható) |
| correlationId | Karakterlánc | Korrelációs azonosító, toocorrelate köszönőüzenetei. (Választható) |
| clientRequestId | Karakterlánc | Ügyfél feltöltheti azt toocorrelate üzeneteket. (Választható) |
| EventLevel |   | Hello esemény szintje. (Kötelező) |
| eventTime |   | Idő hello esemény, éééé-hh-DDTHH:MM:SS.00000Z UTC formátumban. (Kötelező) |
| recordType |   | Hello követése rekord típusa. Az engedélyezett értéket **egyéni**. (Kötelező) |
| rekord |   | Egyéni rekordtípus. Engedélyezett JToken érvénytelen. (Kötelező) |

## <a name="b2b-protocol-tracking-schemas"></a>B2B protokoll követési sémák
Követés sémák B2B protokoll kapcsolatos információkért lásd:
* [AS2-követési sémák](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)   
* [X12-követési sémák](logic-apps-track-integration-account-x12-tracking-schema.md)

## <a name="next-steps"></a>Következő lépések
* További információ [B2B üzenetek figyelése](logic-apps-monitor-b2b-message.md).   
* További tudnivalók [hello Operations Management Suite portálját a B2B üzenetek nyomon követése](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).
* További tudnivalók hello [vállalati integrációs csomag](../logic-apps/logic-apps-enterprise-integration-overview.md).
