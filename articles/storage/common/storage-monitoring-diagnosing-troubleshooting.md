---
title: "aaaMonitor, diagnosztizálása és elhárítása az Azure Storage |} Microsoft Docs"
description: "Az-szolgáltatások, például tárolási analitika, ügyféloldali naplózás, és más külső gyártású eszközöknek tooidentify, diagnosztizálása és Azure tárolással kapcsolatos problémák elhárításához."
services: storage
documentationcenter: 
author: fhryo-msft
manager: jahogg
editor: tysonn
ms.assetid: d1e87d98-c763-4caa-ba20-2cf85f853303
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/11/2017
ms.author: fhryo-msft
ms.openlocfilehash: 43f34a4ccbc3071cdb489958252498a1f88e1a34
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-diagnose-and-troubleshoot-microsoft-azure-storage"></a>Microsoft Azure Storage felügyelete, diagnosztizálása és hibaelhárítása
[!INCLUDE [storage-selector-portal-monitoring-diagnosing-troubleshooting](../../../includes/storage-selector-portal-monitoring-diagnosing-troubleshooting.md)]

## <a name="overview"></a>Áttekintés
Diagnosztizálása és elhárítása egy üzemeltetett felhőalapú környezetek elosztott alkalmazásban lévő lehet bonyolultabb, mint a hagyományos környezetekben. Alkalmazások PaaS vagy infrastruktúra-szolgáltatási infrastruktúra, a helyszíni, egy mobileszközön, vagy ezek valamilyen kombinációjára is telepíthető. Általában az alkalmazás hálózati forgalmat is haladnak át a nyilvános és titkos és az alkalmazás használhat több tárolási technológiák, például a Microsoft Azure Storage-táblákat, BLOB, üzenetsorok vagy hozzáadása tooother adatok fájlokat tárolja, például a relációs és dokumentum-adatbázisokat.

toomanage ilyen alkalmazások sikeresen, figyelheti azokat proaktív módon, és megérteni hogyan toodiagnose és hárítsa el őket, és a függő technológiák minden szempontját. Egy Azure Storage szolgáltatás felhasználóként kell folyamatosan figyelni hello tárolási szolgáltatások váratlan viselkedést (például a lassabb, mint a szokásos válaszidő) módosításai az alkalmazás által, és naplózási toocollect használja az adatok és tooanalyze részletes egy probléma merült fel a mélységét. hello diagnosztika figyelés és naplózás le nyújt segítséget, toodetermine hello okának hello adja ki az alkalmazást észlelt. Ezután hello problémával kapcsolatos hibaelhárítás elősegítéséhez és hello megfelelő lépéseket, amelyek tooremediate határozza meg azt. Az Azure Storage alapszintű Azure-szolgáltatás, és, hogy az ügyfelek telepíthetnek toohello Azure infrastruktúra-megoldások többsége hello fontos részét képezi. Az Azure Storage tartalmazza a képességek toosimplify figyelése, diagnosztizálása és a felhőalapú alkalmazásokban tárolási problémák elhárításához.

> [!NOTE]
> hello Azure File storage nem támogatja a jelenleg naplózását.
> 

A gyakorlati útmutatóban tooend körű hibaelhárítása az Azure Storage-alkalmazásokban, lásd: [-végpontok hibaelhárítása az Azure Storage Metrics és a naplózás, az AzCopy és a Message Analyzer segítségével](../storage-e2e-troubleshooting.md).

* [Bevezetés]
  * [Hogyan szerveződik, ez az útmutató]
* [a tárolás szolgáltatás figyelése]
  * [Figyelési szolgáltatásának állapota]
  * [Kapacitásának figyelése]
  * [Rendelkezésre állás figyelése]
  * [A teljesítmény figyelése]
* [tárolási problémák diagnosztizálása]
  * [Szolgáltatás ügynökállapottal kapcsolatos hibákkal]
  * [Teljesítménnyel kapcsolatos problémák]
  * [Hibák diagnosztizálása]
  * [Tárolási emulátor problémák]
  * [Tárolási naplózási eszközök]
  * [Hálózati naplózási eszközök használatával]
* [végpont nyomkövetés]
  * [Naplóadatok válaszoknak az összekapcsolása]
  * [Ügyfélkérelem-azonosító]
  * [Server Kérelemazonosító]
  * [Időbélyeg helyi időre]
* [hibaelhárítási útmutatás]
  * [metrika AverageE2ELatency magas és alacsony AverageServerLatency]
  * [A metrika alacsony AverageE2ELatency és alacsony AverageServerLatency, de hello ügyfél tapasztal nagy késleltetésű]
  * [A mérőszámok magas AverageServerLatency értéket mutatnak]
  * [Váratlan késést tapasztal az üzenetsorban található üzenetek kézbesítésekor]
  * [PercentThrottlingError metrika növelését]
  * [PercentTimeoutError metrika növelését]
  * [A mérőszámok emelkedő PercentNetworkError értéket mutatnak]
  * [hello ügyfél HTTP 403 (tiltott) üzeneteket fogad]
  * [hello ügyfél (nem található) HTTP 404 üzeneteket fogad]
  * [hello ügyfél HTTP 409 (Ütközés) üzeneteket fogad]
  * [metrika alacsony PercentSuccess vagy analytics naplóbejegyzések rendelkezik műveletek tranzakció állapota ClientOtherErrors]
  * [Teljesítmény-mérőszámait váratlan növekedését megjelenítése a tárolási kapacitás-használat]
  * [Váratlan vesznek el virtuális gépeken, amelyek csatlakoztatott virtuális merevlemezek nagy számú tapasztal]
  * [A probléma merül fel a fejlesztési vagy tesztelési hello storage emulator használatával]
  * [Áll kapcsolatban problémák hello Azure SDK telepítése a .NET-hez]
  * [Van egy másik probléma storage szolgáltatással]
  * [A Windows Azure File storage problémák elhárítása](../files/storage-troubleshoot-windows-file-connection-problems.md)   
  * [A Linux és Azure File storage problémák elhárítása](../files/storage-troubleshoot-linux-file-connection-problems.md)
* [mellékletek]
  * [1 függelék: Fiddler toocapture HTTP és HTTPS-forgalom használatával]
  * [2 függelék: Wireshark toocapture hálózati forgalom használ]
  * [3 függelék: a Microsoft Message Analyzert toocapture hálózati forgalom használ]
  * [4. függelék: Excel tooview metrikákat és naplózási adatok használata]
  * [5. függelék: A Visual Studio Team Services az Application insights szolgáltatással figyelését.]

## <a name="introduction"></a>Bevezetés
Ez az útmutató jeleníti meg, hogyan toouse szolgáltatásait, például az Azure Storage Analytics hello Azure Storage ügyféloldali kódtár ügyféloldali naplózási vagy más külső gyártású eszközöknek tooidentify diagnosztizálása és elhárítása az Azure Storage kapcsolatos hiba lépett fel.

![][1]

Ez az útmutató elsősorban az online szolgáltatások, Azure Storage szolgáltatásainak és az informatikai szakemberek felelős, ilyen online szolgáltatások kezelésére szolgáló használó fejlesztők olvasni kívánt toobe is. a jelen útmutató hello céljai a következők:

* az Azure Storage-fiókok a hello állapotát és teljesítményét karbantartása toohelp.
* tooprovide való hello szükséges eljárások és eszközök toohelp úgy dönt, ha egy hiba vagy probléma merült fel a kérelmet tooAzure tárolási vonatkozik.
* hogy végrehajthatóként útmutatással kapcsolatos problémák megoldásához kapcsolódó tooAzure tárolási tooprovide.

### <a name="how-this-guide-is-organized"></a>Hogyan szerveződik, ez az útmutató
a szakasz hello "[a tárolás szolgáltatás figyelése]" ismerteti, hogyan toomonitor hello állapotának és teljesítményének az Azure Storage szolgáltatások, Azure Storage Analytics Metrics (Storage Metrics) használatával.

a szakasz hello "[tárolási problémák diagnosztizálása]" ismerteti, hogyan toodiagnose állít Azure Storage Analytics-naplózás használata (tárhely-naplózás). Azt is bemutatja, milyen módon tooenable ügyféloldali naplózás használatával hello létesítményekben valamelyik hello klienskódtárak segítségével például hello a Storage ügyféloldali kódtára a .NET-hez és Javához készült Azure SDK hello.

a szakasz hello "[végpont nyomkövetés]" ismerteti, hogyan hozhatók hello szereplő információk különböző naplófájlokat és metrikai adatok.

a szakasz hello "[hibaelhárítási útmutatás]" nyújt hibaelhárítási útmutatót egyes hello közös tárolással kapcsolatos problémák léphetnek fel.

hello "[mellékletek]" egyéb eszközzel, Wireshark vagy netmon eszközzel elemzése hálózati csomagadatok, a Fiddler a HTTP/HTTPS-üzenetek, elemzése és a Microsoft Message Analyzert használatával történik az adatok naplózása információkat tartalmaznak.

## <a name="monitoring-your-storage-service"></a>A storage szolgáltatás figyelése
Ha ismeri a Windows Teljesítményfigyelő, tulajdonképpen Storage Metrics, hogy a Windows Teljesítményfigyelő-számlálókból egy Azure Storage megfelelője. Storage Metrics található széles választékát metrikák (számlálók a Windows Teljesítményfigyelő terminológia) például a szolgáltatás rendelkezésre állása, a tooservice kérelmek teljes száma, vagy a sikeres kérelmek tooservice százalékát. Hello elérhető mérőszámok teljes listáját lásd: [Storage Analytics metrikák táblaséma](http://msdn.microsoft.com/library/azure/hh343264.aspx). Azt is adja meg, hogy hello tárolási szolgáltatás toocollect és összesített metrikák minden órában vagy percben. További információ a hogyan tooenable metrikák és a figyelő a storage-fiókok: [storage mérőszámainak engedélyezése és megtekintése a metrikai adatok](http://go.microsoft.com/fwlink/?LinkId=510865).

Választhat, hogy mely óránkénti metrikák azt szeretné, hogy a hello toodisplay [Azure-portálon](https://portal.azure.com) és értesíthetők a rendszergazdák e-mailben, amikor egy óránkénti mérőszám eléri az adott szabályok konfigurálása. További információkért lásd: [riasztási értesítéseket kapni](/azure/monitoring-and-diagnostics/monitoring-overview-alerts.md). 

hello tároló szolgáltatás segítségével a lehető legkedvezőbb metrikákat gyűjtő, de nem rögzíthet minden tárolási műveletet.

Hello Azure-portálon például a rendelkezésre állási, kérelmek teljes száma és a tárfiókon átlagos várakozási számok metrikák megtekintheti. Egy értesítési szabályt is be van állítva tooalert rendszergazda ha rendelkezésre állási bizonyos szintű alá süllyed. Tekinti meg ezeket az adatokat, hogy meg lehessen vizsgálni egy lehetséges terület nem hello tábla szolgáltatás sikeres arány nem érik el a 100 %-os (további információkért lásd: hello szakasz "[metrika alacsony PercentSuccess vagy analytics naplóbejegyzések rendelkezik műveletek tranzakció állapota ClientOtherErrors]").

Az Azure-alkalmazások tooensure kifogástalan és teljesítő által várt folyamatosan figyelje:

* Létrehozó néhány alapterv metrikák alkalmazáshoz, amelyek lehetővé teszik a toocompare aktuális adatokat és az Azure storage és az alkalmazás hello viselkedését jelentős módosításai azonosításához. a baseline metrikákat hello értékeit, sok esetben adott alkalmazás, és ki kell alakítania őket, amikor az alkalmazás teljesítménytesztelési.
* Percenkénti metrikákat rögzítése, és használatuk toomonitor aktívan váratlan hibák és rendellenességeket, például a hiba számát vagy a kérelem díjszabás teljesítményt.
* Óránkénti metrikák rögzítése, és használatuk toomonitor átlagos értékeit, mint átlagos hiba száma, és díjszabás kérelem.
* Diagnosztikai eszközökkel később hello szakaszban bemutatott lehetséges problémákat vizsgálja "[tárolási problémák diagnosztizálása]."

a következő kép hello hello diagramok bemutatják, hogyan hello átlagolási óránkénti metrikáihoz előforduló elrejthetők igényeiben jelentkező tevékenységben. hello óránkénti metrikák tooshow kérelmek, állandó arányát metrikák azt mutatják, a hely valóban hello ingadozását hello perc során jelennek meg.

![][3]

hello hátralévő részét ez a szakasz ismerteti, milyen, célszerű figyelemmel kísérni metrikák és okát.

### <a name="monitoring-service-health"></a>Figyelési szolgáltatásának állapota
Használhatja a hello [Azure-portálon](https://portal.azure.com) tooview hello állapotát hello tárolási szolgáltatás (és más Azure-szolgáltatások) az összes Azure-régiók körül hello world hello. Ez lehetővé teszi toosee azonnal amennyiben kívül a problémát érinti hello társzolgáltatás hello régióban, az alkalmazás használja.

Hello [Azure-portálon](https://portal.azure.com) különböző Azure-szolgáltatásokat is megadhatja az hello érintő incidensek szóló értesítéseket.
Megjegyzés: Ez az információ korábban rendelkezésre állt, valamint előzményadatokat a hello [Azure szolgáltatás irányítópultját](http://status.azure.com).

Hello közben [Azure-portálon](https://portal.azure.com) gyűjti állapotadatokat belül hello Azure adatközpontjaiban (belső kibővített figyelés), is érdemes lehet egy külső a megközelítést toogenerate szintetikus tranzakciók, amely rendszeres időközönként bevezetése az Azure által üzemeltetett webalkalmazások több helyen elérését. hello által kínált szolgáltatások [Dynatrace](http://www.dynatrace.com/en/synthetic-monitoring) és az Application Insights for Visual Studio Team Services példák a külső hátránya. A Visual Studio Team Services Application insights szolgáltatással kapcsolatos további információkért lásd: hello függelék "[5 függelék: figyelés az Application Insights for Visual Studio Team Services](#appendix-5)."

### <a name="monitoring-capacity"></a>Kapacitásának figyelése
Storage mérőszámainak csak tárolja a teljesítmény-mérőszámait hello blob szolgáltatáshoz, mert blobok általában fiók tárolt adatok hello legnagyobb részét (hello írásának időpontjában, már nem lehetséges toouse Storage Metrics toomonitor hello kapacitás a táblák és-várólista) . Ezek az adatok találhatók hello **$MetricsCapacityBlob** tábla, ha engedélyezte a Blob szolgáltatás hello figyelését. Storage mérőszámainak rögzíti az adatokat naponta egyszer, és használhatja a hello hello értékének **RowKey** toodetermine hogy hello sor tartalmazza-e olyan entitás, amely toouser adatok vonatkozik (érték **adatok**) vagy analytics adatok () érték **analytics**). Minden tárolt entitás felhasznált terület méretének hello adatait tartalmazza (**kapacitás** bájtban mért) és a tárolók hello jelenlegi száma (**ContainerCount**) és a blobok ( **ObjectCount**) hello tárfiókot használja. Hello kapacitási mérőszámokat hello tárolt kapcsolatos további információk **$MetricsCapacityBlob** táblázatban, lásd: [Storage Analytics metrikák táblaséma](http://msdn.microsoft.com/library/azure/hh343264.aspx).

> [!NOTE]
> Ezeket az értékeket, hogy a tárfiók hello kapacitáskorlátait hamarosan eléri a korai figyelmeztetéseket, célszerű figyelemmel kísérni. Hello Azure-portálon Ha összesítő tárolóhely-használat meghaladja vagy alá esik küszöbértékeket, hogy megadja a riasztási szabályok toonotify adhat hozzá.
> 
> 

Különböző tárolási objektumok, például a blobok hello méretének becslése útmutatásért lásd: hello blogbejegyzésben [az Azure Storage számlázási – sávszélesség, a tranzakciók és a kapacitás](http://blogs.msdn.com/b/windowsazurestorage/archive/2010/07/09/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity.aspx).

### <a name="monitoring-availability"></a>Rendelkezésre állás figyelése
Célszerű figyelemmel kísérni hello tárolószolgáltatások hello rendelkezésre állását a tárfiókban lévő hello hello értéket figyelésével **rendelkezésre állási** oszlopa hello óránként vagy percenkénti metrikákat táblák – **$ MetricsHourPrimaryTransactionsBlob**, **$MetricsHourPrimaryTransactionsTable**, **$MetricsHourPrimaryTransactionsQueue**, **$ MetricsMinutePrimaryTransactionsBlob**, **$MetricsMinutePrimaryTransactionsTable**, **$MetricsMinutePrimaryTransactionsQueue**, **$ MetricsCapacityBlob**. Hello **rendelkezésre állási** oszlop, amely jelzi a hello rendelkezésre állását hello szolgáltatást vagy hello sor által képviselt hello API művelet százalékos értéket tartalmaz (hello **RowKey** jeleníti meg, hogy hello sort tartalmazza metrikák hello szolgáltatás egésze vagy egy adott API-művelet).

Bármely érték, amely kisebb, mint 100 %-os azt jelzi, hogy bizonyos tárolási kérelmeket nem működnek. Láthatja, hogy miért ezek nem működnek megvizsgálásával hello metrikai adatok, amelyek különböző típusok kérelmek hello számok, mint a más oszlopok hello **ServerTimeoutError**. Toosee számíthat **rendelkezésre állási** átmeneti kiszolgáló időtúllépése közben hello szolgáltatás áthelyezi toobetter terheléselosztásához kérelem okokból ideiglenesen 100 %-os alá csökken; hello újrapróbálkozási logika az ügyfél-alkalmazásban ilyen időszakos feltételek kell kezelni. hello cikk [Storage Analytics naplózott műveletekkel és az állapotüzenetek](http://msdn.microsoft.com/library/azure/hh343260.aspx) listák hello Storage Metrics tartalmaz a tranzakciótípusok a **rendelkezésre állási** számítási.

A hello [Azure-portálon](https://portal.azure.com), a riasztási szabályok toonotify adhat hozzá, ha **rendelkezésre állási** a szolgáltatás a megadott küszöbérték alá esik.

hello "[hibaelhárítási útmutatás]" című szakaszában talál néhány gyakori tárolási szolgáltatás problémák kapcsolódó tooavailability ismerteti.

### <a name="monitoring-performance"></a>A teljesítmény figyelése
hello tárolószolgáltatások toomonitor hello teljesítményétől, a hello használhatja hello metrikák követően óránként és metrikákat táblák perc.

* hello értékek hello **AverageE2ELatency** és **AverageServerLatency** oszlopaiban hello átlagos idő hello társzolgáltatás vagy API művelet típusa tooprocess kérelmek tart. **AverageE2ELatency** egy mérték, amely tartalmazza az idő hello tooread hello kérelem-végpontok közötti késés és hello válasz hozzáadása toohello igénybe vett idő tooprocess hello kérés küldése (ezért tartalmazza a hálózati késés után hello kérése hello társzolgáltatás eléri); **AverageServerLatency** csak hello feldolgozási időt egy mérték, és ezért nem tartalmazza az összes kapcsolódó hálózati késés toocommunicating hello ügyféllel. Című rész hello "[metrika AverageE2ELatency magas és alacsony AverageServerLatency]" az útmutató információt a miért lehet jelentős különbség a két érték között.
* hello értékek hello **TotalIngress** és **TotalEgress** kívül a társzolgáltatás vagy a megadott API-művelet típusra keresztül tooand várható oszlopaiban hello teljes adatmennyiség bájtban.
* hello értékek hello **TotalRequests** oszlop megjelenítése hello kérelmek teljes száma, amelyek a társzolgáltatás API művelet hello fogadja. **TotalRequests** hello hello tárolási szolgáltatás fogadja a kérelmek teljes száma.

Általában fogja figyelni a ezek bármelyike váratlan változásokat, azt jelzi, hogy rendelkezik-e a hibát, amely vizsgálatot igényel.

A hello [Azure-portálon](https://portal.azure.com), toonotify, ha bármelyik teljesítménymutatók hello szolgáltatás alá esnek vagy meghaladnia a küszöbértéket, melyet a riasztási szabályok is hozzáadhat.

hello "[hibaelhárítási útmutatás]" című szakaszában talál néhány gyakori tárolási szolgáltatás problémák kapcsolódó tooperformance ismerteti.

## <a name="diagnosing-storage-issues"></a>Tárolási problémák diagnosztizálása
Számos módon, hogy előfordulhat, hogy tudomást hiba vagy probléma a alkalmazás is van, ezek közé tartoznak:

* Hello alkalmazás toocrash vagy toostop működő okozó súlyos hiba.
* Az alapértékek hello metrikák hello előző szakaszban leírtak szerint figyeli a jelentős változtatások "[a tárolás szolgáltatás figyelése]."
* A felhasználók az alkalmazás, amely egy adott művelet nem fejeződött be, várt módon jelentések vagy, hogy néhány szolgáltatás nem működik.
* Hibák keletkeztek az alkalmazáson belül, amely jelennek meg a naplófájlokban vagy néhány egyéb értesítési metódussal.

Általában problémák kapcsolódó tooAzure tárolószolgáltatások egyikébe négy kategóriába sorolhatók:

* Az alkalmazásnak a teljesítménybeli problémát, a felhasználók által jelentett vagy hello teljesítménymutatók változása látható.
* A probléma hello Azure tároló-infrastruktúra egy vagy több régióban van.
* Az alkalmazás hibás, vagy a felhasználók által jelentett, vagy megnövelheti a figyelheti hello hiba száma metrikák egyik által feltárt léptek fel.
* Fejlesztési és tesztelési során előfordulhat, hogy használni hello helyi storage emulator; kifejezetten a hello storage emulator toousage bizonyos problémák merülhetnek fel.

hello alábbi szakaszok felsorolják hello lépéseket kell követni toodiagnose, és minden ilyen négy a problémák elhárításához. a szakasz hello "[hibaelhárítási útmutatás]" Ez az útmutató későbbi részletgazdagabb a gyakori problémákat tapasztalhat.

### <a name="service-health-issues"></a>Szolgáltatás ügynökállapottal kapcsolatos hibákkal
Szolgáltatás állapotával kapcsolatos problémák jellemzően a vezérlőn kívül. Hello [Azure-portálon](https://portal.azure.com) Azure szolgáltatásokkal, beleértve a tárolási szolgáltatások folyamatos problémákkal kapcsolatos információkat biztosít. Ha választotta írásvédett Georedundáns tárolás a tárfiók létrehozása után, majd hello eseményben hello elsődleges helyen, éppen nem érhető el az adatok az alkalmazás átállítása volt ideiglenesen toohello írásvédett másolatként hello másodlagos helyen. toodo, ez az alkalmazás képes tooswitch hello elsődleges és másodlagos tárolóhelyek használata között, és képes toowork csak olvasható adatok csökkentett üzemmódban kell. hello Azure Storage ügyfélkódtáraival toodefine által olvasható másodlagos tárolóból, abban az esetben, ha egy elsődleges tárolási való olvasás sikertelen újrapróbálkozási házirendje lehetővé teszi. Az alkalmazás is van szüksége arról, hogy hello adatok hello másodlagos helyen idővel konzisztenssé toobe. További információkért lásd: hello blogbejegyzésben [Azure tárolási redundancia lehetőségek és az írásvédett Georedundáns redundáns tárolás](https://blogs.msdn.microsoft.com/windowsazurestorage/2013/12/11/windows-azure-storage-redundancy-options-and-read-access-geo-redundant-storage/).

### <a name="performance-issues"></a>Teljesítménnyel kapcsolatos problémák
lehet, hogy egy alkalmazás teljesítménye hello szubjektív, különösen a felhasználók szempontjából. Ezért fontos toohave alapterv metrikák elérhető toohelp azonosította amennyiben előfordulhat, hogy a teljesítménybeli problémát. Számos tényező befolyásolhatja hello teljesítmény az Azure storage szolgáltatás hello ügyfél alkalmazás szempontjából. Ezek a tényezők hello társzolgáltatás, hello ügyfél vagy hello hálózati infrastruktúra; előfordulhat, hogy működik. ezért ez azért fontos toohave hello teljesítményprobléma hello származásának azonosítására szolgáló stratégiát.

Miután azonosította hello hello teljesítményprobléma hello metrikákat a hello oka valószínűleg helyét, ezután a használata hello napló fájlok toofind részletes információkat toodiagnose, és további hello probléma elhárításához.

szakasz hello "[hibaelhárítási útmutatás]" kapcsolatos néhány általános teljesítmény kapcsolatban, hogy ez az útmutató későbbi biztosít találkozhat.

### <a name="diagnosing-errors"></a>Hibák diagnosztizálása
Az alkalmazás felhasználók értesítheti a hello ügyfélalkalmazás által jelentett hibák. Storage mérőszámainak rögzíti számát a tárolási szolgáltatások különböző típusok is, mint **NetworkError**, **ClientTimeoutError**, vagy **AuthorizationError**. Storage Metrics csak számát is különböző típusok rögzíti, miközben szerezhet be további részletes információkat az egyes kérelmek kiszolgálóoldali, az ügyféloldali és hálózati naplók megvizsgálásával. Hello hello tároló szolgáltatás által visszaadott HTTP-állapotkód rendszerében általában arra utal, hogy hello kérelem sikertelenségének okát.

> [!NOTE]
> Ne feledje, hogy számíthat toosee időszakos hibák: például tootransient hálózati problémák miatt hibák, vagy az alkalmazáshibák.
> 
> 

hello következőket hasznosak a tárolással kapcsolatos állapotának és kódjainak:

* [Közös REST API hibakódok](http://msdn.microsoft.com/library/azure/dd179357.aspx)
* [BLOB szolgáltatás hibakódok](http://msdn.microsoft.com/library/azure/dd179439.aspx)
* [Várólista hibakódjai](http://msdn.microsoft.com/library/azure/dd179446.aspx)
* [Tábla hibakódjai](http://msdn.microsoft.com/library/azure/dd179438.aspx)
* [Fájl hibakódjai](https://msdn.microsoft.com/library/azure/dn690119.aspx)

### <a name="storage-emulator-issues"></a>Tárolási emulátor problémák
hello Azure SDK magában foglalja a storage emulatort, egy fejlesztő munkaállomás futtathatja. Az emulátor hello viselkedését hello Azure storage szolgáltatások többsége szimulál, és akkor hasznos, fejlesztési és tesztelési során, így már toorun hello nélkül az Azure storage-szolgáltatásokat használó alkalmazások kell az Azure-előfizetés és Azure storage-fiók.

hello "[hibaelhárítási útmutatás]" című szakaszában talál néhány gyakori problémát észlelt a hello storage emulator használatával ismerteti.

### <a name="storage-logging-tools"></a>Tárolási naplózási eszközök
Tároló-naplózás biztosítja a kiszolgálóoldali naplózása tárolási kérelmeket az Azure-tárfiókot. Hogyan tooenable kiszolgálóoldali naplózási és hozzáférési hello napló adatok kapcsolatos további információkért lásd: [tárolási naplózás engedélyezése és használata naplóadatok](http://go.microsoft.com/fwlink/?LinkId=510867).

hello Storage ügyféloldali kódtára a .NET toocollect ügyféloldali naplóadatokat kapcsolódó toostorage műveleteket végzi el az alkalmazás lehetővé teszi. További információkért lásd: [ügyféloldali naplózás hello .NET a Storage ügyféloldali kódtára](http://go.microsoft.com/fwlink/?LinkId=510868).

> [!NOTE]
> Bizonyos esetekben (például a SAS-hitelesítési hibák) a felhasználó, amelynek nincs kérelem adatai az található hello kiszolgálóoldali tárolási naplófájljai hiba előfordulhat, hogy jelentést. Hello naplózás képességei hello a Storage ügyféloldali kódtára tooinvestigate használja, ha hello hello a probléma oka az ügyfél hello, vagy használja a hálózati eszközök tooinvestigate hello hálózat figyelése.
> 
> 

### <a name="using-network-logging-tools"></a>Hálózati naplózási eszközök használatával
Rögzítheti a hello forgalmat közötti hello ügyfél és kiszolgáló tooprovide részletes információk hello adatok hello ügyfél és kiszolgáló cseréjét és az alapjául szolgáló hálózati körülmények hello. Hasznos hálózati naplózási eszközök a következők:

* [Fiddler](http://www.telerik.com/fiddler) egy szabad webalkalmazás-proxy, amely lehetővé teszi tooexamine hello fejlécek és a HTTP és HTTPS kérés- és üzenetek adattartalom hibakeresés. További információkért lásd: [1. függelékében: Fiddler használatával toocapture HTTP és HTTPS-forgalom](#appendix-1).
* [Microsoft Hálózatfigyelő (Netmon)](http://www.microsoft.com/download/details.aspx?id=4865) és [Wireshark](http://www.wireshark.org/) vannak szabad hálózati protokoll lekérdezések, amelyek lehetővé teszik tooview csomag azoknak a hálózati protokollok széles köre. Wireshark kapcsolatos további információkért lásd: "[2. függelékben: használatával Wireshark toocapture hálózati forgalom](#appendix-2)".
* Microsoft Message Analyzert egy eszköz, amely felülírja a Netmon és, hogy ezenkívül toocapturing hálózati csomagadatok, Microsoft tooview segít, és más eszközök rögzítése hello naplóadatokat elemzése. További információkért lásd: "[függelék: 3: Microsoft Message Analyzert használatával toocapture hálózati forgalom](#appendix-3)".
* Ha azt szeretné, hogy a hálózati kapcsolat tesztelése toocheck, hogy az ügyfélszámítógép hello hálózaton keresztül kapcsolódhatnak toohello az Azure storage szolgáltatás tooperform, nem ehhez a hello szabványos **ping** eszköz hello ügyfélen. Azonban használhat hello [ **tcping** eszköz](http://www.elifulkerson.com/projects/tcping.php) toocheck kapcsolat.

Sok esetben hello naplóadatait tárolási naplózás és a Storage ügyféloldali kódtára hello elegendő toodiagnose problémát, de bizonyos helyzetekben szükség lehet a részletes információkat biztosít a hálózati naplózási eszközök hello. Például Fiddler tooview használatával újrapróbálja a tárolási műveletek a HTTP és HTTPS üzenetek lehetővé teszi, hogy tooview fejlécének és adattartalmának bontása adatokat küldött tooand hello a tárolási szolgáltatások, amelyek lehetővé teszik hogy tooexamine egy ügyfél alkalmazások. Protokoll elemzőkkel Wireshark például így már tooview TCP-adatok, amelyek lehetővé teszik a tootroubleshoot, elveszett csomagokat és a kapcsolódási problémák hello csomagok szinten működik. Az Üzenetelemző HTTP és a TCP rétegek is működnek.

## <a name="end-to-end-tracing"></a>Végpont nyomkövetés
Végpont nyomkövetési naplófájlok számos használatát egy olyan hasznos eljárás kapcsolatos lehetséges problémákat vizsgálja. A metrikák adatokból hello dátum-/ időinformációk utalhat, hogy hol hello naplófájlba hello keresése toostart részletes információk segítik hello problémával kapcsolatos hibaelhárítás elősegítéséhez védelemként is alkalmazhatják.

### <a name="correlating-log-data"></a>Naplóadatok válaszoknak az összekapcsolása
Az ügyfélalkalmazásokból napló megtekintésekor követi a hálózat, és kiszolgálóoldali tárolási naplózás azt kritikus toobe képes toocorrelate kérelmek hello különböző naplófájl. hello naplófájlokat, amelyek hasznosak a korrelációs azonosítóként különböző mezők számú tartozik. hello ügyfélkérelem-azonosító hello leghasznosabb mező toouse toocorrelate bejegyzések hello különböző naplókat. Egyes esetekben azonban lehet hasznos toouse hello server kérelemazonosító vagy időbélyeg helyi időre. hello következő szakaszokban ezekről a beállításokról további részleteket.

### <a name="client-request-id"></a>Ügyfélkérelem-azonosító
a Storage ügyféloldali kódtára hello automatikusan létrehoz minden kérelem esetén egyedi ügyfélkérelem-azonosító.

* Hello ügyféloldali naplót, hogy a Storage ügyféloldali kódtára hello hoz létre, hello ügyfélkérelem-azonosító szerepel hello **ügyfélkérelem-azonosító** mezőjét, minden naplóbejegyzés vonatkozó toohello kérelmet.
* A Fiddler rögzített például egy hálózati nyomkövetést, hello ügyfélkérelem-azonosító kérelemüzenetek látható mint hello **x-ms-client-request-id** HTTP-fejléc értéke.
* Hello kiszolgálóoldali tárolási naplózás naplóban hello ügyfélkérelem-azonosító hello ügyfél kérelem azonosító oszlopban jelenik meg.

> [!NOTE]
> Lehetőség a több kérelmek tooshare hello azonos ügyfélkérelem-azonosító, mert hello ügyfelet hozzá lehet rendelni ezt az értéket (bár a Storage ügyféloldali kódtára hello automatikusan hozzárendel egy új értéket). Újrapróbálkozások hello ügyfélről hello esetben bármilyen kísérlet megosztása hello azonos ügyfélkérelem-azonosító. Hello ügyfélről küldött kötegelt hello esetben hello kötegelt egyetlen ügyfélkérelem-azonosító tartozik.
> 
> 

### <a name="server-request-id"></a>Kiszolgáló azonosítója
hello társzolgáltatás kérelem kiszolgálóazonosítók automatikusan létrehozza.

* Hello kiszolgálóoldali tárolási naplózás naplóban, hello server kérelemazonosító megjelenik hello **Kérelemazonosító fejléc** oszlop.
* A Fiddler rögzített például egy hálózati nyomkövetést, hello server kérelemazonosító jelenik meg a válaszüzenetek hello **az x-ms-request-id** HTTP-fejléc értéke.
* Hello ügyféloldali naplót, hogy a Storage ügyféloldali kódtára hello hoz létre, hello server kérelemazonosító megjelenik hello **művelet szöveg** hello kiszolgálóválasz részleteit megjelenítő hello naplóbejegyzés oszlopában.

> [!NOTE]
> hello tároló szolgáltatás mindig rendeli hozzá egyedi kiszolgálói kérelem azonosítója tooevery kérelmet kap, így minden újrapróbálkozások hello ügyfélről és minden műveletet a kötegben szereplő egy egyedi kiszolgálói kérelem azonosítója.
> 
> 

Ha hello okoz-e a Storage ügyféloldali kódtára a **StorageException** hello ügyfél hello **RequestInformation** tulajdonsága tartalmazza egy **RequestResult** objektum, amely tartalmazza a **ServiceRequestID** tulajdonság. Emellett egy **RequestResult** objektum egy **OperationContext** példány.

az alábbi hello mintakód bemutatja, hogyan tooset egyéni **ClientRequestId** csatolásával érték egy **OperationContext** objektum hello kérelem toohello tároló szolgáltatást. Azt is bemutatja, hogyan tooretrieve hello **ServerRequestId** hello válaszüzenetet közötti értéket.

```csharp
//Parse hello connection string for hello storage account.
const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Create an Operation Context that includes custom ClientRequestId string based on constants defined within hello application along with a Guid.
OperationContext oc = new OperationContext();
oc.ClientRequestID = String.Format("{0} {1} {2} {3}", HOSTNAME, APPNAME, USERID, Guid.NewGuid().ToString());

try
{
    CloudBlobContainer container = blobClient.GetContainerReference("democontainer");
    ICloudBlob blob = container.GetBlobReferenceFromServer("testImage.jpg", null, null, oc);  
    var downloadToPath = string.Format("./{0}", blob.Name);
    using (var fs = File.OpenWrite(downloadToPath))
    {
        blob.DownloadToStream(fs, null, null, oc);
        Console.WriteLine("\t Blob downloaded toofile: {0}", downloadToPath);
    }
}
catch (StorageException storageException)
{
    Console.WriteLine("Storage exception {0} occurred", storageException.Message);
    // Multiple results may exist due tooclient side retry logic - each retried operation will have a unique ServiceRequestId
    foreach (var result in oc.RequestResults)
    {
            Console.WriteLine("HttpStatus: {0}, ServiceRequestId {1}", result.HttpStatusCode, result.ServiceRequestID);
    }
}
```

### <a name="timestamps"></a>Időbélyeg helyi időre
Időbélyeg helyi időre is használható toolocate naplóbejegyzések, azonban az legyen óvatos bármely óra eltérésére hello ügyfél és kiszolgáló, előfordulhat, hogy létezik között. Plusz vagy mínusz 15 percenként megfelelő kiszolgálóoldali bejegyzések hello időbélyeg hello ügyfélen alapján kell keresni. Ne feledje, hogy hello hello blobot tartalmazó metrikák jelzi hello időtartomány hello metrikáihoz tárolt hello blob; blob metaadatait Ez akkor hasznos, ha sok metrikák blobokat hello azonos perc vagy óra.

## <a name="troubleshooting-guidance"></a>Hibaelhárítási útmutató
Ez a szakasz segítséget hello diagnosztizálása és hello Azure storage szolgáltatások használata során felmerülő egyes hello kapcsolatos gyakori hibák elhárítása az alkalmazás. Használjon hello toolocate hello információk vonatkozó tooyour meghatározott probléma az alábbi listából.

**Hibaelhárítási döntési fája**

---
A probléma toohello teljesítmény hello tárolási szolgáltatások közül az egyik kapcsolatban áll?

* [metrika AverageE2ELatency magas és alacsony AverageServerLatency]
* [A metrika alacsony AverageE2ELatency és alacsony AverageServerLatency, de hello ügyfél tapasztal nagy késleltetésű]
* [A mérőszámok magas AverageServerLatency értéket mutatnak]
* [Váratlan késést tapasztal az üzenetsorban található üzenetek kézbesítésekor]

---
A probléma toohello rendelkezésre állási hello tárolási szolgáltatások közül az egyik kapcsolatban áll?

* [PercentThrottlingError metrika növelését]
* [PercentTimeoutError metrika növelését]
* [A mérőszámok emelkedő PercentNetworkError értéket mutatnak]

---
 Az ügyfélalkalmazás fogadja-e egy HTTP-válaszok aránya (például a 404-es) 4XX egy társzolgáltatás?

* [hello ügyfél HTTP 403 (tiltott) üzeneteket fogad]
* [hello ügyfél (nem található) HTTP 404 üzeneteket fogad]
* [hello ügyfél HTTP 409 (Ütközés) üzeneteket fogad]

---
[metrika alacsony PercentSuccess vagy analytics naplóbejegyzések rendelkezik műveletek tranzakció állapota ClientOtherErrors]

---
[Teljesítmény-mérőszámait váratlan növekedését megjelenítése a tárolási kapacitás-használat]

---
[Váratlan vesznek el virtuális gépeken, amelyek csatlakoztatott virtuális merevlemezek nagy számú tapasztal]

---
[A probléma merül fel a fejlesztési vagy tesztelési hello storage emulator használatával]

---
[Áll kapcsolatban problémák hello Azure SDK telepítése a .NET-hez]

---
[Van egy másik probléma storage szolgáltatással]

---
### <a name="metrics-show-high-AverageE2ELatency-and-low-AverageServerLatency"></a>Metrikák AverageE2ELatency magas és alacsony AverageServerLatency megjelenítése
hello az alábbi ábra hello [Azure-portálon](https://portal.azure.com) eszköz figyelési mutat egy példát, amelyben hello **AverageE2ELatency** jelentősen nagyobb, mint hello **AverageServerLatency**.

![][4]

Vegye figyelembe, hogy hello társzolgáltatás csak kiszámítja hello metrika **AverageE2ELatency** a sikeres kéréseket, és eltérően **AverageServerLatency**, magában foglalja a hello ügyfél szükséges toosend hello hello idő adatok és nyugtázási fogadása hello tároló szolgáltatást. Ezért közötti különbségek **AverageE2ELatency** és **AverageServerLatency** lehet vagy toohello ügyfél alkalmazás éppen miatt lassú toorespond, vagy onnan tooconditions hello hálózaton.

> [!NOTE]
> Megtekintheti továbbá **E2ELatency** és **ServerLatency** hello tárolási naplózás az egyes tárolási műveletek az adatok naplózása.
> 
> 

#### <a name="investigating-client-performance-issues"></a>Ügyfél teljesítménnyel kapcsolatos problémák vizsgálatában
Hello ügyfél lassan válaszol a lehetséges okok közé tartoznak a rendelkezésre álló kapcsolatok vagy szálak korlátozott számú, illetve ha éppen kevés az erőforrás, például a Processzor, a memória vagy a hálózati sávszélesség. Képes tooresolve hello probléma lehet hello ügyfél kód toobe hatékonyabb (például az aszinkron hívásai toohello tárolási szolgáltatás használatával) módosításával, illetve a nagyobb virtuális gépek (a több maggal és memória) használatával.

Hello tábla és a queue szolgáltatások, a hello Nagle algoritmus is eredményezheti, hogy magas **AverageE2ELatency** túl mint**AverageServerLatency**: további információk: hello utáni [Nagle meg Algoritmus nem rövid kis kérelmek felé](http://blogs.msdn.com/b/windowsazurestorage/archive/2010/06/25/nagle-s-algorithm-is-not-friendly-towards-small-requests.aspx). Hello segítségével letilthatja hello Nagle algoritmus kódban **ServicePointManager** hello osztály **System.NET névtérbeli** névtér. Akkor tegye ezt előtt nyissa meg a várólista-szolgáltatás az alkalmazásban, mert ez nem befolyásolja a már kapcsolatok vagy bármely hívások toohello tábla végez. hello alábbi példa származik hello **Application_Start** a feldolgozói szerepkör metódust.

```csharp
var storageAccount = CloudStorageAccount.Parse(connStr);
ServicePoint tableServicePoint = ServicePointManager.FindServicePoint(storageAccount.TableEndpoint);
tableServicePoint.UseNagleAlgorithm = false;
ServicePoint queueServicePoint = ServicePointManager.FindServicePoint(storageAccount.QueueEndpoint);
queueServicePoint.UseNagleAlgorithm = false;
```

Ellenőrizni kell hello ügyféloldali naplók toosee hány kérelmeket küld az ügyfélalkalmazást, és ellenőrizze a általános .NET-hez kapcsolódó szűk keresztmetszetek a ügyfélprogram, pl. a CPU, a .NET szemétgyűjtés, a hálózat kihasználtságának vagy a memória. Kiindulási pontként .NET ügyfélalkalmazások hibaelhárításhoz, lásd: [profilkészítési, hibakeresés és nyomkövetés](http://msdn.microsoft.com/library/7fe0dd2y).

#### <a name="investigating-network-latency-issues"></a>Hálózati késési problémák kivizsgálása
Hello hálózati okozta magas végpontok közötti késés általában tootransient feltételek miatt. Mindkét ideiglenes és állandó hálózati problémák, például az eldobott csomagok használatával megvizsgálhatja például Wireshark vagy a Microsoft Message Analyzert eszközök segítségével.

Wireshark tootroubleshoot hálózati problémák használatával kapcsolatos további információkért lásd: "[2 függelék: Wireshark toocapture hálózati forgalom használ]."

Microsoft Message Analyzert tootroubleshoot hálózati problémák használatával kapcsolatos további információkért lásd: "[3 függelék: a Microsoft Message Analyzert toocapture hálózati forgalom használ]."

### <a name="metrics-show-low-AverageE2ELatency-and-low-AverageServerLatency"></a>A metrika alacsony AverageE2ELatency és alacsony AverageServerLatency, de hello ügyfél tapasztal nagy késleltetésű
Ebben a forgatókönyvben hello ennek legvalószínűbb oka, a késés hello tárolási kérelmeket hello társzolgáltatás elérése. Ki kell vizsgálni, hogy miért hello ügyfél kérelmeinek nem létesített azt toohello blob szolgáltatáson keresztül.

Egy hello ügyfél küldött kérésekkel késleltetése oka lehet, hogy vannak-e rendelkezésre álló kapcsolatok vagy szálak korlátozott számú.

Ellenőrizze hogy hello ügyfél végrehajtása több újrapróbálkozások, és vizsgálja meg a hello OK, ha ez helyzet hello is. toodetermine hello ügyfél több újrapróbálkozások hajt végre, hogy a következőket teheti:

* Ellenőrizze a hello tárolási analitika eseménynaplóit. Ha több próbálkozás van folyamatban, látni fogja a több művelet található hello azonos ügyfélkérelem-azonosító, de a másik kiszolgáló kérhetnek azonosítók.
* Ellenőrizze a hello ügyfél eseménynaplóit. A részletes naplózás tájékoztatja arról, hogy újbóli kísérlet történt.
* A kód hibakereséséhez, és ellenőrizze a hello hello tulajdonságainak **OperationContext** hello kérelemhez társított objektumot. Ha a rendszer megpróbálja újból végrehajtani a hello művelettel rendelkezik, hello **RequestResults** tulajdonság több egyedi kiszolgálói kérelem azonosítókat tartalmazza. Is ellenőrizze hello kezdési és befejezési időpontja egyes kérésekre vonatkozóan. További információkért lásd: hello kódminta hello szakaszban [Server Kérelemazonosító].

Ha nincsenek problémák hello ügyfél, ki kell vizsgálni a lehetséges hálózati problémák, például a csomagveszteség. Eszközök, például Wireshark vagy a Microsoft Message Analyzert tooinvestigate hálózati probléma is használhatja.

Wireshark tootroubleshoot hálózati problémák használatával kapcsolatos további információkért lásd: "[2 függelék: Wireshark toocapture hálózati forgalom használ]."

Microsoft Message Analyzert tootroubleshoot hálózati problémák használatával kapcsolatos további információkért lásd: "[3 függelék: a Microsoft Message Analyzert toocapture hálózati forgalom használ]."

### <a name="metrics-show-high-AverageServerLatency"></a>Metrika magas AverageServerLatency
A magas hello esetben **AverageServerLatency** blob letöltési kérelmek, érdemes használni a hello toosee tárolási naplózás naplózza, ha ugyanaz a blob-(vagy blobok beállítása) hello ismételt kérés kapcsolódik. BLOB vonatkozó kérések feltöltésére, ki kell vizsgálni, milyen blokk méretét hello ügyfél használ (például legfeljebb 64 KB méretű eredményezhet terhek kivéve hello beolvassa szerepelnek blokkok kevesebb, mint 64K adattömbök), és ha több ügyfél feltölteni blokkol toohello azonos BLOB párhuzamosan. Azt is ellenőrizze igényeiben jelentkező hello kérelmek száma, amelyek egy második méretezhetőségi célok hello túllépését eredményezi a hello perc metrikáját: is láthatja, hogy "[PercentTimeoutError metrika növelését]."

Ha magas **AverageServerLatency** az blob letöltési kérelmek nincs vannak ismételt kérelmek hello azonos blob vagy elosztott blobokat, majd meg kell vegye figyelembe a blobok Azure Cache használatával gyorsítótárazás vagy hello Azure Content Delivery Network (CDN). A feltöltési kérés kapcsolódik a nagyobb blokkméret használatával javíthatja a hello átviteli sebességet. A lekérdezések tootables esetén is lehetséges tooimplement ügyféloldali gyorsítótárazás hello azonos műveletek lekérdezése, és ahol hello végző ügyfelek adatok ritkán módosulnak.

Magas **AverageServerLatency** értékek rosszul megtervezett táblák tünete is lehet, vagy az, hogy a Keresés eredménye, vagy hajtsa végre a hello lekérdezések hozzáfűzése/illesztenie elleni mintát. Lásd: "[PercentThrottlingError metrika növelését]" További információ.

> [!NOTE]
> Egy átfogó ellenőrzőlista teljesítmény ellenőrzőlistát itt találja: [Microsoft Azure tárolási teljesítmény és méretezhetőség ellenőrzőlista](storage-performance-checklist.md).
> 
> 

### <a name="you-are-experiencing-unexpected-delays-in-message-delivery"></a>A várólista üzenetkézbesítést váratlan késést tapasztal
Ha hello idő késleltetés tapasztalja alkalmazás hozzáadja egy üzenet tooa várólista és hello ideje hello várólistából elérhető tooread válik, majd a következő lépéseket toodiagnose hello probléma hello kell tennie:

* Ellenőrizze, hogy hello alkalmazás sikeresen hozzáadásával hello üzenetek toohello várólista. Ellenőrizze, hogy hello alkalmazás nincs újrapróbálkozás hello **AddMessage** metódus többször előtt sikeresen lezajlottak. a Storage ügyféloldali kódtára naplók hello bármely, a tárolási műveletek miatti ismételt próbálkozás jeleníti meg.
* Győződjön meg arról nem óra eltérésére hello feldolgozói szerepkör, amely hello toohello üzenetsor és hello feldolgozói szerepkör, amely megkönnyíti a hello várólistából üdvözlőüzenetére olvasó jelenik meg, mintha késik a feldolgozása között.
* Ellenőrizze, hogy ha hello feldolgozói szerepkör köszönőüzenetei olvasó hello várólistából hibás-e. Ha a várólista ügyfél hello **GetMessage** módszer azonban nem jár sikerrel, a nyugtázást toorespond, üdvözlőüzenetére marad láthatatlan hello várólista hello **invisibilityTimeout** időszakának lejártáig. Ezen a ponton üdvözlőüzenetére feldolgozásának újra elérhetővé válik.
* Ellenőrizze, hogy ha időbeli hello várólista hossza nő. Ez akkor fordulhat elő, ha nem rendelkezik elegendő munkavállalók elérhető tooprocess összes hello üzenetek, hogy más munkavállalók hello várólista helyez. Azt is ellenőrizze hello metrikák toosee Ha törlése sikertelen lekérdezések és hello created száma üzeneteken, ami azt jelezheti ismétlődő sikertelen próbálkozások toodelete üdvözlőüzenetére.
* Tanulmányozza hello tárolási naplózása, amelyek rendelkeznek a vártnál nagyobb várólista műveletekhez **E2ELatency** és **ServerLatency** értékek idő szokásosnál hosszabb időszakra.

### <a name="metrics-show-an-increase-in-PercentThrottlingError"></a>PercentThrottlingError metrika növelését
Sávszélesség-szabályozási hiba történik, amikor a túllépi hello méretezhetőségi célok tároló szolgáltatás. hello tárolási szolgáltatásnak nincs a tooensure, amely nem egy ügyfél vagy bérlői hello szolgáltatást használhatják hello költségén mások. További információkért lásd: [Azure Storage méretezhetőségi és teljesítménycéloknak](storage-scalability-targets.md) méretezhetőségi célok storage-fiókok és a storage-fiókok partíciókról teljesítménycéloknak.

Ha hello **PercentThrottlingError** metrika növelését megjelenítése szabályozási hiba miatt sikertelen kérelemtípusokat hello százalékában, egy két forgatókönyv tooinvestigate van szüksége:

* [PercentThrottlingError átmeneti növekedése]
* [Állandó növekedése PercentThrottlingError hiba]

Megnövelheti a **PercentThrottlingError** gyakran kerül sor, hello azonos tárolási kérések száma hello növekedése idő, vagy ha kezdetben betölteni a alkalmazás teszteléséhez. Ez lehet, hogy is manifest magát a hello ügyfél "503-as kiszolgáló foglalt" vagy "500 művelet időtúllépése" HTTP állapotüzenete tárolási műveletek.

#### <a name="transient-increase-in-PercentThrottlingError"></a>PercentThrottlingError átmeneti növekedése
Ha Ön igényeiben jelentkező hello értékének **PercentThrottlingError** , hogy a nagy aktivitású időszakok hello alkalmazás egybe, meg kell valósítania az ügyfél egy exponenciális (lineáris) vissza az újbóli próbálkozások stratégiával ki: a a rendszer csökkenthető hello azonnali terhelése hello partíció, és segítenek az alkalmazás toosmooth kimenő forgalmat a teljesítményt. Hogyan tooimplement újrapróbálkozási házirendekkel hello a Storage ügyféloldali kódtára kapcsolatos további információkért lásd: [Microsoft.WindowsAzure.Storage.RetryPolicies Namespace](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.retrypolicies.aspx).

> [!NOTE]
> Is találkozhat hello értékének igényeiben jelentkező **PercentThrottlingError** , amely nem esik egybe hello alkalmazáshoz a nagy aktivitású időszakokkal: hello Itt a legvalószínűbb ok az partíciók tooimprove terhelés áthelyezését hello társzolgáltatás terheléselosztás.
> 
> 

#### <a name="permanent-increase-in-PercentThrottlingError"></a>Állandó növekedése PercentThrottlingError hiba
Ha Ön folyamatosan magas értéket **PercentThrottlingError** a végleges növelését a tranzakció-köteteket, vagy amikor a kezdeti betöltés hajtja végre a következő teszteli, az alkalmazásra, majd tooevaluate van szüksége az alkalmazás hogyan használja adattárolási partíciókat, és hogy közeledik hello méretezhetőségi célok tárfiókok esetén. Például ha Ön szabályozás hibákat a várólista (amely számít egyetlen partícióra), majd érdemes lehet használni további várólisták toospread hello tranzakciók több partíciót között. Ön szabályozás hibák táblán, ha szüksége használja egy másik particionálási sémát toospread tooconsider a tranzakciók között több partíciót széles körének partíciókulcs-értékek használatával. Ennek a problémának egy közös ok: hello illesztenie hozzáfűzése elleni mintát ahol hello dátum kiválasztható hello partíciós kulcs nevével, és ezután egy adott napon minden adat tooone partíció: terhelés, ennek eredményeként a írási szűk keresztmetszetek. Fontolja meg egy másik particionálási tervezési vagy mérlegelje, hogy a blob storage használatával jobb megoldás lehet kell. Ellenőrizze, ha hello szabályozás miatt a forgalom igényeiben jelentkező végbemegy, és vizsgálja meg a minta kérelem simítás módokat is.

Ha a tranzakciók szét több partíciót, továbbra is kell tudomást hello méretezhetőségi korlátok hello tárfiók beállítása. Például ha tíz várólisták használt minden egyes feldolgozási hello legfeljebb 2000 1KB-üzenetek / másodperc, akkor lesz hello általános legfeljebb 20 000 üzenetek / másodperc hello tárfiók. Ha tooprocess másodpercenként több mint 20 000 entitások, érdemes lehet több tárfiókot használni. Meg kell is figyelembe kell vennie, hogy a kérelmek hello méretétől és entitások amikor hello társzolgáltatás az ügyfelek azelőtt gyorsítja fel hatással van: Ha nagyobb kérelmek és entitások, akkor előfordulhat, hogy lehet halmozódni hamarabb.

Nem elég hatékony Lekérdezéstervezés is okozhatja toohit hello méretezhetőségi korlátok táblázat partíciókat. Például egy szűrő, amely egy partíció csak kiválaszt egy százalék hello entitások, de, amely megvizsgálja egy partíció összes hello entitásokat tartalmazó lekérdezést kell tooaccess minden entitáshoz. Minden entitás, olvassa el hello partícióban; tranzakciók száma is beleszámít. ezért könnyen képes elérni hello méretezhetőségi célok.

> [!NOTE]
> A teljesítmény tesztelés egyetlen lekérdezés nem elég hatékony tervet, az alkalmazás kódproblémájáról van.
> 
> 

### <a name="metrics-show-an-increase-in-PercentTimeoutError"></a>PercentTimeoutError metrika növelését
A metrika növekedése **PercentTimeoutError** a tárolási szolgáltatások egyike. A hello azonos időben, hello ügyfél nagyszámú "500 művelet időtúllépése" HTTP állapotüzeneteket fogad a tárolási műveletek.

> [!NOTE]
> Ideiglenesen szolgáltatásként hello tárolási időtúllépést kiegyensúlyozza kérelmek betölteni egy partíció tooa új kiszolgálóra helyezi jelenhet meg.
> 
> 

Hello **PercentTimeoutError** mérőszáma összevonható a metrikák a következő hello: **ClientTimeoutError**, **AnonymousClientTimeoutError**,  **SASClientTimeoutError**, **ServerTimeoutError**, **AnonymousServerTimeoutError**, és **SASServerTimeoutError**.

hello kiszolgáló időtúllépése hello kiszolgálón hiba okozza. ügyfél-időtúllépések hello fordulhat elő, mert hello kiszolgálón művelet túllépte hello megadott hello ügyfél; például egy ügyfél a Storage ügyféloldali kódtára hello használatával állíthatja be művelet időtúllépés hello segítségével **ServerTimeout** hello tulajdonságának **QueueRequestOptions** osztály.

Kiszolgáló időtúllépése hello tároló szolgáltatás, amely további vizsgálatot igényel problémát jelez. Metrikák toosee is használhatja, ha hogy elérte-e hello méretezhetőségi korlátok hello szolgáltatást és tooidentify bármely igényeiben jelentkező a forgalmat, a probléma okozza. Ha hello probléma időszakos, miatt lehet tooload terheléselosztás tevékenység hello szolgáltatásban. Hello probléma állandó, és nem okozza az alkalmazás szerezze meg a hello méretezhetőségi korlátok hello szolgáltatást, ha előlépteti kell egy támogatási probléma. Az ügyfél-időtúllépések döntse el, ha hello időtúllépés van beállítva megfelelő értéket a tooan hello ügyfél pedig vagy változás hello időtúllépés értékének megadása a hello ügyfél, vagy vizsgálja meg, hogyan javíthatja hello teljesítmény hello műveletek hello storage szolgáltatásban a Példa a tábla lekérdezések optimalizálása, vagy az üzenetek hello méretének csökkentését.

### <a name="metrics-show-an-increase-in-PercentNetworkError"></a>PercentNetworkError metrika növelését
A metrika növekedése **PercentNetworkError** a tárolási szolgáltatások egyike. Hello **PercentNetworkError** mérőszáma összevonható a metrikák a következő hello: **NetworkError**, **AnonymousNetworkError**, és  **SASNetworkError**. Ezek akkor történik, ha a hello tároló szolgáltatás észleli a hálózati hiba, ha hello ügyfél tárolási kérelmet küld.

hello a hiba leggyakoribb oka az ügyfél leválasztása időtúllépés járjon le a hello tároló szolgáltatást. Az ügyfél toounderstand miért és mikor hello ügyfél hello társzolgáltatás bontja a kapcsolatot a hello kódot kell vizsgálni. Wireshark, Microsoft Message Analyzert vagy Tcping tooinvestigate hálózati problémák hello ügyfélről is használható. Ezek az eszközök ismertetett hello [mellékletek].

### <a name="the-client-is-receiving-403-messages"></a>hello ügyfél HTTP 403 (tiltott) üzeneteket fogad
Ha az ügyfélalkalmazást szűrész HTTP 403 (tiltott) hibák, ennek valószínű oka, hogy hello az ügyfél által használt egy lejárt közös hozzáférésű Jogosultságkód (SAS), tárolási kérelmet küld a (bár egyéb lehetséges okok közé tartoznak az óra eltérésére, érvénytelen kulcsokat, és üres fejlécek). Ha lejárt SAS-kulcs hello OK, nem látják azokat a bejegyzéseket hello kiszolgálóoldali tárolási naplózás naplóadatokat. hello alábbi táblázat megmutatja, amely bemutatja, hogy ez a probléma lépett fel a Storage ügyféloldali kódtára hello által generált hello ügyféloldali naplóból:

| Forrás | A részletességi | A részletességi | Ügyfélkérelem-azonosító | A művelet szöveg |
| --- | --- | --- | --- | --- |
| Microsoft.WindowsAzure.Storage |Információ |3 |85d077ab-... |A hely elsődleges hely módban PrimaryOnly művelet elindítása. |
| Microsoft.WindowsAzure.Storage |Információ |3 |85d077ab-... |Indítása szinkron kérelem toohttps://domemaildist.blob.core.windows.netazureimblobcontainer/blobCreatedViaSAS.txt?sv=2014-02-14&amp;sr = c&amp;si = mypolicy&amp;sig OFnd4Rd7z01fIvh 2BmcR6zbudIH2F5Ikm % = 2FyhNYZEmJNQ % 3D&amp;api-version = 2014-02-14. |
| Microsoft.WindowsAzure.Storage |Információ |3 |85d077ab-... |Válaszra való várakozás közben. |
| Microsoft.WindowsAzure.Storage |Figyelmeztetés |2 |85d077ab-... |Kivétel keletkezett a válaszra való várakozás közben: hello távoli kiszolgáló hibát adott vissza: (403-as) tiltott... |
| Microsoft.WindowsAzure.Storage |Információ |3 |85d077ab-... |A válasz érkezett. Állapotkód = 403-as, a kérelem azonosítója = 9d67c64a-64ed-4b0d-9515-3b14bbcdc63d, Content-MD5 = ETag =. |
| Microsoft.WindowsAzure.Storage |Figyelmeztetés |2 |85d077ab-... |Kivétel lépett fel az hello művelet közben: hello távoli kiszolgáló hibát adott vissza: (403-as) tiltott... |
| Microsoft.WindowsAzure.Storage |Információ |3 |85d077ab-... |Annak ellenőrzése, ha hello műveletet meg kell ismételni. Ismétlések száma = 0, HTTP-állapotkód = 403-as, kivétel = hello távoli kiszolgáló hibát adott vissza: (403-as) tiltott... |
| Microsoft.WindowsAzure.Storage |Információ |3 |85d077ab-... |tooPrimary hello helyen alapuló hello következő helyre lett állítva. |
| Microsoft.WindowsAzure.Storage |Hiba |1 |85d077ab-... |Újrapróbálkozási házirend nem engedélyezi az újrapróbálkozást. Sikertelen a távoli kiszolgálóval hello hibát adott vissza: (403-as) tiltott. |

Ebben az esetben kell vizsgálni, miért hello SAS-token érvényessége hello ügyfél küldi hello token toohello server előtt:

* Általában a kezdő időpont, amikor létrehoz egy SAS-t egy ügyfél toouse azonnal nincs beállítva. Ha kis óra különbségek közötti létrehozásának hello SAS használatával hello állomás hello a jelenlegi idő- és tárolószolgáltatások hello, akkor lehetséges, hogy hello tárolási szolgáltatás tooreceive SAS-kód, amely még nem érvényes.
* SAS-kód nem egy nagyon rövid lejárati időt kell beállítani. Ebben az esetben hello állomás létrehozásakor kis óra különbségei hello SAS és hello társzolgáltatás vezethet tooa SAS látszólag lejár a vártnál korábban.
* Hello hello SAS-kulcsot a version paramétert (például **sv = 2015-04-05**) hello használja a Storage ügyféloldali kódtára egyezés hello verzióját? Azt javasoljuk, hogy mindig használjon hello legújabb verziójának hello [a Storage ügyféloldali kódtára](https://www.nuget.org/packages/WindowsAzure.Storage/).
* A tárelérési kulcsok újragenerálása, ha ez bármely létező SAS-tokenje érvénytelenné tehetik. Erre akkor lehet problémát, ha az ügyfél alkalmazások toocache hosszú lejárati idővel rendelkező SAS-tokenje.

A Storage ügyféloldali kódtára toogenerate SAS-tokenje hello használ, majd esetén könnyen toobuild egy érvényes tokent. Azonban ha hello Storage REST API használatával, és hozhat létre kézzel hello SAS-tokenje figyelmesen olvassa el hello témakör [egy közös hozzáférésű Jogosultságkód hozzáférést delegálása](http://msdn.microsoft.com/library/azure/ee395415.aspx).

### <a name="the-client-is-receiving-404-messages"></a>hello ügyfél (nem található) HTTP 404 üzeneteket fogad
Ha hello ügyfélalkalmazás (nem található) HTTP 404 üzenetben kap hello kiszolgálójáról, ez azt jelenti, adott hello objektum hello ügyfél próbált toouse (például egy entitás, tábla, blob, tároló vagy várólista) nem létezik a hello tároló szolgáltatást. Számos, ennek lehetséges okai például:

* [hello ügyfél vagy egy másik folyamat korábban törölt hello objektum]
* [Egy közös hozzáférésű Jogosultságkód (SAS) hitelesítési hiba]
* [Ügyféloldali JavaScript-kód nem rendelkezik engedéllyel tooaccess hello objektum]
* [Hálózati hiba]

#### <a name="client-previously-deleted-the-object"></a>hello ügyfél vagy egy másik folyamat korábban törölt hello objektum
Forgatókönyvekben, ahol hello ügyfél megpróbál tooread, update vagy a társzolgáltatás általában az adatok könnyen tooidentify hello kiszolgálóoldali egy korábbi művelet hello objektum az adott törölt hello társzolgáltatás naplózza. Nagyon gyakran hello naplóadatokat jeleníti meg, hogy egy másik felhasználó vagy folyamat hello törölt objektum. Hello kiszolgálóoldali tárolási naplózás naplóban hello művelet-típus és a kért objektum-kulcs típusú oszlopok megjelenítése objektum törlésekor az ügyfelet.

Hello forgatókönyvben, ha egy ügyfél megpróbál tooinsert objektum akkor előfordulhat, hogy azonnal nem válik egyértelművé, ezért az eredmény (nem található) HTTP 404-es választ fényében, hogy a hello ügyfél hoz létre egy új objektumot. Azonban ha hello ügyfél hoz létre egy blobot, azt kell tudni toofind hello blob tároló, lehet, ha hello ügyfél képes toofind várólista lehet üzenetet hoz létre, és ha hello ügyfél hozzáadásával egy sort kell legyen képes toofind hello tábla.

Hello ügyféloldali napló a hello a Storage ügyféloldali kódtára toogain egy részletesebb megértése hello ügyfél küld bizonyos kérésekre toohello tároló szolgáltatást is használhatja.

hello hello Storage ügyféloldali kódtár által létrehozott következő ügyféloldali naplófájl látható hello probléma hello ügyfél hello tároló nem található a következő hello blob akkor hoz létre. Ez a napló tartalmazza a következő tárolási műveletek hello:

| Kérelem azonosítója | Művelet |
| --- | --- |
| 07b26a5d-... |**DeleteIfExists** metódus toodelete hello blob tároló. Vegye figyelembe, hogy ez a művelet tartalmaz egy **HEAD** kérelem toocheck hello hello tároló létezésének ellenőrzésével. |
| e2d06d78... |**CreateIfNotExists** metódus toocreate hello blob tároló. Vegye figyelembe, hogy ez a művelet tartalmaz egy **HEAD** kérésére, amely hello tároló hello meglétét ellenőrzi. Hello **HEAD** 404-es üzenetet adja vissza, de továbbra is. |
| de8b1c3c-... |**UploadFromStream** metódus toocreate hello blob. Hello **PUT** kérelem a 404-es üzenettel sikertelen |

Naplóbejegyzéseket:

| Kérelem azonosítója | A művelet szöveg |
| --- | --- |
| 07b26a5d-... |Szinkron kérések toohttps://domemaildist.blob.core.windows.net/azuremmblobcontainer indítása. |
| 07b26a5d-... |StringToSign = HEAD...x-ms-client-request-id:07b26a5d-...x-ms-date:Tue, 2014. jún 03. 10:33:11 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container. |
| 07b26a5d-... |Válaszra való várakozás közben. |
| 07b26a5d-... |A válasz érkezett. Állapotkód = 200 kérés azonosítója = eeead849... Content-MD5 = ETag = &quot;0x8D14D2DC63D059B&quot;. |
| 07b26a5d-... |Válaszfejlécek sikerült feldolgozni, folytatás a hello részeinek hello műveletet. |
| 07b26a5d-... |Választörzs letöltése. |
| 07b26a5d-... |A művelet sikeresen befejeződött. |
| 07b26a5d-... |Szinkron kérések toohttps://domemaildist.blob.core.windows.net/azuremmblobcontainer indítása. |
| 07b26a5d-... |StringToSign = DELETE...x-ms-client-request-id:07b26a5d-...x-ms-date:Tue, 2014. jún 03. 10:33:12 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container. |
| 07b26a5d-... |Válaszra való várakozás közben. |
| 07b26a5d-... |A válasz érkezett. Állapotkód = 202, kérelem azonosítója = 6ab2a4cf-..., Content-MD5 = ETag =. |
| 07b26a5d-... |Válaszfejlécek sikerült feldolgozni, folytatás a hello részeinek hello műveletet. |
| 07b26a5d-... |Választörzs letöltése. |
| 07b26a5d-... |A művelet sikeresen befejeződött. |
| e2d06d78-... |Aszinkron kérelem toohttps://domemaildist.blob.core.windows.net/azuremmblobcontainer indítása.</td> |
| e2d06d78-... |StringToSign = HEAD...x-ms-client-request-id:e2d06d78-...x-ms-date:Tue, 2014. jún 03. 10:33:12 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container. |
| e2d06d78-... |Válaszra való várakozás közben. |
| de8b1c3c-... |Szinkron kérések toohttps://domemaildist.blob.core.windows.net/azuremmblobcontainer/blobCreated.txt indítása. |
| de8b1c3c-... |StringToSign = PUT... 64.qCmF+TQLPhq/YYK50mP9ZQ==...x-MS-BLOB-Type:BlockBlob.x-MS-Client-Request-ID:de8b1c3c-...x-MS-Date:TUE, 2014. jún 03. 10:33:12 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer/blobCreated.txt. |
| de8b1c3c-... |Felkészülés a toowrite kérelem adatai. |
| e2d06d78-... |Kivétel keletkezett a válaszra való várakozás közben: hello távoli kiszolgáló hibát adott vissza: (404) nem található. |
| e2d06d78-... |A válasz érkezett. Állapotkód: 404-es, a kérelem azonosítója = = 353ae3bc-..., Content-MD5 = ETag =. |
| e2d06d78-... |Válaszfejlécek sikerült feldolgozni, folytatás a hello részeinek hello műveletet. |
| e2d06d78-... |Választörzs letöltése. |
| e2d06d78-... |A művelet sikeresen befejeződött. |
| e2d06d78-... |Aszinkron kérelem toohttps://domemaildist.blob.core.windows.net/azuremmblobcontainer indítása. |
| e2d06d78-... |StringToSign = PUT... 0...x-MS-Client-Request-ID:e2d06d78-...x-MS-Date:TUE, 2014. jún 03. 10:33:12 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container. |
| e2d06d78-... |Válaszra való várakozás közben. |
| de8b1c3c-... |Kérelem az adatok írásakor. |
| de8b1c3c-... |Válaszra való várakozás közben. |
| e2d06d78-... |Kivétel keletkezett a válaszra való várakozás közben: hello távoli kiszolgáló hibát adott vissza: (409) ütközés... |
| e2d06d78-... |A válasz érkezett. Állapotkód = 409, a kérelem azonosítója = c27da20e-..., Content-MD5 = ETag =. |
| e2d06d78-... |Csomagletöltési hiba adott válasz törzse. |
| de8b1c3c-... |Kivétel keletkezett a válaszra való várakozás közben: hello távoli kiszolgáló hibát adott vissza: (404) nem található. |
| de8b1c3c-... |A válasz érkezett. Állapotkód: 404-es, a kérelem azonosítója = = 0eaeab3e-..., Content-MD5 = ETag =. |
| de8b1c3c-... |Kivétel lépett fel az hello művelet közben: hello távoli kiszolgáló hibát adott vissza: (404) nem található. |
| de8b1c3c-... |Újrapróbálkozási házirend nem engedélyezi az újrapróbálkozást. Sikertelen a távoli kiszolgálóval hello hibát adott vissza: (404) nem található. |
| e2d06d78-... |Újrapróbálkozási házirend nem engedélyezi az újrapróbálkozást. Sikertelen a távoli kiszolgálóval hello hibát adott vissza: (409) ütközés... |

Ebben a példában hello naplója azt mutatja, hogy az ügyfél hello van kihagyásos hello kérelmeinek **CreateIfNotExists** metódus (... kérelem azonosítója e2d06d78) hello hello kérelmeinek **UploadFromStream** () metódus de8b1c3c-...); Ez történik, mert hello ügyfélalkalmazás aszinkron módon hívja a ezeket a módszereket. Hello hello ügyfél tooensure, hogy létrehozza hello tároló tooupload megkísérlése előtt minden adat tooa blob a tárolóban lévő aszinkron kódot kell módosítani. Ideális esetben a tárolók előre kell létrehoznia.

#### <a name="SAS-authorization-issue"></a>Egy közös hozzáférésű Jogosultságkód (SAS) hitelesítési hiba
Ha hello ügyfélalkalmazás megpróbál toouse hello szükséges engedélyek hello a művelethez nem tartalmazza az SAS-kulcsot, a hello társzolgáltatás egy HTTP 404-es (nem található) üzenet toohello ügyfél adja vissza. Hello: azonos idő, látni fogja emellett nullától eltérő értéket **SASAuthorizationError** a hello metrikákat.

a következő táblázat hello egy minta kiszolgálóoldali naplófájlüzenetre hello tárolási naplózás naplófájl látható:

| Név | Érték |
| --- | --- |
| Kérelem kezdési ideje | 2014-05-30T06:17:48.4473697Z |
| Művelet típusa     | GetBlobProperties            |
| Szolgáltatáskérés állapota     | SASAuthorizationError        |
| HTTP-állapotkód:   | 404                          |
| Hitelesítés típusa| SAS                          |
| Szolgáltatás típusa       | Blob                         |
| Kérelem URL-címe        | https://domemaildist.BLOB.Core.Windows.NET/azureimblobcontainer/blobCreatedViaSAS.txt |
| nbsp;              |   ? sv 2014-02-14 = & sr = c & si = mypolicy & sig = XXXXX&;api-version = 2014-02-14 |
| Kérelemfejlécet azonosítója  | a1f348d5-8032-4912-93EF-b393e5252a3b |
| Ügyfélkérelem-azonosító  | 2d064953-8436-4ee0-aa0c-65cb874f7929 |


Ki kell vizsgálni, hogy miért az ügyfélalkalmazás megpróbál tooperform egy művelet nem kapta meg engedélyeket.

#### <a name="JavaScript-code-does-not-have-permission"></a>Ügyféloldali JavaScript-kód nem rendelkezik engedéllyel tooaccess hello objektum
Ha a JavaScript-ügyfelet kell használnia, és hello társzolgáltatás HTTP 404-es üzeneteket ad eredményül, keressen a következő JavaScript-hibák hello böngészőben hello:

```
SEC7120: Origin http://localhost:56309 not found in Access-Control-Allow-Origin header.
SCRIPT7002: XMLHttpRequest: Network Error 0x80070005, Access is denied.
```

> [!NOTE]
> Az Internet Explorer tootrace köszönőüzenetei hello böngésző- és hello tárolási szolgáltatás között váltott, az ügyféloldali JavaScript problémák elhárításakor hello F12 fejlesztői eszközök is használhatja.
> 
> 

Ezek a hibák fordulhat elő, mert hello webböngésző megvalósítja hello [ugyanazt](http://www.w3.org/Security/wiki/Same_Origin_Policy) biztonsági korlátozása, hogy a webes API hívása egy másik tartományban hello tartomány hello oldalról származik.

toowork hello JavaScript hibát körül, konfigurálhatja Cross Origin Resource Sharing (CORS) a hello tárolási szolgáltatás hello ügyfelet. További információkért lásd: [Azure Storage szolgáltatásainak Cross-Origin Resource Sharing (CORS) támogatása](http://msdn.microsoft.com/library/azure/dn535601.aspx).

a következő példakód hello bemutatja, hogyan tooconfigure a blob szolgáltatás tooallow hello Contoso tartomány tooaccess blob a blob storage szolgáltatásban futó JavaScript:

```csharp
CloudBlobClient client = new CloudBlobClient(blobEndpoint, new StorageCredentials(accountName, accountKey));
// Set hello service properties.
ServiceProperties sp = client.GetServiceProperties();
sp.DefaultServiceVersion = "2013-08-15";
CorsRule cr = new CorsRule();
cr.AllowedHeaders.Add("*");
cr.AllowedMethods = CorsHttpMethods.Get | CorsHttpMethods.Put;
cr.AllowedOrigins.Add("http://www.contoso.com");
cr.ExposedHeaders.Add("x-ms-*");
cr.MaxAgeInSeconds = 5;
sp.Cors.CorsRules.Clear();
sp.Cors.CorsRules.Add(cr);
client.SetServiceProperties(sp);
```

#### <a name="network-failure"></a>Hálózati hiba
Bizonyos körülmények között elveszett csomagokat HTTP 404-es üzenetek toohello ügyfél visszaadó toohello társzolgáltatás is járhat. Például, ha az ügyfélalkalmazást entitás törlése hello table szolgáltatásból megjelenik egy tárolási kivétel jelentés throw hello ügyfél egy "HTTP 404-es (nem található)" hello table szolgáltatásból a hibaállapot-üzeneteket. Hello table storage szolgáltatás hello táblájában vizsgálatánál láthatja, hogy hello szolgáltatás hello entitás volt törölte a kért.

hello kivétel részletei hello ügyfél hello kérelemazonosító (7e84f12d...) hello table szolgáltatás hello kérelem által hozzárendelt tartalmazza: is használhatja a toolocate hello kérelem adatai hello kiszolgálóoldali tárolási naplókban hello kereséssel  **azonosító-kérelemfejléc** oszlop hello naplófájlban. Hello metrikák tooidentify amikor hibák, mint ez történik, és keressen a hello naplófájlok hello idő hello mérőszámok alapján ez a hiba is használhatja. A napló bejegyzés tartalmazza, amelyek hello törlése sikertelen volt egy "HTTP (404) ügyfél más hiba" állapotüzenetet. hello azonos naplóbejegyzés is hello hello ügyfél által generált hello kérelemazonosító **ügyfélkérelem-azonosító** oszlop (813ea74f...).

hello kiszolgálóoldali napló is egy másik bejegyzés hello azonos **ügyfélkérelem-azonosító** értékét (813ea74f...) a sikeres művelet törlése hello ugyanaz az entitás, és a hello azonos ügyfél. A sikeres törlési művelete rövid idő előtt hello sikertelen törlési kérés történt.

hello legvalószínűbb oka az ebben a forgatókönyvben az ügyfél által hello hello entitás toohello table szolgáltatás, amely sikeres volt, de nem kapott nyugtázást hello kiszolgáló (lehet, hogy miatt tooa átmeneti hálózati probléma) delete kérelmet küldött. hello ügyfél ezután automatikusan újra megpróbálja hello művelet (használatával hello azonos **ügyfélkérelem-azonosító**), és ily módon tett újrapróbálkozás sikertelen volt, mert hello entitás már törlődött.

Ha a probléma gyakran előfordul, ki kell vizsgálni, miért hello az ügyfél nem képes tooreceive nyugták hello table szolgáltatásból. Ha hello probléma időszakos, azt kell hello "HTTP (404) nem található" hiba trap és -e jelentkezni az hello ügyfél, azonban hello ügyfél toocontinue engedélyezése.

### <a name="the-client-is-receiving-409-messages"></a>hello ügyfél HTTP 409 (Ütközés) üzeneteket fogad
hello következő táblázatban a két ügyfélműveleteknél hello kiszolgálóoldali napló kivonatát: **DeleteIfExists** után azonnal az **CreateIfNotExists** használatával hello azonos blobtároló neve. Vegye figyelembe, hogy minden ügyfél művelet eredményezi két küldött kérelmek toohello kiszolgáló, először egy **GetContainerProperties** kérelem toocheck hello tároló létezésének követ hello **DeleteContainer** vagy  **CreateContainer** kérelmet.

| időbélyeg | Művelet | eredménye | Tárolónév | Ügyfélkérelem-azonosító |
| --- | --- | --- | --- | --- |
| 05:10:13.7167225 |GetContainerProperties |200 |mmcont |c9f52c89-... |
| 05:10:13.8167325 |DeleteContainer |202 |mmcont |c9f52c89-... |
| 05:10:13.8987407 |GetContainerProperties |404 |mmcont |bc881924-... |
| 05:10:14.2147723 |CreateContainer |409 |mmcont |bc881924-... |

hello hello ügyfélalkalmazás kód törli, és azonnal újra létrehozza a hello segítségével blobtárolóban ugyanazzal a névvel: hello **CreateIfNotExists** (ügyfél kérelem azonosítója bc881924-...) metódus végül meghiúsul, és hello HTTP 409 (Ütközés) Hiba történt. Ha egy ügyfél töröl a blobtárolók, táblák, vagy olyan várólisták esetében egy rövid időszak előtt hello neve ismét elérhetővé válik.

Ha hello törlése és ismételt létrehozása minta közös létrehozott új tárolók hello ügyfélalkalmazás egyedi tárolónévnek kell használni.

### <a name="metrics-show-low-percent-success"></a>Metrika alacsony PercentSuccess vagy analytics naplóbejegyzések rendelkezik ClientOtherErrors működésére a tranzakció állapota
Hello **PercentSuccess** metrika hello százaléka sikerrel járt-e a HTTP-állapotkód alapján műveleteket rögzíti. A 2XX állapotkódok műveletek száma sikeres, mivel az állapotkódnak 3XX, 4XX és 5XX tartományok műveletek számítanak sikertelen és alsó hello **PercentSucess** Átjárómetrika értékeként. Hello kiszolgálóoldali tárolási naplófájlokban, ezek a műveletek vannak rögzített, a tranzakció állapota **ClientOtherErrors**.

Fontos, hogy ezeket a műveleteket sikeresen befejeződött, és ezért nem befolyásolják a más mutatókat, például a rendelkezésre állás fontos toonote. Néhány művelet, hogy sikeresen végrehajtható legyen, de a sikertelen HTTP-állapotkódok eredményezhet, amely többek között:

* **ResourceNotFound** (nem található 404-es), például a GET kérelem tooa blob, amely nem létezik.
* **ResouceAlreadyExists** (ütközés 409), például egy **CreateIfNotExist** művelet, ahol a hello erőforrás már létezik.
* **ConditionNotMet** (nem módosított 304), például az egy feltételes művelet, például amikor egy ügyfél küld egy **ETag** érték és a HTTP **If-None-Match** fejléc toorequest egy lemezképet csak akkor, ha az utolsó művelet hello óta frissítve lett.

Közös REST API hiba megtekintéséhez hello tárolószolgáltatások visszaadó hello oldalon található [közös REST API hibakódok](http://msdn.microsoft.com/library/azure/dd179357.aspx).

### <a name="capacity-metrics-show-an-unexpected-increase"></a>Teljesítmény-mérőszámait váratlan növekedését megjelenítése a tárolási kapacitás-használat
Hirtelen jelenik meg, ha a tárfiókban lévő kapacitás használati váratlan változásai használatával megvizsgálhatja a hello okok első megnézzük a rendelkezésre állási metrikák; például sikertelen törlési kérések száma hello növekedése vezethet tooan növelését hello legrövidebb használ alkalmazás adott karbantartási műveleteket, lehet, hogy rendelkezik megfelelő toobe szabadítson fel helyet előfordulhat, hogy nem az elvárásoknak megfelelően működik (a blob-tároló Példa, mert lejárt hello SAS-tokenje szabadítson fel helyet használja).

### <a name="you-are-experiencing-unexpected-reboots"></a>Váratlan újraindítások Azure virtuális gépek, amelyek csatlakoztatott virtuális merevlemezek nagy számú tapasztal
Ha egy Azure virtuális gép (VM) számos csatolt VHD-k, amelyek a hello ugyanazt a tárfiókot, akkor meghaladhatja a hello méretezhetőségi célok hello VM toofail, amely az egyes tárolási fiókot. Ellenőrizni kell a hello percenkénti metrikákat hello tárfiókhoz (**TotalRequests**/**TotalIngress**/**TotalEgress**) a igényeiben jelentkező hello méretezhetőségi célok tárfiókok esetén, amely lehet. Hello című rész "[PercentThrottlingError metrika növelését]" támogatásért, ha sávszélesség-szabályozás történt a tárfiók számára.

Általában minden egyes bemeneti vagy kimeneti műveletet a virtuális gép virtuális merevlemez a következőkből fordítja le túl**első oldal** vagy **Put lap** az alapul szolgáló oldalakra vonatkozó blob hello műveleteket. Ezért használható hello becsült IOPS a környezet tootune is rendelkezik egyetlen tárfiókokban alapján hello adott viselkedést az alkalmazás hány virtuális merevlemezeket. Legfeljebb 40 lemezek rendelkező egyetlen tárfiókokban nem ajánlott. Lásd: [Azure Storage méretezhetőségi és teljesítménycéloknak](storage-scalability-targets.md) hello aktuális méretezhetőségi célok storage-fiókok részleteit, különösen hello teljes kérelem sebesség és a teljes sávszélesség hello típusú tárfiókot használ .
Ha a tárfiók túllépte hello méretezhetőségi célok, helyezze a VHD-k több különböző tárolási fiókok tooreduce hello tevékenységet minden egyes számlára.

### <a name="your-issue-arises-from-using-the-storage-emulator"></a>A probléma merül fel a fejlesztési vagy tesztelési hello storage emulator használatával
Általában hello storage emulatorral a fejlesztés során, és tesztelje tooavoid hello követelmény az Azure storage-fiók. hello storage emulator használatakor előforduló gyakori problémákat hello a következők:

* ["X" a szolgáltatás nem működik-e a hello storage emulator]
* [Hiba történt "hello hello HTTP-fejlécek egyikéhez értéke nem a megfelelő formátumban hello" Ha hello storage emulator használatával]
* [Futó hello storage emulator rendszergazdai jogosultságokra van szüksége.]

#### <a name="feature-X-is-not-working"></a>"X" a szolgáltatás nem működik-e a hello storage emulator
hello storage emulator nem támogatja az összes hello szolgáltatás hello az Azure storage szolgáltatások például hello szolgáltatás. További információkért lásd: [fejlesztés és tesztelés az Azure Storage Emulator használata hello](storage-use-emulator.md).

Ezen szolgáltatások tárolási hello emulátor nem támogatja, hello felhő hello az Azure storage szolgáltatás használata.

#### <a name="error-HTTP-header-not-correct-format"></a>Hiba történt "hello hello HTTP-fejlécek egyikéhez értéke nem a megfelelő formátumban hello" Ha hello storage emulator használatával
Teszteli az alkalmazás, például a Storage ügyféloldali kódtár hello elleni hello helyi storage emulator és metódus hívások használó **CreateIfNotExists** hello hibával hibaüzenet "hello hello HTTP-fejlécek egyikéhez értéke nem található hello megfelelő formátumban." Ez azt jelzi, hogy hello hello storage emulator használata verziója nem támogatja a hello verzióját hello storage ügyféloldali kódtár használata. a Storage ügyféloldali kódtára hello hello fejlécet ad **x-ms-version** tooall hello kérelmek teszi. Ha hello storage emulator nem ismeri fel a hello hello érték **x-ms-version** fejléc, hogy elutasítja-hello kérelem.

Használhatja a hello tárolási Szalagtári ügyfél naplók toosee hello értékének hello **x-ms-version fejlécnek** akkor küld. Azt is láthatja, hello hello értékének **x-ms-version fejlécnek** Fiddler tootrace hello kérelmek az ügyfélalkalmazás használatakor.

Ebben a forgatókönyvben rendszerint azért fordul elő, ha telepíti és hello storage emulator frissítése nélkül hello hello Storage ügyféloldali kódtár legújabb verzióját használja. Meg kell hello hello storage emulator legújabb verziójának telepítéséhez, vagy használjon felhőalapú tárolás helyett hello emulátor fejlesztési és tesztelési.

#### <a name="storage-emulator-requires-administrative-privileges"></a>Futó hello storage emulator rendszergazdai jogosultságokra van szüksége.
Kéri a rendszergazdai hitelesítő adatok hello storage emulator futtatásakor. Ez csak akkor vannak inicializálásakor hello storage emulator hello az első alkalommal fordul elő. Miután hello storage emulator inicializálta, nincs szükség rendszergazdai jogosultságokkal toorun azt újra.

További információkért lásd: [fejlesztés és tesztelés az Azure Storage Emulator használata hello](storage-use-emulator.md). Vegye figyelembe, hogy akkor is inicializálni a hello storage emulatort, a Visual Studio, amely rendszergazdai jogosultságokkal is szükséges lesz.

### <a name="you-are-encountering-problems-installing-the-Windows-Azure-SDK"></a>Áll kapcsolatban problémák hello Azure SDK telepítése a .NET-hez
Tooinstall hello SDK meg, miközben a rendszer tooinstall hello storage emulatort, a helyi gépen sikertelen lesz. hello telepítési naplóját a következő üzenetek hello egyikét tartalmazza:

* CAQuietExec: Hiba: nem tooaccess SQL-példány
* CAQuietExec: Hiba: nem toocreate adatbázis

hello ok: a meglévő LocalDB telepítési kapcsolatos problémát. Alapértelmezés szerint hello storage emulator LocalDB toopersist adatokat használja, amikor azt hello Azure storage szolgáltatások szimulálja. Futtassa a következő parancsok parancssori ablakban előtt tooinstall hello SDK hello alaphelyzetbe állíthatja a LocalDB példányához.

```
sqllocaldb stop v11.0
sqllocaldb delete v11.0
delete %USERPROFILE%\WAStorageEmulatorDb3*.*
sqllocaldb create v11.0
```

Hello **törlése** parancs eltávolítja a régi adatbázisfájlok hello storage emulator az előző telepítések.

### <a name="you-have-a-different-issue-with-a-storage-service"></a>Van egy másik probléma storage szolgáltatással
Ha hello előző hibaelhárítási szakaszok nem tartalmaznak hello problémát tapasztal storage szolgáltatással, el kell fogadnia hello megközelítés toodiagnosing követi, és a hiba elhárításához.

* Ellenőrizze a metrikák toosee, ha bármi is módosul az a várt viszonyítási viselkedése. A hello metrika előfordulhat, hogy lehet a képes toodetermine, hogy hello probléma átmeneti és végleges, és mely tárolási műveletek hello probléma érinti.
* Hello metrikák információt toohelp keres a kiszolgálóoldali naplóadatokat előforduló hibákat részletesebb információt is használhatja. Ezek az információk segítenek hibaelhárításához és megoldásához hello probléma.
* Ha hello információ hello kiszolgálóoldali naplók elegendő tootroubleshoot hello probléma sikeresen, használhatja a hello a Storage ügyféloldali kódtára ügyféloldali naplók tooinvestigate hello viselkedését az ügyfélalkalmazást és eszközöket, például a Fiddler, Wireshark, Microsoft Message Analyzert tooinvestigate és a hálózaton.

Fiddler használatával kapcsolatos további információkért lásd: "[1 függelék: Fiddler toocapture HTTP és HTTPS-forgalom használatával]."

Wireshark használatával kapcsolatos további információkért lásd: "[2 függelék: Wireshark toocapture hálózati forgalom használ]."

Microsoft Message Analyzert használatával kapcsolatos további információkért lásd: "[3 függelék: a Microsoft Message Analyzert toocapture hálózati forgalom használ]."

## <a name="appendices"></a>Mellékletek
hello mellékletek azt tapasztalhatja hasznos diagnosztizálása és elhárítása az Azure Storage (és az egyéb szolgáltatásokkal) eszközöket írja le. Ezek az eszközök nem része az Azure Storage és harmadik féltől származó termékekre. Ilyen bármely támogatási megállapodás, előfordulhat, hogy a Microsoft Azure vagy az Azure Storage nem vonatkozik e mellékletek tárgyalt hello eszközök, és ezért a kiértékelési folyamat részeként meg kell vizsgálni hello licenceléssel és a támogatási lehetőségekről a Ezek az eszközök hello szolgáltatók.

### <a name="appendix-1"></a>1. függelékében: Használatával Fiddler toocapture HTTP és HTTPS-forgalom
[Fiddler](http://www.telerik.com/fiddler) az eszköz elemzéséhez hello HTTP és HTTPS-forgalmat az ügyfélalkalmazást és hello használata az Azure storage szolgáltatás között.

> [!NOTE]
> Fiddler tudja dekódolni a HTTPS-forgalom; olvassa el hello Fiddler dokumentáció gondosan toounderstand hogyan teszi ezt, és toounderstand hello biztonsági szempontokat.
> 
> 

Ebben a függelékben rövid bemutatóért hogyan tooconfigure Fiddler toocapture hello helyi számítógép, amelyen telepítette a Fiddler és hello Azure storage szolgáltatások közötti forgalmat.

Miután Fiddler indítása, a helyi számítógépen a HTTP és HTTPS-forgalom rögzítése kezdi. Az alábbiakban hello szabályozásához a Fiddler néhány hasznos parancsokat:

* Állítsa le és indítsa el a forgalom rögzítése. Hello főmenü, nyissa meg túl**fájl** majd **forgalom rögzítése** tootoggle rögzítése a be- és kikapcsolható.
* Rögzített forgalmi adatok mentése. Hello főmenü, nyissa meg túl**fájl**, kattintson a **mentése**, és kattintson a **minden munkamenet**: Ez lehetővé teszi, hogy toosave hello forgalom munkamenet archív fájlba. Töltse be újra a később elemzéshez munkamenet-archívum létrehozása, vagy küldje el, ha a kért tooMicrosoft támogatási.

rögzíti a Fiddler forgalom toolimit hello mennyiségének szűrőkkel hello konfigurált **szűrők** a következő képernyőkép külön-külön hello jeleníti meg, amely rögzíti az elküldött forgalom csak toohello szűrő  **contosoemaildist.TABLE.Core.Windows.NET** tárolási végpont:

![][5]

### <a name="appendix-2"></a>2. függelékben: Használatával Wireshark toocapture hálózati forgalom
[Wireshark](http://www.wireshark.org/) van, amely lehetővé teszi tooview hálózati protokollelemző eszköz részletes számos különböző hálózati protokollok csomag adatait.

hello alábbi eljárás bemutatja, hogyan toocapture részletes a forgalmat a helyi számítógépről hello csomagok információit Wireshark toohello table szolgáltatás telepítési az Azure-tárfiókot.

1. Indítsa el a Wireshark a helyi számítógépen.
2. A hello **Start** szakaszban, a select hello helyi hálózati adapter vagy a felületek, amelyek csatlakoztatott toohello internet.
3. Kattintson a **beállítások rögzítése**.
4. Adja hozzá a szűrő toohello **rögzítési szűrő** szövegmező. Például **contosoemaildist.table.core.windows.net gazdagép** Wireshark toocapture úgy konfigurálja, csak a csomagok tooor küldött hello table szolgáltatási végpont a hello **contosoemaildist** tároló fiók. Tekintse meg a hello [rögzítése szűrők teljes listájának](http://wiki.wireshark.org/CaptureFilters).
   
   ![][6]
5. Kattintson a **Start**. Wireshark most rögzíti az összes hello csomagok küldése tooor hello table szolgáltatási végpont a használata az ügyfélalkalmazást a helyi számítógépen.
6. Miután végzett, hello fő menüben kattintson a **rögzítése** , majd **leállítása**.
7. a rögzített adatok Wireshark rögzítése fájlba, a hello főmenü kattintson toosave hello **fájl** , majd **mentése**.

WireShark ki olyan hibákat, amelyek szerepelnek a hello **packetlist** ablak. Is használhatja a hello **szakértő Info** ablakban (kattintson **elemzés**, majd **szakértő Info**) tooview hibák és figyelmeztetések összegzését.

![][7]

Ekkor is kiválaszthatja tooview hello TCP-adatok, hello alkalmazásréteg látja, az TCP-adatok hello csomagot jobb gombbal, majd válassza **hajtsa végre a TCP-folyam**. Ez különösen fontos, ha a biztonsági másolat nélkül rögzítési szűrő rögzített. További információkért lásd: [következő TCP-adatfolyamok](http://www.wireshark.org/docs/wsug_html_chunked/ChAdvFollowTCPSection.html).

![][8]

> [!NOTE]
> Wireshark használatával kapcsolatos további információkért lásd: hello [Wireshark felhasználók útmutató](http://www.wireshark.org/docs/wsug_html_chunked).
> 
> 

### <a name="appendix-3"></a>3. függelék: Használatával a Microsoft Message Analyzert toocapture hálózati forgalom
Microsoft Message Analyzert toocapture HTTP és HTTPS-forgalmat a hasonló módon tooFiddler használja, és egy hasonló módon tooWireshark a hálózati forgalom rögzítése.

#### <a name="configure-a-web-tracing-session-using-microsoft-message-analyzer"></a>Egy webes nyomkövetési munkamenet használ a Microsoft Message Analyzert konfigurálása
egy webes nyomkövetési munkamenet a HTTP és HTTPS-forgalmat használatával a Microsoft Message Analyzert, hello Microsoft Message Analyzert alkalmazás futtatásához, majd a hello tooconfigure **fájl** menüben kattintson a **rögzítési/nyomkövetési**. Rendelkezésre álló nyomkövetési forgatókönyvek hello listában válassza ki a **webproxy**. Ezt a hello **nyomkövetési forgatókönyv konfigurációjának** panel hello **HostnameFilter** szövegmező, a tárolási végpontok hello neveket adhat hozzá (ezekkel a nevekkel hello megjeleníthetők [Azure-portálon](https://portal.azure.com)). Például hello neve az Azure storage-fiók akkor **contosodata**, adja hozzá a következő toohello hello **HostnameFilter** szövegmező:

```
contosodata.blob.core.windows.net contosodata.table.core.windows.net contosodata.queue.core.windows.net
```

> [!NOTE]
> A szóköz karakter hello állomásnevek elválasztja.
> 
> 

Amikor készen áll a toostart nyomkövetési adatokat gyűjt, kattintson a hello **Start With** gombra.

További információt a Microsoft Message Analyzert hello **webproxy** nyomon követése, lásd: [Microsoft-PEF-WebProxy szolgáltató](http://technet.microsoft.com/library/jj674814.aspx).

beépített hello **webproxy** nyomkövetési Microsoft Message Analyzert a Fiddler alapul; ügyféloldali HTTPS-forgalom rögzítése és nem titkosított HTTPS üzenetek megjelenítése. Hello **webproxy** works nyomkövetése minden HTTP és HTTPS-forgalom, mely hozzáférést toounencrypted üzenetek helyi proxy konfigurálása.

#### <a name="diagnosing-network-issues-using-microsoft-message-analyzer"></a>Microsoft Message Analyzert használata a hálózati problémák diagnosztizálása
Ezenkívül toousing hello Microsoft Message Analyzert **webproxy** nyomkövetési toocapture részleteit hello HTTP/HTTPs-forgalom hello ügyfélalkalmazást és hello tárolási szolgáltatás között, használhatja a hello beépített  **Helyi kapcsolat réteg** nyomkövetési toocapture hálózati csomagok információit. Ez lehetővé teszi, hogy Ön toocapture adatok hasonló toothat rendelkező Wireshark rögzítése, és diagnosztizálhatja hálózati problémák, mint eldobott csomagok.

hello alábbi képernyőfelvételen szereplő példán látható **helyi kapcsolat réteg** egy nyomkövetési **tájékoztató** hello üzenetek **DiagnosisTypes** oszlop. Kattintson egy ikonra a hello a **DiagnosisTypes** oszlop üdvözlőüzenetére hello részleteit jeleníti meg. Ebben a példában hello server üzenet #305 újraküldött, mert nem kapott nyugtázást hello ügyfélről:

![][9]

A Microsoft Message Analyzert hello nyomkövetési munkamenet létrehozásakor hello nyomkövetési szűrők tooreduce hello mennyisége zaj adhat meg. A hello **rögzítése / nyomkövetési** lap, ahol megadhatja a hello nyomkövetési, kattintson a hello **konfigurálása** túl melletti hivatkozásra**Microsoft-Windows-NDIS-PacketCapture**. a következő képernyőkép hello jeleníti meg a konfigurációkat, amelyek a TCP-forgalom három tárolási szolgáltatások hello IP-címek szűrése:

![][10]

További információ a Microsoft Message Analyzer helyi kapcsolat réteg nyomkövetési hello: [Microsoft-PEF-NDIS-PacketCapture szolgáltató](http://technet.microsoft.com/library/jj659264.aspx).

### <a name="appendix-4"></a>4. függelék: Excel tooview metrikákat és naplózási adatok használata
Sok eszközei lehetővé teszik toodownload hello Storage mérőszámainak adatokat az Azure table storage egy tagolt formátumú, így könnyen tooload hello adatok az Excel programba megtekintése és elemzése. Az Azure blob storage tárolási naplózási adatok már Excelbe betöltése tagolt formátumú. Azonban szüksége lesz tooadd megfelelő oszlopának fejlécére kattintva rendezhető hello információkat a alapú [Storage Analytics naplóformátumban](http://msdn.microsoft.com/library/azure/hh343259.aspx) és [Storage Analytics metrikák táblaséma](http://msdn.microsoft.com/library/azure/hh343264.aspx).

tooimport a naplózás tárolási adatokat Excelbe követően letölthető blob-tároló:

* A hello **adatok** menüben kattintson a **a szöveg**.
* Tallózás toohello naplófájl tooview ki, majd kattintson **importálási**.
* A hello / 1. lépés **varázslóban**, jelölje be **tagolt**.

A hello / 1. lépés **varázslóban**, jelölje be **pontosvessző** , csak elválasztó hello, és válassza ki, hello idézőjel **Szöveghatároló**. Kattintson a **Befejezés** , és válassza ki, ahol tooplace hello munkafüzete adataihoz.

### <a name="appendix-5"></a>5. függelék: A Visual Studio Team Services az Application insights szolgáltatással figyelését.
Hello Application Insights szolgáltatás Visual Studio Team Services, a teljesítmény és rendelkezésre állásának figyelésére szolgáló részeként is használja. Ez az eszköz a következőket teheti:

* Ellenőrizze, hogy a webes szolgáltatás megfelelően üzemel és rugalmas. -E az alkalmazás egy webhely vagy egy eszköz-alkalmazást, amely egy webszolgáltatás-bővítmény használ, azt az URL-cím néhány percenként tesztelése hello világ különböző helyekről, és jelzi, ha probléma van.
* Gyorsan diagnosztizálhatja a webszolgáltatás kivételek, illetve bármely problémák. Annak megállapítása, ha Processzor- vagy egyéb erőforrások vannak nyújtja, a kivételek az híváslánc megjelenik le, és könnyen megkereshetjük a naplókivonatokat. Ha hello alkalmazás teljesítmény elhagyta elfogadható határértékek alatt, elküldhetjük egy e-mailt. .NET- és a Java webes szolgáltatásokat is figyelhet.

További információt a [Mi az Application Insights?](../../application-insights/app-insights-overview.md).

<!--Anchors-->
[Bevezetés]: #introduction
[Hogyan szerveződik, ez az útmutató]: #how-this-guide-is-organized

[a tárolás szolgáltatás figyelése]: #monitoring-your-storage-service
[Figyelési szolgáltatásának állapota]: #monitoring-service-health
[Kapacitásának figyelése]: #monitoring-capacity
[Rendelkezésre állás figyelése]: #monitoring-availability
[A teljesítmény figyelése]: #monitoring-performance

[tárolási problémák diagnosztizálása]: #diagnosing-storage-issues
[Szolgáltatás ügynökállapottal kapcsolatos hibákkal]: #service-health-issues
[Teljesítménnyel kapcsolatos problémák]: #performance-issues
[Hibák diagnosztizálása]: #diagnosing-errors
[Tárolási emulátor problémák]: #storage-emulator-issues
[Tárolási naplózási eszközök]: #storage-logging-tools
[Hálózati naplózási eszközök használatával]: #using-network-logging-tools

[végpont nyomkövetés]: #end-to-end-tracing
[Naplóadatok válaszoknak az összekapcsolása]: #correlating-log-data
[Ügyfélkérelem-azonosító]: #client-request-id
[Server Kérelemazonosító]: #server-request-id
[Időbélyeg helyi időre]: #timestamps

[hibaelhárítási útmutatás]: #troubleshooting-guidance
[metrika AverageE2ELatency magas és alacsony AverageServerLatency]: #metrics-show-high-AverageE2ELatency-and-low-AverageServerLatency
[A metrika alacsony AverageE2ELatency és alacsony AverageServerLatency, de hello ügyfél tapasztal nagy késleltetésű]: #metrics-show-low-AverageE2ELatency-and-low-AverageServerLatency
[A mérőszámok magas AverageServerLatency értéket mutatnak]: #metrics-show-high-AverageServerLatency
[Váratlan késést tapasztal az üzenetsorban található üzenetek kézbesítésekor]: #you-are-experiencing-unexpected-delays-in-message-delivery

[PercentThrottlingError metrika növelését]: #metrics-show-an-increase-in-PercentThrottlingError
[PercentThrottlingError átmeneti növekedése]: #transient-increase-in-PercentThrottlingError
[Állandó növekedése PercentThrottlingError hiba]: #permanent-increase-in-PercentThrottlingError
[PercentTimeoutError metrika növelését]: #metrics-show-an-increase-in-PercentTimeoutError
[A mérőszámok emelkedő PercentNetworkError értéket mutatnak]: #metrics-show-an-increase-in-PercentNetworkError

[hello ügyfél HTTP 403 (tiltott) üzeneteket fogad]: #the-client-is-receiving-403-messages
[hello ügyfél (nem található) HTTP 404 üzeneteket fogad]: #the-client-is-receiving-404-messages
[hello ügyfél vagy egy másik folyamat korábban törölt hello objektum]: #client-previously-deleted-the-object
[Egy közös hozzáférésű Jogosultságkód (SAS) hitelesítési hiba]: #SAS-authorization-issue
[Ügyféloldali JavaScript-kód nem rendelkezik engedéllyel tooaccess hello objektum]: #JavaScript-code-does-not-have-permission
[Hálózati hiba]: #network-failure
[hello ügyfél HTTP 409 (Ütközés) üzeneteket fogad]: #the-client-is-receiving-409-messages

[metrika alacsony PercentSuccess vagy analytics naplóbejegyzések rendelkezik műveletek tranzakció állapota ClientOtherErrors]: #metrics-show-low-percent-success
[Teljesítmény-mérőszámait váratlan növekedését megjelenítése a tárolási kapacitás-használat]: #capacity-metrics-show-an-unexpected-increase
[Váratlan vesznek el virtuális gépeken, amelyek csatlakoztatott virtuális merevlemezek nagy számú tapasztal]: #you-are-experiencing-unexpected-reboots
[A probléma merül fel a fejlesztési vagy tesztelési hello storage emulator használatával]: #your-issue-arises-from-using-the-storage-emulator
["X" a szolgáltatás nem működik-e a hello storage emulator]: #feature-X-is-not-working
[Hiba történt "hello hello HTTP-fejlécek egyikéhez értéke nem a megfelelő formátumban hello" Ha hello storage emulator használatával]: #error-HTTP-header-not-correct-format
[Futó hello storage emulator rendszergazdai jogosultságokra van szüksége.]: #storage-emulator-requires-administrative-privileges
[Áll kapcsolatban problémák hello Azure SDK telepítése a .NET-hez]: #you-are-encountering-problems-installing-the-Windows-Azure-SDK
[Van egy másik probléma storage szolgáltatással]: #you-have-a-different-issue-with-a-storage-service

[mellékletek]: #appendices
[1 függelék: Fiddler toocapture HTTP és HTTPS-forgalom használatával]: #appendix-1
[2 függelék: Wireshark toocapture hálózati forgalom használ]: #appendix-2
[3 függelék: a Microsoft Message Analyzert toocapture hálózati forgalom használ]: #appendix-3
[4. függelék: Excel tooview metrikákat és naplózási adatok használata]: #appendix-4
[5. függelék: A Visual Studio Team Services az Application insights szolgáltatással figyelését.]: #appendix-5

<!--Image references-->
[1]: ./media/storage-monitoring-diagnosing-troubleshooting/overview.png
[3]: ./media/storage-monitoring-diagnosing-troubleshooting/hour-minute-metrics.png
[4]: ./media/storage-monitoring-diagnosing-troubleshooting/high-e2e-latency.png
[5]: ./media/storage-monitoring-diagnosing-troubleshooting/fiddler-screenshot.png
[6]: ./media/storage-monitoring-diagnosing-troubleshooting/wireshark-screenshot-1.png
[7]: ./media/storage-monitoring-diagnosing-troubleshooting/wireshark-screenshot-2.png
[8]: ./media/storage-monitoring-diagnosing-troubleshooting/wireshark-screenshot-3.png
[9]: ./media/storage-monitoring-diagnosing-troubleshooting/mma-screenshot-1.png
[10]: ./media/storage-monitoring-diagnosing-troubleshooting/mma-screenshot-2.png
