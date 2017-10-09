---
title: "egy Azure Site Recovery segítségével a Hyper-V replikáció (a System Center VMM) tooAzure tárolót fel aaaSet |} Microsoft Docs"
description: "Összefoglalja a hello lépéseket az Azure Site Recovery segítségével a Hyper-V-replikációt (VMM) tooAzure szükséges tooset mentése a tárolóba"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: b3cd6f03-c33c-406d-91d4-5cba69f79abd
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/23/2017
ms.author: raynew
ms.openlocfilehash: f2c90f3c8b0a48db1e57fefd9829d29cffff8d43
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-7-set-up-a-vault-for-hyper-v-replication"></a>7. lépés: Egy tárolót a Hyper-V replikáció beállítása

Ez a cikk ismerteti, hogyan tooset legfeljebb egy tárolót, és határozza meg, hogy a helyszíni helyről, hello segítségével tooAzure tooreplicate [Azure Site Recovery](site-recovery-overview.md) szolgáltatással hello Azure-portálon.


Ez a cikk vagy a hello hello alsó megjegyzések és kérdések utáni [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="create-a-recovery-services-vault"></a>Recovery Services-tároló létrehozása

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]



## <a name="select-a-protection-goal"></a>Válassza ki a védelmi cél

Jelölje ki a tooreplicate, és a tooreplicate, ahová.

1. Kattintson a **Recovery Services-tárolók** > tárolóban.
2. Hello erőforrás menüben, kattintson **Site Recovery** > **infrastruktúra előkészítése** > **védelmi cél**.
3. A **védelmi cél**, jelölje be **tooAzure** > **Igen, a Hyper-V-vel**. Válassza ki **Igen** tooconfirm Ön nhasználják VMM. 

     ![Célok megválasztása](./media/vmm-to-azure-walkthrough-create-vault/choose-goals.png)



## <a name="next-steps"></a>Következő lépések

Nyissa meg túl[8. lépés: állítson be forrása és célja](vmm-to-azure-walkthrough-source-target.md)
