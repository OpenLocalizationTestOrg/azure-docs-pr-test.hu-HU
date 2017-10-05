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
# <a name="create-your-first-java-web-app-in-azure"></a><span data-ttu-id="cb045-103">Az első Java-webalkalmazás létrehozása az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="cb045-103">Create your first Java web app in Azure</span></span>

<span data-ttu-id="cb045-104">Az [Azure App Service](../app-service/app-service-value-prop-what-is.md) [Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) szolgáltatása egy hatékonyan méretezhető, önjavító webes üzemeltetési szolgáltatást biztosít.</span><span class="sxs-lookup"><span data-stu-id="cb045-104">The [Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) feature of [Azure App Service](../app-service/app-service-value-prop-what-is.md) provides a highly scalable, self-patching web hosting service.</span></span> <span data-ttu-id="cb045-105">Ez a gyorsútmutató bemutatja, hogyan helyezhet üzembe Java-webalkalmazásokat az App Service-ben a [Java EE-fejlesztőknek készült Eclipse IDE](http://www.eclipse.org/) használatával.</span><span class="sxs-lookup"><span data-stu-id="cb045-105">This quickstart shows how to deploy a Java web app to App Service by using the [Eclipse IDE for Java EE Developers](http://www.eclipse.org/).</span></span>

![„Hello Azure!”](./media/app-service-web-get-started-java/browse-web-app-1.png)

## <a name="prerequisites"></a><span data-ttu-id="cb045-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="cb045-108">Prerequisites</span></span>

<span data-ttu-id="cb045-109">A gyorsútmutató elvégzéséhez a következők telepítése szükséges:</span><span class="sxs-lookup"><span data-stu-id="cb045-109">To complete this quickstart, install:</span></span>

* <span data-ttu-id="cb045-110">Az ingyenes, [Java EE-fejlesztőknek készült Eclipse IDE](http://www.eclipse.org/downloads/).</span><span class="sxs-lookup"><span data-stu-id="cb045-110">The free [Eclipse IDE for Java EE Developers](http://www.eclipse.org/downloads/).</span></span> <span data-ttu-id="cb045-111">Ez a gyorsútmutató az Eclipse Neont használj.</span><span class="sxs-lookup"><span data-stu-id="cb045-111">This quickstart uses Eclipse Neon.</span></span>
* <span data-ttu-id="cb045-112">Az [Eclipse-hez készült Azure-eszközkészlet](/azure/azure-toolkit-for-eclipse-installation).</span><span class="sxs-lookup"><span data-stu-id="cb045-112">The [Azure Toolkit for Eclipse](/azure/azure-toolkit-for-eclipse-installation).</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-dynamic-web-project-in-eclipse"></a><span data-ttu-id="cb045-113">Dinamikus webes projekt létrehozása az Eclipse-ben</span><span class="sxs-lookup"><span data-stu-id="cb045-113">Create a dynamic web project in Eclipse</span></span>

<span data-ttu-id="cb045-114">Az Eclipse-ben válassza a **File (Fájl)** > **New (Új)** > **Dynamic Web Project** (Dinamikus webprojekt) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="cb045-114">In Eclipse, select **File** > **New** > **Dynamic Web Project**.</span></span>

<span data-ttu-id="cb045-115">A **New Dynamic Web Project** (Új dinamikus webprojekt) párbeszédpanelen adja a **MyFirstJavaOnAzureWebApp** nevet a projektnek, és válassza a **Finish** (Befejezés) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="cb045-115">In the **New Dynamic Web Project** dialog box, name the project **MyFirstJavaOnAzureWebApp**, and select **Finish**.</span></span>
   
![New Dynamic Web Project (Új dinamikus webprojekt) párbeszédpanel](./media/app-service-web-get-started-java/new-dynamic-web-project-dialog-box.png)

### <a name="add-a-jsp-page"></a><span data-ttu-id="cb045-117">JSP-oldal hozzáadása</span><span class="sxs-lookup"><span data-stu-id="cb045-117">Add a JSP page</span></span>

<span data-ttu-id="cb045-118">Ha a Project Explorer (Projektböngésző) nem jelenik meg, állítsa azt vissza.</span><span class="sxs-lookup"><span data-stu-id="cb045-118">If Project Explorer is not displayed, restore it.</span></span>

![Az Eclipse-hez készült Java EE munkaterület](./media/app-service-web-get-started-java/pe.png)

<span data-ttu-id="cb045-120">A Project Explorer (Projektböngésző) nézetben bontsa ki a **MyFirstJavaOnAzureWebApp** projektet.</span><span class="sxs-lookup"><span data-stu-id="cb045-120">In Project Explorer, expand the **MyFirstJavaOnAzureWebApp** project.</span></span>
<span data-ttu-id="cb045-121">Kattintson a jobb gombbal az **WebContent** elemre, majd válassza a **(New) Új** > **JSP File (JSP-fájl)** (JSP-fájl) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="cb045-121">Right-click **WebContent**, and then select **New** > **JSP File**.</span></span>

![Az új JSP-fájl menüje a Project Explorer (Projektböngésző) nézetben](./media/app-service-web-get-started-java/new-jsp-file-menu.png)

<span data-ttu-id="cb045-123">A **New JSP File** (Új JSP-fájl) párbeszédpanelen:</span><span class="sxs-lookup"><span data-stu-id="cb045-123">In the **New JSP File** dialog box:</span></span>

* <span data-ttu-id="cb045-124">Nevezze el a fájlt az alábbi módon: **index.jsp**.</span><span class="sxs-lookup"><span data-stu-id="cb045-124">Name the file **index.jsp**.</span></span>
* <span data-ttu-id="cb045-125">Válassza a **Finish** (Befejezés) elemet.</span><span class="sxs-lookup"><span data-stu-id="cb045-125">Select **Finish**.</span></span>

  ![New JSP File (Új JSP-fájl) párbeszédpanel](./media/app-service-web-get-started-java/new-jsp-file-dialog-box-page-1.png)

<span data-ttu-id="cb045-127">Az index.jsp fájlban cserélje le a `<body></body>` elemet az alábbi jelöléssel:</span><span class="sxs-lookup"><span data-stu-id="cb045-127">In the index.jsp file, replace the `<body></body>` element with the following markup:</span></span>

```jsp
<body>
<h1><% out.println("Hello Azure!"); %></h1>
</body>
```

<span data-ttu-id="cb045-128">Mentse a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="cb045-128">Save the changes.</span></span>

## <a name="publish-the-web-app-to-azure"></a><span data-ttu-id="cb045-129">A webalkalmazás közzététele az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="cb045-129">Publish the web app to Azure</span></span>

<span data-ttu-id="cb045-130">A Project Explorer (Projektböngésző) nézetben kattintson a jobb gombbal a projektre, majd válassza az **Azure** > **Publish as Azure Web App** (Közzététel Azure-webalkalmazásként) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="cb045-130">In Project Explorer, right-click the project, and then select **Azure** > **Publish as Azure Web App**.</span></span>

![A Publish as Azure Web App (Közzététel Azure-webalkalmazásként) helyi menü](./media/app-service-web-get-started-java/publish-as-azure-web-app-context-menu.png)

<span data-ttu-id="cb045-132">Az **Azure bejelentkezési** párbeszédpanelen tartsa meg az **Interactive** (Interaktív) lehetőséget, majd válassza a **Sign in** (Bejelentkezés) gombot.</span><span class="sxs-lookup"><span data-stu-id="cb045-132">In the **Azure Sign In** dialog box, keep the **Interactive** option, and then select **Sign in**.</span></span>

<span data-ttu-id="cb045-133">Kövesse a bejelentkezési utasításokat.</span><span class="sxs-lookup"><span data-stu-id="cb045-133">Follow the sign-in instructions.</span></span>

### <a name="deploy-web-app-dialog-box"></a><span data-ttu-id="cb045-134">Deploy Web App (Webalkalmazás üzembe helyezése) párbeszédpanel</span><span class="sxs-lookup"><span data-stu-id="cb045-134">Deploy Web App dialog box</span></span>

<span data-ttu-id="cb045-135">Miután bejelentkezett Azure-fiókjába, megjelenik a **Deploy Web App** (Webalkalmazás üzembe helyezése) párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="cb045-135">After you have signed in to your Azure account, the **Deploy Web App** dialog box appears.</span></span>

<span data-ttu-id="cb045-136">Kattintson a **Létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="cb045-136">Select **Create**.</span></span>

![Deploy Web App (Webalkalmazás üzembe helyezése) párbeszédpanel](./media/app-service-web-get-started-java/deploy-web-app-dialog-box.png)

### <a name="create-app-service-dialog-box"></a><span data-ttu-id="cb045-138">A Create App Service (App Service létrehozása) párbeszédpanel</span><span class="sxs-lookup"><span data-stu-id="cb045-138">Create App Service dialog box</span></span>

<span data-ttu-id="cb045-139">Megjelenik a **Create App Service** (App Service létrehozása) párbeszédpanel az alapértelmezett értékekkel.</span><span class="sxs-lookup"><span data-stu-id="cb045-139">The **Create App Service** dialog box appears with default values.</span></span> <span data-ttu-id="cb045-140">Az alábbi képen látható **170602185241** szám eltérő az Ön párbeszédpanelén.</span><span class="sxs-lookup"><span data-stu-id="cb045-140">The number **170602185241** shown in the following image is different in your dialog box.</span></span>

![A Create App Service (App Service létrehozása) párbeszédpanel](./media/app-service-web-get-started-java/cas1.png)

<span data-ttu-id="cb045-142">A **Create App Service** (App Service létrehozása) párbeszédpanelen:</span><span class="sxs-lookup"><span data-stu-id="cb045-142">In the **Create App Service** dialog box:</span></span>

* <span data-ttu-id="cb045-143">Tartsa meg a webalkalmazáshoz létrehozott nevet.</span><span class="sxs-lookup"><span data-stu-id="cb045-143">Keep the generated name for the web app.</span></span> <span data-ttu-id="cb045-144">Ennek a névnek az Azure-on belül egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="cb045-144">This name must be unique across Azure.</span></span> <span data-ttu-id="cb045-145">Ez a név a webalkalmazáshoz tartozó URL-cím része.</span><span class="sxs-lookup"><span data-stu-id="cb045-145">The name is part of the URL address for the web app.</span></span> <span data-ttu-id="cb045-146">Ha például a webalkalmazás neve **MyJavaWebApp**, az URL-cím *myjavawebapp.azurewebsites.net*.</span><span class="sxs-lookup"><span data-stu-id="cb045-146">For example: if the web app name is **MyJavaWebApp**, the URL is *myjavawebapp.azurewebsites.net*.</span></span>
* <span data-ttu-id="cb045-147">Tartsa meg az alapértelmezett webes tárolót.</span><span class="sxs-lookup"><span data-stu-id="cb045-147">Keep the default web container.</span></span>
* <span data-ttu-id="cb045-148">Válasszon ki egy Azure-előfizetést.</span><span class="sxs-lookup"><span data-stu-id="cb045-148">Select an Azure subscription.</span></span>
* <span data-ttu-id="cb045-149">Az **App service plan** (App Service-csomag) lapon:</span><span class="sxs-lookup"><span data-stu-id="cb045-149">On the **App service plan** tab:</span></span>

  * <span data-ttu-id="cb045-150">**Create new** (Új létrehozása): tartsa meg az alapértelmezettet, amely az App Service-csomag neve.</span><span class="sxs-lookup"><span data-stu-id="cb045-150">**Create new**: Keep the default, which is the name of the App Service plan.</span></span>
  * <span data-ttu-id="cb045-151">**Location** (Hely): válassza a **West Europe** (Nyugat-Európa) lehetőséget vagy egy Önhöz közeli helyet.</span><span class="sxs-lookup"><span data-stu-id="cb045-151">**Location**: Select **West Europe** or a location near you.</span></span>
  * <span data-ttu-id="cb045-152">**Pricing tier** (Tarifacsomag): válassza az ingyenes lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="cb045-152">**Pricing tier**: Select the free option.</span></span> <span data-ttu-id="cb045-153">A szolgáltatások díját az [App Service díjszabás](https://azure.microsoft.com/pricing/details/app-service/) részben találja.</span><span class="sxs-lookup"><span data-stu-id="cb045-153">For features, see [App Service pricing](https://azure.microsoft.com/pricing/details/app-service/).</span></span>

   ![A Create App Service (App Service létrehozása) párbeszédpanel](./media/app-service-web-get-started-java/create-app-service-dialog-box.png)

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

### <a name="resource-group-tab"></a><span data-ttu-id="cb045-155">Resource group (Erőforráscsoport) lap</span><span class="sxs-lookup"><span data-stu-id="cb045-155">Resource group tab</span></span>

<span data-ttu-id="cb045-156">Válassza ki a **Resource group** (Erőforráscsoport) lapot. Tartsa meg az erőforráscsoporthoz tartozó alapértelmezetten létrehozott értéket.</span><span class="sxs-lookup"><span data-stu-id="cb045-156">Select the **Resource group** tab. Keep the default generated value for the resource group.</span></span>

![Resource group (Erőforráscsoport) lap](./media/app-service-web-get-started-java/create-app-service-resource-group.png)

[!INCLUDE [resource-group](../../includes/resource-group.md)]

<span data-ttu-id="cb045-158">Kattintson a **Létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="cb045-158">Select **Create**.</span></span>

<!--
### The JDK tab

Select the **JDK** tab. Keep the default, and then select **Create**.

![Create App Service plan](./media/app-service-web-get-started-java/create-app-service-specify-jdk.png)
-->

<span data-ttu-id="cb045-159">Az Azure-eszközkészlet létrehozza a webalkalmazást, és megjelenít egy folyamatjelző panelt.</span><span class="sxs-lookup"><span data-stu-id="cb045-159">The Azure Toolkit creates the web app and displays a progress dialog box.</span></span>

![App Service létrehozásának állapotjelző párbeszédpanelje](./media/app-service-web-get-started-java/create-app-service-progress-bar.png)

### <a name="deploy-web-app-dialog-box"></a><span data-ttu-id="cb045-161">Deploy Web App (Webalkalmazás üzembe helyezése) párbeszédpanel</span><span class="sxs-lookup"><span data-stu-id="cb045-161">Deploy Web App dialog box</span></span>

<span data-ttu-id="cb045-162">A **Deploy Web App** (Webalkalmazás üzembe helyezése) párbeszédpanelen válassza a **Deploy to root** (Üzembe helyezés a gyökérnél) beállítást.</span><span class="sxs-lookup"><span data-stu-id="cb045-162">In the **Deploy Web App** dialog box, select **Deploy to root**.</span></span> <span data-ttu-id="cb045-163">Ha egy App Service a *wingtiptoys.azurewebsites.net* helyen, és nem a gyökérnél végzi el az üzembe helyezést, a **MyFirstJavaOnAzureWebApp** nevű webalkalmazás a *wingtiptoys.azurewebsites.net/MyFirstJavaOnAzureWebApp* helyen lesz üzembe helyezve.</span><span class="sxs-lookup"><span data-stu-id="cb045-163">If you have an app service at *wingtiptoys.azurewebsites.net* and you do not deploy to the root, the web app named **MyFirstJavaOnAzureWebApp** is deployed to *wingtiptoys.azurewebsites.net/MyFirstJavaOnAzureWebApp*.</span></span>

![Deploy Web App (Webalkalmazás üzembe helyezése) párbeszédpanel](./media/app-service-web-get-started-java/deploy-web-app-to-root.png)

<span data-ttu-id="cb045-165">A párbeszédpanel megjeleníti az Azure-nál, a JDK-nál és a webes tárolónál kiválasztott beállításokat.</span><span class="sxs-lookup"><span data-stu-id="cb045-165">The dialog box shows the Azure, JDK, and web container selections.</span></span>

<span data-ttu-id="cb045-166">A webalkalmazás Azure-ban történő közzétételéhez válassza a **Deploy** (Üzembe helyezés) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="cb045-166">Select **Deploy** to publish the web app to Azure.</span></span>

<span data-ttu-id="cb045-167">A közzététel befejezése után válassza a **Published** (Közzétéve) hivatkozást az **Azure Activity Log** (Azure tevékenységnapló) párbeszédpanelen.</span><span class="sxs-lookup"><span data-stu-id="cb045-167">When the publishing finishes, select the **Published** link in the **Azure Activity Log** dialog box.</span></span>

![Azure Activity Log (Azure tevékenységnapló) párbeszédpanel](./media/app-service-web-get-started-java/aal.png)

<span data-ttu-id="cb045-169">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="cb045-169">Congratulations!</span></span> <span data-ttu-id="cb045-170">Sikeresen végrehajtotta a webalkalmazás üzembe helyezését az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="cb045-170">You have successfully deployed your web app to Azure.</span></span> 

![„Hello Azure!”](./media/app-service-web-get-started-java/browse-web-app-1.png)

## <a name="update-the-web-app"></a><span data-ttu-id="cb045-173">A webalkalmazás frissítése</span><span class="sxs-lookup"><span data-stu-id="cb045-173">Update the web app</span></span>

<span data-ttu-id="cb045-174">Módosítsa a JSP-mintakódot egy eltérő üzenetre.</span><span class="sxs-lookup"><span data-stu-id="cb045-174">Change the sample JSP code to a different message.</span></span>

```jsp
<body>
<h1><% out.println("Hello again Azure!"); %></h1>
</body>
```

<span data-ttu-id="cb045-175">Mentse a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="cb045-175">Save the changes.</span></span>

<span data-ttu-id="cb045-176">A Project Explorer (Projektböngésző) nézetben kattintson a jobb gombbal a projektre, majd válassza az **Azure** > **Publish as Azure Web App** (Közzététel Azure-webalkalmazásként) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="cb045-176">In Project Explorer, right-click the project, and then select **Azure** > **Publish as Azure Web App**.</span></span>

<span data-ttu-id="cb045-177">Megjelenik a **Deploy Web App** (Webalkalmazás üzembe helyezése) párbeszédpanel, és megjeleníti a korábban létrehozott App Service-t.</span><span class="sxs-lookup"><span data-stu-id="cb045-177">The **Deploy Web App** dialog box appears and shows the app service that you previously created.</span></span> 

> [!NOTE]
> <span data-ttu-id="cb045-178">Minden egyes közzétételkor válassza a **Deploy to root** (Üzembe helyezés a gyökérnél) beállítást.</span><span class="sxs-lookup"><span data-stu-id="cb045-178">Select **Deploy to root** each time you publish.</span></span>
>

<span data-ttu-id="cb045-179">Válassza ki a webalkalmazást, majd a **Deploy** (Üzembe helyezés) lehetőséget, ami közzéteszi a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="cb045-179">Select the web app and select **Deploy**, which publishes the changes.</span></span>

<span data-ttu-id="cb045-180">Amikor megjelenik a **Publishing** (Közzététel) hivatkozás, válassza azt ki a webalkalmazás tallózásához, és tekintse meg a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="cb045-180">When the **Publishing** link appears, select it to browse to the web app and see the changes.</span></span>

## <a name="manage-the-web-app"></a><span data-ttu-id="cb045-181">A webalkalmazás kezelése</span><span class="sxs-lookup"><span data-stu-id="cb045-181">Manage the web app</span></span>

<span data-ttu-id="cb045-182">Ugorjon az <a href="https://portal.azure.com" target="_blank">Azure Portalra</a>, és tekintse meg a létrehozott webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="cb045-182">Go to the <a href="https://portal.azure.com" target="_blank">Azure portal</a> to see the web app that you created.</span></span>

<span data-ttu-id="cb045-183">A bal oldali menüben válassza az **Erőforráscsoportok** elemet.</span><span class="sxs-lookup"><span data-stu-id="cb045-183">From the left menu, select **Resource Groups**.</span></span>

![Navigálás a portálon az erőforráscsoportokhoz](media/app-service-web-get-started-java/rg.png)

<span data-ttu-id="cb045-185">Válassza az erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="cb045-185">Select the resource group.</span></span> <span data-ttu-id="cb045-186">Az oldal megjeleníti a gyorsútmutatóban létrehozott erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="cb045-186">The page shows the resources that you created in this quickstart.</span></span>

![myResourceGroup erőforráscsoport](media/app-service-web-get-started-java/rg2.png)

<span data-ttu-id="cb045-188">Válassza a webalkalmazást (**webapp-170602193915** az előző képen).</span><span class="sxs-lookup"><span data-stu-id="cb045-188">Select the web app (**webapp-170602193915** in the preceding image).</span></span>

<span data-ttu-id="cb045-189">Megjelenik az **Áttekintés** oldal.</span><span class="sxs-lookup"><span data-stu-id="cb045-189">The **Overview** page appears.</span></span> <span data-ttu-id="cb045-190">Ezen az oldalon megtekintheti az alkalmazás állapotát.</span><span class="sxs-lookup"><span data-stu-id="cb045-190">This page gives you a view of how the app is doing.</span></span> <span data-ttu-id="cb045-191">Itt elvégezhet olyan alapszintű felügyeleti feladatokat, mint a tallózás, leállítás, elindítás, újraindítás és törlés.</span><span class="sxs-lookup"><span data-stu-id="cb045-191">Here, you can  perform basic management tasks like browse, stop, start, restart, and delete.</span></span> <span data-ttu-id="cb045-192">A panel bal oldalán lévő lapok a különböző megnyitható konfigurációs oldalakat jelenítik meg.</span><span class="sxs-lookup"><span data-stu-id="cb045-192">The tabs on the left side of the page show the different configurations that you can open.</span></span> 

![Az App Service lap az Azure Portalon](media/app-service-web-get-started-java/web-app-blade.png)

[!INCLUDE [clean-up-section-portal-web-app](../../includes/clean-up-section-portal-web-app.md)]

## <a name="next-steps"></a><span data-ttu-id="cb045-194">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cb045-194">Next steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="cb045-195">Egyéni tartomány leképezése</span><span class="sxs-lookup"><span data-stu-id="cb045-195">Map custom domain</span></span>](app-service-web-tutorial-custom-domain.md)
