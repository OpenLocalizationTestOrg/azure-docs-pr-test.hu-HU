---
title: "502-es aaaFix hibás átjáró, a szolgáltatás nem érhető el 503-as |} Microsoft Docs"
description: "Végezzen hibaelhárítást a 502 Hibás átjáró hibák és 503-as szolgáltatás nem érhető el az Azure App Service-ben futtatott webalkalmazás."
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: top-support-issue
keywords: "502 Hibás átjáró 503-as szolgáltatás nem érhető el, 503-as hiba, 502-es hiba"
ms.assetid: 51cd331a-a3fa-438f-90ef-385e755e50d5
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2016
ms.author: cephalin
ms.openlocfilehash: d9d8dcddaac930967a2e8d2bfd8cad09e6824c17
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-http-errors-of-502-bad-gateway-and-503-service-unavailable-in-your-azure-web-apps"></a><span data-ttu-id="0897f-104">A "502 Hibás átjáró" és "503-as szolgáltatás nem érhető el" az Azure web apps a HTTP-hibák elhárítása</span><span class="sxs-lookup"><span data-stu-id="0897f-104">Troubleshoot HTTP errors of "502 bad gateway" and "503 service unavailable" in your Azure web apps</span></span>
<span data-ttu-id="0897f-105">"502 Hibás átjáró" és "503-as szolgáltatás nem érhető el" tárolva a webalkalmazás gyakori hibáinak [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="0897f-105">"502 bad gateway" and "503 service unavailable" are common errors in your web app hosted in [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span> <span data-ttu-id="0897f-106">Ez a cikk segítséget nyújt a hibák elhárítása.</span><span class="sxs-lookup"><span data-stu-id="0897f-106">This article helps you troubleshoot these errors.</span></span>

<span data-ttu-id="0897f-107">Ha ez a cikk bármely pontján további segítségre van szüksége, forduljon az Azure-szakértők hello [MSDN Azure hello és fórumok Stack Overflow hello](https://azure.microsoft.com/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="0897f-107">If you need more help at any point in this article, you can contact hello Azure experts on [hello MSDN Azure and hello Stack Overflow forums](https://azure.microsoft.com/support/forums/).</span></span> <span data-ttu-id="0897f-108">Másik lehetőségként is fájl is az Azure támogatási incidens.</span><span class="sxs-lookup"><span data-stu-id="0897f-108">Alternatively, you can also file an Azure support incident.</span></span> <span data-ttu-id="0897f-109">Nyissa meg toohello [Azure támogatási webhelyén](https://azure.microsoft.com/support/options/) , majd kattintson a **támogatás**.</span><span class="sxs-lookup"><span data-stu-id="0897f-109">Go toohello [Azure Support site](https://azure.microsoft.com/support/options/) and click on **Get Support**.</span></span>

## <a name="symptom"></a><span data-ttu-id="0897f-110">Jelenség</span><span class="sxs-lookup"><span data-stu-id="0897f-110">Symptom</span></span>
<span data-ttu-id="0897f-111">Ha a felhasználó toohello webes alkalmazás, egy HTTP adja vissza "502 Hibás átjáró" hiba vagy a HTTP "503-as szolgáltatás nem érhető el" hiba.</span><span class="sxs-lookup"><span data-stu-id="0897f-111">When you browse toohello web app, it returns a HTTP "502 Bad Gateway" error or a HTTP "503 Service Unavailable" error.</span></span>

## <a name="cause"></a><span data-ttu-id="0897f-112">Ok</span><span class="sxs-lookup"><span data-stu-id="0897f-112">Cause</span></span>
<span data-ttu-id="0897f-113">Ez a probléma gyakran alkalmazás szintű problémákat, például:</span><span class="sxs-lookup"><span data-stu-id="0897f-113">This problem is often caused by application level issues, such as:</span></span>

* <span data-ttu-id="0897f-114">hosszú ideig kérelmek</span><span class="sxs-lookup"><span data-stu-id="0897f-114">requests taking a long time</span></span>
* <span data-ttu-id="0897f-115">nagy memória/CPU-t használó alkalmazások</span><span class="sxs-lookup"><span data-stu-id="0897f-115">application using high memory/CPU</span></span>
* <span data-ttu-id="0897f-116">az alkalmazás összeomló tooan kivétel miatt.</span><span class="sxs-lookup"><span data-stu-id="0897f-116">application crashing due tooan exception.</span></span>

## <a name="troubleshooting-steps-toosolve-502-bad-gateway-and-503-service-unavailable-errors"></a><span data-ttu-id="0897f-117">Hibaelhárítási lépéseket toosolve "502 Hibás átjáró" és "503-as szolgáltatás nem érhető el" hiba</span><span class="sxs-lookup"><span data-stu-id="0897f-117">Troubleshooting steps toosolve "502 bad gateway" and "503 service unavailable" errors</span></span>
<span data-ttu-id="0897f-118">Hibaelhárítás három különböző feladatokat, egymás utáni sorrendben osztható:</span><span class="sxs-lookup"><span data-stu-id="0897f-118">Troubleshooting can be divided into three distinct tasks, in sequential order:</span></span>

1. [<span data-ttu-id="0897f-119">Figyelje meg, és figyelheti az alkalmazások viselkedését</span><span class="sxs-lookup"><span data-stu-id="0897f-119">Observe and monitor application behavior</span></span>](#observe)
2. [<span data-ttu-id="0897f-120">Adatok gyűjtése</span><span class="sxs-lookup"><span data-stu-id="0897f-120">Collect data</span></span>](#collect)
3. [<span data-ttu-id="0897f-121">Hello probléma elhárítása érdekében</span><span class="sxs-lookup"><span data-stu-id="0897f-121">Mitigate hello issue</span></span>](#mitigate)

<span data-ttu-id="0897f-122">[App Service Web Apps](/services/app-service/web/) minden lépésnél különböző lehetőséget biztosít.</span><span class="sxs-lookup"><span data-stu-id="0897f-122">[App Service Web Apps](/services/app-service/web/) gives you various options at each step.</span></span>

<a name="observe" />

### <a name="1-observe-and-monitor-application-behavior"></a><span data-ttu-id="0897f-123">1. Figyelje meg, és figyelheti az alkalmazások viselkedését</span><span class="sxs-lookup"><span data-stu-id="0897f-123">1. Observe and monitor application behavior</span></span>
#### <a name="track-service-health"></a><span data-ttu-id="0897f-124">Nyomon követése szolgáltatásának állapota</span><span class="sxs-lookup"><span data-stu-id="0897f-124">Track Service health</span></span>
<span data-ttu-id="0897f-125">A Microsoft Azure publicizes minden alkalommal, amikor a szolgáltatás megszakadásának vagy a teljesítmény romlását van.</span><span class="sxs-lookup"><span data-stu-id="0897f-125">Microsoft Azure publicizes each time there is a service interruption or performance degradation.</span></span> <span data-ttu-id="0897f-126">Hello szolgáltatás állapotának hello követheti a hello [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="0897f-126">You can track hello health of hello service on hello [Azure Portal](https://portal.azure.com/).</span></span> <span data-ttu-id="0897f-127">További információkért lásd: [szolgáltatás állapotát nyomon](../monitoring-and-diagnostics/insights-service-health.md).</span><span class="sxs-lookup"><span data-stu-id="0897f-127">For more information, see [Track service health](../monitoring-and-diagnostics/insights-service-health.md).</span></span>

#### <a name="monitor-your-web-app"></a><span data-ttu-id="0897f-128">A webes alkalmazás figyelése</span><span class="sxs-lookup"><span data-stu-id="0897f-128">Monitor your web app</span></span>
<span data-ttu-id="0897f-129">Ezzel a beállítással toofind, ha az alkalmazás problémáit.</span><span class="sxs-lookup"><span data-stu-id="0897f-129">This option enables you toofind out if your application is having any issues.</span></span> <span data-ttu-id="0897f-130">A webalkalmazás panelen kattintson a hello **kérések és hibák követése** csempére.</span><span class="sxs-lookup"><span data-stu-id="0897f-130">In your web app’s blade, click hello **Requests and errors** tile.</span></span> <span data-ttu-id="0897f-131">Hello **metrika** panelen láthatja, adhat hozzá minden hello metrikákat.</span><span class="sxs-lookup"><span data-stu-id="0897f-131">hello **Metric** blade will show you all hello metrics you can add.</span></span>

<span data-ttu-id="0897f-132">Hello metrikákat, érdemes a webalkalmazás toomonitor vannak</span><span class="sxs-lookup"><span data-stu-id="0897f-132">Some of hello metrics that you might want toomonitor for your web app are</span></span>

* <span data-ttu-id="0897f-133">Munkakészlet átlagos memória</span><span class="sxs-lookup"><span data-stu-id="0897f-133">Average memory working set</span></span>
* <span data-ttu-id="0897f-134">Átlagos válaszidő</span><span class="sxs-lookup"><span data-stu-id="0897f-134">Average response time</span></span>
* <span data-ttu-id="0897f-135">CPU-idő</span><span class="sxs-lookup"><span data-stu-id="0897f-135">CPU time</span></span>
* <span data-ttu-id="0897f-136">Memória munkakészlete</span><span class="sxs-lookup"><span data-stu-id="0897f-136">Memory working set</span></span>
* <span data-ttu-id="0897f-137">Kérelmek</span><span class="sxs-lookup"><span data-stu-id="0897f-137">Requests</span></span>

![a figyelő a webalkalmazás 502 Hibás átjáró és 503-as szolgáltatás nem érhető el a HTTP-hibák megoldása felé](./media/app-service-web-troubleshoot-HTTP-502-503/1-monitor-metrics.png)

<span data-ttu-id="0897f-139">További információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="0897f-139">For more information, see:</span></span>

* [<span data-ttu-id="0897f-140">Az Azure App Service Web-alkalmazások figyelése</span><span class="sxs-lookup"><span data-stu-id="0897f-140">Monitor Web Apps in Azure App Service</span></span>](web-sites-monitor.md)
* [<span data-ttu-id="0897f-141">Riasztási értesítések fogadása</span><span class="sxs-lookup"><span data-stu-id="0897f-141">Receive alert notifications</span></span>](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

<a name="collect" />

### <a name="2-collect-data"></a><span data-ttu-id="0897f-142">2. Adatok gyűjtése</span><span class="sxs-lookup"><span data-stu-id="0897f-142">2. Collect data</span></span>
#### <a name="use-hello-azure-app-service-support-portal"></a><span data-ttu-id="0897f-143">Hello Azure App Service támogatási portál használata</span><span class="sxs-lookup"><span data-stu-id="0897f-143">Use hello Azure App Service Support Portal</span></span>
<span data-ttu-id="0897f-144">Web Apps biztosít hello képességét tootroubleshoot problémák kapcsolódó tooyour webalkalmazás HTTP naplókat, eseménynaplók, folyamat memóriaképek és több megtekintésével.</span><span class="sxs-lookup"><span data-stu-id="0897f-144">Web Apps provides you with hello ability tootroubleshoot issues related tooyour web app by looking at HTTP logs, event logs, process dumps, and more.</span></span> <span data-ttu-id="0897f-145">Végezheti el az összes ezt az információt a támogatási portálon, a **http://&lt;az alkalmazás neve >.scm.azurewebsites.net/Support**</span><span class="sxs-lookup"><span data-stu-id="0897f-145">You can access all this information using our Support portal at **http://&lt;your app name>.scm.azurewebsites.net/Support**</span></span>

<span data-ttu-id="0897f-146">hello Azure App Service-támogatás portal nyújt három különálló lapok toosupport hello három lépést a gyakori hibaelhárítási használatára:</span><span class="sxs-lookup"><span data-stu-id="0897f-146">hello Azure App Service Support portal provides you with three separate tabs toosupport hello three steps of a common troubleshooting scenario:</span></span>

1. <span data-ttu-id="0897f-147">Figyelje meg a jelenlegi viselkedése</span><span class="sxs-lookup"><span data-stu-id="0897f-147">Observe current behavior</span></span>
2. <span data-ttu-id="0897f-148">Diagnosztikai adatok gyűjtése és futtató hello beépített elemzőkkel elemzése</span><span class="sxs-lookup"><span data-stu-id="0897f-148">Analyze by collecting diagnostics information and running hello built-in analyzers</span></span>
3. <span data-ttu-id="0897f-149">Csökkentése</span><span class="sxs-lookup"><span data-stu-id="0897f-149">Mitigate</span></span>

<span data-ttu-id="0897f-150">Ha most hello hiba történik, kattintson a **elemzés** > **diagnosztika** > **diagnosztizálása most** toocreate diagnosztikai munkamenet, amely összegyűjti a HTTP jelentkezik, az Eseménynapló, memóriát, kiírt fájlok, a PHP hibanaplókat és a PHP feldolgozni a jelentést.</span><span class="sxs-lookup"><span data-stu-id="0897f-150">If hello issue is happening right now, click **Analyze** > **Diagnostics** > **Diagnose Now** toocreate a diagnostic session for you, which will collect HTTP logs, event viewer logs, memory dumps, PHP error logs and PHP process report.</span></span>

<span data-ttu-id="0897f-151">Miután hello adatgyűjtés, emellett futtatása egy hello adatokon és is biztosítanak egy HTML-jelentést.</span><span class="sxs-lookup"><span data-stu-id="0897f-151">Once hello data is collected, it will also run an analysis on hello data and provide you with an HTML report.</span></span>

<span data-ttu-id="0897f-152">Abban az esetben, ha azt szeretné, toodownload hello adatok, alapértelmezés szerint, akkor legyen tárolva hello D:\home\data\DaaS mappában.</span><span class="sxs-lookup"><span data-stu-id="0897f-152">In case you want toodownload hello data, by default, it would be stored in hello D:\home\data\DaaS folder.</span></span>

<span data-ttu-id="0897f-153">Hello Azure App Service támogatási portálon további információkért lásd: [új frissítések tooSupport hely bővítmény Azure webhelyekhez](https://azure.microsoft.com/blog/new-updates-to-support-site-extension-for-azure-websites).</span><span class="sxs-lookup"><span data-stu-id="0897f-153">For more information on hello Azure App Service Support portal, see [New Updates tooSupport Site Extension for Azure Websites](https://azure.microsoft.com/blog/new-updates-to-support-site-extension-for-azure-websites).</span></span>

#### <a name="use-hello-kudu-debug-console"></a><span data-ttu-id="0897f-154">Hello Kudu hibakereső konzol használata</span><span class="sxs-lookup"><span data-stu-id="0897f-154">Use hello Kudu Debug Console</span></span>
<span data-ttu-id="0897f-155">A hibakereső konzol hibakeresés, felfedezése, fájlok, valamint JSON végpontokat a környezettel kapcsolatos információk beolvasásakor feltöltése használható webes alkalmazásokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="0897f-155">Web Apps comes with a debug console that you can use for debugging, exploring, uploading files, as well as JSON endpoints for getting information about your environment.</span></span> <span data-ttu-id="0897f-156">Ez a lehetőség hello *Kudu konzol* vagy hello *SCM irányítópult* a webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="0897f-156">This is called hello *Kudu Console* or hello *SCM Dashboard* for your web app.</span></span>

<span data-ttu-id="0897f-157">Érhető el ehhez az irányítópulthoz toohello hivatkozást fog **https://&lt;az alkalmazás neve >.scm.azurewebsites.net/**.</span><span class="sxs-lookup"><span data-stu-id="0897f-157">You can access this dashboard by going toohello link **https://&lt;Your app name>.scm.azurewebsites.net/**.</span></span>

<span data-ttu-id="0897f-158">A Kudu biztosítja hello, amit a következők:</span><span class="sxs-lookup"><span data-stu-id="0897f-158">Some of hello things that Kudu provides are:</span></span>

* <span data-ttu-id="0897f-159">az alkalmazás környezeti beállítások</span><span class="sxs-lookup"><span data-stu-id="0897f-159">environment settings for your application</span></span>
* <span data-ttu-id="0897f-160">naplófolyamot</span><span class="sxs-lookup"><span data-stu-id="0897f-160">log stream</span></span>
* <span data-ttu-id="0897f-161">diagnosztikai memóriakép</span><span class="sxs-lookup"><span data-stu-id="0897f-161">diagnostic dump</span></span>
* <span data-ttu-id="0897f-162">a hibakeresési konzol, ahol futtathatja a Powershell-parancsmagok és alapvető DOS parancsok.</span><span class="sxs-lookup"><span data-stu-id="0897f-162">debug console in which you can run Powershell cmdlets and basic DOS commands.</span></span>

<span data-ttu-id="0897f-163">A Kudu egy másik fontos része, hogy de, abban az esetben, ha az alkalmazás első-alkalommal kivételek szűrész, használhatja a Kudu hello SysInternals eszköz Procdump toocreate memóriaképeket.</span><span class="sxs-lookup"><span data-stu-id="0897f-163">Another useful feature of Kudu is that, in case your application is throwing first-chance exceptions, you can use Kudu and hello SysInternals tool Procdump toocreate memory dumps.</span></span> <span data-ttu-id="0897f-164">Ezek memóriaképek pillanatképek hello folyamat, és gyakran segítségével bonyolultabb webalkalmazásokba elhárítása.</span><span class="sxs-lookup"><span data-stu-id="0897f-164">These memory dumps are snapshots of hello process and can often help you troubleshoot more complicated issues with your web app.</span></span>

<span data-ttu-id="0897f-165">A Kudu szolgáltatásainak további információkért lásd: [Azure Websitesra online eszközök kell tudni](https://azure.microsoft.com/blog/windows-azure-websites-online-tools-you-should-know-about/).</span><span class="sxs-lookup"><span data-stu-id="0897f-165">For more information on features available in Kudu, see [Azure Websites online tools you should know about](https://azure.microsoft.com/blog/windows-azure-websites-online-tools-you-should-know-about/).</span></span>

<a name="mitigate" />

### <a name="3-mitigate-hello-issue"></a><span data-ttu-id="0897f-166">3. Hello probléma elhárítása érdekében</span><span class="sxs-lookup"><span data-stu-id="0897f-166">3. Mitigate hello issue</span></span>
#### <a name="scale-hello-web-app"></a><span data-ttu-id="0897f-167">Skála hello webalkalmazás</span><span class="sxs-lookup"><span data-stu-id="0897f-167">Scale hello web app</span></span>
<span data-ttu-id="0897f-168">Az Azure App Service a teljesítmény növelése és a teljesítményt, módosíthatja hello méretezési, amelyen az alkalmazást futtat.</span><span class="sxs-lookup"><span data-stu-id="0897f-168">In Azure App Service, for increased performance and throughput,  you can adjust hello scale at which you are running your application.</span></span> <span data-ttu-id="0897f-169">A webes alkalmazás vertikális felskálázásával magában foglalja a két kapcsolódó műveletek: az App Service csomag tooa magasabb tarifacsomagra vált, és bizonyos beállítások konfigurálása után magasabb tarifacsomagra toohello rendelkezik váltott módosítása.</span><span class="sxs-lookup"><span data-stu-id="0897f-169">Scaling up a web app involves two related actions: changing your App Service plan tooa higher pricing tier, and configuring certain settings after you have switched toohello higher pricing tier.</span></span>

<span data-ttu-id="0897f-170">A méretezés további információkért lásd: [egy webalkalmazás skálázása az Azure App Service](web-sites-scale.md).</span><span class="sxs-lookup"><span data-stu-id="0897f-170">For more information on scaling, see [Scale a web app in Azure App Service](web-sites-scale.md).</span></span>

<span data-ttu-id="0897f-171">Ezenkívül lehetősége van toorun egynél több példány az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="0897f-171">Additionally, you can choose toorun your application on more than one instance .</span></span> <span data-ttu-id="0897f-172">Ez nem csak nyújt további feldolgozási képesség, de is lehetővé teszi bizonyos hibatűrést.</span><span class="sxs-lookup"><span data-stu-id="0897f-172">This not only provides you with more processing capability, but also gives you some amount of fault tolerance.</span></span> <span data-ttu-id="0897f-173">Ha hello folyamat leáll egy példányán, hello más példányt fog továbbra is küldött kérelmek.</span><span class="sxs-lookup"><span data-stu-id="0897f-173">If hello process goes down on one instance, hello other instance will still continue serving requests.</span></span>

<span data-ttu-id="0897f-174">Hello toobe manuális vagy automatikus skálázás állíthatja be.</span><span class="sxs-lookup"><span data-stu-id="0897f-174">You can set hello scaling toobe Manual or Automatic.</span></span>

#### <a name="use-autoheal"></a><span data-ttu-id="0897f-175">Elindulásáról használata</span><span class="sxs-lookup"><span data-stu-id="0897f-175">Use AutoHeal</span></span>
<span data-ttu-id="0897f-176">Elindulásáról hello (ilyen például a konfigurációs módosításokat, kérelmek, memória-alapú korlátok vagy hello idő szükséges tooexecute kérelmet) kiválasztott beállítások alapján az alkalmazás munkavégző folyamata újraindul.</span><span class="sxs-lookup"><span data-stu-id="0897f-176">AutoHeal recycles hello worker process for your app based on settings you choose (like configuration changes, requests, memory-based limits, or hello time needed tooexecute a request).</span></span> <span data-ttu-id="0897f-177">Legtöbbször ennek hello újrahasznosítást hello folyamat hello leggyorsabb módon toorecover a probléma.</span><span class="sxs-lookup"><span data-stu-id="0897f-177">Most of hello time, recycle hello process is hello fastest way toorecover from a problem.</span></span> <span data-ttu-id="0897f-178">Mindig újraindíthatja hello webalkalmazást az közvetlenül belül hello Azure portálon, bár elindulásáról lesz automatikusan elvégezze ezt meg.</span><span class="sxs-lookup"><span data-stu-id="0897f-178">Though you can always restart hello web app from directly within hello Azure Portal, AutoHeal will do it automatically for you.</span></span> <span data-ttu-id="0897f-179">Toodo szüksége néhány eseményindítók hozzáadása a webalkalmazáshoz hello legfelső szintű web.config fájlban.</span><span class="sxs-lookup"><span data-stu-id="0897f-179">All you need toodo is add some triggers in hello root web.config for your web app.</span></span> <span data-ttu-id="0897f-180">Vegye figyelembe, hogy ezek a beállítások akkor működnek a hello azonos módon akkor is, ha az alkalmazás nem egy .net egyet.</span><span class="sxs-lookup"><span data-stu-id="0897f-180">Note that these settings would work in hello same way even if your application is not a .Net one.</span></span>

<span data-ttu-id="0897f-181">További információkért lásd: [automatikus javítás Azure webhelyek](https://azure.microsoft.com/blog/auto-healing-windows-azure-web-sites/).</span><span class="sxs-lookup"><span data-stu-id="0897f-181">For more information, see [Auto-Healing Azure Web Sites](https://azure.microsoft.com/blog/auto-healing-windows-azure-web-sites/).</span></span>

#### <a name="restart-hello-web-app"></a><span data-ttu-id="0897f-182">Hello a webalkalmazás újraindítása</span><span class="sxs-lookup"><span data-stu-id="0897f-182">Restart hello web app</span></span>
<span data-ttu-id="0897f-183">Ez gyakran a legegyszerűbb módja toorecover hello az egyszeri hibák.</span><span class="sxs-lookup"><span data-stu-id="0897f-183">This is often hello simplest way toorecover from one-time issues.</span></span> <span data-ttu-id="0897f-184">A hello [Azure Portal](https://portal.azure.com/), a webalkalmazás panelen hello beállítások toostop van, vagy indítsa újra az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="0897f-184">On hello [Azure Portal](https://portal.azure.com/), on your web app’s blade, you have hello options toostop or restart your app.</span></span>

 ![Indítsa újra az alkalmazást toosolve HTTP-hibák 502 Hibás átjáró és 503-as szolgáltatás nem érhető el](./media/app-service-web-troubleshoot-HTTP-502-503/2-restart.png)

<span data-ttu-id="0897f-186">A webalkalmazás Azure Powershell használatával is kezelheti.</span><span class="sxs-lookup"><span data-stu-id="0897f-186">You can also manage your web app using Azure Powershell.</span></span> <span data-ttu-id="0897f-187">További információ: [Az Azure PowerShell használata az Azure Resource Manager eszközzel](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="0897f-187">For more information, see [Using Azure PowerShell with Azure Resource Manager](../powershell-azure-resource-manager.md).</span></span>

