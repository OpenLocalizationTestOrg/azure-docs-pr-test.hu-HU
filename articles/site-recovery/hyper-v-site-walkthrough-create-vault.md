---
title: "egy Azure Site Recovery segítségével a Hyper-V replikáció (a System Center VMM nélkül) tooAzure tárolót fel aaaSet |} Microsoft Docs"
description: "Mentése a tárolóba tooset van szüksége az Azure Site Recovery segítségével a Hyper-V replikáció tooAzure hello lépéseket foglalja"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: a936b3e2-0dbe-47ac-a98e-5285d96dc786
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/22/2017
ms.author: raynew
ms.openlocfilehash: e3ef8758faab36d19d0968d98a23105bed7830f6
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
3. A **védelmi cél**, jelölje be **tooAzure** > **Igen, a Hyper-V-vel**. Válassza ki **nem** tooconfirm a VMM nem használja. 

    ![Célok megválasztása](./media/hyper-v-site-walkthrough-create-vault/choose-goals2.png)



## <a name="next-steps"></a>Következő lépések

Nyissa meg túl[8. lépés: állítson be forrása és célja](hyper-v-site-walkthrough-source-target.md)
