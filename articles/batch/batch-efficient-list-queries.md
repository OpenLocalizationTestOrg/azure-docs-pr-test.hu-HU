---
title: "aaaDesign hatékony lista lekérdezések - Azure Batch |} Microsoft Docs"
description: "Teljesítmény növeléséhez a lekérdezések szűrést, ha a kért információ kötegelt erőforrásokhoz, mint a gyűjtők, feladatok, feladatok, és a számítási csomópontok."
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
ms.assetid: 031fefeb-248e-4d5a-9bc2-f07e46ddd30d
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 08/02/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b7e554119ec9d0e9e8007ccfb1ca80fe142a5e27
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-queries-toolist-batch-resources-efficiently"></a>Lekérdezések létrehozása toolist kötegelt erőforrások hatékony

Itt megtudhatja, hogyan tooincrease hello adatmennyiség hello szolgáltatás által visszaadott feladatok, előfordulhat, hogy csökkenti a Azure Batch-alkalmazások teljesítményének feladatok, és a számítási csomópontokat a hello [Batch .NET] [ api_net] könyvtárban.

Szinte minden Batch-alkalmazások kell tooperform bizonyos típusú figyelési vagy más műveletet lekérdező hello Batch szolgáltatás, gyakran rendszeres időközönként. Toodetermine például, hogy van-e minden fennmaradó feladatokban aszinkron feladatot, ha előbb telepítik azokra adatok hello feladat minden tevékenység. csomópontok a készlet toodetermine hello állapotát, ha előbb telepítik azokra adatok hello készlet minden egyes csomóponton. Ez a cikk azt ismerteti, hogyan tooexecute ilyen alkalmazás lekérdezései hello lehető leghatékonyabb módon.

> [!NOTE]
> hello Batch szolgáltatás hello közös forgatókönyvhöz egy feladatot a feladatok számbavételi különleges API-támogatást biztosít. Ezek a lista lekérdezés helyett hello hívása [beolvasása feladat száma] [ rest_get_task_counts] műveletet. Get feladat száma azt jelzi, hány feladatok várakoznak, fut, vagy végezze el, és hány feladatok sikeres vagy sikertelen volt. Get-feladat száma hatékonyabb, mint egy listájának lekérdezéséhez. További információkért lásd: [(előzetes verzió) állapota alapján feladat feladatok száma](batch-get-task-counts.md). 
>
> hello beolvasása feladat száma művelet nem érhető el a Batch szolgáltatás verziókban 2017-06-01.5.1 verziójánál. Ha hello szolgáltatást egy régebbi verzióját használja, majd használja egy lekérdezés toocount tevékenységei feladatokban.
>
> 

## <a name="meet-hello-detaillevel"></a>A detaillevel paraméternek hello felel meg
Éles kötegelt alkalmazás entitások, például a feladatok, a feladatok és a számítási csomópontok hello több ezer száma is. Amikor ezeket az erőforrásokat információkat kér le, potenciálisan nagy mennyiségű adatot kell "kereszt-hello vezetékes" hello Batch szolgáltatás tooyour alkalmazásból a lekérdezésekre. Ha korlátozza az elemek hello száma és típusa, amely egy lekérdezés által visszaadott, a lekérdezések hello sebesség növelése, és ezért hello az alkalmazás teljesítményét.

Ez [Batch .NET] [ api_net] API kód részlet listák *minden* , amelyhez társítva van egy feladatot, valamint a feladat *összes* egyes hello tulajdonságai feladat:

```csharp
// Get a collection of all of hello tasks and all of their properties for job-001
IPagedEnumerable<CloudTask> allTasks =
    batchClient.JobOperations.ListTasks("job-001");
```

Végezhet egy sokkal hatékonyabb listalekérdezés azonban "részletességi szint" tooyour lekérdezés alkalmazásával. Ehhez a parancskimenetnél egy [ODATADetailLevel] [ odata] toohello objektum [JobOperations.ListTasks] [ net_list_tasks] metódust. Ezt a kódrészletet adja vissza, csak a hello Azonosítót, a parancssor és a számítási csomópont információk befejezett feladatokhoz:

```csharp
// Configure an ODATADetailLevel specifying a subset of tasks and
// their properties tooreturn
ODATADetailLevel detailLevel = new ODATADetailLevel();
detailLevel.FilterClause = "state eq 'completed'";
detailLevel.SelectClause = "id,commandLine,nodeInfo";

// Supply hello ODATADetailLevel toohello ListTasks method
IPagedEnumerable<CloudTask> completedTasks =
    batchClient.JobOperations.ListTasks("job-001", detailLevel);
```

Ebben a példaforgatókönyvben a több ezer hello feladat, a feladatok esetén hello lekérdezés eredményeként előálló hello második lesz először sokkal gyorsabb, mint hello vissza. Amikor hello Batch .NET API elemek felsorolja ODATADetailLevel használatáról további információkat is megtalálható [alatt](#efficient-querying-in-batch-net).

> [!IMPORTANT]
> Erősen ajánlott, hogy *mindig* szállítási ODATADetailLevel tooyour .NET API objektumlista meghívja a tooensure a maximális hatékonyság és az alkalmazás teljesítményét. A részletességi szint megadásával segíthet a toolower a Batch szolgáltatás válaszidejét, hálózathasználat javításához, illetve minimálisra csökkenthető a memóriahasználat ügyfélalkalmazások.
> 
> 

## <a name="filter-select-and-expand"></a>Szűrés, válassza ki, és bontsa ki a
Hello [Batch .NET] [ api_net] és [Batch REST] [ api_rest] API-k olyan hello képességét tooreduce mindkét hello elemek száma, amelyek a listáját, és vissza valamint az egyes visszaküldött adatmennyiség hello. Megadásával ehhez **szűrő**, **válasszon**, és **bontsa ki a karakterláncok** lista lekérdezések végrehajtása során.

### <a name="filter"></a>Szűrés
hello szűrési karakterláncot, amely csökkenti a hello elemek száma, amelyek a rendszer visszairányítja kifejezés. Például lista csak hello futó feladatok egy feladatot, vagy a lista csak számítási csomópontokat, amelyek készen toorun feladatok.

* hello szűrési karakterláncot tartalmaz egy vagy több kifejezést, egy kifejezés, amely egy tulajdonság neve, a kezelő és a érték áll. hello tulajdonságok adhatók meg adott tooeach entitástípus lekérdező, amelyeket hello operátorok mindegyik tulajdonság támogatott vannak.
* Hello logikai operátorok használatával több kifejezések egyesíthetők `and` és `or`.
* Ez a példa szűrőlista karakterlánc csak a futó hello "leképezési" feladatok: `(state eq 'running') and startswith(id, 'renderTask')`.

### <a name="select"></a>Válassza ezt:
hello válassza karakterláncot adja vissza minden elemhez hello tulajdonságértékeket korlátozza. A tulajdonságnevek listáját adja meg, és csak azokat a tulajdonságértékek hello lekérdezés eredményében hello elemeket adja vissza.

* hello válassza karakterlánc áll tulajdonságnevek vesszővel tagolt listája. Megadhatja a hello tulajdonságok hello entitástípus kérdez le.
* A példában válassza karakterlánc határozza meg, hogy a csak három tulajdonságértékek vissza kell-e az egyes feladatok: `id, state, stateTransitionTime`.

### <a name="expand"></a>Kibontás
hello bontsa ki a karakterláncot, amelyek a szükséges tooobtain API-hívások száma hello bizonyos adatok csökkenti. Egy bővített karakterlánc használata esetén további információt az egyes elemek érhető el egyetlen API-hívással. Ahelyett, hogy első beszerzését hello listájáért entitások, majd a kérést küldő adatai hello lista minden eleme egy kibontott karakterlánc használatához tooobtain hello egyetlen API-hívásban ugyanazokat az információkat. Kevesebb az API-hívásokban azt jelenti, hogy a jobb teljesítmény érdekében.

* Hasonló toohello válassza karakterlánc, hello bontsa ki a karakterlánc vezérlők hogy bizonyos adatok a lista lekérdezés eredményei között szerepel-e.
* hello bontsa ki a karakterlánc csak akkor támogatott, ha a feladatokat, a feladatütemezéseket, a feladatok és a készletek listázása használatban van. Jelenleg csak a támogatott statisztikai adatok.
* Ha az összes tulajdonság szükség, és nincs select karakterlánc lett megadva, a hello bontsa ki a karakterlánc *kell* használt tooget statisztikai adatok lehet. Ha egy select karakterlánca használt tooobtain egy részhalmazát tulajdonságok, majd `stats` hello válassza karakterlánc adható meg, és hello bontsa ki a karakterlánc nem kell a megadott toobe.
* Ez a példa bontsa ki a karakterláncot határozza meg, hogy a statisztikai adatok vissza kell-e a hello lista minden eleme: `stats`.

> [!NOTE]
> Bármelyik hello térített három lekérdezési karakterlánc típusú (szűrés, válassza ki, és bontsa ki a), meg kell győződnie arról, hogy hello tulajdonságnevek és esetben egyezik, mint a REST API elemet. Például, amikor olyan hello .NET [CloudTask](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask) osztály, meg kell adnia **állapot** helyett **állapot**, annak ellenére, hogy hello .NET tulajdonság [ CloudTask.State](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.state). Lásd az alábbi táblázatokban hello tulajdonságleképezései hello .NET és REST API-k között.
> 
> 

### <a name="rules-for-filter-select-and-expand-strings"></a>Szabályok szűrő, válassza ki, és bontsa ki a karakterláncok
* Tulajdonságok nevek szűrő, válassza ki, és bontsa ki a karakterláncok megjelenjen-e, mint a hello [Batch REST] [ api_rest] API – még akkor is, ha használ [Batch .NET] [ api_net] vagy az egyik kötegelt Csomagjától hello.
* Minden tulajdonságnevek megkülönböztetik a kis-és nagybetűket, de a tulajdonságértékek-és nagybetűket.
* Dátum és idő karakterláncok lehet két formátumok egyikét, és utasítás előtt szerepelnie kell a `DateTime`.
  
  * W3C-DTF formátum példa:`creationTime gt DateTime'2011-05-08T08:49:37Z'`
  * RFC 1123 formátum példa:`creationTime gt DateTime'Sun, 08 May 2011 08:49:37 GMT'`
* Logikai értékek a következők: vagy `true` vagy `false`.
* Ha egy tulajdonság érvénytelen vagy operátor van megadva, a `400 (Bad Request)` hibát fog okozni.

## <a name="efficient-querying-in-batch-net"></a>Hatékony, a Batch .NET lekérdezése
Hello belül [Batch .NET] [ api_net] API, hello [ODATADetailLevel] [ odata] osztály szűrő ellátására szolgál, válassza ki, és bontsa ki a karakterláncok toolist műveletek. hello ODataDetailLevel osztály tulajdonságai három nyilvános karakterlánc, amely meg hello a konstruktorban, vagy közvetlenül hello objektumra beállítva. Majd át hello ODataDetailLevel objektum mint egy paraméterrel toohello különböző listázási műveletei például [ListPools][net_list_pools], [ListJobs][net_list_jobs], és [ListTasks][net_list_tasks].

* [ODATADetailLevel][odata].[ FilterClause][odata_filter]: hello eredmények számát.
* [ODATADetailLevel][odata].[ SelectClause][odata_select]: Adja meg a tulajdonságértékek egyes elemek küld vissza a rendszer.
* [ODATADetailLevel][odata].[ ExpandClause][odata_expand]: olvashatók be adatokat az összes egy API hívása helyett külön hívások minden elemhez.

hello következő kódrészletet használnak hello Batch .NET API tooefficiently lekérdezés hello kötegelt szolgáltatást egy adott készletét készletek hello statisztikáját. Ebben a forgatókönyvben hello kötegelt felhasználói teszt- és éles címkészletekkel rendelkezik. hello teszt készlet azonosítók fűzve előtagként a "test", és hello termelési gyűjtő azonosítók fűzve előtagként a "termék". A hello részlet *myBatchClient* hello megfelelően inicializálva példánya [BatchClient](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient) osztály.

```csharp
// First we need an ODATADetailLevel instance on which tooset hello filter, select,
// and expand clause strings
ODATADetailLevel detailLevel = new ODATADetailLevel();

// We want toopull only hello "test" pools, so we limit hello number of items returned
// by using a FilterClause and specifying that hello pool IDs must start with "test"
detailLevel.FilterClause = "startswith(id, 'test')";

// toofurther limit hello data that crosses hello wire, configure hello SelectClause to
// limit hello properties that are returned on each CloudPool object tooonly
// CloudPool.Id and CloudPool.Statistics
detailLevel.SelectClause = "id, stats";

// Specify hello ExpandClause so that hello .NET API pulls hello statistics for the
// CloudPools in a single underlying REST API call. Note that we use hello pool's
// REST API element name "stats" here as opposed too"Statistics" as it appears in
// hello .NET API (CloudPool.Statistics)
detailLevel.ExpandClause = "stats";

// Now get our collection of pools, minimizing hello amount of data that is returned
// by specifying hello detail level that we configured above
List<CloudPool> testPools =
    await myBatchClient.PoolOperations.ListPools(detailLevel).ToListAsync();
```

> [!TIP]
> Példányának [ODATADetailLevel] [ odata] válassza ki a konfigurált és kibontott záradék is átadhatók tooappropriate Get módszerek, például a [PoolOperations.GetPool](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.getpool.aspx) , toolimit hello mennyisége visszaadott adatokat.
> 
> 

## <a name="batch-rest-toonet-api-mappings"></a>A Batch REST API-too.NET hozzárendelések
Tulajdonságnevek szűrő, válassza ki, és bontsa ki a karakterláncok *kell* REST API mint a, mind a név és esetben tükrözi. az alábbi táblázatok hello hello .NET és REST API megfelelők esetében közötti hozzárendelések adja meg.

### <a name="mappings-for-filter-strings"></a>Azon szűrőkarakterláncokban
* **.NET-lista módszerek**: hello .NET API-módszer ebben az oszlopban fogad el egy [ODATADetailLevel] [ odata] objektum paraméterként.
* **REST kérelmek száma**: minden REST API lapon ez az oszlop, amelyben hello tulajdonságok és műveletek táblát tartalmaz, amelyek számára engedélyezett a csatolt tooin *szűrő* karakterláncok. Használhatja a tulajdonság nevét és a műveletek hoz egy [ODATADetailLevel.FilterClause] [ odata_filter] karakterlánc.

| .NET-lista módszerek | REST kérelmek száma |
| --- | --- |
| [CertificateOperations.ListCertificates][net_list_certs] |[Egy fiók hello-tanúsítványok listázása][rest_list_certs] |
| [CloudTask.ListNodeFiles][net_list_task_files] |[A feladathoz társított hello fájlok listázása][rest_list_task_files] |
| [JobOperations.ListJobPreparationAndReleaseTaskStatus][net_list_jobprep_status] |[Hello hello feladat előkészítése és a feladat kiadása feladatok feladat állapota][rest_list_jobprep_status] |
| [JobOperations.ListJobs][net_list_jobs] |[A fiók lista hello feladatok][rest_list_jobs] |
| [JobOperations.ListNodeFiles][net_list_nodefiles] |[A csomópont hello fájlok listázása][rest_list_nodefiles] |
| [JobOperations.ListTasks][net_list_tasks] |[A feladathoz társított hello tevékenységei][rest_list_tasks] |
| [JobScheduleOperations.ListJobSchedules][net_list_job_schedules] |[Lista hello feladatütemezéseket egy fiók][rest_list_job_schedules] |
| [JobScheduleOperations.ListJobs][net_list_schedule_jobs] |[A feladatütemezés társított lista hello feladatok][rest_list_schedule_jobs] |
| [PoolOperations.ListComputeNodes][net_list_compute_nodes] |[Lista hello számítási csomópontjain a készletben][rest_list_compute_nodes] |
| [PoolOperations.ListPools][net_list_pools] |[Egy fiók lista hello készletek][rest_list_pools] |

### <a name="mappings-for-select-strings"></a>Jelölje be karakterláncok leképezéseit
* **A Batch .NET típusok**: Batch .NET API-típusok.
* **REST API entitások**: Ebben az oszlopban lévő tartalmaz egy vagy több táblában, amely a REST API tulajdonságnevek hello hello típus listából. A tulajdonságnevek hoz használt *válasszon* karakterláncok. Ezek azonos tulajdonságnevek fogja használni, amikor, hozhat létre egy [ODATADetailLevel.SelectClause] [ odata_select] karakterlánc.

| Batch .NET típusok | REST API entitások |
| --- | --- |
| [Tanúsítvány][net_cert] |[A tanúsítvány adatainak beolvasása][rest_get_cert] |
| [CloudJob][net_job] |[A feladat adatainak beolvasása][rest_get_job] |
| [CloudJobSchedule][net_schedule] |[A feladatütemezés adatainak beolvasása][rest_get_schedule] |
| [Átjárócsomópontján][net_node] |[A csomópont adatainak beolvasása][rest_get_node] |
| [CloudPool][net_pool] |[Egy készlet adatainak beolvasása][rest_get_pool] |
| [CloudTask][net_task] |[A feladat adatainak beolvasása][rest_get_task] |

## <a name="example-construct-a-filter-string"></a>Példa: egy szűrési karakterláncot létrehozni
Ha, hozhat létre egy szűrési karakterláncot a [ODATADetailLevel.FilterClause][odata_filter], tekintse át a hello "Szűrőkarakterláncokban leképezéseit" toofind hello REST API dokumentációja lap, amely megfelel a fenti táblázatban toohello list művelet, hogy kívánja-e tooperform. Hello szűrhető tulajdonságok és azok támogatott operátort hello többsoros első tábla adott oldalon találja. Ha a tooretrieve minden olyan feladat, amelynek kilépési kód: nem nulla, például a sor: a [a feladathoz társított hello tevékenységeinek] [ rest_list_tasks] hello megfelelő tulajdonság karakterlánc és engedélyezett operátorok adja meg:

| Tulajdonság | Engedélyezett műveletek | Típus |
|:--- |:--- |:--- |
| `executionInfo/exitCode` |`eq, ge, gt, le , lt` |`Int` |

Hello szűrési karakterláncot egy nem nulla kilépési kód minden feladat felsoroló így néz ki:

`(executionInfo/exitCode lt 0) or (executionInfo/exitCode gt 0)`

## <a name="example-construct-a-select-string"></a>Például: select karakterlánc összeállításához
tooconstruct [ODATADetailLevel.SelectClause][odata_select], tekintse át a hello "Select karakterláncok leképezéseit" a fenti táblázatban, és keresse meg a megfelelő toohello típusú entitás toohello REST API lapján, amikor listázásakor. Hello választható tulajdonságok és azok támogatott operátort hello többsoros első tábla adott oldalon találja. Ha tooretrieve csak hello azonosítója és minden feladat parancssori listájában, például találja a sorokra alkalmazandó tábla hello [feladat adatainak beolvasása][rest_get_task]:

| Tulajdonság | Típus | Megjegyzések |
|:--- |:--- |:--- |
| `id` |`String` |`hello ID of hello task.` |
| `commandLine` |`String` |`hello command line of hello task.` |

hello válassza karakterláncot például csak hello azonosítója és minden felsorolt feladat-parancssorból lesz:

`id, commandLine`

## <a name="code-samples"></a>Kódminták
### <a name="efficient-list-queries-code-sample"></a>Hatékony lista lekérdezések kódminta
Tekintse meg a hello [EfficientListQueries] [ efficient_query_sample] mintaprojektet a Githubon toosee hogyan hatékony lista lekérdezése befolyásolhatja a teljesítményt az alkalmazásban. A C# Konzolalkalmazás hoz létre, és hozzáadja a nagyszámú feladatok tooa feladat. Majd, így több hívások toohello [JobOperations.ListTasks] [ net_list_tasks] metódus és fázisok [ODATADetailLevel] [ odata] -objektumok a másik tulajdonságot értékek toovary hello méretű visszaadott adatok toobe konfigurálva. Kimeneti hasonló toohello következő képes:

```
Adding 5000 tasks toojob jobEffQuery...
5000 tasks added in 00:00:47.3467587, hit ENTER tooquery tasks...

4943 tasks retrieved in 00:00:04.3408081 (ExpandClause:  | FilterClause: state eq 'active' | SelectClause: id,state)
0 tasks retrieved in 00:00:00.2662920 (ExpandClause:  | FilterClause: state eq 'running' | SelectClause: id,state)
59 tasks retrieved in 00:00:00.3337760 (ExpandClause:  | FilterClause: state eq 'completed' | SelectClause: id,state)
5000 tasks retrieved in 00:00:04.1429881 (ExpandClause:  | FilterClause:  | SelectClause: id,state)
5000 tasks retrieved in 00:00:15.1016127 (ExpandClause:  | FilterClause:  | SelectClause: id,state,environmentSettings)
5000 tasks retrieved in 00:00:17.0548145 (ExpandClause: stats | FilterClause:  | SelectClause: )

Sample complete, hit ENTER toocontinue...
```

Amint hello eltelt idő, nagy mértékben csökkentheti lekérdezési válaszidőt hello tulajdonságok és hello elemek száma, amelyek a rendszer visszairányítja korlátozásával. Ez, és más mintaprojektjeit az található hello [azure-köteg-minták] [ github_samples] GitHub tárházából.

### <a name="batchmetrics-library-and-code-sample"></a>BatchMetrics és a kód a minta
Ezenkívül toohello EfficientListQueries kódmintában fenti található hello [BatchMetrics] [ batch_metrics] projektre a hello [azure-köteg-minták] [ github_samples] GitHub-tárházban. hello BatchMetrics mintaprojektet bemutatja, hogyan tooefficiently figyelése Azure kötegelt feladatok előrehaladásának hello kötegelt API használatával.

Hello [BatchMetrics] [ batch_metrics] minta tartalmazza a .NET hordozhatóosztálytár-projektjének amely beépítheti saját projektek és egy egyszerű parancssori tooexercise programot, és mutassa be hello hello használata könyvtár.

hello mintaalkalmazás hello projektben mutatja be a következő műveletek hello:

1. Adott attribútumok kiválasztása rendelés toodownload csak hello tulajdonságainál kell
2. Szűrés állapot átmenet alkalommal rendelés toodownload csak akkor változik meg hello legutóbbi lekérdezés óta

Például hello BatchMetrics könyvtár a következő metódus hello jelenik meg. Azt adja vissza, amely meghatározza, hogy csak hello egy ODATADetailLevel `id` és `state` tulajdonságait a rendszer megkérdezi a hello entitások beszerzése. Azt is, amely csak entitások, amelynek állapota módosult az megadott hello `DateTime` vissza kell adni a paraméter.

```csharp
internal static ODATADetailLevel OnlyChangedAfter(DateTime time)
{
    return new ODATADetailLevel(
        selectClause: "id, state",
        filterClause: string.Format("stateTransitionTime gt DateTime'{0:o}'", time)
    );
}
```

## <a name="next-steps"></a>Következő lépések
### <a name="parallel-node-tasks"></a>Párhuzamos csomópont feladatok
[Azure Batch számítási erőforrás-használat csomópont egyidejű feladatok maximális](batch-parallel-node-tasks.md) van egy másik cikkben tooBatch alkalmazásteljesítmény kapcsolatos. Bizonyos típusú munkaterheléseket is kihasználhatja a párhuzamos tevékenységek feldolgozás alatt álló nagyobb – de--kevesebb számítási csomópontot. Tekintse meg a hello [példa](batch-parallel-node-tasks.md#example-scenario) hello cikkben talál részletes információt ilyen esetben.

### <a name="batch-forum"></a>Batch fórum
Hello [Azure Batch fórum] [ forum] MSDN nagyszerű toodiscuss kötegelt helyezze, és hello szolgáltatás kérdése. Központi a keresztül a hasznos "kapcsolódó" bejegyzések, és a kötegelt megoldások létrehozása során felmerülő kérdéseit.

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_listjobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_metrics]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchMetrics
[efficient_query_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/EfficientListQueries
[forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azurebatch
[github_samples]: https://github.com/Azure/azure-batch-samples
[odata]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.aspx
[odata_ctor]: https://msdn.microsoft.com/library/azure/dn866178.aspx
[odata_expand]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.expandclause.aspx
[odata_filter]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.filterclause.aspx
[odata_select]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.selectclause.aspx

[net_list_certs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificateoperations.listcertificates.aspx
[net_list_compute_nodes]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listcomputenodes.aspx
[net_list_job_schedules]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobscheduleoperations.listjobschedules.aspx
[net_list_jobprep_status]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobpreparationandreleasetaskstatus.aspx
[net_list_jobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[net_list_nodefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listnodefiles.aspx
[net_list_pools]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listpools.aspx
[net_list_schedule_jobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobscheduleoperations.listjobs.aspx
[net_list_task_files]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listnodefiles.aspx
[net_list_tasks]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listtasks.aspx

[rest_list_certs]: https://msdn.microsoft.com/library/azure/dn820154.aspx
[rest_list_compute_nodes]: https://msdn.microsoft.com/library/azure/dn820159.aspx
[rest_list_job_schedules]: https://msdn.microsoft.com/library/azure/mt282174.aspx
[rest_list_jobprep_status]: https://msdn.microsoft.com/library/azure/mt282170.aspx
[rest_list_jobs]: https://msdn.microsoft.com/library/azure/dn820117.aspx
[rest_list_nodefiles]: https://msdn.microsoft.com/library/azure/dn820151.aspx
[rest_list_pools]: https://msdn.microsoft.com/library/azure/dn820101.aspx
[rest_list_schedule_jobs]: https://msdn.microsoft.com/library/azure/mt282169.aspx
[rest_list_task_files]: https://msdn.microsoft.com/library/azure/dn820142.aspx
[rest_list_tasks]: https://msdn.microsoft.com/library/azure/dn820187.aspx

[rest_get_cert]: https://msdn.microsoft.com/library/azure/dn820176.aspx
[rest_get_job]: https://msdn.microsoft.com/library/azure/dn820106.aspx
[rest_get_node]: https://msdn.microsoft.com/library/azure/dn820168.aspx
[rest_get_pool]: https://msdn.microsoft.com/library/azure/dn820165.aspx
[rest_get_schedule]: https://msdn.microsoft.com/library/azure/mt282171.aspx
[rest_get_task]: https://msdn.microsoft.com/library/azure/dn820133.aspx

[net_cert]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificate.aspx
[net_job]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_node]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.aspx
[net_pool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_schedule]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjobschedule.aspx
[net_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx

[rest_get_task_counts]: https://docs.microsoft.com/rest/api/batchservice/get-the-task-counts-for-a-job