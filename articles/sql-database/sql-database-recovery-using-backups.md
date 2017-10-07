---
title: "egy Azure SQL Database adatbázist egy biztonsági másolatból aaaRestore |} Microsoft Docs"
description: "További információk a pont időponthoz kötött visszaállítás, amely lehetővé teszi tooroll vissza egy Azure SQL Database tooa korábbi időpontra időben (felfelé too35 nap)."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: monicar
ms.assetid: fd1d334d-a035-4a55-9446-d1cf750d9cf7
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/15/2017
ms.author: carlrab
ms.openlocfilehash: 84ecc306a2072445c10dbf1e9d7cf3d1b43860cf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="recover-an-azure-sql-database-using-automated-database-backups"></a>Automatikus adatbázis biztonsági mentését használó Azure SQL-adatbázis helyreállítása
SQL-adatbázis biztosítja ezeket a beállításokat, az adatbázis helyreállítási használatával [adatbázis biztonsági másolatait automatikus](sql-database-automated-backups.md) és [hosszú távú megőrzési a biztonsági másolatok](sql-database-long-term-retention.md). Egy adatbázis biztonsági másolatát arra állíthatja vissza:

* Egy új adatbázist a helyreállított azonos logikai kiszolgáló hello tooa megadott pont hello megőrzési időn belül. 
* Adatbázisról A hello a azonos logikai kiszolgáló toohello fióktörlési idő törölt adatbázis helyreállítása.
* Egyetlen logikai kiszolgálón minden régióban egy új adatbázist helyre toohello pont hello legutóbbi napi biztonsági mentések georeplikált blob Storage (RA-GRS).

> [!IMPORTANT]
> Meglévő adatbázis visszaállítása során nem írható felül.
>

Is [adatbázis biztonsági másolatait automatikus](sql-database-automated-backups.md) toocreate egy [adatbázis másolása](sql-database-copy.md) bármely régióban egyetlen logikai kiszolgálón. 

## <a name="recovery-time"></a>A helyreállítási idő
hello helyreállítási idő toorestore egy adatbázist automatizált mentéseket számos tényezőtől kihatással van: 

* hello adatbázis hello mérete
* hello teljesítményszintjének hello adatbázis
* tranzakciós naplók érintett hello száma
* a rendszer hello folyó tevékenység, amelyet a toobe játssza toorecover toohello visszaállítási pont
* Ha hello visszaállítása tooa más régióban hello hálózati sávszélesség 
* hello száma párhuzamos visszaállítási hello cél régióban feldolgozott kérelmek száma 
  
  Egy nagyon nagy és/vagy aktív adatbázis hello visszaállítási több óráig is eltarthat. Nincs leállás hosszabb régióban, akkor lehetséges, hogy vannak-e más régiókban által feldolgozott georedundáns helyreállítás kérelmek nagy mennyiségű. Ha sok kérelmet, hello helyreállítás ideje megnőhet adatbázisok az adott régióban. A legtöbb adatbázis visszaállítása befejeződött 12 órában.
  
Nincs beépített funkcióval toodo tömeges visszaállítási van. Hello [Azure SQL Database: a teljes kiszolgáló helyreállítási](https://gallery.technet.microsoft.com/Azure-SQL-Database-Full-82941666) parancsfájl Ez a feladat parancsokról egyik módja egy példát.

> [!IMPORTANT]
> toorecover használatával automatikus biztonsági mentés, csak hello előfizetésben hello SQL Server közreműködői szerepkör tagjai és hello előfizetés tulajdonosa. Hello Azure-portálon, a PowerShell vagy a REST API hello segítségével helyreállítható. A Transact-SQL nem használható. 
> 

## <a name="point-in-time-restore"></a>Pont időponthoz kötött visszaállítás

Visszaállíthatja a meglévő adatbázis tooan korábbi időpontra történő hello új adatbázisként azonos logikai kiszolgáló használatával hello Azure-portálon [PowerShell](https://docs.microsoft.com/en-us/powershell/module/azurerm.sql/restore-azurermsqldatabase), vagy hello [REST API](https://msdn.microsoft.com/library/azure/mt163685.aspx). 

> [!TIP]
> A PowerShell-parancsfájlpélda jelenít meg, hogyan tooperform pont időponthoz kötött visszaállítás, adatbázis, tekintse meg a [állítson vissza egy SQL-adatbázist PowerShell](scripts/sql-database-restore-database-powershell.md).
>

hello adatbázis lehet visszaállított tooany szolgáltatásréteg vagy a teljesítmény szinttel, és egyetlen adatbázisként vagy rugalmas készletbe. Győződjön meg arról, hello logikai kiszolgálón elegendő erőforrással rendelkezik, vagy a rugalmas készlet toowhich hello hello adatbázis visszaállítását végzi. Művelet befejeződése után vissza hello adatbázisa egy normál, teljes mértékben érhető el, online adatbázis. hello visszaállított adatbázis fel van töltve, a normál díjszabás alapján a szolgáltatás és teljesítményszintet szintje. Nem függő díj terheli hello adatbázis visszaállítás befejezéséig.

Egy adatbázis tooan általában visszaállítása korábbi helyreállítási célokra mutasson. Úgy is kezelheti során hello adatbázis visszaállítása helyettesíti a hello eredeti adatbázis vagy tooretrieve adatait használják, és frissítse a hello eredeti adatbázist. 

* ***Adatbázis-csere:*** hello visszaállított adatbázis helyettesíti a hello eredeti adatbázis célja, ha hello teljesítményszintet ellenőrizze és/vagy szolgáltatási rétegben megfelelő, a méretezés hello adatbázis, ha szükséges. Nevezze át hello eredeti adatbázist, és majd nevezze vissza hello adatbázis hello eredeti hello ALTER DATABASE parancs használatával a T-SQL-ben. 
* ***Adat-helyreállítás:*** Ha azt tervezi, hogy a felhasználó vagy alkalmazás hiba hello visszaállított adatbázis toorecover tooretrieve adatait, toowrite kell, és végre hello szükséges adatok helyreállítási parancsfájlok tooextract adatok visszaállítása hello adatbázis toohello eredeti adatbázist. Habár a hello visszaállítási művelet eltarthat egy ideig toocomplete, hello adatbázis visszaállítása a hello visszaállítási folyamat során hello az adatbázisok listája látható. Ha törli a hello adatbázis hello visszaállítás során, hello visszaállítási művelet megszakadt, és nem kell fizetnie, amely nem fejeződött be hello visszaállítási hello adatbázis. 

### <a name="azure-portal"></a>Azure Portal

toorecover tooa pont használatával hello Azure-portálon, az adatbázisról, és kattintson a Megnyitás hello lap **visszaállítása** hello eszköztáron.

![pont – idő visszaállítása](./media/sql-database-recovery-using-backups/point-in-time-recovery.png)

## <a name="deleted-database-restore"></a>A törölt adatbázis visszaállítása
Visszaállíthatja a törölt adatbázisok toohello fióktörlési idő azonos logikai kiszolgáló használ a törölt adatbázisok a hello Azure-portálon hello [PowerShell](https://docs.microsoft.com/en-us/powershell/module/azurerm.sql/restore-azurermsqldatabase), vagy hello [REST (createMode = visszaállítása)](https://msdn.microsoft.com/library/azure/mt163685.aspx). 

> [!TIP]
> Egy minta PowerShell parancsfájl-megjelenítő hogyan toorestore a törölt adatbázisok: [állítson vissza egy SQL-adatbázist PowerShell](scripts/sql-database-restore-database-powershell.md).
>

> [!IMPORTANT]
> Ha töröl egy Azure SQL adatbázis-kiszolgálópéldányra, az összes adatbázis is törlődnek, és nem állítható helyre. Jelenleg egy törölt kiszolgáló helyreállítása nem támogatott.
> 

### <a name="azure-portal"></a>Azure Portal

a törölt toorecover adatbázis-során a [megőrzési időszak](sql-database-service-tiers.md) használatának hello Azure-portálon nyitott hello lap a kiszolgálót és hello műveletek területen, kattintson a **adatbázisait törölte a**.

![törölt-adatbázis-visszaállítási-1](./media/sql-database-recovery-using-backups/deleted-database-restore-1.png)


![törölt-adatbázis-visszaállítási-2](./media/sql-database-recovery-using-backups/deleted-database-restore-2.png)

## <a name="geo-restore"></a>Georedundáns helyreállítás
Hello legutóbbi georeplikált teljes és különbözeti biztonsági mentések állíthatja vissza egy SQL-adatbázis minden olyan kiszolgálón bármely Azure régióban. Georedundáns helyreállítás georedundáns biztonsági használja, mint a forrás- és használt toorecover adatbázis akkor is, ha hello adatbázis vagy datacenter tooan szolgáltatáskimaradás miatt nem érhető el. 

Georedundáns helyreállítás hello alapértelmezett helyreállítási lehetőség esetén hello régióban, ahol hello adatbázis egy esemény miatt az adatbázis nem érhető el. Ha a felügyeleti teendők központjaként, amelyhez a régió eredmények hiányában az adatbázis-alkalmazás, visszaállíthatja egy adatbázis kiszolgálóról hello georeplikált biztonsági mentések tooa más régióban. Amikor a rendszer készít-e a különbözeti biztonsági mentése és georeplikált tooan Azure közötti késés blob egy másik régióban. Ez a késés lehet be az tooan, úgy, ha katasztrófa történik, lehet másolatot tooone óra adatvesztés. a következő ábra hello hello adatbázis visszaállítási hello utolsó rendelkezésre biztonsági másolat egy másik régióban jeleníti meg.

![georedundáns helyreállítás](./media/sql-database-geo-restore/geo-restore-2.png)

> [!TIP]
> Egy minta PowerShell parancsfájl-megjelenítő hogyan tooperform visszaállítás, georedundáns: [állítson vissza egy SQL-adatbázist PowerShell](scripts/sql-database-restore-database-powershell.md).
> 

Egy földrajzi másodlagos időpontban visszaállítás jelenleg nem támogatott. Csak az elsődleges adatbázis pont időponthoz kötött visszaállítás végezhető. A kimaradás georedundáns helyreállítás toorecover használatával kapcsolatos részletes információkért lásd: [kimaradás helyreállíthatók](sql-database-disaster-recovery.md).

> [!IMPORTANT]
> A biztonsági mentésekből helyreállítási hello katasztrófa a legalapvetőbb hello az SQL-adatbázisban elérhető helyreállítási megoldások hello leghosszabb RPO és becsült helyreállítási idő (Beszúrása). Alapszintű adatbázisok alkalmazó megoldások georedundáns helyreállítás esetén gyakran egy ésszerű vész-Helyreállítási megoldást egy Beszúrása 12 órát. A nagyobb Standard vagy prémium adatbázisok rövidebb helyreállítási időkről igénylő alkalmazó megoldások, érdemes lehet használni [aktív georeplikáció](sql-database-geo-replication-overview.md). Aktív georeplikáció sokkal rövidebb RPO és Beszúrása kínál csak szükséges, hogy egy feladatátvételi folyamatosan replikált tooa másodlagos kezdeményezni. Az üzleti contiuity lehetőségek további információkért lásd: [az üzletmenet folytonossága over](sql-database-business-continuity.md).
> 

### <a name="azure-portal"></a>Azure Portal

toogeo-visszaállítási adatbázis során a [megőrzési időszak](sql-database-service-tiers.md) hello Azure-portál használatával, nyissa meg hello SQL-adatbázisok lapját, és kattintson a **Hozzáadás**. A hello **forrás kiválasztása** válassza ki a szövegmezőben **biztonsági mentés**. Adja meg a hello biztonsági mentésre, a tooperform hello helyreállítási hello régióban, és az Ön által választott hello kiszolgálón. 

## <a name="programmatically-performing-recovery-using-automated-backups"></a>Programozott módon az automatikus biztonsági mentés segítségével történő helyreállítás végrehajtása
Korábban már említettük továbbá toohello Azure-portálon, az adatbázis helyreállításának programozott módon az Azure PowerShell használatával végezheti el, vagy hello REST API-t. a következő táblák hello hello készlete használható parancsok ismertetik.

### <a name="powershell"></a>PowerShell
| Parancsmag | Leírás |
| --- | --- |
| [Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase) |Egy vagy több adatbázis lekérdezi. |
| [Get-AzureRMSqlDeletedDatabaseBackup](/powershell/module/azurerm.sql/get-azurermsqldeleteddatabasebackup) | Lekérdezi a törölt adatbázisok, amelyek is helyreállíthatja. |
| [Get-AzureRmSqlDatabaseGeoBackup](/powershell/module/azurerm.sql/get-azurermsqldatabasegeobackup) |Egy adatbázis georedundáns biztonsági másolatot kap. |
| [Visszaállítás-AzureRmSqlDatabase](/powershell/module/azurerm.sql/restore-azurermsqldatabase) |SQL-adatbázis visszaállítása. |
|  | |

### <a name="rest-api"></a>REST API
| API | Leírás |
| --- | --- |
| [REST (createMode helyreállítási =)](https://msdn.microsoft.com/library/azure/mt163685.aspx) |Adatbázis visszaállítása |
| [Get létrehozása, vagy az adatbázis állapotának frissítése](https://msdn.microsoft.com/library/azure/mt643934.aspx) |A visszaállítási művelet során beolvasása hello állapota |
|  | |

## <a name="summary"></a>Összefoglalás
Az automatikus biztonsági mentés az adatbázisok védelméhez a felhasználói és a alkalmazáshibák, a véletlen adatbázisok törlése, a hosszabb kimaradások esetén. A beépített kapacitásprofilokban összes szolgáltatásszintek és teljesítményszintek érhető el. 

## <a name="next-steps"></a>Következő lépések
* Egy üzleti folytonosság – áttekintés és forgatókönyvek: [üzleti folytonosság – áttekintés](sql-database-business-continuity.md)
* tudnivalók Azure SQL adatbázis automatikus biztonsági mentés, toolearn lásd: [SQL-adatbázis automatikus biztonsági mentés](sql-database-automated-backups.md)
* toolearn vonatkozó hosszú távú biztonsági másolatok megőrzésének, lásd: [hosszú távú biztonsági másolatok megőrzésének](sql-database-long-term-retention.md)
* tooconfigure, kezelése és az Azure portál, lásd: az Azure Recovery Services-tároló hello segítségével automatizált a biztonsági másolatok hosszú távú megőrzése visszaállíthatók [konfigurálása és használata a hosszú távú biztonsági mentés megőrzési](sql-database-long-term-backup-retention-configure.md). 
* toolearn gyorsabb helyreállítási beállításokkal kapcsolatban lásd: [aktív georeplikáció](sql-database-geo-replication-overview.md)  
