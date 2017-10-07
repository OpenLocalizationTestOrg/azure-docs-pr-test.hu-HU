---
title: "egy táblát az Azure SQL Database biztonsági másolatból aaaRestore |} Microsoft Docs"
description: "Ismerje meg, hogyan toorestore egyetlen táblát az Azure SQL Database biztonsági másolatból."
services: sql-database
documentationcenter: 
author: dalechen
manager: cshepard
editor: 
ms.assetid: 340b41bd-9df8-47fb-adfc-03216de38a5e
ms.service: sql-database
ms.custom: business continuity
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: daleche
ms.openlocfilehash: 696d2ac87a70bccdf063bfecb8255723404aa5bd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toorestore-a-single-table-from-an-azure-sql-database-backup"></a>Hogyan toorestore egyetlen tábla egy Azure SQL-adatbázis másolatból
Felmerülhet a helyzetet, amelyben véletlenül módosította az SQL-adatbázisban található egyes adatokat, és most szeretné toorecover hello egyetlen érintett tábla. Ez a cikk ismerteti, hogyan toorestore egyetlen tábla az SQL-adatbázis hello egyik adatbázisban [automatikus biztonsági mentésekhez](sql-database-automated-backups.md).

## <a name="preparation-steps-rename-hello-table-and-restore-a-copy-of-hello-database"></a>Előkészítő lépések: hello táblázat átnevezése és hello adatbázis másolatának visszaállítása
1. Azonosítsa az Azure SQL-adatbázis, amelyet vissza hello példányra tooreplace hello táblájában. Microsoft SQL Management Studio toorename hello táblázattal. Például nevezze át a táblázatban hello &lt;táblanév&gt;_old.
   
   > [!NOTE]
   > tooavoid blokkolja, győződjön meg arról, hogy nem készül átnevezni hello tábla futó tevékenység. Ha hibát tapasztal, győződjön meg arról, hogy ezzel az eljárással a karbantartási időszak alatt.
   >

2. A biztonsági másolat visszaállításával a adatbázis tooa pont idő, amelyet az toorecover toousing hello [pontot-In_Time visszaállítása](sql-database-recovery-using-backups.md#point-in-time-restore) lépéseket.
   
   > [!NOTE]
   > hello visszaállított adatbázis hello neve lesz hello DBName + időbélyeg-formátumnak; például **Adventureworks2012_2016-01-01T22-12Z**. Ez a lépés nem írja felül a meglévő adatbázis nevének hello hello kiszolgálón. Ez egy biztonsági intézkedés, és célja tooallow tooverify hello visszaállított adatbázis ahhoz, hogy az aktuális adatbázis törlése és átnevezése hello visszaállított adatbázis üzemi használatra.
   
## <a name="copying-hello-table-from-hello-restored-database-by-using-hello-sql-database-migration-tool"></a>Hello másolása hello tábla adatbázis visszaállítása hello SQL-adatbázis áttelepítéséhez eszközzel

1. Töltse le és telepítse a hello [SQL-adatbázis áttelepítése varázsló](https://sqlazuremw.codeplex.com).
2. Nyitott hello SQL-adatbázis áttelepítése varázsló, a hello **válasszon folyamat** lapon, válassza ki **elemzési vagy áttelepítése alatt adatbázis**, és kattintson a **tovább**.

   ![SQL-adatbázis áttelepítése varázsló – folyamat kiválasztása](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/1.png)

3. A hello **tooServer csatlakozás** párbeszédpanel mezőbe hello a következő beállítások érvényesek:

   * Kiszolgáló neve: **az SQL server**
   * Hitelesítési: **SQL Server-hitelesítés**
   * Bejelentkezés: **bejelentkezési adatait**
   * Jelszó: **jelszavát**
   * Adatbázis: **Master DB (listában az összes adatbázis)**
   
   > [!NOTE]
   > Alapértelmezés szerint a hello varázslója menti a bejelentkezési adatokat. Ha úgy, hogy nem szeretné, válassza ki a **elfelejti bejelentkezési adatok**.
   >
   
     ![SQL-adatbázis áttelepítése varázsló - forrás kiválasztása - 1.lépés](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/2.png)
4. A hello **forrásának kijelölése** párbeszédpanel megnyitásához, jelölje be hello vissza adatbázis nevét a hello **előkészítő lépések** szakaszt, mint a forrás-, és kattintson a **következő**.
   
    ![SQL-adatbázis áttelepítése varázsló - forrás kiválasztása - lépés 2. régiója](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/3.png)
5. A hello **válasszon objektumok** párbeszédpanel megnyitásához, jelölje be hello **válassza ki az adott adatbázis-objektumok** lehetőséget, majd válassza ki a megjeleníteni kívánt toomigrate toohello célkiszolgáló hello table(or tables).
   ![SQL-adatbázis áttelepítése varázsló – objektumok kiválasztása](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/4.png)
6. A hello **parancsfájl varázsló összefoglalás** kattintson **Igen** amikor felszólítást kap arról, hogy a számítógép készen áll a toogenerate egy SQL-parancsfájlt. Akkor is hello beállítás toosave hello TSQL parancsfájl későbbi használatra.
   ![SQL-adatbázis áttelepítése varázsló - parancsfájl varázsló összefoglalás](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/5.png)
7. A hello **a teszteredményeket összegző** kattintson **következő**.
   ![SQL-adatbázis áttelepítése varázsló - eredmények összefoglalása](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/6.png)
8. A hello **cél kiszolgáló kapcsolat beállítása** lapján kattintson **tooServer csatlakozás**, majd adja meg az alábbiak szerint hello részletek:
   
   * **Kiszolgálónév**: cél server-példány
   * **Hitelesítési**: **SQL Server-hitelesítés**. Adja meg a bejelentkezési hitelesítő adatait.
   * **Adatbázis**: **Master DB (listában az összes adatbázis)**. Ez a beállítás minden hello adatbázisok hello célkiszolgálón sorolja fel.
     
     ![SQL-adatbázis áttelepítése varázsló – cél kiszolgáló kapcsolat beállítása](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/7.png)
9. Kattintson a **Connect**, jelölje be hello céladatbázis szeretné toomove hello táblázatot, és kattintson **következő**. Ez be kell fejeződnie korábban létrehozott hello parancsprogram futtatása, és megtekintheti az hello újonnan áthelyezése a táblázat másolt toohello céladatbázis.

## <a name="verification-step"></a>Ellenőrzési lépés

- Lekérdezés és a vizsgálati hello újonnan másolt tábla toomake arról, hogy hello adatok érintetlenül maradnak. Amint megerősíti, dobja el átnevezett hello táblázatos formában **előkészítő lépések** szakasz. (például &lt;táblanév&gt;_old).

## <a name="next-steps"></a>Következő lépések
[SQL-adatbázis automatikus biztonsági mentés](sql-database-automated-backups.md)

