---
title: "aaaAdd feltételeket, és indítsa el az Azure Logic Apps-munkafolyamatok – |} Microsoft Docs"
description: "Szabályozza, hogyan munkafolyamatok futnak, az Azure Logic Apps feltételes logikához, eseményindítók, műveletek és paraméterek hozzáadásával."
author: stepsic-microsoft-com
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: e4e24de4-049a-4b3a-a14c-3bf3163287a8
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/28/2017
ms.author: LADocs; stepsic
ms.openlocfilehash: 76d5e44590ffa14cf70d7a93b99a241d286d555b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-logic-apps-features"></a>Logic Apps funkcióinak használata

Az egy [előző témakörben](../logic-apps/logic-apps-create-a-logic-app.md), az első logikai alkalmazás. toocontrol a Logic Apps alkalmazást munkafolyamat, adja meg a logic app toorun elérési útja eltér, és hogyan túl fel az adatokat a tömböket, gyűjtemények és kötegek. Ezeket az elemeket a logic app munkafolyamat alábbiakból állhat:

* Feltételek és [utasítások kapcsoló](../logic-apps/logic-apps-switch-case.md) lehetővé teszik a Logic Apps alkalmazást, hogy meghatározott feltételek alapján más műveletek futtatása.

* [Hurkok](../logic-apps/logic-apps-loops-and-scopes.md) lehetővé teszik a Logic Apps alkalmazást ismételten lépések futtatásával. Például megismétlésével műveletek keresztül tömb használatakor egy **For_each** hurok. Vagy a művelet megismétlésével egy feltétel teljesüléséig, használatakor egy **amíg** hurok.

* [Hatókörök](../logic-apps/logic-apps-loops-and-scopes.md) lehetővé csoportosítása műveletsorozattal együtt, például tooimplement kivételkezelést.

* [Debatching](../logic-apps/logic-apps-loops-and-scopes.md) lehetővé teszi, hogy a logikai alkalmazás indul elemek külön munkafolyamatok tömbben el hello **SplitOn** parancsot.

Ez a témakör bemutatja a más fogalmak összeállítani saját logikai alkalmazását:

* Egy meglévő logikai alkalmazás nézet tooedit kód
* Egy munkafolyamat beállítások

## <a name="conditions-run-steps-only-after-meeting-a-condition"></a>Feltételek: Lépéseket csak egy feltétel teljesítése után futtassa.

a Logic Apps alkalmazást lépések futtatásával csak akkor, amikor adatokat meghatározott feltételeknek, adhat hozzá egy feltételt, amely összehasonlítja az adatok adott mezők vagy értékek hello munkafolyamat toohave.

Tegyük fel például, hogy egy webhely RSS-hírcsatorna a POST túl sok e-mailt küld logikai alkalmazás. Hozzáadhat egy feltétel, hogy a logikai alkalmazás e-mailt küld, csak akkor, ha új hello utáni tartozik tooa adott konkrét kategóriával.

1. A hello [Azure-portálon](https://portal.azure.com), található, és nyissa meg a Logic Apps alkalmazást a Logic App tervezőben.

2. Adja hozzá a feltétel toohello munkafolyamat helyet használni kívánt. 

   tooadd hello feltétel hello logic app munkafolyamat, meglévő lépések között helyezze hello egérmutatót hello nyíl kívánt tooadd hello feltétel. 
   Válassza ki a hello **pluszjel** (**+**), majd válassza a **feltétel hozzáadása**. Példa:

   ![Az állapot toologic alkalmazás hozzáadása](./media/logic-apps-use-logic-app-features/add-condition.png)

   > [!NOTE]
   > Ha egy feltétel tooadd a jelenlegi munkafolyamatot hello végén, nyissa meg a Logic Apps alkalmazást toohello aljára, és válassza ki **+ új lépés**.

3. Most meghatározhatja hello feltételét. Adja meg, amelyet tooevaluate, hello művelet tooperform, és hello célérték vagy mező hello mezőjének. tooadd meglévő mezők tooyour feltétel hello választhat **hozzáadása a dinamikus tartalom listába**.

   Példa:

   ![Alapszintű módban feltétel szerkesztése](./media/logic-apps-use-logic-app-features/edit-condition-basic-mode.png)

   Az alábbiakban hello teljes feltétel:

   ![Teljes feltétel](./media/logic-apps-use-logic-app-features/edit-condition-basic-mode-2.png)

   > [!TIP]
   > a kódban, toodefine hello feltétel kiválasztása **speciális módban szerkesztése**. Példa:
   > 
   > ![A kód feltétel szerkesztése](./media/logic-apps-use-logic-app-features/edit-condition-advanced-mode.png)

4. A **IF Igen** és **IF nem**, adja hozzá a hello lépéseket tooperform alapján hello feltétel teljesülését vizsgálja.

   Példa:

   ![Igen, és nincsenek elérési utak feltételei](./media/logic-apps-use-logic-app-features/condition-yes-no-path.png)

   > [!TIP]
   > Meglévő műveletek húzza hello **IF Igen** és **IF nem** elérési utakat.

5. Amikor elkészült, mentse a Logic Apps alkalmazást.

Most kapott e-mailek csak hello bejegyzéseket felel meg a feltételnek.

## <a name="repeat-actions-over-a-list-with-foreach"></a>Ismételje meg a műveletek során a forEach

hello forEach hurok egy tömb toorepeat művelet keresztül határozza meg. Ha nem tömb, hello folyamat sikertelen lesz. Például action1 üzenetek tömbjét adja kimenetként, amely rendelkezik, és minden üzenetet toosend szeretnénk, ha is felvehet a forEach-utasítás tulajdonságainak hello a művelethez:`forEach : "@action('action1').outputs.messages"`

## <a name="edit-hello-code-definition-for-a-logic-app"></a>Hello kód megadása a logikai alkalmazás szerkesztése

Bár a Logic App Designer hello, hello kódot, amely meghatározza a logikai alkalmazás közvetlenül szerkesztheti.

1. A hello parancssávon válassza **nézet Code**.

    A teljes szerkesztő megnyílik, és szerkesztett hello definition jeleníti meg.

    ![Kód nézetre](media/logic-apps-use-logic-app-features/codeview.png)

    Hello szövegszerkesztőben, másolja és illessze be a tetszőleges számú műveletet hello belül azonos logikai alkalmazás vagy a logic Apps alkalmazások között. 
    Is egyszerűen adja hozzá vagy távolítsa el a teljes szakaszok hello definícióból, és másokkal is megoszthat definíciókat.

2. toosave a módosításokat, válassza a **mentése**.

## <a name="parameters"></a>Paraméterek

Egyes Logic Apps tulajdonságai csak a kód nézetre, például a paraméterek érhetők el. Paraméterek révén könnyen tooreuse értékét a logikai alkalmazást. Például ha egy e-mail címet, amelyet számos műveletet használja, meg kell határozni, hogy e-mail cím paraméterként.

A paraméterekre ideális húzza ki valószínűleg toochange sokkal értékeket. Különösen akkor hasznos, ha különböző környezetekben toooverride paraméterek van szükség. Hogyan toooverride paraméterek környezet alapján: toolearn [logikai alkalmazás definícióiról szerzői](../logic-apps/logic-apps-author-definitions.md) és [REST API-dokumentáció](https://docs.microsoft.com/rest/api/logic).

A példa bemutatja, hogyan tooupdate a meglévő Logic Apps alkalmazást, hogy a paraméter használható hello lekérdezési kifejezésre.

1. A kód nézetre, megkeresi a hello `parameters : {}` objektumot, és adjon hozzá egy `currentFeedUrl` objektum:

        "currentFeedUrl" : {
            "type" : "string",
            "defaultValue" : "http://rss.cnn.com/rss/cnn_topstories.rss"
        }

2. Nyissa meg toohello `When_a_feed-item_is_published` művelet, a keresés hello `queries` szakaszt, és cserélje le hello lekérdezés értékét:`"feedUrl": "#@{parameters('currentFeedUrl')}"` 

    toojoin két vagy több karakterláncot, használhatja a hello `concat` függvény. 
    Például `"@concat('#',parameters('currentFeedUrl'))"` works hello ugyanaz, mint a fenti hello.

3.  Amikor elkészült, válassza ki a **mentése**. 

    Most hello webhely RSS-hírcsatorna úgy, hogy egy másik URL-címet hello keresztül módosítható `currentFeedURL` objektum.

További információ [hogyan tooauthor logikai alkalmazás definícióiról](../logic-apps/logic-apps-author-definitions.md).

## <a name="start-logic-app-workflows"></a>Indítsa el a logic app munkafolyamatok

A Logic Apps alkalmazást definiált hello munkafolyamat indítása különböző lehetősége van. Egy munkafolyamat-igény szerinti mindig indítható hello [Azure-portálon].

### <a name="recurrence-triggers"></a>Ismétlődés eseményindítók

Ismétlődés eseményindító megadott időközönként fut. Hello eseményindító feltételes logikához rendelkezik, a hello eseményindító meghatározza, hogy hello munkafolyamat toorun kell-e. Egy eseményindító azt jelzi, hogy hello munkafolyamat futtatásának vissza egy `200` állapotkódot. Hello munkafolyamat toorun nem szükséges, ha hello eseményindító adja vissza egy `202` állapotkódot.

### <a name="callback-using-rest-apis"></a>A visszahívási REST API-k használatával

szolgáltatások toostart munkafolyamat, meghívhatja a logic app végpont. toostart logic app igény, az ilyen válasszon **futtatása most** hello parancssávon. Lásd: [indítsa el a munkafolyamatok logic app végpontok eseményindítóként használható meghívásával](../logic-apps/logic-apps-http-endpoint.md). 

<!-- Shared links -->
[Azure-portálon]: https://portal.azure.com

## <a name="next-steps"></a>Következő lépések

* [Kapcsoló utasítások](../logic-apps/logic-apps-switch-case.md) 
* [Hurkok, hatókörök és kibontás](../logic-apps/logic-apps-loops-and-scopes.md)
* [Logikaialkalmazás-definíciók készítése](../logic-apps/logic-apps-author-definitions.md)