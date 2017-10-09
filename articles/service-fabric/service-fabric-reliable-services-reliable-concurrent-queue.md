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
# <a name="introduction-tooreliableconcurrentqueue-in-azure-service-fabric"></a>Az Azure Service Fabric bemutatása tooReliableConcurrentQueue
Megbízható egyidejű várólista egy aszinkron, a tranzakciós és a replikált sor mely szolgáltatások magas CONCURRENCY paraméterének értékét sorba helyezni, és műveletek feldolgozásához. Tervezett toodeliver magas teljesítmény és alacsony késési igényű által lazítani a hello szigorú FIFO rendelési által biztosított [megbízható várólista](https://msdn.microsoft.com/library/azure/dn971527.aspx) és biztosítja a legjobb rendezést.

## <a name="apis"></a>API-k

|Egyidejű várólista                |Megbízható egyidejű várólista                                         |
|--------------------------------|------------------------------------------------------------------|
| "void" Enqueue(T item)           | A feladat EnqueueAsync (ITransaction tx, T elem)                       |
| logikai TryDequeue (kimenő T eredmény)  | A feladat < ConditionalValue < T >> TryDequeueAsync (ITransaction tx)  |
| int Count()                    | hosszú Count()                                                     |

## <a name="comparison-with-reliable-queuehttpsmsdnmicrosoftcomlibraryazuredn971527aspx"></a>Az összehasonlítás [megbízható várólista](https://msdn.microsoft.com/library/azure/dn971527.aspx)

Megbízható egyidejű várólista érhető el alternatív túl[megbízható várólista](https://msdn.microsoft.com/library/azure/dn971527.aspx). Olyan esetekben, ahol szigorú FIFO rendezés nem szükséges, használjon, FIFO együtt van szükség egy kompromisszumot biztosítása.  [Megbízható várólista](https://msdn.microsoft.com/library/azure/dn971527.aspx) zárolások tooenforce FIFO rendezést, és legfeljebb egy tranzakciót tooenqueue engedélyezett toodequeue engedélyezett egyszerre legfeljebb egy tranzakciót használ. Szemben megbízható egyidejű várólista megkötés rendelési hello visszaállítja, és lehetővé teszi, hogy a bármely száma párhuzamos tranzakciók toointerleave a sorba helyezni, és műveletek feldolgozásához. Legjobb rendelési kerül, de hello relatív sorrendjének megbízható egyidejű várólista két értékek soha nem garantálható.

Megbízható egyidejű várólista biztosít nagyobb átviteli teljesítményt és kisebb késést biztosít [megbízható várólista](https://msdn.microsoft.com/library/azure/dn971527.aspx) vannak enqueues végrehajtása több egyidejű tranzakciókat és/vagy dequeues.

Egy minta-és nagybetűhasználattal hello ReliableConcurrentQueue: hello [üzenet-várólista](https://en.wikipedia.org/wiki/Message_queue) forgatókönyv. Ebben a forgatókönyvben egy vagy több üzenetek létrehozói létre elemek toohello várólista hozzáadása és egy vagy több üzenetet fogyasztók hello várólistában lévő üzenetek le és dolgozza fel őket. Több létrehozói és felhasználói is működjön egymástól függetlenül, egyidejű tranzakciókat rendelés tooprocess hello várólistában.

## <a name="usage-guidelines"></a>Használatára vonatkozó irányelvek
* hello várólista vár, hogy hello hello várólista útjuk alacsony megőrzési időtartamot. Ez azt jelenti, hogy hello elemek akkor nem marad hello várólista hosszú ideig.
* hello várólista nem garantálja a szigorú FIFO rendezést.
* hello várólista nem olvassa a saját írási műveleteket. Ha a cikk a várólistában levő tranzakción belül, akkor nem fog látható tooa dequeuer belül hello ugyanabban a tranzakcióban.
* Dequeues nem el különítve egymástól. Ha a cikk *A* várólistából tranzakcióban van-e kivéve *txnA*, annak ellenére, hogy *txnA* nincs véglegesítve, elem *A* nem lenne látható tooa egyidejű tranzakció *txnB*.  Ha *txnA* megszakítja, *A* láthatóvá válnak túl*txnB* azonnal.
* *TryPeekAsync* viselkedés használatával valósítható egy *TryDequeueAsync* és majd a hello tranzakció megszakítása. Például az hello programozási minták szakaszban találhatók.
* Számláló értéke nem tranzakciós. Azt lehet használt tooget hello hello várólistában lévő elemek száma a képet, de egy-időpontban jelöli, és nem lehet hivatkozni.
* Költséges hello feldolgozási várólistából kivéve elemeket kell nem hajtható végre, amíg hello tranzakció már aktív, ami hatással lehet a teljesítmény hello rendszer tooavoid hosszan futó tranzakciók.

## <a name="code-snippets"></a>Kódtöredékek
Ossza meg velünk tekintse meg néhány kódtöredékek és a várható kimenetekkel. Kivétel ebben a szakaszban figyelmen kívül hagyja.

### <a name="enqueueasync"></a>EnqueueAsync
Az alábbiakban néhány kódrészleteket a várt kimeneti követ EnqueueAsync használatával.

- *1. eset: Egy sorba helyezni a feladat*

```
using (var txn = this.StateManager.CreateTransaction())
{
    await this.Queue.EnqueueAsync(txn, 10, cancellationToken);
    await this.Queue.EnqueueAsync(txn, 20, cancellationToken);

    await txn.CommitAsync();
}
```

Tegyük fel, hogy hello feladat sikeresen befejeződött, és, amelyek nincsenek hello várólista módosítása párhuzamos tranzakciók. hello felhasználó számíthat hello várólista toocontain hello elemek hello megrendelések a következő valamelyikén:

> 10, 20

> 20, 10


- *2. eset: Párhuzamos sorba helyezni a feladat*

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

Tegyük fel, hogy hello feladatok, sikeresen befejeződött, hogy hello feladatok párhuzamosan futtatta-e, és, hogy történtek-e más egyidejű hello várólista módosítását tranzakciók. Nincs megállapítás végezhető hello hello várólistában lévő elemek sorrendjét. A következő kódrészletet hello is megjelenhetnek hello 4 bármelyikét! lehetséges rendezés.  hello várólista tookeep hello elemek hello eredeti (várólistán lévő) sorrendben megpróbál, de lehet, hogy kényszerített tooreorder megfelelő tooconcurrent műveleteket vagy hibákat őket.


### <a name="dequeueasync"></a>DequeueAsync
Az alábbiakban néhány kódrészleteket várt hello kimenetek követ TryDequeueAsync használatával. Tegyük fel, hello várólistára már fel van töltve a hello hello várólistában lévő elemek a következő:
> 10, 20, 30, 40, 50, 60

- *1. eset: Egyetlen created feladat*

```
using (var txn = this.StateManager.CreateTransaction())
{
    await this.Queue.TryDequeueAsync(txn, cancellationToken);
    await this.Queue.TryDequeueAsync(txn, cancellationToken);
    await this.Queue.TryDequeueAsync(txn, cancellationToken);

    await txn.CommitAsync();
}
```

Tegyük fel, hogy hello feladat sikeresen befejeződött, és, amelyek nincsenek hello várólista módosítása párhuzamos tranzakciók. Óta nincs megállapítás hello várólista hello elemek sorrendjének hello végezhető, bármely három hello elemek előfordulhat, hogy lehet várólistából kivéve, bármilyen sorrendben. hello várólista tookeep hello elemek hello eredeti (várólistán lévő) sorrendben megpróbál, de lehet, hogy kényszerített tooreorder megfelelő tooconcurrent műveleteket vagy hibákat őket.  

- *2. eset: Párhuzamosan created feladat*

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

Tegyük fel, hogy hello feladatok, sikeresen befejeződött, hogy hello feladatok párhuzamosan futtatta-e, és, hogy történtek-e más egyidejű hello várólista módosítását tranzakciók. Mivel nincs megállapítás hello várólista hello elemek sorrendjének hello végezhető, hello listák *dequeue1* és *dequeue2* egyes bármely két elemet tartalmaz, bármilyen sorrendben.

azonos elem fog hello *nem* mindkét listán. Ezért ha dequeue1 *10*, *30*, majd dequeue2 kellene *20*, *40*.

- *3. eset: Created rendelését tranzakció megszakítása*

Az üzenetsoroktól a tranzakció megszakítása dequeues visszahelyezi hello munkaelemeket visszaküldeni a hello head hello várólista. ahol hello elemek kerülnek vissza hello head hello várólista hello sorrendje nem garantált. Ossza meg velünk nézze meg a következő kód hello:

```
using (var txn = this.StateManager.CreateTransaction())
{
    await this.Queue.TryDequeueAsync(txn, cancellationToken);
    await this.Queue.TryDequeueAsync(txn, cancellationToken);

    // Abort hello transaction
    await txn.AbortAsync();
}
```
Tegyük fel, hogy a hello elemek várólistából volt kivéve sorrend hello:
> 10, 20

Azt a hello tranzakció megszakítása, ha hello elemek hello megrendelések a következő valamelyikén hátsó toohello head hello várólista volna kerülnek:
> 10, 20

> 20, 10

hello esetében is igaz minden olyan esetben, ahol hello tranzakció nem volt sikeresen *lekötött*.

## <a name="programming-patterns"></a>Programozási minták
Ebben a szakaszban ossza meg velünk tekintse meg néhány programozási minta, érdemes lehet megfontolni a ReliableConcurrentQueue használatával.

### <a name="batch-dequeues"></a>Kötegelt Dequeues
A javasolt programozási minta pedig a hello fogyasztói feladat toobatch annak egyik végzett helyett dequeues created egyszerre. hello felhasználó választhat toothrottle késések minden köteg vagy hello kötegméret között. hello következő kódrészletet a programozási-modelljét mutatja.  Vegye figyelembe, hogy ebben a példában hello nem hajtja végre után hello tranzakció véglegesítése, így ha egy tartalék toooccur feldolgozása közben, hello feldolgozatlan elemek elvesznek feldolgozás nélkül.  Másik lehetőségként hello feldolgozási végezhető hello tranzakció hatókörében, azonban ez negatív hatással lehet a teljesítményre, és szükséges hozzá hello cikkek kezelése már feldolgozott.

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

### <a name="best-effort-notification-based-processing"></a>Legjobb értesítés-alapú feldolgozási
Egy másik érdekes programozási mintát hello száma API-t használja. Itt ket lehet megvalósítani legjobb értesítés-alapú feldolgozási hello várólista. hello várólista száma sorba állítani egy használt toothrottle vagy egy dequeue feladat lehet.  Előfordulhat, hogy hello előző példához hasonlóan óta hello feldolgozása befejeződik hello tranzakción kívüli feldolgozatlan elemek elvesznek, ha a hiba akkor fordul elő, feldolgozás során.

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

### <a name="best-effort-drain"></a>Legjobb kiürítési
A kiürítési hello várólista toohello hello adatszerkezet egyidejű jellege miatt nem garantálható.  Lehetséges, hogy akkor is, ha nincsenek felhasználói műveletek hello várólista üzenetsoroktól, egy adott hívás tooTryDequeueAsync sem adja vissza egy elemet, amely korábban a várólistában levő és véglegesítve.  hello a várólistában levő elem túl garantáltan*végül* lesz látható toodequeue, anélkül, hogy egy sávon kívüli kommunikációs mechanizmus egy független fogyasztó nem tudja hello várólistára elérte a stabil állapot még akkor is, ha azonban minden gyártók le lett állítva, és nincsenek-e új sorba helyezni műveletek engedélyezettek. Így a hello kiürítési művelet nem a legjobb alatt megvalósított módon.

hello felhasználó minden további gyártó és a felhasználói feladatok állítsa le, és várjon, amíg bármilyen az üzenetsoroktól tranzakciók toocommit vagy a megszakításhoz, mielőtt megpróbálná toodrain hello várólista.  Hello felhasználó tudja várt hello hello várólistában lévő elemek száma, ha azok egy értesítés, amely jelzi, hogy minden elem rendelkezik várólistából lett kivéve állíthat be.

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

### <a name="peek"></a>Betekintés
ReliableConcurrentQueue nem biztosít hello *TryPeekAsync* api. Felhasználók férhetnek hello betekintés szemantikai használatával egy *TryDequeueAsync* és majd a hello tranzakció megszakítása. Ebben a példában dequeues csak akkor, ha hello elem értéke nagyobb, mint a feldolgozásuk *10*.

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

## <a name="must-read"></a>Olvasási kell
* [Megbízható szolgáltatások – első lépések](service-fabric-reliable-services-quick-start.md)
* [A Reliable Collections használata](service-fabric-work-with-reliable-collections.md)
* [Megbízható szolgáltatások értesítések](service-fabric-reliable-services-notifications.md)
* [Megbízható szolgáltatások biztonsági mentése és visszaállítása (katasztrófa utáni helyreállítás)](service-fabric-reliable-services-backup-restore.md)
* [Megbízható állapot Manager konfigurálása](service-fabric-reliable-services-configuration.md)
* [Bevezetés a Service Fabric webszolgáltatások API használatába](service-fabric-reliable-services-communication-webapi.md)
* [Hello megbízható programozási modell speciális használata](service-fabric-reliable-services-advanced-usage.md)
* [Fejlesztői leírás megbízható gyűjtemények](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
