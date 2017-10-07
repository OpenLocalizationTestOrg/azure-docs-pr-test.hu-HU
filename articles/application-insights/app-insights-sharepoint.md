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
# <a name="monitor-a-sharepoint-site-with-application-insights"></a>SharePoint-hely megfigyelése az Application Insights segítségével
Az Azure Application Insights hello rendelkezésre állási-, teljesítmény- és az alkalmazások használati figyeli. Itt megtudhatja, hogyan tooset azt feliratkozott egy SharePoint-webhelyen.

## <a name="create-an-application-insights-resource"></a>Application Insights-erőforrás létrehozása
A hello [Azure-portálon](https://portal.azure.com), hozzon létre egy új Application Insights-erőforrást. Válassza ki az ASP.NET hello alkalmazás típusként.

![Kattintson a Tulajdonságok parancsra, válassza ki a hello kulcs, használja a ctrl + C](./media/app-insights-sharepoint/01-new.png)

hello panel, amely megnyitja a hello hely, ahol látni fogja, teljesítmény- és használati adatokat az alkalmazásra vonatkozó. tooget hátsó tooit tooAzure, következő bejelentkezéskor látnia kell egy csempe az kifejezésre a kezdőképernyőn hello. Másik lehetőségként kattintson a Tallózás toofind azt.

## <a name="add-our-script-tooyour-web-pages"></a>A parancsfájl tooyour weblapok hozzáadása
A gyors üzembe helyezés hello parancsfájl weblapok érhető el:

![](./media/app-insights-sharepoint/02-monitor-web-page.png)

Hello előtt hello parancsprogram beszúrása &lt;/head&gt; címke tootrack kívánt minden oldalon. Ha a webhely mesterlapra, nem adhat meg hello parancsfájl. Egy ASP.NET MVC-projektben a következő helyre helyezné a szkriptet: View\Shared\_Layout.cshtml

hello hello instrumentation kulcs, amely arra utasítja a hello telemetriai tooyour Application Insights-erőforrást tartalmaz.

### <a name="add-hello-code-tooyour-site-pages"></a>Hello kódlapok tooyour webhely hozzáadása
#### <a name="on-hello-master-page"></a>A hello mesterlap
Ha hello hely mesterlap szerkesztéséhez, amely szerepel figyelésének hello webhely összes lapjára.

Tekintse meg a hello mesterlap és szerkeszthetők a SharePoint Designer vagy bármely más szerkesztő.

![](./media/app-insights-sharepoint/03-master.png)

Adja hozzá hello kódot hello előtt </head> címke. 

![](./media/app-insights-sharepoint/04-code.png)

#### <a name="or-on-individual-pages"></a>Vagy önálló lapokon
toomonitor lap van, korlátozott számú hello parancsfájl külön-külön hozzáadása tooeach lap. 

Kijelző beszúrása és hello kódrészletet beágyazása.

![](./media/app-insights-sharepoint/05-page.png)

## <a name="view-data-about-your-app"></a>Alkalmazás adatainak megtekintése
Helyezze ismét üzembe alkalmazását.

Visszatérési tooyour alkalmazás paneljén hello [Azure-portálon](https://portal.azure.com).

hello első események keresési fog megjelenni. 

![](./media/app-insights-sharepoint/09-search.png)

Ha több adatot vár, néhány másodperc elteltével kattintson a Frissítés elemre.

Hello áttekintése paneljén kattintson **használatelemzés** toosee toocharts a felhasználók, a munkamenetek és az oldalmegtekintéseket:

![](./media/app-insights-sharepoint/06-usage.png)

Kattintson a diagram toosee további részletekért – például a lapmegtekintések:

![](./media/app-insights-sharepoint/07-pages.png)

Vagy a Felhasználók diagram esetében:

![](./media/app-insights-sharepoint/08-users.png)

## <a name="capturing-user-id"></a>Felhasználói azonosító rögzítése
hello szabványos weblap kódrészletet nem rögzíti a hello felhasználói azonosító a SharePoint, azonban lehetőség van, amely egy kis módosítással.

1. Az alkalmazás instrumentation kulcs másolása hello Essentials legördülő az Application insights szolgáltatással. 

    ![](./media/app-insights-sharepoint/02-props.png)

1. Az alábbi hello részlet "XXXX" hello instrumentation kulcs helyettesítse be. 
2. Hello parancsfájl beágyazása a SharePoint-alkalmazásokban beszerzése hello portálról hello részlet helyett.

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



## <a name="next-steps"></a>Következő lépések
* [Webalkalmazás-tesztek](app-insights-monitor-web-app-availability.md) toomonitor hello a hely rendelkezésre állását.
* [Application Insights](app-insights-overview.md) más típusú alkalmazásokhoz.

<!--Link references-->


