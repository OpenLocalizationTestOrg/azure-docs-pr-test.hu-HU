---
title: "SCOM-integráció az Application insights szolgáltatással |} Microsoft Docs"
description: "Ha az SCOM-felhasználó, teljesítmény figyeléséhez és eseményadatokat az Application insights szolgáltatással. Átfogó irányítópultok, intelligens riasztások, hatékony diagnosztikai eszközöket és elemzési lekérdezések."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 606e9d03-c0e6-4a77-80e8-61b75efacde0
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 08/12/2016
ms.author: bwren
ms.openlocfilehash: 9c205465981fabdbb696cdc44f765532bbb992b5
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="application-performance-monitoring-using-application-insights-for-scom"></a><span data-ttu-id="d2b36-104">Alkalmazásteljesítmény-figyelés az Application Insights az SCOM-hoz használatával</span><span class="sxs-lookup"><span data-stu-id="d2b36-104">Application Performance Monitoring using Application Insights for SCOM</span></span>
<span data-ttu-id="d2b36-105">Ha a System Center Operations Manager (SCOM) használatával kezeli a kiszolgálók, teljesítmény figyeléséhez, és segítségével. a teljesítménnyel kapcsolatos problémák diagnosztizálásához [Azure Application Insights](app-insights-asp-net.md).</span><span class="sxs-lookup"><span data-stu-id="d2b36-105">If you use System Center Operations Manager (SCOM) to manage your servers, you can monitor performance and diagnose performance issues with the help of [Azure Application Insights](app-insights-asp-net.md).</span></span> <span data-ttu-id="d2b36-106">Az Application Insights figyeli a webes alkalmazás bejövő kérelmet, REST és SQL-hívások, kivételek és naplókivonatokat kimenő.</span><span class="sxs-lookup"><span data-stu-id="d2b36-106">Application Insights monitors your web application's incoming requests, outgoing REST and SQL calls, exceptions, and log traces.</span></span> <span data-ttu-id="d2b36-107">Nyújtja a irányítópultok metrika diagramok és intelligens riasztások, valamint hatékony diagnosztikai keresési és elemzési lekérdezéseket a telemetriai keresztül.</span><span class="sxs-lookup"><span data-stu-id="d2b36-107">It provides dashboards with metric charts and smart alerts, as well as powerful diagnostic search and analytical queries over this telemetry.</span></span> 

<span data-ttu-id="d2b36-108">Az Application Insights általi figyelés az SCOM felügyeleti csomagok használatával válthat.</span><span class="sxs-lookup"><span data-stu-id="d2b36-108">You can switch on Application Insights monitoring by using an SCOM management pack.</span></span>

## <a name="before-you-start"></a><span data-ttu-id="d2b36-109">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="d2b36-109">Before you start</span></span>
<span data-ttu-id="d2b36-110">Feltételezzük, hogy:</span><span class="sxs-lookup"><span data-stu-id="d2b36-110">We assume:</span></span>

* <span data-ttu-id="d2b36-111">Megismerheti az SCOM, és hogy használatával SCOM 2012 R2 vagy 2016 kezeli az IIS webkiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="d2b36-111">You're familiar with SCOM, and that you use SCOM 2012 R2 or 2016 to manage your IIS web servers.</span></span>
* <span data-ttu-id="d2b36-112">Már telepítette a kiszolgálókon, amelyek az Application insights szolgáltatással figyelni kívánt webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="d2b36-112">You have already installed on your servers a web application that you want to monitor with Application Insights.</span></span>
* <span data-ttu-id="d2b36-113">Az alkalmazás-keretrendszer verziója a .NET 4.5-ös vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="d2b36-113">App framework version is .NET 4.5 or later.</span></span>
* <span data-ttu-id="d2b36-114">Rendelkezik hozzáféréssel az előfizetés [Microsoft Azure](https://azure.com) és való bejelentkezés a [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="d2b36-114">You have access to a subscription in [Microsoft Azure](https://azure.com) and can sign in to the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="d2b36-115">A szervezet előfordulhat, hogy rendelkezik előfizetéssel, és adhat hozzá a Microsoft-fiókjához azt.</span><span class="sxs-lookup"><span data-stu-id="d2b36-115">Your organization may have a subscription, and can add your Microsoft account to it.</span></span>

<span data-ttu-id="d2b36-116">(A fejlesztői csapat létrehozhat a [Application Insights SDK](app-insights-asp-net.md) a webes alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="d2b36-116">(The development team might build the [Application Insights SDK](app-insights-asp-net.md) into the web app.</span></span> <span data-ttu-id="d2b36-117">A felépítés során instrumentation egyéni telemetria írásban nagyobb rugalmasságot biztosít.</span><span class="sxs-lookup"><span data-stu-id="d2b36-117">This build-time instrumentation gives them greater flexibility in writing custom telemetry.</span></span> <span data-ttu-id="d2b36-118">Azonban nem számít: az SDK-t a beépített nélkül akár az itt leírt lépéseket követheti.)</span><span class="sxs-lookup"><span data-stu-id="d2b36-118">However, it doesn't matter: you can follow the steps described here either with or without the SDK built in.)</span></span>

## <a name="one-time-install-application-insights-management-pack"></a><span data-ttu-id="d2b36-119">(Egy alkalommal) Az Application Insights felügyeleti csomag telepítéséhez</span><span class="sxs-lookup"><span data-stu-id="d2b36-119">(One time) Install Application Insights management pack</span></span>
<span data-ttu-id="d2b36-120">A számítógépen, amelyen az Operations Manager futtatja:</span><span class="sxs-lookup"><span data-stu-id="d2b36-120">On the machine where you run Operations Manager:</span></span>

1. <span data-ttu-id="d2b36-121">Távolítsa el a régi verziót a felügyeleti csomag:</span><span class="sxs-lookup"><span data-stu-id="d2b36-121">Uninstall any old version of the management pack:</span></span>
   1. <span data-ttu-id="d2b36-122">Az Operations Manager programban nyissa meg a felügyeleti, a felügyeleti csomagokat.</span><span class="sxs-lookup"><span data-stu-id="d2b36-122">In Operations Manager, open Administration, Management Packs.</span></span> 
   2. <span data-ttu-id="d2b36-123">Törölje a régi verzióját.</span><span class="sxs-lookup"><span data-stu-id="d2b36-123">Delete the old version.</span></span>
2. <span data-ttu-id="d2b36-124">Töltse le és telepítse a felügyeleti csomag a katalógusban.</span><span class="sxs-lookup"><span data-stu-id="d2b36-124">Download and install the management pack from the catalog.</span></span>
3. <span data-ttu-id="d2b36-125">Indítsa újra az Operations Manager.</span><span class="sxs-lookup"><span data-stu-id="d2b36-125">Restart Operations Manager.</span></span>

## <a name="create-a-management-pack"></a><span data-ttu-id="d2b36-126">Felügyeleti csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="d2b36-126">Create a management pack</span></span>
1. <span data-ttu-id="d2b36-127">Az Operations Manager programban nyissa meg a **szerzői műveletek**, **.NET... az Application insights szolgáltatással**, **figyelés felvétele varázsló**, és újra válasszon **... az Application insights szolgáltatással .NET**.</span><span class="sxs-lookup"><span data-stu-id="d2b36-127">In Operations Manager, open **Authoring**, **.NET...with Application Insights**, **Add Monitoring Wizard**, and again choose **.NET...with Application Insights**.</span></span>
   
    ![](./media/app-insights-scom/020.png)
2. <span data-ttu-id="d2b36-128">A konfiguráció után az alkalmazás nevét.</span><span class="sxs-lookup"><span data-stu-id="d2b36-128">Name the configuration after your app.</span></span> <span data-ttu-id="d2b36-129">(Hogy egyetlen alkalmazás állíthatnak be egyszerre.)</span><span class="sxs-lookup"><span data-stu-id="d2b36-129">(You have to instrument one app at a time.)</span></span>
   
    ![](./media/app-insights-scom/030.png)
3. <span data-ttu-id="d2b36-130">Ugyanazon az oldalon varázsló vagy hozzon létre egy új felügyeleti csomagot, vagy válassza ki az Application Insights korábban létrehozott csomagot.</span><span class="sxs-lookup"><span data-stu-id="d2b36-130">On the same wizard page, either create a new management pack, or select a pack that you created for Application Insights earlier.</span></span>
   
     <span data-ttu-id="d2b36-131">(Az Application Insights [felügyeleti csomag](https://technet.microsoft.com/library/cc974491.aspx) egy sablon, amelyből példányt létrehozni.</span><span class="sxs-lookup"><span data-stu-id="d2b36-131">(The Application Insights [management pack](https://technet.microsoft.com/library/cc974491.aspx) is a template, from which you create an instance.</span></span> <span data-ttu-id="d2b36-132">Többször is felhasználhatja ugyanazon később.)</span><span class="sxs-lookup"><span data-stu-id="d2b36-132">You can reuse the same instance later.)</span></span>

    ![Az általános tulajdonságok lapján írja be az alkalmazás nevét.](./media/app-insights-scom/040.png)

1. <span data-ttu-id="d2b36-136">Válasszon egy alkalmazást, amely segítségével nyomon követni kívánt.</span><span class="sxs-lookup"><span data-stu-id="d2b36-136">Choose one app that you want to monitor.</span></span> <span data-ttu-id="d2b36-137">A keresési funkciót keres a kiszolgálókon telepített alkalmazások között.</span><span class="sxs-lookup"><span data-stu-id="d2b36-137">The search feature searches among apps installed on your servers.</span></span>
   
    ![Mit figyeljen lapon kattintson a Hozzáadás gombra, írja be az alkalmazás nevére részét, kattintson a Keresés gombra, válassza ki az alkalmazást, és hozzáadása, az OK gombra.](./media/app-insights-scom/050.png)
   
    <span data-ttu-id="d2b36-139">A figyelés hatóköre nem kötelező mező segítségével egy részét a kiszolgálóit, ha nem kívánja az összes kiszolgálót az alkalmazás figyelése.</span><span class="sxs-lookup"><span data-stu-id="d2b36-139">The optional Monitoring scope field can be used to specify a subset of your servers, if you don't want to monitor the app in all servers.</span></span>
2. <span data-ttu-id="d2b36-140">A varázsló következő lapján meg kell adnia a Microsoft Azure bejelentkezési adatait.</span><span class="sxs-lookup"><span data-stu-id="d2b36-140">On the next wizard page, you must first provide your credentials to sign in to Microsoft Azure.</span></span>
   
    <span data-ttu-id="d2b36-141">Ezen a lapon válassza ki az Application Insights-erőforrást, ha azt szeretné, hogy a telemetriai adatok elemzése és jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="d2b36-141">On this page, you choose the Application Insights resource where you want the telemetry data to be analyzed and displayed.</span></span> 
   
   * <span data-ttu-id="d2b36-142">Ha az alkalmazás fejlesztése során az Application Insights lett konfigurálva, válassza ki a meglévő erőforrást.</span><span class="sxs-lookup"><span data-stu-id="d2b36-142">If the application was configured for Application Insights during development, select its existing resource.</span></span>
   * <span data-ttu-id="d2b36-143">Máskülönben hozzon létre egy új erőforrást az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="d2b36-143">Otherwise, create a new resource named for the app.</span></span> <span data-ttu-id="d2b36-144">Ha más alkalmazásokat, amelyek az adott rendszer összetevők, helyezze el őket ugyanabban az erőforráscsoportban, könnyebben hozzáférés a telemetriai adatok kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="d2b36-144">If there are other apps that are components of the same system, put them in the same resource group, to make access to the telemetry easier to manage.</span></span>
     
     <span data-ttu-id="d2b36-145">Ezeket a beállításokat később bármikor módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="d2b36-145">You can change these settings later.</span></span>
     
     ![Az Application Insights-beállítások lapján kattintson a "bejelentkezés", és adja meg a Microsoft-fiók hitelesítő adatait az Azure-bA.](./media/app-insights-scom/060.png)
3. <span data-ttu-id="d2b36-148">A varázsló befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="d2b36-148">Complete the wizard.</span></span>
   
    ![Kattintson a Létrehozás gombra](./media/app-insights-scom/070.png)

<span data-ttu-id="d2b36-150">Ismételje meg az eljárást minden, amely segítségével nyomon követni kívánt alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="d2b36-150">Repeat this procedure for each app that you want to monitor.</span></span>

<span data-ttu-id="d2b36-151">Ha beállításait később módosítani szeretné, nyissa meg ismét a tulajdonságok a figyelő a szerzői műveletek ablakból.</span><span class="sxs-lookup"><span data-stu-id="d2b36-151">If you need to change settings later, re-open the properties of the monitor from the Authoring window.</span></span>

![A szerzői műveletek, válassza ki a .NET alkalmazásteljesítmény-figyelés az Application insights szolgáltatással, válassza ki a figyelő, és kattintson a Tulajdonságok elemre.](./media/app-insights-scom/080.png)

## <a name="verify-monitoring"></a><span data-ttu-id="d2b36-153">Ellenőrizze a figyelés</span><span class="sxs-lookup"><span data-stu-id="d2b36-153">Verify monitoring</span></span>
<span data-ttu-id="d2b36-154">A figyelő, hogy telepítette minden kiszolgálón megkeresi az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="d2b36-154">The monitor that you have installed searches for your app on every server.</span></span> <span data-ttu-id="d2b36-155">Ha úgy találja, hogy az alkalmazást, és azt konfigurálja az Application Insights Állapotmonitort a figyelheti az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="d2b36-155">Where it finds the app, it configures Application Insights Status Monitor to monitor the app.</span></span> <span data-ttu-id="d2b36-156">Szükség esetén azt először telepíti Állapotmonitort a kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="d2b36-156">If necessary, it first installs Status Monitor on the server.</span></span>

<span data-ttu-id="d2b36-157">Talált alkalmazáspéldányok, mely ellenőrizheti:</span><span class="sxs-lookup"><span data-stu-id="d2b36-157">You can verify which instances of the app it has found:</span></span>

![A figyelés, nyissa meg az Application insights szolgáltatással](./media/app-insights-scom/100.png)

## <a name="view-telemetry-in-application-insights"></a><span data-ttu-id="d2b36-159">Az Application Insightsban nézet telemetria</span><span class="sxs-lookup"><span data-stu-id="d2b36-159">View telemetry in Application Insights</span></span>
<span data-ttu-id="d2b36-160">Az a [Azure-portálon](https://portal.azure.com), keresse meg az alkalmazás erőforrás.</span><span class="sxs-lookup"><span data-stu-id="d2b36-160">In the [Azure portal](https://portal.azure.com), browse to the resource for your app.</span></span> <span data-ttu-id="d2b36-161">Ön [telemetriai ábrázoló diagramok megjelenítéséhez](app-insights-dashboards.md) az alkalmazásból.</span><span class="sxs-lookup"><span data-stu-id="d2b36-161">You [see charts showing telemetry](app-insights-dashboards.md) from your app.</span></span> <span data-ttu-id="d2b36-162">(Ha ez nem látható fő lapján még, kattintson a metrikák élő adatfolyam.)</span><span class="sxs-lookup"><span data-stu-id="d2b36-162">(If it hasn't shown up on the main page yet, click Live Metrics Stream.)</span></span>

## <a name="next-steps"></a><span data-ttu-id="d2b36-163">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d2b36-163">Next steps</span></span>
* <span data-ttu-id="d2b36-164">[Állítson be egy irányítópultot](app-insights-dashboards.md) kell összefogni a legfontosabb diagramok ezzel és más alkalmazások figyelését.</span><span class="sxs-lookup"><span data-stu-id="d2b36-164">[Set up a dashboard](app-insights-dashboards.md) to bring together the most important charts monitoring this and other apps.</span></span>
* [<span data-ttu-id="d2b36-165">További tudnivalók a metrikák</span><span class="sxs-lookup"><span data-stu-id="d2b36-165">Learn about metrics</span></span>](app-insights-metrics-explorer.md)
* [<span data-ttu-id="d2b36-166">Riasztások beállítása</span><span class="sxs-lookup"><span data-stu-id="d2b36-166">Set up alerts</span></span>](app-insights-alerts.md)
* [<span data-ttu-id="d2b36-167">Teljesítménnyel kapcsolatos problémák diagnosztizálása</span><span class="sxs-lookup"><span data-stu-id="d2b36-167">Diagnosing performance issues</span></span>](app-insights-detect-triage-diagnose.md)
* [<span data-ttu-id="d2b36-168">Hatékony elemzési lekérdezések</span><span class="sxs-lookup"><span data-stu-id="d2b36-168">Powerful Analytics queries</span></span>](app-insights-analytics.md)
* [<span data-ttu-id="d2b36-169">A webteszt rendelkezésre állása</span><span class="sxs-lookup"><span data-stu-id="d2b36-169">Availability web tests</span></span>](app-insights-monitor-web-app-availability.md)

