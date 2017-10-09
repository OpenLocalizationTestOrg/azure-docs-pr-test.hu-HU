---
title: "aaaReplicate Azure virtuális gépek Azure-régiók közötti |} Microsoft Docs"
description: "Összefoglalja a hello lépéseket szükséges tooreplicate Azure virtuális gépek közötti hello Azure Site Recovery szolgáltatás hello Azure-portálon az Azure-régiók"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmon
editor: 
ms.assetid: 6dd36239-4363-4538-bf80-a18e71b8ec67
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: raynew
ms.openlocfilehash: f4ac386f040945131f8a10f02143437f4441e298
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-azure-vms-between-regions-with-azure-site-recovery"></a>Azure virtuális gépek replikálása az Azure Site Recovery régiók között

>Ez a cikk hello lépéseket szükséges tooreplicate áttekintést nyújt az Azure virtuális gépek (VM) egy Azure-régiót tooAzure virtuális gépeket egy másik régióban. 

>[!NOTE]
>
> Az Azure virtuális gép replikációs jelenleg előzetes verzió.

Ez a cikk vagy hello hello alsó megjegyzések és kérdések utáni [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="step-1-review-architecture"></a>1. lépés: Felülvizsgálati architektúrája

Központi telepítés megkezdése előtt tekintse át a hello kialakítandó architektúra és hello összetevők toodeploy van szüksége.

Nyissa meg túl[1. lépés: hello architektúra áttekintése](azure-to-azure-walkthrough-architecture.md)


## <a name="step-2-review-prerequisites"></a>2. lépés: Felülvizsgálati Előfeltételek

Ellenőrizze, hogy rendelkezik-e hello helyen, beleértve az előfizetés, a virtuális hálózatok, a storage-fiókok és a virtuális gép Azure-előfeltételeknek.

Nyissa meg túl[2. lépés: Ellenőrizze az Előfeltételek és korlátozások](azure-to-azure-walkthrough-prerequisites.md)


## <a name="step-3-plan-networking"></a>3. lépés: A hálózat megtervezése

Ellenőrizze, hogy a kimenő kapcsolat tooreplicate, valamint, hogy be vannak-e beállítva a helyszíni kapcsolatok Azure virtuális gépeken van beállítva.

Nyissa meg túl[4. lépés: hálózat tervezése](azure-to-azure-walkthrough-network.md)



## <a name="step-4-create-a-vault"></a>4. lépés: A tároló létrehozása 

Tooset a Recovery Services-tároló tooorchestrate fel kell és -replikáció kezelésére, és adja meg a hello forrás régió.

Nyissa meg túl[4. lépés: a tároló létrehozása](azure-to-azure-walkthrough-vault.md)


## <a name="step-5-enable-replication"></a>5. lépés: A replikáció engedélyezése


tooenable replikációs cél hely beállításainak konfigurálása, replikációs házirend beállítása és válassza ki a megjeleníteni kívánt tooreplicate hello Azure virtuális gépeken. Miután engedélyezte, hello virtuális gép kezdeti replikálása következik be.

Nyissa meg túl[5. lépés: replikálás engedélyezése](azure-to-azure-walkthrough-enable-replication.md)


## <a name="step-6-run-a-test-failover"></a>6. lépés: Feladatátvételi teszt futtatása

Miután a kezdeti replikáció befejezését követően, és a változások replikálása fut, a teszt feladatátvételi toomake meg arról, hogy minden megfelelően működik-e is futtathatja.

Nyissa meg túl[6. lépés: feladatátvételi teszt futtatása](azure-to-azure-walkthrough-test-failover.md)


