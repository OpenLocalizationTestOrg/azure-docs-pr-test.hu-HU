---
title: "Azure Cosmos DB: Hogyan lekérdezés a DocumentDB API használatával? | Microsoft Docs"
description: "Ismerje meg, a DocumentDB API-t az Azure Cosmos DB lekérdezése"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: cosmos-db
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 05/10/2017
ms.author: mimig
ms.openlocfilehash: feffc553a9aa931d96cec71c101674fce08a466b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-cosmos-db-how-to-query-with-api-for-mongodb"></a><span data-ttu-id="be3eb-104">Azure Cosmos DB: Hogyan lekérdezni az API-JÁVAL a MongoDB?</span><span class="sxs-lookup"><span data-stu-id="be3eb-104">Azure Cosmos DB: How to query with API for MongoDB?</span></span>

<span data-ttu-id="be3eb-105">Az Azure Cosmos DB [API-t a MongoDB](mongodb-introduction.md) támogatja [MongoDB rendszerhéj lekérdezések](https://docs.mongodb.com/manual/tutorial/query-documents/).</span><span class="sxs-lookup"><span data-stu-id="be3eb-105">The Azure Cosmos DB [API for MongoDB](mongodb-introduction.md) supports [MongoDB shell queries](https://docs.mongodb.com/manual/tutorial/query-documents/).</span></span> 

<span data-ttu-id="be3eb-106">Ez a cikk ismerteti a következő feladatokat:</span><span class="sxs-lookup"><span data-stu-id="be3eb-106">This article covers the following tasks:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="be3eb-107">A MongoDB-vel adatok lekérdezése</span><span class="sxs-lookup"><span data-stu-id="be3eb-107">Querying data with MongoDB</span></span>

## <a name="sample-document"></a><span data-ttu-id="be3eb-108">A minta-dokumentum</span><span class="sxs-lookup"><span data-stu-id="be3eb-108">Sample document</span></span>

<span data-ttu-id="be3eb-109">Ebben a cikkben a lekérdezések használja az alábbi minta-dokumentum.</span><span class="sxs-lookup"><span data-stu-id="be3eb-109">The queries in this article use the following sample document.</span></span>

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
## <span data-ttu-id="be3eb-110"><a id="examplequery1"></a>1. példa lekérdezés</span><span class="sxs-lookup"><span data-stu-id="be3eb-110"><a id="examplequery1"></a> Example query 1</span></span> 

<span data-ttu-id="be3eb-111">A minta termékcsalád dokumentum fenti megadott, a következő lekérdezés adja vissza a dokumentumok Ha az azonosítót tartalmazó mezőt megegyezik `WakefieldFamily`.</span><span class="sxs-lookup"><span data-stu-id="be3eb-111">Given the sample family document above, the following query returns the documents where the id field matches `WakefieldFamily`.</span></span>

<span data-ttu-id="be3eb-112">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="be3eb-112">**Query**</span></span>
    
    db.families.find({ id: “WakefieldFamily”})

<span data-ttu-id="be3eb-113">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="be3eb-113">**Results**</span></span>

    {
    "_id": "ObjectId(\"58f65e1198f3a12c7090e68c\")",
    "id": "WakefieldFamily",
    "parents": [
      {
        "familyName": "Wakefield",
        "givenName": "Robin"
      },
      {
        "familyName": "Miller",
        "givenName": "Ben"
      }
    ],
    "children": [
      {
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female",
        "grade": 1,
        "pets": [
          { "givenName": "Goofy" },
          { "givenName": "Shadow" }
        ]
      },
      {
        "familyName": "Miller",
        "givenName": "Lisa",
        "gender": "female",
        "grade": 8
      }
    ],
    "address": {
      "state": "NY",
      "county": "Manhattan",
      "city": "NY"
    },
    "creationDate": 1431620462,
    "isRegistered": false
    }

## <span data-ttu-id="be3eb-114"><a id="examplequery2"></a>Példalekérdezés 2</span><span class="sxs-lookup"><span data-stu-id="be3eb-114"><a id="examplequery2"></a>Example query 2</span></span> 

<span data-ttu-id="be3eb-115">A következő lekérdezést a termékcsalád összes gyermekeit adja vissza.</span><span class="sxs-lookup"><span data-stu-id="be3eb-115">The next query returns all the children in the family.</span></span> 

<span data-ttu-id="be3eb-116">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="be3eb-116">**Query**</span></span>
    
    db.familes.find( { id: “WakefieldFamily” }, { children: true } )

<span data-ttu-id="be3eb-117">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="be3eb-117">**Results**</span></span>

    {
    "_id": "ObjectId("58f65e1198f3a12c7090e68c")",
    "children": [
      {
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female",
        "grade": 1,
        "pets": [
          { "givenName": "Goofy" },
          { "givenName": "Shadow" }
        ]
      },
      {
        "familyName": "Miller",
        "givenName": "Lisa",
        "gender": "female",
        "grade": 8
      }
    ]
    }


## <span data-ttu-id="be3eb-118"><a id="examplequery3"></a>Példalekérdezés 3</span><span class="sxs-lookup"><span data-stu-id="be3eb-118"><a id="examplequery3"></a>Example query 3</span></span> 

<span data-ttu-id="be3eb-119">A következő lekérdezés a bejegyzett családok adja vissza.</span><span class="sxs-lookup"><span data-stu-id="be3eb-119">The next query returns all the families which are registered.</span></span> 

<span data-ttu-id="be3eb-120">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="be3eb-120">**Query**</span></span>
    
    db.families.find( { "isRegistered" : true })
<span data-ttu-id="be3eb-121">**Eredmények** nincs dokumentum adja vissza.</span><span class="sxs-lookup"><span data-stu-id="be3eb-121">**Results** No document will be returned.</span></span> 

## <span data-ttu-id="be3eb-122"><a id="examplequery4"></a>Példalekérdezés 4</span><span class="sxs-lookup"><span data-stu-id="be3eb-122"><a id="examplequery4"></a>Example query 4</span></span>

<span data-ttu-id="be3eb-123">A következő lekérdezés visszaadja az összes családok, amelyek nincsenek regisztrálva.</span><span class="sxs-lookup"><span data-stu-id="be3eb-123">The next query returns all the families which are not registered.</span></span> 

<span data-ttu-id="be3eb-124">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="be3eb-124">**Query**</span></span>
    
    db.families.find( { "isRegistered" : false })
<span data-ttu-id="be3eb-125">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="be3eb-125">**Results**</span></span>

     {
    "_id": ObjectId("58f65e1198f3a12c7090e68c"),
    "id": "WakefieldFamily",
    "parents": [{
        "familyName": "Wakefield",
        "givenName": "Robin"
    }, {
        "familyName": "Miller",
        "givenName": "Ben"
    }],
    "children": [{
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female",
        "grade": 1,
        "pets": [{
            "givenName": "Goofy"
        }, {
            "givenName": "Shadow"
        }]
    }, {
        "familyName": "Miller",
        "givenName": "Lisa",
        "gender": "female",
        "grade": 8
    }],
    "address": {
        "state": "NY",
        "county": "Manhattan",
        "city": "NY"
    },
    "creationDate": 1431620462,
    "isRegistered": false
<span data-ttu-id="be3eb-126">}</span><span class="sxs-lookup"><span data-stu-id="be3eb-126">}</span></span>

## <span data-ttu-id="be3eb-127"><a id="examplequery5"></a>Példalekérdezés 5</span><span class="sxs-lookup"><span data-stu-id="be3eb-127"><a id="examplequery5"></a>Example query 5</span></span>

<span data-ttu-id="be3eb-128">A következő lekérdezés adja vissza a families, amelyek nincsenek regisztrálva és állapot NY.</span><span class="sxs-lookup"><span data-stu-id="be3eb-128">The next query returns all the families which are not registered and state is NY.</span></span> 

<span data-ttu-id="be3eb-129">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="be3eb-129">**Query**</span></span>
    
     db.families.find( { "isRegistered" : false, "address.state" : "NY" })

<span data-ttu-id="be3eb-130">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="be3eb-130">**Results**</span></span>

     {
    "_id": ObjectId("58f65e1198f3a12c7090e68c"),
    "id": "WakefieldFamily",
    "parents": [{
        "familyName": "Wakefield",
        "givenName": "Robin"
    }, {
        "familyName": "Miller",
        "givenName": "Ben"
    }],
    "children": [{
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female",
        "grade": 1,
        "pets": [{
            "givenName": "Goofy"
        }, {
            "givenName": "Shadow"
        }]
    }, {
        "familyName": "Miller",
        "givenName": "Lisa",
        "gender": "female",
        "grade": 8
    }],
    "address": {
        "state": "NY",
        "county": "Manhattan",
        "city": "NY"
    },
    "creationDate": 1431620462,
    "isRegistered": false
<span data-ttu-id="be3eb-131">}</span><span class="sxs-lookup"><span data-stu-id="be3eb-131">}</span></span>


## <span data-ttu-id="be3eb-132"><a id="examplequery6"></a>Példalekérdezés 6</span><span class="sxs-lookup"><span data-stu-id="be3eb-132"><a id="examplequery6"></a>Example query 6</span></span>

<span data-ttu-id="be3eb-133">A következő lekérdezés értéket ad vissza, a családok ahol gyermekek besorolási 8.</span><span class="sxs-lookup"><span data-stu-id="be3eb-133">The next query returns all the families where children grades are 8.</span></span>

<span data-ttu-id="be3eb-134">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="be3eb-134">**Query**</span></span>
  
     db.families.find( { children : { $elemMatch: { grade : 8 }} } )

<span data-ttu-id="be3eb-135">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="be3eb-135">**Results**</span></span>

     {
    "_id": ObjectId("58f65e1198f3a12c7090e68c"),
    "id": "WakefieldFamily",
    "parents": [{
        "familyName": "Wakefield",
        "givenName": "Robin"
    }, {
        "familyName": "Miller",
        "givenName": "Ben"
    }],
    "children": [{
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female",
        "grade": 1,
        "pets": [{
            "givenName": "Goofy"
        }, {
            "givenName": "Shadow"
        }]
    }, {
        "familyName": "Miller",
        "givenName": "Lisa",
        "gender": "female",
        "grade": 8
    }],
    "address": {
        "state": "NY",
        "county": "Manhattan",
        "city": "NY"
    },
    "creationDate": 1431620462,
    "isRegistered": false
<span data-ttu-id="be3eb-136">}</span><span class="sxs-lookup"><span data-stu-id="be3eb-136">}</span></span>

## <span data-ttu-id="be3eb-137"><a id="examplequery7"></a>Példalekérdezés 7</span><span class="sxs-lookup"><span data-stu-id="be3eb-137"><a id="examplequery7"></a>Example query 7</span></span>

<span data-ttu-id="be3eb-138">A következő lekérdezés adja vissza a families, ahol gyermekek tömb mérete 3.</span><span class="sxs-lookup"><span data-stu-id="be3eb-138">The next query returns all the families where size of children array is 3.</span></span>

<span data-ttu-id="be3eb-139">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="be3eb-139">**Query**</span></span>
  
      db.Family.find( {children: { $size:3} } )

<span data-ttu-id="be3eb-140">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="be3eb-140">**Results**</span></span>

<span data-ttu-id="be3eb-141">Jelenleg nincs 2-nél több gyermekek visszatér nem járt eredménnyel.</span><span class="sxs-lookup"><span data-stu-id="be3eb-141">No results will be returned as we do not have more than 2 children.</span></span> <span data-ttu-id="be3eb-142">Csak akkor, ha a paraméter értéke 2 Ez a lekérdezés sikeresen befejeződik, és térjen vissza a teljes dokumentumot.</span><span class="sxs-lookup"><span data-stu-id="be3eb-142">Only when parameter is 2 this query will succeed and return the full document.</span></span>

## <a name="next-steps"></a><span data-ttu-id="be3eb-143">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="be3eb-143">Next steps</span></span>

<span data-ttu-id="be3eb-144">Ebben az oktatóanyagban ezt a következők:</span><span class="sxs-lookup"><span data-stu-id="be3eb-144">In this tutorial, you've done the following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="be3eb-145">Megtudta, hogyan lekérdezés MongoDB használatával</span><span class="sxs-lookup"><span data-stu-id="be3eb-145">Learned how to query using MongoDB</span></span> 

<span data-ttu-id="be3eb-146">Most már folytathatja a következő oktatóanyag megtudhatja, miként ossza el az adatokat globális.</span><span class="sxs-lookup"><span data-stu-id="be3eb-146">You can now proceed to the next tutorial to learn how to distribute your data globally.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="be3eb-147">Az adatok globálisan terjesztése</span><span class="sxs-lookup"><span data-stu-id="be3eb-147">Distribute your data globally</span></span>](tutorial-global-distribution-documentdb.md)

