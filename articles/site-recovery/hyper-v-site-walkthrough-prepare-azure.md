---
title: "aaaPrepare Azure-erőforrások tooreplicate (a System Center VMM nélkül) a Hyper-V virtuális gépek tooAzure Azure Site Recovery segítségével |} Microsoft Docs"
description: "Ismerteti az Azure-beli replikálása az Azure Site Recovery segítségével a Hyper-V virtuális gépek (a VMM nélkül) tooAzure elindítása előtt"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 28fa722c-675e-4637-98eb-7ccbf3806d69
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/21/2017
ms.author: raynew
ms.openlocfilehash: f659e300c39253b0eaf7218bee9d39b11682edb1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-5-prepare-azure-resources-for-hyper-v-replication-tooazure"></a>5. lépés: Hyper-V replikáció tooAzure Azure-erőforrások előkészítése

Hello utasítások alapján ez a cikk tooprepare Azure az erőforrásokat, hogy replikálhatja a helyszíni Hyper-V virtuális gépek (a System Center VMM nélkül) használatával hello tooAzure [Azure Site Recovery](site-recovery-overview.md) szolgáltatás.

A cikk elolvasása után bármely fűzhetnek hello lap alján, vagy technikai kérdéseket hello [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="before-you-start"></a>Előkészületek

Győződjön meg arról, hogy elolvasta a hello [Előfeltételek](hyper-v-site-walkthrough-prerequisites.md)

## <a name="set-up-an-azure-account"></a>Az Azure-fiók beállítása

- Első egy [Microsoft Azure-fiók](http://azure.microsoft.com/).
- Kezdésként használhatja az [ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/) is.
- Hello támogatott régiók ellenőrizze a hely helyreállítását követően a cikknek a földrajzi elérhetőséggel a [Azure Site Recovery – díjszabás](https://azure.microsoft.com/pricing/details/site-recovery/).
- További tudnivalók [Site Recovery díjszabásáról](site-recovery-faq.md#pricing), és hello [díjszabása](https://azure.microsoft.com/pricing/details/site-recovery/).


## <a name="set-up-an-azure-network"></a>Azure-hálózat beállítása

- Állítsa be az Azure-hálózatot. Azure virtuális gépek kerülnek ehhez a hálózathoz, ha feladatátvételt követően jönnek létre.
- hello hello hálózat legyen ugyanabban a régióban, hello Recovery Services-tároló
- Site Recovery hello Azure-portálon állíthatja be hálózatokat fogja tudni használni [erőforrás-kezelő](../resource-manager-deployment-model.md), vagy a klasszikus mód.
- Javasoljuk, hogy a kezdés előtt hozza létre a hálózatot. Ha ezt elmulasztja, toodo kell azt a Site Recovery üzembe helyezése során.
- További tudnivalók [virtuális hálózati árképzési](https://azure.microsoft.com/pricing/details/virtual-network/).


## <a name="set-up-an-azure-storage-account"></a>Azure-tárfiók beállítása

- A Site Recovery a helyszíni gépeket tooAzure tárolási replikálja. Azure virtuális gépek hello tárolási készített feladatátvételt követően.
- Állítsa be a standard vagy prémium [Azure storage-fiók](../storage/common/storage-create-storage-account.md#create-a-storage-account) toohold adatait tooAzure replikálja.
- [Prémium szintű storage](../storage/common/storage-premium-storage.md) jellemzően egy folyamatosan magas I/O teljesítménye és kis késleltetésű toohost IO-igényes munkaterhelések igénylő virtuális gépektől.
- Ha azt szeretné, hogy toouse egy prémium szintű fiók toostore replikált adatokból, is szükség van egy standard tárolási fiók toostore replikálási naplókhoz, hogy a folyamatban lévő rögzítési tooon helyszíni adatokhoz módosítja.
- Attól függően, hogy az erőforrás-modellje hello meg toouse az Azure virtuális gépek a feladatátvételt, a fiók beállítása [Resource Manager módra](../storage/common/storage-create-storage-account.md), vagy [klasszikus módban](../storage/common/storage-create-storage-account.md).
- Azt javasoljuk, hogy állítsa be a tárfiók megkezdése előtt. Ha most kihagyja ezt toodo kell azt a Site Recovery üzembe helyezése során. hello fiókoknak kell lenniük a hello hello és ugyanabban a régióban Recovery Services-tároló.
- Nem lehet áthelyezni a storage-fiókok keresztül által használt Site Recovery erőforráscsoportok hello belül azonos előfizetés, vagy különböző előfizetésekhez.


## <a name="next-steps"></a>Következő lépések

Nyissa meg túl[6. lépés: Felkészülés a Hyper-V-erőforrások](hyper-v-site-walkthrough-prepare-hyper-v.md)
