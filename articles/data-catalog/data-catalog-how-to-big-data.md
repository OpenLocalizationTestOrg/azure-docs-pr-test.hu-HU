---
title: "\"a big data\" adatforrások aaaHow toowork |} Microsoft Docs"
description: "Hogyan-tooarticle kijelölő mintákat az Azure Data Catalog használatával \"big data\" adatforrások, beleértve az Azure Blob Storage tárolóban, az Azure Data Lake és a Hadoop HDFS."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 626d1568-0780-4726-bad1-9c5000c6b31a
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: e478f71f26744975a7d7e1784b74bf50b424cf65
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toowork-with-big-data-sources-in-azure-data-catalog"></a><span data-ttu-id="876b9-103">Hogyan toowork a big Data típusú adatok az Azure Data Catalog adatforrások</span><span class="sxs-lookup"><span data-stu-id="876b9-103">How toowork with big data sources in Azure Data Catalog</span></span>
## <a name="introduction"></a><span data-ttu-id="876b9-104">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="876b9-104">Introduction</span></span>
<span data-ttu-id="876b9-105">**A Microsoft Azure Data Catalog** egy teljes körűen felügyelt felhőszolgáltatás, amely a regisztráció és a rendszer a vállalati adatforrások felderítési funkcionál.</span><span class="sxs-lookup"><span data-stu-id="876b9-105">**Microsoft Azure Data Catalog** is a fully managed cloud service that serves as a system of registration and system of discovery for enterprise data sources.</span></span> <span data-ttu-id="876b9-106">Az összes vonatkozó útmutatás nyújtása a felhasználók felderítése, megismeréséhez és használatához adatforrások, és több érték szervezetek tooget segíti a meglévő adatforrásokból, beleértve a big Data típusú adatok is.</span><span class="sxs-lookup"><span data-stu-id="876b9-106">It is all about helping people discover, understand, and use data sources, and helping organizations tooget more value from their existing data sources, including big data.</span></span>

<span data-ttu-id="876b9-107">**Az Azure Data Catalog** támogatja hello Azure Blog Storage-BLOB és a könyvtárak, valamint a Hadoop HDFS fájlok és könyvtárak regisztrációja.</span><span class="sxs-lookup"><span data-stu-id="876b9-107">**Azure Data Catalog** supports hello registration of Azure Blog Storage blobs and directories as well as Hadoop HDFS files and directories.</span></span> <span data-ttu-id="876b9-108">Ezeknek az adatforrásoknak félig strukturált jellege hello nagyfokú rugalmasságot biztosít.</span><span class="sxs-lookup"><span data-stu-id="876b9-108">hello semi-structured nature of these data sources provides great flexibility.</span></span> <span data-ttu-id="876b9-109">Azonban tooget hello regisztrálja őket a legtöbb érték **Azure Data Catalog**, felhasználók figyelembe kell venni, hogyan vannak rendszerezve hello adatforrások.</span><span class="sxs-lookup"><span data-stu-id="876b9-109">However, tooget hello most value from registering them with **Azure Data Catalog**, users must consider how hello data sources are organized.</span></span>

## <a name="directories-as-logical-data-sets"></a><span data-ttu-id="876b9-110">Könyvtárak, logikai adatkészletek</span><span class="sxs-lookup"><span data-stu-id="876b9-110">Directories as logical data sets</span></span>
<span data-ttu-id="876b9-111">Az általános mintázatát big Data típusú adatok forrásból szervezése tootreat könyvtárak, mint logikai adatkészletek.</span><span class="sxs-lookup"><span data-stu-id="876b9-111">A common pattern for organizing big data sources is tootreat directories as logical data sets.</span></span> <span data-ttu-id="876b9-112">Legfelső szintű könyvtárak használt toodefine adatkészlet, addig, amíg a almappák definiáljon partíciókat, és a bennük hello fájlok tárolásához hello adatokat mozgatná.</span><span class="sxs-lookup"><span data-stu-id="876b9-112">Top-level directories are used toodefine a data set, while subfolders define partitions, and hello files they contain store hello data itself.</span></span>

<span data-ttu-id="876b9-113">Ez a minta egy példát a következő lehet:</span><span class="sxs-lookup"><span data-stu-id="876b9-113">An example of this pattern might be:</span></span>

    \vehicle_maintenance_events
        \2013
        \2014
        \2015
            \01
                \2015-01-trailer01.csv
                \2015-01-trailer92.csv
                \2015-01-canister9635.csv
                ...
    \location_tracking_events
        \2013
        ...

<span data-ttu-id="876b9-114">Ebben a példában a vehicle_maintenance_events és location_tracking_events logikai adatkészletek jelölik.</span><span class="sxs-lookup"><span data-stu-id="876b9-114">In this example, vehicle_maintenance_events and location_tracking_events represent logical data sets.</span></span> <span data-ttu-id="876b9-115">Ezek a mappák mindegyikének hónap és év szerint vannak csoportosítva almappák adatok fájlokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="876b9-115">Each of these folders contains data files that are organized by year and month into subfolders.</span></span> <span data-ttu-id="876b9-116">Ezek a mappák mindegyikének tartalmazhat több száz vagy ezer.</span><span class="sxs-lookup"><span data-stu-id="876b9-116">Each of these folders could potentially contain hundreds or thousands of files.</span></span>

<span data-ttu-id="876b9-117">Ebben a mintában az egyes fájlok regisztrálása **Azure Data Catalog** valószínűleg nem értelmezhető.</span><span class="sxs-lookup"><span data-stu-id="876b9-117">In this pattern, registering individual files with **Azure Data Catalog** probably does not make sense.</span></span> <span data-ttu-id="876b9-118">Ehelyett regisztrálja, amelyek megfelelnek a jelentéssel bíró toohello felhasználók hello adatokkal végzett munka hello adatkészletek hello könyvtárak.</span><span class="sxs-lookup"><span data-stu-id="876b9-118">Instead, register hello directories that represent hello data sets that be meaningful toohello users working with hello data.</span></span>

## <a name="reference-data-files"></a><span data-ttu-id="876b9-119">Hivatkozás az adatfájlok</span><span class="sxs-lookup"><span data-stu-id="876b9-119">Reference data files</span></span>
<span data-ttu-id="876b9-120">A kiegészítő minta toostore hivatkozási adatkészletek, mint a fájlokat.</span><span class="sxs-lookup"><span data-stu-id="876b9-120">A complementary pattern is toostore reference data sets as individual files.</span></span> <span data-ttu-id="876b9-121">Ezek az adathalmazok is értelmezhető hello "kicsi" big Data típusú adatok oldalán, és gyakran az analitikus adatok modell hasonló toodimensions.</span><span class="sxs-lookup"><span data-stu-id="876b9-121">These data sets may be thought of as hello 'small' side of big data, and are often similar toodimensions in an analytical data model.</span></span> <span data-ttu-id="876b9-122">Hivatkozás az adatfájlok hello tömeges belül máshol tárolódnak hello big Data típusú adatok tárolási hello adatfájlok környezetben használt tooprovide rekordokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="876b9-122">Reference data files contain records that are used tooprovide context for hello bulk of hello data files stored elsewhere in hello big data store.</span></span>

<span data-ttu-id="876b9-123">Ez a minta egy példát a következő lehet:</span><span class="sxs-lookup"><span data-stu-id="876b9-123">An example of this pattern might be:</span></span>

    \vehicles.csv
    \maintenance_facilities.csv
    \maintenance_types.csv

<span data-ttu-id="876b9-124">Egy elemző vagy az adatok tudósok hello nagyobb könyvtárstruktúrák hello adatait az használatakor lehet-e az hello adatokat a referencia-fájlokban használt tooprovide részletes információkat, amelyek entitások említett tooonly név vagy azonosító alapján hello nagyobb adatokban Állítsa be.</span><span class="sxs-lookup"><span data-stu-id="876b9-124">When an analyst or data scientist is working with hello data contained in hello larger directory structures, hello data in these reference files can be used tooprovide more detailed information for entities that are referred tooonly by name or ID in hello larger data set.</span></span>

<span data-ttu-id="876b9-125">Ebben a mintában az így tooregister hello egyedi hivatkozást tartalmazó adatfájlokat **Azure Data Catalog**.</span><span class="sxs-lookup"><span data-stu-id="876b9-125">In this pattern, it makes sense tooregister hello individual reference data files with **Azure Data Catalog**.</span></span> <span data-ttu-id="876b9-126">Minden fájl jelöl egy adatokat, és minden egyes feliratozva, és egyenként felderített.</span><span class="sxs-lookup"><span data-stu-id="876b9-126">Each file represents a data set, and each one can be annotated and discovered individually.</span></span>

## <a name="alternate-patterns"></a><span data-ttu-id="876b9-127">Alternatív minták</span><span class="sxs-lookup"><span data-stu-id="876b9-127">Alternate patterns</span></span>
<span data-ttu-id="876b9-128">hello hello előző szakaszban leírt minták a big Data típusú adatok tárolási rendezi lehetséges, hogy csak két lehetséges módjait, de minden megvalósítása nem egyezik.</span><span class="sxs-lookup"><span data-stu-id="876b9-128">hello patterns described in hello preceding section are just two possible ways a big data store may be organized, but each implementation is different.</span></span> <span data-ttu-id="876b9-129">Függetlenül attól, hogy az adatforrások felépítése, amikor nagy adatforrások regisztrálása **Azure Data Catalog**, összpontosítani hello fájlok és könyvtárak, amelyek megfelelnek a hello adatkészleteket, amelyek érték tooothers belül regisztrálása a szervezet.</span><span class="sxs-lookup"><span data-stu-id="876b9-129">Regardless of how your data sources are structured, when registering big data sources with **Azure Data Catalog**, focus on registering hello files and directories that represent hello data sets that are of value tooothers within your organization.</span></span> <span data-ttu-id="876b9-130">A fájlok és könyvtárak regisztrálása is megzavarhatják hello katalógus, így nehezebb a felhasználók toofind mi van szükségük.</span><span class="sxs-lookup"><span data-stu-id="876b9-130">Registering all files and directories can clutter hello catalog, making it harder for users toofind what they need.</span></span>

## <a name="summary"></a><span data-ttu-id="876b9-131">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="876b9-131">Summary</span></span>
<span data-ttu-id="876b9-132">Az adatforrások nyilvántartására **Azure Data Catalog** teszi ezeket könnyebb toodiscover és megérteni.</span><span class="sxs-lookup"><span data-stu-id="876b9-132">Registering data sources with **Azure Data Catalog** makes them easier toodiscover and understand.</span></span> <span data-ttu-id="876b9-133">Regisztráció és ellátása megjegyzésekkel hello big Data típusú adatok fájlok és könyvtárak, amelyek megfelelnek a logikai adatkészletek, segíthet felhasználók megkeresheti, hello nagy számukra szükséges adatforrásokat.</span><span class="sxs-lookup"><span data-stu-id="876b9-133">By registering and annotating hello big data files and directories that represent logical data sets, you can help users find and use hello big data sources they need.</span></span>
