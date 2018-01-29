---
title: "Az Azure Application Insights gyors üzembe helyezése | Microsoft Docs"
description: "Az ASP.NET Core-webalkalmazások gyors üzembe helyezésével kapcsolatos utasítások az Application Insights-monitorozáshoz"
services: application-insights
keywords: 
author: mrbullwinkle
ms.author: mbullwin
ms.date: 09/15/2017
ms.service: application-insights
ms.custom: mvc
ms.topic: quickstart
manager: carmonm
ms.openlocfilehash: 065638c1d724865f45b0c1e2691041848f90ad00
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 12/08/2017
---
# <a name="start-monitoring-your-aspnet-core-web-application"></a>Az ASP.NET Core-webalkalmazás monitorozásának indítása

Az Azure Application Insights segítségével egyszerűen monitorozhatja webalkalmazása rendelkezésre állását, teljesítményét és használatát. Emellett egyszerűen azonosíthatja és diagnosztizálhatja az alkalmazás hibáit anélkül, hogy meg kellene várnia, amíg egy felhasználó jelenti azokat. 

Ez a rövid útmutató végigvezeti az Application Insights SDK egy meglévő ASP.NET Core-webalkalmazáshoz való hozzáadásának folyamatán. 

## <a name="prerequisites"></a>Előfeltételek

A gyorsútmutató elvégzéséhez:

- [Telepítse a Visual Studio 2017](https://www.visualstudio.com/downloads/) szoftvert a következő számítási feladatokkal:
  - ASP.NET és webfejlesztés
  - Azure-fejlesztés
- [A .NET Core 2.0 SDK telepítése](https://www.microsoft.com/net/core)
- Szüksége lesz egy Azure-előfizetésre és egy meglévő .NET Core-webalkalmazásra.

Ha nem rendelkezik ASP.NET Core-webalkalmazással, a létrehozásához követheti az [ASP.NET Core-webalkalmazások létrehozásához készült útmutató](https://docs.microsoft.com/aspnet/core/tutorials/publish-to-azure-webapp-using-vs) utasításait.

Ha nem rendelkezik Azure-előfizetéssel, első lépésként mindössze néhány perc alatt létrehozhat egy [ingyenes](https://azure.microsoft.com/free/) fiókot.

## <a name="log-in-to-the-azure-portal"></a>Jelentkezzen be az Azure portálra.

Jelentkezzen be az [Azure portálra](https://portal.azure.com/).

## <a name="enable-application-insights"></a>Az Application Insights engedélyezése

Az Application Insights bármely, az internethez csatlakozó alkalmazásról képes telemetriaadatokat gyűjteni, függetlenül attól, hogy az a helyszínen vagy a felhőben fut-e. Az adatok megjelenítéséhez hajtsa végre az alábbi lépéseket.

1. Válassza az **Új** > **Felügyelet és kezelés** > **Application Insights** elemet.

   ![Application Insights-erőforrások hozzáadása](./media/app-insights-dotnetcore-quick-start/0001-dc.png)

    Megjelenik egy konfigurációs képernyő. Az adatbeviteli mezők kitöltéséhez használhatja az alábbi táblázatot.

    | Beállítások        |  Érték           | Leírás  |
   | ------------- |:-------------|:-----|
   | **Name (Név)**      | Globálisan egyedi érték | A figyelt alkalmazást azonosító név |
   | **Alkalmazás típusa** | ASP.NET-es webalkalmazás | A figyelt alkalmazás típusa |
   | **Erőforráscsoport**     | myResourceGroup      | Az App Insights-adatokat futtató új erőforráscsoport neve |
   | **Hely** | USA keleti régiója | Válasszon egy Önhöz vagy az alkalmazást futtató gazdagéphez közeli helyet. |

2. Kattintson a **Create** (Létrehozás) gombra.

## <a name="configure-app-insights-sdk"></a>Az App Insights SDK konfigurálása

1. Nyissa meg az ASP.NET Core-webalkalmazás **projektet** a Visual Studióban, majd kattintson a jobb gombbal az alkalmazás nevére a **Megoldáskezelőben**, és válassza a **Hozzáadás** > **Application Insights Telemetria** elemet.

    ![Application Insights Telemetria hozzáadása](./media/app-insights-dotnetcore-quick-start/0001.png)

2. Kattintson az **Ingyenes próba megkezdése** gombra, válassza ki az Azure Portalon létrehozott **Meglévő erőforrást**, és kattintson a **Regisztráció** parancsra.

3. Az alkalmazás indításához válassza a **Hibakeresés** > **Indítás hibakeresés nélkül** (Ctrl+F5) elemet

> [!NOTE]
> 3–5 percet is igénybe vehet, mire az adatok elkezdenek megjelenni a portálon. Ha az alkalmazás egy alacsony forgalmú tesztalkalmazás, vegye figyelembe, hogy a legtöbb metrika rögzítése csak akkor történik, ha aktív kérések és műveletek vannak folyamatban.

## <a name="start-monitoring-in-the-azure-portal"></a>Monitorozás indítása az Azure Portalon

1. Most megnyithatja ismét az Application Insights **Áttekintés** lapját az Azure Portalon (**Projekt** > **Application Insights** > **Application Insights portál megnyitása**) a jelenleg futó alkalmazás részleteinek megjelenítéséhez.

   ![Az Application Insights áttekintése menü](./media/app-insights-dotnetcore-quick-start/004-Black.png)

2. Kattintson az **Alkalmazástérkép** elemre az alkalmazás-összetevők függőségi viszonyait mutató vizuális elrendezés megjelenítéséhez. Minden egyes összetevőnél megjelennek a KPI-k, például a terhelés, a teljesítmény, a hibák és a riasztások.

   ![Alkalmazástérkép](./media/app-insights-dotnetcore-quick-start/0002-dc.png)

3. Kattintson az **Alkalmazáselemzés** ikonra ![Alkalmazáselemzés ikon](./media/app-insights-dotnetcore-quick-start/006.png).  Megnyílik az **Application Insights Analytics**, amely egy részletes lekérdezési nyelvet biztosít az Application Insights által gyűjtött adatok elemzéséhez. Esetünkben most egy lekérdezés jön létre, amely a kérések számát egy diagramon jeleníti meg. A további adatok elemzéséhez írhat saját lekérdezéseket is.

   ![Az adott időtartamon belüli felhasználói kéréseket mutató elemzési diagram](./media/app-insights-dotnetcore-quick-start/0007-dc.png)

4. Lépjen vissza az **Áttekintés** lapra, és vizsgálja meg az **Állapotáttekintési időegyenest**.  Ezen az irányítópulton az alkalmazás állapotával kapcsolatos statisztikák jelennek meg, köztük a bejövő kérések száma, az egyes kérések időtartama, valamint az esetleges hibák. 

   ![Az Állapotáttekintési időegyenes diagramjai](./media/app-insights-dotnetcore-quick-start/0008-dc.png)

   Ahhoz, hogy a **Lapmegtekintés betöltési ideje** diagramon megjelenjenek az **ügyféloldali telemetria** adatai, adja az alábbi szkriptet minden olyan oldalhoz, amelyet nyomon kíván követni:

   ```HTML
   <!-- 
   To collect end-user usage analytics about your application, 
   insert the following script into each page you want to track.
   Place this code immediately before the closing </head> tag,
   and before any other scripts. Your first data will appear 
   automatically in just a few seconds.
   -->
   <script type="text/javascript">
     var appInsights=window.appInsights||function(config){
       function i(config){t[config]=function(){var i=arguments;t.queue.push(function(){t[config].apply(t,i)})}}var t={config:config},u=document,e=window,o="script",s="AuthenticatedUserContext",h="start",c="stop",l="Track",a=l+"Event",v=l+"Page",y=u.createElement(o),r,f;y.src=config.url||"https://az416426.vo.msecnd.net/scripts/a/ai.0.js";u.getElementsByTagName(o)[0].parentNode.appendChild(y);try{t.cookie=u.cookie}catch(p){}for(t.queue=[],t.version="1.0",r=["Event","Exception","Metric","PageView","Trace","Dependency"];r.length;)i("track"+r.pop());return i("set"+s),i("clear"+s),i(h+a),i(c+a),i(h+v),i(c+v),i("flush"),config.disableExceptionTracking||(r="onerror",i("_"+r),f=e[r],e[r]=function(config,i,u,e,o){var s=f&&f(config,i,u,e,o);return s!==!0&&t["_"+r](config,i,u,e,o),s}),t
       }({
           instrumentationKey:"<insert instrumentation key>"
       });
       
       window.appInsights=appInsights;
       appInsights.trackPageView();
   </script>
   ```

5. Kattintson a **Böngésző** gombra a **Vizsgálat** fejléc alatt. Itt találja az alkalmazás lapjainak teljesítményével kapcsolatos metrikákat. Az **Új diagram hozzáadása** gombra kattintva további egyéni nézeteket adhat hozzá, a **Szerkesztés** gombra kattintva pedig módosíthatja a meglévő diagramok típusát, magasságát, színpalettáját, csoportosításait és metrikáit.

   ![Kiszolgálómetrikák diagram](./media/app-insights-dotnetcore-quick-start/009-Black.png)

## <a name="clean-up-resources"></a>Az erőforrások eltávolítása

Ha azt tervezi, hogy az ezt követő rövid útmutatókkal vagy az oktatóanyagokkal dolgozik tovább, akkor ne törölje az ebben a rövid útmutatóban létrehozott erőforrásokat. Ha nem folytatja a munkát, akkor a következő lépésekkel törölheti az Azure Portalon a rövid útmutatóhoz létrehozott összes erőforrást.

1. Az Azure Portal bal oldali menüjében kattintson az **Erőforráscsoportok** lehetőségre, majd kattintson a **myResourceGroup** elemre.
2. Az erőforráscsoport oldalán kattintson a **Törlés** elemre, írja be a **myResourceGroup** szöveget a szövegmezőbe, majd kattintson a **Törlés** gombra.

## <a name="next-steps"></a>Következő lépések

> [!div class="nextstepaction"]
> [Futásidejű kivételek észlelése és diagnosztizálása](https://docs.microsoft.com/azure/application-insights/app-insights-tutorial-runtime-exceptions)
