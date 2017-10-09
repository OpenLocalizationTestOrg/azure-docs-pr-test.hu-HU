---
title: "a Hyper-V virtuális gép replikációs tooa másodlagos hely az Azure Site Recovery aaaMap hálózatok |} Microsoft Docs"
description: "Ismerteti, hogyan toomap hálózatok Hyper-V virtuális gépek tooa VMM másodlagos hely az Azure Site Recovery replikálása esetén."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 461b7c1c-ef61-4005-aeec-2324e727a3d0
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2017
ms.author: raynew
ms.openlocfilehash: d4f621df4ce08ae055bc6809daea0b71b76754ad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-7-map-networks-for-hyper-v-vm-replication-tooa-secondary-site"></a>7. lépés: Hyper-V virtuális gép replikációs tooa másodlagos hely hálózatok leképezése


Beállítása után [forrás és cél beállításai](vmm-to-vmm-walkthrough-source-target.md) replikálásához a Hyper-V virtuális gépek (VM) tooa másodlagos System Center Virtual Machine Manager (VMM) helyet, ez a cikk tooconfigure hálózatra való leképezést a Hyper-V virtuális használata gép (VM) replikációs tooa másodlagos hely használatával [Azure Site Recovery](site-recovery-overview.md).

A cikk elolvasása után fűzhetnek bármely hello lap alján, vagy a hello [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="before-you-start"></a>Előkészületek

- További tudnivalók [hálózatleképezés](vmm-to-vmm-walkthrough-network.md#network-mapping-overview) megkezdése előtt.
- Ellenőrizze, hogy a virtuális gépek VMM-kiszolgálókon csatlakoztatott tooa Virtuálisgép-hálózatot.

## <a name="configure-network-mapping"></a>Hálózatleképezés konfigurálása

1. A **Hálózatleképezés** > **Hálózatleképezések**, kattintson a **+ Hálózatleképezés**.
2. A **hálózatleképezés hozzáadása** lapon válassza ki a hello forrás és cél VMM-kiszolgálók. VMM-kiszolgálókon hello társított Virtuálisgép-hálózatok hello beolvasása.
3. A **Forráshálózat**, jelölje be azt szeretné, hogy a VMM-kiszolgáló hello társított Virtuálisgép-hálózatok listája hello toouse hello hálózati.
4. A **célhálózat**, jelölje be azt szeretné, hogy a VMM-kiszolgálón hello másodlagos toouse hello hálózati. Ezután kattintson az **OK** gombra.

    ![Hálózatleképezés](./media/vmm-to-vmm-walkthrough-network-mapping/network-mapping2.png)

Ez történik a hálózatleképezés megkezdésekor:

* Minden meglévő replika virtuális gépeket, amelyek megfelelnek a toohello forrás Virtuálisgép-hálózathoz csatlakoztatott toohello cél Virtuálisgép-hálózat lesz.
* Új virtuális gépek, amelyek a forrás Virtuálisgép-hálózathoz csatlakoztatott toohello replikáció után lesz csatlakoztatott toohello cél csatlakoztatott hálózati.
* Ha módosít egy meglévő hozzárendelést egy új hálózati, a replika virtuális gépek csatlakoznak hello új beállításokkal.
* Ha hello célhálózatban már több alhálózat működik, és ezek egyikének rendelkezik hello neve megegyezik a virtual machine hello forrás alhálózati található, akkor hello replika virtuális gép lesz a cél alhálózathoz csatlakoztatott toothat feladatátvételt követően. Ha nincsenek egyező nevű cél alhálózathoz, hello virtuális gép lesz a csatlakoztatott toohello hello hálózat első alhálózatát.



## <a name="next-steps"></a>Következő lépések

Nyissa meg túl[8. lépés: a replikációs házirend konfigurálása](vmm-to-vmm-walkthrough-replication.md).
