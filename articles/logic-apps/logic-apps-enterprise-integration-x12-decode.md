---
title: "aaaDecode X12 üzenetek - Azure Logic Apps |} Microsoft Docs"
description: "EDI érvényesítése és nyugtázását a hello X12 üzenet dekóder hello vállalati integrációs csomagot az Azure Logic Apps a"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: 4fd48d2d-2008-4080-b6a1-8ae183b48131
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: 1ffececca1ff835b319b64c85f86c421395833c8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="decode-x12-messages-for-azure-logic-apps-with-hello-enterprise-integration-pack"></a>Dekódolás X12 hello vállalati integrációs csomagot az Azure Logic Apps állapotüzenete

Hello dekódolási X12 üzenet összekötővel hello boríték elleni kereskedelmipartner-egyezmény érvényesítése, EDI és a partner jellemző tulajdonságok ellenőrzése, felosztott kereszteződéseket tranzakciók be vagy teljes kereszteződéseket megőrzése, és beállíthatja készítése feldolgozott tranzakciók visszaigazoló üzenetet. toouse ezt az összekötőt, hozzá kell adnia a Logic Apps alkalmazást az eseményindító meglévő hello összekötő tooan.

## <a name="before-you-start"></a>Előkészületek

Itt van szüksége hello elemeket:

* Az Azure-fiók; létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free)
* Egy [integrációs fiók](logic-apps-enterprise-integration-create-integration-account.md) , amely már definiált és az Azure-előfizetéshez társítva. Az integráció fiók toouse hello dekódolási X12 üzenet csatlakozó kell rendelkeznie.
* Legalább két [partnerek](logic-apps-enterprise-integration-partners.md) , amely már definiálva vannak az integráció-fiókban
* Egy [X12 megállapodás](logic-apps-enterprise-integration-x12.md) , amely már definiálva van az integráció-fiókban

## <a name="decode-x12-messages"></a>Dekódolás X12 üzenetek

1. [Logikai alkalmazás létrehozása](logic-apps-create-a-logic-app.md).

2. hello dekódolási X12 üzenet összekötő eseményindítókat, nem rendelkezik, hozzá kell adni egy eseményindító indítása el a logikai alkalmazás, például egy kérelem eseményindító. A Logic App Designer hello adja hozzá egy eseményindító, és adja hozzá az egy művelet tooyour logikai alkalmazást.

3.  Hello keresési mezőbe írja be a "x12" a szűrőhöz. Válassza ki **X12-dekódolási X12 üzenet**.
   
    ![Keressen a "x12"](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage1.png)  

3. Ha nem korábban létre a kapcsolatokat tooyour integrációs fiókot, megkéri toocreate most, hogy a kapcsolat. A kapcsolat neve, és válassza ki, hogy szeretné-e tooconnect hello integrációs fiókot. 

    ![Adja meg az integrációs fiók kapcsolódási adatait.](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage4.png)

    Tulajdonságok csillaggal szükség.

    | Tulajdonság | Részletek |
    | --- | --- |
    | Kapcsolat neve * |Adjon meg egy tetszőleges nevet a kapcsolat. |
    | Integráció fiók * |Adja meg a integrációs fiók nevét. Győződjön meg arról, hogy az integrációs fiók és a logic app hello Azure ugyanazon a helyen. |

5.  Amikor elkészült, a kapcsolat adatai alábbihoz hasonló toothis példa. a kapcsolat létrehozása toofinish válassza **létrehozása**.
   
    ![integráció fiók kapcsolódási adatait.](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage5.png) 

6. Miután létrehozta a kapcsolatot, ebben a példában látható módon, válassza ki a hello X12 egybesimított fájl üzenet toodecode.

    ![integráció fiók kapcsolat létrehozása](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage6.png) 

    Példa:

    ![Jelölje be X12 lapos fájl üzenet dekódolását](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage7.png) 

## <a name="x12-decode-details"></a>X12 dekódolása részletei

hello X12 dekódolási összekötő az alábbi feladatokat hajtja végre:

* Kereskedelmipartner-egyezmény elleni hello boríték ellenőrzi
* Érvényesíti EDI- és erőforráspartner jellemző tulajdonságok
  * EDI strukturális érvényesítése és a bővített sémaérvényesítése
  * Hello interchange boríték hello szerkezete érvényesítése.
  * A sémaérvényesítés hello boríték hello vezérlő sémának.
  * A sémaérvényesítés hello tranzakció-set adatelemek hello üzenet sémának.
  * Tranzakció-set adatelemek végre EDI érvényesítése 
* Ellenőrzi, hogy hello interchange, a csoport és a tranzakció set vezérlő számok nem ismétlődő
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
* Létrehozza a műszaki és/vagy funkcionális visszaigazolás (Ha be van állítva).
  * A műszaki visszaigazolás miatt érvényesítési fejléc állít elő. hello műszaki visszaigazolás jelentések hello állapota hello feldolgozási tartozó az adatcsere fejléc és hello cím címzett.
  * A működési visszaigazolás miatt törzs érvényesítési állít elő. hello működési visszaigazolás jelentések minden közben hiba történt hello feldolgozása kapott dokumentum

## <a name="view-hello-swagger"></a>Nézet hello swagger
Lásd: hello [részletek swagger](/connectors/x12/). 

## <a name="next-steps"></a>Következő lépések
[További tudnivalók a vállalati integrációs csomag hello](../logic-apps/logic-apps-enterprise-integration-overview.md "további információ a vállalati integrációs csomag") 

