---
title: "aaaGet Azure Application Insights az eclipse-ben Java használatába |} Microsoft docs"
description: "Hello Eclipse beépülő modul tooadd teljesítmény és a használat figyelés tooyour Java webhely az Application insights szolgáltatással"
services: application-insights
documentationcenter: java
author: CFreemanwa
manager: carmonm
ms.assetid: e88c9f53-cd90-4abc-b097-1f170937908e
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 12/12/2016
ms.author: bwren
ms.openlocfilehash: 3142a26a9e2d14c2c433882e3d337f2a8c8f2247
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-application-insights-with-java-in-eclipse"></a>Ismerkedés az Application Insights Java az eclipse-ben
hello Application Insights SDK, hogy elemezheti a használati és teljesítményadatokat küld a Java-webalkalmazások telemetriai adatokat. hello eclipse-ben az Application Insights beépülő modult úgy, hogy be hello mezőben telemetriai adatokat, valamint az API-k használható toowrite egyéni telemetria kívül automatikusan telepíti hello SDK a projektben.   

## <a name="prerequisites"></a>Előfeltételek
Hello beépülő modul jelenleg működik Maven-projektek és dinamikus webes projektek az eclipse-ben.
([Java-projektet az Application Insights hozzáadása tooother típusú][java].)

A következők szükségesek:

* Oracle JRE 1.6 vagy újabb verzió
* Előfizetés túl[Microsoft Azure](https://azure.microsoft.com/).
* [Java EE-fejlesztőknek IDE Holdas](http://www.eclipse.org/downloads/), Indigo vagy újabb.
* Windows 7 vagy újabb, vagy a Windows Server 2008 vagy újabb

## <a name="install-hello-sdk-on-eclipse-one-time"></a>Az eclipse-ben (egyszer) hello SDK telepítése
Csak akkor kell toodo gépenként egyetlen most. Ez a lépés telepíti egy eszközkészlet, amelyet ezután felvehet hello SDK tooeach dinamikus webes projekt.

1. Az eclipse-ben kattintson a Súgó, új szoftverek telepítése.

    ![Súgó, új szoftverek telepítése](./media/app-insights-java-eclipse/0-plugin.png)
2. hello SDK http://dl.microsoft.com/eclipse Azure eszközkészlet alatt van.
3. Törölje a jelet **lépjen kapcsolatba az összes frissítés hely...**

    ![Application Insights SDK törölje lépjen kapcsolatba az összes frissítés helyek](./media/app-insights-java-eclipse/1-plugin.png)

Kövesse a hátralévő lépéseket minden olyan projekthez Java hello.

## <a name="create-an-application-insights-resource-in-azure"></a>Az Application Insights-erőforrás létrehozása az Azure-ban
1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Hozzon létre egy új Application Insights-erőforrást. Állítsa be a hello alkalmazás típusú tooJava webes alkalmazást.  

    ![Kattintson a + gombra, és válassza az Application Insights lehetőséget.](./media/app-insights-java-eclipse/01-create.png)  

4. Hello instrumentation kulcs hello új erőforrás található. Szüksége lesz toopaste Ez a kód projektben hamarosan.  

    ![Hello új erőforrás-áttekintés kattintson a tulajdonságok és hello Instrumentation kulcs másolása](./media/app-insights-java-eclipse/03-key.png)  

## <a name="add-application-insights-tooyour-project"></a>Az Application Insights tooyour projekt hozzáadása
1. Az Application Insights hozzáadása a Java webes projekt hello helyi menüből.

    ![Hello új erőforrás-áttekintés kattintson a tulajdonságok és hello Instrumentation kulcs másolása](./media/app-insights-java-eclipse/02-context-menu.png)
2. Beillesztés hello instrumentation kulcs portáltól kapott hello Azure-portálon.

    ![Hello új erőforrás-áttekintés kattintson a tulajdonságok és hello Instrumentation kulcs másolása](./media/app-insights-java-eclipse/03-ikey.png)

minden telemetriai tétel együtt küldött hello kulcs, és közli az Application Insights toodisplay legyen az erőforrás.

## <a name="run-hello-application-and-see-metrics"></a>Hello alkalmazás futtatásához, és tekintse meg a metrikák
Futtassa az alkalmazást.

A Microsoft Azure Application Insights-erőforrás tooyour adja vissza.

HTTP-kérelmek adatai hello áttekintése panelen jelenik meg. (Ha nincsenek ott, várjon néhány másodpercig, majd kattintson a Frissítés gombra.)

![Kiszolgáló válasza, az ügyfélkérelmek számát és a hibák ](./media/app-insights-java-eclipse/5-results.png)

Kattintson a diagram toosee keresztül metrikák részletes.

![A kérelmek számát név szerint](./media/app-insights-java-eclipse/6-barchart.png)

[További információk a metrikákról.][metrics]

És egy kérelem hello tulajdonságainak megtekintésekor láthatja hello telemetriai események például a kérelmek és kivételek társítva.

![A kérelemhez tartozó összes adat](./media/app-insights-java-eclipse/7-instance.png)

## <a name="client-side-telemetry"></a>Ügyféloldali telemetriai adat
Hello gyors üzembe helyezés panel kattintson a weblapokat Get kód toomonitor:

![Az alkalmazás áttekintése paneljén válassza a gyors üzembe helyezés, majd kód toomonitor a weblapokat. Másolja a hello parancsfájlt.](./media/app-insights-java-eclipse/02-monitor-web-page.png)

Helyezze be a HTML-fájlok hello vezetője hello kódrészletet.

#### <a name="view-client-side-data"></a>Az ügyféloldali adatok megtekintése
Nyissa meg a frissített weblapok és használhatják azokat. Várjon, amíg egy-két percen, majd térjen vissza a tooApplication elemzések és a nyitott hello használati panelen. (Hello áttekintése panelen görgessen lefelé, és kattintson használati.)

Lap nézet, a felhasználó és a munkamenet metrikák hello használati panelen jelenik meg:

![Munkamenetek, a felhasználók és az oldalmegtekintéseket](./media/app-insights-java-eclipse/appinsights-47usage-2.png)

[További információ az ügyféloldali telemetriai beállítása.][usage]

## <a name="publish-your-application"></a>Az alkalmazás közzététele
Most már a toohello alkalmazáskiszolgálóra közzétételéhez a felhasználók használni, és tekintse meg a hello telemetriai hello portálon jelenik meg.

* Győződjön meg arról, hogy a tűzfala lehetővé teszi, hogy az alkalmazás toosend telemetriai toothese portok:

  * dc.services.visualstudio.com:443
  * dc.services.visualstudio.com:80
  * f5.services.visualstudio.com:443
  * f5.services.visualstudio.com:80
* Windows-kiszolgálókon telepítse a következőt:

  * [Microsoft Visual C++ újraterjeszthető csomag](http://www.microsoft.com/download/details.aspx?id=40784)

    (Ez lehetővé teszi a teljesítményszámlálókat.)

## <a name="exceptions-and-request-failures"></a>Kivételek és kérelemhibák
A rendszer a nem kezelt kivételeket automatikusan begyűjti:

![](./media/app-insights-java-eclipse/21-exceptions.png)

más kivételek toocollect adatok, két lehetősége van:

* [INSERT tooTrackException meghívja a kódban](app-insights-api-custom-events-metrics.md#trackexception).
* [Hello Java-ügynök telepítése a kiszolgálón](app-insights-java-agent.md). Megadhatja a kívánt toowatch hello módszerek.

## <a name="monitor-method-calls-and-external-dependencies"></a>Metódushívások és külső függőségek megfigyelése
[Hello Java-ügynök telepítése](app-insights-java-agent.md) toolog megadott belső módszerek és adatokkal időzítése keresztül JDBC, felé indított hívások.

## <a name="performance-counters"></a>Teljesítményszámlálók
Az Áttekintés panelen görgessen lefelé, és kattintson a hello **kiszolgálók** csempére. Teljesítményszámlálók számos láthatja.

![Görgessen lefelé tooclick hello kiszolgálók csempéje](./media/app-insights-java-eclipse/11-perf-counters.png)

### <a name="customize-performance-counter-collection"></a>Teljesítményszámláló-gyűjtemény testreszabása
hello szabványos készletét, a teljesítményszámlálók gyűjteményét toodisable adja hozzá a következő kódot a gyökércsomópont hello hello ApplicationInsights.xml fájl hello:

```XML

    <PerformanceCounters>
       <UseBuiltIn>False</UseBuiltIn>
    </PerformanceCounters>
```

### <a name="collect-additional-performance-counters"></a>További teljesítményszámlálók gyűjtése
További teljesítmény számlálók toobe összegyűjtött adhatja meg.

#### <a name="jmx-counters-exposed-by-hello-java-virtual-machine"></a>JMX számlálók (hello Java virtuális gép által elérhetővé tett)

```XML

    <PerformanceCounters>
      <Jmx>
        <Add objectName="java.lang:type=ClassLoading" attribute="TotalLoadedClassCount" displayName="Loaded Class Count"/>
        <Add objectName="java.lang:type=Memory" attribute="HeapMemoryUsage.used" displayName="Heap Memory Usage-used" type="composite"/>
      </Jmx>
    </PerformanceCounters>
```

* `displayName`– hello Application Insights portálon megjelenő hello nevét.
* `objectName`– hello JMX objektum nevét.
* `attribute`– hello JMX objektum neve toofetch hello attribútuma
* `type`(nem kötelező) – hello JMX objektum attribútum típusa:
  * Alapértelmezett: egyszerű típus, például int vagy long.
  * `composite`: a "Attribute.Data" hello formátumban van hello teljesítményszámláló adatait
  * `tabular`: a tábla sorának hello formátumban van hello teljesítményszámláló adatait

#### <a name="windows-performance-counters"></a>Windows-teljesítményszámlálók
Minden egyes [Windows teljesítményszámláló](https://msdn.microsoft.com/library/windows/desktop/aa373083.aspx) kategória tagja (a hello azonos módon, hogy egy mező osztály tagjai). A kategóriák globálisak lehetnek, vagy számozott vagy elnevezett példányokkal rendelkezhetnek.

```XML

    <PerformanceCounters>
      <Windows>
        <Add displayName="Process User Time" categoryName="Process" counterName="%User Time" instanceName="__SELF__" />
        <Add displayName="Bytes Printed per Second" categoryName="Print Queue" counterName="Bytes Printed/sec" instanceName="Fax" />
      </Windows>
    </PerformanceCounters>
```

* displayName – hello Application Insights portálon megjelenő hello nevét.
* categoryName – hello teljesítményszámláló-kategória (teljesítményobjektum), amelyhez ez a teljesítményszámláló társítva.
* counterName – hello teljesítményszámláló hello nevét.
* a példánynév – hello hello teljesítményszámláló-kategória példány nevét, vagy üres karakterlánc (""), ha hello kategória egyetlen példányt tartalmaz. Ha hello categoryName folyamat, és azt szeretné, hogy toocollect hello teljesítményszámláló hello aktuális JVM folyamat során a, amely az alkalmazás fut, adja meg `"__SELF__"`.

A teljesítményszámlálói egyéni mérőszámokként láthatók a [Metrikaböngészőben][metrics].

![](./media/app-insights-java-eclipse/12-custom-perfs.png)

### <a name="unix-performance-counters"></a>Unix-teljesítményszámlálók
* [Hello Application Insights beépülő modul telepítése collectd](app-insights-java-collectd.md) tooget széles körének a rendszer és a hálózati adatokat.

## <a name="availability-web-tests"></a>Rendelkezésre állási webes tesztek
Az Application Insights tesztelheti, hogy működik-e rendszeres időközönként toocheck és is válaszol a webhelyen. [másolatot tooset][availability], tooclick rendelkezésre állási görgetve.

![Görgessen lefelé, kattintson a Rendelkezésre állás elemre, majd a Webes teszt hozzáadása elemre](./media/app-insights-java-eclipse/31-config-web-test.png)

Megkapja a válaszidők diagramjait, valamint e-mailes értesítéseket kap, ha a webhely leáll.

![Példa webes tesztre](./media/app-insights-java-eclipse/appinsights-10webtestresult.png)

[További információk a rendelkezésre állási webes tesztekről.][availability]

## <a name="diagnostic-logs"></a>Diagnosztikai naplók
Ha a Log4J vagy a Logback használata (1.2-es verzió vagy 2.0-s) a nyomkövetés, akkor a nyomkövetési naplók automatikusan elküld a tooApplication Insights, ahol vizsgálatát, és keresse meg őket.

[További információ a diagnosztikai naplók][javalogs]

## <a name="custom-telemetry"></a>Egyéni telemetria
Helyezze be néhány sornyi kód a Java webes alkalmazás toofind ki a felhasználók tevékenységeit vele, vagy toohelp problémák diagnosztizálásához.

Kód weblapon JavaScript és hello kiszolgálóoldali Java beszúrásához.

[További tudnivalók az egyéni telemetria][track]

## <a name="next-steps"></a>Következő lépések
#### <a name="detect-and-diagnose-issues"></a>Észlelheti és diagnosztizálhatja a problémákat
* [Adja hozzá a webes ügyfél telemetriai] [ usage] tooget teljesítmény telemetriai hello webes ügyfél.
* [Webalkalmazás-tesztek beállítása] [ availability] toomake meg arról, hogy az alkalmazás élő és rugalmas marad.
* [Keresést az események és a naplók] [ diagnostic] toohelp problémák diagnosztizálásához.
* [Log4J vagy a Logback nyomkövetés rögzítése][javalogs]

#### <a name="track-usage"></a>Használat követése
* [Adja hozzá a webes ügyfél telemetriai] [ usage] toomonitor lapon nézetek és az alapszintű felhasználói metrikákat.
* [Egyéni események és metrikák nyomon](app-insights-web-track-usage.md) módjáról az alkalmazás, mind a hello ügyfélen és kiszolgálón hello toolearn.

<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[metrics]: app-insights-metrics-explorer.md
[track]: app-insights-api-custom-events-metrics.md
[usage]: app-insights-javascript.md
