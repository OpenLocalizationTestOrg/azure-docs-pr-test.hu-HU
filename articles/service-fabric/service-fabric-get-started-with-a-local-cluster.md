---
title: "aaaDeploy, és végezze el az Azure mikroszolgáltatások helyileg |} Microsoft Docs"
description: "Ismerje meg, hogyan tooset be egy helyi Service Fabric-fürt egy meglévő alkalmazás tooit, és frissítse az alkalmazást."
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
ms.openlocfilehash: e5f5adc9edb71433b2a7635e9d661ff92a4b18ec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-deploying-and-upgrading-applications-on-your-local-cluster"></a><span data-ttu-id="3694e-103">A helyi fürtön lévő alkalmazások üzembe helyezésének és frissítésének elsajátítása</span><span class="sxs-lookup"><span data-stu-id="3694e-103">Get started with deploying and upgrading applications on your local cluster</span></span>
<span data-ttu-id="3694e-104">hello Azure Service Fabric SDK teljes helyi fejlesztőkörnyezetet, melyekkel tartalmaz tooquickly telepítése és kezelése a helyi fürtön lévő alkalmazások első lépések.</span><span class="sxs-lookup"><span data-stu-id="3694e-104">hello Azure Service Fabric SDK includes a full local development environment that you can use tooquickly get started with deploying and managing applications on a local cluster.</span></span> <span data-ttu-id="3694e-105">Ebben a cikkben létrehoz egy helyi fürtöt, központi telepítése egy meglévő alkalmazás tooit, és frissítse az adott tooa új verziója, mindezt a Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="3694e-105">In this article, you create a local cluster, deploy an existing application tooit, and then upgrade that application tooa new version, all from Windows PowerShell.</span></span>

> [!NOTE]
> <span data-ttu-id="3694e-106">A cikk feltételezi, hogy Ön már [beállította a fejlesztőkörnyezetet](service-fabric-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="3694e-106">This article assumes that you already [set up your development environment](service-fabric-get-started.md).</span></span>
> 
> 

## <a name="create-a-local-cluster"></a><span data-ttu-id="3694e-107">Helyi fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="3694e-107">Create a local cluster</span></span>
<span data-ttu-id="3694e-108">A Service Fabric-fürt olyan hardver-erőforrások készletét képviseli, amelyeken alkalmazásokat helyezhet üzembe.</span><span class="sxs-lookup"><span data-stu-id="3694e-108">A Service Fabric cluster represents a set of hardware resources that you can deploy applications to.</span></span> <span data-ttu-id="3694e-109">Általában egy fürt áll bárhol az öt toomany több ezer gépek.</span><span class="sxs-lookup"><span data-stu-id="3694e-109">Typically, a cluster is made up of anywhere from five toomany thousands of machines.</span></span> <span data-ttu-id="3694e-110">Service Fabric SDK hello azonban egy gépen futtatható fürtkonfiguráció tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="3694e-110">However, hello Service Fabric SDK includes a cluster configuration that can run on a single machine.</span></span>

<span data-ttu-id="3694e-111">Fontos toounderstand, amely hello Service Fabric helyi fürtje nem emulátor vagy szimulátor.</span><span class="sxs-lookup"><span data-stu-id="3694e-111">It is important toounderstand that hello Service Fabric local cluster is not an emulator or simulator.</span></span> <span data-ttu-id="3694e-112">Az futtatása hello ugyanazt a platformkódot is megtalálható a többgépes fürtök.</span><span class="sxs-lookup"><span data-stu-id="3694e-112">It runs hello same platform code that is found on multi-machine clusters.</span></span> <span data-ttu-id="3694e-113">hello egyetlen különbség, hogy fut-e hello platform folyamatok, amelyek általában öt számítógép egy számítógépen vannak elosztva.</span><span class="sxs-lookup"><span data-stu-id="3694e-113">hello only difference is that it runs hello platform processes that are normally spread across five machines on one machine.</span></span>

<span data-ttu-id="3694e-114">hello SDK biztosít a helyi fürt két módon tooset: a Windows PowerShell parancsfájl és hello Local Cluster Manager rendszertálca alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="3694e-114">hello SDK provides two ways tooset up a local cluster: a Windows PowerShell script and hello Local Cluster Manager system tray app.</span></span> <span data-ttu-id="3694e-115">Ebben az oktatóanyagban hello PowerShell-parancsfájlt használjuk.</span><span class="sxs-lookup"><span data-stu-id="3694e-115">In this tutorial, we use hello PowerShell script.</span></span>

> [!NOTE]
> <span data-ttu-id="3694e-116">Ha a Visual Studióból egy alkalmazás üzembe helyezésével már létrehozott egy a helyi fürtöt, ezt a szakaszt kihagyhatja.</span><span class="sxs-lookup"><span data-stu-id="3694e-116">If you have already created a local cluster by deploying an application from Visual Studio, you can skip this section.</span></span>
> 
> 

1. <span data-ttu-id="3694e-117">Nyisson meg egy új PowerShell-ablakot rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="3694e-117">Launch a new PowerShell window as an administrator.</span></span>
2. <span data-ttu-id="3694e-118">Futtassa a fürtbeállítási parancsfájlt hello hello SDK mappából:</span><span class="sxs-lookup"><span data-stu-id="3694e-118">Run hello cluster setup script from hello SDK folder:</span></span>
   
    ```powershell
    & "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1"
    ```
   
    <span data-ttu-id="3694e-119">A fürt beállítása hosszabb időt vehet igénybe.</span><span class="sxs-lookup"><span data-stu-id="3694e-119">Cluster setup takes a few moments.</span></span> <span data-ttu-id="3694e-120">A beállítást követően a következőhöz hasonló kimenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="3694e-120">After setup is finished, you should see output similar to:</span></span>
   
    ![A fürtbeállítás kimenete][cluster-setup-success]
   
    <span data-ttu-id="3694e-122">Most már áll készen tootry alkalmazás tooyour fürt telepítése.</span><span class="sxs-lookup"><span data-stu-id="3694e-122">You are now ready tootry deploying an application tooyour cluster.</span></span>

## <a name="deploy-an-application"></a><span data-ttu-id="3694e-123">Alkalmazás üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="3694e-123">Deploy an application</span></span>
<span data-ttu-id="3694e-124">Service Fabric SDK hello alkalmazások létrehozásához szükséges keretrendszerek és fejlesztőeszközök gazdag készletét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="3694e-124">hello Service Fabric SDK includes a rich set of frameworks and developer tooling for creating applications.</span></span> <span data-ttu-id="3694e-125">Ha érdekli, hogyan toocreate alkalmazásokat a Visual Studio: tanulási [az első Service Fabric-alkalmazás létrehozása a Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md).</span><span class="sxs-lookup"><span data-stu-id="3694e-125">If you are interested in learning how toocreate applications in Visual Studio, see [Create your first Service Fabric application in Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md).</span></span>

<span data-ttu-id="3694e-126">Ebben az oktatóanyagban használhat egy meglévő mintaalkalmazást (a neve WordCount), hogy hello kezelést a hello platform összpontosíthat: telepítés, a figyelés és a frissítés.</span><span class="sxs-lookup"><span data-stu-id="3694e-126">In this tutorial, you use an existing sample application (called WordCount) so that you can focus on hello management aspects of hello platform: deployment, monitoring, and upgrade.</span></span>

1. <span data-ttu-id="3694e-127">Nyisson meg egy új PowerShell-ablakot rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="3694e-127">Launch a new PowerShell window as an administrator.</span></span>
2. <span data-ttu-id="3694e-128">Hello Service Fabric SDK PowerShell modul importálásához.</span><span class="sxs-lookup"><span data-stu-id="3694e-128">Import hello Service Fabric SDK PowerShell module.</span></span>
   
    ```powershell
    Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
    ```
3. <span data-ttu-id="3694e-129">Hozzon létre egy könyvtár toostore hello alkalmazás letöltése és telepítése, például: C:\ServiceFabric.</span><span class="sxs-lookup"><span data-stu-id="3694e-129">Create a directory toostore hello application that you download and deploy, such as C:\ServiceFabric.</span></span>
   
    ```powershell
    mkdir c:\ServiceFabric\
    cd c:\ServiceFabric\
    ```
4. <span data-ttu-id="3694e-130">[Töltse le a WordCount alkalmazás hello](http://aka.ms/servicefabric-wordcountapp) létrehozott toohello helyre.</span><span class="sxs-lookup"><span data-stu-id="3694e-130">[Download hello WordCount application](http://aka.ms/servicefabric-wordcountapp) toohello location you created.</span></span>  <span data-ttu-id="3694e-131">Megjegyzés: hello Microsoft Edge böngésző menti hello fájlt egy *.zip* bővítmény.</span><span class="sxs-lookup"><span data-stu-id="3694e-131">Note: hello Microsoft Edge browser saves hello file with a *.zip* extension.</span></span>  <span data-ttu-id="3694e-132">Hello fájlkiterjesztés túl módosítása*.sfpkg*.</span><span class="sxs-lookup"><span data-stu-id="3694e-132">Change hello file extension too*.sfpkg*.</span></span>
5. <span data-ttu-id="3694e-133">Csatlakozás a helyi fürt toohello:</span><span class="sxs-lookup"><span data-stu-id="3694e-133">Connect toohello local cluster:</span></span>
   
    ```powershell
    Connect-ServiceFabricCluster localhost:19000
    ```
6. <span data-ttu-id="3694e-134">Hozzon létre egy új alkalmazást hello SDK telepítési parancs használata egy nevet és egy elérési utat toohello alkalmazáscsomagot.</span><span class="sxs-lookup"><span data-stu-id="3694e-134">Create a new application using hello SDK's deployment command with a name and a path toohello application package.</span></span>
   
    ```powershell  
   Publish-NewServiceFabricApplication -ApplicationPackagePath c:\ServiceFabric\WordCountV1.sfpkg -ApplicationName "fabric:/WordCount"
    ```
   
    <span data-ttu-id="3694e-135">Ha minden megfelelően működik, a következő kimeneti hello kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="3694e-135">If all goes well, you should see hello following output:</span></span>
   
    ![Az alkalmazás toohello helyi fürt központi telepítése][deploy-app-to-local-cluster]
7. <span data-ttu-id="3694e-137">a művelet toosee hello alkalmazás hello böngésző indítása, és keresse meg a túl[http://localhost: 8081/WordCount/index.HTML](http://localhost:8081/wordcount/index.html).</span><span class="sxs-lookup"><span data-stu-id="3694e-137">toosee hello application in action, launch hello browser and navigate too[http://localhost:8081/wordcount/index.html](http://localhost:8081/wordcount/index.html).</span></span> <span data-ttu-id="3694e-138">A következőnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="3694e-138">You should see:</span></span>
   
    ![Az üzembe helyezett alkalmazás felhasználói felülete][deployed-app-ui]
   
    <span data-ttu-id="3694e-140">hello WordCount alkalmazás egyszerű.</span><span class="sxs-lookup"><span data-stu-id="3694e-140">hello WordCount application is simple.</span></span> <span data-ttu-id="3694e-141">Ez magában foglalja az ügyféloldali JavaScript code toogenerate véletlenszerű öt karakterből álló "szavak", amely majd továbbítódnak toohello alkalmazás ASP.NET Web API-n keresztül.</span><span class="sxs-lookup"><span data-stu-id="3694e-141">It includes client-side JavaScript code toogenerate random five-character "words", which are then relayed toohello application via ASP.NET Web API.</span></span> <span data-ttu-id="3694e-142">Az állapotalapú szolgáltatás nyomon követi az hello számlált szavak száma.</span><span class="sxs-lookup"><span data-stu-id="3694e-142">A stateful service tracks hello number of words counted.</span></span> <span data-ttu-id="3694e-143">Ezek particionáltak hello hello szó első karaktere alapján.</span><span class="sxs-lookup"><span data-stu-id="3694e-143">They are partitioned based on hello first character of hello word.</span></span> <span data-ttu-id="3694e-144">Hello forráskód hello WordCount alkalmazás hello található [klasszikus bevezetés minták](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Services/WordCount).</span><span class="sxs-lookup"><span data-stu-id="3694e-144">You can find hello source code for hello WordCount app in hello [classic getting started samples](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Services/WordCount).</span></span>
   
    <span data-ttu-id="3694e-145">hello üzembe helyezett alkalmazás négy partíciót tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="3694e-145">hello application that we deployed contains four partitions.</span></span> <span data-ttu-id="3694e-146">Így az A – G kezdetű szavak hello első partíció vannak tárolva, H – N kezdetű szavak vannak tárolva hello második partíció, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="3694e-146">So words beginning with A through G are stored in hello first partition, words beginning with H through N are stored in hello second partition, and so on.</span></span>

## <a name="view-application-details-and-status"></a><span data-ttu-id="3694e-147">Az alkalmazás részleteinek és állapotának megtekintése</span><span class="sxs-lookup"><span data-stu-id="3694e-147">View application details and status</span></span>
<span data-ttu-id="3694e-148">Most, hogy a Microsoft hello alkalmazást telepített, vizsgáljuk meg néhány hello alkalmazás adatait a PowerShellben.</span><span class="sxs-lookup"><span data-stu-id="3694e-148">Now that we have deployed hello application, let's look at some of hello app details in PowerShell.</span></span>

1. <span data-ttu-id="3694e-149">Hello fürt összes üzembe helyezett alkalmazásának lekérdezése:</span><span class="sxs-lookup"><span data-stu-id="3694e-149">Query all deployed applications on hello cluster:</span></span>
   
    ```powershell
    Get-ServiceFabricApplication
    ```
   
    <span data-ttu-id="3694e-150">Feltételezve, hogy csak telepített hello WordCount alkalmazást, akkor valami hasonló:</span><span class="sxs-lookup"><span data-stu-id="3694e-150">Assuming that you have only deployed hello WordCount app, you see something similar to:</span></span>
   
    ![Az összes üzembe helyezett alkalmazás lekérdezése a PowerShellben][ps-getsfapp]
2. <span data-ttu-id="3694e-152">Nyissa meg a toohello új szintre hello WordCount alkalmazásban szereplő hello készlete lekérdezésével.</span><span class="sxs-lookup"><span data-stu-id="3694e-152">Go toohello next level by querying hello set of services that are included in hello WordCount application.</span></span>
   
    ```powershell
    Get-ServiceFabricService -ApplicationName 'fabric:/WordCount'
    ```
   
    ![PowerShell hello alkalmazáshoz használható szolgáltatások listája][ps-getsfsvc]
   
    <span data-ttu-id="3694e-154">hello alkalmazás áll két szolgáltatást, hello előtér-webkiszolgáló és hello állapotalapú szolgáltatásból, amely hello szavakat kezeli.</span><span class="sxs-lookup"><span data-stu-id="3694e-154">hello application is made up of two services, hello web front end, and hello stateful service that manages hello words.</span></span>
3. <span data-ttu-id="3694e-155">Végül tekintse meg hello partícióinak listáját a WordCountService:</span><span class="sxs-lookup"><span data-stu-id="3694e-155">Finally, look at hello list of partitions for WordCountService:</span></span>
   
    ```powershell
    Get-ServiceFabricPartition 'fabric:/WordCount/WordCountService'
    ```
   
    ![Hello szolgáltatáspartíciók megtekintése a PowerShellben][ps-getsfpartitions]
   
    <span data-ttu-id="3694e-157">hello használt, például az összes Service Fabric PowerShell-parancsok parancsokat, érhetők el, hogy előfordulhat, hogy kapcsolódni, helyi vagy távoli fürthöz.</span><span class="sxs-lookup"><span data-stu-id="3694e-157">hello set of commands that you used, like all Service Fabric PowerShell commands, are available for any cluster that you might connect to, local or remote.</span></span>
   
    <span data-ttu-id="3694e-158">Egy vizuálisabban módon toointeract hello fürttel, használhatja hello webalapú Service Fabric Explorer eszközt túl navigálva[19080/Explorer](http://localhost:19080/Explorer) hello böngészőben.</span><span class="sxs-lookup"><span data-stu-id="3694e-158">For a more visual way toointeract with hello cluster, you can use hello web-based Service Fabric Explorer tool by navigating too[http://localhost:19080/Explorer](http://localhost:19080/Explorer) in hello browser.</span></span>
   
    ![Az alkalmazás részleteinek megtekintése a Service Fabric Explorerben][sfx-service-overview]
   
   > [!NOTE]
   > <span data-ttu-id="3694e-160">További információ a Service Fabric Explorer toolearn lásd [a fürt megjelenítése a Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="3694e-160">toolearn more about Service Fabric Explorer, see [Visualizing your cluster with Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).</span></span>
   > 
   > 

## <a name="upgrade-an-application"></a><span data-ttu-id="3694e-161">Alkalmazás frissítése</span><span class="sxs-lookup"><span data-stu-id="3694e-161">Upgrade an application</span></span>
<span data-ttu-id="3694e-162">A Service Fabric állásidő nélküli frissítéseket biztosít a hello hello alkalmazás állapotának figyelésével, miközben megtörténik a bevezetése hello fürtön.</span><span class="sxs-lookup"><span data-stu-id="3694e-162">Service Fabric provides no-downtime upgrades by monitoring hello health of hello application as it rolls out across hello cluster.</span></span> <span data-ttu-id="3694e-163">Hajtsa végre a hello WordCount alkalmazás frissítését.</span><span class="sxs-lookup"><span data-stu-id="3694e-163">Perform an upgrade of hello WordCount application.</span></span>

<span data-ttu-id="3694e-164">most már a hello alkalmazás új verziójának hello száma csak a magánhangzóval kezdődő szavakat.</span><span class="sxs-lookup"><span data-stu-id="3694e-164">hello new version of hello application now counts only words that begin with a vowel.</span></span> <span data-ttu-id="3694e-165">Hello frissítés bevezeti, mivel két változást hello alkalmazás viselkedését látható.</span><span class="sxs-lookup"><span data-stu-id="3694e-165">As hello upgrade rolls out, we see two changes in hello application's behavior.</span></span> <span data-ttu-id="3694e-166">Első lépésként hello sebesség, amellyel hello számok növekedésének le kell lassulnia, mivel kevesebb szót kell megszámolni.</span><span class="sxs-lookup"><span data-stu-id="3694e-166">First, hello rate at which hello count grows should slow, since fewer words are being counted.</span></span> <span data-ttu-id="3694e-167">Második mivel hello első partíció két magánhangzót tartalmaz (A és E), és az összes többi partíció mindegyike csak egyet, a számláló végül elindul toooutpace hello mások.</span><span class="sxs-lookup"><span data-stu-id="3694e-167">Second, since hello first partition has two vowels (A and E) and all other partitions contain only one each, its count should eventually start toooutpace hello others.</span></span>

1. <span data-ttu-id="3694e-168">[Hello WordCount 2-es csomag](http://aka.ms/servicefabric-wordcountappv2) toohello hello az 1-es csomag kezelőportálon ugyanazon a helyen.</span><span class="sxs-lookup"><span data-stu-id="3694e-168">[Download hello WordCount version 2 package](http://aka.ms/servicefabric-wordcountappv2) toohello same location where you downloaded hello version 1 package.</span></span>
2. <span data-ttu-id="3694e-169">Térjen vissza a tooyour PowerShell-ablakot, és hello SDK frissítési parancsával tooregister hello hello fürt új verziót használja.</span><span class="sxs-lookup"><span data-stu-id="3694e-169">Return tooyour PowerShell window and use hello SDK's upgrade command tooregister hello new version in hello cluster.</span></span> <span data-ttu-id="3694e-170">Majd lásson hello fabric: / WordCount alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="3694e-170">Then begin upgrading hello fabric:/WordCount application.</span></span>
   
    ```powershell
    Publish-UpgradedServiceFabricApplication -ApplicationPackagePath C:\ServiceFabric\WordCountV2.sfpkg -ApplicationName "fabric:/WordCount" -UpgradeParameters @{"FailureAction"="Rollback"; "UpgradeReplicaSetCheckTimeout"=1; "Monitored"=$true; "Force"=$true}
    ```
   
    <span data-ttu-id="3694e-171">Meg kell jelennie a PowerShell kimeneti, a frissítés hello hello következő kezdődik.</span><span class="sxs-lookup"><span data-stu-id="3694e-171">You should see hello following output in PowerShell as hello upgrade begins.</span></span>
   
    ![Folyamatban lévő frissítés a PowerShellben][ps-appupgradeprogress]
3. <span data-ttu-id="3694e-173">Míg hello frissítési eljárás, előfordulhat, hogy találja könnyebb toomonitor annak állapotát a Service Fabric Explorerből.</span><span class="sxs-lookup"><span data-stu-id="3694e-173">While hello upgrade is proceeding, you may find it easier toomonitor its status from Service Fabric Explorer.</span></span> <span data-ttu-id="3694e-174">Indítsa el a böngészőablakot, és keresse meg a túl[19080/Explorer](http://localhost:19080/Explorer).</span><span class="sxs-lookup"><span data-stu-id="3694e-174">Launch a browser window and navigate too[http://localhost:19080/Explorer](http://localhost:19080/Explorer).</span></span> <span data-ttu-id="3694e-175">Bontsa ki a **alkalmazások** hello bal oldali fában hello, majd válassza a **WordCount**, és végül **fabric: / WordCount**.</span><span class="sxs-lookup"><span data-stu-id="3694e-175">Expand **Applications** in hello tree on hello left, then choose **WordCount**, and finally **fabric:/WordCount**.</span></span> <span data-ttu-id="3694e-176">Hello alapvető erőforrások lapon látható hello állapot hello frissítési tartományain keresztül hello fürt frissítési tartományt.</span><span class="sxs-lookup"><span data-stu-id="3694e-176">In hello essentials tab, you see hello status of hello upgrade as it proceeds through hello cluster's upgrade domains.</span></span>
   
    ![A frissítés folyamata a Service Fabric Explorerben][sfx-upgradeprogress]
   
    <span data-ttu-id="3694e-178">Hello frissítés előrehalad az egyes tartományokon keresztül, állapot-ellenőrzési eredményeire során a rendszer elvégezte tooensure, amely hello alkalmazás megfelelően működik-e.</span><span class="sxs-lookup"><span data-stu-id="3694e-178">As hello upgrade proceeds through each domain, health checks are performed tooensure that hello application is behaving properly.</span></span>
4. <span data-ttu-id="3694e-179">Ha Újrafuttatja hello korábbi lekérdezése a szolgáltatások hello háló hello készlete: / WordCount alkalmazás, figyelje meg, hogy hello WordCountService verziója megváltozott, de hello wordcountwebservice verziója verziója nem történt meg:</span><span class="sxs-lookup"><span data-stu-id="3694e-179">If you rerun hello earlier query for hello set of services in hello fabric:/WordCount application, notice that hello WordCountService version changed but hello WordCountWebService version did not:</span></span>
   
    ```powershell
    Get-ServiceFabricService -ApplicationName 'fabric:/WordCount'
    ```
   
    ![Az alkalmazásszolgáltatások lekérdezése frissítés után][ps-getsfsvc-postupgrade]
   
    <span data-ttu-id="3694e-181">Ez a példa rávilágít arra, hogyan kezeli a Service Fabric az alkalmazásfrissítéseket.</span><span class="sxs-lookup"><span data-stu-id="3694e-181">This example highlights how Service Fabric manages application upgrades.</span></span> <span data-ttu-id="3694e-182">Csak hello beállítása (vagy a kód, illetve konfigurációs csomagokat ezekbe a szolgáltatásokba belül), amelyek megváltoztak, ezáltal gyorsabb és megbízhatóbb frissítésének hello folyamata érinti.</span><span class="sxs-lookup"><span data-stu-id="3694e-182">It touches only hello set of services (or code/configuration packages within those services) that have changed, which makes hello process of upgrading faster and more reliable.</span></span>
5. <span data-ttu-id="3694e-183">Végül térjen vissza a toohello böngésző tooobserve hello viselkedését hello új verziója.</span><span class="sxs-lookup"><span data-stu-id="3694e-183">Finally, return toohello browser tooobserve hello behavior of hello new application version.</span></span> <span data-ttu-id="3694e-184">Elvárás lassabban hello száma történik, és hello első partíció valamivel nagyobb hello kötet fejeződik be.</span><span class="sxs-lookup"><span data-stu-id="3694e-184">As expected, hello count progresses more slowly, and hello first partition ends up with slightly more of hello volume.</span></span>
   
    ![Nézet hello alkalmazás új verzióját hello hello böngészőben][deployed-app-ui-v2]

## <a name="cleaning-up"></a><span data-ttu-id="3694e-186">Takarítás</span><span class="sxs-lookup"><span data-stu-id="3694e-186">Cleaning up</span></span>
<span data-ttu-id="3694e-187">Lezárása előtt fontos, hogy a helyi fürt hello tooremember valós.</span><span class="sxs-lookup"><span data-stu-id="3694e-187">Before wrapping up, it's important tooremember that hello local cluster is real.</span></span> <span data-ttu-id="3694e-188">Alkalmazások továbbra is toorun hello háttérben, amíg el nem távolítja azokat.</span><span class="sxs-lookup"><span data-stu-id="3694e-188">Applications continue toorun in hello background until you remove them.</span></span>  <span data-ttu-id="3694e-189">Az alkalmazások hello természetétől függően a futó alkalmazások jelentős erőforrásokat a számítógépen is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="3694e-189">Depending on hello nature of your apps, a running app can take up significant resources on your machine.</span></span> <span data-ttu-id="3694e-190">Több beállítások toomanage alkalmazások és hello fürt rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="3694e-190">You have several options toomanage applications and hello cluster:</span></span>

1. <span data-ttu-id="3694e-191">tooremove az egyes alkalmazások és az összes azt tartozó adatok, futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="3694e-191">tooremove an individual application and all it's data, run hello following command:</span></span>
   
    ```powershell
    Unpublish-ServiceFabricApplication -ApplicationName "fabric:/WordCount"
    ```
   
    <span data-ttu-id="3694e-192">Vagy hello alkalmazás törlése a Service Fabric Explorer hello **műveletek** vagy hello helyi menüjének hello bal alkalmazás listanézetben.</span><span class="sxs-lookup"><span data-stu-id="3694e-192">Or, delete hello application from hello Service Fabric Explorer **ACTIONS** menu or hello context menu in hello left-hand application list view.</span></span>
   
    ![Alkalmazás törlése a Service Fabric Explorerrel][sfe-delete-application]
2. <span data-ttu-id="3694e-194">Ha töröl hello alkalmazás hello fürtből, regisztrációjának törlése 1.0.0 és a WordCount alkalmazás típusának hello 2.0.0 verziói.</span><span class="sxs-lookup"><span data-stu-id="3694e-194">After deleting hello application from hello cluster, unregister versions 1.0.0 and 2.0.0 of hello WordCount application type.</span></span> <span data-ttu-id="3694e-195">Törlés hello alkalmazáscsomagok, beleértve hello kód és a konfigurációt, a hello fürt lemezképtárolóhoz törli.</span><span class="sxs-lookup"><span data-stu-id="3694e-195">Deletion removes hello application packages, including hello code and configuration, from hello cluster's image store.</span></span>
   
    ```powershell
    Remove-ServiceFabricApplicationType -ApplicationTypeName WordCount -ApplicationTypeVersion 2.0.0
    Remove-ServiceFabricApplicationType -ApplicationTypeName WordCount -ApplicationTypeVersion 1.0.0
    ```
   
    <span data-ttu-id="3694e-196">Vagy a Service Fabric Explorerben válassza **Unprovision típus** hello alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="3694e-196">Or, in Service Fabric Explorer, choose **Unprovision Type** for hello application.</span></span>
3. <span data-ttu-id="3694e-197">tooshut hello fürt, de tartsa hello alkalmazásadatok és nyomkövetési adatok, kattintson a **helyi fürt leállítása** a hello rendszertálca alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="3694e-197">tooshut down hello cluster but keep hello application data and traces, click **Stop Local Cluster** in hello system tray app.</span></span>
4. <span data-ttu-id="3694e-198">toodelete hello fürt teljesen, kattintson a **helyi fürt eltávolítása** a hello rendszertálca alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="3694e-198">toodelete hello cluster entirely, click **Remove Local Cluster** in hello system tray app.</span></span> <span data-ttu-id="3694e-199">Ez a beállítás egy másik lassú üzembe helyezést hello legközelebb a Visual Studióban lenyomja az F5 okozza.</span><span class="sxs-lookup"><span data-stu-id="3694e-199">This option will result in another slow deployment hello next time you press F5 in Visual Studio.</span></span> <span data-ttu-id="3694e-200">Hello helyi fürt eltávolítása csak akkor, ha nem szeretné toouse azt egy kis ideig, vagy ha szüksége van-e tooreclaim erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="3694e-200">Remove hello local cluster only if you don't intend toouse it for some time or if you need tooreclaim resources.</span></span>

## <a name="one-node-and-five-node-cluster-mode"></a><span data-ttu-id="3694e-201">Egycsomópontos és ötcsomópontos fürt üzemmód</span><span class="sxs-lookup"><span data-stu-id="3694e-201">One-node and five-node cluster mode</span></span>
<span data-ttu-id="3694e-202">Alkalmazások fejlesztésekor gyakran hajtja végre a kódírás, a hibakeresés, a kódmódosítás és a hibakeresés gyors iterációit.</span><span class="sxs-lookup"><span data-stu-id="3694e-202">When developing applications, you often find yourself doing quick iterations of writing code, debugging, changing code, and debugging.</span></span> <span data-ttu-id="3694e-203">toohelp optimalizálása ezt a folyamatot, hello helyi fürt két módban futtatható: egycsomópontos vagy öt csomópontból.</span><span class="sxs-lookup"><span data-stu-id="3694e-203">toohelp optimize this process, hello local cluster can run in two modes: one-node or five-node.</span></span> <span data-ttu-id="3694e-204">Mindkét fürt üzemmódnak megvannak az előnyei.</span><span class="sxs-lookup"><span data-stu-id="3694e-204">Both cluster modes have their benefits.</span></span> <span data-ttu-id="3694e-205">Öt csomópontból álló fürt mód lehetővé teszi egy valódi fürttel toowork.</span><span class="sxs-lookup"><span data-stu-id="3694e-205">Five-node cluster mode enables you toowork with a real cluster.</span></span> <span data-ttu-id="3694e-206">Tesztelheti a feladatátvételi forgatókönyveket, a szolgáltatások több példányát és replikáját használhatja.</span><span class="sxs-lookup"><span data-stu-id="3694e-206">You can test failover scenarios, work with more instances and replicas of your services.</span></span> <span data-ttu-id="3694e-207">Optimalizált toodo gyors telepítéshez és nyilvántartási szolgáltatások, gyorsan érvényesítése kód megadása a Service Fabric-futtatókörnyezet hello toohelp egy csomópontos fürtre módja.</span><span class="sxs-lookup"><span data-stu-id="3694e-207">One-node cluster mode is optimized toodo quick deployment and registration of services, toohelp you quickly validate code using hello Service Fabric runtime.</span></span>

<span data-ttu-id="3694e-208">Sem az egycsomópontos sem az ötcsomópontos fürt üzemmód nem emulátor vagy szimulátor.</span><span class="sxs-lookup"><span data-stu-id="3694e-208">Neither one-node cluster or five-node cluster modes are an emulator or simulator.</span></span> <span data-ttu-id="3694e-209">hello helyi fejlesztési fürtök futtatásakor hello ugyanazt a platformkódot is megtalálható a többgépes fürtök.</span><span class="sxs-lookup"><span data-stu-id="3694e-209">hello local development cluster runs hello same platform code that is found on multi-machine clusters.</span></span>

> [!WARNING]
> <span data-ttu-id="3694e-210">Hello fürt mód módosításakor a jelenlegi fürthöz hello eltávolítják a rendszerből, és egy új fürt létrehozása.</span><span class="sxs-lookup"><span data-stu-id="3694e-210">When you change hello cluster mode, hello current cluster is removed from your system and a new cluster is created.</span></span> <span data-ttu-id="3694e-211">hello fürt hello adat törlődik a fürt mód módosításakor.</span><span class="sxs-lookup"><span data-stu-id="3694e-211">hello data stored in hello cluster is deleted when you change cluster mode.</span></span>
> 
> 

<span data-ttu-id="3694e-212">toochange hello mód tooone csomópontos fürt esetén jelölje be **kapcsoló fürt mód** a Service Fabric Local Cluster Manager hello.</span><span class="sxs-lookup"><span data-stu-id="3694e-212">toochange hello mode tooone-node cluster, select **Switch Cluster Mode** in hello Service Fabric Local Cluster Manager.</span></span>

![Fürt üzemmód átkapcsolása][switch-cluster-mode]

<span data-ttu-id="3694e-214">Vagy hello fürt módváltás PowerShell használatával:</span><span class="sxs-lookup"><span data-stu-id="3694e-214">Or, change hello cluster mode using PowerShell:</span></span>

1. <span data-ttu-id="3694e-215">Nyisson meg egy új PowerShell-ablakot rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="3694e-215">Launch a new PowerShell window as an administrator.</span></span>
2. <span data-ttu-id="3694e-216">Futtassa a fürtbeállítási parancsfájlt hello hello SDK mappából:</span><span class="sxs-lookup"><span data-stu-id="3694e-216">Run hello cluster setup script from hello SDK folder:</span></span>
   
    ```powershell
    & "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1" -CreateOneNodeCluster
    ```
   
    <span data-ttu-id="3694e-217">A fürt beállítása hosszabb időt vehet igénybe.</span><span class="sxs-lookup"><span data-stu-id="3694e-217">Cluster setup takes a few moments.</span></span> <span data-ttu-id="3694e-218">A beállítást követően a következőhöz hasonló kimenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="3694e-218">After setup is finished, you should see output similar to:</span></span>
   
    ![A fürtbeállítás kimenete][cluster-setup-success-1-node]

## <a name="next-steps"></a><span data-ttu-id="3694e-220">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3694e-220">Next steps</span></span>
* <span data-ttu-id="3694e-221">Most, hogy már üzembe helyezett és frissített néhány előre létrehozott alkalmazást, [megpróbálhatja felépíteni a saját alkalmazását a Visual Studióban](service-fabric-create-your-first-application-in-visual-studio.md).</span><span class="sxs-lookup"><span data-stu-id="3694e-221">Now that you have deployed and upgraded some pre-built applications, you can [try building your own in Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md).</span></span>
* <span data-ttu-id="3694e-222">Ebben a cikkben hello helyi fürtön végzett összes hello műveletet lehetne végrehajtani egy [Azure-fürttel](service-fabric-cluster-creation-via-portal.md) is.</span><span class="sxs-lookup"><span data-stu-id="3694e-222">All hello actions performed on hello local cluster in this article can be performed on an [Azure cluster](service-fabric-cluster-creation-via-portal.md) as well.</span></span>
* <span data-ttu-id="3694e-223">Ebben a cikkben végrehajtott frissítés hello alapszintű volt.</span><span class="sxs-lookup"><span data-stu-id="3694e-223">hello upgrade that we performed in this article was basic.</span></span> <span data-ttu-id="3694e-224">Lásd: hello [frissítési dokumentációjában](service-fabric-application-upgrade.md) toolearn hello hatékonyságát és rugalmasságát Service Fabric frissítéskezelésének többet.</span><span class="sxs-lookup"><span data-stu-id="3694e-224">See hello [upgrade documentation](service-fabric-application-upgrade.md) toolearn more about hello power and flexibility of Service Fabric upgrades.</span></span>

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
