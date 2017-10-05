---
title: "A Service Fabric csomóponttípusok és a Virtuálisgép-méretezési készlet |} Microsoft Docs"
description: "Útmutatás a Service Fabric csomóponttípusok Virtuálisgép-méretezési készlet kapcsolódnak és hogyan távolról csatlakozni a Virtuálisgép-méretezési csoportban példányt vagy egy fürt csomópontja."
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: timlt
editor: 
ms.assetid: 5441e7e0-d842-4398-b060-8c9d34b07c48
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/05/2017
ms.author: chackdan
ms.openlocfilehash: 3b1a22bb3653abb68fc73645ad2cb623fabc7736
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="the-relationship-between-service-fabric-node-types-and-virtual-machine-scale-sets"></a><span data-ttu-id="573a0-103">A Service Fabric csomóponttípusok és a virtuálisgép-méretezési csoportok közötti kapcsolat</span><span class="sxs-lookup"><span data-stu-id="573a0-103">The relationship between Service Fabric node types and Virtual Machine Scale Sets</span></span>
<span data-ttu-id="573a0-104">Virtuálisgép-méretezési csoportok olyan Azure számítási erőforrás a telepíthetnek és kezelhetnek olyan virtuális gépek gyűjteménye.</span><span class="sxs-lookup"><span data-stu-id="573a0-104">Virtual Machine Scale Sets are an Azure Compute resource you can use to deploy and manage a collection of virtual machines as a set.</span></span> <span data-ttu-id="573a0-105">Minden csomópont-típus, a Service Fabric-fürt definiált egy külön Virtuálisgép-méretezési csoportban lett beállítva.</span><span class="sxs-lookup"><span data-stu-id="573a0-105">Every node type that is defined in a Service Fabric cluster is set up as a separate VM Scale Set.</span></span> <span data-ttu-id="573a0-106">Az egyes csomóponttípusok akár majd is méretezhető vagy rendelkezik egymástól függetlenül, a portok megnyitása más-más részhalmazához le, és különböző teljesítmény-mérőszámait lehet.</span><span class="sxs-lookup"><span data-stu-id="573a0-106">Each node type can then be scaled up or down independently, have different sets of ports open, and can have different capacity metrics.</span></span>

<span data-ttu-id="573a0-107">Az alábbi képernyőfelvételen látható egy fürt, amely két csomópont tartozik: előtér- és háttérszolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="573a0-107">The following screen shot shows a cluster that has two node types: FrontEnd and BackEnd.</span></span>  <span data-ttu-id="573a0-108">Minden csomópont az öt csomóponttal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="573a0-108">Each node type has five nodes each.</span></span>

![Képernyőfelvétel egy fürt, amely két csomópont tartozik][NodeTypes]

## <a name="mapping-vm-scale-set-instances-to-nodes"></a><span data-ttu-id="573a0-110">Virtuálisgép-méretezési csoportban példányok leképezése-csomópontokra</span><span class="sxs-lookup"><span data-stu-id="573a0-110">Mapping VM Scale Set instances to nodes</span></span>
<span data-ttu-id="573a0-111">Ahogy fent látja, a Virtuálisgép-méretezési csoportban példányok indítsa el példányból 0 és be is megnőnek.</span><span class="sxs-lookup"><span data-stu-id="573a0-111">As you can see above, the VM Scale Set instances start from instance 0 and then goes up.</span></span> <span data-ttu-id="573a0-112">A nevek számozására is megjelenik.</span><span class="sxs-lookup"><span data-stu-id="573a0-112">The numbering is reflected in the names.</span></span> <span data-ttu-id="573a0-113">Például a BackEnd_0 0 példánya a háttérrendszer Virtuálisgép-méretezési csoportban.</span><span class="sxs-lookup"><span data-stu-id="573a0-113">For example, BackEnd_0 is instance 0 of the BackEnd VM Scale Set.</span></span> <span data-ttu-id="573a0-114">Öt példánya, BackEnd_0, BackEnd_1, BackEnd_2, BackEnd_3 és BackEnd_4 az adott Virtuálisgép-méretezési csoportban van.</span><span class="sxs-lookup"><span data-stu-id="573a0-114">This particular VM Scale Set has five instances, named BackEnd_0, BackEnd_1, BackEnd_2, BackEnd_3 and BackEnd_4.</span></span>

<span data-ttu-id="573a0-115">Ha Ön növelheti a Virtuálisgép-méretezési csoportban egy új példány jön létre.</span><span class="sxs-lookup"><span data-stu-id="573a0-115">When you scale up a VM Scale Set a new instance is created.</span></span> <span data-ttu-id="573a0-116">Az új Virtuálisgép-méretezési csoportban példány általában lesz, a Virtuálisgép-méretezési csoportban neve + a következő példányok számát.</span><span class="sxs-lookup"><span data-stu-id="573a0-116">The new VM Scale Set instance name will typically be the VM Scale Set name + the next instance number.</span></span> <span data-ttu-id="573a0-117">A jelen példában BackEnd_5.</span><span class="sxs-lookup"><span data-stu-id="573a0-117">In our example, it is BackEnd_5.</span></span>

## <a name="mapping-vm-scale-set-load-balancers-to-each-node-typevm-scale-set"></a><span data-ttu-id="573a0-118">Virtuálisgép-méretezési leképezési terheléselosztók csomópont típus/virtuális gépek méretezési beállítása</span><span class="sxs-lookup"><span data-stu-id="573a0-118">Mapping VM scale set load balancers to each node type/VM Scale Set</span></span>
<span data-ttu-id="573a0-119">Ha telepítette a fürtöt a portálról, vagy a minta Resource Manager-sablon, amely azt megadva, majd amikor egy erőforráscsoportba tartozó összes erőforrás listáját használja majd látni fogja az egyes Virtuálisgép-méretezési csoportban vagy csomópont azokat a terheléselosztókat.</span><span class="sxs-lookup"><span data-stu-id="573a0-119">If you have deployed your cluster from the portal or have used the sample Resource Manager template that we provided, then when you get a list of all resources under a Resource Group then you will see the load balancers for each VM Scale Set or node type.</span></span>

<span data-ttu-id="573a0-120">A név lesz hasonlót: **LB -&lt;NodeType neve&gt;**.</span><span class="sxs-lookup"><span data-stu-id="573a0-120">The name would something like: **LB-&lt;NodeType name&gt;**.</span></span> <span data-ttu-id="573a0-121">Például LB-sfcluster4doc-0, a képernyőfelvételen látható módon:</span><span class="sxs-lookup"><span data-stu-id="573a0-121">For example, LB-sfcluster4doc-0, as shown in this screenshot:</span></span>

![Erőforrások][Resources]

## <a name="remote-connect-to-a-vm-scale-set-instance-or-a-cluster-node"></a><span data-ttu-id="573a0-123">Távoli kapcsolódás egy Virtuálisgép-méretezési csoportban példányt vagy egy fürt csomópontja</span><span class="sxs-lookup"><span data-stu-id="573a0-123">Remote connect to a VM Scale Set instance or a cluster node</span></span>
<span data-ttu-id="573a0-124">Minden csomópont-típus, egy fürt definiált egy külön Virtuálisgép-méretezési csoportban lett beállítva.</span><span class="sxs-lookup"><span data-stu-id="573a0-124">Every Node type that is defined in a cluster is set up as a separate VM Scale Set.</span></span>  <span data-ttu-id="573a0-125">Ez azt jelenti, hogy azok a csomóponttípusok méretezhetők akár vagy egymástól függetlenül le, és különböző VM termékváltozatok teszik lehetővé.</span><span class="sxs-lookup"><span data-stu-id="573a0-125">That means the node types can be scaled up or down independently and can be made of different VM SKUs.</span></span> <span data-ttu-id="573a0-126">Egypéldányos virtuális gépeket, ellentétben a Virtuálisgép-méretezési csoport példányai nem kérdezhető le egy virtuális IP-címet saját.</span><span class="sxs-lookup"><span data-stu-id="573a0-126">Unlike single instance VMs, the VM Scale Set instances do not get a virtual IP address of their own.</span></span> <span data-ttu-id="573a0-127">Ezért ez lehet egy kicsit kihívást keresett IP-címet és portot, amelyet segítségével távoli csatlakozás egy adott példányt.</span><span class="sxs-lookup"><span data-stu-id="573a0-127">So it can be a bit challenging when you are looking for an IP address and port that you can use to remote connect to a specific instance.</span></span>

<span data-ttu-id="573a0-128">Az alábbiakban a lépéseket, amelyeket követve derítheti fel azokat.</span><span class="sxs-lookup"><span data-stu-id="573a0-128">Here are the steps you can follow to discover them.</span></span>

### <a name="step-1-find-out-the-virtual-ip-address-for-the-node-type-and-then-inbound-nat-rules-for-rdp"></a><span data-ttu-id="573a0-129">1. lépés: A virtuális IP-címet a csomóponttípus és bejövő NAT-szabályokat RDP megállapítása</span><span class="sxs-lookup"><span data-stu-id="573a0-129">Step 1: Find out the virtual IP address for the node type and then Inbound NAT rules for RDP</span></span>
<span data-ttu-id="573a0-130">Ahhoz, hogy beolvasása, amely, le kell töltenie a bejövő NAT szabályok értékek az erőforrás-definíciójának részeként meghatározott **Microsoft.Network/loadBalancers**.</span><span class="sxs-lookup"><span data-stu-id="573a0-130">In order to get that, you need to get the inbound NAT rules values that were defined as a part of the resource definition for **Microsoft.Network/loadBalancers**.</span></span>

<span data-ttu-id="573a0-131">A portálon lépjen a Load balancer paneljét, majd **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="573a0-131">In the portal, navigate to the Load balancer blade and then **Settings**.</span></span>

![LBBlade][LBBlade]

<span data-ttu-id="573a0-133">A **beállítások**, kattintson a **bejövő forgalmat kezelő NAT-szabályok**.</span><span class="sxs-lookup"><span data-stu-id="573a0-133">In **Settings**, click on **Inbound NAT rules**.</span></span> <span data-ttu-id="573a0-134">Ez most nyújt az IP-címet és portot, amelyet segítségével távoli csatlakozás az első Virtuálisgép-méretezési csoportban példányhoz.</span><span class="sxs-lookup"><span data-stu-id="573a0-134">This now gives you the IP address and port that you can use to remote connect to the first VM Scale Set instance.</span></span> <span data-ttu-id="573a0-135">Az alábbi képernyőképen látható, hogy a rendszer **104.42.106.156** és **3389-es**</span><span class="sxs-lookup"><span data-stu-id="573a0-135">In the screenshot below, it is **104.42.106.156** and **3389**</span></span>

![NATRules][NATRules]

### <a name="step-2-find-out-the-port-that-you-can-use-to-remote-connect-to-the-specific-vm-scale-set-instancenode"></a><span data-ttu-id="573a0-137">2. lépés: Kibővített a portot, amelynek távoli csatlakozás a megadott Virtuálisgép-méretezési csoportban példány/csomópont keresése</span><span class="sxs-lookup"><span data-stu-id="573a0-137">Step 2: Find out the port that you can use to remote connect to the specific VM Scale Set instance/node</span></span>
<span data-ttu-id="573a0-138">A jelen dokumentum korábbi I arról volt szó, hogyan a Virtuálisgép-méretezési csoportban példányok hozzárendelését a csomópontok.</span><span class="sxs-lookup"><span data-stu-id="573a0-138">Earlier in this document, I talked about how the VM Scale Set instances map to the nodes.</span></span> <span data-ttu-id="573a0-139">Használjuk, amely mérje fel, a pontos port.</span><span class="sxs-lookup"><span data-stu-id="573a0-139">We will use that to figure out the exact port.</span></span>

<span data-ttu-id="573a0-140">A portok növekvő sorrendben a Virtuálisgép-méretezési csoportban példány foglal le.</span><span class="sxs-lookup"><span data-stu-id="573a0-140">The ports are allocated in ascending order of the VM Scale Set instance.</span></span> <span data-ttu-id="573a0-141">úgy, hogy a példában a FrontEnd csomóponttípus, a portok az egyes öt példánya a következő.</span><span class="sxs-lookup"><span data-stu-id="573a0-141">so in my example for the FrontEnd node type, the ports for each of the five instances are the following.</span></span> <span data-ttu-id="573a0-142">most kell tennie, hogy a Virtuálisgép-méretezési csoportban példány azonosak maradjanak.</span><span class="sxs-lookup"><span data-stu-id="573a0-142">you now need to do the same mapping for your VM Scale Set instance.</span></span>

| <span data-ttu-id="573a0-143">**Virtuálisgép-méretezési készlet példány**</span><span class="sxs-lookup"><span data-stu-id="573a0-143">**VM Scale Set Instance**</span></span> | <span data-ttu-id="573a0-144">**Port**</span><span class="sxs-lookup"><span data-stu-id="573a0-144">**Port**</span></span> |
| --- | --- |
| <span data-ttu-id="573a0-145">FrontEnd_0</span><span class="sxs-lookup"><span data-stu-id="573a0-145">FrontEnd_0</span></span> |<span data-ttu-id="573a0-146">3389</span><span class="sxs-lookup"><span data-stu-id="573a0-146">3389</span></span> |
| <span data-ttu-id="573a0-147">FrontEnd_1</span><span class="sxs-lookup"><span data-stu-id="573a0-147">FrontEnd_1</span></span> |<span data-ttu-id="573a0-148">3390</span><span class="sxs-lookup"><span data-stu-id="573a0-148">3390</span></span> |
| <span data-ttu-id="573a0-149">FrontEnd_2</span><span class="sxs-lookup"><span data-stu-id="573a0-149">FrontEnd_2</span></span> |<span data-ttu-id="573a0-150">3391</span><span class="sxs-lookup"><span data-stu-id="573a0-150">3391</span></span> |
| <span data-ttu-id="573a0-151">FrontEnd_3</span><span class="sxs-lookup"><span data-stu-id="573a0-151">FrontEnd_3</span></span> |<span data-ttu-id="573a0-152">3392</span><span class="sxs-lookup"><span data-stu-id="573a0-152">3392</span></span> |
| <span data-ttu-id="573a0-153">FrontEnd_4</span><span class="sxs-lookup"><span data-stu-id="573a0-153">FrontEnd_4</span></span> |<span data-ttu-id="573a0-154">3393</span><span class="sxs-lookup"><span data-stu-id="573a0-154">3393</span></span> |
| <span data-ttu-id="573a0-155">FrontEnd_5</span><span class="sxs-lookup"><span data-stu-id="573a0-155">FrontEnd_5</span></span> |<span data-ttu-id="573a0-156">3394</span><span class="sxs-lookup"><span data-stu-id="573a0-156">3394</span></span> |

### <a name="step-3-remote-connect-to-the-specific-vm-scale-set-instance"></a><span data-ttu-id="573a0-157">3. lépés: Távoli csatlakozás a megadott Virtuálisgép-méretezési csoportban példányhoz</span><span class="sxs-lookup"><span data-stu-id="573a0-157">Step 3: Remote connect to the specific VM Scale Set instance</span></span>
<span data-ttu-id="573a0-158">Az alábbi képernyőfelvételen a távoli asztali kapcsolat a FrontEnd_1 való kapcsolódáshoz használni:</span><span class="sxs-lookup"><span data-stu-id="573a0-158">In the screenshot below I use Remote Desktop Connection to connect to the FrontEnd_1:</span></span>

![RDP][RDP]

## <a name="how-to-change-the-rdp-port-range-values"></a><span data-ttu-id="573a0-160">Az RDP-port tartományértékeknek módosítása</span><span class="sxs-lookup"><span data-stu-id="573a0-160">How to change the RDP port range values</span></span>
### <a name="before-cluster-deployment"></a><span data-ttu-id="573a0-161">Fürttelepítés előtt</span><span class="sxs-lookup"><span data-stu-id="573a0-161">Before cluster deployment</span></span>
<span data-ttu-id="573a0-162">Állítja be a fürt Resource Manager-sablonnal, megadhatja a tartomány a **inboundNatPools**.</span><span class="sxs-lookup"><span data-stu-id="573a0-162">When you are setting up the cluster using an Resource Manager template, you can specify the range in the **inboundNatPools**.</span></span>

<span data-ttu-id="573a0-163">Keresse fel az erőforrás-definíció **Microsoft.Network/loadBalancers**.</span><span class="sxs-lookup"><span data-stu-id="573a0-163">Go to the resource definition for **Microsoft.Network/loadBalancers**.</span></span> <span data-ttu-id="573a0-164">Amely alatt a leírása található **inboundNatPools**.</span><span class="sxs-lookup"><span data-stu-id="573a0-164">Under that you find the description for **inboundNatPools**.</span></span>  <span data-ttu-id="573a0-165">Cserélje le a *frontendportrangestart értékénél* és *frontendportrangeend értékének* értékeket.</span><span class="sxs-lookup"><span data-stu-id="573a0-165">Replace the *frontendPortRangeStart* and *frontendPortRangeEnd* values.</span></span>

![InboundNatPools][InboundNatPools]

### <a name="after-cluster-deployment"></a><span data-ttu-id="573a0-167">Fürt telepítése után</span><span class="sxs-lookup"><span data-stu-id="573a0-167">After cluster deployment</span></span>
<span data-ttu-id="573a0-168">Ez egy kicsit bonyolultabb, és azt eredményezheti, a virtuális gépek újbóli felhasználását.</span><span class="sxs-lookup"><span data-stu-id="573a0-168">This is a bit more involved and may result in the VMs getting recycled.</span></span> <span data-ttu-id="573a0-169">Most fog Azure PowerShell használatával új értékeinek beállításához.</span><span class="sxs-lookup"><span data-stu-id="573a0-169">You will now have to set new values using Azure PowerShell.</span></span> <span data-ttu-id="573a0-170">Győződjön meg arról, hogy az Azure PowerShell 1.0-s vagy újabb verziója telepítve van-e a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="573a0-170">Make sure that Azure PowerShell 1.0 or later is installed on your machine.</span></span> <span data-ttu-id="573a0-171">Ha nem ezt megelőzően, I erősen javasolt lépéseket a leírt [hogyan Azure PowerShell telepítése és konfigurálása.](/powershell/azure/overview)</span><span class="sxs-lookup"><span data-stu-id="573a0-171">If you have not done this before, I strongly suggest that you follow the steps outlined in [How to install and configure Azure PowerShell.](/powershell/azure/overview)</span></span>

<span data-ttu-id="573a0-172">Jelentkezzen be az Azure-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="573a0-172">Sign in to your Azure account.</span></span> <span data-ttu-id="573a0-173">Ha a PowerShell-parancs valamilyen okból nem sikerül, ellenőrizze az Azure PowerShell megfelelően telepítve van-e.</span><span class="sxs-lookup"><span data-stu-id="573a0-173">If this PowerShell command fails for some reason, you should check whether you have Azure PowerShell installed correctly.</span></span>

```
Login-AzureRmAccount
```

<span data-ttu-id="573a0-174">Futtassa a következő kapcsolatban a terheléselosztó és a leírást a értéke látható **inboundNatPools**:</span><span class="sxs-lookup"><span data-stu-id="573a0-174">Run the following to get details on your load balancer and you see the values for the description for **inboundNatPools**:</span></span>

```
Get-AzureRmResource -ResourceGroupName <RGname> -ResourceType Microsoft.Network/loadBalancers -ResourceName <load balancer name>
```

<span data-ttu-id="573a0-175">Most már készen *frontendportrangeend értékének* és *frontendportrangestart értékénél* az értékeket.</span><span class="sxs-lookup"><span data-stu-id="573a0-175">Now set *frontendPortRangeEnd* and *frontendPortRangeStart* to the values you want.</span></span>

```
$PropertiesObject = @{
    #Property = value;
}
Set-AzureRmResource -PropertyObject $PropertiesObject -ResourceGroupName <RG name> -ResourceType Microsoft.Network/loadBalancers -ResourceName <load Balancer name> -ApiVersion <use the API version that get returned> -Force
```


## <a name="next-steps"></a><span data-ttu-id="573a0-176">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="573a0-176">Next steps</span></span>
* [<span data-ttu-id="573a0-177">A "Üzembe helyezés bárhol" szolgáltatás és az Azure által kezelt fürtökkel összehasonlítása áttekintése</span><span class="sxs-lookup"><span data-stu-id="573a0-177">Overview of the "Deploy anywhere" feature and a comparison with Azure-managed clusters</span></span>](service-fabric-deploy-anywhere.md)
* [<span data-ttu-id="573a0-178">Fürtbiztonság</span><span class="sxs-lookup"><span data-stu-id="573a0-178">Cluster security</span></span>](service-fabric-cluster-security.md)
* [<span data-ttu-id="573a0-179">Service Fabric SDK és az első lépések</span><span class="sxs-lookup"><span data-stu-id="573a0-179"> Service Fabric SDK and getting started</span></span>](service-fabric-get-started.md)

<!--Image references-->
[NodeTypes]: ./media/service-fabric-cluster-nodetypes/NodeTypes.png
[Resources]: ./media/service-fabric-cluster-nodetypes/Resources.png
[InboundNatPools]: ./media/service-fabric-cluster-nodetypes/InboundNatPools.png
[LBBlade]: ./media/service-fabric-cluster-nodetypes/LBBlade.png
[NATRules]: ./media/service-fabric-cluster-nodetypes/NATRules.png
[RDP]: ./media/service-fabric-cluster-nodetypes/RDP.png
