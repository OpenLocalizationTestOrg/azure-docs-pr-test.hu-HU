---
title: "az Azure Service Fabric aaaReport és ellenőrzés állapotának |} Microsoft Docs"
description: "Ismerje meg, hogyan toosend rendszerállapot-jelentéseket a szolgáltatás kódból és hogyan toocheck hello állapotát a szolgáltatás adott Azure Service Fabric hello állapotfigyelési eszközök használatával biztosít."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: mfussell
editor: 
ms.assetid: 7c712c22-d333-44bc-b837-d0b3603d9da8
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/19/2017
ms.author: dekapur
ms.openlocfilehash: bcb838fefe3f2054447e1731d709e455560260e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="report-and-check-service-health"></a><span data-ttu-id="7d348-103">Szolgáltatásállapot jelentése és ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="7d348-103">Report and check service health</span></span>
<span data-ttu-id="7d348-104">A szolgáltatások problémája, amikor a képes toorespond tooand javítás incidensek és kimaradások függ a képes toodetect hello problémák gyorsan.</span><span class="sxs-lookup"><span data-stu-id="7d348-104">When your services encounter problems, your ability toorespond tooand fix incidents and outages depends on your ability toodetect hello issues quickly.</span></span> <span data-ttu-id="7d348-105">Ha a szolgáltatás kódból jelentéssel a problémák és hibák toohello Azure Service Fabric állapotkezelő szabványos állapotfigyelési, hogy a Service Fabric toocheck hello állapot biztosít eszközök is használhatja.</span><span class="sxs-lookup"><span data-stu-id="7d348-105">If you report problems and failures toohello Azure Service Fabric health manager from your service code, you can use standard health monitoring tools that Service Fabric provides toocheck hello health status.</span></span>

<span data-ttu-id="7d348-106">Három módon, hogy jelenthetik-e a hello szolgáltatás állapotát:</span><span class="sxs-lookup"><span data-stu-id="7d348-106">There are three ways that you can report health from hello service:</span></span>

* <span data-ttu-id="7d348-107">Használjon [partíció](https://docs.microsoft.com/dotnet/api/system.fabric.istatefulservicepartition) vagy [CodePackageActivationContext](https://docs.microsoft.com/dotnet/api/system.fabric.codepackageactivationcontext) objektumok.</span><span class="sxs-lookup"><span data-stu-id="7d348-107">Use [Partition](https://docs.microsoft.com/dotnet/api/system.fabric.istatefulservicepartition) or [CodePackageActivationContext](https://docs.microsoft.com/dotnet/api/system.fabric.codepackageactivationcontext) objects.</span></span>  
  <span data-ttu-id="7d348-108">Használhatja a hello `Partition` és `CodePackageActivationContext` tooreport hello állapotát elemeit tartalmazza az aktuális környezetben hello objektumokat.</span><span class="sxs-lookup"><span data-stu-id="7d348-108">You can use hello `Partition` and `CodePackageActivationContext` objects tooreport hello health of elements that are part of hello current context.</span></span> <span data-ttu-id="7d348-109">Például egy replikát részeként futó kódok is jelentés állapotfigyelő csak, hogy a replika tartozik hello partíció és hello alkalmazás, amely egy része.</span><span class="sxs-lookup"><span data-stu-id="7d348-109">For example, code that runs as part of a replica can report health only on that replica, hello partition that it belongs to, and hello application that it is a part of.</span></span>
* <span data-ttu-id="7d348-110">Használjon `FabricClient`.</span><span class="sxs-lookup"><span data-stu-id="7d348-110">Use `FabricClient`.</span></span>   
  <span data-ttu-id="7d348-111">Használhat `FabricClient` tooreport állapotfigyelő hello szolgáltatás kódból Ha hello fürt nem [biztonságos](service-fabric-cluster-security.md) , vagy ha hello szolgáltatás rendszergazdai jogosultságokkal futtatja.</span><span class="sxs-lookup"><span data-stu-id="7d348-111">You can use `FabricClient` tooreport health from hello service code if hello cluster is not [secure](service-fabric-cluster-security.md) or if hello service is running with admin privileges.</span></span> <span data-ttu-id="7d348-112">A legtöbb olyan valós forgatókönyv a nem biztonságos fürtök használata, vagy adjon meg rendszergazdai jogosultságokkal.</span><span class="sxs-lookup"><span data-stu-id="7d348-112">Most real-world scenarios do not use unsecured clusters, or provide admin privileges.</span></span> <span data-ttu-id="7d348-113">A `FabricClient`, minden entitás, amely hello fürt része a rendszerállapot jelentést.</span><span class="sxs-lookup"><span data-stu-id="7d348-113">With `FabricClient`, you can report health on any entity that is a part of hello cluster.</span></span> <span data-ttu-id="7d348-114">Ideális esetben azonban szolgáltatás kódot csak küldjön kapcsolódó tooits egészségügyi jelentéseket.</span><span class="sxs-lookup"><span data-stu-id="7d348-114">Ideally, however, service code should only send reports that are related tooits own health.</span></span>
* <span data-ttu-id="7d348-115">Használjon hello REST API-k hello fürt, alkalmazások, központilag telepített alkalmazás, szolgáltatás, szolgáltatáscsomagot, partíció, replika, vagy a csomópont szintek.</span><span class="sxs-lookup"><span data-stu-id="7d348-115">Use hello REST APIs at hello cluster, application, deployed application, service, service package, partition, replica, or node levels.</span></span> <span data-ttu-id="7d348-116">Ez lehet a használt tooreport állapotát a tárolóban.</span><span class="sxs-lookup"><span data-stu-id="7d348-116">This can be used tooreport health from within a container.</span></span>

<span data-ttu-id="7d348-117">Ez a cikk végigvezeti egy példa, amely a jelentések hello szolgáltatás kódot az állapotát.</span><span class="sxs-lookup"><span data-stu-id="7d348-117">This article walks you through an example that reports health from hello service code.</span></span> <span data-ttu-id="7d348-118">hello példa azt is bemutatja, hogyan a Service Fabric által biztosított hello eszközöket lehet használt toocheck hello állapotát.</span><span class="sxs-lookup"><span data-stu-id="7d348-118">hello example also shows how hello tools provided by Service Fabric can be used toocheck hello health status.</span></span> <span data-ttu-id="7d348-119">Ez a cikk tervezett toobe van egy gyors bevezetés toohello állapotfigyelési Service Fabric képességeit.</span><span class="sxs-lookup"><span data-stu-id="7d348-119">This article is intended toobe a quick introduction toohello health monitoring capabilities of Service Fabric.</span></span> <span data-ttu-id="7d348-120">További részletes információt részletes ismertető cikk gyűjtemény állapotát a cikk végén hello hello hivatkozás kezdődő hello adatsorozat érheti el.</span><span class="sxs-lookup"><span data-stu-id="7d348-120">For more detailed information, you can read hello series of in-depth articles about health that start with hello link at hello end of this article.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7d348-121">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="7d348-121">Prerequisites</span></span>
<span data-ttu-id="7d348-122">Hello következőkkel kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="7d348-122">You must have hello following installed:</span></span>

* <span data-ttu-id="7d348-123">Visual Studio 2015-öt vagy a Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="7d348-123">Visual Studio 2015 or Visual Studio 2017</span></span>
* <span data-ttu-id="7d348-124">A Service Fabric SDK</span><span class="sxs-lookup"><span data-stu-id="7d348-124">Service Fabric SDK</span></span>

## <a name="toocreate-a-local-secure-dev-cluster"></a><span data-ttu-id="7d348-125">egy helyi biztonságos fejlesztési fürtöt toocreate</span><span class="sxs-lookup"><span data-stu-id="7d348-125">toocreate a local secure dev cluster</span></span>
* <span data-ttu-id="7d348-126">Nyissa meg a Powershellt rendszergazdai jogosultságokkal, és futtassa a következő parancsok hello:</span><span class="sxs-lookup"><span data-stu-id="7d348-126">Open PowerShell with admin privileges, and run hello following commands:</span></span>

![A parancsokat, hogy hogyan toocreate egy biztonságos fejlesztési fürtöt](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/create-secure-dev-cluster.png)

## <a name="toodeploy-an-application-and-check-its-health"></a><span data-ttu-id="7d348-128">egy alkalmazás toodeploy és állapotának ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="7d348-128">toodeploy an application and check its health</span></span>
1. <span data-ttu-id="7d348-129">Nyissa meg a Visual Studio rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="7d348-129">Open Visual Studio as an administrator.</span></span>
2. <span data-ttu-id="7d348-130">A projekt létrehozása hello segítségével **állapotalapú alkalmazások és szolgáltatások szolgáltatás** sablont.</span><span class="sxs-lookup"><span data-stu-id="7d348-130">Create a project by using hello **Stateful Service** template.</span></span>
   
    ![Állapotalapú szolgáltatással a Service Fabric-alkalmazás létrehozása](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/create-stateful-service-application-dialog.png)
3. <span data-ttu-id="7d348-132">Nyomja le az **F5** toorun hello alkalmazás hibakeresési módban.</span><span class="sxs-lookup"><span data-stu-id="7d348-132">Press **F5** toorun hello application in debug mode.</span></span> <span data-ttu-id="7d348-133">hello alkalmazás telepített toohello helyi fürtön.</span><span class="sxs-lookup"><span data-stu-id="7d348-133">hello application is deployed toohello local cluster.</span></span>
4. <span data-ttu-id="7d348-134">Miután hello alkalmazás fut, kattintson a jobb gombbal a hello Local Cluster Manager hello értesítési területen megjelenő ikonra, és válassza ki **helyi fürt kezelése** a hello helyi menü tooopen Service Fabric Explorerben talál.</span><span class="sxs-lookup"><span data-stu-id="7d348-134">After hello application is running, right-click hello Local Cluster Manager icon in hello notification area and select **Manage Local Cluster** from hello shortcut menu tooopen Service Fabric Explorer.</span></span>
   
    ![Nyissa meg a Service Fabric Explorer az értesítési területről](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/LaunchSFX.png)
5. <span data-ttu-id="7d348-136">hello alkalmazás állapotának üzenetnek kell megjelennie, mint a kép.</span><span class="sxs-lookup"><span data-stu-id="7d348-136">hello application health should be displayed as in this image.</span></span> <span data-ttu-id="7d348-137">Ilyenkor hello alkalmazás kifogástalan, és hiba nélkül kell lennie.</span><span class="sxs-lookup"><span data-stu-id="7d348-137">At this time, hello application should be healthy with no errors.</span></span>
   
    ![A Service Fabric Explorerben megfelelő alkalmazás](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/sfx-healthy-app.png)
6. <span data-ttu-id="7d348-139">Hello állapotát a PowerShell használatával is ellenőrizheti.</span><span class="sxs-lookup"><span data-stu-id="7d348-139">You can also check hello health by using PowerShell.</span></span> <span data-ttu-id="7d348-140">Használhat ```Get-ServiceFabricApplicationHealth``` használhatja az alkalmazás állapotát, és toocheck ```Get-ServiceFabricServiceHealth``` toocheck egy szolgáltatás állapotát.</span><span class="sxs-lookup"><span data-stu-id="7d348-140">You can use ```Get-ServiceFabricApplicationHealth``` toocheck an application's health, and you can use ```Get-ServiceFabricServiceHealth``` toocheck a service's health.</span></span> <span data-ttu-id="7d348-141">hello állapotjelentése az hello ugyanahhoz az alkalmazáshoz a PowerShellben megtalálható-e a lemezkép.</span><span class="sxs-lookup"><span data-stu-id="7d348-141">hello health report for hello same application in PowerShell is in this image.</span></span>
   
    ![A PowerShell megfelelő alkalmazás](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/ps-healthy-app-report.png)

## <a name="tooadd-custom-health-events-tooyour-service-code"></a><span data-ttu-id="7d348-143">tooadd egyéni állapotfigyelő események tooyour szolgáltatáskódot</span><span class="sxs-lookup"><span data-stu-id="7d348-143">tooadd custom health events tooyour service code</span></span>
<span data-ttu-id="7d348-144">hello Service Fabric a Visual Studio projektsablonjai példakódot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="7d348-144">hello Service Fabric project templates in Visual Studio contain sample code.</span></span> <span data-ttu-id="7d348-145">hello következő lépések bemutatják, hogyan lehet jelentést egyéni állapotával kapcsolatos események a szolgáltatás kódból.</span><span class="sxs-lookup"><span data-stu-id="7d348-145">hello following steps show how you can report custom health events from your service code.</span></span> <span data-ttu-id="7d348-146">Ezek a jelentések jelennek meg automatikusan, hogy a Service Fabric biztosítanak, például a Service Fabric Explorer, az Azure portál állapotának megtekintése és PowerShell állapotfigyelési hello szabványos eszközei.</span><span class="sxs-lookup"><span data-stu-id="7d348-146">Such reports show up automatically in hello standard tools for health monitoring that Service Fabric provides, such as Service Fabric Explorer, Azure portal health view, and PowerShell.</span></span>

1. <span data-ttu-id="7d348-147">Nyissa meg újra a Visual Studio korábban létrehozott alkalmazás hello, vagy hozzon létre egy új alkalmazást hello segítségével **állapotalapú alkalmazások és szolgáltatások szolgáltatás** Visual Studio-sablont.</span><span class="sxs-lookup"><span data-stu-id="7d348-147">Reopen hello application that you created previously in Visual Studio, or create a new application by using hello **Stateful Service** Visual Studio template.</span></span>
2. <span data-ttu-id="7d348-148">Nyissa meg hello Stateful1.cs fájlt, és megkeresi hello `myDictionary.TryGetValueAsync` hello-hívás `RunAsync` metódust.</span><span class="sxs-lookup"><span data-stu-id="7d348-148">Open hello Stateful1.cs file, and find hello `myDictionary.TryGetValueAsync` call in hello `RunAsync` method.</span></span> <span data-ttu-id="7d348-149">Láthatja, hogy ez a metódus visszaadja a `result` , hogy tartás hello hello számláló aktuális értéke, mert ezt az alkalmazást a hello kulcs logikai tookeep száma futó.</span><span class="sxs-lookup"><span data-stu-id="7d348-149">You can see that this method returns a `result` that holds hello current value of hello counter because hello key logic in this application is tookeep a count running.</span></span> <span data-ttu-id="7d348-150">Ha ez egy valós alkalmazás, és az eredmény hello hiánya képviselt Hiba, célszerű az esemény tooflag.</span><span class="sxs-lookup"><span data-stu-id="7d348-150">If this were a real application, and if hello lack of result represented a failure, you would want tooflag that event.</span></span>
3. <span data-ttu-id="7d348-151">tooreport eredmény hello hiánya jelenti. a hiba, ha olyan állapotesemény hello lépések hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="7d348-151">tooreport a health event when hello lack of result represents a failure, add hello following steps.</span></span>
   
    <span data-ttu-id="7d348-152">a.</span><span class="sxs-lookup"><span data-stu-id="7d348-152">a.</span></span> <span data-ttu-id="7d348-153">Adja hozzá a hello `System.Fabric.Health` névtér toohello Stateful1.cs fájlt.</span><span class="sxs-lookup"><span data-stu-id="7d348-153">Add hello `System.Fabric.Health` namespace toohello Stateful1.cs file.</span></span>
   
    ```csharp
    using System.Fabric.Health;
    ```
   
    <span data-ttu-id="7d348-154">b.</span><span class="sxs-lookup"><span data-stu-id="7d348-154">b.</span></span> <span data-ttu-id="7d348-155">Adja hozzá a következő kód után hello hello `myDictionary.TryGetValueAsync` hívása</span><span class="sxs-lookup"><span data-stu-id="7d348-155">Add hello following code after hello `myDictionary.TryGetValueAsync` call</span></span>
   
    ```csharp
    if (!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportReplicaHealth(healthInformation);
    }
    ```
    <span data-ttu-id="7d348-156">A replika állapota azt jelenti, mivel azt egy állapotalapú szolgáltatásból jelentve.</span><span class="sxs-lookup"><span data-stu-id="7d348-156">We report replica health because it's being reported from a stateful service.</span></span> <span data-ttu-id="7d348-157">Hello `HealthInformation` paraméter ügynökökről hello állapottal kapcsolatos probléma kapcsolatos információkat tárolja.</span><span class="sxs-lookup"><span data-stu-id="7d348-157">hello `HealthInformation` parameter stores information about hello health issue that's being reported.</span></span>
   
    <span data-ttu-id="7d348-158">Ha állapotmentes szolgáltatások hozna létre, használja a következő kód hello</span><span class="sxs-lookup"><span data-stu-id="7d348-158">If you had created a stateless service, use hello following code</span></span>
   
    ```csharp
    if (!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportInstanceHealth(healthInformation);
    }
    ```
4. <span data-ttu-id="7d348-159">Ha a szolgáltatás rendszergazda jogosultságokkal fut, vagy ha hello fürt nincs [biztonságos](service-fabric-cluster-security.md), is `FabricClient` tooreport állapotát, ahogy az alábbi lépésekkel hello.</span><span class="sxs-lookup"><span data-stu-id="7d348-159">If your service is running with admin privileges or if hello cluster is not [secure](service-fabric-cluster-security.md), you can also use `FabricClient` tooreport health as shown in hello following steps.</span></span>  
   
    <span data-ttu-id="7d348-160">a.</span><span class="sxs-lookup"><span data-stu-id="7d348-160">a.</span></span> <span data-ttu-id="7d348-161">Hozzon létre hello `FabricClient` után hello példány `var myDictionary` nyilatkozatot.</span><span class="sxs-lookup"><span data-stu-id="7d348-161">Create hello `FabricClient` instance after hello `var myDictionary` declaration.</span></span>
   
    ```csharp
    var fabricClient = new FabricClient(new FabricClientSettings() { HealthReportSendInterval = TimeSpan.FromSeconds(0) });
    ```
   
    <span data-ttu-id="7d348-162">b.</span><span class="sxs-lookup"><span data-stu-id="7d348-162">b.</span></span> <span data-ttu-id="7d348-163">Adja hozzá a következő kód után hello hello `myDictionary.TryGetValueAsync` hívható meg.</span><span class="sxs-lookup"><span data-stu-id="7d348-163">Add hello following code after hello `myDictionary.TryGetValueAsync` call.</span></span>
   
    ```csharp
    if (!result.HasValue)
    {
       var replicaHealthReport = new StatefulServiceReplicaHealthReport(
            this.Context.PartitionId,
            this.Context.ReplicaId,
            new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error));
        fabricClient.HealthManager.ReportHealth(replicaHealthReport);
    }
    ```
5. <span data-ttu-id="7d348-164">Most szimulálása a hiba, és látni hello állapota Hálózatfigyelő eszközök megjelennek.</span><span class="sxs-lookup"><span data-stu-id="7d348-164">Let's simulate this failure and see it show up in hello health monitoring tools.</span></span> <span data-ttu-id="7d348-165">toosimulate hello hiba, hello első sort a korábban hozzáadott kódot reporting hello állapotfigyelő megjegyzésbe.</span><span class="sxs-lookup"><span data-stu-id="7d348-165">toosimulate hello failure, comment out hello first line in hello health reporting code that you added earlier.</span></span> <span data-ttu-id="7d348-166">Miután az első sor hello megjegyzéssé, hello kód a következő példa hello fog megjelenni.</span><span class="sxs-lookup"><span data-stu-id="7d348-166">After you comment out hello first line, hello code will look like hello following example.</span></span>
   
    ```csharp
    //if(!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportReplicaHealth(healthInformation);
    }
    ```
   <span data-ttu-id="7d348-167">Ezzel a kóddal akkor következik be, hello állapotjelentése minden alkalommal, amikor `RunAsync` hajt végre.</span><span class="sxs-lookup"><span data-stu-id="7d348-167">This code fires hello health report each time `RunAsync` executes.</span></span> <span data-ttu-id="7d348-168">Hello módosítása után nyomja le az **F5** toorun hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="7d348-168">After you make hello change, press **F5** toorun hello application.</span></span>
6. <span data-ttu-id="7d348-169">Miután hello alkalmazás fut, nyissa meg a Service Fabric Explorer toocheck hello hello alkalmazás állapotáról.</span><span class="sxs-lookup"><span data-stu-id="7d348-169">After hello application is running, open Service Fabric Explorer toocheck hello health of hello application.</span></span> <span data-ttu-id="7d348-170">Megadott idő Service Fabric Explorer mutatja, hogy hello alkalmazás nem kifogástalan.</span><span class="sxs-lookup"><span data-stu-id="7d348-170">This time, Service Fabric Explorer shows that hello application is unhealthy.</span></span> <span data-ttu-id="7d348-171">Ez a jelentett hello kódból, hogy azt korábban hozzáadott hello hiba miatt.</span><span class="sxs-lookup"><span data-stu-id="7d348-171">This is because of hello error that was reported from hello code that we added previously.</span></span>
   
    ![A Service Fabric Explorerben a nem megfelelő alkalmazás](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/sfx-unhealthy-app.png)
7. <span data-ttu-id="7d348-173">Ha hello faszerkezetes nézetben a Service Fabric Explorer hello elsődleges másodpéldány, láthatja, hogy **állapota** jelzi a hiba, túl.</span><span class="sxs-lookup"><span data-stu-id="7d348-173">If you select hello primary replica in hello tree view of Service Fabric Explorer, you will see that **Health State** indicates an error, too.</span></span> <span data-ttu-id="7d348-174">Service Fabric Explorer is megjeleníti, amelyek részleteit toohello hello állapotjelentése `HealthInformation` hello kódban paraméter.</span><span class="sxs-lookup"><span data-stu-id="7d348-174">Service Fabric Explorer also displays hello health report details that were added toohello `HealthInformation` parameter in hello code.</span></span> <span data-ttu-id="7d348-175">Megtekintheti a PowerShellben ugyanazt állapotjelentések hello és hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="7d348-175">You can see hello same health reports in PowerShell and hello Azure portal.</span></span>
   
    ![A Service Fabric Explorerben a replika állapota](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/replica-health-error-report-sfx.png)

<span data-ttu-id="7d348-177">Ez a jelentés marad, a hello állapotkezelő azt egy másik jelentés, vagy amíg a replika nem cseréli le.</span><span class="sxs-lookup"><span data-stu-id="7d348-177">This report remains in hello health manager until it is replaced by another report or until this replica is deleted.</span></span> <span data-ttu-id="7d348-178">Mivel jelenleg nincs beállítva `TimeToLive` a rendszerállapot-jelentés hello a `HealthInformation` objektum hello jelentés soha nem jár le.</span><span class="sxs-lookup"><span data-stu-id="7d348-178">Because we did not set `TimeToLive` for this health report in hello `HealthInformation` object, hello report never expires.</span></span>

<span data-ttu-id="7d348-179">Azt javasoljuk, hogy egészségügyi hello legtöbb részletes szinten, amely a jelen esetben ez hello replika kell megadni.</span><span class="sxs-lookup"><span data-stu-id="7d348-179">We recommend that health should be reported on hello most granular level, which in this case is hello replica.</span></span> <span data-ttu-id="7d348-180">A health is jelentheti `Partition`.</span><span class="sxs-lookup"><span data-stu-id="7d348-180">You can also report health on `Partition`.</span></span>

```csharp
HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
this.Partition.ReportPartitionHealth(healthInformation);
```

<span data-ttu-id="7d348-181">tooreport-állapot `Application`, `DeployedApplication`, és `DeployedServicePackage`, használjon `CodePackageActivationContext`.</span><span class="sxs-lookup"><span data-stu-id="7d348-181">tooreport health on `Application`, `DeployedApplication`, and `DeployedServicePackage`, use  `CodePackageActivationContext`.</span></span>

```csharp
HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
var activationContext = FabricRuntime.GetActivationContext();
activationContext.ReportApplicationHealth(healthInformation);
```

## <a name="next-steps"></a><span data-ttu-id="7d348-182">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7d348-182">Next steps</span></span>
* [<span data-ttu-id="7d348-183">Részletes bemutatója a Service Fabric állapota</span><span class="sxs-lookup"><span data-stu-id="7d348-183">Deep dive on Service Fabric health</span></span>](service-fabric-health-introduction.md)
* [<span data-ttu-id="7d348-184">REST API-t a jelentéskészítési szolgáltatás állapota</span><span class="sxs-lookup"><span data-stu-id="7d348-184">REST API for reporting service health</span></span>](https://docs.microsoft.com/rest/api/servicefabric/report-the-health-of-a-service)
* [<span data-ttu-id="7d348-185">REST API-alkalmazás állapotának jelentéskészítéshez</span><span class="sxs-lookup"><span data-stu-id="7d348-185">REST API for reporting application health</span></span>](https://docs.microsoft.com/rest/api/servicefabric/report-the-health-of-an-application)

