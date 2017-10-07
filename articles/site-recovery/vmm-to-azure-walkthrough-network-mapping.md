---
title: "aaaConfigure hálózatra való leképezést a VMM-ben a Hyper-V virtuális gépek replikálása az Azure Site Recovery tooAzure felhők |} Microsoft Docs"
description: "Ismerteti, hogyan tooconfigure hálózatra való leképezést a VMM-ben a Hyper-V virtuális gépek replikálása esetén felhők tooAzure az Azure Site Recovery szolgáltatással"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: tysonn
ms.assetid: 8e7d868e-00f3-4e8b-9a9e-f23365abf6ac
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2017
ms.author: raynew
ms.openlocfilehash: 081a9fdb0ffa4114099e9bcb9c1b1e43ad26ecbb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-9-configure-network-mapping-for-hyper-v-replication-with-vmm-tooazure"></a>9. lépés: A Hyper-V-replikációt (VMM) tooAzure hálózatleképezés konfigurálása

Hello beállítása után [forrás és cél replikációs beállítások](vmm-to-azure-walkthrough-source-target.md), használja a cikk tooconfigure hálózati leképezési toomap helyszíni VMM-Virtuálisgép-hálózatok és az Azure-hálózatok között.

Ez a cikk vagy a hello hello alsó megjegyzések és kérdések utáni [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="before-you-start"></a>Előkészületek

- További tudnivalók [hálózatleképezés](vmm-to-azure-walkthrough-network.md#network-mapping-for-replication-to-azure).
- [Hálózatleképezés előkészítése a VMM](vmm-to-azure-walkthrough-network.md#prepare-vmm-for-network-mapping). 
- Győződjön meg arról, hogy virtuális gépek a VMM-kiszolgálón hello csatlakoztatott tooa Virtuálisgép-hálózatot, és, hogy létrehozta-e legalább egy Azure virtuális hálózat. Több Virtuálisgép-hálózatot csatlakoztatott tooa egyetlen Azure-hálózat lehet.

## <a name="configure-mapping"></a>Leképezés konfigurálása

Konfigurálja az alábbiak szerint a leképezéseket:

1. A **Site Recovery-infrastruktúra** > **Hálózatleképezések** > **Hálózatleképezés**, kattintson a hello **+ Hálózatleképezés**  ikonra.

    ![Hálózatleképezés](./media/vmm-to-azure-walkthrough-network-mapping/network-mapping1.png)
2. A **hálózatleképezés hozzáadása**, jelölje be hello forrás VMM-kiszolgálón, és **Azure** hello célként.
3. Ellenőrizze a hello előfizetés és hello telepítési modell a feladatátvételt követően.
4. A **Forráshálózat**, jelölje be hello forrás helyszíni Virtuálisgép-hálózat kívánt toomap hello VMM-kiszolgálóhoz társított hello listából.
5. A **célhálózat**, válassza ki hello Azure hálózati melyik replika Azure virtuális gépeken található során jönnek létre. Ezután kattintson az **OK** gombra.

    ![Hálózatleképezés](./media/vmm-to-azure-walkthrough-network-mapping/network-mapping2.png)

Ez történik a hálózatleképezés megkezdésekor:

* Hello forrás Virtuálisgép-hálózat meglévő virtuális gépekhez csatlakoztatott toohello célhálózat is, ha a leképezés kezdődik. Új virtuális gépek csatlakoztatott toohello forrás Virtuálisgép-hálózathoz kapcsolódó replikáláskor toohello leképezve az Azure-hálózatot.
* Meglévő hálózatleképezés módosítása esetén a replika virtuális gépek új hello-beállítások használatával csatlakozzon.
* Ha hello célhálózatban már több alhálózat működik, és ezek egyikének rendelkezik hello neve megegyezik a virtual machine hello forrás alhálózati található, akkor hello replika virtuális gép a feladatátvételt követően toothat cél alhálózathoz csatlakozik.
* Ha nincsenek egyező nevű cél alhálózathoz, hello virtuálisgép kapcsolódik toohello hello hálózat első alhálózatát.



## <a name="next-steps"></a>Következő lépések

Nyissa meg túl[10. lépés: a replikációs házirend létrehozása](vmm-to-azure-walkthrough-replication.md)
