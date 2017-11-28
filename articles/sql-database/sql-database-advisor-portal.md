---
title: "aaaApply teljesítmény ajánlásokkal – az Azure SQL Database |} Microsoft Docs"
description: "Hello Azure portál toofind teljesítmény javaslatok is optimalizálhatja az Azure SQL Database vagy toocorrect teljesítményét is használhatja a számítási feladatok valamilyen probléma."
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: monicar
ms.assetid: cda8a646-0584-4368-b28a-85cdd9b54fcd
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 07/05/2017
ms.author: sstein
ms.openlocfilehash: 0b2234840fc3254d235db9e7d18f5bc912851c6d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="find-and-apply-performance-recommendations"></a><span data-ttu-id="d3fcb-103">Keresse meg és teljesítmény javaslatok alkalmazása</span><span class="sxs-lookup"><span data-stu-id="d3fcb-103">Find and apply performance recommendations</span></span>

<span data-ttu-id="d3fcb-104">Hello Azure portál toofind teljesítmény javaslatok is optimalizálhatja az Azure SQL Database vagy toocorrect teljesítményét is használhatja a számítási feladatok valamilyen probléma.</span><span class="sxs-lookup"><span data-stu-id="d3fcb-104">You can use hello Azure portal toofind performance recommendations that can optimize performance of your Azure SQL Database or toocorrect some issue identified in your workload.</span></span> <span data-ttu-id="d3fcb-105">**Teljesítmény ajánlás** Azure-portálon lap lehetővé teszi, hogy toofind hello felső javaslatok potenciális hatásuk alapján.</span><span class="sxs-lookup"><span data-stu-id="d3fcb-105">**Performance recommendation** page in Azure portal enables you toofind hello top recommendations based on their potential impact.</span></span> 

## <a name="viewing-recommendations"></a><span data-ttu-id="d3fcb-106">Javaslatok megtekintése</span><span class="sxs-lookup"><span data-stu-id="d3fcb-106">Viewing recommendations</span></span>

<span data-ttu-id="d3fcb-107">tooview és alkalmazza a teljesítmény ajánlásokat kell hello megfelelő [szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-what-is.md) engedélyek az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="d3fcb-107">tooview and apply performance recommendations, you need hello correct [role-based access control](../active-directory/role-based-access-control-what-is.md) permissions in Azure.</span></span> <span data-ttu-id="d3fcb-108">**Olvasó**, **SQL DB Contributor** engedélyekre szükség tooview javaslatok és **tulajdonos**, **SQL DB Contributor** engedélyekre szükség tooexecute műveletek; Hozzon létre vagy dobjon el indexeket, és indexlétrehozás megszakítását.</span><span class="sxs-lookup"><span data-stu-id="d3fcb-108">**Reader**, **SQL DB Contributor** permissions are required tooview recommendations, and **Owner**, **SQL DB Contributor** permissions are required tooexecute any actions; create or drop indexes and cancel index creation.</span></span>

<span data-ttu-id="d3fcb-109">Követő lépéseket toofind teljesítmény javaslatok az Azure-portálon hello használata:</span><span class="sxs-lookup"><span data-stu-id="d3fcb-109">Use hello following steps toofind performance recommendations on Azure portal:</span></span>

1. <span data-ttu-id="d3fcb-110">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="d3fcb-110">Sign in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="d3fcb-111">Nyissa meg túl**további szolgáltatások** > **SQL-adatbázisok**, és válassza ki az adatbázist.</span><span class="sxs-lookup"><span data-stu-id="d3fcb-111">Go too**More services** > **SQL databases**, and select your database.</span></span>
3. <span data-ttu-id="d3fcb-112">Keresse meg a túl**teljesítmény ajánlás** tooview elérhető javaslatok hello kiválasztott adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="d3fcb-112">Navigate too**Performance recommendation** tooview available recommendations for hello selected database.</span></span>

<span data-ttu-id="d3fcb-113">Teljesítmény javaslatok shonw szerepelnek hello tábla hasonló toohello egy hello a következő ábrán látható:</span><span class="sxs-lookup"><span data-stu-id="d3fcb-113">Performance recommendations are shonw in hello table similar toohello one shown on hello following figure:</span></span>

![Javaslatok](./media/sql-database-advisor-portal/recommendations.png)

<span data-ttu-id="d3fcb-115">Javaslatok a potenciális gyakorolt hatása összességében be a következő kategóriák hello vannak rendezve:</span><span class="sxs-lookup"><span data-stu-id="d3fcb-115">Recommendations are sorted by their potential impact on performance into hello following categories:</span></span>

| <span data-ttu-id="d3fcb-116">Gyakorolt hatás</span><span class="sxs-lookup"><span data-stu-id="d3fcb-116">Impact</span></span> | <span data-ttu-id="d3fcb-117">Leírás</span><span class="sxs-lookup"><span data-stu-id="d3fcb-117">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="d3fcb-118">Magas</span><span class="sxs-lookup"><span data-stu-id="d3fcb-118">High</span></span> |<span data-ttu-id="d3fcb-119">Nagy jelentőségű javaslatok hello legjelentősebb teljesítményre gyakorolt hatás kell biztosítania.</span><span class="sxs-lookup"><span data-stu-id="d3fcb-119">High impact recommendations should provide hello most significant performance impact.</span></span> |
| <span data-ttu-id="d3fcb-120">Közepes</span><span class="sxs-lookup"><span data-stu-id="d3fcb-120">Medium</span></span> |<span data-ttu-id="d3fcb-121">Közepes hatás ajánlásokat kell teljesítmény javítása érdekében, de nem jelentősen.</span><span class="sxs-lookup"><span data-stu-id="d3fcb-121">Medium impact recommendations should improve performance, but not substantially.</span></span> |
| <span data-ttu-id="d3fcb-122">Alacsony</span><span class="sxs-lookup"><span data-stu-id="d3fcb-122">Low</span></span> |<span data-ttu-id="d3fcb-123">Alacsony hatás ajánlásokat kell biztosítania nélkül jobb teljesítményt, de fejlesztései nem mindig jelentős.</span><span class="sxs-lookup"><span data-stu-id="d3fcb-123">Low impact recommendations should provide better performance than without, but improvements might not be significant.</span></span> |


> [!NOTE]
> <span data-ttu-id="d3fcb-124">Az Azure SQL Database kell toomonitor tevékenységek legalább egy rendelés tooidentify napjára néhány javaslatot.</span><span class="sxs-lookup"><span data-stu-id="d3fcb-124">Azure SQL Database needs toomonitor activities at least for a day in order tooidentify some recommendations.</span></span> <span data-ttu-id="d3fcb-125">hello Azure SQL Database könnyebben optimalizálható lekérdezés konzisztens mintára, mint a véletlenszerű spotty felszakadásáig tevékenység használhatja.</span><span class="sxs-lookup"><span data-stu-id="d3fcb-125">hello Azure SQL Database can more easily optimize for consistent query patterns than it can for random spotty bursts of activity.</span></span> <span data-ttu-id="d3fcb-126">Ha a javaslatok még nem érhető el corrently, hello **teljesítmény ajánlás** lap biztosít egy üzenet, amely ismerteti, hogy miért.</span><span class="sxs-lookup"><span data-stu-id="d3fcb-126">If recommendations are not corrently available, hello **Performance recommendation** page provides a message explaining why.</span></span>
> 

<span data-ttu-id="d3fcb-127">Hello hello múltbeli működési állapotát is megtekintheti.</span><span class="sxs-lookup"><span data-stu-id="d3fcb-127">You can also view hello status of hello historical operations.</span></span> <span data-ttu-id="d3fcb-128">Válassza ki a javaslat vagy állapot toosee további részleteket.</span><span class="sxs-lookup"><span data-stu-id="d3fcb-128">Select a recommendation or status toosee  more details.</span></span>

<span data-ttu-id="d3fcb-129">Íme egy példa a ajánlás "Create index" hello Azure-portálon a.</span><span class="sxs-lookup"><span data-stu-id="d3fcb-129">Here is an example of "Create index" recommendation in hello Azure portal.</span></span>

![Index létrehozása](./media/sql-database-advisor-portal/sql-database-performance-recommendation.png)

## <a name="applying-recommendations"></a><span data-ttu-id="d3fcb-131">Javaslatok alkalmazása</span><span class="sxs-lookup"><span data-stu-id="d3fcb-131">Applying recommendations</span></span>
<span data-ttu-id="d3fcb-132">Az Azure SQL adatbázis teljes szabályozhatják, hogyan a javaslatok még lehetővé teszi engedélyezve van, használja a következő három lehetőség hello valamelyikét:</span><span class="sxs-lookup"><span data-stu-id="d3fcb-132">Azure SQL Database gives you full control over how recommendations are enabled using any of hello following three options:</span></span> 

* <span data-ttu-id="d3fcb-133">Az egyes javaslatok egy alkalmazza egyszerre.</span><span class="sxs-lookup"><span data-stu-id="d3fcb-133">Apply individual recommendations one at a time.</span></span>
* <span data-ttu-id="d3fcb-134">Enable hello automatikus hangolási tooautomatically javaslatok érvényesek.</span><span class="sxs-lookup"><span data-stu-id="d3fcb-134">Enable hello Automatic tuning tooautomatically apply recommendations.</span></span>
* <span data-ttu-id="d3fcb-135">tooimplement ajánlást manuálisan, futtassa az adatbázison a T-SQL parancsfájl ajánlott hello.</span><span class="sxs-lookup"><span data-stu-id="d3fcb-135">tooimplement a recommendation manually, run hello recommended T-SQL script against your database.</span></span>

<span data-ttu-id="d3fcb-136">Válassza ki a javaslat tooview a részletek, és kattintson a **parancsfájl megtekintése** tooreview hello pontos részleteit hogyan hello javaslat hozták létre.</span><span class="sxs-lookup"><span data-stu-id="d3fcb-136">Select any recommendation tooview its details and then click **View script** tooreview hello exact details of how hello recommendation is created.</span></span>

<span data-ttu-id="d3fcb-137">hello adatbázis online marad, amíg hello javaslat alkalmazása – teljesítmény javasolt vagy automatikus hangolása soha nem fogad egy adatbázis kapcsolat nélküli.</span><span class="sxs-lookup"><span data-stu-id="d3fcb-137">hello database remains online while hello recommendation is applied -- using performance recommendation or automatic tuning never takes a database offline.</span></span>

### <a name="apply-an-individual-recommendation"></a><span data-ttu-id="d3fcb-138">Az egyes javaslat alkalmazása</span><span class="sxs-lookup"><span data-stu-id="d3fcb-138">Apply an individual recommendation</span></span>
<span data-ttu-id="d3fcb-139">Tekintse át, és fogadja el a javaslatok egy egyszerre.</span><span class="sxs-lookup"><span data-stu-id="d3fcb-139">You can review and accept recommendations one at a time.</span></span>

1. <span data-ttu-id="d3fcb-140">A hello **javaslatok** panelen válassza ki a megfelelő javaslat.</span><span class="sxs-lookup"><span data-stu-id="d3fcb-140">On hello **Recommendations** blade, select a recommendation.</span></span>
2. <span data-ttu-id="d3fcb-141">A hello **részletek** panelen kattintson **alkalmaz** gombra.</span><span class="sxs-lookup"><span data-stu-id="d3fcb-141">On hello **Details** blade click **Apply** button.</span></span>
   
    ![A javaslat alkalmazása](./media/sql-database-advisor-portal/apply.png)

<span data-ttu-id="d3fcb-143">Kijelölt javaslat hello adatbázison lépnek érvénybe.</span><span class="sxs-lookup"><span data-stu-id="d3fcb-143">Selected recommendation will be applied on hello database.</span></span>

### <a name="removing-recommendations-from-hello-list"></a><span data-ttu-id="d3fcb-144">Javaslatok eltávolítása hello listájáról</span><span class="sxs-lookup"><span data-stu-id="d3fcb-144">Removing recommendations from hello list</span></span>
<span data-ttu-id="d3fcb-145">Ha a javaslatok listája az, hogy szeretné-e hello listából tooremove elemet tartalmaz, elvetheti hello javaslat:</span><span class="sxs-lookup"><span data-stu-id="d3fcb-145">If your list of recommendations contains items that you want tooremove from hello list, you can discard hello recommendation:</span></span>

1. <span data-ttu-id="d3fcb-146">Jelöljön ki egy javaslatot hello listája **javaslatok** tooopen hello részleteit.</span><span class="sxs-lookup"><span data-stu-id="d3fcb-146">Select a recommendation in hello list of **Recommendations** tooopen hello details.</span></span>
2. <span data-ttu-id="d3fcb-147">Kattintson a **elvetése** a hello **részletek** panelen.</span><span class="sxs-lookup"><span data-stu-id="d3fcb-147">Click **Discard** on hello **Details** blade.</span></span>

<span data-ttu-id="d3fcb-148">Igény szerint is hozzáadhat az elvetett elemek biztonsági toohello **javaslatok** listája:</span><span class="sxs-lookup"><span data-stu-id="d3fcb-148">If desired, you can add discarded items back toohello **Recommendations** list:</span></span>

1. <span data-ttu-id="d3fcb-149">A hello **javaslatok** panelen kattintson **elvetett nézet**.</span><span class="sxs-lookup"><span data-stu-id="d3fcb-149">On hello **Recommendations** blade click **View discarded**.</span></span>
2. <span data-ttu-id="d3fcb-150">Válasszon egy eldobott elemet hello lista tooview hozzá tartozó részletek.</span><span class="sxs-lookup"><span data-stu-id="d3fcb-150">Select a discarded item from hello list tooview its details.</span></span>
3. <span data-ttu-id="d3fcb-151">Ha úgy gondolja, a **visszavonása elvetése** tooadd hello index vissza toohello fő listája **javaslatok**.</span><span class="sxs-lookup"><span data-stu-id="d3fcb-151">Optionally, click **Undo Discard** tooadd hello index back toohello main list of **Recommendations**.</span></span>


### <a name="enable-automatic-tuning"></a><span data-ttu-id="d3fcb-152">Automatikus hangolás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="d3fcb-152">Enable automatic tuning</span></span>
<span data-ttu-id="d3fcb-153">Hello Azure SQL Database tooimplement javaslatok automatikusan állíthatja be.</span><span class="sxs-lookup"><span data-stu-id="d3fcb-153">You can set hello Azure SQL Database tooimplement recommendations automatically.</span></span> <span data-ttu-id="d3fcb-154">Amint elérhetővé válnak a javaslatok azokat a rendszer automatikusan alkalmazza.</span><span class="sxs-lookup"><span data-stu-id="d3fcb-154">As recommendations become available they will automatically be applied.</span></span> <span data-ttu-id="d3fcb-155">Mint hello által kezelt összes ajánlásokkal szolgáltatás negatív hello javaslat hello teljesítményre gyakorolt hatás esetén vissza lesznek vonva.</span><span class="sxs-lookup"><span data-stu-id="d3fcb-155">As with all recommendations managed by hello service if hello performance impact is negative hello recommendation will be reverted.</span></span>

1. <span data-ttu-id="d3fcb-156">A hello **javaslatok** panelen kattintson a **automatizálás**:</span><span class="sxs-lookup"><span data-stu-id="d3fcb-156">On hello **Recommendations** blade, click **Automate**:</span></span>
   
    ![Az Advisor-beállítások](./media/sql-database-advisor-portal/settings.png)
2. <span data-ttu-id="d3fcb-158">Válassza ki a műveletek tooautomate:</span><span class="sxs-lookup"><span data-stu-id="d3fcb-158">Select actions tooautomate:</span></span>
   
    ![Ajánlott indexek](./media/sql-database-advisor-portal/automation.png)

### <a name="manually-run-hello-recommended-t-sql-script"></a><span data-ttu-id="d3fcb-160">Ajánlott a T-SQL parancsfájl hello manuális futtatása</span><span class="sxs-lookup"><span data-stu-id="d3fcb-160">Manually run hello recommended T-SQL script</span></span>
<span data-ttu-id="d3fcb-161">Válassza ki a javaslat, és kattintson a **parancsfájl megtekintése**.</span><span class="sxs-lookup"><span data-stu-id="d3fcb-161">Select any recommendation and then click **View script**.</span></span> <span data-ttu-id="d3fcb-162">Futtassa a parancsfájlt az adatbázis toomanually alkalmaz hello javaslatot.</span><span class="sxs-lookup"><span data-stu-id="d3fcb-162">Run this script against your database toomanually apply hello recommendation.</span></span>

<span data-ttu-id="d3fcb-163">*Manuálisan végrehajtott indexek nem figyeli, és a teljesítményre gyakorolt hatás hello szolgáltatás érvényesíteni* ezért ajánlatos, hogy nyomon, hogy ezek az indexek létrehozása tooverify után azok biztosít teljesítménynövekedést érhet el és módosítsa vagy törölje azokat, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="d3fcb-163">*Indexes that are manually executed are not monitored and validated for performance impact by hello service* so it is suggested that you monitor these indexes after creation tooverify they provide performance gains and adjust or delete them if necessary.</span></span> <span data-ttu-id="d3fcb-164">Indexek létrehozásával kapcsolatos részletekért lásd: [a CREATE INDEX (Transact-SQL)](https://msdn.microsoft.com/library/ms188783.aspx).</span><span class="sxs-lookup"><span data-stu-id="d3fcb-164">For details about creating indexes, see [CREATE INDEX (Transact-SQL)](https://msdn.microsoft.com/library/ms188783.aspx).</span></span>

### <a name="canceling-recommendations"></a><span data-ttu-id="d3fcb-165">Javaslatok megszakítása</span><span class="sxs-lookup"><span data-stu-id="d3fcb-165">Canceling recommendations</span></span>
<span data-ttu-id="d3fcb-166">A javaslatok egy **függőben lévő**, **ellenőrzése**, vagy **sikeres** állapota lehet megszakítani.</span><span class="sxs-lookup"><span data-stu-id="d3fcb-166">Recommendations that are in a **Pending**, **Verifying**, or **Success** status can be canceled.</span></span> <span data-ttu-id="d3fcb-167">Javaslatok állapotú **Executing** nem szakítható meg.</span><span class="sxs-lookup"><span data-stu-id="d3fcb-167">Recommendations with a status of **Executing** cannot be canceled.</span></span>

1. <span data-ttu-id="d3fcb-168">Jelöljön ki egy javaslatot hello **hangolása előzmények** terület tooopen hello **ajánlások részleteit** panelen.</span><span class="sxs-lookup"><span data-stu-id="d3fcb-168">Select a recommendation in hello **Tuning History** area tooopen hello **recommendations details** blade.</span></span>
2. <span data-ttu-id="d3fcb-169">Kattintson a **Mégse** tooabort hello folyamat hello javaslat alkalmazása.</span><span class="sxs-lookup"><span data-stu-id="d3fcb-169">Click **Cancel** tooabort hello process of applying hello recommendation.</span></span>

## <a name="monitoring-operations"></a><span data-ttu-id="d3fcb-170">Figyelési műveletek</span><span class="sxs-lookup"><span data-stu-id="d3fcb-170">Monitoring operations</span></span>
<span data-ttu-id="d3fcb-171">A javaslat alkalmazása nem fordulhat elő, azonnal.</span><span class="sxs-lookup"><span data-stu-id="d3fcb-171">Applying a recommendation might not happen instantaneously.</span></span> <span data-ttu-id="d3fcb-172">hello portal javaslat hello állapotának vonatkozó részletes adatokat biztosít.</span><span class="sxs-lookup"><span data-stu-id="d3fcb-172">hello portal provides details regarding hello status of recommendation.</span></span> <span data-ttu-id="d3fcb-173">Az alábbiakban hello index lehet a lehetséges állapotok:</span><span class="sxs-lookup"><span data-stu-id="d3fcb-173">hello following are possible states that an index can be in:</span></span>

| <span data-ttu-id="d3fcb-174">status</span><span class="sxs-lookup"><span data-stu-id="d3fcb-174">Status</span></span> | <span data-ttu-id="d3fcb-175">Leírás</span><span class="sxs-lookup"><span data-stu-id="d3fcb-175">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="d3fcb-176">Függőben</span><span class="sxs-lookup"><span data-stu-id="d3fcb-176">Pending</span></span> |<span data-ttu-id="d3fcb-177">A javaslat parancs érkezett, és a végrehajtásra ütemezett alkalmazni.</span><span class="sxs-lookup"><span data-stu-id="d3fcb-177">Apply recommendation command has been received and is scheduled for execution.</span></span> |
| <span data-ttu-id="d3fcb-178">Végrehajtása</span><span class="sxs-lookup"><span data-stu-id="d3fcb-178">Executing</span></span> |<span data-ttu-id="d3fcb-179">hello ajánlás vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="d3fcb-179">hello recommendation is being applied.</span></span> |
| <span data-ttu-id="d3fcb-180">Ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="d3fcb-180">Verifying</span></span> |<span data-ttu-id="d3fcb-181">A javaslat alkalmazása sikeresen és hello szolgáltatás mérés hello előnyeit.</span><span class="sxs-lookup"><span data-stu-id="d3fcb-181">Recommendation was successfully applied and hello service is measuring hello benefits.</span></span> |
| <span data-ttu-id="d3fcb-182">Sikeres</span><span class="sxs-lookup"><span data-stu-id="d3fcb-182">Success</span></span> |<span data-ttu-id="d3fcb-183">A javaslat alkalmazása sikeresen és előnyök mért lett.</span><span class="sxs-lookup"><span data-stu-id="d3fcb-183">Recommendation was successfully applied and benefits have been measured.</span></span> |
| <span data-ttu-id="d3fcb-184">Hiba</span><span class="sxs-lookup"><span data-stu-id="d3fcb-184">Error</span></span> |<span data-ttu-id="d3fcb-185">Hiba történt a hello folyamat hello javaslat alkalmazása során.</span><span class="sxs-lookup"><span data-stu-id="d3fcb-185">An error occurred during hello process of applying hello recommendation.</span></span> <span data-ttu-id="d3fcb-186">Ez lehet átmeneti jellegű probléma, vagy esetleg a séma változástábla toohello és hello parancsfájl már nem érvényes.</span><span class="sxs-lookup"><span data-stu-id="d3fcb-186">This can be a transient issue, or possibly a schema change toohello table and hello script is no longer valid.</span></span> |
| <span data-ttu-id="d3fcb-187">Visszaállítás</span><span class="sxs-lookup"><span data-stu-id="d3fcb-187">Reverting</span></span> |<span data-ttu-id="d3fcb-188">hello javaslat alkalmazta, de nem performant megállapítása, és automatikusan vissza.</span><span class="sxs-lookup"><span data-stu-id="d3fcb-188">hello recommendation was applied, but has been deemed non-performant and is being automatically reverted.</span></span> |
| <span data-ttu-id="d3fcb-189">Vissza</span><span class="sxs-lookup"><span data-stu-id="d3fcb-189">Reverted</span></span> |<span data-ttu-id="d3fcb-190">hello javaslat vissza lett állítva.</span><span class="sxs-lookup"><span data-stu-id="d3fcb-190">hello recommendation was reverted.</span></span> |

<span data-ttu-id="d3fcb-191">Kattintson egy hello lista toosee folyamaton belüli ajánlása további részletek:</span><span class="sxs-lookup"><span data-stu-id="d3fcb-191">Click an in-process recommendation from hello list toosee more details:</span></span>

![Ajánlott indexek](./media/sql-database-advisor-portal/operations.png)

### <a name="reverting-a-recommendation"></a><span data-ttu-id="d3fcb-193">Azt ajánljuk, akkor a gyermekrekordokra</span><span class="sxs-lookup"><span data-stu-id="d3fcb-193">Reverting a recommendation</span></span>
<span data-ttu-id="d3fcb-194">Ha hello teljesítmény javaslatok tooapply hello javaslat (azaz a manuális futtatása hello T-SQL parancsfájl) azt automatikusan visszaállítja azt ha hello teljesítmény hatás toobe negatív talál.</span><span class="sxs-lookup"><span data-stu-id="d3fcb-194">If you used hello performance recommendations tooapply hello recommendation (meaning you did not manually run hello T-SQL script) it will automatically revert it if it finds hello performance impact toobe negative.</span></span> <span data-ttu-id="d3fcb-195">Ha bármilyen okból kívánt egyszerűen toorevert ajánlást teheti hello következő:</span><span class="sxs-lookup"><span data-stu-id="d3fcb-195">If for any reason you simply want toorevert a recommendation you can do hello following:</span></span>

1. <span data-ttu-id="d3fcb-196">Válassza ki a sikeresen elvégzett javaslat hello **előzmények hangolása** területen.</span><span class="sxs-lookup"><span data-stu-id="d3fcb-196">Select a successfully applied recommendation in hello **Tuning history** area.</span></span>
2. <span data-ttu-id="d3fcb-197">Kattintson a **visszaállítás** a hello **javaslat részletei** panelen.</span><span class="sxs-lookup"><span data-stu-id="d3fcb-197">Click **Revert** on hello **recommendation details** blade.</span></span>

![Ajánlott indexek](./media/sql-database-advisor-portal/details.png)

## <a name="monitoring-performance-impact-of-index-recommendations"></a><span data-ttu-id="d3fcb-199">Ajánlott index teljesítményre gyakorolt hatásának figyelése</span><span class="sxs-lookup"><span data-stu-id="d3fcb-199">Monitoring performance impact of index recommendations</span></span>
<span data-ttu-id="d3fcb-200">Javaslatok sikeres végrehajtása után (jelenleg műveletek index és csak a lekérdezések javaslatok parametrizálja) kattintva **lekérdezési** a hello javaslat részletei panel tooopen [lekérdezés Teljesítménybe](sql-database-query-performance.md) és tekintse meg a leggyakoribb lekérdezések hello teljesítményre gyakorolt hatást.</span><span class="sxs-lookup"><span data-stu-id="d3fcb-200">After recommendations are successfully implemented (currently, index operations and parameterize queries recommendations only) you can click **Query Insights** on hello recommendation details blade tooopen [Query Performance Insights](sql-database-query-performance.md) and see hello performance impact of your top queries.</span></span>

![A figyelő teljesítményre gyakorolt hatás](./media/sql-database-advisor-portal/query-insights.png)

## <a name="summary"></a><span data-ttu-id="d3fcb-202">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="d3fcb-202">Summary</span></span>
<span data-ttu-id="d3fcb-203">Az Azure SQL-adatbázis SQL-adatbázis teljesítményének javítására vonatkozó javaslatokkal szolgál.</span><span class="sxs-lookup"><span data-stu-id="d3fcb-203">Azure SQL Database provides recommendations for improving SQL database performance.</span></span> <span data-ttu-id="d3fcb-204">Adja meg a T-SQL-parancsfájlok, továbbá az egyéni és a teljesen automatikus, az adatbázis optimalizálása, és végül a lekérdezési teljesítmény javítása a hasznos segítséget kap.</span><span class="sxs-lookup"><span data-stu-id="d3fcb-204">By providing T-SQL scripts, as well as individual and fully-automatic, you get a helpful assistance in optimizing your database and ultimately improving query performance.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d3fcb-205">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d3fcb-205">Next steps</span></span>
<span data-ttu-id="d3fcb-206">A javaslatok figyelése, és továbbra is tooapply őket toorefine teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="d3fcb-206">Monitor your recommendations and continue tooapply them toorefine performance.</span></span> <span data-ttu-id="d3fcb-207">Adatbázis-terhelések dinamikusak, és folyamatosan módosítása.</span><span class="sxs-lookup"><span data-stu-id="d3fcb-207">Database workloads are dynamic and change continuously.</span></span> <span data-ttu-id="d3fcb-208">Azure SQL-adatbázis továbbra is toomonitor, és a ajánlani, amelyek javíthatja az adatbázis teljesítménye.</span><span class="sxs-lookup"><span data-stu-id="d3fcb-208">Azure SQL Database will continue toomonitor and provide recommendations that can potentially improve your database's performance.</span></span> 

* <span data-ttu-id="d3fcb-209">Lásd: [automatikus hangolása](sql-database-automatic-tuning.md) toolearn arról hello Azure SQL adatbázis automatikus hangolása.</span><span class="sxs-lookup"><span data-stu-id="d3fcb-209">See [Automatic tuning](sql-database-automatic-tuning.md) toolearn more about hello automatic tuning in Azure SQL Database.</span></span>
* <span data-ttu-id="d3fcb-210">Lásd: [teljesítmény javaslatok](sql-database-advisor.md) teljesítmény javaslatok az Azure SQL Database áttekintését.</span><span class="sxs-lookup"><span data-stu-id="d3fcb-210">See [Performance recommendations](sql-database-advisor.md) for an overview of Azure SQL Database performance recommendations.</span></span>
* <span data-ttu-id="d3fcb-211">Lásd: [lekérdezési teljesítmény Insights](sql-database-query-performance.md) toolearn hello teljesítményre gyakorolt hatását a leggyakoribb lekérdezések megtekintése.</span><span class="sxs-lookup"><span data-stu-id="d3fcb-211">See [Query Performance Insights](sql-database-query-performance.md) toolearn about viewing hello performance impact of your top queries.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d3fcb-212">További források</span><span class="sxs-lookup"><span data-stu-id="d3fcb-212">Additional resources</span></span>
* [<span data-ttu-id="d3fcb-213">A Lekérdezéstár</span><span class="sxs-lookup"><span data-stu-id="d3fcb-213">Query Store</span></span>](https://msdn.microsoft.com/library/dn817826.aspx)
* [<span data-ttu-id="d3fcb-214">INDEX LÉTREHOZÁSA</span><span class="sxs-lookup"><span data-stu-id="d3fcb-214">CREATE INDEX</span></span>](https://msdn.microsoft.com/library/ms188783.aspx)
* [<span data-ttu-id="d3fcb-215">Szerepköralapú hozzáférés-vezérlés</span><span class="sxs-lookup"><span data-stu-id="d3fcb-215">Role-based access control</span></span>](../active-directory/role-based-access-control-what-is.md)

