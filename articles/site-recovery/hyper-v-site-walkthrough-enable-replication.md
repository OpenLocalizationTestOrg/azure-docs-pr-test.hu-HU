---
title: "Hyper-V virtuális gépek replikálása az Azure Site Recovery (a System Center VMM nélkül) tooAzure aaaEnable replikációs |} Microsoft Docs"
description: "Összefoglalja a hello lépéseit tooenable replikációs tooAzure hello Azure Site Recovery szolgáltatással a Hyper-V virtuális gépek"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: bd31ef01-69f1-4591-a519-e42510a6e2f4
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/21/2017
ms.author: raynew
ms.openlocfilehash: 1589cb7aa1fe954e075cb7bf1f4a4ec199ed3ec7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-10-enable-replication-for-hyper-v-vms-replicating-tooazure"></a>10. lépés: Hyper-V virtuális gépek replikálása tooAzure replikáció engedélyezése


Ez a cikk ismerteti, hogyan tooenable replikálása a helyszíni Hyper-V virtuális gépek (System Center VMM által nem felügyelt) tooAzure hello segítségével [Azure Site Recovery](site-recovery-overview.md) szolgáltatással hello Azure-portálon.

Ez a cikk vagy a hello hello alsó megjegyzések és kérdések utáni [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).




## <a name="before-you-start"></a>Előkészületek

Mielőtt elkezdené, győződjön meg arról, hogy az Azure felhasználói fiók rendelkezik-e a szükséges hello [engedélyek](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) egy új virtuális gép tooAzure tooenable replikációját.

## <a name="exclude-disks-from-replication"></a>Lemezek kizárása a replikációból

Alapértelmezés szerint az összes lemez gépen replikálódnak. Lemezek lehet kizárni a replikálásból. Például előfordulhat, hogy nem kívánja tooreplicate lemezek, amelyek ideiglenes vagy frissítette minden alkalommal, amikor olyan gép, adatok, vagy alkalmazás újraindul, (például pagefile.sys vagy SQL Server tempdb). [További információ](site-recovery-exclude-disk.md)


## <a name="replicate-vms"></a>Virtuális gépek replikálása

Engedélyezze a virtuális gépek replikálása az alábbiak szerint:          

1. Kattintson a **alkalmazás replikálása** > **forrás**. Először hello replikációjának beállítása után kattintson **+ replikálás** a további gépek replikációjának tooenable.

    ![A replikáció engedélyezése](./media/hyper-v-site-walkthrough-enable-replication/enable-replication.png)
2. A **forrás**, jelölje be hello Hyper-V helyet. Ezután kattintson az **OK** gombra.
3. A **cél**, válasszon hello tároló előfizetést és hello feladatátvételi típusú a toouse (klasszikus vagy erőforrás-kezelés) Azure-ban a feladatátvételt követően.
4. Válassza ki a kívánt toouse hello tárfiókot. Ha azt szeretné, hogy toouse fiók nem rendelkezik, akkor [hozzon létre egyet](#set-up-an-azure-storage-account). Ezután kattintson az **OK** gombra.
5. SELECT hello Azure hálózati és alhálózati toowhich Azure virtuális gépeken fog csatlakozni, ha feladatátvételi jönnek létre.

    - Válassza ki a tooapply hello hálózati beállítások tooall gépek engedélyezi a replikáció, **beállítás most a kijelölt gépekhez**.
    - Válassza ki **konfigurálását később** tooselect hello gépenként Azure-hálózatot.
    - Ha azt szeretné, hogy toouse hálózati nincs, akkor [hozzon létre egyet](#set-up-an-azure-network). Ha szükséges, válassza ki az alhálózatot. Ezután kattintson az **OK** gombra.

   ![A replikáció engedélyezése](./media/hyper-v-site-walkthrough-enable-replication/enable-replication11.png)

6. A **virtuális gépek** > **válassza ki a virtuális gépek**kattintson, és válassza az egyes gépek tooreplicate szeretné. Csak olyan gépeket választhat, amelyeken használható a replikáció funkció. Végül kattintson az **OK** gombra.

    ![A replikáció engedélyezése](./media/hyper-v-site-walkthrough-enable-replication/enable-replication5-for-exclude-disk.png)

7. A **tulajdonságok** > **tulajdonságainak konfigurálása**hello operációs rendszert válasszon ki a kijelölt hello virtuális gépek és operációsrendszer-lemez hello.
8. Győződjön meg arról, hogy hello Azure virtuális gép nevét (cél neve) megfelel [Azure virtuális gép követelményeinek](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).
9. Alapértelmezés szerint összes hello lemeze hello virtuális gép ki van jelölve, a replikáció. Törölje a jelet lemezek tooexclude őket.
10. Kattintson a **OK** toosave módosításokat. A további tulajdonságokat később is beállíthatja.

    ![A replikáció engedélyezése](./media/hyper-v-site-walkthrough-enable-replication/enable-replication6-with-exclude-disk.png)

11. A **replikációs beállítások** > **replikáció beállításainak konfigurálása**, válassza ki a kívánt tooapply hello védett virtuális gépek hello replikációs házirend. Ezután kattintson az **OK** gombra. Módosíthatja a hello replikációs házirendet a **replikációs házirendek** > házirend neve > **beállításainak szerkesztése**. Az itt megadott módosítások a már replikálás alatt álló, illetve újonnan hozzáadott gépekre egyaránt érvényesek lesznek.


   ![A replikáció engedélyezése](./media/hyper-v-site-walkthrough-enable-replication/enable-replication7.png)

Hello állapotának nyomon követheti **Védelemengedélyezési** feladat **feladatok** > **Site Recovery-feladatok**. Hello után **Védelemvéglegesítési** feladat futtatása hello gép készen áll a feladatátvételre.


## <a name="next-steps"></a>Következő lépések


Nyissa meg túl[11. lépés: feladatátvételi teszt futtatása](hyper-v-site-walkthrough-test-failover.md)
