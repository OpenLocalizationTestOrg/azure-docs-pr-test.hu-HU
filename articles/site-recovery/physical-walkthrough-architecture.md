---
title: "az Azure Site Recovery segítségével a fizikai kiszolgáló replikációs tooAzure aaaReview hello architektúra |} Microsoft Docs"
description: "Ez a cikk áttekintést összetevők és a helyszíni fizikai kiszolgálók tooAzure hello Azure Site Recovery szolgáltatásban való replikálása esetén használt architektúra"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 09c9b136-35f5-465e-8d0f-f4c5d5d9f880
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: 4f334c181fa6f0821adf978ebe9dbd7d36a3c753
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-1-review-hello-architecture-for-physical-server-replication-tooazure"></a>1. lépés:, Tekintse át a fizikai kiszolgáló replikációs tooAzure hello architektúrája

Ez a cikk ismerteti a hello összetevőiről és folyamataitól a replikált helyszíni windowsos/Linuxos fizikai kiszolgálók tooAzure hello segítségével használt [Azure Site Recovery](site-recovery-overview.md) szolgáltatás.

Ez a cikk hello alján a megjegyzéseket, vagy kérdései vannak a hello [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="architectural-components"></a>Az architektúra összetevői

hello táblázat hello összetevőket foglalja össze.

**Összetevő** | **Követelmény** | **Részletek**
--- | --- | ---
**Azure** | Az Azure-fiók, egy Azure storage-fiókot és egy Azure-hálózatra van szükség. | A replikált adatok hello tárfiók tárolja, és Azure virtuális gépek replikálása hello adatokkal jönnek létre, ha a feladatátvételt hajt végre. Azure virtuális gépek Azure-beli virtuális hálózat toohello csatlakozás, ha a jönnek létre.
**Konfigurációs kiszolgáló** | Egyetlen helyszíni felügyeleti kiszolgáló (a fizikai kiszolgáló vagy a VMware virtuális gép) rendszert futtató összes hello helyszíni Site Recovery-összetevőkhöz. Ezek közé tartozik a konfigurációs kiszolgáló, a folyamatkiszolgáló, a fő célkiszolgálón. | hello konfigurációs kiszolgáló összetevő koordinálja a helyszíni és az Azure közötti kommunikáció és kezeli a replikációs adatokat.
 **Folyamatkiszolgáló**  | Hello konfigurációs kiszolgálón alapértelmezés szerint telepítve. | Replikációs átjáróként üzemel. Kap replikációs adatokat, gyorsítótárazás, tömörítés és titkosítás segítségével optimalizálja őket, és visszaküldi azt tooAzure tárolási.<br/><br/> hello folyamatkiszolgáló is kezeli az ügyfélleküldéses telepítés hello mobilitási szolgáltatás tooprotected gépek.<br/><br/> Hozzáadhat további külön dedikált folyamat kiszolgálókat, toohandle növelése replikációs forgalom mennyisége.
 **Fő célkiszolgáló** | Hello helyszíni konfigurációs kiszolgáló alapértelmezés szerint telepítve. | Az Azure-ból történő feladat-visszavétel során kezeli a replikációs adatokat.<br/><br/> Ha a feladat-visszavételi adatforgalom kötetei nagyok, a feladat-visszavételhez üzembe helyezhet egy másik fő célkiszolgálót.
**A replikált kiszolgálók** | hello mobilitási szolgáltatás összetevőt tooreplicate kívánt Windows/Linux-kiszolgálókon. Telepíthető manuálisan minden számítógépen, vagy a leküldéses telepítés hello folyamat kiszolgálóról.
**Feladat-visszavétel** | A feladat-visszavételre VMware-infrastruktúrára szükség. Nem utasíthat el hátsó tooa fizikai kiszolgálón.


**1. ábra: Fizikai tooAzure összetevők**

![Összetevők](./media/physical-walkthrough-architecture/arch-enhanced.png)

## <a name="replication-process"></a>Replikációs folyamat

1. Hello központi telepítését, beleértve a helyszíni és az Azure összetevők beállítása. A Recovery Services-tároló hello megadhatja a hello replikációs forrása és célja, hello konfigurációs kiszolgáló, hozzon létre egy replikációs házirendet, hello mobilitási szolgáltatás üzembe helyezése, engedélyezze a replikálást és feladatátvételi teszt futtatása.
2.  Gépek replikálásához hello replikációs házirendet, és egy kezdeti másolatot készít hello adatok megfelelően az replikált tooAzure tároló.
4. Kezdeti replikáció befejezését követően, a különbözeti módosítások tooAzure replikálása kezdődik. A gépek nyomon követett módosításait a rendszer egy .hrl fájlban tárolja.
    - Replikáló gépek kommunikálni hello konfigurációs kiszolgáló HTTPS 443-as portot a bejövő, a replikáció kezelését.
    - Replikáló gépek küldése replikációs adatok toohello folyamatkiszolgáló HTTPS 9443 porton bejövő (módosítható).
    - hello konfigurációs kiszolgáló koordinálja a replikáció kezelését az Azure-ral HTTPS 443-as kimenő porton keresztül.
    - hello folyamatkiszolgáló adatokat fogad az forrásgépek, optimalizálja és titkosítja azokat, és elküldi azt tooAzure tárolási 443-as porton keresztül kimenő.
    - Ha engedélyezi a virtuális Gépre kiterjedő konzisztencia, majd gépek hello replikációs csoport kommunikálnak egymással 20004 porton keresztül. Több virtuális gépes környezetről akkor beszélünk, ha a gépek feladatátvételkor azonos összeomlásbiztos és alkalmazáskonzisztens helyreállítási pontokat használó replikációs csoportokba vannak rendezve. Ez akkor hasznos, ha a gépek is működnek, ugyanaz az alkalmazás hello és toobe konzisztens kell.
5. Akkor replikált tooAzure tárolási nyilvános végpontok, több mint hello internet. Erre a célra az Azure ExpressRoute [nyilvános társviszony-létesítési](../expressroute/expressroute-circuit-peerings.md#public-peering) szolgáltatását is használhatja. Forgalom replikál egy helyszíni hely tooAzure az a pont-pont VPN-kapcsolaton keresztül nem támogatott.

**2. ábra: Fizikai tooAzure replikáció**

![Továbbfejlesztett](./media/physical-walkthrough-architecture/v2a-architecture-henry.png)

## <a name="failover-and-failback-process"></a>Feladatátvételi és feladat-visszavételi folyamat

Miután meggyőződött a feladatátvételi teszt a várt módon működik, a nem tervezett feladatátvételek tooAzure szükség szerint is futtathatja. Amikor feladatátvételt, Azure virtuális gépek replikált adatokból jönnek létre. Ezt követően a helyszíni elsődleges hely újra nem érhető el, akkor is feladat-visszavételt. Vegye figyelembe:

- A tervezett feladatátvétel nem támogatott.
- Egyetlen gép feladatátvételt, vagy hozzon létre [helyreállítási tervek](site-recovery-create-recovery-plans.md), egyszerre több gépeket toofail.
- A feladatátvételi toostart elérése során hello munkaterhelés hello replikáról Azure virtuális gép véglegesítése után.
- Hello elsődleges hely újra nem érhető el, hello gép replikálása hello másodlagos toohello elsődleges helyről. Hello másodlagos helyről, majd futtassa a nem tervezett feladatátvételre. Ennél a feladatátvételnél véglegesítést követően adatokat kell vissza a helyszíni, és tooenable replikációs tooAzure újra kell.

Feladat-visszavétel összetevői az alábbiak:

- **Ideiglenes folyamatkiszolgáló az Azure-ban**: egy Azure virtuális gép tooact folyamat-kiszolgálóként, az Azure-ból toohandle replikációs tooset van szüksége. Ez a virtuális gép a feladatok visszaadását követően törölhető.
- **VPN-kapcsolat**: a VPN-kapcsolat (vagy Azure ExpressRoute) szükséges hello Azure hálózati toohello a helyszíni helyről.
- **Önálló helyszíni fő célkiszolgáló**: hello a helyszíni fő célkiszolgáló (hello konfigurációs kiszolgálón alapértelmezés szerint telepítve) kezeli a feladat-visszavételre. Ha nem forgalom vissza nagy mennyiségű, érdemes beállítania egy önálló helyszíni fő célkiszolgáló erre a célra.
- **Feladat-visszavételi házirend**: egy feladatátvételi házirendhez van szüksége. A replikációs szabályzat létrehozásakor a rendszer ezt automatikusan létrehozza.
- **VMware-infrastruktúra**: meg kell a feladat-visszavételt tooan helyszíni VMware virtuális gép. Ez azt jelenti, hogy szükséges egy helyszíni VMware-infrastruktúra, akkor is, ha a helyszíni fizikai kiszolgálók tooAzure replikál.

**3. ábra: Fizikai kiszolgáló feladat-visszavétel**

![Feladat-visszavétel](./media/physical-walkthrough-architecture/enhanced-failback.png)


## <a name="next-steps"></a>Következő lépések

Nyissa meg túl[2. lépés: Ellenőrizze az Előfeltételek és korlátozások](physical-walkthrough-prerequisites.md)
