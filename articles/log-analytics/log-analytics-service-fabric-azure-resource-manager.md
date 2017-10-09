---
title: "Service Fabric-alkalmazások Naplóelemzési használatával aaaAssess hello Azure-portálon |} Microsoft Docs"
description: "Naplóelemzési hello Azure portál tooassess hello kockázat és a Service Fabric-alkalmazások állapotát a hello Service Fabric megoldás is használhat micro-szolgáltatások, csomópontokat és fürtöket."
services: log-analytics
documentationcenter: 
author: niniikhena
manager: jochan
editor: 
ms.assetid: 9c91aacb-c48e-466c-b792-261f25940c0c
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: nini
ms.openlocfilehash: 891c7f6e5ed511ac18599bdc280ab3dc09700fbe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="assess-service-fabric-applications-and-micro-services-with-hello-azure-portal"></a><span data-ttu-id="86172-103">Mérje fel a Service Fabric-alkalmazások és micro-szolgáltatások hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="86172-103">Assess Service Fabric applications and micro-services with hello Azure portal</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="86172-104">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="86172-104">Resource Manager</span></span>](log-analytics-service-fabric-azure-resource-manager.md)
> * [<span data-ttu-id="86172-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="86172-105">PowerShell</span></span>](log-analytics-service-fabric.md)
>
>

![A Service Fabric szimbólum](./media/log-analytics-service-fabric/service-fabric-assessment-symbol.png)

<span data-ttu-id="86172-107">Ez a cikk ismerteti, hogyan toouse hello Naplóelemzési toohelp megoldás a Service Fabric azonosítása és elhárítása a Service Fabric-fürt között.</span><span class="sxs-lookup"><span data-stu-id="86172-107">This article describes how toouse hello Service Fabric solution in Log Analytics toohelp identify and troubleshoot issues across your Service Fabric cluster.</span></span>

<span data-ttu-id="86172-108">hello Service Fabric megoldás Azure diagnosztikai adatokat a Service Fabric virtuális gépek által az adatok gyűjtése az Azure ÜVEGVATTA táblát használ.</span><span class="sxs-lookup"><span data-stu-id="86172-108">hello Service Fabric solution uses Azure Diagnostics data from your Service Fabric VMs, by collecting this data from your Azure WAD tables.</span></span> <span data-ttu-id="86172-109">A Naplóelemzési ezután beolvassa a Service Fabric keretrendszer eseményeihez, beleértve a **megbízható szolgáltatás események**, **szereplő események**, **működési eseményeit**, és **egyéni ETW-események**.</span><span class="sxs-lookup"><span data-stu-id="86172-109">Log Analytics then reads Service Fabric framework events, including **Reliable Service Events**, **Actor Events**, **Operational Events**, and **Custom ETW events**.</span></span> <span data-ttu-id="86172-110">A hello megoldás irányítópultja, esetén képes tooview jelentős problémák és kapcsolódó eseményeket a Service Fabric-környezetben.</span><span class="sxs-lookup"><span data-stu-id="86172-110">With hello solution dashboard, you are able tooview notable issues and relevant events in your Service Fabric environment.</span></span>

<span data-ttu-id="86172-111">tooconnect szüksége tooget hello megoldás használatába, a Service Fabric fürt tooa Naplóelemzési munkaterületet.</span><span class="sxs-lookup"><span data-stu-id="86172-111">tooget started with hello solution, you need tooconnect your Service Fabric cluster tooa Log Analytics workspace.</span></span> <span data-ttu-id="86172-112">Az alábbiakban a három forgatókönyvek tooconsider:</span><span class="sxs-lookup"><span data-stu-id="86172-112">Here are three scenarios tooconsider:</span></span>

1. <span data-ttu-id="86172-113">Ha nem telepítette a Service Fabric-fürt, lépésekkel hello a ***központi telepítése a Service Fabric-fürt csatlakoztatott tooa Naplóelemzési munkaterület*** toodeploy új fürt, és hozzá beállítva tooreport tooLog elemzés.</span><span class="sxs-lookup"><span data-stu-id="86172-113">If you have not deployed your Service Fabric cluster, use hello steps in ***Deploy a Service Fabric Cluster connected tooa Log Analytics workspace*** toodeploy a new cluster and have it configured tooreport tooLog Analytics.</span></span>
2. <span data-ttu-id="86172-114">Ha toocollect teljesítményszámlálók a gazdagépek toouse a más OMS megoldások, például biztonsági meg a Service Fabric-fürt, kövesse hello ***tartalmazó csatlakozott a Service Fabric-fürt tooa Naplóelemzési munkaterület Virtuálisgép-bővítmény telepítése telepítve.***</span><span class="sxs-lookup"><span data-stu-id="86172-114">If you need toocollect performance counters from your hosts toouse other OMS solutions such as Security on your Service Fabric Cluster, follow hello steps in ***Deploy a Service Fabric Cluster connected tooa Log Analytics workspace with VM Extension installed.***</span></span>
3. <span data-ttu-id="86172-115">Ha már telepítette a Service Fabric-fürt, és szeretné, hogy tooconnect azt tooLog elemzés, kövesse hello ***hozzáadása egy meglévő storage-fiók tooLog elemzés.***</span><span class="sxs-lookup"><span data-stu-id="86172-115">If you have already deployed your Service Fabric cluster and want tooconnect it tooLog Analytics, follow hello steps in ***Adding an existing storage account tooLog Analytics.***</span></span>

## <a name="deploy-a-service-fabric-cluster-connected-tooa-log-analytics-workspace"></a><span data-ttu-id="86172-116">A Service Fabric-fürt csatlakoztatott tooa Naplóelemzési munkaterület központi telepítése.</span><span class="sxs-lookup"><span data-stu-id="86172-116">Deploy a Service Fabric Cluster connected tooa Log Analytics workspace.</span></span>
<span data-ttu-id="86172-117">Ez a sablon hello a következő:</span><span class="sxs-lookup"><span data-stu-id="86172-117">This template does hello following:</span></span>

1. <span data-ttu-id="86172-118">Az Azure Service Fabric fürt már csatlakozott tooa Naplóelemzési munkaterület telepíti.</span><span class="sxs-lookup"><span data-stu-id="86172-118">Deploys an Azure Service Fabric cluster already connected tooa Log Analytics workspace.</span></span> <span data-ttu-id="86172-119">Lehetősége van hello beállítás toocreate új munkaterület hello sablont, vagy egy már meglévő Naplóelemzési munkaterület bemeneti hello neve telepítése közben.</span><span class="sxs-lookup"><span data-stu-id="86172-119">You have hello option toocreate a new workspace while deploying hello template, or input hello name of an already existing Log Analytics workspace.</span></span>
2. <span data-ttu-id="86172-120">Hello diagnosztikai tárolási fiók toohello Naplóelemzési munkaterület hozzáadja.</span><span class="sxs-lookup"><span data-stu-id="86172-120">Adds hello diagnostic storage account toohello Log Analytics workspace.</span></span>
3. <span data-ttu-id="86172-121">Lehetővé teszi, hogy a Service Fabric megoldás hello Naplóelemzési munkaterületet.</span><span class="sxs-lookup"><span data-stu-id="86172-121">Enables hello Service Fabric solution in your Log Analytics workspace.</span></span>

<span data-ttu-id="86172-122">[![TooAzure telepítése](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fazure%2Fazure-quickstart-templates%2Fmaster%2Fservice-fabric-oms%2F%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="86172-122">[![Deploy tooAzure](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fazure%2Fazure-quickstart-templates%2Fmaster%2Fservice-fabric-oms%2F%2Fazuredeploy.json)</span></span>

<span data-ttu-id="86172-123">Miután hello gombot, a rendszer hello Azure megnyílik az Ön tooedit paraméterekkel.</span><span class="sxs-lookup"><span data-stu-id="86172-123">Once you select hello deploy button above, hello Azure portal opens with parameters for you tooedit.</span></span> <span data-ttu-id="86172-124">Lehet, hogy toocreate egy új erőforráscsoportot Ha, adjon meg egy új nevet a Naplóelemzési munkaterület:</span><span class="sxs-lookup"><span data-stu-id="86172-124">Be sure toocreate a new resource group if you input a new Log Analytics workspace name:</span></span>

![Service Fabric](./media/log-analytics-service-fabric/2.png)

![Service Fabric](./media/log-analytics-service-fabric/3.png)

<span data-ttu-id="86172-127">Fogadja el a hello jogi feltételeket, majd kattintson a **létrehozása** toostart hello központi telepítés.</span><span class="sxs-lookup"><span data-stu-id="86172-127">Accept hello legal terms and click **Create** toostart hello deployment.</span></span> <span data-ttu-id="86172-128">Ha hello telepítés befejeződött, inkább hello új munkaterületet, és a fürt létrehozása, és hello WADServiceFabric * hozzáadott esemény, WADWindowsEventLogs és WADETWEvent táblák:</span><span class="sxs-lookup"><span data-stu-id="86172-128">Once hello deployment is completed, you should see hello new workspace and cluster created, and hello WADServiceFabric*Event, WADWindowsEventLogs and WADETWEvent tables added:</span></span>

![Service Fabric](./media/log-analytics-service-fabric/4.png)

## <a name="deploy-a-service-fabric-cluster-connected-tooa-log-analytics-workspace-with-vm-extension-installed"></a><span data-ttu-id="86172-130">Telepítse a Service Fabric-fürt csatlakoztatott tooa Naplóelemzési munkaterület telepített Virtuálisgép-bővítménnyel.</span><span class="sxs-lookup"><span data-stu-id="86172-130">Deploy a Service Fabric Cluster connected tooa Log Analytics workspace with VM Extension installed.</span></span>

<span data-ttu-id="86172-131">Ez a sablon hello a következő:</span><span class="sxs-lookup"><span data-stu-id="86172-131">This template does hello following:</span></span>

1. <span data-ttu-id="86172-132">Az Azure Service Fabric fürt már csatlakozott tooa Naplóelemzési munkaterület telepíti.</span><span class="sxs-lookup"><span data-stu-id="86172-132">Deploys an Azure Service Fabric cluster already connected tooa Log Analytics workspace.</span></span> <span data-ttu-id="86172-133">Hozzon létre egy új munkaterületet, vagy használjon egy meglévőt.</span><span class="sxs-lookup"><span data-stu-id="86172-133">You can create a new workspace or use an existing one.</span></span>
2. <span data-ttu-id="86172-134">Hello diagnosztikai tárolási fiókok toohello Naplóelemzési munkaterület hozzáadja.</span><span class="sxs-lookup"><span data-stu-id="86172-134">Adds hello diagnostic storage accounts toohello Log Analytics workspace.</span></span>
3. <span data-ttu-id="86172-135">Lehetővé teszi, hogy a Service Fabric megoldás hello hello Naplóelemzési munkaterület.</span><span class="sxs-lookup"><span data-stu-id="86172-135">Enables hello Service Fabric solution in hello Log Analytics workspace.</span></span>
4. <span data-ttu-id="86172-136">Állítsa be a Service Fabric-fürt minden egyes virtuálisgép-méretezési hello MMA ügynök bővítményt telepít.</span><span class="sxs-lookup"><span data-stu-id="86172-136">Installs hello MMA agent extension in each virtual machine scale set in your Service Fabric cluster.</span></span> <span data-ttu-id="86172-137">Hello MMA ügynök van telepítve készül, amelyek képesek tooview teljesítménymutatók a csomópontokat.</span><span class="sxs-lookup"><span data-stu-id="86172-137">With hello MMA agent installed, you are able tooview performance metrics about your nodes.</span></span>

<span data-ttu-id="86172-138">[![TooAzure telepítése](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fazure%2Fazure-quickstart-templates%2Fmaster%2Fservice-fabric-vmss-oms%2F%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="86172-138">[![Deploy tooAzure](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fazure%2Fazure-quickstart-templates%2Fmaster%2Fservice-fabric-vmss-oms%2F%2Fazuredeploy.json)</span></span>

<span data-ttu-id="86172-139">Következő hello bemeneti hello szükséges paraméterek a fenti lépéseket, és a központi telepítés indítsa.</span><span class="sxs-lookup"><span data-stu-id="86172-139">Following hello same steps above, input hello necessary parameters, and kick off a deployment.</span></span> <span data-ttu-id="86172-140">Még egyszer kell megjelennie hello új munkaterületet, fürt, vagy az összes létrehozott ÜVEGVATTA táblák:</span><span class="sxs-lookup"><span data-stu-id="86172-140">Once again you should see hello new workspace, cluster and WAD tables all created:</span></span>

![Service Fabric](./media/log-analytics-service-fabric/5.png)

### <a name="viewing-performance-data"></a><span data-ttu-id="86172-142">Teljesítményadatok</span><span class="sxs-lookup"><span data-stu-id="86172-142">Viewing Performance Data</span></span>

<span data-ttu-id="86172-143">Teljesítményadatok tooview a csomópontjáról:</span><span class="sxs-lookup"><span data-stu-id="86172-143">tooview Perf Data from your nodes:</span></span>


[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

- <span data-ttu-id="86172-144">Indítsa el a Naplóelemzési munkaterület hello hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="86172-144">Launch hello Log Analytics workspace from hello Azure portal.</span></span>
  <span data-ttu-id="86172-145">![Service Fabric](./media/log-analytics-service-fabric/6.png)</span><span class="sxs-lookup"><span data-stu-id="86172-145">![Service Fabric](./media/log-analytics-service-fabric/6.png)</span></span>
- <span data-ttu-id="86172-146">Nyissa meg tooSettings hello bal oldali ablaktáblán, és válassza ki az adatok >> Windows teljesítményszámlálók >> "Add hello kijelölt teljesítményszámlálók": ![Service Fabric](./media/log-analytics-service-fabric/7.png)</span><span class="sxs-lookup"><span data-stu-id="86172-146">Go tooSettings on hello left pane, and select Data >> Windows Performance Counters >> "Add hello selected performance counters": ![Service Fabric](./media/log-analytics-service-fabric/7.png)</span></span>
- <span data-ttu-id="86172-147">A keresési napló azokat a csomópontokat kapcsolatos alapvető metrikákat a lekérdezések toodelve következő hello használata:</span><span class="sxs-lookup"><span data-stu-id="86172-147">In Log Search, use hello following queries toodelve into key metrics about your nodes:</span></span>

    <span data-ttu-id="86172-148">a.</span><span class="sxs-lookup"><span data-stu-id="86172-148">a.</span></span> <span data-ttu-id="86172-149">Hasonlítsa össze a processzorkihasználtság átlagosan hello hello csomópontok problémát tapasztal elmúlt egy óra toosee minden a csomópontra, és milyen időközönként csomópont kellett egy csúcs:</span><span class="sxs-lookup"><span data-stu-id="86172-149">Compare hello average CPU Utilization across all your nodes in hello last one hour toosee which nodes are having issues and at what time interval a node had a spike:</span></span>

    ```
    Type=Perf ObjectName=Processor CounterName="% Processor Time"|measure avg(CounterValue) by Computer Interval 1HOUR.
    ```

    ![Service Fabric](./media/log-analytics-service-fabric/10.png)

    <span data-ttu-id="86172-151">b.</span><span class="sxs-lookup"><span data-stu-id="86172-151">b.</span></span> <span data-ttu-id="86172-152">Megtekintheti a rendelkezésre álló memória hasonló vonaldiagramok lekérdezéshez minden egyes csomóponton:</span><span class="sxs-lookup"><span data-stu-id="86172-152">View similar line charts for available memory on each node with this query:</span></span>

    ```
    Type=Perf ObjectName=Memory CounterName="Available MBytes Memory" | measure avg(CounterValue) by Computer Interval 1HOUR.
    ```

    <span data-ttu-id="86172-153">az összes a csomópont hello pontos átlagos értéket megjelenítő a rendelkezésre álló memória (MB) az egyes csomópontok listáját tooview használja ezt a lekérdezést:</span><span class="sxs-lookup"><span data-stu-id="86172-153">tooview a listing of all your nodes, showing hello exact average value for Available MBytes for each node, use this query:</span></span>

    ```
    Type=Perf (ObjectName=Memory) (CounterName="Available MBytes") | measure avg(CounterValue) by Computer
    ```

    ![Service Fabric](./media/log-analytics-service-fabric/11.png)

    <span data-ttu-id="86172-155">c.</span><span class="sxs-lookup"><span data-stu-id="86172-155">c.</span></span> <span data-ttu-id="86172-156">Hello esetében, hogy a kívánt toodrill le egy adott csomópont hello óránkénti átlag, megvizsgálásával minimális, maximális és 75-PERCENTILIS CPU-használat, Ön tudja toodo ez által a lekérdezés (a név felülírandó számítógép mező) használata:</span><span class="sxs-lookup"><span data-stu-id="86172-156">In hello case that you want toodrill down into a specific node by examining hello hourly average, minimum, maximum and 75-percentile CPU usage, you're able toodo this by using this query (replace Computer field):</span></span>

    ```
    Type=Perf CounterName="% Processor Time" InstanceName=_Total Computer="BaconDC01.BaconLand.com"| measure min(CounterValue), avg(CounterValue), percentile75(CounterValue), max(CounterValue) by Computer Interval 1HOUR
    ```

    ![Service Fabric](./media/log-analytics-service-fabric/12.png)

<span data-ttu-id="86172-158">További információt szeretne teljesítmény metrikákat a Log Analyticshez hello [Operations Management Suite blog](https://blogs.technet.microsoft.com/msoms/tag/metrics/).</span><span class="sxs-lookup"><span data-stu-id="86172-158">Read more information about performance metrics in Log Analytics at hello [Operations Management Suite blog](https://blogs.technet.microsoft.com/msoms/tag/metrics/).</span></span>


## <a name="adding-an-existing-storage-account-toolog-analytics"></a><span data-ttu-id="86172-159">Egy meglévő storage-fiók tooLog Analytics hozzáadása</span><span class="sxs-lookup"><span data-stu-id="86172-159">Adding an existing storage account tooLog Analytics</span></span>

<span data-ttu-id="86172-160">Ez a sablon egyszerűen hozzáadja a meglévő tárolási fiókok tooa új vagy meglévő Naplóelemzési munkaterületet.</span><span class="sxs-lookup"><span data-stu-id="86172-160">This template simply adds your existing storage accounts tooa new or existing Log Analytics workspace.</span></span>

<span data-ttu-id="86172-161">[![TooAzure telepítése](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Foms-existing-storage-account%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="86172-161">[![Deploy tooAzure](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Foms-existing-storage-account%2Fazuredeploy.json)</span></span>

> [!NOTE]
> <span data-ttu-id="86172-162">Csoport kiválasztása egy erőforrást, ha egy már meglévő Naplóelemzési munkaterület dolgozik, válassza ki "Használata meglévő" és a keresési hello hello Naplóelemzési munkaterületet tartalmazó erőforráscsoport.</span><span class="sxs-lookup"><span data-stu-id="86172-162">In selecting a Resource Group, if you're working with an already existing Log Analytics workspace, select "Use Existing" and search for hello resource group containing hello Log Analytics workspace.</span></span> <span data-ttu-id="86172-163">Hozzon létre egy új egy egyébként.</span><span class="sxs-lookup"><span data-stu-id="86172-163">Create a new one if otherwise.</span></span>
> <span data-ttu-id="86172-164">![Service Fabric](./media/log-analytics-service-fabric/8.png)</span><span class="sxs-lookup"><span data-stu-id="86172-164">![Service Fabric](./media/log-analytics-service-fabric/8.png)</span></span>
>
>

<span data-ttu-id="86172-165">Ez a sablon telepítése után fog tudni toosee hello storage-fiók csatlakoztatva tooyour Naplóelemzési munkaterület.</span><span class="sxs-lookup"><span data-stu-id="86172-165">After this template has been deployed, you will be able toosee hello storage account connected tooyour Log Analytics workspace.</span></span> <span data-ttu-id="86172-166">Ebben a példában egy további tárolási fiók toohello Exchange munkaterület fent létrehozott hozzáadott.</span><span class="sxs-lookup"><span data-stu-id="86172-166">In this instance, I added one more storage account toohello Exchange workspace I created above.</span></span>
<span data-ttu-id="86172-167">![Service Fabric](./media/log-analytics-service-fabric/9.png)</span><span class="sxs-lookup"><span data-stu-id="86172-167">![Service Fabric](./media/log-analytics-service-fabric/9.png)</span></span>

## <a name="view-service-fabric-events"></a><span data-ttu-id="86172-168">A Service Fabric-események megtekintése</span><span class="sxs-lookup"><span data-stu-id="86172-168">View Service Fabric events</span></span>

<span data-ttu-id="86172-169">Miután hello központi telepítések és a Service Fabric megoldás hello engedélyezve van a munkaterületen, válassza ki a hello **Service Fabric** hello Log Analytics portál toolaunch hello Service Fabric dashboard csempére.</span><span class="sxs-lookup"><span data-stu-id="86172-169">Once hello deployments are completed and hello Service Fabric solution has been enabled in your workspace, select hello **Service Fabric** tile in hello Log Analytics portal toolaunch hello Service Fabric dashboard.</span></span> <span data-ttu-id="86172-170">hello irányítópult szerepel a következő táblázat hello hello oszlopa.</span><span class="sxs-lookup"><span data-stu-id="86172-170">hello dashboard includes hello columns in hello following table.</span></span> <span data-ttu-id="86172-171">Egyes oszlopok hello felső 10 események száma a hozzá, hogy az oszlop feltételek hello megadott időtartomány sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="86172-171">Each column lists hello top 10 events by count matching that column's criteria for hello specified time range.</span></span> <span data-ttu-id="86172-172">Futtathatja a napló keresési kattintva hello teljes lista biztosító **láthatja az összes** hello jobb alsó oszlopok, vagy hello oszlop fejlécére kattint.</span><span class="sxs-lookup"><span data-stu-id="86172-172">You can run a log search that provides hello entire list by clicking **See all** at hello right bottom of each column, or by clicking hello column header.</span></span>

| <span data-ttu-id="86172-173">**A Service Fabric-esemény**</span><span class="sxs-lookup"><span data-stu-id="86172-173">**Service Fabric event**</span></span> | <span data-ttu-id="86172-174">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="86172-174">**description**</span></span> |
| --- | --- |
| <span data-ttu-id="86172-175">Jelentős problémák</span><span class="sxs-lookup"><span data-stu-id="86172-175">Notable Issues</span></span> |<span data-ttu-id="86172-176">Megjeleníti a problémák, például RunAsyncFailures RunAsynCancellations és csomópont időszakosan megszakadó.</span><span class="sxs-lookup"><span data-stu-id="86172-176">A Display of issues such as RunAsyncFailures RunAsynCancellations and Node Downs.</span></span> |
| <span data-ttu-id="86172-177">A működési események</span><span class="sxs-lookup"><span data-stu-id="86172-177">Operational Events</span></span> |<span data-ttu-id="86172-178">Például az alkalmazásfrissítés és központi telepítések figyelmet a jelentősebb működési eseményeit.</span><span class="sxs-lookup"><span data-stu-id="86172-178">Notable operational events such as application upgrade and deployments.</span></span> |
| <span data-ttu-id="86172-179">Megbízható eseményei</span><span class="sxs-lookup"><span data-stu-id="86172-179">Reliable Service Events</span></span> |<span data-ttu-id="86172-180">Fontos megbízható szolgáltatás események ilyen Runasyncinvocations.</span><span class="sxs-lookup"><span data-stu-id="86172-180">Notable reliable service events such a Runasyncinvocations.</span></span> |
| <span data-ttu-id="86172-181">Szereplő események</span><span class="sxs-lookup"><span data-stu-id="86172-181">Actor Events</span></span> |<span data-ttu-id="86172-182">Fontos szereplő eseményeit a micro-szolgáltatások, például egy aktormetódus, szereplő aktiválások és deactivations, és így tovább által okozott kivételeket.</span><span class="sxs-lookup"><span data-stu-id="86172-182">Notable actor events generated by your micro-services, such as exceptions thrown by an actor method, actor activations and deactivations, and so on.</span></span> |
| <span data-ttu-id="86172-183">Alkalmazás-események</span><span class="sxs-lookup"><span data-stu-id="86172-183">Application Events</span></span> |<span data-ttu-id="86172-184">Minden egyéni ETW által előállított eseményeket az alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="86172-184">All custom ETW events generated by your applications.</span></span> |

![A Service Fabric irányítópult](./media/log-analytics-service-fabric/sf3.png)

![A Service Fabric irányítópult](./media/log-analytics-service-fabric/sf4.png)

<span data-ttu-id="86172-187">hello következő táblázatban adatgyűjtési módszerek és egyéb adatok gyűjtése hogyan Service Fabric részleteit.</span><span class="sxs-lookup"><span data-stu-id="86172-187">hello following table shows data collection methods and other details about how data is collected for Service Fabric.</span></span>

| <span data-ttu-id="86172-188">Platform</span><span class="sxs-lookup"><span data-stu-id="86172-188">platform</span></span> | <span data-ttu-id="86172-189">Közvetlen ügynök</span><span class="sxs-lookup"><span data-stu-id="86172-189">Direct Agent</span></span> | <span data-ttu-id="86172-190">Operations Manager-ügynök</span><span class="sxs-lookup"><span data-stu-id="86172-190">Operations Manager agent</span></span> | <span data-ttu-id="86172-191">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="86172-191">Azure Storage</span></span> | <span data-ttu-id="86172-192">Az Operations Manager szükséges?</span><span class="sxs-lookup"><span data-stu-id="86172-192">Operations Manager required?</span></span> | <span data-ttu-id="86172-193">Az Operations Manager ügynök adatait a felügyeleti csoport keresztül küldött</span><span class="sxs-lookup"><span data-stu-id="86172-193">Operations Manager agent data sent via management group</span></span> | <span data-ttu-id="86172-194">Gyűjtemény gyakorisága</span><span class="sxs-lookup"><span data-stu-id="86172-194">collection frequency</span></span> |
| --- | --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="86172-195">Windows</span><span class="sxs-lookup"><span data-stu-id="86172-195">Windows</span></span> |  |  | <span data-ttu-id="86172-196">&#8226;</span><span class="sxs-lookup"><span data-stu-id="86172-196">&#8226;</span></span> |  |  |<span data-ttu-id="86172-197">10 perc</span><span class="sxs-lookup"><span data-stu-id="86172-197">10 minutes</span></span> |

> [!NOTE]
> <span data-ttu-id="86172-198">Ezek az események a Service Fabric megoldás hello hello hatókör kattintva módosíthat **adatok alapján az elmúlt 7 napban** hello irányítópult hello tetején.</span><span class="sxs-lookup"><span data-stu-id="86172-198">You can change hello scope of these events in hello Service Fabric solution by clicking **Data based on last 7 days** at hello top of hello dashboard.</span></span> <span data-ttu-id="86172-199">Elmúlt hét napban, egy nap vagy hat órán belül hello generált események akkor is megjelenhet.</span><span class="sxs-lookup"><span data-stu-id="86172-199">You can also show events generated within hello last seven days, one day, or six hours.</span></span> <span data-ttu-id="86172-200">Vagy választhat **egyéni** toospecify egyéni dátumtartományt.</span><span class="sxs-lookup"><span data-stu-id="86172-200">Or, you can select **Custom** toospecify a custom date range.</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="86172-201">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="86172-201">Next steps</span></span>

* <span data-ttu-id="86172-202">Használjon [napló megkeresi a Naplóelemzési](log-analytics-log-searches.md) tooview részletes Service Fabric eseményadatok.</span><span class="sxs-lookup"><span data-stu-id="86172-202">Use [Log Searches in Log Analytics](log-analytics-log-searches.md) tooview detailed Service Fabric event data.</span></span>
