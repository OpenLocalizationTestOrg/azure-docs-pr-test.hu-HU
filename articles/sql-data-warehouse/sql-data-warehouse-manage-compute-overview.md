---
title: "aaaManage számítási teljesítményt az Azure SQL Data Warehouse (áttekintés) |} Microsoft Docs"
description: "Az Azure SQL Data Warehouse képességek kibővítési teljesítményét. Horizontális felskálázás dwu-k módosításával vagy és sablonok felfüggesztése és folytatása a számítási erőforrások toosave költségeket."
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
ms.openlocfilehash: 1ffbe8d694ac181eaeb6f585a2cee87a570ed7d5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-compute-power-in-azure-sql-data-warehouse-overview"></a><span data-ttu-id="66ff8-104">Kezelheti a számítási teljesítményt az Azure SQL Data Warehouse (áttekintés)</span><span class="sxs-lookup"><span data-stu-id="66ff8-104">Manage compute power in Azure SQL Data Warehouse (Overview)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="66ff8-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="66ff8-105">Overview</span></span>](sql-data-warehouse-manage-compute-overview.md)
> * [<span data-ttu-id="66ff8-106">Portál</span><span class="sxs-lookup"><span data-stu-id="66ff8-106">Portal</span></span>](sql-data-warehouse-manage-compute-portal.md)
> * [<span data-ttu-id="66ff8-107">PowerShell</span><span class="sxs-lookup"><span data-stu-id="66ff8-107">PowerShell</span></span>](sql-data-warehouse-manage-compute-powershell.md)
> * [<span data-ttu-id="66ff8-108">REST</span><span class="sxs-lookup"><span data-stu-id="66ff8-108">REST</span></span>](sql-data-warehouse-manage-compute-rest-api.md)
> * [<span data-ttu-id="66ff8-109">TSQL</span><span class="sxs-lookup"><span data-stu-id="66ff8-109">TSQL</span></span>](sql-data-warehouse-manage-compute-tsql.md)
>
>

<span data-ttu-id="66ff8-110">az SQL Data Warehouse architektúrája hello elválasztja a tárolást és számítást, így függetlenül minden tooscale.</span><span class="sxs-lookup"><span data-stu-id="66ff8-110">hello architecture of SQL Data Warehouse separates storage and compute, allowing each tooscale independently.</span></span> <span data-ttu-id="66ff8-111">Ennek eredményeképpen a számítási méretezett toomeet teljesítményigénye hello adatmennyiségtől függetlenül lehet.</span><span class="sxs-lookup"><span data-stu-id="66ff8-111">As a result, compute can be scaled toomeet performance demands independent of hello amount of data.</span></span> <span data-ttu-id="66ff8-112">Ez az architektúra természetes következménye, hogy [számlázási] [ billed] számítási és tárolási elkülönül.</span><span class="sxs-lookup"><span data-stu-id="66ff8-112">A natural consequence of this architecture is that [billing][billed] for compute and storage is separate.</span></span> 

<span data-ttu-id="66ff8-113">Ez az áttekintés bemutatja hogyan terjessze ki az SQL Data Warehouse, és hogyan tooutilize hello szüneteltetése, folytatása és az SQL Data Warehouse méretezési képességeket biztosít.</span><span class="sxs-lookup"><span data-stu-id="66ff8-113">This overview describes how scale out works with SQL Data Warehouse and how tooutilize hello pause, resume, and scale capabilities of SQL Data Warehouse.</span></span> <span data-ttu-id="66ff8-114">Tekintse át a hello [az adatraktár-(dwu)] [ data warehouse units (DWUs)] lap toolearn hogyan dwu-k és teljesítmény kapcsolódnak.</span><span class="sxs-lookup"><span data-stu-id="66ff8-114">Consult hello [data warehouse units (DWUs)][data warehouse units (DWUs)] page toolearn how DWUs and performance are related.</span></span> 

## <a name="how-compute-management-operations-work-in-sql-data-warehouse"></a><span data-ttu-id="66ff8-115">Milyen számítási felügyeleti műveletek a vártnak az SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="66ff8-115">How compute management operations work in SQL Data Warehouse</span></span>
<span data-ttu-id="66ff8-116">hello architektúra az SQL Data Warehouse áll-e a vezérlő csomópont, a számítási csomópontok és hello tárolási réteg 60 terjesztéseket elosztva.</span><span class="sxs-lookup"><span data-stu-id="66ff8-116">hello architecture for SQL Data Warehouse consists of a control node, compute nodes, and hello storage layer spread across 60 distributions.</span></span> 

<span data-ttu-id="66ff8-117">Egy normál aktív munkamenet során az SQL Data Warehouse a rendszer átjárócsomópont hello metaadatok kezeli, és hello elosztott lekérdezésoptimalizáló tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="66ff8-117">During a normal active session in SQL Data Warehouse, your system's head node manages hello metadata and contains hello distributed query optimizer.</span></span> <span data-ttu-id="66ff8-118">A head csomópont alatt a számítási csomópontok és a tárolási rétegből állnak.</span><span class="sxs-lookup"><span data-stu-id="66ff8-118">Beneath this head node are your compute nodes and your storage layer.</span></span> <span data-ttu-id="66ff8-119">A DWU 400 a rendszer egy átjárócsomóponttal, négy számítási csomópontok és hello tárolási rétegből álló 60 terjesztéseket rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="66ff8-119">For a DWU 400, your system has one head node, four compute nodes, and hello storage layer, consisting of 60 distributions.</span></span> 

<span data-ttu-id="66ff8-120">A skála változni vagy művelet felfüggeszti, hello rendszer először használhatatlanná teszi az összes bejövő lekérdezés, és visszavon tranzakciók tooensure konzisztens állapotú legyen.</span><span class="sxs-lookup"><span data-stu-id="66ff8-120">When you undergo a scale or pause operation, hello system first kills all incoming queries and then rolls back transactions tooensure a consistent state.</span></span> <span data-ttu-id="66ff8-121">A skálázási műveletek, a méretezés akkor történik, a tranzakciós visszaállítás befejezése után.</span><span class="sxs-lookup"><span data-stu-id="66ff8-121">For scale operations, scaling will only occur once this transactional rollback has completed.</span></span> <span data-ttu-id="66ff8-122">A méretezett művelet hello rendszerre vonatkozó hello számítási csomópontok extra kívánt számát, és megkezdi a újracsatlakoztatása hello számítási csomópontok toohello tárolási réteg.</span><span class="sxs-lookup"><span data-stu-id="66ff8-122">For a scale-up operation, hello system provisions hello extra desired number of compute nodes, and then begins reattaching hello compute nodes toohello storage layer.</span></span> <span data-ttu-id="66ff8-123">Egy lefelé méretezéshez művelet hello felesleges csomópontok kiadott, majd hello fennmaradó számítási csomópontok újra csatlakoztatja, maguk toohello megfelelő számú terjesztési.</span><span class="sxs-lookup"><span data-stu-id="66ff8-123">For a scale-down operation, hello unneeded nodes are released and hello remaining compute nodes reattach themselves toohello appropriate number of distributions.</span></span> <span data-ttu-id="66ff8-124">A szüneteltetési művelete minden számítási csomópont feloldásáig blokkolva lesz, és a rendszer változni fog metaadat-műveletek tooleave számos a végső rendszer stabil állapotban.</span><span class="sxs-lookup"><span data-stu-id="66ff8-124">For a pause operation, all compute nodes are released and your system will undergo a variety of metadata operations tooleave your final system in a stable state.</span></span>

| <span data-ttu-id="66ff8-125">DWU</span><span class="sxs-lookup"><span data-stu-id="66ff8-125">DWU</span></span>  | <span data-ttu-id="66ff8-126">\#a számítási csomópontok</span><span class="sxs-lookup"><span data-stu-id="66ff8-126">\#of compute nodes</span></span> | <span data-ttu-id="66ff8-127">\#az azokat a terjesztéseket, csomópontonként</span><span class="sxs-lookup"><span data-stu-id="66ff8-127">\# of distributions per node</span></span> |
| ---- | ------------------ | ---------------------------- |
| <span data-ttu-id="66ff8-128">100</span><span class="sxs-lookup"><span data-stu-id="66ff8-128">100</span></span>  | <span data-ttu-id="66ff8-129">1</span><span class="sxs-lookup"><span data-stu-id="66ff8-129">1</span></span>                  | <span data-ttu-id="66ff8-130">60</span><span class="sxs-lookup"><span data-stu-id="66ff8-130">60</span></span>                           |
| <span data-ttu-id="66ff8-131">200</span><span class="sxs-lookup"><span data-stu-id="66ff8-131">200</span></span>  | <span data-ttu-id="66ff8-132">2</span><span class="sxs-lookup"><span data-stu-id="66ff8-132">2</span></span>                  | <span data-ttu-id="66ff8-133">30</span><span class="sxs-lookup"><span data-stu-id="66ff8-133">30</span></span>                           |
| <span data-ttu-id="66ff8-134">300</span><span class="sxs-lookup"><span data-stu-id="66ff8-134">300</span></span>  | <span data-ttu-id="66ff8-135">3</span><span class="sxs-lookup"><span data-stu-id="66ff8-135">3</span></span>                  | <span data-ttu-id="66ff8-136">20</span><span class="sxs-lookup"><span data-stu-id="66ff8-136">20</span></span>                           |
| <span data-ttu-id="66ff8-137">400</span><span class="sxs-lookup"><span data-stu-id="66ff8-137">400</span></span>  | <span data-ttu-id="66ff8-138">4</span><span class="sxs-lookup"><span data-stu-id="66ff8-138">4</span></span>                  | <span data-ttu-id="66ff8-139">15</span><span class="sxs-lookup"><span data-stu-id="66ff8-139">15</span></span>                           |
| <span data-ttu-id="66ff8-140">500</span><span class="sxs-lookup"><span data-stu-id="66ff8-140">500</span></span>  | <span data-ttu-id="66ff8-141">5</span><span class="sxs-lookup"><span data-stu-id="66ff8-141">5</span></span>                  | <span data-ttu-id="66ff8-142">12</span><span class="sxs-lookup"><span data-stu-id="66ff8-142">12</span></span>                           |
| <span data-ttu-id="66ff8-143">600</span><span class="sxs-lookup"><span data-stu-id="66ff8-143">600</span></span>  | <span data-ttu-id="66ff8-144">6</span><span class="sxs-lookup"><span data-stu-id="66ff8-144">6</span></span>                  | <span data-ttu-id="66ff8-145">10</span><span class="sxs-lookup"><span data-stu-id="66ff8-145">10</span></span>                           |
| <span data-ttu-id="66ff8-146">1000</span><span class="sxs-lookup"><span data-stu-id="66ff8-146">1000</span></span> | <span data-ttu-id="66ff8-147">10</span><span class="sxs-lookup"><span data-stu-id="66ff8-147">10</span></span>                 | <span data-ttu-id="66ff8-148">6</span><span class="sxs-lookup"><span data-stu-id="66ff8-148">6</span></span>                            |
| <span data-ttu-id="66ff8-149">1200</span><span class="sxs-lookup"><span data-stu-id="66ff8-149">1200</span></span> | <span data-ttu-id="66ff8-150">12</span><span class="sxs-lookup"><span data-stu-id="66ff8-150">12</span></span>                 | <span data-ttu-id="66ff8-151">5</span><span class="sxs-lookup"><span data-stu-id="66ff8-151">5</span></span>                            |
| <span data-ttu-id="66ff8-152">1500</span><span class="sxs-lookup"><span data-stu-id="66ff8-152">1500</span></span> | <span data-ttu-id="66ff8-153">15</span><span class="sxs-lookup"><span data-stu-id="66ff8-153">15</span></span>                 | <span data-ttu-id="66ff8-154">4</span><span class="sxs-lookup"><span data-stu-id="66ff8-154">4</span></span>                            |
| <span data-ttu-id="66ff8-155">2000</span><span class="sxs-lookup"><span data-stu-id="66ff8-155">2000</span></span> | <span data-ttu-id="66ff8-156">20</span><span class="sxs-lookup"><span data-stu-id="66ff8-156">20</span></span>                 | <span data-ttu-id="66ff8-157">3</span><span class="sxs-lookup"><span data-stu-id="66ff8-157">3</span></span>                            |
| <span data-ttu-id="66ff8-158">3000</span><span class="sxs-lookup"><span data-stu-id="66ff8-158">3000</span></span> | <span data-ttu-id="66ff8-159">30</span><span class="sxs-lookup"><span data-stu-id="66ff8-159">30</span></span>                 | <span data-ttu-id="66ff8-160">2</span><span class="sxs-lookup"><span data-stu-id="66ff8-160">2</span></span>                            |
| <span data-ttu-id="66ff8-161">6000</span><span class="sxs-lookup"><span data-stu-id="66ff8-161">6000</span></span> | <span data-ttu-id="66ff8-162">60</span><span class="sxs-lookup"><span data-stu-id="66ff8-162">60</span></span>                 | <span data-ttu-id="66ff8-163">1</span><span class="sxs-lookup"><span data-stu-id="66ff8-163">1</span></span>                            |

<span data-ttu-id="66ff8-164">hello három elsődleges funkciója számítási kezeléséhez a következők:</span><span class="sxs-lookup"><span data-stu-id="66ff8-164">hello three primary functions for managing compute are:</span></span>

1. <span data-ttu-id="66ff8-165">Szünet</span><span class="sxs-lookup"><span data-stu-id="66ff8-165">Pause</span></span>
2. <span data-ttu-id="66ff8-166">Folytatás</span><span class="sxs-lookup"><span data-stu-id="66ff8-166">Resume</span></span>
3. <span data-ttu-id="66ff8-167">Méretezés</span><span class="sxs-lookup"><span data-stu-id="66ff8-167">Scale</span></span>

<span data-ttu-id="66ff8-168">Mindkét művelet eltarthat néhány percig toocomplete.</span><span class="sxs-lookup"><span data-stu-id="66ff8-168">Each of these operations may take several minutes toocomplete.</span></span> <span data-ttu-id="66ff8-169">Ha skálázás/felfüggesztése vagy folytatása automatikusan, érdemes lehet tooimplement logika tooensure, hogy bizonyos műveletek elvégzése után egy másik művelet folytatása előtt.</span><span class="sxs-lookup"><span data-stu-id="66ff8-169">If you are scaling/pausing/resuming automatically, you may want tooimplement logic tooensure that certain operations have been completed before proceeding with another action.</span></span> 

<span data-ttu-id="66ff8-170">Különböző végpontok keresztül hello adatbázis állapotának ellenőrzése lehetővé teszi az ilyen műveletek toocorrectly megvalósítása automatizálását.</span><span class="sxs-lookup"><span data-stu-id="66ff8-170">Checking hello database state through various endpoints will allow you toocorrectly implement automation of such operations.</span></span> <span data-ttu-id="66ff8-171">hello portal befejezett egy műveletet, és hello adatbázisok aktuális állapot értesítést küldenek, de nem engedélyezi a állapot ellenőrzéséhez szoftveres.</span><span class="sxs-lookup"><span data-stu-id="66ff8-171">hello portal will provide notification upon completion of an operation and hello databases current state but does not allow for programmatic checking of state.</span></span> 

>  [!NOTE]
>
>  <span data-ttu-id="66ff8-172">Számítási felügyeleti funkció végpontjai között nem létezik.</span><span class="sxs-lookup"><span data-stu-id="66ff8-172">Compute management functionality does not exist across all endpoints.</span></span>
>
>  

|              | <span data-ttu-id="66ff8-173">Felfüggesztése vagy folytatása</span><span class="sxs-lookup"><span data-stu-id="66ff8-173">Pause/Resume</span></span> | <span data-ttu-id="66ff8-174">Méretezés</span><span class="sxs-lookup"><span data-stu-id="66ff8-174">Scale</span></span> | <span data-ttu-id="66ff8-175">Ellenőrizze az adatbázis állapota</span><span class="sxs-lookup"><span data-stu-id="66ff8-175">Check database state</span></span> |
| ------------ | ------------ | ----- | -------------------- |
| <span data-ttu-id="66ff8-176">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="66ff8-176">Azure portal</span></span> | <span data-ttu-id="66ff8-177">Igen</span><span class="sxs-lookup"><span data-stu-id="66ff8-177">Yes</span></span>          | <span data-ttu-id="66ff8-178">Igen</span><span class="sxs-lookup"><span data-stu-id="66ff8-178">Yes</span></span>   | <span data-ttu-id="66ff8-179">**Nem**</span><span class="sxs-lookup"><span data-stu-id="66ff8-179">**No**</span></span>               |
| <span data-ttu-id="66ff8-180">PowerShell</span><span class="sxs-lookup"><span data-stu-id="66ff8-180">PowerShell</span></span>   | <span data-ttu-id="66ff8-181">Igen</span><span class="sxs-lookup"><span data-stu-id="66ff8-181">Yes</span></span>          | <span data-ttu-id="66ff8-182">Igen</span><span class="sxs-lookup"><span data-stu-id="66ff8-182">Yes</span></span>   | <span data-ttu-id="66ff8-183">Igen</span><span class="sxs-lookup"><span data-stu-id="66ff8-183">Yes</span></span>                  |
| <span data-ttu-id="66ff8-184">REST API</span><span class="sxs-lookup"><span data-stu-id="66ff8-184">REST API</span></span>     | <span data-ttu-id="66ff8-185">Igen</span><span class="sxs-lookup"><span data-stu-id="66ff8-185">Yes</span></span>          | <span data-ttu-id="66ff8-186">Igen</span><span class="sxs-lookup"><span data-stu-id="66ff8-186">Yes</span></span>   | <span data-ttu-id="66ff8-187">Igen</span><span class="sxs-lookup"><span data-stu-id="66ff8-187">Yes</span></span>                  |
| <span data-ttu-id="66ff8-188">T-SQL</span><span class="sxs-lookup"><span data-stu-id="66ff8-188">T-SQL</span></span>        | <span data-ttu-id="66ff8-189">**Nem**</span><span class="sxs-lookup"><span data-stu-id="66ff8-189">**No**</span></span>       | <span data-ttu-id="66ff8-190">Igen</span><span class="sxs-lookup"><span data-stu-id="66ff8-190">Yes</span></span>   | <span data-ttu-id="66ff8-191">Igen</span><span class="sxs-lookup"><span data-stu-id="66ff8-191">Yes</span></span>                  |



<a name="scale-compute-bk"></a>

## <a name="scale-compute"></a><span data-ttu-id="66ff8-192">Skála számítási</span><span class="sxs-lookup"><span data-stu-id="66ff8-192">Scale compute</span></span>

<span data-ttu-id="66ff8-193">Az SQL Data Warehouse teljesítményének mérése [az adatraktár-(dwu)] [ data warehouse units (DWUs)] Ez az egy számítási erőforrásokat, például a Processzor, memória és I/O sávszélesség abstracted mértékét.</span><span class="sxs-lookup"><span data-stu-id="66ff8-193">Performance in SQL Data Warehouse is measured in [data warehouse units (DWUs)][data warehouse units (DWUs)] which is an abstracted measure of compute resources such as CPU, memory, and I/O bandwidth.</span></span> <span data-ttu-id="66ff8-194">A felhasználó, aki tooscale kívánja a rendszer teljesítményét ehhez különböző eszközökkel, például hello portálon, a T-SQL és a REST API-k segítségével.</span><span class="sxs-lookup"><span data-stu-id="66ff8-194">A user who wishes tooscale their system's performance can do so through various means, such as through hello portal, T-SQL, and REST APIs.</span></span> 

### <a name="how-do-i-scale-compute"></a><span data-ttu-id="66ff8-195">Hogyan méretezni a számítási?</span><span class="sxs-lookup"><span data-stu-id="66ff8-195">How do I scale compute?</span></span>
<span data-ttu-id="66ff8-196">Számítási teljesítményt van kezelve az SQL Data Warehouse hello DWU beállítás módosításával.</span><span class="sxs-lookup"><span data-stu-id="66ff8-196">Compute power is managed for you SQL Data Warehouse by changing hello DWU setting.</span></span> <span data-ttu-id="66ff8-197">Teljesítménynövekedést [lineárisan] [ linearly] további DWU bizonyos műveletek való hozzáadása során.</span><span class="sxs-lookup"><span data-stu-id="66ff8-197">Performance increases [linearly][linearly] as you add more DWU for certain operations.</span></span>  <span data-ttu-id="66ff8-198">Kínálunk DWU ajánlatokat, amelyek biztosítják, hogy a teljesítmény változik feltétlenül felfelé vagy lefelé amikor méretezni a rendszer.</span><span class="sxs-lookup"><span data-stu-id="66ff8-198">We offer DWU offerings that ensure that your performance will change noticeably when you scale your system up or down.</span></span> 

<span data-ttu-id="66ff8-199">tooadjust dwu-k, használja az alábbi egyes módszereket.</span><span class="sxs-lookup"><span data-stu-id="66ff8-199">tooadjust DWUs, you can use any of these individual methods.</span></span>

* <span data-ttu-id="66ff8-200">[Skála számítási teljesítményt az Azure-portálon][Scale compute power with Azure portal]</span><span class="sxs-lookup"><span data-stu-id="66ff8-200">[Scale compute power with Azure portal][Scale compute power with Azure portal]</span></span>
* <span data-ttu-id="66ff8-201">[Skála számítási teljesítményt a PowerShell használatával][Scale compute power with PowerShell]</span><span class="sxs-lookup"><span data-stu-id="66ff8-201">[Scale compute power with PowerShell][Scale compute power with PowerShell]</span></span>
* <span data-ttu-id="66ff8-202">[Skála számítási teljesítményt REST API-khoz][Scale compute power with REST APIs]</span><span class="sxs-lookup"><span data-stu-id="66ff8-202">[Scale compute power with REST APIs][Scale compute power with REST APIs]</span></span>
* <span data-ttu-id="66ff8-203">[Skála számítási teljesítményt a TSQL használatával][Scale compute power with TSQL]</span><span class="sxs-lookup"><span data-stu-id="66ff8-203">[Scale compute power with TSQL][Scale compute power with TSQL]</span></span>

### <a name="how-many-dwus-should-i-use"></a><span data-ttu-id="66ff8-204">Hány dwu-k érdemes használni?</span><span class="sxs-lookup"><span data-stu-id="66ff8-204">How many DWUs should I use?</span></span>

<span data-ttu-id="66ff8-205">toounderstand milyen az ideális DWU érték van, próbálja meg skálázás felfelé és lefelé, és futtasson néhány lekérdezést az adatok betöltése után.</span><span class="sxs-lookup"><span data-stu-id="66ff8-205">toounderstand what your ideal DWU value is, try scaling up and down, and running a few queries after loading your data.</span></span> <span data-ttu-id="66ff8-206">Mivel a skálázás nem időigényes, próbálja meg különböző teljesítményszintek vagy kevesebb mint egy óra alatt.</span><span class="sxs-lookup"><span data-stu-id="66ff8-206">Since scaling is quick, you can try various performance levels in an hour or less.</span></span> 

> [!Note] 
> <span data-ttu-id="66ff8-207">Az SQL Data Warehouse tervezett tooprocess nagy mennyiségű adat.</span><span class="sxs-lookup"><span data-stu-id="66ff8-207">SQL Data Warehouse is designed tooprocess large amounts of data.</span></span> <span data-ttu-id="66ff8-208">toosee toouse a nagy adatkészletet, ami megközelíti vagy meghaladja az 1 TB-os kívánt méretezéshez, különösen a nagyobb dwu-k, igaz képességeit.</span><span class="sxs-lookup"><span data-stu-id="66ff8-208">toosee its true capabilities for scaling, especially at larger DWUs, you want toouse a large data set which approaches or exceeds 1 TB.</span></span>

<span data-ttu-id="66ff8-209">Javaslatok keresése a munkaterhelés számára legjobb DWU hello:</span><span class="sxs-lookup"><span data-stu-id="66ff8-209">Recommendations for finding hello best DWU for your workload:</span></span>

1. <span data-ttu-id="66ff8-210">Az adatok a fejlesztési először egy kisebb DWU teljesítményszintet kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="66ff8-210">For a data warehouse in development, begin by selecting a smaller DWU performance level.</span></span>  <span data-ttu-id="66ff8-211">Jó kiindulópont DW400 vagy DW200.</span><span class="sxs-lookup"><span data-stu-id="66ff8-211">A good starting point is DW400 or DW200.</span></span>
2. <span data-ttu-id="66ff8-212">Az alkalmazás teljesítményének figyelése, erőforrásigények toohello teljesítmény választott dwu-k számának hello betartásával képest.</span><span class="sxs-lookup"><span data-stu-id="66ff8-212">Monitor your application performance, observing hello number of DWUs selected compared toohello performance you observe.</span></span>
3. <span data-ttu-id="66ff8-213">Határozza meg, mennyi gyorsabb vagy alacsonyabb teljesítményt akkor tooreach hello optimális teljesítményszint szükséges a követelmények teljesítéséhez által lineáris skála feltételezve kell lennie.</span><span class="sxs-lookup"><span data-stu-id="66ff8-213">Determine how much faster or slower performance should be for you tooreach hello optimum performance level for your requirements by assuming linear scale.</span></span>
4. <span data-ttu-id="66ff8-214">Növeli vagy csökkenti a hello szám dwu-k a arányban toohow sokkal gyorsabb vagy alacsonyabb, amelyet a munkaterhelés tooperform.</span><span class="sxs-lookup"><span data-stu-id="66ff8-214">Increase or decrease hello number of DWUs in proportion toohow much faster or slower you want your workload tooperform.</span></span> 
5. <span data-ttu-id="66ff8-215">Folytatható, amíg el nem éri az optimális teljesítmény szintű üzleti igényeinek a szükséges módosításokat.</span><span class="sxs-lookup"><span data-stu-id="66ff8-215">Continue making adjustments until you reach an optimum performance level for your business requirements.</span></span>

> [!NOTE]
>
> <span data-ttu-id="66ff8-216">Lekérdezési teljesítmény csak további párhuzamos folyamatkezelést biztosítja a növekszik, ha hello munkahelyi oszthatók számítási csomópontjai között.</span><span class="sxs-lookup"><span data-stu-id="66ff8-216">Query performance only increases with more parallelization if hello work can be split between compute nodes.</span></span> <span data-ttu-id="66ff8-217">Ha úgy találja, hogy skálázás nem változik a teljesítményt, adjon tekintse meg a cikkek toocheck, hogy az adatok nem egyenlően van elosztva, vagy ha bevezették az adatátvitelt jelölik a nagy mennyiségű teljesítményhangolás.</span><span class="sxs-lookup"><span data-stu-id="66ff8-217">If you find that scaling is not changing your performance, please check out our performance tuning articles toocheck whether your data is unevenly distributed or if you are introducing a large amount of data movement.</span></span> 

### <a name="when-should-i-scale-dwus"></a><span data-ttu-id="66ff8-218">Mikor érdemes méretezni a dwu-k?</span><span class="sxs-lookup"><span data-stu-id="66ff8-218">When should I scale DWUs?</span></span>
<span data-ttu-id="66ff8-219">A következő fontos forgatókönyveit hello dwu-k skálázás módosítja:</span><span class="sxs-lookup"><span data-stu-id="66ff8-219">Scaling DWUs alters hello following important scenarios:</span></span>

1. <span data-ttu-id="66ff8-220">A vizsgálatok, összesítések és CTAS utasítások hello rendszer teljesítményét lineárisan módosítása</span><span class="sxs-lookup"><span data-stu-id="66ff8-220">Linearly changing performance of hello system for scans, aggregations, and CTAS statements</span></span>
2. <span data-ttu-id="66ff8-221">Olvasók és a polybase-zel betöltésekor írók növekvő hello száma</span><span class="sxs-lookup"><span data-stu-id="66ff8-221">Increasing hello number of readers and writers when loading with PolyBase</span></span>
3. <span data-ttu-id="66ff8-222">Egyidejű lekérdezések és feldolgozási üzembe helyezési ponti maximális száma</span><span class="sxs-lookup"><span data-stu-id="66ff8-222">Maximum number of concurrent queries and concurrency slots</span></span>

<span data-ttu-id="66ff8-223">A javaslatok tooscale dwu-k számát:</span><span class="sxs-lookup"><span data-stu-id="66ff8-223">Recommendations for when tooscale DWUs:</span></span>

1. <span data-ttu-id="66ff8-224">Mielőtt a nagy mennyiségű adat betöltenie vagy átalakítási műveletet hajt végre, növelheti dwu-k, hogy az adatok elérhető gyorsabban.</span><span class="sxs-lookup"><span data-stu-id="66ff8-224">Before you perform a heavy data loading or transformation operation, scale up DWUs so that your data is available more quickly.</span></span>
2. <span data-ttu-id="66ff8-225">Csúcsidőszak munkaidőben méretezhető tooaccommodate nagy mennyiségű egyidejű lekérdezéseket.</span><span class="sxs-lookup"><span data-stu-id="66ff8-225">During peak business hours, scale tooaccommodate larger numbers of concurrent queries.</span></span> 

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a><span data-ttu-id="66ff8-226">Felfüggesztés számítási</span><span class="sxs-lookup"><span data-stu-id="66ff8-226">Pause compute</span></span>
[!INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

<span data-ttu-id="66ff8-227">toopause egy adatbázisban, használja az alábbi egyes módszereket.</span><span class="sxs-lookup"><span data-stu-id="66ff8-227">toopause a database, use any of these individual methods.</span></span>

* <span data-ttu-id="66ff8-228">[Felfüggesztés számítási az Azure-portálon][Pause compute with Azure portal]</span><span class="sxs-lookup"><span data-stu-id="66ff8-228">[Pause compute with Azure portal][Pause compute with Azure portal]</span></span>
* <span data-ttu-id="66ff8-229">[Felfüggesztés számítási a PowerShell használatával][Pause compute with PowerShell]</span><span class="sxs-lookup"><span data-stu-id="66ff8-229">[Pause compute with PowerShell][Pause compute with PowerShell]</span></span>
* <span data-ttu-id="66ff8-230">[Felfüggesztés számítási REST API-khoz][Pause compute with REST APIs]</span><span class="sxs-lookup"><span data-stu-id="66ff8-230">[Pause compute with REST APIs][Pause compute with REST APIs]</span></span>

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a><span data-ttu-id="66ff8-231">Folytatás számítási</span><span class="sxs-lookup"><span data-stu-id="66ff8-231">Resume compute</span></span>
[!INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

<span data-ttu-id="66ff8-232">tooresume egy adatbázisban, használja az alábbi egyes módszereket.</span><span class="sxs-lookup"><span data-stu-id="66ff8-232">tooresume a database, use any of these individual methods.</span></span>

* <span data-ttu-id="66ff8-233">[Folytatás számítási az Azure-portálon][Resume compute with Azure portal]</span><span class="sxs-lookup"><span data-stu-id="66ff8-233">[Resume compute with Azure portal][Resume compute with Azure portal]</span></span>
* <span data-ttu-id="66ff8-234">[Folytatás számítási a PowerShell használatával][Resume compute with PowerShell]</span><span class="sxs-lookup"><span data-stu-id="66ff8-234">[Resume compute with PowerShell][Resume compute with PowerShell]</span></span>
* <span data-ttu-id="66ff8-235">[Folytatás számítási REST API-khoz][Resume compute with REST APIs]</span><span class="sxs-lookup"><span data-stu-id="66ff8-235">[Resume compute with REST APIs][Resume compute with REST APIs]</span></span>

<a name="check-compute-bk"></a>

## <a name="check-database-state"></a><span data-ttu-id="66ff8-236">Ellenőrizze az adatbázis állapota</span><span class="sxs-lookup"><span data-stu-id="66ff8-236">Check database state</span></span> 

<span data-ttu-id="66ff8-237">tooresume egy adatbázisban, használja az alábbi egyes módszereket.</span><span class="sxs-lookup"><span data-stu-id="66ff8-237">tooresume a database, use any of these individual methods.</span></span>

- <span data-ttu-id="66ff8-238">[A T-SQL adatbázis állapotának ellenőrzése][Check database state with T-SQL]</span><span class="sxs-lookup"><span data-stu-id="66ff8-238">[Check database state with T-SQL][Check database state with T-SQL]</span></span>
- <span data-ttu-id="66ff8-239">[Ellenőrizze az adatbázis állapotát a PowerShell használatával][Check database state with PowerShell]</span><span class="sxs-lookup"><span data-stu-id="66ff8-239">[Check database state with PowerShell][Check database state with PowerShell]</span></span>
- <span data-ttu-id="66ff8-240">[Ellenőrizze az adatbázis állapotát a REST API-k][Check database state with REST APIs]</span><span class="sxs-lookup"><span data-stu-id="66ff8-240">[Check database state with REST APIs][Check database state with REST APIs]</span></span>

## <a name="permissions"></a><span data-ttu-id="66ff8-241">Engedélyek</span><span class="sxs-lookup"><span data-stu-id="66ff8-241">Permissions</span></span>

<span data-ttu-id="66ff8-242">Méretezési hello adatbázis ismertetett hello engedélyekkel kell rendelkeznie [ALTER DATABASE][ALTER DATABASE].</span><span class="sxs-lookup"><span data-stu-id="66ff8-242">Scaling hello database requires hello permissions described in [ALTER DATABASE][ALTER DATABASE].</span></span>  <span data-ttu-id="66ff8-243">Szüneteltetése és folytatása szükséges hello [SQL DB Contributor] [ SQL DB Contributor] engedéllyel, akkor a kifejezetten Microsoft.Sql/servers/databases/action.</span><span class="sxs-lookup"><span data-stu-id="66ff8-243">Pause and Resume require hello [SQL DB Contributor][SQL DB Contributor] permission, specifically Microsoft.Sql/servers/databases/action.</span></span>

<a name="next-steps-bk"></a>

## <a name="next-steps"></a><span data-ttu-id="66ff8-244">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="66ff8-244">Next steps</span></span>
<span data-ttu-id="66ff8-245">Tekintse meg a következő cikkek toohelp ismernie néhány további legfontosabb teljesítményi fogalmak toohello:</span><span class="sxs-lookup"><span data-stu-id="66ff8-245">Refer toohello following articles toohelp you understand some additional key performance concepts:</span></span>

* <span data-ttu-id="66ff8-246">[Munkaterhelés és feldolgozási kezelése][Workload and concurrency management]</span><span class="sxs-lookup"><span data-stu-id="66ff8-246">[Workload and concurrency management][Workload and concurrency management]</span></span>
* <span data-ttu-id="66ff8-247">[Tábla kialakítás áttekintése][Table design overview]</span><span class="sxs-lookup"><span data-stu-id="66ff8-247">[Table design overview][Table design overview]</span></span>
* <span data-ttu-id="66ff8-248">[Elosztása][Table distribution]</span><span class="sxs-lookup"><span data-stu-id="66ff8-248">[Table distribution][Table distribution]</span></span>
* <span data-ttu-id="66ff8-249">[Tábla indexelő][Table indexing]</span><span class="sxs-lookup"><span data-stu-id="66ff8-249">[Table indexing][Table indexing]</span></span>
* <span data-ttu-id="66ff8-250">[Táblaparticionálást][Table partitioning]</span><span class="sxs-lookup"><span data-stu-id="66ff8-250">[Table partitioning][Table partitioning]</span></span>
* <span data-ttu-id="66ff8-251">[Tábla statisztikai adatainak][Table statistics]</span><span class="sxs-lookup"><span data-stu-id="66ff8-251">[Table statistics][Table statistics]</span></span>
* <span data-ttu-id="66ff8-252">[Gyakorlati tanácsok][Best practices]</span><span class="sxs-lookup"><span data-stu-id="66ff8-252">[Best practices][Best practices]</span></span>

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
