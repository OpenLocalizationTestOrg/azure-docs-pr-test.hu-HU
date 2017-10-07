---
title: "a WordPress alkalmazás hello Azure-portálon öt perc múlva aaaDeploy |} Microsoft Docs"
description: "Megtudhatja, milyen egyszerűen toorun webalkalmazásokkal az App Service-ben a WordPress alkalmazás telepítésével. Az eredmények azonnal láthatók."
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
ms.openlocfilehash: 4cd26bbacf89657997847ded1284e472288ddebe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-wordpress-app-in-hello-azure-portal-in-five-minutes"></a><span data-ttu-id="f7819-104">Öt perc múlva hello Azure-portálon a WordPress alkalmazás telepítése</span><span class="sxs-lookup"><span data-stu-id="f7819-104">Deploy a WordPress app in hello Azure portal in five minutes</span></span>

<span data-ttu-id="f7819-105">Az oktatóanyag bemutatja, hogyan toodeploy az első [WordPress](https://wordpress.org/) webalkalmazás túl[Azure App Service](../app-service/app-service-value-prop-what-is.md) perc múlva.</span><span class="sxs-lookup"><span data-stu-id="f7819-105">This tutorial shows you how toodeploy your first [WordPress](https://wordpress.org/) web app too[Azure App Service](../app-service/app-service-value-prop-what-is.md) in minutes.</span></span>

![WordPress-webhely](./media/app-service-web-get-started-php-portal/wpdashboard.png)

## <a name="prerequisites"></a><span data-ttu-id="f7819-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="f7819-107">Prerequisites</span></span>
<span data-ttu-id="f7819-108">Szüksége van egy Microsoft Azure-fiókra.</span><span class="sxs-lookup"><span data-stu-id="f7819-108">You need a Microsoft Azure account.</span></span> <span data-ttu-id="f7819-109">Ha nincs fiókja, [regisztráljon egy ingyenes próbaverzióra](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F), vagy [aktiválhatja a Visual Studio előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="f7819-109">If you don't have an account, you can [sign up for a free trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) or [activate your Visual Studio subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span></span>

> [!NOTE]
> <span data-ttu-id="f7819-110">Az [App Service kipróbálása](https://azure.microsoft.com/try/app-service/) Azure-fiók nélkül is lehetséges.</span><span class="sxs-lookup"><span data-stu-id="f7819-110">You can [Try App Service](https://azure.microsoft.com/try/app-service/) without an Azure account.</span></span> <span data-ttu-id="f7819-111">Hozzon létre egy alapszintű alkalmazást, és nincs szükség bankkártyára, nem jár kötelezettségekkel tooan óra--be azt a lejátszásához.</span><span class="sxs-lookup"><span data-stu-id="f7819-111">Create a starter app and play with it for up tooan hour--no credit card required, no commitments.</span></span>
> 
> 

## <a name="deploy-hello-wordpress-app"></a><span data-ttu-id="f7819-112">Hello WordPress alkalmazás üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="f7819-112">Deploy hello WordPress app</span></span>
1. <span data-ttu-id="f7819-113">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="f7819-113">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="f7819-114">Nyissa meg a [https://portal.azure.com/#create/WordPress.WordPress](https://portal.azure.com/#create/WordPress.WordPress) webhelyet.</span><span class="sxs-lookup"><span data-stu-id="f7819-114">Open [https://portal.azure.com/#create/WordPress.WordPress](https://portal.azure.com/#create/WordPress.WordPress).</span></span>

    <span data-ttu-id="f7819-115">Ez a hivatkozás egy helyi tooimmediately egy új WordPress-alkalmazás beállítása az hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="f7819-115">This link is a shortcut tooimmediately configure a new WordPress app in hello Azure portal.</span></span>

3. <span data-ttu-id="f7819-116">Az **Alkalmazás neve** mezőben adja meg a webapp nevét.</span><span class="sxs-lookup"><span data-stu-id="f7819-116">In **App name**, type a web app name.</span></span> <span data-ttu-id="f7819-117">Látni fogja hello mezőben zöld pipa jelzi, ha hello név egyedi az hello `azurewebsites.net` tartomány.</span><span class="sxs-lookup"><span data-stu-id="f7819-117">You will see a green checkmark in hello box if hello name is unique in hello `azurewebsites.net` domain.</span></span>
   
5. <span data-ttu-id="f7819-118">A **erőforráscsoport**, kattintson a **hozzon létre új** új toocreate [erőforráscsoport](../azure-resource-manager/resource-group-overview.md), majd adjon neki egy nevet.</span><span class="sxs-lookup"><span data-stu-id="f7819-118">In **Resource Group**, click **Create new** toocreate a new [resource group](../azure-resource-manager/resource-group-overview.md), then give it a name.</span></span>

6. <span data-ttu-id="f7819-119">Az **Adatbázis-szolgáltató** területen válassza a **CleaDB** elemet.</span><span class="sxs-lookup"><span data-stu-id="f7819-119">In **Database Provider**, select **CleaDB**.</span></span>

7. <span data-ttu-id="f7819-120">Kattintson az **App Service-csomag/Hely** > **Új létrehozása** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="f7819-120">Click **App Service plan/Location** > **Create New**.</span></span> <span data-ttu-id="f7819-121">Hello konfigurálása [App Service-csomag](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) látható módon:</span><span class="sxs-lookup"><span data-stu-id="f7819-121">Configure hello [App Service plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) as shown:</span></span>

    - <span data-ttu-id="f7819-122">A **App Service-csomag**, hello kívánt neve.</span><span class="sxs-lookup"><span data-stu-id="f7819-122">In **App Service plan**, type hello desired name.</span></span>
    - <span data-ttu-id="f7819-123">A **hely**, egy hely toohost a csomag kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="f7819-123">In **Location**, choose a location toohost your plan.</span></span>
    - <span data-ttu-id="f7819-124">Kattintson a **Tarifacsomag** lehetőségre, majd válassza az **F1 – Ingyenes** elemet vagy egy másik megfelelő szintet, majd kattintson a **Kiválasztás** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="f7819-124">Click **Pricing tier**, then select **F1 Free** or another tier that suits you, and then click **Select**.</span></span>
    - <span data-ttu-id="f7819-125">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="f7819-125">Click **OK**.</span></span>

8. <span data-ttu-id="f7819-126">Kattintson az **Adatbázis** > **Új létrehozása** elemre.</span><span class="sxs-lookup"><span data-stu-id="f7819-126">Click **Database** > **Create New**.</span></span> <span data-ttu-id="f7819-127">Konfigurálja a hello SQL-adatbázis látható módon:</span><span class="sxs-lookup"><span data-stu-id="f7819-127">Configure hello SQL Database as shown:</span></span>

    - <span data-ttu-id="f7819-128">Az **Adatbázis neve** mezőben adjon meg egy adatbázisnevet.</span><span class="sxs-lookup"><span data-stu-id="f7819-128">In **Database Name**, type a database name.</span></span> 
    - <span data-ttu-id="f7819-129">A **hely**, válassza a hello hello App Service-csomag megegyező helyen.</span><span class="sxs-lookup"><span data-stu-id="f7819-129">In **Location**, choose hello same location as hello App Service plan.</span></span>
    - <span data-ttu-id="f7819-130">Kattintson a **Tarifacsomag** lehetőségre, majd válassza a **Merkúr** elemet vagy egy másik megfelelő szintet, majd kattintson a **Kiválasztás** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="f7819-130">Click **Pricing tier**, then select **Mercury** or another tier that suits you, and then click **Select**.</span></span>
    - <span data-ttu-id="f7819-131">Válassza ki a **Jogi feltételek** elemet, és kattintson a **Vásárlás** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="f7819-131">Click **Legal Terms** and click **Purchase**.</span></span>
    - <span data-ttu-id="f7819-132">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="f7819-132">Click **OK**.</span></span>

9. <span data-ttu-id="f7819-133">Kattintson a **Létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="f7819-133">Click **Create**.</span></span>

    <span data-ttu-id="f7819-134">Az Azure ekkor létrehozza a WordPress-alkalmazást a konfiguráció alapján.</span><span class="sxs-lookup"><span data-stu-id="f7819-134">Azure now creates your WordPress app based on your configuration.</span></span> <span data-ttu-id="f7819-135">A **Központi telepítés elindítva...** értesítést kell látnia</span><span class="sxs-lookup"><span data-stu-id="f7819-135">You should see a **Deployment started...** notification.</span></span>

    ![Az üzembe helyezés megkezdődött – az első WordPress az Azure App Service-ben](./media/app-service-web-get-started-php-portal/deployment-started.png)
   
## <a name="launch-and-manage-your-wordpress-web-app"></a><span data-ttu-id="f7819-137">A WordPress-webalkalmazás elindítása és kezelése</span><span class="sxs-lookup"><span data-stu-id="f7819-137">Launch and manage your WordPress web app</span></span>

<span data-ttu-id="f7819-138">Ha az Azure befejezte az alkalmazás üzembe helyezését, megjelenik egy újabb értesítés.</span><span class="sxs-lookup"><span data-stu-id="f7819-138">When Azure completes app deployment you see another notification.</span></span>

![Az üzembe helyezés sikerült– az első WordPress az Azure App Service-ben](./media/app-service-web-get-started-php-portal/deployment-succeeded.png)

1. <span data-ttu-id="f7819-140">Kattintson a hello értesítés.</span><span class="sxs-lookup"><span data-stu-id="f7819-140">Click hello notification.</span></span> <span data-ttu-id="f7819-141">Ha, kihagyva, akkor mindig elérhető legyen az hello értesítési harang gombra kattintva (![értesítési alábbi – az Azure App Service első WordPress](./media/app-service-web-get-started-dotnet-portal/notification.png)).</span><span class="sxs-lookup"><span data-stu-id="f7819-141">If you missed it, you can always access it by clicking hello notification bell (![Notification bellow - first WordPress in Azure App Service](./media/app-service-web-get-started-dotnet-portal/notification.png)).</span></span>

    <span data-ttu-id="f7819-142">Ekkor meg kell jelennie a webapp felügyeleti [paneljének](../azure-resource-manager/resource-group-portal.md#manage-resources) (*panel*: egy vízszintesen megnyíló portáloldal).</span><span class="sxs-lookup"><span data-stu-id="f7819-142">You should now see your web app's management [blade](../azure-resource-manager/resource-group-portal.md#manage-resources) (*blade*: a portal page that opens horizontally).</span></span>

3. <span data-ttu-id="f7819-143">Hello – áttekintés oldalra hello felső részén kattintson **Tallózás**.</span><span class="sxs-lookup"><span data-stu-id="f7819-143">In hello top of hello Overview page, click **Browse**.</span></span>
   
    ![Tallózás – az első WordPress az Azure App Service-ben](./media/app-service-web-get-started-php-portal/browse.png)

    <span data-ttu-id="f7819-145">Most megjelenik a hello WordPress **üdvözlő** lap.</span><span class="sxs-lookup"><span data-stu-id="f7819-145">Now you see hello WordPress **Welcome** page.</span></span> <span data-ttu-id="f7819-146">Hello WordPress telepítésének konfigurálása, és azt lejátszásához!</span><span class="sxs-lookup"><span data-stu-id="f7819-146">Configure hello WordPress installation and start playing with it!</span></span>

    ![A Wordpress konfigurálása – az első WordPress az Azure App Service-ben](./media/app-service-web-get-started-php-portal/wordpress-config.png)
    
## <a name="next-steps"></a><span data-ttu-id="f7819-148">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f7819-148">Next steps</span></span>
* <span data-ttu-id="f7819-149">[Létrehozása, konfigurálása és telepítése a Laravel web app tooAzure](app-service-web-php-get-started.md) -további hello alapvető ismeretek kell toorun bármely PHP webalkalmazást az Azure, például:</span><span class="sxs-lookup"><span data-stu-id="f7819-149">[Create, configure, and deploy a Laravel web app tooAzure](app-service-web-php-get-started.md) - Learn hello basic skills you need toorun any PHP web app in Azure, such as:</span></span>

    * <span data-ttu-id="f7819-150">Alkalmazások létrehozása és konfigurálása az Azure-ban Powershellből/Bashből.</span><span class="sxs-lookup"><span data-stu-id="f7819-150">Create and configure apps in Azure from PowerShell/Bash.</span></span>
    * <span data-ttu-id="f7819-151">A PHP-verzió beállítása.</span><span class="sxs-lookup"><span data-stu-id="f7819-151">Set PHP version.</span></span>
    * <span data-ttu-id="f7819-152">A start-fájl, amely nincs hello alkalmazás gyökérkönyvtárában használható.</span><span class="sxs-lookup"><span data-stu-id="f7819-152">Use a start file that is not in hello root application directory.</span></span>
    * <span data-ttu-id="f7819-153">Composer-automatizálás engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="f7819-153">Enable Composer automation.</span></span>
    * <span data-ttu-id="f7819-154">Hozzáférés környezetspecifikus változókhoz.</span><span class="sxs-lookup"><span data-stu-id="f7819-154">Access environment-specific variables.</span></span>
    * <span data-ttu-id="f7819-155">Gyakori hibák elhárítása.</span><span class="sxs-lookup"><span data-stu-id="f7819-155">Troubleshoot common errors.</span></span>

* <span data-ttu-id="f7819-156">[A kód tooAzure App Service telepítése](web-sites-deploy.md)-megtudhatja, hogyan szabályozza a toodeploy FTP vagy a forrás a tárházak találhatók.</span><span class="sxs-lookup"><span data-stu-id="f7819-156">[Deploy your code tooAzure App Service](web-sites-deploy.md)- Learn how toodeploy from FTP or from source control repositories.</span></span>
* <span data-ttu-id="f7819-157">[Funkció tooyour első webalkalmazás hozzáadása](app-service-web-get-started-2.md) -érvénybe az Azure alkalmazás toohello következő szint.</span><span class="sxs-lookup"><span data-stu-id="f7819-157">[Add functionality tooyour first web app](app-service-web-get-started-2.md) - Take your Azure app toohello next level.</span></span> <span data-ttu-id="f7819-158">Hitelesítheti felhasználóit.</span><span class="sxs-lookup"><span data-stu-id="f7819-158">Authenticate your users.</span></span> <span data-ttu-id="f7819-159">Igény szerint méretezheti.</span><span class="sxs-lookup"><span data-stu-id="f7819-159">Scale it based on demand.</span></span> <span data-ttu-id="f7819-160">Beállíthat a teljesítménnyel kapcsolatos riasztásokat.</span><span class="sxs-lookup"><span data-stu-id="f7819-160">Set up some performance alerts.</span></span> <span data-ttu-id="f7819-161">Mindezt csupán néhány kattintással.</span><span class="sxs-lookup"><span data-stu-id="f7819-161">All with a few clicks.</span></span>
