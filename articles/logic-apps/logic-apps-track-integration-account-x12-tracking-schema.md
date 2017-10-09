---
title: "aaaX12 követési sémák B2B figyelés - Azure Logic Apps |} Microsoft Docs"
description: "Az Azure-integráció fiókban tranzakcióról sémák toomonitor B2B üzenetek nyomon követése X12 használja."
author: padmavc
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: a5413f80-eaad-4bcf-b371-2ad0ef629c3d
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: LADocs; padmavc
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ed1b338730214dcae12c367ebff025d7122328fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="start-or-enable-tracking-of-x12-messages-toomonitor-success-errors-and-message-properties"></a>Kezdeti vagy X12 engedélyezése követését üzenetek toomonitor sikeres, a hibák és az üzenet tulajdonságai
Ezek sémák követési X12 is használhatja az Azure-integráció fiók toohelp az üzleti vállalatközi (B2B) tranzakciók figyelése:

* X12 tranzakció követési séma beállítása
* X12 tranzakció séma követési nyugtázási beállítása
* X12 interchange követési séma
* X12 interchange nyugtázási séma nyomon követése
* Funkcionális X12 csoport követési séma
* Funkcionális X12 csoport nyugtázási séma nyomon követése

## <a name="x12-transaction-set-tracking-schema"></a>X12 tranzakció követési séma beállítása
````java

    {
            "agreementProperties": {
                "senderPartnerName": "",
                "receiverPartnerName": "",
                "senderQualifier": "",
                "senderIdentifier": "",
                "receiverQualifier": "",
                "receiverIdentifier": "",
                "agreementName": ""
            },
            "messageProperties": {
                "direction": "",
                "interchangeControlNumber": "",
                "functionalGroupControlNumber": "",
                "transactionSetControlNumber": "",
                "CorrelationMessageId": "",
                "messageType": "",
                "isMessageFailed": "",
                "isTechnicalAcknowledgmentExpected": "",
                "isFunctionalAcknowledgmentExpected": "",
                "needAk2LoopForValidMessages":  "",
                "segmentsCount": ""
            }
    }
````

| Tulajdonság | Típus | Leírás |
| --- | --- | --- |
| senderPartnerName | Karakterlánc | X12 üzenetet küldő partner neve. (Választható) |
| receiverPartnerName | Karakterlánc | X12 üzenet címzett partner neve. (Választható) |
| senderQualifier | Karakterlánc | A minősítő erőforráspartner küldése. (Kötelező) |
| senderIdentifier | Karakterlánc | Partner azonosítót elküldése. (Kötelező) |
| receiverQualifier | Karakterlánc | A minősítő erőforráspartner kapnak. (Kötelező) |
| receiverIdentifier | Karakterlánc | Partner azonosítót kapnak. (Kötelező) |
| agreementName | Karakterlánc | Hello megállapodás toowhich köszönőüzenetei megoldott X12 neve. (Választható) |
| Iránya | Enum | Hello üzenet folyamata, irányának küld és fogad. (Kötelező) |
| interchangeControlNumber | Karakterlánc | Interchange ellenőrző szám. (Választható) |
| functionalGroupControlNumber | Karakterlánc | Funkcionális ellenőrző szám. (Választható) |
| transactionSetControlNumber | Karakterlánc | Tranzakció ellenőrző szám beállítása. (Választható) |
| correlationMessageId | Karakterlánc | Korrelációs azonosító. {AgreementName} kombinációja {*GroupControlNumber*} {TransactionSetControlNumber}. (Választható) |
| messageType | Karakterlánc | A tranzakció állítsa be, vagy dokumentum típusa. (Választható) |
| isMessageFailed | Logikai érték | E X12 üdvözlőüzenetére nem sikerült. (Kötelező) |
| isTechnicalAcknowledgmentExpected | Logikai érték | E hello műszaki nyugtázási hello X12 megállapodás van konfigurálva. (Kötelező) |
| isFunctionalAcknowledgmentExpected | Logikai érték | E hello működési nyugtázási hello X12 megállapodás van konfigurálva. (Kötelező) |
| needAk2LoopForValidMessages | Logikai érték | E hello AK2 hurok kell egy érvényes üzenetet. (Kötelező) |
| segmentsCount | Egész szám | Hello X12 tranzakció készlet szegmensek száma. (Választható) |

## <a name="x12-transaction-set-acknowledgement-tracking-schema"></a>X12 tranzakció séma követési nyugtázási beállítása
````java

    {
            "agreementProperties": {
                "senderPartnerName": "",
                "receiverPartnerName": "",
                "senderQualifier": "",
                "senderIdentifier": "",
                "receiverQualifier": "",
                "receiverIdentifier": "",
                "agreementName": ""
            },
            "messageProperties": {
                "direction": "",
                "interchangeControlNumber": "",
                "functionalGroupControlNumber": "",
                "isaSegment": "",
                "gsSegment": "",
                "respondingfunctionalGroupControlNumber": "",
                "respondingFunctionalGroupId": "",
                "respondingtransactionSetControlNumber": "",
                "respondingTransactionSetId": "",
                "statusCode": "",
                "processingStatus": "",
                "CorrelationMessageId": ""
                "isMessageFailed": "",
                "ak2Segment": "",
                "ak3Segment": "",
                "ak5Segment": ""
            }
    }
````

| Tulajdonság | Típus | Leírás |
| --- | --- | --- |
| senderPartnerName | Karakterlánc | X12 üzenetet küldő partner neve. (Választható) |
| receiverPartnerName | Karakterlánc | X12 üzenet címzett partner neve. (Választható) |
| senderQualifier | Karakterlánc | A minősítő erőforráspartner küldése. (Kötelező) |
| senderIdentifier | Karakterlánc | Partner azonosítót elküldése. (Kötelező) |
| receiverQualifier | Karakterlánc | A minősítő erőforráspartner kapnak. (Kötelező) |
| receiverIdentifier | Karakterlánc | Partner azonosítót kapnak. (Kötelező) |
| agreementName | Karakterlánc | Hello megállapodás toowhich köszönőüzenetei megoldott X12 neve. (Választható) |
| Iránya | Enum | Hello üzenet folyamata, irányának küld és fogad. (Kötelező) |
| interchangeControlNumber | Karakterlánc | Interchange hello működési nyugtázási ellenőrzési számát. Értéke csak a hello küldési oldalon, ahol működési nyugtázás érkezett hello küldött üzenetek toopartner tölti fel. (Választható) |
| functionalGroupControlNumber | Karakterlánc | Hello működési nyugtázási funkcionális csoport vezérlő száma. Értéke csak a hello küldési oldalon, ahol működési nyugtázás érkezett hello küldött üzenetek toopartner tölti fel. (Választható) |
| isaSegment | Karakterlánc | ISA szegmens hello üzenet. Értéke csak a hello küldési oldalon, ahol működési nyugtázás érkezett hello küldött üzenetek toopartner tölti fel. (Választható) |
| gsSegment | Karakterlánc | GS szegmens hello üzenet. Értéke csak a hello küldési oldalon, ahol működési nyugtázás érkezett hello küldött üzenetek toopartner tölti fel. (Választható) |
| respondingfunctionalGroupControlNumber | Karakterlánc | Interchange ellenőrző szám válaszol. (Választható) |
| respondingFunctionalGroupId | Karakterlánc | Válaszol funkcionális csoport azonosítója, amely a szolgáltatástérképek tooAK101 hello nyugtázási. (Választható) |
| respondingtransactionSetControlNumber | Karakterlánc | Válaszol tranzakció ellenőrző szám beállítása. (Választható) |
| respondingTransactionSetId | Karakterlánc | Válaszol tranzakció azonosítója, amely a szolgáltatástérképek tooAK201 hello nyugtázási beállítása. (Választható) |
| statusCode | Logikai érték | Tranzakció nyugtázási állapotkód beállítása. (Kötelező) |
| segmentsCount | Enum | Nyugtázási állapotkódot. Két érték engedélyezett **elfogadott**, **elutasítva**, és **AcceptedWithErrors**. (Kötelező) |
| processingStatus | Enum | Hello nyugtázási feldolgozási állapotát. Két érték engedélyezett **fogadott**, **Generated**, és **küldött**. (Kötelező) |
| correlationMessageId | Karakterlánc | Korrelációs azonosító. {AgreementName} kombinációja {*GroupControlNumber*} {TransactionSetControlNumber}. (Választható) |
| isMessageFailed | Logikai érték | E X12 üdvözlőüzenetére nem sikerült. (Kötelező) |
| ak2Segment | Karakterlánc | A tranzakció belül kapott hello funkcionális csoport nyugtázási. (Választható) |
| ak3Segment | Karakterlánc | A jelentés egy data szegmenséhez hibáit. (Választható) |
| ak5Segment | Karakterlánc | A jelentés-e beállítva hello AK2 szegmensben lévő azonosított hello tranzakció elfogadott vagy nem utasítható el, és ezért. (Választható) |

## <a name="x12-interchange-tracking-schema"></a>X12 interchange követési séma
````java

    {
            "agreementProperties": {
                "senderPartnerName": "",
                "receiverPartnerName": "",
                "senderQualifier": "",
                "senderIdentifier": "",
                "receiverQualifier": "",
                "receiverIdentifier": "",
                "agreementName": ""
            },
            "messageProperties": {
                "direction": "",
                "interchangeControlNumber": "",
                "isaSegment": "",
                "isTechnicalAcknowledgmentExpected": "",
                "isMessageFailed": "",
                "isa09": "",
                "isa10": "",
                "isa11": "",
                "isa12": "",
                "isa14": "",
                "isa15": "",
                "isa16": ""
            }
    }
````

| Tulajdonság | Típus | Leírás |
| --- | --- | --- |
| senderPartnerName | Karakterlánc | X12 üzenetet küldő partner neve. (Választható) |
| receiverPartnerName | Karakterlánc | X12 üzenet címzett partner neve. (Választható) |
| senderQualifier | Karakterlánc | A minősítő erőforráspartner küldése. (Kötelező) |
| senderIdentifier | Karakterlánc | Partner azonosítót elküldése. (Kötelező) |
| receiverQualifier | Karakterlánc | A minősítő erőforráspartner kapnak. (Kötelező) |
| receiverIdentifier | Karakterlánc | Partner azonosítót kapnak. (Kötelező) |
| agreementName | Karakterlánc | Hello megállapodás toowhich köszönőüzenetei megoldott X12 neve. (Választható) |
| Iránya | Enum | Hello üzenet folyamata, irányának küld és fogad. (Kötelező) |
| interchangeControlNumber | Karakterlánc | Interchange ellenőrző szám. (Választható) |
| isaSegment | Karakterlánc | Üzenet ISA szegmens. (Választható) |
| isTechnicalAcknowledgmentExpected | Logikai érték | E hello műszaki nyugtázási hello X12 megállapodás van konfigurálva. (Kötelező) |
| isMessageFailed | Logikai érték | E X12 üdvözlőüzenetére nem sikerült. (Kötelező) |
| isa09 | Karakterlánc | X12 dokumentum cseréje dátum. (Választható) |
| isa10 | Karakterlánc | A dokumentum interchange idő X12. (Választható) |
| isa11 | Karakterlánc | X12 interchange vezérlő szabványok azonosítója. (Választható) |
| isa12 | Karakterlánc | X12 interchange vezérlő verziószáma. (Választható) |
| isa14 | Karakterlánc | X12 nyugtázási van szükség. (Választható) |
| isa15 | Karakterlánc | A teszt- vagy éles mutató. (Választható) |
| isa16 | Karakterlánc | Elem elválasztó. (Választható) |

## <a name="x12-interchange-acknowledgement-tracking-schema"></a>X12 interchange nyugtázási séma nyomon követése
````java
    {
            "agreementProperties": {
                "senderPartnerName": "",
                "receiverPartnerName": "",
                "senderQualifier": "",
                "senderIdentifier": "",
                "receiverQualifier": "",
                "receiverIdentifier": "",
                "agreementName": ""
            },
            "messageProperties": {
                "direction": "",
                "interchangeControlNumber": "",
                "isaSegment": "",
                "respondingInterchangeControlNumber": "",
                "isMessageFailed": "",
                "statusCode": "",
                "processingStatus": "",
                "ta102": "",
                "ta103": "",
                "ta105": ""
            }
    }
````

| Tulajdonság | Típus | Leírás |
| --- | --- | --- |
| senderPartnerName | Karakterlánc | X12 üzenetet küldő partner neve. (Választható) |
| receiverPartnerName | Karakterlánc | X12 üzenet címzett partner neve. (Választható) |
| senderQualifier | Karakterlánc | A minősítő erőforráspartner küldése. (Kötelező) |
| senderIdentifier | Karakterlánc | Partner azonosítót elküldése. (Kötelező) |
| receiverQualifier | Karakterlánc | A minősítő erőforráspartner kapnak. (Kötelező) |
| receiverIdentifier | Karakterlánc | Partner azonosítót kapnak. (Kötelező) |
| agreementName | Karakterlánc | Hello megállapodás toowhich köszönőüzenetei megoldott X12 neve. (Választható) |
| Iránya | Enum | Hello üzenet folyamata, irányának küld és fogad. (Kötelező) |
| interchangeControlNumber | Karakterlánc | Interchange hello műszaki nyugtázási résztvevőitől kapott ellenőrzési számát. (Választható) |
| isaSegment | Karakterlánc | ISA szegmense hello műszaki nyugtázási résztvevőitől kapott. (Választható) |
| respondingInterchangeControlNumber |Karakterlánc | Interchange hello műszaki nyugtázási résztvevőitől kapott az ellenőrző szám. (Választható) |
| isMessageFailed | Logikai érték | E X12 üdvözlőüzenetére nem sikerült. (Kötelező) |
| statusCode | Enum | Interchange nyugtázási állapotkódot. Két érték engedélyezett **elfogadott**, **elutasítva**, és **AcceptedWithErrors**. (Kötelező) |
| processingStatus | Enum | Nyugtázási állapota. Két érték engedélyezett **fogadott**, **Generated**, és **küldött**. (Kötelező) |
| TA102 | Karakterlánc | Interchange dátum. (Választható) |
| ta103 | Karakterlánc | Interchange idő. (Választható) |
| ta105 | Karakterlánc | Megjegyzés: kód Interchange. (Választható) |

## <a name="x12-functional-group-tracking-schema"></a>Funkcionális X12 csoport követési séma
````java

    {
            "agreementProperties": {
                "senderPartnerName": "",
                "receiverPartnerName": "",
                "senderQualifier": "",
                "senderIdentifier": "",
                "receiverQualifier": "",
                "receiverIdentifier": "",
                "agreementName": ""
            },
            "messageProperties": {
                "direction": "",
                "interchangeControlNumber": "",
                "functionalGroupControlNumber": "",
                "gsSegment": "",
                "isTechnicalAcknowledgmentExpected": "",
                "isFunctionalAcknowledgmentExpected": "",
                "isMessageFailed": "",
                "gs01": "",
                "gs02": "",
                "gs03": "",
                "gs04": "",
                "gs05": "",
                "gs07": "",
                "gs08": ""
            }
    }
````

| Tulajdonság | Típus | Leírás |
| --- | --- | --- |
| senderPartnerName | Karakterlánc | X12 üzenetet küldő partner neve. (Választható) |
| receiverPartnerName | Karakterlánc | X12 üzenet címzett partner neve. (Választható) |
| senderQualifier | Karakterlánc | A minősítő erőforráspartner küldése. (Kötelező) |
| senderIdentifier | Karakterlánc | Partner azonosítót elküldése. (Kötelező) |
| receiverQualifier | Karakterlánc | A minősítő erőforráspartner kapnak. (Kötelező) |
| receiverIdentifier | Karakterlánc | Partner azonosítót kapnak. (Kötelező) |
| agreementName | Karakterlánc | Hello megállapodás toowhich köszönőüzenetei megoldott X12 neve. (Választható) |
| Iránya | Enum | Hello üzenet folyamata, irányának küld és fogad. (Kötelező) |
| interchangeControlNumber | Karakterlánc | Interchange ellenőrző szám. (Választható) |
| functionalGroupControlNumber | Karakterlánc | Funkcionális ellenőrző szám. (Választható) |
| gsSegment | Karakterlánc | Üzenet GS szegmens. (Választható) |
| isTechnicalAcknowledgmentExpected | Logikai érték | E hello műszaki nyugtázási hello X12 megállapodás van konfigurálva. (Kötelező) |
| isFunctionalAcknowledgmentExpected | Logikai érték | E hello működési nyugtázási hello X12 megállapodás van konfigurálva. (Kötelező) |
| isMessageFailed | Logikai érték | E X12 üdvözlőüzenetére nem sikerült. (Kötelező)|
| gs01 | Karakterlánc | Funkcionális kódját. (Választható) |
| gs02 | Karakterlánc | Alkalmazás feladó kódot. (Választható) |
| gs03 | Karakterlánc | Alkalmazás fogadó kódot. (Választható) |
| gs04 | Karakterlánc | Funkcionális csoport dátuma. (Választható) |
| gs05 | Karakterlánc | Funkcionális csoport idő. (Választható) |
| gs07 | Karakterlánc | Felelős ügynökség kódot. (Választható) |
| gs08 | Karakterlánc | Verzió/kiadás/iparági kódját. (Választható) |

## <a name="x12-functional-group-acknowledgement-tracking-schema"></a>Funkcionális X12 csoport nyugtázási séma nyomon követése
````java
    {
            "agreementProperties": {
                "senderPartnerName": "",
                "receiverPartnerName": "",
                "senderQualifier": "",
                "senderIdentifier": "",
                "receiverQualifier": "",
                "receiverIdentifier": "",
                "agreementName": ""
            },
            "messageProperties": {
                "direction": "",
                "interchangeControlNumber": "",
                "functionalGroupControlNumber": "",
                "isaSegment": "",
                "gsSegment": "",
                "respondingfunctionalGroupControlNumber": "",
                "respondingFunctionalGroupId": "",
                "isMessageFailed": "",
                "statusCode": "",
                "processingStatus": "",
                "ak903": "",
                "ak904": "",
                "ak9Segment": ""
            }
    }
````

| Tulajdonság | Típus | Leírás |
| --- | --- | --- |
| senderPartnerName | Karakterlánc | X12 üzenetet küldő partner neve. (Választható) |
| receiverPartnerName | Karakterlánc | X12 üzenet címzett partner neve. (Választható) |
| senderQualifier | Karakterlánc | A minősítő erőforráspartner küldése. (Kötelező) |
| senderIdentifier | Karakterlánc | Partner azonosítót elküldése. (Kötelező) |
| receiverQualifier | Karakterlánc | A minősítő erőforráspartner kapnak. (Kötelező) |
| receiverIdentifier | Karakterlánc | Partner azonosítót kapnak. (Kötelező) |
| agreementName | Karakterlánc | Hello megállapodás toowhich köszönőüzenetei megoldott X12 neve. (Választható) |
| Iránya | Enum | Hello üzenet folyamata, irányának küld és fogad. (Kötelező) |
| interchangeControlNumber | Karakterlánc | Interchange vezérlő száma, amely tölti fel a küldési oldalon hello műszaki nyugtázási partnertől fogadásakor. (Választható) |
| functionalGroupControlNumber | Karakterlánc | Funkcionális vezérlő csoportszámmal hello műszaki nyugtázása, amely feltölti az hello ügyféloldali küldése, ha a partnerek műszaki nyugtázás érkezett. (Választható) |
| isaSegment | Karakterlánc | Interchange azonos szabályozza, hogy a számot, de csak bizonyos esetekben ki van töltve. (Választható) |
| gsSegment | Karakterlánc | Ugyanaz, mint funkcionális csoport szabályozza, hogy a számot, de csak bizonyos esetekben ki van töltve. (Választható) |
| respondingfunctionalGroupControlNumber | Karakterlánc | Hello eredeti funkcionális csoport számát szabályozza. (Választható) |
| respondingFunctionalGroupId | Karakterlánc | A Maps tooAK101 a hello nyugtázási funkcionális azonosítót. (Választható) |
| isMessageFailed | Logikai érték | E X12 üdvözlőüzenetére nem sikerült. (Kötelező) |
| statusCode | Enum | Nyugtázási állapotkódot. Két érték engedélyezett **elfogadott**, **elutasítva**, és **AcceptedWithErrors**. (Kötelező) |
| processingStatus | Enum | Hello nyugtázási feldolgozási állapotát. Két érték engedélyezett **fogadott**, **Generated**, és **küldött**. (Kötelező) |
| ak903 | Karakterlánc | Beérkezett tranzakció halmazok számát. (Választható) |
| ak904 | Karakterlánc | Tranzakció karakterkészleteket azonosított hello funkcionális csoportban fogadja. (Választható) |
| ak9Segment | Karakterlánc | E hello funkcionális csoporttal hello AK1 szegmensben lévő elfogadott vagy nem utasítható el, és ezért. (Választható) |

## <a name="next-steps"></a>Következő lépések
* További információ [B2B üzenetek figyelése](logic-apps-monitor-b2b-message.md).
* További információ [AS2 követési sémák](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md).
* További információ [egyéni sémák követési B2B](../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md).
* További tudnivalók [hello Operations Management Suite portálját a B2B üzenetek nyomon követése](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).
* További tudnivalók hello [vállalati integrációs csomag](../logic-apps/logic-apps-enterprise-integration-overview.md).  
