---
title: "Teljesítmény ajánlásokkal – az Azure SQL Database alkalmazása |} Microsoft Docs"
description: "Használhatja az Azure portálon található teljesítmény javaslatokat is optimalizálhatja az Azure SQL adatbázis teljesítményét, vagy a számítási feladatok valamilyen probléma megoldására."
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
ms.openlocfilehash: 018afaa8b08bd001e55693390e80c8e2c4f33a30
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="find-and-apply-performance-recommendations"></a><span data-ttu-id="a57f7-103">Keresse meg és teljesítmény javaslatok alkalmazása</span><span class="sxs-lookup"><span data-stu-id="a57f7-103">Find and apply performance recommendations</span></span>

<span data-ttu-id="a57f7-104">Használhatja az Azure portálon található teljesítmény javaslatokat is optimalizálhatja az Azure SQL adatbázis teljesítményét, vagy a számítási feladatok valamilyen probléma megoldására.</span><span class="sxs-lookup"><span data-stu-id="a57f7-104">You can use the Azure portal to find performance recommendations that can optimize performance of your Azure SQL Database or to correct some issue identified in your workload.</span></span> <span data-ttu-id="a57f7-105">**Teljesítmény ajánlás** Azure-portálon lapon áttekintheti a célgyűjtemény meg a felső ajánlásainkat található.</span><span class="sxs-lookup"><span data-stu-id="a57f7-105">**Performance recommendation** page in Azure portal enables you to find the top recommendations based on their potential impact.</span></span> 

## <a name="viewing-recommendations"></a><span data-ttu-id="a57f7-106">Javaslatok megtekintése</span><span class="sxs-lookup"><span data-stu-id="a57f7-106">Viewing recommendations</span></span>

<span data-ttu-id="a57f7-107">Megtekintheti és teljesítmény ajánlások érvényesek, kell a megfelelő [szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-what-is.md) engedélyek az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="a57f7-107">To view and apply performance recommendations, you need the correct [role-based access control](../active-directory/role-based-access-control-what-is.md) permissions in Azure.</span></span> <span data-ttu-id="a57f7-108">**Olvasó**, **SQL DB Contributor** engedélyekre van szükség a javaslatok, megtekintése és **tulajdonos**, **SQL DB Contributor** végre semmilyen műveletet; hozzon létre vagy dobjon el indexeket és indexlétrehozás megszakítását jogosultságokra van szükség.</span><span class="sxs-lookup"><span data-stu-id="a57f7-108">**Reader**, **SQL DB Contributor** permissions are required to view recommendations, and **Owner**, **SQL DB Contributor** permissions are required to execute any actions; create or drop indexes and cancel index creation.</span></span>

<span data-ttu-id="a57f7-109">Azure-portál teljesítménye javaslatok kereséséhez tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="a57f7-109">Use the following steps to find performance recommendations on Azure portal:</span></span>

1. <span data-ttu-id="a57f7-110">Jelentkezzen be az [Azure Portalra](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="a57f7-110">Sign in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="a57f7-111">Nyissa meg a **további szolgáltatások** > **SQL-adatbázisok**, és válassza ki az adatbázist.</span><span class="sxs-lookup"><span data-stu-id="a57f7-111">Go to **More services** > **SQL databases**, and select your database.</span></span>
3. <span data-ttu-id="a57f7-112">Navigáljon a **teljesítmény ajánlás** megtekintése a kijelölt adatbázishoz rendelkezésre álló ajánlott.</span><span class="sxs-lookup"><span data-stu-id="a57f7-112">Navigate to **Performance recommendation** to view available recommendations for the selected database.</span></span>

<span data-ttu-id="a57f7-113">Teljesítmény a javaslatok még shonw hasonló jelenik meg az alábbi ábra a táblázatban:</span><span class="sxs-lookup"><span data-stu-id="a57f7-113">Performance recommendations are shonw in the table similar to the one shown on the following figure:</span></span>

![Javaslatok](./media/sql-database-advisor-portal/recommendations.png)

<span data-ttu-id="a57f7-115">Javaslatok a potenciális gyakorolt hatása az alábbi kategóriákba vannak rendezve:</span><span class="sxs-lookup"><span data-stu-id="a57f7-115">Recommendations are sorted by their potential impact on performance into the following categories:</span></span>

| <span data-ttu-id="a57f7-116">Gyakorolt hatás</span><span class="sxs-lookup"><span data-stu-id="a57f7-116">Impact</span></span> | <span data-ttu-id="a57f7-117">Leírás</span><span class="sxs-lookup"><span data-stu-id="a57f7-117">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="a57f7-118">Magas</span><span class="sxs-lookup"><span data-stu-id="a57f7-118">High</span></span> |<span data-ttu-id="a57f7-119">Nagy jelentőségű ajánlásokat kell biztosítania a legjelentősebb teljesítményre gyakorolt hatást.</span><span class="sxs-lookup"><span data-stu-id="a57f7-119">High impact recommendations should provide the most significant performance impact.</span></span> |
| <span data-ttu-id="a57f7-120">Közepes</span><span class="sxs-lookup"><span data-stu-id="a57f7-120">Medium</span></span> |<span data-ttu-id="a57f7-121">Közepes hatás ajánlásokat kell teljesítmény javítása érdekében, de nem jelentősen.</span><span class="sxs-lookup"><span data-stu-id="a57f7-121">Medium impact recommendations should improve performance, but not substantially.</span></span> |
| <span data-ttu-id="a57f7-122">Alacsony</span><span class="sxs-lookup"><span data-stu-id="a57f7-122">Low</span></span> |<span data-ttu-id="a57f7-123">Alacsony hatás ajánlásokat kell biztosítania nélkül jobb teljesítményt, de fejlesztései nem mindig jelentős.</span><span class="sxs-lookup"><span data-stu-id="a57f7-123">Low impact recommendations should provide better performance than without, but improvements might not be significant.</span></span> |


> [!NOTE]
> <span data-ttu-id="a57f7-124">Az Azure SQL-adatbázist vissza kell tevékenységek figyelését, legalább egy napig néhány megismerése érdekében.</span><span class="sxs-lookup"><span data-stu-id="a57f7-124">Azure SQL Database needs to monitor activities at least for a day in order to identify some recommendations.</span></span> <span data-ttu-id="a57f7-125">Az Azure SQL Database könnyebben optimalizálható lekérdezés konzisztens mintára, mint a véletlenszerű spotty felszakadásáig tevékenység használhatja.</span><span class="sxs-lookup"><span data-stu-id="a57f7-125">The Azure SQL Database can more easily optimize for consistent query patterns than it can for random spotty bursts of activity.</span></span> <span data-ttu-id="a57f7-126">Ha a javaslatok még nem érhető el, corrently a **teljesítmény ajánlás** lap egy üzenet, amely ismerteti, hogy miért biztosít.</span><span class="sxs-lookup"><span data-stu-id="a57f7-126">If recommendations are not corrently available, the **Performance recommendation** page provides a message explaining why.</span></span>
> 

<span data-ttu-id="a57f7-127">A múltbeli működési állapotát is megtekintheti.</span><span class="sxs-lookup"><span data-stu-id="a57f7-127">You can also view the status of the historical operations.</span></span> <span data-ttu-id="a57f7-128">További részletek megtekintéséhez javaslat vagy állapot kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="a57f7-128">Select a recommendation or status to see  more details.</span></span>

<span data-ttu-id="a57f7-129">Íme egy példa a ajánlás "Create index" az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="a57f7-129">Here is an example of "Create index" recommendation in the Azure portal.</span></span>

![Index létrehozása](./media/sql-database-advisor-portal/sql-database-performance-recommendation.png)

## <a name="applying-recommendations"></a><span data-ttu-id="a57f7-131">Javaslatok alkalmazása</span><span class="sxs-lookup"><span data-stu-id="a57f7-131">Applying recommendations</span></span>
<span data-ttu-id="a57f7-132">Az Azure SQL adatbázis teljes szabályozhatják, hogyan a javaslatok még lehetővé teszi a következő három beállítások segítségével engedélyezve:</span><span class="sxs-lookup"><span data-stu-id="a57f7-132">Azure SQL Database gives you full control over how recommendations are enabled using any of the following three options:</span></span> 

* <span data-ttu-id="a57f7-133">Az egyes javaslatok egy alkalmazza egyszerre.</span><span class="sxs-lookup"><span data-stu-id="a57f7-133">Apply individual recommendations one at a time.</span></span>
* <span data-ttu-id="a57f7-134">Engedélyezze az automatikus hangolással javaslatok automatikus alkalmazását.</span><span class="sxs-lookup"><span data-stu-id="a57f7-134">Enable the Automatic tuning to automatically apply recommendations.</span></span>
* <span data-ttu-id="a57f7-135">A javaslat manuális alkalmazásához futtassa az ajánlott T-SQL parancsfájlt az adatbázis.</span><span class="sxs-lookup"><span data-stu-id="a57f7-135">To implement a recommendation manually, run the recommended T-SQL script against your database.</span></span>

<span data-ttu-id="a57f7-136">Válassza ki a javaslat a részletek megtekintéséhez, majd **parancsfájl megtekintése** áttekintheti a pontos részleteit a javaslat létrehozását.</span><span class="sxs-lookup"><span data-stu-id="a57f7-136">Select any recommendation to view its details and then click **View script** to review the exact details of how the recommendation is created.</span></span>

<span data-ttu-id="a57f7-137">Az adatbázis online marad, amíg a javaslat alkalmazása--teljesítmény javaslat használatával, vagy automatikus hangolása soha nem fogad egy adatbázis kapcsolat nélküli.</span><span class="sxs-lookup"><span data-stu-id="a57f7-137">The database remains online while the recommendation is applied -- using performance recommendation or automatic tuning never takes a database offline.</span></span>

### <a name="apply-an-individual-recommendation"></a><span data-ttu-id="a57f7-138">Az egyes javaslat alkalmazása</span><span class="sxs-lookup"><span data-stu-id="a57f7-138">Apply an individual recommendation</span></span>
<span data-ttu-id="a57f7-139">Tekintse át, és fogadja el a javaslatok egy egyszerre.</span><span class="sxs-lookup"><span data-stu-id="a57f7-139">You can review and accept recommendations one at a time.</span></span>

1. <span data-ttu-id="a57f7-140">Az a **javaslatok** panelen válassza ki a megfelelő javaslat.</span><span class="sxs-lookup"><span data-stu-id="a57f7-140">On the **Recommendations** blade, select a recommendation.</span></span>
2. <span data-ttu-id="a57f7-141">Az a **részletek** panelen kattintson **alkalmaz** gombra.</span><span class="sxs-lookup"><span data-stu-id="a57f7-141">On the **Details** blade click **Apply** button.</span></span>
   
    ![A javaslat alkalmazása](./media/sql-database-advisor-portal/apply.png)

<span data-ttu-id="a57f7-143">Az adatbázis kijelölt javaslat alkalmazandó.</span><span class="sxs-lookup"><span data-stu-id="a57f7-143">Selected recommendation will be applied on the database.</span></span>

### <a name="removing-recommendations-from-the-list"></a><span data-ttu-id="a57f7-144">Javaslatok eltávolítása a listáról</span><span class="sxs-lookup"><span data-stu-id="a57f7-144">Removing recommendations from the list</span></span>
<span data-ttu-id="a57f7-145">Ha a javaslatok listája az eltávolítani kívánt elemeket tartalmaz, elvetheti a javaslat:</span><span class="sxs-lookup"><span data-stu-id="a57f7-145">If your list of recommendations contains items that you want to remove from the list, you can discard the recommendation:</span></span>

1. <span data-ttu-id="a57f7-146">Jelölje ki az ajánlás listája **javaslatok** részleteit megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="a57f7-146">Select a recommendation in the list of **Recommendations** to open the details.</span></span>
2. <span data-ttu-id="a57f7-147">Kattintson a **elvetése** a a **részletek** panelen.</span><span class="sxs-lookup"><span data-stu-id="a57f7-147">Click **Discard** on the **Details** blade.</span></span>

<span data-ttu-id="a57f7-148">Ha szükséges, hozzáadhat, biztonsági elvetett elemek a **javaslatok** listája:</span><span class="sxs-lookup"><span data-stu-id="a57f7-148">If desired, you can add discarded items back to the **Recommendations** list:</span></span>

1. <span data-ttu-id="a57f7-149">Az a **javaslatok** panelen kattintson **elvetett nézet**.</span><span class="sxs-lookup"><span data-stu-id="a57f7-149">On the **Recommendations** blade click **View discarded**.</span></span>
2. <span data-ttu-id="a57f7-150">Válasszon egy eldobott elemet a listából, a részletek megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="a57f7-150">Select a discarded item from the list to view its details.</span></span>
3. <span data-ttu-id="a57f7-151">Ha úgy gondolja, a **visszavonása elvetése** vissza a fő listáját az index hozzáadása **javaslatok**.</span><span class="sxs-lookup"><span data-stu-id="a57f7-151">Optionally, click **Undo Discard** to add the index back to the main list of **Recommendations**.</span></span>


### <a name="enable-automatic-tuning"></a><span data-ttu-id="a57f7-152">Automatikus hangolás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="a57f7-152">Enable automatic tuning</span></span>
<span data-ttu-id="a57f7-153">Beállíthatja, hogy az Azure SQL Database javaslatok automatikus végrehajtása érdekében.</span><span class="sxs-lookup"><span data-stu-id="a57f7-153">You can set the Azure SQL Database to implement recommendations automatically.</span></span> <span data-ttu-id="a57f7-154">Amint elérhetővé válnak a javaslatok azokat a rendszer automatikusan alkalmazza.</span><span class="sxs-lookup"><span data-stu-id="a57f7-154">As recommendations become available they will automatically be applied.</span></span> <span data-ttu-id="a57f7-155">Mint minden, a szolgáltatás által kezelt, ha a teljesítményre gyakorolt hatás negatív ajánlásokkal az ajánlás vissza lesznek vonva.</span><span class="sxs-lookup"><span data-stu-id="a57f7-155">As with all recommendations managed by the service if the performance impact is negative the recommendation will be reverted.</span></span>

1. <span data-ttu-id="a57f7-156">Az a **javaslatok** panelen kattintson a **automatizálás**:</span><span class="sxs-lookup"><span data-stu-id="a57f7-156">On the **Recommendations** blade, click **Automate**:</span></span>
   
    ![Az Advisor-beállítások](./media/sql-database-advisor-portal/settings.png)
2. <span data-ttu-id="a57f7-158">Válassza ki az automatikus műveleteket:</span><span class="sxs-lookup"><span data-stu-id="a57f7-158">Select actions to automate:</span></span>
   
    ![Ajánlott indexek](./media/sql-database-advisor-portal/automation.png)

### <a name="manually-run-the-recommended-t-sql-script"></a><span data-ttu-id="a57f7-160">Az ajánlott T-SQL parancsfájl manuális futtatása</span><span class="sxs-lookup"><span data-stu-id="a57f7-160">Manually run the recommended T-SQL script</span></span>
<span data-ttu-id="a57f7-161">Válassza ki a javaslat, és kattintson a **parancsfájl megtekintése**.</span><span class="sxs-lookup"><span data-stu-id="a57f7-161">Select any recommendation and then click **View script**.</span></span> <span data-ttu-id="a57f7-162">Futtassa a parancsfájlt az adatbázis a javaslat manuális alkalmazásához.</span><span class="sxs-lookup"><span data-stu-id="a57f7-162">Run this script against your database to manually apply the recommendation.</span></span>

<span data-ttu-id="a57f7-163">*Manuálisan végrehajtott indexek nem figyeli, és a szolgáltatás által a teljesítményre gyakorolt hatás érvényesíteni* , javasolt, ezek az indexek figyelheti azok biztosít teljesítménynövekedést érhet el és módosítsa vagy törölje azokat, ha szükséges ellenőrzése létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="a57f7-163">*Indexes that are manually executed are not monitored and validated for performance impact by the service* so it is suggested that you monitor these indexes after creation to verify they provide performance gains and adjust or delete them if necessary.</span></span> <span data-ttu-id="a57f7-164">Indexek létrehozásával kapcsolatos részletekért lásd: [a CREATE INDEX (Transact-SQL)](https://msdn.microsoft.com/library/ms188783.aspx).</span><span class="sxs-lookup"><span data-stu-id="a57f7-164">For details about creating indexes, see [CREATE INDEX (Transact-SQL)](https://msdn.microsoft.com/library/ms188783.aspx).</span></span>

### <a name="canceling-recommendations"></a><span data-ttu-id="a57f7-165">Javaslatok megszakítása</span><span class="sxs-lookup"><span data-stu-id="a57f7-165">Canceling recommendations</span></span>
<span data-ttu-id="a57f7-166">A javaslatok egy **függőben lévő**, **ellenőrzése**, vagy **sikeres** állapota lehet megszakítani.</span><span class="sxs-lookup"><span data-stu-id="a57f7-166">Recommendations that are in a **Pending**, **Verifying**, or **Success** status can be canceled.</span></span> <span data-ttu-id="a57f7-167">Javaslatok állapotú **Executing** nem szakítható meg.</span><span class="sxs-lookup"><span data-stu-id="a57f7-167">Recommendations with a status of **Executing** cannot be canceled.</span></span>

1. <span data-ttu-id="a57f7-168">Válassza ki az ajánlás a **hangolása előzmények** megnyitásához a **ajánlások részleteit** panelen.</span><span class="sxs-lookup"><span data-stu-id="a57f7-168">Select a recommendation in the **Tuning History** area to open the **recommendations details** blade.</span></span>
2. <span data-ttu-id="a57f7-169">Kattintson a **Mégse** megszakítja a folyamatot a javaslat alkalmazását.</span><span class="sxs-lookup"><span data-stu-id="a57f7-169">Click **Cancel** to abort the process of applying the recommendation.</span></span>

## <a name="monitoring-operations"></a><span data-ttu-id="a57f7-170">Figyelési műveletek</span><span class="sxs-lookup"><span data-stu-id="a57f7-170">Monitoring operations</span></span>
<span data-ttu-id="a57f7-171">A javaslat alkalmazása nem fordulhat elő, azonnal.</span><span class="sxs-lookup"><span data-stu-id="a57f7-171">Applying a recommendation might not happen instantaneously.</span></span> <span data-ttu-id="a57f7-172">A portál részletesen ajánlás állapotával kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="a57f7-172">The portal provides details regarding the status of recommendation.</span></span> <span data-ttu-id="a57f7-173">Az index lehet a lehetséges állapotok a következők:</span><span class="sxs-lookup"><span data-stu-id="a57f7-173">The following are possible states that an index can be in:</span></span>

| <span data-ttu-id="a57f7-174">status</span><span class="sxs-lookup"><span data-stu-id="a57f7-174">Status</span></span> | <span data-ttu-id="a57f7-175">Leírás</span><span class="sxs-lookup"><span data-stu-id="a57f7-175">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="a57f7-176">Függőben</span><span class="sxs-lookup"><span data-stu-id="a57f7-176">Pending</span></span> |<span data-ttu-id="a57f7-177">A javaslat parancs érkezett, és a végrehajtásra ütemezett alkalmazni.</span><span class="sxs-lookup"><span data-stu-id="a57f7-177">Apply recommendation command has been received and is scheduled for execution.</span></span> |
| <span data-ttu-id="a57f7-178">Végrehajtása</span><span class="sxs-lookup"><span data-stu-id="a57f7-178">Executing</span></span> |<span data-ttu-id="a57f7-179">A javaslat vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="a57f7-179">The recommendation is being applied.</span></span> |
| <span data-ttu-id="a57f7-180">Ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="a57f7-180">Verifying</span></span> |<span data-ttu-id="a57f7-181">A javaslat alkalmazása sikeresen, és a szolgáltatás a következő előnyöket mérés.</span><span class="sxs-lookup"><span data-stu-id="a57f7-181">Recommendation was successfully applied and the service is measuring the benefits.</span></span> |
| <span data-ttu-id="a57f7-182">Sikeres</span><span class="sxs-lookup"><span data-stu-id="a57f7-182">Success</span></span> |<span data-ttu-id="a57f7-183">A javaslat alkalmazása sikeresen és előnyök mért lett.</span><span class="sxs-lookup"><span data-stu-id="a57f7-183">Recommendation was successfully applied and benefits have been measured.</span></span> |
| <span data-ttu-id="a57f7-184">Hiba</span><span class="sxs-lookup"><span data-stu-id="a57f7-184">Error</span></span> |<span data-ttu-id="a57f7-185">Hiba történt az ajánlás alkalmazásának folyamatában.</span><span class="sxs-lookup"><span data-stu-id="a57f7-185">An error occurred during the process of applying the recommendation.</span></span> <span data-ttu-id="a57f7-186">Ez lehet átmeneti jellegű probléma, vagy esetleg a séma módosítsa a táblázat, és a parancsfájl már nem érvényes.</span><span class="sxs-lookup"><span data-stu-id="a57f7-186">This can be a transient issue, or possibly a schema change to the table and the script is no longer valid.</span></span> |
| <span data-ttu-id="a57f7-187">Visszaállítás</span><span class="sxs-lookup"><span data-stu-id="a57f7-187">Reverting</span></span> |<span data-ttu-id="a57f7-188">A javaslat alkalmazta, de nem performant megállapítása, és automatikusan vissza.</span><span class="sxs-lookup"><span data-stu-id="a57f7-188">The recommendation was applied, but has been deemed non-performant and is being automatically reverted.</span></span> |
| <span data-ttu-id="a57f7-189">Vissza</span><span class="sxs-lookup"><span data-stu-id="a57f7-189">Reverted</span></span> |<span data-ttu-id="a57f7-190">A javaslat vissza lett állítva.</span><span class="sxs-lookup"><span data-stu-id="a57f7-190">The recommendation was reverted.</span></span> |

<span data-ttu-id="a57f7-191">Kattintson egy folyamaton belüli ajánlott megoldás a listából a további részletek:</span><span class="sxs-lookup"><span data-stu-id="a57f7-191">Click an in-process recommendation from the list to see more details:</span></span>

![Ajánlott indexek](./media/sql-database-advisor-portal/operations.png)

### <a name="reverting-a-recommendation"></a><span data-ttu-id="a57f7-193">Azt ajánljuk, akkor a gyermekrekordokra</span><span class="sxs-lookup"><span data-stu-id="a57f7-193">Reverting a recommendation</span></span>
<span data-ttu-id="a57f7-194">Ha a teljesítmény ajánlások a javaslat alkalmazása (azaz a manuális futtatása a T-SQL parancsfájl) azt automatikusan visszaáll, ha úgy találja, a teljesítményre gyakorolt hatás negatív.</span><span class="sxs-lookup"><span data-stu-id="a57f7-194">If you used the performance recommendations to apply the recommendation (meaning you did not manually run the T-SQL script) it will automatically revert it if it finds the performance impact to be negative.</span></span> <span data-ttu-id="a57f7-195">Ha bármilyen okból egyszerűen vissza szeretné azt ajánljuk a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="a57f7-195">If for any reason you simply want to revert a recommendation you can do the following:</span></span>

1. <span data-ttu-id="a57f7-196">Válassza ki a sikeresen elvégzett ajánlást a **előzmények hangolása** területen.</span><span class="sxs-lookup"><span data-stu-id="a57f7-196">Select a successfully applied recommendation in the **Tuning history** area.</span></span>
2. <span data-ttu-id="a57f7-197">Kattintson a **visszaállítás** a a **javaslat részletei** panelen.</span><span class="sxs-lookup"><span data-stu-id="a57f7-197">Click **Revert** on the **recommendation details** blade.</span></span>

![Ajánlott indexek](./media/sql-database-advisor-portal/details.png)

## <a name="monitoring-performance-impact-of-index-recommendations"></a><span data-ttu-id="a57f7-199">Ajánlott index teljesítményre gyakorolt hatásának figyelése</span><span class="sxs-lookup"><span data-stu-id="a57f7-199">Monitoring performance impact of index recommendations</span></span>
<span data-ttu-id="a57f7-200">Javaslatok sikeres végrehajtása után (jelenleg műveletek index és csak a lekérdezések javaslatok parametrizálja) kattinthat **lekérdezési** az ajánlás Részletek panel megnyitásához [lekérdezési teljesítmény Insights](sql-database-query-performance.md) és tekintse meg a leggyakoribb lekérdezések a teljesítményre gyakorolt hatást.</span><span class="sxs-lookup"><span data-stu-id="a57f7-200">After recommendations are successfully implemented (currently, index operations and parameterize queries recommendations only) you can click **Query Insights** on the recommendation details blade to open [Query Performance Insights](sql-database-query-performance.md) and see the performance impact of your top queries.</span></span>

![A figyelő teljesítményre gyakorolt hatás](./media/sql-database-advisor-portal/query-insights.png)

## <a name="summary"></a><span data-ttu-id="a57f7-202">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="a57f7-202">Summary</span></span>
<span data-ttu-id="a57f7-203">Az Azure SQL-adatbázis SQL-adatbázis teljesítményének javítására vonatkozó javaslatokkal szolgál.</span><span class="sxs-lookup"><span data-stu-id="a57f7-203">Azure SQL Database provides recommendations for improving SQL database performance.</span></span> <span data-ttu-id="a57f7-204">Adja meg a T-SQL-parancsfájlok, továbbá az egyéni és a teljesen automatikus, az adatbázis optimalizálása, és végül a lekérdezési teljesítmény javítása a hasznos segítséget kap.</span><span class="sxs-lookup"><span data-stu-id="a57f7-204">By providing T-SQL scripts, as well as individual and fully-automatic, you get a helpful assistance in optimizing your database and ultimately improving query performance.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a57f7-205">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a57f7-205">Next steps</span></span>
<span data-ttu-id="a57f7-206">A javaslatok figyelése, és továbbra is alkalmazandó, pontosítsa a teljesítményt.</span><span class="sxs-lookup"><span data-stu-id="a57f7-206">Monitor your recommendations and continue to apply them to refine performance.</span></span> <span data-ttu-id="a57f7-207">Adatbázis-terhelések dinamikusak, és folyamatosan módosítása.</span><span class="sxs-lookup"><span data-stu-id="a57f7-207">Database workloads are dynamic and change continuously.</span></span> <span data-ttu-id="a57f7-208">Az Azure SQL-adatbázis továbbra is figyelni, és adja meg a javaslatok, javíthatja az adatbázis teljesítménye.</span><span class="sxs-lookup"><span data-stu-id="a57f7-208">Azure SQL Database will continue to monitor and provide recommendations that can potentially improve your database's performance.</span></span> 

* <span data-ttu-id="a57f7-209">Lásd: [automatikus hangolása](sql-database-automatic-tuning.md) tudhat meg többet az Azure SQL-adatbázis automatikus hangolása.</span><span class="sxs-lookup"><span data-stu-id="a57f7-209">See [Automatic tuning](sql-database-automatic-tuning.md) to learn more about the automatic tuning in Azure SQL Database.</span></span>
* <span data-ttu-id="a57f7-210">Lásd: [teljesítmény javaslatok](sql-database-advisor.md) teljesítmény javaslatok az Azure SQL Database áttekintését.</span><span class="sxs-lookup"><span data-stu-id="a57f7-210">See [Performance recommendations](sql-database-advisor.md) for an overview of Azure SQL Database performance recommendations.</span></span>
* <span data-ttu-id="a57f7-211">Lásd: [lekérdezési teljesítmény Insights](sql-database-query-performance.md) a teljesítményre gyakorolt hatását a leggyakoribb lekérdezések megtekintésének megismerése.</span><span class="sxs-lookup"><span data-stu-id="a57f7-211">See [Query Performance Insights](sql-database-query-performance.md) to learn about viewing the performance impact of your top queries.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a57f7-212">További források</span><span class="sxs-lookup"><span data-stu-id="a57f7-212">Additional resources</span></span>
* [<span data-ttu-id="a57f7-213">A Lekérdezéstár</span><span class="sxs-lookup"><span data-stu-id="a57f7-213">Query Store</span></span>](https://msdn.microsoft.com/library/dn817826.aspx)
* [<span data-ttu-id="a57f7-214">INDEX LÉTREHOZÁSA</span><span class="sxs-lookup"><span data-stu-id="a57f7-214">CREATE INDEX</span></span>](https://msdn.microsoft.com/library/ms188783.aspx)
* [<span data-ttu-id="a57f7-215">Szerepköralapú hozzáférés-vezérlés</span><span class="sxs-lookup"><span data-stu-id="a57f7-215">Role-based access control</span></span>](../active-directory/role-based-access-control-what-is.md)

