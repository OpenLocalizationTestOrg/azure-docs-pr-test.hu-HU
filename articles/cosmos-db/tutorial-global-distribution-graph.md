---
title: "A Graph API Azure Cosmos DB globális telepítési útmutató |} Microsoft Docs"
description: "Útmutató: Azure Cosmos DB globális terjesztési a Graph API-jával beállítása."
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
ms.openlocfilehash: 3c8794fe33c2ff5aa79559ea2c323cf8d92b426a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-setup-azure-cosmos-db-global-distribution-using-the-graph-api"></a><span data-ttu-id="1325a-104">How Azure Cosmos DB globális terjesztési a Graph API-jával beállítása</span><span class="sxs-lookup"><span data-stu-id="1325a-104">How to setup Azure Cosmos DB global distribution using the Graph API</span></span>

<span data-ttu-id="1325a-105">Ebben a cikkben megmutatjuk, hogyan használja az Azure-portált telepítő Azure Cosmos DB globális terjesztési, és csatlakozzon a Graph API-val (előzetes verzió).</span><span class="sxs-lookup"><span data-stu-id="1325a-105">In this article, we show how to use the Azure portal to setup Azure Cosmos DB global distribution and then connect using the Graph API (preview).</span></span>

<span data-ttu-id="1325a-106">Ez a cikk ismerteti a következő feladatokat:</span><span class="sxs-lookup"><span data-stu-id="1325a-106">This article covers the following tasks:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="1325a-107">Az Azure portál használatával globális terjesztési konfigurálása</span><span class="sxs-lookup"><span data-stu-id="1325a-107">Configure global distribution using the Azure portal</span></span>
> * <span data-ttu-id="1325a-108">Globális terjesztési használatával konfigurálja a [Graph API-k](graph-introduction.md) (előzetes verzió)</span><span class="sxs-lookup"><span data-stu-id="1325a-108">Configure global distribution using the [Graph APIs](graph-introduction.md) (preview)</span></span>

[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]


## <a name="connecting-to-a-preferred-region-using-the-graph-api-using-the-net-sdk"></a><span data-ttu-id="1325a-109">Csatlakozás előnyben részesített régióba a Graph API a .NET SDK használatával</span><span class="sxs-lookup"><span data-stu-id="1325a-109">Connecting to a preferred region using the Graph API using the .NET SDK</span></span>

<span data-ttu-id="1325a-110">A Graph API fel van fedve egy bővítmény tárként fölött a DocumentDB SDK-t.</span><span class="sxs-lookup"><span data-stu-id="1325a-110">The Graph API is exposed as an extension library on top of the DocumentDB SDK.</span></span>

<span data-ttu-id="1325a-111">Kihasználása érdekében [globális terjesztési](distribute-data-globally.md), ügyfélalkalmazások is adja meg a dokumentum műveletek végrehajtásához használandó régiók rendezett beállítások listáját.</span><span class="sxs-lookup"><span data-stu-id="1325a-111">In order to take advantage of [global distribution](distribute-data-globally.md), client applications can specify the ordered preference list of regions to be used to perform document operations.</span></span> <span data-ttu-id="1325a-112">Ezt megteheti a kapcsolat házirend beállításával.</span><span class="sxs-lookup"><span data-stu-id="1325a-112">This can be done by setting the connection policy.</span></span> <span data-ttu-id="1325a-113">Az Azure Cosmos DB-fiók konfigurációja, az aktuális területi rendelkezésre állás és a megadott beállításokat szabályozó lista alapján, a legoptimálisabb végpont választja ki az SDK írási és olvasási műveletek.</span><span class="sxs-lookup"><span data-stu-id="1325a-113">Based on the Azure Cosmos DB account configuration, current regional availability and the preference list specified, the most optimal endpoint will be chosen by the SDK to perform write and read operations.</span></span>

<span data-ttu-id="1325a-114">Ez a beállítás lista van megadva, az SDK-k használata a kapcsolat inicializálása közben.</span><span class="sxs-lookup"><span data-stu-id="1325a-114">This preference list is specified when initializing a connection using the SDKs.</span></span> <span data-ttu-id="1325a-115">Az SDK-k elfogadása "PreferredLocations" nem kötelező paraméter, amely egy Azure-régiók rendezett listáját.</span><span class="sxs-lookup"><span data-stu-id="1325a-115">The SDKs accept an optional parameter "PreferredLocations" that is an ordered list of Azure regions.</span></span>

* <span data-ttu-id="1325a-116">**Írási műveletek**: az SDK automatikusan elküld minden írási műveleteket ad ki az aktuális írási régió.</span><span class="sxs-lookup"><span data-stu-id="1325a-116">**Writes**: The SDK will automatically send all writes to the current write region.</span></span>
* <span data-ttu-id="1325a-117">**Beolvassa**: minden olvasási kapnak a PreferredLocations lista első rendelkezésre álló terület.</span><span class="sxs-lookup"><span data-stu-id="1325a-117">**Reads**: All reads will be sent to the first available region in the PreferredLocations list.</span></span> <span data-ttu-id="1325a-118">A kérés nem teljesíthető, ha az ügyfél lefelé a listában, a következő régióban sikertelen, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="1325a-118">If the request fails, the client will fail down the list to the next region, and so on.</span></span> <span data-ttu-id="1325a-119">Az SDK-k csak megpróbálja beolvasni a régió van megadva a PreferredLocations.</span><span class="sxs-lookup"><span data-stu-id="1325a-119">The SDKs will only attempt to read from the regions specified in PreferredLocations.</span></span> <span data-ttu-id="1325a-120">Igen például ha három régiók a Cosmos DB fiók áll rendelkezésre, de az ügyfél csak meghatározza a nem írási régiók két PreferredLocations, majd nincs olvasási szolgáltató kívül az írási régió, feladatátvétel esetén is.</span><span class="sxs-lookup"><span data-stu-id="1325a-120">So, for example, if the Cosmos DB account is available in three regions, but the client only specifies two of the non-write regions for PreferredLocations, then no reads will be served out of the write region, even in the case of failover.</span></span>

<span data-ttu-id="1325a-121">Az alkalmazás a jelenlegi írási végpont ellenőrizheti és olvassa el a két tulajdonság, WriteEndpoint és ReadEndpoint, elérhető, a SDK 1.8-as verzióját és az újabb ellenőrzésével az SDK által választott végpont.</span><span class="sxs-lookup"><span data-stu-id="1325a-121">The application can verify the current write endpoint and read endpoint chosen by the SDK by checking two properties, WriteEndpoint and ReadEndpoint, available in SDK version 1.8 and above.</span></span> <span data-ttu-id="1325a-122">A PreferredLocations tulajdonsága nincs beállítva, ha minden kérésnél szolgáltató aktuális írási régióban.</span><span class="sxs-lookup"><span data-stu-id="1325a-122">If the PreferredLocations property is not set, all requests will be served from the current write region.</span></span>

### <a name="using-the-sdk"></a><span data-ttu-id="1325a-123">Az SDK használatával</span><span class="sxs-lookup"><span data-stu-id="1325a-123">Using the SDK</span></span>

<span data-ttu-id="1325a-124">Például a .NET SDK-ban a `ConnectionPolicy` paramétere a `DocumentClient` konstruktor hívása tulajdonsággal rendelkezik `PreferredLocations`.</span><span class="sxs-lookup"><span data-stu-id="1325a-124">For example, in the .NET SDK, the `ConnectionPolicy` parameter for the `DocumentClient` constructor has a property called `PreferredLocations`.</span></span> <span data-ttu-id="1325a-125">Ez a tulajdonság régió neveinek listáját állítható be.</span><span class="sxs-lookup"><span data-stu-id="1325a-125">This property can be set to a list of region names.</span></span> <span data-ttu-id="1325a-126">A megjelenített neveket a [Azure-régiókat] [ regions] részeként adható meg `PreferredLocations`.</span><span class="sxs-lookup"><span data-stu-id="1325a-126">The display names for [Azure Regions][regions] can be specified as part of `PreferredLocations`.</span></span>

> [!NOTE]
> <span data-ttu-id="1325a-127">Az URL-címeket a végpontok nem hosszú élettartamú állandók kell tekinteni.</span><span class="sxs-lookup"><span data-stu-id="1325a-127">The URLs for the endpoints should not be considered as long-lived constants.</span></span> <span data-ttu-id="1325a-128">A szolgáltatás ezek bármikor előfordulhat, hogy frissíteni.</span><span class="sxs-lookup"><span data-stu-id="1325a-128">The service may update these at any point.</span></span> <span data-ttu-id="1325a-129">Az SDK kezeli automatikusan ezt a módosítást.</span><span class="sxs-lookup"><span data-stu-id="1325a-129">The SDK handles this change automatically.</span></span>
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

// connect to Azure Cosmos DB
await docClient.OpenAsync().ConfigureAwait(false);
```

<span data-ttu-id="1325a-130">Ez azt, hogy ez az oktatóanyag befejezése.</span><span class="sxs-lookup"><span data-stu-id="1325a-130">That's it, that completes this tutorial.</span></span> <span data-ttu-id="1325a-131">Megismerheti a globális replikált fiókja konzisztencia kezeléséhez olvasásával [Azure Cosmos DB-ben konzisztenciaszintek](consistency-levels.md).</span><span class="sxs-lookup"><span data-stu-id="1325a-131">You can learn how to manage the consistency of your globally replicated account by reading [Consistency levels in Azure Cosmos DB](consistency-levels.md).</span></span> <span data-ttu-id="1325a-132">És hogyan globális adatbázis-replikációval kapcsolatos további információk az Azure Cosmos Adatbázisba működik, a következő témakörben: [adatok globálisan Azure Cosmos DB terjesztése](distribute-data-globally.md).</span><span class="sxs-lookup"><span data-stu-id="1325a-132">And for more information about how global database replication works in Azure Cosmos DB, see [Distribute data globally with Azure Cosmos DB](distribute-data-globally.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="1325a-133">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1325a-133">Next steps</span></span>

<span data-ttu-id="1325a-134">Ebben az oktatóanyagban ezt a következők:</span><span class="sxs-lookup"><span data-stu-id="1325a-134">In this tutorial, you've done the following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="1325a-135">Az Azure portál használatával globális terjesztési konfigurálása</span><span class="sxs-lookup"><span data-stu-id="1325a-135">Configure global distribution using the Azure portal</span></span>
> * <span data-ttu-id="1325a-136">A DocumentDB API-k használatával globális terjesztési konfigurálása</span><span class="sxs-lookup"><span data-stu-id="1325a-136">Configure global distribution using the DocumentDB APIs</span></span>

<span data-ttu-id="1325a-137">Most már folytathatja a következő oktatóanyag megtudhatja, hogyan fejleszthet, helyileg emulátorral Azure Cosmos DB helyi.</span><span class="sxs-lookup"><span data-stu-id="1325a-137">You can now proceed to the next tutorial to learn how to develop locally using the Azure Cosmos DB local emulator.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="1325a-138">Helyileg emulátorral fejlesztése</span><span class="sxs-lookup"><span data-stu-id="1325a-138">Develop locally with the emulator</span></span>](local-emulator.md)

[regions]: https://azure.microsoft.com/regions/

