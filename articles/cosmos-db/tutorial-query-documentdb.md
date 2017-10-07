---
title: az SQL Azure Cosmos DB tooquery aaaHow? | Microsoft Docs
description: Ismerje meg az SQL Azure Cosmos DB DocumentDB adatokkal tooquery
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: cosmos-db
ms.custom: tutorial-develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 05/10/2017
ms.author: mimig
ms.openlocfilehash: d3dc51acf92cb78d4f4d9dbac7ec54b1382431cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-how-tooquery-using-sql"></a><span data-ttu-id="46aec-104">A Cosmos DB Azure: Hogyan tooquery SQL használatával?</span><span class="sxs-lookup"><span data-stu-id="46aec-104">Azure Cosmos DB: How tooquery using SQL?</span></span>

<span data-ttu-id="46aec-105">hello Azure Cosmos DB [DocumentDB API](documentdb-introduction.md) SQL használatával dokumentumok lekérdezését támogatja.</span><span class="sxs-lookup"><span data-stu-id="46aec-105">hello Azure Cosmos DB [DocumentDB API](documentdb-introduction.md) supports querying documents using SQL.</span></span> <span data-ttu-id="46aec-106">Ez a cikk ismerteti a minta és két minta az SQL-lekérdezések és dokumentum eredmények.</span><span class="sxs-lookup"><span data-stu-id="46aec-106">This article provides a sample document and two sample SQL queries and results.</span></span>

<span data-ttu-id="46aec-107">Ez a cikk ismerteti a következő feladatok hello:</span><span class="sxs-lookup"><span data-stu-id="46aec-107">This article covers hello following tasks:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="46aec-108">Az SQL adatainak lekérdezése</span><span class="sxs-lookup"><span data-stu-id="46aec-108">Querying data with SQL</span></span>

## <a name="sample-document"></a><span data-ttu-id="46aec-109">A minta-dokumentum</span><span class="sxs-lookup"><span data-stu-id="46aec-109">Sample document</span></span>

<span data-ttu-id="46aec-110">Ebben a cikkben az SQL-lekérdezések hello használja a következő minta dokumentum hello.</span><span class="sxs-lookup"><span data-stu-id="46aec-110">hello SQL queries in this article use hello following sample document.</span></span>

```json
{
  "id": "WakefieldFamily",
  "parents": [
      { "familyName": "Wakefield", "givenName": "Robin" },
      { "familyName": "Miller", "givenName": "Ben" }
  ],
  "children": [
      {
        "familyName": "Merriam", 
        "givenName": "Jesse", 
        "gender": "female", "grade": 1,
        "pets": [
            { "givenName": "Goofy" },
            { "givenName": "Shadow" }
        ]
      },
      { 
        "familyName": "Miller", 
         "givenName": "Lisa", 
         "gender": "female", 
         "grade": 8 }
  ],
  "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
  "creationDate": 1431620462,
  "isRegistered": false
}
```
## <a name="where-can-i-run-sql-queries"></a><span data-ttu-id="46aec-111">Ahol futtathatja az SQL-lekérdezések?</span><span class="sxs-lookup"><span data-stu-id="46aec-111">Where can I run SQL queries?</span></span>

<span data-ttu-id="46aec-112">Az Azure portálon keresztül hello hello adatkezelő hello segítségével lekérdezéseket is futtathat [REST API-t és az SDK-k](documentdb-sdk-dotnet.md), és még akkor is, hello [tesztlekérdezéseket](https://www.documentdb.com/sql/demo), amelyen fut az lekérdezések mintaadatok készlet.</span><span class="sxs-lookup"><span data-stu-id="46aec-112">You can run queries using hello Data Explorer in hello Azure portal, via hello [REST API and SDKs](documentdb-sdk-dotnet.md), and even hello [Query playground](https://www.documentdb.com/sql/demo), which runs queries on an existing set of sample data.</span></span>

<span data-ttu-id="46aec-113">Az SQL-lekérdezések kapcsolatos további információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="46aec-113">For more information about SQL queries, see:</span></span>
* [<span data-ttu-id="46aec-114">SQL-lekérdezést és SQL-szintaxis</span><span class="sxs-lookup"><span data-stu-id="46aec-114">SQL query and SQL syntax</span></span>](documentdb-sql-query.md)

## <a name="prerequisites"></a><span data-ttu-id="46aec-115">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="46aec-115">Prerequisites</span></span>

<span data-ttu-id="46aec-116">Ez az oktatóanyag feltételezi, hogy rendelkezik egy olyan Azure Cosmos DB fiókot és a gyűjteményhez.</span><span class="sxs-lookup"><span data-stu-id="46aec-116">This tutorial assumes you have an Azure Cosmos DB account and collection.</span></span> <span data-ttu-id="46aec-117">Nem rendelkezik egyetlen, az?</span><span class="sxs-lookup"><span data-stu-id="46aec-117">Don't have any of those?</span></span> <span data-ttu-id="46aec-118">Teljes hello [5 perces gyors üzembe helyezés](create-mongodb-nodejs.md) vagy hello [fejlesztői útmutató](tutorial-develop-mongodb.md) toocreate egy olyan fiókot és a gyűjteményhez.</span><span class="sxs-lookup"><span data-stu-id="46aec-118">Complete hello [5-minute quickstart](create-mongodb-nodejs.md) or hello [developer tutorial](tutorial-develop-mongodb.md) toocreate an account and collection.</span></span>

## <a name="example-query-1"></a><span data-ttu-id="46aec-119">1. példa lekérdezés</span><span class="sxs-lookup"><span data-stu-id="46aec-119">Example query 1</span></span>

<span data-ttu-id="46aec-120">Megadott hello minta termékcsalád a dokumentum fenti, a következő SQL-lekérdezés visszaad hello Ha hello id mezője megegyezik `WakefieldFamily`.</span><span class="sxs-lookup"><span data-stu-id="46aec-120">Given hello sample family document above, following SQL query returns hello documents where hello id field matches `WakefieldFamily`.</span></span> <span data-ttu-id="46aec-121">Mivel ez egy `SELECT *` , hello lekérdezés kimenetét hello utasítás hello teljes JSON-dokumentum:</span><span class="sxs-lookup"><span data-stu-id="46aec-121">Since it's a `SELECT *` statement, hello output of hello query is hello complete JSON document:</span></span>

<span data-ttu-id="46aec-122">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="46aec-122">**Query**</span></span>

    SELECT * 
    FROM Families f 
    WHERE f.id = "WakefieldFamily"

<span data-ttu-id="46aec-123">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="46aec-123">**Results**</span></span>

```json
{
  "id": "WakefieldFamily",
  "parents": [
      { "familyName": "Wakefield", "givenName": "Robin" },
      { "familyName": "Miller", "givenName": "Ben" }
  ],
  "children": [
      {
        "familyName": "Merriam", 
        "givenName": "Jesse", 
        "gender": "female", "grade": 1,
        "pets": [
            { "givenName": "Goofy" },
            { "givenName": "Shadow" }
        ]
      },
      { 
        "familyName": "Miller", 
         "givenName": "Lisa", 
         "gender": "female", 
         "grade": 8 }
  ],
  "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
  "creationDate": 1431620462,
  "isRegistered": false
}
```

## <a name="example-query-2"></a><span data-ttu-id="46aec-124">Példalekérdezés 2</span><span class="sxs-lookup"><span data-stu-id="46aec-124">Example query 2</span></span>

<span data-ttu-id="46aec-125">hello tovább lekérdezés visszaadja az összes hello nevet kapnak gyermekek hello termékcsalád, amelynek azonosítója megegyezik `WakefieldFamily` a besorolási rendezve.</span><span class="sxs-lookup"><span data-stu-id="46aec-125">hello next query returns all hello given names of children in hello family whose id matches `WakefieldFamily` ordered by their grade.</span></span>

<span data-ttu-id="46aec-126">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="46aec-126">**Query**</span></span>

    SELECT c.givenName 
    FROM Families f 
    JOIN c IN f.children 
    WHERE f.id = 'WakefieldFamily'
    ORDER BY f.children.grade ASC

<span data-ttu-id="46aec-127">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="46aec-127">**Results**</span></span>

    [
      { "givenName": "Jesse" }, 
      { "givenName": "Lisa"}
    ]


## <a name="next-steps"></a><span data-ttu-id="46aec-128">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="46aec-128">Next steps</span></span>

<span data-ttu-id="46aec-129">Ebben az oktatóanyagban hello következő régebben már kötöttek:</span><span class="sxs-lookup"><span data-stu-id="46aec-129">In this tutorial, you've done hello following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="46aec-130">Megtudta, hogyan tooquery SQL használatával</span><span class="sxs-lookup"><span data-stu-id="46aec-130">Learned how tooquery using SQL</span></span>  

<span data-ttu-id="46aec-131">Most már folytathatja toohello következő útmutató toolearn hogyan toodistribute az adatok globálisan.</span><span class="sxs-lookup"><span data-stu-id="46aec-131">You can now proceed toohello next tutorial toolearn how toodistribute your data globally.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="46aec-132">Az adatok globálisan terjesztése</span><span class="sxs-lookup"><span data-stu-id="46aec-132">Distribute your data globally</span></span>](tutorial-global-distribution-documentdb.md)

