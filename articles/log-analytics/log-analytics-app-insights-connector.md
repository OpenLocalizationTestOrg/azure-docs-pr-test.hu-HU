---
title: "az alkalmazásadatok Azure Application Insights aaaView |} Microsoft Docs"
description: "Hello Application Insights-összekötő megoldás toodiagnose teljesítményproblémák használja, és mit a felhasználók az alkalmazással, ha az Application insights szolgáltatással figyelt ismertetése."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: 49280cad-3526-43e1-a365-c6a3bf66db52
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: banders
ms.openlocfilehash: 38109337ebbc8970dccb65365ba8284d9cee19a1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="application-insights-connector-solution-preview-in-operations-management-suite-oms"></a>Application Insights-összekötő megoldás (előzetes verzió) Operations Management Suite (OMS)

![Application Insights szimbólum](./media/log-analytics-app-insights-connector/app-insights-connector-symbol.png)

hello alkalmazások Insights-összekötő megoldás segítségével teljesítménnyel kapcsolatos problémák diagnosztizálásához és megérteni a felhasználók mit az alkalmazással, ha a számítógép megfigyelés alatt áll a [Application Insights](../application-insights/app-insights-overview.md). Nézetek hello az Application Insights fejlesztők látható azonos telemetriai érhetők el OMS. Azonban ha integrálja az Application Insights alkalmazások OMS, látható-e az alkalmazások jobb lesz, azzal, hogy a művelet és az alkalmazások adatainak egy helyen. Rendelkező hello azonos nézetek segítségével az alkalmazásfejlesztők rendelkező toocollaborate. hello közös nézetek segítségével csökkentheti a hello idő toodetect és az alkalmazás és a platform-problémák megoldása.

Hello megoldás használata esetén is:

- Az Application Insights alkalmazások megtekintése az egyik helyen, akkor is, ha a különböző Azure-előfizetések
- Az alkalmazásadatok infrastruktúra adatainak
- Alkalmazás-adatok ábrázolása a naplófájl-keresési nézetből
- Forgáspont Naplóelemzési adatok tooyour Application Insights alkalmazást az hello OMS és az Azure portálon

## <a name="connected-sources"></a>Összekapcsolt források

A legtöbb más Naplóelemzési megoldásoktól eltérően adatokat nem gyűjtötte be a hello Application Insights-összekötő-ügynökök által. Hello megoldás által használt összes adatok származnak, közvetlenül az Azure-ból.

| Összekapcsolt forrás | Támogatott | Leírás |
| --- | --- | --- |
| [Windows-ügynökök](log-analytics-windows-agents.md) | Nem | hello megoldás a Windows-ügynökök nem gyűjt adatokat. |
| [Linux-ügynökök](log-analytics-linux-agents.md) | Nem | hello megoldás a Linux-ügynökök nem gyűjt adatokat. |
| [SCOM felügyeleti csoport](log-analytics-om-agents.md) | Nem | hello megoldás az ügynökök a csatlakoztatott SCOM felügyeleti csoport nem gyűjt adatokat. |
| [Azure Storage-fiók](log-analytics-azure-storage.md) | Nem | hello megoldás nem az Azure storage nem gyűjtemény adatait. |

## <a name="prerequisites"></a>Előfeltételek

- tooaccess Application Insights-összekötővel kapcsolatos adatokat, egy Azure-előfizetéssel kell rendelkeznie
- Rendelkeznie kell legalább egy konfigurált Application Insights-erőforrást.
- Hello tulajdonos vagy közreműködő szerepkörrel hello Application Insights-erőforrás kell lennie.

## <a name="configuration"></a>Konfiguráció

1. Hello Azure Web Apps Analytics megoldást hello engedélyezése [Azure piactér](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.ApplicationInsights?tab=Overview) vagy ismertetett folyamatot hello segítségével [hozzáadni a Naplóelemzési megoldásokat az hello megoldások gyűjtemény](log-analytics-add-solutions.md).
2. Az OMS-portálon hello kattintson **beállítások** &gt; **adatok** &gt; **Application Insights**.
3. A **válasszon egy előfizetést**, válasszon egy előfizetést, amely rendelkezik az Application Insights-erőforrások majd a **alkalmazásnév**, válassza ki egy vagy több alkalmazást.
4. Kattintson a **Save** (Mentés) gombra.

A körülbelül 30 percet adatok elérhetővé válik, és az Application Insights csempe hello frissül az adatok, például a következő kép hello:

![Az Application Insights csempéje](./media/log-analytics-app-insights-connector/app-insights-tile.png)

Más pontok tookeep figyelembe vételével:

- Csak az Application Insights alkalmazások tooone OMS-munkaterület társíthatja.
- Csak társíthatja [Standard vagy prémium Application Insights-erőforrások](https://azure.microsoft.com/pricing/details/application-insights) tooOMS Naplóelemzési. Naplóelemzési hello ingyenes szint is használhatja.

## <a name="management-packs"></a>Felügyeleti csomagok

Ez a megoldás a csatlakoztatott felügyeleti csoportok nem telepíti a felügyeleti csomagok.

## <a name="use-hello-solution"></a>Hello megoldással

hello alábbiakban is látható módon hello Application Insights irányítópult tooview hello paneleken használatának és alkalmazásokból származó adatok kezeléséhez.

### <a name="view-application-insights-connector-information"></a>Application Insights-összekötő adatainak megtekintése

Kattintson a hello **Application Insights** csempe tooopen hello **Application Insights** irányítópult toosee hello panelen a következő.

![Alkalmazás irányítópult](./media/log-analytics-app-insights-connector/app-insights-dash01.png)

![Alkalmazás irányítópult](./media/log-analytics-app-insights-connector/app-insights-dash02.png)

hello irányítópult hello paneleken hello táblázat tartalmazza. Minden egyes panel be, hogy a panel a feltételek hello megadva hatókör és a kívánt időtartományt megfelelő too10 elemeket sorolja fel. A napló keresési, amely visszaadja az összes rekord kattintva futtathatja **láthatja az összes** hello panelről, vagy hello panel fejléc kattintva hello alján.

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

| **Oszlop** | **Leírás** |
| --- | --- |
| Alkalmazások – alkalmazások száma | Alkalmazások hello számát mutatja az alkalmazás-erőforrásokat. Is listák alkalmazás nevét, és az egyes, hello alkalmazás rekordok száma. Kattintson a szám toorun hello napló keresése<code>Type=ApplicationInsights &#124; measure sum(SampledCount) by ApplicationName</code> <br><br>  Kattintson az alkalmazás neve toorun hello alkalmazás, amely tartalmazza az alkalmazás rekordok száma állomás, telemetriai típus szerint rekordok és minden adat (a alapján hello utolsó napja) típus szerint a napló keresése. |
| Adatmennyiség – küldő adatokat tároló | A számítógépen, amely adatokat küldi hello számát jeleníti meg. A számítógépen és minden állomás számára rekordok száma is tartalmazza. Kattintson a szám toorun hello napló keresése<code>Type=ApplicationInsights &#124; measure sum(SampledCount) by Host</code> <br><br> Kattintson az a számítógép neve toorun hello fogadó alkalmazás rekordok száma állomás, telemetriai típus szerint rekordok és minden adat (a alapján hello utolsó napja) típus szerint megjelenítő napló keresése. |
| Rendelkezésre állás – eredmények webtesztben. | A perecdiagram webteszteredmények, sikeres, vagy sikertelen jelző jeleníti meg. Kattintson a diagram toorun hello napló keresése<code>Type=ApplicationInsights TelemetryType=Availability &#124; measure sum(SampledCount) by AvailabilityResult</code> <br><br> Az eredményben hello műveletek és az összes tesztre vonatkozóan hibák száma. Webalkalmazások összes forgalom az elmúlt percben hello jeleníti meg. Kattintson az alkalmazás neve tooview sikertelen webteszt részletes adatait megjelenítő napló keresést. |
| Kiszolgálói kérelmek – kérések száma óránként | Látható sor hello kiszolgálói kérelem a különböző alkalmazások óránként. Egy sor hello diagram toosee hello első 3 alkalmazásokban pontnál kérelmek fogadásának ideje mutasson. Kérések és a kérések száma hello fogadása hello kijelölt időszakra hello alkalmazások listáját is tartalmazza. <br><br>Kattintson a hello graph toorun napló keresése <code>Type=ApplicationInsights TelemetryType=Request &#124; measure sum(SampledCount) by ApplicationName interval 1hour</code> , amely látható részletesebb sor hello kiszolgálói kérelem a különböző alkalmazások óránként. <br><br> Kattintson az alkalmazás hello lista toorun napló keresése <code>Type=ApplicationInsights  ApplicationName=yourapplicationname  TelemetryType=Request</code> , amely listáját jeleníti meg a kéréseket, a diagramok kérelmek ideje és kérelem időtartama alatt és a kérelem listája válaszkódot.   |
| Hiba – sikertelen kérések száma óránként | A hibás alkalmazást kérelmek óránként sor diagramot ábrázol. Hello diagram toosee hello felső 3 az alkalmazásoknak a ponton a sikertelen kérelmek rámutat időben. Hello sikertelen kérelmek száma az egyes alkalmazások listáját is tartalmazza. Kattintson a diagram toorun hello napló keresése <code>Type=ApplicationInsights TelemetryType=Request  RequestSuccess = false &#124; measure sum(SampledCount) by ApplicationName interval 1hour</code> , amely bemutatja, hogy a hibás alkalmazást kérelmek részletesebb vonaldiagram. <br><br>Kattintson az elemre az lista toorun hello napló keresése <code>Type=ApplicationInsights ApplicationName=yourapplicationname TelemetryType=Request  RequestSuccess=false</code> , hogy megjelenítése sikertelen kérelmek, a diagramok sikertelen kérelmek ideje és kérelem időtartam és a sikertelen kérelmek válaszkódot listáját. |
| Kivételek – a kivételek száma óránként | Kivételek száma óránként sor diagramot ábrázol. Vigye időben hello diagram toosee hello legfontosabb 3 alkalmazást a pontok kivételével. A kivételek az egyes hello rendelkező alkalmazások listáját is tartalmazza. Kattintson a diagram toorun hello napló keresése <code>Type=ApplicationInsights TelemetryType=Exception &#124; measure sum(SampledCount) by ApplicationName interval 1hour</code> kivételek részletesebb hivatkozás diagramot ábrázol, amely. <br><br>Kattintson az elemre az lista toorun hello napló keresése <code>Type=ApplicationInsights  ApplicationName=yourapplicationname TelemetryType=Exception</code> , amelyek kivételek, időt és a sikertelen kérelmek keresztül kivételekhez diagramok és a kivétel típusainak listáját listáját tartalmazza.  |

### <a name="view-hello-application-insights-perspective-with-log-search"></a>Nézet hello napló keresése az Application Insights perspektíva

Ha hello irányítópult valamely elemére kattint, megjelenik az Application Insights perspektíva megjelennek a keresési. hello perspektíva biztosít egy kiterjesztett képi megjelenítés kiválasztott hello telemetriai adatok típusától függően. Igen, a képi megjelenítés a tartalmi változások különféle telemetriai esetében.

Ha kattint bárhol hello alkalmazások panelen, megjelenik az alapértelmezett hello **alkalmazások** szempontjából.

![Application Insights alkalmazások perspektíva](./media/log-analytics-app-insights-connector/applications-blade-drill-search.png)

hello perspektíva a kiválasztott alkalmazás hello áttekintését tartalmazza.

Hello **rendelkezésre állási** panelt jeleníti meg, különböző perspektíva ahol webteszteredmények és a kapcsolódó sikertelen kérelmek láthatók.

![Application Insights rendelkezésre állási perspektíva](./media/log-analytics-app-insights-connector/availability-blade-drill-search.png)

Ha kattint bárhol hello **kiszolgálói kérelmek** vagy **hibák** paneleken, összetevők módosítása toogive hello perspektíva meg, amelyek toorequests képi megjelenítés.

![Application Insights hibák panel](./media/log-analytics-app-insights-connector/server-requests-failures-drill-search.png)

Ha kattint bárhol hello **kivételek** panelen megjelenik a képi megjelenítés megfelelő tooexceptions.

![Application Insights kivételek panel](./media/log-analytics-app-insights-connector/exceptions-blade-drill-search.png)

Függetlenül attól, hogy valami valamelyik gombra hello **Application Insights-összekötő** irányítópult belül hello **keresési** lapon, az Application Insights az adat tartalmazza hello visszaadó lekérdezés Application Insights szempontjából. Ha az Application Insights adatokat, például egy **&#42;** lekérdezés is bemutatja, például a következő kép hello hello perspektíva lapon:

![Application Insights ](./media/log-analytics-app-insights-connector/app-insights-search.png)

Perspektíva összetevők hello keresési lekérdezés attól függően frissülnek. Ez azt jelenti, hogy hello eredményeket szűrheti bármely keresés mezője, amely az Ön hello képességét toosee által biztosított hello származó adatok használatával:

- Az alkalmazások
- Egyetlen kiválasztott alkalmazás
- Az alkalmazások azon csoportját

### <a name="pivot-tooan-app-in-hello-azure-portal"></a>Pivot tooan alkalmazás hello Azure-portálon

Application Insights-összekötő paneleken toopivot toohello kijelölt tervezett tooenable Application Insights app *hello OMS-portálon használatakor*. Egy magas szintű felügyeleti platform, amely segít az alkalmazás hibaelhárítása hello megoldás is használhatja. Amikor megjelenik egy potenciális problémát sem az egymáshoz kapcsolódó alkalmazások, vagy részletezési bele az OMS search is, vagy Ön közvetlenül toohello Application Insights alkalmazást is forgáspont.

toopivot, kattintson a hello folytatást jelző pontokra (**...** ), amely minden hello végén jelenik meg, és válassza ki **nyissa meg az Application Insightsban**.

>[!NOTE]
>**Nyissa meg az Application Insightsban** nem érhető el a hello Azure-portálon.

![Nyissa meg az Application Insightsban](./media/log-analytics-app-insights-connector/open-in-app-insights.png)

### <a name="sample-corrected-data"></a>A minta-javítani adatok

Az Application Insights biztosít  *[javítási mintavételi](../application-insights/app-insights-sampling.md)*  toohelp telemetriai forgalom csökkentése. Az Application Insights-alkalmazás mintavételi engedélyezi, az Application Insights és az OMS tárolt bejegyzések csökkentett számos nyílik meg. Közben az adatok konzisztenciájának megőrződik a hello **Application Insights-összekötő** perspektívák, és a lap a egyéni lekérdezések általában elhárítja mintaadatokat manuálisan.

Itt látható egy példa a naplófájl-keresési lekérdezés mintavételi javítása:

```
Type=ApplicationInsights | measure sum(SampledCount) by TelemetryType
```

Hello **mintát száma** mező összes bejegyzés szerepel-e, és látható hello hello bejegyzést képviselő adatpontok száma. Ha bekapcsolja a mintavétel az Application Insights alkalmazás **mintát száma** nagyobb, mint 1. toocount hello tényleges bejegyzések száma, amely az alkalmazás hoz létre, sum hello **mintát száma** mezőket.

Mintavételi érint csak hello összes bejegyzés száma, amely az alkalmazás létrehozza. Nem kell toocorrect mintavételi vonatkozó Átjárómetrika mezők, például **RequestDuration** vagy **AvailabilityDuration** mivel ezek a mezők megjelenítése hello átlagos képviselt bejegyzések.

## <a name="input-data"></a>A bemeneti adatok

hello megoldás megkapja a következő telemetriai adattípusok csatlakoztatott Application Insights alkalmazásokból származó hello:

- Rendelkezésre állás
- Kivételek
- Kérelmek
- Lapmegtekintések – a munkaterület tooreceive lapmegtekintés, konfigurálnia kell az alkalmazások toocollect ezt az információt. További információkat lásd: [PageViews](../application-insights/app-insights-api-custom-events-metrics.md#page-views).
- Egyéni események – a munkaterület-tooreceive egyéni események, konfigurálnia kell az alkalmazások toocollect ezt az információt. További információkat lásd: [TrackEvent](../application-insights/app-insights-api-custom-events-metrics.md#trackevent).

Amint az elérhetővé válik az Application Insights OMS adatok érkezik.

## <a name="output-data"></a>kimeneti adatok

Egy rekordot egy *típus* a *ApplicationInsights* jön létre az egyes bemeneti adatokat. ApplicationInsights bejegyzések megjelennek a következő részekben hello jellemzőkkel rendelkezik:

### <a name="generic-fields"></a>Általános mezők

| Tulajdonság | Leírás |
| --- | --- |
| Típus | ApplicationInsights |
| Ügyfélip |   |
| TimeGenerated | Hello rekord idő |
| ApplicationId | Az Application Insights alkalmazás hello Instrumentation kulcs |
| ApplicationName | Hello Application Insights alkalmazás nevét |
| RoleInstance | Kiszolgáló állomás azonosítója |
| DeviceType | Ügyféleszközök |
| ScreenResolution |   |
| Kontinens | Hello kérelem származási helyének kontinensen |
| Ország | Hello kérelem származási helyének ország |
| Tartomány | Tartomány vagy állapot, vagy ha a kérelem származási hello területi beállítás |
| Város | Település hello kérelem származási helyének |
| isSynthetic | Azt jelzi, hogy hello iránti kérelem létrehozása egy felhasználó által vagy automatikus módon. = Igaz a felhasználók által létrehozott vagy false = automatikus módszer |
| Érvénytelen a SamplingRate | Hello tooportal küldött SDK által generált telemetriai százalékát. Tartomány 0,0-100.0. |
| SampledCount | 100/(SamplingRate). Például, 4 =&gt; 25 %-át |
| IsAuthenticated | Igaz vagy hamis |
| OperationID azonosítójú | Azonos Műveletazonosító hello portálon sablonobjektumhoz kapcsolódó elemként látható hello rendelkező elemek. Általában a hello kérelem azonosítója |
| ParentOperationID | Hello szülő művelet azonosítója |
| OperationName |   |
| Munkamenet-azonosító | GUID toouniquely azonosítása hello munkamenet, ahol hello iránti kérelem létrehozása |
| SourceSystem | ApplicationInsights |

### <a name="availability-specific-fields"></a>Rendelkezésre állási-specifikus mezők

| Tulajdonság | Leírás |
| --- | --- |
| TelemetryType | Rendelkezésre állás |
| AvailabilityTestName | Hello webalkalmazás-teszt neve |
| AvailabilityRunLocation | A http-kérelem földrajzi forrás |
| AvailabilityResult | Azt jelzi, hello webteszt hello sikeres eredménye |
| AvailabilityMessage | üdvözlőüzenetére csatolt toohello webalkalmazás-teszt |
| AvailabilityCount | 100 /(Sampling Rate). Például, 4 =&gt; 25 %-át |
| DataSizeMetricValue | 1.0 vagy 0,0-nál |
| DataSizeMetricCount | 100 /(Sampling Rate). Például, 4 =&gt; 25 %-át |
| AvailabilityDuration | Idő hello webes vizsgálat időtartama ezredmásodpercben |
| AvailabilityDurationCount | 100 /(Sampling Rate). Például, 4 =&gt; 25 %-át |
| AvailabilityValue |   |
| AvailabilityMetricCount |   |
| AvailabilityTestId | Hello webteszt egyedi GUID azonosítója |
| AvailabilityTimestamp | Hello elérhetőségi teszt pontos időbélyegzője |
| AvailabilityDurationMin | A mintában szereplő rekordok ebben a mezőben látható hello minimális webes vizsgálat időtartama (ezredmásodpercben) helyőrzővel jelzett Eredménymező hello adatpontok |
| AvailabilityDurationMax | A mintában szereplő rekordok ebben a mezőben látható hello maximális webes vizsgálat időtartama (ezredmásodpercben) helyőrzővel jelzett Eredménymező hello adatpontok |
| AvailabilityDurationStdDev | A mintában szereplő rekordok ebben a mezőben látható minden webes teszt időtartamát (ezredmásodperc) helyőrzővel jelzett Eredménymező hello adatpontok közötti hello szórás |
| AvailabilityMin |   |
| AvailabilityMax |   |
| AvailabilityStdDev | &nbsp;  |

### <a name="exception-specific-fields"></a>Kivétel-specifikus mezők

| Típus | ApplicationInsights |
| --- | --- |
| TelemetryType | Kivétel |
| ExceptionType | Hello kivétel típusa |
| ExceptionMethod | hello módszer által létrehozott hello kivétel |
| ExceptionAssembly | Szerelvény tartalmaz hello keretrendszer hello nyilvánoskulcs-kivonatnak továbbá |
| ExceptionGroup | Hello kivétel típusa |
| ExceptionHandledAt | Kezelt kivétel hello hello szintjét jelzi |
| ExceptionCount | 100 /(Sampling Rate). Például, 4 =&gt; 25 %-át |
| ExceptionMessage | Hello kivétel üzenetét |
| ExceptionStack | Teljes verem hello kivétel |
| ExceptionHasStack | IGAZ, ha kivétel a verem |



### <a name="request-specific-fields"></a>Kérelem-specifikus mezők

| Tulajdonság | Leírás |
| --- | --- |
| Típus | ApplicationInsights |
| TelemetryType | Kérés |
| ResponseCode | HTTP-választ küldött tooclient |
| RequestSuccess | Azt jelzi, sikeres vagy sikertelen volt. IGAZ vagy hamis értéket. |
| Kérelemazonosító | Azonosító toouniquely hello kérelem azonosítása |
| RequestName | GET/POST + alap URL-je |
| RequestDuration | Hello kérelem időtartam másodpercben |
| URL-CÍME | Hello kérelem nem többek között a gazdagépre URL-címe |
| Gazdagép | Web server állomás |
| URLBase | Hello kérelem teljes URL-címe |
| ApplicationProtocol | Hello alkalmazás által használt protokoll típusát |
| RequestCount | 100 /(Sampling Rate). Például, 4 =&gt; 25 %-át |
| RequestDurationCount | 100 /(Sampling Rate). Például, 4 =&gt; 25 %-át |
| RequestDurationMin | A mintában szereplő rekordok ebben a mezőben látható hello képviselt hello kiszámításához az adatpontok minimális kérelem időtartama (ezredmásodperc). |
| RequestDurationMax | A mintában szereplő rekordok ebben a mezőben látható képviselt hello adatpontok hello kérelmek maximális időtartama (ezredmásodpercben) |
| RequestDurationStdDev | A mintában szereplő rekordok ebben a mezőben látható minden kérelem időtartamát (ezredmásodperc) helyőrzővel jelzett Eredménymező hello kiszámításához az adatpontok közötti hello szórás |

## <a name="sample-log-searches"></a>Naplókeresési minták

Ez a megoldás nem rendelkeznek egy minta napló keresések hello irányítópulton látható. Azonban leírását a napló keresési lekérdezés hello látható [nézet Application Insights-összekötő információk](#view-application-insights-connector-information) szakasz.

## <a name="next-steps"></a>Következő lépések

- Használjon [naplófájl-keresési](log-analytics-log-searches.md) tooview részletes adatok az Application Insights-alkalmazásokhoz.
