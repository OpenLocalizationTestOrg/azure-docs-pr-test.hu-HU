---
title: "Figyelheti és folyamatok kezelése az Azure portál és a PowerShell használatával |} Microsoft Docs"
description: "Útmutató az Azure portál és az Azure PowerShell használatával az Azure adat-előállítók és a folyamatok létrehozott felügyeletét és kezelését."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 9b0fdc59-5bbe-44d1-9ebc-8be14d44def9
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: spelluru
ms.openlocfilehash: 61bb5379cd94dd00814e14420947e7783999ff0a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="monitor-and-manage-azure-data-factory-pipelines-by-using-the-azure-portal-and-powershell"></a><span data-ttu-id="67368-103">Figyelheti és kezelheti az Azure Data Factory-folyamatok az Azure portál és a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="67368-103">Monitor and manage Azure Data Factory pipelines by using the Azure portal and PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="67368-104">Az Azure portálon vagy az Azure PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="67368-104">Using Azure portal/Azure PowerShell</span></span>](data-factory-monitor-manage-pipelines.md)
> * [<span data-ttu-id="67368-105">Használatával figyelése és a felügyeleti alkalmazás</span><span class="sxs-lookup"><span data-stu-id="67368-105">Using Monitoring and Management app</span></span>](data-factory-monitor-manage-app.md)


> [!IMPORTANT]
> <span data-ttu-id="67368-106">A figyelés a & felügyeleti alkalmazás figyelése és az adatok folyamatok kezelését és hibaelhárítását probléma merül fel egy jobb támogatást biztosít.</span><span class="sxs-lookup"><span data-stu-id="67368-106">The monitoring & management application provides a better support for monitoring and managing your data pipelines, and troubleshooting any issues.</span></span> <span data-ttu-id="67368-107">Az alkalmazás használatával kapcsolatos részletekért lásd: [felügyeletéhez és kezeléséhez az adat-előállító adatcsatornák a figyelés és felügyelet alkalmazással](data-factory-monitor-manage-app.md).</span><span class="sxs-lookup"><span data-stu-id="67368-107">For details about using the application, see [monitor and manage Data Factory pipelines by using the Monitoring and Management app](data-factory-monitor-manage-app.md).</span></span> 


<span data-ttu-id="67368-108">Ez a cikk ismerteti, hogyan figyeléséhez, kezeléséhez és a folyamatok hibakeresése az Azure-portál és a PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="67368-108">This article describes how to monitor, manage, and debug your pipelines by using Azure portal and PowerShell.</span></span> <span data-ttu-id="67368-109">A cikk létre riasztásokat, és értesítést kaphat a hibákkal kapcsolatos információkat is biztosít.</span><span class="sxs-lookup"><span data-stu-id="67368-109">The article also provides information on how to create alerts and get notified about failures.</span></span>

## <a name="understand-pipelines-and-activity-states"></a><span data-ttu-id="67368-110">Adatcsatornák és a tevékenység állapota</span><span class="sxs-lookup"><span data-stu-id="67368-110">Understand pipelines and activity states</span></span>
<span data-ttu-id="67368-111">Az Azure portál használatával a következő műveletek végezhetők el:</span><span class="sxs-lookup"><span data-stu-id="67368-111">By using the Azure portal, you can:</span></span>

* <span data-ttu-id="67368-112">Megtekintheti a data factory mint ábrázoló diagram.</span><span class="sxs-lookup"><span data-stu-id="67368-112">View your data factory as a diagram.</span></span>
* <span data-ttu-id="67368-113">A feldolgozási soros tevékenységek megtekintése.</span><span class="sxs-lookup"><span data-stu-id="67368-113">View activities in a pipeline.</span></span>
* <span data-ttu-id="67368-114">Bemeneti és kimeneti adatkészletek megtekintése.</span><span class="sxs-lookup"><span data-stu-id="67368-114">View input and output datasets.</span></span>

<span data-ttu-id="67368-115">Ez a szakasz azt is ismerteti, hogyan dataset szelet átkerül egy állapotból egy másik állapotba.</span><span class="sxs-lookup"><span data-stu-id="67368-115">This section also describes how a dataset slice transitions from one state to another state.</span></span>   

### <a name="navigate-to-your-data-factory"></a><span data-ttu-id="67368-116">Keresse meg a data factory</span><span class="sxs-lookup"><span data-stu-id="67368-116">Navigate to your data factory</span></span>
1. <span data-ttu-id="67368-117">Jelentkezzen be az [Azure Portalra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="67368-117">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="67368-118">Kattintson a **adat-előállítók** a bal oldali menüben.</span><span class="sxs-lookup"><span data-stu-id="67368-118">Click **Data factories** on the menu on the left.</span></span> <span data-ttu-id="67368-119">Ha nem látja, kattintson a **további szolgáltatások >**, és kattintson a **adat-előállítók** alatt a **ESZKÖZINTELLIGENCIA + ANALITIKA** kategóriát.</span><span class="sxs-lookup"><span data-stu-id="67368-119">If you don't see it, click **More services >**, and then click **Data factories** under the **INTELLIGENCE + ANALYTICS** category.</span></span>

   ![Keresse meg az összes > adat-előállítók](./media/data-factory-monitor-manage-pipelines/browseall-data-factories.png)
3. <span data-ttu-id="67368-121">Az a **adat-előállítók** panelen válassza ki az adat-előállító kíváncsiak vagyunk.</span><span class="sxs-lookup"><span data-stu-id="67368-121">On the **Data factories** blade, select the data factory that you're interested in.</span></span>

    ![Válassza ki az adat-előállító](./media/data-factory-monitor-manage-pipelines/select-data-factory.png)

   <span data-ttu-id="67368-123">A data factory kezdőlapjának kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="67368-123">You should see the home page for the data factory.</span></span>

   ![Data factory panel](./media/data-factory-monitor-manage-pipelines/data-factory-blade.png)

#### <a name="diagram-view-of-your-data-factory"></a><span data-ttu-id="67368-125">A data factory a diagram nézet</span><span class="sxs-lookup"><span data-stu-id="67368-125">Diagram view of your data factory</span></span>
<span data-ttu-id="67368-126">A **Diagram** egy adat-előállító nézete egytáblás üveg felügyeletéhez és a data factory és az eszközök kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="67368-126">The **Diagram** view of a data factory provides a single pane of glass to monitor and manage the data factory and its assets.</span></span> <span data-ttu-id="67368-127">Megtekintéséhez a **Diagram** , a data factory megtekintéséhez kattintson az **Diagram** a kezdőlapon az adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="67368-127">To see the **Diagram** view of your data factory, click **Diagram** on the home page for the data factory.</span></span>

![Diagramnézet](./media/data-factory-monitor-manage-pipelines/diagram-view.png)

<span data-ttu-id="67368-129">Nagyítás, Kicsinyítés, nagyítás elférjen, 100 %-os nagyítás, a diagram elrendezését zárolása és adatcsatornákat és adathalmazokat automatikus formázása.</span><span class="sxs-lookup"><span data-stu-id="67368-129">You can zoom in, zoom out, zoom to fit, zoom to 100%, lock the layout of the diagram, and automatically position pipelines and datasets.</span></span> <span data-ttu-id="67368-130">Megtekintheti az Leszármaztatás adatait is (Ez azt jelenti, hogy a kijelölt elemek felsőbb és alsóbb rétegbeli elemek megjelenítése).</span><span class="sxs-lookup"><span data-stu-id="67368-130">You can also see the data lineage information (that is, show upstream and downstream items of selected items).</span></span>

### <a name="activities-inside-a-pipeline"></a><span data-ttu-id="67368-131">Egy folyamat belüli tevékenységek</span><span class="sxs-lookup"><span data-stu-id="67368-131">Activities inside a pipeline</span></span>
1. <span data-ttu-id="67368-132">Kattintson a jobb gombbal a folyamatot, és kattintson **nyitott folyamat** a sorban, a tevékenységekhez tartozó bemeneti és kimeneti adatkészletek együtt az összes tevékenység megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="67368-132">Right-click the pipeline, and then click **Open pipeline** to see all activities in the pipeline, along with input and output datasets for the activities.</span></span> <span data-ttu-id="67368-133">Ez a szolgáltatás akkor hasznos, ha a feldolgozási sor egynél több tevékenységet tartalmaz, és szeretné tudni, hogy a működési Leszármaztatás egyetlen folyamat.</span><span class="sxs-lookup"><span data-stu-id="67368-133">This feature is useful when your pipeline includes more than one activity and you want to understand the operational lineage of a single pipeline.</span></span>

    ![Folyamat megnyitása menü](./media/data-factory-monitor-manage-pipelines/open-pipeline-menu.png)     
2. <span data-ttu-id="67368-135">Az alábbi példában látható a másolási tevékenység során a folyamat a bemeneti és egy kimeneti.</span><span class="sxs-lookup"><span data-stu-id="67368-135">In the following example, you see a copy activity in the pipeline with an input and an output.</span></span> 

    ![Egy folyamat belüli tevékenységek](./media/data-factory-monitor-manage-pipelines/activities-inside-pipeline.png)
3. <span data-ttu-id="67368-137">Lépjen vissza az adat-előállítóban kezdőlapján kattintson a **adat-előállító** a webhely-navigációs a bal felső sarokban lévő hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="67368-137">You can navigate back to the home page of the data factory by clicking the **Data factory** link in the breadcrumb at the top-left corner.</span></span>

    ![Lépjen vissza az adat-előállító](./media/data-factory-monitor-manage-pipelines/navigate-back-to-data-factory.png)

### <a name="view-the-state-of-each-activity-inside-a-pipeline"></a><span data-ttu-id="67368-139">Egy folyamat belül minden tevékenység állapotának megtekintése</span><span class="sxs-lookup"><span data-stu-id="67368-139">View the state of each activity inside a pipeline</span></span>
<span data-ttu-id="67368-140">A tevékenység jelenlegi állapotában is megtekintheti az adatkészleteket, amelyek a tevékenység által létrehozott bármelyikét állapotának megtekintése.</span><span class="sxs-lookup"><span data-stu-id="67368-140">You can view the current state of an activity by viewing the status of any of the datasets that are produced by the activity.</span></span>

<span data-ttu-id="67368-141">Ehhez kattintson duplán a **OutputBlobTable** a a **Diagram**, megtekintheti a szeleteket, amelyek egy folyamat belül különböző tevékenység fut hozzák létre.</span><span class="sxs-lookup"><span data-stu-id="67368-141">By double-clicking the **OutputBlobTable** in the **Diagram**, you can see all the slices that are produced by different activity runs inside a pipeline.</span></span> <span data-ttu-id="67368-142">Láthatja, hogy a másolási tevékenység futott sikeresen a az utolsó nyolc óra, és a szeleteket előállított a **készen** állapotát.</span><span class="sxs-lookup"><span data-stu-id="67368-142">You can see that the copy activity ran successfully for the last eight hours and produced the slices in the **Ready** state.</span></span>  

![A folyamat állapota](./media/data-factory-monitor-manage-pipelines/state-of-pipeline.png)

<span data-ttu-id="67368-144">A dataset szeletek adat-előállító a következő állapotok egyike lehet:</span><span class="sxs-lookup"><span data-stu-id="67368-144">The dataset slices in the data factory can have one of the following statuses:</span></span>

<table>
<tr>
    <th align="left"><span data-ttu-id="67368-145">Állapot</span><span class="sxs-lookup"><span data-stu-id="67368-145">State</span></span></th><th align="left"><span data-ttu-id="67368-146">Alállapota</span><span class="sxs-lookup"><span data-stu-id="67368-146">Substate</span></span></th><th align="left"><span data-ttu-id="67368-147">Leírás</span><span class="sxs-lookup"><span data-stu-id="67368-147">Description</span></span></th>
</tr>
<tr>
    <td rowspan="8"><span data-ttu-id="67368-148">Várakozás</span><span class="sxs-lookup"><span data-stu-id="67368-148">Waiting</span></span></td><td><span data-ttu-id="67368-149">ScheduleTime</span><span class="sxs-lookup"><span data-stu-id="67368-149">ScheduleTime</span></span></td><td><span data-ttu-id="67368-150">A szelet futtatásának időpontjában még nem érkezett.</span><span class="sxs-lookup"><span data-stu-id="67368-150">The time hasn't come for the slice to run.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="67368-151">DatasetDependencies</span><span class="sxs-lookup"><span data-stu-id="67368-151">DatasetDependencies</span></span></td><td><span data-ttu-id="67368-152">A fölérendelt függőségek nem állnak készen.</span><span class="sxs-lookup"><span data-stu-id="67368-152">The upstream dependencies aren't ready.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="67368-153">ComputeResources</span><span class="sxs-lookup"><span data-stu-id="67368-153">ComputeResources</span></span></td><td><span data-ttu-id="67368-154">A számítási erőforrások nem érhetők el.</span><span class="sxs-lookup"><span data-stu-id="67368-154">The compute resources aren't available.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="67368-155">ConcurrencyLimit</span><span class="sxs-lookup"><span data-stu-id="67368-155">ConcurrencyLimit</span></span></td> <td><span data-ttu-id="67368-156">Az összes tevékenységpéldány más szeletek futtatásával van elfoglalva.</span><span class="sxs-lookup"><span data-stu-id="67368-156">All the activity instances are busy running other slices.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="67368-157">ActivityResume</span><span class="sxs-lookup"><span data-stu-id="67368-157">ActivityResume</span></span></td><td><span data-ttu-id="67368-158">A tevékenység szüneteltetve van, és nem tudja futtatni a szeleteket, amíg a tevékenység folytatja a működését.</span><span class="sxs-lookup"><span data-stu-id="67368-158">The activity is paused and can't run the slices until the activity is resumed.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="67368-159">Próbálja meg újra</span><span class="sxs-lookup"><span data-stu-id="67368-159">Retry</span></span></td><td><span data-ttu-id="67368-160">Tevékenység végrehajtási lesz hajtva.</span><span class="sxs-lookup"><span data-stu-id="67368-160">Activity execution is being retried.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="67368-161">Ellenőrzés</span><span class="sxs-lookup"><span data-stu-id="67368-161">Validation</span></span></td><td><span data-ttu-id="67368-162">Érvényesítés még a még nem indult el.</span><span class="sxs-lookup"><span data-stu-id="67368-162">Validation hasn't started yet.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="67368-163">ValidationRetry</span><span class="sxs-lookup"><span data-stu-id="67368-163">ValidationRetry</span></span></td><td><span data-ttu-id="67368-164">Érvényesítés újrapróbálására vár.</span><span class="sxs-lookup"><span data-stu-id="67368-164">Validation is waiting to be retried.</span></span></td>
</tr>
<tr>
<tr>
<td rowspan="2"><span data-ttu-id="67368-165">Esetbejegyzések</span><span class="sxs-lookup"><span data-stu-id="67368-165">InProgress</span></span></td><td><span data-ttu-id="67368-166">Ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="67368-166">Validating</span></span></td><td><span data-ttu-id="67368-167">Ellenőrzése folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="67368-167">Validation is in progress.</span></span></td>
</tr>
<td>-</td>
<td><span data-ttu-id="67368-168">A szelet feldolgozása folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="67368-168">The slice is being processed.</span></span></td>
</tr>
<tr>
<td rowspan="4"><span data-ttu-id="67368-169">Nem sikerült</span><span class="sxs-lookup"><span data-stu-id="67368-169">Failed</span></span></td><td><span data-ttu-id="67368-170">Időtúllépésbe került</span><span class="sxs-lookup"><span data-stu-id="67368-170">TimedOut</span></span></td><td><span data-ttu-id="67368-171">A tevékenység végrehajtási tevékenység által megengedett érték időt vett igénybe.</span><span class="sxs-lookup"><span data-stu-id="67368-171">The activity execution took longer than what is allowed by the activity.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="67368-172">Törölve</span><span class="sxs-lookup"><span data-stu-id="67368-172">Canceled</span></span></td><td><span data-ttu-id="67368-173">A szelet felhasználói művelet megszakította.</span><span class="sxs-lookup"><span data-stu-id="67368-173">The slice was canceled by user action.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="67368-174">Ellenőrzés</span><span class="sxs-lookup"><span data-stu-id="67368-174">Validation</span></span></td><td><span data-ttu-id="67368-175">Sikertelen volt.</span><span class="sxs-lookup"><span data-stu-id="67368-175">Validation has failed.</span></span></td>
</tr>
<tr>
<td>-</td><td><span data-ttu-id="67368-176">A szelet jön létre, illetve érvényesítése sikertelen.</span><span class="sxs-lookup"><span data-stu-id="67368-176">The slice failed to be generated and/or validated.</span></span></td>
</tr>
<td><span data-ttu-id="67368-177">Készen áll</span><span class="sxs-lookup"><span data-stu-id="67368-177">Ready</span></span></td><td>-</td><td><span data-ttu-id="67368-178">A szelet készen áll a felhasználásra.</span><span class="sxs-lookup"><span data-stu-id="67368-178">The slice is ready for consumption.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="67368-179">Kihagyva</span><span class="sxs-lookup"><span data-stu-id="67368-179">Skipped</span></span></td><td><span data-ttu-id="67368-180">None</span><span class="sxs-lookup"><span data-stu-id="67368-180">None</span></span></td><td><span data-ttu-id="67368-181">A szelet feldolgozása folyamatban nem.</span><span class="sxs-lookup"><span data-stu-id="67368-181">The slice isn't being processed.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="67368-182">None</span><span class="sxs-lookup"><span data-stu-id="67368-182">None</span></span></td><td>-</td><td><span data-ttu-id="67368-183">A szelet használt létezett egy eltérő állapottal, de a rendszer visszaállította.</span><span class="sxs-lookup"><span data-stu-id="67368-183">A slice used to exist with a different status, but it has been reset.</span></span></td>
</tr>
</table>



<span data-ttu-id="67368-184">A szelet bejegyzés kattintson a szelet részleteit is megtekintheti a **szeletek nemrég frissített** panelen.</span><span class="sxs-lookup"><span data-stu-id="67368-184">You can view the details about a slice by clicking a slice entry on the **Recently Updated Slices** blade.</span></span>

![Szelet részletei](./media/data-factory-monitor-manage-pipelines/slice-details.png)

<span data-ttu-id="67368-186">A szelet több alkalommal végre lett hajtva, ha látja-e a több sort a **tevékenység fut** listája.</span><span class="sxs-lookup"><span data-stu-id="67368-186">If the slice has been executed multiple times, you see multiple rows in the **Activity runs** list.</span></span> <span data-ttu-id="67368-187">Futtassa a futtatási bejegyzést kattintva tevékenységgel kapcsolatos részleteket a **tevékenység fut** listája.</span><span class="sxs-lookup"><span data-stu-id="67368-187">You can view details about an activity run by clicking the run entry in the **Activity runs** list.</span></span> <span data-ttu-id="67368-188">A lista tartalmazza az összes naplófájlt, valamint egy hibaüzenet, ha van ilyen.</span><span class="sxs-lookup"><span data-stu-id="67368-188">The list shows all the log files, along with an error message if there is one.</span></span> <span data-ttu-id="67368-189">Ez a szolgáltatás akkor hasznos, megtekintése és hibakeresési naplókat, anélkül, hogy a data factory.</span><span class="sxs-lookup"><span data-stu-id="67368-189">This feature is useful to view and debug logs without having to leave your data factory.</span></span>

![Tevékenységfuttatás részletei](./media/data-factory-monitor-manage-pipelines/activity-run-details.png)

<span data-ttu-id="67368-191">Ha a szelet nem szerepel a **készen** állapot, megtekintheti a felfelé irányuló szeletek nem állnak készen, és az aktuális szelet végrehajtás alatt blokkolják a **felfelé irányuló szeletek nem áll készen** listája.</span><span class="sxs-lookup"><span data-stu-id="67368-191">If the slice isn't in the **Ready** state, you can see the upstream slices that aren't ready and are blocking the current slice from executing in the **Upstream slices that are not ready** list.</span></span> <span data-ttu-id="67368-192">A szolgáltatás akkor hasznos, ha a szelet **Várakozás** állapotát, és szeretné tudni, hogy a szelet vár a fölérendelt függőségek.</span><span class="sxs-lookup"><span data-stu-id="67368-192">This feature is useful when your slice is in **Waiting** state and you want to understand the upstream dependencies that the slice is waiting on.</span></span>

![Nem kész állapotú felfelé irányuló szeletek](./media/data-factory-monitor-manage-pipelines/upstream-slices-not-ready.png)

### <a name="dataset-state-diagram"></a><span data-ttu-id="67368-194">A DataSet állapot diagramja</span><span class="sxs-lookup"><span data-stu-id="67368-194">Dataset state diagram</span></span>
<span data-ttu-id="67368-195">Miután telepít egy adat-előállítót, és a folyamatok érvényes aktív idővel rendelkeznek, az adatkészlet átmenet egy állapot másik szeletek.</span><span class="sxs-lookup"><span data-stu-id="67368-195">After you deploy a data factory and the pipelines have a valid active period, the dataset slices transition from one state to another.</span></span> <span data-ttu-id="67368-196">A szelet állapota jelenleg a következő ábra állapotát követi:</span><span class="sxs-lookup"><span data-stu-id="67368-196">Currently, the slice status follows the following state diagram:</span></span>

![Állapotdiagram](./media/data-factory-monitor-manage-pipelines/state-diagram.png)

<span data-ttu-id="67368-198">A dataset állapot átmenet folyamata adat-előállítót a rendszeren a következő: várakozás közben a rendszer -> folyamatban lévő vagy a-folyamatban (érvényesítés) -> kész/nem sikerült.</span><span class="sxs-lookup"><span data-stu-id="67368-198">The dataset state transition flow in data factory is the following: Waiting -> In-Progress/In-Progress (Validating) -> Ready/Failed.</span></span>

<span data-ttu-id="67368-199">A szelet elindul egy **Várakozás** állapot, Várakozás a végrehajtása előtt teljesítendő előfeltételek.</span><span class="sxs-lookup"><span data-stu-id="67368-199">The slice starts in a **Waiting** state, waiting for preconditions to be met before it executes.</span></span> <span data-ttu-id="67368-200">Ezután a tevékenység végrehajtásának elkezdése, és a szelet kerülnek egy **folyamatban** állapotát.</span><span class="sxs-lookup"><span data-stu-id="67368-200">Then, the activity starts executing, and the slice goes into an **In-Progress** state.</span></span> <span data-ttu-id="67368-201">A tevékenység végrehajtási előfordulhat, hogy sikeres vagy sikertelen.</span><span class="sxs-lookup"><span data-stu-id="67368-201">The activity execution might succeed or fail.</span></span> <span data-ttu-id="67368-202">A szelet jelölésű **készen** vagy **sikertelen**a végrehajtás eredménye alapján.</span><span class="sxs-lookup"><span data-stu-id="67368-202">The slice is marked as **Ready** or **Failed**, based on the result of the execution.</span></span>

<span data-ttu-id="67368-203">Visszaállíthatja a szelet lépjen vissza a a **készen** vagy **sikertelen** állapotát a **Várakozás** állapotát.</span><span class="sxs-lookup"><span data-stu-id="67368-203">You can reset the slice to go back from the **Ready** or **Failed** state to the **Waiting** state.</span></span> <span data-ttu-id="67368-204">A szelet állapot is jelölheti **kihagyása**, amely megakadályozza, hogy a tevékenység végrehajtása, és nem az a szelet feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="67368-204">You can also mark the slice state to **Skip**, which prevents the activity from executing and not processing the slice.</span></span>

## <a name="pause-and-resume-pipelines"></a><span data-ttu-id="67368-205">Felfüggesztése és folytatása folyamatok</span><span class="sxs-lookup"><span data-stu-id="67368-205">Pause and resume pipelines</span></span>
<span data-ttu-id="67368-206">A folyamatok Azure PowerShell használatával kezelheti.</span><span class="sxs-lookup"><span data-stu-id="67368-206">You can manage your pipelines by using Azure PowerShell.</span></span> <span data-ttu-id="67368-207">Például szüneteltetése és folytatása folyamatok Azure PowerShell-parancsmag futtatásával.</span><span class="sxs-lookup"><span data-stu-id="67368-207">For example, you can pause and resume pipelines by running Azure PowerShell cmdlets.</span></span> 

> [!NOTE] 
> <span data-ttu-id="67368-208">A diagram nézet nem támogatja a felfüggesztése és folytatása folyamatok.</span><span class="sxs-lookup"><span data-stu-id="67368-208">The diagram view does not support pausing and resuming pipelines.</span></span> <span data-ttu-id="67368-209">Ha szeretne egy felhasználói felületet használnak, használja a figyelési és kezelését alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="67368-209">If you want to use an user interface, use the monitoring and managing application.</span></span> <span data-ttu-id="67368-210">Az alkalmazás használatával kapcsolatos részletekért lásd: [felügyeletéhez és kezeléséhez az adat-előállító adatcsatornák a figyelés és felügyelet alkalmazással](data-factory-monitor-manage-app.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="67368-210">For details about using the application, see [monitor and manage Data Factory pipelines by using the Monitoring and Management app](data-factory-monitor-manage-app.md) article.</span></span> 

<span data-ttu-id="67368-211">Is szüneteltetése vagy felfüggesztjük folyamatok használatával a **Suspend-AzureRmDataFactoryPipeline** PowerShell-parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="67368-211">You can pause/suspend pipelines by using the **Suspend-AzureRmDataFactoryPipeline** PowerShell cmdlet.</span></span> <span data-ttu-id="67368-212">Ez a parancsmag akkor hasznos, ha nem szeretné futtatni a folyamatok, a probléma a megoldásáig.</span><span class="sxs-lookup"><span data-stu-id="67368-212">This cmdlet is useful when you don’t want to run your pipelines until an issue is fixed.</span></span> 

```powershell
Suspend-AzureRmDataFactoryPipeline [-ResourceGroupName] <String> [-DataFactoryName] <String> [-Name] <String>
```
<span data-ttu-id="67368-213">Példa:</span><span class="sxs-lookup"><span data-stu-id="67368-213">For example:</span></span>

```powershell
Suspend-AzureRmDataFactoryPipeline -ResourceGroupName ADF -DataFactoryName productrecgamalbox1dev -Name PartitionProductsUsagePipeline
```

<span data-ttu-id="67368-214">A probléma kijavítása a a folyamat, után folytathatja a felfüggesztett folyamat a következő PowerShell-parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="67368-214">After the issue has been fixed with the pipeline, you can resume the suspended pipeline by running the following PowerShell command:</span></span>

```powershell
Resume-AzureRmDataFactoryPipeline [-ResourceGroupName] <String> [-DataFactoryName] <String> [-Name] <String>
```
<span data-ttu-id="67368-215">Példa:</span><span class="sxs-lookup"><span data-stu-id="67368-215">For example:</span></span>

```powershell
Resume-AzureRmDataFactoryPipeline -ResourceGroupName ADF -DataFactoryName productrecgamalbox1dev -Name PartitionProductsUsagePipeline
```

## <a name="debug-pipelines"></a><span data-ttu-id="67368-216">Folyamatok hibakeresése</span><span class="sxs-lookup"><span data-stu-id="67368-216">Debug pipelines</span></span>
<span data-ttu-id="67368-217">Az Azure Data Factory debug vagy folyamatok hibaelhárítása az Azure portál és az Azure PowerShell használatával gazdag lehetőségeket kínál.</span><span class="sxs-lookup"><span data-stu-id="67368-217">Azure Data Factory provides rich capabilities for you to debug and troubleshoot pipelines by using the Azure portal and Azure PowerShell.</span></span>

> <span data-ttu-id="67368-218">[! Megjegyzés:} sokkal könnyebb a figyelés és a felügyeleti alkalmazás troubleshot hibák.</span><span class="sxs-lookup"><span data-stu-id="67368-218">[!NOTE} It is much easier to troubleshot errors using the Monitoring & Management App.</span></span> <span data-ttu-id="67368-219">Az alkalmazás használatával kapcsolatos részletekért lásd: [felügyeletéhez és kezeléséhez az adat-előállító adatcsatornák a figyelés és felügyelet alkalmazással](data-factory-monitor-manage-app.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="67368-219">For details about using the application, see [monitor and manage Data Factory pipelines by using the Monitoring and Management app](data-factory-monitor-manage-app.md) article.</span></span> 

### <a name="find-errors-in-a-pipeline"></a><span data-ttu-id="67368-220">Hiba található egy folyamatot</span><span class="sxs-lookup"><span data-stu-id="67368-220">Find errors in a pipeline</span></span>
<span data-ttu-id="67368-221">Ha a tevékenység futtatása sikertelen, a folyamat, a folyamat által létrehozott adatkészlet állapotban van hiba hibája miatt.</span><span class="sxs-lookup"><span data-stu-id="67368-221">If the activity run fails in a pipeline, the dataset that is produced by the pipeline is in an error state because of the failure.</span></span> <span data-ttu-id="67368-222">Hibakeresés, és az Azure Data Factory hibáinak elhárítása az alábbi módszerek segítségével.</span><span class="sxs-lookup"><span data-stu-id="67368-222">You can debug and troubleshoot errors in Azure Data Factory by using the following methods.</span></span>

#### <a name="use-the-azure-portal-to-debug-an-error"></a><span data-ttu-id="67368-223">Hiba történt az Azure portál használata</span><span class="sxs-lookup"><span data-stu-id="67368-223">Use the Azure portal to debug an error</span></span>
1. <span data-ttu-id="67368-224">Az a **tábla** panelen kattintson a probléma szelet, amely rendelkezik a **állapot** beállítása **sikertelen**.</span><span class="sxs-lookup"><span data-stu-id="67368-224">On the **Table** blade, click the problem slice that has the **Status** set to **Failed**.</span></span>

   ![Probléma szelet tábla panelről](./media/data-factory-monitor-manage-pipelines/table-blade-with-error.png)
2. <span data-ttu-id="67368-226">Az a **adatszelet** panelen, kattintson a tevékenység futtatása sikertelen.</span><span class="sxs-lookup"><span data-stu-id="67368-226">On the **Data slice** blade, click the activity run that failed.</span></span>

   ![Hiba történt az adatszelet](./media/data-factory-monitor-manage-pipelines/dataslice-with-error.png)
3. <span data-ttu-id="67368-228">Az a **tevékenység fut részletek** panelen letöltheti a fájlokat, társított HDInsight feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="67368-228">On the **Activity run details** blade, you can download the files that are associated with the HDInsight processing.</span></span> <span data-ttu-id="67368-229">Kattintson a **letöltése** az állapot/stderr letölteni a hibanaplófájlt, amely tartalmazza a hiba részleteit.</span><span class="sxs-lookup"><span data-stu-id="67368-229">Click **Download** for Status/stderr to download the error log file that contains details about the error.</span></span>

   ![A következő hiba tevékenységfuttatási részleteit megjelenítő panelen](./media/data-factory-monitor-manage-pipelines/activity-run-details-with-error.png)     

#### <a name="use-powershell-to-debug-an-error"></a><span data-ttu-id="67368-231">A PowerShell szolgáltatás használatával debug hiba</span><span class="sxs-lookup"><span data-stu-id="67368-231">Use PowerShell to debug an error</span></span>
1. <span data-ttu-id="67368-232">Indítsa el a **PowerShellt**.</span><span class="sxs-lookup"><span data-stu-id="67368-232">Launch **PowerShell**.</span></span>
2. <span data-ttu-id="67368-233">Futtassa a **Get-AzureRmDataFactorySlice** parancsot a szeletek és azok állapotának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="67368-233">Run the **Get-AzureRmDataFactorySlice** command to see the slices and their statuses.</span></span> <span data-ttu-id="67368-234">A szelet és az állapotuk kell megjelennie **sikertelen**.</span><span class="sxs-lookup"><span data-stu-id="67368-234">You should see a slice with the status of **Failed**.</span></span>        

    ```powershell   
    Get-AzureRmDataFactorySlice [-ResourceGroupName] <String> [-DataFactoryName] <String> [-DatasetName] <String> [-StartDateTime] <DateTime> [[-EndDateTime] <DateTime> ] [-Profile <AzureProfile> ] [ <CommonParameters>]
    ```   
   <span data-ttu-id="67368-235">Példa:</span><span class="sxs-lookup"><span data-stu-id="67368-235">For example:</span></span>

    ```powershell   
    Get-AzureRmDataFactorySlice -ResourceGroupName ADF -DataFactoryName LogProcessingFactory -DatasetName EnrichedGameEventsTable -StartDateTime 2014-05-04 20:00:00
    ```

   <span data-ttu-id="67368-236">Cserélje le **StartDateTime** az a folyamat kezdési idejét.</span><span class="sxs-lookup"><span data-stu-id="67368-236">Replace **StartDateTime** with start time of your pipeline.</span></span> 
3. <span data-ttu-id="67368-237">Futtassa a **Get-AzureRmDataFactoryRun** parancsmag részletes információkat a tevékenység Futtatás újrapróbálása.</span><span class="sxs-lookup"><span data-stu-id="67368-237">Now, run the **Get-AzureRmDataFactoryRun** cmdlet to get details about the activity run for the slice.</span></span>

    ```powershell   
    Get-AzureRmDataFactoryRun [-ResourceGroupName] <String> [-DataFactoryName] <String> [-DatasetName] <String> [-StartDateTime]
    <DateTime> [-Profile <AzureProfile> ] [ <CommonParameters>]
    ```

    <span data-ttu-id="67368-238">Példa:</span><span class="sxs-lookup"><span data-stu-id="67368-238">For example:</span></span>

    ```powershell   
    Get-AzureRmDataFactoryRun -ResourceGroupName ADF -DataFactoryName LogProcessingFactory -DatasetName EnrichedGameEventsTable -StartDateTime "5/5/2014 12:00:00 AM"
    ```

    <span data-ttu-id="67368-239">StartDateTime értéke a hiba vagy probléma szelet, amely az előző lépésben feljegyzett kezdési idejét.</span><span class="sxs-lookup"><span data-stu-id="67368-239">The value of StartDateTime is the start time for the error/problem slice that you noted from the previous step.</span></span> <span data-ttu-id="67368-240">A dátum-idő dupla idézőjelek között kell megadni.</span><span class="sxs-lookup"><span data-stu-id="67368-240">The date-time should be enclosed in double quotes.</span></span>
4. <span data-ttu-id="67368-241">Adatokkal kapcsolatban, amelyek a következőhöz hasonló kimenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="67368-241">You should see output with details about the error that is similar to the following:</span></span>

    ```   
    Id                      : 841b77c9-d56c-48d1-99a3-8c16c3e77d39
    ResourceGroupName       : ADF
    DataFactoryName         : LogProcessingFactory3
    DatasetName               : EnrichedGameEventsTable
    ProcessingStartTime     : 10/10/2014 3:04:52 AM
    ProcessingEndTime       : 10/10/2014 3:06:49 AM
    PercentComplete         : 0
    DataSliceStart          : 5/5/2014 12:00:00 AM
    DataSliceEnd            : 5/6/2014 12:00:00 AM
    Status                  : FailedExecution
    Timestamp               : 10/10/2014 3:04:52 AM
    RetryAttempt            : 0
    Properties              : {}
    ErrorMessage            : Pig script failed with exit code '5'. See wasb://        adfjobs@spestore.blob.core.windows.net/PigQuery
                                    Jobs/841b77c9-d56c-48d1-99a3-
                8c16c3e77d39/10_10_2014_03_04_53_277/Status/stderr' for
                more details.
    ActivityName            : PigEnrichLogs
    PipelineName            : EnrichGameLogsPipeline
    Type                    :
    ```
5. <span data-ttu-id="67368-242">Futtathatja a **mentés-AzureRmDataFactoryLog** parancsmagot az azonosító értéke, tekintse meg a kimenetben, és a naplófájlok használva töltik le a **- DownloadLogsoption** a parancsmaghoz.</span><span class="sxs-lookup"><span data-stu-id="67368-242">You can run the **Save-AzureRmDataFactoryLog** cmdlet with the Id value that you see from the output, and download the log files by using the **-DownloadLogsoption** for the cmdlet.</span></span>

    ```powershell
    Save-AzureRmDataFactoryLog -ResourceGroupName "ADF" -DataFactoryName "LogProcessingFactory" -Id "841b77c9-d56c-48d1-99a3-8c16c3e77d39" -DownloadLogs -Output "C:\Test"
    ```

## <a name="rerun-failures-in-a-pipeline"></a><span data-ttu-id="67368-243">Futtassa újra a feldolgozási hibák</span><span class="sxs-lookup"><span data-stu-id="67368-243">Rerun failures in a pipeline</span></span>

> [!IMPORTANT]
> <span data-ttu-id="67368-244">Célszerűbb hibáinak elhárítása, és futtassa újra a sikertelen szeletek a figyelés & a felügyeleti alkalmazás használatával.</span><span class="sxs-lookup"><span data-stu-id="67368-244">It's easier to troubleshoot errors and rerun failed slices by using the Monitoring & Management App.</span></span> <span data-ttu-id="67368-245">Az alkalmazás használatával kapcsolatos részletekért lásd: [felügyeletéhez és kezeléséhez az adat-előállító adatcsatornák a figyelés és felügyelet alkalmazással](data-factory-monitor-manage-app.md).</span><span class="sxs-lookup"><span data-stu-id="67368-245">For details about using the application, see [monitor and manage Data Factory pipelines by using the Monitoring and Management app](data-factory-monitor-manage-app.md).</span></span> 

### <a name="use-the-azure-portal"></a><span data-ttu-id="67368-246">Az Azure Portal használata</span><span class="sxs-lookup"><span data-stu-id="67368-246">Use the Azure portal</span></span>
<span data-ttu-id="67368-247">Hibaelhárítás és a feldolgozási hibák hibakeresése, után ismét lefuttathatja hibák hiba újrapróbálása megnyitásával, majd kattintson a **futtatása** gombra a parancssávon.</span><span class="sxs-lookup"><span data-stu-id="67368-247">After you troubleshoot and debug failures in a pipeline, you can rerun failures by navigating to the error slice and clicking the **Run** button on the command bar.</span></span>

![Futtassa újra a hibás szeletet](./media/data-factory-monitor-manage-pipelines/rerun-slice.png)

<span data-ttu-id="67368-249">Esetében a szelet meghiúsult érvényesítési házirend hiba miatt (Ha például adatok nem érhető el), javítsa ki a hibát, és ellenőrizze, hogy az újra gombra kattintva a **ellenőrzése** gombra a parancssávon.</span><span class="sxs-lookup"><span data-stu-id="67368-249">In case the slice has failed validation because of a policy failure (for example, if data isn't available), you can fix the failure and validate again by clicking the **Validate** button on the command bar.</span></span>

![Javítsa a hibákat, és érvényesítése](./media/data-factory-monitor-manage-pipelines/fix-error-and-validate.png)

### <a name="use-azure-powershell"></a><span data-ttu-id="67368-251">Azure PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="67368-251">Use Azure PowerShell</span></span>
<span data-ttu-id="67368-252">Hibák használatával ismét lefuttathatja a **Set-AzureRmDataFactorySliceStatus** parancsmag.</span><span class="sxs-lookup"><span data-stu-id="67368-252">You can rerun failures by using the **Set-AzureRmDataFactorySliceStatus** cmdlet.</span></span> <span data-ttu-id="67368-253">Tekintse meg a [Set-AzureRmDataFactorySliceStatus](https://msdn.microsoft.com/library/mt603522.aspx) szintaxisát és egyéb adatait a parancsmag a témakörben.</span><span class="sxs-lookup"><span data-stu-id="67368-253">See the [Set-AzureRmDataFactorySliceStatus](https://msdn.microsoft.com/library/mt603522.aspx) topic for syntax and other details about the cmdlet.</span></span>

<span data-ttu-id="67368-254">**Példa**</span><span class="sxs-lookup"><span data-stu-id="67368-254">**Example:**</span></span>

<span data-ttu-id="67368-255">Az alábbi példa állítja be a következő táblázatban minden szeletek állapotának "DAWikiAggregatedData" "Várakozási" az Azure data factory "WikiADF".</span><span class="sxs-lookup"><span data-stu-id="67368-255">The following example sets the status of all slices for the table 'DAWikiAggregatedData' to 'Waiting' in the Azure data factory 'WikiADF'.</span></span>

<span data-ttu-id="67368-256">A "frissítés"típusa "Upstreaminpipeline paramétert", ami azt jelenti, hogy a tábla minden egyes szeletet és minden függő (fölérendelt) tábla állapotok van beállítva "Vár" értékre van állítva.</span><span class="sxs-lookup"><span data-stu-id="67368-256">The 'UpdateType' is set to 'UpstreamInPipeline', which means that statuses of each slice for the table and all the dependent (upstream) tables are set to 'Waiting'.</span></span> <span data-ttu-id="67368-257">Más lehetséges Ez a paraméter értéke "Egyéni".</span><span class="sxs-lookup"><span data-stu-id="67368-257">The other possible value for this parameter is 'Individual'.</span></span>

```powershell
Set-AzureRmDataFactorySliceStatus -ResourceGroupName ADF -DataFactoryName WikiADF -DatasetName DAWikiAggregatedData -Status Waiting -UpdateType UpstreamInPipeline -StartDateTime 2014-05-21T16:00:00 -EndDateTime 2014-05-21T20:00:00
```

## <a name="create-alerts"></a><span data-ttu-id="67368-258">Riasztások létrehozása</span><span class="sxs-lookup"><span data-stu-id="67368-258">Create alerts</span></span>
<span data-ttu-id="67368-259">Azure naplók felhasználói eseményeket egy Azure-erőforrás (például a data factory) létrehozott, frissíteni vagy törölni.</span><span class="sxs-lookup"><span data-stu-id="67368-259">Azure logs user events when an Azure resource (for example, a data factory) is created, updated, or deleted.</span></span> <span data-ttu-id="67368-260">Ezek az események a riasztásokat hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="67368-260">You can create alerts on these events.</span></span> <span data-ttu-id="67368-261">Adat-előállító segítségével különböző metrikák rögzítése, és hozza létre a riasztások a metrikákat.</span><span class="sxs-lookup"><span data-stu-id="67368-261">You can use Data Factory to capture various metrics and create alerts on metrics.</span></span> <span data-ttu-id="67368-262">Azt javasoljuk, hogy használja az eseményeket a valós idejű figyelését, és metrikák használja az előzmények elérése érdekében.</span><span class="sxs-lookup"><span data-stu-id="67368-262">We recommend that you use events for real-time monitoring and use metrics for historical purposes.</span></span>

### <a name="alerts-on-events"></a><span data-ttu-id="67368-263">Riasztások a események</span><span class="sxs-lookup"><span data-stu-id="67368-263">Alerts on events</span></span>
<span data-ttu-id="67368-264">Az Azure események mi történik az Azure-erőforrások a hasznos információkat adnak.</span><span class="sxs-lookup"><span data-stu-id="67368-264">Azure events provide useful insights into what is happening in your Azure resources.</span></span> <span data-ttu-id="67368-265">Azure Data Factory használatakor események akkor jönnek létre, ha:</span><span class="sxs-lookup"><span data-stu-id="67368-265">When you're using Azure Data Factory, events are generated when:</span></span>

* <span data-ttu-id="67368-266">Egy adat-előállító létrehozott, frissíteni vagy törölni.</span><span class="sxs-lookup"><span data-stu-id="67368-266">A data factory is created, updated, or deleted.</span></span>
* <span data-ttu-id="67368-267">Adatok feldolgozása (a "fut") van elindítva, vagy befejeződött.</span><span class="sxs-lookup"><span data-stu-id="67368-267">Data processing (as "runs") has started or completed.</span></span>
* <span data-ttu-id="67368-268">Igény szerinti HDInsight-fürtök létrehozásakor vagy eltávolításakor.</span><span class="sxs-lookup"><span data-stu-id="67368-268">An on-demand HDInsight cluster is created or removed.</span></span>

<span data-ttu-id="67368-269">A felhasználó eseményekből létre riasztásokat, és úgy konfigurálja azokat, a rendszergazda és az előfizetés coadministrators e-mail értesítések küldéséhez.</span><span class="sxs-lookup"><span data-stu-id="67368-269">You can create alerts on these user events and configure them to send email notifications to the administrator and coadministrators of the subscription.</span></span> <span data-ttu-id="67368-270">Emellett a felhasználók, akik e-mail értesítéseket a feltételek teljesülése esetén további e-mail címeket is megadhat.</span><span class="sxs-lookup"><span data-stu-id="67368-270">In addition, you can specify additional email addresses of users who need to receive email notifications when the conditions are met.</span></span> <span data-ttu-id="67368-271">Ez a szolgáltatás akkor hasznos, ha szeretne értesítést kaphat a sikertelen végrehajtása esetén, és nem szeretné a data factory folyamatosan figyelni.</span><span class="sxs-lookup"><span data-stu-id="67368-271">This feature is useful when you want to get notified on failures and don’t want to continuously monitor your data factory.</span></span>

> [!NOTE]
> <span data-ttu-id="67368-272">Jelenleg a portál nem riasztások megjelenítése a események.</span><span class="sxs-lookup"><span data-stu-id="67368-272">Currently, the portal doesn't show alerts on events.</span></span> <span data-ttu-id="67368-273">Használja a [figyelés és a felügyeleti alkalmazás](data-factory-monitor-manage-app.md) riasztások megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="67368-273">Use the [Monitoring and Management app](data-factory-monitor-manage-app.md) to see all alerts.</span></span>


#### <a name="specify-an-alert-definition"></a><span data-ttu-id="67368-274">Adjon meg egy értesítési definíciója</span><span class="sxs-lookup"><span data-stu-id="67368-274">Specify an alert definition</span></span>
<span data-ttu-id="67368-275">Adjon meg egy értesítési definíciója, hozzon létre egy JSON-fájl, amely leírja, amelyet szeretne kapni a műveletek.</span><span class="sxs-lookup"><span data-stu-id="67368-275">To specify an alert definition, you create a JSON file that describes the operations that you want to be alerted on.</span></span> <span data-ttu-id="67368-276">A riasztás a következő példában a RunFinished művelet e-mailben értesítést küld.</span><span class="sxs-lookup"><span data-stu-id="67368-276">In the following example, the alert sends an email notification for the RunFinished operation.</span></span> <span data-ttu-id="67368-277">Ahhoz, hogy adott, e-mailben értesítést küldi, ha az adat-előállító futtatás befejeződött, és a Futtatás nem sikerült (állapot = FailedExecution).</span><span class="sxs-lookup"><span data-stu-id="67368-277">To be specific, an email notification is sent when a run in the data factory has completed and the run has failed (Status = FailedExecution).</span></span>

```JSON
{
    "contentVersion": "1.0.0.0",
     "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "parameters": {},
    "resources":
    [
        {
            "name": "ADFAlertsSlice",
            "type": "microsoft.insights/alertrules",
            "apiVersion": "2014-04-01",
            "location": "East US",
            "properties":
            {
                "name": "ADFAlertsSlice",
                "description": "One or more of the data slices for the Azure Data Factory has failed processing.",
                "isEnabled": true,
                "condition":
                {
                    "odata.type": "Microsoft.Azure.Management.Insights.Models.ManagementEventRuleCondition",
                    "dataSource":
                    {
                        "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleManagementEventDataSource",
                        "operationName": "RunFinished",
                        "status": "Failed",
                        "subStatus": "FailedExecution"   
                    }
                },
                "action":
                {
                    "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleEmailAction",
                    "customEmails": [ "<your alias>@contoso.com" ]
                }
            }
        }
    ]
}
```

<span data-ttu-id="67368-278">Eltávolíthatja **részállapot** a JSON-definícióból Ha nem szeretne kapni a konkrét hiba.</span><span class="sxs-lookup"><span data-stu-id="67368-278">You can remove **subStatus** from the JSON definition if you don’t want to be alerted on a specific failure.</span></span>

<span data-ttu-id="67368-279">Ez a példa állít be az előfizetés az összes adat-előállítók kapcsolatos riasztás.</span><span class="sxs-lookup"><span data-stu-id="67368-279">This example sets up the alert for all data factories in your subscription.</span></span> <span data-ttu-id="67368-280">Ha azt szeretné, hogy egy adott adat-előállító beállítva a kívánt riasztást, megadhatja az adat-előállító **resourceUri** a a **dataSource**:</span><span class="sxs-lookup"><span data-stu-id="67368-280">If you want the alert to be set up for a particular data factory, you can specify data factory **resourceUri** in the **dataSource**:</span></span>

```JSON
"resourceUri" : "/SUBSCRIPTIONS/<subscriptionId>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.DATAFACTORY/DATAFACTORIES/<dataFactoryName>"
```

<span data-ttu-id="67368-281">A következő táblázat az elérhető műveletek és állapotok (és részállapotok) listáját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="67368-281">The following table provides the list of available operations and statuses (and substatuses).</span></span>

| <span data-ttu-id="67368-282">A művelet neve</span><span class="sxs-lookup"><span data-stu-id="67368-282">Operation name</span></span> | <span data-ttu-id="67368-283">status</span><span class="sxs-lookup"><span data-stu-id="67368-283">Status</span></span> | <span data-ttu-id="67368-284">A részállapot</span><span class="sxs-lookup"><span data-stu-id="67368-284">Substatus</span></span> |
| --- | --- | --- |
| <span data-ttu-id="67368-285">RunStarted</span><span class="sxs-lookup"><span data-stu-id="67368-285">RunStarted</span></span> |<span data-ttu-id="67368-286">Megkezdődött</span><span class="sxs-lookup"><span data-stu-id="67368-286">Started</span></span> |<span data-ttu-id="67368-287">Indulás alatt</span><span class="sxs-lookup"><span data-stu-id="67368-287">Starting</span></span> |
| <span data-ttu-id="67368-288">RunFinished</span><span class="sxs-lookup"><span data-stu-id="67368-288">RunFinished</span></span> |<span data-ttu-id="67368-289">Nem sikerült / sikeres volt.</span><span class="sxs-lookup"><span data-stu-id="67368-289">Failed / Succeeded</span></span> |<span data-ttu-id="67368-290">FailedResourceAllocation</span><span class="sxs-lookup"><span data-stu-id="67368-290">FailedResourceAllocation</span></span><br/><br/><span data-ttu-id="67368-291">Sikeres</span><span class="sxs-lookup"><span data-stu-id="67368-291">Succeeded</span></span><br/><br/><span data-ttu-id="67368-292">FailedExecution</span><span class="sxs-lookup"><span data-stu-id="67368-292">FailedExecution</span></span><br/><br/><span data-ttu-id="67368-293">Időtúllépésbe került</span><span class="sxs-lookup"><span data-stu-id="67368-293">TimedOut</span></span><br/><br/><span data-ttu-id="67368-294">< megszakítva</span><span class="sxs-lookup"><span data-stu-id="67368-294"><Canceled</span></span><br/><br/><span data-ttu-id="67368-295">FailedValidation</span><span class="sxs-lookup"><span data-stu-id="67368-295">FailedValidation</span></span><br/><br/><span data-ttu-id="67368-296">Elhagyott</span><span class="sxs-lookup"><span data-stu-id="67368-296">Abandoned</span></span> |
| <span data-ttu-id="67368-297">OnDemandClusterCreateStarted</span><span class="sxs-lookup"><span data-stu-id="67368-297">OnDemandClusterCreateStarted</span></span> |<span data-ttu-id="67368-298">Megkezdődött</span><span class="sxs-lookup"><span data-stu-id="67368-298">Started</span></span> | |
| <span data-ttu-id="67368-299">OnDemandClusterCreateSuccessful</span><span class="sxs-lookup"><span data-stu-id="67368-299">OnDemandClusterCreateSuccessful</span></span> |<span data-ttu-id="67368-300">Sikeres</span><span class="sxs-lookup"><span data-stu-id="67368-300">Succeeded</span></span> | |
| <span data-ttu-id="67368-301">OnDemandClusterDeleted</span><span class="sxs-lookup"><span data-stu-id="67368-301">OnDemandClusterDeleted</span></span> |<span data-ttu-id="67368-302">Sikeres</span><span class="sxs-lookup"><span data-stu-id="67368-302">Succeeded</span></span> | |

<span data-ttu-id="67368-303">Lásd: [riasztási szabály létrehozása](https://msdn.microsoft.com/library/azure/dn510366.aspx) a JSON-elemek szerepelnek a példában használt vonatkozó további információért.</span><span class="sxs-lookup"><span data-stu-id="67368-303">See [Create Alert Rule](https://msdn.microsoft.com/library/azure/dn510366.aspx) for details about the JSON elements that are used in the example.</span></span>

#### <a name="deploy-the-alert"></a><span data-ttu-id="67368-304">A riasztás telepítése</span><span class="sxs-lookup"><span data-stu-id="67368-304">Deploy the alert</span></span>
<span data-ttu-id="67368-305">A riasztás telepítéséhez használja az Azure PowerShell-parancsmag **New-AzureRmResourceGroupDeployment**, a következő példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="67368-305">To deploy the alert, use the Azure PowerShell cmdlet **New-AzureRmResourceGroupDeployment**, as shown in the following example:</span></span>

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName adf -TemplateFile .\ADFAlertFailedSlice.json  
```

<span data-ttu-id="67368-306">Miután az erőforrás-csoport központi telepítése sikeresen befejeződött, megjelenik az alábbi üzenetek:</span><span class="sxs-lookup"><span data-stu-id="67368-306">After the resource group deployment has finished successfully, you see the following messages:</span></span>

```
VERBOSE: 7:00:48 PM - Template is valid.
WARNING: 7:00:48 PM - The StorageAccountName parameter is no longer used and will be removed in a future release.
Please update scripts to remove this parameter.
VERBOSE: 7:00:49 PM - Create template deployment 'ADFAlertFailedSlice'.
VERBOSE: 7:00:57 PM - Resource microsoft.insights/alertrules 'ADFAlertsSlice' provisioning status is succeeded

DeploymentName    : ADFAlertFailedSlice
ResourceGroupName : adf
ProvisioningState : Succeeded
Timestamp         : 10/11/2014 2:01:00 AM
Mode              : Incremental
TemplateLink      :
Parameters        :
Outputs           :
```

> [!NOTE]
> <span data-ttu-id="67368-307">Használhatja a [riasztást szabály létrehozása](https://msdn.microsoft.com/library/azure/dn510366.aspx) REST API-t riasztási szabályt létrehozni.</span><span class="sxs-lookup"><span data-stu-id="67368-307">You can use the [Create Alert Rule](https://msdn.microsoft.com/library/azure/dn510366.aspx) REST API to create an alert rule.</span></span> <span data-ttu-id="67368-308">A JSON-adattartalmat hasonlít a JSON-példa.</span><span class="sxs-lookup"><span data-stu-id="67368-308">The JSON payload is similar to the JSON example.</span></span>  


#### <a name="retrieve-the-list-of-azure-resource-group-deployments"></a><span data-ttu-id="67368-309">Az Azure erőforrás-csoport központi telepítések listájának beolvasása</span><span class="sxs-lookup"><span data-stu-id="67368-309">Retrieve the list of Azure resource group deployments</span></span>
<span data-ttu-id="67368-310">Telepített Azure erőforráscsoport-csoport központi telepítések listájának lekéréséhez használja a parancsmag **Get-AzureRmResourceGroupDeployment**, a következő példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="67368-310">To retrieve the list of deployed Azure resource group deployments, use the cmdlet **Get-AzureRmResourceGroupDeployment**, as shown in the following example:</span></span>

```powershell
Get-AzureRmResourceGroupDeployment -ResourceGroupName adf
```

```
DeploymentName    : ADFAlertFailedSlice
ResourceGroupName : adf
ProvisioningState : Succeeded
Timestamp         : 10/11/2014 2:01:00 AM
Mode              : Incremental
TemplateLink      :
Parameters        :
Outputs           :
```

#### <a name="troubleshoot-user-events"></a><span data-ttu-id="67368-311">Felhasználói események hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="67368-311">Troubleshoot user events</span></span>
1. <span data-ttu-id="67368-312">Kattintás után létrehozott események láthatja a **metrikák és műveletek** csempére.</span><span class="sxs-lookup"><span data-stu-id="67368-312">You can see all the events that are generated after clicking the **Metrics and operations** tile.</span></span>

    ![Metrikák és műveletek csempe](./media/data-factory-monitor-manage-pipelines/metrics-and-operations-tile.png)
2. <span data-ttu-id="67368-314">Kattintson a **események** csempe az eseményeket.</span><span class="sxs-lookup"><span data-stu-id="67368-314">Click the **Events** tile to see the events.</span></span>

    ![Események csempéje](./media/data-factory-monitor-manage-pipelines/events-tile.png)
3. <span data-ttu-id="67368-316">Az a **események** panelen láthatja, hogy eseményeket, szűrt eseményeket, és egyéb adatait.</span><span class="sxs-lookup"><span data-stu-id="67368-316">On the **Events** blade, you can see details about events, filtered events, and so on.</span></span>

    ![Események panel](./media/data-factory-monitor-manage-pipelines/events-blade.png)
4. <span data-ttu-id="67368-318">Kattintson egy **művelet** hibát okoz a műveletek listájában.</span><span class="sxs-lookup"><span data-stu-id="67368-318">Click an **Operation** in the operations list that causes an error.</span></span>

    ![Válasszon ki egy műveletet](./media/data-factory-monitor-manage-pipelines/select-operation.png)
5. <span data-ttu-id="67368-320">Kattintson egy **hiba** esemény a hiba részleteinek megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="67368-320">Click an **Error** event to see details about the error.</span></span>

    ![Esemény hiba](./media/data-factory-monitor-manage-pipelines/operation-error-event.png)

<span data-ttu-id="67368-322">Lásd: [Azure Insight parancsmagok](https://msdn.microsoft.com/library/mt282452.aspx) PowerShell-parancsmagok, amelyekkel hozzáadása, az beszerzése, vagy távolítsa el a riasztásokat.</span><span class="sxs-lookup"><span data-stu-id="67368-322">See [Azure Insight cmdlets](https://msdn.microsoft.com/library/mt282452.aspx) for PowerShell cmdlets that you can use to add, get, or remove alerts.</span></span> <span data-ttu-id="67368-323">Néhány példa a használatával a **Get-AlertRule** parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="67368-323">Here are a few examples of using the **Get-AlertRule** cmdlet:</span></span>

```powershell
get-alertrule -res $resourceGroup -n ADFAlertsSlice -det
```

```
Properties :
Action      : Microsoft.Azure.Management.Insights.Models.RuleEmailAction
Condition   :
DataSource :
EventName             :
Category              :
Level                 :
OperationName         : RunFinished
ResourceGroupName     :
ResourceProviderName  :
ResourceId            :
Status                : Failed
SubStatus             : FailedExecution
Claims                : Microsoft.Azure.Management.Insights.Models.RuleManagementEventClaimsDataSource
Condition      :
Description : One or more of the data slices for the Azure Data Factory has failed processing.
Status      : Enabled
Name:       : ADFAlertsSlice
Tags       :
$type          : Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage
Id: /subscriptions/<subscription ID>/resourceGroups/<resource group name>/providers/microsoft.insights/alertrules/ADFAlertsSlice
Location   : West US
Name       : ADFAlertsSlice
```

```powershell
Get-AlertRule -res $resourceGroup
```
```
Properties : Microsoft.Azure.Management.Insights.Models.Rule
Tags       : {[$type, Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage]}
Id         : /subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/microsoft.insights/alertrules/FailedExecutionRunsWest0
Location   : West US
Name       : FailedExecutionRunsWest0

Properties : Microsoft.Azure.Management.Insights.Models.Rule
Tags       : {[$type, Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage]}
Id         : /subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/microsoft.insights/alertrules/FailedExecutionRunsWest3
Location   : West US
Name       : FailedExecutionRunsWest3
```

```powershell
Get-AlertRule -res $resourceGroup -Name FailedExecutionRunsWest0
```

```
Properties : Microsoft.Azure.Management.Insights.Models.Rule
Tags       : {[$type, Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage]}
Id         : /subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/microsoft.insights/alertrules/FailedExecutionRunsWest0
Location   : West US
Name       : FailedExecutionRunsWest0
```

<span data-ttu-id="67368-324">A következő parancsokat get-help, részleteket, és a Get-AlertRule parancsmag példák.</span><span class="sxs-lookup"><span data-stu-id="67368-324">Run the following get-help commands to see details and examples for the Get-AlertRule cmdlet.</span></span>

```powershell
get-help Get-AlertRule -detailed
```

```powershell
get-help Get-AlertRule -examples
```


<span data-ttu-id="67368-325">Ha a riasztás előállítás események a portál panel látható, de nem kap értesítő e-mailek, ellenőrizze a beállíthat-e a megadott e-mail cím külső feladók e-maileket fogadni.</span><span class="sxs-lookup"><span data-stu-id="67368-325">If you see the alert generation events on the portal blade but you don't receive email notifications, check whether the email address that is specified is set to receive emails from external senders.</span></span> <span data-ttu-id="67368-326">Az értesítő e-mailek előfordulhat, hogy le lettek tiltva az e-mail beállítások szerint.</span><span class="sxs-lookup"><span data-stu-id="67368-326">The alert emails might have been blocked by your email settings.</span></span>

### <a name="alerts-on-metrics"></a><span data-ttu-id="67368-327">Riasztások a metrikák</span><span class="sxs-lookup"><span data-stu-id="67368-327">Alerts on metrics</span></span>
<span data-ttu-id="67368-328">A Data Factory különböző metrikák rögzítése, és létre riasztásokat mérőszámokat.</span><span class="sxs-lookup"><span data-stu-id="67368-328">In Data Factory, you can capture various metrics and create alerts on metrics.</span></span> <span data-ttu-id="67368-329">Figyelheti, és a következő metrikákat a szeletek értesítést létrehozni az adat-előállítóban:</span><span class="sxs-lookup"><span data-stu-id="67368-329">You can monitor and create alerts on the following metrics for the slices in your data factory:</span></span>

* <span data-ttu-id="67368-330">**Nem sikerült fut.**</span><span class="sxs-lookup"><span data-stu-id="67368-330">**Failed Runs**</span></span>
* <span data-ttu-id="67368-331">**Sikeres futtatása**</span><span class="sxs-lookup"><span data-stu-id="67368-331">**Successful Runs**</span></span>

<span data-ttu-id="67368-332">A metrikák hasznosak, és segítségével áttekintheti a teljes és a sikertelen futtatása az adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="67368-332">These metrics are useful and help you to get an overview of overall failed and successful runs in the data factory.</span></span> <span data-ttu-id="67368-333">Minden alkalommal, amikor nincs szelet futtató metrikák kibocsátott.</span><span class="sxs-lookup"><span data-stu-id="67368-333">Metrics are emitted every time there is a slice run.</span></span> <span data-ttu-id="67368-334">Az óra elején metrikákat összesítve, és leküldeni a tárfiók.</span><span class="sxs-lookup"><span data-stu-id="67368-334">At the beginning of the hour, these metrics are aggregated and pushed to your storage account.</span></span> <span data-ttu-id="67368-335">Metrikák engedélyezéséhez állítsa be a tárfiók.</span><span class="sxs-lookup"><span data-stu-id="67368-335">To enable metrics, set up a storage account.</span></span>

#### <a name="enable-metrics"></a><span data-ttu-id="67368-336">Metrikák engedélyezése</span><span class="sxs-lookup"><span data-stu-id="67368-336">Enable metrics</span></span>
<span data-ttu-id="67368-337">Ahhoz, hogy a metrikákat, kattintson a következők egyikét a **adat-előállító** panel:</span><span class="sxs-lookup"><span data-stu-id="67368-337">To enable metrics, click the following from the **Data Factory** blade:</span></span>

<span data-ttu-id="67368-338">**Figyelési** > **metrika** > **diagnosztikai beállítások** > **diagnosztika**</span><span class="sxs-lookup"><span data-stu-id="67368-338">**Monitoring** > **Metric** > **Diagnostic settings** > **Diagnostics**</span></span>

![Diagnosztika hivatkozás](./media/data-factory-monitor-manage-pipelines/diagnostics-link.png)

<span data-ttu-id="67368-340">A a **diagnosztika** panelen kattintson a **a**, válassza ki a tárfiókot, és kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="67368-340">On the **Diagnostics** blade, click **On**, select the storage account, and click **Save**.</span></span>

![Diagnosztika panel](./media/data-factory-monitor-manage-pipelines/diagnostics-blade.png)

<span data-ttu-id="67368-342">A mérni kívánt látható legyen az akár egy óráig is eltarthat a **figyelés** panel mert metrikák összesítési óránként történik.</span><span class="sxs-lookup"><span data-stu-id="67368-342">It might take up to one hour for the metrics to be visible on the **Monitoring** blade because metrics aggregation happens hourly.</span></span>

### <a name="set-up-an-alert-on-metrics"></a><span data-ttu-id="67368-343">A metrikák riasztás beállítása</span><span class="sxs-lookup"><span data-stu-id="67368-343">Set up an alert on metrics</span></span>
<span data-ttu-id="67368-344">Kattintson a **adat-előállító metrikák** csempén:</span><span class="sxs-lookup"><span data-stu-id="67368-344">Click the **Data Factory metrics** tile:</span></span>

![Data factory metrikák csempe](./media/data-factory-monitor-manage-pipelines/data-factory-metrics-tile.png)

<span data-ttu-id="67368-346">Az a **metrika** panelen kattintson a **+ Hozzáadás riasztás** az eszköztáron.</span><span class="sxs-lookup"><span data-stu-id="67368-346">On the **Metric** blade, click **+ Add alert** on the toolbar.</span></span>
<span data-ttu-id="67368-347">![Data factory metrika panel > riasztás hozzáadása](./media/data-factory-monitor-manage-pipelines/add-alert.png)</span><span class="sxs-lookup"><span data-stu-id="67368-347">![Data factory metric blade > Add alert](./media/data-factory-monitor-manage-pipelines/add-alert.png)</span></span>

<span data-ttu-id="67368-348">Az a **riasztási szabály felvétele** lapon tegye a következőket, majd kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="67368-348">On the **Add an alert rule** page, do the following steps, and click **OK**.</span></span>

* <span data-ttu-id="67368-349">Adja meg a riasztás nevét (például: "riasztás sikertelen").</span><span class="sxs-lookup"><span data-stu-id="67368-349">Enter a name for the alert (example: "failed alert").</span></span>
* <span data-ttu-id="67368-350">A riasztás leírásának (Példa: "e-mail küldési hiba esetén").</span><span class="sxs-lookup"><span data-stu-id="67368-350">Enter a description for the alert (example: "send an email when a failure occurs").</span></span>
* <span data-ttu-id="67368-351">Jelöljön ki egy metrikát ("Futtatása sikertelen" vs. "Sikeres futtatása").</span><span class="sxs-lookup"><span data-stu-id="67368-351">Select a metric ("Failed Runs" vs. "Successful Runs").</span></span>
* <span data-ttu-id="67368-352">Adjon meg egy feltételt és egy küszöbértéket.</span><span class="sxs-lookup"><span data-stu-id="67368-352">Specify a condition and a threshold value.</span></span>   
* <span data-ttu-id="67368-353">Adja meg, mennyi ideig.</span><span class="sxs-lookup"><span data-stu-id="67368-353">Specify the period of time.</span></span>
* <span data-ttu-id="67368-354">Adja meg, hogy kell-e e-mailt küldeni a tulajdonosok, közreműködőknek és olvasóknak.</span><span class="sxs-lookup"><span data-stu-id="67368-354">Specify whether an email should be sent to owners, contributors, and readers.</span></span>

![Data factory metrika panel > Hozzáadás riasztási szabálya](./media/data-factory-monitor-manage-pipelines/add-an-alert-rule.png)

<span data-ttu-id="67368-356">A riasztási szabályt hozzáadása sikeresen, a panel bezárása után, és az új riasztás megtekintheti után a **metrika** panelen.</span><span class="sxs-lookup"><span data-stu-id="67368-356">After the alert rule is added successfully, the blade closes and you see the new alert on the **Metric** blade.</span></span>

![Data factory metrika panel > Új értesítés hozzáadása](./media/data-factory-monitor-manage-pipelines/failed-alert-in-metric-blade.png)

<span data-ttu-id="67368-358">Emellett meg kell jelennie a riasztások számának a **riasztási szabályok** csempére.</span><span class="sxs-lookup"><span data-stu-id="67368-358">You should also see the number of alerts in the **Alert rules** tile.</span></span> <span data-ttu-id="67368-359">Kattintson a **riasztási szabályok** csempére.</span><span class="sxs-lookup"><span data-stu-id="67368-359">Click the **Alert rules** tile.</span></span>

![Data factory metrika panelről – riasztási szabályok](./media/data-factory-monitor-manage-pipelines/alert-rules-tile-rules.png)

<span data-ttu-id="67368-361">A a **riasztások, szabályok** panelen láthatja, hogy minden meglévő riasztást.</span><span class="sxs-lookup"><span data-stu-id="67368-361">On the **Alerts rules** blade, you see any existing alerts.</span></span> <span data-ttu-id="67368-362">Riasztás hozzáadásához kattintson **riasztás hozzáadása** az eszköztáron.</span><span class="sxs-lookup"><span data-stu-id="67368-362">To add an alert, click **Add alert** on the toolbar.</span></span>

![A riasztási szabályok panel](./media/data-factory-monitor-manage-pipelines/alert-rules-blade.png)

### <a name="alert-notifications"></a><span data-ttu-id="67368-364">Riasztási értesítések</span><span class="sxs-lookup"><span data-stu-id="67368-364">Alert notifications</span></span>
<span data-ttu-id="67368-365">Miután a riasztási szabály megegyezik a feltételt, egy e-mailt, amely szerint a riasztás aktiválása szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="67368-365">After the alert rule matches the condition, you should get an email that says the alert is activated.</span></span> <span data-ttu-id="67368-366">Miután a probléma megoldódott, és a riasztási feltétel már nem egyezik, kap egy e-mailt, amely szerint a riasztás fel lett oldva.</span><span class="sxs-lookup"><span data-stu-id="67368-366">After the issue is resolved and the alert condition doesn’t match anymore, you get an email that says the alert is resolved.</span></span>

<span data-ttu-id="67368-367">Ez a viselkedés eltér események hol egy értesítést küld az összes hiba, amely jogosult riasztási szabályt.</span><span class="sxs-lookup"><span data-stu-id="67368-367">This behavior is different than events where a notification is sent on every failure that an alert rule qualifies for.</span></span>

### <a name="deploy-alerts-by-using-powershell"></a><span data-ttu-id="67368-368">Riasztások telepítését a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="67368-368">Deploy alerts by using PowerShell</span></span>
<span data-ttu-id="67368-369">Riasztások metrikáihoz az események ugyanúgy telepítheti.</span><span class="sxs-lookup"><span data-stu-id="67368-369">You can deploy alerts for metrics the same way that you do for events.</span></span>

<span data-ttu-id="67368-370">**A riasztás definíciója**</span><span class="sxs-lookup"><span data-stu-id="67368-370">**Alert definition**</span></span>

```JSON
{
    "contentVersion" : "1.0.0.0",
    "$schema" : "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "parameters" : {},
    "resources" : [
    {
            "name" : "FailedRunsGreaterThan5",
            "type" : "microsoft.insights/alertrules",
            "apiVersion" : "2014-04-01",
            "location" : "East US",
            "properties" : {
                "name" : "FailedRunsGreaterThan5",
                "description" : "Failed Runs greater than 5",
                "isEnabled" : true,
                "condition" : {
                    "$type" : "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.ThresholdRuleCondition, Microsoft.WindowsAzure.Management.Mon.Client",
                    "odata.type" : "Microsoft.Azure.Management.Insights.Models.ThresholdRuleCondition",
                    "dataSource" : {
                        "$type" : "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.RuleMetricDataSource, Microsoft.WindowsAzure.Management.Mon.Client",
                        "odata.type" : "Microsoft.Azure.Management.Insights.Models.RuleMetricDataSource",
                        "resourceUri" : "/SUBSCRIPTIONS/<subscriptionId>/RESOURCEGROUPS/<resourceGroupName
>/PROVIDERS/MICROSOFT.DATAFACTORY/DATAFACTORIES/<dataFactoryName>",
                        "metricName" : "FailedRuns"
                    },
                    "threshold" : 5.0,
                    "windowSize" : "PT3H",
                    "timeAggregation" : "Total"
                },
                "action" : {
                    "$type" : "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.RuleEmailAction, Microsoft.WindowsAzure.Management.Mon.Client",
                    "odata.type" : "Microsoft.Azure.Management.Insights.Models.RuleEmailAction",
                    "customEmails" : ["abhinav.gpt@live.com"]
                }
            }
        }
    ]
}
```

<span data-ttu-id="67368-371">Cserélje le *subscriptionId*, *resourceGroupName*, és *dataFactoryName* a mintában a megfelelő értékeket.</span><span class="sxs-lookup"><span data-stu-id="67368-371">Replace *subscriptionId*, *resourceGroupName*, and *dataFactoryName* in the sample with appropriate values.</span></span>

<span data-ttu-id="67368-372">*metricName* jelenleg csak a két érték támogatja:</span><span class="sxs-lookup"><span data-stu-id="67368-372">*metricName* currently supports two values:</span></span>

* <span data-ttu-id="67368-373">FailedRuns</span><span class="sxs-lookup"><span data-stu-id="67368-373">FailedRuns</span></span>
* <span data-ttu-id="67368-374">SuccessfulRuns</span><span class="sxs-lookup"><span data-stu-id="67368-374">SuccessfulRuns</span></span>

<span data-ttu-id="67368-375">**A riasztás telepítése**</span><span class="sxs-lookup"><span data-stu-id="67368-375">**Deploy the alert**</span></span>

<span data-ttu-id="67368-376">A riasztás telepítéséhez használja az Azure PowerShell-parancsmag **New-AzureRmResourceGroupDeployment**, a következő példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="67368-376">To deploy the alert, use the Azure PowerShell cmdlet **New-AzureRmResourceGroupDeployment**, as shown in the following example:</span></span>

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName adf -TemplateFile .\FailedRunsGreaterThan5.json
```

<span data-ttu-id="67368-377">Üzenet a sikeres telepítés után a következő kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="67368-377">You should see following message after a successful deployment:</span></span>

```
VERBOSE: 12:52:47 PM - Template is valid.
VERBOSE: 12:52:48 PM - Create template deployment 'FailedRunsGreaterThan5'.
VERBOSE: 12:52:55 PM - Resource microsoft.insights/alertrules 'FailedRunsGreaterThan5' provisioning status is succeeded


DeploymentName    : FailedRunsGreaterThan5
ResourceGroupName : adf
ProvisioningState : Succeeded
Timestamp         : 7/27/2015 7:52:56 PM
Mode              : Incremental
TemplateLink      :
Parameters        :
Outputs           
```

<span data-ttu-id="67368-378">Használhatja a **Add-AlertRule** parancsmag központi telepítése a riasztási szabályt.</span><span class="sxs-lookup"><span data-stu-id="67368-378">You can also use the **Add-AlertRule** cmdlet to deploy an alert rule.</span></span> <span data-ttu-id="67368-379">Tekintse meg a [Add-AlertRule](https://msdn.microsoft.com/library/mt282468.aspx) részletek és példákat a témakör.</span><span class="sxs-lookup"><span data-stu-id="67368-379">See the [Add-AlertRule](https://msdn.microsoft.com/library/mt282468.aspx) topic for details and examples.</span></span>  

## <a name="move-a-data-factory-to-a-different-resource-group-or-subscription"></a><span data-ttu-id="67368-380">Egy adat-előállító áthelyezése egy másik erőforráscsoportban vagy előfizetés</span><span class="sxs-lookup"><span data-stu-id="67368-380">Move a data factory to a different resource group or subscription</span></span>
<span data-ttu-id="67368-381">Áthelyezheti egy adat-előállító egy másik erőforráscsoportban vagy egy másik előfizetésben található használatával a **áthelyezése** sáv gombjára a data factory kezdőlapján parancsot.</span><span class="sxs-lookup"><span data-stu-id="67368-381">You can move a data factory to a different resource group or a different subscription by using the **Move** command bar button on the home page of your data factory.</span></span>

![Adat-előállító áthelyezése](./media/data-factory-monitor-manage-pipelines/MoveDataFactory.png)

<span data-ttu-id="67368-383">Át is helyezheti a kapcsolódó erőforrások (például a riasztások az adat-előállítóban társított), és az adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="67368-383">You can also move any related resources (such as alerts that are associated with the data factory), along with the data factory.</span></span>

![Források párbeszédpanelt](./media/data-factory-monitor-manage-pipelines/MoveResources.png)
