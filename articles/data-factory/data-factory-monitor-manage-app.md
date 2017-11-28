---
title: "Megfigyelés és kezelés adatok folyamatok - Azure |} Microsoft Docs"
description: "Útmutató: a figyelés és a felügyeleti alkalmazás segítségével Azure adat-előállítók és a folyamatok felügyeletét és kezelését."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: f3f07bc4-6dc3-4d4d-ac22-0be62189d578
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: spelluru
ms.openlocfilehash: d5a2d1f3d85b8a2212326cfcfd0ba5d80356b769
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="monitor-and-manage-azure-data-factory-pipelines-by-using-the-monitoring-and-management-app"></a><span data-ttu-id="a031c-103">Figyelheti és kezelheti az Azure Data Factory adatcsatornák a figyelés és felügyelet alkalmazással</span><span class="sxs-lookup"><span data-stu-id="a031c-103">Monitor and manage Azure Data Factory pipelines by using the Monitoring and Management app</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="a031c-104">Az Azure portálon vagy az Azure PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="a031c-104">Using Azure portal/Azure PowerShell</span></span>](data-factory-monitor-manage-pipelines.md)
> * [<span data-ttu-id="a031c-105">Használatával figyelése és a felügyeleti alkalmazás</span><span class="sxs-lookup"><span data-stu-id="a031c-105">Using Monitoring and Management app</span></span>](data-factory-monitor-manage-app.md)
>
>

<span data-ttu-id="a031c-106">A cikkből megtudhatja, hogyan használható a figyelés és a felügyeleti alkalmazás figyeléséhez, kezeléséhez és az adat-előállító adatcsatornák debug.</span><span class="sxs-lookup"><span data-stu-id="a031c-106">This article describes how to use the Monitoring and Management app to monitor, manage, and debug your Data Factory pipelines.</span></span> <span data-ttu-id="a031c-107">Tájékoztatást kaphat az hibáiról riasztásokat létrehozásával kapcsolatos információkat is biztosít.</span><span class="sxs-lookup"><span data-stu-id="a031c-107">It also provides information on how to create alerts to get notified on failures.</span></span> <span data-ttu-id="a031c-108">Ismerkedés az alkalmazás által a következő videolejátszás használatával:</span><span class="sxs-lookup"><span data-stu-id="a031c-108">You can get started with using the application by watching the following video:</span></span>

> [!NOTE]
> <span data-ttu-id="a031c-109">A felhasználói felületen látható módon a videó előfordulhat, hogy nem egyeznek pontosan kapcsolatban a portálon.</span><span class="sxs-lookup"><span data-stu-id="a031c-109">The user interface shown in the video may not exactly match what you see in the portal.</span></span> <span data-ttu-id="a031c-110">Némileg régebbi, de fogalmak változatlan marad.</span><span class="sxs-lookup"><span data-stu-id="a031c-110">It's slightly older, but concepts remain the same.</span></span> 

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Azure-Data-Factory-Monitoring-and-Managing-Big-Data-Piplines/player]
>

## <a name="launch-the-monitoring-and-management-app"></a><span data-ttu-id="a031c-111">Indítsa el a figyelés és a felügyeleti alkalmazás</span><span class="sxs-lookup"><span data-stu-id="a031c-111">Launch the Monitoring and Management app</span></span>
<span data-ttu-id="a031c-112">A figyelő és a felügyeleti alkalmazás indításához kattintson a **figyelő & kezelése** csempét a **adat-előállító** a data factory paneljén.</span><span class="sxs-lookup"><span data-stu-id="a031c-112">To launch the Monitor and Management app, click the **Monitor & Manage** tile on the **Data Factory** blade for your data factory.</span></span>

![A Data Factory kezdőlapon figyelési csempe](./media/data-factory-monitor-manage-app/MonitoringAppTile.png)

<span data-ttu-id="a031c-114">Meg kell jelennie egy külön ablakban nyissa meg a figyelés és a felügyeleti alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="a031c-114">You should see the Monitoring and Management app open in a separate window.</span></span>  

![Figyelési és felügyeleti alkalmazás](./media/data-factory-monitor-manage-app/AppLaunched.png)

> [!NOTE]
> <span data-ttu-id="a031c-116">Ha azt látja, hogy a webböngésző akadt-e a "Engedélyező...", törölje a jelet a **külső cookie-k blokkolását, és a helyadatok** jelölőnégyzet – vagy a tárolás során is garantálják az kiválasztva, hozzon létre egy kivételt **login.microsoftonline.com**, és próbálja meg újra megnyitni az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="a031c-116">If you see that the web browser is stuck at "Authorizing...", clear the **Block third-party cookies and site data** check box--or keep it selected, create an exception for **login.microsoftonline.com**, and then try to open the app again.</span></span>


<span data-ttu-id="a031c-117">A középső ablaktáblán tevékenység Windows listájában látni egy tevékenység ablakban tevékenység minden egyes futtatásához.</span><span class="sxs-lookup"><span data-stu-id="a031c-117">In the Activity Windows list in the middle pane, you see an activity window for each run of an activity.</span></span> <span data-ttu-id="a031c-118">Például ha a tevékenység öt órán keresztül óránkénti futásra ütemezett, látni öt tevékenység windows társított öt adatszeletek.</span><span class="sxs-lookup"><span data-stu-id="a031c-118">For example, if you have the activity scheduled to run hourly for five hours, you see five activity windows associated with five data slices.</span></span> <span data-ttu-id="a031c-119">Ha nem látja a lista alján tevékenységablakok, tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="a031c-119">If you don't see activity windows in the list at the bottom, do the following:</span></span>
 
- <span data-ttu-id="a031c-120">Frissítés a **kezdési időpont** és **befejező időpontja** szűrők felel meg a kezdési és befejezési időpontjai között a folyamat, és kattintson a lap tetején a **alkalmaz** gombra.</span><span class="sxs-lookup"><span data-stu-id="a031c-120">Update the **start time** and **end time** filters at the top to match the start and end times of your pipeline, and then click the **Apply** button.</span></span>  
- <span data-ttu-id="a031c-121">A tevékenység Windows lista nem frissül automatikusan.</span><span class="sxs-lookup"><span data-stu-id="a031c-121">The Activity Windows list is not automatically refreshed.</span></span> <span data-ttu-id="a031c-122">Kattintson a **frissítése** gombra az eszköztáron a **tevékenység Windows** listája.</span><span class="sxs-lookup"><span data-stu-id="a031c-122">Click the **Refresh** button on the toolbar in the **Activity Windows** list.</span></span>  

<span data-ttu-id="a031c-123">Ha még nem rendelkezik a Data Factory alkalmazás teszteléséhez ezeket a lépéseket, tegye az oktatóanyag: [adatok másolása az Blob-tároló az SQL-adatbázis használata a Data Factory](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="a031c-123">If you don't have a Data Factory application to test these steps with, do the tutorial: [copy data from Blob Storage to SQL Database using Data Factory](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>

## <a name="understand-the-monitoring-and-management-app"></a><span data-ttu-id="a031c-124">A megfigyelés és a felügyeleti alkalmazás</span><span class="sxs-lookup"><span data-stu-id="a031c-124">Understand the Monitoring and Management app</span></span>
<span data-ttu-id="a031c-125">A bal oldali vannak a három lappal: **erőforrás-kezelő**, **figyelési nézetei**, és **riasztások**.</span><span class="sxs-lookup"><span data-stu-id="a031c-125">There are three tabs on the left: **Resource Explorer**, **Monitoring Views**, and **Alerts**.</span></span> <span data-ttu-id="a031c-126">Az első lap (**erőforrás-kezelő**) alapértelmezés szerint engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="a031c-126">The first tab (**Resource Explorer**) is selected by default.</span></span>

### <a name="resource-explorer"></a><span data-ttu-id="a031c-127">Erőforrás-kezelő</span><span class="sxs-lookup"><span data-stu-id="a031c-127">Resource Explorer</span></span>
<span data-ttu-id="a031c-128">Tekintse át a következőket:</span><span class="sxs-lookup"><span data-stu-id="a031c-128">You see the following:</span></span>

* <span data-ttu-id="a031c-129">Az erőforrás-kezelő **fanézetben** a bal oldali ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="a031c-129">The Resource Explorer **tree view** in the left pane.</span></span>
* <span data-ttu-id="a031c-130">A **diagramnézet** a középső ablaktáblán a lap tetején.</span><span class="sxs-lookup"><span data-stu-id="a031c-130">The **Diagram View** at the top in the middle pane.</span></span>
* <span data-ttu-id="a031c-131">A **tevékenység Windows** lista alján a középső ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="a031c-131">The **Activity Windows** list at the bottom in the middle pane.</span></span>
* <span data-ttu-id="a031c-132">A **tulajdonságok**, **tevékenység ablak Explorer**, és **parancsfájl** lapokon a jobb oldali ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="a031c-132">The **Properties**, **Activity Window Explorer**, and **Script** tabs in the right pane.</span></span>

<span data-ttu-id="a031c-133">Az erőforrás-kezelőben lásd: összes erőforrást (adatcsatornák, adatkészleteket, összekapcsolt szolgáltatások) fanézetben adat-előállító.</span><span class="sxs-lookup"><span data-stu-id="a031c-133">In Resource Explorer, you see all resources (pipelines, datasets, linked services) in the data factory in a tree view.</span></span> <span data-ttu-id="a031c-134">Amikor kijelöl egy objektumot az erőforrás-kezelőben:</span><span class="sxs-lookup"><span data-stu-id="a031c-134">When you select an object in Resource Explorer:</span></span>

* <span data-ttu-id="a031c-135">A társított adat-előállító entitás ki van jelölve, a Diagram nézetben.</span><span class="sxs-lookup"><span data-stu-id="a031c-135">The associated Data Factory entity is highlighted in the Diagram View.</span></span>
* <span data-ttu-id="a031c-136">[Hozzárendelt tevékenység windows](data-factory-scheduling-and-execution.md) emel ki a tevékenységet Windows lista alján.</span><span class="sxs-lookup"><span data-stu-id="a031c-136">[Associated activity windows](data-factory-scheduling-and-execution.md) are highlighted in the Activity Windows list at the bottom.</span></span>  
* <span data-ttu-id="a031c-137">A kiválasztott objektum tulajdonságait a Tulajdonságok ablak jobb oldali ablaktáblán jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="a031c-137">The properties of the selected object are shown in the Properties window in the right pane.</span></span>
* <span data-ttu-id="a031c-138">A kijelölt objektum JSON-definícióból jelenik meg, ha van ilyen.</span><span class="sxs-lookup"><span data-stu-id="a031c-138">The JSON definition of the selected object is shown, if applicable.</span></span> <span data-ttu-id="a031c-139">Például: a társított szolgáltatás, a DataSet adatkészlet vagy folyamat.</span><span class="sxs-lookup"><span data-stu-id="a031c-139">For example: a linked service, a dataset, or a pipeline.</span></span>

![Erőforrás-kezelő](./media/data-factory-monitor-manage-app/ResourceExplorer.png)

<span data-ttu-id="a031c-141">Tekintse meg a [ütemezés és a végrehajtás](data-factory-scheduling-and-execution.md) cikk tevékenység windows kapcsolatos további részletes információt.</span><span class="sxs-lookup"><span data-stu-id="a031c-141">See the [Scheduling and Execution](data-factory-scheduling-and-execution.md) article for detailed conceptual information about activity windows.</span></span>

### <a name="diagram-view"></a><span data-ttu-id="a031c-142">Diagramnézet</span><span class="sxs-lookup"><span data-stu-id="a031c-142">Diagram View</span></span>
<span data-ttu-id="a031c-143">A Diagram nézetben az adat-előállító egytáblás üveg felügyeletéhez és a data factory és az eszközök kezeléséhez biztosít.</span><span class="sxs-lookup"><span data-stu-id="a031c-143">The Diagram View of a data factory provides a single pane of glass to monitor and manage a data factory and its assets.</span></span> <span data-ttu-id="a031c-144">Ha a Diagram nézetben kiválaszt egy adat-előállító entitás (dataset/pipeline):</span><span class="sxs-lookup"><span data-stu-id="a031c-144">When you select a Data Factory entity (dataset/pipeline) in the Diagram View:</span></span>

* <span data-ttu-id="a031c-145">A data factory entitás a faszerkezetes nézetben kiválasztott van.</span><span class="sxs-lookup"><span data-stu-id="a031c-145">The data factory entity is selected in the tree view.</span></span>
* <span data-ttu-id="a031c-146">A társított tevékenység windows a tevékenység Windows listán vannak kiemelve.</span><span class="sxs-lookup"><span data-stu-id="a031c-146">The associated activity windows are highlighted in the Activity Windows list.</span></span>
* <span data-ttu-id="a031c-147">A Tulajdonságok ablakban láthatók a kiválasztott objektum tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="a031c-147">The properties of the selected object are shown in the Properties window.</span></span>

<span data-ttu-id="a031c-148">Ha a folyamat (nem a szünetel) engedélyezve van, a zöld vonallal is látható:</span><span class="sxs-lookup"><span data-stu-id="a031c-148">When the pipeline is enabled (not in a paused state), it's shown with a green line:</span></span>

![A folyamat fut](./media/data-factory-monitor-manage-app/PipelineRunning.png)

<span data-ttu-id="a031c-150">Szüneteltetése, folytatása vagy jelölje ki azt a diagram nézetben és a gombok segítségével a parancssávon folyamat leáll.</span><span class="sxs-lookup"><span data-stu-id="a031c-150">You can pause, resume, or terminate a pipeline by selecting it in the diagram view and using the buttons on the command bar.</span></span>

![A parancssávon felfüggesztése vagy folytatása](./media/data-factory-monitor-manage-app/SuspendResumeOnCommandBar.png)
 
<span data-ttu-id="a031c-152">Nincsenek három gombok a tölcsér a Diagram nézetben.</span><span class="sxs-lookup"><span data-stu-id="a031c-152">There are three command bar buttons for the pipeline in the Diagram View.</span></span> <span data-ttu-id="a031c-153">A második gomb segítségével felfüggeszti a folyamatot.</span><span class="sxs-lookup"><span data-stu-id="a031c-153">You can use the second button to pause the pipeline.</span></span> <span data-ttu-id="a031c-154">Felfüggesztése nem leállítja a futó tevékenységeket, és lehetővé teszi, hogy folytatható befejezésig.</span><span class="sxs-lookup"><span data-stu-id="a031c-154">Pausing doesn't terminate the currently running activities and lets them proceed to completion.</span></span> <span data-ttu-id="a031c-155">A harmadik gomb megszakítja a folyamatot, és a meglévő tevékenységek végrehajtása leáll.</span><span class="sxs-lookup"><span data-stu-id="a031c-155">The third button pauses the pipeline and terminates its existing executing activities.</span></span> <span data-ttu-id="a031c-156">Az első gombra a folyamat folytatódik.</span><span class="sxs-lookup"><span data-stu-id="a031c-156">The first button resumes the pipeline.</span></span> <span data-ttu-id="a031c-157">Ha a feldolgozási sor fel van függesztve, a folyamat színe megváltozik.</span><span class="sxs-lookup"><span data-stu-id="a031c-157">When your pipeline is paused, the color of the pipeline changes.</span></span> <span data-ttu-id="a031c-158">Például egy felfüggesztett folyamat néz ki az alábbi képen:</span><span class="sxs-lookup"><span data-stu-id="a031c-158">For example, a paused pipeline looks like in the following image:</span></span> 

![Feldolgozási sor felfüggesztve](./media/data-factory-monitor-manage-app/PipelinePaused.png)

<span data-ttu-id="a031c-160">A Ctrl billentyűt használja a többszörös kiválasztási két vagy több folyamatok segítségével.</span><span class="sxs-lookup"><span data-stu-id="a031c-160">You can multi-select two or more pipelines by using the Ctrl key.</span></span> <span data-ttu-id="a031c-161">A gombok segítségével felfüggesztése vagy folytatása több folyamatok egyszerre.</span><span class="sxs-lookup"><span data-stu-id="a031c-161">You can use the command bar buttons to pause/resume multiple pipelines at a time.</span></span>

<span data-ttu-id="a031c-162">Kattintson a jobb gombbal egy folyamatot is, és beállítások is választhatók felfüggesztése, folytatása vagy egy folyamat leáll.</span><span class="sxs-lookup"><span data-stu-id="a031c-162">You can also right-click a pipeline and select options to suspend, resume, or terminate a pipeline.</span></span> 

![Az adatcsatorna helyi menü](./media/data-factory-monitor-manage-app/right-click-menu-for-pipeline.png)

<span data-ttu-id="a031c-164">Kattintson a **nyitott folyamat** megtekintéséhez az összes tevékenység a feldolgozási beállítás.</span><span class="sxs-lookup"><span data-stu-id="a031c-164">Click the **Open pipeline** option to see all the activities in the pipeline.</span></span> 

![Folyamat megnyitása menü](./media/data-factory-monitor-manage-app/OpenPipelineMenu.png)

<span data-ttu-id="a031c-166">A megnyitott csővezeték nézetben tekintse meg a sorban az összes tevékenység.</span><span class="sxs-lookup"><span data-stu-id="a031c-166">In the opened pipeline view, you see all activities in the pipeline.</span></span> <span data-ttu-id="a031c-167">Ebben a példában csak egy tevékenység nincs: másolási tevékenység.</span><span class="sxs-lookup"><span data-stu-id="a031c-167">In this example, there is only one activity: Copy Activity.</span></span> 

![Megnyitott folyamat](./media/data-factory-monitor-manage-app/OpenedPipeline.png)

<span data-ttu-id="a031c-169">Lépjen vissza az előző nézetével, kattintson a navigációs menü felső részén adat-előállító.</span><span class="sxs-lookup"><span data-stu-id="a031c-169">To go back to the previous view, click the data factory name in the breadcrumb menu at the top.</span></span>

<span data-ttu-id="a031c-170">Az adatcsatorna nézetben egy kimeneti adatkészletet, vagy amikor az egér átvitele a kimeneti adatkészlet kiválasztása után megjelenik a tevékenység Windows előugró ablak, hogy az adatkészlethez.</span><span class="sxs-lookup"><span data-stu-id="a031c-170">In the pipeline view, when you select an output dataset or when you move your mouse over the output dataset, you see the Activity Windows pop-up window for that dataset.</span></span>

![Tevékenység Windows előugró ablak](./media/data-factory-monitor-manage-app/ActivityWindowsPopup.png)

<span data-ttu-id="a031c-172">Egy tevékenység ablakban ki azt a részletek megtekintéséhez kattintson a **tulajdonságok** a jobb oldali ablak.</span><span class="sxs-lookup"><span data-stu-id="a031c-172">You can click an activity window to see details for it in the **Properties** window in the right pane.</span></span>

![Tevékenység ablak tulajdonságai](./media/data-factory-monitor-manage-app/ActivityWindowProperties.png)

<span data-ttu-id="a031c-174">A jobb oldali ablaktáblában váltani a **tevékenység ablak Explorer** lap további részletek megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="a031c-174">In the right pane, switch to the **Activity Window Explorer** tab to see more details.</span></span>

![Tevékenység ablak Explorer](./media/data-factory-monitor-manage-app/ActivityWindowExplorer.png)

<span data-ttu-id="a031c-176">Azt is láthatja, **változók feloldva** az egyes futtatására tett kísérlet egy tevékenység a **kísérletek** szakasz.</span><span class="sxs-lookup"><span data-stu-id="a031c-176">You also see **resolved variables** for each run attempt for an activity in the **Attempts** section.</span></span>

![Megoldott változók](./media/data-factory-monitor-manage-app/ResolvedVariables.PNG)

<span data-ttu-id="a031c-178">Váltás a **parancsfájl** fülre a JSON-parancsfájl definícióból a kijelölt objektum megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="a031c-178">Switch to the **Script** tab to see the JSON script definition for the selected object.</span></span>   

![Parancsfájl lap](./media/data-factory-monitor-manage-app/ScriptTab.png)

<span data-ttu-id="a031c-180">Tevékenység windows három helyen tekintheti meg:</span><span class="sxs-lookup"><span data-stu-id="a031c-180">You can see activity windows in three places:</span></span>

* <span data-ttu-id="a031c-181">A tevékenység Windows előugró ablak az a Diagram nézet (középső ablaktábla).</span><span class="sxs-lookup"><span data-stu-id="a031c-181">The Activity Windows pop-up in the Diagram View (middle pane).</span></span>
* <span data-ttu-id="a031c-182">A tevékenység ablak Explorer, a jobb oldali ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="a031c-182">The Activity Window Explorer in the right pane.</span></span>
* <span data-ttu-id="a031c-183">A tevékenység Windows listája az alsó ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="a031c-183">The Activity Windows list in the bottom pane.</span></span>

<span data-ttu-id="a031c-184">A tevékenység Windows előugró ablak és tevékenység ablak Explorer görgetve az előző héten és a következő hét a jobb és bal nyíl használatával.</span><span class="sxs-lookup"><span data-stu-id="a031c-184">In the Activity Windows pop-up and Activity Window Explorer, you can scroll to the previous week and the next week by using the left and right arrows.</span></span>

![Tevékenység ablak Explorer balra vagy jobbra nyíl](./media/data-factory-monitor-manage-app/ActivityWindowExplorerLeftRightArrows.png)

<span data-ttu-id="a031c-186">A Diagram nézet alján megjelenik az ilyen gombokat: Nagyítás a, Kicsinyítés, nagyítás beállítása, Nagyítás 100 %-os, Elrendezés zárolása.</span><span class="sxs-lookup"><span data-stu-id="a031c-186">At the bottom of the Diagram View, you see these buttons: Zoom In, Zoom Out, Zoom to Fit, Zoom 100%, Lock layout.</span></span> <span data-ttu-id="a031c-187">A **zárolási elrendezés** gomb megakadályozza a táblák és folyamatok véletlen áthelyezését a Diagram nézetben.</span><span class="sxs-lookup"><span data-stu-id="a031c-187">The **Lock layout** button prevents you from accidentally moving tables and pipelines in the Diagram View.</span></span> <span data-ttu-id="a031c-188">Alapértelmezés szerint le van.</span><span class="sxs-lookup"><span data-stu-id="a031c-188">It's on by default.</span></span> <span data-ttu-id="a031c-189">Kapcsolja ki, és entitások Navigálás a diagramban.</span><span class="sxs-lookup"><span data-stu-id="a031c-189">You can turn it off and move entities around in the diagram.</span></span> <span data-ttu-id="a031c-190">Kapcsolja ki, ha az utolsó gomb segítségével táblák és folyamatok automatikus formázása.</span><span class="sxs-lookup"><span data-stu-id="a031c-190">When you turn it off, you can use the last button to automatically position tables and pipelines.</span></span> <span data-ttu-id="a031c-191">Az egér kerekének használatával is bejövő vagy kimenő ráközelíthet.</span><span class="sxs-lookup"><span data-stu-id="a031c-191">You can also zoom in or out by using the mouse wheel.</span></span>

![Diagram nézet Nagyítás parancsok](./media/data-factory-monitor-manage-app/DiagramViewZoomCommands.png)

### <a name="activity-windows-list"></a><span data-ttu-id="a031c-193">A tevékenységek Windows listája</span><span class="sxs-lookup"><span data-stu-id="a031c-193">Activity Windows list</span></span>
<span data-ttu-id="a031c-194">A tevékenység Windows a középső ablaktáblán alján megjelenik az erőforrás-kezelővel vagy a Diagram nézetben kiválasztott adatkészlet összes tevékenység windows.</span><span class="sxs-lookup"><span data-stu-id="a031c-194">The Activity Windows list at the bottom of the middle pane displays all activity windows for the dataset that you selected in the Resource Explorer or the Diagram View.</span></span> <span data-ttu-id="a031c-195">Alapértelmezés szerint a lista van, csökkenő sorrendben, ami azt jelenti, hogy megjelenik-e a legújabb tevékenység ablak tetején.</span><span class="sxs-lookup"><span data-stu-id="a031c-195">By default, the list is in descending order, which means that you see the latest activity window at the top.</span></span>

![A tevékenységek Windows listája](./media/data-factory-monitor-manage-app/ActivityWindowsList.png)

<span data-ttu-id="a031c-197">Ebben a listában nem frissülnek automatikusan, ezért a frissítés gombra az eszköztáron kézi frissítéséhez.</span><span class="sxs-lookup"><span data-stu-id="a031c-197">This list doesn't refresh automatically, so use the refresh button on the toolbar to manually refresh it.</span></span>  

<span data-ttu-id="a031c-198">Tevékenység windows a következő állapotok valamelyikében lehet:</span><span class="sxs-lookup"><span data-stu-id="a031c-198">Activity windows can be in one of the following statuses:</span></span>

<table>
<tr>
    <th align="left"><span data-ttu-id="a031c-199">status</span><span class="sxs-lookup"><span data-stu-id="a031c-199">Status</span></span></th><th align="left"><span data-ttu-id="a031c-200">A részállapot</span><span class="sxs-lookup"><span data-stu-id="a031c-200">Substatus</span></span></th><th align="left"><span data-ttu-id="a031c-201">Leírás</span><span class="sxs-lookup"><span data-stu-id="a031c-201">Description</span></span></th>
</tr>
<tr>
    <td rowspan="8"><span data-ttu-id="a031c-202">Várakozás</span><span class="sxs-lookup"><span data-stu-id="a031c-202">Waiting</span></span></td><td><span data-ttu-id="a031c-203">ScheduleTime</span><span class="sxs-lookup"><span data-stu-id="a031c-203">ScheduleTime</span></span></td><td><span data-ttu-id="a031c-204">Az idő a tevékenység időszakot még nem érkezett.</span><span class="sxs-lookup"><span data-stu-id="a031c-204">The time hasn't come for the activity window to run.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="a031c-205">DatasetDependencies</span><span class="sxs-lookup"><span data-stu-id="a031c-205">DatasetDependencies</span></span></td><td><span data-ttu-id="a031c-206">A fölérendelt függőségek nem állnak készen.</span><span class="sxs-lookup"><span data-stu-id="a031c-206">The upstream dependencies aren't ready.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="a031c-207">ComputeResources</span><span class="sxs-lookup"><span data-stu-id="a031c-207">ComputeResources</span></span></td><td><span data-ttu-id="a031c-208">A számítási erőforrások nem érhetők el.</span><span class="sxs-lookup"><span data-stu-id="a031c-208">The compute resources aren't available.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="a031c-209">ConcurrencyLimit</span><span class="sxs-lookup"><span data-stu-id="a031c-209">ConcurrencyLimit</span></span></td> <td><span data-ttu-id="a031c-210">Az összes Tevékenységpéldány futtatásával elfoglalva más tevékenység windows.</span><span class="sxs-lookup"><span data-stu-id="a031c-210">All the activity instances are busy running other activity windows.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="a031c-211">ActivityResume</span><span class="sxs-lookup"><span data-stu-id="a031c-211">ActivityResume</span></span></td><td><span data-ttu-id="a031c-212">A tevékenység szüneteltetve van, és nem futtatható a tevékenység windows folytatásáig.</span><span class="sxs-lookup"><span data-stu-id="a031c-212">The activity is paused and can't run the activity windows until it's resumed.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="a031c-213">Próbálja meg újra</span><span class="sxs-lookup"><span data-stu-id="a031c-213">Retry</span></span></td><td><span data-ttu-id="a031c-214">A tevékenység végrehajtási lesz hajtva.</span><span class="sxs-lookup"><span data-stu-id="a031c-214">The activity execution is being retried.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="a031c-215">Ellenőrzés</span><span class="sxs-lookup"><span data-stu-id="a031c-215">Validation</span></span></td><td><span data-ttu-id="a031c-216">Érvényesítés még a még nem indult el.</span><span class="sxs-lookup"><span data-stu-id="a031c-216">Validation hasn't started yet.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="a031c-217">ValidationRetry</span><span class="sxs-lookup"><span data-stu-id="a031c-217">ValidationRetry</span></span></td><td><span data-ttu-id="a031c-218">Érvényesítés újrapróbálására vár.</span><span class="sxs-lookup"><span data-stu-id="a031c-218">Validation is waiting to be retried.</span></span></td>
</tr>
<tr>
<tr>
<td rowspan="2"><span data-ttu-id="a031c-219">Esetbejegyzések</span><span class="sxs-lookup"><span data-stu-id="a031c-219">InProgress</span></span></td><td><span data-ttu-id="a031c-220">Ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="a031c-220">Validating</span></span></td><td><span data-ttu-id="a031c-221">Ellenőrzése folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="a031c-221">Validation is in progress.</span></span></td>
</tr>
<td>-</td>
<td><span data-ttu-id="a031c-222">A tevékenység ablakban feldolgozása folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="a031c-222">The activity window is being processed.</span></span></td>
</tr>
<tr>
<td rowspan="4"><span data-ttu-id="a031c-223">Nem sikerült</span><span class="sxs-lookup"><span data-stu-id="a031c-223">Failed</span></span></td><td><span data-ttu-id="a031c-224">Időtúllépésbe került</span><span class="sxs-lookup"><span data-stu-id="a031c-224">TimedOut</span></span></td><td><span data-ttu-id="a031c-225">A tevékenység végrehajtási tevékenység által megengedett érték időt vett igénybe.</span><span class="sxs-lookup"><span data-stu-id="a031c-225">The activity execution took longer than what is allowed by the activity.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="a031c-226">Törölve</span><span class="sxs-lookup"><span data-stu-id="a031c-226">Canceled</span></span></td><td><span data-ttu-id="a031c-227">A tevékenység ablakban felhasználói művelet megszakította.</span><span class="sxs-lookup"><span data-stu-id="a031c-227">The activity window was canceled by user action.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="a031c-228">Ellenőrzés</span><span class="sxs-lookup"><span data-stu-id="a031c-228">Validation</span></span></td><td><span data-ttu-id="a031c-229">Sikertelen volt.</span><span class="sxs-lookup"><span data-stu-id="a031c-229">Validation has failed.</span></span></td>
</tr>
<tr>
<td>-</td><td><span data-ttu-id="a031c-230">Tevékenységéhez generált vagy érvényesítése nem sikerült.</span><span class="sxs-lookup"><span data-stu-id="a031c-230">The activity window failed to be generated or validated.</span></span></td>
</tr>
<td><span data-ttu-id="a031c-231">Készen áll</span><span class="sxs-lookup"><span data-stu-id="a031c-231">Ready</span></span></td><td>-</td><td><span data-ttu-id="a031c-232">A tevékenység ablakban készen áll a felhasználásra.</span><span class="sxs-lookup"><span data-stu-id="a031c-232">The activity window is ready for consumption.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="a031c-233">Kihagyva</span><span class="sxs-lookup"><span data-stu-id="a031c-233">Skipped</span></span></td><td>-</td><td><span data-ttu-id="a031c-234">A tevékenység ablak nincs feldolgozva.</span><span class="sxs-lookup"><span data-stu-id="a031c-234">The activity window wasn't processed.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="a031c-235">None</span><span class="sxs-lookup"><span data-stu-id="a031c-235">None</span></span></td><td>-</td><td><span data-ttu-id="a031c-236">Egy tevékenység ablakban létezett egy eltérő állapottal, de alaphelyzetbe lett állítva.</span><span class="sxs-lookup"><span data-stu-id="a031c-236">An activity window used to exist with a different status, but has been reset.</span></span></td>
</tr>
</table>


<span data-ttu-id="a031c-237">Ha a listában egy tevékenység ablakban gombra kattint, megjelenik az részleteit a a **tevékenység Windows Explorer** vagy a **tulajdonságok** a jobb oldali ablak.</span><span class="sxs-lookup"><span data-stu-id="a031c-237">When you click an activity window in the list, you see details about it in the **Activity Windows Explorer** or the **Properties** window on the right.</span></span>

![Tevékenység ablak Explorer](./media/data-factory-monitor-manage-app/ActivityWindowExplorer-2.png)

### <a name="refresh-activity-windows"></a><span data-ttu-id="a031c-239">Tevékenység windows frissítése</span><span class="sxs-lookup"><span data-stu-id="a031c-239">Refresh activity windows</span></span>
<span data-ttu-id="a031c-240">A részletek nem automatikusan frissülnek, ezért a frissítés (második) gombra a parancssávon manuális frissítéséhez a windows a tevékenységek listája.</span><span class="sxs-lookup"><span data-stu-id="a031c-240">The details aren't automatically refreshed, so use the refresh button (the second button) on the command bar to manually refresh the activity windows list.</span></span>  

### <a name="properties-window"></a><span data-ttu-id="a031c-241">Tulajdonságok ablak</span><span class="sxs-lookup"><span data-stu-id="a031c-241">Properties window</span></span>
<span data-ttu-id="a031c-242">A Tulajdonságok ablak van, a figyelés és a felügyeleti alkalmazás a jobb szélső panelén.</span><span class="sxs-lookup"><span data-stu-id="a031c-242">The Properties window is in the right-most pane of the Monitoring and Management app.</span></span>

![Tulajdonságok ablak](./media/data-factory-monitor-manage-app/PropertiesWindow.png)

<span data-ttu-id="a031c-244">Megjeleníti az erőforrás-kezelő (fanézetben), a Diagram nézet vagy a tevékenység Windows listán kijelölt elem tulajdonságai.</span><span class="sxs-lookup"><span data-stu-id="a031c-244">It displays properties for the item that you selected in the Resource Explorer (tree view), Diagram View, or Activity Windows list.</span></span>

### <a name="activity-window-explorer"></a><span data-ttu-id="a031c-245">Tevékenység ablak Explorer</span><span class="sxs-lookup"><span data-stu-id="a031c-245">Activity Window Explorer</span></span>
<span data-ttu-id="a031c-246">A **tevékenység ablak Explorer** időszak van a figyelés és a felügyeleti alkalmazás a jobb szélső panelén.</span><span class="sxs-lookup"><span data-stu-id="a031c-246">The **Activity Window Explorer** window is in the right-most pane of the Monitoring and Management app.</span></span> <span data-ttu-id="a031c-247">A tevékenység ablakban, a tevékenység Windows előugró ablakban vagy a tevékenység Windows listában kiválasztott részleteit jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="a031c-247">It displays details about the activity window that you selected in the Activity Windows pop-up window or the Activity Windows list.</span></span>

![Tevékenység ablak Explorer](./media/data-factory-monitor-manage-app/ActivityWindowExplorer-3.png)

<span data-ttu-id="a031c-249">Egy másik tevékenység ablak tetején naptár nézetben kattintással lehet váltani.</span><span class="sxs-lookup"><span data-stu-id="a031c-249">You can switch to another activity window by clicking it in the calendar view at the top.</span></span> <span data-ttu-id="a031c-250">Lásd az előző hét vagy a következő hét tevékenység windows tetején a bal oldali/jobbra gomb is használja.</span><span class="sxs-lookup"><span data-stu-id="a031c-250">You can also use the left arrow/right arrow buttons at the top to see activity windows from the previous week or the next week.</span></span>

<span data-ttu-id="a031c-251">Segítségével gombokat az alsó ablaktáblában futtassa újra a tevékenység ablakban, vagy frissítse a részletek ablaktáblájában.</span><span class="sxs-lookup"><span data-stu-id="a031c-251">You can use the toolbar buttons in the bottom pane to rerun the activity window or refresh the details in the pane.</span></span>

### <a name="script"></a><span data-ttu-id="a031c-252">Szkript</span><span class="sxs-lookup"><span data-stu-id="a031c-252">Script</span></span>
<span data-ttu-id="a031c-253">Használhatja a **parancsfájl** fülre kattintva megtekintheti a kijelölt adat-előállító entitás (társított szolgáltatás, adatkészlet vagy csővezeték) JSON-definícióból.</span><span class="sxs-lookup"><span data-stu-id="a031c-253">You can use the **Script** tab to view the JSON definition of the selected Data Factory entity (linked service, dataset, or pipeline).</span></span>

![Parancsfájl lap](./media/data-factory-monitor-manage-app/ScriptTab.png)

## <a name="use-system-views"></a><span data-ttu-id="a031c-255">Rendszer-nézetek</span><span class="sxs-lookup"><span data-stu-id="a031c-255">Use system views</span></span>
<span data-ttu-id="a031c-256">A figyelés és felügyelet alkalmazást tartalmaz, előre összeállított rendszernézetek (**legutóbbi tevékenységek windows**, **sikertelen volt a tevékenység windows**, **folyamatban lévő tevékenységek windows**), amelyek lehetővé teszik, hogy a data factory legutóbbi/nem sikerült/folyamatban tevékenységablakok megtekintése.</span><span class="sxs-lookup"><span data-stu-id="a031c-256">The Monitoring and Management app includes pre-built system views (**Recent activity windows**, **Failed activity windows**, **In-Progress activity windows**) that allow you to view recent/failed/in-progress activity windows for your data factory.</span></span>

<span data-ttu-id="a031c-257">Váltás a **figyelési nézetei** lapon kattintson a bal oldalon.</span><span class="sxs-lookup"><span data-stu-id="a031c-257">Switch to the **Monitoring Views** tab on the left by clicking it.</span></span>

![Figyelési nézetek lap](./media/data-factory-monitor-manage-app/MonitoringViewsTab.png)

<span data-ttu-id="a031c-259">Jelenleg nincsenek három rendszernézetek támogatott.</span><span class="sxs-lookup"><span data-stu-id="a031c-259">Currently, there are three system views that are supported.</span></span> <span data-ttu-id="a031c-260">Válassza ki a legutóbbi tevékenységek windows, a sikertelen tevékenységet windows vagy a folyamatban lévő tevékenységek windows a tevékenység Windows listában (középső ablaktábla alján) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="a031c-260">Select an option to see recent activity windows, failed activity windows, or in-progress activity windows in the Activity Windows list (at the bottom of the middle pane).</span></span>

<span data-ttu-id="a031c-261">Ha bejelöli a **legutóbbi tevékenységek windows** beállítást, megjelenik minden legutóbbi tevékenységek windows csökkenő sorrendben a **utolsó kísérlet ideje**.</span><span class="sxs-lookup"><span data-stu-id="a031c-261">When you select the **Recent activity windows** option, you see all recent activity windows in descending order of the **last attempt time**.</span></span>

<span data-ttu-id="a031c-262">Használhatja a **sikertelen volt a tevékenység windows** nézetre, és tekintse meg a listában az összes sikertelen tevékenység windows.</span><span class="sxs-lookup"><span data-stu-id="a031c-262">You can use the **Failed activity windows** view to see all failed activity windows in the list.</span></span> <span data-ttu-id="a031c-263">Egy meghiúsult tevékenységet ablak az információk a részletes listáján válassza ki a **tulajdonságok** ablakban vagy a **tevékenység ablak Explorer**.</span><span class="sxs-lookup"><span data-stu-id="a031c-263">Select a failed activity window in the list to see details about it in the **Properties** window or the **Activity Window Explorer**.</span></span> <span data-ttu-id="a031c-264">A naplók a sikertelen tevékenységet időszak is letöltheti.</span><span class="sxs-lookup"><span data-stu-id="a031c-264">You can also download any logs for a failed activity window.</span></span>

## <a name="sort-and-filter-activity-windows"></a><span data-ttu-id="a031c-265">Rendezésére és szűrésére tevékenység windows</span><span class="sxs-lookup"><span data-stu-id="a031c-265">Sort and filter activity windows</span></span>
<span data-ttu-id="a031c-266">Módosítsa a **kezdési időpont** és **befejező időpontja** beállításai a szűrő tevékenység windows parancsra a parancssávon.</span><span class="sxs-lookup"><span data-stu-id="a031c-266">Change the **start time** and **end time** settings in the command bar to filter activity windows.</span></span> <span data-ttu-id="a031c-267">A kezdési és befejezési időpontjának módosítása után a gombra a tevékenység Windows listájának frissítése a befejező időpont mellett.</span><span class="sxs-lookup"><span data-stu-id="a031c-267">After you change the start time and end time, click the button next to the end time to refresh the Activity Windows list.</span></span>

![Kezdő és befejező időpontja](./media/data-factory-monitor-manage-app/StartAndEndTimes.png)

> [!NOTE]
> <span data-ttu-id="a031c-269">Minden alkalommal jelenleg a figyelés és a felügyeleti alkalmazás UTC formátumban.</span><span class="sxs-lookup"><span data-stu-id="a031c-269">Currently, all times are in UTC format in the Monitoring and Management app.</span></span>
>
>

<span data-ttu-id="a031c-270">Az a **tevékenység Windows lista**, kattintson az oszlop nevét (például: állapot).</span><span class="sxs-lookup"><span data-stu-id="a031c-270">In the **Activity Windows list**, click the name of a column (for example: Status).</span></span>

![Tevékenység Windows lista oszlop menü](./media/data-factory-monitor-manage-app/ActivityWindowsListColumnMenu.png)

<span data-ttu-id="a031c-272">Tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="a031c-272">You can do the following:</span></span>

* <span data-ttu-id="a031c-273">Rendezés növekvő sorrendben.</span><span class="sxs-lookup"><span data-stu-id="a031c-273">Sort in ascending order.</span></span>
* <span data-ttu-id="a031c-274">Rendezés csökkenő sorrendben.</span><span class="sxs-lookup"><span data-stu-id="a031c-274">Sort in descending order.</span></span>
* <span data-ttu-id="a031c-275">Szűrés egy vagy több (kész, Várakozás, és így tovább).</span><span class="sxs-lookup"><span data-stu-id="a031c-275">Filter by one or more values (Ready, Waiting, and so on).</span></span>

<span data-ttu-id="a031c-276">Ha szűrőt ad meg egy olyan oszlop, tekintse meg a szűrő gombra az adott oszlop, amely azt jelzi, hogy az oszlopban szereplő értékek szűrt értékek engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="a031c-276">When you specify a filter on a column, you see the filter button enabled for that column, which indicates that the values in the column are filtered values.</span></span>

![A tevékenység Windows lista oszlopon szűrése](./media/data-factory-monitor-manage-app/ActivityWindowsListFilterInColumn.png)

<span data-ttu-id="a031c-278">Szűrő törlése használhatja ugyanabban az előugró ablakban.</span><span class="sxs-lookup"><span data-stu-id="a031c-278">You can use the same pop-up window to clear filters.</span></span> <span data-ttu-id="a031c-279">A tevékenység Windows lista az összes szűrő törlése, kattintson a szűrő törlése gombra a parancssávon.</span><span class="sxs-lookup"><span data-stu-id="a031c-279">To clear all filters for the Activity Windows list, click the clear filter button on the command bar.</span></span>

![A tevékenység Windows lista az összes szűrő törlése](./media/data-factory-monitor-manage-app/ClearAllFiltersActivityWindowsList.png)

## <a name="perform-batch-actions"></a><span data-ttu-id="a031c-281">Kötegelt műveleteket</span><span class="sxs-lookup"><span data-stu-id="a031c-281">Perform batch actions</span></span>
### <a name="rerun-selected-activity-windows"></a><span data-ttu-id="a031c-282">Futtassa újra a kijelölt tevékenység windows</span><span class="sxs-lookup"><span data-stu-id="a031c-282">Rerun selected activity windows</span></span>
<span data-ttu-id="a031c-283">Egy tevékenység ablak, kattintson a lefelé mutató nyílra az első parancs sáv gombhoz válassza ki és **újrafuttatása** / **futtassa újra a előtt folyamat**.</span><span class="sxs-lookup"><span data-stu-id="a031c-283">Select an activity window, click the down arrow for the first command bar button, and select **Rerun** / **Rerun with upstream in pipeline**.</span></span> <span data-ttu-id="a031c-284">Ha bejelöli a **futtassa újra a előtt folyamat** beállítás, az összes felsőbb szintű tevékenység windows is Újrafuttatja.</span><span class="sxs-lookup"><span data-stu-id="a031c-284">When you select the **Rerun with upstream in pipeline** option, it reruns all upstream activity windows as well.</span></span>
    <span data-ttu-id="a031c-285">![Futtassa újra a műveletet egy tevékenység ablak](./media/data-factory-monitor-manage-app/ReRunSlice.png)</span><span class="sxs-lookup"><span data-stu-id="a031c-285">![Rerun an activity window](./media/data-factory-monitor-manage-app/ReRunSlice.png)</span></span>

<span data-ttu-id="a031c-286">Is több tevékenység windows listáján válassza ki, és futtassa újra a azokat egy időben.</span><span class="sxs-lookup"><span data-stu-id="a031c-286">You can also select multiple activity windows in the list and rerun them at the same time.</span></span> <span data-ttu-id="a031c-287">Tevékenység windows állapota alapján szűrni kívánt (például: **sikertelen**) –, majd futtassa újra a sikertelen tevékenységet windows a problémát, amelynek hatására a tevékenység windows sikertelen kijavítása után.</span><span class="sxs-lookup"><span data-stu-id="a031c-287">You might want to filter activity windows based on the status (for example: **Failed**)--and then rerun the failed activity windows after correcting the issue that causes the activity windows to fail.</span></span> <span data-ttu-id="a031c-288">Lásd a következő tevékenység windows listájában szűrés vonatkozó további információért.</span><span class="sxs-lookup"><span data-stu-id="a031c-288">See the following section for details about filtering activity windows in the list.</span></span>  

### <a name="pauseresume-multiple-pipelines"></a><span data-ttu-id="a031c-289">Több folyamatok szüneteltet</span><span class="sxs-lookup"><span data-stu-id="a031c-289">Pause/resume multiple pipelines</span></span>
<span data-ttu-id="a031c-290">A Ctrl billentyűt használja a multiselect két vagy több folyamatok segítségével.</span><span class="sxs-lookup"><span data-stu-id="a031c-290">You can multiselect two or more pipelines by using the Ctrl key.</span></span> <span data-ttu-id="a031c-291">A (amely emel ki az alábbi képen piros téglalap) gombok segítségével/szüneteltet őket.</span><span class="sxs-lookup"><span data-stu-id="a031c-291">You can use the command bar buttons (which are highlighted in the red rectangle in the following image) to pause/resume them.</span></span>

![A parancssávon felfüggesztése vagy folytatása](./media/data-factory-monitor-manage-app/SuspendResumeOnCommandBar.png)

## <a name="create-alerts"></a><span data-ttu-id="a031c-293">Riasztások létrehozása</span><span class="sxs-lookup"><span data-stu-id="a031c-293">Create alerts</span></span>
<span data-ttu-id="a031c-294">A **riasztások** lap lehetővé teszi, hogy hozzon létre riasztást és nézet/Szerkesztés/törlés meglévő riasztást.</span><span class="sxs-lookup"><span data-stu-id="a031c-294">The **Alerts** page lets you create an alert and view/edit/delete existing alerts.</span></span> <span data-ttu-id="a031c-295">Akkor is is tiltása/engedélyezése egy riasztást.</span><span class="sxs-lookup"><span data-stu-id="a031c-295">You can also disable/enable an alert.</span></span> <span data-ttu-id="a031c-296">A riasztások lapon megtekintéséhez kattintson a **riasztások** fülre.</span><span class="sxs-lookup"><span data-stu-id="a031c-296">To see the Alerts page, click the **Alerts** tab.</span></span>

![Riasztások lap](./media/data-factory-monitor-manage-app/AlertsTab.png)

### <a name="to-create-an-alert"></a><span data-ttu-id="a031c-298">Riasztás létrehozása</span><span class="sxs-lookup"><span data-stu-id="a031c-298">To create an alert</span></span>
1. <span data-ttu-id="a031c-299">Kattintson a **hozzáadása riasztás** hozzáadása egy riasztást.</span><span class="sxs-lookup"><span data-stu-id="a031c-299">Click **Add Alert** to add an alert.</span></span> <span data-ttu-id="a031c-300">Megjelenik a **részletek** lap.</span><span class="sxs-lookup"><span data-stu-id="a031c-300">You see the **Details** page.</span></span>

    ![Riasztások – Részletek lap létrehozása](./media/data-factory-monitor-manage-app/CreateAlertDetailsPage.png)
2. <span data-ttu-id="a031c-302">Adja meg a **neve** és **leírás** a riasztást, majd kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="a031c-302">Specify the **Name** and **Description** for the alert, and click **Next**.</span></span> <span data-ttu-id="a031c-303">Megjelenik a **szűrők** lap.</span><span class="sxs-lookup"><span data-stu-id="a031c-303">You should see the **Filters** page.</span></span>

    ![Riasztások – szűrők lap létrehozása](./media/data-factory-monitor-manage-app/CreateAlertFiltersPage.png)
3. <span data-ttu-id="a031c-305">Válassza ki a **esemény**, **állapot**, és **részállapot** (nem kötelező), hogy szeretné-e a Data Factory szolgáltatás riasztás létrehozása, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="a031c-305">Select the **event**, **status**, and **substatus** (optional) that you want to create a Data Factory service alert for, and click **Next**.</span></span> <span data-ttu-id="a031c-306">Megjelenik a **címzettek** lap.</span><span class="sxs-lookup"><span data-stu-id="a031c-306">You should see the **Recipients** page.</span></span>

    ![Riasztások – címzettek lap létrehozása](./media/data-factory-monitor-manage-app/CreateAlertRecipientsPage.png)
4. <span data-ttu-id="a031c-308">Válassza ki a **E-mail-előfizetés rendszergazdái** lehetőséget és/vagy adjon meg egy **további rendszergazdai e-mail**, és kattintson a **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="a031c-308">Select the **Email subscription admins** option and/or enter an **additional administrator email**, and click **Finish**.</span></span> <span data-ttu-id="a031c-309">Meg kell jelennie a listában a riasztást.</span><span class="sxs-lookup"><span data-stu-id="a031c-309">You should see the alert in the list.</span></span>

    ![Riasztások listája](./media/data-factory-monitor-manage-app/AlertsList.png)

<span data-ttu-id="a031c-311">A riasztáslistában Szerkesztés/Törlés/tiltása/engedélyezése egy riasztást a riasztás társított gombok használatával.</span><span class="sxs-lookup"><span data-stu-id="a031c-311">In the Alerts list, use the buttons that are associated with the alert to edit/delete/disable/enable an alert.</span></span>

### <a name="eventstatussubstatus"></a><span data-ttu-id="a031c-312">A részállapot/esemény/állapota</span><span class="sxs-lookup"><span data-stu-id="a031c-312">Event/status/substatus</span></span>
<span data-ttu-id="a031c-313">A következő táblázat a rendelkezésre álló eseményeket és állapotokat (és részállapotok) listáját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="a031c-313">The following table provides the list of available events and statuses (and substatuses).</span></span>

| <span data-ttu-id="a031c-314">esemény neve</span><span class="sxs-lookup"><span data-stu-id="a031c-314">Event name</span></span> | <span data-ttu-id="a031c-315">status</span><span class="sxs-lookup"><span data-stu-id="a031c-315">Status</span></span> | <span data-ttu-id="a031c-316">A részállapot</span><span class="sxs-lookup"><span data-stu-id="a031c-316">Substatus</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a031c-317">A tevékenység futtatása megkezdődött</span><span class="sxs-lookup"><span data-stu-id="a031c-317">Activity Run Started</span></span> |<span data-ttu-id="a031c-318">Megkezdődött</span><span class="sxs-lookup"><span data-stu-id="a031c-318">Started</span></span> |<span data-ttu-id="a031c-319">Indulás alatt</span><span class="sxs-lookup"><span data-stu-id="a031c-319">Starting</span></span> |
| <span data-ttu-id="a031c-320">A tevékenység futtatása befejeződött</span><span class="sxs-lookup"><span data-stu-id="a031c-320">Activity Run Finished</span></span> |<span data-ttu-id="a031c-321">Sikeres</span><span class="sxs-lookup"><span data-stu-id="a031c-321">Succeeded</span></span> |<span data-ttu-id="a031c-322">Sikeres</span><span class="sxs-lookup"><span data-stu-id="a031c-322">Succeeded</span></span> |
| <span data-ttu-id="a031c-323">A tevékenység futtatása befejeződött</span><span class="sxs-lookup"><span data-stu-id="a031c-323">Activity Run Finished</span></span> |<span data-ttu-id="a031c-324">Nem sikerült</span><span class="sxs-lookup"><span data-stu-id="a031c-324">Failed</span></span> |<span data-ttu-id="a031c-325">Nem sikerült erőforrás-elosztás</span><span class="sxs-lookup"><span data-stu-id="a031c-325">Failed Resource Allocation</span></span><br/><br/><span data-ttu-id="a031c-326">Sikertelen végrehajtása</span><span class="sxs-lookup"><span data-stu-id="a031c-326">Failed Execution</span></span><br/><br/><span data-ttu-id="a031c-327">Túllépte az időkorlátot</span><span class="sxs-lookup"><span data-stu-id="a031c-327">Timed Out</span></span><br/><br/><span data-ttu-id="a031c-328">Érvényesítés</span><span class="sxs-lookup"><span data-stu-id="a031c-328">Failed Validation</span></span><br/><br/><span data-ttu-id="a031c-329">Elhagyott</span><span class="sxs-lookup"><span data-stu-id="a031c-329">Abandoned</span></span> |
| <span data-ttu-id="a031c-330">Igény szerinti HDI-fürtöt létrehozni elindítva</span><span class="sxs-lookup"><span data-stu-id="a031c-330">On-Demand HDI Cluster Create Started</span></span> |<span data-ttu-id="a031c-331">Megkezdődött</span><span class="sxs-lookup"><span data-stu-id="a031c-331">Started</span></span> |-|
| <span data-ttu-id="a031c-332">Igény szerinti HDI-fürtnek sikeresen létrehozva</span><span class="sxs-lookup"><span data-stu-id="a031c-332">On-Demand HDI Cluster Created Successfully</span></span> |<span data-ttu-id="a031c-333">Sikeres</span><span class="sxs-lookup"><span data-stu-id="a031c-333">Succeeded</span></span> |-|
| <span data-ttu-id="a031c-334">Igény szerinti HDI-fürtnek törlése</span><span class="sxs-lookup"><span data-stu-id="a031c-334">On-Demand HDI Cluster Deleted</span></span> |<span data-ttu-id="a031c-335">Sikeres</span><span class="sxs-lookup"><span data-stu-id="a031c-335">Succeeded</span></span> |-|

### <a name="to-edit-delete-or-disable-an-alert"></a><span data-ttu-id="a031c-336">Szerkesztése, törlése, vagy tiltsa le a riasztás</span><span class="sxs-lookup"><span data-stu-id="a031c-336">To edit, delete, or disable an alert</span></span>

<span data-ttu-id="a031c-337">A (pirossal kiemelt) következő gombok segítségével szerkesztése, törlése, vagy tiltsa le a riasztást.</span><span class="sxs-lookup"><span data-stu-id="a031c-337">Use the following buttons (highlighted in red) to edit, delete, or disable an alert.</span></span>

![Riasztások gombok](./media/data-factory-monitor-manage-app/AlertButtons.png)
