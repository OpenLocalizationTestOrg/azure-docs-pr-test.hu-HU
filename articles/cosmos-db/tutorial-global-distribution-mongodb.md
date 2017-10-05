---
title: "MongoDB API Azure Cosmos DB globális telepítési útmutató |} Microsoft Docs"
description: "Megtudhatja, hogyan beállítani az Azure Cosmos DB globális terjesztési a MongoDB API használatával."
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
ms.openlocfilehash: a2747102f4d8cac412b67abc3fd07cfa3661bcee
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-setup-azure-cosmos-db-global-distribution-using-the-mongodb-api"></a>How Azure Cosmos DB globális terjesztési a MongoDB API-jával beállítása

Ebben a cikkben megmutatjuk, hogyan használható az Azure-portálon Azure Cosmos DB globális terjesztési beállításához, és csatlakoztassa a MongoDB API használatával.

Ez a cikk ismerteti a következő feladatokat: 

> [!div class="checklist"]
> * Az Azure portál használatával globális terjesztési konfigurálása
> * Globális terjesztési használatával konfigurálja a [MongoDB API](mongodb-introduction.md)

[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]

## <a name="verifying-your-regional-setup-using-the-mongodb-api"></a>A területi beállításai a MongoDB API-jával ellenőrzése
A globális konfigurációs API belül ellenőrzése a MongoDB futtatásához dupla legegyszerűbb módja a *isMaster()* a Mongo rendszerhéj parancsot.

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

## <a name="connecting-to-a-preferred-region-using-the-mongodb-api"></a>A preferált régió a MongoDB API-jával való kapcsolódás

A MongoDB API lehetővé teszi, hogy adja meg a gyűjtemény írásvédett globálisan elosztott adatbázis beállításait. Mindkét alacsony késési Olvasás és globális magas rendelkezésre állású, javasoljuk a beállítást a gyűjtemény írásvédett preferencia *legközelebbi*. Olvasási előnyként a *legközelebbi* van konfigurálva, az eredetihez legközelebb eső régiót olvasni.

```csharp
var collection = database.GetCollection<BsonDocument>(collectionName);
collection = collection.WithReadPreference(new ReadPreference(ReadPreferenceMode.Nearest));
```

Az elsődleges alkalmazások olvasási/írási régióhoz és egy másodlagos régióban, a vész-helyreállítási forgatókönyvek, javasolt a beállítást a gyűjtemény írásvédett beállítás *másodlagos előnyben részesített*. Olvasási előnyként a *másodlagos előnyben részesített* van konfigurálva, a másodlagos régióba olvasni, ha az elsődleges régióban nem érhető el.

```csharp
var collection = database.GetCollection<BsonDocument>(collectionName);
collection = collection.WithReadPreference(new ReadPreference(ReadPreferenceMode.SecondaryPreferred));
```

Végül ha szeretné manuálisan adja meg az olvasási régiók. A régió címke belül olvasási igény szerint állíthatja be.

```csharp
var collection = database.GetCollection<BsonDocument>(collectionName);
var tag = new Tag("region", "Southeast Asia");
collection = collection.WithReadPreference(new ReadPreference(ReadPreferenceMode.Secondary, new[] { new TagSet(new[] { tag }) }));
```

Ez azt, hogy ez az oktatóanyag befejezése. Megismerheti a globális replikált fiókja konzisztencia kezeléséhez olvasásával [Azure Cosmos DB-ben konzisztenciaszintek](consistency-levels.md). És hogyan globális adatbázis-replikációval kapcsolatos további információk az Azure Cosmos Adatbázisba működik, a következő témakörben: [adatok globálisan Azure Cosmos DB terjesztése](distribute-data-globally.md).

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban ezt a következők:

> [!div class="checklist"]
> * Az Azure portál használatával globális terjesztési konfigurálása
> * A DocumentDB API-k használatával globális terjesztési konfigurálása

Most már folytathatja a következő oktatóanyag megtudhatja, hogyan fejleszthet, helyileg emulátorral Azure Cosmos DB helyi.

> [!div class="nextstepaction"]
> [Helyileg emulátorral fejlesztése](local-emulator.md)