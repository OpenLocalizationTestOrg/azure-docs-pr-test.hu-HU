---
title: "Telepítse át a meglévő Azure adatraktár prémium szintű Storage |} Microsoft Docs"
description: "Prémium szintű storage egy meglévő data warehouse-ba történő áttelepítéséhez"
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: barbkess
editor: 
ms.assetid: 04b05dea-c066-44a0-9751-0774eb84c689
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: migrate
ms.date: 11/29/2016
ms.author: elbutter;barbkess
ms.openlocfilehash: 860e50b532b4b0a21d3be54f087730070b0e56bb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="migrate-your-data-warehouse-to-premium-storage"></a><span data-ttu-id="7362b-103">Prémium szintű Storage az adatraktár áttelepítése</span><span class="sxs-lookup"><span data-stu-id="7362b-103">Migrate your data warehouse to premium storage</span></span>
<span data-ttu-id="7362b-104">Új Azure SQL Data Warehouse [prémium szintű storage, a nagyobb teljesítmény kiszámíthatóságot][premium storage for greater performance predictability].</span><span class="sxs-lookup"><span data-stu-id="7362b-104">Azure SQL Data Warehouse recently introduced [premium storage for greater performance predictability][premium storage for greater performance predictability].</span></span> <span data-ttu-id="7362b-105">Standard szintű tárolót a jelenleg létező adatraktárak prémium szintű Storage most telepíthető át.</span><span class="sxs-lookup"><span data-stu-id="7362b-105">Existing data warehouses currently on standard storage can now be migrated to premium storage.</span></span> <span data-ttu-id="7362b-106">Kihasználhatja az automatikus áttelepítése, vagy ha jobban szeret szabályozhatja, mikor kell áttelepíteni (amely does tartalmaz, amely bizonyos időre leállítást), teheti az áttelepítés saját maga.</span><span class="sxs-lookup"><span data-stu-id="7362b-106">You can take advantage of automatic migration, or if you prefer to control when to migrate (which does involve some downtime), you can do the migration yourself.</span></span>

<span data-ttu-id="7362b-107">Ha egynél több adatraktár, használja a [automatikus áttelepítése ütemezés] [ automatic migration schedule] meghatározni, ha azt is áttelepíthetők.</span><span class="sxs-lookup"><span data-stu-id="7362b-107">If you have more than one data warehouse, use the [automatic migration schedule][automatic migration schedule] to determine when it will also be migrated.</span></span>

## <a name="determine-storage-type"></a><span data-ttu-id="7362b-108">Tárolási típus meghatározása</span><span class="sxs-lookup"><span data-stu-id="7362b-108">Determine storage type</span></span>
<span data-ttu-id="7362b-109">Ha egy adatraktár hozott létre a következő dátum előtt, Standard szintű storage jelenleg használt.</span><span class="sxs-lookup"><span data-stu-id="7362b-109">If you created a data warehouse before the following dates, you are currently using standard storage.</span></span>

| <span data-ttu-id="7362b-110">**Régió**</span><span class="sxs-lookup"><span data-stu-id="7362b-110">**Region**</span></span> | <span data-ttu-id="7362b-111">**Ez a dátum előtt létrehozott adatraktár**</span><span class="sxs-lookup"><span data-stu-id="7362b-111">**Data warehouse created before this date**</span></span> |
|:--- |:--- |
| <span data-ttu-id="7362b-112">Kelet-Ausztrália</span><span class="sxs-lookup"><span data-stu-id="7362b-112">Australia East</span></span> |<span data-ttu-id="7362b-113">Prémium szintű storage még nem érhető el</span><span class="sxs-lookup"><span data-stu-id="7362b-113">Premium storage not yet available</span></span> |
| <span data-ttu-id="7362b-114">Kelet-Kína</span><span class="sxs-lookup"><span data-stu-id="7362b-114">China East</span></span> |<span data-ttu-id="7362b-115">2016. november 1.</span><span class="sxs-lookup"><span data-stu-id="7362b-115">November 1, 2016</span></span> |
| <span data-ttu-id="7362b-116">Észak-Kína</span><span class="sxs-lookup"><span data-stu-id="7362b-116">China North</span></span> |<span data-ttu-id="7362b-117">2016. november 1.</span><span class="sxs-lookup"><span data-stu-id="7362b-117">November 1, 2016</span></span> |
| <span data-ttu-id="7362b-118">Közép-Németország</span><span class="sxs-lookup"><span data-stu-id="7362b-118">Germany Central</span></span> |<span data-ttu-id="7362b-119">2016. november 1.</span><span class="sxs-lookup"><span data-stu-id="7362b-119">November 1, 2016</span></span> |
| <span data-ttu-id="7362b-120">Északkelet-Németország</span><span class="sxs-lookup"><span data-stu-id="7362b-120">Germany Northeast</span></span> |<span data-ttu-id="7362b-121">2016. november 1.</span><span class="sxs-lookup"><span data-stu-id="7362b-121">November 1, 2016</span></span> |
| <span data-ttu-id="7362b-122">Nyugat-India</span><span class="sxs-lookup"><span data-stu-id="7362b-122">India West</span></span> |<span data-ttu-id="7362b-123">Prémium szintű storage még nem érhető el</span><span class="sxs-lookup"><span data-stu-id="7362b-123">Premium storage not yet available</span></span> |
| <span data-ttu-id="7362b-124">Nyugat-Japán</span><span class="sxs-lookup"><span data-stu-id="7362b-124">Japan West</span></span> |<span data-ttu-id="7362b-125">Prémium szintű storage még nem érhető el</span><span class="sxs-lookup"><span data-stu-id="7362b-125">Premium storage not yet available</span></span> |
| <span data-ttu-id="7362b-126">USA északi középső régiója</span><span class="sxs-lookup"><span data-stu-id="7362b-126">North Central US</span></span> |<span data-ttu-id="7362b-127">2016. november 10.</span><span class="sxs-lookup"><span data-stu-id="7362b-127">November 10, 2016</span></span> |

## <a name="automatic-migration-details"></a><span data-ttu-id="7362b-128">Automatikus áttelepítése részletei</span><span class="sxs-lookup"><span data-stu-id="7362b-128">Automatic migration details</span></span>
<span data-ttu-id="7362b-129">Alapértelmezés szerint kiforrottabb lesz az adatbázis az Ön közötti 18:00:00 és 6:00-kor a régió helyi idő alatt a [automatikus áttelepítése ütemezés][automatic migration schedule].</span><span class="sxs-lookup"><span data-stu-id="7362b-129">By default, we will migrate your database for you between 6:00 PM and 6:00 AM in your region's local time during the [automatic migration schedule][automatic migration schedule].</span></span> <span data-ttu-id="7362b-130">A meglévő adatraktár használhatatlan lesz, az áttelepítés során.</span><span class="sxs-lookup"><span data-stu-id="7362b-130">Your existing data warehouse will be unusable during the migration.</span></span> <span data-ttu-id="7362b-131">Az áttelepítés adatraktár / / terabájt tárhelyet körülbelül egy óra igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="7362b-131">The migration will take approximately one hour per terabyte of storage per data warehouse.</span></span> <span data-ttu-id="7362b-132">Nem kell fizetnie az automatikus áttelepítése bármely részében.</span><span class="sxs-lookup"><span data-stu-id="7362b-132">You will not be charged during any portion of the automatic migration.</span></span>

> [!NOTE]
> <span data-ttu-id="7362b-133">Ha az áttelepítés befejeződött, az adatraktár lesz ismét online állapotú, és használható.</span><span class="sxs-lookup"><span data-stu-id="7362b-133">When the migration is complete, your data warehouse will be back online and usable.</span></span>
>
>

<span data-ttu-id="7362b-134">A Microsoft az alábbi lépéseket az áttelepítéshez (ezek nem igényelnek beavatkozást bármely beavatkozás) tart.</span><span class="sxs-lookup"><span data-stu-id="7362b-134">Microsoft is taking the following steps to complete the migration (these do not require any involvement on your part).</span></span> <span data-ttu-id="7362b-135">Ebben a példában képzelhető el, hogy a meglévő adatraktár szabványos tárolón jelenleg neve "MyDW."</span><span class="sxs-lookup"><span data-stu-id="7362b-135">In this example, imagine that your existing data warehouse on standard storage is currently named “MyDW.”</span></span>

1. <span data-ttu-id="7362b-136">Microsoft átnevezi "MyDW" "[időbélyeg] MyDW_DO_NOT_USE_."</span><span class="sxs-lookup"><span data-stu-id="7362b-136">Microsoft renames “MyDW” to “MyDW_DO_NOT_USE_[Timestamp].”</span></span>
2. <span data-ttu-id="7362b-137">A Microsoft leállítja "[időbélyeg] MyDW_DO_NOT_USE_."</span><span class="sxs-lookup"><span data-stu-id="7362b-137">Microsoft pauses “MyDW_DO_NOT_USE_[Timestamp].”</span></span> <span data-ttu-id="7362b-138">Ebben az időszakban a biztonsági másolat használatban van.</span><span class="sxs-lookup"><span data-stu-id="7362b-138">During this time, a backup is taken.</span></span> <span data-ttu-id="7362b-139">Ha azt a folyamat során problémába ütközik jelenhet meg több szüneteltetése és folytatása.</span><span class="sxs-lookup"><span data-stu-id="7362b-139">You may see multiple pauses and resumes if we encounter any issues during this process.</span></span>
3. <span data-ttu-id="7362b-140">Microsoft hoz létre egy új data warehouse "MyDW" nevű a prémium szintű storage, a 2. lépésben készült biztonsági másolatból.</span><span class="sxs-lookup"><span data-stu-id="7362b-140">Microsoft creates a new data warehouse named “MyDW” on premium storage from the backup taken in step 2.</span></span> <span data-ttu-id="7362b-141">"MyDW" csak kerülnek bele a visszaállítás befejezése után.</span><span class="sxs-lookup"><span data-stu-id="7362b-141">“MyDW” will not appear until after the restore is complete.</span></span>
4. <span data-ttu-id="7362b-142">A visszaállítás befejezése után, "MyDW" arkusz ugyanazon adattárházegységek és állapota (felfüggesztett aktív), hogy az áttelepítés előtt volt.</span><span class="sxs-lookup"><span data-stu-id="7362b-142">After the restore is complete, “MyDW” returns to the same data warehouse units and state (paused or active) that it was before the migration.</span></span>
5. <span data-ttu-id="7362b-143">Az áttelepítés befejezése után, a Microsoft törlése "MyDW_DO_NOT_USE_ [időbélyeg]".</span><span class="sxs-lookup"><span data-stu-id="7362b-143">After the migration is complete, Microsoft deletes “MyDW_DO_NOT_USE_[Timestamp]”.</span></span>

> [!NOTE]
> <span data-ttu-id="7362b-144">A következő beállítások nem jelenik meg az áttelepítés részeként:</span><span class="sxs-lookup"><span data-stu-id="7362b-144">The following settings do not carry over as part of the migration:</span></span>
>
> * <span data-ttu-id="7362b-145">Az adatbázis szintjén naplózás kell ismét engedélyezni kell.</span><span class="sxs-lookup"><span data-stu-id="7362b-145">Auditing at the database level needs to be re-enabled.</span></span>
> * <span data-ttu-id="7362b-146">Az adatbázis szintjén tűzfalszabályok újból hozzá kell.</span><span class="sxs-lookup"><span data-stu-id="7362b-146">Firewall rules at the database level need to be re-added.</span></span> <span data-ttu-id="7362b-147">A kiszolgáló szintjén tűzfalszabályok nem érintettek.</span><span class="sxs-lookup"><span data-stu-id="7362b-147">Firewall rules at the server level are not affected.</span></span>
>
>

### <a name="automatic-migration-schedule"></a><span data-ttu-id="7362b-148">Automatikus áttelepítése ütemezése</span><span class="sxs-lookup"><span data-stu-id="7362b-148">Automatic migration schedule</span></span>
<span data-ttu-id="7362b-149">Automatikus áttelepítések 18:00:00 és 6:00-kor (helyi idő régiónként) a következő kimaradás ütemezése során kerül sor.</span><span class="sxs-lookup"><span data-stu-id="7362b-149">Automatic migrations occur between 6:00 PM and 6:00 AM (local time per region) during the following outage schedule.</span></span>

| <span data-ttu-id="7362b-150">**Régió**</span><span class="sxs-lookup"><span data-stu-id="7362b-150">**Region**</span></span> | <span data-ttu-id="7362b-151">**Becsült kezdő dátuma**</span><span class="sxs-lookup"><span data-stu-id="7362b-151">**Estimated start date**</span></span> | <span data-ttu-id="7362b-152">**Becsült befejezési dátum**</span><span class="sxs-lookup"><span data-stu-id="7362b-152">**Estimated end date**</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="7362b-153">Kelet-Ausztrália</span><span class="sxs-lookup"><span data-stu-id="7362b-153">Australia East</span></span> |<span data-ttu-id="7362b-154">Még nem határozza meg</span><span class="sxs-lookup"><span data-stu-id="7362b-154">Not determined yet</span></span> |<span data-ttu-id="7362b-155">Még nem határozza meg</span><span class="sxs-lookup"><span data-stu-id="7362b-155">Not determined yet</span></span> |
| <span data-ttu-id="7362b-156">Kelet-Kína</span><span class="sxs-lookup"><span data-stu-id="7362b-156">China East</span></span> |<span data-ttu-id="7362b-157">2017. január 9.</span><span class="sxs-lookup"><span data-stu-id="7362b-157">January 9, 2017</span></span> |<span data-ttu-id="7362b-158">2017. január 13.</span><span class="sxs-lookup"><span data-stu-id="7362b-158">January 13, 2017</span></span> |
| <span data-ttu-id="7362b-159">Észak-Kína</span><span class="sxs-lookup"><span data-stu-id="7362b-159">China North</span></span> |<span data-ttu-id="7362b-160">2017. január 9.</span><span class="sxs-lookup"><span data-stu-id="7362b-160">January 9, 2017</span></span> |<span data-ttu-id="7362b-161">2017. január 13.</span><span class="sxs-lookup"><span data-stu-id="7362b-161">January 13, 2017</span></span> |
| <span data-ttu-id="7362b-162">Közép-Németország</span><span class="sxs-lookup"><span data-stu-id="7362b-162">Germany Central</span></span> |<span data-ttu-id="7362b-163">2017. január 9.</span><span class="sxs-lookup"><span data-stu-id="7362b-163">January 9, 2017</span></span> |<span data-ttu-id="7362b-164">2017. január 13.</span><span class="sxs-lookup"><span data-stu-id="7362b-164">January 13, 2017</span></span> |
| <span data-ttu-id="7362b-165">Északkelet-Németország</span><span class="sxs-lookup"><span data-stu-id="7362b-165">Germany Northeast</span></span> |<span data-ttu-id="7362b-166">2017. január 9.</span><span class="sxs-lookup"><span data-stu-id="7362b-166">January 9, 2017</span></span> |<span data-ttu-id="7362b-167">2017. január 13.</span><span class="sxs-lookup"><span data-stu-id="7362b-167">January 13, 2017</span></span> |
| <span data-ttu-id="7362b-168">Nyugat-India</span><span class="sxs-lookup"><span data-stu-id="7362b-168">India West</span></span> |<span data-ttu-id="7362b-169">Még nem határozza meg</span><span class="sxs-lookup"><span data-stu-id="7362b-169">Not determined yet</span></span> |<span data-ttu-id="7362b-170">Még nem határozza meg</span><span class="sxs-lookup"><span data-stu-id="7362b-170">Not determined yet</span></span> |
| <span data-ttu-id="7362b-171">Nyugat-Japán</span><span class="sxs-lookup"><span data-stu-id="7362b-171">Japan West</span></span> |<span data-ttu-id="7362b-172">Még nem határozza meg</span><span class="sxs-lookup"><span data-stu-id="7362b-172">Not determined yet</span></span> |<span data-ttu-id="7362b-173">Még nem határozza meg</span><span class="sxs-lookup"><span data-stu-id="7362b-173">Not determined yet</span></span> |
| <span data-ttu-id="7362b-174">USA északi középső régiója</span><span class="sxs-lookup"><span data-stu-id="7362b-174">North Central US</span></span> |<span data-ttu-id="7362b-175">2017. január 9.</span><span class="sxs-lookup"><span data-stu-id="7362b-175">January 9, 2017</span></span> |<span data-ttu-id="7362b-176">2017. január 13.</span><span class="sxs-lookup"><span data-stu-id="7362b-176">January 13, 2017</span></span> |

## <a name="self-migration-to-premium-storage"></a><span data-ttu-id="7362b-177">Prémium szintű Storage önálló áttelepítés</span><span class="sxs-lookup"><span data-stu-id="7362b-177">Self-migration to premium storage</span></span>
<span data-ttu-id="7362b-178">Ha szabályozni szeretné a leállás esetén a rendszer, az alábbi lépések segítségével egy meglévő adatraktár szabványos tároló áttelepítése a prémium szintű Storage.</span><span class="sxs-lookup"><span data-stu-id="7362b-178">If you want to control when your downtime will occur, you can use the following steps to migrate an existing data warehouse on standard storage to premium storage.</span></span> <span data-ttu-id="7362b-179">Ha ezt a lehetőséget választja, akkor előtt végezze el az önálló áttelepítés automatikus áttelepítésének megkezdése az adott régióban.</span><span class="sxs-lookup"><span data-stu-id="7362b-179">If you choose this option, you must complete the self-migration before the automatic migration begins in that region.</span></span> <span data-ttu-id="7362b-180">Ez biztosítja, hogy az automatikus áttelepítés ütközést okoz veszélyének elkerülése érdekében (tekintse meg a [automatikus áttelepítése ütemezés][automatic migration schedule]).</span><span class="sxs-lookup"><span data-stu-id="7362b-180">This ensures that you avoid any risk of the automatic migration causing a conflict (refer to the [automatic migration schedule][automatic migration schedule]).</span></span>

### <a name="self-migration-instructions"></a><span data-ttu-id="7362b-181">Önálló áttelepítési utasítások</span><span class="sxs-lookup"><span data-stu-id="7362b-181">Self-migration instructions</span></span>
<span data-ttu-id="7362b-182">Az adatraktár áttelepíteni, saját magának, a biztonsági másolat, és állítsa vissza a szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="7362b-182">To migrate your data warehouse yourself, use the backup and restore features.</span></span> <span data-ttu-id="7362b-183">A helyreállítási rész az áttelepítés várható / terabájt tárhelyet körülbelül egy óra érvénybe / data warehouse-bA.</span><span class="sxs-lookup"><span data-stu-id="7362b-183">The restore portion of the migration is expected to take around one hour per terabyte of storage per data warehouse.</span></span> <span data-ttu-id="7362b-184">Ha meg szeretné tartani a néven után az áttelepítés akkor fejeződött be, kövesse a [áttelepítéskor átnevezésére szolgáló lépéseket][steps to rename during migration].</span><span class="sxs-lookup"><span data-stu-id="7362b-184">If you want to keep the same name after migration is complete, follow the [steps to rename during migration][steps to rename during migration].</span></span>

1. <span data-ttu-id="7362b-185">[Felfüggesztés] [ Pause] az adatraktár.</span><span class="sxs-lookup"><span data-stu-id="7362b-185">[Pause][Pause] your data warehouse.</span></span> <span data-ttu-id="7362b-186">Ehhez szükséges, az automatikus biztonsági mentés.</span><span class="sxs-lookup"><span data-stu-id="7362b-186">This takes an automatic backup.</span></span>
2. <span data-ttu-id="7362b-187">[Visszaállítás] [ Restore] a legutóbbi pillanatképből.</span><span class="sxs-lookup"><span data-stu-id="7362b-187">[Restore][Restore] from your most recent snapshot.</span></span>
3. <span data-ttu-id="7362b-188">Törölje a meglévő adatraktár szabványos tárolón.</span><span class="sxs-lookup"><span data-stu-id="7362b-188">Delete your existing data warehouse on standard storage.</span></span> <span data-ttu-id="7362b-189">**Ha ezt a lépést nem sikerül, akkor mindkét adatraktárak fogjuk számlázni.**</span><span class="sxs-lookup"><span data-stu-id="7362b-189">**If you fail to do this step, you will be charged for both data warehouses.**</span></span>

> [!NOTE]
> <span data-ttu-id="7362b-190">A következő beállítások nem jelenik meg az áttelepítés részeként:</span><span class="sxs-lookup"><span data-stu-id="7362b-190">The following settings do not carry over as part of the migration:</span></span>
>
> * <span data-ttu-id="7362b-191">Az adatbázis szintjén naplózás kell ismét engedélyezni kell.</span><span class="sxs-lookup"><span data-stu-id="7362b-191">Auditing at the database level needs to be re-enabled.</span></span>
> * <span data-ttu-id="7362b-192">Az adatbázis szintjén tűzfalszabályok újból hozzá kell.</span><span class="sxs-lookup"><span data-stu-id="7362b-192">Firewall rules at the database level need to be re-added.</span></span> <span data-ttu-id="7362b-193">A kiszolgáló szintjén tűzfalszabályok nem érintettek.</span><span class="sxs-lookup"><span data-stu-id="7362b-193">Firewall rules at the server level are not affected.</span></span>
>
>

#### <a name="rename-data-warehouse-during-migration-optional"></a><span data-ttu-id="7362b-194">Adatraktár átnevezése (választható) az áttelepítés során</span><span class="sxs-lookup"><span data-stu-id="7362b-194">Rename data warehouse during migration (optional)</span></span>
<span data-ttu-id="7362b-195">Két adatbázis ugyanazon a logikai kiszolgálón azonos neve nem lehet.</span><span class="sxs-lookup"><span data-stu-id="7362b-195">Two databases on the same logical server cannot have the same name.</span></span> <span data-ttu-id="7362b-196">Az SQL Data Warehouse mostantól támogatja a adatraktár átnevezése.</span><span class="sxs-lookup"><span data-stu-id="7362b-196">SQL Data Warehouse now supports the ability to rename a data warehouse.</span></span>

<span data-ttu-id="7362b-197">Ebben a példában képzelhető el, hogy a meglévő adatraktár szabványos tárolón jelenleg neve "MyDW."</span><span class="sxs-lookup"><span data-stu-id="7362b-197">In this example, imagine that your existing data warehouse on standard storage is currently named “MyDW.”</span></span>

1. <span data-ttu-id="7362b-198">Nevezze át a "MyDW" a következő ALTER DATABASE parancs használatával.</span><span class="sxs-lookup"><span data-stu-id="7362b-198">Rename "MyDW" by using the following ALTER DATABASE command.</span></span> <span data-ttu-id="7362b-199">(A példában azt kell nevezni "MyDW_BeforeMigration.")  Ez a parancs minden meglévő tranzakciókat, és a sikeres a master adatbázisban kell végrehajtani.</span><span class="sxs-lookup"><span data-stu-id="7362b-199">(In this example, we'll rename it "MyDW_BeforeMigration.")  This command stops all existing transactions, and must be done in the master database to succeed.</span></span>
   ```
   ALTER DATABASE CurrentDatabasename MODIFY NAME = NewDatabaseName;
   ```
2. <span data-ttu-id="7362b-200">[Felfüggesztés] [ Pause] "MyDW_BeforeMigration."</span><span class="sxs-lookup"><span data-stu-id="7362b-200">[Pause][Pause] "MyDW_BeforeMigration."</span></span> <span data-ttu-id="7362b-201">Ehhez szükséges, az automatikus biztonsági mentés.</span><span class="sxs-lookup"><span data-stu-id="7362b-201">This takes an automatic backup.</span></span>
3. <span data-ttu-id="7362b-202">[Visszaállítás] [ Restore] a legutóbbi a pillanatkép készítése a szokásosnál (például "MyDW") nevű új adatbázis.</span><span class="sxs-lookup"><span data-stu-id="7362b-202">[Restore][Restore] from your most recent snapshot a new database with the name it used to be (for example, "MyDW").</span></span>
4. <span data-ttu-id="7362b-203">Törölje a "MyDW_BeforeMigration."</span><span class="sxs-lookup"><span data-stu-id="7362b-203">Delete "MyDW_BeforeMigration."</span></span> <span data-ttu-id="7362b-204">**Ha ezt a lépést nem sikerül, akkor mindkét adatraktárak fogjuk számlázni.**</span><span class="sxs-lookup"><span data-stu-id="7362b-204">**If you fail to do this step, you will be charged for both data warehouses.**</span></span>


## <a name="next-steps"></a><span data-ttu-id="7362b-205">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7362b-205">Next steps</span></span>
<span data-ttu-id="7362b-206">Prémium szintű Storage módosítását akkor is növekvő számának a blob-adatbázisfájlok a mögöttes architektúráját, valamint az adatraktár.</span><span class="sxs-lookup"><span data-stu-id="7362b-206">With the change to premium storage, you also have an increased number of database blob files in the underlying architecture of your data warehouse.</span></span> <span data-ttu-id="7362b-207">Ez a változás teljesítménybeli előnyökben maximalizálása érdekében a fürtözött oszlopcentrikus indexek újraépítése az alábbi parancsfájl használatával.</span><span class="sxs-lookup"><span data-stu-id="7362b-207">To maximize the performance benefits of this change, rebuild your clustered columnstore indexes by using the following script.</span></span> <span data-ttu-id="7362b-208">A parancsfájl használatára, a további tárolókban kényszeríti a meglévő adatok egy részét.</span><span class="sxs-lookup"><span data-stu-id="7362b-208">The script works by forcing some of your existing data to the additional blobs.</span></span> <span data-ttu-id="7362b-209">Ha nem tesz semmit, az adatok fog természetes terjesztenie adott idő alatt, több adatot tölt be a táblába.</span><span class="sxs-lookup"><span data-stu-id="7362b-209">If you take no action, the data will naturally redistribute over time as you load more data into your tables.</span></span>

<span data-ttu-id="7362b-210">**Előfeltételek:**</span><span class="sxs-lookup"><span data-stu-id="7362b-210">**Prerequisites:**</span></span>

- <span data-ttu-id="7362b-211">Az adatraktár futtasson-e 1000 adattárházegységek vagy annál újabb (lásd: [méretezési számítási teljesítményt][scale compute power]).</span><span class="sxs-lookup"><span data-stu-id="7362b-211">The data warehouse should run with 1,000 data warehouse units or higher (see [scale compute power][scale compute power]).</span></span>
- <span data-ttu-id="7362b-212">A parancsfájl végrehajtása felhasználó kell lennie a [mediumrc szerepkör] [ mediumrc role] vagy újabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="7362b-212">The user executing the script should be in the [mediumrc role][mediumrc role] or higher.</span></span> <span data-ttu-id="7362b-213">Felhasználó hozzáadása ehhez a szerepkörhöz, hajtsa végre a következő:````EXEC sp_addrolemember 'xlargerc', 'MyUser'````</span><span class="sxs-lookup"><span data-stu-id="7362b-213">To add a user to this role, execute the following: ````EXEC sp_addrolemember 'xlargerc', 'MyUser'````</span></span>

````sql
-------------------------------------------------------------------------------
-- Step 1: Create table to control index rebuild
-- Run as user in mediumrc or higher
--------------------------------------------------------------------------------
create table sql_statements
WITH (distribution = round_robin)
as select
    'alter index all on ' + s.name + '.' + t.NAME + ' rebuild;' as statement,
    row_number() over (order by s.name, t.name) as sequence
from
    sys.schemas s
    inner join sys.tables t
        on s.schema_id = t.schema_id
where
    is_external = 0
;
go

--------------------------------------------------------------------------------
-- Step 2: Execute index rebuilds. If script fails, the below can be re-run to restart where last left off.
-- Run as user in mediumrc or higher
--------------------------------------------------------------------------------

declare @nbr_statements int = (select count(*) from sql_statements)
declare @i int = 1
while(@i <= @nbr_statements)
begin
      declare @statement nvarchar(1000)= (select statement from sql_statements where sequence = @i)
      print cast(getdate() as nvarchar(1000)) + ' Executing... ' + @statement
      exec (@statement)
      delete from sql_statements where sequence = @i
      set @i += 1
end;
go
-------------------------------------------------------------------------------
-- Step 3: Clean up table created in Step 1
--------------------------------------------------------------------------------
drop table sql_statements;
go
````

<span data-ttu-id="7362b-214">Ha problémába ütközik az adatraktárban [hozzon létre egy támogatási jegy] [ create a support ticket] és a hivatkozás "áttelepítés prémium szintű Storage" lehetséges okozta.</span><span class="sxs-lookup"><span data-stu-id="7362b-214">If you encounter any issues with your data warehouse, [create a support ticket][create a support ticket] and reference “migration to premium storage” as the possible cause.</span></span>

<!--Image references-->

<!--Article references-->
[automatic migration schedule]: #automatic-migration-schedule
[self-migration to Premium Storage]: #self-migration-to-premium-storage
[create a support ticket]: sql-data-warehouse-get-started-create-support-ticket.md
[Azure paired region]: best-practices-availability-paired-regions.md
[main documentation site]: services/sql-data-warehouse.md
[Pause]: sql-data-warehouse-manage-compute-portal.md#pause-compute
[Restore]: sql-data-warehouse-restore-database-portal.md
[steps to rename during migration]: #optional-steps-to-rename-during-migration
[scale compute power]: sql-data-warehouse-manage-compute-portal.md#scale-compute-power
[mediumrc role]: sql-data-warehouse-develop-concurrency.md

<!--MSDN references-->


<!--Other Web references-->
[Premium Storage for greater performance predictability]: https://azure.microsoft.com/en-us/blog/azure-sql-data-warehouse-introduces-premium-storage-for-greater-performance/
[Azure Portal]: https://portal.azure.com
