---
title: "Hálózati ajánlott biztonsági eljárások aaaAzure |} Microsoft Docs"
description: "Ez a cikk ajánlott eljárások a hálózati biztonság Azure-képességek a beépített biztosít."
services: security
documentationcenter: na
author: TomShinder
manager: swadhwa
editor: TomShinder
ms.assetid: 7f6aa45f-138f-4fde-a611-aaf7e8fe56d1
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/09/2017
ms.author: TomSh
ms.openlocfilehash: 5867dea358b4da65c65b3e52fcab7e687e981642
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-network-security-best-practices"></a>Azure-hálózat ajánlott biztonsági eljárások
A Microsoft Azure lehetővé teszi virtuális gépek és a készülék tooconnect tooother hálózati eszközök párt Azure virtuális hálózatok. Egy Azure virtuális hálózatra egy virtuális hálózati szerkezet, amely lehetővé teszi a tooconnect virtuális hálózati illesztő kártyák tooa virtuális hálózati tooallow TCP/IP-alapú közötti kommunikáció engedélyezve van a hálózati eszközök. Az Azure virtuális gépek Azure-beli virtuális hálózathoz csatlakoztatott tooan esetén képes tooconnect toodevices hello Azure virtuális hálózaton, különböző Azure virtuális hálózatok, az hello Internet, vagy akár a saját helyszíni hálózatokon.

Ez a cikk ismertetik Azure-hálózat ajánlott biztonsági eljárások gyűjteménye. Az alábbi gyakorlati tanácsok az Azure hálózati tapasztalatunk származik, és hello véleményeket ügyfelek, például saját maga.

Az egyes ajánlott eljárás azt ismertetjük:

* Milyen hello ajánlott eljárás
* Miért érdemes tooenable, hogy a legjobb
* Mi lehet hello eredmény, ha Ön nem tooenable hello ajánlott
* Lehetséges alternatívák toohello ajánlott
* Hogyan szerezhet tooenable hello ajánlott

Azure hálózati biztonság ajánlott eljárások a cikkben az együttműködési véleményét, és az Azure platform olyan képességeit és a szolgáltatáskészletek, alapján ez a cikk írásának hello időpontban léteznek. Vélemények és technológiák változnak az idők, és ez a cikk lesznek frissítve egy rendszeresen tooreflect ezeket a módosításokat.

Az Azure hálózati ajánlott biztonsági eljárások cikkben említett a következők:

* Logikailag szegmens alhálózatok
* Útválasztási viselkedés szabályozása
* A kényszerített bújtatás engedélyezése
* Virtuális hálózati készülékek használata
* Biztonsági zónázást DMZ-k telepítése
* Kerülje a kitettség toohello Internet a dedikált WAN-kapcsolatok
* Hasznos üzemidő és a teljesítmény optimalizálása
* Használja a globális terheléselosztás
* Tiltsa le a virtuális gép RDP-hozzáférést tooAzure
* Engedélyezze az Azure Security Center
* Az Azure az Adatközpont kiterjesztése

## <a name="logically-segment-subnets"></a>Logikailag szegmens alhálózatok
[Egy Azure virtuális hálózatot](https://azure.microsoft.com/documentation/services/virtual-network/) hasonló tooa LAN a helyszíni hálózaton vannak. hello mögött egy Azure virtuális hálózatra ötlet, hogy hozzon létre egy egyetlen magán IP-cím terület-alapú hálózati amelyen minden elhelyezheti a [Azure virtuális gépek](https://azure.microsoft.com/services/virtual-machines/). hello (10.0.0.0/8), az osztály A B osztály (172.16.0.0/12) és a C osztályú (192.168.0.0/16) tartományok szerepelnek hello privát IP-címterek érhető el.

Hasonló toowhat, a helyszíni, érdemes toosegment hello nagyobb címtartományt alhálózatokra. Használhat [CIDR](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) alapú alhálózatok alapelvek toocreate alhálózatát.

Útválasztás alhálózatok között művelet automatikusan megtörténik, és nem kell toomanually útválasztási táblázataiba konfigurálása. Hello alapértelmezett beállítás azonban, hogy vannak-e nincs hálózati hozzáférés-vezérlést hoz létre az Azure Virtual Network hello hello alhálózatok között. A sorrend toocreate hálózati hozzáférés-vezérlést alhálózatok között szüksége lesz tooput valami hello alhálózatok között.

Hello egyikét használhatja ez a feladat tooaccomplish egy [hálózati biztonsági csoport](../virtual-network/virtual-networks-nsg.md) (NSG). Az NSG-k egyszerű állapot-nyilvántartó Csomagvizsgálat hello 5 rekordos (hello forrás IP-címe, forrásport, cél IP-címe, célport és 4. rétegbeli protokoll) használó eszközöket közelítse toocreate engedélyezése vagy megtagadása szabály a hálózati forgalmat. Akkor engedélyezheti vagy tagadhatja meg tooand forgalom egyetlen IP-címet, több IP-címekről tooand vagy a teljes alhálózat még akkor is, tooand.

Az NSG-k használatával, az alhálózatok közötti hálózati hozzáférés-vezérlés lehetővé teszi tooput erőforrásokhoz tartozó toohello ugyanazt biztonsági zóna vagy a saját alhálózatokon szerepkör. Például egy egyszerű 3 szintű alkalmazás egy webes réteghez, egy alkalmazás logikai rétegből és adatbázis-rétegből gondol. Helyezze a virtuális gépeket, hogy ezek a rétegek tooeach tartozó saját alhálózatokra. Ezután használhatja az NSG-k toocontrol forgalom hello alhálózatok között:

* Webes réteg virtuális gépein csak is kezdeményezhető a kapcsolatok toohello alkalmazás logika gépeket, és csak fogadásra hello Internet közötti kapcsolatok
* Alkalmazás logika virtuális gépek csak kezdeményezhet az adatbázis-rétegből kapcsolatokhoz, és csak fogadásra hello webes réteg közötti kapcsolatok
* Adatbázis réteg virtuális gépein bármi kívül a saját alhálózati kapcsolat nem indítható el, és csak fogadásra hello alkalmazás logikai rétegből közötti kapcsolatok

Hálózati biztonsági csoportok és hogyan használhatja őket toologically szegmens kapcsolatos további toolearn az Azure virtuális hálózat, olvassa el hello cikk [Mi az a hálózati biztonsági csoport](../virtual-network/virtual-networks-nsg.md) (NSG).

## <a name="control-routing-behavior"></a>Útválasztási viselkedés szabályozása
Ha egy virtuális gép elhelyezése egy Azure virtuális hálózatra, láthatja, hogy hello a virtuális gép csatlakozni tud tooany más virtuális gép a hello azonos Azure virtuális hálózatban, akkor is, ha hello más virtuális gépek különböző alhálózatokon. hello miért Ez lehet az oka, hogy nincs-e rendszerútvonalak, amely alapértelmezés szerint engedélyezve vannak, amelyek lehetővé teszik az ilyen típusú kommunikáció gyűjteménye. Ezek az alapértelmezett útvonalak csatlakozhatnak virtuális gépek hello azonos Azure Virtual Network tooinitiate egymással, illetve hello Internet (a kimenő kommunikáció toohello Internet csak).

Számos telepítési forgatókönyv hello alapértelmezett rendszerútvonalak hasznosak lehetnek, amíg vannak olyan helyzetek, amikor kívánt toocustomize hello útválasztási konfigurációja a központi telepítések. Ezek a testreszabások lehetővé teszi a tooconfigure hello következő ugrási cím tooreach megadott célhelyekre.

Azt javasoljuk, hogy felhasználó által megadott útvonalak konfigurálásához, amikor telepít egy biztonsági lesz döntésről bővebben újabb ajánlott eljárás a virtuális hálózati berendezések.

> [!NOTE]
> felhasználó által megadott útvonal nem szükségesek, és a legtöbb esetben hello alapértelmezett rendszerútvonalak működik.
>
>

Felhasználó által definiált útvonalakra vonatkozó részletesebb és hogyan tooconfigure hello cikk elolvasni őket [Mik azok a felhasználó által megadott útvonalak és IP-továbbítás](../virtual-network/virtual-networks-udr-overview.md).

## <a name="enable-forced-tunneling"></a>A kényszerített bújtatás engedélyezése
toobetter megérteni a kényszerített bújtatás, a rendszer milyen "Alagútkezelés" hasznos toounderstand.
hello leggyakoribb példa vegyes alagútkezelést a VPN-kapcsolatok látható. Tegyük fel, hogy a VPN-kapcsolatot a szállodák hely tooyour vállalati hálózatról. Ez a kapcsolat lehetővé teszi tooconnect tooresources a vállalati hálózaton, és nyissa meg a vállalati hálózaton lévő összes kommunikáció tooresources hello VPN-alagúton keresztül.

Mi történik, ha azt szeretné, hogy az hello Internet tooconnect tooresources? Vegyes Alagútkezelés engedélyezése esetén ezeket a kapcsolatokat lépjen közvetlenül toohello Internet, és nem hello VPN használatával bújtatását. Néhány biztonsági szakértők fontolja meg a toobe kockázatot, és ezért javasoljuk, hogy Alagútkezelés le kell tiltani, és az összes kapcsolat, hello Internet felé és szánt a vállalati erőforrásokhoz, nyissa meg hello VPN-alagúton keresztül. hello ennek előnye, hogy kapcsolatok toohello Internet kényszerítve vannak-e majd hello vállalati hálózati biztonsági eszközön, amely így nem hello esetben, ha hello VPN-ügyfél toohello Internet kívül hello VPN-alagút létrejött a kapcsolat.

Most tegyük kapcsolja a biztonsági toovirtual gépek egy Azure virtuális hálózaton. alapértelmezett útvonalak hello Azure virtuális hálózat virtuális gépek tooinitiate forgalom toohello Internet engedélyezése. Ez túl jelenthet biztonsági kockázatot jelent, ezeket a kimenő kapcsolatokat hello támadási felületét a virtuális gépek nő, és a támadók javítható.
Ezért azt javasoljuk, hogy kényszerített bújtatást a virtuális gépeken Ha közötti kapcsolatot nyújthassanak az Azure virtuális hálózat és a helyszíni hálózat között engedélyezze. Az előadás közötti kapcsolatot nyújthassanak található Azure hálózati ajánlott eljárásokat.

Ha nincs helyszínek közötti kapcsolat, ellenőrizze, hogy használatakor élvezheti a (korábban tárgyalt) hálózati biztonsági csoportok vagy Azure virtuális hálózat biztonsági készülékek (tárgyaltuk mellett) tooprevent kimenő kapcsolatok toohello interneten az az Azure virtuális Gépek.

További részletek toolearn kényszerített bújtatás, és hogyan tooenable, olvassa el a cikk hello [PowerShell és az Azure Resource Manager használatával konfigurálja a kényszerített bújtatás](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md).

## <a name="use-virtual-network-appliances"></a>Virtuális hálózati készülékek használata
Amíg a hálózati biztonsági csoportok és felhasználói definiált útválasztási biztosíthat egy bizonyos szintű hálózati biztonság: hello hello hálózati és a szállítási réteg [OSI-modell](https://en.wikipedia.org/wiki/OSI_model), toobe olyan helyzetekben, ahol meg lesz hasznos vagy szükséges tooenable készül hello verem magas szintű biztonsági. Ilyen helyzetekben azt javasoljuk, hogy telepít virtuális hálózati biztonsági készülékek Azure partnerei által biztosított.

Azure hálózati biztonsági készülékek által biztosított jelentősen fokozott biztonsági szintek mi van hálózati szintű vezérlők által szolgáltatott keresztül. Virtuális hálózati biztonsági készülékek által biztosított hello hálózati biztonsági képességei többek között:

* Firewalling
* Behatolás-észlelés/behatolás megelőzése
* A biztonsági rés kezelése
* Alkalmazás-vezérlő
* Hálózatalapú anomáliadetektálás
* Webes szűrése
* A víruskereső
* Botnet védelme

Ha szüksége van egy magasabb szintű hálózati biztonság szerezheti be a hálózati szintű hozzáférés-vezérlést, majd azt javasoljuk, hogy vizsgálja meg, és központi telepítése az Azure-beli virtuális hálózat biztonsági készülékek.

milyen Azure-beli virtuális hálózat biztonsági készülékek érhetők el, és azok képességeinek toolearn látogasson el a hello [Azure piactér](https://azure.microsoft.com/marketplace/) , és keressen a "biztonság" és "hálózati biztonság".

## <a name="deploy-dmzs-for-security-zoning"></a>Biztonsági zónázást DMZ-k telepítése
DMZ "szegélyhálózaton" fizikai vagy logikai hálózati szegmenssel tervezett tooprovide biztonsági az eszközök és az hello Internet között további réteget. hello hello DMZ célja tooplace speciális hálózati hozzáférési eszközök hello DMZ hálózati hello szélén, hogy csak a kívánt forgalom engedélyezve van, hello hálózati biztonsági eszköz túli és az Azure virtuális hálózatra.

DMZ-k hasznosak, mert a hálózati hozzáférés-vezérlési felügyeletet, naplózás a figyelési és jelentéskészítési hello eszközökre az Azure Virtual Network hello szélén összpontosíthat. Itt volna általában engedélyeznie DDoS megelőzése, behatolás-észlelés/behatolás megelőzési rendszerek (Azonosítók/IP-CÍMEK), a tűzfalszabályok és házirendek, webes szűrési, hálózati kártevőirtó és több. hello hálózati biztonsági eszközök hello internetes és az Azure virtuális hálózat között elhelyezkedő, és mindkét hálózat illesztőfelület.

Ez nem DMZ alapszintű hello kialakítása, nincsenek számos különböző DMZ kialakításokról, például a végpontok közötti, hármas többcímes, többhelyű, stb.

Ajánlott az összes kiemelt biztonságú környezetek, fontolja meg, hogy a Szegélyhálózaton tooenhance hello szintű hálózati biztonság, az az Azure-erőforrások telepítése.

További információk a DMZ-k és hogyan toodeploy őket az Azure, olvassa el hello cikk toolearn [Microsoft más Felhőszolgáltatásaival és a hálózati biztonsági](../best-practices-network-security.md).

## <a name="avoid-exposure-toohello-internet-with-dedicated-wan-links"></a>Kerülje a kitettség toohello Internet a dedikált WAN-kapcsolatok
Számos szervezet hello hibrid informatikai útvonal választotta. A hibrid informatikai hello a vállalat információs eszközeinek némelyike az Azure, míg mások továbbra is a helyszíni. Sok esetben a közben egyéb összetevők továbbra is a helyszíni az Azure-ban egy szolgáltatás egyes összetevőinek fog futni.

A hello hibrid informatikai forgatókönyv általában van valamilyen közötti kapcsolatot nyújthassanak. A létesítmények közötti kapcsolat lehetővé teszi hello vállalati tooconnect a helyszíni hálózatokhoz tooAzure virtuális hálózatok. Nincsenek elérhető két létesítmények közötti csatlakozási megoldásokat biztosítják:

* Telephelyek közötti VPN
* ExpressRoute

[Telephelyek közötti VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md) a helyszíni hálózat és az Azure virtuális hálózat közötti virtuális magánhálózati kapcsolat jelöli. A kapcsolat történik több mint hello Internet, és lehetővé teszi a túl "alagút" egy titkosított hivatkozást a hálózat és az Azure között lévő információkat. Telephelyek közötti VPN egy biztonságos, érett technológia, amely különböző méretű vállalatok által évtizedeken van telepítve. Bújtatás titkosítás használatával történik [IPsec-bújtatásos mód](https://technet.microsoft.com/library/cc786385.aspx).

A megbízható, megbízható és a meghatározott technológiája pedig a telephelyek közötti VPN belül hello alagút forgalmi hello Internet bejárása. Ezenkívül sávszélessége viszonylag korlátozott tooa legfeljebb 200 MB/s kapcsolatban.

A létesítmények közötti kapcsolatokat. szükséges egy rendkívüli szintű biztonsági vagy a teljesítmény, azt javasoljuk, hogy a létesítmények közötti kapcsolatot használjon Azure ExpressRoute. ExpressRoute dedikált WAN a helyszíni hely vagy egy Exchange-szolgáltató közötti kapcsolat. Mivel ez egy telco kapcsolat, az adatok nem utazik hello interneten keresztül, így nem kitett toohello internetes adatátvitel járó esetleges kockázatokat.

Azure ExpressRoute működésével kapcsolatos további toolearn és hogyan toodeploy, olvassa el a cikk hello [ExpressRoute műszaki áttekintés](../expressroute/expressroute-introduction.md).

## <a name="optimize-uptime-and-performance"></a>Hasznos üzemidő és a teljesítmény optimalizálása
Titkosítás, integritás és rendelkezésre állás (CIA) alkotják hello háromrészes mai legtöbb segíti elő biztonsági modell. Bizalmas titkosítási és az adatvédelem, integritás kapcsolatos meggyőződött arról, hogy jogosulatlan személyek nem módosítja a adatokat, és rendelkezésre állási tárgya biztosítja, hogy a felhatalmazott személyek képesek tooaccess hello információk jogosultak legyenek tooaccess. Hiba történt a ezek a területek közül bármelyik egy történő lehetséges behatolást biztonsági jelöli.

Rendelkezésre állási elképzelhető, hogy hasznos üzemidő és teljesítményére. Ha a szolgáltatás nem működik, információ nem érhető el. Ha teljesítményt, gyenge adatként toomake hello használhatatlanná, majd azt is érdemes lehet hello adatok toobe nem érhető el. Ezért biztonsági szempontból, igazolnia kell toodo választott azt is, a szolgáltatások rendelkezik-e optimális hasznos üzemidő és teljesítmény toomake.
Olyan népszerű és hatékony módszert használ, tooenhance rendelkezésre állás és teljesítmény toouse terheléselosztási. Terheléselosztás a szolgáltatás részét képező kiszolgálók közötti hálózati forgalom elosztása módszer. Például ha a szolgáltatás részeként előtér-webkiszolgálók, a több előtér-webkiszolgáló között terheléselosztási toodistribute hello forgalom használhatja.

A forgalom eloszlása növeli a rendelkezésre állási, mivel a hello webkiszolgálók egyik nem érhető el, ha hello terheléselosztót a forgalom toothat és átirányítási forgalom toohello kiszolgáló még online küldése leáll. Terheléselosztás lehetővé teszi a teljesítményt, mert hello processzor, a hálózati és a munkaterhelés számára küldött kérelmek terjesztése hello elosztott terhelésű kiszolgálók közötti.

Azt javasoljuk, hogy alkalmaz terheléselosztás együtt, és a szolgáltatások megfelelő. A következő részekben hello indították lesz oldjuk.
: Hello Azure Virtual Network szint az Azure biztosít a három elsődleges betölti terheléselosztási beállításokat:

* HTTP-alapú terheléselosztás
* Külső terheléselosztási
* Belső terheléselosztás

## <a name="http-based-load-balancing"></a>HTTP-alapú terheléselosztás
HTTP-alapú terheléselosztást végző adatbázisok döntéseket hozzanak arról, hogy milyen toosend kiszolgálókapcsolatok jellemzői hello HTTP protokoll használatával. Azure rendelkezik egy HTTP terheléselosztóhoz, amely a hello nevű Alkalmazásátjáró kerül.

Azt javasoljuk, hogy Ön Azure Application Gateway, ha:

* Kérelmek igénylő alkalmazások a hello ugyanazon felhasználó vagy Windows-ügyfélen munkamenet tooreach hello ugyanahhoz a háttér-virtuális géphez. Erre példák volna vásárlás, bevásárlókocsiból alkalmazások és a webes levelezési kiszolgálóján.
* Protokollterhelés Application Gateway kihasználásával server webfarmok toofree az SSL-lezárást alkalmazások [SSL kiszervezési](https://f5.com/glossary/ssl-offloading) szolgáltatás.
* Alkalmazások, például egy tartalomkézbesítési hálózat, amely esetében azonos hosszan futó TCP-kapcsolati toobe irányíthatja át vagy elosztott terhelésű toodifferent háttérkiszolgálók betöltése hello több HTTP-kérelmekre.

További információ az Azure Application Gateway működése, és hogyan használhatja a központi telepítés toolearn olvassa el a hello cikk [átjáró – áttekintés](../application-gateway/application-gateway-introduction.md).

## <a name="external-load-balancing"></a>Külső terheléselosztási
Külső terheléselosztás akkor történik meg a egy Azure virtuális hálózaton található kiszolgálók között elosztott terhelésű hello Internet a bejövő kapcsolatok esetén. hello Azure külső terheléselosztó adja meg ezt a lehetőséget, és azt javasoljuk, hogy használja, amikor nem feltétlenül szükséges a kapcsolódó munkamenetek hello vagy SSL-kiszervezés.

Ezzel szemben tooHTTP-alapú terheléselosztást, külső terheléselosztó hello információkat használja hello hálózati és a szállítási réteg hello OSI hálózati modell toomake döntést arról, milyen server tooload egyenleg kapcsolatot.

Azt javasoljuk, hogy használjon külső terheléselosztás Ha [állapot nélküli alkalmazások](http://whatis.techtarget.com/definition/stateless-app) hello Internet érkező kéréseket fogad.

További információ az Azure külső terheléselosztó hello működése, és hogyan telepítheti azt, toolearn olvassa el a hello cikk [létrehozásához egy Internet Facing terheléselosztót a Resource Manager PowerShell-lel](../load-balancer/load-balancer-get-started-internet-arm-ps.md).

## <a name="internal-load-balancing"></a>Belső terheléselosztás
Belső terheléselosztás a hasonló tooexternal terheléselosztás és felhasználási hello azonos mechanizmus tooload egyenleg kapcsolatok toohello mögött lévő kiszolgálók őket. hello csak különbség az, hogy hello terheléselosztó ebben az esetben elfogadja a virtuális gépekről, amelyek nincsenek hello internetes kapcsolatok. A legtöbb esetben az elfogadás terheléselosztás hello kapcsolatok egy Azure virtuális hálózatán lévő eszközök által kezdeményezett.

Azt javasoljuk, hogy a belső terheléselosztási forgatókönyvek, amelyek előnyösek, ezt a képességet, például amikor tooload egyenleg kapcsolatok tooSQL kiszolgálók vagy belső webkiszolgálók kell használja.

További információ az Azure belső terheléselosztás működése, és hogyan telepítheti azt, toolearn olvassa el a hello cikk [létrehozásához egy PowerShell-lel belső terheléselosztót](../load-balancer/load-balancer-get-started-internet-arm-ps.md#update-an-existing-load-balancer).

## <a name="use-global-load-balancing"></a>Használja a globális terheléselosztás
Lehetővé teszi a nyilvános felhőre toodeploy globálisan elosztott adatközpontokban hello világ számos országában dolgoznak található összetevők rendelkező alkalmazások. Ez akkor lehetséges, a Microsoft Azure tooAzure meg globális adatközpont jelenléte miatt. Ezzel szemben toohello terheléselosztási technológiák már említettük, globális terheléselosztás lehetővé teszi lehetséges toomake szolgáltatások akkor is, ha a teljes adatközpontok válhatnak nem érhető el.

Az ilyen típusú globális terheléselosztásról az Azure kihasználásával kaphat [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/). A TRAFFIC Manager összekapcsolásával lehetséges tooload egyenleg kapcsolatok tooyour szolgáltatások hello felhasználói hello helye alapján.

Például ha hello felhasználó, hogy így egy kérelem tooyour szolgáltatással való hello Európa, a hello kapcsolat irányított tooyour szolgáltatások EU-adatközpontban található. A globális terheléselosztási Traffic Manager segítségével tooimprove teljesítmény mert legközelebbi adatközpont toohello kapcsolódás része gyorsabb, mint toodatacenters, amelyek távolságra.

Hello rendelkezésre állási oldalon globális terheléselosztás lehetővé teszi, hogy a szolgáltatás elérhető legyen-e akkor is, ha az egész adatközpont kell válnia nem érhető el.

Például, ha egy Azure-adatközpontban kell válnia nem érhető el megfelelő tooenvironmental okok vagy kellő toooutages (például a regionális hálózati hibák), a kapcsolatok tooyour szolgáltatás lenne toohello legközelebbi online datacenter átirányítva. A globális terheléselosztás úgy érhető el, amelyet létrehozhat Traffic Manager DNS-házirendek előnyeit.

Azt javasoljuk, hogy használja-e a Traffic Manager minden fejleszt felhőalapú megoldás, amely széles körben elosztott hatókörrel rendelkezik a különféle régiókban, és hasznos üzemidő lehetséges legmagasabb szintű hello igényel.

További tudnivalók Azure Traffic Manager toolearn és hogyan toodeploy, olvassa el a cikk hello [Mi az a Traffic Manager](../traffic-manager/traffic-manager-overview.md).

## <a name="disable-rdpssh-access-tooazure-virtual-machines"></a>Tiltsa le az RDP/SSH-elérést tooAzure virtuális gépek
Már lehetséges tooreach Azure virtuális gépek hello segítségével [a távoli asztal protokoll](https://en.wikipedia.org/wiki/Remote_Desktop_Protocol) (RDP) és hello [Secure Shell](https://en.wikipedia.org/wiki/Secure_Shell) (SSH) protokoll. Ezeket a protokollokat teszik lehetővé toomanage virtuális gépek távoli helyekről és standard, datacenter számítástechnikai.

hello potenciális biztonsági problémát, amelynek segítségével ezeket a protokollokat hello interneten keresztül, mert a támadók használhatják különböző [találgatásos](https://en.wikipedia.org/wiki/Brute-force_attack) technikák toogain hozzáférés tooAzure virtuális gépek. Miután hello támadók hozzáférést szereznek, kiindulási pontot használják a virtuális gép az Azure virtuális hálózaton lévő más gépekkel veszélyeztetése, vagy még akkor is a hálózati eszközök Azure-on kívüli támadás.

Ebből kifolyólag javasoljuk, hogy tiltsa le a közvetlen RDP és SSH hozzáférés tooyour Azure virtuális gépek hello Internet a. Közvetlen RDP és SSH hozzáférést a hello Internet le van tiltva, miután más használható tooaccess ezek a virtuális gépek távoli kezelési lehetőség közül választhat:

* Pont – hely típusú VPN
* Telephelyek közötti VPN
* ExpressRoute

[Pont – hely típusú VPN](../vpn-gateway/vpn-gateway-point-to-site-create.md) a távelérési VPN-ügyfél/kiszolgáló kapcsolatot egy másik kifejezés. A pont-pont VPN lehetővé teszi, hogy egy egyfelhasználós tooconnect tooan Azure Virtual Network hello interneten keresztül. Miután hello pont – hely kapcsolat jön létre, hello felhasználó fog tudni toouse RDP vagy SSH tooconnect tooany virtuális gépeket hello Azure virtuális hálózat található felhasználói hello csatlakoztatja toovia pont-pont VPN. A parancs feltételezi, hogy hello a felhasználó jogosult tooreach azon virtuális gépek van.

Pont – hely típusú VPN, annál biztonságosabb közvetlen RDP és az SSH-kapcsolatok, mivel hello felhasználó tooauthenticate kétszer előtt csatlakozó tooa virtuális gép. Először a felhasználói igények tooauthenticate hello (és engedélyezni kell) tooestablish hello pont-pont VPN-kapcsolat; Ezután felhasználói igények tooauthenticate hello (és engedélyezni kell) tooestablish hello RDP és az SSH-munkamenetet.

A [telephelyek közötti VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md) hello interneten keresztül egy teljes hálózati tooanother hálózathoz csatlakozik. A pont-pont VPN tooconnect használhatja a helyszíni hálózati tooan Azure-beli virtuális hálózathoz. Ha a telephelyek közötti VPN központi telepítése, az a helyi hálózaton tud tooconnect toovirtual gépek az Azure virtuális hálózat lesz hello RDP segítségével vagy SSH protokoll több mint hello pont-pont VPN-kapcsolatot, és nem szükséges tooallow közvetlen RDP és az SSH hello interneten keresztüli eléréséhez.

Használhat egy dedikált WAN-kapcsolaton tooprovide funkció hasonló toohello telephelyek közötti VPN. hello fő különbség a rendszer az 1. hello dedikált WAN-kapcsolat nem bejárása hello Internet és a 2. dedikált WAN-kapcsolatok jellemzően további stabil és performant. Azure hello formájában dedikált WAN-kapcsolaton megoldást kínál, [ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/).

## <a name="enable-azure-security-center"></a>Engedélyezze az Azure Security Center
Az Azure Security Center segít a megakadályozása, észlelésében és kezelésében toothreats, és biztosítja, hogy lássák, és szabályozhatják, az Azure-erőforrások biztonságának hello nőtt. Az ügyfél összes előfizetésére kiterjedő, integrált biztonsági monitorozást és szabályzatkezelést biztosít, megkönnyíti a nehezen észlelhető fenyegetések azonosítását, és számos biztonsági megoldással együttműködik.

Azure Security Center segítségével optimalizálása és figyelheti a hálózati biztonsági megoldást:

* Hálózati biztonsági javaslatokat biztosít
* A hálózati biztonsági konfiguráció hello állapotának figyelése
* Riasztás, akkor alapú toonetwork fenyegetések mindkét hello végpont és a hálózati szintű

Erősen ajánlott, hogy engedélyezi-e az Azure Security Center az összes Azure-környezetekhez.

További információk az Azure Security Center, és hogyan tooenable azt a központi telepítések, olvassa el hello cikk toolearn [Security Center bemutatása tooAzure](../security-center/security-center-intro.md).

## <a name="securely-extend-your-datacenter-into-azure"></a>Az Adatközpont biztonságosan kiterjeszti Azure
Sok vállalati informatikai szervezeteknek olyan eszközökre tooexpand helyett inkább a helyszíni adatközpontjaikat hello felhőbe. A bővítés meglévő informatikai infrastruktúra kiterjesztése hello nyilvános felhőbe jelöli. Ha kihasználja a létesítmények közötti kapcsolati lehetőségek lehetséges tootreat az Azure virtuális hálózatok, mint egy másik annak a helyszíni hálózati infrastruktúrát.

Azonban számos tervezési és tervezési szempontokat, amelyeket toobe címzett első. Ez különösen fontos hálózati biztonsági hello területén. Egy hogyan készíthető elő ilyen tervezési hello legjobb módjai toounderstand toosee egy példa az.

A Microsoft hello létrehozott [Datacenter bővítmény referencia architektúra diagramja](https://gallery.technet.microsoft.com/Datacenter-extension-687b1d84#content) és collateral toohelp támogató tisztában mi néz ki egy ilyen adatközpont kiterjesztése. Ez a példa hivatkozás megvalósítását, hogy tooplan használ, és biztonságos vállalati adatközpontban bővítmény toohello felhő tervezési biztosít. Azt javasoljuk, hogy tekintse át a dokumentum tooget hello kulcsfontosságú összetevők biztonságos megoldás képet.

toolearn hogyan toosecurely kiterjesztése az Azure, az Adatközpont kapcsolatos további információkért tekintse meg az hello videó [Your Datacenter kiterjesztése tooMicrosoft Azure](https://www.youtube.com/watch?v=Th1oQQCb2KA).
