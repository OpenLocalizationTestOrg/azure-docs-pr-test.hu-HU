---
title: "Tekintse át a VMware-replikáció az Azure architektúra |} Microsoft Docs"
description: "Ez a cikk áttekintést összetevők és használható, ha a helyszíni VMware virtuális gépek replikálása Azure-bA az Azure Site Recovery szolgáltatással architektúra"
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
ms.openlocfilehash: 4aae04a793bab11562c20ceec0e1ae8f1a035a0f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="step-1-review-the-architecture-for-vmware-replication-to-azure"></a>1. lépés:, Tekintse át a VMware-replikáció az Azure architektúra

Ez a cikk ismerteti az összetevők és a folyamatok használható, ha a helyszíni VMware virtuális gépek replikálása az Azure használatával a [Azure Site Recovery](site-recovery-overview.md) szolgáltatás.

Megjegyzéseket a cikk alján tehet, kérdéseket pedig az [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) teheti fel.


## <a name="architectural-components"></a>Az architektúra összetevői

A táblázat foglalja össze a szükséges összetevők.

**Összetevő** | **Követelmény** | **Részletek**
--- | --- | ---
**Azure** | Azure-előfizetéssel, egy Azure storage-fiókot és egy Azure-hálózatra van szüksége. | A tárfiók tárolja a replikált adatok. Azure virtuális gépek jönnek létre a replikált adatokat a helyszíni helyről feladatátvétel esetén. Az Azure virtuális gépek a létrejöttükkor csatlakoznak az Azure virtuális hálózathoz.
**Konfigurációs kiszolgáló** | Egyetlen helyszíni felügyeleti kiszolgáló (a VMware virtuális gép) rendszert futtató összes az helyszíni Site Recovery összetevőket a telepítéshez. Ezek közé tartozik a konfigurációs kiszolgáló, a folyamatkiszolgáló, a fő célkiszolgálón. | A konfigurációs kiszolgáló összetevő koordinálja a helyszíni rendszer és az Azure közötti kommunikációt, és felügyeli az adatreplikációt.
 **Folyamatkiszolgáló**:  | Alapértelmezés szerint telepítve van a konfigurációs kiszolgálón. | Replikációs átjáróként üzemel. Fogadja a replikációs adatokat, gyorsítótárazással, tömörítéssel és titkosítással optimalizálja őket, majd továbbítja az Azure Storage-nak.<br/><br/> A folyamatkiszolgáló ezenfelül kezeli a mobilitási szolgáltatás leküldéses telepítését a védett gépekre, és elvégzi a WMware virtuális gépek automatikus felderítését.<br/><br/> Az üzemelő példány bővülésével további, önálló és dedikált folyamatkiszolgálókat helyezhet üzembe, amelyek segítségével képes lesz a megnövekedett replikációs forgalom kezelésére is.
 **Fő célkiszolgáló** | Alapértelmezés szerint telepítve van a helyszíni konfigurációs kiszolgálón. | Az Azure-ból történő feladat-visszavétel során kezeli a replikációs adatokat.<br/><br/> Ha a feladat-visszavételi adatforgalom kötetei nagyok, a feladat-visszavételhez üzembe helyezhet egy másik fő célkiszolgálót.
**VMware-kiszolgálók** | A VMware virtuális gépek vSphere ESXi-kiszolgálókon futnak, és a gazdagépek felügyeletéhez egy vCenter-kiszolgáló üzembe helyezését javasoljuk. | Vegyen fel VMware-kiszolgálókat a Recovery Services-tárolóba.
**Replikált gépek** | A mobilitási szolgáltatás telepíti az egyes VMware virtuális gépek replikálása. A szolgáltatás manuálisan is telepíthető az egyes gépekre, de leküldéses telepítés is végrehajtható a folyamatkiszolgálóról.

**1. ábra: Összetevők: VMware-ről Azure-ra**

![Összetevők](./media/vmware-walkthrough-architecture/arch-enhanced.png)

## <a name="replication-process"></a>Replikációs folyamat

1. A központi telepítését, beleértve a helyszíni és az Azure összetevők beállítása. A Recovery Services-tárolónak kell megadni a forrásként és célként, állítsa be a konfigurációs kiszolgáló, hozzon létre egy replikációs házirendet, telepíteni a mobilitási szolgáltatást, engedélyezze a replikálást és feladatátvételi teszt futtatása.
2. Gépek replikálásához a replikációs házirendet, és egy kezdeti másolatot készít az adatokat a rendszer replikálja az Azure storage.
3. Kezdeti replikáció befejezését követően, a változási különbözeteket az Azure-bA replikálását kezdődik. A gépek nyomon követett módosításait a rendszer egy .hrl fájlban tárolja.
    - A replikációs folyamat kezelése érdekében a replikálást végző gépek a 443-as bejövő HTTPS-porton kommunikálnak a konfigurációs kiszolgálóval.
    - Replikáló gépek küldhet-e adatokat a folyamatkiszolgálónak HTTPS 9443 porton bejövő (módosítható).
    - Az Azure-replikációs folyamat kezelését a konfigurációs kiszolgáló a 443-as kimenő HTTPS-porton keresztül végzi el.
    - A folyamatkiszolgáló adatokat fogad a forrásgépekről, amelyeket optimalizál és titkosít, majd a 443-as kimenő porton küldi elküld az Azure-tárolóba.
    - Ha engedélyezve van a több virtuális gépre kiterjedő konzisztencia, a replikációs csoportban található gépek a 20004-es porton kommunikálnak egymással. Több virtuális gépes környezetről akkor beszélünk, ha a gépek feladatátvételkor azonos összeomlásbiztos és alkalmazáskonzisztens helyreállítási pontokat használó replikációs csoportokba vannak rendezve. Ez akkor lehet hasznos, ha a gépek ugyanazt a számítási feladatot futtatják, így konzisztensnek kell maradniuk.
4. Az adatforgalmat a rendszer az interneten keresztül az Azure Storage nyilvános végpontjaira replikálja. Erre a célra az Azure ExpressRoute [nyilvános társviszony-létesítési](../expressroute/expressroute-circuit-peerings.md#public-peering) szolgáltatását is használhatja. Az adatforgalmat helyszíni helyek és az Azure között helyek közötti VPN-en keresztül nem lehet replikálni.


**2. ábra: Replikáció VMware-ről Azure-ra**

![Továbbfejlesztett](./media/vmware-walkthrough-architecture/v2a-architecture-henry.png)

## <a name="failover-and-failback-process"></a>Feladatátvételi és feladat-visszavételi folyamat

1. Miután ellenőrizte, hogy a feladatátvételi teszt a várt módon működik-e, igény szerint nem tervezett feladatátvételeket futtathat az Azure-hoz. A tervezett feladatátvétel nem támogatott.
2. Elvégezheti egyetlen gép feladatátvételét, vagy létrehozhat több virtuális gép feladatátvételét tartalmazó [helyreállítási terveket](site-recovery-create-recovery-plans.md) is.
3. Feladatátvétel futtatásakor az Azure-ban replikaként létrehozott virtuális gépek jönnek létre. Akkor kell feladatátvételt végezni, ha hozzá szeretne férni a replikaként létrehozott Azure virtuális gép számítási feladataihoz.
4. Amint az elsődleges helyszíni hely megint elérhetővé válik, visszaadhatja a feladatokat. Ehhez be kell állítani a feladat-visszavételi infrastruktúrát, el kell kezdeni a gép replikálását a másodlagos helyről az elsődlegesre, valamint nem tervezett feladatátvételt kell futtatni a másodlagos helyről. Ezen feladatátvétel végrehajtását követően az adatok visszakerülnek a helyszíni helyre, és az Azure-ba történő replikációt újra engedélyezni kell. [További információ](site-recovery-failback-azure-to-vmware.md)

A feladat-visszavételre vonatkozó követelmények:


- **Ideiglenes folyamatkiszolgáló az Azure-ban**: ha feladatátvételt követően szeretné visszaadni a feladatokat az Azure-ból, be kell állítania egy folyamatkiszolgálóként üzemelő Azure virtuális gépet, amely kezeli az Azure-ból való replikációt. Ez a virtuális gép a feladatok visszaadását követően törölhető.
- **VPN-kapcsolat**: a feladat-visszavételhez szüksége lesz egy VPN-kapcsolatra (vagy Azure ExpressRoute-ra), amelyet a helyszíni hely Azure-hálózatában kell beállítania.
- **Önálló helyszíni fő célkiszolgáló**: a helyszíni fő célkiszolgáló kezeli a feladat-visszavételt. A fő célkiszolgálót alapértelmezés szerint a felügyeleti kiszolgálóra kell telepíteni, de nagyobb mértékű forgalom feladat-visszavétele esetén érdemes ebből a célból egy önálló helyszíni fő célkiszolgálót is létrehozni.
- **Feladat-visszavételi szabályzat**: A helyszíni helyre történő újbóli replikáláshoz feladat-visszavételi szabályzatra van szükség. A replikációs szabályzat létrehozásakor a rendszer ezt automatikusan létrehozza.
- **VMware-infrastruktúra**: A feladatot egy helyszíni VMware virtuális gépnek kell visszavennie. Ez azt jelenti, hogy helyszíni VMware-infrastruktúrára akkor is szükség van, ha helyszíni fizikai kiszolgálókat replikál az Azure-ba.

**3. ábra: VMware-/fizikai gépek közötti feladat-visszavétel**

![Feladat-visszavétel](./media/vmware-walkthrough-architecture/enhanced-failback.png)


## <a name="next-steps"></a>Következő lépések

Ugrás a [2. lépés: Ellenőrizze az Előfeltételek és korlátozások](vmware-walkthrough-prerequisites.md)
