---
title: "aaaDocumentDB API teljesítményszintet |} Microsoft Docs"
description: "További információk a hogyan DocumentDB API teljesítményszintek lehetővé teszik a tooreserve átviteli / tároló alapon."
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
ms.openlocfilehash: 716bc11ae238dbb0feebf004ed8d5f8a7515ec6f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="retiring-hello-s1-s2-and-s3-performance-levels"></a><span data-ttu-id="49a84-103">Hello S1, S2 és S3 teljesítményszintet kivonása</span><span class="sxs-lookup"><span data-stu-id="49a84-103">Retiring hello S1, S2, and S3 performance levels</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="49a84-104">hello S1, S2 és S3 teljesítményszintet cikkben említett használatból van, és már nem érhetők el az új DocumentDB API-fiókokat.</span><span class="sxs-lookup"><span data-stu-id="49a84-104">hello S1, S2, and S3 performance levels discussed in this article are being retired and are no longer available for new DocumentDB API accounts.</span></span>
>

<span data-ttu-id="49a84-105">Ez a cikk áttekintést S1, S2 és S3 teljesítményszintet, és ismerteti, hogyan hello gyűjteményeket, a teljesítmény szinteket használó lesz az áttelepített toosingle partíció gyűjtemények 2017 augusztus 1..</span><span class="sxs-lookup"><span data-stu-id="49a84-105">This article provides an overview of S1, S2, and S3 performance levels, and discusses how hello collections that use these performance levels will be migrated toosingle partition collections on August 1st, 2017.</span></span> <span data-ttu-id="49a84-106">A cikk elolvasása után be fog tudni tooanswer hello a következő kérdéseket:</span><span class="sxs-lookup"><span data-stu-id="49a84-106">After reading this article, you'll be able tooanswer hello following questions:</span></span>

- [<span data-ttu-id="49a84-107">Miért van hello S1, S2 és S3 teljesítmény szintek hatókörről?</span><span class="sxs-lookup"><span data-stu-id="49a84-107">Why are hello S1, S2, and S3 performance levels being retired?</span></span>](#why-retired)
- [<span data-ttu-id="49a84-108">Hogyan hajtsa végre az egypartíciós gyűjtemények és a particionált gyűjtemények összehasonlítása toohello S1, S2, S3 teljesítményszintet?</span><span class="sxs-lookup"><span data-stu-id="49a84-108">How do single partition collections and partitioned collections compare toohello S1, S2, S3 performance levels?</span></span>](#compare)
- [<span data-ttu-id="49a84-109">Mi készíthetek kell toodo tooensure folyamatos toomy adatok eléréséhez?</span><span class="sxs-lookup"><span data-stu-id="49a84-109">What do I need toodo tooensure uninterrupted access toomy data?</span></span>](#uninterrupted-access)
- [<span data-ttu-id="49a84-110">Hogyan változik a saját gyűjteményembe hello áttelepítése után?</span><span class="sxs-lookup"><span data-stu-id="49a84-110">How will my collection change after hello migration?</span></span>](#collection-change)
- [<span data-ttu-id="49a84-111">Hogyan fogja módosítani a a számlázási után áttelepített toosingle partíció gyűjtemények vagyok?</span><span class="sxs-lookup"><span data-stu-id="49a84-111">How will my billing change after I’m migrated toosingle partition collections?</span></span>](#billing-change)
- [<span data-ttu-id="49a84-112">Mi történik, ha több mint 10 GB tárhelyet kell?</span><span class="sxs-lookup"><span data-stu-id="49a84-112">What if I need more than 10 GB of storage?</span></span>](#more-storage-needed)
- [<span data-ttu-id="49a84-113">Módosítható hello S1, S2 és S3 teljesítményszintet 2017. augusztus 1. előtt között?</span><span class="sxs-lookup"><span data-stu-id="49a84-113">Can I change between hello S1, S2, and S3 performance levels before August 1, 2017?</span></span>](#change-before)
- [<span data-ttu-id="49a84-114">Milyen operációs rendszer, hogy ha a gyűjtemény át lett telepítve?</span><span class="sxs-lookup"><span data-stu-id="49a84-114">How will I know when my collection has migrated?</span></span>](#when-migrated)
- [<span data-ttu-id="49a84-115">Hogyan hello S1, S2, S3 teljesítmény szintek toosingle partíció gyűjtemények önállóan át?</span><span class="sxs-lookup"><span data-stu-id="49a84-115">How do I migrate from hello S1, S2, S3 performance levels toosingle partition collections on my own?</span></span>](#migrate-diy)
- [<span data-ttu-id="49a84-116">Hogyan vagyok feladatátvétele a Ha az ügyfél egy EA vagyok?</span><span class="sxs-lookup"><span data-stu-id="49a84-116">How am I impacted if I'm an EA customer?</span></span>](#ea-customer)

<a name="why-retired"></a>

## <a name="why-are-hello-s1-s2-and-s3-performance-levels-being-retired"></a><span data-ttu-id="49a84-117">Miért van hello S1, S2 és S3 teljesítmény szintek hatókörről?</span><span class="sxs-lookup"><span data-stu-id="49a84-117">Why are hello S1, S2, and S3 performance levels being retired?</span></span>

<span data-ttu-id="49a84-118">hello S1, S2 és S3 teljesítményszintet nem ajánlat hello rugalmasságot biztosít, amely DocumentDB API gyűjtemények nyújt, hajtsa végre.</span><span class="sxs-lookup"><span data-stu-id="49a84-118">hello S1, S2, and S3 performance levels do not offer hello flexibility that DocumentDB API collections offers.</span></span> <span data-ttu-id="49a84-119">Hello S1, S2, S3 teljesítményszintet, mindkét hello átviteli sebesség és a tárolási kapacitás előre beállított és nem ajánlja fel a rugalmasság.</span><span class="sxs-lookup"><span data-stu-id="49a84-119">With hello S1, S2, S3 performance levels, both hello throughput and storage capacity were pre-set and did not offer elasticity.</span></span> <span data-ttu-id="49a84-120">Azure Cosmos DB kínál: hello képességét toocustomize az átviteli sebesség és tárterület, felkínálva a lehetőséget tooscale sokkal nagyobb rugalmasságot igényeinek módosítása.</span><span class="sxs-lookup"><span data-stu-id="49a84-120">Azure Cosmos DB now offers hello ability toocustomize your throughput and storage, offering you much more flexibility in your ability tooscale as your needs change.</span></span>

<a name="compare"></a>

## <a name="how-do-single-partition-collections-and-partitioned-collections-compare-toohello-s1-s2-s3-performance-levels"></a><span data-ttu-id="49a84-121">Hogyan hajtsa végre az egypartíciós gyűjtemények és a particionált gyűjtemények összehasonlítása toohello S1, S2, S3 teljesítményszintet?</span><span class="sxs-lookup"><span data-stu-id="49a84-121">How do single partition collections and partitioned collections compare toohello S1, S2, S3 performance levels?</span></span>

<span data-ttu-id="49a84-122">a következő táblázat hello hello átviteli sebesség és tárterület rendelkezésre álló lehetőségek az egypartíciós gyűjtemények, a particionált gyűjtemények és S1, S2, S3 teljesítményszintet hasonlítja össze.</span><span class="sxs-lookup"><span data-stu-id="49a84-122">hello following table compares hello throughput and storage options available in single partition collections, partitioned collections, and S1, S2, S3 performance levels.</span></span> <span data-ttu-id="49a84-123">Íme egy példa a amerikai keleti régiója 2 régió:</span><span class="sxs-lookup"><span data-stu-id="49a84-123">Here is an example for US East 2 region:</span></span>

|   |<span data-ttu-id="49a84-124">Particionált gyűjtemény</span><span class="sxs-lookup"><span data-stu-id="49a84-124">Partitioned collection</span></span>|<span data-ttu-id="49a84-125">Az egypartíciós gyűjtemény</span><span class="sxs-lookup"><span data-stu-id="49a84-125">Single partition collection</span></span>|<span data-ttu-id="49a84-126">S1</span><span class="sxs-lookup"><span data-stu-id="49a84-126">S1</span></span>|<span data-ttu-id="49a84-127">S2</span><span class="sxs-lookup"><span data-stu-id="49a84-127">S2</span></span>|<span data-ttu-id="49a84-128">S3</span><span class="sxs-lookup"><span data-stu-id="49a84-128">S3</span></span>|
|---|---|---|---|---|---|
|<span data-ttu-id="49a84-129">Maximális átviteli sebesség</span><span class="sxs-lookup"><span data-stu-id="49a84-129">Maximum throughput</span></span>|<span data-ttu-id="49a84-130">Korlátlan</span><span class="sxs-lookup"><span data-stu-id="49a84-130">Unlimited</span></span>|<span data-ttu-id="49a84-131">10 KB-os RU/mp</span><span class="sxs-lookup"><span data-stu-id="49a84-131">10K RU/s</span></span>|<span data-ttu-id="49a84-132">250 RU/mp</span><span class="sxs-lookup"><span data-stu-id="49a84-132">250 RU/s</span></span>|<span data-ttu-id="49a84-133">1 K RU/mp</span><span class="sxs-lookup"><span data-stu-id="49a84-133">1 K RU/s</span></span>|<span data-ttu-id="49a84-134">2.5-K RU/mp</span><span class="sxs-lookup"><span data-stu-id="49a84-134">2.5 K RU/s</span></span>|
|<span data-ttu-id="49a84-135">Minimális átviteli sebesség</span><span class="sxs-lookup"><span data-stu-id="49a84-135">Minimum throughput</span></span>|<span data-ttu-id="49a84-136">2.5-K RU/mp</span><span class="sxs-lookup"><span data-stu-id="49a84-136">2.5K RU/s</span></span>|<span data-ttu-id="49a84-137">400 RU/mp</span><span class="sxs-lookup"><span data-stu-id="49a84-137">400 RU/s</span></span>|<span data-ttu-id="49a84-138">250 RU/mp</span><span class="sxs-lookup"><span data-stu-id="49a84-138">250 RU/s</span></span>|<span data-ttu-id="49a84-139">1 K RU/mp</span><span class="sxs-lookup"><span data-stu-id="49a84-139">1 K RU/s</span></span>|<span data-ttu-id="49a84-140">2.5-K RU/mp</span><span class="sxs-lookup"><span data-stu-id="49a84-140">2.5 K RU/s</span></span>|
|<span data-ttu-id="49a84-141">Maximális tárolóméret</span><span class="sxs-lookup"><span data-stu-id="49a84-141">Maximum storage</span></span>|<span data-ttu-id="49a84-142">Korlátlan</span><span class="sxs-lookup"><span data-stu-id="49a84-142">Unlimited</span></span>|<span data-ttu-id="49a84-143">10 GB</span><span class="sxs-lookup"><span data-stu-id="49a84-143">10 GB</span></span>|<span data-ttu-id="49a84-144">10 GB</span><span class="sxs-lookup"><span data-stu-id="49a84-144">10 GB</span></span>|<span data-ttu-id="49a84-145">10 GB</span><span class="sxs-lookup"><span data-stu-id="49a84-145">10 GB</span></span>|<span data-ttu-id="49a84-146">10 GB</span><span class="sxs-lookup"><span data-stu-id="49a84-146">10 GB</span></span>|
|<span data-ttu-id="49a84-147">Árlista (havonta)</span><span class="sxs-lookup"><span data-stu-id="49a84-147">Price (monthly)</span></span>|<span data-ttu-id="49a84-148">Átviteli sebesség: $6 / 100 RU/mp</span><span class="sxs-lookup"><span data-stu-id="49a84-148">Throughput: $6 / 100 RU/s</span></span><br><br><span data-ttu-id="49a84-149">Tárolás: $ 0,25/GB</span><span class="sxs-lookup"><span data-stu-id="49a84-149">Storage: $0.25/GB</span></span>|<span data-ttu-id="49a84-150">Átviteli sebesség: $6 / 100 RU/mp</span><span class="sxs-lookup"><span data-stu-id="49a84-150">Throughput: $6 / 100 RU/s</span></span><br><br><span data-ttu-id="49a84-151">Tárolás: $ 0,25/GB</span><span class="sxs-lookup"><span data-stu-id="49a84-151">Storage: $0.25/GB</span></span>|<span data-ttu-id="49a84-152">$25 USD</span><span class="sxs-lookup"><span data-stu-id="49a84-152">$25 USD</span></span>|<span data-ttu-id="49a84-153">$50 USD</span><span class="sxs-lookup"><span data-stu-id="49a84-153">$50 USD</span></span>|<span data-ttu-id="49a84-154">$100 USD</span><span class="sxs-lookup"><span data-stu-id="49a84-154">$100 USD</span></span>|

<span data-ttu-id="49a84-155">Az ügyfél egy EA folytatja?</span><span class="sxs-lookup"><span data-stu-id="49a84-155">Are you an EA customer?</span></span> <span data-ttu-id="49a84-156">Ha igen, tekintse meg a [hogyan vagyok I csökkentheti az EA felhasználóinál vagyok?](#ea-customer)</span><span class="sxs-lookup"><span data-stu-id="49a84-156">If so, see [How am I impacted if I'm an EA customer?](#ea-customer)</span></span>

<a name="uninterrupted-access"></a>

## <a name="what-do-i-need-toodo-tooensure-uninterrupted-access-toomy-data"></a><span data-ttu-id="49a84-157">Mi készíthetek kell toodo tooensure folyamatos toomy adatok eléréséhez?</span><span class="sxs-lookup"><span data-stu-id="49a84-157">What do I need toodo tooensure uninterrupted access toomy data?</span></span>

<span data-ttu-id="49a84-158">Semmi, Cosmos DB hello áttelepítési az Ön kezeli.</span><span class="sxs-lookup"><span data-stu-id="49a84-158">Nothing, Cosmos DB handles hello migration for you.</span></span> <span data-ttu-id="49a84-159">Ha egy S1, S2 vagy S3 gyűjteményt, a jelenlegi gyűjtemény lesz áttelepített tooa egypartíciós gyűjtemény 2017. július 31.</span><span class="sxs-lookup"><span data-stu-id="49a84-159">If you have an S1, S2, or S3 collection, your current collection will be migrated tooa single partition collection on July 31, 2017.</span></span> 

<a name="collection-change"></a>

## <a name="how-will-my-collection-change-after-hello-migration"></a><span data-ttu-id="49a84-160">Hogyan változik a saját gyűjteményembe hello áttelepítése után?</span><span class="sxs-lookup"><span data-stu-id="49a84-160">How will my collection change after hello migration?</span></span>

<span data-ttu-id="49a84-161">Ha egy S1 gyűjteményt, fogja áttelepített tooa egypartíciós gyűjtemény 400 RU/s átviteli sebességgel.</span><span class="sxs-lookup"><span data-stu-id="49a84-161">If you have an S1 collection, you will be migrated tooa single partition collection with 400 RU/s throughput.</span></span> <span data-ttu-id="49a84-162">400 RU/mp hello legalacsonyabb átviteli sebesség érhető el az egypartíciós gyűjtemények.</span><span class="sxs-lookup"><span data-stu-id="49a84-162">400 RU/s is hello lowest throughput available with single partition collections.</span></span> <span data-ttu-id="49a84-163">Az egypartíciós gyűjtemény körülbelül megegyezik az Ön volt az S1 gyűjteménnyel fizető hello hello 400 RU/mp és 250 RU/mp – így a nem fizet hello költsége azonban nagyon 150 RU/mp elérhető tooyou hello.</span><span class="sxs-lookup"><span data-stu-id="49a84-163">However, hello cost for 400 RU/s in hello a single partition collection is approximately hello same as you were paying with your S1 collection and 250 RU/s – so you are not paying for hello extra 150 RU/s available tooyou.</span></span>

<span data-ttu-id="49a84-164">Ha egy S2 gyűjteményt, fogja áttelepített tooa egypartíciós gyűjtemény 1 K RU/mp.</span><span class="sxs-lookup"><span data-stu-id="49a84-164">If you have an S2 collection, you will be migrated tooa single partition collection with 1 K RU/s.</span></span> <span data-ttu-id="49a84-165">Nincs változás tooyour átviteli szint jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="49a84-165">You will see no change tooyour throughput level.</span></span>

<span data-ttu-id="49a84-166">Ha egy S3 gyűjteményt, fogja áttelepített tooa egypartíciós gyűjtemény 2,5 K RU/mp.</span><span class="sxs-lookup"><span data-stu-id="49a84-166">If you have an S3 collection, you will be migrated tooa single partition collection with 2.5 K RU/s.</span></span> <span data-ttu-id="49a84-167">Nincs változás tooyour átviteli szint jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="49a84-167">You will see no change tooyour throughput level.</span></span>

<span data-ttu-id="49a84-168">Az egyes ezekben az esetekben miután áttelepítette a gyűjteményt, hoz kell tudni toocustomize az átviteli szintű, vagy szükséges tooprovide alacsony késésű hozzáférést tooyour felhasználóként vertikális fel-le azt.</span><span class="sxs-lookup"><span data-stu-id="49a84-168">In each of these cases, after your collection is migrated, you will be able toocustomize your throughput level, or scale it up and down as needed tooprovide low-latency access tooyour users.</span></span> <span data-ttu-id="49a84-169">toochange hello átviteli szintű után a gyűjtemény át lett telepítve, egyszerűen nyissa meg a Cosmos DB fiók hello Azure-portálon, kattintson a Scale, válassza ki a gyűjtemény és hello átviteli szint, majd beállításához, ahogy az alábbi képernyőfelvétel a hello:</span><span class="sxs-lookup"><span data-stu-id="49a84-169">toochange hello throughput level after your collection has migrated, simply open your Cosmos DB account in hello Azure portal, click Scale, choose your collection, and then adjust hello throughput level, as shown in hello following screenshot:</span></span>

![Hogyan tooscale átviteli sebesség mértéke a hello Azure-portálon](./media/performance-levels/portal-scale-throughput.png)

<a name="billing-change"></a>

## <a name="how-will-my-billing-change-after-im-migrated-toohello-single-partition-collections"></a><span data-ttu-id="49a84-171">Hogyan fogja módosítani a a számlázási után áttelepített toohello az egypartíciós gyűjtemények vagyok?</span><span class="sxs-lookup"><span data-stu-id="49a84-171">How will my billing change after I’m migrated toohello single partition collections?</span></span>

<span data-ttu-id="49a84-172">Feltéve, hogy 10 S1 gyűjtemények, 1 GB tárhelyet minden hello amerikai keleti terület, és a gyűjteményt telepít át, a 10 S1 gyűjtemények too10 egypartíciós: 400 RU/mp (hello minimális szintet).</span><span class="sxs-lookup"><span data-stu-id="49a84-172">Assuming you have 10 S1 collections, 1 GB of storage for each, in hello US East region, and you migrate these 10 S1 collections too10 single partition collections at 400 RU/sec (hello minimum level).</span></span> <span data-ttu-id="49a84-173">A számlázási a Ha a hello 10 az egypartíciós gyűjtemények esetében a teljes hónap következőképpen fog kinézni:</span><span class="sxs-lookup"><span data-stu-id="49a84-173">Your bill will look as follows if you keep hello 10 single partition collections for a full month:</span></span>

![Hogyan S1 tarifacsomag 10 gyűjtemények hasonlítja össze a too10 gyűjtemények használatával az egypartíciós gyűjtemény díjszabása](./media/performance-levels/s1-vs-standard-pricing.png)

<a name="more-storage-needed"></a>

## <a name="what-if-i-need-more-than-10-gb-of-storage"></a><span data-ttu-id="49a84-175">Mi történik, ha több mint 10 GB tárhelyet kell?</span><span class="sxs-lookup"><span data-stu-id="49a84-175">What if I need more than 10 GB of storage?</span></span>

<span data-ttu-id="49a84-176">Rendelkezik egy gyűjtemény egy S1, S2 vagy S3 teljesítményszint szükséges, vagy az egypartíciós gyűjtemény, ezek mindegyike rendelkezik, 10 GB tárterület érhető el, hogy az adatok tooa particionált gyűjtemény gyakorlatilag hello Cosmos DB adatáttelepítési eszköz toomigrate használhatja korlátlan tárterület.</span><span class="sxs-lookup"><span data-stu-id="49a84-176">Whether you have a collection with an S1, S2, or S3 performance level, or have a single partition collection, all of which have 10 GB of storage available, you can use hello Cosmos DB Data Migration tool toomigrate your data tooa partitioned collection with virtually unlimited storage.</span></span> <span data-ttu-id="49a84-177">Egy particionált gyűjtemény hello előnyöket kapcsolatos információk: [particionálás és az Azure Cosmos Adatbázisba skálázás](documentdb-partition-data.md).</span><span class="sxs-lookup"><span data-stu-id="49a84-177">For information about hello benefits of a partitioned collection, see [Partitioning and scaling in Azure Cosmos DB](documentdb-partition-data.md).</span></span> <span data-ttu-id="49a84-178">További információ toomigrate a S1, S2, S3 vagy az egypartíciós gyűjtemény tooa particionált gyűjtemény, lásd: [toopartitioned egypartíciós gyűjtemények áttelepítése](documentdb-partition-data.md#migrating-from-single-partition).</span><span class="sxs-lookup"><span data-stu-id="49a84-178">For information about how toomigrate your S1, S2, S3, or single partition collection tooa partitioned collection, see [Migrating from single-partition toopartitioned collections](documentdb-partition-data.md#migrating-from-single-partition).</span></span> 

<a name="change-before"></a>

## <a name="can-i-change-between-hello-s1-s2-and-s3-performance-levels-before-august-1-2017"></a><span data-ttu-id="49a84-179">Módosítható hello S1, S2 és S3 teljesítményszintet 2017. augusztus 1. előtt között?</span><span class="sxs-lookup"><span data-stu-id="49a84-179">Can I change between hello S1, S2, and S3 performance levels before August 1, 2017?</span></span>

<span data-ttu-id="49a84-180">S1, S2 és S3 teljesítménnyel csak meglévő fiókok képes toochange kell, és a teljesítmény szintű rétegek hello portálon vagy programozottan az alter.</span><span class="sxs-lookup"><span data-stu-id="49a84-180">Only existing accounts with S1, S2, and S3 performance will be able toochange and alter performance level tiers through hello portal or programmatically.</span></span> <span data-ttu-id="49a84-181">2017. augusztus 1. hello S1, S2, és S3 teljesítményszintet többé nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="49a84-181">By August 1, 2017, hello S1, S2, and S3 performance levels will no longer be available.</span></span> <span data-ttu-id="49a84-182">S1, S3 vagy S3 tooa egypartíciós gyűjtemény módosításakor S1, S2 vagy S3 teljesítményszintet toohello nem lehet visszatérni.</span><span class="sxs-lookup"><span data-stu-id="49a84-182">If you change from S1, S3, or S3 tooa single partition collection, you cannot return toohello S1, S2, or S3 performance levels.</span></span>

<a name="when-migrated"></a>

## <a name="how-will-i-know-when-my-collection-has-migrated"></a><span data-ttu-id="49a84-183">Milyen operációs rendszer, hogy ha a gyűjtemény át lett telepítve?</span><span class="sxs-lookup"><span data-stu-id="49a84-183">How will I know when my collection has migrated?</span></span>

<span data-ttu-id="49a84-184">hello áttelepítési 2017. július 31 fordulhat elő.</span><span class="sxs-lookup"><span data-stu-id="49a84-184">hello migration will occur on July 31, 2017.</span></span> <span data-ttu-id="49a84-185">Ha a gyűjtemény hello S1, S2 használó, vagy S3 teljesítményszintet, hello Cosmos DB team felveszi Önnel a e-mailben történik hello áttelepítés előtt.</span><span class="sxs-lookup"><span data-stu-id="49a84-185">If you have a collection that uses hello S1, S2 or S3 performance levels, hello Cosmos DB team will contact you by email before hello migration takes place.</span></span> <span data-ttu-id="49a84-186">Hello áttelepítés akkor fejeződött be, a 2017. augusztus 1., hello Azure-portálon fognak megjelenni, hogy a gyűjtemény használja, a Standard díjszabás.</span><span class="sxs-lookup"><span data-stu-id="49a84-186">Once hello migration is complete, on August 1, 2017, hello Azure portal will show that your collection uses Standard pricing.</span></span>

![Hogyan tooconfirm a gyűjtemény már áttelepített toohello a Standard tarifacsomag](./media/performance-levels/portal-standard-pricing-applied.png)

<a name="migrate-diy"></a>

## <a name="how-do-i-migrate-from-hello-s1-s2-s3-performance-levels-toosingle-partition-collections-on-my-own"></a><span data-ttu-id="49a84-188">Hogyan hello S1, S2, S3 teljesítmény szintek toosingle partíció gyűjtemények önállóan át?</span><span class="sxs-lookup"><span data-stu-id="49a84-188">How do I migrate from hello S1, S2, S3 performance levels toosingle partition collections on my own?</span></span>

<span data-ttu-id="49a84-189">Hello S1, S2, telepíthet át, és S3 teljesítmény szinteken toosingle partíció gyűjtemények használatával hello Azure-portálon vagy programozottan.</span><span class="sxs-lookup"><span data-stu-id="49a84-189">You can migrate from hello S1, S2, and S3 performance levels toosingle partition collections using hello Azure portal or programmatically.</span></span> <span data-ttu-id="49a84-190">Ehhez a saját előtt augusztus 1 toobenefit hello rugalmas átviteli közül az egypartíciós gyűjtemények érhető el, vagy a gyűjteményeket, a 2017. július 31 áthelyezni.</span><span class="sxs-lookup"><span data-stu-id="49a84-190">You can do this on your own before August 1 toobenefit from hello flexible throughput options available with single partition collections, or we will migrate your collections for you on July 31, 2017.</span></span>

<span data-ttu-id="49a84-191">**toomigrate toosingle partíció gyűjtemények hello Azure-portál használatával**</span><span class="sxs-lookup"><span data-stu-id="49a84-191">**toomigrate toosingle partition collections using hello Azure portal**</span></span>

1. <span data-ttu-id="49a84-192">A hello [ **Azure-portálon**](https://portal.azure.com), kattintson a **Azure Cosmos DB**, majd válassza ki a hello Cosmos DB fiók toomodify.</span><span class="sxs-lookup"><span data-stu-id="49a84-192">In hello [**Azure portal**](https://portal.azure.com), click **Azure Cosmos DB**, then select hello Cosmos DB account toomodify.</span></span> 
 
    <span data-ttu-id="49a84-193">Ha **Azure Cosmos DB** még nincs a hello Ugrósávon, kattintson a >, görgessen túl**adatbázisok**, jelölje be **Azure Cosmos DB**, majd válassza ki a hello DocumentDB-fiók.</span><span class="sxs-lookup"><span data-stu-id="49a84-193">If **Azure Cosmos DB** is not on hello Jumpbar, click >, scroll too**Databases**, select **Azure Cosmos DB**, and then select hello DocumentDB account.</span></span>  

2. <span data-ttu-id="49a84-194">Hello erőforrás menü alatti **tárolók**, kattintson **méretezési**, válassza ki a hello gyűjtemény toomodify hello legördülő listából, és kattintson **Tarifacsomagot**.</span><span class="sxs-lookup"><span data-stu-id="49a84-194">On hello resource menu, under **Containers**, click **Scale**, select hello collection toomodify from hello drop-down list, and then click **Pricing Tier**.</span></span> <span data-ttu-id="49a84-195">Előre definiált átviteli használatával fiókok jogosultak az S1, S2 vagy S3 tarifacsomagot.</span><span class="sxs-lookup"><span data-stu-id="49a84-195">Accounts using pre-defined throughput have a pricing tier of S1, S2, or S3.</span></span>  <span data-ttu-id="49a84-196">A hello **válasszon tarifacsomagot** panelen kattintson a **szabványos** toochange toouser által megadott átviteli sebességet, és kattintson a **válassza ki** toosave a módosítás.</span><span class="sxs-lookup"><span data-stu-id="49a84-196">In hello **Choose your pricing tier** blade, click **Standard** toochange toouser-defined throughput, and then click **Select** toosave your change.</span></span>

    ![Képernyőfelvétel a hello panelen – képernyőfelvétel, amelyen toochange hello átviteli érték](./media/performance-levels/change-performance-set-thoughput.png)

3. <span data-ttu-id="49a84-198">Vissza a hello **méretezési** panelen, hello **Tarifacsomagot** túl megváltozott**szabványos** és hello **átviteli sebesség (RU/mp)** mezőben jelenik meg, amely egy 400 alapértelmezett értéke.</span><span class="sxs-lookup"><span data-stu-id="49a84-198">Back in hello **Scale** blade, hello **Pricing Tier** is changed too**Standard** and hello **Throughput (RU/s)** box is displayed with a default value of 400.</span></span> <span data-ttu-id="49a84-199">Set hello átviteli sebesség 400 és 10 000 között [egységek kérelem](request-units.md)/second (RU/mp).</span><span class="sxs-lookup"><span data-stu-id="49a84-199">Set hello throughput between 400 and 10,000 [Request units](request-units.md)/second (RU/s).</span></span> <span data-ttu-id="49a84-200">Hello **havi számla becsült** hello hello alján lap automatikusan frissül, tooprovide havi költségeket hello becslése.</span><span class="sxs-lookup"><span data-stu-id="49a84-200">hello **Estimated Monthly Bill** at hello bottom of hello page updates automatically tooprovide an estimate of hello monthly cost.</span></span> 

    >[!IMPORTANT] 
    > <span data-ttu-id="49a84-201">Miután menti a módosításokat, és helyezze át a Standard tarifacsomag toohello, nem állítható vissza toohello S1, S2 vagy S3 teljesítményszintet.</span><span class="sxs-lookup"><span data-stu-id="49a84-201">Once you save your changes and move toohello Standard pricing tier, you cannot roll back toohello S1, S2, or S3 performance levels.</span></span>

4. <span data-ttu-id="49a84-202">Kattintson a **mentése** toosave a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="49a84-202">Click **Save** toosave your changes.</span></span>

    <span data-ttu-id="49a84-203">Ha azt állapítja meg, hogy van szüksége további átviteli sebesség (nagyobb, mint 10000 RU/mp) vagy további tárhelyet (10GB-nál nagyobb) particionált gyűjtemény hozható létre.</span><span class="sxs-lookup"><span data-stu-id="49a84-203">If you determine that you need more throughput (greater than 10,000 RU/s) or more storage (greater than 10GB) you can create a partitioned collection.</span></span> <span data-ttu-id="49a84-204">az egypartíciós gyűjtemény tooa particionált gyűjtemény toomigrate lásd: [toopartitioned egypartíciós gyűjtemények áttelepítése](documentdb-partition-data.md#migrating-from-single-partition).</span><span class="sxs-lookup"><span data-stu-id="49a84-204">toomigrate a single partition collection tooa partitioned collection, see [Migrating from single-partition toopartitioned collections](documentdb-partition-data.md#migrating-from-single-partition).</span></span>

    > [!NOTE]
    > <span data-ttu-id="49a84-205">S1, S2 vagy S3 tooStandard korlátlanról too2 percet igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="49a84-205">Changing from S1, S2, or S3 tooStandard may take up too2 minutes.</span></span>
    > 
    > 

<span data-ttu-id="49a84-206">**toomigrate toosingle partíció gyűjtemények hello .NET SDK használatával**</span><span class="sxs-lookup"><span data-stu-id="49a84-206">**toomigrate toosingle partition collections using hello .NET SDK**</span></span>

<span data-ttu-id="49a84-207">A gyűjtemények teljesítményszintet módosítására vonatkozóan egy másik lehetőség az SDK-k keresztül történik.</span><span class="sxs-lookup"><span data-stu-id="49a84-207">Another option for changing your collections' performance levels is through our SDKs.</span></span> <span data-ttu-id="49a84-208">Ez a szakasz csak hozzá van rendelve egy gyűjtési teljesítmény módosítása szinten használatával a [DocumentDB .NET API](documentdb-sdk-dotnet.md), hello folyamata hasonló, ha a Csomagjától, de.</span><span class="sxs-lookup"><span data-stu-id="49a84-208">This section only covers changing a collection's performance level using our [DocumentDB .NET API](documentdb-sdk-dotnet.md), but hello process is similar for our other SDKs.</span></span>

<span data-ttu-id="49a84-209">Íme egy kódrészletet módosítására hello gyűjtemény átviteli too5 000 kérelemegység / másodperc:</span><span class="sxs-lookup"><span data-stu-id="49a84-209">Here is a code snippet for changing hello collection throughput too5,000 request units per second:</span></span>
    
```C#
    //Fetch hello resource toobe updated
    Offer offer = client.CreateOfferQuery()
                      .Where(r => r.ResourceLink == collection.SelfLink)    
                      .AsEnumerable()
                      .SingleOrDefault();

    // Set hello throughput too5000 request units per second
    offer = new OfferV2(offer, 5000);

    //Now persist these changes toohello database by replacing hello original resource
    await client.ReplaceOfferAsync(offer);
```

<span data-ttu-id="49a84-210">Látogasson el [MSDN](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.aspx) tooview további példákat és a további tudnivalók az ajánlat módszerek:</span><span class="sxs-lookup"><span data-stu-id="49a84-210">Visit [MSDN](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.aspx) tooview additional examples and learn more about our offer methods:</span></span>

* [<span data-ttu-id="49a84-211">**ReadOfferAsync**</span><span class="sxs-lookup"><span data-stu-id="49a84-211">**ReadOfferAsync**</span></span>](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.readofferasync.aspx)
* [<span data-ttu-id="49a84-212">**ReadOffersFeedAsync**</span><span class="sxs-lookup"><span data-stu-id="49a84-212">**ReadOffersFeedAsync**</span></span>](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.readoffersfeedasync.aspx)
* [<span data-ttu-id="49a84-213">**ReplaceOfferAsync**</span><span class="sxs-lookup"><span data-stu-id="49a84-213">**ReplaceOfferAsync**</span></span>](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.replaceofferasync.aspx)
* [<span data-ttu-id="49a84-214">**CreateOfferQuery**</span><span class="sxs-lookup"><span data-stu-id="49a84-214">**CreateOfferQuery**</span></span>](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.documentqueryable.createofferquery.aspx)

<a name="ea-customer"></a>

## <a name="how-am-i-impacted-if-im-an-ea-customer"></a><span data-ttu-id="49a84-215">Hogyan vagyok feladatátvétele a Ha az ügyfél egy EA vagyok?</span><span class="sxs-lookup"><span data-stu-id="49a84-215">How am I impacted if I'm an EA customer?</span></span>

<span data-ttu-id="49a84-216">Nagyvállalati ügyfelek az aktuális szerződés hello végéig védett ár is.</span><span class="sxs-lookup"><span data-stu-id="49a84-216">EA customers will be price protected until hello end of their current contract.</span></span>

## <a name="next-steps"></a><span data-ttu-id="49a84-217">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="49a84-217">Next steps</span></span>
<span data-ttu-id="49a84-218">További információ az árak és Azure Cosmos DB, az adatok kezelése toolearn ismerheti meg ezeket az erőforrásokat:</span><span class="sxs-lookup"><span data-stu-id="49a84-218">toolearn more about pricing and managing data with Azure Cosmos DB, explore these resources:</span></span>

1.  <span data-ttu-id="49a84-219">[Particionálás adatokat az Adatbázisba az Cosmos](documentdb-partition-data.md).</span><span class="sxs-lookup"><span data-stu-id="49a84-219">[Partitioning data in Cosmos DB](documentdb-partition-data.md).</span></span> <span data-ttu-id="49a84-220">Ismerje meg az egypartíciós tároló particionált tárolók, valamint a particionálási stratégia tooscale zökkenőmentesen végrehajtási tippek hello különbsége.</span><span class="sxs-lookup"><span data-stu-id="49a84-220">Understand hello difference between single partition container and partitioned containers, as well as tips on implementing a partitioning strategy tooscale seamlessly.</span></span>
2.  <span data-ttu-id="49a84-221">[Cosmos DB árképzési](https://azure.microsoft.com/pricing/details/cosmos-db/).</span><span class="sxs-lookup"><span data-stu-id="49a84-221">[Cosmos DB pricing](https://azure.microsoft.com/pricing/details/cosmos-db/).</span></span> <span data-ttu-id="49a84-222">További információk a telepítés átviteli sebesség és a tároló felhasználása hello költség.</span><span class="sxs-lookup"><span data-stu-id="49a84-222">Learn about hello cost of provisioning throughput and consuming storage.</span></span>
3.  <span data-ttu-id="49a84-223">[Egységek kérelem](request-units.md).</span><span class="sxs-lookup"><span data-stu-id="49a84-223">[Request units](request-units.md).</span></span> <span data-ttu-id="49a84-224">Ismerje meg különböző művelet-típusok, például a rekordhoz olvasási, írási, lekérdezés átviteli hello fogyasztás.</span><span class="sxs-lookup"><span data-stu-id="49a84-224">Understand hello consumption of throughput for different operation types, for example Read, Write, Query.</span></span>
