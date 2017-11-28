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
# <a name="explore-java-trace-logs-in-application-insights"></a><span data-ttu-id="ca990-103">Az Application Insights nyomkövetési naplók Java felfedezés</span><span class="sxs-lookup"><span data-stu-id="ca990-103">Explore Java trace logs in Application Insights</span></span>
<span data-ttu-id="ca990-104">Ha a Log4J vagy a Logback használata (1.2-es verzió vagy 2.0-s) a nyomkövetés, akkor a nyomkövetési naplók automatikusan elküld a tooApplication Insights, ahol vizsgálatát, és keresse meg őket.</span><span class="sxs-lookup"><span data-stu-id="ca990-104">If you're using Logback or Log4J (v1.2 or v2.0) for tracing, you can have your trace logs sent automatically tooApplication Insights where you can explore and search on them.</span></span>

## <a name="install-hello-java-sdk"></a><span data-ttu-id="ca990-105">Hello Java SDK telepítése</span><span class="sxs-lookup"><span data-stu-id="ca990-105">Install hello Java SDK</span></span>

<span data-ttu-id="ca990-106">Telepítés [Javához készült Application Insights SDK][java], ha még nem tette meg, amely.</span><span class="sxs-lookup"><span data-stu-id="ca990-106">Install [Application Insights SDK for Java][java], if you haven't already done that.</span></span>

<span data-ttu-id="ca990-107">(Ha nem szeretné, hogy tootrack HTTP-kérelmek, akkor kihagyhatja a legtöbb hello .xml konfigurációs fájlt, de legalább tartalmaznia kell a hello `InstrumentationKey` elemet.</span><span class="sxs-lookup"><span data-stu-id="ca990-107">(If you don't want tootrack HTTP requests, you can omit most of hello .xml configuration file, but you must at least include hello `InstrumentationKey` element.</span></span> <span data-ttu-id="ca990-108">Is `new TelemetryClient()` tooinitialize hello SDK.)</span><span class="sxs-lookup"><span data-stu-id="ca990-108">You should also call `new TelemetryClient()` tooinitialize hello SDK.)</span></span>


## <a name="add-logging-libraries-tooyour-project"></a><span data-ttu-id="ca990-109">Naplózási szalagtárak tooyour projekt hozzáadása</span><span class="sxs-lookup"><span data-stu-id="ca990-109">Add logging libraries tooyour project</span></span>
<span data-ttu-id="ca990-110">*Válassza ki a projekt hello megfelelő megoldást.*</span><span class="sxs-lookup"><span data-stu-id="ca990-110">*Choose hello appropriate way for your project.*</span></span>

#### <a name="if-youre-using-maven"></a><span data-ttu-id="ca990-111">Ha Mavent használ...</span><span class="sxs-lookup"><span data-stu-id="ca990-111">If you're using Maven...</span></span>
<span data-ttu-id="ca990-112">A projekt már be van állítva toouse Maven build a, ha egyesítési hello kódtöredékek kód a pom.xml fájlt a következő egyikét.</span><span class="sxs-lookup"><span data-stu-id="ca990-112">If your project is already set up toouse Maven for build, merge one of hello following snippets of code into your pom.xml file.</span></span>

<span data-ttu-id="ca990-113">Ezt követően frissítse hello projektfüggőségek, tooget hello bináris fájljainak letöltése.</span><span class="sxs-lookup"><span data-stu-id="ca990-113">Then refresh hello project dependencies, tooget hello binaries downloaded.</span></span>

<span data-ttu-id="ca990-114">*Logback*</span><span class="sxs-lookup"><span data-stu-id="ca990-114">*Logback*</span></span>

```XML

    <dependencies>
       <dependency>
          <groupId>com.microsoft.azure</groupId>
          <artifactId>applicationinsights-logging-logback</artifactId>
          <version>[1.0,)</version>
       </dependency>
    </dependencies>
```

<span data-ttu-id="ca990-115">*Log4J 2.0-s verzió*</span><span class="sxs-lookup"><span data-stu-id="ca990-115">*Log4J v2.0*</span></span>

```XML

    <dependencies>
       <dependency>
          <groupId>com.microsoft.azure</groupId>
          <artifactId>applicationinsights-logging-log4j2</artifactId>
          <version>[1.0,)</version>
       </dependency>
    </dependencies>
```

<span data-ttu-id="ca990-116">*Log4J 1.2-es verzió*</span><span class="sxs-lookup"><span data-stu-id="ca990-116">*Log4J v1.2*</span></span>

```XML

    <dependencies>
       <dependency>
          <groupId>com.microsoft.azure</groupId>
          <artifactId>applicationinsights-logging-log4j1_2</artifactId>
          <version>[1.0,)</version>
       </dependency>
    </dependencies>
```

#### <a name="if-youre-using-gradle"></a><span data-ttu-id="ca990-117">Ha Gradle-t használ...</span><span class="sxs-lookup"><span data-stu-id="ca990-117">If you're using Gradle...</span></span>
<span data-ttu-id="ca990-118">Ha a projekt buildjénél a gradle-lel toouse már be van állítva, a következő sorokat toohello hello egyikét hozzá `dependencies` csoportot a build.gradle fájlban:</span><span class="sxs-lookup"><span data-stu-id="ca990-118">If your project is already set up toouse Gradle for build, add one of hello following lines toohello `dependencies` group in your build.gradle file:</span></span>

<span data-ttu-id="ca990-119">Ezt követően frissítse hello projektfüggőségek, tooget hello bináris fájljainak letöltése.</span><span class="sxs-lookup"><span data-stu-id="ca990-119">Then refresh hello project dependencies, tooget hello binaries downloaded.</span></span>

<span data-ttu-id="ca990-120">**Logback**</span><span class="sxs-lookup"><span data-stu-id="ca990-120">**Logback**</span></span>

```

    compile group: 'com.microsoft.azure', name: 'applicationinsights-logging-logback', version: '1.0.+'
```

<span data-ttu-id="ca990-121">**Log4J 2.0-s verzió**</span><span class="sxs-lookup"><span data-stu-id="ca990-121">**Log4J v2.0**</span></span>

```
    compile group: 'com.microsoft.azure', name: 'applicationinsights-logging-log4j2', version: '1.0.+'
```

<span data-ttu-id="ca990-122">**Log4J 1.2-es verzió**</span><span class="sxs-lookup"><span data-stu-id="ca990-122">**Log4J v1.2**</span></span>

```
    compile group: 'com.microsoft.azure', name: 'applicationinsights-logging-log4j1_2', version: '1.0.+'
```

#### <a name="otherwise-"></a><span data-ttu-id="ca990-123">Egyéb esetben...</span><span class="sxs-lookup"><span data-stu-id="ca990-123">Otherwise ...</span></span>
<span data-ttu-id="ca990-124">Töltse le, majd bontsa ki a megfelelő naplóírói hello hello megfelelő könyvtárat tooyour projekt hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="ca990-124">Download and extract hello appropriate appender, then add hello appropriate library tooyour project:</span></span>

| <span data-ttu-id="ca990-125">Naplózó</span><span class="sxs-lookup"><span data-stu-id="ca990-125">Logger</span></span> | <span data-ttu-id="ca990-126">Letöltés</span><span class="sxs-lookup"><span data-stu-id="ca990-126">Download</span></span> | <span data-ttu-id="ca990-127">Részletes ismertetés</span><span class="sxs-lookup"><span data-stu-id="ca990-127">Library</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ca990-128">Logback</span><span class="sxs-lookup"><span data-stu-id="ca990-128">Logback</span></span> |[<span data-ttu-id="ca990-129">SDK-val Logback naplóírói</span><span class="sxs-lookup"><span data-stu-id="ca990-129">SDK with Logback appender</span></span>](https://aka.ms/xt62a4) |<span data-ttu-id="ca990-130">applicationinsights-naplózás-logback</span><span class="sxs-lookup"><span data-stu-id="ca990-130">applicationinsights-logging-logback</span></span> |
| <span data-ttu-id="ca990-131">Log4J 2.0-s verzió</span><span class="sxs-lookup"><span data-stu-id="ca990-131">Log4J v2.0</span></span> |[<span data-ttu-id="ca990-132">SDK-val Log4J v2 naplóírói</span><span class="sxs-lookup"><span data-stu-id="ca990-132">SDK with Log4J v2 appender</span></span>](https://aka.ms/qypznq) |<span data-ttu-id="ca990-133">applicationinsights-naplózás-log4j2</span><span class="sxs-lookup"><span data-stu-id="ca990-133">applicationinsights-logging-log4j2</span></span> |
| <span data-ttu-id="ca990-134">Log4j 1.2-es verzió</span><span class="sxs-lookup"><span data-stu-id="ca990-134">Log4j v1.2</span></span> |[<span data-ttu-id="ca990-135">SDK-val Log4J 1.2-es verzió naplóírói</span><span class="sxs-lookup"><span data-stu-id="ca990-135">SDK with Log4J v1.2 appender</span></span>](https://aka.ms/ky9cbo) |<span data-ttu-id="ca990-136">applicationinsights-naplózás-log4j1_2</span><span class="sxs-lookup"><span data-stu-id="ca990-136">applicationinsights-logging-log4j1_2</span></span> |

## <a name="add-hello-appender-tooyour-logging-framework"></a><span data-ttu-id="ca990-137">Hello naplóírói tooyour naplózási keretrendszer hozzáadása</span><span class="sxs-lookup"><span data-stu-id="ca990-137">Add hello appender tooyour logging framework</span></span>
<span data-ttu-id="ca990-138">toostart első nyomkövetések, egyesítési hello megfelelő kódrészletét kód toohello Log4J vagy a Logback konfigurációs fájlt:</span><span class="sxs-lookup"><span data-stu-id="ca990-138">toostart getting traces, merge hello relevant snippet of code toohello Log4J or Logback configuration file:</span></span> 

<span data-ttu-id="ca990-139">*Logback*</span><span class="sxs-lookup"><span data-stu-id="ca990-139">*Logback*</span></span>

```XML

    <appender name="aiAppender" 
      class="com.microsoft.applicationinsights.logback.ApplicationInsightsAppender">
    </appender>
    <root level="trace">
      <appender-ref ref="aiAppender" />
    </root>
```

<span data-ttu-id="ca990-140">*Log4J 2.0-s verzió*</span><span class="sxs-lookup"><span data-stu-id="ca990-140">*Log4J v2.0*</span></span>

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

<span data-ttu-id="ca990-141">*Log4J 1.2-es verzió*</span><span class="sxs-lookup"><span data-stu-id="ca990-141">*Log4J v1.2*</span></span>

```XML

    <appender name="aiAppender" 
         class="com.microsoft.applicationinsights.log4j.v1_2.ApplicationInsightsAppender">
    </appender>
    <root>
      <priority value ="trace" />
      <appender-ref ref="aiAppender" />
    </root>
```

<span data-ttu-id="ca990-142">(a fenti hello mintakódok) hello Application Insights appenders bármely konfigurált naplózó, és nem feltétlenül hello legfelső szintű naplózó lehet hivatkozni.</span><span class="sxs-lookup"><span data-stu-id="ca990-142">hello Application Insights appenders can be referenced by any configured logger, and not necessarily by hello root logger (as shown in hello code samples above).</span></span>

## <a name="explore-your-traces-in-hello-application-insights-portal"></a><span data-ttu-id="ca990-143">A nyomkövetések az Application Insights portál hello felfedezés</span><span class="sxs-lookup"><span data-stu-id="ca990-143">Explore your traces in hello Application Insights portal</span></span>
<span data-ttu-id="ca990-144">Most, hogy konfigurálta a projekt toosend hívásláncainak tooApplication Insights, tekintheti meg és keressen a nyomkövetések hello Application Insights portálon, a hello [keresési] [ diagnostic] panelen.</span><span class="sxs-lookup"><span data-stu-id="ca990-144">Now that you've configured your project toosend traces tooApplication Insights, you can view and search these traces in hello Application Insights portal, in hello [Search][diagnostic] blade.</span></span>

![Az Application Insights portál hello nyissa meg a keresési](./media/app-insights-java-trace-logs/10-diagnostics.png)

## <a name="next-steps"></a><span data-ttu-id="ca990-146">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ca990-146">Next steps</span></span>
<span data-ttu-id="ca990-147">[Diagnosztikai keresése][diagnostic]</span><span class="sxs-lookup"><span data-stu-id="ca990-147">[Diagnostic search][diagnostic]</span></span>

<!--Link references-->

[diagnostic]: app-insights-diagnostic-search.md
[java]: app-insights-java-get-started.md


