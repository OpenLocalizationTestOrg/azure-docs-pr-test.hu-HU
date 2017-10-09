---
title: "aaaCreate B2B megoldások - Azure Logic Apps |} Microsoft Docs"
description: "Adatfogadás logic Apps alkalmazásokat a vállalati integrációs csomag hello hello B2B szolgáltatások segítségével"
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: anneta
editor: cgronlun
ms.assetid: 20fc3722-6f8b-402f-b391-b84e9df6fcff
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: 8f01318a0415d81c37b216f9b991c060edec2053
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="receive-data-in-logic-apps-with-hello-b2b-features-in-hello-enterprise-integration-pack"></a>Adatfogadás a logic apps funkcióival hello B2B hello vállalati integrációs csomag

Miután létrehozta a partnerek és megállapodások integrációs fiókkal, készen áll a toocreate üzleti toobusiness (B2B) munkamenet-e a logikai alkalmazásnak a hello [vállalati integrációs csomag](logic-apps-enterprise-integration-overview.md).

## <a name="prerequisites"></a>Előfeltételek

toouse hello AS2 és X12 műveleteket, rendelkeznie kell egy vállalati integrációs fiók. Ismerje meg, [hogyan toocreate vállalati integrációs fiók](../logic-apps/logic-apps-enterprise-integration-accounts.md).

## <a name="create-a-logic-app-with-b2b-connectors"></a>Hozzon létre egy logic app B2B összekötők

Kövesse az alábbi lépéseket toocreate a B2B logikai alkalmazás hello AS2 és X12 használó műveletek tooreceive adatait egy kereskedelmi partner:

1. Hozzon létre egy logikai alkalmazást, majd [a app tooyour integrációs fiókját](../logic-apps/logic-apps-enterprise-integration-accounts.md).

2. Adja hozzá a **kérelem - amikor egy HTTP-kérelem érkezik** eseményindító tooyour logikai alkalmazást.

    ![](./media/logic-apps-enterprise-integration-b2b/flatfile-1.png)

3. tooadd hello **dekódolása AS2** művelet, jelölje be **művelet hozzáadása**.

    ![](./media/logic-apps-enterprise-integration-b2b/transform-2.png)

4. toofilter összes műveletek toohello egy használni kívánt, adja meg a hello word **as2** hello Keresés mezőbe.

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-5.png)

5. Jelölje be hello **AS2 - dekódolási AS2 üzenet** művelet.

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-6.png)

6. Adja hozzá a hello **törzs** , amelyet az toouse bemeneti adatként. Ebben a példában válassza ki, hogy az eseményindítók hello logikai alkalmazás hello HTTP-kérelem hello törzsét. Adjon meg egy kifejezést, amely a bemeneti hello fejlécének hello vagy **FEJLÉCEK** mező:

    @triggerOutputs(["fejléc"])

7. Adja hozzá a szükséges hello **fejlécek** az AS2, amely hello HTTP-kérelemfejlécekben található. Ebben a példában válassza hello fejléceket a HTTP-kérelem hello eseményindító hello logikai alkalmazást.

8. Mostantól hozzáadhatja azok hello dekódolási X12 üzenet művelet. Válassza ki **művelet hozzáadása**.

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-9.png)

9. toofilter összes műveletek toohello egy használni kívánt, adja meg a hello word **x12** hello Keresés mezőbe.

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-10.png)

10. Jelölje be hello **X12-dekódolási X12 üzenet** művelet.

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-as2message.png)

11. Most meg kell adnia hello bemeneti toothis műveletet. A bemeneti érték hello előző AS2 művelet hello kimenetét.

    hello tényleges üzenet tartalma egy JSON-objektum és base64-kódolású, ezért meg kell adnia egy kifejezés hello bemeneti adatként. 
    Adja meg a következő kifejezés hello hello **X12 lapos fájl üzenet tooDECODE** beviteli mezőt:
    
    @base64ToString(body('Decode_AS2_message')? ["AS2Message']? ["Tartalom"])

    Most vegyen fel olyan lépéseket toodecode hello X12 adatok kereskedelmi partner hello kapott, és a kimeneti JSON-objektum elemek. 
    adatok hello toonotify hello partner érkezett, küldhet vissza a válaszban hello AS2 üzenet törlése értesítés (MDN) a HTTP-válasz művelethez.

12. tooadd hello **válasz** művelet, válassza a **művelet hozzáadása**.

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-14.png)

13. toofilter összes műveletek toohello egy használni kívánt, adja meg a hello word **válasz** hello Keresés mezőbe.

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-15.png)

14. Jelölje be hello **válasz** művelet.

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-16.png)

15. tooaccess MDN hello hello hello kimenetét a **dekódolási X12 üzenet** művelet, a set hello válasz **törzs** mezőt a ebben a kifejezésben:

    @base64ToString(body('Decode_AS2_message')? ["OutgoingMdn']? ["Tartalom"])

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-17.png)  

16. Mentse a munkáját.

    ![](./media/logic-apps-enterprise-integration-b2b/transform-5.png)  

Most már befejezte a B2B logikai alkalmazás beállítása. Egy valós alkalmazás érdemes toostore hello dekódolni X12 adatok-üzletági (LOB) alkalmazás vagy adatok tárolóban. tooconnect a saját ÜZLETÁGI alkalmazásai és ezen API-k használata a Logic Apps alkalmazást, vagy adhat hozzá további műveletek írása egyéni API-k.

## <a name="features-and-use-cases"></a>Szolgáltatások és a használati esetek

* hello AS2 és X12 dekódolni, és műveletek kódolása lehetővé, hogy adatcserében működik közre a logic apps iparági szabványos protokollok segítségével kereskedelmi partnerek között.
* tooexchange adatok az üzleti partnerekkel való, használható AS2 és X12 vagy anélkül egymással.
* hello B2B műveletek segítséget nyújt a partnerek és a megállapodások az integráció fiókban könnyen létrehozását és felhasználását azokat a logikai alkalmazás.
* A Logic Apps alkalmazást az egyéb műveletek bővítésekor küldhet és fogadhat adatokat más alkalmazásokat és szolgáltatásokat, mint a SalesForce között.

## <a name="learn-more"></a>Részletek
[További tudnivalók hello vállalati integrációs csomag](logic-apps-enterprise-integration-overview.md)
