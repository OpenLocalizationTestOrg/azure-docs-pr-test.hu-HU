---
title: "aaaAdd, vagy távolítsa el csomópontok tooa különálló Service Fabric-fürt |} Microsoft Docs"
description: "Ismerje meg, hogyan tooadd, vagy távolítsa el az csomópontok tooan Azure Service Fabric fürt egy fizikai gép vagy a helyszíni lehet a Windows Server rendszerű virtuális gép vagy a felhőben."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: bc6b8fc0-d2af-42f8-a164-58538be38d02
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 2/02/2017
ms.author: dekapur
ms.openlocfilehash: 1da908ad9840faa052e0b4021bc2d4ce732b02bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-or-remove-nodes-tooa-standalone-service-fabric-cluster-running-on-windows-server"></a><span data-ttu-id="24d4b-103">Hozzáadása vagy eltávolítása, csomópontok tooa különálló Service Fabric-fürt működő Windows Server</span><span class="sxs-lookup"><span data-stu-id="24d4b-103">Add or remove nodes tooa standalone Service Fabric cluster running on Windows Server</span></span>
<span data-ttu-id="24d4b-104">Miután [a különálló Service Fabric-fürt létrehozása a Windows Server gépeken](service-fabric-cluster-creation-for-windows-server.md), az üzleti igényeknek megfelelően változhat és előfordulhat, hogy tooadd kell, vagy távolítsa el a fürt a csomópontok tooyour.</span><span class="sxs-lookup"><span data-stu-id="24d4b-104">After you have [created your standalone Service Fabric cluster on Windows Server machines](service-fabric-cluster-creation-for-windows-server.md), your business needs may change and you might need tooadd or remove nodes tooyour cluster.</span></span> <span data-ttu-id="24d4b-105">Ez a cikk ismerteti a részletes lépéseket tooachieve ez.</span><span class="sxs-lookup"><span data-stu-id="24d4b-105">This article provides detailed steps tooachieve this.</span></span> <span data-ttu-id="24d4b-106">Vegye figyelembe, hogy hozzáadása csomópont funkció nem támogatott a helyi fejlesztési fürtök.</span><span class="sxs-lookup"><span data-stu-id="24d4b-106">Please note that add/remove node functionality is not supported in local development clusters.</span></span>

## <a name="add-nodes-tooyour-cluster"></a><span data-ttu-id="24d4b-107">Csomópontok tooyour fürt hozzáadása</span><span class="sxs-lookup"><span data-stu-id="24d4b-107">Add nodes tooyour cluster</span></span>
1. <span data-ttu-id="24d4b-108">Prepare hello tooadd tooyour fürt hello említett hello lépéseket követve kívánt virtuális gép/gép [Prepare hello gépek toomeet hello előfeltételei fürttelepítés](service-fabric-cluster-creation-for-windows-server.md) szakasz</span><span class="sxs-lookup"><span data-stu-id="24d4b-108">Prepare hello VM/machine you want tooadd tooyour cluster by following hello steps mentioned in hello [Prepare hello machines toomeet hello prerequisites for cluster deployment](service-fabric-cluster-creation-for-windows-server.md) section</span></span>
2. <span data-ttu-id="24d4b-109">Melyik tartalék tartomány és a frissítési tartomány a virtuális gép/gép áll további folyamatos tooadd azonosítása</span><span class="sxs-lookup"><span data-stu-id="24d4b-109">Identify which fault domain and upgrade domain you are going tooadd this VM/machine to</span></span>
3. <span data-ttu-id="24d4b-110">A távoli asztal (RDP) a virtuális gép vagy gépek, amelyet az tooadd toohello fürt hello</span><span class="sxs-lookup"><span data-stu-id="24d4b-110">Remote desktop (RDP) into hello VM/machine that you want tooadd toohello cluster</span></span>
4. <span data-ttu-id="24d4b-111">Másolás vagy [hello önálló csomag letöltése a Service Fabric Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690) virtuális gép vagy gépek toohello és csomagolja ki a hello csomag</span><span class="sxs-lookup"><span data-stu-id="24d4b-111">Copy or [download hello standalone package for Service Fabric for Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690) toohello VM/machine and unzip hello package</span></span>
5. <span data-ttu-id="24d4b-112">Powershell futtassa emelt szintű jogosultságokkal, és keresse meg a tömörítetlen csomag hello toohello helye</span><span class="sxs-lookup"><span data-stu-id="24d4b-112">Run Powershell with elevated privileges, and navigate toohello location of hello unzipped package</span></span>
6. <span data-ttu-id="24d4b-113">Futtassa a hello *AddNode.ps1* parancsfájl hello új csomópont tooadd leíró hello paraméterekkel.</span><span class="sxs-lookup"><span data-stu-id="24d4b-113">Run hello *AddNode.ps1* script with hello parameters describing hello new node tooadd.</span></span> <span data-ttu-id="24d4b-114">az alábbi példa hello VM5 nevű új csomópont hozzáadása, típussal, NodeType0 és IP-cím 182.17.34.52, UD1 és fd: / dc1/r0.</span><span class="sxs-lookup"><span data-stu-id="24d4b-114">hello example below adds a new node called VM5, with type NodeType0 and IP address 182.17.34.52, into UD1 and fd:/dc1/r0.</span></span> <span data-ttu-id="24d4b-115">Hello *ExistingClusterConnectionEndPoint* már van egy csomópont a csatlakozási végpont hello meglévő fürt, amely lehet hello IP-címe *bármely* hello fürt csomópontja.</span><span class="sxs-lookup"><span data-stu-id="24d4b-115">hello *ExistingClusterConnectionEndPoint* is a connection endpoint for a node already in hello existing cluster, which can be hello IP address of *any* node in hello cluster.</span></span>

    ```
    .\AddNode.ps1 -NodeName VM5 -NodeType NodeType0 -NodeIPAddressorFQDN 182.17.34.52 -ExistingClientConnectionEndpoint 182.17.34.50:19000 -UpgradeDomain UD1 -FaultDomain fd:/dc1/r0 -AcceptEULA
    ```
    <span data-ttu-id="24d4b-116">Miután hello parancsfájl befejezése után történik, ha új csomópont hello bővült hello futtatásával ellenőrizheti [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="24d4b-116">Once hello script finishes running, you can check if hello new node has been added by running hello [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) cmdlet.</span></span>

7. <span data-ttu-id="24d4b-117">tooensure konzisztencia hello fürt csomópontjai között, a felhasználónak kezdeményeznie kell a konfiguráció frissítése.</span><span class="sxs-lookup"><span data-stu-id="24d4b-117">tooensure consistency across different nodes in hello cluster, you must initiate a configuration upgrade.</span></span> <span data-ttu-id="24d4b-118">Futtatás [Get-ServiceFabricClusterConfiguration](/powershell/module/servicefabric/get-servicefabricclusterconfiguration?view=azureservicefabricps) tooget hello legfrissebb konfigurációs fájlt, és adja hozzá a hello az újonnan hozzáadott csomópont túl "Csomópont" szakasz.</span><span class="sxs-lookup"><span data-stu-id="24d4b-118">Run [Get-ServiceFabricClusterConfiguration](/powershell/module/servicefabric/get-servicefabricclusterconfiguration?view=azureservicefabricps) tooget hello latest configuration file and add hello newly added node too"Nodes" section.</span></span> <span data-ttu-id="24d4b-119">Célszerű tooalways rendelkezik hello legújabb fürtkonfiguráció érhető el, hogy kell-e a fürt hello tooredploy hello esetben ugyanazt a konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="24d4b-119">It is also recommended tooalways have hello latest cluster configuration available in hello case that you need tooredploy a cluster with hello same configuration.</span></span>

    ```
        {
            "nodeName": "vm5",
            "iPAddress": "182.17.34.52",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc1/r0",
            "upgradeDomain": "UD1"
        }
    ```
8. <span data-ttu-id="24d4b-120">Futtatás [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps) toobegin hello frissítését.</span><span class="sxs-lookup"><span data-stu-id="24d4b-120">Run [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps) toobegin hello upgrade.</span></span>

    ```
    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path tooConfiguration File>

    ```
    <span data-ttu-id="24d4b-121">Kísérheti hello hello frissítés a Service Fabric Explorerben talál.</span><span class="sxs-lookup"><span data-stu-id="24d4b-121">You can monitor hello progress of hello upgrade on Service Fabric Explorer.</span></span> <span data-ttu-id="24d4b-122">Alternatív megoldásként futtathatja [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)</span><span class="sxs-lookup"><span data-stu-id="24d4b-122">Alternatively, you can run [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)</span></span>

### <a name="add-nodes-tooclusters-configured-with-windows-security-using-gmsa"></a><span data-ttu-id="24d4b-123">Csoportosan felügyelt szolgáltatásfiókot használó Windows biztonsági konfigurált csomópontok tooclusters hozzáadása</span><span class="sxs-lookup"><span data-stu-id="24d4b-123">Add nodes tooclusters configured with Windows Security using gMSA</span></span>
<span data-ttu-id="24d4b-124">A fürtök konfigurált csoport által felügyelt szolgáltatás Account(gMSA) (https://technet.microsoft.com/library/hh831782.aspx) egy új csomópont felveheti egy konfigurációs frissítéssel:</span><span class="sxs-lookup"><span data-stu-id="24d4b-124">For clusters configured with Group Managed Service Account(gMSA)(https://technet.microsoft.com/library/hh831782.aspx), a new node can be added using a configuration upgrade:</span></span>
1. <span data-ttu-id="24d4b-125">Futtatás [Get-ServiceFabricClusterConfiguration](/powershell/module/servicefabric/get-servicefabricclusterconfiguration?view=azureservicefabricps) hello meglévő csomópontok egyikén tooget hello legfrissebb konfigurációs fájlt, és részletek megadása hello új csomópont kívánt tooadd hello "csomópont" szakaszában.</span><span class="sxs-lookup"><span data-stu-id="24d4b-125">Run [Get-ServiceFabricClusterConfiguration](/powershell/module/servicefabric/get-servicefabricclusterconfiguration?view=azureservicefabricps) on any of hello existing nodes tooget hello latest configuration file and add details about hello new node you want tooadd in hello "Nodes" section.</span></span> <span data-ttu-id="24d4b-126">Ellenőrizze, hogy új csomópont hello része a hello azonos csoport felügyelt fiók.</span><span class="sxs-lookup"><span data-stu-id="24d4b-126">Make sure hello new node is part of hello same group managed account.</span></span> <span data-ttu-id="24d4b-127">Ezt a fiókot minden számítógépen rendszergazdának kell lennie.</span><span class="sxs-lookup"><span data-stu-id="24d4b-127">This account should be an Administrator on all machines.</span></span>

    ```
        {
            "nodeName": "vm5",
            "iPAddress": "182.17.34.52",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc1/r0",
            "upgradeDomain": "UD1"
        }
    ```
2. <span data-ttu-id="24d4b-128">Futtatás [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps) toobegin hello frissítését.</span><span class="sxs-lookup"><span data-stu-id="24d4b-128">Run [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps) toobegin hello upgrade.</span></span>

    ```
    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path tooConfiguration File>
    ```
    <span data-ttu-id="24d4b-129">Kísérheti hello hello frissítés a Service Fabric Explorerben talál.</span><span class="sxs-lookup"><span data-stu-id="24d4b-129">You can monitor hello progress of hello upgrade on Service Fabric Explorer.</span></span> <span data-ttu-id="24d4b-130">Alternatív megoldásként futtathatja [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)</span><span class="sxs-lookup"><span data-stu-id="24d4b-130">Alternatively, you can run [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)</span></span>

### <a name="add-node-types-tooyour-cluster"></a><span data-ttu-id="24d4b-131">Csomópont típusok tooyour fürt hozzáadása</span><span class="sxs-lookup"><span data-stu-id="24d4b-131">Add node types tooyour cluster</span></span>
<span data-ttu-id="24d4b-132">Érdekében tooadd egy új típusú csomópont, módosítsa a konfigurációs tooinclude hello új csomóponttípus "NodeType tulajdonságok értéke" szakaszban a "Tulajdonságok" és egy konfigurációs megkezdéséhez használatával frissítse [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps).</span><span class="sxs-lookup"><span data-stu-id="24d4b-132">In order tooadd a new node type, modify your configuration tooinclude hello new node type in "NodeTypes" section under "Properties" and begin a configuration upgrade using [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps).</span></span> <span data-ttu-id="24d4b-133">Hello frissítése után ez a csomóponttípus az új csomópontok tooyour fürt adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="24d4b-133">Once hello upgrade completes, you can add new nodes tooyour cluster with this node type.</span></span>

## <a name="remove-nodes-from-your-cluster"></a><span data-ttu-id="24d4b-134">Csomópontok eltávolítása a fürtből</span><span class="sxs-lookup"><span data-stu-id="24d4b-134">Remove nodes from your cluster</span></span>
<span data-ttu-id="24d4b-135">A csomópont távolíthatók el a fürt egy konfigurációs frissítése a következő módon hello:</span><span class="sxs-lookup"><span data-stu-id="24d4b-135">A node can be removed from a cluster using a configuration upgrade, in hello following manner:</span></span>

1. <span data-ttu-id="24d4b-136">Futtatás [Get-ServiceFabricClusterConfiguration](/powershell/module/servicefabric/get-servicefabricclusterconfiguration?view=azureservicefabricps) tooget hello legfrissebb konfigurációs fájlban és *eltávolítása* hello csomópont "Csomópont" szakaszában.</span><span class="sxs-lookup"><span data-stu-id="24d4b-136">Run [Get-ServiceFabricClusterConfiguration](/powershell/module/servicefabric/get-servicefabricclusterconfiguration?view=azureservicefabricps) tooget hello latest configuration file and *remove* hello node from "Nodes" section.</span></span>
<span data-ttu-id="24d4b-137">Adja hozzá a hello "NodesToBeRemoved" paraméter túl "beállítása" szakasz "FabricSettings" szakaszon belül.</span><span class="sxs-lookup"><span data-stu-id="24d4b-137">Add hello "NodesToBeRemoved" parameter too"Setup" section inside "FabricSettings" section.</span></span> <span data-ttu-id="24d4b-138">"érték" Hello csomópontnevek eltávolított toobe igénylő csomópontok vesszővel elválasztott listája legyen.</span><span class="sxs-lookup"><span data-stu-id="24d4b-138">hello "value" should be a comma separated list of node names of nodes that need toobe removed.</span></span>

    ```
         "fabricSettings": [
            {
            "name": "Setup",
            "parameters": [
                {
                "name": "FabricDataRoot",
                "value": "C:\\ProgramData\\SF"
                },
                {
                "name": "FabricLogRoot",
                "value": "C:\\ProgramData\\SF\\Log"
                },
                {
                "name": "NodesToBeRemoved",
                "value": "vm0, vm1"
                }
            ]
            }
        ]
    ```
2. <span data-ttu-id="24d4b-139">Futtatás [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps) toobegin hello frissítését.</span><span class="sxs-lookup"><span data-stu-id="24d4b-139">Run [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps) toobegin hello upgrade.</span></span>

    ```
    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path tooConfiguration File>

    ```
    <span data-ttu-id="24d4b-140">Kísérheti hello hello frissítés a Service Fabric Explorerben talál.</span><span class="sxs-lookup"><span data-stu-id="24d4b-140">You can monitor hello progress of hello upgrade on Service Fabric Explorer.</span></span> <span data-ttu-id="24d4b-141">Alternatív megoldásként futtathatja [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)</span><span class="sxs-lookup"><span data-stu-id="24d4b-141">Alternatively, you can run [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)</span></span>

> [!NOTE]
> <span data-ttu-id="24d4b-142">Csomópontok eltávolítása több frissítés kezdeményezhet.</span><span class="sxs-lookup"><span data-stu-id="24d4b-142">Removal of nodes may initiate multiple upgrades.</span></span> <span data-ttu-id="24d4b-143">Egyes csomópontok lesznek megjelölve `IsSeedNode=”true”` címkét, és úgy azonosítható, hello fürt lekérdezésével manifest használatával `Get-ServiceFabricClusterManifest`.</span><span class="sxs-lookup"><span data-stu-id="24d4b-143">Some nodes are marked with `IsSeedNode=”true”` tag and can be identified by querying hello cluster manifest using `Get-ServiceFabricClusterManifest`.</span></span> <span data-ttu-id="24d4b-144">Ilyen csomópontok eltávolítása is tovább tarthat a többinél mivel hello kezdőérték csomópontok kell toobe helyezi át az ilyen helyzetekben.</span><span class="sxs-lookup"><span data-stu-id="24d4b-144">Removal of such nodes may take longer than others since hello seed nodes will have toobe moved around in such scenarios.</span></span> <span data-ttu-id="24d4b-145">hello fürt legalább 3 elsődleges típusú csomópontok kell fenntartani.</span><span class="sxs-lookup"><span data-stu-id="24d4b-145">hello cluster must maintain a minimum of 3 primary node type nodes.</span></span>
> 
> 

### <a name="remove-node-types-from-your-cluster"></a><span data-ttu-id="24d4b-146">Csomóponttípusok eltávolítása a fürtből</span><span class="sxs-lookup"><span data-stu-id="24d4b-146">Remove node types from your cluster</span></span>
<span data-ttu-id="24d4b-147">Mielőtt eltávolítaná a csomóponttípus, kérjük, ellenőrizze hivatkozzon a csomóponttípus hello csomópontok esetén.</span><span class="sxs-lookup"><span data-stu-id="24d4b-147">Before removing a node type, please double check if there are any nodes referencing hello node type.</span></span> <span data-ttu-id="24d4b-148">Megfelelő típusú hello a csomópont eltávolítása előtt távolítsa el ezeket a csomópontokat.</span><span class="sxs-lookup"><span data-stu-id="24d4b-148">Remove these nodes before removing hello corresponding node type.</span></span> <span data-ttu-id="24d4b-149">Minden megfelelő csomópontokat törlődik, miután hello NodeType eltávolítása hello fürtkonfiguráció, és egy konfigurációs megkezdéséhez használatával frissítse [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps).</span><span class="sxs-lookup"><span data-stu-id="24d4b-149">Once all corresponding nodes are removed, you can remove hello NodeType from hello cluster configuration and begin a configuration upgrade using [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps).</span></span>


### <a name="replace-primary-nodes-of-your-cluster"></a><span data-ttu-id="24d4b-150">Cserélje le a fürt elsődleges csomópontok</span><span class="sxs-lookup"><span data-stu-id="24d4b-150">Replace primary nodes of your cluster</span></span>
<span data-ttu-id="24d4b-151">hello cseréje az elsődleges csomópont helyett eltávolítása és a kötegek egymás után kell elvégezni egy csomópont.</span><span class="sxs-lookup"><span data-stu-id="24d4b-151">hello replacement of primary nodes should be performed one node after another, instead of removing and then adding in batches.</span></span>


## <a name="next-steps"></a><span data-ttu-id="24d4b-152">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="24d4b-152">Next steps</span></span>
* [<span data-ttu-id="24d4b-153">Önálló Windows-fürt konfigurációs beállításai</span><span class="sxs-lookup"><span data-stu-id="24d4b-153">Configuration settings for standalone Windows cluster</span></span>](service-fabric-cluster-manifest.md)
* [<span data-ttu-id="24d4b-154">Biztonságos Windows X509 használata önálló fürtben, tanúsítványok</span><span class="sxs-lookup"><span data-stu-id="24d4b-154">Secure a standalone cluster on Windows using X509 certificates</span></span>](service-fabric-windows-cluster-x509-security.md)
* [<span data-ttu-id="24d4b-155">Hozzon létre egy különálló Service Fabric-fürt Windowst futtató Azure virtuális gépeken</span><span class="sxs-lookup"><span data-stu-id="24d4b-155">Create a standalone Service Fabric cluster with Azure VMs running Windows</span></span>](service-fabric-cluster-creation-with-windows-azure-vms.md)

