---
title: "aaaAsymmetric útválasztási |} Microsoft Docs"
description: "Ez a cikk végigvezeti az ügyfél több hivatkozások tooa cél egy hálózatban aszimmetrikus útválasztással előfordulhat, hogy szembesülhetnek hello problémákat."
documentationcenter: na
services: expressroute
author: osamazia
manager: carmonm
editor: 
ms.assetid: a754bff9-95c9-44b5-9796-377fc21e8322
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/10/2016
ms.author: osamam
ms.openlocfilehash: 01a16242437a3674dcfe27b074911a829a6c1abd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="asymmetric-routing-with-multiple-network-paths"></a>Aszimmetrikus útválasztás több hálózati elérési úttal
A cikk leírja, hogy hogyan követhet a kimenő és a bejövő hálózati forgalom különböző utakat, amikor a hálózati forrás és cél között több elérési út is rendelkezésre áll.

Annak a két fontos toounderstand fogalmak toounderstand aszimmetrikus útválasztást. Egyik hello hatását, hogy több hálózati útvonalat. más hello hogyan eszközök, például egy tűzfal, tartsa állapotát. Az ilyen típusú eszközöket állapot-nyilvántartó eszközöknek nevezzük. Ez a két tényező kombinációja hoz létre a hálózati forgalom dobnak el egy állapotalapú eszköz mert hello állapot-nyilvántartó eszköz nem találta, hogy a forgalom maga hello eszközzel származnak forgatókönyvek.

## <a name="multiple-network-paths"></a>Több hálózati elérési út
Ha egy vállalati hálózat rendelkezik, csak az interneten keresztül az internetszolgáltatóval, hello Internet származó összes forgalmat tooand választ egy hivatkozás toohello hello ugyanazt az útvonalat. Gyakran a vállalatok beszerzési több körök, mint redundáns elérési utak, tooimprove hálózati üzemideje. Ha ez történik, az lehetséges, hogy a hello hálózaton, az Internet, toohello kívüli irányuló forgalom halad át egy hivatkozást, és hello vissza a forgalom halad át egy másik kapcsolat. Ezt a helyzetet nevezik közkeletűen aszimmetrikus útválasztásnak. Aszimmetrikus útválasztási fordított hálózati forgalom egy másik elérési utat hello eredeti adatfolyamban vesz igénybe.

![Több elérési úttal rendelkező hálózatok](./media/expressroute-asymmetric-routing/AsymmetricRouting3.png)

Bár ez elsősorban hello Internet történik, több elérési út tooother kombinációi aszimmetrikus útválasztási is vonatkozik. Vonatkozik, például tooan Internet elérési út és a egy belső elérési út, amely toohello ugyanarra a célgépre, és toomultiple titkos elérési út, amely toohello azonos céllal.

Minden hello módon, a forrás toodestination útválasztó kiszámítja hello legjobb elérési tooreach célt. hello útválasztó megállapíthatja, hogy a lehető legjobb útvonalat alapuló két fő tényezőket:

* A külső hálózatok közötti útválasztást az általában csak BGP néven emlegetett útválasztási protokoll, a peremátjáró protokoll (Border Gateway Protocol) határozza meg. BGP telik el szomszédok a hirdetmények, és végig a lépéseket toodetermine hello legjobb elérési szánt toohello cél futtatja őket. Hello legjobb útvonalat az útválasztási táblázatot tárolja.
* egy útvonal társított alhálózati maszk hosszának hello hogyan befolyásolja az útválasztási útvonalat. Ha útválasztója kap a több hirdetménnyel hello azonos IP-címet, de különböző alhálózati maszkokkal a hello útválasztó hello hirdetmény hosszabb alhálózati maszkkal inkább, mert egy pontosabb útvonal figyelembe.

## <a name="stateful-devices"></a>Állapot-nyilvántartó eszközök
Útválasztók tekintse meg a csomagok IP-fejlécének hello útválasztási célokra. Egyes eszközök is mélyebb hello csomagba keresse meg. Általában ezek az eszközök a 4. rétegbeli (Transmission Control Protocol, azaz TCP vagy User Datagram Protocol, azaz UDP), sőt akár a 7. rétegbeli (alkalmazási réteg) fejléceket vizsgálják. Ezek az eszközök általában biztonsági vagy sávszélesség-optimalizáló eszközök. 

Gyakori állapot-nyilvántartó eszközök például a tűzfalak. A tűzfal engedélyezi, vagy egy csomag toopass a különböző mezők például protokoll, az TCP/UDP-port és a URL-cím fejlécek alapján felületeken keresztül megtagadja. Ezt a magas fokú Csomagvizsgálat feldolgozási terhelésének hello eszközön nagy helyezi. tooimprove teljesítmény hello tűzfal hello első csomag folyamat megvizsgálja. Lehetővé teszi a hello csomagok tooproceed, ha az állapot tábla tartja hello folyamata adatokat. Az összes későbbi csomagok kapcsolódó toothis folyamata engedélyezettek hello kezdeti meghatározása alapján. Egy csomagot, amely egy meglévő folyamat része lehet, hogy hello tűzfal érkeznie. Ha hello tűzfalának nincs előzetes állapot arra vonatkozó információval, hello tűzfal megállítja az olyan hello csomagot.

## <a name="asymmetric-routing-with-expressroute"></a>Aszimmetrikus útválasztás az ExpressRoute-tal
Amikor tooMicrosoft Azure ExpressRoute keresztül csatlakozik, a hálózati módosítások módja:

* Lehetősége van több hivatkozások tooMicrosoft. Egy kapcsolat meglévő internetkapcsolat, és más hello ExpressRoute keresztül. Előfordulhat, hogy néhány forgalom tooMicrosoft hello interneten keresztül, de a térjen vissza a ExpressRoute keresztül, vagy fordítva.
* Az ExpressRoute konkrétabb IP-címeket küld. Igen a hálózati tooMicrosoft ExpressRoute keresztül felajánlott szolgáltatásokhoz érkező forgalom, az útválasztók mindig inkább ExpressRoute.

a hálózaton van két módosítások toounderstand hello hatás Mérlegeljük, bizonyos esetekben. Tegyük fel csak egy kör toohello Internet rendelkezik, és a felhasznált hello Internet minden Microsoft a szolgáltatásokat. a hálózati tooMicrosoft és vissza traverses hello forgalmát hello azonos Internet kapcsolat és fázisok hello tűzfalon keresztül. hello tűzfal rekordok hello folyamata, hello első csomag látja, és adja vissza, mert hello folyamata hello állapot tábla létezik csomagját.

![Aszimmetrikus útválasztás az ExpressRoute-tal](./media/expressroute-asymmetric-routing/AsymmetricRouting1.png)

Ezután bekapcsolja az ExpressRoute-ot, és a Microsoft-szolgáltatásokat az ExpressRoute-on keresztül veszi igénybe. A Microsoft egyéb szolgáltatások felhasznált hello interneten keresztül. A peremhálózaton, amely csatlakoztatott tooExpressRoute telepít egy külön tűzfal. Microsoft pontosabb előtagok tooyour hálózati ExpressRoute keresztül az adott szolgáltatások hirdeti. Az útválasztási infrastruktúrán úgy dönt, ExpressRoute hello ezen előtagok az elsődleges elérési útjával. Ha, amelyek nem forgalmaz hirdetményt a nyilvános IP-címek tooMicrosoft ExpressRoute keresztül, a Microsoft a nyilvános IP-címek hello interneten keresztül kommunikál. A hálózati tooMicrosoft a forgalom ExpressRoute, és fordított forgalmát a Microsoft hello Internet használja. Amikor hello tűzfal hello peremhálózaton az a folyamat, amely nem található az hello állapottáblázat válaszcsomagot látja, hello forgalom elutasítja azokat.

Ha úgy dönt, hogy azonos hálózati címfordítás (NAT) tárolókészlet ExpressRoute és az hello Internet toouse hello, láthatja hasonló problémák hello ügyfelekkel a magánhálózati IP-címét a hálózaton. A szolgáltatásokat, mint a Windows Update kérelmek tud hello interneten keresztül, mert a IP-címek a szolgáltatások hirdetése ExpressRoute keresztül nem történik. Azonban hello forgalom ismét elérhető lesz az ExpressRoute keresztül. Ha a Microsoft az IP-címet kap hello azonos hello Internet az alhálózati maszk és ExpressRoute, ExpressRoute szívesebben hello interneten keresztül. Ha egy tűzfal vagy egy másik állapot-nyilvántartó eszköz, amely a hálózat széle és az ExpressRoute irányuló a hello folyamata nem előzetes információ, hello csomagok toothat folyamata tartozó elutasítja azokat.

## <a name="asymmetric-routing-solutions"></a>Megoldások az aszimmetrikus útválasztásra
Két fő lehetőség közül választhat toosolve hello probléma aszimmetrikus útválasztási rendelkezik. Egy útválasztási keresztül van, és más hello forrás-alapú hálózati Címfordítás (SNAT) használatával.

### <a name="routing"></a>Útválasztás
Győződjön meg arról, hogy a nyilvános IP-címek hirdetett tooappropriate nagy kiterjedésű hálózati (WAN) kapcsolatok. Például ha szeretne toouse hello internetes hitelesítési forgalmat és ExpressRoute az e-mail forgalom, meg kell hirdetményt az Active Directory összevonási szolgáltatások (AD FS) nyilvános IP-címek ExpressRoute keresztül. Hasonlóképpen, győződjön meg nem tooexpose egy helyszíni AD FS kiszolgálói tooIP címeket útválasztó hello fogad ExpressRoute keresztül. Útvonalak fogadása történik az ExpressRoute több vonatkoznak, így az általuk végrehajtott ExpressRoute hello elérési útnak a hitelesítési forgalom tooMicrosoft. Ez aszimmetrikus útválasztáshoz vezet.

Ha toouse ExpressRoute hitelesítéshez, győződjön meg arról, hogy hirdetési AD FS nyilvános IP-címek keresztül ExpressRoute nélkül hálózati címfordítást. Ezzel a módszerrel a Microsofttól származnak, és tooan kerül forgalom a helyszíni AD FS-kiszolgálón keresztül ExpressRoute kerül. Ügyfél tooMicrosoft visszatérési forgalmát ExpressRoute használja, mert az előnyben részesített útvonal hello hello interneten keresztül.

### <a name="source-based-nat"></a>Forrásalapú NAT
Az aszimmetrikus útválasztás által okozott problémák megoldásának másik módját az SNAT használata jelenti. Például akkor rendelkezik nem meghirdetett hello nyilvános IP-cím egy helyszíni Simple Mail Transfer Protocol (SMTP) kiszolgáló ExpressRoute keresztül, mert azt tervezi, hogy az ilyen típusú kommunikáció Internet toouse hello. A kérelmeket, amelyek a Microsoft származik, és megnyitja tooyour helyszíni SMTP-kiszolgáló internetes hello halad át. Ön SNAT hello bejövő kérelem tooan belső IP-cím. Hello SMTP-kiszolgáló fordított forgalmát toohello peremhálózati tűzfal (amely a NAT számára) helyett ExpressRoute keresztül kerül. hello forgalom Visszalépés hello interneten keresztül.

![A forrásalapú NAT hálózati konfigurációja](./media/expressroute-asymmetric-routing/AsymmetricRouting2.png)

## <a name="asymmetric-routing-detection"></a>Az aszimmetrikus útválasztás észlelése
Útválasztás nyomkövetése hello legjobb módja toomake meg arról, hogy a hálózati forgalom áthaladó hello várt elérési út. Ha a várt forgalom a helyszíni SMTP server tooMicrosoft tootake hello Internet elérési útról, hello várt traceroute a hello SMTP server tooOffice 365. hello eredményt ellenőrzi, hogy forgalom valóban hagyja el a hálózaton hello Internet felé, és nem ExpressRoute felé.

