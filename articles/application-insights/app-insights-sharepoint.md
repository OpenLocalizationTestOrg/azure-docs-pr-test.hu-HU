---
title: "SharePoint-hely megfigyelése az Application Insights segítségével"
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
ms.openlocfilehash: a3b37674469a131016f46af590e1eee3ba4cdc73
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="monitor-a-sharepoint-site-with-application-insights"></a><span data-ttu-id="feaf1-103">SharePoint-hely megfigyelése az Application Insights segítségével</span><span class="sxs-lookup"><span data-stu-id="feaf1-103">Monitor a SharePoint site with Application Insights</span></span>
<span data-ttu-id="feaf1-104">Az Azure Application Insights figyeli alkalmazásai rendelkezésre állását, teljesítményét és használatát.</span><span class="sxs-lookup"><span data-stu-id="feaf1-104">Azure Application Insights monitors the availability, performance and usage of your apps.</span></span> <span data-ttu-id="feaf1-105">Ebből a cikkből megismerheti, hogyan állíthatja be egy SharePoint-helyhez.</span><span class="sxs-lookup"><span data-stu-id="feaf1-105">Here you'll learn how to set it up for a SharePoint site.</span></span>

## <a name="create-an-application-insights-resource"></a><span data-ttu-id="feaf1-106">Application Insights-erőforrás létrehozása</span><span class="sxs-lookup"><span data-stu-id="feaf1-106">Create an Application Insights resource</span></span>
<span data-ttu-id="feaf1-107">Az [Azure Portalon](https://portal.azure.com) hozzon létre egy új Application Insights-erőforrást.</span><span class="sxs-lookup"><span data-stu-id="feaf1-107">In the [Azure portal](https://portal.azure.com), create a new Application Insights resource.</span></span> <span data-ttu-id="feaf1-108">Az alkalmazás típusának válassza az ASP.NET lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="feaf1-108">Choose ASP.NET as the application type.</span></span>

![Kattintson a Tulajdonságok elemre, válassza ki a kulcsot, és nyomja le a ctrl+C billentyűkombinációt.](./media/app-insights-sharepoint/01-new.png)

<span data-ttu-id="feaf1-110">A megnyíló panelen megtekintheti az alkalmazása teljesítmény- és használati adatait.</span><span class="sxs-lookup"><span data-stu-id="feaf1-110">The blade that opens is the place where you'll see performance and usage data about your app.</span></span> <span data-ttu-id="feaf1-111">Amikor legközelebb bejelentkezik az Azure-ba, a kezdőképernyőn található csempére kattintva léphet közvetlenül erre a panelre.</span><span class="sxs-lookup"><span data-stu-id="feaf1-111">To get back to it next time you login to Azure, you should find a tile for it on the start screen.</span></span> <span data-ttu-id="feaf1-112">Vagy a Tallózás gombra kattintva is megkeresheti.</span><span class="sxs-lookup"><span data-stu-id="feaf1-112">Alternatively click Browse to find it.</span></span>

## <a name="add-our-script-to-your-web-pages"></a><span data-ttu-id="feaf1-113">A szkriptünk hozzáadása weblapokhoz</span><span class="sxs-lookup"><span data-stu-id="feaf1-113">Add our script to your web pages</span></span>
<span data-ttu-id="feaf1-114">A Gyors üzembe helyezés területen kérje le a weblapok szkriptjét:</span><span class="sxs-lookup"><span data-stu-id="feaf1-114">In Quick Start, get the script for web pages:</span></span>

![](./media/app-insights-sharepoint/02-monitor-web-page.png)

<span data-ttu-id="feaf1-115">Szúrja be a szkriptet minden olyan lap &lt;/head&gt; címkéje elé, amelyet nyomon szeretne követni. Ha a webhelye mesterlappal rendelkezik, ide helyezheti a szkriptet.</span><span class="sxs-lookup"><span data-stu-id="feaf1-115">Insert the script just before the &lt;/head&gt; tag of every page you want to track. If your website has a master page, you can put the script there.</span></span> <span data-ttu-id="feaf1-116">Egy ASP.NET MVC-projektben a következő helyre helyezné a szkriptet: View\Shared\_Layout.cshtml</span><span class="sxs-lookup"><span data-stu-id="feaf1-116">For example, in an ASP.NET MVC project, you'd put it in View\Shared\_Layout.cshtml</span></span>

<span data-ttu-id="feaf1-117">A szkript tartalmazza a kialakítási kulcsot, amely a telemetriát az Application Insights-erőforrásra irányítja.</span><span class="sxs-lookup"><span data-stu-id="feaf1-117">The script contains the instrumentation key that directs the telemetry to your Application Insights resource.</span></span>

### <a name="add-the-code-to-your-site-pages"></a><span data-ttu-id="feaf1-118">Kód hozzáadása webhely lapjaihoz</span><span class="sxs-lookup"><span data-stu-id="feaf1-118">Add the code to your site pages</span></span>
#### <a name="on-the-master-page"></a><span data-ttu-id="feaf1-119">A mesterlapon</span><span class="sxs-lookup"><span data-stu-id="feaf1-119">On the master page</span></span>
<span data-ttu-id="feaf1-120">Ha szerkeszti webhelye mesterlapját, az biztosítja a webhely összes lapjának figyelését.</span><span class="sxs-lookup"><span data-stu-id="feaf1-120">If you can edit the site's master page, that will provide monitoring for every page in the site.</span></span>

<span data-ttu-id="feaf1-121">Tekintse át a mesterlapot, és szerkessze a SharePoint Designerrel vagy más szerkesztővel.</span><span class="sxs-lookup"><span data-stu-id="feaf1-121">Check out the master page and edit it using SharePoint Designer or any other editor.</span></span>

![](./media/app-insights-sharepoint/03-master.png)

<span data-ttu-id="feaf1-122">Adja hozzá a kódot közvetlenül a(z) </head> címke elé.</span><span class="sxs-lookup"><span data-stu-id="feaf1-122">Add the code just before the </head> tag.</span></span> 

![](./media/app-insights-sharepoint/04-code.png)

#### <a name="or-on-individual-pages"></a><span data-ttu-id="feaf1-123">Vagy önálló lapokon</span><span class="sxs-lookup"><span data-stu-id="feaf1-123">Or on individual pages</span></span>
<span data-ttu-id="feaf1-124">Adott számú lap figyeléséhez adja hozzá a szkriptet egyesével a lapokhoz.</span><span class="sxs-lookup"><span data-stu-id="feaf1-124">To monitor a limited set of pages, add the script separately to each page.</span></span> 

<span data-ttu-id="feaf1-125">Szúrjon be egy kijelzőt, és ágyazza bele a kódrészletet.</span><span class="sxs-lookup"><span data-stu-id="feaf1-125">Insert a web part and embed the code snippet in it.</span></span>

![](./media/app-insights-sharepoint/05-page.png)

## <a name="view-data-about-your-app"></a><span data-ttu-id="feaf1-126">Alkalmazás adatainak megtekintése</span><span class="sxs-lookup"><span data-stu-id="feaf1-126">View data about your app</span></span>
<span data-ttu-id="feaf1-127">Helyezze ismét üzembe alkalmazását.</span><span class="sxs-lookup"><span data-stu-id="feaf1-127">Redeploy your app.</span></span>

<span data-ttu-id="feaf1-128">Térjen vissza alkalmazása paneljéhez az [Azure Portalon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="feaf1-128">Return to your application blade in the [Azure portal](https://portal.azure.com).</span></span>

<span data-ttu-id="feaf1-129">Az első események megjelennek a keresésben.</span><span class="sxs-lookup"><span data-stu-id="feaf1-129">The first events will appear in Search.</span></span> 

![](./media/app-insights-sharepoint/09-search.png)

<span data-ttu-id="feaf1-130">Ha több adatot vár, néhány másodperc elteltével kattintson a Frissítés elemre.</span><span class="sxs-lookup"><span data-stu-id="feaf1-130">Click Refresh after a few seconds if you're expecting more data.</span></span>

<span data-ttu-id="feaf1-131">A felhasználókkal, munkamenetekkel és lapmegtekintésekkel kapcsolatos diagramok megtekintéséhez kattintson a **Használatelemzés** elemre az áttekintési panelen:</span><span class="sxs-lookup"><span data-stu-id="feaf1-131">From the overview blade, click **Usage analytics** to see to charts of users, sessions and page views:</span></span>

![](./media/app-insights-sharepoint/06-usage.png)

<span data-ttu-id="feaf1-132">Bármely diagramra kattintva további részleteket tekinthet meg, például a Lapmegtekintések diagram esetében:</span><span class="sxs-lookup"><span data-stu-id="feaf1-132">Click any chart to see more details - for example Page Views:</span></span>

![](./media/app-insights-sharepoint/07-pages.png)

<span data-ttu-id="feaf1-133">Vagy a Felhasználók diagram esetében:</span><span class="sxs-lookup"><span data-stu-id="feaf1-133">Or Users:</span></span>

![](./media/app-insights-sharepoint/08-users.png)

## <a name="capturing-user-id"></a><span data-ttu-id="feaf1-134">Felhasználói azonosító rögzítése</span><span class="sxs-lookup"><span data-stu-id="feaf1-134">Capturing User Id</span></span>
<span data-ttu-id="feaf1-135">A normál weblapkódrészlet nem rögzíti a felhasználói azonosítót a SharePointból, de ez egy apró módosítással elvégezhető.</span><span class="sxs-lookup"><span data-stu-id="feaf1-135">The standard web page code snippet doesn't capture the user id from SharePoint, but you can do that with a small modification.</span></span>

1. <span data-ttu-id="feaf1-136">Az Application Insights Alapvető erőforrások legördülő menüjéből másolja ki alkalmazása kialakítási kulcsát.</span><span class="sxs-lookup"><span data-stu-id="feaf1-136">Copy your app's instrumentation key from the Essentials drop-down in Application Insights.</span></span> 

    ![](./media/app-insights-sharepoint/02-props.png)

1. <span data-ttu-id="feaf1-137">Az alábbi részletben szereplő „XXXX”-et cserélje le a kialakítási kulcsra.</span><span class="sxs-lookup"><span data-stu-id="feaf1-137">Substitute the instrumentation key for 'XXXX' in the snippet below.</span></span> 
2. <span data-ttu-id="feaf1-138">Ágyazza be a szkriptet a SharePoint alkalmazásába annak a kódrészletnek a helyére, amelyet a portálról töltött le.</span><span class="sxs-lookup"><span data-stu-id="feaf1-138">Embed the script in your SharePoint app instead of the snippet you get from the portal.</span></span>

```


<SharePoint:ScriptLink ID="ScriptLink1" name="SP.js" runat="server" localizable="false" loadafterui="true" /> 
<SharePoint:ScriptLink ID="ScriptLink2" name="SP.UserProfiles.js" runat="server" localizable="false" loadafterui="true" /> 

<script type="text/javascript"> 
var personProperties; 

// Ensure that the SP.UserProfiles.js file is loaded before the custom code runs. 
SP.SOD.executeOrDelayUntilScriptLoaded(getUserProperties, 'SP.UserProfiles.js'); 

function getUserProperties() { 
    // Get the current client context and PeopleManager instance. 
    var clientContext = new SP.ClientContext.get_current(); 
    var peopleManager = new SP.UserProfiles.PeopleManager(clientContext); 

    // Get user properties for the target user. 
    // To get the PersonProperties object for the current user, use the 
    // getMyProperties method. 

    personProperties = peopleManager.getMyProperties(); 

    // Load the PersonProperties object and send the request. 
    clientContext.load(personProperties); 
    clientContext.executeQueryAsync(onRequestSuccess, onRequestFail); 
} 

// This function runs if the executeQueryAsync call succeeds. 
function onRequestSuccess() { 
var appInsights=window.appInsights||function(config){
function s(config){t[config]=function(){var i=arguments;t.queue.push(function(){t[config].apply(t,i)})}}var t={config:config},r=document,f=window,e="script",o=r.createElement(e),i,u;for(o.src=config.url||"//az416426.vo.msecnd.net/scripts/a/ai.0.js",r.getElementsByTagName(e)[0].parentNode.appendChild(o),t.cookie=r.cookie,t.queue=[],i=["Event","Exception","Metric","PageView","Trace"];i.length;)s("track"+i.pop());return config.disableExceptionTracking||(i="onerror",s("_"+i),u=f[i],f[i]=function(config,r,f,e,o){var s=u&&u(config,r,f,e,o);return s!==!0&&t["_"+i](config,r,f,e,o),s}),t
    }({
        instrumentationKey:"XXXX"
    });
    window.appInsights=appInsights;
    appInsights.trackPageView(document.title,window.location.href, {User: personProperties.get_displayName()});
} 

// This function runs if the executeQueryAsync call fails. 
function onRequestFail(sender, args) { 
} 
</script> 


```



## <a name="next-steps"></a><span data-ttu-id="feaf1-139">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="feaf1-139">Next Steps</span></span>
* <span data-ttu-id="feaf1-140">[Webes tesztek](app-insights-monitor-web-app-availability.md) webhelye rendelkezésre állásának figyeléséhez.</span><span class="sxs-lookup"><span data-stu-id="feaf1-140">[Web tests](app-insights-monitor-web-app-availability.md) to monitor the availability of your site.</span></span>
* <span data-ttu-id="feaf1-141">[Application Insights](app-insights-overview.md) más típusú alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="feaf1-141">[Application Insights](app-insights-overview.md) for other types of app.</span></span>

<!--Link references-->


