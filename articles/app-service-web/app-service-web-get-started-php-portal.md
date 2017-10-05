---
title: "WordPress-alkalmazás üzembe helyezése az Azure Portalon öt perc alatt | Microsoft Docs"
description: "Egy WordPress-alkalmazás üzembe helyezésével megtudhatja, mennyire egyszerű a webappok futtatása az App Service-ben. Az eredmények azonnal láthatók."
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: 6feac128-c728-4491-8b79-962da9a40788
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/13/2017
ms.author: cephalin
ms.openlocfilehash: ef3be9823384bd8dc978c86f5cfde00d1117c4a2
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="deploy-a-wordpress-app-in-the-azure-portal-in-five-minutes"></a><span data-ttu-id="61187-104">WordPress-alkalmazás üzembe helyezése az Azure Portalon öt perc alatt</span><span class="sxs-lookup"><span data-stu-id="61187-104">Deploy a WordPress app in the Azure portal in five minutes</span></span>

<span data-ttu-id="61187-105">Ez az oktatóanyag bemutatja, hogyan helyezhet üzembe az első alkalommal egy [WordPress](https://wordpress.org/)-webappot percek alatt az [Azure App Service](../app-service/app-service-value-prop-what-is.md) szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="61187-105">This tutorial shows you how to deploy your first [WordPress](https://wordpress.org/) web app to [Azure App Service](../app-service/app-service-value-prop-what-is.md) in minutes.</span></span>

![WordPress-webhely](./media/app-service-web-get-started-php-portal/wpdashboard.png)

## <a name="prerequisites"></a><span data-ttu-id="61187-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="61187-107">Prerequisites</span></span>
<span data-ttu-id="61187-108">Szüksége van egy Microsoft Azure-fiókra.</span><span class="sxs-lookup"><span data-stu-id="61187-108">You need a Microsoft Azure account.</span></span> <span data-ttu-id="61187-109">Ha nincs fiókja, [regisztráljon egy ingyenes próbaverzióra](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F), vagy [aktiválhatja a Visual Studio előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="61187-109">If you don't have an account, you can [sign up for a free trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) or [activate your Visual Studio subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span></span>

> [!NOTE]
> <span data-ttu-id="61187-110">Az [App Service kipróbálása](https://azure.microsoft.com/try/app-service/) Azure-fiók nélkül is lehetséges.</span><span class="sxs-lookup"><span data-stu-id="61187-110">You can [Try App Service](https://azure.microsoft.com/try/app-service/) without an Azure account.</span></span> <span data-ttu-id="61187-111">Hozzon létre egy kezdő szintű alkalmazást, amellyel legfeljebb egy óráig foglalkozhat – ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.</span><span class="sxs-lookup"><span data-stu-id="61187-111">Create a starter app and play with it for up to an hour--no credit card required, no commitments.</span></span>
> 
> 

## <a name="deploy-the-wordpress-app"></a><span data-ttu-id="61187-112">A WordPress-alkalmazás üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="61187-112">Deploy the WordPress app</span></span>
1. <span data-ttu-id="61187-113">Jelentkezzen be az [Azure Portalra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="61187-113">Sign in to the [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="61187-114">Nyissa meg a [https://portal.azure.com/#create/WordPress.WordPress](https://portal.azure.com/#create/WordPress.WordPress) webhelyet.</span><span class="sxs-lookup"><span data-stu-id="61187-114">Open [https://portal.azure.com/#create/WordPress.WordPress](https://portal.azure.com/#create/WordPress.WordPress).</span></span>

    <span data-ttu-id="61187-115">Ez a hivatkozás egy új WordPress-alkalmazás Azure Portalon való azonnali konfigurálására szolgál.</span><span class="sxs-lookup"><span data-stu-id="61187-115">This link is a shortcut to immediately configure a new WordPress app in the Azure portal.</span></span>

3. <span data-ttu-id="61187-116">Az **Alkalmazás neve** mezőben adja meg a webapp nevét.</span><span class="sxs-lookup"><span data-stu-id="61187-116">In **App name**, type a web app name.</span></span> <span data-ttu-id="61187-117">Ha a név egyedi az `azurewebsites.net` tartományban, megjelenik egy zöld pipa a jelölőnégyzetben.</span><span class="sxs-lookup"><span data-stu-id="61187-117">You will see a green checkmark in the box if the name is unique in the `azurewebsites.net` domain.</span></span>
   
5. <span data-ttu-id="61187-118">Az **Erőforráscsoport** menüben kattintson az **Új létrehozása** elemre egy új [erőforráscsoport](../azure-resource-manager/resource-group-overview.md) létrehozásához, majd nevezze el.</span><span class="sxs-lookup"><span data-stu-id="61187-118">In **Resource Group**, click **Create new** to create a new [resource group](../azure-resource-manager/resource-group-overview.md), then give it a name.</span></span>

6. <span data-ttu-id="61187-119">Az **Adatbázis-szolgáltató** területen válassza a **CleaDB** elemet.</span><span class="sxs-lookup"><span data-stu-id="61187-119">In **Database Provider**, select **CleaDB**.</span></span>

7. <span data-ttu-id="61187-120">Kattintson az **App Service-csomag/Hely** > **Új létrehozása** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="61187-120">Click **App Service plan/Location** > **Create New**.</span></span> <span data-ttu-id="61187-121">Konfigurálja az [App Service-csomagot](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="61187-121">Configure the [App Service plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) as shown:</span></span>

    - <span data-ttu-id="61187-122">Az **App Service-csomag** mezőbe írja be a kívánt nevet.</span><span class="sxs-lookup"><span data-stu-id="61187-122">In **App Service plan**, type the desired name.</span></span>
    - <span data-ttu-id="61187-123">A **Hely** mezőben válasszon ki egy helyet a csomag üzemeltetésére.</span><span class="sxs-lookup"><span data-stu-id="61187-123">In **Location**, choose a location to host your plan.</span></span>
    - <span data-ttu-id="61187-124">Kattintson a **Tarifacsomag** lehetőségre, majd válassza az **F1 – Ingyenes** elemet vagy egy másik megfelelő szintet, majd kattintson a **Kiválasztás** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="61187-124">Click **Pricing tier**, then select **F1 Free** or another tier that suits you, and then click **Select**.</span></span>
    - <span data-ttu-id="61187-125">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="61187-125">Click **OK**.</span></span>

8. <span data-ttu-id="61187-126">Kattintson az **Adatbázis** > **Új létrehozása** elemre.</span><span class="sxs-lookup"><span data-stu-id="61187-126">Click **Database** > **Create New**.</span></span> <span data-ttu-id="61187-127">Állítsa be az SQL Database adatbázist az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="61187-127">Configure the SQL Database as shown:</span></span>

    - <span data-ttu-id="61187-128">Az **Adatbázis neve** mezőben adjon meg egy adatbázisnevet.</span><span class="sxs-lookup"><span data-stu-id="61187-128">In **Database Name**, type a database name.</span></span> 
    - <span data-ttu-id="61187-129">A **Hely** mezőben válassza ki ugyanazt a helyet, amelyet az App Service-csomag számára is választott.</span><span class="sxs-lookup"><span data-stu-id="61187-129">In **Location**, choose the same location as the App Service plan.</span></span>
    - <span data-ttu-id="61187-130">Kattintson a **Tarifacsomag** lehetőségre, majd válassza a **Merkúr** elemet vagy egy másik megfelelő szintet, majd kattintson a **Kiválasztás** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="61187-130">Click **Pricing tier**, then select **Mercury** or another tier that suits you, and then click **Select**.</span></span>
    - <span data-ttu-id="61187-131">Válassza ki a **Jogi feltételek** elemet, és kattintson a **Vásárlás** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="61187-131">Click **Legal Terms** and click **Purchase**.</span></span>
    - <span data-ttu-id="61187-132">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="61187-132">Click **OK**.</span></span>

9. <span data-ttu-id="61187-133">Kattintson a **Létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="61187-133">Click **Create**.</span></span>

    <span data-ttu-id="61187-134">Az Azure ekkor létrehozza a WordPress-alkalmazást a konfiguráció alapján.</span><span class="sxs-lookup"><span data-stu-id="61187-134">Azure now creates your WordPress app based on your configuration.</span></span> <span data-ttu-id="61187-135">A **Központi telepítés elindítva...** értesítést kell látnia</span><span class="sxs-lookup"><span data-stu-id="61187-135">You should see a **Deployment started...** notification.</span></span>

    ![Az üzembe helyezés megkezdődött – az első WordPress az Azure App Service-ben](./media/app-service-web-get-started-php-portal/deployment-started.png)
   
## <a name="launch-and-manage-your-wordpress-web-app"></a><span data-ttu-id="61187-137">A WordPress-webalkalmazás elindítása és kezelése</span><span class="sxs-lookup"><span data-stu-id="61187-137">Launch and manage your WordPress web app</span></span>

<span data-ttu-id="61187-138">Ha az Azure befejezte az alkalmazás üzembe helyezését, megjelenik egy újabb értesítés.</span><span class="sxs-lookup"><span data-stu-id="61187-138">When Azure completes app deployment you see another notification.</span></span>

![Az üzembe helyezés sikerült– az első WordPress az Azure App Service-ben](./media/app-service-web-get-started-php-portal/deployment-succeeded.png)

1. <span data-ttu-id="61187-140">Kattintson az értesítésre.</span><span class="sxs-lookup"><span data-stu-id="61187-140">Click the notification.</span></span> <span data-ttu-id="61187-141">Ha lekéste, az értesítési csengőre kattintva mindig elérheti (![Értesítési csengő – az első WordPress az Azure App Service-ben](./media/app-service-web-get-started-dotnet-portal/notification.png)).</span><span class="sxs-lookup"><span data-stu-id="61187-141">If you missed it, you can always access it by clicking the notification bell (![Notification bellow - first WordPress in Azure App Service](./media/app-service-web-get-started-dotnet-portal/notification.png)).</span></span>

    <span data-ttu-id="61187-142">Ekkor meg kell jelennie a webapp felügyeleti [paneljének](../azure-resource-manager/resource-group-portal.md#manage-resources) (*panel*: egy vízszintesen megnyíló portáloldal).</span><span class="sxs-lookup"><span data-stu-id="61187-142">You should now see your web app's management [blade](../azure-resource-manager/resource-group-portal.md#manage-resources) (*blade*: a portal page that opens horizontally).</span></span>

3. <span data-ttu-id="61187-143">Az Áttekintés oldal tetején kattintson a **Tallózás** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="61187-143">In the top of the Overview page, click **Browse**.</span></span>
   
    ![Tallózás – az első WordPress az Azure App Service-ben](./media/app-service-web-get-started-php-portal/browse.png)

    <span data-ttu-id="61187-145">Ekkor megjelenik a WordPress **Kezdőlapja**.</span><span class="sxs-lookup"><span data-stu-id="61187-145">Now you see the WordPress **Welcome** page.</span></span> <span data-ttu-id="61187-146">Konfigurálja a WordPress-telepítést, és kísérletezzen vele!</span><span class="sxs-lookup"><span data-stu-id="61187-146">Configure the WordPress installation and start playing with it!</span></span>

    ![A Wordpress konfigurálása – az első WordPress az Azure App Service-ben](./media/app-service-web-get-started-php-portal/wordpress-config.png)
    
## <a name="next-steps"></a><span data-ttu-id="61187-148">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="61187-148">Next steps</span></span>
* <span data-ttu-id="61187-149">[Egy Laravel-webapp létrehozása, konfigurálása és üzembe helyezése az Azure-on](app-service-web-php-get-started.md) – Szert tehet a bármely PHP-webapp Azure-ban való futtatásához szükséges alapszintű készségekre, többek között az alábbiakra:</span><span class="sxs-lookup"><span data-stu-id="61187-149">[Create, configure, and deploy a Laravel web app to Azure](app-service-web-php-get-started.md) - Learn the basic skills you need to run any PHP web app in Azure, such as:</span></span>

    * <span data-ttu-id="61187-150">Alkalmazások létrehozása és konfigurálása az Azure-ban Powershellből/Bashből.</span><span class="sxs-lookup"><span data-stu-id="61187-150">Create and configure apps in Azure from PowerShell/Bash.</span></span>
    * <span data-ttu-id="61187-151">A PHP-verzió beállítása.</span><span class="sxs-lookup"><span data-stu-id="61187-151">Set PHP version.</span></span>
    * <span data-ttu-id="61187-152">Egy nem a gyökér-alkalmazáskönyvtárban található indítófájl használata.</span><span class="sxs-lookup"><span data-stu-id="61187-152">Use a start file that is not in the root application directory.</span></span>
    * <span data-ttu-id="61187-153">Composer-automatizálás engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="61187-153">Enable Composer automation.</span></span>
    * <span data-ttu-id="61187-154">Hozzáférés környezetspecifikus változókhoz.</span><span class="sxs-lookup"><span data-stu-id="61187-154">Access environment-specific variables.</span></span>
    * <span data-ttu-id="61187-155">Gyakori hibák elhárítása.</span><span class="sxs-lookup"><span data-stu-id="61187-155">Troubleshoot common errors.</span></span>

* <span data-ttu-id="61187-156">[A kód üzembe helyezése az Azure App Service-ben](web-sites-deploy.md) – Elsajátíthatja az FTP-ről vagy forráskezelő tárházból való üzembe helyezés készségét.</span><span class="sxs-lookup"><span data-stu-id="61187-156">[Deploy your code to Azure App Service](web-sites-deploy.md)- Learn how to deploy from FTP or from source control repositories.</span></span>
* <span data-ttu-id="61187-157">[Funkciók hozzáadása az első webapphoz](app-service-web-get-started-2.md) – Új szintre emelheti Azure alkalmazását.</span><span class="sxs-lookup"><span data-stu-id="61187-157">[Add functionality to your first web app](app-service-web-get-started-2.md) - Take your Azure app to the next level.</span></span> <span data-ttu-id="61187-158">Hitelesítheti felhasználóit.</span><span class="sxs-lookup"><span data-stu-id="61187-158">Authenticate your users.</span></span> <span data-ttu-id="61187-159">Igény szerint méretezheti.</span><span class="sxs-lookup"><span data-stu-id="61187-159">Scale it based on demand.</span></span> <span data-ttu-id="61187-160">Beállíthat a teljesítménnyel kapcsolatos riasztásokat.</span><span class="sxs-lookup"><span data-stu-id="61187-160">Set up some performance alerts.</span></span> <span data-ttu-id="61187-161">Mindezt csupán néhány kattintással.</span><span class="sxs-lookup"><span data-stu-id="61187-161">All with a few clicks.</span></span>
