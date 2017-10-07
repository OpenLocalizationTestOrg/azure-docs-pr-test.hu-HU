---
title: "aaaDecode EDIFACT üzenetek - Azure Logic Apps |} Microsoft Docs"
description: "EDI érvényesítése és nyugtázását a hello EDIFACT üzenet dekóder hello vállalati integrációs csomagot az Azure Logic Apps a"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: 0e61501d-21a2-4419-8c6c-88724d346e81
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: 94faebdec4e4ffc8ad76ad1609495ddf9f002250
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="decode-edifact-messages-for-azure-logic-apps-with-hello-enterprise-integration-pack"></a>EDIFACT üzenetek dekódolási az Azure Logic Apps a hello vállalati integrációs csomag

Hello dekódolása EDIFACT-üzenet összekötővel EDI és a partner jellemző tulajdonságok ellenőrzése, felosztott kereszteződéseket tranzakciók be vagy teljes kereszteződéseket megőrzése és feldolgozott tranzakciók visszaigazoló üzenetet létrehozni. toouse ezt az összekötőt, hozzá kell adnia a Logic Apps alkalmazást az eseményindító meglévő hello összekötő tooan.

## <a name="before-you-start"></a>Előkészületek

Itt van szüksége hello elemeket:

* Az Azure-fiók; létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free)
* Egy [integrációs fiók](logic-apps-enterprise-integration-create-integration-account.md) , amely már definiált és az Azure-előfizetéshez társítva. Az integráció fiók toouse hello dekódolása EDIFACT üzenet csatlakozó kell rendelkeznie. 
* Legalább két [partnerek](logic-apps-enterprise-integration-partners.md) , amely már definiálva vannak az integráció-fiókban
* Egy [EDIFACT megállapodás](logic-apps-enterprise-integration-edifact.md) , amely már definiálva van az integráció-fiókban

## <a name="decode-edifact-messages"></a>EDIFACT üzenetek dekódolása

1. [Logikai alkalmazás létrehozása](logic-apps-create-a-logic-app.md).

2. hello dekódolása EDIFACT üzenet összekötő nincs eseményindítók, hozzá kell adni egy eseményindító indítása el a logikai alkalmazás, például a kérelem eseményindítót. A Logic App Designer hello adja hozzá egy eseményindító, és adja hozzá az egy művelet tooyour logikai alkalmazást.

3. Hello keresési mezőbe írja be a "EDIFACT" szűrőként. Válassza ki **EDIFACT üzenet dekódolása**.
   
    ![EDIFACT keresése](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage1.png)

3. Ha nem korábban létre a kapcsolatokat tooyour integrációs fiókot, megkéri toocreate most, hogy a kapcsolat. A kapcsolat neve, és válassza ki, hogy szeretné-e tooconnect hello integrációs fiókot.
   
    ![integráció-fiók létrehozása](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage2.png)

    Tulajdonságok csillaggal szükség.

    | Tulajdonság | Részletek |
    | --- | --- |
    | Kapcsolat neve * |Adjon meg egy tetszőleges nevet a kapcsolat. |
    | Integráció fiók * |Adja meg a integrációs fiók nevét. Győződjön meg arról, hogy az integrációs fiók és a logic app hello Azure ugyanazon a helyen. |

4. Amikor a kapcsolat létrehozása toofinish elkészült, válassza ki a **létrehozása**. A kapcsolat adatai alábbihoz hasonló toothis példa:

    ![integráció fiókadatok](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage3.png)  

5. Miután létrehozta a kapcsolatot, ebben a példában látható módon, válassza ki a hello EDIFACT egybesimított fájl üzenet toodecode.

    ![integráció fiók kapcsolat létrehozása](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage4.png)  

    Példa:

    ![EDIFACT egybesimított fájl üzenet dekódolását kiválasztása](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage5.png)  

## <a name="edifact-decoder-details"></a>EDIFACT dekóder részletei

hello dekódolása EDIFACT-összekötő az alábbi feladatokat hajtja végre: 

* Hello boríték kereskedelmipartner-egyezmény szemben érvényesíti.
* Oldja fel a hello megállapodás egyeztetésével hello küldő minősítő & azonosítója és a fogadó minősítő & azonosítója.
* Ha az hello interchange fogadni a konfigurációs beállítások több tranzakció hello megállapodás alapján felosztja a több tranzakcióra cseréje.
* Hello interchange visszafejti.
* Ellenőrzi az EDI és a partner jellemző tulajdonságokat, beleértve:
  * Hello interchange boríték struktúra érvényesítése
  * A sémaérvényesítés hello boríték hello vezérlő séma
  * A sémaérvényesítés hello tranzakció-set adatelemek hello üzenet séma
  * Tranzakció-set adatelemek végre EDI érvényesítése
* Ellenőrzi, hogy hello interchange, a csoport és a tranzakció set vezérlő számok nem ismétlődő (Ha be van állítva) 
  * Ellenőrzi, hello interchange ellenőrző szám korábban fogadott kereszteződéseket ellen. 
  * Hello csoport ellenőrző szám más hello interchange csoport vezérlő számoknak ellenőrzi. 
  * Ellenőrzi, hogy hello tranzakció beállítása vezérlő számoknak más tranzakció set vezérlő csoport.
* Felosztja a hello interchange tranzakció be, vagy megőrzi a teljes adatcsere hello:
  * Vegyes Interchange, tranzakció készletek - tranzakció készletek felfüggeszteni hiba: elágazást interchange tranzakcióban állítja be, és minden tranzakció set elemzi. 
  hello X12 dekódolási művelet kimenetében csak tranzakció ezekről érvényesítése túl teljesítő`badMessages`, és kiírja a fennmaradó tranzakciók hello beállítása túl`goodMessages`.
  * Vegyes Interchange, tranzakció készletek - interchange felfüggeszteni hiba: elágazást interchange tranzakcióban állítja be, és minden tranzakció set elemzi. 
  Ha egy vagy több tranzakció hello interchange sikertelen érvényesítési állítja, hello X12 dekódolási művelet kimenetében összes hello tranzakció túl beállítja, hogy interchange`badMessages`.
  * Megőrizheti a adatcsere - tranzakció készletek felfüggeszteni hiba: Preserve hello adatcsere és a folyamat hello teljes kötegelt adatcserét. 
  hello X12 dekódolási művelet kimenetében csak tranzakció ezekről érvényesítése túl teljesítő`badMessages`, és kiírja a fennmaradó tranzakciók hello beállítása túl`goodMessages`.
  * Megőrizheti a adatcsere - interchange felfüggeszteni hiba: Preserve hello adatcsere és a folyamat hello teljes kötegelt adatcserét. 
  Ha egy vagy több tranzakció hello interchange sikertelen érvényesítési állítja, hello X12 dekódolási művelet kimenetében összes hello tranzakció túl beállítja, hogy interchange`badMessages`.
* (Ha be van állítva) hoz létre egy műszaki (vezérlő) és/vagy a működési visszaigazolás.
  * Egy technikai nyugtázások vagy hello CONTRL ACK jelentések hello teljes kapott interchange szintaktikai ellenőrzése hello eredményeit.
  * A működési visszaigazolás elismeri elfogadására vagy elutasítására fogadott interchange vagy felhasználói csoport

## <a name="view-swagger-file"></a>A Swagger-fájl megtekintése
tooview hello Swagger részletek hello EDIFACT-összekötőhöz: [EDIFACT](/connectors/edifact/).

## <a name="next-steps"></a>Következő lépések
[További tudnivalók a vállalati integrációs csomag hello](logic-apps-enterprise-integration-overview.md "további információ a vállalati integrációs csomag") 

