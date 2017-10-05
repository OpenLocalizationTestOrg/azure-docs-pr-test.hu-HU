---
title: "Alkalmazás üzembe helyezése Azure Service Fabric fél fürthöz |} Microsoft Docs"
description: "Megtudhatja, hogyan fél fürtre alkalmazás központi telepítése."
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: msfussell
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/09/2017
ms.author: mikhegn
ms.openlocfilehash: 6624d683edb548a65d07ab4012c599faaf940ed0
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="deploy-an-application-to-a-party-cluster-in-azure"></a><span data-ttu-id="91f62-103">Alkalmazás üzembe helyezése az Azure-ban fél fürtre</span><span class="sxs-lookup"><span data-stu-id="91f62-103">Deploy an application to a Party Cluster in Azure</span></span>
<span data-ttu-id="91f62-104">Ez az oktatóanyag két egy sor és bemutatja, hogyan fél fürthöz az Azure-ban az Azure Service Fabric-alkalmazás központi telepítéséről.</span><span class="sxs-lookup"><span data-stu-id="91f62-104">This tutorial is part two of a series and shows you how to deploy an Azure Service Fabric application to a Party Cluster in Azure.</span></span>

<span data-ttu-id="91f62-105">A második rész az oktatóanyag sorozatának, megismerheti, hogyan:</span><span class="sxs-lookup"><span data-stu-id="91f62-105">In part two of the tutorial series, you learn how to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="91f62-106">A távoli fürt Visual Studio alkalmazás központi telepítése</span><span class="sxs-lookup"><span data-stu-id="91f62-106">Deploy an application to a remote cluster using Visual Studio</span></span>
> * <span data-ttu-id="91f62-107">Az alkalmazás eltávolítása egy fürtről Service Fabric Explorerrel</span><span class="sxs-lookup"><span data-stu-id="91f62-107">Remove an application from a cluster using Service Fabric Explorer</span></span>

<span data-ttu-id="91f62-108">Az oktatóanyag adatsorozat elsajátíthatja, hogyan:</span><span class="sxs-lookup"><span data-stu-id="91f62-108">In this tutorial series you learn how to:</span></span>
> [!div class="checklist"]
> * [<span data-ttu-id="91f62-109">A .NET Service Fabric-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="91f62-109">Build a .NET Service Fabric application</span></span>](service-fabric-tutorial-create-dotnet-app.md)
> * <span data-ttu-id="91f62-110">Telepítse központilag az alkalmazást egy távoli fürthöz</span><span class="sxs-lookup"><span data-stu-id="91f62-110">Deploy the application to a remote cluster</span></span>
> * [<span data-ttu-id="91f62-111">Konfigurálja a CI/CD Visual Studio Team Services használatával</span><span class="sxs-lookup"><span data-stu-id="91f62-111">Configure CI/CD using Visual Studio Team Services</span></span>](service-fabric-tutorial-deploy-app-with-cicd-vsts.md)

## <a name="prerequisites"></a><span data-ttu-id="91f62-112">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="91f62-112">Prerequisites</span></span>
<span data-ttu-id="91f62-113">Ez az oktatóanyag elkezdéséhez:</span><span class="sxs-lookup"><span data-stu-id="91f62-113">Before you begin this tutorial:</span></span>
- <span data-ttu-id="91f62-114">Ha nem rendelkezik Azure-előfizetéssel, hozzon létre egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)</span><span class="sxs-lookup"><span data-stu-id="91f62-114">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)</span></span>
- <span data-ttu-id="91f62-115">[Telepítse a Visual Studio 2017](https://www.visualstudio.com/) és telepítse a **Azure fejlesztési** és **ASP.NET és a webes fejlesztési** munkaterhelések.</span><span class="sxs-lookup"><span data-stu-id="91f62-115">[Install Visual Studio 2017](https://www.visualstudio.com/) and install the **Azure development** and **ASP.NET and web development** workloads.</span></span>
- [<span data-ttu-id="91f62-116">A Service Fabric SDK telepítése</span><span class="sxs-lookup"><span data-stu-id="91f62-116">Install the Service Fabric SDK</span></span>](service-fabric-get-started.md)

## <a name="download-the-voting-sample-application"></a><span data-ttu-id="91f62-117">Töltse le a szavazási mintaalkalmazást</span><span class="sxs-lookup"><span data-stu-id="91f62-117">Download the Voting sample application</span></span>
<span data-ttu-id="91f62-118">Ha Ön nem összeállítása a szavazási mintaalkalmazást [rész az oktatóanyag adatsorozat](service-fabric-tutorial-create-dotnet-app.md), tölthető le.</span><span class="sxs-lookup"><span data-stu-id="91f62-118">If you did not build the Voting sample application in [part one of this tutorial series](service-fabric-tutorial-create-dotnet-app.md), you can download it.</span></span> <span data-ttu-id="91f62-119">Egy parancssorban a következő parancsot a helyi számítógépen, a minta app tárház klónozása.</span><span class="sxs-lookup"><span data-stu-id="91f62-119">In a command window, run the following command to clone the sample app repository to your local machine.</span></span>

```
git clone https://github.com/Azure-Samples/service-fabric-dotnet-quickstart
```

## <a name="set-up-a-party-cluster"></a><span data-ttu-id="91f62-120">Egy entitás fürt beállítása</span><span class="sxs-lookup"><span data-stu-id="91f62-120">Set up a Party Cluster</span></span>
<span data-ttu-id="91f62-121">A nyilvános fürtök ingyenes, korlátozott időtartamú Azure Service Fabric-fürtök, amelyek futtatását a Service Fabric csapata végzi, és amelyeken bárki üzembe helyezhet alkalmazásokat, és megismerkedhet a platform használatával.</span><span class="sxs-lookup"><span data-stu-id="91f62-121">Party clusters are free, limited-time Service Fabric clusters hosted on Azure and run by the Service Fabric team where anyone can deploy applications and learn about the platform.</span></span> <span data-ttu-id="91f62-122">Az ingyenes!</span><span class="sxs-lookup"><span data-stu-id="91f62-122">For free!</span></span>

<span data-ttu-id="91f62-123">Egy entitás fürt eléréséhez, keresse meg a hely: http://aka.ms/tryservicefabric és kövesse az utasításokat a fürt eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="91f62-123">To get access to a Party Cluster, browse to this site: http://aka.ms/tryservicefabric and follow the instructions to get access to a cluster.</span></span> <span data-ttu-id="91f62-124">Egy entitás fürt eléréséhez Facebook-on vagy a GitHub-fiók szükséges.</span><span class="sxs-lookup"><span data-stu-id="91f62-124">You need a Facebook or GitHub account to get access to a Party Cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="91f62-125">Entitás fürtök nem biztonságosak, így az alkalmazások és azok helyezett adatokat esetleg mások is láthatják másoknak.</span><span class="sxs-lookup"><span data-stu-id="91f62-125">Party clusters are not secured, so your applications and any data you put in them may be visible to others.</span></span> <span data-ttu-id="91f62-126">Ne telepítsen semmit nem szeretné, hogy mások is láthatják.</span><span class="sxs-lookup"><span data-stu-id="91f62-126">Don't deploy anything you don't want others to see.</span></span> <span data-ttu-id="91f62-127">Mindenképpen olvassa el a használati feltételek a részletekért keresztül.</span><span class="sxs-lookup"><span data-stu-id="91f62-127">Be sure to read over our Terms of Use for all the details.</span></span>

## <a name="configure-the-listening-port"></a><span data-ttu-id="91f62-128">A figyelő portja konfigurálása</span><span class="sxs-lookup"><span data-stu-id="91f62-128">Configure the listening port</span></span>
<span data-ttu-id="91f62-129">A VotingWeb előtér-szolgáltatás létrehozásakor, a Visual Studio véletlenszerűen választ egy portot a figyelést a szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="91f62-129">When the VotingWeb front-end service is created, Visual Studio randomly selects a port for the service to listen on.</span></span>  <span data-ttu-id="91f62-130">A VotingWeb szolgáltatás úgy működik, mint az előtér-alkalmazás, és elfogadja a külső forgalom, most, hogy a szolgáltatás kötése egy rögzített méretű és jól ismeri port.</span><span class="sxs-lookup"><span data-stu-id="91f62-130">The VotingWeb service acts as the front-end for this application and accepts external traffic, so let's bind that service to a fixed and well-know port.</span></span> <span data-ttu-id="91f62-131">A Solution Explorerben nyissa meg a *VotingWeb/PackageRoot/ServiceManifest.xml*.</span><span class="sxs-lookup"><span data-stu-id="91f62-131">In Solution Explorer, open  *VotingWeb/PackageRoot/ServiceManifest.xml*.</span></span>  <span data-ttu-id="91f62-132">Keresés a **végpont** erőforrás a **erőforrások** szakaszt, és módosítsa a **Port** 80-as értéket.</span><span class="sxs-lookup"><span data-stu-id="91f62-132">Find the **Endpoint** resource in the **Resources** section and change the **Port** value to 80.</span></span>

```xml
<Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port on which to 
           listen. Please note that if your service is partitioned, this port is shared with 
           replicas of different partitions that are placed in your code. -->
      <Endpoint Protocol="http" Name="ServiceEndpoint" Type="Input" Port="80" />
    </Endpoints>
  </Resources>
```

<span data-ttu-id="91f62-133">Az alkalmazás URL-Címének tulajdonság értéke a szavazási projekt frissíteni, egy webböngészőben megnyílik a megfelelő porthoz hibakeresése "F5" használatával.</span><span class="sxs-lookup"><span data-stu-id="91f62-133">Also update the Application URL property value in the Voting project so a web browser opens to the correct port when you debug using 'F5'.</span></span>  <span data-ttu-id="91f62-134">A Megoldáskezelőben, válassza ki a **Voting** projektet és a frissítés a **alkalmazás URL-Címének** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="91f62-134">In Solution Explorer, select the **Voting** project and update the **Application URL** property.</span></span>

![Alkalmazás URL-címe](./media/service-fabric-tutorial-deploy-app-to-party-cluster/application-url.png)

## <a name="deploy-the-app-to-the-azure"></a><span data-ttu-id="91f62-136">Az alkalmazás telepítése az Azure-bA</span><span class="sxs-lookup"><span data-stu-id="91f62-136">Deploy the app to the Azure</span></span>
<span data-ttu-id="91f62-137">Most, hogy az alkalmazás készen áll, ezután telepítheti azt a közvetlen fél fürthöz a Visual Studio eszközből.</span><span class="sxs-lookup"><span data-stu-id="91f62-137">Now that the application is ready, you can deploy it to the Party Cluster direct from Visual Studio.</span></span>

1. <span data-ttu-id="91f62-138">Kattintson a jobb gombbal **Voting** a Megoldáskezelőben kattintson **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="91f62-138">Right-click **Voting** in the Solution Explorer and choose **Publish**.</span></span>

    ![Publish (Közzététel) párbeszédpanel](./media/service-fabric-tutorial-deploy-app-to-party-cluster/publish-app.png)

2. <span data-ttu-id="91f62-140">A csatlakozási végpont a fél fürt írja be a **csatlakozási végpont** mezőben, majd kattintson a **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="91f62-140">Type in the Connection Endpoint of the Party Cluster in the **Connection Endpoint** field and click **Publish**.</span></span>

    <span data-ttu-id="91f62-141">Miután befejezte a közzététel, kell kérelmet küld az alkalmazás böngésző használatával.</span><span class="sxs-lookup"><span data-stu-id="91f62-141">Once the publish has finished, you should be able to send a request to the application via a browser.</span></span>

3. <span data-ttu-id="91f62-142">Nyissa meg előnyben részesített böngészőt, és írja be a fürt címe (a csatlakozási végpont a port adatok, például win1kw5649s.westus.cloudapp.azure.com nélkül).</span><span class="sxs-lookup"><span data-stu-id="91f62-142">Open you preferred browser and type in the cluster address (the connection endpoint without the port information - for example, win1kw5649s.westus.cloudapp.azure.com).</span></span>

    <span data-ttu-id="91f62-143">Ekkor megjelenik a ugyanazt az eredményt, ahogy azt az alkalmazást a helyi futtatás során.</span><span class="sxs-lookup"><span data-stu-id="91f62-143">You should now see the same result as you saw when running the application locally.</span></span>

    ![A fürt API-válaszból](./media/service-fabric-tutorial-deploy-app-to-party-cluster/response-from-cluster.png)

## <a name="remove-the-application-from-a-cluster-using-service-fabric-explorer"></a><span data-ttu-id="91f62-145">Az alkalmazás eltávolítása egy fürtről Service Fabric Explorerrel</span><span class="sxs-lookup"><span data-stu-id="91f62-145">Remove the application from a cluster using Service Fabric Explorer</span></span>
<span data-ttu-id="91f62-146">Service Fabric Explorer egy grafikus felhasználói felület vizsgálatát, és a Service Fabric-fürt alkalmazásokat kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="91f62-146">Service Fabric Explorer is a graphical user interface to explore and manage applications in a Service Fabric cluster.</span></span>

<span data-ttu-id="91f62-147">Az alkalmazás eltávolítása a fél fürt:</span><span class="sxs-lookup"><span data-stu-id="91f62-147">To remove the application from the Party Cluster:</span></span>

1. <span data-ttu-id="91f62-148">Tallózással keresse meg a Service Fabric Explorer által fél fürt feliratkozáshoz hivatkozás segítségével.</span><span class="sxs-lookup"><span data-stu-id="91f62-148">Browse to the Service Fabric Explorer, using the link provided by the Party Cluster sign-up page.</span></span> <span data-ttu-id="91f62-149">Például http://win1kw5649s.westus.cloudapp.azure.com:19080/Explorer/index.html.</span><span class="sxs-lookup"><span data-stu-id="91f62-149">For example, http://win1kw5649s.westus.cloudapp.azure.com:19080/Explorer/index.html.</span></span>

2. <span data-ttu-id="91f62-150">A Service Fabric Explorerben, navigáljon a **fabric://Voting** csomópontot a bal oldali fastruktúrában.</span><span class="sxs-lookup"><span data-stu-id="91f62-150">In Service Fabric Explorer, navigate to the **fabric://Voting** node in the treeview on the left-hand side.</span></span>

3. <span data-ttu-id="91f62-151">Kattintson a **művelet** jobb oldali gomb **Essentials** ablaktáblán, és válassza a **alkalmazás törlése**.</span><span class="sxs-lookup"><span data-stu-id="91f62-151">Click the **Action** button in the right-hand **Essentials** pane, and choose **Delete Application**.</span></span> <span data-ttu-id="91f62-152">Az alkalmazáspéldány, így a fürtben futó alkalmazás példánya nincs törlésének megerősítéséhez.</span><span class="sxs-lookup"><span data-stu-id="91f62-152">Confirm deleting the application instance, which removes the instance of our application running in the cluster.</span></span>

![A Service Fabric Explorerben alkalmazás törlése](./media/service-fabric-tutorial-deploy-app-to-party-cluster/delete-application.png)

## <a name="remove-the-application-type-from-a-cluster-using-service-fabric-explorer"></a><span data-ttu-id="91f62-154">Az alkalmazástípus eltávolítása egy fürtről Service Fabric Explorerrel</span><span class="sxs-lookup"><span data-stu-id="91f62-154">Remove the application type from a cluster using Service Fabric Explorer</span></span>
<span data-ttu-id="91f62-155">A Service Fabric-fürt, amely lehetővé teszi több példány és az alkalmazás fut a fürtön belül alkalmazástípusok alkalmazások üzemelnek.</span><span class="sxs-lookup"><span data-stu-id="91f62-155">Applications are deployed as application types in a Service Fabric cluster, which enables you to have multiple instances and versions of the application running within the cluster.</span></span> <span data-ttu-id="91f62-156">Miután eltávolította az alkalmazás futó példányát, azt is eltávolíthatja a típusát, a tisztítás, a központi telepítés befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="91f62-156">After having removed the running instance of our application, we can also remove the type, to complete the cleanup of the deployment.</span></span>

<span data-ttu-id="91f62-157">A Service Fabric az alkalmazásmodell kapcsolatos további információkért lásd: [egy alkalmazás a Service Fabric modell](service-fabric-application-model.md).</span><span class="sxs-lookup"><span data-stu-id="91f62-157">For more information about the application model in Service Fabric, see [Model an application in Service Fabric](service-fabric-application-model.md).</span></span>

1. <span data-ttu-id="91f62-158">Keresse meg a **VotingType** a TreeView vezérlő csomópont.</span><span class="sxs-lookup"><span data-stu-id="91f62-158">Navigate to the **VotingType** node in the treeview.</span></span>

2. <span data-ttu-id="91f62-159">Kattintson a **művelet** jobb oldali gomb **Essentials** ablaktáblán, és válassza a **Unprovision típus**.</span><span class="sxs-lookup"><span data-stu-id="91f62-159">Click the **Action** button in the right-hand **Essentials** pane, and choose **Unprovision Type**.</span></span> <span data-ttu-id="91f62-160">Ellenőrizze, hogy az alkalmazás típus leépítése.</span><span class="sxs-lookup"><span data-stu-id="91f62-160">Confirm unprovisioning the application type.</span></span>

![A Service Fabric Explorerben alkalmazástípus leépíteni a következőt:](./media/service-fabric-tutorial-deploy-app-to-party-cluster/unprovision-type.png)

<span data-ttu-id="91f62-162">Ezzel befejezte az oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="91f62-162">This concludes the tutorial.</span></span>

## <a name="next-steps"></a><span data-ttu-id="91f62-163">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="91f62-163">Next steps</span></span>
<span data-ttu-id="91f62-164">Ez az oktatóanyag bemutatta, hogyan végezheti el az alábbi műveleteket:</span><span class="sxs-lookup"><span data-stu-id="91f62-164">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="91f62-165">A távoli fürt Visual Studio alkalmazás központi telepítése</span><span class="sxs-lookup"><span data-stu-id="91f62-165">Deploy an application to a remote cluster using Visual Studio</span></span>
> * <span data-ttu-id="91f62-166">Az alkalmazás eltávolítása egy fürtről Service Fabric Explorerrel</span><span class="sxs-lookup"><span data-stu-id="91f62-166">Remove an application from a cluster using Service Fabric Explorer</span></span>

<span data-ttu-id="91f62-167">Előzetes következő oktatóanyagot:</span><span class="sxs-lookup"><span data-stu-id="91f62-167">Advance to the next tutorial:</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="91f62-168">Beállíthat folyamatos integrációt, a Visual Studio Team Services használatával</span><span class="sxs-lookup"><span data-stu-id="91f62-168">Set up continuous integration using Visual Studio Team Services</span></span>](service-fabric-tutorial-deploy-app-with-cicd-vsts.md)