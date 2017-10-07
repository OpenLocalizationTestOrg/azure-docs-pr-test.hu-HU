---
title: "Hyper-V aaaEnable replikációs tooa másodlagos helyet az Azure Site Recovery szolgáltatással |} Microsoft Docs"
description: "Ismerteti, hogyan tooenable tooa replikálásához a Hyper-V virtuális gépek replikálása másodlagos System Center VMM helyet az Azure Site Recovery szolgáltatással."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: d8782d14-9fef-4396-8912-ff945efc851b
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2017
ms.author: raynew
ms.openlocfilehash: b4484e0118cb23f343187fe867d9795d30926baf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-9-enable-replication-tooa-secondary-site-for-hyper-v-vms"></a>9. lépés: A Hyper-V virtuális gépek replikációs tooa másodlagos hely engedélyezése


Miután beállította a replikációs házirendet, helyszíni Hyper-V virtuális gépek (VM), ez a cikk tooenable replikációs tooa másodlagos System Center Virtual Machine Manager (VMM) helyet használja [Azure Site Recovery](site-recovery-overview.md).

A cikk elolvasása után fűzhetnek bármely hello lap alján, vagy a hello [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).



## <a name="select-vms-tooreplicate"></a>Válassza ki a virtuális gépek tooreplicate

1. Kattintson **2. lépés: Az alkalmazás replikálása** > **Forrás** elemre. 

    ![A replikáció engedélyezése](./media/vmm-to-vmm-walkthrough-enable-replication/enable-replication1.png)

2. A **forrás**, válassza ki a VMM-kiszolgáló hello és hello felhő, mely hello tooreplicate kívánt Hyper-V-gazdagépek találhatók. Ezután kattintson az **OK** gombra.

    ![A replikáció engedélyezése](./media/vmm-to-vmm-walkthrough-enable-replication/enable-replication2.png)
3. A **cél**, ellenőrizze a hello másodlagos VMM-kiszolgáló és a felhő.
4. A **virtuális gépek**, válassza ki a kívánt hello listából tooprotect hello virtuális gépeket.

    ![A virtuális gép védelmének engedélyezése](./media/vmm-to-vmm-walkthrough-enable-replication/enable-replication5.png)

Hello állapotának nyomon követheti **Védelemengedélyezési** műveletét **feladatok** > **Site Recovery-feladatok**. Hello után **Védelemvéglegesítési** feladat befejeződik, hello kezdeti replikálás befejeződik, és hello virtuális gép készen áll a feladatátvételre.

Vegye figyelembe:

- Hello VMM-konzol virtuális gépek védelmét is engedélyezheti. Kattintson a **Védelemengedélyezési** hello eszköztár hello virtuális gép tulajdonságai > **Azure Site Recovery** lapon.
- Replikáció engedélyezése után megtekintheti a hello virtuális gép tulajdonságait a **replikált elemek**. A hello **Essentials** az irányítópult hello replikációs házirend hello virtuális gép és annak állapotát kapcsolatos információkat tekintheti meg. Kattintson a **tulajdonságok** további részleteket.

## <a name="onboard-existing-vms"></a>A bevezetni meglévő virtuális gépek

Ha meglévő virtuális gépek a VMM Alkalmazásban – a Hyper-V replika replikálja, hogy discoveryt őket az Azure Site Recovery replikációs az alábbiak szerint:

1. Győződjön meg arról, hogy hello hello elsődleges felhőben található meglévő Virtuálisgép üzemeltetési hello Hyper-V kiszolgálón, és hello replika virtuális gépet szolgáltató hello Hyper-V kiszolgálón hello másodlagos felhőben található.
2. Ellenőrizze, hogy a replikációs házirend hello elsődleges VMM-felhőben van konfigurálva.
3. Engedélyezze a hello elsődleges virtuális gép replikációját. Az Azure Site Recovery és a VMM győződjön meg arról, hogy hello azonos másodpéldány-állomás és a virtuális gép észlel, és Azure Site Recovery szeretné újrafelhasználni újra létrehozni a replikációs hello segítségével megadott beállítások.


## <a name="next-steps"></a>Következő lépések

Nyissa meg túl[10. lépés: feladatátvételi teszt futtatása](vmm-to-vmm-walkthrough-test-failover.md).
