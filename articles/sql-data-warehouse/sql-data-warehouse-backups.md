---
title: "aaaAzure SQL Data Warehouse biztonsági mentések - pillanatképeket, georedundáns |} Microsoft Docs"
description: "Ismerje meg, amelyek lehetővé teszik toorestore SQL Data Warehouse beépített mentéseket egy Azure SQL Data Warehouse tooa visszaállítási pont vagy egy másik földrajzi régióban."
services: sql-data-warehouse
documentationcenter: 
author: Lakshmi1812
manager: jhubbard
editor: 
ms.assetid: b5aff094-05b2-4578-acf3-ec456656febd
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.custom: backup-restore
ms.date: 10/31/2016
ms.author: lakshmir;barbkess
ms.openlocfilehash: 34659480485246f54a1490e185fc1b903fb2520d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sql-data-warehouse-backups"></a><span data-ttu-id="df4b4-103">Az SQL Data Warehouse biztonsági mentések</span><span class="sxs-lookup"><span data-stu-id="df4b4-103">SQL Data Warehouse backups</span></span>
<span data-ttu-id="df4b4-104">Az SQL Data Warehouse helyi, mind a földrajzi adatraktár biztonsági mentési platformképességei adatok részeként biztonsági mentést biztosít.</span><span class="sxs-lookup"><span data-stu-id="df4b4-104">SQL Data Warehouse offers both local and geographical backups as part of its data warehouse backup capabilities.</span></span> <span data-ttu-id="df4b4-105">Ezek közé tartozik az Azure Storage-Blobba pillanatképek és a georedundáns tárolást.</span><span class="sxs-lookup"><span data-stu-id="df4b4-105">These include Azure Storage Blob snapshots and geo-redundant storage.</span></span> <span data-ttu-id="df4b4-106">Használja a data warehouse biztonsági mentések toorestore a data warehouse tooa visszaállítási pont hello elsődleges régióban, vagy állítsa vissza a tooa különböző földrajzi régióban.</span><span class="sxs-lookup"><span data-stu-id="df4b4-106">Use data warehouse backups toorestore your data warehouse tooa restore point in hello primary region, or restore tooa different geographical region.</span></span> <span data-ttu-id="df4b4-107">Ez a cikk ismerteti az SQL Data Warehouse a biztonsági másolatok hello mintaadatokról.</span><span class="sxs-lookup"><span data-stu-id="df4b4-107">This article explains hello specifics of backups in SQL Data Warehouse.</span></span>

## <a name="what-is-a-data-warehouse-backup"></a><span data-ttu-id="df4b4-108">Mi az az adatraktár biztonsági mentését?</span><span class="sxs-lookup"><span data-stu-id="df4b4-108">What is a data warehouse backup?</span></span>
<span data-ttu-id="df4b4-109">Egy adatraktár biztonsági mentését az használható toorestore data warehouse tooa adott időszak hello adatai.</span><span class="sxs-lookup"><span data-stu-id="df4b4-109">A data warehouse backup is hello data that you can use toorestore a data warehouse tooa specific time.</span></span>  <span data-ttu-id="df4b4-110">Mivel az SQL Data Warehouse egy elosztott rendszerek, adatok adatraktár biztonsági sok fájlt tárolnak az Azure BLOB áll.</span><span class="sxs-lookup"><span data-stu-id="df4b4-110">Since SQL Data Warehouse is a distributed system, a data warehouse backup consists of many files that are stored in Azure blobs.</span></span> 

<span data-ttu-id="df4b4-111">Adatbázis biztonsági mentése az üzleti folytonossági és vészhelyreállítási helyreállítási stratégia fontos részét képezik, mivel ezek az adatok védelméhez véletlen sérülése vagy törlése.</span><span class="sxs-lookup"><span data-stu-id="df4b4-111">Database backups are an essential part of any business continuity and disaster recovery strategy because they protect your data from accidental corruption or deletion.</span></span> <span data-ttu-id="df4b4-112">További információkért lásd: [üzleti folytonosság – áttekintés](../sql-database/sql-database-business-continuity.md).</span><span class="sxs-lookup"><span data-stu-id="df4b4-112">For more information, see [Business continuity overview](../sql-database/sql-database-business-continuity.md).</span></span>

## <a name="data-redundancy"></a><span data-ttu-id="df4b4-113">Adatredundancia</span><span class="sxs-lookup"><span data-stu-id="df4b4-113">Data redundancy</span></span>
<span data-ttu-id="df4b4-114">Az SQL Data Warehouse az adatok védelmét az adatok tárolása helyileg redundáns (LRS) prémium szintű Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="df4b4-114">SQL Data Warehouse protects your data by storing your data in locally redundant (LRS) Azure Premium Storage.</span></span> <span data-ttu-id="df4b4-115">Az Azure Storage szolgáltatás hello adatok több szinkron másolatot helyi hibák esetén hello helyi data center tooguarantee transzparens adatvédelmet tárolja.</span><span class="sxs-lookup"><span data-stu-id="df4b4-115">This Azure Storage feature stores multiple synchronous copies of hello data in hello local data center tooguarantee transparent data protection if there are localized failures.</span></span> <span data-ttu-id="df4b4-116">Az redundanciája, amely biztosítja, hogy az adatok hibatűrését infrastruktúra problémák, például a lemezek meghibásodása.</span><span class="sxs-lookup"><span data-stu-id="df4b4-116">Data redundancy ensures that your data can survive infrastructure issues such as disk failures.</span></span> <span data-ttu-id="df4b4-117">Az redundanciája, amely biztosítja a hibatűrő az üzletmenet folytonosságát és a magas rendelkezésre állású infrastruktúra.</span><span class="sxs-lookup"><span data-stu-id="df4b4-117">Data redundancy ensures business continuity with a fault tolerant and highly available infrastructure.</span></span>

<span data-ttu-id="df4b4-118">További információk toolearn:</span><span class="sxs-lookup"><span data-stu-id="df4b4-118">toolearn more about:</span></span>

* <span data-ttu-id="df4b4-119">Prémium szintű storage, lásd: [prémium szintű Storage bemutatása tooAzure](../storage/common/storage-premium-storage.md).</span><span class="sxs-lookup"><span data-stu-id="df4b4-119">Azure Premium storage, see [Introduction tooAzure Premium Storage](../storage/common/storage-premium-storage.md).</span></span>
* <span data-ttu-id="df4b4-120">Helyileg redundáns tárolás, lásd: [Azure Storage replikációs](../storage/common/storage-redundancy.md#locally-redundant-storage).</span><span class="sxs-lookup"><span data-stu-id="df4b4-120">Locally Redundant storage, see [Azure Storage replication](../storage/common/storage-redundancy.md#locally-redundant-storage).</span></span>

## <a name="azure-storage-blob-snapshots"></a><span data-ttu-id="df4b4-121">Az Azure Storage-Blobba pillanatképek</span><span class="sxs-lookup"><span data-stu-id="df4b4-121">Azure Storage Blob snapshots</span></span>
<span data-ttu-id="df4b4-122">Prémium szintű Azure Storage használatának előnye, mint az SQL Data Warehouse használja Azure Storage-Blobba pillanatképek toobackup hello adatraktár helyileg.</span><span class="sxs-lookup"><span data-stu-id="df4b4-122">As a benefit of using Azure Premium Storage, SQL Data Warehouse uses Azure Storage Blob snapshots toobackup hello data warehouse locally.</span></span> <span data-ttu-id="df4b4-123">Visszaállíthatja egy adatok adatraktár tooa pillanatkép-visszaállítási pontot.</span><span class="sxs-lookup"><span data-stu-id="df4b4-123">You can restore a data warehouse tooa snapshot restore point.</span></span> <span data-ttu-id="df4b4-124">A pillanatképek legalább 8 óránként start és hét napja.</span><span class="sxs-lookup"><span data-stu-id="df4b4-124">Snapshots start a minimum of every eight hours and are available for seven days.</span></span>  

<span data-ttu-id="df4b4-125">További információk toolearn:</span><span class="sxs-lookup"><span data-stu-id="df4b4-125">toolearn more about:</span></span>

* <span data-ttu-id="df4b4-126">Az Azure blob-pillanatképeket, lásd: [blob pillanatkép létrehozása](../storage/blobs/storage-blob-snapshots.md).</span><span class="sxs-lookup"><span data-stu-id="df4b4-126">Azure blob snapshots, see [Create a blob snapshot](../storage/blobs/storage-blob-snapshots.md).</span></span>

## <a name="geo-redundant-backups"></a><span data-ttu-id="df4b4-127">Georedundáns biztonsági mentések</span><span class="sxs-lookup"><span data-stu-id="df4b4-127">Geo-redundant backups</span></span>
<span data-ttu-id="df4b4-128">24 óránként, az SQL Data Warehouse tárolja hello teljes adatraktár szabványos tárhelyén.</span><span class="sxs-lookup"><span data-stu-id="df4b4-128">Every 24 hours, SQL Data Warehouse stores hello full data warehouse in Standard storage.</span></span> <span data-ttu-id="df4b4-129">hello teljes adatraktár jön létre toomatch hello hello legutolsó pillanatfelvétel idején.</span><span class="sxs-lookup"><span data-stu-id="df4b4-129">hello full data warehouse is created toomatch hello time of hello last snapshot.</span></span> <span data-ttu-id="df4b4-130">Standard szintű tárolást hello tooa georedundáns tárfiókot olvasási hozzáférést (RA-GRS) tartozik.</span><span class="sxs-lookup"><span data-stu-id="df4b4-130">hello standard storage belongs tooa geo-redundant storage account with read access (RA-GRS).</span></span> <span data-ttu-id="df4b4-131">hello Azure Storage-RA-GRS funkció replikált hello biztonságimásolat-fájlok tooa [párosított adatközpont](../best-practices-availability-paired-regions.md).</span><span class="sxs-lookup"><span data-stu-id="df4b4-131">hello Azure Storage RA-GRS feature replicates hello backup files tooa [paired data center](../best-practices-availability-paired-regions.md).</span></span> <span data-ttu-id="df4b4-132">A georeplikáció biztosítja, hogy visszaállíthassa data warehouse-ba, abban az esetben, ha az elsődleges régióban hello pillanatképek nem férhet hozzá.</span><span class="sxs-lookup"><span data-stu-id="df4b4-132">This geo-replication ensures you can restore data warehouse in case you cannot access hello snapshots in your primary region.</span></span> 

<span data-ttu-id="df4b4-133">Ez a funkció alapértelmezés szerint van bekapcsolva.</span><span class="sxs-lookup"><span data-stu-id="df4b4-133">This feature is on by default.</span></span> <span data-ttu-id="df4b4-134">Ha nem szeretné, hogy toouse georedundáns biztonsági mentést, akkor is [elutasításhoz] (https://docs.microsoft.com/powershell/resourcemanager/Azurerm.sql/v2.1.0/Set-AzureRmSqlDatabaseGeoBackupPolicy?redirectedfrom=msdn).</span><span class="sxs-lookup"><span data-stu-id="df4b4-134">If you don't want toouse geo-redundant backups, you can [opt out] (https://docs.microsoft.com/powershell/resourcemanager/Azurerm.sql/v2.1.0/Set-AzureRmSqlDatabaseGeoBackupPolicy?redirectedfrom=msdn).</span></span> 

> [!NOTE]
> <span data-ttu-id="df4b4-135">Az Azure storage hello kifejezés *replikációs* az egyik helyen tooanother toocopying fájlok hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="df4b4-135">In Azure storage, hello term *replication* refers toocopying files from one location tooanother.</span></span> <span data-ttu-id="df4b4-136">SQL *adatbázis-replikáció* tookeeping toomultiple másodlagos adatbázis szinkronizálva az elsődleges adatbázis hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="df4b4-136">SQL's *database replication* refers tookeeping toomultiple secondary databases synchronized with a primary database.</span></span> 
> 
> 

> [!NOTE]
> <span data-ttu-id="df4b4-137">A DWU 9000 és DWU 18000 georedundáns biztonsági mentések nem tilthatják.</span><span class="sxs-lookup"><span data-stu-id="df4b4-137">You cannot opt out of geo-redundant backups with DWU 9000 and DWU 18000.</span></span> 
>
> 

<span data-ttu-id="df4b4-138">További információk toolearn:</span><span class="sxs-lookup"><span data-stu-id="df4b4-138">toolearn more about:</span></span>

* <span data-ttu-id="df4b4-139">Georedundáns tárolás, lásd: [Azure Storage replikációs](../storage/common/storage-redundancy.md).</span><span class="sxs-lookup"><span data-stu-id="df4b4-139">Geo-redundant storage, see [Azure Storage replication](../storage/common/storage-redundancy.md).</span></span>
* <span data-ttu-id="df4b4-140">RA-GRS-tároló, lásd: [írásvédett georedundáns tárolás](../storage/common/storage-redundancy.md#read-access-geo-redundant-storage).</span><span class="sxs-lookup"><span data-stu-id="df4b4-140">RA-GRS storage, see [Read-access geo-redundant storage](../storage/common/storage-redundancy.md#read-access-geo-redundant-storage).</span></span>

## <a name="data-warehouse-backup-schedule-and-retention-period"></a><span data-ttu-id="df4b4-141">Adatraktár ütemezett biztonsági mentés és a megőrzési időtartam</span><span class="sxs-lookup"><span data-stu-id="df4b4-141">Data warehouse backup schedule and retention period</span></span>
<span data-ttu-id="df4b4-142">Az SQL Data Warehouse pillanatképek a online adatraktárak hoz létre minden négy tooeight óra és minden egyes pillanatkép készítése a hét napja a memóriában tárolja.</span><span class="sxs-lookup"><span data-stu-id="df4b4-142">SQL Data Warehouse creates snapshots on your online data warehouses every four tooeight hours and keeps each snapshot for seven days.</span></span> <span data-ttu-id="df4b4-143">Az online adatbázisban tooone hello visszaállítási pontok hello visszaállíthatja az elmúlt hét napban.</span><span class="sxs-lookup"><span data-stu-id="df4b4-143">You can restore your online database tooone of hello restore points in hello past seven days.</span></span> 

<span data-ttu-id="df4b4-144">toosee hello legutolsó pillanatfelvétel indításakor az online SQL Data Warehouse a lekérdezés futtatása.</span><span class="sxs-lookup"><span data-stu-id="df4b4-144">toosee when hello last snapshot started, run this query on your online SQL Data Warehouse.</span></span> 

```sql
select top 1 *
from sys.pdw_loader_backup_runs 
order by run_id desc;
```

<span data-ttu-id="df4b4-145">Ha több mint hét nap tooretain pillanatképet, helyreállíthatja a visszaállítási pont tooa új adatraktárat.</span><span class="sxs-lookup"><span data-stu-id="df4b4-145">If you need tooretain a snapshot for longer than seven days, you can restore a restore point tooa new data warehouse.</span></span> <span data-ttu-id="df4b4-146">Hello visszaállítás befejeződött, az SQL Data Warehouse elindul pillanatképeinek a hello új adatraktárat.</span><span class="sxs-lookup"><span data-stu-id="df4b4-146">After hello restore is finished, SQL Data Warehouse starts creating snapshots on hello new data warehouse.</span></span> <span data-ttu-id="df4b4-147">Ha nem végez módosításokat toohello új adatraktárat, hello pillanatképek üres marad, így hello pillanatkép költség minimális.</span><span class="sxs-lookup"><span data-stu-id="df4b4-147">If you don't make changes toohello new data warehouse, hello snapshots stay empty and therefore hello snapshot cost is minimal.</span></span> <span data-ttu-id="df4b4-148">Hozzon létre pillanatképek is hello adatbázis tookeep SQL Data Warehouse sikerült szüneteltetése.</span><span class="sxs-lookup"><span data-stu-id="df4b4-148">You could also pause hello database tookeep SQL Data Warehouse from creating snapshots.</span></span>

### <a name="what-happens-toomy-backup-retention-while-my-data-warehouse-is-paused"></a><span data-ttu-id="df4b4-149">Toomy biztonsági másolatok megőrzésének mi történik, amíg a data warehouse-bA szüneteltetve van?</span><span class="sxs-lookup"><span data-stu-id="df4b4-149">What happens toomy backup retention while my data warehouse is paused?</span></span>
<span data-ttu-id="df4b4-150">Az SQL Data Warehouse nem hoz létre pillanatképek, és nem jár le pillanatképek közben az adatraktár működése szünetel.</span><span class="sxs-lookup"><span data-stu-id="df4b4-150">SQL Data Warehouse does not create snapshots and does not expire snapshots while a data warehouse is paused.</span></span> <span data-ttu-id="df4b4-151">hello pillanatkép kora nem változtatja meg, amíg hello adatraktár fel van függesztve.</span><span class="sxs-lookup"><span data-stu-id="df4b4-151">hello snapshot age does not change while hello data warehouse is paused.</span></span> <span data-ttu-id="df4b4-152">Pillanatkép megőrzési hello hány napig hello adatraktár online, nem a naptári napokon alapul.</span><span class="sxs-lookup"><span data-stu-id="df4b4-152">Snapshot retention is based on hello number of days hello data warehouse is online, not calendar days.</span></span>

<span data-ttu-id="df4b4-153">Például ha pillanatkép kezdődik. október 1 du. 4 és hello adatraktár du. 4: 3. októberi szünetel, hello pillanatkép két napos definícióval.</span><span class="sxs-lookup"><span data-stu-id="df4b4-153">For example, if a snapshot starts October 1 at 4 pm and hello data warehouse is paused October 3 at 4 pm, hello snapshot is two days old.</span></span> <span data-ttu-id="df4b4-154">Amikor hello adatraktár ismét online elérhető lesz a hello pillanatkép két napos definícióval.</span><span class="sxs-lookup"><span data-stu-id="df4b4-154">Whenever hello data warehouse comes back online hello snapshot is two days old.</span></span> <span data-ttu-id="df4b4-155">Ha hello adatraktár online állapotba kerül. október 5 du. 4:, hello pillanatkép két napos, és öt nap marad.</span><span class="sxs-lookup"><span data-stu-id="df4b4-155">If hello data warehouse comes online October 5 at 4 pm, hello snapshot is two days old and remains for five more days.</span></span>

<span data-ttu-id="df4b4-156">Hello adatraktár ismét online elérhető lesz, amikor az SQL Data Warehouse új pillanatfelvételek folytatja, és a pillanatképek lejár, ha az adatok több mint hét nap.</span><span class="sxs-lookup"><span data-stu-id="df4b4-156">When hello data warehouse comes back online, SQL Data Warehouse resumes new snapshots and expires snapshots when they have more than seven days of data.</span></span>

### <a name="how-long-is-hello-retention-period-for-a-dropped-data-warehouse"></a><span data-ttu-id="df4b4-157">Milyen hosszú legyen egy eldobott adatraktár hello megőrzési időtartama?</span><span class="sxs-lookup"><span data-stu-id="df4b4-157">How long is hello retention period for a dropped data warehouse?</span></span>
<span data-ttu-id="df4b4-158">Adatraktár megszakad, amikor hello adatraktár hello pillanatképek hét napja mentett és majd eltávolítani.</span><span class="sxs-lookup"><span data-stu-id="df4b4-158">When a data warehouse is dropped, hello data warehouse and hello snapshots are saved for seven days and then removed.</span></span> <span data-ttu-id="df4b4-159">Hello data warehouse tooany mentett hello visszaállítási pontok is helyreállíthatja.</span><span class="sxs-lookup"><span data-stu-id="df4b4-159">You can restore hello data warehouse tooany of hello saved restore points.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="df4b4-160">Ha törli a logikai SQL server-példányt, toohello példányhoz tartozó összes adatbázis is törlődnek, és nem állítható helyre.</span><span class="sxs-lookup"><span data-stu-id="df4b4-160">If you delete a logical SQL server instance, all databases that belong toohello instance are also deleted and cannot be recovered.</span></span> <span data-ttu-id="df4b4-161">A Törölt kiszolgáló nem tudja visszaállítani.</span><span class="sxs-lookup"><span data-stu-id="df4b4-161">You cannot restore a deleted server.</span></span>
> 
> 

## <a name="data-warehouse-backup-costs"></a><span data-ttu-id="df4b4-162">Adatok adatraktár biztonsági mentési költségek</span><span class="sxs-lookup"><span data-stu-id="df4b4-162">Data warehouse backup costs</span></span>
<span data-ttu-id="df4b4-163">hello teljes költségeket, az elsődleges adatraktár és az Azure Blob-pillanatképek hét nap legközelebbi TB kerekített toohello.</span><span class="sxs-lookup"><span data-stu-id="df4b4-163">hello total cost for your primary data warehouse and seven days of Azure Blob snapshots is rounded toohello nearest TB.</span></span> <span data-ttu-id="df4b4-164">Például ha az adatraktár 1,5 TB és hello pillanatképek a 100 GB, díját is felszámítjuk a 2 TB prémium szintű Azure Storage ütemben adat.</span><span class="sxs-lookup"><span data-stu-id="df4b4-164">For example, if your data warehouse is 1.5 TB and hello snapshots use 100 GB, you are billed for 2 TB of data at Azure Premium Storage rates.</span></span> 

> [!NOTE]
> <span data-ttu-id="df4b4-165">Minden egyes pillanatkép üres kezdetben és érő módosítások toohello elsődleges adatraktár elvégezte.</span><span class="sxs-lookup"><span data-stu-id="df4b4-165">Each snapshot is empty initially and grows as you make changes toohello primary data warehouse.</span></span> <span data-ttu-id="df4b4-166">A méret növekedését minden pillanatképet hello data warehouse módosítás.</span><span class="sxs-lookup"><span data-stu-id="df4b4-166">All snapshots increase in size as hello data warehouse changes.</span></span> <span data-ttu-id="df4b4-167">Ezért a hello tárolási költségek a pillanatképek módosítási aránya függően toohello nő.</span><span class="sxs-lookup"><span data-stu-id="df4b4-167">Therefore, hello storage costs for snapshots grow according toohello rate of change.</span></span>
> 
> 

<span data-ttu-id="df4b4-168">Ha georedundáns tárolást használ, akkor kap egy különálló tárhelyet kell fizetni.</span><span class="sxs-lookup"><span data-stu-id="df4b4-168">If you are using geo-redundant storage, you receive a separate storage charge.</span></span> <span data-ttu-id="df4b4-169">hello georedundáns tárolás lesz számlázva díj hello standard írásvédett földrajzilag redundáns tárolás (RA-GRS).</span><span class="sxs-lookup"><span data-stu-id="df4b4-169">hello geo-redundant storage is billed at hello standard Read-Access Geographically Redundant Storage (RA-GRS) rate.</span></span>

<span data-ttu-id="df4b4-170">További információ az SQL Data Warehouse díjszabása: [SQL Data Warehouse díjszabása](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).</span><span class="sxs-lookup"><span data-stu-id="df4b4-170">For more information about SQL Data Warehouse pricing, see [SQL Data Warehouse Pricing](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).</span></span>

## <a name="using-database-backups"></a><span data-ttu-id="df4b4-171">Adatbázis biztonsági másolatokból</span><span class="sxs-lookup"><span data-stu-id="df4b4-171">Using database backups</span></span>
<span data-ttu-id="df4b4-172">hello elsődleges SQL data warehouse biztonsági mentés használata toorestore hello data warehouse tooone hello visszaállítási pontok hello megőrzési időn belül.</span><span class="sxs-lookup"><span data-stu-id="df4b4-172">hello primary use for SQL data warehouse backups is toorestore hello data warehouse tooone of hello restore points within hello retention period.</span></span>  

* <span data-ttu-id="df4b4-173">egy SQL data warehouse toorestore lásd [visszaállítása az SQL data warehouse](sql-data-warehouse-restore-database-overview.md).</span><span class="sxs-lookup"><span data-stu-id="df4b4-173">toorestore a SQL data warehouse, see [Restore a SQL data warehouse](sql-data-warehouse-restore-database-overview.md).</span></span>

## <a name="related-topics"></a><span data-ttu-id="df4b4-174">Kapcsolódó témakörök</span><span class="sxs-lookup"><span data-stu-id="df4b4-174">Related topics</span></span>
### <a name="scenarios"></a><span data-ttu-id="df4b4-175">Forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="df4b4-175">Scenarios</span></span>
* <span data-ttu-id="df4b4-176">Üzleti folytonosság áttekintéséért lásd: [üzleti folytonosság – áttekintés](../sql-database/sql-database-business-continuity.md)</span><span class="sxs-lookup"><span data-stu-id="df4b4-176">For a business continuity overview, see [Business continuity overview](../sql-database/sql-database-business-continuity.md)</span></span>

<!-- ### Tasks -->

* <span data-ttu-id="df4b4-177">egy adatraktár toorestore lásd: [SQL data warehouse visszaállítása](sql-data-warehouse-restore-database-overview.md)</span><span class="sxs-lookup"><span data-stu-id="df4b4-177">toorestore a data warehouse, see [Restore a SQL data warehouse](sql-data-warehouse-restore-database-overview.md)</span></span>

<!-- ### Tutorials -->

