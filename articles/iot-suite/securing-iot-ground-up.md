---
title: "a az eszközök internetes hálózatát a hello szabad mentése aaaSecuring |} Microsoft Docs"
description: "Ez a cikk ismerteti a Microsoft Azure IoT Suite hello hello beépített biztonsági szolgáltatásai"
services: 
suite: iot-suite
documentationcenter: 
author: YuriDio
manager: timlt
editor: 
ms.assetid: 10252dfa-8313-4a97-9bd6-a3f1345dd3be
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: yurid
ms.openlocfilehash: a97e8cea753641e1e3c895f44e3fde1e5739d665
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="internet-of-things-security-from-hello-ground-up"></a>Az eszközök internetes hálózatát biztonsági szabad hello a
az eszközök internetes hálózatát (IoT) hello egyedi biztonsági, adatvédelmi és megfelelőségi kihívást toobusinesses világszerte jelent. Hagyományos számítógépes technológia, ahol a problémák szoftver, és hogyan megvalósított köré szerveződik, eltérően IoT vonatkozik, mi történik, ha hello számítógépes és hello fizikai világot átszervezését. Az IoT-megoldások védelmének biztosítása az eszközök, az adott eszközöket és a hello felhő és a biztonságos adatvédelem hello felhőben feldolgozás és a tárolás során közötti biztonságos kapcsolat biztonságos kiépítése igényel. Eszközök fejlesztésének ilyen funkció, azonban a következők erőforrás-korlátozott eszközök, központi telepítések földrajzi eloszlása, és számos olyan megoldást belüli eszközök.

Ez a cikk ismerteti, hogyan hello Microsoft Azure IoT Suite biztonságban az eszközök internetes hálózatát felhőmegoldást biztosít. hello Azure IoT Suite teljes végpont megoldás, biztonsági szabad hello minden szakasz beépített nyújt. A Microsoft biztonságos szoftver fejlesztése része hello szoftver mérnöki gyakorlat szerint a évtizedekben a feltört hosszú élménye biztonságos szoftver fejlesztése. tooensure, biztonságos fejlesztési Életciklussal (SDL) hello eligazodást fejlesztési módszertan használatával, az infrastruktúra-szintű biztonsági szolgáltatásokat, például működési biztonsági megbízhatósági (OSA) és a Microsoft digitális milyen egység hello rengeteg alapján kialakulhat Microsoft Security Response Center, és a Microsoft Malware Protection Center. 

hello Azure IoT Suite ajánlatok egyedi, szolgáltatásokkal kiépítés, csatlakozik, és egyszerű és átlátható az IoT-eszközökről származó adatok tárolása, és minden, a legtöbb biztonságáról. Ebben a cikkben azt vizsgálja meg a hello Azure IoT Suite biztonsági funkciói, és központi telepítési stratégiát tooensure biztonsági, adatvédelmi és megfelelőségi kihívást tárgyalja. 

## <a name="introduction"></a>Bevezetés
hello az eszközök internetes hálózatát (IoT) hello wave a jövőbeli, azonnali vállalatok ajánlat hello és valós lehetőségek tooreduce költségek, bevétel növelése, valamint az üzleti átalakító. Számos vállalat azonban nem határozatlanok toodeploy IoT szervezetük esedékes tooconcerns az biztonsági, adatvédelmi és megfelelőségi funkcióiról. A fő pont érintő hello egyediségi hello IoT infrastruktúra, amely egyesíti a hello számítógépes és fizikai világot együtt, ezek két világot egyedi kockázatok összetételéhez származik. Az IoT biztonsági számítógépfiókokhoz tooensuring hello eszközökön futó, eszköz- és felhasználói hitelesítés, meghatározása eszközöket (valamint az eszközök által létrehozott adatok) egyértelmű tulajdonjogát, de a kód sértetlenségének rugalmas toocyber és fizikai támadások. 

Ezt követően hello probléma van az adatvédelem. A vállalatok szeretné átláthatóság vonatkozó adatok gyűjtése a alatt gyűjtött adatok, és miért, aki láthatja azt, aki szabályozza a hozzáférést, és így tovább. Végezetül nincsenek az azokat üzemeltető hello személyek együtt hello berendezések általános biztonsági problémákat, és az ipari szabványok megfelelőségi fenntartásának.

Hello biztonsági, adatvédelmi, átláthatóság és megfelelőségi szempontok miatt hello jobb IoT-megoldás szolgáltató választása marad kihívást. Szeretne összefűzni IoT szoftver- és más szállítóktól által nyújtott szolgáltatások egyes adatra vezet be a biztonsági, adatvédelmi, átláthatóság és megfelelőségi, amely előfordulhat, hogy lehet rögzített toodetect, hát alone javítsa ki a hézagok meghatározása érdekében. a választott hello hello jobb IoT szoftver és szolgáltatás szolgáltató alapján rendelkező szolgáltatások, amelyek referenciaegyenesen és földrajzi eloszthatja, de is képes tooscale átlátható és biztonságos módon szerzett, kiterjedt tapasztalatunkat szolgáltatók keresése. Ehhez hasonlóan kijelölt hello szolgáltató toohave évtizedeken gépek világszerte több milliárd futó biztonságos szoftver fejlesztése a felhasználói felület segíti, és rendelkezik hello képességét tooappreciate hello veszélyforrásainak tükrében megfigyelhető által jelentett a hello Internet a lehetőségek új világának az Dolgot.

## <a name="secure-infrastructure-from-hello-ground-up"></a>Biztonságos infrastruktúra hello szabad
Hello [Microsoft Cloud](https://www.microsoft.com/enterprise/microsoftcloud/default.aspx#fbid=WzBsRQi6aGk) infrastruktúra támogatja a több mint egymilliárd ügyfelek 127 országokban. A vállalati szoftverek kialakításához és futtatásához néhány hello hello világ legnagyobb online szolgáltatások évtizedekben-hosszú tapasztalatunk rajzolási, nyújtunk fokozott biztonsági, adatvédelmi, megfelelőségi és fenyegetés nagyobb mértékű megoldás eljárásokat, mint a legtöbb ügyfél sikerült a saját elérése.

A [biztonságos fejlesztési Életciklussal (SDL)](https://www.microsoft.com/sdl/) hello teljes szoftver életciklus biztonsági követelményeket beágyazó kötelező vállalati szintű fejlesztési folyamat nyújt. toohelp győződjön meg arról, hogy az operatív tevékenységek kövesse hello azonos szintű biztonsági eljárásokat, szigorú biztonsági irányelvek jelenjen meg az operatív biztonsági megbízhatósági (OSA) folyamat használjuk. Azt is együttműködnek külső naplózási vállalkozások folyamatos ellenőrzés, hogy a Microsoft megfelel-e a kötelezettségek és azt végezhetnek átfogó biztonsági erőfeszítéseket keresztül képességekkel, beleértve a Microsoft Security Microsoft digitális milyen egység hello középpontja hello létrehozása Válasz Center és a Microsoft Malware Protection Center.

## <a name="microsoft-azure---secure-iot-infrastructure-for-your-business"></a>A Microsoft Azure - biztonságos IoT-infrastruktúra a vállalkozása számára
A Microsoft Azure biztosít egy teljes felhőalapú megoldáson, egyet, amely egyesíti az integrált felhőszolgáltatások folyamatosan növekvő gyűjteménye – analytics, a gépi tanulás, a tárolás, a biztonsági, a hálózat és a webes – egy iparágvezető kötelezettségvállalás toohello protection szolgáltatással és az adatok. A [szabálysértésnek minősülő feltételezik](https://azure.microsoft.com/blog/red-teaming-using-cutting-edge-threat-simulation-to-harden-the-microsoft-enterprise-cloud/) stratégia használ egy dedikált "red" szoftver biztonsági szakértői csoport számára szimulálása támadások, a tesztelés Azure toodetect hello képességét, megjelenő fenyegetéseket, és elleni megszegése helyreállíthatók. A [globális incidensválasz](https://www.microsoft.com/TrustCenter/Security/DesignOpSecurity) team works körül hello órajelsebességgel toomitigate hello hatásait támadások és a rosszindulatú tevékenységhez. hello team elfogadott incidenskezelés kommunikációra és helyreállítási eljárásokat követi, és használja a felderíthető és az előre jelezhető belső és külső partnerekkel.

Rendszer adja meg a folyamatos behatolásérzékelési és megelőzési, szolgáltatás támadás megelőzése, rendszeres behatolás tesztelése és törvényszéki eszközök, amelyek segítenek azonosítani és elhárítani a fenyegetéseket. [A multi-factor authentication](../multi-factor-authentication/multi-factor-authentication.md) egy további biztonsági réteget nyújt a végfelhasználók tooaccess hello hálózati. És a hello alkalmazásokhoz és hello állomás szolgáltató, a hozzáférés-vezérlés, figyelés, kártevőirtó, biztonsági rések keresése, javítások és konfigurációkezelés kínálunk.

kihasználja a Microsoft Azure IoT Suite hello hello biztonsági és adatvédelmi beépített hello Azure platformon az SDL és OSA folyamatok biztonságos fejlesztési és az összes Microsoft szoftver művelet együtt. Ezek az eljárások infrastruktúra védelme, hálózatvédelmi és identitás- és felügyeleti funkciókat alapvető toohello biztonsági megoldások biztosítása. 

Hello [Azure IoT Hub](../iot-hub/iot-hub-what-is-iot-hub.md) belül hello [IoT Suite](iot-suite-what-is-azure-iot.md) teljes körűen felügyelt szolgáltatást, amely lehetővé teszi az IoT-eszközök és az Azure szolgáltatások, például közöttimegbízhatóésbiztonságoskétirányúkommunikáció[Azure Machine Learning](../machine-learning/machine-learning-what-is-machine-learning.md) és [Azure Stream Analytics](../stream-analytics/stream-analytics-introduction.md) eszközönkénti hitelesítő adatokat és a hozzáférés-vezérlés használatával.

toobest kommunikációhoz hello Azure IoT Suite beépített biztonsági és adatvédelmi szolgáltatásokat, azt hogy bontásban hello suite hello három elsődleges biztonsági területre. 

![Azure IoT Suite](media/securing-iot-ground-up/securing-iot-ground-up-fig3.png)

### <a name="secure-device-provisioning-and-authentication"></a>Kiépítés biztonságos eszköz és a hitelesítés
hello Azure IoT Suite biztosítja az eszközök, amíg azok kimenő hello mezőben, adja meg a egy egyedi identitása kulcs az egyes eszközök, amelyek segítségével hello IoT infrastruktúra toocommunicate hello eszközzel bár ez a művelet. hello folyamat gyors és egyszerű tooset fel. hello jönnek létre, hogy egy felhasználó által kiválasztott eszköz azonosítója űrlapok hello alapját hello eszköz és hello Azure IoT-központ közötti összes kommunikáció során használt jogkivonat-kulcsot.

Eszközazonosítókat társítható egy eszközt (például Flash memóriájában rögzített hardver megbízhatósági modulban) gyártási, vagy használhat egy meglévő rögzített identity (például CPU sorozatszámokat) proxy-ként. Mivel a hello eszköz a azonosító adatok módosítása nem egyszerű, fontos toointroduce logikai eszköz azonosítók is, ha az alapul szolgáló eszköz hardvermódosításokat hello de hello logikai eszköz továbbra is hello azonos. Bizonyos esetekben egy eszközidentitás hello hozzárendelését (azaz egy hitelesített mező mérnököt fizikailag konfigurálja egy új eszközt hello megoldás háttér folytatott kommunikáció során) eszközt a központi telepítéskor fordulhat elő. Hello [Azure IoT Hub identitásjegyzékhez](../iot-hub/iot-hub-devguide.md) eszköz identitások és a biztonsági kulcsok megoldás biztonságos tárolására szolgál. Személy vagy eszköz identitások csoportok is hozzáadhatók tooan engedélyezése, vagy egy tiltólista engedélyezése a teljes felügyeletet gyakorolhat az eszközök elérést.

Az Azure IoT Hub hozzáférés-vezérlési házirendeket hello felhőben engedélyezése aktiválás és az eszközidentitás, így egy módon toodisassociate eszköz szükség esetén az IoT-telepítéséből letiltása. A társítás, és az eszközök disassociation minden eszközidentitás alapul.

További eszközök biztonsági funkciói hello alábbiakat foglalja magába:

* Eszközök nem fogadja el a nem kért hálózati kapcsolatokat. Csak kimenő módon azok kapcsolódó összes kapcsolatok és útvonalakat. Az eszköz tooreceive hello háttérrendszerből parancs hello eszköz egy kapcsolat toocheck az összes függőben lévő parancsok tooprocess kell kezdeményeznie. Miután hello eszköz- és IoT-központ közötti kapcsolat biztonságos létrejött, hello felhő toohello eszköz és az eszköz toohello üzenetküldési felhő küldhető transzparens módon.  
* Eszközök csak tooor csatlakoznak, amellyel azok vannak társviszonyban, például az Azure IoT-központ útvonalak toowell ismert szolgáltatások létrehozásához.
* A hitelesítési és engedélyezési rendszerszintű használja eszközönkénti identitások hozzáférési hitelesítő adatok és az engedélyek közelében-azonnal visszavonható.

### <a name="secure-connectivity"></a>Biztonságos kapcsolat
Az üzenetküldési tartóssági az IoT-megoldás egyik fontos szolgáltatása. hello kell toodurably parancsok biztosításához és/vagy eszközök fogadhat adatokat, hogy az IoT-eszközök csatlakoznak-e hello Internet, vagy más hasonló hálózatok, ami megbízhatatlan lehet hello tény aláhúzott. Azure IoT-központ az üzenetküldési és válasz toomessages a nyugtázások rendszert eszközök közötti tartósságot biztosít. Az üzenetkezelés további tartóssági gyorsítótárazási üzenetet az IoT-központ hello tooseven napos, a telemetria és két napot ki parancsokat érhető el.

Hatékonyságát fontos tooensure erőforrások megőrzése és erőforrás-korlátozott környezetben művelet. HTTPS (HTTP biztonságos) hello szabványos biztonságos hello népszerű http protokoll verzióját, hatékony kommunikáció engedélyezése Azure IoT-központ által támogatott. Speciális Message Queuing protokoll (AMQP) és a Message Queuing Telemetriai Transport (MQTT), Azure IoT-központ által támogatott úgy vannak kialakítva, nem csak a hatékonyság erőforrás használja, de megbízható üzenetkézbesítést tekintetében. 

Méretezhetőség hello képességét toosecurely együttműködik az eszközök széles igényel. Az Azure IoT-központ lehetővé teszi, hogy biztonságos kapcsolatot tooboth IP-t használó, és nem IP-kompatibilis eszközök. IP-t használó eszközök is képes toodirectly csatlakozás és az IoT-központ hello biztonságos kapcsolaton keresztül kommunikál. A nem IP-kompatibilis eszközök erőforrás-korlátozott, és csak rövid távolság kommunikációs protokollokat, például Zwave ZigBee vagy Bluetooth-on keresztül. A mező átjáró használt tooaggregate ezeket az eszközöket, valamint protokoll fordítási tooenable biztonságos kétirányú kommunikációt hello felhő hajt végre.

Kapcsolat további funkciókat hello alábbiakat foglalja magába:

* hello eszközök és az Azure IoT-központ között, vagy átjárók és az Azure IoT-központ közötti kommunikáció elérési használatával lett biztonságossá téve szabványos Transport Layer Security (TLS) az Azure IoT Hub hitelesített X.509 protokoll használatával.
* Azure IoT Hub rendelés tooprotect az eszközöknek a kéretlen bejövő kapcsolatokat, nem nyílik meg minden olyan kapcsolat toohello eszköz. hello eszköz kezdeményezi az összes kapcsolat. 
* Azure IoT Hub tartósan tárolja az üzeneteket az eszközök, és megvárja-e a hello eszköz tooconnect. Ezek a parancsok engedélyezése csak időnként, kapcsolódó miatt toopower vagy kapcsolódási problémákat, tooreceive eszközök ezek a parancsok két napig tárolják. Az Azure IoT Hub fenntartja az egyes eszközök eszközönkénti várólista.

### <a name="secure-processing-and-storage-in-hello-cloud"></a>Biztonságos feldolgozási és hello felhőalapú tárolás
A titkosított kommunikáció tooprocessing adataiból hello felhőben hello Azure IoT Suite segít a biztonságos. Rugalmasság tooimplement további titkosítási és a biztonsági kulcsok felügyeleti biztosít. Azure Active Directory (AAD) használ a felhasználói hitelesítés és engedélyezés, Azure IoT Suite biztosíthat egy csoportházirend-alapú engedélyezési modellt hello felhő adatainak naplózva, és tekintse át, mennyire egyszerű a hozzáférés felügyeletének engedélyezésére. Ez a modell is lehetővé teszi a hozzáférés toodata hello felhőben, illetve az eszközök csatlakoztatott toohello Azure IoT Suite szinte azonnali visszavonás.

Ha adatok hello felhőben, feldolgozni, és bármely felhasználó által definiált munkafolyamat tárolja. Hozzáférés tooeach hello adatok egy részét az Azure Active Directoryval, attól függően, hogy a használt hello társzolgáltatás vezérlik.

Hello IoT infrastruktúra által használt összes kulcsok biztonságos tárolóban, a hello képességét tooroll keresztül hello felhőben vannak tárolva, arra az esetre kulcsok toobe újra üzembe kell. Adatok tárolhatók [Azure Cosmos DB](../documentdb/documentdb-introduction.md) vagy [SQL-adatbázisok](../sql-database/sql-database-faq.md), szükséges biztonsági szintjét hello meghatározásának engedélyezése. Emellett Azure tartalmaz egy módja toomonitor és naplózási adatok tooalert minden hozzáférési tooyour bármely behatolás, illetve az illetéktelen hozzáféréstől.

## <a name="conclusion"></a>Összegzés
hello az eszközök internetes hálózatát kezdődik-e a dolgok – hello legfontosabb dolgokról szolgáltatnak legtöbb toobusinesses. Az IoT költségek csökkentése, árbevétel növelését, és üzleti átalakítása elképesztő érték tooa üzleti biztosíthat. Sikerült az átalakítás nagymértékben függ hello jobb IoT szoftver és szolgáltatás szolgáltató kiválasztásához. Ez azt jelenti, hogy olyan szolgáltatót, amely nem csak az átalakítás által az üzleti igények ismertetése és követelmények catalyzes, de emellett a szolgáltatások és szoftverek biztonsági, adatvédelmi, átláthatóság és megfelelőségi fő tervezési megfontolások a következővel keresése. A Microsoft szerzett, kiterjedt tapasztalatunkat fejlesztésük és biztonságos szoftverek és -szolgáltatások üzembe helyezése és az új élettartamát az eszközök internetes hálózatát toobe egy vezető folytatódik. 

úgy lett kialakítva, eszközök tooimprove hatékonyság meghajtó működési teljesítmény tooenable innováció, biztonságos figyelés engedélyezése biztonsági intézkedéseket a Microsoft Azure IoT Suite hello épít fel és alkalmaz advanced adatelemzés tootransform vállalatok számára. A réteges megközelítésének biztonsági, több biztonsági szolgáltatásokat és kialakítási minta felé, az Azure IoT Suite segítségével központi telepítése az infrastruktúra, amely megbízható tootransform lehet bármely üzleti. 

## <a name="additional-information"></a>További információ
Minden Azure IoT Suite előkonfigurált megoldás hoz létre az Azure-szolgáltatások, például hello következő egy példányát:

* [**Az Azure IoT Hub**](https://azure.microsoft.com/services/iot-hub/): az átjáró, amely a hello felhő túl "művelet". Kapcsolatok / hub és a folyamat nagy mennyiségű adatot toomillions eszközönkénti hitelesítést támogató segít a megoldás biztonságos méretezheti.
* [**Az Azure Cosmos DB**](https://azure.microsoft.com/services/documentdb/): egy méretezhető, teljes mértékben indexelt dokumentumadatbázis-szolgáltatás, amely kezeli a metaadatok hello eszközök félig strukturált adatok ellátásához, például az attribútumokat, a konfiguráció és a biztonsági tulajdonságait. Cosmos DB nagy teljesítményű és nagy átviteli feldolgozás, a séma-független indexelő adatokat, és egy részletes SQL-lekérdezési felületet kínál.
* [**Az Azure Stream Analytics**](https://azure.microsoft.com/services/stream-analytics/): valós idejű streamfeldolgozó hello felhőben, amely lehetővé teszi, hogy toorapidly fejlesztéséhez és központi telepítése egy alacsony költségű analytics megoldás toouncover valós idejű elemzése eszközök, érzékelőket, infrastruktúra, és alkalmazások. a teljes körűen felügyelt szolgáltatás hello adatait is méretezhető tooany kötet megőrzése mellett nagy átviteli sebességet, alacsony késéssel és rugalmasság.
* [**Az Azure App Services**](https://azure.microsoft.com/services/app-service/): egy felhőalapú platform toobuild hatékony webes és mobilalkalmazások bárhol; toodata hello felhőalapú vagy helyszíni. Vonzó alkalmazások készítése iOS, Android és Windows rendszerre. Integrálható a szoftver szolgáltatott szoftverként (SaaS) és vállalati alkalmazások a felhő alapú szolgáltatások out-of-az-box kapcsolat toodozens és vállalati alkalmazások. A kedvenc nyelvi és IDE a kód – .NET, Node.js, PHP, Python vagy Java – toobuild webes alkalmazásokat és API-k minden eddiginél gyorsabban.
* [**A Logic Apps**](https://azure.microsoft.com/services/app-service/logic/): hello Logic Apps szolgáltatás az Azure App Service segítségével rendszerintegráció IoT megoldás tooyour meglévő üzleti és munkafolyamat-folyamatok automatizálása. A Logic Apps lehetővé teszi, hogy a fejlesztők toodesign munkafolyamatok, amelyek egy és majd végrehajtanak bizonyos lépéseket – szabályok és a műveletek, amelyeket az üzleti folyamatok hatékony összekötők toointegrate használhat. A Logic Apps kínál out-of-az-box kapcsolat tooa túlnyomó ökoszisztémájának alkalmazásával az SaaS, a felhő alapú, és a helyszíni alkalmazások.
* [**Az Azure blob storage**](https://azure.microsoft.com/services/storage/): hello adatok, az eszközök küldjön toohello felhő gazdaságos, megbízható felhőalapú tárolása.

## <a name="next-steps"></a>Következő lépések
További információ az biztonságossá tétele az IoT-megoldásból toolearn lásd:

* [Az IoT ajánlott biztonsági eljárások][lnk-security-best-practices]
* [Az IoT-biztonsági architektúrája][lnk-security-architecture]
* [Az IoT üzembe][lnk-security-deployment]

[lnk-security-best-practices]: iot-security-best-practices.md
[lnk-security-architecture]: iot-security-architecture.md
[lnk-security-deployment]: iot-suite-security-deployment.md

Akkor is is felfedezheti hello más szolgáltatásokat és képességeket hello előre konfigurált IoT Suite megoldások:

* [Előre konfigurált prediktív karbantartási megoldás áttekintése][lnk-predictive-overview]
* [Gyakran ismételt kérdések az IoT Suite-ról][lnk-faq]

Az IoT-központ biztonsági olvashat [vezérlő hozzáférés tooIoT Hub] [ lnk-devguide-security] az IoT Hub fejlesztői útmutató hello.

[lnk-predictive-overview]: iot-suite-predictive-overview.md
[lnk-faq]: iot-suite-faq.md
[lnk-devguide-security]: ../iot-hub/iot-hub-devguide-security.md
