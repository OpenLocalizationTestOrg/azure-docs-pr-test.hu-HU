---
title: "aaaHow nem a helyi gép replikációs tooa másodlagos helyszíni hely munka az Azure Site Recovery? | Microsoft Docs"
description: "Ez a cikk áttekintése összetevők és architektúra használható, ha replikálása a helyszíni virtuális gépek és fizikai kiszolgálók tooa másodlagos hely hello Azure Site Recovery szolgáltatásban."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: c413efcd-d750-4b22-b34b-15bcaa03934a
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/29/2017
ms.author: raynew
ms.openlocfilehash: 097a3f43446fec69ed7f9e0b7f11e8d11f41cc6a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-does-on-premises-machine-replication-tooa-secondary-site-work-in-site-recovery"></a>Hogyan nem helyszíni a gép replikációs tooa másodlagos hely munka a Site Recovery?

Ez a cikk ismerteti a hello összetevők és a folyamatok replikálása esetén a helyszíni virtuális gépek és fizikai kiszolgálók tooAzure hello segítségével [Azure Site Recovery](site-recovery-overview.md) szolgáltatás.

A következő tooa másodlagos helyszíni helyre hello replikálhatja:
- System Center Virtual Machine Manager-felhőkben (VMM-felhőkben) felügyelt helyszíni Hyper-V virtuális gépek, Hyper-V virtuális gépek Hyper-V fürtökön és különálló gazdagépek.
- Helyszíni VMware virtuális gépek és Windows/Linux fizikai kiszolgálók. Ebben az esetben a replikációt a Scout kezeli.

Ez a cikk vagy hello hello alsó megjegyzések utáni [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="replicate-hyper-v-vms-tooa-secondary-on-premises-site"></a>Hyper-V virtuális gépek tooa másodlagos helyszíni helyre replikálni.


### <a name="architectural-components"></a>Az architektúra összetevői

Ez a Hyper-V virtuális gépek tooa másodlagos helyre replikálni kell.

**Összetevő** | **Hely** | **Részletek**
--- | --- | ---
**Azure** | Ehhez szüksége lesz egy Microsoft-fiókra. |
**VMM-kiszolgáló** | Azt javasoljuk, hogy a VMM-kiszolgáló hello elsődleges hely és egy-egy hello másodlagos helyen | Minden VMM-kiszolgálóhoz csatlakoztatott toohello kell lennie internetes.<br/><br/> Minden kiszolgálónak rendelkeznie kell legalább egy VMM-magánfelhőt, a Hyper-V hello funkció készletet.<br/><br/> Hello Azure Site Recovery Provider telepítése hello VMM-kiszolgálón. hello szolgáltató koordinálja és hello keresztül koordinálja a replikáció a Site Recovery szolgáltatás hello internet. Hello szolgáltató és az Azure közötti kommunikáció biztonságos és titkosított.
**Hyper-V kiszolgáló** |  Egy vagy több Hyper-V gazdakiszolgálók hello elsődleges és másodlagos VMM-felhőkben.<br/><br/> Kiszolgálók csatlakoztatott toohello kell lennie internetes.<br/><br/> Adatait hello elsődleges és másodlagos Hyper-V gazdakiszolgálók közötti hello LAN-vagy VPN-és a Kerberos- vagy Tanúsítványalapú hitelesítés használatával replikálja a rendszer.  
**Hyper-V virtuális gépek** | Hello forrás Hyper-V gazdagép-kiszolgálón található. | hello forrás gazdagép-kiszolgálón legalább egy virtuális gép, amelyet az tooreplicate kell rendelkeznie.

### <a name="replication-process"></a>Replikációs folyamat

1. Hello Azure-fiók beállítása.
2. Létrehoz egy replikációsszolgáltatás-tárolót a Site Recoveryhez, és konfigurálja a tároló beállításait, például:

    - hello replikációs forrása és célja (elsődleges és másodlagos helyek).
    - Hello Azure Site Recovery Provider és hello Microsoft Azure Recovery Services Agent ügynök telepítése. hello szolgáltató VMM-kiszolgálókon, és minden Hyper-V gazdagépen hello ügynök van telepítve.
    - Létrehoz egy replikációs házirendet a forrás VMM-felhőhöz. hello házirend alkalmazott tooall hello felhőben állomáson található virtuális gépek.
    - Engedélyezi a replikációt a Hyper-V virtuális gépek számára. Kezdeti replikáció hello replikációs házirend-beállításoknak megfelelően.
4. Adatok változásait követi, és a különbözeti replikáció vált toobegins, hello kezdeti replikáció befejezését követően. Az elemek nyomon követett módosításait a rendszer .hrl fájlokban tárolja.
5. A teszt feladatátvételi toomake meg arról, hogy futtatása minden működik.

**1. ábra: A VMM tooVMM replikáció**

![A helyszíni tooon helyszíni](./media/site-recovery-components/arch-onprem-onprem.png)

### <a name="failover-and-failback-process"></a>Feladatátvételi és feladat-visszavételi folyamat

1. Futtathat tervezett vagy nem tervezett [feladatátvételt](site-recovery-failover.md) a helyszíni helyek között. Ha a tervezett feladatátvétel végrehajtása, majd a forrás virtuális gépeket állítsa le az tooensure adatvesztés nélküli.
2. Egyetlen gép feladatátvételt, vagy hozzon létre [helyreállítási tervek](site-recovery-create-recovery-plans.md) tooorchestrate több gép feladatátvétele.
4. Ha egy nem tervezett feladatátvétel tooa másodlagos hely hello feladatátvevő gépekhez hello másodlagos helyen nincsenek engedélyezve a védelem vagy replikáció után hajtható végre. Ha egy tervezett feladatátvételt hello feladatátvétel után már futott, védett gépek hello másodlagos helyen.
5. Ezt követően véglegesítse a hello feladatátvételi toostart elérése során hello munkaterhelés VM hello replikából.
6. Az elsődleges hely újra nem érhető el, akkor hello másodlagos hely toohello elsődleges a visszirányú replikálás tooreplicate kezdeményezni. Visszirányú replikálás során hello virtuális gépek kerülnek egy védett állapotban, de hello másodlagos adatközpontba még mindig aktív hely hello.
7. toomake hello elsődleges hely aktív helyre hello újra, elindít egy tervezett feladatátvételt a másodlagos tooprimary, egy másik visszirányú replikálás követ.




## <a name="replicate-vmware-vmsphysical-servers-tooa-secondary-site"></a>VMware virtuális gépek/fizikai kiszolgálók tooa másodlagos helyre replikálni.

VMware virtuális gépek vagy fizikai kiszolgálók tooa másodlagos hely InMage Scout segédprogramot, az az architektúra összetevőket használva használja replikálja:


### <a name="architectural-components"></a>Az architektúra összetevői

**Összetevő** | **Hely** | **Részletek**
--- | --- | ---
**Azure** | InMage Scout. | tooobtain Azure-előfizetés szükséges, InMage Scout segédprogramot.<br/><br/> Miután létrehozta a Recovery Services-tároló, InMage Scout segédprogramot letölteni, és telepítse a legújabb frissítések tooset hello hello központi telepítése szükséges.
**Folyamatkiszolgáló** | Az elsődleges helyen található | Hello folyamat server toohandle gyorsítótárazás, tömörítés és adatok optimalizálása telepít.<br/><br/> Kezeli a Unified Agent ügynököt toomachines tooprotect kívánt hello leküldéses telepítését is.
**Konfigurációs kiszolgáló** | A másodlagos helyen található | hello konfigurációs kiszolgáló kezeli, konfigurálása és a központi telepítés, figyelheti vagy hello felügyeleti weblapon vagy hello vContinuum-konzol használatával.
**vContinuum-kiszolgáló** | Választható. Hello telepített azonos hello konfigurációs kiszolgálón és helyen. | Ez az összetevő elérhetővé tesz egy konzolt, amelyről felügyelheti és figyelheti a védett környezetet.
**Fő célkiszolgáló** | Az adott hello másodlagos helyen található | hello fő célkiszolgáló tárolja a replikált adatokat. Az adatokat fogad az hello folyamatkiszolgáló, létrehozza a replika gépet hello másodlagos helyen, és hello adatmegőrzési pontokat tárolja.<br/><br/> hello száma fő célkiszolgálóra van szükség a gépeket lát el védelemmel hello száma függ.<br/><br/> Ha azt szeretné, hogy toofail hátsó toohello elsődleges webhely, túl kell egy fő célkiszolgálóra van. hello Unified Agent telepítve van ezen a kiszolgálón.
**VMware ESX/ESXi- és vCenter-kiszolgáló** |  A virtuális gépek ESX-/ESXi-gazdagépeken futnak. A gazdagépeket egy vCenter-kiszolgáló felügyeli | A VMware infrastructure tooreplicate VMware virtuális gépek van szüksége.
**Virtuális gépek/fizikai kiszolgálók** |  Az ügynök telepítve a VMware virtuális gépek és fizikai kiszolgálók tooreplicate egységes. | hello ügynök valósítja meg az összes hello összetevők közötti kommunikációt.


### <a name="replication-process"></a>Replikációs folyamat

1. Állítson be hello összetevőkiszolgálókat (konfigurációs, folyamat, a fő célkiszolgáló) minden helyen, és hello egyesített ügynök telepíthető, amelyet az tooreplicate gépek.
2. Kezdeti replikálás után hello ügynök minden egyes számítógépen különbözeti replikáció módosítások toohello folyamatkiszolgáló küld.
3. hello folyamatkiszolgáló optimalizálja a hello adatokat, és átadja toohello fő célkiszolgáló hello másodlagos helyen. hello konfigurációs kiszolgáló kezeli hello replikáció folyamatban.

**2. ábra: VMware tooVMware replikáció**

![VMware tooVMware](./media/site-recovery-components/vmware-to-vmware.png)


## <a name="next-steps"></a>Következő lépések

Felülvizsgálati hello [támogatási mátrix](site-recovery-support-matrix-to-sec-site.md)
