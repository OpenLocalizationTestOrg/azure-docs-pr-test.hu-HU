---
title: "aaaEncode AS2 üzenetek - Azure Logic Apps |} Microsoft Docs"
description: "Hogyan toouse hello AS2 kódoló hello vállalati integrációs csomagot az Azure Logic Apps"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: 332fb9e3-576c-4683-bd10-d177a0ebe9a3
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: 2b372c416512ffa9ea5dc50ce0f767bfd8aefbc4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="encode-as2-messages-for-azure-logic-apps-with-hello-enterprise-integration-pack"></a>AS2 üzenetek kódolása az Azure Logic Apps a hello vállalati integrációs csomag

tooestablish biztonsága és megbízhatósága üzenetek átvitele közben hello kódolása AS2 üzenet összekötő használatára. Ez az összekötő digitális aláírására, a titkosítás és a nyugtázásokhoz keresztül üzenet törlése értesítések (MDN), ami is toosupport a Megtagadás nem biztosít.

## <a name="before-you-start"></a>Előkészületek

Itt van szüksége hello elemeket:

* Az Azure-fiók; létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free)
* Egy [integrációs fiók](logic-apps-enterprise-integration-create-integration-account.md) , amely már definiált és az Azure-előfizetéshez társítva. Az integráció fiók toouse hello kódolása AS2 üzenet csatlakozó kell rendelkeznie.
* Legalább két [partnerek](logic-apps-enterprise-integration-partners.md) , amely már definiálva vannak az integráció-fiókban
* Egy [AS2 megállapodás](logic-apps-enterprise-integration-as2.md) , amely már definiálva van az integráció-fiókban

## <a name="encode-as2-messages"></a>AS2-üzenetek kódolása

1. [Logikai alkalmazás létrehozása](logic-apps-create-a-logic-app.md).

2. hello kódolása AS2-üzenet összekötő nincs eseményindítók, hozzá kell adni egy eseményindító indítása el a logikai alkalmazás, például a kérelem eseményindítót. A Logic App Designer hello adja hozzá egy eseményindító, és adja hozzá az egy művelet tooyour logikai alkalmazást.

3.  Hello keresési mezőbe írja be a "AS2" a szűrőhöz. Válassza ki **AS2 - Encode AS2 üzenet**.
   
    ![Keresse meg a "AS2"](./media/logic-apps-enterprise-integration-as2-encode/as2decodeimage1.png)

4. Ha nem korábban létre a kapcsolatokat tooyour integrációs fiókot, megkéri toocreate most, hogy a kapcsolat. A kapcsolat neve, és válassza ki, hogy szeretné-e tooconnect hello integrációs fiókot. 
   
    ![kapcsolat toointegration-fiók létrehozása](./media/logic-apps-enterprise-integration-as2-encode/as2encodeimage1.png)  

    Tulajdonságok csillaggal szükség.

    | Tulajdonság | Részletek |
    | --- | --- |
    | Kapcsolat neve * |Adjon meg egy tetszőleges nevet a kapcsolat. |
    | Integráció fiók * |Adja meg a integrációs fiók nevét. Győződjön meg arról, hogy az integrációs fiók és a logic app hello Azure ugyanazon a helyen. |

5.  Amikor elkészült, a kapcsolat adatai alábbihoz hasonló toothis példa. a kapcsolat létrehozása toofinish válassza **létrehozása**.
   
    ![integráció kapcsolódási adatait.](./media/logic-apps-enterprise-integration-as2-encode/as2encodeimage2.png)

6. Miután létrehozta a kapcsolatot, ebben a példában látható módon, adja meg a részletes **AS2-a**, **AS2-tooidentifiers** be a szerződés és **törzs**, amely hello üzenetadatokat.
   
    ![Adja meg a kötelező mezők](./media/logic-apps-enterprise-integration-as2-encode/as2encodeimage3.png)

## <a name="as2-encoder-details"></a>AS2-kódoló részletei

hello kódolása AS2-összekötő az alábbi feladatokat hajtja végre: 

* AS2/HTTP-fejlécek vonatkozik
* A jelek kimenő üzenetek (Ha be van állítva)
* Kimenő üzenetek titkosítja (Ha be van állítva)
* Tömöríti a üdvözlőüzenetére (Ha be van állítva)

## <a name="try-this-sample"></a>Ismételje meg ezt a mintát

tootry forgatókönyv üzembe helyezését egy teljesen működőképes logic app és minta AS2, lásd: hello [AS2 logic app-sablon és a forgatókönyv](https://azure.microsoft.com/documentation/templates/201-logic-app-as2-send-receive/).

## <a name="view-hello-swagger"></a>Nézet hello swagger
Lásd: hello [részletek swagger](/connectors/as2/). 

## <a name="next-steps"></a>Következő lépések
[További tudnivalók a vállalati integrációs csomag hello](logic-apps-enterprise-integration-overview.md "további információ a vállalati integrációs csomag") 

