---
title: "aaaHow nem VMware replikációs tooAzure munka az Azure Site Recovery? | Microsoft Docs"
description: "Ez a cikk áttekintést összetevők és használható, ha replikálása a helyszíni VMware virtuális gépek és fizikai kiszolgálók tooAzure a hello Azure Site Recovery szolgáltatás architektúrája"
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
ROBOTS: NOINDEX, NOFOLLOW
redirect_url: vmware-walkthrough-architecture
ms.openlocfilehash: f0fb834f8b251640f97e4d0163b2b9e54de691e1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-does-vmware-replication-tooazure-work-in-site-recovery"></a>Hogyan működik a VMware-replikáció tooAzure a Site Recovery?

Ez a cikk ismerteti a hello összetevők és a folyamatok replikálása esetén a helyszíni VMware virtuális gépek és a windowsos/Linuxos fizikai kiszolgálók, tooAzure hello segítségével [Azure Site Recovery](site-recovery-overview.md) szolgáltatás.

A replikált fizikai, helyszíni kiszolgálók tooAzure replikáció által használt is hello azonos összetevőiről és folyamataitól, a VMware Virtuálisgép-replikációt, ezek a különbségek:

- Hello konfigurációs kiszolgáló, a VMware virtuális gépek helyett egy fizikai kiszolgálót is használhatja.
- A feladat-visszavételhez helyszíni VMware-infrastruktúrára van szükség. Nem utasíthat el hátsó tooa fizikai gépen.

Ez a cikk hello alján a megjegyzéseket, vagy kérdései vannak a hello [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="architectural-components"></a>Az architektúra összetevői

Számos összetevők érintett VMware virtuális gépek és fizikai kiszolgálók tooAzure replikálása esetén.

**Összetevő** | **Hely** | **Részletek**
--- | --- | ---
**Azure** | Az Azure-ban szüksége van egy Azure-fiókra, egy Azure Storage-fiókra és egy Azure-hálózatra. | Hello tárfiók tárolja a replikált adatok, és az Azure virtuális gépek replikálása hello adatokkal jönnek létre, ha a feladatátvétel a helyszíni helyről. hello Azure virtuális gépek Azure-beli virtuális hálózat toohello csatlakozás, ha a jönnek létre.
**Konfigurációs kiszolgáló** | Egyetlen helyszíni hello helyszíni összetevők hello központi telepítését, beleértve a hello konfigurációs kiszolgáló, a folyamatkiszolgáló, a fő célkiszolgálón a szükséges futtató felügyeleti kiszolgálón (a VMWare virtuális gép) | hello konfigurációs kiszolgáló összetevő koordinálja a helyszíni és az Azure közötti kommunikáció és kezeli a replikációs adatokat.
 **Folyamatkiszolgáló**:  | Hello konfigurációs kiszolgálón alapértelmezés szerint telepítve. | Replikációs átjáróként üzemel. Kap replikációs adatokat, gyorsítótárazás, tömörítés és titkosítás segítségével optimalizálja őket, és visszaküldi azt tooAzure tárolási.<br/><br/> hello folyamatkiszolgáló is leküldéses telepítését hello mobilitási szolgáltatás tooprotected gépek kezeli, és végrehajtja a VMware virtuális gépek automatikus észlelése.<br/><br/> A központi telepítés növekedésével további külön dedikált folyamat kiszolgálók toohandle növelése replikációs forgalom mennyisége is hozzáadhat.
 **Fő célkiszolgáló** | Hello helyszíni konfigurációs kiszolgáló alapértelmezés szerint telepítve. | Az Azure-ból történő feladat-visszavétel során kezeli a replikációs adatokat.<br/><br/> Ha a feladat-visszavételi adatforgalom kötetei nagyok, a feladat-visszavételhez üzembe helyezhet egy másik fő célkiszolgálót.
**VMware-kiszolgálók** | VMware virtuális gépeket üzemeltető vSphere ESXi-kiszolgálón, és azt javasoljuk a vCenter server toomanage hello gazdagépek. | VMware kiszolgálók tooyour Recovery Services-tároló hozzáadása
**Replikált gépek** | hello mobilitási szolgáltatás telepíti az egyes VMware virtuális gép kívánt tooreplicate. Telepíthető manuálisan minden számítógépen, vagy a leküldéses telepítés hello folyamat kiszolgálóról.

Hello telepítés előfeltételeit és ezeket az összetevőket a hello követelményeiről további [támogatási mátrix](site-recovery-support-matrix-to-azure.md).

**1. ábra: VMware tooAzure összetevők**

![Összetevők](./media/site-recovery-components/arch-enhanced.png)

## <a name="replication-process"></a>Replikációs folyamat

1. Hello központi telepítését, beleértve az Azure összetevőket, és a Recovery Services-tároló beállítása. Hello tároló hello replikációs forrása és célja, állítsa be a konfigurációs kiszolgálón hello VMware-kiszolgálók hozzáadása, hozzon létre egy replikációs házirendet, hello mobilitási szolgáltatás üzembe helyezése, engedélyezze a replikálást és feladatátvételi teszt futtatása kell megadni.
2.  Gépek hello replikációs nyilatkozatnak replikálást indítani, és egy kezdeti másolatot készít hello adatok replikált tooAzure tároló.
4. Különbözeti módosítások tooAzure replikálása hello kezdeti replikáció befejezését követően megkezdődik. A gépek nyomon követett módosításait a rendszer egy .hrl fájlban tárolja.
    - Replikáló gépek kommunikálni hello konfigurációs kiszolgáló HTTPS 443-as portot a bejövő, a replikáció kezelését.
    - Replikáló gépek küldése replikációs adatok toohello folyamatkiszolgáló HTTPS 9443 porton bejövő (konfigurálható).
    - hello konfigurációs kiszolgáló koordinálja a replikáció kezelését az Azure-ral HTTPS 443-as kimenő porton keresztül.
    - hello folyamatkiszolgáló adatokat fogad az forrásgépek, optimalizálja és titkosítja azokat, és elküldi azt tooAzure tárolási 443-as porton keresztül kimenő.
    - Ha engedélyezi a virtuális Gépre kiterjedő konzisztencia, majd gépek hello replikációs csoport kommunikálnak egymással 20004 porton keresztül. Több virtuális gépes környezetről akkor beszélünk, ha a gépek feladatátvételkor azonos összeomlásbiztos és alkalmazáskonzisztens helyreállítási pontokat használó replikációs csoportokba vannak rendezve. Ez akkor hasznos, ha a gépek is működnek, ugyanaz az alkalmazás hello és toobe konzisztens kell.
5. Akkor replikált tooAzure tárolási nyilvános végpontok, több mint hello internet. Erre a célra az Azure ExpressRoute [nyilvános társviszony-létesítési](../expressroute/expressroute-circuit-peerings.md#public-peering) szolgáltatását is használhatja. Forgalom replikál egy helyszíni hely tooAzure az a pont-pont VPN-kapcsolaton keresztül nem támogatott.

**2. ábra: VMware tooAzure replikáció**

![Továbbfejlesztett](./media/site-recovery-components/v2a-architecture-henry.png)

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

![Feladat-visszavétel](./media/site-recovery-components/enhanced-failback.png)


## <a name="next-steps"></a>Következő lépések

Felülvizsgálati hello [támogatási mátrix](site-recovery-support-matrix-to-azure.md)
