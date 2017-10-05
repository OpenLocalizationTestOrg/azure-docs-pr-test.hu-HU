---
title: "Webhelyek rendelkezésre állásának és válaszkészségének megfigyelése | Microsoft Docs"
description: "Webes teszteket állíthat be az Application Insightsban. Riasztásokat kaphat, ha egy webhely elérhetetlenné válik vagy lassan válaszol."
services: application-insights
documentationcenter: 
author: SoubhagyaDash
manager: carmonm
ms.assetid: 46dc13b4-eb2e-4142-a21c-94a156f760ee
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/25/2017
ms.author: bwren
ms.openlocfilehash: 6c7f52fc3998b0b29301206ffbc6a5a0c4134f6a
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="monitor-availability-and-responsiveness-of-any-web-site"></a>Webhelyek rendelkezésre állásának és válaszkészségének megfigyelése
Miután telepítette a webappot vagy a webhelyet bármely kiszolgálóra, webes teszteket állíthat be az alkalmazás rendelkezésre állásának és válaszkészségének megfigyeléséhez. Az [Azure Application Insights](app-insights-overview.md) rendszeres időközönként, világszerte különböző helyekről webes kéréseket küld az alkalmazására. Riasztást jelenít meg, ha az alkalmazás nem válaszol, vagy lassan válaszol.

Rendelkezésre állási teszteket állíthat be bármely olyan HTTP- vagy HTTPS-végponthoz, amely a nyilvános internetről érhető el. Nem kell semmit hozzáadnia a tesztelt webhelyhez. Még csak arra sincs szükség, hogy a saját webhelye legyen: tesztelhet egy olyan REST API-t is, amelyet használni szokott.

A rendelkezésre állási teszteknek két típusuk van:

* [URL-ping teszt](#create): egyszerű teszt, amelyet az Azure Portalon hozhat létre.
* [Többlépéses webes teszt](#multi-step-web-tests): ezt a Visual Studio Enterprise segítségével hozhatja létre és feltöltheti a portálra.

Alkalmazás-erőforrásonként legfeljebb 25 rendelkezésre állási tesztet hozhat létre.

## <a name="create"></a>1. Erőforrás megnyitása a rendelkezésre állási tesztek jelentéseihez

**Ha már konfigurálta az Application Insights szolgáltatást** a webapphoz, nyissa meg a hozzá tartozó Application Insights-erőforrást [az Azure Portalon](https://portal.azure.com).

**Vagy ha a jelentéseket új erőforrásban szeretné megtekinteni,** regisztráljon a [Microsoft Azure](http://azure.com)-ba, lépjen az [Azure Portalra](https://portal.azure.com), és hozzon létre egy Application Insights-erőforrást.

![Új > Application Insights](./media/app-insights-monitor-web-app-availability/11-new-app.png)

Az új erőforrás Áttekintés paneljének megnyitásához kattintson az **Összes erőforrás** lehetőségre.

## <a name="setup"></a>2. URL-ping teszt létrehozása
Nyissa meg a Rendelkezésre állás panelt, és adjon hozzá egy tesztet.

![Töltse ki legalább a webhelye URL-címét](./media/app-insights-monitor-web-app-availability/13-availability.png)

* **Az URL-cím** bármilyen tesztelni kívánt weblap lehet, de láthatónak kell lennie a nyilvános internetről. Az URL-cím tartalmazhat lekérdezési karakterláncot. Tehát például kísérletezhet egy kicsit az adatbázissal. Ha az URL feloldása egy átirányítást eredményez, legfeljebb 10 átirányításig követjük.
* **Függő kérelmek elemzése**: Ha a beállítás be van jelölve, a teszt lekéri a tesztelt weblap képeit, szkriptjeit, stílusfájljait és más erőforrásait. A rögzített válaszidőbe a fájlok lekérése is beleszámít. A teszt meghiúsul, ha nem tölthető le sikeresen az összes erőforrás a teljes teszt időtúllépése előtt. 

    Ha a beállítás nincs bejelölve, a teszt csak a fájlt és a megadott URL-címet kéri le.
* **Újrapróbálkozások engedélyezése**: Ha ez a beállítás be van jelölve, és a teszt meghiúsul, a rendszer rövid idő után újrapróbálkozik. Csak akkor jelent hibát, ha három egymást követő kísérlet meghiúsul. Ezután a rendszer a teszteket a szokásos tesztelési gyakorisággal végzi el. Az újrapróbálkozás ideiglenesen fel van függesztve a következő sikeres műveletig. Ez a szabály függetlenül van alkalmazva minden egyes teszthelyen. Ezt a beállítást javasoljuk. Átlagosan körülbelül a hibák 80%-a megszűnik az újrapróbálkozáskor.
* **Teszt gyakorisága**: Beállítja, hogy milyen gyakran fut a teszt mindegyik teszthelyen. Öt perces gyakorisággal és öt teszthellyel a helyén átlagosan percenként egy teszt történik.
* A **Teszthelyek** olyan helyek, ahonnan a kiszolgálóink webes kérelmeket küldenek az Ön URL-címére. Többet válasszon, hogy megkülönböztethesse webhelye problémáit a hálózati hibáktól. Legfeljebb 16 hely választható ki.
* **Sikeresség feltétele**:

    **Teszt időkorlátja:** Ha csökkenti az értéket, riasztást kap a lassú válaszokról. A teszt akkor sikertelen, ha nem érkeznek meg a webhely válaszai ezen az időtartamon belül. Ha bejelölte a **Függő kérelmek elemzése** lehetőséget, akkor az összes képnek, stílusfájlnak, szkriptnek és más függő erőforrásnak meg kell érkeznie ezen az időn belül.

    **HTTP-válasz**: A visszaadott, sikert jelző állapotkód. A 200-as kód jelzi, hogy normál weblap lett visszaküldve.

    **Tartalmi egyezés**: egy karakterlánc, például „Üdvözöljük!” Teszteljük, hogy minden válaszban előfordul-e a kis- és nagybetűket figyelembe véve is pontos egyezés. Egyszerű karakterláncnak kell lennie helyettesítő karakterek nélkül. Ne feledje, hogy ha a laptartalom megváltozik, lehet, hogy ezt is frissíteni kell.
* Alapértelmezés szerint akkor kap **riasztásokat**, ha öt perc alatt három helyen fordulnak elő hibák. Ha egy helyen fordul elő a hiba, azt valószínűleg hálózati probléma okozza, és nem a hellyel van probléma. A küszöbérték érzékenységét állítani lehet, és azt is módosíthatja, hogy kinek küldje a rendszer az e-maileket.

    Be lehet állítani egy [webhookot](../monitoring-and-diagnostics/insights-webhooks-alerts.md), amelyet a rendszer egy riasztás megjelenésekor hív meg. (Vegye figyelembe, hogy jelenleg a lekérdezési paraméterek nem továbbítódnak Tulajdonságokként.)

### <a name="test-more-urls"></a>További URL-címek tesztelése
Adjon hozzá további teszteket. Például a kezdőlap tesztelése mellett egy keresés URL-címének tesztelésével ellenőrizni lehet, hogy az adatbázis működik-e.


## <a name="monitor"></a>3. A rendelkezésre állási teszt eredményeinek megtekintése

Néhány perc elteltével kattintson a **Frissítés** elemre a teszteredmények megtekintéséhez. 

![Az összegzés eredményei a kezdőpanelen](./media/app-insights-monitor-web-app-availability/14-availSummary-3.png)

A pontdiagram mintákat mutat be azokból a teszteredményekből, amelyek adatokat tartalmaznak a diagnosztikai teszt lépéseiről. A tesztmotor tárolja a hibákat tartalmazó tesztek diagnosztikai adatait. A sikeres tesztek esetében a végrehajtások részhalmazainak diagnosztikai adatait is tárolja. Vigye a mutatót a zöld vagy a piros pontokra a teszt időbélyegzőjének, időtartamának, helyének és nevének megjelenítéséhez. Kattintson a pontdiagram bármelyik pontjára a részletes teszteredmények megtekintéséhez.  

Ha további adatokat szeretne látni egy konkrét időszakról, válassza ki a megfelelő tesztet vagy helyet, illetve állítsa rövidebbre az időszakot. A Keresési ablak használatával megtekintheti az összes végrehajtás eredményét, vagy az Analytics lekérdezéseinek használatával egyéni jelentéseket hozhat létre ezen adatok alapján.

A nyers eredményeken kívül két rendelkezésre állási metrika is elérhető a Metrikaböngészőben: 

1. Rendelkezésre állás: Az összes végrehajtott teszt közül a sikeresen végrehajtott tesztek százalékos aránya. 
2. Tesztek időtartama: A tesztek átlagos időtartama az összes végrehajtás alapján.

Az eredményeket szűrheti a teszt neve vagy a hely alapján adott tesztek és/vagy helyek trendjeinek elemzéséhez.

## <a name="edit"></a> Tesztek megtekintése és szerkesztése

Az összegzés lapon válasszon ki egy tesztet. Itt megtekintheti a teszt eredményeit, és szerkesztheti vagy átmenetileg letilthatja a tesztet.

![Webes teszt szerkesztése vagy letiltása](./media/app-insights-monitor-web-app-availability/19-availEdit-3.png)

Amikor karbantartást végez a szolgáltatáson, célszerű kikapcsolni a rendelkezésre állási teszteket és az azokhoz kapcsolódó riasztási szabályokat. 

## <a name="failures"></a>Ha hibákat lát
Kattintson egy piros pontra.

![Kattintson egy piros pontra](./media/app-insights-monitor-web-app-availability/open-instance-3.png)


A rendelkezésre állási tesztek eredményei alapján a következőket teheti:

* Megvizsgálhatja a kiszolgálótól érkezett választ.
* Megnyithatja a kiszolgálóalkalmazás által a sikertelen kéréspéldány feldolgozása közben küldött telemetriát.
* Naplózhat egy problémát vagy munkaelemet a Git vagy a VSTS segítségével a probléma nyomon követéséhez. A hiba tartalmazni fog egy hivatkozást erre az eseményre.
* Megnyithatja a webes teszt eredményét a Visual Studióban.


*Úgy tűnik, hogy rendben van, mégis sikertelenként lett jelentve?* Ellenőrizze az összes képet, szkriptet, stíluslapot és a lap által betöltött többi fájlt. Ha ezek közül bármelyik hibás, a teszt sikertelenként lesz jelentve, még akkor is, ha a fő HTML-oldal megfelelően töltődik be.

*Nem találhatók kapcsolódó elemek?* Ha a kiszolgálóoldali alkalmazásához be van állítva az Application Insights, akkor ezt okozhatja az, hogy [mintavételezés](app-insights-sampling.md) van folyamatban. 

## <a name="multi-step-web-tests"></a>Többlépéses webes tesztek
Olyan forgatókönyveket is figyelhet, amelyek egy URL-címek sorozatából állnak. Ha például egy értékesítési webhelyet figyel, tesztelheti, hogy megfelelően működik-e a termékek kosárba helyezése.

> [!NOTE] 
> A többlépéses webes tesztek díjkötelesek. [Díjszabási séma](http://azure.microsoft.com/pricing/details/application-insights/)
> 

Többlépéses teszt létrehozásához rögzíteni kell a forgatókönyvet a Visual Studio Enterprise segítségével, majd fel kell tölteni a felvételt az Application Insights-ba. Az Application Insights időközönként visszajátssza a forgatókönyvet, és ellenőrzi a válaszokat.

> [!NOTE]
> A tesztekben nem használhat kódolt függvényeket vagy hurkokat. A .webtest szkriptnek teljes egészében tartalmaznia kell a tesztet. Ehelyett azonban standard beépülő modulok is használhatók.
>

#### <a name="1-record-a-scenario"></a>1. Forgatókönyv rögzítése
A webes munkamenet rögzítéséhez használja a Visual Studio Enterprise-t.

1. Hozzon létre egy webes teljesítményt tesztelő projektet.

    ![A Visual Studio Enterprise kiadásban hozzon létre egy projektet a webes teljesítmény és a terheléstesztelés sablonjából.](./media/app-insights-monitor-web-app-availability/appinsights-71webtest-multi-vs-create.png)

 * *Nem jelenik meg a webes teljesítmény- és terheléstesztelés sablonja?* – Zárja be a Visual Studio Enterprise-t. Nyissa meg a **Visual Studio telepítőjét** a Visual Studio Enterprise-telepítés módosításához. Az **Individual Components** (Egyedi összetevők) területen válassza a **Web Performance and load testing tools** (Webes teljesítmény- és terheléstesztelési eszközök) lehetőséget.

2. Nyissa meg a .webtest fájlt, és kezdje meg a rögzítést.

    ![Nyissa meg a .webtest fájlt, és kattintson a Record (Rögzítés) gombra.](./media/app-insights-monitor-web-app-availability/appinsights-71webtest-multi-vs-start.png)
3. Végezze el a felhasználói műveleteket, amelyeket a teszt során szimulálni kíván: nyissa meg a webhelyet, tegyen a kosárba egy terméket stb. Ezután állítsa le a tesztet.

    ![A webes teszt rögzítője az Internet Explorerben fut.](./media/app-insights-monitor-web-app-availability/appinsights-71webtest-multi-vs-record.png)

    A forgatókönyv ne legyen hosszú. Legfeljebb 100 lépésből állhat, és 2 percig tarthat.
4. Szerkessze a tesztet:

   * Adjon hozzá ellenőrzéseket a kapott szöveg- és válaszkódok ellenőrzéséhez.
   * Távolítson el minden felesleges interakciót. A képekhez, illetve a hirdető vagy nyomkövetési webhelyekhez tartozó függő kérelmeket is el lehet távolítani.

     Ne feledje, hogy csak a teszt szkriptjét szerkesztheti, tehát nem adhat hozzá egyéni kódot, és nem hívhat meg más webes teszteket. Ne szúrjon be hurkokat a tesztbe. Használhatja a normál webes teszt beépülő modulokat.
5. A működés ellenőrzéséhez futtassa a tesztet a Visual Studióban.

    A webes teszt futtatója megnyit egy webböngészőt, és megismétli a rögzített műveleteket. Győződjön meg arról, hogy a várakozásainak megfelelően működik.

    ![A Visual Studióban nyissa meg a .webtest fájlt, majd kattintson a Run (Futtatás) gombra.](./media/app-insights-monitor-web-app-availability/appinsights-71webtest-multi-vs-run.png)

#### <a name="2-upload-the-web-test-to-application-insights"></a>2. A webes teszt feltöltése az Application Insightsba
1. Az Application Insights portálon hozzon létre egy webes tesztet.

    ![A webes teszt panelen válassza a Hozzáadás elemet.](./media/app-insights-monitor-web-app-availability/16-another-test.png)
2. Válassza ki a többlépéses tesztet, majd töltse fel a .webtest fájlt.

    ![Válassza ki a többlépéses webtesztet.](./media/app-insights-monitor-web-app-availability/appinsights-71webtestUpload.png)

    Válassza ki a tesztelési helyeket, a tesztelés gyakoriságát és riasztási paramétereit ugyanúgy, mint a ping teszteknél.

#### <a name="3-see-the-results"></a>3. Az eredmények megtekintése

Tekintse meg a teszt eredményeit és a hibákat ugyanúgy, mint az egyetlen URL-címre kiterjedő teszteknél.

Emellett le is töltheti és megtekintheti a teszteredményeket a Visual Studióban.

#### <a name="too-many-failures"></a>Túl sok a hiba?

* A hibák gyakori oka, hogy a teszt túl hosszú ideig fut. A teszt nem tarthat tovább két percnél.

* Ne feledje, hogy az oldal összes erőforrásának megfelelően kell betöltődnie ahhoz, hogy a teszt sikeres legyen, beleértve a szkripteket, stíluslapokat, képeket stb.

* A .webtest szkriptnek a teljes webes tesztet tartalmaznia kell: a tesztben nem használhat kódolt függvényeket.

### <a name="plugging-time-and-random-numbers-into-your-multi-step-test"></a>Idő és véletlenszerű számok beiktatása a többlépéses tesztbe
Tegyük fel, hogy egy olyan eszközt tesztel, amely időfüggő adatokat fogad, például részvényeket egy külső adatcsatornától. A webes teszt rögzítésekor meghatározott időpontokat kell használni, de ezeket a teszt StartTime és EndTime paramétereiként kell beállítani.

![Egy webes teszt paraméterekkel.](./media/app-insights-monitor-web-app-availability/appinsights-72webtest-parameters.png)

A teszt futtatásakor az EndTime paraméternek mindig az aktuális időnek kell lennie, a StartTime paraméternek pedig a 15 perccel korábbi időnek.

A webes teszt beépülő modulok lehetővé teszik az idő paraméterezését.

1. Adjon hozzá egy webes teszt beépülő modult minden kívánt változóparaméter-értékhez. A webes teszt eszköztárában válassza ki a **Webes teszt beépülő modul hozzáadása** elemet.

    ![Válassza ki a Webes teszt beépülő modul hozzáadása elemet, majd válasszon egy típust.](./media/app-insights-monitor-web-app-availability/appinsights-72webtest-plugins.png)

    Ebben a példában a Dátum és idő beépülő modul két példányát használjuk. Az egyik példány a „15 perccel ezelőtt” a másik pedig a „most” paraméterértékhez tartozik.
2. Nyissa meg a beépülő modulok tulajdonságait. Adjon meg egy nevet, és állítsa be úgy, hogy az aktuális időt használja. Az egyiknél állítsa be a következőt: Add Minutes = -15.

    ![Beállított Name, Use Current Time, és Add Minutes.](./media/app-insights-monitor-web-app-availability/appinsights-72webtest-plugin-parameters.png)
3. A webes teszt paramétereiben a beépülő modulok nevére a {{plug-in name}} segítségével hivatkozzon.

    ![A tesztparaméterében használja a {{plug-in name}} elemet.](./media/app-insights-monitor-web-app-availability/appinsights-72webtest-plugin-name.png)

Ezután töltse fel a tesztet a portálra. Ez a teszt minden futtatásánál a dinamikus értékeket használja.

## <a name="dealing-with-sign-in"></a>A bejelentkezés kezelése
Ha a felhasználók bejelentkeznek az alkalmazásába, számos lehetősége van a bejelentkezés szimulálására a bejelentkezés után megjelenő lapok teszteléséhez. A használt megközelítés az alkalmazás által kínált biztonság típusától függ.

Minden esetben ajánlott létrehozni egy fiókot az alkalmazásában tesztelési célra. Ha lehetséges, korlátozza az engedélyt ezen a tesztfiókon, így elkerülheti, hogy a webes tesztek hatással legyenek a tényleges felhasználókra.

### <a name="simple-username-and-password"></a>Egyszerű felhasználónév és jelszó
Rögzítsen egy webes tesztet a szokásos módon. Először törölje a cookie-kat.

### <a name="saml-authentication"></a>SAML-hitelesítés
Használhatja a webes tesztek számára elérhető SAML beépülő modult.

### <a name="client-secret"></a>Titkos ügyfélkulcs
Ha az alkalmazás bejelentkezési módja titkos ügyfélkódot igényel, használja azt a módot. Az Azure Active Directory (AAD) mint egy példa olyan szolgáltatásra, amely titkos ügyfélkulccsal történő bejelentkezést biztosít. Az AAD-ben a titkos ügyfélkulcs az alkalmazáskulcs.

Itt egy alkalmazáskulcsot használó Azure-webalkalmazás webes tesztelésre találhat példát:

![Titkos ügyfélkulcs-minta](./media/app-insights-monitor-web-app-availability/110.png)

1. Szerezze be a tokent az AAD-ből a titkos ügyfélkulcs használatával (AppKey).
2. Nyerje ki a tulajdonosi jogkivonatot a válaszból.
3. Az engedélyezési fejléc tulajdonosi jogkivonatával hívja meg az API-t.

Győződjön meg róla, hogy a webes teszt egy tényleges ügyfél – saját alkalmazása van az AAD-ben –, valamint használja a clientId + appkey értékét. A teszt alatt álló szolgáltatása is rendelkezik saját alkalmazással az AAD-ben: az alkalmazás appID URI-ja a webes teszt „resource” mezőjében látható.

### <a name="open-authentication"></a>Nyílt hitelesítés
Nyílt hitelesítés például a Microsoft- vagy Google-fiókkal történő bejelentkezés. Számos OAuth protokollt használó alkalmazás biztosítja a titkos ügyfélkódos alternatívát, így az első feladat ennek a lehetőségnek a megvizsgálása.

Ha a tesztnek az OAuth protokollal kell bejelentkeznie, az általános megközelítés a következő:

* Használjon egy eszközt, például a Fiddlert a webböngésző, a hitelesítési hely és az alkalmazás közötti forgalom vizsgálatához.
* Jelentkezzen be legalább két alkalommal különböző gépek vagy böngészők használatával, illetve hosszú időközönként (hogy a tokennek legyen ideje lejárni).
* A különböző munkamenetek összehasonlításával azonosítsa a hitelesítési helyről visszaadott tokent, amelyet a rendszer a bejelentkezést követően továbbad az alkalmazáskiszolgálónak.
* Rögzítsen egy webes tesztet a Visual Studióval.
* Paraméterezze a tokeneket. Ehhez állítsa be a paramétereket, amikor a hitelesítő visszaküldi a tokent, és használja a webhely felé indított lekérdezésben.
  (A Visual Studio megpróbálja paraméterezni a tesztet, de a tokeneket nem paraméterezi megfelelően.)


## <a name="performance-tests"></a>Teljesítménytesztek
Terheléstesztelést futtathat a webhelyén. Csakúgy, mint a rendelkezésre állási teszt esetében, egyszerű vagy több lépésből álló kéréseket küldhet a világ minden táján található helyekről. A rendelkezésre állási teszttől eltérően sok kérés lesz elküldve, több párhuzamos felhasználót szimulálva.

Az Áttekintés panelről nyissa meg a **Beállítások**, **Teljesítménytesztek** elemet. Teszt létrehozásakor egy kérést kap arra, hogy csatlakozzon a Visual Studio Team Services-fiókhoz, vagy hozzon létre egyet.

A teszt befejezése után a válaszidők és a sikerességi arány jelenik meg.


![Teljesítményteszt](./media/app-insights-monitor-web-app-availability/perf-test.png)

> [!TIP]
> A teljesítményteszt hatásainak megfigyelésére használja az [Élő streamet](app-insights-live-stream.md) és a [Profilkészítőt](app-insights-profiler.md).
>

## <a name="automation"></a>Automatizálás
* [Használjon PowerShell-szkripteket a rendelkezésre állási teszt automatikus beállításához](app-insights-powershell.md#add-an-availability-test).
* Állítson be egy [webhookot](../monitoring-and-diagnostics/insights-webhooks-alerts.md), amelyet a rendszer riasztás esetén hív meg.

## <a name="qna"></a>Kérdése van? Problémákat tapasztal?
* *Meghívhatok egy kódot a webes tesztből?*

    Nem. A .webtest fájlnak tartalmaznia kell a teszt lépéseit. Nem hívhat meg más teszteket, és nem használhat hurkokat. Azonban számos beépülő modul van, amely hasznos lehet.
* *Támogatott a HTTPS?*

    A TLS 1.1 és a TLS 1.2 támogatott.
* *Van különbség a „webes tesztek” és a „rendelkezésre állási tesztek” között?*

    A két kifejezés hasonló értelmű, felcserélhető. A „rendelkezésre állási teszt” általánosabb fogalom, amely az URL-ping tesztek mellett a többlépéses webes teszteket is magában foglalja.
* *Rendelkezésre állási teszteket szeretnék használni a tűzfal mögött futó belső kiszolgálón.*

    Két lehetséges megoldás létezik:
    
    * Konfigurálhatja úgy a tűzfalat, hogy az engedélyezze a [webes tesztügynökök IP-címeiről](app-insights-ip-addresses.md) érkező bejövő kéréseket.
    * Saját kód megírásával rendszeresen ellenőrizheti a belső kiszolgálót. Futtassa a kódot a tűzfal mögötti tesztkiszolgáló háttérfolyamataként. A tesztelési folyamat az eredményeket a Core SDK-csomag [TrackAvailability()](https://docs.microsoft.com/dotnet/api/microsoft.applicationinsights.telemetryclient.trackavailability) API-jával küldheti el az Application Insightsba. Ehhez szükség van arra, hogy a tesztkiszolgáló kimenő hozzáféréssel rendelkezzen az Application Insights betöltési végpontjához, de ez jóval kisebb biztonsági kockázatot jelent a bejövő kérések engedélyezéséhez képest. Az eredmények nem jelennek meg a rendelkezésre állási webes tesztek paneljein, de rendelkezésre állási eredményként megtekinthetők az Elemzés, a Keresés és a Metrikaböngésző panelen.
* *A többlépéses teszt feltöltése sikertelen*

    A méretkorlát: 300 KB.

    A hurkok nem támogatottak.

    A más webes tesztekre mutató hivatkozások nem támogatottak.

    Az adatforrások nem támogatottak.
* *A többlépéses teszt nem fejeződik be*

    Egy teszt legfeljebb 100 kérelemből állhat.

    A teszt leáll, ha két percnél tovább fut.
* *Hogyan futtathatok tesztet ügyféltanúsítványokkal?*

    Sajnos azt nem támogatjuk.


## <a name="next"></a>Következő lépések
[Diagnosztikai naplók keresése][diagnostic]

[Hibaelhárítás][qna]

[A webes tesztügynökök IP-címei](app-insights-ip-addresses.md)

<!--Link references-->

[azure-availability]: ../insights-create-web-tests.md
[diagnostic]: app-insights-diagnostic-search.md
[qna]: app-insights-troubleshoot-faq.md
[start]: app-insights-overview.md
