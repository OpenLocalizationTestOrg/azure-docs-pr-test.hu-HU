---
title: "önálló Azure Service Fabric fürt aaaSet |} Microsoft Docs"
description: "Hozzon létre egy fejlesztési önálló fürtöt hello futó három csomópontja ugyanazon a számítógépen. A telepítés befejezése után készen áll a toocreate egy többgépes fürtben fog."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/06/2017
ms.author: dekapur
ms.openlocfilehash: e4d0ea9fc3b8475160bd8ed19fd3716463791cc5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-service-fabric-standalone-cluster"></a><span data-ttu-id="4153d-104">Az első önálló Service Fabric-fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="4153d-104">Create your first Service Fabric standalone cluster</span></span>
<span data-ttu-id="4153d-105">Létrehozhat különálló Service Fabric-fürt virtuális gépek vagy Windows Server 2012 R2 vagy Windows Server 2016-ot, a helyszíni rendszerű számítógépeket vagy hello felhőben.</span><span class="sxs-lookup"><span data-stu-id="4153d-105">You can create a Service Fabric standalone cluster on any virtual machines or computers running Windows Server 2012 R2 or Windows Server 2016, on-premises or in hello cloud.</span></span> <span data-ttu-id="4153d-106">A gyors üzembe helyezés segít toocreate fejlesztési önálló fürt csak néhány perc múlva.</span><span class="sxs-lookup"><span data-stu-id="4153d-106">This quickstart helps you toocreate a development standalone cluster in just a few minutes.</span></span>  <span data-ttu-id="4153d-107">Amikor végzett, egy egyetlen számítógépen futó, három csomópontot tartalmazó fürttel fog rendelkezni, amelyre appokat telepíthet.</span><span class="sxs-lookup"><span data-stu-id="4153d-107">When you're finished, you have a three-node cluster running on a single computer that you can deploy apps to.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="4153d-108">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="4153d-108">Before you begin</span></span>
<span data-ttu-id="4153d-109">A Service Fabric biztosít egy telepítő csomag toocreate Service Fabric-fürtök önálló.</span><span class="sxs-lookup"><span data-stu-id="4153d-109">Service Fabric provides a setup package toocreate Service Fabric standalone clusters.</span></span>  <span data-ttu-id="4153d-110">[Hello telepítő csomag](http://go.microsoft.com/fwlink/?LinkId=730690).</span><span class="sxs-lookup"><span data-stu-id="4153d-110">[Download hello setup package](http://go.microsoft.com/fwlink/?LinkId=730690).</span></span>  <span data-ttu-id="4153d-111">Bontsa ki a hello beállítása tooa mappájába hello számítógép vagy virtuális gép ahol beállítja hello fejlesztési fürtöt.</span><span class="sxs-lookup"><span data-stu-id="4153d-111">Unzip hello setup package tooa folder on hello computer or virtual machine where you are setting up hello development cluster.</span></span>  <span data-ttu-id="4153d-112">részletesen ismerteti a hello rendszerhez készült hello [Itt](service-fabric-cluster-standalone-package-contents.md).</span><span class="sxs-lookup"><span data-stu-id="4153d-112">hello contents of hello setup package are described in detail [here](service-fabric-cluster-standalone-package-contents.md).</span></span>

<span data-ttu-id="4153d-113">hello Fürtfelügyelő központi telepítését és konfigurálását hello fürt hello számítógépre rendszergazdai jogosultságokkal kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="4153d-113">hello cluster administrator deploying and configuring hello cluster must have administrator privileges on hello computer.</span></span> <span data-ttu-id="4153d-114">A Service Fabric tartományvezérlőn nem telepíthető.</span><span class="sxs-lookup"><span data-stu-id="4153d-114">You cannot install Service Fabric on a domain controller.</span></span>

## <a name="validate-hello-environment"></a><span data-ttu-id="4153d-115">Hello környezet ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="4153d-115">Validate hello environment</span></span>
<span data-ttu-id="4153d-116">Hello *TestConfiguration.ps1* hello önálló csomag parancsfájl szolgál egy ajánlott eljárásokat elemző eszköz toovalidate e fürt telepíthető egy adott környezetben.</span><span class="sxs-lookup"><span data-stu-id="4153d-116">hello *TestConfiguration.ps1* script in hello standalone package is used as a best practices analyzer toovalidate whether a cluster can be deployed on a given environment.</span></span> <span data-ttu-id="4153d-117">[Központi telepítés előkészítése](service-fabric-cluster-standalone-deployment-preparation.md) listák hello szükséges előfeltételek és környezeti követelmények.</span><span class="sxs-lookup"><span data-stu-id="4153d-117">[Deployment preparation](service-fabric-cluster-standalone-deployment-preparation.md) lists hello pre-requisites and environment requirements.</span></span> <span data-ttu-id="4153d-118">Hello parancsfájl tooverify üzemeltetéséhez hello fejlesztési fürtöt hozhat létre:</span><span class="sxs-lookup"><span data-stu-id="4153d-118">Run hello script tooverify if you can create hello development cluster:</span></span>

```powershell
.\TestConfiguration.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json
```
## <a name="create-hello-cluster"></a><span data-ttu-id="4153d-119">Hello fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="4153d-119">Create hello cluster</span></span>
<span data-ttu-id="4153d-120">Több fürt konfigurációs mintafájlok hello telepítőcsomagját együtt települnek.</span><span class="sxs-lookup"><span data-stu-id="4153d-120">Several sample cluster configuration files are installed with hello setup package.</span></span> <span data-ttu-id="4153d-121">*ClusterConfig.Unsecure.DevCluster.json* hello legegyszerűbb fürtkonfiguráció van: egy nem biztonságos, három csomópontos fürt egyetlen számítógépen futó.</span><span class="sxs-lookup"><span data-stu-id="4153d-121">*ClusterConfig.Unsecure.DevCluster.json* is hello simplest cluster configuration: an unsecure, three-node cluster running on a single computer.</span></span>  <span data-ttu-id="4153d-122">A többi konfigurációs fájl X.509 tanúsítványokkal vagy a Windows biztonsági szolgáltatásaival védett egy- vagy többgépes fürtöket ír le.</span><span class="sxs-lookup"><span data-stu-id="4153d-122">Other config files describe single or multi-machine clusters secured with X.509 certificates or Windows security.</span></span>  <span data-ttu-id="4153d-123">Nem kell toomodify hello alapértelmezett konfigurációs beállításokat a jelen oktatóanyag esetében, de nézze át a hello konfigurációs fájlban, és ismerkedjen meg hello-beállítások.</span><span class="sxs-lookup"><span data-stu-id="4153d-123">You don't need toomodify any of hello default config settings for this tutorial, but look through hello config file and get familiar with hello settings.</span></span>  <span data-ttu-id="4153d-124">Hello **csomópontok** a szakasz ismerteti a hello három hello fürt csomópontja: név, IP-cím, [típusú csomópont, a tartalék tartomány és a frissítési tartomány](service-fabric-cluster-manifest.md#nodes-on-the-cluster).</span><span class="sxs-lookup"><span data-stu-id="4153d-124">hello **nodes** section describes hello three nodes in hello cluster: name, IP address, [node type, fault domain, and upgrade domain](service-fabric-cluster-manifest.md#nodes-on-the-cluster).</span></span>  <span data-ttu-id="4153d-125">Hello **tulajdonságok** szakasz határozza meg a hello [biztonsági, megbízhatósági szint, diagnosztika gyűjtemény és csomópontok típusú](service-fabric-cluster-manifest.md#cluster-properties) hello fürthöz.</span><span class="sxs-lookup"><span data-stu-id="4153d-125">hello **properties** section defines hello [security, reliability level, diagnostics collection, and types of nodes](service-fabric-cluster-manifest.md#cluster-properties) for hello cluster.</span></span>

<span data-ttu-id="4153d-126">Ez a fürt nem biztonságos.</span><span class="sxs-lookup"><span data-stu-id="4153d-126">This cluster is unsecure.</span></span>  <span data-ttu-id="4153d-127">Bárki csatlakozhat hozzá névtelenül és végrehajthat kezelési műveleteket, ezért az üzemben lévő fürtöket mindig X.509 tanúsítványok vagy a Windows rendszerbiztonság használatával kell védeni.</span><span class="sxs-lookup"><span data-stu-id="4153d-127">Anyone can connect anonymously and perform management operations, so production clusters should always be secured using X.509 certificates or Windows security.</span></span>  <span data-ttu-id="4153d-128">Biztonsági csak konfigurálni kell a fürt létrehozásának idejét, és már nem lehetséges tooenable biztonsági hello fürt létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="4153d-128">Security is only configured at cluster creation time and it is not possible tooenable security after hello cluster is created.</span></span>  <span data-ttu-id="4153d-129">Olvasási [fürt biztonságos](service-fabric-cluster-security.md) további információk a Service Fabric-fürt biztonsági toolearn.</span><span class="sxs-lookup"><span data-stu-id="4153d-129">Read [Secure a cluster](service-fabric-cluster-security.md) toolearn more about Service Fabric cluster security.</span></span>  

<span data-ttu-id="4153d-130">toocreate hello három csomópontos fejlesztési fürtöt, futtassa a hello *CreateServiceFabricCluster.ps1* parancsfájl egy rendszergazda PowerShell-munkamenetben:</span><span class="sxs-lookup"><span data-stu-id="4153d-130">toocreate hello three-node development cluster, run hello *CreateServiceFabricCluster.ps1* script from an administrator PowerShell session:</span></span>

```powershell
.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json -AcceptEULA
```

<span data-ttu-id="4153d-131">Service Fabric-futtatókörnyezet csomag hello automatikusan letölti és a fürt létrehozásakor telepített.</span><span class="sxs-lookup"><span data-stu-id="4153d-131">hello Service Fabric runtime package is automatically downloaded and installed at time of cluster creation.</span></span>

## <a name="connect-toohello-cluster"></a><span data-ttu-id="4153d-132">Csatlakoztassa toohello fürtöt</span><span class="sxs-lookup"><span data-stu-id="4153d-132">Connect toohello cluster</span></span>
<span data-ttu-id="4153d-133">A három csomópontot tartalmazó fejlesztési fürt most már üzemel.</span><span class="sxs-lookup"><span data-stu-id="4153d-133">Your three-node development cluster is now running.</span></span> <span data-ttu-id="4153d-134">hello futásidejű hello ServiceFabric PowerShell-modul telepítve van.</span><span class="sxs-lookup"><span data-stu-id="4153d-134">hello ServiceFabric PowerShell module is installed with hello runtime.</span></span>  <span data-ttu-id="4153d-135">Ellenőrizheti a hello hello fürtben fut ugyanazon a számítógépen vagy egy távoli számítógépről hello Service Fabric-futtatókörnyezet.</span><span class="sxs-lookup"><span data-stu-id="4153d-135">You can verify that hello cluster is running from hello same computer or from a remote computer with hello Service Fabric runtime.</span></span>  <span data-ttu-id="4153d-136">Hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) parancsmag létesít kapcsolatot toohello fürt.</span><span class="sxs-lookup"><span data-stu-id="4153d-136">hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet establishes a connection toohello cluster.</span></span>   

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint localhost:19000
```
<span data-ttu-id="4153d-137">Lásd: [Connect tooa biztonságos fürt](service-fabric-connect-to-secure-cluster.md) csatlakozó tooa fürt más példákat.</span><span class="sxs-lookup"><span data-stu-id="4153d-137">See [Connect tooa secure cluster](service-fabric-connect-to-secure-cluster.md) for other examples of connecting tooa cluster.</span></span> <span data-ttu-id="4153d-138">Miután toohello fürthöz kapcsolódó, használja a hello [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) parancsmag toodisplay hello fürt és a állapot információt az egyes csomópontok csomópontok listáját.</span><span class="sxs-lookup"><span data-stu-id="4153d-138">After connecting toohello cluster, use hello [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) cmdlet toodisplay a list of nodes in hello cluster and status information for each node.</span></span> <span data-ttu-id="4153d-139">A **HealthState** tulajdonságnak *OK* értékűnek kell lennie minden csomópont esetében.</span><span class="sxs-lookup"><span data-stu-id="4153d-139">**HealthState** should be *OK* for each node.</span></span>

```powershell
PS C:\temp\Microsoft.Azure.ServiceFabric.WindowsServer> Get-ServiceFabricNode |Format-Table

NodeDeactivationInfo NodeName IpAddressOrFQDN NodeType  CodeVersion  ConfigVersion NodeStatus NodeUpTime NodeDownTime HealthState
-------------------- -------- --------------- --------  -----------  ------------- ---------- ---------- ------------ -----------
                     vm2      localhost       NodeType2 5.6.220.9494 0                     Up 00:03:38   00:00:00              OK
                     vm1      localhost       NodeType1 5.6.220.9494 0                     Up 00:03:38   00:00:00              OK
                     vm0      localhost       NodeType0 5.6.220.9494 0                     Up 00:02:43   00:00:00              OK
```

## <a name="visualize-hello-cluster-using-service-fabric-explorer"></a><span data-ttu-id="4153d-140">A Service Fabric Explorerrel hello fürt megjelenítése</span><span class="sxs-lookup"><span data-stu-id="4153d-140">Visualize hello cluster using Service Fabric explorer</span></span>
<span data-ttu-id="4153d-141">A [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) hatékony eszköz a fürtök megjelenítéséhez és az alkalmazások kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="4153d-141">[Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) is a good tool for visualizing your cluster and managing applications.</span></span>  <span data-ttu-id="4153d-142">Service Fabric Explorerben talál egy olyan szolgáltatás, hello fürt, amely egy böngésző segítségével túl útvonalon érhető el futtató[19080/Explorer](http://localhost:19080/Explorer).</span><span class="sxs-lookup"><span data-stu-id="4153d-142">Service Fabric Explorer is a service that runs in hello cluster, which you access using a browser by navigating too[http://localhost:19080/Explorer](http://localhost:19080/Explorer).</span></span> 

<span data-ttu-id="4153d-143">hello fürt irányítópult áttekintése a fürt, beleértve az alkalmazás és a csomópont állapotának összegzését.</span><span class="sxs-lookup"><span data-stu-id="4153d-143">hello cluster dashboard provides an overview of your cluster, including a summary of application and node health.</span></span> <span data-ttu-id="4153d-144">hello csomópont nézetben látható hello hello fürt fizikai elrendezését.</span><span class="sxs-lookup"><span data-stu-id="4153d-144">hello node view shows hello physical layout of hello cluster.</span></span> <span data-ttu-id="4153d-145">Az egyes csomópontoknál megtekintheti, hogy melyik alkalmazások kódja üzemel az adott csomóponton.</span><span class="sxs-lookup"><span data-stu-id="4153d-145">For a given node, you can inspect which applications have code deployed on that node.</span></span>

![Service Fabric Explorer][service-fabric-explorer]

## <a name="remove-hello-cluster"></a><span data-ttu-id="4153d-147">Hello fürt eltávolítása</span><span class="sxs-lookup"><span data-stu-id="4153d-147">Remove hello cluster</span></span>
<span data-ttu-id="4153d-148">tooremove egy fürthöz, futtassa a hello *RemoveServiceFabricCluster.ps1* PowerShell-parancsfájl hello csomag mappából, és adjon át hello elérési toohello JSON-konfigurációs fájlt.</span><span class="sxs-lookup"><span data-stu-id="4153d-148">tooremove a cluster, run hello *RemoveServiceFabricCluster.ps1* PowerShell script from hello package folder and pass in hello path toohello JSON configuration file.</span></span> <span data-ttu-id="4153d-149">Opcionálisan megadhat egy helyet hello napló hello törlését.</span><span class="sxs-lookup"><span data-stu-id="4153d-149">You can optionally specify a location for hello log of hello deletion.</span></span>

```powershell
# Removes Service Fabric cluster nodes from each computer in hello configuration file.
.\RemoveServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json -Force
```

<span data-ttu-id="4153d-150">tooremove hello Service Fabric-futtatókörnyezet hello a számítógépről, futtassa a következő PowerShell-parancsfájl hello csomag mappából hello.</span><span class="sxs-lookup"><span data-stu-id="4153d-150">tooremove hello Service Fabric runtime from hello computer, run hello following PowerShell script from hello package folder.</span></span>

```powershell
# Removes Service Fabric from hello current computer.
.\CleanFabric.ps1
```

## <a name="next-steps"></a><span data-ttu-id="4153d-151">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4153d-151">Next steps</span></span>
<span data-ttu-id="4153d-152">Most, hogy állította be a fejlesztési önálló fürthöz, próbálja meg a következő cikkek hello:</span><span class="sxs-lookup"><span data-stu-id="4153d-152">Now that you have set up a development standalone cluster, try hello following articles:</span></span>
* <span data-ttu-id="4153d-153">[Telepíthet egy többgépes önálló fürtöt](service-fabric-cluster-creation-for-windows-server.md), és engedélyezheti a védelmet.</span><span class="sxs-lookup"><span data-stu-id="4153d-153">[Set up a multi-machine standalone cluster](service-fabric-cluster-creation-for-windows-server.md) and enable security.</span></span>
* [<span data-ttu-id="4153d-154">Appokat helyezhet üzembe a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="4153d-154">Deploy apps using PowerShell</span></span>](service-fabric-deploy-remove-applications.md)

[service-fabric-explorer]: ./media/service-fabric-get-started-standalone-cluster/sfx.png
