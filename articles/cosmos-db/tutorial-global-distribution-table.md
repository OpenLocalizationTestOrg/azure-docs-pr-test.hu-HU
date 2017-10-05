---
title: "Tábla API Azure Cosmos DB globális telepítési útmutató |} Microsoft Docs"
description: "Megtudhatja, hogyan beállítani az Azure Cosmos DB globális terjesztési tábla API használatával."
services: cosmos-db
keywords: "globális terjesztési, tábla"
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
ms.openlocfilehash: 63c9e530a4982e2e6e478fea56e015fc77851e1d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-setup-azure-cosmos-db-global-distribution-using-the-table-api"></a><span data-ttu-id="fd18a-104">Hogyan lehet beállítani az Azure Cosmos DB globális terjesztési tábla API használatával</span><span class="sxs-lookup"><span data-stu-id="fd18a-104">How to setup Azure Cosmos DB global distribution using the Table API</span></span>

<span data-ttu-id="fd18a-105">Ebben a cikkben megmutatjuk, hogyan használható az Azure portál Azure Cosmos DB globális terjesztési telepítési és a tábla API (előzetes verzió) segítségével csatlakozzon.</span><span class="sxs-lookup"><span data-stu-id="fd18a-105">In this article, we show how to use the Azure portal to setup Azure Cosmos DB global distribution and then connect using the Table API (preview).</span></span>

<span data-ttu-id="fd18a-106">Ez a cikk ismerteti a következő feladatokat:</span><span class="sxs-lookup"><span data-stu-id="fd18a-106">This article covers the following tasks:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="fd18a-107">Az Azure portál használatával globális terjesztési konfigurálása</span><span class="sxs-lookup"><span data-stu-id="fd18a-107">Configure global distribution using the Azure portal</span></span>
> * <span data-ttu-id="fd18a-108">Globális terjesztési használatával konfigurálja a [tábla API](table-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="fd18a-108">Configure global distribution using the [Table API](table-introduction.md)</span></span>

[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]


## <a name="connecting-to-a-preferred-region-using-the-table-api"></a><span data-ttu-id="fd18a-109">A preferált régió tábla API használatával csatlakozik</span><span class="sxs-lookup"><span data-stu-id="fd18a-109">Connecting to a preferred region using the Table API</span></span>

<span data-ttu-id="fd18a-110">Kihasználása érdekében [globális terjesztési](distribute-data-globally.md), ügyfélalkalmazások is adja meg a dokumentum műveletek végrehajtásához használandó régiók rendezett beállítások listáját.</span><span class="sxs-lookup"><span data-stu-id="fd18a-110">In order to take advantage of [global distribution](distribute-data-globally.md), client applications can specify the ordered preference list of regions to be used to perform document operations.</span></span> <span data-ttu-id="fd18a-111">Ezt megteheti úgy, hogy a `TablePreferredLocations` konfigurációs érték az előzetes Azure Storage szolgáltatás SDK az alkalmazás Config.</span><span class="sxs-lookup"><span data-stu-id="fd18a-111">This can be done by setting the `TablePreferredLocations` configuration value in the app config for the preview Azure Storage SDK.</span></span> <span data-ttu-id="fd18a-112">Az Azure Cosmos DB-fiók konfigurációja, az aktuális területi rendelkezésre állás és a megadott beállításokat szabályozó lista alapján, a legoptimálisabb végpont választja ki az Azure Storage szolgáltatás SDK írási és olvasási műveletek.</span><span class="sxs-lookup"><span data-stu-id="fd18a-112">Based on the Azure Cosmos DB account configuration, current regional availability and the preference list specified, the most optimal endpoint will be chosen by the Azure Storage SDK to perform write and read operations.</span></span>

<span data-ttu-id="fd18a-113">A `TablePreferredLocations` olvasása előnyben részesített (többhelyű) helyek vesszővel tagolt listáját kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="fd18a-113">The `TablePreferredLocations` should contain a comma-separated list of preferred (multi-homing) locations for reads.</span></span> <span data-ttu-id="fd18a-114">Minden ügyfél példány e régiók részhalmazát megadhat kis késleltetésű olvasása csatlakozási kísérleteinek kívánt sorrendjét.</span><span class="sxs-lookup"><span data-stu-id="fd18a-114">Each client instance can specify a subset of these regions in the preferred order for low latency reads.</span></span> <span data-ttu-id="fd18a-115">A régiók névvel kell ellátni használatával a [megjelenített neveket](https://msdn.microsoft.com/library/azure/gg441293.aspx), például `West US`.</span><span class="sxs-lookup"><span data-stu-id="fd18a-115">The regions must be named using their [display names](https://msdn.microsoft.com/library/azure/gg441293.aspx), for example, `West US`.</span></span>

<span data-ttu-id="fd18a-116">Az összes olvasási kapnak az első rendelkezésre álló terület a `TablePreferredLocations` listája.</span><span class="sxs-lookup"><span data-stu-id="fd18a-116">All reads will be sent to the first available region in the `TablePreferredLocations` list.</span></span> <span data-ttu-id="fd18a-117">A kérés nem teljesíthető, ha az ügyfél lefelé a listában, a következő régióban sikertelen, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="fd18a-117">If the request fails, the client will fail down the list to the next region, and so on.</span></span>

<span data-ttu-id="fd18a-118">Az SDK-t csak megpróbálja beolvasni a régió van megadva a `TablePreferredLocations`.</span><span class="sxs-lookup"><span data-stu-id="fd18a-118">The SDK will only attempt to read from the regions specified in `TablePreferredLocations`.</span></span> <span data-ttu-id="fd18a-119">Igen, például ha az adatbázis-fiókot a három régióban, de az ügyfél csak meghatározza a nem írási területek két `TablePreferredLocations`, akkor nincs olvasási szolgáltató kívül az írási régió, feladatátvétel esetén is.</span><span class="sxs-lookup"><span data-stu-id="fd18a-119">So, for example, if the Database Account is available in three regions, but the client only specifies two of the non-write regions for `TablePreferredLocations`, then no reads will be served out of the write region, even in the case of failover.</span></span>

<span data-ttu-id="fd18a-120">Az SDK automatikusan elküld minden írási műveleteket ad ki az aktuális írási régió.</span><span class="sxs-lookup"><span data-stu-id="fd18a-120">The SDK will automatically send all writes to the current write region.</span></span>

<span data-ttu-id="fd18a-121">Ha a `TablePreferredLocations` tulajdonság nincs beállítva, az összes kérelem szolgáltató aktuális írási régióban.</span><span class="sxs-lookup"><span data-stu-id="fd18a-121">If the `TablePreferredLocations` property is not set, all requests will be served from the current write region.</span></span>

```xml
    <appSettings>
      <add key="TablePreferredLocations" value="East US, West US, North Europe"/>           
    </appSettings>
```

<span data-ttu-id="fd18a-122">Ez azt, hogy ez az oktatóanyag befejezése.</span><span class="sxs-lookup"><span data-stu-id="fd18a-122">That's it, that completes this tutorial.</span></span> <span data-ttu-id="fd18a-123">Megismerheti a globális replikált fiókja konzisztencia kezeléséhez olvasásával [Azure Cosmos DB-ben konzisztenciaszintek](consistency-levels.md).</span><span class="sxs-lookup"><span data-stu-id="fd18a-123">You can learn how to manage the consistency of your globally replicated account by reading [Consistency levels in Azure Cosmos DB](consistency-levels.md).</span></span> <span data-ttu-id="fd18a-124">És hogyan globális adatbázis-replikációval kapcsolatos további információk az Azure Cosmos Adatbázisba működik, a következő témakörben: [adatok globálisan Azure Cosmos DB terjesztése](distribute-data-globally.md).</span><span class="sxs-lookup"><span data-stu-id="fd18a-124">And for more information about how global database replication works in Azure Cosmos DB, see [Distribute data globally with Azure Cosmos DB](distribute-data-globally.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="fd18a-125">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fd18a-125">Next steps</span></span>

<span data-ttu-id="fd18a-126">Ebben az oktatóanyagban ezt a következők:</span><span class="sxs-lookup"><span data-stu-id="fd18a-126">In this tutorial, you've done the following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="fd18a-127">Az Azure portál használatával globális terjesztési konfigurálása</span><span class="sxs-lookup"><span data-stu-id="fd18a-127">Configure global distribution using the Azure portal</span></span>
> * <span data-ttu-id="fd18a-128">A DocumentDB API-k használatával globális terjesztési konfigurálása</span><span class="sxs-lookup"><span data-stu-id="fd18a-128">Configure global distribution using the DocumentDB APIs</span></span>

<span data-ttu-id="fd18a-129">Most már folytathatja a következő oktatóanyag megtudhatja, hogyan fejleszthet, helyileg emulátorral Azure Cosmos DB helyi.</span><span class="sxs-lookup"><span data-stu-id="fd18a-129">You can now proceed to the next tutorial to learn how to develop locally using the Azure Cosmos DB local emulator.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="fd18a-130">Helyileg emulátorral fejlesztése</span><span class="sxs-lookup"><span data-stu-id="fd18a-130">Develop locally with the emulator</span></span>](local-emulator.md)