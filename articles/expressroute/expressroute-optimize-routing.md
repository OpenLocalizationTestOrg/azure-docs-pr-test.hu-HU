---
title: "Az ExpressRoute-útválasztás optimalizálása: Azure | Microsoft Docs"
description: "Ezen a lapon biztosít részletek hogyan toooptimize útválasztási, ha egynél több ExpressRoute áramkörök, amely a csatlakozás a Microsoft és a vállalati hálózat között."
documentationcenter: na
services: expressroute
author: charwen
manager: carmonm
editor: 
ms.assetid: fca53249-d9c3-4cff-8916-f8749386a4dd
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/06/2017
ms.author: charwen
ms.openlocfilehash: ebcfb638f67a9ac78c3e476668bfd0bb0ffb9985
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="optimize-expressroute-routing"></a>Az ExpressRoute-útválasztás optimalizálása
Ha egyszerre több ExpressRoute-Kapcsolatcsoportok, hogy egynél több elérési út tooconnect tooMicrosoft. Ennek eredményeképpen optimálisnál útválasztási fordulhat - ez azt jelenti, hogy a forgalom eltarthat hosszabb elérési tooreach Microsoft, és a Microsoft tooyour hálózat. hello hosszabb hello hálózati elérési út hello nagyobb hello késleltetéssel járhat. A késés közvetlen hatással van az alkalmazások teljesítményére és a felhasználói élményre. Ez a cikk azt mutatják be, a probléma, és a azt ismertetik, hogyan útválasztás használatával toooptimize hello szabványos útválasztási technológiákat.

## <a name="suboptimal-routing-from-customer-toomicrosoft"></a>Az ügyfél tooMicrosoft útválasztási optimálisnál
Most tekintsen meg Bezárás hello útválasztási probléma példa szerint. Tegyük fel, két rendelkező hello USA, a másik Debrecenben és egy Győri. Az irodák egy nagykiterjedésű hálózaton (WAN) keresztül csatlakoznak, amely lehet a saját gerinchálózata vagy a szolgáltatója IP-alapú VPN hálózata. Két ExpressRoute-Kapcsolatcsoportok egy, Nyugat-India és egy US-keleti, hello WAN is csatlakozó rendelkezik. Természetesen két elérési utak tooconnect toohello Microsoft hálózattal rendelkezik. Most tegyük fel, hogy az USA nyugati régiójában és az USA keleti régiójában is rendelkezik egy Azure üzemelő példánnyal (például az Azure App Service-szel). A levelezéshez nem tooconnect a másik Debrecenben tooAzure Velünk nyugati és a felhasználók a győri tooAzure amerikai keleti, mert a szolgáltatás-rendszergazdák kihirdeti, hogy minden egyes office access felhasználói hello Azure-szolgáltatások optimális élmény közelben. Sajnos hello tervnek megfelelően működik jól hello keleti part felhasználók azonban nem hello nyugati part felhasználók. hello hello oka hello következő. Minden egyes ExpressRoute-kapcsolatcsoportot azt helyüket tooyou hello előtag Azure US-keleti (23.100.0.0/16), mind hello előtag Azure, Nyugat-India (13.100.0.0/16). Ha nem tudja, melyik régióban mely előtag tartozik, még nem tud tootreat azt másképp. A WAN hálózaton elképzelhető, hogy az előtagok hello mint Velünk nyugati szorosabb tooUS keleti, és ezért útvonal-amerikai keleti mindkét office felhasználók toohello ExpressRoute-kapcsolatcsoportot. Hello célból Szomorú sokan hello másik Debrecenben office fog.

![1. eset ExpressRoute probléma - optimálisnál ügyfél tooMicrosoft az Útválasztás](./media/expressroute-optimize-routing/expressroute-case1-problem.png)

### <a name="solution-use-bgp-communities"></a>Megoldás: BGP-közösségek használata
toooptimize útválasztási mindkét felhasználók számára, mely előtag származik Azure Velünk nyugati és amelyhez Azure amerikai keleti tooknow kell. Ezt az információt a [BGP-közösség értékeivel](expressroute-routing.md) kódoljuk. Azt például hozzárendelt egy egyedi BGP közösségi érték tooeach az Azure-régió, „12076:51004” előtagot az USA keleti régiójához, a „12076:51006” előtagot az USA nyugati régiójához. Most, hogy tudja, melyik előtag melyik Azure-régióból származik, konfigurálhatja, hogy melyik ExpressRoute-kapcsolatcsoportot részesíti előnyben. Hello BGP tooexchange útválasztási információ használjuk, mert a BGP által helyi preferencia tooinfluence útválasztási is használhatja. A példa kedvéért rendelhet, Nyugat-India-amerikai keleti-nál nagyobb helyi beállításokat szabályozó érték too13.100.0.0/16, és ehhez hasonlóan a helyi beállításokat szabályozó magasabb érték too23.100.0.0/16 US-keleti mint, Nyugat-India. Ez a konfiguráció lesz győződjön meg arról, hogy mindkét elérési utak tooMicrosoft érhetők el, amikor a felhasználók, a másik Debrecenben érvénybe hello ExpressRoute-kapcsolatcsoportot a US nyugati tooconnect tooAzure Velünk nyugati mivel a felhasználók Győri hello ExpressRoute az Amerikai keleti tooAzure amerikai keleti régiója . Az útválasztás minkét oldalon optimalizálva van. 

![1. ExpressRoute-eset megoldása – BGP-közösségek használata](./media/expressroute-optimize-routing/expressroute-case1-solution.png)

> [!NOTE]
> hello azonos technika, helyi prioritást lehet a virtuális hálózati ügyfél tooAzure alkalmazott toorouting. Jelenleg nem címke BGP közösségi érték toohello előtagok Azure tooyour hálózatról hirdetve. Azonban mivel tudja, hogy a virtuális hálózat telepítés közül melyik Bezárás toowhich az Office, konfigurálhatja az útválasztók ennek megfelelően tooprefer egy ExpressRoute-kapcsolatcsoport tooanother.
>
>

## <a name="suboptimal-routing-from-microsoft-toocustomer"></a>A Microsoft toocustomer útválasztási optimálisnál
Íme egy másik példa, ahol kapcsolatok a Microsoft használják a hosszabb elérési tooreach a hálózaton. Ebben az esetben helyszíni Exchange-kiszolgálókat és az Exchange Online-t használja egy [hibrid környezetben](https://technet.microsoft.com/library/jj200581%28v=exchg.150%29.aspx). A használt csatlakoztatott tooa WAN. Hogy hirdesse meg hello előtagjait, a helyszíni kiszolgálók mind az irodák tooMicrosoft hello két ExpressRoute-Kapcsolatcsoportok keresztül. Exchange Online kapcsolatok toohello a helyszíni kiszolgálók azokban az esetekben, például a postaláda áttelepítési kezdeményez. Sajnos hello kapcsolat tooyour másik Debrecenben office irányított toohello ExpressRoute-kapcsolatcsoport-amerikai keleti hello teljes kontinensre hátsó toohello nyugati part áthaladó előtt áll. hello hello oka hasonló toohello először egy. Bármely mutató nélkül hello Microsoft hálózati nem sikerült megállapítani, mely felhasználói előtag Bezárás tooUS keleti, és melyik tooUS nyugati bezárásához. Ez akkor fordul elő, toopick hello megfelelő elérési tooyour irodában másik Debrecenben.

![Az ExpressRoute eset 2 - optimálisnál a Microsoft toocustomer Útválasztás](./media/expressroute-optimize-routing/expressroute-case2-problem.png)

### <a name="solution-use-as-path-prepending"></a>Megoldás: AS PATH előtag-beillesztés
Két megoldások toohello probléma van. hello először egyik, hogy a másik Debrecenben Office 177.2.0.0/31 hello ExpressRoute körön, Nyugat-India egyszerűen hirdetményt a helyszíni előtag és a helyszíni előtag a győri Office 177.2.0.2/31 hello ExpressRoute körön-amerikai keleti. Ennek eredményeképpen nincs Microsoft tooconnect tooeach meg a esetén csak egy elérési útját. Nincs félreérthetőség, és az útválasztás optimalizálva van. Ezzel a kialakítással toothink vonatkozó feladatátvételi stratégiát kell. A hello eseményt, amely az elérési út tooMicrosoft keresztül ExpressRoute hello hibás, akkor kell, hogy, hogy az Exchange Online továbbra is csatlakozni tud tooyour a helyszíni kiszolgálók toomake. 

hello második megoldás, hogy folytatja tooadvertise mindkét hello előtaglistát kezel a mindkét ExpressRoute-Kapcsolatcsoportok, de emellett akkor biztosítják mely előtag van Bezárás toowhich egy, a használt javaslat. Támogatott BGP-AS elérési fertőző, mert a előtag tooinfluence irányításához konfigurálhatja hello AS elérési útja. Ebben a példában az US-keleti 172.2.0.0/31 AS ELÉRÉSI hello úgy, hogy azt főként a hello ExpressRoute-kapcsolatcsoportot, Nyugat-India szánt ezt az előtagot (mivel a hálózati fog gondolja, hogy hello elérési toothis előtag hello Nyugat-India rövidebb) forgalom növelheti meg. Hasonlóképpen meghosszabbíthatja hello AS elérési ÚTJA a 172.2.0.2/31, Nyugat-India, hogy azt fogja inkább-amerikai keleti hello ExpressRoute-kapcsolatcsoportot. Az útválasztás mindkét iroda esetében optimalizálva van. Ezzel a kialakítással, ha az egyik ExpressRoute-kör megszakad, az Exchange Online továbbra is elérheti az Ön hálózatát a másik ExpressRoute-kapcsolatcsoporton és a WAN hálózaton keresztül. 

> [!IMPORTANT]
> Tudjuk eltávolítani saját, a Microsoft Peering kapott hello AS elérési ÚTJA a hello előtagok szereplő számok. Tooappend szüksége nyilvános hello AS ELÉRÉSI tooinfluence útválasztást Microsoft Peering SZÁMKÉNT.
> 
> 

![2. ExpressRoute-eset megoldása – AS PATH előtag-beillesztés](./media/expressroute-optimize-routing/expressroute-case2-solution.png)

> [!NOTE]
> Az alábbi hello többek között a Microsoft és a nyilvános társviszony, amíg támogatjuk hello ugyanazokat a képességeket a hello magánhálózati társviszony-létesítés. Hello AS elérési fertőző belül is működik egy egyszeri ExpressRoute-kapcsolatcsoportot, tooinfluence hello kijelölés hello elsődleges és másodlagos elérési utak közül.
> 
> 

## <a name="suboptimal-routing-between-virtual-networks"></a>Az optimálisnál rosszabb útválasztás a virtuális hálózatok között
Az ExpressRoute, engedélyezheti a virtuális hálózati tooVirtual (amely más néven "VNet") hálózati kommunikációs tooan ExpressRoute-kapcsolatcsoportot létrehozhatja őket. Toomultiple ExpressRoute-Kapcsolatcsoportok kapcsolja őket, amikor optimálisnál útválasztási akkor fordulhat elő, hello Vnetek között. Vegyünk egy példát. Két ExpressRoute-kapcsolatcsoporttal rendelkezik, az egyik az USA nyugati régiójában, a másik az USA keleti régiójában található. Mindkét régióban két VNet van. A kiszolgálók egy Vneten belül vannak telepítve, és az alkalmazáskiszolgálók hello más. A redundancia érdekében csatolunk hello két Vnetek szereplő minden egyes régió tooboth hello helyi ExpressRoute-kapcsolatcsoportot, és hello távoli ExpressRoute-kapcsolatcsoportot. Az alább is látható, minden egyes vnet nincsenek két elérési utak toohello más virtuális hálózat. hello Vnetek nem tudja, melyik ExpressRoute-kapcsolatcsoportot helyi, és azt, amelyiket távoli. Ennek következtében, mint egyenlő-költség-Multi-Path (ECMP) útválasztási tooload-egyenleg többek VNet forgalmát, egyes adatforgalmakat érvénybe hello elérési útja hosszabb és lekérése irányítása: hello távoli ExpressRoute-kapcsolatcsoportot.

![3. ExpressRoute-eset – az optimálisnál rosszabb útválasztás a virtuális hálózatok között](./media/expressroute-optimize-routing/expressroute-case3-problem.png)

### <a name="solution-assign-a-high-weight-toolocal-connection"></a>Megoldás: a nagy súlyozással toolocal kapcsolat hozzárendelése
hello megoldás felettébb egyszerű. Tudjuk, ahol hello virtuális hálózatokat és hello Kapcsolatcsoportok, mivel részleteket megosztani mely minden egyes virtuális hálózaton kell inkább elérési útja. Kifejezetten ebben a példában a rendeljen magasabb súly toohello helyi kapcsolat mint toohello távoli kapcsolat (hello konfigurációs példa [Itt](expressroute-howto-linkvnet-arm.md#modify-a-virtual-network-connection)). Ha egy virtuális hálózat kap hello hello előtagja más azt a hello legmagasabb súly toosend szánt forgalom ezt az előtagot főként a hello kapcsolat több kapcsolatot a virtuális hálózat.

![Az ExpressRoute eset 3 megoldás - hozzárendelése magas súly toolocal kapcsolat](./media/expressroute-optimize-routing/expressroute-case3-solution.png)

> [!NOTE]
> Ha több ExpressRoute-Kapcsolatcsoportok hello súly AS ELÉRÉSI prepending, hello második forgatókönyv fent leírt módszer alkalmazása helyett kapcsolat konfigurálásával a virtuális hálózat tooyour helyszíni hálózatról, Útválasztás is befolyásolják. Az egyes előtag követően mindig áttekintjük hello kapcsolat súly előtt hello AS elérési útra meghatározásakor hogyan toosend forgalmat.
>
>
