---
title: "aaaSet mentése web app analytics az ASP.NET az Azure Application insights szolgáltatással |} Microsoft Docs"
description: "Konfigurálhatja a helyszínen vagy az Azure-ban üzemeltetett ASP.NET-webhely teljesítményét, rendelkezésre állását és használatelemzését."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: d0eee3c0-b328-448f-8123-f478052751db
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/15/2017
ms.author: bwren
ms.openlocfilehash: 61a3cdce68da48bfb9450b1d296acc1535f50a38
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-application-insights-for-your-aspnet-website"></a><span data-ttu-id="fdd84-103">Az Application Insights beállítása az ASP.NET-webhelyhez</span><span class="sxs-lookup"><span data-stu-id="fdd84-103">Set up Application Insights for your ASP.NET website</span></span>

<span data-ttu-id="fdd84-104">Ezzel az eljárással az ASP.NET web app toosend telemetriai toohello [Azure Application Insights](app-insights-overview.md) szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="fdd84-104">This procedure configures your ASP.NET web app toosend telemetry toohello [Azure Application Insights](app-insights-overview.md) service.</span></span> <span data-ttu-id="fdd84-105">A saját IIS-kiszolgálót vagy hello felhőben üzemeltetett ASP.NET alkalmazások esetében működik.</span><span class="sxs-lookup"><span data-stu-id="fdd84-105">It works for ASP.NET apps that are hosted either in your own IIS server or in hello Cloud.</span></span> <span data-ttu-id="fdd84-106">Diagramok kap, és egy hatékony lekérdező nyelv, amelyek segítenek megérteni hello az alkalmazás teljesítményével kapcsolatos és a felhasználók hogyan használják, valamint automatikus riasztások hibák vagy teljesítményproblémák.</span><span class="sxs-lookup"><span data-stu-id="fdd84-106">You get charts and a powerful query language that help you understand hello performance of your app and how people are using it, plus automatic alerts on failures or performance issues.</span></span> <span data-ttu-id="fdd84-107">Sok fejlesztők található ezeket a funkciókat kiváló, –, de is kiterjesztheti és testre szabhatja hello telemetriai adatokat, ha szeretné.</span><span class="sxs-lookup"><span data-stu-id="fdd84-107">Many developers find these features great as they are, but you can also extend and customize hello telemetry if you need to.</span></span>

<span data-ttu-id="fdd84-108">A telepítés mindössze néhány kattintással végrehajtható a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="fdd84-108">Setup takes just a few clicks in Visual Studio.</span></span> <span data-ttu-id="fdd84-109">Telemetriai adatok mennyisége hello korlátozásával hello beállítás tooavoid díjak rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="fdd84-109">You have hello option tooavoid charges by limiting hello volume of telemetry.</span></span> <span data-ttu-id="fdd84-110">Ez lehetővé teszi, hogy tooexperiment és hibakeresési, vagy a hely nem sok felhasználóval rendelkező toomonitor.</span><span class="sxs-lookup"><span data-stu-id="fdd84-110">This allows you tooexperiment and debug, or toomonitor a site with not many users.</span></span> <span data-ttu-id="fdd84-111">Ha mégis ezt szeretné azokat, amelyek toogo, és figyelheti a munkakörnyezeti helyet, később könnyen tooraise hello korlátot.</span><span class="sxs-lookup"><span data-stu-id="fdd84-111">When you decide you want toogo ahead and monitor your production site, it's easy tooraise hello limit later.</span></span>

## <a name="before-you-start"></a><span data-ttu-id="fdd84-112">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="fdd84-112">Before you start</span></span>
<span data-ttu-id="fdd84-113">A következők szükségesek:</span><span class="sxs-lookup"><span data-stu-id="fdd84-113">You need:</span></span>

* <span data-ttu-id="fdd84-114">Visual Studio 2013 3. frissítés vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="fdd84-114">Visual Studio 2013 update 3 or later.</span></span> <span data-ttu-id="fdd84-115">Az újabb jobb.</span><span class="sxs-lookup"><span data-stu-id="fdd84-115">Later is better.</span></span>
* <span data-ttu-id="fdd84-116">Előfizetés túl[Microsoft Azure](http://azure.com).</span><span class="sxs-lookup"><span data-stu-id="fdd84-116">A subscription too[Microsoft Azure](http://azure.com).</span></span> <span data-ttu-id="fdd84-117">Ha a csapat vagy szervezet Azure-előfizetéssel, hello tulajdonos használatával adhat hozzá, tooit, a [Microsoft-fiók](http://live.com).</span><span class="sxs-lookup"><span data-stu-id="fdd84-117">If your team or organization has an Azure subscription, hello owner can add you tooit, by using your [Microsoft account](http://live.com).</span></span>

<span data-ttu-id="fdd84-118">Ha érdekli, számos alternatív témakörök toolook:.</span><span class="sxs-lookup"><span data-stu-id="fdd84-118">There are alternative topics toolook at if you are interested in:</span></span>

* [<span data-ttu-id="fdd84-119">Webalkalmazás beállítása futási időben</span><span class="sxs-lookup"><span data-stu-id="fdd84-119">Instrumenting a web app at runtime</span></span>](app-insights-monitor-performance-live-website-now.md)
* [<span data-ttu-id="fdd84-120">Azure Cloud Services</span><span class="sxs-lookup"><span data-stu-id="fdd84-120">Azure Cloud Services</span></span>](app-insights-cloudservices.md)

## <span data-ttu-id="fdd84-121"><a name="ide"></a>1. lépés: Hello Application Insights SDK hozzáadása</span><span class="sxs-lookup"><span data-stu-id="fdd84-121"><a name="ide"></a> Step 1: Add hello Application Insights SDK</span></span>

<span data-ttu-id="fdd84-122">Kattintson a jobb gombbal a webalkalmazás-projektre a Solution Explorer (Megoldáskezelő) területén, és válassza az **Add** (Hozzáadás),  > **Application Insights Telemetry** (Application Insights-telemetria) vagy a **Configure Application Insights** (Application Insights konfigurálása) elemet.</span><span class="sxs-lookup"><span data-stu-id="fdd84-122">Right-click your web app project in Solution Explorer, and choose **Add** > **Application Insights Telemetry...** or **Configure Application Insights**.</span></span>

![A Solution Explorer (Megoldáskezelő) képernyőképe, a kiemelt Add Application Insights Telemetry (Application Insights telemetria hozzáadása) elemmel](./media/app-insights-asp-net/appinsights-03-addExisting.png)

<span data-ttu-id="fdd84-124">(A Visual Studio 2015, van továbbá egy beállítás tooadd Application Insights hello új projekt párbeszédpanelje.)</span><span class="sxs-lookup"><span data-stu-id="fdd84-124">(In Visual Studio 2015, there's also an option tooadd Application Insights in hello New Project dialog.)</span></span>

<span data-ttu-id="fdd84-125">Továbbra is az Application Insights-konfiguráció lapon toohello:</span><span class="sxs-lookup"><span data-stu-id="fdd84-125">Continue toohello Application Insights configuration page:</span></span>

![Képernyőkép az alkalmazásregisztrációs szakaszról az Application Insights oldalon](./media/app-insights-asp-net/visual-studio-register-dialog.png)

<span data-ttu-id="fdd84-127">**a.**</span><span class="sxs-lookup"><span data-stu-id="fdd84-127">**a.**</span></span> <span data-ttu-id="fdd84-128">Válassza ki a hello fiókja és -előfizetése tooaccess Azure használatát.</span><span class="sxs-lookup"><span data-stu-id="fdd84-128">Select hello account and subscription that you use tooaccess Azure.</span></span>

<span data-ttu-id="fdd84-129">**b.**</span><span class="sxs-lookup"><span data-stu-id="fdd84-129">**b.**</span></span> <span data-ttu-id="fdd84-130">Válassza ki a hello erőforrás ahová toosee hello adatokat az alkalmazásból az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="fdd84-130">Select hello resource in Azure where you want toosee hello data from your app.</span></span> <span data-ttu-id="fdd84-131">Általában:</span><span class="sxs-lookup"><span data-stu-id="fdd84-131">Usually:</span></span>

* <span data-ttu-id="fdd84-132">[Egyetlen erőforrást használjon](app-insights-monitor-multi-role-apps.md) egy alkalmazás különböző összetevőihez.</span><span class="sxs-lookup"><span data-stu-id="fdd84-132">Use a [single resource for different components](app-insights-monitor-multi-role-apps.md) of a single application.</span></span> 
* <span data-ttu-id="fdd84-133">Különálló erőforrásokhoz hozzon létre az egymáshoz nem kapcsolódó alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="fdd84-133">Create separate resources for unrelated applications.</span></span>
 
<span data-ttu-id="fdd84-134">Ha azt szeretné, hogy tooset hello erőforrás csoport vagy hello az adatok tárolási helye, kattintson a **beállításainak**.</span><span class="sxs-lookup"><span data-stu-id="fdd84-134">If you want tooset hello resource group or hello location where your data is stored, click **Configure settings**.</span></span> <span data-ttu-id="fdd84-135">Erőforráscsoportok használt toocontrol hozzáférés toodata.</span><span class="sxs-lookup"><span data-stu-id="fdd84-135">Resource groups are used toocontrol access toodata.</span></span> <span data-ttu-id="fdd84-136">Például, ha több alkalmazást részét képező hello azonos rendszer, akkor előfordulhat, hogy be az Application Insights adatainak hello ugyanabban az erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="fdd84-136">For example, if you have several apps that form part of hello same system, you might put their Application Insights data in hello same resource group.</span></span>

<span data-ttu-id="fdd84-137">**c.**</span><span class="sxs-lookup"><span data-stu-id="fdd84-137">**c.**</span></span> <span data-ttu-id="fdd84-138">Állítsa be a cap hello szabad adatok mennyiség korlátozása, tooavoid díjakat.</span><span class="sxs-lookup"><span data-stu-id="fdd84-138">Set a cap at hello free data volume limit, tooavoid charges.</span></span> <span data-ttu-id="fdd84-139">Az Application Insights szabadítson fel tooa telemetriai adatok mennyiségét.</span><span class="sxs-lookup"><span data-stu-id="fdd84-139">Application Insights is free up tooa certain volume of telemetry.</span></span> <span data-ttu-id="fdd84-140">Hello erőforrás létrehozása után módosíthatja a kiválasztott hello portálon megnyitása **szolgáltatások + díjszabás** > **adatok kötetkezelés** > **napi kötet cap**.</span><span class="sxs-lookup"><span data-stu-id="fdd84-140">After hello resource is created, you can change your selection in hello portal by opening  **Features + pricing** > **Data volume management** > **Daily volume cap**.</span></span>

<span data-ttu-id="fdd84-141">**d.**</span><span class="sxs-lookup"><span data-stu-id="fdd84-141">**d.**</span></span> <span data-ttu-id="fdd84-142">Kattintson a **regisztrálása** azokat, amelyek toogo és konfigurálhatja az Application Insights a webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="fdd84-142">Click **Register** toogo ahead and configure Application Insights for your web app.</span></span> <span data-ttu-id="fdd84-143">Telemetriai adatokat küld toohello [Azure-portálon](https://portal.azure.com), hibakeresési és az alkalmazás közzétételét követően.</span><span class="sxs-lookup"><span data-stu-id="fdd84-143">Telemetry will be sent toohello [Azure portal](https://portal.azure.com), both during debugging and after you have published your app.</span></span>

<span data-ttu-id="fdd84-144">**e.**</span><span class="sxs-lookup"><span data-stu-id="fdd84-144">**e.**</span></span> <span data-ttu-id="fdd84-145">Ha toosend telemetriai toohello portál nem szeretné, hogy hibakeresése közben, csak adja hozzá a hello Application Insights SDK tooyour app, de ne állítson be egy erőforrás hello portálon.</span><span class="sxs-lookup"><span data-stu-id="fdd84-145">If you don't want toosend telemetry toohello portal while you're debugging, just add hello Application Insights SDK tooyour app but don't configure a resource in hello portal.</span></span> <span data-ttu-id="fdd84-146">A Visual Studio képes toosee telemetriai közben a rendszer hibakeresési lesz.</span><span class="sxs-lookup"><span data-stu-id="fdd84-146">You will be able toosee telemetry in Visual Studio while you are debugging.</span></span> <span data-ttu-id="fdd84-147">Később, toothis konfigurálása lapon lépjen vissza, vagy sikerült megvárni az alkalmazás telepítése után, és [futási időben a telemetria kapcsoló](app-insights-monitor-performance-live-website-now.md).</span><span class="sxs-lookup"><span data-stu-id="fdd84-147">Later, you can return toothis configuration page, or you could wait until after you have deployed your app and [switch on telemetry at run time](app-insights-monitor-performance-live-website-now.md).</span></span>


## <span data-ttu-id="fdd84-148"><a name="run"></a> 2. lépés: Az alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="fdd84-148"><a name="run"></a> Step 2: Run your app</span></span>
<span data-ttu-id="fdd84-149">Futtassa az alkalmazást az F5 billentyűvel.</span><span class="sxs-lookup"><span data-stu-id="fdd84-149">Run your app with F5.</span></span> <span data-ttu-id="fdd84-150">Nyissa meg a különböző oldalakhoz toogenerate néhány telemetriai adatokat.</span><span class="sxs-lookup"><span data-stu-id="fdd84-150">Open different pages toogenerate some telemetry.</span></span>

<span data-ttu-id="fdd84-151">A Visual Studio alkalmazásban lásd: hello naplózott események számát.</span><span class="sxs-lookup"><span data-stu-id="fdd84-151">In Visual Studio, you see a count of hello events that have been logged.</span></span>

![A Visual Studio képernyőképe.](./media/app-insights-asp-net/54.png)

## <a name="step-3-see-your-telemetry"></a><span data-ttu-id="fdd84-154">3. lépés: A telemetria megtekintése</span><span class="sxs-lookup"><span data-stu-id="fdd84-154">Step 3: See your telemetry</span></span>
<span data-ttu-id="fdd84-155">A telemetriai adatokat, a Visual Studio vagy hello Application Insights webes portálon tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="fdd84-155">You can see your telemetry either in Visual Studio or in hello Application Insights web portal.</span></span> <span data-ttu-id="fdd84-156">Telemetriai adatok keresése az alkalmazás hibakeresése a Visual Studio toohelp.</span><span class="sxs-lookup"><span data-stu-id="fdd84-156">Search telemetry in Visual Studio toohelp you debug your app.</span></span> <span data-ttu-id="fdd84-157">Ha a rendszer élő, figyelheti a teljesítmény- és használati hello web portal alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="fdd84-157">Monitor performance and usage in hello web portal when your system is live.</span></span> 

### <a name="see-your-telemetry-in-visual-studio"></a><span data-ttu-id="fdd84-158">Telemetria megtekintése a Visual Studióban</span><span class="sxs-lookup"><span data-stu-id="fdd84-158">See your telemetry in Visual Studio</span></span>

<span data-ttu-id="fdd84-159">A Visual Studióban nyissa meg a hello Application Insights ablakot.</span><span class="sxs-lookup"><span data-stu-id="fdd84-159">In Visual Studio, open hello Application Insights window.</span></span> <span data-ttu-id="fdd84-160">Vagy kattintson a hello **Application Insights** gombra, vagy kattintson a jobb gombbal a projektre a Megoldáskezelőben, válassza ki **Application Insights**, és kattintson a **keresési élő Telemetriai**.</span><span class="sxs-lookup"><span data-stu-id="fdd84-160">Either click hello **Application Insights** button, or right-click your project in Solution Explorer, select **Application Insights**, and then click **Search Live Telemetry**.</span></span>

<span data-ttu-id="fdd84-161">Hello Visual Studio Application Insights keresési ablak, tekintse meg a hello **hibakeresési munkamenet adatait** hello kiszolgálóoldali az alkalmazás hozott létre telemetriai nézetet.</span><span class="sxs-lookup"><span data-stu-id="fdd84-161">In hello Visual Studio Application Insights Search window, see hello **Data from Debug session** view for telemetry generated in hello server side of your app.</span></span> <span data-ttu-id="fdd84-162">Hello szűrők kísérletezhet, és kattintson a bármely esemény toosee további információkhoz juthat.</span><span class="sxs-lookup"><span data-stu-id="fdd84-162">Experiment with hello filters, and click any event toosee more detail.</span></span>

![Képernyőfelvétel a hello hibakeresési munkamenetben adatnézet hello Application Insights ablakban.](./media/app-insights-asp-net/55.png)

> [!NOTE]
> <span data-ttu-id="fdd84-164">Ha nem lát adatokat, győződjön meg arról, hello időtartomány helyességéről, majd kattintson a hello keresés ikonra.</span><span class="sxs-lookup"><span data-stu-id="fdd84-164">If you don't see any data, make sure hello time range is correct, and click hello Search icon.</span></span>

<span data-ttu-id="fdd84-165">[További tudnivalók az Application Insights-eszközökről a Visual Studióban](app-insights-visual-studio.md).</span><span class="sxs-lookup"><span data-stu-id="fdd84-165">[Learn more about Application Insights tools in Visual Studio](app-insights-visual-studio.md).</span></span>

<a name="monitor"></a>
### <a name="see-telemetry-in-web-portal"></a><span data-ttu-id="fdd84-166">Telemetria megtekintése a webportálon</span><span class="sxs-lookup"><span data-stu-id="fdd84-166">See telemetry in web portal</span></span>

<span data-ttu-id="fdd84-167">Telemetria hello Application Insights webes portálon megtekintheti (kivéve, ha úgy döntött, hogy tooinstall csak hello SDK) is.</span><span class="sxs-lookup"><span data-stu-id="fdd84-167">You can also see telemetry in hello Application Insights web portal (unless you chose tooinstall only hello SDK).</span></span> <span data-ttu-id="fdd84-168">hello portal további diagramokat, elemzési eszközök és kereszt-összetevő nézetek, mint a Visual Studio rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="fdd84-168">hello portal has more charts, analytic tools, and cross-component views than Visual Studio.</span></span> <span data-ttu-id="fdd84-169">hello portal riasztást küld.</span><span class="sxs-lookup"><span data-stu-id="fdd84-169">hello portal also provides alerts.</span></span>

<span data-ttu-id="fdd84-170">Nyissa meg az Application Insights-erőforrást.</span><span class="sxs-lookup"><span data-stu-id="fdd84-170">Open your Application Insights resource.</span></span> <span data-ttu-id="fdd84-171">Vagy bejelentkezés toohello [Azure-portálon](https://portal.azure.com/) és a Visual Studio van, vagy kattintson a jobb gombbal hello projekt keresse meg és azt nem léphet.</span><span class="sxs-lookup"><span data-stu-id="fdd84-171">Either sign in toohello [Azure portal](https://portal.azure.com/) and find it there, or right-click hello project in Visual Studio, and let it take you there.</span></span>

![Képernyőfelvétel a Visual Studio, hogyan tooopen hello Application Insights portál megjelenítése](./media/app-insights-asp-net/appinsights-04-openPortal.png)

> [!NOTE]
> <span data-ttu-id="fdd84-173">Ha a hozzáférési hibaüzenetet kap: rendelkezik egynél több Microsoft hitelesítőadat-készletet, és van jelentkezett be hello megfelelő készletet?</span><span class="sxs-lookup"><span data-stu-id="fdd84-173">If you get an access error: Do you have more than one set of Microsoft credentials, and are you signed in with hello wrong set?</span></span> <span data-ttu-id="fdd84-174">Hello portálon jelentkezzen ki, és jelentkezzen be újra.</span><span class="sxs-lookup"><span data-stu-id="fdd84-174">In hello portal, sign out and sign in again.</span></span>

<span data-ttu-id="fdd84-175">hello portál megnyitja egy nézeten hello telemetriai adatot az alkalmazásból.</span><span class="sxs-lookup"><span data-stu-id="fdd84-175">hello portal opens on a view of hello telemetry from your app.</span></span>

![Az Application Insights áttekintési oldalának képernyőképe](./media/app-insights-asp-net/66.png)

<span data-ttu-id="fdd84-177">Hello portálon kattintson a csempe vagy diagram toosee további információkhoz juthat.</span><span class="sxs-lookup"><span data-stu-id="fdd84-177">In hello portal, click any tile or chart toosee more detail.</span></span>

<span data-ttu-id="fdd84-178">[További információ az Application Insights az Azure-portálon hello](app-insights-dashboards.md).</span><span class="sxs-lookup"><span data-stu-id="fdd84-178">[Learn more about using Application Insights in hello Azure portal](app-insights-dashboards.md).</span></span>

## <a name="step-4-publish-your-app"></a><span data-ttu-id="fdd84-179">4. lépés: Az alkalmazás közzététele</span><span class="sxs-lookup"><span data-stu-id="fdd84-179">Step 4: Publish your app</span></span>
<span data-ttu-id="fdd84-180">Az alkalmazás tooyour IIS-kiszolgálót vagy tooAzure közzététele.</span><span class="sxs-lookup"><span data-stu-id="fdd84-180">Publish your app tooyour IIS server or tooAzure.</span></span> <span data-ttu-id="fdd84-181">Figyelési [metrikák élő adatfolyam](app-insights-metrics-explorer.md#live-metrics-stream) toomake, minden fut-e zökkenőmentesen működjön.</span><span class="sxs-lookup"><span data-stu-id="fdd84-181">Watch [Live Metrics Stream](app-insights-metrics-explorer.md#live-metrics-stream) toomake sure everything is running smoothly.</span></span>

<span data-ttu-id="fdd84-182">A telemetriai adatok épít fel hello Application Insights portálon, ahol metrikát, a telemetriai adatok keresése és állítson be [irányítópultok](app-insights-dashboards.md).</span><span class="sxs-lookup"><span data-stu-id="fdd84-182">Your telemetry builds up in hello Application Insights portal, where you can monitor metrics, search your telemetry, and set up [dashboards](app-insights-dashboards.md).</span></span> <span data-ttu-id="fdd84-183">Is használhatja hello hatékony [Log Analytics lekérdezési nyelv](https://docs.loganalytics.io/) tooanalyze használati és a teljesítmény vagy a toofind adott események.</span><span class="sxs-lookup"><span data-stu-id="fdd84-183">You can also use hello powerful [Log Analytics query language](https://docs.loganalytics.io/) tooanalyze usage and performance, or toofind specific events.</span></span>

<span data-ttu-id="fdd84-184">Is folytathatja a tooanalyze az a telemetriai [Visual Studio](app-insights-visual-studio.md), az eszközöket, például a diagnosztikai keresési és [trendek](app-insights-visual-studio-trends.md).</span><span class="sxs-lookup"><span data-stu-id="fdd84-184">You can also continue tooanalyze your telemetry in [Visual Studio](app-insights-visual-studio.md), with tools such as diagnostic search and [trends](app-insights-visual-studio-trends.md).</span></span>

> [!NOTE]
> <span data-ttu-id="fdd84-185">Ha az alkalmazás küld elég telemetriai tooapproach hello [szabályozás korlátok](app-insights-pricing.md#limits-summary), az automatikus [mintavételi](app-insights-sampling.md) kapcsolókat.</span><span class="sxs-lookup"><span data-stu-id="fdd84-185">If your app sends enough telemetry tooapproach hello [throttling limits](app-insights-pricing.md#limits-summary), automatic [sampling](app-insights-sampling.md) switches on.</span></span> <span data-ttu-id="fdd84-186">Mintavételi diagnosztikai célokra kapcsolódó adatok megőrzésével az alkalmazásból küldött telemetriai hello mennyiségét csökkenti.</span><span class="sxs-lookup"><span data-stu-id="fdd84-186">Sampling reduces hello quantity of telemetry sent from your app, while preserving correlated data for diagnostic purposes.</span></span>
>
>

## <span data-ttu-id="fdd84-187"><a name="land"></a>Készen is van.</span><span class="sxs-lookup"><span data-stu-id="fdd84-187"><a name="land"></a> You're all set</span></span>

<span data-ttu-id="fdd84-188">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="fdd84-188">Congratulations!</span></span> <span data-ttu-id="fdd84-189">Hello Application Insights csomag telepítése az alkalmazásban, és konfigurálta az Azure toosend telemetriai toohello Application Insights szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="fdd84-189">You installed hello Application Insights package in your app, and configured it toosend telemetry toohello Application Insights service on Azure.</span></span>

![A telemetria mozgásának ábrája](./media/app-insights-asp-net/01-scheme.png)

<span data-ttu-id="fdd84-191">hello Azure-erőforrás, amely megkapja az alkalmazás telemetriai azonosít egy *instrumentation kulcs*.</span><span class="sxs-lookup"><span data-stu-id="fdd84-191">hello Azure resource that receives your app's telemetry is identified by an *instrumentation key*.</span></span> <span data-ttu-id="fdd84-192">Ez a kulcs hello ApplicationInsights.config fájlban talál.</span><span class="sxs-lookup"><span data-stu-id="fdd84-192">You'll find this key in hello ApplicationInsights.config file.</span></span>


## <a name="upgrade-toofuture-sdk-versions"></a><span data-ttu-id="fdd84-193">Toofuture SDK verziók frissítése</span><span class="sxs-lookup"><span data-stu-id="fdd84-193">Upgrade toofuture SDK versions</span></span>
<span data-ttu-id="fdd84-194">tooupgrade tooa [hello SDK új kiadása](https://github.com/Microsoft/ApplicationInsights-dotnet-server/releases), nyissa meg hello **NuGet-Csomagkezelő** újra, és a telepített csomagok szűrőt.</span><span class="sxs-lookup"><span data-stu-id="fdd84-194">tooupgrade tooa [new release of hello SDK](https://github.com/Microsoft/ApplicationInsights-dotnet-server/releases), open hello **NuGet package manager** again, and filter on installed packages.</span></span> <span data-ttu-id="fdd84-195">Jelölje ki a **Microsoft.ApplicationInsights.Web** lehetőséget, és válassza a **Frissítés** elemet.</span><span class="sxs-lookup"><span data-stu-id="fdd84-195">Select **Microsoft.ApplicationInsights.Web**, and choose **Upgrade**.</span></span>

<span data-ttu-id="fdd84-196">Ha végzett a testreszabások tooApplicationInsights.config, mentse egy példányát frissítése előtt.</span><span class="sxs-lookup"><span data-stu-id="fdd84-196">If you made any customizations tooApplicationInsights.config, save a copy of it before you upgrade.</span></span> <span data-ttu-id="fdd84-197">A módosításokat, majd egyesítése hello új verziója.</span><span class="sxs-lookup"><span data-stu-id="fdd84-197">Then, merge your changes into hello new version.</span></span>

## <a name="video"></a><span data-ttu-id="fdd84-198">Videó</span><span class="sxs-lookup"><span data-stu-id="fdd84-198">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a><span data-ttu-id="fdd84-199">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fdd84-199">Next steps</span></span>

### <a name="more-telemetry"></a><span data-ttu-id="fdd84-200">További telemetria</span><span class="sxs-lookup"><span data-stu-id="fdd84-200">More telemetry</span></span>

* <span data-ttu-id="fdd84-201">**[Böngésző és oldalbetöltési adatok](app-insights-javascript.md)** – Kódrészlet beszúrása a weboldalakra.</span><span class="sxs-lookup"><span data-stu-id="fdd84-201">**[Browser and page load data](app-insights-javascript.md)** - Insert a code snippet in your web pages.</span></span>
* <span data-ttu-id="fdd84-202">**[Részletesebb függőség- és kivételfigyelés](app-insights-monitor-performance-live-website-now.md)** – Állapotfigyelő telepítése a kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="fdd84-202">**[Get more detailed dependency and exception monitoring](app-insights-monitor-performance-live-website-now.md)** - Install Status Monitor on your server.</span></span>
* <span data-ttu-id="fdd84-203">**[Egyéni események Code](app-insights-api-custom-events-metrics.md)**  toocount, time, vagy felhasználói műveletek mérését.</span><span class="sxs-lookup"><span data-stu-id="fdd84-203">**[Code custom events](app-insights-api-custom-events-metrics.md)** toocount, time, or measure user actions.</span></span>
* <span data-ttu-id="fdd84-204">**[Naplóadatok lekérése](app-insights-asp-net-trace-logs.md)** – Naplóadatok összevetése a telemetriával.</span><span class="sxs-lookup"><span data-stu-id="fdd84-204">**[Get log data](app-insights-asp-net-trace-logs.md)** - Correlate log data with your telemetry.</span></span>

### <a name="analysis"></a><span data-ttu-id="fdd84-205">Elemzés</span><span class="sxs-lookup"><span data-stu-id="fdd84-205">Analysis</span></span>

* <span data-ttu-id="fdd84-206">**[Az Application Insights használata a Visual Studióban](app-insights-visual-studio.md)**</span><span class="sxs-lookup"><span data-stu-id="fdd84-206">**[Working with Application Insights in Visual Studio](app-insights-visual-studio.md)**</span></span><br/><span data-ttu-id="fdd84-207">Telemetriai adatokat a hibakeresési információkat tartalmaz diagnosztikai keresési, valamint lefúrhat toocode keresztül.</span><span class="sxs-lookup"><span data-stu-id="fdd84-207">Includes information about debugging with telemetry, diagnostic search, and drill through toocode.</span></span>
* <span data-ttu-id="fdd84-208">**[Hello Application Insights portál használata](app-insights-dashboards.md)**</span><span class="sxs-lookup"><span data-stu-id="fdd84-208">**[Working with hello Application Insights portal](app-insights-dashboards.md)**</span></span><br/> <span data-ttu-id="fdd84-209">Az irányítópultokkal, a hatékony diagnosztikai és elemzési eszközökkel, riasztásokkal, az alkalmazás élő függőségi térképével, valamint a telemetria exportálásával kapcsolatos információkat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="fdd84-209">Includes information about dashboards, powerful diagnostic and analytic tools, alerts, a live dependency map of your application, and telemetry export.</span></span>
* <span data-ttu-id="fdd84-210">**[Elemzés](app-insights-analytics-tour.md)**  -hello hatékony lekérdezési nyelv.</span><span class="sxs-lookup"><span data-stu-id="fdd84-210">**[Analytics](app-insights-analytics-tour.md)** - hello powerful query language.</span></span>

### <a name="alerts"></a><span data-ttu-id="fdd84-211">Riasztások</span><span class="sxs-lookup"><span data-stu-id="fdd84-211">Alerts</span></span>

* <span data-ttu-id="fdd84-212">[Rendelkezésreállás figyelésére szolgáló tesztek](app-insights-monitor-web-app-availability.md): tesztek létrehozása, hogy a hely látható hello Web toomake.</span><span class="sxs-lookup"><span data-stu-id="fdd84-212">[Availability tests](app-insights-monitor-web-app-availability.md): Create tests toomake sure your site is visible on hello web.</span></span>
* <span data-ttu-id="fdd84-213">[Diagnosztika intelligens](app-insights-proactive-diagnostics.md): ezek a tesztek automatikusan fut, így nem kell toodo semmi tooset be őket.</span><span class="sxs-lookup"><span data-stu-id="fdd84-213">[Smart diagnostics](app-insights-proactive-diagnostics.md): These tests run automatically, so you don't have toodo anything tooset them up.</span></span> <span data-ttu-id="fdd84-214">Értesítést kap, ha az alkalmazásában szokatlanul magas a meghiúsult kérelmek száma.</span><span class="sxs-lookup"><span data-stu-id="fdd84-214">They tell you if your app has an unusual rate of failed requests.</span></span>
* <span data-ttu-id="fdd84-215">[Metrika riasztások](app-insights-alerts.md): állítsa be ezeket a toowarn, ha egy metrika a küszöbérték keverve használ.</span><span class="sxs-lookup"><span data-stu-id="fdd84-215">[Metric alerts](app-insights-alerts.md): Set these toowarn you if a metric crosses a threshold.</span></span> <span data-ttu-id="fdd84-216">Az alkalmazás kódjába beépített egyedi metrikákhoz is állíthat be riasztásokat.</span><span class="sxs-lookup"><span data-stu-id="fdd84-216">You can set them on custom metrics that you code into your app.</span></span>

### <a name="automation"></a><span data-ttu-id="fdd84-217">Automatizálás</span><span class="sxs-lookup"><span data-stu-id="fdd84-217">Automation</span></span>

* [<span data-ttu-id="fdd84-218">Application Insights-erőforrások létrehozásának automatizálása</span><span class="sxs-lookup"><span data-stu-id="fdd84-218">Automate creating an Application Insights resource</span></span>](app-insights-powershell.md)
