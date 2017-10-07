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
# <a name="monitor-dependencies-exceptions-and-execution-times-in-java-web-apps"></a><span data-ttu-id="3f4c0-103">Függőségek, kivételeket és végrehajtásának lassúságát a Java-webalkalmazások figyelése</span><span class="sxs-lookup"><span data-stu-id="3f4c0-103">Monitor dependencies, exceptions and execution times in Java web apps</span></span>


<span data-ttu-id="3f4c0-104">Ha rendelkezik [a Java-webalkalmazás az Application insights szolgáltatással tagolva][java], hello Java ügynök tooget részleteinek megtekintésével mélyebb betekintést, kód módosítások nélkül használható:</span><span class="sxs-lookup"><span data-stu-id="3f4c0-104">If you have [instrumented your Java web app with Application Insights][java], you can use hello Java Agent tooget deeper insights, without any code changes:</span></span>

* <span data-ttu-id="3f4c0-105">**Függőségek:** hívások, amely az alkalmazás tooother összetevők, beleértve a vonatkozó adatokat:</span><span class="sxs-lookup"><span data-stu-id="3f4c0-105">**Dependencies:** Data about calls that your application makes tooother components, including:</span></span>
  * <span data-ttu-id="3f4c0-106">**REST-hívások** HttpClient OkHttp és RestTemplate (forrás) keresztül történik.</span><span class="sxs-lookup"><span data-stu-id="3f4c0-106">**REST calls** made via HttpClient, OkHttp, and RestTemplate (Spring).</span></span>
  * <span data-ttu-id="3f4c0-107">**Redis** hello Jedis ügyfél keresztül felé indított hívások.</span><span class="sxs-lookup"><span data-stu-id="3f4c0-107">**Redis** calls made via hello Jedis client.</span></span> <span data-ttu-id="3f4c0-108">Ha hello hívás 10 egység hosszabb időbe telik, hello ügynök is beolvassa hello hívás argumentumokat.</span><span class="sxs-lookup"><span data-stu-id="3f4c0-108">If hello call takes longer than 10s, hello agent also fetches hello call arguments.</span></span>
  * <span data-ttu-id="3f4c0-109">**[JDBC-hívások](http://docs.oracle.com/javase/7/docs/technotes/guides/jdbc/)**  -MySQL, SQL Server, PostgreSQL, SQLite, Oracle DB vagy Apache Derby DB.</span><span class="sxs-lookup"><span data-stu-id="3f4c0-109">**[JDBC calls](http://docs.oracle.com/javase/7/docs/technotes/guides/jdbc/)** - MySQL, SQL Server, PostgreSQL, SQLite, Oracle DB or Apache Derby DB.</span></span> <span data-ttu-id="3f4c0-110">"executeBatch" hívások támogatottak.</span><span class="sxs-lookup"><span data-stu-id="3f4c0-110">"executeBatch" calls are supported.</span></span> <span data-ttu-id="3f4c0-111">A MySQL és PostgreSQL, ha hello hívás tovább tart, mint 10 egység, hello ügynök jelentéseket küld hello lekérdezéstervben.</span><span class="sxs-lookup"><span data-stu-id="3f4c0-111">For MySQL and PostgreSQL, if hello call takes longer than 10s, hello agent reports hello query plan.</span></span>
* <span data-ttu-id="3f4c0-112">**Kivétel lépett fel:** a kód által kezelt kivételek adatait.</span><span class="sxs-lookup"><span data-stu-id="3f4c0-112">**Caught exceptions:** Data about exceptions that are handled by your code.</span></span>
* <span data-ttu-id="3f4c0-113">**Módszer végrehajtási ideje:** hello adatait alkalommal vesz tooexecute megadott metódusok.</span><span class="sxs-lookup"><span data-stu-id="3f4c0-113">**Method execution time:** Data about hello time it takes tooexecute specific methods.</span></span>

<span data-ttu-id="3f4c0-114">toouse hello Java ügynök, az a kiszolgálóra telepítette.</span><span class="sxs-lookup"><span data-stu-id="3f4c0-114">toouse hello Java agent, you install it on your server.</span></span> <span data-ttu-id="3f4c0-115">A webalkalmazások kell lesznek tagolva a hello [Application Insights Java SDK][java].</span><span class="sxs-lookup"><span data-stu-id="3f4c0-115">Your web apps must be instrumented with hello [Application Insights Java SDK][java].</span></span> 

## <a name="install-hello-application-insights-agent-for-java"></a><span data-ttu-id="3f4c0-116">Java hello Application Insights-ügynök telepítése</span><span class="sxs-lookup"><span data-stu-id="3f4c0-116">Install hello Application Insights agent for Java</span></span>
1. <span data-ttu-id="3f4c0-117">Hello gépen futó Java kiszolgálóját [hello-ügynök letöltése](https://aka.ms/aijavasdk).</span><span class="sxs-lookup"><span data-stu-id="3f4c0-117">On hello machine running your Java server, [download hello agent](https://aka.ms/aijavasdk).</span></span>
2. <span data-ttu-id="3f4c0-118">Hello application server indítási parancsfájl szerkesztése, és adja hozzá a következő JVM hello:</span><span class="sxs-lookup"><span data-stu-id="3f4c0-118">Edit hello application server startup script, and add hello following JVM:</span></span>
   
    <span data-ttu-id="3f4c0-119">`javaagent:`*teljes elérési útja toohello ügynök JAR-fájlra*</span><span class="sxs-lookup"><span data-stu-id="3f4c0-119">`javaagent:`*full path toohello agent JAR file*</span></span>
   
    <span data-ttu-id="3f4c0-120">Például a Linux rendszerű gépen Tomcat:</span><span class="sxs-lookup"><span data-stu-id="3f4c0-120">For example, in Tomcat on a Linux machine:</span></span>
   
    `export JAVA_OPTS="$JAVA_OPTS -javaagent:<full path tooagent JAR file>"`
3. <span data-ttu-id="3f4c0-121">Indítsa újra az alkalmazáskiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="3f4c0-121">Restart your application server.</span></span>

## <a name="configure-hello-agent"></a><span data-ttu-id="3f4c0-122">Hello ügynök konfigurálása</span><span class="sxs-lookup"><span data-stu-id="3f4c0-122">Configure hello agent</span></span>
<span data-ttu-id="3f4c0-123">Hozzon létre egy fájlt `AI-Agent.xml` és helyezheti el hello hello ügynök JAR fájllal megegyező mappában.</span><span class="sxs-lookup"><span data-stu-id="3f4c0-123">Create a file named `AI-Agent.xml` and place it in hello same folder as hello agent JAR file.</span></span>

<span data-ttu-id="3f4c0-124">Hello XML-fájl tartalmának hello beállítása.</span><span class="sxs-lookup"><span data-stu-id="3f4c0-124">Set hello content of hello xml file.</span></span> <span data-ttu-id="3f4c0-125">Szerkessze a következő példa tooinclude hello, vagy hagyja ki a kívánt hello szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="3f4c0-125">Edit hello following example tooinclude or omit hello features you want.</span></span>

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

<span data-ttu-id="3f4c0-126">Tooenable jelentések kivétel és az egyes módszerek metódus időzítési rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="3f4c0-126">You have tooenable reports exception and method timing for individual methods.</span></span>

<span data-ttu-id="3f4c0-127">Alapértelmezés szerint `reportExecutionTime` IGAZ és `reportCaughtExceptions` értéke "false".</span><span class="sxs-lookup"><span data-stu-id="3f4c0-127">By default, `reportExecutionTime` is true and `reportCaughtExceptions` is false.</span></span>

## <a name="view-hello-data"></a><span data-ttu-id="3f4c0-128">Hello adatok megtekintése</span><span class="sxs-lookup"><span data-stu-id="3f4c0-128">View hello data</span></span>
<span data-ttu-id="3f4c0-129">Összesített távoli függőség és metódus végrehajtásának lassúságát jelenik meg hello Application Insights-erőforrást, [alatt hello teljesítmény csempéje][metrics].</span><span class="sxs-lookup"><span data-stu-id="3f4c0-129">In hello Application Insights resource, aggregated remote dependency and method execution times appears [under hello Performance tile][metrics].</span></span>

<span data-ttu-id="3f4c0-130">Nyissa meg a függőségi kivétel és metódus jelentések egyes példányai toosearch [keresési][diagnostic].</span><span class="sxs-lookup"><span data-stu-id="3f4c0-130">toosearch for individual instances of dependency, exception, and method reports, open [Search][diagnostic].</span></span>

<span data-ttu-id="3f4c0-131">[Diagnosztizálás függőségi problémákhoz – további](app-insights-asp-net-dependencies.md#diagnosis).</span><span class="sxs-lookup"><span data-stu-id="3f4c0-131">[Diagnosing dependency issues - learn more](app-insights-asp-net-dependencies.md#diagnosis).</span></span>

## <a name="questions-problems"></a><span data-ttu-id="3f4c0-132">Kérdései vannak?</span><span class="sxs-lookup"><span data-stu-id="3f4c0-132">Questions?</span></span> <span data-ttu-id="3f4c0-133">Problémákat tapasztal?</span><span class="sxs-lookup"><span data-stu-id="3f4c0-133">Problems?</span></span>
* <span data-ttu-id="3f4c0-134">Nincs adat?</span><span class="sxs-lookup"><span data-stu-id="3f4c0-134">No data?</span></span> [<span data-ttu-id="3f4c0-135">Tűzfalkivételek beállítása</span><span class="sxs-lookup"><span data-stu-id="3f4c0-135">Set firewall exceptions</span></span>](app-insights-ip-addresses.md)
* [<span data-ttu-id="3f4c0-136">A Java hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="3f4c0-136">Troubleshooting Java</span></span>](app-insights-java-troubleshoot.md)

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apiexceptions]: app-insights-api-custom-events-metrics.md#track-exception
[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[eclipse]: app-insights-java-eclipse.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[metrics]: app-insights-metrics-explorer.md
