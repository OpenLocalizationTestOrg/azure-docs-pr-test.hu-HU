---
title: "Nyomkövetési naplók az Azure Application Insights Java felfedezése |} Microsoft Docs"
description: "Keresési Log4J vagy a Logback nyomkövetések az Application Insightsban"
services: application-insights
documentationcenter: java
author: mrbullwinkle
manager: carmonm
ms.assetid: fc0a9e2f-3beb-4f47-a9fe-3f86cd29d97a
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 12/12/2016
ms.author: mbullwin
ms.openlocfilehash: 6e441c9cbd15bb1528ea8e8a781f90900af90cf2
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 11/01/2017
---
# <a name="explore-java-trace-logs-in-application-insights"></a>Az Application Insights nyomkövetési naplók Java felfedezés
Ha a Log4J vagy a Logback használata (1.2-es verzió vagy 2.0-s) a nyomkövetés, akkor a nyomkövetési naplók megtekintése az Application Insights ahol vizsgálatát, és keresse meg őket automatikusan elküldi.

## <a name="install-the-java-sdk"></a>Telepítse a Java SDK

Telepítés [Javához készült Application Insights SDK][java], ha még nem tette meg, amely.

(Ha nem kívánja nyomon követni a HTTP-kérelmekre, akkor kihagyhatja a .xml konfigurációs fájlt a legtöbb, de legalább tartalmaznia kell a `InstrumentationKey` elemet. Is `new TelemetryClient()` inicializálni az SDK-t.)


## <a name="add-logging-libraries-to-your-project"></a>Naplózási kódtárak hozzáadása a projekthez
*Válassza ki a projektnek megfelelő módszert.*

#### <a name="if-youre-using-maven"></a>Ha Mavent használ...
A projekt már be van állítva Maven build használandó, ha egyesítése egy kódot az alábbi kódtöredékek a pom.xml fájlt.

Ezt követően frissítse a Projektfüggőségek lekérni a bináris fájlok letöltése.

*Logback*

```XML

    <dependencies>
       <dependency>
          <groupId>com.microsoft.azure</groupId>
          <artifactId>applicationinsights-logging-logback</artifactId>
          <version>[1.0,)</version>
       </dependency>
    </dependencies>
```

*Log4J 2.0-s verzió*

```XML

    <dependencies>
       <dependency>
          <groupId>com.microsoft.azure</groupId>
          <artifactId>applicationinsights-logging-log4j2</artifactId>
          <version>[1.0,)</version>
       </dependency>
    </dependencies>
```

*Log4J 1.2-es verzió*

```XML

    <dependencies>
       <dependency>
          <groupId>com.microsoft.azure</groupId>
          <artifactId>applicationinsights-logging-log4j1_2</artifactId>
          <version>[1.0,)</version>
       </dependency>
    </dependencies>
```

#### <a name="if-youre-using-gradle"></a>Ha Gradle-t használ...
Ha a projekt már be van állítva Gradle használandó build, vegye fel, a következő sorok valamelyikét a `dependencies` csoportot a build.gradle fájlban:

Ezt követően frissítse a Projektfüggőségek lekérni a bináris fájlok letöltése.

**Logback**

```

    compile group: 'com.microsoft.azure', name: 'applicationinsights-logging-logback', version: '1.0.+'
```

**Log4J 2.0-s verzió**

```
    compile group: 'com.microsoft.azure', name: 'applicationinsights-logging-log4j2', version: '1.0.+'
```

**Log4J 1.2-es verzió**

```
    compile group: 'com.microsoft.azure', name: 'applicationinsights-logging-log4j1_2', version: '1.0.+'
```

#### <a name="otherwise-"></a>Egyéb esetben...
Töltse le, majd bontsa ki a megfelelő naplóírót a megfelelő könyvtár hozzáadása a projekthez:

| Naplózó | Letöltés | Részletes ismertetés |
| --- | --- | --- |
| Logback |[SDK-val Logback naplóírói](https://aka.ms/xt62a4) |applicationinsights-naplózás-logback |
| Log4J 2.0-s verzió |[SDK-val Log4J v2 naplóírói](https://aka.ms/qypznq) |applicationinsights-naplózás-log4j2 |
| Log4j 1.2-es verzió |[SDK-val Log4J 1.2-es verzió naplóírói](https://aka.ms/ky9cbo) |applicationinsights-naplózás-log4j1_2 |

## <a name="add-the-appender-to-your-logging-framework"></a>A naplóíró hozzáadása a naplózási keretrendszer
Nyomok első elindításához a Log4J vagy a Logback konfigurációs fájl a megfelelő kódrészletét egyesítése: 

*Logback*

```XML

    <appender name="aiAppender" 
      class="com.microsoft.applicationinsights.logback.ApplicationInsightsAppender">
    </appender>
    <root level="trace">
      <appender-ref ref="aiAppender" />
    </root>
```

*Log4J 2.0-s verzió*

```XML

    <Configuration packages="com.microsoft.applicationinsights.Log4j">
      <Appenders>
        <ApplicationInsightsAppender name="aiAppender" />
      </Appenders>
      <Loggers>
        <Root level="trace">
          <AppenderRef ref="aiAppender"/>
        </Root>
      </Loggers>
    </Configuration>
```

*Log4J 1.2-es verzió*

```XML

    <appender name="aiAppender" 
         class="com.microsoft.applicationinsights.log4j.v1_2.ApplicationInsightsAppender">
    </appender>
    <root>
      <priority value ="trace" />
      <appender-ref ref="aiAppender" />
    </root>
```

Az Application Insights appenders hivatkozható, azonban bármely konfigurált naplózó, és nem feltétlenül a legfelső szintű naplózó (ahogy a fenti kódpéldák).

## <a name="explore-your-traces-in-the-application-insights-portal"></a>Megismerkedhet a nyomkövetések az Application Insights-portálon
Most, hogy konfigurálta az Application Insights nyomkövetési adatokat küldeni a projekthez, tekintheti meg és keressen a nyomkövetések az Application Insights-portálon a [keresési] [ diagnostic] panelen.

![Az Application Insights portál nyissa meg a keresési](./media/app-insights-java-trace-logs/10-diagnostics.png)

## <a name="next-steps"></a>Következő lépések
[Diagnosztikai keresése][diagnostic]

<!--Link references-->

[diagnostic]: app-insights-diagnostic-search.md
[java]: app-insights-java-get-started.md


