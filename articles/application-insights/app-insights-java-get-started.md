---
title: "aaaJava web app analytics az Azure Application insights szolgáltatással |} Microsoft Docs"
description: "Alkalmazásteljesítmény-figyelés Java-webalkalmazásokhoz az Application Insights használatával. "
services: application-insights
documentationcenter: java
author: harelbr
manager: carmonm
ms.assetid: 051d4285-f38a-45d8-ad8a-45c3be828d91
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/14/2017
ms.author: bwren
ms.openlocfilehash: 6555ee53a44f937350e4fa296080f7dce4f45226
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-application-insights-in-a-java-web-project"></a>Ismerkedés az Application Insights szolgáltatással Java webes projektben


[Az Application Insights](https://azure.microsoft.com/services/application-insights/) van egy bővíthető elemzési szolgáltatás, a webfejlesztőknek, amely segít megérteni a hello teljesítmény- és az élő alkalmazás használati. Ezzel túl[észlelheti és diagnosztizálhatja a teljesítménybeli problémák és kivételek](app-insights-detect-triage-diagnose.md), és [írhat kódot] [ api] tootrack az alkalmazás a felhasználók milyen elvégezni.

![mintaadatok](./media/app-insights-java-get-started/5-results.png)

Az Application Insights a Linux, Unix vagy Windows rendszeren futó Java alkalmazásokat támogatja.

A következők szükségesek:

* Oracle JRE 1.6 vagy újabb, vagy Zulu JRE 1.6 vagy újabb
* Előfizetés túl[Microsoft Azure](https://azure.microsoft.com/).

*Ha a webes alkalmazás, amely már élő, követhető hello alternatív eljárás túl[hello SDK hozzáadása hello webkiszolgálón futásidőben](app-insights-java-live.md). Adott alternatív elkerülhető hello kód újraépítését, de hello beállítás toowrite kód tootrack felhasználói tevékenység nem jelenik.*

## <a name="1-get-an-application-insights-instrumentation-key"></a>1. Application Insights-kialakítási kulcs beszerzése
1. Jelentkezzen be toohello [Microsoft Azure-portálon](https://portal.azure.com).
2. Hozzon létre egy Application Insights-erőforrást. Állítsa be a hello alkalmazás típusú tooJava webes alkalmazást.

    ![Adjon meg egy nevet, válassza ki a Java webalkalmazást, és kattintson a Létrehozás gombra.](./media/app-insights-java-get-started/02-create.png)
3. Hello instrumentation kulcs hello új erőforrás található. Szüksége lesz toopaste ezt a kulcsot a kód projektben hamarosan.

    ![Hello új erőforrás-áttekintés kattintson a tulajdonságok és hello Instrumentation kulcs másolása](./media/app-insights-java-get-started/03-key.png)

## <a name="2-add-hello-application-insights-sdk-for-java-tooyour-project"></a>2. Application Insights SDK hello Java tooyour projekt hozzáadása
*Válassza ki a projekt hello megfelelő megoldást.*

#### <a name="if-youre-using-eclipse-toocreate-a-maven-or-dynamic-web-project-"></a>Ha használ Holdas Maven vagy dinamikus webes projekt toocreate...
Használjon hello [beépülő modul Javához készült Application Insights SDK][eclipse].

#### <a name="if-youre-using-maven"></a>Ha Mavent használ...
Ha a projekt toouse Maven build a már be van állítva, az egyesítési hello következő kódot tooyour pom.xml fájlt.

Ezt követően frissítési hello projekt függőségek tooget hello bináris fájljainak letöltése.

```XML

    <repositories>
       <repository>
          <id>central</id>
          <name>Central</name>
          <url>http://repo1.maven.org/maven2</url>
       </repository>
    </repositories>

    <dependencies>
      <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>applicationinsights-web</artifactId>
        <!-- or applicationinsights-core for bare API -->
        <version>[1.0,)</version>
      </dependency>
    </dependencies>
```

* *Build- vagy ellenőrzőösszeg-érvényesítési hibák?* Próbáljon egy adott verziót használni, például a következőt: `<version>1.0.n</version>`. Hello hello legújabb verziója található [SDK kibocsátási megjegyzéseket](https://github.com/Microsoft/ApplicationInsights-Java#release-notes) vagy a [Maven összetevők](http://search.maven.org/#search%7Cga%7C1%7Capplicationinsights).
* *Tooupdate tooa kell új SDK?* Frissítse a projekt függőségeit.

#### <a name="if-youre-using-gradle"></a>Ha Gradle-t használ...
A projekt buildjénél a gradle-lel toouse már be van állítva, ha egyesítése a következő kód tooyour build.gradle fájl hello.

Majd frissítési hello projekt függőségek tooget hello bináris fájljainak letöltése.

```JSON

    repositories {
      mavenCentral()
    }

    dependencies {
      compile group: 'com.microsoft.azure', name: 'applicationinsights-web', version: '1.+'
      // or applicationinsights-core for bare API
    }
```

* *Build- vagy ellenőrzőösszeg-érvényesítési hibák? Próbáljon adott verziót használni, például a következőt:* `version:'1.0.n'`. *Hello hello legújabb verziója található [SDK kibocsátási megjegyzéseket](https://github.com/Microsoft/ApplicationInsights-Java#release-notes).*
* *tooupdate tooa új SDK*
  * Frissítse a projekt függőségeit.

#### <a name="otherwise-"></a>Egyéb esetben...
Hello SDK manuális hozzáadása:

1. Töltse le a hello [Javához készült Application Insights SDK](https://aka.ms/aijavasdk).
2. Hello bináris kinyerése hello zip-fájl, és tooyour projektben adja hozzá.

### <a name="questions"></a>Kérdések...
* *Mi az az hello hello kapcsolatát `-core` és `-web` hello zip összetevőket?*

  * `applicationinsights-core`operációs rendszer API hello akkor biztosít. Erre az összetevőre mindig szüksége van.
  * `applicationinsights-web`– olyan mérőszámokat biztosít, amelyek nyomon követik a HTTP-kérések számát és a válaszidőket. Ezt az összetevőt kihagyhatja, ha nem szeretné automatikusan gyűjteni ezt a telemetriát. Ha például azt szeretné, toowrite saját.
* *tooupdate hello SDK jelenleg a változtatások közzétételekor*

  * Töltse le a hello legújabb [Javához készült Application Insights SDK](https://aka.ms/qqkaq6) és a régiek hello cseréje.
  * Hello ismerteti a módosítások [SDK kibocsátási megjegyzéseket](https://github.com/Microsoft/ApplicationInsights-Java#release-notes).

## <a name="3-add-an-application-insights-xml-file"></a>3. Application Insights .xml fájl hozzáadása
ApplicationInsights.xml toohello erőforrások mappa hozzáadása a projekthez a, vagy ellenőrizze, hogy tooyour projekt telepítési osztály útvonala kerül. Másolás hello XML következő bele.

Helyettesítő hello instrumentation kulcs portáltól kapott hello Azure-portálon.

```XML

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationInsights xmlns="http://schemas.microsoft.com/ApplicationInsights/2013/Settings" schemaVersion="2014-05-30">


      <!-- hello key from hello portal: -->

      <InstrumentationKey>** Your instrumentation key **</InstrumentationKey>


      <!-- HTTP request component (not required for bare API) -->

      <TelemetryModules>
        <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebRequestTrackingTelemetryModule"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebSessionTrackingTelemetryModule"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebUserTrackingTelemetryModule"/>
      </TelemetryModules>

      <!-- Events correlation (not required for bare API) -->
      <!-- These initializers add context data tooeach event -->

      <TelemetryInitializers>
        <Add   type="com.microsoft.applicationinsights.web.extensibility.initializers.WebOperationIdTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebOperationNameTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebSessionTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebUserTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebUserAgentTelemetryInitializer"/>

      </TelemetryInitializers>
    </ApplicationInsights>
```


* minden telemetriai tétel együtt küldött hello instrumentation kulcs, és közli az Application Insights toodisplay legyen az erőforrás.
* hello HTTP-kérelem összetevő nem kötelező megadni. Automatikusan elküldi a telemetriai kérelem és válasz alkalommal toohello portál.
* Korrelációs események egy hozzáadása toohello HTTP-kérelem összetevő. Hozzárendel egy azonosító tooeach kérelem hello kiszolgáló által fogadott, és felveszi ezt az azonosítót telemetriai adatot tulajdonság tooevery elemként hello tulajdonság "Operation.Id". Állítsa be a szűrőt minden kérelemhez társított toocorrelate hello telemetriai lehetővé teszi [diagnosztikai keresési][diagnostic].
* hello Application Insights kulcs dinamikusan hello az Azure-portálon argumentumként átadhatók a rendszer tulajdonság (-DAPPLICATION_INSIGHTS_IKEY = your_ikey). Ha nincs tulajdonság meghatározva, környezeti változót (APPLICATION_INSIGHTS_IKEY) keres az Azure App-beállításokban. Ha mindkét hello tulajdonság nincs megadva, hello alapértelmezett InstrumentationKey ApplicationInsights.xml használják. Ebben a sorozatban toomanage nyújt segítséget a különböző környezetekhez tartozó különböző InstrumentationKeys dinamikusan.

### <a name="alternative-ways-tooset-hello-instrumentation-key"></a>Alternatív módszereket tooset hello instrumentation kulcs
Application Insights SDK keresi hello kulcs az itt megadott sorrendben:

1. Rendszertulajdonság: -DAPPLICATION_INSIGHTS_IKEY=saját_kialakítási_kulcs
2. Környezeti változó: APPLICATION_INSIGHTS_IKEY
3. Konfigurációs fájl: ApplicationInsights.xml

[Beállíthatja a programkódban](app-insights-api-custom-events-metrics.md#ikey) is:

```Java

    telemetryClient.InstrumentationKey = "...";
```

## <a name="4-add-an-http-filter"></a>4. HTTP-szűrő hozzáadása
hello utolsó konfigurációs lépés lehetővé teszi, hogy hello HTTP kérelem összetevő toolog minden webes kérelem. (Nem kötelező, ha csak hello operációs rendszer API.)

Keresse meg, és nyissa meg a projekt, és a következő kód hello webalkalmazás csomópont alatt, ahol az alkalmazás szűrőit egyesítési hello hello web.xml fájlt.

hello szűrő tooget hello legpontosabb eredmények leképezéshez előtt az összes többi szűrőt.

```XML

    <filter>
      <filter-name>ApplicationInsightsWebFilter</filter-name>
      <filter-class>
        com.microsoft.applicationinsights.web.internal.WebRequestTrackingFilter
      </filter-class>
    </filter>
    <filter-mapping>
       <filter-name>ApplicationInsightsWebFilter</filter-name>
       <url-pattern>/*</url-pattern>
    </filter-mapping>
```

#### <a name="if-youre-using-spring-web-mvc-31-or-later"></a>Ha a Spring Web MVC 3.1-es vagy újabb verzióját használja
Ezeket az elemeket a Szerkesztés *-servlet.xml tooinclude hello Application Insights csomagot:

```XML

    <context:component-scan base-package=" com.springapp.mvc, com.microsoft.applicationinsights.web.spring"/>

    <mvc:interceptors>
        <mvc:interceptor>
            <mvc:mapping path="/**"/>
            <bean class="com.microsoft.applicationinsights.web.spring.RequestNameHandlerInterceptorAdapter" />
        </mvc:interceptor>
    </mvc:interceptors>
```

#### <a name="if-youre-using-struts-2"></a>Ha Struts 2-t használ
Adja hozzá a cikk toohello merevítőket vagy konfigurációs fájl (általában elnevezett struts.xml merevítőket-default.xml):

```XML

     <interceptors>
       <interceptor name="ApplicationInsightsRequestNameInterceptor" class="com.microsoft.applicationinsights.web.struts.RequestNameInterceptor" />
     </interceptors>
     <default-interceptor-ref name="ApplicationInsightsRequestNameInterceptor" />
```

(Ha van egy alapértelmezett verem definiált elfogókat, hello elfogó egyszerűen adhatók hozzá toothat verem.)

## <a name="5-run-your-application"></a>5. Az alkalmazás futtatása
A fejlesztői gépen hibakeresési módban futtatni, vagy tooyour-kiszolgáló közzététele.

## <a name="6-view-your-telemetry-in-application-insights"></a>6. A telemetria megtekintése az Application Insights szolgáltatásban
Térjen vissza az Application Insights-erőforrás tooyour [Microsoft Azure-portálon](https://portal.azure.com).

HTTP-kérelmek adatai hello áttekintése panelen jelenik meg. (Ha nincsenek ott, várjon néhány másodpercig, majd kattintson a Frissítés gombra.)

![mintaadatok](./media/app-insights-java-get-started/5-results.png)

[További információk a metrikákról.][metrics]

Kattintson a diagram toosee részletesebb keresztül mérőszámok összesített értéket.

![](./media/app-insights-java-get-started/6-barchart.png)

> Az Application Insights azt feltételezi, hogy hello MVC-alkalmazások HTTP-kérelmek formátuma: `VERB controller/action`. Például a `GET Home/Product/f9anuh81`, a `GET Home/Product/2dffwrf5` és a `GET Home/Product/sdf96vws` a következőbe van csoportosítva: `GET Home/Product`. Ez a csoportosítás lehetővé teszi a kérelmek fontos információkat biztosító összesítéseit, például a kérelmek számának és a kérelmek átlagos végrehajtási idejének meghatározását.
>
>

### <a name="instance-data"></a>Példányadatok
Kattintson egy adott kérelem toosee egyedi példányaira.

Két adattípus jelenik meg az Application Insights szolgáltatásban: összesített adatok, tárolva és átlagokként, számokként és összegekként megjelenítve; és példányadatok – a HTTP-kérelmek, kivételek, lapmegtekintések vagy egyéni események egyedi jelentései.

Egy kérelem hello tulajdonságainak megtekintésekor láthatja hello telemetriai események például a kérelmek és kivételek társítva.

![](./media/app-insights-java-get-started/7-instance.png)

### <a name="analytics-powerful-query-language"></a>Elemzés: Erőteljes lekérdezési nyelv
Több adat gyűlik össze, mert mindkét tooaggregate adatok és toofind egyes példányok lekérdezéseket is futtathat.  Az [elemzés](app-insights-analytics.md) erőteljes eszköz a teljesítmény és a használat megértéséhez és diagnosztikai célokra is.

![Példa elemzésre](./media/app-insights-java-get-started/025.png)

## <a name="7-install-your-app-on-hello-server"></a>7. Az alkalmazás telepítése hello kiszolgálón
Most már a toohello alkalmazáskiszolgálóra közzétételéhez a felhasználók használni, és tekintse meg a hello telemetriai hello portálon jelenik meg.

* Győződjön meg arról, hogy a tűzfala lehetővé teszi, hogy az alkalmazás toosend telemetriai toothese portok:

  * dc.services.visualstudio.com:443
  * f5.services.visualstudio.com:443

* Ha a kimenő forgalmat át kell irányítani egy tűzfalon, adja meg a `http.proxyHost` és a `http.proxyPort` rendszertulajdonságot.

* Windows-kiszolgálókon telepítse a következőt:

  * [Microsoft Visual C++ újraterjeszthető csomag](http://www.microsoft.com/download/details.aspx?id=40784)

    (Ez az összetevő lehetővé teszi a teljesítményszámlálókat.)


## <a name="exceptions-and-request-failures"></a>Kivételek és kérelemhibák
A rendszer a nem kezelt kivételeket automatikusan begyűjti:

![Beállítások megnyitása, Hibák](./media/app-insights-java-get-started/21-exceptions.png)

más kivételek toocollect adatok, két lehetősége van:

* [INSERT tootrackException() meghívja a kódban][apiexceptions].
* [Hello Java-ügynök telepítése a kiszolgálón](app-insights-java-agent.md). Megadhatja a kívánt toowatch hello módszerek.

## <a name="monitor-method-calls-and-external-dependencies"></a>Metódushívások és külső függőségek megfigyelése
[Hello Java-ügynök telepítése](app-insights-java-agent.md) toolog megadott belső módszerek és adatokkal időzítése keresztül JDBC, felé indított hívások.

## <a name="performance-counters"></a>Teljesítményszámlálók
Nyissa meg **beállítások**, **kiszolgálók**, toosee teljesítményszámlálók számos.

![](./media/app-insights-java-get-started/11-perf-counters.png)

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

![](./media/app-insights-java-get-started/12-custom-perfs.png)

### <a name="unix-performance-counters"></a>Unix-teljesítményszámlálók
* [Hello Application Insights beépülő modul telepítése collectd](app-insights-java-collectd.md) tooget széles körének a rendszer és a hálózati adatokat.

## <a name="get-user-and-session-data"></a>Felhasználói és munkamenetadatok lekérése
Telemetriát küld a webkiszolgálóról. Most tooget hello 360 fok nézet az alkalmazás, hozzáadhat további figyelési:

* [Adja hozzá a telemetriai adatok tooyour weblapok] [ usage] toomonitor nézetek és a felhasználó metrikák lapon.
* [Webalkalmazás-tesztek beállítása] [ availability] toomake meg arról, hogy az alkalmazás élő és rugalmas marad.

## <a name="capture-log-traces"></a>Naplónyomkövetések rögzítése
Az Application Insights tooslice használ, és ezután Log4J, Logback vagy más naplózási keretrendszer naplók. Hello naplók kivizsgált HTTP-kérelmek és egyéb telemetriai adatokat. [Itt megismerkedhet az erre vonatkozó részletekkel][javalogs].

## <a name="send-your-own-telemetry"></a>Saját telemetria küldése
Most, hogy hello SDK telepítése, használhatja a saját telemetriai hello API toosend.

* [Egyéni események és metrikák nyomon] [ api] toolearn teljesítenek a felhasználók számára az alkalmazással.
* [Keresést az események és a naplók] [ diagnostic] toohelp problémák diagnosztizálásához.

## <a name="availability-web-tests"></a>Rendelkezésre állási webes tesztek
Az Application Insights tesztelheti, hogy működik-e rendszeres időközönként toocheck és is válaszol a webhelyen. [másolatot tooset][availability], kattintson a webes tesztjeinek használatát.

![Kattintson a Webes tesztek elemre, majd a Webes teszt hozzáadása elemre.](./media/app-insights-java-get-started/31-config-web-test.png)

Megkapja a válaszidők diagramjait, valamint e-mailes értesítéseket kap, ha a webhely leáll.

![Példa webes tesztre](./media/app-insights-java-get-started/appinsights-10webtestresult.png)

[További információk a rendelkezésre állási webes tesztekről.][availability]

## <a name="questions-problems"></a>Kérdései vannak? Problémákat tapasztal?
[A Java hibaelhárítása](app-insights-java-troubleshoot.md)

## <a name="video"></a>Videó

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a>Következő lépések
* [Függőségi hívások figyelése](app-insights-java-agent.md)
* [Unix-teljesítményszámlálók figyelése](app-insights-java-collectd.md)
* Adja hozzá [tooyour weblapok figyelési](app-insights-javascript.md) toomonitor betöltési időkorlátja, AJAX-hívások, böngésző kivételek.
* Írási [egyéni telemetria](app-insights-api-custom-events-metrics.md) tootrack használati hello böngészőben vagy hello kiszolgálón.
* Hozzon létre [irányítópultok](app-insights-dashboards.md) toobring együtt hello kulcs diagramok esetében a rendszer figyelése.
* Az [Elemzések](app-insights-analytics.md) használatával hatékony telemetriai lekérdezéseket hajthat végre az alkalmazásból
* További információ: [Azure Java-fejlesztőknek](/java/azure).

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apiexceptions]: app-insights-api-custom-events-metrics.md#trackexception
[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[eclipse]: app-insights-java-eclipse.md
[javalogs]: app-insights-java-trace-logs.md
[metrics]: app-insights-metrics-explorer.md
[usage]: app-insights-javascript.md
