---
title: "aaaHow tooData profil adatforrások"
description: "Hogyan-tooarticle hogyan profilok tooinclude tábla és oszlop szintű adatokat, amikor adatforrások regisztrálása az Azure Data Catalog, és hogyan toouse adatok profilok toounderstand adatforrások kiemelve."
services: data-catalog
documentationcenter: 
author: spelluru
manager: NA
editor: 
tags: 
ms.assetid: 94a8274b-5c9c-4962-a4b1-2fed38a3d919
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/03/2017
ms.author: spelluru
ms.openlocfilehash: 12c9f38501cdaee903d0dcbbdd0b82395f35a187
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="data-profile-data-sources"></a><span data-ttu-id="d18e4-103">Adatforrások adatprofiljának készítése</span><span class="sxs-lookup"><span data-stu-id="d18e4-103">Data profile data sources</span></span>
## <a name="introduction"></a><span data-ttu-id="d18e4-104">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="d18e4-104">Introduction</span></span>
<span data-ttu-id="d18e4-105">**A Microsoft Azure Data Catalog** egy teljes körűen felügyelt felhőszolgáltatás, amely a regisztráció és a rendszer a vállalati adatforrások felderítési funkcionál.</span><span class="sxs-lookup"><span data-stu-id="d18e4-105">**Microsoft Azure Data Catalog** is a fully managed cloud service that serves as a system of registration and system of discovery for enterprise data sources.</span></span> <span data-ttu-id="d18e4-106">Más szóval **Azure Data Catalog** összes segítve a személyek ismerje meg, megértése és adatforrások használja, és segíti a szervezeteket tooget több értékét a meglévő adatok.</span><span class="sxs-lookup"><span data-stu-id="d18e4-106">In other words, **Azure Data Catalog** is all about helping people discover, understand, and use data sources, and helping organizations tooget more value from their existing data.</span></span> <span data-ttu-id="d18e4-107">Ha egy adatforrás regisztrálva van **Azure Data Catalog**, a metaadatai másolt és indexelik hello szolgáltatást, de hello szövegegység nincs nem végződhet.</span><span class="sxs-lookup"><span data-stu-id="d18e4-107">When a data source is registered with **Azure Data Catalog**, its metadata is copied and indexed by hello service, but hello story doesn’t end there.</span></span>

<span data-ttu-id="d18e4-108">Hello **adatok profilkészítési** szolgáltatása **Azure Data Catalog** megvizsgálja-e a támogatott adatforrások a katalógusban hello adatait és a statisztikák és adatok információt gyűjt.</span><span class="sxs-lookup"><span data-stu-id="d18e4-108">hello **Data Profiling** feature of **Azure Data Catalog** examines hello data from supported data sources in your catalog and collects statistics and information about that data.</span></span> <span data-ttu-id="d18e4-109">Könnyen tooinclude egy profil az adategységet is.</span><span class="sxs-lookup"><span data-stu-id="d18e4-109">It's easy tooinclude a profile of your data assets.</span></span> <span data-ttu-id="d18e4-110">Amikor regisztrál egy adategységet, válassza ki a **adatok profillal együtt** a hello adatforrás-regisztráló eszköz.</span><span class="sxs-lookup"><span data-stu-id="d18e4-110">When you register a data asset, choose **Include Data Profile** in hello data source registration tool.</span></span>

## <a name="what-is-data-profiling"></a><span data-ttu-id="d18e4-111">Mi az a profilkészítési adatokat</span><span class="sxs-lookup"><span data-stu-id="d18e4-111">What is Data Profiling</span></span>
<span data-ttu-id="d18e4-112">A profilkészítési adatokat regisztrált hello adatforrás hello adatokat vizsgálja, és statisztikák és adatok információt gyűjt.</span><span class="sxs-lookup"><span data-stu-id="d18e4-112">Data profiling examines hello data in hello data source being registered, and collects statistics and information about that data.</span></span> <span data-ttu-id="d18e4-113">Adatforrás-felderítés, során a statisztikai információk segít meghatározni hello adatok toosolve hello megfelelőségét az üzleti probléma.</span><span class="sxs-lookup"><span data-stu-id="d18e4-113">During data source discovery, these statistics can help you determine hello suitability of hello data toosolve their business problem.</span></span>

<!-- In [How toodiscover data sources](data-catalog-how-to-discover.md), you learn about **Azure Data Catalog's** extensive search capabilities including searching for data assets that have a profile. See [How tooinclude a data profile when registering a data source](#howto). -->

<span data-ttu-id="d18e4-114">hello következő adatforrásokat támogatja a profilkészítési adatokat:</span><span class="sxs-lookup"><span data-stu-id="d18e4-114">hello following data sources support data profiling:</span></span>

* <span data-ttu-id="d18e4-115">SQL Server (beleértve az Azure SQL Database és Azure SQL Data Warehouse) táblák és nézetek</span><span class="sxs-lookup"><span data-stu-id="d18e4-115">SQL Server (including Azure SQL DB and Azure SQL Data Warehouse) tables and views</span></span>
* <span data-ttu-id="d18e4-116">Oracle-táblák és nézetek</span><span class="sxs-lookup"><span data-stu-id="d18e4-116">Oracle tables and views</span></span>
* <span data-ttu-id="d18e4-117">Teradata-táblák és nézetek</span><span class="sxs-lookup"><span data-stu-id="d18e4-117">Teradata tables and views</span></span>
* <span data-ttu-id="d18e4-118">Hive táblák</span><span class="sxs-lookup"><span data-stu-id="d18e4-118">Hive tables</span></span>

<span data-ttu-id="d18e4-119">Adategységek regisztrálása segít a felhasználóknak többek között a következőket adatokat-profilok az adatforrásokról, beleértve a kérdések megválaszolása:</span><span class="sxs-lookup"><span data-stu-id="d18e4-119">Including data profiles when registering data assets helps users answer questions about data sources, including:</span></span>

* <span data-ttu-id="d18e4-120">Azt is használt toosolve kell a saját üzleti probléma?</span><span class="sxs-lookup"><span data-stu-id="d18e4-120">Can it be used toosolve my business problem?</span></span>
* <span data-ttu-id="d18e4-121">Hello adatok tooparticular szabványok vagy minták megfelelnek?</span><span class="sxs-lookup"><span data-stu-id="d18e4-121">Does hello data conform tooparticular standards or patterns?</span></span>
* <span data-ttu-id="d18e4-122">Melyek azok hello adatforrás hello rendellenességeket?</span><span class="sxs-lookup"><span data-stu-id="d18e4-122">What are some of hello anomalies of hello data source?</span></span>
* <span data-ttu-id="d18e4-123">Mik azok a lehetséges kihívásai ezeket az adatokat integrálása az alkalmazásban?</span><span class="sxs-lookup"><span data-stu-id="d18e4-123">What are possible challenges of integrating this data into my application?</span></span>

> [!NOTE]
> <span data-ttu-id="d18e4-124">Azt is megteheti dokumentáció tooan eszköz toodescribe hogyan adatok sikerült integrált egy alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="d18e4-124">You can also add documentation tooan asset toodescribe how data could be integrated into an application.</span></span> <span data-ttu-id="d18e4-125">Lásd: [hogyan toodocument adatforrások](data-catalog-how-to-documentation.md).</span><span class="sxs-lookup"><span data-stu-id="d18e4-125">See [How toodocument data sources](data-catalog-how-to-documentation.md).</span></span>
>
>

<a name="howto"/>

## <a name="how-tooinclude-a-data-profile-when-registering-a-data-source"></a><span data-ttu-id="d18e4-126">Hogyan tooinclude egy adatok profiljával egy adatforrás regisztrálása</span><span class="sxs-lookup"><span data-stu-id="d18e4-126">How tooinclude a data profile when registering a data source</span></span>
<span data-ttu-id="d18e4-127">Egyszerű tooinclude adatforrás profil.</span><span class="sxs-lookup"><span data-stu-id="d18e4-127">It's easy tooinclude a profile of your data source.</span></span> <span data-ttu-id="d18e4-128">Amikor regisztrál egy adatforrást a hello **regisztrált objektumok toobe** hello az adatforrás regisztrációja panelje eszköz, válassza a **adatok profillal együtt**.</span><span class="sxs-lookup"><span data-stu-id="d18e4-128">When you register a data source, in hello **Objects toobe registered** panel of hello data source registration tool, choose **Include Data Profile**.</span></span>

![](media/data-catalog-data-profile/data-catalog-register-profile.png)

<span data-ttu-id="d18e4-129">További részletek toolearn, hogyan tooregister adatforrások esetén: [hogyan tooregister adatforrások](data-catalog-how-to-register.md) és [Ismerkedés az Azure Data Catalog](data-catalog-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="d18e4-129">toolearn more about how tooregister data sources, see [How tooregister data sources](data-catalog-how-to-register.md) and [Get started with Azure Data Catalog](data-catalog-get-started.md).</span></span>

## <a name="filtering-on-data-assets-that-include-data-profiles"></a><span data-ttu-id="d18e4-130">Szűrést az adategységek, amely tartalmazza az adat-profilok</span><span class="sxs-lookup"><span data-stu-id="d18e4-130">Filtering on data assets that include data profiles</span></span>
<span data-ttu-id="d18e4-131">tartalmazhatnak, amely tartalmazza az adatok profil toodiscover adategységek, `has:tableDataProfiles` vagy `has:columnsDataProfiles` rendelkezésre álló a keresési feltételeket.</span><span class="sxs-lookup"><span data-stu-id="d18e4-131">toodiscover data assets that include a data profile, you can include `has:tableDataProfiles` or `has:columnsDataProfiles` as one of your search terms.</span></span>

> [!NOTE]
> <span data-ttu-id="d18e4-132">Kiválasztása **adatok profillal együtt** hello adatok adatforrás-regisztráló eszköz tartalmazza a tábla és a oszlopszintű profillal kapcsolatos információk.</span><span class="sxs-lookup"><span data-stu-id="d18e4-132">Selecting **Include Data Profile** in hello data source registration tool includes both table and column-level profile information.</span></span> <span data-ttu-id="d18e4-133">Azonban hello Data Catalog API lehetővé teszi, hogy csak egy készletét tartalmazza profiladatok adatok eszközök toobe regisztrálva.</span><span class="sxs-lookup"><span data-stu-id="d18e4-133">However, hello Data Catalog API allows data assets toobe registered with only one set of profile information included.</span></span>
>
>

## <a name="viewing-data-profile-information"></a><span data-ttu-id="d18e4-134">Adatok profil információk megtekintése</span><span class="sxs-lookup"><span data-stu-id="d18e4-134">Viewing data profile information</span></span>
<span data-ttu-id="d18e4-135">Miután megtalálta a megfelelő adatforrás profillal, hello az profil adatait tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="d18e4-135">Once you find a suitable data source with a profile, you can view hello data profile details.</span></span> <span data-ttu-id="d18e4-136">tooview hello adatok profilt válassza ki egy adategységet, válassza a **adatok profil** hello Data Catalog-portál ablakban.</span><span class="sxs-lookup"><span data-stu-id="d18e4-136">tooview hello data profile, select a data asset and choose **Data Profile** in hello Data Catalog portal window.</span></span>

![](media/data-catalog-data-profile/data-catalog-view.png)

<span data-ttu-id="d18e4-137">Az adatok profil **Azure Data Catalog** jeleníti meg a tábla és oszlop profil információkat, így:</span><span class="sxs-lookup"><span data-stu-id="d18e4-137">A data profile in **Azure Data Catalog** shows table and column profile information including:</span></span>

### <a name="object-data-profile"></a><span data-ttu-id="d18e4-138">Objektum adatok profil</span><span class="sxs-lookup"><span data-stu-id="d18e4-138">Object data profile</span></span>
* <span data-ttu-id="d18e4-139">A sorok számát</span><span class="sxs-lookup"><span data-stu-id="d18e4-139">Number of rows</span></span>
* <span data-ttu-id="d18e4-140">A táblázat mérete</span><span class="sxs-lookup"><span data-stu-id="d18e4-140">Table size</span></span>
* <span data-ttu-id="d18e4-141">Hello objektum utolsó frissítésekor</span><span class="sxs-lookup"><span data-stu-id="d18e4-141">When hello object was last updated</span></span>

### <a name="column-data-profile"></a><span data-ttu-id="d18e4-142">Oszlop adatok profil</span><span class="sxs-lookup"><span data-stu-id="d18e4-142">Column data profile</span></span>
* <span data-ttu-id="d18e4-143">Adattípus oszlop</span><span class="sxs-lookup"><span data-stu-id="d18e4-143">Column data type</span></span>
* <span data-ttu-id="d18e4-144">A distinct érték</span><span class="sxs-lookup"><span data-stu-id="d18e4-144">Number of distinct values</span></span>
* <span data-ttu-id="d18e4-145">NULL értéket tartalmazó sorok száma</span><span class="sxs-lookup"><span data-stu-id="d18e4-145">Number of rows with NULL values</span></span>
* <span data-ttu-id="d18e4-146">Minimum, maximum, átlagos és -értékek szórásának</span><span class="sxs-lookup"><span data-stu-id="d18e4-146">Minimum, maximum, average, and standard deviation for column values</span></span>

## <a name="summary"></a><span data-ttu-id="d18e4-147">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="d18e4-147">Summary</span></span>
<span data-ttu-id="d18e4-148">Profilkészítési adatokat nyújt statisztikai adatokat, és információt regisztrált adatok eszközök toohelp meghatározhatja, hogy hello adatok toosolve üzleti problémák hello megfelelőségét.</span><span class="sxs-lookup"><span data-stu-id="d18e4-148">Data profiling provides statistics and information about registered data assets toohelp you determine hello suitability of hello data toosolve business problems.</span></span> <span data-ttu-id="d18e4-149">Ellátása megjegyzésekkel és adatforrások dokumentálása, valamint adatokat profilok adhat a felhasználóknak bemutatják, az adatokat.</span><span class="sxs-lookup"><span data-stu-id="d18e4-149">Along with annotating, and documenting data sources, data profiles can give users a deeper understanding of your data.</span></span>

## <a name="see-also"></a><span data-ttu-id="d18e4-150">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="d18e4-150">See Also</span></span>
* [<span data-ttu-id="d18e4-151">Hogyan tooregister adatforrások</span><span class="sxs-lookup"><span data-stu-id="d18e4-151">How tooregister data sources</span></span>](data-catalog-how-to-register.md)
* [<span data-ttu-id="d18e4-152">Ismerkedés az Azure Data Catalog szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="d18e4-152">Get started with Azure Data Catalog</span></span>](data-catalog-get-started.md)
