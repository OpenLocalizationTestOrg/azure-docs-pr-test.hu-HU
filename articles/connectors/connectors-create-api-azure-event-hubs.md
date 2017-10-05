---
title: "Az Azure Event Hubs eseményfigyelő beállítása az Azure Logic Apps |} Microsoft Docs"
description: "Az adatfolyamok események fogadásához, és elküldje az eseményeket az Azure Logic Apps az Azure Event Hubs figyelése"
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
ms.openlocfilehash: 2ca27fb8269d1796fb1181fc4d0a8744a592d548
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="monitor-receive-and-send-events-with-the-event-hubs-connector"></a><span data-ttu-id="ed92d-104">Figyelése, fogadására és küldi az eseményeket az Event Hubs-összekötő használatával</span><span class="sxs-lookup"><span data-stu-id="ed92d-104">Monitor, receive, and send events with the Event Hubs connector</span></span>

<span data-ttu-id="ed92d-105">Az eseményfigyelő beállítása, hogy a Logic Apps alkalmazást képesek észlelni események, képes eseményeket fogadni, és elküldje az eseményeket, csatlakoznia kell egy [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs) a logikai alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="ed92d-105">To set up an event monitor so that your logic app can detect events, receive events, and send events, connect to an [Azure Event Hub](https://azure.microsoft.com/services/event-hubs) from your logic app.</span></span> <span data-ttu-id="ed92d-106">További információ [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md).</span><span class="sxs-lookup"><span data-stu-id="ed92d-106">Learn more about [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md).</span></span>

## <a name="requirements"></a><span data-ttu-id="ed92d-107">Követelmények</span><span class="sxs-lookup"><span data-stu-id="ed92d-107">Requirements</span></span>

* <span data-ttu-id="ed92d-108">Rendelkeznie kell egy [Event Hubs-névteret és Eseményközpont](../event-hubs/event-hubs-create.md) az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="ed92d-108">You have to have an [Event Hubs namespace and Event Hub](../event-hubs/event-hubs-create.md) in Azure.</span></span> <span data-ttu-id="ed92d-109">Ismerje meg, [az Event Hubs névtér és az Eseményközpont létrehozása](../event-hubs/event-hubs-create.md).</span><span class="sxs-lookup"><span data-stu-id="ed92d-109">Learn [how to create an Event Hubs namespace and Event Hub](../event-hubs/event-hubs-create.md).</span></span> 

* <span data-ttu-id="ed92d-110">Használandó [a csatlakozókat](https://docs.microsoft.com/azure/connectors/apis-list) a Logic Apps alkalmazást fel kell először hozzon létre egy logikai alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="ed92d-110">To use [any connector](https://docs.microsoft.com/azure/connectors/apis-list) in your logic app, you have to create a logic app first.</span></span> <span data-ttu-id="ed92d-111">Ismerje meg, [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="ed92d-111">Learn [how to create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

<a name="permissions-connection-string"></a>
## <a name="check-event-hubs-namespace-permissions-and-find-the-connection-string"></a><span data-ttu-id="ed92d-112">Ellenőrizze az Event Hubs névtér engedélyeit, és keresse meg a kapcsolati karakterláncot</span><span class="sxs-lookup"><span data-stu-id="ed92d-112">Check Event Hubs namespace permissions and find the connection string</span></span>

<span data-ttu-id="ed92d-113">A logikai alkalmazásnak, a szolgáltatás eléréséhez, létre kell hoznia egy [ *kapcsolat* ](./connectors-overview.md) között a Logic Apps alkalmazást és a szolgáltatást, ha még nem tette meg.</span><span class="sxs-lookup"><span data-stu-id="ed92d-113">For your logic app to access any service, you have to create a [*connection*](./connectors-overview.md) between your logic app and the service, if you haven't already.</span></span> <span data-ttu-id="ed92d-114">Ez a kapcsolat engedélyezi a Logic Apps alkalmazást adatok eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="ed92d-114">This connection authorizes your logic app to access data.</span></span>
<span data-ttu-id="ed92d-115">A logikai alkalmazásnak az Eseményközpont eléréséhez, akkor szükség **kezelése** engedélyek és a kapcsolati karakterlánc az Event Hubs-névtérhez.</span><span class="sxs-lookup"><span data-stu-id="ed92d-115">For your logic app to access your Event Hub, you have to have **Manage** permissions and the connection string for your Event Hubs namespace.</span></span>

<span data-ttu-id="ed92d-116">Ellenőrizze az engedélyeit, és a kapcsolati karakterláncot, kövesse az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="ed92d-116">To check your permissions and get the connection string, follow these steps.</span></span>

1.  <span data-ttu-id="ed92d-117">Jelentkezzen be az [Azure Portalra](https://portal.azure.com "Azure Portal")</span><span class="sxs-lookup"><span data-stu-id="ed92d-117">Sign in to the [Azure portal](https://portal.azure.com "Azure portal").</span></span> 

2.  <span data-ttu-id="ed92d-118">Nyissa meg az Event hubs *névtér*, nem az adott Eseményközpontban.</span><span class="sxs-lookup"><span data-stu-id="ed92d-118">Go to your Event Hubs *namespace*, not the specific Event Hub.</span></span> <span data-ttu-id="ed92d-119">A névtér panelen a **beállítások**, válassza a **megosztott elérési házirendek**.</span><span class="sxs-lookup"><span data-stu-id="ed92d-119">On the namespace blade, under **Settings**, choose **Shared access policies**.</span></span> <span data-ttu-id="ed92d-120">A **jogcímek**, ellenőrizze, hogy rendelkezik **kezelése** engedélyeket a névtérhez.</span><span class="sxs-lookup"><span data-stu-id="ed92d-120">Under **Claims**, check that you have **Manage** permissions for that namespace.</span></span>

    ![Az Event Hubs névtér engedélyeinek kezelése](./media/connectors-create-api-azure-event-hubs/event-hubs-namespace.png)

3.  <span data-ttu-id="ed92d-122">Másolja a kapcsolati karakterláncot az Event Hubs névtérhez, válassza a **RootManageSharedAccessKey**.</span><span class="sxs-lookup"><span data-stu-id="ed92d-122">To copy the connection string for the Event Hubs namespace, choose **RootManageSharedAccessKey**.</span></span> <span data-ttu-id="ed92d-123">Mellett az elsődleges kulcs kapcsolati karakterláncot válassza a Másolás gombra.</span><span class="sxs-lookup"><span data-stu-id="ed92d-123">Next to your primary key connection string, choose the copy button.</span></span>

    ![Másolja az Event Hubs névtér kapcsolati karakterláncot](media/connectors-create-api-azure-event-hubs/find-event-hub-namespace-connection-string.png)

    > [!TIP]
    > <span data-ttu-id="ed92d-125">Győződjön meg arról, hogy a kapcsolati karakterlánc tartozik az Event Hubs névtér vagy egy adott Eseményközpontban, ellenőrizze a kapcsolati karakterláncot a `EntityPath` paraméter.</span><span class="sxs-lookup"><span data-stu-id="ed92d-125">To confirm whether your connection string is associated with your Event Hubs namespace or with a specific Event Hub, check the connection string for the `EntityPath` parameter.</span></span> <span data-ttu-id="ed92d-126">Ha ezt a paramétert, a kapcsolati karakterlánc az egy adott Eseményközpontban "entitás", pedig nem a megfelelő karakterlánc használata a Logic Apps alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="ed92d-126">If you find this parameter, the connection string is for a specific Event Hub "entity", and is not the correct string to use with your logic app.</span></span>

4.  <span data-ttu-id="ed92d-127">Most amikor a rendszer kéri a hitelesítő adatokat az Event Hubs eseményindító vagy a művelet a Logic Apps alkalmazást való hozzáadását követően, csatlakozhat az Event Hubs névtér.</span><span class="sxs-lookup"><span data-stu-id="ed92d-127">Now when you're prompted for credentials after adding an Event Hubs trigger or action to your logic app, you can connect to your Event Hubs namespace.</span></span> <span data-ttu-id="ed92d-128">Nevezze el a kapcsolatot, adja meg a kapcsolati karakterláncot, amely a másolt, és válassza a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="ed92d-128">Give your connection a name, enter the connection string that you copied, and choose **Create**.</span></span>

    ![Adja meg a kapcsolati karakterlánc az Event Hubs névtér](./media/connectors-create-api-azure-event-hubs/event-hubs-connection.png)

    <span data-ttu-id="ed92d-130">Miután létrehozta a kapcsolatot, a kapcsolat nevét meg kell jelennie az Eseménynapló hubok eseményindító vagy műveletet.</span><span class="sxs-lookup"><span data-stu-id="ed92d-130">After you create your connection, the connection name should appear in the Event Hubs trigger or action.</span></span> 
    <span data-ttu-id="ed92d-131">Majd folytathatja a többi lépés a a Logic Apps alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="ed92d-131">You can then continue with the other steps in your logic app.</span></span>

    ![Event Hubs névtér kapcsolat létre](./media/connectors-create-api-azure-event-hubs/event-hubs-connection-created.png)

## <a name="start-workflow-when-your-event-hub-receives-new-events"></a><span data-ttu-id="ed92d-133">Ha az Eseményközpont kap új események munkafolyamat indítása</span><span class="sxs-lookup"><span data-stu-id="ed92d-133">Start workflow when your Event Hub receives new events</span></span>

<span data-ttu-id="ed92d-134">A [ *eseményindító* ](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts) olyan esemény, amely a Logic Apps alkalmazást a munkamenet indítása.</span><span class="sxs-lookup"><span data-stu-id="ed92d-134">A [*trigger*](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts) is an event that starts a workflow in your logic app.</span></span> <span data-ttu-id="ed92d-135">Indítani egy munkafolyamatot, ha új események küldhetők az Eseményközpontba, kövesse az alábbi lépéseket az eseményindító által észlelt Ez az esemény felvételéhez.</span><span class="sxs-lookup"><span data-stu-id="ed92d-135">To start a workflow when new events are sent to your Event Hub, follow these steps for adding the trigger that detects this event.</span></span>

1.  <span data-ttu-id="ed92d-136">Az a [Azure-portálon](https://portal.azure.com "Azure-portálon"), látogasson el a meglévő Logic Apps alkalmazást, vagy hozzon létre egy üres logikai alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="ed92d-136">In the [Azure portal](https://portal.azure.com "Azure portal"), go to your existing logic app or create a blank logic app.</span></span>

2.  <span data-ttu-id="ed92d-137">Írja be a keresőmezőbe a Logic App Designer `event hubs` a szűrőhöz.</span><span class="sxs-lookup"><span data-stu-id="ed92d-137">In the search box for the Logic App Designer, enter `event hubs` for your filter.</span></span> <span data-ttu-id="ed92d-138">Válassza ki az ehhez az eseményindítóhoz: **mikor események érhetők el az Event Hubs**</span><span class="sxs-lookup"><span data-stu-id="ed92d-138">Select this trigger: **When events are available in Event Hub**</span></span>

    ![Válassza ki a eseményindítója a következőnek: Ha az Eseményközpont kap új események](./media/connectors-create-api-azure-event-hubs/find-event-hubs-trigger.png)

    <span data-ttu-id="ed92d-140">Ha még nem rendelkezik a kapcsolat az Event Hubs névtérhez, felszólítást hozzon létre most ezt a kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="ed92d-140">If you don't already have a connection to your Event Hubs namespace, you're prompted to create this connection now.</span></span> <span data-ttu-id="ed92d-141">Nevezze el a kapcsolatot, és adja meg a kapcsolati karakterláncot az Event Hubs névtérhez.</span><span class="sxs-lookup"><span data-stu-id="ed92d-141">Give your connection a name, and enter the connection string for your Event Hubs namespace.</span></span> 
    <span data-ttu-id="ed92d-142">Szükség esetén további [a kapcsolati karakterlánc megkeresése](#permissions-connection-string).</span><span class="sxs-lookup"><span data-stu-id="ed92d-142">If necessary, learn [how to find your connection string](#permissions-connection-string).</span></span>

    ![Adja meg a kapcsolati karakterlánc az Event Hubs névtér](./media/connectors-create-api-azure-event-hubs/event-hubs-connection.png)

    <span data-ttu-id="ed92d-144">Miután létrehozta a kapcsolatot, a beállításait a **Ha olyan esemény az elérhető eseményközpontban** eseményindító jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="ed92d-144">After you create the connection, the settings for the **When an event in available in an Event Hub** trigger appear.</span></span>

    ![Ha az Eseményközpont kap új események eseményindító beállításai](./media/connectors-create-api-azure-event-hubs/event-hubs-trigger.png)

3.  <span data-ttu-id="ed92d-146">Adja meg, vagy válassza ki a figyelni kívánt Eseményközpont nevét.</span><span class="sxs-lookup"><span data-stu-id="ed92d-146">Enter or select the name for the Event Hub that you want to monitor.</span></span> <span data-ttu-id="ed92d-147">Válassza ki a gyakoriságát és milyen gyakran az Event Hubs ellenőrizni kívánja intervallumát.</span><span class="sxs-lookup"><span data-stu-id="ed92d-147">Select the frequency and interval for how often you want to check the Event Hub.</span></span>

    > [!TIP]
    > <span data-ttu-id="ed92d-148">Opcionálisan válassza ki a fogyasztói csoportot események olvasása, válassza a **speciális beállítások megjelenítése**.</span><span class="sxs-lookup"><span data-stu-id="ed92d-148">To optionally select a consumer group for reading events, choose **Show advanced options**.</span></span> 

    ![Adja meg az Event Hubs vagy felhasználói csoport](./media/connectors-create-api-azure-event-hubs/event-hubs-trigger-details.png)

    <span data-ttu-id="ed92d-150">Most már beállította egy eseményindító indítani egy munkafolyamatot a logikai alkalmazásnak.</span><span class="sxs-lookup"><span data-stu-id="ed92d-150">You've now set up a trigger to start a workflow for your logic app.</span></span> 
    <span data-ttu-id="ed92d-151">A Logic Apps alkalmazást az adott Event Hubs beállított ütemezés szerint ellenőrzi.</span><span class="sxs-lookup"><span data-stu-id="ed92d-151">Your logic app checks the specified Event Hub based on the schedule that you set.</span></span> 
    <span data-ttu-id="ed92d-152">Ha az alkalmazás megkeresi az új eseményeket az Eseménynapló Hub, az eseményindító más műveletek futtatása vagy elindítja a Logic Apps alkalmazást a.</span><span class="sxs-lookup"><span data-stu-id="ed92d-152">If your app finds new events in the Event Hub, the trigger runs other actions or triggers in your logic app.</span></span>

## <a name="send-events-to-your-event-hub-from-your-logic-app"></a><span data-ttu-id="ed92d-153">Események küldése az Eseményközpont a logikai alkalmazás</span><span class="sxs-lookup"><span data-stu-id="ed92d-153">Send events to your Event Hub from your logic app</span></span>

<span data-ttu-id="ed92d-154">A [*művelet*](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts) a logikai alkalmazás munkafolyamata által végrehajtott feladat.</span><span class="sxs-lookup"><span data-stu-id="ed92d-154">An [*action*](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts) is a task performed by your logic app workflow.</span></span> <span data-ttu-id="ed92d-155">Miután hozzáadott egy eseményindítót a logikai alkalmazáshoz, hozzáadhat egy műveletet is, amely további műveleteket végez el az eseményindító által előállított adatokkal.</span><span class="sxs-lookup"><span data-stu-id="ed92d-155">After you add a trigger to your logic app, you can add an action to perform operations with data generated by that trigger.</span></span> <span data-ttu-id="ed92d-156">Esemény küldése az Eseményközpont a logic App, kövesse az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="ed92d-156">To send an event to your Event Hub from your logic app, follow these steps.</span></span>

1.  <span data-ttu-id="ed92d-157">Logic App tervezőben, a logic app eseményindító alatt válassza a **új lépés** > **művelet hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="ed92d-157">In Logic App Designer, under your logic app trigger, choose **New step** > **Add an action**.</span></span>

    ![Válassza a "Új lépés", majd "Művelet hozzáadása"](./media/connectors-create-api-azure-event-hubs/add-action.png)

    <span data-ttu-id="ed92d-159">Most keresse meg és jelölje ki a végrehajtani kívánt műveletet.</span><span class="sxs-lookup"><span data-stu-id="ed92d-159">Now you can find and select an action to perform.</span></span> 
    <span data-ttu-id="ed92d-160">Azonban bármely művelet, például kiválaszthatja azt szeretnénk, az Event Hubs művelet is küldi az eseményeket.</span><span class="sxs-lookup"><span data-stu-id="ed92d-160">Although you can select any action, for this example, we want the Event Hubs action to send events.</span></span>

2.  <span data-ttu-id="ed92d-161">A keresési mezőbe, írja be a `event hubs` a szűrőhöz.</span><span class="sxs-lookup"><span data-stu-id="ed92d-161">In the search box, enter `event hubs` for your filter.</span></span>
<span data-ttu-id="ed92d-162">Ez a művelet kiválasztása: **küldési esemény**</span><span class="sxs-lookup"><span data-stu-id="ed92d-162">Select this action: **Send event**</span></span>

    ![Válassza ki az "Event Hubs - küldési esemény" művelet](./media/connectors-create-api-azure-event-hubs/find-event-hubs-action.png)

3.  <span data-ttu-id="ed92d-164">Az esemény, például az Event Hubs, ha szeretne küldeni az esemény nevét adja meg a szükséges adatokat.</span><span class="sxs-lookup"><span data-stu-id="ed92d-164">Enter the required details for the event, such as the name for the Event Hub where you want to send the event.</span></span> <span data-ttu-id="ed92d-165">Adja meg az esemény, például tartalom esemény bármely egyéb választható részleteit.</span><span class="sxs-lookup"><span data-stu-id="ed92d-165">Enter any other optional details about the event, such as content for that event.</span></span>

    > [!TIP]
    > <span data-ttu-id="ed92d-166">Igény szerint adja meg az Eseményközpont-partíción hova küldje a esemény, válassza a **speciális beállítások megjelenítése**.</span><span class="sxs-lookup"><span data-stu-id="ed92d-166">To optionally specify the Event Hub partition where to send the event, choose **Show advanced options**.</span></span> 

    ![Adja meg az Eseményközpont nevét és opcionális esemény részletei](./media/connectors-create-api-azure-event-hubs/event-hubs-send-event-action.png)

6.  <span data-ttu-id="ed92d-168">Mentse a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="ed92d-168">Save your changes.</span></span>

    ![A logikai alkalmazás mentése](./media/connectors-create-api-azure-event-hubs/save-logic-app.png)

    <span data-ttu-id="ed92d-170">Most már beállította művelet események küldése a Logic Apps alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="ed92d-170">You've now set up an action to send events from your logic app.</span></span> 

## <a name="connector-specific-details"></a><span data-ttu-id="ed92d-171">Összekötő-specifikus részletei</span><span class="sxs-lookup"><span data-stu-id="ed92d-171">Connector-specific details</span></span>

<span data-ttu-id="ed92d-172">Bármely eseményindítók és a swagger definiált műveletek megtekintése, és semmilyen határnak a Lásd még: a [connector részleteket](/connectors/eventhubs/).</span><span class="sxs-lookup"><span data-stu-id="ed92d-172">View any triggers and actions defined in the swagger, and also see any limits in the [connector details](/connectors/eventhubs/).</span></span> 

## <a name="get-help"></a><span data-ttu-id="ed92d-173">Segítségkérés</span><span class="sxs-lookup"><span data-stu-id="ed92d-173">Get help</span></span>

<span data-ttu-id="ed92d-174">Látogasson el az [Azure Logic Apps fórumára](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps), ahol kérdéseket tehet fel és válaszolhat meg, valamint megtudhatja, mivel foglalkoznak az Azure Logic Apps más felhasználói.</span><span class="sxs-lookup"><span data-stu-id="ed92d-174">To ask questions, answer questions, and see what other Azure Logic Apps users are doing, visit the [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).</span></span>

<span data-ttu-id="ed92d-175">Ha szeretne segíteni a Logic Apps és összekötők fejlesztésében, szavazzon vagy küldje el javaslatait a [Logic Apps felhasználói visszajelzések oldalon](http://aka.ms/logicapps-wish).</span><span class="sxs-lookup"><span data-stu-id="ed92d-175">To help improve Logic Apps and connectors, vote on or submit ideas at the [Logic Apps user feedback site](http://aka.ms/logicapps-wish).</span></span>

## <a name="next-steps"></a><span data-ttu-id="ed92d-176">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ed92d-176">Next steps</span></span>

*  [<span data-ttu-id="ed92d-177">Más összekötők keresése Azure Logic Apps alkalmazások</span><span class="sxs-lookup"><span data-stu-id="ed92d-177">Find other connectors for Azure Logic apps</span></span>](./apis-list.md)