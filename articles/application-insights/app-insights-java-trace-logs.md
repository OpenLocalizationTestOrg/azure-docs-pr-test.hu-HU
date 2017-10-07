---
title: "Java-nyomkövetési aaaExplore bejelentkezik Azure Application Insights |} Microsoft Docs"
description: "Keresési Log4J vagy a Logback nyomkövetések az Application Insightsban"
services: application-insights
documentationcenter: java
author: CFreemanwa
manager: carmonm
ms.assetid: fc0a9e2f-3beb-4f47-a9fe-3f86cd29d97a
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 12/12/2016
ms.author: bwren
ms.openlocfilehash: e5f8e8c67e57753ba7574b97aa96dbb41db00ce1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="explore-java-trace-logs-in-application-insights"></a>Az Application Insights nyomkövetési naplók Java felfedezés
Ha a Log4J vagy a Logback használata (1.2-es verzió vagy 2.0-s) a nyomkövetés, akkor a nyomkövetési naplók automatikusan elküld a tooApplication Insights, ahol vizsgálatát, és keresse meg őket.

## <a name="install-hello-java-sdk"></a>Hello Java SDK telepítése

Telepítés [Javához készült Application Insights SDK][java], ha még nem tette meg, amely.

(Ha nem szeretné, hogy tootrack HTTP-kérelmek, akkor kihagyhatja a legtöbb hello .xml konfigurációs fájlt, de legalább tartalmaznia kell a hello `InstrumentationKey` elemet. Is `new TelemetryClient()` tooinitialize hello SDK.)


## <a name="add-logging-libraries-tooyour-project"></a>Naplózási szalagtárak tooyour projekt hozzáadása
*Válassza ki a projekt hello megfelelő megoldást.*

#### <a name="if-youre-using-maven"></a>Ha Mavent használ...
A projekt már be van állítva toouse Maven build a, ha egyesítési hello kódtöredékek kód a pom.xml fájlt a következő egyikét.

Ezt követően frissítse hello projektfüggőségek, tooget hello bináris fájljainak letöltése.

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
Ha a projekt buildjénél a gradle-lel toouse már be van állítva, a következő sorokat toohello hello egyikét hozzá `dependencies` csoportot a build.gradle fájlban:

Ezt követően frissítse hello projektfüggőségek, tooget hello bináris fájljainak letöltése.

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
Töltse le, majd bontsa ki a megfelelő naplóírói hello hello megfelelő könyvtárat tooyour projekt hozzáadása:

| Naplózó | Letöltés | Részletes ismertetés |
| --- | --- | --- |
| Logback |[SDK-val Logback naplóírói](https://aka.ms/xt62a4) |applicationinsights-naplózás-logback |
| Log4J 2.0-s verzió |[SDK-val Log4J v2 naplóírói](https://aka.ms/qypznq) |applicationinsights-naplózás-log4j2 |
| Log4j 1.2-es verzió |[SDK-val Log4J 1.2-es verzió naplóírói](https://aka.ms/ky9cbo) |applicationinsights-naplózás-log4j1_2 |

## <a name="add-hello-appender-tooyour-logging-framework"></a>Hello naplóírói tooyour naplózási keretrendszer hozzáadása
toostart első nyomkövetések, egyesítési hello megfelelő kódrészletét kód toohello Log4J vagy a Logback konfigurációs fájlt: 

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

(a fenti hello mintakódok) hello Application Insights appenders bármely konfigurált naplózó, és nem feltétlenül hello legfelső szintű naplózó lehet hivatkozni.

## <a name="explore-your-traces-in-hello-application-insights-portal"></a>A nyomkövetések az Application Insights portál hello felfedezés
Most, hogy konfigurálta a projekt toosend hívásláncainak tooApplication Insights, tekintheti meg és keressen a nyomkövetések hello Application Insights portálon, a hello [keresési] [ diagnostic] panelen.

![Az Application Insights portál hello nyissa meg a keresési](./media/app-insights-java-trace-logs/10-diagnostics.png)

## <a name="next-steps"></a>Következő lépések
[Diagnosztikai keresése][diagnostic]

<!--Link references-->

[diagnostic]: app-insights-diagnostic-search.md
[java]: app-insights-java-get-started.md


