---
title: "Hogyan adatok profil adatforrások"
description: "Útmutató a cikk adatforrások regisztrálása az Azure Data Catalog tábla és oszlop szintű adatok profilok szerepeljen, és adatok profilok használható az adatforrások megértéséhez kiemelve."
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
ms.openlocfilehash: 8f4174f0ed74706b8275c8b1f0a62753f2834fa2
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="data-profile-data-sources"></a><span data-ttu-id="428f0-103">Adatforrások adatprofiljának készítése</span><span class="sxs-lookup"><span data-stu-id="428f0-103">Data profile data sources</span></span>
## <a name="introduction"></a><span data-ttu-id="428f0-104">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="428f0-104">Introduction</span></span>
<span data-ttu-id="428f0-105">**A Microsoft Azure Data Catalog** egy teljes körűen felügyelt felhőszolgáltatás, amely a regisztráció és a rendszer a vállalati adatforrások felderítési funkcionál.</span><span class="sxs-lookup"><span data-stu-id="428f0-105">**Microsoft Azure Data Catalog** is a fully managed cloud service that serves as a system of registration and system of discovery for enterprise data sources.</span></span> <span data-ttu-id="428f0-106">Más szóval **Azure Data Catalog** minden útmutatás nyújtása a felhasználók felderítése, megismeréséhez és használatához adatforrások, és segíti a szervezeteket kihasználása érdekében a meglévő adatok körül forog.</span><span class="sxs-lookup"><span data-stu-id="428f0-106">In other words, **Azure Data Catalog** is all about helping people discover, understand, and use data sources, and helping organizations to get more value from their existing data.</span></span> <span data-ttu-id="428f0-107">Ha egy adatforrás regisztrálva van **Azure Data Catalog**, a metaadatai másolt és a szolgáltatás indexelni, de a szövegegység nincs nem végződhet.</span><span class="sxs-lookup"><span data-stu-id="428f0-107">When a data source is registered with **Azure Data Catalog**, its metadata is copied and indexed by the service, but the story doesn’t end there.</span></span>

<span data-ttu-id="428f0-108">A **adatok profilkészítési** szolgáltatása **Azure Data Catalog** megvizsgálja-e a támogatott adatforrások a katalógusban szereplő adatok és statisztikák és adatok információt gyűjt.</span><span class="sxs-lookup"><span data-stu-id="428f0-108">The **Data Profiling** feature of **Azure Data Catalog** examines the data from supported data sources in your catalog and collects statistics and information about that data.</span></span> <span data-ttu-id="428f0-109">Nem tartalmazza az adategységet egy profilt.</span><span class="sxs-lookup"><span data-stu-id="428f0-109">It's easy to include a profile of your data assets.</span></span> <span data-ttu-id="428f0-110">Amikor regisztrál egy adategységet, válassza ki a **adatok profillal együtt** az az adatforrás-regisztráló eszköz.</span><span class="sxs-lookup"><span data-stu-id="428f0-110">When you register a data asset, choose **Include Data Profile** in the data source registration tool.</span></span>

## <a name="what-is-data-profiling"></a><span data-ttu-id="428f0-111">Mi az a profilkészítési adatokat</span><span class="sxs-lookup"><span data-stu-id="428f0-111">What is Data Profiling</span></span>
<span data-ttu-id="428f0-112">Profilkészítési adatokat a regisztrált adatforrás adatokat vizsgálja, és statisztikák és adatok információt gyűjt.</span><span class="sxs-lookup"><span data-stu-id="428f0-112">Data profiling examines the data in the data source being registered, and collects statistics and information about that data.</span></span> <span data-ttu-id="428f0-113">Adatforrás-felderítés során a statisztikai információk segítségével meghatározhatja, hogy az adatok az üzleti probléma megoldására megfelelőségét.</span><span class="sxs-lookup"><span data-stu-id="428f0-113">During data source discovery, these statistics can help you determine the suitability of the data to solve their business problem.</span></span>

<!-- In [How to discover data sources](data-catalog-how-to-discover.md), you learn about **Azure Data Catalog's** extensive search capabilities including searching for data assets that have a profile. See [How to include a data profile when registering a data source](#howto). -->

<span data-ttu-id="428f0-114">A következő adatforrások támogatja a profilkészítési adatokat:</span><span class="sxs-lookup"><span data-stu-id="428f0-114">The following data sources support data profiling:</span></span>

* <span data-ttu-id="428f0-115">SQL Server (beleértve az Azure SQL Database és Azure SQL Data Warehouse) táblák és nézetek</span><span class="sxs-lookup"><span data-stu-id="428f0-115">SQL Server (including Azure SQL DB and Azure SQL Data Warehouse) tables and views</span></span>
* <span data-ttu-id="428f0-116">Oracle-táblák és nézetek</span><span class="sxs-lookup"><span data-stu-id="428f0-116">Oracle tables and views</span></span>
* <span data-ttu-id="428f0-117">Teradata-táblák és nézetek</span><span class="sxs-lookup"><span data-stu-id="428f0-117">Teradata tables and views</span></span>
* <span data-ttu-id="428f0-118">Hive táblák</span><span class="sxs-lookup"><span data-stu-id="428f0-118">Hive tables</span></span>

<span data-ttu-id="428f0-119">Adategységek regisztrálása segít a felhasználóknak többek között a következőket adatokat-profilok az adatforrásokról, beleértve a kérdések megválaszolása:</span><span class="sxs-lookup"><span data-stu-id="428f0-119">Including data profiles when registering data assets helps users answer questions about data sources, including:</span></span>

* <span data-ttu-id="428f0-120">Ez használható a saját üzleti probléma megoldására?</span><span class="sxs-lookup"><span data-stu-id="428f0-120">Can it be used to solve my business problem?</span></span>
* <span data-ttu-id="428f0-121">Felel meg az adatok adott szabványoknak vagy minták?</span><span class="sxs-lookup"><span data-stu-id="428f0-121">Does the data conform to particular standards or patterns?</span></span>
* <span data-ttu-id="428f0-122">Melyek azok a adatforrás rendellenességeket?</span><span class="sxs-lookup"><span data-stu-id="428f0-122">What are some of the anomalies of the data source?</span></span>
* <span data-ttu-id="428f0-123">Mik azok a lehetséges kihívásai ezeket az adatokat integrálása az alkalmazásban?</span><span class="sxs-lookup"><span data-stu-id="428f0-123">What are possible challenges of integrating this data into my application?</span></span>

> [!NOTE]
> <span data-ttu-id="428f0-124">Dokumentáció is az eszköz ismertetik, hogyan adatok sikerült integrálni kell egy alkalmazást adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="428f0-124">You can also add documentation to an asset to describe how data could be integrated into an application.</span></span> <span data-ttu-id="428f0-125">Lásd: [adatforrások dokumentálása](data-catalog-how-to-documentation.md).</span><span class="sxs-lookup"><span data-stu-id="428f0-125">See [How to document data sources](data-catalog-how-to-documentation.md).</span></span>
>
>

<a name="howto"/>

## <a name="how-to-include-a-data-profile-when-registering-a-data-source"></a><span data-ttu-id="428f0-126">Hogyan adatok profil tartalmazhat, ha egy adatforrás regisztrálása</span><span class="sxs-lookup"><span data-stu-id="428f0-126">How to include a data profile when registering a data source</span></span>
<span data-ttu-id="428f0-127">Nem tartalmazza az adatforrás egy profilt.</span><span class="sxs-lookup"><span data-stu-id="428f0-127">It's easy to include a profile of your data source.</span></span> <span data-ttu-id="428f0-128">Amikor regisztrál egy adatforrást, a **regisztrálandó objektumok** panelen válassza ki az adatforrás-regisztráló eszköz, **adatok profillal együtt**.</span><span class="sxs-lookup"><span data-stu-id="428f0-128">When you register a data source, in the **Objects to be registered** panel of the data source registration tool, choose **Include Data Profile**.</span></span>

![](media/data-catalog-data-profile/data-catalog-register-profile.png)

<span data-ttu-id="428f0-129">Adatforrások regisztrálása kapcsolatos további információkért lásd: [adatforrások regisztrálása](data-catalog-how-to-register.md) és [Ismerkedés az Azure Data Catalog](data-catalog-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="428f0-129">To learn more about how to register data sources, see [How to register data sources](data-catalog-how-to-register.md) and [Get started with Azure Data Catalog](data-catalog-get-started.md).</span></span>

## <a name="filtering-on-data-assets-that-include-data-profiles"></a><span data-ttu-id="428f0-130">Szűrést az adategységek, amely tartalmazza az adat-profilok</span><span class="sxs-lookup"><span data-stu-id="428f0-130">Filtering on data assets that include data profiles</span></span>
<span data-ttu-id="428f0-131">Adategységeket, amely tartalmazza az adatok profil, megadhat `has:tableDataProfiles` vagy `has:columnsDataProfiles` rendelkezésre álló a keresési feltételeket.</span><span class="sxs-lookup"><span data-stu-id="428f0-131">To discover data assets that include a data profile, you can include `has:tableDataProfiles` or `has:columnsDataProfiles` as one of your search terms.</span></span>

> [!NOTE]
> <span data-ttu-id="428f0-132">Kiválasztása **adatok profillal együtt** az adatforrás frissítésregisztráló eszköz tartalmazza a tábla és a oszlopszintű profillal kapcsolatos információk.</span><span class="sxs-lookup"><span data-stu-id="428f0-132">Selecting **Include Data Profile** in the data source registration tool includes both table and column-level profile information.</span></span> <span data-ttu-id="428f0-133">Azonban a Data Catalog API lehetővé teszi, hogy lehet regisztrálni a profil kacsolódjanak csak egy készletét adategységeket.</span><span class="sxs-lookup"><span data-stu-id="428f0-133">However, the Data Catalog API allows data assets to be registered with only one set of profile information included.</span></span>
>
>

## <a name="viewing-data-profile-information"></a><span data-ttu-id="428f0-134">Adatok profil információk megtekintése</span><span class="sxs-lookup"><span data-stu-id="428f0-134">Viewing data profile information</span></span>
<span data-ttu-id="428f0-135">Miután megtalálta a megfelelő adatforrás profillal, megtekintheti az adatok profil részleteit.</span><span class="sxs-lookup"><span data-stu-id="428f0-135">Once you find a suitable data source with a profile, you can view the data profile details.</span></span> <span data-ttu-id="428f0-136">Az adatok profil megtekintéséhez jelöljön ki egy adategységet, majd válassza **adatok profil** a Data Catalog-portál ablakban.</span><span class="sxs-lookup"><span data-stu-id="428f0-136">To view the data profile, select a data asset and choose **Data Profile** in the Data Catalog portal window.</span></span>

![](media/data-catalog-data-profile/data-catalog-view.png)

<span data-ttu-id="428f0-137">Az adatok profil **Azure Data Catalog** jeleníti meg a tábla és oszlop profil információkat, így:</span><span class="sxs-lookup"><span data-stu-id="428f0-137">A data profile in **Azure Data Catalog** shows table and column profile information including:</span></span>

### <a name="object-data-profile"></a><span data-ttu-id="428f0-138">Objektum adatok profil</span><span class="sxs-lookup"><span data-stu-id="428f0-138">Object data profile</span></span>
* <span data-ttu-id="428f0-139">A sorok számát</span><span class="sxs-lookup"><span data-stu-id="428f0-139">Number of rows</span></span>
* <span data-ttu-id="428f0-140">A táblázat mérete</span><span class="sxs-lookup"><span data-stu-id="428f0-140">Table size</span></span>
* <span data-ttu-id="428f0-141">Az objektum utolsó frissítésekor</span><span class="sxs-lookup"><span data-stu-id="428f0-141">When the object was last updated</span></span>

### <a name="column-data-profile"></a><span data-ttu-id="428f0-142">Oszlop adatok profil</span><span class="sxs-lookup"><span data-stu-id="428f0-142">Column data profile</span></span>
* <span data-ttu-id="428f0-143">Adattípus oszlop</span><span class="sxs-lookup"><span data-stu-id="428f0-143">Column data type</span></span>
* <span data-ttu-id="428f0-144">A distinct érték</span><span class="sxs-lookup"><span data-stu-id="428f0-144">Number of distinct values</span></span>
* <span data-ttu-id="428f0-145">NULL értéket tartalmazó sorok száma</span><span class="sxs-lookup"><span data-stu-id="428f0-145">Number of rows with NULL values</span></span>
* <span data-ttu-id="428f0-146">Minimum, maximum, átlagos és -értékek szórásának</span><span class="sxs-lookup"><span data-stu-id="428f0-146">Minimum, maximum, average, and standard deviation for column values</span></span>

## <a name="summary"></a><span data-ttu-id="428f0-147">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="428f0-147">Summary</span></span>
<span data-ttu-id="428f0-148">Profilkészítési adatokat biztosít a statisztikák és a regisztrált adategységeket segítségével meghatározhatja, hogy az adatok üzleti problémák megoldására való megfelelőségével kapcsolatos információk.</span><span class="sxs-lookup"><span data-stu-id="428f0-148">Data profiling provides statistics and information about registered data assets to help you determine the suitability of the data to solve business problems.</span></span> <span data-ttu-id="428f0-149">Ellátása megjegyzésekkel és adatforrások dokumentálása, valamint adatokat profilok adhat a felhasználóknak bemutatják, az adatokat.</span><span class="sxs-lookup"><span data-stu-id="428f0-149">Along with annotating, and documenting data sources, data profiles can give users a deeper understanding of your data.</span></span>

## <a name="see-also"></a><span data-ttu-id="428f0-150">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="428f0-150">See Also</span></span>
* [<span data-ttu-id="428f0-151">Adatforrások regisztrálása</span><span class="sxs-lookup"><span data-stu-id="428f0-151">How to register data sources</span></span>](data-catalog-how-to-register.md)
* [<span data-ttu-id="428f0-152">Ismerkedés az Azure Data Catalog szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="428f0-152">Get started with Azure Data Catalog</span></span>](data-catalog-get-started.md)
