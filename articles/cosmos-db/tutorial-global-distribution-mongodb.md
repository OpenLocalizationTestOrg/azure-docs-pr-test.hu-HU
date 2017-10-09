---
title: "MongoDB API aaaAzure Cosmos DB globális terjesztési oktatóanyaga |} Microsoft Docs"
description: "Ismerje meg, hogyan hello MongoDB API toosetup Azure Cosmos DB globális terjesztési használatával."
services: cosmos-db
keywords: "globális terjesztési, a mongodb-Protokolltámogatással"
documentationcenter: 
author: mimig1
manager: jhubbard
editor: cgronlun
ms.assetid: 8b815047-2868-4b10-af1d-40a1af419a70
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: mimig
ms.openlocfilehash: 0fc2d670bb4e21ac5f813f9586b407ba06ccf354
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toosetup-azure-cosmos-db-global-distribution-using-hello-mongodb-api"></a>Hogyan toosetup Azure Cosmos DB globális terjesztési használatával hello MongoDB API

Ez a cikk megmutatjuk, hogyan toouse hello Azure portál toosetup Azure Cosmos DB globális terjesztési, és csatlakoztassa a hello MongoDB API használatával.

Ez a cikk ismerteti a következő feladatok hello: 

> [!div class="checklist"]
> * Konfigurálja a globális terjesztési hello Azure-portál használatával
> * Globális terjesztési használatával hello konfigurálása [MongoDB API](mongodb-introduction.md)

[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]

## <a name="verifying-your-regional-setup-using-hello-mongodb-api"></a>A területi beállításai hello MongoDB API használatával ellenőrzése
hello legegyszerűbb módja a dupla ellenőrzése a globális konfigurációs API belül, a MongoDB toorun hello *isMaster()* hello Mongo rendszerhéj parancsot.

Az a Mongo parancskörnyezet:

   ```
      db.isMaster()
   ```
   
Példa eredménye:

   ```JSON
      {
         "_t": "IsMasterResponse",
         "ok": 1,
         "ismaster": true,
         "maxMessageSizeBytes": 4194304,
         "maxWriteBatchSize": 1000,
         "minWireVersion": 0,
         "maxWireVersion": 2,
         "tags": {
            "region": "South India"
         },
         "hosts": [
            "vishi-api-for-mongodb-southcentralus.documents.azure.com:10255",
            "vishi-api-for-mongodb-westeurope.documents.azure.com:10255",
            "vishi-api-for-mongodb-southindia.documents.azure.com:10255"
         ],
         "setName": "globaldb",
         "setVersion": 1,
         "primary": "vishi-api-for-mongodb-southindia.documents.azure.com:10255",
         "me": "vishi-api-for-mongodb-southindia.documents.azure.com:10255"
      }
   ```

## <a name="connecting-tooa-preferred-region-using-hello-mongodb-api"></a>Csatlakozás előnyben részesített régióba tooa hello MongoDB API

hello MongoDB API lehetővé teszi, hogy Ön toospecify a gyűjtemény írásvédett globálisan elosztott adatbázis beállításait. Mindkét alacsony késési Olvasás és globális magas rendelkezésre állású, javasoljuk a gyűjtemény írásvédett beállításokat szabályozó beállítás túl*legközelebbi*. Olvasási előnyként a *legközelebbi* konfigurált tooread hello legközelebbi régióban van.

```csharp
var collection = database.GetCollection<BsonDocument>(collectionName);
collection = collection.WithReadPreference(new ReadPreference(ReadPreferenceMode.Nearest));
```

Az elsődleges alkalmazások olvasási/írási régióhoz és egy másodlagos régióban, a vész-helyreállítási forgatókönyvek, javasoljuk, a gyűjtemény írásvédett beállítás túl*másodlagos előnyben részesített*. Olvasási előnyként a *másodlagos előnyben részesített* hello másodlagos régióban konfigurált tooread esetén hello elsődleges régió nem érhető el.

```csharp
var collection = database.GetCollection<BsonDocument>(collectionName);
collection = collection.WithReadPreference(new ReadPreference(ReadPreferenceMode.SecondaryPreferred));
```

Végül ha például a toomanually lehet megadni a olvasási régiók. Hello régió címke belül olvasási igény szerint állíthatja be.

```csharp
var collection = database.GetCollection<BsonDocument>(collectionName);
var tag = new Tag("region", "Southeast Asia");
collection = collection.WithReadPreference(new ReadPreference(ReadPreferenceMode.Secondary, new[] { new TagSet(new[] { tag }) }));
```

Ez azt, hogy ez az oktatóanyag befejezése. Megismerheti, hogyan toomanage hello olvasásával globálisan replikált fiókja konzisztencia [Azure Cosmos DB-ben konzisztenciaszintek](consistency-levels.md). És hogyan globális adatbázis-replikációval kapcsolatos további információk az Azure Cosmos Adatbázisba működik, a következő témakörben: [adatok globálisan Azure Cosmos DB terjesztése](distribute-data-globally.md).

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban hello következő régebben már kötöttek:

> [!div class="checklist"]
> * Konfigurálja a globális terjesztési hello Azure-portál használatával
> * Globális terjesztési hello DocumentDB API-k használatával konfigurálása

Most már folytathatja a következő útmutató toolearn toohello hogyan helyileg toodevelop hello Azure Cosmos DB helyi emulátor.

> [!div class="nextstepaction"]
> [Hello emulátorral helyileg fejlesztése](local-emulator.md)
