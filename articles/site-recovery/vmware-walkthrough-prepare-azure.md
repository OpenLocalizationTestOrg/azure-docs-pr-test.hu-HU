---
title: "Azure-erőforrások tooreplicate aaaPrepare helyszíni VMware virtuális gépek tooAzure Azure Site Recovery segítségével |} Microsoft Docs"
description: "Ismerteti az Azure-beli replikálása az Azure Site Recovery segítségével helyszíni VMware virtuális gépek tooAzure elindítása előtt"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 4e320d9b-8bb8-46bb-ba21-77c5d16748ac
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: ac72fff0593783add789408ecfeb1812d70108b9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-5-prepare-azure-resources-for-vmware-replication-tooazure"></a>5. lépés: Felkészülés VMWare replikációs tooAzure Azure-erőforrások


Ez a cikk tooprepare Azure hello utasításokat használható erőforrásokat, hogy a helyszíni gépeket tooAzure hello segítségével replikálhatja [Azure Site Recovery](site-recovery-overview.md) szolgáltatás.

Ez a cikk vagy a hello hello alsó megjegyzések és kérdések utáni [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="before-you-start"></a>Előkészületek

Győződjön meg arról, hogy elolvasta a hello [Előfeltételek](vmware-walkthrough-prerequisites.md)

## <a name="set-up-an-azure-account"></a>Az Azure-fiók beállítása

- Első egy [Microsoft Azure-fiók](http://azure.microsoft.com/).
- Kezdésként használhatja az [ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/) is.
- Hello támogatott régiók ellenőrizze a hely helyreállítását követően a cikknek a földrajzi elérhetőséggel a [Azure Site Recovery – díjszabás](https://azure.microsoft.com/pricing/details/site-recovery/).
- További tudnivalók [Site Recovery díjszabásáról](site-recovery-faq.md#pricing), és hello [díjszabása](https://azure.microsoft.com/pricing/details/site-recovery/).



## <a name="set-up-an-azure-network"></a>Azure-hálózat beállítása

- Állítsa be az Azure-hálózatot. Azure virtuális gépek kerülnek ehhez a hálózathoz, ha feladatátvételt követően jönnek létre.
- Site Recovery hello Azure-portálon állíthatja be hálózatokat fogja tudni használni [erőforrás-kezelő](../resource-manager-deployment-model.md), vagy a klasszikus mód.
- hello hello hálózat legyen ugyanabban a régióban, hello Recovery Services-tároló
- További tudnivalók [virtuális hálózati árképzési](https://azure.microsoft.com/pricing/details/virtual-network/).
- További információ [Azure Virtuálisgép-kapcsolat](site-recovery-network-design.md) feladatátvételt követően.


## <a name="set-up-an-azure-storage-account"></a>Azure-tárfiók beállítása

- A Site Recovery a helyszíni gépeket tooAzure tárolási replikálja. Azure virtuális gépek hello tárolási készített feladatátvételt követően.
- Állítson be egy [Azure storage-fiók](../storage/common/storage-create-storage-account.md#create-a-storage-account) a replikált adatok tárolásához.
- Site Recovery hello Azure-portálon használható storage-fiókok beállítása az erőforrás-kezelőben, vagy a klasszikus módban.
- lehet, hogy a tárfiók hello standard vagy [prémium](../storage/common/storage-premium-storage.md).
- Ha a prémium szintű fiók beállítása is szüksége lesz egy további standard fiók naplóadatokat.


## <a name="next-steps"></a>Következő lépések

Nyissa meg túl[6. lépés: Felkészülés VMware erőforrások](vmware-walkthrough-prepare-vmware.md)
