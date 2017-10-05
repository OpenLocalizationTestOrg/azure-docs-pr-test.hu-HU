---
title: "Azure mikroszolgáltatások helyi telepítése és frissítése | Microsoft Docs"
description: "Megtudhatja, hogyan állítson be egy helyi Service Fabric-fürtöt, helyezzen üzembe rajta egy meglévő alkalmazást, majd frissítse az adott alkalmazást."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 60a1f6a5-5478-46c0-80a8-18fe62da17a8
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/13/2017
ms.author: ryanwi;mikhegn
ms.openlocfilehash: 359677972c7e1fa3f7435052021ddfae5b1ed85e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="get-started-with-deploying-and-upgrading-applications-on-your-local-cluster"></a><span data-ttu-id="0fc36-103">A helyi fürtön lévő alkalmazások üzembe helyezésének és frissítésének elsajátítása</span><span class="sxs-lookup"><span data-stu-id="0fc36-103">Get started with deploying and upgrading applications on your local cluster</span></span>
<span data-ttu-id="0fc36-104">Az Azure Service Fabric SDK teljes helyi fejlesztőkörnyezetet tartalmaz, amely segítségével gyorsan megismerkedhet a helyi fürtön lévő alkalmazások üzembe helyezésével és kezelésével.</span><span class="sxs-lookup"><span data-stu-id="0fc36-104">The Azure Service Fabric SDK includes a full local development environment that you can use to quickly get started with deploying and managing applications on a local cluster.</span></span> <span data-ttu-id="0fc36-105">Ebben a cikkben létrehoz egy helyi fürtöt, üzembe helyez rajta egy meglévő alkalmazást, majd új verzióra frissíti, és mindezt a Windows PowerShellből fogja elvégezni.</span><span class="sxs-lookup"><span data-stu-id="0fc36-105">In this article, you create a local cluster, deploy an existing application to it, and then upgrade that application to a new version, all from Windows PowerShell.</span></span>

> [!NOTE]
> <span data-ttu-id="0fc36-106">A cikk feltételezi, hogy Ön már [beállította a fejlesztőkörnyezetet](service-fabric-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="0fc36-106">This article assumes that you already [set up your development environment](service-fabric-get-started.md).</span></span>
> 
> 

## <a name="create-a-local-cluster"></a><span data-ttu-id="0fc36-107">Helyi fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="0fc36-107">Create a local cluster</span></span>
<span data-ttu-id="0fc36-108">A Service Fabric-fürt olyan hardver-erőforrások készletét képviseli, amelyeken alkalmazásokat helyezhet üzembe.</span><span class="sxs-lookup"><span data-stu-id="0fc36-108">A Service Fabric cluster represents a set of hardware resources that you can deploy applications to.</span></span> <span data-ttu-id="0fc36-109">Egy fürt általában tetszőleges számú gépből épül fel, a számuk öttől akár több ezerig terjedhet.</span><span class="sxs-lookup"><span data-stu-id="0fc36-109">Typically, a cluster is made up of anywhere from five to many thousands of machines.</span></span> <span data-ttu-id="0fc36-110">A Service Fabric azonban SDK olyan fürtkonfigurációt tartalmaz, amely csak egyetlen gépen futhat.</span><span class="sxs-lookup"><span data-stu-id="0fc36-110">However, the Service Fabric SDK includes a cluster configuration that can run on a single machine.</span></span>

<span data-ttu-id="0fc36-111">Fontos tisztában lenni azzal, hogy a Service Fabric helyi fürtje nem emulátor vagy szimulátor.</span><span class="sxs-lookup"><span data-stu-id="0fc36-111">It is important to understand that the Service Fabric local cluster is not an emulator or simulator.</span></span> <span data-ttu-id="0fc36-112">Ugyanazt a platformkódot futtatja, mint ami a több számítógépes fürtökön található.</span><span class="sxs-lookup"><span data-stu-id="0fc36-112">It runs the same platform code that is found on multi-machine clusters.</span></span> <span data-ttu-id="0fc36-113">Az egyetlen különbség, hogy az általában öt számítógép között elosztott platformfolyamatokat egyetlen számítógépen futtatja.</span><span class="sxs-lookup"><span data-stu-id="0fc36-113">The only difference is that it runs the platform processes that are normally spread across five machines on one machine.</span></span>

<span data-ttu-id="0fc36-114">Az SDK két lehetőséget biztosít a helyi fürt beállításához: egy Windows PowerShell-parancsfájlt és a Local Cluster Manager rendszertálca-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="0fc36-114">The SDK provides two ways to set up a local cluster: a Windows PowerShell script and the Local Cluster Manager system tray app.</span></span> <span data-ttu-id="0fc36-115">Ebben az oktatóanyagban a PowerShell-szkriptet használjuk.</span><span class="sxs-lookup"><span data-stu-id="0fc36-115">In this tutorial, we use the PowerShell script.</span></span>

> [!NOTE]
> <span data-ttu-id="0fc36-116">Ha a Visual Studióból egy alkalmazás üzembe helyezésével már létrehozott egy a helyi fürtöt, ezt a szakaszt kihagyhatja.</span><span class="sxs-lookup"><span data-stu-id="0fc36-116">If you have already created a local cluster by deploying an application from Visual Studio, you can skip this section.</span></span>
> 
> 

1. <span data-ttu-id="0fc36-117">Nyisson meg egy új PowerShell-ablakot rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="0fc36-117">Launch a new PowerShell window as an administrator.</span></span>
2. <span data-ttu-id="0fc36-118">Futtassa a fürtbeállítási parancsfájlt az SDK-mappából:</span><span class="sxs-lookup"><span data-stu-id="0fc36-118">Run the cluster setup script from the SDK folder:</span></span>
   
    ```powershell
    & "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1"
    ```
   
    <span data-ttu-id="0fc36-119">A fürt beállítása hosszabb időt vehet igénybe.</span><span class="sxs-lookup"><span data-stu-id="0fc36-119">Cluster setup takes a few moments.</span></span> <span data-ttu-id="0fc36-120">A beállítást követően a következőhöz hasonló kimenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="0fc36-120">After setup is finished, you should see output similar to:</span></span>
   
    ![A fürtbeállítás kimenete][cluster-setup-success]
   
    <span data-ttu-id="0fc36-122">Most már készen arra, hogy megpróbáljon üzembe helyezni egy alkalmazást a fürtön.</span><span class="sxs-lookup"><span data-stu-id="0fc36-122">You are now ready to try deploying an application to your cluster.</span></span>

## <a name="deploy-an-application"></a><span data-ttu-id="0fc36-123">Alkalmazás üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="0fc36-123">Deploy an application</span></span>
<span data-ttu-id="0fc36-124">A Service Fabric SDK az alkalmazások létrehozásához szükséges keretrendszerek és fejlesztőeszközök gazdag készletét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="0fc36-124">The Service Fabric SDK includes a rich set of frameworks and developer tooling for creating applications.</span></span> <span data-ttu-id="0fc36-125">Ha meg szeretné ismerni az alkalmazások létrehozási módját a Visual Studióban, tekintse meg [Az első Service Fabric-alkalmazás létrehozása a Visual Studióban](service-fabric-create-your-first-application-in-visual-studio.md) témakört.</span><span class="sxs-lookup"><span data-stu-id="0fc36-125">If you are interested in learning how to create applications in Visual Studio, see [Create your first Service Fabric application in Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md).</span></span>

<span data-ttu-id="0fc36-126">Ebben az oktatóanyag egy meglévő mintaalkalmazást (a neve WordCount) használ, így a platformkezelési szempontokra (üzembe helyezés, figyelés és frissítés) összpontosíthat.</span><span class="sxs-lookup"><span data-stu-id="0fc36-126">In this tutorial, you use an existing sample application (called WordCount) so that you can focus on the management aspects of the platform: deployment, monitoring, and upgrade.</span></span>

1. <span data-ttu-id="0fc36-127">Nyisson meg egy új PowerShell-ablakot rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="0fc36-127">Launch a new PowerShell window as an administrator.</span></span>
2. <span data-ttu-id="0fc36-128">Importálja a Service Fabric SDK PowerShell-modulját.</span><span class="sxs-lookup"><span data-stu-id="0fc36-128">Import the Service Fabric SDK PowerShell module.</span></span>
   
    ```powershell
    Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
    ```
3. <span data-ttu-id="0fc36-129">Hozzon létre a letöltendő és üzembe helyezendő alkalmazás tárolására szolgáló könyvtárat, például: C:\ServiceFabric.</span><span class="sxs-lookup"><span data-stu-id="0fc36-129">Create a directory to store the application that you download and deploy, such as C:\ServiceFabric.</span></span>
   
    ```powershell
    mkdir c:\ServiceFabric\
    cd c:\ServiceFabric\
    ```
4. <span data-ttu-id="0fc36-130">[Töltse le a WordCount alkalmazást](http://aka.ms/servicefabric-wordcountapp) a létrehozott helyre.</span><span class="sxs-lookup"><span data-stu-id="0fc36-130">[Download the WordCount application](http://aka.ms/servicefabric-wordcountapp) to the location you created.</span></span>  <span data-ttu-id="0fc36-131">Megjegyzés: A Microsoft Edge böngésző *.zip* kiterjesztéssel menti a fájlt.</span><span class="sxs-lookup"><span data-stu-id="0fc36-131">Note: the Microsoft Edge browser saves the file with a *.zip* extension.</span></span>  <span data-ttu-id="0fc36-132">Módosítsa a fájl kiterjesztését a következőre: *.sfpkg*.</span><span class="sxs-lookup"><span data-stu-id="0fc36-132">Change the file extension to *.sfpkg*.</span></span>
5. <span data-ttu-id="0fc36-133">Csatlakozzon a helyi fürthöz:</span><span class="sxs-lookup"><span data-stu-id="0fc36-133">Connect to the local cluster:</span></span>
   
    ```powershell
    Connect-ServiceFabricCluster localhost:19000
    ```
6. <span data-ttu-id="0fc36-134">Az SDK telepítési parancsának használatával hozzon létre egy új alkalmazást, adjon meg hozzá egy nevet és egy, az alkalmazáscsomagra mutató elérési utat.</span><span class="sxs-lookup"><span data-stu-id="0fc36-134">Create a new application using the SDK's deployment command with a name and a path to the application package.</span></span>
   
    ```powershell  
   Publish-NewServiceFabricApplication -ApplicationPackagePath c:\ServiceFabric\WordCountV1.sfpkg -ApplicationName "fabric:/WordCount"
    ```
   
    <span data-ttu-id="0fc36-135">Ha minden megfelelően működik, a következő kimenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="0fc36-135">If all goes well, you should see the following output:</span></span>
   
    ![Alkalmazás üzembe helyezése a helyi fürtön][deploy-app-to-local-cluster]
7. <span data-ttu-id="0fc36-137">Ha szeretné az alkalmazást működés közben megtekinteni, indítsa el a böngészőt, és navigáljon a következő webhelyre: [http://localhost:8081/wordcount/index.html](http://localhost:8081/wordcount/index.html).</span><span class="sxs-lookup"><span data-stu-id="0fc36-137">To see the application in action, launch the browser and navigate to [http://localhost:8081/wordcount/index.html](http://localhost:8081/wordcount/index.html).</span></span> <span data-ttu-id="0fc36-138">A következőnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="0fc36-138">You should see:</span></span>
   
    ![Az üzembe helyezett alkalmazás felhasználói felülete][deployed-app-ui]
   
    <span data-ttu-id="0fc36-140">A WordCount alkalmazás egyszerűen használható.</span><span class="sxs-lookup"><span data-stu-id="0fc36-140">The WordCount application is simple.</span></span> <span data-ttu-id="0fc36-141">Ügyféloldali JavaScript-kódot tartalmaz öt karakterből álló „szavak” véletlenszerű előállításához, amelyek aztán az ASP.NET Web API-n keresztül továbbítódnak az alkalmazásnak.</span><span class="sxs-lookup"><span data-stu-id="0fc36-141">It includes client-side JavaScript code to generate random five-character "words", which are then relayed to the application via ASP.NET Web API.</span></span> <span data-ttu-id="0fc36-142">Az állapotalapú szolgáltatások nyomon követik a szavak számának változását.</span><span class="sxs-lookup"><span data-stu-id="0fc36-142">A stateful service tracks the number of words counted.</span></span> <span data-ttu-id="0fc36-143">Ezek particionálása a szó első karaktere alapján történik.</span><span class="sxs-lookup"><span data-stu-id="0fc36-143">They are partitioned based on the first character of the word.</span></span> <span data-ttu-id="0fc36-144">A WordCount alkalmazás forráskódját a [klasszikus első lépéseket ismertető minták](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Services/WordCount) könyvtárában érheti el.</span><span class="sxs-lookup"><span data-stu-id="0fc36-144">You can find the source code for the WordCount app in the [classic getting started samples](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Services/WordCount).</span></span>
   
    <span data-ttu-id="0fc36-145">Az üzembe helyezett alkalmazás négy partíciót tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="0fc36-145">The application that we deployed contains four partitions.</span></span> <span data-ttu-id="0fc36-146">Az A–G kezdetű szavak az első partícióban, a H–N kezdetű szavak a második partícióban vannak tárolva és így tovább.</span><span class="sxs-lookup"><span data-stu-id="0fc36-146">So words beginning with A through G are stored in the first partition, words beginning with H through N are stored in the second partition, and so on.</span></span>

## <a name="view-application-details-and-status"></a><span data-ttu-id="0fc36-147">Az alkalmazás részleteinek és állapotának megtekintése</span><span class="sxs-lookup"><span data-stu-id="0fc36-147">View application details and status</span></span>
<span data-ttu-id="0fc36-148">Miután üzembe helyeztük az alkalmazást, nézzük meg a PowerShellben az alkalmazás egyes részleteit.</span><span class="sxs-lookup"><span data-stu-id="0fc36-148">Now that we have deployed the application, let's look at some of the app details in PowerShell.</span></span>

1. <span data-ttu-id="0fc36-149">A fürt összes üzembe helyezett alkalmazásának lekérdezése:</span><span class="sxs-lookup"><span data-stu-id="0fc36-149">Query all deployed applications on the cluster:</span></span>
   
    ```powershell
    Get-ServiceFabricApplication
    ```
   
    <span data-ttu-id="0fc36-150">Feltételezve, hogy kizárólag a WordCount alkalmazás lett üzembe helyezve, egy ehhez hasonló képernyőnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="0fc36-150">Assuming that you have only deployed the WordCount app, you see something similar to:</span></span>
   
    ![Az összes üzembe helyezett alkalmazás lekérdezése a PowerShellben][ps-getsfapp]
2. <span data-ttu-id="0fc36-152">Lépjen a következő szintre a WordCount alkalmazásban szereplő szolgáltatáskészlet lekérdezésével.</span><span class="sxs-lookup"><span data-stu-id="0fc36-152">Go to the next level by querying the set of services that are included in the WordCount application.</span></span>
   
    ```powershell
    Get-ServiceFabricService -ApplicationName 'fabric:/WordCount'
    ```
   
    ![Az alkalmazáshoz használható szolgáltatások listája a PowerShellben][ps-getsfsvc]
   
    <span data-ttu-id="0fc36-154">Az alkalmazás két szolgáltatásból áll: a webes kezelőfelületből és az állapotalapú szolgáltatásból, amely a szavakat kezeli.</span><span class="sxs-lookup"><span data-stu-id="0fc36-154">The application is made up of two services, the web front end, and the stateful service that manages the words.</span></span>
3. <span data-ttu-id="0fc36-155">Végül tekintse meg a WordCountService partícióinak listáját:</span><span class="sxs-lookup"><span data-stu-id="0fc36-155">Finally, look at the list of partitions for WordCountService:</span></span>
   
    ```powershell
    Get-ServiceFabricPartition 'fabric:/WordCount/WordCountService'
    ```
   
    ![A szolgáltatáspartíciók megtekintése a PowerShellben][ps-getsfpartitions]
   
    <span data-ttu-id="0fc36-157">Az Ön által használt parancsok készlete, például a Service Fabric PowerShell-parancsok, bármilyen (helyi vagy távoli) fürt számára elérhetők, amelyekhez kapcsolódni tud.</span><span class="sxs-lookup"><span data-stu-id="0fc36-157">The set of commands that you used, like all Service Fabric PowerShell commands, are available for any cluster that you might connect to, local or remote.</span></span>
   
    <span data-ttu-id="0fc36-158">Ha vizuálisabban szeretne kommunikálni a fürttel, használhatja a webalapú Service Fabric Explorer eszközt is, ha a böngészőben megnyitja a [http://localhost:19080/Explorer](http://localhost:19080/Explorer) weblapot.</span><span class="sxs-lookup"><span data-stu-id="0fc36-158">For a more visual way to interact with the cluster, you can use the web-based Service Fabric Explorer tool by navigating to [http://localhost:19080/Explorer](http://localhost:19080/Explorer) in the browser.</span></span>
   
    ![Az alkalmazás részleteinek megtekintése a Service Fabric Explorerben][sfx-service-overview]
   
   > [!NOTE]
   > <span data-ttu-id="0fc36-160">A Service Fabric Explorer használatával kapcsolatos további tudnivalókért lásd: [A fürt megjelenítése a Service Fabric Explorerrel](service-fabric-visualizing-your-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="0fc36-160">To learn more about Service Fabric Explorer, see [Visualizing your cluster with Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).</span></span>
   > 
   > 

## <a name="upgrade-an-application"></a><span data-ttu-id="0fc36-161">Alkalmazás frissítése</span><span class="sxs-lookup"><span data-stu-id="0fc36-161">Upgrade an application</span></span>
<span data-ttu-id="0fc36-162">A Service Fabric állásidő nélküli frissítéseket biztosít az alkalmazás állapotának figyelésével, miközben megtörténik a bevezetése az egész fürtön.</span><span class="sxs-lookup"><span data-stu-id="0fc36-162">Service Fabric provides no-downtime upgrades by monitoring the health of the application as it rolls out across the cluster.</span></span> <span data-ttu-id="0fc36-163">Végezze el a WordCount alkalmazás frissítését.</span><span class="sxs-lookup"><span data-stu-id="0fc36-163">Perform an upgrade of the WordCount application.</span></span>

<span data-ttu-id="0fc36-164">Az alkalmazás új verziója kizárólag a magánhangzóval kezdődő szavakat számolja össze.</span><span class="sxs-lookup"><span data-stu-id="0fc36-164">The new version of the application now counts only words that begin with a vowel.</span></span> <span data-ttu-id="0fc36-165">A frissítés előre haladtával két változást vehet észre az alkalmazás viselkedésében.</span><span class="sxs-lookup"><span data-stu-id="0fc36-165">As the upgrade rolls out, we see two changes in the application's behavior.</span></span> <span data-ttu-id="0fc36-166">Az első az a sebesség, amelynél a számok növekedésének le kell lassulnia, mivel kevesebb szót kell megszámolni.</span><span class="sxs-lookup"><span data-stu-id="0fc36-166">First, the rate at which the count grows should slow, since fewer words are being counted.</span></span> <span data-ttu-id="0fc36-167">A második, mivel az első partíció két magánhangzót tartalmaz (A és E), és az összes többi partíció mindegyike csak egyet, az első partíciónál a szám gyorsabban fog növekedni, mint a többinél.</span><span class="sxs-lookup"><span data-stu-id="0fc36-167">Second, since the first partition has two vowels (A and E) and all other partitions contain only one each, its count should eventually start to outpace the others.</span></span>

1. <span data-ttu-id="0fc36-168">[Töltse le a WordCount 2-es verziójának csomagját](http://aka.ms/servicefabric-wordcountappv2) ugyanarra a helyre, ahová az 1-es verzió csomagját letöltötte.</span><span class="sxs-lookup"><span data-stu-id="0fc36-168">[Download the WordCount version 2 package](http://aka.ms/servicefabric-wordcountappv2) to the same location where you downloaded the version 1 package.</span></span>
2. <span data-ttu-id="0fc36-169">Térjen vissza a PowerShell-ablakhoz, és az SDK frissítési parancsával regisztrálja az új verziót a fürtben.</span><span class="sxs-lookup"><span data-stu-id="0fc36-169">Return to your PowerShell window and use the SDK's upgrade command to register the new version in the cluster.</span></span> <span data-ttu-id="0fc36-170">Ezután indítsa el a fabric:/WordCount alkalmazás frissítését.</span><span class="sxs-lookup"><span data-stu-id="0fc36-170">Then begin upgrading the fabric:/WordCount application.</span></span>
   
    ```powershell
    Publish-UpgradedServiceFabricApplication -ApplicationPackagePath C:\ServiceFabric\WordCountV2.sfpkg -ApplicationName "fabric:/WordCount" -UpgradeParameters @{"FailureAction"="Rollback"; "UpgradeReplicaSetCheckTimeout"=1; "Monitored"=$true; "Force"=$true}
    ```
   
    <span data-ttu-id="0fc36-171">A frissítés indításakor a következő kimenetnek kell megjelennie a PowerShellben.</span><span class="sxs-lookup"><span data-stu-id="0fc36-171">You should see the following output in PowerShell as the upgrade begins.</span></span>
   
    ![Folyamatban lévő frissítés a PowerShellben][ps-appupgradeprogress]
3. <span data-ttu-id="0fc36-173">A frissítés folyamán lehet, hogy egyszerűbb az állapot figyelése a Service Fabric Explorerből.</span><span class="sxs-lookup"><span data-stu-id="0fc36-173">While the upgrade is proceeding, you may find it easier to monitor its status from Service Fabric Explorer.</span></span> <span data-ttu-id="0fc36-174">Nyisson meg egy böngészőablakot, és navigáljon a [http://localhost:19080/Explorer](http://localhost:19080/Explorer) weboldalra.</span><span class="sxs-lookup"><span data-stu-id="0fc36-174">Launch a browser window and navigate to [http://localhost:19080/Explorer](http://localhost:19080/Explorer).</span></span> <span data-ttu-id="0fc36-175">A bal oldali fában bontsa ki az **Alkalmazások** elemet, majd válassza ki a **WordCount**, végül a **fabric:/WordCount** elemet.</span><span class="sxs-lookup"><span data-stu-id="0fc36-175">Expand **Applications** in the tree on the left, then choose **WordCount**, and finally **fabric:/WordCount**.</span></span> <span data-ttu-id="0fc36-176">Az alapvető erőforrások lapján láthatja a frissítés állapotát a fürt frissítési tartományaiban.</span><span class="sxs-lookup"><span data-stu-id="0fc36-176">In the essentials tab, you see the status of the upgrade as it proceeds through the cluster's upgrade domains.</span></span>
   
    ![A frissítés folyamata a Service Fabric Explorerben][sfx-upgradeprogress]
   
    <span data-ttu-id="0fc36-178">Ahogy a frissítés előrehalad az egyes tartományokon keresztül, a rendszer mindig állapotellenőrzést végez annak érdekében, hogy biztosítsa az alkalmazás megfelelő viselkedését.</span><span class="sxs-lookup"><span data-stu-id="0fc36-178">As the upgrade proceeds through each domain, health checks are performed to ensure that the application is behaving properly.</span></span>
4. <span data-ttu-id="0fc36-179">Ha újra futtatja a korábbi, a fabric:/WordCount alkalmazás szolgáltatáskészletére irányuló lekérdezést, azt láthatja, hogy a WordCountService szolgáltatás verziószáma megváltozott, a WordCountWebService szolgáltatásé viszont nem:</span><span class="sxs-lookup"><span data-stu-id="0fc36-179">If you rerun the earlier query for the set of services in the fabric:/WordCount application, notice that the WordCountService version changed but the WordCountWebService version did not:</span></span>
   
    ```powershell
    Get-ServiceFabricService -ApplicationName 'fabric:/WordCount'
    ```
   
    ![Az alkalmazásszolgáltatások lekérdezése frissítés után][ps-getsfsvc-postupgrade]
   
    <span data-ttu-id="0fc36-181">Ez a példa rávilágít arra, hogyan kezeli a Service Fabric az alkalmazásfrissítéseket.</span><span class="sxs-lookup"><span data-stu-id="0fc36-181">This example highlights how Service Fabric manages application upgrades.</span></span> <span data-ttu-id="0fc36-182">Csak azokat a szolgáltatásokat (vagy a szolgáltatásokban található kódokat, illetve konfigurációs csomagokat) érinti, amelyek megváltoztak, ezáltal a frissítés folyamata sokkal gyorsabb és megbízhatóbb lesz.</span><span class="sxs-lookup"><span data-stu-id="0fc36-182">It touches only the set of services (or code/configuration packages within those services) that have changed, which makes the process of upgrading faster and more reliable.</span></span>
5. <span data-ttu-id="0fc36-183">Végül térjen vissza a böngészőhöz, és figyelje meg az új alkalmazásverzió működését.</span><span class="sxs-lookup"><span data-stu-id="0fc36-183">Finally, return to the browser to observe the behavior of the new application version.</span></span> <span data-ttu-id="0fc36-184">A számláló a vártnak megfelelően lassabban számol, és az első partíció valamivel nagyobb kötettel fejeződik be.</span><span class="sxs-lookup"><span data-stu-id="0fc36-184">As expected, the count progresses more slowly, and the first partition ends up with slightly more of the volume.</span></span>
   
    ![Az alkalmazás új verziójának megtekintése a böngészőben][deployed-app-ui-v2]

## <a name="cleaning-up"></a><span data-ttu-id="0fc36-186">Takarítás</span><span class="sxs-lookup"><span data-stu-id="0fc36-186">Cleaning up</span></span>
<span data-ttu-id="0fc36-187">A téma lezárása előtt fontos megjegyezni, hogy a helyi fürt valós.</span><span class="sxs-lookup"><span data-stu-id="0fc36-187">Before wrapping up, it's important to remember that the local cluster is real.</span></span> <span data-ttu-id="0fc36-188">Az alkalmazások futtatása azonban a háttérben tovább folytatódik mindaddig, amíg el nem távolítja azokat.</span><span class="sxs-lookup"><span data-stu-id="0fc36-188">Applications continue to run in the background until you remove them.</span></span>  <span data-ttu-id="0fc36-189">Az alkalmazások jellegétől függően azok jelentős erőforrásokat is igénybe vehetnek a gépen.</span><span class="sxs-lookup"><span data-stu-id="0fc36-189">Depending on the nature of your apps, a running app can take up significant resources on your machine.</span></span> <span data-ttu-id="0fc36-190">Az alkalmazás és a fürt kezelésére számos lehetősége van:</span><span class="sxs-lookup"><span data-stu-id="0fc36-190">You have several options to manage applications and the cluster:</span></span>

1. <span data-ttu-id="0fc36-191">Egyedi alkalmazások és azok adatainak eltávolításához futtassa a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="0fc36-191">To remove an individual application and all it's data, run the following command:</span></span>
   
    ```powershell
    Unpublish-ServiceFabricApplication -ApplicationName "fabric:/WordCount"
    ```
   
    <span data-ttu-id="0fc36-192">Másik megoldásként törölje az alkalmazást a Service Fabric Explorer **MŰVELETEK** menüjéből, vagy a bal oldali alkalmazáslista-nézet helyi menüjéből.</span><span class="sxs-lookup"><span data-stu-id="0fc36-192">Or, delete the application from the Service Fabric Explorer **ACTIONS** menu or the context menu in the left-hand application list view.</span></span>
   
    ![Alkalmazás törlése a Service Fabric Explorerrel][sfe-delete-application]
2. <span data-ttu-id="0fc36-194">Az alkalmazás a fürtről történő törlését követően törölje a WordCount alkalmazástípus 1.0.0-s és 2.0.0-s verziójának regisztrációját is.</span><span class="sxs-lookup"><span data-stu-id="0fc36-194">After deleting the application from the cluster, unregister versions 1.0.0 and 2.0.0 of the WordCount application type.</span></span> <span data-ttu-id="0fc36-195">A törléssel eltávolítja az alkalmazáscsomagokat a fürt lemezképtárolójából, beleértve a kódot és a konfigurációt is.</span><span class="sxs-lookup"><span data-stu-id="0fc36-195">Deletion removes the application packages, including the code and configuration, from the cluster's image store.</span></span>
   
    ```powershell
    Remove-ServiceFabricApplicationType -ApplicationTypeName WordCount -ApplicationTypeVersion 2.0.0
    Remove-ServiceFabricApplicationType -ApplicationTypeName WordCount -ApplicationTypeVersion 1.0.0
    ```
   
    <span data-ttu-id="0fc36-196">Esetleg a Service Fabric Explorerben válassza az **Unprovision Type** (Típus telepítésének visszavonása) lehetőséget az adott alkalmazás esetében.</span><span class="sxs-lookup"><span data-stu-id="0fc36-196">Or, in Service Fabric Explorer, choose **Unprovision Type** for the application.</span></span>
3. <span data-ttu-id="0fc36-197">A fürt leállításához, ugyanakkor az alkalmazás adatainak és nyomkövetéseinek megtartásához a rendszertálca alkalmazásban kattintson a **Stop Local Cluster** (Helyi fürt leállítása) parancsra.</span><span class="sxs-lookup"><span data-stu-id="0fc36-197">To shut down the cluster but keep the application data and traces, click **Stop Local Cluster** in the system tray app.</span></span>
4. <span data-ttu-id="0fc36-198">A fürt teljes törléséhez a rendszertálca alkalmazásban kattintson a **Remove Local Cluster** (Helyi fürt eltávolítása) parancsra.</span><span class="sxs-lookup"><span data-stu-id="0fc36-198">To delete the cluster entirely, click **Remove Local Cluster** in the system tray app.</span></span> <span data-ttu-id="0fc36-199">Ez a beállítás egy másik lassú üzembe helyezést fog eredményezni, amikor legközelebb a Visual Studióban lenyomja az F5 billentyűt.</span><span class="sxs-lookup"><span data-stu-id="0fc36-199">This option will result in another slow deployment the next time you press F5 in Visual Studio.</span></span> <span data-ttu-id="0fc36-200">A helyi fürtöt csak abban az esetben távolítsa el, ha egy ideig nem kívánja azt használni, vagy ha erőforrásokat kíván felszabadítani.</span><span class="sxs-lookup"><span data-stu-id="0fc36-200">Remove the local cluster only if you don't intend to use it for some time or if you need to reclaim resources.</span></span>

## <a name="one-node-and-five-node-cluster-mode"></a><span data-ttu-id="0fc36-201">Egycsomópontos és ötcsomópontos fürt üzemmód</span><span class="sxs-lookup"><span data-stu-id="0fc36-201">One-node and five-node cluster mode</span></span>
<span data-ttu-id="0fc36-202">Alkalmazások fejlesztésekor gyakran hajtja végre a kódírás, a hibakeresés, a kódmódosítás és a hibakeresés gyors iterációit.</span><span class="sxs-lookup"><span data-stu-id="0fc36-202">When developing applications, you often find yourself doing quick iterations of writing code, debugging, changing code, and debugging.</span></span> <span data-ttu-id="0fc36-203">A folyamat optimalizálása érdekében a helyi fürt két módban futhat: egycsomópontos és ötcsomópontos módban.</span><span class="sxs-lookup"><span data-stu-id="0fc36-203">To help optimize this process, the local cluster can run in two modes: one-node or five-node.</span></span> <span data-ttu-id="0fc36-204">Mindkét fürt üzemmódnak megvannak az előnyei.</span><span class="sxs-lookup"><span data-stu-id="0fc36-204">Both cluster modes have their benefits.</span></span> <span data-ttu-id="0fc36-205">Az ötcsomópontos fürt üzemmód lehetővé teszi, hogy egy valódi fürtön dolgozzon.</span><span class="sxs-lookup"><span data-stu-id="0fc36-205">Five-node cluster mode enables you to work with a real cluster.</span></span> <span data-ttu-id="0fc36-206">Tesztelheti a feladatátvételi forgatókönyveket, a szolgáltatások több példányát és replikáját használhatja.</span><span class="sxs-lookup"><span data-stu-id="0fc36-206">You can test failover scenarios, work with more instances and replicas of your services.</span></span> <span data-ttu-id="0fc36-207">Az egycsomópontos fürt üzemmód a gyors üzembe helyezésre és szolgáltatásregisztrációra van optimalizálva, így gyorsan ellenőrizheti a kódot a Service Fabric-futtatókörnyezettel.</span><span class="sxs-lookup"><span data-stu-id="0fc36-207">One-node cluster mode is optimized to do quick deployment and registration of services, to help you quickly validate code using the Service Fabric runtime.</span></span>

<span data-ttu-id="0fc36-208">Sem az egycsomópontos sem az ötcsomópontos fürt üzemmód nem emulátor vagy szimulátor.</span><span class="sxs-lookup"><span data-stu-id="0fc36-208">Neither one-node cluster or five-node cluster modes are an emulator or simulator.</span></span> <span data-ttu-id="0fc36-209">A helyi fejlesztési fürt ugyanazt a platformkódot futtatja, mint ami a többgépes fürtökön található.</span><span class="sxs-lookup"><span data-stu-id="0fc36-209">The local development cluster runs the same platform code that is found on multi-machine clusters.</span></span>

> [!WARNING]
> <span data-ttu-id="0fc36-210">Amikor módosítja a fürt üzemmódját, az aktuális fürtöt a rendszer eltávolítja, és egy új fürtöt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="0fc36-210">When you change the cluster mode, the current cluster is removed from your system and a new cluster is created.</span></span> <span data-ttu-id="0fc36-211">A fürtben tárolt adatok törlődnek a fürt üzemmódjának váltásakor.</span><span class="sxs-lookup"><span data-stu-id="0fc36-211">The data stored in the cluster is deleted when you change cluster mode.</span></span>
> 
> 

<span data-ttu-id="0fc36-212">Az üzemmód egycsomópontos fürtre módosításához válassza a **Fürt üzemmód átkapcsolása** lehetőséget a Service Fabric Local Cluster Manager alkalmazásában.</span><span class="sxs-lookup"><span data-stu-id="0fc36-212">To change the mode to one-node cluster, select **Switch Cluster Mode** in the Service Fabric Local Cluster Manager.</span></span>

![Fürt üzemmód átkapcsolása][switch-cluster-mode]

<span data-ttu-id="0fc36-214">Vagy módosítsa a fürt üzemmódját a PowerShell-lel:</span><span class="sxs-lookup"><span data-stu-id="0fc36-214">Or, change the cluster mode using PowerShell:</span></span>

1. <span data-ttu-id="0fc36-215">Nyisson meg egy új PowerShell-ablakot rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="0fc36-215">Launch a new PowerShell window as an administrator.</span></span>
2. <span data-ttu-id="0fc36-216">Futtassa a fürtbeállítási parancsfájlt az SDK-mappából:</span><span class="sxs-lookup"><span data-stu-id="0fc36-216">Run the cluster setup script from the SDK folder:</span></span>
   
    ```powershell
    & "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1" -CreateOneNodeCluster
    ```
   
    <span data-ttu-id="0fc36-217">A fürt beállítása hosszabb időt vehet igénybe.</span><span class="sxs-lookup"><span data-stu-id="0fc36-217">Cluster setup takes a few moments.</span></span> <span data-ttu-id="0fc36-218">A beállítást követően a következőhöz hasonló kimenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="0fc36-218">After setup is finished, you should see output similar to:</span></span>
   
    ![A fürtbeállítás kimenete][cluster-setup-success-1-node]

## <a name="next-steps"></a><span data-ttu-id="0fc36-220">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0fc36-220">Next steps</span></span>
* <span data-ttu-id="0fc36-221">Most, hogy már üzembe helyezett és frissített néhány előre létrehozott alkalmazást, [megpróbálhatja felépíteni a saját alkalmazását a Visual Studióban](service-fabric-create-your-first-application-in-visual-studio.md).</span><span class="sxs-lookup"><span data-stu-id="0fc36-221">Now that you have deployed and upgraded some pre-built applications, you can [try building your own in Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md).</span></span>
* <span data-ttu-id="0fc36-222">A jelen cikkben a helyi fürttel elvégzett összes művelet elvégezhető egy [Azure-fürttel](service-fabric-cluster-creation-via-portal.md) is.</span><span class="sxs-lookup"><span data-stu-id="0fc36-222">All the actions performed on the local cluster in this article can be performed on an [Azure cluster](service-fabric-cluster-creation-via-portal.md) as well.</span></span>
* <span data-ttu-id="0fc36-223">A jelen cikkben végrehajtott frissítés alapszintű volt.</span><span class="sxs-lookup"><span data-stu-id="0fc36-223">The upgrade that we performed in this article was basic.</span></span> <span data-ttu-id="0fc36-224">A Service Fabric frissítéskezelésének hatékonyságát és rugalmasságát a [frissítési dokumentációból](service-fabric-application-upgrade.md) ismerheti meg.</span><span class="sxs-lookup"><span data-stu-id="0fc36-224">See the [upgrade documentation](service-fabric-application-upgrade.md) to learn more about the power and flexibility of Service Fabric upgrades.</span></span>

<!-- Images -->

[cluster-setup-success]: ./media/service-fabric-get-started-with-a-local-cluster/LocalClusterSetup.png
[extracted-app-package]: ./media/service-fabric-get-started-with-a-local-cluster/ExtractedAppPackage.png
[deploy-app-to-local-cluster]: ./media/service-fabric-get-started-with-a-local-cluster/DeployAppToLocalCluster.png
[deployed-app-ui]: ./media/service-fabric-get-started-with-a-local-cluster/DeployedAppUI-v1.png
[deployed-app-ui-v2]: ./media/service-fabric-get-started-with-a-local-cluster/DeployedAppUI-PostUpgrade.png
[sfx-app-instance]: ./media/service-fabric-get-started-with-a-local-cluster/SfxAppInstance.png
[sfx-two-app-instances-different-partitions]: ./media/service-fabric-get-started-with-a-local-cluster/SfxTwoAppInstances-DifferentPartitionCount.png
[ps-getsfapp]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFApp.png
[ps-getsfsvc]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFSvc.png
[ps-getsfpartitions]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFPartitions.png
[ps-appupgradeprogress]: ./media/service-fabric-get-started-with-a-local-cluster/PS-AppUpgradeProgress.png
[ps-getsfsvc-postupgrade]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFSvc-PostUpgrade.png
[sfx-upgradeprogress]: ./media/service-fabric-get-started-with-a-local-cluster/SfxUpgradeOverview.png
[sfx-service-overview]: ./media/service-fabric-get-started-with-a-local-cluster/sfx-service-overview.png
[sfe-delete-application]: ./media/service-fabric-get-started-with-a-local-cluster/sfe-delete-application.png
[cluster-setup-success-1-node]: ./media/service-fabric-get-started-with-a-local-cluster/cluster-setup-success-1-node.png
[switch-cluster-mode]: ./media/service-fabric-get-started-with-a-local-cluster/switch-cluster-mode.png
