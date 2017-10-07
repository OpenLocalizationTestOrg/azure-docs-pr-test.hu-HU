---
title: "hello Azure-portálon öt perc múlva Umbraco webes alkalmazás aaaDeploy |} Microsoft Docs"
description: "Megtudhatja, milyen egyszerűen toorun webalkalmazásokkal az App Service-ben a minta az ASP.NET-alkalmazások üzembe helyezésével. Az eredmények azonnal láthatók."
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
ms.openlocfilehash: 6da45f99b3043a3684e3d99c14e6443d597b5212
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-an-umbraco-web-app-in-hello-azure-portal-in-five-minutes"></a><span data-ttu-id="3720a-104">Öt perc múlva a hello Azure-portálon Umbraco webes alkalmazás telepítése</span><span class="sxs-lookup"><span data-stu-id="3720a-104">Deploy an Umbraco web app in hello Azure portal in five minutes</span></span>

<span data-ttu-id="3720a-105">Ez az oktatóanyag segít telepíteni n [Umbraco](https://our.umbraco.org/) webalkalmazás túl[Azure App Service](../app-service/app-service-value-prop-what-is.md) perc múlva.</span><span class="sxs-lookup"><span data-stu-id="3720a-105">This tutorial helps you deploy n [Umbraco](https://our.umbraco.org/) web app too[Azure App Service](../app-service/app-service-value-prop-what-is.md) in minutes.</span></span>

![Umbraco-alkalmazás](./media/app-service-web-get-started-dotnet-portal/defaultpage.png)

## <a name="prerequisites"></a><span data-ttu-id="3720a-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="3720a-107">Prerequisites</span></span>
<span data-ttu-id="3720a-108">Szüksége van egy Microsoft Azure-fiókra.</span><span class="sxs-lookup"><span data-stu-id="3720a-108">You need a Microsoft Azure account.</span></span> <span data-ttu-id="3720a-109">Ha nincs fiókja, [regisztráljon egy ingyenes próbaverzióra](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F), vagy [aktiválhatja a Visual Studio előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="3720a-109">If you don't have an account, you can [sign up for a free trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) or [activate your Visual Studio subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span></span>

> [!NOTE]
> <span data-ttu-id="3720a-110">Az [App Service kipróbálása](https://azure.microsoft.com/try/app-service/) Azure-fiók nélkül is lehetséges.</span><span class="sxs-lookup"><span data-stu-id="3720a-110">You can [Try App Service](https://azure.microsoft.com/try/app-service/) without an Azure account.</span></span> <span data-ttu-id="3720a-111">Hozzon létre egy alapszintű alkalmazást, és nincs szükség bankkártyára, nem jár kötelezettségekkel tooan óra--be azt a lejátszásához.</span><span class="sxs-lookup"><span data-stu-id="3720a-111">Create a starter app and play with it for up tooan hour--no credit card required, no commitments.</span></span>
> 
> 

## <a name="deploy-hello-aspnet-app"></a><span data-ttu-id="3720a-112">Hello ASP.NET alkalmazás üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="3720a-112">Deploy hello ASP.NET app</span></span>
1. <span data-ttu-id="3720a-113">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="3720a-113">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="3720a-114">Nyissa meg a [https://portal.azure.com/#create/umbracoorg.UmbracoCMS](https://portal.azure.com/#create/umbracoorg.UmbracoCMS) webhelyet.</span><span class="sxs-lookup"><span data-stu-id="3720a-114">Open [https://portal.azure.com/#create/umbracoorg.UmbracoCMS](https://portal.azure.com/#create/umbracoorg.UmbracoCMS).</span></span>

    <span data-ttu-id="3720a-115">Ez a hivatkozás egy helyi tooimmediately egy új Umbraco-alkalmazás beállítása az Azure-portálon hello.</span><span class="sxs-lookup"><span data-stu-id="3720a-115">This link is a shortcut tooimmediately configure a new Umbraco app in hello Azure portal.</span></span>

3. <span data-ttu-id="3720a-116">Az **Alkalmazás neve** mezőben adja meg a webapp nevét.</span><span class="sxs-lookup"><span data-stu-id="3720a-116">In **App name**, type a web app name.</span></span> <span data-ttu-id="3720a-117">Látni fogja hello mezőben zöld pipa jelzi, ha hello név egyedi az hello `azurewebsites.net` tartomány.</span><span class="sxs-lookup"><span data-stu-id="3720a-117">You will see a green checkmark in hello box if hello name is unique in hello `azurewebsites.net` domain.</span></span>
   
5. <span data-ttu-id="3720a-118">A **erőforráscsoport**, kattintson a **hozzon létre új** új toocreate [erőforráscsoport](../azure-resource-manager/resource-group-overview.md), majd adjon neki egy nevet.</span><span class="sxs-lookup"><span data-stu-id="3720a-118">In **Resource Group**, click **Create new** toocreate a new [resource group](../azure-resource-manager/resource-group-overview.md), then give it a name.</span></span>

7. <span data-ttu-id="3720a-119">Kattintson az **App Service-csomag/Hely** > **Új létrehozása** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="3720a-119">Click **App Service plan/Location** > **Create New**.</span></span> <span data-ttu-id="3720a-120">Hello konfigurálása [App Service-csomag](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) látható módon:</span><span class="sxs-lookup"><span data-stu-id="3720a-120">Configure hello [App Service plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) as shown:</span></span>

    - <span data-ttu-id="3720a-121">A **App Service-csomag**, hello kívánt neve.</span><span class="sxs-lookup"><span data-stu-id="3720a-121">In **App Service plan**, type hello desired name.</span></span>
    - <span data-ttu-id="3720a-122">A **hely**, egy hely toohost a csomag kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="3720a-122">In **Location**, choose a location toohost your plan.</span></span>
    - <span data-ttu-id="3720a-123">Kattintson a **Tarifacsomag** lehetőségre, majd válassza az **F1 – Ingyenes** elemet vagy egy másik megfelelő szintet, majd kattintson a **Kiválasztás** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="3720a-123">Click **Pricing tier**, then select **F1 Free** or another tier that suits you, and then click **Select**.</span></span>
    - <span data-ttu-id="3720a-124">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="3720a-124">Click **OK**.</span></span>

    <span data-ttu-id="3720a-125">A Umbraco CMS konfigurációs mostantól a következő képernyőkép hello kell hasonlítania:</span><span class="sxs-lookup"><span data-stu-id="3720a-125">Your Umbraco CMS configuration should now look like hello following screenshot:</span></span>

    ![A konfiguráció folyamatban – az első Umbraco-alkalmazás az Azure App Service-ben](./media/app-service-web-get-started-dotnet-portal/configure-in-progress.png)

12. <span data-ttu-id="3720a-127">Kattintson az **SQL Database** > **Új adatbázis létrehozása** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="3720a-127">Click **SQL Database** > **Create a new database**.</span></span> <span data-ttu-id="3720a-128">Konfigurálja a hello SQL-adatbázis látható módon:</span><span class="sxs-lookup"><span data-stu-id="3720a-128">Configure hello SQL Database as shown:</span></span>

    - <span data-ttu-id="3720a-129">A **Név** mezőben írjon be egy nevet, például a **myDB**-t.</span><span class="sxs-lookup"><span data-stu-id="3720a-129">In **Name**, type a name, such as **myDB**.</span></span>
    - <span data-ttu-id="3720a-130">Kattintson a **Tarifacsomag** lehetőségre, majd válassza az **F – Ingyenes** elemet vagy egy másik megfelelő szintet, majd kattintson a **Kiválasztás** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="3720a-130">Click **Pricing tier**, then select **F Free** or another tier that suits you, and then click **Select**.</span></span>
    - <span data-ttu-id="3720a-131">Kattintson a **Célkiszogáló** > **Új kiszolgáló létrehozása** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="3720a-131">Click **Target server** > **Create a new server**.</span></span> <span data-ttu-id="3720a-132">Konfigurálja a hello adatbázis-kiszolgáló látható módon:</span><span class="sxs-lookup"><span data-stu-id="3720a-132">Configure hello database server as shown:</span></span>

        - <span data-ttu-id="3720a-133">A **Kiszolgálónév** mezőben adjon meg egy kiszolgálónevet.</span><span class="sxs-lookup"><span data-stu-id="3720a-133">In **Server name**, type a server name.</span></span> <span data-ttu-id="3720a-134">Látni fogja hello mezőben zöld pipa jelzi, ha hello név egyedi az hello `.database.windows.net` tartomány.</span><span class="sxs-lookup"><span data-stu-id="3720a-134">You will see a green checkmark in hello box if hello name is unique in hello `.database.windows.net` domain.</span></span>
        - <span data-ttu-id="3720a-135">A **kiszolgáló-rendszergazdai bejelentkezés**, típus hello rendszergazdán felhasználónév szükséges.</span><span class="sxs-lookup"><span data-stu-id="3720a-135">In **Server admin login**, type hello desired admininistrator username.</span></span>
        - <span data-ttu-id="3720a-136">A **jelszó** és **jelszó megerősítése**, írja be a hello kívánt jelszót.</span><span class="sxs-lookup"><span data-stu-id="3720a-136">In **Password** and **Confirm password**, type hello desired password.</span></span>
        - <span data-ttu-id="3720a-137">A hely kiválasztása hello hello webalkalmazás használhatja ugyanazon a helyen.</span><span class="sxs-lookup"><span data-stu-id="3720a-137">In Location, select hello same location you use for hello web app.</span></span>
        - <span data-ttu-id="3720a-138">Győződjön meg arról, hogy **engedélyezése az azure szolgáltatások tooaccess server** van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="3720a-138">Make sure **Allow azure services tooaccess server** is selected.</span></span>
        - <span data-ttu-id="3720a-139">Kattintson a **Kiválasztás** gombra.</span><span class="sxs-lookup"><span data-stu-id="3720a-139">Click **Select**.</span></span>
    
    - <span data-ttu-id="3720a-140">Kattintson a **Kiválasztás** gombra.</span><span class="sxs-lookup"><span data-stu-id="3720a-140">Click **Select**.</span></span>

13. <span data-ttu-id="3720a-141">Kattintson a **webalkalmazás-beállításainak**, adja meg a hello felhasználónevet és jelszót, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="3720a-141">Click **Web app settings**, specify hello database username and password, and click **OK**.</span></span>

    <span data-ttu-id="3720a-142">A Umbraco CMS konfigurációs mostantól a következő képernyőkép hello kell hasonlítania:</span><span class="sxs-lookup"><span data-stu-id="3720a-142">Your Umbraco CMS configuration should now look like hello following screenshot:</span></span>

    ![A konfiguráció kész – az első Umbraco-alkalmazás az Azure App Service-ben](./media/app-service-web-get-started-dotnet-portal/configure-complete.png)

14. <span data-ttu-id="3720a-144">Kattintson a  **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="3720a-144">Click **Create**.</span></span>
    
    <span data-ttu-id="3720a-145">Az Azure ekkor létrehozza az Umbraco-alkalmazást a konfiguráció alapján.</span><span class="sxs-lookup"><span data-stu-id="3720a-145">Azure now creates your Umbraco app based on your configuration.</span></span> <span data-ttu-id="3720a-146">A **Központi telepítés elindítva...** értesítést kell látnia</span><span class="sxs-lookup"><span data-stu-id="3720a-146">You should see a **Deployment started...** notification.</span></span>

    ![Az üzembe helyezés sikerült– az első Umbraco-alkalmazás az Azure App Service-ben](./media/app-service-web-get-started-dotnet-portal/deployment-started.png)
   
## <a name="launch-and-manage-your-umrbaco-web-app"></a><span data-ttu-id="3720a-148">Az Umbraco-alkalmazás elindítása és kezelése</span><span class="sxs-lookup"><span data-stu-id="3720a-148">Launch and manage your Umrbaco web app</span></span>

<span data-ttu-id="3720a-149">Ha az Azure befejezte az alkalmazás üzembe helyezését, megjelenik egy újabb értesítés.</span><span class="sxs-lookup"><span data-stu-id="3720a-149">When Azure completes app deployment you see another notification.</span></span>

![Az üzembe helyezés sikerült– az első Umbraco-alkalmazás az Azure App Service-ben](./media/app-service-web-get-started-dotnet-portal/deployment-succeeded.png)

1. <span data-ttu-id="3720a-151">Kattintson a hello értesítés.</span><span class="sxs-lookup"><span data-stu-id="3720a-151">Click hello notification.</span></span> <span data-ttu-id="3720a-152">Ha, kihagyva, akkor mindig elérhető legyen az hello értesítési harang gombra kattintva (![értesítési alábbi – az Azure App Service első Umbraco](./media/app-service-web-get-started-dotnet-portal/notification.png)).</span><span class="sxs-lookup"><span data-stu-id="3720a-152">If you missed it, you can always access it by clicking hello notification bell (![Notification bellow - first Umbraco in Azure App Service](./media/app-service-web-get-started-dotnet-portal/notification.png)).</span></span>

    <span data-ttu-id="3720a-153">Ekkor meg kell jelennie a webapp felügyeleti [paneljének](../azure-resource-manager/resource-group-portal.md#manage-resources) (*panel*: egy vízszintesen megnyíló portáloldal).</span><span class="sxs-lookup"><span data-stu-id="3720a-153">You should now see your web app's management [blade](../azure-resource-manager/resource-group-portal.md#manage-resources) (*blade*: a portal page that opens horizontally).</span></span>

3. <span data-ttu-id="3720a-154">Hello – áttekintés oldalra hello felső részén kattintson **Tallózás**.</span><span class="sxs-lookup"><span data-stu-id="3720a-154">In hello top of hello Overview page, click **Browse**.</span></span>
   
    ![Tallózás – az első Umbraco-alkalmazás az Azure App Service-ben](./media/app-service-web-get-started-dotnet-portal/browse.png)

    <span data-ttu-id="3720a-156">Most láthatja hello Umbraco **üdvözlő** lap.</span><span class="sxs-lookup"><span data-stu-id="3720a-156">Now you see hello Umbraco **Welcome** page.</span></span> <span data-ttu-id="3720a-157">Hello Umbraco telepítésének konfigurálása, és azt lejátszásához!</span><span class="sxs-lookup"><span data-stu-id="3720a-157">Configure hello Umbraco installation and start playing with it!</span></span>

    ![Az Umbraco konfigurálása – az első Umbraco-alkalmazás az Azure App Service-ben](./media/app-service-web-get-started-dotnet-portal/umbraco-config.png)
    
## <a name="next-steps"></a><span data-ttu-id="3720a-159">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3720a-159">Next steps</span></span>
* <span data-ttu-id="3720a-160">[Az ASP.NET web app tooAzure App Service szolgáltatásban a Visual Studio használatával telepítheti](app-service-web-get-started-dotnet.md) -megtudhatja, hogyan toocreate egy új Azure-webalkalmazásban a Visual Studio eszközből hello egyikét sem tartalmazza alkalmazássablonok.</span><span class="sxs-lookup"><span data-stu-id="3720a-160">[Deploy an ASP.NET web app tooAzure App Service, using Visual Studio](app-service-web-get-started-dotnet.md) - Learn how toocreate a new Azure web app from Visual Studio, using any one of hello included application templates.</span></span>
* <span data-ttu-id="3720a-161">[A kód tooAzure App Service telepítése](web-sites-deploy.md)-megtudhatja, hogyan szabályozza a toodeploy FTP vagy a forrás a tárházak találhatók.</span><span class="sxs-lookup"><span data-stu-id="3720a-161">[Deploy your code tooAzure App Service](web-sites-deploy.md)- Learn how toodeploy from FTP or from source control repositories.</span></span>
* <span data-ttu-id="3720a-162">[Funkció tooyour első webalkalmazás hozzáadása](app-service-web-get-started-2.md) -érvénybe az Azure alkalmazás toohello következő szint.</span><span class="sxs-lookup"><span data-stu-id="3720a-162">[Add functionality tooyour first web app](app-service-web-get-started-2.md) - Take your Azure app toohello next level.</span></span> <span data-ttu-id="3720a-163">Hitelesítheti felhasználóit.</span><span class="sxs-lookup"><span data-stu-id="3720a-163">Authenticate your users.</span></span> <span data-ttu-id="3720a-164">Igény szerint méretezheti.</span><span class="sxs-lookup"><span data-stu-id="3720a-164">Scale it based on demand.</span></span> <span data-ttu-id="3720a-165">Beállíthat a teljesítménnyel kapcsolatos riasztásokat.</span><span class="sxs-lookup"><span data-stu-id="3720a-165">Set up some performance alerts.</span></span> <span data-ttu-id="3720a-166">Mindezt csupán néhány kattintással.</span><span class="sxs-lookup"><span data-stu-id="3720a-166">All with a few clicks.</span></span>
