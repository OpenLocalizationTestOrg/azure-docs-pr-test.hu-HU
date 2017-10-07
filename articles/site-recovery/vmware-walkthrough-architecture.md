---
title: "a VMware-replikáció tooAzure aaaReview hello architektúra |} Microsoft Docs"
description: "Ez a cikk áttekintést összetevők és a helyszíni VMware virtuális gépek tooAzure hello Azure Site Recovery szolgáltatásban való replikálása esetén használt architektúra"
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
ms.topic: article
ms.date: 06/27.017
ms.author: raynew
ms.openlocfilehash: 7acb1d8e83a846dca79e175fd1af9324f2c31e65
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-1-review-hello-architecture-for-vmware-replication-tooazure"></a>1. lépés:, Tekintse át a VMware-replikáció tooAzure hello architektúrája

Ez a cikk ismerteti a hello összetevőiről és folyamataitól használható, ha replikálása a helyszíni VMware virtuális gépek tooAzure hello segítségével [Azure Site Recovery](site-recovery-overview.md) szolgáltatás.

Ez a cikk hello alján a megjegyzéseket, vagy kérdései vannak a hello [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="architectural-components"></a>Az architektúra összetevői

hello táblázat hello összetevőket foglalja össze.

**Összetevő** | **Követelmény** | **Részletek**
--- | --- | ---
**Azure** | Azure-előfizetéssel, egy Azure storage-fiókot és egy Azure-hálózatra van szüksége. | A replikált adatok hello tárfiók tárolja. Azure virtuális gépek replikálása hello adatokkal jönnek létre, a helyszíni helyről feladatátvétel esetén. hello Azure virtuális gépek Azure-beli virtuális hálózat toohello csatlakozás, ha a jönnek létre.
**Konfigurációs kiszolgáló** | Egyetlen helyszíni felügyeleti kiszolgáló (a VMware virtuális gép) rendszert futtató összes hello helyszíni Site Recovery összetevők hello központi telepítéshez. Ezek közé tartozik a konfigurációs kiszolgáló, a folyamatkiszolgáló, a fő célkiszolgálón. | hello konfigurációs kiszolgáló összetevő koordinálja a helyszíni és az Azure közötti kommunikáció és kezeli a replikációs adatokat.
 **Folyamatkiszolgáló**:  | Hello konfigurációs kiszolgálón alapértelmezés szerint telepítve. | Replikációs átjáróként üzemel. Kap replikációs adatokat, gyorsítótárazás, tömörítés és titkosítás segítségével optimalizálja őket, és visszaküldi azt tooAzure tárolási.<br/><br/> hello folyamatkiszolgáló is leküldéses telepítését hello mobilitási szolgáltatás tooprotected gépek kezeli, és végrehajtja a VMware virtuális gépek automatikus észlelése.<br/><br/> A központi telepítés növekedésével további külön dedikált folyamat kiszolgálók toohandle növelése replikációs forgalom mennyisége is hozzáadhat.
 **Fő célkiszolgáló** | Hello helyszíni konfigurációs kiszolgáló alapértelmezés szerint telepítve. | Az Azure-ból történő feladat-visszavétel során kezeli a replikációs adatokat.<br/><br/> Ha a feladat-visszavételi adatforgalom kötetei nagyok, a feladat-visszavételhez üzembe helyezhet egy másik fő célkiszolgálót.
**VMware-kiszolgálók** | VMware virtuális gépeket üzemeltető vSphere ESXi-kiszolgálón, és azt javasoljuk a vCenter server toomanage hello gazdagépek. | VMware kiszolgálók tooyour Recovery Services-tároló hozzáadása
**Replikált gépek** | hello mobilitási szolgáltatás telepíti az egyes VMware virtuális gépek replikálása. Telepíthető manuálisan minden számítógépen, vagy a leküldéses telepítés hello folyamat kiszolgálóról.

**1. ábra: VMware tooAzure összetevők**

![Összetevők](./media/vmware-walkthrough-architecture/arch-enhanced.png)

## <a name="replication-process"></a>Replikációs folyamat

1. Hello központi telepítését, beleértve a helyszíni és az Azure összetevők beállítása. A Recovery Services-tároló hello megadhatja a hello replikációs forrása és célja, hello konfigurációs kiszolgáló, hozzon létre egy replikációs házirendet, hello mobilitási szolgáltatás üzembe helyezése, engedélyezze a replikálást és feladatátvételi teszt futtatása.
2. Gépek replikálásához hello replikációs házirendet, és egy kezdeti másolatot készít hello adatok megfelelően az replikált tooAzure tároló.
3. Kezdeti replikáció befejezését követően, a különbözeti módosítások tooAzure replikálása kezdődik. A gépek nyomon követett módosításait a rendszer egy .hrl fájlban tárolja.
    - Replikáló gépek kommunikálni hello konfigurációs kiszolgáló HTTPS 443-as portot a bejövő, a replikáció kezelését.
    - Replikáló gépek küldése replikációs adatok toohello folyamatkiszolgáló HTTPS 9443 porton bejövő (módosítható).
    - hello konfigurációs kiszolgáló koordinálja a replikáció kezelését az Azure-ral HTTPS 443-as kimenő porton keresztül.
    - hello folyamatkiszolgáló adatokat fogad az forrásgépek, optimalizálja és titkosítja azokat, és elküldi azt tooAzure tárolási 443-as porton keresztül kimenő.
    - Ha engedélyezi a virtuális Gépre kiterjedő konzisztencia, majd gépek hello replikációs csoport kommunikálnak egymással 20004 porton keresztül. Több virtuális gépes környezetről akkor beszélünk, ha a gépek feladatátvételkor azonos összeomlásbiztos és alkalmazáskonzisztens helyreállítási pontokat használó replikációs csoportokba vannak rendezve. Ez akkor hasznos, ha a gépek is működnek, ugyanaz az alkalmazás hello és toobe konzisztens kell.
4. Akkor replikált tooAzure tárolási nyilvános végpontok, több mint hello internet. Erre a célra az Azure ExpressRoute [nyilvános társviszony-létesítési](../expressroute/expressroute-circuit-peerings.md#public-peering) szolgáltatását is használhatja. Forgalom replikál egy helyszíni hely tooAzure az a pont-pont VPN-kapcsolaton keresztül nem támogatott.


**2. ábra: VMware tooAzure replikáció**

![Továbbfejlesztett](./media/vmware-walkthrough-architecture/v2a-architecture-henry.png)

## <a name="failover-and-failback-process"></a>Feladatátvételi és feladat-visszavételi folyamat

1. Miután meggyőződött a feladatátvételi teszt a várt módon működik, a nem tervezett feladatátvételek tooAzure szükség szerint is futtathatja. A tervezett feladatátvétel nem támogatott.
2. Egyetlen gép feladatátvételt, vagy hozzon létre [helyreállítási tervek](site-recovery-create-recovery-plans.md), toofail át több virtuális géphez.
3. Feladatátvétel futtatásakor az Azure-ban replikaként létrehozott virtuális gépek jönnek létre. A feladatátvételi toostart elérése során hello munkaterhelés hello replikáról Azure virtuális gép véglegesítése után.
4. Amint az elsődleges helyszíni hely megint elérhetővé válik, visszaadhatja a feladatokat. A feladat-visszavétel infrastruktúra beállítása, start hello gépek replikálásához hello másodlagos hely toohello elsődleges és egy nem tervezett feladatátvételt hello másodlagos helyekről futtatni. Ennél a feladatátvételnél véglegesítést követően adatokat kell vissza a helyszíni, és tooenable replikációs tooAzure újra kell. [További információ](site-recovery-failback-azure-to-vmware.md)

A feladat-visszavételre vonatkozó követelmények:


- **Ideiglenes folyamatkiszolgáló az Azure-ban**: Ha feladatátvételt követően az Azure-ból toofail szüksége lesz egy Azure virtuális gépet a konfigurálva folyamatot, az Azure-ból toohandle replikációs tooset. Ez a virtuális gép a feladatok visszaadását követően törölhető.
- **VPN-kapcsolat**: A feladat-visszavételre szüksége lesz egy VPN-kapcsolat (vagy Azure ExpressRoute) beállítása hello Azure hálózati toohello a helyszíni helyről.
- **Önálló helyszíni fő célkiszolgáló**: hello a helyszíni fő célkiszolgáló kezeli a feladat-visszavételre. fő célkiszolgáló hello hello felügyeleti kiszolgálón alapértelmezés szerint telepítve van, de nagyobb mértékű forgalom visszavétele esetén érdemes beállítania egy önálló helyszíni fő célkiszolgáló erre a célra.
- **Feladat-visszavételi házirend**: tooreplicate hátsó tooyour helyszíni hely, egy feladatátvételi házirendhez van szüksége. A replikációs szabályzat létrehozásakor a rendszer ezt automatikusan létrehozza.
- **VMware-infrastruktúra**: meg kell a feladat-visszavételt tooan helyszíni VMware virtuális gép. Ez azt jelenti, hogy szükséges egy helyszíni VMware-infrastruktúra, akkor is, ha a helyszíni fizikai kiszolgálók tooAzure replikál.

**3. ábra: VMware-/fizikai gépek közötti feladat-visszavétel**

![Feladat-visszavétel](./media/vmware-walkthrough-architecture/enhanced-failback.png)


## <a name="next-steps"></a>Következő lépések

Nyissa meg túl[2. lépés: Ellenőrizze az Előfeltételek és korlátozások](vmware-walkthrough-prerequisites.md)
