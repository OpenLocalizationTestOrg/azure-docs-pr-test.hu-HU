---
title: "Párhuzamossági és munkaterhelés-kezelés az SQL Data Warehouse |} Microsoft Docs"
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
ms.openlocfilehash: eaf2d43286dbaa52ada1430fbb7ce1e37f41c0d4
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="concurrency-and-workload-management-in-sql-data-warehouse"></a><span data-ttu-id="b2c88-103">Párhuzamossági és munkaterhelés-kezelés az SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="b2c88-103">Concurrency and workload management in SQL Data Warehouse</span></span>
<span data-ttu-id="b2c88-104">Kiszámítható teljesítményt biztosítanak a Microsoft Azure SQL Data Warehouse segítséget nyújt a nagyságrendnél párhuzamossági szintek és erőforrás-hozzárendelések például memória és CPU rangsorolási ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="b2c88-104">To deliver predictable performance at scale, Microsoft Azure SQL Data Warehouse helps you control concurrency levels and resource allocations like memory and CPU prioritization.</span></span> <span data-ttu-id="b2c88-105">Ez a cikk bemutatja a feldolgozási és munkaterhelés-kezelés, elmagyarázza, hogyan mindkét szolgáltatásokat nyújt, és hogy miként szabályozható őket az adatraktár elveit.</span><span class="sxs-lookup"><span data-stu-id="b2c88-105">This article introduces you to the concepts of concurrency and workload management, explaining how both features have been implemented and how you can control them in your data warehouse.</span></span> <span data-ttu-id="b2c88-106">SQL Data Warehouse munkaterhelés felügyeleti Többfelhasználós környezetek támogatására segítenek célja.</span><span class="sxs-lookup"><span data-stu-id="b2c88-106">SQL Data Warehouse workload management is intended to help you support multi-user environments.</span></span> <span data-ttu-id="b2c88-107">Nem célja a több-bérlős munkaterhelésekhez.</span><span class="sxs-lookup"><span data-stu-id="b2c88-107">It is not intended for multi-tenant workloads.</span></span>

## <a name="concurrency-limits"></a><span data-ttu-id="b2c88-108">Feldolgozási korlátok</span><span class="sxs-lookup"><span data-stu-id="b2c88-108">Concurrency limits</span></span>
<span data-ttu-id="b2c88-109">Az SQL Data Warehouse lehetővé teszi, hogy legfeljebb 1024 egyidejű kapcsolatok.</span><span class="sxs-lookup"><span data-stu-id="b2c88-109">SQL Data Warehouse allows up to 1,024 concurrent connections.</span></span> <span data-ttu-id="b2c88-110">Az összes 1024 kapcsolat egyidejű lekérdezések küldje el.</span><span class="sxs-lookup"><span data-stu-id="b2c88-110">All 1,024 connections can submit queries concurrently.</span></span> <span data-ttu-id="b2c88-111">Azonban a teljesítmény optimalizálása érdekében az SQL Data Warehouse is várólista néhány lekérdezést biztosítja, hogy minden egyes lekérdezés megkapják-e a minimális memória biztosítása.</span><span class="sxs-lookup"><span data-stu-id="b2c88-111">However, to optimize throughput, SQL Data Warehouse may queue some queries to ensure that each query receives a minimal memory grant.</span></span> <span data-ttu-id="b2c88-112">A lekérdezés-végrehajtás során Queuing következik be.</span><span class="sxs-lookup"><span data-stu-id="b2c88-112">Queuing occurs at query execution time.</span></span> <span data-ttu-id="b2c88-113">Üzenetsor-kezelés lekérdezések feldolgozási korlátok elérésekor, az SQL Data Warehouse növelheti teljes átviteli sebesség győződjön meg arról, hogy aktív lekérdezések kritikusan szükséges memória-erőforrások eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="b2c88-113">By queuing queries when concurrency limits are reached, SQL Data Warehouse can increase total throughput by ensuring that active queries get access to critically needed memory resources.</span></span>  

<span data-ttu-id="b2c88-114">Feldolgozási korlátok vonatkoznak két fogalom: *párhuzamos lekérdezések* és *egyidejűségi üzembe helyezési ponti*.</span><span class="sxs-lookup"><span data-stu-id="b2c88-114">Concurrency limits are governed by two concepts: *concurrent queries* and *concurrency slots*.</span></span> <span data-ttu-id="b2c88-115">A lekérdezés végrehajtásához azt végre kell hajtani a lekérdezés feldolgozási korlát és a párhuzamosság tárolóhely lefoglalása belül.</span><span class="sxs-lookup"><span data-stu-id="b2c88-115">For a query to execute, it must execute within both the query concurrency limit and the concurrency slot allocation.</span></span>

* <span data-ttu-id="b2c88-116">Egyidejű lekérdezések a lekérdezések végrehajtása egy időben.</span><span class="sxs-lookup"><span data-stu-id="b2c88-116">Concurrent queries are the queries executing at the same time.</span></span> <span data-ttu-id="b2c88-117">SQL Data Warehouse legfeljebb 32 egyidejű lekérdezések a DWU nagyobb méretű használatát teszi lehetővé.</span><span class="sxs-lookup"><span data-stu-id="b2c88-117">SQL Data Warehouse supports up to 32 concurrent queries on the larger DWU sizes.</span></span>
* <span data-ttu-id="b2c88-118">Párhuzamossági üzembe helyezési ponti DWU alapján foglal le.</span><span class="sxs-lookup"><span data-stu-id="b2c88-118">Concurrency slots are allocated based on DWU.</span></span> <span data-ttu-id="b2c88-119">Minden 100 DWU 4 párhuzamossági üzembe helyezési ponti biztosít.</span><span class="sxs-lookup"><span data-stu-id="b2c88-119">Each 100 DWU provides 4 concurrency slots.</span></span> <span data-ttu-id="b2c88-120">Például egy DW100 4 párhuzamossági üzembe helyezési ponti foglal le, és DW1000 40 foglal le.</span><span class="sxs-lookup"><span data-stu-id="b2c88-120">For example, a DW100 allocates 4 concurrency slots and DW1000 allocates 40.</span></span> <span data-ttu-id="b2c88-121">Minden egyes lekérdezés használ egy vagy több egyidejű tárhelyek, függ a [erőforrásosztály](#resource-classes) a lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="b2c88-121">Each query consumes one or more concurrency slots, dependent on the [resource class](#resource-classes) of the query.</span></span> <span data-ttu-id="b2c88-122">A smallrc erőforrásosztály futó lekérdezések egy feldolgozási tárolóhely felhasználását.</span><span class="sxs-lookup"><span data-stu-id="b2c88-122">Queries running in the smallrc resource class consume one concurrency slot.</span></span> <span data-ttu-id="b2c88-123">Egy magasabb erőforrásosztály futó lekérdezések több egyidejű üzembe helyezési ponti felhasználását.</span><span class="sxs-lookup"><span data-stu-id="b2c88-123">Queries running in a higher resource class consume more concurrency slots.</span></span>

<span data-ttu-id="b2c88-124">A következő táblázat ismerteti a korlátozhatja a párhuzamos lekérdezések és a feldolgozási tárhelyek a különböző DWU méretben.</span><span class="sxs-lookup"><span data-stu-id="b2c88-124">The following table describes the limits for both concurrent queries and concurrency slots at the various DWU sizes.</span></span>

### <a name="concurrency-limits"></a><span data-ttu-id="b2c88-125">Feldolgozási korlátok</span><span class="sxs-lookup"><span data-stu-id="b2c88-125">Concurrency limits</span></span>
| <span data-ttu-id="b2c88-126">DWU</span><span class="sxs-lookup"><span data-stu-id="b2c88-126">DWU</span></span> | <span data-ttu-id="b2c88-127">Maximális párhuzamos lekérdezések</span><span class="sxs-lookup"><span data-stu-id="b2c88-127">Max concurrent queries</span></span> | <span data-ttu-id="b2c88-128">Lefoglalt párhuzamossági tárhelyek</span><span class="sxs-lookup"><span data-stu-id="b2c88-128">Concurrency slots allocated</span></span> |
|:--- |:---:|:---:|
| <span data-ttu-id="b2c88-129">DW100</span><span class="sxs-lookup"><span data-stu-id="b2c88-129">DW100</span></span> |<span data-ttu-id="b2c88-130">4</span><span class="sxs-lookup"><span data-stu-id="b2c88-130">4</span></span> |<span data-ttu-id="b2c88-131">4</span><span class="sxs-lookup"><span data-stu-id="b2c88-131">4</span></span> |
| <span data-ttu-id="b2c88-132">DW200</span><span class="sxs-lookup"><span data-stu-id="b2c88-132">DW200</span></span> |<span data-ttu-id="b2c88-133">8</span><span class="sxs-lookup"><span data-stu-id="b2c88-133">8</span></span> |<span data-ttu-id="b2c88-134">8</span><span class="sxs-lookup"><span data-stu-id="b2c88-134">8</span></span> |
| <span data-ttu-id="b2c88-135">DW300</span><span class="sxs-lookup"><span data-stu-id="b2c88-135">DW300</span></span> |<span data-ttu-id="b2c88-136">12</span><span class="sxs-lookup"><span data-stu-id="b2c88-136">12</span></span> |<span data-ttu-id="b2c88-137">12</span><span class="sxs-lookup"><span data-stu-id="b2c88-137">12</span></span> |
| <span data-ttu-id="b2c88-138">DW400</span><span class="sxs-lookup"><span data-stu-id="b2c88-138">DW400</span></span> |<span data-ttu-id="b2c88-139">16</span><span class="sxs-lookup"><span data-stu-id="b2c88-139">16</span></span> |<span data-ttu-id="b2c88-140">16</span><span class="sxs-lookup"><span data-stu-id="b2c88-140">16</span></span> |
| <span data-ttu-id="b2c88-141">DW500</span><span class="sxs-lookup"><span data-stu-id="b2c88-141">DW500</span></span> |<span data-ttu-id="b2c88-142">20</span><span class="sxs-lookup"><span data-stu-id="b2c88-142">20</span></span> |<span data-ttu-id="b2c88-143">20</span><span class="sxs-lookup"><span data-stu-id="b2c88-143">20</span></span> |
| <span data-ttu-id="b2c88-144">DW600</span><span class="sxs-lookup"><span data-stu-id="b2c88-144">DW600</span></span> |<span data-ttu-id="b2c88-145">24</span><span class="sxs-lookup"><span data-stu-id="b2c88-145">24</span></span> |<span data-ttu-id="b2c88-146">24</span><span class="sxs-lookup"><span data-stu-id="b2c88-146">24</span></span> |
| <span data-ttu-id="b2c88-147">DW1000</span><span class="sxs-lookup"><span data-stu-id="b2c88-147">DW1000</span></span> |<span data-ttu-id="b2c88-148">32</span><span class="sxs-lookup"><span data-stu-id="b2c88-148">32</span></span> |<span data-ttu-id="b2c88-149">40</span><span class="sxs-lookup"><span data-stu-id="b2c88-149">40</span></span> |
| <span data-ttu-id="b2c88-150">DW1200</span><span class="sxs-lookup"><span data-stu-id="b2c88-150">DW1200</span></span> |<span data-ttu-id="b2c88-151">32</span><span class="sxs-lookup"><span data-stu-id="b2c88-151">32</span></span> |<span data-ttu-id="b2c88-152">48</span><span class="sxs-lookup"><span data-stu-id="b2c88-152">48</span></span> |
| <span data-ttu-id="b2c88-153">DW1500</span><span class="sxs-lookup"><span data-stu-id="b2c88-153">DW1500</span></span> |<span data-ttu-id="b2c88-154">32</span><span class="sxs-lookup"><span data-stu-id="b2c88-154">32</span></span> |<span data-ttu-id="b2c88-155">60</span><span class="sxs-lookup"><span data-stu-id="b2c88-155">60</span></span> |
| <span data-ttu-id="b2c88-156">DW2000</span><span class="sxs-lookup"><span data-stu-id="b2c88-156">DW2000</span></span> |<span data-ttu-id="b2c88-157">32</span><span class="sxs-lookup"><span data-stu-id="b2c88-157">32</span></span> |<span data-ttu-id="b2c88-158">80</span><span class="sxs-lookup"><span data-stu-id="b2c88-158">80</span></span> |
| <span data-ttu-id="b2c88-159">DW3000</span><span class="sxs-lookup"><span data-stu-id="b2c88-159">DW3000</span></span> |<span data-ttu-id="b2c88-160">32</span><span class="sxs-lookup"><span data-stu-id="b2c88-160">32</span></span> |<span data-ttu-id="b2c88-161">120</span><span class="sxs-lookup"><span data-stu-id="b2c88-161">120</span></span> |
| <span data-ttu-id="b2c88-162">DW6000</span><span class="sxs-lookup"><span data-stu-id="b2c88-162">DW6000</span></span> |<span data-ttu-id="b2c88-163">32</span><span class="sxs-lookup"><span data-stu-id="b2c88-163">32</span></span> |<span data-ttu-id="b2c88-164">240</span><span class="sxs-lookup"><span data-stu-id="b2c88-164">240</span></span> |

<span data-ttu-id="b2c88-165">E küszöbértékek valamelyike teljesül, amikor új lekérdezések helyezi várólistára, és végre érkezési sorrendben történő kiküldési alapon.</span><span class="sxs-lookup"><span data-stu-id="b2c88-165">When one of these thresholds is met, new queries are queued and executed on a first-in, first-out basis.</span></span>  <span data-ttu-id="b2c88-166">A lekérdezések befejeződése és a lekérdezések és üzembe helyezési ponti süllyed korlátokat, várólistára helyezett lekérdezések kiadásakor.</span><span class="sxs-lookup"><span data-stu-id="b2c88-166">As a queries finishes and the number of queries and slots falls below the limits, queued queries are released.</span></span> 

> [!NOTE]  
> <span data-ttu-id="b2c88-167">*Válassza ki* lekérdezések végrehajtása kizárólag a dinamikus felügyeleti nézetek (dinamikus felügyeleti nézetek) vagy a katalógus nézetek nem szabályozza a feldolgozási korlátok bármelyikét.</span><span class="sxs-lookup"><span data-stu-id="b2c88-167">*Select* queries executing exclusively on dynamic management views (DMVs) or catalog views are not governed by any of the concurrency limits.</span></span> <span data-ttu-id="b2c88-168">A rendszer függetlenül lekérdezések végrehajtása a figyelheti.</span><span class="sxs-lookup"><span data-stu-id="b2c88-168">You can monitor the system regardless of the number of queries executing on it.</span></span>
> 
> 

## <a name="resource-classes"></a><span data-ttu-id="b2c88-169">Erőforrás-osztályok</span><span class="sxs-lookup"><span data-stu-id="b2c88-169">Resource classes</span></span>
<span data-ttu-id="b2c88-170">Az erőforrásosztályok segítségével szabályozhatja a lekérdezésekhez rendelt memóriakiosztást és CPU-ciklusokat.</span><span class="sxs-lookup"><span data-stu-id="b2c88-170">Resource classes help you control memory allocation and CPU cycles given to a query.</span></span> <span data-ttu-id="b2c88-171">Kétféle típusú erőforrás osztályok rendelhet egy felhasználói adatbázis-szerepkörök formájában.</span><span class="sxs-lookup"><span data-stu-id="b2c88-171">You can assign two types of resource classes to a user in the form of database roles.</span></span> <span data-ttu-id="b2c88-172">A két típusú erőforrás-osztályok a következők:</span><span class="sxs-lookup"><span data-stu-id="b2c88-172">The two types of resource classes are as follows:</span></span>
1. <span data-ttu-id="b2c88-173">Dinamikus erőforrás-osztályok (**smallrc, mediumrc, largerc, xlargerc**) foglaljon le a változó méretű memória, attól függően, hogy az aktuális DWU.</span><span class="sxs-lookup"><span data-stu-id="b2c88-173">Dynamic Resource Classes (**smallrc, mediumrc, largerc, xlargerc**) allocate a variable amount of memory depending on the current DWU.</span></span> <span data-ttu-id="b2c88-174">Ez azt jelenti, hogy ha egy nagyobb adattárházegységre növelheti a lekérdezések automatikusan beolvasása több memóriát.</span><span class="sxs-lookup"><span data-stu-id="b2c88-174">This means that when you scale up to a larger DWU, your queries automatically get more memory.</span></span> 
2. <span data-ttu-id="b2c88-175">Statikus erőforrás-osztály (**staticrc10, staticrc20, staticrc30, staticrc40, staticrc50, staticrc60, staticrc70, staticrc80**) lefoglalni az aktuális DWU függetlenül azonos memóriamennyiség (feltéve, hogy a DWU maga elegendő memória).</span><span class="sxs-lookup"><span data-stu-id="b2c88-175">Static Resource Classes (**staticrc10, staticrc20, staticrc30, staticrc40, staticrc50, staticrc60, staticrc70, staticrc80**) allocate the same amount of memory regardless of the current DWU (provided that the DWU itself has enough memory).</span></span> <span data-ttu-id="b2c88-176">Ez azt jelenti, hogy nagyobb dwu-k, a lekérdezéseket is futtathat további erőforrás az osztályok egyidejűleg.</span><span class="sxs-lookup"><span data-stu-id="b2c88-176">This means that on larger DWUs, you can run more queries in each resource class concurrently.</span></span>

<span data-ttu-id="b2c88-177">A felhasználók **smallrc** és **staticrc10** kap a kisebb méretű memória, és magasabb szintű párhuzamosság előnyeit.</span><span class="sxs-lookup"><span data-stu-id="b2c88-177">Users in **smallrc** and **staticrc10** are given a smaller amount of memory and can take advantage of higher concurrency.</span></span> <span data-ttu-id="b2c88-178">Ezzel szemben rendelt felhasználók **xlargerc** vagy **staticrc80** kap nagy mennyiségű memóriát, és ezért a lekérdezések kevesebb egyidejűleg is futtathatók.</span><span class="sxs-lookup"><span data-stu-id="b2c88-178">In contrast, users assigned to **xlargerc** or **staticrc80** are given large amounts of memory, and therefore fewer of their queries can run concurrently.</span></span>

<span data-ttu-id="b2c88-179">Alapértelmezés szerint minden felhasználó tagja a kis erőforrásosztály **smallrc**.</span><span class="sxs-lookup"><span data-stu-id="b2c88-179">By default, each user is a member of the small resource class, **smallrc**.</span></span> <span data-ttu-id="b2c88-180">Az eljárás `sp_addrolemember` használatával növelheti a erőforrásosztály és `sp_droprolemember` csökkenti a erőforrásosztály szolgál.</span><span class="sxs-lookup"><span data-stu-id="b2c88-180">The procedure `sp_addrolemember` is used to increase the resource class, and `sp_droprolemember` is used to decrease the resource class.</span></span> <span data-ttu-id="b2c88-181">Például a parancs a loaduser erőforrásosztály nőni fog **largerc**:</span><span class="sxs-lookup"><span data-stu-id="b2c88-181">For example, this command would increase the resource class of loaduser to **largerc**:</span></span>

```sql
EXEC sp_addrolemember 'largerc', 'loaduser'
```


### <a name="queries-that-do-not-honor-resource-classes"></a><span data-ttu-id="b2c88-182">Lekérdezések, amelyek nem fogadják el erőforrás-osztályok</span><span class="sxs-lookup"><span data-stu-id="b2c88-182">Queries that do not honor resource classes</span></span>

<span data-ttu-id="b2c88-183">A lekérdezések, amelyek nem tudják igénybe venni egy nagyobb memóriafoglalás néhány típusa van.</span><span class="sxs-lookup"><span data-stu-id="b2c88-183">There are a few types of queries that do not benefit from a larger memory allocation.</span></span> <span data-ttu-id="b2c88-184">A rendszer figyelmen kívül hagyja az erőforrás-osztály terhelése, és mindig futni ezeket a lekérdezéseket a kis erőforrás osztály.</span><span class="sxs-lookup"><span data-stu-id="b2c88-184">The system ignores their resource class allocation and always run these queries in the small resource class instead.</span></span> <span data-ttu-id="b2c88-185">Ha kis erőforrásosztály mindig fut, ezeket a lekérdezéseket, ha párhuzamossági üzembe helyezési ponti nyomás alatt, és nem használja a tárolóhelyek több, mint amennyi szükséges futtathatják.</span><span class="sxs-lookup"><span data-stu-id="b2c88-185">If these queries always run in the small resource class, they can run when concurrency slots are under pressure and they won't consume more slots than needed.</span></span> <span data-ttu-id="b2c88-186">Lásd: [erőforrás osztály kivételek](#query-exceptions-to-concurrency-limits) további információt.</span><span class="sxs-lookup"><span data-stu-id="b2c88-186">See [Resource class exceptions](#query-exceptions-to-concurrency-limits) for more information.</span></span>

## <a name="details-on-resource-class-assignment"></a><span data-ttu-id="b2c88-187">Az erőforrás osztály-hozzárendelés részletei</span><span class="sxs-lookup"><span data-stu-id="b2c88-187">Details on resource class assignment</span></span>


<span data-ttu-id="b2c88-188">Néhány további részleteket a erőforrásosztály:</span><span class="sxs-lookup"><span data-stu-id="b2c88-188">A few more details on resource class:</span></span>

* <span data-ttu-id="b2c88-189">*Az ALTER szerepkör* engedély szükséges egy olyan felhasználó erőforrásosztály módosítása.</span><span class="sxs-lookup"><span data-stu-id="b2c88-189">*Alter role* permission is required to change the resource class of a user.</span></span>
* <span data-ttu-id="b2c88-190">Bár hozzáadhat egy felhasználó egy vagy több, a magasabb szintű erőforrás-osztályok, akkor dinamikus erőforrás-osztályok élveznek statikus erőforrás osztályok.</span><span class="sxs-lookup"><span data-stu-id="b2c88-190">Although you can add a user to one or more of the higher resource classes, dynamic resource classes take precedence over static resource classes.</span></span> <span data-ttu-id="b2c88-191">Ez azt jelenti, hogy ha egy felhasználó mindkét rendelve **mediumrc**(dinamikus) és **staticrc80**(statikus), **mediumrc** az erőforrás-osztály, amely van figyelembe véve.</span><span class="sxs-lookup"><span data-stu-id="b2c88-191">That is, if a user is assigned to both **mediumrc**(dynamic) and **staticrc80**(static), **mediumrc** is the resource class that is honored.</span></span>
 * <span data-ttu-id="b2c88-192">Ha a felhasználó egynél több erőforrás osztály egy adott osztály erőforrástípusra (egynél több dinamikus erőforrásosztály vagy több statikus erőforrásosztály) van rendelve, akkor a legmagasabb erőforrásosztály figyelembe véve.</span><span class="sxs-lookup"><span data-stu-id="b2c88-192">When a user is assigned to more than one resource class in a specific resource class type (more than one dynamic resource class or more than one static resource class), the highest resource class is honored.</span></span> <span data-ttu-id="b2c88-193">Ez azt jelenti, hogy ha egy felhasználó mediumrc és a largerc van rendelve, a magasabb szintű erőforrás-osztály (largerc) van figyelembe véve.</span><span class="sxs-lookup"><span data-stu-id="b2c88-193">That is, if a user is assigned to both mediumrc and largerc, the higher resource class (largerc) is honored.</span></span> <span data-ttu-id="b2c88-194">És ha egy felhasználó mindkét rendelve **staticrc20** és **statirc80**, **staticrc80** kell figyelembe venni.</span><span class="sxs-lookup"><span data-stu-id="b2c88-194">And if a user is assigned to both **staticrc20** and **statirc80**, **staticrc80** is honored.</span></span>
* <span data-ttu-id="b2c88-195">A rendszer rendszergazda felhasználó erőforrásosztály nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="b2c88-195">The resource class of the system administrative user cannot be changed.</span></span>

<span data-ttu-id="b2c88-196">Részletes példákat lásd: [módosul felhasználói erőforrás osztály példa](#changing-user-resource-class-example).</span><span class="sxs-lookup"><span data-stu-id="b2c88-196">For a detailed example, see [Changing user resource class example](#changing-user-resource-class-example).</span></span>

## <a name="memory-allocation"></a><span data-ttu-id="b2c88-197">A memóriafoglalás</span><span class="sxs-lookup"><span data-stu-id="b2c88-197">Memory allocation</span></span>
<span data-ttu-id="b2c88-198">Vannak előnyei és hátrányai a növekvő számú egy felhasználó erőforrásosztály.</span><span class="sxs-lookup"><span data-stu-id="b2c88-198">There are pros and cons to increasing a user's resource class.</span></span> <span data-ttu-id="b2c88-199">Egy felhasználó egy erőforrásosztály növekszik, a lekérdezések hozzáférést ad további memória, amely azt jelentheti, lekérdezések gyorsabban hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="b2c88-199">Increasing a resource class for a user, gives their queries access to more memory, which may mean queries execute faster.</span></span>  <span data-ttu-id="b2c88-200">Azonban a magasabb erőforrás osztályok is csökkentheti a futtatható egyidejű lekérdezések száma.</span><span class="sxs-lookup"><span data-stu-id="b2c88-200">However, higher resource classes also reduce the number of concurrent queries that can run.</span></span> <span data-ttu-id="b2c88-201">Ez egy nagy mennyiségű egyetlen lekérdezést memória lefoglalásakor vagy más lekérdezések, memória-hozzárendelések egyidejű futtatását is igénylő így között kompromisszum.</span><span class="sxs-lookup"><span data-stu-id="b2c88-201">This is the trade-off between allocating large amounts of memory to a single query or allowing other queries, which also need memory allocations, to run concurrently.</span></span> <span data-ttu-id="b2c88-202">Ha egy felhasználó kap a lekérdezés számára magas foglalásokat, más felhasználók nem hozzáférhetnek, hogy ugyanazt a memória-lekérdezés futtatható.</span><span class="sxs-lookup"><span data-stu-id="b2c88-202">If one user is given high allocations of memory for a query, other users will not have access to that same memory to run a query.</span></span>

<span data-ttu-id="b2c88-203">A következő táblázat a DWU- és erőforrás-osztály minden egyes terjesztési kiosztott memória képezi le.</span><span class="sxs-lookup"><span data-stu-id="b2c88-203">The following table maps the memory allocated to each distribution by DWU and resource class.</span></span>

### <a name="memory-allocations-per-distribution-for-dynamic-resource-classes-mb"></a><span data-ttu-id="b2c88-204">Memória kiosztásokat eloszlása dinamikus erőforrás osztályok (MB)</span><span class="sxs-lookup"><span data-stu-id="b2c88-204">Memory allocations per distribution for dynamic resource classes (MB)</span></span>
| <span data-ttu-id="b2c88-205">DWU</span><span class="sxs-lookup"><span data-stu-id="b2c88-205">DWU</span></span> | <span data-ttu-id="b2c88-206">smallrc</span><span class="sxs-lookup"><span data-stu-id="b2c88-206">smallrc</span></span> | <span data-ttu-id="b2c88-207">mediumrc</span><span class="sxs-lookup"><span data-stu-id="b2c88-207">mediumrc</span></span> | <span data-ttu-id="b2c88-208">largerc</span><span class="sxs-lookup"><span data-stu-id="b2c88-208">largerc</span></span> | <span data-ttu-id="b2c88-209">xlargerc</span><span class="sxs-lookup"><span data-stu-id="b2c88-209">xlargerc</span></span> |
|:--- |:---:|:---:|:---:|:---:|
| <span data-ttu-id="b2c88-210">DW100</span><span class="sxs-lookup"><span data-stu-id="b2c88-210">DW100</span></span> |<span data-ttu-id="b2c88-211">100</span><span class="sxs-lookup"><span data-stu-id="b2c88-211">100</span></span> |<span data-ttu-id="b2c88-212">100</span><span class="sxs-lookup"><span data-stu-id="b2c88-212">100</span></span> |<span data-ttu-id="b2c88-213">200</span><span class="sxs-lookup"><span data-stu-id="b2c88-213">200</span></span> |<span data-ttu-id="b2c88-214">400</span><span class="sxs-lookup"><span data-stu-id="b2c88-214">400</span></span> |
| <span data-ttu-id="b2c88-215">DW200</span><span class="sxs-lookup"><span data-stu-id="b2c88-215">DW200</span></span> |<span data-ttu-id="b2c88-216">100</span><span class="sxs-lookup"><span data-stu-id="b2c88-216">100</span></span> |<span data-ttu-id="b2c88-217">200</span><span class="sxs-lookup"><span data-stu-id="b2c88-217">200</span></span> |<span data-ttu-id="b2c88-218">400</span><span class="sxs-lookup"><span data-stu-id="b2c88-218">400</span></span> |<span data-ttu-id="b2c88-219">800</span><span class="sxs-lookup"><span data-stu-id="b2c88-219">800</span></span> |
| <span data-ttu-id="b2c88-220">DW300</span><span class="sxs-lookup"><span data-stu-id="b2c88-220">DW300</span></span> |<span data-ttu-id="b2c88-221">100</span><span class="sxs-lookup"><span data-stu-id="b2c88-221">100</span></span> |<span data-ttu-id="b2c88-222">200</span><span class="sxs-lookup"><span data-stu-id="b2c88-222">200</span></span> |<span data-ttu-id="b2c88-223">400</span><span class="sxs-lookup"><span data-stu-id="b2c88-223">400</span></span> |<span data-ttu-id="b2c88-224">800</span><span class="sxs-lookup"><span data-stu-id="b2c88-224">800</span></span> |
| <span data-ttu-id="b2c88-225">DW400</span><span class="sxs-lookup"><span data-stu-id="b2c88-225">DW400</span></span> |<span data-ttu-id="b2c88-226">100</span><span class="sxs-lookup"><span data-stu-id="b2c88-226">100</span></span> |<span data-ttu-id="b2c88-227">400</span><span class="sxs-lookup"><span data-stu-id="b2c88-227">400</span></span> |<span data-ttu-id="b2c88-228">800</span><span class="sxs-lookup"><span data-stu-id="b2c88-228">800</span></span> |<span data-ttu-id="b2c88-229">1,600</span><span class="sxs-lookup"><span data-stu-id="b2c88-229">1,600</span></span> |
| <span data-ttu-id="b2c88-230">DW500</span><span class="sxs-lookup"><span data-stu-id="b2c88-230">DW500</span></span> |<span data-ttu-id="b2c88-231">100</span><span class="sxs-lookup"><span data-stu-id="b2c88-231">100</span></span> |<span data-ttu-id="b2c88-232">400</span><span class="sxs-lookup"><span data-stu-id="b2c88-232">400</span></span> |<span data-ttu-id="b2c88-233">800</span><span class="sxs-lookup"><span data-stu-id="b2c88-233">800</span></span> |<span data-ttu-id="b2c88-234">1,600</span><span class="sxs-lookup"><span data-stu-id="b2c88-234">1,600</span></span> |
| <span data-ttu-id="b2c88-235">DW600</span><span class="sxs-lookup"><span data-stu-id="b2c88-235">DW600</span></span> |<span data-ttu-id="b2c88-236">100</span><span class="sxs-lookup"><span data-stu-id="b2c88-236">100</span></span> |<span data-ttu-id="b2c88-237">400</span><span class="sxs-lookup"><span data-stu-id="b2c88-237">400</span></span> |<span data-ttu-id="b2c88-238">800</span><span class="sxs-lookup"><span data-stu-id="b2c88-238">800</span></span> |<span data-ttu-id="b2c88-239">1,600</span><span class="sxs-lookup"><span data-stu-id="b2c88-239">1,600</span></span> |
| <span data-ttu-id="b2c88-240">DW1000</span><span class="sxs-lookup"><span data-stu-id="b2c88-240">DW1000</span></span> |<span data-ttu-id="b2c88-241">100</span><span class="sxs-lookup"><span data-stu-id="b2c88-241">100</span></span> |<span data-ttu-id="b2c88-242">800</span><span class="sxs-lookup"><span data-stu-id="b2c88-242">800</span></span> |<span data-ttu-id="b2c88-243">1,600</span><span class="sxs-lookup"><span data-stu-id="b2c88-243">1,600</span></span> |<span data-ttu-id="b2c88-244">3,200</span><span class="sxs-lookup"><span data-stu-id="b2c88-244">3,200</span></span> |
| <span data-ttu-id="b2c88-245">DW1200</span><span class="sxs-lookup"><span data-stu-id="b2c88-245">DW1200</span></span> |<span data-ttu-id="b2c88-246">100</span><span class="sxs-lookup"><span data-stu-id="b2c88-246">100</span></span> |<span data-ttu-id="b2c88-247">800</span><span class="sxs-lookup"><span data-stu-id="b2c88-247">800</span></span> |<span data-ttu-id="b2c88-248">1,600</span><span class="sxs-lookup"><span data-stu-id="b2c88-248">1,600</span></span> |<span data-ttu-id="b2c88-249">3,200</span><span class="sxs-lookup"><span data-stu-id="b2c88-249">3,200</span></span> |
| <span data-ttu-id="b2c88-250">DW1500</span><span class="sxs-lookup"><span data-stu-id="b2c88-250">DW1500</span></span> |<span data-ttu-id="b2c88-251">100</span><span class="sxs-lookup"><span data-stu-id="b2c88-251">100</span></span> |<span data-ttu-id="b2c88-252">800</span><span class="sxs-lookup"><span data-stu-id="b2c88-252">800</span></span> |<span data-ttu-id="b2c88-253">1,600</span><span class="sxs-lookup"><span data-stu-id="b2c88-253">1,600</span></span> |<span data-ttu-id="b2c88-254">3,200</span><span class="sxs-lookup"><span data-stu-id="b2c88-254">3,200</span></span> |
| <span data-ttu-id="b2c88-255">DW2000</span><span class="sxs-lookup"><span data-stu-id="b2c88-255">DW2000</span></span> |<span data-ttu-id="b2c88-256">100</span><span class="sxs-lookup"><span data-stu-id="b2c88-256">100</span></span> |<span data-ttu-id="b2c88-257">1,600</span><span class="sxs-lookup"><span data-stu-id="b2c88-257">1,600</span></span> |<span data-ttu-id="b2c88-258">3,200</span><span class="sxs-lookup"><span data-stu-id="b2c88-258">3,200</span></span> |<span data-ttu-id="b2c88-259">6,400</span><span class="sxs-lookup"><span data-stu-id="b2c88-259">6,400</span></span> |
| <span data-ttu-id="b2c88-260">DW3000</span><span class="sxs-lookup"><span data-stu-id="b2c88-260">DW3000</span></span> |<span data-ttu-id="b2c88-261">100</span><span class="sxs-lookup"><span data-stu-id="b2c88-261">100</span></span> |<span data-ttu-id="b2c88-262">1,600</span><span class="sxs-lookup"><span data-stu-id="b2c88-262">1,600</span></span> |<span data-ttu-id="b2c88-263">3,200</span><span class="sxs-lookup"><span data-stu-id="b2c88-263">3,200</span></span> |<span data-ttu-id="b2c88-264">6,400</span><span class="sxs-lookup"><span data-stu-id="b2c88-264">6,400</span></span> |
| <span data-ttu-id="b2c88-265">DW6000</span><span class="sxs-lookup"><span data-stu-id="b2c88-265">DW6000</span></span> |<span data-ttu-id="b2c88-266">100</span><span class="sxs-lookup"><span data-stu-id="b2c88-266">100</span></span> |<span data-ttu-id="b2c88-267">3,200</span><span class="sxs-lookup"><span data-stu-id="b2c88-267">3,200</span></span> |<span data-ttu-id="b2c88-268">6,400</span><span class="sxs-lookup"><span data-stu-id="b2c88-268">6,400</span></span> |<span data-ttu-id="b2c88-269">12,800</span><span class="sxs-lookup"><span data-stu-id="b2c88-269">12,800</span></span> |

<span data-ttu-id="b2c88-270">A következő táblázat minden egyes terjesztési DWU és statikus erőforrásosztály által kiosztott memória képezi le.</span><span class="sxs-lookup"><span data-stu-id="b2c88-270">The following table maps the memory allocated to each distribution by DWU and static resource class.</span></span> <span data-ttu-id="b2c88-271">Ne feledje, hogy a magasabb szintű erőforrás-osztályok a memória, a globális DWU korlátok tiszteletben csökken.</span><span class="sxs-lookup"><span data-stu-id="b2c88-271">Note that the higher resource classes have their memory reduced to honor the global DWU limits.</span></span>

### <a name="memory-allocations-per-distribution-for-static-resource-classes-mb"></a><span data-ttu-id="b2c88-272">Memória kiosztásokat eloszlása statikus erőforrás osztályok (MB)</span><span class="sxs-lookup"><span data-stu-id="b2c88-272">Memory allocations per distribution for static resource classes (MB)</span></span>
| <span data-ttu-id="b2c88-273">DWU</span><span class="sxs-lookup"><span data-stu-id="b2c88-273">DWU</span></span> | <span data-ttu-id="b2c88-274">staticrc10</span><span class="sxs-lookup"><span data-stu-id="b2c88-274">staticrc10</span></span> | <span data-ttu-id="b2c88-275">staticrc20</span><span class="sxs-lookup"><span data-stu-id="b2c88-275">staticrc20</span></span> | <span data-ttu-id="b2c88-276">staticrc30</span><span class="sxs-lookup"><span data-stu-id="b2c88-276">staticrc30</span></span> | <span data-ttu-id="b2c88-277">staticrc40</span><span class="sxs-lookup"><span data-stu-id="b2c88-277">staticrc40</span></span> | <span data-ttu-id="b2c88-278">staticrc50</span><span class="sxs-lookup"><span data-stu-id="b2c88-278">staticrc50</span></span> | <span data-ttu-id="b2c88-279">staticrc60</span><span class="sxs-lookup"><span data-stu-id="b2c88-279">staticrc60</span></span> | <span data-ttu-id="b2c88-280">staticrc70</span><span class="sxs-lookup"><span data-stu-id="b2c88-280">staticrc70</span></span> | <span data-ttu-id="b2c88-281">staticrc80</span><span class="sxs-lookup"><span data-stu-id="b2c88-281">staticrc80</span></span> |
|:--- |:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| <span data-ttu-id="b2c88-282">DW100</span><span class="sxs-lookup"><span data-stu-id="b2c88-282">DW100</span></span> |<span data-ttu-id="b2c88-283">100</span><span class="sxs-lookup"><span data-stu-id="b2c88-283">100</span></span> |<span data-ttu-id="b2c88-284">200</span><span class="sxs-lookup"><span data-stu-id="b2c88-284">200</span></span> |<span data-ttu-id="b2c88-285">400</span><span class="sxs-lookup"><span data-stu-id="b2c88-285">400</span></span> |<span data-ttu-id="b2c88-286">400</span><span class="sxs-lookup"><span data-stu-id="b2c88-286">400</span></span> |<span data-ttu-id="b2c88-287">400</span><span class="sxs-lookup"><span data-stu-id="b2c88-287">400</span></span> |<span data-ttu-id="b2c88-288">400</span><span class="sxs-lookup"><span data-stu-id="b2c88-288">400</span></span> |<span data-ttu-id="b2c88-289">400</span><span class="sxs-lookup"><span data-stu-id="b2c88-289">400</span></span> |<span data-ttu-id="b2c88-290">400</span><span class="sxs-lookup"><span data-stu-id="b2c88-290">400</span></span> |
| <span data-ttu-id="b2c88-291">DW200</span><span class="sxs-lookup"><span data-stu-id="b2c88-291">DW200</span></span> |<span data-ttu-id="b2c88-292">100</span><span class="sxs-lookup"><span data-stu-id="b2c88-292">100</span></span> |<span data-ttu-id="b2c88-293">200</span><span class="sxs-lookup"><span data-stu-id="b2c88-293">200</span></span> |<span data-ttu-id="b2c88-294">400</span><span class="sxs-lookup"><span data-stu-id="b2c88-294">400</span></span> |<span data-ttu-id="b2c88-295">800</span><span class="sxs-lookup"><span data-stu-id="b2c88-295">800</span></span> |<span data-ttu-id="b2c88-296">800</span><span class="sxs-lookup"><span data-stu-id="b2c88-296">800</span></span> |<span data-ttu-id="b2c88-297">800</span><span class="sxs-lookup"><span data-stu-id="b2c88-297">800</span></span> |<span data-ttu-id="b2c88-298">800</span><span class="sxs-lookup"><span data-stu-id="b2c88-298">800</span></span> |<span data-ttu-id="b2c88-299">800</span><span class="sxs-lookup"><span data-stu-id="b2c88-299">800</span></span> |
| <span data-ttu-id="b2c88-300">DW300</span><span class="sxs-lookup"><span data-stu-id="b2c88-300">DW300</span></span> |<span data-ttu-id="b2c88-301">100</span><span class="sxs-lookup"><span data-stu-id="b2c88-301">100</span></span> |<span data-ttu-id="b2c88-302">200</span><span class="sxs-lookup"><span data-stu-id="b2c88-302">200</span></span> |<span data-ttu-id="b2c88-303">400</span><span class="sxs-lookup"><span data-stu-id="b2c88-303">400</span></span> |<span data-ttu-id="b2c88-304">800</span><span class="sxs-lookup"><span data-stu-id="b2c88-304">800</span></span> |<span data-ttu-id="b2c88-305">800</span><span class="sxs-lookup"><span data-stu-id="b2c88-305">800</span></span> |<span data-ttu-id="b2c88-306">800</span><span class="sxs-lookup"><span data-stu-id="b2c88-306">800</span></span> |<span data-ttu-id="b2c88-307">800</span><span class="sxs-lookup"><span data-stu-id="b2c88-307">800</span></span> |<span data-ttu-id="b2c88-308">800</span><span class="sxs-lookup"><span data-stu-id="b2c88-308">800</span></span> |
| <span data-ttu-id="b2c88-309">DW400</span><span class="sxs-lookup"><span data-stu-id="b2c88-309">DW400</span></span> |<span data-ttu-id="b2c88-310">100</span><span class="sxs-lookup"><span data-stu-id="b2c88-310">100</span></span> |<span data-ttu-id="b2c88-311">200</span><span class="sxs-lookup"><span data-stu-id="b2c88-311">200</span></span> |<span data-ttu-id="b2c88-312">400</span><span class="sxs-lookup"><span data-stu-id="b2c88-312">400</span></span> |<span data-ttu-id="b2c88-313">800</span><span class="sxs-lookup"><span data-stu-id="b2c88-313">800</span></span> |<span data-ttu-id="b2c88-314">1,600</span><span class="sxs-lookup"><span data-stu-id="b2c88-314">1,600</span></span> |<span data-ttu-id="b2c88-315">1,600</span><span class="sxs-lookup"><span data-stu-id="b2c88-315">1,600</span></span> |<span data-ttu-id="b2c88-316">1,600</span><span class="sxs-lookup"><span data-stu-id="b2c88-316">1,600</span></span> |<span data-ttu-id="b2c88-317">1,600</span><span class="sxs-lookup"><span data-stu-id="b2c88-317">1,600</span></span> |
| <span data-ttu-id="b2c88-318">DW500</span><span class="sxs-lookup"><span data-stu-id="b2c88-318">DW500</span></span> |<span data-ttu-id="b2c88-319">100</span><span class="sxs-lookup"><span data-stu-id="b2c88-319">100</span></span> |<span data-ttu-id="b2c88-320">200</span><span class="sxs-lookup"><span data-stu-id="b2c88-320">200</span></span> |<span data-ttu-id="b2c88-321">400</span><span class="sxs-lookup"><span data-stu-id="b2c88-321">400</span></span> |<span data-ttu-id="b2c88-322">800</span><span class="sxs-lookup"><span data-stu-id="b2c88-322">800</span></span> |<span data-ttu-id="b2c88-323">1,600</span><span class="sxs-lookup"><span data-stu-id="b2c88-323">1,600</span></span> |<span data-ttu-id="b2c88-324">1,600</span><span class="sxs-lookup"><span data-stu-id="b2c88-324">1,600</span></span> |<span data-ttu-id="b2c88-325">1,600</span><span class="sxs-lookup"><span data-stu-id="b2c88-325">1,600</span></span> |<span data-ttu-id="b2c88-326">1,600</span><span class="sxs-lookup"><span data-stu-id="b2c88-326">1,600</span></span> |
| <span data-ttu-id="b2c88-327">DW600</span><span class="sxs-lookup"><span data-stu-id="b2c88-327">DW600</span></span> |<span data-ttu-id="b2c88-328">100</span><span class="sxs-lookup"><span data-stu-id="b2c88-328">100</span></span> |<span data-ttu-id="b2c88-329">200</span><span class="sxs-lookup"><span data-stu-id="b2c88-329">200</span></span> |<span data-ttu-id="b2c88-330">400</span><span class="sxs-lookup"><span data-stu-id="b2c88-330">400</span></span> |<span data-ttu-id="b2c88-331">800</span><span class="sxs-lookup"><span data-stu-id="b2c88-331">800</span></span> |<span data-ttu-id="b2c88-332">1,600</span><span class="sxs-lookup"><span data-stu-id="b2c88-332">1,600</span></span> |<span data-ttu-id="b2c88-333">1,600</span><span class="sxs-lookup"><span data-stu-id="b2c88-333">1,600</span></span> |<span data-ttu-id="b2c88-334">1,600</span><span class="sxs-lookup"><span data-stu-id="b2c88-334">1,600</span></span> |<span data-ttu-id="b2c88-335">1,600</span><span class="sxs-lookup"><span data-stu-id="b2c88-335">1,600</span></span> |
| <span data-ttu-id="b2c88-336">DW1000</span><span class="sxs-lookup"><span data-stu-id="b2c88-336">DW1000</span></span> |<span data-ttu-id="b2c88-337">100</span><span class="sxs-lookup"><span data-stu-id="b2c88-337">100</span></span> |<span data-ttu-id="b2c88-338">200</span><span class="sxs-lookup"><span data-stu-id="b2c88-338">200</span></span> |<span data-ttu-id="b2c88-339">400</span><span class="sxs-lookup"><span data-stu-id="b2c88-339">400</span></span> |<span data-ttu-id="b2c88-340">800</span><span class="sxs-lookup"><span data-stu-id="b2c88-340">800</span></span> |<span data-ttu-id="b2c88-341">1,600</span><span class="sxs-lookup"><span data-stu-id="b2c88-341">1,600</span></span> |<span data-ttu-id="b2c88-342">3,200</span><span class="sxs-lookup"><span data-stu-id="b2c88-342">3,200</span></span> |<span data-ttu-id="b2c88-343">3,200</span><span class="sxs-lookup"><span data-stu-id="b2c88-343">3,200</span></span> |<span data-ttu-id="b2c88-344">3,200</span><span class="sxs-lookup"><span data-stu-id="b2c88-344">3,200</span></span> |
| <span data-ttu-id="b2c88-345">DW1200</span><span class="sxs-lookup"><span data-stu-id="b2c88-345">DW1200</span></span> |<span data-ttu-id="b2c88-346">100</span><span class="sxs-lookup"><span data-stu-id="b2c88-346">100</span></span> |<span data-ttu-id="b2c88-347">200</span><span class="sxs-lookup"><span data-stu-id="b2c88-347">200</span></span> |<span data-ttu-id="b2c88-348">400</span><span class="sxs-lookup"><span data-stu-id="b2c88-348">400</span></span> |<span data-ttu-id="b2c88-349">800</span><span class="sxs-lookup"><span data-stu-id="b2c88-349">800</span></span> |<span data-ttu-id="b2c88-350">1,600</span><span class="sxs-lookup"><span data-stu-id="b2c88-350">1,600</span></span> |<span data-ttu-id="b2c88-351">3,200</span><span class="sxs-lookup"><span data-stu-id="b2c88-351">3,200</span></span> |<span data-ttu-id="b2c88-352">3,200</span><span class="sxs-lookup"><span data-stu-id="b2c88-352">3,200</span></span> |<span data-ttu-id="b2c88-353">3,200</span><span class="sxs-lookup"><span data-stu-id="b2c88-353">3,200</span></span> |
| <span data-ttu-id="b2c88-354">DW1500</span><span class="sxs-lookup"><span data-stu-id="b2c88-354">DW1500</span></span> |<span data-ttu-id="b2c88-355">100</span><span class="sxs-lookup"><span data-stu-id="b2c88-355">100</span></span> |<span data-ttu-id="b2c88-356">200</span><span class="sxs-lookup"><span data-stu-id="b2c88-356">200</span></span> |<span data-ttu-id="b2c88-357">400</span><span class="sxs-lookup"><span data-stu-id="b2c88-357">400</span></span> |<span data-ttu-id="b2c88-358">800</span><span class="sxs-lookup"><span data-stu-id="b2c88-358">800</span></span> |<span data-ttu-id="b2c88-359">1,600</span><span class="sxs-lookup"><span data-stu-id="b2c88-359">1,600</span></span> |<span data-ttu-id="b2c88-360">3,200</span><span class="sxs-lookup"><span data-stu-id="b2c88-360">3,200</span></span> |<span data-ttu-id="b2c88-361">3,200</span><span class="sxs-lookup"><span data-stu-id="b2c88-361">3,200</span></span> |<span data-ttu-id="b2c88-362">3,200</span><span class="sxs-lookup"><span data-stu-id="b2c88-362">3,200</span></span> |
| <span data-ttu-id="b2c88-363">DW2000</span><span class="sxs-lookup"><span data-stu-id="b2c88-363">DW2000</span></span> |<span data-ttu-id="b2c88-364">100</span><span class="sxs-lookup"><span data-stu-id="b2c88-364">100</span></span> |<span data-ttu-id="b2c88-365">200</span><span class="sxs-lookup"><span data-stu-id="b2c88-365">200</span></span> |<span data-ttu-id="b2c88-366">400</span><span class="sxs-lookup"><span data-stu-id="b2c88-366">400</span></span> |<span data-ttu-id="b2c88-367">800</span><span class="sxs-lookup"><span data-stu-id="b2c88-367">800</span></span> |<span data-ttu-id="b2c88-368">1,600</span><span class="sxs-lookup"><span data-stu-id="b2c88-368">1,600</span></span> |<span data-ttu-id="b2c88-369">3,200</span><span class="sxs-lookup"><span data-stu-id="b2c88-369">3,200</span></span> |<span data-ttu-id="b2c88-370">6,400</span><span class="sxs-lookup"><span data-stu-id="b2c88-370">6,400</span></span> |<span data-ttu-id="b2c88-371">6,400</span><span class="sxs-lookup"><span data-stu-id="b2c88-371">6,400</span></span> |
| <span data-ttu-id="b2c88-372">DW3000</span><span class="sxs-lookup"><span data-stu-id="b2c88-372">DW3000</span></span> |<span data-ttu-id="b2c88-373">100</span><span class="sxs-lookup"><span data-stu-id="b2c88-373">100</span></span> |<span data-ttu-id="b2c88-374">200</span><span class="sxs-lookup"><span data-stu-id="b2c88-374">200</span></span> |<span data-ttu-id="b2c88-375">400</span><span class="sxs-lookup"><span data-stu-id="b2c88-375">400</span></span> |<span data-ttu-id="b2c88-376">800</span><span class="sxs-lookup"><span data-stu-id="b2c88-376">800</span></span> |<span data-ttu-id="b2c88-377">1,600</span><span class="sxs-lookup"><span data-stu-id="b2c88-377">1,600</span></span> |<span data-ttu-id="b2c88-378">3,200</span><span class="sxs-lookup"><span data-stu-id="b2c88-378">3,200</span></span> |<span data-ttu-id="b2c88-379">6,400</span><span class="sxs-lookup"><span data-stu-id="b2c88-379">6,400</span></span> |<span data-ttu-id="b2c88-380">6,400</span><span class="sxs-lookup"><span data-stu-id="b2c88-380">6,400</span></span> |
| <span data-ttu-id="b2c88-381">DW6000</span><span class="sxs-lookup"><span data-stu-id="b2c88-381">DW6000</span></span> |<span data-ttu-id="b2c88-382">100</span><span class="sxs-lookup"><span data-stu-id="b2c88-382">100</span></span> |<span data-ttu-id="b2c88-383">200</span><span class="sxs-lookup"><span data-stu-id="b2c88-383">200</span></span> |<span data-ttu-id="b2c88-384">400</span><span class="sxs-lookup"><span data-stu-id="b2c88-384">400</span></span> |<span data-ttu-id="b2c88-385">800</span><span class="sxs-lookup"><span data-stu-id="b2c88-385">800</span></span> |<span data-ttu-id="b2c88-386">1,600</span><span class="sxs-lookup"><span data-stu-id="b2c88-386">1,600</span></span> |<span data-ttu-id="b2c88-387">3,200</span><span class="sxs-lookup"><span data-stu-id="b2c88-387">3,200</span></span> |<span data-ttu-id="b2c88-388">6,400</span><span class="sxs-lookup"><span data-stu-id="b2c88-388">6,400</span></span> |<span data-ttu-id="b2c88-389">12,800</span><span class="sxs-lookup"><span data-stu-id="b2c88-389">12,800</span></span> |

<span data-ttu-id="b2c88-390">Az előző táblázatban láthatja, hogy fut egy DW2000 a lekérdezés a **xlargerc** erőforrásosztály kellene 6400 MB memória belül 60 elosztott adatbázis elérésére.</span><span class="sxs-lookup"><span data-stu-id="b2c88-390">From the preceding table, you can see that a query running on a DW2000 in the **xlargerc** resource class would have access to 6,400 MB of memory within each of the 60 distributed databases.</span></span>  <span data-ttu-id="b2c88-391">Az SQL Data Warehouse nincsenek 60 terjesztéseket.</span><span class="sxs-lookup"><span data-stu-id="b2c88-391">In SQL Data Warehouse, there are 60 distributions.</span></span> <span data-ttu-id="b2c88-392">Ezért a teljes memória-foglalás a megadott erőforrásosztály lekérdezés kiszámításához, a fenti értékeket be kell szorozni 60.</span><span class="sxs-lookup"><span data-stu-id="b2c88-392">Therefore, to calculate the total memory allocation for a query in a given resource class, the above values should be multiplied by 60.</span></span>

### <a name="memory-allocations-system-wide-gb"></a><span data-ttu-id="b2c88-393">Memória kiosztásokat rendszerszintű (GB)</span><span class="sxs-lookup"><span data-stu-id="b2c88-393">Memory allocations system-wide (GB)</span></span>
| <span data-ttu-id="b2c88-394">DWU</span><span class="sxs-lookup"><span data-stu-id="b2c88-394">DWU</span></span> | <span data-ttu-id="b2c88-395">smallrc</span><span class="sxs-lookup"><span data-stu-id="b2c88-395">smallrc</span></span> | <span data-ttu-id="b2c88-396">mediumrc</span><span class="sxs-lookup"><span data-stu-id="b2c88-396">mediumrc</span></span> | <span data-ttu-id="b2c88-397">largerc</span><span class="sxs-lookup"><span data-stu-id="b2c88-397">largerc</span></span> | <span data-ttu-id="b2c88-398">xlargerc</span><span class="sxs-lookup"><span data-stu-id="b2c88-398">xlargerc</span></span> |
|:--- |:---:|:---:|:---:|:---:|
| <span data-ttu-id="b2c88-399">DW100</span><span class="sxs-lookup"><span data-stu-id="b2c88-399">DW100</span></span> |<span data-ttu-id="b2c88-400">6</span><span class="sxs-lookup"><span data-stu-id="b2c88-400">6</span></span> |<span data-ttu-id="b2c88-401">6</span><span class="sxs-lookup"><span data-stu-id="b2c88-401">6</span></span> |<span data-ttu-id="b2c88-402">12</span><span class="sxs-lookup"><span data-stu-id="b2c88-402">12</span></span> |<span data-ttu-id="b2c88-403">23</span><span class="sxs-lookup"><span data-stu-id="b2c88-403">23</span></span> |
| <span data-ttu-id="b2c88-404">DW200</span><span class="sxs-lookup"><span data-stu-id="b2c88-404">DW200</span></span> |<span data-ttu-id="b2c88-405">6</span><span class="sxs-lookup"><span data-stu-id="b2c88-405">6</span></span> |<span data-ttu-id="b2c88-406">12</span><span class="sxs-lookup"><span data-stu-id="b2c88-406">12</span></span> |<span data-ttu-id="b2c88-407">23</span><span class="sxs-lookup"><span data-stu-id="b2c88-407">23</span></span> |<span data-ttu-id="b2c88-408">47</span><span class="sxs-lookup"><span data-stu-id="b2c88-408">47</span></span> |
| <span data-ttu-id="b2c88-409">DW300</span><span class="sxs-lookup"><span data-stu-id="b2c88-409">DW300</span></span> |<span data-ttu-id="b2c88-410">6</span><span class="sxs-lookup"><span data-stu-id="b2c88-410">6</span></span> |<span data-ttu-id="b2c88-411">12</span><span class="sxs-lookup"><span data-stu-id="b2c88-411">12</span></span> |<span data-ttu-id="b2c88-412">23</span><span class="sxs-lookup"><span data-stu-id="b2c88-412">23</span></span> |<span data-ttu-id="b2c88-413">47</span><span class="sxs-lookup"><span data-stu-id="b2c88-413">47</span></span> |
| <span data-ttu-id="b2c88-414">DW400</span><span class="sxs-lookup"><span data-stu-id="b2c88-414">DW400</span></span> |<span data-ttu-id="b2c88-415">6</span><span class="sxs-lookup"><span data-stu-id="b2c88-415">6</span></span> |<span data-ttu-id="b2c88-416">23</span><span class="sxs-lookup"><span data-stu-id="b2c88-416">23</span></span> |<span data-ttu-id="b2c88-417">47</span><span class="sxs-lookup"><span data-stu-id="b2c88-417">47</span></span> |<span data-ttu-id="b2c88-418">94</span><span class="sxs-lookup"><span data-stu-id="b2c88-418">94</span></span> |
| <span data-ttu-id="b2c88-419">DW500</span><span class="sxs-lookup"><span data-stu-id="b2c88-419">DW500</span></span> |<span data-ttu-id="b2c88-420">6</span><span class="sxs-lookup"><span data-stu-id="b2c88-420">6</span></span> |<span data-ttu-id="b2c88-421">23</span><span class="sxs-lookup"><span data-stu-id="b2c88-421">23</span></span> |<span data-ttu-id="b2c88-422">47</span><span class="sxs-lookup"><span data-stu-id="b2c88-422">47</span></span> |<span data-ttu-id="b2c88-423">94</span><span class="sxs-lookup"><span data-stu-id="b2c88-423">94</span></span> |
| <span data-ttu-id="b2c88-424">DW600</span><span class="sxs-lookup"><span data-stu-id="b2c88-424">DW600</span></span> |<span data-ttu-id="b2c88-425">6</span><span class="sxs-lookup"><span data-stu-id="b2c88-425">6</span></span> |<span data-ttu-id="b2c88-426">23</span><span class="sxs-lookup"><span data-stu-id="b2c88-426">23</span></span> |<span data-ttu-id="b2c88-427">47</span><span class="sxs-lookup"><span data-stu-id="b2c88-427">47</span></span> |<span data-ttu-id="b2c88-428">94</span><span class="sxs-lookup"><span data-stu-id="b2c88-428">94</span></span> |
| <span data-ttu-id="b2c88-429">DW1000</span><span class="sxs-lookup"><span data-stu-id="b2c88-429">DW1000</span></span> |<span data-ttu-id="b2c88-430">6</span><span class="sxs-lookup"><span data-stu-id="b2c88-430">6</span></span> |<span data-ttu-id="b2c88-431">47</span><span class="sxs-lookup"><span data-stu-id="b2c88-431">47</span></span> |<span data-ttu-id="b2c88-432">94</span><span class="sxs-lookup"><span data-stu-id="b2c88-432">94</span></span> |<span data-ttu-id="b2c88-433">188</span><span class="sxs-lookup"><span data-stu-id="b2c88-433">188</span></span> |
| <span data-ttu-id="b2c88-434">DW1200</span><span class="sxs-lookup"><span data-stu-id="b2c88-434">DW1200</span></span> |<span data-ttu-id="b2c88-435">6</span><span class="sxs-lookup"><span data-stu-id="b2c88-435">6</span></span> |<span data-ttu-id="b2c88-436">47</span><span class="sxs-lookup"><span data-stu-id="b2c88-436">47</span></span> |<span data-ttu-id="b2c88-437">94</span><span class="sxs-lookup"><span data-stu-id="b2c88-437">94</span></span> |<span data-ttu-id="b2c88-438">188</span><span class="sxs-lookup"><span data-stu-id="b2c88-438">188</span></span> |
| <span data-ttu-id="b2c88-439">DW1500</span><span class="sxs-lookup"><span data-stu-id="b2c88-439">DW1500</span></span> |<span data-ttu-id="b2c88-440">6</span><span class="sxs-lookup"><span data-stu-id="b2c88-440">6</span></span> |<span data-ttu-id="b2c88-441">47</span><span class="sxs-lookup"><span data-stu-id="b2c88-441">47</span></span> |<span data-ttu-id="b2c88-442">94</span><span class="sxs-lookup"><span data-stu-id="b2c88-442">94</span></span> |<span data-ttu-id="b2c88-443">188</span><span class="sxs-lookup"><span data-stu-id="b2c88-443">188</span></span> |
| <span data-ttu-id="b2c88-444">DW2000</span><span class="sxs-lookup"><span data-stu-id="b2c88-444">DW2000</span></span> |<span data-ttu-id="b2c88-445">6</span><span class="sxs-lookup"><span data-stu-id="b2c88-445">6</span></span> |<span data-ttu-id="b2c88-446">94</span><span class="sxs-lookup"><span data-stu-id="b2c88-446">94</span></span> |<span data-ttu-id="b2c88-447">188</span><span class="sxs-lookup"><span data-stu-id="b2c88-447">188</span></span> |<span data-ttu-id="b2c88-448">375</span><span class="sxs-lookup"><span data-stu-id="b2c88-448">375</span></span> |
| <span data-ttu-id="b2c88-449">DW3000</span><span class="sxs-lookup"><span data-stu-id="b2c88-449">DW3000</span></span> |<span data-ttu-id="b2c88-450">6</span><span class="sxs-lookup"><span data-stu-id="b2c88-450">6</span></span> |<span data-ttu-id="b2c88-451">94</span><span class="sxs-lookup"><span data-stu-id="b2c88-451">94</span></span> |<span data-ttu-id="b2c88-452">188</span><span class="sxs-lookup"><span data-stu-id="b2c88-452">188</span></span> |<span data-ttu-id="b2c88-453">375</span><span class="sxs-lookup"><span data-stu-id="b2c88-453">375</span></span> |
| <span data-ttu-id="b2c88-454">DW6000</span><span class="sxs-lookup"><span data-stu-id="b2c88-454">DW6000</span></span> |<span data-ttu-id="b2c88-455">6</span><span class="sxs-lookup"><span data-stu-id="b2c88-455">6</span></span> |<span data-ttu-id="b2c88-456">188</span><span class="sxs-lookup"><span data-stu-id="b2c88-456">188</span></span> |<span data-ttu-id="b2c88-457">375</span><span class="sxs-lookup"><span data-stu-id="b2c88-457">375</span></span> |<span data-ttu-id="b2c88-458">750</span><span class="sxs-lookup"><span data-stu-id="b2c88-458">750</span></span> |

<span data-ttu-id="b2c88-459">Ebből a táblázatból tartozó rendszerszintű memória-hozzárendelések, láthatja, hogy a xlargerc erőforrás osztály egy DW2000 futó lekérdezés lefoglalt 375 GB memória összesen (6400 MB * 60 azokat a terjesztéseket / 1024 GB átalakítása) az SQL Data Warehouse a a teljes keresztül.</span><span class="sxs-lookup"><span data-stu-id="b2c88-459">From this table of system-wide memory allocations, you can see that a query running on a DW2000 in the xlargerc resource class is allocated a total of 375 GB of memory (6,400 MB * 60 distributions / 1,024 to convert to GB) over the entirety of your SQL Data Warehouse.</span></span>

<span data-ttu-id="b2c88-460">Ugyanazt a számítást vonatkozik statikus erőforrás.</span><span class="sxs-lookup"><span data-stu-id="b2c88-460">The same calculation applies to static resource classes.</span></span>
 
## <a name="concurrency-slot-consumption"></a><span data-ttu-id="b2c88-461">Párhuzamossági tárolóhely-használat</span><span class="sxs-lookup"><span data-stu-id="b2c88-461">Concurrency slot consumption</span></span>  
<span data-ttu-id="b2c88-462">Az SQL Data Warehouse biztosít több memóriát az magasabb erőforrás osztályok futó lekérdezésekhez.</span><span class="sxs-lookup"><span data-stu-id="b2c88-462">SQL Data Warehouse grants more memory to queries running in higher resource classes.</span></span> <span data-ttu-id="b2c88-463">Memória mérete rögzített erőforrás.</span><span class="sxs-lookup"><span data-stu-id="b2c88-463">Memory is a fixed resource.</span></span>  <span data-ttu-id="b2c88-464">Ezért további fenntartott memória mérete lekérdezésenként, kevesebb párhuzamos lekérdezések hajthat végre.</span><span class="sxs-lookup"><span data-stu-id="b2c88-464">Therefore, the more memory allocated per query, the fewer concurrent queries can execute.</span></span> <span data-ttu-id="b2c88-465">A következő táblázat ismételten kifejezi összes, a korábbi fogalmakat, amelyek a feldolgozási bővítőhelyek DWU által elérhető és a tárolóhelyek minden erőforrásosztály által felhasznált számát jeleníti meg egyetlen nézetben.</span><span class="sxs-lookup"><span data-stu-id="b2c88-465">The following table reiterates all of the previous concepts in a single view that shows the number of concurrency slots available by DWU and the slots consumed by each resource class.</span></span>  

### <a name="allocation-and-consumption-of-concurrency-slots-for-dynamic-resource-classes"></a><span data-ttu-id="b2c88-466">Foglalási és felhasználási párhuzamossági tárolóhelyek dinamikus erőforrás-osztályok</span><span class="sxs-lookup"><span data-stu-id="b2c88-466">Allocation and consumption of concurrency slots for dynamic resource classes</span></span>  
| <span data-ttu-id="b2c88-467">DWU</span><span class="sxs-lookup"><span data-stu-id="b2c88-467">DWU</span></span> | <span data-ttu-id="b2c88-468">Maximális párhuzamos lekérdezések</span><span class="sxs-lookup"><span data-stu-id="b2c88-468">Maximum concurrent queries</span></span> | <span data-ttu-id="b2c88-469">Lefoglalt párhuzamossági tárhelyek</span><span class="sxs-lookup"><span data-stu-id="b2c88-469">Concurrency slots allocated</span></span> | <span data-ttu-id="b2c88-470">Üzembe helyezési ponti smallrc által használt</span><span class="sxs-lookup"><span data-stu-id="b2c88-470">Slots used by smallrc</span></span> | <span data-ttu-id="b2c88-471">Üzembe helyezési ponti mediumrc által használt</span><span class="sxs-lookup"><span data-stu-id="b2c88-471">Slots used by mediumrc</span></span> | <span data-ttu-id="b2c88-472">Üzembe helyezési ponti largerc által használt</span><span class="sxs-lookup"><span data-stu-id="b2c88-472">Slots used by largerc</span></span> | <span data-ttu-id="b2c88-473">Üzembe helyezési ponti xlargerc által használt</span><span class="sxs-lookup"><span data-stu-id="b2c88-473">Slots used by xlargerc</span></span> |
|:--- |:---:|:---:|:---:|:---:|:---:|:---:|
| <span data-ttu-id="b2c88-474">DW100</span><span class="sxs-lookup"><span data-stu-id="b2c88-474">DW100</span></span> |<span data-ttu-id="b2c88-475">4</span><span class="sxs-lookup"><span data-stu-id="b2c88-475">4</span></span> |<span data-ttu-id="b2c88-476">4</span><span class="sxs-lookup"><span data-stu-id="b2c88-476">4</span></span> |<span data-ttu-id="b2c88-477">1</span><span class="sxs-lookup"><span data-stu-id="b2c88-477">1</span></span> |<span data-ttu-id="b2c88-478">1</span><span class="sxs-lookup"><span data-stu-id="b2c88-478">1</span></span> |<span data-ttu-id="b2c88-479">2</span><span class="sxs-lookup"><span data-stu-id="b2c88-479">2</span></span> |<span data-ttu-id="b2c88-480">4</span><span class="sxs-lookup"><span data-stu-id="b2c88-480">4</span></span> |
| <span data-ttu-id="b2c88-481">DW200</span><span class="sxs-lookup"><span data-stu-id="b2c88-481">DW200</span></span> |<span data-ttu-id="b2c88-482">8</span><span class="sxs-lookup"><span data-stu-id="b2c88-482">8</span></span> |<span data-ttu-id="b2c88-483">8</span><span class="sxs-lookup"><span data-stu-id="b2c88-483">8</span></span> |<span data-ttu-id="b2c88-484">1</span><span class="sxs-lookup"><span data-stu-id="b2c88-484">1</span></span> |<span data-ttu-id="b2c88-485">2</span><span class="sxs-lookup"><span data-stu-id="b2c88-485">2</span></span> |<span data-ttu-id="b2c88-486">4</span><span class="sxs-lookup"><span data-stu-id="b2c88-486">4</span></span> |<span data-ttu-id="b2c88-487">8</span><span class="sxs-lookup"><span data-stu-id="b2c88-487">8</span></span> |
| <span data-ttu-id="b2c88-488">DW300</span><span class="sxs-lookup"><span data-stu-id="b2c88-488">DW300</span></span> |<span data-ttu-id="b2c88-489">12</span><span class="sxs-lookup"><span data-stu-id="b2c88-489">12</span></span> |<span data-ttu-id="b2c88-490">12</span><span class="sxs-lookup"><span data-stu-id="b2c88-490">12</span></span> |<span data-ttu-id="b2c88-491">1</span><span class="sxs-lookup"><span data-stu-id="b2c88-491">1</span></span> |<span data-ttu-id="b2c88-492">2</span><span class="sxs-lookup"><span data-stu-id="b2c88-492">2</span></span> |<span data-ttu-id="b2c88-493">4</span><span class="sxs-lookup"><span data-stu-id="b2c88-493">4</span></span> |<span data-ttu-id="b2c88-494">8</span><span class="sxs-lookup"><span data-stu-id="b2c88-494">8</span></span> |
| <span data-ttu-id="b2c88-495">DW400</span><span class="sxs-lookup"><span data-stu-id="b2c88-495">DW400</span></span> |<span data-ttu-id="b2c88-496">16</span><span class="sxs-lookup"><span data-stu-id="b2c88-496">16</span></span> |<span data-ttu-id="b2c88-497">16</span><span class="sxs-lookup"><span data-stu-id="b2c88-497">16</span></span> |<span data-ttu-id="b2c88-498">1</span><span class="sxs-lookup"><span data-stu-id="b2c88-498">1</span></span> |<span data-ttu-id="b2c88-499">4</span><span class="sxs-lookup"><span data-stu-id="b2c88-499">4</span></span> |<span data-ttu-id="b2c88-500">8</span><span class="sxs-lookup"><span data-stu-id="b2c88-500">8</span></span> |<span data-ttu-id="b2c88-501">16</span><span class="sxs-lookup"><span data-stu-id="b2c88-501">16</span></span> |
| <span data-ttu-id="b2c88-502">DW500</span><span class="sxs-lookup"><span data-stu-id="b2c88-502">DW500</span></span> |<span data-ttu-id="b2c88-503">20</span><span class="sxs-lookup"><span data-stu-id="b2c88-503">20</span></span> |<span data-ttu-id="b2c88-504">20</span><span class="sxs-lookup"><span data-stu-id="b2c88-504">20</span></span> |<span data-ttu-id="b2c88-505">1</span><span class="sxs-lookup"><span data-stu-id="b2c88-505">1</span></span> |<span data-ttu-id="b2c88-506">4</span><span class="sxs-lookup"><span data-stu-id="b2c88-506">4</span></span> |<span data-ttu-id="b2c88-507">8</span><span class="sxs-lookup"><span data-stu-id="b2c88-507">8</span></span> |<span data-ttu-id="b2c88-508">16</span><span class="sxs-lookup"><span data-stu-id="b2c88-508">16</span></span> |
| <span data-ttu-id="b2c88-509">DW600</span><span class="sxs-lookup"><span data-stu-id="b2c88-509">DW600</span></span> |<span data-ttu-id="b2c88-510">24</span><span class="sxs-lookup"><span data-stu-id="b2c88-510">24</span></span> |<span data-ttu-id="b2c88-511">24</span><span class="sxs-lookup"><span data-stu-id="b2c88-511">24</span></span> |<span data-ttu-id="b2c88-512">1</span><span class="sxs-lookup"><span data-stu-id="b2c88-512">1</span></span> |<span data-ttu-id="b2c88-513">4</span><span class="sxs-lookup"><span data-stu-id="b2c88-513">4</span></span> |<span data-ttu-id="b2c88-514">8</span><span class="sxs-lookup"><span data-stu-id="b2c88-514">8</span></span> |<span data-ttu-id="b2c88-515">16</span><span class="sxs-lookup"><span data-stu-id="b2c88-515">16</span></span> |
| <span data-ttu-id="b2c88-516">DW1000</span><span class="sxs-lookup"><span data-stu-id="b2c88-516">DW1000</span></span> |<span data-ttu-id="b2c88-517">32</span><span class="sxs-lookup"><span data-stu-id="b2c88-517">32</span></span> |<span data-ttu-id="b2c88-518">40</span><span class="sxs-lookup"><span data-stu-id="b2c88-518">40</span></span> |<span data-ttu-id="b2c88-519">1</span><span class="sxs-lookup"><span data-stu-id="b2c88-519">1</span></span> |<span data-ttu-id="b2c88-520">8</span><span class="sxs-lookup"><span data-stu-id="b2c88-520">8</span></span> |<span data-ttu-id="b2c88-521">16</span><span class="sxs-lookup"><span data-stu-id="b2c88-521">16</span></span> |<span data-ttu-id="b2c88-522">32</span><span class="sxs-lookup"><span data-stu-id="b2c88-522">32</span></span> |
| <span data-ttu-id="b2c88-523">DW1200</span><span class="sxs-lookup"><span data-stu-id="b2c88-523">DW1200</span></span> |<span data-ttu-id="b2c88-524">32</span><span class="sxs-lookup"><span data-stu-id="b2c88-524">32</span></span> |<span data-ttu-id="b2c88-525">48</span><span class="sxs-lookup"><span data-stu-id="b2c88-525">48</span></span> |<span data-ttu-id="b2c88-526">1</span><span class="sxs-lookup"><span data-stu-id="b2c88-526">1</span></span> |<span data-ttu-id="b2c88-527">8</span><span class="sxs-lookup"><span data-stu-id="b2c88-527">8</span></span> |<span data-ttu-id="b2c88-528">16</span><span class="sxs-lookup"><span data-stu-id="b2c88-528">16</span></span> |<span data-ttu-id="b2c88-529">32</span><span class="sxs-lookup"><span data-stu-id="b2c88-529">32</span></span> |
| <span data-ttu-id="b2c88-530">DW1500</span><span class="sxs-lookup"><span data-stu-id="b2c88-530">DW1500</span></span> |<span data-ttu-id="b2c88-531">32</span><span class="sxs-lookup"><span data-stu-id="b2c88-531">32</span></span> |<span data-ttu-id="b2c88-532">60</span><span class="sxs-lookup"><span data-stu-id="b2c88-532">60</span></span> |<span data-ttu-id="b2c88-533">1</span><span class="sxs-lookup"><span data-stu-id="b2c88-533">1</span></span> |<span data-ttu-id="b2c88-534">8</span><span class="sxs-lookup"><span data-stu-id="b2c88-534">8</span></span> |<span data-ttu-id="b2c88-535">16</span><span class="sxs-lookup"><span data-stu-id="b2c88-535">16</span></span> |<span data-ttu-id="b2c88-536">32</span><span class="sxs-lookup"><span data-stu-id="b2c88-536">32</span></span> |
| <span data-ttu-id="b2c88-537">DW2000</span><span class="sxs-lookup"><span data-stu-id="b2c88-537">DW2000</span></span> |<span data-ttu-id="b2c88-538">32</span><span class="sxs-lookup"><span data-stu-id="b2c88-538">32</span></span> |<span data-ttu-id="b2c88-539">80</span><span class="sxs-lookup"><span data-stu-id="b2c88-539">80</span></span> |<span data-ttu-id="b2c88-540">1</span><span class="sxs-lookup"><span data-stu-id="b2c88-540">1</span></span> |<span data-ttu-id="b2c88-541">16</span><span class="sxs-lookup"><span data-stu-id="b2c88-541">16</span></span> |<span data-ttu-id="b2c88-542">32</span><span class="sxs-lookup"><span data-stu-id="b2c88-542">32</span></span> |<span data-ttu-id="b2c88-543">64</span><span class="sxs-lookup"><span data-stu-id="b2c88-543">64</span></span> |
| <span data-ttu-id="b2c88-544">DW3000</span><span class="sxs-lookup"><span data-stu-id="b2c88-544">DW3000</span></span> |<span data-ttu-id="b2c88-545">32</span><span class="sxs-lookup"><span data-stu-id="b2c88-545">32</span></span> |<span data-ttu-id="b2c88-546">120</span><span class="sxs-lookup"><span data-stu-id="b2c88-546">120</span></span> |<span data-ttu-id="b2c88-547">1</span><span class="sxs-lookup"><span data-stu-id="b2c88-547">1</span></span> |<span data-ttu-id="b2c88-548">16</span><span class="sxs-lookup"><span data-stu-id="b2c88-548">16</span></span> |<span data-ttu-id="b2c88-549">32</span><span class="sxs-lookup"><span data-stu-id="b2c88-549">32</span></span> |<span data-ttu-id="b2c88-550">64</span><span class="sxs-lookup"><span data-stu-id="b2c88-550">64</span></span> |
| <span data-ttu-id="b2c88-551">DW6000</span><span class="sxs-lookup"><span data-stu-id="b2c88-551">DW6000</span></span> |<span data-ttu-id="b2c88-552">32</span><span class="sxs-lookup"><span data-stu-id="b2c88-552">32</span></span> |<span data-ttu-id="b2c88-553">240</span><span class="sxs-lookup"><span data-stu-id="b2c88-553">240</span></span> |<span data-ttu-id="b2c88-554">1</span><span class="sxs-lookup"><span data-stu-id="b2c88-554">1</span></span> |<span data-ttu-id="b2c88-555">32</span><span class="sxs-lookup"><span data-stu-id="b2c88-555">32</span></span> |<span data-ttu-id="b2c88-556">64</span><span class="sxs-lookup"><span data-stu-id="b2c88-556">64</span></span> |<span data-ttu-id="b2c88-557">128</span><span class="sxs-lookup"><span data-stu-id="b2c88-557">128</span></span> |

### <a name="allocation-and-consumption-of-concurrency-slots-for-static-resource-classes"></a><span data-ttu-id="b2c88-558">Foglalási és felhasználási párhuzamossági tárolóhelyek statikus erőforrás osztályok</span><span class="sxs-lookup"><span data-stu-id="b2c88-558">Allocation and consumption of concurrency slots for static resource classes</span></span>  
| <span data-ttu-id="b2c88-559">DWU</span><span class="sxs-lookup"><span data-stu-id="b2c88-559">DWU</span></span> | <span data-ttu-id="b2c88-560">Maximális párhuzamos lekérdezések</span><span class="sxs-lookup"><span data-stu-id="b2c88-560">Maximum concurrent queries</span></span> | <span data-ttu-id="b2c88-561">Lefoglalt párhuzamossági tárhelyek</span><span class="sxs-lookup"><span data-stu-id="b2c88-561">Concurrency slots allocated</span></span> |<span data-ttu-id="b2c88-562">staticrc10</span><span class="sxs-lookup"><span data-stu-id="b2c88-562">staticrc10</span></span> | <span data-ttu-id="b2c88-563">staticrc20</span><span class="sxs-lookup"><span data-stu-id="b2c88-563">staticrc20</span></span> | <span data-ttu-id="b2c88-564">staticrc30</span><span class="sxs-lookup"><span data-stu-id="b2c88-564">staticrc30</span></span> | <span data-ttu-id="b2c88-565">staticrc40</span><span class="sxs-lookup"><span data-stu-id="b2c88-565">staticrc40</span></span> | <span data-ttu-id="b2c88-566">staticrc50</span><span class="sxs-lookup"><span data-stu-id="b2c88-566">staticrc50</span></span> | <span data-ttu-id="b2c88-567">staticrc60</span><span class="sxs-lookup"><span data-stu-id="b2c88-567">staticrc60</span></span> | <span data-ttu-id="b2c88-568">staticrc70</span><span class="sxs-lookup"><span data-stu-id="b2c88-568">staticrc70</span></span> | <span data-ttu-id="b2c88-569">staticrc80</span><span class="sxs-lookup"><span data-stu-id="b2c88-569">staticrc80</span></span> |
|:--- |:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| <span data-ttu-id="b2c88-570">DW100</span><span class="sxs-lookup"><span data-stu-id="b2c88-570">DW100</span></span> |<span data-ttu-id="b2c88-571">4</span><span class="sxs-lookup"><span data-stu-id="b2c88-571">4</span></span> |<span data-ttu-id="b2c88-572">4</span><span class="sxs-lookup"><span data-stu-id="b2c88-572">4</span></span> |<span data-ttu-id="b2c88-573">1</span><span class="sxs-lookup"><span data-stu-id="b2c88-573">1</span></span> |<span data-ttu-id="b2c88-574">2</span><span class="sxs-lookup"><span data-stu-id="b2c88-574">2</span></span> |<span data-ttu-id="b2c88-575">4</span><span class="sxs-lookup"><span data-stu-id="b2c88-575">4</span></span> |<span data-ttu-id="b2c88-576">4</span><span class="sxs-lookup"><span data-stu-id="b2c88-576">4</span></span> |<span data-ttu-id="b2c88-577">4</span><span class="sxs-lookup"><span data-stu-id="b2c88-577">4</span></span> |<span data-ttu-id="b2c88-578">4</span><span class="sxs-lookup"><span data-stu-id="b2c88-578">4</span></span> |<span data-ttu-id="b2c88-579">4</span><span class="sxs-lookup"><span data-stu-id="b2c88-579">4</span></span> |<span data-ttu-id="b2c88-580">4</span><span class="sxs-lookup"><span data-stu-id="b2c88-580">4</span></span> |
| <span data-ttu-id="b2c88-581">DW200</span><span class="sxs-lookup"><span data-stu-id="b2c88-581">DW200</span></span> |<span data-ttu-id="b2c88-582">8</span><span class="sxs-lookup"><span data-stu-id="b2c88-582">8</span></span> |<span data-ttu-id="b2c88-583">8</span><span class="sxs-lookup"><span data-stu-id="b2c88-583">8</span></span> |<span data-ttu-id="b2c88-584">1</span><span class="sxs-lookup"><span data-stu-id="b2c88-584">1</span></span> |<span data-ttu-id="b2c88-585">2</span><span class="sxs-lookup"><span data-stu-id="b2c88-585">2</span></span> |<span data-ttu-id="b2c88-586">4</span><span class="sxs-lookup"><span data-stu-id="b2c88-586">4</span></span> |<span data-ttu-id="b2c88-587">8</span><span class="sxs-lookup"><span data-stu-id="b2c88-587">8</span></span> |<span data-ttu-id="b2c88-588">8</span><span class="sxs-lookup"><span data-stu-id="b2c88-588">8</span></span> |<span data-ttu-id="b2c88-589">8</span><span class="sxs-lookup"><span data-stu-id="b2c88-589">8</span></span> |<span data-ttu-id="b2c88-590">8</span><span class="sxs-lookup"><span data-stu-id="b2c88-590">8</span></span> |<span data-ttu-id="b2c88-591">8</span><span class="sxs-lookup"><span data-stu-id="b2c88-591">8</span></span> |
| <span data-ttu-id="b2c88-592">DW300</span><span class="sxs-lookup"><span data-stu-id="b2c88-592">DW300</span></span> |<span data-ttu-id="b2c88-593">12</span><span class="sxs-lookup"><span data-stu-id="b2c88-593">12</span></span> |<span data-ttu-id="b2c88-594">12</span><span class="sxs-lookup"><span data-stu-id="b2c88-594">12</span></span> |<span data-ttu-id="b2c88-595">1</span><span class="sxs-lookup"><span data-stu-id="b2c88-595">1</span></span> |<span data-ttu-id="b2c88-596">2</span><span class="sxs-lookup"><span data-stu-id="b2c88-596">2</span></span> |<span data-ttu-id="b2c88-597">4</span><span class="sxs-lookup"><span data-stu-id="b2c88-597">4</span></span> |<span data-ttu-id="b2c88-598">8</span><span class="sxs-lookup"><span data-stu-id="b2c88-598">8</span></span> |<span data-ttu-id="b2c88-599">8</span><span class="sxs-lookup"><span data-stu-id="b2c88-599">8</span></span> |<span data-ttu-id="b2c88-600">8</span><span class="sxs-lookup"><span data-stu-id="b2c88-600">8</span></span> |<span data-ttu-id="b2c88-601">8</span><span class="sxs-lookup"><span data-stu-id="b2c88-601">8</span></span> |<span data-ttu-id="b2c88-602">8</span><span class="sxs-lookup"><span data-stu-id="b2c88-602">8</span></span> |
| <span data-ttu-id="b2c88-603">DW400</span><span class="sxs-lookup"><span data-stu-id="b2c88-603">DW400</span></span> |<span data-ttu-id="b2c88-604">16</span><span class="sxs-lookup"><span data-stu-id="b2c88-604">16</span></span> |<span data-ttu-id="b2c88-605">16</span><span class="sxs-lookup"><span data-stu-id="b2c88-605">16</span></span> |<span data-ttu-id="b2c88-606">1</span><span class="sxs-lookup"><span data-stu-id="b2c88-606">1</span></span> |<span data-ttu-id="b2c88-607">2</span><span class="sxs-lookup"><span data-stu-id="b2c88-607">2</span></span> |<span data-ttu-id="b2c88-608">4</span><span class="sxs-lookup"><span data-stu-id="b2c88-608">4</span></span> |<span data-ttu-id="b2c88-609">8</span><span class="sxs-lookup"><span data-stu-id="b2c88-609">8</span></span> |<span data-ttu-id="b2c88-610">16</span><span class="sxs-lookup"><span data-stu-id="b2c88-610">16</span></span> |<span data-ttu-id="b2c88-611">16</span><span class="sxs-lookup"><span data-stu-id="b2c88-611">16</span></span> |<span data-ttu-id="b2c88-612">16</span><span class="sxs-lookup"><span data-stu-id="b2c88-612">16</span></span> |<span data-ttu-id="b2c88-613">16</span><span class="sxs-lookup"><span data-stu-id="b2c88-613">16</span></span> |
| <span data-ttu-id="b2c88-614">DW500</span><span class="sxs-lookup"><span data-stu-id="b2c88-614">DW500</span></span> | <span data-ttu-id="b2c88-615">20</span><span class="sxs-lookup"><span data-stu-id="b2c88-615">20</span></span>| <span data-ttu-id="b2c88-616">20</span><span class="sxs-lookup"><span data-stu-id="b2c88-616">20</span></span>| <span data-ttu-id="b2c88-617">1</span><span class="sxs-lookup"><span data-stu-id="b2c88-617">1</span></span>| <span data-ttu-id="b2c88-618">2</span><span class="sxs-lookup"><span data-stu-id="b2c88-618">2</span></span>| <span data-ttu-id="b2c88-619">4</span><span class="sxs-lookup"><span data-stu-id="b2c88-619">4</span></span>| <span data-ttu-id="b2c88-620">8</span><span class="sxs-lookup"><span data-stu-id="b2c88-620">8</span></span>| <span data-ttu-id="b2c88-621">16</span><span class="sxs-lookup"><span data-stu-id="b2c88-621">16</span></span>| <span data-ttu-id="b2c88-622">16</span><span class="sxs-lookup"><span data-stu-id="b2c88-622">16</span></span>| <span data-ttu-id="b2c88-623">16</span><span class="sxs-lookup"><span data-stu-id="b2c88-623">16</span></span>| <span data-ttu-id="b2c88-624">16</span><span class="sxs-lookup"><span data-stu-id="b2c88-624">16</span></span>|
| <span data-ttu-id="b2c88-625">DW600</span><span class="sxs-lookup"><span data-stu-id="b2c88-625">DW600</span></span> | <span data-ttu-id="b2c88-626">24</span><span class="sxs-lookup"><span data-stu-id="b2c88-626">24</span></span>| <span data-ttu-id="b2c88-627">24</span><span class="sxs-lookup"><span data-stu-id="b2c88-627">24</span></span>| <span data-ttu-id="b2c88-628">1</span><span class="sxs-lookup"><span data-stu-id="b2c88-628">1</span></span>| <span data-ttu-id="b2c88-629">2</span><span class="sxs-lookup"><span data-stu-id="b2c88-629">2</span></span>| <span data-ttu-id="b2c88-630">4</span><span class="sxs-lookup"><span data-stu-id="b2c88-630">4</span></span>| <span data-ttu-id="b2c88-631">8</span><span class="sxs-lookup"><span data-stu-id="b2c88-631">8</span></span>| <span data-ttu-id="b2c88-632">16</span><span class="sxs-lookup"><span data-stu-id="b2c88-632">16</span></span>| <span data-ttu-id="b2c88-633">16</span><span class="sxs-lookup"><span data-stu-id="b2c88-633">16</span></span>| <span data-ttu-id="b2c88-634">16</span><span class="sxs-lookup"><span data-stu-id="b2c88-634">16</span></span>| <span data-ttu-id="b2c88-635">16</span><span class="sxs-lookup"><span data-stu-id="b2c88-635">16</span></span>|
| <span data-ttu-id="b2c88-636">DW1000</span><span class="sxs-lookup"><span data-stu-id="b2c88-636">DW1000</span></span> | <span data-ttu-id="b2c88-637">32</span><span class="sxs-lookup"><span data-stu-id="b2c88-637">32</span></span>| <span data-ttu-id="b2c88-638">40</span><span class="sxs-lookup"><span data-stu-id="b2c88-638">40</span></span>| <span data-ttu-id="b2c88-639">1</span><span class="sxs-lookup"><span data-stu-id="b2c88-639">1</span></span>| <span data-ttu-id="b2c88-640">2</span><span class="sxs-lookup"><span data-stu-id="b2c88-640">2</span></span>| <span data-ttu-id="b2c88-641">4</span><span class="sxs-lookup"><span data-stu-id="b2c88-641">4</span></span>| <span data-ttu-id="b2c88-642">8</span><span class="sxs-lookup"><span data-stu-id="b2c88-642">8</span></span>| <span data-ttu-id="b2c88-643">16</span><span class="sxs-lookup"><span data-stu-id="b2c88-643">16</span></span>| <span data-ttu-id="b2c88-644">32</span><span class="sxs-lookup"><span data-stu-id="b2c88-644">32</span></span>| <span data-ttu-id="b2c88-645">32</span><span class="sxs-lookup"><span data-stu-id="b2c88-645">32</span></span>| <span data-ttu-id="b2c88-646">32</span><span class="sxs-lookup"><span data-stu-id="b2c88-646">32</span></span>|
| <span data-ttu-id="b2c88-647">DW1200</span><span class="sxs-lookup"><span data-stu-id="b2c88-647">DW1200</span></span> | <span data-ttu-id="b2c88-648">32</span><span class="sxs-lookup"><span data-stu-id="b2c88-648">32</span></span>| <span data-ttu-id="b2c88-649">48</span><span class="sxs-lookup"><span data-stu-id="b2c88-649">48</span></span>| <span data-ttu-id="b2c88-650">1</span><span class="sxs-lookup"><span data-stu-id="b2c88-650">1</span></span>| <span data-ttu-id="b2c88-651">2</span><span class="sxs-lookup"><span data-stu-id="b2c88-651">2</span></span>| <span data-ttu-id="b2c88-652">4</span><span class="sxs-lookup"><span data-stu-id="b2c88-652">4</span></span>| <span data-ttu-id="b2c88-653">8</span><span class="sxs-lookup"><span data-stu-id="b2c88-653">8</span></span>| <span data-ttu-id="b2c88-654">16</span><span class="sxs-lookup"><span data-stu-id="b2c88-654">16</span></span>| <span data-ttu-id="b2c88-655">32</span><span class="sxs-lookup"><span data-stu-id="b2c88-655">32</span></span>| <span data-ttu-id="b2c88-656">32</span><span class="sxs-lookup"><span data-stu-id="b2c88-656">32</span></span>| <span data-ttu-id="b2c88-657">32</span><span class="sxs-lookup"><span data-stu-id="b2c88-657">32</span></span>|
| <span data-ttu-id="b2c88-658">DW1500</span><span class="sxs-lookup"><span data-stu-id="b2c88-658">DW1500</span></span> | <span data-ttu-id="b2c88-659">32</span><span class="sxs-lookup"><span data-stu-id="b2c88-659">32</span></span>| <span data-ttu-id="b2c88-660">60</span><span class="sxs-lookup"><span data-stu-id="b2c88-660">60</span></span>| <span data-ttu-id="b2c88-661">1</span><span class="sxs-lookup"><span data-stu-id="b2c88-661">1</span></span>| <span data-ttu-id="b2c88-662">2</span><span class="sxs-lookup"><span data-stu-id="b2c88-662">2</span></span>| <span data-ttu-id="b2c88-663">4</span><span class="sxs-lookup"><span data-stu-id="b2c88-663">4</span></span>| <span data-ttu-id="b2c88-664">8</span><span class="sxs-lookup"><span data-stu-id="b2c88-664">8</span></span>| <span data-ttu-id="b2c88-665">16</span><span class="sxs-lookup"><span data-stu-id="b2c88-665">16</span></span>| <span data-ttu-id="b2c88-666">32</span><span class="sxs-lookup"><span data-stu-id="b2c88-666">32</span></span>| <span data-ttu-id="b2c88-667">32</span><span class="sxs-lookup"><span data-stu-id="b2c88-667">32</span></span>| <span data-ttu-id="b2c88-668">32</span><span class="sxs-lookup"><span data-stu-id="b2c88-668">32</span></span>|
| <span data-ttu-id="b2c88-669">DW2000</span><span class="sxs-lookup"><span data-stu-id="b2c88-669">DW2000</span></span> | <span data-ttu-id="b2c88-670">32</span><span class="sxs-lookup"><span data-stu-id="b2c88-670">32</span></span>| <span data-ttu-id="b2c88-671">80</span><span class="sxs-lookup"><span data-stu-id="b2c88-671">80</span></span>| <span data-ttu-id="b2c88-672">1</span><span class="sxs-lookup"><span data-stu-id="b2c88-672">1</span></span>| <span data-ttu-id="b2c88-673">2</span><span class="sxs-lookup"><span data-stu-id="b2c88-673">2</span></span>| <span data-ttu-id="b2c88-674">4</span><span class="sxs-lookup"><span data-stu-id="b2c88-674">4</span></span>| <span data-ttu-id="b2c88-675">8</span><span class="sxs-lookup"><span data-stu-id="b2c88-675">8</span></span>| <span data-ttu-id="b2c88-676">16</span><span class="sxs-lookup"><span data-stu-id="b2c88-676">16</span></span>| <span data-ttu-id="b2c88-677">32</span><span class="sxs-lookup"><span data-stu-id="b2c88-677">32</span></span>| <span data-ttu-id="b2c88-678">64</span><span class="sxs-lookup"><span data-stu-id="b2c88-678">64</span></span>| <span data-ttu-id="b2c88-679">64</span><span class="sxs-lookup"><span data-stu-id="b2c88-679">64</span></span>|
| <span data-ttu-id="b2c88-680">DW3000</span><span class="sxs-lookup"><span data-stu-id="b2c88-680">DW3000</span></span> | <span data-ttu-id="b2c88-681">32</span><span class="sxs-lookup"><span data-stu-id="b2c88-681">32</span></span>| <span data-ttu-id="b2c88-682">120</span><span class="sxs-lookup"><span data-stu-id="b2c88-682">120</span></span>| <span data-ttu-id="b2c88-683">1</span><span class="sxs-lookup"><span data-stu-id="b2c88-683">1</span></span>| <span data-ttu-id="b2c88-684">2</span><span class="sxs-lookup"><span data-stu-id="b2c88-684">2</span></span>| <span data-ttu-id="b2c88-685">4</span><span class="sxs-lookup"><span data-stu-id="b2c88-685">4</span></span>| <span data-ttu-id="b2c88-686">8</span><span class="sxs-lookup"><span data-stu-id="b2c88-686">8</span></span>| <span data-ttu-id="b2c88-687">16</span><span class="sxs-lookup"><span data-stu-id="b2c88-687">16</span></span>| <span data-ttu-id="b2c88-688">32</span><span class="sxs-lookup"><span data-stu-id="b2c88-688">32</span></span>| <span data-ttu-id="b2c88-689">64</span><span class="sxs-lookup"><span data-stu-id="b2c88-689">64</span></span>| <span data-ttu-id="b2c88-690">64</span><span class="sxs-lookup"><span data-stu-id="b2c88-690">64</span></span>|
| <span data-ttu-id="b2c88-691">DW6000</span><span class="sxs-lookup"><span data-stu-id="b2c88-691">DW6000</span></span> | <span data-ttu-id="b2c88-692">32</span><span class="sxs-lookup"><span data-stu-id="b2c88-692">32</span></span>| <span data-ttu-id="b2c88-693">240</span><span class="sxs-lookup"><span data-stu-id="b2c88-693">240</span></span>| <span data-ttu-id="b2c88-694">1</span><span class="sxs-lookup"><span data-stu-id="b2c88-694">1</span></span>| <span data-ttu-id="b2c88-695">2</span><span class="sxs-lookup"><span data-stu-id="b2c88-695">2</span></span>| <span data-ttu-id="b2c88-696">4</span><span class="sxs-lookup"><span data-stu-id="b2c88-696">4</span></span>| <span data-ttu-id="b2c88-697">8</span><span class="sxs-lookup"><span data-stu-id="b2c88-697">8</span></span>| <span data-ttu-id="b2c88-698">16</span><span class="sxs-lookup"><span data-stu-id="b2c88-698">16</span></span>| <span data-ttu-id="b2c88-699">32</span><span class="sxs-lookup"><span data-stu-id="b2c88-699">32</span></span>| <span data-ttu-id="b2c88-700">64</span><span class="sxs-lookup"><span data-stu-id="b2c88-700">64</span></span>| <span data-ttu-id="b2c88-701">128</span><span class="sxs-lookup"><span data-stu-id="b2c88-701">128</span></span>|

<span data-ttu-id="b2c88-702">Ezek a táblázatok a láthatja, hogy az SQL Data Warehouse fut, DW1000 foglal le, mely legfeljebb 32 egyidejű lekérdezéseket és 40 párhuzamossági tárolóhelyek összesen.</span><span class="sxs-lookup"><span data-stu-id="b2c88-702">From these tables, you can see that SQL Data Warehouse running as DW1000 allocates a maximum of 32 concurrent queries and a total of 40 concurrency slots.</span></span> <span data-ttu-id="b2c88-703">Minden felhasználó smallrc fut, ha 32 egyidejű lekérdezések volna kell engedélyezett, mert minden egyes lekérdezés 1 párhuzamossági tárolóhely volna felhasználását.</span><span class="sxs-lookup"><span data-stu-id="b2c88-703">If all users are running in smallrc, 32 concurrent queries would be allowed because each query would consume 1 concurrency slot.</span></span> <span data-ttu-id="b2c88-704">Ha minden felhasználó egy DW1000 mediumrc a futtatást, minden egyes lekérdezés szeretné kiosztani 800 MB eloszlása lekérdezésenként 47 GB memória összesen kiosztható és feldolgozási korlátozódik, 5 felhasználók (40 párhuzamossági üzembe helyezési ponti / 8 tárolóhely mediumrc felhasználónként).</span><span class="sxs-lookup"><span data-stu-id="b2c88-704">If all users on a DW1000 were running in mediumrc, each query would be allocated 800 MB per distribution for a total memory allocation of 47 GB per query, and concurrency would be limited to 5 users (40 concurrency slots / 8 slots per mediumrc user).</span></span>

## <a name="selecting-proper-resource-class"></a><span data-ttu-id="b2c88-705">Megfelelő erőforrásosztály kiválasztása</span><span class="sxs-lookup"><span data-stu-id="b2c88-705">Selecting proper resource class</span></span>  
<span data-ttu-id="b2c88-706">Egy jó gyakorlat az, hogy véglegesen felhasználók hozzárendelése az erőforrás-osztályok módosítása helyett egy erőforrásosztály.</span><span class="sxs-lookup"><span data-stu-id="b2c88-706">A good practice is to permanently assign users to a resource class rather than changing their resource classes.</span></span> <span data-ttu-id="b2c88-707">Fürtözött oszloptárindexű táblákat terhelések hozzon létre például a következő: jobb minőségű indexeket, amikor több memóriát foglal le.</span><span class="sxs-lookup"><span data-stu-id="b2c88-707">For example, loads to clustered columnstore tables create higher-quality indexes when allocated more memory.</span></span> <span data-ttu-id="b2c88-708">Győződjön meg arról, hogy terhelések hozzáférhetnek a nagyobb memóriába, kifejezetten az adatok betöltése a felhasználó létrehozása, és a felhasználó véglegesen hozzárendelése egy magasabb erőforrásosztály.</span><span class="sxs-lookup"><span data-stu-id="b2c88-708">To ensure that loads have access to higher memory, create a user specifically for loading data and permanently assign this user to a higher resource class.</span></span>
<span data-ttu-id="b2c88-709">Nincsenek ajánlott eljárásokat Itt néhány.</span><span class="sxs-lookup"><span data-stu-id="b2c88-709">There are a couple of best practices to follow here.</span></span> <span data-ttu-id="b2c88-710">Fent említett SQL DW támogatja-e a kétféle típusú erőforrások osztály: erőforrás statikus és dinamikus erőforrás-osztályok.</span><span class="sxs-lookup"><span data-stu-id="b2c88-710">As mentioned above, SQL DW supports two kinds of resource class types: static resource classes and dynamic resource classes.</span></span>
### <a name="loading-best-practices"></a><span data-ttu-id="b2c88-711">Gyakorlati tanácsok betöltése</span><span class="sxs-lookup"><span data-stu-id="b2c88-711">Loading best practices</span></span>
1.  <span data-ttu-id="b2c88-712">Ha az elvárások betölti a rendszeres adatmennyiséget, statikus erőforrásosztály érdemes használni.</span><span class="sxs-lookup"><span data-stu-id="b2c88-712">If the expectations are loads with regular amount of data, a static resource class is a good choice.</span></span> <span data-ttu-id="b2c88-713">Később Ha vertikális felskálázásával eléréséhez további számítási teljesítménnyel rendelkezik, az adatraktár tudnak futni több egyidejű lekérdezéseket, a-kész, a betöltés felhasználó nem több memóriát igényel.</span><span class="sxs-lookup"><span data-stu-id="b2c88-713">Later, when scaling up to get more computational power, the data warehouse will be able to run more concurrent queries out-of-the-box, as the load user does not consume more memory.</span></span>
2.  <span data-ttu-id="b2c88-714">Ha az elvárások nagyobb terhelés egyes esetekben, dinamikus erőforrásosztály érdemes használni.</span><span class="sxs-lookup"><span data-stu-id="b2c88-714">If the expectations are bigger loads in some occasions, a dynamic resource class is a good choice.</span></span> <span data-ttu-id="b2c88-715">Később, ha vertikális felskálázásával eléréséhez további számítási teljesítménnyel rendelkezik, a betöltés felhasználó kap további memória az a-kész, így lehetővé téve a gyorsabb végrehajtásához a terhelés.</span><span class="sxs-lookup"><span data-stu-id="b2c88-715">Later, when scaling up to get more computational power, the load user will get more memory out-of-the-box, hence allowing the load to perform faster.</span></span>

<span data-ttu-id="b2c88-716">A terhelés hatékonyan feldolgozásához szükséges memória betöltve a tábla és a feldolgozott adatok mennyisége jellegétől függ.</span><span class="sxs-lookup"><span data-stu-id="b2c88-716">The memory needed to process loads efficiently depends on the nature of the table loaded and the amount of data processed.</span></span> <span data-ttu-id="b2c88-717">Például adatok közösségi koordináló intézet táblába töltéséhez ahhoz, hogy a közösségi koordináló intézet rowgroups optimalizálási elérni bizonyos memóriát igényel.</span><span class="sxs-lookup"><span data-stu-id="b2c88-717">For instance, loading data into CCI tables requires some memory to let CCI rowgroups reach optimality.</span></span> <span data-ttu-id="b2c88-718">További részletekért tekintse meg az Oszlopcentrikus indexek - Adatbetöltési útmutatást.</span><span class="sxs-lookup"><span data-stu-id="b2c88-718">For more details, see the Columnstore indexes - data loading guidance.</span></span>

<span data-ttu-id="b2c88-719">Ajánlott eljárásként javasoljuk terhelések legalább 200MB memória használata.</span><span class="sxs-lookup"><span data-stu-id="b2c88-719">As a best practice, we advise you to use at least 200MB of memory for loads.</span></span>

### <a name="querying-best-practices"></a><span data-ttu-id="b2c88-720">Gyakorlati tanácsok lekérdezése</span><span class="sxs-lookup"><span data-stu-id="b2c88-720">Querying best practices</span></span>
<span data-ttu-id="b2c88-721">Lekérdezések összetettségük függően különböző követelményekkel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="b2c88-721">Queries have different requirements depending on their complexity.</span></span> <span data-ttu-id="b2c88-722">Növelje a lekérdezésenként memóriát, vagy a feldolgozási növelését módon egyaránt érvényes lekérdezés igényeitől függően a teljes átviteli sebesség révén.</span><span class="sxs-lookup"><span data-stu-id="b2c88-722">Increasing memory per query or increasing the concurrency are both valid ways to augment overall throughput depending on the query needs.</span></span>
1.  <span data-ttu-id="b2c88-723">Az elvárások rendszeres, összetett lekérdezések esetén (például napi és heti jelentések készítéséhez), és nem kell párhuzamossági előnyeit, dinamikus erőforrásosztály, akkor a.</span><span class="sxs-lookup"><span data-stu-id="b2c88-723">If the expectations are regular, complex queries (for instance, to generate daily and weekly reports) and do not need to take advantage of concurrency, a dynamic resource class is a good choice.</span></span> <span data-ttu-id="b2c88-724">Ha a rendszer további adatokat feldolgozni, az adatraktár vertikális felskálázásával ezért automatikusan nyújtják több memóriát a lekérdezés futtatása a felhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="b2c88-724">If the system has more data to process, scaling up the data warehouse will therefore automatically provide more memory to the user running the query.</span></span>
2.  <span data-ttu-id="b2c88-725">Ha elvárásainak változó vagy diurnal egyidejűségi minták (például ha az adatbázis lekérik a webes felhasználói felület széles körben elérhető keresztül), egy statikus erőforrásosztály, akkor a.</span><span class="sxs-lookup"><span data-stu-id="b2c88-725">If the expectations are variable or diurnal concurrency patterns (for instance if the database is queried through a web UI broadly accessible), a static resource class is a good choice.</span></span> <span data-ttu-id="b2c88-726">Később Ha vertikális felskálázásával adatraktár, a felhasználó statikus erőforrásosztály társított automatikusan tudnak több egyidejű lekérdezések futtatásához.</span><span class="sxs-lookup"><span data-stu-id="b2c88-726">Later, when scaling up to data warehouse, the user associated with the static resource class will automatically be able to run more concurrent queries.</span></span>

<span data-ttu-id="b2c88-727">Ez számos tényezőtől függ, például a lekérdezett, adatok mennyiségét a táblasémákat és különböző csatlakozást, kiválasztása és a csoport predikátumok jellege nem nem triviális, attól függően, hogy a lekérdezés kellene kiválasztásával megfelelő memóriaengedély.</span><span class="sxs-lookup"><span data-stu-id="b2c88-727">Selecting proper memory grant depending on the need of your query is non-trivial, as it depends on many factors, such as the amount of data queried, the nature of the table schemas, and various join, selection, and group predicates.</span></span> <span data-ttu-id="b2c88-728">Egy általános tükrözik, rendeljen több memóriát lehetővé teszi a lekérdezéseket, amelyekkel gyorsabban befejeződjenek, de a teljes feldolgozási csökkenne.</span><span class="sxs-lookup"><span data-stu-id="b2c88-728">From a general standpoint, allocating more memory will allow queries to complete faster, but would reduce the overall concurrency.</span></span> <span data-ttu-id="b2c88-729">Párhuzamossági darabolása nem okoz problémát, ha nem árt túlzott memória lefoglalásakor.</span><span class="sxs-lookup"><span data-stu-id="b2c88-729">If concurrency is not an issue, over-allocating memory does not harm.</span></span> <span data-ttu-id="b2c88-730">Átviteli sebesség finomhangolása, különböző változataira jellemző az erőforrás-osztályok tett kísérlet lehet szükség.</span><span class="sxs-lookup"><span data-stu-id="b2c88-730">To fine-tune throughput, trying various flavors of resource classes may be required.</span></span>

<span data-ttu-id="b2c88-731">A következő tárolt eljárás segítségével mérje fel, egyidejűségi és a memória grant / erőforrás egy adott slo-t, és a legközelebbi legjobb erőforrás osztály memória intenzív közösségi koordináló intézet műveletekhez közösségi koordináló intézet tábla particionálva adott erőforrás osztályra:</span><span class="sxs-lookup"><span data-stu-id="b2c88-731">You can use the following stored procedure to figure out concurrency and memory grant per resource class at a given SLO and the closest best resource class for memory intensive CCI operations on non-partitioned CCI table at a given resource class:</span></span>

#### <a name="description"></a><span data-ttu-id="b2c88-732">Leírás:</span><span class="sxs-lookup"><span data-stu-id="b2c88-732">Description:</span></span>  
<span data-ttu-id="b2c88-733">A tárolt eljárás célja van:</span><span class="sxs-lookup"><span data-stu-id="b2c88-733">Here's the purpose of this stored procedure:</span></span>  
1. <span data-ttu-id="b2c88-734">Segítségével mérje fel, felhasználói egyidejűségi és a memória adja meg egy erőforrás osztály egy adott SLO.</span><span class="sxs-lookup"><span data-stu-id="b2c88-734">To help user figure out concurrency and memory grant per resource class at a given SLO.</span></span> <span data-ttu-id="b2c88-735">Felhasználói kell megadni. null értékű séma és a tablename ehhez az alábbi példában látható módon.</span><span class="sxs-lookup"><span data-stu-id="b2c88-735">User needs to provide NULL for both schema and tablename for this as shown in the example below.</span></span>  
2. <span data-ttu-id="b2c88-736">Felhasználói segítségével mérje fel, a memória intensed legközelebbi legjobb erőforrásosztály közösségi koordináló intézet műveleteket (terhelésétől, a másolási tábla rebuild index stb.) a megadott erőforrásosztály nem particionált közösségi koordináló intézet táblázat.</span><span class="sxs-lookup"><span data-stu-id="b2c88-736">To help user figure out the closest best resource class for the memory intensed CCI operations (load, copy table, rebuild index, etc.) on non partitioned CCI table at a given resource class.</span></span> <span data-ttu-id="b2c88-737">A tárolt eljárás tudja meg a szükséges memória biztosítása a következő tábla sémáját használja.</span><span class="sxs-lookup"><span data-stu-id="b2c88-737">The stored proc uses table schema to find out the required memory grant for this.</span></span>

#### <a name="dependencies--restrictions"></a><span data-ttu-id="b2c88-738">Függőségek és korlátozások:</span><span class="sxs-lookup"><span data-stu-id="b2c88-738">Dependencies & Restrictions:</span></span>
- <span data-ttu-id="b2c88-739">A tárolt eljárás nem célja, hogy a tábla particionálva közösségi koordináló intézet memóriakövetelményét kiszámításához.</span><span class="sxs-lookup"><span data-stu-id="b2c88-739">This stored proc is not designed to calculate memory requirement for partitioned-cci table.</span></span>    
- <span data-ttu-id="b2c88-740">A tárolt eljárás nem memóriakövetelményét CTAS/INSERT-VÁLASZTANI SELECT részében figyelembe veszi, és feltételezi, hogy lehet egy egyszerű jelöljön ki.</span><span class="sxs-lookup"><span data-stu-id="b2c88-740">This stored proc doesn't take memory requirement into account for the SELECT part of CTAS/INSERT-SELECT and assumes it to be a simple SELECT.</span></span>
- <span data-ttu-id="b2c88-741">A tárolt eljárás egy ideiglenes táblát használ, így használható a munkamenet hol jött létre a tárolt eljáráshoz.</span><span class="sxs-lookup"><span data-stu-id="b2c88-741">This stored proc uses a temp table so this can be used in the session where this stored proc was created.</span></span>    
- <span data-ttu-id="b2c88-742">A tárolt eljárás attól függ, az aktuális offerings (pl. hardverkonfiguráció, DMS config), és ha bármelyik, amely módosítja majd a tárolt eljárás nem megfelelően fog működni.</span><span class="sxs-lookup"><span data-stu-id="b2c88-742">This stored proc depends on the current offerings (e.g. hardware configuration, DMS config) and if any of that changes then this stored proc would not work correctly.</span></span>  
- <span data-ttu-id="b2c88-743">A tárolt eljárás attól függ, meglévő felajánlott feldolgozási korlátja, és ha megváltozik, majd a tárolt eljárás nem megfelelően fog működni.</span><span class="sxs-lookup"><span data-stu-id="b2c88-743">This stored proc depends on existing offered concurrency limit and if that changes then this stored proc would not work correctly.</span></span>  
- <span data-ttu-id="b2c88-744">A tárolt eljárás attól függ, meglévő erőforrás osztály ajánlatokat, és ha, amely módosítja majd ez tárolása megfelelő proc wuold nem működik.</span><span class="sxs-lookup"><span data-stu-id="b2c88-744">This stored proc depends on existing resource class offerings and if that changes then this stored proc wuold not work correctly.</span></span>  

>  [!NOTE]  
>  <span data-ttu-id="b2c88-745">Ha nem kap kimeneti megadva, előfordulhat, hogy két esetben paraméterekkel tárolt eljárás végrehajtása után.</span><span class="sxs-lookup"><span data-stu-id="b2c88-745">If you are not getting output after executing stored procedure with parameters provided then there could be two cases.</span></span> <br /><span data-ttu-id="b2c88-746">1. Vagy DW paraméter érvénytelen SLO értéket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="b2c88-746">1. Either DW Parameter contains invalid SLO value</span></span> <br /><span data-ttu-id="b2c88-747">2. VAGY nem megfelelő erőforrásosztály közösségi koordináló intézet művelet Ha megadva tábla neve.</span><span class="sxs-lookup"><span data-stu-id="b2c88-747">2. OR there are no matching resource class for CCI operation if table name was provided.</span></span> <br /><span data-ttu-id="b2c88-748">Például: DW100, legmagasabb rendelkezésre memóriaengedély 400MB, és ha táblaséma elég széles kereszt-követelmény 400MB.</span><span class="sxs-lookup"><span data-stu-id="b2c88-748">For example, at DW100, highest memory grant available is 400MB and if table schema is wide enough to cross the requirement of 400MB.</span></span>
      
#### <a name="usage-example"></a><span data-ttu-id="b2c88-749">Példa:</span><span class="sxs-lookup"><span data-stu-id="b2c88-749">Usage example:</span></span>
<span data-ttu-id="b2c88-750">Szintaxis:</span><span class="sxs-lookup"><span data-stu-id="b2c88-750">Syntax:</span></span>  
`EXEC dbo.prc_workload_management_by_DWU @DWU VARCHAR(7), @SCHEMA_NAME VARCHAR(128), @TABLE_NAME VARCHAR(128)`  
1. <span data-ttu-id="b2c88-751">@DWU:Adja meg az aktuális DWU kinyerése az Adatraktár-adatbázisban, vagy bármely támogatott DWU "DW100" formájában adja meg egy NULL értékű paramétert vagy</span><span class="sxs-lookup"><span data-stu-id="b2c88-751">@DWU: Either provide a NULL parameter to extract the current DWU from the DW DB or provide any supported DWU in the form of 'DW100'</span></span>
2. <span data-ttu-id="b2c88-752">@SCHEMA_NAME:Adja meg a tábla a séma neve</span><span class="sxs-lookup"><span data-stu-id="b2c88-752">@SCHEMA_NAME: Provide a schema name of the table</span></span>
3. <span data-ttu-id="b2c88-753">@TABLE_NAME:Adjon meg egy tábla nevét, a fontos</span><span class="sxs-lookup"><span data-stu-id="b2c88-753">@TABLE_NAME: Provide a table name of the interest</span></span>

<span data-ttu-id="b2c88-754">A tárolt eljárás végrehajtása példák:</span><span class="sxs-lookup"><span data-stu-id="b2c88-754">Examples executing this stored proc:</span></span>  
```sql  
EXEC dbo.prc_workload_management_by_DWU 'DW2000', 'dbo', 'Table1';  
EXEC dbo.prc_workload_management_by_DWU NULL, 'dbo', 'Table1';  
EXEC dbo.prc_workload_management_by_DWU 'DW6000', NULL, NULL;  
EXEC dbo.prc_workload_management_by_DWU NULL, NULL, NULL;  
```

<span data-ttu-id="b2c88-755">A fenti példákban használt Table1 sikerült létrehozni a következő:</span><span class="sxs-lookup"><span data-stu-id="b2c88-755">Table1 used in the above examples could be created as below:</span></span>  
`CREATE TABLE Table1 (a int, b varchar(50), c decimal (18,10), d char(10), e varbinary(15), f float, g datetime, h date);`

#### <a name="heres-the-stored-procedure-definition"></a><span data-ttu-id="b2c88-756">Ez a tárolt eljárás definíciója:</span><span class="sxs-lookup"><span data-stu-id="b2c88-756">Here's the stored procedure definition:</span></span>
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
-- Selecting proper DWU for the current DB if not specified.
SET @DWU = (
  SELECT 'DW'+CAST(COUNT(*)*100 AS VARCHAR(10))
  FROM sys.dm_pdw_nodes
  WHERE type = 'COMPUTE')
END

DECLARE @DWU_NUM INT
SET @DWU_NUM = CAST (SUBSTRING(@DWU, 3, LEN(@DWU)-2) AS INT)

-- Raise error if either schema name or table name is supplied but not both them supplied
--IF ((@SCHEMA_NAME IS NOT NULL AND @TABLE_NAME IS NULL) OR (@TABLE_NAME IS NULL AND @SCHEMA_NAME IS NOT NULL))
--     RAISEERROR('User need to supply either both Schema Name and Table Name or none of them')
       
-- Dropping temp table if exists.
IF OBJECT_ID('tempdb..#ref') IS NOT NULL
BEGIN
  DROP TABLE #ref;
END

-- Creating ref. temptable (CTAS) to hold mapping info.
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
-- Creating workload mapping to their corresponding slot consumption and default memory grant.
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

## <a name="query-importance"></a><span data-ttu-id="b2c88-757">Lekérdezés fontossága</span><span class="sxs-lookup"><span data-stu-id="b2c88-757">Query importance</span></span>
<span data-ttu-id="b2c88-758">Az SQL Data Warehouse erőforrás osztályok munkaterhelés-csoport használatával valósít meg.</span><span class="sxs-lookup"><span data-stu-id="b2c88-758">SQL Data Warehouse implements resource classes by using workload groups.</span></span> <span data-ttu-id="b2c88-759">Nincsenek összesen nyolc munkaterhelés-csoport, amely felügyeli az erőforrás osztályok között a különböző DWU méretű.</span><span class="sxs-lookup"><span data-stu-id="b2c88-759">There are a total of eight workload groups that control the behavior of the resource classes across the various DWU sizes.</span></span> <span data-ttu-id="b2c88-760">A DWU az SQL Data Warehouse csak közül négyet használ a nyolc munkaterhelés-csoport.</span><span class="sxs-lookup"><span data-stu-id="b2c88-760">For any DWU, SQL Data Warehouse uses only four of the eight workload groups.</span></span> <span data-ttu-id="b2c88-761">Ez teljesen logikus, mert egyes tevékenységprofil-csoport van rendelve egy négy erőforrás osztályok: smallrc, mediumrc, largerc, vagy xlargerc.</span><span class="sxs-lookup"><span data-stu-id="b2c88-761">This makes sense because each workload group is assigned to one of four resource classes: smallrc, mediumrc, largerc, or xlargerc.</span></span> <span data-ttu-id="b2c88-762">A munkaterhelés-csoport ismertetése fontosságát az, hogy a munkaterhelés csoportok vannak beállítva a magasabb *fontossági*.</span><span class="sxs-lookup"><span data-stu-id="b2c88-762">The importance of understanding the workload groups is that some of these workload groups are set to higher *importance*.</span></span> <span data-ttu-id="b2c88-763">Fontos használt CPU ütemezés.</span><span class="sxs-lookup"><span data-stu-id="b2c88-763">Importance is used for CPU scheduling.</span></span> <span data-ttu-id="b2c88-764">A nagyon fontos futtatása lekérdezések háromszor több, mint a közepes fontos CPU-ciklusok fogja kapni.</span><span class="sxs-lookup"><span data-stu-id="b2c88-764">Queries run with high importance will get three times more CPU cycles than those with medium importance.</span></span> <span data-ttu-id="b2c88-765">Ezért a feldolgozási tárolóhely hozzárendelések is CPU prioritásának meghatározása.</span><span class="sxs-lookup"><span data-stu-id="b2c88-765">Therefore, concurrency slot mappings also determine CPU priority.</span></span> <span data-ttu-id="b2c88-766">Lekérdezés 16 vagy több üzembe helyezési ponti használ fel, ha fut, nagyon fontos.</span><span class="sxs-lookup"><span data-stu-id="b2c88-766">When a query consumes 16 or more slots, it runs as high importance.</span></span>

<span data-ttu-id="b2c88-767">Az alábbi táblázat az egyes tevékenységprofil-csoport fontosság leképezéseit.</span><span class="sxs-lookup"><span data-stu-id="b2c88-767">The following table shows the importance mappings for each workload group.</span></span>

### <a name="workload-group-mappings-to-concurrency-slots-and-importance"></a><span data-ttu-id="b2c88-768">Párhuzamossági tárhelyek és a fontosság munkaterhelési csoport leképezései</span><span class="sxs-lookup"><span data-stu-id="b2c88-768">Workload group mappings to concurrency slots and importance</span></span>
| <span data-ttu-id="b2c88-769">Munkaterhelés-csoport</span><span class="sxs-lookup"><span data-stu-id="b2c88-769">Workload groups</span></span> | <span data-ttu-id="b2c88-770">Párhuzamossági tárolóhely leképezése</span><span class="sxs-lookup"><span data-stu-id="b2c88-770">Concurrency slot mapping</span></span> | <span data-ttu-id="b2c88-771">MB / terjesztési</span><span class="sxs-lookup"><span data-stu-id="b2c88-771">MB / Distribution</span></span> | <span data-ttu-id="b2c88-772">Fontos leképezése</span><span class="sxs-lookup"><span data-stu-id="b2c88-772">Importance mapping</span></span> |
|:--- |:---:|:---:|:--- |
| <span data-ttu-id="b2c88-773">SloDWGroupC00</span><span class="sxs-lookup"><span data-stu-id="b2c88-773">SloDWGroupC00</span></span> |<span data-ttu-id="b2c88-774">1</span><span class="sxs-lookup"><span data-stu-id="b2c88-774">1</span></span> |<span data-ttu-id="b2c88-775">100</span><span class="sxs-lookup"><span data-stu-id="b2c88-775">100</span></span> |<span data-ttu-id="b2c88-776">Közepes</span><span class="sxs-lookup"><span data-stu-id="b2c88-776">Medium</span></span> |
| <span data-ttu-id="b2c88-777">SloDWGroupC01</span><span class="sxs-lookup"><span data-stu-id="b2c88-777">SloDWGroupC01</span></span> |<span data-ttu-id="b2c88-778">2</span><span class="sxs-lookup"><span data-stu-id="b2c88-778">2</span></span> |<span data-ttu-id="b2c88-779">200</span><span class="sxs-lookup"><span data-stu-id="b2c88-779">200</span></span> |<span data-ttu-id="b2c88-780">Közepes</span><span class="sxs-lookup"><span data-stu-id="b2c88-780">Medium</span></span> |
| <span data-ttu-id="b2c88-781">SloDWGroupC02</span><span class="sxs-lookup"><span data-stu-id="b2c88-781">SloDWGroupC02</span></span> |<span data-ttu-id="b2c88-782">4</span><span class="sxs-lookup"><span data-stu-id="b2c88-782">4</span></span> |<span data-ttu-id="b2c88-783">400</span><span class="sxs-lookup"><span data-stu-id="b2c88-783">400</span></span> |<span data-ttu-id="b2c88-784">Közepes</span><span class="sxs-lookup"><span data-stu-id="b2c88-784">Medium</span></span> |
| <span data-ttu-id="b2c88-785">SloDWGroupC03</span><span class="sxs-lookup"><span data-stu-id="b2c88-785">SloDWGroupC03</span></span> |<span data-ttu-id="b2c88-786">8</span><span class="sxs-lookup"><span data-stu-id="b2c88-786">8</span></span> |<span data-ttu-id="b2c88-787">800</span><span class="sxs-lookup"><span data-stu-id="b2c88-787">800</span></span> |<span data-ttu-id="b2c88-788">Közepes</span><span class="sxs-lookup"><span data-stu-id="b2c88-788">Medium</span></span> |
| <span data-ttu-id="b2c88-789">SloDWGroupC04</span><span class="sxs-lookup"><span data-stu-id="b2c88-789">SloDWGroupC04</span></span> |<span data-ttu-id="b2c88-790">16</span><span class="sxs-lookup"><span data-stu-id="b2c88-790">16</span></span> |<span data-ttu-id="b2c88-791">1,600</span><span class="sxs-lookup"><span data-stu-id="b2c88-791">1,600</span></span> |<span data-ttu-id="b2c88-792">Magas</span><span class="sxs-lookup"><span data-stu-id="b2c88-792">High</span></span> |
| <span data-ttu-id="b2c88-793">SloDWGroupC05</span><span class="sxs-lookup"><span data-stu-id="b2c88-793">SloDWGroupC05</span></span> |<span data-ttu-id="b2c88-794">32</span><span class="sxs-lookup"><span data-stu-id="b2c88-794">32</span></span> |<span data-ttu-id="b2c88-795">3,200</span><span class="sxs-lookup"><span data-stu-id="b2c88-795">3,200</span></span> |<span data-ttu-id="b2c88-796">Magas</span><span class="sxs-lookup"><span data-stu-id="b2c88-796">High</span></span> |
| <span data-ttu-id="b2c88-797">SloDWGroupC06</span><span class="sxs-lookup"><span data-stu-id="b2c88-797">SloDWGroupC06</span></span> |<span data-ttu-id="b2c88-798">64</span><span class="sxs-lookup"><span data-stu-id="b2c88-798">64</span></span> |<span data-ttu-id="b2c88-799">6,400</span><span class="sxs-lookup"><span data-stu-id="b2c88-799">6,400</span></span> |<span data-ttu-id="b2c88-800">Magas</span><span class="sxs-lookup"><span data-stu-id="b2c88-800">High</span></span> |
| <span data-ttu-id="b2c88-801">SloDWGroupC07</span><span class="sxs-lookup"><span data-stu-id="b2c88-801">SloDWGroupC07</span></span> |<span data-ttu-id="b2c88-802">128</span><span class="sxs-lookup"><span data-stu-id="b2c88-802">128</span></span> |<span data-ttu-id="b2c88-803">12,800</span><span class="sxs-lookup"><span data-stu-id="b2c88-803">12,800</span></span> |<span data-ttu-id="b2c88-804">Magas</span><span class="sxs-lookup"><span data-stu-id="b2c88-804">High</span></span> |

<span data-ttu-id="b2c88-805">Az a **foglalási és felhasználási párhuzamossági tárolóhelyek** diagram, ellenőrizheti, hogy egy DW500 használ 1, 4, 8, vagy 16 párhuzamossági üzembe helyezési ponti smallrc, mediumrc, largerc és xlargerc, illetve.</span><span class="sxs-lookup"><span data-stu-id="b2c88-805">From the **Allocation and consumption of concurrency slots** chart, you can see that a DW500 uses 1, 4, 8 or 16 concurrency slots for smallrc, mediumrc, largerc, and xlargerc, respectively.</span></span> <span data-ttu-id="b2c88-806">Megtekintheti ezeket az értékeket az előző táblázat az egyes erőforrás fontossága kereséséhez.</span><span class="sxs-lookup"><span data-stu-id="b2c88-806">You can look those values up in the preceding chart to find the importance for each resource class.</span></span>

### <a name="dw500-mapping-of-resource-classes-to-importance"></a><span data-ttu-id="b2c88-807">Az erőforrás fontossága osztályok DW500 leképezése</span><span class="sxs-lookup"><span data-stu-id="b2c88-807">DW500 mapping of resource classes to importance</span></span>
| <span data-ttu-id="b2c88-808">Erőforrásosztály</span><span class="sxs-lookup"><span data-stu-id="b2c88-808">Resource class</span></span> | <span data-ttu-id="b2c88-809">A tevékenységprofil-csoport</span><span class="sxs-lookup"><span data-stu-id="b2c88-809">Workload group</span></span> | <span data-ttu-id="b2c88-810">Párhuzamossági tárhelyek használt</span><span class="sxs-lookup"><span data-stu-id="b2c88-810">Concurrency slots used</span></span> | <span data-ttu-id="b2c88-811">MB / terjesztési</span><span class="sxs-lookup"><span data-stu-id="b2c88-811">MB / Distribution</span></span> | <span data-ttu-id="b2c88-812">Fontos</span><span class="sxs-lookup"><span data-stu-id="b2c88-812">Importance</span></span> |
|:--- |:--- |:---:|:---:|:--- |
| <span data-ttu-id="b2c88-813">smallrc</span><span class="sxs-lookup"><span data-stu-id="b2c88-813">smallrc</span></span> |<span data-ttu-id="b2c88-814">SloDWGroupC00</span><span class="sxs-lookup"><span data-stu-id="b2c88-814">SloDWGroupC00</span></span> |<span data-ttu-id="b2c88-815">1</span><span class="sxs-lookup"><span data-stu-id="b2c88-815">1</span></span> |<span data-ttu-id="b2c88-816">100</span><span class="sxs-lookup"><span data-stu-id="b2c88-816">100</span></span> |<span data-ttu-id="b2c88-817">Közepes</span><span class="sxs-lookup"><span data-stu-id="b2c88-817">Medium</span></span> |
| <span data-ttu-id="b2c88-818">mediumrc</span><span class="sxs-lookup"><span data-stu-id="b2c88-818">mediumrc</span></span> |<span data-ttu-id="b2c88-819">SloDWGroupC02</span><span class="sxs-lookup"><span data-stu-id="b2c88-819">SloDWGroupC02</span></span> |<span data-ttu-id="b2c88-820">4</span><span class="sxs-lookup"><span data-stu-id="b2c88-820">4</span></span> |<span data-ttu-id="b2c88-821">400</span><span class="sxs-lookup"><span data-stu-id="b2c88-821">400</span></span> |<span data-ttu-id="b2c88-822">Közepes</span><span class="sxs-lookup"><span data-stu-id="b2c88-822">Medium</span></span> |
| <span data-ttu-id="b2c88-823">largerc</span><span class="sxs-lookup"><span data-stu-id="b2c88-823">largerc</span></span> |<span data-ttu-id="b2c88-824">SloDWGroupC03</span><span class="sxs-lookup"><span data-stu-id="b2c88-824">SloDWGroupC03</span></span> |<span data-ttu-id="b2c88-825">8</span><span class="sxs-lookup"><span data-stu-id="b2c88-825">8</span></span> |<span data-ttu-id="b2c88-826">800</span><span class="sxs-lookup"><span data-stu-id="b2c88-826">800</span></span> |<span data-ttu-id="b2c88-827">Közepes</span><span class="sxs-lookup"><span data-stu-id="b2c88-827">Medium</span></span> |
| <span data-ttu-id="b2c88-828">xlargerc</span><span class="sxs-lookup"><span data-stu-id="b2c88-828">xlargerc</span></span> |<span data-ttu-id="b2c88-829">SloDWGroupC04</span><span class="sxs-lookup"><span data-stu-id="b2c88-829">SloDWGroupC04</span></span> |<span data-ttu-id="b2c88-830">16</span><span class="sxs-lookup"><span data-stu-id="b2c88-830">16</span></span> |<span data-ttu-id="b2c88-831">1,600</span><span class="sxs-lookup"><span data-stu-id="b2c88-831">1,600</span></span> |<span data-ttu-id="b2c88-832">Magas</span><span class="sxs-lookup"><span data-stu-id="b2c88-832">High</span></span> |
| <span data-ttu-id="b2c88-833">staticrc10</span><span class="sxs-lookup"><span data-stu-id="b2c88-833">staticrc10</span></span> |<span data-ttu-id="b2c88-834">SloDWGroupC00</span><span class="sxs-lookup"><span data-stu-id="b2c88-834">SloDWGroupC00</span></span> |<span data-ttu-id="b2c88-835">1</span><span class="sxs-lookup"><span data-stu-id="b2c88-835">1</span></span> |<span data-ttu-id="b2c88-836">100</span><span class="sxs-lookup"><span data-stu-id="b2c88-836">100</span></span> |<span data-ttu-id="b2c88-837">Közepes</span><span class="sxs-lookup"><span data-stu-id="b2c88-837">Medium</span></span> |
| <span data-ttu-id="b2c88-838">staticrc20</span><span class="sxs-lookup"><span data-stu-id="b2c88-838">staticrc20</span></span> |<span data-ttu-id="b2c88-839">SloDWGroupC01</span><span class="sxs-lookup"><span data-stu-id="b2c88-839">SloDWGroupC01</span></span> |<span data-ttu-id="b2c88-840">2</span><span class="sxs-lookup"><span data-stu-id="b2c88-840">2</span></span> |<span data-ttu-id="b2c88-841">200</span><span class="sxs-lookup"><span data-stu-id="b2c88-841">200</span></span> |<span data-ttu-id="b2c88-842">Közepes</span><span class="sxs-lookup"><span data-stu-id="b2c88-842">Medium</span></span> |
| <span data-ttu-id="b2c88-843">staticrc30</span><span class="sxs-lookup"><span data-stu-id="b2c88-843">staticrc30</span></span> |<span data-ttu-id="b2c88-844">SloDWGroupC02</span><span class="sxs-lookup"><span data-stu-id="b2c88-844">SloDWGroupC02</span></span> |<span data-ttu-id="b2c88-845">4</span><span class="sxs-lookup"><span data-stu-id="b2c88-845">4</span></span> |<span data-ttu-id="b2c88-846">400</span><span class="sxs-lookup"><span data-stu-id="b2c88-846">400</span></span> |<span data-ttu-id="b2c88-847">Közepes</span><span class="sxs-lookup"><span data-stu-id="b2c88-847">Medium</span></span> |
| <span data-ttu-id="b2c88-848">staticrc40</span><span class="sxs-lookup"><span data-stu-id="b2c88-848">staticrc40</span></span> |<span data-ttu-id="b2c88-849">SloDWGroupC03</span><span class="sxs-lookup"><span data-stu-id="b2c88-849">SloDWGroupC03</span></span> |<span data-ttu-id="b2c88-850">8</span><span class="sxs-lookup"><span data-stu-id="b2c88-850">8</span></span> |<span data-ttu-id="b2c88-851">800</span><span class="sxs-lookup"><span data-stu-id="b2c88-851">800</span></span> |<span data-ttu-id="b2c88-852">Közepes</span><span class="sxs-lookup"><span data-stu-id="b2c88-852">Medium</span></span> |
| <span data-ttu-id="b2c88-853">staticrc50</span><span class="sxs-lookup"><span data-stu-id="b2c88-853">staticrc50</span></span> |<span data-ttu-id="b2c88-854">SloDWGroupC03</span><span class="sxs-lookup"><span data-stu-id="b2c88-854">SloDWGroupC03</span></span> |<span data-ttu-id="b2c88-855">16</span><span class="sxs-lookup"><span data-stu-id="b2c88-855">16</span></span> |<span data-ttu-id="b2c88-856">1,600</span><span class="sxs-lookup"><span data-stu-id="b2c88-856">1,600</span></span> |<span data-ttu-id="b2c88-857">Magas</span><span class="sxs-lookup"><span data-stu-id="b2c88-857">High</span></span> |
| <span data-ttu-id="b2c88-858">staticrc60</span><span class="sxs-lookup"><span data-stu-id="b2c88-858">staticrc60</span></span> |<span data-ttu-id="b2c88-859">SloDWGroupC03</span><span class="sxs-lookup"><span data-stu-id="b2c88-859">SloDWGroupC03</span></span> |<span data-ttu-id="b2c88-860">16</span><span class="sxs-lookup"><span data-stu-id="b2c88-860">16</span></span> |<span data-ttu-id="b2c88-861">1,600</span><span class="sxs-lookup"><span data-stu-id="b2c88-861">1,600</span></span> |<span data-ttu-id="b2c88-862">Magas</span><span class="sxs-lookup"><span data-stu-id="b2c88-862">High</span></span> |
| <span data-ttu-id="b2c88-863">staticrc70</span><span class="sxs-lookup"><span data-stu-id="b2c88-863">staticrc70</span></span> |<span data-ttu-id="b2c88-864">SloDWGroupC03</span><span class="sxs-lookup"><span data-stu-id="b2c88-864">SloDWGroupC03</span></span> |<span data-ttu-id="b2c88-865">16</span><span class="sxs-lookup"><span data-stu-id="b2c88-865">16</span></span> |<span data-ttu-id="b2c88-866">1,600</span><span class="sxs-lookup"><span data-stu-id="b2c88-866">1,600</span></span> |<span data-ttu-id="b2c88-867">Magas</span><span class="sxs-lookup"><span data-stu-id="b2c88-867">High</span></span> |
| <span data-ttu-id="b2c88-868">staticrc80</span><span class="sxs-lookup"><span data-stu-id="b2c88-868">staticrc80</span></span> |<span data-ttu-id="b2c88-869">SloDWGroupC03</span><span class="sxs-lookup"><span data-stu-id="b2c88-869">SloDWGroupC03</span></span> |<span data-ttu-id="b2c88-870">16</span><span class="sxs-lookup"><span data-stu-id="b2c88-870">16</span></span> |<span data-ttu-id="b2c88-871">1,600</span><span class="sxs-lookup"><span data-stu-id="b2c88-871">1,600</span></span> |<span data-ttu-id="b2c88-872">Magas</span><span class="sxs-lookup"><span data-stu-id="b2c88-872">High</span></span> |

<span data-ttu-id="b2c88-873">A következő DMV-lekérdezés is használhatja, nézze meg a memória erőforrás-elosztás részletesen különbségek szempontjából az erőforrás-vezérlő, illetve elemzése a munkaterhelés-csoport aktív és előzménynaplók használata esetén végzett hibaelhárításhoz.</span><span class="sxs-lookup"><span data-stu-id="b2c88-873">You can use the following DMV query to look at the differences in memory resource allocation in detail from the perspective of the resource governor, or to analyze active and historic usage of the workload groups when troubleshooting.</span></span>

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

## <a name="queries-that-honor-concurrency-limits"></a><span data-ttu-id="b2c88-874">Feldolgozási korlátok tiszteletben lekérdezések</span><span class="sxs-lookup"><span data-stu-id="b2c88-874">Queries that honor concurrency limits</span></span>
<span data-ttu-id="b2c88-875">A legtöbb lekérdezések erőforrás osztályok vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="b2c88-875">Most queries are governed by resource classes.</span></span> <span data-ttu-id="b2c88-876">Ezeket a lekérdezéseket is a párhuzamos lekérdezés és feldolgozási tárolóhely küszöbértékek kell férnie.</span><span class="sxs-lookup"><span data-stu-id="b2c88-876">These queries must fit inside both the concurrent query and concurrency slot thresholds.</span></span> <span data-ttu-id="b2c88-877">A felhasználó nem választható, egy lekérdezés kizárását a párhuzamossági tárolóhely modell.</span><span class="sxs-lookup"><span data-stu-id="b2c88-877">A user cannot choose to exclude a query from the concurrency slot model.</span></span>

<span data-ttu-id="b2c88-878">A következő utasításokat megújítja, tiszteletben az erőforrás-osztályok:</span><span class="sxs-lookup"><span data-stu-id="b2c88-878">To reiterate, the following statements honor resource classes:</span></span>

* <span data-ttu-id="b2c88-879">INSERT-KIVÁLASZTÁSA</span><span class="sxs-lookup"><span data-stu-id="b2c88-879">INSERT-SELECT</span></span>
* <span data-ttu-id="b2c88-880">FRISSÍTÉS</span><span class="sxs-lookup"><span data-stu-id="b2c88-880">UPDATE</span></span>
* <span data-ttu-id="b2c88-881">TÖRLÉSE</span><span class="sxs-lookup"><span data-stu-id="b2c88-881">DELETE</span></span>
* <span data-ttu-id="b2c88-882">Válassza ki a (felhasználói táblák lekérdezésekor)</span><span class="sxs-lookup"><span data-stu-id="b2c88-882">SELECT (when querying user tables)</span></span>
* <span data-ttu-id="b2c88-883">ALTER INDEX REBUILD</span><span class="sxs-lookup"><span data-stu-id="b2c88-883">ALTER INDEX REBUILD</span></span>
* <span data-ttu-id="b2c88-884">ALTER INDEX ÁTSZERVEZ</span><span class="sxs-lookup"><span data-stu-id="b2c88-884">ALTER INDEX REORGANIZE</span></span>
* <span data-ttu-id="b2c88-885">ALTER TABLE REBUILD</span><span class="sxs-lookup"><span data-stu-id="b2c88-885">ALTER TABLE REBUILD</span></span>
* <span data-ttu-id="b2c88-886">INDEX LÉTREHOZÁSA</span><span class="sxs-lookup"><span data-stu-id="b2c88-886">CREATE INDEX</span></span>
* <span data-ttu-id="b2c88-887">HOZZON LÉTRE FÜRTÖZÖTT OSZLOPCENTRIKUS INDEXET</span><span class="sxs-lookup"><span data-stu-id="b2c88-887">CREATE CLUSTERED COLUMNSTORE INDEX</span></span>
* <span data-ttu-id="b2c88-888">TABLE AS SELECT (CTAS) LÉTREHOZÁSA</span><span class="sxs-lookup"><span data-stu-id="b2c88-888">CREATE TABLE AS SELECT (CTAS)</span></span>
* <span data-ttu-id="b2c88-889">Az adatok betöltése</span><span class="sxs-lookup"><span data-stu-id="b2c88-889">Data loading</span></span>
* <span data-ttu-id="b2c88-890">Az adatátviteli műveletek elvégzése az adatok adatátviteli szolgáltatás (DMS)</span><span class="sxs-lookup"><span data-stu-id="b2c88-890">Data movement operations conducted by the Data Movement Service (DMS)</span></span>

## <a name="query-exceptions-to-concurrency-limits"></a><span data-ttu-id="b2c88-891">Lekérdezés kivételek feldolgozási korlátok</span><span class="sxs-lookup"><span data-stu-id="b2c88-891">Query exceptions to concurrency limits</span></span>
<span data-ttu-id="b2c88-892">Egyes lekérdezések nem fogadják el a erőforrásosztály, amelyhez a felhasználó hozzá van rendelve.</span><span class="sxs-lookup"><span data-stu-id="b2c88-892">Some queries do not honor the resource class to which the user is assigned.</span></span> <span data-ttu-id="b2c88-893">Ezeket a kivételeket a feldolgozási korlátok válnak, amikor egy adott parancshoz szükséges memória-erőforrások terhelése alacsony, gyakran mivel a parancs a metaadat-művelet.</span><span class="sxs-lookup"><span data-stu-id="b2c88-893">These exceptions to the concurrency limits are made when the memory resources needed for a particular command are low, often because the command is a metadata operation.</span></span> <span data-ttu-id="b2c88-894">Az ilyen kivételek célja lekérdezések, amelyek soha nem kell őket nagyobb memória-foglalásának elkerülése érdekében.</span><span class="sxs-lookup"><span data-stu-id="b2c88-894">The goal of these exceptions is to avoid larger memory allocations for queries that will never need them.</span></span> <span data-ttu-id="b2c88-895">A felhasználóhoz rendelt tényleges erőforrásosztály függetlenül mindig használatra a ezekben az esetekben az alapértelmezett kis erőforrásosztály (smallrc).</span><span class="sxs-lookup"><span data-stu-id="b2c88-895">In these cases, the default small resource class (smallrc) is always used regardless of the actual resource class assigned to the user.</span></span> <span data-ttu-id="b2c88-896">Például `CREATE LOGIN` mindig smallrc fog futni.</span><span class="sxs-lookup"><span data-stu-id="b2c88-896">For example, `CREATE LOGIN` will always run in smallrc.</span></span> <span data-ttu-id="b2c88-897">Ez a művelet teljesítéséhez szükséges erőforrások nagyon alacsony, így nem célszerű a lekérdezés tartalmazza a feldolgozási tárolóhely modellben.</span><span class="sxs-lookup"><span data-stu-id="b2c88-897">The resources required to fulfill this operation are very low, so it does not make sense to include the query in the concurrency slot model.</span></span>  <span data-ttu-id="b2c88-898">Ezeket a lekérdezéseket is nem korlátozza a 32 felhasználói feldolgozási korlát, korlátlan számú ezeket a lekérdezéseket is futtathat a munkamenet legfeljebb 1024 munkamenetek.</span><span class="sxs-lookup"><span data-stu-id="b2c88-898">These queries are also not limited by the 32 user concurrency limit, an unlimited number of these queries can run up to the session limit of 1,024 sessions.</span></span>

<span data-ttu-id="b2c88-899">A következő utasítás nem fogadják el az erőforrás-osztályok:</span><span class="sxs-lookup"><span data-stu-id="b2c88-899">The following statements do not honor resource classes:</span></span>

* <span data-ttu-id="b2c88-900">DROP TABLE vagy létrehozása</span><span class="sxs-lookup"><span data-stu-id="b2c88-900">CREATE or DROP TABLE</span></span>
* <span data-ttu-id="b2c88-901">AZ ALTER TABLE... KAPCSOLÓ, a megosztott vagy a partíció EGYESÍTÉSE</span><span class="sxs-lookup"><span data-stu-id="b2c88-901">ALTER TABLE ... SWITCH, SPLIT, or MERGE PARTITION</span></span>
* <span data-ttu-id="b2c88-902">AZ ALTER INDEX LETILTÁSA</span><span class="sxs-lookup"><span data-stu-id="b2c88-902">ALTER INDEX DISABLE</span></span>
* <span data-ttu-id="b2c88-903">A DROP INDEX</span><span class="sxs-lookup"><span data-stu-id="b2c88-903">DROP INDEX</span></span>
* <span data-ttu-id="b2c88-904">LÉTREHOZÁSI, frissítési vagy a DROP STATISTICS</span><span class="sxs-lookup"><span data-stu-id="b2c88-904">CREATE, UPDATE, or DROP STATISTICS</span></span>
* <span data-ttu-id="b2c88-905">A TRUNCATE TABLE</span><span class="sxs-lookup"><span data-stu-id="b2c88-905">TRUNCATE TABLE</span></span>
* <span data-ttu-id="b2c88-906">AZ ALTER ENGEDÉLYEZÉSI</span><span class="sxs-lookup"><span data-stu-id="b2c88-906">ALTER AUTHORIZATION</span></span>
* <span data-ttu-id="b2c88-907">BEJELENTKEZÉS LÉTREHOZÁSA</span><span class="sxs-lookup"><span data-stu-id="b2c88-907">CREATE LOGIN</span></span>
* <span data-ttu-id="b2c88-908">CREATE, ALTER és a DROP USER</span><span class="sxs-lookup"><span data-stu-id="b2c88-908">CREATE, ALTER or DROP USER</span></span>
* <span data-ttu-id="b2c88-909">CREATE, ALTER vagy DROP ELJÁRÁST</span><span class="sxs-lookup"><span data-stu-id="b2c88-909">CREATE, ALTER or DROP PROCEDURE</span></span>
* <span data-ttu-id="b2c88-910">Vagy DROP NÉZET létrehozása</span><span class="sxs-lookup"><span data-stu-id="b2c88-910">CREATE or DROP VIEW</span></span>
* <span data-ttu-id="b2c88-911">ÉRTÉKEK BESZÚRÁSA</span><span class="sxs-lookup"><span data-stu-id="b2c88-911">INSERT VALUES</span></span>
* <span data-ttu-id="b2c88-912">Válassza ki a rendszer nézetek és dinamikus felügyeleti nézetek</span><span class="sxs-lookup"><span data-stu-id="b2c88-912">SELECT from system views and DMVs</span></span>
* <span data-ttu-id="b2c88-913">MAGYARÁZÓ</span><span class="sxs-lookup"><span data-stu-id="b2c88-913">EXPLAIN</span></span>
* <span data-ttu-id="b2c88-914">DBCC</span><span class="sxs-lookup"><span data-stu-id="b2c88-914">DBCC</span></span>

<!--
Removed as these two are not confirmed / supported under SQLDW
- CREATE REMOTE TABLE AS SELECT
- CREATE EXTERNAL TABLE AS SELECT
- REDISTRIBUTE
-->

##  <span data-ttu-id="b2c88-915"><a name="changing-user-resource-class-example"></a>A felhasználói erőforrás osztály példa módosítása</span><span class="sxs-lookup"><span data-stu-id="b2c88-915"><a name="changing-user-resource-class-example"></a> Change a user resource class example</span></span>
1. <span data-ttu-id="b2c88-916">**Hozzon létre bejelentkezési:** kapcsolatot létesíteni a **fő** adatbázis az SQL Data Warehouse-adatbázist futtató SQL-kiszolgálón, és a következő parancsok.</span><span class="sxs-lookup"><span data-stu-id="b2c88-916">**Create login:** Open a connection to your **master** database on the SQL server hosting your SQL Data Warehouse database and execute the following commands.</span></span>
   
    ```sql
    CREATE LOGIN newperson WITH PASSWORD = 'mypassword';
    CREATE USER newperson for LOGIN newperson;
    ```
   
   > [!NOTE]
   > <span data-ttu-id="b2c88-917">Célszerű egy felhasználó létrehozása az Azure SQL Data Warehouse-felhasználók számára a fő adatbázist.</span><span class="sxs-lookup"><span data-stu-id="b2c88-917">It is a good idea to create a user in the master database for Azure SQL Data Warehouse users.</span></span> <span data-ttu-id="b2c88-918">A felhasználó létrehozása a fő lehetővé teszi a felhasználóknak például az SSMS használatával adatbázis nevének megadása nélkül.</span><span class="sxs-lookup"><span data-stu-id="b2c88-918">Creating a user in master allows a user to login using tools like SSMS without specifying a database name.</span></span>  <span data-ttu-id="b2c88-919">Emellett lehetővé teszi az object explorer segítségével megtekintheti az összes adatbázis SQL-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="b2c88-919">It also allows them to use the object explorer to view all databases on a SQL server.</span></span>  <span data-ttu-id="b2c88-920">További létrehozásával és felhasználók kezelésével kapcsolatos további információkért lásd: [az SQL Data Warehouse adatbázis védelme][Secure a database in SQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="b2c88-920">For more details about creating and managing users, see [Secure a database in SQL Data Warehouse][Secure a database in SQL Data Warehouse].</span></span>
   > 
   > 
2. <span data-ttu-id="b2c88-921">**Az SQL Data Warehouse-felhasználó létrehozása:** kapcsolatot létesíteni a **SQL Data Warehouse** adatbázis, és hajtsa végre a következő parancsot.</span><span class="sxs-lookup"><span data-stu-id="b2c88-921">**Create SQL Data Warehouse user:** Open a connection to the **SQL Data Warehouse** database and execute the following command.</span></span>
   
    ```sql
    CREATE USER newperson FOR LOGIN newperson;
    ```
3. <span data-ttu-id="b2c88-922">**Engedélyek:** a következő példa engedélyezi a `CONTROL` a a **SQL Data Warehouse** adatbázis.</span><span class="sxs-lookup"><span data-stu-id="b2c88-922">**Grant permissions:** The following example grants `CONTROL` on the **SQL Data Warehouse** database.</span></span> <span data-ttu-id="b2c88-923">`CONTROL`az adatbázist szintje megegyezik a db_owner az SQL Server kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="b2c88-923">`CONTROL` at the database level is the equivalent of db_owner in SQL Server.</span></span>
   
    ```sql
    GRANT CONTROL ON DATABASE::MySQLDW to newperson;
    ```
4. <span data-ttu-id="b2c88-924">**Erőforrásosztály növelése:** használja a következő lekérdezést a felhasználó egy nagyobb munkaterhelés felügyeleti szerepkörbe való felvételre.</span><span class="sxs-lookup"><span data-stu-id="b2c88-924">**Increase resource class:** Use the following query to add a user to a higher workload management role.</span></span>
   
    ```sql
    EXEC sp_addrolemember 'largerc', 'newperson'
    ```
5. <span data-ttu-id="b2c88-925">**Erőforrásosztály csökkentése:** felhasználó eltávolítása egy munkaterhelés felügyeleti szerepkört a következő lekérdezés segítségével.</span><span class="sxs-lookup"><span data-stu-id="b2c88-925">**Decrease resource class:** Use the following query to remove a user from a workload management role.</span></span>
   
    ```sql
    EXEC sp_droprolemember 'largerc', 'newperson';
    ```
   
   > [!NOTE]
   > <span data-ttu-id="b2c88-926">Nincs lehetőség a felhasználó eltávolítása smallrc.</span><span class="sxs-lookup"><span data-stu-id="b2c88-926">It is not possible to remove a user from smallrc.</span></span>
   > 
   > 

## <a name="queued-query-detection-and-other-dmvs"></a><span data-ttu-id="b2c88-927">Várólistára helyezett lekérdezés felderítését és egyéb dinamikus felügyeleti nézetek</span><span class="sxs-lookup"><span data-stu-id="b2c88-927">Queued query detection and other DMVs</span></span>
<span data-ttu-id="b2c88-928">Használhatja a `sys.dm_pdw_exec_requests` DMV lekérdezések egy feldolgozási sorban várakozó azonosításához.</span><span class="sxs-lookup"><span data-stu-id="b2c88-928">You can use the `sys.dm_pdw_exec_requests` DMV to identify queries that are waiting in a concurrency queue.</span></span> <span data-ttu-id="b2c88-929">Lekérdezi a feldolgozási tárhely állapottal fog rendelkezni várakozással **felfüggesztve**.</span><span class="sxs-lookup"><span data-stu-id="b2c88-929">Queries waiting for a concurrency slot will have a status of **suspended**.</span></span>

```sql
SELECT      r.[request_id]                 AS Request_ID
        ,r.[status]                 AS Request_Status
        ,r.[submit_time]             AS Request_SubmitTime
        ,r.[start_time]                 AS Request_StartTime
        ,DATEDIFF(ms,[submit_time],[start_time]) AS Request_InitiateDuration_ms
        ,r.resource_class                         AS Request_resource_class
FROM    sys.dm_pdw_exec_requests r;
```

<span data-ttu-id="b2c88-930">Alkalmazások és szolgáltatások felügyeleti szerepköre megtekinthetők az `sys.database_principals`.</span><span class="sxs-lookup"><span data-stu-id="b2c88-930">Workload management roles can be viewed with `sys.database_principals`.</span></span>

```sql
SELECT  ro.[name]           AS [db_role_name]
FROM    sys.database_principals ro
WHERE   ro.[type_desc]      = 'DATABASE_ROLE'
AND     ro.[is_fixed_role]  = 0;
```

<span data-ttu-id="b2c88-931">A következő lekérdezés jeleníti meg, hogy melyik szerepkört minden felhasználóhoz hozzá van rendelve.</span><span class="sxs-lookup"><span data-stu-id="b2c88-931">The following query shows which role each user is assigned to.</span></span>

```sql
SELECT     r.name AS role_principal_name
        ,m.name AS member_principal_name
FROM    sys.database_role_members rm
JOIN    sys.database_principals AS r            ON rm.role_principal_id        = r.principal_id
JOIN    sys.database_principals AS m            ON rm.member_principal_id    = m.principal_id
WHERE    r.name IN ('mediumrc','largerc', 'xlargerc');
```

<span data-ttu-id="b2c88-932">Az SQL Data Warehouse rendelkezik várjon típusok a következők:</span><span class="sxs-lookup"><span data-stu-id="b2c88-932">SQL Data Warehouse has the following wait types:</span></span>

* <span data-ttu-id="b2c88-933">**LocalQueriesConcurrencyResourceType**: lekérdezések, amelyek a feldolgozási tárolóhely keretrendszer kívül elhelyezkedik.</span><span class="sxs-lookup"><span data-stu-id="b2c88-933">**LocalQueriesConcurrencyResourceType**: Queries that sit outside of the concurrency slot framework.</span></span> <span data-ttu-id="b2c88-934">DMV lekérdezések és a rendszer funkciókkal, mint például `SELECT @@VERSION` példák a helyi lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="b2c88-934">DMV queries and system functions such as `SELECT @@VERSION` are examples of local queries.</span></span>
* <span data-ttu-id="b2c88-935">**UserConcurrencyResourceType**: egyidejűségi tárolóhely keretein belül elhelyezkedik lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="b2c88-935">**UserConcurrencyResourceType**: Queries that sit inside the concurrency slot framework.</span></span> <span data-ttu-id="b2c88-936">Végfelhasználói táblák lekérdezéseket képviselő példák, amelyek szeretné használni az erőforrástípus.</span><span class="sxs-lookup"><span data-stu-id="b2c88-936">Queries against end-user tables represent examples that would use this resource type.</span></span>
* <span data-ttu-id="b2c88-937">**DmsConcurrencyResourceType**: megvárja-e az adatátviteli műveletek eredő.</span><span class="sxs-lookup"><span data-stu-id="b2c88-937">**DmsConcurrencyResourceType**: Waits resulting from data movement operations.</span></span>
* <span data-ttu-id="b2c88-938">**BackupConcurrencyResourceType**: A várakozás azt jelzi, hogy egy adatbázis biztonsági mentése van folyamatban.</span><span class="sxs-lookup"><span data-stu-id="b2c88-938">**BackupConcurrencyResourceType**: This wait indicates that a database is being backed up.</span></span> <span data-ttu-id="b2c88-939">Az erőforrástípus maximális értéke 1.</span><span class="sxs-lookup"><span data-stu-id="b2c88-939">The maximum value for this resource type is 1.</span></span> <span data-ttu-id="b2c88-940">Ha több biztonsági mentés kért egy időben, a többi várólistájára.</span><span class="sxs-lookup"><span data-stu-id="b2c88-940">If multiple backups have been requested at the same time, the others will queue.</span></span>

<span data-ttu-id="b2c88-941">A `sys.dm_pdw_waits` DMV kérelmet arra vár, hogy milyen erőforrásokat is használható.</span><span class="sxs-lookup"><span data-stu-id="b2c88-941">The `sys.dm_pdw_waits` DMV can be used to see which resources a request is waiting for.</span></span>

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

<span data-ttu-id="b2c88-942">A `sys.dm_pdw_resource_waits` DMV csak egy adott lekérdezésre által felhasznált erőforrások vár jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="b2c88-942">The `sys.dm_pdw_resource_waits` DMV shows only the resource waits consumed by a given query.</span></span> <span data-ttu-id="b2c88-943">Erőforrás várakozási idő csak meg kell adni, szemben a jel várakozási időt, ami az alapul szolgáló SQL-kiszolgálók ütemezni a CPU, a lekérdezés szükséges idő erőforrások Várakozás időt méri.</span><span class="sxs-lookup"><span data-stu-id="b2c88-943">Resource wait time only measures the time waiting for resources to be provided, as opposed to signal wait time, which is the time it takes for the underlying SQL servers to schedule the query onto the CPU.</span></span>

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

<span data-ttu-id="b2c88-944">A `sys.dm_pdw_wait_stats` történelmi trendelemzés vár a DMV is használható.</span><span class="sxs-lookup"><span data-stu-id="b2c88-944">The `sys.dm_pdw_wait_stats` DMV can be used for historic trend analysis of waits.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="b2c88-945">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b2c88-945">Next steps</span></span>
<span data-ttu-id="b2c88-946">Adatbázis-felhasználók és biztonsági kezelésével kapcsolatos további információkért lásd: [az SQL Data Warehouse adatbázis védelme][Secure a database in SQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="b2c88-946">For more information about managing database users and security, see [Secure a database in SQL Data Warehouse][Secure a database in SQL Data Warehouse].</span></span> <span data-ttu-id="b2c88-947">További információ a hogyan nagyobb erőforrás osztályok javíthatja a fürtözött oszlopcentrikus index minőségének, lásd: [szegmens minőségének javítására indexek újraépítése].</span><span class="sxs-lookup"><span data-stu-id="b2c88-947">For more information about how larger resource classes can improve clustered columnstore index quality, see [Rebuilding indexes to improve segment quality].</span></span>

<!--Image references-->

<!--Article references-->
[Secure a database in SQL Data Warehouse]: ./sql-data-warehouse-overview-manage-security.md
[szegmens minőségének javítására indexek újraépítése]: ./sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality
[Secure a database in SQL Data Warehouse]: ./sql-data-warehouse-overview-manage-security.md

<!--MSDN references-->
[Managing Databases and Logins in Azure SQL Database]:https://msdn.microsoft.com/library/azure/ee336235.aspx

<!--Other Web references-->
