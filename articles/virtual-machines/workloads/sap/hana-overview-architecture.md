---
title: "aaaOverview és architektúra az SAP HANA, az Azure (nagy példányok) |} Microsoft Docs"
description: "Az architektúra áttekintése tooDeploy SAP HANA Azure (nagy példány)."
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
ms.openlocfilehash: e3ee6864af37ac322635eaef43e3c20101e3a769
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sap-hana-large-instances-overview-and-architecture-on-azure"></a>SAP HANA (nagy példányok) – áttekintés és az Azure-architektúra

## <a name="what-is-sap-hana-on-azure-large-instances"></a>Mi az Azure (nagy példányok) SAP HANA?

SAP HANA Azure (nagy példány) egy egyedi megoldás tooAzure. Továbbá tooproviding Azure virtuális gépek üzembe helyezéséhez és futtatásához az SAP HANA, Azure hello célját kínálja lehetőségét toorun hello és SAP HANA telepítése operációs rendszer nélküli kiszolgálókon, amelyek dedikált tooyou ügyfélként. SAP HANA hello Azure (nagy példányok) megoldás, amely hozzá van rendelve egy ügyfélként tooyou nem megosztott gazdakiszolgáló/operációs rendszer nélküli hardver építkezik. hello Kiszolgálóhardver nagyobb számítási és a kiszolgáló, a hálózat és tároló-infrastruktúra tartalmazó bélyegzők van beágyazva. Amellyel kombinációja lesz hitelesített HANA TDI. hello szolgáltatás ajánlat az SAP HANA Azure (nagy példányok) kínál különböző különböző kiszolgálótermékek vagy 72 processzorok rendelkező egységek és 768 GB memória toounits 960 processzorok és 20 TB memóriával rendelkező kezdve méretét.

-bérlőkkel, amelyek részletesen a következőképpen néz hello ügyfél elkülönítési hello infrastruktúra stamp belül történik:

- Hálózatkezelés: Elkülönítési ügyfelek belül infrastruktúra verem hozzárendelt ügyfél bérlőnként virtuális hálózatokon keresztül. A bérlő tooa egyetlen ügyfél hozzá van rendelve. Egy ügyfél rendelkezhet több bérlő. hello hálózati elkülönítés a bérlő nem engedélyezi a hálózati kommunikáció a bérlők a hello infrastruktúra stamp szint között. Akkor is, ha a bérlők tartozik toohello azonos felhasználói.
- Tárolási összetevőinek: tároló virtuális gépeken, amelyek tárolókötetek elkülönítését tooit rendelve. Tároló kötetek tooone tároló virtuális gép csak rendelhetők hozzá. A tároló virtuális gépek kizárólag tooone egybérlős hello SAP HANA TDI hitelesített infrastruktúra verem van hozzárendelve. Ennek eredményeképpen tooa tároló virtuális géphez hozzárendelt tárolókötetek érhetők el egy adott és kapcsolódó bérlői csak. És nem látható hello különböző telepített bérlők között.
- Kiszolgáló vagy a gazdagép: A kiszolgáló vagy a gazdagép egység nem megosztott ügyfelei vagy a bérlők között. A kiszolgáló vagy a gazdagép tooa ügyfél telepített, tooone egybérlős hozzárendelt atomi operációs rendszer nélküli számítási egység. **Nem** hardver particionálási vagy soft-particionálást használnak, amelyek eredményezheti, hogy ügyfélként, a gazdagépen vagy egy kiszolgáló megosztása egy másik ügyféllel. Tárolási rendelt toohello tároló virtuális gép hello adott bérlő fájlhelyekhez csatlakoztatott toosuch egy kiszolgálót. A bérlő rendelkezhet egy toomany server egységek különböző SKU kizárólag hozzárendelve.
- Egy SAP HANA (nagy példány) Azure infrastruktúra stamp, belül számos különböző bérlők telepített és egymással szembeni keresztül hello bérlői fogalmak hálózati, tárolási és számítási szinten elkülönített. 


A kiszolgáló operációs rendszer nélküli egységeket támogatott toorun SAP HANA csak. hello SAP alkalmazásréteg vagy munkaterhelés közel-vő réteg fut a Microsoft Azure virtuális gépeken. hello infrastruktúra bélyegzők hello SAP HANA futó Azure (nagy példány) egységek olyan csatlakoztatott toohello Azure hálózati gerinchálózatok esetében alkalmazzák, tehát, amely a kis késleltetésű kapcsolat az Azure (nagy példány) egységek SAP HANA és Azure virtuális gépek közötti valósul meg.

Ez a dokumentum az egyik öt dokumentumok, amely magában foglalja a hello a témakör az SAP HANA Azure (nagy példány). Ez a dokumentum azt lépjen hello alapvető architektúráját, feladatok, szolgáltatásait, és a magas szintű keresztül hello megoldás képességeit. A legtöbb hello területek, mint például a hálózati és a csatlakozási, hello más négy dokumentumok részletek takarja el és időszakosan megszakadó részletezést. hello dokumentáció az Azure (nagy példány) SAP HANA nem fedi fel az SAP NetWeaver telepítési szempontok vagy SAP NetWeaver az Azure virtuális gépeken a központi telepítéséhez. Ez a témakör tárgyalja hello található azonos külön dokumentáció dokumentáció tároló. 


Ez az útmutató hello öt része a következő témakörök hello terjed ki:

- [SAP HANA (nagy példány) – áttekintés és az Azure-architektúra](hana-overview-architecture.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [SAP HANA (nagy példányok) infrastruktúra és az Azure-kapcsolat](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Hogyan tooinstall és SAP HANA (nagy példányok) konfigurálása az Azure-on](hana-installation.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [SAP HANA (nagy példányok) magas rendelkezésre állás és vészhelyreállítás Azure](hana-overview-high-availability-disaster-recovery.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [SAP HANA (nagy példányok) hibaelhárítás és Azure figyelése](troubleshooting-monitoring.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="definitions"></a>Meghatározások

Több, közös definíciók széles körben használt hello architektúra és a műszaki üzembe helyezési útmutatójában. Megjegyzés: hello feltételeket és azok jelentését a következő:

- **IaaS:** szolgáltatott infrastruktúra
- **A PaaS:** platformok
- **SaaS:** szolgáltatott szoftver
- **SAP összetevő:** az egyes SAP-alkalmazások, például az ECC, BW, megoldás kezelő vagy EP SAP-összetevők a hagyományos ABAP vagy Java technológiák vagy egy nem NetWeaver alapú alkalmazás, például üzleti objektumok is alapulhat.
- **SAP-környezetben:** logikailag egy csoportba tooperform egy üzleti funkciót, fejlesztési, a QAS, a képzési, a vész-Helyreállítási vagy a termelési például SAP-összetevők közül.
- **SAP fekvő:** toohello egész SAP eszközök az informatikai fekvő hivatkozik. hello SAP fekvő minden üzemi és nem éles környezetben tartalmaz.
- **SAP-verzió:** hello DBMS réteg és a alkalmazásréteg egy SAP ERP fejlesztőrendszer SAP BW gazdagépes tesztrendszer, SAP CRM éles rendszer, stb. Azure-környezetekhez nem támogatják ezeket a helyszíni és az Azure közötti két réteget elválasztó. Azt jelenti, hogy egy SAP rendszerben telepített helyi, illetve az Azure-ban telepítve van. Telepíthet azonban hello különböző egy SAP fekvő Azure vagy a helyszíni rendszeren. Például elvégezheti hello SAP CRM fejlesztési és tesztelési Azure, a rendszer hello SAP CRM éles rendszer helyszíni telepítése közben. SAP Hana (nagy példányok) Azure-on célja, hogy hello SAP alkalmazásréteg SAP rendszerek az Azure virtuális gépeken üzemeltet, és hello hello HANA nagy példány bélyegző egység kapcsolódó SAP HANA-példányához.
- **Nagy példány stamp:** hardver infrastruktúra verem egy SAP HANA TDI van hitelesített, és a dedikált toorun SAP HANA-példányok Azure-ban.
- **SAP HANA Azure (nagy példány):** hello ajánlat Azure toorun HANA-példány az SAP HANA TDI hardver, amely telepítve van a különböző Azure-régiók nagy példány bélyegzők hitelesített hivatalos nevét. hello kapcsolódó kifejezés **HANA nagy példány** rövid a SAP HANA Azure (nagy példányokat), és széles körben használt műszaki telepítési útmutatóban.
- **Létesítmények közötti:** hol a virtuális gépek az Azure-előfizetéssel, amely rendelkezik pont-pont, többhelyes vagy hello helyszíni datacenter(s) és az Azure között ExpressRoute kapcsolat telepített tooan egy olyan forgatókönyvet ismertet. Közös Azure dokumentációja, az ilyen típusú központi telepítések egyaránt létesítmények közötti forgatókönyv leírása. hello hello kapcsolat oka, hogy a helyi DNS az Azure, a tooextend helyi tartományoknak és a helyszíni Active Directory/OpenLDAP. hello helyszíni fekvő kiterjesztett toohello hello Azure előfizetés(ek) Azure eszközeinek van. Ha ezt a bővítményt, hello virtuális gépek hello a helyi tartomány része lehet. Tartományi felhasználók hello helyszíni tartomány hello kiszolgálókat érhetnek el, és szolgáltatásokat futtathatja a virtuális gépek (például adatbázis-kezelő szolgáltatás). Telepített virtuális gépek a helyszíni és az Azure telepített virtuális gépek közötti kommunikációt és a névfeloldás lehetőség. Ilyen hello jellemző forgatókönyv a legtöbb SAP mely eszközök vannak telepítve. Tekintse meg a hello útmutatók [tervezése és kialakítása VPN-átjáró](../../../vpn-gateway/vpn-gateway-plan-design.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) és [hozhat létre egy Vnetet hello Azure-portál használatával webhelyek kapcsolattal rendelkező](../../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) részletes információkat.
- **Bérlői:** HANA nagy példányok stamp telepített ügyfél lekérdezi elkülönített be a "bérlő." A bérlő elkülönül a hello hálózati, tárolási és számítási rétegét a többi bérlőtől. Igen a tárolási és számítási egység hozzárendelt toohello különböző bérlők nem tekintse meg egymással, vagy a hello kommunikálnak egymással HANA nagy példány időbélyegző szintjét. Az ügyfél kiválaszthatja a különböző bérlők toohave telepítések. Nincs még ilyen esetben is hello HANA nagy stamp példányszintű a bérlők közötti kommunikáció.

Nincsenek további források hello témakör központi telepítése a Microsoft Azure nyilvános felhőjében SAP terhelése közzétett különböző. Erősen ajánlott, hogy bárki tervezési és egy SAP HANA központi telepítés végrehajtása az Azure-ban tapasztalt és tudomást hello rendszerbiztonsági tagoknak az Azure infrastruktúra-szolgáltatási és hello Azure IaaS SAP munkaterhelését központi telepítését. hello következő erőforrások nyújtanak további információt és a folytatás előtt meg lehet hivatkozni:


- [SAP megoldások segítségével a Microsoft Azure virtuális gépeken](get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="certification"></a>Minősítés

Módosításokon kívül hello NetWeaver hitelesítő, SAP egy különös hitelesítő SAP HANA toosupport SAP HANA egyes infrastrukturális, például az Azure infrastruktúra-szolgáltatási igényel.

hello Core SAP Megjegyzés NetWeaver és tooa mértékben SAP HANA hitelesítő [SAP Megjegyzés #1928533 – Azure SAP-alkalmazásokból: támogatott termékek és az Azure virtuális gép típusok](https://launchpad.support.sap.com/#/notes/1928533).

Ez [SAP Megjegyzés #2316233 - SAP HANA a Microsoft Azure (nagy példányok)](https://launchpad.support.sap.com/#/notes/2316233/E) is fontos. A jelen útmutatóban ismertetett hello megoldás magában foglalja. Emellett támogatott toorun SAP HANA hello GS5 VM típusú Azure áll. [Ebben az esetben információi hello SAP webhelyen van közzétéve](http://global.sap.com/community/ebook/2014-09-02-hana-hardware/enEN/iaas.html).

SAP ügyfelek hello képességét toodeploy és hello Azure (nagy példányok) megoldás SAP HANA említett SAP Megjegyzés #2316233 biztosít a Microsoft tooin nagy SAP Business Suite, SAP Business Warehouse (BW), S/4 HANA, BW/4HANA vagy más SAP HANA-munkaterhelések az Azure-ban. hello megoldás hello SAP HANA dedikált hardver stamp hitelesített alapul ([SAP HANA igazított Datacenter integrációs – TDI](https://scn.sap.com/docs/DOC-63140)). Egy SAP HANA TDI futtató beállított megoldás lehetővé teszi hello abban, hogy annak ismerete, hogy minden SAP HANA-alapú alkalmazások (beleértve az SAP Business Suite az SAP HANA, SAP Business Warehouse (BW) SAP HANA, S4/HANA és BW4/HANA) hello toowork folyamatban van hardver infrastruktúra.

Az összehasonlított toorunning SAP HANA az Azure virtuális gépeken Ez a megoldás előnye van – sokkal nagyobb memória kötetek biztosít. Ha azt szeretné, tooenable ebben a megoldásban, van néhány kulcsfontosságú elemeit toounderstand:

- hello SAP alkalmazásréteg és nem SAP alkalmazások futtatása az Azure virtuális gépek (VM) hello szokásos Azure hardverkonfiguráción bélyegek tárolt.
- Ügyfél a helyszíni infrastruktúrát, adatközpontok, és alkalmazások központi telepítésének csatlakoztatott toohello Microsoft Azure cloud platform keresztül Azure ExpressRoute (ajánlott) vagy virtuális magánhálózati (VPN). Active Directory (AD) és a DNS is kiterjeszthetőek az Azure.
- hello SAP HANA-adatbázispéldány HANA munkaterhelés SAP HANA (nagy példányok) Azure-on futtatja. hello nagy példány stamp be Azure networking csatlakozik, ezért az Azure virtuális gépeken futó szoftver hello HANA-példány futtatási HANA nagy példányát használhatják.
- Hardver-vagy Azure (nagy példányok) SAP HANA megadott infrastruktúra (IaaS) szolgáltatás a SUSE Linux Enterprise Server dedikált hardver- vagy a Red Hat Enterprise Linux, előre telepítve. Mivel az Azure virtuális gépek, a frissítések és karbantartás toohello operációs rendszer további a feladata.
- HANA és minden további összetevők szükséges toorun SAP HANA, egységeken HANA nagy példányok telepítésének feladata a, az összes megfelelő folyamatban lévő műveletek és SAP HANA Azure számára.
- Emellett itt leírt toohello megoldások, telepíthető más összetevők az Azure-előfizetéssel, amely a tooSAP HANA Azure (nagy példány).  Például olyan összetevők, amelyek lehetővé teszik a kommunikációt, illetve közvetlenül toohello SAP HANA-adatbázisból (jump-kiszolgálók, RDP-kiszolgálók, SAP HANA Studio, a SAP Data Services SAP BI forgatókönyvek esetén, vagy hálózati figyelési megoldások).
- Azure, mint HANA nagy példányok támogató magas rendelkezésre állású és vész-helyreállítási funkciókat kínál.

## <a name="architecture"></a>Architektúra

Egy magas szintű: hello Azure (nagy példányok) megoldás SAP HANA rendelkezik hello SAP alkalmazásréteg szereplő Azure virtuális gépek és hello Adatbázisréteg levő egy nagy példány stamp található SAP TDI konfigurált hardver csatlakoztatva van egy Azure-régióban hello infrastruktúra-szolgáltatási tooAzure.

> [!NOTE]
> Toodeploy hello SAP alkalmazásréteg a hello van szüksége egy Azure-régióban van hello SAP DBMS réteg. Ez a szabály szerepel a dokumentált Azure SAP munkaterhelés közzétett adatait. 

hello átfogó architektúrája SAP HANA Azure (nagy példányok) biztosít egy SAP TDI jóváhagyott hardver configuration (a nem virtualizált, operációs rendszer nélküli, hello SAP HANA-adatbázishoz tartozó nagyteljesítményű kiszolgáló), és hello képességét és rugalmasságát Azure tooscale hello erőforrásait SAP alkalmazás réteg toomeet igényeinek.

![Az SAP HANA (nagy példányok) Azure-architektúra áttekintése](./media/hana-overview-architecture/image1-architecture.png)

hello architektúra látható három részből áll:

- **Jobb:** az adatközpontok különböző alkalmazások futtatása a végfelhasználók számára (például SAP) LOB-alkalmazásokhoz való hozzáférés a helyszíni infrastruktúrát. Ideális esetben ez a helyszíni infrastruktúra majd csatlakozik az Azure-ral tooAzure [ExpressRoute](https://azure.microsoft.com/services/expressroute/).

- **Központ:** látható Azure IaaS és az Azure virtuális gépek toohost SAP vagy más SAP HANA adatbázis-kezelő rendszert használó alkalmazások ebben az esetben használja. Adja meg a függvény a hello memória Azure virtuális gépek kisebb HANA-példányok az alkalmazási rétegre együtt az Azure virtuális gépeken vannak telepítve. Bővebben [virtuális gépek](https://azure.microsoft.com/services/virtual-machines/).
<br />Az Azure hálózatkezelés használt toogroup SAP rendszerek együtt más alkalmazások az Azure virtuális hálózatokról (Vnetekről). A Vnetek tooon helyszíni rendszerek, valamint az Azure (nagy példányok) HANA tooSAP csatlakozzon.
<br />SAP NetWeaver alkalmazások és -adatbázisok, amelyek támogatott toorun a Microsoft Azure-ban, a következő témakörben: [SAP támogatási Megjegyzés #1928533 – Azure SAP-alkalmazásokból: támogatott termékek és az Azure virtuális gép típusok](https://launchpad.support.sap.com/#/notes/1928533). Üzembe helyezni az SAP-megoldások Azure dokumentációját lásd:

  -  [A Windows virtuális gépek (VM) SAP használatával](../../virtual-machines-windows-sap-get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
  -  [SAP megoldások segítségével a Microsoft Azure virtuális gépeken](get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

- **Balra:** mutat be hello SAP HANA TDI hitelesített hello Azure nagy példány bélyegző hardver. hello HANA nagy példány egységgel csatlakoztatott toohello Azure Vnetekhez hello ugyanazt a technológiát, hello kapcsolat a helyszíni az Azure használatával előfizetése.

hello Azure nagy példány stamp maga egyesíti a következő összetevők hello:

- **Számítások:** Intel Xeon E7-8890v3 vagy Intel Xeon E7-8890v4 processzorral hello szükséges számítási képességeket biztosítanak, és SAP HANA hitelesített alapuló kiszolgálók.
- **Hálózati:** A nagy sebességű hálózati háló kapcsolja össze a hello számítási, tárolási és hálózati összetevők egységes.
- **Tárolás:** olyan tárolási infrastruktúra, amely egy egységes hálózati háló keresztül érhető el. Bizonyos tárolási kapacitás kerül a attól függően, hogy hello adott SAP HANA Azure (nagy példányok) konfigurációban telepített (további tárolási kapacitás érhető el további havi költségén).

Az ügyfelek infrastruktúrájában hello több-bérlős hello nagy példány stamp, elkülönített bérlők üzemelnek. Hello bérlő központi telepítését, a tooname Azure-előfizetés az Azure regisztrációs belül kell. A folyamatos toobe hello Azure-előfizetéssel, hello HANA nagy példány (oka) alapján számlázzuk toobe lesz. Ezek a bérlők egy 1:1 számosságú kapcsolatok toohello Azure-előfizetéssel rendelkezik. Hálózati bölcs lehetséges tooaccess HANA nagy példány egység telepítve egy Azure-régióban a különböző Azure Vnetekhez, amely toodifferent tartozik egy bérlő Azure-előfizetések. Bár ezek Azure-előfizetések kell toobelong toohello azonos Azure regisztrációs. 

Azure virtuális gépeken, a több Azure-régiók a SAP HANA Azure (nagy példányok) kínálják. Rendelés toooffer vész-helyreállítási képességek a tooopt választhatja ki. Egy földrajzi politikai régión belül különböző nagy példány bélyegzők más csatlakoztatott tooeach. Például a HANA nagy példány bélyegzők Velünk nyugati és amerikai keleti hello céllal vész-Helyreállítási replikációs, a dedikált hálózati kapcsolaton keresztül csatlakoznak. 

Ugyanúgy, mint a virtuális gép különböző Azure virtuális gépek közül választhat, SKU, HANA nagy példány, amely különböző alkalmazások és szolgáltatások típusú SAP HANA is lefednek közül választhat. SAP vonatkozik memória tooprocessor szoftvercsatorna arányok hello Intel processzorral generációja alapján különböző munkaterhelések – kínált négy különböző SKU típusa van:

Től július 2017 SAP HANA Azure (nagy példányok) érhető el több konfiguráció hello Azure régiók a US nyugati amerikai keleti, Kelet-Ausztrália, Ausztrália délkeleti, Nyugat-Európa és Észak-Európa:

| SAP-megoldás | CPU | Memory (Memória) | Storage | Rendelkezésre állás |
| --- | --- | --- | --- | --- |
| Az OLAP-re optimalizált: FF/4HANA, az SAP BW Programhoz<br /> vagy SAP HANA általános OLAP-munkaterhelés | Az Azure S72 SAP HANA<br /> – 2 x Intel® Xeon® processzor E7-8890 v3<br /> A Processzormagok 36 és 72 CPU szálak |  768 GB |  3 TB | Elérhető |
| --- | Az Azure S144 SAP HANA<br /> – 4 x Intel® Xeon® processzor E7-8890 v3<br /> A Processzormagok 72 és 144 CPU szálak |  1,5 TB |  6 TB | Nem érhető el többé |
| --- | Az Azure S192 SAP HANA<br /> – 4 x Intel® Xeon® processzor E7-8890 v4<br /> A Processzormagok 96 és 192 CPU szálak |  2.0 TB |  8 TB | Elérhető |
| --- | Az Azure S384 SAP HANA<br /> – 8 x Intel® Xeon® processzor E7-8890 v4<br /> A Processzormagok 192 és 384 CPU szálak |  4.0 TB |  16 TB | Készen áll a tooOrder |
| Az OLTP optimalizált: SAP Business csomag<br /> SAP HANA vagy az S/4HANA (OLTP)<br /> általános OLTP | Az Azure S72m SAP HANA<br /> – 2 x Intel® Xeon® processzor E7-8890 v3<br /> A Processzormagok 36 és 72 CPU szálak |  1,5 TB |  6 TB | Elérhető |
|---| Az Azure S144m SAP HANA<br /> – 4 x Intel® Xeon® processzor E7-8890 v3<br /> A Processzormagok 72 és 144 CPU szálak |  3.0 TB |  12 TB | Nem érhető el többé |
|---| Az Azure S192m SAP HANA<br /> – 4 x Intel® Xeon® processzor E7-8890 v4<br /> A Processzormagok 96 és 192 CPU szálak  |  4.0 TB |  16 TB | Elérhető |
|---| Az Azure S384m SAP HANA<br /> – 8 x Intel® Xeon® processzor E7-8890 v4<br /> A Processzormagok 192 és 384 CPU szálak |  6.0 TB |  18 TB | Készen áll a tooOrder |
|---| Az Azure S384xm SAP HANA<br /> – 8 x Intel® Xeon® processzor E7-8890 v4<br /> A Processzormagok 192 és 384 CPU szálak |  8.0 TB |  22 TB |  Készen áll a tooOrder |
|---| Az Azure S576 SAP HANA<br /> – 12 x Intel® Xeon® processzor E7-8890 v4<br /> A Processzormagok 288 és 576 CPU szálak |  12.0 TB |  28 TB | Készen áll a tooOrder |
|---| Az Azure S768 SAP HANA<br /> – 16 x Intel® Xeon® processzor E7-8890 v4<br /> A Processzormagok 384 és 768 CPU szálak |  16,0 TB |  36 TB | Készen áll a tooOrder |
|---| Az Azure S960 SAP HANA<br /> – 20 x Intel® Xeon® processzor E7-8890 v4<br /> 480 Processzormagok és 960 CPU szálak |  20.0 TB |  46 TB | Készen áll a tooOrder |

- A Processzormagok Processzormagok nem-többszálú hello Sum hello server egység hello processzorok számának összege, azaz =.
- CPU szálak CPU-magokat többszálú hello processzorok hello server egység hello összegére által biztosított számítási szálak számának összege, azaz =. A Hyper-Threading alapértelmezett toouse által konfigurált összes egység.


hello különböző konfigurációkat fenti állnak rendelkezésre, vagy "SE ajánlja többé" hivatkozott [SAP támogatási Megjegyzés #2316233 – a Microsoft Azure (nagy példányok) SAP HANA](https://launchpad.support.sap.com/#/notes/2316233/E). hello konfigurációkra, amelyek "Kész tooOrder" jelölésű hamarosan található SAP Megjegyzés hello beléptetése. Azonban ezen példány termékváltozatok által rendelhető már hello hat különböző Azure-régiók hello HANA nagy példány szolgáltatás érhető el.

a kiválasztott hello konfigurációkkal munkaterhelés, a Processzor-erőforrások és a kívánt memória függenek. Az OLTP munkaterhelés tooleverage hello OLAP munkaterhelések optimalizált termékváltozatok lehetőség. 

a minden hello ajánlatok alapszintű hello hardver SAP HANA TDI hitelesített. Azonban különböztetünk hardver, hello termékváltozatok az osztó két különböző osztály között:

- S72, S72m, S144, S144m, S192 és S192m, amelyek azt tooas hello "Típusú i. osztály" SKU.
- S384, S384m, S384xm, S576, S768 és S960, amely az tooas hello "Típusú class" SKU.

Fontos, hogy egy teljes HANA nagy példány stamp nem kizárólag számára kíván lefoglalni az egyetlen ügyfél &#39; toonote s használja. Ez a tény számítási és tárolási erőforrásokat egy Azure szolgáltatásba telepített, valamint hálózati háló keresztül csatlakozó toohello rackszekrények vonatkozik. HANA nagy példányok infrastruktúra, például Azure, telepíti a különböző ügyfél &quot;bérlők&quot; , amelyek elkülönülnek egymástól a következő három szintje hello:

- Hálózati: Virtuális hálózatok hello HANA nagy példány stamp elkülönítését.
- Tárolás: A tároló virtuális gépeken, amelyek tárolókötetek elkülönítését hozzárendelt, és tárolókötetek bérlők között elkülönítése.
- Számítási: A kiszolgáló egységek tooa egybérlős dedikált rendelését. Nem rögzített vagy soft particionálási kiszolgáló egységek. Egyetlen kiszolgáló vagy a gazdagép egység bérlők között nincs megosztás. 

Ilyen hello telepítések HANA nagy példányok egységek különböző bérlők között nincsenek más látható tooeach. Sem HANA a különböző bérlők rendszerbe nagy példány egységek közvetlenül kommunikálhatnak egymással hello HANA nagy példány stamp szinten. Csak HANA nagy példány egység egy bérlő belül a hello HANA nagy stamp példányszintű tooeach tudjanak kommunikálni.
Egy telepített tenant hello nagy példány stamp számlázási bölcs tooone Azure-előfizetés hozzá van rendelve. Azonban a tartalomtárban belül más Azure-előfizetések az Azure Vnetekhez hálózati wise azonos Azure regisztrációs hello. Ha telepít egy másik Azure-előfizetés a hello azonos Azure-régió, is választhat tooask elkülönített HANA nagy példány bérlőhöz.

SAP HANA futó HANA nagy példányok és az Azure szolgáltatásba telepített Azure virtuális gépeken futó SAP HANA között jelentős különbség van:

- Nincs nincs virtualizálási réteg SAP Hana Azure (nagy példány). Hello alapul szolgáló operációs rendszer nélküli hardver teljesítményét hello kap.
- Azure-ban eltérően SAP HANA hello Azure (nagy példányok) kiszolgálón dedikált tooa adott ügyfélhez. Nincs lehetőségét, hogy a kiszolgáló egység vagy a gazdagép nehéz vagy soft-particionálva van. Ennek eredményeképpen a HANA nagy példány egység rendeli, a teljes tooa bérlők és az adott tooyou ügyfélként szolgál. Újraindítás vagy leállítás hello kiszolgáló nem vezet automatikusan toohello operációs rendszer és az SAP HANA üzembe helyezéséhez egy másik kiszolgálón. (Típus i. osztály SKU, hello egyetlen kivétel, ha egy kiszolgáló problémák léphetnek fel, és újratelepítés toobe egy másik kiszolgálón történik.)
- Eltérően Azure, ahol processzor állomástípusok hello legjobb price/teljesítményesemények aránya az van kiválasztva, az Azure (nagy példányok) SAP HANA választott hello processzor típusok vannak hello hello Intel E7v3 és E7v4 processzor sorban legmagasabb végrehajtása.


### <a name="running-multiple-sap-hana-instances-on-one-hana-large-instance-unit"></a>Több SAP HANA-példány fut egy HANA nagy példány egység
Több lehetséges toohost, mint hello HANA nagy példány egység egy aktív SAP HANA példány. Ahhoz, toostill Storage-pillanatfelvételekkel hello képességeit adja meg, és a vészhelyreállítás, ilyen konfiguráció esetén példányonként kötet. Mostantól hello HANA nagy példány egység az alábbiak szerint lehet osztani:

- S72, S72m, S144, S192: A legkisebb 256 GB hello 256 GB-os lépésekben egység indítása. Különböző nagyobb, mint 256 GB, 512 GB és így tovább kombinált toohello legfeljebb hello memória hello egység is lehet.
- S144m és S192m: 512 GB hello legkisebb egységgel 256 GB-os lépésekben. Különböző nagyobb, mint 512 GB, 768 GB és így tovább kombinált toohello legfeljebb hello memória hello egység is lehet.
- Írja be a II osztály: a legkisebb egység 2 TB-os indítása hello 512 GB-os lépésekben. 512 GB, 1 TB-os, 1,5 TB és így tovább, például különböző nagyobb kombinált toohello legfeljebb hello memória hello egység is lehet.

Néhány példa a több SAP HANA-példányt futtató nézhet:

| SKU | Memória mérete | Storage mérete | Több adatbázisból méretek |
| --- | --- | --- | --- |
| S72 | 768 GB | 3 TB | 1 x 768 GB HANA példány<br /> vagy példányt. 1 x 512 GB + 1 x 256 GB példány<br /> vagy 3 x 256 GB példányok | 
| S72m | 768 GB | 3 TB | 3x512GB HANA példányok<br />vagy példányt. 1 x 512 GB + 1 darab 1 TB-példány<br />vagy 6 x 256 GB példányok<br />vagy 1x1.5 TB-példány | 
| S192m | 4 TB | 16 TB | 8 x 512 GB példányok<br />vagy 4 x 1 TB-példányok<br />vagy 4 x 512 GB példányok + 2 darab 1 TB-példányok<br />vagy 4 x 768 GB példányok + 2 darab 512 GB példányok<br />vagy 1 darab 4 TB-példány |
| S384xm | 8 TB | 22 TB | 4 x 2 TB-példányok<br />vagy 2 darab 4 TB-példányok<br />vagy példányok 2 x 3 TB + 1 x 2 TB-példányok<br />vagy példányok 2x2.5 TB + 1 x 3 TB-példányok<br />vagy 1 x 8 TB-példány |


Hello képet kaphat. Biztosan más változatok vannak is. 


## <a name="operations-model-and-responsibilities"></a>Operatív modell és felelősségek

az Azure (nagy példányok) SAP HANA hello szolgáltatás Azure IaaS szolgáltatások igazodik. Egy SAP HANA optimalizált telepített operációs rendszer HANA nagy példányok példány elérhetővé. Az Azure IaaS virtuális gépeket, az operációs rendszer, akkor telepítésére van szükség, HANA, további szoftverek telepítése hello megerősítésének hello feladatok többsége működő hello operációs rendszer és HANA, és az operációs rendszer és HANA frissítése hello a feladata. A Microsoft nem kényszeríti az operációs rendszer vagy HANA-frissítéseket, a.

![SAP HANA Azure (nagy példányok) feladatai](./media/hana-overview-architecture/image2-responsibilities.png)

Ahogy látja, a fenti diagram hello, SAP HANA Azure (nagy példányok) egy több-bérlős infrastruktúra, mint a szolgáltatás ajánlat. És emiatt hello osztás felelősségi hello operációsrendszer-infrastruktúra határa, hello, legtöbb része. Microsoft hello szolgáltatást hello vonal hello operációs rendszer alatt minden szempontját felelős, és az Ön felelőssége hello vonal, beleértve a hello operációs rendszer felett. Ezért a legfrissebb helyszíni módszerek is alkalmazó megfelelőségi, biztonsági, alkalmazáskezelés, alapján, és az operációs rendszer felügyeleti továbbra is használt toobe. hello rendszerek jelennek meg, ha a hálózat összes tanúsítványinformációit vannak.

Azonban ez a szolgáltatás megfelelően lett optimalizálva SAP Hana, ahol Ön és a Microsoft kell toowork együtt toouse hello alapul szolgáló infrastruktúra lehetőségeit a legjobb eredmények elérése érdekében területet.

a következő lista hello a részletgazdagabb hello rétegek és az Ön feladatkörei:

**Hálózatkezelés:** összes hello belső hálózatok hello nagy példány stamp futtató SAP HANA, hozzáférési toohello tárolóval, hello példányok (a kibővített és egyéb funkciók), a kapcsolat toohello fekvő és a kapcsolat közötti kapcsolat ahol hello SAP alkalmazásréteg Azure virtuális gépek tárolása tooAzure. A vész-helyreállítási célokra replikáció Azure-adatközpontokban közötti WAN-kapcsolatra is tartalmaz. Minden hálózat hello bérlők által particionáltak, és QOS alkalmazott.

**Tárolás:** hello virtualizált particionált tárolót a hello SAP HANA-kiszolgálók által igényelt összes kötet, illetve a pillanatképeket. 

**Kiszolgálók:** hello dedikált fizikai kiszolgálók toorun hello SAP HANA-adatbázisok tootenants rendelve. hello kiszolgálók hello i. osztály típus a rendszer azért hardver SKU. Az ilyen kiszolgálók hello kiszolgálókonfiguráció gyűjti és profilok, amelyek egy fizikai hardver tooanother fizikai hardver eltolható megőrződik. Ilyen (manuális) áthelyezésre profil műveletek kicsit hasonlítható tooAzure szolgáltatás javítás. a hello hello kiszolgálók típusú osztály SKU nem áll rendelkezésre ilyen funkció.

**SDDC:** szoftveralapú entitásként adatközpontok hello felügyeleti szoftverek használt toomanage adatokat. Ez lehetővé teszi, hogy a Microsoft toopool erőforrások a méretezés, a rendelkezésre állás és a teljesítmény szempontjából.

**Rendszer:** hello (SUSE Linux vagy a Red Hat Linux) operációs rendszer futtató hello kiszolgálókon. hello operációsrendszer-lemezképek rendelkezésre állnak a SAP HANA futó hello célra hello egyes Linux szállító tooMicrosoft által biztosított hello képek. Hello Linux szállító hello SAP HANA-re optimalizált lemezkép rendelkező előfizetés szükséges toohave áll. Az Ön feladatkörei tartalmaznak hello képek regisztrálása hello az operációs rendszer szállítójához. A Microsoft által átadási hello pontról is való telepítésért felelős semmilyen további javítását hello Linux operációs rendszer. A javítás is tartalmaz, amely egy SAP HANA sikeres telepítéséhez szükséges lehet további csomagokat (lásd a tooSAP tartozó HANA dokumentáció és SAP megjegyzések) és amelyek nem szereplő adott Linux szállító hello az SAP HANA optimalizált operációsrendszer-lemezképek. hello ügyfél hello felelőssége is, a kapcsolódó toomalfunction/optimalizálási hello az operációs rendszer és az illesztőprogramok kapcsolódó toohello adott Kiszolgálóhardver hello OS javítását. Vagy semmilyen biztonsági vagy hello az operációs rendszer működési javítását. Az ügyfél felelőssége, valamint a figyelési- és a kapacitás-tervezési:

- Processzor-erőforrás-felhasználás
- Memória-felhasználás
- Kötetek kapcsolódó toofree lemezterület, az iops-érték és a késleltetés
- Kötet forgalmat HANA nagy példány és az SAP alkalmazásréteg között

hello alapul szolgáló infrastruktúra HANA nagy példányok a biztonsági mentési és visszaállítási hello OS kötet funkciókat biztosít. Ez a funkció használata is a felelős.

**Köztes:** SAP HANA-példány, elsősorban hello. Felügyeleti műveletek és figyelési az Ön felelőssége. Nincs funkció, amely lehetővé teszi a biztonsági mentés/visszaállítás és vész-helyreállítási célokra toouse storage-pillanatfelvételekkel megadott. Ezek a képességek hello infrastruktúra által biztosított. Azonban az Ön feladatkörei is magas rendelkezésre állás és vészhelyreállítás tervezése ezekkel a lehetőségekkel, használhatja őket és megfigyelést a storage-pillanatfelvételekkel végrehajtása sikerült.

**Adatok:** az SAP HANA által kezelt adatok, és más adatok, például biztonsági másolatok köteteken található fájlok vagy a fájl megosztja. Az Ön feladatkörei közé tartozik a szabad logikailemez-terület figyelési hello köteteken hello tartalmat, és felügyelete hello kötetek biztonsági mentése lemezre és storage-pillanatfelvételekkel sikeres végrehajtását. Adatok replikációs tooDR helyek sikeres végrehajtását azonban Microsoft hello felelőssége.

**Alkalmazások:** SAP alkalmazáspéldányok hello, vagy nem SAP-alkalmazásokból, ezeknek az alkalmazásoknak hello alkalmazásréteg esetén. Az Ön feladatkörei például központi telepítés, a felügyeleti, a műveletek, és ezek az alkalmazások figyelését kapcsolatos toocapacity CPU erőforrás-felhasználás, memória-felhasználás, Azure-tároló fogyasztása és hálózati sávszélesség-használat belül tervezése Az Azure Vnetekhez, és az Azure Vnetekhez tooSAP HANA Azure (nagy példány).

**WAN:** hello kapcsolatok létrehozása a helyszíni tooAzure tapasztaltaktól munkaterhelésekhez. Ügyfeleink HANA nagy osztályt Azure ExpressRoute kapcsolatot használjon. Ez a kapcsolat nem nem SAP HANA hello Azure (nagy példányok) megoldás részét úgy, hogy Ön felelős hello beállítása ehhez a kapcsolathoz.

**Archív:** inkább tooarchive saját módszerekkel tárfiókokban lévő adatok másolatát. Archiválás szükséges felügyeleti, a megfelelőségi, a költségek és a műveletek. Ön felelősséggel tartozik archív másolatok és az Azure biztonsági mentések létrehozásához, és tárolja őket a megfelelő módon.

Lásd: hello [SAP Hana (nagy példányok) Azure SLA](https://azure.microsoft.com/support/legal/sla/sap-hana-large/v1_0/).

## <a name="sizing"></a>Méretezése

Nagy HANA-példányok méretezési ugyanolyan helyzetet teremt, mint általában Hana méretezése. Meglévő telepített rendszereken, és azt szeretné, hogy a többi RDBMS tooHANA toomove, SAP számos olyan jelentést, a meglévő SAP rendszereken futó biztosít. Ha hello adatbázis áthelyezett tooHANA, ezekre a jelentésekre hello adatok ellenőrzése és számítja ki a memória hello HANA példány. Olvassa el a következő SAP megjegyzések tooget hello olvashat, hogyan toorun ezek azt jelenti, és hogyan tooobtain a legújabb javítások verzióját:

- [SAP Megjegyzés #1793345 - HANA az SAP Suite méretezése](https://launchpad.support.sap.com/#/notes/1793345)
- [Megjegyzés: #1872170 - csomag HANA és S/4 HANA méretezési jelentésre SAP](https://launchpad.support.sap.com/#/notes/1872170)
- [SAP Megjegyzés #2121330 - gyakran ismételt kérdések: A jelentés méretezése HANA SAP BW Programhoz](https://launchpad.support.sap.com/#/notes/2121330)
- [Megjegyzés: #1736976 - HANA méretezési BW jelentés SAP](https://launchpad.support.sap.com/#/notes/1736976)
- [Megjegyzés: #2296290 - új méretezési jelentés SAP BW a HANA a](https://launchpad.support.sap.com/#/notes/2296290)

Zöld mező megvalósítások esetében SAP gyors Szimbólumméretező elérhető toocalculate memóriára vonatkozó követelményeknek hello végrehajtásának SAP szoftver HANA felett.

Memóriára vonatkozó követelményeknek Hana növekednek adatmennyiség növekedésével, így toobe memóriahasználatának hello tudomása és esnie képes toopredict Mi a folyamatos toobe hello jövőbeli. Hello memória követelmények alapján, majd leképezheti a igény szerinti hello HANA nagy példány termékváltozatok egyikére.

## <a name="requirements"></a>Követelmények

Ez a lista állítja össze a SAP HANA futtatásához Azure (nagyobb példány).

**A Microsoft Azure:**

- Egy Azure-előfizetéshez társított tooSAP HANA Azure (nagy példányok) lehet.
- Microsoft Premier szintű támogatási szerződése. Lásd: [SAP támogatási Megjegyzés #2015553 – a Microsoft Azure SAP: támogatás Előfeltételek](https://launchpad.support.sap.com/#/notes/2015553) a konkrét információkra vonatkozó kapcsolódó toorunning SAP, az Azure-ban. HANA nagy példány egységek használatával 384 és több processzor szükség tooextend hello Premier szintű támogatási szerződése tooinclude Azure gyors válasz (ARR).
- Hello HANA nagy példányok termékváltozatok meg kell a méretezési végrehajtása után gyakorolni az SAP ezekre a műveletekre.

**Hálózati kapcsolat:**

- A helyszíni tooAzure közötti Azure ExpressRoute: tooconnect a helyszíni adatközpont tooAzure, győződjön meg arról, hogy tooorder legalább 1 GB/s kapcsolat az internetszolgáltató által biztosított. 

**Operációs rendszer:**

- Licencek az SUSE Linux Enterprise Server 12 SAP-alkalmazásokból.

> [!NOTE] 
> Operációs rendszer, amelyeket a Microsoft hello nincs regisztrálva a SUSE, és nem kapcsolódik SMT példánya.

- SUSE Linux előfizetés felügyeleti eszköz (SMT) üzembe helyezett Azure egy Azure virtuális gépen. SAP Hana Azure (nagy példányok) toobe és rendre SUSE (mivel nincs internet-hozzáférés HANA nagy példányok adatközpont belül) biztosít hello képességét. 
- Red Hat Enterprise Linux 6.7 vagy 7.2 SAP Hana licencet.

> [!NOTE]
> Operációs rendszer, amelyeket a Microsoft hello nincs regisztrálva a Red Hat, se nem csatlakoztatott informatikai tooa Red Hat előfizetés Manager példányához.

- Red Hat előfizetés kezelő üzembe helyezett Azure egy Azure virtuális gépen. Red Hat előfizetés Manager hello biztosít hello képességét SAP Hana Azure (nagy példányok) toobe és rendre Red Hat (mivel nincs közvetlen internet-hozzáférést a belül hello bérlői hello Azure nagy példány stamp telepítve van).
- SAP támogatási szerződés, valamint a Linux-szolgáltatónál toohave igényel. Ez a követelmény nem törlődnek, hello megoldás HANA nagy példányok vagy hello tényt, amely a futtatási Linux az Azure-ban. Ellentétben egyes hello Linux Azure katalógusában képek hello szolgáltatás díj nem szerepel hello megoldás ajánlat HANA nagy példányok. Van, mint egy ügyfél toofulfill hello követelményeinek SAP támogatási szerződésekre vonatkozó hello Linux terjesztő.   
   - SUSE Linux keressük meg a szerződés támogatási követelményeinek hello [SAP Megjegyzés #1984787 - SUSE LINUX Enterprise Server 12: telepítési jegyzetek](https://launchpad.support.sap.com/#/notes/1984787) és [SAP Megjegyzés #1056161 - SUSE prioritás támogatása SAP-alkalmazásokból](https://launchpad.support.sap.com/#/notes/1056161).
   - Red Hat Linux szüksége toohave hello megfelelő előfizetés szintek, amelyek tartalmazzák a támogatási és a szolgáltatás (toohello operációs rendszerek HANA nagy példányok frissíti. Red Hat azt javasolja, hogy egy "RHEL az SAP üzleti alkalmazások" előfizetés beolvasásakor. Támogatási és a szolgáltatások, ellenőrizze [SAP Megjegyzés #2002167 - Red Hat Enterprise Linux 7.x: telepítés és frissítés](https://launchpad.support.sap.com/#/notes/2002167) és [SAP Megjegyzés #1496410 - Red Hat Enterprise Linux 6.x: telepítés és frissítés](https://launchpad.support.sap.com/#/notes/1496410) a részletes adatokat.

**Adatbázis:**

- Licencek és a szoftver telepítési összetevők SAP Hana (platform vagy enterprise edition).

**Alkalmazások:**

- Licencek és a szoftver telepítési összetevők SAP tooSAP HANA és a kapcsolódó SAP támogatási szerződések összekötő alkalmazások.
- Licencek és a szoftver telepítési összetevők nem SAP alkalmazások kapcsolat tooSAP HANA Azure (nagy példányok) környezetben használt, és a kapcsolódó támogatási szerződés.

**Képességek:**

- Felhasználói élmény és az Azure infrastruktúra-szolgáltatási és az összetevőinek a Tudásbázis.
- Felhasználói élmény és az Azure-ban SAP alkalmazások és szolgáltatások telepítése a Tudásbázis.
- SAP HANA-telepítés hitelesített személyes.
- SAP felelős mérnök ismeretek toodesign magas rendelkezésre állású és vész-helyreállítási SAP HANA körül.

**SAP:**

- Általános gyakorlat, hogy az ügyfél egy SAP, és rendelkezik a támogatási szolgálathoz, SAP szerződés
- A hello típusú osztály HANA nagy példány SKU megvalósításokhoz, különösen a ajánlott tooconsult az SAP SAP HANA és az esetleges nagy méretű méretezett hardver-konfigurációit verzióiban.


## <a name="storage"></a>Storage

hello tárolási elrendezés SAP Hana Azure (nagy példányok) be van állítva az Azure szolgáltatásfelügyeleti ajánlott irányelvek részletes ismertetését lásd: hello SAP keresztül SAP HANA [SAP HANA tárhellyel kapcsolatos követelmények](http://go.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html) találhatók meg.

tárolási kötetként négy alkalommal hello memória kötet hello HANA nagy osztályt hello i. osztály típus rendelkeznek. Hello típus II osztály HANA nagy példány egységek hello tárolási nem lesz toobe több négy alkalommal. hello egységek rendelkeznek egy köteten, HANA tranzakciós napló biztonsági mentések tárolására számára készült. A további részletekért [hogyan tooinstall és SAP HANA (nagy példányok) konfigurálása az Azure-on](hana-installation.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Tekintse meg a következő táblázat tekintetében foglalás hello. hello táblázat nagyjából hello más köteteket hello különböző HANA nagy példány egységek megadott hello igénybe vehető.

| HANA nagy példány Termékváltozat | Hana/adatok | Hana/napló | Hana/megosztott | Hana/naplómásolat |
| --- | --- | --- | --- | --- |
| S72 | 1280 GB | 512 GB | 768 GB | 512 GB |
| S72m | 3328 GB | 768 GB |1280 GB | 768 GB |
| S192 | 4608 GB | 1024 GB | 1536 GB | 1024 GB |
| S192m | 11,520 GB | 1536 GB | 1792 GB | 1536 GB |
| S384 | 11,520 GB | 1536 GB | 1792 GB | 1536 GB |
| S384m | 12 000 GB | 2050 GB | 2050 GB | 2040 GB |
| S384xm | 16 000 GB | 2050 GB | 2050 GB | 2040 GB |
| S576 | 20 000 GB | 3100 GB | 2050 GB | 3100 GB |
| S768 | 28 000 GB | 3100 GB | 2050 GB | 3100 GB |
| S960 | 36,000 GB | 4100 GB | 2050 GB | 4100 GB |


Tényleges telepített kötetek üzembe helyezési és eszköz, amely használt tooshow hello kötetméreteket kicsit függően eltérhetnek.

Ha tovább HANA nagy példány SKU, néhány példa a lehetséges osztás darab alábbihoz hasonlóan fog kinézni:

| Memória partíció GB-ban | Hana/adatok | Hana/napló | Hana/megosztott | Hana/naplómásolat |
| --- | --- | --- | --- | --- |
| 256 | 400 GB | 160 GB | 304-ES GB | 160 GB |
| 512 | 768 GB | 384 GB | 512 GB | 384 GB |
| 768 | 1280 GB | 512 GB | 768 GB | 512 GB |
| 1024 | 1792 GB | 640 GB | 1024 GB | 640 GB |
| 1536 | 3328 GB | 768 GB | 1280 GB | 768 GB |


Ezen értékek nyers mennyiségi számát, amelyek a központi telepítés alapján némileg eltérőek lehetnek, és az eszközök toolook kihasználtságú hello kötetek. Is vannak más partíció méretét thinkable, például a 2,5 TB. Ezen tárolási méretek volna számítható ki, egy hasonló képlettel használt a fenti hello partíciók. hello kifejezés "partitions" nem jelzi, hogy hello operációs rendszer, a memória vagy a Processzor-erőforrások bármely olyan módon particionáltak. Adattárolási partíciókat csak a hello HANA példány egy toodeploy érdemes egyetlen HANA nagy példány egység jelzi. 

1 TB-os egységekben hello lehetőségét tooadd tárolási toopurchase további tárhely telepítve, az ügyfél lehet, hogy van szüksége további tárhelyet. A további tárhely további kötetként felveheti, vagy egy vagy több meglévő kötetek hello használt tooextend. Már nem lehetséges toodecrease hello méretek hello kötetek eredetileg telepített, és többnyire dokumentálnia hello oszlopfejlécneve(i) fenti. Nincs is lehetséges toochange hello nevek hello kötetek vagy csatlakoztatási nevek. hello tárolókötetek fent leírt módon csatolt toohello HANA nagy példány egységek NFS4 kötetként.

Ügyfélként dönthet úgy toouse storage-pillanatfelvételekkel a biztonsági mentés/visszaállítás és a katasztrófa utáni helyreállítás céljából. További részleteket a jelen témakör részletes leírást talál [SAP HANA (nagy példányok) magas rendelkezésre állás és vészhelyreállítás Azure](hana-overview-high-availability-disaster-recovery.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

### <a name="encryption-of-data-at-rest"></a>A tárolt adatok titkosítása
Nagy HANA-példányok használt hello tároló lehetővé teszi, hogy hello adatok átlátható titkosítási hello lemezen tárolt. A központi telepítéskor HANA nagy példány egység engedélyezhető a titkosítás eredő hello beállítás toohave lehetősége van. Is választhat toochange tooencrypted kötetek már hello telepítést követően. hello áthelyezést tooencrypted nem titkosított kötetek átlátható, és nem igényli a leállás. 

Hello típus i. osztály SKU, hello kötet hello rendszerindító LUN tárolják, a titkosított. Hello típus II osztály példányainak termékváltozatok a HANA nagy, esetén tooencrypt hello rendszerindító LUN operációsrendszer-módszerekkel együtt kell. 


## <a name="networking"></a>Hálózat

Azure hálózati hello architektúra egy nyilvános kulcsokra épülő toosuccessful alkalmazások központi telepítését figyelemmel SAP HANA-nagy példányokon. SAP HANA Azure (nagy példányok) üzemelő példányok esetében általában egy nagyobb SAP, számos különböző SAP-megoldások különböző méretű adatbázisok, a Processzor-erőforrás-felhasználás és a memóriahasználat a fekvő tájolás kell. Valószínűleg nem az összes az SAP rendszereket alapulnak SAP HANA, a SAP fekvő valószínűleg használó hibrid:

- SAP rendszerek helyszíni üzembe. Tootheir mérete miatt ezek a rendszerek nem jelenleg Azure-ban üzemeltetett; a klasszikus példa erre egy éles SAP ERP rendszer fut a Microsoft SQL Server (hello adatbázisként) további Processzort igényel, vagy a memória-erőforrások Azure virtuális gépeket biztosíthatnak.
- SAP SAP HANA-alapú rendszerek helyszíni üzembe.
- Azure virtuális gépeken telepített SAP rendszerek. Ezek a rendszerek fejlesztési, tesztelési, védőfal, vagy éles példányának hello SAP NetWeaver-alapú alkalmazások, amelyek sikeresen telepítheti az erőforrás-használat és a memória igény alapján (a virtuális gépeken), Azure-ban. Ezek a rendszerek is alapulhatnak például az SQL Server adatbázisok (lásd: [SAP támogatási Megjegyzés #1928533 – Azure SAP-alkalmazásokból: támogatott termékek és az Azure virtuális gép típusok](https://launchpad.support.sap.com/#/notes/1928533/E)) vagy SAP HANA (lásd: [SAP HANA hitelesített IaaS platformok](http://global.sap.com/community/ebook/2014-09-02-hana-hardware/enEN/iaas.html)).
- Telepített SAP alkalmazáskiszolgálók (a virtuális gépeken) Azure-ban Azure (nagy példány) SAP HANA kihasználhatja az Azure nagy példány stampek.

Míg egy hibrid SAP fekvő (a négy vagy több különböző központi telepítési forgatókönyv) jellemző, nincsenek Azure-beli teljes SAP fekvő eseteit sok ügyfél. A Microsoft Azure virtuális gépek egyre nagyobb teljesítményű, helyezze át az SAP-megoldások Azure ügyfelek hello száma növekszik.

Hálózati környezetében hello Azure szolgáltatásba telepített SAP rendszerek Azure nincs bonyolult. A következő alapelveket hello alapul:

- Az Azure virtuális hálózatokról (Vnetekről) kapcsolódó toobe toohello Azure ExpressRoute-kapcsolatcsoportot, amely a tooon helyi hálózaton kell.
- Csatlakozás a helyi tooAzure általában ExpressRoute-kapcsolatcsoportot rendelkeznie kell a sávszélesség 1 GB/s vagy újabb. A minimális sávszélesség lehetővé teszi a megfelelő sávszélesség az adatok átviteléhez a helyszíni és a futó Azure virtuális gépeken (valamint a végfelhasználók számára a helyi kapcsolat tooAzure rendszerek) rendszerek között.
- Azure kell toobe SAP rendszereinek állítsa be a Azure Vnetekhez toocommunicate egymással.
- Active Directory és a DNS a helyben tárolt kiterjeszthetőek az Azure ExpressRoute segítségével a helyi.


> [!NOTE] 
> A számlázási szempontjából csak egy Azure-előfizetéssel csak tooone egyetlen bérlő által egy adott Azure-régió, egy nagy példány stamp lehet társítani, és ezzel szemben egy nagy példány stamp egybérlős kapcsolható csak tooone Azure-előfizetés. Ez a tény nem különböző tooany van más számlázható objektumok az Azure-ban

SAP HANA Azure (nagy példányok) több különböző Azure-régiókban telepítéséhez, egy külön bérlői toobe eredményez üzembe hello nagy példány stamp. Azonban futtathatja mindkét alatt hello azonos Azure-előfizetés amennyiben ezek a példányok hello részei ugyanazon SAP fekvő. 

> [!IMPORTANT] 
> Csak az Azure Resource Manager deployment SAP HANA Azure (nagy példányok) használata támogatott.

 

### <a name="additional-azure-vnet-information"></a>További Azure VNet-információk

A sorrend tooconnect egy Azure virtuális hálózat tooExpressRoute, létre kell hozni egy Azure-átjáró (lásd: [kapcsolatos az ExpressRoute virtuális hálózati átjárók](../../../expressroute/expressroute-about-virtual-network-gateways.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). Használható az Azure-átjáró vagy ExpressRoute tooan infrastruktúra kívüli Azure (vagy tooan Azure nagy példány stamp), vagy Azure Vnet közötti tooconnect (lásd: [VNet – VNet-kapcsolat konfigurálva a Resource Manager PowerShell használatával ](../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). Hello Azure átjáró tooa legfeljebb négy különböző ExpressRoute-kapcsolatot is elérheti, mindaddig, amíg másik MS vállalati szélek (MSEE) útválasztók érkező ezeket a kapcsolatokat.  További információkért lásd: [SAP HANA (nagy példányok) infrastruktúra és az Azure-kapcsolatokat](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

> [!NOTE] 
> hello egy Azure átjáró átirányítást biztosít átviteli eltér mindkét esetben használja (lásd: [VPN-átjáró](../../../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). hello maximális átviteli sebesség azt érhető el a virtuális hálózat átjáróval 10 GB/s, egy ExpressRoute-kapcsolat használatával. Ne feledje, hogy egy Azure virtuális gép található egy Azure virtuális hálózatot és a rendszer között a fájlok másolása a helyszíni (adatfolyamként egyetlen másolatának) nem fog hello teljes átviteli képessége – hello különböző gateway SKU. tooleverage hello teljes sávszélesség hello hálózatok átjáró, akkor kell használnia több adatfolyamokat, vagy más fájlok másolása egyetlen fájl párhuzamos adatfolyamokat.


### <a name="networking-architecture-for-hana-large-instances"></a>Hálózati architektúra nagy HANA-példányok
hálózati architektúra HANA nagy példányok hello az alábbi módon el lehet különíteni a négy részből:

- A helyszíni hálózat és az ExpressRoute-kapcsolat tooAzure. Ez a kijelző hello ügyfelek tartomány és a csatlakoztatott tooAzure ExpressRoute keresztül. Ez a hello részt hello hello grafikus alábbi jobb alsó sarkában.
- Az Azure hálózatkezelés szerint röviden kapcsolatban a fentiekben ismertetett az Azure Vnetekhez, újra rendelkező átjárókat. Ez az egy olyan területre, ahol toofind hello megfelelő terveket kell az alkalmazások követelményeinek, biztonsági és megfelelőségi követelményeknek. HANA nagy példányok használata egy másik virtuális hálózatokat és Azure gateway SKU toochoose a számát figyelembe pontját. Ez a hello hello grafikus jobb felső sarkában hello részt.
- Nagy HANA-példányok keresztüli kapcsolat ExpressRoute technológia az Azure. Ez a kijelző telepített, és a Microsoft kezeli. Azure VNet(s) toohello áramkör, az ügyfél tooprovide bizonyos IP-címtartományok és hello központi telepítése a eszközök HANA nagy példányok kapcsolódás után hello ExpressRoute toodo szüksége (lásd: [(nagy példányok) SAP HANA-infrastruktúra és kapcsolat az Azure-on](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). 
- Hálózati HANA nagy példányát, ez utóbbi érték többnyire átlátszó meg ügyfélként.

![Az Azure VNet tooSAP Azure (nagy példányok) és a helyszíni HANA csatlakoztatva](./media/hana-overview-architecture/image3-on-premises-infrastructure.png)

hello tényt, hogy használja-e HANA nagy példányok nem változtatja meg a hello követelmény tooget a helyszíni eszközök ExpressRoute tooAzure keresztül csatlakoznak. Azt is nem változtatja meg hello követelményt, amely hello Azure virtuális gépek futtassa mely állomás hello alkalmazásréteg, amely a toohello HANA példányok HANA nagy példány egységekben üzemeltetett egy vagy több Vnetek-hez. 

hello különbség tooSAP a központi telepítések tiszta hello tények rendelkezik Azure-ban, amely:

- hello HANA nagy példány egységekbe, az ügyfél-bérlőt az Azure VNet(s) be egy másik ExpressRoute-kapcsolatcsoportot keresztül kapcsolódó. A sorrend tooseparate munkaterhelés esetére, hello helyi tooAzure Vnetek ExpressRoute- és hivatkozásokat Azure Vnet és HANA nagy példányok között nem osztható meg hello azonos útválasztók.
- hello munkaterhelés profil hello SAP alkalmazásréteg és hello HANA példány között a különböző jellegű a sok kisméretű kérelmek és kapacitásnövelés adatok például SAP HANA (eredményhalmazt) átviszi hello alkalmazás rétegbe.
- hello SAP alkalmazásarchitektúra érzékenyebb toonetwork késést biztosít a jellemző forgatókönyvek, ahol lekérdezi adatcsere a helyszíni és az Azure között.
- hello hálózatok átjáró rendelkezik, legalább két ExpressRoute-kapcsolatot, és mindkét kapcsolatok osztja hello hello hálózatok átjáró bejövő adatok maximális sávszélessége.

lehet, hogy Azure virtuális gépek és HANA nagy példány egységek közötti tapasztalt hello hálózati késés nagyobb, mint a szokásos hálózati oda-vissza késés VM-VM. Függő hello Azure-régió, mért hello értékek lépheti túl hello 0,7 ms körbejárási késleltetés átlagos alá besorolt [SAP Megjegyzés #1100926 - gyakran ismételt kérdések: hálózati teljesítményt](https://launchpad.support.sap.com/#/notes/1100926/E). Az ügyfelek azonban SAP HANA-alapú üzemi SAP alkalmazások nagyon meg SAP HANA nagy példányok telepítését. hello ügyfelek, akik telepítették az SAP-alkalmazásokból futó HANA nagy példány egységek használatával SAP HANA jelentése nagy fejlesztései. Mindazonáltal tesztelni kell az üzleti folyamatok alaposan Azure HANA nagy példányát.
 
A sorrend tooprovide determinisztikus hálózati késés Azure virtuális gépek és HANA nagy példány között hello választott hello Azure VNet átjáró-Termékváltozat nem lényeges. Hello a helyszíni és az Azure virtuális gépek közötti forgalmat, eltérően hello forgalom minta Azure virtuális gépek és nagy HANA-példányok közötti fejleszthet kicsi, de nagy felszakadásáig kérelmek és továbbított adatok kötetek toobe. A sorrend toohave ilyen felszakadásáig kezelni, erősen ajánlott hello UltraPerformance átjáró-Termékváltozat hello használata. Hello típusú osztály HANA nagy példány SKU hello UltraPerformance gateway SKU Azure VNet átjáróként hello használata megadása kötelező.  

> [!IMPORTANT] 
> A megadott hello hello SAP alkalmazás és az adatbázis rétegek közötti teljes hálózati forgalom csak hello HighPerformance, vagy UltraPerformance gateway SKU tartozó Vnetek esetében tooSAP Azure (nagy példányok) HANA kapcsolódás esetén támogatott. Típus II termékváltozatok példány HANA nagy csak UltraPerformance átjáró-Termékváltozat hello támogatott Azure VNet átjáróként.



### <a name="single-sap-system"></a>Egyetlen SAP rendszer

hello helyszíni infrastruktúra fent látható az Azure ExpressRoute keresztül csatlakozik, és hello ExpressRoute-kapcsolatcsoportot összekapcsolja azokat a Microsoft vállalati peremhálózati útválasztó (MSEE) (lásd: [ExpressRoute műszaki áttekintés](../../../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). Miután létrejött, adott útvonal csatlakozik a Microsoft Azure gerincét hello és, minden Azure-régió.

> [!NOTE] 
> Az Azure-beli SAP tájak, csatlakoztassa a toohello MSEE legközelebbi toohello hello SAP fekvő Azure-régiót. Az Azure nagy példány bélyegzők dedikált MSEE eszközök toominimize hálózati késés Azure virtuális gépek az Azure infrastruktúra-szolgáltatási és nagy példány bélyegzők keresztül csatlakoznak.

hello hello Azure virtuális gépeken, SAP alkalmazáspéldányok, üzemeltető virtuális hálózat átjárója csatlakoztatott toothat ExpressRoute-kapcsolatcsoportot, továbbá hello ugyanazt a virtuális hálózatot csatlakoztatott tooa külön dedikált MSEE útválasztó tooconnecting tooLarge példány stampek.

Ez az egyetlen SAP rendszer, ahol hello SAP alkalmazásréteg van az Azure-ban és az SAP HANA (nagy példányok) Azure-on fut hello SAP HANA-adatbázisból egy egyszerű példa. hello feltételezi, hogy hello hálózatok átjáró sávszélesség 2 GB/s, vagy 10 GB/s átviteli sebesség nem felel meg a szűk keresztmetszetek.

### <a name="multiple-sap-systems-or-large-sap-systems"></a>Több SAP rendszerek vagy nagy számú SAP-rendszerek

Ha több SAP rendszerek vagy nagy számú SAP-rendszerek van telepítve, csatlakozás tooSAP HANA Azure (nagy példányokat), akkor &#39; s ésszerű tooassume hello átviteli hello hálózatok átjáró válhat a szűk keresztmetszetek. Ebben az esetben a toosplit hello alkalmazás rétegek a több Azure Vnetekhez szüksége. Emellett előfordulhat, hogy recommendable toocreate különleges Vnetek tooHANA nagy példányok-hez az esetekben, például:

- NFS-megosztások mentéseket közvetlenül a hello HANA példányok a HANA nagy példányok tooa Azure-ban, amely futtatja
- Nagy biztonsági másolatait és egyéb fájlok másolása HANA nagy egységek toodisk példánytér által felügyelt Azure-ban.

Külön Vnetek, hogy a gazdagépen virtuális gépeket kezelő hello tárolási elkerülhetők a nagy méretű fájlok hatás vagy adatok átviteléhez HANA nagy példányok tooAzure hello hello virtuális gépeken futó hello SAP alkalmazásréteg ellátó hálózatok átjáró segítségével. 

Több méretezhető hálózati architektúra:

- Használja ki az egyetlen, nagyobb SAP alkalmazásréteg több Azure Vnet.
- Központi telepítése egy külön Azure virtuális hálózat minden egyes SAP rendszerben telepített, összehasonlított toocombining ezek SAP rendszerek alatt külön alhálózatokon hello ugyanazt a virtuális hálózatot.

 A több méretezhető hálózati architektúra SAP Hana Azure (nagy példány):

![SAP alkalmazásréteg telepítését a több Azure Vnetekhez keresztül](./media/hana-overview-architecture/image4-networking-architecture.png)

Ezeket az Azure Vnetekhez üzemeltetett hello alkalmazások közötti kommunikáció során történt elkerülhetetlen késés terhet hello SAP alkalmazásréteg vagy az összetevők telepítését a fent látható több Azure Vnetekhez keresztül vezette be. Alapértelmezés szerint hello hálózati forgalom Azure virtuális gépek között található különböző Vnetek útvonal hello MSEE útválasztók ebben a konfigurációban. Azonban óta 2016 szeptemberétől a útválasztási is lehet optimalizálni. hello módon toooptimize, és a kettő közötti kommunikáció hello késés kevesebb Vnetek által társviszony-létesítési Azure Vnetekhez belül hello ugyanabban a régióban. Akkor is, ha ezek Vnetekhez különböző előfizetésekhez. Azure VNet társviszony-létesítést, virtuális gépek két különböző Azure Vnetekhez hello kommunikációját segítségével használata hello Azure hálózati gerincét toodirectly kommunikálhatnak egymással. Ezáltal ábrázoló hasonló késéssel, mintha a virtuális gépek hello lenne hello ugyanazt a virtuális hálózatot. Mivel forgalom címzési IP-címtartományok hello Azure VNet-átjárón keresztül csatlakozó áthalad hello hello VNet egyedi virtuális hálózat átjárója. Kaphat Azure virtuális hálózat adatait társviszony-létesítés hello cikkben [Vnetben társviszony-létesítés](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview).


### <a name="routing-in-azure"></a>Az Azure-Útválasztás

Nincsenek két fontos hálózati útválasztási szempontok SAP Hana Azure (nagy példány):

1. Az Azure (nagy példányok) SAP HANA csak elérhetők, Azure virtuális gépek a dedikált hello ExpressRoute-kapcsolat; nem közvetlenül a helyszíni. Egyes felügyeleti ügyfelek és alkalmazások közvetlen hozzáférést igénylő, például SAP megoldás Manager fut a helyi, nem lehet kapcsolódni a toohello SAP HANA-adatbázisból.

2. SAP HANA Azure (nagy példányok) egységeken rendelkezik hozzárendelt IP-címeit hello kiszolgáló IP-készlet cím benyújtott hello ügyfélként között meg (lásd: [SAP HANA (nagy példányok) infrastruktúra és az Azure-kapcsolatokat](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) részletekért).  Az IP-cím hello Azure keresztül érhető el előfizetések és ExpressRoute, amely összeköti az Azure Vnetekhez tooHANA Azure (nagy példány). hello adott kiszolgálói IP-készlet címtartománya kívül hozzárendelt IP-címet közvetlenül hozzá rendelt toohello hardver egység és NAT'ed nincs többé hello első telepítések esetén az ilyen típusú megoldásra hello eset volt. 

> [!NOTE] 
> Ha szükséges tooconnect tooSAP HANA Azure (nagy példányok) egy _adatraktár_ forgatókönyv, ahol az alkalmazások és/vagy a végfelhasználók kell tooconnect toohello SAP HANA-adatbázisból (közvetlenül fut), egy másik hálózati összetevőjének kell lennie használt: egy fordított proxy tooroute adatok, a tooand. Ha például F5 BIG-IP, NGINX a Traffic Managerrel virtuális tűzfal/forgalom útválasztási megoldásként Azure szolgáltatásba telepített.

### <a name="internet-connectivity-of-hana-large-instances"></a>Internetkapcsolat nagy HANA-példányok
Nagy HANA-példányok nem rendelkezik közvetlen internetkapcsolattal. Ez korlátozza a képessé válik, például regisztrálása hello operációsrendszer-lemezképek közvetlenül hello az operációs rendszer szállítójához. Ezért szükség lehet a helyi SLES SMT kiszolgáló vagy az RHEL előfizetés Managerrel toowork

### <a name="data-encryption-between-azure-vms-and-hana-large-instances"></a>Azure virtuális gépek és nagy HANA-példányok közötti adattitkosítás
Nagy HANA-példányok és az Azure virtuális gépek között továbbított adatok nincsenek titkosítva. Azonban kizárólag hello exchange hello HANA DBMS ügyféloldali és JDBC-/ ODBC-alapú alkalmazások között engedélyezheti forgalom titkosítása. Hivatkozás [ebben a dokumentációban SAP által](http://help-legacy.sap.com/saphelp_hanaplatform/helpdata/en/db/d3d887bb571014bf05ca887f897b99/content.htm?frameset=/en/dd/a2ae94bb571014a48fc3b22f8e919e/frameset.htm&current_toc=/en/de/ec02ebbb57101483bdf3194c301d2e/plain.htm&node_id=20&show_children=false)

### <a name="using-hana-large-instance-units-in-multiple-regions"></a>Több régióba HANA nagy példány egységek használatával

Lehetséges, hogy más okból toodeploy SAP HANA Azure (nagy példányok) a több Azure-régiók vész-helyreállítási mellett. Lehet, hogy azt szeretné, hogy az egyes üzembe helyezett hello hello virtuális gépek HANA nagy példányok tooaccess különböző Vnetek hello régióban. Mivel hello IP-címek hozzárendelve másik toohello HANA nagy példányok egység a rendszer nem propagálja túl hello Azure Vnet (közvetlenül csatlakozó az átjárópéldányokról toohello keresztül), nincs enyhe módosítása toohello fent bevezetett hálózatok tervezési: egy Azure VNet-átjáró kezelni tud a négy különböző ExpressRoute-Kapcsolatcsoportok különböző MSEEs kívül, és minden egyes virtuális hálózatot, amely hello nagy példány bélyegzők csatlakoztatott tooone csatlakoztatott toohello nagy példány stamp egy másik Azure-régiót.

![Az Azure Vnetekhez különböző Azure-régiókban tooAzure nagy példány bélyegzők csatlakoztatva](./media/hana-overview-architecture/image8-multiple-regions.png)

hello a fenti ábra azt mutatja be, hogyan hello különböző Azure Vnet is régiók a következők csatlakoztatott tootwo különböző ExpressRoute-Kapcsolatcsoportok tartozó mindkét Azure-régiók használt tooconnect tooSAP HANA Azure (nagy példány). újonnan bevezetett hello kapcsolatok olyan hello téglalap alakú piros sorok. Ezeket a kapcsolatokat kívül hello Azure Vnet, egy adott Vnetek futó hello virtuális gépeket érhetik el mindegyik hello hello két régióban üzembe különböző HANA nagy példányok egység. Ahogy a fenti hello grafikus látni, feltételezzük, hogy rendelkezik-e két ExpressRoute-kapcsolatot a helyszíni toohello két Azure-régiók; Vész-helyreállítási okokból ajánlott.

> [!IMPORTANT] 
> Több ExpressRoute-Kapcsolatcsoportok használata esetén AS elérési fertőző és a helyi beállításokat szabályozó BGP beállítások megfelelő forgalom használt tooensure kell lennie.


