---
title: "aaaWeb alkalmazásteljesítmény-figyelő - Azure Application Insights |} Microsoft Docs"
description: Hogyan illeszkedik az Application Insights hello devOps ciklus
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 479522a9-ff5c-471e-a405-b8fa221aedb3
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 03/14/2017
ms.author: bwren
ms.openlocfilehash: bba2d6c59de1794adcbf8e298d0ef4f0dbaa700f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deep-diagnostics-for-web-apps-and-services-with-application-insights"></a>Webalkalmazások és szolgáltatások részletes diagnosztikája az Application Insights szolgáltatással
## <a name="why-do-i-need-application-insights"></a>Miért kell az Application Insights?
Az Application Insights figyeli a futó webes alkalmazást. Jelzi, hogy kapcsolatos hibáiról és teljesítményproblémáiról, és segít elemzése, hogy az ügyfelek hogyan használják az alkalmazást. (J2EE, az ASP.NET, Node.js,...) számos platformon futó alkalmazások működését, és hello felhőalapú vagy helyszíni van-e tárolva. 

![Webalkalmazások elérése hello összetettségi szempontok](./media/app-insights-devops/010.png)

Alapvető toomonitor a modern alkalmazás futása közben is. A legfontosabb érdemes toodetect hibák az ügyfelek többsége előtt. Is szeretné, hogy toodiscover, és javítsa ki a teljesítménnyel kapcsolatos problémákat, amelyek, során nem végzetes, lehet, hogy lassítják a gépet, vagy bizonyos kellemetlenségért tooyour felhasználók okozhat. És hello rendszer tooyour elégedettségének hajt végre, ha azt szeretné, a felhasználók milyen hello tűnik, hogy minden vele tooknow: hello legújabb funkciójával? Ezek sikeresen lezajlottak vele?

Modern webalkalmazások fejlesztett folyamatos szállítási ciklusban: kiadás egy új funkció vagy fokozása; Figyelje meg, milyen jól működik a hello felhasználók; Tervezze meg ezt az információt alapján fejlesztési hello tovább biztonsági. Ez a ciklus kulcsfontosságú része hello megfigyelési fázis. Az Application Insights az hello eszközök toomonitor a teljesítmény- és használati webes alkalmazásokat tartalmaz.

hello egyik legfontosabb szempontja, hogy ez a folyamat diagnosztika és elemzés céljából. Ha nem sikerül hello alkalmazást, majd üzleti van elvesztését. hello elsődleges szerepkör figyelési keretrendszer ezért toodetect hibák megbízhatóan, értesíti, akkor azonnal és toopresent hello adatokkal kellett toodiagnose hello probléma. Ez az pontosan, az Application Insights funkciója.

### <a name="where-do-bugs-come-from"></a>Honnan származnak hibák?
A webes rendszerek hibák jellemzően akkor keletkeznek, konfigurációs problémák vagy a számos összetevő közötti hibás interakciókat. ezért tooidentify hello helyi hello probléma hello első feladatot, amikor egy élő webhelyet incidens elleni: mely összetevő vagy a kapcsolat: hello OK?

Néhány velünk, szürke haj rendelkező emlékszik egy egyszerűbb éra egy számítógép a programot futtatásakor. hello fejlesztők volna tesztelheti alaposan; mielőtt és azt, hogy rendszerrel szállított volna ritkán tekintse meg vagy gondolja át azt újra. hello felhasználók kellene tooput a hello maradék hibák sok éve. 

Minden olyan így nagyon különböző most. Az alkalmazás különböző eszközök toorun formátum rendelkezik, és nehéz tooguarantee hello pontos kívánt viselkedést eredményező beállítást az egyes lehet. Hello felhőben található alkalmazásokat futtató azt jelenti, hogy gyors hibák lehet meghatározni, de azt is jelenti, folyamatos verseny és hello várt értéket az új funkciók gyakori időközönként. 

Ezek a feltételek a hello hello hiba száma fixen vezérlő csak úgy tookeep automatikus egységek tesztelése. Nem lehet toomanually mindent minden kézbesítés újra teszteljen. Egységteszt most hello alkotómunkájának részét létrehozása folyamatban van. Például hello Xamarin teszt felhőalapú eszközök, adja meg a böngésző-verziókban tesztelés automatizált felhasználói felület segítségével. Ezek a rendszerek tesztelés lehetővé teszik a számunkra toohope, hogy egy alkalmazás belül található hibák hello arányát is meg kell őrizni tooa minimális.

Tipikus webalkalmazások számos élő összetevő rendelkezik. Továbbá toohello ügyfél (alkalmazásban egy böngésző vagy eszköz) és hello webkiszolgáló, az valószínűleg toobe jelentős háttér feldolgozásával van. Lehet, hogy hello háttér egy folyamatot, az összetevők, vagy együttműködés darab lazább gyűjteménye. És ezek a vezérlőben nem lehet – külső, amelyen függő szolgáltatások fontosságúak.

Ezek például konfigurációk azt is, nehéz és uneconomical tootest vagy várható, minden lehetséges hibaállapotba, eltérő hello élő a rendszer. 

### <a name="questions-"></a>Kérdés...
Jelenleg folyamatban fejlesztésekor, hogy egy webes rendszer megkérjük kérdések merülhetnek fel:

* Saját alkalmazás összeomló? 
* Pontosan történtekről? -Ha a kérelem sikertelen volt, hogyan, nem kapott tooknow szeretnék. Igazolnia kell a nyomkövetési események...
* Elég gyorsan megnövelni az alkalmazásom? Mennyi ideig tart toorespond tootypical kérelmek?
* Hello server leíró hello be tudják tölteni? Hello kérelmek száma nő, amikor nem hello válaszidő tartsa állandó?
* Mennyire reagáljon a függőségek - hello REST API-k, adatbázisok és más összetevőket, saját alkalmazás meghívja a rendszer. Ebben az esetben ha hello rendszer lassú, azt a összetevő, vagy jelenik meg lassú válasz a valaki más?
* Felfelé vagy lefelé, az alkalmazásom? Ez látható a hello világ? Értesítsen Ha leállítása...
* Mi az az alapvető ok hello? Az összetevő vagy egy függőséget hello hiba történt? Ennyi az egész egy kommunikációs hiba?
* Hány felhasználó érintett? Ha egynél több problémát tootackle, ami a legfontosabb hello?

## <a name="what-is-application-insights"></a>Mi az Application Insights?
![Az Application Insights alapvető munkafolyamata](./media/app-insights-devops/020.png)

1. Az Application Insights eszközök az alkalmazást, és telemetriai adatainak azt küldi hello alkalmazás futtatása közben. Hello Application Insights SDK hello alkalmazásba hozhat létre, vagy alkalmazhat instrumentation futásidőben. hello első módszer esetén rugalmasabb, mert a saját telemetriai toohello rendszeres modulokat is hozzáadhat.
2. hello telemetriai toohello Application Insights portáljáról, amennyiben tárolása és feldolgozása zajlik. (Bár az Application Insights a Microsoft Azure-ban üzemel, figyelheti minden webes alkalmazás - csak Azure apps.)
3. hello telemetriai adat áll rendelkezésre tooyou hello formátumú diagramok és események táblában.

Telemetria két fő típusa van: nyers, összesített és példányok. 

* Példányok adatait tartalmazza, például a webes alkalmazás által fogadott kérelem jelentést. Található, és vizsgálja meg a hello keresési eszközzel hello Application Insights portál kérelmet hello részleteit. hello példány volna adatok, például, hogy mennyi ideig tartott az az alkalmazás a toorespond toohello kérelem tartalmazza, valamint hello a kért URL-cím, hello ügyfél hozzávetőleges tartózkodási helyét és egyéb adatok.
* Összesített adatokat tartalmaz események száma egység idő összesítése összehasonlíthatja hello Alkalmazásverzió hello válaszidejét. A kérelem válaszidejének például mérőszámok átlagok is tartalmaz.

hello fő kategóriába az adatok a következők:

* Kérelmek tooyour app (általában a HTTP-kérések) URL-címet, a válaszidőt, és a sikeres vagy sikertelen adatokkal.
* Függőségek - által az alkalmazás, is URI, válaszidejét és sikerességi REST és SQL hívások
* Kivételek, beleértve a híváslánc megjelenik.
* Lapmegtekintések adatainak, amely hello felhasználók böngészőjének származik.
* Például a teljesítményszámlálók metrikák, valamint metrikák lehet írni. 
* Egyéni események használható tootrack üzleti események
* A naplókivonatokat hibakereséshez.

## <a name="case-study-real-madrid-fc"></a>Esettanulmány: Valós Madridi F.C.
a webszolgáltatás hello [valós Madridi bemutatjuk Club](http://www.realmadrid.com/) hello világ készül 450 millió ventilátorok szolgál. Ventilátorok webböngészők keresztül érhető el mindkét és klub mobilalkalmazások hello. Ventilátorok is nemcsak lefoglalhatja a jegyektől, de is elérhetik az adatokat és a videó videóklipeket eredmények, a játékosok és a közelgő játékok. Akkor is keresési szűrők célok számú program pontozza a mennyiségeket. Hivatkozások toosocial adathordozó is vannak. hello felhasználói élmény nagymértékben testreszabott, és úgy van kialakítva, mint a kétirányú kommunikációt tooengage ventilátor.

megoldás hello [egy rendszer, a szolgáltatások és alkalmazások a Microsoft Azure](https://www.microsoft.com/en-us/enterprise/microsoftcloud/realmadrid.aspx). A méretezhetőség azt a kulcsfontosságú követelmény: forgalom változó, és képes elérni nagyon nagy kötetek során, és megfelel körül.

Valós Madridi is létfontosságú toomonitor hello rendszer teljesítményét. Azure Application Insights lehetővé teszi az átfogó képet kaphat hello rendszer, a megbízható és magas szintű szolgáltatást biztosítva. 

hello Club is lekérdezi a ventilátorok részletes ismertetése: hol vannak (csak 3 % vannak Spanyolország), lejátszókról, korábbi eredmények jövőbeli játékok és hogyan válaszoljanak toomatch eredményekkel rendelkeznek milyen iránt.

A telemetriai adatok automatikusan összegyűjtött nem hozzáadott kódra, amely egyszerűsített hello megoldás és a működési összetettsége csökken.  Valós Madridi, az Application Insights foglalkozik 3.8 milliárd telemetriai pontok minden hónapban.

Valós Madridi használ hello Power BI modul tooview a telemetriai adatokat.

![Az Application Insights telemetria ábrázolása a Power BI](./media/app-insights-devops/080.png)

## <a name="smart-detection"></a>Intelligens észlelése
[Proaktív diagnosztika](app-insights-proactive-diagnostics.md) legutóbbi szolgáltatása. Ön semmiféle speciális beállítást nélkül a Application Insights automatikusan észleli, és értesítést kaphat a az alkalmazás hiba díjszabás szokatlan emelkedést. Ez elég intelligens tooignore alkalmi hibák háttér, és is, amelyek emelkedést egyszerűen arányos tooa megnövekedhet a kérelmeket. Így például ha hiba történik a hello szolgáltatást, attól függ, vagy ha új hello build csak telepített nem működik úgy is, akkor tudhatja, információt, amint az e-maileket tekinti meg. (És webhookokkal, hogy más alkalmazásokat indíthat el.)

Ez a szolgáltatás egy másik aspektusa hajt végre a telemetriai, teljesítmény, amelyek merevlemez toodiscover szokatlan mintáinak keres egy napi részletes elemzéséhez. Például egy adott földrajzi területen, vagy egy adott böngészőverzió kapcsolódó teljesítménycsökkenést talál.

Mindkét esetben hello a riasztás nem csak azt ismerteti, hello jelenségek fel van derítve, de is biztosítja az adatok toohelp van szüksége, diagnosztizálása hello probléma, például a jelentések vonatkozó kivétel.

![A proaktív diagnosztika e-mailekhez](./media/app-insights-devops/030.png)

Ügyfél Samtec említett: "cutover legutóbbi funkció során észleltünk egy alapján méretezi adatbázis elérte-e az erőforrás-korlátozások és időtúllépéseket okoz. A proaktív azonosítási riasztások érkezett hirdetett azt volt triaging hello probléma nagyon közel valós idejű szó. Ez a riasztás hello Azure platformon riasztások alapján kialakulhat segített velünk szinte azonnal a hello probléma megoldásához. Összesített állásidő < 10 perc."

## <a name="live-metrics-stream"></a>Élő Stream metrikák
Hello legújabb buildjével telepítése lehet törekedve arra élményt. Ha problémákat, kívánt róluk tooknow azonnal, úgy, hogy szükség esetén készítsen biztonsági. Élő metrikák adatfolyam lehetővé teszi az alapvető metrikákat körülbelül egy második késéssel.

![Élő metrikák](./media/app-insights-devops/040.png)

És lehetővé teszi, hogy azonnal ellenőrizze a hibákat és kivételeket minta.

![Élő hibaesemények](./media/app-insights-devops/live-stream-failures.png)

## <a name="application-map"></a>Alkalmazástérkép
Alkalmazás-hozzárendelés automatikusan észleli a az alkalmazás topológiát hello teljesítményadatok utasítást, egyszerű azonosításában teljesítmény szűk keresztmetszetei és a problematikus adatfolyamok teljes az elosztott környezet toolet elrendezése. Lehetővé teszi toodiscover alkalmazásfüggőségek a Azure-szolgáltatásokra. Ha kód kapcsolatos ismertetése vagy a kapcsolódó függőségi hello probléma osztályozhatja, és a kapcsolódó diagnosztikai egy egyetlen hely részletek tapasztalhat. Például az alkalmazás sikertelenségét tooperformance teljesítménycsökkenést SQL réteg miatt. Az alkalmazás-hozzárendelés azonnal látni, és elemezze a hello SQL Index Advisor vagy lekérdezési élmény.

![Alkalmazástérkép](./media/app-insights-devops/050.png)

## <a name="application-insights-analytics"></a>Application Insights elemzés
A [Analytics](app-insights-analytics.md), tetszőleges lekérdezéseket írhat egy hatékony SQL-szerű nyelven.  Hello egész alkalmazás verem között diagnosztizálása lesz könnyen, különböző szempontok szerint felveheti a kapcsolatot, és kérje meg hello megfelelő kérdéseket tehesse fel toocorrelate szolgáltatás teljesítmény üzleti mérőszámok és a felhasználói élmény. 

Lekérheti az telemetria-példány és hello portálon tárolt metrika nyers adatok. hello nyelvi szűrő, illesztési, összesítési és más műveleteket tartalmaz. Kiszámíthatja a mezők és statisztikai elemzést. Nincsenek a tabular és a grafikus képi megjelenítések.

![Lekérdezés- és eredmények diagramra](./media/app-insights-devops/025.png)

Például is könnyen:

* Az alkalmazás kérelem teljesítményadatokat ügyfél tiers toounderstand tapasztalataikat szegmentálhatja.
* Keresés adott hibakódok vagy egyéni esemény élő webhelyet vizsgálatok során.
* Részletekbe menően tárhatják fel az adott ügyfélnek toounderstand hello Alkalmazáshasználat szolgáltatások szerzett és elfogadott hogyan.
* Nyomon követheti a munkamenetek és válaszidejének előrejelzését bizonyos felhasználók tooenable támogatás és műveletek csapatok tooprovide azonnali ügyfél-támogatási szolgálatához.
* Határozza meg a gyakran használt alkalmazás szolgáltatások tooanswer szolgáltatás rangsorolási kérdéseket.

Ügyfél DNN említett: "Application Insights már rendelkezik velünk hello ahhoz, hogy meg tudja toocombine, rendezés, lekérdezés és adatok szűrése, igény szerint hello egyenlet egy része hiányzik. Így a csapat toouse saját nyújt és felhasználói élményt toofind hatékony lekérdezésnyelvet adatok engedélyezték, velünk toofind insights és a problémák megoldása jelenleg nem fogják tudni volt. Nagy mennyiségű érdekes válaszok származhat kezdve hello kérdések *"I lett, ha...".*"

## <a name="development-tools-integration"></a>Fejlesztői eszközök integráció
### <a name="configuring-application-insights"></a>Az Application Insights konfigurálása
A Visual Studio és az Eclipse rendelkezik eszközök tooconfigure hello megfelelő SDK csomagok hello projekt fejleszt. A menü parancs tooadd Application Insights van.

Log4N, NLog vagy System.Diagnostics.Trace nyomkövetési naplózási keretrendszer használatával toobe fordulhat elő, ha majd kap hello beállítás toosend hello naplók tooApplication Insights hello együtt egyéb telemetriai adatokat, így egyszerűen hozhatók hello nyomkövetések kérések , függőségi hívások esetében, és kivételeket.

### <a name="search-telemetry-in-visual-studio"></a>A Visual Studio keresési telemetria
Fejlesztési, és egy szolgáltatás hibakeresési, miközben megtekintése és keresése hello telemetriai közvetlenül a Visual Studio eszközből hello azonos keresési létesítményekben, hello web portal alkalmazásban.

És az Application Insights kivétel naplózza, ha a Visual Studio hello adatpont tekintheti meg és jump egyenes toohello megfelelő kódot.

![A Visual Studio-keresés](./media/app-insights-devops/060.png)

Hibakeresési, vannak hello beállítás tookeep hello telemetriai adatokat a fejlesztői számítógépén, megtekintés, a Visual Studio alkalmazásban, de toohello portal küldés nélkül. A helyi beállítás elkerüli a termelési telemetriai feltárására keverése.

### <a name="build-annotations"></a>Jegyzetek létrehozása
Ha a Visual Studio Team Services toobuild használja, és telepítse az alkalmazást, és a központi telepítési jegyzetek diagramok hello portálon jelenik meg. Ha a legújabb kiadás hatása, ha a hello metrikákat, válik egyértelművé.

![Jegyzetek létrehozása](./media/app-insights-devops/070.png)

### <a name="work-items"></a>A munkaelemek
Riasztást hoz létre, amikor az Application Insights automatikusan létrehozhat az munkaelem nyomkövetési rendszer munkáját.

## <a name="but-what-about"></a>De mi a helyzet...?
* [Adatvédelem és a tárolási](app-insights-data-retention-privacy.md) -a telemetriai adatok másolatok Azure kiszolgálók védelmének biztosításához.
* Teljesítmény - hello szempontjából nagyon alacsony. Telemetria kötegelni van.
* [Árképzési](app-insights-pricing.md) - szabad kezdheti, és, hogy továbbra is fennáll, kis terjedelmű közben.


## <a name="video"></a>Videó

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a>Következő lépések
Ismerkedés az Application Insights szolgáltatással is könnyen. hello fő lehetőségek közül választhat:

* Egy már futó webalkalmazás állíthatnak be. Ez lehetővé teszi az összes hello beépített teljesítmény telemetriai adat. Használható a [Java](app-insights-java-live.md) és [IIS-kiszolgálókkal](app-insights-monitor-performance-live-website-now.md), és is [Azure-webalkalmazásokban](app-insights-azure.md).
* A projekt állíthatnak be a fejlesztés során. Ehhez [ASP.NET](app-insights-asp-net.md) vagy [Java](app-insights-java-get-started.md) alkalmazások esetén, valamint [Node.js](app-insights-nodejs.md) és [más](app-insights-platforms.md). 
* Eszköz [bármilyen weblap](app-insights-javascript.md) egy rövid kódrészletet hozzáadásával.

