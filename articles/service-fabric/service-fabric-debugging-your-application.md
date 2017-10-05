---
title: "A Visual Studio alkalmazás hibakeresése |} Microsoft Docs"
description: "Tovább fejlesztheti megbízhatóságának és teljesítményének a szolgáltatások fejlesztéséhez és a helyi fejlesztési fürtök hibakeresés a Visual Studio őket."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: cb888532-bcdb-4e47-95e4-bfbb1f644da4
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/29/2017
ms.author: vturecek;mikhegn
ms.openlocfilehash: 2459025899a7f5ffebf44fa104ed112c0eb99dfa
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="debug-your-service-fabric-application-by-using-visual-studio"></a><span data-ttu-id="ef121-103">A Service Fabric-alkalmazás hibakeresése a Visual Studio használatával</span><span class="sxs-lookup"><span data-stu-id="ef121-103">Debug your Service Fabric application by using Visual Studio</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="ef121-104">A Visual Studio/CSharp</span><span class="sxs-lookup"><span data-stu-id="ef121-104">Visual Studio/CSharp</span></span>](service-fabric-debugging-your-application.md) 
> * [<span data-ttu-id="ef121-105">Eclipse/Java</span><span class="sxs-lookup"><span data-stu-id="ef121-105">Eclipse/Java</span></span>](service-fabric-debugging-your-application-java.md)
>


## <a name="debug-a-local-service-fabric-application"></a><span data-ttu-id="ef121-106">Egy helyi Service Fabric-alkalmazás hibakeresése</span><span class="sxs-lookup"><span data-stu-id="ef121-106">Debug a local Service Fabric application</span></span>
<span data-ttu-id="ef121-107">Időt és pénzt takaríthat telepítésével és az Azure Service Fabric-alkalmazás egy számítógép helyi fejlesztési fürtöt a hibakeresést.</span><span class="sxs-lookup"><span data-stu-id="ef121-107">You can save time and money by deploying and debugging your Azure Service Fabric application in a local computer development cluster.</span></span> <span data-ttu-id="ef121-108">A Visual Studio 2017 vagy Visual Studio 2015 Telepítse központilag az alkalmazást a helyi fürthöz, és automatikusan csatlakoznak a hibakereső az alkalmazás összes példányát.</span><span class="sxs-lookup"><span data-stu-id="ef121-108">Visual Studio 2017 or Visual Studio 2015 can deploy the application to the local cluster and automatically connect the debugger to all instances of your application.</span></span>

1. <span data-ttu-id="ef121-109">Indítsa el a helyi fejlesztési fürtök lépéseit követve [a Service Fabric fejlesztési környezet létrehozása](service-fabric-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="ef121-109">Start a local development cluster by following the steps in [Setting up your Service Fabric development environment](service-fabric-get-started.md).</span></span>
2. <span data-ttu-id="ef121-110">Nyomja le az **F5** , vagy kattintson a **Debug** > **Start Debugging**.</span><span class="sxs-lookup"><span data-stu-id="ef121-110">Press **F5** or click **Debug** > **Start Debugging**.</span></span>
   
    ![Egy alkalmazást a hibakeresés elindításához][startdebugging]
3. <span data-ttu-id="ef121-112">Állítson be töréspontokat a kódot és az alkalmazás lépésenként parancsok kattintva a **Debug** menü.</span><span class="sxs-lookup"><span data-stu-id="ef121-112">Set breakpoints in your code and step through the application by clicking commands in the **Debug** menu.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="ef121-113">A Visual Studio csatolja az alkalmazás összes példányát.</span><span class="sxs-lookup"><span data-stu-id="ef121-113">Visual Studio attaches to all instances of your application.</span></span> <span data-ttu-id="ef121-114">Kód most lépjen, amíg töréspontok előfordulhat, hogy beolvasása kattintson az egyidejű munkamenetek eredményezve több folyamat.</span><span class="sxs-lookup"><span data-stu-id="ef121-114">While you're stepping through code, breakpoints may get hit by multiple processes resulting in concurrent sessions.</span></span> <span data-ttu-id="ef121-115">Próbálja a töréspontok letiltása után azok még találat, azáltal, hogy minden töréspont feltétele a Szálazonosító vagy diagnosztikai eseményeket használ.</span><span class="sxs-lookup"><span data-stu-id="ef121-115">Try disabling the breakpoints after they're hit, by making each breakpoint conditional on the thread ID or by using diagnostic events.</span></span>
   > 
   > 
4. <span data-ttu-id="ef121-116">A **a diagnosztikai** automatikusan megnyílik, megtekintheti a diagnosztikai valós időben.</span><span class="sxs-lookup"><span data-stu-id="ef121-116">The **Diagnostic Events** window will automatically open so you can view diagnostic events in real time.</span></span>
   
    ![Valós idejű diagnosztikai eseményeinek megtekintése][diagnosticevents]
5. <span data-ttu-id="ef121-118">Is megnyithatja a **a diagnosztikai** Cloud Explorer ablakban.</span><span class="sxs-lookup"><span data-stu-id="ef121-118">You can also open the **Diagnostic Events** window in Cloud Explorer.</span></span>  <span data-ttu-id="ef121-119">A **Service Fabric**, kattintson a jobb gombbal bármelyik csomópont, és válassza a **nézet adatfolyam-nyomkövetések**.</span><span class="sxs-lookup"><span data-stu-id="ef121-119">Under **Service Fabric**, right-click any node and choose **View Streaming Traces**.</span></span>
   
    ![A diagnosztikai események ablak megnyitása][viewdiagnosticevents]
   
    <span data-ttu-id="ef121-121">Ha szeretne szűrni a nyomkövetések és egy adott szolgáltatás vagy alkalmazás, egyszerűen engedélyezése, hogy adott szolgáltatás vagy alkalmazás adatfolyam címein.</span><span class="sxs-lookup"><span data-stu-id="ef121-121">If you want to filter your traces to a specific service or application, simply enable streaming traces on that specific service or application.</span></span>
6. <span data-ttu-id="ef121-122">A diagnosztikai események láthatók a automatikusan létrehozott **ServiceEventSource.cs** fájlt, és az alkalmazás kódjában nevezzük.</span><span class="sxs-lookup"><span data-stu-id="ef121-122">The diagnostic events can be seen in the automatically generated **ServiceEventSource.cs** file and are called from application code.</span></span>
   
    ```csharp
    ServiceEventSource.Current.ServiceMessage(this, "My ServiceMessage with a parameter {0}", result.Value.ToString());
    ```
7. <span data-ttu-id="ef121-123">A **a diagnosztikai** ablak támogatja a szűrést, a felfüggesztés és a valós idejű események ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="ef121-123">The **Diagnostic Events** window supports filtering, pausing, and inspecting events in real time.</span></span>  <span data-ttu-id="ef121-124">A szűrő egy egyszerű karakterlánc-keresés az eseményüzenet, beleértve annak tartalmát.</span><span class="sxs-lookup"><span data-stu-id="ef121-124">The filter is a simple string search of the event message, including its contents.</span></span>
   
    ![Szűréséhez, szüneteltetése és folytatása és vizsgálja meg a valós idejű események][diagnosticeventsactions]
8. <span data-ttu-id="ef121-126">Hibakereső szolgáltatásának olyan, mint bármely más alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="ef121-126">Debugging services is like debugging any other application.</span></span> <span data-ttu-id="ef121-127">Visual Studio alkalmazással töréspontok általában könnyen hibakeresési állítja be.</span><span class="sxs-lookup"><span data-stu-id="ef121-127">You will normally set Breakpoints through Visual Studio for easy debugging.</span></span> <span data-ttu-id="ef121-128">Annak ellenére, hogy a megbízható gyűjtemények replikálásához több csomópont, azok továbbra is valósítania az IEnumerable illesztőfelületet.</span><span class="sxs-lookup"><span data-stu-id="ef121-128">Even though Reliable Collections replicate across multiple nodes, they still implement IEnumerable.</span></span> <span data-ttu-id="ef121-129">Ez azt jelenti, hogy az eredmények megtekintése a Visual Studio során használhatók hibakeresés megjelenítéséhez belül már tárolja.</span><span class="sxs-lookup"><span data-stu-id="ef121-129">This means that you can use the Results View in Visual Studio while debugging to see what you've stored inside.</span></span> <span data-ttu-id="ef121-130">Egyszerűen állítson be egy töréspontot bárhol a kódban.</span><span class="sxs-lookup"><span data-stu-id="ef121-130">Simply set a breakpoint anywhere in your code.</span></span>
   
    ![Egy alkalmazást a hibakeresés elindításához][breakpoint]

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->

## <a name="debug-a-remote-service-fabric-application"></a><span data-ttu-id="ef121-132">A távoli Service Fabric-alkalmazás hibakeresése</span><span class="sxs-lookup"><span data-stu-id="ef121-132">Debug a remote Service Fabric application</span></span>
<span data-ttu-id="ef121-133">Ha a Service Fabric-alkalmazások az Azure Service Fabric-fürt futtatja, akkor is távolról hibakeresése a Visual Studio-ről.</span><span class="sxs-lookup"><span data-stu-id="ef121-133">If your Service Fabric applications are running on a Service Fabric cluster in Azure, you are able to remotely debug these, directly from Visual Studio.</span></span>

> [!NOTE]
> <span data-ttu-id="ef121-134">A funkció használatához [Service Fabric SDK 2.0](http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015) és [Azure SDK for .NET 2.9](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="ef121-134">The feature requires [Service Fabric SDK 2.0](http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015) and [Azure SDK for .NET 2.9](https://azure.microsoft.com/downloads/).</span></span>    
> 
> 

<!-- -->
> [!WARNING]
> <span data-ttu-id="ef121-135">Fejlesztési/Tesztelési forgatókönyvek, és nem használható üzemi környezetben, mert a futó alkalmazások gyakorolt hatás célja a távoli hibakeresés.</span><span class="sxs-lookup"><span data-stu-id="ef121-135">Remote debugging is meant for dev/test scenarios and not to be used in production environments, because of the impact on the running applications.</span></span>
> 
> 

1. <span data-ttu-id="ef121-136">Keresse meg a fürt **Cloud Explorer**, kattintson a jobb gombbal, és válassza a **hibakeresés engedélyezése**</span><span class="sxs-lookup"><span data-stu-id="ef121-136">Navigate to your cluster in **Cloud Explorer**, right-click and choose **Enable Debugging**</span></span>
   
    ![Távoli hibakeresésének engedélyezése][enableremotedebugging]
   
    <span data-ttu-id="ef121-138">Ez fogja indítsa a távoli hibakeresési bővítménnyel a fürtcsomópontokat, valamint a szükséges hálózati konfigurációk folyamata.</span><span class="sxs-lookup"><span data-stu-id="ef121-138">This will kick off the process of enabling the remote debugging extension on your cluster nodes, as well as required network configurations.</span></span>
2. <span data-ttu-id="ef121-139">Kattintson a jobb gombbal a fürt csomópontja **Cloud Explorer**, és válassza a **hibakereső csatolása**</span><span class="sxs-lookup"><span data-stu-id="ef121-139">Right-click the cluster node in **Cloud Explorer**, and choose **Attach Debugger**</span></span>
   
    ![Hibakereső csatlakoztatása][attachdebugger]
3. <span data-ttu-id="ef121-141">Az a **folyamat csatolása** párbeszédpanelen válassza ki a hibakeresési, és kattintson a kívánt folyamat **csatolása**</span><span class="sxs-lookup"><span data-stu-id="ef121-141">In the **Attach to process** dialog, choose the process you want to debug, and click **Attach**</span></span>
   
    ![Folyamat kiválasztása][chooseprocess]
   
    <span data-ttu-id="ef121-143">Szeretne csatlakozni, a folyamat neve megegyezik a szolgáltatási projekt szerelvény neve.</span><span class="sxs-lookup"><span data-stu-id="ef121-143">The name of the process you want to attach to, equals the name of your service project assembly name.</span></span>
   
    <span data-ttu-id="ef121-144">A hibakereső kapcsolódni fog, a folyamat futó összes csomópontján.</span><span class="sxs-lookup"><span data-stu-id="ef121-144">The debugger will attach to all nodes running the process.</span></span>
   
   * <span data-ttu-id="ef121-145">Abban az esetben, ha egy állapotmentes szolgáltatások hibakeresést az összes csomóponton a szolgáltatás minden példányának a hibakeresési munkamenetben részét képezik.</span><span class="sxs-lookup"><span data-stu-id="ef121-145">In the case where you are debugging a stateless service, all instances of the service on all nodes are part of the debug session.</span></span>
   * <span data-ttu-id="ef121-146">Állapotalapú szolgáltatási hibakeresést, ha minden olyan partíció csak az elsődleges másodpéldány lesz aktív, és ezért kifogott által a hibakereső.</span><span class="sxs-lookup"><span data-stu-id="ef121-146">If you are debugging a stateful service, only the primary replica of any partition will be active and therefore caught by the debugger.</span></span> <span data-ttu-id="ef121-147">Ha az elsődleges másodpéldány helyezi a hibakeresési munkamenetben, a replika feldolgozása továbbra is része lesz a hibakeresési munkamenetben.</span><span class="sxs-lookup"><span data-stu-id="ef121-147">If the primary replica moves during the debug session, the processing of that replica will still be part of the debug session.</span></span>
   * <span data-ttu-id="ef121-148">Ahhoz, hogy csak a megfelelő partíciók vagy egy megadott szolgáltatás példányának tényleges, a feltételes töréspontok segítségével csak törés az egy adott partícióra vagy a példány.</span><span class="sxs-lookup"><span data-stu-id="ef121-148">In order to only catch relevant partitions or instances of a given service, you can use conditional breakpoints to only break a specific partition or instance.</span></span>
     
     ![Feltételes töréspont][conditionalbreakpoint]
     
     > [!NOTE]
     > <span data-ttu-id="ef121-150">Jelenleg nem támogatjuk a szolgáltatás végrehajtható néven több példánya a Service Fabric-fürt hibakeresést.</span><span class="sxs-lookup"><span data-stu-id="ef121-150">Currently we do not support debugging a Service Fabric cluster with multiple instances of the same service executable name.</span></span>
     > 
     > 
4. <span data-ttu-id="ef121-151">Ha az alkalmazás hibakeresése végzett, kattintson a jobb gombbal a fürt a távoli hibakeresési bővítmény letiltása **Cloud Explorer** válassza **tiltsa le a hibakeresést.**</span><span class="sxs-lookup"><span data-stu-id="ef121-151">Once you finish debugging your application, you can disable the remote debugging extension by right-clicking the cluster in **Cloud Explorer** and choose **Disable Debugging**</span></span>
   
    ![Tiltsa le a távoli hibakeresés][disableremotedebugging]

## <a name="streaming-traces-from-a-remote-cluster-node"></a><span data-ttu-id="ef121-153">Adatfolyam-nyomkövetések távoli fürtcsomópontból</span><span class="sxs-lookup"><span data-stu-id="ef121-153">Streaming traces from a remote cluster node</span></span>
<span data-ttu-id="ef121-154">Akkor is alkalmasak adatfolyam nyomkövetések közvetlenül a Visual Studio egy távoli fürt csomópontja.</span><span class="sxs-lookup"><span data-stu-id="ef121-154">You are also able to stream traces directly from a remote cluster node to Visual Studio.</span></span> <span data-ttu-id="ef121-155">Ez a funkció lehetővé teszi az adatfolyam ETW nyomkövetési eseményeit, a Service Fabric fürtcsomóponton előállított.</span><span class="sxs-lookup"><span data-stu-id="ef121-155">This feature allows you to stream ETW trace events, produced on a Service Fabric cluster node.</span></span>

> [!NOTE]
> <span data-ttu-id="ef121-156">A szolgáltatás használatához [Service Fabric SDK 2.0](http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015) és [Azure SDK for .NET 2.9](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="ef121-156">This feature requires [Service Fabric SDK 2.0](http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015) and [Azure SDK for .NET 2.9](https://azure.microsoft.com/downloads/).</span></span>
> <span data-ttu-id="ef121-157">Ez a funkció csak az Azure-ban futó fürtök támogatja.</span><span class="sxs-lookup"><span data-stu-id="ef121-157">This feature only supports clusters running in Azure.</span></span>
> 
> 

<!-- -->
> [!WARNING]
> <span data-ttu-id="ef121-158">Adatfolyam-nyomkövetési szolgáltatásának fejlesztési/tesztelési és nem használható üzemi környezetben, mert a futó alkalmazások gyakorolt hatás célja.</span><span class="sxs-lookup"><span data-stu-id="ef121-158">Streaming traces is meant for dev/test scenarios and not to be used in production environments, because of the impact on the running applications.</span></span>
> <span data-ttu-id="ef121-159">A termelési forgatókönyvek hagyatkozzon kizárólag az Azure Diagnostics használatával eseménye továbbítását.</span><span class="sxs-lookup"><span data-stu-id="ef121-159">In a production scenario, you should rely on forwarding events using Azure Diagnostics.</span></span>
> 
> 

1. <span data-ttu-id="ef121-160">Keresse meg a fürt **Cloud Explorer**, kattintson a jobb gombbal, és válassza a **adatfolyam-nyomkövetés engedélyezése**</span><span class="sxs-lookup"><span data-stu-id="ef121-160">Navigate to your cluster in **Cloud Explorer**, right-click and choose **Enable Streaming Traces**</span></span>
   
    ![Távoli adatfolyam nyomkövetés engedélyezése][enablestreamingtraces]
   
    <span data-ttu-id="ef121-162">Ez a folyamat, ha engedélyezi a nyomkövetési adatokat adatfolyam a fürtcsomópontokat, valamint a szükséges hálózati konfigurációk kiterjesztés fog indítsa.</span><span class="sxs-lookup"><span data-stu-id="ef121-162">This will kick off the process of enabling the streaming traces extension on your cluster nodes, as well as required network configurations.</span></span>
2. <span data-ttu-id="ef121-163">Bontsa ki a **csomópontok** elemében **Cloud Explorer**, kattintson a jobb gombbal a csomópont adatfolyam-nyomkövetéseit, és válassza a kívánt **nézet adatfolyam-nyomkövetések**</span><span class="sxs-lookup"><span data-stu-id="ef121-163">Expand the **Nodes** element in **Cloud Explorer**, right-click the node you want to stream traces from and choose **View Streaming Traces**</span></span>
   
    ![Távoli adatfolyam-nyomkövetések megtekintése][viewremotestreamingtraces]
   
    <span data-ttu-id="ef121-165">Ismételje meg a 2. meg szeretné tekinteni a nyomkövetések csomópont.</span><span class="sxs-lookup"><span data-stu-id="ef121-165">Repeat step 2 for as many nodes as you want to see traces from.</span></span> <span data-ttu-id="ef121-166">Az egyes csomópontok adatfolyamokkal dedikált ablakban jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="ef121-166">Each nodes stream will show up in a dedicated window.</span></span>
   
    <span data-ttu-id="ef121-167">A Service Fabric és a szolgáltatások által kibocsátott nyomkövetések látni áll.</span><span class="sxs-lookup"><span data-stu-id="ef121-167">You are now able to see the traces emitted by Service Fabric, and your services.</span></span> <span data-ttu-id="ef121-168">Ha azt szeretné, csak az adott alkalmazást megjelenítendő események szűrésére, egyszerűen csak írja be a szűrő alkalmazása nevében.</span><span class="sxs-lookup"><span data-stu-id="ef121-168">If you want to filter the events to only show a specific application, simply type in the name of the application in the filter.</span></span>
   
    ![Adatfolyam-nyomkövetések megtekintése][viewingstreamingtraces]
3. <span data-ttu-id="ef121-170">Miután a folyamatos átviteli nyomkövetési adatokat a fürtről, bármikor letilthatja távoli adatfolyam nyomkövetési adatok, kattintson a jobb gombbal a fürtre **Cloud Explorer** válassza **adatfolyam-nyomkövetések letiltása**</span><span class="sxs-lookup"><span data-stu-id="ef121-170">Once you are done streaming traces from your cluster, you can disable remote streaming traces, by right-clicking the cluster in **Cloud Explorer** and choose **Disable Streaming Traces**</span></span>
   
    ![Tiltsa le a távoli adatfolyam nyomkövetések][disablestreamingtraces]

## <a name="next-steps"></a><span data-ttu-id="ef121-172">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ef121-172">Next steps</span></span>
* <span data-ttu-id="ef121-173">[A Service Fabric szolgáltatás tesztelése](service-fabric-testability-overview.md).</span><span class="sxs-lookup"><span data-stu-id="ef121-173">[Test a Service Fabric service](service-fabric-testability-overview.md).</span></span>
* <span data-ttu-id="ef121-174">[A Service Fabric-alkalmazások, a Visual Studio kezelése](service-fabric-manage-application-in-visual-studio.md).</span><span class="sxs-lookup"><span data-stu-id="ef121-174">[Manage your Service Fabric applications in Visual Studio](service-fabric-manage-application-in-visual-studio.md).</span></span>

<!--Image references-->
[startdebugging]: ./media/service-fabric-debugging-your-application/startdebugging.png
[diagnosticevents]: ./media/service-fabric-debugging-your-application/diagnosticevents.png
[viewdiagnosticevents]: ./media/service-fabric-debugging-your-application/viewdiagnosticevents.png
[diagnosticeventsactions]: ./media/service-fabric-debugging-your-application/diagnosticeventsactions.png
[breakpoint]: ./media/service-fabric-debugging-your-application/breakpoint.png
[enableremotedebugging]: ./media/service-fabric-debugging-your-application/enableremotedebugging.png
[attachdebugger]: ./media/service-fabric-debugging-your-application/attachdebugger.png
[chooseprocess]: ./media/service-fabric-debugging-your-application/chooseprocess.png
[conditionalbreakpoint]: ./media/service-fabric-debugging-your-application/conditionalbreakpoint.png
[disableremotedebugging]: ./media/service-fabric-debugging-your-application/disableremotedebugging.png
[enablestreamingtraces]: ./media/service-fabric-debugging-your-application/enablestreamingtraces.png
[viewingstreamingtraces]: ./media/service-fabric-debugging-your-application/viewingstreamingtraces.png
[viewremotestreamingtraces]: ./media/service-fabric-debugging-your-application/viewremotestreamingtraces.png
[disablestreamingtraces]: ./media/service-fabric-debugging-your-application/disablestreamingtraces.png
