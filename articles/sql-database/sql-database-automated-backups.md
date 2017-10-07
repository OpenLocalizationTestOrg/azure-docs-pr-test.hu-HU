---
title: "aaaAzure SQL-adatbázis automatikus georedundáns biztonsági mentés |} Microsoft Docs"
description: "SQL-adatbázis automatikusan létrehoz egy helyi adatbázis biztonsági másolatának néhány percenként és Azure írásvédett georedundáns tárolást használ georedundancia."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 3ee3d49d-16fa-47cf-a3ab-7b22aa491a8d
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/05/2017
ms.author: carlrab
ms.openlocfilehash: 8aff5356e8142707dd7cd2533a4aa5ea8fec866d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="learn-about-automatic-sql-database-backups"></a>További tudnivalók az automatikus SQL-adatbázis biztonsági mentése

SQL-adatbázis automatikusan létrehozza, és az Azure-írásvédett georedundáns tárolás (RA-GRS) tooprovide-georedundancia használja. Ezek a biztonsági másolatok jönnek létre automatikusan és nem kell külön fizetni. Nincs szükség a toodo semmi toomake őket fordulhat elő. Adatbázis biztonsági mentése az üzleti folytonossági és vészhelyreállítási helyreállítási stratégia fontos részét képezik, mivel ezek az adatok védelméhez véletlen sérülése vagy törlése. Ha a saját tároló tookeep biztonsági mentések kívánt konfigurálhatja egy hosszú távú biztonsági mentés megőrzési házirend. További információkért lásd: [Hosszú távú megőrzés](sql-database-long-term-retention.md).

## <a name="what-is-a-sql-database-backup"></a>Mi az az SQL-adatbázis biztonsági másolatának?

SQL-adatbázist használja az SQL Server-technológia toocreate [teljes](https://msdn.microsoft.com/library/ms186289.aspx), [különbözeti](https://msdn.microsoft.com/library/ms175526.aspx), és [tranzakciónapló](https://msdn.microsoft.com/library/ms191429.aspx) biztonsági mentéseket. hello tranzakciónapló biztonsági mentései hello gyakorisággal hello teljesítményszintet és adatbázis-tevékenység általában 5-10 percenként fordulhat elő. Tranzakciónapló biztonsági mentései, teljes és különbözeti biztonsági másolat, lehetővé teszi egy adatbázis tooa adott időpontban toohello toorestore ugyanarra a kiszolgálóra, amelyen hello adatbázis. Ha egy adatbázis visszaállításához hello szolgáltatás adatok melyik teljes, különbségi és tranzakciós naplók biztonsági másolatainak toobe vissza kell.


A biztonsági mentések használhatók:

* Állítsa vissza egy adatbázis tooa-időpontban hello megőrzési időn belül. Ez a művelet egy új adatbázist fog létrehozni hello hello eredeti adatbázist ugyanarra a kiszolgálóra.
* Állítsa vissza a törölt adatbázisok toohello idő törölve lett vagy hello megőrzési időn belül. hello törölt adatbázis visszaállítása hello csak végezhető ahol hello az eredeti adatbázis készült ugyanarra a kiszolgálóra.
* Állítsa vissza az adatbázis-tooanother földrajzi területen. Ez lehetővé teszi egy földrajzi katasztrófa utáni toorecover amikor nem fér hozzá a kiszolgáló és az adatbázis. Bármelyik meglévő kiszolgálóról bárhol hello world hozna létre egy új adatbázist. 
* Adatbázis visszaállítása egy adott biztonsági másolatból az Azure Recovery Services-tároló tárolja. Ez lehetővé teszi egy régi verziója hello adatbázis toosatisfy megfelelőségi kérelmet toorestore vagy toorun hello alkalmazás egy régi verziója. Lásd: [hosszú távú megőrzési](sql-database-long-term-retention.md).
* a visszaállítás tooperform lásd: [adatbázis visszaállítása biztonsági másolatból](sql-database-recovery-using-backups.md).

> [!NOTE]
> Az Azure storage hello kifejezés *replikációs* az egyik helyen tooanother toocopying fájlok hivatkozik. SQL *adatbázis-replikáció* tookeeping toomultiple másodlagos adatbázis szinkronizálva az elsődleges adatbázis hivatkozik. 
> 

## <a name="how-much-backup-storage-is-included-at-no-cost"></a>Hány biztonsági másolatokat tároló megtalálható ingyenesen?
SQL-adatbázis biztonsági másolatok tárolásának további költségek nélkül biztosít a maximális kiosztott adatbázistár too200 %-át. Például ha egy szabványos DB példány kiosztott DB méretű 250 GB-os, 500 GB van biztonsági másolat tárolási használatáért nem kell külön fizetni. Ha az adatbázis meghaladja a megadott biztonsági másolatok tárolásának hello, dönthet úgy tooreduce hello megőrzési időszak lépjen kapcsolatba az Azure támogatási szolgálatához. Másik lehetőség is toopay díj hello standard írásvédett földrajzilag redundáns tárolás (RA-GRS) számlázott extra biztonsági másolatok tárolására. 

## <a name="how-often-do-backups-happen"></a>Milyen gyakran történik a biztonsági mentések?
Adatbázis teljes biztonsági mentések fordulhat elő, hetente, adatbázis különbözeti biztonsági mentések általánosan fordulhat elő, minden néhány óra, és a tranzakciós napló biztonsági mentések általában 5 – 10 percenként kerül sor. hello első teljes biztonsági mentés van ütemezve, azonnal adatbázis létrehozása után. Általában 30 percen belül befejeződik, de azt is tovább tarthat jelentős méretű hello adatbázis esetén. Például hello kezdeti biztonsági másolatot több időt vesz igénybe a visszaállított adatbázis vagy egy adatbázis-másolat. Hello első teljes biztonsági mentés óta minden további biztonsági mentések ütemezett automatikusan és hello háttérben csendes felügyelni. hello pontos összes adatbázis biztonsági mentés ütemezése határozza meg hello SQL adatbázis-szolgáltatás által általános hello egyenlegének munkaterhelés. 

hello biztonsági mentési tároló földrajzi régiók közötti replikáció hello Azure Storage replikációs ütemezés alapján történik.

## <a name="how-long-do-you-keep-my-backups"></a>Mennyi ideig megtartja a biztonsági másolatok?
Minden egyes SQL-adatbázis biztonsági mentése van a megőrzési időtartam hello alapuló [szolgáltatásréteg](sql-database-service-tiers.md) hello adatbázis. egy adatbázis hello megőrzési időtartama a:


* Alapszintű service érvényben lévő korlát miatt 7 nap.
* Standard szintű szolgáltatási rétegben 35 napon.
* Prémium szolgáltatásszintet 35 napon.

Ha használni a hello Standard vagy Premium szolgáltatás rétegek tooBasic adatbázist, hello biztonsági másolatok hét napja. Már nem érhetők el minden létező biztonsági másolatai, régebbi, mint hét nap. 

Ha az adatbázis hello alapszintű service réteg tooStandard vagy Premium frissít, SQL-adatbázis meglévő biztonsági másolatok tartja, amíg azokat régi 35 napon. Új biztonsági másolatok 35 napon előforduló tartja.

Ha töröl egy adatbázist, SQL-adatbázis hello hello biztonsági mentések tartja azt szeretné, online adatbázis azonos módon. Tegyük fel, hogy törli a hét napos megőrzési idővel rendelkező alapszintű adatbázis. Olyan biztonsági mentésből négy napos három további napig kerül.

> [!IMPORTANT]
> Ha törli hello Azure SQL-kiszolgálót futtató SQL-adatbázisok, toohello kiszolgálóhoz tartozó összes adatbázis is törlődnek, és nem állítható helyre. A Törölt kiszolgáló nem tudja visszaállítani.
> 

## <a name="how-tooextend-hello-backup-retention-period"></a>Hogyan tooextend hello biztonsági mentés megőrzési időszak?
Ha az alkalmazás által igényelt hello biztonsági mentések érhetők el a hosszabb ideig hello beépített megőrzési időszak kibővítheti hello hosszú távú biztonsági másolatok megőrzésének-házirend konfigurálása az egyes adatbázisok (balról jobbra házirend). Ez lehetővé teszi tooextend hello beépített informatikai megőrzési időszak 35 napon tooup too10 évek. További információkért lásd: [Hosszú távú megőrzés](sql-database-long-term-retention.md).

Miután hozzáadta a hello balról jobbra házirend tooa adatbázis az Azure portál vagy API használatával, hello heti teljes adatbázis biztonsági másolatait lesz automatikusan másolt tooyour saját Azure Backup szolgáltatás tárolóba. Ha az adatbázis titkosított TDE hello biztonsági mentések automatikusan titkosítva vannak, aktívan.  hello Services-tároló automatikusan törli a lejárt biztonsági mentések időbélyeg és hello balról jobbra házirend alapján.  Így nem kell toomanage hello biztonsági mentési ütemezés vagy kapcsolatos hello törlésének aggódjon hello régi fájlokat. hello visszaállítási API támogatja hello tárolt biztonsági mindaddig, amíg hello tárolóban található tároló hello ugyanahhoz az előfizetéshez legyen az SQL-adatbázis. Használhatja hello Azure-portál vagy PowerShell tooaccess biztonsági mentéseket.

> [!TIP]
> Tekintse meg a-tooguide, hogyan [és az Azure SQL Database hosszú távú biztonsági mentés megőrzési visszaállítás konfigurálása](sql-database-long-term-backup-retention-configure.md)
>

## <a name="are-backups-encrypted"></a>Biztonsági mentés titkosítása?

Ha a TDE engedélyezve van az Azure SQL-adatbázis, a biztonsági mentések is titkosítást kapnak. Minden új Azure SQL-adatbázisok TDE alapértelmezés szerint engedélyezve vannak konfigurálva. A TDE további információkért lásd: [átlátható adattitkosítást az Azure SQL Database](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-with-azure-sql-database).

## <a name="next-steps"></a>Következő lépések

- Adatbázis biztonsági mentése az üzleti folytonossági és vészhelyreállítási helyreállítási stratégia fontos részét képezik, mivel ezek az adatok védelméhez véletlen sérülése vagy törlése. toolearn készül hello Azure SQL Database üzleti folytonossági megoldásait, lásd: [üzleti folytonosság – áttekintés](sql-database-business-continuity.md).
- toorestore tooa pont időben hello Azure-portál használatával lásd: [visszaállítási adatbázis tooa pont, amikor Azure-portálon hello](sql-database-recovery-using-backups.md).
- toorestore tooa pont, amikor PowerShell, lásd: [visszaállítási adatbázis tooa pont, amikor PowerShell](scripts/sql-database-restore-database-powershell.md).
- tooconfigure, kezelése és az Azure portál, lásd: az Azure Recovery Services-tároló hello segítségével automatizált a biztonsági másolatok hosszú távú megőrzése visszaállíthatók [kezelése hosszú távú biztonsági másolatok megőrzésének használatával hello Azure-portálon](sql-database-long-term-backup-retention-configure.md).
- tooconfigure, kezeléséhez, és az automatikus biztonsági mentések egy PowerShell-lel Azure Recovery Services-tárolónak a hosszú távú megőrzési visszaállítás, lásd: [kezelése a PowerShell használatával hosszú távú biztonsági másolatok megőrzésének](sql-database-long-term-backup-retention-configure.md).
