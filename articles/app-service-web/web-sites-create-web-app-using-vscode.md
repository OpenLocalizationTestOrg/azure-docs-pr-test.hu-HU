---
title: "Az ASP.NET Core webalkalmazás létrehozása a Visual Studio Code"
description: "Ez az oktatóanyag bemutatja, hogyan használja a Visual Studio Code az ASP.NET Core-webalkalmazás létrehozása."
services: app-service\web
documentationcenter: .net
author: erikre
manager: erikre
editor: jimbe
ms.assetid: 877bff08-9ef7-405a-a1ca-1194f33c55f2
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: dotnet
ms.devlang: na
ms.topic: article
ms.date: 02/26/2016
ms.author: cephalin
ms.openlocfilehash: 46e3852dc84265de41bb358f482dec06608e7efa
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-aspnet-core-web-app-in-visual-studio-code"></a><span data-ttu-id="79bd8-103">Az ASP.NET Core webalkalmazás létrehozása a Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="79bd8-103">Create an ASP.NET Core web app in Visual Studio Code</span></span>
## <a name="overview"></a><span data-ttu-id="79bd8-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="79bd8-104">Overview</span></span>
<span data-ttu-id="79bd8-105">Ez az oktatóanyag bemutatja, hogyan hozzon létre az ASP.NET Core web app [Visual Studio (kód VS)](http://code.visualstudio.com//Docs/whyvscode) és telepítheti azt [Azure App Service](../app-service/app-service-value-prop-what-is.md).</span><span class="sxs-lookup"><span data-stu-id="79bd8-105">This tutorial shows you how to create an ASP.NET Core web app using [Visual Studio Code (VS Code)](http://code.visualstudio.com//Docs/whyvscode) and deploy it to [Azure App Service](../app-service/app-service-value-prop-what-is.md).</span></span> 

> [!NOTE]
> <span data-ttu-id="79bd8-106">Habár ez a cikk a webalkalmazásokra vonatkozik, az API-alkalmazásokra és mobilalkalmazásokra egyaránt érvényes.</span><span class="sxs-lookup"><span data-stu-id="79bd8-106">Although this article refers to web apps, it also applies to API apps and mobile apps.</span></span> 
> 
> 

<span data-ttu-id="79bd8-107">Az ASP.NET Core ASP.NET jelentős újratervezése.</span><span class="sxs-lookup"><span data-stu-id="79bd8-107">ASP.NET Core is a significant redesign of ASP.NET.</span></span> <span data-ttu-id="79bd8-108">Az ASP.NET Core egy új nyílt forráskódú és platformfüggetlen keretrendszer modern felhőalapú használó webalkalmazások .NET készítéséhez.</span><span class="sxs-lookup"><span data-stu-id="79bd8-108">ASP.NET Core is a new open-source and cross-platform framework for building modern cloud-based web apps using .NET.</span></span> <span data-ttu-id="79bd8-109">További információkért lásd: [Bevezetés az ASP.NET Core](http://docs.asp.net/latest/conceptual-overview/aspnet.html).</span><span class="sxs-lookup"><span data-stu-id="79bd8-109">For more information, see [Introduction to ASP.NET Core](http://docs.asp.net/latest/conceptual-overview/aspnet.html).</span></span> <span data-ttu-id="79bd8-110">További információ az Azure App Service web apps: [webes alkalmazások – áttekintés](app-service-web-overview.md).</span><span class="sxs-lookup"><span data-stu-id="79bd8-110">For information about Azure App Service web apps, see [Web Apps Overview](app-service-web-overview.md).</span></span>

[!INCLUDE [app-service-web-try-app-service.md](../../includes/app-service-web-try-app-service.md)]

## <a name="prerequisites"></a><span data-ttu-id="79bd8-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="79bd8-111">Prerequisites</span></span>
* <span data-ttu-id="79bd8-112">Telepítés [Visual STUDIO Code](http://code.visualstudio.com/Docs/setup).</span><span class="sxs-lookup"><span data-stu-id="79bd8-112">Install [VS Code](http://code.visualstudio.com/Docs/setup).</span></span>
* <span data-ttu-id="79bd8-113">Telepítse a Git - telepítheti ezeket a helyeket származó: [Chocolatey](https://chocolatey.org/packages/git) vagy [git-scm.com](http://git-scm.com/downloads).</span><span class="sxs-lookup"><span data-stu-id="79bd8-113">Install Git - You can install it from either of these locations: [Chocolatey](https://chocolatey.org/packages/git) or [git-scm.com](http://git-scm.com/downloads).</span></span> <span data-ttu-id="79bd8-114">Ha most ismerkedik a Git, válassza a [git-scm.com](http://git-scm.com/downloads) , és jelölje be a **használható Git a Windows parancssor**.</span><span class="sxs-lookup"><span data-stu-id="79bd8-114">If you are new to Git, choose [git-scm.com](http://git-scm.com/downloads) and select the option to **Use Git from the Windows Command Prompt**.</span></span> <span data-ttu-id="79bd8-115">Miután telepíti a Git, is szüksége lesz a Git-felhasználó nevét és e-mail-, szükség van az oktatóanyag későbbi részében (végrehajtásakor a véglegesítési VS kódból).</span><span class="sxs-lookup"><span data-stu-id="79bd8-115">Once you install Git, you'll also need to set the Git user name and email as it's required later in the tutorial (when performing a commit from VS Code).</span></span>  

## <a name="install-aspnet-core"></a><span data-ttu-id="79bd8-116">Az ASP.NET Core telepítése</span><span class="sxs-lookup"><span data-stu-id="79bd8-116">Install ASP.NET Core</span></span>
<span data-ttu-id="79bd8-117">Az ASP.NET Core egy lean .NET verem modern felhő- és OS X, Linux és Windows rendszeren futó webalkalmazások készítéséhez.</span><span class="sxs-lookup"><span data-stu-id="79bd8-117">ASP.NET Core is a lean .NET stack for building modern cloud and web apps that run on OS X, Linux, and Windows.</span></span> <span data-ttu-id="79bd8-118">Az épített egészen az alapoktól egy optimalizált fejlesztési keretrendszer biztosít a felhőbe telepített vagy a helyszíni alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="79bd8-118">It has been built from the ground up to provide an optimized development framework for apps that are either deployed to the cloud or run on-premises.</span></span> <span data-ttu-id="79bd8-119">Azt moduláris összetevőből áll, a minimális terhelés mellett, így rugalmasságot megőrzi a megoldások létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="79bd8-119">It consists of modular components with minimal overhead, so you retain flexibility while constructing your solutions.</span></span>

<span data-ttu-id="79bd8-120">Ez az oktatóanyag célja, hogy elkezdhesse alkalmazásokat ASP.NET Core legújabb fejlesztői verzióját.</span><span class="sxs-lookup"><span data-stu-id="79bd8-120">This tutorial is designed to get you started building applications with the latest development versions of ASP.NET Core.</span></span> <span data-ttu-id="79bd8-121">Az alábbi utasítások alapján Windows vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="79bd8-121">The following instructions are specific to Windows.</span></span> <span data-ttu-id="79bd8-122">OS X, Linux és a Windows telepítési utasításokért lásd: [Ismerkedés az ASP.NET Core](https://docs.microsoft.com/aspnet/core/getting-started).</span><span class="sxs-lookup"><span data-stu-id="79bd8-122">For installation instructions on OS X, Linux, and Windows, see [Getting Started with ASP.NET Core](https://docs.microsoft.com/aspnet/core/getting-started).</span></span> 


> [!NOTE]
> <span data-ttu-id="79bd8-123">Az OS X, Linux és a Windows telepítési utasításokat, lásd: [telepíti az ASP.NET Core](https://code.visualstudio.com/Docs/ASPnet5#_installing-aspnet-5-and-dnx).</span><span class="sxs-lookup"><span data-stu-id="79bd8-123">For more detailed installation instructions for OS X, Linux, and Windows, see [Installing ASP.NET Core](https://code.visualstudio.com/Docs/ASPnet5#_installing-aspnet-5-and-dnx).</span></span> 
> 
> 

## <a name="create-the-web-app"></a><span data-ttu-id="79bd8-124">A webapp létrehozása</span><span class="sxs-lookup"><span data-stu-id="79bd8-124">Create the web app</span></span>
<span data-ttu-id="79bd8-125">Ez a szakasz bemutatja, hogyan generálja le egy új alkalmazást ASP.NET webalkalmazást a .NET parancssori eszközzel.</span><span class="sxs-lookup"><span data-stu-id="79bd8-125">This section shows you how to scaffold a new app ASP.NET web app using the .NET CLI tool.</span></span> 

1. <span data-ttu-id="79bd8-126">Adja meg a következő parancsot a parancssorba a projektmappa létrehozásához, és ezek az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="79bd8-126">Enter the following at the command prompt to create the project folder and scaffold the app.</span></span>
   
```terminal
mkdir SampleWebApp
cd SampleWebApp
dotnet new mvc
```
![CLI - ASP.NET Core generátor DotNet](./media/web-sites-create-web-app-using-vscode/dotnetcore-mvc-01.png)

2. <span data-ttu-id="79bd8-128">A szükséges NuGet-csomagok visszaállításához futtassa a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="79bd8-128">To restore the necessary NuGet packages, run the following command:</span></span>
   
    ```terminal
    dotnet restore
    ```

## <a name="run-the-web-app-locally"></a><span data-ttu-id="79bd8-129">Futtassa helyben a webes alkalmazás</span><span class="sxs-lookup"><span data-stu-id="79bd8-129">Run the web app locally</span></span>
<span data-ttu-id="79bd8-130">Most, hogy létrehozta a webalkalmazást és lekérése az alkalmazáshoz tartozó NuGet-csomagok, a webalkalmazást helyileg is futtathatja.</span><span class="sxs-lookup"><span data-stu-id="79bd8-130">Now that you have created the web app and retrieved all the NuGet packages for the app, you can run the web app locally.</span></span>

1. <span data-ttu-id="79bd8-131">Futtassa az alkalmazást (a `dotnet run` parancs fog létrehozni az alkalmazás elavult esetén):</span><span class="sxs-lookup"><span data-stu-id="79bd8-131">Run the app  (the `dotnet run` command will build the app when it's out of date):</span></span>
    ```terminal
    dotnet run
    ```
2. <span data-ttu-id="79bd8-132">Nyisson meg egy böngészőt, és keresse meg a következő URL-címet.</span><span class="sxs-lookup"><span data-stu-id="79bd8-132">Open a browser and navigate to the following URL.</span></span>
   
    <span data-ttu-id="79bd8-133">**http://localhost:5000**</span><span class="sxs-lookup"><span data-stu-id="79bd8-133">**http://localhost:5000**</span></span>
   
    <span data-ttu-id="79bd8-134">A webalkalmazás az alapértelmezett lapon jelenik meg az alábbiak szerint.</span><span class="sxs-lookup"><span data-stu-id="79bd8-134">The default page of the web app will appear as follows.</span></span>
   
    ![A böngészőben a helyi webalkalmazás](./media/web-sites-create-web-app-using-vscode/08-web-app.png)
3. <span data-ttu-id="79bd8-136">Zárja be a böngészőt.</span><span class="sxs-lookup"><span data-stu-id="79bd8-136">Close your browser.</span></span> <span data-ttu-id="79bd8-137">Az a **parancsablakot**, nyomja le az ENTER **Ctrl + C** az alkalmazás és a záró le kell állítania a **parancsablakot**.</span><span class="sxs-lookup"><span data-stu-id="79bd8-137">In the **Command Window**, press **Ctrl+C** to shut down the application and close the **Command Window**.</span></span> 

## <a name="create-a-web-app-in-the-azure-portal"></a><span data-ttu-id="79bd8-138">Webalkalmazás létrehozása az Azure portálon</span><span class="sxs-lookup"><span data-stu-id="79bd8-138">Create a web app in the Azure Portal</span></span>
<span data-ttu-id="79bd8-139">Az alábbi lépéseket végigvezeti Önt egy webalkalmazás létrehozása az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="79bd8-139">The following steps will guide you through creating a web app in the Azure Portal.</span></span>

1. <span data-ttu-id="79bd8-140">Jelentkezzen be az [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="79bd8-140">Log in to the [Azure Portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="79bd8-141">Kattintson a **új** a portál bal felső részén.</span><span class="sxs-lookup"><span data-stu-id="79bd8-141">Click **NEW** at the top left of the Portal.</span></span>
3. <span data-ttu-id="79bd8-142">Kattintson a **webalkalmazások > webalkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="79bd8-142">Click **Web Apps > Web App**.</span></span>
   
    ![Új Azure webalkalmazás](./media/web-sites-create-web-app-using-vscode/09-azure-newwebapp.png)
4. <span data-ttu-id="79bd8-144">Adjon meg egy értéket a **neve**, például a **SampleWebAppDemo**.</span><span class="sxs-lookup"><span data-stu-id="79bd8-144">Enter a value for **Name**, such as **SampleWebAppDemo**.</span></span> <span data-ttu-id="79bd8-145">Vegye figyelembe, hogy ez a név egyedinek kell lennie, és a portál érvényesíti, amely a nevének megadása tett kísérlet során.</span><span class="sxs-lookup"><span data-stu-id="79bd8-145">Note that this name needs to be unique, and the portal will enforce that when you attempt to enter the name.</span></span> <span data-ttu-id="79bd8-146">Ezért ha egy adjon meg egy másik értéket választja, meg kell helyettesítse be ezt az értéket minden egyes előfordulásakor **SampleWebAppDemo** ebben az oktatóanyagban látható.</span><span class="sxs-lookup"><span data-stu-id="79bd8-146">Therefore, if you select a enter a different value, you'll need to substitute that value for each occurrence of **SampleWebAppDemo** that you see in this tutorial.</span></span> 
5. <span data-ttu-id="79bd8-147">Válasszon ki egy létező **App Service-csomag** vagy hozzon létre egy újat.</span><span class="sxs-lookup"><span data-stu-id="79bd8-147">Select an existing **App Service Plan** or create a new one.</span></span> <span data-ttu-id="79bd8-148">Ha létrehoz egy új tervet, válassza ki a tarifacsomagot, helyét és egyéb beállítások.</span><span class="sxs-lookup"><span data-stu-id="79bd8-148">If you create a new plan, select the pricing tier, location, and other options.</span></span> <span data-ttu-id="79bd8-149">App Service-csomagokról a további információkért lásd: a cikk [Azure App Service-csomagok részletes áttekintése](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).</span><span class="sxs-lookup"><span data-stu-id="79bd8-149">For more information on App Service plans, see the article, [Azure App Service plans in-depth overview](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).</span></span>
   
    ![Az Azure új webalkalmazás panelen](./media/web-sites-create-web-app-using-vscode/10-azure-newappblade.png)
6. <span data-ttu-id="79bd8-151">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="79bd8-151">Click **Create**.</span></span>
   
    ![webalkalmazás panelen](./media/web-sites-create-web-app-using-vscode/11-azure-webappblade.png)

## <a name="enable-git-publishing-for-the-new-web-app"></a><span data-ttu-id="79bd8-153">Az új webalkalmazáshoz tartozó Git-közzététel engedélyezése</span><span class="sxs-lookup"><span data-stu-id="79bd8-153">Enable Git publishing for the new web app</span></span>
<span data-ttu-id="79bd8-154">Git olyan elosztott verziókezelő rendszer, amely segítségével az Azure App Service webalkalmazás telepítése.</span><span class="sxs-lookup"><span data-stu-id="79bd8-154">Git is a distributed version control system that you can use to deploy your Azure App Service web app.</span></span> <span data-ttu-id="79bd8-155">A webalkalmazásához írt kódot egy helyi Git-tárházban fogja tárolni, és a kódot az Azure-on a kód egy távoli tárházba történő küldésével fogja üzembe helyezni.</span><span class="sxs-lookup"><span data-stu-id="79bd8-155">You'll store the code you write for your web app in a local Git repository, and you'll deploy your code to Azure by pushing to a remote repository.</span></span>   

1. <span data-ttu-id="79bd8-156">Jelentkezzen be a [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="79bd8-156">Log into the [Azure Portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="79bd8-157">Kattintson a **Browse** (Tallózás) gombra.</span><span class="sxs-lookup"><span data-stu-id="79bd8-157">Click **Browse**.</span></span>
3. <span data-ttu-id="79bd8-158">Kattintson a **webalkalmazások** a web Apps, az Azure-előfizetéshez társított alkalmazások listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="79bd8-158">Click **Web Apps** to view a list of the web apps associated with your Azure subscription.</span></span>
4. <span data-ttu-id="79bd8-159">Ebben az oktatóanyagban létrehozott webalkalmazás kiválasztása</span><span class="sxs-lookup"><span data-stu-id="79bd8-159">Select the web app you created in this tutorial.</span></span>
5. <span data-ttu-id="79bd8-160">A webalkalmazás panelen kattintson **beállítások** > **folyamatos üzembe helyezés**.</span><span class="sxs-lookup"><span data-stu-id="79bd8-160">In the web app blade, click **Settings** > **Continuous deployment**.</span></span> 
   
    ![Az Azure web app állomás](./media/web-sites-create-web-app-using-vscode/14-azure-deployment.png)
6. <span data-ttu-id="79bd8-162">Kattintson a **forrás kiválasztása > helyi Git-tárház**.</span><span class="sxs-lookup"><span data-stu-id="79bd8-162">Click **Choose Source > Local Git Repository**.</span></span>
7. <span data-ttu-id="79bd8-163">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="79bd8-163">Click **OK**.</span></span>
   
    ![Az Azure helyi Git-tárház](./media/web-sites-create-web-app-using-vscode/15-azure-localrepository.png)
8. <span data-ttu-id="79bd8-165">Ha korábban már nem beállított üzembe helyezési hitelesítő adatok webes alkalmazás vagy többi App Service alkalmazás-közzététel, állítsa be őket most:</span><span class="sxs-lookup"><span data-stu-id="79bd8-165">If you have not previously set up deployment credentials for publishing a web app or other App Service app, set them up now:</span></span>
   
   * <span data-ttu-id="79bd8-166">Kattintson a **beállítások** > **üzembe helyezési hitelesítő adatok**.</span><span class="sxs-lookup"><span data-stu-id="79bd8-166">Click **Settings** > **Deployment credentials**.</span></span> <span data-ttu-id="79bd8-167">A **üzembe helyezési hitelesítő adatok beállítása** panel fog megjelenni.</span><span class="sxs-lookup"><span data-stu-id="79bd8-167">The **Set deployment credentials** blade will be displayed.</span></span>
   * <span data-ttu-id="79bd8-168">Hozzon létre felhasználónevet és jelszót.</span><span class="sxs-lookup"><span data-stu-id="79bd8-168">Create a user name and password.</span></span>  <span data-ttu-id="79bd8-169">Szüksége lesz a jelszót később Git beállítása során.</span><span class="sxs-lookup"><span data-stu-id="79bd8-169">You'll need this password later when setting up Git.</span></span>
   * <span data-ttu-id="79bd8-170">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="79bd8-170">Click **Save**.</span></span>
9. <span data-ttu-id="79bd8-171">A webalkalmazás panelen kattintson **beállítások > Tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="79bd8-171">In your web app's blade, click **Settings > Properties**.</span></span> <span data-ttu-id="79bd8-172">A távoli teheti elérhetővé a Git-tárház URL-címe alatt látható **GIT URL-cím**.</span><span class="sxs-lookup"><span data-stu-id="79bd8-172">The URL of the remote Git repository that you'll deploy to is shown under **GIT URL**.</span></span>
10. <span data-ttu-id="79bd8-173">Másolás a **GIT URL-cím** értéket az oktatóanyag későbbi használatra.</span><span class="sxs-lookup"><span data-stu-id="79bd8-173">Copy the **GIT URL** value for later use in the tutorial.</span></span>
    
    ![Az Azure Git URL-cím](./media/web-sites-create-web-app-using-vscode/17-azure-giturl.png)

## <a name="publish-your-web-app-to-azure-app-service"></a><span data-ttu-id="79bd8-175">A webalkalmazás közzététele az Azure App Service</span><span class="sxs-lookup"><span data-stu-id="79bd8-175">Publish your web app to Azure App Service</span></span>
<span data-ttu-id="79bd8-176">Ebben a szakaszban hoz létre egy helyi Git-tárház és leküldéses ebből az Azure-bA a webalkalmazás telepítése az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="79bd8-176">In this section, you will create a local Git repository and push from that repository to Azure to deploy your web app to Azure.</span></span>

1. <span data-ttu-id="79bd8-177">A Visual STUDIO Code, válassza ki a **Git** lehetőséget a bal oldali navigációs sávon.</span><span class="sxs-lookup"><span data-stu-id="79bd8-177">In VS Code, select the **Git** option in the left navigation bar.</span></span>
   
    ![Visual STUDIO Code Git ikon](./media/web-sites-create-web-app-using-vscode/git-icon.png)
2. <span data-ttu-id="79bd8-179">Válassza ki **inicializálása git-tárház** ellenőrizze, hogy a munkaterület git verziókezelő alatt.</span><span class="sxs-lookup"><span data-stu-id="79bd8-179">Select **Initialize git repository** to make sure your workspace is under git source control.</span></span> 
   
    ![Git inicializálása](./media/web-sites-create-web-app-using-vscode/19-initgit.png)
3. <span data-ttu-id="79bd8-181">Nyissa meg a parancsablakot, és módosítsa a könyvtárat könyvtárához, a webes alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="79bd8-181">Open the Command Window and change directories to the directory of your web app.</span></span> <span data-ttu-id="79bd8-182">Ezután írja be a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="79bd8-182">Then, enter the following command:</span></span>
   
        git config core.autocrlf false
   
    <span data-ttu-id="79bd8-183">Ez a parancs megakadályozza, hogy a szöveg, amelyben CRLF végződések javítása és Soremelés végződések javítása van szó kapcsolatos problémát.</span><span class="sxs-lookup"><span data-stu-id="79bd8-183">This command prevents an issue about text where CRLF endings and LF endings are involved.</span></span>
4. <span data-ttu-id="79bd8-184">Visual STUDIO Code, adja hozzá a véglegesítési üzenetet, majd kattintson a **véglegesítési összes** pipa ikon.</span><span class="sxs-lookup"><span data-stu-id="79bd8-184">In VS Code, add a commit message and click the **Commit All** check icon.</span></span>
   
    ![Git összes véglegesítése](./media/web-sites-create-web-app-using-vscode/20-git-commit.png)
5. <span data-ttu-id="79bd8-186">Miután Git feldolgozása befejeződött, látni fogja, hogy nincsenek-e a Git ablak felsorolt fájlok **módosítások**.</span><span class="sxs-lookup"><span data-stu-id="79bd8-186">After Git has completed processing, you'll see that there are no files listed in the Git window under **Changes**.</span></span> 
   
    ![Git nem történtek változások](./media/web-sites-create-web-app-using-vscode/no-changes.png)
6. <span data-ttu-id="79bd8-188">Állítsa vissza a hol helyezkedik el a webalkalmazás ahol arra a könyvtárra mutat a parancssor parancssori ablakban.</span><span class="sxs-lookup"><span data-stu-id="79bd8-188">Change back to the Command Window where the command prompt points to the directory where your web app is located.</span></span>
7. <span data-ttu-id="79bd8-189">A frissítések terjesztése a webalkalmazáshoz korábban kimásolt Git URL (".git" végződő) segítségével távoli hivatkozás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="79bd8-189">Create a remote reference for pushing updates to your web app by using the Git URL (ending in ".git") that you copied earlier.</span></span>
   
        git remote add azure [URL for remote repository]
8. <span data-ttu-id="79bd8-190">Helyileg mentheti a hitelesítő adatokat, így azok automatikusan hozzáfűzi a Visual STUDIO Code előállított leküldéses parancsok Git beállítható.</span><span class="sxs-lookup"><span data-stu-id="79bd8-190">Configure Git to save your credentials locally so that they will be automatically appended to your push commands generated from VS Code.</span></span>
   
        git config credential.helper store
9. <span data-ttu-id="79bd8-191">Továbbítsa a módosításokat az Azure, a következő parancs beírásával.</span><span class="sxs-lookup"><span data-stu-id="79bd8-191">Push your changes to Azure by entering the following command.</span></span> <span data-ttu-id="79bd8-192">Az Azure-ba, a kezdeti leküldéses után lesz a leküldéses parancsok ehhez a Visual STUDIO-kódjában.</span><span class="sxs-lookup"><span data-stu-id="79bd8-192">After this initial push to Azure, you will be able to do all the push commands from VS Code.</span></span> 
   
        git push -u azure master
   
    <span data-ttu-id="79bd8-193">Kéri a jelszót, amelyet korábban az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="79bd8-193">You are prompted for the password you created earlier in Azure.</span></span> <span data-ttu-id="79bd8-194">**Megjegyzés: A jelszó nem lesznek láthatók.**</span><span class="sxs-lookup"><span data-stu-id="79bd8-194">**Note: Your password will not be visible.**</span></span>
   
    <span data-ttu-id="79bd8-195">A fenti parancs kimenetében, hogy a telepítés sikeres üzenet végződik.</span><span class="sxs-lookup"><span data-stu-id="79bd8-195">The output from the above command ends with a message that deployment is successful.</span></span>
   
        remote: Deployment successful.
        To https://user@testsite.scm.azurewebsites.net/testsite.git
        [new branch]      master -> master

> [!NOTE]
> <span data-ttu-id="79bd8-196">Ha módosítja az alkalmazáshoz, közvetlenül a funkcióval a beépített Git kiválasztásával VS kód újbóli a **véglegesíti az összes** beállítás követi a **leküldéses** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="79bd8-196">If you make changes to your app, you can republish directly in VS Code using the built-in Git functionality by selecting the **Commit All** option followed by the **Push** option.</span></span> <span data-ttu-id="79bd8-197">Megtalálja a **leküldéses** beállítás található meg a legördülő menü mellett a **véglegesíti az összes** és **frissítése** gombokat.</span><span class="sxs-lookup"><span data-stu-id="79bd8-197">You will find the **Push** option available in the drop-down menu next to the **Commit All** and **Refresh** buttons.</span></span>
> 
> 

<span data-ttu-id="79bd8-198">Ha ülve dolgozhatnak van szüksége, fontolja meg a GitHub Between kérdez le, hogy Azure kérdez le.</span><span class="sxs-lookup"><span data-stu-id="79bd8-198">If you need to collaborate on a project, you should consider pushing to GitHub in between pushing to Azure.</span></span>

## <a name="run-the-app-in-azure"></a><span data-ttu-id="79bd8-199">Futtassa az alkalmazást az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="79bd8-199">Run the app in Azure</span></span>
<span data-ttu-id="79bd8-200">Most, hogy a webes alkalmazást telepített, akkor most futtassa az alkalmazást, miközben az Azure-ban üzemeltetett.</span><span class="sxs-lookup"><span data-stu-id="79bd8-200">Now that you have deployed your web app, let's run the app while hosted in Azure.</span></span> 

<span data-ttu-id="79bd8-201">Ez kétféleképpen teheti:</span><span class="sxs-lookup"><span data-stu-id="79bd8-201">This can be done in two ways:</span></span>

* <span data-ttu-id="79bd8-202">Nyisson meg egy böngészőt, és az alábbiak szerint adja meg a webalkalmazás nevét.</span><span class="sxs-lookup"><span data-stu-id="79bd8-202">Open a browser and enter the name of your web app as follows.</span></span>   
  
        http://SampleWebAppDemo.azurewebsites.net
* <span data-ttu-id="79bd8-203">Az Azure portálon keresse meg a webalkalmazás panelen a webalkalmazás, és kattintson **Tallózás** az alkalmazás megtekintéséhez</span><span class="sxs-lookup"><span data-stu-id="79bd8-203">In the Azure Portal, locate the web app blade for your web app, and click **Browse** to view your app</span></span> 
* <span data-ttu-id="79bd8-204">az alapértelmezett böngészőben.</span><span class="sxs-lookup"><span data-stu-id="79bd8-204">in your default browser.</span></span>

![Azure-webalkalmazásban](./media/web-sites-create-web-app-using-vscode/21-azurewebapp.png)

## <a name="summary"></a><span data-ttu-id="79bd8-206">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="79bd8-206">Summary</span></span>
<span data-ttu-id="79bd8-207">Ebben az oktatóprogramban megtanulhatta webalkalmazás létrehozása a Visual STUDIO Code, és telepítheti az Azure.</span><span class="sxs-lookup"><span data-stu-id="79bd8-207">In this tutorial, you learned how to create a web app in VS Code and deploy it to Azure.</span></span> <span data-ttu-id="79bd8-208">Visual STUDIO Code kapcsolatos további információkért lásd: a cikk [miért Visual Studio Code?](https://code.visualstudio.com/Docs/)</span><span class="sxs-lookup"><span data-stu-id="79bd8-208">For more information about VS Code, see the article, [Why Visual Studio Code?](https://code.visualstudio.com/Docs/)</span></span> <span data-ttu-id="79bd8-209">További információ az App Service web apps: [webes alkalmazások – áttekintés](app-service-web-overview.md).</span><span class="sxs-lookup"><span data-stu-id="79bd8-209">For information about App Service web apps, see [Web Apps Overview](app-service-web-overview.md).</span></span> 

