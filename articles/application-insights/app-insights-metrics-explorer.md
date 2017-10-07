---
title: "a Azure Application Insights metrikák aaaExploring |} Microsoft Docs"
description: "Hogyan toointerpret a metrika explorer diagramokat, és hogyan toocustomize metrika explorer paneleken."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 1f471176-38f3-40b3-bc6d-3f47d0cbaaa2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/08/2017
ms.author: bwren
ms.openlocfilehash: b77ae227ae61e800ad6f3af8a05cd123ea1d69e1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="exploring-metrics-in-application-insights"></a>Az Application Insightsban metrikák felfedezése
A metrikák [Application Insights] [ start] mért értékek és az alkalmazásból küldött telemetriai az események számát. Segítenek azoknak teljesítményproblémák észlelését, és tekintse meg az alkalmazás használatának alakulását. Számos különböző szabványos metrikákat, és a saját egyéni metrikákkal és eseményekkel is létrehozhat.

Metrikák és az esemény számát, például összegeket, átlagok vagy számok összesített értékek diagramokban jelennek meg.

Itt egy olyan minta diagramok:

![](./media/app-insights-metrics-explorer/01-overview.png)

Metrikák diagramok mindenhol hello Application Insights portálon találja. A legtöbb esetben testre szabható, és hozzáadhat további diagramok toohello panelen. Hello áttekintése panelen, kattintson a toomore részletes diagramokat, (amelyek címek például a "Kiszolgáló"), vagy kattintson a **Metrikaböngésző** tooopen egy új panel, ahol egyéni diagramokat hozhat létre.

## <a name="time-range"></a>Időtartomány
Hello időtartomány hello diagramok vagy rácsok bármely panelen módosíthatja.

![Nyissa meg hello áttekintése panel az alkalmazás a hello Azure-portálon](./media/app-insights-metrics-explorer/03-range.png)

Ha néhány adat, amely még nem történt vár, a frissítés gombra. Diagramok időközönként frissítse magát, de hello időközök hosszabb ideig nagyobb időtartomány megadásával. A diagram alakzatot is igénybe vehet igénybe az adatok toocome hello analysis-feldolgozási folyamaton keresztül.

a diagram részére történő toozoom húzza azt:

![Húzza a diagram része keresztül.](./media/app-insights-metrics-explorer/12-drag.png)

Kattintson a hello visszavonása Nagyítás gomb toorestore azt.

## <a name="granularity-and-point-values"></a>Granularitási és a pont értékét
Az egérmutatóval rámutat hello toodisplay hello Diagramértékek hello mérőszámokat ezen a ponton.

![Hello egér rámutat egy diagram](./media/app-insights-metrics-explorer/02-focus.png)

adott helyen hello metrika hello értékének hello megelőző mintavételi időköze alatt összesített értéket.

hello mintavételi időköze vagy "granularitási" hello panel hello tetején látható.

![a panel hello fejlécében.](./media/app-insights-metrics-explorer/11-grain.png)

Hello granularitási hello idő a tartomány panelen módosíthatja:

![a panel hello fejlécében.](./media/app-insights-metrics-explorer/grain.png)

hello Granularitás van elérhető választja hello időtartomány függ. hello explicit Granularitás van olyan alternatív toohello "automatikus" részletességgel hello időtartományát.


## <a name="editing-charts-and-grids"></a>Diagramok és táblázatok szerkesztése
Új diagram toohello panel tooadd:

![A Metrikaböngésző válassza a diagram hozzáadása](./media/app-insights-metrics-explorer/04-add.png)

Válassza ki **szerkesztése** egy meglévő vagy új diagram tooedit a mi mutatja:

![Válasszon egy vagy több metrikák](./media/app-insights-metrics-explorer/08-select.png)

Jelenítheti meg több mint egy metrika a diagramon, bár hello kombinációk együtt megjeleníthető kapcsolatos korlátozások vonatkoznak. Amint egy metrika, mások le vannak tiltva hello némelyike választja.

Ha Ön a kódolt [egyéni metrikák] [ track] az alkalmazásba (hívások tooTrackMetric és TrackEvent) azokat a rendszer itt listázza.

## <a name="segment-your-data"></a>Az adatok szegmens
A metrika fel tulajdonság - például toocompare oldalmegtekintéseket ügyfelek operációs rendszerrel.

Jelöljön ki egy diagram vagy a rács, váltson csoportosítás, és válasszon egy tulajdonság toogroup szerint:

![Jelölje be csoportosítás a, majd a készlet válasszon ki egy tulajdonságot a Group By](./media/app-insights-metrics-explorer/15-segment.png)

> [!NOTE]
> Csoportosítás használatakor hello terület és sávdiagram típusok halmozott megjelenítésre adja meg. Ez a megfelelő ahol hello összesítési módszer összege. De hello összesítési típusát az átlagot, hello vonal, vagy a rács megjelenítési típusa.
>
>

Ha Ön a kódolt [egyéni metrikák] [ track] az alkalmazásba azok tulajdonság értékével, és be fog tudni tooselect hello tulajdonság hello listában.

Az hello diagram túl kicsi a szegmentált adatokat? Magasságának beállítása:

![Hello csúszka](./media/app-insights-metrics-explorer/18-height.png)

## <a name="aggregation-types"></a>Aggregáció típusa
hello jelmagyarázat hello ügyféloldali alapértelmezés szerint a hello időszakon belül hello diagram általában jeleníti meg hello összesített értékét. Ha hello diagram mutat, ezen a ponton mutatja hello érték.

Minden adatpontnál hello diagram érkezett az előző mintavételi időköze vagy "granularitási" hello hello adatértékek összesítő. hello granularitási hello panel hello tetején látható, és hello művelettől hello diagram átfogó időskálára.

Metrikák összesíthetők különböző módon:

* **Count** hello mintavételi időköze kapott hello események száma. Az események például kérések szolgál. Hello diagram változatait hello magassága változatok a hello aránya hello események előfordulási gyakoriságát jelzi. De hello számértéket módosul hello mintavételi időköze módosításakor.
* **Sum** hello mintavételi időköze vagy hello időszak hello diagram protokollal fogadott összes hello adatpontok hello értékeket ad hozzá.
* **Átlagos** felosztása Sum hello hello hello időszakban kapott adatpontok száma alapján.
* **Egyedi** számát is, hogy a felhasználók és számok használhatók. Hello mintavételi időszakban, vagy hello diagram hello időszakban hello ábrán látható, hogy inkonzisztensek különböző felhasználók hello száma.
* **%**-minden Összesítés százalékos verzióit csak szegmentált diagramok használt. teljes hello mindig too100 % ad hozzá, és hello diagram ábrázolja hello relatív hozzájárulás összesen különböző összetevőinek.

    ![Összesítési százalékos aránya](./media/app-insights-metrics-explorer/percentage-aggregation.png)

### <a name="change-hello-aggregation-type"></a>Hello összesítési típusának módosítása

![Hello diagram szerkesztése, és válassza ki az összesítés](./media/app-insights-metrics-explorer/05-aggregation.png)

Új diagram és mikor van bejelölve összes metrikák létrehozásakor hello alapértelmezett eszköze mindegyik metrikát látható:

![Törölje az összes metrikák toosee hello alapértelmezett értéket](./media/app-insights-metrics-explorer/06-total.png)

## <a name="pin-y-axis"></a>PIN-kód y tengely 
Alapértelmezés szerint a diagram Y tengely értékeit: hello adatok közé, toogive hello értékek quantum vizuális ábrázolását maximális értékeket nulla-től kezdődő jeleníti meg. De egyes esetekben több mint előfordulhat, hogy érdekes toovisually hello quantum vizsgálhatja értékek kisebb változásokat. Az egyéni beállításokat, például ehhez hello y tengely szerkesztési szolgáltatás toopin hello y tengely minimális vagy maximális tartományértéke kívánt helyen.
Kattintson a "Speciális beállítások" jelölőnégyzetet toobring mentése hello y tengely tartomány beállításai

![Kattintson a Speciális beállítások, egyéni tartományt, adja meg a minimális maximális értékek](./media/app-insights-metrics-explorer/y-axis-range.png)

## <a name="filter-your-data"></a>Szűrje az adatokat
egy kijelölt tulajdonság értékhalmazt toosee csak hello metrikáját:

![Kattintson a szűrő, bontsa ki a tulajdonságot, és ellenőrizze az egyes értékeket](./media/app-insights-metrics-explorer/19-filter.png)

Minden olyan értéket adott tulajdonság nem adja meg, ha az rendelkezik hello azonos, válassza ki azokat az összes: nincs szűrő az adott tulajdonsághoz.

Értesítés hello álló események mellett minden egyes tulajdonság értéke. Amikor kiválaszt egy tulajdonság értékének, hello mellett egyéb tulajdonság értékek száma.

Szűrők tooall hello diagramok a panelek lesznek alkalmazva. Ha azt szeretné, hogy a különböző szűrőket toodifferent diagramok, hozzon létre, és más metrikák paneleken mentéséhez. Ha azt szeretné, különböző paneleken toohello irányítópultról diagramok rögzítheti, így egymás mellett láthatja.

### <a name="remove-bot-and-web-test-traffic"></a>Távolítsa el a botot és webes teszt forgalom
Hello szűrő használata **valós vagy szintetikus forgalom** , és ellenőrizze, **valós**.

Alapján is szűrheti **szintetikus forgalom forrása**.

### <a name="tooadd-properties-toohello-filter-list"></a>tooadd tulajdonságok toohello szűrőlista
Szeretné toofilter telemetriai adatokat saját kiválasztása kategóriát? Például lehet, hogy a felhasználók különböző kategóriákba fel osztani, és szeretné szegmentálni adatait ezen kategóriák szerinti.

[Hozzon létre egy saját tulajdonságot](app-insights-api-custom-events-metrics.md#properties). Állítsa be azt az egy [Telemetriai inicializáló](app-insights-api-custom-events-metrics.md#defaults) toohave akkor jelenik meg a összes telemetriai adat - különböző SDK modulok által küldött hello szabványos telemetriai beleértve.

## <a name="edit-hello-chart-type"></a>Hello diagramtípus szerkesztése
Figyelje meg, hogy válthat között rácsok és diagramokat:

![A rács és a graph, majd a diagram típusát](./media/app-insights-metrics-explorer/16-chart-grid.png)

## <a name="save-your-metrics-blade"></a>A metrikák panel mentése
Amikor az egyes diagramok létrehozott, mentse azokat a Kedvencek közé. Kiválaszthatja, hogy tooshare azt más csapattagok számára, ha a szervezeti fiókot használjon.

![Kattintson a kedvenc](./media/app-insights-metrics-explorer/21-favorite-save.png)

Ebben az esetben a toosee hello panel **lépjen toohello áttekintése panel** , és nyissa meg a Kedvencek:

![Hello áttekintése paneljén válassza a Kedvencek közé](./media/app-insights-metrics-explorer/22-favorite-get.png)

Ha relatív időtartomány mentésekor, hello panel frissítődik hello legújabb metrikákat. Ha úgy dönt, hogy abszolút időtartomány, meg fog jelenni hello ugyanazokat az adatokat, minden alkalommal.

## <a name="reset-hello-blade"></a>Hello panel alaphelyzetbe állítása
Ha szerkeszti egy panel, de majd tooget hátsó toohello eredeti mentett készletet szeretné, kattintson alaphelyzetbe állítása.

![A metrika Explorer hello tetején hello gombok](./media/app-insights-metrics-explorer/17-reset.png)

## <a name="live-metrics-stream"></a>Élő metrikák adatfolyam

A telemetriai adatok sokkal azonnali megtekintéséhez nyissa meg a [élő adatfolyam](app-insights-live-stream.md). Legtöbb metrikák hello folyamat összesítés miatt igénybe vehet néhány percet tooappear. Ezzel szemben élő metrikákat a kis késleltetésű vannak optimalizálva. 

## <a name="set-alerts"></a>Riasztások beállítása
rendkívüli értékek a metrika az e-mailben értesítést toobe adja hozzá a riasztást. Kiválaszthatja a toosend hello e-mail toohello a rendszergazdák vagy toospecific e-mail címet.

![A Metrikaböngészőben válassza ki a riasztási szabályok, riasztások hozzáadása](./media/app-insights-metrics-explorer/appinsights-413setMetricAlert.png)

[További információ a riasztások][alerts].


## <a name="continuous-export"></a>Folyamatos exportálás
Ha azt szeretné, hogy külsőleg is feldolgozhat folyamatosan exportált adatok, érdemes lehet [a folyamatos exportálás](app-insights-export-telemetry.md).

### <a name="power-bi"></a>Power BI
Ha azt szeretné, hogy az adatok is nézeteket, akkor [tooPower BI exportálása](http://blogs.msdn.com/b/powerbi/archive/2015/11/04/explore-your-application-insights-data-with-power-bi.aspx).

## <a name="analytics"></a>Elemzés
[Elemzés](app-insights-analytics.md) van egy rugalmasabb módon tooanalyze a telemetriai hatékony lekérdezési nyelv használatával. Használnia, ha szeretné, hogy toocombine vagy számítási metrikák eredményeinek, vagy indítson el egy részletes feltárása az alkalmazás legújabb teljesítményt. 

Metrika diagram, hello Analytics ikon tooget rákattinthat közvetlenül toohello egyenértékű Analytics-lekérdezés.

## <a name="troubleshooting"></a>Hibaelhárítás
*A diagram nem szerepel az adatokat.*

* Szűrők alkalmazása tooall hello diagramok hello panelen. Győződjön meg arról, hogy egy diagram van összpontosít, amíg nem adott meg, amely nem tartalmazza az összes hello adatokat egy másik szűrőt.

    Ha azt szeretné, hogy a különböző szűrőket tooset különböző diagramok, hozza létre azokat különböző paneleken, mentse azokat a szerint külön Kedvencek. Ha azt szeretné, akkor is rögzítheti őket toohello irányítópult, hogy egymás mellett láthatja.
* Ha a diagram olyan tulajdonságon, amely nincs definiálva a következő hello metrika szerint csoportosítja, majd lesz semmi hello diagram. Próbálja meg törölni a "group by", vagy válasszon egy másik csoportosítási tulajdonságot.
* Teljesítményadatok (CPU, IO sebessége, és így tovább) Java web Services, a Windows asztali alkalmazások esetén érhető el [IIS webes alkalmazások és szolgáltatások telepítése állapotfigyelő](app-insights-monitor-performance-live-website-now.md), és [Azure Cloud Services](app-insights-azure.md). Nem érhető el az Azure-webhelyek.

## <a name="video"></a>Videó

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a>Következő lépések
* [Az Application insights szolgáltatással megfigyelési kihasználtsága](app-insights-web-track-usage.md)
* [Diagnosztikai keresés](app-insights-diagnostic-search.md)

<!--Link references-->

[alerts]: app-insights-alerts.md
[start]: app-insights-overview.md
[track]: app-insights-api-custom-events-metrics.md
