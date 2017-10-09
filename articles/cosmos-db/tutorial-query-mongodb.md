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
# <a name="azure-cosmos-db-how-tooquery-with-api-for-mongodb"></a>A Cosmos DB Azure: Hogyan tooquery mongodb API-t?

hello Azure Cosmos DB [API-t a MongoDB](mongodb-introduction.md) támogatja [MongoDB rendszerhéj lekérdezések](https://docs.mongodb.com/manual/tutorial/query-documents/). 

Ez a cikk ismerteti a következő feladatok hello: 

> [!div class="checklist"]
> * A MongoDB-vel adatok lekérdezése

## <a name="sample-document"></a>A minta-dokumentum

Ebben a cikkben hello lekérdezések használja a következő minta dokumentum hello.

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
## <a id="examplequery1"></a>1. példa lekérdezés 

Hello következő lekérdezés megadott hello minta termékcsalád a dokumentum fenti, amelyekben hello id mezője megegyezik beolvasása hello dokumentumok `WakefieldFamily`.

**Lekérdezés**
    
    db.families.find({ id: “WakefieldFamily”})

**Eredmények**

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

## <a id="examplequery2"></a>Példalekérdezés 2 

hello következő lekérdezés az összes hello gyermekek hello termékcsalád adja vissza. 

**Lekérdezés**
    
    db.familes.find( { id: “WakefieldFamily” }, { children: true } )

**Eredmények**

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


## <a id="examplequery3"></a>Példalekérdezés 3 

hello tovább lekérdezés visszaadja az összes hello családok bejegyzett. 

**Lekérdezés**
    
    db.families.find( { "isRegistered" : true })
**Eredmények** nincs dokumentum adja vissza. 

## <a id="examplequery4"></a>Példalekérdezés 4

hello tovább lekérdezés visszaadja az összes hello családok, amelyek nincsenek regisztrálva. 

**Lekérdezés**
    
    db.families.find( { "isRegistered" : false })
**Eredmények**

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
}

## <a id="examplequery5"></a>Példalekérdezés 5

hello tovább lekérdezés adja vissza minden hello családok, amelyek nincsenek regisztrálva és állapot NY. 

**Lekérdezés**
    
     db.families.find( { "isRegistered" : false, "address.state" : "NY" })

**Eredmények**

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
}


## <a id="examplequery6"></a>Példalekérdezés 6

hello tovább lekérdezés visszaadja az összes hello családok gyermekek besorolási esetén 8.

**Lekérdezés**
  
     db.families.find( { children : { $elemMatch: { grade : 8 }} } )

**Eredmények**

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
}

## <a id="examplequery7"></a>Példalekérdezés 7

hello tovább lekérdezés visszaadja az összes hello családok ahol gyermekek tömb mérete 3.

**Lekérdezés**
  
      db.Family.find( {children: { $size:3} } )

**Eredmények**

Jelenleg nincs 2-nél több gyermekek visszatér nem járt eredménnyel. Csak akkor, ha a paraméter értéke 2 Ez a lekérdezés sikeresen befejeződik, és térjen vissza a teljes dokumentum hello.

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban hello következő régebben már kötöttek:

> [!div class="checklist"]
> * Megtudta, hogyan használja a MongoDB tooquery 

Most már folytathatja toohello következő útmutató toolearn hogyan toodistribute az adatok globálisan.

> [!div class="nextstepaction"]
> [Az adatok globálisan terjesztése](tutorial-global-distribution-documentdb.md)

