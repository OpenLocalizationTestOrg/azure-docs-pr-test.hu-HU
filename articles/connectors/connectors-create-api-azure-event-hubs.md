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
# <a name="monitor-receive-and-send-events-with-hello-event-hubs-connector"></a><span data-ttu-id="5144a-104">Figyelése, fogadására és küldi az eseményeket hello Event Hubs-összekötőn keresztül</span><span class="sxs-lookup"><span data-stu-id="5144a-104">Monitor, receive, and send events with hello Event Hubs connector</span></span>

<span data-ttu-id="5144a-105">tooset az eseményfigyelő be, hogy a Logic Apps alkalmazást képesek észlelni események, képes eseményeket fogadni, és elküldje az eseményeket, csatlakozás tooan [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs) a logikai alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="5144a-105">tooset up an event monitor so that your logic app can detect events, receive events, and send events, connect tooan [Azure Event Hub](https://azure.microsoft.com/services/event-hubs) from your logic app.</span></span> <span data-ttu-id="5144a-106">További információ [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md).</span><span class="sxs-lookup"><span data-stu-id="5144a-106">Learn more about [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md).</span></span>

## <a name="requirements"></a><span data-ttu-id="5144a-107">Követelmények</span><span class="sxs-lookup"><span data-stu-id="5144a-107">Requirements</span></span>

* <span data-ttu-id="5144a-108">Toohave rendelkezik egy [Event Hubs-névteret és Eseményközpont](../event-hubs/event-hubs-create.md) az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="5144a-108">You have toohave an [Event Hubs namespace and Event Hub](../event-hubs/event-hubs-create.md) in Azure.</span></span> <span data-ttu-id="5144a-109">Ismerje meg, [hogyan toocreate az Event Hubs névtér és az Event Hubs](../event-hubs/event-hubs-create.md).</span><span class="sxs-lookup"><span data-stu-id="5144a-109">Learn [how toocreate an Event Hubs namespace and Event Hub](../event-hubs/event-hubs-create.md).</span></span> 

* <span data-ttu-id="5144a-110">toouse [a csatlakozókat](https://docs.microsoft.com/azure/connectors/apis-list) a Logic Apps alkalmazást fel kell toocreate logikai alkalmazás először.</span><span class="sxs-lookup"><span data-stu-id="5144a-110">toouse [any connector](https://docs.microsoft.com/azure/connectors/apis-list) in your logic app, you have toocreate a logic app first.</span></span> <span data-ttu-id="5144a-111">Ismerje meg, [hogyan toocreate logikai alkalmazás](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="5144a-111">Learn [how toocreate a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

<a name="permissions-connection-string"></a>
## <a name="check-event-hubs-namespace-permissions-and-find-hello-connection-string"></a><span data-ttu-id="5144a-112">Ellenőrizze az Event Hubs névtér engedélyeit, és keresse meg hello kapcsolati karakterláncot</span><span class="sxs-lookup"><span data-stu-id="5144a-112">Check Event Hubs namespace permissions and find hello connection string</span></span>

<span data-ttu-id="5144a-113">A logic app tooaccess tartozó bármely szolgáltatás toocreate rendelkezik egy [ *kapcsolat* ](./connectors-overview.md) között a logic app és hello szolgáltatást, ha még nem tette meg.</span><span class="sxs-lookup"><span data-stu-id="5144a-113">For your logic app tooaccess any service, you have toocreate a [*connection*](./connectors-overview.md) between your logic app and hello service, if you haven't already.</span></span> <span data-ttu-id="5144a-114">Ezt a kapcsolatot a logic app tooaccess adatok engedélyezi.</span><span class="sxs-lookup"><span data-stu-id="5144a-114">This connection authorizes your logic app tooaccess data.</span></span>
<span data-ttu-id="5144a-115">A logic app tooaccess az Eseményközpont rendelkezik toohave **kezelése** engedélyek és hello kapcsolati karakterláncot az Event Hubs-névtérhez.</span><span class="sxs-lookup"><span data-stu-id="5144a-115">For your logic app tooaccess your Event Hub, you have toohave **Manage** permissions and hello connection string for your Event Hubs namespace.</span></span>

<span data-ttu-id="5144a-116">toocheck az engedélyeit, és a get hello kapcsolati karakterláncot, kövesse az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="5144a-116">toocheck your permissions and get hello connection string, follow these steps.</span></span>

1.  <span data-ttu-id="5144a-117">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com "Azure-portálon").</span><span class="sxs-lookup"><span data-stu-id="5144a-117">Sign in toohello [Azure portal](https://portal.azure.com "Azure portal").</span></span> 

2.  <span data-ttu-id="5144a-118">Nyissa meg az Event Hubs tooyour *névtér*, nem az adott Eseményközpontban hello.</span><span class="sxs-lookup"><span data-stu-id="5144a-118">Go tooyour Event Hubs *namespace*, not hello specific Event Hub.</span></span> <span data-ttu-id="5144a-119">Hello névtér paneljén alatt **beállítások**, válassza a **megosztott elérési házirendek**.</span><span class="sxs-lookup"><span data-stu-id="5144a-119">On hello namespace blade, under **Settings**, choose **Shared access policies**.</span></span> <span data-ttu-id="5144a-120">A **jogcímek**, ellenőrizze, hogy rendelkezik **kezelése** engedélyeket a névtérhez.</span><span class="sxs-lookup"><span data-stu-id="5144a-120">Under **Claims**, check that you have **Manage** permissions for that namespace.</span></span>

    ![Az Event Hubs névtér engedélyeinek kezelése](./media/connectors-create-api-azure-event-hubs/event-hubs-namespace.png)

3.  <span data-ttu-id="5144a-122">toocopy hello kapcsolati karakterlánc hello Event Hubs névtér esetén válasszon **RootManageSharedAccessKey**.</span><span class="sxs-lookup"><span data-stu-id="5144a-122">toocopy hello connection string for hello Event Hubs namespace, choose **RootManageSharedAccessKey**.</span></span> <span data-ttu-id="5144a-123">Következő tooyour elsődleges kulcs kapcsolati karakterláncot, válassza a hello Másolás gombra.</span><span class="sxs-lookup"><span data-stu-id="5144a-123">Next tooyour primary key connection string, choose hello copy button.</span></span>

    ![Másolja az Event Hubs névtér kapcsolati karakterláncot](media/connectors-create-api-azure-event-hubs/find-event-hub-namespace-connection-string.png)

    > [!TIP]
    > <span data-ttu-id="5144a-125">a kapcsolati karakterlánc társítva, az Event Hubs névtér vagy egy adott Eseményközpontban, hogy ellenőrizze hello kapcsolati karakterláncot hello tooconfirm `EntityPath` paraméter.</span><span class="sxs-lookup"><span data-stu-id="5144a-125">tooconfirm whether your connection string is associated with your Event Hubs namespace or with a specific Event Hub, check hello connection string for hello `EntityPath` parameter.</span></span> <span data-ttu-id="5144a-126">Ha ezt a paramétert, a hello kapcsolati karakterlánc az egy adott Eseményközpontban "entitás", és nincs hello megfelelő karakterlánc toouse a Logic Apps alkalmazást az.</span><span class="sxs-lookup"><span data-stu-id="5144a-126">If you find this parameter, hello connection string is for a specific Event Hub "entity", and is not hello correct string toouse with your logic app.</span></span>

4.  <span data-ttu-id="5144a-127">Amikor a rendszer kéri a hitelesítő adatokat az Event Hubs eseményindító vagy művelet tooyour logikai alkalmazás hozzáadása után, most is tooyour Event Hubs névtér elérheti.</span><span class="sxs-lookup"><span data-stu-id="5144a-127">Now when you're prompted for credentials after adding an Event Hubs trigger or action tooyour logic app, you can connect tooyour Event Hubs namespace.</span></span> <span data-ttu-id="5144a-128">Nevezze el a kapcsolatot, adjon meg hello kapcsolati karakterláncot, amely a másolt, és válassza a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="5144a-128">Give your connection a name, enter hello connection string that you copied, and choose **Create**.</span></span>

    ![Adja meg a kapcsolati karakterlánc az Event Hubs névtér](./media/connectors-create-api-azure-event-hubs/event-hubs-connection.png)

    <span data-ttu-id="5144a-130">Miután létrehozta a kapcsolatot, hello kapcsolat nevét meg kell jelennie hello Event Hubs eseményindító vagy a műveletet.</span><span class="sxs-lookup"><span data-stu-id="5144a-130">After you create your connection, hello connection name should appear in hello Event Hubs trigger or action.</span></span> 
    <span data-ttu-id="5144a-131">Ezután folytathatja a hello más lépéseket a Logic Apps alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="5144a-131">You can then continue with hello other steps in your logic app.</span></span>

    ![Event Hubs névtér kapcsolat létre](./media/connectors-create-api-azure-event-hubs/event-hubs-connection-created.png)

## <a name="start-workflow-when-your-event-hub-receives-new-events"></a><span data-ttu-id="5144a-133">Ha az Eseményközpont kap új események munkafolyamat indítása</span><span class="sxs-lookup"><span data-stu-id="5144a-133">Start workflow when your Event Hub receives new events</span></span>

<span data-ttu-id="5144a-134">A [ *eseményindító* ](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts) olyan esemény, amely a Logic Apps alkalmazást a munkamenet indítása.</span><span class="sxs-lookup"><span data-stu-id="5144a-134">A [*trigger*](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts) is an event that starts a workflow in your logic app.</span></span> <span data-ttu-id="5144a-135">Indítani egy munkafolyamatot, ha új események tooyour Eseményközpont küld, kövesse az alábbi lépéseket a hello eseményindító, melyeket észlel, ezt az eseményt hozzáadásához.</span><span class="sxs-lookup"><span data-stu-id="5144a-135">To start a workflow when new events are sent tooyour Event Hub, follow these steps for adding hello trigger that detects this event.</span></span>

1.  <span data-ttu-id="5144a-136">A hello [Azure-portálon](https://portal.azure.com "Azure-portálon"), nyissa meg tooyour meglévő Logic Apps alkalmazást, vagy egy üres logikai alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="5144a-136">In hello [Azure portal](https://portal.azure.com "Azure portal"), go tooyour existing logic app or create a blank logic app.</span></span>

2.  <span data-ttu-id="5144a-137">A Logic App Designer hello hello a keresési mezőbe, írja be a `event hubs` a szűrőhöz.</span><span class="sxs-lookup"><span data-stu-id="5144a-137">In hello search box for hello Logic App Designer, enter `event hubs` for your filter.</span></span> <span data-ttu-id="5144a-138">Válassza ki az ehhez az eseményindítóhoz: **mikor események érhetők el az Event Hubs**</span><span class="sxs-lookup"><span data-stu-id="5144a-138">Select this trigger: **When events are available in Event Hub**</span></span>

    ![Válassza ki a eseményindítója a következőnek: Ha az Eseményközpont kap új események](./media/connectors-create-api-azure-event-hubs/find-event-hubs-trigger.png)

    <span data-ttu-id="5144a-140">Ha még nem rendelkezik a kapcsolati tooyour Event Hubs-névtér, megkéri toocreate most ezt a kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="5144a-140">If you don't already have a connection tooyour Event Hubs namespace, you're prompted toocreate this connection now.</span></span> <span data-ttu-id="5144a-141">Nevezze el a kapcsolatot, és adjon meg az Event Hubs névtér hello kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="5144a-141">Give your connection a name, and enter hello connection string for your Event Hubs namespace.</span></span> 
    <span data-ttu-id="5144a-142">Szükség esetén további [hogyan toofind a kapcsolati karakterlánc](#permissions-connection-string).</span><span class="sxs-lookup"><span data-stu-id="5144a-142">If necessary, learn [how toofind your connection string](#permissions-connection-string).</span></span>

    ![Adja meg a kapcsolati karakterlánc az Event Hubs névtér](./media/connectors-create-api-azure-event-hubs/event-hubs-connection.png)

    <span data-ttu-id="5144a-144">Hello kapcsolat létrehozása után hello hello beállításainak **Ha olyan esemény az elérhető eseményközpontban** eseményindító jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="5144a-144">After you create hello connection, hello settings for hello **When an event in available in an Event Hub** trigger appear.</span></span>

    ![Ha az Eseményközpont kap új események eseményindító beállításai](./media/connectors-create-api-azure-event-hubs/event-hubs-trigger.png)

3.  <span data-ttu-id="5144a-146">Adja meg, vagy válassza ki a megjeleníteni kívánt toomonitor Eseményközpont hello hello nevét.</span><span class="sxs-lookup"><span data-stu-id="5144a-146">Enter or select hello name for hello Event Hub that you want toomonitor.</span></span> <span data-ttu-id="5144a-147">Válassza ki a hello gyakoriságát és milyen gyakran szeretné toocheck hello Eseményközpont intervallumát.</span><span class="sxs-lookup"><span data-stu-id="5144a-147">Select hello frequency and interval for how often you want toocheck hello Event Hub.</span></span>

    > [!TIP]
    > <span data-ttu-id="5144a-148">toooptionally válassza ki a fogyasztói csoportot események olvasása, válassza a **speciális beállítások megjelenítése**.</span><span class="sxs-lookup"><span data-stu-id="5144a-148">toooptionally select a consumer group for reading events, choose **Show advanced options**.</span></span> 

    ![Adja meg az Event Hubs vagy felhasználói csoport](./media/connectors-create-api-azure-event-hubs/event-hubs-trigger-details.png)

    <span data-ttu-id="5144a-150">Most létrehozott egy eseményindító toostart munkafolyamat a logikai alkalmazásnak.</span><span class="sxs-lookup"><span data-stu-id="5144a-150">You've now set up a trigger toostart a workflow for your logic app.</span></span> 
    <span data-ttu-id="5144a-151">A logikai alkalmazás ellenőrzi, hogy hello megadott Eseményközpont hello beállított ütemezés szerint alapján.</span><span class="sxs-lookup"><span data-stu-id="5144a-151">Your logic app checks hello specified Event Hub based on hello schedule that you set.</span></span> 
    <span data-ttu-id="5144a-152">Ha az alkalmazás új események hello Eseményközpont talál, hello eseményindító futtatja más műveletek vagy eseményindítók a Logic Apps alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="5144a-152">If your app finds new events in hello Event Hub, hello trigger runs other actions or triggers in your logic app.</span></span>

## <a name="send-events-tooyour-event-hub-from-your-logic-app"></a><span data-ttu-id="5144a-153">A Logic Apps alkalmazást események tooyour Eseményközpont küldése</span><span class="sxs-lookup"><span data-stu-id="5144a-153">Send events tooyour Event Hub from your logic app</span></span>

<span data-ttu-id="5144a-154">A [*művelet*](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts) a logikai alkalmazás munkafolyamata által végrehajtott feladat.</span><span class="sxs-lookup"><span data-stu-id="5144a-154">An [*action*](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts) is a task performed by your logic app workflow.</span></span> <span data-ttu-id="5144a-155">Egy eseményindító tooyour logikai alkalmazás hozzáadása után az, hogy az eseményindító által létrehozott adatok egy művelet tooperform műveletek is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="5144a-155">After you add a trigger tooyour logic app, you can add an action tooperform operations with data generated by that trigger.</span></span> <span data-ttu-id="5144a-156">egy esemény tooyour Eseményközpont a logic App toosend kövesse az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="5144a-156">toosend an event tooyour Event Hub from your logic app, follow these steps.</span></span>

1.  <span data-ttu-id="5144a-157">Logic App tervezőben, a logic app eseményindító alatt válassza a **új lépés** > **művelet hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="5144a-157">In Logic App Designer, under your logic app trigger, choose **New step** > **Add an action**.</span></span>

    ![Válassza a "Új lépés", majd "Művelet hozzáadása"](./media/connectors-create-api-azure-event-hubs/add-action.png)

    <span data-ttu-id="5144a-159">Most keresse meg és válassza ki egy művelet tooperform.</span><span class="sxs-lookup"><span data-stu-id="5144a-159">Now you can find and select an action tooperform.</span></span> 
    <span data-ttu-id="5144a-160">Azonban bármely művelet, például kiválaszthatja azt szeretnénk, ha hello Event Hubs művelet toosend események.</span><span class="sxs-lookup"><span data-stu-id="5144a-160">Although you can select any action, for this example, we want hello Event Hubs action toosend events.</span></span>

2.  <span data-ttu-id="5144a-161">Hello keresési mezőbe, írja be a `event hubs` a szűrőhöz.</span><span class="sxs-lookup"><span data-stu-id="5144a-161">In hello search box, enter `event hubs` for your filter.</span></span>
<span data-ttu-id="5144a-162">Ez a művelet kiválasztása: **küldési esemény**</span><span class="sxs-lookup"><span data-stu-id="5144a-162">Select this action: **Send event**</span></span>

    ![Válassza ki az "Event Hubs - küldési esemény" művelet](./media/connectors-create-api-azure-event-hubs/find-event-hubs-action.png)

3.  <span data-ttu-id="5144a-164">Írja be a szükséges hello adatait hello esemény, például a hello toosend hello esemény, ahová az Event Hubs hello nevét.</span><span class="sxs-lookup"><span data-stu-id="5144a-164">Enter hello required details for hello event, such as hello name for hello Event Hub where you want toosend hello event.</span></span> <span data-ttu-id="5144a-165">Írja be a hello eseményről, például tartalom esemény más választható adatait.</span><span class="sxs-lookup"><span data-stu-id="5144a-165">Enter any other optional details about hello event, such as content for that event.</span></span>

    > [!TIP]
    > <span data-ttu-id="5144a-166">toooptionally adja meg, ahol toosend hello esemény, hello Eseményközpont-partíción válasszon **speciális beállítások megjelenítése**.</span><span class="sxs-lookup"><span data-stu-id="5144a-166">toooptionally specify hello Event Hub partition where toosend hello event, choose **Show advanced options**.</span></span> 

    ![Adja meg az Eseményközpont nevét és opcionális esemény részletei](./media/connectors-create-api-azure-event-hubs/event-hubs-send-event-action.png)

6.  <span data-ttu-id="5144a-168">Mentse a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="5144a-168">Save your changes.</span></span>

    ![A logikai alkalmazás mentése](./media/connectors-create-api-azure-event-hubs/save-logic-app.png)

    <span data-ttu-id="5144a-170">Most létrehozott egy művelet toosend események a logikai alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="5144a-170">You've now set up an action toosend events from your logic app.</span></span> 

## <a name="connector-specific-details"></a><span data-ttu-id="5144a-171">Összekötő-specifikus részletei</span><span class="sxs-lookup"><span data-stu-id="5144a-171">Connector-specific details</span></span>

<span data-ttu-id="5144a-172">Bármely eseményindítók és hello swagger definiált műveletek megtekintése, és semmilyen határnak hello a Lásd még: [connector részleteket](/connectors/eventhubs/).</span><span class="sxs-lookup"><span data-stu-id="5144a-172">View any triggers and actions defined in hello swagger, and also see any limits in hello [connector details](/connectors/eventhubs/).</span></span> 

## <a name="get-help"></a><span data-ttu-id="5144a-173">Segítségkérés</span><span class="sxs-lookup"><span data-stu-id="5144a-173">Get help</span></span>

<span data-ttu-id="5144a-174">tooask kérdéseket, válasz kérdések és milyen egyéb Azure Logic Apps felhasználók végzi, tekintse meg a Microsoft hello [Azure Logic Apps-fórum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).</span><span class="sxs-lookup"><span data-stu-id="5144a-174">tooask questions, answer questions, and see what other Azure Logic Apps users are doing, visit hello [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).</span></span>

<span data-ttu-id="5144a-175">Logic Apps alkalmazások és összekötők javítása, szavazhatnak vagy küldje el a következő hello ötleteket toohelp [Logic Apps felhasználói visszajelzési webhelyet](http://aka.ms/logicapps-wish).</span><span class="sxs-lookup"><span data-stu-id="5144a-175">toohelp improve Logic Apps and connectors, vote on or submit ideas at hello [Logic Apps user feedback site](http://aka.ms/logicapps-wish).</span></span>

## <a name="next-steps"></a><span data-ttu-id="5144a-176">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5144a-176">Next steps</span></span>

*  [<span data-ttu-id="5144a-177">Más összekötők keresése Azure Logic Apps alkalmazások</span><span class="sxs-lookup"><span data-stu-id="5144a-177">Find other connectors for Azure Logic apps</span></span>](./apis-list.md)