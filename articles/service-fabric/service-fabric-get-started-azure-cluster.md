---
title: "az Azure Service Fabric-fürt aaaSet |} Microsoft Docs"
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
ms.openlocfilehash: 13c60e293d19d607bb41ee4859706508c219a833
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-service-fabric-cluster-on-azure"></a><span data-ttu-id="a4039-103">Az első saját Service Fabric-fürt létrehozása az Azure-on</span><span class="sxs-lookup"><span data-stu-id="a4039-103">Create your first Service Fabric cluster on Azure</span></span>
<span data-ttu-id="a4039-104">A [Service Fabric-fürt](service-fabric-deploy-anywhere.md) virtuális és fizikai gépek hálózaton keresztül csatlakozó készlete, amelyen mikroszolgáltatásokat helyezhet üzembe és felügyelhet.</span><span class="sxs-lookup"><span data-stu-id="a4039-104">A [Service Fabric cluster](service-fabric-deploy-anywhere.md) is a network-connected set of virtual or physical machines into which your microservices are deployed and managed.</span></span> <span data-ttu-id="a4039-105">A gyors üzembe helyezés toocreate öt csomópontból álló fürt, hello keresztül futó Windows vagy Linux-segít [Azure PowerShell](https://msdn.microsoft.com/library/dn135248) vagy [Azure-portálon](http://portal.azure.com) csak néhány perc múlva.</span><span class="sxs-lookup"><span data-stu-id="a4039-105">This quickstart helps you toocreate a five-node cluster, running on either Windows or Linux, through hello [Azure PowerShell](https://msdn.microsoft.com/library/dn135248) or [Azure portal](http://portal.azure.com) in just a few minutes.</span></span>  

<span data-ttu-id="a4039-106">Ha nem rendelkezik Azure-előfizetéssel, mindössze néhány perc alatt létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) a virtuális gép létrehozásának megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="a4039-106">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>


## <a name="use-hello-azure-portal"></a><span data-ttu-id="a4039-107">Hello Azure portál használata</span><span class="sxs-lookup"><span data-stu-id="a4039-107">Use hello Azure portal</span></span>

<span data-ttu-id="a4039-108">Jelentkezzen be toohello: az Azure portál [http://portal.azure.com](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="a4039-108">Log in toohello Azure portal at [http://portal.azure.com](http://portal.azure.com).</span></span>

### <a name="create-hello-cluster"></a><span data-ttu-id="a4039-109">Hello fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="a4039-109">Create hello cluster</span></span>

1. <span data-ttu-id="a4039-110">Kattintson a hello **új** hello bal felső sarkában hello Azure-portálon található gombra.</span><span class="sxs-lookup"><span data-stu-id="a4039-110">Click hello **New** button found on hello upper left-hand corner of hello Azure portal.</span></span>
2. <span data-ttu-id="a4039-111">Válassza ki **számítási** a hello **új** panelen, majd válassza ki **Service Fabric-fürt** a hello **számítási** panelen.</span><span class="sxs-lookup"><span data-stu-id="a4039-111">Select **Compute** from hello **New** blade and then select **Service Fabric Cluster** from hello **Compute** blade.</span></span>
3. <span data-ttu-id="a4039-112">Töltse ki a Service Fabric hello **alapjai** űrlap.</span><span class="sxs-lookup"><span data-stu-id="a4039-112">Fill out hello Service Fabric **Basics** form.</span></span> <span data-ttu-id="a4039-113">A **operációs rendszer**, jelölje be a Windows vagy Linux azt szeretné, hogy a fürt csomópontjai toorun hello hello verzióját.</span><span class="sxs-lookup"><span data-stu-id="a4039-113">For **Operating system**, select hello version of Windows or Linux you want hello cluster nodes toorun.</span></span> <span data-ttu-id="a4039-114">hello felhasználónevét és jelszavát, az itt megadott használt toolog toohello virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="a4039-114">hello user name and password entered here is used toolog in toohello virtual machine.</span></span> <span data-ttu-id="a4039-115">Hozzon létre egy új **Erőforráscsoportot**.</span><span class="sxs-lookup"><span data-stu-id="a4039-115">For **Resource group**, create a new one.</span></span> <span data-ttu-id="a4039-116">Az erőforráscsoport olyan logikai tároló, amelybe a rendszer létrehozza és együttesen kezeli az Azure-erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="a4039-116">A resource group is a logical container into which Azure resources are created and collectively managed.</span></span> <span data-ttu-id="a4039-117">Amikor végzett, kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="a4039-117">When complete, click **OK**.</span></span>

    ![A fürtbeállítás kimenete][cluster-setup-basics]

4. <span data-ttu-id="a4039-119">Töltse ki a hello **fürtkonfiguráció** űrlap.</span><span class="sxs-lookup"><span data-stu-id="a4039-119">Fill out hello **Cluster configuration** form.</span></span>  <span data-ttu-id="a4039-120">A **Csomóponttípusok száma** elemnél adja meg az „1” értéket.</span><span class="sxs-lookup"><span data-stu-id="a4039-120">For **Node type count**, enter "1".</span></span>

5. <span data-ttu-id="a4039-121">Válassza ki **1 (elsődleges) típusú csomópont** és kitöltése a hello **csomópont típuskonfigurációban** űrlap.</span><span class="sxs-lookup"><span data-stu-id="a4039-121">Select **Node type 1 (Primary)** and fill out hello **Node type configuration** form.</span></span>  <span data-ttu-id="a4039-122">A csomóponttípus nevének megadása és hello beállítása [tartóssági szint](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster) túl "bronz."</span><span class="sxs-lookup"><span data-stu-id="a4039-122">Enter a node type name and set hello [Durability tier](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster) too"Bronze."</span></span>  <span data-ttu-id="a4039-123">Válassza ki a virtuális gép méretét.</span><span class="sxs-lookup"><span data-stu-id="a4039-123">Select a VM size.</span></span>

    <span data-ttu-id="a4039-124">Csomóponttípusok hello Virtuálisgép-méretet, a virtuális gépek, egyéni végpontokat számát határozza meg, és más beállításait az adott típusú virtuális gépek hello.</span><span class="sxs-lookup"><span data-stu-id="a4039-124">Node types define hello VM size, number of VMs, custom endpoints, and other settings for hello VMs of that type.</span></span> <span data-ttu-id="a4039-125">Az egyes csomóponttípusok definiált egy különálló virtuális gép méretezési csoport, amely használt toodeploy és felügyelt virtuális gépek egyetlen egységként lett beállítva.</span><span class="sxs-lookup"><span data-stu-id="a4039-125">Each node type defined is set up as a separate virtual machine scale set, which is used toodeploy and managed virtual machines as a set.</span></span> <span data-ttu-id="a4039-126">Mindegyik csomóponttípus egymástól függetlenül skálázható vertikálisan le vagy föl, eltérő nyitott portokkal rendelkezik, és eltérő kapacitásmetrikái lehetnek.</span><span class="sxs-lookup"><span data-stu-id="a4039-126">Each node type can be scaled up or down independently, have different sets of ports open, and can have different capacity metrics.</span></span>  <span data-ttu-id="a4039-127">első vagy elsődleges, csomóponttípus hello, ahol a Service Fabric rendszerszolgáltatások és kell rendelkeznie legalább öt virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="a4039-127">hello first, or primary, node type is where Service Fabric system services are hosted and must have five or more VMs.</span></span>

    <span data-ttu-id="a4039-128">A [kapacitástervezés](service-fabric-cluster-capacity.md) az éles rendszerek üzembe helyezésének lényeges lépése.</span><span class="sxs-lookup"><span data-stu-id="a4039-128">For any production deployment, [capacity planning](service-fabric-cluster-capacity.md) is an important step.</span></span>  <span data-ttu-id="a4039-129">A jelen rövid útmutatóban azonban nem fog alkalmazásokat futtatni, így válassza a *DS1_v2 Standard* VM-méretet.</span><span class="sxs-lookup"><span data-stu-id="a4039-129">For this quick start, however, you aren't running applications so select a *DS1_v2 Standard* VM size.</span></span>  <span data-ttu-id="a4039-130">Válassza ki a "Silver" hello a [megbízhatósági szint](service-fabric-cluster-capacity.md#the-reliability-characteristics-of-the-cluster) és egy kezdeti virtuálisgép-méretezési csoport 5 kapacitását.</span><span class="sxs-lookup"><span data-stu-id="a4039-130">Select "Silver" for hello [reliability tier](service-fabric-cluster-capacity.md#the-reliability-characteristics-of-the-cluster) and an initial virtual machine scale set capacity of 5.</span></span>  

    <span data-ttu-id="a4039-131">Egyéni végpontokat, hogy hello fürtben futó alkalmazások kapcsolatba léphet hello Azure terheléselosztó a portok megnyitása.</span><span class="sxs-lookup"><span data-stu-id="a4039-131">Custom endpoints open up ports in hello Azure load balancer so that you can connect with applications running on hello cluster.</span></span>  <span data-ttu-id="a4039-132">Adja meg a "80, 8172" tooopen mentése 80-as és 8172-es portot.</span><span class="sxs-lookup"><span data-stu-id="a4039-132">Enter "80, 8172" tooopen up ports 80 and 8172.</span></span>

    <span data-ttu-id="a4039-133">Ne ellenőrizze hello **speciális beállítások konfigurálása** jelenik meg, amelyen a TCP/HTTP felügyeleti végpontok alkalmazás porttartományok, testreszabásához használt [placement Constraints korlátozásokat](service-fabric-cluster-resource-manager-configure-services.md#placement-constraints), és [kapacitás Tulajdonságok](service-fabric-cluster-resource-manager-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="a4039-133">Do not check hello **Configure advanced settings** box, which is used for customizing TCP/HTTP management endpoints, application port ranges, [placement constraints](service-fabric-cluster-resource-manager-configure-services.md#placement-constraints), and [capacity properties](service-fabric-cluster-resource-manager-metrics.md).</span></span>    

    <span data-ttu-id="a4039-134">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="a4039-134">Select **OK**.</span></span>

6. <span data-ttu-id="a4039-135">A hello **fürtkonfiguráció** alkotnak, állítsa **diagnosztika** túl**a**.</span><span class="sxs-lookup"><span data-stu-id="a4039-135">In hello **Cluster configuration** form, set **Diagnostics** too**On**.</span></span>  <span data-ttu-id="a4039-136">A gyors üzembe helyezés, nem kell tooenter minden [beállítás háló](service-fabric-cluster-fabric-settings.md) tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="a4039-136">For this quickstart, you do not need tooenter any [fabric setting](service-fabric-cluster-fabric-settings.md) properties.</span></span>  <span data-ttu-id="a4039-137">A **Fabric-verzió**, jelölje be **automatikus** frissítési mód, hogy a Microsoft automatikusan frissíti a hello fürtön futó hello háló kód hello verzióját.</span><span class="sxs-lookup"><span data-stu-id="a4039-137">In **Fabric version**, select **Automatic** upgrade mode so that Microsoft automatically updates hello version of hello fabric code running hello cluster.</span></span>  <span data-ttu-id="a4039-138">Hello mód beállítása túl**manuális** Ha túl[válasszon egy támogatott verzióját](service-fabric-cluster-upgrade.md) tooupgrade számára.</span><span class="sxs-lookup"><span data-stu-id="a4039-138">Set hello mode too**Manual** if you want too[choose a supported version](service-fabric-cluster-upgrade.md) tooupgrade to.</span></span> 

    ![Csomóponttípus konfigurálása][node-type-config]

    <span data-ttu-id="a4039-140">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="a4039-140">Select **OK**.</span></span>

7. <span data-ttu-id="a4039-141">Töltse ki a hello **biztonsági** űrlap.</span><span class="sxs-lookup"><span data-stu-id="a4039-141">Fill out hello **Security** form.</span></span>  <span data-ttu-id="a4039-142">Ehhez a rövid útmutatóhoz válassza a **Nem biztonságos** beállítást.</span><span class="sxs-lookup"><span data-stu-id="a4039-142">For this quick start select **Unsecure**.</span></span>  <span data-ttu-id="a4039-143">Nagyon fontos ajánlott toocreate a termelési számítási feladatokhoz, biztonságos fürt, bárki névtelenül csatlakoztassa tooan nem biztonságos fürtöt és felügyeleti műveletek végrehajtása óta.</span><span class="sxs-lookup"><span data-stu-id="a4039-143">It is highly recommended toocreate a secure cluster for production workloads, however, since anyone can anonymously connect tooan unsecure cluster and perform management operations.</span></span>  

    <span data-ttu-id="a4039-144">A rendszer tanúsítványokat használ a Service Fabric tooprovide hitelesítési és titkosítási toosecure különböző szempontjairól a fürt és az alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="a4039-144">Certificates are used in Service Fabric tooprovide authentication and encryption toosecure various aspects of a cluster and its applications.</span></span> <span data-ttu-id="a4039-145">A tanúsítványok Service Fabricban való használatával kapcsolatos további információkért lásd a [Service Fabric-fürtök biztonsági forgatókönyveit](service-fabric-cluster-security.md).</span><span class="sxs-lookup"><span data-stu-id="a4039-145">For more information on how certificates are used in Service Fabric, see [Service Fabric cluster security scenarios](service-fabric-cluster-security.md).</span></span>  <span data-ttu-id="a4039-146">Azure Active Directory vagy a szükséges tanúsítványok tooset használatával az alkalmazásbiztonság, tooenable felhasználóhitelesítés [fürt létrehozása a Resource Manager-sablon](service-fabric-cluster-creation-via-arm.md).</span><span class="sxs-lookup"><span data-stu-id="a4039-146">tooenable user authentication using Azure Active Directory or tooset up certificates for application security, [create a cluster from a Resource Manager template](service-fabric-cluster-creation-via-arm.md).</span></span>

    <span data-ttu-id="a4039-147">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="a4039-147">Select **OK**.</span></span>

8. <span data-ttu-id="a4039-148">Tekintse át a hello összegzése.</span><span class="sxs-lookup"><span data-stu-id="a4039-148">Review hello summary.</span></span>  <span data-ttu-id="a4039-149">Ha azt szeretné, hogy toodownload megadott beépített hello-beállítások a Resource Manager-sablon, jelölje be **töltse le a sablon és a paraméterek**.</span><span class="sxs-lookup"><span data-stu-id="a4039-149">If you'd like toodownload a Resource Manager template built from hello settings you entered, select **Download template and parameters**.</span></span>  <span data-ttu-id="a4039-150">Válassza ki **létrehozása** toocreate hello fürt.</span><span class="sxs-lookup"><span data-stu-id="a4039-150">Select **Create** toocreate hello cluster.</span></span>

    <span data-ttu-id="a4039-151">Hello létrehozásának folyamata hello értesítések tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="a4039-151">You can see hello creation progress in hello notifications.</span></span> <span data-ttu-id="a4039-152">(Kattintson a hello "harang" ikonra hello állapotsorban hello jobb felső sarkában a képernyőn közelében.) Kattintott **PIN-kód tooStartboard** hello fürt létrehozásakor, lásd: **Fabric-fürt üzembe helyezése** toohello rögzítve **Start** tábla.</span><span class="sxs-lookup"><span data-stu-id="a4039-152">(Click hello "Bell" icon near hello status bar at hello upper right of your screen.) If you clicked **Pin tooStartboard** while creating hello cluster, you see **Deploying Service Fabric Cluster** pinned toohello **Start** board.</span></span>

### <a name="view-cluster-status"></a><span data-ttu-id="a4039-153">Fürt állapotának megtekintése</span><span class="sxs-lookup"><span data-stu-id="a4039-153">View cluster status</span></span>
<span data-ttu-id="a4039-154">A fürt létrehozása után vizsgálhatja meg a fürt hello **áttekintése** hello portál panel.</span><span class="sxs-lookup"><span data-stu-id="a4039-154">Once your cluster is created, you can inspect your cluster in hello **Overview** blade in hello portal.</span></span> <span data-ttu-id="a4039-155">Most már megtekintheti a fürt hello irányítópulton, beleértve a nyilvános végpontot hello fürt és a hivatkozás tooService Fabric Explorer hello részleteit.</span><span class="sxs-lookup"><span data-stu-id="a4039-155">You can now see hello details of your cluster in hello dashboard, including hello cluster's public endpoint and a link tooService Fabric Explorer.</span></span>

![Fürt állapota][cluster-status]

### <a name="visualize-hello-cluster-using-service-fabric-explorer"></a><span data-ttu-id="a4039-157">A Service Fabric Explorerrel hello fürt megjelenítése</span><span class="sxs-lookup"><span data-stu-id="a4039-157">Visualize hello cluster using Service Fabric explorer</span></span>
<span data-ttu-id="a4039-158">A [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) hatékony eszköz a fürtök megjelenítéséhez és az alkalmazások kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="a4039-158">[Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) is a good tool for visualizing your cluster and managing applications.</span></span>  <span data-ttu-id="a4039-159">Service Fabric Explorerben talál egy olyan szolgáltatás, hello fürtben futó.</span><span class="sxs-lookup"><span data-stu-id="a4039-159">Service Fabric Explorer is a service that runs in hello cluster.</span></span>  <span data-ttu-id="a4039-160">Hozzáférési jogosultsága hello a webböngésző használatával **Service Fabric Explorer** hello fürt hivatkozás **áttekintése** hello lapjára.</span><span class="sxs-lookup"><span data-stu-id="a4039-160">Access it using a web browser by clicking hello **Service Fabric Explorer** link of hello cluster **Overview** page in hello portal.</span></span>  <span data-ttu-id="a4039-161">Hello címet adja meg közvetlenül hello böngészőbe: [http://quickstartcluster.westus.cloudapp.azure.com:19080/Explorer](http://quickstartcluster.westus.cloudapp.azure.com:19080/Explorer)</span><span class="sxs-lookup"><span data-stu-id="a4039-161">You can also enter hello address directly into hello browser: [http://quickstartcluster.westus.cloudapp.azure.com:19080/Explorer](http://quickstartcluster.westus.cloudapp.azure.com:19080/Explorer)</span></span>

<span data-ttu-id="a4039-162">hello fürt irányítópult áttekintése a fürt, beleértve az alkalmazás és a csomópont állapotának összegzését.</span><span class="sxs-lookup"><span data-stu-id="a4039-162">hello cluster dashboard provides an overview of your cluster, including a summary of application and node health.</span></span> <span data-ttu-id="a4039-163">hello csomópont nézetben látható hello hello fürt fizikai elrendezését.</span><span class="sxs-lookup"><span data-stu-id="a4039-163">hello node view shows hello physical layout of hello cluster.</span></span> <span data-ttu-id="a4039-164">Az egyes csomópontoknál megtekintheti, hogy melyik alkalmazások kódja üzemel az adott csomóponton.</span><span class="sxs-lookup"><span data-stu-id="a4039-164">For a given node, you can inspect which applications have code deployed on that node.</span></span>

![Service Fabric Explorer][service-fabric-explorer]

### <a name="connect-toohello-cluster-using-powershell"></a><span data-ttu-id="a4039-166">Csatlakozás toohello fürt PowerShell-lel</span><span class="sxs-lookup"><span data-stu-id="a4039-166">Connect toohello cluster using PowerShell</span></span>
<span data-ttu-id="a4039-167">Győződjön meg arról, hogy hello fürt fut. csatlakozzon a PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="a4039-167">Verify that hello cluster is running by connecting using PowerShell.</span></span>  <span data-ttu-id="a4039-168">hello ServiceFabric PowerShell modul telepítve van a hello [Service Fabric SDK](service-fabric-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="a4039-168">hello ServiceFabric PowerShell module is installed with hello [Service Fabric SDK](service-fabric-get-started.md).</span></span>  <span data-ttu-id="a4039-169">Hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) parancsmag létesít kapcsolatot toohello fürt.</span><span class="sxs-lookup"><span data-stu-id="a4039-169">hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet establishes a connection toohello cluster.</span></span>   

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint quickstartcluster.westus2.cloudapp.azure.com:19000
```
<span data-ttu-id="a4039-170">Lásd: [Connect tooa biztonságos fürt](service-fabric-connect-to-secure-cluster.md) csatlakozó tooa fürt más példákat.</span><span class="sxs-lookup"><span data-stu-id="a4039-170">See [Connect tooa secure cluster](service-fabric-connect-to-secure-cluster.md) for other examples of connecting tooa cluster.</span></span> <span data-ttu-id="a4039-171">Miután toohello fürthöz kapcsolódó, használja a hello [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) parancsmag toodisplay hello fürt és a állapot információt az egyes csomópontok csomópontok listáját.</span><span class="sxs-lookup"><span data-stu-id="a4039-171">After connecting toohello cluster, use hello [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) cmdlet toodisplay a list of nodes in hello cluster and status information for each node.</span></span> <span data-ttu-id="a4039-172">A **HealthState** tulajdonságnak *OK* értékűnek kell lennie minden csomópont esetében.</span><span class="sxs-lookup"><span data-stu-id="a4039-172">**HealthState** should be *OK* for each node.</span></span>

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

### <a name="remove-hello-cluster"></a><span data-ttu-id="a4039-173">Hello fürt eltávolítása</span><span class="sxs-lookup"><span data-stu-id="a4039-173">Remove hello cluster</span></span>
<span data-ttu-id="a4039-174">A Service Fabric-fürt épül fel más Azure-erőforrások továbbá toohello fürterőforrás magát.</span><span class="sxs-lookup"><span data-stu-id="a4039-174">A Service Fabric cluster is made up of other Azure resources in addition toohello cluster resource itself.</span></span> <span data-ttu-id="a4039-175">Így toocompletely a Service Fabric-fürt törlése szükség toodelete összes hello történik, az erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="a4039-175">So toocompletely delete a Service Fabric cluster you also need toodelete all hello resources it is made of.</span></span> <span data-ttu-id="a4039-176">hello legegyszerűbb módja toodelete hello fürt összes hello erőforrások pedig toodelete hello erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="a4039-176">hello simplest way toodelete hello cluster and all hello resources it consumes is toodelete hello resource group.</span></span> <span data-ttu-id="a4039-177">Más módokon toodelete egy fürtöt vagy toodelete néhány (de nem minden) hello erőforrások az erőforráscsoportban, tekintse meg a [egy fürt törlése](service-fabric-cluster-delete.md)</span><span class="sxs-lookup"><span data-stu-id="a4039-177">For other ways toodelete a cluster or toodelete some (but not all) hello resources in a resource group, see [Delete a cluster](service-fabric-cluster-delete.md)</span></span>

<span data-ttu-id="a4039-178">Az Azure-portálon hello erőforráscsoport törlése:</span><span class="sxs-lookup"><span data-stu-id="a4039-178">Delete a resource group in hello Azure portal:</span></span>
1. <span data-ttu-id="a4039-179">Keresse meg a kívánt toodelete toohello Service Fabric-fürt.</span><span class="sxs-lookup"><span data-stu-id="a4039-179">Navigate toohello Service Fabric cluster you want toodelete.</span></span>
2. <span data-ttu-id="a4039-180">Kattintson a hello **erőforráscsoport** nevű hello fürt alapvető erőforrások lapon.</span><span class="sxs-lookup"><span data-stu-id="a4039-180">Click hello **Resource Group** name on hello cluster essentials page.</span></span>
3. <span data-ttu-id="a4039-181">A hello **erőforrás csoport Essentials** kattintson **erőforrás csoport törlése** és az adott lapon toocomplete hello törlési hello erőforráscsoport hello útmutatás.</span><span class="sxs-lookup"><span data-stu-id="a4039-181">In hello **Resource Group Essentials** page, click **Delete resource group** and follow hello instructions on that page toocomplete hello deletion of hello resource group.</span></span>
    <span data-ttu-id="a4039-182">![Hello erőforráscsoport törlése][cluster-delete]</span><span class="sxs-lookup"><span data-stu-id="a4039-182">![Delete hello resource group][cluster-delete]</span></span>


## <a name="use-azure-powershell-toodeploy-a-secure-cluster"></a><span data-ttu-id="a4039-183">Azure Powershell toodeploy biztonságos fürt használatára</span><span class="sxs-lookup"><span data-stu-id="a4039-183">Use Azure Powershell toodeploy a secure cluster</span></span>
1. <span data-ttu-id="a4039-184">Töltse le a hello [Azure Powershell 4.0-s vagy újabb verziója](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="a4039-184">Download hello [Azure Powershell module version 4.0 or higher](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) on your machine.</span></span>

2. <span data-ttu-id="a4039-185">Nyisson meg egy Windows PowerShell ablakot, a következő parancs futtatása hello.</span><span class="sxs-lookup"><span data-stu-id="a4039-185">Open a Windows PowerShell window, Run hello following command.</span></span> 
    
    ```powershell

    Get-Command -Module AzureRM.ServiceFabric 
    ```

    <span data-ttu-id="a4039-186">Meg kell jelennie egy kimeneti hasonló toohello következő.</span><span class="sxs-lookup"><span data-stu-id="a4039-186">You should see an output similar toohello following.</span></span>

    ![ps-list][ps-list]

3. <span data-ttu-id="a4039-188">Bejelentkezési tooAzure és Select hello előfizetés toowhich toocreate hello fürt</span><span class="sxs-lookup"><span data-stu-id="a4039-188">Login tooAzure and Select hello subscription toowhich you want toocreate hello cluster</span></span>

    ```powershell

    Login-AzureRmAccount

    Select-AzureRmSubscription -SubscriptionId "Subcription ID" 
    ```

4. <span data-ttu-id="a4039-189">Futtassa a következő parancs toonow hello biztonságos fürt létrehozása.</span><span class="sxs-lookup"><span data-stu-id="a4039-189">Run hello following command toonow create a secure cluster.</span></span> <span data-ttu-id="a4039-190">Ne feledje toocustomize hello paraméterek.</span><span class="sxs-lookup"><span data-stu-id="a4039-190">Do not forget toocustomize hello parameters.</span></span> 

    ```powershell
    $certpwd="Password#1234" | ConvertTo-SecureString -AsPlainText -Force
    $RDPpwd="Password#1234" | ConvertTo-SecureString -AsPlainText -Force 
    $RDPuser="vmadmin"
    $RGname="mycluster" # this is also hello name of your cluster
    $clusterloc="SouthCentralUS"
    $subname="$RGname.$clusterloc.cloudapp.azure.com"
    $certfolder="c:\mycertificates\"
    $clustersize=1 # can take values 1, 3-99

    New-AzureRmServiceFabricCluster -ResourceGroupName $RGname -Location $clusterloc -ClusterSize $clustersize -VmUserName $RDPuser -VmPassword $RDPpwd -CertificateSubjectName $subname -CertificatePassword $certpwd -CertificateOutputFolder $certfolder
    ```

    <span data-ttu-id="a4039-191">Hello parancsot is igénybe vehet 10 perc too30 perc toocomplete, azt a hello végén, egy kimeneti hasonló toohello következő kapja meg.</span><span class="sxs-lookup"><span data-stu-id="a4039-191">hello command can take anywhere from 10 minutes too30 minutes toocomplete, at hello end of it, you should get an output similar toohello following.</span></span> <span data-ttu-id="a4039-192">hello kimeneti hello tanúsítvány, ahol töltöttek fel, KeyVault hello adatait és a helyi mappát, amelybe a hello tanúsítvány van másolja hello.</span><span class="sxs-lookup"><span data-stu-id="a4039-192">hello output has information about hello certificate, hello KeyVault where it was uploaded to, and hello local folder where hello certificate is copied.</span></span> 

    ![ps-out][ps-out]

5. <span data-ttu-id="a4039-194">Hello teljes kimenetének másolása HTML-, és mentse a szövegfájlt tooa toorefer tooit kell.</span><span class="sxs-lookup"><span data-stu-id="a4039-194">Copy hello entire output and save tooa text file as we need toorefer tooit.</span></span> <span data-ttu-id="a4039-195">Jegyezze fel a következő információ hello kimenetből hello.</span><span class="sxs-lookup"><span data-stu-id="a4039-195">Make a note of hello following information from hello output.</span></span> 

    - <span data-ttu-id="a4039-196">**CertificateSavedLocalPath** : c:\mycertificates\mycluster20170504141137.pfx</span><span class="sxs-lookup"><span data-stu-id="a4039-196">**CertificateSavedLocalPath** : c:\mycertificates\mycluster20170504141137.pfx</span></span>
    - <span data-ttu-id="a4039-197">**CertificateThumbprint** : C4C1E541AD512B8065280292A8BA6079C3F26F10</span><span class="sxs-lookup"><span data-stu-id="a4039-197">**CertificateThumbprint** : C4C1E541AD512B8065280292A8BA6079C3F26F10</span></span>
    - <span data-ttu-id="a4039-198">**ManagementEndpoint** : https://mycluster.southcentralus.cloudapp.azure.com:19080</span><span class="sxs-lookup"><span data-stu-id="a4039-198">**ManagementEndpoint** : https://mycluster.southcentralus.cloudapp.azure.com:19080</span></span>
    - <span data-ttu-id="a4039-199">**ClientConnectionEndpointPort** : 19000</span><span class="sxs-lookup"><span data-stu-id="a4039-199">**ClientConnectionEndpointPort** : 19000</span></span>

### <a name="install-hello-certificate-on-your-local-machine"></a><span data-ttu-id="a4039-200">A helyi gépen hello tanúsítvány telepítése</span><span class="sxs-lookup"><span data-stu-id="a4039-200">Install hello certificate on your local machine</span></span>
  
<span data-ttu-id="a4039-201">tooconnect toohello fürt, tanúsítványra van szükség tooinstall hello hello aktuális felhasználó hello (a) személyes tárolóba.</span><span class="sxs-lookup"><span data-stu-id="a4039-201">tooconnect toohello cluster, you need tooinstall hello certificate into hello Personal (My) store of hello current user.</span></span> 

<span data-ttu-id="a4039-202">Futtassa a következő PowerShell hello</span><span class="sxs-lookup"><span data-stu-id="a4039-202">Run hello following PowerShell</span></span>

```powershell
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My `
        -FilePath C:\mycertificates\hello name of hello cert.pfx `
        -Password (ConvertTo-SecureString -String certpwd -AsPlainText -Force)
```

<span data-ttu-id="a4039-203">Most már áll készen tooconnect tooyour biztonságos fürt.</span><span class="sxs-lookup"><span data-stu-id="a4039-203">You are now ready tooconnect tooyour secure cluster.</span></span>

### <a name="connect-tooa-secure-cluster"></a><span data-ttu-id="a4039-204">Csatlakoztassa tooa biztonságos fürtöt</span><span class="sxs-lookup"><span data-stu-id="a4039-204">Connect tooa secure cluster</span></span> 

<span data-ttu-id="a4039-205">Futtassa a következő PowerShell parancs tooconnect tooa biztonságos fürt hello.</span><span class="sxs-lookup"><span data-stu-id="a4039-205">Run hello following PowerShell command tooconnect tooa secure cluster.</span></span> <span data-ttu-id="a4039-206">hello Tanúsítványadatok meg kell egyeznie egy tanúsítványt, de a használt tooset be hello fürtöt.</span><span class="sxs-lookup"><span data-stu-id="a4039-206">hello certificate details must match a certificate that was used tooset up hello cluster.</span></span> 

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint <Certificate Thumbprint> `
          -FindType FindByThumbprint -FindValue <Certificate Thumbprint> `
          -StoreLocation CurrentUser -StoreName My
```


<span data-ttu-id="a4039-207">a következő példa azt mutatja be hello hello paraméterek befejeződött:</span><span class="sxs-lookup"><span data-stu-id="a4039-207">hello following example shows hello completed parameters:</span></span> 

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint mycluster.southcentralus.cloudapp.azure.com:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -FindType FindByThumbprint -FindValue C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -StoreLocation CurrentUser -StoreName My
```

<span data-ttu-id="a4039-208">Futtassa a következő parancs toocheck, hogy csatlakozott hello, és hello fürt megfelelő állapotban.</span><span class="sxs-lookup"><span data-stu-id="a4039-208">Run hello following command toocheck that you are connected and hello cluster is healthy.</span></span>

```powershell

Get-ServiceFabricClusterHealth

```
### <a name="publish-your-apps-tooyour-cluster-from-visual-studio"></a><span data-ttu-id="a4039-209">Visual Studio tooyour fürtöt alkalmazások közzététele</span><span class="sxs-lookup"><span data-stu-id="a4039-209">Publish your apps tooyour cluster from Visual Studio</span></span>

<span data-ttu-id="a4039-210">Most, hogy állította be az Azure-fürttel, közzéteheti a alkalmazások tooit Visual Studio által következő hello [közzététel tooan fürt](service-fabric-publish-app-remote-cluster.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="a4039-210">Now that you have set up an Azure cluster, you can publish your applications tooit from Visual Studio by following hello [Publish tooan cluster](service-fabric-publish-app-remote-cluster.md) document.</span></span> 

### <a name="remove-hello-cluster"></a><span data-ttu-id="a4039-211">Hello fürt eltávolítása</span><span class="sxs-lookup"><span data-stu-id="a4039-211">Remove hello cluster</span></span>
<span data-ttu-id="a4039-212">A fürt épül fel más Azure-erőforrások továbbá toohello fürterőforrás magát.</span><span class="sxs-lookup"><span data-stu-id="a4039-212">A cluster is made up of other Azure resources in addition toohello cluster resource itself.</span></span> <span data-ttu-id="a4039-213">hello legegyszerűbb módja toodelete hello fürt összes hello erőforrások pedig toodelete hello erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="a4039-213">hello simplest way toodelete hello cluster and all hello resources it consumes is toodelete hello resource group.</span></span> 

```powershell

Remove-AzureRmResourceGroup -Name $RGname -Force

```

## <a name="next-steps"></a><span data-ttu-id="a4039-214">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a4039-214">Next steps</span></span>
<span data-ttu-id="a4039-215">Most, hogy úgy állította be a fejlesztési fürtöt, próbálkozzon a hello következő:</span><span class="sxs-lookup"><span data-stu-id="a4039-215">Now that you have set up a development cluster, try hello following:</span></span>
* [<span data-ttu-id="a4039-216">Hozzon létre egy biztonságos fürt hello portálon</span><span class="sxs-lookup"><span data-stu-id="a4039-216">Create a secure cluster in hello portal</span></span>](service-fabric-cluster-creation-via-portal.md)
* [<span data-ttu-id="a4039-217">Fürt létrehozása sablonból</span><span class="sxs-lookup"><span data-stu-id="a4039-217">Create a cluster from a template</span></span>](service-fabric-cluster-creation-via-arm.md) 
* [<span data-ttu-id="a4039-218">Appokat helyezhet üzembe a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="a4039-218">Deploy apps using PowerShell</span></span>](service-fabric-deploy-remove-applications.md)


[cluster-setup-basics]: ./media/service-fabric-get-started-azure-cluster/basics.png
[node-type-config]: ./media/service-fabric-get-started-azure-cluster/nodetypeconfig.png
[cluster-status]: ./media/service-fabric-get-started-azure-cluster/clusterstatus.png
[service-fabric-explorer]: ./media/service-fabric-get-started-azure-cluster/sfx.png
[cluster-delete]: ./media/service-fabric-get-started-azure-cluster/delete.png
[ps-list]: ./media/service-fabric-get-started-azure-cluster/pslist.PNG
[ps-out]: ./media/service-fabric-get-started-azure-cluster/psout.PNG
