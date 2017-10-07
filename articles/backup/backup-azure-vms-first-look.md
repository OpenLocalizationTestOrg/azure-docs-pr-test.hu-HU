---
title: "Áttekintés: Azure virtuális gépek biztonsági mentése Backup-tárolóval | Microsoft Docs"
description: "Használja a hello klasszikus portál tooback mentése Azure virtuális gépek tooa Backup-tárolóban. Ez az oktatóanyag azt ismerteti, például hello mentési tároló létrehozása, hello virtuális gépek regisztrálása, biztonsági mentési házirend létrehozása és hello kezdeti biztonsági mentési feladat futtató összes fázisban."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
ms.assetid: 722820dc-b65f-425c-a9e5-c1946e896a87
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/2/2017
ms.author: markgal;
ms.openlocfilehash: 77700e69eab9faccbc7ef923e1eb4e1f97be75d7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="first-look-backing-up-azure-virtual-machines"></a>Áttekintés: Azure virtuális gépek biztonsági mentése
> [!div class="op_single_selector"]
> * [Virtuális gépek védelme Recovery Services-tárolóval](backup-azure-vms-first-look-arm.md)
> * [Azure virtuális gépek védelme Backup-tárolóval](backup-azure-vms-first-look.md)
>
>

Ez az oktatóanyag végigvezeti egy Azure virtuális gép (VM) tooa biztonságimásolat-tárolóban az Azure biztonsági mentéséről hello lépéseket. Ez a cikk ismerteti a klasszikus modellt hello vagy a Service Manager telepítési modell, virtuális gépek biztonsági mentéséről. hello lépések alkalmazása csak hello a klasszikus portálon létrehozott tooBackup tárolók. A Microsoft azt javasolja, hogy az új központi telepítéseknél hello Resource Manager modellt használja.

Ha érdekli VM tooa Recovery Services-tároló tooa erőforráscsoporthoz tartozó biztonsági mentéséről, olvassa el [először: virtuális gépek védelme a recovery services-tároló](backup-azure-vms-first-look-arm.md).

toosuccessfully hello következő végezze el az oktatóanyagban az Előfeltételek léteznie kell:

* Létrehozott egy virtuális gépet az Azure-előfizetésben.
* virtuális gép hello kapcsolat tooAzure nyilvános IP-címmel rendelkezik. További információkért lásd: [Hálózati kapcsolatok](backup-azure-vms-prepare.md#network-connectivity).


> [!NOTE]
> Az Azure két üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md). Ez az oktatóanyag hello a klasszikus portálon létrehozott virtuális gépekkel történő használatra szolgál.
>
>

## <a name="create-a-backup-vault"></a>Backup-tároló létrehozása
A mentési tárolóban olyan entitás, amely hello biztonsági mentések és adott idő alatt létrehozott helyreállítási pontokat tárolja. hello mentési tároló hello biztonsági mentési házirendek, amelyek alkalmazott toohello virtuális gépek biztonsági mentését is tartalmaz.

> [!IMPORTANT]
> 2017. március-től kezdődően már nem használható hello klasszikus portál toocreate mentési tárolókban.
> A mentési tárolók tooRecovery szolgáltatások tárolókban frissítheti. További információkért lásd: hello cikk [frissíteni a biztonsági mentési tároló tooa Recovery Services-tároló](backup-azure-upgrade-backup-to-recovery-services.md). A Microsoft tooupgrade támogatja a mentési tárolókat tooRecovery szolgáltatások tárolók.<br/> 2017. október 15. után PowerShell toocreate mentési tárolókban nem használható. **2017. november 1-től**:
>- Az összes többi biztonsági mentési tárolók lesz automatikusan frissített tooRecovery szolgáltatások tárolók.
>- Ön nem fogja tudni tooaccess a biztonsági mentési adatok hello a klasszikus portálon. Ehelyett használjon hello Azure portál tooaccess a biztonsági mentési adatok a Recovery Services-tárolók.
>

## <a name="discover-and-register-azure-virtual-machines"></a>Az Azure virtuális gépek felderítése és regisztrálása
Mielőtt regisztrálná hello VM egy tárolóban, futtassa hello felderítési folyamat tooidentify bármely új virtuális gépek. Ez a virtuális gépek listáját hello az előfizetést, és további információkat, például a felhőalapú szolgáltatás- és hello régió hello adja vissza.

1. Jelentkezzen be toohello [a klasszikus Azure portálon](http://manage.windowsazure.com/)
2. Hello a klasszikus Azure portálon, kattintson **Recovery Services** Recovery Services-tárolók tooopen hello listája.
    ![Számítási feladat kiválasztása](./media/backup-azure-vms-first-look/recovery-services-icon.png)
3. Tárolók hello listában jelölje ki hello tároló tooback virtuális gépet.

    A tároló kiválasztásakor megnyílik a hello **gyors üzembe helyezés** lap
4. Hello tároló menüben kattintson **regisztrált elemek**.

    ![Számítási feladat kiválasztása](./media/backup-azure-vms-first-look/configure-registered-items.png)
5. A hello **típus** menü **Azure virtuális gép**.

    ![Számítási feladat kiválasztása](./media/backup-azure-vms/discovery-select-workload.png)
6. Kattintson a **felderítési** hello lap hello alján.
    ![Felderítés gomb](./media/backup-azure-vms/discover-button-only.png)

    hello felderítési folyamat eltarthat néhány percig, amíg hello virtuális gépek megjelennének alatt. Nincs üdvözlő képernyőt, amely lehetővé teszi, hogy tudja, hogy fut-e hello folyamat hello alján értesítést.

    ![Virtuális gépek felderítése](./media/backup-azure-vms/discovering-vms.png)

    hello értesítési módosításokat, ha hello folyamat befejeződik.

    ![A felderítés kész](./media/backup-azure-vms-first-look/discovery-complete.png)
7. Kattintson a **REGISZTRÁLÁSA** hello lap hello alján.
    ![Regisztrálás gomb](./media/backup-azure-vms-first-look/register-icon.png)
8. A hello **regisztrálni elemek** helyi menü, jelölje be hello virtuális gépek, amelyet az tooregister.

   > [!TIP]
   > Egyszerre több virtuális gép is regisztrálható.
   >
   >

    Létrejön egy feladat minden egyes kiválasztott virtuális géphez.
9. Kattintson a **feladat megtekintése** a hello értesítési toogo toohello **feladatok** lap.

    ![Regisztrációs feladat](./media/backup-azure-vms/register-create-job.png)

    hello virtuális gép is regisztrált elemek mellett hello regisztrációs művelet hello állapotának hello listájában jelenik meg.

    ![1. regisztrációs állapot](./media/backup-azure-vms/register-status01.png)

    Hello művelet befejeződésekor hello állapota tooreflect hello *regisztrált* állapotát.

    ![2. regisztrációs állapot](./media/backup-azure-vms/register-status02.png)

## <a name="install-hello-vm-agent-on-hello-virtual-machine"></a>Hello Virtuálisgép-ügynök telepítése hello virtuális gépen
hello Azure Virtuálisgép-ügynök hello hello biztonsági mentés bővítmény toowork Azure virtuális gép telepítve kell lennie. Ha a virtuális Gépet az Azure katalógusában hello lett létrehozva, a Virtuálisgép-ügynök hello már szerepel a hello VM; túl kihagyhatja[a virtuális gépek védelmének](backup-azure-vms-first-look.md#create-the-backup-policy).

Ha a virtuális Gépet egy olyan helyszíni adatközpontban átemelt, hello VM valószínűleg nem rendelkezik Virtuálisgép-ügynök telepítve hello. Hello virtuális gépen a Folytatás tooprotect hello VM előtt telepítenie kell hello Virtuálisgép-ügynök. Telepítéséről részletes lépéseket hello Virtuálisgép-ügynök, a következő témakörben: hello [hello biztonsági másolat virtuális gépek cikk Virtuálisgép-ügynök része](backup-azure-vms-prepare.md#vm-agent).

## <a name="create-hello-backup-policy"></a>Hello biztonsági mentési házirend létrehozása
Mielőtt hello kezdeti biztonsági mentési feladatot indít, hello ütemezés beállítása, ha a biztonsági mentési pillanatképet készít a. hello ütemezés biztonsági mentési pillanatképet készít, és mennyi ideig hello ezeket a pillanatképeket a rendszer megőrzi, hello biztonsági mentési házirend. hello megőrzési információ szerzett-édesapja-fia biztonsági mentési rotációs rendszerben alapul.

1. Keresse meg a toohello mentési tároló alatt **Recovery Services** hello a klasszikus Azure portálon, és kattintson a **regisztrált elemek**.
2. Válassza ki **Azure virtuális gép** hello legördülő menüből.

    ![Számítási feladat kiválasztása a portálon](./media/backup-azure-vms/select-workload.png)
3. Kattintson a **védelme** hello lap hello alján.
    ![Kattintson a Védelem gombra](./media/backup-azure-vms-first-look/protect-icon.png)

    Hello **elemek védelme varázsló** vagy látható *csak* regisztrálva és a nem védett virtuális gépek.

    ![Méretezett védelem konfigurálása](./media/backup-azure-vms/protect-at-scale.png)
4. Válassza ki a megjeleníteni kívánt tooprotect hello virtuális gépek.

    Ha hello két vagy több virtuális gép azonos neve, használjon hello Felhőszolgáltatás toodistinguish hello virtuális gépek között.
5. A hello **Védelemkonfigurálás** menüben válassza ki a meglévő házirend, vagy hozzon létre egy új házirend tooprotect azonosított hello virtuális gépek.

    Új mentési tárolókban hello tárolóhoz társított alapértelmezett házirenddel rendelkezhetnek. Ez a házirend egyes este pillanatkép napi, valamint a hello napi pillanatkép 30 napig őrzi meg. Minden biztonsági mentési házirendhez több virtuális gép társítható. Azonban hello virtuális gép csak társítható egy házirend egyszerre.

    ![Védelem új házirenddel](./media/backup-azure-vms/policy-schedule.png)

   > [!NOTE]
   > A biztonsági mentési házirend hello ütemezett biztonsági mentések megőrzési rendszert tartalmaznak. Ha egy meglévő biztonsági mentési házirend, hello következő lépésben fogja nem toomodify hello adatmegőrzési beállítások.
   >
   >
6. A **megőrzési időtartam** határozza meg az adott biztonsági mentési pontok hello napi, heti, havi vagy éves hatókör hello.

    ![A virtuális gép helyreállítási ponttal van mentve](./media/backup-azure-vms/long-term-retention.png)

    Adatmegőrzési idő másolatának tárolására szolgáló hello hosszát adja meg. Az eltérő megőrzési házirendek hello biztonsági mentés időpontjában alapján is megadhat.
7. Kattintson a **feladatok** tooview hello listája **Védelemkonfigurálási** feladatok.

    ![Védelemkonfigurálási feladat](./media/backup-azure-vms/protect-configureprotection.png)

    Most, hogy létrehozta a hello házirend, toohello következő lépést, és hello kezdeti biztonsági mentés futtatására.

## <a name="initial-backup"></a>Kezdeti biztonsági mentés
Ha egy virtuális gép védett, házirend, megtekintheti a hello kapcsolathoz **védett elemek** fülre. Amíg nem hello kezdeti biztonsági másolatot történik, hello **védelmi állapot** jeleníti meg, mint a **védett - (függőben lévő kezdeti biztonsági másolatot)**. Alapértelmezés szerint hello első ütemezett biztonsági mentés hello *kezdeti biztonsági másolatot*.

![Biztonsági mentés függőben](./media/backup-azure-vms-first-look/protection-pending-border.png)

toostart hello kezdeti biztonsági mentés most:

1. A hello **védett elemek** kattintson **biztonsági mentés most** hello lap hello alján.
    ![Biztonsági mentés ikon](./media/backup-azure-vms-first-look/backup-now-icon.png)

    hello Azure Backup szolgáltatás létrehoz egy biztonsági mentési feladatot az hello kezdeti biztonsági mentési műveletet.
2. Kattintson a hello **feladatok** lapon tooview hello feladatok listája.

    ![Biztonsági mentés folyamatban](./media/backup-azure-vms-first-look/protect-inprogress.png)

    Ha a kezdeti biztonsági mentés befejeződött, a hello állapot hello virtuális gép hello **védett elemek** lap *védett*.

    ![A virtuális gép helyreállítási ponttal van mentve](./media/backup-azure-vms/protect-backedupvm.png)

   > [!NOTE]
   > A virtuális gépek biztonsági mentése egy helyi folyamat. Nem készíthet biztonsági másolatot a virtuális gépek az egy régióban tooa biztonsági mentési tárolóból egy másik régióban. Igen minden Azure-régió, amely rendelkezik a virtuális gépek biztonsági mentése toobe igénylő, legalább egy mentési tárolóból, amelyben létre kell hozni az adott régióban.
   >
   >

## <a name="next-steps"></a>Következő lépések
Most, hogy sikeresen készített biztonsági mentést egy virtuális gépről, számos további lépés végezhető. hello legtöbb logikai lépésre toofamiliarize saját magának az adatok tooa virtuális gép visszaállítására. Van azonban feladat, amely segít megérteni hogyan tookeep biztonságos adatait és a költségek minimalizálása érdekében.

* [A virtuális gépek kezelése és figyelése](backup-azure-manage-vms.md)
* [Virtuális gépek visszaállítása](backup-azure-restore-vms.md)
* [Hibaelhárítási útmutató](backup-azure-vms-troubleshoot.md)

## <a name="questions"></a>Kérdései vannak?
Ha kérdése van, vagy ha bármely új szolgáltatása része, toosee szeretné [visszajelzést küldhet](http://aka.ms/azurebackup_feedback).
