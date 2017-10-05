---
title: "A DocumentDB API teljesítményszintet |} Microsoft Docs"
description: "További információk a hogyan DocumentDB API teljesítményszintek lehetővé teszik a tároló / alapon átviteli lefoglalni."
services: cosmos-db
author: mimig1
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: 7dc21c71-47e2-4e06-aa21-e84af52866f4
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/24/2017
ms.author: mimig
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c8d4733e57eb760dbb8e8ca96f6ba55671d1742f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="retiring-the-s1-s2-and-s3-performance-levels"></a><span data-ttu-id="f23d1-103">A S1, S2 és S3 teljesítményszintet kivonása</span><span class="sxs-lookup"><span data-stu-id="f23d1-103">Retiring the S1, S2, and S3 performance levels</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="f23d1-104">A cikkben szereplő S1, S2 és S3 teljesítményszintet használatból van, és már nem érhetők el az új DocumentDB API-fiókokat.</span><span class="sxs-lookup"><span data-stu-id="f23d1-104">The S1, S2, and S3 performance levels discussed in this article are being retired and are no longer available for new DocumentDB API accounts.</span></span>
>

<span data-ttu-id="f23d1-105">Ez a cikk áttekintést S1, S2 és S3 teljesítményszintet, és ismerteti, hogyan a gyűjteményeket, a teljesítmény szinteket használó telepíti át az egypartíciós gyűjtemények 2017 augusztus 1. a.</span><span class="sxs-lookup"><span data-stu-id="f23d1-105">This article provides an overview of S1, S2, and S3 performance levels, and discusses how the collections that use these performance levels will be migrated to single partition collections on August 1st, 2017.</span></span> <span data-ttu-id="f23d1-106">A cikk elolvasása után képes lesz a következő kérdések megválaszolásához:</span><span class="sxs-lookup"><span data-stu-id="f23d1-106">After reading this article, you'll be able to answer the following questions:</span></span>

- [<span data-ttu-id="f23d1-107">Miért van a S1, S2 és S3 teljesítmény szintek hatókörről?</span><span class="sxs-lookup"><span data-stu-id="f23d1-107">Why are the S1, S2, and S3 performance levels being retired?</span></span>](#why-retired)
- [<span data-ttu-id="f23d1-108">Hogyan hajtsa végre az egypartíciós gyűjtemények és a particionált gyűjtemények hasonlítsa össze a S1, S2, S3 teljesítményszintet?</span><span class="sxs-lookup"><span data-stu-id="f23d1-108">How do single partition collections and partitioned collections compare to the S1, S2, S3 performance levels?</span></span>](#compare)
- [<span data-ttu-id="f23d1-109">Mit kell tennie, az adatok folyamatos hozzáférés érdekében?</span><span class="sxs-lookup"><span data-stu-id="f23d1-109">What do I need to do to ensure uninterrupted access to my data?</span></span>](#uninterrupted-access)
- [<span data-ttu-id="f23d1-110">Hogyan változik a saját gyűjteményembe az áttelepítést követően?</span><span class="sxs-lookup"><span data-stu-id="f23d1-110">How will my collection change after the migration?</span></span>](#collection-change)
- [<span data-ttu-id="f23d1-111">Hogyan fogja módosítani a saját számlázási I vagyok történő áttelepítése után az egypartíciós gyűjtemények?</span><span class="sxs-lookup"><span data-stu-id="f23d1-111">How will my billing change after I’m migrated to single partition collections?</span></span>](#billing-change)
- [<span data-ttu-id="f23d1-112">Mi történik, ha több mint 10 GB tárhelyet kell?</span><span class="sxs-lookup"><span data-stu-id="f23d1-112">What if I need more than 10 GB of storage?</span></span>](#more-storage-needed)
- [<span data-ttu-id="f23d1-113">Módosítható a S1, S2 és S3 között teljesítményszintet 2017. augusztus 1. előtt?</span><span class="sxs-lookup"><span data-stu-id="f23d1-113">Can I change between the S1, S2, and S3 performance levels before August 1, 2017?</span></span>](#change-before)
- [<span data-ttu-id="f23d1-114">Milyen operációs rendszer, hogy ha a gyűjtemény át lett telepítve?</span><span class="sxs-lookup"><span data-stu-id="f23d1-114">How will I know when my collection has migrated?</span></span>](#when-migrated)
- [<span data-ttu-id="f23d1-115">Hogyan tudom át az S1, S2, S3 teljesítményszintek az egypartíciós gyűjtemények önállóan?</span><span class="sxs-lookup"><span data-stu-id="f23d1-115">How do I migrate from the S1, S2, S3 performance levels to single partition collections on my own?</span></span>](#migrate-diy)
- [<span data-ttu-id="f23d1-116">Hogyan vagyok feladatátvétele a Ha az ügyfél egy EA vagyok?</span><span class="sxs-lookup"><span data-stu-id="f23d1-116">How am I impacted if I'm an EA customer?</span></span>](#ea-customer)

<a name="why-retired"></a>

## <a name="why-are-the-s1-s2-and-s3-performance-levels-being-retired"></a><span data-ttu-id="f23d1-117">Miért van a S1, S2 és S3 teljesítmény szintek hatókörről?</span><span class="sxs-lookup"><span data-stu-id="f23d1-117">Why are the S1, S2, and S3 performance levels being retired?</span></span>

<span data-ttu-id="f23d1-118">A S1, S2 és S3 teljesítményszintet biztosít, amely DocumentDB API gyűjtemények nyújt rugalmasságot.</span><span class="sxs-lookup"><span data-stu-id="f23d1-118">The S1, S2, and S3 performance levels do not offer the flexibility that DocumentDB API collections offers.</span></span> <span data-ttu-id="f23d1-119">S1, S2, S3 teljesítményszintet, az átviteli sebesség és a tárolási kapacitás előre beállított és nem ajánlja fel a rugalmasság.</span><span class="sxs-lookup"><span data-stu-id="f23d1-119">With the S1, S2, S3 performance levels, both the throughput and storage capacity were pre-set and did not offer elasticity.</span></span> <span data-ttu-id="f23d1-120">Azure Cosmos DB kínál testreszabása az átviteli sebesség és tárterület, felkínálva sokkal nagyobb rugalmasságot biztosít arra, hogy a méretezés pedig az igényeinek módosítása.</span><span class="sxs-lookup"><span data-stu-id="f23d1-120">Azure Cosmos DB now offers the ability to customize your throughput and storage, offering you much more flexibility in your ability to scale as your needs change.</span></span>

<a name="compare"></a>

## <a name="how-do-single-partition-collections-and-partitioned-collections-compare-to-the-s1-s2-s3-performance-levels"></a><span data-ttu-id="f23d1-121">Hogyan hajtsa végre az egypartíciós gyűjtemények és a particionált gyűjtemények hasonlítsa össze a S1, S2, S3 teljesítményszintet?</span><span class="sxs-lookup"><span data-stu-id="f23d1-121">How do single partition collections and partitioned collections compare to the S1, S2, S3 performance levels?</span></span>

<span data-ttu-id="f23d1-122">Az alábbi táblázat összehasonlítja az átviteli sebesség és tárterület beállításait az egypartíciós gyűjtemények, a particionált gyűjtemények és S1, S2, S3 teljesítményszintet.</span><span class="sxs-lookup"><span data-stu-id="f23d1-122">The following table compares the throughput and storage options available in single partition collections, partitioned collections, and S1, S2, S3 performance levels.</span></span> <span data-ttu-id="f23d1-123">Íme egy példa a amerikai keleti régiója 2 régió:</span><span class="sxs-lookup"><span data-stu-id="f23d1-123">Here is an example for US East 2 region:</span></span>

|   |<span data-ttu-id="f23d1-124">Particionált gyűjtemény</span><span class="sxs-lookup"><span data-stu-id="f23d1-124">Partitioned collection</span></span>|<span data-ttu-id="f23d1-125">Az egypartíciós gyűjtemény</span><span class="sxs-lookup"><span data-stu-id="f23d1-125">Single partition collection</span></span>|<span data-ttu-id="f23d1-126">S1</span><span class="sxs-lookup"><span data-stu-id="f23d1-126">S1</span></span>|<span data-ttu-id="f23d1-127">S2</span><span class="sxs-lookup"><span data-stu-id="f23d1-127">S2</span></span>|<span data-ttu-id="f23d1-128">S3</span><span class="sxs-lookup"><span data-stu-id="f23d1-128">S3</span></span>|
|---|---|---|---|---|---|
|<span data-ttu-id="f23d1-129">Maximális átviteli sebesség</span><span class="sxs-lookup"><span data-stu-id="f23d1-129">Maximum throughput</span></span>|<span data-ttu-id="f23d1-130">Korlátlan</span><span class="sxs-lookup"><span data-stu-id="f23d1-130">Unlimited</span></span>|<span data-ttu-id="f23d1-131">10 KB-os RU/mp</span><span class="sxs-lookup"><span data-stu-id="f23d1-131">10K RU/s</span></span>|<span data-ttu-id="f23d1-132">250 RU/mp</span><span class="sxs-lookup"><span data-stu-id="f23d1-132">250 RU/s</span></span>|<span data-ttu-id="f23d1-133">1 K RU/mp</span><span class="sxs-lookup"><span data-stu-id="f23d1-133">1 K RU/s</span></span>|<span data-ttu-id="f23d1-134">2.5-K RU/mp</span><span class="sxs-lookup"><span data-stu-id="f23d1-134">2.5 K RU/s</span></span>|
|<span data-ttu-id="f23d1-135">Minimális átviteli sebesség</span><span class="sxs-lookup"><span data-stu-id="f23d1-135">Minimum throughput</span></span>|<span data-ttu-id="f23d1-136">2.5-K RU/mp</span><span class="sxs-lookup"><span data-stu-id="f23d1-136">2.5K RU/s</span></span>|<span data-ttu-id="f23d1-137">400 RU/mp</span><span class="sxs-lookup"><span data-stu-id="f23d1-137">400 RU/s</span></span>|<span data-ttu-id="f23d1-138">250 RU/mp</span><span class="sxs-lookup"><span data-stu-id="f23d1-138">250 RU/s</span></span>|<span data-ttu-id="f23d1-139">1 K RU/mp</span><span class="sxs-lookup"><span data-stu-id="f23d1-139">1 K RU/s</span></span>|<span data-ttu-id="f23d1-140">2.5-K RU/mp</span><span class="sxs-lookup"><span data-stu-id="f23d1-140">2.5 K RU/s</span></span>|
|<span data-ttu-id="f23d1-141">Maximális tárolóméret</span><span class="sxs-lookup"><span data-stu-id="f23d1-141">Maximum storage</span></span>|<span data-ttu-id="f23d1-142">Korlátlan</span><span class="sxs-lookup"><span data-stu-id="f23d1-142">Unlimited</span></span>|<span data-ttu-id="f23d1-143">10 GB</span><span class="sxs-lookup"><span data-stu-id="f23d1-143">10 GB</span></span>|<span data-ttu-id="f23d1-144">10 GB</span><span class="sxs-lookup"><span data-stu-id="f23d1-144">10 GB</span></span>|<span data-ttu-id="f23d1-145">10 GB</span><span class="sxs-lookup"><span data-stu-id="f23d1-145">10 GB</span></span>|<span data-ttu-id="f23d1-146">10 GB</span><span class="sxs-lookup"><span data-stu-id="f23d1-146">10 GB</span></span>|
|<span data-ttu-id="f23d1-147">Árlista (havonta)</span><span class="sxs-lookup"><span data-stu-id="f23d1-147">Price (monthly)</span></span>|<span data-ttu-id="f23d1-148">Átviteli sebesség: $6 / 100 RU/mp</span><span class="sxs-lookup"><span data-stu-id="f23d1-148">Throughput: $6 / 100 RU/s</span></span><br><br><span data-ttu-id="f23d1-149">Tárolás: $ 0,25/GB</span><span class="sxs-lookup"><span data-stu-id="f23d1-149">Storage: $0.25/GB</span></span>|<span data-ttu-id="f23d1-150">Átviteli sebesség: $6 / 100 RU/mp</span><span class="sxs-lookup"><span data-stu-id="f23d1-150">Throughput: $6 / 100 RU/s</span></span><br><br><span data-ttu-id="f23d1-151">Tárolás: $ 0,25/GB</span><span class="sxs-lookup"><span data-stu-id="f23d1-151">Storage: $0.25/GB</span></span>|<span data-ttu-id="f23d1-152">$25 USD</span><span class="sxs-lookup"><span data-stu-id="f23d1-152">$25 USD</span></span>|<span data-ttu-id="f23d1-153">$50 USD</span><span class="sxs-lookup"><span data-stu-id="f23d1-153">$50 USD</span></span>|<span data-ttu-id="f23d1-154">$100 USD</span><span class="sxs-lookup"><span data-stu-id="f23d1-154">$100 USD</span></span>|

<span data-ttu-id="f23d1-155">Az ügyfél egy EA folytatja?</span><span class="sxs-lookup"><span data-stu-id="f23d1-155">Are you an EA customer?</span></span> <span data-ttu-id="f23d1-156">Ha igen, tekintse meg a [hogyan vagyok I csökkentheti az EA felhasználóinál vagyok?](#ea-customer)</span><span class="sxs-lookup"><span data-stu-id="f23d1-156">If so, see [How am I impacted if I'm an EA customer?](#ea-customer)</span></span>

<a name="uninterrupted-access"></a>

## <a name="what-do-i-need-to-do-to-ensure-uninterrupted-access-to-my-data"></a><span data-ttu-id="f23d1-157">Mit kell tennie, az adatok folyamatos hozzáférés érdekében?</span><span class="sxs-lookup"><span data-stu-id="f23d1-157">What do I need to do to ensure uninterrupted access to my data?</span></span>

<span data-ttu-id="f23d1-158">Semmi, Cosmos DB kezeli, az áttelepítés.</span><span class="sxs-lookup"><span data-stu-id="f23d1-158">Nothing, Cosmos DB handles the migration for you.</span></span> <span data-ttu-id="f23d1-159">Ha egy S1, S2 vagy S3 gyűjteményt, a jelenlegi gyűjtemény telepíti át a 2017. július 31 egypartíciós gyűjtemény.</span><span class="sxs-lookup"><span data-stu-id="f23d1-159">If you have an S1, S2, or S3 collection, your current collection will be migrated to a single partition collection on July 31, 2017.</span></span> 

<a name="collection-change"></a>

## <a name="how-will-my-collection-change-after-the-migration"></a><span data-ttu-id="f23d1-160">Hogyan változik a saját gyűjteményembe az áttelepítést követően?</span><span class="sxs-lookup"><span data-stu-id="f23d1-160">How will my collection change after the migration?</span></span>

<span data-ttu-id="f23d1-161">Ha egy S1 gyűjteményt, akkor telepíti át egy egypartíciós gyűjtemény átviteli sebességgel 400 RU/mp.</span><span class="sxs-lookup"><span data-stu-id="f23d1-161">If you have an S1 collection, you will be migrated to a single partition collection with 400 RU/s throughput.</span></span> <span data-ttu-id="f23d1-162">400 RU/mp a legalacsonyabb átviteli sebesség érhető el az egypartíciós gyűjtemények.</span><span class="sxs-lookup"><span data-stu-id="f23d1-162">400 RU/s is the lowest throughput available with single partition collections.</span></span> <span data-ttu-id="f23d1-163">Azonban a 400 RU/mp a költsége az egypartíciós gyűjtemény célja megközelítőleg azonos, amelyben meg volt fizető S1 gyűjteményének és 250 RU/mp –, így nem kell fizet az extra 150 RU/mp elérhető.</span><span class="sxs-lookup"><span data-stu-id="f23d1-163">However, the cost for 400 RU/s in the a single partition collection is approximately the same as you were paying with your S1 collection and 250 RU/s – so you are not paying for the extra 150 RU/s available to you.</span></span>

<span data-ttu-id="f23d1-164">Ha egy S2 gyűjteményt, akkor telepíti át egy 1 KB-os RU/mp egypartíciós gyűjtemény.</span><span class="sxs-lookup"><span data-stu-id="f23d1-164">If you have an S2 collection, you will be migrated to a single partition collection with 1 K RU/s.</span></span> <span data-ttu-id="f23d1-165">Nincs változás az átviteli szinten jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="f23d1-165">You will see no change to your throughput level.</span></span>

<span data-ttu-id="f23d1-166">Ha egy S3 gyűjteményt, akkor telepíti át egy egypartíciós gyűjtemény 2,5 K RU/mp.</span><span class="sxs-lookup"><span data-stu-id="f23d1-166">If you have an S3 collection, you will be migrated to a single partition collection with 2.5 K RU/s.</span></span> <span data-ttu-id="f23d1-167">Nincs változás az átviteli szinten jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="f23d1-167">You will see no change to your throughput level.</span></span>

<span data-ttu-id="f23d1-168">Minden ezekben az esetekben miután áttelepítette a gyűjteményt, meg fog tudni az átviteli szintű testreszabása, vagy skálázhatja azt felfelé és lefelé alacsony késésű hozzáférést biztosítani a felhasználók igény szerint.</span><span class="sxs-lookup"><span data-stu-id="f23d1-168">In each of these cases, after your collection is migrated, you will be able to customize your throughput level, or scale it up and down as needed to provide low-latency access to your users.</span></span> <span data-ttu-id="f23d1-169">Átviteli szintjének módosítása után a gyűjtemény át lett telepítve, egyszerűen nyissa meg a Cosmos DB fiók az Azure portálon, kattintson a Scale, válassza ki a gyűjteményt, és az alábbi képernyőfelvételen látható módon adja meg az átviteli szintű:</span><span class="sxs-lookup"><span data-stu-id="f23d1-169">To change the throughput level after your collection has migrated, simply open your Cosmos DB account in the Azure portal, click Scale, choose your collection, and then adjust the throughput level, as shown in the following screenshot:</span></span>

![Az Azure portálon átviteli méretezése](./media/performance-levels/portal-scale-throughput.png)

<a name="billing-change"></a>

## <a name="how-will-my-billing-change-after-im-migrated-to-the-single-partition-collections"></a><span data-ttu-id="f23d1-171">Hogyan fogja módosítani a saját számlázási I vagyok történő áttelepítése után az egypartíciós gyűjtemények?</span><span class="sxs-lookup"><span data-stu-id="f23d1-171">How will my billing change after I’m migrated to the single partition collections?</span></span>

<span data-ttu-id="f23d1-172">Feltéve, hogy 10 S1 gyűjtemények, 1 GB tárhelyet minden, a US keleti terület rendelkezik, és ezek 10 S1 gyűjteményt telepít át, a 10 az egypartíciós gyűjtemények: 400 RU/mp (minimális).</span><span class="sxs-lookup"><span data-stu-id="f23d1-172">Assuming you have 10 S1 collections, 1 GB of storage for each, in the US East region, and you migrate these 10 S1 collections to 10 single partition collections at 400 RU/sec (the minimum level).</span></span> <span data-ttu-id="f23d1-173">A számlázási következőképpen fog kinézni, ha a 10 az egypartíciós gyűjtemények esetében a teljes hónap:</span><span class="sxs-lookup"><span data-stu-id="f23d1-173">Your bill will look as follows if you keep the 10 single partition collections for a full month:</span></span>

![Hogyan S1 tarifacsomag 10 gyűjtemények összehasonlítja 10 gyűjtemények használatával az egypartíciós gyűjtemény díjszabása](./media/performance-levels/s1-vs-standard-pricing.png)

<a name="more-storage-needed"></a>

## <a name="what-if-i-need-more-than-10-gb-of-storage"></a><span data-ttu-id="f23d1-175">Mi történik, ha több mint 10 GB tárhelyet kell?</span><span class="sxs-lookup"><span data-stu-id="f23d1-175">What if I need more than 10 GB of storage?</span></span>

<span data-ttu-id="f23d1-176">E rendelkezik egy gyűjtemény egy S1, S2 vagy S3 teljesítményszint szükséges, vagy ezek mindegyike rendelkezik, 10 GB-os kapacitású, a Cosmos DB adatáttelepítés eszközzel az adatok áttelepítéséhez egy particionált gyűjtemény gyakorlatilag az egypartíciós gyűjtemény korlátlan tárterület.</span><span class="sxs-lookup"><span data-stu-id="f23d1-176">Whether you have a collection with an S1, S2, or S3 performance level, or have a single partition collection, all of which have 10 GB of storage available, you can use the Cosmos DB Data Migration tool to migrate your data to a partitioned collection with virtually unlimited storage.</span></span> <span data-ttu-id="f23d1-177">A particionált gyűjtemény előnyeivel kapcsolatos információk: [particionálás és az Azure Cosmos Adatbázisba skálázás](documentdb-partition-data.md).</span><span class="sxs-lookup"><span data-stu-id="f23d1-177">For information about the benefits of a partitioned collection, see [Partitioning and scaling in Azure Cosmos DB](documentdb-partition-data.md).</span></span> <span data-ttu-id="f23d1-178">A S1, S2, S3 vagy az egypartíciós gyűjtemény egy particionált gyűjtemény áttelepítésével kapcsolatos információkért lásd: [egypartíciós telepít át a particionált gyűjtemények](documentdb-partition-data.md#migrating-from-single-partition).</span><span class="sxs-lookup"><span data-stu-id="f23d1-178">For information about how to migrate your S1, S2, S3, or single partition collection to a partitioned collection, see [Migrating from single-partition to partitioned collections](documentdb-partition-data.md#migrating-from-single-partition).</span></span> 

<a name="change-before"></a>

## <a name="can-i-change-between-the-s1-s2-and-s3-performance-levels-before-august-1-2017"></a><span data-ttu-id="f23d1-179">Módosítható a S1, S2 és S3 között teljesítményszintet 2017. augusztus 1. előtt?</span><span class="sxs-lookup"><span data-stu-id="f23d1-179">Can I change between the S1, S2, and S3 performance levels before August 1, 2017?</span></span>

<span data-ttu-id="f23d1-180">S1, S2 és S3 teljesítménnyel csak meglévő fiókok tudják módosítani, és módosítsa a teljesítmény szintű rétegek a portálon vagy programozottan.</span><span class="sxs-lookup"><span data-stu-id="f23d1-180">Only existing accounts with S1, S2, and S3 performance will be able to change and alter performance level tiers through the portal or programmatically.</span></span> <span data-ttu-id="f23d1-181">2017. augusztus 1., amelyet a S1, S2 és S3 teljesítményszintet már nem lesz elérhető.</span><span class="sxs-lookup"><span data-stu-id="f23d1-181">By August 1, 2017, the S1, S2, and S3 performance levels will no longer be available.</span></span> <span data-ttu-id="f23d1-182">Ha egy egypartíciós gyűjtemény S1, S3 vagy S3 módosítja, a S1, S2 vagy S3 teljesítményszintet nem lehet visszatérni.</span><span class="sxs-lookup"><span data-stu-id="f23d1-182">If you change from S1, S3, or S3 to a single partition collection, you cannot return to the S1, S2, or S3 performance levels.</span></span>

<a name="when-migrated"></a>

## <a name="how-will-i-know-when-my-collection-has-migrated"></a><span data-ttu-id="f23d1-183">Milyen operációs rendszer, hogy ha a gyűjtemény át lett telepítve?</span><span class="sxs-lookup"><span data-stu-id="f23d1-183">How will I know when my collection has migrated?</span></span>

<span data-ttu-id="f23d1-184">Az áttelepítés 2017. július 31 fordulhat elő.</span><span class="sxs-lookup"><span data-stu-id="f23d1-184">The migration will occur on July 31, 2017.</span></span> <span data-ttu-id="f23d1-185">Ha a S1 alkalmazó gyűjteményt, S2 vagy S3 teljesítményszintet, a Cosmos DB csapata kapcsolatba lép Önnel e-mailben történik az áttelepítés előtt.</span><span class="sxs-lookup"><span data-stu-id="f23d1-185">If you have a collection that uses the S1, S2 or S3 performance levels, the Cosmos DB team will contact you by email before the migration takes place.</span></span> <span data-ttu-id="f23d1-186">Az áttelepítés befejezése után, a 2017. augusztus 1., az Azure-portálon jelenik meg, hogy a gyűjtemény használja, a Standard díjszabás.</span><span class="sxs-lookup"><span data-stu-id="f23d1-186">Once the migration is complete, on August 1, 2017, the Azure portal will show that your collection uses Standard pricing.</span></span>

![Bemutatja, hogyan ellenőrizheti a gyűjtemény át lett telepítve, a standard tarifacsomag](./media/performance-levels/portal-standard-pricing-applied.png)

<a name="migrate-diy"></a>

## <a name="how-do-i-migrate-from-the-s1-s2-s3-performance-levels-to-single-partition-collections-on-my-own"></a><span data-ttu-id="f23d1-188">Hogyan tudom át az S1, S2, S3 teljesítményszintek az egypartíciós gyűjtemények önállóan?</span><span class="sxs-lookup"><span data-stu-id="f23d1-188">How do I migrate from the S1, S2, S3 performance levels to single partition collections on my own?</span></span>

<span data-ttu-id="f23d1-189">A S1, S2 és S3 teljesítményszintek az Azure portál használatával az egypartíciós gyűjtemények áttelepíthetők vagy programon keresztül.</span><span class="sxs-lookup"><span data-stu-id="f23d1-189">You can migrate from the S1, S2, and S3 performance levels to single partition collections using the Azure portal or programmatically.</span></span> <span data-ttu-id="f23d1-190">Ehhez a saját augusztus 1 rugalmas átviteli lehetőségekről az egypartíciós gyűjtemények kihasználják előtt, vagy áthelyezni, a gyűjtemények a 2017. július 31.</span><span class="sxs-lookup"><span data-stu-id="f23d1-190">You can do this on your own before August 1 to benefit from the flexible throughput options available with single partition collections, or we will migrate your collections for you on July 31, 2017.</span></span>

<span data-ttu-id="f23d1-191">**Az Azure portál használata az egypartíciós gyűjtemények áttelepítése**</span><span class="sxs-lookup"><span data-stu-id="f23d1-191">**To migrate to single partition collections using the Azure portal**</span></span>

1. <span data-ttu-id="f23d1-192">A a [ **Azure-portálon**](https://portal.azure.com), kattintson a **Azure Cosmos DB**, majd válassza ki a Cosmos DB fiók módosítása.</span><span class="sxs-lookup"><span data-stu-id="f23d1-192">In the [**Azure portal**](https://portal.azure.com), click **Azure Cosmos DB**, then select the Cosmos DB account to modify.</span></span> 
 
    <span data-ttu-id="f23d1-193">Ha **Azure Cosmos DB** értéke nem az Ugrósávon kattintson >, görgessen **adatbázisok**, jelölje be **Azure Cosmos DB**, majd válassza ki a DocumentDB-fiók.</span><span class="sxs-lookup"><span data-stu-id="f23d1-193">If **Azure Cosmos DB** is not on the Jumpbar, click >, scroll to **Databases**, select **Azure Cosmos DB**, and then select the DocumentDB account.</span></span>  

2. <span data-ttu-id="f23d1-194">Erőforrás menüjében a **tárolók**, kattintson a **méretezési**, válassza ki a gyűjteményt, és kattintson a legördülő listából módosításához **árképzési szintjében**.</span><span class="sxs-lookup"><span data-stu-id="f23d1-194">On the resource menu, under **Containers**, click **Scale**, select the collection to modify from the drop-down list, and then click **Pricing Tier**.</span></span> <span data-ttu-id="f23d1-195">Előre definiált átviteli használatával fiókok jogosultak az S1, S2 vagy S3 tarifacsomagot.</span><span class="sxs-lookup"><span data-stu-id="f23d1-195">Accounts using pre-defined throughput have a pricing tier of S1, S2, or S3.</span></span>  <span data-ttu-id="f23d1-196">A a **válasszon tarifacsomagot** panelen kattintson **szabványos** felhasználói átviteli módosítsa, majd **válasszon** menteni a módosítást.</span><span class="sxs-lookup"><span data-stu-id="f23d1-196">In the **Choose your pricing tier** blade, click **Standard** to change to user-defined throughput, and then click **Select** to save your change.</span></span>

    ![A beállítások panelről, hol változtatható meg az átviteli sebesség értéket ábrázoló képernyőfelvétel](./media/performance-levels/change-performance-set-thoughput.png)

3. <span data-ttu-id="f23d1-198">Vissza a **méretezési** panelen a **Tarifacsomagot** változott **szabványos** és a **átviteli sebesség (RU/mp)** mezőben jelenik meg az alapértelmezett érték 400 értéke.</span><span class="sxs-lookup"><span data-stu-id="f23d1-198">Back in the **Scale** blade, the **Pricing Tier** is changed to **Standard** and the **Throughput (RU/s)** box is displayed with a default value of 400.</span></span> <span data-ttu-id="f23d1-199">Állítsa be az átviteli sebesség 400 és 10 000 között [egységek kérelem](request-units.md)/second (RU/mp).</span><span class="sxs-lookup"><span data-stu-id="f23d1-199">Set the throughput between 400 and 10,000 [Request units](request-units.md)/second (RU/s).</span></span> <span data-ttu-id="f23d1-200">A **havi számla becsült** a lap frissítések, automatikusan a havi költségeket becsült alján.</span><span class="sxs-lookup"><span data-stu-id="f23d1-200">The **Estimated Monthly Bill** at the bottom of the page updates automatically to provide an estimate of the monthly cost.</span></span> 

    >[!IMPORTANT] 
    > <span data-ttu-id="f23d1-201">Miután menti a módosításokat, és helyezze át a Standard tarifacsomag, nem állítható vissza a S1, S2 vagy S3 teljesítmény szintre.</span><span class="sxs-lookup"><span data-stu-id="f23d1-201">Once you save your changes and move to the Standard pricing tier, you cannot roll back to the S1, S2, or S3 performance levels.</span></span>

4. <span data-ttu-id="f23d1-202">Kattintson a **mentése** menti a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="f23d1-202">Click **Save** to save your changes.</span></span>

    <span data-ttu-id="f23d1-203">Ha azt állapítja meg, hogy van szüksége további átviteli sebesség (nagyobb, mint 10000 RU/mp) vagy további tárhelyet (10GB-nál nagyobb) particionált gyűjtemény hozható létre.</span><span class="sxs-lookup"><span data-stu-id="f23d1-203">If you determine that you need more throughput (greater than 10,000 RU/s) or more storage (greater than 10GB) you can create a partitioned collection.</span></span> <span data-ttu-id="f23d1-204">Az egypartíciós gyűjtemény egy particionált gyűjtemény áttelepítéséhez lásd: [egypartíciós telepít át a particionált gyűjtemények](documentdb-partition-data.md#migrating-from-single-partition).</span><span class="sxs-lookup"><span data-stu-id="f23d1-204">To migrate a single partition collection to a partitioned collection, see [Migrating from single-partition to partitioned collections](documentdb-partition-data.md#migrating-from-single-partition).</span></span>

    > [!NOTE]
    > <span data-ttu-id="f23d1-205">Standard S1, S2 vagy S3 módosítása 2 percet is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="f23d1-205">Changing from S1, S2, or S3 to Standard may take up to 2 minutes.</span></span>
    > 
    > 

<span data-ttu-id="f23d1-206">**A .NET SDK használatával az egypartíciós gyűjtemények áttelepítése**</span><span class="sxs-lookup"><span data-stu-id="f23d1-206">**To migrate to single partition collections using the .NET SDK**</span></span>

<span data-ttu-id="f23d1-207">A gyűjtemények teljesítményszintet módosítására vonatkozóan egy másik lehetőség az SDK-k keresztül történik.</span><span class="sxs-lookup"><span data-stu-id="f23d1-207">Another option for changing your collections' performance levels is through our SDKs.</span></span> <span data-ttu-id="f23d1-208">Ez a szakasz csak hozzá van rendelve egy gyűjtési teljesítmény módosítása szinten használatával a [DocumentDB .NET API](documentdb-sdk-dotnet.md), de a folyamat hasonló, ha az egyéb SDK-k.</span><span class="sxs-lookup"><span data-stu-id="f23d1-208">This section only covers changing a collection's performance level using our [DocumentDB .NET API](documentdb-sdk-dotnet.md), but the process is similar for our other SDKs.</span></span>

<span data-ttu-id="f23d1-209">Íme egy kódrészletet a a gyűjtemény átviteli sebességének módosítása a 5 000 kérelemegység / másodperc:</span><span class="sxs-lookup"><span data-stu-id="f23d1-209">Here is a code snippet for changing the collection throughput to 5,000 request units per second:</span></span>
    
```C#
    //Fetch the resource to be updated
    Offer offer = client.CreateOfferQuery()
                      .Where(r => r.ResourceLink == collection.SelfLink)    
                      .AsEnumerable()
                      .SingleOrDefault();

    // Set the throughput to 5000 request units per second
    offer = new OfferV2(offer, 5000);

    //Now persist these changes to the database by replacing the original resource
    await client.ReplaceOfferAsync(offer);
```

<span data-ttu-id="f23d1-210">Látogasson el [MSDN](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.aspx) további példákat és a további tudnivalók az ajánlat módszerek:</span><span class="sxs-lookup"><span data-stu-id="f23d1-210">Visit [MSDN](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.aspx) to view additional examples and learn more about our offer methods:</span></span>

* [<span data-ttu-id="f23d1-211">**ReadOfferAsync**</span><span class="sxs-lookup"><span data-stu-id="f23d1-211">**ReadOfferAsync**</span></span>](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.readofferasync.aspx)
* [<span data-ttu-id="f23d1-212">**ReadOffersFeedAsync**</span><span class="sxs-lookup"><span data-stu-id="f23d1-212">**ReadOffersFeedAsync**</span></span>](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.readoffersfeedasync.aspx)
* [<span data-ttu-id="f23d1-213">**ReplaceOfferAsync**</span><span class="sxs-lookup"><span data-stu-id="f23d1-213">**ReplaceOfferAsync**</span></span>](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.replaceofferasync.aspx)
* [<span data-ttu-id="f23d1-214">**CreateOfferQuery**</span><span class="sxs-lookup"><span data-stu-id="f23d1-214">**CreateOfferQuery**</span></span>](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.documentqueryable.createofferquery.aspx)

<a name="ea-customer"></a>

## <a name="how-am-i-impacted-if-im-an-ea-customer"></a><span data-ttu-id="f23d1-215">Hogyan vagyok feladatátvétele a Ha az ügyfél egy EA vagyok?</span><span class="sxs-lookup"><span data-stu-id="f23d1-215">How am I impacted if I'm an EA customer?</span></span>

<span data-ttu-id="f23d1-216">Nagyvállalati ügyfelek az aktuális szerződés végéig védett ár is.</span><span class="sxs-lookup"><span data-stu-id="f23d1-216">EA customers will be price protected until the end of their current contract.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f23d1-217">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f23d1-217">Next steps</span></span>
<span data-ttu-id="f23d1-218">Tarifa- és Azure Cosmos DB adatok kezelésével kapcsolatos további tudnivalókért ismerheti meg ezeket az erőforrásokat:</span><span class="sxs-lookup"><span data-stu-id="f23d1-218">To learn more about pricing and managing data with Azure Cosmos DB, explore these resources:</span></span>

1.  <span data-ttu-id="f23d1-219">[Particionálás adatokat az Adatbázisba az Cosmos](documentdb-partition-data.md).</span><span class="sxs-lookup"><span data-stu-id="f23d1-219">[Partitioning data in Cosmos DB](documentdb-partition-data.md).</span></span> <span data-ttu-id="f23d1-220">Az egypartíciós tároló particionált tárolók, valamint tippek az zökkenőmentesen méretezési jó particionálási stratégia megvalósítása közötti különbségek megértése.</span><span class="sxs-lookup"><span data-stu-id="f23d1-220">Understand the difference between single partition container and partitioned containers, as well as tips on implementing a partitioning strategy to scale seamlessly.</span></span>
2.  <span data-ttu-id="f23d1-221">[Cosmos DB árképzési](https://azure.microsoft.com/pricing/details/cosmos-db/).</span><span class="sxs-lookup"><span data-stu-id="f23d1-221">[Cosmos DB pricing](https://azure.microsoft.com/pricing/details/cosmos-db/).</span></span> <span data-ttu-id="f23d1-222">További információk a telepítés átviteli sebesség és a tároló felhasználása költsége.</span><span class="sxs-lookup"><span data-stu-id="f23d1-222">Learn about the cost of provisioning throughput and consuming storage.</span></span>
3.  <span data-ttu-id="f23d1-223">[Egységek kérelem](request-units.md).</span><span class="sxs-lookup"><span data-stu-id="f23d1-223">[Request units](request-units.md).</span></span> <span data-ttu-id="f23d1-224">Ismerje meg, a felhasználás átviteli különböző művelet-típusok, például a rekordhoz olvasási, írási, lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="f23d1-224">Understand the consumption of throughput for different operation types, for example Read, Write, Query.</span></span>
