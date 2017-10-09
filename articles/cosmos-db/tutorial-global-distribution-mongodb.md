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
# <a name="how-toosetup-azure-cosmos-db-global-distribution-using-hello-mongodb-api"></a><span data-ttu-id="d1997-104">Hogyan toosetup Azure Cosmos DB globális terjesztési használatával hello MongoDB API</span><span class="sxs-lookup"><span data-stu-id="d1997-104">How toosetup Azure Cosmos DB global distribution using hello MongoDB API</span></span>

<span data-ttu-id="d1997-105">Ez a cikk megmutatjuk, hogyan toouse hello Azure portál toosetup Azure Cosmos DB globális terjesztési, és csatlakoztassa a hello MongoDB API használatával.</span><span class="sxs-lookup"><span data-stu-id="d1997-105">In this article, we show how toouse hello Azure portal toosetup Azure Cosmos DB global distribution and then connect using hello MongoDB API.</span></span>

<span data-ttu-id="d1997-106">Ez a cikk ismerteti a következő feladatok hello:</span><span class="sxs-lookup"><span data-stu-id="d1997-106">This article covers hello following tasks:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="d1997-107">Konfigurálja a globális terjesztési hello Azure-portál használatával</span><span class="sxs-lookup"><span data-stu-id="d1997-107">Configure global distribution using hello Azure portal</span></span>
> * <span data-ttu-id="d1997-108">Globális terjesztési használatával hello konfigurálása [MongoDB API](mongodb-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="d1997-108">Configure global distribution using hello [MongoDB API](mongodb-introduction.md)</span></span>

[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]

## <a name="verifying-your-regional-setup-using-hello-mongodb-api"></a><span data-ttu-id="d1997-109">A területi beállításai hello MongoDB API használatával ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="d1997-109">Verifying your regional setup using hello MongoDB API</span></span>
<span data-ttu-id="d1997-110">hello legegyszerűbb módja a dupla ellenőrzése a globális konfigurációs API belül, a MongoDB toorun hello *isMaster()* hello Mongo rendszerhéj parancsot.</span><span class="sxs-lookup"><span data-stu-id="d1997-110">hello simplest way of double checking your global configuration within API for MongoDB is toorun hello *isMaster()* command from hello Mongo Shell.</span></span>

<span data-ttu-id="d1997-111">Az a Mongo parancskörnyezet:</span><span class="sxs-lookup"><span data-stu-id="d1997-111">From your Mongo Shell:</span></span>

   ```
      db.isMaster()
   ```
   
<span data-ttu-id="d1997-112">Példa eredménye:</span><span class="sxs-lookup"><span data-stu-id="d1997-112">Example results:</span></span>

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

## <a name="connecting-tooa-preferred-region-using-hello-mongodb-api"></a><span data-ttu-id="d1997-113">Csatlakozás előnyben részesített régióba tooa hello MongoDB API</span><span class="sxs-lookup"><span data-stu-id="d1997-113">Connecting tooa preferred region using hello MongoDB API</span></span>

<span data-ttu-id="d1997-114">hello MongoDB API lehetővé teszi, hogy Ön toospecify a gyűjtemény írásvédett globálisan elosztott adatbázis beállításait.</span><span class="sxs-lookup"><span data-stu-id="d1997-114">hello MongoDB API enables you toospecify your collection's read preference for a globally distributed database.</span></span> <span data-ttu-id="d1997-115">Mindkét alacsony késési Olvasás és globális magas rendelkezésre állású, javasoljuk a gyűjtemény írásvédett beállításokat szabályozó beállítás túl*legközelebbi*.</span><span class="sxs-lookup"><span data-stu-id="d1997-115">For both low latency reads and global high availability, we recommend setting your collection's read preference too*nearest*.</span></span> <span data-ttu-id="d1997-116">Olvasási előnyként a *legközelebbi* konfigurált tooread hello legközelebbi régióban van.</span><span class="sxs-lookup"><span data-stu-id="d1997-116">A read preference of *nearest* is configured tooread from hello closest region.</span></span>

```csharp
var collection = database.GetCollection<BsonDocument>(collectionName);
collection = collection.WithReadPreference(new ReadPreference(ReadPreferenceMode.Nearest));
```

<span data-ttu-id="d1997-117">Az elsődleges alkalmazások olvasási/írási régióhoz és egy másodlagos régióban, a vész-helyreállítási forgatókönyvek, javasoljuk, a gyűjtemény írásvédett beállítás túl*másodlagos előnyben részesített*.</span><span class="sxs-lookup"><span data-stu-id="d1997-117">For applications with a primary read/write region and a secondary region for disaster recovery (DR) scenarios, we recommend setting your collection's read preference too*secondary preferred*.</span></span> <span data-ttu-id="d1997-118">Olvasási előnyként a *másodlagos előnyben részesített* hello másodlagos régióban konfigurált tooread esetén hello elsődleges régió nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="d1997-118">A read preference of *secondary preferred* is configured tooread from hello secondary region when hello primary region is unavailable.</span></span>

```csharp
var collection = database.GetCollection<BsonDocument>(collectionName);
collection = collection.WithReadPreference(new ReadPreference(ReadPreferenceMode.SecondaryPreferred));
```

<span data-ttu-id="d1997-119">Végül ha például a toomanually lehet megadni a olvasási régiók.</span><span class="sxs-lookup"><span data-stu-id="d1997-119">Lastly, if you would like toomanually specify your read regions.</span></span> <span data-ttu-id="d1997-120">Hello régió címke belül olvasási igény szerint állíthatja be.</span><span class="sxs-lookup"><span data-stu-id="d1997-120">You can set hello region Tag within your read preference.</span></span>

```csharp
var collection = database.GetCollection<BsonDocument>(collectionName);
var tag = new Tag("region", "Southeast Asia");
collection = collection.WithReadPreference(new ReadPreference(ReadPreferenceMode.Secondary, new[] { new TagSet(new[] { tag }) }));
```

<span data-ttu-id="d1997-121">Ez azt, hogy ez az oktatóanyag befejezése.</span><span class="sxs-lookup"><span data-stu-id="d1997-121">That's it, that completes this tutorial.</span></span> <span data-ttu-id="d1997-122">Megismerheti, hogyan toomanage hello olvasásával globálisan replikált fiókja konzisztencia [Azure Cosmos DB-ben konzisztenciaszintek](consistency-levels.md).</span><span class="sxs-lookup"><span data-stu-id="d1997-122">You can learn how toomanage hello consistency of your globally replicated account by reading [Consistency levels in Azure Cosmos DB](consistency-levels.md).</span></span> <span data-ttu-id="d1997-123">És hogyan globális adatbázis-replikációval kapcsolatos további információk az Azure Cosmos Adatbázisba működik, a következő témakörben: [adatok globálisan Azure Cosmos DB terjesztése](distribute-data-globally.md).</span><span class="sxs-lookup"><span data-stu-id="d1997-123">And for more information about how global database replication works in Azure Cosmos DB, see [Distribute data globally with Azure Cosmos DB](distribute-data-globally.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="d1997-124">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d1997-124">Next steps</span></span>

<span data-ttu-id="d1997-125">Ebben az oktatóanyagban hello következő régebben már kötöttek:</span><span class="sxs-lookup"><span data-stu-id="d1997-125">In this tutorial, you've done hello following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d1997-126">Konfigurálja a globális terjesztési hello Azure-portál használatával</span><span class="sxs-lookup"><span data-stu-id="d1997-126">Configure global distribution using hello Azure portal</span></span>
> * <span data-ttu-id="d1997-127">Globális terjesztési hello DocumentDB API-k használatával konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d1997-127">Configure global distribution using hello DocumentDB APIs</span></span>

<span data-ttu-id="d1997-128">Most már folytathatja a következő útmutató toolearn toohello hogyan helyileg toodevelop hello Azure Cosmos DB helyi emulátor.</span><span class="sxs-lookup"><span data-stu-id="d1997-128">You can now proceed toohello next tutorial toolearn how toodevelop locally using hello Azure Cosmos DB local emulator.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="d1997-129">Hello emulátorral helyileg fejlesztése</span><span class="sxs-lookup"><span data-stu-id="d1997-129">Develop locally with hello emulator</span></span>](local-emulator.md)
