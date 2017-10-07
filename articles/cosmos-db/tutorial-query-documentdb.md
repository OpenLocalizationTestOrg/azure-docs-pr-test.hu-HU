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
# <a name="azure-cosmos-db-how-tooquery-using-sql"></a>A Cosmos DB Azure: Hogyan tooquery SQL használatával?

hello Azure Cosmos DB [DocumentDB API](documentdb-introduction.md) SQL használatával dokumentumok lekérdezését támogatja. Ez a cikk ismerteti a minta és két minta az SQL-lekérdezések és dokumentum eredmények.

Ez a cikk ismerteti a következő feladatok hello: 

> [!div class="checklist"]
> * Az SQL adatainak lekérdezése

## <a name="sample-document"></a>A minta-dokumentum

Ebben a cikkben az SQL-lekérdezések hello használja a következő minta dokumentum hello.

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
## <a name="where-can-i-run-sql-queries"></a>Ahol futtathatja az SQL-lekérdezések?

Az Azure portálon keresztül hello hello adatkezelő hello segítségével lekérdezéseket is futtathat [REST API-t és az SDK-k](documentdb-sdk-dotnet.md), és még akkor is, hello [tesztlekérdezéseket](https://www.documentdb.com/sql/demo), amelyen fut az lekérdezések mintaadatok készlet.

Az SQL-lekérdezések kapcsolatos további információkért lásd:
* [SQL-lekérdezést és SQL-szintaxis](documentdb-sql-query.md)

## <a name="prerequisites"></a>Előfeltételek

Ez az oktatóanyag feltételezi, hogy rendelkezik egy olyan Azure Cosmos DB fiókot és a gyűjteményhez. Nem rendelkezik egyetlen, az? Teljes hello [5 perces gyors üzembe helyezés](create-mongodb-nodejs.md) vagy hello [fejlesztői útmutató](tutorial-develop-mongodb.md) toocreate egy olyan fiókot és a gyűjteményhez.

## <a name="example-query-1"></a>1. példa lekérdezés

Megadott hello minta termékcsalád a dokumentum fenti, a következő SQL-lekérdezés visszaad hello Ha hello id mezője megegyezik `WakefieldFamily`. Mivel ez egy `SELECT *` , hello lekérdezés kimenetét hello utasítás hello teljes JSON-dokumentum:

**Lekérdezés**

    SELECT * 
    FROM Families f 
    WHERE f.id = "WakefieldFamily"

**Eredmények**

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

## <a name="example-query-2"></a>Példalekérdezés 2

hello tovább lekérdezés visszaadja az összes hello nevet kapnak gyermekek hello termékcsalád, amelynek azonosítója megegyezik `WakefieldFamily` a besorolási rendezve.

**Lekérdezés**

    SELECT c.givenName 
    FROM Families f 
    JOIN c IN f.children 
    WHERE f.id = 'WakefieldFamily'
    ORDER BY f.children.grade ASC

**Eredmények**

    [
      { "givenName": "Jesse" }, 
      { "givenName": "Lisa"}
    ]


## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban hello következő régebben már kötöttek:

> [!div class="checklist"]
> * Megtudta, hogyan tooquery SQL használatával  

Most már folytathatja toohello következő útmutató toolearn hogyan toodistribute az adatok globálisan.

> [!div class="nextstepaction"]
> [Az adatok globálisan terjesztése](tutorial-global-distribution-documentdb.md)

