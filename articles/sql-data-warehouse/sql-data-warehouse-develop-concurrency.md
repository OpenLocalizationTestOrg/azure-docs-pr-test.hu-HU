---
title: "aaaConcurrency és munkaterhelés-kezelés az SQL Data Warehouse |} Microsoft Docs"
description: "Ismerje meg a feldolgozási és munkaterhelés-kezelés az Azure SQL Data Warehouse adattárházzal történő, megoldások."
services: sql-data-warehouse
documentationcenter: NA
author: sqlmojo
manager: jhubbard
editor: 
ms.assetid: ef170f39-ae24-4b04-af76-53bb4c4d16d3
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: performance
ms.date: 08/23/2017
ms.author: joeyong;barbkess;kavithaj
ms.openlocfilehash: 7f7e77aa687760252aed16573b609817ed9111c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="concurrency-and-workload-management-in-sql-data-warehouse"></a><span data-ttu-id="96012-103">Párhuzamossági és munkaterhelés-kezelés az SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="96012-103">Concurrency and workload management in SQL Data Warehouse</span></span>
<span data-ttu-id="96012-104">toodeliver kiszámítható teljesítményt biztosít, a Microsoft Azure SQL Data Warehouse segít párhuzamossági szintek és erőforrás-hozzárendelések például memória és CPU rangsorolását.</span><span class="sxs-lookup"><span data-stu-id="96012-104">toodeliver predictable performance at scale, Microsoft Azure SQL Data Warehouse helps you control concurrency levels and resource allocations like memory and CPU prioritization.</span></span> <span data-ttu-id="96012-105">Ez a cikk bemutatja a feldolgozási és munkaterhelés-kezelés, elmagyarázza, hogyan mindkét szolgáltatásokat nyújt, és hogy miként szabályozható őket az adatraktár toohello elveit.</span><span class="sxs-lookup"><span data-stu-id="96012-105">This article introduces you toohello concepts of concurrency and workload management, explaining how both features have been implemented and how you can control them in your data warehouse.</span></span> <span data-ttu-id="96012-106">Az SQL Data Warehouse munkaterhelés-kezelés a tervezett toohelp Többfelhasználós környezetek támogatására.</span><span class="sxs-lookup"><span data-stu-id="96012-106">SQL Data Warehouse workload management is intended toohelp you support multi-user environments.</span></span> <span data-ttu-id="96012-107">Nem célja a több-bérlős munkaterhelésekhez.</span><span class="sxs-lookup"><span data-stu-id="96012-107">It is not intended for multi-tenant workloads.</span></span>

## <a name="concurrency-limits"></a><span data-ttu-id="96012-108">Feldolgozási korlátok</span><span class="sxs-lookup"><span data-stu-id="96012-108">Concurrency limits</span></span>
<span data-ttu-id="96012-109">Az SQL Data Warehouse lehetővé teszi, hogy too1, 024 egyidejű kapcsolatok mentése.</span><span class="sxs-lookup"><span data-stu-id="96012-109">SQL Data Warehouse allows up too1,024 concurrent connections.</span></span> <span data-ttu-id="96012-110">Az összes 1024 kapcsolat egyidejű lekérdezések küldje el.</span><span class="sxs-lookup"><span data-stu-id="96012-110">All 1,024 connections can submit queries concurrently.</span></span> <span data-ttu-id="96012-111">Toooptimize átviteli, az SQL Data Warehouse azonban előfordulhat, hogy várólista egyes lekérdezések tooensure, hogy minden egyes lekérdezés megkapja-e a minimális memória biztosítása.</span><span class="sxs-lookup"><span data-stu-id="96012-111">However, toooptimize throughput, SQL Data Warehouse may queue some queries tooensure that each query receives a minimal memory grant.</span></span> <span data-ttu-id="96012-112">A lekérdezés-végrehajtás során Queuing következik be.</span><span class="sxs-lookup"><span data-stu-id="96012-112">Queuing occurs at query execution time.</span></span> <span data-ttu-id="96012-113">Üzenetsor-kezelés lekérdezések a amikor növelheti a feldolgozási korlátok elérésekor az SQL Data Warehouse teljes átviteli sebesség biztosításával, hogy aktív lekérdezések beolvasása hozzáférés toocritically szükséges memória-erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="96012-113">By queuing queries when concurrency limits are reached, SQL Data Warehouse can increase total throughput by ensuring that active queries get access toocritically needed memory resources.</span></span>  

<span data-ttu-id="96012-114">Feldolgozási korlátok vonatkoznak két fogalom: *párhuzamos lekérdezések* és *egyidejűségi üzembe helyezési ponti*.</span><span class="sxs-lookup"><span data-stu-id="96012-114">Concurrency limits are governed by two concepts: *concurrent queries* and *concurrency slots*.</span></span> <span data-ttu-id="96012-115">Az egy lekérdezésben tooexecute, végre kell hajtani hello lekérdezés feldolgozási korlát és a hello párhuzamossági tárolóhely foglalási belül.</span><span class="sxs-lookup"><span data-stu-id="96012-115">For a query tooexecute, it must execute within both hello query concurrency limit and hello concurrency slot allocation.</span></span>

* <span data-ttu-id="96012-116">Egyidejű lekérdezések hello végrehajtása azonos hello lekérdezések idő.</span><span class="sxs-lookup"><span data-stu-id="96012-116">Concurrent queries are hello queries executing at hello same time.</span></span> <span data-ttu-id="96012-117">Az SQL Data Warehouse too32 párhuzamos lekérdezések hello nagyobb DWU mérete legfeljebb támogat.</span><span class="sxs-lookup"><span data-stu-id="96012-117">SQL Data Warehouse supports up too32 concurrent queries on hello larger DWU sizes.</span></span>
* <span data-ttu-id="96012-118">Párhuzamossági üzembe helyezési ponti DWU alapján foglal le.</span><span class="sxs-lookup"><span data-stu-id="96012-118">Concurrency slots are allocated based on DWU.</span></span> <span data-ttu-id="96012-119">Minden 100 DWU 4 párhuzamossági üzembe helyezési ponti biztosít.</span><span class="sxs-lookup"><span data-stu-id="96012-119">Each 100 DWU provides 4 concurrency slots.</span></span> <span data-ttu-id="96012-120">Például egy DW100 4 párhuzamossági üzembe helyezési ponti foglal le, és DW1000 40 foglal le.</span><span class="sxs-lookup"><span data-stu-id="96012-120">For example, a DW100 allocates 4 concurrency slots and DW1000 allocates 40.</span></span> <span data-ttu-id="96012-121">Minden egyes lekérdezés használ egy vagy több egyidejű tárhelyek, hello függő [erőforrásosztály](#resource-classes) hello lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="96012-121">Each query consumes one or more concurrency slots, dependent on hello [resource class](#resource-classes) of hello query.</span></span> <span data-ttu-id="96012-122">Hello smallrc erőforrásosztály futó lekérdezések egy feldolgozási tárolóhely felhasználását.</span><span class="sxs-lookup"><span data-stu-id="96012-122">Queries running in hello smallrc resource class consume one concurrency slot.</span></span> <span data-ttu-id="96012-123">Egy magasabb erőforrásosztály futó lekérdezések több egyidejű üzembe helyezési ponti felhasználását.</span><span class="sxs-lookup"><span data-stu-id="96012-123">Queries running in a higher resource class consume more concurrency slots.</span></span>

<span data-ttu-id="96012-124">hello következő táblázatban hello korlátozhatja a párhuzamos lekérdezések és a párhuzamosság tárhelyek hello, különböző DWU méretű.</span><span class="sxs-lookup"><span data-stu-id="96012-124">hello following table describes hello limits for both concurrent queries and concurrency slots at hello various DWU sizes.</span></span>

### <a name="concurrency-limits"></a><span data-ttu-id="96012-125">Feldolgozási korlátok</span><span class="sxs-lookup"><span data-stu-id="96012-125">Concurrency limits</span></span>
| <span data-ttu-id="96012-126">DWU</span><span class="sxs-lookup"><span data-stu-id="96012-126">DWU</span></span> | <span data-ttu-id="96012-127">Maximális párhuzamos lekérdezések</span><span class="sxs-lookup"><span data-stu-id="96012-127">Max concurrent queries</span></span> | <span data-ttu-id="96012-128">Lefoglalt párhuzamossági tárhelyek</span><span class="sxs-lookup"><span data-stu-id="96012-128">Concurrency slots allocated</span></span> |
|:--- |:---:|:---:|
| <span data-ttu-id="96012-129">DW100</span><span class="sxs-lookup"><span data-stu-id="96012-129">DW100</span></span> |<span data-ttu-id="96012-130">4</span><span class="sxs-lookup"><span data-stu-id="96012-130">4</span></span> |<span data-ttu-id="96012-131">4</span><span class="sxs-lookup"><span data-stu-id="96012-131">4</span></span> |
| <span data-ttu-id="96012-132">DW200</span><span class="sxs-lookup"><span data-stu-id="96012-132">DW200</span></span> |<span data-ttu-id="96012-133">8</span><span class="sxs-lookup"><span data-stu-id="96012-133">8</span></span> |<span data-ttu-id="96012-134">8</span><span class="sxs-lookup"><span data-stu-id="96012-134">8</span></span> |
| <span data-ttu-id="96012-135">DW300</span><span class="sxs-lookup"><span data-stu-id="96012-135">DW300</span></span> |<span data-ttu-id="96012-136">12</span><span class="sxs-lookup"><span data-stu-id="96012-136">12</span></span> |<span data-ttu-id="96012-137">12</span><span class="sxs-lookup"><span data-stu-id="96012-137">12</span></span> |
| <span data-ttu-id="96012-138">DW400</span><span class="sxs-lookup"><span data-stu-id="96012-138">DW400</span></span> |<span data-ttu-id="96012-139">16</span><span class="sxs-lookup"><span data-stu-id="96012-139">16</span></span> |<span data-ttu-id="96012-140">16</span><span class="sxs-lookup"><span data-stu-id="96012-140">16</span></span> |
| <span data-ttu-id="96012-141">DW500</span><span class="sxs-lookup"><span data-stu-id="96012-141">DW500</span></span> |<span data-ttu-id="96012-142">20</span><span class="sxs-lookup"><span data-stu-id="96012-142">20</span></span> |<span data-ttu-id="96012-143">20</span><span class="sxs-lookup"><span data-stu-id="96012-143">20</span></span> |
| <span data-ttu-id="96012-144">DW600</span><span class="sxs-lookup"><span data-stu-id="96012-144">DW600</span></span> |<span data-ttu-id="96012-145">24</span><span class="sxs-lookup"><span data-stu-id="96012-145">24</span></span> |<span data-ttu-id="96012-146">24</span><span class="sxs-lookup"><span data-stu-id="96012-146">24</span></span> |
| <span data-ttu-id="96012-147">DW1000</span><span class="sxs-lookup"><span data-stu-id="96012-147">DW1000</span></span> |<span data-ttu-id="96012-148">32</span><span class="sxs-lookup"><span data-stu-id="96012-148">32</span></span> |<span data-ttu-id="96012-149">40</span><span class="sxs-lookup"><span data-stu-id="96012-149">40</span></span> |
| <span data-ttu-id="96012-150">DW1200</span><span class="sxs-lookup"><span data-stu-id="96012-150">DW1200</span></span> |<span data-ttu-id="96012-151">32</span><span class="sxs-lookup"><span data-stu-id="96012-151">32</span></span> |<span data-ttu-id="96012-152">48</span><span class="sxs-lookup"><span data-stu-id="96012-152">48</span></span> |
| <span data-ttu-id="96012-153">DW1500</span><span class="sxs-lookup"><span data-stu-id="96012-153">DW1500</span></span> |<span data-ttu-id="96012-154">32</span><span class="sxs-lookup"><span data-stu-id="96012-154">32</span></span> |<span data-ttu-id="96012-155">60</span><span class="sxs-lookup"><span data-stu-id="96012-155">60</span></span> |
| <span data-ttu-id="96012-156">DW2000</span><span class="sxs-lookup"><span data-stu-id="96012-156">DW2000</span></span> |<span data-ttu-id="96012-157">32</span><span class="sxs-lookup"><span data-stu-id="96012-157">32</span></span> |<span data-ttu-id="96012-158">80</span><span class="sxs-lookup"><span data-stu-id="96012-158">80</span></span> |
| <span data-ttu-id="96012-159">DW3000</span><span class="sxs-lookup"><span data-stu-id="96012-159">DW3000</span></span> |<span data-ttu-id="96012-160">32</span><span class="sxs-lookup"><span data-stu-id="96012-160">32</span></span> |<span data-ttu-id="96012-161">120</span><span class="sxs-lookup"><span data-stu-id="96012-161">120</span></span> |
| <span data-ttu-id="96012-162">DW6000</span><span class="sxs-lookup"><span data-stu-id="96012-162">DW6000</span></span> |<span data-ttu-id="96012-163">32</span><span class="sxs-lookup"><span data-stu-id="96012-163">32</span></span> |<span data-ttu-id="96012-164">240</span><span class="sxs-lookup"><span data-stu-id="96012-164">240</span></span> |

<span data-ttu-id="96012-165">E küszöbértékek valamelyike teljesül, amikor új lekérdezések helyezi várólistára, és végre érkezési sorrendben történő kiküldési alapon.</span><span class="sxs-lookup"><span data-stu-id="96012-165">When one of these thresholds is met, new queries are queued and executed on a first-in, first-out basis.</span></span>  <span data-ttu-id="96012-166">A lekérdezések befejeződik, és hello a lekérdezések és üzembe helyezési ponti süllyed hello korlátok, várólistára helyezett lekérdezések kiadásakor.</span><span class="sxs-lookup"><span data-stu-id="96012-166">As a queries finishes and hello number of queries and slots falls below hello limits, queued queries are released.</span></span> 

> [!NOTE]  
> <span data-ttu-id="96012-167">*Válassza ki* lekérdezések végrehajtása kizárólag a dinamikus felügyeleti nézetek (dinamikus felügyeleti nézetek) vagy a katalógus nézetek nem szabályozza a hello feldolgozási korlátok.</span><span class="sxs-lookup"><span data-stu-id="96012-167">*Select* queries executing exclusively on dynamic management views (DMVs) or catalog views are not governed by any of hello concurrency limits.</span></span> <span data-ttu-id="96012-168">Hello rendszer függetlenül hello száma lekérdezések végrehajtása a figyelheti.</span><span class="sxs-lookup"><span data-stu-id="96012-168">You can monitor hello system regardless of hello number of queries executing on it.</span></span>
> 
> 

## <a name="resource-classes"></a><span data-ttu-id="96012-169">Erőforrás-osztályok</span><span class="sxs-lookup"><span data-stu-id="96012-169">Resource classes</span></span>
<span data-ttu-id="96012-170">Erőforrás segítségével szabályozható a memóriafoglalás és CPU-ciklusok tooa lekérdezés megadott osztályokat.</span><span class="sxs-lookup"><span data-stu-id="96012-170">Resource classes help you control memory allocation and CPU cycles given tooa query.</span></span> <span data-ttu-id="96012-171">Kétféle típusú erőforrás osztályok tooa felhasználói adatbázis-szerepkörök hello formájában rendelhet hozzá.</span><span class="sxs-lookup"><span data-stu-id="96012-171">You can assign two types of resource classes tooa user in hello form of database roles.</span></span> <span data-ttu-id="96012-172">hello kétféle típusú erőforrás-osztályok a következők:</span><span class="sxs-lookup"><span data-stu-id="96012-172">hello two types of resource classes are as follows:</span></span>
1. <span data-ttu-id="96012-173">Dinamikus erőforrás-osztályok (**smallrc, mediumrc, largerc, xlargerc**) foglaljon le a változó méretű memória, attól függően, hogy hello aktuális DWU.</span><span class="sxs-lookup"><span data-stu-id="96012-173">Dynamic Resource Classes (**smallrc, mediumrc, largerc, xlargerc**) allocate a variable amount of memory depending on hello current DWU.</span></span> <span data-ttu-id="96012-174">Ez azt jelenti, hogy amikor Ön vertikális felskálázás tooa nagyobb DWU, a lekérdezések automatikusan lekérni memóriáját.</span><span class="sxs-lookup"><span data-stu-id="96012-174">This means that when you scale up tooa larger DWU, your queries automatically get more memory.</span></span> 
2. <span data-ttu-id="96012-175">Statikus erőforrás osztályok (**staticrc10, staticrc20, staticrc30, staticrc40, staticrc50, staticrc60, staticrc70, staticrc80**) lefoglalni hello függetlenül attól, hogy ugyanazon memóriamennyiség hello aktuális DWU (feltéve, hogy maga DWU hello elegendő memóriával rendelkezik).</span><span class="sxs-lookup"><span data-stu-id="96012-175">Static Resource Classes (**staticrc10, staticrc20, staticrc30, staticrc40, staticrc50, staticrc60, staticrc70, staticrc80**) allocate hello same amount of memory regardless of hello current DWU (provided that hello DWU itself has enough memory).</span></span> <span data-ttu-id="96012-176">Ez azt jelenti, hogy nagyobb dwu-k, a lekérdezéseket is futtathat további erőforrás az osztályok egyidejűleg.</span><span class="sxs-lookup"><span data-stu-id="96012-176">This means that on larger DWUs, you can run more queries in each resource class concurrently.</span></span>

<span data-ttu-id="96012-177">A felhasználók **smallrc** és **staticrc10** kap a kisebb méretű memória, és magasabb szintű párhuzamosság előnyeit.</span><span class="sxs-lookup"><span data-stu-id="96012-177">Users in **smallrc** and **staticrc10** are given a smaller amount of memory and can take advantage of higher concurrency.</span></span> <span data-ttu-id="96012-178">Ezzel szemben, minden felhasználóhoz rendelni túl**xlargerc** vagy **staticrc80** kap nagy mennyiségű memóriát, és ezért a lekérdezések kevesebb egyidejűleg is futtathatók.</span><span class="sxs-lookup"><span data-stu-id="96012-178">In contrast, users assigned too**xlargerc** or **staticrc80** are given large amounts of memory, and therefore fewer of their queries can run concurrently.</span></span>

<span data-ttu-id="96012-179">Alapértelmezés szerint minden felhasználó tagja hello kis erőforrásosztály, **smallrc**.</span><span class="sxs-lookup"><span data-stu-id="96012-179">By default, each user is a member of hello small resource class, **smallrc**.</span></span> <span data-ttu-id="96012-180">az eljárás hello `sp_addrolemember` van használt tooincrease hello erőforrásosztály, és `sp_droprolemember` van toodecrease hello erőforrásosztály használt.</span><span class="sxs-lookup"><span data-stu-id="96012-180">hello procedure `sp_addrolemember` is used tooincrease hello resource class, and `sp_droprolemember` is used toodecrease hello resource class.</span></span> <span data-ttu-id="96012-181">Például a parancs nőne hello erőforrásosztály a loaduser túl**largerc**:</span><span class="sxs-lookup"><span data-stu-id="96012-181">For example, this command would increase hello resource class of loaduser too**largerc**:</span></span>

```sql
EXEC sp_addrolemember 'largerc', 'loaduser'
```


### <a name="queries-that-do-not-honor-resource-classes"></a><span data-ttu-id="96012-182">Lekérdezések, amelyek nem fogadják el erőforrás-osztályok</span><span class="sxs-lookup"><span data-stu-id="96012-182">Queries that do not honor resource classes</span></span>

<span data-ttu-id="96012-183">A lekérdezések, amelyek nem tudják igénybe venni egy nagyobb memóriafoglalás néhány típusa van.</span><span class="sxs-lookup"><span data-stu-id="96012-183">There are a few types of queries that do not benefit from a larger memory allocation.</span></span> <span data-ttu-id="96012-184">hello rendszer figyelmen kívül hagyja az erőforrás-osztály terhelése, és mindig futtassa ezeket a lekérdezéseket hello kis erőforrásosztály helyette.</span><span class="sxs-lookup"><span data-stu-id="96012-184">hello system ignores their resource class allocation and always run these queries in hello small resource class instead.</span></span> <span data-ttu-id="96012-185">Hello kis erőforrásosztály mindig fut, ezeket a lekérdezéseket, ha azok futtathatja, ha a feldolgozási üzembe helyezési ponti nyomás alatt, és nem használja a tárolóhelyek több, mint amennyi szükséges.</span><span class="sxs-lookup"><span data-stu-id="96012-185">If these queries always run in hello small resource class, they can run when concurrency slots are under pressure and they won't consume more slots than needed.</span></span> <span data-ttu-id="96012-186">Lásd: [erőforrás osztály kivételek](#query-exceptions-to-concurrency-limits) további információt.</span><span class="sxs-lookup"><span data-stu-id="96012-186">See [Resource class exceptions](#query-exceptions-to-concurrency-limits) for more information.</span></span>

## <a name="details-on-resource-class-assignment"></a><span data-ttu-id="96012-187">Az erőforrás osztály-hozzárendelés részletei</span><span class="sxs-lookup"><span data-stu-id="96012-187">Details on resource class assignment</span></span>


<span data-ttu-id="96012-188">Néhány további részleteket a erőforrásosztály:</span><span class="sxs-lookup"><span data-stu-id="96012-188">A few more details on resource class:</span></span>

* <span data-ttu-id="96012-189">*Az ALTER szerepkör* engedély is szükséges toochange hello erőforrásosztály egy felhasználó.</span><span class="sxs-lookup"><span data-stu-id="96012-189">*Alter role* permission is required toochange hello resource class of a user.</span></span>
* <span data-ttu-id="96012-190">Bár hozzáadhatja a felhasználói tooone vagy több hello magasabb erőforrás osztályok, akkor a dinamikus erőforrás-osztályok élveznek statikus erőforrás osztályok.</span><span class="sxs-lookup"><span data-stu-id="96012-190">Although you can add a user tooone or more of hello higher resource classes, dynamic resource classes take precedence over static resource classes.</span></span> <span data-ttu-id="96012-191">Ez azt jelenti, hogy ha a felhasználó hozzá van rendelve a tooboth **mediumrc**(dinamikus) és **staticrc80**(statikus), **mediumrc** hello erőforrás osztály, amely van figyelembe véve.</span><span class="sxs-lookup"><span data-stu-id="96012-191">That is, if a user is assigned tooboth **mediumrc**(dynamic) and **staticrc80**(static), **mediumrc** is hello resource class that is honored.</span></span>
 * <span data-ttu-id="96012-192">Amikor a felhasználó hozzá van rendelve egy erőforrás osztály egy adott osztály erőforrástípusra (egynél több dinamikus erőforrásosztály vagy több statikus erőforrásosztály) mint toomore, akkor hello legmagasabb erőforrásosztály figyelembe véve.</span><span class="sxs-lookup"><span data-stu-id="96012-192">When a user is assigned toomore than one resource class in a specific resource class type (more than one dynamic resource class or more than one static resource class), hello highest resource class is honored.</span></span> <span data-ttu-id="96012-193">Ez azt jelenti, hogy ha a felhasználó hozzá van rendelve, tooboth mediumrc és largerc, hello magasabb erőforrásosztály (largerc) van figyelembe véve.</span><span class="sxs-lookup"><span data-stu-id="96012-193">That is, if a user is assigned tooboth mediumrc and largerc, hello higher resource class (largerc) is honored.</span></span> <span data-ttu-id="96012-194">És ha a felhasználó hozzá van rendelve a tooboth **staticrc20** és **statirc80**, **staticrc80** kell figyelembe venni.</span><span class="sxs-lookup"><span data-stu-id="96012-194">And if a user is assigned tooboth **staticrc20** and **statirc80**, **staticrc80** is honored.</span></span>
* <span data-ttu-id="96012-195">hello erőforrásosztály hello rendszer rendszergazda felhasználó nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="96012-195">hello resource class of hello system administrative user cannot be changed.</span></span>

<span data-ttu-id="96012-196">Részletes példákat lásd: [módosul felhasználói erőforrás osztály példa](#changing-user-resource-class-example).</span><span class="sxs-lookup"><span data-stu-id="96012-196">For a detailed example, see [Changing user resource class example](#changing-user-resource-class-example).</span></span>

## <a name="memory-allocation"></a><span data-ttu-id="96012-197">A memóriafoglalás</span><span class="sxs-lookup"><span data-stu-id="96012-197">Memory allocation</span></span>
<span data-ttu-id="96012-198">Vannak előnyei és hátrányai tooincreasing egy felhasználó erőforrásosztály.</span><span class="sxs-lookup"><span data-stu-id="96012-198">There are pros and cons tooincreasing a user's resource class.</span></span> <span data-ttu-id="96012-199">A lekérdezések egy felhasználó egy erőforrásosztály növelését, ad hozzáférési toomore memóriába, ami azt jelentheti, lekérdezések gyorsabban hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="96012-199">Increasing a resource class for a user, gives their queries access toomore memory, which may mean queries execute faster.</span></span>  <span data-ttu-id="96012-200">Azonban a magasabb erőforrás osztályok is csökkentheti a hello száma párhuzamos lekérdezések futtatható.</span><span class="sxs-lookup"><span data-stu-id="96012-200">However, higher resource classes also reduce hello number of concurrent queries that can run.</span></span> <span data-ttu-id="96012-201">Ez a hello kompromisszum nagy mennyiségű memória tooa egyetlen lekérdezés elosztásáról, és így további lekérdezések, memória-foglalásokat, egyidejűleg toorun igénylő között.</span><span class="sxs-lookup"><span data-stu-id="96012-201">This is hello trade-off between allocating large amounts of memory tooa single query or allowing other queries, which also need memory allocations, toorun concurrently.</span></span> <span data-ttu-id="96012-202">Ha egy felhasználó kap a lekérdezés számára magas foglalásokat, a más felhasználók nem fogják tudni hozzáférés toothat azonos memória toorun lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="96012-202">If one user is given high allocations of memory for a query, other users will not have access toothat same memory toorun a query.</span></span>

<span data-ttu-id="96012-203">a következő táblázat hello hello fenntartott memória mérete tooeach terjesztési DWU- és erőforrás-osztály képezi le.</span><span class="sxs-lookup"><span data-stu-id="96012-203">hello following table maps hello memory allocated tooeach distribution by DWU and resource class.</span></span>

### <a name="memory-allocations-per-distribution-for-dynamic-resource-classes-mb"></a><span data-ttu-id="96012-204">Memória kiosztásokat eloszlása dinamikus erőforrás osztályok (MB)</span><span class="sxs-lookup"><span data-stu-id="96012-204">Memory allocations per distribution for dynamic resource classes (MB)</span></span>
| <span data-ttu-id="96012-205">DWU</span><span class="sxs-lookup"><span data-stu-id="96012-205">DWU</span></span> | <span data-ttu-id="96012-206">smallrc</span><span class="sxs-lookup"><span data-stu-id="96012-206">smallrc</span></span> | <span data-ttu-id="96012-207">mediumrc</span><span class="sxs-lookup"><span data-stu-id="96012-207">mediumrc</span></span> | <span data-ttu-id="96012-208">largerc</span><span class="sxs-lookup"><span data-stu-id="96012-208">largerc</span></span> | <span data-ttu-id="96012-209">xlargerc</span><span class="sxs-lookup"><span data-stu-id="96012-209">xlargerc</span></span> |
|:--- |:---:|:---:|:---:|:---:|
| <span data-ttu-id="96012-210">DW100</span><span class="sxs-lookup"><span data-stu-id="96012-210">DW100</span></span> |<span data-ttu-id="96012-211">100</span><span class="sxs-lookup"><span data-stu-id="96012-211">100</span></span> |<span data-ttu-id="96012-212">100</span><span class="sxs-lookup"><span data-stu-id="96012-212">100</span></span> |<span data-ttu-id="96012-213">200</span><span class="sxs-lookup"><span data-stu-id="96012-213">200</span></span> |<span data-ttu-id="96012-214">400</span><span class="sxs-lookup"><span data-stu-id="96012-214">400</span></span> |
| <span data-ttu-id="96012-215">DW200</span><span class="sxs-lookup"><span data-stu-id="96012-215">DW200</span></span> |<span data-ttu-id="96012-216">100</span><span class="sxs-lookup"><span data-stu-id="96012-216">100</span></span> |<span data-ttu-id="96012-217">200</span><span class="sxs-lookup"><span data-stu-id="96012-217">200</span></span> |<span data-ttu-id="96012-218">400</span><span class="sxs-lookup"><span data-stu-id="96012-218">400</span></span> |<span data-ttu-id="96012-219">800</span><span class="sxs-lookup"><span data-stu-id="96012-219">800</span></span> |
| <span data-ttu-id="96012-220">DW300</span><span class="sxs-lookup"><span data-stu-id="96012-220">DW300</span></span> |<span data-ttu-id="96012-221">100</span><span class="sxs-lookup"><span data-stu-id="96012-221">100</span></span> |<span data-ttu-id="96012-222">200</span><span class="sxs-lookup"><span data-stu-id="96012-222">200</span></span> |<span data-ttu-id="96012-223">400</span><span class="sxs-lookup"><span data-stu-id="96012-223">400</span></span> |<span data-ttu-id="96012-224">800</span><span class="sxs-lookup"><span data-stu-id="96012-224">800</span></span> |
| <span data-ttu-id="96012-225">DW400</span><span class="sxs-lookup"><span data-stu-id="96012-225">DW400</span></span> |<span data-ttu-id="96012-226">100</span><span class="sxs-lookup"><span data-stu-id="96012-226">100</span></span> |<span data-ttu-id="96012-227">400</span><span class="sxs-lookup"><span data-stu-id="96012-227">400</span></span> |<span data-ttu-id="96012-228">800</span><span class="sxs-lookup"><span data-stu-id="96012-228">800</span></span> |<span data-ttu-id="96012-229">1,600</span><span class="sxs-lookup"><span data-stu-id="96012-229">1,600</span></span> |
| <span data-ttu-id="96012-230">DW500</span><span class="sxs-lookup"><span data-stu-id="96012-230">DW500</span></span> |<span data-ttu-id="96012-231">100</span><span class="sxs-lookup"><span data-stu-id="96012-231">100</span></span> |<span data-ttu-id="96012-232">400</span><span class="sxs-lookup"><span data-stu-id="96012-232">400</span></span> |<span data-ttu-id="96012-233">800</span><span class="sxs-lookup"><span data-stu-id="96012-233">800</span></span> |<span data-ttu-id="96012-234">1,600</span><span class="sxs-lookup"><span data-stu-id="96012-234">1,600</span></span> |
| <span data-ttu-id="96012-235">DW600</span><span class="sxs-lookup"><span data-stu-id="96012-235">DW600</span></span> |<span data-ttu-id="96012-236">100</span><span class="sxs-lookup"><span data-stu-id="96012-236">100</span></span> |<span data-ttu-id="96012-237">400</span><span class="sxs-lookup"><span data-stu-id="96012-237">400</span></span> |<span data-ttu-id="96012-238">800</span><span class="sxs-lookup"><span data-stu-id="96012-238">800</span></span> |<span data-ttu-id="96012-239">1,600</span><span class="sxs-lookup"><span data-stu-id="96012-239">1,600</span></span> |
| <span data-ttu-id="96012-240">DW1000</span><span class="sxs-lookup"><span data-stu-id="96012-240">DW1000</span></span> |<span data-ttu-id="96012-241">100</span><span class="sxs-lookup"><span data-stu-id="96012-241">100</span></span> |<span data-ttu-id="96012-242">800</span><span class="sxs-lookup"><span data-stu-id="96012-242">800</span></span> |<span data-ttu-id="96012-243">1,600</span><span class="sxs-lookup"><span data-stu-id="96012-243">1,600</span></span> |<span data-ttu-id="96012-244">3,200</span><span class="sxs-lookup"><span data-stu-id="96012-244">3,200</span></span> |
| <span data-ttu-id="96012-245">DW1200</span><span class="sxs-lookup"><span data-stu-id="96012-245">DW1200</span></span> |<span data-ttu-id="96012-246">100</span><span class="sxs-lookup"><span data-stu-id="96012-246">100</span></span> |<span data-ttu-id="96012-247">800</span><span class="sxs-lookup"><span data-stu-id="96012-247">800</span></span> |<span data-ttu-id="96012-248">1,600</span><span class="sxs-lookup"><span data-stu-id="96012-248">1,600</span></span> |<span data-ttu-id="96012-249">3,200</span><span class="sxs-lookup"><span data-stu-id="96012-249">3,200</span></span> |
| <span data-ttu-id="96012-250">DW1500</span><span class="sxs-lookup"><span data-stu-id="96012-250">DW1500</span></span> |<span data-ttu-id="96012-251">100</span><span class="sxs-lookup"><span data-stu-id="96012-251">100</span></span> |<span data-ttu-id="96012-252">800</span><span class="sxs-lookup"><span data-stu-id="96012-252">800</span></span> |<span data-ttu-id="96012-253">1,600</span><span class="sxs-lookup"><span data-stu-id="96012-253">1,600</span></span> |<span data-ttu-id="96012-254">3,200</span><span class="sxs-lookup"><span data-stu-id="96012-254">3,200</span></span> |
| <span data-ttu-id="96012-255">DW2000</span><span class="sxs-lookup"><span data-stu-id="96012-255">DW2000</span></span> |<span data-ttu-id="96012-256">100</span><span class="sxs-lookup"><span data-stu-id="96012-256">100</span></span> |<span data-ttu-id="96012-257">1,600</span><span class="sxs-lookup"><span data-stu-id="96012-257">1,600</span></span> |<span data-ttu-id="96012-258">3,200</span><span class="sxs-lookup"><span data-stu-id="96012-258">3,200</span></span> |<span data-ttu-id="96012-259">6,400</span><span class="sxs-lookup"><span data-stu-id="96012-259">6,400</span></span> |
| <span data-ttu-id="96012-260">DW3000</span><span class="sxs-lookup"><span data-stu-id="96012-260">DW3000</span></span> |<span data-ttu-id="96012-261">100</span><span class="sxs-lookup"><span data-stu-id="96012-261">100</span></span> |<span data-ttu-id="96012-262">1,600</span><span class="sxs-lookup"><span data-stu-id="96012-262">1,600</span></span> |<span data-ttu-id="96012-263">3,200</span><span class="sxs-lookup"><span data-stu-id="96012-263">3,200</span></span> |<span data-ttu-id="96012-264">6,400</span><span class="sxs-lookup"><span data-stu-id="96012-264">6,400</span></span> |
| <span data-ttu-id="96012-265">DW6000</span><span class="sxs-lookup"><span data-stu-id="96012-265">DW6000</span></span> |<span data-ttu-id="96012-266">100</span><span class="sxs-lookup"><span data-stu-id="96012-266">100</span></span> |<span data-ttu-id="96012-267">3,200</span><span class="sxs-lookup"><span data-stu-id="96012-267">3,200</span></span> |<span data-ttu-id="96012-268">6,400</span><span class="sxs-lookup"><span data-stu-id="96012-268">6,400</span></span> |<span data-ttu-id="96012-269">12,800</span><span class="sxs-lookup"><span data-stu-id="96012-269">12,800</span></span> |

<span data-ttu-id="96012-270">a következő táblázat hello le van foglalva tooeach terjesztési DWU és statikus erőforrásosztály hello memória képezi le.</span><span class="sxs-lookup"><span data-stu-id="96012-270">hello following table maps hello memory allocated tooeach distribution by DWU and static resource class.</span></span> <span data-ttu-id="96012-271">Vegye figyelembe, hello magasabb erőforrás osztályokkal rendelkezik-e a saját memória toohonor hello globális DWU korlátok csökken.</span><span class="sxs-lookup"><span data-stu-id="96012-271">Note that hello higher resource classes have their memory reduced toohonor hello global DWU limits.</span></span>

### <a name="memory-allocations-per-distribution-for-static-resource-classes-mb"></a><span data-ttu-id="96012-272">Memória kiosztásokat eloszlása statikus erőforrás osztályok (MB)</span><span class="sxs-lookup"><span data-stu-id="96012-272">Memory allocations per distribution for static resource classes (MB)</span></span>
| <span data-ttu-id="96012-273">DWU</span><span class="sxs-lookup"><span data-stu-id="96012-273">DWU</span></span> | <span data-ttu-id="96012-274">staticrc10</span><span class="sxs-lookup"><span data-stu-id="96012-274">staticrc10</span></span> | <span data-ttu-id="96012-275">staticrc20</span><span class="sxs-lookup"><span data-stu-id="96012-275">staticrc20</span></span> | <span data-ttu-id="96012-276">staticrc30</span><span class="sxs-lookup"><span data-stu-id="96012-276">staticrc30</span></span> | <span data-ttu-id="96012-277">staticrc40</span><span class="sxs-lookup"><span data-stu-id="96012-277">staticrc40</span></span> | <span data-ttu-id="96012-278">staticrc50</span><span class="sxs-lookup"><span data-stu-id="96012-278">staticrc50</span></span> | <span data-ttu-id="96012-279">staticrc60</span><span class="sxs-lookup"><span data-stu-id="96012-279">staticrc60</span></span> | <span data-ttu-id="96012-280">staticrc70</span><span class="sxs-lookup"><span data-stu-id="96012-280">staticrc70</span></span> | <span data-ttu-id="96012-281">staticrc80</span><span class="sxs-lookup"><span data-stu-id="96012-281">staticrc80</span></span> |
|:--- |:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| <span data-ttu-id="96012-282">DW100</span><span class="sxs-lookup"><span data-stu-id="96012-282">DW100</span></span> |<span data-ttu-id="96012-283">100</span><span class="sxs-lookup"><span data-stu-id="96012-283">100</span></span> |<span data-ttu-id="96012-284">200</span><span class="sxs-lookup"><span data-stu-id="96012-284">200</span></span> |<span data-ttu-id="96012-285">400</span><span class="sxs-lookup"><span data-stu-id="96012-285">400</span></span> |<span data-ttu-id="96012-286">400</span><span class="sxs-lookup"><span data-stu-id="96012-286">400</span></span> |<span data-ttu-id="96012-287">400</span><span class="sxs-lookup"><span data-stu-id="96012-287">400</span></span> |<span data-ttu-id="96012-288">400</span><span class="sxs-lookup"><span data-stu-id="96012-288">400</span></span> |<span data-ttu-id="96012-289">400</span><span class="sxs-lookup"><span data-stu-id="96012-289">400</span></span> |<span data-ttu-id="96012-290">400</span><span class="sxs-lookup"><span data-stu-id="96012-290">400</span></span> |
| <span data-ttu-id="96012-291">DW200</span><span class="sxs-lookup"><span data-stu-id="96012-291">DW200</span></span> |<span data-ttu-id="96012-292">100</span><span class="sxs-lookup"><span data-stu-id="96012-292">100</span></span> |<span data-ttu-id="96012-293">200</span><span class="sxs-lookup"><span data-stu-id="96012-293">200</span></span> |<span data-ttu-id="96012-294">400</span><span class="sxs-lookup"><span data-stu-id="96012-294">400</span></span> |<span data-ttu-id="96012-295">800</span><span class="sxs-lookup"><span data-stu-id="96012-295">800</span></span> |<span data-ttu-id="96012-296">800</span><span class="sxs-lookup"><span data-stu-id="96012-296">800</span></span> |<span data-ttu-id="96012-297">800</span><span class="sxs-lookup"><span data-stu-id="96012-297">800</span></span> |<span data-ttu-id="96012-298">800</span><span class="sxs-lookup"><span data-stu-id="96012-298">800</span></span> |<span data-ttu-id="96012-299">800</span><span class="sxs-lookup"><span data-stu-id="96012-299">800</span></span> |
| <span data-ttu-id="96012-300">DW300</span><span class="sxs-lookup"><span data-stu-id="96012-300">DW300</span></span> |<span data-ttu-id="96012-301">100</span><span class="sxs-lookup"><span data-stu-id="96012-301">100</span></span> |<span data-ttu-id="96012-302">200</span><span class="sxs-lookup"><span data-stu-id="96012-302">200</span></span> |<span data-ttu-id="96012-303">400</span><span class="sxs-lookup"><span data-stu-id="96012-303">400</span></span> |<span data-ttu-id="96012-304">800</span><span class="sxs-lookup"><span data-stu-id="96012-304">800</span></span> |<span data-ttu-id="96012-305">800</span><span class="sxs-lookup"><span data-stu-id="96012-305">800</span></span> |<span data-ttu-id="96012-306">800</span><span class="sxs-lookup"><span data-stu-id="96012-306">800</span></span> |<span data-ttu-id="96012-307">800</span><span class="sxs-lookup"><span data-stu-id="96012-307">800</span></span> |<span data-ttu-id="96012-308">800</span><span class="sxs-lookup"><span data-stu-id="96012-308">800</span></span> |
| <span data-ttu-id="96012-309">DW400</span><span class="sxs-lookup"><span data-stu-id="96012-309">DW400</span></span> |<span data-ttu-id="96012-310">100</span><span class="sxs-lookup"><span data-stu-id="96012-310">100</span></span> |<span data-ttu-id="96012-311">200</span><span class="sxs-lookup"><span data-stu-id="96012-311">200</span></span> |<span data-ttu-id="96012-312">400</span><span class="sxs-lookup"><span data-stu-id="96012-312">400</span></span> |<span data-ttu-id="96012-313">800</span><span class="sxs-lookup"><span data-stu-id="96012-313">800</span></span> |<span data-ttu-id="96012-314">1,600</span><span class="sxs-lookup"><span data-stu-id="96012-314">1,600</span></span> |<span data-ttu-id="96012-315">1,600</span><span class="sxs-lookup"><span data-stu-id="96012-315">1,600</span></span> |<span data-ttu-id="96012-316">1,600</span><span class="sxs-lookup"><span data-stu-id="96012-316">1,600</span></span> |<span data-ttu-id="96012-317">1,600</span><span class="sxs-lookup"><span data-stu-id="96012-317">1,600</span></span> |
| <span data-ttu-id="96012-318">DW500</span><span class="sxs-lookup"><span data-stu-id="96012-318">DW500</span></span> |<span data-ttu-id="96012-319">100</span><span class="sxs-lookup"><span data-stu-id="96012-319">100</span></span> |<span data-ttu-id="96012-320">200</span><span class="sxs-lookup"><span data-stu-id="96012-320">200</span></span> |<span data-ttu-id="96012-321">400</span><span class="sxs-lookup"><span data-stu-id="96012-321">400</span></span> |<span data-ttu-id="96012-322">800</span><span class="sxs-lookup"><span data-stu-id="96012-322">800</span></span> |<span data-ttu-id="96012-323">1,600</span><span class="sxs-lookup"><span data-stu-id="96012-323">1,600</span></span> |<span data-ttu-id="96012-324">1,600</span><span class="sxs-lookup"><span data-stu-id="96012-324">1,600</span></span> |<span data-ttu-id="96012-325">1,600</span><span class="sxs-lookup"><span data-stu-id="96012-325">1,600</span></span> |<span data-ttu-id="96012-326">1,600</span><span class="sxs-lookup"><span data-stu-id="96012-326">1,600</span></span> |
| <span data-ttu-id="96012-327">DW600</span><span class="sxs-lookup"><span data-stu-id="96012-327">DW600</span></span> |<span data-ttu-id="96012-328">100</span><span class="sxs-lookup"><span data-stu-id="96012-328">100</span></span> |<span data-ttu-id="96012-329">200</span><span class="sxs-lookup"><span data-stu-id="96012-329">200</span></span> |<span data-ttu-id="96012-330">400</span><span class="sxs-lookup"><span data-stu-id="96012-330">400</span></span> |<span data-ttu-id="96012-331">800</span><span class="sxs-lookup"><span data-stu-id="96012-331">800</span></span> |<span data-ttu-id="96012-332">1,600</span><span class="sxs-lookup"><span data-stu-id="96012-332">1,600</span></span> |<span data-ttu-id="96012-333">1,600</span><span class="sxs-lookup"><span data-stu-id="96012-333">1,600</span></span> |<span data-ttu-id="96012-334">1,600</span><span class="sxs-lookup"><span data-stu-id="96012-334">1,600</span></span> |<span data-ttu-id="96012-335">1,600</span><span class="sxs-lookup"><span data-stu-id="96012-335">1,600</span></span> |
| <span data-ttu-id="96012-336">DW1000</span><span class="sxs-lookup"><span data-stu-id="96012-336">DW1000</span></span> |<span data-ttu-id="96012-337">100</span><span class="sxs-lookup"><span data-stu-id="96012-337">100</span></span> |<span data-ttu-id="96012-338">200</span><span class="sxs-lookup"><span data-stu-id="96012-338">200</span></span> |<span data-ttu-id="96012-339">400</span><span class="sxs-lookup"><span data-stu-id="96012-339">400</span></span> |<span data-ttu-id="96012-340">800</span><span class="sxs-lookup"><span data-stu-id="96012-340">800</span></span> |<span data-ttu-id="96012-341">1,600</span><span class="sxs-lookup"><span data-stu-id="96012-341">1,600</span></span> |<span data-ttu-id="96012-342">3,200</span><span class="sxs-lookup"><span data-stu-id="96012-342">3,200</span></span> |<span data-ttu-id="96012-343">3,200</span><span class="sxs-lookup"><span data-stu-id="96012-343">3,200</span></span> |<span data-ttu-id="96012-344">3,200</span><span class="sxs-lookup"><span data-stu-id="96012-344">3,200</span></span> |
| <span data-ttu-id="96012-345">DW1200</span><span class="sxs-lookup"><span data-stu-id="96012-345">DW1200</span></span> |<span data-ttu-id="96012-346">100</span><span class="sxs-lookup"><span data-stu-id="96012-346">100</span></span> |<span data-ttu-id="96012-347">200</span><span class="sxs-lookup"><span data-stu-id="96012-347">200</span></span> |<span data-ttu-id="96012-348">400</span><span class="sxs-lookup"><span data-stu-id="96012-348">400</span></span> |<span data-ttu-id="96012-349">800</span><span class="sxs-lookup"><span data-stu-id="96012-349">800</span></span> |<span data-ttu-id="96012-350">1,600</span><span class="sxs-lookup"><span data-stu-id="96012-350">1,600</span></span> |<span data-ttu-id="96012-351">3,200</span><span class="sxs-lookup"><span data-stu-id="96012-351">3,200</span></span> |<span data-ttu-id="96012-352">3,200</span><span class="sxs-lookup"><span data-stu-id="96012-352">3,200</span></span> |<span data-ttu-id="96012-353">3,200</span><span class="sxs-lookup"><span data-stu-id="96012-353">3,200</span></span> |
| <span data-ttu-id="96012-354">DW1500</span><span class="sxs-lookup"><span data-stu-id="96012-354">DW1500</span></span> |<span data-ttu-id="96012-355">100</span><span class="sxs-lookup"><span data-stu-id="96012-355">100</span></span> |<span data-ttu-id="96012-356">200</span><span class="sxs-lookup"><span data-stu-id="96012-356">200</span></span> |<span data-ttu-id="96012-357">400</span><span class="sxs-lookup"><span data-stu-id="96012-357">400</span></span> |<span data-ttu-id="96012-358">800</span><span class="sxs-lookup"><span data-stu-id="96012-358">800</span></span> |<span data-ttu-id="96012-359">1,600</span><span class="sxs-lookup"><span data-stu-id="96012-359">1,600</span></span> |<span data-ttu-id="96012-360">3,200</span><span class="sxs-lookup"><span data-stu-id="96012-360">3,200</span></span> |<span data-ttu-id="96012-361">3,200</span><span class="sxs-lookup"><span data-stu-id="96012-361">3,200</span></span> |<span data-ttu-id="96012-362">3,200</span><span class="sxs-lookup"><span data-stu-id="96012-362">3,200</span></span> |
| <span data-ttu-id="96012-363">DW2000</span><span class="sxs-lookup"><span data-stu-id="96012-363">DW2000</span></span> |<span data-ttu-id="96012-364">100</span><span class="sxs-lookup"><span data-stu-id="96012-364">100</span></span> |<span data-ttu-id="96012-365">200</span><span class="sxs-lookup"><span data-stu-id="96012-365">200</span></span> |<span data-ttu-id="96012-366">400</span><span class="sxs-lookup"><span data-stu-id="96012-366">400</span></span> |<span data-ttu-id="96012-367">800</span><span class="sxs-lookup"><span data-stu-id="96012-367">800</span></span> |<span data-ttu-id="96012-368">1,600</span><span class="sxs-lookup"><span data-stu-id="96012-368">1,600</span></span> |<span data-ttu-id="96012-369">3,200</span><span class="sxs-lookup"><span data-stu-id="96012-369">3,200</span></span> |<span data-ttu-id="96012-370">6,400</span><span class="sxs-lookup"><span data-stu-id="96012-370">6,400</span></span> |<span data-ttu-id="96012-371">6,400</span><span class="sxs-lookup"><span data-stu-id="96012-371">6,400</span></span> |
| <span data-ttu-id="96012-372">DW3000</span><span class="sxs-lookup"><span data-stu-id="96012-372">DW3000</span></span> |<span data-ttu-id="96012-373">100</span><span class="sxs-lookup"><span data-stu-id="96012-373">100</span></span> |<span data-ttu-id="96012-374">200</span><span class="sxs-lookup"><span data-stu-id="96012-374">200</span></span> |<span data-ttu-id="96012-375">400</span><span class="sxs-lookup"><span data-stu-id="96012-375">400</span></span> |<span data-ttu-id="96012-376">800</span><span class="sxs-lookup"><span data-stu-id="96012-376">800</span></span> |<span data-ttu-id="96012-377">1,600</span><span class="sxs-lookup"><span data-stu-id="96012-377">1,600</span></span> |<span data-ttu-id="96012-378">3,200</span><span class="sxs-lookup"><span data-stu-id="96012-378">3,200</span></span> |<span data-ttu-id="96012-379">6,400</span><span class="sxs-lookup"><span data-stu-id="96012-379">6,400</span></span> |<span data-ttu-id="96012-380">6,400</span><span class="sxs-lookup"><span data-stu-id="96012-380">6,400</span></span> |
| <span data-ttu-id="96012-381">DW6000</span><span class="sxs-lookup"><span data-stu-id="96012-381">DW6000</span></span> |<span data-ttu-id="96012-382">100</span><span class="sxs-lookup"><span data-stu-id="96012-382">100</span></span> |<span data-ttu-id="96012-383">200</span><span class="sxs-lookup"><span data-stu-id="96012-383">200</span></span> |<span data-ttu-id="96012-384">400</span><span class="sxs-lookup"><span data-stu-id="96012-384">400</span></span> |<span data-ttu-id="96012-385">800</span><span class="sxs-lookup"><span data-stu-id="96012-385">800</span></span> |<span data-ttu-id="96012-386">1,600</span><span class="sxs-lookup"><span data-stu-id="96012-386">1,600</span></span> |<span data-ttu-id="96012-387">3,200</span><span class="sxs-lookup"><span data-stu-id="96012-387">3,200</span></span> |<span data-ttu-id="96012-388">6,400</span><span class="sxs-lookup"><span data-stu-id="96012-388">6,400</span></span> |<span data-ttu-id="96012-389">12,800</span><span class="sxs-lookup"><span data-stu-id="96012-389">12,800</span></span> |

<span data-ttu-id="96012-390">A tábla megelőző hello, láthatja, hogy a lekérdezés egy DW2000 hello a futó **xlargerc** erőforrásosztály kellene hozzáférés too6, 400 MB memória belül hello 60 elosztott adatbázisok mindegyike esetében.</span><span class="sxs-lookup"><span data-stu-id="96012-390">From hello preceding table, you can see that a query running on a DW2000 in hello **xlargerc** resource class would have access too6,400 MB of memory within each of hello 60 distributed databases.</span></span>  <span data-ttu-id="96012-391">Az SQL Data Warehouse nincsenek 60 terjesztéseket.</span><span class="sxs-lookup"><span data-stu-id="96012-391">In SQL Data Warehouse, there are 60 distributions.</span></span> <span data-ttu-id="96012-392">Ezért toocalculate hello teljes memória lefoglalása egy lekérdezést a megadott erőforrásosztály, hello meghaladja értékeket meg kell szorozni 60.</span><span class="sxs-lookup"><span data-stu-id="96012-392">Therefore, toocalculate hello total memory allocation for a query in a given resource class, hello above values should be multiplied by 60.</span></span>

### <a name="memory-allocations-system-wide-gb"></a><span data-ttu-id="96012-393">Memória kiosztásokat rendszerszintű (GB)</span><span class="sxs-lookup"><span data-stu-id="96012-393">Memory allocations system-wide (GB)</span></span>
| <span data-ttu-id="96012-394">DWU</span><span class="sxs-lookup"><span data-stu-id="96012-394">DWU</span></span> | <span data-ttu-id="96012-395">smallrc</span><span class="sxs-lookup"><span data-stu-id="96012-395">smallrc</span></span> | <span data-ttu-id="96012-396">mediumrc</span><span class="sxs-lookup"><span data-stu-id="96012-396">mediumrc</span></span> | <span data-ttu-id="96012-397">largerc</span><span class="sxs-lookup"><span data-stu-id="96012-397">largerc</span></span> | <span data-ttu-id="96012-398">xlargerc</span><span class="sxs-lookup"><span data-stu-id="96012-398">xlargerc</span></span> |
|:--- |:---:|:---:|:---:|:---:|
| <span data-ttu-id="96012-399">DW100</span><span class="sxs-lookup"><span data-stu-id="96012-399">DW100</span></span> |<span data-ttu-id="96012-400">6</span><span class="sxs-lookup"><span data-stu-id="96012-400">6</span></span> |<span data-ttu-id="96012-401">6</span><span class="sxs-lookup"><span data-stu-id="96012-401">6</span></span> |<span data-ttu-id="96012-402">12</span><span class="sxs-lookup"><span data-stu-id="96012-402">12</span></span> |<span data-ttu-id="96012-403">23</span><span class="sxs-lookup"><span data-stu-id="96012-403">23</span></span> |
| <span data-ttu-id="96012-404">DW200</span><span class="sxs-lookup"><span data-stu-id="96012-404">DW200</span></span> |<span data-ttu-id="96012-405">6</span><span class="sxs-lookup"><span data-stu-id="96012-405">6</span></span> |<span data-ttu-id="96012-406">12</span><span class="sxs-lookup"><span data-stu-id="96012-406">12</span></span> |<span data-ttu-id="96012-407">23</span><span class="sxs-lookup"><span data-stu-id="96012-407">23</span></span> |<span data-ttu-id="96012-408">47</span><span class="sxs-lookup"><span data-stu-id="96012-408">47</span></span> |
| <span data-ttu-id="96012-409">DW300</span><span class="sxs-lookup"><span data-stu-id="96012-409">DW300</span></span> |<span data-ttu-id="96012-410">6</span><span class="sxs-lookup"><span data-stu-id="96012-410">6</span></span> |<span data-ttu-id="96012-411">12</span><span class="sxs-lookup"><span data-stu-id="96012-411">12</span></span> |<span data-ttu-id="96012-412">23</span><span class="sxs-lookup"><span data-stu-id="96012-412">23</span></span> |<span data-ttu-id="96012-413">47</span><span class="sxs-lookup"><span data-stu-id="96012-413">47</span></span> |
| <span data-ttu-id="96012-414">DW400</span><span class="sxs-lookup"><span data-stu-id="96012-414">DW400</span></span> |<span data-ttu-id="96012-415">6</span><span class="sxs-lookup"><span data-stu-id="96012-415">6</span></span> |<span data-ttu-id="96012-416">23</span><span class="sxs-lookup"><span data-stu-id="96012-416">23</span></span> |<span data-ttu-id="96012-417">47</span><span class="sxs-lookup"><span data-stu-id="96012-417">47</span></span> |<span data-ttu-id="96012-418">94</span><span class="sxs-lookup"><span data-stu-id="96012-418">94</span></span> |
| <span data-ttu-id="96012-419">DW500</span><span class="sxs-lookup"><span data-stu-id="96012-419">DW500</span></span> |<span data-ttu-id="96012-420">6</span><span class="sxs-lookup"><span data-stu-id="96012-420">6</span></span> |<span data-ttu-id="96012-421">23</span><span class="sxs-lookup"><span data-stu-id="96012-421">23</span></span> |<span data-ttu-id="96012-422">47</span><span class="sxs-lookup"><span data-stu-id="96012-422">47</span></span> |<span data-ttu-id="96012-423">94</span><span class="sxs-lookup"><span data-stu-id="96012-423">94</span></span> |
| <span data-ttu-id="96012-424">DW600</span><span class="sxs-lookup"><span data-stu-id="96012-424">DW600</span></span> |<span data-ttu-id="96012-425">6</span><span class="sxs-lookup"><span data-stu-id="96012-425">6</span></span> |<span data-ttu-id="96012-426">23</span><span class="sxs-lookup"><span data-stu-id="96012-426">23</span></span> |<span data-ttu-id="96012-427">47</span><span class="sxs-lookup"><span data-stu-id="96012-427">47</span></span> |<span data-ttu-id="96012-428">94</span><span class="sxs-lookup"><span data-stu-id="96012-428">94</span></span> |
| <span data-ttu-id="96012-429">DW1000</span><span class="sxs-lookup"><span data-stu-id="96012-429">DW1000</span></span> |<span data-ttu-id="96012-430">6</span><span class="sxs-lookup"><span data-stu-id="96012-430">6</span></span> |<span data-ttu-id="96012-431">47</span><span class="sxs-lookup"><span data-stu-id="96012-431">47</span></span> |<span data-ttu-id="96012-432">94</span><span class="sxs-lookup"><span data-stu-id="96012-432">94</span></span> |<span data-ttu-id="96012-433">188</span><span class="sxs-lookup"><span data-stu-id="96012-433">188</span></span> |
| <span data-ttu-id="96012-434">DW1200</span><span class="sxs-lookup"><span data-stu-id="96012-434">DW1200</span></span> |<span data-ttu-id="96012-435">6</span><span class="sxs-lookup"><span data-stu-id="96012-435">6</span></span> |<span data-ttu-id="96012-436">47</span><span class="sxs-lookup"><span data-stu-id="96012-436">47</span></span> |<span data-ttu-id="96012-437">94</span><span class="sxs-lookup"><span data-stu-id="96012-437">94</span></span> |<span data-ttu-id="96012-438">188</span><span class="sxs-lookup"><span data-stu-id="96012-438">188</span></span> |
| <span data-ttu-id="96012-439">DW1500</span><span class="sxs-lookup"><span data-stu-id="96012-439">DW1500</span></span> |<span data-ttu-id="96012-440">6</span><span class="sxs-lookup"><span data-stu-id="96012-440">6</span></span> |<span data-ttu-id="96012-441">47</span><span class="sxs-lookup"><span data-stu-id="96012-441">47</span></span> |<span data-ttu-id="96012-442">94</span><span class="sxs-lookup"><span data-stu-id="96012-442">94</span></span> |<span data-ttu-id="96012-443">188</span><span class="sxs-lookup"><span data-stu-id="96012-443">188</span></span> |
| <span data-ttu-id="96012-444">DW2000</span><span class="sxs-lookup"><span data-stu-id="96012-444">DW2000</span></span> |<span data-ttu-id="96012-445">6</span><span class="sxs-lookup"><span data-stu-id="96012-445">6</span></span> |<span data-ttu-id="96012-446">94</span><span class="sxs-lookup"><span data-stu-id="96012-446">94</span></span> |<span data-ttu-id="96012-447">188</span><span class="sxs-lookup"><span data-stu-id="96012-447">188</span></span> |<span data-ttu-id="96012-448">375</span><span class="sxs-lookup"><span data-stu-id="96012-448">375</span></span> |
| <span data-ttu-id="96012-449">DW3000</span><span class="sxs-lookup"><span data-stu-id="96012-449">DW3000</span></span> |<span data-ttu-id="96012-450">6</span><span class="sxs-lookup"><span data-stu-id="96012-450">6</span></span> |<span data-ttu-id="96012-451">94</span><span class="sxs-lookup"><span data-stu-id="96012-451">94</span></span> |<span data-ttu-id="96012-452">188</span><span class="sxs-lookup"><span data-stu-id="96012-452">188</span></span> |<span data-ttu-id="96012-453">375</span><span class="sxs-lookup"><span data-stu-id="96012-453">375</span></span> |
| <span data-ttu-id="96012-454">DW6000</span><span class="sxs-lookup"><span data-stu-id="96012-454">DW6000</span></span> |<span data-ttu-id="96012-455">6</span><span class="sxs-lookup"><span data-stu-id="96012-455">6</span></span> |<span data-ttu-id="96012-456">188</span><span class="sxs-lookup"><span data-stu-id="96012-456">188</span></span> |<span data-ttu-id="96012-457">375</span><span class="sxs-lookup"><span data-stu-id="96012-457">375</span></span> |<span data-ttu-id="96012-458">750</span><span class="sxs-lookup"><span data-stu-id="96012-458">750</span></span> |

<span data-ttu-id="96012-459">Ebből a táblázatból tartozó rendszerszintű memória-hozzárendelések, láthatja, hogy a lekérdezés egy DW2000 hello xlargerc erőforrás osztályban futó lefoglalt 375 GB memória összesen (6400 MB * 60 terjesztéseket / 1024 tooconvert tooGB) az SQL Data Warehouse hello a teljes keresztül.</span><span class="sxs-lookup"><span data-stu-id="96012-459">From this table of system-wide memory allocations, you can see that a query running on a DW2000 in hello xlargerc resource class is allocated a total of 375 GB of memory (6,400 MB * 60 distributions / 1,024 tooconvert tooGB) over hello entirety of your SQL Data Warehouse.</span></span>

<span data-ttu-id="96012-460">hello azonos számítási érvényes toostatic erőforrás osztályok.</span><span class="sxs-lookup"><span data-stu-id="96012-460">hello same calculation applies toostatic resource classes.</span></span>
 
## <a name="concurrency-slot-consumption"></a><span data-ttu-id="96012-461">Párhuzamossági tárolóhely-használat</span><span class="sxs-lookup"><span data-stu-id="96012-461">Concurrency slot consumption</span></span>  
<span data-ttu-id="96012-462">Az SQL Data Warehouse magasabb erőforrás osztályok futtató további memória tooqueries biztosít.</span><span class="sxs-lookup"><span data-stu-id="96012-462">SQL Data Warehouse grants more memory tooqueries running in higher resource classes.</span></span> <span data-ttu-id="96012-463">Memória mérete rögzített erőforrás.</span><span class="sxs-lookup"><span data-stu-id="96012-463">Memory is a fixed resource.</span></span>  <span data-ttu-id="96012-464">Ezért hello lekérdezésenként kevesebb egyidejű lekérdezések végrehajtható hello lefoglalt memória.</span><span class="sxs-lookup"><span data-stu-id="96012-464">Therefore, hello more memory allocated per query, hello fewer concurrent queries can execute.</span></span> <span data-ttu-id="96012-465">hello következő tábla ismételten kifejezi összes hello előző fogalmak DWU által elérhető párhuzamossági bővítőhelyek és minden erőforrásosztály által felhasznált hello tárhelyek hello számát jeleníti meg egyetlen nézetben.</span><span class="sxs-lookup"><span data-stu-id="96012-465">hello following table reiterates all of hello previous concepts in a single view that shows hello number of concurrency slots available by DWU and hello slots consumed by each resource class.</span></span>  

### <a name="allocation-and-consumption-of-concurrency-slots-for-dynamic-resource-classes"></a><span data-ttu-id="96012-466">Foglalási és felhasználási párhuzamossági tárolóhelyek dinamikus erőforrás-osztályok</span><span class="sxs-lookup"><span data-stu-id="96012-466">Allocation and consumption of concurrency slots for dynamic resource classes</span></span>  
| <span data-ttu-id="96012-467">DWU</span><span class="sxs-lookup"><span data-stu-id="96012-467">DWU</span></span> | <span data-ttu-id="96012-468">Maximális párhuzamos lekérdezések</span><span class="sxs-lookup"><span data-stu-id="96012-468">Maximum concurrent queries</span></span> | <span data-ttu-id="96012-469">Lefoglalt párhuzamossági tárhelyek</span><span class="sxs-lookup"><span data-stu-id="96012-469">Concurrency slots allocated</span></span> | <span data-ttu-id="96012-470">Üzembe helyezési ponti smallrc által használt</span><span class="sxs-lookup"><span data-stu-id="96012-470">Slots used by smallrc</span></span> | <span data-ttu-id="96012-471">Üzembe helyezési ponti mediumrc által használt</span><span class="sxs-lookup"><span data-stu-id="96012-471">Slots used by mediumrc</span></span> | <span data-ttu-id="96012-472">Üzembe helyezési ponti largerc által használt</span><span class="sxs-lookup"><span data-stu-id="96012-472">Slots used by largerc</span></span> | <span data-ttu-id="96012-473">Üzembe helyezési ponti xlargerc által használt</span><span class="sxs-lookup"><span data-stu-id="96012-473">Slots used by xlargerc</span></span> |
|:--- |:---:|:---:|:---:|:---:|:---:|:---:|
| <span data-ttu-id="96012-474">DW100</span><span class="sxs-lookup"><span data-stu-id="96012-474">DW100</span></span> |<span data-ttu-id="96012-475">4</span><span class="sxs-lookup"><span data-stu-id="96012-475">4</span></span> |<span data-ttu-id="96012-476">4</span><span class="sxs-lookup"><span data-stu-id="96012-476">4</span></span> |<span data-ttu-id="96012-477">1</span><span class="sxs-lookup"><span data-stu-id="96012-477">1</span></span> |<span data-ttu-id="96012-478">1</span><span class="sxs-lookup"><span data-stu-id="96012-478">1</span></span> |<span data-ttu-id="96012-479">2</span><span class="sxs-lookup"><span data-stu-id="96012-479">2</span></span> |<span data-ttu-id="96012-480">4</span><span class="sxs-lookup"><span data-stu-id="96012-480">4</span></span> |
| <span data-ttu-id="96012-481">DW200</span><span class="sxs-lookup"><span data-stu-id="96012-481">DW200</span></span> |<span data-ttu-id="96012-482">8</span><span class="sxs-lookup"><span data-stu-id="96012-482">8</span></span> |<span data-ttu-id="96012-483">8</span><span class="sxs-lookup"><span data-stu-id="96012-483">8</span></span> |<span data-ttu-id="96012-484">1</span><span class="sxs-lookup"><span data-stu-id="96012-484">1</span></span> |<span data-ttu-id="96012-485">2</span><span class="sxs-lookup"><span data-stu-id="96012-485">2</span></span> |<span data-ttu-id="96012-486">4</span><span class="sxs-lookup"><span data-stu-id="96012-486">4</span></span> |<span data-ttu-id="96012-487">8</span><span class="sxs-lookup"><span data-stu-id="96012-487">8</span></span> |
| <span data-ttu-id="96012-488">DW300</span><span class="sxs-lookup"><span data-stu-id="96012-488">DW300</span></span> |<span data-ttu-id="96012-489">12</span><span class="sxs-lookup"><span data-stu-id="96012-489">12</span></span> |<span data-ttu-id="96012-490">12</span><span class="sxs-lookup"><span data-stu-id="96012-490">12</span></span> |<span data-ttu-id="96012-491">1</span><span class="sxs-lookup"><span data-stu-id="96012-491">1</span></span> |<span data-ttu-id="96012-492">2</span><span class="sxs-lookup"><span data-stu-id="96012-492">2</span></span> |<span data-ttu-id="96012-493">4</span><span class="sxs-lookup"><span data-stu-id="96012-493">4</span></span> |<span data-ttu-id="96012-494">8</span><span class="sxs-lookup"><span data-stu-id="96012-494">8</span></span> |
| <span data-ttu-id="96012-495">DW400</span><span class="sxs-lookup"><span data-stu-id="96012-495">DW400</span></span> |<span data-ttu-id="96012-496">16</span><span class="sxs-lookup"><span data-stu-id="96012-496">16</span></span> |<span data-ttu-id="96012-497">16</span><span class="sxs-lookup"><span data-stu-id="96012-497">16</span></span> |<span data-ttu-id="96012-498">1</span><span class="sxs-lookup"><span data-stu-id="96012-498">1</span></span> |<span data-ttu-id="96012-499">4</span><span class="sxs-lookup"><span data-stu-id="96012-499">4</span></span> |<span data-ttu-id="96012-500">8</span><span class="sxs-lookup"><span data-stu-id="96012-500">8</span></span> |<span data-ttu-id="96012-501">16</span><span class="sxs-lookup"><span data-stu-id="96012-501">16</span></span> |
| <span data-ttu-id="96012-502">DW500</span><span class="sxs-lookup"><span data-stu-id="96012-502">DW500</span></span> |<span data-ttu-id="96012-503">20</span><span class="sxs-lookup"><span data-stu-id="96012-503">20</span></span> |<span data-ttu-id="96012-504">20</span><span class="sxs-lookup"><span data-stu-id="96012-504">20</span></span> |<span data-ttu-id="96012-505">1</span><span class="sxs-lookup"><span data-stu-id="96012-505">1</span></span> |<span data-ttu-id="96012-506">4</span><span class="sxs-lookup"><span data-stu-id="96012-506">4</span></span> |<span data-ttu-id="96012-507">8</span><span class="sxs-lookup"><span data-stu-id="96012-507">8</span></span> |<span data-ttu-id="96012-508">16</span><span class="sxs-lookup"><span data-stu-id="96012-508">16</span></span> |
| <span data-ttu-id="96012-509">DW600</span><span class="sxs-lookup"><span data-stu-id="96012-509">DW600</span></span> |<span data-ttu-id="96012-510">24</span><span class="sxs-lookup"><span data-stu-id="96012-510">24</span></span> |<span data-ttu-id="96012-511">24</span><span class="sxs-lookup"><span data-stu-id="96012-511">24</span></span> |<span data-ttu-id="96012-512">1</span><span class="sxs-lookup"><span data-stu-id="96012-512">1</span></span> |<span data-ttu-id="96012-513">4</span><span class="sxs-lookup"><span data-stu-id="96012-513">4</span></span> |<span data-ttu-id="96012-514">8</span><span class="sxs-lookup"><span data-stu-id="96012-514">8</span></span> |<span data-ttu-id="96012-515">16</span><span class="sxs-lookup"><span data-stu-id="96012-515">16</span></span> |
| <span data-ttu-id="96012-516">DW1000</span><span class="sxs-lookup"><span data-stu-id="96012-516">DW1000</span></span> |<span data-ttu-id="96012-517">32</span><span class="sxs-lookup"><span data-stu-id="96012-517">32</span></span> |<span data-ttu-id="96012-518">40</span><span class="sxs-lookup"><span data-stu-id="96012-518">40</span></span> |<span data-ttu-id="96012-519">1</span><span class="sxs-lookup"><span data-stu-id="96012-519">1</span></span> |<span data-ttu-id="96012-520">8</span><span class="sxs-lookup"><span data-stu-id="96012-520">8</span></span> |<span data-ttu-id="96012-521">16</span><span class="sxs-lookup"><span data-stu-id="96012-521">16</span></span> |<span data-ttu-id="96012-522">32</span><span class="sxs-lookup"><span data-stu-id="96012-522">32</span></span> |
| <span data-ttu-id="96012-523">DW1200</span><span class="sxs-lookup"><span data-stu-id="96012-523">DW1200</span></span> |<span data-ttu-id="96012-524">32</span><span class="sxs-lookup"><span data-stu-id="96012-524">32</span></span> |<span data-ttu-id="96012-525">48</span><span class="sxs-lookup"><span data-stu-id="96012-525">48</span></span> |<span data-ttu-id="96012-526">1</span><span class="sxs-lookup"><span data-stu-id="96012-526">1</span></span> |<span data-ttu-id="96012-527">8</span><span class="sxs-lookup"><span data-stu-id="96012-527">8</span></span> |<span data-ttu-id="96012-528">16</span><span class="sxs-lookup"><span data-stu-id="96012-528">16</span></span> |<span data-ttu-id="96012-529">32</span><span class="sxs-lookup"><span data-stu-id="96012-529">32</span></span> |
| <span data-ttu-id="96012-530">DW1500</span><span class="sxs-lookup"><span data-stu-id="96012-530">DW1500</span></span> |<span data-ttu-id="96012-531">32</span><span class="sxs-lookup"><span data-stu-id="96012-531">32</span></span> |<span data-ttu-id="96012-532">60</span><span class="sxs-lookup"><span data-stu-id="96012-532">60</span></span> |<span data-ttu-id="96012-533">1</span><span class="sxs-lookup"><span data-stu-id="96012-533">1</span></span> |<span data-ttu-id="96012-534">8</span><span class="sxs-lookup"><span data-stu-id="96012-534">8</span></span> |<span data-ttu-id="96012-535">16</span><span class="sxs-lookup"><span data-stu-id="96012-535">16</span></span> |<span data-ttu-id="96012-536">32</span><span class="sxs-lookup"><span data-stu-id="96012-536">32</span></span> |
| <span data-ttu-id="96012-537">DW2000</span><span class="sxs-lookup"><span data-stu-id="96012-537">DW2000</span></span> |<span data-ttu-id="96012-538">32</span><span class="sxs-lookup"><span data-stu-id="96012-538">32</span></span> |<span data-ttu-id="96012-539">80</span><span class="sxs-lookup"><span data-stu-id="96012-539">80</span></span> |<span data-ttu-id="96012-540">1</span><span class="sxs-lookup"><span data-stu-id="96012-540">1</span></span> |<span data-ttu-id="96012-541">16</span><span class="sxs-lookup"><span data-stu-id="96012-541">16</span></span> |<span data-ttu-id="96012-542">32</span><span class="sxs-lookup"><span data-stu-id="96012-542">32</span></span> |<span data-ttu-id="96012-543">64</span><span class="sxs-lookup"><span data-stu-id="96012-543">64</span></span> |
| <span data-ttu-id="96012-544">DW3000</span><span class="sxs-lookup"><span data-stu-id="96012-544">DW3000</span></span> |<span data-ttu-id="96012-545">32</span><span class="sxs-lookup"><span data-stu-id="96012-545">32</span></span> |<span data-ttu-id="96012-546">120</span><span class="sxs-lookup"><span data-stu-id="96012-546">120</span></span> |<span data-ttu-id="96012-547">1</span><span class="sxs-lookup"><span data-stu-id="96012-547">1</span></span> |<span data-ttu-id="96012-548">16</span><span class="sxs-lookup"><span data-stu-id="96012-548">16</span></span> |<span data-ttu-id="96012-549">32</span><span class="sxs-lookup"><span data-stu-id="96012-549">32</span></span> |<span data-ttu-id="96012-550">64</span><span class="sxs-lookup"><span data-stu-id="96012-550">64</span></span> |
| <span data-ttu-id="96012-551">DW6000</span><span class="sxs-lookup"><span data-stu-id="96012-551">DW6000</span></span> |<span data-ttu-id="96012-552">32</span><span class="sxs-lookup"><span data-stu-id="96012-552">32</span></span> |<span data-ttu-id="96012-553">240</span><span class="sxs-lookup"><span data-stu-id="96012-553">240</span></span> |<span data-ttu-id="96012-554">1</span><span class="sxs-lookup"><span data-stu-id="96012-554">1</span></span> |<span data-ttu-id="96012-555">32</span><span class="sxs-lookup"><span data-stu-id="96012-555">32</span></span> |<span data-ttu-id="96012-556">64</span><span class="sxs-lookup"><span data-stu-id="96012-556">64</span></span> |<span data-ttu-id="96012-557">128</span><span class="sxs-lookup"><span data-stu-id="96012-557">128</span></span> |

### <a name="allocation-and-consumption-of-concurrency-slots-for-static-resource-classes"></a><span data-ttu-id="96012-558">Foglalási és felhasználási párhuzamossági tárolóhelyek statikus erőforrás osztályok</span><span class="sxs-lookup"><span data-stu-id="96012-558">Allocation and consumption of concurrency slots for static resource classes</span></span>  
| <span data-ttu-id="96012-559">DWU</span><span class="sxs-lookup"><span data-stu-id="96012-559">DWU</span></span> | <span data-ttu-id="96012-560">Maximális párhuzamos lekérdezések</span><span class="sxs-lookup"><span data-stu-id="96012-560">Maximum concurrent queries</span></span> | <span data-ttu-id="96012-561">Lefoglalt párhuzamossági tárhelyek</span><span class="sxs-lookup"><span data-stu-id="96012-561">Concurrency slots allocated</span></span> |<span data-ttu-id="96012-562">staticrc10</span><span class="sxs-lookup"><span data-stu-id="96012-562">staticrc10</span></span> | <span data-ttu-id="96012-563">staticrc20</span><span class="sxs-lookup"><span data-stu-id="96012-563">staticrc20</span></span> | <span data-ttu-id="96012-564">staticrc30</span><span class="sxs-lookup"><span data-stu-id="96012-564">staticrc30</span></span> | <span data-ttu-id="96012-565">staticrc40</span><span class="sxs-lookup"><span data-stu-id="96012-565">staticrc40</span></span> | <span data-ttu-id="96012-566">staticrc50</span><span class="sxs-lookup"><span data-stu-id="96012-566">staticrc50</span></span> | <span data-ttu-id="96012-567">staticrc60</span><span class="sxs-lookup"><span data-stu-id="96012-567">staticrc60</span></span> | <span data-ttu-id="96012-568">staticrc70</span><span class="sxs-lookup"><span data-stu-id="96012-568">staticrc70</span></span> | <span data-ttu-id="96012-569">staticrc80</span><span class="sxs-lookup"><span data-stu-id="96012-569">staticrc80</span></span> |
|:--- |:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| <span data-ttu-id="96012-570">DW100</span><span class="sxs-lookup"><span data-stu-id="96012-570">DW100</span></span> |<span data-ttu-id="96012-571">4</span><span class="sxs-lookup"><span data-stu-id="96012-571">4</span></span> |<span data-ttu-id="96012-572">4</span><span class="sxs-lookup"><span data-stu-id="96012-572">4</span></span> |<span data-ttu-id="96012-573">1</span><span class="sxs-lookup"><span data-stu-id="96012-573">1</span></span> |<span data-ttu-id="96012-574">2</span><span class="sxs-lookup"><span data-stu-id="96012-574">2</span></span> |<span data-ttu-id="96012-575">4</span><span class="sxs-lookup"><span data-stu-id="96012-575">4</span></span> |<span data-ttu-id="96012-576">4</span><span class="sxs-lookup"><span data-stu-id="96012-576">4</span></span> |<span data-ttu-id="96012-577">4</span><span class="sxs-lookup"><span data-stu-id="96012-577">4</span></span> |<span data-ttu-id="96012-578">4</span><span class="sxs-lookup"><span data-stu-id="96012-578">4</span></span> |<span data-ttu-id="96012-579">4</span><span class="sxs-lookup"><span data-stu-id="96012-579">4</span></span> |<span data-ttu-id="96012-580">4</span><span class="sxs-lookup"><span data-stu-id="96012-580">4</span></span> |
| <span data-ttu-id="96012-581">DW200</span><span class="sxs-lookup"><span data-stu-id="96012-581">DW200</span></span> |<span data-ttu-id="96012-582">8</span><span class="sxs-lookup"><span data-stu-id="96012-582">8</span></span> |<span data-ttu-id="96012-583">8</span><span class="sxs-lookup"><span data-stu-id="96012-583">8</span></span> |<span data-ttu-id="96012-584">1</span><span class="sxs-lookup"><span data-stu-id="96012-584">1</span></span> |<span data-ttu-id="96012-585">2</span><span class="sxs-lookup"><span data-stu-id="96012-585">2</span></span> |<span data-ttu-id="96012-586">4</span><span class="sxs-lookup"><span data-stu-id="96012-586">4</span></span> |<span data-ttu-id="96012-587">8</span><span class="sxs-lookup"><span data-stu-id="96012-587">8</span></span> |<span data-ttu-id="96012-588">8</span><span class="sxs-lookup"><span data-stu-id="96012-588">8</span></span> |<span data-ttu-id="96012-589">8</span><span class="sxs-lookup"><span data-stu-id="96012-589">8</span></span> |<span data-ttu-id="96012-590">8</span><span class="sxs-lookup"><span data-stu-id="96012-590">8</span></span> |<span data-ttu-id="96012-591">8</span><span class="sxs-lookup"><span data-stu-id="96012-591">8</span></span> |
| <span data-ttu-id="96012-592">DW300</span><span class="sxs-lookup"><span data-stu-id="96012-592">DW300</span></span> |<span data-ttu-id="96012-593">12</span><span class="sxs-lookup"><span data-stu-id="96012-593">12</span></span> |<span data-ttu-id="96012-594">12</span><span class="sxs-lookup"><span data-stu-id="96012-594">12</span></span> |<span data-ttu-id="96012-595">1</span><span class="sxs-lookup"><span data-stu-id="96012-595">1</span></span> |<span data-ttu-id="96012-596">2</span><span class="sxs-lookup"><span data-stu-id="96012-596">2</span></span> |<span data-ttu-id="96012-597">4</span><span class="sxs-lookup"><span data-stu-id="96012-597">4</span></span> |<span data-ttu-id="96012-598">8</span><span class="sxs-lookup"><span data-stu-id="96012-598">8</span></span> |<span data-ttu-id="96012-599">8</span><span class="sxs-lookup"><span data-stu-id="96012-599">8</span></span> |<span data-ttu-id="96012-600">8</span><span class="sxs-lookup"><span data-stu-id="96012-600">8</span></span> |<span data-ttu-id="96012-601">8</span><span class="sxs-lookup"><span data-stu-id="96012-601">8</span></span> |<span data-ttu-id="96012-602">8</span><span class="sxs-lookup"><span data-stu-id="96012-602">8</span></span> |
| <span data-ttu-id="96012-603">DW400</span><span class="sxs-lookup"><span data-stu-id="96012-603">DW400</span></span> |<span data-ttu-id="96012-604">16</span><span class="sxs-lookup"><span data-stu-id="96012-604">16</span></span> |<span data-ttu-id="96012-605">16</span><span class="sxs-lookup"><span data-stu-id="96012-605">16</span></span> |<span data-ttu-id="96012-606">1</span><span class="sxs-lookup"><span data-stu-id="96012-606">1</span></span> |<span data-ttu-id="96012-607">2</span><span class="sxs-lookup"><span data-stu-id="96012-607">2</span></span> |<span data-ttu-id="96012-608">4</span><span class="sxs-lookup"><span data-stu-id="96012-608">4</span></span> |<span data-ttu-id="96012-609">8</span><span class="sxs-lookup"><span data-stu-id="96012-609">8</span></span> |<span data-ttu-id="96012-610">16</span><span class="sxs-lookup"><span data-stu-id="96012-610">16</span></span> |<span data-ttu-id="96012-611">16</span><span class="sxs-lookup"><span data-stu-id="96012-611">16</span></span> |<span data-ttu-id="96012-612">16</span><span class="sxs-lookup"><span data-stu-id="96012-612">16</span></span> |<span data-ttu-id="96012-613">16</span><span class="sxs-lookup"><span data-stu-id="96012-613">16</span></span> |
| <span data-ttu-id="96012-614">DW500</span><span class="sxs-lookup"><span data-stu-id="96012-614">DW500</span></span> | <span data-ttu-id="96012-615">20</span><span class="sxs-lookup"><span data-stu-id="96012-615">20</span></span>| <span data-ttu-id="96012-616">20</span><span class="sxs-lookup"><span data-stu-id="96012-616">20</span></span>| <span data-ttu-id="96012-617">1</span><span class="sxs-lookup"><span data-stu-id="96012-617">1</span></span>| <span data-ttu-id="96012-618">2</span><span class="sxs-lookup"><span data-stu-id="96012-618">2</span></span>| <span data-ttu-id="96012-619">4</span><span class="sxs-lookup"><span data-stu-id="96012-619">4</span></span>| <span data-ttu-id="96012-620">8</span><span class="sxs-lookup"><span data-stu-id="96012-620">8</span></span>| <span data-ttu-id="96012-621">16</span><span class="sxs-lookup"><span data-stu-id="96012-621">16</span></span>| <span data-ttu-id="96012-622">16</span><span class="sxs-lookup"><span data-stu-id="96012-622">16</span></span>| <span data-ttu-id="96012-623">16</span><span class="sxs-lookup"><span data-stu-id="96012-623">16</span></span>| <span data-ttu-id="96012-624">16</span><span class="sxs-lookup"><span data-stu-id="96012-624">16</span></span>|
| <span data-ttu-id="96012-625">DW600</span><span class="sxs-lookup"><span data-stu-id="96012-625">DW600</span></span> | <span data-ttu-id="96012-626">24</span><span class="sxs-lookup"><span data-stu-id="96012-626">24</span></span>| <span data-ttu-id="96012-627">24</span><span class="sxs-lookup"><span data-stu-id="96012-627">24</span></span>| <span data-ttu-id="96012-628">1</span><span class="sxs-lookup"><span data-stu-id="96012-628">1</span></span>| <span data-ttu-id="96012-629">2</span><span class="sxs-lookup"><span data-stu-id="96012-629">2</span></span>| <span data-ttu-id="96012-630">4</span><span class="sxs-lookup"><span data-stu-id="96012-630">4</span></span>| <span data-ttu-id="96012-631">8</span><span class="sxs-lookup"><span data-stu-id="96012-631">8</span></span>| <span data-ttu-id="96012-632">16</span><span class="sxs-lookup"><span data-stu-id="96012-632">16</span></span>| <span data-ttu-id="96012-633">16</span><span class="sxs-lookup"><span data-stu-id="96012-633">16</span></span>| <span data-ttu-id="96012-634">16</span><span class="sxs-lookup"><span data-stu-id="96012-634">16</span></span>| <span data-ttu-id="96012-635">16</span><span class="sxs-lookup"><span data-stu-id="96012-635">16</span></span>|
| <span data-ttu-id="96012-636">DW1000</span><span class="sxs-lookup"><span data-stu-id="96012-636">DW1000</span></span> | <span data-ttu-id="96012-637">32</span><span class="sxs-lookup"><span data-stu-id="96012-637">32</span></span>| <span data-ttu-id="96012-638">40</span><span class="sxs-lookup"><span data-stu-id="96012-638">40</span></span>| <span data-ttu-id="96012-639">1</span><span class="sxs-lookup"><span data-stu-id="96012-639">1</span></span>| <span data-ttu-id="96012-640">2</span><span class="sxs-lookup"><span data-stu-id="96012-640">2</span></span>| <span data-ttu-id="96012-641">4</span><span class="sxs-lookup"><span data-stu-id="96012-641">4</span></span>| <span data-ttu-id="96012-642">8</span><span class="sxs-lookup"><span data-stu-id="96012-642">8</span></span>| <span data-ttu-id="96012-643">16</span><span class="sxs-lookup"><span data-stu-id="96012-643">16</span></span>| <span data-ttu-id="96012-644">32</span><span class="sxs-lookup"><span data-stu-id="96012-644">32</span></span>| <span data-ttu-id="96012-645">32</span><span class="sxs-lookup"><span data-stu-id="96012-645">32</span></span>| <span data-ttu-id="96012-646">32</span><span class="sxs-lookup"><span data-stu-id="96012-646">32</span></span>|
| <span data-ttu-id="96012-647">DW1200</span><span class="sxs-lookup"><span data-stu-id="96012-647">DW1200</span></span> | <span data-ttu-id="96012-648">32</span><span class="sxs-lookup"><span data-stu-id="96012-648">32</span></span>| <span data-ttu-id="96012-649">48</span><span class="sxs-lookup"><span data-stu-id="96012-649">48</span></span>| <span data-ttu-id="96012-650">1</span><span class="sxs-lookup"><span data-stu-id="96012-650">1</span></span>| <span data-ttu-id="96012-651">2</span><span class="sxs-lookup"><span data-stu-id="96012-651">2</span></span>| <span data-ttu-id="96012-652">4</span><span class="sxs-lookup"><span data-stu-id="96012-652">4</span></span>| <span data-ttu-id="96012-653">8</span><span class="sxs-lookup"><span data-stu-id="96012-653">8</span></span>| <span data-ttu-id="96012-654">16</span><span class="sxs-lookup"><span data-stu-id="96012-654">16</span></span>| <span data-ttu-id="96012-655">32</span><span class="sxs-lookup"><span data-stu-id="96012-655">32</span></span>| <span data-ttu-id="96012-656">32</span><span class="sxs-lookup"><span data-stu-id="96012-656">32</span></span>| <span data-ttu-id="96012-657">32</span><span class="sxs-lookup"><span data-stu-id="96012-657">32</span></span>|
| <span data-ttu-id="96012-658">DW1500</span><span class="sxs-lookup"><span data-stu-id="96012-658">DW1500</span></span> | <span data-ttu-id="96012-659">32</span><span class="sxs-lookup"><span data-stu-id="96012-659">32</span></span>| <span data-ttu-id="96012-660">60</span><span class="sxs-lookup"><span data-stu-id="96012-660">60</span></span>| <span data-ttu-id="96012-661">1</span><span class="sxs-lookup"><span data-stu-id="96012-661">1</span></span>| <span data-ttu-id="96012-662">2</span><span class="sxs-lookup"><span data-stu-id="96012-662">2</span></span>| <span data-ttu-id="96012-663">4</span><span class="sxs-lookup"><span data-stu-id="96012-663">4</span></span>| <span data-ttu-id="96012-664">8</span><span class="sxs-lookup"><span data-stu-id="96012-664">8</span></span>| <span data-ttu-id="96012-665">16</span><span class="sxs-lookup"><span data-stu-id="96012-665">16</span></span>| <span data-ttu-id="96012-666">32</span><span class="sxs-lookup"><span data-stu-id="96012-666">32</span></span>| <span data-ttu-id="96012-667">32</span><span class="sxs-lookup"><span data-stu-id="96012-667">32</span></span>| <span data-ttu-id="96012-668">32</span><span class="sxs-lookup"><span data-stu-id="96012-668">32</span></span>|
| <span data-ttu-id="96012-669">DW2000</span><span class="sxs-lookup"><span data-stu-id="96012-669">DW2000</span></span> | <span data-ttu-id="96012-670">32</span><span class="sxs-lookup"><span data-stu-id="96012-670">32</span></span>| <span data-ttu-id="96012-671">80</span><span class="sxs-lookup"><span data-stu-id="96012-671">80</span></span>| <span data-ttu-id="96012-672">1</span><span class="sxs-lookup"><span data-stu-id="96012-672">1</span></span>| <span data-ttu-id="96012-673">2</span><span class="sxs-lookup"><span data-stu-id="96012-673">2</span></span>| <span data-ttu-id="96012-674">4</span><span class="sxs-lookup"><span data-stu-id="96012-674">4</span></span>| <span data-ttu-id="96012-675">8</span><span class="sxs-lookup"><span data-stu-id="96012-675">8</span></span>| <span data-ttu-id="96012-676">16</span><span class="sxs-lookup"><span data-stu-id="96012-676">16</span></span>| <span data-ttu-id="96012-677">32</span><span class="sxs-lookup"><span data-stu-id="96012-677">32</span></span>| <span data-ttu-id="96012-678">64</span><span class="sxs-lookup"><span data-stu-id="96012-678">64</span></span>| <span data-ttu-id="96012-679">64</span><span class="sxs-lookup"><span data-stu-id="96012-679">64</span></span>|
| <span data-ttu-id="96012-680">DW3000</span><span class="sxs-lookup"><span data-stu-id="96012-680">DW3000</span></span> | <span data-ttu-id="96012-681">32</span><span class="sxs-lookup"><span data-stu-id="96012-681">32</span></span>| <span data-ttu-id="96012-682">120</span><span class="sxs-lookup"><span data-stu-id="96012-682">120</span></span>| <span data-ttu-id="96012-683">1</span><span class="sxs-lookup"><span data-stu-id="96012-683">1</span></span>| <span data-ttu-id="96012-684">2</span><span class="sxs-lookup"><span data-stu-id="96012-684">2</span></span>| <span data-ttu-id="96012-685">4</span><span class="sxs-lookup"><span data-stu-id="96012-685">4</span></span>| <span data-ttu-id="96012-686">8</span><span class="sxs-lookup"><span data-stu-id="96012-686">8</span></span>| <span data-ttu-id="96012-687">16</span><span class="sxs-lookup"><span data-stu-id="96012-687">16</span></span>| <span data-ttu-id="96012-688">32</span><span class="sxs-lookup"><span data-stu-id="96012-688">32</span></span>| <span data-ttu-id="96012-689">64</span><span class="sxs-lookup"><span data-stu-id="96012-689">64</span></span>| <span data-ttu-id="96012-690">64</span><span class="sxs-lookup"><span data-stu-id="96012-690">64</span></span>|
| <span data-ttu-id="96012-691">DW6000</span><span class="sxs-lookup"><span data-stu-id="96012-691">DW6000</span></span> | <span data-ttu-id="96012-692">32</span><span class="sxs-lookup"><span data-stu-id="96012-692">32</span></span>| <span data-ttu-id="96012-693">240</span><span class="sxs-lookup"><span data-stu-id="96012-693">240</span></span>| <span data-ttu-id="96012-694">1</span><span class="sxs-lookup"><span data-stu-id="96012-694">1</span></span>| <span data-ttu-id="96012-695">2</span><span class="sxs-lookup"><span data-stu-id="96012-695">2</span></span>| <span data-ttu-id="96012-696">4</span><span class="sxs-lookup"><span data-stu-id="96012-696">4</span></span>| <span data-ttu-id="96012-697">8</span><span class="sxs-lookup"><span data-stu-id="96012-697">8</span></span>| <span data-ttu-id="96012-698">16</span><span class="sxs-lookup"><span data-stu-id="96012-698">16</span></span>| <span data-ttu-id="96012-699">32</span><span class="sxs-lookup"><span data-stu-id="96012-699">32</span></span>| <span data-ttu-id="96012-700">64</span><span class="sxs-lookup"><span data-stu-id="96012-700">64</span></span>| <span data-ttu-id="96012-701">128</span><span class="sxs-lookup"><span data-stu-id="96012-701">128</span></span>|

<span data-ttu-id="96012-702">Ezek a táblázatok a láthatja, hogy az SQL Data Warehouse fut, DW1000 foglal le, mely legfeljebb 32 egyidejű lekérdezéseket és 40 párhuzamossági tárolóhelyek összesen.</span><span class="sxs-lookup"><span data-stu-id="96012-702">From these tables, you can see that SQL Data Warehouse running as DW1000 allocates a maximum of 32 concurrent queries and a total of 40 concurrency slots.</span></span> <span data-ttu-id="96012-703">Minden felhasználó smallrc fut, ha 32 egyidejű lekérdezések volna kell engedélyezett, mert minden egyes lekérdezés 1 párhuzamossági tárolóhely volna felhasználását.</span><span class="sxs-lookup"><span data-stu-id="96012-703">If all users are running in smallrc, 32 concurrent queries would be allowed because each query would consume 1 concurrency slot.</span></span> <span data-ttu-id="96012-704">Ha minden felhasználó egy DW1000 mediumrc a futtatást, minden egyes lekérdezés szeretné kiosztani 800 MB eloszlása lekérdezésenként 47 GB memória összesen kiosztható és feldolgozási lenne korlátozott too5 felhasználók (40 párhuzamossági üzembe helyezési ponti / 8 tárolóhely mediumrc felhasználónként).</span><span class="sxs-lookup"><span data-stu-id="96012-704">If all users on a DW1000 were running in mediumrc, each query would be allocated 800 MB per distribution for a total memory allocation of 47 GB per query, and concurrency would be limited too5 users (40 concurrency slots / 8 slots per mediumrc user).</span></span>

## <a name="selecting-proper-resource-class"></a><span data-ttu-id="96012-705">Megfelelő erőforrásosztály kiválasztása</span><span class="sxs-lookup"><span data-stu-id="96012-705">Selecting proper resource class</span></span>  
<span data-ttu-id="96012-706">Bevált gyakorlat az erőforrás-osztályok módosítása helyett toopermanently hozzárendelése felhasználók tooa erőforrásosztály.</span><span class="sxs-lookup"><span data-stu-id="96012-706">A good practice is toopermanently assign users tooa resource class rather than changing their resource classes.</span></span> <span data-ttu-id="96012-707">Például, terhelések tooclustered oszloptárindexű táblákat hoz létre magasabb színvonalú indexeket, amikor több memóriát foglal le.</span><span class="sxs-lookup"><span data-stu-id="96012-707">For example, loads tooclustered columnstore tables create higher-quality indexes when allocated more memory.</span></span> <span data-ttu-id="96012-708">tooensure, hogy terhelés toohigher memória rendelkezik-e, kifejezetten az adatok betöltése a felhasználó létrehozása, és véglegesen rendelje hozzá a felhasználó tooa magasabb erőforrásosztály.</span><span class="sxs-lookup"><span data-stu-id="96012-708">tooensure that loads have access toohigher memory, create a user specifically for loading data and permanently assign this user tooa higher resource class.</span></span>
<span data-ttu-id="96012-709">Nincsenek ajánlott eljárások toofollow Itt néhány.</span><span class="sxs-lookup"><span data-stu-id="96012-709">There are a couple of best practices toofollow here.</span></span> <span data-ttu-id="96012-710">Fent említett SQL DW támogatja-e a kétféle típusú erőforrások osztály: erőforrás statikus és dinamikus erőforrás-osztályok.</span><span class="sxs-lookup"><span data-stu-id="96012-710">As mentioned above, SQL DW supports two kinds of resource class types: static resource classes and dynamic resource classes.</span></span>
### <a name="loading-best-practices"></a><span data-ttu-id="96012-711">Gyakorlati tanácsok betöltése</span><span class="sxs-lookup"><span data-stu-id="96012-711">Loading best practices</span></span>
1.  <span data-ttu-id="96012-712">Ha hello elvárásainak betölti a rendszeres adatmennyiséget, statikus erőforrásosztály érdemes használni.</span><span class="sxs-lookup"><span data-stu-id="96012-712">If hello expectations are loads with regular amount of data, a static resource class is a good choice.</span></span> <span data-ttu-id="96012-713">Később további számítási teljesítménnyel rendelkezik, hello adatraktár vertikális felskálázásával tooget tudnak több egyidejű toorun lekérdezi out-of-az-box, hello terhelés felhasználói nem több memóriát igényel.</span><span class="sxs-lookup"><span data-stu-id="96012-713">Later, when scaling up tooget more computational power, hello data warehouse will be able toorun more concurrent queries out-of-the-box, as hello load user does not consume more memory.</span></span>
2.  <span data-ttu-id="96012-714">Ha hello elvárásainak nagyobb terhelés bizonyos esetekben a, dinamikus erőforrásosztály érdemes használni.</span><span class="sxs-lookup"><span data-stu-id="96012-714">If hello expectations are bigger loads in some occasions, a dynamic resource class is a good choice.</span></span> <span data-ttu-id="96012-715">Később, amikor vertikális felskálázásával tooget további számítási teljesítménnyel rendelkezik, hello terhelés felhasználó kap további memória az a-kész, ezért így gyorsabban hello terhelés tooperform.</span><span class="sxs-lookup"><span data-stu-id="96012-715">Later, when scaling up tooget more computational power, hello load user will get more memory out-of-the-box, hence allowing hello load tooperform faster.</span></span>

<span data-ttu-id="96012-716">hello szükséges memóriát tooprocess terhelések hatékonyan függ betöltött hello tábla és a feldolgozandó adatok mennyiségétől hello hello jellegét.</span><span class="sxs-lookup"><span data-stu-id="96012-716">hello memory needed tooprocess loads efficiently depends on hello nature of hello table loaded and hello amount of data processed.</span></span> <span data-ttu-id="96012-717">Például adatok közösségi koordináló intézet táblába töltéséhez szükséges néhány memória toolet közösségi koordináló intézet rowgroups optimalizálási elérni.</span><span class="sxs-lookup"><span data-stu-id="96012-717">For instance, loading data into CCI tables requires some memory toolet CCI rowgroups reach optimality.</span></span> <span data-ttu-id="96012-718">További részletekért lásd: hello Oszlopcentrikus indexek - Adatbetöltési útmutatást.</span><span class="sxs-lookup"><span data-stu-id="96012-718">For more details, see hello Columnstore indexes - data loading guidance.</span></span>

<span data-ttu-id="96012-719">Ajánlott eljárásként javasoljuk toouse legalább 200MB memória terhelések.</span><span class="sxs-lookup"><span data-stu-id="96012-719">As a best practice, we advise you toouse at least 200MB of memory for loads.</span></span>

### <a name="querying-best-practices"></a><span data-ttu-id="96012-720">Gyakorlati tanácsok lekérdezése</span><span class="sxs-lookup"><span data-stu-id="96012-720">Querying best practices</span></span>
<span data-ttu-id="96012-721">Lekérdezések összetettségük függően különböző követelményekkel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="96012-721">Queries have different requirements depending on their complexity.</span></span> <span data-ttu-id="96012-722">Memória lekérdezésenként megnövelni, vagy növekvő hello párhuzamossági mindkét érvényes módon tooaugment hello lekérdezés igényeitől függően a teljes teljesítményt.</span><span class="sxs-lookup"><span data-stu-id="96012-722">Increasing memory per query or increasing hello concurrency are both valid ways tooaugment overall throughput depending on hello query needs.</span></span>
1.  <span data-ttu-id="96012-723">Hello elvárásainak rendszeres, összetett lekérdezések esetén (például toogenerate napi és heti jelentések), és nem kell egyidejűségi tootake előnyeit, dinamikus erőforrásosztály, akkor a.</span><span class="sxs-lookup"><span data-stu-id="96012-723">If hello expectations are regular, complex queries (for instance, toogenerate daily and weekly reports) and do not need tootake advantage of concurrency, a dynamic resource class is a good choice.</span></span> <span data-ttu-id="96012-724">Ha hello rendszer további adatokat tooprocess, hello adatraktár vertikális felskálázásával ezért automatikusan nyújt további memória toohello felhasználói hello a lekérdezésnek a futtatása.</span><span class="sxs-lookup"><span data-stu-id="96012-724">If hello system has more data tooprocess, scaling up hello data warehouse will therefore automatically provide more memory toohello user running hello query.</span></span>
2.  <span data-ttu-id="96012-725">Ha hello elvárásainak változó vagy diurnal egyidejűségi minták (például ha hello adatbázis lekérik a webes felhasználói felület széles körben elérhető keresztül), egy statikus erőforrásosztály, akkor a.</span><span class="sxs-lookup"><span data-stu-id="96012-725">If hello expectations are variable or diurnal concurrency patterns (for instance if hello database is queried through a web UI broadly accessible), a static resource class is a good choice.</span></span> <span data-ttu-id="96012-726">Később, amikor toodata adatraktár vertikális felskálázásával, hello statikus erőforrásosztály társított hello felhasználó automatikusan kell tudni toorun több egyidejű lekérdezéseket.</span><span class="sxs-lookup"><span data-stu-id="96012-726">Later, when scaling up toodata warehouse, hello user associated with hello static resource class will automatically be able toorun more concurrent queries.</span></span>

<span data-ttu-id="96012-727">Ez számos tényezőtől függ, például hello mennyiségét, lekérdezett adatok hello táblasémákat, és különböző csatlakozást, kiválasztása és a csoport predikátumok hello jellege nem nem triviális, attól függően, hogy a lekérdezés hello kell kiválasztásával megfelelő memóriaengedély.</span><span class="sxs-lookup"><span data-stu-id="96012-727">Selecting proper memory grant depending on hello need of your query is non-trivial, as it depends on many factors, such as hello amount of data queried, hello nature of hello table schemas, and various join, selection, and group predicates.</span></span> <span data-ttu-id="96012-728">Egy általános tükrözik, rendeljen több memóriát lehetővé teszi lekérdezések toocomplete gyorsabb, de csökkenne teljes feldolgozási hello.</span><span class="sxs-lookup"><span data-stu-id="96012-728">From a general standpoint, allocating more memory will allow queries toocomplete faster, but would reduce hello overall concurrency.</span></span> <span data-ttu-id="96012-729">Párhuzamossági darabolása nem okoz problémát, ha nem árt túlzott memória lefoglalásakor.</span><span class="sxs-lookup"><span data-stu-id="96012-729">If concurrency is not an issue, over-allocating memory does not harm.</span></span> <span data-ttu-id="96012-730">toofine-hangolási átviteli különböző változataira jellemző az erőforrás-osztályok tett kísérlet lehet szükség.</span><span class="sxs-lookup"><span data-stu-id="96012-730">toofine-tune throughput, trying various flavors of resource classes may be required.</span></span>

<span data-ttu-id="96012-731">Hello alábbi tárolt eljárás toofigure kimenő feldolgozási és memória grant / erőforrásosztály adott SLO és hello legközelebbi legjobb erőforrás osztályra memória intenzív közösségi koordináló intézet műveletekhez közösségi koordináló intézet tábla particionálva adott erőforrás osztályra:</span><span class="sxs-lookup"><span data-stu-id="96012-731">You can use hello following stored procedure toofigure out concurrency and memory grant per resource class at a given SLO and hello closest best resource class for memory intensive CCI operations on non-partitioned CCI table at a given resource class:</span></span>

#### <a name="description"></a><span data-ttu-id="96012-732">Leírás:</span><span class="sxs-lookup"><span data-stu-id="96012-732">Description:</span></span>  
<span data-ttu-id="96012-733">A tárolt eljárás hello célja van:</span><span class="sxs-lookup"><span data-stu-id="96012-733">Here's hello purpose of this stored procedure:</span></span>  
1. <span data-ttu-id="96012-734">toohelp felhasználói vizsgálhatja meg erőforrás osztály egy adott SLO / feldolgozási és memória biztosítása.</span><span class="sxs-lookup"><span data-stu-id="96012-734">toohelp user figure out concurrency and memory grant per resource class at a given SLO.</span></span> <span data-ttu-id="96012-735">Felhasználói igényeihez tooprovide NULL séma és a tablename ehhez az alábbi hello példában látható módon.</span><span class="sxs-lookup"><span data-stu-id="96012-735">User needs tooprovide NULL for both schema and tablename for this as shown in hello example below.</span></span>  
2. <span data-ttu-id="96012-736">hello legközelebbi legjobb tartozó erőforrásosztály hello memória intensed közösségi koordináló intézet műveleteket (terhelésétől, a másolási tábla rebuild index stb.) a megadott erőforrásosztály nem particionált közösségi koordináló intézet táblázat elháríthassa toohelp felhasználó.</span><span class="sxs-lookup"><span data-stu-id="96012-736">toohelp user figure out hello closest best resource class for hello memory intensed CCI operations (load, copy table, rebuild index, etc.) on non partitioned CCI table at a given resource class.</span></span> <span data-ttu-id="96012-737">hello tárolt eljárás tábla séma toofind kimenő hello szükséges memória biztosítása a használja.</span><span class="sxs-lookup"><span data-stu-id="96012-737">hello stored proc uses table schema toofind out hello required memory grant for this.</span></span>

#### <a name="dependencies--restrictions"></a><span data-ttu-id="96012-738">Függőségek és korlátozások:</span><span class="sxs-lookup"><span data-stu-id="96012-738">Dependencies & Restrictions:</span></span>
- <span data-ttu-id="96012-739">A tárolt eljárás nincs particionálva közösségi koordináló intézet tábla tervezett toocalculate memóriakövetelményét.</span><span class="sxs-lookup"><span data-stu-id="96012-739">This stored proc is not designed toocalculate memory requirement for partitioned-cci table.</span></span>    
- <span data-ttu-id="96012-740">A tárolt eljárás nem memóriakövetelményét CTAS/INSERT-VÁLASZTANI VÁLASSZA részét hello figyelembe veszi, és azt feltételezi, hogy toobe egy egyszerű jelöljön ki.</span><span class="sxs-lookup"><span data-stu-id="96012-740">This stored proc doesn't take memory requirement into account for hello SELECT part of CTAS/INSERT-SELECT and assumes it toobe a simple SELECT.</span></span>
- <span data-ttu-id="96012-741">A tárolt eljárás egy ideiglenes táblát használ, így ez használható hello munkamenetben hol jött létre a tárolt eljáráshoz.</span><span class="sxs-lookup"><span data-stu-id="96012-741">This stored proc uses a temp table so this can be used in hello session where this stored proc was created.</span></span>    
- <span data-ttu-id="96012-742">A tárolt eljárás hello aktuális offerings (pl. hardverkonfiguráció, DMS config) függ, és ha bármelyik, amely módosítja majd a tárolt eljárás nem megfelelően fog működni.</span><span class="sxs-lookup"><span data-stu-id="96012-742">This stored proc depends on hello current offerings (e.g. hardware configuration, DMS config) and if any of that changes then this stored proc would not work correctly.</span></span>  
- <span data-ttu-id="96012-743">A tárolt eljárás attól függ, meglévő felajánlott feldolgozási korlátja, és ha megváltozik, majd a tárolt eljárás nem megfelelően fog működni.</span><span class="sxs-lookup"><span data-stu-id="96012-743">This stored proc depends on existing offered concurrency limit and if that changes then this stored proc would not work correctly.</span></span>  
- <span data-ttu-id="96012-744">A tárolt eljárás attól függ, meglévő erőforrás osztály ajánlatokat, és ha, amely módosítja majd ez tárolása megfelelő proc wuold nem működik.</span><span class="sxs-lookup"><span data-stu-id="96012-744">This stored proc depends on existing resource class offerings and if that changes then this stored proc wuold not work correctly.</span></span>  

>  [!NOTE]  
>  <span data-ttu-id="96012-745">Ha nem kap kimeneti megadva, előfordulhat, hogy két esetben paraméterekkel tárolt eljárás végrehajtása után.</span><span class="sxs-lookup"><span data-stu-id="96012-745">If you are not getting output after executing stored procedure with parameters provided then there could be two cases.</span></span> <br /><span data-ttu-id="96012-746">1. Vagy DW paraméter érvénytelen SLO értéket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="96012-746">1. Either DW Parameter contains invalid SLO value</span></span> <br /><span data-ttu-id="96012-747">2. VAGY nem megfelelő erőforrásosztály közösségi koordináló intézet művelet Ha megadva tábla neve.</span><span class="sxs-lookup"><span data-stu-id="96012-747">2. OR there are no matching resource class for CCI operation if table name was provided.</span></span> <br /><span data-ttu-id="96012-748">Például: DW100, legmagasabb rendelkezésre memóriaengedély 400MB és széles táblaséma esetén elegendő toocross hello követelmény 400 MB.</span><span class="sxs-lookup"><span data-stu-id="96012-748">For example, at DW100, highest memory grant available is 400MB and if table schema is wide enough toocross hello requirement of 400MB.</span></span>
      
#### <a name="usage-example"></a><span data-ttu-id="96012-749">Példa:</span><span class="sxs-lookup"><span data-stu-id="96012-749">Usage example:</span></span>
<span data-ttu-id="96012-750">Szintaxis:</span><span class="sxs-lookup"><span data-stu-id="96012-750">Syntax:</span></span>  
`EXEC dbo.prc_workload_management_by_DWU @DWU VARCHAR(7), @SCHEMA_NAME VARCHAR(128), @TABLE_NAME VARCHAR(128)`  
1. <span data-ttu-id="96012-751">@DWU:Adjon meg egy NULL paraméter tooextract aktuális DWU az Adatraktár-adatbázisban hello hello, vagy adjon meg, az "DW100" hello formájában DWU támogatott</span><span class="sxs-lookup"><span data-stu-id="96012-751">@DWU: Either provide a NULL parameter tooextract hello current DWU from hello DW DB or provide any supported DWU in hello form of 'DW100'</span></span>
2. <span data-ttu-id="96012-752">@SCHEMA_NAME:Adjon meg egy séma hello tábla neve</span><span class="sxs-lookup"><span data-stu-id="96012-752">@SCHEMA_NAME: Provide a schema name of hello table</span></span>
3. <span data-ttu-id="96012-753">@TABLE_NAME:Adjon meg egy tábla nevét hello érdeklő</span><span class="sxs-lookup"><span data-stu-id="96012-753">@TABLE_NAME: Provide a table name of hello interest</span></span>

<span data-ttu-id="96012-754">A tárolt eljárás végrehajtása példák:</span><span class="sxs-lookup"><span data-stu-id="96012-754">Examples executing this stored proc:</span></span>  
```sql  
EXEC dbo.prc_workload_management_by_DWU 'DW2000', 'dbo', 'Table1';  
EXEC dbo.prc_workload_management_by_DWU NULL, 'dbo', 'Table1';  
EXEC dbo.prc_workload_management_by_DWU 'DW6000', NULL, NULL;  
EXEC dbo.prc_workload_management_by_DWU NULL, NULL, NULL;  
```

<span data-ttu-id="96012-755">A fenti példák hello használt Table1 sikerült létrehozni, az alábbi:</span><span class="sxs-lookup"><span data-stu-id="96012-755">Table1 used in hello above examples could be created as below:</span></span>  
`CREATE TABLE Table1 (a int, b varchar(50), c decimal (18,10), d char(10), e varbinary(15), f float, g datetime, h date);`

#### <a name="heres-hello-stored-procedure-definition"></a><span data-ttu-id="96012-756">Íme hello tárolt eljárás definíciója:</span><span class="sxs-lookup"><span data-stu-id="96012-756">Here's hello stored procedure definition:</span></span>
```sql  
-------------------------------------------------------------------------------
-- Dropping prc_workload_management_by_DWU procedure if it exists.
-------------------------------------------------------------------------------
IF EXISTS (SELECT * FROM sys.objects WHERE type = 'P' AND name = 'prc_workload_management_by_DWU')
DROP PROCEDURE dbo.prc_workload_management_by_DWU
GO

-------------------------------------------------------------------------------
-- Creating prc_workload_management_by_DWU.
-------------------------------------------------------------------------------
CREATE PROCEDURE dbo.prc_workload_management_by_DWU
(   @DWU VARCHAR(7),
      @SCHEMA_NAME VARCHAR(128),
       @TABLE_NAME VARCHAR(128)
)
AS
IF @DWU IS NULL
BEGIN
-- Selecting proper DWU for hello current DB if not specified.
SET @DWU = (
  SELECT 'DW'+CAST(COUNT(*)*100 AS VARCHAR(10))
  FROM sys.dm_pdw_nodes
  WHERE type = 'COMPUTE')
END

DECLARE @DWU_NUM INT
SET @DWU_NUM = CAST (SUBSTRING(@DWU, 3, LEN(@DWU)-2) AS INT)

-- Raise error if either schema name or table name is supplied but not both them supplied
--IF ((@SCHEMA_NAME IS NOT NULL AND @TABLE_NAME IS NULL) OR (@TABLE_NAME IS NULL AND @SCHEMA_NAME IS NOT NULL))
--     RAISEERROR('User need toosupply either both Schema Name and Table Name or none of them')
       
-- Dropping temp table if exists.
IF OBJECT_ID('tempdb..#ref') IS NOT NULL
BEGIN
  DROP TABLE #ref;
END

-- Creating ref. temptable (CTAS) toohold mapping info.
-- CREATE TABLE #ref
CREATE TABLE #ref
WITH (DISTRIBUTION = ROUND_ROBIN)
AS 
WITH
-- Creating concurrency slots mapping for various DWUs.
alloc
AS
(
  SELECT 'DW100' AS DWU, 4 AS max_queries, 4 AS max_slots, 1 AS slots_used_smallrc, 1 AS slots_used_mediumrc,
        2 AS slots_used_largerc, 4 AS slots_used_xlargerc, 1 AS slots_used_staticrc10, 2 AS slots_used_staticrc20,
        4 AS slots_used_staticrc30, 4 AS slots_used_staticrc40, 4 AS slots_used_staticrc50,
        4 AS slots_used_staticrc60, 4 AS slots_used_staticrc70, 4 AS slots_used_staticrc80
  UNION ALL
    SELECT 'DW200', 8, 8, 1, 2, 4, 8, 1, 2, 4, 8, 8, 8, 8, 8
  UNION ALL
    SELECT 'DW300', 12, 12, 1, 2, 4, 8, 1, 2, 4, 8, 8, 8, 8, 8
  UNION ALL
    SELECT 'DW400', 16, 16, 1, 4, 8, 16, 1, 2, 4, 8, 16, 16, 16, 16
  UNION ALL
     SELECT 'DW500', 20, 20, 1, 4, 8, 16, 1, 2, 4, 8, 16, 16, 16, 16
  UNION ALL
    SELECT 'DW600', 24, 24, 1, 4, 8, 16, 1, 2, 4, 8, 16, 16, 16, 16
  UNION ALL
    SELECT 'DW1000', 32, 40, 1, 8, 16, 32, 1, 2, 4, 8, 16, 32, 32, 32
  UNION ALL
    SELECT 'DW1200', 32, 48, 1, 8, 16, 32, 1, 2, 4, 8, 16, 32, 32, 32
  UNION ALL
    SELECT 'DW1500', 32, 60, 1, 8, 16, 32, 1, 2, 4, 8, 16, 32, 32, 32
  UNION ALL
    SELECT 'DW2000', 32, 80, 1, 16, 32, 64, 1, 2, 4, 8, 16, 32, 64, 64
  UNION ALL
   SELECT 'DW3000', 32, 120, 1, 16, 32, 64, 1, 2, 4, 8, 16, 32, 64, 64
  UNION ALL
    SELECT 'DW6000', 32, 240, 1, 32, 64, 128, 1, 2, 4, 8, 16, 32, 64, 128
)
-- Creating workload mapping tootheir corresponding slot consumption and default memory grant.
,map
AS
(
  SELECT 'SloDWGroupC00' AS wg_name,1 AS slots_used,100 AS tgt_mem_grant_MB
  UNION ALL
    SELECT 'SloDWGroupC01',2,200
  UNION ALL
    SELECT 'SloDWGroupC02',4,400
  UNION ALL
    SELECT 'SloDWGroupC03',8,800
  UNION ALL
    SELECT 'SloDWGroupC04',16,1600
  UNION ALL
    SELECT 'SloDWGroupC05',32,3200
  UNION ALL
    SELECT 'SloDWGroupC06',64,6400
  UNION ALL
    SELECT 'SloDWGroupC07',128,12800
)
-- Creating ref based on current / asked DWU.
, ref
AS
(
  SELECT  a1.*
  ,       m1.wg_name          AS wg_name_smallrc
  ,       m1.tgt_mem_grant_MB AS tgt_mem_grant_MB_smallrc
  ,       m2.wg_name          AS wg_name_mediumrc
  ,       m2.tgt_mem_grant_MB AS tgt_mem_grant_MB_mediumrc
  ,       m3.wg_name          AS wg_name_largerc
  ,       m3.tgt_mem_grant_MB AS tgt_mem_grant_MB_largerc
  ,       m4.wg_name          AS wg_name_xlargerc
  ,       m4.tgt_mem_grant_MB AS tgt_mem_grant_MB_xlargerc
  ,       m5.wg_name          AS wg_name_staticrc10
  ,       m5.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc10
  ,       m6.wg_name          AS wg_name_staticrc20
  ,       m6.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc20
  ,       m7.wg_name          AS wg_name_staticrc30
  ,       m7.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc30
  ,       m8.wg_name          AS wg_name_staticrc40
  ,       m8.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc40
  ,       m9.wg_name          AS wg_name_staticrc50
  ,       m9.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc50
  ,       m10.wg_name          AS wg_name_staticrc60
  ,       m10.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc60
  ,       m11.wg_name          AS wg_name_staticrc70
  ,       m11.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc70
  ,       m12.wg_name          AS wg_name_staticrc80
  ,       m12.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc80
  FROM alloc a1
  JOIN map   m1  ON a1.slots_used_smallrc     = m1.slots_used
  JOIN map   m2  ON a1.slots_used_mediumrc    = m2.slots_used
  JOIN map   m3  ON a1.slots_used_largerc     = m3.slots_used
  JOIN map   m4  ON a1.slots_used_xlargerc    = m4.slots_used
  JOIN map   m5  ON a1.slots_used_staticrc10    = m5.slots_used
  JOIN map   m6  ON a1.slots_used_staticrc20    = m6.slots_used
  JOIN map   m7  ON a1.slots_used_staticrc30    = m7.slots_used
  JOIN map   m8  ON a1.slots_used_staticrc40    = m8.slots_used
  JOIN map   m9  ON a1.slots_used_staticrc50    = m9.slots_used
  JOIN map   m10  ON a1.slots_used_staticrc60    = m10.slots_used
  JOIN map   m11  ON a1.slots_used_staticrc70    = m11.slots_used
  JOIN map   m12  ON a1.slots_used_staticrc80    = m12.slots_used
-- WHERE   a1.DWU = @DWU
  WHERE   a1.DWU = UPPER(@DWU)
)
SELECT  DWU
,       max_queries
,       max_slots
,       slots_used
,       wg_name
,       tgt_mem_grant_MB
,       up1 as rc
,       (ROW_NUMBER() OVER(PARTITION BY DWU ORDER BY DWU)) as rc_id
FROM
(
    SELECT  DWU
    ,       max_queries
    ,       max_slots
    ,       slots_used
    ,       wg_name
    ,       tgt_mem_grant_MB
    ,       REVERSE(SUBSTRING(REVERSE(wg_names),1,CHARINDEX('_',REVERSE(wg_names),1)-1)) as up1
    ,       REVERSE(SUBSTRING(REVERSE(tgt_mem_grant_MBs),1,CHARINDEX('_',REVERSE(tgt_mem_grant_MBs),1)-1)) as up2
    ,       REVERSE(SUBSTRING(REVERSE(slots_used_all),1,CHARINDEX('_',REVERSE(slots_used_all),1)-1)) as up3
    FROM    ref AS r1
    UNPIVOT
    (
        wg_name FOR wg_names IN (wg_name_smallrc,wg_name_mediumrc,wg_name_largerc,wg_name_xlargerc,
        wg_name_staticrc10, wg_name_staticrc20, wg_name_staticrc30, wg_name_staticrc40, wg_name_staticrc50,
        wg_name_staticrc60, wg_name_staticrc70, wg_name_staticrc80)
    ) AS r2
    UNPIVOT
    (
        tgt_mem_grant_MB FOR tgt_mem_grant_MBs IN (tgt_mem_grant_MB_smallrc,tgt_mem_grant_MB_mediumrc,
        tgt_mem_grant_MB_largerc,tgt_mem_grant_MB_xlargerc, tgt_mem_grant_MB_staticrc10, tgt_mem_grant_MB_staticrc20,
        tgt_mem_grant_MB_staticrc30, tgt_mem_grant_MB_staticrc40, tgt_mem_grant_MB_staticrc50,
        tgt_mem_grant_MB_staticrc60, tgt_mem_grant_MB_staticrc70, tgt_mem_grant_MB_staticrc80)
    ) AS r3
    UNPIVOT
    (
        slots_used FOR slots_used_all IN (slots_used_smallrc,slots_used_mediumrc,slots_used_largerc,
        slots_used_xlargerc, slots_used_staticrc10, slots_used_staticrc20, slots_used_staticrc30,
        slots_used_staticrc40, slots_used_staticrc50, slots_used_staticrc60, slots_used_staticrc70,
        slots_used_staticrc80)
    ) AS r4
) a
WHERE   up1 = up2
AND     up1 = up3
;
-- Getting current info about workload groups.
WITH  
dmv  
AS  
(
  SELECT
          rp.name                                           AS rp_name
  ,       rp.max_memory_kb*1.0/1048576                      AS rp_max_mem_GB
  ,       (rp.max_memory_kb*1.0/1024)
          *(request_max_memory_grant_percent/100)           AS max_memory_grant_MB
  ,       (rp.max_memory_kb*1.0/1048576)
          *(request_max_memory_grant_percent/100)           AS max_memory_grant_GB
  ,       wg.name                                           AS wg_name
  ,       wg.importance                                     AS importance
  ,       wg.request_max_memory_grant_percent               AS request_max_memory_grant_percent
  FROM    sys.dm_pdw_nodes_resource_governor_workload_groups wg
  JOIN    sys.dm_pdw_nodes_resource_governor_resource_pools rp    ON  wg.pdw_node_id  = rp.pdw_node_id
                                                                  AND wg.pool_id      = rp.pool_id
  WHERE   rp.name = 'SloDWPool'
  GROUP BY
          rp.name
  ,       rp.max_memory_kb
  ,       wg.name
  ,       wg.importance
  ,       wg.request_max_memory_grant_percent
)
-- Creating resource class name mapping.
,names
AS
(
  SELECT 'smallrc' as resource_class, 1 as rc_id
  UNION ALL
    SELECT 'mediumrc', 2
  UNION ALL
    SELECT 'largerc', 3
  UNION ALL
    SELECT 'xlargerc', 4
  UNION ALL
    SELECT 'staticrc10', 5
  UNION ALL
    SELECT 'staticrc20', 6
  UNION ALL
    SELECT 'staticrc30', 7
  UNION ALL
    SELECT 'staticrc40', 8
  UNION ALL
    SELECT 'staticrc50', 9
  UNION ALL
    SELECT 'staticrc60', 10
  UNION ALL
    SELECT 'staticrc70', 11
  UNION ALL
    SELECT 'staticrc80', 12
)
,base AS
(   SELECT  schema_name
    ,       table_name
    ,       SUM(column_count)                   AS column_count
    ,       ISNULL(SUM(short_string_column_count),0)   AS short_string_column_count
    ,       ISNULL(SUM(long_string_column_count),0)    AS long_string_column_count
    FROM    (   SELECT  sm.name                                             AS schema_name
                ,       tb.name                                             AS table_name
                ,       COUNT(co.column_id)                                 AS column_count
                           ,       CASE    WHEN co.system_type_id IN (36,43,106,108,165,167,173,175,231,239) 
                                AND  co.max_length <= 32 
                                THEN COUNT(co.column_id) 
                        END                                                 AS short_string_column_count
                ,       CASE    WHEN co.system_type_id IN (165,167,173,175,231,239) 
                                AND  co.max_length > 32 and co.max_length <=8000
                                THEN COUNT(co.column_id) 
                        END                                                 AS long_string_column_count
                FROM    sys.schemas AS sm
                JOIN    sys.tables  AS tb   on sm.[schema_id] = tb.[schema_id]
                JOIN    sys.columns AS co   ON tb.[object_id] = co.[object_id]
                           WHERE tb.name = @TABLE_NAME AND sm.name = @SCHEMA_NAME
                GROUP BY sm.name
                ,        tb.name
                ,        co.system_type_id
                ,        co.max_length            ) a
GROUP BY schema_name
,        table_name
)
, size AS
(
SELECT  schema_name
,       table_name
,       75497472                                            AS table_overhead

,       column_count*1048576*8                              AS column_size
,       short_string_column_count*1048576*32                       AS short_string_size,       (long_string_column_count*16777216) AS long_string_size
FROM    base
UNION
SELECT CASE WHEN COUNT(*) = 0 THEN 'EMPTY' END as schema_name
         ,CASE WHEN COUNT(*) = 0 THEN 'EMPTY' END as table_name
         ,CASE WHEN COUNT(*) = 0 THEN 0 END as table_overhead
         ,CASE WHEN COUNT(*) = 0 THEN 0 END as column_size
         ,CASE WHEN COUNT(*) = 0 THEN 0 END as short_string_size

,CASE WHEN COUNT(*) = 0 THEN 0 END as long_string_size
FROM   base
)
, load_multiplier as 
(
SELECT  CASE 
                     WHEN FLOOR(8 * (CAST (@DWU_NUM AS FLOAT)/6000)) > 0 THEN FLOOR(8 * (CAST (@DWU_NUM AS FLOAT)/6000)) 
                     ELSE 1 
              END AS multipliplication_factor
) 
       SELECT  r1.DWU
       , schema_name
       , table_name
       , rc.resource_class as closest_rc_in_increasing_order
       , max_queries_at_this_rc = CASE
             WHEN (r1.max_slots / r1.slots_used > r1.max_queries)
                  THEN r1.max_queries
             ELSE r1.max_slots / r1.slots_used
                  END
       , r1.max_slots as max_concurrency_slots
       , r1.slots_used as required_slots_for_the_rc
       , r1.tgt_mem_grant_MB  as rc_mem_grant_MB
       , CAST((table_overhead*1.0+column_size+short_string_size+long_string_size)*multipliplication_factor/1048576    AS DECIMAL(18,2)) AS est_mem_grant_required_for_cci_operation_MB       
       FROM    size, load_multiplier, #ref r1, names  rc
       WHERE r1.rc_id=rc.rc_id
                     AND CAST((table_overhead*1.0+column_size+short_string_size+long_string_size)*multipliplication_factor/1048576    AS DECIMAL(18,2)) < r1.tgt_mem_grant_MB
       ORDER BY ABS(CAST((table_overhead*1.0+column_size+short_string_size+long_string_size)*multipliplication_factor/1048576    AS DECIMAL(18,2)) - r1.tgt_mem_grant_MB)
GO
```

## <a name="query-importance"></a><span data-ttu-id="96012-757">Lekérdezés fontossága</span><span class="sxs-lookup"><span data-stu-id="96012-757">Query importance</span></span>
<span data-ttu-id="96012-758">Az SQL Data Warehouse erőforrás osztályok munkaterhelés-csoport használatával valósít meg.</span><span class="sxs-lookup"><span data-stu-id="96012-758">SQL Data Warehouse implements resource classes by using workload groups.</span></span> <span data-ttu-id="96012-759">Nincsenek összesen nyolc hello erőforrás osztályok hello viselkedését szabályozó hello között különböző DWU méretű munkaterhelés-csoport.</span><span class="sxs-lookup"><span data-stu-id="96012-759">There are a total of eight workload groups that control hello behavior of hello resource classes across hello various DWU sizes.</span></span> <span data-ttu-id="96012-760">A DWU az SQL Data Warehouse csak közül négyet használ hello nyolc munkaterhelés-csoport.</span><span class="sxs-lookup"><span data-stu-id="96012-760">For any DWU, SQL Data Warehouse uses only four of hello eight workload groups.</span></span> <span data-ttu-id="96012-761">Ez teljesen logikus, mert egyes tevékenységprofil-csoport hozzá van rendelve a négy erőforrás osztályok tooone: smallrc, mediumrc, largerc, vagy xlargerc.</span><span class="sxs-lookup"><span data-stu-id="96012-761">This makes sense because each workload group is assigned tooone of four resource classes: smallrc, mediumrc, largerc, or xlargerc.</span></span> <span data-ttu-id="96012-762">hello hello munkaterhelés csoportok ismertetése fontosságát az, hogy a munkaterhelés csoportok beállítása toohigher *fontossági*.</span><span class="sxs-lookup"><span data-stu-id="96012-762">hello importance of understanding hello workload groups is that some of these workload groups are set toohigher *importance*.</span></span> <span data-ttu-id="96012-763">Fontos használt CPU ütemezés.</span><span class="sxs-lookup"><span data-stu-id="96012-763">Importance is used for CPU scheduling.</span></span> <span data-ttu-id="96012-764">A nagyon fontos futtatása lekérdezések háromszor több, mint a közepes fontos CPU-ciklusok fogja kapni.</span><span class="sxs-lookup"><span data-stu-id="96012-764">Queries run with high importance will get three times more CPU cycles than those with medium importance.</span></span> <span data-ttu-id="96012-765">Ezért a feldolgozási tárolóhely hozzárendelések is CPU prioritásának meghatározása.</span><span class="sxs-lookup"><span data-stu-id="96012-765">Therefore, concurrency slot mappings also determine CPU priority.</span></span> <span data-ttu-id="96012-766">Lekérdezés 16 vagy több üzembe helyezési ponti használ fel, ha fut, nagyon fontos.</span><span class="sxs-lookup"><span data-stu-id="96012-766">When a query consumes 16 or more slots, it runs as high importance.</span></span>

<span data-ttu-id="96012-767">hello következő táblázatban minden tevékenységprofil-csoport leképezéseit hello fontosságát.</span><span class="sxs-lookup"><span data-stu-id="96012-767">hello following table shows hello importance mappings for each workload group.</span></span>

### <a name="workload-group-mappings-tooconcurrency-slots-and-importance"></a><span data-ttu-id="96012-768">Munkaterhelési csoport hozzárendelések tooconcurrency tárhelyek és fontossága</span><span class="sxs-lookup"><span data-stu-id="96012-768">Workload group mappings tooconcurrency slots and importance</span></span>
| <span data-ttu-id="96012-769">Munkaterhelés-csoport</span><span class="sxs-lookup"><span data-stu-id="96012-769">Workload groups</span></span> | <span data-ttu-id="96012-770">Párhuzamossági tárolóhely leképezése</span><span class="sxs-lookup"><span data-stu-id="96012-770">Concurrency slot mapping</span></span> | <span data-ttu-id="96012-771">MB / terjesztési</span><span class="sxs-lookup"><span data-stu-id="96012-771">MB / Distribution</span></span> | <span data-ttu-id="96012-772">Fontos leképezése</span><span class="sxs-lookup"><span data-stu-id="96012-772">Importance mapping</span></span> |
|:--- |:---:|:---:|:--- |
| <span data-ttu-id="96012-773">SloDWGroupC00</span><span class="sxs-lookup"><span data-stu-id="96012-773">SloDWGroupC00</span></span> |<span data-ttu-id="96012-774">1</span><span class="sxs-lookup"><span data-stu-id="96012-774">1</span></span> |<span data-ttu-id="96012-775">100</span><span class="sxs-lookup"><span data-stu-id="96012-775">100</span></span> |<span data-ttu-id="96012-776">Közepes</span><span class="sxs-lookup"><span data-stu-id="96012-776">Medium</span></span> |
| <span data-ttu-id="96012-777">SloDWGroupC01</span><span class="sxs-lookup"><span data-stu-id="96012-777">SloDWGroupC01</span></span> |<span data-ttu-id="96012-778">2</span><span class="sxs-lookup"><span data-stu-id="96012-778">2</span></span> |<span data-ttu-id="96012-779">200</span><span class="sxs-lookup"><span data-stu-id="96012-779">200</span></span> |<span data-ttu-id="96012-780">Közepes</span><span class="sxs-lookup"><span data-stu-id="96012-780">Medium</span></span> |
| <span data-ttu-id="96012-781">SloDWGroupC02</span><span class="sxs-lookup"><span data-stu-id="96012-781">SloDWGroupC02</span></span> |<span data-ttu-id="96012-782">4</span><span class="sxs-lookup"><span data-stu-id="96012-782">4</span></span> |<span data-ttu-id="96012-783">400</span><span class="sxs-lookup"><span data-stu-id="96012-783">400</span></span> |<span data-ttu-id="96012-784">Közepes</span><span class="sxs-lookup"><span data-stu-id="96012-784">Medium</span></span> |
| <span data-ttu-id="96012-785">SloDWGroupC03</span><span class="sxs-lookup"><span data-stu-id="96012-785">SloDWGroupC03</span></span> |<span data-ttu-id="96012-786">8</span><span class="sxs-lookup"><span data-stu-id="96012-786">8</span></span> |<span data-ttu-id="96012-787">800</span><span class="sxs-lookup"><span data-stu-id="96012-787">800</span></span> |<span data-ttu-id="96012-788">Közepes</span><span class="sxs-lookup"><span data-stu-id="96012-788">Medium</span></span> |
| <span data-ttu-id="96012-789">SloDWGroupC04</span><span class="sxs-lookup"><span data-stu-id="96012-789">SloDWGroupC04</span></span> |<span data-ttu-id="96012-790">16</span><span class="sxs-lookup"><span data-stu-id="96012-790">16</span></span> |<span data-ttu-id="96012-791">1,600</span><span class="sxs-lookup"><span data-stu-id="96012-791">1,600</span></span> |<span data-ttu-id="96012-792">Magas</span><span class="sxs-lookup"><span data-stu-id="96012-792">High</span></span> |
| <span data-ttu-id="96012-793">SloDWGroupC05</span><span class="sxs-lookup"><span data-stu-id="96012-793">SloDWGroupC05</span></span> |<span data-ttu-id="96012-794">32</span><span class="sxs-lookup"><span data-stu-id="96012-794">32</span></span> |<span data-ttu-id="96012-795">3,200</span><span class="sxs-lookup"><span data-stu-id="96012-795">3,200</span></span> |<span data-ttu-id="96012-796">Magas</span><span class="sxs-lookup"><span data-stu-id="96012-796">High</span></span> |
| <span data-ttu-id="96012-797">SloDWGroupC06</span><span class="sxs-lookup"><span data-stu-id="96012-797">SloDWGroupC06</span></span> |<span data-ttu-id="96012-798">64</span><span class="sxs-lookup"><span data-stu-id="96012-798">64</span></span> |<span data-ttu-id="96012-799">6,400</span><span class="sxs-lookup"><span data-stu-id="96012-799">6,400</span></span> |<span data-ttu-id="96012-800">Magas</span><span class="sxs-lookup"><span data-stu-id="96012-800">High</span></span> |
| <span data-ttu-id="96012-801">SloDWGroupC07</span><span class="sxs-lookup"><span data-stu-id="96012-801">SloDWGroupC07</span></span> |<span data-ttu-id="96012-802">128</span><span class="sxs-lookup"><span data-stu-id="96012-802">128</span></span> |<span data-ttu-id="96012-803">12,800</span><span class="sxs-lookup"><span data-stu-id="96012-803">12,800</span></span> |<span data-ttu-id="96012-804">Magas</span><span class="sxs-lookup"><span data-stu-id="96012-804">High</span></span> |

<span data-ttu-id="96012-805">A hello **foglalási és felhasználási párhuzamossági tárolóhelyek** diagram, ellenőrizheti, hogy egy DW500 használ 1, 4, 8, vagy 16 párhuzamossági üzembe helyezési ponti smallrc, mediumrc, largerc és xlargerc, illetve.</span><span class="sxs-lookup"><span data-stu-id="96012-805">From hello **Allocation and consumption of concurrency slots** chart, you can see that a DW500 uses 1, 4, 8 or 16 concurrency slots for smallrc, mediumrc, largerc, and xlargerc, respectively.</span></span> <span data-ttu-id="96012-806">Megtekintheti ezeket az értékeket a diagram toofind hello fontos az egyes erőforrás megelőző hello.</span><span class="sxs-lookup"><span data-stu-id="96012-806">You can look those values up in hello preceding chart toofind hello importance for each resource class.</span></span>

### <a name="dw500-mapping-of-resource-classes-tooimportance"></a><span data-ttu-id="96012-807">Erőforrás-osztályok tooimportance DW500 leképezése</span><span class="sxs-lookup"><span data-stu-id="96012-807">DW500 mapping of resource classes tooimportance</span></span>
| <span data-ttu-id="96012-808">Erőforrásosztály</span><span class="sxs-lookup"><span data-stu-id="96012-808">Resource class</span></span> | <span data-ttu-id="96012-809">A tevékenységprofil-csoport</span><span class="sxs-lookup"><span data-stu-id="96012-809">Workload group</span></span> | <span data-ttu-id="96012-810">Párhuzamossági tárhelyek használt</span><span class="sxs-lookup"><span data-stu-id="96012-810">Concurrency slots used</span></span> | <span data-ttu-id="96012-811">MB / terjesztési</span><span class="sxs-lookup"><span data-stu-id="96012-811">MB / Distribution</span></span> | <span data-ttu-id="96012-812">Fontos</span><span class="sxs-lookup"><span data-stu-id="96012-812">Importance</span></span> |
|:--- |:--- |:---:|:---:|:--- |
| <span data-ttu-id="96012-813">smallrc</span><span class="sxs-lookup"><span data-stu-id="96012-813">smallrc</span></span> |<span data-ttu-id="96012-814">SloDWGroupC00</span><span class="sxs-lookup"><span data-stu-id="96012-814">SloDWGroupC00</span></span> |<span data-ttu-id="96012-815">1</span><span class="sxs-lookup"><span data-stu-id="96012-815">1</span></span> |<span data-ttu-id="96012-816">100</span><span class="sxs-lookup"><span data-stu-id="96012-816">100</span></span> |<span data-ttu-id="96012-817">Közepes</span><span class="sxs-lookup"><span data-stu-id="96012-817">Medium</span></span> |
| <span data-ttu-id="96012-818">mediumrc</span><span class="sxs-lookup"><span data-stu-id="96012-818">mediumrc</span></span> |<span data-ttu-id="96012-819">SloDWGroupC02</span><span class="sxs-lookup"><span data-stu-id="96012-819">SloDWGroupC02</span></span> |<span data-ttu-id="96012-820">4</span><span class="sxs-lookup"><span data-stu-id="96012-820">4</span></span> |<span data-ttu-id="96012-821">400</span><span class="sxs-lookup"><span data-stu-id="96012-821">400</span></span> |<span data-ttu-id="96012-822">Közepes</span><span class="sxs-lookup"><span data-stu-id="96012-822">Medium</span></span> |
| <span data-ttu-id="96012-823">largerc</span><span class="sxs-lookup"><span data-stu-id="96012-823">largerc</span></span> |<span data-ttu-id="96012-824">SloDWGroupC03</span><span class="sxs-lookup"><span data-stu-id="96012-824">SloDWGroupC03</span></span> |<span data-ttu-id="96012-825">8</span><span class="sxs-lookup"><span data-stu-id="96012-825">8</span></span> |<span data-ttu-id="96012-826">800</span><span class="sxs-lookup"><span data-stu-id="96012-826">800</span></span> |<span data-ttu-id="96012-827">Közepes</span><span class="sxs-lookup"><span data-stu-id="96012-827">Medium</span></span> |
| <span data-ttu-id="96012-828">xlargerc</span><span class="sxs-lookup"><span data-stu-id="96012-828">xlargerc</span></span> |<span data-ttu-id="96012-829">SloDWGroupC04</span><span class="sxs-lookup"><span data-stu-id="96012-829">SloDWGroupC04</span></span> |<span data-ttu-id="96012-830">16</span><span class="sxs-lookup"><span data-stu-id="96012-830">16</span></span> |<span data-ttu-id="96012-831">1,600</span><span class="sxs-lookup"><span data-stu-id="96012-831">1,600</span></span> |<span data-ttu-id="96012-832">Magas</span><span class="sxs-lookup"><span data-stu-id="96012-832">High</span></span> |
| <span data-ttu-id="96012-833">staticrc10</span><span class="sxs-lookup"><span data-stu-id="96012-833">staticrc10</span></span> |<span data-ttu-id="96012-834">SloDWGroupC00</span><span class="sxs-lookup"><span data-stu-id="96012-834">SloDWGroupC00</span></span> |<span data-ttu-id="96012-835">1</span><span class="sxs-lookup"><span data-stu-id="96012-835">1</span></span> |<span data-ttu-id="96012-836">100</span><span class="sxs-lookup"><span data-stu-id="96012-836">100</span></span> |<span data-ttu-id="96012-837">Közepes</span><span class="sxs-lookup"><span data-stu-id="96012-837">Medium</span></span> |
| <span data-ttu-id="96012-838">staticrc20</span><span class="sxs-lookup"><span data-stu-id="96012-838">staticrc20</span></span> |<span data-ttu-id="96012-839">SloDWGroupC01</span><span class="sxs-lookup"><span data-stu-id="96012-839">SloDWGroupC01</span></span> |<span data-ttu-id="96012-840">2</span><span class="sxs-lookup"><span data-stu-id="96012-840">2</span></span> |<span data-ttu-id="96012-841">200</span><span class="sxs-lookup"><span data-stu-id="96012-841">200</span></span> |<span data-ttu-id="96012-842">Közepes</span><span class="sxs-lookup"><span data-stu-id="96012-842">Medium</span></span> |
| <span data-ttu-id="96012-843">staticrc30</span><span class="sxs-lookup"><span data-stu-id="96012-843">staticrc30</span></span> |<span data-ttu-id="96012-844">SloDWGroupC02</span><span class="sxs-lookup"><span data-stu-id="96012-844">SloDWGroupC02</span></span> |<span data-ttu-id="96012-845">4</span><span class="sxs-lookup"><span data-stu-id="96012-845">4</span></span> |<span data-ttu-id="96012-846">400</span><span class="sxs-lookup"><span data-stu-id="96012-846">400</span></span> |<span data-ttu-id="96012-847">Közepes</span><span class="sxs-lookup"><span data-stu-id="96012-847">Medium</span></span> |
| <span data-ttu-id="96012-848">staticrc40</span><span class="sxs-lookup"><span data-stu-id="96012-848">staticrc40</span></span> |<span data-ttu-id="96012-849">SloDWGroupC03</span><span class="sxs-lookup"><span data-stu-id="96012-849">SloDWGroupC03</span></span> |<span data-ttu-id="96012-850">8</span><span class="sxs-lookup"><span data-stu-id="96012-850">8</span></span> |<span data-ttu-id="96012-851">800</span><span class="sxs-lookup"><span data-stu-id="96012-851">800</span></span> |<span data-ttu-id="96012-852">Közepes</span><span class="sxs-lookup"><span data-stu-id="96012-852">Medium</span></span> |
| <span data-ttu-id="96012-853">staticrc50</span><span class="sxs-lookup"><span data-stu-id="96012-853">staticrc50</span></span> |<span data-ttu-id="96012-854">SloDWGroupC03</span><span class="sxs-lookup"><span data-stu-id="96012-854">SloDWGroupC03</span></span> |<span data-ttu-id="96012-855">16</span><span class="sxs-lookup"><span data-stu-id="96012-855">16</span></span> |<span data-ttu-id="96012-856">1,600</span><span class="sxs-lookup"><span data-stu-id="96012-856">1,600</span></span> |<span data-ttu-id="96012-857">Magas</span><span class="sxs-lookup"><span data-stu-id="96012-857">High</span></span> |
| <span data-ttu-id="96012-858">staticrc60</span><span class="sxs-lookup"><span data-stu-id="96012-858">staticrc60</span></span> |<span data-ttu-id="96012-859">SloDWGroupC03</span><span class="sxs-lookup"><span data-stu-id="96012-859">SloDWGroupC03</span></span> |<span data-ttu-id="96012-860">16</span><span class="sxs-lookup"><span data-stu-id="96012-860">16</span></span> |<span data-ttu-id="96012-861">1,600</span><span class="sxs-lookup"><span data-stu-id="96012-861">1,600</span></span> |<span data-ttu-id="96012-862">Magas</span><span class="sxs-lookup"><span data-stu-id="96012-862">High</span></span> |
| <span data-ttu-id="96012-863">staticrc70</span><span class="sxs-lookup"><span data-stu-id="96012-863">staticrc70</span></span> |<span data-ttu-id="96012-864">SloDWGroupC03</span><span class="sxs-lookup"><span data-stu-id="96012-864">SloDWGroupC03</span></span> |<span data-ttu-id="96012-865">16</span><span class="sxs-lookup"><span data-stu-id="96012-865">16</span></span> |<span data-ttu-id="96012-866">1,600</span><span class="sxs-lookup"><span data-stu-id="96012-866">1,600</span></span> |<span data-ttu-id="96012-867">Magas</span><span class="sxs-lookup"><span data-stu-id="96012-867">High</span></span> |
| <span data-ttu-id="96012-868">staticrc80</span><span class="sxs-lookup"><span data-stu-id="96012-868">staticrc80</span></span> |<span data-ttu-id="96012-869">SloDWGroupC03</span><span class="sxs-lookup"><span data-stu-id="96012-869">SloDWGroupC03</span></span> |<span data-ttu-id="96012-870">16</span><span class="sxs-lookup"><span data-stu-id="96012-870">16</span></span> |<span data-ttu-id="96012-871">1,600</span><span class="sxs-lookup"><span data-stu-id="96012-871">1,600</span></span> |<span data-ttu-id="96012-872">Magas</span><span class="sxs-lookup"><span data-stu-id="96012-872">High</span></span> |

<span data-ttu-id="96012-873">Hello hibaelhárításához a következő DMV-lekérdezés toolook hello különbségeit memória erőforrás-elosztás részletesen hello szempontjából erőforrás-vezérlő hello vagy tooanalyze aktív és előzménynaplók használatát hello munkaterhelés-csoport a következő is használhatja.</span><span class="sxs-lookup"><span data-stu-id="96012-873">You can use hello following DMV query toolook at hello differences in memory resource allocation in detail from hello perspective of hello resource governor, or tooanalyze active and historic usage of hello workload groups when troubleshooting.</span></span>

```sql
WITH rg
AS
(   SELECT  
     pn.name                        AS node_name
    ,pn.[type]                        AS node_type
    ,pn.pdw_node_id                    AS node_id
    ,rp.name                        AS pool_name
    ,rp.max_memory_kb*1.0/1024                AS pool_max_mem_MB
    ,wg.name                        AS group_name
    ,wg.importance                    AS group_importance
    ,wg.request_max_memory_grant_percent        AS group_request_max_memory_grant_pcnt
    ,wg.max_dop                        AS group_max_dop
    ,wg.effective_max_dop                AS group_effective_max_dop
    ,wg.total_request_count                AS group_total_request_count
    ,wg.total_queued_request_count            AS group_total_queued_request_count
    ,wg.active_request_count                AS group_active_request_count
    ,wg.queued_request_count                AS group_queued_request_count
    FROM    sys.dm_pdw_nodes_resource_governor_workload_groups wg
    JOIN    sys.dm_pdw_nodes_resource_governor_resource_pools rp    
            ON  wg.pdw_node_id  = rp.pdw_node_id
            AND wg.pool_id      = rp.pool_id
    JOIN    sys.dm_pdw_nodes pn
            ON    wg.pdw_node_id    = pn.pdw_node_id
    WHERE   wg.name like 'SloDWGroup%'
        AND     rp.name = 'SloDWPool'
)
SELECT    pool_name
,        pool_max_mem_MB
,        group_name
,        group_importance
,        (pool_max_mem_MB/100)*group_request_max_memory_grant_pcnt AS max_memory_grant_MB
,        node_name
,        node_type
,       group_total_request_count
,       group_total_queued_request_count
,       group_active_request_count
,       group_queued_request_count
FROM    rg
ORDER BY
    node_name
,    group_request_max_memory_grant_pcnt
,    group_importance
;
```

## <a name="queries-that-honor-concurrency-limits"></a><span data-ttu-id="96012-874">Feldolgozási korlátok tiszteletben lekérdezések</span><span class="sxs-lookup"><span data-stu-id="96012-874">Queries that honor concurrency limits</span></span>
<span data-ttu-id="96012-875">A legtöbb lekérdezések erőforrás osztályok vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="96012-875">Most queries are governed by resource classes.</span></span> <span data-ttu-id="96012-876">Ezeket a lekérdezéseket hello párhuzamos lekérdezés és a párhuzamosság tárolóhely küszöbértékek kell férnie.</span><span class="sxs-lookup"><span data-stu-id="96012-876">These queries must fit inside both hello concurrent query and concurrency slot thresholds.</span></span> <span data-ttu-id="96012-877">A felhasználó nem választhat tooexclude lekérdezés hello párhuzamossági tárolóhely modell.</span><span class="sxs-lookup"><span data-stu-id="96012-877">A user cannot choose tooexclude a query from hello concurrency slot model.</span></span>

<span data-ttu-id="96012-878">tooreiterate, hello következő utasításokat tiszteletben erőforrás osztályok:</span><span class="sxs-lookup"><span data-stu-id="96012-878">tooreiterate, hello following statements honor resource classes:</span></span>

* <span data-ttu-id="96012-879">INSERT-KIVÁLASZTÁSA</span><span class="sxs-lookup"><span data-stu-id="96012-879">INSERT-SELECT</span></span>
* <span data-ttu-id="96012-880">FRISSÍTÉS</span><span class="sxs-lookup"><span data-stu-id="96012-880">UPDATE</span></span>
* <span data-ttu-id="96012-881">TÖRLÉSE</span><span class="sxs-lookup"><span data-stu-id="96012-881">DELETE</span></span>
* <span data-ttu-id="96012-882">Válassza ki a (felhasználói táblák lekérdezésekor)</span><span class="sxs-lookup"><span data-stu-id="96012-882">SELECT (when querying user tables)</span></span>
* <span data-ttu-id="96012-883">ALTER INDEX REBUILD</span><span class="sxs-lookup"><span data-stu-id="96012-883">ALTER INDEX REBUILD</span></span>
* <span data-ttu-id="96012-884">ALTER INDEX ÁTSZERVEZ</span><span class="sxs-lookup"><span data-stu-id="96012-884">ALTER INDEX REORGANIZE</span></span>
* <span data-ttu-id="96012-885">ALTER TABLE REBUILD</span><span class="sxs-lookup"><span data-stu-id="96012-885">ALTER TABLE REBUILD</span></span>
* <span data-ttu-id="96012-886">INDEX LÉTREHOZÁSA</span><span class="sxs-lookup"><span data-stu-id="96012-886">CREATE INDEX</span></span>
* <span data-ttu-id="96012-887">HOZZON LÉTRE FÜRTÖZÖTT OSZLOPCENTRIKUS INDEXET</span><span class="sxs-lookup"><span data-stu-id="96012-887">CREATE CLUSTERED COLUMNSTORE INDEX</span></span>
* <span data-ttu-id="96012-888">TABLE AS SELECT (CTAS) LÉTREHOZÁSA</span><span class="sxs-lookup"><span data-stu-id="96012-888">CREATE TABLE AS SELECT (CTAS)</span></span>
* <span data-ttu-id="96012-889">Az adatok betöltése</span><span class="sxs-lookup"><span data-stu-id="96012-889">Data loading</span></span>
* <span data-ttu-id="96012-890">Az adatátviteli műveletek hello adatok adatátviteli szolgáltatás (DMS) által</span><span class="sxs-lookup"><span data-stu-id="96012-890">Data movement operations conducted by hello Data Movement Service (DMS)</span></span>

## <a name="query-exceptions-tooconcurrency-limits"></a><span data-ttu-id="96012-891">Lekérdezés kivételek tooconcurrency korlátok</span><span class="sxs-lookup"><span data-stu-id="96012-891">Query exceptions tooconcurrency limits</span></span>
<span data-ttu-id="96012-892">Egyes lekérdezések nem fogadják el hello erőforrás osztály toowhich hello felhasználó hozzá van rendelve.</span><span class="sxs-lookup"><span data-stu-id="96012-892">Some queries do not honor hello resource class toowhich hello user is assigned.</span></span> <span data-ttu-id="96012-893">A kivételek toohello feldolgozási korlátok válnak, amikor egy adott parancshoz szükséges hello memória-erőforrások terhelése alacsony, gyakran mivel hello parancs a metaadat-művelet.</span><span class="sxs-lookup"><span data-stu-id="96012-893">These exceptions toohello concurrency limits are made when hello memory resources needed for a particular command are low, often because hello command is a metadata operation.</span></span> <span data-ttu-id="96012-894">hello ezeket a kivételeket célja tooavoid nagyobb memória helylefoglalását lekérdezések, amelyek soha nem kell őket.</span><span class="sxs-lookup"><span data-stu-id="96012-894">hello goal of these exceptions is tooavoid larger memory allocations for queries that will never need them.</span></span> <span data-ttu-id="96012-895">Ezekben az esetekben a tényleges erőforrásosztály hello függetlenül mindig használatra a kis erőforrásosztály (smallrc) hello alapértelmezett hozzárendelt toohello felhasználó.</span><span class="sxs-lookup"><span data-stu-id="96012-895">In these cases, hello default small resource class (smallrc) is always used regardless of hello actual resource class assigned toohello user.</span></span> <span data-ttu-id="96012-896">Például `CREATE LOGIN` mindig smallrc fog futni.</span><span class="sxs-lookup"><span data-stu-id="96012-896">For example, `CREATE LOGIN` will always run in smallrc.</span></span> <span data-ttu-id="96012-897">hello szükséges erőforrások toofulfill ezt a műveletet olyan nagyon alacsony, így nem tesz logika tooinclude hello lekérdezés hello párhuzamossági tárolóhely modellben.</span><span class="sxs-lookup"><span data-stu-id="96012-897">hello resources required toofulfill this operation are very low, so it does not make sense tooinclude hello query in hello concurrency slot model.</span></span>  <span data-ttu-id="96012-898">Ezeket a lekérdezéseket is nincs korlátozva hello 32 felhasználói feldolgozási korlát, korlátlan számú ezeket a lekérdezéseket futtathat toohello munkamenet legfeljebb 1024 munkamenetek fel.</span><span class="sxs-lookup"><span data-stu-id="96012-898">These queries are also not limited by hello 32 user concurrency limit, an unlimited number of these queries can run up toohello session limit of 1,024 sessions.</span></span>

<span data-ttu-id="96012-899">a következő utasítások hello nem fogadják el az erőforrás osztályok:</span><span class="sxs-lookup"><span data-stu-id="96012-899">hello following statements do not honor resource classes:</span></span>

* <span data-ttu-id="96012-900">DROP TABLE vagy létrehozása</span><span class="sxs-lookup"><span data-stu-id="96012-900">CREATE or DROP TABLE</span></span>
* <span data-ttu-id="96012-901">AZ ALTER TABLE... KAPCSOLÓ, a megosztott vagy a partíció EGYESÍTÉSE</span><span class="sxs-lookup"><span data-stu-id="96012-901">ALTER TABLE ... SWITCH, SPLIT, or MERGE PARTITION</span></span>
* <span data-ttu-id="96012-902">AZ ALTER INDEX LETILTÁSA</span><span class="sxs-lookup"><span data-stu-id="96012-902">ALTER INDEX DISABLE</span></span>
* <span data-ttu-id="96012-903">A DROP INDEX</span><span class="sxs-lookup"><span data-stu-id="96012-903">DROP INDEX</span></span>
* <span data-ttu-id="96012-904">LÉTREHOZÁSI, frissítési vagy a DROP STATISTICS</span><span class="sxs-lookup"><span data-stu-id="96012-904">CREATE, UPDATE, or DROP STATISTICS</span></span>
* <span data-ttu-id="96012-905">A TRUNCATE TABLE</span><span class="sxs-lookup"><span data-stu-id="96012-905">TRUNCATE TABLE</span></span>
* <span data-ttu-id="96012-906">AZ ALTER ENGEDÉLYEZÉSI</span><span class="sxs-lookup"><span data-stu-id="96012-906">ALTER AUTHORIZATION</span></span>
* <span data-ttu-id="96012-907">BEJELENTKEZÉS LÉTREHOZÁSA</span><span class="sxs-lookup"><span data-stu-id="96012-907">CREATE LOGIN</span></span>
* <span data-ttu-id="96012-908">CREATE, ALTER és a DROP USER</span><span class="sxs-lookup"><span data-stu-id="96012-908">CREATE, ALTER or DROP USER</span></span>
* <span data-ttu-id="96012-909">CREATE, ALTER vagy DROP ELJÁRÁST</span><span class="sxs-lookup"><span data-stu-id="96012-909">CREATE, ALTER or DROP PROCEDURE</span></span>
* <span data-ttu-id="96012-910">Vagy DROP NÉZET létrehozása</span><span class="sxs-lookup"><span data-stu-id="96012-910">CREATE or DROP VIEW</span></span>
* <span data-ttu-id="96012-911">ÉRTÉKEK BESZÚRÁSA</span><span class="sxs-lookup"><span data-stu-id="96012-911">INSERT VALUES</span></span>
* <span data-ttu-id="96012-912">Válassza ki a rendszer nézetek és dinamikus felügyeleti nézetek</span><span class="sxs-lookup"><span data-stu-id="96012-912">SELECT from system views and DMVs</span></span>
* <span data-ttu-id="96012-913">MAGYARÁZÓ</span><span class="sxs-lookup"><span data-stu-id="96012-913">EXPLAIN</span></span>
* <span data-ttu-id="96012-914">DBCC</span><span class="sxs-lookup"><span data-stu-id="96012-914">DBCC</span></span>

<!--
Removed as these two are not confirmed / supported under SQLDW
- CREATE REMOTE TABLE AS SELECT
- CREATE EXTERNAL TABLE AS SELECT
- REDISTRIBUTE
-->

##  <span data-ttu-id="96012-915"><a name="changing-user-resource-class-example"></a>A felhasználói erőforrás osztály példa módosítása</span><span class="sxs-lookup"><span data-stu-id="96012-915"><a name="changing-user-resource-class-example"></a> Change a user resource class example</span></span>
1. <span data-ttu-id="96012-916">**Hozzon létre bejelentkezési:** nyissa meg a kapcsolat tooyour **fő** adatbázis-hello az SQL Data Warehouse-adatbázist futtató SQL-kiszolgálón, és hajtsa végre a következő parancsok hello.</span><span class="sxs-lookup"><span data-stu-id="96012-916">**Create login:** Open a connection tooyour **master** database on hello SQL server hosting your SQL Data Warehouse database and execute hello following commands.</span></span>
   
    ```sql
    CREATE LOGIN newperson WITH PASSWORD = 'mypassword';
    CREATE USER newperson for LOGIN newperson;
    ```
   
   > [!NOTE]
   > <span data-ttu-id="96012-917">Egy jó ötlet toocreate egy felhasználó fő adatbázis hello Azure SQL Data Warehouse felhasználók.</span><span class="sxs-lookup"><span data-stu-id="96012-917">It is a good idea toocreate a user in hello master database for Azure SQL Data Warehouse users.</span></span> <span data-ttu-id="96012-918">A felhasználó létrehozása a fő lehetővé teszi, hogy egy felhasználó toologin, például az SSMS használatával adatbázis nevének megadása nélkül.</span><span class="sxs-lookup"><span data-stu-id="96012-918">Creating a user in master allows a user toologin using tools like SSMS without specifying a database name.</span></span>  <span data-ttu-id="96012-919">Lehetővé teszi őket toouse hello object explorer tooview összes adatbázist egy SQL-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="96012-919">It also allows them toouse hello object explorer tooview all databases on a SQL server.</span></span>  <span data-ttu-id="96012-920">További létrehozásával és felhasználók kezelésével kapcsolatos további információkért lásd: [az SQL Data Warehouse adatbázis védelme][Secure a database in SQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="96012-920">For more details about creating and managing users, see [Secure a database in SQL Data Warehouse][Secure a database in SQL Data Warehouse].</span></span>
   > 
   > 
2. <span data-ttu-id="96012-921">**Az SQL Data Warehouse-felhasználó létrehozása:** nyissa meg a kapcsolat toohello **SQL Data Warehouse** adatbázis, és hajtsa végre a következő parancs hello.</span><span class="sxs-lookup"><span data-stu-id="96012-921">**Create SQL Data Warehouse user:** Open a connection toohello **SQL Data Warehouse** database and execute hello following command.</span></span>
   
    ```sql
    CREATE USER newperson FOR LOGIN newperson;
    ```
3. <span data-ttu-id="96012-922">**Engedélyek:** hello a következő példa engedélyezi `CONTROL` a hello **SQL Data Warehouse** adatbázis.</span><span class="sxs-lookup"><span data-stu-id="96012-922">**Grant permissions:** hello following example grants `CONTROL` on hello **SQL Data Warehouse** database.</span></span> <span data-ttu-id="96012-923">`CONTROL`a hello adatbázis szintje hello egyenértékű az SQL Server db_owner.</span><span class="sxs-lookup"><span data-stu-id="96012-923">`CONTROL` at hello database level is hello equivalent of db_owner in SQL Server.</span></span>
   
    ```sql
    GRANT CONTROL ON DATABASE::MySQLDW toonewperson;
    ```
4. <span data-ttu-id="96012-924">**Erőforrásosztály növelése:** használata hello következő lekérdezés tooadd felhasználói tooa nagyobb munkaterhelés felügyeleti szerepkört.</span><span class="sxs-lookup"><span data-stu-id="96012-924">**Increase resource class:** Use hello following query tooadd a user tooa higher workload management role.</span></span>
   
    ```sql
    EXEC sp_addrolemember 'largerc', 'newperson'
    ```
5. <span data-ttu-id="96012-925">**Erőforrásosztály csökkentése:** használata hello következő lekérdezés tooremove a felhasználó az alkalmazások és szolgáltatások felügyeleti szerepkör.</span><span class="sxs-lookup"><span data-stu-id="96012-925">**Decrease resource class:** Use hello following query tooremove a user from a workload management role.</span></span>
   
    ```sql
    EXEC sp_droprolemember 'largerc', 'newperson';
    ```
   
   > [!NOTE]
   > <span data-ttu-id="96012-926">Már nem lehetséges tooremove smallrc egy felhasználót.</span><span class="sxs-lookup"><span data-stu-id="96012-926">It is not possible tooremove a user from smallrc.</span></span>
   > 
   > 

## <a name="queued-query-detection-and-other-dmvs"></a><span data-ttu-id="96012-927">Várólistára helyezett lekérdezés felderítését és egyéb dinamikus felügyeleti nézetek</span><span class="sxs-lookup"><span data-stu-id="96012-927">Queued query detection and other DMVs</span></span>
<span data-ttu-id="96012-928">Használhatja a hello `sys.dm_pdw_exec_requests` DMV tooidentify lekérdezések, amelyek a feldolgozási sorban várnak.</span><span class="sxs-lookup"><span data-stu-id="96012-928">You can use hello `sys.dm_pdw_exec_requests` DMV tooidentify queries that are waiting in a concurrency queue.</span></span> <span data-ttu-id="96012-929">Lekérdezi a feldolgozási tárhely állapottal fog rendelkezni várakozással **felfüggesztve**.</span><span class="sxs-lookup"><span data-stu-id="96012-929">Queries waiting for a concurrency slot will have a status of **suspended**.</span></span>

```sql
SELECT      r.[request_id]                 AS Request_ID
        ,r.[status]                 AS Request_Status
        ,r.[submit_time]             AS Request_SubmitTime
        ,r.[start_time]                 AS Request_StartTime
        ,DATEDIFF(ms,[submit_time],[start_time]) AS Request_InitiateDuration_ms
        ,r.resource_class                         AS Request_resource_class
FROM    sys.dm_pdw_exec_requests r;
```

<span data-ttu-id="96012-930">Alkalmazások és szolgáltatások felügyeleti szerepköre megtekinthetők az `sys.database_principals`.</span><span class="sxs-lookup"><span data-stu-id="96012-930">Workload management roles can be viewed with `sys.database_principals`.</span></span>

```sql
SELECT  ro.[name]           AS [db_role_name]
FROM    sys.database_principals ro
WHERE   ro.[type_desc]      = 'DATABASE_ROLE'
AND     ro.[is_fixed_role]  = 0;
```

<span data-ttu-id="96012-931">a következő lekérdezés hello jeleníti meg, milyen szerepkört minden felhasználóhoz hozzá van rendelve.</span><span class="sxs-lookup"><span data-stu-id="96012-931">hello following query shows which role each user is assigned to.</span></span>

```sql
SELECT     r.name AS role_principal_name
        ,m.name AS member_principal_name
FROM    sys.database_role_members rm
JOIN    sys.database_principals AS r            ON rm.role_principal_id        = r.principal_id
JOIN    sys.database_principals AS m            ON rm.member_principal_id    = m.principal_id
WHERE    r.name IN ('mediumrc','largerc', 'xlargerc');
```

<span data-ttu-id="96012-932">Az SQL Data Warehouse rendelkezik hello következő várjon típusok:</span><span class="sxs-lookup"><span data-stu-id="96012-932">SQL Data Warehouse has hello following wait types:</span></span>

* <span data-ttu-id="96012-933">**LocalQueriesConcurrencyResourceType**: hello párhuzamossági tárolóhely keretrendszer kívül elhelyezkedik lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="96012-933">**LocalQueriesConcurrencyResourceType**: Queries that sit outside of hello concurrency slot framework.</span></span> <span data-ttu-id="96012-934">DMV lekérdezések és a rendszer funkciókkal, mint például `SELECT @@VERSION` példák a helyi lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="96012-934">DMV queries and system functions such as `SELECT @@VERSION` are examples of local queries.</span></span>
* <span data-ttu-id="96012-935">**UserConcurrencyResourceType**: hello párhuzamossági tárolóhely keretrendszer belül elhelyezkedik lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="96012-935">**UserConcurrencyResourceType**: Queries that sit inside hello concurrency slot framework.</span></span> <span data-ttu-id="96012-936">Végfelhasználói táblák lekérdezéseket képviselő példák, amelyek szeretné használni az erőforrástípus.</span><span class="sxs-lookup"><span data-stu-id="96012-936">Queries against end-user tables represent examples that would use this resource type.</span></span>
* <span data-ttu-id="96012-937">**DmsConcurrencyResourceType**: megvárja-e az adatátviteli műveletek eredő.</span><span class="sxs-lookup"><span data-stu-id="96012-937">**DmsConcurrencyResourceType**: Waits resulting from data movement operations.</span></span>
* <span data-ttu-id="96012-938">**BackupConcurrencyResourceType**: A várakozás azt jelzi, hogy egy adatbázis biztonsági mentése van folyamatban.</span><span class="sxs-lookup"><span data-stu-id="96012-938">**BackupConcurrencyResourceType**: This wait indicates that a database is being backed up.</span></span> <span data-ttu-id="96012-939">az erőforrástípus hello maximális értéke: 1.</span><span class="sxs-lookup"><span data-stu-id="96012-939">hello maximum value for this resource type is 1.</span></span> <span data-ttu-id="96012-940">Ha több biztonsági mentés: hello kérték ugyanannyi időt vesz igénybe, mások várólistájára hello.</span><span class="sxs-lookup"><span data-stu-id="96012-940">If multiple backups have been requested at hello same time, hello others will queue.</span></span>

<span data-ttu-id="96012-941">Hello `sys.dm_pdw_waits` DMV lehet használt toosee erőforrások kérést vár.</span><span class="sxs-lookup"><span data-stu-id="96012-941">hello `sys.dm_pdw_waits` DMV can be used toosee which resources a request is waiting for.</span></span>

```sql
SELECT  w.[wait_id]
,       w.[session_id]
,       w.[type]            AS Wait_type
,       w.[object_type]
,       w.[object_name]
,       w.[request_id]
,       w.[request_time]
,       w.[acquire_time]
,       w.[state]
,       w.[priority]
,    SESSION_ID()            AS Current_session
,    s.[status]            AS Session_status
,    s.[login_name]
,    s.[query_count]
,    s.[client_id]
,    s.[sql_spid]
,    r.[command]            AS Request_command
,    r.[label]
,    r.[status]            AS Request_status
,    r.[submit_time]
,    r.[start_time]
,    r.[end_compile_time]
,    r.[end_time]
,    DATEDIFF(ms,r.[submit_time],r.[start_time])        AS Request_queue_time_ms
,    DATEDIFF(ms,r.[start_time],r.[end_compile_time])    AS Request_compile_time_ms
,    DATEDIFF(ms,r.[end_compile_time],r.[end_time])        AS Request_execution_time_ms
,    r.[total_elapsed_time]
FROM    sys.dm_pdw_waits w
JOIN    sys.dm_pdw_exec_sessions s  ON w.[session_id] = s.[session_id]
JOIN    sys.dm_pdw_exec_requests r  ON w.[request_id] = r.[request_id]
WHERE    w.[session_id] <> SESSION_ID();
```

<span data-ttu-id="96012-942">Hello `sys.dm_pdw_resource_waits` DMV csak hello erőforrás vár egy adott lekérdezésre által felhasznált jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="96012-942">hello `sys.dm_pdw_resource_waits` DMV shows only hello resource waits consumed by a given query.</span></span> <span data-ttu-id="96012-943">Erőforrás várakozási idő csak a megadott erőforrások toobe Várakozás hello idő méri, megakadályozását toosignal várnia kell, amely hello ideje szerint az SQL kiszolgáló tooschedule hello lekérdezés alakzatot hello CPU alapjául szolgáló hello szükséges.</span><span class="sxs-lookup"><span data-stu-id="96012-943">Resource wait time only measures hello time waiting for resources toobe provided, as opposed toosignal wait time, which is hello time it takes for hello underlying SQL servers tooschedule hello query onto hello CPU.</span></span>

```sql
SELECT  [session_id]
,       [type]
,       [object_type]
,       [object_name]
,       [request_id]
,       [request_time]
,       [acquire_time]
,       DATEDIFF(ms,[request_time],[acquire_time])  AS acquire_duration_ms
,       [concurrency_slots_used]                    AS concurrency_slots_reserved
,       [resource_class]
,       [wait_id]                                   AS queue_position
FROM    sys.dm_pdw_resource_waits
WHERE    [session_id] <> SESSION_ID();
```

<span data-ttu-id="96012-944">Hello `sys.dm_pdw_wait_stats` történelmi trendelemzés vár a DMV is használható.</span><span class="sxs-lookup"><span data-stu-id="96012-944">hello `sys.dm_pdw_wait_stats` DMV can be used for historic trend analysis of waits.</span></span>

```sql
SELECT    w.[pdw_node_id]
,        w.[wait_name]
,        w.[max_wait_time]
,        w.[request_count]
,        w.[signal_time]
,        w.[completed_count]
,        w.[wait_time]
FROM    sys.dm_pdw_wait_stats w;
```

## <a name="next-steps"></a><span data-ttu-id="96012-945">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="96012-945">Next steps</span></span>
<span data-ttu-id="96012-946">Adatbázis-felhasználók és biztonsági kezelésével kapcsolatos további információkért lásd: [az SQL Data Warehouse adatbázis védelme][Secure a database in SQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="96012-946">For more information about managing database users and security, see [Secure a database in SQL Data Warehouse][Secure a database in SQL Data Warehouse].</span></span> <span data-ttu-id="96012-947">További információ a hogyan nagyobb erőforrás osztályok javíthatja a fürtözött oszlopcentrikus index minőségének, lásd: [indexek tooimprove szegmens minőségi újraépítése].</span><span class="sxs-lookup"><span data-stu-id="96012-947">For more information about how larger resource classes can improve clustered columnstore index quality, see [Rebuilding indexes tooimprove segment quality].</span></span>

<!--Image references-->

<!--Article references-->
[Secure a database in SQL Data Warehouse]: ./sql-data-warehouse-overview-manage-security.md
[indexek tooimprove szegmens minőségi újraépítése]: ./sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality
[Secure a database in SQL Data Warehouse]: ./sql-data-warehouse-overview-manage-security.md

<!--MSDN references-->
[Managing Databases and Logins in Azure SQL Database]:https://msdn.microsoft.com/library/azure/ee336235.aspx

<!--Other Web references-->
