---
title: "aaaMonitor és folyamatok kezelése hello Azure-portál és a PowerShell használatával |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse hello Azure-portál és az Azure PowerShell toomonitor és hello Azure adat-előállítók és a létrehozott folyamatok kezelése."
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
ms.openlocfilehash: a8d3c7943e79450895ff754f06a37fdad1cbef92
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-and-manage-azure-data-factory-pipelines-by-using-hello-azure-portal-and-powershell"></a><span data-ttu-id="2348b-103">Figyelheti és kezelheti az Azure Data Factory folyamatok hello Azure-portál és a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="2348b-103">Monitor and manage Azure Data Factory pipelines by using hello Azure portal and PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="2348b-104">Az Azure portálon vagy az Azure PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="2348b-104">Using Azure portal/Azure PowerShell</span></span>](data-factory-monitor-manage-pipelines.md)
> * [<span data-ttu-id="2348b-105">Használatával figyelése és a felügyeleti alkalmazás</span><span class="sxs-lookup"><span data-stu-id="2348b-105">Using Monitoring and Management app</span></span>](data-factory-monitor-manage-app.md)


> [!IMPORTANT]
> <span data-ttu-id="2348b-106">hello figyelési & a felügyeleti alkalmazás figyelése és az adatok folyamatok kezelését és hibaelhárítását probléma merül fel egy jobb támogatást biztosít.</span><span class="sxs-lookup"><span data-stu-id="2348b-106">hello monitoring & management application provides a better support for monitoring and managing your data pipelines, and troubleshooting any issues.</span></span> <span data-ttu-id="2348b-107">Hello alkalmazás használatával kapcsolatos részletekért lásd: [felügyeletéhez és kezeléséhez az adat-előállító adatcsatornák hello figyelés és felügyelet alkalmazással](data-factory-monitor-manage-app.md).</span><span class="sxs-lookup"><span data-stu-id="2348b-107">For details about using hello application, see [monitor and manage Data Factory pipelines by using hello Monitoring and Management app](data-factory-monitor-manage-app.md).</span></span> 


<span data-ttu-id="2348b-108">Ez a cikk ismerteti, hogyan toomonitor, kezelése és a folyamatok hibakeresése az Azure-portál és a PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="2348b-108">This article describes how toomonitor, manage, and debug your pipelines by using Azure portal and PowerShell.</span></span> <span data-ttu-id="2348b-109">hello cikk információkat is biztosít toocreate riasztások és a get hibákkal kapcsolatos értesítést.</span><span class="sxs-lookup"><span data-stu-id="2348b-109">hello article also provides information on how toocreate alerts and get notified about failures.</span></span>

## <a name="understand-pipelines-and-activity-states"></a><span data-ttu-id="2348b-110">Adatcsatornák és a tevékenység állapota</span><span class="sxs-lookup"><span data-stu-id="2348b-110">Understand pipelines and activity states</span></span>
<span data-ttu-id="2348b-111">Hello Azure-portál használatával a következő műveletek végezhetők el:</span><span class="sxs-lookup"><span data-stu-id="2348b-111">By using hello Azure portal, you can:</span></span>

* <span data-ttu-id="2348b-112">Megtekintheti a data factory mint ábrázoló diagram.</span><span class="sxs-lookup"><span data-stu-id="2348b-112">View your data factory as a diagram.</span></span>
* <span data-ttu-id="2348b-113">A feldolgozási soros tevékenységek megtekintése.</span><span class="sxs-lookup"><span data-stu-id="2348b-113">View activities in a pipeline.</span></span>
* <span data-ttu-id="2348b-114">Bemeneti és kimeneti adatkészletek megtekintése.</span><span class="sxs-lookup"><span data-stu-id="2348b-114">View input and output datasets.</span></span>

<span data-ttu-id="2348b-115">Ez a szakasz azt is ismerteti, hogyan dataset szelet átkerül egy állapot tooanother állapotból.</span><span class="sxs-lookup"><span data-stu-id="2348b-115">This section also describes how a dataset slice transitions from one state tooanother state.</span></span>   

### <a name="navigate-tooyour-data-factory"></a><span data-ttu-id="2348b-116">Keresse meg az adat-előállító tooyour</span><span class="sxs-lookup"><span data-stu-id="2348b-116">Navigate tooyour data factory</span></span>
1. <span data-ttu-id="2348b-117">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="2348b-117">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="2348b-118">Kattintson a **adat-előállítók** hello menü hello bal oldalon.</span><span class="sxs-lookup"><span data-stu-id="2348b-118">Click **Data factories** on hello menu on hello left.</span></span> <span data-ttu-id="2348b-119">Ha nem látja, kattintson a **további szolgáltatások >**, és kattintson a **adat-előállítók** alatt hello **ESZKÖZINTELLIGENCIA + ANALITIKA** kategóriát.</span><span class="sxs-lookup"><span data-stu-id="2348b-119">If you don't see it, click **More services >**, and then click **Data factories** under hello **INTELLIGENCE + ANALYTICS** category.</span></span>

   ![Keresse meg az összes > adat-előállítók](./media/data-factory-monitor-manage-pipelines/browseall-data-factories.png)
3. <span data-ttu-id="2348b-121">A hello **adat-előállítók** panelen, jelölje be hello adat-előállító kíváncsiak vagyunk.</span><span class="sxs-lookup"><span data-stu-id="2348b-121">On hello **Data factories** blade, select hello data factory that you're interested in.</span></span>

    ![Válassza ki az adat-előállító](./media/data-factory-monitor-manage-pipelines/select-data-factory.png)

   <span data-ttu-id="2348b-123">Hello kezdőlapját hello adat-előállító kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="2348b-123">You should see hello home page for hello data factory.</span></span>

   ![Data factory panel](./media/data-factory-monitor-manage-pipelines/data-factory-blade.png)

#### <a name="diagram-view-of-your-data-factory"></a><span data-ttu-id="2348b-125">A data factory a diagram nézet</span><span class="sxs-lookup"><span data-stu-id="2348b-125">Diagram view of your data factory</span></span>
<span data-ttu-id="2348b-126">Hello **Diagram** egy adat-előállító nézete egytáblás, üveghatású toomonitor biztosít, és hello data factory és az eszközök kezelése.</span><span class="sxs-lookup"><span data-stu-id="2348b-126">hello **Diagram** view of a data factory provides a single pane of glass toomonitor and manage hello data factory and its assets.</span></span> <span data-ttu-id="2348b-127">toosee hello **Diagram** , a data factory megtekintéséhez kattintson az **Diagram** hello kezdőlap hello adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="2348b-127">toosee hello **Diagram** view of your data factory, click **Diagram** on hello home page for hello data factory.</span></span>

![Diagramnézet](./media/data-factory-monitor-manage-pipelines/diagram-view.png)

<span data-ttu-id="2348b-129">Nagyítás, Kicsinyítés, nagyítás toofit, a Nagyítás too100 %, a zárolás hello hello diagram elrendezését, és adatcsatornákat és adathalmazokat automatikus formázása.</span><span class="sxs-lookup"><span data-stu-id="2348b-129">You can zoom in, zoom out, zoom toofit, zoom too100%, lock hello layout of hello diagram, and automatically position pipelines and datasets.</span></span> <span data-ttu-id="2348b-130">Hello adatok Leszármaztatás információ is megtekinthető (Ez azt jelenti, hogy a kijelölt elemek felsőbb és alsóbb rétegbeli elemek megjelenítése).</span><span class="sxs-lookup"><span data-stu-id="2348b-130">You can also see hello data lineage information (that is, show upstream and downstream items of selected items).</span></span>

### <a name="activities-inside-a-pipeline"></a><span data-ttu-id="2348b-131">Egy folyamat belüli tevékenységek</span><span class="sxs-lookup"><span data-stu-id="2348b-131">Activities inside a pipeline</span></span>
1. <span data-ttu-id="2348b-132">Kattintson a jobb gombbal a hello folyamat, és kattintson **nyitott folyamat** toosee hello szereplő összes tevékenységnek a következő feldolgozási sorban, valamint bemeneti és kimeneti adatkészletek hello tevékenységekhez.</span><span class="sxs-lookup"><span data-stu-id="2348b-132">Right-click hello pipeline, and then click **Open pipeline** toosee all activities in hello pipeline, along with input and output datasets for hello activities.</span></span> <span data-ttu-id="2348b-133">Ez a szolgáltatás akkor hasznos, ha a feldolgozási sor egynél több tevékenységet tartalmaz, és azt szeretné, hogy toounderstand hello működési Leszármaztatás egyetlen folyamat.</span><span class="sxs-lookup"><span data-stu-id="2348b-133">This feature is useful when your pipeline includes more than one activity and you want toounderstand hello operational lineage of a single pipeline.</span></span>

    ![Folyamat megnyitása menü](./media/data-factory-monitor-manage-pipelines/open-pipeline-menu.png)     
2. <span data-ttu-id="2348b-135">A következő példa hello a másolási tevékenység során hello folyamat a bemeneti és egy kimeneti látható.</span><span class="sxs-lookup"><span data-stu-id="2348b-135">In hello following example, you see a copy activity in hello pipeline with an input and an output.</span></span> 

    ![Egy folyamat belüli tevékenységek](./media/data-factory-monitor-manage-pipelines/activities-inside-pipeline.png)
3. <span data-ttu-id="2348b-137">Hello kattintva léphet vissza toohello kezdőlapján hello adat-előállító **adat-előállító** hello navigációs hello bal felső sarokban lévő hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="2348b-137">You can navigate back toohello home page of hello data factory by clicking hello **Data factory** link in hello breadcrumb at hello top-left corner.</span></span>

    ![Keresse meg a visszafelé toodata gyári](./media/data-factory-monitor-manage-pipelines/navigate-back-to-data-factory.png)

### <a name="view-hello-state-of-each-activity-inside-a-pipeline"></a><span data-ttu-id="2348b-139">Minden tevékenység egy folyamat belül hello állapotának megtekintése</span><span class="sxs-lookup"><span data-stu-id="2348b-139">View hello state of each activity inside a pipeline</span></span>
<span data-ttu-id="2348b-140">Egy tevékenység jelenlegi állapotában hello megtekintéséhez hello adatkészletek hello tevékenység által gyártott bármelyikének hello állapotának megtekintése.</span><span class="sxs-lookup"><span data-stu-id="2348b-140">You can view hello current state of an activity by viewing hello status of any of hello datasets that are produced by hello activity.</span></span>

<span data-ttu-id="2348b-141">Ehhez kattintson duplán a hello **OutputBlobTable** a hello **Diagram**, megtekintheti az összes hello szelet, amely különböző tevékenység fut egy folyamat belül hozzák létre.</span><span class="sxs-lookup"><span data-stu-id="2348b-141">By double-clicking hello **OutputBlobTable** in hello **Diagram**, you can see all hello slices that are produced by different activity runs inside a pipeline.</span></span> <span data-ttu-id="2348b-142">Láthatja, hogy hello másolási tevékenység már sikeresen futott az hello utolsó nyolc óra és hello hello szeletek előállított **készen** állapotát.</span><span class="sxs-lookup"><span data-stu-id="2348b-142">You can see that hello copy activity ran successfully for hello last eight hours and produced hello slices in hello **Ready** state.</span></span>  

![Hello folyamatának állapota](./media/data-factory-monitor-manage-pipelines/state-of-pipeline.png)

<span data-ttu-id="2348b-144">hello dataset szeletek hello adat-előállítóban hello a következő állapotok egyike lehet:</span><span class="sxs-lookup"><span data-stu-id="2348b-144">hello dataset slices in hello data factory can have one of hello following statuses:</span></span>

<table>
<tr>
    <th align="left"><span data-ttu-id="2348b-145">Állapot</span><span class="sxs-lookup"><span data-stu-id="2348b-145">State</span></span></th><th align="left"><span data-ttu-id="2348b-146">Alállapota</span><span class="sxs-lookup"><span data-stu-id="2348b-146">Substate</span></span></th><th align="left"><span data-ttu-id="2348b-147">Leírás</span><span class="sxs-lookup"><span data-stu-id="2348b-147">Description</span></span></th>
</tr>
<tr>
    <td rowspan="8"><span data-ttu-id="2348b-148">Várakozás</span><span class="sxs-lookup"><span data-stu-id="2348b-148">Waiting</span></span></td><td><span data-ttu-id="2348b-149">ScheduleTime</span><span class="sxs-lookup"><span data-stu-id="2348b-149">ScheduleTime</span></span></td><td><span data-ttu-id="2348b-150">hello szelet toorun hello ideje még nem érkezett.</span><span class="sxs-lookup"><span data-stu-id="2348b-150">hello time hasn't come for hello slice toorun.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="2348b-151">DatasetDependencies</span><span class="sxs-lookup"><span data-stu-id="2348b-151">DatasetDependencies</span></span></td><td><span data-ttu-id="2348b-152">hello fölérendelt függőségek nem állnak készen.</span><span class="sxs-lookup"><span data-stu-id="2348b-152">hello upstream dependencies aren't ready.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="2348b-153">ComputeResources</span><span class="sxs-lookup"><span data-stu-id="2348b-153">ComputeResources</span></span></td><td><span data-ttu-id="2348b-154">hello számítási erőforrások nem érhetők el.</span><span class="sxs-lookup"><span data-stu-id="2348b-154">hello compute resources aren't available.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="2348b-155">ConcurrencyLimit</span><span class="sxs-lookup"><span data-stu-id="2348b-155">ConcurrencyLimit</span></span></td> <td><span data-ttu-id="2348b-156">Minden hello Tevékenységpéldány elfoglalva más szeletek futtatásával.</span><span class="sxs-lookup"><span data-stu-id="2348b-156">All hello activity instances are busy running other slices.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="2348b-157">ActivityResume</span><span class="sxs-lookup"><span data-stu-id="2348b-157">ActivityResume</span></span></td><td><span data-ttu-id="2348b-158">hello tevékenység szüneteltetve van, és csak hello tevékenység folytatásakor hello szeletek futtatható.</span><span class="sxs-lookup"><span data-stu-id="2348b-158">hello activity is paused and can't run hello slices until hello activity is resumed.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="2348b-159">Próbálja meg újra</span><span class="sxs-lookup"><span data-stu-id="2348b-159">Retry</span></span></td><td><span data-ttu-id="2348b-160">Tevékenység végrehajtási lesz hajtva.</span><span class="sxs-lookup"><span data-stu-id="2348b-160">Activity execution is being retried.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="2348b-161">Ellenőrzés</span><span class="sxs-lookup"><span data-stu-id="2348b-161">Validation</span></span></td><td><span data-ttu-id="2348b-162">Érvényesítés még a még nem indult el.</span><span class="sxs-lookup"><span data-stu-id="2348b-162">Validation hasn't started yet.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="2348b-163">ValidationRetry</span><span class="sxs-lookup"><span data-stu-id="2348b-163">ValidationRetry</span></span></td><td><span data-ttu-id="2348b-164">Érvényesítési újrapróbált várakozási toobe.</span><span class="sxs-lookup"><span data-stu-id="2348b-164">Validation is waiting toobe retried.</span></span></td>
</tr>
<tr>
<tr>
<td rowspan="2"><span data-ttu-id="2348b-165">Esetbejegyzések</span><span class="sxs-lookup"><span data-stu-id="2348b-165">InProgress</span></span></td><td><span data-ttu-id="2348b-166">Ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="2348b-166">Validating</span></span></td><td><span data-ttu-id="2348b-167">Ellenőrzése folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="2348b-167">Validation is in progress.</span></span></td>
</tr>
<td>-</td>
<td><span data-ttu-id="2348b-168">hello szelet feldolgozása folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="2348b-168">hello slice is being processed.</span></span></td>
</tr>
<tr>
<td rowspan="4"><span data-ttu-id="2348b-169">Nem sikerült</span><span class="sxs-lookup"><span data-stu-id="2348b-169">Failed</span></span></td><td><span data-ttu-id="2348b-170">Időtúllépésbe került</span><span class="sxs-lookup"><span data-stu-id="2348b-170">TimedOut</span></span></td><td><span data-ttu-id="2348b-171">hello tevékenység végrehajtási hello tevékenység által megengedett érték időt vett igénybe.</span><span class="sxs-lookup"><span data-stu-id="2348b-171">hello activity execution took longer than what is allowed by hello activity.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="2348b-172">Törölve</span><span class="sxs-lookup"><span data-stu-id="2348b-172">Canceled</span></span></td><td><span data-ttu-id="2348b-173">hello szelet felhasználói művelet megszakította.</span><span class="sxs-lookup"><span data-stu-id="2348b-173">hello slice was canceled by user action.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="2348b-174">Ellenőrzés</span><span class="sxs-lookup"><span data-stu-id="2348b-174">Validation</span></span></td><td><span data-ttu-id="2348b-175">Sikertelen volt.</span><span class="sxs-lookup"><span data-stu-id="2348b-175">Validation has failed.</span></span></td>
</tr>
<tr>
<td>-</td><td><span data-ttu-id="2348b-176">hello szelet toobe jön létre és/vagy érvényesítése nem sikerült.</span><span class="sxs-lookup"><span data-stu-id="2348b-176">hello slice failed toobe generated and/or validated.</span></span></td>
</tr>
<td><span data-ttu-id="2348b-177">Készen áll</span><span class="sxs-lookup"><span data-stu-id="2348b-177">Ready</span></span></td><td>-</td><td><span data-ttu-id="2348b-178">hello szelet készen áll a felhasználásra.</span><span class="sxs-lookup"><span data-stu-id="2348b-178">hello slice is ready for consumption.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="2348b-179">Kihagyva</span><span class="sxs-lookup"><span data-stu-id="2348b-179">Skipped</span></span></td><td><span data-ttu-id="2348b-180">None</span><span class="sxs-lookup"><span data-stu-id="2348b-180">None</span></span></td><td><span data-ttu-id="2348b-181">hello szelet feldolgozása folyamatban nem.</span><span class="sxs-lookup"><span data-stu-id="2348b-181">hello slice isn't being processed.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="2348b-182">None</span><span class="sxs-lookup"><span data-stu-id="2348b-182">None</span></span></td><td>-</td><td><span data-ttu-id="2348b-183">A szelet használt tooexist egy eltérő állapottal, de a rendszer visszaállította.</span><span class="sxs-lookup"><span data-stu-id="2348b-183">A slice used tooexist with a different status, but it has been reset.</span></span></td>
</tr>
</table>



<span data-ttu-id="2348b-184">A szelet bevitelének hello kattintva megtekintheti a szelet hello adatait **szeletek nemrég frissített** panelen.</span><span class="sxs-lookup"><span data-stu-id="2348b-184">You can view hello details about a slice by clicking a slice entry on hello **Recently Updated Slices** blade.</span></span>

![Szelet részletei](./media/data-factory-monitor-manage-pipelines/slice-details.png)

<span data-ttu-id="2348b-186">Ha hello szelet több alkalommal végre lett hajtva, hello több sor látható **tevékenység fut** listája.</span><span class="sxs-lookup"><span data-stu-id="2348b-186">If hello slice has been executed multiple times, you see multiple rows in hello **Activity runs** list.</span></span> <span data-ttu-id="2348b-187">Hello futtatása hello bejegyzést kattintva futtatható tevékenységgel kapcsolatos részleteket **tevékenység fut** listája.</span><span class="sxs-lookup"><span data-stu-id="2348b-187">You can view details about an activity run by clicking hello run entry in hello **Activity runs** list.</span></span> <span data-ttu-id="2348b-188">hello listán látható összes hello naplófájl, valamint egy hibaüzenet, ha van ilyen.</span><span class="sxs-lookup"><span data-stu-id="2348b-188">hello list shows all hello log files, along with an error message if there is one.</span></span> <span data-ttu-id="2348b-189">Ez a szolgáltatás akkor hasznos tooview és hibakeresési naplók a data factory tooleave nélkül.</span><span class="sxs-lookup"><span data-stu-id="2348b-189">This feature is useful tooview and debug logs without having tooleave your data factory.</span></span>

![Tevékenységfuttatás részletei](./media/data-factory-monitor-manage-pipelines/activity-run-details.png)

<span data-ttu-id="2348b-191">Ha hello szelet nem a hello **készen áll a** állapot, hello felfelé irányuló szeletek nem állnak készen, és blokkol hello aktuális szelet hello végrehajtás alatt látható **felfelé irányuló szeletek nem áll készen** listája.</span><span class="sxs-lookup"><span data-stu-id="2348b-191">If hello slice isn't in hello **Ready** state, you can see hello upstream slices that aren't ready and are blocking hello current slice from executing in hello **Upstream slices that are not ready** list.</span></span> <span data-ttu-id="2348b-192">A szolgáltatás akkor hasznos, ha a szelet **Várakozás** állapotát és azt szeretné, toounderstand hello fölérendelt függőségek, hogy a szelet hello vár a.</span><span class="sxs-lookup"><span data-stu-id="2348b-192">This feature is useful when your slice is in **Waiting** state and you want toounderstand hello upstream dependencies that hello slice is waiting on.</span></span>

![Nem kész állapotú felfelé irányuló szeletek](./media/data-factory-monitor-manage-pipelines/upstream-slices-not-ready.png)

### <a name="dataset-state-diagram"></a><span data-ttu-id="2348b-194">A DataSet állapot diagramja</span><span class="sxs-lookup"><span data-stu-id="2348b-194">Dataset state diagram</span></span>
<span data-ttu-id="2348b-195">Miután telepít egy adat-előállítót, és hello folyamatok érvényes aktív idővel rendelkeznek, hello dataset szeletek több állapotot tooanother átmenetet.</span><span class="sxs-lookup"><span data-stu-id="2348b-195">After you deploy a data factory and hello pipelines have a valid active period, hello dataset slices transition from one state tooanother.</span></span> <span data-ttu-id="2348b-196">Hello szelet állapota jelenleg a következő ábra állapot hello követi:</span><span class="sxs-lookup"><span data-stu-id="2348b-196">Currently, hello slice status follows hello following state diagram:</span></span>

![Állapotdiagram](./media/data-factory-monitor-manage-pipelines/state-diagram.png)

<span data-ttu-id="2348b-198">hello dataset állapot átmenet folyamata a data factory hello alábbi: várakozás közben a rendszer -> folyamatban lévő vagy a-folyamatban (érvényesítés) -> kész/nem sikerült.</span><span class="sxs-lookup"><span data-stu-id="2348b-198">hello dataset state transition flow in data factory is hello following: Waiting -> In-Progress/In-Progress (Validating) -> Ready/Failed.</span></span>

<span data-ttu-id="2348b-199">hello szelet elindul egy **Várakozás** állapot teljesíteni végrehajtása előtt az Előfeltételek toobe vár.</span><span class="sxs-lookup"><span data-stu-id="2348b-199">hello slice starts in a **Waiting** state, waiting for preconditions toobe met before it executes.</span></span> <span data-ttu-id="2348b-200">Ezután hello tevékenység végrehajtásának elkezdése, és hello szelet kerülnek egy **folyamatban** állapotát.</span><span class="sxs-lookup"><span data-stu-id="2348b-200">Then, hello activity starts executing, and hello slice goes into an **In-Progress** state.</span></span> <span data-ttu-id="2348b-201">hello tevékenység végrehajtási előfordulhat, hogy sikeres vagy sikertelen.</span><span class="sxs-lookup"><span data-stu-id="2348b-201">hello activity execution might succeed or fail.</span></span> <span data-ttu-id="2348b-202">hello szelet jelölésű **készen** vagy **sikertelen**hello végrehajtási hello eredménye alapján.</span><span class="sxs-lookup"><span data-stu-id="2348b-202">hello slice is marked as **Ready** or **Failed**, based on hello result of hello execution.</span></span>

<span data-ttu-id="2348b-203">Alaphelyzetbe állíthatja a hello szelet toogo újból az hello **készen** vagy **sikertelen** állapot toohello **Várakozás** állapotát.</span><span class="sxs-lookup"><span data-stu-id="2348b-203">You can reset hello slice toogo back from hello **Ready** or **Failed** state toohello **Waiting** state.</span></span> <span data-ttu-id="2348b-204">Megjelölheti a hello szelet állapota túl**kihagyása**, amely megakadályozza, hogy hello tevékenység végrehajtása, és nem a hello szelet feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="2348b-204">You can also mark hello slice state too**Skip**, which prevents hello activity from executing and not processing hello slice.</span></span>

## <a name="pause-and-resume-pipelines"></a><span data-ttu-id="2348b-205">Felfüggesztése és folytatása folyamatok</span><span class="sxs-lookup"><span data-stu-id="2348b-205">Pause and resume pipelines</span></span>
<span data-ttu-id="2348b-206">A folyamatok Azure PowerShell használatával kezelheti.</span><span class="sxs-lookup"><span data-stu-id="2348b-206">You can manage your pipelines by using Azure PowerShell.</span></span> <span data-ttu-id="2348b-207">Például szüneteltetése és folytatása folyamatok Azure PowerShell-parancsmag futtatásával.</span><span class="sxs-lookup"><span data-stu-id="2348b-207">For example, you can pause and resume pipelines by running Azure PowerShell cmdlets.</span></span> 

> [!NOTE] 
> <span data-ttu-id="2348b-208">hello diagram nézet nem támogatja a felfüggesztése és folytatása folyamatok.</span><span class="sxs-lookup"><span data-stu-id="2348b-208">hello diagram view does not support pausing and resuming pipelines.</span></span> <span data-ttu-id="2348b-209">Ha azt szeretné, hogy egy felhasználói felületet toouse, használja a hello figyeléséhez és felügyeletéhez alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="2348b-209">If you want toouse an user interface, use hello monitoring and managing application.</span></span> <span data-ttu-id="2348b-210">Hello alkalmazás használatával kapcsolatos részletekért lásd: [felügyeletéhez és kezeléséhez az adat-előállító adatcsatornák hello figyelés és felügyelet alkalmazással](data-factory-monitor-manage-app.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="2348b-210">For details about using hello application, see [monitor and manage Data Factory pipelines by using hello Monitoring and Management app](data-factory-monitor-manage-app.md) article.</span></span> 

<span data-ttu-id="2348b-211">Is szüneteltetése vagy felfüggesztjük folyamatok hello segítségével **Suspend-AzureRmDataFactoryPipeline** PowerShell-parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="2348b-211">You can pause/suspend pipelines by using hello **Suspend-AzureRmDataFactoryPipeline** PowerShell cmdlet.</span></span> <span data-ttu-id="2348b-212">Ez a parancsmag akkor hasznos, ha nem szeretné, hogy toorun a folyamatok egy probléma a megoldásáig.</span><span class="sxs-lookup"><span data-stu-id="2348b-212">This cmdlet is useful when you don’t want toorun your pipelines until an issue is fixed.</span></span> 

```powershell
Suspend-AzureRmDataFactoryPipeline [-ResourceGroupName] <String> [-DataFactoryName] <String> [-Name] <String>
```
<span data-ttu-id="2348b-213">Példa:</span><span class="sxs-lookup"><span data-stu-id="2348b-213">For example:</span></span>

```powershell
Suspend-AzureRmDataFactoryPipeline -ResourceGroupName ADF -DataFactoryName productrecgamalbox1dev -Name PartitionProductsUsagePipeline
```

<span data-ttu-id="2348b-214">Hello a probléma kijavítása a hello csővezeték, után folytathatja a felfüggesztett hello csővezeték hello a következő PowerShell-parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="2348b-214">After hello issue has been fixed with hello pipeline, you can resume hello suspended pipeline by running hello following PowerShell command:</span></span>

```powershell
Resume-AzureRmDataFactoryPipeline [-ResourceGroupName] <String> [-DataFactoryName] <String> [-Name] <String>
```
<span data-ttu-id="2348b-215">Példa:</span><span class="sxs-lookup"><span data-stu-id="2348b-215">For example:</span></span>

```powershell
Resume-AzureRmDataFactoryPipeline -ResourceGroupName ADF -DataFactoryName productrecgamalbox1dev -Name PartitionProductsUsagePipeline
```

## <a name="debug-pipelines"></a><span data-ttu-id="2348b-216">Folyamatok hibakeresése</span><span class="sxs-lookup"><span data-stu-id="2348b-216">Debug pipelines</span></span>
<span data-ttu-id="2348b-217">Az Azure Data Factory gazdag képességeket biztosít az Ön toodebug, és a folyamatok hibaelhárítása hello Azure-portál és az Azure PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="2348b-217">Azure Data Factory provides rich capabilities for you toodebug and troubleshoot pipelines by using hello Azure portal and Azure PowerShell.</span></span>

> <span data-ttu-id="2348b-218">[! Vegye figyelembe} sokkal könnyebben használatával hibák hello figyelési tootroubleshot & felügyeleti alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="2348b-218">[!NOTE} It is much easier tootroubleshot errors using hello Monitoring & Management App.</span></span> <span data-ttu-id="2348b-219">Hello alkalmazás használatával kapcsolatos részletekért lásd: [felügyeletéhez és kezeléséhez az adat-előállító adatcsatornák hello figyelés és felügyelet alkalmazással](data-factory-monitor-manage-app.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="2348b-219">For details about using hello application, see [monitor and manage Data Factory pipelines by using hello Monitoring and Management app](data-factory-monitor-manage-app.md) article.</span></span> 

### <a name="find-errors-in-a-pipeline"></a><span data-ttu-id="2348b-220">Hiba található egy folyamatot</span><span class="sxs-lookup"><span data-stu-id="2348b-220">Find errors in a pipeline</span></span>
<span data-ttu-id="2348b-221">Hello tevékenységfuttatási egy folyamat sikertelen lesz, hello dataset hello folyamat által létrehozott van hello hibája miatt hibás állapotú.</span><span class="sxs-lookup"><span data-stu-id="2348b-221">If hello activity run fails in a pipeline, hello dataset that is produced by hello pipeline is in an error state because of hello failure.</span></span> <span data-ttu-id="2348b-222">Hibakeresés, és az Azure Data Factory hibák elhárítása hello a következő módszerek használatával.</span><span class="sxs-lookup"><span data-stu-id="2348b-222">You can debug and troubleshoot errors in Azure Data Factory by using hello following methods.</span></span>

#### <a name="use-hello-azure-portal-toodebug-an-error"></a><span data-ttu-id="2348b-223">Az Azure portál toodebug hiba hello használata</span><span class="sxs-lookup"><span data-stu-id="2348b-223">Use hello Azure portal toodebug an error</span></span>
1. <span data-ttu-id="2348b-224">A hello **tábla** panelen hello probléma szeletre hello rendelkező **állapot** túl beállítása**sikertelen**.</span><span class="sxs-lookup"><span data-stu-id="2348b-224">On hello **Table** blade, click hello problem slice that has hello **Status** set too**Failed**.</span></span>

   ![Probléma szelet tábla panelről](./media/data-factory-monitor-manage-pipelines/table-blade-with-error.png)
2. <span data-ttu-id="2348b-226">A hello **adatszelet** panelen kattintson a hello tevékenység futtatása sikertelen.</span><span class="sxs-lookup"><span data-stu-id="2348b-226">On hello **Data slice** blade, click hello activity run that failed.</span></span>

   ![Hiba történt az adatszelet](./media/data-factory-monitor-manage-pipelines/dataslice-with-error.png)
3. <span data-ttu-id="2348b-228">A hello **tevékenység fut részletek** panelen letöltheti a hello HDInsight feldolgozási társított hello fájlokat.</span><span class="sxs-lookup"><span data-stu-id="2348b-228">On hello **Activity run details** blade, you can download hello files that are associated with hello HDInsight processing.</span></span> <span data-ttu-id="2348b-229">Kattintson a **letöltése** állapot/stderr toodownload hello hiba naplófájlt, amely hello hiba részleteit tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="2348b-229">Click **Download** for Status/stderr toodownload hello error log file that contains details about hello error.</span></span>

   ![A következő hiba tevékenységfuttatási részleteit megjelenítő panelen](./media/data-factory-monitor-manage-pipelines/activity-run-details-with-error.png)     

#### <a name="use-powershell-toodebug-an-error"></a><span data-ttu-id="2348b-231">Használjon PowerShell toodebug hiba</span><span class="sxs-lookup"><span data-stu-id="2348b-231">Use PowerShell toodebug an error</span></span>
1. <span data-ttu-id="2348b-232">Indítsa el a **PowerShellt**.</span><span class="sxs-lookup"><span data-stu-id="2348b-232">Launch **PowerShell**.</span></span>
2. <span data-ttu-id="2348b-233">Futtassa a hello **Get-AzureRmDataFactorySlice** toosee hello szeletek és az állapotok parancsot.</span><span class="sxs-lookup"><span data-stu-id="2348b-233">Run hello **Get-AzureRmDataFactorySlice** command toosee hello slices and their statuses.</span></span> <span data-ttu-id="2348b-234">A szelet hello állapottal kell megjelennie **sikertelen**.</span><span class="sxs-lookup"><span data-stu-id="2348b-234">You should see a slice with hello status of **Failed**.</span></span>        

    ```powershell   
    Get-AzureRmDataFactorySlice [-ResourceGroupName] <String> [-DataFactoryName] <String> [-DatasetName] <String> [-StartDateTime] <DateTime> [[-EndDateTime] <DateTime> ] [-Profile <AzureProfile> ] [ <CommonParameters>]
    ```   
   <span data-ttu-id="2348b-235">Példa:</span><span class="sxs-lookup"><span data-stu-id="2348b-235">For example:</span></span>

    ```powershell   
    Get-AzureRmDataFactorySlice -ResourceGroupName ADF -DataFactoryName LogProcessingFactory -DatasetName EnrichedGameEventsTable -StartDateTime 2014-05-04 20:00:00
    ```

   <span data-ttu-id="2348b-236">Cserélje le **StartDateTime** az a folyamat kezdési idejét.</span><span class="sxs-lookup"><span data-stu-id="2348b-236">Replace **StartDateTime** with start time of your pipeline.</span></span> 
3. <span data-ttu-id="2348b-237">Most, futtassa a hello **Get-AzureRmDataFactoryRun** hello szelet Futtatás hello tevékenység parancsmag tooget részleteit.</span><span class="sxs-lookup"><span data-stu-id="2348b-237">Now, run hello **Get-AzureRmDataFactoryRun** cmdlet tooget details about hello activity run for hello slice.</span></span>

    ```powershell   
    Get-AzureRmDataFactoryRun [-ResourceGroupName] <String> [-DataFactoryName] <String> [-DatasetName] <String> [-StartDateTime]
    <DateTime> [-Profile <AzureProfile> ] [ <CommonParameters>]
    ```

    <span data-ttu-id="2348b-238">Példa:</span><span class="sxs-lookup"><span data-stu-id="2348b-238">For example:</span></span>

    ```powershell   
    Get-AzureRmDataFactoryRun -ResourceGroupName ADF -DataFactoryName LogProcessingFactory -DatasetName EnrichedGameEventsTable -StartDateTime "5/5/2014 12:00:00 AM"
    ```

    <span data-ttu-id="2348b-239">hello StartDateTime értéke hello hiba vagy probléma szelet hello előző lépésben feljegyzett hello kezdési idejét.</span><span class="sxs-lookup"><span data-stu-id="2348b-239">hello value of StartDateTime is hello start time for hello error/problem slice that you noted from hello previous step.</span></span> <span data-ttu-id="2348b-240">hello dátum idejű dupla idézőjelek között kell megadni.</span><span class="sxs-lookup"><span data-stu-id="2348b-240">hello date-time should be enclosed in double quotes.</span></span>
4. <span data-ttu-id="2348b-241">Hello hiba, amely hasonló toohello következő részleteivel kimenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="2348b-241">You should see output with details about hello error that is similar toohello following:</span></span>

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
5. <span data-ttu-id="2348b-242">Hello futtatása **mentés-AzureRmDataFactoryLog** hello azonosítóérték hello kimeneti megtekintheti és hello használva töltik le a hello naplófájlok parancsmagot **- DownloadLogsoption** hello parancsmag.</span><span class="sxs-lookup"><span data-stu-id="2348b-242">You can run hello **Save-AzureRmDataFactoryLog** cmdlet with hello Id value that you see from hello output, and download hello log files by using hello **-DownloadLogsoption** for hello cmdlet.</span></span>

    ```powershell
    Save-AzureRmDataFactoryLog -ResourceGroupName "ADF" -DataFactoryName "LogProcessingFactory" -Id "841b77c9-d56c-48d1-99a3-8c16c3e77d39" -DownloadLogs -Output "C:\Test"
    ```

## <a name="rerun-failures-in-a-pipeline"></a><span data-ttu-id="2348b-243">Futtassa újra a feldolgozási hibák</span><span class="sxs-lookup"><span data-stu-id="2348b-243">Rerun failures in a pipeline</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2348b-244">Egyszerűbb tootroubleshoot hibák, és futtassa újra a sikertelen szeletek használatával hello figyelés & a felügyeleti alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="2348b-244">It's easier tootroubleshoot errors and rerun failed slices by using hello Monitoring & Management App.</span></span> <span data-ttu-id="2348b-245">Hello alkalmazás használatával kapcsolatos részletekért lásd: [felügyeletéhez és kezeléséhez az adat-előállító adatcsatornák hello figyelés és felügyelet alkalmazással](data-factory-monitor-manage-app.md).</span><span class="sxs-lookup"><span data-stu-id="2348b-245">For details about using hello application, see [monitor and manage Data Factory pipelines by using hello Monitoring and Management app](data-factory-monitor-manage-app.md).</span></span> 

### <a name="use-hello-azure-portal"></a><span data-ttu-id="2348b-246">Hello Azure portál használata</span><span class="sxs-lookup"><span data-stu-id="2348b-246">Use hello Azure portal</span></span>
<span data-ttu-id="2348b-247">Hibaelhárítás és a feldolgozási hibák hibakeresése, után újra futtathatja hibák toohello hiba szelet Navigálás, majd kattintson a hello **futtatása** hello parancssáv gombjára.</span><span class="sxs-lookup"><span data-stu-id="2348b-247">After you troubleshoot and debug failures in a pipeline, you can rerun failures by navigating toohello error slice and clicking hello **Run** button on hello command bar.</span></span>

![Futtassa újra a hibás szeletet](./media/data-factory-monitor-manage-pipelines/rerun-slice.png)

<span data-ttu-id="2348b-249">Esetben hello szelet meghiúsult érvényesítési házirend hiba miatt (például ha adatokat nem érhető el), javítsa ki a hello hibát, és újra hello gombra kattintva érvényesítenie **ellenőrzése** hello parancssáv gombjára.</span><span class="sxs-lookup"><span data-stu-id="2348b-249">In case hello slice has failed validation because of a policy failure (for example, if data isn't available), you can fix hello failure and validate again by clicking hello **Validate** button on hello command bar.</span></span>

![Javítsa a hibákat, és érvényesítése](./media/data-factory-monitor-manage-pipelines/fix-error-and-validate.png)

### <a name="use-azure-powershell"></a><span data-ttu-id="2348b-251">Azure PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="2348b-251">Use Azure PowerShell</span></span>
<span data-ttu-id="2348b-252">Hibák hello segítségével futtathatja **Set-AzureRmDataFactorySliceStatus** parancsmag.</span><span class="sxs-lookup"><span data-stu-id="2348b-252">You can rerun failures by using hello **Set-AzureRmDataFactorySliceStatus** cmdlet.</span></span> <span data-ttu-id="2348b-253">Lásd: hello [Set-AzureRmDataFactorySliceStatus](https://msdn.microsoft.com/library/mt603522.aspx) témakör szintaxisát és egyéb hello parancsmag részleteit.</span><span class="sxs-lookup"><span data-stu-id="2348b-253">See hello [Set-AzureRmDataFactorySliceStatus](https://msdn.microsoft.com/library/mt603522.aspx) topic for syntax and other details about hello cmdlet.</span></span>

<span data-ttu-id="2348b-254">**Példa**</span><span class="sxs-lookup"><span data-stu-id="2348b-254">**Example:**</span></span>

<span data-ttu-id="2348b-255">hello alábbi mintakód hello állapotának összes szeletek hello tábla "DAWikiAggregatedData" too'Waiting a "hello Azure data factoryban"WikiADF".</span><span class="sxs-lookup"><span data-stu-id="2348b-255">hello following example sets hello status of all slices for hello table 'DAWikiAggregatedData' too'Waiting' in hello Azure data factory 'WikiADF'.</span></span>

<span data-ttu-id="2348b-256">too'UpstreamInPipeline beállítása "Frissítés típusa" Hello ", ami azt jelenti, hogy minden szelet hello tábla és az összes hello függő (fölérendelt) tábla állapotok too'Waiting beállítása".</span><span class="sxs-lookup"><span data-stu-id="2348b-256">hello 'UpdateType' is set too'UpstreamInPipeline', which means that statuses of each slice for hello table and all hello dependent (upstream) tables are set too'Waiting'.</span></span> <span data-ttu-id="2348b-257">hello más lehetséges Ez a paraméter értéke "Egyéni".</span><span class="sxs-lookup"><span data-stu-id="2348b-257">hello other possible value for this parameter is 'Individual'.</span></span>

```powershell
Set-AzureRmDataFactorySliceStatus -ResourceGroupName ADF -DataFactoryName WikiADF -DatasetName DAWikiAggregatedData -Status Waiting -UpdateType UpstreamInPipeline -StartDateTime 2014-05-21T16:00:00 -EndDateTime 2014-05-21T20:00:00
```

## <a name="create-alerts"></a><span data-ttu-id="2348b-258">Riasztások létrehozása</span><span class="sxs-lookup"><span data-stu-id="2348b-258">Create alerts</span></span>
<span data-ttu-id="2348b-259">Azure naplók felhasználói eseményeket egy Azure-erőforrás (például a data factory) létrehozott, frissíteni vagy törölni.</span><span class="sxs-lookup"><span data-stu-id="2348b-259">Azure logs user events when an Azure resource (for example, a data factory) is created, updated, or deleted.</span></span> <span data-ttu-id="2348b-260">Ezek az események a riasztásokat hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="2348b-260">You can create alerts on these events.</span></span> <span data-ttu-id="2348b-261">Használja a Data Factory toocapture különböző metrikákat, és hozzon létre a riasztások mérőszámokat.</span><span class="sxs-lookup"><span data-stu-id="2348b-261">You can use Data Factory toocapture various metrics and create alerts on metrics.</span></span> <span data-ttu-id="2348b-262">Azt javasoljuk, hogy használja az eseményeket a valós idejű figyelését, és metrikák használja az előzmények elérése érdekében.</span><span class="sxs-lookup"><span data-stu-id="2348b-262">We recommend that you use events for real-time monitoring and use metrics for historical purposes.</span></span>

### <a name="alerts-on-events"></a><span data-ttu-id="2348b-263">Riasztások a események</span><span class="sxs-lookup"><span data-stu-id="2348b-263">Alerts on events</span></span>
<span data-ttu-id="2348b-264">Az Azure események mi történik az Azure-erőforrások a hasznos információkat adnak.</span><span class="sxs-lookup"><span data-stu-id="2348b-264">Azure events provide useful insights into what is happening in your Azure resources.</span></span> <span data-ttu-id="2348b-265">Azure Data Factory használatakor események akkor jönnek létre, ha:</span><span class="sxs-lookup"><span data-stu-id="2348b-265">When you're using Azure Data Factory, events are generated when:</span></span>

* <span data-ttu-id="2348b-266">Egy adat-előállító létrehozott, frissíteni vagy törölni.</span><span class="sxs-lookup"><span data-stu-id="2348b-266">A data factory is created, updated, or deleted.</span></span>
* <span data-ttu-id="2348b-267">Adatok feldolgozása (a "fut") van elindítva, vagy befejeződött.</span><span class="sxs-lookup"><span data-stu-id="2348b-267">Data processing (as "runs") has started or completed.</span></span>
* <span data-ttu-id="2348b-268">Igény szerinti HDInsight-fürtök létrehozásakor vagy eltávolításakor.</span><span class="sxs-lookup"><span data-stu-id="2348b-268">An on-demand HDInsight cluster is created or removed.</span></span>

<span data-ttu-id="2348b-269">A felhasználó eseményekből létre riasztásokat, és konfigurálásuk toosend e-mail értesítések toohello rendszergazda és coadministrators hello előfizetés.</span><span class="sxs-lookup"><span data-stu-id="2348b-269">You can create alerts on these user events and configure them toosend email notifications toohello administrator and coadministrators of hello subscription.</span></span> <span data-ttu-id="2348b-270">Emellett a felhasználók, akik tooreceive értesítő e-mailek hello feltételek teljesülése esetén további e-mail címeket is megadhat.</span><span class="sxs-lookup"><span data-stu-id="2348b-270">In addition, you can specify additional email addresses of users who need tooreceive email notifications when hello conditions are met.</span></span> <span data-ttu-id="2348b-271">Ez a szolgáltatás akkor hasznos, ha szeretné, hogy tooget hibáiról értesítést kap, és nem szeretné toocontinuously a figyelő a data factory.</span><span class="sxs-lookup"><span data-stu-id="2348b-271">This feature is useful when you want tooget notified on failures and don’t want toocontinuously monitor your data factory.</span></span>

> [!NOTE]
> <span data-ttu-id="2348b-272">Jelenleg hello portál nem riasztások megjelenítése a események.</span><span class="sxs-lookup"><span data-stu-id="2348b-272">Currently, hello portal doesn't show alerts on events.</span></span> <span data-ttu-id="2348b-273">Használjon hello [figyelés és a felügyeleti alkalmazás](data-factory-monitor-manage-app.md) toosee minden riasztás.</span><span class="sxs-lookup"><span data-stu-id="2348b-273">Use hello [Monitoring and Management app](data-factory-monitor-manage-app.md) toosee all alerts.</span></span>


#### <a name="specify-an-alert-definition"></a><span data-ttu-id="2348b-274">Adjon meg egy értesítési definíciója</span><span class="sxs-lookup"><span data-stu-id="2348b-274">Specify an alert definition</span></span>
<span data-ttu-id="2348b-275">egy riasztás definíciójának toospecify, létrehozhat egy JSON-fájl, amely leírja a riasztást kap a toobe kívánt hello műveletek.</span><span class="sxs-lookup"><span data-stu-id="2348b-275">toospecify an alert definition, you create a JSON file that describes hello operations that you want toobe alerted on.</span></span> <span data-ttu-id="2348b-276">A következő példa hello hello riasztás hello RunFinished művelet az e-mailben értesítést küld.</span><span class="sxs-lookup"><span data-stu-id="2348b-276">In hello following example, hello alert sends an email notification for hello RunFinished operation.</span></span> <span data-ttu-id="2348b-277">adott toobe, e-mailben értesítést küldött hello adat-előállítóban futtatás befejeződött, és hello futtatása sikertelen volt (állapot = FailedExecution).</span><span class="sxs-lookup"><span data-stu-id="2348b-277">toobe specific, an email notification is sent when a run in hello data factory has completed and hello run has failed (Status = FailedExecution).</span></span>

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
                "description": "One or more of hello data slices for hello Azure Data Factory has failed processing.",
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

<span data-ttu-id="2348b-278">Eltávolíthatja **részállapot** a JSON-definícióból, ha nem szeretné, hogy egy adott hiba esetén figyelmeztetés toobe hello.</span><span class="sxs-lookup"><span data-stu-id="2348b-278">You can remove **subStatus** from hello JSON definition if you don’t want toobe alerted on a specific failure.</span></span>

<span data-ttu-id="2348b-279">Ebben a példában az előfizetés az összes adat-előállítók hello riasztás beállítása.</span><span class="sxs-lookup"><span data-stu-id="2348b-279">This example sets up hello alert for all data factories in your subscription.</span></span> <span data-ttu-id="2348b-280">Ha azt szeretné, hogy állítsa be az egy adott data factory hello riasztási toobe, megadhatja az adat-előállító **resourceUri** a hello **dataSource**:</span><span class="sxs-lookup"><span data-stu-id="2348b-280">If you want hello alert toobe set up for a particular data factory, you can specify data factory **resourceUri** in hello **dataSource**:</span></span>

```JSON
"resourceUri" : "/SUBSCRIPTIONS/<subscriptionId>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.DATAFACTORY/DATAFACTORIES/<dataFactoryName>"
```

<span data-ttu-id="2348b-281">hello következő táblázat elérhető műveletek és állapotok (és részállapotok) hello listája.</span><span class="sxs-lookup"><span data-stu-id="2348b-281">hello following table provides hello list of available operations and statuses (and substatuses).</span></span>

| <span data-ttu-id="2348b-282">A művelet neve</span><span class="sxs-lookup"><span data-stu-id="2348b-282">Operation name</span></span> | <span data-ttu-id="2348b-283">status</span><span class="sxs-lookup"><span data-stu-id="2348b-283">Status</span></span> | <span data-ttu-id="2348b-284">A részállapot</span><span class="sxs-lookup"><span data-stu-id="2348b-284">Substatus</span></span> |
| --- | --- | --- |
| <span data-ttu-id="2348b-285">RunStarted</span><span class="sxs-lookup"><span data-stu-id="2348b-285">RunStarted</span></span> |<span data-ttu-id="2348b-286">Megkezdődött</span><span class="sxs-lookup"><span data-stu-id="2348b-286">Started</span></span> |<span data-ttu-id="2348b-287">Indulás alatt</span><span class="sxs-lookup"><span data-stu-id="2348b-287">Starting</span></span> |
| <span data-ttu-id="2348b-288">RunFinished</span><span class="sxs-lookup"><span data-stu-id="2348b-288">RunFinished</span></span> |<span data-ttu-id="2348b-289">Nem sikerült / sikeres volt.</span><span class="sxs-lookup"><span data-stu-id="2348b-289">Failed / Succeeded</span></span> |<span data-ttu-id="2348b-290">FailedResourceAllocation</span><span class="sxs-lookup"><span data-stu-id="2348b-290">FailedResourceAllocation</span></span><br/><br/><span data-ttu-id="2348b-291">Sikeres</span><span class="sxs-lookup"><span data-stu-id="2348b-291">Succeeded</span></span><br/><br/><span data-ttu-id="2348b-292">FailedExecution</span><span class="sxs-lookup"><span data-stu-id="2348b-292">FailedExecution</span></span><br/><br/><span data-ttu-id="2348b-293">Időtúllépésbe került</span><span class="sxs-lookup"><span data-stu-id="2348b-293">TimedOut</span></span><br/><br/><span data-ttu-id="2348b-294">< megszakítva</span><span class="sxs-lookup"><span data-stu-id="2348b-294"><Canceled</span></span><br/><br/><span data-ttu-id="2348b-295">FailedValidation</span><span class="sxs-lookup"><span data-stu-id="2348b-295">FailedValidation</span></span><br/><br/><span data-ttu-id="2348b-296">Elhagyott</span><span class="sxs-lookup"><span data-stu-id="2348b-296">Abandoned</span></span> |
| <span data-ttu-id="2348b-297">OnDemandClusterCreateStarted</span><span class="sxs-lookup"><span data-stu-id="2348b-297">OnDemandClusterCreateStarted</span></span> |<span data-ttu-id="2348b-298">Megkezdődött</span><span class="sxs-lookup"><span data-stu-id="2348b-298">Started</span></span> | |
| <span data-ttu-id="2348b-299">OnDemandClusterCreateSuccessful</span><span class="sxs-lookup"><span data-stu-id="2348b-299">OnDemandClusterCreateSuccessful</span></span> |<span data-ttu-id="2348b-300">Sikeres</span><span class="sxs-lookup"><span data-stu-id="2348b-300">Succeeded</span></span> | |
| <span data-ttu-id="2348b-301">OnDemandClusterDeleted</span><span class="sxs-lookup"><span data-stu-id="2348b-301">OnDemandClusterDeleted</span></span> |<span data-ttu-id="2348b-302">Sikeres</span><span class="sxs-lookup"><span data-stu-id="2348b-302">Succeeded</span></span> | |

<span data-ttu-id="2348b-303">Lásd: [riasztási szabály létrehozása](https://msdn.microsoft.com/library/azure/dn510366.aspx) hello JSON-elemek szerepelnek hello példában használt vonatkozó további információért.</span><span class="sxs-lookup"><span data-stu-id="2348b-303">See [Create Alert Rule](https://msdn.microsoft.com/library/azure/dn510366.aspx) for details about hello JSON elements that are used in hello example.</span></span>

#### <a name="deploy-hello-alert"></a><span data-ttu-id="2348b-304">Hello riasztás telepítése</span><span class="sxs-lookup"><span data-stu-id="2348b-304">Deploy hello alert</span></span>
<span data-ttu-id="2348b-305">toodeploy hello riasztás használata hello Azure PowerShell-parancsmag **New-AzureRmResourceGroupDeployment**, ahogy az alábbi példa hello:</span><span class="sxs-lookup"><span data-stu-id="2348b-305">toodeploy hello alert, use hello Azure PowerShell cmdlet **New-AzureRmResourceGroupDeployment**, as shown in hello following example:</span></span>

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName adf -TemplateFile .\ADFAlertFailedSlice.json  
```

<span data-ttu-id="2348b-306">Miután hello erőforrás csoport központi telepítése sikeresen befejeződött, a következő üzenetek hello jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="2348b-306">After hello resource group deployment has finished successfully, you see hello following messages:</span></span>

```
VERBOSE: 7:00:48 PM - Template is valid.
WARNING: 7:00:48 PM - hello StorageAccountName parameter is no longer used and will be removed in a future release.
Please update scripts tooremove this parameter.
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
> <span data-ttu-id="2348b-307">Használhatja a hello [riasztást szabály létrehozása](https://msdn.microsoft.com/library/azure/dn510366.aspx) REST API toocreate riasztási szabályt.</span><span class="sxs-lookup"><span data-stu-id="2348b-307">You can use hello [Create Alert Rule](https://msdn.microsoft.com/library/azure/dn510366.aspx) REST API toocreate an alert rule.</span></span> <span data-ttu-id="2348b-308">hello JSON-adattartalmat hasonló toohello JSON példaként szolgál.</span><span class="sxs-lookup"><span data-stu-id="2348b-308">hello JSON payload is similar toohello JSON example.</span></span>  


#### <a name="retrieve-hello-list-of-azure-resource-group-deployments"></a><span data-ttu-id="2348b-309">Hello listáját, az Azure erőforrás-csoport központi telepítések</span><span class="sxs-lookup"><span data-stu-id="2348b-309">Retrieve hello list of Azure resource group deployments</span></span>
<span data-ttu-id="2348b-310">tooretrieve hello listája az Azure erőforrás-csoport központi telepítése, hello parancsmaggal **Get-AzureRmResourceGroupDeployment**, ahogy az alábbi példa hello:</span><span class="sxs-lookup"><span data-stu-id="2348b-310">tooretrieve hello list of deployed Azure resource group deployments, use hello cmdlet **Get-AzureRmResourceGroupDeployment**, as shown in hello following example:</span></span>

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

#### <a name="troubleshoot-user-events"></a><span data-ttu-id="2348b-311">Felhasználói események hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="2348b-311">Troubleshoot user events</span></span>
1. <span data-ttu-id="2348b-312">Megtekintheti a hello kattintás után létrehozott összes hello események **metrikák és műveletek** csempére.</span><span class="sxs-lookup"><span data-stu-id="2348b-312">You can see all hello events that are generated after clicking hello **Metrics and operations** tile.</span></span>

    ![Metrikák és műveletek csempe](./media/data-factory-monitor-manage-pipelines/metrics-and-operations-tile.png)
2. <span data-ttu-id="2348b-314">Kattintson a hello **események** toosee hello események csempére.</span><span class="sxs-lookup"><span data-stu-id="2348b-314">Click hello **Events** tile toosee hello events.</span></span>

    ![Események csempéje](./media/data-factory-monitor-manage-pipelines/events-tile.png)
3. <span data-ttu-id="2348b-316">A hello **események** panelen láthatja, hogy eseményeket, szűrt eseményeket, és egyéb adatait.</span><span class="sxs-lookup"><span data-stu-id="2348b-316">On hello **Events** blade, you can see details about events, filtered events, and so on.</span></span>

    ![Események panel](./media/data-factory-monitor-manage-pipelines/events-blade.png)
4. <span data-ttu-id="2348b-318">Kattintson egy **művelet** hello műveletek lista, amely a hibát.</span><span class="sxs-lookup"><span data-stu-id="2348b-318">Click an **Operation** in hello operations list that causes an error.</span></span>

    ![Válasszon ki egy műveletet](./media/data-factory-monitor-manage-pipelines/select-operation.png)
5. <span data-ttu-id="2348b-320">Kattintson egy **hiba** hello hiba esemény toosee részleteit.</span><span class="sxs-lookup"><span data-stu-id="2348b-320">Click an **Error** event toosee details about hello error.</span></span>

    ![Esemény hiba](./media/data-factory-monitor-manage-pipelines/operation-error-event.png)

<span data-ttu-id="2348b-322">Lásd: [Azure Insight parancsmagok](https://msdn.microsoft.com/library/mt282452.aspx) tooadd használt PowerShell-parancsmagokkal, beolvasása, vagy távolítsa el a riasztásokat.</span><span class="sxs-lookup"><span data-stu-id="2348b-322">See [Azure Insight cmdlets](https://msdn.microsoft.com/library/mt282452.aspx) for PowerShell cmdlets that you can use tooadd, get, or remove alerts.</span></span> <span data-ttu-id="2348b-323">Néhány példa a hello segítségével **Get-AlertRule** parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="2348b-323">Here are a few examples of using hello **Get-AlertRule** cmdlet:</span></span>

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
Description : One or more of hello data slices for hello Azure Data Factory has failed processing.
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

<span data-ttu-id="2348b-324">Futtassa a get-help parancsok toosee részletek és a példákat hello Get-AlertRule parancsmag a következő hello.</span><span class="sxs-lookup"><span data-stu-id="2348b-324">Run hello following get-help commands toosee details and examples for hello Get-AlertRule cmdlet.</span></span>

```powershell
get-help Get-AlertRule -detailed
```

```powershell
get-help Get-AlertRule -examples
```


<span data-ttu-id="2348b-325">Ha látható a riasztás előállítás események hello hello portál panel, de nem kap értesítő e-mailek, ellenőrizze, hogy hello e-mail címet, amely a megadott külső feladóktól tooreceive e-mailek van beállítva.</span><span class="sxs-lookup"><span data-stu-id="2348b-325">If you see hello alert generation events on hello portal blade but you don't receive email notifications, check whether hello email address that is specified is set tooreceive emails from external senders.</span></span> <span data-ttu-id="2348b-326">hello értesítő e-mailek előfordulhat, hogy le lettek tiltva az e-mail beállítások szerint.</span><span class="sxs-lookup"><span data-stu-id="2348b-326">hello alert emails might have been blocked by your email settings.</span></span>

### <a name="alerts-on-metrics"></a><span data-ttu-id="2348b-327">Riasztások a metrikák</span><span class="sxs-lookup"><span data-stu-id="2348b-327">Alerts on metrics</span></span>
<span data-ttu-id="2348b-328">A Data Factory különböző metrikák rögzítése, és létre riasztásokat mérőszámokat.</span><span class="sxs-lookup"><span data-stu-id="2348b-328">In Data Factory, you can capture various metrics and create alerts on metrics.</span></span> <span data-ttu-id="2348b-329">Figyelheti, és értesítést létrehozni a következő metrikák hello szeletek az adat-előállítóban hello:</span><span class="sxs-lookup"><span data-stu-id="2348b-329">You can monitor and create alerts on hello following metrics for hello slices in your data factory:</span></span>

* <span data-ttu-id="2348b-330">**Nem sikerült fut.**</span><span class="sxs-lookup"><span data-stu-id="2348b-330">**Failed Runs**</span></span>
* <span data-ttu-id="2348b-331">**Sikeres futtatása**</span><span class="sxs-lookup"><span data-stu-id="2348b-331">**Successful Runs**</span></span>

<span data-ttu-id="2348b-332">A metrikák hasznosak, és segítséget nyújtson tooget hello adat-előállítóban futtató áttekintését a teljes és a sikertelen.</span><span class="sxs-lookup"><span data-stu-id="2348b-332">These metrics are useful and help you tooget an overview of overall failed and successful runs in hello data factory.</span></span> <span data-ttu-id="2348b-333">Minden alkalommal, amikor nincs szelet futtató metrikák kibocsátott.</span><span class="sxs-lookup"><span data-stu-id="2348b-333">Metrics are emitted every time there is a slice run.</span></span> <span data-ttu-id="2348b-334">Elején hello hello óra, a metrikák összesítése és leküldött tooyour tárfiók.</span><span class="sxs-lookup"><span data-stu-id="2348b-334">At hello beginning of hello hour, these metrics are aggregated and pushed tooyour storage account.</span></span> <span data-ttu-id="2348b-335">a storage-fiók beállítása tooenable metrikákat.</span><span class="sxs-lookup"><span data-stu-id="2348b-335">tooenable metrics, set up a storage account.</span></span>

#### <a name="enable-metrics"></a><span data-ttu-id="2348b-336">Metrikák engedélyezése</span><span class="sxs-lookup"><span data-stu-id="2348b-336">Enable metrics</span></span>
<span data-ttu-id="2348b-337">tooenable metrika, kattintson a hello hello következő **adat-előállító** panel:</span><span class="sxs-lookup"><span data-stu-id="2348b-337">tooenable metrics, click hello following from hello **Data Factory** blade:</span></span>

<span data-ttu-id="2348b-338">**Figyelési** > **metrika** > **diagnosztikai beállítások** > **diagnosztika**</span><span class="sxs-lookup"><span data-stu-id="2348b-338">**Monitoring** > **Metric** > **Diagnostic settings** > **Diagnostics**</span></span>

![Diagnosztika hivatkozás](./media/data-factory-monitor-manage-pipelines/diagnostics-link.png)

<span data-ttu-id="2348b-340">A hello **diagnosztika** panelen kattintson a **a**, válassza ki hello tárfiókot, és kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="2348b-340">On hello **Diagnostics** blade, click **On**, select hello storage account, and click **Save**.</span></span>

![Diagnosztika panel](./media/data-factory-monitor-manage-pipelines/diagnostics-blade.png)

<span data-ttu-id="2348b-342">Másolatot hello metrikák toobe hello látható tooone órát vehet igénybe **figyelés** panel mert metrikák összesítési óránként történik.</span><span class="sxs-lookup"><span data-stu-id="2348b-342">It might take up tooone hour for hello metrics toobe visible on hello **Monitoring** blade because metrics aggregation happens hourly.</span></span>

### <a name="set-up-an-alert-on-metrics"></a><span data-ttu-id="2348b-343">A metrikák riasztás beállítása</span><span class="sxs-lookup"><span data-stu-id="2348b-343">Set up an alert on metrics</span></span>
<span data-ttu-id="2348b-344">Kattintson a hello **adat-előállító metrikák** csempén:</span><span class="sxs-lookup"><span data-stu-id="2348b-344">Click hello **Data Factory metrics** tile:</span></span>

![Data factory metrikák csempe](./media/data-factory-monitor-manage-pipelines/data-factory-metrics-tile.png)

<span data-ttu-id="2348b-346">A hello **metrika** panelen kattintson a **+ Hozzáadás riasztás** hello eszköztáron.</span><span class="sxs-lookup"><span data-stu-id="2348b-346">On hello **Metric** blade, click **+ Add alert** on hello toolbar.</span></span>
<span data-ttu-id="2348b-347">![Data factory metrika panel > riasztás hozzáadása](./media/data-factory-monitor-manage-pipelines/add-alert.png)</span><span class="sxs-lookup"><span data-stu-id="2348b-347">![Data factory metric blade > Add alert](./media/data-factory-monitor-manage-pipelines/add-alert.png)</span></span>

<span data-ttu-id="2348b-348">A hello **riasztási szabály felvétele** lapon hello a következő lépéseket, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="2348b-348">On hello **Add an alert rule** page, do hello following steps, and click **OK**.</span></span>

* <span data-ttu-id="2348b-349">Adjon meg egy nevet hello riasztás (Példa: "riasztás sikertelen").</span><span class="sxs-lookup"><span data-stu-id="2348b-349">Enter a name for hello alert (example: "failed alert").</span></span>
* <span data-ttu-id="2348b-350">Adjon meg egy leírást, hello riasztás (Példa: "e-mail küldési hiba esetén").</span><span class="sxs-lookup"><span data-stu-id="2348b-350">Enter a description for hello alert (example: "send an email when a failure occurs").</span></span>
* <span data-ttu-id="2348b-351">Jelöljön ki egy metrikát ("Futtatása sikertelen" vs. "Sikeres futtatása").</span><span class="sxs-lookup"><span data-stu-id="2348b-351">Select a metric ("Failed Runs" vs. "Successful Runs").</span></span>
* <span data-ttu-id="2348b-352">Adjon meg egy feltételt és egy küszöbértéket.</span><span class="sxs-lookup"><span data-stu-id="2348b-352">Specify a condition and a threshold value.</span></span>   
* <span data-ttu-id="2348b-353">Adja meg a hello időn belül.</span><span class="sxs-lookup"><span data-stu-id="2348b-353">Specify hello period of time.</span></span>
* <span data-ttu-id="2348b-354">Adja meg, hogy egy e-mailt kell küldeni, tooowners, közreműködőknek és olvasóknak.</span><span class="sxs-lookup"><span data-stu-id="2348b-354">Specify whether an email should be sent tooowners, contributors, and readers.</span></span>

![Data factory metrika panel > Hozzáadás riasztási szabálya](./media/data-factory-monitor-manage-pipelines/add-an-alert-rule.png)

<span data-ttu-id="2348b-356">Hello riasztási szabályt hozzáadása sikeresen, hello panel bezárása után, és megtekintheti hello új riasztás hello után **metrika** panelen.</span><span class="sxs-lookup"><span data-stu-id="2348b-356">After hello alert rule is added successfully, hello blade closes and you see hello new alert on hello **Metric** blade.</span></span>

![Data factory metrika panel > Új értesítés hozzáadása](./media/data-factory-monitor-manage-pipelines/failed-alert-in-metric-blade.png)

<span data-ttu-id="2348b-358">Emellett meg kell jelennie a hello értesítések hello száma **riasztási szabályok** csempére.</span><span class="sxs-lookup"><span data-stu-id="2348b-358">You should also see hello number of alerts in hello **Alert rules** tile.</span></span> <span data-ttu-id="2348b-359">Kattintson a hello **riasztási szabályok** csempére.</span><span class="sxs-lookup"><span data-stu-id="2348b-359">Click hello **Alert rules** tile.</span></span>

![Data factory metrika panelről – riasztási szabályok](./media/data-factory-monitor-manage-pipelines/alert-rules-tile-rules.png)

<span data-ttu-id="2348b-361">A hello **szabályok riasztást** panelen bármely létező riasztások megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="2348b-361">On hello **Alerts rules** blade, you see any existing alerts.</span></span> <span data-ttu-id="2348b-362">Kattintson az értesítés, tooadd **riasztás hozzáadása** hello eszköztáron.</span><span class="sxs-lookup"><span data-stu-id="2348b-362">tooadd an alert, click **Add alert** on hello toolbar.</span></span>

![A riasztási szabályok panel](./media/data-factory-monitor-manage-pipelines/alert-rules-blade.png)

### <a name="alert-notifications"></a><span data-ttu-id="2348b-364">Riasztási értesítések</span><span class="sxs-lookup"><span data-stu-id="2348b-364">Alert notifications</span></span>
<span data-ttu-id="2348b-365">Miután hello riasztási szabály hello feltétel megegyezik, egy e-mailt, amely szerint a hello riasztás aktiválva szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="2348b-365">After hello alert rule matches hello condition, you should get an email that says hello alert is activated.</span></span> <span data-ttu-id="2348b-366">Miután hello probléma megoldódott, és többé nem egyezik a hello riasztási feltétel, kap egy e-mailt, amely szerint a hello riasztás oldódik meg.</span><span class="sxs-lookup"><span data-stu-id="2348b-366">After hello issue is resolved and hello alert condition doesn’t match anymore, you get an email that says hello alert is resolved.</span></span>

<span data-ttu-id="2348b-367">Ez a viselkedés eltér események hol egy értesítést küld az összes hiba, amely jogosult riasztási szabályt.</span><span class="sxs-lookup"><span data-stu-id="2348b-367">This behavior is different than events where a notification is sent on every failure that an alert rule qualifies for.</span></span>

### <a name="deploy-alerts-by-using-powershell"></a><span data-ttu-id="2348b-368">Riasztások telepítését a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="2348b-368">Deploy alerts by using PowerShell</span></span>
<span data-ttu-id="2348b-369">A metrikák hello azonos riasztások telepítheti úgy, hogy az eseményeket.</span><span class="sxs-lookup"><span data-stu-id="2348b-369">You can deploy alerts for metrics hello same way that you do for events.</span></span>

<span data-ttu-id="2348b-370">**A riasztás definíciója**</span><span class="sxs-lookup"><span data-stu-id="2348b-370">**Alert definition**</span></span>

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

<span data-ttu-id="2348b-371">Cserélje le *subscriptionId*, *resourceGroupName*, és *dataFactoryName* hello mintában megfelelő értékekkel.</span><span class="sxs-lookup"><span data-stu-id="2348b-371">Replace *subscriptionId*, *resourceGroupName*, and *dataFactoryName* in hello sample with appropriate values.</span></span>

<span data-ttu-id="2348b-372">*metricName* jelenleg csak a két érték támogatja:</span><span class="sxs-lookup"><span data-stu-id="2348b-372">*metricName* currently supports two values:</span></span>

* <span data-ttu-id="2348b-373">FailedRuns</span><span class="sxs-lookup"><span data-stu-id="2348b-373">FailedRuns</span></span>
* <span data-ttu-id="2348b-374">SuccessfulRuns</span><span class="sxs-lookup"><span data-stu-id="2348b-374">SuccessfulRuns</span></span>

<span data-ttu-id="2348b-375">**Hello riasztás telepítése**</span><span class="sxs-lookup"><span data-stu-id="2348b-375">**Deploy hello alert**</span></span>

<span data-ttu-id="2348b-376">toodeploy hello riasztás használata hello Azure PowerShell-parancsmag **New-AzureRmResourceGroupDeployment**, ahogy az alábbi példa hello:</span><span class="sxs-lookup"><span data-stu-id="2348b-376">toodeploy hello alert, use hello Azure PowerShell cmdlet **New-AzureRmResourceGroupDeployment**, as shown in hello following example:</span></span>

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName adf -TemplateFile .\FailedRunsGreaterThan5.json
```

<span data-ttu-id="2348b-377">Üzenet a sikeres telepítés után a következő kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="2348b-377">You should see following message after a successful deployment:</span></span>

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

<span data-ttu-id="2348b-378">Is használhatja a hello **Add-AlertRule** parancsmag toodeploy riasztási szabályt.</span><span class="sxs-lookup"><span data-stu-id="2348b-378">You can also use hello **Add-AlertRule** cmdlet toodeploy an alert rule.</span></span> <span data-ttu-id="2348b-379">Lásd: hello [Add-AlertRule](https://msdn.microsoft.com/library/mt282468.aspx) részletek és példákat a témakör.</span><span class="sxs-lookup"><span data-stu-id="2348b-379">See hello [Add-AlertRule](https://msdn.microsoft.com/library/mt282468.aspx) topic for details and examples.</span></span>  

## <a name="move-a-data-factory-tooa-different-resource-group-or-subscription"></a><span data-ttu-id="2348b-380">Helyezze át a data factory tooa másik erőforráscsoportba vagy előfizetésbe</span><span class="sxs-lookup"><span data-stu-id="2348b-380">Move a data factory tooa different resource group or subscription</span></span>
<span data-ttu-id="2348b-381">Áthelyezheti a data factory tooa eltérő erőforráscsoportban vagy egy másik előfizetésben található hello segítségével **áthelyezése** sáv gombra a data factory hello kezdőlapján parancsot.</span><span class="sxs-lookup"><span data-stu-id="2348b-381">You can move a data factory tooa different resource group or a different subscription by using hello **Move** command bar button on hello home page of your data factory.</span></span>

![Adat-előállító áthelyezése](./media/data-factory-monitor-manage-pipelines/MoveDataFactory.png)

<span data-ttu-id="2348b-383">Is áthelyezheti bármely kapcsolódó erőforrások (például a riasztások hello adat-előállító társított), adat-előállító hello együtt.</span><span class="sxs-lookup"><span data-stu-id="2348b-383">You can also move any related resources (such as alerts that are associated with hello data factory), along with hello data factory.</span></span>

![Források párbeszédpanelt](./media/data-factory-monitor-manage-pipelines/MoveResources.png)
