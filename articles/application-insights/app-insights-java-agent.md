---
title: "Java-webalkalmazások Azure Application insightsban figyelésének aaaPerformance |} Microsoft Docs"
description: "Kiterjesztett teljesítmény és a Java-webhely, az Application Insights-használat figyelését."
services: application-insights
documentationcenter: java
author: harelbr
manager: carmonm
ms.assetid: 84017a48-1cb3-40c8-aab1-ff68d65e2128
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 08/24/2016
ms.author: bwren
ms.openlocfilehash: bf3983e3b4a16e72bc606b6468a757288d05ebaa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-dependencies-exceptions-and-execution-times-in-java-web-apps"></a>Függőségek, kivételeket és végrehajtásának lassúságát a Java-webalkalmazások figyelése


Ha rendelkezik [a Java-webalkalmazás az Application insights szolgáltatással tagolva][java], hello Java ügynök tooget részleteinek megtekintésével mélyebb betekintést, kód módosítások nélkül használható:

* **Függőségek:** hívások, amely az alkalmazás tooother összetevők, beleértve a vonatkozó adatokat:
  * **REST-hívások** HttpClient OkHttp és RestTemplate (forrás) keresztül történik.
  * **Redis** hello Jedis ügyfél keresztül felé indított hívások. Ha hello hívás 10 egység hosszabb időbe telik, hello ügynök is beolvassa hello hívás argumentumokat.
  * **[JDBC-hívások](http://docs.oracle.com/javase/7/docs/technotes/guides/jdbc/)**  -MySQL, SQL Server, PostgreSQL, SQLite, Oracle DB vagy Apache Derby DB. "executeBatch" hívások támogatottak. A MySQL és PostgreSQL, ha hello hívás tovább tart, mint 10 egység, hello ügynök jelentéseket küld hello lekérdezéstervben.
* **Kivétel lépett fel:** a kód által kezelt kivételek adatait.
* **Módszer végrehajtási ideje:** hello adatait alkalommal vesz tooexecute megadott metódusok.

toouse hello Java ügynök, az a kiszolgálóra telepítette. A webalkalmazások kell lesznek tagolva a hello [Application Insights Java SDK][java]. 

## <a name="install-hello-application-insights-agent-for-java"></a>Java hello Application Insights-ügynök telepítése
1. Hello gépen futó Java kiszolgálóját [hello-ügynök letöltése](https://aka.ms/aijavasdk).
2. Hello application server indítási parancsfájl szerkesztése, és adja hozzá a következő JVM hello:
   
    `javaagent:`*teljes elérési útja toohello ügynök JAR-fájlra*
   
    Például a Linux rendszerű gépen Tomcat:
   
    `export JAVA_OPTS="$JAVA_OPTS -javaagent:<full path tooagent JAR file>"`
3. Indítsa újra az alkalmazáskiszolgáló.

## <a name="configure-hello-agent"></a>Hello ügynök konfigurálása
Hozzon létre egy fájlt `AI-Agent.xml` és helyezheti el hello hello ügynök JAR fájllal megegyező mappában.

Hello XML-fájl tartalmának hello beállítása. Szerkessze a következő példa tooinclude hello, vagy hagyja ki a kívánt hello szolgáltatásokat.

```XML

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationInsightsAgent>
      <Instrumentation>

        <!-- Collect remote dependency data -->
        <BuiltIn enabled="true">
           <!-- Disable Redis or alter threshold call duration above which arguments are sent.
               Defaults: enabled, 10000 ms -->
           <Jedis enabled="true" thresholdInMS="1000"/>

           <!-- Set SQL query duration above which query plan is reported (MySQL, PostgreSQL). Default is 10000 ms. -->
           <MaxStatementQueryLimitInMS>1000</MaxStatementQueryLimitInMS>
        </BuiltIn>

        <!-- Collect data about caught exceptions
             and method execution times -->

        <Class name="com.myCompany.MyClass">
           <Method name="methodOne"
               reportCaughtExceptions="true"
               reportExecutionTime="true"
               />

           <!-- Report on hello particular signature
                void methodTwo(String, int) -->
           <Method name="methodTwo"
              reportExecutionTime="true"
              signature="(Ljava/lang/String;I)V" />
        </Class>

      </Instrumentation>
    </ApplicationInsightsAgent>

```

Tooenable jelentések kivétel és az egyes módszerek metódus időzítési rendelkezik.

Alapértelmezés szerint `reportExecutionTime` IGAZ és `reportCaughtExceptions` értéke "false".

## <a name="view-hello-data"></a>Hello adatok megtekintése
Összesített távoli függőség és metódus végrehajtásának lassúságát jelenik meg hello Application Insights-erőforrást, [alatt hello teljesítmény csempéje][metrics].

Nyissa meg a függőségi kivétel és metódus jelentések egyes példányai toosearch [keresési][diagnostic].

[Diagnosztizálás függőségi problémákhoz – további](app-insights-asp-net-dependencies.md#diagnosis).

## <a name="questions-problems"></a>Kérdései vannak? Problémákat tapasztal?
* Nincs adat? [Tűzfalkivételek beállítása](app-insights-ip-addresses.md)
* [A Java hibaelhárítása](app-insights-java-troubleshoot.md)

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apiexceptions]: app-insights-api-custom-events-metrics.md#track-exception
[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[eclipse]: app-insights-java-eclipse.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[metrics]: app-insights-metrics-explorer.md
