---
title: "Tábla API aaaAzure Cosmos DB globális terjesztési oktatóanyaga |} Microsoft Docs"
description: "Ismerje meg, hogyan hello tábla API toosetup Azure Cosmos DB globális terjesztési használatával."
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
ms.openlocfilehash: d2a995e09c37f9449856aef2ab707e95eb8a540c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toosetup-azure-cosmos-db-global-distribution-using-hello-table-api"></a><span data-ttu-id="4fecf-104">Hogyan toosetup Azure Cosmos DB globális terjesztési használatával hello tábla API</span><span class="sxs-lookup"><span data-stu-id="4fecf-104">How toosetup Azure Cosmos DB global distribution using hello Table API</span></span>

<span data-ttu-id="4fecf-105">Ez a cikk megmutatjuk, hogyan toouse hello Azure portál toosetup Azure Cosmos DB globális terjesztési, és csatlakoztassa a hello tábla API (előzetes verzió) használatával.</span><span class="sxs-lookup"><span data-stu-id="4fecf-105">In this article, we show how toouse hello Azure portal toosetup Azure Cosmos DB global distribution and then connect using hello Table API (preview).</span></span>

<span data-ttu-id="4fecf-106">Ez a cikk ismerteti a következő feladatok hello:</span><span class="sxs-lookup"><span data-stu-id="4fecf-106">This article covers hello following tasks:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="4fecf-107">Konfigurálja a globális terjesztési hello Azure-portál használatával</span><span class="sxs-lookup"><span data-stu-id="4fecf-107">Configure global distribution using hello Azure portal</span></span>
> * <span data-ttu-id="4fecf-108">Globális terjesztési használatával hello konfigurálása [tábla API](table-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="4fecf-108">Configure global distribution using hello [Table API](table-introduction.md)</span></span>

[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]


## <a name="connecting-tooa-preferred-region-using-hello-table-api"></a><span data-ttu-id="4fecf-109">Csatlakozás előnyben részesített régióba tooa hello tábla API</span><span class="sxs-lookup"><span data-stu-id="4fecf-109">Connecting tooa preferred region using hello Table API</span></span>

<span data-ttu-id="4fecf-110">A rendelés tootake előnyeit [globális terjesztési](distribute-data-globally.md), ügyfélalkalmazások hello rendezett használt tooperform dokumentum műveletek régiók toobe preferencia listája adhat meg.</span><span class="sxs-lookup"><span data-stu-id="4fecf-110">In order tootake advantage of [global distribution](distribute-data-globally.md), client applications can specify hello ordered preference list of regions toobe used tooperform document operations.</span></span> <span data-ttu-id="4fecf-111">Ezt úgy teheti hello beállítása `TablePreferredLocations` hello alkalmazások konfigurációja az Azure Storage szolgáltatás SDK hello előzetes konfigurációs értéket.</span><span class="sxs-lookup"><span data-stu-id="4fecf-111">This can be done by setting hello `TablePreferredLocations` configuration value in hello app config for hello preview Azure Storage SDK.</span></span> <span data-ttu-id="4fecf-112">Hello Azure Cosmos DB fiók konfigurációtól függően aktuális területi rendelkezésre állási és hello beállításokat szabályozó lista megadva, a legtöbb optimális végpont választja ki hello Azure Storage szolgáltatás SDK tooperform hello írási és olvasási műveletek.</span><span class="sxs-lookup"><span data-stu-id="4fecf-112">Based on hello Azure Cosmos DB account configuration, current regional availability and hello preference list specified, hello most optimal endpoint will be chosen by hello Azure Storage SDK tooperform write and read operations.</span></span>

<span data-ttu-id="4fecf-113">Hello `TablePreferredLocations` olvasása előnyben részesített (többhelyű) helyek vesszővel tagolt listáját kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="4fecf-113">hello `TablePreferredLocations` should contain a comma-separated list of preferred (multi-homing) locations for reads.</span></span> <span data-ttu-id="4fecf-114">Minden ügyfél példány előnyben részesített hello ahhoz, hogy kis késleltetésű olvasási adhatja meg e régiók egy részét.</span><span class="sxs-lookup"><span data-stu-id="4fecf-114">Each client instance can specify a subset of these regions in hello preferred order for low latency reads.</span></span> <span data-ttu-id="4fecf-115">hello régiók névvel kell ellátni használatával a [megjelenített neveket](https://msdn.microsoft.com/library/azure/gg441293.aspx), például `West US`.</span><span class="sxs-lookup"><span data-stu-id="4fecf-115">hello regions must be named using their [display names](https://msdn.microsoft.com/library/azure/gg441293.aspx), for example, `West US`.</span></span>

<span data-ttu-id="4fecf-116">Az összes olvasási küldi el toohello első rendelkezésre álló terület hello `TablePreferredLocations` listája.</span><span class="sxs-lookup"><span data-stu-id="4fecf-116">All reads will be sent toohello first available region in hello `TablePreferredLocations` list.</span></span> <span data-ttu-id="4fecf-117">Hello kérelem sikertelen lesz, ha hello ügyfél le hello lista toohello következő terület sikertelen, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="4fecf-117">If hello request fails, hello client will fail down hello list toohello next region, and so on.</span></span>

<span data-ttu-id="4fecf-118">hello SDK csak kísérli meg a megadott hello régiók tooread `TablePreferredLocations`.</span><span class="sxs-lookup"><span data-stu-id="4fecf-118">hello SDK will only attempt tooread from hello regions specified in `TablePreferredLocations`.</span></span> <span data-ttu-id="4fecf-119">Így például ha hello adatbázisfiók három régiókban, de hello csak ügyfélszámítógép két hello nem írási régiók `TablePreferredLocations`, majd nincs olvasási szolgáltató hello írási régió, még akkor is, a feladatátvétel hello eset kívül.</span><span class="sxs-lookup"><span data-stu-id="4fecf-119">So, for example, if hello Database Account is available in three regions, but hello client only specifies two of hello non-write regions for `TablePreferredLocations`, then no reads will be served out of hello write region, even in hello case of failover.</span></span>

<span data-ttu-id="4fecf-120">hello SDK automatikusan elküld minden írások toohello aktuális írási terület.</span><span class="sxs-lookup"><span data-stu-id="4fecf-120">hello SDK will automatically send all writes toohello current write region.</span></span>

<span data-ttu-id="4fecf-121">Ha hello `TablePreferredLocations` tulajdonsága nincs beállítva, az összes kérelem szolgáltató hello aktuális írási régióban.</span><span class="sxs-lookup"><span data-stu-id="4fecf-121">If hello `TablePreferredLocations` property is not set, all requests will be served from hello current write region.</span></span>

```xml
    <appSettings>
      <add key="TablePreferredLocations" value="East US, West US, North Europe"/>           
    </appSettings>
```

<span data-ttu-id="4fecf-122">Ez azt, hogy ez az oktatóanyag befejezése.</span><span class="sxs-lookup"><span data-stu-id="4fecf-122">That's it, that completes this tutorial.</span></span> <span data-ttu-id="4fecf-123">Megismerheti, hogyan toomanage hello olvasásával globálisan replikált fiókja konzisztencia [Azure Cosmos DB-ben konzisztenciaszintek](consistency-levels.md).</span><span class="sxs-lookup"><span data-stu-id="4fecf-123">You can learn how toomanage hello consistency of your globally replicated account by reading [Consistency levels in Azure Cosmos DB](consistency-levels.md).</span></span> <span data-ttu-id="4fecf-124">És hogyan globális adatbázis-replikációval kapcsolatos további információk az Azure Cosmos Adatbázisba működik, a következő témakörben: [adatok globálisan Azure Cosmos DB terjesztése](distribute-data-globally.md).</span><span class="sxs-lookup"><span data-stu-id="4fecf-124">And for more information about how global database replication works in Azure Cosmos DB, see [Distribute data globally with Azure Cosmos DB](distribute-data-globally.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="4fecf-125">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4fecf-125">Next steps</span></span>

<span data-ttu-id="4fecf-126">Ebben az oktatóanyagban hello következő régebben már kötöttek:</span><span class="sxs-lookup"><span data-stu-id="4fecf-126">In this tutorial, you've done hello following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="4fecf-127">Konfigurálja a globális terjesztési hello Azure-portál használatával</span><span class="sxs-lookup"><span data-stu-id="4fecf-127">Configure global distribution using hello Azure portal</span></span>
> * <span data-ttu-id="4fecf-128">Globális terjesztési hello DocumentDB API-k használatával konfigurálása</span><span class="sxs-lookup"><span data-stu-id="4fecf-128">Configure global distribution using hello DocumentDB APIs</span></span>

<span data-ttu-id="4fecf-129">Most már folytathatja a következő útmutató toolearn toohello hogyan helyileg toodevelop hello Azure Cosmos DB helyi emulátor.</span><span class="sxs-lookup"><span data-stu-id="4fecf-129">You can now proceed toohello next tutorial toolearn how toodevelop locally using hello Azure Cosmos DB local emulator.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="4fecf-130">Hello emulátorral helyileg fejlesztése</span><span class="sxs-lookup"><span data-stu-id="4fecf-130">Develop locally with hello emulator</span></span>](local-emulator.md)
