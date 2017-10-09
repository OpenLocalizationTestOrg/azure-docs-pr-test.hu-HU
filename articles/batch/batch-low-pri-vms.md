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
# <a name="use-low-priority-vms-with-batch-preview"></a>Kis prioritású virtuális gépek használata a kötegelt (előzetes verzió)

Az Azure Batch biztosít alacsony-priorty virtuális gépek (VM) tooreduce hello kötegelt munkaterhelések költségét. Kis prioritású virtuális gépek új kötegelt munkaterhelés-típusok lehetővé teszik, adja meg a számítási teljesítményt, amely egyúttal gazdaságos nagy mennyiségű.

Kis prioritású virtuális gépek előnyeit felesleges kapacitás az Azure-ban. Kis prioritású virtuális gépek a készletek megadásakor Azure Batch automatikusan használhatják az eredmény, ha elérhető.

kis prioritású virtuális gépek használatának hello kompromisszumot, hogy virtuális gépek az Azure-ban elérhető nincs felesleges kapacitás esetén előfordulhat, hogy foglalható. Emiatt alacsony prioritású virtuális gépek legmegfelelőbb bizonyos munkaterhelések esetében. Alacsony prioritású virtuális gépek használata kötegelt és aszinkron feldolgozási terheléshez, ahol hello feladat befejezési idő rugalmas, és hello munkahelyi sok virtuális gép között van elosztva.

Kis prioritású virtuális gépek jelentősen olcsóbb dedikált virtuális gépek. Díjszabása, lásd: [kötegelt árképzési](https://azure.microsoft.com/pricing/details/batch/).

Egy alacsony prioritású virtuális gépek további tárgyalását lásd: hello blogbejegyzésben található: [hello ár töredéke alatt, a számítástechnikai kötegelt](https://azure.microsoft.com/blog/announcing-public-preview-of-azure-batch-low-priority-vms/).

> [!IMPORTANT]
> Alacsony prioritású virtuális gép jelenleg előzetes verziójúak, és csak a kötegelt futó feladatok. 
>
>

## <a name="use-cases-for-low-priority-vms"></a>Kis prioritású virtuális gépek alkalmazási helyzetei

Alacsony prioritású virtuális gépek jellemzői hello kap, milyen számítási feladatokat is, és nem használja őket? Általában kötegelt feldolgozási terheléshez amelyeket remekül beválik, feladatok számos párhuzamos feladat van felosztva, vagy sok feladat horizontálisan és sok virtuális gép pontjain.

-   az Azure, a megfelelő feladatoknál felesleges kapacitás toomaximize használatát lehet horizontálisan.

-   Alkalmanként a virtuális gépek nem érhető el, vagy fog foglalható, amely a feladatok korlátozott kapacitás eredményez, így tootask megszakítás és ismétlések. Feladatok ezért rugalmas hello időben toorun érvénybe kell lennie.

-   Feladatok hosszabb ideig feladatok több érintheti, ha megszakad. Ha hosszú ideig futó feladatok végrehajtása ellenőrzőpontok toosave folyamatban van, akkor hajtható végre, majd a hatása megszakítás lenne sokkal kevesebb. Rövidebb végrehajtásának lassúságát a feladatok általában toowork legjobb alacsony prioritású virtuális gépek megszakítás hello hatása sokkal kevesebb mint.

-   Hosszú futású MPI-feladatok, amelyek ténylegesen használják több virtuális gépek nincsenek kiválóan alkalmas toouse alacsony prioritású virtuális gépeket egy leállított virtuális gépként valószínűleg az érdeklődési toohello teljes feladat újrafuttatása toobe rendelkező lesz.

Néhány példa a köteges feldolgozás esetekben megfelelőek toouse alacsony prioritású virtuális gépeken használja:

-   **Fejlesztéshez és teszteléshez**: különösen akkor, ha a felügyeleti teendők központjaként megoldások fejlesztés alatt jelentős megtakarítást is kell megvalósítani. Tesztelési minden típusú kihasználhassa, de nagy terhelés tesztelése és regressziós tesztelés kiváló használ.

-   **Igény szerinti kapacitás kiegészítése**: alacsony prioritású virtuális gépek segítségével kiegészíti az regular dedikált virtuális gépek – Ha elérhető, a feladatok méretezése, és ezért alacsonyabb költségek gyorsabb befejezéséhez; Ha nem érhető el, hello alapkonfiguráció a dedikált virtuális gépek érhetők el.

-   **Rugalmas feladat végrehajtási ideje**: Ha hello idő feladatok rugalmasságot toocomplete, majd lehetséges elhagyta a kapacitás is megengedett megegyezni azonban hello alacsony prioritású virtuális gépek feladatok hozzáadását fog gyakran futtatja a gyorsabb és az alacsonyabb költségek.

Kötegelt készletek lehet konfigurált toouse, alacsony prioritású virtuális gépek által számos módja, attól függően, hello rugalmasságot feladat-végrehajtási idő:

-   Kis prioritású virtuális gépek kizárólag a készletben használható, és kötegelt egyszerűen állítsa helyre a preempted kapacitás, ha elérhető. Ez az hello legolcsóbb módon tooexecute feladatok csak alacsony prioritású virtuális gép használja.

-   Kis prioritású virtuális gépek dedikált virtuális gépek rögzített alapterv együtt használható. hello dedikált virtuális gépek rögzített száma biztosítja, van néhány kapacitás tookeep mindig egy feladat halad.

-   Lehet dedikált és alacsony prioritású virtuális gépeket, a dinamikus kombinációját, hogy az olcsóbb alacsony prioritású virtuális gépek kizárólag akkor használatosak, ha elérhető, de hello teljes árú dedikált virtuális gépek méretezhető, ha szükséges, a minimális kapacitás elérhető tookeep hello feladatok tookeep halad.

## <a name="batch-support-for-low-priority-vms"></a>Kis prioritású virtuális gépek kötegelt támogatása

Az Azure Batch szolgáltatás különböző képességeket, amelyek révén könnyen tooconsume, és igénybe vehesse az alacsony prioritású virtuális gépek:

-   Kötegelt készletek dedikált virtuális gépek és az alacsonyabb prioritású virtuális gépek is tartalmazhat. virtuális gép különböző típusú hello száma készlet létrehozásakor, vagy módosítani egy meglévő készlet hello explicit átméretezés vagy automatikus skálázása használatával bármikor adható meg. Feladat- és küldésének változatlan marad, és nem kell érintett hello VM típusú hello készletben. Akkor is lehetséges toohave készlet teljesen használja, alacsony prioritású virtuális gépek toorun feladatok, de dedikált virtuális gépeinek léptetéses olcsón Ha hello kapacitás, futó feladatok tartani a minimális küszöbérték alá csökken.

-   Kötegelt készletek automatikusan keresni alacsony prioritású virtuális gépek száma toohello cél. Ha a leállított virtuális gépeken, kötegelt elvesztette a kapacitás és a visszatérési toohello cél tooreplace hello megpróbál.

-   Kötegelt feladatok alatt megszakadt hello esetben azonosítja, és automatikusan újra a feladatok toobe futtassa újra a várólistába helyezi.

-   Kis prioritású virtuális gépek eltér dedikált virtuális core kvóta rendelkezik. 
    hello idézőjel alacsony prioritású virtuális gép esetében nem nagyobb, mint a dedikált virtuális gépek, mert alacsony prioritású virtuális gépek költséget. Lásd: [a Batch szolgáltatás kvótái és korlátai](batch-quota-limit.md#resource-quotas) további információt.    

> [!NOTE]
> Alacsony prioritású virtuális gép jelenleg nem támogatottak a Batch-fiókok, amelyben hello tárolókészlet foglalási mód van megadva túl[felhasználói előfizetési](batch-account-create-portal.md#user-subscription-mode).
>
>

## <a name="create-and-update-pools"></a>Hozzon létre, és a készletek frissítése

A Batch-készlet dedikált és alacsony prioritású virtuális gépek (is hivatkozott tooas számítási csomópontok) tartalmazhat. Beállíthatja a számítási csomópontok száma hello cél dedikált és alacsony prioritású virtuális gép esetében. hello csomópontok száma cél számát határozza meg hello toohave hello készlet kívánt virtuális gépek.

Például a virtuális gépek Azure-felhőszolgáltatásban használata 5 célja készlet toocreate dedikált virtuális gépek és 20 alacsony prioritású virtuális gép:

```csharp
CloudPool pool = batchClient.PoolOperations.CreatePool(
    poolId: "cspool",
    targetDedicatedComputeNodes: 5,
    targetLowPriorityComputeNodes: 20,
    virtualMachineSize: "Standard_D2_v2",
    cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4") // WS 2012 R2
);
```

a készlet (ebben az esetben Linux virtuális gépek) az Azure virtuális gépek használata 5 célja toocreate dedikált virtuális gépek és 20 alacsony prioritású virtuális gép:

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

Dedikált és alacsony prioritású virtuális gépek kaphat hello aktuális csomópontok száma:

```csharp
int? numDedicated = pool1.CurrentDedicatedComputeNodes;
int? numLowPri = pool1.CurrentLowPriorityComputeNodes;
```

Készlet csomópontjain van-e egy tulajdonság tooindicate hello csomópont egy dedikált vagy alacsony prioritású virtuális gép esetén:

```csharp
bool? isNodeDedicated = poolNode.IsDedicated;
```

Ha a készlet egy vagy több csomópontja részesítés, a készlet egy lista csomópontok művelet továbbra is visszaállítja azokat a csomópontokat, alacsony prioritású csomópontok aktuális száma hello változatlan marad, de azokat a csomópontokat, kell állapotukra beállítása toothe **Prioritás miatt kihagyva**állapotát. Kötegelt megpróbál toofind helyettesítő virtuális gépeket, és ha sikeres, hello csomópontok végighaladnia **létrehozása** , majd **indítása** állapotok ahhoz, hogy elérhető-e a feladat végrehajtásához, ugyanúgy, mint az új csomópontok.

## <a name="scale-a-pool-containing-low-priority-vms"></a>Kis prioritású virtuális gépeket tartalmazó készlet méretezése

Csakúgy, mint a gyűjtők kizárólag álló dedikált virtuális gépek, is lehetséges tooscale a készletet tartalmazó alacsony prioritású virtuális gépek hello átméretezési metódus hívása vagy automatikus skálázása.

hello készlet átméretezés egy második választható paramétert fogad, amely frissíti a **targetLowPriorityNodes**:

```csharp
pool.Resize(targetDedicatedComputeNodes: 0, targetLowPriorityComputeNodes: 25);
```

hello készlet automatikus méretezése képlet az alábbiak szerint támogatja az alacsony prioritású virtuális gépek:

-   Beolvasása, vagy állítsa be hello hello szolgáltatás által definiált változó **$TargetLowPriorityNodes**.

-   Hello szolgáltatás által definiált változó értékének hello kaphat **$CurrentLowPriorityNodes**.

-   Hello szolgáltatás által definiált változó értékének hello kaphat **$PreemptedNodeCount**. 
    Ez a változó hello hello csomópontok száma a leállított állapotot, és méretezheti felfelé vagy lefelé dedikált csomópontok, attól függően, hogy nem érhetők el a leállított csomópontok száma hello hello számát adja vissza.

## <a name="jobs-and-tasks"></a>Feladatok és feladatok

Feladatok és a feladatok csekély-támogatásra van szüksége az alacsony prioritású csomópont; hello csak támogatása a következőképpen történik:

-   hello JobManagerTask tulajdonság egy feladat tulajdonsága egy új, **AllowLowPriorityNode**. 
    Ez a tulajdonság értéke igaz, amikor egy dedikált vagy alacsony prioritású csomópont hello manager feladat is ütemezhető. Ha ez a tulajdonság értéke HAMIS, hello kezelő feladat nem ütemezett tooa csak a kijelölt csomópont.

-   Egy [környezeti változó](batch-compute-node-environment-variables.md) elérhető tooa feladat alkalmazás van, hogy megállapíthassa, hogy egy alacsony prioritású vagy dedikált csomóponton futó. hello környezeti változó AZ_BATCH_NODE_IS_DEDICATED.

## <a name="handling-preemption"></a>A megelőlegezés kezelése

Virtuális gépek alkalmanként foglalható; Ha ez történik, a kötegelt hello a következő:

-   hello a leállított virtuális gépek rendelkezik frissíti állapotukra**Prioritás miatt kihagyva**.
-   Ha futó feladatok a hello leállított csomópont virtuális gépeket, majd ezeket a feladatokat helyezte, és futtassa újból.
-   virtuális gép hello hatékonyan törölték, helyileg tárolt virtuális gép elvesztését hello tooany adatok vezető.
-   hello készlet folyamatosan megpróbál tooreach hello cél elérhető alacsony prioritású csomópontok száma. Ha helyettesítő kapacitás tartalmaz, a csomópontok tartani az azonosítók, de újbóli inicializálása, keresztül haladó **létrehozása** és **indítása** előtt a feladat ütemezés szerint.
-   A megelőlegezés számok állnak rendelkezésre, egy metrika a hello Azure-portálon.

## <a name="metrics"></a>Mérőszámok

Új mérőszámok érhetők el hello [Azure-portálon](https://portal.azure.com) az alacsony prioritású csomópont. Ezen adatok gyűjtése le van:

- Alacsony prioritású csomópontok száma
- Alacsony prioritású magok száma 
- Csomópont Count részesítés

tooview metrikát a hello Azure-portálon:

1. Keresse meg a Batch-fiók hello portálon tooyour, és a Batch-fiók hello beállítások megtekintése.
2. Válassza ki **metrikák** a hello **figyelés** szakasz.
3. Válassza ki a felügyelni hello metrikákat a hello **elérhető** listája.

![Alacsony prioritású csomópontok metrikák](media/batch-low-pri-vms/low-pri-metrics.png)

## <a name="next-steps"></a>Következő lépések

* Olvasási hello [Batch funkcióinak áttekintése a fejlesztők](batch-api-basics.md), bárki toouse kötegelt előkészítése alapvető információkat. hello cikkben Batch szolgáltatás erőforrásokhoz, mint a készletek, a csomópontok, a feladatok, és a feladatok és a hello részletesebb információkat is használhatja a kötegelt kérelem felépítésekor sok API-funkciókat.
* További tudnivalók: hello [kötegelt API-k és eszközök](batch-apis-tools.md) kötegelt megoldások érhetők el.
