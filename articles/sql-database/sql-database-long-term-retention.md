---
title: "aaaStore Azure SQL Database készített biztonsági másolatot too10 év |} Microsoft Docs"
description: "Ismerje meg, hogyan támogatja az Azure SQL Database a tárolni készített biztonsági másolatot too10 év."
keywords: 
services: sql-database
documentationcenter: 
author: anosov1960
manager: jhubbard
editor: 
ms.assetid: 66fdb8b8-5903-4d3a-802e-af08d204566e
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/22/2016
ms.author: sashan
ms.openlocfilehash: 5825ebd4e3bd66b59b13aea603d377ef814a1df3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="store-azure-sql-database-backups-for-up-too10-years"></a>Az Azure SQL Database biztonsági másolatok a too10 év mentése
Számos alkalmazás rendelkezik szabályozó, megfelelőségi vagy egyéb üzleti célokat szolgál, amely a használatba tooretain adatbázis biztonsági másolatait túl hello Azure SQL Database által biztosított 7 – 35 nap [automatikus biztonsági mentésekhez](sql-database-automated-backups.md). Hello hosszú távú biztonsági másolatok megőrzésének funkció használatával tárolhatja az Azure Recovery Services-tároló a too10 év fel az SQL-adatbázis biztonsági másolatait. Too1, 000 adatbázisokat tároló egy másolatot is tárolhatja. Majd igény szerint biztonsági másolat a hello tároló toorestore azt egy új adatbázist.

> [!IMPORTANT]
> Hosszú távú biztonsági másolatok megőrzésének jelenleg jelenleg előzetes és érhető el a következő régiókban hello: Kelet-Ausztrália, Ausztrália délkeleti, Dél-Brazília, USA középső RÉGIÓJA, Kelet-Ázsia, USA keleti régiója, USA keleti régiója 2. régiója, közép-India, Dél-India, kelet-japán, Nyugat-japán, északi középső Régiójában, Észak Európa, USA déli középső RÉGIÓJA, Délkelet-Ázsiában, Nyugat-Európában és USA nyugati régiója
>

> [!NOTE]
> Engedélyezheti / tároló too200 adatbázisok egy 24 órás időszakban. Azt javasoljuk, hogy minden kiszolgáló toominimize hello hatása ezt a határt egy külön tárolóba használjon. 
> 

## <a name="how-sql-database-long-term-backup-retention-works"></a>SQL-adatbázis hosszú távú biztonsági másolatok megőrzésének működése

A hosszú távú biztonsági másolatok megőrzésének az Azure Recovery Services-tároló SQL adatbázis-kiszolgálót is társíthat. 

* Hello tárolóban kell létrehoznia a hello azonos Azure-előfizetéshez létrehozott hello SQL server és a hello azonos földrajzi régióban és erőforrás-csoport. 
* Konfigurálhat egy megőrzési házirend bármely adatbázis. hello házirend okok hello heti adatbázis teljes biztonsági mentések toobe toohello Recovery Services-tároló másolta, és a (felfelé too10 év) hello megadott megőrzési ideig őrzi meg. 
* Visszaállíthatja a hello adatbázis bármelyik a biztonsági mentések tooa új adatbázis bármely kiszolgálón hello előfizetésben. Az Azure storage létrehoz egy másolatot a meglévő biztonsági mentésekből, és hello példány nincs hatása van hello létező adatbázis felett.

> [!TIP]
> Tekintse meg a-tooguide, hogyan [és konfigurálása az Azure SQL Database hosszú távú biztonsági mentés megőrzési visszaállítás](sql-database-long-term-backup-retention-configure.md).

## <a name="enable-long-term-backup-retention"></a>Hosszú távú biztonsági másolatok megőrzésének engedélyezése

tooconfigure hosszú távú biztonsági másolatok megőrzésének adatbázis esetén:

1. Hozzon létre egy Azure Recovery Services-tároló hello azonos régióban, az előfizetés és az erőforrás tartozik, mint az SQL database-kiszolgálóhoz. 
2. Regisztrálja a hello server toohello tárolóban.
3. Hozzon létre egy Azure Recovery Services védelmi házirendet.
4. Hello védelmi házirend toohello adatbázisok hosszú távú biztonsági másolatok megőrzésének igénylő alkalmazni.

tooconfigure, kezeléséhez, és állítsa vissza egy adatbázist egy Azure Recovery Services-tároló az automatikus biztonsági másolatok hosszú távú biztonsági másolatok megőrzésének, hello alábbi módszerekkel:

* Az Azure portál használatával hello: kattintson a **hosszú távú biztonsági másolatok megőrzésének**, válasszon ki egy adatbázist, és kattintson a **konfigurálása**. 

   ![Válasszon egy adatbázist, a hosszú távú biztonsági másolatok megőrzésének](./media/sql-database-get-started-backup-recovery/select-database-for-long-term-backup-retention.png)

* PowerShell használatával: Nyissa meg túl[és konfigurálása az Azure SQL Database hosszú távú biztonsági mentés megőrzési visszaállítás](sql-database-long-term-backup-retention-configure.md).

## <a name="restore-a-database-thats-stored-with-hello-long-term-backup-retention-feature"></a>Hello hosszú távú biztonsági másolatok megőrzésének szolgáltatásával tárolt adatbázis visszaállítása

hosszú távú biztonsági másolatok megőrzésének másolatból toorecover:

1. Lista hello tároló hello biztonsági mentési tároló.
2. Lista hello-tárolóhoz csatlakoztatott tooyour logikai kiszolgáló.
3. Lista hello adatforráshoz olyan csatlakoztatott tooyour adatbázis hello tárolóból.
4. Lista hello helyreállítási pontok, amelyek a rendelkezésre álló toorestore.
5. Hello adatbázis visszaállítása a célkiszolgálóról hello helyreállítási pont toohello az előfizetésen belül.

tooconfigure, kezeléséhez, és állítsa vissza egy adatbázist egy Azure Recovery Services-tároló az automatikus biztonsági másolatok hosszú távú biztonsági másolatok megőrzésének, hello alábbi módszerekkel:

* Az Azure portál használatával hello: Nyissa meg túl[kezelése hosszú távú biztonsági másolatok megőrzésének használatával hello Azure-portálon](sql-database-long-term-backup-retention-configure.md). 

* PowerShell használatával: Nyissa meg túl[kezelése a PowerShell használatával hosszú távú biztonsági másolatok megőrzésének](sql-database-long-term-backup-retention-configure.md).

## <a name="get-pricing-for-long-term-backup-retention"></a>A hosszú távú biztonsági másolatok megőrzésének Árképzés beolvasása

Hosszú távú biztonsági másolatok megőrzésének SQL-adatbázis szerint toohello díjfizetéssel [díjszabás árképzési Azure biztonsági mentési szolgáltatások](https://azure.microsoft.com/pricing/details/backup/).

Miután hello SQL adatbázis-kiszolgáló regisztrált toohello tárolóban, van szó, a teljes tárterület hello hello heti biztonsági mentései hello tárolóban tárolt által használt.

## <a name="view-available-backups-that-are-stored-in-long-term-backup-retention"></a>Elérhető biztonsági másolatok megtekintése, hosszú távú biztonsági másolatok megőrzésének vannak tárolva

tooconfigure, kezeléséhez, és állítsa vissza egy adatbázist egy Azure Recovery Services-tároló az automatikus biztonsági másolatok hosszú távú biztonsági másolatok megőrzésének hello Azure-portál használatával, hello alábbi módszerekkel:

* Az Azure portál használatával hello: Nyissa meg túl[kezelése hosszú távú biztonsági másolatok megőrzésének használatával hello Azure-portálon](sql-database-long-term-backup-retention-configure.md). 

* PowerShell használatával: Nyissa meg túl[kezelése a PowerShell használatával hosszú távú biztonsági másolatok megőrzésének](sql-database-long-term-backup-retention-configure.md).

## <a name="disable-long-term-retention"></a>Hosszú távú megőrzési letiltása

hello szolgáltatásban automatikusan kezeli a biztonsági mentések adatmegőrzési megadott hello alapján hello karbantartása. 

egy adott adatbázis toohello tároló küldő toostop hello biztonsági mentések adatmegőrzési hello az adatbázishoz tartozó eltávolítása.
  
```
Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy -ResourceGroupName 'RG1' -ServerName 'Server1' -DatabaseName 'DB1' -State 'Disabled' -ResourceId $policy.Id
```

> [!NOTE]
> hello biztonsági mentések, amelyek már hello tárolóban nem érinti. Ezek automatikusan törli hello szolgáltatásban a megőrzési időszak lejártával.

## <a name="long-term-backup-retention-faq"></a>Hosszú távú biztonsági másolatok megőrzésének – gyakori kérdések

**Manuálisan törölhetek egyedi biztonsági másolatok hello tárolóban?**

Jelenleg nem. hello tárolóban automatikusan törli azokat a biztonsági mentések hello megőrzési időszak lejárta.

**Regisztrálhatja a saját kiszolgáló toostore biztonsági mentések toomore mint külön-külön tárolóba?**

Egyszerre nem, biztonsági mentések tooonly külön-külön tárolóba jelenleg tárolhatja.

**Is kell a tároló és a kiszolgáló különböző előfizetésekhez?**

Nem, jelenleg hello tároló és a kiszolgáló kell hello azonos előfizetésbe és erőforráscsoportba csoport.

**Használhatók egy olyan, amely eltér a kiszolgálója régió régióban létrehozott tárolóból?**

Nem, hello tároló és a kiszolgáló kell hello ugyanabban a régióban toominimize idő másolása és forgalom díjak elkerülése érdekében.

**Hány adatbázisokat is tárolnak a külön-külön tárolóba?**

Jelenleg támogatott too1, tároló / 000 adatbázisok fel. 

**Hány tárolók hozhat létre előfizetésenként?**

Előfizetésenként too25 tárolók másolatot hozhat létre.

**Hány adatbázisokat is konfigurálja / nap / tároló?**

200 adatbázisok / nap / tároló állíthat be.

**Hosszú távú biztonsági másolatok megőrzésének rugalmas készletek működik?**

Igen. Minden adatbázis hello készletben hello adatmegőrzési konfigurálható.

**Választhatja a hello idő, amellyel hello biztonsági másolat készítése során?**

Nem, az SQL-adatbázis hello biztonsági mentés ütemezése toominimize hello teljesítményre gyakorolt hatás az adatbázisok szabályozza.

**Átlátható adattitkosítás az adatbázis engedélyezve van. Használhatom az hello tárolóban?** 

Igen, támogatja a transzparens adattitkosítás. Visszaállíthatja hello adatbázis hello tárolóból, még akkor is, ha az eredeti adatbázis hello már nem létezik.

**Mi történik hello biztonsági hello tárolóban, ha az előfizetés fel van függesztve?** 

Ha az előfizetés fel van függesztve, a Microsoft hello meglévő adatbázisok és a biztonsági másolatok megőrzése mellett. Új biztonsági másolatok csak másolt toohello tárolóban. Miután hello előfizetés újraaktiválásához hello szolgáltatás folytatása biztonsági mentések toohello tároló másolása A tároló másolt nincs előtt hello előfizetés fel lett függesztve hello biztonsági mentések használatával elérhető toohello visszaállítási műveletek válik. 

**Kaphatok hozzáférés toohello SQL adatbázis biztonságimásolat-fájlok, letöltése és visszaállíthatják toohello SQL server is?**

Nem, jelenleg nem.

**Ennyi az lehetséges toohave több ütemezi SQL adatmegőrzési belül (napi, heti, havi, évente).**

Nem, több ütemezések jelenleg csak virtuális gépek biztonsági mentéseinek érhető el.

**Mi történik, ha hosszú távú biztonsági másolatok megőrzésének olyan adatbázisban, amely egy aktív georeplikáció másodlagos adatbázis beállítani?**

A replikák biztonsági mentések nem vesszük, mert jelenleg nincs beállítás, a hosszú távú biztonsági másolatok megőrzésének a másodlagos adatbázisok. Azonban fontos felhasználók tooset fel egy aktív georeplikáció másodlagos adatbázison hosszú távú biztonsági másolatok megőrzésének ezen okok miatt:
* Ha feladatátvétel történik, és hello adatbázis elsődleges adatbázis válik, azt egy teljes biztonsági másolatok készítéséhez, amely feltöltött toovault.
* Nincs semmilyen további költség toohello vevő egy másodlagos adatbázison hosszú távú biztonsági másolatok megőrzésének beállításához.

## <a name="next-steps"></a>Következő lépések
Adatbázis biztonsági másolatait véletlen sérülése vagy a törlés adatok védelme, mert nagyon fontos részét képezik az üzletmenet folytonosságának és a vész-helyreállítási stratégiát fontosságúak. toolearn készül más SQL-adatbázis üzleti folytonosságot biztosító megoldások hello című [üzleti folytonosság – áttekintés](sql-database-business-continuity.md).
