---
title: "Azure Batch munkaterhelését aaaRun költséghatékony alacsony prioritású virtuális gépek (előzetes verzió) |} Microsoft Docs"
description: "Ismerje meg, hogyan tooprovision alacsony prioritású virtuális gépek tooreduce hello Azure Batch-munkaterhelések költsége."
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
ms.openlocfilehash: 91a5e89a819d05583e6b146932d925e217b4be4e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-low-priority-vms-with-batch-preview"></a><span data-ttu-id="7b6f2-103">Kis prioritású virtuális gépek használata a kötegelt (előzetes verzió)</span><span class="sxs-lookup"><span data-stu-id="7b6f2-103">Use low-priority VMs with Batch (Preview)</span></span>

<span data-ttu-id="7b6f2-104">Az Azure Batch biztosít alacsony-priorty virtuális gépek (VM) tooreduce hello kötegelt munkaterhelések költségét.</span><span class="sxs-lookup"><span data-stu-id="7b6f2-104">Azure Batch offers low-priorty virtual machines (VMs) tooreduce hello cost of Batch workloads.</span></span> <span data-ttu-id="7b6f2-105">Kis prioritású virtuális gépek új kötegelt munkaterhelés-típusok lehetővé teszik, adja meg a számítási teljesítményt, amely egyúttal gazdaságos nagy mennyiségű.</span><span class="sxs-lookup"><span data-stu-id="7b6f2-105">Low-priority VMs make new types of Batch workloads possible by providing a large amount of compute power that is also economical.</span></span>

<span data-ttu-id="7b6f2-106">Kis prioritású virtuális gépek előnyeit felesleges kapacitás az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="7b6f2-106">Low-priority VMs take advantage of surplus capacity in Azure.</span></span> <span data-ttu-id="7b6f2-107">Kis prioritású virtuális gépek a készletek megadásakor Azure Batch automatikusan használhatják az eredmény, ha elérhető.</span><span class="sxs-lookup"><span data-stu-id="7b6f2-107">When you specify low-priority VMs in your pools, Azure Batch can automatically use this surplus when available.</span></span>

<span data-ttu-id="7b6f2-108">kis prioritású virtuális gépek használatának hello kompromisszumot, hogy virtuális gépek az Azure-ban elérhető nincs felesleges kapacitás esetén előfordulhat, hogy foglalható.</span><span class="sxs-lookup"><span data-stu-id="7b6f2-108">hello tradeoff for using low-priority VMs is that those VMs may be preempted when no surplus capacity is available in Azure.</span></span> <span data-ttu-id="7b6f2-109">Emiatt alacsony prioritású virtuális gépek legmegfelelőbb bizonyos munkaterhelések esetében.</span><span class="sxs-lookup"><span data-stu-id="7b6f2-109">For this reason, low-priority VMs are most suitable for certain types of workloads.</span></span> <span data-ttu-id="7b6f2-110">Alacsony prioritású virtuális gépek használata kötegelt és aszinkron feldolgozási terheléshez, ahol hello feladat befejezési idő rugalmas, és hello munkahelyi sok virtuális gép között van elosztva.</span><span class="sxs-lookup"><span data-stu-id="7b6f2-110">Use low-priority VMs for batch and asynchronous processing workloads where hello job completion time is flexible and hello work is distributed across many VMs.</span></span>

<span data-ttu-id="7b6f2-111">Kis prioritású virtuális gépek jelentősen olcsóbb dedikált virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="7b6f2-111">Low-priority VMs are significantly less expensive than dedicated VMs.</span></span> <span data-ttu-id="7b6f2-112">Díjszabása, lásd: [kötegelt árképzési](https://azure.microsoft.com/pricing/details/batch/).</span><span class="sxs-lookup"><span data-stu-id="7b6f2-112">For pricing details, see [Batch Pricing](https://azure.microsoft.com/pricing/details/batch/).</span></span>

<span data-ttu-id="7b6f2-113">Egy alacsony prioritású virtuális gépek további tárgyalását lásd: hello blogbejegyzésben található: [hello ár töredéke alatt, a számítástechnikai kötegelt](https://azure.microsoft.com/blog/announcing-public-preview-of-azure-batch-low-priority-vms/).</span><span class="sxs-lookup"><span data-stu-id="7b6f2-113">For an additional discussion of low-priority VMs, see hello blog post announcement: [Batch computing at a fraction of hello price](https://azure.microsoft.com/blog/announcing-public-preview-of-azure-batch-low-priority-vms/).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7b6f2-114">Alacsony prioritású virtuális gép jelenleg előzetes verziójúak, és csak a kötegelt futó feladatok.</span><span class="sxs-lookup"><span data-stu-id="7b6f2-114">Low-priority VMs are currently in preview, and are available only for workloads running in Batch.</span></span> 
>
>

## <a name="use-cases-for-low-priority-vms"></a><span data-ttu-id="7b6f2-115">Kis prioritású virtuális gépek alkalmazási helyzetei</span><span class="sxs-lookup"><span data-stu-id="7b6f2-115">Use cases for low-priority VMs</span></span>

<span data-ttu-id="7b6f2-116">Alacsony prioritású virtuális gépek jellemzői hello kap, milyen számítási feladatokat is, és nem használja őket?</span><span class="sxs-lookup"><span data-stu-id="7b6f2-116">Given hello characteristics of low-priority VMs, what workloads can and cannot use them?</span></span> <span data-ttu-id="7b6f2-117">Általában kötegelt feldolgozási terheléshez amelyeket remekül beválik, feladatok számos párhuzamos feladat van felosztva, vagy sok feladat horizontálisan és sok virtuális gép pontjain.</span><span class="sxs-lookup"><span data-stu-id="7b6f2-117">In general, batch processing workloads are a good fit, as jobs are broken into many parallel tasks or there are many jobs that are scaled out and distributed across many VMs.</span></span>

-   <span data-ttu-id="7b6f2-118">az Azure, a megfelelő feladatoknál felesleges kapacitás toomaximize használatát lehet horizontálisan.</span><span class="sxs-lookup"><span data-stu-id="7b6f2-118">toomaximize use of surplus capacity in Azure, suitable jobs can scale out.</span></span>

-   <span data-ttu-id="7b6f2-119">Alkalmanként a virtuális gépek nem érhető el, vagy fog foglalható, amely a feladatok korlátozott kapacitás eredményez, így tootask megszakítás és ismétlések.</span><span class="sxs-lookup"><span data-stu-id="7b6f2-119">Occasionally VMs may not be available or will be preempted, which will result in reduced capacity for jobs and may lead tootask interruption and reruns.</span></span> <span data-ttu-id="7b6f2-120">Feladatok ezért rugalmas hello időben toorun érvénybe kell lennie.</span><span class="sxs-lookup"><span data-stu-id="7b6f2-120">Jobs must therefore be flexible in hello time they can take toorun.</span></span>

-   <span data-ttu-id="7b6f2-121">Feladatok hosszabb ideig feladatok több érintheti, ha megszakad.</span><span class="sxs-lookup"><span data-stu-id="7b6f2-121">Jobs with longer tasks may be impacted more if interrupted.</span></span> <span data-ttu-id="7b6f2-122">Ha hosszú ideig futó feladatok végrehajtása ellenőrzőpontok toosave folyamatban van, akkor hajtható végre, majd a hatása megszakítás lenne sokkal kevesebb.</span><span class="sxs-lookup"><span data-stu-id="7b6f2-122">If long-running tasks implement checkpointing toosave progress as they execute, then the impact of interruption would be far less.</span></span> <span data-ttu-id="7b6f2-123">Rövidebb végrehajtásának lassúságát a feladatok általában toowork legjobb alacsony prioritású virtuális gépek megszakítás hello hatása sokkal kevesebb mint.</span><span class="sxs-lookup"><span data-stu-id="7b6f2-123">Tasks with shorter execution times tend toowork best with low-priority VMs as hello impact of interruption is far less.</span></span>

-   <span data-ttu-id="7b6f2-124">Hosszú futású MPI-feladatok, amelyek ténylegesen használják több virtuális gépek nincsenek kiválóan alkalmas toouse alacsony prioritású virtuális gépeket egy leállított virtuális gépként valószínűleg az érdeklődési toohello teljes feladat újrafuttatása toobe rendelkező lesz.</span><span class="sxs-lookup"><span data-stu-id="7b6f2-124">Long-running MPI jobs that utilize multiple VMs are not well suited toouse low-priority VMs as one preempted VM will likely lead toohello whole job having toobe run again.</span></span>

<span data-ttu-id="7b6f2-125">Néhány példa a köteges feldolgozás esetekben megfelelőek toouse alacsony prioritású virtuális gépeken használja:</span><span class="sxs-lookup"><span data-stu-id="7b6f2-125">Some examples of batch processing use cases well suited toouse low-priority VMs are:</span></span>

-   <span data-ttu-id="7b6f2-126">**Fejlesztéshez és teszteléshez**: különösen akkor, ha a felügyeleti teendők központjaként megoldások fejlesztés alatt jelentős megtakarítást is kell megvalósítani.</span><span class="sxs-lookup"><span data-stu-id="7b6f2-126">**Development and testing**: In particular, if large-scale solutions are being developed significant savings can be realized.</span></span> <span data-ttu-id="7b6f2-127">Tesztelési minden típusú kihasználhassa, de nagy terhelés tesztelése és regressziós tesztelés kiváló használ.</span><span class="sxs-lookup"><span data-stu-id="7b6f2-127">All types of testing can benefit, but large-scale load testing and regression testing are great uses.</span></span>

-   <span data-ttu-id="7b6f2-128">**Igény szerinti kapacitás kiegészítése**: alacsony prioritású virtuális gépek segítségével kiegészíti az regular dedikált virtuális gépek – Ha elérhető, a feladatok méretezése, és ezért alacsonyabb költségek gyorsabb befejezéséhez; Ha nem érhető el, hello alapkonfiguráció a dedikált virtuális gépek érhetők el.</span><span class="sxs-lookup"><span data-stu-id="7b6f2-128">**Supplementing on-demand capacity**: Low-priority VMs can be used to supplement regular dedicated VMs - when available, jobs can scale and therefore complete quicker for lower cost; when not available, hello baseline of dedicated VMs are available.</span></span>

-   <span data-ttu-id="7b6f2-129">**Rugalmas feladat végrehajtási ideje**: Ha hello idő feladatok rugalmasságot toocomplete, majd lehetséges elhagyta a kapacitás is megengedett megegyezni azonban hello alacsony prioritású virtuális gépek feladatok hozzáadását fog gyakran futtatja a gyorsabb és az alacsonyabb költségek.</span><span class="sxs-lookup"><span data-stu-id="7b6f2-129">**Flexible job execution time**: If there is flexibility in hello time jobs have toocomplete, then potential drops in capacity can be tolerated; however, with hello addition of low-priority VMs jobs will frequently run faster and for a lower cost.</span></span>

<span data-ttu-id="7b6f2-130">Kötegelt készletek lehet konfigurált toouse, alacsony prioritású virtuális gépek által számos módja, attól függően, hello rugalmasságot feladat-végrehajtási idő:</span><span class="sxs-lookup"><span data-stu-id="7b6f2-130">Batch pools can be configured toouse low-priority VMs in a few ways, depending on hello flexibility in job execution time:</span></span>

-   <span data-ttu-id="7b6f2-131">Kis prioritású virtuális gépek kizárólag a készletben használható, és kötegelt egyszerűen állítsa helyre a preempted kapacitás, ha elérhető.</span><span class="sxs-lookup"><span data-stu-id="7b6f2-131">Low-priority VMs can solely be used in a pool and Batch will simply recover any preempted capacity when available.</span></span> <span data-ttu-id="7b6f2-132">Ez az hello legolcsóbb módon tooexecute feladatok csak alacsony prioritású virtuális gép használja.</span><span class="sxs-lookup"><span data-stu-id="7b6f2-132">This is hello cheapest way tooexecute jobs as only low-priority VMs are used.</span></span>

-   <span data-ttu-id="7b6f2-133">Kis prioritású virtuális gépek dedikált virtuális gépek rögzített alapterv együtt használható.</span><span class="sxs-lookup"><span data-stu-id="7b6f2-133">Low-priority VMs can be used in conjunction with a fixed baseline of dedicated VMs.</span></span> <span data-ttu-id="7b6f2-134">hello dedikált virtuális gépek rögzített száma biztosítja, van néhány kapacitás tookeep mindig egy feladat halad.</span><span class="sxs-lookup"><span data-stu-id="7b6f2-134">hello fixed number of dedicated VMs ensures there is always some capacity tookeep a job progressing.</span></span>

-   <span data-ttu-id="7b6f2-135">Lehet dedikált és alacsony prioritású virtuális gépeket, a dinamikus kombinációját, hogy az olcsóbb alacsony prioritású virtuális gépek kizárólag akkor használatosak, ha elérhető, de hello teljes árú dedikált virtuális gépek méretezhető, ha szükséges, a minimális kapacitás elérhető tookeep hello feladatok tookeep halad.</span><span class="sxs-lookup"><span data-stu-id="7b6f2-135">There can be dynamic mix of dedicated and low-priority VMs, so that the cheaper low-priority VMs are solely used when available, but hello full-priced dedicated VMs are scaled up when required, tookeep a minimum amount of capacity available tookeep hello jobs progressing.</span></span>

## <a name="batch-support-for-low-priority-vms"></a><span data-ttu-id="7b6f2-136">Kis prioritású virtuális gépek kötegelt támogatása</span><span class="sxs-lookup"><span data-stu-id="7b6f2-136">Batch support for low-priority VMs</span></span>

<span data-ttu-id="7b6f2-137">Az Azure Batch szolgáltatás különböző képességeket, amelyek révén könnyen tooconsume, és igénybe vehesse az alacsony prioritású virtuális gépek:</span><span class="sxs-lookup"><span data-stu-id="7b6f2-137">Azure Batch provides several capabilities that make it easy tooconsume and benefit from low-priority VMs:</span></span>

-   <span data-ttu-id="7b6f2-138">Kötegelt készletek dedikált virtuális gépek és az alacsonyabb prioritású virtuális gépek is tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="7b6f2-138">Batch pools can contain both dedicated VMs and low-priority VMs.</span></span> <span data-ttu-id="7b6f2-139">virtuális gép különböző típusú hello száma készlet létrehozásakor, vagy módosítani egy meglévő készlet hello explicit átméretezés vagy automatikus skálázása használatával bármikor adható meg.</span><span class="sxs-lookup"><span data-stu-id="7b6f2-139">hello number of each type of VM can be specified when a pool is created, or changed at any time for an existing pool, using hello explicit resize operation or using auto-scale.</span></span> <span data-ttu-id="7b6f2-140">Feladat- és küldésének változatlan marad, és nem kell érintett hello VM típusú hello készletben.</span><span class="sxs-lookup"><span data-stu-id="7b6f2-140">Job and task submission can remain unchanged and do not need to be concerned with hello VM types in hello pool.</span></span> <span data-ttu-id="7b6f2-141">Akkor is lehetséges toohave készlet teljesen használja, alacsony prioritású virtuális gépek toorun feladatok, de dedikált virtuális gépeinek léptetéses olcsón Ha hello kapacitás, futó feladatok tartani a minimális küszöbérték alá csökken.</span><span class="sxs-lookup"><span data-stu-id="7b6f2-141">It is also possible toohave a pool completely use low-priority VMs toorun jobs as cheaply as possible, but spin up dedicated VMs if hello capacity drops below a minimum threshold, to keep jobs running.</span></span>

-   <span data-ttu-id="7b6f2-142">Kötegelt készletek automatikusan keresni alacsony prioritású virtuális gépek száma toohello cél.</span><span class="sxs-lookup"><span data-stu-id="7b6f2-142">Batch pools automatically seek toohello target number of low-priority VMs.</span></span> <span data-ttu-id="7b6f2-143">Ha a leállított virtuális gépeken, kötegelt elvesztette a kapacitás és a visszatérési toohello cél tooreplace hello megpróbál.</span><span class="sxs-lookup"><span data-stu-id="7b6f2-143">If VMs are preempted, then Batch will attempt tooreplace hello lost capacity and return toohello target.</span></span>

-   <span data-ttu-id="7b6f2-144">Kötegelt feladatok alatt megszakadt hello esetben azonosítja, és automatikusan újra a feladatok toobe futtassa újra a várólistába helyezi.</span><span class="sxs-lookup"><span data-stu-id="7b6f2-144">In hello case of tasks being interrupted, Batch will detect and automatically requeue tasks toobe run again.</span></span>

-   <span data-ttu-id="7b6f2-145">Kis prioritású virtuális gépek eltér dedikált virtuális core kvóta rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="7b6f2-145">Low-priority VMs have a core quota that differs from that of dedicated VMs.</span></span> 
    <span data-ttu-id="7b6f2-146">hello idézőjel alacsony prioritású virtuális gép esetében nem nagyobb, mint a dedikált virtuális gépek, mert alacsony prioritású virtuális gépek költséget.</span><span class="sxs-lookup"><span data-stu-id="7b6f2-146">hello quote for low-priority VMs is higher than that of dedicated VMs, because low-priority VMs cost less.</span></span> <span data-ttu-id="7b6f2-147">Lásd: [a Batch szolgáltatás kvótái és korlátai](batch-quota-limit.md#resource-quotas) további információt.</span><span class="sxs-lookup"><span data-stu-id="7b6f2-147">See [Batch service quotas and limits](batch-quota-limit.md#resource-quotas) for more information.</span></span>    

> [!NOTE]
> <span data-ttu-id="7b6f2-148">Alacsony prioritású virtuális gép jelenleg nem támogatottak a Batch-fiókok, amelyben hello tárolókészlet foglalási mód van megadva túl[felhasználói előfizetési](batch-account-create-portal.md#user-subscription-mode).</span><span class="sxs-lookup"><span data-stu-id="7b6f2-148">Low-priority VMs are not currently supported for Batch accounts where hello pool allocation mode is set too[User subscription](batch-account-create-portal.md#user-subscription-mode).</span></span>
>
>

## <a name="create-and-update-pools"></a><span data-ttu-id="7b6f2-149">Hozzon létre, és a készletek frissítése</span><span class="sxs-lookup"><span data-stu-id="7b6f2-149">Create and update pools</span></span>

<span data-ttu-id="7b6f2-150">A Batch-készlet dedikált és alacsony prioritású virtuális gépek (is hivatkozott tooas számítási csomópontok) tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="7b6f2-150">A Batch pool can contain both dedicated and low-priority VMs (also referred tooas compute nodes).</span></span> <span data-ttu-id="7b6f2-151">Beállíthatja a számítási csomópontok száma hello cél dedikált és alacsony prioritású virtuális gép esetében.</span><span class="sxs-lookup"><span data-stu-id="7b6f2-151">You can set hello target number of compute nodes for both dedicated and low-priority VMs.</span></span> <span data-ttu-id="7b6f2-152">hello csomópontok száma cél számát határozza meg hello toohave hello készlet kívánt virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="7b6f2-152">hello target number of nodes specifies hello number of VMs you want toohave in hello pool.</span></span>

<span data-ttu-id="7b6f2-153">Például a virtuális gépek Azure-felhőszolgáltatásban használata 5 célja készlet toocreate dedikált virtuális gépek és 20 alacsony prioritású virtuális gép:</span><span class="sxs-lookup"><span data-stu-id="7b6f2-153">For example, toocreate a pool using Azure cloud service VMs with a target of 5 dedicated VMs and 20 low-priority VMs:</span></span>

```csharp
CloudPool pool = batchClient.PoolOperations.CreatePool(
    poolId: "cspool",
    targetDedicatedComputeNodes: 5,
    targetLowPriorityComputeNodes: 20,
    virtualMachineSize: "Standard_D2_v2",
    cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4") // WS 2012 R2
);
```

<span data-ttu-id="7b6f2-154">a készlet (ebben az esetben Linux virtuális gépek) az Azure virtuális gépek használata 5 célja toocreate dedikált virtuális gépek és 20 alacsony prioritású virtuális gép:</span><span class="sxs-lookup"><span data-stu-id="7b6f2-154">toocreate a pool using Azure virtual machines (in this case Linux VMs) with a target of 5 dedicated VMs and 20 low-priority VMs:</span></span>

```csharp
ImageReference imageRef = new ImageReference(
    publisher: "Canonical",
    offer: "UbuntuServer",
    sku: "16.04.0-LTS",
    version: "latest");

// Create hello pool
VirtualMachineConfiguration virtualMachineConfiguration =
    new VirtualMachineConfiguration("batch.node.ubuntu 16.04", imageRef);

pool = batchClient.PoolOperations.CreatePool(
    poolId: "vmpool",
    targetDedicatedComputeNodes: 5,
    targetLowPriorityComputeNodes: 20,
    virtualMachineSize: "Standard\_D2\_v2",
    virtualMachineConfiguration: virtualMachineConfiguration);
```

<span data-ttu-id="7b6f2-155">Dedikált és alacsony prioritású virtuális gépek kaphat hello aktuális csomópontok száma:</span><span class="sxs-lookup"><span data-stu-id="7b6f2-155">You can get hello current number of nodes for both dedicated and low-priority VMs:</span></span>

```csharp
int? numDedicated = pool1.CurrentDedicatedComputeNodes;
int? numLowPri = pool1.CurrentLowPriorityComputeNodes;
```

<span data-ttu-id="7b6f2-156">Készlet csomópontjain van-e egy tulajdonság tooindicate hello csomópont egy dedikált vagy alacsony prioritású virtuális gép esetén:</span><span class="sxs-lookup"><span data-stu-id="7b6f2-156">Pool nodes have a property tooindicate if hello node is a dedicated or low-priority VM:</span></span>

```csharp
bool? isNodeDedicated = poolNode.IsDedicated;
```

<span data-ttu-id="7b6f2-157">Ha a készlet egy vagy több csomópontja részesítés, a készlet egy lista csomópontok művelet továbbra is visszaállítja azokat a csomópontokat, alacsony prioritású csomópontok aktuális száma hello változatlan marad, de azokat a csomópontokat, kell állapotukra beállítása toothe **Prioritás miatt kihagyva**állapotát.</span><span class="sxs-lookup"><span data-stu-id="7b6f2-157">When one or more nodes in a pool are preempted, a list nodes operation on the pool will still return those nodes, hello current number of low-priority nodes will remain unchanged, but those nodes will have their state set toothe **Preempted** state.</span></span> <span data-ttu-id="7b6f2-158">Kötegelt megpróbál toofind helyettesítő virtuális gépeket, és ha sikeres, hello csomópontok végighaladnia **létrehozása** , majd **indítása** állapotok ahhoz, hogy elérhető-e a feladat végrehajtásához, ugyanúgy, mint az új csomópontok.</span><span class="sxs-lookup"><span data-stu-id="7b6f2-158">Batch will attempt toofind replacement VMs and, if successful, hello nodes will go through **Creating** and then **Starting** states before becoming available for task execution, just like new nodes.</span></span>

## <a name="scale-a-pool-containing-low-priority-vms"></a><span data-ttu-id="7b6f2-159">Kis prioritású virtuális gépeket tartalmazó készlet méretezése</span><span class="sxs-lookup"><span data-stu-id="7b6f2-159">Scale a pool containing low-priority VMs</span></span>

<span data-ttu-id="7b6f2-160">Csakúgy, mint a gyűjtők kizárólag álló dedikált virtuális gépek, is lehetséges tooscale a készletet tartalmazó alacsony prioritású virtuális gépek hello átméretezési metódus hívása vagy automatikus skálázása.</span><span class="sxs-lookup"><span data-stu-id="7b6f2-160">As with pools solely consisting of dedicated VMs, it is possible tooscale a pool containing low-priority VMs by calling hello Resize method or by using auto-scale.</span></span>

<span data-ttu-id="7b6f2-161">hello készlet átméretezés egy második választható paramétert fogad, amely frissíti a **targetLowPriorityNodes**:</span><span class="sxs-lookup"><span data-stu-id="7b6f2-161">hello pool resize operation takes a second optional parameter that updates the value of **targetLowPriorityNodes**:</span></span>

```csharp
pool.Resize(targetDedicatedComputeNodes: 0, targetLowPriorityComputeNodes: 25);
```

<span data-ttu-id="7b6f2-162">hello készlet automatikus méretezése képlet az alábbiak szerint támogatja az alacsony prioritású virtuális gépek:</span><span class="sxs-lookup"><span data-stu-id="7b6f2-162">hello pool auto-scale formula supports low-priority VMs as follows:</span></span>

-   <span data-ttu-id="7b6f2-163">Beolvasása, vagy állítsa be hello hello szolgáltatás által definiált változó **$TargetLowPriorityNodes**.</span><span class="sxs-lookup"><span data-stu-id="7b6f2-163">You can get or set hello value of hello service-defined variable **$TargetLowPriorityNodes**.</span></span>

-   <span data-ttu-id="7b6f2-164">Hello szolgáltatás által definiált változó értékének hello kaphat **$CurrentLowPriorityNodes**.</span><span class="sxs-lookup"><span data-stu-id="7b6f2-164">You can get hello value of hello service-defined variable **$CurrentLowPriorityNodes**.</span></span>

-   <span data-ttu-id="7b6f2-165">Hello szolgáltatás által definiált változó értékének hello kaphat **$PreemptedNodeCount**.</span><span class="sxs-lookup"><span data-stu-id="7b6f2-165">You can get hello value of hello service-defined variable **$PreemptedNodeCount**.</span></span> 
    <span data-ttu-id="7b6f2-166">Ez a változó hello hello csomópontok száma a leállított állapotot, és méretezheti felfelé vagy lefelé dedikált csomópontok, attól függően, hogy nem érhetők el a leállított csomópontok száma hello hello számát adja vissza.</span><span class="sxs-lookup"><span data-stu-id="7b6f2-166">This variable returns hello number of nodes in hello preempted state and allows you to scale up or down hello number of dedicated nodes, depending on hello number of preempted nodes that are unavailable.</span></span>

## <a name="jobs-and-tasks"></a><span data-ttu-id="7b6f2-167">Feladatok és feladatok</span><span class="sxs-lookup"><span data-stu-id="7b6f2-167">Jobs and tasks</span></span>

<span data-ttu-id="7b6f2-168">Feladatok és a feladatok csekély-támogatásra van szüksége az alacsony prioritású csomópont; hello csak támogatása a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="7b6f2-168">Jobs and tasks require very little support for low-priority nodes; hello only support is as follows:</span></span>

-   <span data-ttu-id="7b6f2-169">hello JobManagerTask tulajdonság egy feladat tulajdonsága egy új, **AllowLowPriorityNode**.</span><span class="sxs-lookup"><span data-stu-id="7b6f2-169">hello JobManagerTask property of a job has a new property, **AllowLowPriorityNode**.</span></span> 
    <span data-ttu-id="7b6f2-170">Ez a tulajdonság értéke igaz, amikor egy dedikált vagy alacsony prioritású csomópont hello manager feladat is ütemezhető.</span><span class="sxs-lookup"><span data-stu-id="7b6f2-170">When this property is true, hello job manager task can be scheduled on either a dedicated or low-priority node.</span></span> <span data-ttu-id="7b6f2-171">Ha ez a tulajdonság értéke HAMIS, hello kezelő feladat nem ütemezett tooa csak a kijelölt csomópont.</span><span class="sxs-lookup"><span data-stu-id="7b6f2-171">If this property is false, hello job manager task will be scheduled tooa dedicated node only.</span></span>

-   <span data-ttu-id="7b6f2-172">Egy [környezeti változó](batch-compute-node-environment-variables.md) elérhető tooa feladat alkalmazás van, hogy megállapíthassa, hogy egy alacsony prioritású vagy dedikált csomóponton futó.</span><span class="sxs-lookup"><span data-stu-id="7b6f2-172">An [environment variable](batch-compute-node-environment-variables.md) is available tooa task application so that it can determine whether it is running on a low-priority or dedicated node.</span></span> <span data-ttu-id="7b6f2-173">hello környezeti változó AZ_BATCH_NODE_IS_DEDICATED.</span><span class="sxs-lookup"><span data-stu-id="7b6f2-173">hello environment variable is AZ_BATCH_NODE_IS_DEDICATED.</span></span>

## <a name="handling-preemption"></a><span data-ttu-id="7b6f2-174">A megelőlegezés kezelése</span><span class="sxs-lookup"><span data-stu-id="7b6f2-174">Handling preemption</span></span>

<span data-ttu-id="7b6f2-175">Virtuális gépek alkalmanként foglalható; Ha ez történik, a kötegelt hello a következő:</span><span class="sxs-lookup"><span data-stu-id="7b6f2-175">VMs may occasionally be preempted; when this happens, Batch does hello following:</span></span>

-   <span data-ttu-id="7b6f2-176">hello a leállított virtuális gépek rendelkezik frissíti állapotukra**Prioritás miatt kihagyva**.</span><span class="sxs-lookup"><span data-stu-id="7b6f2-176">hello preempted VMs have their state updated too**Preempted**.</span></span>
-   <span data-ttu-id="7b6f2-177">Ha futó feladatok a hello leállított csomópont virtuális gépeket, majd ezeket a feladatokat helyezte, és futtassa újból.</span><span class="sxs-lookup"><span data-stu-id="7b6f2-177">If tasks were running on hello preempted node VMs, then those tasks are requeued and run again.</span></span>
-   <span data-ttu-id="7b6f2-178">virtuális gép hello hatékonyan törölték, helyileg tárolt virtuális gép elvesztését hello tooany adatok vezető.</span><span class="sxs-lookup"><span data-stu-id="7b6f2-178">hello VM is effectively deleted, leading tooany data stored locally on hello VM being lost.</span></span>
-   <span data-ttu-id="7b6f2-179">hello készlet folyamatosan megpróbál tooreach hello cél elérhető alacsony prioritású csomópontok száma.</span><span class="sxs-lookup"><span data-stu-id="7b6f2-179">hello pool continually attempts tooreach hello target number of low-priority nodes available.</span></span> <span data-ttu-id="7b6f2-180">Ha helyettesítő kapacitás tartalmaz, a csomópontok tartani az azonosítók, de újbóli inicializálása, keresztül haladó **létrehozása** és **indítása** előtt a feladat ütemezés szerint.</span><span class="sxs-lookup"><span data-stu-id="7b6f2-180">When replacement capacity is found, the nodes keep their ids, but are re-initialized, going through **Creating** and **Starting** states before they are available for task scheduling.</span></span>
-   <span data-ttu-id="7b6f2-181">A megelőlegezés számok állnak rendelkezésre, egy metrika a hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="7b6f2-181">Preemption counts are available as a metric in hello Azure portal.</span></span>

## <a name="metrics"></a><span data-ttu-id="7b6f2-182">Mérőszámok</span><span class="sxs-lookup"><span data-stu-id="7b6f2-182">Metrics</span></span>

<span data-ttu-id="7b6f2-183">Új mérőszámok érhetők el hello [Azure-portálon](https://portal.azure.com) az alacsony prioritású csomópont.</span><span class="sxs-lookup"><span data-stu-id="7b6f2-183">New metrics are available in hello [Azure portal](https://portal.azure.com) for low-priority nodes.</span></span> <span data-ttu-id="7b6f2-184">Ezen adatok gyűjtése le van:</span><span class="sxs-lookup"><span data-stu-id="7b6f2-184">These metrics are:</span></span>

- <span data-ttu-id="7b6f2-185">Alacsony prioritású csomópontok száma</span><span class="sxs-lookup"><span data-stu-id="7b6f2-185">Low-Priority Node Count</span></span>
- <span data-ttu-id="7b6f2-186">Alacsony prioritású magok száma</span><span class="sxs-lookup"><span data-stu-id="7b6f2-186">Low-Priority Core Count</span></span> 
- <span data-ttu-id="7b6f2-187">Csomópont Count részesítés</span><span class="sxs-lookup"><span data-stu-id="7b6f2-187">Preempted Node Count</span></span>

<span data-ttu-id="7b6f2-188">tooview metrikát a hello Azure-portálon:</span><span class="sxs-lookup"><span data-stu-id="7b6f2-188">tooview metrics in hello Azure portal:</span></span>

1. <span data-ttu-id="7b6f2-189">Keresse meg a Batch-fiók hello portálon tooyour, és a Batch-fiók hello beállítások megtekintése.</span><span class="sxs-lookup"><span data-stu-id="7b6f2-189">Navigate tooyour Batch account in hello portal, and view hello settings for your Batch account.</span></span>
2. <span data-ttu-id="7b6f2-190">Válassza ki **metrikák** a hello **figyelés** szakasz.</span><span class="sxs-lookup"><span data-stu-id="7b6f2-190">Select **Metrics** from hello **Monitoring** section.</span></span>
3. <span data-ttu-id="7b6f2-191">Válassza ki a felügyelni hello metrikákat a hello **elérhető** listája.</span><span class="sxs-lookup"><span data-stu-id="7b6f2-191">Select hello metrics you desire from hello **Available Metrics** list.</span></span>

![Alacsony prioritású csomópontok metrikák](media/batch-low-pri-vms/low-pri-metrics.png)

## <a name="next-steps"></a><span data-ttu-id="7b6f2-193">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7b6f2-193">Next steps</span></span>

* <span data-ttu-id="7b6f2-194">Olvasási hello [Batch funkcióinak áttekintése a fejlesztők](batch-api-basics.md), bárki toouse kötegelt előkészítése alapvető információkat.</span><span class="sxs-lookup"><span data-stu-id="7b6f2-194">Read hello [Batch feature overview for developers](batch-api-basics.md), essential information for anyone preparing toouse Batch.</span></span> <span data-ttu-id="7b6f2-195">hello cikkben Batch szolgáltatás erőforrásokhoz, mint a készletek, a csomópontok, a feladatok, és a feladatok és a hello részletesebb információkat is használhatja a kötegelt kérelem felépítésekor sok API-funkciókat.</span><span class="sxs-lookup"><span data-stu-id="7b6f2-195">hello article contains more detailed information about Batch service resources like pools, nodes, jobs, and tasks, and hello many API features that you can use while building your Batch application.</span></span>
* <span data-ttu-id="7b6f2-196">További tudnivalók: hello [kötegelt API-k és eszközök](batch-apis-tools.md) kötegelt megoldások érhetők el.</span><span class="sxs-lookup"><span data-stu-id="7b6f2-196">Learn about hello [Batch APIs and tools](batch-apis-tools.md) available for building Batch solutions.</span></span>
