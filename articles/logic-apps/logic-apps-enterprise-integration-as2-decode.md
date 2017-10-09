---
title: "aaaDecode AS2 üzenetek - Azure Logic Apps |} Microsoft Docs"
description: "Hogyan toouse hello AS2 dekóder hello vállalati integrációs csomagot az Azure Logic Apps"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: cf44af18-1fe5-41d5-9e06-cc57a968207c
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: 2406e5ec68e0906700fad97d60cb83ef0d106cd6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="decode-as2-messages-for-azure-logic-apps-with-hello-enterprise-integration-pack"></a>AS2-üzenetek dekódolási az Azure Logic Apps a hello vállalati integrációs csomag 

tooestablish biztonsága és megbízhatósága üzenetek átvitele közben hello dekódolása AS2 üzenet összekötő használatára. Ez az összekötő biztosít digitális aláírására, visszafejtéshez és nyugták üzenet törlése értesítések (MDN) keresztül.

## <a name="before-you-start"></a>Előkészületek

Itt van szüksége hello elemeket:

* Az Azure-fiók; létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free)
* Egy [integrációs fiók](logic-apps-enterprise-integration-create-integration-account.md) , amely már definiált és az Azure-előfizetéshez társítva. Az integráció fiók toouse hello dekódolása AS2 üzenet csatlakozó kell rendelkeznie.
* Legalább két [partnerek](logic-apps-enterprise-integration-partners.md) , amely már definiálva vannak az integráció-fiókban
* Egy [AS2 megállapodás](logic-apps-enterprise-integration-as2.md) , amely már definiálva van az integráció-fiókban

## <a name="decode-as2-messages"></a>AS2-üzenetek dekódolása

1. [Logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).

2. hello dekódolása AS2-üzenet összekötő nincs eseményindítók, hozzá kell adni egy eseményindító indítása el a logikai alkalmazás, például a kérelem eseményindítót. A Logic App Designer hello adja hozzá egy eseményindító, és adja hozzá az egy művelet tooyour logikai alkalmazást.

3.  Hello keresési mezőbe írja be a "AS2" a szűrőhöz. Válassza ki **AS2 - dekódolási AS2 üzenet**.
   
    ![Keresse meg a "AS2"](media/logic-apps-enterprise-integration-as2-decode/as2decodeimage1.png)

4. Ha nem korábban létre a kapcsolatokat tooyour integrációs fiókot, megkéri toocreate most, hogy a kapcsolat. A kapcsolat neve, és válassza ki, hogy szeretné-e tooconnect hello integrációs fiókot.
   
    ![Integráció kapcsolat létrehozása](media/logic-apps-enterprise-integration-as2-decode/as2decodeimage2.png)

    Tulajdonságok csillaggal szükség.

    | Tulajdonság | Részletek |
    | --- | --- |
    | Kapcsolat neve * |Adjon meg egy tetszőleges nevet a kapcsolat. |
    | Integráció fiók * |Adja meg a integrációs fiók nevét. Győződjön meg arról, hogy az integrációs fiók és a logic app hello Azure ugyanazon a helyen. |

5.  Amikor elkészült, a kapcsolat adatai alábbihoz hasonló toothis példa. a kapcsolat létrehozása toofinish válassza **létrehozása**.

    ![integráció kapcsolódási adatait.](media/logic-apps-enterprise-integration-as2-decode/as2decodeimage3.png)

6. A kapcsolat létrehozása után ebben a példában látható módon, válassza ki a **törzs** és **fejlécek** hello a kérelem kimenetek.
   
    ![integráció kapcsolat létrehozása](media/logic-apps-enterprise-integration-as2-decode/as2decodeimage4.png) 

    Példa:

    ![Válassza ki a szervezet és a fejlécek a kérelem kimenetek](media/logic-apps-enterprise-integration-as2-decode/as2decodeimage5.png) 

## <a name="as2-decoder-details"></a>AS2 dekóder részletei

hello dekódolása AS2-összekötő az alábbi feladatokat hajtja végre: 

* Feldolgozza a AS2/HTTP-fejlécek
* Ellenőrzi a hello aláírás (Ha be van állítva)
* Visszafejti köszönőüzenetei (Ha be van állítva)
* Kibontja a üdvözlőüzenetére (Ha be van állítva)
* Egyezteti a fogadott MDN hello eredeti kimenő üzenet
* Frissíti, és korrelálja hello nem megtagadás adatbázis
* Írja a rekordokat az AS2 állapotjelentés
* hello kimeneti hasznos tartalma base64-kódolású
* Beállítja, hogy egy MDN szükség, hogy hello MDN kell lennie a szinkron vagy aszinkron konfigurációtól függően az AS2 egyezményben
* Létrehoz egy szinkron vagy aszinkron MDN (megállapodás konfigurációk alapján)
* Hello MDN hello korrelációs jogkivonat és a tulajdonságok beállítása

## <a name="try-this-sample"></a>Ismételje meg ezt a mintát

tootry forgatókönyv üzembe helyezését egy teljesen működőképes logic app és minta AS2, lásd: hello [AS2 logic app-sablon és a forgatókönyv](https://azure.microsoft.com/documentation/templates/201-logic-app-as2-send-receive/).

## <a name="view-hello-swagger"></a>Nézet hello swagger
Lásd: hello [részletek swagger](/connectors/as2/). 

## <a name="next-steps"></a>Következő lépések
[További tudnivalók hello vállalati integrációs csomag](logic-apps-enterprise-integration-overview.md) 

