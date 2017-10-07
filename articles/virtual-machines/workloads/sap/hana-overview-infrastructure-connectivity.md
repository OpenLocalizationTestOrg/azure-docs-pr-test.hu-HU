---
title: "aaaInfrastructure és csatlakozási tooSAP HANA (nagy példányok) Azure-on |} Microsoft Docs"
description: "Kapcsolat szükséges infrastruktúra toouse SAP HANA konfigurálása az Azure (nagy példány)."
services: virtual-machines-linux
documentationcenter: 
author: RicksterCDN
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/01/2016
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0af34fbd82413bf63981036a76eaa264d8299ec1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sap-hana-large-instances-infrastructure-and-connectivity-on-azure"></a>SAP HANA (nagy példányok) infrastruktúra és az Azure-kapcsolat 

Néhány definíciók előzetes megfizetése esetén ez az útmutató olvasása előtt. A [SAP HANA (nagy példányok) – áttekintés és Azure architektúra](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) HANA nagy példány egység, amelyek két különböző osztályú bevezetett azt:

- S72, S72m, S144, S144m, S192 és S192m, amelyek azt tooas hello "Típusú i. osztály" SKU.
- S384, S384m, S384xm, S576, S768 és S960, amely az tooas hello "Típusú class" SKU.

hello osztály kulcsszavak folyamatos toobe hello HANA dokumentáció tooeventually tekintse meg a toodifferent lehetőségei és követelményei alapján HANA nagy példány termékváltozatok nagy példány használatban.

Más definíciók gyakran használjuk a következők:
- **Nagy példány stamp:** hardver infrastruktúra verem egy SAP HANA TDI van hitelesített, és a dedikált toorun SAP HANA-példányok Azure-ban.
- **SAP HANA Azure (nagy példány):** hello ajánlat Azure toorun HANA-példány az SAP HANA TDI hardver, amely telepítve van a különböző Azure-régiók nagy példány bélyegzők hitelesített hivatalos nevét. hello kapcsolódó kifejezés **HANA nagy példány** rövid a SAP HANA Azure (nagy példányokat), és széles körben használt műszaki telepítési útmutatóban.
 

SAP HANA (nagy példányok) Azure megvásárlása hello akkor fejeződik be, és a vállalati Microsoft-fiókok ügyfélszolgálatától hello között, miután hello következőket által igényelt Microsoft toodeploy HANA nagy példány egységek:

- Ügyfél neve
- Céges kapcsolattartási adatait (beleértve az e-mail címét és telefonszámát)
- Műszaki kapcsolattartási adatait (beleértve az e-mail címét és telefonszámát száma)
- Műszaki kapcsolattartási adatait (beleértve az e-mail címét és telefonszámát száma)
- Az Azure-telepítés régió (USA nyugati régiója, USA keleti régiója, Kelet-Ausztrália, Ausztrália délkeleti, Nyugat-Európában és frissítésétől július Észak-Európa 
- 2017)
- Erősítse meg az Azure (nagy példányok) Termékváltozat (konfiguráció) SAP HANA
- Már az részletes hello áttekintése és architektúra dokumentum HANA nagy példányok, minden Azure-régió telepített:
    - Egy /29 IP-címtartomány, amelyek kapcsolódnak az Azure Vnetekhez tooHANA nagy példányok ER-P2P kapcsolatokhoz
    - Egy/24 hello HANA nagy példányok kiszolgáló IP-készlet használt CIDR-blokkja
- hello IP cím tartomány használt értékek hello VNet címterület attribútumának minden Azure virtuális hálózatot, amely a toohello HANA nagy példányok
- Az egyes HANA nagy példányok rendszer adatokat:
  - Kívánt gazdagépnévvel - ideális esetben a teljesen minősített tartománynevét.
  - Hello HANA nagy példány egység kívül hello kiszolgáló IP-készlet címtartománya - kívánt IP-címet vegye figyelembe, hogy hello első 30 hello kiszolgáló IP-készlet címtartománya HANA nagy példányok belüli belső használatra fenntartott IP-címek
  - SAP HANA SID neve hello SAP HANA-példány (szükséges toocreate hello szükséges SAP HANA-kapcsolódó lemezkötetek). hello HANA SID hello engedélyeinek létrehozásához szükség <sidadm> hello NFS köteteken, amelyek csatlakoztatott toohello HANA nagy példány egység kap. Azt is használatos hello lemezkötetek lekérése csatlakoztatott hello neve összetevői. Toorun hello egységen több HANA példány, meg kell toolist több HANA azonosítókkal. Mindegyik lekérdezi a hozzárendelt kötetek külön készletét.
  - hello groupid hello hana-sidadm felhasználó rendelkezik-e a Linux operációs rendszert futtató hello van szükség toocreate hello szükséges SAP HANA-kapcsolódó kötetek. hello SAP HANA-telepítés egy csoport azonosítójú 1001, általában hello sapsys csoportot hoz létre. hello hana-sidadm felhasználói csoport tagja
  - hello userid hello hana-sidadm felhasználó rendelkezik-e a Linux operációs rendszert futtató hello van szükség toocreate hello szükséges SAP HANA-kapcsolódó kötetek. Több HANA példány hello egységen futnak, ha szüksége van-e minden hello toolist <sid>adm-felhasználók 
- Azure-előfizetés azonosítója hello Azure-előfizetés toowhich SAP HANA Azure HANA nagy példányokon közvetlenül csatlakoztatott toobe fog. Az előfizetés-azonosító az Azure-előfizetéssel, hello HANA nagy példány egység(ek) megbízott toobe elemleírásként hello hivatkozik.

Miután megadta a hello információ Microsoft rendelkezések SAP HANA Azure (nagy példányok) és az Azure Vnetekhez tooHANA nagy példányok és tooaccess hello HANA nagy példány egységek visszaadható hello információ szükséges toolink.

## <a name="connecting-azure-vms-toohana-large-instances"></a>Csatlakozás az Azure virtuális gépek tooHANA nagy példányok

A már említett [SAP HANA (nagy példányok) – áttekintés és Azure architektúra](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) hello minimális központi telepítés HANA nagy példányok hello SAP alkalmazásréteg Azure keres, például:

![Az Azure VNet tooSAP Azure (nagy példányok) és a helyszíni HANA csatlakoztatva](./media/hana-overview-architecture/image3-on-premises-infrastructure.png)

Keresése a hello Azure VNet ügyféloldali szorosabb, azt utólagosan hello szükség:

- egy Azure virtuális hálózatot, amelyet használt toobe toodeploy hello virtuális gépeinek hello SAP alkalmazás réteget, hello meghatározása.
- Amely automatikusan azt jelenti, hogy hello Azure virtuális hálózat van meghatározva, amely tényleg hello egy használt toodeploy hello virtuális gépeket az egyik alapértelmezett alhálózatát.
- hello létrehozott Azure virtuális hálózat toohave kell legalább egy Virtuálisgép-alhálózatot és egy ExpressRoute-átjáró-alhálózatot. Ezek alhálózatok hello IP-címtartományok megadott és a következő részekben hello tárgyaltuk rendelhető.

Igen most megismerhetők kicsit szorosabb hello Azure VNet létrehozása nagy HANA-példányok

### <a name="creating-hello-azure-vnet-for-hana-large-instances"></a>Nagy HANA-példányok hello Azure virtuális hálózat létrehozása

>[!Note]
>hello Azure VNet HANA nagy példány hello Azure Resource Manager telepítési modell segítségével kell létrehozni. hello régebbi Azure telepítési modell, klasszikus telepítési modell-alkalmazásokként ismert hello HANA nagy példány megoldás van támogatva.

hello VNet segítségével hozhatók létre hello Azure-portálon, a PowerShell, a Azure-sablon alapján vagy az Azure parancssori felület (lásd: [hello Azure-portál virtuális hálózat létrehozása](../../../virtual-network/virtual-networks-create-vnet-arm-pportal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). A következő példa hello azt megismerhetők a virtuális hálózaton keresztül hello Azure-portálon létrehozott.

Ha megnézzük hello definíciók egy Azure vnet hello Azure-portálon keresztül történő, vizsgáljuk meg az egyes hello definíciókat, és milyen azokat áll toowhat azt másik IP-címtartományok listája. A Microsoft hello vannak van szó, **Címterület**, azt jelenti, hogy hello Azure VNet hello címtér toouse engedélyezett. Ezen a címterületen is hello tartománya, virtuális hálózatot használ a BGP útválasztási propagálás hello. Ez **Címterület** itt találja:

![Azure VNet terület cím jelenik meg hello Azure-portálon](./media/hana-overview-connectivity/image1-azure-vnet-address-space.png)

Hello esetben 10.16.0.0/16, a megelőző hello Azure VNet ahelyett, hogy nagy és széles IP cím tartomány toouse lett megadva. Azt jelenti, hogy minden hello IP-címtartományok ezen a virtuális hálózaton belül további alhálózatokat lehet a tartományokat a "címtartomány" belül. Általában nem javasoljuk ilyen egy nagy-címtartományt az egyetlen virtuális hálózat az Azure-ban. De kap egy lépéssel tovább, hello Azure VNet definiált hello alhálózatokra vizsgáljuk meg:

![Az Azure VNet-alhálózatok és az IP-címtartományok](./media/hana-overview-connectivity/image2b-vnet-subnets.png)

Ahogy látja, úgy tekintünk, a virtuális hálózat (Itt "alapértelmezett" is nevezik) első Virtuálisgép-alhálózatot és a "GatewaySubnet" nevű alhálózat.
A következő szakasz hello, toohello IP-címtartomány hello alhálózat, amely hello grafikus, mint a "default" hívása történt irányítjuk **Azure virtuális alhálózat IP-címtartomány**. A következő részekben hello, irányítjuk toohello IP-címtartománya a hello Átjáróalhálózatot, **hálózatok átjáró alhálózati IP-címtartomány**. 

Hello esetében a fenti két hello grafikus által bemutatott, láthatja, hogy hello **VNet Címterület** egyaránt vonatkozik, hello **Azure virtuális alhálózat IP-címtartomány** és hello **hálózatok átjáró alhálózati IP-címe tartomány**. 

Más esetekben, amikor tooconserve szükséges, vagy adott kell az IP-címtartományok, korlátozhatja a hello **VNet Címterület** egy VNet toohello adott címtartományok alhálózatok használják. Ebben az esetben megadhat hello **VNet Címterület** egy vnet több adott címtartományok itt látható módon:

![Az Azure VNet-címtér és két szóköz](./media/hana-overview-connectivity/image3-azure-vnet-address-space_alternate.png)

Ebben az esetben hello **VNet Címterület** definiált két szóközt tartalmaz. E két szóközt tartalmaz, azonos toohello IP-címtartományok definiált **Azure virtuális alhálózat IP-címtartomány** és hello **hálózatok átjáró alhálózati IP-címtartományt.**

A bérlői alhálózat (Virtuálisgép-alhálózatok) bármilyen tetszés elnevezési szabványnak is használhatja. Azonban **mindig kell egy és csak egy átjáró alhálózatának minden vnet** , amely összeköti toohello SAP HANA (nagy példányok) Azure ExpressRoute körön. És **az átjáró-alhálózat neve kötelezően "GatewaySubnet"** tooensure hello ExpressRoute-átjáró megfelelő elhelyezését.

> [!WARNING] 
> Alapvető fontosságú, hogy hello átjáróalhálózatot mindig neve "GatewaySubnet".

Több Virtuálisgép-alhálózatok is használható, még a nem összefüggő címtartományai használatával. Azonban amint azt korábban említettük, a cím tartományok hatálya hello **VNet Címterület** hello VNet összesített formában vagy hello listájának pontos tartományok hello Virtuálisgép-alhálózatok és hello átjáró-alhálózatot.

Egy Azure virtuális hálózatot, amely a tooHANA nagy példányok hello fontos tény összefoglalása:

- Toosubmit tooMicrosoft hello kell **VNet Címterület** HANA nagy példányok egy kezdeti telepítés végrehajtása során. 
- Hello **VNet Címterület** lehet egy nagyobb tartományt, amely Azure virtuális gép alhálózat IP-cím tranzakciónaplók és hello hálózatok átjáró alhálózati IP-címtartomány hello tartománya lefedi.
- Vagy elküldheti a **VNet Címterület** több tartomány választékából hello különböző IP-címtartományok VM alhálózat IP-cím tranzakciónaplók és hello hálózatok átjáró alhálózati IP-címtartományt.
- meghatározott hello **VNet Címterület** használt BGP útválasztási propagálás van.
- hello átjáróalhálózatot hello nevének kell lennie: **"GatewaySubnet".**
- Hello **VNet Címterület** hello HANA nagy példány ügyféloldali tooallow szűrésére szolgál, vagy letilthatja a forgalmat toohello HANA nagy példány egység az Azure-ból. Ha hello BGP útválasztási adatokat hello Azure VNet és hello IP-címtartományok hello HANA nagy példány kiszolgálóoldali szűrés beállítása nem egyezik, a kapcsolat problémák merülhetnek fel.
- Nincsenek a hello átjáróalhálózatot bizonyos adatai, amelyek lejjebb "Csatlakozás a virtuális hálózat tooHANA nagy példány ExpressRoute" szakaszban tárgyalt



### <a name="different-ip-address-ranges-toobe-defined"></a>Különböző IP-címtartományt definiált toobe 

A Microsoft már néhány hello IP cím tartományokhoz szükséges toodeploy HANA nagy példányok korábbi szakaszokban bemutatott. Azonban néhány további IP-címtartományokat, amelyek fontosak. Ugorjunk keresztül néhány további részleteket. hello következő IP-címek, amelyek nem az összes szükséges toobe benyújtott tooMicrosoft kell toobe definiált, a kezdeti telepítés kérelem elküldése előtt:

- **Virtuális hálózat címtartomány:** a már korábban bemutatott, vagy a hello IP-cím range(s) Ön rendelt hozzá (vagy tooassign megtervezése) tooyour cím terület paraméter hello Azure virtuális hálózatok felé (VNet) a csatlakozó toohello SAP HANA nagy példány környezet. Javasoljuk, hogy a címtartomány paraméter értéke egy többsoros hello Azure Virtuálisgép-alhálózat tranzakciónaplók és hello Azure átjáró-alhálózat tartományának hello grafikus korábban ismertetett módon. Ezt a tartományt a helyi vagy a kiszolgáló IP-készletet vagy ER-P2P címtartományai nem lehetnek átfedők. Hogyan tooget ez vagy ezek IP-cím tranzakciónaplók? A vállalati hálózat team vagy szolgáltatás szolgáltató egy vagy több IP cím tranzakciónaplók, vagy nem használják a hálózaton belüli kell biztosítania. Példa: Ha az Azure Virtuálisgép-alhálózatot (lásd fentebb) 10.0.1.0/24, és az Azure-átjáró alhálózatának (lásd alább) 10.0.2.0/28, majd az Azure VNet címteret ajánlott toobe két sorok; 10.0.1.0/24 és 10.0.2.0/28. Bár hello címterület értékek összesíthetők, toohello alhálózati tartományok egyeztetése ajánlott tooavoid véletlen újbóli nem használt IP-címtartományok a jövőbeli hello nagyobb címterek belül máshol a hálózaton. **virtuális hálózat címterület hello az IP-címtartományok, mely igények toobe benyújtott tooMicrosoft kérése az első központi telepítése során**

- **Az Azure virtuális gép alhálózat IP-címtartomány:** az IP-címtartományt, korábban már című szakaszban leírtaknak megfelelően hello egy hozzárendelt (vagy tooassign megtervezése) toohello Azure VNet alhálózati paramétere az Azure virtuális hálózat Csatlakozás toohello SAP HANA nagy példány környezet . Az IP-címtartomány használt tooassign IP-címek tooyour Azure virtuális gépeken. hello IP-címek a tartományon kívül tooconnect tooyour SAP HANA nagy példány kiszolgáló (k) használata engedélyezett. Ha szükséges, a több Azure Virtuálisgép-alhálózatok is használható. A/24 CIDR-blokkja Microsoft által ajánlott minden Azure Virtuálisgép-alhálózathoz. Ez a címtartomány hello Azure VNet címterület használt hello értékek egy részének kell lennie. Hogyan tooget IP-címtartomány? A vállalati hálózat team vagy szolgáltatás szolgáltató IP-címtartományt, amely jelenleg nincs használatban a hálózaton belüli kell biztosítania.

- **Virtuális hálózat átjáró alhálózati IP-címtartomány:** attól függően, hogy hello tervezi toouse, hello szolgáltatások ajánlott méret lenne:
   - Ultranagy teljesítményű ExpressRoute-átjáró: / 26 címterület - típus II osztály SKU szükséges
   - A VPN- és egy nagy teljesítményű ExpressRoute-átjáró használatával ExpressRoute létezzenek (vagy kisebb): / 27 címterülete
   - Más helyzetekben: / 28 címterület. Ez a címtartomány hello "VNet címterület" értékek használt hello értékek egy részének kell lennie. Ez a címtartomány hello Azure VNet címterület értékeket, hogy kell-e toosubmit tooMicrosoft használt hello értékek egy részének kell lennie. Hogyan tooget IP-címtartomány? A vállalati hálózat team vagy szolgáltatás szolgáltató IP-címtartományt, amely jelenleg nincs használatban a hálózaton belüli kell biztosítania. 

- **ER-P2P csatlakozási címtartomány:** ezt a tartományt a SAP HANA nagy példány ExpressRoute (ER) P2P kapcsolat hello IP-címtartománya. Ez az IP-címeknek kell lenniük egy /29 CIDR IP-címtartományt. Ebben a tartományban kell nem lehetnek a helyszíni vagy más Azure IP-címtartományokat. Az IP-címtartomány használt tooset mentése az Azure VNet ExpressRoute-átjáró toohello SAP HANA nagy példány kiszolgálók hello ER kapcsolat. Hogyan tooget IP-címtartomány? A vállalati hálózat team vagy szolgáltatás szolgáltató IP-címtartományt, amely jelenleg nincs használatban a hálózaton belüli kell biztosítania. **A tartomány a következő IP-címtartományt, mely igények toobe benyújtott tooMicrosoft kérése az első központi telepítése során:**
  
- **Kiszolgáló IP-készlet címtartománya:** az IP-címtartomány használt tooassign hello egyedi IP-tooHANA nagy példány kiszolgálóit. hello ajánlott alhálózat méretét egy/24 CIDR blokkolása - Ha azonban szükség lehet kisebb tooa legalább 64 IP-címek megadása. Ezt a tartományt a hello első 30 IP-címek számára vannak fenntartva a Microsoft által. Győződjön meg arról, ez a tény biztonságos kimenekítésére is hangsúlyt hello tartomány hello méretére kiválasztásakor. Ebben a tartományban kell nem lehetnek a helyszíni vagy más Azure IP-címeket. Hogyan tooget IP-címtartomány? A vállalati hálózat team vagy szolgáltatás szolgáltató biztosítania kell az IP-címtartományok, amely jelenleg nincs használatban a hálózaton belül. Egy/24 hello Azure (nagy példányok) SAP Hana szükséges adott IP-címek hozzárendelése használt toobe (ajánlott) egyedi CIDR blokkolása. **A tartomány a következő IP-címtartományt, mely igények toobe benyújtott tooMicrosoft kérése az első központi telepítése során:**
 
Bár toodefine kell, és tervezze meg hello IP-címtartományok fenti, nem minden őket továbbított toobe tooMicrosoft kell. toosummarize hello a fenti hello IP-címtartományok szükséges tooname tooMicrosoft áll a következők:

- Az Azure VNet cím tárolóhelyeit
- ER-P2P kapcsolat címtartománya
- Kiszolgáló IP-készlet címtartománya

További Vnetek tooconnect tooHANA nagy példányok igénylő hozzáadása szükséges toosubmit hello új Azure VNet címtartományt tooMicrosoft adja hozzá. 

Az alábbiakban az hello különböző tartományokhoz és néhány példa tartományok például tooconfigure és végül adjon a tooMicrosoft meg. Ahogy látja, hello Azure VNet címterület hello értéke nem összesített értéket az első példában hello, de hello első Azure virtuális alhálózat IP-címtartomány és hello hálózatok átjáró alhálózati IP-címtartomány hello tartomány alapján van definiálva. Volna munkahelyi ennek megfelelően konfigurálásával és a Küldés hello további IP hello Azure virtuális hálózaton belül több Virtuálisgép-alhálózatok használata címtartományainak hello hello Azure VNet címterület részeként további Virtuálisgép-alhálózat.

![Az IP-címtartományok SAP HANA szükséges minimális üzemelő Azure (nagy példány)](./media/hana-overview-connectivity/image4b-ip-addres-ranges-necessary.png)

Akkor is tooMicrosoft elküldését hello-adatok összesítésének hello lehetőségét. Ebben az esetben hello hello Azure virtuális hálózat címtartománya csak tartalmazhat egy szóközzel. Hello hello példában korábban használt IP-címtartományok használatával. A virtuális hálózat összesített címtartomány nézhet:

![Második lehetőségét, amely az IP-címtartományok SAP HANA szükséges minimális üzemelő Azure (nagy példány)](./media/hana-overview-connectivity/image5b-ip-addres-ranges-necessary-one-value.png)

Helyett két kisebb tartományt megadva címterében hello hello Azure virtuális hálózatot, a fent látható, amely lefedi a 4096 IP-címek egy nagyobb tartományon van. Ilyen hello címterület nagy meghatározását elhagyja néhány ahelyett, hogy nagy adattartományokat vizsgálnak nem használt. Hello VNet címterület érték(ek) végzi, a BGP útválasztási propagálás, mivel használata nem használt tartományok a helyszíni hello, vagy máshol a hálózat útválasztási problémákat okozhatnak. Ezért ajánlott tookeep hello címterület szorosan hello tényleges alhálózati címterület használt összhangban. Ha szükséges, a hello VNet állásidő nélkül mindig értékeket is hozzáadhat új címterület később.
 
> [!IMPORTANT] 
> Minden IP-cím tartomány a ER-P2P, kiszolgáló IP-készlet, Azure virtuális hálózat címterület kell **nem** átfedésben vannak egymással vagy bármely más tartomány használt valahol máshol a hálózat; egyes kell lennie, és két hello grafikus korábbi megjelenítése nem lehet egy bármely más tartomány alhálózat. Tartományok között átfedések fordulhat elő, ha hello Azure virtuális hálózat nem lehetséges, hogy kapcsolatot toohello ExpressRoute-kapcsolatcsoportot.

### <a name="next-steps-after-address-ranges-have-been-defined"></a>Címtartomány definiált követő lépések
Hello IP-címtartományok meghatározása után hello következő tevékenységek kell toohappen:

1. Küldje el a hello IP-címtartományok Azure VNet címterület, hello ER-P2P kapcsolat és a kiszolgáló IP-készlet címtartománya együtt más hello dokumentum hello elején szereplő adatok. Ezen a ponton az időben is sikerült megkezdése toocreate hello virtuális hálózatot és Virtuálisgép-alhálózatok hello. 
2. Az Express Route-körhöz hozta létre a Microsoft hello HANA nagy példány stamp a Azure-előfizetés között.
3. A bérlői hálózat Microsoft által készített hello nagy példány stamp.
4. Microsoft konfigurálja az SAP HANA hello Azure (nagy példányok) infrastruktúra tooaccept a hálózat IP-címek az Azure VNet címteret kommunikáló HANA nagy osztályt.
5. Attól függően, hogy az Azure (nagy példányok) SKU adott SAP HANA vásárolt hello Microsoft hozzárendel egy számítási egység a bérlői hálózat, lefoglalása és tároló csatlakoztatása és hello operációs rendszer (SUSE vagy Red Hat Linux) telepítése. IP-címet a egységeket hello kiszolgáló IP-készlet címtartomány Ön által küldött tooMicrosoft kiveszik.

Hello központi telepítési folyamat végén hello Microsoft kézbesíti a következő adatok tooyou hello:
- Szükséges adatok tooconnect az Azure VNet(s) toohello ExpressRoute-kapcsolatcsoportot, amely összeköti az Azure Vnetekhez tooHANA nagy példányok:
     - Hitelesítési kulcs vagy kulcsok
     - ExpressRoute PeerID
- Adatok tooaccess HANA nagy példányok követően létrehozott ExpressRoute-kapcsolatcsoportot és az Azure virtuális hálózatot.

Hello feladatütemezési HANA nagy példányok csatlakozás hello dokumentumban talál [tooEnd SAP HANA-nagy példányok telepítő befejezése](https://azure.microsoft.com/resources/sap-hana-on-azure-large-instances-setup/). A lépéseket követve hello számos központi telepítésre példát a nevezett dokumentum láthatók. 


## <a name="connecting-a-vnet-toohana-large-instance-expressroute"></a>Csatlakozás a virtuális hálózat tooHANA nagy példány ExpressRoute

Definiált összes hello IP-címtartományok, és most kapott hello adatok vissza a Microsoft, a csatlakozás hello tooHANA nagy példányok előtt létrehozott VNet is elindítható. Egyszer hello Azure virtuális hálózat jön létre, ExpressRoute-átjárót kell létrehozni hello VNet toolink hello VNet toohello toohello ügyfél bérlői hello nagy példány stamp csatlakozó ExpressRoute-kapcsolatcsoportot.

> [!NOTE] 
> Ez a lépés akár is igénybe vehet too30 perc toocomplete, hello új átjáró létrehozása a kijelölt Azure-előfizetés hello és a csatlakoztatott toohello kötelező Azure virtuális hálózatot.

Ha már létezik egy átjáró, ellenőrizze, hogy azt egy ExpressRoute-átjárót, vagy nem. Ha nem, akkor hello átjáró kell törölni, majd újból ExpressRoute átjáróként. Ha egy ExpressRoute-átjáró már létrejött, mivel az Azure virtuális hálózat már hello toohello ExpressRoute-kapcsolatcsoportot a helyi kapcsolat kapcsolódnak, lépjen a toohello Linking Vnetek az alábbi szakasz.

- Használjon vagy hello (új) [Azure-portálon](https://portal.azure.com/), vagy a PowerShell toocreate egy ExpressRoute VPN-átjáró csatlakoztatva tooyour virtuális hálózat.
  - Ha hello Azure-portált használja, adjon hozzá egy új **virtuális hálózati átjáró** majd **ExpressRoute** hello átjáró típusként.
  - Ha inkább PowerShell, először töltse le, és használja legújabb hello [Azure PowerShell SDK](https://azure.microsoft.com/downloads/) tooensure az optimális működés érdekében. a következő parancsok hello hozzon létre egy ExpressRoute-átjárót. hello szövegek utasításnak egy  _$_  vannak felhasználó által definiált változókat, hogy szükség toobe frissült a kért adatokat.

```PowerShell
# These Values should already exist, update toomatch your environment
$myAzureRegion = "eastus"
$myGroupName = "SAP-East-Coast"
$myVNetName = "VNet01"

# These values are used toocreate hello gateway, update for how you wish hello GW components toobe named
$myGWName = "VNet01GW"
$myGWConfig = "VNet01GWConfig"
$myGWPIPName = "VNet01GWPIP"
$myGWSku = "HighPerformance" # Supported values for HANA Large Instances are: HighPerformance or UltraPerformance

# These Commands create hello Public IP and ExpressRoute Gateway
$vnet = Get-AzureRmVirtualNetwork -Name $myVNetName -ResourceGroupName $myGroupName
$subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
New-AzureRmPublicIpAddress -Name $myGWPIPName -ResourceGroupName $myGroupName `
-Location $myAzureRegion -AllocationMethod Dynamic
$gwpip = Get-AzureRmPublicIpAddress -Name $myGWPIPName -ResourceGroupName $myGroupName
$gwipconfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name $myGWConfig -SubnetId $subnet.Id `
-PublicIpAddressId $gwpip.Id

New-AzureRmVirtualNetworkGateway -Name $myGWName -ResourceGroupName $myGroupName -Location $myAzureRegion `
-IpConfigurations $gwipconfig -GatewayType ExpressRoute `
-GatewaySku $myGWSku -VpnType PolicyBased -EnableBgp $true
```

Ebben a példában a hello HighPerformance gateway SKU lett megadva. Az alábbi lehetőségek állnak HighPerformance vagy UltraPerformance hello csak gateway SKU-n SAP Hana Azure (nagy példányok) által támogatott módon.

> [!IMPORTANT]
> Az hello SKU nagy példányai HANA meg kell adnia S384, S384m, S384xm, S576, S768, és S960 (II osztály termékváltozatok típusa), a hello UltraPerformance átjáró-Termékváltozat hello használata kötelező.

### <a name="linking-vnets"></a>Hivatkozási Vnetek

Most, hogy hello Azure virtuális hálózat rendelkezik egy ExpressRoute-átjáró, használhat hello engedélyezési információ Microsoft tooconnect hello ExpressRoute átjáró toohello SAP HANA (nagy példányok) Azure ExpressRoute-kapcsolatcsoportot létrehozni a kapcsolatot. Ez a lépés hello Azure-portál vagy PowerShell-lel hajtható végre. hello portal ajánlott, azonban PowerShell-utasításokat a következők. 

- A következő parancsokat minden egyes virtuális hálózat átjárót egy másik AuthGUID minden egyes kapcsolathoz hello végrehajtása. hello hello parancsfájl következő látható első két bejegyzések hello információkat a Microsoft által biztosított határozza meg. Hello AuthGUID is minden virtuális hálózat és az átjáróhoz. Azt jelenti, ha azt szeretné, tooadd egy másik Azure virtuális hálózatot, be kell tooget egy másik AuthID a HANA nagy példányok csatlakozik az Azure ExpressRoute-kapcsolatcsoport esetében. 

```PowerShell
# Populate with information provided by Microsoft Onboarding team
$PeerID = "/subscriptions/9cb43037-9195-4420-a798-f87681a0e380/resourceGroups/Customer-USE-Circuits/providers/Microsoft.Network/expressRouteCircuits/Customer-USE01"
$AuthGUID = "76d40466-c458-4d14-adcf-3d1b56d1cd61"

# Your ExpressRoute Gateway Information
$myGroupName = "SAP-East-Coast"
$myGWName = "VNet01GW"
$myGWLocation = "East US"

# Define hello name for your connection
$myConnectionName = "VNet01GWConnection"

# Create a new connection between hello ER Circuit and your Gateway using hello Authorization
$gw = Get-AzureRmVirtualNetworkGateway -Name $myGWName -ResourceGroupName $myGroupName
    
New-AzureRmVirtualNetworkGatewayConnection -Name $myConnectionName `
-ResourceGroupName $myGroupName -Location $myGWLocation -VirtualNetworkGateway1 $gw `
-PeerId $PeerID -ConnectionType ExpressRoute -AuthorizationKey $AuthGUID
```

Ha azt szeretné, hogy tooconnect hello átjáró toomultiple ExpressRoute-Kapcsolatcsoportok az előfizetéshez társított, szükség lehet tooexecute ebben a lépésben egynél többször. Például valószínűleg fog tooconnect hello azonos virtuális hálózaton átjáró toohello ExpressRoute-kapcsolatcsoportot hello VNet tooyour helyi hálózathoz csatlakozó.

## <a name="adding-more-ip-addresses-or-subnets"></a>Több IP-cím vagy alhálózat hozzáadása

Több IP-címek és alhálózatok hozzáadásakor, használja a PowerShell vagy a CLI vagy hello Azure-portálon.

Ebben az esetben hello ajánljuk, tooadd hello új IP-címtartomány új tartomány toohello VNet címtér, egy új összevont tartományt generálása helyett. Mindkét esetben szüksége toosubmit e módosítás tooMicrosoft tooallow kapcsolat kívül, hogy az új IP cím tartomány toohello HANA nagy példány egységek az ügyfél. Az Azure támogatási kérelem tooget hello új VNet-címtér hozzáadott nyithatja meg. Megerősítés megjelenése után hajtsa végre a következő lépések hello.

toocreate egy további alhálózathoz hello Azure-portálon a hello cikke [hello Azure-portál virtuális hálózat létrehozása](../../../virtual-network/virtual-networks-create-vnet-arm-pportal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), és a Powershellből toocreate [PowerShellvirtuálishálózatlétrehozása](../../../virtual-network/virtual-networks-create-vnet-arm-ps.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="adding-vnets"></a>Vnetek hozzáadása

Egy vagy több Azure Vnetekhez kezdetben csatlakozás után érdemes tooadd újak Azure (nagy példányok) SAP HANA-et elérő. Először az Azure támogatási kérelmet, a kérésre magukban foglalják az hello konkrét információk azonosító hello adott Azure-telepítés, és hello IP cím terület tranzakciónaplók hello Azure VNet címtartomány a. Az Azure szolgáltatásfelügyelet SAP HANA majd hello tooconnect szükséges szükséges információkat biztosít további virtuális hálózatokat és ExpressRoute hello. Minden virtuális hálózaton kell egy egyedi Engedélykulcs tooconnect toohello ExpressRoute-Kapcsolatcsoportot tooHANA nagy példányok.

Egy új Azure VNet tooadd lépések:

1. Első lépéseként hello hello bevezetési folyamat befejeződik, tekintse meg a hello _Azure VNet létrehozása_ szakasz.
2. Második lépése hello hello bevezetési folyamat befejeződik, tekintse meg a hello _átjáróalhálózat létrehozásához_ szakasz.
3. tooconnect a további Vnetek toohello HANA nagy példány ExpressRoute-kapcsolatcsoportot, nyissa meg információkat az Azure támogatási kérést az új VNet hello, és igényeljen új hitelesítési kulcsot.
4. Ha értesítést arról, hogy hello engedélyezési befejeződik, hello Microsoft által biztosított engedélyezési információ toocomplete hello harmadik lépése hello bevezetési folyamat használja, tekintse meg a hello _Linking Vnetek_ szakasz.

## <a name="increasing-expressroute-circuit-bandwidth"></a>Az ExpressRoute-kapcsolatcsoport sávszélessége növelése

Vegye fel a kapcsolatot az Azure szolgáltatásfelügyelet SAP HANA. Ha elkeverni tooincrease hello sávszélesség az SAP HANA hello (nagy példányok) Azure ExpressRoute körön, hozzon létre egy Azure támogatási kérelmet. (Kérhet egy egyetlen kapcsolatcsoport sávszélessége tooa legfeljebb 10 GB/s fel egy növelését.) Ezt követően kap értesítést hello befejezése után végezze el; További szükség semmilyen műveletre tooenable a nagyobb sebesség az Azure-ban.

## <a name="adding-an-additional-expressroute-circuit"></a>További ExpressRoute-kapcsolatcsoportot hozzáadása

Vegye fel a kapcsolatot SAP HANA az Azure szolgáltatásfelügyelet, ha azt javasolja, hogy további ExpressRoute-kapcsolatcsoportot van szükség, hogy az Azure támogatási kérelem toocreate, egy új ExpressRoute-kapcsolatcsoportot (és a tooget engedélyezési adatokat tooconnect tooit). hello címterület, amelyet hello Vnetek ahhoz, hogy SAP HANA az Azure szolgáltatásfelügyelet tooprovide engedélyezési hello kérelem végrehajtása előtt definiálni kell használni.

Miután hello új kapcsolat jön létre, és az SAP HANA hello Azure szolgáltatásfelügyelet konfiguráció befejeződött, fog tooreceive értesítési hello információkat tooproceed szükségesek. Hajtsa végre a hello itt hoz létre és hello új virtuális hálózat toothis további áramkör csatlakoztatja a fent ismertetett lépéseket. Még nem tud tooconnect Azure Vnetekhez toothis további áramkör már kapcsolódnának (nagy példány) Azure ExpressRoute körön hello az SAP HANA tooanother egy Azure-régióban.

## <a name="deleting-a-subnet"></a>Egy alhálózat törlése

tooremove VNet alhálózat, PowerShell vagy a CLI vagy hello Azure-portálon is használható. Abban az esetben, ha az Azure VNet IP cím tartomány vagy az Azure VNet címterület volt egy összevont tartományt, nincs nem a Microsoft az Ön szolgáltatáshoz hajtsa végre. Kivéve, hogy hello VNet továbbra is propagálása BGP útvonal címtartomány törlése hello alhálózatot tartalmaz. Ha hello Azure VNet IP cím tartomány vagy az Azure VNet címterület meghatározott több IP-címtartományok melyik érkezett törölt hozzárendelt tooyour alhálózati, törölje, amely a virtuális hálózat címterület kívül, és ezt követően tájékoztatja a Azure szolgáltatásfelügyelet SAP HANA hello azt, hogy az Azure (nagy példányok) SAP HANA címtartományok tooremove toocommunicate az engedélyezett.

Nem létezik, de egyedi, dedikált Azure.com webhelyre útmutatást alhálózatok eltávolítása, hello alhálózatok eltávolításának folyamata közben hello hello folyamat hozzáadja őket a fordított irányú. Hello cikke [hello Azure-portál virtuális hálózat létrehozása](../../../virtual-network/virtual-networks-create-vnet-arm-pportal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) alhálózatok létrehozásáról további információt.

## <a name="deleting-a-vnet"></a>A virtuális hálózat törlése

Használja a PowerShell vagy a CLI vagy hello Azure-portálon, ha egy virtuális hálózat törlése. Az Azure szolgáltatásfelügyelet SAP HANA eltávolítja a meglévő engedélyek hello az SAP HANA hello (nagy példány) az Azure ExpressRoute körön, és távolítsa el a hello Azure VNet IP cím tartomány vagy az Azure VNet címtartomány hello kommunikációs HANA nagy osztályt.

Hello VNet az Eltávolítás után, nyissa meg az Azure támogatási kérelem tooprovide hello IP-címtér range(s) toobe eltávolítva.

Amíg nem létezik, de egyedi, dedikált Azure.com webhelyre útmutatást Vnetek eltávolítása, a Vnetek eltávolításához hello folyamat hello hello folyamatának való hozzáadásukkal, amely a fent ismertetett fordított irányú. Lásd: hello cikkek [hello Azure-portál virtuális hálózat létrehozása](../../../virtual-network/virtual-networks-create-vnet-arm-pportal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) és [PowerShell virtuális hálózat létrehozása](../../../virtual-network/virtual-networks-create-vnet-arm-ps.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) Vnetek létrehozásáról további információt.

tooensure mindent távolítja el, törölje a következő elemek hello:

- ExpressRoute-kapcsolatot, a virtuális hálózat átjáró hello hálózatok átjáró nyilvános IP-címet, és virtuális hálózat

## <a name="deleting-an-expressroute-circuit"></a>Egy ExpressRoute-kapcsolatcsoport törlése

tooremove egy további SAP HANA (nagy példányok) Azure ExpressRoute körön, nyissa meg az Azure támogatási kérelmet az Azure szolgáltatásfelügyelet SAP HANA, és kérje a hello áramkör törölni kell. Belül hello Azure-előfizetéssel törölheti vagy hello VNet, szükség esetén hagyja. Azonban törölnie kell az hello kapcsolat hello HANA nagy példányok ExpressRoute-kapcsolatcsoport között, és hello kapcsolódó virtuális hálózatot átjáró.

Ha szeretné tooremove egy Vnetet, kövesse a hello útmutatást a fenti hello részben egy virtuális hálózat törlése.


