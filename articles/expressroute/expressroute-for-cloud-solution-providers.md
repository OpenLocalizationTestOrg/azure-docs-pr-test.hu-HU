---
title: "a felhőalapú megoldás szolgáltatók ExpressRoute aaaAzure |} Microsoft Docs"
description: "Ez a cikk felhőszolgáltatóknál információkat nyújt a kívánt tooincorporate Azure szolgáltatások és ExpressRoute azokat az ajánlatokat."
documentationcenter: na
services: expressroute
author: richcar
manager: carmonm
editor: 
ms.assetid: f6c5f8ee-40ba-41a1-ae31-67669ca419a6
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/10/2016
ms.author: richcar
ms.openlocfilehash: 062ecbb5e461e4384b01c4ac478cab696b7ed398
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="expressroute-for-cloud-solution-providers-csp"></a>ExpressRoute felhőszolgáltatók (CSP) számára
Microsoft Hyper-skálázási szolgáltatásokat nyújt hagyományos viszonteladók és forgalmazók (CSP) toobe képes toorapidly rendelkezni az új szolgáltatásokat és az ügyfelek nélkül hello megoldásait kell tooinvest ezek a új szolgáltatások fejlesztésében. tooallow hello Cloud Solution Provider (CSP) hello képességét toodirectly ezek a új szolgáltatások kezelése, a Microsoft biztosít a programok és API-t a Microsoft Azure-erőforrások hello CSP toomanage engedélyezése a felhasználók nevében. Ezeknek az erőforrásoknak az egyike az ExpressRoute. ExpressRoute lehetővé teszi, hogy hello CSP tooconnect meglévő ügyfél erőforrások tooAzure szolgálathoz. ExpressRoute a nagy sebességű személyes kommunikációt hivatkozás tooservices az Azure-ban. 

A magas rendelkezésre állású Kapcsolatcsoportok csatolt tooa az egyetlen ügyfél előfizetés(ek) és nem osztható meg több ügyfél két ExpresRoute magában foglalja. Minden kapcsolatnak meg kell szüntetni a különböző útválasztó toomaintain hello magas rendelkezésre állású.

> [!NOTE]
> Az ExpressRoute sávszélesség- és kapcsolatkorlátokkal rendelkezik, ami azt jelenti, hogy a nagyméretű/összetett megvalósításokhoz több ExpressRoute-kapcsolatcsoport szükséges ügyfelenként.
> 
> 

Microsoft Azure biztosítja, hogy az ügyfelek tooyour kínálhat szolgáltatások egyre több.  Ezek a szolgáltatások előnyeit toobest van szükség a hello használata ExpressRoute kapcsolatok tooprovide nagy sebességű kis késésű hozzáférést toohello Microsoft Azure környezetben.

## <a name="microsoft-azure-management"></a>Microsoft Azure-kezelés
A Microsoft biztosít a kriptográfiai API-k toomanage hello Azure ügyfél-előfizetések azzal, hogy a saját szolgáltatás felügyeleti rendszerekkel programozott integráció lehetővé. A támogatott kezelési képességek [itt](https://msdn.microsoft.com/library/partnercenter/dn974944.aspx) találhatók.

## <a name="microsoft-azure-resource-management"></a>Microsoft Azure erőforrás-kezelés
Attól függően, hogy hello szerződés úgy, hogy az ügyfél állapítja meg hello előfizetés kezelésének. hello CSP közvetlen kezelése hello létrehozása és az erőforrások vagy hello ügyfél karbantartása is a Microsoft Azure-előfizetés hello irányítást tarthat fenn, és hozzon létre hello Azure-erőforrások módon van szükségük. Ha az ügyfél hello létrehozása a Microsoft Azure-előfizetés az erőforrások ezek egyikét fogja használni két modell: "Connect-keresztül" modell vagy a "Közvetlen-a következőnek" modell. Ezek a modellek hello a következő szakaszok részletesen ismerteti.  

### <a name="connect-through-model"></a>Szolgáltatón keresztüli csatlakozás modell
![helyettesítő szöveg](./media/expressroute-for-cloud-solution-providers/connect-through.png)  

Hello csatlakozás révén a modellben hello CSP kapcsolatot hoz létre közvetlen az adatközpont és az ügyfél Azure-előfizetés között. hello közvetlen kapcsolat ExpressRoute, a hálózati csatlakozás az Azure használatával. Ezt követően az ügyfél tooyour hálózati kapcsolatot. Ebben a forgatókönyvben szükséges tartozó hello ügyfél haladnak keresztül hello CSP hálózati tooaccess Azure-szolgáltatásokhoz. 

Más Azure-előfizetések nem felügyeli az ügyfél-e hello meg, a nyilvános interneten vagy a saját személyes csatlakozási tooconnect toothose szolgáltatás hello nem CSP előfizetésben kiépített hello használhatják. 

CSP kezelése az Azure-szolgáltatások, a feltételezzük, hogy hello CSP identitása egy korábban létrehozott felhasználói tárolja, amely volna replikálja az Azure Active Directoryba a CSP-előfizetést keresztül Administrate-On-Behalf-Of (AOBO) kezelését. Ebben a forgatókönyvben fő tényezői tartalmaz, ahol egy adott partnert vagy a szolgáltató egy hello ügyfél meglévő kapcsolattal rendelkezik, hello ügyfél jelenleg fel a szolgáltató szolgáltatások, illetve hello partner rendelkezik egy desire tooprovide szolgáltató által szolgáltatott és megoldások Azure által üzemeltetett tooprovide rugalmasságot és cím ügyfél felkéri, amely kriptográfiai Szolgáltató önmagában nem tud teljesíteni. A modell az alábbi **ábrán** látható.

![helyettesítő szöveg](./media/expressroute-for-cloud-solution-providers/connect-through-model.png)

### <a name="connect-toomodel"></a>Csatlakozás toomodel
![helyettesítő szöveg](./media/expressroute-for-cloud-solution-providers/connect-to.png)

Hello Connect-toomodel, hello szolgáltató hoz létre az ügyfél adatközpont között közvetlen kapcsolat és hello CSP-hez kiépített Azure-előfizetés ExpressRoute hello ügyfél (ügyfél) protokollt használó hálózati.

> [!NOTE]
> Az ExpressRoute hello ügyfél ehhez toocreate kell és karbantartása hello ExpressRoute-kapcsolatcsoportot.  
> 
> 

Ebben a forgatókönyvben kapcsolatot igényel, létrehozott, tulajdonában lévő és részben vagy egészben hello ügyfél által felügyelt közvetlen hálózati kapcsolaton keresztül közvetlenül a felhasználói hálózati tooaccess CSP által felügyelt Azure-előfizetéssel, tartozó hello ügyfél csatlakozik. Az ezen ügyfelek feltételezzük, hogy hello szolgáltató jelenleg nincs a felhasználói identitás tárolására létrehozott, és hello szolgáltató segítik hello ügyfél volna az aktuális azonosítása áruházat replikálása az Azure Active Directoryba kezelését a előfizetés AOBO keresztül. Ebben a forgatókönyvben fő tényezői tartalmazza, ahol egy adott partnert vagy a szolgáltató egy létesített kapcsolat hello ügyféllel, hello ügyfél jelenleg fel a szolgáltató szolgáltatások, illetve hello partner egy desire tooprovide szolgáltatások kizárólag a alapuló Azure által üzemeltetett megoldások hello nélkül egy meglévő szolgáltató-adatközpontját, illetve az infrastruktúra szükséges.

![helyettesítő szöveg](./media/expressroute-for-cloud-solution-providers/connect-to-model.png)

hello e két beállítás közötti választás alapuló az ügyfelek igényeinek megfelelően, és az aktuális kell tooprovide Azure szolgáltatások. Ezek a modellek és hello hello részleteit tartozó szerepköralapú hozzáférés-vezérlés, hálózat, és identitás kialakítási minta hello hivatkozásokat a következő részleteket ismertetnek:

* **Szerepköralapú hozzáférés-vezérlés (RBAC)** – Az RBAC az Azure Active Directoryn alapul.  Az Azure RBAC-ról [itt](../active-directory/role-based-access-control-configure.md) talál további információt.
* **Hálózati** – magában foglalja a hálózat a Microsoft Azure-ban különböző témakörök hello.
* **Az Azure Active Directory (AAD)** – AAD biztosít hello Identitáskezelés a Microsoft Azure és a 3. fél SaaS-alkalmazásokhoz. Az Azure AD-vel kapcsolatos további információkat lásd [itt](https://azure.microsoft.com/documentation/services/active-directory/).  

## <a name="network-speeds"></a>Hálózati sebességek
ExpressRoute támogatja a hálózati sebességet, 50 Mb/s too10Gb/s. Ez lehetővé teszi az ügyfelek toopurchase hello mekkora hálózati sávszélesség a egyedi környezet szükséges.

> [!NOTE]
> Hálózati sávszélesség kommunikáció megszakítása nélkül igény szerint növelhető, de tooreduce hello hálózati sebesség szükséges le hello áramkör szakadás és adatok ismételt létrehozása hello alacsonyabb hálózati sebesség.  
> 
> 

ExpressRoute jobb kihasználtságát hello nagyobb sebességű kapcsolat több Vnetek tooa egyetlen ExpressRoute-kapcsolatcsoportot hello kapcsolatot támogat. Egy ExpressRoute-kapcsolatcsoport között hello tulajdonában több Azure-előfizetések osztható azonos felhasználói.

## <a name="configuring-expressroute"></a>Az ExpressRoute konfigurálása
ExpressRoute lehet konfigurált toosupport három típusú forgalom ([útválasztási tartományok](#ExpressRoute-routing-domains)) keresztül egy ExpressRoute-kapcsolatcsoportot. Ezek a forgalomtípusok a Microsoft társviszony-létesítés, az Azure nyilvános társviszony-létesítés és a privát társviszony-létesítés. Válasszon egy vagy minden típusú adatforgalom toobe egyetlen ExpressRoute-kapcsolatcsoportot keresztül küldött, vagy használjon ExpressRoute-kapcsolatcsoportot hello és az ügyfél által igényelt elkülönítési hello méretétől függően több ExpressRoute-Kapcsolatcsoportok. hello biztonsági állapotát az ügyfél nem engedélyezi a nyilvános és a titkos forgalom tootraverse keresztül hello azonos áramkör.

### <a name="connect-through-model"></a>Szolgáltatón keresztüli csatlakozás modell
A connect-keresztül konfigurációs hello meg lesz az összes hálózati az ügyfelek datacenter erőforrások toohello előfizetések Azure-ban üzemeltetett underpinnings tooconnect hello felelős. Minden ügyfele toouse Azure-képességek lesz kívánt kell a saját ExpressRoute-kapcsolat, amely kezeli az hello meg. hello használandó hello azonos módszerek hello ügyfél tooprocure hello ExpressRoute-kapcsolatcsoportot szeretné használni. hello meg követi hello cikkben ismertetett lépéseket hello [ExpressRoute-munkafolyamatokat](expressroute-workflows.md) kiépítésével és áramkör állapotát. Hello, majd konfigurálja hello Border Gateway Protocol (BGP) útvonalak toocontrol hello forgalom továbbítására hello a helyszíni hálózat és az Azure virtuális hálózat között.

### <a name="connect-toomodel"></a>Csatlakozás toomodel
A connect-tooconfiguration, az ügyfél már rendelkezik egy meglévő kapcsolat tooAzure vagy kezdeményez egy kapcsolat toohello internetszolgáltató ExpressRoute hozzákapcsolva, az ügyfél a saját adatközpont közvetlenül tooAzure helyett az adatközpontban. toobegin hello telepítési folyamatot, az ügyfél fogja lépésekkel hello hello csatlakozás révén a modellben a fent leírt módon. Hello kapcsolat létrehozása után az ügyfél a hálózat és az Azure Vnetekhez tooconfigure hello helyszíni útválasztók toobe képes tooaccess kell.

Segítséget nyújthat hello kapcsolat beállításához, és tooallow hello erőforrások hello konfigurálása továbbítja a datacenter(s) toocommunicate hello ügyfél erőforrásaikhoz az adatközpontban, illetve az Azure-ban üzemeltetett hello erőforrások a.

## <a name="expressroute-routing-domains"></a>ExpressRoute útválasztási tartományok
Az ExpressRoute három útválasztási tartományt kínál: nyilvános, privát és Microsoft társviszony-létesítés. Hello útválasztási tartományok mindegyikében azonos útválasztók aktív-aktív konfigurációban a magas rendelkezésre állású legyen konfigurálva. Az ExpressRoute útválasztási tartományokkal kapcsolatos további részleteket lásd [itt](expressroute-circuit-peerings.md).

Megadhat egyéni útvonalak szűrők tooallow csak hello módra tooallow hasznos vagy szükséges. További információk és toosee hogyan toomake ezeket a módosításokat: a cikk: [létrehozása és módosítása a powershellel ExpressRoute-kapcsolatcsoportot útválasztást](expressroute-howto-routing-classic.md) útválasztási szűrők kapcsolatos további részletekért.

> [!NOTE]
> A Microsoft és a nyilvános Társviszony kapcsolatot kell lennie, ha egy nyilvános IP-cím hello az ügyfél vagy a CSP a tulajdonosa, és tooall definiált szabályok meg kell felelnie. További információkért lásd: hello [ExpressRoute Előfeltételek](expressroute-prerequisites.md) lap.  
> 
> 

## <a name="routing"></a>Útválasztás
ExpressRoute csatlakozik toohello Azure hálózatok hello Azure virtuális hálózati átjárón keresztül. A hálózati átjárók biztosítják az útvonalválasztást az Azure virtuális hálózatok számára.

Hello vNet toodirect forgalom hello vnet hello alhálózatok és a alapértelmezett útválasztási táblázat létrehozása is Azure virtuális hálózatok létrehozásához. Ha hello alapértelmezett útválasztási táblázatot nem elegendő hello megoldás egyéni útvonalak tooroute kimenő forgalom toocustom készülékek hozhatók létre, vagy tooblock toospecific alhálózatok és a külső hálózati útvonalak.

### <a name="default-routing"></a>Alapértelmezett útválasztás
hello alapértelmezett útválasztási táblázatot a következő útvonalak hello tartalmazza:

* Alhálózaton belüli útválasztás
* Alhálózat-az-alhálózat hello virtuális hálózaton belül
* toohello Internet
* Virtuális hálózatok közti útvonalak a VPN Gateway használatával
* Virtuális hálózat és helyszíni hálózat közötti útvonal VPN- vagy ExpressRoute-átjáró használatával

![helyettesítő szöveg](./media/expressroute-for-cloud-solution-providers/default-routing.png)  

### <a name="user-defined-routing-udr"></a>Felhasználó által meghatározott útválasztás (UDR)
Felhasználó által definiált útvonalak engedélyezése hello vezérlő hello a kimenő forgalom hozzárendelt alhálózat hello virtuális hálózat alhálózatai tooother, vagy több mint egy hello más előre definiált átjárók (ExpressRoute; internet vagy VPN). alapértelmezett útválasztási rendszertábla hello cserélhetősége egy felhasználó által definiált útválasztási táblázatot, amely kiváltja az egyéni útvonalakat a hello alapértelmezett útválasztási táblázathoz. Felhasználói útválasztási, az ügyfelek hozzon létre például a tűzfalak és a behatolás készülékek meghatározott útvonalakat tooappliances, vagy blokkolni lehessen hozzáférés toospecific alhálózatok hello alhálózatból üzemeltető hello felhasználói útvonal. A felhasználó által megadott útvonalak áttekintését lásd [itt](../virtual-network/virtual-networks-udr-overview.md). 

## <a name="security"></a>Biztonság
Attól függően, hogy melyik modellt használja, a Connect-tooor Connect keresztül, az ügyfél hello biztonsági szabályzatokat határoz meg a Vneten belül, vagy hello biztonsági házirend követelményeinek toohello CSP toodefine tootheir Vnetek biztosít. a következő biztonsági feltételek hello lehet meghatározni:

1. **Ügyfél elkülönítési** – hello Azure platformon el vannak különítve ügyfél felhasználói Azonosítót és a virtuális hálózat információ tárolása egy biztonságos adatbázis, amely használt tooencapsulate egyes ügyfelek forgalmat egy GRE csatornán.
2. **Hálózati biztonsági csoport (NSG)** szabályok a következők engedélyezett forgalmat, és az Azure Vnetekhez hello alhálózatát kimenő meghatározásához. Alapértelmezés szerint a hello NSG tartalmaz tooblock kommunikációt blokkoló szabályokat, hello Internet toohello vNet és engedélyezési szabályok a forgalmat a Vneten belül. A hálózati biztonsági csoportokkal kapcsolatos további információkat [itt](https://azure.microsoft.com/blog/network-security-groups/) tekintheti meg.
3. **A kényszerített bújtatás** – Ez az egy beállítás tooredirect internet kötött hello ExpressRoute kapcsolat toohello helyszíni adatközpontjában keresztül átirányítja az Azure toobe származó forgalmat. A kényszerített bújtatással kapcsolatos további információkat [itt](expressroute-routing.md#advertising-default-routes) tekintheti meg.  
4. **Titkosítási** – annak ellenére, hogy hello ExpressRoute-Kapcsolatcsoportok dedikált tooa adott ügyfélhez, nincs hello lehetséges, hogy a hálózati szolgáltató hello megszegése volt, hogy egy behatoló tooexamine csomag forgalmat. a lehetséges, az ügyfél vagy a CSP is titkosítani forgalom tooaddress kapcsolat hello áramló hello a helyi erőforrások és az Azure-erőforrások (lásd az ügyfél 1 toohello választható bújtatási mód IPSec közötti összes forgalom IPSec-alagút módú házirendek meghatározásával az 5. ábra: ExpressRoute biztonsági fent). hello második lehetőség lenne toouse egy tűzfal készülék minden egyes hello end pontján hello ExpressRoute-kapcsolatcsoportot. Ehhez szükséges további 3. fél tűzfal mindkét ends tooencrypt hello forgalom hello ExpressRoute-kapcsolatcsoportot keresztül telepített virtuális gépek/készülékek toobe.

![helyettesítő szöveg](./media/expressroute-for-cloud-solution-providers/expressroute-security.png)  

## <a name="next-steps"></a>Következő lépések
hello Cloud Solution Provider szolgáltatást biztosít egy módon tooincrease nélkül hello tooyour érték ügyfelei szükséges drága infrastruktúra, és képes vásárlások a pozíció hello elsődleges kiszervezési szolgáltatóként megőrzésével. A Microsoft Azure-ban való zökkenőmentes integrációt hello CSP API, így toointegrate kezelése a Microsoft Azure a meglévő felügyeleti keretrendszerek belül érhető el.  

További információk találhatók a következő hivatkozások hello:

[A Microsoft felhőszolgáltatói programja](https://partner.microsoft.com/en-US/Solutions/cloud-reseller-overview).  
[A Cloud Solution Provider, készen áll a tootransact beolvasása](https://partner.microsoft.com/en-us/solutions/cloud-reseller-pre-launch).  
[A Microsoft felhőszolgáltatói erőforrásai](https://partner.microsoft.com/en-us/solutions/cloud-reseller-resources).

