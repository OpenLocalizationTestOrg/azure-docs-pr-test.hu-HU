---
title: "aaaCreate az első Java-webalkalmazás az Azure-ban"
description: "Ismerje meg, hogyan toorun webalkalmazások az App Service egy alapszintű Java-alkalmazás központi telepítésével."
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
ms.openlocfilehash: 81315c07b5aa84cbec50a17b2cb3914927b19c00
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-java-web-app-in-azure"></a><span data-ttu-id="35412-103">Az első Java-webalkalmazás létrehozása az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="35412-103">Create your first Java web app in Azure</span></span>

<span data-ttu-id="35412-104">Hello [webalkalmazások](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) szolgáltatása [Azure App Service](../app-service/app-service-value-prop-what-is.md) jól skálázható, önálló javítási a webhelyszolgáltató biztosít.</span><span class="sxs-lookup"><span data-stu-id="35412-104">hello [Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) feature of [Azure App Service](../app-service/app-service-value-prop-what-is.md) provides a highly scalable, self-patching web hosting service.</span></span> <span data-ttu-id="35412-105">A gyors üzembe helyezés bemutatja, hogyan toodeploy Java webes hello segítségével app tooApp szolgáltatás [Eclipse IDE Java EE-fejlesztőknek](http://www.eclipse.org/).</span><span class="sxs-lookup"><span data-stu-id="35412-105">This quickstart shows how toodeploy a Java web app tooApp Service by using hello [Eclipse IDE for Java EE Developers](http://www.eclipse.org/).</span></span>

![„Hello Azure!”](./media/app-service-web-get-started-java/browse-web-app-1.png)

## <a name="prerequisites"></a><span data-ttu-id="35412-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="35412-108">Prerequisites</span></span>

<span data-ttu-id="35412-109">toocomplete a gyors üzembe helyezés telepítése:</span><span class="sxs-lookup"><span data-stu-id="35412-109">toocomplete this quickstart, install:</span></span>

* <span data-ttu-id="35412-110">szabad hello [Eclipse IDE Java EE-fejlesztőknek](http://www.eclipse.org/downloads/).</span><span class="sxs-lookup"><span data-stu-id="35412-110">hello free [Eclipse IDE for Java EE Developers](http://www.eclipse.org/downloads/).</span></span> <span data-ttu-id="35412-111">Ez a gyorsútmutató az Eclipse Neont használj.</span><span class="sxs-lookup"><span data-stu-id="35412-111">This quickstart uses Eclipse Neon.</span></span>
* <span data-ttu-id="35412-112">Hello [Eclipse Azure eszköztára](/azure/azure-toolkit-for-eclipse-installation).</span><span class="sxs-lookup"><span data-stu-id="35412-112">hello [Azure Toolkit for Eclipse](/azure/azure-toolkit-for-eclipse-installation).</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-dynamic-web-project-in-eclipse"></a><span data-ttu-id="35412-113">Dinamikus webes projekt létrehozása az Eclipse-ben</span><span class="sxs-lookup"><span data-stu-id="35412-113">Create a dynamic web project in Eclipse</span></span>

<span data-ttu-id="35412-114">Az Eclipse-ben válassza a **File (Fájl)** > **New (Új)** > **Dynamic Web Project** (Dinamikus webprojekt) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="35412-114">In Eclipse, select **File** > **New** > **Dynamic Web Project**.</span></span>

<span data-ttu-id="35412-115">A hello **új dinamikus webes projekt** párbeszédpanelen neve hello projekt **MyFirstJavaOnAzureWebApp**, és válassza ki **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="35412-115">In hello **New Dynamic Web Project** dialog box, name hello project **MyFirstJavaOnAzureWebApp**, and select **Finish**.</span></span>
   
![New Dynamic Web Project (Új dinamikus webprojekt) párbeszédpanel](./media/app-service-web-get-started-java/new-dynamic-web-project-dialog-box.png)

### <a name="add-a-jsp-page"></a><span data-ttu-id="35412-117">JSP-oldal hozzáadása</span><span class="sxs-lookup"><span data-stu-id="35412-117">Add a JSP page</span></span>

<span data-ttu-id="35412-118">Ha a Project Explorer (Projektböngésző) nem jelenik meg, állítsa azt vissza.</span><span class="sxs-lookup"><span data-stu-id="35412-118">If Project Explorer is not displayed, restore it.</span></span>

![Az Eclipse-hez készült Java EE munkaterület](./media/app-service-web-get-started-java/pe.png)

<span data-ttu-id="35412-120">A Project Explorer bontsa ki a hello **MyFirstJavaOnAzureWebApp** projekt.</span><span class="sxs-lookup"><span data-stu-id="35412-120">In Project Explorer, expand hello **MyFirstJavaOnAzureWebApp** project.</span></span>
<span data-ttu-id="35412-121">Kattintson a jobb gombbal az **WebContent** elemre, majd válassza a **(New) Új** > **JSP File (JSP-fájl)** (JSP-fájl) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="35412-121">Right-click **WebContent**, and then select **New** > **JSP File**.</span></span>

![Az új JSP-fájl menüje a Project Explorer (Projektböngésző) nézetben](./media/app-service-web-get-started-java/new-jsp-file-menu.png)

<span data-ttu-id="35412-123">A hello **új JSP-fájl** párbeszédpanel:</span><span class="sxs-lookup"><span data-stu-id="35412-123">In hello **New JSP File** dialog box:</span></span>

* <span data-ttu-id="35412-124">Nevű hello fájl **index.jsp**.</span><span class="sxs-lookup"><span data-stu-id="35412-124">Name hello file **index.jsp**.</span></span>
* <span data-ttu-id="35412-125">Válassza a **Finish** (Befejezés) elemet.</span><span class="sxs-lookup"><span data-stu-id="35412-125">Select **Finish**.</span></span>

  ![New JSP File (Új JSP-fájl) párbeszédpanel](./media/app-service-web-get-started-java/new-jsp-file-dialog-box-page-1.png)

<span data-ttu-id="35412-127">Hello index.jsp fájlban cserélje le hello `<body></body>` hello jelölés során a következő elem:</span><span class="sxs-lookup"><span data-stu-id="35412-127">In hello index.jsp file, replace hello `<body></body>` element with hello following markup:</span></span>

```jsp
<body>
<h1><% out.println("Hello Azure!"); %></h1>
</body>
```

<span data-ttu-id="35412-128">Hello módosítások mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="35412-128">Save hello changes.</span></span>

## <a name="publish-hello-web-app-tooazure"></a><span data-ttu-id="35412-129">Hello web app tooAzure közzététele</span><span class="sxs-lookup"><span data-stu-id="35412-129">Publish hello web app tooAzure</span></span>

<span data-ttu-id="35412-130">A Project Explorer, kattintson a jobb gombbal a projekt hello, és válassza **Azure** > **Azure Web Apps közzététel**.</span><span class="sxs-lookup"><span data-stu-id="35412-130">In Project Explorer, right-click hello project, and then select **Azure** > **Publish as Azure Web App**.</span></span>

![A Publish as Azure Web App (Közzététel Azure-webalkalmazásként) helyi menü](./media/app-service-web-get-started-java/publish-as-azure-web-app-context-menu.png)

<span data-ttu-id="35412-132">A hello **Azure bejelentkezés** párbeszédpanelen, a tárolás során is garantálják hello **interaktív** lehetőséget, majd válassza ki **bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="35412-132">In hello **Azure Sign In** dialog box, keep hello **Interactive** option, and then select **Sign in**.</span></span>

<span data-ttu-id="35412-133">Kövesse az utasításokat bejelentkezési hello.</span><span class="sxs-lookup"><span data-stu-id="35412-133">Follow hello sign-in instructions.</span></span>

### <a name="deploy-web-app-dialog-box"></a><span data-ttu-id="35412-134">Deploy Web App (Webalkalmazás üzembe helyezése) párbeszédpanel</span><span class="sxs-lookup"><span data-stu-id="35412-134">Deploy Web App dialog box</span></span>

<span data-ttu-id="35412-135">Miután bejelentkezett az Azure-fiók tooyour, hello **webalkalmazás telepítése** párbeszédpanel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="35412-135">After you have signed in tooyour Azure account, hello **Deploy Web App** dialog box appears.</span></span>

<span data-ttu-id="35412-136">Kattintson a **Létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="35412-136">Select **Create**.</span></span>

![Deploy Web App (Webalkalmazás üzembe helyezése) párbeszédpanel](./media/app-service-web-get-started-java/deploy-web-app-dialog-box.png)

### <a name="create-app-service-dialog-box"></a><span data-ttu-id="35412-138">A Create App Service (App Service létrehozása) párbeszédpanel</span><span class="sxs-lookup"><span data-stu-id="35412-138">Create App Service dialog box</span></span>

<span data-ttu-id="35412-139">Hello **létrehozása az App Service** párbeszédpanel jelenik meg az alapértelmezett értékekkel.</span><span class="sxs-lookup"><span data-stu-id="35412-139">hello **Create App Service** dialog box appears with default values.</span></span> <span data-ttu-id="35412-140">szám hello **170602185241** hello a következő kép nem azonos a párbeszédpanelen látható.</span><span class="sxs-lookup"><span data-stu-id="35412-140">hello number **170602185241** shown in hello following image is different in your dialog box.</span></span>

![A Create App Service (App Service létrehozása) párbeszédpanel](./media/app-service-web-get-started-java/cas1.png)

<span data-ttu-id="35412-142">A hello **létrehozása az App Service** párbeszédpanel:</span><span class="sxs-lookup"><span data-stu-id="35412-142">In hello **Create App Service** dialog box:</span></span>

* <span data-ttu-id="35412-143">Tartsa hello webalkalmazás hello generált név.</span><span class="sxs-lookup"><span data-stu-id="35412-143">Keep hello generated name for hello web app.</span></span> <span data-ttu-id="35412-144">Ennek a névnek az Azure-on belül egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="35412-144">This name must be unique across Azure.</span></span> <span data-ttu-id="35412-145">hello neve hello URL-címet hello webalkalmazás részét képezi.</span><span class="sxs-lookup"><span data-stu-id="35412-145">hello name is part of hello URL address for hello web app.</span></span> <span data-ttu-id="35412-146">Példa: Ha hello webes alkalmazás neve **MyJavaWebApp**, URL-cím hello *myjavawebapp.azurewebsites.net*.</span><span class="sxs-lookup"><span data-stu-id="35412-146">For example: if hello web app name is **MyJavaWebApp**, hello URL is *myjavawebapp.azurewebsites.net*.</span></span>
* <span data-ttu-id="35412-147">Tartsa hello alapértelmezett webes tárolót.</span><span class="sxs-lookup"><span data-stu-id="35412-147">Keep hello default web container.</span></span>
* <span data-ttu-id="35412-148">Válasszon ki egy Azure-előfizetést.</span><span class="sxs-lookup"><span data-stu-id="35412-148">Select an Azure subscription.</span></span>
* <span data-ttu-id="35412-149">A hello **App service-csomag** lapon:</span><span class="sxs-lookup"><span data-stu-id="35412-149">On hello **App service plan** tab:</span></span>

  * <span data-ttu-id="35412-150">**Új**: tartsa hello alapértelmezett értéket, amely hello hello App Service-csomag nevét.</span><span class="sxs-lookup"><span data-stu-id="35412-150">**Create new**: Keep hello default, which is hello name of hello App Service plan.</span></span>
  * <span data-ttu-id="35412-151">**Location** (Hely): válassza a **West Europe** (Nyugat-Európa) lehetőséget vagy egy Önhöz közeli helyet.</span><span class="sxs-lookup"><span data-stu-id="35412-151">**Location**: Select **West Europe** or a location near you.</span></span>
  * <span data-ttu-id="35412-152">**A tarifacsomag**: Válasszon hello ingyenes lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="35412-152">**Pricing tier**: Select hello free option.</span></span> <span data-ttu-id="35412-153">A szolgáltatások díját az [App Service díjszabás](https://azure.microsoft.com/pricing/details/app-service/) részben találja.</span><span class="sxs-lookup"><span data-stu-id="35412-153">For features, see [App Service pricing](https://azure.microsoft.com/pricing/details/app-service/).</span></span>

   ![A Create App Service (App Service létrehozása) párbeszédpanel](./media/app-service-web-get-started-java/create-app-service-dialog-box.png)

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

### <a name="resource-group-tab"></a><span data-ttu-id="35412-155">Resource group (Erőforráscsoport) lap</span><span class="sxs-lookup"><span data-stu-id="35412-155">Resource group tab</span></span>

<span data-ttu-id="35412-156">Jelölje be hello **erőforráscsoport** fülre. Tartsa hello alapértelmezés szerint generált érték hello erőforráscsoport.</span><span class="sxs-lookup"><span data-stu-id="35412-156">Select hello **Resource group** tab. Keep hello default generated value for hello resource group.</span></span>

![Resource group (Erőforráscsoport) lap](./media/app-service-web-get-started-java/create-app-service-resource-group.png)

[!INCLUDE [resource-group](../../includes/resource-group.md)]

<span data-ttu-id="35412-158">Kattintson a **Létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="35412-158">Select **Create**.</span></span>

<!--
### hello JDK tab

Select hello **JDK** tab. Keep hello default, and then select **Create**.

![Create App Service plan](./media/app-service-web-get-started-java/create-app-service-specify-jdk.png)
-->

<span data-ttu-id="35412-159">hello Azure eszközkészlet hello webalkalmazás hoz létre, és megjelenik a folyamatjelző párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="35412-159">hello Azure Toolkit creates hello web app and displays a progress dialog box.</span></span>

![App Service létrehozásának állapotjelző párbeszédpanelje](./media/app-service-web-get-started-java/create-app-service-progress-bar.png)

### <a name="deploy-web-app-dialog-box"></a><span data-ttu-id="35412-161">Deploy Web App (Webalkalmazás üzembe helyezése) párbeszédpanel</span><span class="sxs-lookup"><span data-stu-id="35412-161">Deploy Web App dialog box</span></span>

<span data-ttu-id="35412-162">A hello **webalkalmazás telepítése** párbeszédpanelen jelölje ki **tooroot telepítése**.</span><span class="sxs-lookup"><span data-stu-id="35412-162">In hello **Deploy Web App** dialog box, select **Deploy tooroot**.</span></span> <span data-ttu-id="35412-163">Ha egy alkalmazás szolgáltatás: *wingtiptoys.azurewebsites.net* toohello gyökér, hello webes alkalmazás neve nem telepít és **MyFirstJavaOnAzureWebApp** a rendszer túl *wingtiptoys.azurewebsites.net/MyFirstJavaOnAzureWebApp*.</span><span class="sxs-lookup"><span data-stu-id="35412-163">If you have an app service at *wingtiptoys.azurewebsites.net* and you do not deploy toohello root, hello web app named **MyFirstJavaOnAzureWebApp** is deployed too*wingtiptoys.azurewebsites.net/MyFirstJavaOnAzureWebApp*.</span></span>

![Deploy Web App (Webalkalmazás üzembe helyezése) párbeszédpanel](./media/app-service-web-get-started-java/deploy-web-app-to-root.png)

<span data-ttu-id="35412-165">hello párbeszédpanel mezőben látható hello Azure JDK és webes tároló beállításokat.</span><span class="sxs-lookup"><span data-stu-id="35412-165">hello dialog box shows hello Azure, JDK, and web container selections.</span></span>

<span data-ttu-id="35412-166">Válassza ki **telepítés** toopublish hello web app tooAzure.</span><span class="sxs-lookup"><span data-stu-id="35412-166">Select **Deploy** toopublish hello web app tooAzure.</span></span>

<span data-ttu-id="35412-167">Hello közzététel befejezése után válassza ki a hello **közzétett** hello hivatkozásra **Azure tevékenységnapló** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="35412-167">When hello publishing finishes, select hello **Published** link in hello **Azure Activity Log** dialog box.</span></span>

![Azure Activity Log (Azure tevékenységnapló) párbeszédpanel](./media/app-service-web-get-started-java/aal.png)

<span data-ttu-id="35412-169">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="35412-169">Congratulations!</span></span> <span data-ttu-id="35412-170">Sikeresen telepítette a webes alkalmazás tooAzure.</span><span class="sxs-lookup"><span data-stu-id="35412-170">You have successfully deployed your web app tooAzure.</span></span> 

![„Hello Azure!”](./media/app-service-web-get-started-java/browse-web-app-1.png)

## <a name="update-hello-web-app"></a><span data-ttu-id="35412-173">Hello webes alkalmazás frissítése</span><span class="sxs-lookup"><span data-stu-id="35412-173">Update hello web app</span></span>

<span data-ttu-id="35412-174">Módosítsa a minta JSP kód tooa különböző üdvözlőüzenetére.</span><span class="sxs-lookup"><span data-stu-id="35412-174">Change hello sample JSP code tooa different message.</span></span>

```jsp
<body>
<h1><% out.println("Hello again Azure!"); %></h1>
</body>
```

<span data-ttu-id="35412-175">Hello módosítások mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="35412-175">Save hello changes.</span></span>

<span data-ttu-id="35412-176">A Project Explorer, kattintson a jobb gombbal a projekt hello, és válassza **Azure** > **Azure Web Apps közzététel**.</span><span class="sxs-lookup"><span data-stu-id="35412-176">In Project Explorer, right-click hello project, and then select **Azure** > **Publish as Azure Web App**.</span></span>

<span data-ttu-id="35412-177">Hello **webalkalmazás telepítése** párbeszédpanel jelenik meg, és azt mutatja be, amelyet korábban hozott létre az app service hello.</span><span class="sxs-lookup"><span data-stu-id="35412-177">hello **Deploy Web App** dialog box appears and shows hello app service that you previously created.</span></span> 

> [!NOTE]
> <span data-ttu-id="35412-178">Válassza ki **tooroot telepítése** minden alkalommal, amikor a közzététele.</span><span class="sxs-lookup"><span data-stu-id="35412-178">Select **Deploy tooroot** each time you publish.</span></span>
>

<span data-ttu-id="35412-179">Hello webalkalmazás válassza ki, majd **telepítés**, amely közzéteszi a hello módosításokat.</span><span class="sxs-lookup"><span data-stu-id="35412-179">Select hello web app and select **Deploy**, which publishes hello changes.</span></span>

<span data-ttu-id="35412-180">Ha hello **közzétételi** hivatkozás jelenik meg, válassza ki azt a toobrowse toohello web app és hello módosítások megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="35412-180">When hello **Publishing** link appears, select it toobrowse toohello web app and see hello changes.</span></span>

## <a name="manage-hello-web-app"></a><span data-ttu-id="35412-181">Hello webes alkalmazás kezelése</span><span class="sxs-lookup"><span data-stu-id="35412-181">Manage hello web app</span></span>

<span data-ttu-id="35412-182">Nyissa meg toohello <a href="https://portal.azure.com" target="_blank">Azure-portálon</a> toosee hello webalkalmazást, amelyet létrehozott.</span><span class="sxs-lookup"><span data-stu-id="35412-182">Go toohello <a href="https://portal.azure.com" target="_blank">Azure portal</a> toosee hello web app that you created.</span></span>

<span data-ttu-id="35412-183">Hello bal oldali menüben válassza ki a **erőforráscsoportok**.</span><span class="sxs-lookup"><span data-stu-id="35412-183">From hello left menu, select **Resource Groups**.</span></span>

![Portálnavigációjával tooresource csoportok](media/app-service-web-get-started-java/rg.png)

<span data-ttu-id="35412-185">Válasszon hello erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="35412-185">Select hello resource group.</span></span> <span data-ttu-id="35412-186">hello lapon látható a gyors üzembe helyezés létrehozott hello erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="35412-186">hello page shows hello resources that you created in this quickstart.</span></span>

![myResourceGroup erőforráscsoport](media/app-service-web-get-started-java/rg2.png)

<span data-ttu-id="35412-188">Jelölje be hello web app (**webapp-170602193915** a kép megelőző hello).</span><span class="sxs-lookup"><span data-stu-id="35412-188">Select hello web app (**webapp-170602193915** in hello preceding image).</span></span>

<span data-ttu-id="35412-189">Hello **áttekintése** lap jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="35412-189">hello **Overview** page appears.</span></span> <span data-ttu-id="35412-190">Ezen a lapon hogyan hello app tesz a nézetét biztosítja.</span><span class="sxs-lookup"><span data-stu-id="35412-190">This page gives you a view of how hello app is doing.</span></span> <span data-ttu-id="35412-191">Itt elvégezhet olyan alapszintű felügyeleti feladatokat, mint a tallózás, leállítás, elindítás, újraindítás és törlés.</span><span class="sxs-lookup"><span data-stu-id="35412-191">Here, you can  perform basic management tasks like browse, stop, start, restart, and delete.</span></span> <span data-ttu-id="35412-192">hello lapok hello bal oldalán található hello megjelenítése hello megnyithatja különböző konfigurációkat.</span><span class="sxs-lookup"><span data-stu-id="35412-192">hello tabs on hello left side of hello page show hello different configurations that you can open.</span></span> 

![Az App Service lap az Azure Portalon](media/app-service-web-get-started-java/web-app-blade.png)

[!INCLUDE [clean-up-section-portal-web-app](../../includes/clean-up-section-portal-web-app.md)]

## <a name="next-steps"></a><span data-ttu-id="35412-194">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="35412-194">Next steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="35412-195">Egyéni tartomány leképezése</span><span class="sxs-lookup"><span data-stu-id="35412-195">Map custom domain</span></span>](app-service-web-tutorial-custom-domain.md)
