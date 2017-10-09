---
title: "aaaData megőrzési és a tárolás Azure Application Insights |} Microsoft Docs"
description: "Megőrzési és adatvédelme házirendutasítás"
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: a6268811-c8df-42b5-8b1b-1d5a7e94cbca
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 04/07/2017
ms.author: bwren
ms.openlocfilehash: 7823431d03a57db5268d2724a0604e40666009f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="data-collection-retention-and-storage-in-application-insights"></a>Adatgyűjtés, megőrzés és tárolás az Application Insights szolgáltatásban


Ha telepíti az [Azure Application Insights] [ start] SDK az alkalmazás, elküldi az alkalmazás toohello felhő telemetriai. Természetesen a felelős a fejlesztők tooknow szeretné pontosan milyen adatokat küldi el, mi történik, toohello adatok és hogyan akkor is megtarthatják az irányítást, akkor. Ebben az esetben sikerült bizalmas adatokat elküldeni, hol található a tárolt, és hogy mennyire vannak biztonságban úgy? 

Első, hello rövid válasz:

* hello szabványos telemetriai "hello kezdő" verzióról futtató modulokra valószínű toosend bizalmas adatok toohello szolgáltatás. hello telemetriai aggasztanak terhelés, teljesítmény- és használati metrikák, kivétel jelentések és más diagnosztikai adatokat. hello fő felhasználói adatok hello diagnosztikai jelentései látható URL-címek; azonban az alkalmazás nem minden esetben bizalmas adatok egyszerű szövegként a URL-címében.
* Írhat kódot, amely további egyéni telemetria toohelp elküldi a diagnosztikai és használati figyelési való. (A bővíthetőség az Application Insights kiváló szolgáltatása.) Ki lehet, a hibát, ezzel a kóddal, amelyekben szerepelnek személyes toowrite és más bizalmas adatokat. Ha az alkalmazás ilyen adatokkal dolgozik, alkalmazni egy alapos felülvizsgálati folyamatok tooall hello írt kódot.
* Fejlesztés és tesztelés az alkalmazás során milyen küldi hello SDK által könnyen tooinspect. hello adat hello hibakeresés kimeneti windows hello IDE és a böngésző jelenik meg. 
* hello adatok használatban van [Microsoft Azure](http://azure.com) hello USA vagy Európa kiszolgálók. (De az alkalmazás bárhol futhatnak.) Azure rendelkezik [erős biztonságot dolgozza fel, és megfelel a megfelelőségi követelményeket széles körének](https://azure.microsoft.com/support/trust-center/). Csak Ön és kijelölt csapata hozzáférési tooyour adatokat. Microsoft személyzete is korlátozott hozzáférés tooit csak bizonyos körülmények korlátozott a ismeretekkel. Az átvitel során, bár a nem a hello kiszolgálók titkosított.

Ez a cikk többi hello teljes körűen alapuló, ezek a válaszok. Az önálló, toobe van kialakítva, hogy toocolleagues, akik az azonnali csapat része nem lehet megjeleníteni.

## <a name="what-is-application-insights"></a>Mi az Application Insights?
[Az Azure Application Insights] [ start] , amelynek segítségével a Microsoft által biztosított szolgáltatás hello teljesítmény és az élő alkalmazás használhatóságuk javításában. Az alkalmazás fut, tesztelés során és azt követően, hogy közzétett vagy telepítve lett minden hello idő figyeli. Az Application Insights hoz létre a diagramok és táblákat, amelyek bemutatják, például a legtöbb felhasználó kapott mely napszakokban hogyan teheti gyorsabban kezelhetővé hello alkalmazás, és hogyan well bármely külső által kiszolgált szolgáltatás, amely függ. Ha összeomlik, hibák vagy teljesítményproblémák, kereshet hello telemetriai adatokat a részletes toodiagnose hello OK keresztül. És hello szolgáltatást fog küldeni e-mailek módosításai hello rendelkezésre állását és teljesítményét az alkalmazás esetén.

A sorrend tooget ezt a funkciót, telepítenie az Application Insights SDK az alkalmazás, amely a kód részévé válik. Az alkalmazás futtatásakor a hello SDK annak működését figyeli, és elküldi a telemetriai toohello Application Insights szolgáltatással. Ez az által üzemeltetett felhőszolgáltatásként [Microsoft Azure](http://azure.com). (De Application Insights olyan alkalmazások, nem csak azokat az Azure-ban tárolt működik.)

![az alkalmazás SDK hello telemetriai toohello Application Insights szolgáltatás küld.](./media/app-insights-data-retention-privacy/01-scheme.png)

az Application Insights szolgáltatás hello tárolja, és elemzi a hello. toosee hello elemzés vagy keresési keresztül hello tárolt telemetriai adatokat bejelentkezik tooyour Azure-fiók és a nyitott hello Application Insights-erőforrást az alkalmazáshoz. Is hozzáférést toohello adatok megosztása a csoport többi tagjával, vagy a megadott Azure-előfizetők.

Az Application Insights szolgáltatás hello exportált adatok lehet például tooa adatbázis vagy tooexternal eszközök. Minden eszköz származó hello szolgáltatás különleges kulccsal adja meg. hello kulcs is visszavonhatók, ha szükséges. 

Application Insights SDK-k érhetők el a alkalmazástípusok számos: webszolgáltatások saját J2EE vagy az ASP.NET-kiszolgálókon, vagy egy Azure; a webes ügyfelek – Ez azt jelenti, hogy hello kódot egy weblapon; fut asztali alkalmazások és szolgáltatások; eszköz alkalmazások, például a Windows Phone, iOS és Android. Összes küldött telemetriai toohello ugyanazt a szolgáltatást.

## <a name="what-data-does-it-collect"></a>Milyen adatok azt gyűjt?
### <a name="how-is-hello-data-is-collected"></a>Mitől hello adatokat gyűjt a rendszer?
Az adatok három forrásának van:

* SDK-t, amely vagy integráció saját alkalmazással hello [a fejlesztési](app-insights-asp-net.md) vagy [futási időben](app-insights-monitor-performance-live-website-now.md). Nincsenek különböző SDK-k különböző alkalmazás esetében. Is egy [webes SDK](app-insights-javascript.md), amely betölti a hello végfelhasználó böngésző hello lap együtt.
  
  * Minden SDK számos [modulok](app-insights-configuration-with-applicationinsights-config.md), amely használja, különböző módszereket toocollect különféle telemetriai.
  * Hello SDK telepítésekor a fejlesztési használhatja az API toosend saját telemetriai hozzáadása toohello szabványos modulban. Az egyéni telemetriai adatokat tartalmazhatnak toosend kívánt adatokat.
* Az olyan webkiszolgálók vannak is ügynökök, amelyek mellett hello app futnak, és a Processzor, memória és hálózati Foglaltság kapcsolatos telemetriai adatokat küldhet. Például Azure virtuális gépeken, Docker-gazdagépekből és [J2EE kiszolgálók](app-insights-java-agent.md) ilyen ügynökök rendelkezhet.
* [Rendelkezésreállás figyelésére szolgáló tesztek](app-insights-monitor-web-app-availability.md) folyamatok végzi a Microsoft által küldött kérelmek tooyour webalkalmazás rendszeres időközönként. hello eredmények toohello Application Insights szolgáltatás küldött.

### <a name="what-kinds-of-data-are-collected"></a>Milyen típusú adatok összegyűjtése?
hello fő kategóriába vannak:

* [Webalkalmazás-kiszolgáló telemetriai](app-insights-asp-net.md) -HTTP-kérelmekre.  URI, igénybe vett idő tooprocess hello kérelem, a válaszkód, az ügyfél IP-címét. Munkamenet-azonosító.
* [Weblapok](app-insights-javascript.md) -oldalon, a felhasználó és a munkamenet számát. Lapbetöltési idők. Kivételek. AJAX-hívások.
* Teljesítmény számlálók - memória, Processzor, IO, hálózati Foglaltság.
* Ügyfél és kiszolgáló context - OS, területi beállítás, eszköztípus, böngésző, felbontást.
* [Kivételek](app-insights-asp-net-exceptions.md) és az összeomlások - **a verem memóriaképek**, build azonosítóját, a CPU típusát. 
* [Függőségek](app-insights-asp-net-dependencies.md) -tooexternal szolgáltatások, mint a többi SQL, az AJAX hívja. URI vagy kapcsolati karakterlánc, időtartama, sikeres, a parancsot.
* [Rendelkezésreállás figyelésére szolgáló tesztek](app-insights-monitor-web-app-availability.md) -vizsgálati és a lépéseket, a válaszok időtartama.
* [Nyomkövetési naplók](app-insights-asp-net-trace-logs.md) és [egyéni telemetria](app-insights-api-custom-events-metrics.md) - **semmit a naplók vagy telemetriai kódaláírással**.

[További részletek](#data-sent-by-application-insights).

## <a name="how-can-i-verify-whats-being-collected"></a>Hogyan ellenőrizhetem alatt gyűjtött adatok?
Ha az hello app Visual Studio használatával, futtassa a hello alkalmazást hibakeresési módban (F5). hello telemetriai hello kimeneti ablakban jelenik meg. Ott másolja, és formázza az as JSON egyszerű ellenőrzést. 

![](./media/app-insights-data-retention-privacy/06-vs.png)

Hello Diagnostics ablakban is van olvashatóbb nézetet.

A weblapokat nyissa meg a böngésző hibakeresési ablakot.

![Nyomja le az F12 és hello hálózati lap megnyitásához.](./media/app-insights-data-retention-privacy/08-browser.png)

### <a name="can-i-write-code-toofilter-hello-telemetry-before-it-is-sent"></a>Írhatok kód toofilter hello telemetriai, elküldés előtt?
Ez lehet írásával egy [telemetriai processzor beépülő modul](app-insights-api-filtering-sampling.md).

## <a name="how-long-is-hello-data-kept"></a>Milyen hosszú legyen hello adatok?
Nyers adatok pontok (Ez azt jelenti, hogy elemekre Analytics-lekérdezést, és vizsgálja meg a keresési) mentése too90 nap tartanak. Ha hosszabb, mint tookeep adatok van szüksége, használhatja [a folyamatos exportálás](app-insights-export-telemetry.md) toocopy azt tooa tárfiók.

Összesített adatokat (Ez azt jelenti, számok, átlagok és más szereplő metrikája Explorer statisztikai adatokat), egy olyan aggregációs időközt 90 napig 1 perces megmaradnak.

## <a name="who-can-access-hello-data"></a>Hello adatok hozzáférő felhasználók?
hello adata látható tooyou és, ha a szervezeti fiókkal rendelkezik, a csoport tagjai. 

Az Ön és a csoport tagjai által exportálhatók és lehet, másolt tooother helyek, tooother személyek átadja.

#### <a name="what-does-microsoft-do-with-hello-information-my-app-sends-tooapplication-insights"></a>Milyen biztosítja a Microsoft teendő velük a begyűjtésük hello alkalmazásom küld tooApplication Insights?
A Microsoft hello adatokat csak a rendelés tooprovide hello szolgáltatás tooyou használja.

## <a name="where-is-hello-data-held"></a>Hello adatok tárolási helye?
* Hello USA vagy Európa. Kiválaszthatja a hello helyét, amikor létrehoz egy új Application Insights-erőforrást. 


#### <a name="does-that-mean-my-app-has-toobe-hosted-in-hello-usa-or-europe"></a>Amely azt jelenti a az alkalmazás rendelkezik hello USA vagy Európa üzemeltetett toobe?
* Nem. Az alkalmazás bárhol futhatnak, vagy a saját helyszíni állomások vagy a hello felhő.

## <a name="how-secure-is-my-data"></a>Hogy mennyire vannak biztonságban vannak az adatok?
Az Application Insights egy olyan Azure-szolgáltatás. Biztonsági házirendek ismerteti a hello [Azure biztonsági, adatvédelmi és megfelelőségi tanulmány](http://go.microsoft.com/fwlink/?linkid=392408).

a Microsoft Azure-kiszolgálók hello adatokat tárolja. Hello Azure portálon lévő fiókokhoz, ismerteti a fiók korlátozásai hello [Azure biztonsági, adatvédelmi és megfelelőségi dokumentum](http://go.microsoft.com/fwlink/?linkid=392408).

Hozzáférés tooyour adatok Microsofton korlátozódik. Jelenleg csak az Ön engedélyével az adatokat, és esetén szükséges toosupport a használatára az Application Insights. 

A felhasználók alkalmazásokat (például adatforgalmi díjakat és nyomkövetési átlagos mérete) keresztül összesítő adatai használt tooimprove Application insights szolgáltatással.

#### <a name="could-someone-elses-telemetry-interfere-with-my-application-insights-data"></a>Akadályozhatja valaki más telemetriai Application Insights adataimat?
További telemetriai tooyour fiók hello instrumentation kulccsal, amely a weblapok hello kódban található azok küldhet. Elegendő további adatokkal a metrikákat, akkor nem megfelelően képviselje az alkalmazás teljesítményét és használatának.

Ha a kód megosztása más projektek, ne feledje tooremove a rendszerállapot-kulcs.

## <a name="is-hello-data-encrypted"></a>Hello adatok titkosítva van?
Nincs a jelenlegi hello kiszolgálók belül.

Összes adata titkosításra kerül az adatközpontok közötti átvitel során.

#### <a name="is-hello-data-encrypted-in-transit-from-my-application-tooapplication-insights-servers"></a>Az átvitel során, az alkalmazás tooApplication Insights kiszolgálókról Titkosított hello adatokat?
Igen, a https toosend adatok toohello portálról, szinte minden SDK-k, beleértve a webkiszolgálók, eszközök és HTTPS weblapok használjuk. hello egyetlen kivétel, egyszerű HTTP-weblapokhoz küldött adatokat. 

## <a name="personally-identifiable-information"></a>Személyes azonosításra alkalmas adatok
#### <a name="could-personally-identifiable-information-pii-be-sent-tooapplication-insights"></a>Sikerült személyes azonosításra alkalmas adatokat (PII) elküldeni tooApplication Insights?
Igen, akkor lehet. 

Általános útmutatásként:

* Legtöbb szabványos telemetriai adatokat (Ez azt jelenti, hogy programozás nélkül telemetria) nem tartalmaz explicit személyhez köthető adatokat. Azonban lehetséges tooidentify egyének megállapítás egy gyűjteményből események által lehet.
* Kivétel és nyomkövetési üzenetek tartalmazhatnak személyhez köthető adatokat
* Egyéni telemetria - Ez azt jelenti, például a kód megadása hello API-t vagy a napló nyomkövetések az írást TrackEvent hívások - választja adatokat is tartalmazhat.

Ez a dokumentum végén hello hello tábla hello adatgyűjtés részletes leírását tartalmazza.

#### <a name="am-i-responsible-for-complying-with-laws-and-regulations-in-regard-toopii"></a>Tudom törvény és szabályozás a legutóbb tooPII felel?
Igen. A felelősség tooensure, amely megfelel törvény és szabályozás, valamint hello Microsoft Online Services használati hello gyűjtése és felhasználása hello adatok is.

Az ügyfelek megfelelően kell tájékoztatnia hello adatokat gyűjt az alkalmazás és a hello adatok felhasználásáról.

#### <a name="can-my-users-turn-off-application-insights"></a>A felhasználók kikapcsolható Application Insights?
Közvetlenül nem. A kapcsoló nem nyújtunk, hogy a felhasználók kapnak tooturn Application Insights ki.

A funkció azonban az alkalmazás is létrehozható. Minden hello SDK-k többek között a egy API-t, hogy kikapcsolja a telemetriai adatok gyűjtése. 

#### <a name="my-application-is-unintentionally-collecting-sensitive-information-can-application-insights-scrub-this-data-so-it-isnt-retained"></a>Saját alkalmazás véletlenül bizalmas információkat gyűjt. Is az Application Insights megtisztítás ezeket az adatokat, akkor nem marad?
Az Application Insights szűrése vagy nem törli az adatokat. Hello adatok kezelésében az megfelelően kell, és ne küldjön az ilyen adatok tooApplication Insights.

## <a name="data-sent-by-application-insights"></a>Az Application Insights által küldött adatokat
hello SDK-k platformok változhat, és több összetevőből is telepítheti. (Lásd túl[Application Insights – áttekintés][start].) Minden egyes összetevő különböző adatokat küld.

#### <a name="classes-of-data-sent-in-different-scenarios"></a>A különböző alkalmazási helyzetek küldött adatok osztályok
| A művelet | Adatosztályok gyűjtött (lásd a következő táblázatban) |
| --- | --- |
| [Application Insights SDK tooa .NET webes projekt hozzáadása][greenbrown] |Kiszolgálói környezet<br/>Következtetni<br/>Teljesítményszámlálói<br/>Kérelmek<br/>**Kivételek**<br/>Munkamenet<br/>felhasználók |
| [Állapotmonitor telepítése az IIS-kiszolgálón][redfield] |Függőségek<br/>Kiszolgálói környezet<br/>Következtetni<br/>Teljesítményszámlálói |
| [Application Insights SDK tooa Java-webalkalmazás hozzáadása][java] |Kiszolgálói környezet<br/>Következtetni<br/>Kérés<br/>Munkamenet<br/>felhasználók |
| [JavaScript SDK tooweb-weblap hozzáadása][client] |ClientContext <br/>Következtetni<br/>Lap<br/>ClientPerf<br/>AJAX |
| [Alapértelmezett tulajdonságok meghatározása][apiproperties] |**Tulajdonságok** összes szabványos és az egyéni esemény |
| [Hívás TrackMetric][api] |Numerikus értékek<br/>**Tulajdonságok** |
| [Hívás követése *][api] |esemény neve<br/>**Tulajdonságok** |
| [Hívás TrackException][api] |**Kivételek**<br/>Veremkiíratás<br/>**Tulajdonságok** |
| SDK nem gyűjt adatokat. Példa: <br/> -teljesítményszámlálói nem érhető el.<br/> -Kivétel fordult elő a telemetriai adatok inicializáló |SDK-diagnosztika |

A [más platformokhoz készült SDK-k][platforms], tekintse meg a dokumentumokat.

#### <a name="hello-classes-of-collected-data"></a>az összegyűjtött adatokat hello osztályok
| Összegyűjtött adatok osztály | (Nem teljesnek) tartalmazza. |
| --- | --- |
| **Tulajdonságok** |**Adatok - határozza meg a kódot** |
| DeviceContext |Azonosító, IP, területi beállítás esetén eszközmodell, hálózati, hálózati típusa, az OEM neve, képernyőfelbontás Szerepkörpéldányt, a szerepkör neve, az eszköz típusa |
| ClientContext |Az operációs rendszer, területi beállítás, nyelv, hálózati, ablakban felbontás |
| Munkamenet |munkamenet-azonosító |
| Kiszolgálói környezet |Számítógép neve, területi beállítás, az operációs rendszer, a eszköz, a felhasználói munkamenet, a felhasználói környezet, a művelet |
| Következtetni |földrajzi hely, IP-cím, timestamp, az operációs rendszer, böngésző |
| Mérőszámok |Metrika neve és értéke |
| Események |Az esemény neve és értéke |
| PageViews |URL-cím és a lap neve vagy a képernyő neve |
| Ügyfél-teljesítmény |URL-cím vagy a lap neve, a böngésző lapbetöltési ideje |
| AJAX |A weblap tooserver HTTP-hívások |
| Kérelmek |URL-címe, időtartama, válaszkód |
| Függőségek |Típus (SQL, HTTP,...), a kapcsolati karakterlánc vagy a URI, szinkronizálási vagy aszinkron, időtartama, sikeres, SQL-utasítás (az állapotfigyelő) |
| **Kivételek** |Típus, **üzenet**, verem hívja, a forrás-fájl és a sor száma, Szálazonosító |
| Összeomlások |Folyamatazonosító, szülő folyamatazonosító, összeomlási szálazonosító; alkalmazás-javítás, a-azonosító, a build;  Kivétel típusa, a cím, a reason; rejtjelezett szimbólumok és regiszterekben, bináris kezdő és záró címek, bináris nevek és elérési útja, a processzor típusa |
| Nyomkövetés |**Üzenet** és súlyossági szint |
| Teljesítményszámlálói |Processzor kihasználtsága, rendelkezésre álló memória, lekérdezési gyakorisága, kivétel sebessége, folyamat saját bájtok, IO sebessége, kérelem időtartama, kérelem-várólista hossza |
| Rendelkezésre állás |Webalkalmazás-teszt válaszkód, időtartama minden teszt lépés, teszt neve, timestamp, sikeres, válaszideje, teszt helye |
| SDK-diagnosztika |Nyomkövetési üzenet vagy kivétel |

Is [ApplicationInsights.config szerkesztésével kikapcsolni hello adatok egy részét][config]

## <a name="credits"></a>Kreditek
A termék MaxMind, elérhető által létrehozott GeoLite2 adatokat tartalmaz [http://www.maxmind.com](http://www.maxmind.com).



<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apiproperties]: app-insights-api-custom-events-metrics.md#properties
[client]: app-insights-javascript.md
[config]: app-insights-configuration-with-applicationinsights-config.md
[greenbrown]: app-insights-asp-net.md
[java]: app-insights-java-get-started.md
[platforms]: app-insights-platforms.md
[pricing]: http://azure.microsoft.com/pricing/details/application-insights/
[redfield]: app-insights-monitor-performance-live-website-now.md
[start]: app-insights-overview.md

