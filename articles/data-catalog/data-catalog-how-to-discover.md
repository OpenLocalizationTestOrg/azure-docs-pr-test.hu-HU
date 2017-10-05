---
title: "Az Azure Data Catalog adatforrások felfedezése |} Microsoft Docs"
description: "Ez a cikk emel ki, hogyan találhat meg regisztrált adategységeket az Azure Data Catalog, beleértve a Keresés és szűrés és az Azure Data Catalog-portál a találati kijelölő képességeivel."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: f72ae3a3-6573-4710-89a7-f13555e1968c
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: 9ff67dcb5ecb00440f73f979fd8d2b79a570c674
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-discover-data-sources-in-azure-data-catalog"></a><span data-ttu-id="bb6d0-103">Az Azure Data Catalog adatforrások felfedezése</span><span class="sxs-lookup"><span data-stu-id="bb6d0-103">How to discover data sources in Azure Data Catalog</span></span>
## <a name="introduction"></a><span data-ttu-id="bb6d0-104">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="bb6d0-104">Introduction</span></span>
<span data-ttu-id="bb6d0-105">Az Azure Data Catalog egy teljes körűen felügyelt felhőszolgáltatás, amely a regisztráció és a vállalati adatforrások felderítését a rendszer funkcionál.</span><span class="sxs-lookup"><span data-stu-id="bb6d0-105">Azure Data Catalog is a fully managed cloud service that serves as a system of registration and discovery for enterprise data sources.</span></span> <span data-ttu-id="bb6d0-106">Más szóval a Data Catalog segít személyek felderítése, megismeréséhez és használatához adatforrások, és segít a szervezeteknek több érték lekérése a meglévő adatokat.</span><span class="sxs-lookup"><span data-stu-id="bb6d0-106">In other words, Data Catalog helps people discover, understand, and use data sources, and it helps organizations get more value from their existing data.</span></span> <span data-ttu-id="bb6d0-107">A Data Catalog egy adatforrás regisztrálása után a metaadatait indexelik a szolgáltatást, így könnyen kereshet felderítéséhez szükséges adatok.</span><span class="sxs-lookup"><span data-stu-id="bb6d0-107">After a data source is registered with Data Catalog, its metadata is indexed by the service, so that you can easily search to discover the data you need.</span></span>

## <a name="searching-and-filtering"></a><span data-ttu-id="bb6d0-108">A Keresés és szűrés</span><span class="sxs-lookup"><span data-stu-id="bb6d0-108">Searching and filtering</span></span>
<span data-ttu-id="bb6d0-109">A Data Catalog felderítési két elsődleges mechanizmus használja: a Keresés és szűrés.</span><span class="sxs-lookup"><span data-stu-id="bb6d0-109">Discovery in Data Catalog uses two primary mechanisms: searching and filtering.</span></span>

<span data-ttu-id="bb6d0-110">A keresés nem csupán magától értetődő, de rendkívül hatékony is.</span><span class="sxs-lookup"><span data-stu-id="bb6d0-110">Searching is designed to be both intuitive and powerful.</span></span> <span data-ttu-id="bb6d0-111">Alapértelmezés szerint a keresőkifejezéseket a rendszer összeveti a katalógusban szereplő összes tulajdonsággal, még a felhasználók által beírt dekorációkkal is.</span><span class="sxs-lookup"><span data-stu-id="bb6d0-111">By default, search terms are matched against any property in the catalog, including user-provided annotations.</span></span>

<span data-ttu-id="bb6d0-112">A szűrés a keresést hivatott kiegészíteni.</span><span class="sxs-lookup"><span data-stu-id="bb6d0-112">Filtering is designed to complement searching.</span></span> <span data-ttu-id="bb6d0-113">Kiválaszthatja, hogy adott jellemzőiket, például a szakértők, adatforrástípust, objektumtípus és címkék.</span><span class="sxs-lookup"><span data-stu-id="bb6d0-113">You can select specific characteristics such as experts, data source type, object type, and tags.</span></span> <span data-ttu-id="bb6d0-114">Csak megfelelő adategységeket megtekintheti, és korlátozhatja a keresési eredményeket annak megfelelő eszközökkel.</span><span class="sxs-lookup"><span data-stu-id="bb6d0-114">You can view only matching data assets, and constrain search results to matching assets.</span></span>

<span data-ttu-id="bb6d0-115">A Keresés és szűrés együttes használatával a Data Catalog kell adatforrások felfedezése regisztrált adatforrások gyorsan lépjen.</span><span class="sxs-lookup"><span data-stu-id="bb6d0-115">By using a combination of searching and filtering, you can quickly navigate the data sources that have been registered with Data Catalog to discover the data sources you need.</span></span>

## <a name="search-syntax"></a><span data-ttu-id="bb6d0-116">Keresési szintaxis</span><span class="sxs-lookup"><span data-stu-id="bb6d0-116">Search syntax</span></span>
<span data-ttu-id="bb6d0-117">Bár az alapértelmezett szabad szöveges keresés egyszerű és intuitív, is használható a Data Catalog keresési szintaxisának nagyobb mértékben vezérelheti a keresési eredmények között.</span><span class="sxs-lookup"><span data-stu-id="bb6d0-117">Although the default free text search is simple and intuitive, you can also use Data Catalog search syntax for greater control over the search results.</span></span> <span data-ttu-id="bb6d0-118">Data Catalog keresési a következő módszereket támogatja:</span><span class="sxs-lookup"><span data-stu-id="bb6d0-118">Data Catalog search supports the following techniques:</span></span>

| <span data-ttu-id="bb6d0-119">Módszer</span><span class="sxs-lookup"><span data-stu-id="bb6d0-119">Technique</span></span> | <span data-ttu-id="bb6d0-120">Használat</span><span class="sxs-lookup"><span data-stu-id="bb6d0-120">Use</span></span> | <span data-ttu-id="bb6d0-121">Példa</span><span class="sxs-lookup"><span data-stu-id="bb6d0-121">Example</span></span> |
| --- | --- | --- |
| <span data-ttu-id="bb6d0-122">Az egyszerű keresés</span><span class="sxs-lookup"><span data-stu-id="bb6d0-122">Basic search</span></span> |<span data-ttu-id="bb6d0-123">Az egyszerű keresés által használt egy vagy több keresési feltételeket.</span><span class="sxs-lookup"><span data-stu-id="bb6d0-123">Basic search that uses one or more search terms.</span></span> <span data-ttu-id="bb6d0-124">A eredményei bármely eszközök, amelyek megfelelnek egy vagy több meghatározott egyik tulajdonságnak sem.</span><span class="sxs-lookup"><span data-stu-id="bb6d0-124">Results are any assets that match any property with one or more of the terms specified.</span></span> |`sales data` |
| <span data-ttu-id="bb6d0-125">Tulajdonság keresése</span><span class="sxs-lookup"><span data-stu-id="bb6d0-125">Property scoping</span></span> |<span data-ttu-id="bb6d0-126">Csak az adatforrások, ahol a keresési kifejezés egyeztetve van-e a megadott tulajdonság visszaadása.</span><span class="sxs-lookup"><span data-stu-id="bb6d0-126">Return only data sources where the search term is matched with the specified property.</span></span> |`name:finance` |
| <span data-ttu-id="bb6d0-127">Logikai operátorok</span><span class="sxs-lookup"><span data-stu-id="bb6d0-127">Boolean operators</span></span> |<span data-ttu-id="bb6d0-128">Bővíthetők, vagy a megadásával szűkíthető, a Keresés logikai műveletekkel.</span><span class="sxs-lookup"><span data-stu-id="bb6d0-128">Broaden or narrow a search by using Boolean operations.</span></span> |`finance NOT corporate` |
| <span data-ttu-id="bb6d0-129">Csoportosítás zárójelekkel</span><span class="sxs-lookup"><span data-stu-id="bb6d0-129">Grouping with parenthesis</span></span> |<span data-ttu-id="bb6d0-130">A lekérdezés csoport részei zárójelekkel segítségével logikai elkülönítés érdekében, különösen a logikai operátorokkal együtt használva.</span><span class="sxs-lookup"><span data-stu-id="bb6d0-130">Use parentheses to group parts of the query to achieve logical isolation, especially in conjunction with Boolean operators.</span></span> |`name:finance AND (tags:Q1 OR tags:Q2)` |
| <span data-ttu-id="bb6d0-131">Összehasonlító operátorok</span><span class="sxs-lookup"><span data-stu-id="bb6d0-131">Comparison operators</span></span> |<span data-ttu-id="bb6d0-132">Nem egyenlő összehasonlítások is használni azokhoz a tulajdonságokhoz, szám és adat adattípusúak.</span><span class="sxs-lookup"><span data-stu-id="bb6d0-132">Use comparisons other than equality for properties that have numeric and date data types.</span></span> |`modifiedTime > "11/05/2014"` |

<span data-ttu-id="bb6d0-133">A Data Catalog keresési kapcsolatos további információkért tekintse meg a [Azure Data Catalog](https://msdn.microsoft.com/library/azure/mt267594.aspx) cikk.</span><span class="sxs-lookup"><span data-stu-id="bb6d0-133">For more information about Data Catalog search, see the [Azure Data Catalog](https://msdn.microsoft.com/library/azure/mt267594.aspx) article.</span></span>

## <a name="hit-highlighting"></a><span data-ttu-id="bb6d0-134">Találatok kiemelése</span><span class="sxs-lookup"><span data-stu-id="bb6d0-134">Hit highlighting</span></span>
<span data-ttu-id="bb6d0-135">Amikor megtekinti a keresési eredmények között, minden megjelenített tulajdonságok (például az adatok eszköz neve, leírása és címkék) a megadott keresési feltételeknek megfelelő vannak kiemelve abba, hogy könnyebben azonosíthatja, ezért a megadott eszköz által visszaadott egy adott keresési.</span><span class="sxs-lookup"><span data-stu-id="bb6d0-135">When you view search results, any displayed properties that match the specified search terms (such as the data asset name, description, and tags) are highlighted to make it easier to identify why a given data asset was returned by a given search.</span></span>

> [!NOTE]
> <span data-ttu-id="bb6d0-136">A találatok kiemelése kikapcsolásához használja a **kiemelése** kapcsoló számára a Data Catalog-portált.</span><span class="sxs-lookup"><span data-stu-id="bb6d0-136">To turn off hit highlighting, use the **Highlight** switch in the Data Catalog portal.</span></span>
>
>

<span data-ttu-id="bb6d0-137">Amikor a keresési eredmények között, nem mindig lehet nyilvánvaló ezért egy adategységet megtalálható, még akkor is kiemeléssel találati engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="bb6d0-137">When you view search results, it might not always be obvious why a data asset is included, even with hit highlighting enabled.</span></span> <span data-ttu-id="bb6d0-138">Az összes tulajdonság figyelembe veszi a keresésnél alapértelmezés szerint, mert egy adategységet esetleg visszaadott egyezés miatt az oszlopszintű tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="bb6d0-138">Because all properties are searched by default, a data asset might be returned because of a match on a column-level property.</span></span> <span data-ttu-id="bb6d0-139">És több felhasználó megjegyzéseket fűzhet regisztrált adategységeket saját címkéket és leírásokat, mert nem minden metaadat előfordulhat jelenik meg a keresési eredmények listájában.</span><span class="sxs-lookup"><span data-stu-id="bb6d0-139">And because multiple users can annotate registered data assets with their own tags and descriptions, not all metadata might be displayed in the list of search results.</span></span>

<span data-ttu-id="bb6d0-140">Az alapértelmezett mozaik elrendezés nézet, minden egyes csempe jelenik meg a keresési eredmények tartalmaz egy **nézet keresési kifejezés megegyezik** ikonra, így gyorsan megtekintheti a találatok és a helyét, valamint ugrani rájuk, ha azt szeretné.</span><span class="sxs-lookup"><span data-stu-id="bb6d0-140">In the default tile view, each tile displayed in the search results includes a **View search term matches** icon, so that you can quickly view the number of matches and their location, and to jump to them if you want.</span></span>

 ![Találati kiemelve, és megkeresi a megadott Azure Data Catalog-portálon](./media/data-catalog-how-to-discover/search-matches.png)

## <a name="summary"></a><span data-ttu-id="bb6d0-142">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="bb6d0-142">Summary</span></span>
<span data-ttu-id="bb6d0-143">Mivel a Data Catalog egy adatforrás regisztrálása másolja szerkezeti és leíró metaadatok az adatforrásból a katalógus szolgáltatáshoz, az adatforrás könnyebben megtalálhatóvá és értelmezhetővé válik.</span><span class="sxs-lookup"><span data-stu-id="bb6d0-143">Because registering a data source with Data Catalog copies structural and descriptive metadata from the data source to the catalog service, the data source becomes easier to discover and understand.</span></span> <span data-ttu-id="bb6d0-144">Egy adatforrás regisztrálása után tudja azt deríteni szűrés használatával, és a keresni a Data Catalog-portált.</span><span class="sxs-lookup"><span data-stu-id="bb6d0-144">After you've registered a data source, you can discover it by using filtering and search from within the Data Catalog portal.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bb6d0-145">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bb6d0-145">Next steps</span></span>
* <span data-ttu-id="bb6d0-146">További adatforrások felfedezése kapcsolatos részletes információkért lásd: [Ismerkedés az Azure Data Catalog](data-catalog-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="bb6d0-146">For step-by-step details about how to discover data sources, see [Get Started with Azure Data Catalog](data-catalog-get-started.md).</span></span>
