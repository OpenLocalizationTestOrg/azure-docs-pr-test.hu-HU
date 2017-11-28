---
title: "aaaCheck állapotát, a naplózás beállítása és értesítéskérés - Azure Logic Apps |} Microsoft Docs"
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
ms.openlocfilehash: 81f186e11a669b710f4c06089597eb5a76f7a44e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-status-set-up-diagnostics-logging-and-turn-on-alerts-for-azure-logic-apps"></a><span data-ttu-id="01e99-103">Állapotának figyelésére, diagnosztikai naplózás beállítása és az Azure Logic Apps riasztás bekapcsolása</span><span class="sxs-lookup"><span data-stu-id="01e99-103">Monitor status, set up diagnostics logging, and turn on alerts for Azure Logic Apps</span></span>

<span data-ttu-id="01e99-104">Miután [létrehozása és futtatása a logikai alkalmazás](../logic-apps/logic-apps-create-a-logic-app.md), ellenőrizheti a futtatása előzmények, eseményindító előzmények, állapotát és teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="01e99-104">After you [create and run a logic app](../logic-apps/logic-apps-create-a-logic-app.md), you can check its runs history, trigger history, status, and performance.</span></span> <span data-ttu-id="01e99-105">A valós idejű esemény figyelése és részletesebb hibakeresés, állítson be [diagnosztikai naplózás](#azure-diagnostics) a logikai alkalmazásnak.</span><span class="sxs-lookup"><span data-stu-id="01e99-105">For real-time event monitoring and richer debugging, set up [diagnostics logging](#azure-diagnostics) for your logic app.</span></span> <span data-ttu-id="01e99-106">Ily módon is [található, és tekintse meg az eseményeket](#find-events), például az eseményindító események, futtatási események és műveleti események.</span><span class="sxs-lookup"><span data-stu-id="01e99-106">That way, you can [find and view events](#find-events), like trigger events, run events, and action events.</span></span> <span data-ttu-id="01e99-107">Emellett ezzel [diagnosztikai adatok más szolgáltatásokkal](#extend-diagnostic-data), például az Azure Storage és az Azure Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="01e99-107">You can also use this [diagnostics data with other services](#extend-diagnostic-data), like Azure Storage and Azure Event Hubs.</span></span> 

<span data-ttu-id="01e99-108">hibák vagy egyéb lehetséges problémákat tooget értesítések beállítása [riasztások](#add-azure-alerts).</span><span class="sxs-lookup"><span data-stu-id="01e99-108">tooget notifications about failures or other possible problems, set up [alerts](#add-azure-alerts).</span></span> <span data-ttu-id="01e99-109">Például létrehozhat egy riasztást, amelyek észlelik a ""több mint öt nem egy óra alatt.</span><span class="sxs-lookup"><span data-stu-id="01e99-109">For example, you can create an alert that detects "when more than five runs fail in an hour."</span></span> <span data-ttu-id="01e99-110">Is beállíthat figyelését, figyelemmel kíséri, és a naplózást programozott módon segítségével [Azure Diagnostics esemény beállításai és a Tulajdonságok](#diagnostic-event-properties).</span><span class="sxs-lookup"><span data-stu-id="01e99-110">You can also set up monitoring, tracking, and logging programmatically by using [Azure Diagnostics event settings and properties](#diagnostic-event-properties).</span></span>

## <a name="view-runs-and-trigger-history-for-your-logic-app"></a><span data-ttu-id="01e99-111">Nézet futtatása és a Logic Apps alkalmazást eseményindító előzményei</span><span class="sxs-lookup"><span data-stu-id="01e99-111">View runs and trigger history for your logic app</span></span>

1. <span data-ttu-id="01e99-112">toofind a Logic Apps alkalmazást a hello [Azure-portálon](https://portal.azure.com), a hello Azure főmenü, majd a **további szolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="01e99-112">toofind your logic app in hello [Azure portal](https://portal.azure.com), on hello main Azure menu, choose **More services**.</span></span> <span data-ttu-id="01e99-113">Hello a keresőmezőbe a "logic apps" található, és válassza a **a Logic apps**.</span><span class="sxs-lookup"><span data-stu-id="01e99-113">In hello search box, find "logic apps", and choose **Logic apps**.</span></span>

   ![A logikai alkalmazás keresése](./media/logic-apps-monitor-your-logic-apps/find-your-logic-app.png)

   <span data-ttu-id="01e99-115">hello Azure-portálon az Azure-előfizetéshez társított összes hello logikai alkalmazások jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="01e99-115">hello Azure portal shows all hello logic apps that are associated with your Azure subscription.</span></span> 

2. <span data-ttu-id="01e99-116">Válassza ki a Logic Apps alkalmazást, majd válassza a **áttekintése**.</span><span class="sxs-lookup"><span data-stu-id="01e99-116">Select your logic app, then choose **Overview**.</span></span>

   <span data-ttu-id="01e99-117">hello Azure-portálon hello futtatása előzmények és a Logic Apps alkalmazást eseményindító előzményeit jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="01e99-117">hello Azure portal shows hello runs history and trigger history for your logic app.</span></span> <span data-ttu-id="01e99-118">Példa:</span><span class="sxs-lookup"><span data-stu-id="01e99-118">For example:</span></span>

   ![Logikai alkalmazás fut előzmények és eseményindító előzményei](media/logic-apps-monitor-your-logic-apps/overview.png)

   * <span data-ttu-id="01e99-120">**Futtatja az előzmények** jeleníti meg a Logic Apps alkalmazást minden hello tartoznak futtatások.</span><span class="sxs-lookup"><span data-stu-id="01e99-120">**Runs history** shows all hello runs for your logic app.</span></span> 
   * <span data-ttu-id="01e99-121">**Indítás, előzmények** a Logic Apps alkalmazást minden hello eseményindító tevékenység műveleteit jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="01e99-121">**Trigger History** shows all hello trigger activity for your logic app.</span></span>

   <span data-ttu-id="01e99-122">A leírások, lásd: [hibaelhárítása a Logic Apps alkalmazást](../logic-apps/logic-apps-diagnosing-failures.md).</span><span class="sxs-lookup"><span data-stu-id="01e99-122">For status descriptions, see [Troubleshoot your logic app](../logic-apps/logic-apps-diagnosing-failures.md).</span></span>

   > [!TIP]
   > <span data-ttu-id="01e99-123">Ha nem találja a hello adatokat várt, hello eszköztáron válassza a **frissítése**.</span><span class="sxs-lookup"><span data-stu-id="01e99-123">If you don't find hello data that you expect, on hello toolbar, choose **Refresh**.</span></span>

3. <span data-ttu-id="01e99-124">egy adott futtatható, a lépések tooview hello **előzmények fut**, válassza ki a futtató.</span><span class="sxs-lookup"><span data-stu-id="01e99-124">tooview hello steps from a specific run, under **Runs history**, select that run.</span></span> 

   <span data-ttu-id="01e99-125">hello figyelő nézet futtató egyes lépéseit mutatja.</span><span class="sxs-lookup"><span data-stu-id="01e99-125">hello monitor view shows each step in that run.</span></span> <span data-ttu-id="01e99-126">Példa:</span><span class="sxs-lookup"><span data-stu-id="01e99-126">For example:</span></span>

   ![A megadott futtató műveletek](media/logic-apps-monitor-your-logic-apps/monitor-view-updated.png)

4. <span data-ttu-id="01e99-128">tooget részletesebben hello futtatja, válasszon **futtatása részletek**.</span><span class="sxs-lookup"><span data-stu-id="01e99-128">tooget more details about hello run, choose **Run Details**.</span></span> <span data-ttu-id="01e99-129">Ezek az információk összesíti hello lépéseket, állapot, bemeneti adatokat, és kimeneti hello futtassa.</span><span class="sxs-lookup"><span data-stu-id="01e99-129">This information summarizes hello steps, status, inputs, and outputs for hello run.</span></span> 

   ![Válassza a "Futtatás részletei"](media/logic-apps-monitor-your-logic-apps/run-details.png)

   <span data-ttu-id="01e99-131">Például, hogy megkaphassa hello futtassa a **korrelációs azonosító**, amely szükség lehet a hello használatakor [REST API-t a Logic Apps](https://docs.microsoft.com/rest/api/logic).</span><span class="sxs-lookup"><span data-stu-id="01e99-131">For example, you can get hello run's **Correlation ID**, which you might need when you use hello [REST API for Logic Apps](https://docs.microsoft.com/rest/api/logic).</span></span>

5. <span data-ttu-id="01e99-132">egy adott lépésre tooget adatait válassza ki a lépés.</span><span class="sxs-lookup"><span data-stu-id="01e99-132">tooget details about a specific step, choose that step.</span></span> <span data-ttu-id="01e99-133">Most adatait, például bemenetek, kimenetek és hibáit, és ismételje meg a lépés a tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="01e99-133">You can now review details like inputs, outputs, and any errors that happened for that step.</span></span> <span data-ttu-id="01e99-134">Példa:</span><span class="sxs-lookup"><span data-stu-id="01e99-134">For example:</span></span>

   ![Lépés részletei](media/logic-apps-monitor-your-logic-apps/monitor-view-details.png)
   
   > [!NOTE]
   > <span data-ttu-id="01e99-136">Az összes futásidejű adatokat és eseményeket hello Logic Apps szolgáltatás belül vannak titkosítva.</span><span class="sxs-lookup"><span data-stu-id="01e99-136">All runtime details and events are encrypted within hello Logic Apps service.</span></span> <span data-ttu-id="01e99-137">Akkor lesznek visszafejtve, csak akkor, ha egy felhasználó kéri tooview adatokat.</span><span class="sxs-lookup"><span data-stu-id="01e99-137">They are decrypted only when a user requests tooview that data.</span></span> <span data-ttu-id="01e99-138">Azt is meghatározhatja, hozzáférési toothese eseményeket [átruházásához hozzáférés-vezérlés (RBAC)](../active-directory/role-based-access-control-what-is.md).</span><span class="sxs-lookup"><span data-stu-id="01e99-138">You can also control access toothese events with [Azure Role-Based Access Control (RBAC)](../active-directory/role-based-access-control-what-is.md).</span></span>

6. <span data-ttu-id="01e99-139">egy adott eseményindító esemény tooget adatait vissza toohello **áttekintése** ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="01e99-139">tooget details about a specific trigger event, go back toohello **Overview** pane.</span></span> <span data-ttu-id="01e99-140">A **előzmények indítás**, jelölje be hello indítási esemény.</span><span class="sxs-lookup"><span data-stu-id="01e99-140">Under **Trigger history**, select hello trigger event.</span></span> <span data-ttu-id="01e99-141">Most már megtekintheti adatait, például bemenetekhez és kimenetekhez, például:</span><span class="sxs-lookup"><span data-stu-id="01e99-141">You can now review details like inputs and outputs, for example:</span></span>

   ![Indítási esemény kimeneti részletei](media/logic-apps-monitor-your-logic-apps/trigger-details.png)

<a name="azure-diagnostics"></a>

## <a name="turn-on-diagnostics-logging-for-your-logic-app"></a><span data-ttu-id="01e99-143">Diagnosztika a logikai alkalmazás naplózásának engedélyezése</span><span class="sxs-lookup"><span data-stu-id="01e99-143">Turn on diagnostics logging for your logic app</span></span>

<span data-ttu-id="01e99-144">Gazdagabb feltárására futásidejű adatokat és eseményeket, állíthatja be a diagnosztikai naplózás [Azure Naplóelemzés](../log-analytics/log-analytics-overview.md).</span><span class="sxs-lookup"><span data-stu-id="01e99-144">For richer debugging with runtime details and events, you can set up diagnostics logging with [Azure Log Analytics](../log-analytics/log-analytics-overview.md).</span></span> <span data-ttu-id="01e99-145">A Naplóelemzési rendszer szolgáltatása [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md) , amely figyeli a felhőben és a helyszíni környezetben toohelp azok rendelkezésre állását és teljesítményét karbantartása.</span><span class="sxs-lookup"><span data-stu-id="01e99-145">Log Analytics is a service in [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md) that monitors your cloud and on-premises environments toohelp you maintain their availability and performance.</span></span> 

<span data-ttu-id="01e99-146">Mielőtt elkezdené, toohave OMS-munkaterület szüksége.</span><span class="sxs-lookup"><span data-stu-id="01e99-146">Before you start, you need toohave an OMS workspace.</span></span> <span data-ttu-id="01e99-147">Ismerje meg, [hogyan toocreate OMS-munkaterület](../log-analytics/log-analytics-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="01e99-147">Learn [how toocreate an OMS workspace](../log-analytics/log-analytics-get-started.md).</span></span>

1. <span data-ttu-id="01e99-148">A hello [Azure-portálon](https://portal.azure.com), keresse meg és jelölje meg a Logic Apps alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="01e99-148">In hello [Azure portal](https://portal.azure.com), find and select your logic app.</span></span> 

2. <span data-ttu-id="01e99-149">Hello logic app panel menü alatti **figyelés**, válassza ki **diagnosztika** > **diagnosztikai beállítások**.</span><span class="sxs-lookup"><span data-stu-id="01e99-149">On hello logic app blade menu, under **Monitoring**, choose **Diagnostics** > **Diagnostic Settings**.</span></span>

   ![Nyissa meg tooMonitoring, diagnosztikát, diagnosztikai beállítások](media/logic-apps-monitor-your-logic-apps/logic-app-diagnostics.png)

3. <span data-ttu-id="01e99-151">A **diagnosztikai beállítások**, válassza a **a**.</span><span class="sxs-lookup"><span data-stu-id="01e99-151">Under **Diagnostics settings**, choose **On**.</span></span>

   ![Kapcsolja be a diagnosztikai naplók](media/logic-apps-monitor-your-logic-apps/turn-on-diagnostics-logic-app.png)

4. <span data-ttu-id="01e99-153">Most válasszon hello OMS munkaterületet, és az esemény naplózási kategóriát látható módon:</span><span class="sxs-lookup"><span data-stu-id="01e99-153">Now select hello OMS workspace and event category for logging as shown:</span></span>

   1. <span data-ttu-id="01e99-154">Válassza ki **tooLog Analytics küldése**.</span><span class="sxs-lookup"><span data-stu-id="01e99-154">Select **Send tooLog Analytics**.</span></span> 
   2. <span data-ttu-id="01e99-155">A **Naplóelemzési**, válassza a **konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="01e99-155">Under **Log Analytics**, choose **Configure**.</span></span> 
   3. <span data-ttu-id="01e99-156">A **OMS-munkaterület**, jelölje be az OMS-munkaterület toouse hello a naplózás.</span><span class="sxs-lookup"><span data-stu-id="01e99-156">Under **OMS Workspaces**, select hello OMS workspace toouse for logging.</span></span>
   4. <span data-ttu-id="01e99-157">A **napló**, jelölje be hello **WorkflowRuntime** kategóriát.</span><span class="sxs-lookup"><span data-stu-id="01e99-157">Under **Log**, select hello **WorkflowRuntime** category.</span></span>
   5. <span data-ttu-id="01e99-158">Válassza ki a hello metrika időköz.</span><span class="sxs-lookup"><span data-stu-id="01e99-158">Choose hello metric interval.</span></span>
   6. <span data-ttu-id="01e99-159">Amikor elkészült, válassza ki a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="01e99-159">When you're done, choose **Save**.</span></span>

   ![Válassza ki az OMS-munkaterület és a naplózási adatokat](media/logic-apps-monitor-your-logic-apps/send-diagnostics-data-log-analytics-workspace.png)

<span data-ttu-id="01e99-161">Most található események és egyéb adatok eseményindító események, eseményeket és a műveleti események futtatják.</span><span class="sxs-lookup"><span data-stu-id="01e99-161">Now, you can find events and other data for trigger events, run events, and action events.</span></span>

<a name="find-events"></a>

## <a name="find-events-and-data-for-your-logic-app"></a><span data-ttu-id="01e99-162">A Logic Apps alkalmazást talált események és adatok</span><span class="sxs-lookup"><span data-stu-id="01e99-162">Find events and data for your logic app</span></span>

<span data-ttu-id="01e99-163">a Logic Apps alkalmazást, eseményindító, futtassa az eseményeket, és műveleti események, például toofind és nézet események kövesse az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="01e99-163">toofind and view events in your logic app, like trigger events, run events, and action events, follow these steps.</span></span>

1. <span data-ttu-id="01e99-164">A hello [Azure-portálon](https://portal.azure.com), válassza a **több szolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="01e99-164">In hello [Azure portal](https://portal.azure.com), choose **More Services**.</span></span> <span data-ttu-id="01e99-165">Keresse meg a "naplóelemzési", és válassza a **Naplóelemzési** itt látható módon:</span><span class="sxs-lookup"><span data-stu-id="01e99-165">Search for "log analytics", then choose **Log Analytics** as shown here:</span></span>

   ![Válassza ki a "Naplóelemzési"](media/logic-apps-monitor-your-logic-apps/browseloganalytics.png)

2. <span data-ttu-id="01e99-167">A **Naplóelemzési**, található, és válassza ki az OMS-munkaterület.</span><span class="sxs-lookup"><span data-stu-id="01e99-167">Under **Log Analytics**, find and select your OMS workspace.</span></span> 

   ![Az OMS-munkaterület kiválasztása](media/logic-apps-monitor-your-logic-apps/selectla.png)

3. <span data-ttu-id="01e99-169">A **felügyeleti**, válassza a **OMS-portálon**.</span><span class="sxs-lookup"><span data-stu-id="01e99-169">Under **Management**, choose **OMS Portal**.</span></span>

   ![Válassza ki a "OMS-portálon"](media/logic-apps-monitor-your-logic-apps/omsportalpage.png)

4. <span data-ttu-id="01e99-171">Az OMS kezdőlapján válassza **naplófájl-keresési**.</span><span class="sxs-lookup"><span data-stu-id="01e99-171">On your OMS home page, choose **Log Search**.</span></span>

   ![Az OMS kezdőlapján válassza a "Naplófájl-keresési"](media/logic-apps-monitor-your-logic-apps/logsearch.png)

   <span data-ttu-id="01e99-173">– vagy –</span><span class="sxs-lookup"><span data-stu-id="01e99-173">-or-</span></span>

   ![A hello OMS menüben válassza a "Naplófájl-keresési"](media/logic-apps-monitor-your-logic-apps/logsearch-2.png)

5. <span data-ttu-id="01e99-175">Hello keresési mezőbe, adjon meg egy mező, amelyet az toofind, és nyomja le az ENTER **Enter**.</span><span class="sxs-lookup"><span data-stu-id="01e99-175">In hello search box, specify a field that you want toofind, and press **Enter**.</span></span> <span data-ttu-id="01e99-176">Amikor elkezdi beírni, OMS megjeleníti a lehetséges találatok és műveletek közül választhat.</span><span class="sxs-lookup"><span data-stu-id="01e99-176">When you start typing, OMS shows you possible matches and operations that you can use.</span></span> 

   <span data-ttu-id="01e99-177">Például toofind hello első 10 eseményeket, amelyek történtek, adja meg, és válassza ki a keresési lekérdezés: **kategória = WorkflowRuntime |} felső 10**</span><span class="sxs-lookup"><span data-stu-id="01e99-177">For example, toofind hello top 10 events that happened, enter and select this search query: **Category=WorkflowRuntime |top 10**</span></span>

   ![Adja meg a keresési karakterláncot](media/logic-apps-monitor-your-logic-apps/oms-start-query.png)

   <span data-ttu-id="01e99-179">További információ [hogyan Naplóelemzési toofind adatok](../log-analytics/log-analytics-log-searches.md).</span><span class="sxs-lookup"><span data-stu-id="01e99-179">Learn more about [how toofind data in Log Analytics](../log-analytics/log-analytics-log-searches.md).</span></span>

6. <span data-ttu-id="01e99-180">A hello eredmények lapon hello bal oldali sávon, válassza a hello időkeretet, amelyet az tooview.</span><span class="sxs-lookup"><span data-stu-id="01e99-180">On hello results page, in hello left bar, choose hello timeframe that you want tooview.</span></span>
<span data-ttu-id="01e99-181">toorefine adja hozzá egy szűrőt, a lekérdezés válasszon **+ Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="01e99-181">toorefine your query by adding a filter, choose **+Add**.</span></span>

   ![Válassza ki a lekérdezési eredmények időkeretre](media/logic-apps-monitor-your-logic-apps/query-results.png)

7. <span data-ttu-id="01e99-183">A **szűrők hozzáadása**, adja meg a hello szűrő nevét, így kívánt hello szűrő található.</span><span class="sxs-lookup"><span data-stu-id="01e99-183">Under **Add Filters**, enter hello filter name so you can find hello filter you want.</span></span> <span data-ttu-id="01e99-184">Válassza ki a hello szűrőt, és válassza a **+ Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="01e99-184">Select hello filter, and choose **+Add**.</span></span>

   <span data-ttu-id="01e99-185">Ebben a példában használt hello word "állapot" toofind sikertelen volt az események **AzureDiagnostics**.</span><span class="sxs-lookup"><span data-stu-id="01e99-185">This example uses hello word "status" toofind failed events under **AzureDiagnostics**.</span></span>
   <span data-ttu-id="01e99-186">Itt hello szűrőt **status_s** már be van jelölve.</span><span class="sxs-lookup"><span data-stu-id="01e99-186">Here hello filter for **status_s** is already selected.</span></span>

   ![Válassza ki a szűrő](media/logic-apps-monitor-your-logic-apps/log-search-add-filter.png)

8. <span data-ttu-id="01e99-188">Hello bal oldali sávon, adja meg a hello szűrő értéket, hogy szeretné, hogy toouse, és válassza **alkalmaz**.</span><span class="sxs-lookup"><span data-stu-id="01e99-188">In hello left bar, select hello filter value that you want toouse, and choose **Apply**.</span></span>

   ![Adja meg a szűrő értéket, válassza az "Alkalmaz"](media/logic-apps-monitor-your-logic-apps/log-search-apply-filter.png)

9. <span data-ttu-id="01e99-190">Térjen vissza van felépítése toohello lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="01e99-190">Now return toohello query that you're building.</span></span> <span data-ttu-id="01e99-191">A lekérdezés frissül a kijelölt szűrő és az értéke.</span><span class="sxs-lookup"><span data-stu-id="01e99-191">Your query is updated with your selected filter and value.</span></span> <span data-ttu-id="01e99-192">Az előző eredmények most túl szűrve.</span><span class="sxs-lookup"><span data-stu-id="01e99-192">Your previous results are now filtered too.</span></span>

   ![Térjen vissza a szűrt eredményekkel tooyour lekérdezés](media/logic-apps-monitor-your-logic-apps/log-search-query-filtered-results.png)

10. <span data-ttu-id="01e99-194">toosave későbbi használatra a lekérdezés válasszon **mentése**.</span><span class="sxs-lookup"><span data-stu-id="01e99-194">toosave your query for future use, choose **Save**.</span></span> <span data-ttu-id="01e99-195">Ismerje meg, [hogyan toosave a lekérdezés](../logic-apps/logic-apps-track-b2b-messages-omsportal-query-filter-control-number.md#save-oms-query).</span><span class="sxs-lookup"><span data-stu-id="01e99-195">Learn [how toosave your query](../logic-apps/logic-apps-track-b2b-messages-omsportal-query-filter-control-number.md#save-oms-query).</span></span>

<a name="extend-diagnostic-data"></a>

## <a name="extend-how-and-where-you-use-diagnostic-data-with-other-services"></a><span data-ttu-id="01e99-196">Hogyan és hol diagnosztikai adatok használja más szolgáltatásokkal</span><span class="sxs-lookup"><span data-stu-id="01e99-196">Extend how and where you use diagnostic data with other services</span></span>

<span data-ttu-id="01e99-197">Azure Naplóelemzés, valamint bővítheti, hogyan használhatja a Logic Apps alkalmazást diagnosztikai adatok más Azure-szolgáltatásokkal, például:</span><span class="sxs-lookup"><span data-stu-id="01e99-197">Along with Azure Log Analytics, you can extend how you use your logic app's diagnostic data with other Azure services, for example:</span></span> 

* [<span data-ttu-id="01e99-198">Az Azure diagnosztikai naplók az Azure Storage archív</span><span class="sxs-lookup"><span data-stu-id="01e99-198">Archive Azure Diagnostics Logs in Azure Storage</span></span>](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md)
* [<span data-ttu-id="01e99-199">Az adatfolyam Azure diagnosztikai naplók tooAzure Event Hubs</span><span class="sxs-lookup"><span data-stu-id="01e99-199">Stream Azure Diagnostics Logs tooAzure Event Hubs</span></span>](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md) 

<span data-ttu-id="01e99-200">Ezután a get valós idejű figyelés telemetriai adatok és más szolgáltatások analytics segítségével például [Azure Stream Analytics](../stream-analytics/stream-analytics-introduction.md) és [Power BI](../log-analytics/log-analytics-powerbi.md).</span><span class="sxs-lookup"><span data-stu-id="01e99-200">You can then get real-time monitoring by using telemetry and analytics from other services, like [Azure Stream Analytics](../stream-analytics/stream-analytics-introduction.md) and [Power BI](../log-analytics/log-analytics-powerbi.md).</span></span> <span data-ttu-id="01e99-201">Példa:</span><span class="sxs-lookup"><span data-stu-id="01e99-201">For example:</span></span>

* [<span data-ttu-id="01e99-202">Az Event Hubs tooStream Analytics adatfolyam adatait</span><span class="sxs-lookup"><span data-stu-id="01e99-202">Stream data from Event Hubs tooStream Analytics</span></span>](../stream-analytics/stream-analytics-define-inputs.md)
* [<span data-ttu-id="01e99-203">A Stream Analytics adatfolyam-továbbítási adatok elemzése és a Power bi-ban a valós idejű elemzési irányítópult létrehozása</span><span class="sxs-lookup"><span data-stu-id="01e99-203">Analyze streaming data with Stream Analytics and create a real-time analytics dashboard in Power BI</span></span>](../stream-analytics/stream-analytics-power-bi-dashboard.md)

<span data-ttu-id="01e99-204">Alapján hello beállítása kívánt beállításokat, győződjön meg arról, hogy Ön első [az Azure storage-fiók létrehozása](../storage/common/storage-create-storage-account.md) vagy [hozzon létre egy Azure event hubs](../event-hubs/event-hubs-create.md).</span><span class="sxs-lookup"><span data-stu-id="01e99-204">Based on hello options that you want set up, make sure that you first [create an Azure storage account](../storage/common/storage-create-storage-account.md) or [create an Azure event hub](../event-hubs/event-hubs-create.md).</span></span> <span data-ttu-id="01e99-205">Ezután válasszon hello beállítások ahová toosend diagnosztikai adatokat:</span><span class="sxs-lookup"><span data-stu-id="01e99-205">Then select hello options for where you want toosend diagnostic data:</span></span>

![TooAzure tárolási fiók vagy esemény hub adatok küldése](./media/logic-apps-monitor-your-logic-apps/storage-account-event-hubs.png)

> [!NOTE]
> <span data-ttu-id="01e99-207">Megőrzési időtartamú csak válasszon toouse tárfiókot vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="01e99-207">Retention periods apply only when you choose toouse a storage account.</span></span>

<a name="add-azure-alerts"></a>

## <a name="set-up-alerts-for-your-logic-app"></a><span data-ttu-id="01e99-208">A logikai alkalmazás beállítása</span><span class="sxs-lookup"><span data-stu-id="01e99-208">Set up alerts for your logic app</span></span>

<span data-ttu-id="01e99-209">adott mérőszámok toomonitor vagy a Logic Apps alkalmazást küszöbértékek túllépése beállítása [értesítések az Azure-ban](../monitoring-and-diagnostics/monitoring-overview-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="01e99-209">toomonitor specific metrics or exceeded thresholds for your logic app, set up [alerts in Azure](../monitoring-and-diagnostics/monitoring-overview-alerts.md).</span></span> <span data-ttu-id="01e99-210">További tudnivalók [az Azure-ban mérőszámok](../monitoring-and-diagnostics/monitoring-overview-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="01e99-210">Learn about [metrics in Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md).</span></span> 

<span data-ttu-id="01e99-211">figyelmeztetéseket nélkül tooset [Azure Naplóelemzés](../log-analytics/log-analytics-overview.md), kövesse az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="01e99-211">tooset up alerts without [Azure Log Analytics](../log-analytics/log-analytics-overview.md), follow these steps.</span></span> <span data-ttu-id="01e99-212">A riasztások feltételek és a műveletek, speciális [Naplóelemzési beállítása](#azure-diagnostics) túl.</span><span class="sxs-lookup"><span data-stu-id="01e99-212">For more advanced alerts criteria and actions, [set up Log Analytics](#azure-diagnostics) too.</span></span>

1. <span data-ttu-id="01e99-213">Hello logic app panel menü alatti **figyelés**, válassza ki **diagnosztika** > **riasztási szabályok** > **riasztáshozzáadása**itt látható módon:</span><span class="sxs-lookup"><span data-stu-id="01e99-213">On hello logic app blade menu, under **Monitoring**, choose **Diagnostics** > **Alert rules** > **Add alert** as shown here:</span></span>

   ![A Logic Apps alkalmazást értesítések hozzáadása](media/logic-apps-monitor-your-logic-apps/set-up-alerts.png)

2. <span data-ttu-id="01e99-215">A hello **riasztási szabály felvétele** panelen, a riasztás létrehozása, látható módon:</span><span class="sxs-lookup"><span data-stu-id="01e99-215">On hello **Add an alert rule** blade, create your alert as shown:</span></span>

   1. <span data-ttu-id="01e99-216">A **erőforrás**, válassza ki a logikai alkalmazás, ha nincsenek használatban.</span><span class="sxs-lookup"><span data-stu-id="01e99-216">Under **Resource**, select your logic app, if not already selected.</span></span> 
   2. <span data-ttu-id="01e99-217">Adjon meg egy nevének és leírásának megadása a riasztás.</span><span class="sxs-lookup"><span data-stu-id="01e99-217">Give a name and description for your alert.</span></span>
   3. <span data-ttu-id="01e99-218">Válassza ki a **metrika** vagy tootrack kívánt eseményt.</span><span class="sxs-lookup"><span data-stu-id="01e99-218">Select a **Metric** or event that you want tootrack.</span></span>
   4. <span data-ttu-id="01e99-219">Válassza ki a **feltétel**, adja meg egy **küszöbérték** hello metrika, és jelölje be hello **időszak** Ez a metrika figyelésre.</span><span class="sxs-lookup"><span data-stu-id="01e99-219">Select a **Condition**, specify a **Threshold** for hello metric, and select hello **Period** for monitoring this metric.</span></span>
   5. <span data-ttu-id="01e99-220">Adja meg, hogy toosend levelezési hello riasztás.</span><span class="sxs-lookup"><span data-stu-id="01e99-220">Select whether toosend mail for hello alert.</span></span> 
   6. <span data-ttu-id="01e99-221">Adjon meg más e-mail címek hello értesítés küldéséhez.</span><span class="sxs-lookup"><span data-stu-id="01e99-221">Specify any other email addresses for sending hello alert.</span></span> 
   <span data-ttu-id="01e99-222">A webhook URL-CÍMÉT, ha azt szeretné, hogy toosend hello riasztás is megadható.</span><span class="sxs-lookup"><span data-stu-id="01e99-222">You can also specify a webhook URL where you want toosend hello alert.</span></span>

   <span data-ttu-id="01e99-223">Például ez a szabály küld riasztást, amikor öt vagy több futtatása sikertelen lesz, egy óra alatt:</span><span class="sxs-lookup"><span data-stu-id="01e99-223">For example, this rule sends an alert when five or more runs fail in an hour:</span></span>

   ![Metrika riasztási szabály létrehozása](media/logic-apps-monitor-your-logic-apps/create-alert-rule.png)

> [!TIP]
> <span data-ttu-id="01e99-225">logikai alkalmazás toorun riasztásból, megadhat hello [kérelem eseményindító](../connectors/connectors-native-reqres.md) a munkafolyamat, amellyel feladatokhoz a példák mint:</span><span class="sxs-lookup"><span data-stu-id="01e99-225">toorun a logic app from an alert, you can include hello [request trigger](../connectors/connectors-native-reqres.md) in your workflow, which lets you perform tasks like these examples:</span></span>
> 
> * [<span data-ttu-id="01e99-226">POST tooSlack</span><span class="sxs-lookup"><span data-stu-id="01e99-226">Post tooSlack</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app)
> * [<span data-ttu-id="01e99-227">A szöveg küldése</span><span class="sxs-lookup"><span data-stu-id="01e99-227">Send a text</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app)
> * [<span data-ttu-id="01e99-228">Egy üzenetsor tooa hozzáadása</span><span class="sxs-lookup"><span data-stu-id="01e99-228">Add a message tooa queue</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app)

<a name="diagnostic-event-properties"></a>

## <a name="azure-diagnostics-event-settings-and-details"></a><span data-ttu-id="01e99-229">Az Azure Diagnostics esemény beállításai és részletek</span><span class="sxs-lookup"><span data-stu-id="01e99-229">Azure Diagnostics event settings and details</span></span>

<span data-ttu-id="01e99-230">Minden egyes diagnosztikai esemény részleteit a Logic Apps alkalmazást és az esemény, például hello állapotát, a start idő, befejezési idő, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="01e99-230">Each diagnostic event has details about your logic app and that event, for example, hello status, start time, end time, and so on.</span></span> <span data-ttu-id="01e99-231">figyelés, a nyomon követési és a naplózás beállítása tooprogrammatically, szervezeti egység használható ezen adatok hello [REST API-t az Azure Logic Apps](https://docs.microsoft.com/rest/api/logic) és hello [REST API-t az Azure Diagnostics](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftlogicworkflows).</span><span class="sxs-lookup"><span data-stu-id="01e99-231">tooprogrammatically set up monitoring, tracking, and logging, ou can use these details with hello [REST API for Azure Logic Apps](https://docs.microsoft.com/rest/api/logic) and hello [REST API for Azure Diagnostics](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftlogicworkflows).</span></span>

<span data-ttu-id="01e99-232">Például hello `ActionCompleted` eseményhez tartozik hello `clientTrackingId` és `trackedProperties` nyomon követése és figyelésére használható tulajdonságokról:</span><span class="sxs-lookup"><span data-stu-id="01e99-232">For example, hello `ActionCompleted` event has hello `clientTrackingId` and `trackedProperties` properties that you can use for tracking and monitoring:</span></span>

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

* <span data-ttu-id="01e99-233">`clientTrackingId`: Ha nincs megadott Azure automatikusan létrehozza ezt az Azonosítót, és korrelálja események futtatása logikai alkalmazás között, beleértve a beágyazott munkafolyamatok, amelyek nevezzük hello logic App.</span><span class="sxs-lookup"><span data-stu-id="01e99-233">`clientTrackingId`: If not provided, Azure automatically generates this ID and correlates events across a logic app run, including any nested workflows that are called from hello logic app.</span></span> <span data-ttu-id="01e99-234">Ez az azonosító egy úgy, hogy manuálisan megadhatja egy `x-ms-client-tracking-id` fejléc a következő hello eseményindító kérelem az egyéni azonosító értéke.</span><span class="sxs-lookup"><span data-stu-id="01e99-234">You can manually specify this ID from a trigger by passing a `x-ms-client-tracking-id` header with your custom ID value in hello trigger request.</span></span> <span data-ttu-id="01e99-235">A kérelem eseményindító, a HTTP-eseményindítóval, illetve a webhook eseményindító is használhatja.</span><span class="sxs-lookup"><span data-stu-id="01e99-235">You can use a request trigger, HTTP trigger, or webhook trigger.</span></span>

* <span data-ttu-id="01e99-236">`trackedProperties`: tootrack bemeneti vagy kimeneti diagnosztikai adatok, adhat hozzá a nyomon követett tulajdonságok tooactions a Logic Apps alkalmazást JSON-definícióból.</span><span class="sxs-lookup"><span data-stu-id="01e99-236">`trackedProperties`: tootrack inputs or outputs in diagnostics data, you can add tracked properties tooactions in your logic app's JSON definition.</span></span> <span data-ttu-id="01e99-237">Csak egyetlen művelettel bemenetekhez és kimenetekhez nyomon követheti nyomon követett tulajdonságok, de használhat hello `correlation` események toocorrelate futtató műveletek között tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="01e99-237">Tracked properties can track only a single action's inputs and outputs, but you can use hello `correlation` properties of events toocorrelate across actions in a run.</span></span>

  <span data-ttu-id="01e99-238">tootrack egy vagy több tulajdonságának hozzáadása hello `trackedProperties` toohello művelet definition kívánt szakasz és hello tulajdonságokat.</span><span class="sxs-lookup"><span data-stu-id="01e99-238">tootrack one or more properties, add hello `trackedProperties` section and hello properties you want toohello action definition.</span></span> <span data-ttu-id="01e99-239">Tegyük fel, hogy a telemetriai adatok tootrack adatok, például egy "order ID" kívánt:</span><span class="sxs-lookup"><span data-stu-id="01e99-239">For example, suppose you want tootrack data like an "order ID" in your telemetry:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="01e99-240">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="01e99-240">Next steps</span></span>

* [<span data-ttu-id="01e99-241">A logic app központi telepítési sablonok létrehozása és a kiadáskezelés</span><span class="sxs-lookup"><span data-stu-id="01e99-241">Create templates for logic app deployment and release management</span></span>](../logic-apps/logic-apps-create-deploy-template.md)
* [<span data-ttu-id="01e99-242">A vállalati integrációs csomag B2B forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="01e99-242">B2B scenarios with Enterprise Integration Pack</span></span>](../logic-apps/logic-apps-enterprise-integration-overview.md)
* [<span data-ttu-id="01e99-243">B2B üzenetek megfigyelése</span><span class="sxs-lookup"><span data-stu-id="01e99-243">Monitor B2B messages</span></span>](../logic-apps/logic-apps-monitor-b2b-message.md)