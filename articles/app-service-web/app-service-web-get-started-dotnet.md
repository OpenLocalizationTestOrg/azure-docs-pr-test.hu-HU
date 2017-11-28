---
title: "aaaCreate az ASP.NET webalkalmazás az Azure-ban |} Microsoft Docs"
description: "Ismerje meg, hogyan toorun webalkalmazásokat az Azure App Service telepítésével hello alapértelmezett ASP.NET webalkalmazás."
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
ms.openlocfilehash: eec916b3c32b6c8b68083177938c5c822a9782b8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-aspnet-web-app-in-azure"></a><span data-ttu-id="95dd3-103">ASP.NET-webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="95dd3-103">Create an ASP.NET web app in Azure</span></span>

<span data-ttu-id="95dd3-104">Az [Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) egy hatékonyan méretezhető, önjavító webes üzemeltetési szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="95dd3-104">[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) provides a highly scalable, self-patching web hosting service.</span></span>  <span data-ttu-id="95dd3-105">A gyors üzembe helyezés bemutatja, hogyan toodeploy az első ASP.NET web app tooAzure webalkalmazások.</span><span class="sxs-lookup"><span data-stu-id="95dd3-105">This quickstart shows how toodeploy your first ASP.NET web app tooAzure Web Apps.</span></span> <span data-ttu-id="95dd3-106">Az oktatóanyag végére egy olyan erőforráscsoport lesz elérhető, amely egy App Service-csomagból és egy üzembe helyezett webalkalmazással rendelkező Azure webalkalmazásból áll.</span><span class="sxs-lookup"><span data-stu-id="95dd3-106">When you're finished, you'll have a resource group that consists of an App Service plan and an Azure web app with a deployed web application.</span></span>

<span data-ttu-id="95dd3-107">Bemutató videó toosee hello a gyors üzembe helyezés, a művelet, és hajtsa végre hello lépések saját kezűleg toopublish az első .NET-alkalmazás az Azure-on.</span><span class="sxs-lookup"><span data-stu-id="95dd3-107">Watch hello video toosee this quickstart in action and then follow hello steps yourself toopublish your first .NET app on Azure.</span></span>

> [!VIDEO https://channel9.msdn.com/Shows/Azure-for-NET-Developers/Create-a-NET-app-in-Azure-Quickstart/player]

## <a name="prerequisites"></a><span data-ttu-id="95dd3-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="95dd3-108">Prerequisites</span></span>

<span data-ttu-id="95dd3-109">toocomplete Ez az oktatóanyag:</span><span class="sxs-lookup"><span data-stu-id="95dd3-109">toocomplete this tutorial:</span></span>

* <span data-ttu-id="95dd3-110">Telepítés [Visual Studio 2017](https://www.visualstudio.com/downloads/) az alábbi munkaterhelések hello:</span><span class="sxs-lookup"><span data-stu-id="95dd3-110">Install [Visual Studio 2017](https://www.visualstudio.com/downloads/) with hello following workloads:</span></span>
    - <span data-ttu-id="95dd3-111">**ASP.NET és webfejlesztés**</span><span class="sxs-lookup"><span data-stu-id="95dd3-111">**ASP.NET and web development**</span></span>
    - <span data-ttu-id="95dd3-112">**Azure-fejlesztés**</span><span class="sxs-lookup"><span data-stu-id="95dd3-112">**Azure development**</span></span>

    ![ASP.NET és webfejlesztés és Azure-fejlesztés (Web és felhőszolgáltatások alatt)](media/app-service-web-tutorial-dotnet-sqldatabase/workloads.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-an-aspnet-web-app"></a><span data-ttu-id="95dd3-114">ASP.NET-webapp létrehozása</span><span class="sxs-lookup"><span data-stu-id="95dd3-114">Create an ASP.NET web app</span></span>

<span data-ttu-id="95dd3-115">Hozzon létre egy projektet a Visual Studióban a **File > New > Project** (Fájl > Új > Projekt) lehetőség kiválasztásával.</span><span class="sxs-lookup"><span data-stu-id="95dd3-115">In Visual Studio, create a project by selecting **File > New > Project**.</span></span> 

<span data-ttu-id="95dd3-116">A hello **új projekt** párbeszédablakban válassza **Visual C# > Web > ASP.NET Web Application (.NET-keretrendszer)**.</span><span class="sxs-lookup"><span data-stu-id="95dd3-116">In hello **New Project** dialog, select **Visual C# > Web > ASP.NET Web Application (.NET Framework)**.</span></span>

<span data-ttu-id="95dd3-117">Hello alkalmazás neve _myFirstAzureWebApp_, majd válassza ki **OK**.</span><span class="sxs-lookup"><span data-stu-id="95dd3-117">Name hello application _myFirstAzureWebApp_, and then select **OK**.</span></span>
   
![A New Project (Új projekt) párbeszédpanel](./media/app-service-web-get-started-dotnet/new-project.png)

<span data-ttu-id="95dd3-119">Az ASP.NET web app tooAzure bármilyen típusú telepítése.</span><span class="sxs-lookup"><span data-stu-id="95dd3-119">You can deploy any type of ASP.NET web app tooAzure.</span></span> <span data-ttu-id="95dd3-120">A gyors üzembe helyezés, válassza a hello **MVC** sablont, és ellenőrizze, hogy a hitelesítés beállítása túl**nem hitelesítési**.</span><span class="sxs-lookup"><span data-stu-id="95dd3-120">For this quickstart, select hello **MVC** template, and make sure authentication is set too**No Authentication**.</span></span>
      
<span data-ttu-id="95dd3-121">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="95dd3-121">Select **OK**.</span></span>

![A New ASP.NET Project (Új ASP.NET-projekt) párbeszédpanel](./media/app-service-web-get-started-dotnet/select-mvc-template.png)

<span data-ttu-id="95dd3-123">Hello menüben válassza ki **hibakeresés > hibakeresés nélkül** toorun hello webalkalmazást helyileg.</span><span class="sxs-lookup"><span data-stu-id="95dd3-123">From hello menu, select **Debug > Start without Debugging** toorun hello web app locally.</span></span>

![Az alkalmazás futtatása helyileg](./media/app-service-web-get-started-dotnet/local-web-app.png)

## <a name="publish-tooazure"></a><span data-ttu-id="95dd3-125">TooAzure közzététele</span><span class="sxs-lookup"><span data-stu-id="95dd3-125">Publish tooAzure</span></span>

<span data-ttu-id="95dd3-126">A hello **Megoldáskezelőben**, kattintson a jobb gombbal hello **myFirstAzureWebApp** projektre, és válassza ki **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="95dd3-126">In hello **Solution Explorer**, right-click hello **myFirstAzureWebApp** project and select **Publish**.</span></span>

![Közzététel a Megoldáskezelőből](./media/app-service-web-get-started-dotnet/solution-explorer-publish.png)

<span data-ttu-id="95dd3-128">Győződjön meg arról, hogy a **Microsoft Azure App Service** van kiválasztva, és kattintson a **Publish** (Közzététel) elemre.</span><span class="sxs-lookup"><span data-stu-id="95dd3-128">Make sure that **Microsoft Azure App Service** is selected and select **Publish**.</span></span>

![Közzététel a projekt áttekintő oldaláról](./media/app-service-web-get-started-dotnet/publish-to-app-service.png)

<span data-ttu-id="95dd3-130">Ekkor megnyílik a hello **létrehozása az App Service** párbeszédpanel, amely segítséget nyújt az összes hello szükséges Azure-erőforrások toorun hello ASP.NET-webalkalmazás létrehozása az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="95dd3-130">This opens hello **Create App Service** dialog, which helps you create all hello necessary Azure resources toorun hello ASP.NET web app in Azure.</span></span>

## <a name="sign-in-tooazure"></a><span data-ttu-id="95dd3-131">Jelentkezzen be tooAzure</span><span class="sxs-lookup"><span data-stu-id="95dd3-131">Sign in tooAzure</span></span>

<span data-ttu-id="95dd3-132">A hello **létrehozása az App Service** párbeszédablakban válassza **vegyen fel egy fiókot**, és jelentkezzen be Azure-előfizetés tooyour.</span><span class="sxs-lookup"><span data-stu-id="95dd3-132">In hello **Create App Service** dialog, select **Add an account**, and sign in tooyour Azure subscription.</span></span> <span data-ttu-id="95dd3-133">Ha már bejelentkezett ezen, hello tartalmazó select hello fiók kívánt előfizetést hello legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="95dd3-133">If you're already signed in, select hello account containing hello desired subscription from hello dropdown.</span></span>

> [!NOTE]
> <span data-ttu-id="95dd3-134">Ha már be van jelentkezve, akkor még ne válassza a **Create** (Létrehozás) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="95dd3-134">If you're already signed in, don't select **Create** yet.</span></span>
>
>
   
![Jelentkezzen be tooAzure](./media/app-service-web-get-started-dotnet/sign-in-azure.png)

## <a name="create-a-resource-group"></a><span data-ttu-id="95dd3-136">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="95dd3-136">Create a resource group</span></span>

[!INCLUDE [resource group intro text](../../includes/resource-group.md)]

<span data-ttu-id="95dd3-137">Következő túl**erőforráscsoport**, jelölje be **új**.</span><span class="sxs-lookup"><span data-stu-id="95dd3-137">Next too**Resource Group**, select **New**.</span></span>

<span data-ttu-id="95dd3-138">Hello erőforráscsoport neve **myResourceGroup** válassza **OK**.</span><span class="sxs-lookup"><span data-stu-id="95dd3-138">Name hello resource group **myResourceGroup** and select **OK**.</span></span>

## <a name="create-an-app-service-plan"></a><span data-ttu-id="95dd3-139">App Service-csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="95dd3-139">Create an App Service plan</span></span>

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

<span data-ttu-id="95dd3-140">Következő túl**App Service-csomag**, jelölje be **új**.</span><span class="sxs-lookup"><span data-stu-id="95dd3-140">Next too**App Service Plan**, select **New**.</span></span> 

<span data-ttu-id="95dd3-141">A hello **konfigurálása App Service-csomag** párbeszédpanelen hello tábla a következő hello képernyőfelvétel a hello beállítások használata.</span><span class="sxs-lookup"><span data-stu-id="95dd3-141">In hello **Configure App Service Plan** dialog, use hello settings in hello table following hello screenshot.</span></span>

![App Service-csomag létrehozása](./media/app-service-web-get-started-dotnet/configure-app-service-plan.png)

| <span data-ttu-id="95dd3-143">Beállítás</span><span class="sxs-lookup"><span data-stu-id="95dd3-143">Setting</span></span> | <span data-ttu-id="95dd3-144">Ajánlott érték</span><span class="sxs-lookup"><span data-stu-id="95dd3-144">Suggested Value</span></span> | <span data-ttu-id="95dd3-145">Leírás</span><span class="sxs-lookup"><span data-stu-id="95dd3-145">Description</span></span> |
|-|-|-|
|<span data-ttu-id="95dd3-146">App Service-csomag</span><span class="sxs-lookup"><span data-stu-id="95dd3-146">App Service Plan</span></span>| <span data-ttu-id="95dd3-147">myAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="95dd3-147">myAppServicePlan</span></span> | <span data-ttu-id="95dd3-148">Hello App Service-csomag neve.</span><span class="sxs-lookup"><span data-stu-id="95dd3-148">Name of hello App Service plan.</span></span> |
| <span data-ttu-id="95dd3-149">Hely</span><span class="sxs-lookup"><span data-stu-id="95dd3-149">Location</span></span> | <span data-ttu-id="95dd3-150">Nyugat-Európa</span><span class="sxs-lookup"><span data-stu-id="95dd3-150">West Europe</span></span> | <span data-ttu-id="95dd3-151">hello adatközpont, amelyen hello webalkalmazás található.</span><span class="sxs-lookup"><span data-stu-id="95dd3-151">hello datacenter where hello web app is hosted.</span></span> |
| <span data-ttu-id="95dd3-152">Méret</span><span class="sxs-lookup"><span data-stu-id="95dd3-152">Size</span></span> | <span data-ttu-id="95dd3-153">Ingyenes</span><span class="sxs-lookup"><span data-stu-id="95dd3-153">Free</span></span> | <span data-ttu-id="95dd3-154">A [tarifacsomag](https://azure.microsoft.com/pricing/details/app-service/) meghatározza az üzemeltetési funkciókat.</span><span class="sxs-lookup"><span data-stu-id="95dd3-154">[Pricing tier](https://azure.microsoft.com/pricing/details/app-service/) determines hosting features.</span></span> |

<span data-ttu-id="95dd3-155">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="95dd3-155">Select **OK**.</span></span>

## <a name="create-and-publish-hello-web-app"></a><span data-ttu-id="95dd3-156">Hozzon létre és hello webes alkalmazás közzététele</span><span class="sxs-lookup"><span data-stu-id="95dd3-156">Create and publish hello web app</span></span>

<span data-ttu-id="95dd3-157">A **webalkalmazásnév**, írja be egy egyedi alkalmazásnévvel (érvényes karakterek: `a-z`, `0-9`, és `-`), vagy fogadja el a hello automatikusan hozott létre egyedi neve.</span><span class="sxs-lookup"><span data-stu-id="95dd3-157">In **Web App Name**, type a unique app name (valid characters are `a-z`, `0-9`, and `-`), or accept hello automatically generated unique name.</span></span> <span data-ttu-id="95dd3-158">hello webalkalmazás URL-címe hello `http://<app_name>.azurewebsites.net`, ahol `<app_name>` a webes alkalmazás neve.</span><span class="sxs-lookup"><span data-stu-id="95dd3-158">hello URL of hello web app is `http://<app_name>.azurewebsites.net`, where `<app_name>` is your web app name.</span></span>

<span data-ttu-id="95dd3-159">Válassza ki **létrehozása** toostart hello Azure-erőforrások létrehozása.</span><span class="sxs-lookup"><span data-stu-id="95dd3-159">Select **Create** toostart creating hello Azure resources.</span></span>

![A webapp nevének konfigurálása](./media/app-service-web-get-started-dotnet/web-app-name.png)

<span data-ttu-id="95dd3-161">Hello varázsló befejezése után hello ASP.NET web app tooAzure teszi közzé, és ezután elindítja hello app hello alapértelmezett böngészőben.</span><span class="sxs-lookup"><span data-stu-id="95dd3-161">Once hello wizard completes, it publishes hello ASP.NET web app tooAzure, and then launches hello app in hello default browser.</span></span>

![Közzétett ASP.NET-webapp az Azure-ban](./media/app-service-web-get-started-dotnet/published-azure-web-app.png)

<span data-ttu-id="95dd3-163">webalkalmazás neve hello hello megadott [létrehozása és közzététele lépés](#create-and-publish-the-web-app) használatos az URL-előtagját: hello formátumban hello `http://<app_name>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="95dd3-163">hello web app name specified in hello [create and publish step](#create-and-publish-the-web-app) is used as hello URL prefix in hello format `http://<app_name>.azurewebsites.net`.</span></span>

<span data-ttu-id="95dd3-164">Gratulálunk, az ASP.NET-webalkalmazás immáron elérhető az Azure App Service-ben.</span><span class="sxs-lookup"><span data-stu-id="95dd3-164">Congratulations, your ASP.NET web app is running live in Azure App Service.</span></span>

## <a name="update-hello-app-and-redeploy"></a><span data-ttu-id="95dd3-165">Frissítés hello alkalmazást, és helyezze üzembe újra</span><span class="sxs-lookup"><span data-stu-id="95dd3-165">Update hello app and redeploy</span></span>

<span data-ttu-id="95dd3-166">A hello **Megoldáskezelőben**, nyissa meg _Views\Home\Index.cshtml_.</span><span class="sxs-lookup"><span data-stu-id="95dd3-166">From hello **Solution Explorer**, open _Views\Home\Index.cshtml_.</span></span>

<span data-ttu-id="95dd3-167">Hello található `<div class="jumbotron">` HTML-címke közelében hello felső, és hello teljes elemet cserélje le a következő kód hello:</span><span class="sxs-lookup"><span data-stu-id="95dd3-167">Find hello `<div class="jumbotron">` HTML tag near hello top, and replace hello entire element with hello following code:</span></span>

```HTML
<div class="jumbotron">
    <h1>ASP.NET in Azure!</h1>
    <p class="lead">This is a simple app that we’ve built that demonstrates how toodeploy a .NET app tooAzure App Service.</p>
</div>
```

<span data-ttu-id="95dd3-168">tooredeploy tooAzure, kattintson a jobb gombbal hello **myFirstAzureWebApp** a projekt **Megoldáskezelőben** válassza **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="95dd3-168">tooredeploy tooAzure, right-click hello **myFirstAzureWebApp** project in **Solution Explorer** and select **Publish**.</span></span>

<span data-ttu-id="95dd3-169">A hello közzéteszi a lapot, jelölje be **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="95dd3-169">In hello publish page, select **Publish**.</span></span>

<span data-ttu-id="95dd3-170">Közzététel befejezését követően a Visual Studio elindítja egy böngésző toohello webalkalmazás URL-címe hello.</span><span class="sxs-lookup"><span data-stu-id="95dd3-170">When publishing completes, Visual Studio launches a browser toohello URL of hello web app.</span></span>

![Frissített ASP.NET-webapp az Azure-ban](./media/app-service-web-get-started-dotnet/updated-azure-web-app.png)

## <a name="manage-hello-azure-web-app"></a><span data-ttu-id="95dd3-172">Hello Azure web apphoz kezelése</span><span class="sxs-lookup"><span data-stu-id="95dd3-172">Manage hello Azure web app</span></span>

<span data-ttu-id="95dd3-173">Nyissa meg toohello <a href="https://portal.azure.com" target="_blank">Azure-portálon</a> toomanage hello webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="95dd3-173">Go toohello <a href="https://portal.azure.com" target="_blank">Azure portal</a> toomanage hello web app.</span></span>

<span data-ttu-id="95dd3-174">Hello bal oldali menüben válassza ki a **alkalmazásszolgáltatások**, majd válassza ki az Azure-webalkalmazásban hello nevét.</span><span class="sxs-lookup"><span data-stu-id="95dd3-174">From hello left menu, select **App Services**, and then select hello name of your Azure web app.</span></span>

![Portálnavigációjával tooAzure webalkalmazás](./media/app-service-web-get-started-dotnet/access-portal.png)

<span data-ttu-id="95dd3-176">Megtekintheti a webalkalmazás Áttekintés oldalát.</span><span class="sxs-lookup"><span data-stu-id="95dd3-176">You see your web app's Overview page.</span></span> <span data-ttu-id="95dd3-177">Itt elvégezhet olyan alapszintű felügyeleti feladatokat, mint a tallózás, leállítás, elindítás, újraindítás és törlés.</span><span class="sxs-lookup"><span data-stu-id="95dd3-177">Here, you can perform basic management tasks like browse, stop, start, restart, and delete.</span></span> 

![Az App Service panel az Azure Portalon](./media/app-service-web-get-started-dotnet/web-app-blade.png)

<span data-ttu-id="95dd3-179">hello bal oldali menü különböző oldalain biztosít az alkalmazás konfigurálását.</span><span class="sxs-lookup"><span data-stu-id="95dd3-179">hello left menu provides different pages for configuring your app.</span></span> 

[!INCLUDE [Clean-up section](../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a><span data-ttu-id="95dd3-180">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="95dd3-180">Next steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="95dd3-181">ASP.NET-alkalmazás és SQL Database</span><span class="sxs-lookup"><span data-stu-id="95dd3-181">ASP.NET with SQL Database</span></span>](app-service-web-tutorial-dotnet-sqldatabase.md)
