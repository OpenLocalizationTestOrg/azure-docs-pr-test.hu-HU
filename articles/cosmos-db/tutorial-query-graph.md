---
title: "aaaHow tooquery graph adatokat az Adatbázisba az Azure Cosmos? | Microsoft Docs"
description: "További tudnivalók az Azure Cosmos Adatbázisba tooquery Diagramadatok"
services: cosmos-db
documentationcenter: 
author: dennyglee
manager: jhubbard
editor: 
tags: 
ms.assetid: 8bde5c80-581c-4f70-acb4-9578873c92fa
ms.service: cosmos-db
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 05/10/2017
ms.author: denlee
ms.openlocfilehash: fdde881edd6c488e2fea51e5c9665e1d736009fa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-how-tooquery-with-hello-graph-api-preview"></a><span data-ttu-id="49f3f-104">A Cosmos DB Azure: Hogyan tooquery a hello Graph API-val (előzetes verzió)?</span><span class="sxs-lookup"><span data-stu-id="49f3f-104">Azure Cosmos DB: How tooquery with hello Graph API (preview)?</span></span>

<span data-ttu-id="49f3f-105">hello Azure Cosmos DB [Graph API](graph-introduction.md) (előzetes verzió) támogatja a [Gremlin](https://docs.mongodb.com/manual/tutorial/query-documents/) lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="49f3f-105">hello Azure Cosmos DB [Graph API](graph-introduction.md) (preview) supports [Gremlin](https://docs.mongodb.com/manual/tutorial/query-documents/) queries.</span></span> <span data-ttu-id="49f3f-106">Ez a cikk minta dokumentumok biztosít, és lekérdezi a tooget-t elindította.</span><span class="sxs-lookup"><span data-stu-id="49f3f-106">This article provides sample documents and queries tooget you started.</span></span> <span data-ttu-id="49f3f-107">Részletes Gremlin hivatkozás megtalálható hello [Gremlin támogatási](gremlin-support.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="49f3f-107">A detailed Gremlin reference is provided in hello [Gremlin support](gremlin-support.md) article.</span></span>

<span data-ttu-id="49f3f-108">Ez a cikk ismerteti a következő feladatok hello:</span><span class="sxs-lookup"><span data-stu-id="49f3f-108">This article covers hello following tasks:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="49f3f-109">Gremlin az adatok lekérdezése</span><span class="sxs-lookup"><span data-stu-id="49f3f-109">Querying data with Gremlin</span></span>

## <a name="prerequisites"></a><span data-ttu-id="49f3f-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="49f3f-110">Prerequisites</span></span>

<span data-ttu-id="49f3f-111">Az alábbi lekérdezéseket toowork van Azure Cosmos DB-fiókja és Diagramadatok hello tárolóban van.</span><span class="sxs-lookup"><span data-stu-id="49f3f-111">For these queries toowork, you must have an Azure Cosmos DB account and have graph data in hello container.</span></span> <span data-ttu-id="49f3f-112">Nem rendelkezik egyetlen, az?</span><span class="sxs-lookup"><span data-stu-id="49f3f-112">Don't have any of those?</span></span> <span data-ttu-id="49f3f-113">Teljes hello [5 perces gyors üzembe helyezés](create-graph-dotnet.md) vagy hello [fejlesztői útmutató](tutorial-query-graph.md) toocreate egy fiókot, és töltse fel az adatbázist.</span><span class="sxs-lookup"><span data-stu-id="49f3f-113">Complete hello [5-minute quickstart](create-graph-dotnet.md) or hello [developer tutorial](tutorial-query-graph.md) toocreate an account and populate your database.</span></span> <span data-ttu-id="49f3f-114">Futtathatja a következő lekérdezést hello hello [Azure Cosmos DB .NET graph könyvtár](graph-sdk-dotnet.md), [Gremlin konzol](https://tinkerpop.apache.org/docs/current/reference/#gremlin-console), vagy a kedvenc Gremlin illesztőprogramot.</span><span class="sxs-lookup"><span data-stu-id="49f3f-114">You can run hello following queries using hello [Azure Cosmos DB .NET graph library](graph-sdk-dotnet.md), [Gremlin console](https://tinkerpop.apache.org/docs/current/reference/#gremlin-console), or your favorite Gremlin driver.</span></span>

## <a name="count-vertices-in-hello-graph"></a><span data-ttu-id="49f3f-115">Count csúcsban hello Graph</span><span class="sxs-lookup"><span data-stu-id="49f3f-115">Count vertices in hello graph</span></span>

<span data-ttu-id="49f3f-116">a következő kódrészletet hello bemutatja, hogyan toocount hello hello Graph csúcsban száma:</span><span class="sxs-lookup"><span data-stu-id="49f3f-116">hello following snippet shows how toocount hello number of vertices in hello graph:</span></span>

```
g.V().count()
```

## <a name="filters"></a><span data-ttu-id="49f3f-117">Szűrők</span><span class="sxs-lookup"><span data-stu-id="49f3f-117">Filters</span></span>

<span data-ttu-id="49f3f-118">Szűrők Gremlin tartozó használatával végezheti el `has` és `hasLabel` lépéseit, és egyesíthet használatával `and`, `or`, és `not` toobuild összetettebb szűrők.</span><span class="sxs-lookup"><span data-stu-id="49f3f-118">You can perform filters using Gremlin's `has` and `hasLabel` steps, and combine them using `and`, `or`, and `not` toobuild more complex filters.</span></span> <span data-ttu-id="49f3f-119">Azure Cosmos DB biztosít a csúcsban és gyors lekérdezések fok belül az összes tulajdonság séma-független indexelése:</span><span class="sxs-lookup"><span data-stu-id="49f3f-119">Azure Cosmos DB provides schema-agnostic indexing of all properties within your vertices and degrees for fast queries:</span></span>

```
g.V().hasLabel('person').has('age', gt(40))
```

## <a name="projection"></a><span data-ttu-id="49f3f-120">Leképezése</span><span class="sxs-lookup"><span data-stu-id="49f3f-120">Projection</span></span>

<span data-ttu-id="49f3f-121">Kivetítheti az egyes tulajdonságok hello lekérdezés eredményében hello segítségével `values` . lépés:</span><span class="sxs-lookup"><span data-stu-id="49f3f-121">You can project certain properties in hello query results using hello `values` step:</span></span>

```
g.V().hasLabel('person').values('firstName')
```

## <a name="find-related-edges-and-vertices"></a><span data-ttu-id="49f3f-122">Kapcsolódó szélek és csúcsban keresése</span><span class="sxs-lookup"><span data-stu-id="49f3f-122">Find related edges and vertices</span></span>

<span data-ttu-id="49f3f-123">Eddig is csak láttuk lekérdezési operátorok, amelyek működnek a bármely adatbázis.</span><span class="sxs-lookup"><span data-stu-id="49f3f-123">So far, we've only seen query operators that work in any database.</span></span> <span data-ttu-id="49f3f-124">Diagramok esetén gyors és hatékony átjárás műveletekhez toonavigate toorelated szélek és csúcsban van szükség.</span><span class="sxs-lookup"><span data-stu-id="49f3f-124">Graphs are fast and efficient for traversal operations when you need toonavigate toorelated edges and vertices.</span></span> <span data-ttu-id="49f3f-125">Keressük Thomas összes barátok.</span><span class="sxs-lookup"><span data-stu-id="49f3f-125">Let's find all friends of Thomas.</span></span> <span data-ttu-id="49f3f-126">Jelenleg ezt úgy teheti meg Gremlin `outE` toofind összes hello kimenő éleinek Thomas a lépést, majd toohello a-csúcsban áthaladó Gremlin tartozó használatával szegélyek a `inV` . lépés:</span><span class="sxs-lookup"><span data-stu-id="49f3f-126">We do this by using Gremlin's `outE` step toofind all hello out-edges from Thomas, then traversing toohello in-vertices from those edges using Gremlin's `inV` step:</span></span>

```cs
g.V('thomas').outE('knows').inV().hasLabel('person')
```

<span data-ttu-id="49f3f-127">hello a következő lekérdezés hajt végre két ugrások toofind összes Thomas' "ismerősök olyan ismerőseinek", meghívásával `outE` és `inV` kétszer.</span><span class="sxs-lookup"><span data-stu-id="49f3f-127">hello next query performs two hops toofind all of Thomas' "friends of friends", by calling `outE` and `inV` two times.</span></span> 

```cs
g.V('thomas').outE('knows').inV().hasLabel('person').outE('knows').inV().hasLabel('person')
```

<span data-ttu-id="49f3f-128">Összetettebb lekérdezések létrehozhatja és hatékony graph átjárás logika használatával Gremlin, többek között a következőket keverési szűrő kifejezések használatával ismétlési végrehajtása hello megvalósítása `loop` lépést, és végrehajtási feltételes navigációs hello segítségével `choose` lépés.</span><span class="sxs-lookup"><span data-stu-id="49f3f-128">You can build more complex queries and implement powerful graph traversal logic using Gremlin, including mixing filter expressions, performing looping using hello `loop` step, and implementing conditional navigation using hello `choose` step.</span></span> <span data-ttu-id="49f3f-129">További információ a teendők [Gremlin támogatási](gremlin-support.md)!</span><span class="sxs-lookup"><span data-stu-id="49f3f-129">Learn more about what you can do with [Gremlin support](gremlin-support.md)!</span></span>

## <a name="next-steps"></a><span data-ttu-id="49f3f-130">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="49f3f-130">Next steps</span></span>

<span data-ttu-id="49f3f-131">Ebben az oktatóanyagban hello következő régebben már kötöttek:</span><span class="sxs-lookup"><span data-stu-id="49f3f-131">In this tutorial, you've done hello following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="49f3f-132">Megtudta, hogyan tooquery Graph használatával</span><span class="sxs-lookup"><span data-stu-id="49f3f-132">Learned how tooquery using Graph</span></span> 

<span data-ttu-id="49f3f-133">Most már folytathatja toohello következő útmutató toolearn hogyan toodistribute az adatok globálisan.</span><span class="sxs-lookup"><span data-stu-id="49f3f-133">You can now proceed toohello next tutorial toolearn how toodistribute your data globally.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="49f3f-134">Az adatok globálisan terjesztése</span><span class="sxs-lookup"><span data-stu-id="49f3f-134">Distribute your data globally</span></span>](tutorial-global-distribution-documentdb.md)