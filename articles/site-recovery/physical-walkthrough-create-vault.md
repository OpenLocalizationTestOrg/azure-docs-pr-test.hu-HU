---
title: "egy Azure Site Recovery segítségével a fizikai kiszolgáló replikációs tooAzure tárolót fel aaaSet |} Microsoft Docs"
description: "Összefoglalja a hello lépéseket be Azure Site Recovery segítségével tároló tooreplicate fizikai kiszolgálók tooAzure tooset van szüksége"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: d99f2605-f417-4995-be77-5323136b814f
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: 988928e3ece31116823f132cc39223fe44443468
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-6-set-up-a-vault-for-physical-server-replication-tooazure"></a>6. lépés: Állítson be egy fizikai kiszolgáló replikációs tooAzure tárolót


Ez a cikk ismerteti, hogyan tooset mentése a tárolóba. Hozzon létre hello tárolót, és határozza meg, hogy a helyszíni hely tooAzure hello segítségével a tooreplicate [Azure Site Recovery](site-recovery-overview.md) szolgáltatással hello Azure-portálon.


Ez a cikk vagy a hello hello alsó megjegyzések és kérdések utáni [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).




## <a name="create-a-recovery-services-vault"></a>Recovery Services-tároló létrehozása

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]

## <a name="select-a-protection-goal"></a>Válassza ki a védelmi cél

Jelölje ki a tooreplicate, és a tooreplicate, ahová.

1. Kattintson a **Recovery Services-tárolók** > tárolóban.
2. Hello erőforrás menüben, kattintson **Site Recovery** > **infrastruktúra előkészítése** > **védelmi cél**.
3. A **védelmi cél**, jelölje be **tooAzure** > **nem virtualizált vagy egyéb**.


## <a name="next-steps"></a>Következő lépések

Nyissa meg túl[7. lépés: állítson be forrása és célja](physical-walkthrough-source-target.md)
