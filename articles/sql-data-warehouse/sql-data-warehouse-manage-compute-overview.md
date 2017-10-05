---
title: "Az Azure SQL Data Warehouse (áttekintés) számítási teljesítményt kezelése |} Microsoft Docs"
description: "Az Azure SQL Data Warehouse képességek kibővítési teljesítményét. Horizontális felskálázás dwu-k módosításával vagy és sablonok felfüggesztése és folytatása a számítási erőforrásokat költségek csökkentése érdekében."
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: johnmac
editor: 
ms.assetid: e13a82b0-abfe-429f-ac3c-f2b6789a70c6
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 03/22/2017
ms.author: elbutter
ms.openlocfilehash: abe22f542a79714f6e894870872ee6b76ffe7633
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="manage-compute-power-in-azure-sql-data-warehouse-overview"></a><span data-ttu-id="adaa9-104">Kezelheti a számítási teljesítményt az Azure SQL Data Warehouse (áttekintés)</span><span class="sxs-lookup"><span data-stu-id="adaa9-104">Manage compute power in Azure SQL Data Warehouse (Overview)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="adaa9-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="adaa9-105">Overview</span></span>](sql-data-warehouse-manage-compute-overview.md)
> * [<span data-ttu-id="adaa9-106">Portal</span><span class="sxs-lookup"><span data-stu-id="adaa9-106">Portal</span></span>](sql-data-warehouse-manage-compute-portal.md)
> * [<span data-ttu-id="adaa9-107">PowerShell</span><span class="sxs-lookup"><span data-stu-id="adaa9-107">PowerShell</span></span>](sql-data-warehouse-manage-compute-powershell.md)
> * [<span data-ttu-id="adaa9-108">REST</span><span class="sxs-lookup"><span data-stu-id="adaa9-108">REST</span></span>](sql-data-warehouse-manage-compute-rest-api.md)
> * [<span data-ttu-id="adaa9-109">TSQL</span><span class="sxs-lookup"><span data-stu-id="adaa9-109">TSQL</span></span>](sql-data-warehouse-manage-compute-tsql.md)
>
>

<span data-ttu-id="adaa9-110">Az SQL Data Warehouse architektúrája elválasztja a tárolást és számítást, hogy egymástól független méretezését.</span><span class="sxs-lookup"><span data-stu-id="adaa9-110">The architecture of SQL Data Warehouse separates storage and compute, allowing each to scale independently.</span></span> <span data-ttu-id="adaa9-111">Ennek eredményeképpen számítási is méretezhető teljesítményigények kielégítése függetlenül az adatok mennyiségét.</span><span class="sxs-lookup"><span data-stu-id="adaa9-111">As a result, compute can be scaled to meet performance demands independent of the amount of data.</span></span> <span data-ttu-id="adaa9-112">Ez az architektúra természetes következménye, hogy [számlázási] [ billed] számítási és tárolási elkülönül.</span><span class="sxs-lookup"><span data-stu-id="adaa9-112">A natural consequence of this architecture is that [billing][billed] for compute and storage is separate.</span></span> 

<span data-ttu-id="adaa9-113">Ez az áttekintés bemutatja hogyan működik az SQL Data Warehouse és hogyan használják a szüneteltetési kiterjesztése folytatása, és méretezhető SQL Data Warehouse képességeit.</span><span class="sxs-lookup"><span data-stu-id="adaa9-113">This overview describes how scale out works with SQL Data Warehouse and how to utilize the pause, resume, and scale capabilities of SQL Data Warehouse.</span></span> <span data-ttu-id="adaa9-114">Tekintse át a [az adatraktár-(dwu)] [ data warehouse units (DWUs)] lap megtudhatja, hogyan kapcsolódnak dwu-k és teljesítmény.</span><span class="sxs-lookup"><span data-stu-id="adaa9-114">Consult the [data warehouse units (DWUs)][data warehouse units (DWUs)] page to learn how DWUs and performance are related.</span></span> 

## <a name="how-compute-management-operations-work-in-sql-data-warehouse"></a><span data-ttu-id="adaa9-115">Milyen számítási felügyeleti műveletek a vártnak az SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="adaa9-115">How compute management operations work in SQL Data Warehouse</span></span>
<span data-ttu-id="adaa9-116">Az SQL Data Warehouse architektúrája áll a vezérlő csomópont, a számítási csomópontok és a tárolási réteg 60 terjesztéseket elosztva.</span><span class="sxs-lookup"><span data-stu-id="adaa9-116">The architecture for SQL Data Warehouse consists of a control node, compute nodes, and the storage layer spread across 60 distributions.</span></span> 

<span data-ttu-id="adaa9-117">Egy normál aktív munkamenet során az SQL Data Warehouse a rendszer átjárócsomópont kezeli a metaadatok, és az elosztott lekérdezésoptimalizáló tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="adaa9-117">During a normal active session in SQL Data Warehouse, your system's head node manages the metadata and contains the distributed query optimizer.</span></span> <span data-ttu-id="adaa9-118">A head csomópont alatt a számítási csomópontok és a tárolási rétegből állnak.</span><span class="sxs-lookup"><span data-stu-id="adaa9-118">Beneath this head node are your compute nodes and your storage layer.</span></span> <span data-ttu-id="adaa9-119">A DWU 400 a rendszer egy átjárócsomópont, négy számítási csomópontok és a tárolási rétegből álló 60 terjesztéseket rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="adaa9-119">For a DWU 400, your system has one head node, four compute nodes, and the storage layer, consisting of 60 distributions.</span></span> 

<span data-ttu-id="adaa9-120">A skála változni vagy művelet felfüggeszti, a rendszer először használhatatlanná teszi az összes bejövő lekérdezés, és visszavon tranzakciók konzisztens állapotú biztosításához.</span><span class="sxs-lookup"><span data-stu-id="adaa9-120">When you undergo a scale or pause operation, the system first kills all incoming queries and then rolls back transactions to ensure a consistent state.</span></span> <span data-ttu-id="adaa9-121">A skálázási műveletek, a méretezés akkor történik, a tranzakciós visszaállítás befejezése után.</span><span class="sxs-lookup"><span data-stu-id="adaa9-121">For scale operations, scaling will only occur once this transactional rollback has completed.</span></span> <span data-ttu-id="adaa9-122">Méretezett művelet a rendszer kiosztja a felesleges szükséges száma számítási csomópontokat, és megkezdi a újracsatlakoztatása a számítási csomópontok a tárolási réteg.</span><span class="sxs-lookup"><span data-stu-id="adaa9-122">For a scale-up operation, the system provisions the extra desired number of compute nodes, and then begins reattaching the compute nodes to the storage layer.</span></span> <span data-ttu-id="adaa9-123">Lefelé méretezéshez művelet a felesleges csomópontok kiadott, majd a többi számítási csomópontot újra csatlakoztatja, magukat a megfelelő számú terjesztési.</span><span class="sxs-lookup"><span data-stu-id="adaa9-123">For a scale-down operation, the unneeded nodes are released and the remaining compute nodes reattach themselves to the appropriate number of distributions.</span></span> <span data-ttu-id="adaa9-124">A szüneteltetési művelete minden számítási csomópontok feloldásáig blokkolva lesz, és a rendszer metaadat-művelet a végső rendszer stabil állapotban, hogy számos változni fog.</span><span class="sxs-lookup"><span data-stu-id="adaa9-124">For a pause operation, all compute nodes are released and your system will undergo a variety of metadata operations to leave your final system in a stable state.</span></span>

| <span data-ttu-id="adaa9-125">DWU</span><span class="sxs-lookup"><span data-stu-id="adaa9-125">DWU</span></span>  | <span data-ttu-id="adaa9-126">\#a számítási csomópontok</span><span class="sxs-lookup"><span data-stu-id="adaa9-126">\#of compute nodes</span></span> | <span data-ttu-id="adaa9-127">\#az azokat a terjesztéseket, csomópontonként</span><span class="sxs-lookup"><span data-stu-id="adaa9-127">\# of distributions per node</span></span> |
| ---- | ------------------ | ---------------------------- |
| <span data-ttu-id="adaa9-128">100</span><span class="sxs-lookup"><span data-stu-id="adaa9-128">100</span></span>  | <span data-ttu-id="adaa9-129">1</span><span class="sxs-lookup"><span data-stu-id="adaa9-129">1</span></span>                  | <span data-ttu-id="adaa9-130">60</span><span class="sxs-lookup"><span data-stu-id="adaa9-130">60</span></span>                           |
| <span data-ttu-id="adaa9-131">200</span><span class="sxs-lookup"><span data-stu-id="adaa9-131">200</span></span>  | <span data-ttu-id="adaa9-132">2</span><span class="sxs-lookup"><span data-stu-id="adaa9-132">2</span></span>                  | <span data-ttu-id="adaa9-133">30</span><span class="sxs-lookup"><span data-stu-id="adaa9-133">30</span></span>                           |
| <span data-ttu-id="adaa9-134">300</span><span class="sxs-lookup"><span data-stu-id="adaa9-134">300</span></span>  | <span data-ttu-id="adaa9-135">3</span><span class="sxs-lookup"><span data-stu-id="adaa9-135">3</span></span>                  | <span data-ttu-id="adaa9-136">20</span><span class="sxs-lookup"><span data-stu-id="adaa9-136">20</span></span>                           |
| <span data-ttu-id="adaa9-137">400</span><span class="sxs-lookup"><span data-stu-id="adaa9-137">400</span></span>  | <span data-ttu-id="adaa9-138">4</span><span class="sxs-lookup"><span data-stu-id="adaa9-138">4</span></span>                  | <span data-ttu-id="adaa9-139">15</span><span class="sxs-lookup"><span data-stu-id="adaa9-139">15</span></span>                           |
| <span data-ttu-id="adaa9-140">500</span><span class="sxs-lookup"><span data-stu-id="adaa9-140">500</span></span>  | <span data-ttu-id="adaa9-141">5</span><span class="sxs-lookup"><span data-stu-id="adaa9-141">5</span></span>                  | <span data-ttu-id="adaa9-142">12</span><span class="sxs-lookup"><span data-stu-id="adaa9-142">12</span></span>                           |
| <span data-ttu-id="adaa9-143">600</span><span class="sxs-lookup"><span data-stu-id="adaa9-143">600</span></span>  | <span data-ttu-id="adaa9-144">6</span><span class="sxs-lookup"><span data-stu-id="adaa9-144">6</span></span>                  | <span data-ttu-id="adaa9-145">10</span><span class="sxs-lookup"><span data-stu-id="adaa9-145">10</span></span>                           |
| <span data-ttu-id="adaa9-146">1000</span><span class="sxs-lookup"><span data-stu-id="adaa9-146">1000</span></span> | <span data-ttu-id="adaa9-147">10</span><span class="sxs-lookup"><span data-stu-id="adaa9-147">10</span></span>                 | <span data-ttu-id="adaa9-148">6</span><span class="sxs-lookup"><span data-stu-id="adaa9-148">6</span></span>                            |
| <span data-ttu-id="adaa9-149">1200</span><span class="sxs-lookup"><span data-stu-id="adaa9-149">1200</span></span> | <span data-ttu-id="adaa9-150">12</span><span class="sxs-lookup"><span data-stu-id="adaa9-150">12</span></span>                 | <span data-ttu-id="adaa9-151">5</span><span class="sxs-lookup"><span data-stu-id="adaa9-151">5</span></span>                            |
| <span data-ttu-id="adaa9-152">1500</span><span class="sxs-lookup"><span data-stu-id="adaa9-152">1500</span></span> | <span data-ttu-id="adaa9-153">15</span><span class="sxs-lookup"><span data-stu-id="adaa9-153">15</span></span>                 | <span data-ttu-id="adaa9-154">4</span><span class="sxs-lookup"><span data-stu-id="adaa9-154">4</span></span>                            |
| <span data-ttu-id="adaa9-155">2000</span><span class="sxs-lookup"><span data-stu-id="adaa9-155">2000</span></span> | <span data-ttu-id="adaa9-156">20</span><span class="sxs-lookup"><span data-stu-id="adaa9-156">20</span></span>                 | <span data-ttu-id="adaa9-157">3</span><span class="sxs-lookup"><span data-stu-id="adaa9-157">3</span></span>                            |
| <span data-ttu-id="adaa9-158">3000</span><span class="sxs-lookup"><span data-stu-id="adaa9-158">3000</span></span> | <span data-ttu-id="adaa9-159">30</span><span class="sxs-lookup"><span data-stu-id="adaa9-159">30</span></span>                 | <span data-ttu-id="adaa9-160">2</span><span class="sxs-lookup"><span data-stu-id="adaa9-160">2</span></span>                            |
| <span data-ttu-id="adaa9-161">6000</span><span class="sxs-lookup"><span data-stu-id="adaa9-161">6000</span></span> | <span data-ttu-id="adaa9-162">60</span><span class="sxs-lookup"><span data-stu-id="adaa9-162">60</span></span>                 | <span data-ttu-id="adaa9-163">1</span><span class="sxs-lookup"><span data-stu-id="adaa9-163">1</span></span>                            |

<span data-ttu-id="adaa9-164">A három elsődleges funkciója számítási kezeléséhez a következők:</span><span class="sxs-lookup"><span data-stu-id="adaa9-164">The three primary functions for managing compute are:</span></span>

1. <span data-ttu-id="adaa9-165">Szünet</span><span class="sxs-lookup"><span data-stu-id="adaa9-165">Pause</span></span>
2. <span data-ttu-id="adaa9-166">Folytatás</span><span class="sxs-lookup"><span data-stu-id="adaa9-166">Resume</span></span>
3. <span data-ttu-id="adaa9-167">Méretezés</span><span class="sxs-lookup"><span data-stu-id="adaa9-167">Scale</span></span>

<span data-ttu-id="adaa9-168">Mindkét művelet több percet is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="adaa9-168">Each of these operations may take several minutes to complete.</span></span> <span data-ttu-id="adaa9-169">Ha skálázás/felfüggesztése vagy folytatása automatikusan, érdemes lehet megvalósítani logika győződjön meg arról, hogy bizonyos műveletek befejeződtek-e egy másik művelet folytatása előtt.</span><span class="sxs-lookup"><span data-stu-id="adaa9-169">If you are scaling/pausing/resuming automatically, you may want to implement logic to ensure that certain operations have been completed before proceeding with another action.</span></span> 

<span data-ttu-id="adaa9-170">Az adatbázis állapotát a különböző végpontok ellenőrzése lehetővé teszik megfelelően megvalósítását az ilyen műveletek automatizálását.</span><span class="sxs-lookup"><span data-stu-id="adaa9-170">Checking the database state through various endpoints will allow you to correctly implement automation of such operations.</span></span> <span data-ttu-id="adaa9-171">A portál értesítést küldenek befejezett egy műveletet, és az adatbázisok aktuális állapotát, de nem engedélyezi a állapot ellenőrzéséhez szoftveres.</span><span class="sxs-lookup"><span data-stu-id="adaa9-171">The portal will provide notification upon completion of an operation and the databases current state but does not allow for programmatic checking of state.</span></span> 

>  [!NOTE]
>
>  <span data-ttu-id="adaa9-172">Számítási felügyeleti funkció végpontjai között nem létezik.</span><span class="sxs-lookup"><span data-stu-id="adaa9-172">Compute management functionality does not exist across all endpoints.</span></span>
>
>  

|              | <span data-ttu-id="adaa9-173">Felfüggesztése vagy folytatása</span><span class="sxs-lookup"><span data-stu-id="adaa9-173">Pause/Resume</span></span> | <span data-ttu-id="adaa9-174">Méretezés</span><span class="sxs-lookup"><span data-stu-id="adaa9-174">Scale</span></span> | <span data-ttu-id="adaa9-175">Ellenőrizze az adatbázis állapota</span><span class="sxs-lookup"><span data-stu-id="adaa9-175">Check database state</span></span> |
| ------------ | ------------ | ----- | -------------------- |
| <span data-ttu-id="adaa9-176">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="adaa9-176">Azure portal</span></span> | <span data-ttu-id="adaa9-177">Igen</span><span class="sxs-lookup"><span data-stu-id="adaa9-177">Yes</span></span>          | <span data-ttu-id="adaa9-178">Igen</span><span class="sxs-lookup"><span data-stu-id="adaa9-178">Yes</span></span>   | <span data-ttu-id="adaa9-179">**Nem**</span><span class="sxs-lookup"><span data-stu-id="adaa9-179">**No**</span></span>               |
| <span data-ttu-id="adaa9-180">PowerShell</span><span class="sxs-lookup"><span data-stu-id="adaa9-180">PowerShell</span></span>   | <span data-ttu-id="adaa9-181">Igen</span><span class="sxs-lookup"><span data-stu-id="adaa9-181">Yes</span></span>          | <span data-ttu-id="adaa9-182">Igen</span><span class="sxs-lookup"><span data-stu-id="adaa9-182">Yes</span></span>   | <span data-ttu-id="adaa9-183">Igen</span><span class="sxs-lookup"><span data-stu-id="adaa9-183">Yes</span></span>                  |
| <span data-ttu-id="adaa9-184">REST API</span><span class="sxs-lookup"><span data-stu-id="adaa9-184">REST API</span></span>     | <span data-ttu-id="adaa9-185">Igen</span><span class="sxs-lookup"><span data-stu-id="adaa9-185">Yes</span></span>          | <span data-ttu-id="adaa9-186">Igen</span><span class="sxs-lookup"><span data-stu-id="adaa9-186">Yes</span></span>   | <span data-ttu-id="adaa9-187">Igen</span><span class="sxs-lookup"><span data-stu-id="adaa9-187">Yes</span></span>                  |
| <span data-ttu-id="adaa9-188">T-SQL</span><span class="sxs-lookup"><span data-stu-id="adaa9-188">T-SQL</span></span>        | <span data-ttu-id="adaa9-189">**Nem**</span><span class="sxs-lookup"><span data-stu-id="adaa9-189">**No**</span></span>       | <span data-ttu-id="adaa9-190">Igen</span><span class="sxs-lookup"><span data-stu-id="adaa9-190">Yes</span></span>   | <span data-ttu-id="adaa9-191">Igen</span><span class="sxs-lookup"><span data-stu-id="adaa9-191">Yes</span></span>                  |



<a name="scale-compute-bk"></a>

## <a name="scale-compute"></a><span data-ttu-id="adaa9-192">Skála számítási</span><span class="sxs-lookup"><span data-stu-id="adaa9-192">Scale compute</span></span>

<span data-ttu-id="adaa9-193">Az SQL Data Warehouse teljesítményének mérése [az adatraktár-(dwu)] [ data warehouse units (DWUs)] Ez az egy számítási erőforrásokat, például a Processzor, memória és I/O sávszélesség abstracted mértékét.</span><span class="sxs-lookup"><span data-stu-id="adaa9-193">Performance in SQL Data Warehouse is measured in [data warehouse units (DWUs)][data warehouse units (DWUs)] which is an abstracted measure of compute resources such as CPU, memory, and I/O bandwidth.</span></span> <span data-ttu-id="adaa9-194">A felhasználó, aki a rendszer teljesítményét méretezési kívánja ehhez különböző eszközökkel, például a portálon, a T-SQL és a REST API-k segítségével.</span><span class="sxs-lookup"><span data-stu-id="adaa9-194">A user who wishes to scale their system's performance can do so through various means, such as through the portal, T-SQL, and REST APIs.</span></span> 

### <a name="how-do-i-scale-compute"></a><span data-ttu-id="adaa9-195">Hogyan méretezni a számítási?</span><span class="sxs-lookup"><span data-stu-id="adaa9-195">How do I scale compute?</span></span>
<span data-ttu-id="adaa9-196">Számítási power van kezelve az SQL Data Warehouse DWU beállítás módosításával.</span><span class="sxs-lookup"><span data-stu-id="adaa9-196">Compute power is managed for you SQL Data Warehouse by changing the DWU setting.</span></span> <span data-ttu-id="adaa9-197">Teljesítménynövekedést [lineárisan] [ linearly] további DWU bizonyos műveletek való hozzáadása során.</span><span class="sxs-lookup"><span data-stu-id="adaa9-197">Performance increases [linearly][linearly] as you add more DWU for certain operations.</span></span>  <span data-ttu-id="adaa9-198">Kínálunk DWU ajánlatokat, amelyek biztosítják, hogy a teljesítmény változik feltétlenül felfelé vagy lefelé amikor méretezni a rendszer.</span><span class="sxs-lookup"><span data-stu-id="adaa9-198">We offer DWU offerings that ensure that your performance will change noticeably when you scale your system up or down.</span></span> 

<span data-ttu-id="adaa9-199">Úgy, hogy dwu-k, ezek az egyes módszerek bármelyikét használhatja.</span><span class="sxs-lookup"><span data-stu-id="adaa9-199">To adjust DWUs, you can use any of these individual methods.</span></span>

* <span data-ttu-id="adaa9-200">[Skála számítási teljesítményt az Azure-portálon][Scale compute power with Azure portal]</span><span class="sxs-lookup"><span data-stu-id="adaa9-200">[Scale compute power with Azure portal][Scale compute power with Azure portal]</span></span>
* <span data-ttu-id="adaa9-201">[Skála számítási teljesítményt a PowerShell használatával][Scale compute power with PowerShell]</span><span class="sxs-lookup"><span data-stu-id="adaa9-201">[Scale compute power with PowerShell][Scale compute power with PowerShell]</span></span>
* <span data-ttu-id="adaa9-202">[Skála számítási teljesítményt REST API-khoz][Scale compute power with REST APIs]</span><span class="sxs-lookup"><span data-stu-id="adaa9-202">[Scale compute power with REST APIs][Scale compute power with REST APIs]</span></span>
* <span data-ttu-id="adaa9-203">[Skála számítási teljesítményt a TSQL használatával][Scale compute power with TSQL]</span><span class="sxs-lookup"><span data-stu-id="adaa9-203">[Scale compute power with TSQL][Scale compute power with TSQL]</span></span>

### <a name="how-many-dwus-should-i-use"></a><span data-ttu-id="adaa9-204">Hány dwu-k érdemes használni?</span><span class="sxs-lookup"><span data-stu-id="adaa9-204">How many DWUs should I use?</span></span>

<span data-ttu-id="adaa9-205">Az ideális DWU érték megtalálásához próbáljon meg vertikálisan le- és felskálázni, az adatok betöltése után pedig futtasson néhány lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="adaa9-205">To understand what your ideal DWU value is, try scaling up and down, and running a few queries after loading your data.</span></span> <span data-ttu-id="adaa9-206">Mivel a skálázás nem időigényes, próbálja meg különböző teljesítményszintek vagy kevesebb mint egy óra alatt.</span><span class="sxs-lookup"><span data-stu-id="adaa9-206">Since scaling is quick, you can try various performance levels in an hour or less.</span></span> 

> [!Note] 
> <span data-ttu-id="adaa9-207">Az SQL Data Warehouse nagy mennyiségű adat feldolgozására szolgál.</span><span class="sxs-lookup"><span data-stu-id="adaa9-207">SQL Data Warehouse is designed to process large amounts of data.</span></span> <span data-ttu-id="adaa9-208">Igaz platformképességei méretezéshez, különösen a nagyobb dwu-k, hogy használni kívánt a nagy adatkészletet, ami megközelíti vagy meghaladja az 1 TB.</span><span class="sxs-lookup"><span data-stu-id="adaa9-208">To see its true capabilities for scaling, especially at larger DWUs, you want to use a large data set which approaches or exceeds 1 TB.</span></span>

<span data-ttu-id="adaa9-209">Javaslatok a munkaterhelés számára a legjobb DWU kereséséhez:</span><span class="sxs-lookup"><span data-stu-id="adaa9-209">Recommendations for finding the best DWU for your workload:</span></span>

1. <span data-ttu-id="adaa9-210">Az adatok a fejlesztési először egy kisebb DWU teljesítményszintet kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="adaa9-210">For a data warehouse in development, begin by selecting a smaller DWU performance level.</span></span>  <span data-ttu-id="adaa9-211">Jó kiindulópont DW400 vagy DW200.</span><span class="sxs-lookup"><span data-stu-id="adaa9-211">A good starting point is DW400 or DW200.</span></span>
2. <span data-ttu-id="adaa9-212">Az alkalmazás teljesítményének figyelése, a teljesítmény azt láthatja a dwu-k száma a kijelölt megfigyelő képest.</span><span class="sxs-lookup"><span data-stu-id="adaa9-212">Monitor your application performance, observing the number of DWUs selected compared to the performance you observe.</span></span>
3. <span data-ttu-id="adaa9-213">Határozza meg, mennyi gyorsabb vagy alacsonyabb teljesítményt kell lennie ahhoz, hogy a lineáris skála feltételezve elérni az optimális teljesítményszint szükséges a követelmények teljesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="adaa9-213">Determine how much faster or slower performance should be for you to reach the optimum performance level for your requirements by assuming linear scale.</span></span>
4. <span data-ttu-id="adaa9-214">Növelheti vagy csökkentheti a dwu-k számát arányában hogyan sokkal gyorsabban vagy lassabban szeretné-e a számítási feladatok végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="adaa9-214">Increase or decrease the number of DWUs in proportion to how much faster or slower you want your workload to perform.</span></span> 
5. <span data-ttu-id="adaa9-215">Folytatható, amíg el nem éri az optimális teljesítmény szintű üzleti igényeinek a szükséges módosításokat.</span><span class="sxs-lookup"><span data-stu-id="adaa9-215">Continue making adjustments until you reach an optimum performance level for your business requirements.</span></span>

> [!NOTE]
>
> <span data-ttu-id="adaa9-216">Lekérdezési teljesítmény csak további párhuzamos folyamatkezelést biztosítja a növekszik, ha a munkahelyi oszthatók számítási csomópontjai között.</span><span class="sxs-lookup"><span data-stu-id="adaa9-216">Query performance only increases with more parallelization if the work can be split between compute nodes.</span></span> <span data-ttu-id="adaa9-217">Ha úgy találja, hogy skálázás nem változik a teljesítményt, adjon tekintse meg a teljesítményhangolás annak ellenőrzéséhez, hogy az adatok nem egyenlően van elosztva, vagy ha bevezették az adatátvitelt jelölik a nagy mennyiségű.</span><span class="sxs-lookup"><span data-stu-id="adaa9-217">If you find that scaling is not changing your performance, please check out our performance tuning articles to check whether your data is unevenly distributed or if you are introducing a large amount of data movement.</span></span> 

### <a name="when-should-i-scale-dwus"></a><span data-ttu-id="adaa9-218">Mikor érdemes méretezni a dwu-k?</span><span class="sxs-lookup"><span data-stu-id="adaa9-218">When should I scale DWUs?</span></span>
<span data-ttu-id="adaa9-219">A következő fontos forgatókönyveit dwu-k skálázás módosítja:</span><span class="sxs-lookup"><span data-stu-id="adaa9-219">Scaling DWUs alters the following important scenarios:</span></span>

1. <span data-ttu-id="adaa9-220">A rendszer a vizsgálatokat, összesítések és CTAS utasítások lineárisan módosítása</span><span class="sxs-lookup"><span data-stu-id="adaa9-220">Linearly changing performance of the system for scans, aggregations, and CTAS statements</span></span>
2. <span data-ttu-id="adaa9-221">A polybase-zel betöltésekor olvasók és írók számának növelése</span><span class="sxs-lookup"><span data-stu-id="adaa9-221">Increasing the number of readers and writers when loading with PolyBase</span></span>
3. <span data-ttu-id="adaa9-222">Egyidejű lekérdezések és feldolgozási üzembe helyezési ponti maximális száma</span><span class="sxs-lookup"><span data-stu-id="adaa9-222">Maximum number of concurrent queries and concurrency slots</span></span>

<span data-ttu-id="adaa9-223">Mikor érdemes méretezni a dwu-k javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="adaa9-223">Recommendations for when to scale DWUs:</span></span>

1. <span data-ttu-id="adaa9-224">Mielőtt a nagy mennyiségű adat betöltenie vagy átalakítási műveletet hajt végre, növelheti dwu-k, hogy az adatok elérhető gyorsabban.</span><span class="sxs-lookup"><span data-stu-id="adaa9-224">Before you perform a heavy data loading or transformation operation, scale up DWUs so that your data is available more quickly.</span></span>
2. <span data-ttu-id="adaa9-225">Csúcsidőszak munkaidőben méretezhető egyidejű lekérdezések nagy mennyiségű befogadásához.</span><span class="sxs-lookup"><span data-stu-id="adaa9-225">During peak business hours, scale to accommodate larger numbers of concurrent queries.</span></span> 

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a><span data-ttu-id="adaa9-226">Felfüggesztés számítási</span><span class="sxs-lookup"><span data-stu-id="adaa9-226">Pause compute</span></span>
[!INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

<span data-ttu-id="adaa9-227">Egy adatbázis felfüggesztése, használja az alábbi egyes módszereket.</span><span class="sxs-lookup"><span data-stu-id="adaa9-227">To pause a database, use any of these individual methods.</span></span>

* <span data-ttu-id="adaa9-228">[Felfüggesztés számítási az Azure-portálon][Pause compute with Azure portal]</span><span class="sxs-lookup"><span data-stu-id="adaa9-228">[Pause compute with Azure portal][Pause compute with Azure portal]</span></span>
* <span data-ttu-id="adaa9-229">[Felfüggesztés számítási a PowerShell használatával][Pause compute with PowerShell]</span><span class="sxs-lookup"><span data-stu-id="adaa9-229">[Pause compute with PowerShell][Pause compute with PowerShell]</span></span>
* <span data-ttu-id="adaa9-230">[Felfüggesztés számítási REST API-khoz][Pause compute with REST APIs]</span><span class="sxs-lookup"><span data-stu-id="adaa9-230">[Pause compute with REST APIs][Pause compute with REST APIs]</span></span>

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a><span data-ttu-id="adaa9-231">Folytatás számítási</span><span class="sxs-lookup"><span data-stu-id="adaa9-231">Resume compute</span></span>
[!INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

<span data-ttu-id="adaa9-232">Egy adatbázis folytatásához használja az alábbi egyes módszereket.</span><span class="sxs-lookup"><span data-stu-id="adaa9-232">To resume a database, use any of these individual methods.</span></span>

* <span data-ttu-id="adaa9-233">[Folytatás számítási az Azure-portálon][Resume compute with Azure portal]</span><span class="sxs-lookup"><span data-stu-id="adaa9-233">[Resume compute with Azure portal][Resume compute with Azure portal]</span></span>
* <span data-ttu-id="adaa9-234">[Folytatás számítási a PowerShell használatával][Resume compute with PowerShell]</span><span class="sxs-lookup"><span data-stu-id="adaa9-234">[Resume compute with PowerShell][Resume compute with PowerShell]</span></span>
* <span data-ttu-id="adaa9-235">[Folytatás számítási REST API-khoz][Resume compute with REST APIs]</span><span class="sxs-lookup"><span data-stu-id="adaa9-235">[Resume compute with REST APIs][Resume compute with REST APIs]</span></span>

<a name="check-compute-bk"></a>

## <a name="check-database-state"></a><span data-ttu-id="adaa9-236">Ellenőrizze az adatbázis állapota</span><span class="sxs-lookup"><span data-stu-id="adaa9-236">Check database state</span></span> 

<span data-ttu-id="adaa9-237">Egy adatbázis folytatásához használja az alábbi egyes módszereket.</span><span class="sxs-lookup"><span data-stu-id="adaa9-237">To resume a database, use any of these individual methods.</span></span>

- <span data-ttu-id="adaa9-238">[A T-SQL adatbázis állapotának ellenőrzése][Check database state with T-SQL]</span><span class="sxs-lookup"><span data-stu-id="adaa9-238">[Check database state with T-SQL][Check database state with T-SQL]</span></span>
- <span data-ttu-id="adaa9-239">[Ellenőrizze az adatbázis állapotát a PowerShell használatával][Check database state with PowerShell]</span><span class="sxs-lookup"><span data-stu-id="adaa9-239">[Check database state with PowerShell][Check database state with PowerShell]</span></span>
- <span data-ttu-id="adaa9-240">[Ellenőrizze az adatbázis állapotát a REST API-k][Check database state with REST APIs]</span><span class="sxs-lookup"><span data-stu-id="adaa9-240">[Check database state with REST APIs][Check database state with REST APIs]</span></span>

## <a name="permissions"></a><span data-ttu-id="adaa9-241">Engedélyek</span><span class="sxs-lookup"><span data-stu-id="adaa9-241">Permissions</span></span>

<span data-ttu-id="adaa9-242">Az adatbázis skálázás az engedélyekkel kell rendelkeznie a leírt [ALTER DATABASE][ALTER DATABASE].</span><span class="sxs-lookup"><span data-stu-id="adaa9-242">Scaling the database requires the permissions described in [ALTER DATABASE][ALTER DATABASE].</span></span>  <span data-ttu-id="adaa9-243">Szüneteltetése és folytatása szükséges a [SQL DB Contributor] [ SQL DB Contributor] engedéllyel, akkor a kifejezetten Microsoft.Sql/servers/databases/action.</span><span class="sxs-lookup"><span data-stu-id="adaa9-243">Pause and Resume require the [SQL DB Contributor][SQL DB Contributor] permission, specifically Microsoft.Sql/servers/databases/action.</span></span>

<a name="next-steps-bk"></a>

## <a name="next-steps"></a><span data-ttu-id="adaa9-244">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="adaa9-244">Next steps</span></span>
<span data-ttu-id="adaa9-245">Tekintse meg a következő cikkek segítenek megismerni néhány további legfontosabb teljesítményi fogalmak:</span><span class="sxs-lookup"><span data-stu-id="adaa9-245">Refer to the following articles to help you understand some additional key performance concepts:</span></span>

* <span data-ttu-id="adaa9-246">[Munkaterhelés és feldolgozási kezelése][Workload and concurrency management]</span><span class="sxs-lookup"><span data-stu-id="adaa9-246">[Workload and concurrency management][Workload and concurrency management]</span></span>
* <span data-ttu-id="adaa9-247">[Tábla kialakítás áttekintése][Table design overview]</span><span class="sxs-lookup"><span data-stu-id="adaa9-247">[Table design overview][Table design overview]</span></span>
* <span data-ttu-id="adaa9-248">[Elosztása][Table distribution]</span><span class="sxs-lookup"><span data-stu-id="adaa9-248">[Table distribution][Table distribution]</span></span>
* <span data-ttu-id="adaa9-249">[Tábla indexelő][Table indexing]</span><span class="sxs-lookup"><span data-stu-id="adaa9-249">[Table indexing][Table indexing]</span></span>
* <span data-ttu-id="adaa9-250">[Táblaparticionálást][Table partitioning]</span><span class="sxs-lookup"><span data-stu-id="adaa9-250">[Table partitioning][Table partitioning]</span></span>
* <span data-ttu-id="adaa9-251">[Tábla statisztikai adatainak][Table statistics]</span><span class="sxs-lookup"><span data-stu-id="adaa9-251">[Table statistics][Table statistics]</span></span>
* <span data-ttu-id="adaa9-252">[Gyakorlati tanácsok][Best practices]</span><span class="sxs-lookup"><span data-stu-id="adaa9-252">[Best practices][Best practices]</span></span>

<!--Image reference-->

<!--Article references-->
[data warehouse units (DWUs)]: ./sql-data-warehouse-overview-what-is.md#predictable-and-scalable-performance-with-data-warehouse-units
[billed]: https://azure.microsoft.com/en-us/pricing/details/sql-data-warehouse/
[linearly]: ./sql-data-warehouse-overview-what-is.md#predictable-and-scalable-performance-with-data-warehouse-units
[Scale compute power with Azure portal]: ./sql-data-warehouse-manage-compute-portal.md#scale-compute-power
[Scale compute power with PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#scale-compute-bk
[Scale compute power with REST APIs]: ./sql-data-warehouse-manage-compute-rest-api.md#scale-compute-bk
[Scale compute power with TSQL]: ./sql-data-warehouse-manage-compute-tsql.md#scale-compute-bk

[capacity limits]: ./sql-data-warehouse-service-capacity-limits.md

[Pause compute with Azure portal]:  ./sql-data-warehouse-manage-compute-portal.md#pause-compute-bk
[Pause compute with PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#pause-compute-bk
[Pause compute with REST APIs]: ./sql-data-warehouse-manage-compute-rest-api.md#pause-compute-bk

[Resume compute with Azure portal]:  ./sql-data-warehouse-manage-compute-portal.md#resume-compute-bk
[Resume compute with PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#resume-compute-bk
[Resume compute with REST APIs]: ./sql-data-warehouse-manage-compute-rest-api.md#resume-compute-bk

[Check database state with T-SQL]: ./sql-data-warehouse-manage-compute-tsql.md#check-database-state-and-operation-progress
[Check database state with PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#check-database-state
[Check database state with REST APIs]: ./sql-data-warehouse-manage-compute-rest-api.md#check-database-state

[Workload and concurrency management]: ./sql-data-warehouse-develop-concurrency.md
[Table design overview]: ./sql-data-warehouse-tables-overview.md
[Table distribution]: ./sql-data-warehouse-tables-distribute.md
[Table indexing]: ./sql-data-warehouse-tables-index.md
[Table partitioning]: ./sql-data-warehouse-tables-partition.md
[Table statistics]: ./sql-data-warehouse-tables-statistics.md
[Best practices]: ./sql-data-warehouse-best-practices.md
[development overview]: ./sql-data-warehouse-overview-develop.md

[SQL DB Contributor]: ../active-directory/role-based-access-built-in-roles.md#sql-db-contributor

<!--MSDN references-->
[ALTER DATABASE]: https://msdn.microsoft.com/library/mt204042.aspx

<!--Other Web references-->
[Azure portal]: http://portal.azure.com/
