---
title: "egy csoport vagy a gyűjtemény - Azure Logic Apps aaaBatch folyamat üzenetek |} Microsoft Docs"
description: "A kötegelt feldolgozáson a logic apps üzeneteket küldjön és fogadjon"
keywords: "kötegelt, kötegelt folyamat"
author: jonfancey
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/7/2017
ms.author: LADocs; estfan; jonfan
ms.openlocfilehash: 2603db71ee0659d5b6bf5ce3d32f1b0d13c34194
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="send-receive-and-batch-process-messages-in-logic-apps"></a>Küldése, fogadására és a batch-folyamat üzenetek a logic Apps alkalmazások

tooprocess üzenetek csoportokban együtt, küldhet adatokat elemek vagy az üzenetek, tooa *kötegelt*, és majd dolgozni elemeket a köteg. Ezt a módszert akkor hasznos, ha azt szeretné, hogy toomake meg arról, hogy az elemeket a meghatározott módon vannak csoportosítva, és dolgoznak együtt. 

A logic apps elemek fogadó kötegként hello segítségével hozhat létre **kötegelt** eseményindító. A logic apps által küldött elemek tooa kötegelt hello segítségével is létrehozhat **kötegelt** művelet.

Ez a témakör bemutatja, hogyan hozhat létre egy kötegelési megoldás ezen feladatok végrehajtásával: 

* [Fogadó és elemek gyűjt kötegként logikai alkalmazás létrehozása](#batch-receiver). A "batch fogadó" logikai alkalmazás hello kötegelt nevét és verzióját leíró feltételeket toomeet határozza meg, mielőtt hello fogadó logikai alkalmazás kiadja, és feldolgozza elemeket. 

* [Hozzon létre egy logikai alkalmazás által küldött elemek tooa kötegelt](#batch-sender). A "batch küldő" logikai alkalmazás meghatározza, hogy hol toosend elemek, amely lehet egy meglévő kötegelt fogadó logikai alkalmazást. Is adjon meg egy egyedi kulcs, például egy ügyfél száma, túl "partíció", vagy ossza, hello cél kötegelt kulcs alapján részekre. Így minden elem ezzel a kulccsal összegyűjti és feldolgozza együtt. 

## <a name="requirements"></a>Követelmények

toofollow ebben a példában a következőkre van szükség:

* Azure-előfizetés. Ha nem rendelkezik előfizetéssel, [kezdhet egy ingyenes Azure-fiókkal](https://azure.microsoft.com/free/). Egyéb esetben [regisztrálhat használatalapú fizetéses előfizetésre](https://azure.microsoft.com/pricing/purchase-options/).

* Alapszintű ismerete [hogyan toocreate logic Apps alkalmazások](../logic-apps/logic-apps-create-a-logic-app.md) 

* Az összes e-mail fiókot [Azure Logic Apps által támogatott e-mail szolgáltató](../connectors/apis-list.md)

<a name="batch-receiver"></a>

## <a name="create-logic-apps-that-receive-messages-as-a-batch"></a>Üzenetek fogadása egy kötegelt logic Apps-alkalmazások létrehozása

Üzenetek tooa kötegelt elküldése előtt, előbb létre kell hoznia egy "batch fogadó" logikai alkalmazás hello **kötegelt** eseményindító. Ezzel a módszerrel választhat a fogadó logikai alkalmazás hello küldő logikai alkalmazás létrehozásakor. Hello fogadó megadhatja, hello neve, a kiadási feltételek és az egyéb beállításokat. 

Küldő logic apps kell tudnia, ahol toosend elem, amíg a fogadó logic Apps alkalmazásokat nem kell semmi kapcsolatos hello feladók tooknow.

1. A hello [Azure-portálon](https://portal.azure.com), logikai alkalmazás létrehozása ezen a néven: "BatchReceiver" 

2. A Logic Apps Designer, adja hozzá a hello **kötegelt** eseményindító, amely elindítja a logic app munkafolyamatot. Hello keresési mezőbe írja be a "batch" szűrőként. Válassza ki az ehhez az eseményindítóhoz: **kötegelt – kötegelt üzenetek**

   ![Kötegelt eseményindító hozzáadása](./media/logic-apps-batch-process-send-receive-messages/add-batch-receiver-trigger.png)

3. Adjon meg egy nevet hello kötegelt, és a feltételek megadása hello kötegelt, például feloldása:

   * **A Batch-név**: hello használt név tooidentify hello kötegelt, amely "TestBatch" Ebben a példában.
   * **Üzenetek száma**: hello toohold üzenetek száma a kötegelt feldolgozásra, amely "5" Ebben a példában feloldása előtt.

   ![Kötegelt eseményindító részleteinek megadása](./media/logic-apps-batch-process-send-receive-messages/receive-batch-trigger-details.png)

4. Adja hozzá egy másik művelet, amely egy e-mailt küld, amikor hello kötegelt eseményindító következik be. Minden alkalommal hello kötegelt öt elemek, a hello logikai alkalmazás elküld egy e-mailt.

   1. Hello kötegelt eseményindító, válassza ki **+ új lépés** > **művelet hozzáadása**.

   2. Hello keresési mezőbe írja be az "e-mail" szűrőként.
   Az e-mailt provider alapján, válassza ki az e-mailek csatlakozó.
   
      Például ha egy munkahelyi vagy iskolai fiókjával rendelkezik, válassza ki az hello Office 365 Outlook összekötő. 
      Ha Gmail fiókkal rendelkezik, válassza ki a hello Gmail összekötőt.

   3. Válassza ki a művelet az összekötőhöz:  **{*e-mailt provider*}-küldjön egy e-mailek **

      Példa:

      ![Válassza ki az e-mailt Provider "E-mail küldési" műveletet](./media/logic-apps-batch-process-send-receive-messages/add-send-email-action.png)

5. Ha a kapcsolat korábban az e-mailek szolgáltató nem hozott létre, e-mailek hitelesítő adatok megadása a hitelesítéshez, ha a rendszer kéri. További információ [az e-mailek hitelesítő adatok hitelesítése](../logic-apps/logic-apps-create-a-logic-app.md).

6. Az előzőekben adott hozzá hello művelet hello tulajdonságainak beállítása.

   * A hello **való** hello címzett e-mail címét adja meg. 
   Tesztelési célokra használhatja az e-mail címét.

   * A hello **tulajdonos** mezőben, ha hello **dinamikus tartalom** megjelenik a listán, válassza ki a hello **partíciónév** mező.

     ![Hello "Dinamikus tartalom" listából válassza a "Partíció neve"](./media/logic-apps-batch-process-send-receive-messages/send-email-action-details.png)

     Egy későbbi szakasz ismerteti megadhatja, hogy felosztása a logikai cél kötegelt hello egyedi partíciókulcsot beállítja toowhere küldhet üzeneteket. 
     Minden hello küldő logikai alkalmazás által létrehozott egyedi szám van. 
     A funkció lehetővé teszi a kötegek használata több részhalmazának, és adja meg a hello névvel, Ön által biztosított minden részhalmaza.

   * A hello **törzs** mezőben, ha hello **dinamikus tartalom** megjelenik a listán, válassza ki a hello **üzenet azonosítója** mező.

     ![A "Törzs" válassza a "Üzenet azonosítója"](./media/logic-apps-batch-process-send-receive-messages/send-email-action-details-for-each.png)

     Mivel hello hello küldési e-mail művelet a megadott tömb, hello designer automatikusan hozzáadja a **minden** körül hello hurok **egy e-mailek küldése** művelet. 
     Ez a ciklus egyes elemek hello kötegben hello belső művelet végez. 
     Igen hello kötegelt eseményindító set toofive elemekhez, kapott öt e-mailek, minden alkalommal hello eseményindító következik be.

7.  Most, hogy a létrehozott logikai alkalmazás kötegelt fogadó, mentse a Logic Apps alkalmazást.

    ![A logikai alkalmazás mentése](./media/logic-apps-batch-process-send-receive-messages/save-batch-receiver-logic-app.png)

<a name="batch-sender"></a>

## <a name="create-logic-apps-that-send-messages-tooa-batch"></a>A logic apps által küldött üzenetek tooa kötegelt létrehozása

Most hozzon létre egy vagy több logikai alkalmazások által küldött hello fogadó logikai alkalmazás által meghatározott elemek toohello kötegelt. Hello küldő megadhatja a hello fogadó logikai alkalmazás és a neve, üzenet tartalma és egyéb beállításokat. Is megadhat egy egyedi partíciós kulcs toodivide hello kötegelt részhalmazának toocollect elemek az ezzel a kulccsal.

Küldő logic apps kell tudnia, ahol toosend elem, amíg a fogadó logic Apps alkalmazásokat nem kell semmi kapcsolatos hello feladók tooknow.

1. Egy másik logikai alkalmazás létrehozása ezen a néven: "BatchSender"

   1. Hello keresési mezőbe írja be a "ismétlődési" szűrőként. 
   Válassza ki az ehhez az eseményindítóhoz: **ütemezés - ismétlődési**

      ![Hello "Ismétlődés ütemezése" eseményindító hozzáadása](./media/logic-apps-batch-process-send-receive-messages/add-schedule-trigger-batch-receiver.png)

   2. Állítsa be hello gyakoriságát és időköz toorun hello küldő logikai alkalmazás percenként.

      ![Gyakoriság és eseményindító ismétlődési időköz beállítása](./media/logic-apps-batch-process-send-receive-messages/recurrence-trigger-batch-receiver-details.png)

2. Adjon hozzá egy új lépést tooa kötegelt üzenetek küldéséhez.

   1. Hello ismétlődési eseményindító, válassza ki **+ új lépés** > **művelet hozzáadása**.

   2. Hello keresési mezőbe írja be a "batch" szűrőként. 

   3. Ez a művelet kiválasztása: **toobatch üzenetek küldése – a Logic Apps-munkafolyamat kötegelt eseményindító kiválasztása**

      ![Válassza a "Küldési üzenetek toobatch"](./media/logic-apps-batch-process-send-receive-messages/send-messages-batch-action.png)

   4. Most válassza a "BatchReceiver" logikai alkalmazás, amely korábban hozott létre, amely csomópontként jelenik meg a műveletet.

      ![Válassza ki a "batch fogadó" logikai alkalmazás](./media/logic-apps-batch-process-send-receive-messages/send-batch-select-batch-receiver.png)

      > [!NOTE]
      > hello lista is mutatja az más logikai alkalmazások kötegelt eseményindítók.

3. Hello kötegelt tulajdonságainak beállítása.

   * **A Batch-név**: hello hello fogadó logikai alkalmazás, amely "TestBatch" Ebben a példában, és futásidőben érvényesíti által definiált neve.

     > [!IMPORTANT]
     > Győződjön meg arról, hogy meg kell egyeznie a hello neve hello fogadó logikai alkalmazás által meghatározott hello kötegelt nevét ne módosítsa.
     > Hello változó neve a logic app toofail hello küldő okoz.

   * **Üzenet-tartalom**: hello üzenet tartalmát, amelyet az toosend. 
   Ennél a példánál adja hozzá a kifejezést, hogy Beszúrások aktuális dátum és idő hello tartalom, hogy küldjön toohello kötegelt hello üzenetbe:

     1. Ha hello **dinamikus tartalom** megjelenik a listán, válassza a **kifejezés**. 
     2. Adja meg a hello kifejezés **utcnow()**, és válassza a **OK**. 

        ![A "Üzenet tartalom" válassza a "Kifejezése". Adja meg a "utcnow()".](./media/logic-apps-batch-process-send-receive-messages/send-batch-receiver-details.png)

4. Most már készen hello köteg partíció. Hello "BatchReceiver" művelet, válassza a **speciális beállítások megjelenítése**.

   * **A partíció neve**: egy nem kötelező egyedi partíciós kulcs toouse hello cél kötegelt megosztásának. Vegye fel az ebben a példában egy kifejezés, amely egy és öt közötti véletlenszerű számot állít elő.
   
     1. Ha hello **dinamikus tartalom** megjelenik a listán, válassza a **kifejezés**.
     2. Adja meg a kifejezés: **rand(1,6)**

        ![A célként megadott köteg partíció beállítása](./media/logic-apps-batch-process-send-receive-messages/send-batch-receiver-partition-advanced-options.png)

        Ez **VÉL** függvény hoz létre egy és 5 közötti számot. 
        Ezért ez a köteg vannak felosztásával öt számozott partíciókra, amely ebben a kifejezésben dinamikusan.

   * **Üzenet azonosítója**: nem kötelező azonosítót és egy előállított GUID Azonosítóhoz, ha üres. 
   Ehhez a példához hagyja üresen a mezőt.

5. Mentse a Logic Apps alkalmazást. A küldő logikai alkalmazás most következőhöz hasonló toothis példa:

   ![Mentse a küldő Logic Apps alkalmazást](./media/logic-apps-batch-process-send-receive-messages/send-batch-receiver-details-finished.png)

## <a name="test-your-logic-apps"></a>A logic Apps alkalmazások tesztelése

tootest a kötegelés megoldás, hagyja a logic Apps alkalmazásokat futtató néhány percig. Hamarosan e-mailek első öt csoportokban indítása, mindezt hello azonos partícióazonosító kulcs.

A BatchSender Logic Apps alkalmazást percenként fut, egy és öt közötti véletlenszerű számot állít elő, és a létrehozott szám használ kulcsaként hello partíció hello cél kötegelt, ahol az üzenetek küldése történik. Minden alkalommal, amikor hello kötegelt hello öt elemek rendelkezik ugyanazzal a partíciókulccsal, a BatchReceiver Logic Apps alkalmazást következik be, és elküldi mail minden egyes üzenet esetében.

> [!IMPORTANT]
> Amikor végzett tesztelése, győződjön meg arról, tiltsa le a hello BatchSender logic app toostop üzenetküldésre, és el lehessen kerülni a beérkezett túlterhelését.

## <a name="next-steps"></a>Következő lépések

* [A JSON logikai alkalmazás definícióiról létrehozása](../logic-apps/logic-apps-author-definitions.md)
* [Egy kiszolgáló nélküli alkalmazást a Visual Studio és az Azure Logic Apps és függvények létrehozása](../logic-apps/logic-apps-serverless-get-started-vs.md)
* [Kivétel kezelése és a hibanaplózás a logic Apps alkalmazások](../logic-apps/logic-apps-scenario-error-and-exception-handling.md)
