---
title: "az Azure Backup Server használatával az SQL Server-munkaterhelések biztonsági mentése aaaAzure |} Microsoft Docs"
description: "Egy bevezető toobacking SQL Server-adatbázisok Azure Backup Server használatával"
services: backup
documentationcenter: 
author: pvrk
manager: Shivamg
editor: 
ms.assetid: c8b1f7ec-26b1-4ef0-a3f2-91aec959daea
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: pullabhk
ms.openlocfilehash: 3a94338e8aca3f9d8611a72bcd223397ffb96f3c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-sql-server-tooazure-with-azure-backup-server"></a>Készítsen biztonsági másolatot az SQL Server tooAzure az Azure Backup Server
Ez a cikk végigvezeti Önt a hello konfigurációs lépések a Microsoft Azure biztonsági mentési Server (MABS) használó SQL Server-adatbázisok biztonsági mentéséhez.

SQL Server-adatbázis biztonsági mentési tooAzure és az Azure-ból helyreállítási hello felügyeleti három lépést foglal magában:

1. Hozzon létre egy biztonsági mentési házirend tooprotect SQL Server adatbázisok tooAzure.
2. Hozzon létre igény szerinti biztonsági másolatok tooAzure.
3. Hello adatbázis helyreállítása az Azure-ból.

## <a name="before-you-start"></a>Előkészületek
Mielőtt elkezdené, győződjön meg arról, hogy [telepítve, és előkészített hello Azure Backup Server](backup-azure-microsoft-azure-backup.md).

## <a name="create-a-backup-policy-tooprotect-sql-server-databases-tooazure"></a>Hozzon létre egy biztonsági mentési házirend tooprotect tooAzure SQL Server-adatbázisok
1. A hello Azure Backup Server felhasználói felületén, kattintson a hello **védelmi** munkaterületen.
2. Hello eszközsávon kattintson **új** toocreate egy új védelmi csoportot.

    ![Védelmi csoport létrehozása](./media/backup-azure-backup-sql/protection-group.png)
3. MABS hello kezdőképernyőn hello útmutatással látható létrehozása egy **védelmi csoport**. Kattintson a **Tovább** gombra.
4. Válassza ki **kiszolgálók**.

    ![Válassza ki a védelmi csoport típusa: "Kiszolgáló"](./media/backup-azure-backup-sql/pg-servers.png)
5. Bontsa ki a ahol jelen-e biztonsági másolat hello adatbázisok toobe hello SQL Server-számítógépen. MABS biztonsági másolat készíthető, a kiszolgáló a különféle adatforrások jeleníti meg. Bontsa ki a hello **minden SQL-megosztás** hello (ebben az esetben azt kijelölt adatbázisok ReportServer$ MSDPM2012 és ReportServer$ MSDPM2012TempDB), majd toobe biztonsági mentése. Kattintson a **Tovább** gombra.

    ![Válassza ki az SQL-adatbázis](./media/backup-azure-backup-sql/pg-databases.png)
6. Adjon nevet a hello védelmi csoporthoz, majd válassza ki a hello **online védelmet szeretnék** jelölőnégyzetet.

    ![Adatvédelmi módszer - rövid távú lemez & Online Azure](./media/backup-azure-backup-sql/pg-name.png)
7. A hello **rövid távú célok megadása** jobb hello szükséges bemeneti adatok toocreate biztonsági mentési pontok toodisk tartalmazza.

    Itt látható, amely **megőrzési időtartam** értéke túl*5 napos*, **szinkronizálási gyakoriság** tooonce beállítása minden *15 perc* hello Ez a gyakoriság, amellyel a biztonsági mentés használatban van. **Expressz teljes biztonsági mentés** értéke túl*8:00 óráig*.

    ![Rövid távú célok](./media/backup-azure-backup-sql/pg-shortterm.png)

   > [!NOTE]
   > 8:00 PM (toohello képernyő bemeneti szerint), egy biztonsági mentési pont hozta létre naponta módosult hello adatokat visz át hello előző napi 8:00 PM biztonsági mentési pont. Ez a folyamat **expressz teljes biztonsági mentés**. Amíg hello tranzakciós naplók szinkronizálódnak 15 percenként, ha nincs szükség toorecover hello adatbázis du. 9:00 –, akkor hello pont hozta létre a hello hello naplók visszajátszását utolsó expressz teljes biztonsági mentési pont (ebben az esetben 8 óra).
   >
   >

8. Kattintson a **Tovább** gombra

    MABS jeleníti meg a teljes tárterülete és hello lehetséges lemezterület-kihasználás hello.

    ![A lemezfoglalás](./media/backup-azure-backup-sql/pg-storage.png)

    Alapértelmezés szerint a MABS egy kötet található adatforrás (SQL Server-adatbázis) használt hello kezdeti biztonsági másolatot hoz létre. Ezzel a megközelítéssel hello logikai lemezkezelő (LDM) korlátozza MABS védelmi too300 adatforrások (SQL Server-adatbázisok). a korlátozás, jelölje be hello toowork **adatok közös elhelyezése a DPM-Tárolókészletben**, lehetőséget. Ha ezt a beállítást használja, MABS használ csak egy kötet több adatforrást, amely lehetővé teszi a MABS tooprotect too2000 SQL-adatbázisok.

    Ha **hello kötetek méretének automatikus növelése** lehetőséget választja, a hello termelési adatok növekedésével fiókot hello növelhető a biztonsági mentési kötet MABS is. Ha **hello kötetek méretének automatikus növelése** beállítás nincs bejelölve, MABS korlátozza hello használt biztonsági mentési tároló toohello adatforrások hello védelmi csoportban.
9. A rendszergazdák vagy hello hálózaton keresztül történő továbbítása a kezdeti biztonsági mentési manuálisan (hálózati) ki tooavoid sávszélesség torlódás hello választott kap. Ezenkívül konfigurálhatják hello idő, mely hello kezdeti átvitel akkor fordulhat elő. Kattintson a **Tovább** gombra.

    ![Kezdeti replikációs módszer](./media/backup-azure-backup-sql/pg-manual.png)

    hello kezdeti biztonsági másolatot az üzemi kiszolgáló (SQL Server-számítógépen) tooMABS hello teljes adatforrás (SQL Server-adatbázis) átvitelét igényli. Lehet, hogy ezek az adatok nagy, és adatok hello hálózaton keresztül való továbbítása hello lépheti túl a sávszélesség. Ezért a rendszergazdák kiválaszthatják tootransfer hello kezdeti biztonsági másolatot: **manuálisan** (cserélhető adathordozóval) tooavoid sávszélesség torlódás esetén vagy **automatikusan hello hálózaton keresztül** (meglétét a megadott (idő).

    Amikor hello kezdeti biztonsági másolat elkészült, a hello rest hello biztonsági mentések hello kezdeti biztonsági másolatot a növekményes biztonsági mentések. Növekményes biztonsági mentések általában toobe kicsi, és egyszerűen továbbítja a hello hálózaton.
10. Adja meg, amikor hello konzisztencia-ellenőrzés toorun, és kattintson a **következő**.

    ![Konzisztencia-ellenőrzés](./media/backup-azure-backup-sql/pg-consistent.png)

    MABS hajthat végre a konzisztencia ellenőrzése toocheck hello sértetlenségének hello biztonsági mentési pontok. Hello ellenőrzőösszeg hello biztonságimásolat-fájl hello az üzemi kiszolgáló (SQL Server-számítógépen ebben a forgatókönyvben) és hello biztonsági másolatot az adott fájlban a következő MABS számítja ki. Az ütközés hello esetben feltételezzük, hogy hello biztonsági másolat fájlban a következő MABS sérültek. MABS hello adatok biztonsági másolatai rectifies toohello ellenőrzőösszeg eltérése megfelelő hello blokkok küldésével. Mivel hello konzisztencia-ellenőrzést a teljesítmény-igényes művelet, a rendszergazdák rendelkezik hello lehetőségével hello konzisztencia-ellenőrzés ütemezése, vagy azt automatikusan futtatja.
11. toospecify online védelemre hello adatforrást, tooAzure, majd kattintson a select hello adatbázisok toobe védett **következő**.

    ![Válassza ki az adatforrások](./media/backup-azure-backup-sql/pg-sqldatabases.png)
12. Rendszergazdák eldönthetik, hogy a mentési ütemezések és az adatmegőrzési, melyek illeszkednek a szervezet házirendjeiket.

    ![Ütemezés és a megőrzési](./media/backup-azure-backup-sql/pg-schedule.png)

    Ebben a példában a biztonsági mentések naponta egyszer kerül 12:00 PM és 8 PM (hello képernyő alsó részén)

    > [!NOTE]
    > Egy jó gyakorlat toohave néhány rövid távú helyreállítási pont a lemezen, a gyors helyreállítás. A helyreállítási pontok "műveleti helyreállítási" érvényesek. Azure magasabb SLA-k a helyes külső helyszíni hely funkcionál, és rendelkezésre állását garantálja.
    >
    >

    **Az ajánlott eljárás**: Győződjön meg arról, hogy Azure biztonsági mentések ütemezett hello helyi lemezes biztonsági mentések a DPM használatának befejezése után. Ez lehetővé teszi a legújabb lemezre másolt biztonsági mentési toobe tooAzure hello.

13. Válassza ki a hello megőrzési házirend-ütemezést. hello adatmegőrzési működéséről hello részletek találhatók a [használata Azure biztonsági mentés tooreplace a szalag infrastruktúra cikk](backup-azure-backup-cloud-as-tape.md).

    ![Adatmegőrzési házirend](./media/backup-azure-backup-sql/pg-retentionschedule.png)

    Ebben a példában:

    * Biztonsági mentések naponta egyszer készít a 12:00 PM és 8 PM (hello képernyő alsó részén), és 180 napig megőrződnek.
    * hello biztonsági mentés szombaton déli 12:00 órakor 104 hétig őrződnek meg
    * hello biztonsági mentés utolsó szombaton déli 12:00 órakor 60 hónapig őrzi meg
    * hello biztonsági mentés utolsó szombaton déli 12:00 órakor március 10 évig őrzi meg
14. Kattintson a **következő** , és válassza ki a megfelelő beállítást hello kezdeti biztonsági másolatot tooAzure átvitelére hello. Választhat **automatikusan hello hálózaton keresztül** vagy **Offline biztonsági másolat**.

    * **Automatikusan hello hálózaton keresztül** átvitelek hello biztonsági mentési adatok tooAzure biztonsági mentésre kiválasztott hello ütemezés szerint.
    * Hogyan **Offline biztonsági másolat** works van arra a [Offline biztonsági másolat munkafolyamat Azure backup](backup-azure-backup-import-export.md).

    Válassza ki azt hello vonatkozó adatátviteli mechanizmus toosend hello kezdeti biztonsági másolatot tooAzure kattintson **következő**.
15. Miután tekintse át a hello hello házirend részleteket **összegzése** képernyőn, kattintson a hello **csoport létrehozása** gomb toocomplete hello munkafolyamat. Kattinthat a hello **Bezárás** gombra és a figyelő hello feladat előrehaladását a figyelés munkaterületen.

    ![Védelmi csoport folyamatban létrehozása](./media/backup-azure-backup-sql/pg-summary.png)

## <a name="on-demand-backup-of-a-sql-server-database"></a>Igény szerinti biztonsági mentés SQL Server-adatbázis
Amíg hello előző lépést a biztonsági mentési házirend létrehozása, a "helyreállítási pont" hello első biztonsági mentés esetén csak jön létre. Ahelyett, hogy a hello Feladatütemező tookick várakozik, hello lépéseket hello eseményindító létrehozása a helyreállítás alatt manuálisan mutasson.

1. Várjon, amíg a hello védelmi csoport állapota látható **OK** hello adatbázis hello helyreállítási pont létrehozása előtt.

    ![Védelmi csoport tagjai](./media/backup-azure-backup-sql/sqlbackup-recoverypoint.png)
2. Kattintson a jobb gombbal a hello adatbázis, és válassza ki **helyreállítási pont létrehozása**.

    ![Online helyreállítási pont létrehozása](./media/backup-azure-backup-sql/sqlbackup-createrp.png)
3. Válasszon **Online védelem** hello legördülő menüre, majd a **OK**. Ekkor elindul a helyreállítási pont létrehozása hello az Azure-ban.

    ![Helyreállítási pont létrehozása](./media/backup-azure-backup-sql/sqlbackup-azure.png)
4. Megtekintheti a hello feladat előrehaladását a hello **figyelés** munkaterület találhatók egy folyamatban lévő feladat például hello egyet az alábbi ábrán hello ábrázolva.

    ![Felügyeleti konzol](./media/backup-azure-backup-sql/sqlbackup-monitoring.png)

## <a name="recover-a-sql-server-database-from-azure"></a>SQL Server-adatbázis helyreállítása az Azure-ból
hello következő lépések végrehajtására szükség toorecover az Azure-ból (SQL Server-adatbázis) védett entitás.

1. Nyissa meg a hello DPM-kiszolgáló kezelési konzolján. Keresse meg a túl**helyreállítási** munkaterületen, ahol láthatja hello kiszolgálók biztonsági mentése a DPM. Keresse meg a hello szükséges adatbázis (Ez esetben ReportServer$ MSDPM2012). Válassza ki a **helyreállítás** amelyek végződik **Online**.

    ![Válassza ki a helyreállítási pont](./media/backup-azure-backup-sql/sqlbackup-restorepoint.png)
2. Kattintson a jobb gombbal a hello adatbázis nevét, és kattintson a **helyreállítása**.

    ![Az Azure-ból helyreállítása](./media/backup-azure-backup-sql/sqlbackup-recover.png)
3. A DPM hello helyreállítási pont hello részleteit jeleníti meg. Kattintson a **Tovább** gombra. toooverwrite hello adatbázis helyreállítási típus kiválasztása hello **helyreállítás toooriginal példány az SQL Server**. Kattintson a **Tovább** gombra.

    ![TooOriginal hely helyreállítása](./media/backup-azure-backup-sql/sqlbackup-recoveroriginal.png)

    Ebben a példában a DPM lehetővé teszi, hogy helyreállítási hello adatbázis tooanother SQL Server-példány vagy tooa különálló hálózati mappába.
4. A hello **adja meg a helyreállítási beállítások** képernyő, a például a hálózati sávszélesség használatának szabályozását helyreállítási által használt toothrottle hello sávszélesség hello helyreállítási lehetőségek közül választhat. Kattintson a **Tovább** gombra.
5. A hello **összegzés** képernyőn látható, hogy minden hello helyreállítási konfigurációk, amennyiben a megadott. Kattintson a **helyreállítása**.

    hello helyreállítási állapot hello adatbázis helyreállítás alatt álló jeleníti meg. Kattinthat **Bezárás** tooclose hello varázsló és a nézet hello folyamat állapotát a hello **figyelés** munkaterületen.

    ![A helyreállítási folyamat elindítása](./media/backup-azure-backup-sql/sqlbackup-recoverying.png)

    Amikor befejeződött a hello helyreállítási, vissza hello adatbázisa konzisztens alkalmazás.

### <a name="next-steps"></a>További lépések:
• [Azure biztonsági mentési – gyakori kérdések](backup-azure-backup-faq.md)
