---
title: "Különálló Service Fabric-fürt a csomópontok hozzáadásához és eltávolításához |} Microsoft Docs"
description: "Útmutató egy fizikai gép vagy a helyszíni lehet a Windows Server rendszerű virtuális gép vagy a felhőben az Azure Service Fabric-fürt a csomópontok hozzáadásához és eltávolításához."
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
ms.openlocfilehash: 9c6035e97de38ff63ef074109afd9f3c7484f828
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="add-or-remove-nodes-to-a-standalone-service-fabric-cluster-running-on-windows-server"></a><span data-ttu-id="95cfa-103">A Windows Server rendszert futtató önálló Service Fabric-fürt a csomópontok hozzáadásához és eltávolításához</span><span class="sxs-lookup"><span data-stu-id="95cfa-103">Add or remove nodes to a standalone Service Fabric cluster running on Windows Server</span></span>
<span data-ttu-id="95cfa-104">Miután [a különálló Service Fabric-fürt létrehozása a Windows Server gépeken](service-fabric-cluster-creation-for-windows-server.md), az üzleti igényeknek megfelelően változhat, és szükség lehet az a fürt a csomópontok hozzáadásához és eltávolításához.</span><span class="sxs-lookup"><span data-stu-id="95cfa-104">After you have [created your standalone Service Fabric cluster on Windows Server machines](service-fabric-cluster-creation-for-windows-server.md), your business needs may change and you might need to add or remove nodes to your cluster.</span></span> <span data-ttu-id="95cfa-105">Ez a cikk részletes lépéseit ennek érdekében.</span><span class="sxs-lookup"><span data-stu-id="95cfa-105">This article provides detailed steps to achieve this.</span></span> <span data-ttu-id="95cfa-106">Vegye figyelembe, hogy hozzáadása csomópont funkció nem támogatott a helyi fejlesztési fürtök.</span><span class="sxs-lookup"><span data-stu-id="95cfa-106">Please note that add/remove node functionality is not supported in local development clusters.</span></span>

## <a name="add-nodes-to-your-cluster"></a><span data-ttu-id="95cfa-107">Csomópontok hozzáadása a fürthöz</span><span class="sxs-lookup"><span data-stu-id="95cfa-107">Add nodes to your cluster</span></span>
1. <span data-ttu-id="95cfa-108">Az említett lépések a fürthöz hozzáadni kívánt virtuális gép/gép előkészítése a [készítse elő a fürtöt tartalmazó környezetben az Előfeltételek teljesítése érdekében a gépek](service-fabric-cluster-creation-for-windows-server.md) szakasz</span><span class="sxs-lookup"><span data-stu-id="95cfa-108">Prepare the VM/machine you want to add to your cluster by following the steps mentioned in the [Prepare the machines to meet the prerequisites for cluster deployment](service-fabric-cluster-creation-for-windows-server.md) section</span></span>
2. <span data-ttu-id="95cfa-109">Melyik tartalék tartomány és a frissítési tartomány hozzáadása a virtuális gép/gép kívánja azonosítása</span><span class="sxs-lookup"><span data-stu-id="95cfa-109">Identify which fault domain and upgrade domain you are going to add this VM/machine to</span></span>
3. <span data-ttu-id="95cfa-110">A távoli asztal (RDP) a fürthöz hozzáadni kívánt virtuális gép vagy gépek be</span><span class="sxs-lookup"><span data-stu-id="95cfa-110">Remote desktop (RDP) into the VM/machine that you want to add to the cluster</span></span>
4. <span data-ttu-id="95cfa-111">Másolás vagy [töltse le az önálló csomag a Service Fabric Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690) a virtuális géphez, és bontsa ki a csomagot</span><span class="sxs-lookup"><span data-stu-id="95cfa-111">Copy or [download the standalone package for Service Fabric for Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690) to the VM/machine and unzip the package</span></span>
5. <span data-ttu-id="95cfa-112">Powershell futtassa emelt szintű jogosultságokkal, és keresse meg a helyet a tömörítetlen csomag</span><span class="sxs-lookup"><span data-stu-id="95cfa-112">Run Powershell with elevated privileges, and navigate to the location of the unzipped package</span></span>
6. <span data-ttu-id="95cfa-113">Futtassa a *AddNode.ps1* parancsfájl hozzáadása az új csomópont leíró paraméterekkel.</span><span class="sxs-lookup"><span data-stu-id="95cfa-113">Run the *AddNode.ps1* script with the parameters describing the new node to add.</span></span> <span data-ttu-id="95cfa-114">Az alábbi példában egy VM5 nevű új csomópont hozzáadása, típussal, NodeType0 és IP-cím 182.17.34.52, UD1 és fd: / dc1/r0.</span><span class="sxs-lookup"><span data-stu-id="95cfa-114">The example below adds a new node called VM5, with type NodeType0 and IP address 182.17.34.52, into UD1 and fd:/dc1/r0.</span></span> <span data-ttu-id="95cfa-115">A *ExistingClusterConnectionEndPoint* van a kapcsolat végpontja már a meglévő fürt csomópontja, amely az IP-címe lehet *bármely* a fürt csomópontja.</span><span class="sxs-lookup"><span data-stu-id="95cfa-115">The *ExistingClusterConnectionEndPoint* is a connection endpoint for a node already in the existing cluster, which can be the IP address of *any* node in the cluster.</span></span>

    ```
    .\AddNode.ps1 -NodeName VM5 -NodeType NodeType0 -NodeIPAddressorFQDN 182.17.34.52 -ExistingClientConnectionEndpoint 182.17.34.50:19000 -UpgradeDomain UD1 -FaultDomain fd:/dc1/r0 -AcceptEULA
    ```
    <span data-ttu-id="95cfa-116">Miután a parancsfájl befejezése után történik, ha az új csomópont bővült futtatásával ellenőrizheti a [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="95cfa-116">Once the script finishes running, you can check if the new node has been added by running the [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) cmdlet.</span></span>

7. <span data-ttu-id="95cfa-117">Konzisztencia biztosításához a fürt csomópontjai között, a felhasználónak kezdeményeznie kell a konfiguráció frissítése.</span><span class="sxs-lookup"><span data-stu-id="95cfa-117">To ensure consistency across different nodes in the cluster, you must initiate a configuration upgrade.</span></span> <span data-ttu-id="95cfa-118">Futtatás [Get-ServiceFabricClusterConfiguration](/powershell/module/servicefabric/get-servicefabricclusterconfiguration?view=azureservicefabricps) a legfrissebb konfigurációs fájlt, és adja hozzá az újonnan hozzáadott csomópontot "Csomópont" szakasz.</span><span class="sxs-lookup"><span data-stu-id="95cfa-118">Run [Get-ServiceFabricClusterConfiguration](/powershell/module/servicefabric/get-servicefabricclusterconfiguration?view=azureservicefabricps) to get the latest configuration file and add the newly added node to "Nodes" section.</span></span> <span data-ttu-id="95cfa-119">Emellett javasoljuk, hogy mindig a legújabb elérhető abban az esetben újra kell redploy ugyanazt a konfigurációt a fürt fürt konfigurációval rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="95cfa-119">It is also recommended to always have the latest cluster configuration available in the case that you need to redploy a cluster with the same configuration.</span></span>

    ```
        {
            "nodeName": "vm5",
            "iPAddress": "182.17.34.52",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc1/r0",
            "upgradeDomain": "UD1"
        }
    ```
8. <span data-ttu-id="95cfa-120">Futtatás [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps) a frissítésének megkezdésére.</span><span class="sxs-lookup"><span data-stu-id="95cfa-120">Run [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps) to begin the upgrade.</span></span>

    ```
    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path to Configuration File>

    ```
    <span data-ttu-id="95cfa-121">Az előrehaladást a frissítés a Service Fabric Explorerben talál.</span><span class="sxs-lookup"><span data-stu-id="95cfa-121">You can monitor the progress of the upgrade on Service Fabric Explorer.</span></span> <span data-ttu-id="95cfa-122">Alternatív megoldásként futtathatja [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)</span><span class="sxs-lookup"><span data-stu-id="95cfa-122">Alternatively, you can run [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)</span></span>

### <a name="add-nodes-to-clusters-configured-with-windows-security-using-gmsa"></a><span data-ttu-id="95cfa-123">Csomópontok hozzáadása a konfigurált Windows biztonsági csoportosan felügyelt szolgáltatásfiókot használó fürtök</span><span class="sxs-lookup"><span data-stu-id="95cfa-123">Add nodes to clusters configured with Windows Security using gMSA</span></span>
<span data-ttu-id="95cfa-124">A fürtök konfigurált csoport által felügyelt szolgáltatás Account(gMSA) (https://technet.microsoft.com/library/hh831782.aspx) egy új csomópont felveheti egy konfigurációs frissítéssel:</span><span class="sxs-lookup"><span data-stu-id="95cfa-124">For clusters configured with Group Managed Service Account(gMSA)(https://technet.microsoft.com/library/hh831782.aspx), a new node can be added using a configuration upgrade:</span></span>
1. <span data-ttu-id="95cfa-125">Futtatás [Get-ServiceFabricClusterConfiguration](/powershell/module/servicefabric/get-servicefabricclusterconfiguration?view=azureservicefabricps) a legfrissebb konfigurációs fájlt, és adja hozzá az új csomópont hozzá szeretne adni az "Csomópont" szakaszban adatait a meglévő csomópontok egyikén.</span><span class="sxs-lookup"><span data-stu-id="95cfa-125">Run [Get-ServiceFabricClusterConfiguration](/powershell/module/servicefabric/get-servicefabricclusterconfiguration?view=azureservicefabricps) on any of the existing nodes to get the latest configuration file and add details about the new node you want to add in the "Nodes" section.</span></span> <span data-ttu-id="95cfa-126">Ellenőrizze, hogy az új csomópont ugyanazt a csoportosan felügyelt fiókot részét képezi.</span><span class="sxs-lookup"><span data-stu-id="95cfa-126">Make sure the new node is part of the same group managed account.</span></span> <span data-ttu-id="95cfa-127">Ezt a fiókot minden számítógépen rendszergazdának kell lennie.</span><span class="sxs-lookup"><span data-stu-id="95cfa-127">This account should be an Administrator on all machines.</span></span>

    ```
        {
            "nodeName": "vm5",
            "iPAddress": "182.17.34.52",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc1/r0",
            "upgradeDomain": "UD1"
        }
    ```
2. <span data-ttu-id="95cfa-128">Futtatás [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps) a frissítésének megkezdésére.</span><span class="sxs-lookup"><span data-stu-id="95cfa-128">Run [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps) to begin the upgrade.</span></span>

    ```
    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path to Configuration File>
    ```
    <span data-ttu-id="95cfa-129">Az előrehaladást a frissítés a Service Fabric Explorerben talál.</span><span class="sxs-lookup"><span data-stu-id="95cfa-129">You can monitor the progress of the upgrade on Service Fabric Explorer.</span></span> <span data-ttu-id="95cfa-130">Alternatív megoldásként futtathatja [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)</span><span class="sxs-lookup"><span data-stu-id="95cfa-130">Alternatively, you can run [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)</span></span>

### <a name="add-node-types-to-your-cluster"></a><span data-ttu-id="95cfa-131">Csomóponttípusok hozzáadása a fürthöz</span><span class="sxs-lookup"><span data-stu-id="95cfa-131">Add node types to your cluster</span></span>
<span data-ttu-id="95cfa-132">Ahhoz, hogy az új csomópont-típus hozzáadása, módosítása a konfigurációját, és tartalmazza az új csomópont típus az "a NodeType tulajdonságok értéke" a "Tulajdonságok" szakaszt, és egy konfigurációs megkezdéséhez használatával frissítse [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps).</span><span class="sxs-lookup"><span data-stu-id="95cfa-132">In order to add a new node type, modify your configuration to include the new node type in "NodeTypes" section under "Properties" and begin a configuration upgrade using [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps).</span></span> <span data-ttu-id="95cfa-133">A frissítés után is hozzáadhat új csomópontok a fürthöz, ez a csomóponttípus.</span><span class="sxs-lookup"><span data-stu-id="95cfa-133">Once the upgrade completes, you can add new nodes to your cluster with this node type.</span></span>

## <a name="remove-nodes-from-your-cluster"></a><span data-ttu-id="95cfa-134">Csomópontok eltávolítása a fürtből</span><span class="sxs-lookup"><span data-stu-id="95cfa-134">Remove nodes from your cluster</span></span>
<span data-ttu-id="95cfa-135">A csomópont távolíthatók el a fürt egy konfigurációs frissítése a következő módon:</span><span class="sxs-lookup"><span data-stu-id="95cfa-135">A node can be removed from a cluster using a configuration upgrade, in the following manner:</span></span>

1. <span data-ttu-id="95cfa-136">Futtatás [Get-ServiceFabricClusterConfiguration](/powershell/module/servicefabric/get-servicefabricclusterconfiguration?view=azureservicefabricps) beolvasni a legfrissebb konfigurációs fájl és *eltávolítása* a csomópont "Csomópont" szakaszában.</span><span class="sxs-lookup"><span data-stu-id="95cfa-136">Run [Get-ServiceFabricClusterConfiguration](/powershell/module/servicefabric/get-servicefabricclusterconfiguration?view=azureservicefabricps) to get the latest configuration file and *remove* the node from "Nodes" section.</span></span>
<span data-ttu-id="95cfa-137">A "NodesToBeRemoved" paraméter hozzáadása "FabricSettings" szakasz "Beállítása" szakaszát.</span><span class="sxs-lookup"><span data-stu-id="95cfa-137">Add the "NodesToBeRemoved" parameter to "Setup" section inside "FabricSettings" section.</span></span> <span data-ttu-id="95cfa-138">A "érték" csomópont nevének a csomópontokra, amelyeket el kell távolítani a vesszővel tagolt listáját kell lennie.</span><span class="sxs-lookup"><span data-stu-id="95cfa-138">The "value" should be a comma separated list of node names of nodes that need to be removed.</span></span>

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
2. <span data-ttu-id="95cfa-139">Futtatás [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps) a frissítésének megkezdésére.</span><span class="sxs-lookup"><span data-stu-id="95cfa-139">Run [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps) to begin the upgrade.</span></span>

    ```
    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path to Configuration File>

    ```
    <span data-ttu-id="95cfa-140">Az előrehaladást a frissítés a Service Fabric Explorerben talál.</span><span class="sxs-lookup"><span data-stu-id="95cfa-140">You can monitor the progress of the upgrade on Service Fabric Explorer.</span></span> <span data-ttu-id="95cfa-141">Alternatív megoldásként futtathatja [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)</span><span class="sxs-lookup"><span data-stu-id="95cfa-141">Alternatively, you can run [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)</span></span>

> [!NOTE]
> <span data-ttu-id="95cfa-142">Csomópontok eltávolítása több frissítés kezdeményezhet.</span><span class="sxs-lookup"><span data-stu-id="95cfa-142">Removal of nodes may initiate multiple upgrades.</span></span> <span data-ttu-id="95cfa-143">Egyes csomópontok lesznek megjelölve `IsSeedNode=”true”` címkét, és a fürt lekérdezésével azonosítható manifest használatával `Get-ServiceFabricClusterManifest`.</span><span class="sxs-lookup"><span data-stu-id="95cfa-143">Some nodes are marked with `IsSeedNode=”true”` tag and can be identified by querying the cluster manifest using `Get-ServiceFabricClusterManifest`.</span></span> <span data-ttu-id="95cfa-144">Ilyen csomópontok eltávolítása is tovább tarthat a többinél mivel helyezi át az ilyen helyzetekben a kezdőérték csomópontok kell.</span><span class="sxs-lookup"><span data-stu-id="95cfa-144">Removal of such nodes may take longer than others since the seed nodes will have to be moved around in such scenarios.</span></span> <span data-ttu-id="95cfa-145">A fürt legalább 3 elsődleges típusú csomópontok kell fenntartani.</span><span class="sxs-lookup"><span data-stu-id="95cfa-145">The cluster must maintain a minimum of 3 primary node type nodes.</span></span>
> 
> 

### <a name="remove-node-types-from-your-cluster"></a><span data-ttu-id="95cfa-146">Csomóponttípusok eltávolítása a fürtből</span><span class="sxs-lookup"><span data-stu-id="95cfa-146">Remove node types from your cluster</span></span>
<span data-ttu-id="95cfa-147">Mielőtt eltávolítaná a csomóponttípus, kérjük, ellenőrizze hivatkozzon a csomóponttípus csomópontok esetén.</span><span class="sxs-lookup"><span data-stu-id="95cfa-147">Before removing a node type, please double check if there are any nodes referencing the node type.</span></span> <span data-ttu-id="95cfa-148">A megfelelő típusú csomópont eltávolítása előtt távolítsa el ezeket a csomópontokat.</span><span class="sxs-lookup"><span data-stu-id="95cfa-148">Remove these nodes before removing the corresponding node type.</span></span> <span data-ttu-id="95cfa-149">Minden megfelelő csomópontokat törlődik, miután a NodeType távolítsa el a fürtkonfiguráció, és egy konfigurációs megkezdéséhez használatával frissítse [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps).</span><span class="sxs-lookup"><span data-stu-id="95cfa-149">Once all corresponding nodes are removed, you can remove the NodeType from the cluster configuration and begin a configuration upgrade using [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps).</span></span>


### <a name="replace-primary-nodes-of-your-cluster"></a><span data-ttu-id="95cfa-150">Cserélje le a fürt elsődleges csomópontok</span><span class="sxs-lookup"><span data-stu-id="95cfa-150">Replace primary nodes of your cluster</span></span>
<span data-ttu-id="95cfa-151">A cseréje az elsődleges csomópont helyett eltávolítása és a kötegek egymás után kell elvégezni egy csomópont.</span><span class="sxs-lookup"><span data-stu-id="95cfa-151">The replacement of primary nodes should be performed one node after another, instead of removing and then adding in batches.</span></span>


## <a name="next-steps"></a><span data-ttu-id="95cfa-152">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="95cfa-152">Next steps</span></span>
* [<span data-ttu-id="95cfa-153">Önálló Windows-fürt konfigurációs beállításai</span><span class="sxs-lookup"><span data-stu-id="95cfa-153">Configuration settings for standalone Windows cluster</span></span>](service-fabric-cluster-manifest.md)
* [<span data-ttu-id="95cfa-154">Biztonságos Windows X509 használata önálló fürtben, tanúsítványok</span><span class="sxs-lookup"><span data-stu-id="95cfa-154">Secure a standalone cluster on Windows using X509 certificates</span></span>](service-fabric-windows-cluster-x509-security.md)
* [<span data-ttu-id="95cfa-155">Hozzon létre egy különálló Service Fabric-fürt Windowst futtató Azure virtuális gépeken</span><span class="sxs-lookup"><span data-stu-id="95cfa-155">Create a standalone Service Fabric cluster with Azure VMs running Windows</span></span>](service-fabric-cluster-creation-with-windows-azure-vms.md)

