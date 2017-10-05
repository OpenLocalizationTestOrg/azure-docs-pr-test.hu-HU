---
title: "Umbraco-webapp üzembe helyezése az Azure Portalon öt perc alatt | Microsoft Docs"
description: "Egy ASP.NET-mintaalkalmazás üzembe helyezésével megtudhatja, mennyire egyszerű a webalkalmazások futtatása az App Service-ben. Az eredmények azonnal láthatók."
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: b1e6bd58-48d1-4007-9d6c-53fd6db061e3
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2017
ms.author: cephalin
ms.openlocfilehash: 9e3e2130a66cdfe5f06eb3b366e53028c44e7e6a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="deploy-an-umbraco-web-app-in-the-azure-portal-in-five-minutes"></a><span data-ttu-id="1e06f-104">Umbraco-webapp üzembe helyezése az Azure Portalon öt perc alatt</span><span class="sxs-lookup"><span data-stu-id="1e06f-104">Deploy an Umbraco web app in the Azure portal in five minutes</span></span>

<span data-ttu-id="1e06f-105">Ez az oktatóanyag segítséget nyújt az [Umbraco](https://our.umbraco.org/)-webalkalmazás gyors üzembe helyezéséhez az [Azure App Service-ben](../app-service/app-service-value-prop-what-is.md).</span><span class="sxs-lookup"><span data-stu-id="1e06f-105">This tutorial helps you deploy n [Umbraco](https://our.umbraco.org/) web app to [Azure App Service](../app-service/app-service-value-prop-what-is.md) in minutes.</span></span>

![Umbraco-alkalmazás](./media/app-service-web-get-started-dotnet-portal/defaultpage.png)

## <a name="prerequisites"></a><span data-ttu-id="1e06f-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="1e06f-107">Prerequisites</span></span>
<span data-ttu-id="1e06f-108">Szüksége van egy Microsoft Azure-fiókra.</span><span class="sxs-lookup"><span data-stu-id="1e06f-108">You need a Microsoft Azure account.</span></span> <span data-ttu-id="1e06f-109">Ha nincs fiókja, [regisztráljon egy ingyenes próbaverzióra](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F), vagy [aktiválhatja a Visual Studio előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="1e06f-109">If you don't have an account, you can [sign up for a free trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) or [activate your Visual Studio subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span></span>

> [!NOTE]
> <span data-ttu-id="1e06f-110">Az [App Service kipróbálása](https://azure.microsoft.com/try/app-service/) Azure-fiók nélkül is lehetséges.</span><span class="sxs-lookup"><span data-stu-id="1e06f-110">You can [Try App Service](https://azure.microsoft.com/try/app-service/) without an Azure account.</span></span> <span data-ttu-id="1e06f-111">Hozzon létre egy kezdő szintű alkalmazást, amellyel legfeljebb egy óráig foglalkozhat – ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.</span><span class="sxs-lookup"><span data-stu-id="1e06f-111">Create a starter app and play with it for up to an hour--no credit card required, no commitments.</span></span>
> 
> 

## <a name="deploy-the-aspnet-app"></a><span data-ttu-id="1e06f-112">Az ASP.NET-alkalmazás üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="1e06f-112">Deploy the ASP.NET app</span></span>
1. <span data-ttu-id="1e06f-113">Jelentkezzen be az [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="1e06f-113">Sign in to the [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="1e06f-114">Nyissa meg a [https://portal.azure.com/#create/umbracoorg.UmbracoCMS](https://portal.azure.com/#create/umbracoorg.UmbracoCMS) webhelyet.</span><span class="sxs-lookup"><span data-stu-id="1e06f-114">Open [https://portal.azure.com/#create/umbracoorg.UmbracoCMS](https://portal.azure.com/#create/umbracoorg.UmbracoCMS).</span></span>

    <span data-ttu-id="1e06f-115">Ez a hivatkozás egy új Umbraco-alkalmazás Azure Portalon való azonnali konfigurálására szolgál.</span><span class="sxs-lookup"><span data-stu-id="1e06f-115">This link is a shortcut to immediately configure a new Umbraco app in the Azure portal.</span></span>

3. <span data-ttu-id="1e06f-116">Az **Alkalmazás neve** mezőben adja meg a webapp nevét.</span><span class="sxs-lookup"><span data-stu-id="1e06f-116">In **App name**, type a web app name.</span></span> <span data-ttu-id="1e06f-117">Ha a név egyedi az `azurewebsites.net` tartományban, megjelenik egy zöld pipa a jelölőnégyzetben.</span><span class="sxs-lookup"><span data-stu-id="1e06f-117">You will see a green checkmark in the box if the name is unique in the `azurewebsites.net` domain.</span></span>
   
5. <span data-ttu-id="1e06f-118">Az **Erőforráscsoport** menüben kattintson az **Új létrehozása** elemre egy új [erőforráscsoport](../azure-resource-manager/resource-group-overview.md) létrehozásához, majd nevezze el.</span><span class="sxs-lookup"><span data-stu-id="1e06f-118">In **Resource Group**, click **Create new** to create a new [resource group](../azure-resource-manager/resource-group-overview.md), then give it a name.</span></span>

7. <span data-ttu-id="1e06f-119">Kattintson az **App Service-csomag/Hely** > **Új létrehozása** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="1e06f-119">Click **App Service plan/Location** > **Create New**.</span></span> <span data-ttu-id="1e06f-120">Konfigurálja az [App Service-csomagot](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="1e06f-120">Configure the [App Service plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) as shown:</span></span>

    - <span data-ttu-id="1e06f-121">Az **App Service-csomag** mezőbe írja be a kívánt nevet.</span><span class="sxs-lookup"><span data-stu-id="1e06f-121">In **App Service plan**, type the desired name.</span></span>
    - <span data-ttu-id="1e06f-122">A **Hely** mezőben válasszon ki egy helyet a csomag üzemeltetésére.</span><span class="sxs-lookup"><span data-stu-id="1e06f-122">In **Location**, choose a location to host your plan.</span></span>
    - <span data-ttu-id="1e06f-123">Kattintson a **Tarifacsomag** lehetőségre, majd válassza az **F1 – Ingyenes** elemet vagy egy másik megfelelő szintet, majd kattintson a **Kiválasztás** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="1e06f-123">Click **Pricing tier**, then select **F1 Free** or another tier that suits you, and then click **Select**.</span></span>
    - <span data-ttu-id="1e06f-124">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="1e06f-124">Click **OK**.</span></span>

    <span data-ttu-id="1e06f-125">Umbraco CMS-konfigurációjának az alábbi képernyőképhez hasonlóan kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="1e06f-125">Your Umbraco CMS configuration should now look like the following screenshot:</span></span>

    ![A konfiguráció folyamatban – az első Umbraco-alkalmazás az Azure App Service-ben](./media/app-service-web-get-started-dotnet-portal/configure-in-progress.png)

12. <span data-ttu-id="1e06f-127">Kattintson az **SQL Database** > **Új adatbázis létrehozása** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="1e06f-127">Click **SQL Database** > **Create a new database**.</span></span> <span data-ttu-id="1e06f-128">Állítsa be az SQL Database adatbázist az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="1e06f-128">Configure the SQL Database as shown:</span></span>

    - <span data-ttu-id="1e06f-129">A **Név** mezőben írjon be egy nevet, például a **myDB**-t.</span><span class="sxs-lookup"><span data-stu-id="1e06f-129">In **Name**, type a name, such as **myDB**.</span></span>
    - <span data-ttu-id="1e06f-130">Kattintson a **Tarifacsomag** lehetőségre, majd válassza az **F – Ingyenes** elemet vagy egy másik megfelelő szintet, majd kattintson a **Kiválasztás** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="1e06f-130">Click **Pricing tier**, then select **F Free** or another tier that suits you, and then click **Select**.</span></span>
    - <span data-ttu-id="1e06f-131">Kattintson a **Célkiszogáló** > **Új kiszolgáló létrehozása** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="1e06f-131">Click **Target server** > **Create a new server**.</span></span> <span data-ttu-id="1e06f-132">Állítsa be az adatbázist az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="1e06f-132">Configure the database server as shown:</span></span>

        - <span data-ttu-id="1e06f-133">A **Kiszolgálónév** mezőben adjon meg egy kiszolgálónevet.</span><span class="sxs-lookup"><span data-stu-id="1e06f-133">In **Server name**, type a server name.</span></span> <span data-ttu-id="1e06f-134">Ha a név egyedi az `.database.windows.net` tartományban, megjelenik egy zöld pipa a jelölőnégyzetben.</span><span class="sxs-lookup"><span data-stu-id="1e06f-134">You will see a green checkmark in the box if the name is unique in the `.database.windows.net` domain.</span></span>
        - <span data-ttu-id="1e06f-135">A **Kiszolgáló-rendszergazdai bejelentkezés** szövegbeviteli mezőben írja be a kívánt rendszergazdai felhasználónevet.</span><span class="sxs-lookup"><span data-stu-id="1e06f-135">In **Server admin login**, type the desired admininistrator username.</span></span>
        - <span data-ttu-id="1e06f-136">A **Jelszó** és a **Jelszó megerősítése** mezőkbe írja be a kívánt jelszót.</span><span class="sxs-lookup"><span data-stu-id="1e06f-136">In **Password** and **Confirm password**, type the desired password.</span></span>
        - <span data-ttu-id="1e06f-137">A Hely mezőben válassza ki ugyanazt a helyet, amelyet a webappban is használ.</span><span class="sxs-lookup"><span data-stu-id="1e06f-137">In Location, select the same location you use for the web app.</span></span>
        - <span data-ttu-id="1e06f-138">Győződjön meg róla, hogy az **Azure-szolgáltatások kiszolgálói hozzáférésének engedélyezése** jelölőnégyzet ki van választva.</span><span class="sxs-lookup"><span data-stu-id="1e06f-138">Make sure **Allow azure services to access server** is selected.</span></span>
        - <span data-ttu-id="1e06f-139">Kattintson a **Kiválasztás** gombra.</span><span class="sxs-lookup"><span data-stu-id="1e06f-139">Click **Select**.</span></span>
    
    - <span data-ttu-id="1e06f-140">Kattintson a **Kiválasztás** gombra.</span><span class="sxs-lookup"><span data-stu-id="1e06f-140">Click **Select**.</span></span>

13. <span data-ttu-id="1e06f-141">Kattintson a **Webapp beállításai** lehetőségre, adja meg az adatbázishoz tartozó felhasználónevet és jelszót, majd kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="1e06f-141">Click **Web app settings**, specify the database username and password, and click **OK**.</span></span>

    <span data-ttu-id="1e06f-142">Umbraco CMS-konfigurációjának az alábbi képernyőképhez hasonlóan kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="1e06f-142">Your Umbraco CMS configuration should now look like the following screenshot:</span></span>

    ![A konfiguráció kész – az első Umbraco-alkalmazás az Azure App Service-ben](./media/app-service-web-get-started-dotnet-portal/configure-complete.png)

14. <span data-ttu-id="1e06f-144">Kattintson a  **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="1e06f-144">Click **Create**.</span></span>
    
    <span data-ttu-id="1e06f-145">Az Azure ekkor létrehozza az Umbraco-alkalmazást a konfiguráció alapján.</span><span class="sxs-lookup"><span data-stu-id="1e06f-145">Azure now creates your Umbraco app based on your configuration.</span></span> <span data-ttu-id="1e06f-146">A **Központi telepítés elindítva...** értesítést kell látnia</span><span class="sxs-lookup"><span data-stu-id="1e06f-146">You should see a **Deployment started...** notification.</span></span>

    ![Az üzembe helyezés sikerült– az első Umbraco-alkalmazás az Azure App Service-ben](./media/app-service-web-get-started-dotnet-portal/deployment-started.png)
   
## <a name="launch-and-manage-your-umrbaco-web-app"></a><span data-ttu-id="1e06f-148">Az Umbraco-alkalmazás elindítása és kezelése</span><span class="sxs-lookup"><span data-stu-id="1e06f-148">Launch and manage your Umrbaco web app</span></span>

<span data-ttu-id="1e06f-149">Ha az Azure befejezte az alkalmazás üzembe helyezését, megjelenik egy újabb értesítés.</span><span class="sxs-lookup"><span data-stu-id="1e06f-149">When Azure completes app deployment you see another notification.</span></span>

![Az üzembe helyezés sikerült– az első Umbraco-alkalmazás az Azure App Service-ben](./media/app-service-web-get-started-dotnet-portal/deployment-succeeded.png)

1. <span data-ttu-id="1e06f-151">Kattintson az értesítésre.</span><span class="sxs-lookup"><span data-stu-id="1e06f-151">Click the notification.</span></span> <span data-ttu-id="1e06f-152">Ha lekéste, az értesítési csengőre kattintva mindig elérheti (![Értesítési csengő – az első Umbraco az Azure App Service-ben](./media/app-service-web-get-started-dotnet-portal/notification.png)).</span><span class="sxs-lookup"><span data-stu-id="1e06f-152">If you missed it, you can always access it by clicking the notification bell (![Notification bellow - first Umbraco in Azure App Service](./media/app-service-web-get-started-dotnet-portal/notification.png)).</span></span>

    <span data-ttu-id="1e06f-153">Ekkor meg kell jelennie a webapp felügyeleti [paneljének](../azure-resource-manager/resource-group-portal.md#manage-resources) (*panel*: egy vízszintesen megnyíló portáloldal).</span><span class="sxs-lookup"><span data-stu-id="1e06f-153">You should now see your web app's management [blade](../azure-resource-manager/resource-group-portal.md#manage-resources) (*blade*: a portal page that opens horizontally).</span></span>

3. <span data-ttu-id="1e06f-154">Az Áttekintés oldal tetején kattintson a **Tallózás** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="1e06f-154">In the top of the Overview page, click **Browse**.</span></span>
   
    ![Tallózás – az első Umbraco-alkalmazás az Azure App Service-ben](./media/app-service-web-get-started-dotnet-portal/browse.png)

    <span data-ttu-id="1e06f-156">Ekkor megjelenik az Umbraco **kezdőlapja**.</span><span class="sxs-lookup"><span data-stu-id="1e06f-156">Now you see the Umbraco **Welcome** page.</span></span> <span data-ttu-id="1e06f-157">Konfigurálja az Umbraco-telepítést, és kísérletezzen vele!</span><span class="sxs-lookup"><span data-stu-id="1e06f-157">Configure the Umbraco installation and start playing with it!</span></span>

    ![Az Umbraco konfigurálása – az első Umbraco-alkalmazás az Azure App Service-ben](./media/app-service-web-get-started-dotnet-portal/umbraco-config.png)
    
## <a name="next-steps"></a><span data-ttu-id="1e06f-159">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1e06f-159">Next steps</span></span>
* <span data-ttu-id="1e06f-160">[ASP.NET webapp telepítése az Azure App Service szolgáltatásba a Visual Studio használatával](app-service-web-get-started-dotnet.md) – Megtudhatja, hogyan hozhat létre új Azure-webappot a Visual Studióból a mellékelt alkalmazássablonok használatával.</span><span class="sxs-lookup"><span data-stu-id="1e06f-160">[Deploy an ASP.NET web app to Azure App Service, using Visual Studio](app-service-web-get-started-dotnet.md) - Learn how to create a new Azure web app from Visual Studio, using any one of the included application templates.</span></span>
* <span data-ttu-id="1e06f-161">[A kód üzembe helyezése az Azure App Service-ben](web-sites-deploy.md) – Elsajátíthatja az FTP-ről vagy forráskezelő tárházból való üzembe helyezés készségét.</span><span class="sxs-lookup"><span data-stu-id="1e06f-161">[Deploy your code to Azure App Service](web-sites-deploy.md)- Learn how to deploy from FTP or from source control repositories.</span></span>
* <span data-ttu-id="1e06f-162">[Funkciók hozzáadása az első webapphoz](app-service-web-get-started-2.md) – Új szintre emelheti Azure alkalmazását.</span><span class="sxs-lookup"><span data-stu-id="1e06f-162">[Add functionality to your first web app](app-service-web-get-started-2.md) - Take your Azure app to the next level.</span></span> <span data-ttu-id="1e06f-163">Hitelesítheti felhasználóit.</span><span class="sxs-lookup"><span data-stu-id="1e06f-163">Authenticate your users.</span></span> <span data-ttu-id="1e06f-164">Igény szerint méretezheti.</span><span class="sxs-lookup"><span data-stu-id="1e06f-164">Scale it based on demand.</span></span> <span data-ttu-id="1e06f-165">Beállíthat a teljesítménnyel kapcsolatos riasztásokat.</span><span class="sxs-lookup"><span data-stu-id="1e06f-165">Set up some performance alerts.</span></span> <span data-ttu-id="1e06f-166">Mindezt csupán néhány kattintással.</span><span class="sxs-lookup"><span data-stu-id="1e06f-166">All with a few clicks.</span></span>
