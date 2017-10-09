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
# <a name="sql-data-warehouse-backups"></a>Az SQL Data Warehouse biztonsági mentések
Az SQL Data Warehouse helyi, mind a földrajzi adatraktár biztonsági mentési platformképességei adatok részeként biztonsági mentést biztosít. Ezek közé tartozik az Azure Storage-Blobba pillanatképek és a georedundáns tárolást. Használja a data warehouse biztonsági mentések toorestore a data warehouse tooa visszaállítási pont hello elsődleges régióban, vagy állítsa vissza a tooa különböző földrajzi régióban. Ez a cikk ismerteti az SQL Data Warehouse a biztonsági másolatok hello mintaadatokról.

## <a name="what-is-a-data-warehouse-backup"></a>Mi az az adatraktár biztonsági mentését?
Egy adatraktár biztonsági mentését az használható toorestore data warehouse tooa adott időszak hello adatai.  Mivel az SQL Data Warehouse egy elosztott rendszerek, adatok adatraktár biztonsági sok fájlt tárolnak az Azure BLOB áll. 

Adatbázis biztonsági mentése az üzleti folytonossági és vészhelyreállítási helyreállítási stratégia fontos részét képezik, mivel ezek az adatok védelméhez véletlen sérülése vagy törlése. További információkért lásd: [üzleti folytonosság – áttekintés](../sql-database/sql-database-business-continuity.md).

## <a name="data-redundancy"></a>Adatredundancia
Az SQL Data Warehouse az adatok védelmét az adatok tárolása helyileg redundáns (LRS) prémium szintű Azure Storage. Az Azure Storage szolgáltatás hello adatok több szinkron másolatot helyi hibák esetén hello helyi data center tooguarantee transzparens adatvédelmet tárolja. Az redundanciája, amely biztosítja, hogy az adatok hibatűrését infrastruktúra problémák, például a lemezek meghibásodása. Az redundanciája, amely biztosítja a hibatűrő az üzletmenet folytonosságát és a magas rendelkezésre állású infrastruktúra.

További információk toolearn:

* Prémium szintű storage, lásd: [prémium szintű Storage bemutatása tooAzure](../storage/common/storage-premium-storage.md).
* Helyileg redundáns tárolás, lásd: [Azure Storage replikációs](../storage/common/storage-redundancy.md#locally-redundant-storage).

## <a name="azure-storage-blob-snapshots"></a>Az Azure Storage-Blobba pillanatképek
Prémium szintű Azure Storage használatának előnye, mint az SQL Data Warehouse használja Azure Storage-Blobba pillanatképek toobackup hello adatraktár helyileg. Visszaállíthatja egy adatok adatraktár tooa pillanatkép-visszaállítási pontot. A pillanatképek legalább 8 óránként start és hét napja.  

További információk toolearn:

* Az Azure blob-pillanatképeket, lásd: [blob pillanatkép létrehozása](../storage/blobs/storage-blob-snapshots.md).

## <a name="geo-redundant-backups"></a>Georedundáns biztonsági mentések
24 óránként, az SQL Data Warehouse tárolja hello teljes adatraktár szabványos tárhelyén. hello teljes adatraktár jön létre toomatch hello hello legutolsó pillanatfelvétel idején. Standard szintű tárolást hello tooa georedundáns tárfiókot olvasási hozzáférést (RA-GRS) tartozik. hello Azure Storage-RA-GRS funkció replikált hello biztonságimásolat-fájlok tooa [párosított adatközpont](../best-practices-availability-paired-regions.md). A georeplikáció biztosítja, hogy visszaállíthassa data warehouse-ba, abban az esetben, ha az elsődleges régióban hello pillanatképek nem férhet hozzá. 

Ez a funkció alapértelmezés szerint van bekapcsolva. Ha nem szeretné, hogy toouse georedundáns biztonsági mentést, akkor is [elutasításhoz] (https://docs.microsoft.com/powershell/resourcemanager/Azurerm.sql/v2.1.0/Set-AzureRmSqlDatabaseGeoBackupPolicy?redirectedfrom=msdn). 

> [!NOTE]
> Az Azure storage hello kifejezés *replikációs* az egyik helyen tooanother toocopying fájlok hivatkozik. SQL *adatbázis-replikáció* tookeeping toomultiple másodlagos adatbázis szinkronizálva az elsődleges adatbázis hivatkozik. 
> 
> 

> [!NOTE]
> A DWU 9000 és DWU 18000 georedundáns biztonsági mentések nem tilthatják. 
>
> 

További információk toolearn:

* Georedundáns tárolás, lásd: [Azure Storage replikációs](../storage/common/storage-redundancy.md).
* RA-GRS-tároló, lásd: [írásvédett georedundáns tárolás](../storage/common/storage-redundancy.md#read-access-geo-redundant-storage).

## <a name="data-warehouse-backup-schedule-and-retention-period"></a>Adatraktár ütemezett biztonsági mentés és a megőrzési időtartam
Az SQL Data Warehouse pillanatképek a online adatraktárak hoz létre minden négy tooeight óra és minden egyes pillanatkép készítése a hét napja a memóriában tárolja. Az online adatbázisban tooone hello visszaállítási pontok hello visszaállíthatja az elmúlt hét napban. 

toosee hello legutolsó pillanatfelvétel indításakor az online SQL Data Warehouse a lekérdezés futtatása. 

```sql
select top 1 *
from sys.pdw_loader_backup_runs 
order by run_id desc;
```

Ha több mint hét nap tooretain pillanatképet, helyreállíthatja a visszaállítási pont tooa új adatraktárat. Hello visszaállítás befejeződött, az SQL Data Warehouse elindul pillanatképeinek a hello új adatraktárat. Ha nem végez módosításokat toohello új adatraktárat, hello pillanatképek üres marad, így hello pillanatkép költség minimális. Hozzon létre pillanatképek is hello adatbázis tookeep SQL Data Warehouse sikerült szüneteltetése.

### <a name="what-happens-toomy-backup-retention-while-my-data-warehouse-is-paused"></a>Toomy biztonsági másolatok megőrzésének mi történik, amíg a data warehouse-bA szüneteltetve van?
Az SQL Data Warehouse nem hoz létre pillanatképek, és nem jár le pillanatképek közben az adatraktár működése szünetel. hello pillanatkép kora nem változtatja meg, amíg hello adatraktár fel van függesztve. Pillanatkép megőrzési hello hány napig hello adatraktár online, nem a naptári napokon alapul.

Például ha pillanatkép kezdődik. október 1 du. 4 és hello adatraktár du. 4: 3. októberi szünetel, hello pillanatkép két napos definícióval. Amikor hello adatraktár ismét online elérhető lesz a hello pillanatkép két napos definícióval. Ha hello adatraktár online állapotba kerül. október 5 du. 4:, hello pillanatkép két napos, és öt nap marad.

Hello adatraktár ismét online elérhető lesz, amikor az SQL Data Warehouse új pillanatfelvételek folytatja, és a pillanatképek lejár, ha az adatok több mint hét nap.

### <a name="how-long-is-hello-retention-period-for-a-dropped-data-warehouse"></a>Milyen hosszú legyen egy eldobott adatraktár hello megőrzési időtartama?
Adatraktár megszakad, amikor hello adatraktár hello pillanatképek hét napja mentett és majd eltávolítani. Hello data warehouse tooany mentett hello visszaállítási pontok is helyreállíthatja.

> [!IMPORTANT]
> Ha törli a logikai SQL server-példányt, toohello példányhoz tartozó összes adatbázis is törlődnek, és nem állítható helyre. A Törölt kiszolgáló nem tudja visszaállítani.
> 
> 

## <a name="data-warehouse-backup-costs"></a>Adatok adatraktár biztonsági mentési költségek
hello teljes költségeket, az elsődleges adatraktár és az Azure Blob-pillanatképek hét nap legközelebbi TB kerekített toohello. Például ha az adatraktár 1,5 TB és hello pillanatképek a 100 GB, díját is felszámítjuk a 2 TB prémium szintű Azure Storage ütemben adat. 

> [!NOTE]
> Minden egyes pillanatkép üres kezdetben és érő módosítások toohello elsődleges adatraktár elvégezte. A méret növekedését minden pillanatképet hello data warehouse módosítás. Ezért a hello tárolási költségek a pillanatképek módosítási aránya függően toohello nő.
> 
> 

Ha georedundáns tárolást használ, akkor kap egy különálló tárhelyet kell fizetni. hello georedundáns tárolás lesz számlázva díj hello standard írásvédett földrajzilag redundáns tárolás (RA-GRS).

További információ az SQL Data Warehouse díjszabása: [SQL Data Warehouse díjszabása](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).

## <a name="using-database-backups"></a>Adatbázis biztonsági másolatokból
hello elsődleges SQL data warehouse biztonsági mentés használata toorestore hello data warehouse tooone hello visszaállítási pontok hello megőrzési időn belül.  

* egy SQL data warehouse toorestore lásd [visszaállítása az SQL data warehouse](sql-data-warehouse-restore-database-overview.md).

## <a name="related-topics"></a>Kapcsolódó témakörök
### <a name="scenarios"></a>Forgatókönyvek
* Üzleti folytonosság áttekintéséért lásd: [üzleti folytonosság – áttekintés](../sql-database/sql-database-business-continuity.md)

<!-- ### Tasks -->

* egy adatraktár toorestore lásd: [SQL data warehouse visszaállítása](sql-data-warehouse-restore-database-overview.md)

<!-- ### Tutorials -->

