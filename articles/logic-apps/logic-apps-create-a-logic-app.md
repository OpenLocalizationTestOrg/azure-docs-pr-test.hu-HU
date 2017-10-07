---
title: "aaaCreate a felhőalapú alkalmazások és a felhőszolgáltatás - Azure Logic Apps közötti első munkafolyamat |} Microsoft Docs"
description: "Üzleti folyamatok automatizálása a rendszerintegrációs és vállalati alkalmazásintegrációs (EAI) forgatókönyvekhez Azure Logic Appsban létrehozott és futtatott munkafolyamatok segítségével"
author: jeffhollan
manager: anneta
editor: 
services: logic-apps
keywords: "munkafolyamat, felhőalapú alkalmazások, felhőszolgáltatások, üzleti folyamatok, rendszerintegráció, vállalati alkalmazásintegráció, EAI"
documentationcenter: 
ms.assetid: ce3582b5-9c58-4637-9379-75ff99878dcd
ms.service: logic-apps
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/31/2017
ms.author: LADocs; jehollan; estfan
ms.openlocfilehash: 17ec589b1c8923b5ad3e6479fc856b6ac81754ab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-logic-app-workflow-tooautomate-processes-between-cloud-apps-and-cloud-services"></a>Az első logikai alkalmazás létrehozása tooautomate munkafolyamatok közötti felhőalapú alkalmazások és a cloud services csomag

Könnyebben és gyorsabban automatizálhatja az üzleti folyamatokat kód megírása nélkül is, ha az [Azure Logic Apps](logic-apps-what-are-logic-apps.md) segítségével hoz létre és futtat munkafolyamatokat. A első példa bemutatja, hogyan toocreate egy alapszintű logikája app munkafolyamat, amely ellenőrzi az RSS-hírcsatorna egy webhelyen új tartalom. Új elemek hello webhely adatcsatorna jelenik meg, amikor a hello logikai alkalmazás e-mailt küld az Outlook vagy Gmailes fiókból.

toocreate, és futtassa a logic app, ezek az elemek szükségesek:

* Azure-előfizetés. Ha nem rendelkezik előfizetéssel, [kezdhet egy ingyenes Azure-fiókkal](https://azure.microsoft.com/free/). Egyéb esetben [regisztrálhat használatalapú fizetéses előfizetésre](https://azure.microsoft.com/pricing/purchase-options/).

  Az Azure-előfizetés a logikaialkalmazás-használat számlázására szolgál. Ismerje meg, hogyan működik a [használatmérés](../logic-apps/logic-apps-pricing.md) és a [díjszabás](https://azure.microsoft.com/pricing/details/logic-apps) az Azure Logic Apps esetében.

A példához a következő elemek is szükségesek:

* Outlook.com-, Office 365 Outlook- vagy Gmail-fiók

    > [!TIP]
    > Ha rendelkezik személyes [Microsoft-fiókkal](https://account.microsoft.com/account), akkor Outlook.com-fiókja van. Egyéb esetben, ha rendelkezik Azure munkahelyi vagy iskolai fiókkal, **Office 365 Outlook**-fiókja van.

* A hivatkozás tooa webhely RSS-hírcsatornára. Ez a példa hello [RSS-hírcsatorna a legfontosabb események hello CNN.com webhelyről](http://rss.cnn.com/rss/cnn_topstories.rss):`http://rss.cnn.com/rss/cnn_topstories.rss`

## <a name="add-a-trigger-that-starts-your-workflow"></a>Eseményindító hozzáadása a munkafolyamat indításához

A [ *eseményindító* ](./logic-apps-what-are-logic-apps.md#logic-app-concepts) olyan esemény, amely elindítja a logic app munkafolyamatot és hello első elem, amelyet a Logic Apps alkalmazást.

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com "Azure-portálon").

2. Hello bal oldali menüből **új** > **vállalati integrációs** > **logikai alkalmazás** itt látható módon:

     ![Azure Portal, Új, Vállalati integráció, Logikai alkalmazás](media/logic-apps-create-a-logic-app/azure-portal-create-logic-app.png)

   > [!TIP]
   > Dönthet úgy is **új**, hello a keresőmezőbe írja be a `logic app`, és nyomja le az ENTER billentyűt. Válassza a **Logikai alkalmazás** > **Létrehozás** elemet.

3. Nevezze el a logikai alkalmazást, majd válassza ki az Azure-előfizetését. Most hozzon létre vagy válasszon ki egy Azure-erőforráscsoportot, amely segít a kapcsolódó Azure-erőforrások rendszerezésében és kezelésében. Végül válassza ki a Logic Apps alkalmazást üzemeltető hello datacenter helyét. Ha elkészült, válassza ki a **PIN-kód toodashboard** , majd **létrehozása**.

     ![Logikai alkalmazás részletei](media/logic-apps-create-a-logic-app/logic-app-settings.png)

   > [!NOTE]
   > Ha bejelöli **PIN-kód toodashboard**, a Logic Apps alkalmazást a telepítés utáni hello Azure irányítópulton megjelenik, és automatikusan megnyílik. Ha nem szerepel a Logic Apps alkalmazást hello irányítópultot, hello **összes erőforrás** csempére, válassza a **lásd: több**, és válassza ki a Logic Apps alkalmazást. Vagy a hello bal oldali menüben válassza a **további szolgáltatások**. A **Vállalati integráció** résznél válassza a **Logikai alkalmazások** elemet, majd válassza ki a logikai alkalmazást.

4. A Logic Apps alkalmazást az hello megnyitásakor első alkalommal a hello Logic App Designer látható sablonok használható tooget elindult. Jelen esetben válassza az **Üres logikai alkalmazás** elemet, így az alapoktól építheti fel saját logikai alkalmazását.

    hello Logic App Designer megnyílik, és megjeleníti az elérhető szolgáltatások és *eseményindítók* , amely a Logic Apps alkalmazást is használhatja.

5. Hello keresési mezőbe, írja be a `RSS`, és válassza ki az ehhez az eseményindítóhoz: **RSS - hírcsatorna elem meg van nyitva** 

    ![RSS-eseményindító](media/logic-apps-create-a-logic-app/rss-trigger.png)

6. Adja meg, amelyet az tootrack hello webhely RSS-hírcsatorna hello hivatkozás. 

     Módosíthatja a **Gyakoriság** és az **Időköz** értékeket is. 
     Ezek a beállítások határozzák meg, hogy a logikai alkalmazás milyen gyakran keres új elemeket és ad vissza minden megtalált elemet az adott időtartományon belül.

     Ebben a példában most ellenőrizze naponta a legfontosabb események toohello CNN webhelyen közzétett.

     ![Eseményindító beállítása RSS-hírcsatornával, gyakorisággal és időközzel](media/logic-apps-create-a-logic-app/rss-trigger-setup.png)

7. Egyelőre mentse a munkáját. (A hello Tervező parancssávon válassza **mentése**.)

   ![A logikai alkalmazás mentése](media/logic-apps-create-a-logic-app/save-logic-app.png)

   Ha menti, a logikai alkalmazás élő kerül, de jelenleg a Logic Apps alkalmazást csak keres új elemek hello megadott RSS-hírcsatornára. 
   toomake ebben a példában hasznosabb, azt a logikai alkalmazás végző után az eseményindító művelet hozzáadása következik be.

## <a name="add-an-action-that-responds-tooyour-trigger"></a>Amely válaszol a tooyour eseményindító művelet hozzáadása

A [*művelet*](./logic-apps-what-are-logic-apps.md#logic-app-concepts) a logikai alkalmazás munkafolyamata által végrehajtott feladat. Egy eseményindító tooyour logikai alkalmazás hozzáadása után az, hogy az eseményindító által létrehozott adatok egy művelet tooperform műveletek is hozzáadhat. A példa kedvéért azt most művelet hozzáadása, amely e-mailt küld, amikor új elemek jelennek meg a hello webhely RSS-hírcsatornára.

1. Hello Designer alapján az eseményindító válasszon **új lépés** > **művelet hozzáadása** itt látható módon:

   ![Művelet hozzáadása](media/logic-apps-create-a-logic-app/add-new-action.png)

   hello a Tervező látható szövegrészt [összekötőket](../connectors/apis-list.md) , hogy egy művelet tooperform kiválaszthatja, ha az eseményindító következik be.

2. Az Outlook vagy Gmailes e-mail fiókja alapján, kövesse az hello lépéseket.

   * toosend e-mailt az Outlook fiókból hello keresési mezőbe, írja be `outlook`. A **Szolgáltatások** területen válassza az **Outlook.com** elemet személyes Microsoft-fiók, illetve az **Office 365 Outlook** elemet Azure munkahelyi vagy iskolai fiókok esetében. 
   A **Műveletek** területen válassza az **E-mal küldése** lehetőséget.

       ![Az Outlook „E-mail küldése” műveletének kiválasztása](media/logic-apps-create-a-logic-app/actions.png)

   * toosend e-mail fiókjából Gmail, hello keresési mezőbe, írja be `gmail`. 
   A **Műveletek** területen válassza az **E-mal küldése** lehetőséget.

       ![Válassza a „Gmail – E-mail küldése” elemet](media/logic-apps-create-a-logic-app/actions-gmail.png)

3. Amikor a rendszer kéri a hitelesítő adatokat, jelentkezzen be a hello felhasználónevet és jelszót e-mail fiókjához. 

4. Hello részletesen a művelet, például hello cél e-mail címét, és hello adatok tooinclude hello paramétereinek választani hello e-mailben, például:

   ![Válassza az adatok tooinclude e-mailben](media/logic-apps-create-a-logic-app/rss-action-setup.png)

    Ha az Outlookot választja, a logikai alkalmazás a következőhöz hasonlóan nézhet ki:

    ![Az elkészült logikai alkalmazás](media/logic-apps-create-a-logic-app/save-run-complete-logic-app.png)

5.  Mentse a módosításokat. (A hello Tervező parancssávon válassza **mentése**.)

6. Mostantól manuálisan futtathatja a logikai alkalmazást tesztelés céljából. A hello Tervező parancssávon válassza **futtatása**. Ellenkező esetben hogy a Logic Apps alkalmazást ellenőrizze hello megadott RSS-hírcsatorna a beállított hello ütemezés szerint.

   Ha a logikai alkalmazás új elemek talál, a hello logikai alkalmazást a kijelölt adatokat tartalmazó e-mailt küld. 
   Ha új elemek találhatók, a logikai alkalmazás kihagyja, hogy e-mailt küld hello műveletet.

7. toomonitor és ellenőrizze a Logic Apps alkalmazást tartozó futtatása és indul el, előzmények a logic app menüben válasszon **áttekintése**.

   ![A logikai alkalmazás futtatási és eseményindítási előzményeinek figyelése és ellenőrzése](media/logic-apps-create-a-logic-app/logic-app-run-trigger-history.png)

   > [!TIP]
   > Ha nem találja a hello adatokat várt, hello parancssávon, válasszon **frissítése**.

   További információk a Logic Apps alkalmazást állapot toolearn vagy futtatása és előzmények vagy toodiagnose a Logic Apps alkalmazást indítható el, lásd: [hibaelhárítása a Logic Apps alkalmazást](logic-apps-diagnosing-failures.md).

      > [!NOTE]
      > A logikai alkalmazás addig fut, amíg ki nem kapcsolja az alkalmazást. Válassza ki az alkalmazás most a logic app menüjében tooturn **áttekintése**. A hello parancssávon válassza **letiltása**.

Gratulálunk, sikeresen beállította és futtatta az első logikai alkalmazását. Azt is megtanulta, milyen egyszerű a folyamatokat automatizáló munkafolyamat létrehozása, valamint a felhőalapú alkalmazások és felhőszolgáltatások integrálása – mindez kód nélkül.

## <a name="manage-your-logic-app"></a>A logikai alkalmazás kezelése

toomanage az alkalmazás feladatokat hajthat végre hasonló hello állapotának, szerkesztése, előzményeinek megtekintése, kapcsolja ki, vagy törölje a logikai alkalmazást.

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com "Azure-portálon").

2. A hello bal oldali menüben válassza a **további szolgáltatások**. A **Vállalati integráció** résznél válassza a **Logikai alkalmazások** elemet. Válassza ki a logikai alkalmazást. 

   Hello logic app menüjében találja meg a logic app felügyeleti feladatok:

   |Tevékenység|Lépések| 
   |:---|:---| 
   | Az alkalmazás állapotának, végrehajtási előzményeinek és általános információinak megtekintése| Válassza az **Áttekintés** elemet.| 
   | Az alkalmazás szerkesztése | Válassza a **Logikaialkalmazás-tervező** lehetőséget. | 
   | Az alkalmazáshoz tartozó munkafolyamat JSON-definíciójának megtekintése | Válassza a **Logikai alkalmazás kódnézete** elemet. | 
   | A logikai alkalmazáson végrehajtott műveletek megtekintése | Válassza a **Tevékenységnapló** elemet. | 
   | A logikai alkalmazás korábbi verzióinak megtekintése | Válassza a **Verziók** elemet. | 
   | Az alkalmazás ideiglenes kikapcsolása | Válassza ki **áttekintése**, majd a hello parancssávon válassza **tiltsa le a**. | 
   | Az alkalmazás törlése | Válassza ki **áttekintése**, majd a hello parancssávon válassza **törlése**. Adja meg a logikai alkalmazás nevét, majd válassza a **Törlés** elemet. | 

## <a name="get-help"></a>Segítségkérés

tooask kérdések kérdést, és ismerje meg, milyen egyéb Azure Logic Apps felhasználók végzi, látogasson el hello [Azure Logic Apps-fórum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).

toohelp Azure Logic Apps alkalmazások és összekötők javítása, szavazhatnak, vagy küldje el a következő hello ötleteket [Azure Logic Apps felhasználói visszajelzési webhelyet](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Következő lépések

*  [Feltételek hozzáadása és munkafolyamatok futtatása](../logic-apps/logic-apps-use-logic-app-features.md)
*    [A logikai alkalmazások sablonjai](../logic-apps/logic-apps-use-logic-app-templates.md)
*  [Logikai alkalmazások létrehozása Azure Resource Manager-sablonokból](../logic-apps/logic-apps-arm-provision.md)
