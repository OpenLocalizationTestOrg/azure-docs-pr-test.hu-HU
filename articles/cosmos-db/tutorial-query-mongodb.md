---
title: "A Cosmos DB Azure: Hogyan tooquery használatával hello DocumentDB API? | Microsoft Docs"
description: "A DocumentDB API hello tooquery Azure Cosmos DB megismerése"
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
ms.openlocfilehash: e3e5a49f7510942bcfb15330e5f86c5dd8b1e5d9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-how-tooquery-with-api-for-mongodb"></a><span data-ttu-id="6c59f-104">A Cosmos DB Azure: Hogyan tooquery mongodb API-t?</span><span class="sxs-lookup"><span data-stu-id="6c59f-104">Azure Cosmos DB: How tooquery with API for MongoDB?</span></span>

<span data-ttu-id="6c59f-105">hello Azure Cosmos DB [API-t a MongoDB](mongodb-introduction.md) támogatja [MongoDB rendszerhéj lekérdezések](https://docs.mongodb.com/manual/tutorial/query-documents/).</span><span class="sxs-lookup"><span data-stu-id="6c59f-105">hello Azure Cosmos DB [API for MongoDB](mongodb-introduction.md) supports [MongoDB shell queries](https://docs.mongodb.com/manual/tutorial/query-documents/).</span></span> 

<span data-ttu-id="6c59f-106">Ez a cikk ismerteti a következő feladatok hello:</span><span class="sxs-lookup"><span data-stu-id="6c59f-106">This article covers hello following tasks:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="6c59f-107">A MongoDB-vel adatok lekérdezése</span><span class="sxs-lookup"><span data-stu-id="6c59f-107">Querying data with MongoDB</span></span>

## <a name="sample-document"></a><span data-ttu-id="6c59f-108">A minta-dokumentum</span><span class="sxs-lookup"><span data-stu-id="6c59f-108">Sample document</span></span>

<span data-ttu-id="6c59f-109">Ebben a cikkben hello lekérdezések használja a következő minta dokumentum hello.</span><span class="sxs-lookup"><span data-stu-id="6c59f-109">hello queries in this article use hello following sample document.</span></span>

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
## <span data-ttu-id="6c59f-110"><a id="examplequery1"></a>1. példa lekérdezés</span><span class="sxs-lookup"><span data-stu-id="6c59f-110"><a id="examplequery1"></a> Example query 1</span></span> 

<span data-ttu-id="6c59f-111">Hello következő lekérdezés megadott hello minta termékcsalád a dokumentum fenti, amelyekben hello id mezője megegyezik beolvasása hello dokumentumok `WakefieldFamily`.</span><span class="sxs-lookup"><span data-stu-id="6c59f-111">Given hello sample family document above, hello following query returns hello documents where hello id field matches `WakefieldFamily`.</span></span>

<span data-ttu-id="6c59f-112">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="6c59f-112">**Query**</span></span>
    
    db.families.find({ id: “WakefieldFamily”})

<span data-ttu-id="6c59f-113">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="6c59f-113">**Results**</span></span>

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

## <span data-ttu-id="6c59f-114"><a id="examplequery2"></a>Példalekérdezés 2</span><span class="sxs-lookup"><span data-stu-id="6c59f-114"><a id="examplequery2"></a>Example query 2</span></span> 

<span data-ttu-id="6c59f-115">hello következő lekérdezés az összes hello gyermekek hello termékcsalád adja vissza.</span><span class="sxs-lookup"><span data-stu-id="6c59f-115">hello next query returns all hello children in hello family.</span></span> 

<span data-ttu-id="6c59f-116">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="6c59f-116">**Query**</span></span>
    
    db.familes.find( { id: “WakefieldFamily” }, { children: true } )

<span data-ttu-id="6c59f-117">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="6c59f-117">**Results**</span></span>

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


## <span data-ttu-id="6c59f-118"><a id="examplequery3"></a>Példalekérdezés 3</span><span class="sxs-lookup"><span data-stu-id="6c59f-118"><a id="examplequery3"></a>Example query 3</span></span> 

<span data-ttu-id="6c59f-119">hello tovább lekérdezés visszaadja az összes hello családok bejegyzett.</span><span class="sxs-lookup"><span data-stu-id="6c59f-119">hello next query returns all hello families which are registered.</span></span> 

<span data-ttu-id="6c59f-120">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="6c59f-120">**Query**</span></span>
    
    db.families.find( { "isRegistered" : true })
<span data-ttu-id="6c59f-121">**Eredmények** nincs dokumentum adja vissza.</span><span class="sxs-lookup"><span data-stu-id="6c59f-121">**Results** No document will be returned.</span></span> 

## <span data-ttu-id="6c59f-122"><a id="examplequery4"></a>Példalekérdezés 4</span><span class="sxs-lookup"><span data-stu-id="6c59f-122"><a id="examplequery4"></a>Example query 4</span></span>

<span data-ttu-id="6c59f-123">hello tovább lekérdezés visszaadja az összes hello családok, amelyek nincsenek regisztrálva.</span><span class="sxs-lookup"><span data-stu-id="6c59f-123">hello next query returns all hello families which are not registered.</span></span> 

<span data-ttu-id="6c59f-124">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="6c59f-124">**Query**</span></span>
    
    db.families.find( { "isRegistered" : false })
<span data-ttu-id="6c59f-125">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="6c59f-125">**Results**</span></span>

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
<span data-ttu-id="6c59f-126">}</span><span class="sxs-lookup"><span data-stu-id="6c59f-126">}</span></span>

## <span data-ttu-id="6c59f-127"><a id="examplequery5"></a>Példalekérdezés 5</span><span class="sxs-lookup"><span data-stu-id="6c59f-127"><a id="examplequery5"></a>Example query 5</span></span>

<span data-ttu-id="6c59f-128">hello tovább lekérdezés adja vissza minden hello családok, amelyek nincsenek regisztrálva és állapot NY.</span><span class="sxs-lookup"><span data-stu-id="6c59f-128">hello next query returns all hello families which are not registered and state is NY.</span></span> 

<span data-ttu-id="6c59f-129">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="6c59f-129">**Query**</span></span>
    
     db.families.find( { "isRegistered" : false, "address.state" : "NY" })

<span data-ttu-id="6c59f-130">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="6c59f-130">**Results**</span></span>

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
<span data-ttu-id="6c59f-131">}</span><span class="sxs-lookup"><span data-stu-id="6c59f-131">}</span></span>


## <span data-ttu-id="6c59f-132"><a id="examplequery6"></a>Példalekérdezés 6</span><span class="sxs-lookup"><span data-stu-id="6c59f-132"><a id="examplequery6"></a>Example query 6</span></span>

<span data-ttu-id="6c59f-133">hello tovább lekérdezés visszaadja az összes hello családok gyermekek besorolási esetén 8.</span><span class="sxs-lookup"><span data-stu-id="6c59f-133">hello next query returns all hello families where children grades are 8.</span></span>

<span data-ttu-id="6c59f-134">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="6c59f-134">**Query**</span></span>
  
     db.families.find( { children : { $elemMatch: { grade : 8 }} } )

<span data-ttu-id="6c59f-135">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="6c59f-135">**Results**</span></span>

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
<span data-ttu-id="6c59f-136">}</span><span class="sxs-lookup"><span data-stu-id="6c59f-136">}</span></span>

## <span data-ttu-id="6c59f-137"><a id="examplequery7"></a>Példalekérdezés 7</span><span class="sxs-lookup"><span data-stu-id="6c59f-137"><a id="examplequery7"></a>Example query 7</span></span>

<span data-ttu-id="6c59f-138">hello tovább lekérdezés visszaadja az összes hello családok ahol gyermekek tömb mérete 3.</span><span class="sxs-lookup"><span data-stu-id="6c59f-138">hello next query returns all hello families where size of children array is 3.</span></span>

<span data-ttu-id="6c59f-139">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="6c59f-139">**Query**</span></span>
  
      db.Family.find( {children: { $size:3} } )

<span data-ttu-id="6c59f-140">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="6c59f-140">**Results**</span></span>

<span data-ttu-id="6c59f-141">Jelenleg nincs 2-nél több gyermekek visszatér nem járt eredménnyel.</span><span class="sxs-lookup"><span data-stu-id="6c59f-141">No results will be returned as we do not have more than 2 children.</span></span> <span data-ttu-id="6c59f-142">Csak akkor, ha a paraméter értéke 2 Ez a lekérdezés sikeresen befejeződik, és térjen vissza a teljes dokumentum hello.</span><span class="sxs-lookup"><span data-stu-id="6c59f-142">Only when parameter is 2 this query will succeed and return hello full document.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6c59f-143">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6c59f-143">Next steps</span></span>

<span data-ttu-id="6c59f-144">Ebben az oktatóanyagban hello következő régebben már kötöttek:</span><span class="sxs-lookup"><span data-stu-id="6c59f-144">In this tutorial, you've done hello following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="6c59f-145">Megtudta, hogyan használja a MongoDB tooquery</span><span class="sxs-lookup"><span data-stu-id="6c59f-145">Learned how tooquery using MongoDB</span></span> 

<span data-ttu-id="6c59f-146">Most már folytathatja toohello következő útmutató toolearn hogyan toodistribute az adatok globálisan.</span><span class="sxs-lookup"><span data-stu-id="6c59f-146">You can now proceed toohello next tutorial toolearn how toodistribute your data globally.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="6c59f-147">Az adatok globálisan terjesztése</span><span class="sxs-lookup"><span data-stu-id="6c59f-147">Distribute your data globally</span></span>](tutorial-global-distribution-documentdb.md)

