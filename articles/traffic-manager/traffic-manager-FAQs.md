---
title: "a Traffic Manager - aaaAzure – gyakori kérdések |} Microsoft Docs"
description: "Ez a cikk ismerteti a válaszok kérdések a Traffic Manager toofrequently"
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: 75d5ff9a-f4b9-4b05-af32-700e7bdfea5a
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/15/2017
ms.author: kumud
ms.openlocfilehash: 5530d03b55bbf49a52002841e281e2cf5819c2b8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="traffic-manager-frequently-asked-questions-faq"></a>A TRAFFIC Manager gyakori kérdések (GYIK)

## <a name="traffic-manager-basics"></a>A TRAFFIC Manager alapjai

### <a name="what-ip-address-does-traffic-manager-use"></a>Milyen IP-címet használ a Traffic Manager?

A [Traffic Manager működése](../traffic-manager/traffic-manager-overview.md#how-traffic-manager-works), a Traffic Manager DNS szint hello működik. DNS-válaszok toodirect ügyfelek toohello megfelelő szolgáltatás elküldi végpont. Az ügyfelek ezután csatlakoznak toohello szolgáltatásvégpont közvetlenül, nem Traffic Manager használatával.

Ezért a Traffic Manager nem ad meg egy végpont vagy IP-címe az ügyfelek tooconnect. Ezért ha a szolgáltatás szeretne statikus IP-cím, amely is be kell állítani hello szolgáltatást, nem a Traffic Manager.

### <a name="does-traffic-manager-support-sticky-sessions"></a>Támogatja a Traffic Manager "kapcsolódó" munkamenetek?

A [Traffic Manager működése](../traffic-manager/traffic-manager-overview.md#how-traffic-manager-works), a Traffic Manager DNS szint hello működik. DNS-válaszok toodirect ügyfelek toohello megfelelő szolgáltatási végpont használ. Az ügyfelek kapcsolódnak toohello szolgáltatásvégpont közvetlenül, nem Traffic Manager használatával. A Traffic Manager, ezért nem látja a hello HTTP-forgalom hello ügyfél és hello kiszolgáló között.

Emellett a hello forrás IP-címe hello DNS-lekérdezés Traffic Manager által fogadott toohello rekurzív DNS-szolgáltatás, nem hello ügyfél tartozik. Ezért a Traffic Manager módon tootrack egyes ügyfelek nem rendelkezik, és "kapcsolódó" munkamenetek nem valósítható meg. Ez a korlátozás közös tooall DNS-alapú forgalom felügyeleti rendszerekkel, és nincs meghatározott tooTraffic Manager.

### <a name="why-am-i-seeing-an-http-error-when-using-traffic-manager"></a>Miért jelenik meg egy HTTP-hiba a Traffic Manager használata esetén?

A [Traffic Manager működése](../traffic-manager/traffic-manager-overview.md#how-traffic-manager-works), a Traffic Manager DNS szint hello működik. DNS-válaszok toodirect ügyfelek toohello megfelelő szolgáltatási végpont használ. Az ügyfelek ezután csatlakoznak toohello szolgáltatásvégpont közvetlenül, nem Traffic Manager használatával. A TRAFFIC Manager funkciója nem lásd: HTTP-forgalom ügyfél és kiszolgáló között. Ezért a HTTP-hibát látja kell hamarosan az alkalmazásból. Hello ügyfélalkalmazáshoz tooconnect toohello minden DNS-feloldási lépést teljesülnek. Beavatkozás, amely a Traffic Manager hello alkalmazás adatforgalmat rendelkezik, amely tartalmazza.

További vizsgálat ezért kell összpontosítania hello alkalmazás.

hello ügyfél böngészője által küldött hello HTTP állomásfejléc hello leggyakoribb problémák forrását. Győződjön meg arról, hogy hello alkalmazás konfigurált tooaccept hello megfelelő állomásfejlécét hello tartománynevet használja. Végpontjaitól hello Azure App Service szolgáltatásban, lásd: [egy webalkalmazást az egyéni tartománynév konfigurálása az Azure App Service szolgáltatásban a Traffic Managerrel](../app-service-web/web-sites-traffic-manager-custom-domain-name.md).

### <a name="what-is-hello-performance-impact-of-using-traffic-manager"></a>Mi az a hello teljesítményre gyakorolt hatását a Traffic Manager használatával?

A [Traffic Manager működése](../traffic-manager/traffic-manager-overview.md#how-traffic-manager-works), a Traffic Manager DNS szint hello működik. Az ügyfelek kapcsolódnak tooyour Szolgáltatásvégpontok közvetlenül, mert nincs a Traffic Manager használatakor, miután létrejött a kapcsolat hello felmerülő teljesítmény hatással.

A Traffic Manager DNS szint hello alkalmazásokat integrálható, mert egy további DNS keresési toobe hello DNS-feloldási lánc beszúrt legyen szükség. hello gyakorolt hatása a Traffic Manager DNS-feloldási idő minimális. A TRAFFIC Manager névkiszolgálók globális hálózata, és használja a [nem egyedi](https://en.wikipedia.org/wiki/Anycast) hálózati tooensure DNS-lekérdezéseket a rendszer mindig irányított toohello legközelebbi elérhető névkiszolgálót. Ezenkívül DNS-válaszok gyorsítótárazásának azt jelenti, hogy hello további DNS késését a Traffic Manager használatával alkalmazza a munkamenetek csak tooa hányadát.

hello teljesítmény metódus útvonalak forgalom toohello legközelebbi elérhető végpontot. hello nettó eredménye, hogy hello társított ezt a módszert, általános teljesítményre gyakorolt hatás minimális kell lennie. A DNS-késés növekedése a hálózati késés toohello végpont alacsonyabb lesz eltolva.

### <a name="what-application-protocols-can-i-use-with-traffic-manager"></a>Milyen alkalmazás protokollok használhatók a Traffic Managerrel?

A [Traffic Manager működése](../traffic-manager/traffic-manager-overview.md#how-traffic-manager-works), a Traffic Manager DNS szint hello működik. Ha hello DNS-keresés befejeződött, az ügyfelek kapcsolódnak toohello alkalmazás végpontjának közvetlenül, nem Traffic Manager használatával. Ezért a hello kapcsolat bármely alkalmazás protokoll használható. Ha TCP, figyelési protokoll, a Traffic Manager hello végpont állapotfigyelés állásideje nélkül végezhetők el bármely alkalmazás protokollok használatával. Ha úgy dönt, hogy toohave hello állapotának ellenőrzése az alkalmazás protokollal, hello végponthoz toobe képes toorespond tooeither HTTP vagy HTTPS GET kérelmek.

### <a name="can-i-use-traffic-manager-with-a-naked-domain-name"></a>Használható a Traffic Manager "csupasz" tartománynévvel?

Nem. hello DNS-szabványokból nem teszik lehetővé a CNAME tooco-létezhetnek más DNS-rekordjait hello azonos nevet. hello legfelső pontján (vagy a főtanúsítvány) a DNS-zónák mindig tartalmazza a már meglévő DNS-rekordok két; hello SOA és hello mérvadó Névkiszolgálói rekordokat. Ez azt jelenti, hogy a CNAME rekord nem hozható létre hello zóna felső pontja hello DNS-szabványokból megsértése nélkül.

A TRAFFIC Manager DNS CNAME rekord toomap hello kreatív DNS-név szükséges. Például www.contoso.com toohello Traffic Manager profil DNS nevét contoso.trafficmanager.net rendelni. Emellett a hello Traffic Manager-profil egy második, melyik végponthoz hello ügyfélnek csatlakoznia kell a DNS CNAME REKORDJÁNAK tooindicate adja vissza.

a probléma megoldásához toowork, azt javasoljuk, egy HTTP-átirányítási toodirect hello csupasz tartomány tooa másik URL-CÍMÉT, amely ezután használhatja a Traffic Manager érkező forgalmat. Például hello csupasz "contoso.com" tartomány átirányíthatók felhasználók toohello CNAME "www.contoso.com" mutat, toohello Traffic Manager DNS-nevét.

Teljes támogatást nyújt a Traffic Manager csupasz tartományok zajlik, a szolgáltatás várakozó fájlok számát a. A támogatási kérelem által a szolgáltatás regisztrálhatja [a közösségi visszajelzés helyen szavazott](https://feedback.azure.com/forums/217313-networking/suggestions/5485350-support-apex-naked-domains-more-seamlessly).

### <a name="does-traffic-manager-consider-hello-client-subnet-address-when-handling-dns-queries"></a>Nem a Traffic Manager megfontolandó hello ügyfél alhálózati cím DNS-lekérdezések kezelése? 
Nem, jelenleg a Traffic Manager úgy ítéli meg, csak hello forrás IP-címe hello DNS-lekérdezést kap, amely általában hello IP-címe hello DNS-feloldási, Geographic és teljesítmény útválasztási metódusait végrehajtott keresési műveletek végrehajtásakor.  
Pontosabban [RFC 7871 – ügyfél alhálózati a DNS-lekérdezések](https://tools.ietf.org/html/rfc7871) biztosít egy [bővítmény mechanizmus a DNS (EDNS0)](https://tools.ietf.org/html/rfc2671) az azt támogató tooDNS kiszolgálók feloldókat hello ügyfél alhálózati cím át is van jelenleg nem támogatott a Traffic Manager. Ez a szolgáltatás kérelmet a támogatása regisztrálhatja a [közösségi visszajelzési webhelyet](https://feedback.azure.com/forums/217313-networking).


### <a name="what-is-dns-ttl-and-how-does-it-impact-my-users"></a>DNS-élettartam és mi az a hatása a felhasználók?

Ha egy DNS-lekérdezést a Traffic Manager fájljai, idő-Élettartam (TTL) nevű hello válaszul megad egy értéket. Ez az érték, amelynek egység másodpercen belül van, tooDNS feloldókat után mennyi ideig toocache a Ez a válasz azt jelzi. Amíg a DNS-feloldókat nem garantált toocache az eredménye, gyorsítótárazás lehetővé teszi, toorespond tooany lekérdezések hello gyorsítótár tooTraffic Manager DNS-kiszolgálók megkerülve ki. Ez hatással van hello válaszok az alábbiak szerint:
- magasabb TTL hello lekérdezések száma, amelyek a hello megnyílik a Traffic Manager DNS-kiszolgálók, amelyek csökkentheti hello költség ügyfél mivel kiszolgált lekérdezések számlázható használati csökkenti.
- magasabb TTL potenciálisan csökkentheti a DNS-címkeresés szükséges toodo hello idő.
- magasabb TTL is azt jelenti, hogy, hogy az adatok nem tükrözi hello legújabb állapottal kapcsolatos adatok, amelyek a Traffic Manager megkapta a vizsgálathoz használt ügynökök keresztül.

### <a name="how-high-or-low-can-i-set-hello-ttl-for-traffic-manager-responses"></a>Hogyan magas vagy alacsony lehet hello élettartam a Traffic Manager-válaszok beállítani?

Beállíthatja, egy profil szintenként hello 0 másodpercig alacsony és magas, mint 2 147 483 647 másodperc, a DNS-élettartam toobe (megfelelnek a tartomány maximális hello [RFC-1035 szabványnak megfelelően](https://www.ietf.org/rfc/rfc1035.txt )). A 0 azt jelenti, hogy az alárendelt DNS-feloldókat nem gyorsítótárazzák a válaszokban, és az összes lekérdezés TTL várt tooreach hello Traffic Manager DNS-kiszolgálók a feloldásához.

## <a name="traffic-manager-geographic-traffic-routing-method"></a>A TRAFFIC Manager Geographic forgalom-útválasztási módszert

### <a name="what-are-some-use-cases-where-geographic-routing-is-useful"></a>Néhány alkalmazási helyzetét, ahol földrajzi útválasztási akkor hasznos, melyek? 
Ha egy Azure ügyféligények toodistinguish földrajzi régió alapján a felhasználók földrajzi útválasztási típus használható egyik szituációban. Például hello földrajzi forgalom-útválasztási módszert használ, engedélyezheti a felhasználók számára az adott régióban el más régiókból mint egy másik felhasználói élményt. Egy másik példa megfelel a helyi adatok közös joghatóság alá megbízás szükséges, hogy a felhasználók egy adott régióban tudja megjeleníteni csak az adott régióban végpontok.

### <a name="what-are-hello-regions-that-are-supported-by-traffic-manager-for-geographic-routing"></a>Mik azok a Traffic Manager által támogatott földrajzi irányításához hello régiók? 
Traffic Manager által használt hello ország vagy régió hierarchiában található [Itt](traffic-manager-geographic-regions.md). Ezen a lapon módosításai naprakészen szerepeljenek tartják, amíg szoftveresen is le ugyanazokat az információkat hello hello segítségével [Azure Traffic Manager REST API](https://docs.microsoft.com/rest/api/trafficmanager/). 

### <a name="how-does-traffic-manager-determine-where-a-user-is-querying-from"></a>Hogyan nem a traffic manager meg, ha a felhasználó kérdezi le, a? 
A TRAFFIC Manager hello forrás IP-címe (Ez a legvalószínűbb ok az a helyi DNS-feloldási hello hello felhasználó nevében lekérdezése során) hello lekérdezés megvizsgálja, és egy belső IP tooregion térkép toodetermine hello helyet használja. Ez a térkép frissül, a hello változásokat egy folyamatosan tooaccount internet. 

### <a name="is-it-guaranteed-that-traffic-manager-can-correctly-determine-hello-exact-geographic-location-of-hello-user-in-every-case"></a>Ez garantálja, hogy a Traffic Manager is meg megfelelően hello pontos földrajzi helye hello felhasználó minden esetben?
Nem, a Traffic Manager nem garantálható, hogy azt az IP-forráscím hello DNS-lekérdezés következtethető ki hello földrajzi régió mindig felelnek meg toohello felhasználó helye miatt toohello következő okok miatt: 

- Első lépésként hello előző gyakran ismételt kérdések leírtak hello forrás IP-cím látható, hogy a DNS-feloldási módon hello keresési hello felhasználó nevében. Jó proxyjaként való hello hello felhasználó földrajzi helye pedig a földrajzi helye hello hello DNS-feloldóját is lehet hello erőforrásigényét hello DNS-feloldó szolgáltatás és hello adott DNS-feloldó szolgáltatás úgy döntött, az ügyfél másik függően toouse. Tegyük fel az ügyfél malajziai található kell megadni az eszköz beállításait használja a DNS-feloldó szolgáltatás juthat, amelynek DNS-kiszolgáló szingapúri kivételezett toohandle hello lekérdezés megoldások, hogy a felhasználó vagy eszköz. Abban az esetben a Traffic Manager csak látható hello feloldó IP-cím, amelynek megfelelő toohello szingapúri helyét. További tájékoztatás a hello ügyfél alhálózati címmel kapcsolatos korábbi GYIK támogatja az ezen a lapon.

- A Traffic Manager második, egy belső térkép toodo hello IP-toogeographic régió címfordítással használja. Amíg ez a leképezés érvényesítése és frissíteni az olyan folyamatosan tooincrease pontosságát és folyamatosan változó jellege hello fiók hello internet, továbbra is fennáll, hogy az információ nincs hello földrajzi hely, az összes pontos másolatát hello lehetősége hello IP-címeket.


###  <a name="does-an-endpoint-need-toobe-physically-located-in-hello-same-region-as-hello-one-it-is-configured-with-for-geographic-routing"></a>Nem olyan végpont szükséges toobe fizikailag található hello hello és ugyanabban a régióban egy földrajzi útválasztásra van konfigurálva a? 
Nem, hello végpont hello helyét korlátozza a dokumentumkulcsban nem amelyen régiók csatlakoztatott tooit is lehet. Például az Egyesült Államok középső Azure azon régióját végpont lehet vezérelt India tooit minden felhasználó számára.

### <a name="can-i-assign-geographic-regions-tooendpoints-in-a-profile-that-is-not-configured-toodo-geographic-routing"></a>Is hozzárendelek a földrajzi régiók tooendpoints, amely nem profilban konfigurált toodo földrajzi útválasztási? 

Igen, ha hello útválasztási módszer profil nincs földrajzi, az hello [Azure Traffic Manager REST API](https://docs.microsoft.com/rest/api/trafficmanager/) tooassign földrajzi régiókhoz tooendpoints az adott profilhoz. Nem földrajzi útválasztási típus profilra hello esetben figyelmen kívül ezt a konfigurációt. Ha ilyen egy profil toogeographic útválasztási típusát módosítja egy későbbi időpontban, Traffic Manager használja ezeket a leképezéseket.


### <a name="why-am-i-getting-an-error-when-i-try-toochange-hello-routing-method-of-an-existing-profile-toogeographic"></a>Miért jelenik hiba jelenik meg, hogy toochange hello útválasztási módszer egy meglévő profil tooGeographic?

A földrajzi útválasztási profil végpontjai hello toohave kell legalább egy régió tooit leképezve. egy már meglévő profil toogeographic útválasztási típus tooconvert, először tooassociate földrajzi régiókhoz tooall hello segítségével végpontját [Azure Traffic Manager REST API](https://docs.microsoft.com/rest/api/trafficmanager/) írja toogeographic útválasztási hello módosítása előtt. Ha portált használja, először törölje az hello végpontok, hello útválasztási mód hello-profil toogeographic módosítása, és adja hozzá az hello végpontok együtt a földrajzi régió leképezése. 


###  <a name="why-is-it-strongly-recommended-that-customers-create-nested-profiles-instead-of-endpoints-under-a-profile-with-geographic-routing-enabled"></a>Miért érdemes az erősen ajánlott, ha az ügyfelek beágyazott profilok olyan profilban végpontok helyett hozzon létre a földrajzi engedélyezett útválasztással? 

A régió tooonly egy végpont egy profilon belül rendelhető, ha a földrajzi útválasztási típus használatával. Ha adott végpont nem egy gyermek csatolt profil tooit, a beágyazott típus Ha végpontra is nem megfelelő, a Traffic Manager óta nem küldött minden forgalom nem minden jobb hello alternatív toosend forgalom tooit továbbra is. A TRAFFIC Manager elvégzi a nem feladatátvételi tooanother végpont, még akkor is, amikor a hello régió "szülőeleme" hello régió hozzárendelt merült fel a nem megfelelő (Ha például régió Spanyolország rendelkező végpont kerül sérült állapotba, fedi le nem feladatátvételi tooanother toohello végpontot hello régió Európa rendelkező végpont hozzárendelt tooit). Ebben az esetben tooensure, amelyek a Traffic Manager tekintetben hello földrajzi határokat, amely az ügyfél rendelkezik a profilban. tooget hello előnye, hogy a végpont kerül sérült állapotba, ha tooanother végpontra futtatása sikertelen, javasoljuk, hogy a földrajzi régióban kell-e hozzárendelve a toonested profilokat az egyes végpontok helyett belül több végpontot. Így ha a beágyazott hello gyermek profil sikertelen lesz, forgalom végpont is feladatátvételi tooanother végpont hello belül azonos beágyazott gyermek profil.

### <a name="are-there-any-restrictions-on-hello-api-version-that-supports-this-routing-type"></a>Vannak-e hello API-verziója támogatja a útválasztási típus korlátozásai?

Igen, csak az API-verzió 2017-03-01 és újabb támogatja hello földrajzi útválasztás típusa. Bármely régebbi kiadását API nem használt toocreated profilok földrajzi útválasztási típus lehet, és rendelje hozzá a földrajzi régiók tooendpoints. Ha egy régebbi API-verzió az Azure-előfizetésből használt tooretrieve profilok, a profil földrajzi útválasztási típusú nem jelennek meg. Emellett régebbi API-verziók használata esetén olyan profilt, amely a földrajzi régióban hozzárendelésekor végpontok, akkor nem rendelkezik a földrajzi régióban hozzárendelése látható adott vissza.



## <a name="traffic-manager-endpoints"></a>Traffic Manager-végpontok

### <a name="can-i-use-traffic-manager-with-endpoints-from-multiple-subscriptions"></a>Használhatom a Traffic Manager végpontok több előfizetést?

Több előfizetés végpontok használata nem lehetséges az Azure Web Apps. Az Azure Web Apps megköveteli, hogy bármely a webalkalmazásokkal használt egyéni tartománynév csak egyetlen előfizetéssel belül használják. Nincs több előfizetéssel, valamint hello lehetséges toouse webes alkalmazások ugyanazon a néven.

Más típusú végpontok esetében, a lehetséges toouse Traffic Manager végpontokon egynél több előfizetés esetén. Az erőforrás-kezelőben a bármely előfizetés is lehet végpontokat hozzáadni tooTraffic-kezelő mindaddig, amíg hello Traffic Manager-profil konfigurálása hello személynek van olvasási hozzáférési toohello végpont. Ezek az engedélyek segítségével engedélyezhetők [Azure Resource Manager szerepköralapú hozzáférés-vezérlést (RBAC)](../active-directory/role-based-access-control-configure.md).


### <a name="can-i-use-traffic-manager-with-cloud-service-staging-slots"></a>Használhatom a Traffic Manager Felhőszolgáltatás "Átmeneti" üzembe helyezési ponti?

Igen. A felhőalapú szolgáltatás "átmeneti" üzembe helyezési ponti konfigurálhatók a Traffic Manager külső végpontok száma. Állapot-ellenőrzési eredményeire továbbra is van szó, hello Azure-végpontok díj. Hello típusú külső végpont használatban van, mert módosítások toohello mögöttes szolgáltatás nem átveszik automatikusan. Külső végpontok száma, a Traffic Manager nem tudja észlelni az hello felhőalapú szolgáltatás leállítása vagy törlése. Ezért hello Traffic Manager továbbra is fennáll az állapot-ellenőrzési eredményeire számlázási amíg hello végpont letiltott vagy törölt.

### <a name="does-traffic-manager-support-ipv6-endpoints"></a>A Traffic Manager támogatja az IPv6-végpontot?

A TRAFFIC Manager jelenleg nem biztosít IPv6-addressible névkiszolgálók. Azonban a Traffic Manager továbbra is használhatják tooIPv6 végpontok csatlakozó IPv6-ügyfeleket. A ügyfelek nem tudnak közvetlenül tooTraffic Manager kérelmek DNS. Ehelyett hello ügyfél használja egy rekurzív DNS-szolgáltatás. Egy csak IPv6-alapú ügyfél kérelmeket küld toohello rekurzív DNS-szolgáltatás IPv6. Majd hello rekurzív szolgáltatás képes toocontact hello Traffic Manager névkiszolgálók használatával az IPv4-alapú kell lennie.

A TRAFFIC Manager válaszol, hello végpont hello DNS-nevét. toosupport IPv6 végpont, a DNS AAAA rögzítse mutató hello végpont DNS nevét toohello IPv6-címnek már léteznie kell. A TRAFFIC Manager állapotellenőrzést csak támogatja az IPv4-címeket. hello szolgáltatást kell hello tooexpose egy IPv4-végpont azonos DNS-nevét.

### <a name="can-i-use-traffic-manager-with-more-than-one-web-app-in-hello-same-region"></a>Használhatok Traffic Manager hello egynél több webalkalmazás ugyanabban a régióban?

A Traffic Manager általában használt toodirect forgalom tooapplications különböző régiókban telepítve. Azonban azt is használható Ha egy alkalmazás több központi telepítési rendelkezik hello ugyanabban a régióban. hello Azure Traffic Manager-végpont nem teszik több webalkalmazás-végpont a hello azonos Azure-régiót toobe hozzáadott toohello ugyanazt a Traffic Manager-profilt.

##  <a name="traffic-manager-endpoint-monitoring"></a>A TRAFFIC Manager-végpont figyelése

### <a name="is-traffic-manager-resilient-tooazure-region-failures"></a>A Traffic Manager rugalmas tooAzure régió hibák van?

A TRAFFIC Manager hello kézbesítését az Azure-ban magas rendelkezésre állású alkalmazások nyilvános kulcsokra épülő.
toodeliver magas rendelkezésre állás, a Traffic Manager kell egy rendkívül magas szintű rendelkezésre állási és rugalmas tooregional sikertelen lehet.

Úgy lett kialakítva a Traffic Manager összetevői rugalmas tooa befejezése sikertelen volt-e bármely Azure-régiót. A rugalmasság tooall Traffic Manager-összetevők vonatkozik: hello DNS névkiszolgálóit, hello API, hello tárolási réteg és hello végpont szolgáltatás figyelése.

Hello esetére kimaradás lép fel egy teljes Azure-régió, a Traffic Manager várt toocontinue toofunction általános az. A több Azure-régiók telepített alkalmazások támaszkodhat a Traffic Manager toodirect forgalom tooan rendelkezésre álló példányát az alkalmazásokban.

### <a name="how-does-hello-choice-of-resource-group-location-affect-traffic-manager"></a>Hogyan befolyásolja a hello választás az erőforráscsoport helye a Traffic Manager?

A TRAFFIC Manager szolgáltatás egyetlen globális szolgáltatás. Nincs regionális. hello választás az erőforráscsoport helye nem különbség tooTraffic kezelő profil telepítve az erőforráscsoport teszi.

Az Azure Resource Manager az összes erőforrás csoportok toospecify egy helyet, amely megadja, hogy az erőforráscsoport üzembe helyezett erőforrás hello alapértelmezett helye van szükség. Traffic Manager-profil létrehozásakor az erőforráscsoportban jön létre. Traffic Manager-profilokat használja **globális** az helyeként, hello erőforrás csoport alapértelmezett felülbírálása.

### <a name="how-do-i-determine-hello-current-health-of-each-endpoint"></a>Hogyan állapítható meg az egyes végpontok hello aktuális állapotát?

hello aktuális végpontok állapotának figyelését, továbbá toohello teljes profil jelenik meg hello Azure-portálon. Ez az információ is megtalálható hello forgalmat figyelő [REST API](https://msdn.microsoft.com/library/azure/mt163667.aspx), [PowerShell-parancsmagok](https://msdn.microsoft.com/library/mt125941.aspx), és [platformfüggetlen Azure CLI](../cli-install-nodejs.md).

Azure nem biztosít múltbeli végpont vonatkozó előzményadatok állapotfigyelő vagy hello képességét tooraise kapcsolatos riasztások módosítások tooendpoint állapotát.

### <a name="can-i-monitor-https-endpoints"></a>Figyelheti a HTTPS-végpontnak?

Igen. A TRAFFIC Manager támogatja a HTTPS-KAPCSOLATON keresztül számlálása. Konfigurálása **HTTPS** hello figyelési konfiguráció hello protokollként.

A TRAFFIC manager nem tud biztosítani a leellenőrizni a tanúsítvány, beleértve:

* Kiszolgálóoldali tanúsítványokat a rendszer nem érvényesíti
* Az SNI kiszolgálóoldali tanúsítványok nem támogatottak.
* Ügyfél-tanúsítványok használata nem támogatott

### <a name="can-i-use-traffic-manager-even-if-my-application-does-not-have-support-for-http-or-https"></a>Használhatom a Traffic Manager, még akkor is, ha az alkalmazás nem rendelkezik támogatja a HTTP vagy HTTPS?

Igen. TCP protokollt és a Traffic Manager figyelési hello TCP-kapcsolatot kezdeményez, és hello végpont válaszára Várakozás adhatja meg. Ha hello végpont toohello kapcsolódási kérelem és válasz tooestablish hello kapcsolattal ügyfélkéréseire válaszol, hello időkorláton belül időszak, akkor az adott végpontra van megjelölve kifogástalan állapotúként.

### <a name="what-specific-responses-are-required-from-hello-endpoint-when-using-tcp-monitoring"></a>Milyen adott válaszok szükségesek a hello végpont, ha a TCP-figyelés használatával?

TCP-figyelési használata esetén a Traffic Manager elindul egy háromutas TCP kézfogás egy SZIN küldött kérelmek tooendpoint: hello megadott port. Majd megvárja, egy adott időn belül (ahogy a hello időtúllépés beállítása) hello végpont válaszára. Ha hello végpont válaszol toohello SZIN kérelem SZIN-Nyugtázási választ hello figyelési beállítások hello megadott időkorláton belül, akkor kifogástalan tekinti a végpontot. Ha hello SZIN-ACK-válasz érkezett, hello Traffic Manager hello kapcsolat által vissza egy RST válaszol alaphelyzetbe állítása.

### <a name="how-fast-does-traffic-manager-move-my-users-away-from-an-unhealthy-endpoint"></a>Milyen gyors helyezze át a Traffic Manager elhagyja a nem megfelelő végpont a felhasználók?

A TRAFFIC Manager biztosít több beállítások, melyek segíthetnek toocontrol hello feladatátvételt Traffic Manager-profil a következőképpen:
- megadhatja, hogy a hello Traffic Manager mintavételt hello végpontok gyakrabban beállításával Probing időköz hello: 10 másodperc. Ez biztosítja, hogy a végpont nem kifogástalan állapotra minél hamarabb észlelte. 
- megadhatja, hogy mennyi ideig toowait előtt állapotának ellenőrzése kérelem időtúllépése (minimális időtúllépési érték 5 másodperc).
- megadhatja, hogy hány hiba akkor fordulhat elő, mielőtt hello végpont megfelelő állapotúként van megjelölve. Ez az érték alacsony, mint 0, melyik eset hello végponthoz van jelölve, amint hello nem sikerül az első állapotának ellenőrzése. Azonban hello minimális értéke 0 használata hello megengedett számú sikertelen eredményezhet kívül Elforgatás probing hello során esetlegesen előforduló tooany átmeneti hibái miatt elérhetetlenné tooendpoints.
- megadhat hello DNS válasz toobe 0 a lehető hello-élettartama (TTL). Ennek során, így azt jelenti, hogy a DNS-feloldókat nem gyorsítótárazza a hello választ, és minden új lekérdezés lekérdezi egy választ, amely magában foglalja a legnaprakészebb állapotadatokat hello adott hello Traffic Manager.

Ezekkel a beállításokkal a Traffic Manager biztosíthat a feladatátvételeket a 10 másodperc után a végpont kerül sérült állapotba, és egy DNS-lekérdezés megfelelő hello-profil alapján történik.

### <a name="how-can-i-specify-different-monitoring-settings-for-different-endpoints-in-a-profile"></a>Hogyan adhat meg egy másik végpontok különböző monitoringbeállításokat?

A TRAFFIC Manager figyelési beállítások erővel egy profil szintenként. Ha csak egy végpont toouse figyelési beállítás van szüksége, azt is megteheti azzal, hogy az adott végpontra, mint egy [profil beágyazott](traffic-manager-nested-profiles.md) hello szülő profil amelynek figyelési beállítások eltérnek.

### <a name="what-host-header-do-endpoint-health-checks-use"></a>Milyen host fejléc nincs végpont ellenőrzi használata?

A TRAFFIC Manager állomásfejléc használja a HTTP és HTTPS állapotellenőrzést. Traffic Manager által használt hello állomásfejléc hello végpont cél hello profilban konfigurált hello neve. hello állomásfejléc használt hello érték nem adható meg külön-külön a hello target tulajdonság.

### <a name="what-are-hello-ip-addresses-from-which-hello-health-checks-originate"></a>Mik azok a hello IP-címek, amelyből hello ellenőrzést származik?

hello alábbi hello IP-cím szerepel, amelyből a Traffic Manager ellenőrzést is származik. A lista tooensure használhatja, hogy ezek IP-címekről érkező bejövő kapcsolatok engedélyezve legyenek hello végpontok toocheck, az állapotadatok.

* 40.68.30.66
* 40.68.31.178
* 137.135.80.149
* 137.135.82.249
* 23.96.236.252
* 65.52.217.19
* 40.87.147.10
* 40.87.151.34
* 13.75.124.254
* 13.75.127.63
* 52.172.155.168
* 52.172.158.37
* 104.215.91.84
* 13.75.153.124
* 13.84.222.37
* 23.101.191.199
* 23.96.213.12
* 137.135.46.163
* 137.135.47.215
* 191.232.208.52
* 191.232.214.62
* 13.75.152.253
* 104.41.187.209
* 104.41.190.203

### <a name="how-many-health-checks-toomy-endpoint-can-i-expect-from-traffic-manager"></a>Hány egészségügyi ellenőrzések toomy végpont várhatók a Traffic Manager?

Traffic Manager egészségügyi hello számát ellenőrzi, hogy elérte volna a végpont hello következő függ:
- érték, amely hello figyelési időköz beállítása hello (kisebb időköz azt jelenti, hogy a végpont a megadott időtartamon üzenetsorokra további kérelmeket).
- a helyek a ahol hello állapotellenőrzést származnak (hello IP-címek is várt ezen ellenőrzések szerepel hello megelőző – gyakori kérdések) hello maximális számát.

## <a name="traffic-manager-nested-profiles"></a>A TRAFFIC Manager beágyazott profilok

### <a name="how-do-i-configure-nested-profiles"></a>Hogyan konfigurálhatók a beágyazott profilok?

Beágyazott Traffic Manager-profilok mindkét hello Azure Resource Manager használatával konfigurálható, és a klasszikus Azure REST API-k, Azure PowerShell-parancsmagok és platformfüggetlen Azure parancssori felület parancsait hello. Támogatottak továbbá akkor hello új Azure-portálon keresztül. A klasszikus portálon hello azok nem támogatottak.

### <a name="how-many-layers-of-nesting-does-traffic-manger-support"></a>Hány rétegének beágyazási biztosítja Traffic Manager támogatja?

Ágyazhatja be too10 szintnél mélyebb profilok. "A hurokban" nem engedélyezettek.

### <a name="can-i-mix-other-endpoint-types-with-nested-child-profiles-in-hello-same-traffic-manager-profile"></a>Kombinálhatom a más típusú végpontok esetében az egymásba ágyazott alárendelt profiljaival, a hello ugyanazt a Traffic Manager-profilt?

Igen. Nincsenek a hogyan kombinálásával egy profilon belül a különböző típusú végpontok korlátozások.

### <a name="how-does-hello-billing-model-apply-for-nested-profiles"></a>Hogyan vonatkozik a számlázási modellt hello beágyazott profilok?

Nincs nem negatív árképzési hatását a beágyazott profilok használatával.

A TRAFFIC Manager számlázási két részből áll: végpont állapot-ellenőrzési eredményeire és a DNS-lekérdezések több millió

* Végpont állapotellenőrzést: ingyenesek gyermek profil konfigurálásakor szülő profilban végpontjaként. Hello végpontok hello gyermek profil figyelési lesz számlázva hello a szokásos módon.
* DNS-lekérdezések: minden egyes lekérdezés csak egyszer akkor számít. Egy lekérdezést hajtanak egy szülő-profilt, amely a végpont ad vissza egy gyermek profil elleni hello szülő profil csak akkor számít.

Teljes további információkért lásd: hello [árképzést ismertető oldalra Traffic Manager](https://azure.microsoft.com/pricing/details/traffic-manager/).

### <a name="is-there-a-performance-impact-for-nested-profiles"></a>Van a teljesítményre gyakorolt hatás beágyazott profilok?

Nem. Nincs beágyazott profilok használata során felmerülő teljesítmény hatással.

a Traffic Manager névkiszolgálók hello hello-profil hierarchia minden DNS-lekérdezés feldolgozása közben belső haladnak át. Egy DNS-lekérdezés tooa szülő profilt, amelynek van végpontja egy DNS-választ kaphat gyermek-profilból. CNAME rekord szolgál, függetlenül attól, hogy egyetlen profilhoz vagy beágyazott profilok. Nincs nincs szükség toocreate az egyes profilok hello hierarchia egy olyan CNAME rekordot.

### <a name="how-does-traffic-manager-compute-hello-health-of-a-nested-endpoint-in-a-parent-profile"></a>Hogyan számítja ki a Traffic Manager szülő profilban egy beágyazott végpont hello állapotát?

hello szülő profil nem ellenőrzi a health hello gyermek közvetlenül. Ehelyett hello gyermek profil végpontok hello állapotának használt toocalculate hello hello gyermek profil általános állapotát. Ez az információ mentése beágyazott hello profil hierarchia toodetermine hello állapota beágyazott hello végpont propagálja. hello szülő profil az összesített állapotát toodetermine használ, hogy hello forgalom irányított toohello gyermeke lehet.

hello következő táblázat ismerteti a Traffic Manager ellenőrzi egy beágyazott végpont hello viselkedését.

| Gyermek profil figyelő állapota | Szülőhely végpont-figyelő állapota | Megjegyzések |
| --- | --- | --- |
| Letiltott. hello gyermek profil le van tiltva. |Leállítva |hello szülő végpont állapota leáll, nincs letiltva. hello letiltott állapotát jelzi, hogy le van tiltva hello végpont hello szülő profil számára van fenntartva. |
| A csökkentett teljesítményt. Legalább egy alárendelt profilvégpontjához csökkentett teljesítményű állapotban van. |Online: hello gyermek profilban Online végpontok számának hello van legalább hello MinChildEndpoints értékét.<BR>CheckingEndpoint: hello Online plus CheckingEndpoint végpontok hello gyermek profil van legalább hello MinChildEndpoints értékét.<BR>Csökkentett teljesítményű: más módon. |Forgalom irányított tooan végpont állapot CheckingEndpoint. Ha a MinChildEndpoints túl magasra van állítva, hello végpont mindig csökkentett teljesítményt. |
| Online. Legalább egy alárendelt profilvégpontjához Online állapotban. Nincs végpont hello csökkentett teljesítményű állapotban van. |Fent látható. | |
| CheckingEndpoints. Legalább egy profil maxcardinality értéke "CheckingEndpoint". Nincsenek végpontok "Online" vagy "Csökkentett teljesítményű" |Ugyanaz, mint a fentiek közül. | |
| Inaktív. Gyermek profil végpontjai, vagy le van tiltva, vagy leállt, vagy ehhez a profilhoz nincsenek végpontok rendelkezik. |Leállítva | |

## <a name="next-steps"></a>Következő lépések:
- A Traffic Manager további [végpont figyelési és automatikus feladatátvételt](../traffic-manager/traffic-manager-monitoring.md).
- A Traffic Manager további [forgalom-útválasztási módszerei](../traffic-manager/traffic-manager-routing-methods.md).