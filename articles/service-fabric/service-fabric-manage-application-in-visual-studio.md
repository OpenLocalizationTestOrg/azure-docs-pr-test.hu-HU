---
title: "Kezeli az alkalmazásokat a Visual Studio |} Microsoft Docs"
description: "Visual Studio használatával hozzon létre, fejlesztése, csomag, a központi telepítése és a hibakereséshez a Service Fabric-alkalmazások és szolgáltatások."
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: timlt
editor: 
ms.assetid: c317cb7e-7eae-466e-ba41-6aa2518be5cf
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/07/2017
ms.author: mikkelhegn
ms.openlocfilehash: 3f6a47a15b74a7ceb6504b2834be62e76ab70bcc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-visual-studio-to-simplify-writing-and-managing-your-service-fabric-applications"></a><span data-ttu-id="5cd0a-103">Visual Studio segítségével egyszerűsítheti az írást, és a Service Fabric-alkalmazások kezelése</span><span class="sxs-lookup"><span data-stu-id="5cd0a-103">Use Visual Studio to simplify writing and managing your Service Fabric applications</span></span>
<span data-ttu-id="5cd0a-104">Az Azure Service Fabric-alkalmazások és szolgáltatások Visual Studio alkalmazással kezelheti.</span><span class="sxs-lookup"><span data-stu-id="5cd0a-104">You can manage your Azure Service Fabric applications and services through Visual Studio.</span></span> <span data-ttu-id="5cd0a-105">Miután megismerte [beállította a fejlesztőkörnyezetet](service-fabric-get-started.md), használhatja a Visual Studio Service Fabric-alkalmazások létrehozása, adja hozzá a szolgáltatásokhoz, vagy a csomag, regisztrálása és alkalmazások telepítése a helyi fejlesztési fürtöt.</span><span class="sxs-lookup"><span data-stu-id="5cd0a-105">Once you've [set up your development environment](service-fabric-get-started.md), you can use Visual Studio to create Service Fabric applications, add services, or package, register, and deploy applications in your local development cluster.</span></span>

## <a name="deploy-your-service-fabric-application"></a><span data-ttu-id="5cd0a-106">A Service Fabric-alkalmazás központi telepítése</span><span class="sxs-lookup"><span data-stu-id="5cd0a-106">Deploy your Service Fabric application</span></span>
<span data-ttu-id="5cd0a-107">Alapértelmezés szerint az alkalmazás központi telepítése egyesíti be egy egyszerű feladat a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="5cd0a-107">By default, deploying an application combines the following steps into one simple operation:</span></span>

1. <span data-ttu-id="5cd0a-108">Az alkalmazáscsomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="5cd0a-108">Creating the application package</span></span>
2. <span data-ttu-id="5cd0a-109">Az image store az alkalmazáscsomag feltöltése</span><span class="sxs-lookup"><span data-stu-id="5cd0a-109">Uploading the application package to the image store</span></span>
3. <span data-ttu-id="5cd0a-110">Az alkalmazástípus regisztrálása</span><span class="sxs-lookup"><span data-stu-id="5cd0a-110">Registering the application type</span></span>
4. <span data-ttu-id="5cd0a-111">Eltávolítani a alkalmazáspéldányok fut</span><span class="sxs-lookup"><span data-stu-id="5cd0a-111">Removing any running application instances</span></span>
5. <span data-ttu-id="5cd0a-112">Az alkalmazáspéldány létrehozása</span><span class="sxs-lookup"><span data-stu-id="5cd0a-112">Creating an application instance</span></span>

<span data-ttu-id="5cd0a-113">A Visual Studio lenyomásával **F5** telepíti az alkalmazást, és csatolja a hibakereső alkalmazás összes példányát.</span><span class="sxs-lookup"><span data-stu-id="5cd0a-113">In Visual Studio, pressing **F5** deploys your application and attach the debugger to all application instances.</span></span> <span data-ttu-id="5cd0a-114">Használhat **Ctrl + F5** nem hibakeresés, illetve hogy az alkalmazás központi telepítéséről is közzéteheti a helyi vagy távoli fürt a közzétételi profil használatával.</span><span class="sxs-lookup"><span data-stu-id="5cd0a-114">You can use **Ctrl+F5** to deploy an application without debugging, or you can publish to a local or remote cluster by using the publish profile.</span></span> <span data-ttu-id="5cd0a-115">További információkért lásd: [távoli fürtre alkalmazás közzététele a Visual Studio használatával](service-fabric-publish-app-remote-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="5cd0a-115">For more information, see [Publish an application to a remote cluster by using Visual Studio](service-fabric-publish-app-remote-cluster.md).</span></span>

### <a name="application-debug-mode"></a><span data-ttu-id="5cd0a-116">Alkalmazás hibakeresési módban</span><span class="sxs-lookup"><span data-stu-id="5cd0a-116">Application Debug Mode</span></span>
<span data-ttu-id="5cd0a-117">A Visual Studio adja meg a tulajdonságot, **alkalmazás hibakeresési módban**, amely meghatározza, hogyan szeretné kezelni az alkalmazás központi telepítésének részeként a Hibakeresés Visual Studios.</span><span class="sxs-lookup"><span data-stu-id="5cd0a-117">Visual Studio provide a property called **Application Debug Mode**, which controls how you want Visual Studios to handle Application deployment as part of debugging.</span></span>

#### <a name="to-set-the-application-debug-mode-property"></a><span data-ttu-id="5cd0a-118">Az alkalmazás hibakeresési módban tulajdonság beállítása</span><span class="sxs-lookup"><span data-stu-id="5cd0a-118">To set the Application Debug Mode property</span></span>
1. <span data-ttu-id="5cd0a-119">A Service Fabric application projekt (*.sfproj) a helyi menüben válassza a **tulajdonságok** (vagy nyomja le az ENTER a **F4** kulcs).</span><span class="sxs-lookup"><span data-stu-id="5cd0a-119">On the Service Fabric application project's (*.sfproj) shortcut menu, choose **Properties** (or press the **F4** key).</span></span>
2. <span data-ttu-id="5cd0a-120">Az a **tulajdonságok** ablakban a **alkalmazás hibakeresési módban** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="5cd0a-120">In the **Properties** window, set the **Application Debug Mode** property.</span></span>

![Állítsa be az alkalmazás hibakeresési módban tulajdonság][debugmodeproperty]

#### <a name="application-debug-modes"></a><span data-ttu-id="5cd0a-122">Alkalmazás hibakeresési üzemmód</span><span class="sxs-lookup"><span data-stu-id="5cd0a-122">Application Debug Modes</span></span>

1. <span data-ttu-id="5cd0a-123">**Frissítse az alkalmazás** ebben a módban lehetővé teszi, hogy gyorsan módosítása és hibakeresése a kód és a statikus webes fájlok szerkesztésének hibakeresés során támogatja.</span><span class="sxs-lookup"><span data-stu-id="5cd0a-123">**Refresh Application** This mode enables you to quickly change and debug your code and supports editing static web files while debugging.</span></span> <span data-ttu-id="5cd0a-124">Ebben az üzemmódban csak akkor működik, ha a helyi fejlesztési fürtök [1-csomópont mód](/service-fabric-get-started-with-a-local-cluster.md#one-node-and-five-node-cluster-mode).</span><span class="sxs-lookup"><span data-stu-id="5cd0a-124">This mode only works if your local development cluster is in [1-Node mode](/service-fabric-get-started-with-a-local-cluster.md#one-node-and-five-node-cluster-mode).</span></span>
2. <span data-ttu-id="5cd0a-125">**Távolítsa el az alkalmazás** az alkalmazás távolítható el, amikor a hibakeresési munkamenet befejeződik.</span><span class="sxs-lookup"><span data-stu-id="5cd0a-125">**Remove Application** causes the application to be removed when the debug session ends.</span></span>
3. <span data-ttu-id="5cd0a-126">**Automatikus frissítés** az alkalmazás továbbra is fut, amikor a hibakeresési munkamenet befejeződik.</span><span class="sxs-lookup"><span data-stu-id="5cd0a-126">**Auto Upgrade** The application continues to run when the debug session ends.</span></span> <span data-ttu-id="5cd0a-127">A következő hibakeresési munkamenetben kezeli az üzemelő példány frissítése.</span><span class="sxs-lookup"><span data-stu-id="5cd0a-127">The next debug session will treat the deployment as an upgrade.</span></span> <span data-ttu-id="5cd0a-128">A frissítési folyamat megőrzi a korábbi hibakeresési munkamenetben megadott adatokat.</span><span class="sxs-lookup"><span data-stu-id="5cd0a-128">The upgrade process preserves any data that you entered in a previous debug session.</span></span>
4. <span data-ttu-id="5cd0a-129">**Alkalmazás megtartása** az alkalmazás a fürt folyamatosan működik, amikor a hibakeresési munkamenet befejeződik.</span><span class="sxs-lookup"><span data-stu-id="5cd0a-129">**Keep Application** The application keeps running in the cluster when the debug session ends.</span></span> <span data-ttu-id="5cd0a-130">A következő hibakeresési munkamenet elején, az alkalmazás törlődni fog.</span><span class="sxs-lookup"><span data-stu-id="5cd0a-130">At the beginning of the next debug session, the application will be removed.</span></span>

<span data-ttu-id="5cd0a-131">A **automatikus frissítése** adatok megmaradjanak a alkalmazás Service Fabric frissítési lehetőségeit alkalmazásával.</span><span class="sxs-lookup"><span data-stu-id="5cd0a-131">For **Auto Upgrade** data is preserved by applying the application upgrade capabilities of Service Fabric.</span></span> <span data-ttu-id="5cd0a-132">Alkalmazások, és hogyan lehet, hogy frissíti az valós környezetben történő frissítéssel kapcsolatos további információkért lásd: [Service Fabric az alkalmazásfrissítés](service-fabric-application-upgrade.md).</span><span class="sxs-lookup"><span data-stu-id="5cd0a-132">For more information about upgrading applications and how you might perform an upgrade in a real environment, see [Service Fabric application upgrade](service-fabric-application-upgrade.md).</span></span>

## <a name="add-a-service-to-your-service-fabric-application"></a><span data-ttu-id="5cd0a-133">Szolgáltatás hozzáadása a Service Fabric-alkalmazás</span><span class="sxs-lookup"><span data-stu-id="5cd0a-133">Add a service to your Service Fabric application</span></span>
<span data-ttu-id="5cd0a-134">Új szolgáltatások adhat hozzá az alkalmazás a bővíthetők.</span><span class="sxs-lookup"><span data-stu-id="5cd0a-134">You can add new services to your application to extend its functionality.</span></span>  <span data-ttu-id="5cd0a-135">Győződjön meg arról, hogy a szolgáltatás része az alkalmazáscsomag, vegye fel a szolgáltatásba a **új Fabric-szolgáltatás...**  menüpont.</span><span class="sxs-lookup"><span data-stu-id="5cd0a-135">To ensure that the service is included in your application package, add the service through the **New Fabric Service...** menu item.</span></span>

![Egy új Service Fabric-szolgáltatás hozzáadása][newservice]

<span data-ttu-id="5cd0a-137">Jelöljön ki egy Service Fabric projekt hozzá az alkalmazáshoz, és adja meg a szolgáltatás nevét.</span><span class="sxs-lookup"><span data-stu-id="5cd0a-137">Select a Service Fabric project type to add to your application, and specify a name for the service.</span></span>  <span data-ttu-id="5cd0a-138">Lásd: [keretrendszere, amely a szolgáltatás kiválasztása](service-fabric-choose-framework.md) , hogy eldönthesse, melyik szolgáltatás típust használ.</span><span class="sxs-lookup"><span data-stu-id="5cd0a-138">See [Choosing a framework for your service](service-fabric-choose-framework.md) to help you decide which service type to use.</span></span>

![Jelöljön ki egy Service Fabric szolgáltatás projekt hozzá az alkalmazáshoz][addserviceproject]

<span data-ttu-id="5cd0a-140">Az új szolgáltatás a megoldás és a meglévő alkalmazáscsomag kerül.</span><span class="sxs-lookup"><span data-stu-id="5cd0a-140">The new service is added to your solution and existing application package.</span></span> <span data-ttu-id="5cd0a-141">A szolgáltatási hivatkozást lekérni, és egy alapértelmezett szolgáltatáspéldány kerülnek be az alkalmazás jegyzékében, amely a szolgáltatás létrehozása és a következő alkalommal, az alkalmazás központi telepítése megkezdődött.</span><span class="sxs-lookup"><span data-stu-id="5cd0a-141">The service references and a default service instance will be added to the application manifest, causing the service to be created and started the next time you deploy the application.</span></span>

![Az új szolgáltatást hozzáadta az alkalmazásjegyzék][newserviceapplicationmanifest]

## <a name="package-your-service-fabric-application"></a><span data-ttu-id="5cd0a-143">A Service Fabric-alkalmazás becsomagolása</span><span class="sxs-lookup"><span data-stu-id="5cd0a-143">Package your Service Fabric application</span></span>
<span data-ttu-id="5cd0a-144">Az alkalmazás- és a fürt telepítéséhez szüksége egy alkalmazáscsomag létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="5cd0a-144">To deploy the application and its services to a cluster, you need to create an application package.</span></span>  <span data-ttu-id="5cd0a-145">A csomag rendszerezi az alkalmazás jegyzékében szolgáltatás jegyzékfájlban és egyéb szükséges fájlok egy adott elrendezésben.</span><span class="sxs-lookup"><span data-stu-id="5cd0a-145">The package organizes the application manifest, service manifests, and other necessary files in a specific layout.</span></span>  <span data-ttu-id="5cd0a-146">A Visual Studio állít be, és az alkalmazási projekt mappában, a "pkg" könyvtárban csomag kezeli.</span><span class="sxs-lookup"><span data-stu-id="5cd0a-146">Visual Studio sets up and manages the package in the application project's folder, in the 'pkg' directory.</span></span>  <span data-ttu-id="5cd0a-147">Kattintson a **csomag** a a **alkalmazás** helyi menü hoz létre, vagy az alkalmazás-csomag frissítése.</span><span class="sxs-lookup"><span data-stu-id="5cd0a-147">Clicking **Package** from the **Application** context menu creates or updates the application package.</span></span>

## <a name="remove-applications-and-application-types-using-cloud-explorer"></a><span data-ttu-id="5cd0a-148">Távolítsa el az alkalmazások és a Cloud Explorer használatával alkalmazástípusok</span><span class="sxs-lookup"><span data-stu-id="5cd0a-148">Remove applications and application types using Cloud Explorer</span></span>
<span data-ttu-id="5cd0a-149">Műveleteket hajthat végre alapszintű fürt felügyeleti belül a Visual Studio használatával indítja el a Cloud Explorerben a **nézet** menü.</span><span class="sxs-lookup"><span data-stu-id="5cd0a-149">You can perform basic cluster management operations from within Visual Studio using Cloud Explorer, which you can launch from the **View** menu.</span></span> <span data-ttu-id="5cd0a-150">Például alkalmazások törlése és leépíteni a következőt: alkalmazástípusok helyi vagy távoli fürtökön.</span><span class="sxs-lookup"><span data-stu-id="5cd0a-150">For instance, you can delete applications and unprovision application types on local or remote clusters.</span></span>

![Alkalmazások eltávolítása][removeapplication]

> [!TIP]
> <span data-ttu-id="5cd0a-152">Több fürt kezelési funkcióját, lásd: [a fürt megjelenítése a Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="5cd0a-152">For richer cluster management functionality, see [Visualizing your cluster with Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).</span></span>
>
>

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a><span data-ttu-id="5cd0a-153">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5cd0a-153">Next steps</span></span>
* [<span data-ttu-id="5cd0a-154">A Service Fabric alkalmazásmodellt.</span><span class="sxs-lookup"><span data-stu-id="5cd0a-154">Service Fabric application model</span></span>](service-fabric-application-model.md)
* [<span data-ttu-id="5cd0a-155">A Service Fabric-alkalmazás központi telepítése</span><span class="sxs-lookup"><span data-stu-id="5cd0a-155">Service Fabric application deployment</span></span>](service-fabric-deploy-remove-applications.md)
* [<span data-ttu-id="5cd0a-156">Alkalmazás paramétereinek több környezet kezelése</span><span class="sxs-lookup"><span data-stu-id="5cd0a-156">Managing application parameters for multiple environments</span></span>](service-fabric-manage-multiple-environment-app-configuration.md)
* [<span data-ttu-id="5cd0a-157">A Service Fabric-alkalmazás hibakeresés</span><span class="sxs-lookup"><span data-stu-id="5cd0a-157">Debugging your Service Fabric application</span></span>](service-fabric-debugging-your-application.md)
* [<span data-ttu-id="5cd0a-158">A fürt megjelenítése a Service Fabric Explorer használatával</span><span class="sxs-lookup"><span data-stu-id="5cd0a-158">Visualizing your cluster by using Service Fabric Explorer</span></span>](service-fabric-visualizing-your-cluster.md)

<!--Image references-->
[addserviceproject]:./media/service-fabric-manage-application-in-visual-studio/addserviceproject.png
[manageservicefabric]: ./media/service-fabric-manage-application-in-visual-studio/manageservicefabric.png
[newservice]:./media/service-fabric-manage-application-in-visual-studio/newservice.png
[newserviceapplicationmanifest]:./media/service-fabric-manage-application-in-visual-studio/newserviceapplicationmanifest.png
[debugmodeproperty]:./media/service-fabric-manage-application-in-visual-studio/debugmodeproperty.png
[removeapplication]:./media/service-fabric-manage-application-in-visual-studio/removeapplication.png