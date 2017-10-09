---
title: "egy Azure Site Recovery segítségével VMware-replikáció tooAzure tárolót fel aaaSet |} Microsoft Docs"
description: "Tooset mentése a tárolóba van szüksége az Azure Site Recovery segítségével VMware-replikáció tooAzure hello lépéseket foglalja"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 8bce940e-f19f-4418-8360-aee7b073519a
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: 8a7755a6c9a3f55f241c615e425285bc4b782493
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-7-set-up-a-vault-for-vmware-replication-tooazure"></a>7. lépés: Állítson be egy VMware-replikáció tooAzure tárolót


Ez a cikk ismerteti, hogyan tooset legfeljebb egy tárolót, és határozza meg, hogy a helyszíni helyről, hello segítségével tooAzure tooreplicate [Azure Site Recovery](site-recovery-overview.md) szolgáltatással hello Azure-portálon.


Ez a cikk vagy a hello hello alsó megjegyzések és kérdések utáni [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).




## <a name="create-a-recovery-services-vault"></a>Recovery Services-tároló létrehozása

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]

## <a name="select-a-protection-goal"></a>Válassza ki a védelmi cél

Jelölje ki a tooreplicate, és a tooreplicate, ahová.

1. Kattintson a **Recovery Services-tárolók** > tárolóban.
2. Hello erőforrás menüben, kattintson **Site Recovery** > **infrastruktúra előkészítése** > **védelmi cél**.
3. A **védelmi cél**, jelölje be **tooAzure** > **Igen, amelyen a VMware vSphere Hipervizorra**.



## <a name="next-steps"></a>Következő lépések

Nyissa meg túl[8. lépés: állítson be forrása és célja](vmware-walkthrough-source-target.md)
