---
title: "Graph API aaaAzure Cosmos DB globális terjesztési oktatóanyaga |} Microsoft Docs"
description: "Ismerje meg, hogyan toosetup Azure Cosmos DB globális terjesztési használatával hello Graph API-val."
services: cosmos-db
keywords: "globális terjesztési, graph, gremlin"
documentationcenter: 
author: dennyglee
manager: jhubbard
editor: cgronlun
ms.assetid: 8b815047-2868-4b10-af1d-40a1af419a70
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: denlee
ms.openlocfilehash: 1629a31e12a18079f63e07c4909862b36b5f4c0e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toosetup-azure-cosmos-db-global-distribution-using-hello-graph-api"></a><span data-ttu-id="42f00-104">Hogyan toosetup Azure Cosmos DB globális terjesztési használatával hello Graph API-val</span><span class="sxs-lookup"><span data-stu-id="42f00-104">How toosetup Azure Cosmos DB global distribution using hello Graph API</span></span>

<span data-ttu-id="42f00-105">Ez a cikk megmutatjuk, hogyan toouse hello Azure portál toosetup Azure Cosmos DB globális terjesztési, és csatlakoztassa a hello Graph API-val (előzetes verzió) használatával.</span><span class="sxs-lookup"><span data-stu-id="42f00-105">In this article, we show how toouse hello Azure portal toosetup Azure Cosmos DB global distribution and then connect using hello Graph API (preview).</span></span>

<span data-ttu-id="42f00-106">Ez a cikk ismerteti a következő feladatok hello:</span><span class="sxs-lookup"><span data-stu-id="42f00-106">This article covers hello following tasks:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="42f00-107">Konfigurálja a globális terjesztési hello Azure-portál használatával</span><span class="sxs-lookup"><span data-stu-id="42f00-107">Configure global distribution using hello Azure portal</span></span>
> * <span data-ttu-id="42f00-108">Globális terjesztési használatával hello konfigurálása [Graph API-k](graph-introduction.md) (előzetes verzió)</span><span class="sxs-lookup"><span data-stu-id="42f00-108">Configure global distribution using hello [Graph APIs](graph-introduction.md) (preview)</span></span>

[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]


## <a name="connecting-tooa-preferred-region-using-hello-graph-api-using-hello-net-sdk"></a><span data-ttu-id="42f00-109">Csatlakozás előnyben részesített régióba tooa hello Graph API hello .NET SDK használatával</span><span class="sxs-lookup"><span data-stu-id="42f00-109">Connecting tooa preferred region using hello Graph API using hello .NET SDK</span></span>

<span data-ttu-id="42f00-110">Graph API hello fel van fedve egy bővítmény tárként fölött hello DocumentDB SDK-t.</span><span class="sxs-lookup"><span data-stu-id="42f00-110">hello Graph API is exposed as an extension library on top of hello DocumentDB SDK.</span></span>

<span data-ttu-id="42f00-111">A rendelés tootake előnyeit [globális terjesztési](distribute-data-globally.md), ügyfélalkalmazások hello rendezett használt tooperform dokumentum műveletek régiók toobe preferencia listája adhat meg.</span><span class="sxs-lookup"><span data-stu-id="42f00-111">In order tootake advantage of [global distribution](distribute-data-globally.md), client applications can specify hello ordered preference list of regions toobe used tooperform document operations.</span></span> <span data-ttu-id="42f00-112">Ezt megteheti hello kapcsolat házirend beállításával.</span><span class="sxs-lookup"><span data-stu-id="42f00-112">This can be done by setting hello connection policy.</span></span> <span data-ttu-id="42f00-113">Hello Azure Cosmos DB fiók konfigurációtól függően aktuális területi rendelkezésre állását és hello preferencia lista megadott, hello legtöbb optimális végpont rendszer hello SDK tooperform írási által választott és olvasási műveletek.</span><span class="sxs-lookup"><span data-stu-id="42f00-113">Based on hello Azure Cosmos DB account configuration, current regional availability and hello preference list specified, hello most optimal endpoint will be chosen by hello SDK tooperform write and read operations.</span></span>

<span data-ttu-id="42f00-114">Ez a beállítás lista inicializálása közben hello SDK-k használatával van megadva.</span><span class="sxs-lookup"><span data-stu-id="42f00-114">This preference list is specified when initializing a connection using hello SDKs.</span></span> <span data-ttu-id="42f00-115">hello SDK-k elfogadása "PreferredLocations" nem kötelező paraméter, amely egy Azure-régiók rendezett listáját.</span><span class="sxs-lookup"><span data-stu-id="42f00-115">hello SDKs accept an optional parameter "PreferredLocations" that is an ordered list of Azure regions.</span></span>

* <span data-ttu-id="42f00-116">**Írási műveletek**: hello SDK automatikusan elküld minden ír toohello aktuális írási terület.</span><span class="sxs-lookup"><span data-stu-id="42f00-116">**Writes**: hello SDK will automatically send all writes toohello current write region.</span></span>
* <span data-ttu-id="42f00-117">**Beolvassa**: minden olvasási küld toohello első rendelkezésre álló terület hello PreferredLocations listában.</span><span class="sxs-lookup"><span data-stu-id="42f00-117">**Reads**: All reads will be sent toohello first available region in hello PreferredLocations list.</span></span> <span data-ttu-id="42f00-118">Hello kérelem sikertelen lesz, ha hello ügyfél le hello lista toohello következő terület sikertelen, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="42f00-118">If hello request fails, hello client will fail down hello list toohello next region, and so on.</span></span> <span data-ttu-id="42f00-119">hello SDK-k csak kísérli meg a hello régió van megadva a PreferredLocations tooread.</span><span class="sxs-lookup"><span data-stu-id="42f00-119">hello SDKs will only attempt tooread from hello regions specified in PreferredLocations.</span></span> <span data-ttu-id="42f00-120">Így például ha hello Cosmos DB fiók három régiókban, de hello ügyfél csak határozza meg két hello nem írási régiók PreferredLocations, majd nincs olvasási szolgáltató hello írási régió, még akkor is, a feladatátvétel hello eset kívül.</span><span class="sxs-lookup"><span data-stu-id="42f00-120">So, for example, if hello Cosmos DB account is available in three regions, but hello client only specifies two of hello non-write regions for PreferredLocations, then no reads will be served out of hello write region, even in hello case of failover.</span></span>

<span data-ttu-id="42f00-121">hello alkalmazás hello aktuális írási végpont ellenőrizheti és olvassa el a végpont által hello SDK WriteEndpoint és ReadEndpoint, elérhető, a SDK 1.8-as verzióját és az újabb ellenőrzési két tulajdonság által választott.</span><span class="sxs-lookup"><span data-stu-id="42f00-121">hello application can verify hello current write endpoint and read endpoint chosen by hello SDK by checking two properties, WriteEndpoint and ReadEndpoint, available in SDK version 1.8 and above.</span></span> <span data-ttu-id="42f00-122">Hello PreferredLocations tulajdonság nincs beállítva, ha minden kérésnél hello aktuális írási régióban kell kézbesíteni.</span><span class="sxs-lookup"><span data-stu-id="42f00-122">If hello PreferredLocations property is not set, all requests will be served from hello current write region.</span></span>

### <a name="using-hello-sdk"></a><span data-ttu-id="42f00-123">Hello SDK használatával</span><span class="sxs-lookup"><span data-stu-id="42f00-123">Using hello SDK</span></span>

<span data-ttu-id="42f00-124">Például a .NET SDK hello, hello `ConnectionPolicy` hello paramétere `DocumentClient` konstruktor hívása tulajdonsággal rendelkezik `PreferredLocations`.</span><span class="sxs-lookup"><span data-stu-id="42f00-124">For example, in hello .NET SDK, hello `ConnectionPolicy` parameter for hello `DocumentClient` constructor has a property called `PreferredLocations`.</span></span> <span data-ttu-id="42f00-125">Ez a tulajdonság beállítható tooa régió nevek listája.</span><span class="sxs-lookup"><span data-stu-id="42f00-125">This property can be set tooa list of region names.</span></span> <span data-ttu-id="42f00-126">hello megjelenített neveket a [Azure-régiókat] [ regions] részeként adható meg `PreferredLocations`.</span><span class="sxs-lookup"><span data-stu-id="42f00-126">hello display names for [Azure Regions][regions] can be specified as part of `PreferredLocations`.</span></span>

> [!NOTE]
> <span data-ttu-id="42f00-127">hello URL-címek hello végpontok hosszú élettartamú állandóként nem tekinthető.</span><span class="sxs-lookup"><span data-stu-id="42f00-127">hello URLs for hello endpoints should not be considered as long-lived constants.</span></span> <span data-ttu-id="42f00-128">hello szolgáltatás ezek bármikor előfordulhat, hogy frissíteni.</span><span class="sxs-lookup"><span data-stu-id="42f00-128">hello service may update these at any point.</span></span> <span data-ttu-id="42f00-129">hello SDK automatikusan kezeli ezt a módosítást.</span><span class="sxs-lookup"><span data-stu-id="42f00-129">hello SDK handles this change automatically.</span></span>
>
>

```cs
// Getting endpoints from application settings or other configuration location
Uri accountEndPoint = new Uri(Properties.Settings.Default.GlobalDatabaseUri);
string accountKey = Properties.Settings.Default.GlobalDatabaseKey;

ConnectionPolicy connectionPolicy = new ConnectionPolicy();

//Setting read region selection preference
connectionPolicy.PreferredLocations.Add(LocationNames.WestUS); // first preference
connectionPolicy.PreferredLocations.Add(LocationNames.EastUS); // second preference
connectionPolicy.PreferredLocations.Add(LocationNames.NorthEurope); // third preference

// initialize connection
DocumentClient docClient = new DocumentClient(
    accountEndPoint,
    accountKey,
    connectionPolicy);

// connect tooAzure Cosmos DB
await docClient.OpenAsync().ConfigureAwait(false);
```

<span data-ttu-id="42f00-130">Ez azt, hogy ez az oktatóanyag befejezése.</span><span class="sxs-lookup"><span data-stu-id="42f00-130">That's it, that completes this tutorial.</span></span> <span data-ttu-id="42f00-131">Megismerheti, hogyan toomanage hello olvasásával globálisan replikált fiókja konzisztencia [Azure Cosmos DB-ben konzisztenciaszintek](consistency-levels.md).</span><span class="sxs-lookup"><span data-stu-id="42f00-131">You can learn how toomanage hello consistency of your globally replicated account by reading [Consistency levels in Azure Cosmos DB](consistency-levels.md).</span></span> <span data-ttu-id="42f00-132">És hogyan globális adatbázis-replikációval kapcsolatos további információk az Azure Cosmos Adatbázisba működik, a következő témakörben: [adatok globálisan Azure Cosmos DB terjesztése](distribute-data-globally.md).</span><span class="sxs-lookup"><span data-stu-id="42f00-132">And for more information about how global database replication works in Azure Cosmos DB, see [Distribute data globally with Azure Cosmos DB](distribute-data-globally.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="42f00-133">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="42f00-133">Next steps</span></span>

<span data-ttu-id="42f00-134">Ebben az oktatóanyagban hello következő régebben már kötöttek:</span><span class="sxs-lookup"><span data-stu-id="42f00-134">In this tutorial, you've done hello following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="42f00-135">Konfigurálja a globális terjesztési hello Azure-portál használatával</span><span class="sxs-lookup"><span data-stu-id="42f00-135">Configure global distribution using hello Azure portal</span></span>
> * <span data-ttu-id="42f00-136">Globális terjesztési hello DocumentDB API-k használatával konfigurálása</span><span class="sxs-lookup"><span data-stu-id="42f00-136">Configure global distribution using hello DocumentDB APIs</span></span>

<span data-ttu-id="42f00-137">Most már folytathatja a következő útmutató toolearn toohello hogyan helyileg toodevelop hello Azure Cosmos DB helyi emulátor.</span><span class="sxs-lookup"><span data-stu-id="42f00-137">You can now proceed toohello next tutorial toolearn how toodevelop locally using hello Azure Cosmos DB local emulator.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="42f00-138">Hello emulátorral helyileg fejlesztése</span><span class="sxs-lookup"><span data-stu-id="42f00-138">Develop locally with hello emulator</span></span>](local-emulator.md)

[regions]: https://azure.microsoft.com/regions/

