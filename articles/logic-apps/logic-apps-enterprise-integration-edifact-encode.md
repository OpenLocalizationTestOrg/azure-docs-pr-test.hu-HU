---
title: "aaaEncode EDIFACT üzenetek - Azure Logic Apps |} Microsoft Docs"
description: "EDI érvényesítése és EDIFACT üzenet Encoder hello vállalati integrációs csomagot az XML létrehozása az Azure Logic Apps"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: 974ac339-d97a-4715-bc92-62d02281e900
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: b3799dbd2492adef597022d017cf28f5ceb1085c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="encode-edifact-messages-for-azure-logic-apps-with-hello-enterprise-integration-pack"></a>EDIFACT üzenetek kódolása az Azure Logic Apps a hello vállalati integrációs csomag

Hello kódolása EDIFACT-üzenet összekötővel EDI és a partner jellemző tulajdonságok ellenőrzése, az XML-dokumentum az egyes tranzakció készítése és műszaki nyugtázási vagy működési visszaigazolás kérése.
toouse ezt az összekötőt, hozzá kell adnia a Logic Apps alkalmazást az eseményindító meglévő hello összekötő tooan.

## <a name="before-you-start"></a>Előkészületek

Itt van szüksége hello elemeket:

* Az Azure-fiók; létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free)
* Egy [integrációs fiók](logic-apps-enterprise-integration-create-integration-account.md) , amely már definiált és az Azure-előfizetéshez társítva. Az integráció fiók toouse hello kódolása EDIFACT üzenet csatlakozó kell rendelkeznie. 
* Legalább két [partnerek](logic-apps-enterprise-integration-partners.md) , amely már definiálva vannak az integráció-fiókban
* Egy [EDIFACT megállapodás](logic-apps-enterprise-integration-edifact.md) , amely már definiálva van az integráció-fiókban

## <a name="encode-edifact-messages"></a>EDIFACT üzenetek kódolása

1. [Logikai alkalmazás létrehozása](logic-apps-create-a-logic-app.md).

2. hello kódolása EDIFACT üzenet összekötő nincs eseményindítók, hozzá kell adni egy eseményindító indítása el a logikai alkalmazás, például a kérelem eseményindítót. A Logic App Designer hello adja hozzá egy eseményindító, és adja hozzá az egy művelet tooyour logikai alkalmazást.

3.  Hello keresési mezőbe írja be a "EDIFACT" szűrőként. Válassza **EDIFACT üzenetből kódolása szerződésnév** vagy **Encode tooEDIFACT üzenetre identitások**.
   
    ![EDIFACT keresése](media/logic-apps-enterprise-integration-edifact-encode/edifactdecodeimage1.png)  

3. Ha nem korábban létre a kapcsolatokat tooyour integrációs fiókot, megkéri toocreate most, hogy a kapcsolat. A kapcsolat neve, és válassza ki, hogy szeretné-e tooconnect hello integrációs fiókot.

    ![integráció fiók kapcsolat létrehozása](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage1.png)  

    Tulajdonságok csillaggal szükség.

    | Tulajdonság | Részletek |
    | --- | --- |
    | Kapcsolat neve * |Adjon meg egy tetszőleges nevet a kapcsolat. |
    | Integráció fiók * |Adja meg a integrációs fiók nevét. Győződjön meg arról, hogy az integrációs fiók és a logic app hello Azure ugyanazon a helyen. |

5.  Amikor elkészült, a kapcsolat adatai alábbihoz hasonló toothis példa. a kapcsolat létrehozása toofinish válassza **létrehozása**.

    ![integráció fiók kapcsolódási adatait.](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage2.png)

    A kapcsolat létrejön.

    ![integráció fiók kapcsolat létrehozása](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage4.png)

#### <a name="encode-edifact-message-by-agreement-name"></a>EDIFACT üzenetek kódolása szerződés neve

Ha úgy dönt, tooencode EDIFACT üzenetek szerződés neve, nyissa meg a hello **nevét a EDIFACT megállapodás** listában, adja meg vagy válassza ki a EDIFACT a szerződés nevét. Adja meg a hello XML üzenet tooencode.

![Adja meg a EDIFACT szerződés neve és XML üzenet tooencode](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage6.png)

#### <a name="encode-edifact-message-by-identities"></a>EDIFACT üzenetek által identitások kódolása

Ha tooencode EDIFACT üzenetek által identitások, adja meg a hello a küldő azonosítója, a küldő minősítő, a fogadó azonosítója és a fogadó minősítő EDIFACT kötött konfigurált módon. Válassza ki a hello XML üzenet tooencode.

![Adjon meg a küldő és fogadó identitásainak, válassza az XML-üzenet tooencode](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage7.png)

## <a name="edifact-encode-details"></a>EDIFACT kódolása részletei

hello kódolása EDIFACT-összekötő az alábbi feladatokat hajtja végre: 

* Hello megállapodás oldani az egyező hello küldő minősítő & azonosítója és a fogadó minősítő és azonosítója
* XML-kódolású üzenetekkel konvertálásakor EDI tranzakció halmazok hello interchange hello EDI-interchange rendezi sorba.
* Tranzakció set fejléc és pótkocsi szegmensek vonatkozik
* Az adatcsere ellenőrző szám, a vezérlő csoportszámmal és a tranzakció beállítása vezérlő száma minden kimenő adatcserét hoz létre
* A felváltja elválasztók hello hasznos adatban
* Érvényesíti EDI- és erőforráspartner jellemző tulajdonságok
  * A sémaérvényesítés hello tranzakció-set adatelemek hello üzenet sémának.
  * EDI érvényesítési tranzakció-set adatelemek végre.
  * Tranzakció-set adatelemek végre kiterjesztett érvényesítése
* Az XML-dokumentum az egyes tranzakció állít elő.
* A műszaki és/vagy funkcionális visszaigazolás-kérelmek (Ha be van állítva).
  * A műszaki visszaigazolás, mint hello CONTRL üzenet azt jelzi, az cseréje fogadását.
  * A működési visszaigazolás, mint hello CONTRL az üzenet azt jelzi, elfogadási vagy a kapott hello interchange, vagy az üzenet, a hibák vagy nem támogatott funkciók listáját elutasítása

## <a name="view-swagger-file"></a>A Swagger-fájl megtekintése
tooview hello Swagger részletek hello EDIFACT-összekötőhöz: [EDIFACT](/connectors/edifact/).

## <a name="next-steps"></a>Következő lépések
[További tudnivalók a vállalati integrációs csomag hello](logic-apps-enterprise-integration-overview.md "további információ a vállalati integrációs csomag") 

