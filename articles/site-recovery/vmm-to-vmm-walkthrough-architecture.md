---
title: "a Hyper-V replikáció tooa másodlagos hely az Azure Site Recovery aaaReview hello architektúra |} Microsoft Docs"
description: "Ez a cikk áttekintést hello architektúra a helyszíni Hyper-V virtuális gépek tooa System Center VMM rendelkező másodlagos helyről Azure Site Recovery replikálására."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 07099161-4cc7-4f32-8eb9-2a71bbf0750b
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/30/2017
ms.author: raynew
ms.openlocfilehash: 0de4b4e8601116c73e6fd710597ce4e561884368
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-1-review-hello-architecture-for-hyper-v-replication-tooa-secondary-site"></a>1. lépés:, Tekintse át a Hyper-V replikáció tooa másodlagos hely hello architektúrája

Ez a cikk ismerteti a hello összetevők és a folyamatok replikálása esetén a helyszíni Hyper-V virtuális gépek (VM) a System Center Virtual Machine Manager (VMM) felhők, tooa VMM másodlagos hely hello segítségével [Azure Site Recovery](site-recovery-overview.md)szolgáltatással hello Azure-portálon.

Ez a cikk vagy hello hello alsó megjegyzések utáni [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).



## <a name="architectural-components"></a>Az architektúra összetevői

Ez a Hyper-V virtuális gépek tooa VMM másodlagos hely replikálására kell.

**Összetevő** | **Hely** | **Részletek**
--- | --- | ---
**Azure** | Előfizetés az Azure-ban. | Hello Azure-előfizetésre, tooorchestrate a Recovery Services-tároló létrehozása és kezelése a VMM-helyek közötti replikációval.
**VMM-kiszolgáló** | Szüksége lesz egy elsődleges és egy másodlagos VMM-helyre. | Azt javasoljuk, hogy a VMM-kiszolgáló hello elsődleges hely és egy-egy hello másodlagos helyen 
**Hyper-V kiszolgáló** |  Egy vagy több Hyper-V gazdakiszolgálók hello elsődleges és másodlagos VMM-felhőkben. | Adatait hello elsődleges és másodlagos Hyper-V gazdakiszolgálók közötti hello LAN-vagy VPN-és a Kerberos- vagy Tanúsítványalapú hitelesítés használatával replikálja a rendszer.  
**Hyper-V virtuális gépek** | A Hyper-V gazdakiszolgálón. | hello forrás gazdagép-kiszolgálón legalább egy virtuális gép, amelyet az tooreplicate kell rendelkeznie.

## <a name="replication-process"></a>Replikációs folyamat

1. Hello Azure-fiók beállítása, a Recovery Services-tároló létrehozása, és határozza meg, hogy tooreplicate.
2. Beállítások hello forrás és cél replikációs, beleértve a hello Azure Site Recovery Provider telepítése a VMM-kiszolgálókon és hello Microsoft Azure Recovery Services Agent ügynököt minden Hyper-V gazdagépen.
3. Hello forrás VMM-felhő replikációs házirend létrehozása. hello házirend alkalmazott tooall hello felhőben állomáson található virtuális gépek.
4. Minden VMM-replikáció engedélyezi, és a virtuális gépek kezdeti replikálást, a kiválasztott hello beállításokat megfelelően.
5. A kezdeti replikáció után megkezdődik a változáskülönbözetek replikációja. Az elemek nyomon követett módosításait a rendszer .hrl fájlokban tárolja.


![A helyszíni tooon helyszíni](./media/vmm-to-vmm-walkthrough-architecture/arch-onprem-onprem.png)

## <a name="failover-and-failback-process"></a>Feladatátvételi és feladat-visszavételi folyamat

1. Futtathat tervezett vagy nem tervezett [feladatátvételt](site-recovery-failover.md) a helyszíni helyek között. Ha a tervezett feladatátvétel végrehajtása, majd a forrás virtuális gépeket állítsa le az tooensure adatvesztés nélküli.
2. Egyetlen gép feladatátvételt, vagy hozzon létre [helyreállítási tervek](site-recovery-create-recovery-plans.md) tooorchestrate több gép feladatátvétele.
4. Ha egy nem tervezett feladatátvétel tooa másodlagos hely hello feladatátvevő gépekhez hello másodlagos helyen nincsenek engedélyezve a védelem vagy replikáció után hajtható végre. Ha egy tervezett feladatátvételt hello feladatátvétel után már futott, védett gépek hello másodlagos helyen.
5. Ezt követően véglegesítse a hello feladatátvételi toostart elérése során hello munkaterhelés VM hello replikából.
6. Az elsődleges hely újra nem érhető el, akkor hello másodlagos hely toohello elsődleges a visszirányú replikálás tooreplicate kezdeményezni. Visszirányú replikálás során hello virtuális gépek kerülnek egy védett állapotban, de hello másodlagos adatközpontba még mindig aktív hely hello.
7. toomake hello elsődleges hely aktív helyre hello újra, elindít egy tervezett feladatátvételt a másodlagos tooprimary, egy másik visszirányú replikálás követ.



## <a name="next-steps"></a>Következő lépések

Nyissa meg túl[2. lépés: tekintse át a hello Előfeltételek és korlátozások](vmm-to-vmm-walkthrough-prerequisites.md).
