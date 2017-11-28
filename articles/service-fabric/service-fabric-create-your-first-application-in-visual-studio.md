---
title: "egy megbízható Azure Service Fabric-szolgáltatás, a C# aaaCreate"
description: "Azure Service Fabric-alapú Reliable Services-alkalmazás létrehozása, üzembe helyezése és hibakeresése a Visual Studio használatával."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: vturecek
ms.assetid: c3655b7b-de78-4eac-99eb-012f8e042109
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/28/2017
ms.author: ryanwi
ms.openlocfilehash: 740c866da6e639219b529fe92ed63cbeaa702a35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-c-service-fabric-stateful-reliable-services-application"></a><span data-ttu-id="42ed0-103">Az első Service Fabric Stateful Reliable Services-alkalmazás létrehozása C#-környezettel</span><span class="sxs-lookup"><span data-stu-id="42ed0-103">Create your first C# Service Fabric stateful reliable services application</span></span>

<span data-ttu-id="42ed0-104">Megtudhatja, hogyan toodeploy az első Service Fabric-alkalmazás a .NET-hez a Windows néhány perc múlva.</span><span class="sxs-lookup"><span data-stu-id="42ed0-104">Learn how toodeploy your first Service Fabric application for .NET on Windows in just a few minutes.</span></span> <span data-ttu-id="42ed0-105">Ha elkészült, rendelkezni fog egy Reliable Services-alkalmazással futó helyi fürttel.</span><span class="sxs-lookup"><span data-stu-id="42ed0-105">When you're finished, you'll have a local cluster running with a reliable service application.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="42ed0-106">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="42ed0-106">Prerequisites</span></span>

<span data-ttu-id="42ed0-107">Mielőtt elkezdené, győződjön meg arról, hogy [beállította a fejlesztőkörnyezetet](service-fabric-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="42ed0-107">Before you get started, make sure that you have [set up your development environment](service-fabric-get-started.md).</span></span> <span data-ttu-id="42ed0-108">Ez magában foglalja a Service Fabric SDK hello és a Visual Studio 2017 vagy 2015 telepítése.</span><span class="sxs-lookup"><span data-stu-id="42ed0-108">This includes installing hello Service Fabric SDK and Visual Studio 2017 or 2015.</span></span>

## <a name="create-hello-application"></a><span data-ttu-id="42ed0-109">Hello alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="42ed0-109">Create hello application</span></span>

<span data-ttu-id="42ed0-110">Indítsa el a Visual Studiót **rendszergazdaként**.</span><span class="sxs-lookup"><span data-stu-id="42ed0-110">Launch Visual Studio as an **administrator**.</span></span>

<span data-ttu-id="42ed0-111">Projekt létrehozása `CTRL`+`SHIFT`+`N` használatával</span><span class="sxs-lookup"><span data-stu-id="42ed0-111">Create a project with `CTRL`+`SHIFT`+`N`</span></span>

<span data-ttu-id="42ed0-112">A hello **új projekt** párbeszédpanelen válasszon **felhő > Service Fabric-alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="42ed0-112">In hello **New Project** dialog, choose **Cloud > Service Fabric Application**.</span></span>

<span data-ttu-id="42ed0-113">Hello alkalmazás neve **MyApplication** nyomja le az ENTER **OK**.</span><span class="sxs-lookup"><span data-stu-id="42ed0-113">Name hello application **MyApplication** and press **OK**.</span></span>

   
![A Visual Studio Új projekt párbeszédpanelje][1]

<span data-ttu-id="42ed0-115">Service Fabric-alkalmazás bármilyen típusú hello tovább párbeszédpanelről hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="42ed0-115">You can create any type of Service Fabric application from hello next dialog.</span></span> <span data-ttu-id="42ed0-116">Ebben a rövid útmutatóban válassza az **Állapotalapú szolgáltatás** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="42ed0-116">For this Quickstart, choose **Stateful Service**.</span></span>

<span data-ttu-id="42ed0-117">Hello szolgáltatás **MyStatefulService** nyomja le az ENTER **OK**.</span><span class="sxs-lookup"><span data-stu-id="42ed0-117">Name hello service **MyStatefulService** and press **OK**.</span></span>

![A Visual Studio Új szolgáltatás párbeszédpanelje][2]


<span data-ttu-id="42ed0-119">A Visual Studio létrehozza hello projektet és hello állapotalapú szolgáltatási projektet, és megjeleníti őket a Megoldáskezelőben.</span><span class="sxs-lookup"><span data-stu-id="42ed0-119">Visual Studio creates hello application project and hello stateful service project and displays them in Solution Explorer.</span></span>

![A Megoldáskezelő folytatja az alkalmazás létrehozását állapotalapú szolgáltatással][3]

<span data-ttu-id="42ed0-121">hello projektet (**MyApplication**) nem tartalmaz közvetlenül semmilyen kódot.</span><span class="sxs-lookup"><span data-stu-id="42ed0-121">hello application project (**MyApplication**) does not contain any code directly.</span></span> <span data-ttu-id="42ed0-122">Helyette számos szolgáltatási projektre hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="42ed0-122">Instead, it references a set of service projects.</span></span> <span data-ttu-id="42ed0-123">Ezenfelül három egyéb típusú tartalmat is tartalmaz:</span><span class="sxs-lookup"><span data-stu-id="42ed0-123">In addition, it contains three other types of content:</span></span>

* <span data-ttu-id="42ed0-124">**Profilok közzététele**</span><span class="sxs-lookup"><span data-stu-id="42ed0-124">**Publish profiles**</span></span>  
<span data-ttu-id="42ed0-125">Profilok üzembe helyezéséhez toodifferent környezetekben.</span><span class="sxs-lookup"><span data-stu-id="42ed0-125">Profiles for deploying toodifferent environments.</span></span>

* <span data-ttu-id="42ed0-126">**Szkriptek**</span><span class="sxs-lookup"><span data-stu-id="42ed0-126">**Scripts**</span></span>  
<span data-ttu-id="42ed0-127">Az alkalmazás üzembe helyezéséhez/frissítéséhez szükséges PowerShell-szkript.</span><span class="sxs-lookup"><span data-stu-id="42ed0-127">PowerShell script for deploying/upgrading your application.</span></span>

* <span data-ttu-id="42ed0-128">**Alkalmazásdefiníció**</span><span class="sxs-lookup"><span data-stu-id="42ed0-128">**Application definition**</span></span>  
<span data-ttu-id="42ed0-129">Hello ApplicationManifest.xml fájlt tartalmaz *ApplicationPackageRoot* amely ismerteti, hogy az alkalmazás az összeállításban.</span><span class="sxs-lookup"><span data-stu-id="42ed0-129">Includes hello ApplicationManifest.xml file under *ApplicationPackageRoot* which describes your application's composition.</span></span> <span data-ttu-id="42ed0-130">Kapcsolódó alkalmazásparaméter-fájlokat a rendszer *ApplicationParameters*, amely lehet használt toospecify környezet-specifikus paramétereket.</span><span class="sxs-lookup"><span data-stu-id="42ed0-130">Associated application parameter files are under *ApplicationParameters*, which can be used toospecify environment-specific parameters.</span></span> <span data-ttu-id="42ed0-131">Visual Studio választja ki, egy alkalmazás paraméterfájl hello megadott tartozó közzétételi profil központi telepítési tooa adott környezet során.</span><span class="sxs-lookup"><span data-stu-id="42ed0-131">Visual Studio selects an application parameter file that's specified in hello associated publish profile during deployment tooa specific environment.</span></span>
    
<span data-ttu-id="42ed0-132">Hello hello szolgáltatási projekt tartalmának áttekintéséhez lásd: [Bevezetés a Reliable Services használatába](service-fabric-reliable-services-quick-start.md).</span><span class="sxs-lookup"><span data-stu-id="42ed0-132">For an overview of hello contents of hello service project, see [Getting started with Reliable Services](service-fabric-reliable-services-quick-start.md).</span></span>

## <a name="deploy-and-debug-hello-application"></a><span data-ttu-id="42ed0-133">Üzembe helyezése és hibakeresése hello alkalmazás</span><span class="sxs-lookup"><span data-stu-id="42ed0-133">Deploy and debug hello application</span></span>

<span data-ttu-id="42ed0-134">Most, hogy már van egy alkalmazása, futtassa.</span><span class="sxs-lookup"><span data-stu-id="42ed0-134">Now that you have an application, run it.</span></span>

<span data-ttu-id="42ed0-135">A Visual Studio, nyomja le a `F5` toodeploy hello alkalmazást a hibakereséshez.</span><span class="sxs-lookup"><span data-stu-id="42ed0-135">In Visual Studio, press `F5` toodeploy hello application for debugging.</span></span>

>[!NOTE]
><span data-ttu-id="42ed0-136">hello első alkalommal futtatja, és hello-alkalmazás központi telepítése helyileg, a Visual Studio hibakeresési egy helyi fürtöt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="42ed0-136">hello first time you run and deploy hello application locally, Visual Studio creates a local cluster for debugging.</span></span> <span data-ttu-id="42ed0-137">Ez eltarthat egy ideig.</span><span class="sxs-lookup"><span data-stu-id="42ed0-137">This may take some time.</span></span> <span data-ttu-id="42ed0-138">hello fürt létrehozási állapota hello Visual Studio kimeneti ablakában jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="42ed0-138">hello cluster creation status is displayed in hello Visual Studio output window.</span></span>

<span data-ttu-id="42ed0-139">Amikor készen áll a fürt hello, hello helyi fürt rendszer manager alkalmazást hello SDK részét képező értesítést kap.</span><span class="sxs-lookup"><span data-stu-id="42ed0-139">When hello cluster is ready, you get a notification from hello local cluster system tray manager application included with hello SDK.</span></span>
   
![A helyifürt-rendszertálca értesítése][4]

<span data-ttu-id="42ed0-141">Egyszer hello alkalmazás elindul, a Visual Studio automatikusan megjeleníti hello **diagnosztikai eseménynapló**, ahol megtekintheti a nyomkövetési kimeneti a szolgáltatásokból.</span><span class="sxs-lookup"><span data-stu-id="42ed0-141">Once hello application starts, Visual Studio automatically brings up hello **Diagnostics Event Viewer**, where you can see trace output from your services.</span></span>
   
![Diagnosztikai eseménynapló][5]

<span data-ttu-id="42ed0-143">hello használtuk állapotalapú Szolgáltatássablon egyszerűen jeleníti meg a számláló értéke növekvő a hello `RunAsync` metódusában **MyStatefulService.cs**.</span><span class="sxs-lookup"><span data-stu-id="42ed0-143">hello stateful service template we used simply shows a counter value incrementing in hello `RunAsync` method of **MyStatefulService.cs**.</span></span>

<span data-ttu-id="42ed0-144">Bontsa ki az egyik hello események toosee további adatait, többek között a hello kódot futtató hello csomópont.</span><span class="sxs-lookup"><span data-stu-id="42ed0-144">Expand one of hello events toosee more details, including hello node where hello code is running.</span></span> <span data-ttu-id="42ed0-145">Ebben az esetben ez a \_Node\_2, de az Ön számítógépén ez eltérő lehet.</span><span class="sxs-lookup"><span data-stu-id="42ed0-145">In this case, it is \_Node\_2, though it may differ on your machine.</span></span>
   
![A diagnosztikai eseménynapló részletei][6]

<span data-ttu-id="42ed0-147">hello helyi fürt egyetlen számítógépen lévő öt csomópontot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="42ed0-147">hello local cluster contains five nodes hosted on a single machine.</span></span> <span data-ttu-id="42ed0-148">Éles környezetben minden egyes csomópont más fizikai vagy virtuális gépen üzemel.</span><span class="sxs-lookup"><span data-stu-id="42ed0-148">In a production environment, each node is hosted on a distinct physical or virtual machine.</span></span> <span data-ttu-id="42ed0-149">ugyanaz a hello hibakereső toosimulate hello egy gép elvesztését, miközben gyakorló hello Visual Studio idő, vegyük le hello helyi fürtön lévő hello csomópontok egyikét.</span><span class="sxs-lookup"><span data-stu-id="42ed0-149">toosimulate hello loss of a machine while exercising hello Visual Studio debugger at hello same time, let's take down one of hello nodes on hello local cluster.</span></span>

<span data-ttu-id="42ed0-150">A hello **Megoldáskezelőben** ablakban megnyitott **MyStatefulService.cs**.</span><span class="sxs-lookup"><span data-stu-id="42ed0-150">In hello **Solution Explorer** window, open **MyStatefulService.cs**.</span></span> 

<span data-ttu-id="42ed0-151">Hello található `RunAsync` metódus, és állítsa be töréspont hello hello metódus első sorában.</span><span class="sxs-lookup"><span data-stu-id="42ed0-151">Find hello `RunAsync` method and set a breakpoint on hello first line of hello method.</span></span>

![Töréspont az állapotalapú szolgáltatás RunAsync metódusában][7]

<span data-ttu-id="42ed0-153">Indítsa el a hello **Service Fabric Explorer** kattintson a jobb gombbal a hello eszköz **Local Cluster Manager** rendszer alkalmazást, és válassza a **helyi fürt kezelése**.</span><span class="sxs-lookup"><span data-stu-id="42ed0-153">Launch hello **Service Fabric Explorer** tool by right-clicking on hello **Local Cluster Manager** system tray application and choose **Manage Local Cluster**.</span></span>

![Service Fabric Explorer elindításához a Local Cluster Manager hello][systray-launch-sfx]

<span data-ttu-id="42ed0-155">A [**Service Fabric Explorer**](service-fabric-visualizing-your-cluster.md) vizuálisan is megjeleníti a fürtöket,</span><span class="sxs-lookup"><span data-stu-id="42ed0-155">[**Service Fabric Explorer**](service-fabric-visualizing-your-cluster.md) offers a visual representation of a cluster.</span></span> <span data-ttu-id="42ed0-156">Ez magában foglalja a központilag telepített alkalmazások tooit hello készlete és az azt alkotó fizikai csomópontok készletét hello.</span><span class="sxs-lookup"><span data-stu-id="42ed0-156">It includes hello set of applications deployed tooit and hello set of physical nodes that make it up.</span></span>

<span data-ttu-id="42ed0-157">Hello bal oldali ablaktáblán bontsa ki a **fürt > csomópontok** és a kódot futtató keresés hello csomópont.</span><span class="sxs-lookup"><span data-stu-id="42ed0-157">In hello left pane, expand **Cluster > Nodes** and find hello node where your code is running.</span></span>

<span data-ttu-id="42ed0-158">Kattintson a **műveletek > inaktiválás (újraindítás)** toosimulate a számítógép-újraindítás.</span><span class="sxs-lookup"><span data-stu-id="42ed0-158">Click **Actions > Deactivate (Restart)** toosimulate a machine restarting.</span></span>

![Csomópont leállítása a Service Fabric Explorerben][sfx-stop-node]

<span data-ttu-id="42ed0-160">Pillanatnyilag a töréspont megjelenését megjelennie elérte a Visual Studio, a hello korábban végzett számítása az egyik csomópont zökkenőmentesen átadja a feladatokat tooanother.</span><span class="sxs-lookup"><span data-stu-id="42ed0-160">Momentarily, you should see your breakpoint hit in Visual Studio as hello computation you were doing on one node seamlessly fails over tooanother.</span></span>


<span data-ttu-id="42ed0-161">Ezután térjen vissza a diagnosztikai eseménynaplóhoz toohello, és tekintse meg az üdvözlő üzenetek.</span><span class="sxs-lookup"><span data-stu-id="42ed0-161">Next, return toohello Diagnostic Events Viewer and observe hello messages.</span></span> <span data-ttu-id="42ed0-162">hello számláló értéke továbbra is növekedett, annak ellenére, hogy hello események valójában egy másik csomópont származik.</span><span class="sxs-lookup"><span data-stu-id="42ed0-162">hello counter has continued incrementing, even though hello events are actually coming from a different node.</span></span>

![A diagnosztikai eseménynapló a feladatátvétel után][diagnostic-events-viewer-detail-post-failover]

## <a name="cleaning-up-hello-local-cluster-optional"></a><span data-ttu-id="42ed0-164">Törölje a helyi fürt hello (nem kötelező)</span><span class="sxs-lookup"><span data-stu-id="42ed0-164">Cleaning up hello local cluster (optional)</span></span>

<span data-ttu-id="42ed0-165">Ne feledje, hogy ez a helyi fürt valós.</span><span class="sxs-lookup"><span data-stu-id="42ed0-165">Remember, this local cluster is real.</span></span> <span data-ttu-id="42ed0-166">Hello hibakereső leállítása eltávolítja az alkalmazáspéldány és hello alkalmazástípus regisztrációjának törlése.</span><span class="sxs-lookup"><span data-stu-id="42ed0-166">Stopping hello debugger removes your application instance and unregisters hello application type.</span></span> <span data-ttu-id="42ed0-167">Azonban hello fürt toorun hello háttérben folytatódik.</span><span class="sxs-lookup"><span data-stu-id="42ed0-167">However, hello cluster continues toorun in hello background.</span></span> <span data-ttu-id="42ed0-168">Amikor készen áll a toostop hello helyi fürthöz, van néhány lehetőség áll.</span><span class="sxs-lookup"><span data-stu-id="42ed0-168">When you're ready toostop hello local cluster, there are a couple options.</span></span>

### <a name="keep-application-and-trace-data"></a><span data-ttu-id="42ed0-169">Alkalmazás- és nyomkövetési adatok megtartása</span><span class="sxs-lookup"><span data-stu-id="42ed0-169">Keep application and trace data</span></span>

<span data-ttu-id="42ed0-170">Kattintson a jobb gombbal a hello hello fürt leállítása **Local Cluster Manager** rendszer alkalmazást majd **helyi fürt leállítása**.</span><span class="sxs-lookup"><span data-stu-id="42ed0-170">Shut down hello cluster by right-clicking on hello **Local Cluster Manager** system tray application and then choose **Stop Local Cluster**.</span></span>

### <a name="delete-hello-cluster-and-all-data"></a><span data-ttu-id="42ed0-171">Hello fürt és az összes adat törlése</span><span class="sxs-lookup"><span data-stu-id="42ed0-171">Delete hello cluster and all data</span></span>

<span data-ttu-id="42ed0-172">Kattintson a jobb gombbal a hello hello fürt eltávolítása **Local Cluster Manager** rendszer alkalmazást majd **helyi fürt eltávolítása**.</span><span class="sxs-lookup"><span data-stu-id="42ed0-172">Remove hello cluster by right-clicking on hello **Local Cluster Manager** system tray application and then choose **Remove Local Cluster**.</span></span> 

<span data-ttu-id="42ed0-173">Ha ezt a lehetőséget választja, a Visual Studio fog telepíteni hello fürt hello a Futtatás hello alkalmazás következő indításakor.</span><span class="sxs-lookup"><span data-stu-id="42ed0-173">If you choose this option, Visual Studio will redeploy hello cluster hello next time your run hello application.</span></span> <span data-ttu-id="42ed0-174">Válassza ezt a beállítást, ha egy kis ideig nem szándékozik a toouse hello helyi fürtöt, vagy ha tooreclaim erőforrásokat kell.</span><span class="sxs-lookup"><span data-stu-id="42ed0-174">Choose this option if you don't intend toouse hello local cluster for some time or if you need tooreclaim resources.</span></span>

## <a name="next-steps"></a><span data-ttu-id="42ed0-175">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="42ed0-175">Next steps</span></span>
<span data-ttu-id="42ed0-176">További információk a [Reliable Services](service-fabric-reliable-services-introduction.md)-szolgáltatásokról.</span><span class="sxs-lookup"><span data-stu-id="42ed0-176">Read more about [reliable services](service-fabric-reliable-services-introduction.md).</span></span>
<!-- Image References -->

[1]: ./media/service-fabric-create-your-first-application-in-visual-studio/new-project-dialog.png
[2]: ./media/service-fabric-create-your-first-application-in-visual-studio/new-project-dialog-2.png
[3]: ./media/service-fabric-create-your-first-application-in-visual-studio/solution-explorer-stateful-service-template.png
[4]: ./media/service-fabric-create-your-first-application-in-visual-studio/local-cluster-manager-notification.png
[5]: ./media/service-fabric-create-your-first-application-in-visual-studio/diagnostic-events-viewer.png
[6]: ./media/service-fabric-create-your-first-application-in-visual-studio/diagnostic-events-viewer-detail.png
[7]: ./media/service-fabric-create-your-first-application-in-visual-studio/runasync-breakpoint.png
[sfx-stop-node]: ./media/service-fabric-create-your-first-application-in-visual-studio/sfe-deactivate-node.png
[systray-launch-sfx]: ./media/service-fabric-create-your-first-application-in-visual-studio/launch-sfx.png
[diagnostic-events-viewer-detail-post-failover]: ./media/service-fabric-create-your-first-application-in-visual-studio/diagnostic-events-viewer-detail-post-failover.png
[sfe-delete-application]: ./media/service-fabric-create-your-first-application-in-visual-studio/sfe-delete-application.png
[switch-cluster-mode]: ./media/service-fabric-create-your-first-application-in-visual-studio/switch-cluster-mode.png
[cluster-setup-success-1-node]: ./media/service-fabric-get-started-with-a-local-cluster/cluster-setup-success-1-node.png
