---
title: "Azure Batch feladatokat futtató virtuális gépeken költséghatékony alacsony prioritású (előzetes verzió) |} Microsoft Docs"
description: "Ismerje meg, alacsony prioritású virtuális gépeket az Azure Batch-munkaterhelések költségek csökkentése a kiépítése."
services: batch
author: mscurrell
manager: timlt
ms.assetid: dc6ba151-1718-468a-b455-2da549225ab2
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.workload: na
ms.date: 07/21/2017
ms.author: markscu
ms.openlocfilehash: 9bf0ac322020d8a8453011c3207c1930175db6d3
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="use-low-priority-vms-with-batch-preview"></a><span data-ttu-id="dfffe-103">Kis prioritású virtuális gépek használata a kötegelt (előzetes verzió)</span><span class="sxs-lookup"><span data-stu-id="dfffe-103">Use low-priority VMs with Batch (Preview)</span></span>

<span data-ttu-id="dfffe-104">Az Azure Batch biztosít alacsony-priorty virtuális gépek (VM) kötegelt feladatok a költségek csökkentése.</span><span class="sxs-lookup"><span data-stu-id="dfffe-104">Azure Batch offers low-priorty virtual machines (VMs) to reduce the cost of Batch workloads.</span></span> <span data-ttu-id="dfffe-105">Kis prioritású virtuális gépek új kötegelt munkaterhelés-típusok lehetővé teszik, adja meg a számítási teljesítményt, amely egyúttal gazdaságos nagy mennyiségű.</span><span class="sxs-lookup"><span data-stu-id="dfffe-105">Low-priority VMs make new types of Batch workloads possible by providing a large amount of compute power that is also economical.</span></span>

<span data-ttu-id="dfffe-106">Kis prioritású virtuális gépek előnyeit felesleges kapacitás az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="dfffe-106">Low-priority VMs take advantage of surplus capacity in Azure.</span></span> <span data-ttu-id="dfffe-107">Kis prioritású virtuális gépek a készletek megadásakor Azure Batch automatikusan használhatják az eredmény, ha elérhető.</span><span class="sxs-lookup"><span data-stu-id="dfffe-107">When you specify low-priority VMs in your pools, Azure Batch can automatically use this surplus when available.</span></span>

<span data-ttu-id="dfffe-108">Mi a fontosabb: az alacsony prioritású virtuális gép, hogy virtuális gépek az Azure-ban elérhető nincs felesleges kapacitás esetén előfordulhat, hogy foglalható.</span><span class="sxs-lookup"><span data-stu-id="dfffe-108">The tradeoff for using low-priority VMs is that those VMs may be preempted when no surplus capacity is available in Azure.</span></span> <span data-ttu-id="dfffe-109">Emiatt alacsony prioritású virtuális gépek legmegfelelőbb bizonyos munkaterhelések esetében.</span><span class="sxs-lookup"><span data-stu-id="dfffe-109">For this reason, low-priority VMs are most suitable for certain types of workloads.</span></span> <span data-ttu-id="dfffe-110">Alacsony prioritású virtuális gépek használata kötegelt és aszinkron feldolgozási terheléshez, ahol a feladat befejezési idő rugalmas, és a munka sok virtuális gép között van elosztva.</span><span class="sxs-lookup"><span data-stu-id="dfffe-110">Use low-priority VMs for batch and asynchronous processing workloads where the job completion time is flexible and the work is distributed across many VMs.</span></span>

<span data-ttu-id="dfffe-111">Kis prioritású virtuális gépek jelentősen olcsóbb dedikált virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="dfffe-111">Low-priority VMs are significantly less expensive than dedicated VMs.</span></span> <span data-ttu-id="dfffe-112">Díjszabása, lásd: [kötegelt árképzési](https://azure.microsoft.com/pricing/details/batch/).</span><span class="sxs-lookup"><span data-stu-id="dfffe-112">For pricing details, see [Batch Pricing](https://azure.microsoft.com/pricing/details/batch/).</span></span>

<span data-ttu-id="dfffe-113">Egy alacsony prioritású virtuális gépek további tárgyalását lásd a következő blogbejegyzésben található: [ár töredéke alatt, a számítástechnikai kötegelt](https://azure.microsoft.com/blog/announcing-public-preview-of-azure-batch-low-priority-vms/).</span><span class="sxs-lookup"><span data-stu-id="dfffe-113">For an additional discussion of low-priority VMs, see the blog post announcement: [Batch computing at a fraction of the price](https://azure.microsoft.com/blog/announcing-public-preview-of-azure-batch-low-priority-vms/).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="dfffe-114">Alacsony prioritású virtuális gép jelenleg előzetes verziójúak, és csak a kötegelt futó feladatok.</span><span class="sxs-lookup"><span data-stu-id="dfffe-114">Low-priority VMs are currently in preview, and are available only for workloads running in Batch.</span></span> 
>
>

## <a name="use-cases-for-low-priority-vms"></a><span data-ttu-id="dfffe-115">Kis prioritású virtuális gépek alkalmazási helyzetei</span><span class="sxs-lookup"><span data-stu-id="dfffe-115">Use cases for low-priority VMs</span></span>

<span data-ttu-id="dfffe-116">Alacsony prioritású virtuális gépeket, mi munkaterhelések is, és nem használja őket figyelembe véve?</span><span class="sxs-lookup"><span data-stu-id="dfffe-116">Given the characteristics of low-priority VMs, what workloads can and cannot use them?</span></span> <span data-ttu-id="dfffe-117">Általában kötegelt feldolgozási terheléshez amelyeket remekül beválik, feladatok számos párhuzamos feladat van felosztva, vagy sok feladat horizontálisan és sok virtuális gép pontjain.</span><span class="sxs-lookup"><span data-stu-id="dfffe-117">In general, batch processing workloads are a good fit, as jobs are broken into many parallel tasks or there are many jobs that are scaled out and distributed across many VMs.</span></span>

-   <span data-ttu-id="dfffe-118">Használja az Azure-ban felesleges kapacitás maximalizálása érdekében megfelelő feladatok lehet terjeszteni.</span><span class="sxs-lookup"><span data-stu-id="dfffe-118">To maximize use of surplus capacity in Azure, suitable jobs can scale out.</span></span>

-   <span data-ttu-id="dfffe-119">Alkalmanként a virtuális gépek nem érhető el, vagy fog foglalható, ami azt eredményezheti, hogy a feladat megszakítása és ismétlések és feladatok korlátozott kapacitás eredményezi.</span><span class="sxs-lookup"><span data-stu-id="dfffe-119">Occasionally VMs may not be available or will be preempted, which will result in reduced capacity for jobs and may lead to task interruption and reruns.</span></span> <span data-ttu-id="dfffe-120">Feladatok ezért rugalmas a futtatásához érvénybe belül kell lennie.</span><span class="sxs-lookup"><span data-stu-id="dfffe-120">Jobs must therefore be flexible in the time they can take to run.</span></span>

-   <span data-ttu-id="dfffe-121">Feladatok hosszabb ideig feladatok több érintheti, ha megszakad.</span><span class="sxs-lookup"><span data-stu-id="dfffe-121">Jobs with longer tasks may be impacted more if interrupted.</span></span> <span data-ttu-id="dfffe-122">Ha hosszú ideig futó feladatok végrehajtása ellenőrzőpontok mentése folyamatban van, akkor hajtható végre, majd a hatása megszakítás lenne sokkal kevesebb.</span><span class="sxs-lookup"><span data-stu-id="dfffe-122">If long-running tasks implement checkpointing to save progress as they execute, then the impact of interruption would be far less.</span></span> <span data-ttu-id="dfffe-123">Rövidebb végrehajtásának lassúságát a feladatok általában a legmegfelelőbb alacsony prioritású virtuális gépek, a megszakítás hatását sokkal kevesebb.</span><span class="sxs-lookup"><span data-stu-id="dfffe-123">Tasks with shorter execution times tend to work best with low-priority VMs as the impact of interruption is far less.</span></span>

-   <span data-ttu-id="dfffe-124">Hosszú futású MPI-feladatok, amelyek ténylegesen használják több virtuális gépek nincsenek kiválóan alkalmas alacsony prioritású virtuális gépek használatára, mert egy virtuális gép leállított valószínűleg vezet a teljes feladat nem futtatható újra kell.</span><span class="sxs-lookup"><span data-stu-id="dfffe-124">Long-running MPI jobs that utilize multiple VMs are not well suited to use low-priority VMs as one preempted VM will likely lead to the whole job having to be run again.</span></span>

<span data-ttu-id="dfffe-125">Néhány példa a kötegelt feldolgozásra használati esetek jól, alacsony prioritású virtuális gépek használatára alkalmas a következők:</span><span class="sxs-lookup"><span data-stu-id="dfffe-125">Some examples of batch processing use cases well suited to use low-priority VMs are:</span></span>

-   <span data-ttu-id="dfffe-126">**Fejlesztéshez és teszteléshez**: különösen akkor, ha a felügyeleti teendők központjaként megoldások fejlesztés alatt jelentős megtakarítást is kell megvalósítani.</span><span class="sxs-lookup"><span data-stu-id="dfffe-126">**Development and testing**: In particular, if large-scale solutions are being developed significant savings can be realized.</span></span> <span data-ttu-id="dfffe-127">Tesztelési minden típusú kihasználhassa, de nagy terhelés tesztelése és regressziós tesztelés kiváló használ.</span><span class="sxs-lookup"><span data-stu-id="dfffe-127">All types of testing can benefit, but large-scale load testing and regression testing are great uses.</span></span>

-   <span data-ttu-id="dfffe-128">**Igény szerinti kapacitás kiegészítése**: alacsony prioritású virtuális gépek segítségével kiegészíti az regular dedikált virtuális gépek – Ha elérhető, a feladatok méretezése, és ezért alacsonyabb költségek gyorsabb befejezéséhez; nem érhető el, ha az alapkonfiguráció dedikált virtuális gépek érhetők el.</span><span class="sxs-lookup"><span data-stu-id="dfffe-128">**Supplementing on-demand capacity**: Low-priority VMs can be used to supplement regular dedicated VMs - when available, jobs can scale and therefore complete quicker for lower cost; when not available, the baseline of dedicated VMs are available.</span></span>

-   <span data-ttu-id="dfffe-129">**Rugalmas feladat végrehajtási ideje**: esetén az idő-feladatok rugalmasságot kell végrehajtania, majd lehetséges elhagyta a kapacitás is megengedett; alacsony prioritású virtuális gépek hozzáadásával feladatok gyakran futtathatók gyorsabb és az alacsonyabb költségek.</span><span class="sxs-lookup"><span data-stu-id="dfffe-129">**Flexible job execution time**: If there is flexibility in the time jobs have to complete, then potential drops in capacity can be tolerated; however, with the addition of low-priority VMs jobs will frequently run faster and for a lower cost.</span></span>

<span data-ttu-id="dfffe-130">Kötegelt készletek beállítható úgy, hogy számos módja, attól függően, hogy a feladat végrehajtási ideje rugalmasságot alacsony prioritású virtuális gépek használata:</span><span class="sxs-lookup"><span data-stu-id="dfffe-130">Batch pools can be configured to use low-priority VMs in a few ways, depending on the flexibility in job execution time:</span></span>

-   <span data-ttu-id="dfffe-131">Kis prioritású virtuális gépek kizárólag a készletben használható, és kötegelt egyszerűen állítsa helyre a preempted kapacitás, ha elérhető.</span><span class="sxs-lookup"><span data-stu-id="dfffe-131">Low-priority VMs can solely be used in a pool and Batch will simply recover any preempted capacity when available.</span></span> <span data-ttu-id="dfffe-132">Ez módja a legolcsóbb feladatok végrehajtásához, csak alacsony prioritású virtuális gép használja.</span><span class="sxs-lookup"><span data-stu-id="dfffe-132">This is the cheapest way to execute jobs as only low-priority VMs are used.</span></span>

-   <span data-ttu-id="dfffe-133">Kis prioritású virtuális gépek dedikált virtuális gépek rögzített alapterv együtt használható.</span><span class="sxs-lookup"><span data-stu-id="dfffe-133">Low-priority VMs can be used in conjunction with a fixed baseline of dedicated VMs.</span></span> <span data-ttu-id="dfffe-134">A dedikált virtuális gépek rögzített száma biztosítja, hogy mindig van néhány kapacitás tartani a feladat halad.</span><span class="sxs-lookup"><span data-stu-id="dfffe-134">The fixed number of dedicated VMs ensures there is always some capacity to keep a job progressing.</span></span>

-   <span data-ttu-id="dfffe-135">Lehet dedikált és alacsony prioritású virtuális gépeket, a dinamikus kombinációját, hogy az olcsóbb alacsony prioritású virtuális gépek kizárólag akkor használatosak, ha elérhető, de a teljes árú dedikált virtuális gépek vannak kiterjesztett szükség esetén elérhető, a feladatok elmélyedne a kapacitás minimális tartani.</span><span class="sxs-lookup"><span data-stu-id="dfffe-135">There can be dynamic mix of dedicated and low-priority VMs, so that the cheaper low-priority VMs are solely used when available, but the full-priced dedicated VMs are scaled up when required, to keep a minimum amount of capacity available to keep the jobs progressing.</span></span>

## <a name="batch-support-for-low-priority-vms"></a><span data-ttu-id="dfffe-136">Kis prioritású virtuális gépek kötegelt támogatása</span><span class="sxs-lookup"><span data-stu-id="dfffe-136">Batch support for low-priority VMs</span></span>

<span data-ttu-id="dfffe-137">Az Azure Batch szolgáltatás különböző képességeket, amelyek megkönnyítik az használnak, és igénybe vehesse az alacsony prioritású virtuális gépek:</span><span class="sxs-lookup"><span data-stu-id="dfffe-137">Azure Batch provides several capabilities that make it easy to consume and benefit from low-priority VMs:</span></span>

-   <span data-ttu-id="dfffe-138">Kötegelt készletek dedikált virtuális gépek és az alacsonyabb prioritású virtuális gépek is tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="dfffe-138">Batch pools can contain both dedicated VMs and low-priority VMs.</span></span> <span data-ttu-id="dfffe-139">A virtuális gép típusonkénti számát a készlet létrehozásakor, vagy megváltozott a meglévő készletek, az explicit átméretezés vagy automatikus skálázása használatával bármikor adható meg.</span><span class="sxs-lookup"><span data-stu-id="dfffe-139">The number of each type of VM can be specified when a pool is created, or changed at any time for an existing pool, using the explicit resize operation or using auto-scale.</span></span> <span data-ttu-id="dfffe-140">Feladat- és küldésének változatlan marad, és nem kell a virtuális gép típusú, a készlet érintett.</span><span class="sxs-lookup"><span data-stu-id="dfffe-140">Job and task submission can remain unchanged and do not need to be concerned with the VM types in the pool.</span></span> <span data-ttu-id="dfffe-141">Úgy is is rendelkezik, és teljesen használnak alacsony prioritású virtuális gépeket, feladatok, de dedikált virtuális gépeinek léptetéses olcsón futtatása, ha a kapacitás, futó feladatok tartani a minimális küszöbérték alá csökken.</span><span class="sxs-lookup"><span data-stu-id="dfffe-141">It is also possible to have a pool completely use low-priority VMs to run jobs as cheaply as possible, but spin up dedicated VMs if the capacity drops below a minimum threshold, to keep jobs running.</span></span>

-   <span data-ttu-id="dfffe-142">Kötegelt készletek automatikusan kereshető a cél alacsony prioritású virtuális gépek számát.</span><span class="sxs-lookup"><span data-stu-id="dfffe-142">Batch pools automatically seek to the target number of low-priority VMs.</span></span> <span data-ttu-id="dfffe-143">Ha a leállított virtuális gépeket, majd kötegelt megpróbálja cserélje ki az elveszett kapacitás, és térjen vissza a cél.</span><span class="sxs-lookup"><span data-stu-id="dfffe-143">If VMs are preempted, then Batch will attempt to replace the lost capacity and return to the target.</span></span>

-   <span data-ttu-id="dfffe-144">Folyamatban megszakadt, feladatok, ha kötegelt azonosítja, és automatikusan újra a várólistába helyezi feladatok újra kell futtatni.</span><span class="sxs-lookup"><span data-stu-id="dfffe-144">In the case of tasks being interrupted, Batch will detect and automatically requeue tasks to be run again.</span></span>

-   <span data-ttu-id="dfffe-145">Kis prioritású virtuális gépek eltér dedikált virtuális core kvóta rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="dfffe-145">Low-priority VMs have a core quota that differs from that of dedicated VMs.</span></span> 
    <span data-ttu-id="dfffe-146">Az ajánlat, alacsony prioritású virtuális gépek nem nagyobb, mint a dedikált virtuális gépek, mert alacsony prioritású virtuális gépek költséget.</span><span class="sxs-lookup"><span data-stu-id="dfffe-146">The quote for low-priority VMs is higher than that of dedicated VMs, because low-priority VMs cost less.</span></span> <span data-ttu-id="dfffe-147">Lásd: [a Batch szolgáltatás kvótái és korlátai](batch-quota-limit.md#resource-quotas) további információt.</span><span class="sxs-lookup"><span data-stu-id="dfffe-147">See [Batch service quotas and limits](batch-quota-limit.md#resource-quotas) for more information.</span></span>    

> [!NOTE]
> <span data-ttu-id="dfffe-148">Alacsony prioritású virtuális gépek jelenleg nem támogatottak a Batch-fiókok, ahol a tárolókészlet foglalási mód értéke [felhasználói előfizetési](batch-account-create-portal.md#user-subscription-mode).</span><span class="sxs-lookup"><span data-stu-id="dfffe-148">Low-priority VMs are not currently supported for Batch accounts where the pool allocation mode is set to [User subscription](batch-account-create-portal.md#user-subscription-mode).</span></span>
>
>

## <a name="create-and-update-pools"></a><span data-ttu-id="dfffe-149">Hozzon létre, és a készletek frissítése</span><span class="sxs-lookup"><span data-stu-id="dfffe-149">Create and update pools</span></span>

<span data-ttu-id="dfffe-150">A Batch-készlet dedikált és alacsony prioritású virtuális gép (más néven a a számítási csomópontokkal) tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="dfffe-150">A Batch pool can contain both dedicated and low-priority VMs (also referred to as compute nodes).</span></span> <span data-ttu-id="dfffe-151">Beállíthatja a számítási csomópontok száma cél dedikált és alacsony prioritású virtuális gép esetében.</span><span class="sxs-lookup"><span data-stu-id="dfffe-151">You can set the target number of compute nodes for both dedicated and low-priority VMs.</span></span> <span data-ttu-id="dfffe-152">A tároló csomópontok száma azt a van a tárolókészletben kívánt virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="dfffe-152">The target number of nodes specifies the number of VMs you want to have in the pool.</span></span>

<span data-ttu-id="dfffe-153">Például készlet létrehozása a virtuális gépek Azure-felhőszolgáltatásban egy target 5 dedikált virtuális gépek és 20 alacsony prioritású virtuális gép:</span><span class="sxs-lookup"><span data-stu-id="dfffe-153">For example, to create a pool using Azure cloud service VMs with a target of 5 dedicated VMs and 20 low-priority VMs:</span></span>

```csharp
CloudPool pool = batchClient.PoolOperations.CreatePool(
    poolId: "cspool",
    targetDedicatedComputeNodes: 5,
    targetLowPriorityComputeNodes: 20,
    virtualMachineSize: "Standard_D2_v2",
    cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4") // WS 2012 R2
);
```

<span data-ttu-id="dfffe-154">Készlet létrehozása az Azure virtuális gépek (ebben az esetben Linux virtuális gépek) 5 cél dedikált használó virtuális gépek és 20 alacsony prioritású virtuális gép:</span><span class="sxs-lookup"><span data-stu-id="dfffe-154">To create a pool using Azure virtual machines (in this case Linux VMs) with a target of 5 dedicated VMs and 20 low-priority VMs:</span></span>

```csharp
ImageReference imageRef = new ImageReference(
    publisher: "Canonical",
    offer: "UbuntuServer",
    sku: "16.04.0-LTS",
    version: "latest");

// Create the pool
VirtualMachineConfiguration virtualMachineConfiguration =
    new VirtualMachineConfiguration("batch.node.ubuntu 16.04", imageRef);

pool = batchClient.PoolOperations.CreatePool(
    poolId: "vmpool",
    targetDedicatedComputeNodes: 5,
    targetLowPriorityComputeNodes: 20,
    virtualMachineSize: "Standard\_D2\_v2",
    virtualMachineConfiguration: virtualMachineConfiguration);
```

<span data-ttu-id="dfffe-155">Dedikált és alacsony prioritású virtuális gépek kaphat a csomópontok száma:</span><span class="sxs-lookup"><span data-stu-id="dfffe-155">You can get the current number of nodes for both dedicated and low-priority VMs:</span></span>

```csharp
int? numDedicated = pool1.CurrentDedicatedComputeNodes;
int? numLowPri = pool1.CurrentLowPriorityComputeNodes;
```

<span data-ttu-id="dfffe-156">Készlet csomópontjain van-e a tulajdonság azt jelzi, ha a csomópont egy alacsony prioritású vagy dedikált virtuális Gépet:</span><span class="sxs-lookup"><span data-stu-id="dfffe-156">Pool nodes have a property to indicate if the node is a dedicated or low-priority VM:</span></span>

```csharp
bool? isNodeDedicated = poolNode.IsDedicated;
```

<span data-ttu-id="dfffe-157">Amikor egy alkalmazáskészlet egy vagy több csomópontja részesítés, a készlet egy lista csomópontok művelet továbbra is visszaállítja azokat a csomópontokat, alacsony prioritású csomópontok száma változatlan marad, de azokat a csomópontokat, kell az állapot értéke a **Prioritás miatt kihagyva** állapotát.</span><span class="sxs-lookup"><span data-stu-id="dfffe-157">When one or more nodes in a pool are preempted, a list nodes operation on the pool will still return those nodes, the current number of low-priority nodes will remain unchanged, but those nodes will have their state set to the **Preempted** state.</span></span> <span data-ttu-id="dfffe-158">Kötegelt megkísérli található virtuális gépek cserélni, és ha sikeres, a csomópontok végighaladnia **létrehozása** , majd **indítása** állapotok ahhoz, hogy elérhető-e a feladat végrehajtásához, ugyanúgy, mint az új csomópontok.</span><span class="sxs-lookup"><span data-stu-id="dfffe-158">Batch will attempt to find replacement VMs and, if successful, the nodes will go through **Creating** and then **Starting** states before becoming available for task execution, just like new nodes.</span></span>

## <a name="scale-a-pool-containing-low-priority-vms"></a><span data-ttu-id="dfffe-159">Kis prioritású virtuális gépeket tartalmazó készlet méretezése</span><span class="sxs-lookup"><span data-stu-id="dfffe-159">Scale a pool containing low-priority VMs</span></span>

<span data-ttu-id="dfffe-160">Csakúgy, mint a gyűjtők kizárólag álló dedikált virtuális gépek, egy készletet, alacsony prioritású virtuális gépeket tartalmazó az átméretezési metódus hívása vagy automatikus skálázása segítségével méretezése.</span><span class="sxs-lookup"><span data-stu-id="dfffe-160">As with pools solely consisting of dedicated VMs, it is possible to scale a pool containing low-priority VMs by calling the Resize method or by using auto-scale.</span></span>

<span data-ttu-id="dfffe-161">Az alkalmazáskészlet átméretezés egy második választható-paramétert fogad, amely frissíti a **targetLowPriorityNodes**:</span><span class="sxs-lookup"><span data-stu-id="dfffe-161">The pool resize operation takes a second optional parameter that updates the value of **targetLowPriorityNodes**:</span></span>

```csharp
pool.Resize(targetDedicatedComputeNodes: 0, targetLowPriorityComputeNodes: 25);
```

<span data-ttu-id="dfffe-162">A készlet automatikus méretezése képlet az alábbiak szerint támogatja az alacsony prioritású virtuális gépek:</span><span class="sxs-lookup"><span data-stu-id="dfffe-162">The pool auto-scale formula supports low-priority VMs as follows:</span></span>

-   <span data-ttu-id="dfffe-163">Futtasson, vagy a szolgáltatás által definiált változó értékét állíthatja be **$TargetLowPriorityNodes**.</span><span class="sxs-lookup"><span data-stu-id="dfffe-163">You can get or set the value of the service-defined variable **$TargetLowPriorityNodes**.</span></span>

-   <span data-ttu-id="dfffe-164">A szolgáltatás által definiált változó értékének kaphat **$CurrentLowPriorityNodes**.</span><span class="sxs-lookup"><span data-stu-id="dfffe-164">You can get the value of the service-defined variable **$CurrentLowPriorityNodes**.</span></span>

-   <span data-ttu-id="dfffe-165">A szolgáltatás által definiált változó értékének kaphat **$PreemptedNodeCount**.</span><span class="sxs-lookup"><span data-stu-id="dfffe-165">You can get the value of the service-defined variable **$PreemptedNodeCount**.</span></span> 
    <span data-ttu-id="dfffe-166">Ez a változó preempted állapotban csomópontok számát adja vissza, és lehetővé teszi a méretezési felfelé vagy lefelé a dedikált csomópontok, attól függően, hogy nem használható a leállított csomópontok száma száma.</span><span class="sxs-lookup"><span data-stu-id="dfffe-166">This variable returns the number of nodes in the preempted state and allows you to scale up or down the number of dedicated nodes, depending on the number of preempted nodes that are unavailable.</span></span>

## <a name="jobs-and-tasks"></a><span data-ttu-id="dfffe-167">Feladatok és feladatok</span><span class="sxs-lookup"><span data-stu-id="dfffe-167">Jobs and tasks</span></span>

<span data-ttu-id="dfffe-168">Feladatok és a feladatok csekély-támogatásra van szüksége az alacsony prioritású csomópont; a csak támogatása a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="dfffe-168">Jobs and tasks require very little support for low-priority nodes; the only support is as follows:</span></span>

-   <span data-ttu-id="dfffe-169">Egy feladat JobManagerTask tulajdonságnak egy új tulajdonság **AllowLowPriorityNode**.</span><span class="sxs-lookup"><span data-stu-id="dfffe-169">The JobManagerTask property of a job has a new property, **AllowLowPriorityNode**.</span></span> 
    <span data-ttu-id="dfffe-170">Ez a tulajdonság értéke igaz, ha a kezelő feladat egy dedikált vagy alacsony prioritású csomópont is ütemezhető.</span><span class="sxs-lookup"><span data-stu-id="dfffe-170">When this property is true, the job manager task can be scheduled on either a dedicated or low-priority node.</span></span> <span data-ttu-id="dfffe-171">Ez a tulajdonság értéke HAMIS, ha a kezelő feladat ütemezi a csak egy dedikált csomópontra.</span><span class="sxs-lookup"><span data-stu-id="dfffe-171">If this property is false, the job manager task will be scheduled to a dedicated node only.</span></span>

-   <span data-ttu-id="dfffe-172">Egy [környezeti változó](batch-compute-node-environment-variables.md) érhető el egy feladat alkalmazást, hogy megállapíthassa, hogy egy alacsony prioritású vagy dedikált csomóponton futó.</span><span class="sxs-lookup"><span data-stu-id="dfffe-172">An [environment variable](batch-compute-node-environment-variables.md) is available to a task application so that it can determine whether it is running on a low-priority or dedicated node.</span></span> <span data-ttu-id="dfffe-173">A környezeti változó AZ_BATCH_NODE_IS_DEDICATED.</span><span class="sxs-lookup"><span data-stu-id="dfffe-173">The environment variable is AZ_BATCH_NODE_IS_DEDICATED.</span></span>

## <a name="handling-preemption"></a><span data-ttu-id="dfffe-174">A megelőlegezés kezelése</span><span class="sxs-lookup"><span data-stu-id="dfffe-174">Handling preemption</span></span>

<span data-ttu-id="dfffe-175">Virtuális gépek alkalmanként foglalható; Ha ez történik, a Batch a következőket teszi:</span><span class="sxs-lookup"><span data-stu-id="dfffe-175">VMs may occasionally be preempted; when this happens, Batch does the following:</span></span>

-   <span data-ttu-id="dfffe-176">A preempted virtuális gépek rendelkezik a frissített állapotukra **Prioritás miatt kihagyva**.</span><span class="sxs-lookup"><span data-stu-id="dfffe-176">The preempted VMs have their state updated to **Preempted**.</span></span>
-   <span data-ttu-id="dfffe-177">Ha a csomópont a leállított virtuális gépeken futó feladatokat, majd ezeket a feladatokat helyezte és futtassa újból.</span><span class="sxs-lookup"><span data-stu-id="dfffe-177">If tasks were running on the preempted node VMs, then those tasks are requeued and run again.</span></span>
-   <span data-ttu-id="dfffe-178">A virtuális gép hatékonyan törölték, ami a virtuális gép elvesztését helyileg tárolt összes adatot.</span><span class="sxs-lookup"><span data-stu-id="dfffe-178">The VM is effectively deleted, leading to any data stored locally on the VM being lost.</span></span>
-   <span data-ttu-id="dfffe-179">A készlet folyamatosan kísérletet tesz az alacsony prioritású csomópontok elérhető cél számát.</span><span class="sxs-lookup"><span data-stu-id="dfffe-179">The pool continually attempts to reach the target number of low-priority nodes available.</span></span> <span data-ttu-id="dfffe-180">Ha helyettesítő kapacitás tartalmaz, a csomópontok tartani az azonosítók, de újbóli inicializálása, keresztül haladó **létrehozása** és **indítása** előtt a feladat ütemezés szerint.</span><span class="sxs-lookup"><span data-stu-id="dfffe-180">When replacement capacity is found, the nodes keep their ids, but are re-initialized, going through **Creating** and **Starting** states before they are available for task scheduling.</span></span>
-   <span data-ttu-id="dfffe-181">A megelőlegezés számát, az Azure portálon metrika érhetők el.</span><span class="sxs-lookup"><span data-stu-id="dfffe-181">Preemption counts are available as a metric in the Azure portal.</span></span>

## <a name="metrics"></a><span data-ttu-id="dfffe-182">Mérőszámok</span><span class="sxs-lookup"><span data-stu-id="dfffe-182">Metrics</span></span>

<span data-ttu-id="dfffe-183">Új mérőszámok érhetők el a [Azure-portálon](https://portal.azure.com) az alacsony prioritású csomópont.</span><span class="sxs-lookup"><span data-stu-id="dfffe-183">New metrics are available in the [Azure portal](https://portal.azure.com) for low-priority nodes.</span></span> <span data-ttu-id="dfffe-184">Ezen adatok gyűjtése le van:</span><span class="sxs-lookup"><span data-stu-id="dfffe-184">These metrics are:</span></span>

- <span data-ttu-id="dfffe-185">Alacsony prioritású csomópontok száma</span><span class="sxs-lookup"><span data-stu-id="dfffe-185">Low-Priority Node Count</span></span>
- <span data-ttu-id="dfffe-186">Alacsony prioritású magok száma</span><span class="sxs-lookup"><span data-stu-id="dfffe-186">Low-Priority Core Count</span></span> 
- <span data-ttu-id="dfffe-187">Csomópont Count részesítés</span><span class="sxs-lookup"><span data-stu-id="dfffe-187">Preempted Node Count</span></span>

<span data-ttu-id="dfffe-188">Metrikák megtekintése az Azure-portálon:</span><span class="sxs-lookup"><span data-stu-id="dfffe-188">To view metrics in the Azure portal:</span></span>

1. <span data-ttu-id="dfffe-189">Keresse meg a Batch-fiók a portálon, és a Batch-fiók beállításainak megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="dfffe-189">Navigate to your Batch account in the portal, and view the settings for your Batch account.</span></span>
2. <span data-ttu-id="dfffe-190">Válassza ki **metrikák** a a **figyelés** szakasz.</span><span class="sxs-lookup"><span data-stu-id="dfffe-190">Select **Metrics** from the **Monitoring** section.</span></span>
3. <span data-ttu-id="dfffe-191">Válassza ki a vár a metrikák a **elérhető** listája.</span><span class="sxs-lookup"><span data-stu-id="dfffe-191">Select the metrics you desire from the **Available Metrics** list.</span></span>

![Alacsony prioritású csomópontok metrikák](media/batch-low-pri-vms/low-pri-metrics.png)

## <a name="next-steps"></a><span data-ttu-id="dfffe-193">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="dfffe-193">Next steps</span></span>

* <span data-ttu-id="dfffe-194">Olvassa el [Az Azure Batch funkcióinak áttekintése](batch-api-basics.md) című témakört, amely hasznos információkkal szolgál a Batch használatára készülőknek.</span><span class="sxs-lookup"><span data-stu-id="dfffe-194">Read the [Batch feature overview for developers](batch-api-basics.md), essential information for anyone preparing to use Batch.</span></span> <span data-ttu-id="dfffe-195">A cikk a Batch szolgáltatás erőforrásainak, például a készleteknek, csomópontoknak, feladatoknak, tevékenységeknek és sok olyan API funkciónak a részletesebb információit tartalmazza, amelyeket a Batch-alkalmazás kiépítésekor használhat.</span><span class="sxs-lookup"><span data-stu-id="dfffe-195">The article contains more detailed information about Batch service resources like pools, nodes, jobs, and tasks, and the many API features that you can use while building your Batch application.</span></span>
* <span data-ttu-id="dfffe-196">Megismerheti a Batch-megoldások fejlesztéséhez rendelkezésre álló [Batch API-kat és eszközöket](batch-apis-tools.md).</span><span class="sxs-lookup"><span data-stu-id="dfffe-196">Learn about the [Batch APIs and tools](batch-apis-tools.md) available for building Batch solutions.</span></span>
