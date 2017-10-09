---
title: "aaaMonitor és adatok folyamatok - Azure kezelése |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse figyelés és a felügyeleti alkalmazás toomonitor hello és Azure adat-előállítók és folyamatok kezelése."
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
ms.openlocfilehash: 5e4ef6ec5fb8ebc9bda0be7899a39a51d58403d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-and-manage-azure-data-factory-pipelines-by-using-hello-monitoring-and-management-app"></a><span data-ttu-id="58ad7-103">Figyelheti és kezelheti az Azure Data Factory folyamatok hello figyelés és felügyelet alkalmazással</span><span class="sxs-lookup"><span data-stu-id="58ad7-103">Monitor and manage Azure Data Factory pipelines by using hello Monitoring and Management app</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="58ad7-104">Az Azure portálon vagy az Azure PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="58ad7-104">Using Azure portal/Azure PowerShell</span></span>](data-factory-monitor-manage-pipelines.md)
> * [<span data-ttu-id="58ad7-105">Használatával figyelése és a felügyeleti alkalmazás</span><span class="sxs-lookup"><span data-stu-id="58ad7-105">Using Monitoring and Management app</span></span>](data-factory-monitor-manage-app.md)
>
>

<span data-ttu-id="58ad7-106">Ez a cikk ismerteti, hogyan toouse hello figyelés és a felügyeleti alkalmazás toomonitor, kezelése és az adat-előállító adatcsatornák hibakeresését.</span><span class="sxs-lookup"><span data-stu-id="58ad7-106">This article describes how toouse hello Monitoring and Management app toomonitor, manage, and debug your Data Factory pipelines.</span></span> <span data-ttu-id="58ad7-107">Azt is megtudhatja hogyan toocreate riasztások hibáiról értesítés tooget.</span><span class="sxs-lookup"><span data-stu-id="58ad7-107">It also provides information on how toocreate alerts tooget notified on failures.</span></span> <span data-ttu-id="58ad7-108">Ismerkedés hello alkalmazás használatával a következő néznek hello által videót:</span><span class="sxs-lookup"><span data-stu-id="58ad7-108">You can get started with using hello application by watching hello following video:</span></span>

> [!NOTE]
> <span data-ttu-id="58ad7-109">hello felhasználói felülete látható hello videó előfordulhat, hogy nem egyeznek pontosan kapcsolatban a hello portálon.</span><span class="sxs-lookup"><span data-stu-id="58ad7-109">hello user interface shown in hello video may not exactly match what you see in hello portal.</span></span> <span data-ttu-id="58ad7-110">Némileg régebbi, de fogalmak hello továbbra is azonos.</span><span class="sxs-lookup"><span data-stu-id="58ad7-110">It's slightly older, but concepts remain hello same.</span></span> 

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Azure-Data-Factory-Monitoring-and-Managing-Big-Data-Piplines/player]
>

## <a name="launch-hello-monitoring-and-management-app"></a><span data-ttu-id="58ad7-111">Hello figyelés és a felügyeleti alkalmazás indítása</span><span class="sxs-lookup"><span data-stu-id="58ad7-111">Launch hello Monitoring and Management app</span></span>
<span data-ttu-id="58ad7-112">toolaunch hello figyelése és a felügyeleti alkalmazás, kattintson a hello **figyelő & kezelése** hello csempét **adat-előállító** a data factory paneljét.</span><span class="sxs-lookup"><span data-stu-id="58ad7-112">toolaunch hello Monitor and Management app, click hello **Monitor & Manage** tile on hello **Data Factory** blade for your data factory.</span></span>

![Hello adat-előállító kezdőlapján csempe figyelése](./media/data-factory-monitor-manage-app/MonitoringAppTile.png)

<span data-ttu-id="58ad7-114">Nyisson meg egy külön ablakban hello figyelés és a felügyeleti alkalmazás kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="58ad7-114">You should see hello Monitoring and Management app open in a separate window.</span></span>  

![Figyelési és felügyeleti alkalmazás](./media/data-factory-monitor-manage-app/AppLaunched.png)

> [!NOTE]
> <span data-ttu-id="58ad7-116">Ha azt látja, hogy hello webböngésző akadt-e a "Engedélyező...", törölje a jelet hello **külső cookie-k blokkolását, és a helyadatok** jelölőnégyzet – vagy a tárolás során is garantálják az kiválasztva, hozzon létre egy kivételt **login.microsoftonline.com** , majd próbálkozzon újra a tooopen hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="58ad7-116">If you see that hello web browser is stuck at "Authorizing...", clear hello **Block third-party cookies and site data** check box--or keep it selected, create an exception for **login.microsoftonline.com**, and then try tooopen hello app again.</span></span>


<span data-ttu-id="58ad7-117">Hello tevékenység Windows hello középső ablaktábla listájában megjelenik egy tevékenység ablakban tevékenység minden egyes futtatásához.</span><span class="sxs-lookup"><span data-stu-id="58ad7-117">In hello Activity Windows list in hello middle pane, you see an activity window for each run of an activity.</span></span> <span data-ttu-id="58ad7-118">Például ha hello ütemezett tevékenység toorun óránkénti rendelkezik öt órát, látható öt tevékenység windows társított öt adatszeletek.</span><span class="sxs-lookup"><span data-stu-id="58ad7-118">For example, if you have hello activity scheduled toorun hourly for five hours, you see five activity windows associated with five data slices.</span></span> <span data-ttu-id="58ad7-119">Ha nem lát tevékenység windows hello listában hello lap alján, a hello, a következő:</span><span class="sxs-lookup"><span data-stu-id="58ad7-119">If you don't see activity windows in hello list at hello bottom, do hello following:</span></span>
 
- <span data-ttu-id="58ad7-120">Frissítés hello **kezdési időpont** és **befejező időpontja** szűrők hello felső toomatch hello: start és befejezési időpontja, a folyamat, és kattintson a hello **alkalmaz** gombra.</span><span class="sxs-lookup"><span data-stu-id="58ad7-120">Update hello **start time** and **end time** filters at hello top toomatch hello start and end times of your pipeline, and then click hello **Apply** button.</span></span>  
- <span data-ttu-id="58ad7-121">hello tevékenység Windows lista nem frissül automatikusan.</span><span class="sxs-lookup"><span data-stu-id="58ad7-121">hello Activity Windows list is not automatically refreshed.</span></span> <span data-ttu-id="58ad7-122">Kattintson a hello **frissítése** hello gombjára a hello **tevékenység Windows** lista.</span><span class="sxs-lookup"><span data-stu-id="58ad7-122">Click hello **Refresh** button on hello toolbar in hello **Activity Windows** list.</span></span>  

<span data-ttu-id="58ad7-123">Ha még nem rendelkezik a Data Factory alkalmazás tootest ezeket a lépéseket, hello oktatóanyag: [adatokat másolni a Blob Storage tooSQL adatbázis használata a Data Factory](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="58ad7-123">If you don't have a Data Factory application tootest these steps with, do hello tutorial: [copy data from Blob Storage tooSQL Database using Data Factory](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>

## <a name="understand-hello-monitoring-and-management-app"></a><span data-ttu-id="58ad7-124">Figyelés és a felügyeleti alkalmazás hello ismertetése</span><span class="sxs-lookup"><span data-stu-id="58ad7-124">Understand hello Monitoring and Management app</span></span>
<span data-ttu-id="58ad7-125">Nincsenek hello bal oldali három lappal: **erőforrás-kezelő**, **figyelési nézetei**, és **riasztások**.</span><span class="sxs-lookup"><span data-stu-id="58ad7-125">There are three tabs on hello left: **Resource Explorer**, **Monitoring Views**, and **Alerts**.</span></span> <span data-ttu-id="58ad7-126">hello első lapon (**erőforrás-kezelő**) alapértelmezés szerint engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="58ad7-126">hello first tab (**Resource Explorer**) is selected by default.</span></span>

### <a name="resource-explorer"></a><span data-ttu-id="58ad7-127">Erőforrás-kezelő</span><span class="sxs-lookup"><span data-stu-id="58ad7-127">Resource Explorer</span></span>
<span data-ttu-id="58ad7-128">Hello következő jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="58ad7-128">You see hello following:</span></span>

* <span data-ttu-id="58ad7-129">az erőforrás-kezelő hello **fanézetben** hello bal oldali ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="58ad7-129">hello Resource Explorer **tree view** in hello left pane.</span></span>
* <span data-ttu-id="58ad7-130">Hello **diagramnézet** hello felső hello középső ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="58ad7-130">hello **Diagram View** at hello top in hello middle pane.</span></span>
* <span data-ttu-id="58ad7-131">Hello **tevékenység Windows** lista hello alsó hello középső ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="58ad7-131">hello **Activity Windows** list at hello bottom in hello middle pane.</span></span>
* <span data-ttu-id="58ad7-132">Hello **tulajdonságok**, **tevékenység ablak Explorer**, és **parancsfájl** lapok hello jobb oldali ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="58ad7-132">hello **Properties**, **Activity Window Explorer**, and **Script** tabs in hello right pane.</span></span>

<span data-ttu-id="58ad7-133">Az erőforrás-kezelőben lásd: összes erőforrást (adatcsatornák, adatkészleteket, összekapcsolt szolgáltatások) fanézetben hello adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="58ad7-133">In Resource Explorer, you see all resources (pipelines, datasets, linked services) in hello data factory in a tree view.</span></span> <span data-ttu-id="58ad7-134">Amikor kijelöl egy objektumot az erőforrás-kezelőben:</span><span class="sxs-lookup"><span data-stu-id="58ad7-134">When you select an object in Resource Explorer:</span></span>

* <span data-ttu-id="58ad7-135">hello entitás ki van jelölve, a Diagram nézet hello adat-előállító tartozik.</span><span class="sxs-lookup"><span data-stu-id="58ad7-135">hello associated Data Factory entity is highlighted in hello Diagram View.</span></span>
* <span data-ttu-id="58ad7-136">[Hozzárendelt tevékenység windows](data-factory-scheduling-and-execution.md) hello tevékenység Windows listában hello alsó vannak kiemelve.</span><span class="sxs-lookup"><span data-stu-id="58ad7-136">[Associated activity windows](data-factory-scheduling-and-execution.md) are highlighted in hello Activity Windows list at hello bottom.</span></span>  
* <span data-ttu-id="58ad7-137">hello kijelölt objektum tulajdonságainak hello hello jobb oldali hello tulajdonságai ablakban láthatók.</span><span class="sxs-lookup"><span data-stu-id="58ad7-137">hello properties of hello selected object are shown in hello Properties window in hello right pane.</span></span>
* <span data-ttu-id="58ad7-138">hello JSON-definícióból hello kijelölt objektum jelenik meg, ha van ilyen.</span><span class="sxs-lookup"><span data-stu-id="58ad7-138">hello JSON definition of hello selected object is shown, if applicable.</span></span> <span data-ttu-id="58ad7-139">Például: a társított szolgáltatás, a DataSet adatkészlet vagy folyamat.</span><span class="sxs-lookup"><span data-stu-id="58ad7-139">For example: a linked service, a dataset, or a pipeline.</span></span>

![Erőforrás-kezelő](./media/data-factory-monitor-manage-app/ResourceExplorer.png)

<span data-ttu-id="58ad7-141">Lásd: hello [ütemezés és a végrehajtás](data-factory-scheduling-and-execution.md) cikk tevékenység windows kapcsolatos további részletes információt.</span><span class="sxs-lookup"><span data-stu-id="58ad7-141">See hello [Scheduling and Execution](data-factory-scheduling-and-execution.md) article for detailed conceptual information about activity windows.</span></span>

### <a name="diagram-view"></a><span data-ttu-id="58ad7-142">Diagramnézet</span><span class="sxs-lookup"><span data-stu-id="58ad7-142">Diagram View</span></span>
<span data-ttu-id="58ad7-143">egy adat-előállító Diagramnézetében hello egytáblás, üveghatású toomonitor biztosít, és a data factory és az eszközök kezelése.</span><span class="sxs-lookup"><span data-stu-id="58ad7-143">hello Diagram View of a data factory provides a single pane of glass toomonitor and manage a data factory and its assets.</span></span> <span data-ttu-id="58ad7-144">Ha bejelöli a Data Factory entitás (dataset/pipeline) a Diagram nézet hello:</span><span class="sxs-lookup"><span data-stu-id="58ad7-144">When you select a Data Factory entity (dataset/pipeline) in hello Diagram View:</span></span>

* <span data-ttu-id="58ad7-145">hello data factory entitás hello faszerkezetes nézetben kiválasztott.</span><span class="sxs-lookup"><span data-stu-id="58ad7-145">hello data factory entity is selected in hello tree view.</span></span>
* <span data-ttu-id="58ad7-146">hello hozzárendelt tevékenység windows hello tevékenység Windows listán vannak kiemelve.</span><span class="sxs-lookup"><span data-stu-id="58ad7-146">hello associated activity windows are highlighted in hello Activity Windows list.</span></span>
* <span data-ttu-id="58ad7-147">hello kijelölt objektum tulajdonságainak hello hello tulajdonságai ablakban láthatók.</span><span class="sxs-lookup"><span data-stu-id="58ad7-147">hello properties of hello selected object are shown in hello Properties window.</span></span>

<span data-ttu-id="58ad7-148">Ha hello folyamat engedélyezve van (nem szünetel), a zöld vonallal is látható:</span><span class="sxs-lookup"><span data-stu-id="58ad7-148">When hello pipeline is enabled (not in a paused state), it's shown with a green line:</span></span>

![A folyamat fut](./media/data-factory-monitor-manage-app/PipelineRunning.png)

<span data-ttu-id="58ad7-150">Szüneteltetése, folytatása vagy hello diagram nézetben jelölje ki azt, és hello gombokkal hello parancssávon folyamat leáll.</span><span class="sxs-lookup"><span data-stu-id="58ad7-150">You can pause, resume, or terminate a pipeline by selecting it in hello diagram view and using hello buttons on hello command bar.</span></span>

![/ Szüneteltet hello parancssávon](./media/data-factory-monitor-manage-app/SuspendResumeOnCommandBar.png)
 
<span data-ttu-id="58ad7-152">Nincsenek három gombok hello adatcsatorna a Diagram nézet hello.</span><span class="sxs-lookup"><span data-stu-id="58ad7-152">There are three command bar buttons for hello pipeline in hello Diagram View.</span></span> <span data-ttu-id="58ad7-153">Hello második gomb toopause hello folyamat is használhatja.</span><span class="sxs-lookup"><span data-stu-id="58ad7-153">You can use hello second button toopause hello pipeline.</span></span> <span data-ttu-id="58ad7-154">Felfüggesztése nem a jelenleg futó tevékenységek hello leáll, és lehetővé teszi, hogy folytatni toocompletion.</span><span class="sxs-lookup"><span data-stu-id="58ad7-154">Pausing doesn't terminate hello currently running activities and lets them proceed toocompletion.</span></span> <span data-ttu-id="58ad7-155">hello harmadik gomb hello folyamat megáll, és a meglévő tevékenységek végrehajtása leáll.</span><span class="sxs-lookup"><span data-stu-id="58ad7-155">hello third button pauses hello pipeline and terminates its existing executing activities.</span></span> <span data-ttu-id="58ad7-156">hello első gomb hello folyamat folytatódik.</span><span class="sxs-lookup"><span data-stu-id="58ad7-156">hello first button resumes hello pipeline.</span></span> <span data-ttu-id="58ad7-157">Ha a feldolgozási sor fel van függesztve, hello csővezeték hello színe megváltozik.</span><span class="sxs-lookup"><span data-stu-id="58ad7-157">When your pipeline is paused, hello color of hello pipeline changes.</span></span> <span data-ttu-id="58ad7-158">Például egy felfüggesztett folyamat a következőképpen néz a kép a következő hello:</span><span class="sxs-lookup"><span data-stu-id="58ad7-158">For example, a paused pipeline looks like in hello following image:</span></span> 

![Feldolgozási sor felfüggesztve](./media/data-factory-monitor-manage-app/PipelinePaused.png)

<span data-ttu-id="58ad7-160">Segítségével többszörös kiválasztási két vagy több folyamatok hello Ctrl billentyűt.</span><span class="sxs-lookup"><span data-stu-id="58ad7-160">You can multi-select two or more pipelines by using hello Ctrl key.</span></span> <span data-ttu-id="58ad7-161">Használhat hello parancs sáv gombok toopause/Folytatás több folyamatok egyszerre.</span><span class="sxs-lookup"><span data-stu-id="58ad7-161">You can use hello command bar buttons toopause/resume multiple pipelines at a time.</span></span>

<span data-ttu-id="58ad7-162">Akkor is a jobb gombbal a feldolgozási sorban lévő és válassza a beállítások toosuspend folytatásához, vagy a folyamat leáll.</span><span class="sxs-lookup"><span data-stu-id="58ad7-162">You can also right-click a pipeline and select options toosuspend, resume, or terminate a pipeline.</span></span> 

![Az adatcsatorna helyi menü](./media/data-factory-monitor-manage-app/right-click-menu-for-pipeline.png)

<span data-ttu-id="58ad7-164">Hello kattintson **nyitott folyamat** toosee hello folyamat összes hello tevékenység lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="58ad7-164">Click hello **Open pipeline** option toosee all hello activities in hello pipeline.</span></span> 

![Folyamat megnyitása menü](./media/data-factory-monitor-manage-app/OpenPipelineMenu.png)

<span data-ttu-id="58ad7-166">Megnyitott hello csővezeték nézetben lásd: az összes tevékenység hello folyamat.</span><span class="sxs-lookup"><span data-stu-id="58ad7-166">In hello opened pipeline view, you see all activities in hello pipeline.</span></span> <span data-ttu-id="58ad7-167">Ebben a példában csak egy tevékenység nincs: másolási tevékenység.</span><span class="sxs-lookup"><span data-stu-id="58ad7-167">In this example, there is only one activity: Copy Activity.</span></span> 

![Megnyitott folyamat](./media/data-factory-monitor-manage-app/OpenedPipeline.png)

<span data-ttu-id="58ad7-169">toogo biztonsági toohello előző nézetével, kattintson a hello adat-előállító hello navigációs menüjében hello tetején.</span><span class="sxs-lookup"><span data-stu-id="58ad7-169">toogo back toohello previous view, click hello data factory name in hello breadcrumb menu at hello top.</span></span>

<span data-ttu-id="58ad7-170">Hello csővezeték nézetben amikor kijelöl egy kimeneti adatkészletet, vagy amikor az egér átvitele hello kimeneti adatkészletet, látható hello tevékenység Windows előugró ablak, hogy a DataSet.</span><span class="sxs-lookup"><span data-stu-id="58ad7-170">In hello pipeline view, when you select an output dataset or when you move your mouse over hello output dataset, you see hello Activity Windows pop-up window for that dataset.</span></span>

![Tevékenység Windows előugró ablak](./media/data-factory-monitor-manage-app/ActivityWindowsPopup.png)

<span data-ttu-id="58ad7-172">Egy tevékenység ablak toosee a Részletek gombra ki azt a hello **tulajdonságok** hello jobb oldali ablak.</span><span class="sxs-lookup"><span data-stu-id="58ad7-172">You can click an activity window toosee details for it in hello **Properties** window in hello right pane.</span></span>

![Tevékenység ablak tulajdonságai](./media/data-factory-monitor-manage-app/ActivityWindowProperties.png)

<span data-ttu-id="58ad7-174">Hello jobb oldali ablaktáblában kapcsoló toohello **tevékenység ablak Explorer** toosee további részletek lap.</span><span class="sxs-lookup"><span data-stu-id="58ad7-174">In hello right pane, switch toohello **Activity Window Explorer** tab toosee more details.</span></span>

![Tevékenység ablak Explorer](./media/data-factory-monitor-manage-app/ActivityWindowExplorer.png)

<span data-ttu-id="58ad7-176">Azt is láthatja, **változók feloldva** az egyes futtatására tett kísérlet egy tevékenység hello **kísérletek** szakasz.</span><span class="sxs-lookup"><span data-stu-id="58ad7-176">You also see **resolved variables** for each run attempt for an activity in hello **Attempts** section.</span></span>

![Megoldott változók](./media/data-factory-monitor-manage-app/ResolvedVariables.PNG)

<span data-ttu-id="58ad7-178">Váltás toohello **parancsfájl** toosee hello JSON parancsfájl definíciója hello kijelölt objektum fülre.</span><span class="sxs-lookup"><span data-stu-id="58ad7-178">Switch toohello **Script** tab toosee hello JSON script definition for hello selected object.</span></span>   

![Parancsfájl lap](./media/data-factory-monitor-manage-app/ScriptTab.png)

<span data-ttu-id="58ad7-180">Tevékenység windows három helyen tekintheti meg:</span><span class="sxs-lookup"><span data-stu-id="58ad7-180">You can see activity windows in three places:</span></span>

* <span data-ttu-id="58ad7-181">a Diagram nézet (középső ablaktábla) hello előugró tevékenység Windows hello.</span><span class="sxs-lookup"><span data-stu-id="58ad7-181">hello Activity Windows pop-up in hello Diagram View (middle pane).</span></span>
* <span data-ttu-id="58ad7-182">hello tevékenység ablak Explorer hello jobb oldali ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="58ad7-182">hello Activity Window Explorer in hello right pane.</span></span>
* <span data-ttu-id="58ad7-183">hello tevékenység Windows hello alsó ablaktábla listáján.</span><span class="sxs-lookup"><span data-stu-id="58ad7-183">hello Activity Windows list in hello bottom pane.</span></span>

<span data-ttu-id="58ad7-184">Hello tevékenység Windows előugró üzenet és a tevékenység ablak Explorer görgetve toohello előző hét, és hello segítségével jövő héten hello jobb és bal nyíl.</span><span class="sxs-lookup"><span data-stu-id="58ad7-184">In hello Activity Windows pop-up and Activity Window Explorer, you can scroll toohello previous week and hello next week by using hello left and right arrows.</span></span>

![Tevékenység ablak Explorer balra vagy jobbra nyíl](./media/data-factory-monitor-manage-app/ActivityWindowExplorerLeftRightArrows.png)

<span data-ttu-id="58ad7-186">Hello diagramnézet hello alsó részén, az ilyen gombokat látja: Nagyítás, Kicsinyítés, nagyítás tooFit Nagyítás 100 %-os, Elrendezés zárolása.</span><span class="sxs-lookup"><span data-stu-id="58ad7-186">At hello bottom of hello Diagram View, you see these buttons: Zoom In, Zoom Out, Zoom tooFit, Zoom 100%, Lock layout.</span></span> <span data-ttu-id="58ad7-187">Hello **zárolási elrendezés** gomb megakadályozza a táblák és folyamatok véletlen áthelyezését a hello Diagram nézetben.</span><span class="sxs-lookup"><span data-stu-id="58ad7-187">hello **Lock layout** button prevents you from accidentally moving tables and pipelines in hello Diagram View.</span></span> <span data-ttu-id="58ad7-188">Alapértelmezés szerint le van.</span><span class="sxs-lookup"><span data-stu-id="58ad7-188">It's on by default.</span></span> <span data-ttu-id="58ad7-189">Kapcsolja ki, és entitások Navigálás hello diagramban.</span><span class="sxs-lookup"><span data-stu-id="58ad7-189">You can turn it off and move entities around in hello diagram.</span></span> <span data-ttu-id="58ad7-190">Kapcsolja ki, amikor hello utolsó gomb tooautomatically pozíció táblák és folyamatok is használhatja.</span><span class="sxs-lookup"><span data-stu-id="58ad7-190">When you turn it off, you can use hello last button tooautomatically position tables and pipelines.</span></span> <span data-ttu-id="58ad7-191">Bejövő vagy kimenő hello egérkerék használatával is nagyítás.</span><span class="sxs-lookup"><span data-stu-id="58ad7-191">You can also zoom in or out by using hello mouse wheel.</span></span>

![Diagram nézet Nagyítás parancsok](./media/data-factory-monitor-manage-app/DiagramViewZoomCommands.png)

### <a name="activity-windows-list"></a><span data-ttu-id="58ad7-193">A tevékenységek Windows listája</span><span class="sxs-lookup"><span data-stu-id="58ad7-193">Activity Windows list</span></span>
<span data-ttu-id="58ad7-194">hello tevékenység Windows hello középső ablaktábláján hello alján megjelenik minden tevékenység windows hello adatkészlet hello erőforrás-kezelő vagy hello Diagram nézetben kiválasztott.</span><span class="sxs-lookup"><span data-stu-id="58ad7-194">hello Activity Windows list at hello bottom of hello middle pane displays all activity windows for hello dataset that you selected in hello Resource Explorer or hello Diagram View.</span></span> <span data-ttu-id="58ad7-195">Alapértelmezés szerint hello listát csökkenő sorrendben, ami azt jelenti, hogy megjelenik-e hello legújabb tevékenység ablak hello felső van.</span><span class="sxs-lookup"><span data-stu-id="58ad7-195">By default, hello list is in descending order, which means that you see hello latest activity window at hello top.</span></span>

![A tevékenységek Windows listája](./media/data-factory-monitor-manage-app/ActivityWindowsList.png)

<span data-ttu-id="58ad7-197">Ebben a listában nem automatikusan, frissítse, használjon hello frissítés gomb hello eszköztár toomanually frissíti.</span><span class="sxs-lookup"><span data-stu-id="58ad7-197">This list doesn't refresh automatically, so use hello refresh button on hello toolbar toomanually refresh it.</span></span>  

<span data-ttu-id="58ad7-198">Tevékenység windows hello a következő állapotok valamelyikében lehet:</span><span class="sxs-lookup"><span data-stu-id="58ad7-198">Activity windows can be in one of hello following statuses:</span></span>

<table>
<tr>
    <th align="left"><span data-ttu-id="58ad7-199">status</span><span class="sxs-lookup"><span data-stu-id="58ad7-199">Status</span></span></th><th align="left"><span data-ttu-id="58ad7-200">A részállapot</span><span class="sxs-lookup"><span data-stu-id="58ad7-200">Substatus</span></span></th><th align="left"><span data-ttu-id="58ad7-201">Leírás</span><span class="sxs-lookup"><span data-stu-id="58ad7-201">Description</span></span></th>
</tr>
<tr>
    <td rowspan="8"><span data-ttu-id="58ad7-202">Várakozás</span><span class="sxs-lookup"><span data-stu-id="58ad7-202">Waiting</span></span></td><td><span data-ttu-id="58ad7-203">ScheduleTime</span><span class="sxs-lookup"><span data-stu-id="58ad7-203">ScheduleTime</span></span></td><td><span data-ttu-id="58ad7-204">hello tevékenység ablak toorun hello ideje még nem érkezett.</span><span class="sxs-lookup"><span data-stu-id="58ad7-204">hello time hasn't come for hello activity window toorun.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="58ad7-205">DatasetDependencies</span><span class="sxs-lookup"><span data-stu-id="58ad7-205">DatasetDependencies</span></span></td><td><span data-ttu-id="58ad7-206">hello fölérendelt függőségek nem állnak készen.</span><span class="sxs-lookup"><span data-stu-id="58ad7-206">hello upstream dependencies aren't ready.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="58ad7-207">ComputeResources</span><span class="sxs-lookup"><span data-stu-id="58ad7-207">ComputeResources</span></span></td><td><span data-ttu-id="58ad7-208">hello számítási erőforrások nem érhetők el.</span><span class="sxs-lookup"><span data-stu-id="58ad7-208">hello compute resources aren't available.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="58ad7-209">ConcurrencyLimit</span><span class="sxs-lookup"><span data-stu-id="58ad7-209">ConcurrencyLimit</span></span></td> <td><span data-ttu-id="58ad7-210">Minden hello Tevékenységpéldány futtatásával elfoglalva más tevékenység windows.</span><span class="sxs-lookup"><span data-stu-id="58ad7-210">All hello activity instances are busy running other activity windows.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="58ad7-211">ActivityResume</span><span class="sxs-lookup"><span data-stu-id="58ad7-211">ActivityResume</span></span></td><td><span data-ttu-id="58ad7-212">hello tevékenység szüneteltetve van, és nem futtatható tevékenység windows hello folytatásáig.</span><span class="sxs-lookup"><span data-stu-id="58ad7-212">hello activity is paused and can't run hello activity windows until it's resumed.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="58ad7-213">Próbálja meg újra</span><span class="sxs-lookup"><span data-stu-id="58ad7-213">Retry</span></span></td><td><span data-ttu-id="58ad7-214">hello tevékenység végrehajtási lesz hajtva.</span><span class="sxs-lookup"><span data-stu-id="58ad7-214">hello activity execution is being retried.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="58ad7-215">Ellenőrzés</span><span class="sxs-lookup"><span data-stu-id="58ad7-215">Validation</span></span></td><td><span data-ttu-id="58ad7-216">Érvényesítés még a még nem indult el.</span><span class="sxs-lookup"><span data-stu-id="58ad7-216">Validation hasn't started yet.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="58ad7-217">ValidationRetry</span><span class="sxs-lookup"><span data-stu-id="58ad7-217">ValidationRetry</span></span></td><td><span data-ttu-id="58ad7-218">Érvényesítési újrapróbált várakozási toobe.</span><span class="sxs-lookup"><span data-stu-id="58ad7-218">Validation is waiting toobe retried.</span></span></td>
</tr>
<tr>
<tr>
<td rowspan="2"><span data-ttu-id="58ad7-219">Esetbejegyzések</span><span class="sxs-lookup"><span data-stu-id="58ad7-219">InProgress</span></span></td><td><span data-ttu-id="58ad7-220">Ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="58ad7-220">Validating</span></span></td><td><span data-ttu-id="58ad7-221">Ellenőrzése folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="58ad7-221">Validation is in progress.</span></span></td>
</tr>
<td>-</td>
<td><span data-ttu-id="58ad7-222">hello tevékenység ablakban feldolgozása folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="58ad7-222">hello activity window is being processed.</span></span></td>
</tr>
<tr>
<td rowspan="4"><span data-ttu-id="58ad7-223">Nem sikerült</span><span class="sxs-lookup"><span data-stu-id="58ad7-223">Failed</span></span></td><td><span data-ttu-id="58ad7-224">Időtúllépésbe került</span><span class="sxs-lookup"><span data-stu-id="58ad7-224">TimedOut</span></span></td><td><span data-ttu-id="58ad7-225">hello tevékenység végrehajtási hello tevékenység által megengedett érték időt vett igénybe.</span><span class="sxs-lookup"><span data-stu-id="58ad7-225">hello activity execution took longer than what is allowed by hello activity.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="58ad7-226">Törölve</span><span class="sxs-lookup"><span data-stu-id="58ad7-226">Canceled</span></span></td><td><span data-ttu-id="58ad7-227">hello tevékenység ablakban felhasználói művelet megszakította.</span><span class="sxs-lookup"><span data-stu-id="58ad7-227">hello activity window was canceled by user action.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="58ad7-228">Ellenőrzés</span><span class="sxs-lookup"><span data-stu-id="58ad7-228">Validation</span></span></td><td><span data-ttu-id="58ad7-229">Sikertelen volt.</span><span class="sxs-lookup"><span data-stu-id="58ad7-229">Validation has failed.</span></span></td>
</tr>
<tr>
<td>-</td><td><span data-ttu-id="58ad7-230">hello tevékenység ablakban toobe jön létre, vagy ha ellenőrizni nem sikerült.</span><span class="sxs-lookup"><span data-stu-id="58ad7-230">hello activity window failed toobe generated or validated.</span></span></td>
</tr>
<td><span data-ttu-id="58ad7-231">Készen áll</span><span class="sxs-lookup"><span data-stu-id="58ad7-231">Ready</span></span></td><td>-</td><td><span data-ttu-id="58ad7-232">hello tevékenység ablakban készen áll a felhasználásra.</span><span class="sxs-lookup"><span data-stu-id="58ad7-232">hello activity window is ready for consumption.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="58ad7-233">Kihagyva</span><span class="sxs-lookup"><span data-stu-id="58ad7-233">Skipped</span></span></td><td>-</td><td><span data-ttu-id="58ad7-234">hello tevékenység ablak nem lett feldolgozva.</span><span class="sxs-lookup"><span data-stu-id="58ad7-234">hello activity window wasn't processed.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="58ad7-235">None</span><span class="sxs-lookup"><span data-stu-id="58ad7-235">None</span></span></td><td>-</td><td><span data-ttu-id="58ad7-236">Egy tevékenység ablakban tooexist használja egy eltérő állapottal, de alaphelyzetbe lett állítva.</span><span class="sxs-lookup"><span data-stu-id="58ad7-236">An activity window used tooexist with a different status, but has been reset.</span></span></td>
</tr>
</table>


<span data-ttu-id="58ad7-237">Ha egy tevékenység ablakban hello listában gombra kattint, megjelenik az részleteit a hello **tevékenység Windows Explorer** vagy hello **tulajdonságok** hello jobb oldali ablak.</span><span class="sxs-lookup"><span data-stu-id="58ad7-237">When you click an activity window in hello list, you see details about it in hello **Activity Windows Explorer** or hello **Properties** window on hello right.</span></span>

![Tevékenység ablak Explorer](./media/data-factory-monitor-manage-app/ActivityWindowExplorer-2.png)

### <a name="refresh-activity-windows"></a><span data-ttu-id="58ad7-239">Tevékenység windows frissítése</span><span class="sxs-lookup"><span data-stu-id="58ad7-239">Refresh activity windows</span></span>
<span data-ttu-id="58ad7-240">hello részletek nem automatikusan frissülnek, ezért hello frissítési (gomb hello második) a hello parancssávon toomanually frissítési hello tevékenységek windows listája.</span><span class="sxs-lookup"><span data-stu-id="58ad7-240">hello details aren't automatically refreshed, so use hello refresh button (hello second button) on hello command bar toomanually refresh hello activity windows list.</span></span>  

### <a name="properties-window"></a><span data-ttu-id="58ad7-241">Tulajdonságok ablak</span><span class="sxs-lookup"><span data-stu-id="58ad7-241">Properties window</span></span>
<span data-ttu-id="58ad7-242">hello tulajdonságai ablakban hello figyelés és a felügyeleti alkalmazás hello jobb szélső panelén megtalálható.</span><span class="sxs-lookup"><span data-stu-id="58ad7-242">hello Properties window is in hello right-most pane of hello Monitoring and Management app.</span></span>

![Tulajdonságok ablak](./media/data-factory-monitor-manage-app/PropertiesWindow.png)

<span data-ttu-id="58ad7-244">Megjeleníti az erőforrás-kezelő (fanézetben) hello kiválasztott hello elem tulajdonságai Diagram nézetet, vagy a tevékenység Windows listája.</span><span class="sxs-lookup"><span data-stu-id="58ad7-244">It displays properties for hello item that you selected in hello Resource Explorer (tree view), Diagram View, or Activity Windows list.</span></span>

### <a name="activity-window-explorer"></a><span data-ttu-id="58ad7-245">Tevékenység ablak Explorer</span><span class="sxs-lookup"><span data-stu-id="58ad7-245">Activity Window Explorer</span></span>
<span data-ttu-id="58ad7-246">Hello **tevékenység ablak Explorer** időszak van hello figyelés és a felügyeleti alkalmazás hello jobb szélső panelén.</span><span class="sxs-lookup"><span data-stu-id="58ad7-246">hello **Activity Window Explorer** window is in hello right-most pane of hello Monitoring and Management app.</span></span> <span data-ttu-id="58ad7-247">Hello tevékenység ablakban hello tevékenység Windows előugró ablak vagy hello tevékenység Windows listán kijelölt részleteit jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="58ad7-247">It displays details about hello activity window that you selected in hello Activity Windows pop-up window or hello Activity Windows list.</span></span>

![Tevékenység ablak Explorer](./media/data-factory-monitor-manage-app/ActivityWindowExplorer-3.png)

<span data-ttu-id="58ad7-249">Tooanother tevékenység ablakban hello naptár nézetben hello felső kattintva válthat.</span><span class="sxs-lookup"><span data-stu-id="58ad7-249">You can switch tooanother activity window by clicking it in hello calendar view at hello top.</span></span> <span data-ttu-id="58ad7-250">Hello/jobbra bal oldali gomb használatának hello felső toosee tevékenység windows hello az előző hét vagy jövő héten hello is.</span><span class="sxs-lookup"><span data-stu-id="58ad7-250">You can also use hello left arrow/right arrow buttons at hello top toosee activity windows from hello previous week or hello next week.</span></span>

<span data-ttu-id="58ad7-251">Hello eszköztár gombjaival hello alsó ablaktáblán toorerun hello tevékenység ablakban, vagy hello részletek hello ablaktáblán frissítése.</span><span class="sxs-lookup"><span data-stu-id="58ad7-251">You can use hello toolbar buttons in hello bottom pane toorerun hello activity window or refresh hello details in hello pane.</span></span>

### <a name="script"></a><span data-ttu-id="58ad7-252">Szkript</span><span class="sxs-lookup"><span data-stu-id="58ad7-252">Script</span></span>
<span data-ttu-id="58ad7-253">Használhatja a hello **parancsfájl** lapon tooview hello JSON-definícióból hello a kiválasztott adat-előállító entitás (társított szolgáltatás, adatkészlet vagy csővezeték).</span><span class="sxs-lookup"><span data-stu-id="58ad7-253">You can use hello **Script** tab tooview hello JSON definition of hello selected Data Factory entity (linked service, dataset, or pipeline).</span></span>

![Parancsfájl lap](./media/data-factory-monitor-manage-app/ScriptTab.png)

## <a name="use-system-views"></a><span data-ttu-id="58ad7-255">Rendszer-nézetek</span><span class="sxs-lookup"><span data-stu-id="58ad7-255">Use system views</span></span>
<span data-ttu-id="58ad7-256">hello figyelés és a felügyeleti alkalmazás tartalmaz előre elkészített rendszernézetek (**legutóbbi tevékenységek windows**, **sikertelen volt a tevékenység windows**, **folyamatban lévő tevékenységek windows**), lehetővé teszi a data factory tooview legutóbbi/nem sikerült/folyamatban lévő tevékenységek időszakokat.</span><span class="sxs-lookup"><span data-stu-id="58ad7-256">hello Monitoring and Management app includes pre-built system views (**Recent activity windows**, **Failed activity windows**, **In-Progress activity windows**) that allow you tooview recent/failed/in-progress activity windows for your data factory.</span></span>

<span data-ttu-id="58ad7-257">Váltás toohello **figyelési nézetei** fülre kattintva hello bal oldalon.</span><span class="sxs-lookup"><span data-stu-id="58ad7-257">Switch toohello **Monitoring Views** tab on hello left by clicking it.</span></span>

![Figyelési nézetek lap](./media/data-factory-monitor-manage-app/MonitoringViewsTab.png)

<span data-ttu-id="58ad7-259">Jelenleg nincsenek három rendszernézetek támogatott.</span><span class="sxs-lookup"><span data-stu-id="58ad7-259">Currently, there are three system views that are supported.</span></span> <span data-ttu-id="58ad7-260">Válasszon egy lehetőséget toosee legutóbbi tevékenységek windows, a sikertelen tevékenységet windows vagy a folyamatban lévő tevékenységek windows hello tevékenység Windows listában (alján hello hello középső ablaktábla).</span><span class="sxs-lookup"><span data-stu-id="58ad7-260">Select an option toosee recent activity windows, failed activity windows, or in-progress activity windows in hello Activity Windows list (at hello bottom of hello middle pane).</span></span>

<span data-ttu-id="58ad7-261">Ha bejelöli hello **legutóbbi tevékenységek windows** beállítást, megjelenik minden legutóbbi tevékenységek windows hello csökkenő **utolsó kísérlet ideje**.</span><span class="sxs-lookup"><span data-stu-id="58ad7-261">When you select hello **Recent activity windows** option, you see all recent activity windows in descending order of hello **last attempt time**.</span></span>

<span data-ttu-id="58ad7-262">Használhatja a hello **sikertelen volt a tevékenység windows** megtekintéséhez toosee összes sikertelen tevékenység windows hello listáján.</span><span class="sxs-lookup"><span data-stu-id="58ad7-262">You can use hello **Failed activity windows** view toosee all failed activity windows in hello list.</span></span> <span data-ttu-id="58ad7-263">Válassza ki a sikertelen tevékenységek ablakát hello lista toosee részleteit a hello **tulajdonságok** ablak vagy hello **tevékenység ablak Explorer**.</span><span class="sxs-lookup"><span data-stu-id="58ad7-263">Select a failed activity window in hello list toosee details about it in hello **Properties** window or hello **Activity Window Explorer**.</span></span> <span data-ttu-id="58ad7-264">A naplók a sikertelen tevékenységet időszak is letöltheti.</span><span class="sxs-lookup"><span data-stu-id="58ad7-264">You can also download any logs for a failed activity window.</span></span>

## <a name="sort-and-filter-activity-windows"></a><span data-ttu-id="58ad7-265">Rendezésére és szűrésére tevékenység windows</span><span class="sxs-lookup"><span data-stu-id="58ad7-265">Sort and filter activity windows</span></span>
<span data-ttu-id="58ad7-266">Változás hello **kezdési időpont** és **befejező időpontja** toofilter tevékenység windows hello parancssávon beállításait.</span><span class="sxs-lookup"><span data-stu-id="58ad7-266">Change hello **start time** and **end time** settings in hello command bar toofilter activity windows.</span></span> <span data-ttu-id="58ad7-267">Hello kezdési és befejezési időpontjának módosítása után kattintson hello gomb következő toohello befejezési idő toorefresh hello tevékenység Windows listára.</span><span class="sxs-lookup"><span data-stu-id="58ad7-267">After you change hello start time and end time, click hello button next toohello end time toorefresh hello Activity Windows list.</span></span>

![Kezdő és befejező időpontja](./media/data-factory-monitor-manage-app/StartAndEndTimes.png)

> [!NOTE]
> <span data-ttu-id="58ad7-269">Minden alkalommal jelenleg hello figyelés és a felügyeleti alkalmazás UTC formátumban.</span><span class="sxs-lookup"><span data-stu-id="58ad7-269">Currently, all times are in UTC format in hello Monitoring and Management app.</span></span>
>
>

<span data-ttu-id="58ad7-270">A hello **tevékenység Windows lista**, kattintson egy hello neve (például: állapot).</span><span class="sxs-lookup"><span data-stu-id="58ad7-270">In hello **Activity Windows list**, click hello name of a column (for example: Status).</span></span>

![Tevékenység Windows lista oszlop menü](./media/data-factory-monitor-manage-app/ActivityWindowsListColumnMenu.png)

<span data-ttu-id="58ad7-272">Mindent hello következő:</span><span class="sxs-lookup"><span data-stu-id="58ad7-272">You can do hello following:</span></span>

* <span data-ttu-id="58ad7-273">Rendezés növekvő sorrendben.</span><span class="sxs-lookup"><span data-stu-id="58ad7-273">Sort in ascending order.</span></span>
* <span data-ttu-id="58ad7-274">Rendezés csökkenő sorrendben.</span><span class="sxs-lookup"><span data-stu-id="58ad7-274">Sort in descending order.</span></span>
* <span data-ttu-id="58ad7-275">Szűrés egy vagy több (kész, Várakozás, és így tovább).</span><span class="sxs-lookup"><span data-stu-id="58ad7-275">Filter by one or more values (Ready, Waiting, and so on).</span></span>

<span data-ttu-id="58ad7-276">Ha szűrőt ad meg egy olyan oszlop, lásd: engedélyezve van az adott oszlop, amely azt jelzi, hogy hello hello oszlopban szereplő értékek szűrt értékek hello a szűrő gombra.</span><span class="sxs-lookup"><span data-stu-id="58ad7-276">When you specify a filter on a column, you see hello filter button enabled for that column, which indicates that hello values in hello column are filtered values.</span></span>

![Egy oszlop hello tevékenység Windows lista alapján szűrés](./media/data-factory-monitor-manage-app/ActivityWindowsListFilterInColumn.png)

<span data-ttu-id="58ad7-278">Használhatja ugyanazt az előugró ablakban tooclear szűrők hello.</span><span class="sxs-lookup"><span data-stu-id="58ad7-278">You can use hello same pop-up window tooclear filters.</span></span> <span data-ttu-id="58ad7-279">Minden tevékenység Windows hello listát szűrők tooclear hello törlése gomb hello parancssávon kattintson.</span><span class="sxs-lookup"><span data-stu-id="58ad7-279">tooclear all filters for hello Activity Windows list, click hello clear filter button on hello command bar.</span></span>

![Hello tevékenység Windows lista az összes szűrő törlése](./media/data-factory-monitor-manage-app/ClearAllFiltersActivityWindowsList.png)

## <a name="perform-batch-actions"></a><span data-ttu-id="58ad7-281">Kötegelt műveleteket</span><span class="sxs-lookup"><span data-stu-id="58ad7-281">Perform batch actions</span></span>
### <a name="rerun-selected-activity-windows"></a><span data-ttu-id="58ad7-282">Futtassa újra a kijelölt tevékenység windows</span><span class="sxs-lookup"><span data-stu-id="58ad7-282">Rerun selected activity windows</span></span>
<span data-ttu-id="58ad7-283">Egy tevékenység ablak, kattintson a lefelé mutató nyíl hello első sáv parancsgomb hello válassza ki és **újrafuttatása** / **futtassa újra a előtt folyamat**.</span><span class="sxs-lookup"><span data-stu-id="58ad7-283">Select an activity window, click hello down arrow for hello first command bar button, and select **Rerun** / **Rerun with upstream in pipeline**.</span></span> <span data-ttu-id="58ad7-284">Hello kiválasztásakor **futtassa újra a előtt folyamat** beállítás, az összes felsőbb szintű tevékenység windows is Újrafuttatja.</span><span class="sxs-lookup"><span data-stu-id="58ad7-284">When you select hello **Rerun with upstream in pipeline** option, it reruns all upstream activity windows as well.</span></span>
    <span data-ttu-id="58ad7-285">![Futtassa újra a műveletet egy tevékenység ablak](./media/data-factory-monitor-manage-app/ReRunSlice.png)</span><span class="sxs-lookup"><span data-stu-id="58ad7-285">![Rerun an activity window](./media/data-factory-monitor-manage-app/ReRunSlice.png)</span></span>

<span data-ttu-id="58ad7-286">Is több tevékenység windows hello listában jelölje ki, és futtatnia őket: hello ugyanannyi időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="58ad7-286">You can also select multiple activity windows in hello list and rerun them at hello same time.</span></span> <span data-ttu-id="58ad7-287">Érdemes lehet toofilter tevékenység windows hello állapota alapján (például: **sikertelen**) –, majd futtassa újra a sikertelen hello tevékenység windows hello tevékenység windows toofail okozó hello a probléma kijavítása után.</span><span class="sxs-lookup"><span data-stu-id="58ad7-287">You might want toofilter activity windows based on hello status (for example: **Failed**)--and then rerun hello failed activity windows after correcting hello issue that causes hello activity windows toofail.</span></span> <span data-ttu-id="58ad7-288">Tekintse meg a következő tevékenység windows hello listában szűrése szakaszát hello.</span><span class="sxs-lookup"><span data-stu-id="58ad7-288">See hello following section for details about filtering activity windows in hello list.</span></span>  

### <a name="pauseresume-multiple-pipelines"></a><span data-ttu-id="58ad7-289">Több folyamatok szüneteltet</span><span class="sxs-lookup"><span data-stu-id="58ad7-289">Pause/resume multiple pipelines</span></span>
<span data-ttu-id="58ad7-290">Segítségével multiselect két vagy több folyamatok hello Ctrl billentyűt.</span><span class="sxs-lookup"><span data-stu-id="58ad7-290">You can multiselect two or more pipelines by using hello Ctrl key.</span></span> <span data-ttu-id="58ad7-291">Hello gombok (amely a következő kép hello hello piros téglalap kijelölt) is használhat toopause/Folytatás őket.</span><span class="sxs-lookup"><span data-stu-id="58ad7-291">You can use hello command bar buttons (which are highlighted in hello red rectangle in hello following image) toopause/resume them.</span></span>

![/ Szüneteltet hello parancssávon](./media/data-factory-monitor-manage-app/SuspendResumeOnCommandBar.png)

## <a name="create-alerts"></a><span data-ttu-id="58ad7-293">Riasztások létrehozása</span><span class="sxs-lookup"><span data-stu-id="58ad7-293">Create alerts</span></span>
<span data-ttu-id="58ad7-294">Hello **riasztások** lap lehetővé teszi, hogy hozzon létre riasztást és nézet/Szerkesztés/törlés meglévő riasztást.</span><span class="sxs-lookup"><span data-stu-id="58ad7-294">hello **Alerts** page lets you create an alert and view/edit/delete existing alerts.</span></span> <span data-ttu-id="58ad7-295">Akkor is is tiltása/engedélyezése egy riasztást.</span><span class="sxs-lookup"><span data-stu-id="58ad7-295">You can also disable/enable an alert.</span></span> <span data-ttu-id="58ad7-296">toosee hello riasztások lapján, kattintson a hello **riasztások** fülre.</span><span class="sxs-lookup"><span data-stu-id="58ad7-296">toosee hello Alerts page, click hello **Alerts** tab.</span></span>

![Riasztások lap](./media/data-factory-monitor-manage-app/AlertsTab.png)

### <a name="toocreate-an-alert"></a><span data-ttu-id="58ad7-298">toocreate riasztás</span><span class="sxs-lookup"><span data-stu-id="58ad7-298">toocreate an alert</span></span>
1. <span data-ttu-id="58ad7-299">Kattintson a **hozzáadása riasztás** tooadd riasztást.</span><span class="sxs-lookup"><span data-stu-id="58ad7-299">Click **Add Alert** tooadd an alert.</span></span> <span data-ttu-id="58ad7-300">Megjelenik a hello **részletek** lap.</span><span class="sxs-lookup"><span data-stu-id="58ad7-300">You see hello **Details** page.</span></span>

    ![Riasztások – Részletek lap létrehozása](./media/data-factory-monitor-manage-app/CreateAlertDetailsPage.png)
2. <span data-ttu-id="58ad7-302">Adja meg a hello **neve** és **leírás** hello riasztást, majd kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="58ad7-302">Specify hello **Name** and **Description** for hello alert, and click **Next**.</span></span> <span data-ttu-id="58ad7-303">Megtekintheti az hello **szűrők** lap.</span><span class="sxs-lookup"><span data-stu-id="58ad7-303">You should see hello **Filters** page.</span></span>

    ![Riasztások – szűrők lap létrehozása](./media/data-factory-monitor-manage-app/CreateAlertFiltersPage.png)
3. <span data-ttu-id="58ad7-305">Jelölje be hello **esemény**, **állapot**, és **részállapot** (nem kötelező), amelyet az toocreate a Data Factory szolgáltatásnak a riasztásra, majd kattintson **tovább**.</span><span class="sxs-lookup"><span data-stu-id="58ad7-305">Select hello **event**, **status**, and **substatus** (optional) that you want toocreate a Data Factory service alert for, and click **Next**.</span></span> <span data-ttu-id="58ad7-306">Megtekintheti az hello **címzettek** lap.</span><span class="sxs-lookup"><span data-stu-id="58ad7-306">You should see hello **Recipients** page.</span></span>

    ![Riasztások – címzettek lap létrehozása](./media/data-factory-monitor-manage-app/CreateAlertRecipientsPage.png)
4. <span data-ttu-id="58ad7-308">SELECT hello **E-mail-előfizetés rendszergazdái** lehetőséget és/vagy adjon meg egy **további rendszergazdai e-mail**, és kattintson a **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="58ad7-308">Select hello **Email subscription admins** option and/or enter an **additional administrator email**, and click **Finish**.</span></span> <span data-ttu-id="58ad7-309">Hello riasztás hello listában kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="58ad7-309">You should see hello alert in hello list.</span></span>

    ![Riasztások listája](./media/data-factory-monitor-manage-app/AlertsList.png)

<span data-ttu-id="58ad7-311">Hello riasztáslistában gombokkal hello társított hello riasztási tooedit/Törlés/tiltása/engedélyezése egy riasztást.</span><span class="sxs-lookup"><span data-stu-id="58ad7-311">In hello Alerts list, use hello buttons that are associated with hello alert tooedit/delete/disable/enable an alert.</span></span>

### <a name="eventstatussubstatus"></a><span data-ttu-id="58ad7-312">A részállapot/esemény/állapota</span><span class="sxs-lookup"><span data-stu-id="58ad7-312">Event/status/substatus</span></span>
<span data-ttu-id="58ad7-313">hello következő táblázat elérhető eseményeket és állapotokat (és részállapotok) hello listája.</span><span class="sxs-lookup"><span data-stu-id="58ad7-313">hello following table provides hello list of available events and statuses (and substatuses).</span></span>

| <span data-ttu-id="58ad7-314">esemény neve</span><span class="sxs-lookup"><span data-stu-id="58ad7-314">Event name</span></span> | <span data-ttu-id="58ad7-315">status</span><span class="sxs-lookup"><span data-stu-id="58ad7-315">Status</span></span> | <span data-ttu-id="58ad7-316">A részállapot</span><span class="sxs-lookup"><span data-stu-id="58ad7-316">Substatus</span></span> |
| --- | --- | --- |
| <span data-ttu-id="58ad7-317">A tevékenység futtatása megkezdődött</span><span class="sxs-lookup"><span data-stu-id="58ad7-317">Activity Run Started</span></span> |<span data-ttu-id="58ad7-318">Megkezdődött</span><span class="sxs-lookup"><span data-stu-id="58ad7-318">Started</span></span> |<span data-ttu-id="58ad7-319">Indulás alatt</span><span class="sxs-lookup"><span data-stu-id="58ad7-319">Starting</span></span> |
| <span data-ttu-id="58ad7-320">A tevékenység futtatása befejeződött</span><span class="sxs-lookup"><span data-stu-id="58ad7-320">Activity Run Finished</span></span> |<span data-ttu-id="58ad7-321">Sikeres</span><span class="sxs-lookup"><span data-stu-id="58ad7-321">Succeeded</span></span> |<span data-ttu-id="58ad7-322">Sikeres</span><span class="sxs-lookup"><span data-stu-id="58ad7-322">Succeeded</span></span> |
| <span data-ttu-id="58ad7-323">A tevékenység futtatása befejeződött</span><span class="sxs-lookup"><span data-stu-id="58ad7-323">Activity Run Finished</span></span> |<span data-ttu-id="58ad7-324">Nem sikerült</span><span class="sxs-lookup"><span data-stu-id="58ad7-324">Failed</span></span> |<span data-ttu-id="58ad7-325">Nem sikerült erőforrás-elosztás</span><span class="sxs-lookup"><span data-stu-id="58ad7-325">Failed Resource Allocation</span></span><br/><br/><span data-ttu-id="58ad7-326">Sikertelen végrehajtása</span><span class="sxs-lookup"><span data-stu-id="58ad7-326">Failed Execution</span></span><br/><br/><span data-ttu-id="58ad7-327">Túllépte az időkorlátot</span><span class="sxs-lookup"><span data-stu-id="58ad7-327">Timed Out</span></span><br/><br/><span data-ttu-id="58ad7-328">Érvényesítés</span><span class="sxs-lookup"><span data-stu-id="58ad7-328">Failed Validation</span></span><br/><br/><span data-ttu-id="58ad7-329">Elhagyott</span><span class="sxs-lookup"><span data-stu-id="58ad7-329">Abandoned</span></span> |
| <span data-ttu-id="58ad7-330">Igény szerinti HDI-fürtöt létrehozni elindítva</span><span class="sxs-lookup"><span data-stu-id="58ad7-330">On-Demand HDI Cluster Create Started</span></span> |<span data-ttu-id="58ad7-331">Megkezdődött</span><span class="sxs-lookup"><span data-stu-id="58ad7-331">Started</span></span> |-|
| <span data-ttu-id="58ad7-332">Igény szerinti HDI-fürtnek sikeresen létrehozva</span><span class="sxs-lookup"><span data-stu-id="58ad7-332">On-Demand HDI Cluster Created Successfully</span></span> |<span data-ttu-id="58ad7-333">Sikeres</span><span class="sxs-lookup"><span data-stu-id="58ad7-333">Succeeded</span></span> |-|
| <span data-ttu-id="58ad7-334">Igény szerinti HDI-fürtnek törlése</span><span class="sxs-lookup"><span data-stu-id="58ad7-334">On-Demand HDI Cluster Deleted</span></span> |<span data-ttu-id="58ad7-335">Sikeres</span><span class="sxs-lookup"><span data-stu-id="58ad7-335">Succeeded</span></span> |-|

### <a name="tooedit-delete-or-disable-an-alert"></a><span data-ttu-id="58ad7-336">tooedit, törlése, vagy tiltsa le a riasztás</span><span class="sxs-lookup"><span data-stu-id="58ad7-336">tooedit, delete, or disable an alert</span></span>

<span data-ttu-id="58ad7-337">Használja a (pirossal kiemelt) gombok tooedit, delete vagy tiltsa le a riasztás a következő hello.</span><span class="sxs-lookup"><span data-stu-id="58ad7-337">Use hello following buttons (highlighted in red) tooedit, delete, or disable an alert.</span></span>

![Riasztások gombok](./media/data-factory-monitor-manage-app/AlertButtons.png)
