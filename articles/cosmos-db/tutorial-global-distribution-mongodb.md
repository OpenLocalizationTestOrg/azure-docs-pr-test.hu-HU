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
# <a name="how-to-setup-azure-cosmos-db-global-distribution-using-the-mongodb-api"></a><span data-ttu-id="e3ab1-104">How Azure Cosmos DB globális terjesztési a MongoDB API-jával beállítása</span><span class="sxs-lookup"><span data-stu-id="e3ab1-104">How to setup Azure Cosmos DB global distribution using the MongoDB API</span></span>

<span data-ttu-id="e3ab1-105">Ebben a cikkben megmutatjuk, hogyan használható az Azure-portálon Azure Cosmos DB globális terjesztési beállításához, és csatlakoztassa a MongoDB API használatával.</span><span class="sxs-lookup"><span data-stu-id="e3ab1-105">In this article, we show how to use the Azure portal to setup Azure Cosmos DB global distribution and then connect using the MongoDB API.</span></span>

<span data-ttu-id="e3ab1-106">Ez a cikk ismerteti a következő feladatokat:</span><span class="sxs-lookup"><span data-stu-id="e3ab1-106">This article covers the following tasks:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="e3ab1-107">Az Azure portál használatával globális terjesztési konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e3ab1-107">Configure global distribution using the Azure portal</span></span>
> * <span data-ttu-id="e3ab1-108">Globális terjesztési használatával konfigurálja a [MongoDB API](mongodb-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="e3ab1-108">Configure global distribution using the [MongoDB API](mongodb-introduction.md)</span></span>

[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]

## <a name="verifying-your-regional-setup-using-the-mongodb-api"></a><span data-ttu-id="e3ab1-109">A területi beállításai a MongoDB API-jával ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="e3ab1-109">Verifying your regional setup using the MongoDB API</span></span>
<span data-ttu-id="e3ab1-110">A globális konfigurációs API belül ellenőrzése a MongoDB futtatásához dupla legegyszerűbb módja a *isMaster()* a Mongo rendszerhéj parancsot.</span><span class="sxs-lookup"><span data-stu-id="e3ab1-110">The simplest way of double checking your global configuration within API for MongoDB is to run the *isMaster()* command from the Mongo Shell.</span></span>

<span data-ttu-id="e3ab1-111">Az a Mongo parancskörnyezet:</span><span class="sxs-lookup"><span data-stu-id="e3ab1-111">From your Mongo Shell:</span></span>

   ```
      db.isMaster()
   ```
   
<span data-ttu-id="e3ab1-112">Példa eredménye:</span><span class="sxs-lookup"><span data-stu-id="e3ab1-112">Example results:</span></span>

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

## <a name="connecting-to-a-preferred-region-using-the-mongodb-api"></a><span data-ttu-id="e3ab1-113">A preferált régió a MongoDB API-jával való kapcsolódás</span><span class="sxs-lookup"><span data-stu-id="e3ab1-113">Connecting to a preferred region using the MongoDB API</span></span>

<span data-ttu-id="e3ab1-114">A MongoDB API lehetővé teszi, hogy adja meg a gyűjtemény írásvédett globálisan elosztott adatbázis beállításait.</span><span class="sxs-lookup"><span data-stu-id="e3ab1-114">The MongoDB API enables you to specify your collection's read preference for a globally distributed database.</span></span> <span data-ttu-id="e3ab1-115">Mindkét alacsony késési Olvasás és globális magas rendelkezésre állású, javasoljuk a beállítást a gyűjtemény írásvédett preferencia *legközelebbi*.</span><span class="sxs-lookup"><span data-stu-id="e3ab1-115">For both low latency reads and global high availability, we recommend setting your collection's read preference to *nearest*.</span></span> <span data-ttu-id="e3ab1-116">Olvasási előnyként a *legközelebbi* van konfigurálva, az eredetihez legközelebb eső régiót olvasni.</span><span class="sxs-lookup"><span data-stu-id="e3ab1-116">A read preference of *nearest* is configured to read from the closest region.</span></span>

```csharp
var collection = database.GetCollection<BsonDocument>(collectionName);
collection = collection.WithReadPreference(new ReadPreference(ReadPreferenceMode.Nearest));
```

<span data-ttu-id="e3ab1-117">Az elsődleges alkalmazások olvasási/írási régióhoz és egy másodlagos régióban, a vész-helyreállítási forgatókönyvek, javasolt a beállítást a gyűjtemény írásvédett beállítás *másodlagos előnyben részesített*.</span><span class="sxs-lookup"><span data-stu-id="e3ab1-117">For applications with a primary read/write region and a secondary region for disaster recovery (DR) scenarios, we recommend setting your collection's read preference to *secondary preferred*.</span></span> <span data-ttu-id="e3ab1-118">Olvasási előnyként a *másodlagos előnyben részesített* van konfigurálva, a másodlagos régióba olvasni, ha az elsődleges régióban nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="e3ab1-118">A read preference of *secondary preferred* is configured to read from the secondary region when the primary region is unavailable.</span></span>

```csharp
var collection = database.GetCollection<BsonDocument>(collectionName);
collection = collection.WithReadPreference(new ReadPreference(ReadPreferenceMode.SecondaryPreferred));
```

<span data-ttu-id="e3ab1-119">Végül ha szeretné manuálisan adja meg az olvasási régiók.</span><span class="sxs-lookup"><span data-stu-id="e3ab1-119">Lastly, if you would like to manually specify your read regions.</span></span> <span data-ttu-id="e3ab1-120">A régió címke belül olvasási igény szerint állíthatja be.</span><span class="sxs-lookup"><span data-stu-id="e3ab1-120">You can set the region Tag within your read preference.</span></span>

```csharp
var collection = database.GetCollection<BsonDocument>(collectionName);
var tag = new Tag("region", "Southeast Asia");
collection = collection.WithReadPreference(new ReadPreference(ReadPreferenceMode.Secondary, new[] { new TagSet(new[] { tag }) }));
```

<span data-ttu-id="e3ab1-121">Ez azt, hogy ez az oktatóanyag befejezése.</span><span class="sxs-lookup"><span data-stu-id="e3ab1-121">That's it, that completes this tutorial.</span></span> <span data-ttu-id="e3ab1-122">Megismerheti a globális replikált fiókja konzisztencia kezeléséhez olvasásával [Azure Cosmos DB-ben konzisztenciaszintek](consistency-levels.md).</span><span class="sxs-lookup"><span data-stu-id="e3ab1-122">You can learn how to manage the consistency of your globally replicated account by reading [Consistency levels in Azure Cosmos DB](consistency-levels.md).</span></span> <span data-ttu-id="e3ab1-123">És hogyan globális adatbázis-replikációval kapcsolatos további információk az Azure Cosmos Adatbázisba működik, a következő témakörben: [adatok globálisan Azure Cosmos DB terjesztése](distribute-data-globally.md).</span><span class="sxs-lookup"><span data-stu-id="e3ab1-123">And for more information about how global database replication works in Azure Cosmos DB, see [Distribute data globally with Azure Cosmos DB](distribute-data-globally.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="e3ab1-124">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e3ab1-124">Next steps</span></span>

<span data-ttu-id="e3ab1-125">Ebben az oktatóanyagban ezt a következők:</span><span class="sxs-lookup"><span data-stu-id="e3ab1-125">In this tutorial, you've done the following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e3ab1-126">Az Azure portál használatával globális terjesztési konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e3ab1-126">Configure global distribution using the Azure portal</span></span>
> * <span data-ttu-id="e3ab1-127">A DocumentDB API-k használatával globális terjesztési konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e3ab1-127">Configure global distribution using the DocumentDB APIs</span></span>

<span data-ttu-id="e3ab1-128">Most már folytathatja a következő oktatóanyag megtudhatja, hogyan fejleszthet, helyileg emulátorral Azure Cosmos DB helyi.</span><span class="sxs-lookup"><span data-stu-id="e3ab1-128">You can now proceed to the next tutorial to learn how to develop locally using the Azure Cosmos DB local emulator.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="e3ab1-129">Helyileg emulátorral fejlesztése</span><span class="sxs-lookup"><span data-stu-id="e3ab1-129">Develop locally with the emulator</span></span>](local-emulator.md)