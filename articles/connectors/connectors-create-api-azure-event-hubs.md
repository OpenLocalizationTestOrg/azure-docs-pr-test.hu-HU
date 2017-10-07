---
title: "mentése az Azure Event Hubs az Azure Logic Apps eseményfigyelő aaaSet |} Microsoft Docs"
description: "Adatok adatfolyamok tooreceive események figyelése és események küldése az Azure Logic Apps az Azure Event Hubs"
services: logic-apps
keywords: "az adatfolyam, eseményfigyelő, az event hubs"
author: ecfan
manager: anneta
editor: 
documentationcenter: 
tags: connectors
ms.assetid: 
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/31/2017
ms.author: estfan; LADocs
ms.openlocfilehash: 4aad2c2ac1134b4d4d440019b4773559e49be122
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-receive-and-send-events-with-hello-event-hubs-connector"></a>Figyelése, fogadására és küldi az eseményeket hello Event Hubs-összekötőn keresztül

tooset az eseményfigyelő be, hogy a Logic Apps alkalmazást képesek észlelni események, képes eseményeket fogadni, és elküldje az eseményeket, csatlakozás tooan [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs) a logikai alkalmazás. További információ [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md).

## <a name="requirements"></a>Követelmények

* Toohave rendelkezik egy [Event Hubs-névteret és Eseményközpont](../event-hubs/event-hubs-create.md) az Azure-ban. Ismerje meg, [hogyan toocreate az Event Hubs névtér és az Event Hubs](../event-hubs/event-hubs-create.md). 

* toouse [a csatlakozókat](https://docs.microsoft.com/azure/connectors/apis-list) a Logic Apps alkalmazást fel kell toocreate logikai alkalmazás először. Ismerje meg, [hogyan toocreate logikai alkalmazás](../logic-apps/logic-apps-create-a-logic-app.md).

<a name="permissions-connection-string"></a>
## <a name="check-event-hubs-namespace-permissions-and-find-hello-connection-string"></a>Ellenőrizze az Event Hubs névtér engedélyeit, és keresse meg hello kapcsolati karakterláncot

A logic app tooaccess tartozó bármely szolgáltatás toocreate rendelkezik egy [ *kapcsolat* ](./connectors-overview.md) között a logic app és hello szolgáltatást, ha még nem tette meg. Ezt a kapcsolatot a logic app tooaccess adatok engedélyezi.
A logic app tooaccess az Eseményközpont rendelkezik toohave **kezelése** engedélyek és hello kapcsolati karakterláncot az Event Hubs-névtérhez.

toocheck az engedélyeit, és a get hello kapcsolati karakterláncot, kövesse az alábbi lépéseket.

1.  Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com "Azure-portálon"). 

2.  Nyissa meg az Event Hubs tooyour *névtér*, nem az adott Eseményközpontban hello. Hello névtér paneljén alatt **beállítások**, válassza a **megosztott elérési házirendek**. A **jogcímek**, ellenőrizze, hogy rendelkezik **kezelése** engedélyeket a névtérhez.

    ![Az Event Hubs névtér engedélyeinek kezelése](./media/connectors-create-api-azure-event-hubs/event-hubs-namespace.png)

3.  toocopy hello kapcsolati karakterlánc hello Event Hubs névtér esetén válasszon **RootManageSharedAccessKey**. Következő tooyour elsődleges kulcs kapcsolati karakterláncot, válassza a hello Másolás gombra.

    ![Másolja az Event Hubs névtér kapcsolati karakterláncot](media/connectors-create-api-azure-event-hubs/find-event-hub-namespace-connection-string.png)

    > [!TIP]
    > a kapcsolati karakterlánc társítva, az Event Hubs névtér vagy egy adott Eseményközpontban, hogy ellenőrizze hello kapcsolati karakterláncot hello tooconfirm `EntityPath` paraméter. Ha ezt a paramétert, a hello kapcsolati karakterlánc az egy adott Eseményközpontban "entitás", és nincs hello megfelelő karakterlánc toouse a Logic Apps alkalmazást az.

4.  Amikor a rendszer kéri a hitelesítő adatokat az Event Hubs eseményindító vagy művelet tooyour logikai alkalmazás hozzáadása után, most is tooyour Event Hubs névtér elérheti. Nevezze el a kapcsolatot, adjon meg hello kapcsolati karakterláncot, amely a másolt, és válassza a **létrehozása**.

    ![Adja meg a kapcsolati karakterlánc az Event Hubs névtér](./media/connectors-create-api-azure-event-hubs/event-hubs-connection.png)

    Miután létrehozta a kapcsolatot, hello kapcsolat nevét meg kell jelennie hello Event Hubs eseményindító vagy a műveletet. 
    Ezután folytathatja a hello más lépéseket a Logic Apps alkalmazást.

    ![Event Hubs névtér kapcsolat létre](./media/connectors-create-api-azure-event-hubs/event-hubs-connection-created.png)

## <a name="start-workflow-when-your-event-hub-receives-new-events"></a>Ha az Eseményközpont kap új események munkafolyamat indítása

A [ *eseményindító* ](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts) olyan esemény, amely a Logic Apps alkalmazást a munkamenet indítása. Indítani egy munkafolyamatot, ha új események tooyour Eseményközpont küld, kövesse az alábbi lépéseket a hello eseményindító, melyeket észlel, ezt az eseményt hozzáadásához.

1.  A hello [Azure-portálon](https://portal.azure.com "Azure-portálon"), nyissa meg tooyour meglévő Logic Apps alkalmazást, vagy egy üres logikai alkalmazás létrehozása.

2.  A Logic App Designer hello hello a keresési mezőbe, írja be a `event hubs` a szűrőhöz. Válassza ki az ehhez az eseményindítóhoz: **mikor események érhetők el az Event Hubs**

    ![Válassza ki a eseményindítója a következőnek: Ha az Eseményközpont kap új események](./media/connectors-create-api-azure-event-hubs/find-event-hubs-trigger.png)

    Ha még nem rendelkezik a kapcsolati tooyour Event Hubs-névtér, megkéri toocreate most ezt a kapcsolatot. Nevezze el a kapcsolatot, és adjon meg az Event Hubs névtér hello kapcsolati karakterláncot. 
    Szükség esetén további [hogyan toofind a kapcsolati karakterlánc](#permissions-connection-string).

    ![Adja meg a kapcsolati karakterlánc az Event Hubs névtér](./media/connectors-create-api-azure-event-hubs/event-hubs-connection.png)

    Hello kapcsolat létrehozása után hello hello beállításainak **Ha olyan esemény az elérhető eseményközpontban** eseményindító jelennek meg.

    ![Ha az Eseményközpont kap új események eseményindító beállításai](./media/connectors-create-api-azure-event-hubs/event-hubs-trigger.png)

3.  Adja meg, vagy válassza ki a megjeleníteni kívánt toomonitor Eseményközpont hello hello nevét. Válassza ki a hello gyakoriságát és milyen gyakran szeretné toocheck hello Eseményközpont intervallumát.

    > [!TIP]
    > toooptionally válassza ki a fogyasztói csoportot események olvasása, válassza a **speciális beállítások megjelenítése**. 

    ![Adja meg az Event Hubs vagy felhasználói csoport](./media/connectors-create-api-azure-event-hubs/event-hubs-trigger-details.png)

    Most létrehozott egy eseményindító toostart munkafolyamat a logikai alkalmazásnak. 
    A logikai alkalmazás ellenőrzi, hogy hello megadott Eseményközpont hello beállított ütemezés szerint alapján. 
    Ha az alkalmazás új események hello Eseményközpont talál, hello eseményindító futtatja más műveletek vagy eseményindítók a Logic Apps alkalmazást.

## <a name="send-events-tooyour-event-hub-from-your-logic-app"></a>A Logic Apps alkalmazást események tooyour Eseményközpont küldése

A [*művelet*](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts) a logikai alkalmazás munkafolyamata által végrehajtott feladat. Egy eseményindító tooyour logikai alkalmazás hozzáadása után az, hogy az eseményindító által létrehozott adatok egy művelet tooperform műveletek is hozzáadhat. egy esemény tooyour Eseményközpont a logic App toosend kövesse az alábbi lépéseket.

1.  Logic App tervezőben, a logic app eseményindító alatt válassza a **új lépés** > **művelet hozzáadása**.

    ![Válassza a "Új lépés", majd "Művelet hozzáadása"](./media/connectors-create-api-azure-event-hubs/add-action.png)

    Most keresse meg és válassza ki egy művelet tooperform. 
    Azonban bármely művelet, például kiválaszthatja azt szeretnénk, ha hello Event Hubs művelet toosend események.

2.  Hello keresési mezőbe, írja be a `event hubs` a szűrőhöz.
Ez a művelet kiválasztása: **küldési esemény**

    ![Válassza ki az "Event Hubs - küldési esemény" művelet](./media/connectors-create-api-azure-event-hubs/find-event-hubs-action.png)

3.  Írja be a szükséges hello adatait hello esemény, például a hello toosend hello esemény, ahová az Event Hubs hello nevét. Írja be a hello eseményről, például tartalom esemény más választható adatait.

    > [!TIP]
    > toooptionally adja meg, ahol toosend hello esemény, hello Eseményközpont-partíción válasszon **speciális beállítások megjelenítése**. 

    ![Adja meg az Eseményközpont nevét és opcionális esemény részletei](./media/connectors-create-api-azure-event-hubs/event-hubs-send-event-action.png)

6.  Mentse a módosításokat.

    ![A logikai alkalmazás mentése](./media/connectors-create-api-azure-event-hubs/save-logic-app.png)

    Most létrehozott egy művelet toosend események a logikai alkalmazás. 

## <a name="connector-specific-details"></a>Összekötő-specifikus részletei

Bármely eseményindítók és hello swagger definiált műveletek megtekintése, és semmilyen határnak hello a Lásd még: [connector részleteket](/connectors/eventhubs/). 

## <a name="get-help"></a>Segítségkérés

tooask kérdéseket, válasz kérdések és milyen egyéb Azure Logic Apps felhasználók végzi, tekintse meg a Microsoft hello [Azure Logic Apps-fórum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).

Logic Apps alkalmazások és összekötők javítása, szavazhatnak vagy küldje el a következő hello ötleteket toohelp [Logic Apps felhasználói visszajelzési webhelyet](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Következő lépések

*  [Más összekötők keresése Azure Logic Apps alkalmazások](./apis-list.md)