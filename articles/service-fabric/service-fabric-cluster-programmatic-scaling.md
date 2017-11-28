---
title: "Service Fabric programozott skálázás aaaAzure |} Microsoft Docs"
description: "Egy bejövő vagy kimenő Azure Service Fabric-fürt méretezése programozott módon, a toocustom eseményindítók szerint"
services: service-fabric
documentationcenter: .net
author: mjrousos
manager: jonjung
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/29/2017
ms.author: mikerou
ms.openlocfilehash: a0c6499b1a143a173006248cf8a15380632637e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="scale-a-service-fabric-cluster-programmatically"></a><span data-ttu-id="611cb-103">A Service Fabric-fürt méretezése programozott módon</span><span class="sxs-lookup"><span data-stu-id="611cb-103">Scale a Service Fabric cluster programmatically</span></span> 

<span data-ttu-id="611cb-104">Az Azure Service Fabric-fürt méretezése alapjai dokumentációjában ismertetett a [fürtméretezés](./service-fabric-cluster-scale-up-down.md).</span><span class="sxs-lookup"><span data-stu-id="611cb-104">Fundamentals of scaling a Service Fabric cluster in Azure are covered in documentation on [cluster scaling](./service-fabric-cluster-scale-up-down.md).</span></span> <span data-ttu-id="611cb-105">Hogy a cikk ismerteti, hogyan Service Fabric-fürtök épülő virtuálisgép-méretezési csoportok és a manuális vagy automatikus skálázása szabályokkal is méretezhető.</span><span class="sxs-lookup"><span data-stu-id="611cb-105">That article covers how Service Fabric clusters are built on top of virtual machine scale sets and can be scaled either manually or with auto-scale rules.</span></span> <span data-ttu-id="611cb-106">Ez a dokumentum speciális forgatókönyvekhez koordináló Azure skálázási műveletek programozási módszerek megvizsgálja.</span><span class="sxs-lookup"><span data-stu-id="611cb-106">This document looks at programmatic methods of coordinating Azure scaling operations for more advanced scenarios.</span></span> 

## <a name="reasons-for-programmatic-scaling"></a><span data-ttu-id="611cb-107">Programozott méretezéshez okok</span><span class="sxs-lookup"><span data-stu-id="611cb-107">Reasons for programmatic scaling</span></span>
<span data-ttu-id="611cb-108">A sok esetben skálázás manuális vagy automatikus skálázása szabályok segítségével található jó megoldás.</span><span class="sxs-lookup"><span data-stu-id="611cb-108">In many scenarios, scaling manually or via auto-scale rules are good solutions.</span></span> <span data-ttu-id="611cb-109">Más esetekben azonban nem feltétlenül hello jobbra igazítása.</span><span class="sxs-lookup"><span data-stu-id="611cb-109">In other scenarios, though, they may not be hello right fit.</span></span> <span data-ttu-id="611cb-110">Lehetséges hátrányai toothese módszerek a következők:</span><span class="sxs-lookup"><span data-stu-id="611cb-110">Potential drawbacks toothese approaches include:</span></span>

- <span data-ttu-id="611cb-111">Manuális skálázás megköveteli a toolog és explicit módon műveletek skálázás kérelem.</span><span class="sxs-lookup"><span data-stu-id="611cb-111">Manually scaling requires you toolog in and explicitly request scaling operations.</span></span> <span data-ttu-id="611cb-112">Ha a skálázási műveletek szükségesek gyakran vagy időpontban előre nem látható, ez a megközelítés nem lehet jó megoldás.</span><span class="sxs-lookup"><span data-stu-id="611cb-112">If scaling operations are required frequently or at unpredictable times, this approach may not be a good solution.</span></span>
- <span data-ttu-id="611cb-113">Automatikus méretezése szabályok eltávolítása egy virtuálisgép-méretezési csoport egy példányát, ha azok nem automatikusan eltávolítása Tudásbázis az adott csomópont tartozó hello Service Fabric-fürt kivéve hello csomóponttípus ezüst vagy arany a tartóssági szint.</span><span class="sxs-lookup"><span data-stu-id="611cb-113">When auto-scale rules remove an instance from a virtual machine scale set, they do not automatically remove knowledge of that node from hello associated Service Fabric cluster unless hello node type has a durability level of Silver or Gold.</span></span> <span data-ttu-id="611cb-114">Automatikus méretezése szabályok hello léptékű szint beállítása (és nem a Service Fabric szint hello) működik, mert automatikus méretezése szabályok is távolítsa el a Service Fabric-csomópont őket szabályosan leállítása nélkül.</span><span class="sxs-lookup"><span data-stu-id="611cb-114">Because auto-scale rules work at hello scale set level (rather than at hello Service Fabric level), auto-scale rules can remove Service Fabric nodes without shutting them down gracefully.</span></span> <span data-ttu-id="611cb-115">Goromba csomópont eltávolítása fog hagy "szellemrekord" a Service Fabric-csomópont állapota méretezési a műveletek után.</span><span class="sxs-lookup"><span data-stu-id="611cb-115">This rude node removal will leave 'ghost' Service Fabric node state behind after scale-in operations.</span></span> <span data-ttu-id="611cb-116">Egy adott (vagy egy szolgáltatás) kell tooperiodically karbantartása hello Service Fabric-fürtöt az eltávolított csomópont állapota.</span><span class="sxs-lookup"><span data-stu-id="611cb-116">An individual (or a service) would need tooperiodically clean up removed node state in hello Service Fabric cluster.</span></span>
  - <span data-ttu-id="611cb-117">Vegye figyelembe, hogy az arany vagy ezüst tartóssági szint csomóponttípus automatikusan be lesz tisztítása csomópontokat eltávolítani.</span><span class="sxs-lookup"><span data-stu-id="611cb-117">Note that a node type with a durability level of Gold or Silver will automatically clean up removed nodes.</span></span>  
- <span data-ttu-id="611cb-118">Bár vannak [sok metrikák](../monitoring-and-diagnostics/insights-autoscale-common-metrics.md) automatikus méretezése szabályok által támogatott, akkor még mindig korlátozott.</span><span class="sxs-lookup"><span data-stu-id="611cb-118">Although there are [many metrics](../monitoring-and-diagnostics/insights-autoscale-common-metrics.md) supported by auto-scale rules, it is still a limited set.</span></span> <span data-ttu-id="611cb-119">Ha a forgatókönyv nem vonatkozik a készletben lévő egyes metrika alapján méretezéshez, majd automatikus méretezése szabályok nem lehet jó választás.</span><span class="sxs-lookup"><span data-stu-id="611cb-119">If your scenario calls for scaling based on some metric not covered in that set, then auto-scale rules may not be a good option.</span></span>

<span data-ttu-id="611cb-120">E tényezők alapján, érdemes tooimplement több testreszabott automatikus méretezési modellek.</span><span class="sxs-lookup"><span data-stu-id="611cb-120">Based on these limitations, you may wish tooimplement more customized automatic scaling models.</span></span> 

## <a name="scaling-apis"></a><span data-ttu-id="611cb-121">Méretezési API-k</span><span class="sxs-lookup"><span data-stu-id="611cb-121">Scaling APIs</span></span>
<span data-ttu-id="611cb-122">Az Azure API-k léteznek, amelyek lehetővé teszik az alkalmazások tooprogrammatically együttműködnek a virtuálisgép-méretezési csoportok és a Service Fabric-fürtök.</span><span class="sxs-lookup"><span data-stu-id="611cb-122">Azure APIs exist which allow applications tooprogrammatically work with virtual machine scale sets and Service Fabric clusters.</span></span> <span data-ttu-id="611cb-123">Meglévő automatikus méretezése lehetőségek nem működnek az adott esetben, ha ezen API-k legyen lehetséges tooimplement egyéni méretezési logika.</span><span class="sxs-lookup"><span data-stu-id="611cb-123">If existing auto-scale options don't work for your scenario, these APIs make it possible tooimplement custom scaling logic.</span></span> 

<span data-ttu-id="611cb-124">A "Kezdőlap-végrehajtott" automatikus skálázás egyik módszer tooimplementing funkció tooadd van egy új állapotmentes szolgáltatások toohello Service Fabric application toomanage skálázás műveletek.</span><span class="sxs-lookup"><span data-stu-id="611cb-124">One approach tooimplementing this 'home-made' auto-scaling functionality is tooadd a new stateless service toohello Service Fabric application toomanage scaling operations.</span></span> <span data-ttu-id="611cb-125">Hello szolgáltatáson belül `RunAsync` módszer, eseményindítók készlete is megállapítja, hogy skálázás szükség (többek között a paraméterek maximális fürt például ellenőrzése és a méretezés cooldowns).</span><span class="sxs-lookup"><span data-stu-id="611cb-125">Within hello service's `RunAsync` method, a set of triggers can determine if scaling is required (including checking parameters such as maximum cluster size and scaling cooldowns).</span></span>   

<span data-ttu-id="611cb-126">virtuális gép méretezési készlet kapcsolati használt API hello (mindkét toocheck hello aktuális száma a virtuálisgép-példányok és toomodify azt) hello van [Folyékonyan beszél Azure felügyeleti számítási könyvtár](https://www.nuget.org/packages/Microsoft.Azure.Management.Compute.Fluent/).</span><span class="sxs-lookup"><span data-stu-id="611cb-126">hello API used for virtual machine scale set interactions (both toocheck hello current number of virtual machine instances and toomodify it) is hello [fluent Azure Management Compute library](https://www.nuget.org/packages/Microsoft.Azure.Management.Compute.Fluent/).</span></span> <span data-ttu-id="611cb-127">hello Folyékonyan beszél számítási kódtár biztosít egy könnyen használható API-t használják a virtuálisgép-méretezési készlet.</span><span class="sxs-lookup"><span data-stu-id="611cb-127">hello fluent compute library provides an easy-to-use API for interacting with virtual machine scale sets.</span></span>

<span data-ttu-id="611cb-128">hello Service Fabric-fürt magát, a toointeract használja [System.Fabric.FabricClient](/dotnet/api/system.fabric.fabricclient).</span><span class="sxs-lookup"><span data-stu-id="611cb-128">toointeract with hello Service Fabric cluster itself, use [System.Fabric.FabricClient](/dotnet/api/system.fabric.fabricclient).</span></span>

<span data-ttu-id="611cb-129">Természetesen kód skálázás hello szolgáltatás toorun nem szükséges a hello fürt toobe méretezhető.</span><span class="sxs-lookup"><span data-stu-id="611cb-129">Of course, hello scaling code doesn't need toorun as a service in hello cluster toobe scaled.</span></span> <span data-ttu-id="611cb-130">Mindkét `IAzure` és `FabricClient` kapcsolódhatnak tootheir társított Azure-erőforrások távolról, így hello szolgáltatás skálázás könnyen lehet egy konzolalkalmazás vagy a Windows-szolgáltatás fut a külső hello Service Fabric-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="611cb-130">Both `IAzure` and `FabricClient` can connect tootheir associated Azure resources remotely, so hello scaling service could easily be a console application or Windows service running from outside hello Service Fabric application.</span></span> 

## <a name="credential-management"></a><span data-ttu-id="611cb-131">Hitelesítő adatok kezelése</span><span class="sxs-lookup"><span data-stu-id="611cb-131">Credential management</span></span>
<span data-ttu-id="611cb-132">Egy szolgáltatás toohandle skálázás rendszerekben az egyik kihívás az, hogy kell-e képes tooaccess virtuális gép méretezési készlet erőforrások egy interaktív bejelentkezési azonosító nélküli hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="611cb-132">One challenge of writing a service toohandle scaling is that hello service must be able tooaccess virtual machine scale set resources without an interactive login.</span></span> <span data-ttu-id="611cb-133">A Service Fabric hello elérése fürt esetén könnyen hello szolgáltatás skálázás módosítja a saját Service Fabric-alkalmazás, de a hitelesítő adatok szükséges tooaccess hello méretezési készlet.</span><span class="sxs-lookup"><span data-stu-id="611cb-133">Accessing hello Service Fabric cluster is easy if hello scaling service is modifying its own Service Fabric application, but credentials are needed tooaccess hello scale set.</span></span> <span data-ttu-id="611cb-134">a toolog, használhatja a [egyszerű](https://github.com/Azure/azure-sdk-for-net/blob/Fluent/AUTH.md#creating-a-service-principal-in-azure) hello létre [Azure CLI 2.0](https://github.com/azure/azure-cli).</span><span class="sxs-lookup"><span data-stu-id="611cb-134">toolog in, you can use a [service principal](https://github.com/Azure/azure-sdk-for-net/blob/Fluent/AUTH.md#creating-a-service-principal-in-azure) created with hello [Azure CLI 2.0](https://github.com/azure/azure-cli).</span></span>

<span data-ttu-id="611cb-135">Egy egyszerű hello lépésekkel hozhatja létre:</span><span class="sxs-lookup"><span data-stu-id="611cb-135">A service principal can be created with hello following steps:</span></span>

1. <span data-ttu-id="611cb-136">Jelentkezzen be az Azure parancssori felület toohello (`az login`) hozzáférési toohello virtuálisgép-méretezési rendelkező felhasználóként beállítása</span><span class="sxs-lookup"><span data-stu-id="611cb-136">Log in toohello Azure CLI (`az login`) as a user with access toohello virtual machine scale set</span></span>
2. <span data-ttu-id="611cb-137">Az egyszerű hello szolgáltatás létrehozása`az ad sp create-for-rbac`</span><span class="sxs-lookup"><span data-stu-id="611cb-137">Create hello service principal with `az ad sp create-for-rbac`</span></span>
    1. <span data-ttu-id="611cb-138">Jegyezze fel a hello appId (más néven "ügyfél-azonosító" máshol), a neve, a jelszót és a bérlői későbbi használatra.</span><span class="sxs-lookup"><span data-stu-id="611cb-138">Make note of hello appId (called 'client ID' elsewhere), name, password, and tenant for later use.</span></span>
    2. <span data-ttu-id="611cb-139">Konfigurálnia kell az előfizetési azonosító, amely tekinthetők meg`az account list`</span><span class="sxs-lookup"><span data-stu-id="611cb-139">You will also need your subscription ID, which can be viewed with `az account list`</span></span>

<span data-ttu-id="611cb-140">hello Folyékonyan beszél számítási könyvtár jelentkezhetnek be ezeket a hitelesítő adatokat az alábbiak szerint (vegye figyelembe, hogy core Folyékonyan beszél Azure típusok hasonlóan `IAzure` a hello [Microsoft.Azure.Management.Fluent](https://www.nuget.org/packages/Microsoft.Azure.Management.Fluent/) csomag):</span><span class="sxs-lookup"><span data-stu-id="611cb-140">hello fluent compute library can log in using these credentials as follows (note that core fluent Azure types like `IAzure` are in hello [Microsoft.Azure.Management.Fluent](https://www.nuget.org/packages/Microsoft.Azure.Management.Fluent/) package):</span></span>

```C#
var credentials = new AzureCredentials(new ServicePrincipalLoginInformation {
                ClientId = AzureClientId,
                ClientSecret = 
                AzureClientKey }, AzureTenantId, AzureEnvironment.AzureGlobalCloud);
IAzure AzureClient = Azure.Authenticate(credentials).WithSubscription(AzureSubscriptionId);

if (AzureClient?.SubscriptionId == AzureSubscriptionId)
{
    ServiceEventSource.Current.ServiceMessage(Context, "Successfully logged into Azure");
}
else
{
    ServiceEventSource.Current.ServiceMessage(Context, "ERROR: Failed toologin tooAzure");
}
```

<span data-ttu-id="611cb-141">Miután bejelentkezett, méretezési készlet példányszáma lekérdezhetők, keresztül `AzureClient.VirtualMachineScaleSets.GetById(ScaleSetId).Capacity`.</span><span class="sxs-lookup"><span data-stu-id="611cb-141">Once logged in, scale set instance count can be queried via `AzureClient.VirtualMachineScaleSets.GetById(ScaleSetId).Capacity`.</span></span>

## <a name="scaling-out"></a><span data-ttu-id="611cb-142">Méretezés</span><span class="sxs-lookup"><span data-stu-id="611cb-142">Scaling out</span></span>
<span data-ttu-id="611cb-143">Folyékonyan beszél hello használata Azure számítási SDK, példányok felveheti toohello virtuálisgép-méretezési pár hívások - beállítása</span><span class="sxs-lookup"><span data-stu-id="611cb-143">Using hello fluent Azure compute SDK, instances can be added toohello virtual machine scale set with just a few calls -</span></span>

```C#
var scaleSet = AzureClient.VirtualMachineScaleSets.GetById(ScaleSetId);
var newCapacity = (int)Math.Min(MaximumNodeCount, scaleSet.Capacity + 1);
scaleSet.Update().WithCapacity(newCapacity).Apply(); 
``` 

<span data-ttu-id="611cb-144">Azt is megteheti virtuális gép méretezési készlet méretét is felügyelhetők a PowerShell-parancsmagokkal.</span><span class="sxs-lookup"><span data-stu-id="611cb-144">Alternatively, virtual machine scale set size can also be managed with PowerShell cmdlets.</span></span> <span data-ttu-id="611cb-145">[`Get-AzureRmVmss`](https://docs.microsoft.com/en-us/powershell/module/azurerm.compute/get-azurermvmss)lekérheti hello virtuálisgép-méretezési készlet objektumot.</span><span class="sxs-lookup"><span data-stu-id="611cb-145">[`Get-AzureRmVmss`](https://docs.microsoft.com/en-us/powershell/module/azurerm.compute/get-azurermvmss) can retrieve hello virtual machine scale set object.</span></span> <span data-ttu-id="611cb-146">hello aktuális kapacitás fogja tárolni hello `.sku.capacity` tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="611cb-146">hello current capacity will be stored in hello `.sku.capacity` property.</span></span> <span data-ttu-id="611cb-147">Miután hello kapacitás toohello módosítása a keresett, állítsa be az Azure-ban hello virtuálisgép-méretezési frissíthető hello [ `Update-AzureRmVmss` ](https://docs.microsoft.com/en-us/powershell/module/azurerm.compute/update-azurermvmss) parancsot.</span><span class="sxs-lookup"><span data-stu-id="611cb-147">After changing hello capacity toohello desired value, hello virtual machine scale set in Azure can be updated with hello [`Update-AzureRmVmss`](https://docs.microsoft.com/en-us/powershell/module/azurerm.compute/update-azurermvmss) command.</span></span>

<span data-ttu-id="611cb-148">Ha a csomópont manuális hozzáadása egy méretezési példány hozzáadása egy új Service Fabric csomópont hello méretezési sablon tartalmazza az összes meg szükséges toostart kell bővítmények tooautomatically csatlakoztassa az új példányok toohello Service Fabric-fürt.</span><span class="sxs-lookup"><span data-stu-id="611cb-148">As when adding a node manually, adding a scale set instance should be all that's needed toostart a new Service Fabric node since hello scale set template includes extensions tooautomatically join new instances toohello Service Fabric cluster.</span></span> 

## <a name="scaling-in"></a><span data-ttu-id="611cb-149">A méretezés</span><span class="sxs-lookup"><span data-stu-id="611cb-149">Scaling in</span></span>

<span data-ttu-id="611cb-150">A méretezés akkor hasonló tooscaling ki. hello tényleges virtuálisgép-méretezési csoport változtatások vannak gyakorlatilag hello azonos.</span><span class="sxs-lookup"><span data-stu-id="611cb-150">Scaling in is similar tooscaling out. hello actual virtual machine scale set changes are practically hello same.</span></span> <span data-ttu-id="611cb-151">De volt korábban bemutatott, a Service Fabric csak automatikusan törli a szükségtelenné az eltávolított csomópontokat a tartós arany vagy ezüst.</span><span class="sxs-lookup"><span data-stu-id="611cb-151">But, as was discussed previously, Service Fabric only automatically cleans up removed nodes with a durability of Gold or Silver.</span></span> <span data-ttu-id="611cb-152">Így az hello bronz-tartóssági méretezési a helyzet, a Service Fabric hello szükséges toointeract is a fürt tooshut hello csomópont toobe eltávolított le, majd a tooremove állapotában.</span><span class="sxs-lookup"><span data-stu-id="611cb-152">So, in hello Bronze-durability scale-in case, it's necessary toointeract with hello Service Fabric cluster tooshut down hello node toobe removed and then tooremove its state.</span></span>

<span data-ttu-id="611cb-153">Hello csomópont Felkészülés leállítási magában foglalja a hello csomópont toobe keresése eltávolítása (hello legutóbb hozzáadott csomópont) és inaktiválása.</span><span class="sxs-lookup"><span data-stu-id="611cb-153">Preparing hello node for shutdown involves finding hello node toobe removed (hello most recently added node) and deactivating it.</span></span> <span data-ttu-id="611cb-154">Nem kezdőérték csomópontok újabb csomópontok található összehasonlításával `NodeInstanceId`.</span><span class="sxs-lookup"><span data-stu-id="611cb-154">For non-seed nodes, newer nodes can be found by comparing `NodeInstanceId`.</span></span> 

```C#
using (var client = new FabricClient())
{
    var mostRecentLiveNode = (await client.QueryManager.GetNodeListAsync())
        .Where(n => n.NodeType.Equals(NodeTypeToScale, StringComparison.OrdinalIgnoreCase))
        .Where(n => n.NodeStatus == System.Fabric.Query.NodeStatus.Up)
        .OrderByDescending(n => n.NodeInstanceId)
        .FirstOrDefault();
```

<span data-ttu-id="611cb-155">Vegye figyelembe, hogy *kezdőérték* csomópontok látszólag nem tooalways hajtsa végre a nagyobb példány azonosítók először eltávolításuk hello egyezmény.</span><span class="sxs-lookup"><span data-stu-id="611cb-155">Be aware that *seed* nodes don't seem tooalways follow hello convention that greater instance IDs are removed first.</span></span>

<span data-ttu-id="611cb-156">Miután hello csomópont toobe eltávolítani megtalálható, kikapcsolható, és eltávolított használatával hello azonos `FabricClient` példány és hello `IAzure` példányának regisztrációját a korábbi.</span><span class="sxs-lookup"><span data-stu-id="611cb-156">Once hello node toobe removed is found, it can be deactivated and removed using hello same `FabricClient` instance and hello `IAzure` instance from earlier.</span></span>

```C#
var scaleSet = AzureClient.VirtualMachineScaleSets.GetById(ScaleSetId);

// Remove hello node from hello Service Fabric cluster
ServiceEventSource.Current.ServiceMessage(Context, $"Disabling node {mostRecentLiveNode.NodeName}");
await client.ClusterManager.DeactivateNodeAsync(mostRecentLiveNode.NodeName, NodeDeactivationIntent.RemoveNode);

// Wait (up tooa timeout) for hello node toogracefully shutdown
var timeout = TimeSpan.FromMinutes(5);
var waitStart = DateTime.Now;
while ((mostRecentLiveNode.NodeStatus == System.Fabric.Query.NodeStatus.Up || mostRecentLiveNode.NodeStatus == System.Fabric.Query.NodeStatus.Disabling) &&
        DateTime.Now - waitStart < timeout)
{
    mostRecentLiveNode = (await client.QueryManager.GetNodeListAsync()).FirstOrDefault(n => n.NodeName == mostRecentLiveNode.NodeName);
    await Task.Delay(10 * 1000);
}

// Decrement VMSS capacity
var newCapacity = (int)Math.Max(MinimumNodeCount, scaleSet.Capacity - 1); // Check min count 

scaleSet.Update().WithCapacity(newCapacity).Apply(); 
```

<span data-ttu-id="611cb-157">Mint meg, a PowerShell-parancsmagok a módosítását a virtuálisgép-méretezési készlet kapacitása is használható itt egy parancsfájl-kezelési megoldás hatékonyabb, ha.</span><span class="sxs-lookup"><span data-stu-id="611cb-157">As with scaling out, PowerShell cmdlets for modifying virtual machine scale set capacity can also be used here if a scripting approach is preferable.</span></span> <span data-ttu-id="611cb-158">Miután hello virtuálisgép-példányt eltávolítják, a Service Fabric-csomópont állapota lehet eltávolítani.</span><span class="sxs-lookup"><span data-stu-id="611cb-158">Once hello virtual machine instance is removed, Service Fabric node state can be removed.</span></span>

```C#
await client.ClusterManager.RemoveNodeStateAsync(mostRecentLiveNode.NodeName);
```

## <a name="potential-drawbacks"></a><span data-ttu-id="611cb-159">Lehetséges hátrányai</span><span class="sxs-lookup"><span data-stu-id="611cb-159">Potential drawbacks</span></span>

<span data-ttu-id="611cb-160">Ahogyan az kódrészleteket megelőző hello, biztosít a saját méretezési szolgáltatás létrehozása a hello legmagasabb szintű vezérlő és az alkalmazás keresztül testreszabhatóság miatt a skálázás viselkedését.</span><span class="sxs-lookup"><span data-stu-id="611cb-160">As demonstrated in hello preceding code snippets, creating your own scaling service provides hello highest degree of control and customizability over your application's scaling behavior.</span></span> <span data-ttu-id="611cb-161">Ez lehet hasznos, ha pontosan meghatározhatja hogyan vagy ha egy alkalmazás méretezi-e a bejövő vagy kimenő igénylő forgatókönyvek. Azonban ez a vezérlő tartalmaz a kompromisszummal jár, kód összetettségét.</span><span class="sxs-lookup"><span data-stu-id="611cb-161">This can be useful for scenarios requiring precise control over when or how an application scales in or out. However, this control comes with a tradeoff of code complexity.</span></span> <span data-ttu-id="611cb-162">Ezzel a megközelítéssel, az azt jelenti, hogy kell-e kód, amely nem triviális skálázás tooown.</span><span class="sxs-lookup"><span data-stu-id="611cb-162">Using this approach means that you need tooown scaling code, which is non-trivial.</span></span>

<span data-ttu-id="611cb-163">Hogyan meg kell megközelíti a Service Fabric skálázás attól függ, hogy a forgatókönyvéhez.</span><span class="sxs-lookup"><span data-stu-id="611cb-163">How you should approach Service Fabric scaling depends on your scenario.</span></span> <span data-ttu-id="611cb-164">Skálázás ritka, hello képességét tooadd, vagy távolítsa el a csomópontok manuálisan akkor elegendő.</span><span class="sxs-lookup"><span data-stu-id="611cb-164">If scaling is uncommon, hello ability tooadd or remove nodes manually is probably sufficient.</span></span> <span data-ttu-id="611cb-165">Az összetettebb forgatókönyveket automatikus méretezése szabályok és SDK-k hello képességét tooscale programozott módon kitettségének kínál az hatékony megoldások.</span><span class="sxs-lookup"><span data-stu-id="611cb-165">For more complex scenarios, auto-scale rules and SDKs exposing hello ability tooscale programmatically offer powerful alternatives.</span></span>

## <a name="next-steps"></a><span data-ttu-id="611cb-166">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="611cb-166">Next steps</span></span>

<span data-ttu-id="611cb-167">megkezdődött a saját automatikus skálázás logikát megvalósító tooget ismerkedésre hello fogalmakat és hasznos API-k a következő:</span><span class="sxs-lookup"><span data-stu-id="611cb-167">tooget started implementing your own auto-scaling logic, familiarize yourself with hello following concepts and useful APIs:</span></span>

- [<span data-ttu-id="611cb-168">Manuális vagy automatikus skálázása szabályokkal skálázás</span><span class="sxs-lookup"><span data-stu-id="611cb-168">Scaling manually or with auto-scale rules</span></span>](./service-fabric-cluster-scale-up-down.md)
- <span data-ttu-id="611cb-169">[A .NET-hez Folyékonyan beszél Azure kezelési kódtárakat](https://github.com/Azure/azure-sdk-for-net/tree/Fluent) (hasznos a Service Fabric-fürt alapul szolgáló virtuálisgép-méretezési csoportok való kommunikáció)</span><span class="sxs-lookup"><span data-stu-id="611cb-169">[Fluent Azure Management Libraries for .NET](https://github.com/Azure/azure-sdk-for-net/tree/Fluent) (useful for interacting with a Service Fabric cluster's underlying virtual machine scale sets)</span></span>
- <span data-ttu-id="611cb-170">[System.Fabric.FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) (hasznos a Service Fabric-fürt és csomópontjainak való kommunikáció)</span><span class="sxs-lookup"><span data-stu-id="611cb-170">[System.Fabric.FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) (useful for interacting with a Service Fabric cluster and its nodes)</span></span>
