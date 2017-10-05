---
title: "Állapotának, a naplózás beállítása és értesítéskérés - Azure Logic Apps |} Microsoft Docs"
description: "A figyelő állapotát és teljesítményét a logic apps diagnosztikai adatok naplózása, és értesítések beállítása"
author: jeffhollan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 5c1b1e15-3b6c-49dc-98a6-bdbe7cb75339
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 07/21/2017
ms.author: LADocs; jehollan
ms.openlocfilehash: 4795f5728d4ce6ff21b97bc3fefd6a53e0c6a11b
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="monitor-status-set-up-diagnostics-logging-and-turn-on-alerts-for-azure-logic-apps"></a><span data-ttu-id="2f47f-103">Állapotának figyelésére, diagnosztikai naplózás beállítása és az Azure Logic Apps riasztás bekapcsolása</span><span class="sxs-lookup"><span data-stu-id="2f47f-103">Monitor status, set up diagnostics logging, and turn on alerts for Azure Logic Apps</span></span>

<span data-ttu-id="2f47f-104">Miután [létrehozása és futtatása a logikai alkalmazás](../logic-apps/logic-apps-create-a-logic-app.md), ellenőrizheti a futtatása előzmények, eseményindító előzmények, állapotát és teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="2f47f-104">After you [create and run a logic app](../logic-apps/logic-apps-create-a-logic-app.md), you can check its runs history, trigger history, status, and performance.</span></span> <span data-ttu-id="2f47f-105">A valós idejű esemény figyelése és részletesebb hibakeresés, állítson be [diagnosztikai naplózás](#azure-diagnostics) a logikai alkalmazásnak.</span><span class="sxs-lookup"><span data-stu-id="2f47f-105">For real-time event monitoring and richer debugging, set up [diagnostics logging](#azure-diagnostics) for your logic app.</span></span> <span data-ttu-id="2f47f-106">Ily módon is [található, és tekintse meg az eseményeket](#find-events), például az eseményindító események, futtatási események és műveleti események.</span><span class="sxs-lookup"><span data-stu-id="2f47f-106">That way, you can [find and view events](#find-events), like trigger events, run events, and action events.</span></span> <span data-ttu-id="2f47f-107">Emellett ezzel [diagnosztikai adatok más szolgáltatásokkal](#extend-diagnostic-data), például az Azure Storage és az Azure Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="2f47f-107">You can also use this [diagnostics data with other services](#extend-diagnostic-data), like Azure Storage and Azure Event Hubs.</span></span> 

<span data-ttu-id="2f47f-108">Hibák vagy egyéb lehetséges problémákat kapcsolatos értesítéseket kapni, állítsa be a [riasztások](#add-azure-alerts).</span><span class="sxs-lookup"><span data-stu-id="2f47f-108">To get notifications about failures or other possible problems, set up [alerts](#add-azure-alerts).</span></span> <span data-ttu-id="2f47f-109">Például létrehozhat egy riasztást, amelyek észlelik a ""több mint öt nem egy óra alatt.</span><span class="sxs-lookup"><span data-stu-id="2f47f-109">For example, you can create an alert that detects "when more than five runs fail in an hour."</span></span> <span data-ttu-id="2f47f-110">Is beállíthat figyelését, figyelemmel kíséri, és a naplózást programozott módon segítségével [Azure Diagnostics esemény beállításai és a Tulajdonságok](#diagnostic-event-properties).</span><span class="sxs-lookup"><span data-stu-id="2f47f-110">You can also set up monitoring, tracking, and logging programmatically by using [Azure Diagnostics event settings and properties](#diagnostic-event-properties).</span></span>

## <a name="view-runs-and-trigger-history-for-your-logic-app"></a><span data-ttu-id="2f47f-111">Nézet futtatása és a Logic Apps alkalmazást eseményindító előzményei</span><span class="sxs-lookup"><span data-stu-id="2f47f-111">View runs and trigger history for your logic app</span></span>

1. <span data-ttu-id="2f47f-112">A Logic Apps alkalmazást az kereséséhez a [Azure-portálon](https://portal.azure.com), a fő Azure menüben válassza a **további szolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="2f47f-112">To find your logic app in the [Azure portal](https://portal.azure.com), on the main Azure menu, choose **More services**.</span></span> <span data-ttu-id="2f47f-113">A keresőmezőbe a "logic apps" található, és válassza a **a Logic apps**.</span><span class="sxs-lookup"><span data-stu-id="2f47f-113">In the search box, find "logic apps", and choose **Logic apps**.</span></span>

   ![A logikai alkalmazás keresése](./media/logic-apps-monitor-your-logic-apps/find-your-logic-app.png)

   <span data-ttu-id="2f47f-115">Az Azure-portálon az Azure-előfizetéshez társított összes logikai alkalmazást jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="2f47f-115">The Azure portal shows all the logic apps that are associated with your Azure subscription.</span></span> 

2. <span data-ttu-id="2f47f-116">Válassza ki a Logic Apps alkalmazást, majd válassza a **áttekintése**.</span><span class="sxs-lookup"><span data-stu-id="2f47f-116">Select your logic app, then choose **Overview**.</span></span>

   <span data-ttu-id="2f47f-117">Az Azure-portálon futtatása előzményeinek és a Logic Apps alkalmazást eseményindító előzményeit jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="2f47f-117">The Azure portal shows the runs history and trigger history for your logic app.</span></span> <span data-ttu-id="2f47f-118">Példa:</span><span class="sxs-lookup"><span data-stu-id="2f47f-118">For example:</span></span>

   ![Logikai alkalmazás fut előzmények és eseményindító előzményei](media/logic-apps-monitor-your-logic-apps/overview.png)

   * <span data-ttu-id="2f47f-120">**Futtatja az előzmények** jeleníti meg a logikai alkalmazásnak a futtatják.</span><span class="sxs-lookup"><span data-stu-id="2f47f-120">**Runs history** shows all the runs for your logic app.</span></span> 
   * <span data-ttu-id="2f47f-121">**Indítás, előzmények** a Logic Apps alkalmazást az eseményindító-tevékenység műveleteit jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="2f47f-121">**Trigger History** shows all the trigger activity for your logic app.</span></span>

   <span data-ttu-id="2f47f-122">A leírások, lásd: [hibaelhárítása a Logic Apps alkalmazást](../logic-apps/logic-apps-diagnosing-failures.md).</span><span class="sxs-lookup"><span data-stu-id="2f47f-122">For status descriptions, see [Troubleshoot your logic app](../logic-apps/logic-apps-diagnosing-failures.md).</span></span>

   > [!TIP]
   > <span data-ttu-id="2f47f-123">Ha nem találja az adatokat várt, az eszköztáron válassza **frissítése**.</span><span class="sxs-lookup"><span data-stu-id="2f47f-123">If you don't find the data that you expect, on the toolbar, choose **Refresh**.</span></span>

3. <span data-ttu-id="2f47f-124">A lépéseket egy adott futtatható megtekintéséhez az **előzmények fut**, válassza ki a futtató.</span><span class="sxs-lookup"><span data-stu-id="2f47f-124">To view the steps from a specific run, under **Runs history**, select that run.</span></span> 

   <span data-ttu-id="2f47f-125">A figyelő nézet futtató egyes lépéseit mutatja.</span><span class="sxs-lookup"><span data-stu-id="2f47f-125">The monitor view shows each step in that run.</span></span> <span data-ttu-id="2f47f-126">Példa:</span><span class="sxs-lookup"><span data-stu-id="2f47f-126">For example:</span></span>

   ![A megadott futtató műveletek](media/logic-apps-monitor-your-logic-apps/monitor-view-updated.png)

4. <span data-ttu-id="2f47f-128">A Futtatás kapcsolatos további információért kattintson a **futtatása részletek**.</span><span class="sxs-lookup"><span data-stu-id="2f47f-128">To get more details about the run, choose **Run Details**.</span></span> <span data-ttu-id="2f47f-129">Ezt az információt a lépéseket, állapot, bemeneti és kimeneti a Futtatás foglalja össze.</span><span class="sxs-lookup"><span data-stu-id="2f47f-129">This information summarizes the steps, status, inputs, and outputs for the run.</span></span> 

   ![Válassza a "Futtatás részletei"](media/logic-apps-monitor-your-logic-apps/run-details.png)

   <span data-ttu-id="2f47f-131">Például kaphat a Futtatás **korrelációs azonosító**, amely szükség lehet a használatakor a [REST API-t a Logic Apps](https://docs.microsoft.com/rest/api/logic).</span><span class="sxs-lookup"><span data-stu-id="2f47f-131">For example, you can get the run's **Correlation ID**, which you might need when you use the [REST API for Logic Apps](https://docs.microsoft.com/rest/api/logic).</span></span>

5. <span data-ttu-id="2f47f-132">Ahhoz, hogy egy adott lépésre kapcsolatos adatokat, válassza ki a lépés.</span><span class="sxs-lookup"><span data-stu-id="2f47f-132">To get details about a specific step, choose that step.</span></span> <span data-ttu-id="2f47f-133">Most adatait, például bemenetek, kimenetek és hibáit, és ismételje meg a lépés a tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="2f47f-133">You can now review details like inputs, outputs, and any errors that happened for that step.</span></span> <span data-ttu-id="2f47f-134">Példa:</span><span class="sxs-lookup"><span data-stu-id="2f47f-134">For example:</span></span>

   ![Lépés részletei](media/logic-apps-monitor-your-logic-apps/monitor-view-details.png)
   
   > [!NOTE]
   > <span data-ttu-id="2f47f-136">Az összes futásidejű adatokat és eseményeket a Logic Apps szolgáltatáson belül vannak titkosítva.</span><span class="sxs-lookup"><span data-stu-id="2f47f-136">All runtime details and events are encrypted within the Logic Apps service.</span></span> <span data-ttu-id="2f47f-137">Akkor lesznek visszafejtve, csak akkor, ha egy felhasználó kéri, hogy az adatok megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="2f47f-137">They are decrypted only when a user requests to view that data.</span></span> <span data-ttu-id="2f47f-138">Ezeket az eseményeket az elérésére is szabályozhatja [átruházásához hozzáférés-vezérlés (RBAC)](../active-directory/role-based-access-control-what-is.md).</span><span class="sxs-lookup"><span data-stu-id="2f47f-138">You can also control access to these events with [Azure Role-Based Access Control (RBAC)](../active-directory/role-based-access-control-what-is.md).</span></span>

6. <span data-ttu-id="2f47f-139">Ahhoz, hogy egy adott indítási esemény részleteit, lépjen vissza a **áttekintése** ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="2f47f-139">To get details about a specific trigger event, go back to the **Overview** pane.</span></span> <span data-ttu-id="2f47f-140">A **előzmények indítás**, jelölje be az indítási esemény.</span><span class="sxs-lookup"><span data-stu-id="2f47f-140">Under **Trigger history**, select the trigger event.</span></span> <span data-ttu-id="2f47f-141">Most már megtekintheti adatait, például bemenetekhez és kimenetekhez, például:</span><span class="sxs-lookup"><span data-stu-id="2f47f-141">You can now review details like inputs and outputs, for example:</span></span>

   ![Indítási esemény kimeneti részletei](media/logic-apps-monitor-your-logic-apps/trigger-details.png)

<a name="azure-diagnostics"></a>

## <a name="turn-on-diagnostics-logging-for-your-logic-app"></a><span data-ttu-id="2f47f-143">Diagnosztika a logikai alkalmazás naplózásának engedélyezése</span><span class="sxs-lookup"><span data-stu-id="2f47f-143">Turn on diagnostics logging for your logic app</span></span>

<span data-ttu-id="2f47f-144">Gazdagabb feltárására futásidejű adatokat és eseményeket, állíthatja be a diagnosztikai naplózás [Azure Naplóelemzés](../log-analytics/log-analytics-overview.md).</span><span class="sxs-lookup"><span data-stu-id="2f47f-144">For richer debugging with runtime details and events, you can set up diagnostics logging with [Azure Log Analytics](../log-analytics/log-analytics-overview.md).</span></span> <span data-ttu-id="2f47f-145">A Naplóelemzési rendszer szolgáltatása [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md) , amely figyeli a felhőben és a helyszíni környezetek karbantartásához azok rendelkezésre állását és teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="2f47f-145">Log Analytics is a service in [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md) that monitors your cloud and on-premises environments to help you maintain their availability and performance.</span></span> 

<span data-ttu-id="2f47f-146">Kezdés előtt kell az OMS-munkaterület rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="2f47f-146">Before you start, you need to have an OMS workspace.</span></span> <span data-ttu-id="2f47f-147">Ismerje meg, [OMS-munkaterület létrehozása](../log-analytics/log-analytics-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="2f47f-147">Learn [how to create an OMS workspace](../log-analytics/log-analytics-get-started.md).</span></span>

1. <span data-ttu-id="2f47f-148">Az a [Azure-portálon](https://portal.azure.com), keresse meg és jelölje meg a Logic Apps alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="2f47f-148">In the [Azure portal](https://portal.azure.com), find and select your logic app.</span></span> 

2. <span data-ttu-id="2f47f-149">A logic app panel menü alatti **figyelés**, válassza a **diagnosztika** > **diagnosztikai beállítások**.</span><span class="sxs-lookup"><span data-stu-id="2f47f-149">On the logic app blade menu, under **Monitoring**, choose **Diagnostics** > **Diagnostic Settings**.</span></span>

   ![Ugrás a figyelési, diagnosztikai, diagnosztikai beállítások](media/logic-apps-monitor-your-logic-apps/logic-app-diagnostics.png)

3. <span data-ttu-id="2f47f-151">A **diagnosztikai beállítások**, válassza a **a**.</span><span class="sxs-lookup"><span data-stu-id="2f47f-151">Under **Diagnostics settings**, choose **On**.</span></span>

   ![Kapcsolja be a diagnosztikai naplók](media/logic-apps-monitor-your-logic-apps/turn-on-diagnostics-logic-app.png)

4. <span data-ttu-id="2f47f-153">Naplózási OMS munkaterületet, és az esemény kategória most kijelölése látható módon:</span><span class="sxs-lookup"><span data-stu-id="2f47f-153">Now select the OMS workspace and event category for logging as shown:</span></span>

   1. <span data-ttu-id="2f47f-154">Válassza ki **küldeni a Naplóelemzési**.</span><span class="sxs-lookup"><span data-stu-id="2f47f-154">Select **Send to Log Analytics**.</span></span> 
   2. <span data-ttu-id="2f47f-155">A **Naplóelemzési**, válassza a **konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="2f47f-155">Under **Log Analytics**, choose **Configure**.</span></span> 
   3. <span data-ttu-id="2f47f-156">A **OMS-munkaterület**, jelölje be az OMS-munkaterület naplózásának használni.</span><span class="sxs-lookup"><span data-stu-id="2f47f-156">Under **OMS Workspaces**, select the OMS workspace to use for logging.</span></span>
   4. <span data-ttu-id="2f47f-157">A **napló**, jelölje be a **WorkflowRuntime** kategóriát.</span><span class="sxs-lookup"><span data-stu-id="2f47f-157">Under **Log**, select the **WorkflowRuntime** category.</span></span>
   5. <span data-ttu-id="2f47f-158">Válassza ki a metrika időközt.</span><span class="sxs-lookup"><span data-stu-id="2f47f-158">Choose the metric interval.</span></span>
   6. <span data-ttu-id="2f47f-159">Amikor elkészült, válassza ki a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="2f47f-159">When you're done, choose **Save**.</span></span>

   ![Válassza ki az OMS-munkaterület és a naplózási adatokat](media/logic-apps-monitor-your-logic-apps/send-diagnostics-data-log-analytics-workspace.png)

<span data-ttu-id="2f47f-161">Most található események és egyéb adatok eseményindító események, eseményeket és a műveleti események futtatják.</span><span class="sxs-lookup"><span data-stu-id="2f47f-161">Now, you can find events and other data for trigger events, run events, and action events.</span></span>

<a name="find-events"></a>

## <a name="find-events-and-data-for-your-logic-app"></a><span data-ttu-id="2f47f-162">A Logic Apps alkalmazást talált események és adatok</span><span class="sxs-lookup"><span data-stu-id="2f47f-162">Find events and data for your logic app</span></span>

<span data-ttu-id="2f47f-163">Található, és tekintse meg az eseményeket a Logic Apps alkalmazást, például Indítás, futtassa az eseményeket, és műveleti események, kövesse az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="2f47f-163">To find and view events in your logic app, like trigger events, run events, and action events, follow these steps.</span></span>

1. <span data-ttu-id="2f47f-164">Az a [Azure-portálon](https://portal.azure.com), válassza a **több szolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="2f47f-164">In the [Azure portal](https://portal.azure.com), choose **More Services**.</span></span> <span data-ttu-id="2f47f-165">Keresse meg a "naplóelemzési", és válassza a **Naplóelemzési** itt látható módon:</span><span class="sxs-lookup"><span data-stu-id="2f47f-165">Search for "log analytics", then choose **Log Analytics** as shown here:</span></span>

   ![Válassza ki a "Naplóelemzési"](media/logic-apps-monitor-your-logic-apps/browseloganalytics.png)

2. <span data-ttu-id="2f47f-167">A **Naplóelemzési**, található, és válassza ki az OMS-munkaterület.</span><span class="sxs-lookup"><span data-stu-id="2f47f-167">Under **Log Analytics**, find and select your OMS workspace.</span></span> 

   ![Az OMS-munkaterület kiválasztása](media/logic-apps-monitor-your-logic-apps/selectla.png)

3. <span data-ttu-id="2f47f-169">A **felügyeleti**, válassza a **OMS-portálon**.</span><span class="sxs-lookup"><span data-stu-id="2f47f-169">Under **Management**, choose **OMS Portal**.</span></span>

   ![Válassza ki a "OMS-portálon"](media/logic-apps-monitor-your-logic-apps/omsportalpage.png)

4. <span data-ttu-id="2f47f-171">Az OMS kezdőlapján válassza **naplófájl-keresési**.</span><span class="sxs-lookup"><span data-stu-id="2f47f-171">On your OMS home page, choose **Log Search**.</span></span>

   ![Az OMS kezdőlapján válassza a "Naplófájl-keresési"](media/logic-apps-monitor-your-logic-apps/logsearch.png)

   <span data-ttu-id="2f47f-173">– vagy –</span><span class="sxs-lookup"><span data-stu-id="2f47f-173">-or-</span></span>

   ![A OMS menüben válassza a "Naplófájl-keresési"](media/logic-apps-monitor-your-logic-apps/logsearch-2.png)

5. <span data-ttu-id="2f47f-175">A keresési mezőbe, adjon meg egy mező található, és nyomja le az ENTER kívánt **Enter**.</span><span class="sxs-lookup"><span data-stu-id="2f47f-175">In the search box, specify a field that you want to find, and press **Enter**.</span></span> <span data-ttu-id="2f47f-176">Amikor elkezdi beírni, OMS megjeleníti a lehetséges találatok és műveletek közül választhat.</span><span class="sxs-lookup"><span data-stu-id="2f47f-176">When you start typing, OMS shows you possible matches and operations that you can use.</span></span> 

   <span data-ttu-id="2f47f-177">Például az első 10 eseményeket, amelyek történtek található, adja meg, és válassza a keresési lekérdezés: **kategória = WorkflowRuntime |} felső 10**</span><span class="sxs-lookup"><span data-stu-id="2f47f-177">For example, to find the top 10 events that happened, enter and select this search query: **Category=WorkflowRuntime |top 10**</span></span>

   ![Adja meg a keresési karakterláncot](media/logic-apps-monitor-your-logic-apps/oms-start-query.png)

   <span data-ttu-id="2f47f-179">További információ [adatok megkeresése a Naplóelemzési](../log-analytics/log-analytics-log-searches.md).</span><span class="sxs-lookup"><span data-stu-id="2f47f-179">Learn more about [how to find data in Log Analytics](../log-analytics/log-analytics-log-searches.md).</span></span>

6. <span data-ttu-id="2f47f-180">Az eredmények lapon bal oldali sávon, válassza ki a megtekinteni kívánt időkeretet.</span><span class="sxs-lookup"><span data-stu-id="2f47f-180">On the results page, in the left bar, choose the timeframe that you want to view.</span></span>
<span data-ttu-id="2f47f-181">Pontosítsa a lekérdezést egy szűrő hozzáadásával, válassza a **+ Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="2f47f-181">To refine your query by adding a filter, choose **+Add**.</span></span>

   ![Válassza ki a lekérdezési eredmények időkeretre](media/logic-apps-monitor-your-logic-apps/query-results.png)

7. <span data-ttu-id="2f47f-183">A **szűrők hozzáadása**, így megtalálja a kívánt szűrőt, adja meg a szűrő nevét.</span><span class="sxs-lookup"><span data-stu-id="2f47f-183">Under **Add Filters**, enter the filter name so you can find the filter you want.</span></span> <span data-ttu-id="2f47f-184">Válassza ki a szűrőt, és válassza a **+ Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="2f47f-184">Select the filter, and choose **+Add**.</span></span>

   <span data-ttu-id="2f47f-185">A példában a "status" szó a sikertelen események kereséséhez **AzureDiagnostics**.</span><span class="sxs-lookup"><span data-stu-id="2f47f-185">This example uses the word "status" to find failed events under **AzureDiagnostics**.</span></span>
   <span data-ttu-id="2f47f-186">Itt a szűrő a **status_s** már be van jelölve.</span><span class="sxs-lookup"><span data-stu-id="2f47f-186">Here the filter for **status_s** is already selected.</span></span>

   ![Válassza ki a szűrő](media/logic-apps-monitor-your-logic-apps/log-search-add-filter.png)

8. <span data-ttu-id="2f47f-188">A bal oldali sávon, válassza ki a szűrő értéket használja, és válassza a kívánt **alkalmaz**.</span><span class="sxs-lookup"><span data-stu-id="2f47f-188">In the left bar, select the filter value that you want to use, and choose **Apply**.</span></span>

   ![Adja meg a szűrő értéket, válassza az "Alkalmaz"](media/logic-apps-monitor-your-logic-apps/log-search-apply-filter.png)

9. <span data-ttu-id="2f47f-190">Térjen vissza a lekérdezést, amely éppen összeállításakor.</span><span class="sxs-lookup"><span data-stu-id="2f47f-190">Now return to the query that you're building.</span></span> <span data-ttu-id="2f47f-191">A lekérdezés frissül a kijelölt szűrő és az értéke.</span><span class="sxs-lookup"><span data-stu-id="2f47f-191">Your query is updated with your selected filter and value.</span></span> <span data-ttu-id="2f47f-192">Az előző eredmények most túl szűrve.</span><span class="sxs-lookup"><span data-stu-id="2f47f-192">Your previous results are now filtered too.</span></span>

   ![Térjen vissza a lekérdezés szűrt eredményekkel](media/logic-apps-monitor-your-logic-apps/log-search-query-filtered-results.png)

10. <span data-ttu-id="2f47f-194">Jövőbeli használatra a lekérdezés mentéséhez válassza **mentése**.</span><span class="sxs-lookup"><span data-stu-id="2f47f-194">To save your query for future use, choose **Save**.</span></span> <span data-ttu-id="2f47f-195">Ismerje meg, [a lekérdezés mentése](../logic-apps/logic-apps-track-b2b-messages-omsportal-query-filter-control-number.md#save-oms-query).</span><span class="sxs-lookup"><span data-stu-id="2f47f-195">Learn [how to save your query](../logic-apps/logic-apps-track-b2b-messages-omsportal-query-filter-control-number.md#save-oms-query).</span></span>

<a name="extend-diagnostic-data"></a>

## <a name="extend-how-and-where-you-use-diagnostic-data-with-other-services"></a><span data-ttu-id="2f47f-196">Hogyan és hol diagnosztikai adatok használja más szolgáltatásokkal</span><span class="sxs-lookup"><span data-stu-id="2f47f-196">Extend how and where you use diagnostic data with other services</span></span>

<span data-ttu-id="2f47f-197">Azure Naplóelemzés, valamint bővítheti, hogyan használhatja a Logic Apps alkalmazást diagnosztikai adatok más Azure-szolgáltatásokkal, például:</span><span class="sxs-lookup"><span data-stu-id="2f47f-197">Along with Azure Log Analytics, you can extend how you use your logic app's diagnostic data with other Azure services, for example:</span></span> 

* [<span data-ttu-id="2f47f-198">Az Azure diagnosztikai naplók az Azure Storage archív</span><span class="sxs-lookup"><span data-stu-id="2f47f-198">Archive Azure Diagnostics Logs in Azure Storage</span></span>](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md)
* [<span data-ttu-id="2f47f-199">Az adatfolyam Azure diagnosztikai naplók az Azure Event hubs</span><span class="sxs-lookup"><span data-stu-id="2f47f-199">Stream Azure Diagnostics Logs to Azure Event Hubs</span></span>](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md) 

<span data-ttu-id="2f47f-200">Ezután a get valós idejű figyelés telemetriai adatok és más szolgáltatások analytics segítségével például [Azure Stream Analytics](../stream-analytics/stream-analytics-introduction.md) és [Power BI](../log-analytics/log-analytics-powerbi.md).</span><span class="sxs-lookup"><span data-stu-id="2f47f-200">You can then get real-time monitoring by using telemetry and analytics from other services, like [Azure Stream Analytics](../stream-analytics/stream-analytics-introduction.md) and [Power BI](../log-analytics/log-analytics-powerbi.md).</span></span> <span data-ttu-id="2f47f-201">Példa:</span><span class="sxs-lookup"><span data-stu-id="2f47f-201">For example:</span></span>

* [<span data-ttu-id="2f47f-202">Az adatfolyam Stream Analytics Eseményközpontokból származó adatokat</span><span class="sxs-lookup"><span data-stu-id="2f47f-202">Stream data from Event Hubs to Stream Analytics</span></span>](../stream-analytics/stream-analytics-define-inputs.md)
* [<span data-ttu-id="2f47f-203">A Stream Analytics adatfolyam-továbbítási adatok elemzése és a Power bi-ban a valós idejű elemzési irányítópult létrehozása</span><span class="sxs-lookup"><span data-stu-id="2f47f-203">Analyze streaming data with Stream Analytics and create a real-time analytics dashboard in Power BI</span></span>](../stream-analytics/stream-analytics-power-bi-dashboard.md)

<span data-ttu-id="2f47f-204">A beállítások határozzák meg, amely azt szeretné beállítani, győződjön meg arról, hogy Ön első [az Azure storage-fiók létrehozása](../storage/common/storage-create-storage-account.md) vagy [hozzon létre egy Azure event hubs](../event-hubs/event-hubs-create.md).</span><span class="sxs-lookup"><span data-stu-id="2f47f-204">Based on the options that you want set up, make sure that you first [create an Azure storage account](../storage/common/storage-create-storage-account.md) or [create an Azure event hub](../event-hubs/event-hubs-create.md).</span></span> <span data-ttu-id="2f47f-205">Válassza a beállítások érhetők el, hol szeretné elküldeni a diagnosztikai adatok:</span><span class="sxs-lookup"><span data-stu-id="2f47f-205">Then select the options for where you want to send diagnostic data:</span></span>

![Adatok küldése az Azure storage-fiók vagy esemény hub](./media/logic-apps-monitor-your-logic-apps/storage-account-event-hubs.png)

> [!NOTE]
> <span data-ttu-id="2f47f-207">Megőrzési időtartamú alkalmazása csak akkor, ha úgy dönt, hogy a tárfiókot használja.</span><span class="sxs-lookup"><span data-stu-id="2f47f-207">Retention periods apply only when you choose to use a storage account.</span></span>

<a name="add-azure-alerts"></a>

## <a name="set-up-alerts-for-your-logic-app"></a><span data-ttu-id="2f47f-208">A logikai alkalmazás beállítása</span><span class="sxs-lookup"><span data-stu-id="2f47f-208">Set up alerts for your logic app</span></span>

<span data-ttu-id="2f47f-209">Adott mérőszámok vagy a Logic Apps alkalmazást küszöbértékek túllépése figyeléséhez, állítsa be a [értesítések az Azure-ban](../monitoring-and-diagnostics/monitoring-overview-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="2f47f-209">To monitor specific metrics or exceeded thresholds for your logic app, set up [alerts in Azure](../monitoring-and-diagnostics/monitoring-overview-alerts.md).</span></span> <span data-ttu-id="2f47f-210">További tudnivalók [az Azure-ban mérőszámok](../monitoring-and-diagnostics/monitoring-overview-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="2f47f-210">Learn about [metrics in Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md).</span></span> 

<span data-ttu-id="2f47f-211">Riasztások nélkül beállítása [Azure Naplóelemzés](../log-analytics/log-analytics-overview.md), kövesse az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="2f47f-211">To set up alerts without [Azure Log Analytics](../log-analytics/log-analytics-overview.md), follow these steps.</span></span> <span data-ttu-id="2f47f-212">A riasztások feltételek és a műveletek, speciális [Naplóelemzési beállítása](#azure-diagnostics) túl.</span><span class="sxs-lookup"><span data-stu-id="2f47f-212">For more advanced alerts criteria and actions, [set up Log Analytics](#azure-diagnostics) too.</span></span>

1. <span data-ttu-id="2f47f-213">A logic app panel menü alatti **figyelés**, válassza a **diagnosztika** > **riasztási szabályok** > **riasztáshozzáadása**itt látható módon:</span><span class="sxs-lookup"><span data-stu-id="2f47f-213">On the logic app blade menu, under **Monitoring**, choose **Diagnostics** > **Alert rules** > **Add alert** as shown here:</span></span>

   ![A Logic Apps alkalmazást értesítések hozzáadása](media/logic-apps-monitor-your-logic-apps/set-up-alerts.png)

2. <span data-ttu-id="2f47f-215">A a **riasztási szabály felvétele** panelen, a riasztás létrehozása, látható módon:</span><span class="sxs-lookup"><span data-stu-id="2f47f-215">On the **Add an alert rule** blade, create your alert as shown:</span></span>

   1. <span data-ttu-id="2f47f-216">A **erőforrás**, válassza ki a logikai alkalmazás, ha nincsenek használatban.</span><span class="sxs-lookup"><span data-stu-id="2f47f-216">Under **Resource**, select your logic app, if not already selected.</span></span> 
   2. <span data-ttu-id="2f47f-217">Adjon meg egy nevének és leírásának megadása a riasztás.</span><span class="sxs-lookup"><span data-stu-id="2f47f-217">Give a name and description for your alert.</span></span>
   3. <span data-ttu-id="2f47f-218">Válassza ki a **metrika** vagy nyomon követni kívánt eseményt.</span><span class="sxs-lookup"><span data-stu-id="2f47f-218">Select a **Metric** or event that you want to track.</span></span>
   4. <span data-ttu-id="2f47f-219">Válassza ki a **feltétel**, adja meg egy **küszöbérték** metrika, és válassza ki a **időszak** Ez a metrika figyelésre.</span><span class="sxs-lookup"><span data-stu-id="2f47f-219">Select a **Condition**, specify a **Threshold** for the metric, and select the **Period** for monitoring this metric.</span></span>
   5. <span data-ttu-id="2f47f-220">Válassza ki, hogy a riasztás leveleket is küldhet.</span><span class="sxs-lookup"><span data-stu-id="2f47f-220">Select whether to send mail for the alert.</span></span> 
   6. <span data-ttu-id="2f47f-221">Adjon meg bármely más e-mail címet az értesítés küldéséhez.</span><span class="sxs-lookup"><span data-stu-id="2f47f-221">Specify any other email addresses for sending the alert.</span></span> 
   <span data-ttu-id="2f47f-222">A webhook URL-CÍMÉT, amelyen szeretné elküldeni a riasztás is megadható.</span><span class="sxs-lookup"><span data-stu-id="2f47f-222">You can also specify a webhook URL where you want to send the alert.</span></span>

   <span data-ttu-id="2f47f-223">Például ez a szabály küld riasztást, amikor öt vagy több futtatása sikertelen lesz, egy óra alatt:</span><span class="sxs-lookup"><span data-stu-id="2f47f-223">For example, this rule sends an alert when five or more runs fail in an hour:</span></span>

   ![Metrika riasztási szabály létrehozása](media/logic-apps-monitor-your-logic-apps/create-alert-rule.png)

> [!TIP]
> <span data-ttu-id="2f47f-225">Logikai alkalmazás futtatása egy riasztást, megadhatja a [kérelem eseményindító](../connectors/connectors-native-reqres.md) a munkafolyamat, amellyel feladatokhoz a példák mint:</span><span class="sxs-lookup"><span data-stu-id="2f47f-225">To run a logic app from an alert, you can include the [request trigger](../connectors/connectors-native-reqres.md) in your workflow, which lets you perform tasks like these examples:</span></span>
> 
> * [<span data-ttu-id="2f47f-226">A POST Slack-</span><span class="sxs-lookup"><span data-stu-id="2f47f-226">Post to Slack</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app)
> * [<span data-ttu-id="2f47f-227">A szöveg küldése</span><span class="sxs-lookup"><span data-stu-id="2f47f-227">Send a text</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app)
> * [<span data-ttu-id="2f47f-228">A várólista üzenet hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2f47f-228">Add a message to a queue</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app)

<a name="diagnostic-event-properties"></a>

## <a name="azure-diagnostics-event-settings-and-details"></a><span data-ttu-id="2f47f-229">Az Azure Diagnostics esemény beállításai és részletek</span><span class="sxs-lookup"><span data-stu-id="2f47f-229">Azure Diagnostics event settings and details</span></span>

<span data-ttu-id="2f47f-230">Minden egyes diagnosztikai esemény részleteit a Logic Apps alkalmazást és az esemény, például az állapot, a start idő, befejezési idő, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="2f47f-230">Each diagnostic event has details about your logic app and that event, for example, the status, start time, end time, and so on.</span></span> <span data-ttu-id="2f47f-231">Programozott módon állítsa be a figyelés, nyomon követését és naplózását, szervezeti egység használja ezeket az adatokat a a [REST API-t az Azure Logic Apps](https://docs.microsoft.com/rest/api/logic) és a [REST API-t az Azure Diagnostics](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftlogicworkflows).</span><span class="sxs-lookup"><span data-stu-id="2f47f-231">To programmatically set up monitoring, tracking, and logging, ou can use these details with the [REST API for Azure Logic Apps](https://docs.microsoft.com/rest/api/logic) and the [REST API for Azure Diagnostics](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftlogicworkflows).</span></span>

<span data-ttu-id="2f47f-232">Például a `ActionCompleted` eseménynek a `clientTrackingId` és `trackedProperties` nyomon követése és figyelésére használható tulajdonságokról:</span><span class="sxs-lookup"><span data-stu-id="2f47f-232">For example, the `ActionCompleted` event has the `clientTrackingId` and `trackedProperties` properties that you can use for tracking and monitoring:</span></span>

``` json
{
    "time": "2016-07-09T17:09:54.4773148Z",
    "workflowId": "/SUBSCRIPTIONS/<subscription-ID>/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/MYLOGICAPP",
    "resourceId": "/SUBSCRIPTIONS/<subscription-ID>/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/MYLOGICAPP/RUNS/08587361146922712057/ACTIONS/HTTP",
    "category": "WorkflowRuntime",
    "level": "Information",
    "operationName": "Microsoft.Logic/workflows/workflowActionCompleted",
    "properties": {
        "$schema": "2016-06-01",
        "startTime": "2016-07-09T17:09:53.4336305Z",
        "endTime": "2016-07-09T17:09:53.5430281Z",
        "status": "Succeeded",
        "code": "OK",
        "resource": {
            "subscriptionId": "<subscription-ID>",
            "resourceGroupName": "MyResourceGroup",
            "workflowId": "cff00d5458f944d5a766f2f9ad142553",
            "workflowName": "MyLogicApp",
            "runId": "08587361146922712057",
            "location": "westus",
            "actionName": "Http"
        },
        "correlation": {
            "actionTrackingId": "e1931543-906d-4d1d-baed-dee72ddf1047",
            "clientTrackingId": "<my-custom-tracking-ID>"
        },
        "trackedProperties": {
            "myTrackedProperty": "<value>"
        }
    }
}
```

* <span data-ttu-id="2f47f-233">`clientTrackingId`: Ha nincs megadott Azure automatikusan létrehozza ezt az Azonosítót, és korrelálja események futtatása logikai alkalmazás között, beleértve a beágyazott munkafolyamatok, amelyek nevezzük a logic App.</span><span class="sxs-lookup"><span data-stu-id="2f47f-233">`clientTrackingId`: If not provided, Azure automatically generates this ID and correlates events across a logic app run, including any nested workflows that are called from the logic app.</span></span> <span data-ttu-id="2f47f-234">Ez az azonosító egy úgy, hogy manuálisan megadhatja egy `x-ms-client-tracking-id` fejléc, az egyéni azonosító értéket ad meg a kérésére.</span><span class="sxs-lookup"><span data-stu-id="2f47f-234">You can manually specify this ID from a trigger by passing a `x-ms-client-tracking-id` header with your custom ID value in the trigger request.</span></span> <span data-ttu-id="2f47f-235">A kérelem eseményindító, a HTTP-eseményindítóval, illetve a webhook eseményindító is használhatja.</span><span class="sxs-lookup"><span data-stu-id="2f47f-235">You can use a request trigger, HTTP trigger, or webhook trigger.</span></span>

* <span data-ttu-id="2f47f-236">`trackedProperties`: A bemeneti vagy kimeneti diagnosztikai adatok követheti nyomon, akkor a nyomon követett tulajdonságokat adhat hozzá műveletek a Logic Apps alkalmazást JSON-definícióban.</span><span class="sxs-lookup"><span data-stu-id="2f47f-236">`trackedProperties`: To track inputs or outputs in diagnostics data, you can add tracked properties to actions in your logic app's JSON definition.</span></span> <span data-ttu-id="2f47f-237">A nyomon követett tulajdonságok követheti nyomon, csak egyetlen művelettel bemenetekhez és kimenetekhez, de használhatja a `correlation` eseményeket, a Futtatás műveletek között tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="2f47f-237">Tracked properties can track only a single action's inputs and outputs, but you can use the `correlation` properties of events to correlate across actions in a run.</span></span>

  <span data-ttu-id="2f47f-238">Egy vagy több tulajdonságának nyomon követésére, vegye fel a `trackedProperties` szakasz és a tulajdonságokat, a művelet definíciójához.</span><span class="sxs-lookup"><span data-stu-id="2f47f-238">To track one or more properties, add the `trackedProperties` section and the properties you want to the action definition.</span></span> <span data-ttu-id="2f47f-239">Tegyük fel például, például egy "order Azonosítóra" a telemetriai adatok nyomon követni kívánt:</span><span class="sxs-lookup"><span data-stu-id="2f47f-239">For example, suppose you want to track data like an "order ID" in your telemetry:</span></span>

  ``` json
  "myAction": {
    "type": "http",
    "inputs": {
        "uri": "http://uri",
        "headers": {
            "Content-Type": "application/json"
        },
        "body": "@triggerBody()"
    },
    "trackedProperties": {
        "myActionHTTPStatusCode": "@action()['outputs']['statusCode']",
        "myActionHTTPValue": "@action()['outputs']['body']['<content>']",
        "transactionId": "@action()['inputs']['body']['<content>']"
    }
  }
  ```

## <a name="next-steps"></a><span data-ttu-id="2f47f-240">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2f47f-240">Next steps</span></span>

* [<span data-ttu-id="2f47f-241">A logic app központi telepítési sablonok létrehozása és a kiadáskezelés</span><span class="sxs-lookup"><span data-stu-id="2f47f-241">Create templates for logic app deployment and release management</span></span>](../logic-apps/logic-apps-create-deploy-template.md)
* [<span data-ttu-id="2f47f-242">A vállalati integrációs csomag B2B forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="2f47f-242">B2B scenarios with Enterprise Integration Pack</span></span>](../logic-apps/logic-apps-enterprise-integration-overview.md)
* [<span data-ttu-id="2f47f-243">B2B üzenetek megfigyelése</span><span class="sxs-lookup"><span data-stu-id="2f47f-243">Monitor B2B messages</span></span>](../logic-apps/logic-apps-monitor-b2b-message.md)