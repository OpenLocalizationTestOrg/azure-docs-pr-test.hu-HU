---
title: "Állítsa vissza az Azure adatraktár - a helyi és georedundáns |} Microsoft Docs"
description: "Az Azure SQL Data Warehouse adatbázis helyreállítása adatbázis visszaállítási lehetőségek áttekintése."
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
ms.openlocfilehash: ea42b7135d0695b66d569095e70bb3d9f8b9594b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="sql-data-warehouse-restore"></a><span data-ttu-id="b08ae-103">Az SQL Data Warehouse visszaállítása</span><span class="sxs-lookup"><span data-stu-id="b08ae-103">SQL Data Warehouse restore</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="b08ae-104">[– Áttekintés][Overview]</span><span class="sxs-lookup"><span data-stu-id="b08ae-104">[Overview][Overview]</span></span>
> * <span data-ttu-id="b08ae-105">[Portál][Portal]</span><span class="sxs-lookup"><span data-stu-id="b08ae-105">[Portal][Portal]</span></span>
> * <span data-ttu-id="b08ae-106">[PowerShell][PowerShell]</span><span class="sxs-lookup"><span data-stu-id="b08ae-106">[PowerShell][PowerShell]</span></span>
> * <span data-ttu-id="b08ae-107">[REST][REST]</span><span class="sxs-lookup"><span data-stu-id="b08ae-107">[REST][REST]</span></span>
> 
> 

<span data-ttu-id="b08ae-108">Az SQL Data Warehouse a helyi és földrajzi visszaállítások részeként a data warehouse vész helyreállítási lehetőségeket nyújt.</span><span class="sxs-lookup"><span data-stu-id="b08ae-108">SQL Data Warehouse offers both local and geographical restores as part of its data warehouse disaster recovery capabilities.</span></span> <span data-ttu-id="b08ae-109">Az adatok adatraktár biztonsági használatával állítsa vissza az adatraktár egy visszaállítási pontot az elsődleges régióban, vagy georedundáns biztonsági mentések használatával állítsa vissza egy másik földrajzi régióban.</span><span class="sxs-lookup"><span data-stu-id="b08ae-109">Use data warehouse backups to restore your data warehouse to a restore point in the primary region, or use geo-redundant backups to restore to a different geographical region.</span></span> <span data-ttu-id="b08ae-110">Ez a cikk ismerteti a mintaadatokról egy adatraktár a visszaállításakor.</span><span class="sxs-lookup"><span data-stu-id="b08ae-110">This article explains the specifics of restoring a data warehouse.</span></span>

## <a name="what-is-a-data-warehouse-restore"></a><span data-ttu-id="b08ae-111">Mi az a data warehouse visszaállítás?</span><span class="sxs-lookup"><span data-stu-id="b08ae-111">What is a data warehouse restore?</span></span>
<span data-ttu-id="b08ae-112">A data warehouse visszaállítás egy új adatraktárat egy meglévő biztonsági másolatából létrehozott vagy törölt adatraktár.</span><span class="sxs-lookup"><span data-stu-id="b08ae-112">A data warehouse restore is a new data warehouse that is created from a backup of an existing or deleted data warehouse.</span></span> <span data-ttu-id="b08ae-113">A visszaállított adatok adatraktár újra létrehozza a biztonsági másolat adatraktár adott időpontban.</span><span class="sxs-lookup"><span data-stu-id="b08ae-113">The restored data warehouse re-creates the backed-up data warehouse at a specific time.</span></span> <span data-ttu-id="b08ae-114">Mivel az SQL Data Warehouse egy elosztott rendszert, a data warehouse visszaállítás számos biztonsági mentés fájljaiból, az Azure-blobokat tárolt jön létre.</span><span class="sxs-lookup"><span data-stu-id="b08ae-114">Since SQL Data Warehouse is a distributed system, a data warehouse restore is created from many backup files that are stored in Azure blobs.</span></span> 

<span data-ttu-id="b08ae-115">Adatbázis visszaállítása nem minden üzleti folytonossági és vészhelyreállítási helyreállítási stratégia nagyon fontos részét képezik, mert az adatok véletlen sérülés vagy törlése után újból létrehozza.</span><span class="sxs-lookup"><span data-stu-id="b08ae-115">Database restore is an essential part of any business continuity and disaster recovery strategy because it re-creates your data after accidental corruption or deletion.</span></span>

<span data-ttu-id="b08ae-116">További információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="b08ae-116">For more information, see:</span></span>

* [<span data-ttu-id="b08ae-117">Az SQL Data Warehouse biztonsági mentések</span><span class="sxs-lookup"><span data-stu-id="b08ae-117">SQL Data Warehouse backups</span></span>](sql-data-warehouse-backups.md)
* [<span data-ttu-id="b08ae-118">Üzleti folytonosság – áttekintés</span><span class="sxs-lookup"><span data-stu-id="b08ae-118">Business continuity overview</span></span>](../sql-database/sql-database-business-continuity.md)

## <a name="data-warehouse-restore-points"></a><span data-ttu-id="b08ae-119">Data warehouse visszaállítási pontok</span><span class="sxs-lookup"><span data-stu-id="b08ae-119">Data warehouse restore points</span></span>
<span data-ttu-id="b08ae-120">Prémium szintű Azure Storage használatának előnye, mint az SQL Data Warehouse használatával Azure Storage-Blobba pillanatképek biztonsági mentését az elsődleges adatraktár.</span><span class="sxs-lookup"><span data-stu-id="b08ae-120">As a benefit of using Azure Premium Storage, SQL Data Warehouse uses Azure Storage Blob snapshots to backup the primary data warehouse.</span></span> <span data-ttu-id="b08ae-121">Minden pillanatképet egy visszaállítási pontot a pillanatkép elindításakor jelölő rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="b08ae-121">Each snapshot has a restore point that represents the time the snapshot started.</span></span> <span data-ttu-id="b08ae-122">Adatraktár visszaállításához válasszon visszaállítási pontot, és adja ki a restore parancsot.</span><span class="sxs-lookup"><span data-stu-id="b08ae-122">To restore a data warehouse, you choose a restore point and issue a restore command.</span></span>  

<span data-ttu-id="b08ae-123">Az SQL Data Warehouse mindig visszaállítja a biztonsági mentés új adatraktárat.</span><span class="sxs-lookup"><span data-stu-id="b08ae-123">SQL Data Warehouse always restores the backup to a new data warehouse.</span></span> <span data-ttu-id="b08ae-124">A visszaállított adatok adatraktár és az aktuális hagyja, vagy törölje az egyik legyen.</span><span class="sxs-lookup"><span data-stu-id="b08ae-124">You can either keep the restored data warehouse and the current one, or delete one of them.</span></span> <span data-ttu-id="b08ae-125">Ha szeretné a jelenlegi adatraktárban cserélje le a visszaállított adatok adatraktár, átnevezheti azt.</span><span class="sxs-lookup"><span data-stu-id="b08ae-125">If you want to replace the current data warehouse with the restored data warehouse, you can rename it.</span></span>

<span data-ttu-id="b08ae-126">Ha a visszaállítandó törölt vagy szüneteltetett adatraktár, akkor [támogatási jegy létrehozása](sql-data-warehouse-get-started-create-support-ticket.md).</span><span class="sxs-lookup"><span data-stu-id="b08ae-126">If you need to restore a deleted or paused data warehouse, you can [create a support ticket](sql-data-warehouse-get-started-create-support-ticket.md).</span></span> 

<!-- 
### Can I restore a deleted data warehouse?

Yes, you can restore the last available restore point.

Yes, for the next seven calendar days. When you delete a data warehouse, SQL Data Warehouse actually keeps the data warehouse and its snapshots for seven days just in case you need the data. After seven days, you won't be able to restore to any of the restore points. -->

## <a name="geo-redundant-restore"></a><span data-ttu-id="b08ae-127">Georedundáns helyreállítás</span><span class="sxs-lookup"><span data-stu-id="b08ae-127">Geo-redundant restore</span></span>
<span data-ttu-id="b08ae-128">Visszaállíthatja az adatraktár minden régióban támogató Azure SQL Data Warehouse, a kiválasztott teljesítményszint szükséges.</span><span class="sxs-lookup"><span data-stu-id="b08ae-128">You can restore your data warehouse to any region supporting Azure SQL Data Warehouse at your chosen performance level.</span></span> <span data-ttu-id="b08ae-129">Vegye figyelembe, hogy 9000 és 18000 DWU nem támogatottak minden régióban az előzetes.</span><span class="sxs-lookup"><span data-stu-id="b08ae-129">Please note that 9000 and 18000 DWU are not supported in all regions during the preview.</span></span>

> [!NOTE]
> <span data-ttu-id="b08ae-130">Georedundáns visszaállításhoz kell nem visszavonta igényét ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="b08ae-130">To perform a geo-redundant restore you must not have opted out of this feature.</span></span>
> 
> 

## <a name="restore-timeline"></a><span data-ttu-id="b08ae-131">Az idősor visszaállítása</span><span class="sxs-lookup"><span data-stu-id="b08ae-131">Restore timeline</span></span>
<span data-ttu-id="b08ae-132">Visszaállíthatja egy adatbázis bármely elérhető visszaállítási pont az utóbbi hét napban.</span><span class="sxs-lookup"><span data-stu-id="b08ae-132">You can restore a database to any available restore point within the last seven days.</span></span> <span data-ttu-id="b08ae-133">A pillanatképek négy és nyolc óránként start és hét napja.</span><span class="sxs-lookup"><span data-stu-id="b08ae-133">Snapshots start every four to eight hours and are available for seven days.</span></span> <span data-ttu-id="b08ae-134">Ha pillanatkép régebbi, mint hét nap, jár le, és a helyreállítási pont már nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="b08ae-134">When a snapshot is older than seven days, it expires and its restore point is no longer available.</span></span>

## <a name="restore-costs"></a><span data-ttu-id="b08ae-135">Állítsa vissza a költségek</span><span class="sxs-lookup"><span data-stu-id="b08ae-135">Restore costs</span></span>
<span data-ttu-id="b08ae-136">A tárolási költség a visszaállított adatraktár számlázása a prémium szintű Azure Storage ütemben történik.</span><span class="sxs-lookup"><span data-stu-id="b08ae-136">The storage charge for the restored data warehouse is billed at the Azure Premium Storage rate.</span></span> 

<span data-ttu-id="b08ae-137">A visszaállított adatok adatraktár szünetelteti, ha van szó, a prémium szintű Azure Storage díj tárolására.</span><span class="sxs-lookup"><span data-stu-id="b08ae-137">If you pause a restored data warehouse, you are charged for storage at the Azure Premium Storage rate.</span></span> <span data-ttu-id="b08ae-138">Felfüggesztés előnye nem kell fizetnie a DWU számítási erőforrások.</span><span class="sxs-lookup"><span data-stu-id="b08ae-138">The advantage of pausing is you are not charged for the DWU computing resources.</span></span>

<span data-ttu-id="b08ae-139">További információ az SQL Data Warehouse díjszabása: [SQL Data Warehouse díjszabása](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).</span><span class="sxs-lookup"><span data-stu-id="b08ae-139">For more information about SQL Data Warehouse pricing, see [SQL Data Warehouse Pricing](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).</span></span>

## <a name="uses-for-restore"></a><span data-ttu-id="b08ae-140">A visszaállítási használ</span><span class="sxs-lookup"><span data-stu-id="b08ae-140">Uses for restore</span></span>
<span data-ttu-id="b08ae-141">Az elsődleges a data warehouse visszaállítási használata adatok véletlen adatvesztést vagy -sérülés utáni helyreállításhoz.</span><span class="sxs-lookup"><span data-stu-id="b08ae-141">The primary use for data warehouse restore is to recover data after accidental data loss or corruption.</span></span>

<span data-ttu-id="b08ae-142">Data warehouse visszaállítási használatával több mint hét napja a biztonsági másolatok megőrzése.</span><span class="sxs-lookup"><span data-stu-id="b08ae-142">You can also use data warehouse restore to retain a backup for longer than seven days.</span></span> <span data-ttu-id="b08ae-143">A biztonsági mentés helyreáll, amennyiben az adatraktár online rendelkezik, és akár szüneteltetheti is, hogy határozatlan ideig számítási költségek csökkentése érdekében.</span><span class="sxs-lookup"><span data-stu-id="b08ae-143">Once the backup is restored, you have the data warehouse online and can pause it indefinitely to save compute costs.</span></span> <span data-ttu-id="b08ae-144">A felfüggesztett adatbázis terhel tárolási a prémium szintű Azure Storage díj.</span><span class="sxs-lookup"><span data-stu-id="b08ae-144">The paused database incurs storage charges at the Azure Premium Storage rate.</span></span> 

## <a name="related-topics"></a><span data-ttu-id="b08ae-145">Kapcsolódó témakörök</span><span class="sxs-lookup"><span data-stu-id="b08ae-145">Related topics</span></span>
### <a name="scenarios"></a><span data-ttu-id="b08ae-146">Forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="b08ae-146">Scenarios</span></span>
* <span data-ttu-id="b08ae-147">Üzleti folytonosság áttekintéséért lásd: [üzleti folytonosság – áttekintés](../sql-database/sql-database-business-continuity.md)</span><span class="sxs-lookup"><span data-stu-id="b08ae-147">For a business continuity overview, see [Business continuity overview](../sql-database/sql-database-business-continuity.md)</span></span>

<!-- ### Tasks -->

<span data-ttu-id="b08ae-148">A data warehouse visszaállításhoz, visszaállítás segítségével:</span><span class="sxs-lookup"><span data-stu-id="b08ae-148">To perform a data warehouse restore, restore using:</span></span>

* <span data-ttu-id="b08ae-149">Az Azure portál, lásd: [visszaállítása egy adatraktár, az Azure portál használatával](sql-data-warehouse-restore-database-portal.md)</span><span class="sxs-lookup"><span data-stu-id="b08ae-149">Azure portal, see [Restore a data warehouse using the Azure portal](sql-data-warehouse-restore-database-portal.md)</span></span>
* <span data-ttu-id="b08ae-150">PowerShell-parancsmagok [visszaállítani az PowerShell-parancsmagok használatával adatraktár](sql-data-warehouse-restore-database-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="b08ae-150">PowerShell cmdlets, see [Restore a data warehouse using PowerShell cmdlets](sql-data-warehouse-restore-database-powershell.md)</span></span>
* <span data-ttu-id="b08ae-151">REST-API-k, lásd: [a REST API-k használatával adatraktár visszaállítása](sql-data-warehouse-restore-database-rest-api.md)</span><span class="sxs-lookup"><span data-stu-id="b08ae-151">REST APIs, see [Restore a data warehouse using the REST APIs](sql-data-warehouse-restore-database-rest-api.md)</span></span>

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
