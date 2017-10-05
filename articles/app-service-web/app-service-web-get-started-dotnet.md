---
title: "ASP.NET-webalkalmazás létrehozása az Azure-ban | Microsoft Docs"
description: "Az alapértelmezett ASP.NET-webalkalmazás üzembe helyezésével megtudhatja, hogy miként futtathat webalkalmazásokat az Azure App Service-ben."
services: app-service\web
documentationcenter: 
author: cephalin
manager: wpickett
editor: 
ms.assetid: b1e6bd58-48d1-4007-9d6c-53fd6db061e3
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 06/14/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 0f0035f6fef03ddcbb500b78f3445ced5b749808
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="create-an-aspnet-web-app-in-azure"></a><span data-ttu-id="a9174-103">ASP.NET-webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="a9174-103">Create an ASP.NET web app in Azure</span></span>

<span data-ttu-id="a9174-104">Az [Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) egy hatékonyan méretezhető, önjavító webes üzemeltetési szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="a9174-104">[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) provides a highly scalable, self-patching web hosting service.</span></span>  <span data-ttu-id="a9174-105">Ez a gyorsútmutató az ASP.NET-webalkalmazás Azure Web Apps szolgáltatásban történő üzembe helyezésén vezeti végig.</span><span class="sxs-lookup"><span data-stu-id="a9174-105">This quickstart shows how to deploy your first ASP.NET web app to Azure Web Apps.</span></span> <span data-ttu-id="a9174-106">Az oktatóanyag végére egy olyan erőforráscsoport lesz elérhető, amely egy App Service-csomagból és egy üzembe helyezett webalkalmazással rendelkező Azure webalkalmazásból áll.</span><span class="sxs-lookup"><span data-stu-id="a9174-106">When you're finished, you'll have a resource group that consists of an App Service plan and an Azure web app with a deployed web application.</span></span>

<span data-ttu-id="a9174-107">Tekintse meg a videót, amelyben működés közben láthatja ezt a gyorsútmutatót, majd kövesse végig a lépéseket az első .NET-alkalmazása közzétételéhez az Azure-on.</span><span class="sxs-lookup"><span data-stu-id="a9174-107">Watch the video to see this quickstart in action and then follow the steps yourself to publish your first .NET app on Azure.</span></span>

> [!VIDEO https://channel9.msdn.com/Shows/Azure-for-NET-Developers/Create-a-NET-app-in-Azure-Quickstart/player]

## <a name="prerequisites"></a><span data-ttu-id="a9174-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="a9174-108">Prerequisites</span></span>

<span data-ttu-id="a9174-109">Az oktatóanyag elvégzéséhez:</span><span class="sxs-lookup"><span data-stu-id="a9174-109">To complete this tutorial:</span></span>

* <span data-ttu-id="a9174-110">Telepítse a [Visual Studio 2017](https://www.visualstudio.com/downloads/) szoftvert a következő számítási feladatokkal:</span><span class="sxs-lookup"><span data-stu-id="a9174-110">Install [Visual Studio 2017](https://www.visualstudio.com/downloads/) with the following workloads:</span></span>
    - <span data-ttu-id="a9174-111">**ASP.NET és webfejlesztés**</span><span class="sxs-lookup"><span data-stu-id="a9174-111">**ASP.NET and web development**</span></span>
    - <span data-ttu-id="a9174-112">**Azure-fejlesztés**</span><span class="sxs-lookup"><span data-stu-id="a9174-112">**Azure development**</span></span>

    ![ASP.NET és webfejlesztés és Azure-fejlesztés (Web és felhőszolgáltatások alatt)](media/app-service-web-tutorial-dotnet-sqldatabase/workloads.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-an-aspnet-web-app"></a><span data-ttu-id="a9174-114">ASP.NET-webapp létrehozása</span><span class="sxs-lookup"><span data-stu-id="a9174-114">Create an ASP.NET web app</span></span>

<span data-ttu-id="a9174-115">Hozzon létre egy projektet a Visual Studióban a **File > New > Project** (Fájl > Új > Projekt) lehetőség kiválasztásával.</span><span class="sxs-lookup"><span data-stu-id="a9174-115">In Visual Studio, create a project by selecting **File > New > Project**.</span></span> 

<span data-ttu-id="a9174-116">A **New Project** (Új projekt) párbeszédpanelen válassza a **Visual C# > Web > ASP.NET Web Application (.NET Framework)** (ASP.NET-webalkalmazás (.NET-keretrendszer)) elemet.</span><span class="sxs-lookup"><span data-stu-id="a9174-116">In the **New Project** dialog, select **Visual C# > Web > ASP.NET Web Application (.NET Framework)**.</span></span>

<span data-ttu-id="a9174-117">Nevezze el az alkalmazást _myFirstAzureWebApp_ néven, majd kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="a9174-117">Name the application _myFirstAzureWebApp_, and then select **OK**.</span></span>
   
![A New Project (Új projekt) párbeszédpanel](./media/app-service-web-get-started-dotnet/new-project.png)

<span data-ttu-id="a9174-119">Bármilyen ASP.NET-webappot üzembe helyezhet az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="a9174-119">You can deploy any type of ASP.NET web app to Azure.</span></span> <span data-ttu-id="a9174-120">Ennél a gyorsútmutatónál válassza az **MVC** sablont, és ügyeljen arra, hogy a hitelesítés beállítása **No Authentication** (Nincs hitelesítés) legyen.</span><span class="sxs-lookup"><span data-stu-id="a9174-120">For this quickstart, select the **MVC** template, and make sure authentication is set to **No Authentication**.</span></span>
      
<span data-ttu-id="a9174-121">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="a9174-121">Select **OK**.</span></span>

![A New ASP.NET Project (Új ASP.NET-projekt) párbeszédpanel](./media/app-service-web-get-started-dotnet/select-mvc-template.png)

<span data-ttu-id="a9174-123">A menüből válassza a **Debug > Start without Debugging** (Hibakeresés > Indítás hibakeresés nélkül) lehetőséget a webalkalmazás helyi futtatásához.</span><span class="sxs-lookup"><span data-stu-id="a9174-123">From the menu, select **Debug > Start without Debugging** to run the web app locally.</span></span>

![Az alkalmazás futtatása helyileg](./media/app-service-web-get-started-dotnet/local-web-app.png)

## <a name="publish-to-azure"></a><span data-ttu-id="a9174-125">Közzététel az Azure platformon</span><span class="sxs-lookup"><span data-stu-id="a9174-125">Publish to Azure</span></span>

<span data-ttu-id="a9174-126">A **Solution Explorer** (Megoldáskezelő) lapon kattintson a jobb gombbal a **myFirstAzureWebApp** projektre, és válassza a **Publish** (Közzététel) elemet.</span><span class="sxs-lookup"><span data-stu-id="a9174-126">In the **Solution Explorer**, right-click the **myFirstAzureWebApp** project and select **Publish**.</span></span>

![Közzététel a Megoldáskezelőből](./media/app-service-web-get-started-dotnet/solution-explorer-publish.png)

<span data-ttu-id="a9174-128">Győződjön meg arról, hogy a **Microsoft Azure App Service** van kiválasztva, és kattintson a **Publish** (Közzététel) elemre.</span><span class="sxs-lookup"><span data-stu-id="a9174-128">Make sure that **Microsoft Azure App Service** is selected and select **Publish**.</span></span>

![Közzététel a projekt áttekintő oldaláról](./media/app-service-web-get-started-dotnet/publish-to-app-service.png)

<span data-ttu-id="a9174-130">Ez megnyitja a **Create App Service** (App Service létrehozása) párbeszédpanelt, amely segítségével létrehozhatja az összes, az ASP.NET-webalkalmazás Azure-ban való futtatásához szükséges Azure-erőforrást.</span><span class="sxs-lookup"><span data-stu-id="a9174-130">This opens the **Create App Service** dialog, which helps you create all the necessary Azure resources to run the ASP.NET web app in Azure.</span></span>

## <a name="sign-in-to-azure"></a><span data-ttu-id="a9174-131">Bejelentkezés az Azure-ba</span><span class="sxs-lookup"><span data-stu-id="a9174-131">Sign in to Azure</span></span>

<span data-ttu-id="a9174-132">A **Create App Service** (App Service létrehozása) párbeszédpanelen kattintson az **Add an account** (Fiók hozzáadása) gombra, majd jelentkezzen be az Azure-előfizetésébe.</span><span class="sxs-lookup"><span data-stu-id="a9174-132">In the **Create App Service** dialog, select **Add an account**, and sign in to your Azure subscription.</span></span> <span data-ttu-id="a9174-133">Ha már bejelentkezett, válassza ki a kívánt előfizetést tartalmazó fiókot a legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="a9174-133">If you're already signed in, select the account containing the desired subscription from the dropdown.</span></span>

> [!NOTE]
> <span data-ttu-id="a9174-134">Ha már be van jelentkezve, akkor még ne válassza a **Create** (Létrehozás) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="a9174-134">If you're already signed in, don't select **Create** yet.</span></span>
>
>
   
![Bejelentkezés az Azure-ba](./media/app-service-web-get-started-dotnet/sign-in-azure.png)

## <a name="create-a-resource-group"></a><span data-ttu-id="a9174-136">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="a9174-136">Create a resource group</span></span>

[!INCLUDE [resource group intro text](../../includes/resource-group.md)]

<span data-ttu-id="a9174-137">Az **Erőforráscsoport** mellett válassza az **Új** elemet.</span><span class="sxs-lookup"><span data-stu-id="a9174-137">Next to **Resource Group**, select **New**.</span></span>

<span data-ttu-id="a9174-138">Nevezze el a **myResourceGroup** erőforráscsoportot, majd kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="a9174-138">Name the resource group **myResourceGroup** and select **OK**.</span></span>

## <a name="create-an-app-service-plan"></a><span data-ttu-id="a9174-139">App Service-csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="a9174-139">Create an App Service plan</span></span>

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

<span data-ttu-id="a9174-140">Az **App Service Plan** (App Service-csomag) mellett válassza a **New** (Új) elemet.</span><span class="sxs-lookup"><span data-stu-id="a9174-140">Next to **App Service Plan**, select **New**.</span></span> 

<span data-ttu-id="a9174-141">A **Configure App Service Plan** (App Service-csomag konfigurálása) párbeszédpanelen a képernyőképet követve használja a táblázatban szereplő beállításokat.</span><span class="sxs-lookup"><span data-stu-id="a9174-141">In the **Configure App Service Plan** dialog, use the settings in the table following the screenshot.</span></span>

![App Service-csomag létrehozása](./media/app-service-web-get-started-dotnet/configure-app-service-plan.png)

| <span data-ttu-id="a9174-143">Beállítás</span><span class="sxs-lookup"><span data-stu-id="a9174-143">Setting</span></span> | <span data-ttu-id="a9174-144">Ajánlott érték</span><span class="sxs-lookup"><span data-stu-id="a9174-144">Suggested Value</span></span> | <span data-ttu-id="a9174-145">Leírás</span><span class="sxs-lookup"><span data-stu-id="a9174-145">Description</span></span> |
|-|-|-|
|<span data-ttu-id="a9174-146">App Service-csomag</span><span class="sxs-lookup"><span data-stu-id="a9174-146">App Service Plan</span></span>| <span data-ttu-id="a9174-147">myAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="a9174-147">myAppServicePlan</span></span> | <span data-ttu-id="a9174-148">Az App Service-csomag neve.</span><span class="sxs-lookup"><span data-stu-id="a9174-148">Name of the App Service plan.</span></span> |
| <span data-ttu-id="a9174-149">Hely</span><span class="sxs-lookup"><span data-stu-id="a9174-149">Location</span></span> | <span data-ttu-id="a9174-150">Nyugat-Európa</span><span class="sxs-lookup"><span data-stu-id="a9174-150">West Europe</span></span> | <span data-ttu-id="a9174-151">Az adatközpont, ahol a webalkalmazást üzemeltetik.</span><span class="sxs-lookup"><span data-stu-id="a9174-151">The datacenter where the web app is hosted.</span></span> |
| <span data-ttu-id="a9174-152">Méret</span><span class="sxs-lookup"><span data-stu-id="a9174-152">Size</span></span> | <span data-ttu-id="a9174-153">Ingyenes</span><span class="sxs-lookup"><span data-stu-id="a9174-153">Free</span></span> | <span data-ttu-id="a9174-154">A [tarifacsomag](https://azure.microsoft.com/pricing/details/app-service/) meghatározza az üzemeltetési funkciókat.</span><span class="sxs-lookup"><span data-stu-id="a9174-154">[Pricing tier](https://azure.microsoft.com/pricing/details/app-service/) determines hosting features.</span></span> |

<span data-ttu-id="a9174-155">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="a9174-155">Select **OK**.</span></span>

## <a name="create-and-publish-the-web-app"></a><span data-ttu-id="a9174-156">A webapp létrehozása és közzététele</span><span class="sxs-lookup"><span data-stu-id="a9174-156">Create and publish the web app</span></span>

<span data-ttu-id="a9174-157">A **Web App Name** (Webalkalmazás neve) mezőben adjon meg egy egyedi nevet az alkalmazáshoz (érvényes karakterek: `a-z`, `0-9` és `-`) vagy fogadja el az automatikusan létrehozott egyedi nevet.</span><span class="sxs-lookup"><span data-stu-id="a9174-157">In **Web App Name**, type a unique app name (valid characters are `a-z`, `0-9`, and `-`), or accept the automatically generated unique name.</span></span> <span data-ttu-id="a9174-158">A webalkalmazás URL-címe `http://<app_name>.azurewebsites.net`, amelyben a `<app_name>` a webalkalmazás neve.</span><span class="sxs-lookup"><span data-stu-id="a9174-158">The URL of the web app is `http://<app_name>.azurewebsites.net`, where `<app_name>` is your web app name.</span></span>

<span data-ttu-id="a9174-159">A **Create** (Létrehozás) gombra kattintva hozzákezdhet az Azure-erőforrások létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="a9174-159">Select **Create** to start creating the Azure resources.</span></span>

![A webapp nevének konfigurálása](./media/app-service-web-get-started-dotnet/web-app-name.png)

<span data-ttu-id="a9174-161">Miután a varázsló befejeződött, közzéteszi az ASP.NET webalkalmazást az Azure-on, majd elindítja azt az alapértelmezett böngészőben.</span><span class="sxs-lookup"><span data-stu-id="a9174-161">Once the wizard completes, it publishes the ASP.NET web app to Azure, and then launches the app in the default browser.</span></span>

![Közzétett ASP.NET-webapp az Azure-ban](./media/app-service-web-get-started-dotnet/published-azure-web-app.png)

<span data-ttu-id="a9174-163">A [létrehozás és közzététel lépésben](#create-and-publish-the-web-app) megadott webalkalmazás neve lesz az URL-előtag `http://<app_name>.azurewebsites.net` formátumban.</span><span class="sxs-lookup"><span data-stu-id="a9174-163">The web app name specified in the [create and publish step](#create-and-publish-the-web-app) is used as the URL prefix in the format `http://<app_name>.azurewebsites.net`.</span></span>

<span data-ttu-id="a9174-164">Gratulálunk, az ASP.NET-webalkalmazás immáron elérhető az Azure App Service-ben.</span><span class="sxs-lookup"><span data-stu-id="a9174-164">Congratulations, your ASP.NET web app is running live in Azure App Service.</span></span>

## <a name="update-the-app-and-redeploy"></a><span data-ttu-id="a9174-165">Az alkalmazás frissítése és ismételt üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="a9174-165">Update the app and redeploy</span></span>

<span data-ttu-id="a9174-166">A **Solution Explorer** (Megoldáskezelő) lapon nyissa meg a következőt: _Views\Home\Index.cshtml_.</span><span class="sxs-lookup"><span data-stu-id="a9174-166">From the **Solution Explorer**, open _Views\Home\Index.cshtml_.</span></span>

<span data-ttu-id="a9174-167">Keresse meg a `<div class="jumbotron">` HTML-címkét felül, és cserélje le az egész elemet az alábbi kódra:</span><span class="sxs-lookup"><span data-stu-id="a9174-167">Find the `<div class="jumbotron">` HTML tag near the top, and replace the entire element with the following code:</span></span>

```HTML
<div class="jumbotron">
    <h1>ASP.NET in Azure!</h1>
    <p class="lead">This is a simple app that we’ve built that demonstrates how to deploy a .NET app to Azure App Service.</p>
</div>
```

<span data-ttu-id="a9174-168">Az Azure-beli ismételt üzembe helyezéshez kattintson a jobb gombbal a **myFirstAzureWebApp** projektre a **Solution Explorer** (Megoldáskezelő) lapon, és válassza a **Publish** (Közzététel) elemet.</span><span class="sxs-lookup"><span data-stu-id="a9174-168">To redeploy to Azure, right-click the **myFirstAzureWebApp** project in **Solution Explorer** and select **Publish**.</span></span>

<span data-ttu-id="a9174-169">A közzétételi oldalon válassza a **Publish** (Közzététel) elemet.</span><span class="sxs-lookup"><span data-stu-id="a9174-169">In the publish page, select **Publish**.</span></span>

<span data-ttu-id="a9174-170">Miután a közzététel befejeződött, a Visual Studio tallózza a webalkalmazás URL-címét.</span><span class="sxs-lookup"><span data-stu-id="a9174-170">When publishing completes, Visual Studio launches a browser to the URL of the web app.</span></span>

![Frissített ASP.NET-webapp az Azure-ban](./media/app-service-web-get-started-dotnet/updated-azure-web-app.png)

## <a name="manage-the-azure-web-app"></a><span data-ttu-id="a9174-172">Az Azure webalkalmazás felügyelete</span><span class="sxs-lookup"><span data-stu-id="a9174-172">Manage the Azure web app</span></span>

<span data-ttu-id="a9174-173">Ugorjon az <a href="https://portal.azure.com" target="_blank">Azure Portalra</a>, és felügyelje a létrehozott webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="a9174-173">Go to the <a href="https://portal.azure.com" target="_blank">Azure portal</a> to manage the web app.</span></span>

<span data-ttu-id="a9174-174">A baloldali menüben válassza az **App Services** lehetőséget, majd az Azure-webalkalmazás nevét.</span><span class="sxs-lookup"><span data-stu-id="a9174-174">From the left menu, select **App Services**, and then select the name of your Azure web app.</span></span>

![Navigálás a portálon az Azure-webapphoz](./media/app-service-web-get-started-dotnet/access-portal.png)

<span data-ttu-id="a9174-176">Megtekintheti a webalkalmazás Áttekintés oldalát.</span><span class="sxs-lookup"><span data-stu-id="a9174-176">You see your web app's Overview page.</span></span> <span data-ttu-id="a9174-177">Itt elvégezhet olyan alapszintű felügyeleti feladatokat, mint a tallózás, leállítás, elindítás, újraindítás és törlés.</span><span class="sxs-lookup"><span data-stu-id="a9174-177">Here, you can perform basic management tasks like browse, stop, start, restart, and delete.</span></span> 

![Az App Service panel az Azure Portalon](./media/app-service-web-get-started-dotnet/web-app-blade.png)

<span data-ttu-id="a9174-179">A bal oldali menü az alkalmazás konfigurálásához biztosít különböző oldalakat.</span><span class="sxs-lookup"><span data-stu-id="a9174-179">The left menu provides different pages for configuring your app.</span></span> 

[!INCLUDE [Clean-up section](../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a><span data-ttu-id="a9174-180">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a9174-180">Next steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="a9174-181">ASP.NET-alkalmazás és SQL Database</span><span class="sxs-lookup"><span data-stu-id="a9174-181">ASP.NET with SQL Database</span></span>](app-service-web-tutorial-dotnet-sqldatabase.md)
