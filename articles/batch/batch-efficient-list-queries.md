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
# <a name="create-queries-toolist-batch-resources-efficiently"></a><span data-ttu-id="2829b-103">Lekérdezések létrehozása toolist kötegelt erőforrások hatékony</span><span class="sxs-lookup"><span data-stu-id="2829b-103">Create queries toolist Batch resources efficiently</span></span>

<span data-ttu-id="2829b-104">Itt megtudhatja, hogyan tooincrease hello adatmennyiség hello szolgáltatás által visszaadott feladatok, előfordulhat, hogy csökkenti a Azure Batch-alkalmazások teljesítményének feladatok, és a számítási csomópontokat a hello [Batch .NET] [ api_net] könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="2829b-104">Here you'll learn how tooincrease your Azure Batch application's performance by reducing hello amount of data that is returned by hello service when you query jobs, tasks, and compute nodes with hello [Batch .NET][api_net] library.</span></span>

<span data-ttu-id="2829b-105">Szinte minden Batch-alkalmazások kell tooperform bizonyos típusú figyelési vagy más műveletet lekérdező hello Batch szolgáltatás, gyakran rendszeres időközönként.</span><span class="sxs-lookup"><span data-stu-id="2829b-105">Nearly all Batch applications need tooperform some type of monitoring or other operation that queries hello Batch service, often at regular intervals.</span></span> <span data-ttu-id="2829b-106">Toodetermine például, hogy van-e minden fennmaradó feladatokban aszinkron feladatot, ha előbb telepítik azokra adatok hello feladat minden tevékenység.</span><span class="sxs-lookup"><span data-stu-id="2829b-106">For example, toodetermine whether there are any queued tasks remaining in a job, you must get data on every task in hello job.</span></span> <span data-ttu-id="2829b-107">csomópontok a készlet toodetermine hello állapotát, ha előbb telepítik azokra adatok hello készlet minden egyes csomóponton.</span><span class="sxs-lookup"><span data-stu-id="2829b-107">toodetermine hello status of nodes in your pool, you must get data on every node in hello pool.</span></span> <span data-ttu-id="2829b-108">Ez a cikk azt ismerteti, hogyan tooexecute ilyen alkalmazás lekérdezései hello lehető leghatékonyabb módon.</span><span class="sxs-lookup"><span data-stu-id="2829b-108">This article explains how tooexecute such queries in hello most efficient way.</span></span>

> [!NOTE]
> <span data-ttu-id="2829b-109">hello Batch szolgáltatás hello közös forgatókönyvhöz egy feladatot a feladatok számbavételi különleges API-támogatást biztosít.</span><span class="sxs-lookup"><span data-stu-id="2829b-109">hello Batch service provides special API support for hello common scenario of counting tasks in a job.</span></span> <span data-ttu-id="2829b-110">Ezek a lista lekérdezés helyett hello hívása [beolvasása feladat száma] [ rest_get_task_counts] műveletet.</span><span class="sxs-lookup"><span data-stu-id="2829b-110">Instead of using a list query for these, you can call hello [Get Task Counts][rest_get_task_counts] operation.</span></span> <span data-ttu-id="2829b-111">Get feladat száma azt jelzi, hány feladatok várakoznak, fut, vagy végezze el, és hány feladatok sikeres vagy sikertelen volt.</span><span class="sxs-lookup"><span data-stu-id="2829b-111">Get Task Counts indicates how many tasks are pending, running or complete, and how many tasks have succeeded or failed.</span></span> <span data-ttu-id="2829b-112">Get-feladat száma hatékonyabb, mint egy listájának lekérdezéséhez.</span><span class="sxs-lookup"><span data-stu-id="2829b-112">Get Task Counts is more efficient than a list query.</span></span> <span data-ttu-id="2829b-113">További információkért lásd: [(előzetes verzió) állapota alapján feladat feladatok száma](batch-get-task-counts.md).</span><span class="sxs-lookup"><span data-stu-id="2829b-113">For more information, see [Count tasks for a job by state (Preview)](batch-get-task-counts.md).</span></span> 
>
> <span data-ttu-id="2829b-114">hello beolvasása feladat száma művelet nem érhető el a Batch szolgáltatás verziókban 2017-06-01.5.1 verziójánál.</span><span class="sxs-lookup"><span data-stu-id="2829b-114">hello Get Task Counts operation is not available in Batch service versions earlier than 2017-06-01.5.1.</span></span> <span data-ttu-id="2829b-115">Ha hello szolgáltatást egy régebbi verzióját használja, majd használja egy lekérdezés toocount tevékenységei feladatokban.</span><span class="sxs-lookup"><span data-stu-id="2829b-115">If you are using an older version of hello service, then use a list query toocount tasks in a job instead.</span></span>
>
> 

## <a name="meet-hello-detaillevel"></a><span data-ttu-id="2829b-116">A detaillevel paraméternek hello felel meg</span><span class="sxs-lookup"><span data-stu-id="2829b-116">Meet hello DetailLevel</span></span>
<span data-ttu-id="2829b-117">Éles kötegelt alkalmazás entitások, például a feladatok, a feladatok és a számítási csomópontok hello több ezer száma is.</span><span class="sxs-lookup"><span data-stu-id="2829b-117">In a production Batch application, entities like jobs, tasks, and compute nodes can number in hello thousands.</span></span> <span data-ttu-id="2829b-118">Amikor ezeket az erőforrásokat információkat kér le, potenciálisan nagy mennyiségű adatot kell "kereszt-hello vezetékes" hello Batch szolgáltatás tooyour alkalmazásból a lekérdezésekre.</span><span class="sxs-lookup"><span data-stu-id="2829b-118">When you request information on these resources, a potentially large amount of data must "cross hello wire" from hello Batch service tooyour application on each query.</span></span> <span data-ttu-id="2829b-119">Ha korlátozza az elemek hello száma és típusa, amely egy lekérdezés által visszaadott, a lekérdezések hello sebesség növelése, és ezért hello az alkalmazás teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="2829b-119">By limiting hello number of items and type of information that is returned by a query, you can increase hello speed of your queries, and therefore hello performance of your application.</span></span>

<span data-ttu-id="2829b-120">Ez [Batch .NET] [ api_net] API kód részlet listák *minden* , amelyhez társítva van egy feladatot, valamint a feladat *összes* egyes hello tulajdonságai feladat:</span><span class="sxs-lookup"><span data-stu-id="2829b-120">This [Batch .NET][api_net] API code snippet lists *every* task that is associated with a job, along with *all* of hello properties of each task:</span></span>

```csharp
// Get a collection of all of hello tasks and all of their properties for job-001
IPagedEnumerable<CloudTask> allTasks =
    batchClient.JobOperations.ListTasks("job-001");
```

<span data-ttu-id="2829b-121">Végezhet egy sokkal hatékonyabb listalekérdezés azonban "részletességi szint" tooyour lekérdezés alkalmazásával.</span><span class="sxs-lookup"><span data-stu-id="2829b-121">You can perform a much more efficient list query, however, by applying a "detail level" tooyour query.</span></span> <span data-ttu-id="2829b-122">Ehhez a parancskimenetnél egy [ODATADetailLevel] [ odata] toohello objektum [JobOperations.ListTasks] [ net_list_tasks] metódust.</span><span class="sxs-lookup"><span data-stu-id="2829b-122">You do this by supplying an [ODATADetailLevel][odata] object toohello [JobOperations.ListTasks][net_list_tasks] method.</span></span> <span data-ttu-id="2829b-123">Ezt a kódrészletet adja vissza, csak a hello Azonosítót, a parancssor és a számítási csomópont információk befejezett feladatokhoz:</span><span class="sxs-lookup"><span data-stu-id="2829b-123">This snippet returns only hello ID, command line, and compute node information properties of completed tasks:</span></span>

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

<span data-ttu-id="2829b-124">Ebben a példaforgatókönyvben a több ezer hello feladat, a feladatok esetén hello lekérdezés eredményeként előálló hello második lesz először sokkal gyorsabb, mint hello vissza.</span><span class="sxs-lookup"><span data-stu-id="2829b-124">In this example scenario, if there are thousands of tasks in hello job, hello results from hello second query will typically be returned much quicker than hello first.</span></span> <span data-ttu-id="2829b-125">Amikor hello Batch .NET API elemek felsorolja ODATADetailLevel használatáról további információkat is megtalálható [alatt](#efficient-querying-in-batch-net).</span><span class="sxs-lookup"><span data-stu-id="2829b-125">More information about using ODATADetailLevel when you list items with hello Batch .NET API is included [below](#efficient-querying-in-batch-net).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2829b-126">Erősen ajánlott, hogy *mindig* szállítási ODATADetailLevel tooyour .NET API objektumlista meghívja a tooensure a maximális hatékonyság és az alkalmazás teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="2829b-126">We highly recommend that you *always* supply an ODATADetailLevel object tooyour .NET API list calls tooensure maximum efficiency and performance of your application.</span></span> <span data-ttu-id="2829b-127">A részletességi szint megadásával segíthet a toolower a Batch szolgáltatás válaszidejét, hálózathasználat javításához, illetve minimálisra csökkenthető a memóriahasználat ügyfélalkalmazások.</span><span class="sxs-lookup"><span data-stu-id="2829b-127">By specifying a detail level, you can help toolower Batch service response times, improve network utilization, and minimize memory usage by client applications.</span></span>
> 
> 

## <a name="filter-select-and-expand"></a><span data-ttu-id="2829b-128">Szűrés, válassza ki, és bontsa ki a</span><span class="sxs-lookup"><span data-stu-id="2829b-128">Filter, select, and expand</span></span>
<span data-ttu-id="2829b-129">Hello [Batch .NET] [ api_net] és [Batch REST] [ api_rest] API-k olyan hello képességét tooreduce mindkét hello elemek száma, amelyek a listáját, és vissza valamint az egyes visszaküldött adatmennyiség hello.</span><span class="sxs-lookup"><span data-stu-id="2829b-129">hello [Batch .NET][api_net] and [Batch REST][api_rest] APIs provide hello ability tooreduce both hello number of items that are returned in a list, as well as hello amount of information that is returned for each.</span></span> <span data-ttu-id="2829b-130">Megadásával ehhez **szűrő**, **válasszon**, és **bontsa ki a karakterláncok** lista lekérdezések végrehajtása során.</span><span class="sxs-lookup"><span data-stu-id="2829b-130">You do so by specifying **filter**, **select**, and **expand strings** when performing list queries.</span></span>

### <a name="filter"></a><span data-ttu-id="2829b-131">Szűrés</span><span class="sxs-lookup"><span data-stu-id="2829b-131">Filter</span></span>
<span data-ttu-id="2829b-132">hello szűrési karakterláncot, amely csökkenti a hello elemek száma, amelyek a rendszer visszairányítja kifejezés.</span><span class="sxs-lookup"><span data-stu-id="2829b-132">hello filter string is an expression that reduces hello number of items that are returned.</span></span> <span data-ttu-id="2829b-133">Például lista csak hello futó feladatok egy feladatot, vagy a lista csak számítási csomópontokat, amelyek készen toorun feladatok.</span><span class="sxs-lookup"><span data-stu-id="2829b-133">For example, list only hello running tasks for a job, or list only compute nodes that are ready toorun tasks.</span></span>

* <span data-ttu-id="2829b-134">hello szűrési karakterláncot tartalmaz egy vagy több kifejezést, egy kifejezés, amely egy tulajdonság neve, a kezelő és a érték áll.</span><span class="sxs-lookup"><span data-stu-id="2829b-134">hello filter string consists of one or more expressions, with an expression that consists of a property name, operator, and value.</span></span> <span data-ttu-id="2829b-135">hello tulajdonságok adhatók meg adott tooeach entitástípus lekérdező, amelyeket hello operátorok mindegyik tulajdonság támogatott vannak.</span><span class="sxs-lookup"><span data-stu-id="2829b-135">hello properties that can be specified are specific tooeach entity type that you query, as are hello operators that are supported for each property.</span></span>
* <span data-ttu-id="2829b-136">Hello logikai operátorok használatával több kifejezések egyesíthetők `and` és `or`.</span><span class="sxs-lookup"><span data-stu-id="2829b-136">Multiple expressions can be combined by using hello logical operators `and` and `or`.</span></span>
* <span data-ttu-id="2829b-137">Ez a példa szűrőlista karakterlánc csak a futó hello "leképezési" feladatok: `(state eq 'running') and startswith(id, 'renderTask')`.</span><span class="sxs-lookup"><span data-stu-id="2829b-137">This example filter string lists only hello running "render" tasks: `(state eq 'running') and startswith(id, 'renderTask')`.</span></span>

### <a name="select"></a><span data-ttu-id="2829b-138">Válassza ezt:</span><span class="sxs-lookup"><span data-stu-id="2829b-138">Select</span></span>
<span data-ttu-id="2829b-139">hello válassza karakterláncot adja vissza minden elemhez hello tulajdonságértékeket korlátozza.</span><span class="sxs-lookup"><span data-stu-id="2829b-139">hello select string limits hello property values that are returned for each item.</span></span> <span data-ttu-id="2829b-140">A tulajdonságnevek listáját adja meg, és csak azokat a tulajdonságértékek hello lekérdezés eredményében hello elemeket adja vissza.</span><span class="sxs-lookup"><span data-stu-id="2829b-140">You specify a list of property names, and only those property values are returned for hello items in hello query results.</span></span>

* <span data-ttu-id="2829b-141">hello válassza karakterlánc áll tulajdonságnevek vesszővel tagolt listája.</span><span class="sxs-lookup"><span data-stu-id="2829b-141">hello select string consists of a comma-separated list of property names.</span></span> <span data-ttu-id="2829b-142">Megadhatja a hello tulajdonságok hello entitástípus kérdez le.</span><span class="sxs-lookup"><span data-stu-id="2829b-142">You can specify any of hello properties for hello entity type you are querying.</span></span>
* <span data-ttu-id="2829b-143">A példában válassza karakterlánc határozza meg, hogy a csak három tulajdonságértékek vissza kell-e az egyes feladatok: `id, state, stateTransitionTime`.</span><span class="sxs-lookup"><span data-stu-id="2829b-143">This example select string specifies that only three property values should be returned for each task: `id, state, stateTransitionTime`.</span></span>

### <a name="expand"></a><span data-ttu-id="2829b-144">Kibontás</span><span class="sxs-lookup"><span data-stu-id="2829b-144">Expand</span></span>
<span data-ttu-id="2829b-145">hello bontsa ki a karakterláncot, amelyek a szükséges tooobtain API-hívások száma hello bizonyos adatok csökkenti.</span><span class="sxs-lookup"><span data-stu-id="2829b-145">hello expand string reduces hello number of API calls that are required tooobtain certain information.</span></span> <span data-ttu-id="2829b-146">Egy bővített karakterlánc használata esetén további információt az egyes elemek érhető el egyetlen API-hívással.</span><span class="sxs-lookup"><span data-stu-id="2829b-146">When you use an expand string, more information about each item can be obtained with a single API call.</span></span> <span data-ttu-id="2829b-147">Ahelyett, hogy első beszerzését hello listájáért entitások, majd a kérést küldő adatai hello lista minden eleme egy kibontott karakterlánc használatához tooobtain hello egyetlen API-hívásban ugyanazokat az információkat.</span><span class="sxs-lookup"><span data-stu-id="2829b-147">Rather than first obtaining hello list of entities, then requesting information for each item in hello list, you use an expand string tooobtain hello same information in a single API call.</span></span> <span data-ttu-id="2829b-148">Kevesebb az API-hívásokban azt jelenti, hogy a jobb teljesítmény érdekében.</span><span class="sxs-lookup"><span data-stu-id="2829b-148">Less API calls means better performance.</span></span>

* <span data-ttu-id="2829b-149">Hasonló toohello válassza karakterlánc, hello bontsa ki a karakterlánc vezérlők hogy bizonyos adatok a lista lekérdezés eredményei között szerepel-e.</span><span class="sxs-lookup"><span data-stu-id="2829b-149">Similar toohello select string, hello expand string controls whether certain data is included in list query results.</span></span>
* <span data-ttu-id="2829b-150">hello bontsa ki a karakterlánc csak akkor támogatott, ha a feladatokat, a feladatütemezéseket, a feladatok és a készletek listázása használatban van.</span><span class="sxs-lookup"><span data-stu-id="2829b-150">hello expand string is only supported when it is used in listing jobs, job schedules, tasks, and pools.</span></span> <span data-ttu-id="2829b-151">Jelenleg csak a támogatott statisztikai adatok.</span><span class="sxs-lookup"><span data-stu-id="2829b-151">Currently, it only supports statistics information.</span></span>
* <span data-ttu-id="2829b-152">Ha az összes tulajdonság szükség, és nincs select karakterlánc lett megadva, a hello bontsa ki a karakterlánc *kell* használt tooget statisztikai adatok lehet.</span><span class="sxs-lookup"><span data-stu-id="2829b-152">When all properties are required and no select string is specified, hello expand string *must* be used tooget statistics information.</span></span> <span data-ttu-id="2829b-153">Ha egy select karakterlánca használt tooobtain egy részhalmazát tulajdonságok, majd `stats` hello válassza karakterlánc adható meg, és hello bontsa ki a karakterlánc nem kell a megadott toobe.</span><span class="sxs-lookup"><span data-stu-id="2829b-153">If a select string is used tooobtain a subset of properties, then `stats` can be specified in hello select string, and hello expand string does not need toobe specified.</span></span>
* <span data-ttu-id="2829b-154">Ez a példa bontsa ki a karakterláncot határozza meg, hogy a statisztikai adatok vissza kell-e a hello lista minden eleme: `stats`.</span><span class="sxs-lookup"><span data-stu-id="2829b-154">This example expand string specifies that statistics information should be returned for each item in hello list: `stats`.</span></span>

> [!NOTE]
> <span data-ttu-id="2829b-155">Bármelyik hello térített három lekérdezési karakterlánc típusú (szűrés, válassza ki, és bontsa ki a), meg kell győződnie arról, hogy hello tulajdonságnevek és esetben egyezik, mint a REST API elemet.</span><span class="sxs-lookup"><span data-stu-id="2829b-155">When constructing any of hello three query string types (filter, select, and expand), you must ensure that hello property names and case match that of their REST API element counterparts.</span></span> <span data-ttu-id="2829b-156">Például, amikor olyan hello .NET [CloudTask](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask) osztály, meg kell adnia **állapot** helyett **állapot**, annak ellenére, hogy hello .NET tulajdonság [ CloudTask.State](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.state).</span><span class="sxs-lookup"><span data-stu-id="2829b-156">For example, when working with hello .NET [CloudTask](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask) class, you must specify **state** instead of **State**, even though hello .NET property is [CloudTask.State](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.state).</span></span> <span data-ttu-id="2829b-157">Lásd az alábbi táblázatokban hello tulajdonságleképezései hello .NET és REST API-k között.</span><span class="sxs-lookup"><span data-stu-id="2829b-157">See hello tables below for property mappings between hello .NET and REST APIs.</span></span>
> 
> 

### <a name="rules-for-filter-select-and-expand-strings"></a><span data-ttu-id="2829b-158">Szabályok szűrő, válassza ki, és bontsa ki a karakterláncok</span><span class="sxs-lookup"><span data-stu-id="2829b-158">Rules for filter, select, and expand strings</span></span>
* <span data-ttu-id="2829b-159">Tulajdonságok nevek szűrő, válassza ki, és bontsa ki a karakterláncok megjelenjen-e, mint a hello [Batch REST] [ api_rest] API – még akkor is, ha használ [Batch .NET] [ api_net] vagy az egyik kötegelt Csomagjától hello.</span><span class="sxs-lookup"><span data-stu-id="2829b-159">Properties names in filter, select, and expand strings should appear as they do in hello [Batch REST][api_rest] API--even when you use [Batch .NET][api_net] or one of hello other Batch SDKs.</span></span>
* <span data-ttu-id="2829b-160">Minden tulajdonságnevek megkülönböztetik a kis-és nagybetűket, de a tulajdonságértékek-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="2829b-160">All property names are case-sensitive, but property values are case insensitive.</span></span>
* <span data-ttu-id="2829b-161">Dátum és idő karakterláncok lehet két formátumok egyikét, és utasítás előtt szerepelnie kell a `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="2829b-161">Date/time strings can be one of two formats, and must be preceded with `DateTime`.</span></span>
  
  * <span data-ttu-id="2829b-162">W3C-DTF formátum példa:`creationTime gt DateTime'2011-05-08T08:49:37Z'`</span><span class="sxs-lookup"><span data-stu-id="2829b-162">W3C-DTF format example: `creationTime gt DateTime'2011-05-08T08:49:37Z'`</span></span>
  * <span data-ttu-id="2829b-163">RFC 1123 formátum példa:`creationTime gt DateTime'Sun, 08 May 2011 08:49:37 GMT'`</span><span class="sxs-lookup"><span data-stu-id="2829b-163">RFC 1123 format example: `creationTime gt DateTime'Sun, 08 May 2011 08:49:37 GMT'`</span></span>
* <span data-ttu-id="2829b-164">Logikai értékek a következők: vagy `true` vagy `false`.</span><span class="sxs-lookup"><span data-stu-id="2829b-164">Boolean strings are either `true` or `false`.</span></span>
* <span data-ttu-id="2829b-165">Ha egy tulajdonság érvénytelen vagy operátor van megadva, a `400 (Bad Request)` hibát fog okozni.</span><span class="sxs-lookup"><span data-stu-id="2829b-165">If an invalid property or operator is specified, a `400 (Bad Request)` error will result.</span></span>

## <a name="efficient-querying-in-batch-net"></a><span data-ttu-id="2829b-166">Hatékony, a Batch .NET lekérdezése</span><span class="sxs-lookup"><span data-stu-id="2829b-166">Efficient querying in Batch .NET</span></span>
<span data-ttu-id="2829b-167">Hello belül [Batch .NET] [ api_net] API, hello [ODATADetailLevel] [ odata] osztály szűrő ellátására szolgál, válassza ki, és bontsa ki a karakterláncok toolist műveletek.</span><span class="sxs-lookup"><span data-stu-id="2829b-167">Within hello [Batch .NET][api_net] API, hello [ODATADetailLevel][odata] class is used for supplying filter, select, and expand strings toolist operations.</span></span> <span data-ttu-id="2829b-168">hello ODataDetailLevel osztály tulajdonságai három nyilvános karakterlánc, amely meg hello a konstruktorban, vagy közvetlenül hello objektumra beállítva.</span><span class="sxs-lookup"><span data-stu-id="2829b-168">hello ODataDetailLevel class has three public string properties that can be specified in hello constructor, or set directly on hello object.</span></span> <span data-ttu-id="2829b-169">Majd át hello ODataDetailLevel objektum mint egy paraméterrel toohello különböző listázási műveletei például [ListPools][net_list_pools], [ListJobs][net_list_jobs], és [ListTasks][net_list_tasks].</span><span class="sxs-lookup"><span data-stu-id="2829b-169">You then pass hello ODataDetailLevel object as a parameter toohello various list operations such as [ListPools][net_list_pools], [ListJobs][net_list_jobs], and [ListTasks][net_list_tasks].</span></span>

* <span data-ttu-id="2829b-170">[ODATADetailLevel][odata].[ FilterClause][odata_filter]: hello eredmények számát.</span><span class="sxs-lookup"><span data-stu-id="2829b-170">[ODATADetailLevel][odata].[FilterClause][odata_filter]: Limit hello number of items that are returned.</span></span>
* <span data-ttu-id="2829b-171">[ODATADetailLevel][odata].[ SelectClause][odata_select]: Adja meg a tulajdonságértékek egyes elemek küld vissza a rendszer.</span><span class="sxs-lookup"><span data-stu-id="2829b-171">[ODATADetailLevel][odata].[SelectClause][odata_select]: Specify which property values are returned with each item.</span></span>
* <span data-ttu-id="2829b-172">[ODATADetailLevel][odata].[ ExpandClause][odata_expand]: olvashatók be adatokat az összes egy API hívása helyett külön hívások minden elemhez.</span><span class="sxs-lookup"><span data-stu-id="2829b-172">[ODATADetailLevel][odata].[ExpandClause][odata_expand]: Retrieve data for all items in a single API call instead of separate calls for each item.</span></span>

<span data-ttu-id="2829b-173">hello következő kódrészletet használnak hello Batch .NET API tooefficiently lekérdezés hello kötegelt szolgáltatást egy adott készletét készletek hello statisztikáját.</span><span class="sxs-lookup"><span data-stu-id="2829b-173">hello following code snippet uses hello Batch .NET API tooefficiently query hello Batch service for hello statistics of a specific set of pools.</span></span> <span data-ttu-id="2829b-174">Ebben a forgatókönyvben hello kötegelt felhasználói teszt- és éles címkészletekkel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="2829b-174">In this scenario, hello Batch user has both test and production pools.</span></span> <span data-ttu-id="2829b-175">hello teszt készlet azonosítók fűzve előtagként a "test", és hello termelési gyűjtő azonosítók fűzve előtagként a "termék".</span><span class="sxs-lookup"><span data-stu-id="2829b-175">hello test pool IDs are prefixed with "test", and hello production pool IDs are prefixed with "prod".</span></span> <span data-ttu-id="2829b-176">A hello részlet *myBatchClient* hello megfelelően inicializálva példánya [BatchClient](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient) osztály.</span><span class="sxs-lookup"><span data-stu-id="2829b-176">In hello snippet, *myBatchClient* is a properly initialized instance of hello [BatchClient](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient) class.</span></span>

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
> <span data-ttu-id="2829b-177">Példányának [ODATADetailLevel] [ odata] válassza ki a konfigurált és kibontott záradék is átadhatók tooappropriate Get módszerek, például a [PoolOperations.GetPool](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.getpool.aspx) , toolimit hello mennyisége visszaadott adatokat.</span><span class="sxs-lookup"><span data-stu-id="2829b-177">An instance of [ODATADetailLevel][odata] that is configured with Select and Expand clauses can also be passed tooappropriate Get methods, such as [PoolOperations.GetPool](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.getpool.aspx), toolimit hello amount of data that is returned.</span></span>
> 
> 

## <a name="batch-rest-toonet-api-mappings"></a><span data-ttu-id="2829b-178">A Batch REST API-too.NET hozzárendelések</span><span class="sxs-lookup"><span data-stu-id="2829b-178">Batch REST too.NET API mappings</span></span>
<span data-ttu-id="2829b-179">Tulajdonságnevek szűrő, válassza ki, és bontsa ki a karakterláncok *kell* REST API mint a, mind a név és esetben tükrözi.</span><span class="sxs-lookup"><span data-stu-id="2829b-179">Property names in filter, select, and expand strings *must* reflect their REST API counterparts, both in name and case.</span></span> <span data-ttu-id="2829b-180">az alábbi táblázatok hello hello .NET és REST API megfelelők esetében közötti hozzárendelések adja meg.</span><span class="sxs-lookup"><span data-stu-id="2829b-180">hello tables below provide mappings between hello .NET and REST API counterparts.</span></span>

### <a name="mappings-for-filter-strings"></a><span data-ttu-id="2829b-181">Azon szűrőkarakterláncokban</span><span class="sxs-lookup"><span data-stu-id="2829b-181">Mappings for filter strings</span></span>
* <span data-ttu-id="2829b-182">**.NET-lista módszerek**: hello .NET API-módszer ebben az oszlopban fogad el egy [ODATADetailLevel] [ odata] objektum paraméterként.</span><span class="sxs-lookup"><span data-stu-id="2829b-182">**.NET list methods**: Each of hello .NET API methods in this column accepts an [ODATADetailLevel][odata] object as a parameter.</span></span>
* <span data-ttu-id="2829b-183">**REST kérelmek száma**: minden REST API lapon ez az oszlop, amelyben hello tulajdonságok és műveletek táblát tartalmaz, amelyek számára engedélyezett a csatolt tooin *szűrő* karakterláncok.</span><span class="sxs-lookup"><span data-stu-id="2829b-183">**REST list requests**: Each REST API page linked tooin this column contains a table that specifies hello properties and operations that are allowed in *filter* strings.</span></span> <span data-ttu-id="2829b-184">Használhatja a tulajdonság nevét és a műveletek hoz egy [ODATADetailLevel.FilterClause] [ odata_filter] karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="2829b-184">You will use these property names and operations when you construct an [ODATADetailLevel.FilterClause][odata_filter] string.</span></span>

| <span data-ttu-id="2829b-185">.NET-lista módszerek</span><span class="sxs-lookup"><span data-stu-id="2829b-185">.NET list methods</span></span> | <span data-ttu-id="2829b-186">REST kérelmek száma</span><span class="sxs-lookup"><span data-stu-id="2829b-186">REST list requests</span></span> |
| --- | --- |
| <span data-ttu-id="2829b-187">[CertificateOperations.ListCertificates][net_list_certs]</span><span class="sxs-lookup"><span data-stu-id="2829b-187">[CertificateOperations.ListCertificates][net_list_certs]</span></span> |<span data-ttu-id="2829b-188">[Egy fiók hello-tanúsítványok listázása][rest_list_certs]</span><span class="sxs-lookup"><span data-stu-id="2829b-188">[List hello certificates in an account][rest_list_certs]</span></span> |
| <span data-ttu-id="2829b-189">[CloudTask.ListNodeFiles][net_list_task_files]</span><span class="sxs-lookup"><span data-stu-id="2829b-189">[CloudTask.ListNodeFiles][net_list_task_files]</span></span> |<span data-ttu-id="2829b-190">[A feladathoz társított hello fájlok listázása][rest_list_task_files]</span><span class="sxs-lookup"><span data-stu-id="2829b-190">[List hello files associated with a task][rest_list_task_files]</span></span> |
| <span data-ttu-id="2829b-191">[JobOperations.ListJobPreparationAndReleaseTaskStatus][net_list_jobprep_status]</span><span class="sxs-lookup"><span data-stu-id="2829b-191">[JobOperations.ListJobPreparationAndReleaseTaskStatus][net_list_jobprep_status]</span></span> |<span data-ttu-id="2829b-192">[Hello hello feladat előkészítése és a feladat kiadása feladatok feladat állapota][rest_list_jobprep_status]</span><span class="sxs-lookup"><span data-stu-id="2829b-192">[List hello status of hello job preparation and job release tasks for a job][rest_list_jobprep_status]</span></span> |
| <span data-ttu-id="2829b-193">[JobOperations.ListJobs][net_list_jobs]</span><span class="sxs-lookup"><span data-stu-id="2829b-193">[JobOperations.ListJobs][net_list_jobs]</span></span> |<span data-ttu-id="2829b-194">[A fiók lista hello feladatok][rest_list_jobs]</span><span class="sxs-lookup"><span data-stu-id="2829b-194">[List hello jobs in an account][rest_list_jobs]</span></span> |
| <span data-ttu-id="2829b-195">[JobOperations.ListNodeFiles][net_list_nodefiles]</span><span class="sxs-lookup"><span data-stu-id="2829b-195">[JobOperations.ListNodeFiles][net_list_nodefiles]</span></span> |<span data-ttu-id="2829b-196">[A csomópont hello fájlok listázása][rest_list_nodefiles]</span><span class="sxs-lookup"><span data-stu-id="2829b-196">[List hello files on a node][rest_list_nodefiles]</span></span> |
| <span data-ttu-id="2829b-197">[JobOperations.ListTasks][net_list_tasks]</span><span class="sxs-lookup"><span data-stu-id="2829b-197">[JobOperations.ListTasks][net_list_tasks]</span></span> |<span data-ttu-id="2829b-198">[A feladathoz társított hello tevékenységei][rest_list_tasks]</span><span class="sxs-lookup"><span data-stu-id="2829b-198">[List hello tasks associated with a job][rest_list_tasks]</span></span> |
| <span data-ttu-id="2829b-199">[JobScheduleOperations.ListJobSchedules][net_list_job_schedules]</span><span class="sxs-lookup"><span data-stu-id="2829b-199">[JobScheduleOperations.ListJobSchedules][net_list_job_schedules]</span></span> |<span data-ttu-id="2829b-200">[Lista hello feladatütemezéseket egy fiók][rest_list_job_schedules]</span><span class="sxs-lookup"><span data-stu-id="2829b-200">[List hello job schedules in an account][rest_list_job_schedules]</span></span> |
| <span data-ttu-id="2829b-201">[JobScheduleOperations.ListJobs][net_list_schedule_jobs]</span><span class="sxs-lookup"><span data-stu-id="2829b-201">[JobScheduleOperations.ListJobs][net_list_schedule_jobs]</span></span> |<span data-ttu-id="2829b-202">[A feladatütemezés társított lista hello feladatok][rest_list_schedule_jobs]</span><span class="sxs-lookup"><span data-stu-id="2829b-202">[List hello jobs associated with a job schedule][rest_list_schedule_jobs]</span></span> |
| <span data-ttu-id="2829b-203">[PoolOperations.ListComputeNodes][net_list_compute_nodes]</span><span class="sxs-lookup"><span data-stu-id="2829b-203">[PoolOperations.ListComputeNodes][net_list_compute_nodes]</span></span> |<span data-ttu-id="2829b-204">[Lista hello számítási csomópontjain a készletben][rest_list_compute_nodes]</span><span class="sxs-lookup"><span data-stu-id="2829b-204">[List hello compute nodes in a pool][rest_list_compute_nodes]</span></span> |
| <span data-ttu-id="2829b-205">[PoolOperations.ListPools][net_list_pools]</span><span class="sxs-lookup"><span data-stu-id="2829b-205">[PoolOperations.ListPools][net_list_pools]</span></span> |<span data-ttu-id="2829b-206">[Egy fiók lista hello készletek][rest_list_pools]</span><span class="sxs-lookup"><span data-stu-id="2829b-206">[List hello pools in an account][rest_list_pools]</span></span> |

### <a name="mappings-for-select-strings"></a><span data-ttu-id="2829b-207">Jelölje be karakterláncok leképezéseit</span><span class="sxs-lookup"><span data-stu-id="2829b-207">Mappings for select strings</span></span>
* <span data-ttu-id="2829b-208">**A Batch .NET típusok**: Batch .NET API-típusok.</span><span class="sxs-lookup"><span data-stu-id="2829b-208">**Batch .NET types**: Batch .NET API types.</span></span>
* <span data-ttu-id="2829b-209">**REST API entitások**: Ebben az oszlopban lévő tartalmaz egy vagy több táblában, amely a REST API tulajdonságnevek hello hello típus listából.</span><span class="sxs-lookup"><span data-stu-id="2829b-209">**REST API entities**: Each page in this column contains one or more tables that list hello REST API property names for hello type.</span></span> <span data-ttu-id="2829b-210">A tulajdonságnevek hoz használt *válasszon* karakterláncok.</span><span class="sxs-lookup"><span data-stu-id="2829b-210">These property names are used when you construct *select* strings.</span></span> <span data-ttu-id="2829b-211">Ezek azonos tulajdonságnevek fogja használni, amikor, hozhat létre egy [ODATADetailLevel.SelectClause] [ odata_select] karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="2829b-211">You will use these same property names when you construct an [ODATADetailLevel.SelectClause][odata_select] string.</span></span>

| <span data-ttu-id="2829b-212">Batch .NET típusok</span><span class="sxs-lookup"><span data-stu-id="2829b-212">Batch .NET types</span></span> | <span data-ttu-id="2829b-213">REST API entitások</span><span class="sxs-lookup"><span data-stu-id="2829b-213">REST API entities</span></span> |
| --- | --- |
| <span data-ttu-id="2829b-214">[Tanúsítvány][net_cert]</span><span class="sxs-lookup"><span data-stu-id="2829b-214">[Certificate][net_cert]</span></span> |<span data-ttu-id="2829b-215">[A tanúsítvány adatainak beolvasása][rest_get_cert]</span><span class="sxs-lookup"><span data-stu-id="2829b-215">[Get information about a certificate][rest_get_cert]</span></span> |
| <span data-ttu-id="2829b-216">[CloudJob][net_job]</span><span class="sxs-lookup"><span data-stu-id="2829b-216">[CloudJob][net_job]</span></span> |<span data-ttu-id="2829b-217">[A feladat adatainak beolvasása][rest_get_job]</span><span class="sxs-lookup"><span data-stu-id="2829b-217">[Get information about a job][rest_get_job]</span></span> |
| <span data-ttu-id="2829b-218">[CloudJobSchedule][net_schedule]</span><span class="sxs-lookup"><span data-stu-id="2829b-218">[CloudJobSchedule][net_schedule]</span></span> |<span data-ttu-id="2829b-219">[A feladatütemezés adatainak beolvasása][rest_get_schedule]</span><span class="sxs-lookup"><span data-stu-id="2829b-219">[Get information about a job schedule][rest_get_schedule]</span></span> |
| <span data-ttu-id="2829b-220">[Átjárócsomópontján][net_node]</span><span class="sxs-lookup"><span data-stu-id="2829b-220">[ComputeNode][net_node]</span></span> |<span data-ttu-id="2829b-221">[A csomópont adatainak beolvasása][rest_get_node]</span><span class="sxs-lookup"><span data-stu-id="2829b-221">[Get information about a node][rest_get_node]</span></span> |
| <span data-ttu-id="2829b-222">[CloudPool][net_pool]</span><span class="sxs-lookup"><span data-stu-id="2829b-222">[CloudPool][net_pool]</span></span> |<span data-ttu-id="2829b-223">[Egy készlet adatainak beolvasása][rest_get_pool]</span><span class="sxs-lookup"><span data-stu-id="2829b-223">[Get information about a pool][rest_get_pool]</span></span> |
| <span data-ttu-id="2829b-224">[CloudTask][net_task]</span><span class="sxs-lookup"><span data-stu-id="2829b-224">[CloudTask][net_task]</span></span> |<span data-ttu-id="2829b-225">[A feladat adatainak beolvasása][rest_get_task]</span><span class="sxs-lookup"><span data-stu-id="2829b-225">[Get information about a task][rest_get_task]</span></span> |

## <a name="example-construct-a-filter-string"></a><span data-ttu-id="2829b-226">Példa: egy szűrési karakterláncot létrehozni</span><span class="sxs-lookup"><span data-stu-id="2829b-226">Example: construct a filter string</span></span>
<span data-ttu-id="2829b-227">Ha, hozhat létre egy szűrési karakterláncot a [ODATADetailLevel.FilterClause][odata_filter], tekintse át a hello "Szűrőkarakterláncokban leképezéseit" toofind hello REST API dokumentációja lap, amely megfelel a fenti táblázatban toohello list művelet, hogy kívánja-e tooperform.</span><span class="sxs-lookup"><span data-stu-id="2829b-227">When you construct a filter string for [ODATADetailLevel.FilterClause][odata_filter], consult hello table above under "Mappings for filter strings" toofind hello REST API documentation page that corresponds toohello list operation that you wish tooperform.</span></span> <span data-ttu-id="2829b-228">Hello szűrhető tulajdonságok és azok támogatott operátort hello többsoros első tábla adott oldalon találja.</span><span class="sxs-lookup"><span data-stu-id="2829b-228">You will find hello filterable properties and their supported operators in hello first multirow table on that page.</span></span> <span data-ttu-id="2829b-229">Ha a tooretrieve minden olyan feladat, amelynek kilépési kód: nem nulla, például a sor: a [a feladathoz társított hello tevékenységeinek] [ rest_list_tasks] hello megfelelő tulajdonság karakterlánc és engedélyezett operátorok adja meg:</span><span class="sxs-lookup"><span data-stu-id="2829b-229">If you wish tooretrieve all tasks whose exit code was nonzero, for example, this row on [List hello tasks associated with a job][rest_list_tasks] specifies hello applicable property string and allowable operators:</span></span>

| <span data-ttu-id="2829b-230">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="2829b-230">Property</span></span> | <span data-ttu-id="2829b-231">Engedélyezett műveletek</span><span class="sxs-lookup"><span data-stu-id="2829b-231">Operations allowed</span></span> | <span data-ttu-id="2829b-232">Típus</span><span class="sxs-lookup"><span data-stu-id="2829b-232">Type</span></span> |
|:--- |:--- |:--- |
| `executionInfo/exitCode` |`eq, ge, gt, le , lt` |`Int` |

<span data-ttu-id="2829b-233">Hello szűrési karakterláncot egy nem nulla kilépési kód minden feladat felsoroló így néz ki:</span><span class="sxs-lookup"><span data-stu-id="2829b-233">Thus, hello filter string for listing all tasks with a nonzero exit code would be:</span></span>

`(executionInfo/exitCode lt 0) or (executionInfo/exitCode gt 0)`

## <a name="example-construct-a-select-string"></a><span data-ttu-id="2829b-234">Például: select karakterlánc összeállításához</span><span class="sxs-lookup"><span data-stu-id="2829b-234">Example: construct a select string</span></span>
<span data-ttu-id="2829b-235">tooconstruct [ODATADetailLevel.SelectClause][odata_select], tekintse át a hello "Select karakterláncok leképezéseit" a fenti táblázatban, és keresse meg a megfelelő toohello típusú entitás toohello REST API lapján, amikor listázásakor.</span><span class="sxs-lookup"><span data-stu-id="2829b-235">tooconstruct [ODATADetailLevel.SelectClause][odata_select], consult hello table above under "Mappings for select strings" and navigate toohello REST API page that corresponds toohello type of entity that you are listing.</span></span> <span data-ttu-id="2829b-236">Hello választható tulajdonságok és azok támogatott operátort hello többsoros első tábla adott oldalon találja.</span><span class="sxs-lookup"><span data-stu-id="2829b-236">You will find hello selectable properties and their supported operators in hello first multirow table on that page.</span></span> <span data-ttu-id="2829b-237">Ha tooretrieve csak hello azonosítója és minden feladat parancssori listájában, például találja a sorokra alkalmazandó tábla hello [feladat adatainak beolvasása][rest_get_task]:</span><span class="sxs-lookup"><span data-stu-id="2829b-237">If you wish tooretrieve only hello ID and command line for each task in a list, for example, you will find these rows in hello applicable table on [Get information about a task][rest_get_task]:</span></span>

| <span data-ttu-id="2829b-238">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="2829b-238">Property</span></span> | <span data-ttu-id="2829b-239">Típus</span><span class="sxs-lookup"><span data-stu-id="2829b-239">Type</span></span> | <span data-ttu-id="2829b-240">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="2829b-240">Notes</span></span> |
|:--- |:--- |:--- |
| `id` |`String` |`hello ID of hello task.` |
| `commandLine` |`String` |`hello command line of hello task.` |

<span data-ttu-id="2829b-241">hello válassza karakterláncot például csak hello azonosítója és minden felsorolt feladat-parancssorból lesz:</span><span class="sxs-lookup"><span data-stu-id="2829b-241">hello select string for including only hello ID and command line with each listed task would then be:</span></span>

`id, commandLine`

## <a name="code-samples"></a><span data-ttu-id="2829b-242">Kódminták</span><span class="sxs-lookup"><span data-stu-id="2829b-242">Code samples</span></span>
### <a name="efficient-list-queries-code-sample"></a><span data-ttu-id="2829b-243">Hatékony lista lekérdezések kódminta</span><span class="sxs-lookup"><span data-stu-id="2829b-243">Efficient list queries code sample</span></span>
<span data-ttu-id="2829b-244">Tekintse meg a hello [EfficientListQueries] [ efficient_query_sample] mintaprojektet a Githubon toosee hogyan hatékony lista lekérdezése befolyásolhatja a teljesítményt az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="2829b-244">Check out hello [EfficientListQueries][efficient_query_sample] sample project on GitHub toosee how efficient list querying can affect performance in an application.</span></span> <span data-ttu-id="2829b-245">A C# Konzolalkalmazás hoz létre, és hozzáadja a nagyszámú feladatok tooa feladat.</span><span class="sxs-lookup"><span data-stu-id="2829b-245">This C# console application creates and adds a large number of tasks tooa job.</span></span> <span data-ttu-id="2829b-246">Majd, így több hívások toohello [JobOperations.ListTasks] [ net_list_tasks] metódus és fázisok [ODATADetailLevel] [ odata] -objektumok a másik tulajdonságot értékek toovary hello méretű visszaadott adatok toobe konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="2829b-246">Then, it makes multiple calls toohello [JobOperations.ListTasks][net_list_tasks] method and passes [ODATADetailLevel][odata] objects that are configured with different property values toovary hello amount of data toobe returned.</span></span> <span data-ttu-id="2829b-247">Kimeneti hasonló toohello következő képes:</span><span class="sxs-lookup"><span data-stu-id="2829b-247">It produces output similar toohello following:</span></span>

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

<span data-ttu-id="2829b-248">Amint hello eltelt idő, nagy mértékben csökkentheti lekérdezési válaszidőt hello tulajdonságok és hello elemek száma, amelyek a rendszer visszairányítja korlátozásával.</span><span class="sxs-lookup"><span data-stu-id="2829b-248">As shown in hello elapsed times, you can greatly lower query response times by limiting hello properties and hello number of items that are returned.</span></span> <span data-ttu-id="2829b-249">Ez, és más mintaprojektjeit az található hello [azure-köteg-minták] [ github_samples] GitHub tárházából.</span><span class="sxs-lookup"><span data-stu-id="2829b-249">You can find this and other sample projects in hello [azure-batch-samples][github_samples] repository on GitHub.</span></span>

### <a name="batchmetrics-library-and-code-sample"></a><span data-ttu-id="2829b-250">BatchMetrics és a kód a minta</span><span class="sxs-lookup"><span data-stu-id="2829b-250">BatchMetrics library and code sample</span></span>
<span data-ttu-id="2829b-251">Ezenkívül toohello EfficientListQueries kódmintában fenti található hello [BatchMetrics] [ batch_metrics] projektre a hello [azure-köteg-minták] [ github_samples] GitHub-tárházban.</span><span class="sxs-lookup"><span data-stu-id="2829b-251">In addition toohello EfficientListQueries code sample above, you can find hello [BatchMetrics][batch_metrics] project in hello [azure-batch-samples][github_samples] GitHub repository.</span></span> <span data-ttu-id="2829b-252">hello BatchMetrics mintaprojektet bemutatja, hogyan tooefficiently figyelése Azure kötegelt feladatok előrehaladásának hello kötegelt API használatával.</span><span class="sxs-lookup"><span data-stu-id="2829b-252">hello BatchMetrics sample project demonstrates how tooefficiently monitor Azure Batch job progress using hello Batch API.</span></span>

<span data-ttu-id="2829b-253">Hello [BatchMetrics] [ batch_metrics] minta tartalmazza a .NET hordozhatóosztálytár-projektjének amely beépítheti saját projektek és egy egyszerű parancssori tooexercise programot, és mutassa be hello hello használata könyvtár.</span><span class="sxs-lookup"><span data-stu-id="2829b-253">hello [BatchMetrics][batch_metrics] sample includes a .NET class library project which you can incorporate into your own projects, and a simple command-line program tooexercise and demonstrate hello use of hello library.</span></span>

<span data-ttu-id="2829b-254">hello mintaalkalmazás hello projektben mutatja be a következő műveletek hello:</span><span class="sxs-lookup"><span data-stu-id="2829b-254">hello sample application within hello project demonstrates hello following operations:</span></span>

1. <span data-ttu-id="2829b-255">Adott attribútumok kiválasztása rendelés toodownload csak hello tulajdonságainál kell</span><span class="sxs-lookup"><span data-stu-id="2829b-255">Selecting specific attributes in order toodownload only hello properties you need</span></span>
2. <span data-ttu-id="2829b-256">Szűrés állapot átmenet alkalommal rendelés toodownload csak akkor változik meg hello legutóbbi lekérdezés óta</span><span class="sxs-lookup"><span data-stu-id="2829b-256">Filtering on state transition times in order toodownload only changes since hello last query</span></span>

<span data-ttu-id="2829b-257">Például hello BatchMetrics könyvtár a következő metódus hello jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="2829b-257">For example, hello following method appears in hello BatchMetrics library.</span></span> <span data-ttu-id="2829b-258">Azt adja vissza, amely meghatározza, hogy csak hello egy ODATADetailLevel `id` és `state` tulajdonságait a rendszer megkérdezi a hello entitások beszerzése.</span><span class="sxs-lookup"><span data-stu-id="2829b-258">It returns an ODATADetailLevel that specifies that only hello `id` and `state` properties should be obtained for hello entities that are queried.</span></span> <span data-ttu-id="2829b-259">Azt is, amely csak entitások, amelynek állapota módosult az megadott hello `DateTime` vissza kell adni a paraméter.</span><span class="sxs-lookup"><span data-stu-id="2829b-259">It also specifies that only entities whose state has changed since hello specified `DateTime` parameter should be returned.</span></span>

```csharp
internal static ODATADetailLevel OnlyChangedAfter(DateTime time)
{
    return new ODATADetailLevel(
        selectClause: "id, state",
        filterClause: string.Format("stateTransitionTime gt DateTime'{0:o}'", time)
    );
}
```

## <a name="next-steps"></a><span data-ttu-id="2829b-260">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2829b-260">Next steps</span></span>
### <a name="parallel-node-tasks"></a><span data-ttu-id="2829b-261">Párhuzamos csomópont feladatok</span><span class="sxs-lookup"><span data-stu-id="2829b-261">Parallel node tasks</span></span>
<span data-ttu-id="2829b-262">[Azure Batch számítási erőforrás-használat csomópont egyidejű feladatok maximális](batch-parallel-node-tasks.md) van egy másik cikkben tooBatch alkalmazásteljesítmény kapcsolatos.</span><span class="sxs-lookup"><span data-stu-id="2829b-262">[Maximize Azure Batch compute resource usage with concurrent node tasks](batch-parallel-node-tasks.md) is another article related tooBatch application performance.</span></span> <span data-ttu-id="2829b-263">Bizonyos típusú munkaterheléseket is kihasználhatja a párhuzamos tevékenységek feldolgozás alatt álló nagyobb – de--kevesebb számítási csomópontot.</span><span class="sxs-lookup"><span data-stu-id="2829b-263">Some types of workloads can benefit from executing parallel tasks on larger--but fewer--compute nodes.</span></span> <span data-ttu-id="2829b-264">Tekintse meg a hello [példa](batch-parallel-node-tasks.md#example-scenario) hello cikkben talál részletes információt ilyen esetben.</span><span class="sxs-lookup"><span data-stu-id="2829b-264">Check out hello [example scenario](batch-parallel-node-tasks.md#example-scenario) in hello article for details on such a scenario.</span></span>

### <a name="batch-forum"></a><span data-ttu-id="2829b-265">Batch fórum</span><span class="sxs-lookup"><span data-stu-id="2829b-265">Batch Forum</span></span>
<span data-ttu-id="2829b-266">Hello [Azure Batch fórum] [ forum] MSDN nagyszerű toodiscuss kötegelt helyezze, és hello szolgáltatás kérdése.</span><span class="sxs-lookup"><span data-stu-id="2829b-266">hello [Azure Batch Forum][forum] on MSDN is a great place toodiscuss Batch and ask questions about hello service.</span></span> <span data-ttu-id="2829b-267">Központi a keresztül a hasznos "kapcsolódó" bejegyzések, és a kötegelt megoldások létrehozása során felmerülő kérdéseit.</span><span class="sxs-lookup"><span data-stu-id="2829b-267">Head on over for helpful "sticky" posts, and post your questions as they arise while you build your Batch solutions.</span></span>

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