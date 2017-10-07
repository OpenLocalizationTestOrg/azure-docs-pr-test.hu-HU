---
title: "az Application Insights aaaSCOM integrálása |} Microsoft Docs"
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
ms.openlocfilehash: ee9ad462610fd916098a0e292c5bd44f2a873c10
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="application-performance-monitoring-using-application-insights-for-scom"></a><span data-ttu-id="c86d1-104">Alkalmazásteljesítmény-figyelés az Application Insights az SCOM-hoz használatával</span><span class="sxs-lookup"><span data-stu-id="c86d1-104">Application Performance Monitoring using Application Insights for SCOM</span></span>
<span data-ttu-id="c86d1-105">Ha a System Center Operations Manager (SCOM) toomanage a kiszolgálók, teljesítmény figyeléséhez és hello segítségével a teljesítménnyel kapcsolatos problémák diagnosztizálásához [Azure Application Insights](app-insights-asp-net.md).</span><span class="sxs-lookup"><span data-stu-id="c86d1-105">If you use System Center Operations Manager (SCOM) toomanage your servers, you can monitor performance and diagnose performance issues with hello help of [Azure Application Insights](app-insights-asp-net.md).</span></span> <span data-ttu-id="c86d1-106">Az Application Insights figyeli a webes alkalmazás bejövő kérelmet, REST és SQL-hívások, kivételek és naplókivonatokat kimenő.</span><span class="sxs-lookup"><span data-stu-id="c86d1-106">Application Insights monitors your web application's incoming requests, outgoing REST and SQL calls, exceptions, and log traces.</span></span> <span data-ttu-id="c86d1-107">Nyújtja a irányítópultok metrika diagramok és intelligens riasztások, valamint hatékony diagnosztikai keresési és elemzési lekérdezéseket a telemetriai keresztül.</span><span class="sxs-lookup"><span data-stu-id="c86d1-107">It provides dashboards with metric charts and smart alerts, as well as powerful diagnostic search and analytical queries over this telemetry.</span></span> 

<span data-ttu-id="c86d1-108">Az Application Insights általi figyelés az SCOM felügyeleti csomagok használatával válthat.</span><span class="sxs-lookup"><span data-stu-id="c86d1-108">You can switch on Application Insights monitoring by using an SCOM management pack.</span></span>

## <a name="before-you-start"></a><span data-ttu-id="c86d1-109">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="c86d1-109">Before you start</span></span>
<span data-ttu-id="c86d1-110">Feltételezzük, hogy:</span><span class="sxs-lookup"><span data-stu-id="c86d1-110">We assume:</span></span>

* <span data-ttu-id="c86d1-111">Megismerheti az SCOM, és a használt SCOM 2012 R2 vagy 2016 toomanage az IIS webkiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="c86d1-111">You're familiar with SCOM, and that you use SCOM 2012 R2 or 2016 toomanage your IIS web servers.</span></span>
* <span data-ttu-id="c86d1-112">Már telepítette a kiszolgálókon, amelyet az Application insights szolgáltatással toomonitor webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="c86d1-112">You have already installed on your servers a web application that you want toomonitor with Application Insights.</span></span>
* <span data-ttu-id="c86d1-113">Az alkalmazás-keretrendszer verziója a .NET 4.5-ös vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="c86d1-113">App framework version is .NET 4.5 or later.</span></span>
* <span data-ttu-id="c86d1-114">Hozzáférés tooa előfizetéssel rendelkezik [Microsoft Azure](https://azure.com) és bejelentkezhet a toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="c86d1-114">You have access tooa subscription in [Microsoft Azure](https://azure.com) and can sign in toohello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="c86d1-115">A szervezet előfordulhat, hogy rendelkezik előfizetéssel, és a Microsoft-fiók tooit adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="c86d1-115">Your organization may have a subscription, and can add your Microsoft account tooit.</span></span>

<span data-ttu-id="c86d1-116">(hello fejlesztői csapat létrehozhat hello [Application Insights SDK](app-insights-asp-net.md) hello webes alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="c86d1-116">(hello development team might build hello [Application Insights SDK](app-insights-asp-net.md) into hello web app.</span></span> <span data-ttu-id="c86d1-117">A felépítés során instrumentation egyéni telemetria írásban nagyobb rugalmasságot biztosít.</span><span class="sxs-lookup"><span data-stu-id="c86d1-117">This build-time instrumentation gives them greater flexibility in writing custom telemetry.</span></span> <span data-ttu-id="c86d1-118">Azonban nem számít: hello SDK beépített nélkül akár az itt leírt hello lépések végrehajtásával.)</span><span class="sxs-lookup"><span data-stu-id="c86d1-118">However, it doesn't matter: you can follow hello steps described here either with or without hello SDK built in.)</span></span>

## <a name="one-time-install-application-insights-management-pack"></a><span data-ttu-id="c86d1-119">(Egy alkalommal) Az Application Insights felügyeleti csomag telepítéséhez</span><span class="sxs-lookup"><span data-stu-id="c86d1-119">(One time) Install Application Insights management pack</span></span>
<span data-ttu-id="c86d1-120">Az Operations Manager futtató hello gépen:</span><span class="sxs-lookup"><span data-stu-id="c86d1-120">On hello machine where you run Operations Manager:</span></span>

1. <span data-ttu-id="c86d1-121">Távolítsa el a régi verziójú hello felügyeleti csomag:</span><span class="sxs-lookup"><span data-stu-id="c86d1-121">Uninstall any old version of hello management pack:</span></span>
   1. <span data-ttu-id="c86d1-122">Az Operations Manager programban nyissa meg a felügyeleti, a felügyeleti csomagokat.</span><span class="sxs-lookup"><span data-stu-id="c86d1-122">In Operations Manager, open Administration, Management Packs.</span></span> 
   2. <span data-ttu-id="c86d1-123">Törölje a hello régi verzióját.</span><span class="sxs-lookup"><span data-stu-id="c86d1-123">Delete hello old version.</span></span>
2. <span data-ttu-id="c86d1-124">Töltse le és hello katalógusból hello felügyeleti csomag telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="c86d1-124">Download and install hello management pack from hello catalog.</span></span>
3. <span data-ttu-id="c86d1-125">Indítsa újra az Operations Manager.</span><span class="sxs-lookup"><span data-stu-id="c86d1-125">Restart Operations Manager.</span></span>

## <a name="create-a-management-pack"></a><span data-ttu-id="c86d1-126">Felügyeleti csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="c86d1-126">Create a management pack</span></span>
1. <span data-ttu-id="c86d1-127">Az Operations Manager programban nyissa meg a **szerzői műveletek**, **.NET... az Application insights szolgáltatással**, **figyelés felvétele varázsló**, és újra válasszon **... az Application insights szolgáltatással .NET**.</span><span class="sxs-lookup"><span data-stu-id="c86d1-127">In Operations Manager, open **Authoring**, **.NET...with Application Insights**, **Add Monitoring Wizard**, and again choose **.NET...with Application Insights**.</span></span>
   
    ![](./media/app-insights-scom/020.png)
2. <span data-ttu-id="c86d1-128">Hello konfigurációja után az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="c86d1-128">Name hello configuration after your app.</span></span> <span data-ttu-id="c86d1-129">(A telepítve tooinstrument egy alkalmazás egyszerre.)</span><span class="sxs-lookup"><span data-stu-id="c86d1-129">(You have tooinstrument one app at a time.)</span></span>
   
    ![](./media/app-insights-scom/030.png)
3. <span data-ttu-id="c86d1-130">A hello azonos varázslólap, vagy létrehozni egy új felügyeleti csomag, vagy válassza ki az Application Insights korábban létrehozott csomagot.</span><span class="sxs-lookup"><span data-stu-id="c86d1-130">On hello same wizard page, either create a new management pack, or select a pack that you created for Application Insights earlier.</span></span>
   
     <span data-ttu-id="c86d1-131">(az Application Insights hello [felügyeleti csomag](https://technet.microsoft.com/library/cc974491.aspx) egy sablon, amelyből példányt létrehozni.</span><span class="sxs-lookup"><span data-stu-id="c86d1-131">(hello Application Insights [management pack](https://technet.microsoft.com/library/cc974491.aspx) is a template, from which you create an instance.</span></span> <span data-ttu-id="c86d1-132">Újrahasználhatja azonos újabb példány hello.)</span><span class="sxs-lookup"><span data-stu-id="c86d1-132">You can reuse hello same instance later.)</span></span>

    ![Az általános tulajdonságok lapján hello írja be a hello app hello nevét.](./media/app-insights-scom/040.png)

1. <span data-ttu-id="c86d1-136">Válassza ki egy alkalmazást, amelyet az toomonitor.</span><span class="sxs-lookup"><span data-stu-id="c86d1-136">Choose one app that you want toomonitor.</span></span> <span data-ttu-id="c86d1-137">hello keresési funkciót keres a kiszolgálókon telepített alkalmazások között.</span><span class="sxs-lookup"><span data-stu-id="c86d1-137">hello search feature searches among apps installed on your servers.</span></span>
   
    ![Milyen tooMonitor lapon kattintson a Hozzáadás gombra, írja be hello alkalmazásnév részét, kattintson a keresés, hello alkalmazást, majd a Hozzáadás, válassza az OK gombra.](./media/app-insights-scom/050.png)
   
    <span data-ttu-id="c86d1-139">hello választható figyelés hatókör mező lehet használt toospecify a kiszolgálók részhalmazát, ha nem szeretné, hogy toomonitor hello alkalmazást az összes kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="c86d1-139">hello optional Monitoring scope field can be used toospecify a subset of your servers, if you don't want toomonitor hello app in all servers.</span></span>
2. <span data-ttu-id="c86d1-140">Hello varázsló következő lapján meg kell adnia a hitelesítő adatok toosign a tooMicrosoft Azure.</span><span class="sxs-lookup"><span data-stu-id="c86d1-140">On hello next wizard page, you must first provide your credentials toosign in tooMicrosoft Azure.</span></span>
   
    <span data-ttu-id="c86d1-141">Ezen a lapon választhat hello telemetriai adatok toobe elemzése és jelenik meg, ahová hello Application Insights-erőforrást.</span><span class="sxs-lookup"><span data-stu-id="c86d1-141">On this page, you choose hello Application Insights resource where you want hello telemetry data toobe analyzed and displayed.</span></span> 
   
   * <span data-ttu-id="c86d1-142">Ha hello alkalmazások fejlesztése során az Application Insights lett konfigurálva, válassza ki a meglévő erőforrást.</span><span class="sxs-lookup"><span data-stu-id="c86d1-142">If hello application was configured for Application Insights during development, select its existing resource.</span></span>
   * <span data-ttu-id="c86d1-143">Máskülönben hozzon létre egy új erőforrást: hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="c86d1-143">Otherwise, create a new resource named for hello app.</span></span> <span data-ttu-id="c86d1-144">Ha más alkalmazásokat, amelyek összetevők hello az adott rendszer, helyezze őket hello ugyanabban az erőforráscsoportban, toomake hozzáférés toohello telemetriai könnyebb toomanage.</span><span class="sxs-lookup"><span data-stu-id="c86d1-144">If there are other apps that are components of hello same system, put them in hello same resource group, toomake access toohello telemetry easier toomanage.</span></span>
     
     <span data-ttu-id="c86d1-145">Ezeket a beállításokat később bármikor módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="c86d1-145">You can change these settings later.</span></span>
     
     ![Az Application Insights-beállítások lapján kattintson a "bejelentkezés", és adja meg a Microsoft-fiók hitelesítő adatait az Azure-bA.](./media/app-insights-scom/060.png)
3. <span data-ttu-id="c86d1-148">Teljes hello varázsló.</span><span class="sxs-lookup"><span data-stu-id="c86d1-148">Complete hello wizard.</span></span>
   
    ![Kattintson a Létrehozás gombra](./media/app-insights-scom/070.png)

<span data-ttu-id="c86d1-150">Ismételje meg ezt az eljárást minden alkalmazás, amelyet az toomonitor.</span><span class="sxs-lookup"><span data-stu-id="c86d1-150">Repeat this procedure for each app that you want toomonitor.</span></span>

<span data-ttu-id="c86d1-151">Ha később kell toochange beállításokat, nyissa meg újra hello tulajdonságok hello figyelő hello szerzői műveletek ablakból.</span><span class="sxs-lookup"><span data-stu-id="c86d1-151">If you need toochange settings later, re-open hello properties of hello monitor from hello Authoring window.</span></span>

![A szerzői műveletek, válassza ki a .NET alkalmazásteljesítmény-figyelés az Application insights szolgáltatással, válassza ki a figyelő, és kattintson a Tulajdonságok elemre.](./media/app-insights-scom/080.png)

## <a name="verify-monitoring"></a><span data-ttu-id="c86d1-153">Ellenőrizze a figyelés</span><span class="sxs-lookup"><span data-stu-id="c86d1-153">Verify monitoring</span></span>
<span data-ttu-id="c86d1-154">hello figyelése, hogy telepítette minden kiszolgálón megkeresi az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="c86d1-154">hello monitor that you have installed searches for your app on every server.</span></span> <span data-ttu-id="c86d1-155">Ha úgy találja, hello alkalmazást, és beállítja az Application Insights Állapotmonitor toomonitor hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="c86d1-155">Where it finds hello app, it configures Application Insights Status Monitor toomonitor hello app.</span></span> <span data-ttu-id="c86d1-156">Ha szükséges, állapotfigyelő először hello kiszolgálót telepít.</span><span class="sxs-lookup"><span data-stu-id="c86d1-156">If necessary, it first installs Status Monitor on hello server.</span></span>

<span data-ttu-id="c86d1-157">Talált hello alkalmazás előfordulások ellenőrizheti:</span><span class="sxs-lookup"><span data-stu-id="c86d1-157">You can verify which instances of hello app it has found:</span></span>

![A figyelés, nyissa meg az Application insights szolgáltatással](./media/app-insights-scom/100.png)

## <a name="view-telemetry-in-application-insights"></a><span data-ttu-id="c86d1-159">Az Application Insightsban nézet telemetria</span><span class="sxs-lookup"><span data-stu-id="c86d1-159">View telemetry in Application Insights</span></span>
<span data-ttu-id="c86d1-160">A hello [Azure-portálon](https://portal.azure.com), keresse meg az alkalmazás toohello erőforrás.</span><span class="sxs-lookup"><span data-stu-id="c86d1-160">In hello [Azure portal](https://portal.azure.com), browse toohello resource for your app.</span></span> <span data-ttu-id="c86d1-161">Ön [telemetriai ábrázoló diagramok megjelenítéséhez](app-insights-dashboards.md) az alkalmazásból.</span><span class="sxs-lookup"><span data-stu-id="c86d1-161">You [see charts showing telemetry](app-insights-dashboards.md) from your app.</span></span> <span data-ttu-id="c86d1-162">(Ha az nem látható hello főoldalon még, kattintson a metrikák élő adatfolyam.)</span><span class="sxs-lookup"><span data-stu-id="c86d1-162">(If it hasn't shown up on hello main page yet, click Live Metrics Stream.)</span></span>

## <a name="next-steps"></a><span data-ttu-id="c86d1-163">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c86d1-163">Next steps</span></span>
* <span data-ttu-id="c86d1-164">[Állítson be egy irányítópultot](app-insights-dashboards.md) toobring együtt hello legfontosabb diagramok ezzel és más alkalmazások figyelését.</span><span class="sxs-lookup"><span data-stu-id="c86d1-164">[Set up a dashboard](app-insights-dashboards.md) toobring together hello most important charts monitoring this and other apps.</span></span>
* [<span data-ttu-id="c86d1-165">További tudnivalók a metrikák</span><span class="sxs-lookup"><span data-stu-id="c86d1-165">Learn about metrics</span></span>](app-insights-metrics-explorer.md)
* [<span data-ttu-id="c86d1-166">Riasztások beállítása</span><span class="sxs-lookup"><span data-stu-id="c86d1-166">Set up alerts</span></span>](app-insights-alerts.md)
* [<span data-ttu-id="c86d1-167">Teljesítménnyel kapcsolatos problémák diagnosztizálása</span><span class="sxs-lookup"><span data-stu-id="c86d1-167">Diagnosing performance issues</span></span>](app-insights-detect-triage-diagnose.md)
* [<span data-ttu-id="c86d1-168">Hatékony elemzési lekérdezések</span><span class="sxs-lookup"><span data-stu-id="c86d1-168">Powerful Analytics queries</span></span>](app-insights-analytics.md)
* [<span data-ttu-id="c86d1-169">A webteszt rendelkezésre állása</span><span class="sxs-lookup"><span data-stu-id="c86d1-169">Availability web tests</span></span>](app-insights-monitor-web-app-availability.md)

