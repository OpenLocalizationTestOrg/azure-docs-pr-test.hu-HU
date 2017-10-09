---
title: "aaaManage az Azure recovery services-tárolók és a kiszolgálók |} Microsoft Docs"
description: "Az oktatóanyag toolearn hogyan toomanage az Azure recovery services-tárolók és a kiszolgálók használata."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: tysonn
ms.assetid: 4eea984b-7ed6-4600-ac60-99d2e9cb6d8a
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: markgal
ms.openlocfilehash: b4c35c86faa0828b3c63a13b85c095c0cbaba50e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-and-manage-azure-recovery-services-vaults-and-servers-for-windows-machines"></a>Windows-gépek Azure Recovery Services-tárolóinak és -kiszolgálóinak figyelése és kezelése
> [!div class="op_single_selector"]
> * [Resource Manager](backup-azure-manage-windows-server.md)
> * [Klasszikus](backup-azure-manage-windows-server-classic.md)
>
>

Ebben a cikkben találhat hello áttekintést hello Azure portál és hello Microsoft Azure biztonságimásolat-készítő ügynök keresztül elérhető biztonsági mentési figyelő és a felügyeleti feladatokat. Ez a cikk azt feltételezi, hogy már rendelkezik Azure-előfizetéssel, és legalább egy Recovery Services-tároló létrehozása.

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]


## <a name="open-a-recovery-services-vault"></a>Nyissa meg a Recovery Services-tároló

hello Recovery Services-tároló irányítópult látható hello részleteit, illetve a Recovery Services-tároló attribútumait.

1. Jelentkezzen be toohello [Azure Portal](https://portal.azure.com/) használata az Azure-előfizetéshez.
2. Hello központ menüben kattintson a **több szolgáltatások**.

    ![Recovery Services-tárolók 1.lépés listájának megnyitása](./media/backup-azure-manage-windows-server/open-rs-vault-list.png) <br/>

3. Azt szeretné, hogy tooopen Recovery Services-tároló. A hello párbeszédpanelen kezdje el begépelni **Recovery Services**. Írja be megkezdése előtt, hello listát szűrheti a megadott feltételeknek. Kattintson a **Recovery Services-tárolók** toodisplay hello listája Recovery Services-tárolók az előfizetésben.

    ![Recovery Services-tároló létrehozása – 1. lépés](./media/backup-azure-manage-windows-server/browse-to-rs-vaults-2.png) <br/>

    Recovery Services-tárolók hello listáját nyitja meg.

    ![Recovery Services-tároló létrehozása – 1. lépés](./media/backup-azure-manage-windows-server/list-of-rs-vaults.png) <br/>

4. Tárolók hello listában jelölje ki azt szeretné, hogy tooopen Recovery Services-tároló hello hello neve. hello Recovery Services-tároló irányítópult panel nyílik meg.

    ![helyreállítási szolgáltatások tároló irányítópult](./media/backup-azure-manage-windows-server/rs-vault-blade.png) <br/>

    Most, hogy a Recovery Services-tároló hello nyitotta meg, próbálkozzon a hello figyelés vagy a felügyeleti feladatokat.

## <a name="monitor-backup-jobs-and-alerts"></a>A figyelő biztonsági mentési feladatok és riasztások

Feladatok figyelése és hello riasztásait Recovery Services tároló irányítópult, ahol láthatja:

* Biztonsági mentési riasztás részletei
* Fájlok és mappák, valamint a védett hello felhőben Azure virtuális gépeken
* Teljes Azure-ban felhasznált tárterület
* Biztonsági mentési feladat állapota

![Biztonsági mentési irányítópult feladatok](./media/backup-azure-manage-windows-server/dashboard-tiles.png)

Hello információt az egyes ezen csempék kattintva megnyílik a hello társított panel, ahol a kapcsolódó feladatok kezelése.

Az irányítópult hello hello elejéhez:

* Beállítások hozzáférést biztosít az elérhető biztonsági mentési feladatokat.
* Biztonsági mentés - segítségével biztonsági mentést új fájlok és mappák (vagy Azure virtuális gépeken) toohello Recovery Services-tároló.
* Törlés – ha egy helyreállítási szolgáltatások tároló már nem használja, törölheti azt toofree tárolási helyet. Törlése csak akkor engedélyezett, miután az összes védett kiszolgálók, hogy törölték a hello tárolójából.

![Biztonsági mentési irányítópult feladatok](./media/backup-azure-manage-windows-server/dashboard-tasks.png)

## <a name="alerts-for-backups-using-azure-backup-agent"></a>A biztonsági mentéseket az Azure Backup szolgáltatás ügynöke riasztások:
| Riasztási szint | Értesítések küldése |
| --- | --- |
| Kritikus |Sikertelen biztonsági mentéshez, helyreállítási hiba |
| Figyelmeztetés |A biztonsági mentés befejeződött, de figyelmeztetéseket generált (Ha kevesebb mint száz fájlok nem készül biztonsági mentés toocorruption problémák miatt, és több mint egymillió fájlok sikeresen biztonsági mentése) |
| Tájékoztató |None |

## <a name="manage-backup-alerts"></a>Biztonsági riasztások kezelése
Hello kattintson **biztonsági mentési riasztások** csempe tooopen hello **biztonsági mentési riasztások** panel és kezelheti a riasztásokat.

![Biztonsági mentési riasztás](./media/backup-azure-manage-windows-server/manage-backup-alerts.png)

hello biztonsági riasztások csempe megjeleníti hello száma:

* kritikus riasztás feloldva a legutóbbi 24 órában
* az elmúlt 24 órában feloldatlan figyelmeztető riasztások

Kattintson a valamennyi kapcsolatot tart toohello **biztonsági mentési riasztások** szűrt láthassák ezeket a riasztásokat (kritikus vagy figyelmeztetési) panelen.

A hello biztonsági riasztások panel hogy:

* Válassza ki a riasztások hello megfelelő adatokat tooinclude.

    ![Oszlopok kiválasztása](./media/backup-azure-manage-windows-server/choose-alerts-colunms.png)
* Riasztások szűrése a súlyosság, állapota és a kezdő és záró időpont.

    ![Riasztások szűrése](./media/backup-azure-manage-windows-server/filter-alerts.png)
* Súlyosság, a gyakoriság és a címzettek értesítések konfigurálása, valamint figyelmeztetések engedélyezése vagy letiltása.

    ![Riasztások szűrése](./media/backup-azure-manage-windows-server/configure-notifications.png)

Ha **/ riasztási** hello választotta **értesítendő** gyakoriság nincs csoportosítás vagy e-mailek csökkenése történik. Minden egyes riasztás 1 értesítési eredményez. Ez az alapértelmezett beállítás hello és hello feloldási e-mailt is küld azonnal.

Ha **óránkénti kivonatoló** hello választotta **értesítendő** egy e-mailt küld toohello felhasználói közölve, hogy van a gyakoriság feloldatlan új riasztások jönnek létre hello az elmúlt egy óra. A feloldási e-mail által kiküldött hello óra hello végén.

Is elküldi a riasztásokat a következő súlyossági szintek hello:

* Kritikus
* Figyelmeztetés
* Információ

Hello hello riasztás inaktiválja **inaktiválása** hello feladat részletei panel gombjára. Kattintva inaktiválása, megadhatja a feloldási megjegyzések.

Hello oszlopok kiválasztása tooappear kívánt hello hello riasztás részeként **oszlopok kiválasztása** gombra.

> [!NOTE]
> A hello **beállítások** panelen, kezelheti a biztonsági mentési riasztás kiválasztásával **figyelés és jelentéskészítés > riasztások és események > biztonsági mentésekkel kapcsolatos riasztások** , majd kattintson **szűrő** vagy  **Értesítések konfigurálása**.
>
>

## <a name="manage-backup-items"></a>Biztonsági mentés elemek kezelése
A helyi biztonsági kezelése mostantól elérhető hello felügyeleti portálon. Hello irányítópult hello biztonsági mentés területen hello **biztonsági másolati elemei** csempe megjeleníti hello biztonsági mentési elemszáma védett toohello tárolóban.

Kattintson a **-mappákban** a biztonsági mentés elemek csempe hello.

![Biztonsági másolati elemei csempe](./media/backup-azure-manage-windows-server/backup-items-tile.png)

hello biztonsági másolati elemei panel nyílik meg hello szűrése minden felsorolt elem biztonsági másolat megtapasztalhatja tooFile-mappa.

![Biztonsági másolati elemei](./media/backup-azure-manage-windows-server/backup-item-list.png)

Ha egy biztonsági mentési cikket hello listából válassza ki, hello alapvető adatait, hogy az elem látható.

> [!NOTE]
> A hello **beállítások** panelen, kezelheti a fájlok és mappák kiválasztásával **védett elemek > biztonsági mentés elemek** és jelölje be **-mappákban** hello a legördülő menü.
>
>

![A beállítások biztonsági másolati elemei](./media/backup-azure-manage-windows-server/backup-files-and-folders.png)

## <a name="manage-backup-jobs"></a>Biztonsági mentési feladatok kezelése
Mind a helyszíni (ha hello helyszíni kiszolgáló biztonsági másolatot készít a tooAzure), és az Azure biztonsági mentések tartozó biztonsági mentési feladatok hello irányítópult láthatók.

A biztonsági mentés szakasz hello irányítópult hello hello biztonsági mentési feladat csempe feladatok hello számát jeleníti meg:

* folyamatban van
* nem sikerült hello utolsó 24 órában.

toomanage a biztonsági mentési feladatok kattintson hello **biztonsági mentési feladatok** csempe, amely hello biztonsági mentési feladatok paneljének megnyitása.

![A beállítások biztonsági másolati elemei](./media/backup-azure-manage-windows-server/backup-jobs.png)

Hello információk érhetők el a biztonsági mentési feladatok panelről hello hello módosítása **oszlopok kiválasztása** hello oldal hello tetején gombra.

Használjon hello **szűrő** gomb tooselect fájlok és mappák és az Azure virtuális gép biztonsági mentése között.

Ha nem látja a biztonsági mentés fájlokat és mappákat, kattintson a **szűrő** hello lap, és válassza ki a hello tetején gomb **fájlok és mappák** hello elemtípus menüből.

> [!NOTE]
> A hello **beállítások** panelen kiválasztásával kezelheti a biztonsági mentési feladatok **figyelés és jelentéskészítés > feladatok > biztonsági mentési feladatok** és jelölje be **-mappákban** hello legördülő menüből menüre.
>
>

## <a name="monitor-backup-usage"></a>Biztonsági másolat használatának figyelése
A biztonsági mentés szakasz hello irányítópult hello hello biztonsági mentés használata csempe az Azure-ban felhasznált hello tárhely jeleníti meg. Tárhelyhasználatot biztosított:

* A felhőalapú LRS storage használata hello tárolóhoz társított
* A felhőalapú Georedundáns tárolás használata hello tárolóhoz társított

## <a name="manage-your-production-servers"></a>Az üzemi kiszolgálók kezelése
toomanage az üzemi kiszolgálók kattintson **beállítások**.

Kattintson a kezelés **biztonsági infrastruktúra > az üzemi kiszolgálók**.

hello az üzemi kiszolgálók panel listák az összes rendelkezésre álló üzemi kiszolgáló. Kattintson az adott kiszolgálón az hello lista tooopen hello kiszolgáló adatait.

![Védett elemek](./media/backup-azure-manage-windows-server/production-server-list.png)


## <a name="open-hello-azure-backup-agent"></a>Nyissa meg hello Azure Backup szolgáltatás ügynöke
Nyissa meg hello **Microsoft Azure Backup szolgáltatás ügynökének** (rákeresve a gépen található *a Microsoft Azure Backup szolgáltatás*).

![Windows Server biztonsági mentés ütemezése](./media/backup-azure-manage-windows-server/snap-in-search.png)

A hello **műveletek** hello sarkában hello biztonságimásolat-készítő ügynök konzolon elérhető hajt végre a következő felügyeleti feladatok hello:

* Kiszolgáló regisztrálása
* Biztonsági mentés ütemezése
* Biztonsági másolat készítése
* Tulajdonságainak módosítása

![A Microsoft Azure Backup szolgáltatás ügynöke konzol műveletek](./media/backup-azure-manage-windows-server/console-actions.png)

> [!NOTE]
> túl**adatok helyreállítása**, lásd: [fájlok tooa Windows server vagy Windows-ügyfélszámítógép visszaállítása](backup-azure-restore-windows-server.md).
>
>

## <a name="modify-hello-backup-schedule"></a>Hello biztonsági mentési ütemezés módosítása
1. A Microsoft Azure Backup szolgáltatás ügynökének hello kattintson **biztonsági mentés ütemezése**.

    ![Windows Server biztonsági mentés ütemezése](./media/backup-azure-manage-windows-server/schedule-backup.png)
2. A hello **ütemezett biztonsági mentés varázsló** hello hagyja **módosítása toobackup elemek vagy időpontok szerepelnek** jelölőnégyzetet és kattintson **következő**.

    ![Windows Server biztonsági mentés ütemezése](./media/backup-azure-manage-windows-server/modify-or-stop-a-scheduled-backup.png)
3. Ha szeretné, hogy tooadd, vagy módosítsa az elemeket, hello **elemek kijelölése tooBackup** képernyőn kattintson **elemek hozzáadása**.

    Azt is beállíthatja **kizárások beállításai** hello varázslóban ezen a lapon. Ha azt szeretné, hogy tooexclude fájlok vagy fájltípusok olvasási hozzáadására szolgáló eljárást hello [kizárások beállításai](#manage-exclusion-settings).
4. Válassza ki a hello fájlok és mappák tooback szeretné, hogy fel, és kattintson a **gépházban**.

    ![Windows Server biztonsági mentés ütemezése](./media/backup-azure-manage-windows-server/add-items-modify.png)
5. Adja meg a hello **biztonsági mentés ütemezése** kattintson **következő**.

    (A 3-szor naponkénti maximum) napi vagy heti biztonsági mentései is ütemezheti.

    ![Windows Server biztonsági mentés elemei](./media/backup-azure-manage-windows-server/specify-backup-schedule-modify-close.png)

   > [!NOTE]
   > Adja meg a hello biztonsági mentés ütemezése esetén, tekintse meg a részletes [cikk](backup-azure-backup-cloud-as-tape.md).
   >

6. Jelölje be hello **adatmegőrzési** hello biztonsági másolatot, majd kattintson **következő**.

    ![Windows Server biztonsági mentés elemei](./media/backup-azure-manage-windows-server/select-retention-policy-modify.png)
7. A hello **megerősítő** képernyőn hello információk áttekintése, és kattintson a **Befejezés**.
8. Miután hello varázsló létrehozta a hello **biztonsági mentés ütemezése**, kattintson a **Bezárás**.

    Védelmi módosítása, után ellenőrizheti, hogy biztonsági mentések váltanak ki megfelelően fog toohello által **feladatok** lapra, és erősítse meg, hogy a módosítások megjelennek hello biztonsági mentési feladatok.

## <a name="enable-network-throttling"></a>Hálózati sávszélesség-szabályozás engedélyezése

hello Azure Backup szolgáltatás ügynökének a sávszélesség-szabályozási fülre, amely lehetővé teszi a hálózati sávszélesség használatának adatátvitel során toocontrol biztosít. Ez a vezérlő akkor lehet hasznos, ha tooback adatokat kell munkaidőben, de nem hello biztonsági mentési folyamat toointerfere a más internetes forgalmat. Sávszélesség-szabályozás adatok átvitel mentése tooback vonatkozik, és állítsa vissza a tevékenységek.  

sávszélesség-szabályozás tooenable:

1. A hello **Backup szolgáltatás ügynökének**, kattintson a **tulajdonságainak módosítása**.
2. A hello ** szabályozás lapra, válassza ki **engedélyezi az internetes sávszélesség szabályozásának a biztonsági mentési műveleteknél**.

    ![Hálózati sávszélesség-szabályozás](./media/backup-azure-manage-windows-server/throttling-dialog.png)

    Miután engedélyezte a sávszélesség-szabályozás, adja meg a biztonsági mentési adatátvitel során sávszélesség engedélyezett hello **időpontokat a munkaidőhöz** és **munkaidőn kívüli**.

    hello sávszélesség értékek kezdődjenek 512 kilobájt / másodperc (Kbps), és lépjen be too1023 megabájt / másodperc (Mbps). Is kijelölése hello kezdő és a Befejezés **időpontokat a munkaidőhöz**, és hello hét mely napján minősülnek munkahelyi nap. hello kívül időpontokat a munkaidőhöz kijelölt hello ideje figyelembe vett toobe munkaidőn kívüli órákra.
3. Kattintson az **OK** gombra.

## <a name="manage-exclusion-settings"></a>Kizárási beállításainak kezelése
1. Nyissa meg hello **Microsoft Azure Backup szolgáltatás ügynökének** (a gép kereséssel megtalálja *a Microsoft Azure Backup szolgáltatás*).

    ![Windows Server biztonsági mentés ütemezése](./media/backup-azure-manage-windows-server/snap-in-search.png)
2. A Microsoft Azure Backup szolgáltatás ügynökének hello kattintson **biztonsági mentés ütemezése**.

    ![Windows Server biztonsági mentés ütemezése](./media/backup-azure-manage-windows-server/schedule-backup.png)
3. Az ütemezett biztonsági mentés varázsló hello hagyja hello **módosítása toobackup elemek vagy időpontok szerepelnek** jelölőnégyzetet és kattintson **következő**.

    ![Windows Server biztonsági mentés ütemezése](./media/backup-azure-manage-windows-server/modify-or-stop-a-scheduled-backup.png)
4. Kattintson a **kizárások beállításai**.

    ![Windows Server biztonsági mentés ütemezése](./media/backup-azure-manage-windows-server/exclusion-settings.png)
5. Kattintson a **adja hozzá a kizárási**.

    ![Windows Server biztonsági mentés ütemezése](./media/backup-azure-manage-windows-server/add-exclusion.png)
6. Adja meg hello helyét, és kattintson a **OK**.

    ![Windows Server biztonsági mentés ütemezése](./media/backup-azure-manage-windows-server/exclusion-location.png)
7. Hello fájlkiterjesztés hozzáadása a hello **Fájltípus** mező.

    ![Windows Server biztonsági mentés ütemezése](./media/backup-azure-manage-windows-server/exclude-file-type.png)

    .Mp3 bővítmény felvétele

    ![Windows Server biztonsági mentés ütemezése](./media/backup-azure-manage-windows-server/exclude-mp3.png)

    tooadd egy másik kiterjesztést, kattintson a **Kizárás hozzáadása** , és adja meg egy másik típus fájlnévkiterjesztés (.jpeg-bővítmény hozzáadása).

    ![Windows Server biztonsági mentés ütemezése](./media/backup-azure-manage-windows-server/exclude-jpg.png)
8. Ha az összes hello bővítmény felvett, kattintson **OK**.
9. Ütemezett biztonsági mentés varázsló hello keresztül kattintva továbbra is **következő** amíg hello **visszaigazolási lapja**, kattintson a **Befejezés**.

    ![Windows Server biztonsági mentés ütemezése](./media/backup-azure-manage-windows-server/finish-exclusions.png)

## <a name="frequently-asked-questions"></a>Gyakori kérdések
**1. KÉRDÉS. hello biztonsági mentési feladat állapotüzenet látható módon fejezte be hello Azure Backup szolgáltatás ügynöke, miért nem az beszerzése kerülnek azonnal portálon?**

1. válasz Nincs maximális késleltetés 15 perc közötti hello biztonsági mentési feladat állapotát a következő tükrözi hello Azure Backup szolgáltatás ügynöke és hello Azure-portálon.

**Q.2, amikor egy biztonsági mentési feladat sikertelen lesz, hogy mennyi ideig tart tooraise riasztást?**

Riasztás A.2 hello Azure sikertelen biztonsági mentéshez, 20 perc belül következik be.

**3. NEGYEDÉVÉBEN. Ha egy e-mailt nem küldi el, ha értesítések beállítása eset van?**

3. válasz Amikor hello értesítés nem lesz elküldve a rendelés tooreduce hello riasztási zaj az alábbiakban hello esetekben:

* Ha az értesítések beállítása óránkénti, és az következik be van-e és hello órán belül megoldott riasztás
* Feladat megszakadt.
* Második biztonsági mentési feladat sikertelen volt, mert az eredeti biztonsági mentési feladat van folyamatban.

## <a name="troubleshooting-monitoring-issues"></a>Figyelési problémák elhárítása
**Probléma:** feladatok és/vagy hello Azure Backup szolgáltatás ügynökének származó riasztások nem jelennek meg hello portálon.

**Hibaelhárítási lépések:** folyamat hello ```OBRecoveryServicesManagementAgent```, küld hello feladat és riasztás adatok toohello Azure Backup szolgáltatás. Ez a folyamat állapottal alkalmanként vagy -leállítás.

1. tooverify hello folyamat nem fut, nyissa meg a **Feladatkezelő** , és ellenőrizze, hogy hello ```OBRecoveryServicesManagementAgent``` folyamat fut.
2. Feltételezve, hogy hello folyamat nem fut, nyissa meg a **Vezérlőpult** , és keresse meg a szolgáltatás hello listája. Indítsa el, vagy indítsa újra a **Microsoft Azure Recovery Services Management Agent**.

    További információért keresse meg a hello naplózásra kerül:<br/>
   `<AzureBackup_agent_install_folder>\Microsoft Azure Recovery Services Agent\Temp\GatewayProvider*`Példa:<br/>
   `C:\Program Files\Microsoft Azure Recovery Services Agent\Temp\GatewayProvider0.errlog`

## <a name="next-steps"></a>Következő lépések
* [Windows Server vagy a Windows ügyfél visszaállítása az Azure-ból](backup-azure-restore-windows-server.md)
* További információ az Azure Backup szolgáltatásban toolearn lásd [Azure biztonsági mentés áttekintése](backup-introduction-to-azure-backup.md)
* A Microsoft hello [Azure biztonsági mentés fórum](http://go.microsoft.com/fwlink/p/?LinkId=290933)
