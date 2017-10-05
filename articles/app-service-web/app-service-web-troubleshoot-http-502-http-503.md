---
title: "Javítsa ki a hibás átjáró 502, 503-as szolgáltatás nem érhető el |} Microsoft Docs"
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
ms.openlocfilehash: 397a6aaf7dc27adfa0fc0e722b8a2be5cc1d75f0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-http-errors-of-502-bad-gateway-and-503-service-unavailable-in-your-azure-web-apps"></a><span data-ttu-id="92e4d-104">A "502 Hibás átjáró" és "503-as szolgáltatás nem érhető el" az Azure web apps a HTTP-hibák elhárítása</span><span class="sxs-lookup"><span data-stu-id="92e4d-104">Troubleshoot HTTP errors of "502 bad gateway" and "503 service unavailable" in your Azure web apps</span></span>
<span data-ttu-id="92e4d-105">"502 Hibás átjáró" és "503-as szolgáltatás nem érhető el" tárolva a webalkalmazás gyakori hibáinak [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="92e4d-105">"502 bad gateway" and "503 service unavailable" are common errors in your web app hosted in [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span> <span data-ttu-id="92e4d-106">Ez a cikk segítséget nyújt a hibák elhárítása.</span><span class="sxs-lookup"><span data-stu-id="92e4d-106">This article helps you troubleshoot these errors.</span></span>

<span data-ttu-id="92e4d-107">Ha ez a cikk bármely pontján további segítségre van szüksége, forduljon az Azure-szakértők a [az MSDN Azure és a Stack Overflow fórumok](https://azure.microsoft.com/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="92e4d-107">If you need more help at any point in this article, you can contact the Azure experts on [the MSDN Azure and the Stack Overflow forums](https://azure.microsoft.com/support/forums/).</span></span> <span data-ttu-id="92e4d-108">Másik lehetőségként is fájl is az Azure támogatási incidens.</span><span class="sxs-lookup"><span data-stu-id="92e4d-108">Alternatively, you can also file an Azure support incident.</span></span> <span data-ttu-id="92e4d-109">Lépjen a [Azure támogatási webhelyén](https://azure.microsoft.com/support/options/) , majd kattintson a **támogatás**.</span><span class="sxs-lookup"><span data-stu-id="92e4d-109">Go to the [Azure Support site](https://azure.microsoft.com/support/options/) and click on **Get Support**.</span></span>

## <a name="symptom"></a><span data-ttu-id="92e4d-110">Jelenség</span><span class="sxs-lookup"><span data-stu-id="92e4d-110">Symptom</span></span>
<span data-ttu-id="92e4d-111">Ha a felhasználó azt a webalkalmazást, egy HTTP adja vissza "502 Hibás átjáró" hiba vagy a HTTP "503-as szolgáltatás nem érhető el" hiba.</span><span class="sxs-lookup"><span data-stu-id="92e4d-111">When you browse to the web app, it returns a HTTP "502 Bad Gateway" error or a HTTP "503 Service Unavailable" error.</span></span>

## <a name="cause"></a><span data-ttu-id="92e4d-112">Ok</span><span class="sxs-lookup"><span data-stu-id="92e4d-112">Cause</span></span>
<span data-ttu-id="92e4d-113">Ez a probléma gyakran alkalmazás szintű problémákat, például:</span><span class="sxs-lookup"><span data-stu-id="92e4d-113">This problem is often caused by application level issues, such as:</span></span>

* <span data-ttu-id="92e4d-114">hosszú ideig kérelmek</span><span class="sxs-lookup"><span data-stu-id="92e4d-114">requests taking a long time</span></span>
* <span data-ttu-id="92e4d-115">nagy memória/CPU-t használó alkalmazások</span><span class="sxs-lookup"><span data-stu-id="92e4d-115">application using high memory/CPU</span></span>
* <span data-ttu-id="92e4d-116">az alkalmazás összeomló kivétel miatt.</span><span class="sxs-lookup"><span data-stu-id="92e4d-116">application crashing due to an exception.</span></span>

## <a name="troubleshooting-steps-to-solve-502-bad-gateway-and-503-service-unavailable-errors"></a><span data-ttu-id="92e4d-117">Hibaelhárítási lépések "502 Hibás átjáró" és "503-as szolgáltatás nem érhető el" hiba elhárítása érdekében</span><span class="sxs-lookup"><span data-stu-id="92e4d-117">Troubleshooting steps to solve "502 bad gateway" and "503 service unavailable" errors</span></span>
<span data-ttu-id="92e4d-118">Hibaelhárítás három különböző feladatokat, egymás utáni sorrendben osztható:</span><span class="sxs-lookup"><span data-stu-id="92e4d-118">Troubleshooting can be divided into three distinct tasks, in sequential order:</span></span>

1. [<span data-ttu-id="92e4d-119">Figyelje meg, és figyelheti az alkalmazások viselkedését</span><span class="sxs-lookup"><span data-stu-id="92e4d-119">Observe and monitor application behavior</span></span>](#observe)
2. [<span data-ttu-id="92e4d-120">Adatok gyűjtése</span><span class="sxs-lookup"><span data-stu-id="92e4d-120">Collect data</span></span>](#collect)
3. [<span data-ttu-id="92e4d-121">A probléma elhárítása érdekében</span><span class="sxs-lookup"><span data-stu-id="92e4d-121">Mitigate the issue</span></span>](#mitigate)

<span data-ttu-id="92e4d-122">[App Service Web Apps](/services/app-service/web/) minden lépésnél különböző lehetőséget biztosít.</span><span class="sxs-lookup"><span data-stu-id="92e4d-122">[App Service Web Apps](/services/app-service/web/) gives you various options at each step.</span></span>

<a name="observe" />

### <a name="1-observe-and-monitor-application-behavior"></a><span data-ttu-id="92e4d-123">1. Figyelje meg, és figyelheti az alkalmazások viselkedését</span><span class="sxs-lookup"><span data-stu-id="92e4d-123">1. Observe and monitor application behavior</span></span>
#### <a name="track-service-health"></a><span data-ttu-id="92e4d-124">Nyomon követése szolgáltatásának állapota</span><span class="sxs-lookup"><span data-stu-id="92e4d-124">Track Service health</span></span>
<span data-ttu-id="92e4d-125">A Microsoft Azure publicizes minden alkalommal, amikor a szolgáltatás megszakadásának vagy a teljesítmény romlását van.</span><span class="sxs-lookup"><span data-stu-id="92e4d-125">Microsoft Azure publicizes each time there is a service interruption or performance degradation.</span></span> <span data-ttu-id="92e4d-126">A a szolgáltatás állapotának nyomon követheti a [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="92e4d-126">You can track the health of the service on the [Azure Portal](https://portal.azure.com/).</span></span> <span data-ttu-id="92e4d-127">További információkért lásd: [szolgáltatás állapotát nyomon](../monitoring-and-diagnostics/insights-service-health.md).</span><span class="sxs-lookup"><span data-stu-id="92e4d-127">For more information, see [Track service health](../monitoring-and-diagnostics/insights-service-health.md).</span></span>

#### <a name="monitor-your-web-app"></a><span data-ttu-id="92e4d-128">A webes alkalmazás figyelése</span><span class="sxs-lookup"><span data-stu-id="92e4d-128">Monitor your web app</span></span>
<span data-ttu-id="92e4d-129">Ez a beállítás lehetővé teszi annak megállapítása, ha az alkalmazás van bármilyen probléma merül.</span><span class="sxs-lookup"><span data-stu-id="92e4d-129">This option enables you to find out if your application is having any issues.</span></span> <span data-ttu-id="92e4d-130">A webalkalmazás panelen kattintson a **kérések és hibák követése** csempére.</span><span class="sxs-lookup"><span data-stu-id="92e4d-130">In your web app’s blade, click the **Requests and errors** tile.</span></span> <span data-ttu-id="92e4d-131">A **metrika** panelen láthatja, adhat hozzá minden metrikákat.</span><span class="sxs-lookup"><span data-stu-id="92e4d-131">The **Metric** blade will show you all the metrics you can add.</span></span>

<span data-ttu-id="92e4d-132">A metrikák, amelyeket a webalkalmazás figyelésére érdemes vannak</span><span class="sxs-lookup"><span data-stu-id="92e4d-132">Some of the metrics that you might want to monitor for your web app are</span></span>

* <span data-ttu-id="92e4d-133">Munkakészlet átlagos memória</span><span class="sxs-lookup"><span data-stu-id="92e4d-133">Average memory working set</span></span>
* <span data-ttu-id="92e4d-134">Átlagos válaszidő</span><span class="sxs-lookup"><span data-stu-id="92e4d-134">Average response time</span></span>
* <span data-ttu-id="92e4d-135">CPU-idő</span><span class="sxs-lookup"><span data-stu-id="92e4d-135">CPU time</span></span>
* <span data-ttu-id="92e4d-136">Memória munkakészlete</span><span class="sxs-lookup"><span data-stu-id="92e4d-136">Memory working set</span></span>
* <span data-ttu-id="92e4d-137">Kérelmek</span><span class="sxs-lookup"><span data-stu-id="92e4d-137">Requests</span></span>

![a figyelő a webalkalmazás 502 Hibás átjáró és 503-as szolgáltatás nem érhető el a HTTP-hibák megoldása felé](./media/app-service-web-troubleshoot-HTTP-502-503/1-monitor-metrics.png)

<span data-ttu-id="92e4d-139">További információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="92e4d-139">For more information, see:</span></span>

* [<span data-ttu-id="92e4d-140">Az Azure App Service Web-alkalmazások figyelése</span><span class="sxs-lookup"><span data-stu-id="92e4d-140">Monitor Web Apps in Azure App Service</span></span>](web-sites-monitor.md)
* [<span data-ttu-id="92e4d-141">Riasztási értesítések fogadása</span><span class="sxs-lookup"><span data-stu-id="92e4d-141">Receive alert notifications</span></span>](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

<a name="collect" />

### <a name="2-collect-data"></a><span data-ttu-id="92e4d-142">2. Adatok gyűjtése</span><span class="sxs-lookup"><span data-stu-id="92e4d-142">2. Collect data</span></span>
#### <a name="use-the-azure-app-service-support-portal"></a><span data-ttu-id="92e4d-143">Az Azure App Service támogatási portálon</span><span class="sxs-lookup"><span data-stu-id="92e4d-143">Use the Azure App Service Support Portal</span></span>
<span data-ttu-id="92e4d-144">Web Apps tesz lehetővé teszi a webalkalmazás HTTP naplókat, eseménynaplók, folyamat memóriaképek és több megtekintésével kapcsolatos problémák elhárítása.</span><span class="sxs-lookup"><span data-stu-id="92e4d-144">Web Apps provides you with the ability to troubleshoot issues related to your web app by looking at HTTP logs, event logs, process dumps, and more.</span></span> <span data-ttu-id="92e4d-145">Végezheti el az összes ezt az információt a támogatási portálon, a **http://&lt;az alkalmazás neve >.scm.azurewebsites.net/Support**</span><span class="sxs-lookup"><span data-stu-id="92e4d-145">You can access all this information using our Support portal at **http://&lt;your app name>.scm.azurewebsites.net/Support**</span></span>

<span data-ttu-id="92e4d-146">Az Azure App Service támogatja a portál tesz lehetővé a három lépést az általános hibaelhárítási forgatókönyv támogatásához három különálló lappal:</span><span class="sxs-lookup"><span data-stu-id="92e4d-146">The Azure App Service Support portal provides you with three separate tabs to support the three steps of a common troubleshooting scenario:</span></span>

1. <span data-ttu-id="92e4d-147">Figyelje meg a jelenlegi viselkedése</span><span class="sxs-lookup"><span data-stu-id="92e4d-147">Observe current behavior</span></span>
2. <span data-ttu-id="92e4d-148">Diagnosztikai adatok gyűjtése és a beépített elemzőkkel futtató elemzése</span><span class="sxs-lookup"><span data-stu-id="92e4d-148">Analyze by collecting diagnostics information and running the built-in analyzers</span></span>
3. <span data-ttu-id="92e4d-149">Csökkentése</span><span class="sxs-lookup"><span data-stu-id="92e4d-149">Mitigate</span></span>

<span data-ttu-id="92e4d-150">Ha a probléma most történik, kattintson a **elemzés** > **diagnosztika** > **diagnosztizálása most** diagnosztikai munkamenet létrehozására meg, amely összegyűjti a HTTP jelentkezik, az Eseménynapló, memóriát, kiírt fájlok, a PHP hibanaplókat és a PHP feldolgozni a jelentést.</span><span class="sxs-lookup"><span data-stu-id="92e4d-150">If the issue is happening right now, click **Analyze** > **Diagnostics** > **Diagnose Now** to create a diagnostic session for you, which will collect HTTP logs, event viewer logs, memory dumps, PHP error logs and PHP process report.</span></span>

<span data-ttu-id="92e4d-151">Miután az adatgyűjtés a következők, emellett futtassa az adatok elemzése és is biztosítanak egy HTML-jelentést.</span><span class="sxs-lookup"><span data-stu-id="92e4d-151">Once the data is collected, it will also run an analysis on the data and provide you with an HTML report.</span></span>

<span data-ttu-id="92e4d-152">Abban az esetben, ha szeretné letölteni az adatokat, alapértelmezés szerint, akkor legyen tárolva a D:\home\data\DaaS mappában.</span><span class="sxs-lookup"><span data-stu-id="92e4d-152">In case you want to download the data, by default, it would be stored in the D:\home\data\DaaS folder.</span></span>

<span data-ttu-id="92e4d-153">Az Azure App Service-támogatás Portal további információkért lásd: [támogatási webhely bővítménynek az Azure-webhelyek új frissítések](https://azure.microsoft.com/blog/new-updates-to-support-site-extension-for-azure-websites).</span><span class="sxs-lookup"><span data-stu-id="92e4d-153">For more information on the Azure App Service Support portal, see [New Updates to Support Site Extension for Azure Websites](https://azure.microsoft.com/blog/new-updates-to-support-site-extension-for-azure-websites).</span></span>

#### <a name="use-the-kudu-debug-console"></a><span data-ttu-id="92e4d-154">A Kudu hibakereső konzol használata</span><span class="sxs-lookup"><span data-stu-id="92e4d-154">Use the Kudu Debug Console</span></span>
<span data-ttu-id="92e4d-155">A hibakereső konzol hibakeresés, felfedezése, fájlok, valamint JSON végpontokat a környezettel kapcsolatos információk beolvasásakor feltöltése használható webes alkalmazásokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="92e4d-155">Web Apps comes with a debug console that you can use for debugging, exploring, uploading files, as well as JSON endpoints for getting information about your environment.</span></span> <span data-ttu-id="92e4d-156">Ez a lehetőség a *Kudu konzol* vagy a *SCM irányítópult* a webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="92e4d-156">This is called the *Kudu Console* or the *SCM Dashboard* for your web app.</span></span>

<span data-ttu-id="92e4d-157">Nyissa meg a hivatkozás férhet hozzá az irányítópulthoz **https://&lt;az alkalmazás neve >.scm.azurewebsites.net/**.</span><span class="sxs-lookup"><span data-stu-id="92e4d-157">You can access this dashboard by going to the link **https://&lt;Your app name>.scm.azurewebsites.net/**.</span></span>

<span data-ttu-id="92e4d-158">Az, amit a Kudu biztosítja a következők:</span><span class="sxs-lookup"><span data-stu-id="92e4d-158">Some of the things that Kudu provides are:</span></span>

* <span data-ttu-id="92e4d-159">az alkalmazás környezeti beállítások</span><span class="sxs-lookup"><span data-stu-id="92e4d-159">environment settings for your application</span></span>
* <span data-ttu-id="92e4d-160">naplófolyamot</span><span class="sxs-lookup"><span data-stu-id="92e4d-160">log stream</span></span>
* <span data-ttu-id="92e4d-161">diagnosztikai memóriakép</span><span class="sxs-lookup"><span data-stu-id="92e4d-161">diagnostic dump</span></span>
* <span data-ttu-id="92e4d-162">a hibakeresési konzol, ahol futtathatja a Powershell-parancsmagok és alapvető DOS parancsok.</span><span class="sxs-lookup"><span data-stu-id="92e4d-162">debug console in which you can run Powershell cmdlets and basic DOS commands.</span></span>

<span data-ttu-id="92e4d-163">Egy másik fontos része a Kudu, abban az esetben, ha az alkalmazás első-alkalommal kivételek szűrész, használhatja a Kudu, és kiírja a SysInternals eszköz Procdump memória létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="92e4d-163">Another useful feature of Kudu is that, in case your application is throwing first-chance exceptions, you can use Kudu and the SysInternals tool Procdump to create memory dumps.</span></span> <span data-ttu-id="92e4d-164">Ezek memóriaképek pillanatképek a folyamat, és gyakran segítségével bonyolultabb webalkalmazásokba elhárítása.</span><span class="sxs-lookup"><span data-stu-id="92e4d-164">These memory dumps are snapshots of the process and can often help you troubleshoot more complicated issues with your web app.</span></span>

<span data-ttu-id="92e4d-165">A Kudu szolgáltatásainak további információkért lásd: [Azure Websitesra online eszközök kell tudni](https://azure.microsoft.com/blog/windows-azure-websites-online-tools-you-should-know-about/).</span><span class="sxs-lookup"><span data-stu-id="92e4d-165">For more information on features available in Kudu, see [Azure Websites online tools you should know about](https://azure.microsoft.com/blog/windows-azure-websites-online-tools-you-should-know-about/).</span></span>

<a name="mitigate" />

### <a name="3-mitigate-the-issue"></a><span data-ttu-id="92e4d-166">3. A probléma elhárítása érdekében</span><span class="sxs-lookup"><span data-stu-id="92e4d-166">3. Mitigate the issue</span></span>
#### <a name="scale-the-web-app"></a><span data-ttu-id="92e4d-167">A webalkalmazás skálázása</span><span class="sxs-lookup"><span data-stu-id="92e4d-167">Scale the web app</span></span>
<span data-ttu-id="92e4d-168">Az Azure App Service a teljesítmény növelése és a teljesítményt, módosíthatja a skála, amelyen az alkalmazást futtat.</span><span class="sxs-lookup"><span data-stu-id="92e4d-168">In Azure App Service, for increased performance and throughput,  you can adjust the scale at which you are running your application.</span></span> <span data-ttu-id="92e4d-169">A webes alkalmazás vertikális felskálázásával magában foglalja a két kapcsolódó műveletek: az App Service-csomag módosítása egy magasabb szintű tarifacsomagban használható, és bizonyos beállítások konfigurálása után a magasabb szintű tarifacsomagban használható váltott.</span><span class="sxs-lookup"><span data-stu-id="92e4d-169">Scaling up a web app involves two related actions: changing your App Service plan to a higher pricing tier, and configuring certain settings after you have switched to the higher pricing tier.</span></span>

<span data-ttu-id="92e4d-170">A méretezés további információkért lásd: [egy webalkalmazás skálázása az Azure App Service](web-sites-scale.md).</span><span class="sxs-lookup"><span data-stu-id="92e4d-170">For more information on scaling, see [Scale a web app in Azure App Service](web-sites-scale.md).</span></span>

<span data-ttu-id="92e4d-171">Továbbá ha szeretné, futtassa az alkalmazást a egynél több példány.</span><span class="sxs-lookup"><span data-stu-id="92e4d-171">Additionally, you can choose to run your application on more than one instance .</span></span> <span data-ttu-id="92e4d-172">Ez nem csak nyújt további feldolgozási képesség, de is lehetővé teszi bizonyos hibatűrést.</span><span class="sxs-lookup"><span data-stu-id="92e4d-172">This not only provides you with more processing capability, but also gives you some amount of fault tolerance.</span></span> <span data-ttu-id="92e4d-173">Ha a folyamat leáll egy példányán, a többi példány továbbra is küldött kérelmek.</span><span class="sxs-lookup"><span data-stu-id="92e4d-173">If the process goes down on one instance, the other instance will still continue serving requests.</span></span>

<span data-ttu-id="92e4d-174">Manuális vagy automatikus skálázás állíthatja be.</span><span class="sxs-lookup"><span data-stu-id="92e4d-174">You can set the scaling to be Manual or Automatic.</span></span>

#### <a name="use-autoheal"></a><span data-ttu-id="92e4d-175">Elindulásáról használata</span><span class="sxs-lookup"><span data-stu-id="92e4d-175">Use AutoHeal</span></span>
<span data-ttu-id="92e4d-176">Elindulásáról (például a konfigurációs módosításokat, kérelmek, memória-alapú korlátok vagy egy kérelem végrehajtásához szükséges idő) kiválasztott beállítások alapján az alkalmazás munkavégző folyamata újraindul.</span><span class="sxs-lookup"><span data-stu-id="92e4d-176">AutoHeal recycles the worker process for your app based on settings you choose (like configuration changes, requests, memory-based limits, or the time needed to execute a request).</span></span> <span data-ttu-id="92e4d-177">Az esetek többségében a folyamat újrahasznosítását leggyorsabban úgy tudja elhárítani a problémát az.</span><span class="sxs-lookup"><span data-stu-id="92e4d-177">Most of the time, recycle the process is the fastest way to recover from a problem.</span></span> <span data-ttu-id="92e4d-178">Abban az esetben, ha a webes alkalmazás közvetlenül az Azure portálon belül mindig újraindíthatja, elindulásáról lesz automatikusan elvégezze ezt meg.</span><span class="sxs-lookup"><span data-stu-id="92e4d-178">Though you can always restart the web app from directly within the Azure Portal, AutoHeal will do it automatically for you.</span></span> <span data-ttu-id="92e4d-179">Végre kell hajtani mindössze néhány eseményindítók hozzáadása a webalkalmazás a legfelső szintű web.config fájlban.</span><span class="sxs-lookup"><span data-stu-id="92e4d-179">All you need to do is add some triggers in the root web.config for your web app.</span></span> <span data-ttu-id="92e4d-180">Vegye figyelembe, hogy ezek a beállítások csatlakoztatás működik ugyanúgy akkor is, ha az alkalmazás nem a .net, egyet.</span><span class="sxs-lookup"><span data-stu-id="92e4d-180">Note that these settings would work in the same way even if your application is not a .Net one.</span></span>

<span data-ttu-id="92e4d-181">További információkért lásd: [automatikus javítás Azure webhelyek](https://azure.microsoft.com/blog/auto-healing-windows-azure-web-sites/).</span><span class="sxs-lookup"><span data-stu-id="92e4d-181">For more information, see [Auto-Healing Azure Web Sites](https://azure.microsoft.com/blog/auto-healing-windows-azure-web-sites/).</span></span>

#### <a name="restart-the-web-app"></a><span data-ttu-id="92e4d-182">A webalkalmazás újraindítása</span><span class="sxs-lookup"><span data-stu-id="92e4d-182">Restart the web app</span></span>
<span data-ttu-id="92e4d-183">Ez gyakran az az egyszeri hibák legegyszerűbb módja.</span><span class="sxs-lookup"><span data-stu-id="92e4d-183">This is often the simplest way to recover from one-time issues.</span></span> <span data-ttu-id="92e4d-184">Az a [Azure Portal](https://portal.azure.com/), a webalkalmazás panelen, lehetősége van a leállítása, vagy indítsa újra az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="92e4d-184">On the [Azure Portal](https://portal.azure.com/), on your web app’s blade, you have the options to stop or restart your app.</span></span>

 ![Indítsa újra a hibás átjáró 502 és 503-as szolgáltatás nem érhető el a HTTP-hibák megoldására alkalmazást](./media/app-service-web-troubleshoot-HTTP-502-503/2-restart.png)

<span data-ttu-id="92e4d-186">A webalkalmazás Azure Powershell használatával is kezelheti.</span><span class="sxs-lookup"><span data-stu-id="92e4d-186">You can also manage your web app using Azure Powershell.</span></span> <span data-ttu-id="92e4d-187">További információ: [Az Azure PowerShell használata az Azure Resource Manager eszközzel](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="92e4d-187">For more information, see [Using Azure PowerShell with Azure Resource Manager](../powershell-azure-resource-manager.md).</span></span>

