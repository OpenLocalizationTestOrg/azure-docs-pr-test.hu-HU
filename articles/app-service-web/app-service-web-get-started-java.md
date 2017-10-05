---
title: "Az első Java-webalkalmazás létrehozása az Azure-ban"
description: "Egy alapszintű Java-alkalmazás üzembe helyezésével megtudhatja, hogy miként futtathat webalkalmazásokat az App Service-ben."
services: app-service\web
documentationcenter: 
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 8bacfe3e-7f0b-4394-959a-a88618cb31e1
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: quickstart
ms.date: 6/7/2017
ms.author: cephalin;robmcm
ms.custom: mvc
ms.openlocfilehash: b91b9bde5eb8ea0d7e2196056b635fe54095e748
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="create-your-first-java-web-app-in-azure"></a>Az első Java-webalkalmazás létrehozása az Azure-ban

Az [Azure App Service](../app-service/app-service-value-prop-what-is.md) [Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) szolgáltatása egy hatékonyan méretezhető, önjavító webes üzemeltetési szolgáltatást biztosít. Ez a gyorsútmutató bemutatja, hogyan helyezhet üzembe Java-webalkalmazásokat az App Service-ben a [Java EE-fejlesztőknek készült Eclipse IDE](http://www.eclipse.org/) használatával.

![„Hello Azure!” példa webalkalmazás](./media/app-service-web-get-started-java/browse-web-app-1.png)

## <a name="prerequisites"></a>Előfeltételek

A gyorsútmutató elvégzéséhez a következők telepítése szükséges:

* Az ingyenes, [Java EE-fejlesztőknek készült Eclipse IDE](http://www.eclipse.org/downloads/). Ez a gyorsútmutató az Eclipse Neont használj.
* Az [Eclipse-hez készült Azure-eszközkészlet](/azure/azure-toolkit-for-eclipse-installation).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-dynamic-web-project-in-eclipse"></a>Dinamikus webes projekt létrehozása az Eclipse-ben

Az Eclipse-ben válassza a **File (Fájl)** > **New (Új)** > **Dynamic Web Project** (Dinamikus webprojekt) lehetőséget.

A **New Dynamic Web Project** (Új dinamikus webprojekt) párbeszédpanelen adja a **MyFirstJavaOnAzureWebApp** nevet a projektnek, és válassza a **Finish** (Befejezés) lehetőséget.
   
![New Dynamic Web Project (Új dinamikus webprojekt) párbeszédpanel](./media/app-service-web-get-started-java/new-dynamic-web-project-dialog-box.png)

### <a name="add-a-jsp-page"></a>JSP-oldal hozzáadása

Ha a Project Explorer (Projektböngésző) nem jelenik meg, állítsa azt vissza.

![Az Eclipse-hez készült Java EE munkaterület](./media/app-service-web-get-started-java/pe.png)

A Project Explorer (Projektböngésző) nézetben bontsa ki a **MyFirstJavaOnAzureWebApp** projektet.
Kattintson a jobb gombbal az **WebContent** elemre, majd válassza a **(New) Új** > **JSP File (JSP-fájl)** (JSP-fájl) lehetőséget.

![Az új JSP-fájl menüje a Project Explorer (Projektböngésző) nézetben](./media/app-service-web-get-started-java/new-jsp-file-menu.png)

A **New JSP File** (Új JSP-fájl) párbeszédpanelen:

* Nevezze el a fájlt az alábbi módon: **index.jsp**.
* Válassza a **Finish** (Befejezés) elemet.

  ![New JSP File (Új JSP-fájl) párbeszédpanel](./media/app-service-web-get-started-java/new-jsp-file-dialog-box-page-1.png)

Az index.jsp fájlban cserélje le a `<body></body>` elemet az alábbi jelöléssel:

```jsp
<body>
<h1><% out.println("Hello Azure!"); %></h1>
</body>
```

Mentse a módosításokat.

## <a name="publish-the-web-app-to-azure"></a>A webalkalmazás közzététele az Azure-ban

A Project Explorer (Projektböngésző) nézetben kattintson a jobb gombbal a projektre, majd válassza az **Azure** > **Publish as Azure Web App** (Közzététel Azure-webalkalmazásként) lehetőséget.

![A Publish as Azure Web App (Közzététel Azure-webalkalmazásként) helyi menü](./media/app-service-web-get-started-java/publish-as-azure-web-app-context-menu.png)

Az **Azure bejelentkezési** párbeszédpanelen tartsa meg az **Interactive** (Interaktív) lehetőséget, majd válassza a **Sign in** (Bejelentkezés) gombot.

Kövesse a bejelentkezési utasításokat.

### <a name="deploy-web-app-dialog-box"></a>Deploy Web App (Webalkalmazás üzembe helyezése) párbeszédpanel

Miután bejelentkezett Azure-fiókjába, megjelenik a **Deploy Web App** (Webalkalmazás üzembe helyezése) párbeszédpanel.

Kattintson a **Létrehozás** gombra.

![Deploy Web App (Webalkalmazás üzembe helyezése) párbeszédpanel](./media/app-service-web-get-started-java/deploy-web-app-dialog-box.png)

### <a name="create-app-service-dialog-box"></a>A Create App Service (App Service létrehozása) párbeszédpanel

Megjelenik a **Create App Service** (App Service létrehozása) párbeszédpanel az alapértelmezett értékekkel. Az alábbi képen látható **170602185241** szám eltérő az Ön párbeszédpanelén.

![A Create App Service (App Service létrehozása) párbeszédpanel](./media/app-service-web-get-started-java/cas1.png)

A **Create App Service** (App Service létrehozása) párbeszédpanelen:

* Tartsa meg a webalkalmazáshoz létrehozott nevet. Ennek a névnek az Azure-on belül egyedinek kell lennie. Ez a név a webalkalmazáshoz tartozó URL-cím része. Ha például a webalkalmazás neve **MyJavaWebApp**, az URL-cím *myjavawebapp.azurewebsites.net*.
* Tartsa meg az alapértelmezett webes tárolót.
* Válasszon ki egy Azure-előfizetést.
* Az **App service plan** (App Service-csomag) lapon:

  * **Create new** (Új létrehozása): tartsa meg az alapértelmezettet, amely az App Service-csomag neve.
  * **Location** (Hely): válassza a **West Europe** (Nyugat-Európa) lehetőséget vagy egy Önhöz közeli helyet.
  * **Pricing tier** (Tarifacsomag): válassza az ingyenes lehetőséget. A szolgáltatások díját az [App Service díjszabás](https://azure.microsoft.com/pricing/details/app-service/) részben találja.

   ![A Create App Service (App Service létrehozása) párbeszédpanel](./media/app-service-web-get-started-java/create-app-service-dialog-box.png)

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

### <a name="resource-group-tab"></a>Resource group (Erőforráscsoport) lap

Válassza ki a **Resource group** (Erőforráscsoport) lapot. Tartsa meg az erőforráscsoporthoz tartozó alapértelmezetten létrehozott értéket.

![Resource group (Erőforráscsoport) lap](./media/app-service-web-get-started-java/create-app-service-resource-group.png)

[!INCLUDE [resource-group](../../includes/resource-group.md)]

Kattintson a **Létrehozás** gombra.

<!--
### The JDK tab

Select the **JDK** tab. Keep the default, and then select **Create**.

![Create App Service plan](./media/app-service-web-get-started-java/create-app-service-specify-jdk.png)
-->

Az Azure-eszközkészlet létrehozza a webalkalmazást, és megjelenít egy folyamatjelző panelt.

![App Service létrehozásának állapotjelző párbeszédpanelje](./media/app-service-web-get-started-java/create-app-service-progress-bar.png)

### <a name="deploy-web-app-dialog-box"></a>Deploy Web App (Webalkalmazás üzembe helyezése) párbeszédpanel

A **Deploy Web App** (Webalkalmazás üzembe helyezése) párbeszédpanelen válassza a **Deploy to root** (Üzembe helyezés a gyökérnél) beállítást. Ha egy App Service a *wingtiptoys.azurewebsites.net* helyen, és nem a gyökérnél végzi el az üzembe helyezést, a **MyFirstJavaOnAzureWebApp** nevű webalkalmazás a *wingtiptoys.azurewebsites.net/MyFirstJavaOnAzureWebApp* helyen lesz üzembe helyezve.

![Deploy Web App (Webalkalmazás üzembe helyezése) párbeszédpanel](./media/app-service-web-get-started-java/deploy-web-app-to-root.png)

A párbeszédpanel megjeleníti az Azure-nál, a JDK-nál és a webes tárolónál kiválasztott beállításokat.

A webalkalmazás Azure-ban történő közzétételéhez válassza a **Deploy** (Üzembe helyezés) lehetőséget.

A közzététel befejezése után válassza a **Published** (Közzétéve) hivatkozást az **Azure Activity Log** (Azure tevékenységnapló) párbeszédpanelen.

![Azure Activity Log (Azure tevékenységnapló) párbeszédpanel](./media/app-service-web-get-started-java/aal.png)

Gratulálunk! Sikeresen végrehajtotta a webalkalmazás üzembe helyezését az Azure-ban. 

![„Hello Azure!” példa webalkalmazás](./media/app-service-web-get-started-java/browse-web-app-1.png)

## <a name="update-the-web-app"></a>A webalkalmazás frissítése

Módosítsa a JSP-mintakódot egy eltérő üzenetre.

```jsp
<body>
<h1><% out.println("Hello again Azure!"); %></h1>
</body>
```

Mentse a módosításokat.

A Project Explorer (Projektböngésző) nézetben kattintson a jobb gombbal a projektre, majd válassza az **Azure** > **Publish as Azure Web App** (Közzététel Azure-webalkalmazásként) lehetőséget.

Megjelenik a **Deploy Web App** (Webalkalmazás üzembe helyezése) párbeszédpanel, és megjeleníti a korábban létrehozott App Service-t. 

> [!NOTE]
> Minden egyes közzétételkor válassza a **Deploy to root** (Üzembe helyezés a gyökérnél) beállítást.
>

Válassza ki a webalkalmazást, majd a **Deploy** (Üzembe helyezés) lehetőséget, ami közzéteszi a módosításokat.

Amikor megjelenik a **Publishing** (Közzététel) hivatkozás, válassza azt ki a webalkalmazás tallózásához, és tekintse meg a módosításokat.

## <a name="manage-the-web-app"></a>A webalkalmazás kezelése

Ugorjon az <a href="https://portal.azure.com" target="_blank">Azure Portalra</a>, és tekintse meg a létrehozott webalkalmazást.

A bal oldali menüben válassza az **Erőforráscsoportok** elemet.

![Navigálás a portálon az erőforráscsoportokhoz](media/app-service-web-get-started-java/rg.png)

Válassza az erőforráscsoportot. Az oldal megjeleníti a gyorsútmutatóban létrehozott erőforrásokat.

![myResourceGroup erőforráscsoport](media/app-service-web-get-started-java/rg2.png)

Válassza a webalkalmazást (**webapp-170602193915** az előző képen).

Megjelenik az **Áttekintés** oldal. Ezen az oldalon megtekintheti az alkalmazás állapotát. Itt elvégezhet olyan alapszintű felügyeleti feladatokat, mint a tallózás, leállítás, elindítás, újraindítás és törlés. A panel bal oldalán lévő lapok a különböző megnyitható konfigurációs oldalakat jelenítik meg. 

![Az App Service lap az Azure Portalon](media/app-service-web-get-started-java/web-app-blade.png)

[!INCLUDE [clean-up-section-portal-web-app](../../includes/clean-up-section-portal-web-app.md)]

## <a name="next-steps"></a>Következő lépések

> [!div class="nextstepaction"]
> [Egyéni tartomány leképezése](app-service-web-tutorial-custom-domain.md)
