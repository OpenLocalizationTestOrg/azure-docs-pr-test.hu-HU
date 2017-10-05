---
title: "Hogyan lehet lekérdezni az SQL Azure Cosmos DB? | Microsoft Docs"
description: "Ismerje meg az SQL Azure Cosmos DB DocumentDB adatokkal lekérdezése"
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
ms.openlocfilehash: a2a562c06c6302b9548e758b4c6754ec13b6001d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-cosmos-db-how-to-query-using-sql"></a><span data-ttu-id="b40b6-104">Azure Cosmos DB: Hogyan lekérdezés SQL használatával?</span><span class="sxs-lookup"><span data-stu-id="b40b6-104">Azure Cosmos DB: How to query using SQL?</span></span>

<span data-ttu-id="b40b6-105">Az Azure Cosmos DB [DocumentDB API](documentdb-introduction.md) SQL használatával dokumentumok lekérdezését támogatja.</span><span class="sxs-lookup"><span data-stu-id="b40b6-105">The Azure Cosmos DB [DocumentDB API](documentdb-introduction.md) supports querying documents using SQL.</span></span> <span data-ttu-id="b40b6-106">Ez a cikk ismerteti a minta és két minta az SQL-lekérdezések és dokumentum eredmények.</span><span class="sxs-lookup"><span data-stu-id="b40b6-106">This article provides a sample document and two sample SQL queries and results.</span></span>

<span data-ttu-id="b40b6-107">Ez a cikk ismerteti a következő feladatokat:</span><span class="sxs-lookup"><span data-stu-id="b40b6-107">This article covers the following tasks:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="b40b6-108">Az SQL adatainak lekérdezése</span><span class="sxs-lookup"><span data-stu-id="b40b6-108">Querying data with SQL</span></span>

## <a name="sample-document"></a><span data-ttu-id="b40b6-109">A minta-dokumentum</span><span class="sxs-lookup"><span data-stu-id="b40b6-109">Sample document</span></span>

<span data-ttu-id="b40b6-110">Ebben a cikkben az SQL-lekérdezések használata a következő minta dokumentumot.</span><span class="sxs-lookup"><span data-stu-id="b40b6-110">The SQL queries in this article use the following sample document.</span></span>

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
## <a name="where-can-i-run-sql-queries"></a><span data-ttu-id="b40b6-111">Ahol futtathatja az SQL-lekérdezések?</span><span class="sxs-lookup"><span data-stu-id="b40b6-111">Where can I run SQL queries?</span></span>

<span data-ttu-id="b40b6-112">Az Azure-portálon az adatkezelő használatával keresztül lekérdezéseket is futtathat a [REST API-t és az SDK-k](documentdb-sdk-dotnet.md), és még a [tesztlekérdezéseket](https://www.documentdb.com/sql/demo), amelyen fut az lekérdezések mintaadatok készlet.</span><span class="sxs-lookup"><span data-stu-id="b40b6-112">You can run queries using the Data Explorer in the Azure portal, via the [REST API and SDKs](documentdb-sdk-dotnet.md), and even the [Query playground](https://www.documentdb.com/sql/demo), which runs queries on an existing set of sample data.</span></span>

<span data-ttu-id="b40b6-113">Az SQL-lekérdezések kapcsolatos további információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="b40b6-113">For more information about SQL queries, see:</span></span>
* [<span data-ttu-id="b40b6-114">SQL-lekérdezést és SQL-szintaxis</span><span class="sxs-lookup"><span data-stu-id="b40b6-114">SQL query and SQL syntax</span></span>](documentdb-sql-query.md)

## <a name="prerequisites"></a><span data-ttu-id="b40b6-115">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="b40b6-115">Prerequisites</span></span>

<span data-ttu-id="b40b6-116">Ez az oktatóanyag feltételezi, hogy rendelkezik egy olyan Azure Cosmos DB fiókot és a gyűjteményhez.</span><span class="sxs-lookup"><span data-stu-id="b40b6-116">This tutorial assumes you have an Azure Cosmos DB account and collection.</span></span> <span data-ttu-id="b40b6-117">Nem rendelkezik egyetlen, az?</span><span class="sxs-lookup"><span data-stu-id="b40b6-117">Don't have any of those?</span></span> <span data-ttu-id="b40b6-118">Fejezze be a [5 perces gyors üzembe helyezés](create-mongodb-nodejs.md) vagy a [fejlesztői útmutató](tutorial-develop-mongodb.md) egy olyan fiókot és a gyűjtemény létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="b40b6-118">Complete the [5-minute quickstart](create-mongodb-nodejs.md) or the [developer tutorial](tutorial-develop-mongodb.md) to create an account and collection.</span></span>

## <a name="example-query-1"></a><span data-ttu-id="b40b6-119">1. példa lekérdezés</span><span class="sxs-lookup"><span data-stu-id="b40b6-119">Example query 1</span></span>

<span data-ttu-id="b40b6-120">A minta termékcsalád dokumentum fenti megadott, a következő SQL-lekérdezésben adja vissza a dokumentumok Ha az azonosítót tartalmazó mezőt megegyezik `WakefieldFamily`.</span><span class="sxs-lookup"><span data-stu-id="b40b6-120">Given the sample family document above, following SQL query returns the documents where the id field matches `WakefieldFamily`.</span></span> <span data-ttu-id="b40b6-121">Mivel ez egy `SELECT *` nyilatkozat, a lekérdezés kimenetét a teljes JSON-dokumentum is:</span><span class="sxs-lookup"><span data-stu-id="b40b6-121">Since it's a `SELECT *` statement, the output of the query is the complete JSON document:</span></span>

<span data-ttu-id="b40b6-122">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="b40b6-122">**Query**</span></span>

    SELECT * 
    FROM Families f 
    WHERE f.id = "WakefieldFamily"

<span data-ttu-id="b40b6-123">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="b40b6-123">**Results**</span></span>

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

## <a name="example-query-2"></a><span data-ttu-id="b40b6-124">Példalekérdezés 2</span><span class="sxs-lookup"><span data-stu-id="b40b6-124">Example query 2</span></span>

<span data-ttu-id="b40b6-125">A következő lekérdezés gyermekek minden megadott nevét adja vissza a termékcsalád, amelynek azonosítója megegyezik `WakefieldFamily` a besorolási rendezve.</span><span class="sxs-lookup"><span data-stu-id="b40b6-125">The next query returns all the given names of children in the family whose id matches `WakefieldFamily` ordered by their grade.</span></span>

<span data-ttu-id="b40b6-126">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="b40b6-126">**Query**</span></span>

    SELECT c.givenName 
    FROM Families f 
    JOIN c IN f.children 
    WHERE f.id = 'WakefieldFamily'
    ORDER BY f.children.grade ASC

<span data-ttu-id="b40b6-127">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="b40b6-127">**Results**</span></span>

    [
      { "givenName": "Jesse" }, 
      { "givenName": "Lisa"}
    ]


## <a name="next-steps"></a><span data-ttu-id="b40b6-128">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b40b6-128">Next steps</span></span>

<span data-ttu-id="b40b6-129">Ebben az oktatóanyagban ezt a következők:</span><span class="sxs-lookup"><span data-stu-id="b40b6-129">In this tutorial, you've done the following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b40b6-130">Megtudta, hogyan lekérdezés SQL használatával</span><span class="sxs-lookup"><span data-stu-id="b40b6-130">Learned how to query using SQL</span></span>  

<span data-ttu-id="b40b6-131">Most már folytathatja a következő oktatóanyag megtudhatja, miként ossza el az adatokat globális.</span><span class="sxs-lookup"><span data-stu-id="b40b6-131">You can now proceed to the next tutorial to learn how to distribute your data globally.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="b40b6-132">Az adatok globálisan terjesztése</span><span class="sxs-lookup"><span data-stu-id="b40b6-132">Distribute your data globally</span></span>](tutorial-global-distribution-documentdb.md)

