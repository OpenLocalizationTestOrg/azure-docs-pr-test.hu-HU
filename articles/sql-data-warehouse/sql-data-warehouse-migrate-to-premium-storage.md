---
title: "aaaMigrate a meglévő Azure adatraktár toopremium tárolási |} Microsoft Docs"
description: "Áttelepítését egy meglévő adatraktár toopremium adattárolás"
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
ms.openlocfilehash: 145199c2da1f6f1fb8898626cd04886c42d82204
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-your-data-warehouse-toopremium-storage"></a><span data-ttu-id="b131d-103">A data warehouse toopremium tároló áttelepítése</span><span class="sxs-lookup"><span data-stu-id="b131d-103">Migrate your data warehouse toopremium storage</span></span>
<span data-ttu-id="b131d-104">Új Azure SQL Data Warehouse [prémium szintű storage, a nagyobb teljesítmény kiszámíthatóságot][premium storage for greater performance predictability].</span><span class="sxs-lookup"><span data-stu-id="b131d-104">Azure SQL Data Warehouse recently introduced [premium storage for greater performance predictability][premium storage for greater performance predictability].</span></span> <span data-ttu-id="b131d-105">Jelenleg a szabványos tárolón adatraktárakra mostantól lehet meglévő adatok toopremium tárolási át.</span><span class="sxs-lookup"><span data-stu-id="b131d-105">Existing data warehouses currently on standard storage can now be migrated toopremium storage.</span></span> <span data-ttu-id="b131d-106">Automatikus áttelepítése előnyeit élvezheti, vagy ha jobban szeret toocontrol során (amely tartalmaz, amely bizonyos időre leállítást) toomigrate, teheti hello áttelepítési saját maga.</span><span class="sxs-lookup"><span data-stu-id="b131d-106">You can take advantage of automatic migration, or if you prefer toocontrol when toomigrate (which does involve some downtime), you can do hello migration yourself.</span></span>

<span data-ttu-id="b131d-107">Ha egynél több adatraktár, használja a hello [automatikus áttelepítése ütemezés] [ automatic migration schedule] toodetermine, ha akkor is telepíthetők át.</span><span class="sxs-lookup"><span data-stu-id="b131d-107">If you have more than one data warehouse, use hello [automatic migration schedule][automatic migration schedule] toodetermine when it will also be migrated.</span></span>

## <a name="determine-storage-type"></a><span data-ttu-id="b131d-108">Tárolási típus meghatározása</span><span class="sxs-lookup"><span data-stu-id="b131d-108">Determine storage type</span></span>
<span data-ttu-id="b131d-109">Ha egy adatraktár hello következő dátumok előtt létrehozott, Standard szintű storage jelenleg használt.</span><span class="sxs-lookup"><span data-stu-id="b131d-109">If you created a data warehouse before hello following dates, you are currently using standard storage.</span></span>

| <span data-ttu-id="b131d-110">**Régió**</span><span class="sxs-lookup"><span data-stu-id="b131d-110">**Region**</span></span> | <span data-ttu-id="b131d-111">**Ez a dátum előtt létrehozott adatraktár**</span><span class="sxs-lookup"><span data-stu-id="b131d-111">**Data warehouse created before this date**</span></span> |
|:--- |:--- |
| <span data-ttu-id="b131d-112">Kelet-Ausztrália</span><span class="sxs-lookup"><span data-stu-id="b131d-112">Australia East</span></span> |<span data-ttu-id="b131d-113">Prémium szintű storage még nem érhető el</span><span class="sxs-lookup"><span data-stu-id="b131d-113">Premium storage not yet available</span></span> |
| <span data-ttu-id="b131d-114">Kelet-Kína</span><span class="sxs-lookup"><span data-stu-id="b131d-114">China East</span></span> |<span data-ttu-id="b131d-115">2016. november 1.</span><span class="sxs-lookup"><span data-stu-id="b131d-115">November 1, 2016</span></span> |
| <span data-ttu-id="b131d-116">Észak-Kína</span><span class="sxs-lookup"><span data-stu-id="b131d-116">China North</span></span> |<span data-ttu-id="b131d-117">2016. november 1.</span><span class="sxs-lookup"><span data-stu-id="b131d-117">November 1, 2016</span></span> |
| <span data-ttu-id="b131d-118">Közép-Németország</span><span class="sxs-lookup"><span data-stu-id="b131d-118">Germany Central</span></span> |<span data-ttu-id="b131d-119">2016. november 1.</span><span class="sxs-lookup"><span data-stu-id="b131d-119">November 1, 2016</span></span> |
| <span data-ttu-id="b131d-120">Északkelet-Németország</span><span class="sxs-lookup"><span data-stu-id="b131d-120">Germany Northeast</span></span> |<span data-ttu-id="b131d-121">2016. november 1.</span><span class="sxs-lookup"><span data-stu-id="b131d-121">November 1, 2016</span></span> |
| <span data-ttu-id="b131d-122">Nyugat-India</span><span class="sxs-lookup"><span data-stu-id="b131d-122">India West</span></span> |<span data-ttu-id="b131d-123">Prémium szintű storage még nem érhető el</span><span class="sxs-lookup"><span data-stu-id="b131d-123">Premium storage not yet available</span></span> |
| <span data-ttu-id="b131d-124">Nyugat-Japán</span><span class="sxs-lookup"><span data-stu-id="b131d-124">Japan West</span></span> |<span data-ttu-id="b131d-125">Prémium szintű storage még nem érhető el</span><span class="sxs-lookup"><span data-stu-id="b131d-125">Premium storage not yet available</span></span> |
| <span data-ttu-id="b131d-126">USA északi középső régiója</span><span class="sxs-lookup"><span data-stu-id="b131d-126">North Central US</span></span> |<span data-ttu-id="b131d-127">2016. november 10.</span><span class="sxs-lookup"><span data-stu-id="b131d-127">November 10, 2016</span></span> |

## <a name="automatic-migration-details"></a><span data-ttu-id="b131d-128">Automatikus áttelepítése részletei</span><span class="sxs-lookup"><span data-stu-id="b131d-128">Automatic migration details</span></span>
<span data-ttu-id="b131d-129">Alapértelmezés szerint kiforrottabb lesz az adatbázis az Ön 18:00:00 és 6:00-kor a régió helyi idő közötti során hello [automatikus áttelepítése ütemezés][automatic migration schedule].</span><span class="sxs-lookup"><span data-stu-id="b131d-129">By default, we will migrate your database for you between 6:00 PM and 6:00 AM in your region's local time during hello [automatic migration schedule][automatic migration schedule].</span></span> <span data-ttu-id="b131d-130">A meglévő adatraktár használhatatlan lesz, hello az áttelepítés során.</span><span class="sxs-lookup"><span data-stu-id="b131d-130">Your existing data warehouse will be unusable during hello migration.</span></span> <span data-ttu-id="b131d-131">hello áttelepítési / adatraktár lépnek / terabájt tárhelyet körülbelül egy óra.</span><span class="sxs-lookup"><span data-stu-id="b131d-131">hello migration will take approximately one hour per terabyte of storage per data warehouse.</span></span> <span data-ttu-id="b131d-132">Nem kell fizetnie bármely részének hello automatikus áttelepítése során.</span><span class="sxs-lookup"><span data-stu-id="b131d-132">You will not be charged during any portion of hello automatic migration.</span></span>

> [!NOTE]
> <span data-ttu-id="b131d-133">Hello áttelepítés akkor fejeződött be, amikor az adatraktár lesznek ismét online állapotú, és használható.</span><span class="sxs-lookup"><span data-stu-id="b131d-133">When hello migration is complete, your data warehouse will be back online and usable.</span></span>
>
>

<span data-ttu-id="b131d-134">A Microsoft hello a következő lépéseket toocomplete hello áttelepítési (ezek nem igényelnek beavatkozást bármely beavatkozás) tart.</span><span class="sxs-lookup"><span data-stu-id="b131d-134">Microsoft is taking hello following steps toocomplete hello migration (these do not require any involvement on your part).</span></span> <span data-ttu-id="b131d-135">Ebben a példában képzelhető el, hogy a meglévő adatraktár szabványos tárolón jelenleg neve "MyDW."</span><span class="sxs-lookup"><span data-stu-id="b131d-135">In this example, imagine that your existing data warehouse on standard storage is currently named “MyDW.”</span></span>

1. <span data-ttu-id="b131d-136">Microsoft túl átnevezi "MyDW" "[időbélyeg] MyDW_DO_NOT_USE_."</span><span class="sxs-lookup"><span data-stu-id="b131d-136">Microsoft renames “MyDW” too“MyDW_DO_NOT_USE_[Timestamp].”</span></span>
2. <span data-ttu-id="b131d-137">A Microsoft leállítja "[időbélyeg] MyDW_DO_NOT_USE_."</span><span class="sxs-lookup"><span data-stu-id="b131d-137">Microsoft pauses “MyDW_DO_NOT_USE_[Timestamp].”</span></span> <span data-ttu-id="b131d-138">Ebben az időszakban a biztonsági másolat használatban van.</span><span class="sxs-lookup"><span data-stu-id="b131d-138">During this time, a backup is taken.</span></span> <span data-ttu-id="b131d-139">Ha azt a folyamat során problémába ütközik jelenhet meg több szüneteltetése és folytatása.</span><span class="sxs-lookup"><span data-stu-id="b131d-139">You may see multiple pauses and resumes if we encounter any issues during this process.</span></span>
3. <span data-ttu-id="b131d-140">Microsoft hoz létre egy új data warehouse "MyDW" a prémium szintű storage hello biztonsági másolatból a 2. lépésben hozott nevű.</span><span class="sxs-lookup"><span data-stu-id="b131d-140">Microsoft creates a new data warehouse named “MyDW” on premium storage from hello backup taken in step 2.</span></span> <span data-ttu-id="b131d-141">"MyDW" csak illeszti hello visszaállítás befejezése után.</span><span class="sxs-lookup"><span data-stu-id="b131d-141">“MyDW” will not appear until after hello restore is complete.</span></span>
4. <span data-ttu-id="b131d-142">Hello visszaállítás befejezése után "MyDW" arkusz toohello ugyanazokat az adatokat az adatraktár-egységek, és állapot (felfüggesztett aktív), de a hello áttelepítés előtt.</span><span class="sxs-lookup"><span data-stu-id="b131d-142">After hello restore is complete, “MyDW” returns toohello same data warehouse units and state (paused or active) that it was before hello migration.</span></span>
5. <span data-ttu-id="b131d-143">A Microsoft hello áttelepítés befejezése után törli az "MyDW_DO_NOT_USE_ [időbélyeg]".</span><span class="sxs-lookup"><span data-stu-id="b131d-143">After hello migration is complete, Microsoft deletes “MyDW_DO_NOT_USE_[Timestamp]”.</span></span>

> [!NOTE]
> <span data-ttu-id="b131d-144">hello következő beállítások nem jelenik meg az áttelepítés hello:</span><span class="sxs-lookup"><span data-stu-id="b131d-144">hello following settings do not carry over as part of hello migration:</span></span>
>
> * <span data-ttu-id="b131d-145">Naplózását hello adatbázis szintjén toobe újra engedélyezni kell.</span><span class="sxs-lookup"><span data-stu-id="b131d-145">Auditing at hello database level needs toobe re-enabled.</span></span>
> * <span data-ttu-id="b131d-146">Tűzfalszabályok hello adatbázis szintjén kell toobe újból hozzáadták.</span><span class="sxs-lookup"><span data-stu-id="b131d-146">Firewall rules at hello database level need toobe re-added.</span></span> <span data-ttu-id="b131d-147">Hello kiszolgálószintű tűzfal-szabályokat, nem érintettek.</span><span class="sxs-lookup"><span data-stu-id="b131d-147">Firewall rules at hello server level are not affected.</span></span>
>
>

### <a name="automatic-migration-schedule"></a><span data-ttu-id="b131d-148">Automatikus áttelepítése ütemezése</span><span class="sxs-lookup"><span data-stu-id="b131d-148">Automatic migration schedule</span></span>
<span data-ttu-id="b131d-149">Automatikus áttelepítések során kimaradás ütemezés a következő hello elő 18:00:00 és 6:00-kor (helyi idő régiónként) között.</span><span class="sxs-lookup"><span data-stu-id="b131d-149">Automatic migrations occur between 6:00 PM and 6:00 AM (local time per region) during hello following outage schedule.</span></span>

| <span data-ttu-id="b131d-150">**Régió**</span><span class="sxs-lookup"><span data-stu-id="b131d-150">**Region**</span></span> | <span data-ttu-id="b131d-151">**Becsült kezdő dátuma**</span><span class="sxs-lookup"><span data-stu-id="b131d-151">**Estimated start date**</span></span> | <span data-ttu-id="b131d-152">**Becsült befejezési dátum**</span><span class="sxs-lookup"><span data-stu-id="b131d-152">**Estimated end date**</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="b131d-153">Kelet-Ausztrália</span><span class="sxs-lookup"><span data-stu-id="b131d-153">Australia East</span></span> |<span data-ttu-id="b131d-154">Még nem határozza meg</span><span class="sxs-lookup"><span data-stu-id="b131d-154">Not determined yet</span></span> |<span data-ttu-id="b131d-155">Még nem határozza meg</span><span class="sxs-lookup"><span data-stu-id="b131d-155">Not determined yet</span></span> |
| <span data-ttu-id="b131d-156">Kelet-Kína</span><span class="sxs-lookup"><span data-stu-id="b131d-156">China East</span></span> |<span data-ttu-id="b131d-157">2017. január 9.</span><span class="sxs-lookup"><span data-stu-id="b131d-157">January 9, 2017</span></span> |<span data-ttu-id="b131d-158">2017. január 13.</span><span class="sxs-lookup"><span data-stu-id="b131d-158">January 13, 2017</span></span> |
| <span data-ttu-id="b131d-159">Észak-Kína</span><span class="sxs-lookup"><span data-stu-id="b131d-159">China North</span></span> |<span data-ttu-id="b131d-160">2017. január 9.</span><span class="sxs-lookup"><span data-stu-id="b131d-160">January 9, 2017</span></span> |<span data-ttu-id="b131d-161">2017. január 13.</span><span class="sxs-lookup"><span data-stu-id="b131d-161">January 13, 2017</span></span> |
| <span data-ttu-id="b131d-162">Közép-Németország</span><span class="sxs-lookup"><span data-stu-id="b131d-162">Germany Central</span></span> |<span data-ttu-id="b131d-163">2017. január 9.</span><span class="sxs-lookup"><span data-stu-id="b131d-163">January 9, 2017</span></span> |<span data-ttu-id="b131d-164">2017. január 13.</span><span class="sxs-lookup"><span data-stu-id="b131d-164">January 13, 2017</span></span> |
| <span data-ttu-id="b131d-165">Északkelet-Németország</span><span class="sxs-lookup"><span data-stu-id="b131d-165">Germany Northeast</span></span> |<span data-ttu-id="b131d-166">2017. január 9.</span><span class="sxs-lookup"><span data-stu-id="b131d-166">January 9, 2017</span></span> |<span data-ttu-id="b131d-167">2017. január 13.</span><span class="sxs-lookup"><span data-stu-id="b131d-167">January 13, 2017</span></span> |
| <span data-ttu-id="b131d-168">Nyugat-India</span><span class="sxs-lookup"><span data-stu-id="b131d-168">India West</span></span> |<span data-ttu-id="b131d-169">Még nem határozza meg</span><span class="sxs-lookup"><span data-stu-id="b131d-169">Not determined yet</span></span> |<span data-ttu-id="b131d-170">Még nem határozza meg</span><span class="sxs-lookup"><span data-stu-id="b131d-170">Not determined yet</span></span> |
| <span data-ttu-id="b131d-171">Nyugat-Japán</span><span class="sxs-lookup"><span data-stu-id="b131d-171">Japan West</span></span> |<span data-ttu-id="b131d-172">Még nem határozza meg</span><span class="sxs-lookup"><span data-stu-id="b131d-172">Not determined yet</span></span> |<span data-ttu-id="b131d-173">Még nem határozza meg</span><span class="sxs-lookup"><span data-stu-id="b131d-173">Not determined yet</span></span> |
| <span data-ttu-id="b131d-174">USA északi középső régiója</span><span class="sxs-lookup"><span data-stu-id="b131d-174">North Central US</span></span> |<span data-ttu-id="b131d-175">2017. január 9.</span><span class="sxs-lookup"><span data-stu-id="b131d-175">January 9, 2017</span></span> |<span data-ttu-id="b131d-176">2017. január 13.</span><span class="sxs-lookup"><span data-stu-id="b131d-176">January 13, 2017</span></span> |

## <a name="self-migration-toopremium-storage"></a><span data-ttu-id="b131d-177">Önálló áttelepítés toopremium tároló</span><span class="sxs-lookup"><span data-stu-id="b131d-177">Self-migration toopremium storage</span></span>
<span data-ttu-id="b131d-178">Ha azt szeretné toocontrol, amikor megtörténik a leállás, követő lépéseket toomigrate egy meglévő adatraktár szabványos tárolási toopremium tároló hello is használhatja.</span><span class="sxs-lookup"><span data-stu-id="b131d-178">If you want toocontrol when your downtime will occur, you can use hello following steps toomigrate an existing data warehouse on standard storage toopremium storage.</span></span> <span data-ttu-id="b131d-179">Ha ezt a lehetőséget választja, el kell végeznie a hello önálló áttelepítés, az adott régióban megkezdése hello automatikus áttelepítése előtt.</span><span class="sxs-lookup"><span data-stu-id="b131d-179">If you choose this option, you must complete hello self-migration before hello automatic migration begins in that region.</span></span> <span data-ttu-id="b131d-180">Ez biztosítja, hogy azt ne hello automatikus áttelepítése ütközést okoz (tekintse meg a toohello [automatikus áttelepítése ütemezés][automatic migration schedule]).</span><span class="sxs-lookup"><span data-stu-id="b131d-180">This ensures that you avoid any risk of hello automatic migration causing a conflict (refer toohello [automatic migration schedule][automatic migration schedule]).</span></span>

### <a name="self-migration-instructions"></a><span data-ttu-id="b131d-181">Önálló áttelepítési utasítások</span><span class="sxs-lookup"><span data-stu-id="b131d-181">Self-migration instructions</span></span>
<span data-ttu-id="b131d-182">az adatok adatraktár saját magának, hello biztonságimásolat-készítő és szolgáltatások visszaállítása toomigrate.</span><span class="sxs-lookup"><span data-stu-id="b131d-182">toomigrate your data warehouse yourself, use hello backup and restore features.</span></span> <span data-ttu-id="b131d-183">hello visszaállítási hello áttelepítési része várt tootake / / adatraktár tárolási terabájt körülbelül egy óra.</span><span class="sxs-lookup"><span data-stu-id="b131d-183">hello restore portion of hello migration is expected tootake around one hour per terabyte of storage per data warehouse.</span></span> <span data-ttu-id="b131d-184">Ha azt szeretné, hogy tookeep hello azonos nevet áttelepítés befejezése után, kövesse a hello [lépéseket toorename áttelepítéskor][steps toorename during migration].</span><span class="sxs-lookup"><span data-stu-id="b131d-184">If you want tookeep hello same name after migration is complete, follow hello [steps toorename during migration][steps toorename during migration].</span></span>

1. <span data-ttu-id="b131d-185">[Felfüggesztés] [ Pause] az adatraktár.</span><span class="sxs-lookup"><span data-stu-id="b131d-185">[Pause][Pause] your data warehouse.</span></span> <span data-ttu-id="b131d-186">Ehhez szükséges, az automatikus biztonsági mentés.</span><span class="sxs-lookup"><span data-stu-id="b131d-186">This takes an automatic backup.</span></span>
2. <span data-ttu-id="b131d-187">[Visszaállítás] [ Restore] a legutóbbi pillanatképből.</span><span class="sxs-lookup"><span data-stu-id="b131d-187">[Restore][Restore] from your most recent snapshot.</span></span>
3. <span data-ttu-id="b131d-188">Törölje a meglévő adatraktár szabványos tárolón.</span><span class="sxs-lookup"><span data-stu-id="b131d-188">Delete your existing data warehouse on standard storage.</span></span> <span data-ttu-id="b131d-189">**Ha ezt a lépést nem toodo, akkor mindkét adatraktárak fogjuk számlázni.**</span><span class="sxs-lookup"><span data-stu-id="b131d-189">**If you fail toodo this step, you will be charged for both data warehouses.**</span></span>

> [!NOTE]
> <span data-ttu-id="b131d-190">hello következő beállítások nem jelenik meg az áttelepítés hello:</span><span class="sxs-lookup"><span data-stu-id="b131d-190">hello following settings do not carry over as part of hello migration:</span></span>
>
> * <span data-ttu-id="b131d-191">Naplózását hello adatbázis szintjén toobe újra engedélyezni kell.</span><span class="sxs-lookup"><span data-stu-id="b131d-191">Auditing at hello database level needs toobe re-enabled.</span></span>
> * <span data-ttu-id="b131d-192">Tűzfalszabályok hello adatbázis szintjén kell toobe újból hozzáadták.</span><span class="sxs-lookup"><span data-stu-id="b131d-192">Firewall rules at hello database level need toobe re-added.</span></span> <span data-ttu-id="b131d-193">Hello kiszolgálószintű tűzfal-szabályokat, nem érintettek.</span><span class="sxs-lookup"><span data-stu-id="b131d-193">Firewall rules at hello server level are not affected.</span></span>
>
>

#### <a name="rename-data-warehouse-during-migration-optional"></a><span data-ttu-id="b131d-194">Adatraktár átnevezése (választható) az áttelepítés során</span><span class="sxs-lookup"><span data-stu-id="b131d-194">Rename data warehouse during migration (optional)</span></span>
<span data-ttu-id="b131d-195">Hello azonos logikai kiszolgáló nem rendelkezik a két adatbázis hello ugyanazzal a névvel.</span><span class="sxs-lookup"><span data-stu-id="b131d-195">Two databases on hello same logical server cannot have hello same name.</span></span> <span data-ttu-id="b131d-196">Az SQL Data Warehouse mostantól támogatja a hello képességét toorename adatraktár.</span><span class="sxs-lookup"><span data-stu-id="b131d-196">SQL Data Warehouse now supports hello ability toorename a data warehouse.</span></span>

<span data-ttu-id="b131d-197">Ebben a példában képzelhető el, hogy a meglévő adatraktár szabványos tárolón jelenleg neve "MyDW."</span><span class="sxs-lookup"><span data-stu-id="b131d-197">In this example, imagine that your existing data warehouse on standard storage is currently named “MyDW.”</span></span>

1. <span data-ttu-id="b131d-198">Nevezze át a "MyDW" hello következő ALTER DATABASE parancs használatával.</span><span class="sxs-lookup"><span data-stu-id="b131d-198">Rename "MyDW" by using hello following ALTER DATABASE command.</span></span> <span data-ttu-id="b131d-199">(A példában azt kell nevezni "MyDW_BeforeMigration.")  Ez a parancs minden meglévő tranzakciókat, és hello főadatbázis toosucceed kell elvégezni.</span><span class="sxs-lookup"><span data-stu-id="b131d-199">(In this example, we'll rename it "MyDW_BeforeMigration.")  This command stops all existing transactions, and must be done in hello master database toosucceed.</span></span>
   ```
   ALTER DATABASE CurrentDatabasename MODIFY NAME = NewDatabaseName;
   ```
2. <span data-ttu-id="b131d-200">[Felfüggesztés] [ Pause] "MyDW_BeforeMigration."</span><span class="sxs-lookup"><span data-stu-id="b131d-200">[Pause][Pause] "MyDW_BeforeMigration."</span></span> <span data-ttu-id="b131d-201">Ehhez szükséges, az automatikus biztonsági mentés.</span><span class="sxs-lookup"><span data-stu-id="b131d-201">This takes an automatic backup.</span></span>
3. <span data-ttu-id="b131d-202">[Visszaállítás] [ Restore] hello nevű új adatbázis a legutóbbi pillanatképből toobe (például "MyDW") használt.</span><span class="sxs-lookup"><span data-stu-id="b131d-202">[Restore][Restore] from your most recent snapshot a new database with hello name it used toobe (for example, "MyDW").</span></span>
4. <span data-ttu-id="b131d-203">Törölje a "MyDW_BeforeMigration."</span><span class="sxs-lookup"><span data-stu-id="b131d-203">Delete "MyDW_BeforeMigration."</span></span> <span data-ttu-id="b131d-204">**Ha ezt a lépést nem toodo, akkor mindkét adatraktárak fogjuk számlázni.**</span><span class="sxs-lookup"><span data-stu-id="b131d-204">**If you fail toodo this step, you will be charged for both data warehouses.**</span></span>


## <a name="next-steps"></a><span data-ttu-id="b131d-205">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b131d-205">Next steps</span></span>
<span data-ttu-id="b131d-206">A hello toopremium tárolási módosításához hello mögöttes architektúráját, valamint az adatraktár adatbázis-blob fájlok számának is rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="b131d-206">With hello change toopremium storage, you also have an increased number of database blob files in hello underlying architecture of your data warehouse.</span></span> <span data-ttu-id="b131d-207">Ez a változás toomaximize hello által a fürtözött oszlopcentrikus indexek újraépítése hello a következő parancsfájl használatával.</span><span class="sxs-lookup"><span data-stu-id="b131d-207">toomaximize hello performance benefits of this change, rebuild your clustered columnstore indexes by using hello following script.</span></span> <span data-ttu-id="b131d-208">hello parancsfájl néhányat a meglévő toohello további a BLOB-adatobjektumok kényszeríthet működik.</span><span class="sxs-lookup"><span data-stu-id="b131d-208">hello script works by forcing some of your existing data toohello additional blobs.</span></span> <span data-ttu-id="b131d-209">Ha nem tesz semmit, hello adatokat fog természetes terjesztenie adott idő alatt, több adatot tölt be a táblába.</span><span class="sxs-lookup"><span data-stu-id="b131d-209">If you take no action, hello data will naturally redistribute over time as you load more data into your tables.</span></span>

<span data-ttu-id="b131d-210">**Előfeltételek:**</span><span class="sxs-lookup"><span data-stu-id="b131d-210">**Prerequisites:**</span></span>

- <span data-ttu-id="b131d-211">hello adatraktár futtasson-e 1000 adattárházegységek vagy annál újabb (lásd: [méretezési számítási teljesítményt][scale compute power]).</span><span class="sxs-lookup"><span data-stu-id="b131d-211">hello data warehouse should run with 1,000 data warehouse units or higher (see [scale compute power][scale compute power]).</span></span>
- <span data-ttu-id="b131d-212">hello hello parancsfájlt végrehajtó felhasználó kell hello [mediumrc szerepkör] [ mediumrc role] vagy újabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="b131d-212">hello user executing hello script should be in hello [mediumrc role][mediumrc role] or higher.</span></span> <span data-ttu-id="b131d-213">egy felhasználó toothis szerepkör tooadd hello következő hajtható végre:````EXEC sp_addrolemember 'xlargerc', 'MyUser'````</span><span class="sxs-lookup"><span data-stu-id="b131d-213">tooadd a user toothis role, execute hello following: ````EXEC sp_addrolemember 'xlargerc', 'MyUser'````</span></span>

````sql
-------------------------------------------------------------------------------
-- Step 1: Create table toocontrol index rebuild
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
-- Step 2: Execute index rebuilds. If script fails, hello below can be re-run toorestart where last left off.
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

<span data-ttu-id="b131d-214">Ha problémába ütközik az adatraktárban [hozzon létre egy támogatási jegy] [ create a support ticket] és "áttelepítési toopremium tároló" hello lehetséges ok, hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="b131d-214">If you encounter any issues with your data warehouse, [create a support ticket][create a support ticket] and reference “migration toopremium storage” as hello possible cause.</span></span>

<!--Image references-->

<!--Article references-->
[automatic migration schedule]: #automatic-migration-schedule
[self-migration tooPremium Storage]: #self-migration-to-premium-storage
[create a support ticket]: sql-data-warehouse-get-started-create-support-ticket.md
[Azure paired region]: best-practices-availability-paired-regions.md
[main documentation site]: services/sql-data-warehouse.md
[Pause]: sql-data-warehouse-manage-compute-portal.md#pause-compute
[Restore]: sql-data-warehouse-restore-database-portal.md
[steps toorename during migration]: #optional-steps-to-rename-during-migration
[scale compute power]: sql-data-warehouse-manage-compute-portal.md#scale-compute-power
[mediumrc role]: sql-data-warehouse-develop-concurrency.md

<!--MSDN references-->


<!--Other Web references-->
[Premium Storage for greater performance predictability]: https://azure.microsoft.com/en-us/blog/azure-sql-data-warehouse-introduces-premium-storage-for-greater-performance/
[Azure Portal]: https://portal.azure.com
