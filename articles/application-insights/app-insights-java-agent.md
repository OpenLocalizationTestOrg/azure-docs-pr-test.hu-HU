---
title: "Java-webalkalmazások Azure Application Insights az alkalmazásteljesítmény-figyelés |} Microsoft Docs"
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
ms.openlocfilehash: 4e56998382610ad3d7224e6a8de5aee5419ebe43
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="monitor-dependencies-exceptions-and-execution-times-in-java-web-apps"></a><span data-ttu-id="dd1ef-103">Függőségek, kivételeket és végrehajtásának lassúságát a Java-webalkalmazások figyelése</span><span class="sxs-lookup"><span data-stu-id="dd1ef-103">Monitor dependencies, exceptions and execution times in Java web apps</span></span>


<span data-ttu-id="dd1ef-104">Ha rendelkezik [a Java-webalkalmazás az Application insights szolgáltatással tagolva][java], a Java-ügynök részleteinek megtekintésével mélyebb betekintést kód módosítások nélkül használható:</span><span class="sxs-lookup"><span data-stu-id="dd1ef-104">If you have [instrumented your Java web app with Application Insights][java], you can use the Java Agent to get deeper insights, without any code changes:</span></span>

* <span data-ttu-id="dd1ef-105">**Függőségek:** hívások, az alkalmazás által az egyéb összetevők, beleértve a vonatkozó adatokat:</span><span class="sxs-lookup"><span data-stu-id="dd1ef-105">**Dependencies:** Data about calls that your application makes to other components, including:</span></span>
  * <span data-ttu-id="dd1ef-106">**REST-hívások** HttpClient OkHttp és RestTemplate (forrás) keresztül történik.</span><span class="sxs-lookup"><span data-stu-id="dd1ef-106">**REST calls** made via HttpClient, OkHttp, and RestTemplate (Spring).</span></span>
  * <span data-ttu-id="dd1ef-107">**Redis** keresztül a Jedis ügyfél felé indított hívások.</span><span class="sxs-lookup"><span data-stu-id="dd1ef-107">**Redis** calls made via the Jedis client.</span></span> <span data-ttu-id="dd1ef-108">A hívás tovább tart, mint 10 egység, ha az ügynök is beolvassa a hívás argumentumokkal.</span><span class="sxs-lookup"><span data-stu-id="dd1ef-108">If the call takes longer than 10s, the agent also fetches the call arguments.</span></span>
  * <span data-ttu-id="dd1ef-109">**[JDBC-hívások](http://docs.oracle.com/javase/7/docs/technotes/guides/jdbc/)**  -MySQL, SQL Server, PostgreSQL, SQLite, Oracle DB vagy Apache Derby DB.</span><span class="sxs-lookup"><span data-stu-id="dd1ef-109">**[JDBC calls](http://docs.oracle.com/javase/7/docs/technotes/guides/jdbc/)** - MySQL, SQL Server, PostgreSQL, SQLite, Oracle DB or Apache Derby DB.</span></span> <span data-ttu-id="dd1ef-110">"executeBatch" hívások támogatottak.</span><span class="sxs-lookup"><span data-stu-id="dd1ef-110">"executeBatch" calls are supported.</span></span> <span data-ttu-id="dd1ef-111">A MySQL és PostgreSQL, ha a hívás tovább tart, mint 10 egység, az ügynök jelentéseket küld a lekérdezéstervben.</span><span class="sxs-lookup"><span data-stu-id="dd1ef-111">For MySQL and PostgreSQL, if the call takes longer than 10s, the agent reports the query plan.</span></span>
* <span data-ttu-id="dd1ef-112">**Kivétel lépett fel:** a kód által kezelt kivételek adatait.</span><span class="sxs-lookup"><span data-stu-id="dd1ef-112">**Caught exceptions:** Data about exceptions that are handled by your code.</span></span>
* <span data-ttu-id="dd1ef-113">**Módszer végrehajtási ideje:** bizonyos eljárások végrehajtásához szükséges idő az adatait.</span><span class="sxs-lookup"><span data-stu-id="dd1ef-113">**Method execution time:** Data about the time it takes to execute specific methods.</span></span>

<span data-ttu-id="dd1ef-114">A Java-ügynök használatára, akkor a kiszolgálóra telepítette.</span><span class="sxs-lookup"><span data-stu-id="dd1ef-114">To use the Java agent, you install it on your server.</span></span> <span data-ttu-id="dd1ef-115">A webalkalmazások kell tagolva, és a [Application Insights Java SDK][java].</span><span class="sxs-lookup"><span data-stu-id="dd1ef-115">Your web apps must be instrumented with the [Application Insights Java SDK][java].</span></span> 

## <a name="install-the-application-insights-agent-for-java"></a><span data-ttu-id="dd1ef-116">A Javához készült Application Insights-ügynök telepítése</span><span class="sxs-lookup"><span data-stu-id="dd1ef-116">Install the Application Insights agent for Java</span></span>
1. <span data-ttu-id="dd1ef-117">A számítógépen a Java Servert futtató [töltse le az ügynököt](https://aka.ms/aijavasdk).</span><span class="sxs-lookup"><span data-stu-id="dd1ef-117">On the machine running your Java server, [download the agent](https://aka.ms/aijavasdk).</span></span>
2. <span data-ttu-id="dd1ef-118">Az alkalmazás server indítási parancsfájl szerkesztése, és adja hozzá az alábbi JVM-et:</span><span class="sxs-lookup"><span data-stu-id="dd1ef-118">Edit the application server startup script, and add the following JVM:</span></span>
   
    <span data-ttu-id="dd1ef-119">`javaagent:`*az ügynök JAR-fájl teljes elérési útja*</span><span class="sxs-lookup"><span data-stu-id="dd1ef-119">`javaagent:`*full path to the agent JAR file*</span></span>
   
    <span data-ttu-id="dd1ef-120">Például a Linux rendszerű gépen Tomcat:</span><span class="sxs-lookup"><span data-stu-id="dd1ef-120">For example, in Tomcat on a Linux machine:</span></span>
   
    `export JAVA_OPTS="$JAVA_OPTS -javaagent:<full path to agent JAR file>"`
3. <span data-ttu-id="dd1ef-121">Indítsa újra az alkalmazáskiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="dd1ef-121">Restart your application server.</span></span>

## <a name="configure-the-agent"></a><span data-ttu-id="dd1ef-122">Az ügynök konfigurálása</span><span class="sxs-lookup"><span data-stu-id="dd1ef-122">Configure the agent</span></span>
<span data-ttu-id="dd1ef-123">Hozzon létre egy fájlt `AI-Agent.xml` és naplózza azt a mappában, amelyben az ügynök JAR-fájlra.</span><span class="sxs-lookup"><span data-stu-id="dd1ef-123">Create a file named `AI-Agent.xml` and place it in the same folder as the agent JAR file.</span></span>

<span data-ttu-id="dd1ef-124">Állítsa be az XML-fájl tartalmát.</span><span class="sxs-lookup"><span data-stu-id="dd1ef-124">Set the content of the xml file.</span></span> <span data-ttu-id="dd1ef-125">Szerkessze a következő példa a azt szeretné, vagy hagyja el a szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="dd1ef-125">Edit the following example to include or omit the features you want.</span></span>

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

           <!-- Report on the particular signature
                void methodTwo(String, int) -->
           <Method name="methodTwo"
              reportExecutionTime="true"
              signature="(Ljava/lang/String;I)V" />
        </Class>

      </Instrumentation>
    </ApplicationInsightsAgent>

```

<span data-ttu-id="dd1ef-126">Kell engedélyezni a jelentések kivétel és az egyes módszerek metódus ütemezését.</span><span class="sxs-lookup"><span data-stu-id="dd1ef-126">You have to enable reports exception and method timing for individual methods.</span></span>

<span data-ttu-id="dd1ef-127">Alapértelmezés szerint `reportExecutionTime` IGAZ és `reportCaughtExceptions` értéke "false".</span><span class="sxs-lookup"><span data-stu-id="dd1ef-127">By default, `reportExecutionTime` is true and `reportCaughtExceptions` is false.</span></span>

## <a name="view-the-data"></a><span data-ttu-id="dd1ef-128">Az adatok megjelenítése</span><span class="sxs-lookup"><span data-stu-id="dd1ef-128">View the data</span></span>
<span data-ttu-id="dd1ef-129">Összesített távoli függőség és metódus végrehajtásának lassúságát jelenik meg az Application Insights-erőforrás [alatt a teljesítmény csempéje][metrics].</span><span class="sxs-lookup"><span data-stu-id="dd1ef-129">In the Application Insights resource, aggregated remote dependency and method execution times appears [under the Performance tile][metrics].</span></span>

<span data-ttu-id="dd1ef-130">Nyissa meg a keresendő függőségi kivétel és metódus jelentések egyes példányai, [keresési][diagnostic].</span><span class="sxs-lookup"><span data-stu-id="dd1ef-130">To search for individual instances of dependency, exception, and method reports, open [Search][diagnostic].</span></span>

<span data-ttu-id="dd1ef-131">[Diagnosztizálás függőségi problémákhoz – további](app-insights-asp-net-dependencies.md#diagnosis).</span><span class="sxs-lookup"><span data-stu-id="dd1ef-131">[Diagnosing dependency issues - learn more](app-insights-asp-net-dependencies.md#diagnosis).</span></span>

## <a name="questions-problems"></a><span data-ttu-id="dd1ef-132">Kérdései vannak?</span><span class="sxs-lookup"><span data-stu-id="dd1ef-132">Questions?</span></span> <span data-ttu-id="dd1ef-133">Problémákat tapasztal?</span><span class="sxs-lookup"><span data-stu-id="dd1ef-133">Problems?</span></span>
* <span data-ttu-id="dd1ef-134">Nincs adat?</span><span class="sxs-lookup"><span data-stu-id="dd1ef-134">No data?</span></span> [<span data-ttu-id="dd1ef-135">Tűzfalkivételek beállítása</span><span class="sxs-lookup"><span data-stu-id="dd1ef-135">Set firewall exceptions</span></span>](app-insights-ip-addresses.md)
* [<span data-ttu-id="dd1ef-136">A Java hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="dd1ef-136">Troubleshooting Java</span></span>](app-insights-java-troubleshoot.md)

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apiexceptions]: app-insights-api-custom-events-metrics.md#track-exception
[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[eclipse]: app-insights-java-eclipse.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[metrics]: app-insights-metrics-explorer.md
