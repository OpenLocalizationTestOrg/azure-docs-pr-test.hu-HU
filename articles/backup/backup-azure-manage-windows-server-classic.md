---
title: "aaaManage Azure Backup-tárolók és a kiszolgálók Azure hello klasszikus üzembe helyezési modellel |} Microsoft Docs"
description: "Az oktatóanyag toolearn hogyan toomanage Azure Backup-tárolók és a kiszolgálók használata."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: tysonn
ms.assetid: f175eb12-0905-437f-91fd-eaee03ab6e81
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: markgal;
ms.openlocfilehash: 6c38b04f4a76604bfd639a9b2d58237ce44e2386
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-backup-vaults-and-servers-using-hello-classic-deployment-model"></a>Azure mentési tárolókban és hello klasszikus üzembe helyezési modellel kiszolgálók kezelése
> [!div class="op_single_selector"]
> * [Resource Manager](backup-azure-manage-windows-server.md)
> * [Klasszikus](backup-azure-manage-windows-server-classic.md)
>
>

Ebben a cikkben találhat hello biztonságimásolat-felügyeleti feladatok hello a klasszikus Azure portálon keresztül elérhető és a Microsoft Azure Backup szolgáltatás ügynöke hello áttekintését.

> [!IMPORTANT]
> Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md). Ez a cikk hello klasszikus telepítési modell használatát bemutatja. A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.

> [!IMPORTANT]
> Most már frissítheti a mentési tárolók tooRecovery szolgáltatások tárolókban. További információkért lásd: hello cikk [frissíteni a biztonsági mentési tároló tooa Recovery Services-tároló](backup-azure-upgrade-backup-to-recovery-services.md). A Microsoft tooupgrade támogatja a mentési tárolókat tooRecovery szolgáltatások tárolók.<br/> 2017. október 15. után PowerShell toocreate mentési tárolókban nem használható. **2017. november 1-től**:
>- Az összes többi biztonsági mentési tárolók lesz automatikusan frissített tooRecovery szolgáltatások tárolók.
>- Ön nem fogja tudni tooaccess a biztonsági mentési adatok hello a klasszikus portálon. Ehelyett használjon hello Azure portál tooaccess a biztonsági mentési adatok a Recovery Services-tárolók.
>

## <a name="management-portal-tasks"></a>Kezelési portál feladatok
1. Jelentkezzen be toohello [kezelési portál](https://manage.windowsazure.com).
2. Kattintson a **Recovery Services**, majd kattintson a mentési tároló tooview hello gyors kezdés lapon hello nevére.

    ![Helyreállítási szolgáltatások](./media/backup-azure-manage-windows-server-classic/rs-left-nav.png)

Hello gyors kezdés lapon hello tetején hello lehetőségek kiválasztásával megtekintheti hello elérhető felügyeleti feladatait.

![Lapok kezelése](./media/backup-azure-manage-windows-server-classic/qs-page.png)

### <a name="dashboard"></a>Irányítópult
Válassza ki **irányítópult** toosee hello használatának áttekintése hello kiszolgáló. Hello **használatának áttekintése** tartalmazza:

* a regisztrált toocloud Windows kiszolgálók hello száma
* az Azure virtuális gépek védett felhőben hello száma
* az Azure-ban felhasznált hello teljes tárhely
* hello legutóbbi feladatok állapota

Hello irányítópult hello alján hello a következő feladatokat végezheti el:

* **A tanúsítványnak a** – Ha egy tanúsítvány használt tooregister hello kiszolgáló volt, akkor a tooupdate hello tanúsítvány használatához. Ha a tárolói hitelesítő adatokat használ, ne használjon **szükséges tanúsítvány kezelése**.
* **Törlés** -törlések hello aktuális mentési tároló. A mentési tárolóban már nem használatos, ha törlése toofree tárolási helyet. **Törlés** csak akkor engedélyezett, miután az összes regisztrált kiszolgálókat, hogy törölték a hello tárolójából.

![Biztonsági mentési irányítópult feladatok](./media/backup-azure-manage-windows-server-classic/dashboard-tasks.png)

## <a name="registered-items"></a>Regisztrált cikkek
Válassza ki **regisztrált elemek** tooview hello neveket hello kiszolgálók regisztrálni toothis tárolóban.

![Regisztrált cikkek](./media/backup-azure-manage-windows-server-classic/registered-items.png)

Hello **típus** szűrő alapértelmezés szerint a virtuális gép tooAzure. tooview hello nevek regisztrált toothis tároló hello kiszolgálók kiválasztása **Windows server** hello a legördülő menü.

Itt hello a következő feladatokat végezheti el:

* **Az Újraregisztrálás engedélyezése** – Ha ezt a beállítást is használhatja a kiszolgáló hello **regisztrációs varázsló** hello a helyszíni Microsoft Azure Backup agent tooregister hello Server hello mentési tárolóban még egyszer . Szükség lehet toore-nyilvántartás hello tanúsítványt, vagy ha egy kiszolgáló volt-e az újonnan létrehozott toobe tooan hiba miatt.
* **Törlés** -kiszolgáló törlése hello biztonsági mentési tárolóból. Hello kiszolgálóhoz társított hello tárolt adatok azonnal törli.

    ![Regisztrált konfigurációelemekkel kapcsolatos feladatokhoz](./media/backup-azure-manage-windows-server-classic/registered-items-tasks.png)

## <a name="protected-items"></a>Védett elemek
Válassza ki **védett elemek** tooview hello elemekre mentett hello kiszolgálókról.

![Védett elemek](./media/backup-azure-manage-windows-server-classic/protected-items.png)

## <a name="configure"></a>Konfigurálás
A hello **konfigurálása** lapon hello megfelelő tárolási redundancia lehetőséget. hello idő tooselect hello tárolási redundancia legcélszerűbb egy tároló létrehozása után, és mielőtt a számítógépek regisztrált tooit.

> [!WARNING]
> Egy elem regisztrált toohello tároló követően hello redundancia tárolómegoldást zárolva van, és nem módosítható.
>
>

![Konfigurálás](./media/backup-azure-manage-windows-server-classic/configure.png)

További információ a jelen cikk tudnivalók [adattároló redundanciája, amely](../storage/common/storage-redundancy.md).

## <a name="microsoft-azure-backup-agent-tasks"></a>A Microsoft Azure Backup szolgáltatás ügynöke feladatok
### <a name="console"></a>Konzol
Nyissa meg hello **Microsoft Azure Backup szolgáltatás ügynökének** (a gép kereséssel megtalálja *a Microsoft Azure Backup szolgáltatás*).

![Biztonságimásolat-készítő ügynökkel](./media/backup-azure-manage-windows-server-classic/snap-in-search.png)

A hello **műveletek** hello sarkában hello biztonságimásolat-készítő ügynök konzolon elérhető végezheti el a következő felügyeleti feladatok hello:

* Kiszolgáló regisztrálása
* Biztonsági mentés ütemezése
* Biztonsági másolat készítése
* Tulajdonságainak módosítása

![Ügynök konzol műveletek](./media/backup-azure-manage-windows-server-classic/console-actions.png)

> [!NOTE]
> túl**adatok helyreállítása**, lásd: [fájlok tooa Windows server vagy Windows-ügyfélszámítógép visszaállítása](backup-azure-restore-windows-server.md).
>
>

### <a name="modify-an-existing-backup"></a>Módosítsa a meglévő biztonsági mentésből
1. A Microsoft Azure Backup szolgáltatás ügynökének hello kattintson **biztonsági mentés ütemezése**.

    ![Windows Server biztonsági mentés ütemezése](./media/backup-azure-manage-windows-server-classic/schedule-backup.png)
2. A hello **ütemezett biztonsági mentés varázsló** hello hagyja **módosítása toobackup elemek vagy időpontok szerepelnek** jelölőnégyzetet és kattintson **következő**.

    ![Ütemezett biztonsági mentés módosítása](./media/backup-azure-manage-windows-server-classic/modify-or-stop-a-scheduled-backup.png)
3. Ha szeretné, hogy tooadd, vagy módosítsa az elemeket, hello **elemek kijelölése tooBackup** képernyőn kattintson **elemek hozzáadása**.

    Azt is beállíthatja **kizárások beállításai** hello varázslóban ezen a lapon. Ha azt szeretné, hogy tooexclude fájlok vagy fájltípusok olvasási hozzáadására szolgáló eljárást hello [kizárások beállításai](#exclusion-settings).
4. Válassza ki a hello fájlok és mappák tooback szeretné, hogy fel, és kattintson a **gépházban**.

    ![Elemek hozzáadása](./media/backup-azure-manage-windows-server-classic/add-items-modify.png)
5. Adja meg a hello **biztonsági mentés ütemezése** kattintson **következő**.

    (A 3-szor naponkénti maximum) napi vagy heti biztonsági mentései is ütemezheti.

    ![Adja meg a biztonsági mentés ütemezését](./media/backup-azure-manage-windows-server-classic/specify-backup-schedule-modify-close.png)

   > [!NOTE]
   > Adja meg a hello biztonsági mentés ütemezése esetén, tekintse meg a részletes [cikk](backup-azure-backup-cloud-as-tape.md).
   >
   >
6. Jelölje be hello **adatmegőrzési** hello biztonsági másolatot, majd kattintson **következő**.

    ![Válassza ki az adatmegőrzési házirend](./media/backup-azure-manage-windows-server-classic/select-retention-policy-modify.png)
7. A hello **megerősítő** képernyőn hello információk áttekintése, és kattintson a **Befejezés**.
8. Miután hello varázsló létrehozta a hello **biztonsági mentés ütemezése**, kattintson a **Bezárás**.

    Védelmi módosítása, után ellenőrizheti, hogy biztonsági mentések váltanak ki megfelelően fog toohello által **feladatok** lapra, és erősítse meg, hogy a módosítások megjelennek hello biztonsági mentési feladatok.

### <a name="enable-network-throttling"></a>Hálózati sávszélesség-szabályozás engedélyezése
hello Azure Backup szolgáltatás ügynökének a sávszélesség-szabályozási fülre, amely lehetővé teszi a hálózati sávszélesség használatának adatátvitel során toocontrol biztosít. Ez a vezérlő akkor lehet hasznos, ha tooback adatokat kell munkaidőben, de nem hello biztonsági mentési folyamat toointerfere a más internetes forgalmat. Sávszélesség-szabályozás adatok átvitel mentése tooback vonatkozik, és állítsa vissza a tevékenységek.  

sávszélesség-szabályozás tooenable:

1. A hello **Backup szolgáltatás ügynökének**, kattintson a **tulajdonságainak módosítása**.
2. Jelölje be hello **engedélyezi az internetes sávszélesség szabályozásának a biztonsági mentési műveleteknél** jelölőnégyzetet.

    ![Hálózati sávszélesség-szabályozás](./media/backup-azure-manage-windows-server-classic/throttling-dialog.png)
3. Miután engedélyezte a sávszélesség-szabályozás, adja meg a biztonsági mentési adatátvitel során sávszélesség engedélyezett hello **időpontokat a munkaidőhöz** és **munkaidőn kívüli**.

    hello sávszélesség értékek kezdődjenek 512 kilobájt / másodperc (Kbps), és lépjen be too1023 megabájt / másodperc (Mbps). Is kijelölése hello kezdő és a Befejezés **időpontokat a munkaidőhöz**, és hello hét mely napján minősülnek munkahelyi nap. hello kívül időpontokat a munkaidőhöz kijelölt hello ideje figyelembe vett toobe munkaidőn kívüli órákra.
4. Kattintson az **OK** gombra.

## <a name="exclusion-settings"></a>Kizárások beállításai
1. Nyissa meg hello **Microsoft Azure Backup szolgáltatás ügynökének** (a gép kereséssel megtalálja *a Microsoft Azure Backup szolgáltatás*).

    ![Nyissa meg a biztonságimásolat-készítő ügynökkel](./media/backup-azure-manage-windows-server-classic/snap-in-search.png)
2. A Microsoft Azure Backup szolgáltatás ügynökének hello kattintson **biztonsági mentés ütemezése**.

    ![Windows Server biztonsági mentés ütemezése](./media/backup-azure-manage-windows-server-classic/schedule-backup.png)
3. Az ütemezett biztonsági mentés varázsló hello hagyja hello **módosítása toobackup elemek vagy időpontok szerepelnek** jelölőnégyzetet és kattintson **következő**.

    ![Ütemezésének módosítása](./media/backup-azure-manage-windows-server-classic/modify-or-stop-a-scheduled-backup.png)
4. Kattintson a **kizárások beállításai**.

    ![Válassza ki az elemek tooexclude](./media/backup-azure-manage-windows-server-classic/exclusion-settings.png)
5. Kattintson a **adja hozzá a kizárási**.

    ![Kizárások hozzáadása](./media/backup-azure-manage-windows-server-classic/add-exclusion.png)
6. Adja meg hello helyét, és kattintson a **OK**.

    ![Válassza ki a kizárási helyét](./media/backup-azure-manage-windows-server-classic/exclusion-location.png)
7. Hello fájlkiterjesztés hozzáadása a hello **Fájltípus** mező.

    ![Fájltípus kizárása](./media/backup-azure-manage-windows-server-classic/exclude-file-type.png)

    .Mp3 bővítmény felvétele

    ![Példa a fájl típusa](./media/backup-azure-manage-windows-server-classic/exclude-mp3.png)

    tooadd egy másik kiterjesztést, kattintson a **Kizárás hozzáadása** , és adja meg egy másik típus fájlnévkiterjesztés (.jpeg-bővítmény hozzáadása).

    ![Egy másik példában a fájl típusa](./media/backup-azure-manage-windows-server-classic/exclude-jpg.png)
8. Ha az összes hello bővítmény felvett, kattintson **OK**.
9. Ütemezett biztonsági mentés varázsló hello keresztül kattintva továbbra is **következő** amíg hello **visszaigazolási lapja**, kattintson a **Befejezés**.

    ![Kizárási megerősítése](./media/backup-azure-manage-windows-server-classic/finish-exclusions.png)

## <a name="next-steps"></a>Következő lépések
* [Windows Server vagy a Windows ügyfél visszaállítása az Azure-ból](backup-azure-restore-windows-server.md)
* További információ az Azure Backup szolgáltatásban toolearn lásd [Azure biztonsági mentés áttekintése](backup-introduction-to-azure-backup.md)
* A Microsoft hello [Azure biztonsági mentés fórum](http://go.microsoft.com/fwlink/p/?LinkId=290933)
