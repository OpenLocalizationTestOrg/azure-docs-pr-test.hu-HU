---
title: aaaConnect tooDynamics 365 (online) az Azure Logic Apps |} Microsoft Docs
description: "Munkafolyamatokat logic app, Dynamics 365 (online) entitások hello hello Dynamics 365-összekötő által nyújtott API használatával kezelhető"
services: logic-apps
cloud: Azure Stack
author: Mattp123
manager: anneta
documentationcenter: 
tags: connectors
ms.assetid: 0dc2abef-7d2c-4a2d-87ca-fad21367d135
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2017
ms.author: matp; LADocs
ms.openlocfilehash: 183d7a6b8e5d2c0eecc70da0da3806e06c382df4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-toodynamics-365-from-logic-app-workflows"></a>TooDynamics 365 csatlakoztatja a logic app munkafolyamatok

A Logic Apps tooDynamics 365 (online) csatlakozhat, és hozzon létre hasznos üzleti flow-rekordok létrehozása, elemek frissítését vagy a rekordok listáját adja vissza. Hello Dynamics 365-összekötővel a következőket teheti:

* A Dynamics 365 (online) kap hello adatok alapján üzleti folyamat létrehozása.
* Amely válaszol, és ellenőrizze az egyéb műveletek lehetőség hello kimeneti műveleteket használni. Például frissítésekor elem Dynamics 365 (online), az Office 365-tel e-mailt küldhet.

Ez a témakör bemutatja, hogyan toocreate logikai alkalmazás, amely feladatot hoz létre a Dynamics 365 amikor létrejön egy új vezető Dynamics 365.

## <a name="prerequisites"></a>Előfeltételek
* Egy Azure-fiók.
* Dynamics 365 (online) fiók.

## <a name="create-a-task-when-a-new-lead-is-created-in-dynamics-365"></a>Hozzon létre egy feladatot, amikor egy új vezető Dynamics 365 hoz létre

1.  [Jelentkezzen be tooAzure](https://portal.azure.com).

2.  Hello Azure keresési mezőbe, írja be a `Logic apps`, és nyomja le az ENTER BILLENTYŰT.

      ![Logic Apps alkalmazások keresése](./media/connectors-create-api-crmonline/find-logic-apps.png)

3.  A **a Logic apps**, kattintson a **Hozzáadás**.

      ![LogicApp hozzáadása](./media/connectors-create-api-crmonline/add-logic-app.png)

4.  toocreate hello logic app, teljes hello **neve**, **előfizetés**, **erőforráscsoport**, és **hely** mezők, és kattintson a  **Hozzon létre**.

5.  Válassza ki az új logikai alkalmazás hello. Hello érkezésekor **sikeres telepítés** értesítést, kattintson a **frissítése**.

6.  A **Fejlesztőeszközök**, kattintson a **Logic App Designer**. Hello listájában kattintson **üres logikai alkalmazás**.

7.  Hello keresési mezőbe, írja be a `Dynamics 365`. A hello Dynamics 365 elindítja a listán, válassza a **Dynamics 365 – amikor létrejön egy bejegyzés**.

8.  Ha a tooDynamics 365 felszólító toosign, tegye meg.

9.  Hello eseményindító részleteit adja meg a következő információ hello:

  * **Szervezet neve**. Válassza ki, amelyet hello logic app toolisten való hello Dynamics 365 példányt.

  * **Entitás neve**. Válassza ki, amelyet a toolisten hello entitást. Ez az esemény úgy működik, mint egy eseményindító toostart hello logikai alkalmazást. 
  Ebben a bemutatóban **érdeklődők** van kiválasztva.

  * **Milyen gyakran szeretné toocheck elemek?** Ezek az értékek gyakoriságának beállítása hello logikai alkalmazás keres frissítéseket kapcsolódó toohello eseményindító. hello alapértelmezett érték három percenként frissítések toocheck.

    * **Gyakoriság**. Válassza ki a másodperc, perc, órák vagy napok.

    * **Időköz**. Adja meg másodpercben, perc, óra hello számát, vagy Ezután ellenőrizze, hogy szeretné-e előtt hello toopass nap.

      ![Logic App eseményindító részletei](./media/connectors-create-api-crmonline/trigger-details.png)

10. Kattintson a **új lépés**, és kattintson a **művelet hozzáadása**.

11. Hello keresési mezőbe, írja be a `Dynamics 365`. Hello műveletek listájából válassza ki **Dynamics 365 – új rekord létrehozása**.

12. Adja meg a következő információ hello:

    * **Szervezet neve**. Válassza ki a hello Dynamics 365-példányt, ahol hello folyamat toocreate hello rekord. 
    Figyelje meg, hogy ez a példány nem rendelkezik azonos hello toobe ahol hello esemény akkor váltódik ki, a példány.

    * **Entitás neve**. Válassza ki a megjeleníteni kívánt toocreate rekord hello esemény kiváltásakor hello entitást. 
    Ebben a bemutatóban **feladatok** van kiválasztva.

13. Kattintson a hello **tulajdonos** meg. Hello dinamikus tartalom megjelenő listában kiválaszthatja a mezők valamelyikét:

    * **Vezetéknév**. Hello vezetéknevet hello átfutási kiválasztása ebben a mezőben szúr be hello Tárgy mező hello feladathoz, hello feladat rekord létrehozásakor.
    * **A témakör**. A mező beszúrása hello témakör mezője hello átfutási hello tulajdonos mezőbe hello tevékenység kiválasztása esetén hello feladat rekord létrehozása esetén. 
    Kattintson a **témakör** tooadd adott toohello **tulajdonos** mezőbe.

      ![Logic App hozzon létre új rekord részletei](./media/connectors-create-api-crmonline/create-record-details.png)

14. Hello Logic App Designer eszköztáron kattintson **mentése**.

    ![Logic App Designer eszköztáron menteni](./media/connectors-create-api-crmonline/designer-toolbar-save.png)

15. toostart hello Logic App, kattintson a **futtatása**.

    ![Logic App Designer eszköztáron menteni](./media/connectors-create-api-crmonline/designer-toolbar-run.png)

16. Most hozzon létre rendszer Dynamics 365 az értékesítés, és tekintse meg a folyamat a művelet!

## <a name="set-advanced-options-for-a-logic-app-step"></a>A logic app lépés speciális beállításainak megadása

Hogyan toofilter adatok egy logic app lépésben kattintson toospecify **speciális beállítások megjelenítése** a lépés, majd adja hozzá egy szűrőt, vagy az order lekérdezésével.

Például egy szűrő lekérdezés tooget csak az aktív fiókok használata, és a rendezés hello fiók neve. tooperform ennek a feladatnak, adja meg az OData-szűrő lekérdezés hello `statuscode eq 1`, és válassza ki **fióknév** hello dinamikus tartalom listából. További információ: [MSDN: $filter](https://msdn.microsoft.com/library/gg309461.aspx#Anchor_1) és [$orderby](https://msdn.microsoft.com/library/gg309461.aspx#Anchor_2).

![Speciális beállítások logikai alkalmazás](./media/connectors-create-api-crmonline/advanced-options.png)

### <a name="best-practices-when-using-advanced-options"></a>Gyakorlati tanácsok a speciális beállításokkal

Érték tooa mező hozzáadásakor meg kell egyeznie hello mező típusa, írjon be egy értéket, vagy kiválaszthat egy értéket a hello dinamikus tartalom listából.

Mező típusa  |Hogyan toouse  |Ha toofind  |Név  |Adattípus  
---------|---------|---------|---------|---------
Szövegmező|Szövegmezők egyetlen sor szöveget vagy a dinamikus tartalmat, amely szöveges típusú mező szükséges. Például hello kategória és alkategória mezőket.|Beállítások > testreszabások > Testreszabás hello rendszer > entitások > Feladat > mezők |category |Egysoros szövegmező        
Egész mezők | Egyes mezők egész szám vagy a dinamikus tartalmat a mezőnek egész típus szükséges. Például készültségi szint és időtartama. |Beállítások > testreszabások > Testreszabás hello rendszer > entitások > Feladat > mezők |KészültségiSzint |Egész szám         
Dátummezők | Egyes mezők dátummal hh/nn/éééé formátumban vagy dinamikus tartalmat a dátum típusú mező szükséges. Ilyen például létrehozva, kezdő dátum, Tényleges kezdés utolsó tartsa idő, a tényleges befejezési és a határidő. | Beállítások > testreszabások > Testreszabás hello rendszer > entitások > Feladat > mezők |createdon |Dátum és idő
Írja be a szükséges mind a rekord azonosítója és a keresési mezők |Néhány mező egy másik entitás bejegyzés hivatkozó kötelező a hello Rekordazonosítója és hello keresési típus. |Beállítások > testreszabások > Testreszabás hello rendszer > entitások > fiók > mezők  | accountid  | Elsődleges kulcs

### <a name="more-examples-of-fields-that-require-both-a-record-id-and-lookup-type"></a>További példák a mezőket, amelyeknek szükséges a rekord azonosítója és a keresési írja be.
Bővíteni hello előző táblán, az alábbiakban további példák a mezőket, amelyeknek hello dinamikus tartalom listában kijelölt értékek nem működik. Ehelyett ezeket a mezőket kell mindkét egy rekord azonosítója és a keresési típusú hello mezőkbe a powerapps segítségével.  
* Tulajdonos és a tulajdonos típusa. hello tulajdonos írjon be egy érvényes felhasználó vagy csoport rekord. hello Tulajdonostípusa kell lennie, vagy **systemusers** vagy **csapatok**.
* Ügyfél és az ügyfél típusa. hello ügyfél mező kell egy érvényes fiókot vagy lépjen kapcsolatba a rekordazonosító. hello Tulajdonostípusa kell lennie, vagy **fiókok** vagy **névjegyek**.
* Valamint típus tekintetében. hello vonatkozó mező kell érvényes Rekordazonosítója, például egy fiók vagy lépjen kapcsolatba a rekordazonosító. hello vonatkozó típusnak kell lennie hello keresési hello rekord, például a **fiókok** vagy **névjegyek**.

hello következő feladat létrehozása művelet példakóddal felveheti egy fiók bejegyzést, amely megfelel a vonatkozó hello feladat mező toohello hozzáadná toohello Rekordazonosítója.

![Attribútumfolyam recordId, és írja be a fiók](./media/connectors-create-api-crmonline/recordid-type-account.png)

Ebben a példában is rendel hello feladat tooa adott felhasználó az hello felhasználói rekord azonosítója alapján.

![Attribútumfolyam recordId, és írja be a fiók](./media/connectors-create-api-crmonline/recordid-type-user.png)

egy olyan rekordot toofind tartozó azonosító, tekintse meg a következő szakasz hello: *található hello rekord azonosítója*

## <a name="find-hello-record-id"></a>Található hello rekord azonosítója

1. Nyissa meg a rekord, például egy fiók rekordot.

2. Hello műveletek eszköztáron kattintson **kiugró** ![felbukkanó rekord](./media/connectors-create-api-crmonline/popout-record.png).
Másik lehetőségként kattintson hello műveletek eszköztár toocopy hello teljes URL-címet az alapértelmezett e-mail programba **e-MAILT A hivatkozás**.

   hello Rekordazonosítója Between hello 7b és %7 %d karakter hello URL-kódolást jelenik meg.

   ![Attribútumfolyam recordId, és írja be a fiók](./media/connectors-create-api-crmonline/recordid.png)

## <a name="troubleshooting"></a>Hibaelhárítás
tootroubleshoot egy hibás lépés a logikai alkalmazás, hello hello esemény állapot részleteinek megtekintése.

1. A **Logic Apps**, válassza ki a Logic Apps alkalmazást, és kattintson a **áttekintése**. 

   hello összegzési területen látható, és futtassa hello állapot biztosít hello logikai alkalmazást. 

   ![Logikai alkalmazás futtatási állapot](./media/connectors-create-api-crmonline/tshoot1.png)

2. tooview további információ a bármely sikertelen fut, kattintson a sikertelen hello esemény. tooexpand egy hibás lépés, kattintson az adott lépésre.

   ![Bontsa ki a hibás lépést](./media/connectors-create-api-crmonline/tshoot2.png)

   hello lépés részletei jelennek meg, és segít a hello hello hiba okának elhárítása.

   ![Nem sikerült lépés részletei](./media/connectors-create-api-crmonline/tshoot3.png)

A logic apps hibaelhárítással kapcsolatos további információkért lásd: [logic app hibák diagnosztizálása](../logic-apps/logic-apps-diagnosing-failures.md).

## <a name="connector-specific-details"></a>Összekötő-specifikus részletei

Bármely eseményindítók és hello swagger definiált műveletek megtekintése, és semmilyen határnak hello a Lásd még: [connector részleteket](/connectors/crm/). 

## <a name="next-steps"></a>Következő lépések
Fedezze fel más rendelkezésre álló összekötők Logic Apps: hello a [API-k lista](apis-list.md).
