---
title: "aaaHow működik a Site Recovery munkahelyi? | Microsoft Docs"
description: "Ebben a cikkben a Site Recovery architektúráját mutatjuk be."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: c413efcd-d750-4b22-b34b-15bcaa03934a
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/14/2017
ms.author: raynew
ROBOTS: NOINDEX, NOFOLLOW
redirect_url: site-recovery-azure-to-azure-architecture
ms.openlocfilehash: ff1580d0fe294148dc0c621728491e6119c3048a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-does-azure-site-recovery-work-for-on-premises-infrastructure"></a>Hogy működik az Azure Site Recovery egy helyszíni infrastruktúra esetében?

> [!div class="op_single_selector"]
> * [Azure-alapú virtuális gépek replikálása](site-recovery-azure-to-azure-architecture.md)
> * [Helyszíni gépek replikálása](site-recovery-components.md)

Ez a cikk ismerteti a mögöttes architektúráját, valamint hello [Azure Site Recovery](site-recovery-overview.md) szolgáltatás, és hello összetevőket hogyan működik a helyszíni tooAzure munkaterhelések replikálásához.

Ez a cikk vagy hello hello alsó megjegyzések utáni [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="replicate-tooazure"></a>TooAzure replikálása

Replikálja, és a helyszíni infrastruktúra tooAzure hello következő védelme:

- **VMware**: [Támogatott gazdagépen](site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers) futó helyszíni VMware virtuális gépek. A [támogatott operációs rendszereket](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions) futtató VMware virtuális gépeket replikálhatja.
- **Hyper-V**: [Támogatott gazdagépeken](site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers) futó helyszíni Hyper-V virtuális gépek.
- **Fizikai gépek**: [Támogatott operációs rendszereken](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions) Windowst vagy Linuxot futtató helyszíni fizikai kiszolgálók. A [Hyper-V és az Azure által támogatott](https://technet.microsoft.com/en-us/windows-server-docs/compute/hyper-v/supported-windows-guest-operating-systems-for-hyper-v-on-windows) bármilyen vendég operációs rendszert futtató Hyper-V-alapú virtuális gépet replikálhat.

## <a name="vmware-tooazure"></a>VMware tooAzure

Ez a VMware virtuális gépek tooAzure replikálására kell.

Terület | Összetevő | Részletek
--- | --- | ---
**Konfigurációs kiszolgáló** | Egyetlen felügyeleti kiszolgáló (VMware virtuális gép) futtatja az összes helyszíni összetevőt – a konfigurációs kiszolgálót, a folyamatkiszolgálót és a fő célkiszolgálót | hello konfigurációs kiszolgáló koordinálja a helyszíni és az Azure közötti kommunikáció, és adatreplikáció kezeli.
 **Folyamatkiszolgáló**:  | Hello konfigurációs kiszolgálón alapértelmezés szerint telepítve. | Replikációs átjáróként üzemel. Kap replikációs adatokat, gyorsítótárazás, tömörítés és titkosítás segítségével optimalizálja őket, és visszaküldi azt tooAzure tárolási.<br/><br/> hello folyamatkiszolgáló is leküldéses telepítését hello mobilitási szolgáltatás tooprotected gépek kezeli, és végrehajtja a VMware virtuális gépek automatikus észlelése.<br/><br/> A központi telepítés növekedésével további külön dedikált folyamat kiszolgálók toohandle növelése replikációs forgalom mennyisége is hozzáadhat.
 **Fő célkiszolgáló** | Hello helyszíni konfigurációs kiszolgáló alapértelmezés szerint telepítve. | Az Azure-ból történő feladat-visszavétel során kezeli a replikációs adatokat.<br/><br/> Ha a feladat-visszavételi adatforgalom kötetei nagyok, a feladat-visszavételhez üzembe helyezhet egy másik fő célkiszolgálót.
**VMware-kiszolgálók** | VMware virtuális gépeket üzemeltető vSphere ESXi-kiszolgálón, és azt javasoljuk a vCenter server toomanage hello gazdagépek. | VMware kiszolgálók tooyour Recovery Services-tároló hozzáadása<br/><br/>
**Replikált gépek** | hello mobilitási szolgáltatás telepíti az egyes VMware virtuális gép kívánt tooreplicate. Telepíthető manuálisan minden számítógépen, vagy a leküldéses telepítés hello folyamat kiszolgálóról.| -

**1. ábra: VMware tooAzure összetevők**

![Összetevők](./media/site-recovery-components/arch-enhanced.png)

### <a name="replication-process"></a>Replikációs folyamat

1. Hello központi telepítését, beleértve az Azure összetevőket, és a Recovery Services-tároló beállítása. Hello tároló hello replikációs forrása és célja, állítsa be a konfigurációs kiszolgálón hello VMware-kiszolgálók hozzáadása, hozzon létre egy replikációs házirendet, hello mobilitási szolgáltatás üzembe helyezése, engedélyezze a replikálást és feladatátvételi teszt futtatása kell megadni.
2.  Gépek hello replikációs nyilatkozatnak replikálást indítani, és egy kezdeti másolatot készít hello adatok replikált tooAzure tároló.
4. Különbözeti módosítások tooAzure replikálása hello kezdeti replikáció befejezését követően megkezdődik. A gépek nyomon követett módosításait a rendszer egy .hrl fájlban tárolja.
    - Replikáló gépek kommunikálni hello konfigurációs kiszolgáló HTTPS 443-as portot a bejövő, a replikáció kezelését.
    - Replikáló gépek küldése replikációs adatok toohello folyamatkiszolgáló HTTPS 9443 porton bejövő (konfigurálható).
    - hello konfigurációs kiszolgáló koordinálja a replikáció kezelését az Azure-ral HTTPS 443-as kimenő porton keresztül.
    - hello folyamatkiszolgáló adatokat fogad az forrásgépek, optimalizálja és titkosítja azokat, és elküldi azt tooAzure tárolási 443-as porton keresztül kimenő.
    - Ha engedélyezi a virtuális Gépre kiterjedő konzisztencia, majd gépek hello replikációs csoport kommunikálnak egymással 20004 porton keresztül. Több virtuális gépes környezetről akkor beszélünk, ha a gépek feladatátvételkor azonos összeomlásbiztos és alkalmazáskonzisztens helyreállítási pontokat használó replikációs csoportokba vannak rendezve. Ez akkor hasznos, ha a gépek is működnek, ugyanaz az alkalmazás hello és toobe konzisztens kell.
5. Akkor replikált tooAzure tárolási nyilvános végpontok, több mint hello internet. Erre a célra az Azure ExpressRoute [nyilvános társviszony-létesítési](https://docs.microsoft.com/en-us/azure/expressroute/expressroute-circuit-peerings#public-peering) szolgáltatását is használhatja. Forgalom replikál egy helyszíni hely tooAzure az a pont-pont VPN-kapcsolaton keresztül nem támogatott.

**2. ábra: VMware tooAzure replikáció**

![Továbbfejlesztett](./media/site-recovery-components/v2a-architecture-henry.png)

### <a name="failover-and-failback"></a>Feladatátvétel és feladat-visszavétel

1. Miután meggyőződött a feladatátvételi teszt a várt módon működik, a nem tervezett feladatátvételek tooAzure szükség szerint is futtathatja. A tervezett feladatátvétel nem támogatott.
2. Egyetlen gép feladatátvételt, vagy hozzon létre [helyreállítási tervek](site-recovery-create-recovery-plans.md), toofail át több virtuális géphez.
3. Feladatátvétel futtatásakor az Azure-ban replikaként létrehozott virtuális gépek jönnek létre. A feladatátvételi toostart elérése során hello munkaterhelés hello replikáról Azure virtuális gép véglegesítése után.
4. Amint az elsődleges helyszíni hely megint elérhetővé válik, visszaadhatja a feladatokat. A feladat-visszavétel infrastruktúra beállítása, start hello gépek replikálásához hello másodlagos hely toohello elsődleges és egy nem tervezett feladatátvételt hello másodlagos helyekről futtatni. Ennél a feladatátvételnél véglegesítést követően adatokat kell vissza a helyszíni, és tooenable replikációs tooAzure újra kell. [További információ](site-recovery-failback-azure-to-vmware.md)

**3. ábra: VMware-/fizikai gépek közötti feladat-visszavétel**

![Feladat-visszavétel](./media/site-recovery-components/enhanced-failback.png)

## <a name="physical-tooazure"></a>Fizikai tooAzure

A replikált fizikai, helyszíni kiszolgálók tooAzure replikáció által használt is hello azonos összetevők és folyamatokat [VMware tooAzure](#vmware-replication-to-azure), de ezek a különbségek:

- Hello konfigurációs kiszolgáló, a VMware virtuális gépek helyett használhat egy fizikai kiszolgáló
- A feladat-visszavételhez helyszíni VMware-infrastruktúrára van szükség. Nem utasíthat el hátsó tooa fizikai gépen.

## <a name="hyper-v-tooazure"></a>Hyper-V tooAzure

### <a name="replication-process"></a>Replikációs folyamat

1. Beállítása hello Azure összetevőket. Javasoljuk, hogy a Site Recovery üzembe helyezésének megkezdése előtt hozza létre a Storage- és hálózati fiókokat.
2. Létrehoz egy replikációsszolgáltatás-tárolót a Site Recoveryhez, és konfigurálja a tároló beállításait, például:
    - Forrás- és célbeállítások. Ha nem Ön által felügyelt Hyper-V gazdagépek a VMM-felhőben, hello cél kell létrehozni egy Hyper-V hely tárolót, és adja hozzá a Hyper-V gazdagépek tooit. Ha a Hyper-V-gazdagépek a VMM kezeli, hello forrása hello VMM-felhőben. hello Azure célja.
    - Hello Azure Site Recovery Provider és hello Microsoft Azure Recovery Services Agent ügynök telepítése. Ha a VMM hello szolgáltató lesz telepítve, és minden Hyper-V gazdagépen ügynök hello. Ha a VMM nem rendelkezik, mind a hello szolgáltató, és az ügynök települnek minden állomáson.
    - Hello Hyper-V hely vagy a VMM-felhő replikációs házirend létrehozása. hello házirend alkalmazott tooall virtuális gépek gazdagépén hello hely vagy a felhőben található.
    - Engedélyezi a replikációt a Hyper-V virtuális gépek számára. Kezdeti replikáció hello replikációs házirend-beállításoknak megfelelően.
4. Adatok változások nyomon követése, és a különbözeti módosítások tooAzure replikálása hello kezdeti replikáció befejezését követően megkezdődik. Az elemek nyomon követett módosításait a rendszer .hrl fájlokban tárolja.
5. A teszt feladatátvételi toomake meg arról, hogy futtatása minden működik.

### <a name="failover-and-failback-process"></a>Feladatátvételi és feladat-visszavételi folyamat

1. Futtathatja a tervezett vagy nem tervezett [feladatátvételi](site-recovery-failover.md) a helyszíni Hyper-V virtuális gépek tooAzure. Ha a tervezett feladatátvétel végrehajtása, majd a forrás virtuális gépeket állítsa le az tooensure adatvesztés nélküli.
2. Egyetlen gép feladatátvételt, vagy hozzon létre [helyreállítási tervek](site-recovery-create-recovery-plans.md) tooorchestrate több gép feladatátvétele.
4. Hello feladatátvétel futtatása után meg kell tudni toosee hello replika virtuális gép létrehozása az Azure-ban. A nyilvános IP-cím toohello VM rendelhet is, ha szükséges.
5. Majd véglegesíteni hello feladatátvételi toostart hello replika Azure virtuális gép hello alkalmazások és szolgáltatások eléréséhez.
6. Amint az elsődleges helyszíni hely megint elérhetővé válik, [visszaadhatja](site-recovery-failback-from-azure-to-hyper-v.md) a feladatokat. Egy tervezett feladatátvételt az Azure toohello elsődleges helyről indítsa el. A tervezett feladatátvételhez is válassza toofailback toohello azonos virtuális gép vagy tooan másik helyre, és szinkronizálja a módosítása Azure és a helyszíni, tooensure között adatvesztés nélküli Virtuális gépek létrehozásakor a helyszíni, hello feladatátvételi véglegesítést.

**4. ábra: A Hyper-V helyek tooAzure replikációs**

![Összetevők](./media/site-recovery-components/arch-onprem-azure-hypervsite.png)

**5. ábra: A Hyper-V a VMM-felhők tooAzure replikáció**

![Összetevők](./media/site-recovery-components/arch-onprem-onprem-azure-vmm.png)


## <a name="replicate-tooa-secondary-site"></a>Másodlagos hely tooa replikálása

A következő tooyour másodlagos helyhez hello replikálhatja:

- **VMware**: [Támogatott gazdagépen](site-recovery-support-matrix-to-sec-site.md#on-premises-servers) futó helyszíni VMware virtuális gépek. A [támogatott operációs rendszereket](site-recovery-support-matrix-to-sec-site.md#support-for-replicated-machine-os-versions) futtató VMware virtuális gépeket replikálhatja.
- **Fizikai gépek**: [Támogatott operációs rendszereken](site-recovery-support-matrix-to-sec-site.md#support-for-replicated-machine-os-versions) Windowst vagy Linuxot futtató helyszíni fizikai kiszolgálók.
- **Hyper-V**: VMM-felhőkben felügyelt [támogatott Hyper-V-gazdagépeken](site-recovery-support-matrix-to-sec-site.md#on-premises-servers) futó helyszíni Hyper-V virtuális gépek. [támogatott gazdagépek](site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers). A [Hyper-V és az Azure által támogatott](https://technet.microsoft.com/en-us/windows-server-docs/compute/hyper-v/supported-windows-guest-operating-systems-for-hyper-v-on-windows) bármilyen vendég operációs rendszert futtató Hyper-V-alapú virtuális gépet replikálhat.


## <a name="vmwarephysical-tooa-secondary-site"></a>VMware vagy fizikai tooa másodlagos helyen

VMware virtuális gépek vagy fizikai kiszolgálók tooa másodlagos hely InMage Scout segédprogramot használja replikálja.

### <a name="components"></a>Összetevők

**Terület** | **Összetevő** | **Részletek**
--- | --- | ---
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

**6. ábra: VMware tooVMware replikáció**

![VMware tooVMware](./media/site-recovery-components/vmware-to-vmware.png)



## <a name="hyper-v-tooa-secondary-site"></a>Hyper-V tooa másodlagos helyen

Ez a Hyper-V virtuális gépek tooa másodlagos helyre replikálni kell.


**Terület** | **Összetevő** | **Részletek**
--- | --- | ---
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

**7. ábra: A VMM tooVMM replikáció**

![A helyszíni tooon helyszíni](./media/site-recovery-components/arch-onprem-onprem.png)

### <a name="failover-and-failback"></a>Feladatátvétel és feladat-visszavétel

1. Futtathat tervezett vagy nem tervezett [feladatátvételt](site-recovery-failover.md) a helyszíni helyek között. Ha a tervezett feladatátvétel végrehajtása, majd a forrás virtuális gépeket állítsa le az tooensure adatvesztés nélküli.
2. Egyetlen gép feladatátvételt, vagy hozzon létre [helyreállítási tervek](site-recovery-create-recovery-plans.md) tooorchestrate több gép feladatátvétele.
4. Ha egy nem tervezett feladatátvétel tooa másodlagos hely hello feladatátvevő gépekhez hello másodlagos helyen nincsenek engedélyezve a védelem vagy replikáció után hajtható végre. Ha egy tervezett feladatátvételt hello feladatátvétel után már futott, védett gépek hello másodlagos helyen.
5. Ezt követően véglegesítse a hello feladatátvételi toostart elérése során hello munkaterhelés VM hello replikából.
6. Az elsődleges hely újra nem érhető el, akkor hello másodlagos hely toohello elsődleges a visszirányú replikálás tooreplicate kezdeményezni. Visszirányú replikálás során hello virtuális gépek kerülnek egy védett állapotban, de hello másodlagos adatközpontba még mindig aktív hely hello.
7. toomake hello elsődleges hely aktív helyre hello újra, elindít egy tervezett feladatátvételt a másodlagos tooprimary, egy másik visszirányú replikálás követ.


## <a name="next-steps"></a>Következő lépések

- [További](site-recovery-hyper-v-azure-architecture.md) hello Hyper-V replikáció munkafolyamat kapcsolatban.
- [Előfeltételek ellenőrzése](site-recovery-prereq.md)
