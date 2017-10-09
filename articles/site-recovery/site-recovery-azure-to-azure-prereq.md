---
title: "az Azure Site Recovery segítségével replikációs tooAzure aaaPrerequisites |} Microsoft Docs"
description: "Ez a cikk összefoglalja az előfeltételeket replikáló virtuális gépek és fizikai gépek tooAzure hello Azure Site Recovery szolgáltatással."
services: site-recovery
documentationcenter: 
author: rajani-janaki-ram
manager: jwhit
editor: tysonn
ms.assetid: e24eea6c-50a7-4cd5-aab4-2c5c4d72ee2d
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 08/01/2017
ms.author: rajanaki
ms.openlocfilehash: c66cea8b097a872bae57e7b42e659e58c4b0b1f5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
#  <a name="prerequisites-for-replicating-azure-virtual-machines-tooanother-region-by-using-azure-site-recovery"></a>Az Azure virtuális gépek tooanother régió replikálása az Azure Site Recovery segítségével előfeltételei

> [!div class="op_single_selector"]
> * [Az Azure tooAzure replikáláshoz](site-recovery-azure-to-azure-prereq.md)
> * [A helyszíni tooAzure replikáláshoz](site-recovery-prereq.md)

hello Azure Site Recovery szolgáltatás hozzájárul tooyour üzleti folytonossági és vészhelyreállítási (BCDR) helyreállítási stratégia megvalósításában:
* Az Azure virtuális gépek tooanother az Azure-régió replikációját.
* A helyszíni fizikai kiszolgálók és virtuális gépek tooAzure vagy tooa másodlagos adatközpontba replikációját. 

Ha az elsődleges helyen valamilyen okból kimaradás lép fel, akkor átveheti tooa másodlagos helyre tookeep alkalmazások és számítási feladatok nem állnak. Elsődleges hely hátsó tooyour sikertelen lehet, ha toonormal műveletek adja vissza. A Site Recovery kapcsolatban bővebben lásd: [Mi az Site Recovery?](site-recovery-overview.md).

Ez a cikk hello Előfeltételek szükséges toobegin replikáció a Site Recovery a helyszíni tooAzure foglalja össze.

Bármely fűzhetnek hello cikk hello alján, vagy technikai kérdéseket hello [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="azure-requirements"></a>Azure-követelmények

**Követelmény** | **Részletek**
--- | ---
**Azure-fiók** | A [Microsoft Azure](http://azure.microsoft.com/) fiók.<br/><br/> Kezdésként használhatja az [ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/) is.
**Site Recovery szolgáltatás** | A Site Recovery díjszabásáról kapcsolatban bővebben lásd: [Site Recovery díjszabásáról](https://azure.microsoft.com/pricing/details/site-recovery/). Azt javasoljuk, hogy hozzon létre egy Recovery Services-tároló hello cél Azure-régiót, amelyet a vész-helyreállítási helyként toouse. Például ha a forrás virtuális gépek futnak, az USA keleti régiója, és tooreplicate tooCentral VELÜNK, azt javasoljuk, hogy Államok középső hello tároló létrehozása.|
**Az Azure kapacitás** | Hello cél Azure-régiót, amelyet az toouse a vész-helyreállítási helyként van szüksége a virtuális gépeket, a storage-fiókok és a hálózati összetevők elegendő kapacitással rendelkező előfizetés toohave. Támogatási tooadd nagyobb kapacitású felveheti a kapcsolatot.
**Tárolási útmutatásokkal** | Győződjön meg arról, hogy kövesse a hello [tárolási útmutatásokkal](../storage/common/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks) a forrás Azure virtuális gépen tooavoid bármely teljesítményproblémákat. Ha követi hello alapértelmezett beállításokat, a Site Recovery hello forrás konfigurációtól függően szükséges hello tárfiókok hoz létre. Ha testre szabhatja, és válassza ki a saját beállításait, győződjön meg arról, hogy hello kövesse [méretezhetőségi célok a virtuális gép lemezeinek](../storage/common/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks).
**Hálózati útmutató** | Szükséges toowhitelist hello kimenő kapcsolódás az Azure virtuális gép a megadott URL-címek vagy IP-címtartományok. További részletekért tekintse meg a toohello [útmutató az Azure virtuális gépek replikálása a hálózat](site-recovery-azure-to-azure-networking-guidance.md) cikk.
**Azure virtuális gép** | Győződjön meg arról, hogy minden hello legújabb legfelső szintű tanúsítványok hello Windows vagy Linux virtuális gép jelen-e. Hello legújabb legfelső szintű tanúsítványok nem szerepelnek, ha a hello VM regisztrált tooSite helyreállítási toosecurity megkötések miatt nem lehet.

>[!NOTE]
>Specifikus konfigurációk-támogatással kapcsolatos további tudnivalókért olvassa el a hello [támogatási mátrix](site-recovery-support-matrix-azure-to-azure.md).

## <a name="next-steps"></a>Következő lépések
- További információ [útmutató az Azure virtuális gépek replikálása a hálózat](site-recovery-azure-to-azure-networking-guidance.md).
- A munkaterhelések számára a védelmének megkezdéséhez [Azure virtuális gépek replikálásához](site-recovery-azure-to-azure.md).
