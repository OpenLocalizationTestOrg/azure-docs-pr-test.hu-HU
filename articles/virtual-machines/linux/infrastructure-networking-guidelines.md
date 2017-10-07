---
title: "hálózati infrastruktúra irányelvek - Linux aaaAzure |} Microsoft Docs"
description: "Ismerje meg hello kulcs tervezési és megvalósítási irányelvek Azure infrastruktúra-szolgáltatásokat a virtuális hálózatkezelés telepítéséhez."
documentationcenter: 
services: virtual-machines-linux
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: a7ebd5da-3c62-45e8-ad90-6c73a37ebeb1
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f3f49b135b1f9bca3fd463e9ff27299fb97908c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-networking-infrastructure-guidelines-for-linux-vms"></a>Hálózati infrastruktúra irányelvek Linux virtuális gépek Azure

[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)]

Ez a cikk foglalkozik a ismertetése hello szükséges tervezési lépéseket az Azure virtuális hálózat és a kapcsolatot a meglévő helyszíni környezetek között.

## <a name="implementation-guidelines-for-virtual-networks"></a>Virtuális hálózatok implementációs segédlet
Döntéseket:

* Milyen típusú virtuális hálózat van szüksége toohost a informatikai munkaterhelés vagy az infrastrukturális (csak felhőalapú vagy létesítmények közötti)?
* A létesítmények közötti virtuális hálózatokra, mennyire címterének van szüksége toohost hello alhálózatok és virtuális gépek most és ésszerű kiterjesztésének hello a jövőbeli?
* Akkor is toocreate virtuális hálózatok központi, vagy hozzon létre egyéni virtuális hálózatok, az egyes erőforráscsoportokban?

Feladatok:

* Adja meg a létrehozott hello virtuális hálózatok toobe hello címtartomány.
* Az egyes alhálózatok és hello címterület hello a kulcstulajdonságokat határozza meg.
* A létesítmények közötti virtuális hálózatok hello helyszíni hely, amely a virtuális gépek hello hello virtuális hálózat szükséges tooreach a helyi hálózati-címterét hello a kulcstulajdonságokat határozza meg.
* A helyszíni hálózati team tooensure hello megfelelő útválasztási van konfigurálva, létrehozásakor munkahelyi létesítmények közötti virtuális hálózatok.
* Hello virtuális hálózat létrehozása az elnevezési konvenciót.

## <a name="virtual-networks"></a>Virtuális hálózatok
Virtuális hálózatok közötti virtuális gépek (VM) szükséges toosupport kommunikációs találhatók. Adja meg az alhálózat, a egyéni IP-cím, a DNS-beállítások, a biztonsági szűrést, és a terheléselosztás, csakúgy, mint a fizikai hálózatok. Használatával egy [VPN-átjáró](../../vpn-gateway/vpn-gateway-about-vpngateways.md) vagy [Express Route-körhöz](../../expressroute/expressroute-introduction.md), csatlakoztathatja egy Azure virtuális hálózatot tooyour a helyszíni hálózatokban. További tudnivalók [virtuális hálózatok és az összetevők](../../virtual-network/virtual-networks-overview.md).

Erőforráscsoportok segítségével kialakításának a módját a virtuális hálózati összetevők beleszólása van. Virtuális gépek toovirtual hálózatok kívül a saját erőforráscsoportot is elérheti. A gyakori tervezési megközelítés toocreate központi erőforrás-csoportok, amelyek tartalmazzák az alapvető hálózati infrastruktúra, amely egy közös csapat fogja kezelni akkor is lehet. Virtuális gépek és az alkalmazások üzembe tooseparate erőforráscsoportok. Ez a megközelítés lehetővé teszi, hogy alkalmazástulajdonosok hozzáférés hozzáférés toohello konfiguráció hello szélesebb virtuális hálózati erőforrások megnyitása nélkül a virtuális gépeket tartalmazó erőforráscsoport toohello.

## <a name="site-connectivity"></a>Hely kapcsolatot
### <a name="cloud-only-virtual-networks"></a>Csak felhőalapú virtuális hálózatok
Ha a helyi felhasználók és számítógépek nem igényelnek folyamatos kapcsolatot tooVMs egy Azure virtuális hálózatban, a virtuális hálózati kialakítása közvetlen esetén:

![Alapszintű csak felhőalapú virtuális hálózati diagramja](./media/infrastructure-networking-guidelines/vnet01.png)

Ez a megközelítés általában az internetre a munkaterhelések, például az Internet alapú webkiszolgáló van. SSH vagy a pont-pont VPN-kapcsolatok használata virtuális gépeken is kezelheti.

Nem kapcsolódnak a helyszíni hálózat tooyour, mert csak Azure virtuális hálózatok használható bármely hello privát IP-címtér része. hello címterület lehet hello azonos titkos területet, amely a helyszíni használja.

### <a name="cross-premises-virtual-networks"></a>Virtuális hálózatok létesítmények közötti
Ha a helyi felhasználók és számítógépek igényelnek folyamatos kapcsolatot tooVMs egy Azure virtuális hálózatban, létesítmények közötti virtuális hálózat létrehozása. Csatlakozás hello virtuális hálózati tooyour a helyi hálózaton egy expressroute-on vagy a pont-pont VPN-kapcsolatot.

![Létesítmények közötti virtuális hálózati diagramja](./media/infrastructure-networking-guidelines/vnet02.png)

Ebben a konfigurációban hello Azure-beli virtuális hálózat lényegében egy felhőalapú bővítmény a helyszíni hálózat.

Mivel csatlakozó tooyour a helyszíni hálózat, létesítmények közötti virtuális hálózatok kell használnia, amely egyedi a szervezet által használt hello címterület része. A hello azonos módon, hogy különböző vállalati helyek vannak hozzárendelve a megadott IP-alhálózat, Azure egy másik helyre válik, mivel a hálózat kimenő kiterjeszti.

tooallow csomagok tootravel a létesítmények közötti virtuális hálózati tooyour helyszíni hálózatról, konfigurálnia kell vonatkozó helyszíni címelőtagokat hello készlete hello virtuális hálózat hello helyi hálózat definíciójának részeként. Attól függően, hogy hello cím hello virtuális hálózati és hello készlet megfelelő helyet a helyszíni helyeken, akkor lehet sok címelőtagokat hello helyi hálózathoz.

Átválthat egy kizárólag felhőalapú virtuális hálózati tooa létesítmények közötti virtuális hálózat, de nagy valószínűséggel igényel, toore IP-a virtuális hálózat címtere és az Azure-erőforrások. Ezért alaposan gondolja át, ha egy virtuális hálózati kell toobe csatlakoztatott tooyour a helyszíni hálózat IP-alhálózat rendel.

## <a name="subnets"></a>Alhálózatok
Alhálózatok teszik lehetővé, amelyek kapcsolódnak, vagy logikailag tooorganize erőforrások (például egy alhálózat virtuális gépekhez tartozó toohello ugyanazt az alkalmazást), vagy fizikailag (például az egyik alhálózatba erőforráscsoportra). A nagyobb biztonság alhálózati elkülönítési technológiája is alkalmazhat.

A létesítmények közötti virtuális hálózatok, akkor tervezzen hello alhálózat azonos egyezmények, amelyekkel a helyszíni erőforrások. **Azure mindig használja hello hello címtér az egyes alhálózatok első három IP-címek**. toodetermine hello szám címek hello alhálózati szükséges, indítsa el hello most igénylő virtuális gépek száma alapján. Becsléséhez a jövőbeli növekedésre, és használja a következő tábla toodetermine hello mérete hello alhálózati hello.

| Szükséges virtuális gépek száma | A szükséges állomás bitek száma | Hello alhálózat mérete |
| --- | --- | --- |
| 1–3 |3 |/29 |
| 4–11 |4 |/28 |
| 12–27 |5 |/27 |
| 28–59 |6 |/26 |
| 60–123 |7 |/25 |

> [!NOTE]
> Normál helyi alhálózatok, az n állomás bits alhálózat állomás címek maximális száma hello: 2<sup> n </sup> – 2. Egy Azure-alhálózathoz, az n állomás bits alhálózat állomás címek maximális száma hello: 2<sup> n </sup> – 5 (2 és 3 értéket az Azure által az egyes alhálózatokon hello címek).
> 
> 

Ha egy alhálózat mérete túl kicsi, toore-IP rendelkezik, és telepítse újra a hello hello alhálózat virtuális gépeinek.

## <a name="network-security-groups"></a>Network Security Groups (Hálózati biztonsági csoportok)
Szűrési szabályok toohello szolgáltatások haladó adatforgalom esetén a virtuális hálózatokon keresztül alkalmazhatja a hálózati biztonsági csoportok használatával. Hozhat létre a részletes szűrési szabályok toosecure a virtuális hálózati környezetben, szabályozása a bejövő és kimenő forgalmat, a forrás és a cél IP-címtartományai, engedélyezett portok stb. Hálózati biztonsági csoportok virtuális hálózatban vagy közvetlenül a megadott virtuális hálózati adapter tooa belül alkalmazott toosubnets lehet. Az ajánlott toohave bizonyos fokú szűrése a forgalmat a virtuális hálózatokon hálózati biztonsági csoport. További tudnivalók [hálózati biztonsági csoportok](../../virtual-network/virtual-networks-nsg.md).

## <a name="additional-network-components"></a>További hálózati összetevők
Egy helyszíni fizikai hálózati infrastruktúra, az Azure virtuális hálózat nem csak alhálózatokat és IP-címzés tartalmazhat. Az alkalmazás-infrastruktúra tervezésének, érdemes lehet a tooincorporate néhány további összetevők:

* [VPN-átjárók](../../vpn-gateway/vpn-gateway-about-vpngateways.md) -csatlakozás az Azure virtuális hálózatok tooother Azure virtuális hálózatok, vagy csatlakoztassa a tooon helyszíni hálózatokban telephelyek közötti VPN-kapcsolaton keresztül. Dedikált, biztonságos kapcsolatok Express Route-kapcsolatokat megvalósításához. Pont-pont típusú VPN-kapcsolatokat is biztosíthat a felhasználók közvetlen hozzáférést.
* [Terheléselosztó](../../load-balancer/load-balancer-overview.md) -forgalom a belső és külső forgalom tetszés szerint terheléselosztás biztosít.
* [Alkalmazásátjáró](../../application-gateway/application-gateway-introduction.md) – HTTP terheléselosztás hello alkalmazás rétegben, néhány további előnyökkel is jár biztosít a webalkalmazások számára történő telepítése helyett hello Azure terheléselosztó.
* [A TRAFFIC Manager](../../traffic-manager/traffic-manager-overview.md) - DNS-alapú forgalmat terjesztési toodirect végfelhasználók toohello legközelebbi rendelkezésre álló végpont, így Ön toohost, az alkalmazás különböző régiókban Azure adatközpontjaiban kívül.

## <a name="next-steps"></a>Következő lépések
[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)]

