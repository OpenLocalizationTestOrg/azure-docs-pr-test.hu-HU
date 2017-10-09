---
title: az Azure Service Fabric aaaReliableConcurrentQueue
description: "ReliableConcurrentQueue a nagy átviteli beolvasása, amely lehetővé teszi, hogy a párhuzamos enqueues és dequeues."
services: service-fabric
documentationcenter: .net
author: sangarg
manager: timlt
editor: raja,tyadam,masnider,vturecek
ms.assetid: 62857523-604b-434e-bd1c-2141ea4b00d1
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 5/1/2017
ms.author: sangarg
ms.openlocfilehash: 78a9905996b9ab265c1288d2b49753638d7bc445
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooreliableconcurrentqueue-in-azure-service-fabric"></a><span data-ttu-id="931ce-103">Az Azure Service Fabric bemutatása tooReliableConcurrentQueue</span><span class="sxs-lookup"><span data-stu-id="931ce-103">Introduction tooReliableConcurrentQueue in Azure Service Fabric</span></span>
<span data-ttu-id="931ce-104">Megbízható egyidejű várólista egy aszinkron, a tranzakciós és a replikált sor mely szolgáltatások magas CONCURRENCY paraméterének értékét sorba helyezni, és műveletek feldolgozásához.</span><span class="sxs-lookup"><span data-stu-id="931ce-104">Reliable Concurrent Queue is an asynchronous, transactional, and replicated queue which features high concurrency for enqueue and dequeue operations.</span></span> <span data-ttu-id="931ce-105">Tervezett toodeliver magas teljesítmény és alacsony késési igényű által lazítani a hello szigorú FIFO rendelési által biztosított [megbízható várólista](https://msdn.microsoft.com/library/azure/dn971527.aspx) és biztosítja a legjobb rendezést.</span><span class="sxs-lookup"><span data-stu-id="931ce-105">It is designed toodeliver high throughput and low latency by relaxing hello strict FIFO ordering provided by [Reliable Queue](https://msdn.microsoft.com/library/azure/dn971527.aspx) and instead provides a best-effort ordering.</span></span>

## <a name="apis"></a><span data-ttu-id="931ce-106">API-k</span><span class="sxs-lookup"><span data-stu-id="931ce-106">APIs</span></span>

|<span data-ttu-id="931ce-107">Egyidejű várólista</span><span class="sxs-lookup"><span data-stu-id="931ce-107">Concurrent Queue</span></span>                |<span data-ttu-id="931ce-108">Megbízható egyidejű várólista</span><span class="sxs-lookup"><span data-stu-id="931ce-108">Reliable Concurrent Queue</span></span>                                         |
|--------------------------------|------------------------------------------------------------------|
| <span data-ttu-id="931ce-109">"void" Enqueue(T item)</span><span class="sxs-lookup"><span data-stu-id="931ce-109">void Enqueue(T item)</span></span>           | <span data-ttu-id="931ce-110">A feladat EnqueueAsync (ITransaction tx, T elem)</span><span class="sxs-lookup"><span data-stu-id="931ce-110">Task EnqueueAsync(ITransaction tx, T item)</span></span>                       |
| <span data-ttu-id="931ce-111">logikai TryDequeue (kimenő T eredmény)</span><span class="sxs-lookup"><span data-stu-id="931ce-111">bool TryDequeue(out T result)</span></span>  | <span data-ttu-id="931ce-112">A feladat < ConditionalValue < T >> TryDequeueAsync (ITransaction tx)</span><span class="sxs-lookup"><span data-stu-id="931ce-112">Task< ConditionalValue < T > > TryDequeueAsync(ITransaction tx)</span></span>  |
| <span data-ttu-id="931ce-113">int Count()</span><span class="sxs-lookup"><span data-stu-id="931ce-113">int Count()</span></span>                    | <span data-ttu-id="931ce-114">hosszú Count()</span><span class="sxs-lookup"><span data-stu-id="931ce-114">long Count()</span></span>                                                     |

## <a name="comparison-with-reliable-queuehttpsmsdnmicrosoftcomlibraryazuredn971527aspx"></a><span data-ttu-id="931ce-115">Az összehasonlítás [megbízható várólista](https://msdn.microsoft.com/library/azure/dn971527.aspx)</span><span class="sxs-lookup"><span data-stu-id="931ce-115">Comparison with [Reliable Queue](https://msdn.microsoft.com/library/azure/dn971527.aspx)</span></span>

<span data-ttu-id="931ce-116">Megbízható egyidejű várólista érhető el alternatív túl[megbízható várólista](https://msdn.microsoft.com/library/azure/dn971527.aspx).</span><span class="sxs-lookup"><span data-stu-id="931ce-116">Reliable Concurrent Queue is offered as an alternative too[Reliable Queue](https://msdn.microsoft.com/library/azure/dn971527.aspx).</span></span> <span data-ttu-id="931ce-117">Olyan esetekben, ahol szigorú FIFO rendezés nem szükséges, használjon, FIFO együtt van szükség egy kompromisszumot biztosítása.</span><span class="sxs-lookup"><span data-stu-id="931ce-117">It should be used in cases where strict FIFO ordering is not required, as guaranteeing FIFO requires a tradeoff with concurrency.</span></span>  <span data-ttu-id="931ce-118">[Megbízható várólista](https://msdn.microsoft.com/library/azure/dn971527.aspx) zárolások tooenforce FIFO rendezést, és legfeljebb egy tranzakciót tooenqueue engedélyezett toodequeue engedélyezett egyszerre legfeljebb egy tranzakciót használ.</span><span class="sxs-lookup"><span data-stu-id="931ce-118">[Reliable Queue](https://msdn.microsoft.com/library/azure/dn971527.aspx) uses locks tooenforce FIFO ordering, with at most one transaction allowed tooenqueue and at most one transaction allowed toodequeue at a time.</span></span> <span data-ttu-id="931ce-119">Szemben megbízható egyidejű várólista megkötés rendelési hello visszaállítja, és lehetővé teszi, hogy a bármely száma párhuzamos tranzakciók toointerleave a sorba helyezni, és műveletek feldolgozásához.</span><span class="sxs-lookup"><span data-stu-id="931ce-119">In comparison, Reliable Concurrent Queue relaxes hello ordering constraint and allows any number concurrent transactions toointerleave their enqueue and dequeue operations.</span></span> <span data-ttu-id="931ce-120">Legjobb rendelési kerül, de hello relatív sorrendjének megbízható egyidejű várólista két értékek soha nem garantálható.</span><span class="sxs-lookup"><span data-stu-id="931ce-120">Best-effort ordering is provided, however hello relative ordering of two values in a Reliable Concurrent Queue can never be guaranteed.</span></span>

<span data-ttu-id="931ce-121">Megbízható egyidejű várólista biztosít nagyobb átviteli teljesítményt és kisebb késést biztosít [megbízható várólista](https://msdn.microsoft.com/library/azure/dn971527.aspx) vannak enqueues végrehajtása több egyidejű tranzakciókat és/vagy dequeues.</span><span class="sxs-lookup"><span data-stu-id="931ce-121">Reliable Concurrent Queue provides higher throughput and lower latency than [Reliable Queue](https://msdn.microsoft.com/library/azure/dn971527.aspx) whenever there are multiple concurrent transactions performing enqueues and/or dequeues.</span></span>

<span data-ttu-id="931ce-122">Egy minta-és nagybetűhasználattal hello ReliableConcurrentQueue: hello [üzenet-várólista](https://en.wikipedia.org/wiki/Message_queue) forgatókönyv.</span><span class="sxs-lookup"><span data-stu-id="931ce-122">A sample use case for hello ReliableConcurrentQueue is hello [Message Queue](https://en.wikipedia.org/wiki/Message_queue) scenario.</span></span> <span data-ttu-id="931ce-123">Ebben a forgatókönyvben egy vagy több üzenetek létrehozói létre elemek toohello várólista hozzáadása és egy vagy több üzenetet fogyasztók hello várólistában lévő üzenetek le és dolgozza fel őket.</span><span class="sxs-lookup"><span data-stu-id="931ce-123">In this scenario, one or more message producers create and add items toohello queue, and one or more message consumers pull messages from hello queue and process them.</span></span> <span data-ttu-id="931ce-124">Több létrehozói és felhasználói is működjön egymástól függetlenül, egyidejű tranzakciókat rendelés tooprocess hello várólistában.</span><span class="sxs-lookup"><span data-stu-id="931ce-124">Multiple producers and consumers can work independently, using concurrent transactions in order tooprocess hello queue.</span></span>

## <a name="usage-guidelines"></a><span data-ttu-id="931ce-125">Használatára vonatkozó irányelvek</span><span class="sxs-lookup"><span data-stu-id="931ce-125">Usage Guidelines</span></span>
* <span data-ttu-id="931ce-126">hello várólista vár, hogy hello hello várólista útjuk alacsony megőrzési időtartamot.</span><span class="sxs-lookup"><span data-stu-id="931ce-126">hello queue expects that hello items in hello queue have a low retention period.</span></span> <span data-ttu-id="931ce-127">Ez azt jelenti, hogy hello elemek akkor nem marad hello várólista hosszú ideig.</span><span class="sxs-lookup"><span data-stu-id="931ce-127">That is, hello items would not stay in hello queue for a long time.</span></span>
* <span data-ttu-id="931ce-128">hello várólista nem garantálja a szigorú FIFO rendezést.</span><span class="sxs-lookup"><span data-stu-id="931ce-128">hello queue does not guarantee strict FIFO ordering.</span></span>
* <span data-ttu-id="931ce-129">hello várólista nem olvassa a saját írási műveleteket.</span><span class="sxs-lookup"><span data-stu-id="931ce-129">hello queue does not read its own writes.</span></span> <span data-ttu-id="931ce-130">Ha a cikk a várólistában levő tranzakción belül, akkor nem fog látható tooa dequeuer belül hello ugyanabban a tranzakcióban.</span><span class="sxs-lookup"><span data-stu-id="931ce-130">If an item is enqueued within a transaction, it will not be visible tooa dequeuer within hello same transaction.</span></span>
* <span data-ttu-id="931ce-131">Dequeues nem el különítve egymástól.</span><span class="sxs-lookup"><span data-stu-id="931ce-131">Dequeues are not isolated from each other.</span></span> <span data-ttu-id="931ce-132">Ha a cikk *A* várólistából tranzakcióban van-e kivéve *txnA*, annak ellenére, hogy *txnA* nincs véglegesítve, elem *A* nem lenne látható tooa egyidejű tranzakció *txnB*.</span><span class="sxs-lookup"><span data-stu-id="931ce-132">If item *A* is dequeued in transaction *txnA*, even though *txnA* is not committed, item *A* would not be visible tooa concurrent transaction *txnB*.</span></span>  <span data-ttu-id="931ce-133">Ha *txnA* megszakítja, *A* láthatóvá válnak túl*txnB* azonnal.</span><span class="sxs-lookup"><span data-stu-id="931ce-133">If *txnA* aborts, *A* will become visible too*txnB* immediately.</span></span>
* <span data-ttu-id="931ce-134">*TryPeekAsync* viselkedés használatával valósítható egy *TryDequeueAsync* és majd a hello tranzakció megszakítása.</span><span class="sxs-lookup"><span data-stu-id="931ce-134">*TryPeekAsync* behavior can be implemented by using a *TryDequeueAsync* and then aborting hello transaction.</span></span> <span data-ttu-id="931ce-135">Például az hello programozási minták szakaszban találhatók.</span><span class="sxs-lookup"><span data-stu-id="931ce-135">An example of this can be found in hello Programming Patterns section.</span></span>
* <span data-ttu-id="931ce-136">Számláló értéke nem tranzakciós.</span><span class="sxs-lookup"><span data-stu-id="931ce-136">Count is non-transactional.</span></span> <span data-ttu-id="931ce-137">Azt lehet használt tooget hello hello várólistában lévő elemek száma a képet, de egy-időpontban jelöli, és nem lehet hivatkozni.</span><span class="sxs-lookup"><span data-stu-id="931ce-137">It can be used tooget an idea of hello number of elements in hello queue, but represents a point-in-time and cannot be relied upon.</span></span>
* <span data-ttu-id="931ce-138">Költséges hello feldolgozási várólistából kivéve elemeket kell nem hajtható végre, amíg hello tranzakció már aktív, ami hatással lehet a teljesítmény hello rendszer tooavoid hosszan futó tranzakciók.</span><span class="sxs-lookup"><span data-stu-id="931ce-138">Expensive processing on hello dequeued items should not be performed while hello transaction is active, tooavoid long-running transactions which may have a performance impact on hello system.</span></span>

## <a name="code-snippets"></a><span data-ttu-id="931ce-139">Kódtöredékek</span><span class="sxs-lookup"><span data-stu-id="931ce-139">Code Snippets</span></span>
<span data-ttu-id="931ce-140">Ossza meg velünk tekintse meg néhány kódtöredékek és a várható kimenetekkel.</span><span class="sxs-lookup"><span data-stu-id="931ce-140">Let us look at a few code snippets and their expected outputs.</span></span> <span data-ttu-id="931ce-141">Kivétel ebben a szakaszban figyelmen kívül hagyja.</span><span class="sxs-lookup"><span data-stu-id="931ce-141">Exception handling is ignored in this section.</span></span>

### <a name="enqueueasync"></a><span data-ttu-id="931ce-142">EnqueueAsync</span><span class="sxs-lookup"><span data-stu-id="931ce-142">EnqueueAsync</span></span>
<span data-ttu-id="931ce-143">Az alábbiakban néhány kódrészleteket a várt kimeneti követ EnqueueAsync használatával.</span><span class="sxs-lookup"><span data-stu-id="931ce-143">Here are a few code snippets for using EnqueueAsync followed by their expected outputs.</span></span>

- <span data-ttu-id="931ce-144">*1. eset: Egy sorba helyezni a feladat*</span><span class="sxs-lookup"><span data-stu-id="931ce-144">*Case 1: Single Enqueue Task*</span></span>

```
using (var txn = this.StateManager.CreateTransaction())
{
    await this.Queue.EnqueueAsync(txn, 10, cancellationToken);
    await this.Queue.EnqueueAsync(txn, 20, cancellationToken);

    await txn.CommitAsync();
}
```

<span data-ttu-id="931ce-145">Tegyük fel, hogy hello feladat sikeresen befejeződött, és, amelyek nincsenek hello várólista módosítása párhuzamos tranzakciók.</span><span class="sxs-lookup"><span data-stu-id="931ce-145">Assume that hello task completed successfully, and that there were no concurrent transactions modifying hello queue.</span></span> <span data-ttu-id="931ce-146">hello felhasználó számíthat hello várólista toocontain hello elemek hello megrendelések a következő valamelyikén:</span><span class="sxs-lookup"><span data-stu-id="931ce-146">hello user can expect hello queue toocontain hello items in any of hello following orders:</span></span>

> <span data-ttu-id="931ce-147">10, 20</span><span class="sxs-lookup"><span data-stu-id="931ce-147">10, 20</span></span>

> <span data-ttu-id="931ce-148">20, 10</span><span class="sxs-lookup"><span data-stu-id="931ce-148">20, 10</span></span>


- <span data-ttu-id="931ce-149">*2. eset: Párhuzamos sorba helyezni a feladat*</span><span class="sxs-lookup"><span data-stu-id="931ce-149">*Case 2: Parallel Enqueue Task*</span></span>

```
// Parallel Task 1
using (var txn = this.StateManager.CreateTransaction())
{
    await this.Queue.EnqueueAsync(txn, 10, cancellationToken);
    await this.Queue.EnqueueAsync(txn, 20, cancellationToken);

    await txn.CommitAsync();
}

// Parallel Task 2
using (var txn = this.StateManager.CreateTransaction())
{
    await this.Queue.EnqueueAsync(txn, 30, cancellationToken);
    await this.Queue.EnqueueAsync(txn, 40, cancellationToken);

    await txn.CommitAsync();
}
```

<span data-ttu-id="931ce-150">Tegyük fel, hogy hello feladatok, sikeresen befejeződött, hogy hello feladatok párhuzamosan futtatta-e, és, hogy történtek-e más egyidejű hello várólista módosítását tranzakciók.</span><span class="sxs-lookup"><span data-stu-id="931ce-150">Assume that hello tasks completed successfully, that hello tasks ran in parallel, and that there were no other concurrent transactions modifying hello queue.</span></span> <span data-ttu-id="931ce-151">Nincs megállapítás végezhető hello hello várólistában lévő elemek sorrendjét.</span><span class="sxs-lookup"><span data-stu-id="931ce-151">No inference can be made about hello order of items in hello queue.</span></span> <span data-ttu-id="931ce-152">A következő kódrészletet hello is megjelenhetnek hello 4 bármelyikét!</span><span class="sxs-lookup"><span data-stu-id="931ce-152">For this code snippet, hello items may appear in any of hello 4!</span></span> <span data-ttu-id="931ce-153">lehetséges rendezés.</span><span class="sxs-lookup"><span data-stu-id="931ce-153">possible orderings.</span></span>  <span data-ttu-id="931ce-154">hello várólista tookeep hello elemek hello eredeti (várólistán lévő) sorrendben megpróbál, de lehet, hogy kényszerített tooreorder megfelelő tooconcurrent műveleteket vagy hibákat őket.</span><span class="sxs-lookup"><span data-stu-id="931ce-154">hello queue will attempt tookeep hello items in hello original (enqueued) order, but may be forced tooreorder them due tooconcurrent operations or faults.</span></span>


### <a name="dequeueasync"></a><span data-ttu-id="931ce-155">DequeueAsync</span><span class="sxs-lookup"><span data-stu-id="931ce-155">DequeueAsync</span></span>
<span data-ttu-id="931ce-156">Az alábbiakban néhány kódrészleteket várt hello kimenetek követ TryDequeueAsync használatával.</span><span class="sxs-lookup"><span data-stu-id="931ce-156">Here are a few code snippets for using TryDequeueAsync followed by hello expected outputs.</span></span> <span data-ttu-id="931ce-157">Tegyük fel, hello várólistára már fel van töltve a hello hello várólistában lévő elemek a következő:</span><span class="sxs-lookup"><span data-stu-id="931ce-157">Assume that hello queue is already populated with hello following items in hello queue:</span></span>
> <span data-ttu-id="931ce-158">10, 20, 30, 40, 50, 60</span><span class="sxs-lookup"><span data-stu-id="931ce-158">10, 20, 30, 40, 50, 60</span></span>

- <span data-ttu-id="931ce-159">*1. eset: Egyetlen created feladat*</span><span class="sxs-lookup"><span data-stu-id="931ce-159">*Case 1: Single Dequeue Task*</span></span>

```
using (var txn = this.StateManager.CreateTransaction())
{
    await this.Queue.TryDequeueAsync(txn, cancellationToken);
    await this.Queue.TryDequeueAsync(txn, cancellationToken);
    await this.Queue.TryDequeueAsync(txn, cancellationToken);

    await txn.CommitAsync();
}
```

<span data-ttu-id="931ce-160">Tegyük fel, hogy hello feladat sikeresen befejeződött, és, amelyek nincsenek hello várólista módosítása párhuzamos tranzakciók.</span><span class="sxs-lookup"><span data-stu-id="931ce-160">Assume that hello task completed successfully, and that there were no concurrent transactions modifying hello queue.</span></span> <span data-ttu-id="931ce-161">Óta nincs megállapítás hello várólista hello elemek sorrendjének hello végezhető, bármely három hello elemek előfordulhat, hogy lehet várólistából kivéve, bármilyen sorrendben.</span><span class="sxs-lookup"><span data-stu-id="931ce-161">Since no inference can be made about hello order of hello items in hello queue, any three of hello items may be dequeued, in any order.</span></span> <span data-ttu-id="931ce-162">hello várólista tookeep hello elemek hello eredeti (várólistán lévő) sorrendben megpróbál, de lehet, hogy kényszerített tooreorder megfelelő tooconcurrent műveleteket vagy hibákat őket.</span><span class="sxs-lookup"><span data-stu-id="931ce-162">hello queue will attempt tookeep hello items in hello original (enqueued) order, but may be forced tooreorder them due tooconcurrent operations or faults.</span></span>  

- <span data-ttu-id="931ce-163">*2. eset: Párhuzamosan created feladat*</span><span class="sxs-lookup"><span data-stu-id="931ce-163">*Case 2: Parallel Dequeue Task*</span></span>

```
// Parallel Task 1
List<int> dequeue1;
using (var txn = this.StateManager.CreateTransaction())
{
    dequeue1.Add(await this.Queue.TryDequeueAsync(txn, cancellationToken)).val;
    dequeue1.Add(await this.Queue.TryDequeueAsync(txn, cancellationToken)).val;

    await txn.CommitAsync();
}

// Parallel Task 2
List<int> dequeue2;
using (var txn = this.StateManager.CreateTransaction())
{
    dequeue2.Add(await this.Queue.TryDequeueAsync(txn, cancellationToken)).val;
    dequeue2.Add(await this.Queue.TryDequeueAsync(txn, cancellationToken)).val;

    await txn.CommitAsync();
}
```

<span data-ttu-id="931ce-164">Tegyük fel, hogy hello feladatok, sikeresen befejeződött, hogy hello feladatok párhuzamosan futtatta-e, és, hogy történtek-e más egyidejű hello várólista módosítását tranzakciók.</span><span class="sxs-lookup"><span data-stu-id="931ce-164">Assume that hello tasks completed successfully, that hello tasks ran in parallel, and that there were no other concurrent transactions modifying hello queue.</span></span> <span data-ttu-id="931ce-165">Mivel nincs megállapítás hello várólista hello elemek sorrendjének hello végezhető, hello listák *dequeue1* és *dequeue2* egyes bármely két elemet tartalmaz, bármilyen sorrendben.</span><span class="sxs-lookup"><span data-stu-id="931ce-165">Since no inference can be made about hello order of hello items in hello queue, hello lists *dequeue1* and *dequeue2* will each contain any two items, in any order.</span></span>

<span data-ttu-id="931ce-166">azonos elem fog hello *nem* mindkét listán.</span><span class="sxs-lookup"><span data-stu-id="931ce-166">hello same item will *not* appear in both lists.</span></span> <span data-ttu-id="931ce-167">Ezért ha dequeue1 *10*, *30*, majd dequeue2 kellene *20*, *40*.</span><span class="sxs-lookup"><span data-stu-id="931ce-167">Hence, if dequeue1 has *10*, *30*, then dequeue2 would have *20*, *40*.</span></span>

- <span data-ttu-id="931ce-168">*3. eset: Created rendelését tranzakció megszakítása*</span><span class="sxs-lookup"><span data-stu-id="931ce-168">*Case 3: Dequeue Ordering With Transaction Abort*</span></span>

<span data-ttu-id="931ce-169">Az üzenetsoroktól a tranzakció megszakítása dequeues visszahelyezi hello munkaelemeket visszaküldeni a hello head hello várólista.</span><span class="sxs-lookup"><span data-stu-id="931ce-169">Aborting a transaction with in-flight dequeues puts hello items back on hello head of hello queue.</span></span> <span data-ttu-id="931ce-170">ahol hello elemek kerülnek vissza hello head hello várólista hello sorrendje nem garantált.</span><span class="sxs-lookup"><span data-stu-id="931ce-170">hello order in which hello items are put back on hello head of hello queue is not guaranteed.</span></span> <span data-ttu-id="931ce-171">Ossza meg velünk nézze meg a következő kód hello:</span><span class="sxs-lookup"><span data-stu-id="931ce-171">Let us look at hello following code:</span></span>

```
using (var txn = this.StateManager.CreateTransaction())
{
    await this.Queue.TryDequeueAsync(txn, cancellationToken);
    await this.Queue.TryDequeueAsync(txn, cancellationToken);

    // Abort hello transaction
    await txn.AbortAsync();
}
```
<span data-ttu-id="931ce-172">Tegyük fel, hogy a hello elemek várólistából volt kivéve sorrend hello:</span><span class="sxs-lookup"><span data-stu-id="931ce-172">Assume that hello items were dequeued in hello following order:</span></span>
> <span data-ttu-id="931ce-173">10, 20</span><span class="sxs-lookup"><span data-stu-id="931ce-173">10, 20</span></span>

<span data-ttu-id="931ce-174">Azt a hello tranzakció megszakítása, ha hello elemek hello megrendelések a következő valamelyikén hátsó toohello head hello várólista volna kerülnek:</span><span class="sxs-lookup"><span data-stu-id="931ce-174">When we abort hello transaction, hello items would be added back toohello head of hello queue in any of hello following orders:</span></span>
> <span data-ttu-id="931ce-175">10, 20</span><span class="sxs-lookup"><span data-stu-id="931ce-175">10, 20</span></span>

> <span data-ttu-id="931ce-176">20, 10</span><span class="sxs-lookup"><span data-stu-id="931ce-176">20, 10</span></span>

<span data-ttu-id="931ce-177">hello esetében is igaz minden olyan esetben, ahol hello tranzakció nem volt sikeresen *lekötött*.</span><span class="sxs-lookup"><span data-stu-id="931ce-177">hello same is true for all cases where hello transaction was not successfully *Committed*.</span></span>

## <a name="programming-patterns"></a><span data-ttu-id="931ce-178">Programozási minták</span><span class="sxs-lookup"><span data-stu-id="931ce-178">Programming Patterns</span></span>
<span data-ttu-id="931ce-179">Ebben a szakaszban ossza meg velünk tekintse meg néhány programozási minta, érdemes lehet megfontolni a ReliableConcurrentQueue használatával.</span><span class="sxs-lookup"><span data-stu-id="931ce-179">In this section, let us look at a few programming patterns that might be helpful in using ReliableConcurrentQueue.</span></span>

### <a name="batch-dequeues"></a><span data-ttu-id="931ce-180">Kötegelt Dequeues</span><span class="sxs-lookup"><span data-stu-id="931ce-180">Batch Dequeues</span></span>
<span data-ttu-id="931ce-181">A javasolt programozási minta pedig a hello fogyasztói feladat toobatch annak egyik végzett helyett dequeues created egyszerre.</span><span class="sxs-lookup"><span data-stu-id="931ce-181">A recommended programming pattern is for hello consumer task toobatch its dequeues instead of performing one dequeue at a time.</span></span> <span data-ttu-id="931ce-182">hello felhasználó választhat toothrottle késések minden köteg vagy hello kötegméret között.</span><span class="sxs-lookup"><span data-stu-id="931ce-182">hello user can choose toothrottle delays between every batch or hello batch size.</span></span> <span data-ttu-id="931ce-183">hello következő kódrészletet a programozási-modelljét mutatja.</span><span class="sxs-lookup"><span data-stu-id="931ce-183">hello following code snippet shows this programming model.</span></span>  <span data-ttu-id="931ce-184">Vegye figyelembe, hogy ebben a példában hello nem hajtja végre után hello tranzakció véglegesítése, így ha egy tartalék toooccur feldolgozása közben, hello feldolgozatlan elemek elvesznek feldolgozás nélkül.</span><span class="sxs-lookup"><span data-stu-id="931ce-184">Note that in this example, hello processing is done after hello transaction is committed, so if a fault were toooccur while processing, hello unprocessed items will be lost without having been processed.</span></span>  <span data-ttu-id="931ce-185">Másik lehetőségként hello feldolgozási végezhető hello tranzakció hatókörében, azonban ez negatív hatással lehet a teljesítményre, és szükséges hozzá hello cikkek kezelése már feldolgozott.</span><span class="sxs-lookup"><span data-stu-id="931ce-185">Alternatively, hello processing can be done within hello transaction's scope, however this may have a negative impact on performance and requires handling of hello items already processed.</span></span>

```
int batchSize = 5;
long delayMs = 100;

while(!cancellationToken.IsCancellationRequested)
{
    // Buffer for dequeued items
    List<int> processItems = new List<int>();

    using (var txn = this.StateManager.CreateTransaction())
    {
        ConditionalValue<int> ret;

        for(int i = 0; i < batchSize; ++i)
        {
            ret = await this.Queue.TryDequeueAsync(txn, cancellationToken);

            if (ret.HasValue)
            {
                // If an item was dequeued, add toohello buffer for processing
                processItems.Add(ret.Value);
            }
            else
            {
                // else break hello for loop
                break;
            }
        }

        await txn.CommitAsync();
    }

    // Process hello dequeues
    for (int i = 0; i < processItems.Count; ++i)
    {
        Console.WriteLine("Value : " + processItems[i]);
    }

    int delayFactor = batchSize - processItems.Count;
    await Task.Delay(TimeSpan.FromMilliseconds(delayMs * delayFactor), cancellationToken);
}
```

### <a name="best-effort-notification-based-processing"></a><span data-ttu-id="931ce-186">Legjobb értesítés-alapú feldolgozási</span><span class="sxs-lookup"><span data-stu-id="931ce-186">Best-Effort Notification-Based Processing</span></span>
<span data-ttu-id="931ce-187">Egy másik érdekes programozási mintát hello száma API-t használja.</span><span class="sxs-lookup"><span data-stu-id="931ce-187">Another interesting programming pattern uses hello Count API.</span></span> <span data-ttu-id="931ce-188">Itt ket lehet megvalósítani legjobb értesítés-alapú feldolgozási hello várólista.</span><span class="sxs-lookup"><span data-stu-id="931ce-188">Here, we can implement best-effort notification-based processing for hello queue.</span></span> <span data-ttu-id="931ce-189">hello várólista száma sorba állítani egy használt toothrottle vagy egy dequeue feladat lehet.</span><span class="sxs-lookup"><span data-stu-id="931ce-189">hello queue Count can be used toothrottle an enqueue or a dequeue task.</span></span>  <span data-ttu-id="931ce-190">Előfordulhat, hogy hello előző példához hasonlóan óta hello feldolgozása befejeződik hello tranzakción kívüli feldolgozatlan elemek elvesznek, ha a hiba akkor fordul elő, feldolgozás során.</span><span class="sxs-lookup"><span data-stu-id="931ce-190">Note that as in hello previous example, since hello processing occurs outside hello transaction, unprocessed items may be lost if a fault occurs during processing.</span></span>

```
int threshold = 5;
long delayMs = 1000;

while(!cancellationToken.IsCancellationRequested)
{
    while (this.Queue.Count < threshold)
    {
        cancellationToken.ThrowIfCancellationRequested();

        // If hello queue does not have hello threshold number of items, delay hello task and check again
        await Task.Delay(TimeSpan.FromMilliseconds(delayMs), cancellationToken);
    }

    // If there are approximately threshold number of items, try and process hello queue

    // Buffer for dequeued items
    List<int> processItems = new List<int>();

    using (var txn = this.StateManager.CreateTransaction())
    {
        ConditionalValue<int> ret;

        do
        {
            ret = await this.Queue.TryDequeueAsync(txn, cancellationToken);

            if (ret.HasValue)
            {
                // If an item was dequeued, add toohello buffer for processing
                processItems.Add(ret.Value);
            }
        } while (processItems.Count < threshold && ret.HasValue);

        await txn.CommitAsync();
    }

    // Process hello dequeues
    for (int i = 0; i < processItems.Count; ++i)
    {
        Console.WriteLine("Value : " + processItems[i]);
    }
}
```

### <a name="best-effort-drain"></a><span data-ttu-id="931ce-191">Legjobb kiürítési</span><span class="sxs-lookup"><span data-stu-id="931ce-191">Best-Effort Drain</span></span>
<span data-ttu-id="931ce-192">A kiürítési hello várólista toohello hello adatszerkezet egyidejű jellege miatt nem garantálható.</span><span class="sxs-lookup"><span data-stu-id="931ce-192">A drain of hello queue cannot be guaranteed due toohello concurrent nature of hello data structure.</span></span>  <span data-ttu-id="931ce-193">Lehetséges, hogy akkor is, ha nincsenek felhasználói műveletek hello várólista üzenetsoroktól, egy adott hívás tooTryDequeueAsync sem adja vissza egy elemet, amely korábban a várólistában levő és véglegesítve.</span><span class="sxs-lookup"><span data-stu-id="931ce-193">It is possible that, even if no user operations on hello queue are in-flight, a particular call tooTryDequeueAsync may not return an item which was previously enqueued and committed.</span></span>  <span data-ttu-id="931ce-194">hello a várólistában levő elem túl garantáltan*végül* lesz látható toodequeue, anélkül, hogy egy sávon kívüli kommunikációs mechanizmus egy független fogyasztó nem tudja hello várólistára elérte a stabil állapot még akkor is, ha azonban minden gyártók le lett állítva, és nincsenek-e új sorba helyezni műveletek engedélyezettek.</span><span class="sxs-lookup"><span data-stu-id="931ce-194">hello enqueued item is guaranteed too*eventually* become visible toodequeue, however without an out-of-band communication mechanism, an independent consumer cannot know that hello queue has reached a steady-state even if all producers have been stopped and no new enqueue operations are allowed.</span></span> <span data-ttu-id="931ce-195">Így a hello kiürítési művelet nem a legjobb alatt megvalósított módon.</span><span class="sxs-lookup"><span data-stu-id="931ce-195">Thus, hello drain operation is best-effort as implemented below.</span></span>

<span data-ttu-id="931ce-196">hello felhasználó minden további gyártó és a felhasználói feladatok állítsa le, és várjon, amíg bármilyen az üzenetsoroktól tranzakciók toocommit vagy a megszakításhoz, mielőtt megpróbálná toodrain hello várólista.</span><span class="sxs-lookup"><span data-stu-id="931ce-196">hello user should stop all further producer and consumer tasks, and wait for any in-flight transactions toocommit or abort, before attempting toodrain hello queue.</span></span>  <span data-ttu-id="931ce-197">Hello felhasználó tudja várt hello hello várólistában lévő elemek száma, ha azok egy értesítés, amely jelzi, hogy minden elem rendelkezik várólistából lett kivéve állíthat be.</span><span class="sxs-lookup"><span data-stu-id="931ce-197">If hello user knows hello expected number of items in hello queue, they can set up a notification which signals that all items have been dequeued.</span></span>

```
int numItemsDequeued;
int batchSize = 5;

ConditionalValue ret;

do
{
    List<int> processItems = new List<int>();

    using (var txn = this.StateManager.CreateTransaction())
    {
        do
        {
            ret = await this.Queue.TryDequeueAsync(txn, cancellationToken);

            if(ret.HasValue)
            {
                // Buffer hello dequeues
                processItems.Add(ret.Value);
            }
        } while (ret.HasValue && processItems.Count < batchSize);

        await txn.CommitAsync();
    }

    // Process hello dequeues
    for (int i = 0; i < processItems.Count; ++i)
    {
        Console.WriteLine("Value : " + processItems[i]);
    }
} while (ret.HasValue);
```

### <a name="peek"></a><span data-ttu-id="931ce-198">Betekintés</span><span class="sxs-lookup"><span data-stu-id="931ce-198">Peek</span></span>
<span data-ttu-id="931ce-199">ReliableConcurrentQueue nem biztosít hello *TryPeekAsync* api.</span><span class="sxs-lookup"><span data-stu-id="931ce-199">ReliableConcurrentQueue does not provide hello *TryPeekAsync* api.</span></span> <span data-ttu-id="931ce-200">Felhasználók férhetnek hello betekintés szemantikai használatával egy *TryDequeueAsync* és majd a hello tranzakció megszakítása.</span><span class="sxs-lookup"><span data-stu-id="931ce-200">Users can get hello peek semantic by using a *TryDequeueAsync* and then aborting hello transaction.</span></span> <span data-ttu-id="931ce-201">Ebben a példában dequeues csak akkor, ha hello elem értéke nagyobb, mint a feldolgozásuk *10*.</span><span class="sxs-lookup"><span data-stu-id="931ce-201">In this example, dequeues are processed only if hello item's value is greater than *10*.</span></span>

```
using (var txn = this.StateManager.CreateTransaction())
{
    ConditionalValue ret = await this.Queue.TryDequeueAsync(txn, cancellationToken);
    bool valueProcessed = false;

    if (ret.HasValue)
    {
        if (ret.Value > 10)
        {
            // Process hello item
            Console.WriteLine("Value : " + ret.Value);
            valueProcessed = true;
        }
    }

    if (valueProcessed)
    {
        await txn.CommitAsync();    
    }
    else
    {
        await txn.AbortAsync();
    }
}
```

## <a name="must-read"></a><span data-ttu-id="931ce-202">Olvasási kell</span><span class="sxs-lookup"><span data-stu-id="931ce-202">Must Read</span></span>
* [<span data-ttu-id="931ce-203">Megbízható szolgáltatások – első lépések</span><span class="sxs-lookup"><span data-stu-id="931ce-203">Reliable Services Quick Start</span></span>](service-fabric-reliable-services-quick-start.md)
* [<span data-ttu-id="931ce-204">A Reliable Collections használata</span><span class="sxs-lookup"><span data-stu-id="931ce-204">Working with Reliable Collections</span></span>](service-fabric-work-with-reliable-collections.md)
* [<span data-ttu-id="931ce-205">Megbízható szolgáltatások értesítések</span><span class="sxs-lookup"><span data-stu-id="931ce-205">Reliable Services notifications</span></span>](service-fabric-reliable-services-notifications.md)
* [<span data-ttu-id="931ce-206">Megbízható szolgáltatások biztonsági mentése és visszaállítása (katasztrófa utáni helyreállítás)</span><span class="sxs-lookup"><span data-stu-id="931ce-206">Reliable Services Backup and Restore (Disaster Recovery)</span></span>](service-fabric-reliable-services-backup-restore.md)
* [<span data-ttu-id="931ce-207">Megbízható állapot Manager konfigurálása</span><span class="sxs-lookup"><span data-stu-id="931ce-207">Reliable State Manager Configuration</span></span>](service-fabric-reliable-services-configuration.md)
* [<span data-ttu-id="931ce-208">Bevezetés a Service Fabric webszolgáltatások API használatába</span><span class="sxs-lookup"><span data-stu-id="931ce-208">Getting Started with Service Fabric Web API Services</span></span>](service-fabric-reliable-services-communication-webapi.md)
* [<span data-ttu-id="931ce-209">Hello megbízható programozási modell speciális használata</span><span class="sxs-lookup"><span data-stu-id="931ce-209">Advanced Usage of hello Reliable Services Programming Model</span></span>](service-fabric-reliable-services-advanced-usage.md)
* [<span data-ttu-id="931ce-210">Fejlesztői leírás megbízható gyűjtemények</span><span class="sxs-lookup"><span data-stu-id="931ce-210">Developer Reference for Reliable Collections</span></span>](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
