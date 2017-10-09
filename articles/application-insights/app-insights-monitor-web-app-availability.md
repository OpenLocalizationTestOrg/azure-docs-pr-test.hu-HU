---
title: "aaaMonitor rendelkezésre állási és reakcióidőt bármely webhely |} Microsoft Docs"
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
ms.openlocfilehash: 4c5425c948770cc57a648ca50e217c75ac75fbd7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-availability-and-responsiveness-of-any-web-site"></a>Webhelyek rendelkezésre állásának és válaszkészségének megfigyelése
A webes alkalmazás vagy webhely tooany kiszolgáló telepítése után, miután állíthat be tesztek toomonitor elérhetőségét és a válaszképességét. [Az Azure Application Insights](app-insights-overview.md) webes kérelmeket küld tooyour alkalmazás rendszeres időközönként hello világ pontokból. Riasztást jelenít meg, ha az alkalmazás nem válaszol, vagy lassan válaszol.

Bármely HTTP rendelkezésreállás figyelésére szolgáló tesztek állíthat be, vagy a HTTPS-végpont elérhető-e a nyilvános interneten hello. Nincs tooadd semmi tesztelést toohello webhelyen. Még nincs toobe a webhely: sikerült tesztelni, amelyen függő REST API-szolgáltatás.

A rendelkezésre állási teszteknek két típusuk van:

* [URL-cím ellenőrzés](#create): egy egyszerű tesztet, amely a hello Azure-portálon hozhatja létre.
* [Több webalkalmazás-teszt](#multi-step-web-tests): amely Visual Studio Enterprise és a feltöltés toohello portal létrehozni.

Létrehozhat, too25 rendelkezésreállás figyelésére szolgáló tesztek egyes alkalmazás-erőforrás mentése.

## <a name="create"></a>1. Erőforrás megnyitása a rendelkezésre állási tesztek jelentéseihez

**Ha már beállította az Application Insights** a webalkalmazás, nyissa meg az Application Insights-erőforrás hello [Azure-portálon](https://portal.azure.com).

**Vagy, ha azt szeretné, toosee egy új erőforrást, a jelentések** túl regisztráljon[Microsoft Azure](http://azure.com), toohello nyissa [Azure-portálon](https://portal.azure.com), és hozzon létre egy Application Insights-erőforrást.

![Új > Application Insights](./media/app-insights-monitor-web-app-availability/11-new-app.png)

Kattintson a **összes erőforrás** tooopen hello áttekintése panel hello új erőforrás.

## <a name="setup"></a>2. URL-ping teszt létrehozása
Hello rendelkezésre állási panel megnyitásához, és adja hozzá a vizsgálatot.

![Fill legalább hello a webhely URL-címe](./media/app-insights-monitor-web-app-availability/13-availability.png)

* **hello URL-cím** lehet bármilyen weblap tootest kívánt, de kell lennie a nyilvános internethez hello. hello URL-címet tartalmazhat lekérdezési karakterláncot. Tehát például kísérletezhet egy kicsit az adatbázissal. Hello URL-cím tooa átirányítási oldja fel, ha azt az nyomon követéséhez too10 átirányítások.
* **Függő kérések elemezni**: Ha ez a beállítás be van jelölve, hello teszt lemezképek, parancsprogramok, stílus fájlok és egyéb fájlok hello weblap vizsgálandó részét képező kéri-e. hello rögzített válaszidő tartalmaz hello igénybe vett idő tooget ezeket a fájlokat. hello teszt sikertelen, ha ezeket az erőforrásokat sikeresen le nem hello teljes vizsgálat hello időkorláton belül. 

    Ha hello beállítás nincs bejelölve, a hello teszt csak kérelmek hello fájl a megadott hello URL-címen.
* **Újrapróbálkozások engedélyezése**: Ez a beállítás be van jelölve, amikor hello teszt sikertelen, ha egy rövid időszak után újból. Csak akkor jelent hibát, ha három egymást követő kísérlet meghiúsul. További vizsgálatok hello szokásos teszt gyakorisággal követően elvégezni. Újrapróbálkozási ideiglenesen fel van függesztve, amíg hello következő sikeres. Ez a szabály függetlenül van alkalmazva minden egyes teszthelyen. Ezt a beállítást javasoljuk. Átlagosan körülbelül a hibák 80%-a megszűnik az újrapróbálkozáskor.
* **Tesztelési gyakoriság**: Beállítja, hogy milyen gyakran hello teszt van helyről futtatja minden teszt. Öt perces gyakorisággal és öt teszthellyel a helyén átlagosan percenként egy teszt történik.
* **Tesztelje a helyek** , ahol a kiszolgálóink küldése webes kérelmek tooyour URL-címe helyezi hello. Többet válasszon, hogy megkülönböztethesse webhelye problémáit a hálózati hibáktól. Kiválaszthatja, too16 helyek fel.
* **Sikeresség feltétele**:

    **Teszt időtúllépése**: a lassú válaszok figyelmeztetést érték toobe csökkentése. hello teszt hiba számít, ha ez idő alatt nem fogadott hello válaszokat az Ön webhelyéről. Ha a kiválasztott **függő kérelmek elemezni**, majd az összes hello képek, stílus fájlok, parancsfájlok és más függő erőforrások kell fogadott ez idő alatt.

    **HTTP-válasz**: hello állapotkódot, amely sikeres számít. 200 a hello kódot, amely jelzi, hogy a normál weblap hibát adott vissza.

    **Tartalmi egyezés**: egy karakterlánc, például „Üdvözöljük!” Teszteljük, hogy minden válaszban előfordul-e a kis- és nagybetűket figyelembe véve is pontos egyezés. Egyszerű karakterláncnak kell lennie helyettesítő karakterek nélkül. Ne feledje, hogy ha a lap a tartalmi változások tooupdate lehetséges, hogy azt.
* **Riasztások** , alapértelmezés szerint küldött tooyou Ha nincsenek hibák három helyen több mint öt perc. Egy helyen hibát valószínűleg toobe hálózati hiba, de nem webhelyét problémáját. Hello küszöbérték toobe további módosíthatja, de érzékeny, és kevésbé is módosítása, akik e-mailek hello kell lehet küldeni.

    Be lehet állítani egy [webhookot](../monitoring-and-diagnostics/insights-webhooks-alerts.md), amelyet a rendszer egy riasztás megjelenésekor hív meg. (Vegye figyelembe, hogy jelenleg a lekérdezési paraméterek nem továbbítódnak Tulajdonságokként.)

### <a name="test-more-urls"></a>További URL-címek tesztelése
Adjon hozzá további teszteket. Például a hozzáadása tootesting a kezdőlap biztosíthatja, az adatbázis fut hello URL-cím keresés környezetben végzett teszteléséhez.


## <a name="monitor"></a>3. A rendelkezésre állási teszt eredményeinek megtekintése

Néhány perc múlva kattintson **frissítése** toosee teszteredmények. 

![Hello otthoni panel összefoglaló eredmények](./media/app-insights-monitor-web-app-availability/14-availSummary-3.png)

hello scatterplot hello mintáit rajtuk diagnosztikai teszt-lépés részletes rendelkező vizsgálati eredményeket jeleníti meg. hello teszt motor hibákkal rendelkező tesztek a diagnosztikai részletek tárolja. A sikeres vizsgálat diagnosztikai részletek tárolják hello végrehajtások egy része számára. Bármelyik hello zöld/piros pont toosee hello teszt timestamp, a vizsgálat időtartama, a hely és a teszt neve mutasson. Kattintson a bármely ponttal a hello teszteredménye hello hello pont rajzot toosee részletes adatait.  

Egy adott vizsgálat, a hely, válasszon, vagy csökkentse a period toosee további eredmények körül hello érdeklő időszak hello idő. Keresési ablak toosee eredményeinek összes végrehajtások használja, vagy elemzési lekérdezések toorun egyéni jelentések ezeken az adatokon.

Továbbá toohello nyers eredmények van két rendelkezésre állási metrikák a Metrikaböngészőben: 

1. Rendelkezésre állás: Százalékos hello tesztet hajt végre, hogy sikerült-e, az összes teszt végrehajtások között. 
2. Tesztek időtartama: A tesztek átlagos időtartama az összes végrehajtás alapján.

Szűrőket hello teszt neve, a hely tooanalyze trendek a vizsgált és/vagy a hely.

## <a name="edit"></a> Tesztek megtekintése és szerkesztése

Hello összefoglalás lapon jelölje ki, egy adott tesztet. Itt megtekintheti a teszt eredményeit, és szerkesztheti vagy átmenetileg letilthatja a tesztet.

![Webes teszt szerkesztése vagy letiltása](./media/app-insights-monitor-web-app-availability/19-availEdit-3.png)

Érdemes toodisable rendelkezésreállás figyelésére szolgáló tesztek vagy a hello riasztási szabályok a hozzájuk kapcsolódó, amíg a szolgáltatás karbantartási hajtja végre. 

## <a name="failures"></a>Ha hibákat lát
Kattintson egy piros pontra.

![Kattintson egy piros pontra](./media/app-insights-monitor-web-app-availability/open-instance-3.png)


A rendelkezésre állási tesztek eredményei alapján a következőket teheti:

* Vizsgálja meg a hello választ kapott a kiszolgálótól.
* Nyissa meg a hello sikertelen kérelmek példány feldolgozása közben a kiszolgáló alkalmazás által küldött hello telemetriai adatokat.
* Probléma bejelentkezhet, illetve a Git vagy VSTS tootrack hello probléma munkaelem. hello hiba a hivatkozás toothis esemény fogja tartalmazni.
* Nyissa meg a Visual Studio hello webes tesztelési eredménye.


*Úgy tűnik, hogy rendben van, mégis sikertelenként lett jelentve?* Ellenőrizze az összes hello lemezképek, parancsprogramok, stíluslapok és hello lap által betöltött egyéb fájlok. Ha valamelyiket sikertelen, hello teszt van jelentett sikertelen volt, akkor is, ha hello fő html-weblap betölti az OK gombra.

*Nem találhatók kapcsolódó elemek?* Ha a kiszolgálóoldali alkalmazásához be van állítva az Application Insights, akkor ezt okozhatja az, hogy [mintavételezés](app-insights-sampling.md) van folyamatban. 

## <a name="multi-step-web-tests"></a>Többlépéses webes tesztek
Olyan forgatókönyveket is figyelhet, amelyek egy URL-címek sorozatából állnak. Például ha egy értékesítési webhely figyeli, tesztelheti, hogy elemek toohello hozzáadása kosár megfelelően működik-e.

> [!NOTE] 
> A többlépéses webes tesztek díjkötelesek. [Díjszabási séma](http://azure.microsoft.com/pricing/details/application-insights/)
> 

toocreate többlépéses teszt, akkor hello forgatókönyv jegyezze fel a Visual Studio Enterprise, majd töltse fel hello tooApplication Insights rögzítése. Az Application Insights felhasználásait hello forgatókönyv időközönként és hello válaszok ellenőrzi.

> [!NOTE]
> A tesztekben nem használhat kódolt függvényeket vagy hurkokat. hello tesztelése szerepelnie kell teljesen hello .webtest parancsfájl. Ehelyett azonban standard beépülő modulok is használhatók.
>

#### <a name="1-record-a-scenario"></a>1. Forgatókönyv rögzítése
Használja a Visual Studio Enterprise toorecord webes munkamenet.

1. Hozzon létre egy webes teljesítményt tesztelő projektet.

    ![A Visual Studio Enterprise edition a projekt létrehozása hello webalkalmazás teljesítmény- és terheléstesztet sablonból.](./media/app-insights-monitor-web-app-availability/appinsights-71webtest-multi-vs-create.png)

 * *Hello teljesítményét és terheléstesztet sablon nem jelenik meg?* – Zárja be a Visual Studio Enterprise-t. Nyissa meg **Visual Studio telepítő** toomodify a Visual Studio Enterprise telepítése. Az **Individual Components** (Egyedi összetevők) területen válassza a **Web Performance and load testing tools** (Webes teljesítmény- és terheléstesztelési eszközök) lehetőséget.

2. Nyissa meg hello .webtest fájlt, és indítsa el a rögzítése.

    ![Nyissa meg a hello .webtest fájlt, és kattintson a rekordot.](./media/app-insights-monitor-web-app-availability/appinsights-71webtest-multi-vs-start.png)
3. A teszt toosimulate kívánt felhasználói műveletek hello: Nyissa meg a webhely, vegye fel a termék toohello kosárhoz, és így tovább. Ezután állítsa le a tesztet.

    ![hello webteszt író fut az Internet Explorerben.](./media/app-insights-monitor-web-app-availability/appinsights-71webtest-multi-vs-record.png)

    A forgatókönyv ne legyen hosszú. Legfeljebb 100 lépésből állhat, és 2 percig tarthat.
4. A vizsgálat hello szerkesztése:

   * Adja hozzá a toocheck kapott hello text és response kódok érvényesítést.
   * Távolítson el minden felesleges interakciót. Függő kérések a képek vagy tooad vagy helyek követési is eltávolítható.

     Ne feledje, hogy csak akkor szerkeszthető hello teszt parancsfájl - nem adja hozzá az egyéni kód, és nem hívható meg más webes tesztjeinek használatát. Hurkok nem beszúrása hello tesztben. Használhatja a normál webes teszt beépülő modulokat.
5. Hello teszt futtatása a Visual Studio toomake meg arról, hogy működik.

    hello webes tesztfuttató egy webböngészőben megnyílik, és ismétlődések hello rögzített műveletek. Győződjön meg arról, hogy a várakozásainak megfelelően működik.

    ![A Visual Studióban nyissa meg hello .webtest fájlt, és kattintson a Futtatás gombra.](./media/app-insights-monitor-web-app-availability/appinsights-71webtest-multi-vs-run.png)

#### <a name="2-upload-hello-web-test-tooapplication-insights"></a>2. Hello webes teszt tooApplication Insights feltöltése
1. Hello Application Insights portáljáról hozzon létre egy webtesztet.

    ![Hello webalkalmazás-tesztek paneljén a Hozzáadás gombra.](./media/app-insights-monitor-web-app-availability/16-another-test.png)
2. Válassza ki a többlépéses tesztelése, és hello .webtest fájl feltöltése.

    ![Válassza ki a többlépéses webtesztet.](./media/app-insights-monitor-web-app-availability/appinsights-71webtestUpload.png)

    Set hello teszt helyek, a gyakoriság és a riasztási paraméterekkel a hello azonos módon, a ping teszteli.

#### <a name="3-see-hello-results"></a>3. Hello eredmények megtekintése

Megtekintheti a vizsgálati eredmények és az esetleges hibákat hello azonos módon, egyetlen-url tesztelések.

Emellett letöltheti a hello vizsgálati eredmények tooview őket a Visual Studióban.

#### <a name="too-many-failures"></a>Túl sok a hiba?

* Egy általános hiba oka, hogy hello tesztre túl hosszú. A teszt nem tarthat tovább két percnél.

* Ne feledje, hogy az oldal összes hello erőforrásokat kell töltődik be megfelelően az hello teszt toosucceed, beleértve a parancsfájlok, stíluslapok, lemezképek és így tovább.

* hello webteszt teljesen tartalmaznia kell hello .webtest parancsfájl: hello teszt kódolt függvények nem használhatók.

### <a name="plugging-time-and-random-numbers-into-your-multi-step-test"></a>Idő és véletlenszerű számok beiktatása a többlépéses tesztbe
Tegyük fel, hogy egy olyan eszközt tesztel, amely időfüggő adatokat fogad, például részvényeket egy külső adatcsatornától. A webalkalmazás-teszt rögzítésekor toouse meghatározott alkalommal van, de meg őket, hello paraméterei teszteléséhez StartTime és az EndTime megadása.

![Egy webes teszt paraméterekkel.](./media/app-insights-monitor-web-app-availability/appinsights-72webtest-parameters.png)

Hello teszt futtatása, azt szeretné, hogy mindig toobe hello jelen idő EndTime, és a StartTime kell 15 perc telt el.

Webes teszt beépülő modulok lehetőséget nyújtanak hello toodo parametrizálja alkalommal.

1. Adjon hozzá egy webes teszt beépülő modult minden kívánt változóparaméter-értékhez. Hello web teszt eszköztáron válassza **hozzáadása webes tesztelése beépülő modul**.

    ![Válassza ki a Webes teszt beépülő modul hozzáadása elemet, majd válasszon egy típust.](./media/app-insights-monitor-web-app-availability/appinsights-72webtest-plugins.png)

    A jelen példában használjuk hello dátum idő beépülő modul két példánya. Az egyik példány a „15 perccel ezelőtt” a másik pedig a „most” paraméterértékhez tartozik.
2. Minden egyes beépülő modul hello tulajdonságai párbeszédpanel megnyitásához. Adjon neki egy nevet, majd állítsa be toouse hello aktuális idő. Az egyiknél állítsa be a következőt: Add Minutes = -15.

    ![Beállított Name, Use Current Time, és Add Minutes.](./media/app-insights-monitor-web-app-availability/appinsights-72webtest-plugin-parameters.png)
3. A hello webes paraméterei használja az {{a beépülő modul neve}} tooreference a beépülő modul nevét.

    ![A hello paraméter használja az {{a beépülő modul neve}}.](./media/app-insights-monitor-web-app-availability/appinsights-72webtest-plugin-name.png)

Most töltse fel a vizsgált toohello portál. Minden futtatáskor hello vizsgálat hello dinamikus értékeket használ.

## <a name="dealing-with-sign-in"></a>A bejelentkezés kezelése
Ha a felhasználói bejelentkezés tooyour app, lehetősége van különböző bejelentkezési megtekintheti, hogy a lapok mögött hello bejelentkezés tesztelheti. hello megközelítést használ hello alkalmazás által biztosított biztonsági hello típusától függ.

Minden esetben fiókot kell létrehozni az csak az alkalmazás tesztelése hello célját. Ha lehetséges korlátozza a hello engedélyeit teszt fiókhoz tartozó, így nincs lehetősége, hogy valós felhasználókat érintő hello webteszt.

### <a name="simple-username-and-password"></a>Egyszerű felhasználónév és jelszó
Rögzítse a webalkalmazás-teszt hello a szokásos módon. Először törölje a cookie-kat.

### <a name="saml-authentication"></a>SAML-hitelesítés
Hello SAML-alapú beépülő modul, amely elérhető a web Tests használja.

### <a name="client-secret"></a>Titkos ügyfélkulcs
Ha az alkalmazás bejelentkezési módja titkos ügyfélkódot igényel, használja azt a módot. Az Azure Active Directory (AAD) mint egy példa olyan szolgáltatásra, amely titkos ügyfélkulccsal történő bejelentkezést biztosít. Az aad-ben hello ügyfélkulcs hello Alkalmazáskulcs.

Itt egy alkalmazáskulcsot használó Azure-webalkalmazás webes tesztelésre találhat példát:

![Titkos ügyfélkulcs-minta](./media/app-insights-monitor-web-app-availability/110.png)

1. Szerezze be a tokent az AAD-ből a titkos ügyfélkulcs használatával (AppKey).
2. Nyerje ki a tulajdonosi jogkivonatot a válaszból.
3. Tulajdonosi jogkivonattal használatát hello authorization fejlécet API hívása.

Győződjön meg arról, hogy hello webteszt az ügyfél tényleges – Ez azt jelenti, hogy rendelkezik a saját alkalmazás AAD -, és használja a clientId + appkey. A szolgáltatás vizsgálandó is van a saját alkalmazás az aad-ben: hello alkalmazásazonosító URI-címe, az alkalmazás hello webteszt hello "erőforrás" mezőben is megjelenik.

### <a name="open-authentication"></a>Nyílt hitelesítés
Nyílt hitelesítés például a Microsoft- vagy Google-fiókkal történő bejelentkezés. Számos alkalmazás, hogy OAuth nyújtanak hello ügyfél titkos alternatív, ezért az első tactic tooinvestigate kell lennie. ezt a lehetőséget.

Ha a teszt be kell jelentkeznie a OAuth protokollt használó, hello általános módszer áll.

* Használjon olyan eszköz, például a Fiddler tooexamine hello forgalom a webböngészőt, hello hitelesítési hely és az alkalmazás között.
* Hajtsa végre a különböző gépek vagy a böngésző, két vagy több bejelentkezéseket vagy hosszú időközönként (tooallow jogkivonatok tooexpire).
* Különböző munkamenetekben összehasonlításával azonosítsa hello hitelesítéséhez a helyet, majd átadott tooyour alkalmazáskiszolgálóval után bejelentkezhet újból az átadott hello token.
* Rögzítsen egy webes tesztet a Visual Studióval.
* Hello jogkivonatokat, ha hello paramétert, ha hello jogkivonatot adott vissza a hello hitelesítő, és használattal hello lekérdezés toohello hely paraméterezni.
  (A visual Studio kísérleteket tooparameterize hello teszt, de megfelelően parametrizálja hello jogkivonatokat.)


## <a name="performance-tests"></a>Teljesítménytesztek
Terheléstesztelést futtathat a webhelyén. Hello elérhetőségi teszt, például küldhet egyszerű kérések vagy a többlépéses kérelmek hello világ a pontokból. A rendelkezésre állási teszttől eltérően sok kérés lesz elküldve, több párhuzamos felhasználót szimulálva.

Hello áttekintése panelen, nyissa meg a **beállítások**, **teljesítménytesztek**. Amikor létrehoz egy tesztelési, felkérést tooconnect tooor Visual Studio Team Services-fiók létrehozása.

Hello teszt befejeződése után jelennek meg, hogy válaszidejét és sikerességi.


![Teljesítményteszt](./media/app-insights-monitor-web-app-availability/perf-test.png)

> [!TIP]
> tooobserve hello hatásait teljesítményének teszteléséhez használja [élő adatfolyam](app-insights-live-stream.md) és [Profilkészítő](app-insights-profiler.md).
>

## <a name="automation"></a>Automatizálás
* [Használja a PowerShell parancsfájlok tooset fel egy elérhetőségi teszt](app-insights-powershell.md#add-an-availability-test) automatikusan.
* Állítson be egy [webhookot](../monitoring-and-diagnostics/insights-webhooks-alerts.md), amelyet a rendszer riasztás esetén hív meg.

## <a name="qna"></a>Kérdése van? Problémákat tapasztal?
* *Meghívhatok egy kódot a webes tesztből?*

    Nem. hello lépéseket hello vizsgálat hello .webtest fájl kell lennie. Nem hívhat meg más teszteket, és nem használhat hurkokat. Azonban számos beépülő modul van, amely hasznos lehet.
* *Támogatott a HTTPS?*

    A TLS 1.1 és a TLS 1.2 támogatott.
* *Van különbség a „webes tesztek” és a „rendelkezésre állási tesztek” között?*

    hello két feltételek készletekre lehet hivatkozni gombokra. Rendelkezésreállás figyelésére szolgáló tesztek egy több általános kifejezés, amely tartalmazza az hello egyetlen URL-cím ping teszteli, továbbá toohello többlépéses webes tesztjeinek használatát.
* *Szeretném toouse rendelkezésreállás figyelésére szolgáló tesztek a tűzfal mögött futó belső kiszolgálón.*

    Két lehetséges megoldás létezik:
    
    * Konfigurálja a tűzfal toopermit érkező kéréseket hello [IP-címek internetes tesztelése ügynökök](app-insights-ip-addresses.md).
    * A saját kód tooperiodically teszt írni a belső kiszolgálót. A kódra hello háttérfolyamatként egy tesztkiszolgálón, a tűzfal mögött található. A tesztelési folyamat küldhet az eredményeket tooApplication Insights segítségével [TrackAvailability()](https://docs.microsoft.com/dotnet/api/microsoft.applicationinsights.telemetryclient.trackavailability) API-nak hello core SDK-csomagot. Ez a vizsgálat server toohave kimenő hozzáférést toohello Application Insights adatfeldolgozást végpont szükséges, de ez sokkal kisebb biztonsági kockázatot mint hello alternatív, amely lehetővé teszi a bejövő kérelmeket. hello eredmények nem jelennek meg hello rendelkezésre állási webes tesztek paneleken, de Analytics, a Keresés és a metrika Explorer a rendelkezésre állási eredmények jelenik meg.
* *A többlépéses teszt feltöltése sikertelen*

    A méretkorlát: 300 KB.

    A hurkok nem támogatottak.

    Hivatkozások tooother webes vizsgálatok nem támogatottak.

    Az adatforrások nem támogatottak.
* *A többlépéses teszt nem fejeződik be*

    Egy teszt legfeljebb 100 kérelemből állhat.

    Ha két percnél hosszabb ideig fut hello teszt le van állítva.
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
