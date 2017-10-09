---
title: "aaaDebug az alkalmazást a Visual Studio |} Microsoft Docs"
description: "Tovább fejlesztheti hello megbízhatóságának és teljesítményének a szolgáltatások fejlesztéséhez és a helyi fejlesztési fürtök hibakeresés a Visual Studio őket."
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
ms.openlocfilehash: 8d08689e82195d09f57b462631ad04fd237bc4fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="debug-your-service-fabric-application-by-using-visual-studio"></a><span data-ttu-id="68f7f-103">A Service Fabric-alkalmazás hibakeresése a Visual Studio használatával</span><span class="sxs-lookup"><span data-stu-id="68f7f-103">Debug your Service Fabric application by using Visual Studio</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="68f7f-104">A Visual Studio/CSharp</span><span class="sxs-lookup"><span data-stu-id="68f7f-104">Visual Studio/CSharp</span></span>](service-fabric-debugging-your-application.md) 
> * [<span data-ttu-id="68f7f-105">Eclipse/Java</span><span class="sxs-lookup"><span data-stu-id="68f7f-105">Eclipse/Java</span></span>](service-fabric-debugging-your-application-java.md)
>


## <a name="debug-a-local-service-fabric-application"></a><span data-ttu-id="68f7f-106">Egy helyi Service Fabric-alkalmazás hibakeresése</span><span class="sxs-lookup"><span data-stu-id="68f7f-106">Debug a local Service Fabric application</span></span>
<span data-ttu-id="68f7f-107">Időt és pénzt takaríthat telepítésével és az Azure Service Fabric-alkalmazás egy számítógép helyi fejlesztési fürtöt a hibakeresést.</span><span class="sxs-lookup"><span data-stu-id="68f7f-107">You can save time and money by deploying and debugging your Azure Service Fabric application in a local computer development cluster.</span></span> <span data-ttu-id="68f7f-108">Visual Studio 2017 vagy Visual Studio 2015 hello alkalmazás toohello helyi fürt központi telepítése, és automatikusan kapcsolódhatnak hello hibakereső tooall példányok az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="68f7f-108">Visual Studio 2017 or Visual Studio 2015 can deploy hello application toohello local cluster and automatically connect hello debugger tooall instances of your application.</span></span>

1. <span data-ttu-id="68f7f-109">Indítsa el a helyi fejlesztési fürtök hello utasításait követve [a Service Fabric fejlesztési környezet létrehozása](service-fabric-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="68f7f-109">Start a local development cluster by following hello steps in [Setting up your Service Fabric development environment](service-fabric-get-started.md).</span></span>
2. <span data-ttu-id="68f7f-110">Nyomja le az **F5** , vagy kattintson a **Debug** > **Start Debugging**.</span><span class="sxs-lookup"><span data-stu-id="68f7f-110">Press **F5** or click **Debug** > **Start Debugging**.</span></span>
   
    ![Egy alkalmazást a hibakeresés elindításához][startdebugging]
3. <span data-ttu-id="68f7f-112">Állítson be töréspontokat a kódot és hello alkalmazáson keresztül lépés hello parancsok kattintva **Debug** menü.</span><span class="sxs-lookup"><span data-stu-id="68f7f-112">Set breakpoints in your code and step through hello application by clicking commands in hello **Debug** menu.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="68f7f-113">A Visual Studio csatol az alkalmazás tooall példányát.</span><span class="sxs-lookup"><span data-stu-id="68f7f-113">Visual Studio attaches tooall instances of your application.</span></span> <span data-ttu-id="68f7f-114">Kód most lépjen, amíg töréspontok előfordulhat, hogy beolvasása kattintson az egyidejű munkamenetek eredményezve több folyamat.</span><span class="sxs-lookup"><span data-stu-id="68f7f-114">While you're stepping through code, breakpoints may get hit by multiple processes resulting in concurrent sessions.</span></span> <span data-ttu-id="68f7f-115">Próbálja hello töréspontok letiltása után azok még találat, azáltal, hogy minden töréspont hello Szálazonosító függővé vagy diagnosztikai eseményeket használ.</span><span class="sxs-lookup"><span data-stu-id="68f7f-115">Try disabling hello breakpoints after they're hit, by making each breakpoint conditional on hello thread ID or by using diagnostic events.</span></span>
   > 
   > 
4. <span data-ttu-id="68f7f-116">Hello **a diagnosztikai** automatikusan megnyílik, megtekintheti a diagnosztikai valós időben.</span><span class="sxs-lookup"><span data-stu-id="68f7f-116">hello **Diagnostic Events** window will automatically open so you can view diagnostic events in real time.</span></span>
   
    ![Valós idejű diagnosztikai eseményeinek megtekintése][diagnosticevents]
5. <span data-ttu-id="68f7f-118">Hello is megnyithatja **a diagnosztikai** Cloud Explorer ablakban.</span><span class="sxs-lookup"><span data-stu-id="68f7f-118">You can also open hello **Diagnostic Events** window in Cloud Explorer.</span></span>  <span data-ttu-id="68f7f-119">A **Service Fabric**, kattintson a jobb gombbal bármelyik csomópont, és válassza a **nézet adatfolyam-nyomkövetések**.</span><span class="sxs-lookup"><span data-stu-id="68f7f-119">Under **Service Fabric**, right-click any node and choose **View Streaming Traces**.</span></span>
   
    ![Nyissa meg hello diagnosztikai események ablak][viewdiagnosticevents]
   
    <span data-ttu-id="68f7f-121">Ha toofilter a nyomkövetések tooa adott szolgáltatással vagy alkalmazással, egyszerűen engedélyezze a folyamatos átviteli nyomkövetési adatokat az adott adott szolgáltatás vagy alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="68f7f-121">If you want toofilter your traces tooa specific service or application, simply enable streaming traces on that specific service or application.</span></span>
6. <span data-ttu-id="68f7f-122">hello diagnosztikai események automatikusan generált hello látható **ServiceEventSource.cs** fájlt, és az alkalmazás kódjában nevezzük.</span><span class="sxs-lookup"><span data-stu-id="68f7f-122">hello diagnostic events can be seen in hello automatically generated **ServiceEventSource.cs** file and are called from application code.</span></span>
   
    ```csharp
    ServiceEventSource.Current.ServiceMessage(this, "My ServiceMessage with a parameter {0}", result.Value.ToString());
    ```
7. <span data-ttu-id="68f7f-123">Hello **a diagnosztikai** ablak támogatja a szűrést, a felfüggesztés és a valós idejű események ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="68f7f-123">hello **Diagnostic Events** window supports filtering, pausing, and inspecting events in real time.</span></span>  <span data-ttu-id="68f7f-124">hello szűrő egy egyszerű karakterlánc-keresés hello eseményüzenet, beleértve annak tartalmát.</span><span class="sxs-lookup"><span data-stu-id="68f7f-124">hello filter is a simple string search of hello event message, including its contents.</span></span>
   
    ![Szűréséhez, szüneteltetése és folytatása és vizsgálja meg a valós idejű események][diagnosticeventsactions]
8. <span data-ttu-id="68f7f-126">Hibakereső szolgáltatásának olyan, mint bármely más alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="68f7f-126">Debugging services is like debugging any other application.</span></span> <span data-ttu-id="68f7f-127">Visual Studio alkalmazással töréspontok általában könnyen hibakeresési állítja be.</span><span class="sxs-lookup"><span data-stu-id="68f7f-127">You will normally set Breakpoints through Visual Studio for easy debugging.</span></span> <span data-ttu-id="68f7f-128">Annak ellenére, hogy a megbízható gyűjtemények replikálásához több csomópont, azok továbbra is valósítania az IEnumerable illesztőfelületet.</span><span class="sxs-lookup"><span data-stu-id="68f7f-128">Even though Reliable Collections replicate across multiple nodes, they still implement IEnumerable.</span></span> <span data-ttu-id="68f7f-129">Ez azt jelenti, hogy használható hello eredmények megtekintése a Visual Studio toosee hibakeresés során belül már tárolja.</span><span class="sxs-lookup"><span data-stu-id="68f7f-129">This means that you can use hello Results View in Visual Studio while debugging toosee what you've stored inside.</span></span> <span data-ttu-id="68f7f-130">Egyszerűen állítson be egy töréspontot bárhol a kódban.</span><span class="sxs-lookup"><span data-stu-id="68f7f-130">Simply set a breakpoint anywhere in your code.</span></span>
   
    ![Egy alkalmazást a hibakeresés elindításához][breakpoint]

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->

## <a name="debug-a-remote-service-fabric-application"></a><span data-ttu-id="68f7f-132">A távoli Service Fabric-alkalmazás hibakeresése</span><span class="sxs-lookup"><span data-stu-id="68f7f-132">Debug a remote Service Fabric application</span></span>
<span data-ttu-id="68f7f-133">Ha a Service Fabric-alkalmazások az Azure Service Fabric-fürt futtatja, is tooremotely hibakeresése a Visual Studio-ről.</span><span class="sxs-lookup"><span data-stu-id="68f7f-133">If your Service Fabric applications are running on a Service Fabric cluster in Azure, you are able tooremotely debug these, directly from Visual Studio.</span></span>

> [!NOTE]
> <span data-ttu-id="68f7f-134">hello használatához [Service Fabric SDK 2.0](http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015) és [Azure SDK for .NET 2.9](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="68f7f-134">hello feature requires [Service Fabric SDK 2.0](http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015) and [Azure SDK for .NET 2.9](https://azure.microsoft.com/downloads/).</span></span>    
> 
> 

<!-- -->
> [!WARNING]
> <span data-ttu-id="68f7f-135">Távoli hibakeresés szolgáltatásának fejlesztési/tesztelési és nem használja termelési környezetben futó alkalmazások hello hello gyakorolt miatt toobe tervezték.</span><span class="sxs-lookup"><span data-stu-id="68f7f-135">Remote debugging is meant for dev/test scenarios and not toobe used in production environments, because of hello impact on hello running applications.</span></span>
> 
> 

1. <span data-ttu-id="68f7f-136">Keresse meg a fürt tooyour **Cloud Explorer**, kattintson a jobb gombbal, és válassza a **hibakeresés engedélyezése**</span><span class="sxs-lookup"><span data-stu-id="68f7f-136">Navigate tooyour cluster in **Cloud Explorer**, right-click and choose **Enable Debugging**</span></span>
   
    ![Távoli hibakeresésének engedélyezése][enableremotedebugging]
   
    <span data-ttu-id="68f7f-138">Ez fogja, hogy a távoli hibakeresés a fürtcsomópontokon bővítmény hello hello folyamat indítsa, valamint hálózati konfiguráció szükséges.</span><span class="sxs-lookup"><span data-stu-id="68f7f-138">This will kick off hello process of enabling hello remote debugging extension on your cluster nodes, as well as required network configurations.</span></span>
2. <span data-ttu-id="68f7f-139">Kattintson a jobb gombbal hello lévő fürtcsomópont **Cloud Explorer**, és válassza ki **csatolása hibakereső**</span><span class="sxs-lookup"><span data-stu-id="68f7f-139">Right-click hello cluster node in **Cloud Explorer**, and choose **Attach Debugger**</span></span>
   
    ![Hibakereső csatlakoztatása][attachdebugger]
3. <span data-ttu-id="68f7f-141">A hello **tooprocess csatolása** párbeszédpanelen válassza ki, szeretné, hogy toodebug, majd kattintson az hello folyamatot **csatolása**</span><span class="sxs-lookup"><span data-stu-id="68f7f-141">In hello **Attach tooprocess** dialog, choose hello process you want toodebug, and click **Attach**</span></span>
   
    ![Folyamat kiválasztása][chooseprocess]
   
    <span data-ttu-id="68f7f-143">hello neve hello folyamat azt szeretné, hogy tooattach, egyenlő hello a szolgáltatási projekt szerelvény neve alapján.</span><span class="sxs-lookup"><span data-stu-id="68f7f-143">hello name of hello process you want tooattach to, equals hello name of your service project assembly name.</span></span>
   
    <span data-ttu-id="68f7f-144">hello hibakereső hello folyamatának futtatása tooall csomópontok kapcsolódni fog.</span><span class="sxs-lookup"><span data-stu-id="68f7f-144">hello debugger will attach tooall nodes running hello process.</span></span>
   
   * <span data-ttu-id="68f7f-145">Ahol állapotmentes szolgáltatások hibakeresést hello esetben az összes olyan csomóponton hello szolgáltatás minden példányának részei hello hibakeresési munkamenetben.</span><span class="sxs-lookup"><span data-stu-id="68f7f-145">In hello case where you are debugging a stateless service, all instances of hello service on all nodes are part of hello debug session.</span></span>
   * <span data-ttu-id="68f7f-146">Állapotalapú szolgáltatási hibakeresést, ha csak hello elsődleges másodpéldányát minden olyan partíció lesz aktív, és ezért kifogott hello hibakereső által.</span><span class="sxs-lookup"><span data-stu-id="68f7f-146">If you are debugging a stateful service, only hello primary replica of any partition will be active and therefore caught by hello debugger.</span></span> <span data-ttu-id="68f7f-147">Hello elsődleges replika hello hibakeresési munkamenetben helyezi át, ha adott másodpéldány hello feldolgozását is hello hibakeresési munkamenet része.</span><span class="sxs-lookup"><span data-stu-id="68f7f-147">If hello primary replica moves during hello debug session, hello processing of that replica will still be part of hello debug session.</span></span>
   * <span data-ttu-id="68f7f-148">Rendelés tooonly catch vonatkozó partíciók vagy egy megadott szolgáltatás példányának használhatja a feltételes töréspontok tooonly break egy adott partícióra vagy a példány.</span><span class="sxs-lookup"><span data-stu-id="68f7f-148">In order tooonly catch relevant partitions or instances of a given service, you can use conditional breakpoints tooonly break a specific partition or instance.</span></span>
     
     ![Feltételes töréspont][conditionalbreakpoint]
     
     > [!NOTE]
     > <span data-ttu-id="68f7f-150">Jelenleg nem támogatjuk hibakeresés hello több példánya a Service Fabric-fürt azonos szolgáltatás végrehajtható fájljának nevét.</span><span class="sxs-lookup"><span data-stu-id="68f7f-150">Currently we do not support debugging a Service Fabric cluster with multiple instances of hello same service executable name.</span></span>
     > 
     > 
4. <span data-ttu-id="68f7f-151">Ha az alkalmazás hibakeresése végzett, kattintson a jobb gombbal a fürt hello hello távoli hibakeresési bővítmény letiltása **Cloud Explorer** válassza **tiltsa le a hibakeresést.**</span><span class="sxs-lookup"><span data-stu-id="68f7f-151">Once you finish debugging your application, you can disable hello remote debugging extension by right-clicking hello cluster in **Cloud Explorer** and choose **Disable Debugging**</span></span>
   
    ![Tiltsa le a távoli hibakeresés][disableremotedebugging]

## <a name="streaming-traces-from-a-remote-cluster-node"></a><span data-ttu-id="68f7f-153">Adatfolyam-nyomkövetések távoli fürtcsomópontból</span><span class="sxs-lookup"><span data-stu-id="68f7f-153">Streaming traces from a remote cluster node</span></span>
<span data-ttu-id="68f7f-154">Hogy egyaránt közvetlenül egy távoli fürt csomópont tooVisual Studio képes toostream nyomkövetéseit.</span><span class="sxs-lookup"><span data-stu-id="68f7f-154">You are also able toostream traces directly from a remote cluster node tooVisual Studio.</span></span> <span data-ttu-id="68f7f-155">Ez a funkció lehetővé teszi toostream ETW nyomkövetési eseményeit, a Service Fabric fürtcsomóponton hozott létre.</span><span class="sxs-lookup"><span data-stu-id="68f7f-155">This feature allows you toostream ETW trace events, produced on a Service Fabric cluster node.</span></span>

> [!NOTE]
> <span data-ttu-id="68f7f-156">A szolgáltatás használatához [Service Fabric SDK 2.0](http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015) és [Azure SDK for .NET 2.9](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="68f7f-156">This feature requires [Service Fabric SDK 2.0](http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015) and [Azure SDK for .NET 2.9](https://azure.microsoft.com/downloads/).</span></span>
> <span data-ttu-id="68f7f-157">Ez a funkció csak az Azure-ban futó fürtök támogatja.</span><span class="sxs-lookup"><span data-stu-id="68f7f-157">This feature only supports clusters running in Azure.</span></span>
> 
> 

<!-- -->
> [!WARNING]
> <span data-ttu-id="68f7f-158">Adatfolyam-nyomkövetési szolgáltatásának fejlesztési/tesztelési és nem használja termelési környezetben futó alkalmazások hello hello gyakorolt miatt toobe tervezték.</span><span class="sxs-lookup"><span data-stu-id="68f7f-158">Streaming traces is meant for dev/test scenarios and not toobe used in production environments, because of hello impact on hello running applications.</span></span>
> <span data-ttu-id="68f7f-159">A termelési forgatókönyvek hagyatkozzon kizárólag az Azure Diagnostics használatával eseménye továbbítását.</span><span class="sxs-lookup"><span data-stu-id="68f7f-159">In a production scenario, you should rely on forwarding events using Azure Diagnostics.</span></span>
> 
> 

1. <span data-ttu-id="68f7f-160">Keresse meg a fürt tooyour **Cloud Explorer**, kattintson a jobb gombbal, és válassza a **adatfolyam-nyomkövetés engedélyezése**</span><span class="sxs-lookup"><span data-stu-id="68f7f-160">Navigate tooyour cluster in **Cloud Explorer**, right-click and choose **Enable Streaming Traces**</span></span>
   
    ![Távoli adatfolyam nyomkövetés engedélyezése][enablestreamingtraces]
   
    <span data-ttu-id="68f7f-162">Ez lesz, ha engedélyezi a nyomkövetési adatokat bővítmény a fürtcsomópontokon streaming hello hello folyamat indítsa, valamint hálózati konfiguráció szükséges.</span><span class="sxs-lookup"><span data-stu-id="68f7f-162">This will kick off hello process of enabling hello streaming traces extension on your cluster nodes, as well as required network configurations.</span></span>
2. <span data-ttu-id="68f7f-163">Bontsa ki a hello **csomópontok** elemében **Cloud Explorer**, kattintson a jobb gombbal hello csomópont szeretné, hogy toostream nyomkövetéseit, és válassza a **nézet adatfolyam-nyomkövetések**</span><span class="sxs-lookup"><span data-stu-id="68f7f-163">Expand hello **Nodes** element in **Cloud Explorer**, right-click hello node you want toostream traces from and choose **View Streaming Traces**</span></span>
   
    ![Távoli adatfolyam-nyomkövetések megtekintése][viewremotestreamingtraces]
   
    <span data-ttu-id="68f7f-165">Azt szeretné, hogy toosee nyomkövetéseit csomópont ismételje meg a 2.</span><span class="sxs-lookup"><span data-stu-id="68f7f-165">Repeat step 2 for as many nodes as you want toosee traces from.</span></span> <span data-ttu-id="68f7f-166">Az egyes csomópontok adatfolyamokkal dedikált ablakban jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="68f7f-166">Each nodes stream will show up in a dedicated window.</span></span>
   
    <span data-ttu-id="68f7f-167">A Service Fabric és a szolgáltatások által kibocsátott képes toosee hello nyomkövetések is.</span><span class="sxs-lookup"><span data-stu-id="68f7f-167">You are now able toosee hello traces emitted by Service Fabric, and your services.</span></span> <span data-ttu-id="68f7f-168">Ha azt szeretné, toofilter hello események tooonly megjelenítése egy adott alkalmazáshoz, egyszerűen az alkalmazástípus hello nevében hello hello szűrőben.</span><span class="sxs-lookup"><span data-stu-id="68f7f-168">If you want toofilter hello events tooonly show a specific application, simply type in hello name of hello application in hello filter.</span></span>
   
    ![Adatfolyam-nyomkövetések megtekintése][viewingstreamingtraces]
3. <span data-ttu-id="68f7f-170">Miután a folyamatos átviteli nyomkövetési adatokat a fürtről, bármikor letilthatja távoli adatfolyam nyomkövetési adatok, kattintson a jobb gombbal a fürt hello **Cloud Explorer** válassza **adatfolyam-nyomkövetések letiltása**</span><span class="sxs-lookup"><span data-stu-id="68f7f-170">Once you are done streaming traces from your cluster, you can disable remote streaming traces, by right-clicking hello cluster in **Cloud Explorer** and choose **Disable Streaming Traces**</span></span>
   
    ![Tiltsa le a távoli adatfolyam nyomkövetések][disablestreamingtraces]

## <a name="next-steps"></a><span data-ttu-id="68f7f-172">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="68f7f-172">Next steps</span></span>
* <span data-ttu-id="68f7f-173">[A Service Fabric szolgáltatás tesztelése](service-fabric-testability-overview.md).</span><span class="sxs-lookup"><span data-stu-id="68f7f-173">[Test a Service Fabric service](service-fabric-testability-overview.md).</span></span>
* <span data-ttu-id="68f7f-174">[A Service Fabric-alkalmazások, a Visual Studio kezelése](service-fabric-manage-application-in-visual-studio.md).</span><span class="sxs-lookup"><span data-stu-id="68f7f-174">[Manage your Service Fabric applications in Visual Studio](service-fabric-manage-application-in-visual-studio.md).</span></span>

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
