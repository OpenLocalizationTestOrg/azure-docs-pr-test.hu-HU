---
title: "aaaAS2 követési sémák B2B figyelés - Azure Logic Apps |} Microsoft Docs"
description: "AS2 használja az Azure-integráció fiókban tranzakcióról sémák toomonitor B2B üzenetek nyomon követése."
author: padmavc
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: f169c411-1bd7-4554-80c1-84351247bf94
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: LADocs; padmavc
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: fe3c5845e2e80160d6857d8c308d836e88af7331
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="start-or-enable-tracking-of-as2-messages-and-mdns-toomonitor-success-errors-and-message-properties"></a>Indítsa el, vagy az AS2-üzeneteket és a MDNs toomonitor sikeres, a hibák és a üzenettulajdonságok követését engedélyezése
Ezek AS2 követési sémákat is használhatja az Azure-integráció fiók toohelp az üzleti vállalatközi (B2B) tranzakciók figyelése:

* AS2 üzenet követési séma
* AS2 MDN követési séma

## <a name="as2-message-tracking-schema"></a>AS2 üzenet követési séma
````java

    {
       "agreementProperties": {  
            "senderPartnerName": "",  
            "receiverPartnerName": "",  
            "as2To": "",  
            "as2From": "",  
            "agreementName": ""  
        },  
        "messageProperties": {
            "direction": "",
            "messageId": "",
            "dispositionType": "",
            "fileName": "",
            "isMessageFailed": "",
            "isMessageSigned": "",
            "isMessageEncrypted": "",
            "isMessageCompressed": "",
            "correlationMessageId": "",
            "incomingHeaders": {
            },
            "outgoingHeaders": {
            },
        "isNrrEnabled": "",
        "isMdnExpected": "",
        "mdnType": ""
        }
    }
````

| Tulajdonság | Típus | Leírás |
| --- | --- | --- |
| senderPartnerName | Karakterlánc | AS2 üzenet küldőjének partner neve. (Választható) |
| receiverPartnerName | Karakterlánc | AS2 üzenetet fogadó partner neve. (Választható) |
| as2To | Karakterlánc | AS2 üzenetet fogadó nevét, a hello fejlécek hello AS2-üzenet. (Kötelező) |
| as2From | Karakterlánc | AS2 üzenet küldőjének nevét, a hello fejlécek hello AS2-üzenet. (Kötelező) |
| agreementName | Karakterlánc | Megoldott hello AS2 megállapodás toowhich köszönőüzenetei nevét. (Választható) |
| Iránya | Karakterlánc | Hello üzenet folyamata, irányának küld és fogad. (Kötelező) |
| messageId | Karakterlánc | AS2-azonosító tartozik, a hello fejlécek hello AS2-üzenet (nem kötelező) |
| dispositionType |Karakterlánc | Üzenet törlése értesítés (MDN) típusú érték. (Választható) |
| fileName | Karakterlánc | Fájl neve, az hello AS2 üzenet hello fejlécből. (Választható) |
| isMessageFailed |Logikai érték | E AS2 üdvözlőüzenetére nem sikerült. (Kötelező) |
| isMessageSigned | Logikai érték | E hello AS2 üzenet aláírása. (Kötelező) |
| isMessageEncrypted | Logikai érték | E hello AS2 üzenet titkosítva van. (Kötelező) |
| isMessageCompressed |Logikai érték | E AS2 üdvözlőüzenetére tömörítették. (Kötelező) |
| correlationMessageId | Karakterlánc | AS2-azonosító tartozik, MDNs toocorrelate üzenetek. (Választható) |
| incomingHeaders |JToken álló Dictionary | Bejövő AS2 üzenet fejlécének adatai. (Választható) |
| outgoingHeaders |JToken álló Dictionary | Kimenő AS2 üzenet fejlécének adatai. (Választható) |
| isNrrEnabled | Logikai érték | Használja az alapértelmezett értéket, ha hello érték nem ismert. (Kötelező) |
| isMdnExpected | Logikai érték | Használja az alapértelmezett értéket, ha hello érték nem ismert. (Kötelező) |
| mdnType | Enum | Két érték engedélyezett **NotConfigured**, **szinkronizálási**, és **aszinkron**. (Kötelező) |

## <a name="as2-mdn-tracking-schema"></a>AS2 MDN követési séma
````java

    {
        "agreementProperties": {
                "senderPartnerName": "",
                "receiverPartnerName": "",
                "as2To": "",
                "as2From": "",
                "agreementName": "g"
            },
            "messageProperties": {
                "direction": "",
                "messageId": "",
                "originalMessageId": "",
                "dispositionType": "",
                "isMessageFailed": "",
                "isMessageSigned": "",
                "isNrrEnabled": "",
                "statusCode": "",
                "micVerificationStatus": "",
                "correlationMessageId": "",
                "incomingHeaders": {
                },
                "outgoingHeaders": {
                }
            }
    }
````

| Tulajdonság | Típus | Leírás |
| --- | --- | --- |
| senderPartnerName | Karakterlánc | AS2 üzenet küldőjének partner neve. (Választható) |
| receiverPartnerName | Karakterlánc | AS2 üzenetet fogadó partner neve. (Választható) |
| as2To | Karakterlánc | AS2 üdvözlőüzenetére fogadó partner neve. (Kötelező) |
| as2From | Karakterlánc | Partner neve, akik hello AS2 üzenetet küld. (Kötelező) |
| agreementName | Karakterlánc | Megoldott hello AS2 megállapodás toowhich köszönőüzenetei nevét. (Választható) |
| Iránya |Karakterlánc | Hello üzenet folyamata, irányának küld és fogad. (Kötelező) |
| messageId | Karakterlánc | AS2 azonosító. (Választható) |
| originalMessageId |Karakterlánc | AS2 eredeti azonosító. (Választható) |
| dispositionType | Karakterlánc | MDN típusú érték. (Választható) |
| isMessageFailed |Logikai érték | E AS2 üdvözlőüzenetére nem sikerült. (Kötelező) |
| isMessageSigned |Logikai érték | E hello AS2 üzenet aláírása. (Kötelező) |
| isNrrEnabled | Logikai érték | Használja az alapértelmezett értéket, ha hello érték nem ismert. (Kötelező) |
| statusCode | Enum | Két érték engedélyezett **elfogadott**, **elutasítva**, és **AcceptedWithErrors**. (Kötelező) |
| micVerificationStatus | Enum | Két érték engedélyezett **NotApplicable**, **sikeres**, és **sikertelen**. (Kötelező) |
| correlationMessageId | Karakterlánc | Korrelációs azonosítója. eredeti hello messaged azonosítója (hello hello üzenet, amely MDN beállított üzenet azonosítója). (Választható) |
| incomingHeaders | JToken álló Dictionary | Azt jelzi, hogy a bejövő üzenet fejlécének adatai. (Választható) |
| outgoingHeaders |JToken álló Dictionary | Azt jelzi, hogy a kimenő üzenet fejlécének adatai. (Választható) |

## <a name="next-steps"></a>Következő lépések
* További tudnivalók hello [vállalati integrációs csomag](../logic-apps/logic-apps-enterprise-integration-overview.md).    
* További információ [B2B üzenetek figyelése](logic-apps-monitor-b2b-message.md).   
* További információ [egyéni sémák követési B2B](logic-apps-track-integration-account-custom-tracking-schema.md).   
* További információ [sémák követési X12](logic-apps-track-integration-account-x12-tracking-schema.md).   
* További tudnivalók [hello Operations Management Suite portálját a B2B üzenetek nyomon követése](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).
