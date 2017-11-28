---
title: "az Azure adatraktár - a helyi és georedundáns aaaRestore |} Microsoft Docs"
description: "Áttekintés a hello adatbázis visszaállítási lehetőségek az Azure SQL Data Warehouse adatbázis helyreállítása."
services: sql-data-warehouse
documentationcenter: NA
author: Lakshmi1812
manager: jhubbard
editor: 
ms.assetid: 3e01c65c-6708-4fd7-82f5-4e1b5f61d304
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: backup-restore
ms.date: 10/31/2016
ms.author: lakshmir;barbkess
ms.openlocfilehash: a96b898372b29d420e1416ca93a172ff8af47fc7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sql-data-warehouse-restore"></a><span data-ttu-id="56938-103">Az SQL Data Warehouse visszaállítása</span><span class="sxs-lookup"><span data-stu-id="56938-103">SQL Data Warehouse restore</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="56938-104">[– Áttekintés][Overview]</span><span class="sxs-lookup"><span data-stu-id="56938-104">[Overview][Overview]</span></span>
> * <span data-ttu-id="56938-105">[Portál][Portal]</span><span class="sxs-lookup"><span data-stu-id="56938-105">[Portal][Portal]</span></span>
> * <span data-ttu-id="56938-106">[PowerShell][PowerShell]</span><span class="sxs-lookup"><span data-stu-id="56938-106">[PowerShell][PowerShell]</span></span>
> * <span data-ttu-id="56938-107">[REST][REST]</span><span class="sxs-lookup"><span data-stu-id="56938-107">[REST][REST]</span></span>
> 
> 

<span data-ttu-id="56938-108">Az SQL Data Warehouse a helyi és földrajzi visszaállítások részeként a data warehouse vész helyreállítási lehetőségeket nyújt.</span><span class="sxs-lookup"><span data-stu-id="56938-108">SQL Data Warehouse offers both local and geographical restores as part of its data warehouse disaster recovery capabilities.</span></span> <span data-ttu-id="56938-109">Használja a data warehouse biztonsági mentések toorestore a data warehouse tooa visszaállítási pont hello elsődleges régióban, vagy georedundáns biztonsági mentések toorestore tooa különböző földrajzi régiót kell használnia.</span><span class="sxs-lookup"><span data-stu-id="56938-109">Use data warehouse backups toorestore your data warehouse tooa restore point in hello primary region, or use geo-redundant backups toorestore tooa different geographical region.</span></span> <span data-ttu-id="56938-110">Ez a cikk ismerteti a hello mintaadatokról egy adatraktár a visszaállításakor.</span><span class="sxs-lookup"><span data-stu-id="56938-110">This article explains hello specifics of restoring a data warehouse.</span></span>

## <a name="what-is-a-data-warehouse-restore"></a><span data-ttu-id="56938-111">Mi az a data warehouse visszaállítás?</span><span class="sxs-lookup"><span data-stu-id="56938-111">What is a data warehouse restore?</span></span>
<span data-ttu-id="56938-112">A data warehouse visszaállítás egy új adatraktárat egy meglévő biztonsági másolatából létrehozott vagy törölt adatraktár.</span><span class="sxs-lookup"><span data-stu-id="56938-112">A data warehouse restore is a new data warehouse that is created from a backup of an existing or deleted data warehouse.</span></span> <span data-ttu-id="56938-113">hello visszaállított adatraktár újra létrehozza a biztonsági másolat adatraktár hello adott időpontban.</span><span class="sxs-lookup"><span data-stu-id="56938-113">hello restored data warehouse re-creates hello backed-up data warehouse at a specific time.</span></span> <span data-ttu-id="56938-114">Mivel az SQL Data Warehouse egy elosztott rendszert, a data warehouse visszaállítás számos biztonsági mentés fájljaiból, az Azure-blobokat tárolt jön létre.</span><span class="sxs-lookup"><span data-stu-id="56938-114">Since SQL Data Warehouse is a distributed system, a data warehouse restore is created from many backup files that are stored in Azure blobs.</span></span> 

<span data-ttu-id="56938-115">Adatbázis visszaállítása nem minden üzleti folytonossági és vészhelyreállítási helyreállítási stratégia nagyon fontos részét képezik, mert az adatok véletlen sérülés vagy törlése után újból létrehozza.</span><span class="sxs-lookup"><span data-stu-id="56938-115">Database restore is an essential part of any business continuity and disaster recovery strategy because it re-creates your data after accidental corruption or deletion.</span></span>

<span data-ttu-id="56938-116">További információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="56938-116">For more information, see:</span></span>

* [<span data-ttu-id="56938-117">Az SQL Data Warehouse biztonsági mentések</span><span class="sxs-lookup"><span data-stu-id="56938-117">SQL Data Warehouse backups</span></span>](sql-data-warehouse-backups.md)
* [<span data-ttu-id="56938-118">Üzleti folytonosság – áttekintés</span><span class="sxs-lookup"><span data-stu-id="56938-118">Business continuity overview</span></span>](../sql-database/sql-database-business-continuity.md)

## <a name="data-warehouse-restore-points"></a><span data-ttu-id="56938-119">Data warehouse visszaállítási pontok</span><span class="sxs-lookup"><span data-stu-id="56938-119">Data warehouse restore points</span></span>
<span data-ttu-id="56938-120">Prémium szintű Azure Storage használatának előnye, mint az SQL Data Warehouse Azure Storage-Blobba pillanatképek toobackup hello elsődleges adatraktár használja.</span><span class="sxs-lookup"><span data-stu-id="56938-120">As a benefit of using Azure Premium Storage, SQL Data Warehouse uses Azure Storage Blob snapshots toobackup hello primary data warehouse.</span></span> <span data-ttu-id="56938-121">Minden egyes pillanatkép van egy olyan visszaállítási hello időt jelölő hello pillanatkép elindult.</span><span class="sxs-lookup"><span data-stu-id="56938-121">Each snapshot has a restore point that represents hello time hello snapshot started.</span></span> <span data-ttu-id="56938-122">adatraktár toorestore, visszaállítási pont választja, és adja ki a restore parancsot.</span><span class="sxs-lookup"><span data-stu-id="56938-122">toorestore a data warehouse, you choose a restore point and issue a restore command.</span></span>  

<span data-ttu-id="56938-123">Az SQL Data Warehouse mindig hello biztonsági mentési tooa új adatraktárat állítja vissza.</span><span class="sxs-lookup"><span data-stu-id="56938-123">SQL Data Warehouse always restores hello backup tooa new data warehouse.</span></span> <span data-ttu-id="56938-124">Akkor tartsa hello visszaállított adatraktár, és az aktuális hello, vagy törölje az egyik legyen.</span><span class="sxs-lookup"><span data-stu-id="56938-124">You can either keep hello restored data warehouse and hello current one, or delete one of them.</span></span> <span data-ttu-id="56938-125">Ha szeretné tooreplace hello aktuális data warehouse hello vissza data warehouse-ba, hogy át lehessen nevezni.</span><span class="sxs-lookup"><span data-stu-id="56938-125">If you want tooreplace hello current data warehouse with hello restored data warehouse, you can rename it.</span></span>

<span data-ttu-id="56938-126">Ha egy törölt vagy szüneteltetett adatraktár toorestore van szüksége, akkor [támogatási jegy létrehozása](sql-data-warehouse-get-started-create-support-ticket.md).</span><span class="sxs-lookup"><span data-stu-id="56938-126">If you need toorestore a deleted or paused data warehouse, you can [create a support ticket](sql-data-warehouse-get-started-create-support-ticket.md).</span></span> 

<!-- 
### Can I restore a deleted data warehouse?

Yes, you can restore hello last available restore point.

Yes, for hello next seven calendar days. When you delete a data warehouse, SQL Data Warehouse actually keeps hello data warehouse and its snapshots for seven days just in case you need hello data. After seven days, you won't be able toorestore tooany of hello restore points. -->

## <a name="geo-redundant-restore"></a><span data-ttu-id="56938-127">Georedundáns helyreállítás</span><span class="sxs-lookup"><span data-stu-id="56938-127">Geo-redundant restore</span></span>
<span data-ttu-id="56938-128">Az adatraktár tooany adatterület támogató Azure SQL Data Warehouse a kiválasztott teljesítmény szinten állíthatja vissza.</span><span class="sxs-lookup"><span data-stu-id="56938-128">You can restore your data warehouse tooany region supporting Azure SQL Data Warehouse at your chosen performance level.</span></span> <span data-ttu-id="56938-129">Vegye figyelembe, hogy 9000 és 18000 DWU nem támogatottak minden régióban hello előzetes.</span><span class="sxs-lookup"><span data-stu-id="56938-129">Please note that 9000 and 18000 DWU are not supported in all regions during hello preview.</span></span>

> [!NOTE]
> <span data-ttu-id="56938-130">a georedundáns tooperform visszaállítási kell nem visszavonta igényét ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="56938-130">tooperform a geo-redundant restore you must not have opted out of this feature.</span></span>
> 
> 

## <a name="restore-timeline"></a><span data-ttu-id="56938-131">Az idősor visszaállítása</span><span class="sxs-lookup"><span data-stu-id="56938-131">Restore timeline</span></span>
<span data-ttu-id="56938-132">Visszaállíthatja egy adatbázis tooany elérhető visszaállítási pont belül hello elmúlt hét napban.</span><span class="sxs-lookup"><span data-stu-id="56938-132">You can restore a database tooany available restore point within hello last seven days.</span></span> <span data-ttu-id="56938-133">A pillanatképek tooeight négy óránként start és hét napja.</span><span class="sxs-lookup"><span data-stu-id="56938-133">Snapshots start every four tooeight hours and are available for seven days.</span></span> <span data-ttu-id="56938-134">Ha pillanatkép régebbi, mint hét nap, jár le, és a helyreállítási pont már nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="56938-134">When a snapshot is older than seven days, it expires and its restore point is no longer available.</span></span>

## <a name="restore-costs"></a><span data-ttu-id="56938-135">Állítsa vissza a költségek</span><span class="sxs-lookup"><span data-stu-id="56938-135">Restore costs</span></span>
<span data-ttu-id="56938-136">hello tárolási kell fizetni hello visszaállított adatraktár hello prémium szintű Azure Storage díj számlázása történik.</span><span class="sxs-lookup"><span data-stu-id="56938-136">hello storage charge for hello restored data warehouse is billed at hello Azure Premium Storage rate.</span></span> 

<span data-ttu-id="56938-137">Ha egy visszaállított adatraktár felfüggesztéséhez van szó, a tárolási hello prémium szintű Azure Storage díj.</span><span class="sxs-lookup"><span data-stu-id="56938-137">If you pause a restored data warehouse, you are charged for storage at hello Azure Premium Storage rate.</span></span> <span data-ttu-id="56938-138">hello felfüggesztéséhez előnye, nem kell fizetnie hello DWU számítási erőforrások.</span><span class="sxs-lookup"><span data-stu-id="56938-138">hello advantage of pausing is you are not charged for hello DWU computing resources.</span></span>

<span data-ttu-id="56938-139">További információ az SQL Data Warehouse díjszabása: [SQL Data Warehouse díjszabása](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).</span><span class="sxs-lookup"><span data-stu-id="56938-139">For more information about SQL Data Warehouse pricing, see [SQL Data Warehouse Pricing](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).</span></span>

## <a name="uses-for-restore"></a><span data-ttu-id="56938-140">A visszaállítási használ</span><span class="sxs-lookup"><span data-stu-id="56938-140">Uses for restore</span></span>
<span data-ttu-id="56938-141">hello elsődleges funkciója a data warehouse visszaállítási toorecover adatok véletlen adatvesztés vagy adatsérülés utánra esik.</span><span class="sxs-lookup"><span data-stu-id="56938-141">hello primary use for data warehouse restore is toorecover data after accidental data loss or corruption.</span></span>

<span data-ttu-id="56938-142">Használhatja a data warehouse visszaállítási tooretain biztonsági több mint hét nap.</span><span class="sxs-lookup"><span data-stu-id="56938-142">You can also use data warehouse restore tooretain a backup for longer than seven days.</span></span> <span data-ttu-id="56938-143">Miután visszaállította hello biztonsági mentés, hello adatraktár online rendelkezik, és akár szüneteltetheti is, hogy határozatlan ideig toosave számítási költségek.</span><span class="sxs-lookup"><span data-stu-id="56938-143">Once hello backup is restored, you have hello data warehouse online and can pause it indefinitely toosave compute costs.</span></span> <span data-ttu-id="56938-144">hello szüneteltetett adatbázis terhel tárolási hello prémium szintű Azure Storage díj.</span><span class="sxs-lookup"><span data-stu-id="56938-144">hello paused database incurs storage charges at hello Azure Premium Storage rate.</span></span> 

## <a name="related-topics"></a><span data-ttu-id="56938-145">Kapcsolódó témakörök</span><span class="sxs-lookup"><span data-stu-id="56938-145">Related topics</span></span>
### <a name="scenarios"></a><span data-ttu-id="56938-146">Forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="56938-146">Scenarios</span></span>
* <span data-ttu-id="56938-147">Üzleti folytonosság áttekintéséért lásd: [üzleti folytonosság – áttekintés](../sql-database/sql-database-business-continuity.md)</span><span class="sxs-lookup"><span data-stu-id="56938-147">For a business continuity overview, see [Business continuity overview](../sql-database/sql-database-business-continuity.md)</span></span>

<!-- ### Tasks -->

<span data-ttu-id="56938-148">adatraktár tooperform helyreállítása, visszaállítás segítségével:</span><span class="sxs-lookup"><span data-stu-id="56938-148">tooperform a data warehouse restore, restore using:</span></span>

* <span data-ttu-id="56938-149">Az Azure portál, lásd: [hello Azure-portál használatával adatraktár visszaállítása](sql-data-warehouse-restore-database-portal.md)</span><span class="sxs-lookup"><span data-stu-id="56938-149">Azure portal, see [Restore a data warehouse using hello Azure portal](sql-data-warehouse-restore-database-portal.md)</span></span>
* <span data-ttu-id="56938-150">PowerShell-parancsmagok [visszaállítani az PowerShell-parancsmagok használatával adatraktár](sql-data-warehouse-restore-database-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="56938-150">PowerShell cmdlets, see [Restore a data warehouse using PowerShell cmdlets](sql-data-warehouse-restore-database-powershell.md)</span></span>
* <span data-ttu-id="56938-151">REST-API-k, lásd: [hello REST API-k használatával adatraktár visszaállítása](sql-data-warehouse-restore-database-rest-api.md)</span><span class="sxs-lookup"><span data-stu-id="56938-151">REST APIs, see [Restore a data warehouse using hello REST APIs](sql-data-warehouse-restore-database-rest-api.md)</span></span>

<!-- ### Tutorials -->

<!--Image references-->

<!--Article references-->
[Azure SQL Database business continuity overview]: ../sql-database/sql-database-business-continuity.md
[Overview]: ./sql-data-warehouse-restore-database-overview.md
[Portal]: ./sql-data-warehouse-restore-database-portal.md
[PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[REST]: ./sql-data-warehouse-restore-database-rest-api.md

<!--MSDN references-->


<!--Other Web references-->
