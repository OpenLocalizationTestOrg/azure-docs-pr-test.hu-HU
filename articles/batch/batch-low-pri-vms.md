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
# <a name="use-low-priority-vms-with-batch-preview"></a>Kis prioritású virtuális gépek használata a kötegelt (előzetes verzió)

Az Azure Batch biztosít alacsony-priorty virtuális gépek (VM) kötegelt feladatok a költségek csökkentése. Kis prioritású virtuális gépek új kötegelt munkaterhelés-típusok lehetővé teszik, adja meg a számítási teljesítményt, amely egyúttal gazdaságos nagy mennyiségű.

Kis prioritású virtuális gépek előnyeit felesleges kapacitás az Azure-ban. Kis prioritású virtuális gépek a készletek megadásakor Azure Batch automatikusan használhatják az eredmény, ha elérhető.

Mi a fontosabb: az alacsony prioritású virtuális gép, hogy virtuális gépek az Azure-ban elérhető nincs felesleges kapacitás esetén előfordulhat, hogy foglalható. Emiatt alacsony prioritású virtuális gépek legmegfelelőbb bizonyos munkaterhelések esetében. Alacsony prioritású virtuális gépek használata kötegelt és aszinkron feldolgozási terheléshez, ahol a feladat befejezési idő rugalmas, és a munka sok virtuális gép között van elosztva.

Kis prioritású virtuális gépek jelentősen olcsóbb dedikált virtuális gépek. Díjszabása, lásd: [kötegelt árképzési](https://azure.microsoft.com/pricing/details/batch/).

Egy alacsony prioritású virtuális gépek további tárgyalását lásd a következő blogbejegyzésben található: [ár töredéke alatt, a számítástechnikai kötegelt](https://azure.microsoft.com/blog/announcing-public-preview-of-azure-batch-low-priority-vms/).

> [!IMPORTANT]
> Alacsony prioritású virtuális gép jelenleg előzetes verziójúak, és csak a kötegelt futó feladatok. 
>
>

## <a name="use-cases-for-low-priority-vms"></a>Kis prioritású virtuális gépek alkalmazási helyzetei

Alacsony prioritású virtuális gépeket, mi munkaterhelések is, és nem használja őket figyelembe véve? Általában kötegelt feldolgozási terheléshez amelyeket remekül beválik, feladatok számos párhuzamos feladat van felosztva, vagy sok feladat horizontálisan és sok virtuális gép pontjain.

-   Használja az Azure-ban felesleges kapacitás maximalizálása érdekében megfelelő feladatok lehet terjeszteni.

-   Alkalmanként a virtuális gépek nem érhető el, vagy fog foglalható, ami azt eredményezheti, hogy a feladat megszakítása és ismétlések és feladatok korlátozott kapacitás eredményezi. Feladatok ezért rugalmas a futtatásához érvénybe belül kell lennie.

-   Feladatok hosszabb ideig feladatok több érintheti, ha megszakad. Ha hosszú ideig futó feladatok végrehajtása ellenőrzőpontok mentése folyamatban van, akkor hajtható végre, majd a hatása megszakítás lenne sokkal kevesebb. Rövidebb végrehajtásának lassúságát a feladatok általában a legmegfelelőbb alacsony prioritású virtuális gépek, a megszakítás hatását sokkal kevesebb.

-   Hosszú futású MPI-feladatok, amelyek ténylegesen használják több virtuális gépek nincsenek kiválóan alkalmas alacsony prioritású virtuális gépek használatára, mert egy virtuális gép leállított valószínűleg vezet a teljes feladat nem futtatható újra kell.

Néhány példa a kötegelt feldolgozásra használati esetek jól, alacsony prioritású virtuális gépek használatára alkalmas a következők:

-   **Fejlesztéshez és teszteléshez**: különösen akkor, ha a felügyeleti teendők központjaként megoldások fejlesztés alatt jelentős megtakarítást is kell megvalósítani. Tesztelési minden típusú kihasználhassa, de nagy terhelés tesztelése és regressziós tesztelés kiváló használ.

-   **Igény szerinti kapacitás kiegészítése**: alacsony prioritású virtuális gépek segítségével kiegészíti az regular dedikált virtuális gépek – Ha elérhető, a feladatok méretezése, és ezért alacsonyabb költségek gyorsabb befejezéséhez; nem érhető el, ha az alapkonfiguráció dedikált virtuális gépek érhetők el.

-   **Rugalmas feladat végrehajtási ideje**: esetén az idő-feladatok rugalmasságot kell végrehajtania, majd lehetséges elhagyta a kapacitás is megengedett; alacsony prioritású virtuális gépek hozzáadásával feladatok gyakran futtathatók gyorsabb és az alacsonyabb költségek.

Kötegelt készletek beállítható úgy, hogy számos módja, attól függően, hogy a feladat végrehajtási ideje rugalmasságot alacsony prioritású virtuális gépek használata:

-   Kis prioritású virtuális gépek kizárólag a készletben használható, és kötegelt egyszerűen állítsa helyre a preempted kapacitás, ha elérhető. Ez módja a legolcsóbb feladatok végrehajtásához, csak alacsony prioritású virtuális gép használja.

-   Kis prioritású virtuális gépek dedikált virtuális gépek rögzített alapterv együtt használható. A dedikált virtuális gépek rögzített száma biztosítja, hogy mindig van néhány kapacitás tartani a feladat halad.

-   Lehet dedikált és alacsony prioritású virtuális gépeket, a dinamikus kombinációját, hogy az olcsóbb alacsony prioritású virtuális gépek kizárólag akkor használatosak, ha elérhető, de a teljes árú dedikált virtuális gépek vannak kiterjesztett szükség esetén elérhető, a feladatok elmélyedne a kapacitás minimális tartani.

## <a name="batch-support-for-low-priority-vms"></a>Kis prioritású virtuális gépek kötegelt támogatása

Az Azure Batch szolgáltatás különböző képességeket, amelyek megkönnyítik az használnak, és igénybe vehesse az alacsony prioritású virtuális gépek:

-   Kötegelt készletek dedikált virtuális gépek és az alacsonyabb prioritású virtuális gépek is tartalmazhat. A virtuális gép típusonkénti számát a készlet létrehozásakor, vagy megváltozott a meglévő készletek, az explicit átméretezés vagy automatikus skálázása használatával bármikor adható meg. Feladat- és küldésének változatlan marad, és nem kell a virtuális gép típusú, a készlet érintett. Úgy is is rendelkezik, és teljesen használnak alacsony prioritású virtuális gépeket, feladatok, de dedikált virtuális gépeinek léptetéses olcsón futtatása, ha a kapacitás, futó feladatok tartani a minimális küszöbérték alá csökken.

-   Kötegelt készletek automatikusan kereshető a cél alacsony prioritású virtuális gépek számát. Ha a leállított virtuális gépeket, majd kötegelt megpróbálja cserélje ki az elveszett kapacitás, és térjen vissza a cél.

-   Folyamatban megszakadt, feladatok, ha kötegelt azonosítja, és automatikusan újra a várólistába helyezi feladatok újra kell futtatni.

-   Kis prioritású virtuális gépek eltér dedikált virtuális core kvóta rendelkezik. 
    Az ajánlat, alacsony prioritású virtuális gépek nem nagyobb, mint a dedikált virtuális gépek, mert alacsony prioritású virtuális gépek költséget. Lásd: [a Batch szolgáltatás kvótái és korlátai](batch-quota-limit.md#resource-quotas) további információt.    

> [!NOTE]
> Alacsony prioritású virtuális gépek jelenleg nem támogatottak a Batch-fiókok, ahol a tárolókészlet foglalási mód értéke [felhasználói előfizetési](batch-account-create-portal.md#user-subscription-mode).
>
>

## <a name="create-and-update-pools"></a>Hozzon létre, és a készletek frissítése

A Batch-készlet dedikált és alacsony prioritású virtuális gép (más néven a a számítási csomópontokkal) tartalmazhat. Beállíthatja a számítási csomópontok száma cél dedikált és alacsony prioritású virtuális gép esetében. A tároló csomópontok száma azt a van a tárolókészletben kívánt virtuális gépeket.

Például készlet létrehozása a virtuális gépek Azure-felhőszolgáltatásban egy target 5 dedikált virtuális gépek és 20 alacsony prioritású virtuális gép:

```csharp
CloudPool pool = batchClient.PoolOperations.CreatePool(
    poolId: "cspool",
    targetDedicatedComputeNodes: 5,
    targetLowPriorityComputeNodes: 20,
    virtualMachineSize: "Standard_D2_v2",
    cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4") // WS 2012 R2
);
```

Készlet létrehozása az Azure virtuális gépek (ebben az esetben Linux virtuális gépek) 5 cél dedikált használó virtuális gépek és 20 alacsony prioritású virtuális gép:

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

Dedikált és alacsony prioritású virtuális gépek kaphat a csomópontok száma:

```csharp
int? numDedicated = pool1.CurrentDedicatedComputeNodes;
int? numLowPri = pool1.CurrentLowPriorityComputeNodes;
```

Készlet csomópontjain van-e a tulajdonság azt jelzi, ha a csomópont egy alacsony prioritású vagy dedikált virtuális Gépet:

```csharp
bool? isNodeDedicated = poolNode.IsDedicated;
```

Amikor egy alkalmazáskészlet egy vagy több csomópontja részesítés, a készlet egy lista csomópontok művelet továbbra is visszaállítja azokat a csomópontokat, alacsony prioritású csomópontok száma változatlan marad, de azokat a csomópontokat, kell az állapot értéke a **Prioritás miatt kihagyva** állapotát. Kötegelt megkísérli található virtuális gépek cserélni, és ha sikeres, a csomópontok végighaladnia **létrehozása** , majd **indítása** állapotok ahhoz, hogy elérhető-e a feladat végrehajtásához, ugyanúgy, mint az új csomópontok.

## <a name="scale-a-pool-containing-low-priority-vms"></a>Kis prioritású virtuális gépeket tartalmazó készlet méretezése

Csakúgy, mint a gyűjtők kizárólag álló dedikált virtuális gépek, egy készletet, alacsony prioritású virtuális gépeket tartalmazó az átméretezési metódus hívása vagy automatikus skálázása segítségével méretezése.

Az alkalmazáskészlet átméretezés egy második választható-paramétert fogad, amely frissíti a **targetLowPriorityNodes**:

```csharp
pool.Resize(targetDedicatedComputeNodes: 0, targetLowPriorityComputeNodes: 25);
```

A készlet automatikus méretezése képlet az alábbiak szerint támogatja az alacsony prioritású virtuális gépek:

-   Futtasson, vagy a szolgáltatás által definiált változó értékét állíthatja be **$TargetLowPriorityNodes**.

-   A szolgáltatás által definiált változó értékének kaphat **$CurrentLowPriorityNodes**.

-   A szolgáltatás által definiált változó értékének kaphat **$PreemptedNodeCount**. 
    Ez a változó preempted állapotban csomópontok számát adja vissza, és lehetővé teszi a méretezési felfelé vagy lefelé a dedikált csomópontok, attól függően, hogy nem használható a leállított csomópontok száma száma.

## <a name="jobs-and-tasks"></a>Feladatok és feladatok

Feladatok és a feladatok csekély-támogatásra van szüksége az alacsony prioritású csomópont; a csak támogatása a következőképpen történik:

-   Egy feladat JobManagerTask tulajdonságnak egy új tulajdonság **AllowLowPriorityNode**. 
    Ez a tulajdonság értéke igaz, ha a kezelő feladat egy dedikált vagy alacsony prioritású csomópont is ütemezhető. Ez a tulajdonság értéke HAMIS, ha a kezelő feladat ütemezi a csak egy dedikált csomópontra.

-   Egy [környezeti változó](batch-compute-node-environment-variables.md) érhető el egy feladat alkalmazást, hogy megállapíthassa, hogy egy alacsony prioritású vagy dedikált csomóponton futó. A környezeti változó AZ_BATCH_NODE_IS_DEDICATED.

## <a name="handling-preemption"></a>A megelőlegezés kezelése

Virtuális gépek alkalmanként foglalható; Ha ez történik, a Batch a következőket teszi:

-   A preempted virtuális gépek rendelkezik a frissített állapotukra **Prioritás miatt kihagyva**.
-   Ha a csomópont a leállított virtuális gépeken futó feladatokat, majd ezeket a feladatokat helyezte és futtassa újból.
-   A virtuális gép hatékonyan törölték, ami a virtuális gép elvesztését helyileg tárolt összes adatot.
-   A készlet folyamatosan kísérletet tesz az alacsony prioritású csomópontok elérhető cél számát. Ha helyettesítő kapacitás tartalmaz, a csomópontok tartani az azonosítók, de újbóli inicializálása, keresztül haladó **létrehozása** és **indítása** előtt a feladat ütemezés szerint.
-   A megelőlegezés számát, az Azure portálon metrika érhetők el.

## <a name="metrics"></a>Mérőszámok

Új mérőszámok érhetők el a [Azure-portálon](https://portal.azure.com) az alacsony prioritású csomópont. Ezen adatok gyűjtése le van:

- Alacsony prioritású csomópontok száma
- Alacsony prioritású magok száma 
- Csomópont Count részesítés

Metrikák megtekintése az Azure-portálon:

1. Keresse meg a Batch-fiók a portálon, és a Batch-fiók beállításainak megtekintéséhez.
2. Válassza ki **metrikák** a a **figyelés** szakasz.
3. Válassza ki a vár a metrikák a **elérhető** listája.

![Alacsony prioritású csomópontok metrikák](media/batch-low-pri-vms/low-pri-metrics.png)

## <a name="next-steps"></a>Következő lépések

* Olvassa el [Az Azure Batch funkcióinak áttekintése](batch-api-basics.md) című témakört, amely hasznos információkkal szolgál a Batch használatára készülőknek. A cikk a Batch szolgáltatás erőforrásainak, például a készleteknek, csomópontoknak, feladatoknak, tevékenységeknek és sok olyan API funkciónak a részletesebb információit tartalmazza, amelyeket a Batch-alkalmazás kiépítésekor használhat.
* Megismerheti a Batch-megoldások fejlesztéséhez rendelkezésre álló [Batch API-kat és eszközöket](batch-apis-tools.md).
