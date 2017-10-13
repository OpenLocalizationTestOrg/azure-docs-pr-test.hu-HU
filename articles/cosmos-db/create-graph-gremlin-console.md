---
title: "Oktatóanyag az Azure Cosmos DB használatához: Elemek létrehozása, lekérdezése és gráfbejárás az Apache TinkerPops Gremlin-konzolban | Microsoft Docs"
description: "Egy Cosmos-DB Azure gyors üzembe helyezés toocreates csúcsban szélek és lekérdezések hello Azure Cosmos DB Graph API segítségével."
services: cosmos-db
author: dennyglee
manager: jhubbard
editor: monicar
ms.assetid: bf08e031-718a-4a2a-89d6-91e12ff8797d
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: terminal
ms.topic: hero-article
ms.date: 07/27/2017
ms.author: denlee
ms.openlocfilehash: 9de64c97fec89c45cecba9e14214db472ec76f57
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-create-query-and-traverse-a-graph-in-hello-gremlin-console"></a><span data-ttu-id="9c809-103">Az Azure Cosmos DB: Hozzon létre a lekérdezést, és haladnak át egy grafikonon hello Gremlin konzolon</span><span class="sxs-lookup"><span data-stu-id="9c809-103">Azure Cosmos DB: Create, query, and traverse a graph in hello Gremlin console</span></span>

<span data-ttu-id="9c809-104">Az Azure Cosmos DB a Microsoft globálisan elosztott többmodelles adatbázis-szolgáltatása.</span><span class="sxs-lookup"><span data-stu-id="9c809-104">Azure Cosmos DB is Microsoft’s globally distributed multi-model database service.</span></span> <span data-ttu-id="9c809-105">Gyorsan hozzon létre, és a dokumentum, a kulcs/érték és a graph adatbázisok, amelyek kihasználhassa hello globális terjesztési és horizontális skálázhatóságot képességekről az Azure-Cosmos adatbázis hello core lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="9c809-105">You can quickly create and query document, key/value, and graph databases, all of which benefit from hello global distribution and horizontal scale capabilities at hello core of Azure Cosmos DB.</span></span> 

<span data-ttu-id="9c809-106">A gyors üzembe helyezési bemutatja, hogyan toocreate Azure Cosmos DB fiókkal, az adatbázis és a graph (tároló) használatával hello Azure-portálon, majd használja hello [Gremlin konzol](https://tinkerpop.apache.org/docs/current/reference/#gremlin-console) a [Apache TinkerPop](http://tinkerpop.apache.org) rendelkező toowork Diagramadatok API (előzetes verzió).</span><span class="sxs-lookup"><span data-stu-id="9c809-106">This quick start demonstrates how toocreate an Azure Cosmos DB account, database, and graph (container) using hello Azure portal and then use hello [Gremlin Console](https://tinkerpop.apache.org/docs/current/reference/#gremlin-console) from  [Apache TinkerPop](http://tinkerpop.apache.org) toowork with Graph API (preview) data.</span></span> <span data-ttu-id="9c809-107">Ebben az oktatóanyagban, és lekérdezi a csúcsban és szélén, egy csúcsának tulajdonság csúcsban lekérdezésére, hello graph haladnak át, és dobja el a csúcspont.</span><span class="sxs-lookup"><span data-stu-id="9c809-107">In this tutorial, you create and query vertices and edges, updating a vertex property, query vertices, traverse hello graph, and drop a vertex.</span></span>

![Az Azure Cosmos DB hello Apache Gremlin konzolról](./media/create-graph-gremlin-console/gremlin-console.png)

<span data-ttu-id="9c809-109">hello Gremlin konzol Groovy/Java-alapú, és a Linux, Mac és Windows futtatja.</span><span class="sxs-lookup"><span data-stu-id="9c809-109">hello Gremlin console is Groovy/Java based and runs on Linux, Mac, and Windows.</span></span> <span data-ttu-id="9c809-110">Letölthető hello [Apache TinkerPop hely](https://www.apache.org/dyn/closer.lua/tinkerpop/3.2.5/apache-tinkerpop-gremlin-console-3.2.5-bin.zip).</span><span class="sxs-lookup"><span data-stu-id="9c809-110">You can download it from hello [Apache TinkerPop site](https://www.apache.org/dyn/closer.lua/tinkerpop/3.2.5/apache-tinkerpop-gremlin-console-3.2.5-bin.zip).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9c809-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="9c809-111">Prerequisites</span></span>

<span data-ttu-id="9c809-112">A gyors üzembe helyezés toohave egy Azure-előfizetés toocreate Azure Cosmos DB fiók szükséges.</span><span class="sxs-lookup"><span data-stu-id="9c809-112">You need toohave an Azure subscription toocreate an Azure Cosmos DB account for this quickstart.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

<span data-ttu-id="9c809-113">Szükség tooinstall hello [Gremlin konzol](http://tinkerpop.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="9c809-113">You also need tooinstall hello [Gremlin Console](http://tinkerpop.apache.org/).</span></span> <span data-ttu-id="9c809-114">A 3.2.5-ös vagy újabb verziót használja.</span><span class="sxs-lookup"><span data-stu-id="9c809-114">Use version 3.2.5 or above.</span></span>

## <a name="create-a-database-account"></a><span data-ttu-id="9c809-115">Adatbázisfiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="9c809-115">Create a database account</span></span>

[!INCLUDE [cosmos-db-create-dbaccount-graph](../../includes/cosmos-db-create-dbaccount-graph.md)]

## <a name="add-a-graph"></a><span data-ttu-id="9c809-116">Gráf hozzáadása</span><span class="sxs-lookup"><span data-stu-id="9c809-116">Add a graph</span></span>

[!INCLUDE [cosmos-db-create-graph](../../includes/cosmos-db-create-graph.md)]

## <span data-ttu-id="9c809-117"><a id="ConnectAppService"></a>Csatlakozás tooyour app service</span><span class="sxs-lookup"><span data-stu-id="9c809-117"><a id="ConnectAppService"></a>Connect tooyour app service</span></span>
1. <span data-ttu-id="9c809-118">Elindítása előtt hello Gremlin konzol, hozzon létre, vagy módosítsa a hello távoli-secure.yaml konfigurációs fájl hello apache-tinkerpop-gremlin-console-3.2.5/conf könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="9c809-118">Before starting hello Gremlin Console, create or modify hello remote-secure.yaml configuration file in hello apache-tinkerpop-gremlin-console-3.2.5/conf directory.</span></span>
2. <span data-ttu-id="9c809-119">Adja meg a *host* (gazdagép), *Port*, *username* (felhasználónév), *password* (jelszó), *ConnectionPool* (kapcsolatkészlet), és *serializer* (szerializáló) beállításokat:</span><span class="sxs-lookup"><span data-stu-id="9c809-119">Fill in your *host*, *port*, *username*, *password*, *connectionPool*, and *serializer* configurations:</span></span>

    <span data-ttu-id="9c809-120">Beállítás</span><span class="sxs-lookup"><span data-stu-id="9c809-120">Setting</span></span>|<span data-ttu-id="9c809-121">Ajánlott érték</span><span class="sxs-lookup"><span data-stu-id="9c809-121">Suggested value</span></span>|<span data-ttu-id="9c809-122">Leírás</span><span class="sxs-lookup"><span data-stu-id="9c809-122">Description</span></span>
    ---|---|---
    <span data-ttu-id="9c809-123">gazdagépek</span><span class="sxs-lookup"><span data-stu-id="9c809-123">hosts</span></span>|<span data-ttu-id="9c809-124">[***.graphs.azure.com]</span><span class="sxs-lookup"><span data-stu-id="9c809-124">[***.graphs.azure.com]</span></span>|<span data-ttu-id="9c809-125">Lásd az alábbi képernyőképet.</span><span class="sxs-lookup"><span data-stu-id="9c809-125">See screenshot below.</span></span> <span data-ttu-id="9c809-126">Ez az hello Gremlin URI azonosítóját az Azure-portálon, a hello záró szögletes zárójelben hello hello áttekintése lapon: 443 / eltávolított.</span><span class="sxs-lookup"><span data-stu-id="9c809-126">This is hello Gremlin URI value on hello Overview page of hello Azure portal, in square brackets, with hello trailing :443/ removed.</span></span><br><br><span data-ttu-id="9c809-127">Ez az érték is lekérhetők hello kulcsok lapon hello URI érték használatával https:// eltávolítása, dokumentumok toographs módosítása és eltávolítása a hello záró: 443 /.</span><span class="sxs-lookup"><span data-stu-id="9c809-127">This value can also be retrieved from hello Keys tab, using hello URI value by removing https://, changing documents toographs, and removing hello trailing :443/.</span></span>
    <span data-ttu-id="9c809-128">port</span><span class="sxs-lookup"><span data-stu-id="9c809-128">port</span></span>|<span data-ttu-id="9c809-129">443</span><span class="sxs-lookup"><span data-stu-id="9c809-129">443</span></span>|<span data-ttu-id="9c809-130">Állítsa be a too443.</span><span class="sxs-lookup"><span data-stu-id="9c809-130">Set too443.</span></span>
    <span data-ttu-id="9c809-131">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="9c809-131">username</span></span>|<span data-ttu-id="9c809-132">*Az Ön felhasználóneve*</span><span class="sxs-lookup"><span data-stu-id="9c809-132">*Your username*</span></span>|<span data-ttu-id="9c809-133">erőforrás hello űrlap hello `/dbs/<db>/colls/<coll>` ahol `<db>` az adatbázisnév és `<coll>` a gyűjtemény neve.</span><span class="sxs-lookup"><span data-stu-id="9c809-133">hello resource of hello form `/dbs/<db>/colls/<coll>` where `<db>` is your database name and `<coll>` is your collection name.</span></span>
    <span data-ttu-id="9c809-134">jelszó</span><span class="sxs-lookup"><span data-stu-id="9c809-134">password</span></span>|<span data-ttu-id="9c809-135">*Az Ön elsődleges kulcsa*</span><span class="sxs-lookup"><span data-stu-id="9c809-135">*Your primary key*</span></span>| <span data-ttu-id="9c809-136">Lásd az alábbiakban a második képernyőképet.</span><span class="sxs-lookup"><span data-stu-id="9c809-136">See second screenshot below.</span></span> <span data-ttu-id="9c809-137">Ez az az elsődleges kulcs, és beolvasható hello kulcsok lapján hello hello elsődleges kulcs mezőben Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="9c809-137">This is your primary key, which you can retrieve from hello Keys page of hello Azure portal, in hello Primary Key box.</span></span> <span data-ttu-id="9c809-138">Hello bal oldalán hello lista toocopy hello értékét használja a hello Másolás gombra.</span><span class="sxs-lookup"><span data-stu-id="9c809-138">Use hello copy button on hello left side of hello box toocopy hello value.</span></span>
    <span data-ttu-id="9c809-139">kapcsolatkészlet</span><span class="sxs-lookup"><span data-stu-id="9c809-139">connectionPool</span></span>|<span data-ttu-id="9c809-140">{enableSsl: true}</span><span class="sxs-lookup"><span data-stu-id="9c809-140">{enableSsl: true}</span></span>|<span data-ttu-id="9c809-141">A kapcsolatkészletre vonatkozó beállítás az SSL-hez.</span><span class="sxs-lookup"><span data-stu-id="9c809-141">Your connection pool setting for SSL.</span></span>
    <span data-ttu-id="9c809-142">szerializáló</span><span class="sxs-lookup"><span data-stu-id="9c809-142">serializer</span></span>|<span data-ttu-id="9c809-143">{ className: org.apache.tinkerpop.gremlin.</span><span class="sxs-lookup"><span data-stu-id="9c809-143">{ className: org.apache.tinkerpop.gremlin.</span></span><br><span data-ttu-id="9c809-144">driver.ser.GraphSONMessageSerializerV1d0,</span><span class="sxs-lookup"><span data-stu-id="9c809-144">driver.ser.GraphSONMessageSerializerV1d0,</span></span><br> <span data-ttu-id="9c809-145">config: { serializeResultToString: true }}</span><span class="sxs-lookup"><span data-stu-id="9c809-145">config: { serializeResultToString: true }}</span></span>|<span data-ttu-id="9c809-146">Toothis érték, és töröl minden `\n` oldaltörések sor hello érték beillesztéskor.</span><span class="sxs-lookup"><span data-stu-id="9c809-146">Set toothis value and delete any `\n` line breaks when pasting in hello value.</span></span>

    <span data-ttu-id="9c809-147">Hello állomások értékhez, másolja a hello **Gremlin URI** hello értéket **áttekintése** lap: ![megtekintése és másolása hello Gremlin URI érték hello áttekintése lapon a hello Azure-portálon](./media/create-graph-gremlin-console/gremlin-uri.png)</span><span class="sxs-lookup"><span data-stu-id="9c809-147">For hello hosts value, copy hello **Gremlin URI** value from hello **Overview** page: ![View and copy hello Gremlin URI value on hello Overview page in hello Azure portal](./media/create-graph-gremlin-console/gremlin-uri.png)</span></span>

    <span data-ttu-id="9c809-148">Hello jelszó értékét, másolja a hello **elsődleges kulcs** a hello **kulcsok** lap: ![megtekintése és másolása az elsődleges kulcs hello Azure-portálon, a kulcsok lap](./media/create-graph-gremlin-console/keys.png)</span><span class="sxs-lookup"><span data-stu-id="9c809-148">For hello password value, copy hello **Primary key** from hello **Keys** page: ![View and copy your primary key in hello Azure portal, Keys page](./media/create-graph-gremlin-console/keys.png)</span></span>


3. <span data-ttu-id="9c809-149">Futtassa a terminálon `bin/gremlin.bat` vagy `bin/gremlin.sh` toostart hello [Gremlin konzol](http://tinkerpop.apache.org/docs/3.2.5/tutorials/getting-started/).</span><span class="sxs-lookup"><span data-stu-id="9c809-149">In your terminal, run `bin/gremlin.bat` or `bin/gremlin.sh` toostart hello [Gremlin Console](http://tinkerpop.apache.org/docs/3.2.5/tutorials/getting-started/).</span></span>
4. <span data-ttu-id="9c809-150">Futtassa a terminálon `:remote connect tinkerpop.server conf/remote-secure.yaml` tooconnect tooyour app service.</span><span class="sxs-lookup"><span data-stu-id="9c809-150">In your terminal, run `:remote connect tinkerpop.server conf/remote-secure.yaml` tooconnect tooyour app service.</span></span>

    > [!TIP]
    > <span data-ttu-id="9c809-151">Ha hello hibaüzenet `No appenders could be found for logger` ellenőrizze, hogy a 2. lépésben leírtak hello szerializáló érték hello távoli-secure.yaml fájl frissítése.</span><span class="sxs-lookup"><span data-stu-id="9c809-151">If you receive hello error `No appenders could be found for logger` ensure that you updated hello serializer value in hello remote-secure.yaml file as described in step 2.</span></span> 

<span data-ttu-id="9c809-152">Remek!</span><span class="sxs-lookup"><span data-stu-id="9c809-152">Great!</span></span> <span data-ttu-id="9c809-153">Most, hogy befejeztük hello beállítása, először néhány konzol parancsok futtatása.</span><span class="sxs-lookup"><span data-stu-id="9c809-153">Now that we finished hello setup, let's start running some console commands.</span></span>

<span data-ttu-id="9c809-154">Próbáljon ki egy egyszerű count() parancsot.</span><span class="sxs-lookup"><span data-stu-id="9c809-154">Let's try a simple count() command.</span></span> <span data-ttu-id="9c809-155">Írja be a hello következő hello konzolba hello parancssorba:</span><span class="sxs-lookup"><span data-stu-id="9c809-155">Type hello following into hello console at hello prompt:</span></span>
```
:> g.V().count()
```

> [!TIP]
> <span data-ttu-id="9c809-156">Értesítés hello `:>` , amely megelőzi hello `g.V().count()` szöveg?</span><span class="sxs-lookup"><span data-stu-id="9c809-156">Notice hello `:>` that precedes hello `g.V().count()` text?</span></span> 
>
> <span data-ttu-id="9c809-157">Ez tootype kell hello parancs része.</span><span class="sxs-lookup"><span data-stu-id="9c809-157">This is part of hello command you need tootype.</span></span> <span data-ttu-id="9c809-158">Fontos hello Gremlin konzol, az Azure Cosmos DB használatakor.</span><span class="sxs-lookup"><span data-stu-id="9c809-158">It is important when using hello Gremlin console, with Azure Cosmos DB.</span></span>  
>
> <span data-ttu-id="9c809-159">Ez kihagyásával `:>` előtag utasítja hello konzol tooexecute hello parancsot helyileg, gyakran egy memórián belüli graph ellen.</span><span class="sxs-lookup"><span data-stu-id="9c809-159">Omitting this `:>` prefix instructs hello console tooexecute hello command locally, often against an in-memory graph.</span></span>
> <span data-ttu-id="9c809-160">Ennek segítségével `:>` hatására hello konzol tooexecute egy távoli parancs, ebben az esetben Cosmos DB ellen (vagy hello localhost emulátor, vagy egy > Azure-példányt).</span><span class="sxs-lookup"><span data-stu-id="9c809-160">Using this `:>` tells hello console tooexecute a remote command, in this case against Cosmos DB (either hello localhost emulator, or an > Azure instance).</span></span>


## <a name="create-vertices-and-edges"></a><span data-ttu-id="9c809-161">Csúcsok és élek létrehozása</span><span class="sxs-lookup"><span data-stu-id="9c809-161">Create vertices and edges</span></span>

<span data-ttu-id="9c809-162">Először hozzon létre öt darab, egy-egy személyt jelölő csúcsot *Thomas*, *Mary Kay*, *Robin*, *Ben*, és *Jack* néven.</span><span class="sxs-lookup"><span data-stu-id="9c809-162">Let's begin by adding five person vertices for *Thomas*, *Mary Kay*, *Robin*, *Ben*, and *Jack*.</span></span>

<span data-ttu-id="9c809-163">Bemenet (Thomas):</span><span class="sxs-lookup"><span data-stu-id="9c809-163">Input (Thomas):</span></span>

```
:> g.addV('person').property('firstName', 'Thomas').property('lastName', 'Andersen').property('age', 44).property('userid', 1)
```

<span data-ttu-id="9c809-164">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="9c809-164">Output:</span></span>

```
==>[id:796cdccc-2acd-4e58-a324-91d6f6f5ed6d,label:person,type:vertex,properties:[firstName:[[id:f02a749f-b67c-4016-850e-910242d68953,value:Thomas]],lastName:[[id:f5fa3126-8818-4fda-88b0-9bb55145ce5c,value:Andersen]],age:[[id:f6390f9c-e563-433e-acbf-25627628016e,value:44]],userid:[[id:796cdccc-2acd-4e58-a324-91d6f6f5ed6d|userid,value:1]]]]
```
<span data-ttu-id="9c809-165">Bemenet (Mary Kay):</span><span class="sxs-lookup"><span data-stu-id="9c809-165">Input (Mary Kay):</span></span>

```
:> g.addV('person').property('firstName', 'Mary Kay').property('lastName', 'Andersen').property('age', 39).property('userid', 2)

```

<span data-ttu-id="9c809-166">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="9c809-166">Output:</span></span>

```
==>[id:0ac9be25-a476-4a30-8da8-e79f0119ea5e,label:person,type:vertex,properties:[firstName:[[id:ea0604f8-14ee-4513-a48a-1734a1f28dc0,value:Mary Kay]],lastName:[[id:86d3bba5-fd60-4856-9396-c195ef7d7f4b,value:Andersen]],age:[[id:bc81b78d-30c4-4e03-8f40-50f72eb5f6da,value:39]],userid:[[id:0ac9be25-a476-4a30-8da8-e79f0119ea5e|userid,value:2]]]]

```

<span data-ttu-id="9c809-167">Bemenet (Robin):</span><span class="sxs-lookup"><span data-stu-id="9c809-167">Input (Robin):</span></span>

```
:> g.addV('person').property('firstName', 'Robin').property('lastName', 'Wakefield').property('userid', 3)
```

<span data-ttu-id="9c809-168">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="9c809-168">Output:</span></span>

```
==>[id:8dc14d6a-8683-4a54-8d74-7eef1fb43a3e,label:person,type:vertex,properties:[firstName:[[id:ec65f078-7a43-4cbe-bc06-e50f2640dc4e,value:Robin]],lastName:[[id:a3937d07-0e88-45d3-a442-26fcdfb042ce,value:Wakefield]],userid:[[id:8dc14d6a-8683-4a54-8d74-7eef1fb43a3e|userid,value:3]]]]
```

<span data-ttu-id="9c809-169">Bemenet (Ben):</span><span class="sxs-lookup"><span data-stu-id="9c809-169">Input (Ben):</span></span>

```
:> g.addV('person').property('firstName', 'Ben').property('lastName', 'Miller').property('userid', 4)

```

<span data-ttu-id="9c809-170">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="9c809-170">Output:</span></span>

```
==>[id:ee86b670-4d24-4966-9a39-30529284b66f,label:person,type:vertex,properties:[firstName:[[id:a632469b-30fc-4157-840c-b80260871e9a,value:Ben]],lastName:[[id:4a08d307-0719-47c6-84ae-1b0b06630928,value:Miller]],userid:[[id:ee86b670-4d24-4966-9a39-30529284b66f|userid,value:4]]]]
```

<span data-ttu-id="9c809-171">Bemenet (Jack):</span><span class="sxs-lookup"><span data-stu-id="9c809-171">Input (Jack):</span></span>

```
:> g.addV('person').property('firstName', 'Jack').property('lastName', 'Connor').property('userid', 5)
```

<span data-ttu-id="9c809-172">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="9c809-172">Output:</span></span>

```
==>[id:4c835f2a-ea5b-43bb-9b6b-215488ad8469,label:person,type:vertex,properties:[firstName:[[id:4250824e-4b72-417f-af98-8034aa15559f,value:Jack]],lastName:[[id:44c1d5e1-a831-480a-bf94-5167d133549e,value:Connor]],userid:[[id:4c835f2a-ea5b-43bb-9b6b-215488ad8469|userid,value:5]]]]
```


<span data-ttu-id="9c809-173">Ezután adjunk meg éleket. Ezek a személyek közötti kapcsolatokat jelölik.</span><span class="sxs-lookup"><span data-stu-id="9c809-173">Next, let's add edges for relationships between our people.</span></span>

<span data-ttu-id="9c809-174">Bemenet (Thomas -> Mary Kay):</span><span class="sxs-lookup"><span data-stu-id="9c809-174">Input (Thomas -> Mary Kay):</span></span>

```
:> g.V().hasLabel('person').has('firstName', 'Thomas').addE('knows').to(g.V().hasLabel('person').has('firstName', 'Mary Kay'))
```

<span data-ttu-id="9c809-175">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="9c809-175">Output:</span></span>

```
==>[id:c12bf9fb-96a1-4cb7-a3f8-431e196e702f,label:knows,type:edge,inVLabel:person,outVLabel:person,inV:0d1fa428-780c-49a5-bd3a-a68d96391d5c,outV:1ce821c6-aa3d-4170-a0b7-d14d2a4d18c3]
```

<span data-ttu-id="9c809-176">Bemenet (Thomas -> Robin):</span><span class="sxs-lookup"><span data-stu-id="9c809-176">Input (Thomas -> Robin):</span></span>

```
:> g.V().hasLabel('person').has('firstName', 'Thomas').addE('knows').to(g.V().hasLabel('person').has('firstName', 'Robin'))
```

<span data-ttu-id="9c809-177">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="9c809-177">Output:</span></span>

```
==>[id:58319bdd-1d3e-4f17-a106-0ddf18719d15,label:knows,type:edge,inVLabel:person,outVLabel:person,inV:3e324073-ccfc-4ae1-8675-d450858ca116,outV:1ce821c6-aa3d-4170-a0b7-d14d2a4d18c3]
```

<span data-ttu-id="9c809-178">Bemenet (Robin -> Ben):</span><span class="sxs-lookup"><span data-stu-id="9c809-178">Input (Robin -> Ben):</span></span>

```
:> g.V().hasLabel('person').has('firstName', 'Robin').addE('knows').to(g.V().hasLabel('person').has('firstName', 'Ben'))
```

<span data-ttu-id="9c809-179">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="9c809-179">Output:</span></span>

```
==>[id:889c4d3c-549e-4d35-bc21-a3d1bfa11e00,label:knows,type:edge,inVLabel:person,outVLabel:person,inV:40fd641d-546e-412a-abcc-58fe53891aab,outV:3e324073-ccfc-4ae1-8675-d450858ca116]
```

## <a name="update-a-vertex"></a><span data-ttu-id="9c809-180">Csúcs frissítése</span><span class="sxs-lookup"><span data-stu-id="9c809-180">Update a vertex</span></span>

<span data-ttu-id="9c809-181">Most frissíteni hello *Thomas* rendelkező új korát csúcspont *45*.</span><span class="sxs-lookup"><span data-stu-id="9c809-181">Let's update hello *Thomas* vertex with a new age of *45*.</span></span>

<span data-ttu-id="9c809-182">Bemenet:</span><span class="sxs-lookup"><span data-stu-id="9c809-182">Input:</span></span>
```
:> g.V().hasLabel('person').has('firstName', 'Thomas').property('age', 45)
```
<span data-ttu-id="9c809-183">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="9c809-183">Output:</span></span>

```
==>[id:ae36f938-210e-445a-92df-519f2b64c8ec,label:person,type:vertex,properties:[firstName:[[id:872090b6-6a77-456a-9a55-a59141d4ebc2,value:Thomas]],lastName:[[id:7ee7a39a-a414-4127-89b4-870bc4ef99f3,value:Andersen]],age:[[id:a2a75d5a-ae70-4095-806d-a35abcbfe71d,value:45]]]]
```

## <a name="query-your-graph"></a><span data-ttu-id="9c809-184">Gráf lekérdezése</span><span class="sxs-lookup"><span data-stu-id="9c809-184">Query your graph</span></span>

<span data-ttu-id="9c809-185">Futtassunk néhány lekérdezést a gráfon.</span><span class="sxs-lookup"><span data-stu-id="9c809-185">Now, let's run a variety of queries against your graph.</span></span>

<span data-ttu-id="9c809-186">Első lépésként próbáljuk meg egy lekérdezést egy szűrő tooreturn csak akik régebbi, mint a 40 éves.</span><span class="sxs-lookup"><span data-stu-id="9c809-186">First, let's try a query with a filter tooreturn only people who are older than 40 years old.</span></span>

<span data-ttu-id="9c809-187">Bemenet (szűrőlekérdezés):</span><span class="sxs-lookup"><span data-stu-id="9c809-187">Input (filter query):</span></span>

```
:> g.V().hasLabel('person').has('age', gt(40))
```

<span data-ttu-id="9c809-188">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="9c809-188">Output:</span></span>

```
==>[id:ae36f938-210e-445a-92df-519f2b64c8ec,label:person,type:vertex,properties:[firstName:[[id:872090b6-6a77-456a-9a55-a59141d4ebc2,value:Thomas]],lastName:[[id:7ee7a39a-a414-4127-89b4-870bc4ef99f3,value:Andersen]],age:[[id:a2a75d5a-ae70-4095-806d-a35abcbfe71d,value:45]]]]
```

<span data-ttu-id="9c809-189">Ezt követően most project hello Keresztnév régebbi, mint a 40 éves hello személyek számára.</span><span class="sxs-lookup"><span data-stu-id="9c809-189">Next, let's project hello first name for hello people who are older than 40 years old.</span></span>

<span data-ttu-id="9c809-190">Bemenet (szűrő- és kivetítési lekérdezés):</span><span class="sxs-lookup"><span data-stu-id="9c809-190">Input (filter + projection query):</span></span>

```
:> g.V().hasLabel('person').has('age', gt(40)).values('firstName')
```

<span data-ttu-id="9c809-191">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="9c809-191">Output:</span></span>

```
==>Thomas
```

## <a name="traverse-your-graph"></a><span data-ttu-id="9c809-192">A gráf bejárása</span><span class="sxs-lookup"><span data-stu-id="9c809-192">Traverse your graph</span></span>

<span data-ttu-id="9c809-193">Most bejárása hello graph tooreturn Thomas tartozó ismerősök mindegyikét.</span><span class="sxs-lookup"><span data-stu-id="9c809-193">Let's traverse hello graph tooreturn all of Thomas's friends.</span></span>

<span data-ttu-id="9c809-194">Bemenet (Thomas barátai):</span><span class="sxs-lookup"><span data-stu-id="9c809-194">Input (friends of Thomas):</span></span>

```
:> g.V().hasLabel('person').has('firstName', 'Thomas').outE('knows').inV().hasLabel('person')
```

<span data-ttu-id="9c809-195">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="9c809-195">Output:</span></span> 

```
==>[id:f04bc00b-cb56-46c4-a3bb-a5870c42f7ff,label:person,type:vertex,properties:[firstName:[[id:14feedec-b070-444e-b544-62be15c7167c,value:Mary Kay]],lastName:[[id:107ab421-7208-45d4-b969-bbc54481992a,value:Andersen]],age:[[id:4b08d6e4-58f5-45df-8e69-6b790b692e0a,value:39]]]]
==>[id:91605c63-4988-4b60-9a30-5144719ae326,label:person,type:vertex,properties:[firstName:[[id:f760e0e6-652a-481a-92b0-1767d9bf372e,value:Robin]],lastName:[[id:352a4caa-bad6-47e3-a7dc-90ff342cf870,value:Wakefield]]]]
```

<span data-ttu-id="9c809-196">A következő folytassuk hello következő rétege a csúcsban.</span><span class="sxs-lookup"><span data-stu-id="9c809-196">Next, let's get hello next layer of vertices.</span></span> <span data-ttu-id="9c809-197">Hello graph tooreturn összes hello ismerősök Thomas tartozó ismerősök haladnak át.</span><span class="sxs-lookup"><span data-stu-id="9c809-197">Traverse hello graph tooreturn all hello friends of Thomas's friends.</span></span>

<span data-ttu-id="9c809-198">Bemenet (Thomas barátainak barátai):</span><span class="sxs-lookup"><span data-stu-id="9c809-198">Input (friends of friends of Thomas):</span></span>

```
:> g.V().hasLabel('person').has('firstName', 'Thomas').outE('knows').inV().hasLabel('person').outE('knows').inV().hasLabel('person')
```
<span data-ttu-id="9c809-199">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="9c809-199">Output:</span></span>

```
==>[id:a801a0cb-ee85-44ee-a502-271685ef212e,label:person,type:vertex,properties:[firstName:[[id:b9489902-d29a-4673-8c09-c2b3fe7f8b94,value:Ben]],lastName:[[id:e084f933-9a4b-4dbc-8273-f0171265cf1d,value:Miller]]]]
```

## <a name="drop-a-vertex"></a><span data-ttu-id="9c809-200">Csúcs elvetése</span><span class="sxs-lookup"><span data-stu-id="9c809-200">Drop a vertex</span></span>

<span data-ttu-id="9c809-201">Most most törlése csúcspont hello graph-adatbázisból.</span><span class="sxs-lookup"><span data-stu-id="9c809-201">Let's now delete a vertex from hello graph database.</span></span>

<span data-ttu-id="9c809-202">Bemenet (a Jack csúcs elvetése):</span><span class="sxs-lookup"><span data-stu-id="9c809-202">Input (drop Jack vertex):</span></span>

```
:> g.V().hasLabel('person').has('firstName', 'Jack').drop()
```

## <a name="clear-your-graph"></a><span data-ttu-id="9c809-203">Gráf adatainak törlése</span><span class="sxs-lookup"><span data-stu-id="9c809-203">Clear your graph</span></span>

<span data-ttu-id="9c809-204">Végezetül most hello adatbázisból törölni az összes csúcsban és szélén.</span><span class="sxs-lookup"><span data-stu-id="9c809-204">Finally, let's clear hello database of all vertices and edges.</span></span>

<span data-ttu-id="9c809-205">Bemenet:</span><span class="sxs-lookup"><span data-stu-id="9c809-205">Input:</span></span>

```
:> g.E().drop()
:> g.V().drop()
```

<span data-ttu-id="9c809-206">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="9c809-206">Congratulations!</span></span> <span data-ttu-id="9c809-207">Az Azure Cosmos DB: Graph API-oktatóanyag végére ért.</span><span class="sxs-lookup"><span data-stu-id="9c809-207">You've completed this Azure Cosmos DB: Graph API tutorial!</span></span>

## <a name="review-slas-in-hello-azure-portal"></a><span data-ttu-id="9c809-208">Tekintse át a szolgáltatásiszint-szerződések a hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="9c809-208">Review SLAs in hello Azure portal</span></span>

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a><span data-ttu-id="9c809-209">Az erőforrások eltávolítása</span><span class="sxs-lookup"><span data-stu-id="9c809-209">Clean up resources</span></span>

<span data-ttu-id="9c809-210">Toocontinue toouse az alkalmazás nem fog, ha törli az összes erőforrást hozta létre a gyors üzembe helyezés hello az Azure-portálon az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="9c809-210">If you're not going toocontinue toouse this app, delete all resources created by this quickstart in hello Azure portal with hello following steps:</span></span>  

1. <span data-ttu-id="9c809-211">A hello hello Azure-portálon a bal oldali menüből, kattintson az **erőforráscsoportok** és kattintson a létrehozott hello erőforrás hello nevét.</span><span class="sxs-lookup"><span data-stu-id="9c809-211">From hello left-hand menu in hello Azure portal, click **Resource groups** and then click hello name of hello resource you created.</span></span> 
2. <span data-ttu-id="9c809-212">Az erőforrás csoport lapján kattintson a **törlése**, írja be a hello szövegmező hello erőforrás toodelete hello nevét, és kattintson **törlése**.</span><span class="sxs-lookup"><span data-stu-id="9c809-212">On your resource group page, click **Delete**, type hello name of hello resource toodelete in hello text box, and then click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9c809-213">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9c809-213">Next steps</span></span>

<span data-ttu-id="9c809-214">A gyors üzembe helyezés hogy megtanulta, hogyan toocreate Azure Cosmos DB adatait, hozzon létre egy grafikonon hello adatkezelő használatával, és a csúcsban létrehozása és haladnak át a grafikonon hello Gremlin konzol használatával.</span><span class="sxs-lookup"><span data-stu-id="9c809-214">In this quickstart, you've learned how toocreate an Azure Cosmos DB account, create a graph using hello Data Explorer, create vertices and edges, and traverse your graph using hello Gremlin console.</span></span> <span data-ttu-id="9c809-215">Most már készen áll arra, hogy a Gremlin használatával összetettebb lekérdezéseket hozzon létre és hatékony gráfbejárási logikákat implementáljon.</span><span class="sxs-lookup"><span data-stu-id="9c809-215">You can now build more complex queries and implement powerful graph traversal logic using Gremlin.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="9c809-216">Lekérdezés a Gremlin használatával</span><span class="sxs-lookup"><span data-stu-id="9c809-216">Query using Gremlin</span></span>](tutorial-query-graph.md)