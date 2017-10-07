---
title: "aaaEncode X12 üzenetek - Azure Logic Apps |} Microsoft Docs"
description: "EDI érvényesítése és átalakítani az XML-kódolású X12 helyű üzenet hello vállalati integrációs csomag a kódoló Azure Logic Apps"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: a01e9ca9-816b-479e-ab11-4a984f10f62d
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: 785dbd2c7c82463154732921e07e3586d2307840
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="encode-x12-messages-for-azure-logic-apps-with-hello-enterprise-integration-pack"></a>Kódolja X12 hello vállalati integrációs csomagot az Azure Logic Apps állapotüzenete

Hello Encode X12 üzenet összekötővel EDI és a partner jellemző tulajdonságok ellenőrzése, XML-kódolású üzenetekkel átalakítása EDI tranzakció halmazok hello adatcsere és kérelem műszaki nyugtázási, működési nyugtázási vagy mindkettőt.
toouse ezt az összekötőt, hozzá kell adnia a Logic Apps alkalmazást az eseményindító meglévő hello összekötő tooan.

## <a name="before-you-start"></a>Előkészületek

Itt van szüksége hello elemeket:

* Az Azure-fiók; létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free)
* Egy [integrációs fiók](logic-apps-enterprise-integration-create-integration-account.md) , amely már definiált és az Azure-előfizetéshez társítva. Az integráció fiók toouse hello Encode X12 üzenet csatlakozó kell rendelkeznie.
* Legalább két [partnerek](logic-apps-enterprise-integration-partners.md) , amely már definiálva vannak az integráció-fiókban
* Egy [X12 megállapodás](logic-apps-enterprise-integration-x12.md) , amely már definiálva van az integráció-fiókban

## <a name="encode-x12-messages"></a>Kódolja X12 üzenetek

1. [Logikai alkalmazás létrehozása](logic-apps-create-a-logic-app.md).

2. hello Encode X12 üzenet összekötő eseményindítókat, nem rendelkezik, hozzá kell adni egy eseményindító indítása el a logikai alkalmazás, például egy kérelem eseményindító. A Logic App Designer hello adja hozzá egy eseményindító, és adja hozzá az egy művelet tooyour logikai alkalmazást.

3.  Hello keresési mezőbe írja be a "x12" a szűrőhöz. Válassza ki vagy **X12-tooX12 üzenetek kódolása szerződésnév által** vagy **X12-tooX12 üzenetek kódolása identitások által**.
   
    ![Keressen a "x12"](./media/logic-apps-enterprise-integration-x12-encode/x12decodeimage1.png) 

3. Ha nem korábban létre a kapcsolatokat tooyour integrációs fiókot, megkéri toocreate most, hogy a kapcsolat. A kapcsolat neve, és válassza ki, hogy szeretné-e tooconnect hello integrációs fiókot. 
   
    ![integráció fiók kapcsolat](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage1.png)

    Tulajdonságok csillaggal szükség.

    | Tulajdonság | Részletek |
    | --- | --- |
    | Kapcsolat neve * |Adjon meg egy tetszőleges nevet a kapcsolat. |
    | Integráció fiók * |Adja meg a integrációs fiók nevét. Győződjön meg arról, hogy az integrációs fiók és a logic app hello Azure ugyanazon a helyen. |

5.  Amikor elkészült, a kapcsolat adatai alábbihoz hasonló toothis példa. a kapcsolat létrehozása toofinish válassza **létrehozása**.

    ![integráció fiók kapcsolat létrehozása](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage2.png)

    A kapcsolat létrejön.

    ![integráció fiók kapcsolódási adatait.](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage3.png) 

#### <a name="encode-x12-messages-by-agreement-name"></a>Kódolja X12 szerződés neve üzeneteket

Ha úgy dönt, tooencode X12 üzenetek szerződés neve, nyissa meg a hello **X12 neve megállapodás** listában, adja meg vagy válassza ki a meglévő X12 szerződést. Adja meg a hello XML üzenet tooencode.

![Adja meg a X12 szerződés neve és az XML-üzenet tooencode](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage4.png)

#### <a name="encode-x12-messages-by-identities"></a>Kódolja X12 identitások üzenetek

Ha tooencode X12 üzenetek által identitások választja, adja meg hello a küldő azonosítója, a küldő minősítő, a fogadó azonosítója és a fogadó minősítő a X12 a szerződést. Válassza ki a hello XML üzenet tooencode.
   
![Adjon meg a küldő és fogadó identitásainak, válassza az XML-üzenet tooencode](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage5.png) 

## <a name="x12-encode-details"></a>X12 részletek kódolása

hello X12 Encode összekötő az alábbi feladatokat hajtja végre:

* A szerződés feloldási összekapcsolja a küldő és fogadó környezeti tulajdonságok.
* XML-kódolású üzenetekkel konvertálásakor EDI tranzakció halmazok hello interchange hello EDI-interchange rendezi sorba.
* Tranzakció set fejléc és pótkocsi szegmensek vonatkozik
* Az adatcsere ellenőrző szám, a vezérlő csoportszámmal és a tranzakció beállítása vezérlő száma minden kimenő adatcserét hoz létre
* A felváltja elválasztók hello hasznos adatban
* Érvényesíti EDI- és erőforráspartner jellemző tulajdonságok
  * A sémaérvényesítés hello tranzakció-set adatelemek üdvözlőüzenetére séma ellen
  * EDI érvényesítési tranzakció-set adatelemek végre.
  * Tranzakció-set adatelemek végre kiterjesztett érvényesítése
* A műszaki és/vagy funkcionális visszaigazolás-kérelmek (Ha be van állítva).
  * A műszaki visszaigazolás miatt érvényesítési fejléc állít elő. hello műszaki visszaigazolás jelentések hello állapot hello feldolgozási tartozó az adatcsere fejléc és hello cím címzett
  * A működési visszaigazolás miatt törzs érvényesítési állít elő. hello működési visszaigazolás jelentések minden közben hiba történt hello feldolgozása kapott dokumentum

## <a name="view-hello-swagger"></a>Nézet hello swagger
Lásd: hello [részletek swagger](/connectors/x12/). 

## <a name="next-steps"></a>Következő lépések
[További tudnivalók a vállalati integrációs csomag hello](logic-apps-enterprise-integration-overview.md "további információ a vállalati integrációs csomag") 

