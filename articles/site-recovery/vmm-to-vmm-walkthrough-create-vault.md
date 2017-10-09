---
title: "a Hyper-V replikáció tooa másodlagos hely az Azure Site Recovery tárolójából aaaCreate |} Microsoft Docs"
description: "Ismerteti, hogyan toocreate egy tároló tooa Hyper-V virtuális gépek replikálása esetén másodlagos System Center VMM helyet az Azure Site Recovery szolgáltatással."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: ff65dbfb-cb26-410e-ab48-76971625db08
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2017
ms.author: raynew
ms.openlocfilehash: 96ee09cbf2376a5089b9efa09dc7ab3fb7d472cb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-5-create-a-vault-for-hyper-v-replication-tooa-secondary-site"></a>5. lépés: Hyper-V replikáció tooa másodlagos hely tároló létrehozása

A helyszíni előkészítését követően [System Center Virtual Machine Manager (VMM) kiszolgáló és Hyper-V gazdagépek/fürt](vmm-to-vmm-walkthrough-vmm-hyper-v.md) a Hyper-V replikáció tooa másodlagos hely [Azure Site Recovery](site-recovery-overview.md), létrehozhat egy Recovery Services-tároló, és jelölje be hello replikációs forgatókönyv.

A cikk elolvasása után fűzhetnek bármely hello lap alján, vagy a hello [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="create-a-recovery-services-vault"></a>Recovery Services-tároló létrehozása

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]


## <a name="choose-a-protection-goal"></a>Válassza ki a védelmi cél

Válassza ki a kívánt tooreplicate és a tooreplicate, ahová.

1. Kattintson a **Site Recovery** > **1. lépés: az infrastruktúra előkészítése** > **védelmi cél**.
2. Válassza ki **toorecovery hely**, és válassza ki **Igen, a Hyper-V-vel**.
3. Válassza ki **Igen** tooindicate VMM toomanage hello Hyper-V-gazdagépek használata.
4. Válassza ki **Igen** Ha egy másodlagos VMM-kiszolgálóval rendelkezik. Ha egy VMM-kiszolgálón felhők közötti replikáció telepít, kattintson a **nem**. Ezután kattintson az **OK** gombra.

    ![Célok megválasztása](./media/vmm-to-vmm-walkthrough-create-vault/choose-goals.png)



## <a name="next-steps"></a>Következő lépések

Nyissa meg túl[6. lépés: hello replikációs forrás és cél beállítása](vmm-to-vmm-walkthrough-source-target.md).
