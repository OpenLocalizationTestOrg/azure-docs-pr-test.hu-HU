---
title: "aaaAzure hálózati ajánlott biztonsági eljárások |} Microsoft Docs"
description: "Ismerje meg, néhány hello fő szolgáltatásainak Azure toohelp biztonságos hálózati környezetben létrehozása"
services: virtual-network
documentationcenter: na
author: tracsman
manager: rossort
editor: 
ms.assetid: d169387a-1243-4867-a602-01d6f2d8a2a1
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/03/2017
ms.author: jonor
ms.openlocfilehash: b851b2862428a8bd5e7525c85584fc1c14ffcabe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="microsoft-cloud-services-and-network-security"></a>A Microsoft cloud services és a hálózati biztonság
A Microsoft cloud services csomag kézbesítése kapacitású szolgáltatások és az infrastruktúra, a vállalati szintű képességet és a számos választhat való hibrid kapcsolathoz. Az ügyfelek választható tooaccess hello interneten keresztül vagy a magánhálózati kapcsolatot biztosít, az Azure ExpressRoute ezeket a szolgáltatásokat. hello Microsoft Azure platform lehetővé teszi az ügyfelek tooseamlessly bővítheti az infrastruktúrát hello felhőbe, és hozhat létre többrétegű architektúrák. Emellett harmadik felek engedélyezéséhez bővített szolgáltatásokat nyújtó biztonsági szolgáltatásait és a virtuális készülékek. Ez a dokumentum áttekintést nyújt biztonsági és architekturális problémák, az ügyfelek figyelembe kell venni a Microsoft más felhőszolgáltatásaival ExpressRoute keresztül elért használatakor. Azt is magában foglalja biztonságosabb szolgáltatások létrehozása az Azure virtuális hálózatot.

## <a name="fast-start"></a>Gyors indítás
a következő logikai diagram hello utasíthatja meg tooa adott példa hello hello Azure platformon elérhető számos biztonsági módszerek. Gyors referenciaként található hello például, hogy a legjobban megfelel-e az Ön esetében. Bővített ismereteket szeretnének elsajátítani folytassa a keresztül hello dokumentum olvasása.
[![0]][0]

[1. példa: Build a szegélyhálózaton (más néven DMZ, demilitarizált zóna vagy demilitarizált) toohelp hálózati biztonsági csoportokkal (NSG-k)-alkalmazások védelmét.](#example-1-build-a-perimeter-network-to-help-protect-applications-with-nsgs)</br>
[2. példa: Build a külső hálózati toohelp tűzfal és az NSG-alkalmazások védelmét.](#example-2-build-a-perimeter-network-to-help-protect-applications-with-a-firewall-and-nsgs)</br>
[3. példa: Build a külső hálózati toohelp védelme hálózatokat és a tűzfalon, felhasználó által megadott útvonal (UDR) és NSG.](#example-3-build-a-perimeter-network-to-help-protect-networks-with-a-firewall-and-udr-and-nsg)</br>
[4. példa: Egy hibrid kapcsolat-helyek, virtuális berendezés virtuális magánhálózat (VPN) hozzáadása.](#example-4-add-a-hybrid-connection-with-a-site-to-site-virtual-appliance-vpn)</br>
[5. példa: Egy pont-pont, Azure VPN-átjáró hibrid kapcsolat hozzáadása.](#example-5-add-a-hybrid-connection-with-a-site-to-site-azure-vpn-gateway)</br>
[6. példa: ExpressRoute hibrid kapcsolat hozzáadása.](#example-6-add-a-hybrid-connection-with-expressroute)</br>
Például a virtuális hálózatok, a magas rendelkezésre állás és a szolgáltatás láncolás közötti kapcsolatok hozzáadása megkapja toothis dokumentum keresztül hello következő néhány hónap.

## <a name="microsoft-compliance-and-infrastructure-protection"></a>Microsoft megfelelőségi és infrastruktúra védelme
toohelp szervezetek ahhoz nemzeti, regionális, és iparág-specifikus követelmények betartatását hello gyűjtése és felhasználása egyének adatok, több mint 40 tanúsítványokról és tanúsítványokat a Microsoft kínál. minden részletre hello készlet összes felhőbeli szolgáltatás szolgáltatója.

További információkért lásd: hello megfelelőségi információkat a hello [Microsoft Trust Center][TrustCenter].

A Microsoft egy átfogó megközelítés tooprotect szükséges infrastruktúrát toorun kapacitású globális felhőszolgáltatások rendelkezik. A Microsoft felhőalapú infrastruktúra tartalmazza a hardverek, szoftverek, hálózatok, és a felügyeleti és továbbá a műveleti személyzet toohello fizikai az adatközpontok.

![2]

Ezt a módszert használja az ügyfelek toodeploy több biztonságos alapot nyújt hello Microsoft felhőalapú szolgáltatásokat. következő lépés hello ügyfelek toodesign, és hozzon létre egy biztonsági architektúra tooprotect ezeket a szolgáltatásokat.

## <a name="traditional-security-architectures-and-perimeter-networks"></a>Hagyományos hálózatbiztonsági architektúrák és szegélyhálózat
Bár Microsoft fokozottan fektet hello felhő-infrastruktúra védelméhez, ügyfelek is kell védeni a felhőszolgáltatások és erőforrás-csoportok. A többrétegű megközelítés toosecurity hello legjobb védelmet nyújt. A külső hálózati biztonsági szintű zónában védi a belső hálózati erőforrásokhoz a nem megbízható hálózaton. Szegélyhálózaton toohello szélén vagy hello hálózati azon részeit, amelyek között hello Internet és védett hello vállalati informatikai infrastruktúra elhelyezkedő hivatkozik.

A vállalati hálózatokban hello alapvető infrastruktúra fokozottan dúsítjuk: hello kialakítását, a biztonsági eszközök rétege. hello határ az egyes rétegek eszközök és a házirend-érvényesítési pontok áll. Minden egyes réteg a hálózati biztonsági eszközök a következő hello kombinációját tartalmazhatja: tűzfalak, szolgáltatásmegtagadásos (DoS) megelőzése, behatolásérzékelési vagy (Azonosítók/IP-CÍMEK) rendszereket és VPN-eszközök. Házirend betartatása tűzfal házirendek, a hozzáférés-vezérlési listák (ACL) vagy a megadott útválasztási hello formáját is igénybe vehet. hello első védelmi vonalat a hello hálózat közvetlenül elfogadja a bejövő forgalom hello Internet, miközben lehetővé teszi további jogos kérelmek hello hálózatra ezen mechanizmusok tooblock támadások és káros forgalom kombinációjából. A forgalmat továbbítja az közvetlenül tooresources hello szegélyhálózaton. Adott erőforrás majd "beszél" hello hálózaton áthaladó hello következő határ érvényesítéshez mélyebb tooresources első. hello legkülső réteg hello szegélyhálózaton nevezik, mivel ezen része hello hálózati kitett toohello internetes, általában valamilyen védelmi mindkét oldalon. hello alábbi ábrán is látható példáját mutatja be egy alhálózat szegélyhálózaton a vállalati hálózat, a két biztonsági határokat.

![3]

Nincsenek sok használt architektúrák tooimplement a szegélyhálózaton. Ezek architektúrák változatos mechanizmusokkal, minden egyes határ tooblock forgalom egyszerű load balancer tooa több alhálózattal szegélyhálózaton között, és mélyebb rétegek hello hello vállalati hálózat védelme. Hello konkrét igényeinek hello szervezet és annak teljes kockázattűrése függ, hogy hogyan hello szegélyhálózaton épül.

Az ügyfelek a munkaterhelések toopublic felhők mozgatása, kritikus toosupport hasonló képességet biztosít a külső hálózati architektúra Azure toomeet megfelelőségi és biztonsági követelmények is. Ez a dokumentum hasznos útmutatást ad meg, hogyan hozhat létre az ügyfelek a biztonságos hálózati környezet az Azure-ban. Hello szegélyhálózaton összpontosít, de a hálózati biztonsági sok szempontból átfogó leírást is tartalmaz. a következő kérdések hello tájékoztatja az ismertető:

* Hogyan építhető is a szegélyhálózaton, az Azure-ban?
* Mik az egyes hello Azure-szolgáltatások rendelkezésre toobuild hello szegélyhálózaton?
* Hogyan háttér-munkaterhelések védelme?
* Hogyan történik az interneten vezérelt kommunikációs toohello munkaterhelések az Azure-ban?
* Hogyan hello a helyszíni hálózatokban védelme a központi telepítések az Azure-ban?
* Ha natív Azure biztonsági funkciók használ, és a külső eszközöknek és a szolgáltatások?

a következő diagram hello Azure biztosít toocustomers biztonsági különböző rétegeinek jeleníti meg. Ezek a rétegek natív hello Azure platformon, maga a, mind az ügyfél által megadott funkciók:

![4]

Bejövő hello Internet, Azure DDoS szemben Azure nagyméretű támadások elleni védelmét. hello következő réteg az ügyfél által megadott nyilvános IP-címek (végpont), amelyek használt toodetermine mely forgalom teljen hello cloud service toohello a virtuális hálózaton keresztül. Natív Azure virtuális hálózatelkülönítés biztosítja minden más hálózati teljes elkülönítést és, hogy csak a forgalom metódusok és konfigurált felhasználói elérési útját. Ezek elérési útja és módszerek hello következő rétege, ahol az NSG-k, UDR és virtuális hálózati berendezések lehet a használt toocreate biztonsági határokat tooprotect hello alkalmazástelepítések hello védett hálózat.

hello a következő szakasz az Azure virtuális hálózatok áttekintése. Ezek a virtuális hálózatok az ügyfelek által létrehozott, és mi az üzembe helyezett munkaterhelések csatlakozik. Virtuális hálózatok hello alapja a hello hálózati biztonsági szolgáltatásait szükséges tooestablish egy külső hálózati tooprotect felhasználói telepítés az Azure-ban.

## <a name="overview-of-azure-virtual-networks"></a>Az Azure virtuális hálózatok áttekintése
Mielőtt internetes forgalom toohello kaphat az Azure virtuális hálózatok, az alábbi két réteg biztonsági rejlő toohello Azure platformon:

1.    **DDoS-védelem**: DDoS-védelem Azure-fizikai hálózatot védő hello Azure platformon, maga a nagyméretű Internet-alapú támadások hello réteget. Ilyen támadások egy kísérlet toooverwhelm egy internetes szolgáltatás több "bot" csomópont használja. Azure rendelkezik egy robusztus DDoS védelem háló összes bemeneti, kimeneti és kereszt-Azure régió kapcsolata. DDoS védelem réteg nincsenek felhasználói konfigurálható attribútumai, és nem érhető el toohello ügyfél. hello DDoS védelmi réteget Azure védi a nagyméretű támadások platformként, azt is figyeli a kimenő kötött és a kereszt-Azure azon régióját forgalom. Hello virtuális hálózat virtuális hálózati berendezések használva rugalmasság további rétegei konfigurálhatja hello ügyfél egy olyan hello platform szintű védelmet vissza nem kisebb méretezési támadás ellen. Példa DDoS működés közben; Ha az internet felé néző IP-cím által a felügyeleti teendők központjaként DDoS-támadások támadásnak kitett erőforrásra, Azure ehhez hello források hello támadások észlelésére és megtisztítás a problémát okozó forgalom, mielőtt elérte a kívánt rendeltetési hello. Szinte minden esetben hello megtámadott végpont nem érinti a hello támadás. Hello bizonyos ritkán előforduló esetekben, hogy a végpont kihatással van nem forgalom érintett tooother végpontok, csak a megtámadott hello végpont. Így más ügyfelek és a szolgáltatások jelenik meg, hogy a támadások elleni nincs hatással. Fontos, hogy Azure DDoS csak keres a nagyméretű támadások toonote. Akkor lehet, hogy az adott szolgáltatás sikerült túlterhelik, mielőtt hello platform szintű védelmet küszöbérték túllépése. Például egy webhely egy A0 IIS kiszolgálón sikerült offline állapotra állítja a DDoS-támadások által előtt. az Azure platform szint DDoS-védelem fenyegetést.

2.  **Nyilvános IP-címek**: nyilvános IP-cím (keresztül Szolgáltatásvégpontok, nyilvános IP-cím címek, Alkalmazásátjáró és más Azure-szolgáltatások, amelyek egy nyilvános IP-cím toohello irányított internet tooyour erőforrás van engedélyezve) címek lehetővé teszik a felhőszolgáltatások vagy erőforráscsoportokat toohave nyilvános Internet IP-címek és portok közzétéve. hello végpont hello Azure-beli virtuális hálózat hálózati címfordítás (NAT) tooroute forgalom toohello belső címet és portot használja. Ez az elérési út hello elsődleges módja a külső forgalom toopass hello virtuális hálózathoz. hello nyilvános IP-címek mely forgalom átadott konfigurálható toodetermine, és hogyan és hol toohello virtuális hálózaton lefordítva.

Forgalom virtuális hálózati hello ér el, ha vannak, amelyek play számos szolgáltatást. Az Azure virtuális hálózatok vannak hello alap, amelyre az ügyfelek tooattach, a munkaterhelések és ahol az alapvető hálózati szintű biztonsági alkalmazza. Már egy magánhálózathoz (egy virtuális hálózati területre) az Azure-ban hello rendelkező ügyfelek esetén a következő szolgáltatásokat és jellemzők:

* **A forgalom elkülönítése**: virtuális hálózat az Azure platformon hello hello forgalom elkülönítési határt alkotnak. Virtuális gépek (VM) több virtuális hálózat közvetlenül egy másik virtuális hálózatban, még akkor is, ha mindkét virtuális hálózat által létrehozott tooVMs hello ugyanazon ügyfél nem tud kommunikálni. Elkülönítési kritikus tulajdonság, amely biztosítja a felhasználói virtuális gépeket, kommunikációs titkos virtuális hálózaton belül marad.

>[!NOTE]
>Adatforgalom-elkülönítés hivatkozik csak tootraffic *bejövő* toohello virtuális hálózat. Alapértelmezett kimenő forgalmát hello VNet toohello internet engedélyezett, de igény szerint NSG-ket megakadályozása.
>
>

* **Többrétegű topológia**: virtuális hálózatok lehetővé teszi az ügyfeleknek toodefine többrétegű topológia alhálózatok hozzárendelésének és különböző elemek vagy a munkaterhelések "réteg" külön címterek kijelölése. A logikai csoportok topológiák hello számítási feladattípust alapuló ügyfelek toodefine eltérő hozzáférési házirend engedélyezése és hello rétegek közötti forgalmat is szabályozhatja.
* **Közötti kapcsolatot nyújthassanak**: az ügyfelek közötti kapcsolatot nyújthassanak virtuális hálózat és több helyszíni hely vagy más virtuális hálózatok közötti hozhatnak létre az Azure-ban. a kapcsolat tooconstruct, az ügyfelek használhatják Vnetben társviszony-létesítést, Azure VPN Gatewayek, külső hálózat virtuális készülékek vagy ExpressRoute. Azure-helyek (közötti S2S) VPN szabványos IPsec/IKE protokoll és a saját ExpressRoute-kapcsolat használatával támogatja.
* **NSG** lehetővé teszi az ügyfelek toocreate szabályok (ACL) granularitási szükséges hello szintjén: hálózati felületek, az egyes virtuális gépeken vagy a virtuális alhálózatok. Ügyfelek hozzáférés vezérléséhez engedélyezése vagy megtagadása hello futó alkalmazások és szolgáltatások a virtuális hálózaton, az ügyfél hálózatokon keresztül közötti kapcsolatot nyújthassanak, rendszerek közötti kommunikáció vagy közvetlen Internet-kommunikációt.
* **UDR** és **IP-továbbítás** lehetővé teszi az ügyfeleknek toodefine hello kommunikációs útvonala a virtuális hálózaton belül különböző rétegek között. Ügyfelek központi telepítése egy tűzfal, Azonosítók/IP-CÍMEK és más virtuális készülékek, és irányítható a hálózati forgalom keresztül e biztonsági készülékek biztonsági határ házirendek betartatását, a naplózás és a hálózatfelügyeleti.
* **Virtuális készülékekre** az Azure piactér hello: biztonsági készülékek, mint például tűzfalak, terheléselosztók, és Azonosítók/IP-CÍMEK hello Azure piactéren elérhető, és hello VM-lemezkép gyűjteménye. A virtuális hálózatba szervezheti, és kifejezetten, a biztonsági határokat (beleértve a hello szegélyhálózati alhálózatok) egy többrétegű toocomplete biztonságos hálózati környezetben e készülékek telepítését.

Ezen funkciók és képességek például olyan hogyan egy külső hálózati architektúra sikerült létrehozni az Azure-ban a következő diagram hello:

![5]

## <a name="perimeter-network-characteristics-and-requirements"></a>Külső hálózati tulajdonságok és követelmények
hello szegélyhálózaton hello előtér hello hálózat közvetlenül kapcsolatba, hello Internet érkező kommunikációt. bejövő hello csomagok kell hello biztonsági készülékek, például a hello tűzfal, Azonosítók és IP-CÍMEK, áramlása hello háttér-kiszolgálók elérése előtt. Internet-kötött a hello munkaterhelések is adatcsomagok hello biztonsági készülékek hello peremhálózati a házirendek betartatását, vizsgálati és naplózási célokra, mielőtt elhagynák a hello hálózati keresztül. Emellett hello szegélyhálózaton tárolhatja, létesítmények közötti VPN-átjárók ügyfél virtuális hálózatokat és a helyszíni hálózat között.

### <a name="perimeter-network-characteristics"></a>Külső hálózati jellemzői
Hello előző ábra hivatkozik, néhány jó szegélyhálózaton hello jellemzői a következők:

* Az Internet felé néző:
  * hello szegélyhálózati hálózati alhálózatról Internet felé néző, hello Internet közvetlenül kommunikál.
  * Nyilvános IP-címek, a virtuális IP-címek és/vagy a végpontok adja át az internetes forgalmat toohello előtér-hálózati és eszközök.
  * Bejövő hello Internet áthalad előtt további erőforrások biztonsági eszközök hello előtér-hálózati forgalmát.
  * Kimenő biztonsági engedélyezve van, ha forgalmat haladnak keresztül biztonsági eszközöket hello utolsó lépésként toohello interneten előtt.
* Védett hálózati:
  * Nincs közvetlen útvonal a hello Internet toohello alapvető infrastruktúra.
  * Csatornák toohello alapvető infrastruktúra be kell járnia biztonsági eszközök, például az NSG-k, a tűzfalakon és a VPN-eszközök segítségével.
  * Más eszközök nem kell híd Internet és hello alapvető infrastruktúra.
  * Biztonsági eszközök mind hello internetre és hello védett hálózati hello szegélyhálózaton (például hello két tűzfal ikonok hello előző ábrán látható) határain irányuló ténylegesen lehet, hogy egy egyetlen virtuális készülék eltérő szabályokkal vagy minden határ felületek. Például egy fizikai eszköz, logikailag elválasztva, mindkét hello szegélyhálózaton határain hello terhelés kezelésére.
* Egyéb gyakori eljárások és korlátozások:
  * Munkaterhelések nem kell tárolnia a fontos üzleti adatok.
  * Hozzáférés és a frissítések tooperimeter hálózati konfigurációk és a központi telepítések is jogosult korlátozott tooonly rendszergazdák.

### <a name="perimeter-network-requirements"></a>Külső hálózati követelmények
tooenable ezeket a virtuális hálózati követelmények tooimplement sikeres szegélyhálózaton kövesse az alábbi irányelveket:

* **Alhálózati architektúra:** megadása hello virtuális hálózati úgy, hogy egy teljes alhálózathoz külön hello szegélyhálózaton, mint különíteni a hello alhálózatok azonos virtuális hálózaton. Ez az elkülönítés biztosítja hello forgalom hello szegélyhálózat és egyéb belső vagy privát alhálózati rétegek folyamatok tűzfalon vagy virtuális készülék Azonosítók/IP-CÍMEK között.  Felhasználó által definiált útvonalak alhálózat hello határt a szükséges tooforward a forgalom toohello virtuális készüléknek.
* **NSG:** hello szegélyhálózati hálózati alhálózatról hello internetes kommunikáció nyitott tooallow kell lennie, de, amely nem jelenti azt, az ügyfelek kell kihagyásával NSG-ket. Kövesse az általános biztonsági eljárásokat toominimize hello hálózati felületek kitett toohello Internet. Hello távoli címtartomány engedélyezett tooaccess hello központi telepítések vagy hello az adott alkalmazást protokollok és portok nyitva lévő zárolását. Előfordulhat, hogy olyan körülmények között, azonban a teljes zár ki nincs lehetőség. Például, ha a vevők külső webhelyek az Azure-ban, hello szegélyhálózaton lehetővé kell tennie a beérkező webes kérelmek hello bármely nyilvános IP-címekről, de csak meg kell nyitnia a hello webes alkalmazás portok: TCP 80-as porton és/vagy a TCP 443-as porton.
* **Útvonaltábla:** hello szegélyhálózati hálózati alhálózatról közvetlenül kell tudni toocommunicate toohello Internet, de ne engedélyezze, hogy közvetlen kommunikációt tooand hello hátsó vége vagy a helyszíni hálózat a tűzfalon keresztül nélkül, vagy biztonsági készülékek.
* **Biztonsági készülékek konfigurációs:** tooroute és vizsgálja meg a csomagok hello szegélyhálózat és hello többi védett hello hálózatok között, például a tűzfal, Azonosítók, biztonsági készülékek hello és IP-CÍMEK eszközök többhelyű. Előfordulhat, hogy rendelkeznek, külön hálózati adaptert hello szegélyhálózaton és hello háttér-alhálózatokat. hello szegélyhálózaton hello hálózati adapter közvetlen kommunikációra az hello Internet tooand, hello megfelelő NSG-ket és hello szegélyhálózati hálózati útválasztási táblázathoz. Csatlakozás toohello háttér-alhálózatok hello hálózati adapter több korlátozta NSG-ket és útválasztási táblázataiba hello megfelelő háttér-alhálózatokat.
* **Biztonsági átjárófunkció:** hello biztonsági készülékek jellemzően hello szegélyhálózaton telepített hajtsa végre a következő funkciók hello:
  * Tűzfal: A tűzfal- és hozzáférés-vezérlési házirendeket a bejövő kéréseket hello kényszerítése.
  * A fenyegetés felderítésére és megelőzésére: észlelésére és orvoslása hello Internet rosszindulatú támadások ellen.
  * A vizsgálati és naplózási: részletes naplózási vizsgálati és elemzési karbantartása.
  * Fordított proxy: átirányítás hello bejövő kérelmek toohello megfelelő háttér-kiszolgálók. Az átirányítást magában foglalja a leképezést, és általában fordítása hello célcímek hello előtér-eszközökön, tűzfalak, toohello háttér-kiszolgáló címei.
  * Proxy továbbítsa: NAT biztosítása, és végrehajtása belül hello virtuális hálózati toohello Internet által kezdeményezett kommunikáció naplózását.
  * Útválasztó: Továbbítja a bejövő és a döntést az alhálózatok közötti forgalmat hello virtuális hálózaton belül.
  * VPN-eszközön: Forráscsoportként hello létesítmények közötti VPN-átjárók a létesítmények közötti VPN-kapcsolat a helyszíni ügyfélhálózatok és az Azure virtuális hálózatok között.
  * VPN-kiszolgáló: elfogadása tooAzure virtuális hálózatok csatlakoztatása VPN-ügyfelek.

> [!TIP]
> A következő két csoportok külön hello megőrzése: hello egyéni felhasználók számára engedélyezett tooaccess hello szegélyhálózati hálózati biztonsági fogaskerék és hello egyéni felhasználók számára engedélyezett alkalmazások fejlesztési, a telepítés vagy a műveletek rendszergazdaként. Mivel külön kezeljük ezeket a csoportokat lehetővé teszi, hogy a feladatok elkülönítése, és megakadályozza, hogy egy személy kihagyásával alkalmazások biztonsági és a hálózati biztonsági vezérlők.
>
>

### <a name="questions-toobe-asked-when-building-network-boundaries"></a>Hálózati határok létrehozásakor feltett kérdésekre toobe
Ebben a szakaszban említett, hello "hálózat" kifejezés tooprivate Azure előfizetés-rendszergazda által létrehozott virtuális hálózatokat. hello kifejezés toohello mögöttes fizikai hálózatok az Azure nem utal.

Egy Azure virtuális hálózatot is gyakran használt tooextend hagyományos helyszíni hálózatokhoz. Már lehetséges tooincorporate webhelyek vagy ExpressRoute hibrid hálózatkezelési megoldások a peremhálózati hálózati architektúra. A hibrid kapcsolat a hálózati biztonsági határokat felépítése fontos szempont.

hello következő három kérdést kritikus tooanswer szegélyhálózaton és több biztonsági határon tartalmazó fejlesztéskor van.

#### <a name="1-how-many-boundaries-are-needed"></a>1.) hány határok van szükség?
hello első döntési tényező toodecide hány biztonsági határokat egy adott forgatókönyv esetén van szükség:

* Egyetlen határvonal: egy hello előtér-szegélyhálózaton hello virtuális hálózat és hello Internet között.
* Két határok: egy a hello hello szegélyhálózaton Internet oldalán, és egy másik hello szegélyhálózati hálózati és a háttér-alhálózatai hello között hello Azure virtuális hálózatot.
* Három határok: hello Internet oldalán hello peremhálózaton egy, és az egyik hello szegélyhálózat és a háttér-alhálózatok között, egy pedig hello háttér-alhálózatok és hello a helyszíni hálózat között.
* N határok: egy változó száma. Biztonsági követelményeitől függően a rendszer nem alkalmazható a megadott hálózati biztonsági határokat toohello száma.

hello száma és típusa eltérő szükséges határok alapján a vállalat kockázat tűréshatár és hello adott forgatókönyv megvalósítása. Ehhez a döntéshez gyakran történik a szervezeten belüli gyakran többek között a kockázat és megfelelőségi csoport, a hálózati és platform csoport és egy alkalmazást fejlesztői csapat több csoport együtt. Biztonsági, hello jár, és a használt hello technológiák Tudásbázis élők egy szóbeli kell rendelkezniük a döntési tooensure hello megfelelő biztonsági hatékonyabb védelem minden végrehajtásához.

> [!TIP]
> Hello legkisebb száma, amelyek megfelelnek egy adott helyzetre követelményei hello biztonsági határokat használja. További határokkal rendelkező műveletek és a hibaelhárítás is megnehezítheti, valamint adott idő alatt több határ házirend hello felügyeleti hello kezelésével járó nagyobb terhelés. Azonban elegendő határok növeljék. Keresés hello egyenleg fontos.
>
>

![6]

hello előző ábra mutatja a három biztonsági határ hálózat áttekintése látható. hello határok vannak hello szegélyhálózaton és hello Internet, hello Azure előtér- és háttér-titkos alhálózatait, és hello Azure háttér-alhálózat és hello helyszíni vállalati hálózat között.

#### <a name="2-where-are-hello-boundaries-located"></a>2.) hello határok helyét?
Miután határok száma hello dönt, ahol tooimplement őket a rendszer tovább döntési tényező hello. Nincsenek általában három választási lehetőségek:

* Az Internet alapú közbenső szolgáltatási (például egy felhőalapú webalkalmazási tűzfal, amely nem a dokumentumban ismertetett) használatával
* Natív szolgáltatások és/vagy a virtuális hálózati készülékek használata Azure-ban
* Fizikai eszközzel hello a helyszíni hálózat

Csak Azure-hálózatok, a hello beállítások natív Azure-szolgáltatások (például a Azure Load Balancer Terheléselosztók) vagy a hálózat a virtuális készülékek hello gazdag partner ökoszisztémájának alkalmazásával az Azure (például Check Point tűzfalak).

A határ Azure és a helyszíni hálózat között van szüksége, hello biztonsági eszközök a hello kapcsolat (vagy mindkét oldalon) mindkét oldalán találhatók. Így egy kell döntést hello hely tooplace biztonsági eszközökön.

Hello az előző ábrán hello Internet-peremhálózathoz és hello első-back-end-határok teljesen tartalmazza az Azure, és natív Azure-szolgáltatások vagy a virtuális hálózati berendezések értékűnek kell lennie. Biztonsági eszközök a hello Azure (háttér-alhálózat) közötti határokat, és hello vállalati hálózat bármelyik hello Azure ügyféloldali vagy hello helyszíni oldalon, vagy akár mindkét oldalon eszközök kombinációja lehet. Jelentős előnyeit és hátrányait tooeither beállítás súlyosan figyelembe veendő lehet.

Például hello a meglévő fizikai biztonsági eszközökön használatával a helyszíni hálózati oldal hello előnye, hogy szükség van-e az új állapotban van. Csak az újrakonfigurálás van szüksége. Hello, azonban hátránya, hogy az összes forgalom kell származnia vissza Azure toohello a helyszíni hálózati toobe hello biztonsági fogaskerék láthatók. Így az Azure-Azure traffic képes jelentős késés merülnek fel, és hatással vannak az alkalmazás teljesítményét és a felhasználói élmény, ha kénytelen volt az hátsó toohello helyszíni hálózatot biztonsági házirendek betartását.

#### <a name="3-how-are-hello-boundaries-implemented"></a>3.) hogyan vannak megvalósítva hello határok?
Minden biztonsági határ valószínűleg kell különböző képességekre vonatkozó követelmények (például Azonosítók és hello hello szegélyhálózaton, de csak ACL-ek hello szegélyhálózat és a háttér-alhálózat között Internet oldalán kiterjedő tűzfalszabályok). A legmegfelelőbb toouse attól függ, mely eszköz (vagy hány eszköz) hello forgatókönyv és a biztonsági követelményeket. A következő szakasz hello például 1, 2 és 3 bizonyos beállítások, amelyek felhasználhatók ismertetik. Hello Azure natív hálózati szolgáltatások és az Azure-ban elérhető hello partner ökoszisztéma hello eszközök felülvizsgálata hello tárfiókokhoz beállítások elérhető toosolve szinte bármilyen forgatókönyvet mutat.

Kulcs megvalósítási dönteni hogyan tooconnect hello helyszíni Azure-hálózat. Használja az Azure virtuális átjáró hello vagy a hálózati virtuális készülék? Ezek a beállítások (például 4, 5 és 6) szakaszban a következő hello részletesebben ismertetése.

Emellett az Azure virtuális hálózatok közötti forgalom lehet szükség. Ezek a forgatókönyvek a jövőbeli hello lesz hozzáadva.

Miután eldöntötte hello válaszok toohello előző kérdésre, hello [gyorsindítási](#fast-start) szakasz segítségével azonosítható, amelyek többek között a legmegfelelőbb az adott forgatókönyvnek.

## <a name="examples-building-security-boundaries-with-azure-virtual-networks"></a>Példák: Épület biztonsági határokat Azure virtuális hálózatokat
### <a name="example-1-build-a-perimeter-network-toohelp-protect-applications-with-nsgs"></a>1. példa Build egy külső hálózati toohelp az NSG-ket-alkalmazások védelmét.
[Biztonsági tooFast start](#fast-start) | [részletes utasításokat ehhez a példához összeállítása][Example1]

[![7]][7]

#### <a name="environment-description"></a>Környezet leírása
Ebben a példában a következő erőforrások hello tartalmazó előfizetés van:

- Egyetlen erőforráscsoportként működnek
- Két alhálózat virtuális hálózat: "Előtér" és "Háttér"
- A hálózati biztonsági csoport, amely alkalmazott tooboth alhálózatok
- A Windows server egy webalkalmazás-kiszolgáló ("IIS01") jelölő
- Két Windows-kiszolgálók, amelyek megfelelnek az alkalmazás háttérkiszolgálók ("AppVM01", "AppVM02")
- A Windows server DNS-kiszolgáló ("DNS01") jelölő
- Egy alkalmazás webkiszolgáló hello társított nyilvános IP-cím

Parancsfájlok és Azure Resource Manager-sablonokban: hello [részletes utasításokat build][Example1].

#### <a name="nsg-description"></a>NSG leírása
Ebben a példában egy NSG-csoport összeállítása és hat szabályokkal majd betölteni.

> [!TIP]
> Általánosan fogalmazva, készítsen az "Engedélyezés" speciális szabályok először általánosabbá hello követ "Elutasítás" szabályokat. kiemelt fontossággal hello határozzák meg, hogy mely szabályokat értékeli ki a rendszer először. Forgalom található tooapply tooa meghatározott szabály, ha nincsenek további szabályok kiértékelése. Az NSG-szabályok alkalmazhatók vagy hello bejövő vagy kimenő irányban (szempontjából hello hello alhálózati).
>
>

A következő szabályok hello deklaratív módon, a bejövő forgalom alatt beépített:

1. Belső DNS-forgalom (53-as port) engedélyezett.
2. Hello Internet tooany virtuális gép RDP forgalom (3389-es port) engedélyezett.
3. HTTP-forgalom (80-as port) hello internetes tooweb kiszolgáló (IIS01) engedélyezett.
4. Minden forgalmat (minden porthoz) IIS01 tooAppVM1 engedélyezett.
5. Minden forgalmat (minden porthoz) hello Internet toohello teljes virtuális hálózat (alhálózat mindkét) megtagadva.
6. Minden forgalmat (minden porthoz) hello előtér alhálózati toohello háttér-alhálózatból megtagadva.

E szabályok kötött tooeach alhálózattal, ha a HTTP-kérelem nem bejövő webkiszolgálóról hello Internet toohello, mindkét szabályok 3 (engedélyezése) és 5 (megtagadni) szeretné alkalmazni. De %3 szabályt egy magasabb prioritással bír, mert csak akkor vonatkozik, és 5 szabály nem lesz play kerülhet. Így hello HTTP-kérelem volna engedélyezett toohello webkiszolgáló. Ha ugyanaz a forgalom próbált tooreach hello DNS01 server, a szabályok 5 (megtagadni) első tooapply hello és hello forgalom volna nem engedélyezett toopass toohello kiszolgáló lenne. 6. szabály (megtagadni) blokkok hello van szó toohello háttér-alhálózat (kivéve a engedélyezett forgalom szabályokban 1 és 4) az előtér-alhálózatot. A szabálykészlet hello háttérhálózatot védi, abban az esetben, ha egy támadó gyengíti hello webalkalmazás hello kezelőfelületre. hello támadó volna csak korlátozott hozzáférés toohello "védett" háttérhálózatot (csak hello AppVM01 kiszolgálón kitett tooresources).

Nincs olyan alapértelmezett kimenő szabály, amely lehetővé teszi, hogy a kimenő toohello internetes forgalom. Ebben a példában a Microsoft most átengedi a kimenő forgalmat, és nem módosítja az egyetlen kimenő szabályok. toolock le mindkét irányú forgalmát, felhasználó által definiált útválasztási szükség (lásd a 3. példa:).

#### <a name="conclusion"></a>Összegzés
Ez a példa egy viszonylag egyszerű és magától értetődő mód a hello háttér-alhálózathoz, a bejövő forgalom elkülönítésére. További információkért lásd: hello [részletes utasításokat build][Example1]. Ezek az utasítások tartalmaznak:

* Hogyan toobuild a külső hálózati klasszikus PowerShell-parancsfájlokkal.
* Hogyan toobuild a külső hálózati az Azure Resource Manager sablonnal.
* Minden NSG parancs részletes leírását.
* Részletes forgalom folyamata forgatókönyvek, hogyan forgalom engedélyezett vagy megtagadott a két réteg megjelenítése.


### <a name="example-2-build-a-perimeter-network-toohelp-protect-applications-with-a-firewall-and-nsgs"></a>2. példa Build egy külső hálózati toohelp tűzfal és az NSG-alkalmazások védelmét.
[Biztonsági tooFast start](#fast-start) | [részletes utasításokat ehhez a példához összeállítása][Example2]

[![8]][8]

#### <a name="environment-description"></a>Környezet leírása
Ebben a példában a következő erőforrások hello tartalmazó előfizetés van:

* Egyetlen erőforráscsoportként működnek
* Két alhálózat virtuális hálózat: "Előtér" és "Háttér"
* A hálózati biztonsági csoport, amely alkalmazott tooboth alhálózatok
* Ebben az esetben a tűzfal, a hálózati virtuális készülék csatlakoztatott toohello előtér-alhálózat
* A Windows server egy webalkalmazás-kiszolgáló ("IIS01") jelölő
* Két Windows-kiszolgálók, amelyek megfelelnek az alkalmazás háttérkiszolgálók ("AppVM01", "AppVM02")
* A Windows server DNS-kiszolgáló ("DNS01") jelölő

Parancsfájlok és Azure Resource Manager-sablonokban: hello [részletes utasításokat build][Example2].

#### <a name="nsg-description"></a>NSG leírása
Ebben a példában egy NSG-csoport összeállítása és hat szabályokkal majd betölteni.

> [!TIP]
> Általánosan fogalmazva, készítsen az "Engedélyezés" speciális szabályok először általánosabbá hello követ "Elutasítás" szabályokat. kiemelt fontossággal hello határozzák meg, hogy mely szabályokat értékeli ki a rendszer először. Forgalom található tooapply tooa meghatározott szabály, ha nincsenek további szabályok kiértékelése. Az NSG-szabályok alkalmazhatók vagy hello bejövő vagy kimenő irányban (szempontjából hello hello alhálózati).
>
>

A következő szabályok hello deklaratív módon, a bejövő forgalom alatt beépített:

1. Belső DNS-forgalom (53-as port) engedélyezett.
2. Hello Internet tooany virtuális gép RDP forgalom (3389-es port) engedélyezett.
3. Bármely internetes forgalom (minden porthoz) toohello hálózati virtuális készülék (tűzfal) engedélyezve van.
4. Minden forgalmat (minden porthoz) IIS01 tooAppVM1 engedélyezett.
5. Minden forgalmat (minden porthoz) hello Internet toohello teljes virtuális hálózat (alhálózat mindkét) megtagadva.
6. Minden forgalmat (minden porthoz) hello előtér alhálózati toohello háttér-alhálózatból megtagadva.

Ezen szabályok kötött tooeach alhálózattal, ha a HTTP-kérelem nem bejövő hello Internet toohello tűzfaltól, mindkét szabályok 3 (engedélyezése) és 5 (megtagadni) szeretné alkalmazni. De %3 szabályt egy magasabb prioritással bír, mert csak akkor vonatkozik, és 5 szabály nem lesz play kerülhet. Így toohello tűzfal hello HTTP-kérelem szeretné tenni. Ha ugyanaz a forgalom próbált tooreach hello IIS01 kiszolgáló, annak ellenére, hogy hello előtér-alhálózaton van, a szabályok 5 (megtagadni) szeretné alkalmazni, és hello forgalom volna nem engedélyezett toopass toohello kiszolgáló. 6. szabály (megtagadni) blokkok hello van szó toohello háttér-alhálózat (kivéve a engedélyezett forgalom szabályokban 1 és 4) az előtér-alhálózatot. A szabálykészlet hello háttérhálózatot védi, abban az esetben, ha egy támadó gyengíti hello webalkalmazás hello kezelőfelületre. hello támadó volna csak korlátozott hozzáférés toohello "védett" háttérhálózatot (csak hello AppVM01 kiszolgálón kitett tooresources).

Nincs olyan alapértelmezett kimenő szabály, amely lehetővé teszi, hogy a kimenő toohello internetes forgalom. Ebben a példában a Microsoft most átengedi a kimenő forgalmat, és nem módosítja az egyetlen kimenő szabályok. toolock le mindkét irányú forgalmát, felhasználó által definiált útválasztási szükség (lásd a 3. példa:).

#### <a name="firewall-rule-description"></a>Tűzfal szabály leírása
Hello tűzfalon továbbítás szabályokat kell létrehozni. Mivel a példa csak internetes forgalmat az adathoz kötött útvonalak toohello tűzfal és a majd toohello webkiszolgáló, csak egy továbbító hálózati címfordítás (NAT) szabály van szükség.

szabály továbbítási hello bármely találatok tooreach HTTP (80-as vagy 443-as HTTPS port) tett kísérlet hello tűzfal bejövő forráscím fogad el. Hello tűzfal helyi illesztőfelületen kívül küldött, és átirányítja a toohello web server hello 10.0.1.5 IP-címét.

#### <a name="conclusion"></a>Összegzés
Ez a példa viszonylag egyszerű módja a hello háttér-alhálózathoz, a bejövő forgalom elkülönítése, valamint az alkalmazás egy tűzfal védi. További információkért lásd: hello [részletes utasításokat build][Example2]. Ezek az utasítások tartalmaznak:

* Hogyan toobuild a külső hálózati klasszikus PowerShell-parancsfájlokkal.
* Hogyan toobuild ebben a példában az Azure Resource Manager sablonnal.
* Minden NSG parancs és a tűzfalon szabály részletes leírását.
* Részletes forgalom folyamata forgatókönyvek, hogyan forgalom engedélyezett vagy megtagadott a két réteg megjelenítése.

### <a name="example-3-build-a-perimeter-network-toohelp-protect-networks-with-a-firewall-and-udr-and-nsg"></a>3. példa Build egy külső hálózati toohelp hálózatokat és a tűzfalon és UDR és NSG védelme
[Biztonsági tooFast start](#fast-start) | [részletes utasításokat ehhez a példához összeállítása][Example3]

[![9]][9]

#### <a name="environment-description"></a>Környezet leírása
Ebben a példában a következő erőforrások hello tartalmazó előfizetés van:

* Egyetlen erőforráscsoportként működnek
* Három alhálózat virtuális hálózat: "SecNet", "Előtér" és "Háttér"
* Ebben az esetben a tűzfal, a hálózati virtuális készülék toohello SecNet alhálózati csatlakoztatva
* A Windows server egy webalkalmazás-kiszolgáló ("IIS01") jelölő
* Két Windows-kiszolgálók, amelyek megfelelnek az alkalmazás háttérkiszolgálók ("AppVM01", "AppVM02")
* A Windows server DNS-kiszolgáló ("DNS01") jelölő

Parancsfájlok és Azure Resource Manager-sablonokban: hello [részletes utasításokat build][Example3].

#### <a name="udr-description"></a>UDR leírása
Alapértelmezés szerint a következő rendszerútvonalak hello is meg van adva:

        Effective routes :
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.0.0/16}     VNETLocal                            Active   Default    
         {0.0.0.0/0}       Internet                             Active   Default    
         {10.0.0.0/8}      Null                                 Active   Default    
         {100.64.0.0/10}   Null                                 Active   Default    
         {172.16.0.0/12}   Null                                 Active   Default    
         {192.168.0.0/16}  Null                                 Active   Default

hello VNETLocal mindig egy vagy több meghatározott címelőtagokat, hogy bizonyos hálózati hello virtuális hálózatot alkotó (Ez azt jelenti, hogy módosítja a virtuális hálózati toovirtual hálózatról, attól függően, hogy hogyan minden adott virtuális hálózati van meghatározva). hello fennmaradó rendszerútvonalak statikusak, és az alapértelmezett a hello tábla.

Ebben a példában két útválasztási táblázatok jönnek létre, egyet hello előtér- és alhálózatok. Minden tábla megfelelő a megadott alhálózati hello statikus útvonalakkal be van töltve. Ebben a példában minden egyes táblához három útvonalakat, amelyek minden forgalmat (0.0.0.0/0) közvetlen hello tűzfalon keresztül (a következő ugrás = virtuális készülék IP-cím):

1. A következő ugrás a helyi alhálózat forgalmának tooallow helyi alhálózati forgalom toobypass hello tűzfal definiálva.
2. Virtuális hálózati forgalmat a következő ugrásaként tűzfal meghatározva. A következő ugrás hello alapértelmezett szabály, amely lehetővé teszi a virtuális helyi hálózati forgalom tooroute közvetlenül felülbírálja.
3. Minden fennmaradó forgalmat (0/0) a következő ugrásaként hello tűzfal meghatározva.

> [!TIP]
> Nem rendelkező hello UDR oldaltörések helyi alhálózati kommunikációs hello helyi alhálózatra.
>
> * A jelen példában tooVNETLocal mutató 10.0.1.0/24 fontos! Nélkül csomag változatlanul hello Web Server (10.0.1.4) szánt tooanother helyi kiszolgáló (például) 10.0.1.25 sikertelen lesz, mivel azok toohello NVA kapnak. hello NVA toohello alhálózati küldi azt, és hello alhálózati fog küldeni toohello NVA e végtelen ciklusban.
> * hello útválasztási hurok a több hálózati adapterrel, amelyek csatlakoztatott tooseparate alhálózatok, ami általában hagyományos, a helyszíni készülékek rendelkező készülékek jellemzően magasabb.
>
>

Ha létrehozta a hello útválasztási táblázataiba, kötött tootheir alhálózatok kell lenniük. hello útvonalválasztási táblán, előtér-alhálózat, létrehozását követően kötött toohello alhálózati, a kimenetet jelenne meg:

        Effective routes :
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.1.0/24}     VNETLocal                            Active
         {10.0.0.0/16}     VirtualAppliance 10.0.0.4            Active    
         {0.0.0.0/0}       VirtualAppliance 10.0.0.4            Active

> [!NOTE]
> UDR mostantól lehet alkalmazott toohello átjáró-alhálózatot a mely hello ExpressRoute körön csatlakoztatva van.
>
> Hogyan tooenable a külső hálózat expressroute-on vagy a pont-pont hálózati példák láthatók példák 3. és 4.
>
>

#### <a name="ip-forwarding-description"></a>Az IP-továbbítás leírása
A kiegészítő funkció tooUDR IP-továbbítás. Az IP-továbbítás egy-egy virtuális készüléket, amely lehetővé teszi, hogy tooreceive nem kifejezetten forgalmat toohello készüléknek, majd továbbítsa a forgalmat tooits végső rendeltetési beállítást.

Ha AppVM01 kiszolgálóvá egy kérelem toohello DNS01, például UDR útvonal lenne a forgalom toohello tűzfal. Az IP-továbbítás engedélyezve van hello forgalom hello DNS01 cél (10.0.2.4) hello készülék (10.0.0.4) által elfogadott, és továbbítja a végső rendeltetési tooits (10.0.2.4). IP-továbbítást a hello tűzfal engedélyezve van, nem forgalom volna nem fogadja el hello készülék annak ellenére, hogy hello útvonaltábla legyen hello tűzfal hello következő ugrásként. toouse virtuális készüléknek, kritikus tooremember tooenable IP együtt UDR továbbítás.

#### <a name="nsg-description"></a>NSG leírása
Ebben a példában egy NSG-csoport összeállítása és kezelhető egyetlen szabállyal majd betölteni. Ez a csoport van, majd kötött csak toohello előtér- és alhálózatok (nem a SecNet hello). Deklaratív módon a következő szabály hello éppen készül:

* Minden forgalmat (minden porthoz) hello Internet toohello teljes virtuális hálózat (összes alhálózatot) megtagadva.

Bár ebben a példában NSG-ket használ, a fő célja azoknak a manuális helytelen konfiguráció másodlagos rétegként. hello célja tooblock teljes bejövő forgalmát hello Internet tooeither hello előtér- vagy a háttér-alhálózat. Forgalom csak áramlása hello SecNet alhálózati toohello tűzfal (, majd, ha szükséges, toohello előtér- vagy a háttér-alhálózatok). Plus hello UDR szabályokkal helyen, minden forgalom, amelyek adott hello az előtér- vagy a háttér-alhálózatok volna átirányítja kimenő toohello tűzfal (Köszönjük tooUDR). hello tűzfal jelenik meg a forgalmat egy aszimmetrikus folyamatként és hello kimenő forgalom volna eldobni. Így az alábbi három réteg biztonsági védelmének hello alhálózatok:

* Nincs nyilvános IP-címek előtér vagy háttér hálózati adaptert.
* Az NSG-k megtagadása forgalmat az hello Internet.
* hello tűzfal figyelmen kívül hagyása aszimmetrikus forgalom.

Vonatkozó hello NSG ebben a példában egy érdekes pont, hogy csak egy szabályt, amely toodeny internetes forgalom toohello teljes virtuális hálózat, beleértve a hello biztonság – alhálózati tartalmaz. Azonban mivel hello NSG csak van kötve toohello előtér- és háttér-alhálózatok, hello szabály nem feldolgozott forgalom bejövő toohello biztonság – alhálózati. Ennek eredményeképpen a forgalom toohello biztonság – alhálózati.

#### <a name="firewall-rules"></a>Tűzfalszabályok
Hello tűzfalon továbbítás szabályokat kell létrehozni. Mivel hello tűzfal blokkolja vagy továbbító minden bejövő, kimenő, és a belüli virtuális hálózati forgalmat, sok tűzfalszabályok van szükség. Emellett minden bejövő forgalom találatok hello biztonsági szolgáltatás nyilvános IP-cím (különböző portok), toobe feldolgozva hello tűzfal. Az ajánlott eljárás toodiagram hello logikai adatfolyamok hello alhálózatok beállítása előtt, és tűzfalszabályok, tooavoid átdolgozási később. hello alábbi ábrán is látható az ebben a példában hello tűzfalszabályok logikai nézetében:

![10]

> [!NOTE]
> A virtuális hálózati berendezések használt hello alapján, hello kezelőportjai eltérőek lehetnek. Ebben a példában a Barracuda NextGen tűzfal hivatkozik, 22, 801 és 807 portot használó. Tekintse át a hello készülék gyártói dokumentációjában toofind hello pontos használt portok használt hello eszköz kezelését.
>
>

#### <a name="firewall-rules-description"></a>Tűzfal-szabályok leírása
A logikai diagramja megelőző hello hello biztonság – alhálózati nem látható, mert hello tűzfal hello csak erőforrás adott alhálózaton. hello ábrán látható hello tűzfal szabályok és hogyan azok logikailag engedélyezi vagy megtagadja a forgalmat, nem hello tényleges irányított elérési utat. Is, hello külső portok kiválasztott hello RDP-forgalmát magasabb címkiosztási portok (8014 – 8026), és volt kijelölt tooloosely igazodni hello utolsó két oktetben hello helyi IP-cím könnyebb olvashatóság érdekében (például helyi kiszolgáló címe 10.0.1.4 társítva a külső portra 8014). Magasabb nem ütköző portja, azonban is használható.

Ebben a példában a hét típusú szabályokat kell:

* Külső szabályainak (bejövő forgalom):
  1. Felügyeleti tűzfalszabály: az alkalmazás átirányítási szabály lehetővé teszi, hogy forgalom toopass toohello kezelőportjai hello hálózati virtuális készülék.
  2. RDP-szabályok (az egyes Windows-kiszolgálók): A négy szabályok (kiszolgálónként egyet) engedélyezése hello kezelését az egyes kiszolgálók RDP-kapcsolaton keresztül. hello négy RDP szabályok is sikerült összecsukott egy szabályt, attól függően, hogy hello képességek hello hálózati virtuális készülék használják azokat.
  3. Alkalmazás forgalomra vonatkozó szabályok: kettő, a szabályok, a hello először adatforgalomhoz hello előtér-webkiszolgáló és a második a hello háttér-forgalom (például web server toodata réteg) hello. Ezek a szabályok hello konfigurálása hello hálózati architektúra (ahol a kiszolgálók kerülnek) és a forgalom (melyik irányba hello forgalmat, és mely portokat kell használni) függ.
     * hello első szabály lehetővé teszi, hogy a hello tényleges alkalmazás forgalom tooreach hello alkalmazáskiszolgáló. Hello más szabályok engedélyezik a biztonsági és felügyeleti, alkalmazás forgalomra vonatkozó szabályok is mi lehetővé teszik külső felhasználók vagy szolgáltatások tooaccess hello alkalmazások. Ehhez a példához nincs egyetlen webkiszolgálón való üzemeltetésekor 80-as porton. Így egyetlen alkalmazás tűzfalszabály átirányítja a bejövő forgalom toohello külső IP-toohello webes kiszolgálók belső IP-címet. hello átirányítva forgalom munkamenet volna fordítható NAT toohello belső kiszolgálón keresztül.
     * hello második szabály nem hello háttér-szabály tooallow hello web server tootalk toohello AppVM01 kiszolgáló (de nem AppVM02) bármely porton keresztül.
* Belső szabályainak (belüli virtuális hálózati forgalom)
  1. Kimenő tooInternet szabály: Ez a szabály lehetővé teszi, hogy minden hálózati forgalmat toopass kijelölt toohello hálózatok. Ez a szabály az általában egy alapértelmezett szabály már a hello tűzfalat, de a letiltott állapotú. Ez a szabály ehhez a példához engedélyezni kell.
  2. DNS-szabály: Ez a szabály lehetővé teszi, hogy csak a DNS (53-as port) forgalmat toopass toohello DNS-kiszolgáló. Ebben a környezetben a legtöbb forgalmat hello előtér toohello háttérből le van tiltva. Ez a szabály kifejezetten engedélyezi a DNS, az a helyi alhálózaton.
  3. Alhálózati toosubnet szabály: Ez a szabály van tooallow bármely kiszolgálón hello háttér-alhálózat tooconnect tooany hello előtér-alhálózat (de nem fordított hello).
* Hibamentes (forgalomra vonatkozó szabály, amely nem felel meg a bármelyik előző hello):
  1. Minden forgalmi szabály megtagadása: A Megtagadás szabály mindig hello végső szabály (tekintetében prioritás) kell lennie, és használja, így ha a forgalom áramlását sikertelen toomatch szabályok megelőző hello bármelyikét helyezés Ez a szabály. Ez a szabály az alapértelmezett szabály, és általában helyszíni és az aktív. Módosítás nélkül általában szükséges toothis szabály.

> [!TIP]
> Hello második alkalmazás forgalmi szabály, toosimplify ebben a példában bármely portra van engedélyezve. Egy valós forgatókönyv esetén hello legjobban megfelelő portok és címtartományok használt tooreduce hello támadási felületét Ez a szabály kell lennie.
>
>

Hello előző szabályok jönnek létre, ha fontos, minden egyes szabály tooensure forgalom tooreview hello rangsorának engedélyezett vagy megtagadott a kívánt módon működjenek. Ehhez a példához hello szabályok prioritási sorrendben szerepelnek.

#### <a name="conclusion"></a>Összegzés
Ebben a példában az összetettebb de védelmének és hello hálózati elkülönítése, mint az előző példákban hello módja befejeződik. (2. példa csak hello alkalmazás védi, és 1. példa csak korlátozása alhálózatok). Ez a kialakítás lehetővé teszi, hogy mindkét irányban forgalom figyelése, és nem csak hello bejövő alkalmazáskiszolgálót véd, de a hálózat kiszolgálóinak hálózati biztonsági házirend érvénybe lépteti. Is attól függően, hogy hello készülék használt, teljes forgalom naplózás és a tájékoztatási érhető el. További információkért lásd: hello [részletes utasításokat build][Example3]. Ezek az utasítások tartalmaznak:

* Hogyan toobuild a példa szegélyhálózati hálózati klasszikus PowerShell-parancsfájlokkal.
* Hogyan toobuild ebben a példában az Azure Resource Manager sablonnal.
* Részletes leírását minden UDR NSG parancsot, és a tűzfalszabály.
* Részletes forgalom folyamata forgatókönyvek, hogyan forgalom engedélyezett vagy megtagadott a két réteg megjelenítése.

### <a name="example-4-add-a-hybrid-connection-with-a-site-to-site-virtual-appliance-vpn"></a>4. példa egy hibrid kapcsolat hozzáadása az a pont-pont, a virtuális készülék VPN
[Biztonsági tooFast start](#fast-start) |} Részletes hamarosan elérhető build utasításokat

[![11]][11]

#### <a name="environment-description"></a>Környezet leírása
A hálózati virtuális készülék (NVA) használó hibrid hálózatkezelés tooany a peremhálózat típusai hello bemutatott példákban 1, 2 vagy 3 lehet hozzáadni.

Hello előző ábrán látható, a VPN-kapcsolaton keresztül hello Internet (pont-pont) használt tooconnect egy helyi hálózati tooan keresztül NVA Azure virtuális hálózat.

> [!NOTE]
> Ha ExpressRoute hello Azure nyilvános társviszony-létesítés engedélyezi a beállítást használja, létre kell hozni egy statikus útvonal. Statikus útvonal toohello NVA VPN IP-címet a vállalati internetes és nem ExpressRoute-kapcsolat hello keresztül kell továbbítani. hello hello ExpressRoute Azure nyilvános Társviszony beállítás szükséges NAT hello VPN-munkamenet meghibásodásához vezethet.
>
>

VPN van beállítva, hello hello NVA hello központi helyet alakíthatnak minden hálózatok és alhálózatok lesz. hello tűzfal továbbítási szabályaik határozza meg, mely forgalom adatfolyamok engedélyezettek, NAT keresztül van lefordítva, a rendszer, vagy dobja (akár még a hello a helyszíni hálózat és az Azure közötti forgalom).

Forgalom kell tekinteni, is lehet optimalizálni, vagy attól függően, hogy az adott hello csökken, mert ebben a kialakításban használati eset.

3. példa a beépített hello környezet használatával, és a pont-pont VPN hibrid hálózati kapcsolatot, hozza létre a következő tervezési hello:

[![12]][12]

hello helyszíni útválasztót, vagy bármely más hálózati eszköz, amely kompatibilis a VPN-, a NVA lenne hello VPN-ügyfél. A fizikai eszköz lenne kezdeményezése és hello VPN-kapcsolattal rendelkezik a NVA karbantartásáért felelős.

Logikailag toohello NVA, hello hálózati néz négy külön "biztonsági zónák" hello szabályokkal azon hello NVA hello elsődleges igazgató zónákhoz közötti forgalom:

![13]

#### <a name="conclusion"></a>Összegzés
pont-pont VPN hibrid hálózati kapcsolat tooan Azure-beli virtuális hálózat hozzáadása hello kiterjesztheti hello a helyszíni hálózat az Azure biztonságos módon. A VPN-kapcsolattal, a forgalom titkosítva van, és továbbítja a hello interneten keresztül. hello NVA ebben a példában egy központi helyen tooenforce biztosít, és hello biztonsági házirend kezelése. További információkért lásd: hello részletes utasításokat (szolgáltatnak) létrehozása. Ezek az utasítások tartalmaznak:

* Hogyan toobuild a példa szegélyhálózati hálózati PowerShell-parancsfájlokkal.
* Hogyan toobuild ebben a példában az Azure Resource Manager sablonnal.
* Részletes forgalom folyamata forgatókönyvek, jelenít meg, hogyan forgalom Ez a kialakítás keresztül zajlik.

### <a name="example-5-add-a-hybrid-connection-with-a-site-to-site-azure-vpn-gateway"></a>5. példa egy hibrid kapcsolat-webhelyek, Azure VPN-átjáró hozzáadása
[Biztonsági tooFast start](#fast-start) |} Részletes hamarosan elérhető build utasításokat

[![14]][14]

#### <a name="environment-description"></a>Környezet leírása
Az Azure VPN-átjáró használata hibrid hálózatkezelés peremhálózat típusa tooeither bemutatott példákban 1 vagy 2 lehet hozzáadni.

Ahogy az ábra megelőző hello, a VPN-kapcsolaton keresztül hello Internet (pont-pont) használt tooconnect egy helyi hálózati tooan Azure virtuális hálózat az Azure VPN-átjárón keresztül.

> [!NOTE]
> Ha ExpressRoute hello Azure nyilvános társviszony-létesítés engedélyezi a beállítást használja, létre kell hozni egy statikus útvonal. Statikus útvonal toohello NVA VPN IP-címet a vállalati internetes ki, és nem ExpressRoute WAN hello keresztül kell továbbítani. hello hello ExpressRoute Azure nyilvános Társviszony beállítás szükséges NAT hello VPN-munkamenet meghibásodásához vezethet.
>
>

hello alábbi ábrán látható hello két hálózati élt ebben a példában. Hello első oldal hello NVA és NSG-ket belüli Azure-hálózatok és az Azure közötti forgalom szabályozása, és internetes hello. hello második peremhálózati hello Azure VPN átjáró, amely egy külön és elkülönített hálózati edge-hez és az Azure között.

Forgalom kell tekinteni, is lehet optimalizálni, vagy attól függően, hogy az adott hello csökken, mert ebben a kialakításban használati eset.

1. példa a beépített hello környezet használatával, és a pont-pont VPN hibrid hálózati kapcsolatot, hozza létre a következő tervezési hello:

[![15]][15]

#### <a name="conclusion"></a>Összegzés
pont-pont VPN hibrid hálózati kapcsolat tooan Azure-beli virtuális hálózat hozzáadása hello kiterjesztheti hello a helyszíni hálózat az Azure biztonságos módon. Hello natív Azure VPN-átjárót használ, a forgalom IPSec-titkosítású és hello interneten keresztül irányítja. Emellett hello Azure VPN gateway használatával biztosít egy alacsonyabb költségű lehetőséget (nincs további licencelési költségeket, mint a külső NVAs). Ez a beállítás a leggazdaságosabb példában 1, ahol nincs NVA használt. További információkért lásd: hello részletes utasításokat (szolgáltatnak) létrehozása. Ezek az utasítások tartalmaznak:

* Hogyan toobuild a példa szegélyhálózati hálózati PowerShell-parancsfájlokkal.
* Hogyan toobuild ebben a példában az Azure Resource Manager sablonnal.
* Részletes forgalom folyamata forgatókönyvek, jelenít meg, hogyan forgalom Ez a kialakítás keresztül zajlik.

### <a name="example-6-add-a-hybrid-connection-with-expressroute"></a>6. példa az ExpressRoute hibrid kapcsolat hozzáadása
[Biztonsági tooFast start](#fast-start) |} Részletes hamarosan elérhető build utasításokat

[![16]][16]

#### <a name="environment-description"></a>Környezet leírása
Hibrid hálózatkezelés használatával egy ExpressRoute magánhálózati társviszony-létesítési kapcsolat lehet hozzáadni a peremhálózat típusa tooeither bemutatott példákban 1 vagy 2.

Ahogy az ábra megelőző hello, közvetlen kapcsolatra a helyszíni hálózat és a hello Azure-beli virtuális hálózat között ExpressRoute magánhálózati társviszony-létesítés biztosít. Csak hello szolgáltatás szolgáltató hálózati és soha nem érinti az hello Internet hello Microsoft Azure-hálózati forgalom továbbítását.

> [!TIP]
> Vállalati hálózati forgalom ExpressRoute segítségével tartja hello Internet ki. Lehetővé teszi a is garantált szolgáltatási szintek az ExpressRoute-szolgáltatótól. hello Azure átjáró teljen mentése too10 Gbps az ExpressRoute, mivel az a pont-pont VPN, hello Azure átjáró maximális átviteli sebesség 200 MB/s.
>
>

Hello a következő ábrán látható, ez a beállítás hello a környezet most már két hálózati szélén. hello NVA és NSG vezérlési forgalmat belüli Azure-hálózatok és az Azure és a hello hello átjáró pedig egy külön és elkülönített hálózati edge-hez és az Azure között, az Internet között.

Forgalom kell tekinteni, is lehet optimalizálni, vagy attól függően, hogy az adott hello csökken, mert ebben a kialakításban használati eset.

1. példa a beépített hello környezet használatával, és a ExpressRoute hibrid hálózati kapcsolat, hozza létre a következő tervezési hello:

[![17]][17]

#### <a name="conclusion"></a>Összegzés
az ExpressRoute privát társviszony-létesítés kapcsolatot hello hozzáadása kiterjesztheti hello a helyszíni hálózat az Azure a biztonságos, alacsonyabb késés, nagyobb végrehajtása módon. Is hello segítségével biztosítja a natív Azure-átjárón, például egy alacsonyabb költségű (nem kiegészítő licencelési, mint a külső NVAs) lehetőséget. További információkért lásd: hello részletes utasításokat (szolgáltatnak) létrehozása. Ezek az utasítások tartalmaznak:

* Hogyan toobuild a példa szegélyhálózati hálózati PowerShell-parancsfájlokkal.
* Hogyan toobuild ebben a példában az Azure Resource Manager sablonnal.
* Részletes forgalom folyamata forgatókönyvek, jelenít meg, hogyan forgalom Ez a kialakítás keresztül zajlik.

## <a name="references"></a>Referencia
### <a name="helpful-websites-and-documentation"></a>Hasznos webhelyek és dokumentáció
* Access Azure az Azure Resource Manager eszközzel:
* Azure PowerShell használata: [https://docs.microsoft.com/powershell/azureps-cmdlets-docs/](/powershell/azure/overview)
* Virtuális hálózati dokumentáció: [https://docs.microsoft.com/azure/virtual-network/](https://docs.microsoft.com/azure/virtual-network/)
* Hálózati biztonsági csoport dokumentáció: [https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg](virtual-network/virtual-networks-nsg.md)
* Felhasználó által definiált útválasztási dokumentáció: [https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview](virtual-network/virtual-networks-udr-overview.md)
* Az Azure virtuális átjárók: [https://docs.microsoft.com/azure/vpn-gateway/](https://docs.microsoft.com/azure/vpn-gateway/)
* Pont-pont VPN: [https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell](vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)
* Az ExpressRoute dokumentációja (lehet, hogy toocheck kimenő hello "Első lépések" és "Hogyan a következőnek" szakasz): [https://docs.microsoft.com/azure/expressroute/](https://docs.microsoft.com/azure/expressroute/)

<!--Image References-->
[0]: ./media/best-practices-network-security/flowchart.png "Biztonsági beállítások folyamatábrája"
[2]: ./media/best-practices-network-security/azuresecurityfeatures.png "azure biztonsági funkciói"
[3]: ./media/best-practices-network-security/dmzcorporate.png "A Szegélyhálózaton a vállalati hálózat"
[4]: ./media/best-practices-network-security/azuresecurityarchitecture.png "azure biztonsági architektúrája"
[5]: ./media/best-practices-network-security/dmzazure.png "DMZ egy Azure virtuális hálózatban"
[6]: ./media/best-practices-network-security/dmzhybrid.png "három biztonsági határokat hibrid hálózat"
[7]: ./media/best-practices-network-security/example1design.png "Az NSG bejövő DMZ"
[8]: ./media/best-practices-network-security/example2design.png "NVA és NSG bejövő DMZ"
[9]: ./media/best-practices-network-security/example3design.png "Kétirányú DMZ NVA, NSG és UDR"
[10]: ./media/best-practices-network-security/example3firewalllogical.png "hello tűzfalszabályok logikai ábrázolása"
[11]: ./media/best-practices-network-security/example3designoptions.png "Az NVA DMZ csatlakoztatott hibrid hálózati"
[12]: ./media/best-practices-network-security/example4designs2s.png "Az a pont-pont VPN-nel csatlakoztatott NVA DMZ"
[13]: ./media/best-practices-network-security/example4networklogical.png "NVA szempontjából logikai hálózat"
[14]: ./media/best-practices-network-security/example5designoptions.png "Az Azure-átjáróval DMZ csatlakoztatott pont-pont hibrid hálózati"
[15]: ./media/best-practices-network-security/example5designs2s.png "Az Azure-webhelyek közötti VPN használatával átjáróval DMZ"
[16]: ./media/best-practices-network-security/example6designoptions.png "Az Azure-átjáróval DMZ csatlakoztatott ExpressRoute hibrid hálózati"
[17]: ./media/best-practices-network-security/example6designexpressroute.png "Egy ExpressRoute-kapcsolat használatával az Azure-átjáróval DMZ"

<!--Link References-->
[TrustCenter]: https://azure.microsoft.com/support/trust-center/compliance/
[Example1]: ./virtual-network/virtual-networks-dmz-nsg.md
[Example2]: ./virtual-network/virtual-networks-dmz-nsg-fw-asm.md
[Example3]: ./virtual-network/virtual-networks-dmz-nsg-fw-udr-asm.md
[Example4]: ./virtual-network/virtual-networks-hybrid-s2s-nva-asm.md
[Example5]: ./virtual-network/virtual-networks-hybrid-s2s-agw-asm.md
[Example6]: ./virtual-network/virtual-networks-hybrid-expressroute-asm.md
[Example7]: ./virtual-network/virtual-networks-vnet2vnet-direct-asm.md
[Example8]: ./virtual-network/virtual-networks-vnet2vnet-transit-asm.md
