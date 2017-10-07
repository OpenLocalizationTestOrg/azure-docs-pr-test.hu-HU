---
title: "a VMM-ben a Hyper-V virtuális gépek aaaEnable replikációs tooAzure felhők az Azure Site Recovery szolgáltatással |} Microsoft Docs"
description: "Ismerteti, hogyan tooenable replikációs tooAzure Hyper-V virtuális gépek a VMM-felhők, az Azure Site Recovery szolgáltatás hello"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 89a2c4fc-7e03-4a86-a2c0-52831ccebc1a
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2017
ms.author: raynew
ms.openlocfilehash: 1a39bf65490c59a89a5e891184cadfacc2adee6d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-11-enable-replication-tooazure-for-hyper-v-vms-in-vmm-clouds"></a>11. lépés: A Hyper-V virtuális gépek VMM-felhőkben replikációs tooAzure engedélyezése

Miután beállította a replikációs házirendet, használja a cikk tooenable replikálása a helyszíni Hyper-V virtuális gépek (VM) kezelése a System Center Virtual Machine Manager (VMM) felhők), tooAzure hello segítségével [Azure Site Recovery](site-recovery-overview.md)szolgáltatással hello Azure-portálon.

Ez a cikk vagy a hello hello alsó megjegyzések és kérdések utáni [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="before-you-start"></a>Előkészületek

Ellenőrizze, hogy az Azure-fiókjával rendelkezik a megfelelő hello [engedélyek](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines)toocreate Azure virtuális gépeken. [További](../active-directory/role-based-access-built-in-roles.md) Azure szerepköralapú hozzáférés-vezérléssel kapcsolatos.

## <a name="exclude-disks-from-replication"></a>Lemezek kizárása a replikációból

Alapértelmezés szerint az összes lemez gépen replikálódnak. Lemezek lehet kizárni a replikálásból. Például előfordulhat, hogy nem kívánja tooreplicate lemezek, amelyek ideiglenes vagy frissítette minden alkalommal, amikor olyan gép, adatok, vagy alkalmazás újraindul, (például pagefile.sys vagy SQL Server tempdb). [További információ](site-recovery-exclude-disk.md)

## <a name="replicate-vms"></a>Virtuális gépek replikálása

Engedélyezze a virtuális gépek replikálása az alábbiak szerint:  

1. Kattintson **2. lépés: Az alkalmazás replikálása** > **Forrás** elemre. Hello replikáció első alkalommal engedélyezését, kattintson a **+ replikálás** a hello tároló tooenable a további gépek replikációjának.

    ![A replikáció engedélyezése](./media/vmm-to-azure-walkthrough-enable-replication/enable-replication1.png)
2. A hello **forrás** panelen, jelölje be hello VMM-kiszolgáló és hello felhő, mely hello Hyper-V gazdagépek találhatók. Ezután kattintson az **OK** gombra.

    ![A replikáció engedélyezése](./media/vmm-to-azure-walkthrough-enable-replication/enable-replication-source.png)
3. A **cél**hello előfizetés, a feladatátvételt követően telepítési modell, válassza ki, és hello replikált adatok tárolására használni kívánt tárfiókot.

    ![A replikáció engedélyezése](./media/vmm-to-azure-walkthrough-enable-replication/enable-replication-target.png)
4. Válassza ki a kívánt toouse hello tárfiókot. Ha azt szeretné, hogy egy másik tárolóhelyre fiókhoz, mint a toouse, akkor [hozzon létre egyet](#set-up-an-azure-storage-account). Ha a replikált adatok használata a prémium szintű storage-fiók esetén kell tooselect egy további standard tárolási fiók toostore replikációs naplók rögzítése folyamatban lévő változtatások tooon helyszíni data.toocreate hello Resource Manager modellt használó tárfiókot Kattintson a **hozzon létre új**. Ha azt szeretné, toocreate hello klasszikus modellt használó tárfiókot, ehhez [hello Azure-portálon a](../storage/common/storage-create-storage-account.md). Ezután kattintson az **OK** gombra.
5. SELECT hello Azure hálózati és alhálózati toowhich Azure virtuális gépeken fog csatlakozni, ha a feladatátvételt követően jönnek létre. Válassza ki **beállítás most a kijelölt gépekhez**, tooapply hello beállítás tooall számítógépek választja a védelem. Válassza ki **konfigurálását később**, tooselect hello gépenként Azure-hálózatot. Ha azt szeretné, hogy toouse azokat van egy másik hálózaton, akkor [hozzon létre egyet](#set-up-an-azure-network). egy toocreate hello Resource Manager modellt kattintson **hozzon létre új**. Ha azt szeretné, toocreate hello klasszikus modellt használó hálózatot, ehhez [hello Azure-portálon a](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). Ha szükséges, válassza ki az alhálózatot. Ezután kattintson az **OK** gombra.
6. A **virtuális gépek** > **válassza ki a virtuális gépek**kattintson, és válassza az egyes gépek tooreplicate szeretné. Csak olyan gépeket választhat, amelyeken használható a replikáció funkció. Végül kattintson az **OK** gombra.

    ![A replikáció engedélyezése](./media/vmm-to-azure-walkthrough-enable-replication/enable-replication5.png)

7. A **tulajdonságok** > **tulajdonságainak konfigurálása**hello operációs rendszert válasszon ki a kijelölt hello virtuális gépek és operációsrendszer-lemez hello.

    - Győződjön meg arról, hogy hello Azure virtuális gép nevét (cél neve) megfelel [Azure virtuális gép követelményeinek](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).   
    - Alapértelmezés szerint az összes hello lemez a virtuális gép hello replikációhoz vannak kijelölve, de a lemezek tooexclude törölheti őket.

        - Érdemes lehet tooexclude lemezek tooreduce replikáció sávszélesség. Például előfordulhat, hogy nem kívánja tooreplicate lemezek ideiglenes adatokkal, vagy adatokat, minden alkalommal, amikor frissítette a gépek vagy alkalmazások (például pagefile.sys vagy Microsoft SQL Server tempdb) újraindul. Unselecting hello lemez hello lemez lehet kizárni a replikálásból.
        - Csak az alaplemezek zárhatók ki. Operációsrendszer-lemezek nem zárhatók ki.
        - Azt javasoljuk, hogy ne zárja ki a dinamikus lemezeket. A Site Recovery nem tudja azonosítani, hogy egy virtuális merevlemez a vendég virtuális gépben alap- vagy dinamikus lemez-e. Ha az összes függő dinamikus kötet lemez nem zárható ki, hello védett dinamikus lemez állapotúként lesz megjelölve, a hibás lemez hello VM átadja a feladatokat, és a lemezen levő hello adatok nem lesz elérhető.
        - A replikáció engedélyezése után már nem lehet ahhoz lemezeket hozzáadni, vagy lemezeket eltávolítani belőle. Ha szeretné, hogy tooadd, vagy zárja ki egy lemezt, toodisable védelmi hello VM van szükség, és majd engedélyezze újra.
        - Az Azure-ban manuálisan létrehozott lemezek nem vesznek részt a feladatátvételben. Például ha több mint három lemezt sikertelen, és hozzon létre két közvetlenül az Azure virtuális Gépen, csak hello három lemezeket, amelyeket a feladatátvételt volt sikertelen lesz újból az Azure tooHyper-V. Feladat-visszavétel, vagy a fordított replikálást a Hyper-V tooAzure manuálisan létrehozott lemezek nem adhat meg.
        - Ha kizárja a olyan lemezzel, amelyet egy alkalmazás toooperate van szükség, miután feladatátvételi tooAzure kell toocreate manuálisan az Azure, így az adott hello replikálva alkalmazás futtatható. Azt is megteheti az Azure automation sikerült integrálja a helyreállítási terv toocreate hello lemez hello gép feladatátvétele során.

    Kattintson a **OK** toosave módosításokat. A további tulajdonságokat később is beállíthatja.

    ![A replikáció engedélyezése](./media/vmm-to-azure-walkthrough-enable-replication/enable-replication6-with-exclude-disk.png)

8. A **replikációs beállítások** > **replikáció beállításainak konfigurálása**, válassza ki a kívánt tooapply hello védett virtuális gépek hello replikációs házirend. Ezután kattintson az **OK** gombra. Módosíthatja a hello replikációs házirendet a **replikációs házirendek** > szabályzat neve > **beállításainak szerkesztése**. Az itt megadott módosítások a már replikálás alatt álló, illetve az újonnan hozzáadott gépekre egyaránt érvényesek.

   ![A replikáció engedélyezése](./media/vmm-to-azure-walkthrough-enable-replication/enable-replication7.png)

Hello állapotának nyomon követheti **Védelemengedélyezési** feladat **feladatok** > **Site Recovery-feladatok**. Hello után **Védelemvéglegesítési** feladat fut, hello gép készen áll a feladatátvételre.



## <a name="next-steps"></a>Következő lépések

Nyissa meg túl[12. lépés: feladatátvételi teszt futtatása](vmm-to-azure-walkthrough-test-failover.md)
