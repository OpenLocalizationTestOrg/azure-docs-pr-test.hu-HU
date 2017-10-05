---
title: "Azure Service Fabric programozott skálázás |} Microsoft Docs"
description: "Egy bejövő vagy kimenő Azure Service Fabric-fürt méretezése programozott módon, az egyéni eseményindítók szerint"
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
ms.openlocfilehash: 46b0b62f92abbac57bc27bbcdd5821eafedf5519
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="scale-a-service-fabric-cluster-programmatically"></a><span data-ttu-id="a7bc1-103">A Service Fabric-fürt méretezése programozott módon</span><span class="sxs-lookup"><span data-stu-id="a7bc1-103">Scale a Service Fabric cluster programmatically</span></span> 

<span data-ttu-id="a7bc1-104">Az Azure Service Fabric-fürt méretezése alapjai dokumentációjában ismertetett a [fürtméretezés](./service-fabric-cluster-scale-up-down.md).</span><span class="sxs-lookup"><span data-stu-id="a7bc1-104">Fundamentals of scaling a Service Fabric cluster in Azure are covered in documentation on [cluster scaling](./service-fabric-cluster-scale-up-down.md).</span></span> <span data-ttu-id="a7bc1-105">Hogy a cikk ismerteti, hogyan Service Fabric-fürtök épülő virtuálisgép-méretezési csoportok és a manuális vagy automatikus skálázása szabályokkal is méretezhető.</span><span class="sxs-lookup"><span data-stu-id="a7bc1-105">That article covers how Service Fabric clusters are built on top of virtual machine scale sets and can be scaled either manually or with auto-scale rules.</span></span> <span data-ttu-id="a7bc1-106">Ez a dokumentum speciális forgatókönyvekhez koordináló Azure skálázási műveletek programozási módszerek megvizsgálja.</span><span class="sxs-lookup"><span data-stu-id="a7bc1-106">This document looks at programmatic methods of coordinating Azure scaling operations for more advanced scenarios.</span></span> 

## <a name="reasons-for-programmatic-scaling"></a><span data-ttu-id="a7bc1-107">Programozott méretezéshez okok</span><span class="sxs-lookup"><span data-stu-id="a7bc1-107">Reasons for programmatic scaling</span></span>
<span data-ttu-id="a7bc1-108">A sok esetben skálázás manuális vagy automatikus skálázása szabályok segítségével található jó megoldás.</span><span class="sxs-lookup"><span data-stu-id="a7bc1-108">In many scenarios, scaling manually or via auto-scale rules are good solutions.</span></span> <span data-ttu-id="a7bc1-109">Más esetekben azonban nem feltétlenül a megfelelő méretezése.</span><span class="sxs-lookup"><span data-stu-id="a7bc1-109">In other scenarios, though, they may not be the right fit.</span></span> <span data-ttu-id="a7bc1-110">Ezek a módszerek lehetséges hátrányai a következők:</span><span class="sxs-lookup"><span data-stu-id="a7bc1-110">Potential drawbacks to these approaches include:</span></span>

- <span data-ttu-id="a7bc1-111">Manuális skálázás meg kell-e jelentkezni, és igényelhetnek a skálázási műveletek.</span><span class="sxs-lookup"><span data-stu-id="a7bc1-111">Manually scaling requires you to log in and explicitly request scaling operations.</span></span> <span data-ttu-id="a7bc1-112">Ha a skálázási műveletek szükségesek gyakran vagy időpontban előre nem látható, ez a megközelítés nem lehet jó megoldás.</span><span class="sxs-lookup"><span data-stu-id="a7bc1-112">If scaling operations are required frequently or at unpredictable times, this approach may not be a good solution.</span></span>
- <span data-ttu-id="a7bc1-113">Automatikus méretezése szabályok eltávolítása egy virtuálisgép-méretezési csoport egy példányát, ha azok nem automatikusan eltávolítása az adott csomópont Tudásbázis társított Service Fabric-fürt kivéve, ha a csomópont a tartóssági szint ezüst vagy arany van.</span><span class="sxs-lookup"><span data-stu-id="a7bc1-113">When auto-scale rules remove an instance from a virtual machine scale set, they do not automatically remove knowledge of that node from the associated Service Fabric cluster unless the node type has a durability level of Silver or Gold.</span></span> <span data-ttu-id="a7bc1-114">Automatikus méretezése szabályok működik, a méretezési készletben szint (és nem a Service Fabric szinten), mert automatikus méretezése szabályok is távolítsa el a Service Fabric-csomópont őket szabályosan leállítása nélkül.</span><span class="sxs-lookup"><span data-stu-id="a7bc1-114">Because auto-scale rules work at the scale set level (rather than at the Service Fabric level), auto-scale rules can remove Service Fabric nodes without shutting them down gracefully.</span></span> <span data-ttu-id="a7bc1-115">Goromba csomópont eltávolítása fog hagy "szellemrekord" a Service Fabric-csomópont állapota méretezési a műveletek után.</span><span class="sxs-lookup"><span data-stu-id="a7bc1-115">This rude node removal will leave 'ghost' Service Fabric node state behind after scale-in operations.</span></span> <span data-ttu-id="a7bc1-116">Egy adott (vagy egy szolgáltatás) kell rendszeresen törölnie a Service Fabric-fürtöt az eltávolított csomópont állapota.</span><span class="sxs-lookup"><span data-stu-id="a7bc1-116">An individual (or a service) would need to periodically clean up removed node state in the Service Fabric cluster.</span></span>
  - <span data-ttu-id="a7bc1-117">Vegye figyelembe, hogy az arany vagy ezüst tartóssági szint csomóponttípus automatikusan be lesz tisztítása csomópontokat eltávolítani.</span><span class="sxs-lookup"><span data-stu-id="a7bc1-117">Note that a node type with a durability level of Gold or Silver will automatically clean up removed nodes.</span></span>  
- <span data-ttu-id="a7bc1-118">Bár vannak [sok metrikák](../monitoring-and-diagnostics/insights-autoscale-common-metrics.md) automatikus méretezése szabályok által támogatott, akkor még mindig korlátozott.</span><span class="sxs-lookup"><span data-stu-id="a7bc1-118">Although there are [many metrics](../monitoring-and-diagnostics/insights-autoscale-common-metrics.md) supported by auto-scale rules, it is still a limited set.</span></span> <span data-ttu-id="a7bc1-119">Ha a forgatókönyv nem vonatkozik a készletben lévő egyes metrika alapján méretezéshez, majd automatikus méretezése szabályok nem lehet jó választás.</span><span class="sxs-lookup"><span data-stu-id="a7bc1-119">If your scenario calls for scaling based on some metric not covered in that set, then auto-scale rules may not be a good option.</span></span>

<span data-ttu-id="a7bc1-120">E tényezők alapján, érdemes több testreszabott automatikus méretezési modellek végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="a7bc1-120">Based on these limitations, you may wish to implement more customized automatic scaling models.</span></span> 

## <a name="scaling-apis"></a><span data-ttu-id="a7bc1-121">Méretezési API-k</span><span class="sxs-lookup"><span data-stu-id="a7bc1-121">Scaling APIs</span></span>
<span data-ttu-id="a7bc1-122">Az Azure API-k léteznek, amelyek lehetővé teszik a virtuális gép programozott módon használható alkalmazások méretezési készletek és a Service Fabric-fürtök.</span><span class="sxs-lookup"><span data-stu-id="a7bc1-122">Azure APIs exist which allow applications to programmatically work with virtual machine scale sets and Service Fabric clusters.</span></span> <span data-ttu-id="a7bc1-123">Meglévő automatikus méretezése lehetőségek nem működnek az adott esetben, ha ezen API-k lehetővé teszik egyéni méretezési logika végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="a7bc1-123">If existing auto-scale options don't work for your scenario, these APIs make it possible to implement custom scaling logic.</span></span> 

<span data-ttu-id="a7bc1-124">Egy megközelítést a "otthoni végrehajtott" az automatikus skálázás funkció végrehajtására egy új állapotmentes szolgáltatások hozzáadása a Service Fabric-alkalmazás skálázási műveleteinek a felügyeletét.</span><span class="sxs-lookup"><span data-stu-id="a7bc1-124">One approach to implementing this 'home-made' auto-scaling functionality is to add a new stateless service to the Service Fabric application to manage scaling operations.</span></span> <span data-ttu-id="a7bc1-125">A szolgáltatáson belül `RunAsync` módszer, eseményindítók készlete is megállapítja, hogy skálázás szükség (többek között a paraméterek maximális fürt például ellenőrzése és a méretezés cooldowns).</span><span class="sxs-lookup"><span data-stu-id="a7bc1-125">Within the service's `RunAsync` method, a set of triggers can determine if scaling is required (including checking parameters such as maximum cluster size and scaling cooldowns).</span></span>   

<span data-ttu-id="a7bc1-126">A virtuális gép méretezési készlet kapcsolati (mindkettő ellenőrizze a virtuálisgép-példányok jelenlegi száma és módosítható) használja az API a [Folyékonyan beszél Azure felügyeleti számítási könyvtár](https://www.nuget.org/packages/Microsoft.Azure.Management.Compute.Fluent/).</span><span class="sxs-lookup"><span data-stu-id="a7bc1-126">The API used for virtual machine scale set interactions (both to check the current number of virtual machine instances and to modify it) is the [fluent Azure Management Compute library](https://www.nuget.org/packages/Microsoft.Azure.Management.Compute.Fluent/).</span></span> <span data-ttu-id="a7bc1-127">A Folyékonyan beszél számítási kódtár biztosít egy könnyen használható API-t használják a virtuálisgép-méretezési készlet.</span><span class="sxs-lookup"><span data-stu-id="a7bc1-127">The fluent compute library provides an easy-to-use API for interacting with virtual machine scale sets.</span></span>

<span data-ttu-id="a7bc1-128">Segítségével kommunikál a Service Fabric-fürt magát, [System.Fabric.FabricClient](/dotnet/api/system.fabric.fabricclient).</span><span class="sxs-lookup"><span data-stu-id="a7bc1-128">To interact with the Service Fabric cluster itself, use [System.Fabric.FabricClient](/dotnet/api/system.fabric.fabricclient).</span></span>

<span data-ttu-id="a7bc1-129">A fürt méretezhető szolgáltatásként természetesen a méretezési kód nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="a7bc1-129">Of course, the scaling code doesn't need to run as a service in the cluster to be scaled.</span></span> <span data-ttu-id="a7bc1-130">Mindkét `IAzure` és `FabricClient` kapcsolódhatnak a kapcsolódó Azure-erőforrások távolról, így a méretezési szolgáltatás könnyen lehet egy konzolalkalmazás vagy a Windows-szolgáltatás fut a kívül a Service Fabric-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="a7bc1-130">Both `IAzure` and `FabricClient` can connect to their associated Azure resources remotely, so the scaling service could easily be a console application or Windows service running from outside the Service Fabric application.</span></span> 

## <a name="credential-management"></a><span data-ttu-id="a7bc1-131">Hitelesítő adatok kezelése</span><span class="sxs-lookup"><span data-stu-id="a7bc1-131">Credential management</span></span>
<span data-ttu-id="a7bc1-132">Méretezés kezelésére rendszerekben a szolgáltatás az egyik kihívás az, hogy a szolgáltatás kell tudni hozzáférni a virtuálisgép-méretezési készlet erőforrások egy interaktív bejelentkezés nélkül.</span><span class="sxs-lookup"><span data-stu-id="a7bc1-132">One challenge of writing a service to handle scaling is that the service must be able to access virtual machine scale set resources without an interactive login.</span></span> <span data-ttu-id="a7bc1-133">A Service Fabric-fürt használata esetén könnyen a méretezési szolgáltatás módosítja a saját Service Fabric-alkalmazás, de a méretezési eléréséhez szükséges hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="a7bc1-133">Accessing the Service Fabric cluster is easy if the scaling service is modifying its own Service Fabric application, but credentials are needed to access the scale set.</span></span> <span data-ttu-id="a7bc1-134">Jelentkezzen be, használhatja a [egyszerű](https://github.com/Azure/azure-sdk-for-net/blob/Fluent/AUTH.md#creating-a-service-principal-in-azure) létrehozni a [Azure CLI 2.0](https://github.com/azure/azure-cli).</span><span class="sxs-lookup"><span data-stu-id="a7bc1-134">To log in, you can use a [service principal](https://github.com/Azure/azure-sdk-for-net/blob/Fluent/AUTH.md#creating-a-service-principal-in-azure) created with the [Azure CLI 2.0](https://github.com/azure/azure-cli).</span></span>

<span data-ttu-id="a7bc1-135">A szolgáltatás egyszerű hozhatók létre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="a7bc1-135">A service principal can be created with the following steps:</span></span>

1. <span data-ttu-id="a7bc1-136">Jelentkezzen be az Azure parancssori felület (`az login`) egy olyan felhasználó nevében, aki hozzáféréssel rendelkezik a virtuálisgép-méretezési beállítása</span><span class="sxs-lookup"><span data-stu-id="a7bc1-136">Log in to the Azure CLI (`az login`) as a user with access to the virtual machine scale set</span></span>
2. <span data-ttu-id="a7bc1-137">Az egyszerű szolgáltatásnév létrehozása a`az ad sp create-for-rbac`</span><span class="sxs-lookup"><span data-stu-id="a7bc1-137">Create the service principal with `az ad sp create-for-rbac`</span></span>
    1. <span data-ttu-id="a7bc1-138">Jegyezze meg a appId (más néven "ügyfél-azonosító" máshol), a neve, a jelszót és a bérlői későbbi használatra.</span><span class="sxs-lookup"><span data-stu-id="a7bc1-138">Make note of the appId (called 'client ID' elsewhere), name, password, and tenant for later use.</span></span>
    2. <span data-ttu-id="a7bc1-139">Konfigurálnia kell az előfizetési azonosító, amely tekinthetők meg`az account list`</span><span class="sxs-lookup"><span data-stu-id="a7bc1-139">You will also need your subscription ID, which can be viewed with `az account list`</span></span>

<span data-ttu-id="a7bc1-140">A Folyékonyan beszél számítási könyvtár jelentkezhetnek be ezeket a hitelesítő adatokat az alábbiak szerint (vegye figyelembe, hogy core Folyékonyan beszél Azure típusok hasonlóan `IAzure` szerepelnek a [Microsoft.Azure.Management.Fluent](https://www.nuget.org/packages/Microsoft.Azure.Management.Fluent/) csomag):</span><span class="sxs-lookup"><span data-stu-id="a7bc1-140">The fluent compute library can log in using these credentials as follows (note that core fluent Azure types like `IAzure` are in the [Microsoft.Azure.Management.Fluent](https://www.nuget.org/packages/Microsoft.Azure.Management.Fluent/) package):</span></span>

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
    ServiceEventSource.Current.ServiceMessage(Context, "ERROR: Failed to login to Azure");
}
```

<span data-ttu-id="a7bc1-141">Miután bejelentkezett, méretezési készlet példányszáma lekérdezhetők, keresztül `AzureClient.VirtualMachineScaleSets.GetById(ScaleSetId).Capacity`.</span><span class="sxs-lookup"><span data-stu-id="a7bc1-141">Once logged in, scale set instance count can be queried via `AzureClient.VirtualMachineScaleSets.GetById(ScaleSetId).Capacity`.</span></span>

## <a name="scaling-out"></a><span data-ttu-id="a7bc1-142">Méretezés</span><span class="sxs-lookup"><span data-stu-id="a7bc1-142">Scaling out</span></span>
<span data-ttu-id="a7bc1-143">SDK használata a Folyékonyan beszél Azure számítási, példányok adhatók hozzá a virtuálisgép-méretezési pár hívások - beállítása</span><span class="sxs-lookup"><span data-stu-id="a7bc1-143">Using the fluent Azure compute SDK, instances can be added to the virtual machine scale set with just a few calls -</span></span>

```C#
var scaleSet = AzureClient.VirtualMachineScaleSets.GetById(ScaleSetId);
var newCapacity = (int)Math.Min(MaximumNodeCount, scaleSet.Capacity + 1);
scaleSet.Update().WithCapacity(newCapacity).Apply(); 
``` 

<span data-ttu-id="a7bc1-144">Azt is megteheti virtuális gép méretezési készlet méretét is felügyelhetők a PowerShell-parancsmagokkal.</span><span class="sxs-lookup"><span data-stu-id="a7bc1-144">Alternatively, virtual machine scale set size can also be managed with PowerShell cmdlets.</span></span> <span data-ttu-id="a7bc1-145">[`Get-AzureRmVmss`](https://docs.microsoft.com/en-us/powershell/module/azurerm.compute/get-azurermvmss)lekérheti a virtuálisgép-méretezési készlet objektumot.</span><span class="sxs-lookup"><span data-stu-id="a7bc1-145">[`Get-AzureRmVmss`](https://docs.microsoft.com/en-us/powershell/module/azurerm.compute/get-azurermvmss) can retrieve the virtual machine scale set object.</span></span> <span data-ttu-id="a7bc1-146">A jelenlegi kapacitásnál fogja tárolni a `.sku.capacity` tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="a7bc1-146">The current capacity will be stored in the `.sku.capacity` property.</span></span> <span data-ttu-id="a7bc1-147">Miután megváltoztatta a kapacitás a kívánt értékre, az Azure állítsa be a virtuálisgép-méretezési frissíthető a [ `Update-AzureRmVmss` ](https://docs.microsoft.com/en-us/powershell/module/azurerm.compute/update-azurermvmss) parancsot.</span><span class="sxs-lookup"><span data-stu-id="a7bc1-147">After changing the capacity to the desired value, the virtual machine scale set in Azure can be updated with the [`Update-AzureRmVmss`](https://docs.microsoft.com/en-us/powershell/module/azurerm.compute/update-azurermvmss) command.</span></span>

<span data-ttu-id="a7bc1-148">Ha manuálisan ad hozzá egy csomópont, a méretezési példány hozzáadása kell lennie egy új Service Fabric-csomópont el, mivel a méretezési sablon elegendő automatikusan új példányok csatlakoztatása a Service Fabric-fürt kiterjesztéseket is tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="a7bc1-148">As when adding a node manually, adding a scale set instance should be all that's needed to start a new Service Fabric node since the scale set template includes extensions to automatically join new instances to the Service Fabric cluster.</span></span> 

## <a name="scaling-in"></a><span data-ttu-id="a7bc1-149">A méretezés</span><span class="sxs-lookup"><span data-stu-id="a7bc1-149">Scaling in</span></span>

<span data-ttu-id="a7bc1-150">A méretezés hasonlít kiterjesztése.</span><span class="sxs-lookup"><span data-stu-id="a7bc1-150">Scaling in is similar to scaling out.</span></span> <span data-ttu-id="a7bc1-151">A tényleges virtuálisgép-méretezési beállítása módosítások vannak gyakorlatilag azonos.</span><span class="sxs-lookup"><span data-stu-id="a7bc1-151">The actual virtual machine scale set changes are practically the same.</span></span> <span data-ttu-id="a7bc1-152">De volt korábban bemutatott, a Service Fabric csak automatikusan törli a szükségtelenné az eltávolított csomópontokat a tartós arany vagy ezüst.</span><span class="sxs-lookup"><span data-stu-id="a7bc1-152">But, as was discussed previously, Service Fabric only automatically cleans up removed nodes with a durability of Gold or Silver.</span></span> <span data-ttu-id="a7bc1-153">Igen, a bronz-tartóssági méretezési a helyzet, szükség az eltávolítandó csomópont leállítása a Service Fabric-fürt kommunikál, majd távolítsa el az állapotát.</span><span class="sxs-lookup"><span data-stu-id="a7bc1-153">So, in the Bronze-durability scale-in case, it's necessary to interact with the Service Fabric cluster to shut down the node to be removed and then to remove its state.</span></span>

<span data-ttu-id="a7bc1-154">A csomópont előkészítése az leállítási magában foglalja a tekinti a csomópontot eltávolítani (a legutóbb hozzáadott csomópont) keresése és inaktiválása.</span><span class="sxs-lookup"><span data-stu-id="a7bc1-154">Preparing the node for shutdown involves finding the node to be removed (the most recently added node) and deactivating it.</span></span> <span data-ttu-id="a7bc1-155">Nem kezdőérték csomópontok újabb csomópontok található összehasonlításával `NodeInstanceId`.</span><span class="sxs-lookup"><span data-stu-id="a7bc1-155">For non-seed nodes, newer nodes can be found by comparing `NodeInstanceId`.</span></span> 

```C#
using (var client = new FabricClient())
{
    var mostRecentLiveNode = (await client.QueryManager.GetNodeListAsync())
        .Where(n => n.NodeType.Equals(NodeTypeToScale, StringComparison.OrdinalIgnoreCase))
        .Where(n => n.NodeStatus == System.Fabric.Query.NodeStatus.Up)
        .OrderByDescending(n => n.NodeInstanceId)
        .FirstOrDefault();
```

<span data-ttu-id="a7bc1-156">Vegye figyelembe, hogy *kezdőérték* csomópontok látszólag nem mindig kövesse az egyezmény nagyobb példány azonosítók először törlődnek.</span><span class="sxs-lookup"><span data-stu-id="a7bc1-156">Be aware that *seed* nodes don't seem to always follow the convention that greater instance IDs are removed first.</span></span>

<span data-ttu-id="a7bc1-157">Az eltávolítandó csomópont található, ha inaktiválhatók, és eltávolítja a azonos `FabricClient` példány és a `IAzure` példányának regisztrációját a korábbi.</span><span class="sxs-lookup"><span data-stu-id="a7bc1-157">Once the node to be removed is found, it can be deactivated and removed using the same `FabricClient` instance and the `IAzure` instance from earlier.</span></span>

```C#
var scaleSet = AzureClient.VirtualMachineScaleSets.GetById(ScaleSetId);

// Remove the node from the Service Fabric cluster
ServiceEventSource.Current.ServiceMessage(Context, $"Disabling node {mostRecentLiveNode.NodeName}");
await client.ClusterManager.DeactivateNodeAsync(mostRecentLiveNode.NodeName, NodeDeactivationIntent.RemoveNode);

// Wait (up to a timeout) for the node to gracefully shutdown
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

<span data-ttu-id="a7bc1-158">Mint meg, a PowerShell-parancsmagok a módosítását a virtuálisgép-méretezési készlet kapacitása is használható itt egy parancsfájl-kezelési megoldás hatékonyabb, ha.</span><span class="sxs-lookup"><span data-stu-id="a7bc1-158">As with scaling out, PowerShell cmdlets for modifying virtual machine scale set capacity can also be used here if a scripting approach is preferable.</span></span> <span data-ttu-id="a7bc1-159">A virtuálisgép-példányt eltávolítást követően a Service Fabric-csomópont állapota lehet eltávolítani.</span><span class="sxs-lookup"><span data-stu-id="a7bc1-159">Once the virtual machine instance is removed, Service Fabric node state can be removed.</span></span>

```C#
await client.ClusterManager.RemoveNodeStateAsync(mostRecentLiveNode.NodeName);
```

## <a name="potential-drawbacks"></a><span data-ttu-id="a7bc1-160">Lehetséges hátrányai</span><span class="sxs-lookup"><span data-stu-id="a7bc1-160">Potential drawbacks</span></span>

<span data-ttu-id="a7bc1-161">Ahogyan az a megelőző kódrészletek, létrehozása a saját skálázás szolgáltatás biztosítja a legmagasabb szintű vezérlő és az alkalmazás keresztül testreszabhatóság miatt a skálázás viselkedését.</span><span class="sxs-lookup"><span data-stu-id="a7bc1-161">As demonstrated in the preceding code snippets, creating your own scaling service provides the highest degree of control and customizability over your application's scaling behavior.</span></span> <span data-ttu-id="a7bc1-162">Ez lehet hasznos, ha pontosan meghatározhatja hogyan vagy ha egy alkalmazás méretezi-e a bejövő vagy kimenő igénylő forgatókönyvek.</span><span class="sxs-lookup"><span data-stu-id="a7bc1-162">This can be useful for scenarios requiring precise control over when or how an application scales in or out.</span></span> <span data-ttu-id="a7bc1-163">Azonban ez a vezérlő tartalmaz a kompromisszummal jár, kód összetettségét.</span><span class="sxs-lookup"><span data-stu-id="a7bc1-163">However, this control comes with a tradeoff of code complexity.</span></span> <span data-ttu-id="a7bc1-164">Ezzel a megközelítéssel azt jelenti, hogy saját méretezési kódot, amely nem triviális kell.</span><span class="sxs-lookup"><span data-stu-id="a7bc1-164">Using this approach means that you need to own scaling code, which is non-trivial.</span></span>

<span data-ttu-id="a7bc1-165">Hogyan meg kell megközelíti a Service Fabric skálázás attól függ, hogy a forgatókönyvéhez.</span><span class="sxs-lookup"><span data-stu-id="a7bc1-165">How you should approach Service Fabric scaling depends on your scenario.</span></span> <span data-ttu-id="a7bc1-166">Ha skálázás ritka, csomópontok hozzáadásához és eltávolításához manuálisan nem elegendő.</span><span class="sxs-lookup"><span data-stu-id="a7bc1-166">If scaling is uncommon, the ability to add or remove nodes manually is probably sufficient.</span></span> <span data-ttu-id="a7bc1-167">Az összetettebb forgatókönyveket automatikus méretezése szabályok és SDK-k teszi ki a szolgáltatás szoftveres kínál az hatékony alternatívák.</span><span class="sxs-lookup"><span data-stu-id="a7bc1-167">For more complex scenarios, auto-scale rules and SDKs exposing the ability to scale programmatically offer powerful alternatives.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a7bc1-168">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a7bc1-168">Next steps</span></span>

<span data-ttu-id="a7bc1-169">Első lépésként a saját automatikus skálázás logikát megvalósító, ismerkedjen meg a következő fogalmakat és hasznos API-kat:</span><span class="sxs-lookup"><span data-stu-id="a7bc1-169">To get started implementing your own auto-scaling logic, familiarize yourself with the following concepts and useful APIs:</span></span>

- [<span data-ttu-id="a7bc1-170">Manuális vagy automatikus skálázása szabályokkal skálázás</span><span class="sxs-lookup"><span data-stu-id="a7bc1-170">Scaling manually or with auto-scale rules</span></span>](./service-fabric-cluster-scale-up-down.md)
- <span data-ttu-id="a7bc1-171">[A .NET-hez Folyékonyan beszél Azure kezelési kódtárakat](https://github.com/Azure/azure-sdk-for-net/tree/Fluent) (hasznos a Service Fabric-fürt alapul szolgáló virtuálisgép-méretezési csoportok való kommunikáció)</span><span class="sxs-lookup"><span data-stu-id="a7bc1-171">[Fluent Azure Management Libraries for .NET](https://github.com/Azure/azure-sdk-for-net/tree/Fluent) (useful for interacting with a Service Fabric cluster's underlying virtual machine scale sets)</span></span>
- <span data-ttu-id="a7bc1-172">[System.Fabric.FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) (hasznos a Service Fabric-fürt és csomópontjainak való kommunikáció)</span><span class="sxs-lookup"><span data-stu-id="a7bc1-172">[System.Fabric.FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) (useful for interacting with a Service Fabric cluster and its nodes)</span></span>
