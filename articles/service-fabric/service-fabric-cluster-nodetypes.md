---
title: "Háló aaaService csomóponttípusok és a Virtuálisgép-méretezési készlet |} Microsoft Docs"
description: "Ismerteti, milyen kapcsolatban áll a Service Fabric csomóponttípusok tooVM méretezési csoportok és hogyan kapcsolódnak az tooremote a tooa Virtuálisgép-méretezési csoportban példányt vagy egy fürt csomópontja."
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
ms.openlocfilehash: 830ea2816f5864de146a77483c85de26f91c2425
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="hello-relationship-between-service-fabric-node-types-and-virtual-machine-scale-sets"></a><span data-ttu-id="a6a06-103">a Service Fabric csomóponttípusok és a virtuálisgép-méretezési csoportok hello kapcsolata</span><span class="sxs-lookup"><span data-stu-id="a6a06-103">hello relationship between Service Fabric node types and Virtual Machine Scale Sets</span></span>
<span data-ttu-id="a6a06-104">Virtuálisgép-méretezési csoportok olyan Azure számítási erőforrás toodeploy használja, és a virtuális gépek készletként gyűjteményeinek kezelését.</span><span class="sxs-lookup"><span data-stu-id="a6a06-104">Virtual Machine Scale Sets are an Azure Compute resource you can use toodeploy and manage a collection of virtual machines as a set.</span></span> <span data-ttu-id="a6a06-105">Minden csomópont-típus, a Service Fabric-fürt definiált egy külön Virtuálisgép-méretezési csoportban lett beállítva.</span><span class="sxs-lookup"><span data-stu-id="a6a06-105">Every node type that is defined in a Service Fabric cluster is set up as a separate VM Scale Set.</span></span> <span data-ttu-id="a6a06-106">Az egyes csomóponttípusok akár majd is méretezhető vagy rendelkezik egymástól függetlenül, a portok megnyitása más-más részhalmazához le, és különböző teljesítmény-mérőszámait lehet.</span><span class="sxs-lookup"><span data-stu-id="a6a06-106">Each node type can then be scaled up or down independently, have different sets of ports open, and can have different capacity metrics.</span></span>

<span data-ttu-id="a6a06-107">képernyőfelvétel a következő hello mutatja egy fürt, amely két csomópont tartozik: előtér- és háttérszolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="a6a06-107">hello following screen shot shows a cluster that has two node types: FrontEnd and BackEnd.</span></span>  <span data-ttu-id="a6a06-108">Minden csomópont az öt csomóponttal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="a6a06-108">Each node type has five nodes each.</span></span>

![Képernyőfelvétel egy fürt, amely két csomópont tartozik][NodeTypes]

## <a name="mapping-vm-scale-set-instances-toonodes"></a><span data-ttu-id="a6a06-110">Virtuálisgép-méretezési csoportban példányok toonodes leképezése</span><span class="sxs-lookup"><span data-stu-id="a6a06-110">Mapping VM Scale Set instances toonodes</span></span>
<span data-ttu-id="a6a06-111">Fent látható, hello Virtuálisgép-méretezési csoportban példányok példányt 0-tól kezdődnek, majd következik be.</span><span class="sxs-lookup"><span data-stu-id="a6a06-111">As you can see above, hello VM Scale Set instances start from instance 0 and then goes up.</span></span> <span data-ttu-id="a6a06-112">hello számozására hello neveket is megjelenik.</span><span class="sxs-lookup"><span data-stu-id="a6a06-112">hello numbering is reflected in hello names.</span></span> <span data-ttu-id="a6a06-113">Például BackEnd_0 háttér Virtuálisgép-méretezési csoportban hello 0-példány.</span><span class="sxs-lookup"><span data-stu-id="a6a06-113">For example, BackEnd_0 is instance 0 of hello BackEnd VM Scale Set.</span></span> <span data-ttu-id="a6a06-114">Öt példánya, BackEnd_0, BackEnd_1, BackEnd_2, BackEnd_3 és BackEnd_4 az adott Virtuálisgép-méretezési csoportban van.</span><span class="sxs-lookup"><span data-stu-id="a6a06-114">This particular VM Scale Set has five instances, named BackEnd_0, BackEnd_1, BackEnd_2, BackEnd_3 and BackEnd_4.</span></span>

<span data-ttu-id="a6a06-115">Ha Ön növelheti a Virtuálisgép-méretezési csoportban egy új példány jön létre.</span><span class="sxs-lookup"><span data-stu-id="a6a06-115">When you scale up a VM Scale Set a new instance is created.</span></span> <span data-ttu-id="a6a06-116">hello új Virtuálisgép-méretezési csoportban példánynév hello Virtuálisgép-méretezési csoportban neve + hello következő példányszámának általában legyen.</span><span class="sxs-lookup"><span data-stu-id="a6a06-116">hello new VM Scale Set instance name will typically be hello VM Scale Set name + hello next instance number.</span></span> <span data-ttu-id="a6a06-117">A jelen példában BackEnd_5.</span><span class="sxs-lookup"><span data-stu-id="a6a06-117">In our example, it is BackEnd_5.</span></span>

## <a name="mapping-vm-scale-set-load-balancers-tooeach-node-typevm-scale-set"></a><span data-ttu-id="a6a06-118">Méretezési betöltése hozzárendelése a virtuális gép egy terheléselosztó tooeach csomópont típus/VM méretezési</span><span class="sxs-lookup"><span data-stu-id="a6a06-118">Mapping VM scale set load balancers tooeach node type/VM Scale Set</span></span>
<span data-ttu-id="a6a06-119">Ha telepítette a fürt hello portálról, vagy hello minta Resource Manager-sablon, amely azt megadva, majd amikor egy erőforráscsoportba tartozó összes erőforrás listáját használja majd látni fogja az egyes Virtuálisgép-méretezési csoportban vagy csomópont hello terheléselosztók.</span><span class="sxs-lookup"><span data-stu-id="a6a06-119">If you have deployed your cluster from hello portal or have used hello sample Resource Manager template that we provided, then when you get a list of all resources under a Resource Group then you will see hello load balancers for each VM Scale Set or node type.</span></span>

<span data-ttu-id="a6a06-120">hello neve volna hasonlót: **LB -&lt;NodeType neve&gt;**.</span><span class="sxs-lookup"><span data-stu-id="a6a06-120">hello name would something like: **LB-&lt;NodeType name&gt;**.</span></span> <span data-ttu-id="a6a06-121">Például LB-sfcluster4doc-0, a képernyőfelvételen látható módon:</span><span class="sxs-lookup"><span data-stu-id="a6a06-121">For example, LB-sfcluster4doc-0, as shown in this screenshot:</span></span>

![Erőforrások][Resources]

## <a name="remote-connect-tooa-vm-scale-set-instance-or-a-cluster-node"></a><span data-ttu-id="a6a06-123">Távoli kapcsolódás tooa Virtuálisgép-méretezési csoportban példányt vagy egy fürt csomópontja</span><span class="sxs-lookup"><span data-stu-id="a6a06-123">Remote connect tooa VM Scale Set instance or a cluster node</span></span>
<span data-ttu-id="a6a06-124">Minden csomópont-típus, egy fürt definiált egy külön Virtuálisgép-méretezési csoportban lett beállítva.</span><span class="sxs-lookup"><span data-stu-id="a6a06-124">Every Node type that is defined in a cluster is set up as a separate VM Scale Set.</span></span>  <span data-ttu-id="a6a06-125">Megadhat az, hogy azt jelenti, hogy hello csomóponttípusok akár vagy egymástól függetlenül le, és különböző VM termékváltozatok teszik lehetővé.</span><span class="sxs-lookup"><span data-stu-id="a6a06-125">That means hello node types can be scaled up or down independently and can be made of different VM SKUs.</span></span> <span data-ttu-id="a6a06-126">Egypéldányos virtuális gépeket, eltérően hello Virtuálisgép-méretezési csoport példányai nem kérdezhető le egy virtuális IP-címet saját.</span><span class="sxs-lookup"><span data-stu-id="a6a06-126">Unlike single instance VMs, hello VM Scale Set instances do not get a virtual IP address of their own.</span></span> <span data-ttu-id="a6a06-127">Így jelző bit lehet kihívást, ha a keresett IP cím és port használható tooremote csatlakozás tooa példány.</span><span class="sxs-lookup"><span data-stu-id="a6a06-127">So it can be a bit challenging when you are looking for an IP address and port that you can use tooremote connect tooa specific instance.</span></span>

<span data-ttu-id="a6a06-128">Az alábbiakban hello lépések követésével toodiscover őket.</span><span class="sxs-lookup"><span data-stu-id="a6a06-128">Here are hello steps you can follow toodiscover them.</span></span>

### <a name="step-1-find-out-hello-virtual-ip-address-for-hello-node-type-and-then-inbound-nat-rules-for-rdp"></a><span data-ttu-id="a6a06-129">1. lépés: Hello csomóponttípus hello virtuális IP-címét, majd a bejövő forgalmat kezelő NAT-szabályokat RDP megállapítása</span><span class="sxs-lookup"><span data-stu-id="a6a06-129">Step 1: Find out hello virtual IP address for hello node type and then Inbound NAT rules for RDP</span></span>
<span data-ttu-id="a6a06-130">A sorrend tooget, telepíteni kell, amely tooget hello bejövő forgalmat kezelő NAT szabályok hello erőforrás-definíció részeként meghatározott értékek **Microsoft.Network/loadBalancers**.</span><span class="sxs-lookup"><span data-stu-id="a6a06-130">In order tooget that, you need tooget hello inbound NAT rules values that were defined as a part of hello resource definition for **Microsoft.Network/loadBalancers**.</span></span>

<span data-ttu-id="a6a06-131">Hello portálon lépjen toohello Load balancer panelen, majd **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="a6a06-131">In hello portal, navigate toohello Load balancer blade and then **Settings**.</span></span>

![LBBlade][LBBlade]

<span data-ttu-id="a6a06-133">A **beállítások**, kattintson a **bejövő forgalmat kezelő NAT-szabályok**.</span><span class="sxs-lookup"><span data-stu-id="a6a06-133">In **Settings**, click on **Inbound NAT rules**.</span></span> <span data-ttu-id="a6a06-134">Ez most által biztosított IP-cím és port használható tooremote hello toohello Virtuálisgép-méretezési csoportban elsőként csatlakoztassa.</span><span class="sxs-lookup"><span data-stu-id="a6a06-134">This now gives you hello IP address and port that you can use tooremote connect toohello first VM Scale Set instance.</span></span> <span data-ttu-id="a6a06-135">Hello alábbi képernyőképen látható, hogy a rendszer **104.42.106.156** és **3389-es**</span><span class="sxs-lookup"><span data-stu-id="a6a06-135">In hello screenshot below, it is **104.42.106.156** and **3389**</span></span>

![NATRules][NATRules]

### <a name="step-2-find-out-hello-port-that-you-can-use-tooremote-connect-toohello-specific-vm-scale-set-instancenode"></a><span data-ttu-id="a6a06-137">2. lépés: Hello port megtudhatja, hogy használhatja-e tooremote csatlakozás toohello adott Virtuálisgép-méretezési csoportban példány/csomópont</span><span class="sxs-lookup"><span data-stu-id="a6a06-137">Step 2: Find out hello port that you can use tooremote connect toohello specific VM Scale Set instance/node</span></span>
<span data-ttu-id="a6a06-138">A jelen dokumentum korábbi I arról volt szó, hogyan hello Virtuálisgép-méretezési csoportban példányok leképezése toohello csomópontok.</span><span class="sxs-lookup"><span data-stu-id="a6a06-138">Earlier in this document, I talked about how hello VM Scale Set instances map toohello nodes.</span></span> <span data-ttu-id="a6a06-139">Adott toofigure hello pontos port kimenő használjuk.</span><span class="sxs-lookup"><span data-stu-id="a6a06-139">We will use that toofigure out hello exact port.</span></span>

<span data-ttu-id="a6a06-140">hello portok hello Virtuálisgép-méretezési csoportban példány növekvő sorrendben foglal le.</span><span class="sxs-lookup"><span data-stu-id="a6a06-140">hello ports are allocated in ascending order of hello VM Scale Set instance.</span></span> <span data-ttu-id="a6a06-141">ezért a hello előtér csomóponttípus példában hello hello öt példányok mindegyikének portjait hello következő.</span><span class="sxs-lookup"><span data-stu-id="a6a06-141">so in my example for hello FrontEnd node type, hello ports for each of hello five instances are hello following.</span></span> <span data-ttu-id="a6a06-142">Ön most kell toodo hello a Virtuálisgép-méretezési csoportban példány azonosak maradjanak.</span><span class="sxs-lookup"><span data-stu-id="a6a06-142">you now need toodo hello same mapping for your VM Scale Set instance.</span></span>

| <span data-ttu-id="a6a06-143">**Virtuálisgép-méretezési készlet példány**</span><span class="sxs-lookup"><span data-stu-id="a6a06-143">**VM Scale Set Instance**</span></span> | <span data-ttu-id="a6a06-144">**Port**</span><span class="sxs-lookup"><span data-stu-id="a6a06-144">**Port**</span></span> |
| --- | --- |
| <span data-ttu-id="a6a06-145">FrontEnd_0</span><span class="sxs-lookup"><span data-stu-id="a6a06-145">FrontEnd_0</span></span> |<span data-ttu-id="a6a06-146">3389</span><span class="sxs-lookup"><span data-stu-id="a6a06-146">3389</span></span> |
| <span data-ttu-id="a6a06-147">FrontEnd_1</span><span class="sxs-lookup"><span data-stu-id="a6a06-147">FrontEnd_1</span></span> |<span data-ttu-id="a6a06-148">3390</span><span class="sxs-lookup"><span data-stu-id="a6a06-148">3390</span></span> |
| <span data-ttu-id="a6a06-149">FrontEnd_2</span><span class="sxs-lookup"><span data-stu-id="a6a06-149">FrontEnd_2</span></span> |<span data-ttu-id="a6a06-150">3391</span><span class="sxs-lookup"><span data-stu-id="a6a06-150">3391</span></span> |
| <span data-ttu-id="a6a06-151">FrontEnd_3</span><span class="sxs-lookup"><span data-stu-id="a6a06-151">FrontEnd_3</span></span> |<span data-ttu-id="a6a06-152">3392</span><span class="sxs-lookup"><span data-stu-id="a6a06-152">3392</span></span> |
| <span data-ttu-id="a6a06-153">FrontEnd_4</span><span class="sxs-lookup"><span data-stu-id="a6a06-153">FrontEnd_4</span></span> |<span data-ttu-id="a6a06-154">3393</span><span class="sxs-lookup"><span data-stu-id="a6a06-154">3393</span></span> |
| <span data-ttu-id="a6a06-155">FrontEnd_5</span><span class="sxs-lookup"><span data-stu-id="a6a06-155">FrontEnd_5</span></span> |<span data-ttu-id="a6a06-156">3394</span><span class="sxs-lookup"><span data-stu-id="a6a06-156">3394</span></span> |

### <a name="step-3-remote-connect-toohello-specific-vm-scale-set-instance"></a><span data-ttu-id="a6a06-157">3. lépés: Távoli csatlakozás toohello Virtuálisgép-méretezési csoportban példány</span><span class="sxs-lookup"><span data-stu-id="a6a06-157">Step 3: Remote connect toohello specific VM Scale Set instance</span></span>
<span data-ttu-id="a6a06-158">Az alábbi képernyőképen hello használata a távoli asztali kapcsolat tooconnect toohello FrontEnd_1:</span><span class="sxs-lookup"><span data-stu-id="a6a06-158">In hello screenshot below I use Remote Desktop Connection tooconnect toohello FrontEnd_1:</span></span>

![RDP][RDP]

## <a name="how-toochange-hello-rdp-port-range-values"></a><span data-ttu-id="a6a06-160">Hogyan toochange hello RDP-portjára tartomány értékek</span><span class="sxs-lookup"><span data-stu-id="a6a06-160">How toochange hello RDP port range values</span></span>
### <a name="before-cluster-deployment"></a><span data-ttu-id="a6a06-161">Fürttelepítés előtt</span><span class="sxs-lookup"><span data-stu-id="a6a06-161">Before cluster deployment</span></span>
<span data-ttu-id="a6a06-162">Hello fürt Resource Manager-sablonnal állít be, amikor megadhat hello tartomány hello **inboundNatPools**.</span><span class="sxs-lookup"><span data-stu-id="a6a06-162">When you are setting up hello cluster using an Resource Manager template, you can specify hello range in hello **inboundNatPools**.</span></span>

<span data-ttu-id="a6a06-163">Nyissa meg az erőforrás-definíció toohello **Microsoft.Network/loadBalancers**.</span><span class="sxs-lookup"><span data-stu-id="a6a06-163">Go toohello resource definition for **Microsoft.Network/loadBalancers**.</span></span> <span data-ttu-id="a6a06-164">Amely alatt található hello leírását **inboundNatPools**.</span><span class="sxs-lookup"><span data-stu-id="a6a06-164">Under that you find hello description for **inboundNatPools**.</span></span>  <span data-ttu-id="a6a06-165">Cserélje le a hello *frontendportrangestart értékénél* és *frontendportrangeend értékének* értékeket.</span><span class="sxs-lookup"><span data-stu-id="a6a06-165">Replace hello *frontendPortRangeStart* and *frontendPortRangeEnd* values.</span></span>

![InboundNatPools][InboundNatPools]

### <a name="after-cluster-deployment"></a><span data-ttu-id="a6a06-167">Fürt telepítése után</span><span class="sxs-lookup"><span data-stu-id="a6a06-167">After cluster deployment</span></span>
<span data-ttu-id="a6a06-168">Ez egy kicsit bonyolultabb, és azt eredményezheti, hello virtuális gépek újbóli felhasználását.</span><span class="sxs-lookup"><span data-stu-id="a6a06-168">This is a bit more involved and may result in hello VMs getting recycled.</span></span> <span data-ttu-id="a6a06-169">Most kell tooset Azure PowerShell használatával új értékeket.</span><span class="sxs-lookup"><span data-stu-id="a6a06-169">You will now have tooset new values using Azure PowerShell.</span></span> <span data-ttu-id="a6a06-170">Győződjön meg arról, hogy az Azure PowerShell 1.0-s vagy újabb verziója telepítve van-e a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="a6a06-170">Make sure that Azure PowerShell 1.0 or later is installed on your machine.</span></span> <span data-ttu-id="a6a06-171">Ha nem ezt megelőzően, I erősen javasolt lépéseket hello leírt [hogyan tooinstall Azure PowerShell és konfigurálása.](/powershell/azure/overview)</span><span class="sxs-lookup"><span data-stu-id="a6a06-171">If you have not done this before, I strongly suggest that you follow hello steps outlined in [How tooinstall and configure Azure PowerShell.](/powershell/azure/overview)</span></span>

<span data-ttu-id="a6a06-172">Bejelentkezés tooyour Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="a6a06-172">Sign in tooyour Azure account.</span></span> <span data-ttu-id="a6a06-173">Ha a PowerShell-parancs valamilyen okból nem sikerül, ellenőrizze az Azure PowerShell megfelelően telepítve van-e.</span><span class="sxs-lookup"><span data-stu-id="a6a06-173">If this PowerShell command fails for some reason, you should check whether you have Azure PowerShell installed correctly.</span></span>

```
Login-AzureRmAccount
```

<span data-ttu-id="a6a06-174">Futtassa a következő tooget adatok a terheléselosztón hello és hello leírását hello értéke látható **inboundNatPools**:</span><span class="sxs-lookup"><span data-stu-id="a6a06-174">Run hello following tooget details on your load balancer and you see hello values for hello description for **inboundNatPools**:</span></span>

```
Get-AzureRmResource -ResourceGroupName <RGname> -ResourceType Microsoft.Network/loadBalancers -ResourceName <load balancer name>
```

<span data-ttu-id="a6a06-175">Most már készen *frontendportrangeend értékének* és *frontendportrangestart értékénél* toohello kívánt értékeket.</span><span class="sxs-lookup"><span data-stu-id="a6a06-175">Now set *frontendPortRangeEnd* and *frontendPortRangeStart* toohello values you want.</span></span>

```
$PropertiesObject = @{
    #Property = value;
}
Set-AzureRmResource -PropertyObject $PropertiesObject -ResourceGroupName <RG name> -ResourceType Microsoft.Network/loadBalancers -ResourceName <load Balancer name> -ApiVersion <use hello API version that get returned> -Force
```


## <a name="next-steps"></a><span data-ttu-id="a6a06-176">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a6a06-176">Next steps</span></span>
* [<span data-ttu-id="a6a06-177">Hello "Bárhol rendszerbe állítás" szolgáltatás és az Azure által kezelt fürtökkel összehasonlítása áttekintése</span><span class="sxs-lookup"><span data-stu-id="a6a06-177">Overview of hello "Deploy anywhere" feature and a comparison with Azure-managed clusters</span></span>](service-fabric-deploy-anywhere.md)
* [<span data-ttu-id="a6a06-178">Fürtbiztonság</span><span class="sxs-lookup"><span data-stu-id="a6a06-178">Cluster security</span></span>](service-fabric-cluster-security.md)
* [<span data-ttu-id="a6a06-179">Service Fabric SDK és az első lépések</span><span class="sxs-lookup"><span data-stu-id="a6a06-179"> Service Fabric SDK and getting started</span></span>](service-fabric-get-started.md)

<!--Image references-->
[NodeTypes]: ./media/service-fabric-cluster-nodetypes/NodeTypes.png
[Resources]: ./media/service-fabric-cluster-nodetypes/Resources.png
[InboundNatPools]: ./media/service-fabric-cluster-nodetypes/InboundNatPools.png
[LBBlade]: ./media/service-fabric-cluster-nodetypes/LBBlade.png
[NATRules]: ./media/service-fabric-cluster-nodetypes/NATRules.png
[RDP]: ./media/service-fabric-cluster-nodetypes/RDP.png
