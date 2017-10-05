---
title: "Azure Service Fabric-fürt beállítása | Microsoft Docs"
description: "Rövid útmutató – Windows vagy Linux Service Fabric-fürt létrehozása az Azure-ban."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/24/2017
ms.author: ryanwi
ms.openlocfilehash: ec59450052b377412a28f7eaf55d1f1512b55195
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="create-your-first-service-fabric-cluster-on-azure"></a><span data-ttu-id="42768-103">Az első saját Service Fabric-fürt létrehozása az Azure-on</span><span class="sxs-lookup"><span data-stu-id="42768-103">Create your first Service Fabric cluster on Azure</span></span>
<span data-ttu-id="42768-104">A [Service Fabric-fürt](service-fabric-deploy-anywhere.md) virtuális és fizikai gépek hálózaton keresztül csatlakozó készlete, amelyen mikroszolgáltatásokat helyezhet üzembe és felügyelhet.</span><span class="sxs-lookup"><span data-stu-id="42768-104">A [Service Fabric cluster](service-fabric-deploy-anywhere.md) is a network-connected set of virtual or physical machines into which your microservices are deployed and managed.</span></span> <span data-ttu-id="42768-105">A rövid útmutató segítségével csupán pár perc alatt létrehozhat egy öt csomópontot számláló, Windows- vagy Linux-alapú fürtöt az [Azure PowerShellen](https://msdn.microsoft.com/library/dn135248) vagy az [Azure Portalon](http://portal.azure.com) keresztül.</span><span class="sxs-lookup"><span data-stu-id="42768-105">This quickstart helps you to create a five-node cluster, running on either Windows or Linux, through the [Azure PowerShell](https://msdn.microsoft.com/library/dn135248) or [Azure portal](http://portal.azure.com) in just a few minutes.</span></span>  

<span data-ttu-id="42768-106">Ha nem rendelkezik Azure-előfizetéssel, mindössze néhány perc alatt létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) a virtuális gép létrehozásának megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="42768-106">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>


## <a name="use-the-azure-portal"></a><span data-ttu-id="42768-107">Az Azure Portal használata</span><span class="sxs-lookup"><span data-stu-id="42768-107">Use the Azure portal</span></span>

<span data-ttu-id="42768-108">Jelentkezzen be az Azure Portalra a [http://portal.azure.com](http://portal.azure.com) webhelyen.</span><span class="sxs-lookup"><span data-stu-id="42768-108">Log in to the Azure portal at [http://portal.azure.com](http://portal.azure.com).</span></span>

### <a name="create-the-cluster"></a><span data-ttu-id="42768-109">A fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="42768-109">Create the cluster</span></span>

1. <span data-ttu-id="42768-110">Kattintson az Azure Portal bal felső sarkában található **Új** gombra.</span><span class="sxs-lookup"><span data-stu-id="42768-110">Click the **New** button found on the upper left-hand corner of the Azure portal.</span></span>
2. <span data-ttu-id="42768-111">Válassza a **Számítás** elemet az **Új** panelen, majd a **Service Fabric-fürt** elemet a **Számítás** panelen.</span><span class="sxs-lookup"><span data-stu-id="42768-111">Select **Compute** from the **New** blade and then select **Service Fabric Cluster** from the **Compute** blade.</span></span>
3. <span data-ttu-id="42768-112">Töltse ki a Service Fabric **Alapvető beállítások** űrlapját.</span><span class="sxs-lookup"><span data-stu-id="42768-112">Fill out the Service Fabric **Basics** form.</span></span> <span data-ttu-id="42768-113">Az **Operációs rendszer** beállításban adja meg a Windows vagy Linux azon verzióját, amelyiken a fürt csomópontjait futtatni szeretné.</span><span class="sxs-lookup"><span data-stu-id="42768-113">For **Operating system**, select the version of Windows or Linux you want the cluster nodes to run.</span></span> <span data-ttu-id="42768-114">Az itt megadott felhasználónévvel és jelszóval bejelentkezhet a virtuális gépbe.</span><span class="sxs-lookup"><span data-stu-id="42768-114">The user name and password entered here is used to log in to the virtual machine.</span></span> <span data-ttu-id="42768-115">Hozzon létre egy új **Erőforráscsoportot**.</span><span class="sxs-lookup"><span data-stu-id="42768-115">For **Resource group**, create a new one.</span></span> <span data-ttu-id="42768-116">Az erőforráscsoport olyan logikai tároló, amelybe a rendszer létrehozza és együttesen kezeli az Azure-erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="42768-116">A resource group is a logical container into which Azure resources are created and collectively managed.</span></span> <span data-ttu-id="42768-117">Amikor végzett, kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="42768-117">When complete, click **OK**.</span></span>

    ![A fürtbeállítás kimenete][cluster-setup-basics]

4. <span data-ttu-id="42768-119">Töltse ki a **Fürtkonfiguráció** űrlapot.</span><span class="sxs-lookup"><span data-stu-id="42768-119">Fill out the **Cluster configuration** form.</span></span>  <span data-ttu-id="42768-120">A **Csomóponttípusok száma** elemnél adja meg az „1” értéket.</span><span class="sxs-lookup"><span data-stu-id="42768-120">For **Node type count**, enter "1".</span></span>

5. <span data-ttu-id="42768-121">Válassza az **1. csomóponttípus (elsődleges)** lehetőséget, és töltse ki a **Csomóponttípus konfigurációja** űrlapot.</span><span class="sxs-lookup"><span data-stu-id="42768-121">Select **Node type 1 (Primary)** and fill out the **Node type configuration** form.</span></span>  <span data-ttu-id="42768-122">Írjon be egy csomóponttípust, és adja meg a [Tartóssági szint](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster) elemnél a „Bronz” értéket.</span><span class="sxs-lookup"><span data-stu-id="42768-122">Enter a node type name and set the [Durability tier](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster) to "Bronze."</span></span>  <span data-ttu-id="42768-123">Válassza ki a virtuális gép méretét.</span><span class="sxs-lookup"><span data-stu-id="42768-123">Select a VM size.</span></span>

    <span data-ttu-id="42768-124">A csomóponttípusok határozzák meg a virtuális gépek méretét, számát, egyedi végpontjait, valamint az adott típusú virtuális gépek egyéb beállításait.</span><span class="sxs-lookup"><span data-stu-id="42768-124">Node types define the VM size, number of VMs, custom endpoints, and other settings for the VMs of that type.</span></span> <span data-ttu-id="42768-125">Mindegyik megadott csomóponttípus külön virtuálisgép-méretezési csoportként lesz beállítva, amely a virtuális gépek csoportként való üzembe helyezésére és felügyeletére használható.</span><span class="sxs-lookup"><span data-stu-id="42768-125">Each node type defined is set up as a separate virtual machine scale set, which is used to deploy and managed virtual machines as a set.</span></span> <span data-ttu-id="42768-126">Mindegyik csomóponttípus egymástól függetlenül skálázható vertikálisan le vagy föl, eltérő nyitott portokkal rendelkezik, és eltérő kapacitásmetrikái lehetnek.</span><span class="sxs-lookup"><span data-stu-id="42768-126">Each node type can be scaled up or down independently, have different sets of ports open, and can have different capacity metrics.</span></span>  <span data-ttu-id="42768-127">Az első – vagy elsődleges – csomóponttípus az, ahol a Service Fabric rendszerszolgáltatásai futnak, és ennek öt vagy annál több virtuális gépet kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="42768-127">The first, or primary, node type is where Service Fabric system services are hosted and must have five or more VMs.</span></span>

    <span data-ttu-id="42768-128">A [kapacitástervezés](service-fabric-cluster-capacity.md) az éles rendszerek üzembe helyezésének lényeges lépése.</span><span class="sxs-lookup"><span data-stu-id="42768-128">For any production deployment, [capacity planning](service-fabric-cluster-capacity.md) is an important step.</span></span>  <span data-ttu-id="42768-129">A jelen rövid útmutatóban azonban nem fog alkalmazásokat futtatni, így válassza a *DS1_v2 Standard* VM-méretet.</span><span class="sxs-lookup"><span data-stu-id="42768-129">For this quick start, however, you aren't running applications so select a *DS1_v2 Standard* VM size.</span></span>  <span data-ttu-id="42768-130">[Megbízhatósági szintként](service-fabric-cluster-capacity.md#the-reliability-characteristics-of-the-cluster) adja meg az „Ezüst” értéket, a virtuálisgép-méretezési csoport kezdő kapacitását pedig állítsa 5-re.</span><span class="sxs-lookup"><span data-stu-id="42768-130">Select "Silver" for the [reliability tier](service-fabric-cluster-capacity.md#the-reliability-characteristics-of-the-cluster) and an initial virtual machine scale set capacity of 5.</span></span>  

    <span data-ttu-id="42768-131">Az egyedi végpontok portokat nyitnak meg az Azure Load Balancerben, hogy csatlakozni tudjon a fürtben futó alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="42768-131">Custom endpoints open up ports in the Azure load balancer so that you can connect with applications running on the cluster.</span></span>  <span data-ttu-id="42768-132">Adja meg a „80, 8172” értéket a 80-as és a 8172-es portok megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="42768-132">Enter "80, 8172" to open up ports 80 and 8172.</span></span>

    <span data-ttu-id="42768-133">Ne jelölje be a **Speciális beállítások konfigurálása** jelölőnégyzetet, amely a TCP/HTTP felügyeleti végpontok, alkalmazásport-tartományok, [elhelyezési korlátozások](service-fabric-cluster-resource-manager-configure-services.md#placement-constraints) és [kapacitástulajdonságok](service-fabric-cluster-resource-manager-metrics.md) testreszabására szolgál.</span><span class="sxs-lookup"><span data-stu-id="42768-133">Do not check the **Configure advanced settings** box, which is used for customizing TCP/HTTP management endpoints, application port ranges, [placement constraints](service-fabric-cluster-resource-manager-configure-services.md#placement-constraints), and [capacity properties](service-fabric-cluster-resource-manager-metrics.md).</span></span>    

    <span data-ttu-id="42768-134">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="42768-134">Select **OK**.</span></span>

6. <span data-ttu-id="42768-135">A **Fürtkonfiguráció** űrlapon állítsa a **Diagnosztika** beállítást **Be** értékre.</span><span class="sxs-lookup"><span data-stu-id="42768-135">In the **Cluster configuration** form, set **Diagnostics** to **On**.</span></span>  <span data-ttu-id="42768-136">A jelen rövid útmutató során nem kell megadnia [hálóbeállítási](service-fabric-cluster-fabric-settings.md) tulajdonságokat.</span><span class="sxs-lookup"><span data-stu-id="42768-136">For this quickstart, you do not need to enter any [fabric setting](service-fabric-cluster-fabric-settings.md) properties.</span></span>  <span data-ttu-id="42768-137">A **Fabric verziója** elemnél válassza az **Automatikus** frissítési módot, így a Microsoft automatikusan frissíti a fürtön futó Fabric-kód verzióját.</span><span class="sxs-lookup"><span data-stu-id="42768-137">In **Fabric version**, select **Automatic** upgrade mode so that Microsoft automatically updates the version of the fabric code running the cluster.</span></span>  <span data-ttu-id="42768-138">Állítsa a módot **Manuálisra**, ha ki szeretne [választani egy támogatott verziót](service-fabric-cluster-upgrade.md), amelyre frissíteni szeretne.</span><span class="sxs-lookup"><span data-stu-id="42768-138">Set the mode to **Manual** if you want to [choose a supported version](service-fabric-cluster-upgrade.md) to upgrade to.</span></span> 

    ![Csomóponttípus konfigurálása][node-type-config]

    <span data-ttu-id="42768-140">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="42768-140">Select **OK**.</span></span>

7. <span data-ttu-id="42768-141">Töltse ki a **Biztonság** űrlapot.</span><span class="sxs-lookup"><span data-stu-id="42768-141">Fill out the **Security** form.</span></span>  <span data-ttu-id="42768-142">Ehhez a rövid útmutatóhoz válassza a **Nem biztonságos** beállítást.</span><span class="sxs-lookup"><span data-stu-id="42768-142">For this quick start select **Unsecure**.</span></span>  <span data-ttu-id="42768-143">Az éles számítási feladatokhoz azonban határozottan javasolt biztonságos fürtöket létrehozni, mivel a nem biztonságos fürtökhöz bárki név nélkül csatlakozhat, és ott felügyeleti tevékenységeket hajthat végre.</span><span class="sxs-lookup"><span data-stu-id="42768-143">It is highly recommended to create a secure cluster for production workloads, however, since anyone can anonymously connect to an unsecure cluster and perform management operations.</span></span>  

    <span data-ttu-id="42768-144">A Service Fabric tanúsítványokat használ a hitelesítéshez és titkosításhoz a fürtök és a rajtuk található alkalmazások különféle részeinek védelmére.</span><span class="sxs-lookup"><span data-stu-id="42768-144">Certificates are used in Service Fabric to provide authentication and encryption to secure various aspects of a cluster and its applications.</span></span> <span data-ttu-id="42768-145">A tanúsítványok Service Fabricban való használatával kapcsolatos további információkért lásd a [Service Fabric-fürtök biztonsági forgatókönyveit](service-fabric-cluster-security.md).</span><span class="sxs-lookup"><span data-stu-id="42768-145">For more information on how certificates are used in Service Fabric, see [Service Fabric cluster security scenarios](service-fabric-cluster-security.md).</span></span>  <span data-ttu-id="42768-146">Az Azure Active Directoryval végzett felhasználóhitelesítés engedélyezéséhez, illetve a tanúsítványok alkalmazásbiztonsági célú beállításához [a fürtöt Resource Manager-sablonból hozza létre](service-fabric-cluster-creation-via-arm.md).</span><span class="sxs-lookup"><span data-stu-id="42768-146">To enable user authentication using Azure Active Directory or to set up certificates for application security, [create a cluster from a Resource Manager template](service-fabric-cluster-creation-via-arm.md).</span></span>

    <span data-ttu-id="42768-147">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="42768-147">Select **OK**.</span></span>

8. <span data-ttu-id="42768-148">Tekintse át az összegzést.</span><span class="sxs-lookup"><span data-stu-id="42768-148">Review the summary.</span></span>  <span data-ttu-id="42768-149">Ha le szeretné tölteni a megadott beállítások alapján összeállított Resource Manager-sablont, válassza a **Sablon és paraméterek letöltése** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="42768-149">If you'd like to download a Resource Manager template built from the settings you entered, select **Download template and parameters**.</span></span>  <span data-ttu-id="42768-150">A fürt létrehozásához kattintson a **Létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="42768-150">Select **Create** to create the cluster.</span></span>

    <span data-ttu-id="42768-151">A létrehozás folyamatát az értesítésekben követheti nyomon.</span><span class="sxs-lookup"><span data-stu-id="42768-151">You can see the creation progress in the notifications.</span></span> <span data-ttu-id="42768-152">(Kattintson a „Harang” ikonra az állapotsor mellett, a képernyő jobb felső részén.) Ha a fürt létrehozásakor **A kezdőpulton rögzít** lehetőségre kattintott, a **Service Fabric-fürt üzembe helyezése** a **Kezdőpultra** rögzítve látható.</span><span class="sxs-lookup"><span data-stu-id="42768-152">(Click the "Bell" icon near the status bar at the upper right of your screen.) If you clicked **Pin to Startboard** while creating the cluster, you see **Deploying Service Fabric Cluster** pinned to the **Start** board.</span></span>

### <a name="view-cluster-status"></a><span data-ttu-id="42768-153">Fürt állapotának megtekintése</span><span class="sxs-lookup"><span data-stu-id="42768-153">View cluster status</span></span>
<span data-ttu-id="42768-154">Miután a fürt létrejött, a Portal **Áttekintés** paneljén tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="42768-154">Once your cluster is created, you can inspect your cluster in the **Overview** blade in the portal.</span></span> <span data-ttu-id="42768-155">A fürt részletei megjelennek az irányítópulton, beleértve a fürt nyilvános végpontját és egy, a Service Fabric Explorerre mutató hivatkozást.</span><span class="sxs-lookup"><span data-stu-id="42768-155">You can now see the details of your cluster in the dashboard, including the cluster's public endpoint and a link to Service Fabric Explorer.</span></span>

![Fürt állapota][cluster-status]

### <a name="visualize-the-cluster-using-service-fabric-explorer"></a><span data-ttu-id="42768-157">A fürt megjelenítése a Service Fabric Explorerrel</span><span class="sxs-lookup"><span data-stu-id="42768-157">Visualize the cluster using Service Fabric explorer</span></span>
<span data-ttu-id="42768-158">A [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) hatékony eszköz a fürtök megjelenítéséhez és az alkalmazások kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="42768-158">[Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) is a good tool for visualizing your cluster and managing applications.</span></span>  <span data-ttu-id="42768-159">A Service Fabric Explorer a fürtben futó szolgáltatás,</span><span class="sxs-lookup"><span data-stu-id="42768-159">Service Fabric Explorer is a service that runs in the cluster.</span></span>  <span data-ttu-id="42768-160">amely a webböngészőből érhető el, ha a **Service Fabric Explorer** hivatkozásra kattint a fürt **Áttekintés** oldalán a Portalon.</span><span class="sxs-lookup"><span data-stu-id="42768-160">Access it using a web browser by clicking the **Service Fabric Explorer** link of the cluster **Overview** page in the portal.</span></span>  <span data-ttu-id="42768-161">A címet közvetlenül a böngészőben is megadhatja: [http://quickstartcluster.westus.cloudapp.azure.com:19080/Explorer](http://quickstartcluster.westus.cloudapp.azure.com:19080/Explorer)</span><span class="sxs-lookup"><span data-stu-id="42768-161">You can also enter the address directly into the browser: [http://quickstartcluster.westus.cloudapp.azure.com:19080/Explorer](http://quickstartcluster.westus.cloudapp.azure.com:19080/Explorer)</span></span>

<span data-ttu-id="42768-162">A fürt irányítópultja áttekintést nyújt a fürtről, beleértve az alkalmazások és a csomópontok állapotának összefoglalását.</span><span class="sxs-lookup"><span data-stu-id="42768-162">The cluster dashboard provides an overview of your cluster, including a summary of application and node health.</span></span> <span data-ttu-id="42768-163">A csomópontnézet a fürt fizikai elrendezését mutatja.</span><span class="sxs-lookup"><span data-stu-id="42768-163">The node view shows the physical layout of the cluster.</span></span> <span data-ttu-id="42768-164">Az egyes csomópontoknál megtekintheti, hogy melyik alkalmazások kódja üzemel az adott csomóponton.</span><span class="sxs-lookup"><span data-stu-id="42768-164">For a given node, you can inspect which applications have code deployed on that node.</span></span>

![Service Fabric Explorer][service-fabric-explorer]

### <a name="connect-to-the-cluster-using-powershell"></a><span data-ttu-id="42768-166">Csatlakozás a fürthöz PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="42768-166">Connect to the cluster using PowerShell</span></span>
<span data-ttu-id="42768-167">Ha ellenőrizni szeretné, hogy a fürt fut-e, csatlakozzon hozzá a PowerShell-lel.</span><span class="sxs-lookup"><span data-stu-id="42768-167">Verify that the cluster is running by connecting using PowerShell.</span></span>  <span data-ttu-id="42768-168">A ServiceFabric PowerShell-modul a [Service Fabric SDK](service-fabric-get-started.md)-val települ.</span><span class="sxs-lookup"><span data-stu-id="42768-168">The ServiceFabric PowerShell module is installed with the [Service Fabric SDK](service-fabric-get-started.md).</span></span>  <span data-ttu-id="42768-169">A [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) parancsmag kiépít egy kapcsolatot a fürttel.</span><span class="sxs-lookup"><span data-stu-id="42768-169">The [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet establishes a connection to the cluster.</span></span>   

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint quickstartcluster.westus2.cloudapp.azure.com:19000
```
<span data-ttu-id="42768-170">A fürtökhöz való csatlakozást bemutató egyéb példákért lásd: [Csatlakozás biztonságos fürthöz](service-fabric-connect-to-secure-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="42768-170">See [Connect to a secure cluster](service-fabric-connect-to-secure-cluster.md) for other examples of connecting to a cluster.</span></span> <span data-ttu-id="42768-171">Miután csatlakozott a fürthöz, a [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) parancsmaggal listázza a fürtben lévő csomópontokat és az egyes csomópontok állapotinformációit.</span><span class="sxs-lookup"><span data-stu-id="42768-171">After connecting to the cluster, use the [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) cmdlet to display a list of nodes in the cluster and status information for each node.</span></span> <span data-ttu-id="42768-172">A **HealthState** tulajdonságnak *OK* értékűnek kell lennie minden csomópont esetében.</span><span class="sxs-lookup"><span data-stu-id="42768-172">**HealthState** should be *OK* for each node.</span></span>

```powershell
PS C:\Users\sfuser> Get-ServiceFabricNode |Format-Table

NodeDeactivationInfo NodeName     IpAddressOrFQDN NodeType  CodeVersion  ConfigVersion NodeStatus NodeUpTime NodeDownTime HealthState
-------------------- --------     --------------- --------  -----------  ------------- ---------- ---------- ------------ -----------
                     _nodetype1_2 10.0.0.6        nodetype1 5.7.198.9494 1                     Up 03:00:38   00:00:00              Ok
                     _nodetype1_1 10.0.0.5        nodetype1 5.7.198.9494 1                     Up 03:00:38   00:00:00              Ok
                     _nodetype1_0 10.0.0.4        nodetype1 5.7.198.9494 1                     Up 03:00:38   00:00:00              Ok
                     _nodetype1_4 10.0.0.8        nodetype1 5.7.198.9494 1                     Up 03:00:38   00:00:00              Ok
                     _nodetype1_3 10.0.0.7        nodetype1 5.7.198.9494 1                     Up 03:00:38   00:00:00              Ok
```

### <a name="remove-the-cluster"></a><span data-ttu-id="42768-173">A fürt eltávolítása</span><span class="sxs-lookup"><span data-stu-id="42768-173">Remove the cluster</span></span>
<span data-ttu-id="42768-174">A Service Fabric-fürtben a fürt erőforrásán felül egyéb Azure-erőforrások is megtalálhatók.</span><span class="sxs-lookup"><span data-stu-id="42768-174">A Service Fabric cluster is made up of other Azure resources in addition to the cluster resource itself.</span></span> <span data-ttu-id="42768-175">A Service Fabric-fürtök teljes törléséhez ezért az összes őket alkotó erőforrást is törölni kell.</span><span class="sxs-lookup"><span data-stu-id="42768-175">So to completely delete a Service Fabric cluster you also need to delete all the resources it is made of.</span></span> <span data-ttu-id="42768-176">A fürt és az összes általa használt erőforrás törlésének legegyszerűbb módja az erőforráscsoport törlése.</span><span class="sxs-lookup"><span data-stu-id="42768-176">The simplest way to delete the cluster and all the resources it consumes is to delete the resource group.</span></span> <span data-ttu-id="42768-177">A fürt törlésének egyéb módjaival, illetve egy erőforráscsoportba tartozó némely (de nem az összes) erőforrás törlésével kapcsolatban lásd a [fürt törlésével](service-fabric-cluster-delete.md) foglalkozó témakört.</span><span class="sxs-lookup"><span data-stu-id="42768-177">For other ways to delete a cluster or to delete some (but not all) the resources in a resource group, see [Delete a cluster](service-fabric-cluster-delete.md)</span></span>

<span data-ttu-id="42768-178">Erőforráscsoport törlése az Azure Portalon:</span><span class="sxs-lookup"><span data-stu-id="42768-178">Delete a resource group in the Azure portal:</span></span>
1. <span data-ttu-id="42768-179">Lépjen a törölni kívánt Service Fabric-fürtre.</span><span class="sxs-lookup"><span data-stu-id="42768-179">Navigate to the Service Fabric cluster you want to delete.</span></span>
2. <span data-ttu-id="42768-180">Kattintson az **Erőforráscsoport** nevére a fürt alapvető erőforrásainak lapján.</span><span class="sxs-lookup"><span data-stu-id="42768-180">Click the **Resource Group** name on the cluster essentials page.</span></span>
3. <span data-ttu-id="42768-181">Az **Erőforráscsoport alapvető erőforrásai** oldalon kattintson az **Erőforráscsoport törlése** gombra, és kövesse a lapon megjelenő utasításokat az erőforráscsoport törléséhez.</span><span class="sxs-lookup"><span data-stu-id="42768-181">In the **Resource Group Essentials** page, click **Delete resource group** and follow the instructions on that page to complete the deletion of the resource group.</span></span>
    <span data-ttu-id="42768-182">![Az erőforráscsoport törlése][cluster-delete]</span><span class="sxs-lookup"><span data-stu-id="42768-182">![Delete the resource group][cluster-delete]</span></span>


## <a name="use-azure-powershell-to-deploy-a-secure-cluster"></a><span data-ttu-id="42768-183">Biztonságos fürt üzembe helyezése az Azure PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="42768-183">Use Azure Powershell to deploy a secure cluster</span></span>
1. <span data-ttu-id="42768-184">Töltse le a számítógépre az [Azure PowerShell-modul 4.0-s vagy újabb](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) verzióját.</span><span class="sxs-lookup"><span data-stu-id="42768-184">Download the [Azure Powershell module version 4.0 or higher](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) on your machine.</span></span>

2. <span data-ttu-id="42768-185">Nyisson meg egy Windows PowerShell-ablakot, és futtassa a következő parancsot.</span><span class="sxs-lookup"><span data-stu-id="42768-185">Open a Windows PowerShell window, Run the following command.</span></span> 
    
    ```powershell

    Get-Command -Module AzureRM.ServiceFabric 
    ```

    <span data-ttu-id="42768-186">Az alábbihoz hasonló kimenetnek kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="42768-186">You should see an output similar to the following.</span></span>

    ![ps-list][ps-list]

3. <span data-ttu-id="42768-188">Jelentkezzen be az Azure-ba, és válassza ki azt az előfizetést, amelyikben létre szeretné hozni a fürtöt</span><span class="sxs-lookup"><span data-stu-id="42768-188">Login to Azure and Select the subscription to which you want to create the cluster</span></span>

    ```powershell

    Login-AzureRmAccount

    Select-AzureRmSubscription -SubscriptionId "Subcription ID" 
    ```

4. <span data-ttu-id="42768-189">Futtassa az alábbi parancsot egy biztonságos fürt létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="42768-189">Run the following command to now create a secure cluster.</span></span> <span data-ttu-id="42768-190">Ne felejtse el testre szabni a paramétereket.</span><span class="sxs-lookup"><span data-stu-id="42768-190">Do not forget to customize the parameters.</span></span> 

    ```powershell
    $certpwd="Password#1234" | ConvertTo-SecureString -AsPlainText -Force
    $RDPpwd="Password#1234" | ConvertTo-SecureString -AsPlainText -Force 
    $RDPuser="vmadmin"
    $RGname="mycluster" # this is also the name of your cluster
    $clusterloc="SouthCentralUS"
    $subname="$RGname.$clusterloc.cloudapp.azure.com"
    $certfolder="c:\mycertificates\"
    $clustersize=1 # can take values 1, 3-99

    New-AzureRmServiceFabricCluster -ResourceGroupName $RGname -Location $clusterloc -ClusterSize $clustersize -VmUserName $RDPuser -VmPassword $RDPpwd -CertificateSubjectName $subname -CertificatePassword $certpwd -CertificateOutputFolder $certfolder
    ```

    <span data-ttu-id="42768-191">A parancs végrehajtása 10 perctől akár 30 percig is eltarthat, és a végén az alábbihoz hasonló kimenetnek kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="42768-191">The command can take anywhere from 10 minutes to 30 minutes to complete, at the end of it, you should get an output similar to the following.</span></span> <span data-ttu-id="42768-192">A kimenet információkat tartalmaz a tanúsítványról, arról a KeyVault tárolóról, ahová feltöltötték, és a helyi mappáról, ahová másolták.</span><span class="sxs-lookup"><span data-stu-id="42768-192">The output has information about the certificate, the KeyVault where it was uploaded to, and the local folder where the certificate is copied.</span></span> 

    ![ps-out][ps-out]

5. <span data-ttu-id="42768-194">Másolja ki a teljes kimenetet, és mentse egy szövegfájlba, mivel később hivatkoznia kell rá.</span><span class="sxs-lookup"><span data-stu-id="42768-194">Copy the entire output and save to a text file as we need to refer to it.</span></span> <span data-ttu-id="42768-195">Jegyezze fel az alábbi, kimenetből származó adatokat.</span><span class="sxs-lookup"><span data-stu-id="42768-195">Make a note of the following information from the output.</span></span> 

    - <span data-ttu-id="42768-196">**CertificateSavedLocalPath** : c:\mycertificates\mycluster20170504141137.pfx</span><span class="sxs-lookup"><span data-stu-id="42768-196">**CertificateSavedLocalPath** : c:\mycertificates\mycluster20170504141137.pfx</span></span>
    - <span data-ttu-id="42768-197">**CertificateThumbprint** : C4C1E541AD512B8065280292A8BA6079C3F26F10</span><span class="sxs-lookup"><span data-stu-id="42768-197">**CertificateThumbprint** : C4C1E541AD512B8065280292A8BA6079C3F26F10</span></span>
    - <span data-ttu-id="42768-198">**ManagementEndpoint** : https://mycluster.southcentralus.cloudapp.azure.com:19080</span><span class="sxs-lookup"><span data-stu-id="42768-198">**ManagementEndpoint** : https://mycluster.southcentralus.cloudapp.azure.com:19080</span></span>
    - <span data-ttu-id="42768-199">**ClientConnectionEndpointPort** : 19000</span><span class="sxs-lookup"><span data-stu-id="42768-199">**ClientConnectionEndpointPort** : 19000</span></span>

### <a name="install-the-certificate-on-your-local-machine"></a><span data-ttu-id="42768-200">A tanúsítvány telepítése helyi számítógépre</span><span class="sxs-lookup"><span data-stu-id="42768-200">Install the certificate on your local machine</span></span>
  
<span data-ttu-id="42768-201">A fürthöz való csatlakozáshoz telepíteni kell a tanúsítványt az aktuális felhasználó Személyes (Saját) tárolójába.</span><span class="sxs-lookup"><span data-stu-id="42768-201">To connect to the cluster, you need to install the certificate into the Personal (My) store of the current user.</span></span> 

<span data-ttu-id="42768-202">Futtassa az alábbi PowerShell-parancsot</span><span class="sxs-lookup"><span data-stu-id="42768-202">Run the following PowerShell</span></span>

```powershell
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My `
        -FilePath C:\mycertificates\the name of the cert.pfx `
        -Password (ConvertTo-SecureString -String certpwd -AsPlainText -Force)
```

<span data-ttu-id="42768-203">Most már készen áll a biztonságos fürthöz való csatlakozásra.</span><span class="sxs-lookup"><span data-stu-id="42768-203">You are now ready to connect to your secure cluster.</span></span>

### <a name="connect-to-a-secure-cluster"></a><span data-ttu-id="42768-204">Csatlakozás biztonságos fürthöz</span><span class="sxs-lookup"><span data-stu-id="42768-204">Connect to a secure cluster</span></span> 

<span data-ttu-id="42768-205">A biztonságos fürthöz való csatlakozáshoz futtassa az alábbi PowerShell-parancsot.</span><span class="sxs-lookup"><span data-stu-id="42768-205">Run the following PowerShell command to connect to a secure cluster.</span></span> <span data-ttu-id="42768-206">A tanúsítvány részleteinek meg kell egyezniük a fürt beállításához használt egyik tanúsítvány részleteivel.</span><span class="sxs-lookup"><span data-stu-id="42768-206">The certificate details must match a certificate that was used to set up the cluster.</span></span> 

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint <Certificate Thumbprint> `
          -FindType FindByThumbprint -FindValue <Certificate Thumbprint> `
          -StoreLocation CurrentUser -StoreName My
```


<span data-ttu-id="42768-207">Az alábbi példa az eredményül kapott paramétereket mutatja be:</span><span class="sxs-lookup"><span data-stu-id="42768-207">The following example shows the completed parameters:</span></span> 

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint mycluster.southcentralus.cloudapp.azure.com:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -FindType FindByThumbprint -FindValue C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -StoreLocation CurrentUser -StoreName My
```

<span data-ttu-id="42768-208">Az alábbi parancs futtatásával ellenőrizze, hogy csatlakozik-e, és hogy a fürt állapota kifogástalan-e.</span><span class="sxs-lookup"><span data-stu-id="42768-208">Run the following command to check that you are connected and the cluster is healthy.</span></span>

```powershell

Get-ServiceFabricClusterHealth

```
### <a name="publish-your-apps-to-your-cluster-from-visual-studio"></a><span data-ttu-id="42768-209">Alkalmazások közzététele a fürtön a Visual Studióból</span><span class="sxs-lookup"><span data-stu-id="42768-209">Publish your apps to your cluster from Visual Studio</span></span>

<span data-ttu-id="42768-210">Most, hogy beállított egy Azure-fürtöt, a [fürtön történő közzétételt](service-fabric-publish-app-remote-cluster.md) ismertető dokumentum utasításait követve közzéteheti rajta az alkalmazásait a Visual Studióból.</span><span class="sxs-lookup"><span data-stu-id="42768-210">Now that you have set up an Azure cluster, you can publish your applications to it from Visual Studio by following the [Publish to an cluster](service-fabric-publish-app-remote-cluster.md) document.</span></span> 

### <a name="remove-the-cluster"></a><span data-ttu-id="42768-211">A fürt eltávolítása</span><span class="sxs-lookup"><span data-stu-id="42768-211">Remove the cluster</span></span>
<span data-ttu-id="42768-212">A fürtben a fürt erőforrásán felül egyéb Azure-erőforrások is megtalálhatók.</span><span class="sxs-lookup"><span data-stu-id="42768-212">A cluster is made up of other Azure resources in addition to the cluster resource itself.</span></span> <span data-ttu-id="42768-213">A fürt és az összes általa használt erőforrás törlésének legegyszerűbb módja az erőforráscsoport törlése.</span><span class="sxs-lookup"><span data-stu-id="42768-213">The simplest way to delete the cluster and all the resources it consumes is to delete the resource group.</span></span> 

```powershell

Remove-AzureRmResourceGroup -Name $RGname -Force

```

## <a name="next-steps"></a><span data-ttu-id="42768-214">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="42768-214">Next steps</span></span>
<span data-ttu-id="42768-215">Most, hogy üzembe helyezett egy fejlesztési fürtöt, megpróbálkozhat a következőkkel:</span><span class="sxs-lookup"><span data-stu-id="42768-215">Now that you have set up a development cluster, try the following:</span></span>
* [<span data-ttu-id="42768-216">Biztonságos fürt létrehozása a Portalon</span><span class="sxs-lookup"><span data-stu-id="42768-216">Create a secure cluster in the portal</span></span>](service-fabric-cluster-creation-via-portal.md)
* [<span data-ttu-id="42768-217">Fürt létrehozása sablonból</span><span class="sxs-lookup"><span data-stu-id="42768-217">Create a cluster from a template</span></span>](service-fabric-cluster-creation-via-arm.md) 
* [<span data-ttu-id="42768-218">Appokat helyezhet üzembe a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="42768-218">Deploy apps using PowerShell</span></span>](service-fabric-deploy-remove-applications.md)


[cluster-setup-basics]: ./media/service-fabric-get-started-azure-cluster/basics.png
[node-type-config]: ./media/service-fabric-get-started-azure-cluster/nodetypeconfig.png
[cluster-status]: ./media/service-fabric-get-started-azure-cluster/clusterstatus.png
[service-fabric-explorer]: ./media/service-fabric-get-started-azure-cluster/sfx.png
[cluster-delete]: ./media/service-fabric-get-started-azure-cluster/delete.png
[ps-list]: ./media/service-fabric-get-started-azure-cluster/pslist.PNG
[ps-out]: ./media/service-fabric-get-started-azure-cluster/psout.PNG
