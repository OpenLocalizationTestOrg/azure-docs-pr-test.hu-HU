---
title: "mentése az Azure virtual machines klasszikus telepített tooa mentési tároló aaaBack |} Microsoft Docs"
description: "Ismerje meg, és regisztrálja, készítsen biztonsági másolatot a virtuális gépek ezekkel az eljárásokkal az Azure virtuális gépek biztonsági mentéséhez."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: "virtuális gép biztonsági mentése; Készítsen biztonsági másolatot a virtuális gép; biztonsági mentés és katasztrófa utáni helyreállítás; virtuális gép biztonsági mentése"
ms.assetid: c0ab5469-65fd-4af5-ae9b-f5d183f82ce8
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: markgal;trinadhk;
ms.openlocfilehash: 048e32d9b2bd5bdd7a125225a71a6d805bb4fbd4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-azure-virtual-machines-classic-portal"></a>Készítsen biztonsági másolatot az Azure virtuális gépek (klasszikus portál)
> [!div class="op_single_selector"]
> * [Készítsen biztonsági másolatot a virtuális gépek tooRecovery Services-tároló](backup-azure-arm-vms.md)
> * [Készítsen biztonsági másolatot a virtuális gépek tooBackup tároló](backup-azure-vms.md)
>
>

Ez a cikk hello eljárásokat biztosít biztonsági mentése egy klasszikus telepített Azure virtuális gép (VM) tooa Backup-tárolóban. Nincsenek tootake care, mielőtt készíthet biztonsági másolatot egy Azure virtuális gépen kell néhány feladatot. Ha még nem tette Igen, a teljes hello [Előfeltételek](backup-azure-vms-prepare.md) tooprepare környezetében a virtuális gépek biztonsági mentéséről.

További információkért lásd: hello cikkeket a [az Azure virtuális gép biztonsági mentési infrastruktúrájának megtervezésével](backup-azure-vms-introduction.md) és [Azure virtuális gépek](https://azure.microsoft.com/documentation/services/virtual-machines/).

> [!NOTE]
> Az Azure két üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md). A biztonsági másolatok tárolóját csak klasszikus telepített virtuális gépek védelmére. A biztonsági másolatok tárolóját erőforrás-kezelő telepített virtuális gépek nem védhetők. Lásd: [tooRecovery Services-tároló virtuális gépek biztonsági mentése](backup-azure-arm-vms.md) talál részletes információt használata a Recovery Services-tárolók.
>
>

Az Azure virtuális gépek biztonsági mentését három fő lépésből áll:

![Három lépést tooback egy Azure IaaS virtuális gépet](./media/backup-azure-vms/3-steps-for-backup.png)

> [!NOTE]
> A virtuális gépek biztonsági mentése egy helyi folyamat. Nem készíthet biztonsági másolatot a virtuális gépek egy régió tooa biztonságimásolat-tárolóban egy másik régióban. Ezért létre kell hozni egy mentési tárolót minden Azure-régió, ahol a virtuális gép készül biztonsági másolat.
>
> [!IMPORTANT]
> 2017. március-től kezdődően már nem használható hello klasszikus portál toocreate mentési tárolókban.
> Most már frissítheti a mentési tárolók tooRecovery szolgáltatások tárolókban. További információkért lásd: hello cikk [frissíteni a biztonsági mentési tároló tooa Recovery Services-tároló](backup-azure-upgrade-backup-to-recovery-services.md). A Microsoft tooupgrade támogatja a mentési tárolókat tooRecovery szolgáltatások tárolók.<br/> 2017. október 15. után PowerShell toocreate mentési tárolókban nem használható. **2017. november 1-től**:
>- Az összes többi biztonsági mentési tárolók lesz automatikusan frissített tooRecovery szolgáltatások tárolók.
>- Ön nem fogja tudni tooaccess a biztonsági mentési adatok hello a klasszikus portálon. Ehelyett használjon hello Azure portál tooaccess a biztonsági mentési adatok a Recovery Services-tárolók.
>

## <a name="step-1---discover-azure-virtual-machines"></a>1. lépés – az Azure virtuális gépek észlelése
bármely új virtuális gépek (VM) hozzáadott toohello előfizetés azonosítják, mielőtt regisztrálná, tooensure hello felderítési folyamatának futtatása során. hello folyamat lekérdezések Azure hello hello az előfizetést, valamint további információkat a virtuális gépek listája például a felhőalapú szolgáltatás- és hello régió hello.

1. Jelentkezzen be toohello [klasszikus portál](http://manage.windowsazure.com/)
2. Azure-szolgáltatások hello listájában kattintson **Recovery Services** tárolók biztonsági mentés és helyreállítás tooopen hello listája.
    ![Nyissa meg a tároló listája](./media/backup-azure-vms/choose-vault-list.png)
3. Hello mentési tárolókban, jelölje ki hello tároló tooback virtuális gépet.

    Ha ez egy új tároló hello portál megnyitása toohello **gyors üzembe helyezés** lap.

    ![Regisztrált elemek menü megnyitása](./media/backup-azure-vms/vault-quick-start.png)

    Ha hello tároló már be lett állítva, a hello portál legutóbb használt toohello menü megnyitása.
4. (Hello hello oldal tetején) hello tároló menüben kattintson **regisztrált elemek**.

    ![Regisztrált elemek menü megnyitása](./media/backup-azure-vms/vault-menu.png)
5. A hello **típus** menü **Azure virtuális gép**.

    ![Számítási feladat kiválasztása](./media/backup-azure-vms/discovery-select-workload.png)
6. Kattintson a **felderítési** hello lap hello alján.
    ![Felderítés gomb](./media/backup-azure-vms/discover-button-only.png)

    hello felderítési folyamat eltarthat néhány percig, amíg hello virtuális gépek megjelennének alatt. Nincs üdvözlő képernyőt, amely lehetővé teszi, hogy tudja, hogy fut-e hello folyamat hello alján értesítést.

    ![Virtuális gépek felderítése](./media/backup-azure-vms/discovering-vms.png)

    hello értesítési módosításokat, ha hello folyamat befejeződik. Ha hello felderítési folyamat nem találta meg a virtuális gépek hello, először biztosítson hello virtuális gépeket. Hello virtuális gépek esetén győződjön meg arról, hello virtuális gépek vannak a hello azonos régióban legyen, mint a hello mentési tároló. Ha hello virtuális gépek és azok a hello ugyanabban a régióban, győződjön meg arról, hello virtuális gépek még nem regisztrált tooa mentési tároló. Ha a virtuális gép hozzárendelt tooa mentési tároló már nem érhető el toobe hozzárendelt tooother mentési tárolók.

    ![A felderítés kész](./media/backup-azure-vms/discovery-complete.png)

    Miután a felfedezett hello új elemek tooStep 2 lépjen, és regisztrálja a virtuális gépek.

## <a name="step-2---register-azure-virtual-machines"></a>2. lépés – regisztrálása az Azure virtuális gépek
Egy Azure virtuális gép tooassociate regisztrálja azt hello Azure Backup szolgáltatás. Ez általában az egyszeri tevékenység.

1. Keresse meg a toohello mentési tároló alatt **Recovery Services** hello Azure-portálon, és kattintson a **regisztrált elemek**.
2. Válassza ki **Azure virtuális gép** hello legördülő menüből.

    ![Számítási feladat kiválasztása](./media/backup-azure-vms/discovery-select-workload.png)
3. Kattintson a **REGISZTRÁLÁSA** hello lap hello alján.
    ![Regisztrálás gomb](./media/backup-azure-vms/register-button-only.png)
4. A hello **regisztrálni elemek** helyi menü, jelölje be hello virtuális gépek, amelyet az tooregister. Ha a két vagy több virtuális gép hello azonos hello cloud service toodistinguish közöttük használja.

   > [!TIP]
   > Egyszerre több virtuális gép is regisztrálható.
   >
   >

    Létrejön egy feladat minden egyes kiválasztott virtuális géphez.
5. Kattintson a **feladat megtekintése** a hello értesítési toogo toohello **feladatok** lap.

    ![Regisztrációs feladat](./media/backup-azure-vms/register-create-job.png)

    hello virtuális gép is regisztrált elemek mellett hello regisztrációs művelet hello állapotának hello listájában jelenik meg.

    ![1. regisztrációs állapot](./media/backup-azure-vms/register-status01.png)

    Hello művelet befejeződésekor hello állapota tooreflect hello *regisztrált* állapotát.

    ![2. regisztrációs állapot](./media/backup-azure-vms/register-status02.png)

## <a name="step-3---protect-azure-virtual-machines"></a>3. lépés - az Azure virtuális gépek védelme
Most állíthat be egy biztonsági mentési és adatmegőrzési házirend hello virtuális géphez. Több virtuális gépek védhetők egyetlen művelet védelme.

Az Azure mentési tárolókban 2015. május kapható után létrehozott alapértelmezett házirend hello tároló beépített. Ez az alapértelmezett házirend tartalmaz egy alapértelmezett megőrzési 30 nap és a biztonsági mentési ütemezés szerint naponta egyszer.

1. Keresse meg a toohello mentési tároló alatt **Recovery Services** hello Azure-portálon, és kattintson a **regisztrált elemek**.
2. Válassza ki **Azure virtuális gép** hello legördülő menüből.

    ![Számítási feladat kiválasztása a portálon](./media/backup-azure-vms/select-workload.png)
3. Kattintson a **védelme** hello lap hello alján.

    Hello **elemek védelme varázsló** jelenik meg. hello varázsló csak a regisztrált és nem védett virtuális gépek sorolja fel. Válassza ki a megjeleníteni kívánt tooprotect hello virtuális gépek.

    Ha a két vagy több virtuális gép hello azonos hello cloud service toodistinguish hello virtuális gépek közötti használja.

   > [!TIP]
   > Egyszerre több virtuális gép védhet.
   >
   >

    ![Méretezett védelem konfigurálása](./media/backup-azure-vms/protect-at-scale.png)

4. Válasszon egy **biztonsági mentés ütemezése** tooback kijelölt hello virtuális gépek. Válasszon egy meglévő házirendcsoport címet, vagy új megadása.

    Minden biztonsági mentési házirendhez több virtuális gép társítható. Azonban hello virtuális gép csak társítható egy házirend álljon időben.

    ![Védelem új házirenddel](./media/backup-azure-vms/policy-schedule.png)

   > [!NOTE]
   > A biztonsági mentési házirend hello ütemezett biztonsági mentések megőrzési rendszert tartalmaznak. Ha egy meglévő biztonsági mentési házirend, hello adatmegőrzési beállítások hello következő lépésben nem módosíthatja.
   >
   >

5. Válasszon egy **megőrzési időtartam** hello biztonsági tooassociate.

    ![Rugalmas megőrzést védelme](./media/backup-azure-vms/policy-retention.png)

    Adatmegőrzési idő másolatának tárolására szolgáló hello hosszát adja meg. Az eltérő megőrzési házirendek hello biztonsági mentés időpontjában alapján is megadhat. Például egy biztonsági mentési pont végrehajtott naponta (ami működési helyreállítási pontjaként szolgál) lehet, hogy megőrzi a 90 napig. Szemben egy biztonsági mentési pont (naplózási célokra) negyedévenként hello végén végrehajtott esetleg toobe sok hónapokban vagy években maradnak.

    ![A virtuális gép helyreállítási ponttal van mentve](./media/backup-azure-vms/long-term-retention.png)

    A példa képen:

   * **Napi adatmegőrzési**: naponta készített biztonsági másolatok 30 napig tárolja.
   * **Heti adatmegőrzési**: 104 hétig minden héten vasárnap készített biztonsági másolatok megmaradnak.
   * **Havi adatmegőrzési**: 120 havi biztonsági másolatokat hello minden hónap utolsó vasárnap megmaradnak.
   * **Éves adatmegőrzési**: biztonsági másolatokat hello minden január első vasárnap 99 évig megmaradnak.

     Egy feladat jön létre tooconfigure hello védelmi házirendje, és társítsa hello virtuális gépek toothat házirendet, amely a kijelölt virtuális gépek.
6. tooview hello listája **Védelemkonfigurálási** feladatok hello tárolók menüben kattintson **feladatok** válassza **Védelemkonfigurálási** a hello **művelet**  szűrő.

    ![Védelemkonfigurálási feladat](./media/backup-azure-vms/protect-configureprotection.png)

## <a name="initial-backup"></a>Kezdeti biztonsági mentés
Miután hello virtuális gép védett házirendnek, azt jeleníti meg a hello **védett elemek** hello az állapota lapon *védett - (függőben lévő kezdeti biztonsági másolatot)*. Alapértelmezés szerint hello első ütemezett biztonsági mentés hello *kezdeti biztonsági másolatot*.

tootrigger hello kezdeti biztonsági mentés közvetlenül a védelem beállítása után:

1. Hello hello alján **védett elemek** kattintson **biztonsági mentés most**.

    hello Azure Backup szolgáltatás létrehoz egy biztonsági mentési feladatot az hello kezdeti biztonsági mentési műveletet.
2. Kattintson a hello **feladatok** lapon tooview hello feladatok listája.

    ![Biztonsági mentés folyamatban](./media/backup-azure-vms/protect-inprogress.png)

> [!NOTE]
> Hello biztonsági mentési művelet során hello Azure Backup szolgáltatás kibocsát egy parancs toohello biztonsági mentési feladatok írási hibák, és egységes pillanatképet készít minden virtuális gép tooflush bővítményt.
>
>

Ha hello kezdeti biztonsági mentés befejezése után hello állapot hello hello virtuális gép **védett elemek** lap *védett*.

![A virtuális gép helyreállítási ponttal van mentve](./media/backup-azure-vms/protect-backedupvm.png)

## <a name="viewing-backup-status-and-details"></a>Biztonsági mentés állapotának és a részletek megtekintése
Ha védett, hello virtuális gépek számát is növekszik hello **irányítópult** összefoglaló lap. Hello **irányítópult** lapon a feladatok az elmúlt 24 órában, melyeket hello hello száma is látható *sikeres*, rendelkezik *sikertelen*, és a *folyamatban*. A hello **feladatok** lapján hello használata **állapot**, **művelet**, vagy **a** és **való** menük toofilter hello feladatok.

![Biztonsági mentés a irányítópult-oldalon állapota](./media/backup-azure-vms/dashboard-protectedvms.png)

Hello irányítópult értékeket 24 óránként egyszer frissülnek.

## <a name="troubleshooting-errors"></a>Kapcsolatos hibák elhárítása
Ha biztonsági során problémákat tapasztal a virtuális gép, nézze meg hello [VM hibaelhárítási cikke](backup-azure-vms-troubleshoot.md) segítségét.

## <a name="next-steps"></a>Következő lépések
* [A virtuális gépek kezelése és figyelése](backup-azure-manage-vms.md)
* [Virtuális gépek visszaállítása](backup-azure-restore-vms.md)
