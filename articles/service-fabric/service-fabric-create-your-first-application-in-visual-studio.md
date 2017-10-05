---
title: "Azure Service Fabric Reliable Service-szolgáltatás létrehozása C#-környezettel"
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
ms.openlocfilehash: f93298e6483fd8c9dfda835964aeebd1a430af69
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="create-your-first-c-service-fabric-stateful-reliable-services-application"></a><span data-ttu-id="1cc7d-103">Az első Service Fabric Stateful Reliable Services-alkalmazás létrehozása C#-környezettel</span><span class="sxs-lookup"><span data-stu-id="1cc7d-103">Create your first C# Service Fabric stateful reliable services application</span></span>

<span data-ttu-id="1cc7d-104">Megtudhatja, hogyan helyezheti üzembe mindössze néhány perc alatt első .NET-es Service Fabric-alkalmazását Windows rendszeren.</span><span class="sxs-lookup"><span data-stu-id="1cc7d-104">Learn how to deploy your first Service Fabric application for .NET on Windows in just a few minutes.</span></span> <span data-ttu-id="1cc7d-105">Ha elkészült, rendelkezni fog egy Reliable Services-alkalmazással futó helyi fürttel.</span><span class="sxs-lookup"><span data-stu-id="1cc7d-105">When you're finished, you'll have a local cluster running with a reliable service application.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1cc7d-106">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="1cc7d-106">Prerequisites</span></span>

<span data-ttu-id="1cc7d-107">Mielőtt elkezdené, győződjön meg arról, hogy [beállította a fejlesztőkörnyezetet](service-fabric-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="1cc7d-107">Before you get started, make sure that you have [set up your development environment](service-fabric-get-started.md).</span></span> <span data-ttu-id="1cc7d-108">Ebbe beletartozik a Service Fabric SDK és a Visual Studio 2017 vagy 2015 telepítése is.</span><span class="sxs-lookup"><span data-stu-id="1cc7d-108">This includes installing the Service Fabric SDK and Visual Studio 2017 or 2015.</span></span>

## <a name="create-the-application"></a><span data-ttu-id="1cc7d-109">Az alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="1cc7d-109">Create the application</span></span>

<span data-ttu-id="1cc7d-110">Indítsa el a Visual Studiót **rendszergazdaként**.</span><span class="sxs-lookup"><span data-stu-id="1cc7d-110">Launch Visual Studio as an **administrator**.</span></span>

<span data-ttu-id="1cc7d-111">Projekt létrehozása `CTRL`+`SHIFT`+`N` használatával</span><span class="sxs-lookup"><span data-stu-id="1cc7d-111">Create a project with `CTRL`+`SHIFT`+`N`</span></span>

<span data-ttu-id="1cc7d-112">Az **Új projekt** párbeszédpanelen válassza a **Felhő > Service Fabric-alkalmazás** elemet.</span><span class="sxs-lookup"><span data-stu-id="1cc7d-112">In the **New Project** dialog, choose **Cloud > Service Fabric Application**.</span></span>

<span data-ttu-id="1cc7d-113">Adja a **MyApplication** nevet az alkalmazásnak, majd kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="1cc7d-113">Name the application **MyApplication** and press **OK**.</span></span>

   
![A Visual Studio Új projekt párbeszédpanelje][1]

<span data-ttu-id="1cc7d-115">A következő párbeszédpanelen bármilyen típusú Service Fabric-alkalmazást létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="1cc7d-115">You can create any type of Service Fabric application from the next dialog.</span></span> <span data-ttu-id="1cc7d-116">Ebben a rövid útmutatóban válassza az **Állapotalapú szolgáltatás** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="1cc7d-116">For this Quickstart, choose **Stateful Service**.</span></span>

<span data-ttu-id="1cc7d-117">Adja a **MyStatefulService** nevet a szolgáltatásnak, majd kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="1cc7d-117">Name the service **MyStatefulService** and press **OK**.</span></span>

![A Visual Studio Új szolgáltatás párbeszédpanelje][2]


<span data-ttu-id="1cc7d-119">A Visual Studio létrehozza az alkalmazási projektet és az állapotalapú szolgáltatási projektet, és megjeleníti őket a Megoldáskezelőben.</span><span class="sxs-lookup"><span data-stu-id="1cc7d-119">Visual Studio creates the application project and the stateful service project and displays them in Solution Explorer.</span></span>

![A Megoldáskezelő folytatja az alkalmazás létrehozását állapotalapú szolgáltatással][3]

<span data-ttu-id="1cc7d-121">Az alkalmazásprojekt (**MyApplication**) nem tartalmaz közvetlenül semmilyen kódot.</span><span class="sxs-lookup"><span data-stu-id="1cc7d-121">The application project (**MyApplication**) does not contain any code directly.</span></span> <span data-ttu-id="1cc7d-122">Helyette számos szolgáltatási projektre hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="1cc7d-122">Instead, it references a set of service projects.</span></span> <span data-ttu-id="1cc7d-123">Ezenfelül három egyéb típusú tartalmat is tartalmaz:</span><span class="sxs-lookup"><span data-stu-id="1cc7d-123">In addition, it contains three other types of content:</span></span>

* <span data-ttu-id="1cc7d-124">**Profilok közzététele**</span><span class="sxs-lookup"><span data-stu-id="1cc7d-124">**Publish profiles**</span></span>  
<span data-ttu-id="1cc7d-125">Más környezetekben való üzembe helyezésre szolgáló profilok.</span><span class="sxs-lookup"><span data-stu-id="1cc7d-125">Profiles for deploying to different environments.</span></span>

* <span data-ttu-id="1cc7d-126">**Szkriptek**</span><span class="sxs-lookup"><span data-stu-id="1cc7d-126">**Scripts**</span></span>  
<span data-ttu-id="1cc7d-127">Az alkalmazás üzembe helyezéséhez/frissítéséhez szükséges PowerShell-szkript.</span><span class="sxs-lookup"><span data-stu-id="1cc7d-127">PowerShell script for deploying/upgrading your application.</span></span>

* <span data-ttu-id="1cc7d-128">**Alkalmazásdefiníció**</span><span class="sxs-lookup"><span data-stu-id="1cc7d-128">**Application definition**</span></span>  
<span data-ttu-id="1cc7d-129">Tartalmazza az *ApplicationPackageRoot* területen található ApplicationManifest.xml fájlt, amely leírja az alkalmazás összeállítását.</span><span class="sxs-lookup"><span data-stu-id="1cc7d-129">Includes the ApplicationManifest.xml file under *ApplicationPackageRoot* which describes your application's composition.</span></span> <span data-ttu-id="1cc7d-130">A társított alkalmazásparaméter-fájlok az *ApplicationParameters* területen találhatók, és a környezetspecifikus paraméterek megadására használhatók.</span><span class="sxs-lookup"><span data-stu-id="1cc7d-130">Associated application parameter files are under *ApplicationParameters*, which can be used to specify environment-specific parameters.</span></span> <span data-ttu-id="1cc7d-131">A Visual Studio kiválaszt alkalmazásparaméter-fájlt, amelyet egy adott környezetben való üzembe helyezéskor adott meg a kapcsolódó közzétételi profilban.</span><span class="sxs-lookup"><span data-stu-id="1cc7d-131">Visual Studio selects an application parameter file that's specified in the associated publish profile during deployment to a specific environment.</span></span>
    
<span data-ttu-id="1cc7d-132">A szolgáltatási projekt tartalmának áttekintéséhez lásd: [Bevezetés a Reliable Services használatába](service-fabric-reliable-services-quick-start.md).</span><span class="sxs-lookup"><span data-stu-id="1cc7d-132">For an overview of the contents of the service project, see [Getting started with Reliable Services](service-fabric-reliable-services-quick-start.md).</span></span>

## <a name="deploy-and-debug-the-application"></a><span data-ttu-id="1cc7d-133">Az alkalmazás üzembe helyezése és hibakeresése</span><span class="sxs-lookup"><span data-stu-id="1cc7d-133">Deploy and debug the application</span></span>

<span data-ttu-id="1cc7d-134">Most, hogy már van egy alkalmazása, futtassa.</span><span class="sxs-lookup"><span data-stu-id="1cc7d-134">Now that you have an application, run it.</span></span>

<span data-ttu-id="1cc7d-135">Nyomja le az `F5` billentyűt a Visual Studióban, hogy üzembe helyezze az alkalmazást a hibakereséshez.</span><span class="sxs-lookup"><span data-stu-id="1cc7d-135">In Visual Studio, press `F5` to deploy the application for debugging.</span></span>

>[!NOTE]
><span data-ttu-id="1cc7d-136">Az alkalmazás első helyi történő üzembe helyezésekor a Visual Studio létrehoz egy helyi hibakeresési fürtöt.</span><span class="sxs-lookup"><span data-stu-id="1cc7d-136">The first time you run and deploy the application locally, Visual Studio creates a local cluster for debugging.</span></span> <span data-ttu-id="1cc7d-137">Ez eltarthat egy ideig.</span><span class="sxs-lookup"><span data-stu-id="1cc7d-137">This may take some time.</span></span> <span data-ttu-id="1cc7d-138">A fürt létrehozási állapota a Visual Studio kimeneti ablakában jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="1cc7d-138">The cluster creation status is displayed in the Visual Studio output window.</span></span>

<span data-ttu-id="1cc7d-139">Amikor a fürt létrejött, értesítést kap a helyi fürt SDK-hoz tartozó rendszertálca-kezelő alkalmazásától.</span><span class="sxs-lookup"><span data-stu-id="1cc7d-139">When the cluster is ready, you get a notification from the local cluster system tray manager application included with the SDK.</span></span>
   
![A helyifürt-rendszertálca értesítése][4]

<span data-ttu-id="1cc7d-141">Az alkalmazás elindításakor a Visual Studio automatikusan megjeleníti a **Diagnosztikai eseménynaplót**, ahol az Ön szolgáltatásainak nyomkövetési kimenetei tekinthetők meg.</span><span class="sxs-lookup"><span data-stu-id="1cc7d-141">Once the application starts, Visual Studio automatically brings up the **Diagnostics Event Viewer**, where you can see trace output from your services.</span></span>
   
![Diagnosztikai eseménynapló][5]

<span data-ttu-id="1cc7d-143">Az általunk használt állapotalapúszolgáltatás-sablon esetében egyszerűen a **MyStatefulService.cs** `RunAsync` metódusához tartozó számláló növekvő értékei jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="1cc7d-143">The stateful service template we used simply shows a counter value incrementing in the `RunAsync` method of **MyStatefulService.cs**.</span></span>

<span data-ttu-id="1cc7d-144">Bontsa ki az egyik eseményt, hogy további részleteket tekinthessen meg, beleértve azt a csomópontot is, amelyben a kód fut.</span><span class="sxs-lookup"><span data-stu-id="1cc7d-144">Expand one of the events to see more details, including the node where the code is running.</span></span> <span data-ttu-id="1cc7d-145">Ebben az esetben ez a \_Node\_2, de az Ön számítógépén ez eltérő lehet.</span><span class="sxs-lookup"><span data-stu-id="1cc7d-145">In this case, it is \_Node\_2, though it may differ on your machine.</span></span>
   
![A diagnosztikai eseménynapló részletei][6]

<span data-ttu-id="1cc7d-147">A helyi fürt egyetlen gépen üzemeltetett öt csomópontot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="1cc7d-147">The local cluster contains five nodes hosted on a single machine.</span></span> <span data-ttu-id="1cc7d-148">Éles környezetben minden egyes csomópont más fizikai vagy virtuális gépen üzemel.</span><span class="sxs-lookup"><span data-stu-id="1cc7d-148">In a production environment, each node is hosted on a distinct physical or virtual machine.</span></span> <span data-ttu-id="1cc7d-149">Állítsuk le a helyi fürt egyik csomópontját, hogy szimuláljuk egy gép elvesztését, és kipróbáljuk a Visual Studio hibakereső funkcióját.</span><span class="sxs-lookup"><span data-stu-id="1cc7d-149">To simulate the loss of a machine while exercising the Visual Studio debugger at the same time, let's take down one of the nodes on the local cluster.</span></span>

<span data-ttu-id="1cc7d-150">A **Megoldáskezelő** ablakában nyissa meg a **MyStatefulService.cs** fájlt.</span><span class="sxs-lookup"><span data-stu-id="1cc7d-150">In the **Solution Explorer** window, open **MyStatefulService.cs**.</span></span> 

<span data-ttu-id="1cc7d-151">Keresse meg a(z) `RunAsync` metódust, és az első sorában állítson be egy töréspontot.</span><span class="sxs-lookup"><span data-stu-id="1cc7d-151">Find the `RunAsync` method and set a breakpoint on the first line of the method.</span></span>

![Töréspont az állapotalapú szolgáltatás RunAsync metódusában][7]

<span data-ttu-id="1cc7d-153">A **Service Fabric Explorer** eszköz elindításához kattintson a jobb gombbal a **Local Cluster Manager** rendszertálca-alkalmazásra, és válassza a **Helyi fürt kezelése** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="1cc7d-153">Launch the **Service Fabric Explorer** tool by right-clicking on the **Local Cluster Manager** system tray application and choose **Manage Local Cluster**.</span></span>

![A Manage Local Cluster alkalmazásból indítsa el a Service Fabric Explorert.][systray-launch-sfx]

<span data-ttu-id="1cc7d-155">A [**Service Fabric Explorer**](service-fabric-visualizing-your-cluster.md) vizuálisan is megjeleníti a fürtöket,</span><span class="sxs-lookup"><span data-stu-id="1cc7d-155">[**Service Fabric Explorer**](service-fabric-visualizing-your-cluster.md) offers a visual representation of a cluster.</span></span> <span data-ttu-id="1cc7d-156">beleértve a rajtuk üzembe helyezett alkalmazáskészletet és az őket felépítő fizikai csomópontokat is.</span><span class="sxs-lookup"><span data-stu-id="1cc7d-156">It includes the set of applications deployed to it and the set of physical nodes that make it up.</span></span>

<span data-ttu-id="1cc7d-157">A bal oldali panelen bontsa ki a **Cluster > Nodes** (Fürt > Csomópontok) elemet, és keresse meg azt csomópont, amelyikben a kódja fut.</span><span class="sxs-lookup"><span data-stu-id="1cc7d-157">In the left pane, expand **Cluster > Nodes** and find the node where your code is running.</span></span>

<span data-ttu-id="1cc7d-158">Kattintson az **Actions > Deactivate (Restart)** (Műveletek > Inaktiválás (Újraindítás)) elemre a számítógép-újraindítás szimulálásához.</span><span class="sxs-lookup"><span data-stu-id="1cc7d-158">Click **Actions > Deactivate (Restart)** to simulate a machine restarting.</span></span>

![Csomópont leállítása a Service Fabric Explorerben][sfx-stop-node]

<span data-ttu-id="1cc7d-160">Pillanatnyilag a töréspont megjelenését tekintheti meg a Visual Studióban, mivel az egyik csomóponton korábban végzett számítása zökkenőmentesen átadja a feladatokat egy másiknak.</span><span class="sxs-lookup"><span data-stu-id="1cc7d-160">Momentarily, you should see your breakpoint hit in Visual Studio as the computation you were doing on one node seamlessly fails over to another.</span></span>


<span data-ttu-id="1cc7d-161">Ezután térjen vissza a Diagnosztikai eseménynaplóhoz, és vizsgálja meg az üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="1cc7d-161">Next, return to the Diagnostic Events Viewer and observe the messages.</span></span> <span data-ttu-id="1cc7d-162">A számláló értéke továbbra is növekszik, annak ellenére, hogy az események valójában egy másik csomópontról érkeznek.</span><span class="sxs-lookup"><span data-stu-id="1cc7d-162">The counter has continued incrementing, even though the events are actually coming from a different node.</span></span>

![A diagnosztikai eseménynapló a feladatátvétel után][diagnostic-events-viewer-detail-post-failover]

## <a name="cleaning-up-the-local-cluster-optional"></a><span data-ttu-id="1cc7d-164">A helyi fürt törlése (nem kötelező)</span><span class="sxs-lookup"><span data-stu-id="1cc7d-164">Cleaning up the local cluster (optional)</span></span>

<span data-ttu-id="1cc7d-165">Ne feledje, hogy ez a helyi fürt valós.</span><span class="sxs-lookup"><span data-stu-id="1cc7d-165">Remember, this local cluster is real.</span></span> <span data-ttu-id="1cc7d-166">A hibakereső leállításával eltávolítja az adott alkalmazáspéldányt, és törli az alkalmazástípus regisztrációját.</span><span class="sxs-lookup"><span data-stu-id="1cc7d-166">Stopping the debugger removes your application instance and unregisters the application type.</span></span> <span data-ttu-id="1cc7d-167">A fürt futtatása azonban tovább folytatódik a háttérben.</span><span class="sxs-lookup"><span data-stu-id="1cc7d-167">However, the cluster continues to run in the background.</span></span> <span data-ttu-id="1cc7d-168">Ha felkészült a helyi fürt leállítására, erre többféle lehetőség is rendelkezésre áll.</span><span class="sxs-lookup"><span data-stu-id="1cc7d-168">When you're ready to stop the local cluster, there are a couple options.</span></span>

### <a name="keep-application-and-trace-data"></a><span data-ttu-id="1cc7d-169">Alkalmazás- és nyomkövetési adatok megtartása</span><span class="sxs-lookup"><span data-stu-id="1cc7d-169">Keep application and trace data</span></span>

<span data-ttu-id="1cc7d-170">A fürt leállításához kattintson a jobb gombbal a **Local Cluster Manager** rendszertálca-alkalmazásra, és válassza a **Helyi fürt leállítása** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="1cc7d-170">Shut down the cluster by right-clicking on the **Local Cluster Manager** system tray application and then choose **Stop Local Cluster**.</span></span>

### <a name="delete-the-cluster-and-all-data"></a><span data-ttu-id="1cc7d-171">A fürt és minden adat törlése</span><span class="sxs-lookup"><span data-stu-id="1cc7d-171">Delete the cluster and all data</span></span>

<span data-ttu-id="1cc7d-172">A fürt eltávolításához kattintson a jobb gombbal a **Local Cluster Manager** rendszertálca-alkalmazásra, és válassza a **Helyi fürt eltávolítása** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="1cc7d-172">Remove the cluster by right-clicking on the **Local Cluster Manager** system tray application and then choose **Remove Local Cluster**.</span></span> 

<span data-ttu-id="1cc7d-173">Ha ezt a lehetőséget választja, a Visual Studio az alkalmazás legközelebbi futtatásakor ismét üzembe helyezi a fürtöt.</span><span class="sxs-lookup"><span data-stu-id="1cc7d-173">If you choose this option, Visual Studio will redeploy the cluster the next time your run the application.</span></span> <span data-ttu-id="1cc7d-174">Akkor válassza ezt a beállítást, ha egy ideig nem kívánja használni a helyi fürtöt, vagy ha erőforrásokat kíván felszabadítani.</span><span class="sxs-lookup"><span data-stu-id="1cc7d-174">Choose this option if you don't intend to use the local cluster for some time or if you need to reclaim resources.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1cc7d-175">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1cc7d-175">Next steps</span></span>
<span data-ttu-id="1cc7d-176">További információk a [Reliable Services](service-fabric-reliable-services-introduction.md)-szolgáltatásokról.</span><span class="sxs-lookup"><span data-stu-id="1cc7d-176">Read more about [reliable services](service-fabric-reliable-services-introduction.md).</span></span>
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
