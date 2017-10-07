---
title: "aaaMonitor egy SharePoint-webhely, az Application insights szolgáltatással"
description: "Új alkalmazás figyelésének megkezdése új kialakítási kulccsal"
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 2bfe5910-d673-4cf6-a5c1-4c115eae1be0
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/24/2016
ms.author: bwren
ms.openlocfilehash: acfe99c24a4d77daec1017de0442ec952a1faba2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-a-sharepoint-site-with-application-insights"></a><span data-ttu-id="55038-103">SharePoint-hely megfigyelése az Application Insights segítségével</span><span class="sxs-lookup"><span data-stu-id="55038-103">Monitor a SharePoint site with Application Insights</span></span>
<span data-ttu-id="55038-104">Az Azure Application Insights hello rendelkezésre állási-, teljesítmény- és az alkalmazások használati figyeli.</span><span class="sxs-lookup"><span data-stu-id="55038-104">Azure Application Insights monitors hello availability, performance and usage of your apps.</span></span> <span data-ttu-id="55038-105">Itt megtudhatja, hogyan tooset azt feliratkozott egy SharePoint-webhelyen.</span><span class="sxs-lookup"><span data-stu-id="55038-105">Here you'll learn how tooset it up for a SharePoint site.</span></span>

## <a name="create-an-application-insights-resource"></a><span data-ttu-id="55038-106">Application Insights-erőforrás létrehozása</span><span class="sxs-lookup"><span data-stu-id="55038-106">Create an Application Insights resource</span></span>
<span data-ttu-id="55038-107">A hello [Azure-portálon](https://portal.azure.com), hozzon létre egy új Application Insights-erőforrást.</span><span class="sxs-lookup"><span data-stu-id="55038-107">In hello [Azure portal](https://portal.azure.com), create a new Application Insights resource.</span></span> <span data-ttu-id="55038-108">Válassza ki az ASP.NET hello alkalmazás típusként.</span><span class="sxs-lookup"><span data-stu-id="55038-108">Choose ASP.NET as hello application type.</span></span>

![Kattintson a Tulajdonságok parancsra, válassza ki a hello kulcs, használja a ctrl + C](./media/app-insights-sharepoint/01-new.png)

<span data-ttu-id="55038-110">hello panel, amely megnyitja a hello hely, ahol látni fogja, teljesítmény- és használati adatokat az alkalmazásra vonatkozó.</span><span class="sxs-lookup"><span data-stu-id="55038-110">hello blade that opens is hello place where you'll see performance and usage data about your app.</span></span> <span data-ttu-id="55038-111">tooget hátsó tooit tooAzure, következő bejelentkezéskor látnia kell egy csempe az kifejezésre a kezdőképernyőn hello.</span><span class="sxs-lookup"><span data-stu-id="55038-111">tooget back tooit next time you login tooAzure, you should find a tile for it on hello start screen.</span></span> <span data-ttu-id="55038-112">Másik lehetőségként kattintson a Tallózás toofind azt.</span><span class="sxs-lookup"><span data-stu-id="55038-112">Alternatively click Browse toofind it.</span></span>

## <a name="add-our-script-tooyour-web-pages"></a><span data-ttu-id="55038-113">A parancsfájl tooyour weblapok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="55038-113">Add our script tooyour web pages</span></span>
<span data-ttu-id="55038-114">A gyors üzembe helyezés hello parancsfájl weblapok érhető el:</span><span class="sxs-lookup"><span data-stu-id="55038-114">In Quick Start, get hello script for web pages:</span></span>

![](./media/app-insights-sharepoint/02-monitor-web-page.png)

<span data-ttu-id="55038-115">Hello előtt hello parancsprogram beszúrása &lt;/head&gt; címke tootrack kívánt minden oldalon.</span><span class="sxs-lookup"><span data-stu-id="55038-115">Insert hello script just before hello &lt;/head&gt; tag of every page you want tootrack.</span></span> <span data-ttu-id="55038-116">Ha a webhely mesterlapra, nem adhat meg hello parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="55038-116">If your website has a master page, you can put hello script there.</span></span> <span data-ttu-id="55038-117">Egy ASP.NET MVC-projektben a következő helyre helyezné a szkriptet: View\Shared\_Layout.cshtml</span><span class="sxs-lookup"><span data-stu-id="55038-117">For example, in an ASP.NET MVC project, you'd put it in View\Shared\_Layout.cshtml</span></span>

<span data-ttu-id="55038-118">hello hello instrumentation kulcs, amely arra utasítja a hello telemetriai tooyour Application Insights-erőforrást tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="55038-118">hello script contains hello instrumentation key that directs hello telemetry tooyour Application Insights resource.</span></span>

### <a name="add-hello-code-tooyour-site-pages"></a><span data-ttu-id="55038-119">Hello kódlapok tooyour webhely hozzáadása</span><span class="sxs-lookup"><span data-stu-id="55038-119">Add hello code tooyour site pages</span></span>
#### <a name="on-hello-master-page"></a><span data-ttu-id="55038-120">A hello mesterlap</span><span class="sxs-lookup"><span data-stu-id="55038-120">On hello master page</span></span>
<span data-ttu-id="55038-121">Ha hello hely mesterlap szerkesztéséhez, amely szerepel figyelésének hello webhely összes lapjára.</span><span class="sxs-lookup"><span data-stu-id="55038-121">If you can edit hello site's master page, that will provide monitoring for every page in hello site.</span></span>

<span data-ttu-id="55038-122">Tekintse meg a hello mesterlap és szerkeszthetők a SharePoint Designer vagy bármely más szerkesztő.</span><span class="sxs-lookup"><span data-stu-id="55038-122">Check out hello master page and edit it using SharePoint Designer or any other editor.</span></span>

![](./media/app-insights-sharepoint/03-master.png)

<span data-ttu-id="55038-123">Adja hozzá hello kódot hello előtt </head> címke.</span><span class="sxs-lookup"><span data-stu-id="55038-123">Add hello code just before hello </head> tag.</span></span> 

![](./media/app-insights-sharepoint/04-code.png)

#### <a name="or-on-individual-pages"></a><span data-ttu-id="55038-124">Vagy önálló lapokon</span><span class="sxs-lookup"><span data-stu-id="55038-124">Or on individual pages</span></span>
<span data-ttu-id="55038-125">toomonitor lap van, korlátozott számú hello parancsfájl külön-külön hozzáadása tooeach lap.</span><span class="sxs-lookup"><span data-stu-id="55038-125">toomonitor a limited set of pages, add hello script separately tooeach page.</span></span> 

<span data-ttu-id="55038-126">Kijelző beszúrása és hello kódrészletet beágyazása.</span><span class="sxs-lookup"><span data-stu-id="55038-126">Insert a web part and embed hello code snippet in it.</span></span>

![](./media/app-insights-sharepoint/05-page.png)

## <a name="view-data-about-your-app"></a><span data-ttu-id="55038-127">Alkalmazás adatainak megtekintése</span><span class="sxs-lookup"><span data-stu-id="55038-127">View data about your app</span></span>
<span data-ttu-id="55038-128">Helyezze ismét üzembe alkalmazását.</span><span class="sxs-lookup"><span data-stu-id="55038-128">Redeploy your app.</span></span>

<span data-ttu-id="55038-129">Visszatérési tooyour alkalmazás paneljén hello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="55038-129">Return tooyour application blade in hello [Azure portal](https://portal.azure.com).</span></span>

<span data-ttu-id="55038-130">hello első események keresési fog megjelenni.</span><span class="sxs-lookup"><span data-stu-id="55038-130">hello first events will appear in Search.</span></span> 

![](./media/app-insights-sharepoint/09-search.png)

<span data-ttu-id="55038-131">Ha több adatot vár, néhány másodperc elteltével kattintson a Frissítés elemre.</span><span class="sxs-lookup"><span data-stu-id="55038-131">Click Refresh after a few seconds if you're expecting more data.</span></span>

<span data-ttu-id="55038-132">Hello áttekintése paneljén kattintson **használatelemzés** toosee toocharts a felhasználók, a munkamenetek és az oldalmegtekintéseket:</span><span class="sxs-lookup"><span data-stu-id="55038-132">From hello overview blade, click **Usage analytics** toosee toocharts of users, sessions and page views:</span></span>

![](./media/app-insights-sharepoint/06-usage.png)

<span data-ttu-id="55038-133">Kattintson a diagram toosee további részletekért – például a lapmegtekintések:</span><span class="sxs-lookup"><span data-stu-id="55038-133">Click any chart toosee more details - for example Page Views:</span></span>

![](./media/app-insights-sharepoint/07-pages.png)

<span data-ttu-id="55038-134">Vagy a Felhasználók diagram esetében:</span><span class="sxs-lookup"><span data-stu-id="55038-134">Or Users:</span></span>

![](./media/app-insights-sharepoint/08-users.png)

## <a name="capturing-user-id"></a><span data-ttu-id="55038-135">Felhasználói azonosító rögzítése</span><span class="sxs-lookup"><span data-stu-id="55038-135">Capturing User Id</span></span>
<span data-ttu-id="55038-136">hello szabványos weblap kódrészletet nem rögzíti a hello felhasználói azonosító a SharePoint, azonban lehetőség van, amely egy kis módosítással.</span><span class="sxs-lookup"><span data-stu-id="55038-136">hello standard web page code snippet doesn't capture hello user id from SharePoint, but you can do that with a small modification.</span></span>

1. <span data-ttu-id="55038-137">Az alkalmazás instrumentation kulcs másolása hello Essentials legördülő az Application insights szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="55038-137">Copy your app's instrumentation key from hello Essentials drop-down in Application Insights.</span></span> 

    ![](./media/app-insights-sharepoint/02-props.png)

1. <span data-ttu-id="55038-138">Az alábbi hello részlet "XXXX" hello instrumentation kulcs helyettesítse be.</span><span class="sxs-lookup"><span data-stu-id="55038-138">Substitute hello instrumentation key for 'XXXX' in hello snippet below.</span></span> 
2. <span data-ttu-id="55038-139">Hello parancsfájl beágyazása a SharePoint-alkalmazásokban beszerzése hello portálról hello részlet helyett.</span><span class="sxs-lookup"><span data-stu-id="55038-139">Embed hello script in your SharePoint app instead of hello snippet you get from hello portal.</span></span>

```


<SharePoint:ScriptLink ID="ScriptLink1" name="SP.js" runat="server" localizable="false" loadafterui="true" /> 
<SharePoint:ScriptLink ID="ScriptLink2" name="SP.UserProfiles.js" runat="server" localizable="false" loadafterui="true" /> 

<script type="text/javascript"> 
var personProperties; 

// Ensure that hello SP.UserProfiles.js file is loaded before hello custom code runs. 
SP.SOD.executeOrDelayUntilScriptLoaded(getUserProperties, 'SP.UserProfiles.js'); 

function getUserProperties() { 
    // Get hello current client context and PeopleManager instance. 
    var clientContext = new SP.ClientContext.get_current(); 
    var peopleManager = new SP.UserProfiles.PeopleManager(clientContext); 

    // Get user properties for hello target user. 
    // tooget hello PersonProperties object for hello current user, use hello 
    // getMyProperties method. 

    personProperties = peopleManager.getMyProperties(); 

    // Load hello PersonProperties object and send hello request. 
    clientContext.load(personProperties); 
    clientContext.executeQueryAsync(onRequestSuccess, onRequestFail); 
} 

// This function runs if hello executeQueryAsync call succeeds. 
function onRequestSuccess() { 
var appInsights=window.appInsights||function(config){
function s(config){t[config]=function(){var i=arguments;t.queue.push(function(){t[config].apply(t,i)})}}var t={config:config},r=document,f=window,e="script",o=r.createElement(e),i,u;for(o.src=config.url||"//az416426.vo.msecnd.net/scripts/a/ai.0.js",r.getElementsByTagName(e)[0].parentNode.appendChild(o),t.cookie=r.cookie,t.queue=[],i=["Event","Exception","Metric","PageView","Trace"];i.length;)s("track"+i.pop());return config.disableExceptionTracking||(i="onerror",s("_"+i),u=f[i],f[i]=function(config,r,f,e,o){var s=u&&u(config,r,f,e,o);return s!==!0&&t["_"+i](config,r,f,e,o),s}),t
    }({
        instrumentationKey:"XXXX"
    });
    window.appInsights=appInsights;
    appInsights.trackPageView(document.title,window.location.href, {User: personProperties.get_displayName()});
} 

// This function runs if hello executeQueryAsync call fails. 
function onRequestFail(sender, args) { 
} 
</script> 


```



## <a name="next-steps"></a><span data-ttu-id="55038-140">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="55038-140">Next Steps</span></span>
* <span data-ttu-id="55038-141">[Webalkalmazás-tesztek](app-insights-monitor-web-app-availability.md) toomonitor hello a hely rendelkezésre állását.</span><span class="sxs-lookup"><span data-stu-id="55038-141">[Web tests](app-insights-monitor-web-app-availability.md) toomonitor hello availability of your site.</span></span>
* <span data-ttu-id="55038-142">[Application Insights](app-insights-overview.md) más típusú alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="55038-142">[Application Insights](app-insights-overview.md) for other types of app.</span></span>

<!--Link references-->


