---
title: "egy ASP.NET Core webalkalmazásba a Visual Studio Code aaaCreate"
description: "Ez az oktatóanyag bemutatja, hogyan toocreate az ASP.NET Core webalkalmazás Visual Studio Code használatával."
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
ms.openlocfilehash: 1c18c94984d71e88d2a5b792d68cb1c81e4a96d8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-aspnet-core-web-app-in-visual-studio-code"></a><span data-ttu-id="6ea05-103">Az ASP.NET Core webalkalmazás létrehozása a Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6ea05-103">Create an ASP.NET Core web app in Visual Studio Code</span></span>
## <a name="overview"></a><span data-ttu-id="6ea05-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="6ea05-104">Overview</span></span>
<span data-ttu-id="6ea05-105">Az oktatóanyag bemutatja, hogyan toocreate az ASP.NET Core web app használatával [Visual Studio (kód VS)](http://code.visualstudio.com//Docs/whyvscode) , és telepítse azt túl[Azure App Service](../app-service/app-service-value-prop-what-is.md).</span><span class="sxs-lookup"><span data-stu-id="6ea05-105">This tutorial shows you how toocreate an ASP.NET Core web app using [Visual Studio Code (VS Code)](http://code.visualstudio.com//Docs/whyvscode) and deploy it too[Azure App Service](../app-service/app-service-value-prop-what-is.md).</span></span> 

> [!NOTE]
> <span data-ttu-id="6ea05-106">Bár ez a cikk tooweb alkalmazások hivatkozik, is érvényes tooAPI és mobilalkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="6ea05-106">Although this article refers tooweb apps, it also applies tooAPI apps and mobile apps.</span></span> 
> 
> 

<span data-ttu-id="6ea05-107">Az ASP.NET Core ASP.NET jelentős újratervezése.</span><span class="sxs-lookup"><span data-stu-id="6ea05-107">ASP.NET Core is a significant redesign of ASP.NET.</span></span> <span data-ttu-id="6ea05-108">Az ASP.NET Core egy új nyílt forráskódú és platformfüggetlen keretrendszer modern felhőalapú használó webalkalmazások .NET készítéséhez.</span><span class="sxs-lookup"><span data-stu-id="6ea05-108">ASP.NET Core is a new open-source and cross-platform framework for building modern cloud-based web apps using .NET.</span></span> <span data-ttu-id="6ea05-109">További információkért lásd: [bemutatása tooASP.NET Core](http://docs.asp.net/latest/conceptual-overview/aspnet.html).</span><span class="sxs-lookup"><span data-stu-id="6ea05-109">For more information, see [Introduction tooASP.NET Core](http://docs.asp.net/latest/conceptual-overview/aspnet.html).</span></span> <span data-ttu-id="6ea05-110">További információ az Azure App Service web apps: [webes alkalmazások – áttekintés](app-service-web-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6ea05-110">For information about Azure App Service web apps, see [Web Apps Overview](app-service-web-overview.md).</span></span>

[!INCLUDE [app-service-web-try-app-service.md](../../includes/app-service-web-try-app-service.md)]

## <a name="prerequisites"></a><span data-ttu-id="6ea05-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="6ea05-111">Prerequisites</span></span>
* <span data-ttu-id="6ea05-112">Telepítés [Visual STUDIO Code](http://code.visualstudio.com/Docs/setup).</span><span class="sxs-lookup"><span data-stu-id="6ea05-112">Install [VS Code](http://code.visualstudio.com/Docs/setup).</span></span>
* <span data-ttu-id="6ea05-113">Telepítse a Git - telepítheti ezeket a helyeket származó: [Chocolatey](https://chocolatey.org/packages/git) vagy [git-scm.com](http://git-scm.com/downloads). Ha új tooGit, kattintson az [git-scm.com](http://git-scm.com/downloads) , hello lehetőségen túl**a parancssort a Windows hello használata Git**.</span><span class="sxs-lookup"><span data-stu-id="6ea05-113">Install Git - You can install it from either of these locations: [Chocolatey](https://chocolatey.org/packages/git) or [git-scm.com](http://git-scm.com/downloads). If you are new tooGit, choose [git-scm.com](http://git-scm.com/downloads) and select hello option too**Use Git from hello Windows Command Prompt**.</span></span> <span data-ttu-id="6ea05-114">Miután telepíti a Git, biztosítani kell tooset hello Git felhasználói név és e-mail, szükség van hello oktatóanyag későbbi részében (végrehajtásakor a véglegesítési VS kódból).</span><span class="sxs-lookup"><span data-stu-id="6ea05-114">Once you install Git, you'll also need tooset hello Git user name and email as it's required later in hello tutorial (when performing a commit from VS Code).</span></span>  

## <a name="install-aspnet-core"></a><span data-ttu-id="6ea05-115">Az ASP.NET Core telepítése</span><span class="sxs-lookup"><span data-stu-id="6ea05-115">Install ASP.NET Core</span></span>
<span data-ttu-id="6ea05-116">Az ASP.NET Core egy lean .NET verem modern felhő- és OS X, Linux és Windows rendszeren futó webalkalmazások készítéséhez.</span><span class="sxs-lookup"><span data-stu-id="6ea05-116">ASP.NET Core is a lean .NET stack for building modern cloud and web apps that run on OS X, Linux, and Windows.</span></span> <span data-ttu-id="6ea05-117">Az készült másolatot tooprovide szabad hello a egy optimalizált fejlesztési keretrendszer vagy telepített toohello felhőalapú vagy helyszíni alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="6ea05-117">It has been built from hello ground up tooprovide an optimized development framework for apps that are either deployed toohello cloud or run on-premises.</span></span> <span data-ttu-id="6ea05-118">Azt moduláris összetevőből áll, a minimális terhelés mellett, így rugalmasságot megőrzi a megoldások létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="6ea05-118">It consists of modular components with minimal overhead, so you retain flexibility while constructing your solutions.</span></span>

<span data-ttu-id="6ea05-119">Ez az oktatóanyag használatba az ASP.NET Core hello legújabb fejlesztői verzióval rendelkező alkalmazások tervezett tooget.</span><span class="sxs-lookup"><span data-stu-id="6ea05-119">This tutorial is designed tooget you started building applications with hello latest development versions of ASP.NET Core.</span></span> <span data-ttu-id="6ea05-120">az utasításoknak hello adott tooWindows.</span><span class="sxs-lookup"><span data-stu-id="6ea05-120">hello following instructions are specific tooWindows.</span></span> <span data-ttu-id="6ea05-121">OS X, Linux és a Windows telepítési utasításokért lásd: [Ismerkedés az ASP.NET Core](https://docs.microsoft.com/aspnet/core/getting-started).</span><span class="sxs-lookup"><span data-stu-id="6ea05-121">For installation instructions on OS X, Linux, and Windows, see [Getting Started with ASP.NET Core](https://docs.microsoft.com/aspnet/core/getting-started).</span></span> 


> [!NOTE]
> <span data-ttu-id="6ea05-122">Az OS X, Linux és a Windows telepítési utasításokat, lásd: [telepíti az ASP.NET Core](https://code.visualstudio.com/Docs/ASPnet5#_installing-aspnet-5-and-dnx).</span><span class="sxs-lookup"><span data-stu-id="6ea05-122">For more detailed installation instructions for OS X, Linux, and Windows, see [Installing ASP.NET Core](https://code.visualstudio.com/Docs/ASPnet5#_installing-aspnet-5-and-dnx).</span></span> 
> 
> 

## <a name="create-hello-web-app"></a><span data-ttu-id="6ea05-123">Hello webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="6ea05-123">Create hello web app</span></span>
<span data-ttu-id="6ea05-124">Ez a szakasz bemutatja, hogyan tooscaffold egy új alkalmazást ASP.NET webalkalmazás hello .NET parancssori eszköz segítségével.</span><span class="sxs-lookup"><span data-stu-id="6ea05-124">This section shows you how tooscaffold a new app ASP.NET web app using hello .NET CLI tool.</span></span> 

1. <span data-ttu-id="6ea05-125">Adja meg a hello következő hello parancssor toocreate hello projekt mappa és scaffold hello app parancsot.</span><span class="sxs-lookup"><span data-stu-id="6ea05-125">Enter hello following at hello command prompt toocreate hello project folder and scaffold hello app.</span></span>
   
```terminal
mkdir SampleWebApp
cd SampleWebApp
dotnet new mvc
```
![CLI - ASP.NET Core generátor DotNet](./media/web-sites-create-web-app-using-vscode/dotnetcore-mvc-01.png)

2. <span data-ttu-id="6ea05-127">toorestore hello szükséges NuGet-csomagok, futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="6ea05-127">toorestore hello necessary NuGet packages, run hello following command:</span></span>
   
    ```terminal
    dotnet restore
    ```

## <a name="run-hello-web-app-locally"></a><span data-ttu-id="6ea05-128">Futtassa helyben a hello webalkalmazás</span><span class="sxs-lookup"><span data-stu-id="6ea05-128">Run hello web app locally</span></span>
<span data-ttu-id="6ea05-129">Hello webalkalmazás létrehozása, és minden hello NuGet-csomagok hello alkalmazás beolvasni, hello webalkalmazás helyileg is futtathatja.</span><span class="sxs-lookup"><span data-stu-id="6ea05-129">Now that you have created hello web app and retrieved all hello NuGet packages for hello app, you can run hello web app locally.</span></span>

1. <span data-ttu-id="6ea05-130">Hello alkalmazás futtatását (hello `dotnet run` parancs hello alkalmazást fog létrehozni, ha az elavult):</span><span class="sxs-lookup"><span data-stu-id="6ea05-130">Run hello app  (hello `dotnet run` command will build hello app when it's out of date):</span></span>
    ```terminal
    dotnet run
    ```
2. <span data-ttu-id="6ea05-131">Nyisson meg egy böngészőt, és keresse meg a következő URL-cím toohello.</span><span class="sxs-lookup"><span data-stu-id="6ea05-131">Open a browser and navigate toohello following URL.</span></span>
   
    <span data-ttu-id="6ea05-132">**http://localhost:5000**</span><span class="sxs-lookup"><span data-stu-id="6ea05-132">**http://localhost:5000**</span></span>
   
    <span data-ttu-id="6ea05-133">hello alapértelmezett lap hello webes alkalmazás a következőképpen jelenik.</span><span class="sxs-lookup"><span data-stu-id="6ea05-133">hello default page of hello web app will appear as follows.</span></span>
   
    ![A böngészőben a helyi webalkalmazás](./media/web-sites-create-web-app-using-vscode/08-web-app.png)
3. <span data-ttu-id="6ea05-135">Zárja be a böngészőt.</span><span class="sxs-lookup"><span data-stu-id="6ea05-135">Close your browser.</span></span> <span data-ttu-id="6ea05-136">A hello **parancsablakot**, nyomja le az ENTER **Ctrl + C** hello alkalmazás és Bezárás hello tooshut **parancsablakot**.</span><span class="sxs-lookup"><span data-stu-id="6ea05-136">In hello **Command Window**, press **Ctrl+C** tooshut down hello application and close hello **Command Window**.</span></span> 

## <a name="create-a-web-app-in-hello-azure-portal"></a><span data-ttu-id="6ea05-137">A webalkalmazás létrehozása az Azure portál hello</span><span class="sxs-lookup"><span data-stu-id="6ea05-137">Create a web app in hello Azure Portal</span></span>
<span data-ttu-id="6ea05-138">hello lépések végigvezeti Önt egy webalkalmazás létrehozása a hello Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="6ea05-138">hello following steps will guide you through creating a web app in hello Azure Portal.</span></span>

1. <span data-ttu-id="6ea05-139">Jelentkezzen be toohello [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="6ea05-139">Log in toohello [Azure Portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="6ea05-140">Kattintson a **új** hello a bal felső részén hello portálon.</span><span class="sxs-lookup"><span data-stu-id="6ea05-140">Click **NEW** at hello top left of hello Portal.</span></span>
3. <span data-ttu-id="6ea05-141">Kattintson a **webalkalmazások > webalkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="6ea05-141">Click **Web Apps > Web App**.</span></span>
   
    ![Új Azure webalkalmazás](./media/web-sites-create-web-app-using-vscode/09-azure-newwebapp.png)
4. <span data-ttu-id="6ea05-143">Adjon meg egy értéket a **neve**, például a **SampleWebAppDemo**.</span><span class="sxs-lookup"><span data-stu-id="6ea05-143">Enter a value for **Name**, such as **SampleWebAppDemo**.</span></span> <span data-ttu-id="6ea05-144">Vegye figyelembe, hogy ez a név toobe egyedi kell, és hello portal érvényesíti, amely tooenter hello neve tett kísérlet során.</span><span class="sxs-lookup"><span data-stu-id="6ea05-144">Note that this name needs toobe unique, and hello portal will enforce that when you attempt tooenter hello name.</span></span> <span data-ttu-id="6ea05-145">Ezért ha egy adjon meg egy másik értéket választja, lesz szüksége, amely érték toosubstitute minden egyes előfordulásakor **SampleWebAppDemo** ebben az oktatóanyagban látható.</span><span class="sxs-lookup"><span data-stu-id="6ea05-145">Therefore, if you select a enter a different value, you'll need toosubstitute that value for each occurrence of **SampleWebAppDemo** that you see in this tutorial.</span></span> 
5. <span data-ttu-id="6ea05-146">Válasszon ki egy létező **App Service-csomag** vagy hozzon létre egy újat.</span><span class="sxs-lookup"><span data-stu-id="6ea05-146">Select an existing **App Service Plan** or create a new one.</span></span> <span data-ttu-id="6ea05-147">Ha létrehoz egy új tervet, jelölje be a hello tarifacsomag, hely, és egyéb beállítások.</span><span class="sxs-lookup"><span data-stu-id="6ea05-147">If you create a new plan, select hello pricing tier, location, and other options.</span></span> <span data-ttu-id="6ea05-148">App Service-csomagokról a további információkért lásd: hello cikk [Azure App Service-csomagok részletes áttekintése](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6ea05-148">For more information on App Service plans, see hello article, [Azure App Service plans in-depth overview](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).</span></span>
   
    ![Az Azure új webalkalmazás panelen](./media/web-sites-create-web-app-using-vscode/10-azure-newappblade.png)
6. <span data-ttu-id="6ea05-150">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="6ea05-150">Click **Create**.</span></span>
   
    ![webalkalmazás panelen](./media/web-sites-create-web-app-using-vscode/11-azure-webappblade.png)

## <a name="enable-git-publishing-for-hello-new-web-app"></a><span data-ttu-id="6ea05-152">Hello új webalkalmazáshoz tartozó Git-közzététel engedélyezése</span><span class="sxs-lookup"><span data-stu-id="6ea05-152">Enable Git publishing for hello new web app</span></span>
<span data-ttu-id="6ea05-153">Git olyan elosztott verziókezelő rendszer használható toodeploy az Azure App Service webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="6ea05-153">Git is a distributed version control system that you can use toodeploy your Azure App Service web app.</span></span> <span data-ttu-id="6ea05-154">A webalkalmazás egy helyi Git-tárházban írt hello kódot fogja tárolni, és a kód tooAzure tooa távoli tárházba küldésével fogja telepíteni.</span><span class="sxs-lookup"><span data-stu-id="6ea05-154">You'll store hello code you write for your web app in a local Git repository, and you'll deploy your code tooAzure by pushing tooa remote repository.</span></span>   

1. <span data-ttu-id="6ea05-155">Jelentkezzen be a hello [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="6ea05-155">Log into hello [Azure Portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="6ea05-156">Kattintson a **Browse** (Tallózás) gombra.</span><span class="sxs-lookup"><span data-stu-id="6ea05-156">Click **Browse**.</span></span>
3. <span data-ttu-id="6ea05-157">Kattintson a **webalkalmazások** tooview hello webes alkalmazásokat az Azure-előfizetéshez társítva.</span><span class="sxs-lookup"><span data-stu-id="6ea05-157">Click **Web Apps** tooview a list of hello web apps associated with your Azure subscription.</span></span>
4. <span data-ttu-id="6ea05-158">Ebben az oktatóanyagban létrehozott hello webalkalmazás kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="6ea05-158">Select hello web app you created in this tutorial.</span></span>
5. <span data-ttu-id="6ea05-159">Hello webalkalmazás panelen kattintson **beállítások** > **folyamatos üzembe helyezés**.</span><span class="sxs-lookup"><span data-stu-id="6ea05-159">In hello web app blade, click **Settings** > **Continuous deployment**.</span></span> 
   
    ![Az Azure web app állomás](./media/web-sites-create-web-app-using-vscode/14-azure-deployment.png)
6. <span data-ttu-id="6ea05-161">Kattintson a **forrás kiválasztása > helyi Git-tárház**.</span><span class="sxs-lookup"><span data-stu-id="6ea05-161">Click **Choose Source > Local Git Repository**.</span></span>
7. <span data-ttu-id="6ea05-162">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="6ea05-162">Click **OK**.</span></span>
   
    ![Az Azure helyi Git-tárház](./media/web-sites-create-web-app-using-vscode/15-azure-localrepository.png)
8. <span data-ttu-id="6ea05-164">Ha korábban már nem beállított üzembe helyezési hitelesítő adatok webes alkalmazás vagy többi App Service alkalmazás-közzététel, állítsa be őket most:</span><span class="sxs-lookup"><span data-stu-id="6ea05-164">If you have not previously set up deployment credentials for publishing a web app or other App Service app, set them up now:</span></span>
   
   * <span data-ttu-id="6ea05-165">Kattintson a **beállítások** > **üzembe helyezési hitelesítő adatok**.</span><span class="sxs-lookup"><span data-stu-id="6ea05-165">Click **Settings** > **Deployment credentials**.</span></span> <span data-ttu-id="6ea05-166">Hello **üzembe helyezési hitelesítő adatok beállítása** panel fog megjelenni.</span><span class="sxs-lookup"><span data-stu-id="6ea05-166">hello **Set deployment credentials** blade will be displayed.</span></span>
   * <span data-ttu-id="6ea05-167">Hozzon létre felhasználónevet és jelszót.</span><span class="sxs-lookup"><span data-stu-id="6ea05-167">Create a user name and password.</span></span>  <span data-ttu-id="6ea05-168">Szüksége lesz a jelszót később Git beállítása során.</span><span class="sxs-lookup"><span data-stu-id="6ea05-168">You'll need this password later when setting up Git.</span></span>
   * <span data-ttu-id="6ea05-169">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="6ea05-169">Click **Save**.</span></span>
9. <span data-ttu-id="6ea05-170">A webalkalmazás panelen kattintson **beállítások > Tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="6ea05-170">In your web app's blade, click **Settings > Properties**.</span></span> <span data-ttu-id="6ea05-171">hello távoli Git-tárház, amely üzembe toois mezőben látható URL-címe hello **GIT URL-cím**.</span><span class="sxs-lookup"><span data-stu-id="6ea05-171">hello URL of hello remote Git repository that you'll deploy toois shown under **GIT URL**.</span></span>
10. <span data-ttu-id="6ea05-172">Másolás hello **GIT URL-cím** érték hello az oktatóanyag későbbi használatra.</span><span class="sxs-lookup"><span data-stu-id="6ea05-172">Copy hello **GIT URL** value for later use in hello tutorial.</span></span>
    
    ![Az Azure Git URL-cím](./media/web-sites-create-web-app-using-vscode/17-azure-giturl.png)

## <a name="publish-your-web-app-tooazure-app-service"></a><span data-ttu-id="6ea05-174">A webes alkalmazás tooAzure App Service közzététele</span><span class="sxs-lookup"><span data-stu-id="6ea05-174">Publish your web app tooAzure App Service</span></span>
<span data-ttu-id="6ea05-175">Ebben a szakaszban egy helyi Git-tárház létrehozása, és a webes alkalmazás tooAzure az adott tárház tooAzure toodeploy leküldéses.</span><span class="sxs-lookup"><span data-stu-id="6ea05-175">In this section, you will create a local Git repository and push from that repository tooAzure toodeploy your web app tooAzure.</span></span>

1. <span data-ttu-id="6ea05-176">A Visual STUDIO Code, válassza ki a hello **Git** hello bal oldali navigációs sávon a beállítást.</span><span class="sxs-lookup"><span data-stu-id="6ea05-176">In VS Code, select hello **Git** option in hello left navigation bar.</span></span>
   
    ![Visual STUDIO Code Git ikon](./media/web-sites-create-web-app-using-vscode/git-icon.png)
2. <span data-ttu-id="6ea05-178">Válassza ki **inicializálása git-tárház** toomake meg arról, hogy a munkaterület egy git verziókövetési.</span><span class="sxs-lookup"><span data-stu-id="6ea05-178">Select **Initialize git repository** toomake sure your workspace is under git source control.</span></span> 
   
    ![Git inicializálása](./media/web-sites-create-web-app-using-vscode/19-initgit.png)
3. <span data-ttu-id="6ea05-180">Nyissa meg a hello parancsablakot, és módosítsa a könyvtárakat toohello könyvtárához, a webes alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="6ea05-180">Open hello Command Window and change directories toohello directory of your web app.</span></span> <span data-ttu-id="6ea05-181">Ezután írja be a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="6ea05-181">Then, enter hello following command:</span></span>
   
        git config core.autocrlf false
   
    <span data-ttu-id="6ea05-182">Ez a parancs megakadályozza, hogy a szöveg, amelyben CRLF végződések javítása és Soremelés végződések javítása van szó kapcsolatos problémát.</span><span class="sxs-lookup"><span data-stu-id="6ea05-182">This command prevents an issue about text where CRLF endings and LF endings are involved.</span></span>
4. <span data-ttu-id="6ea05-183">Visual STUDIO Code, adja hozzá a véglegesítési üzenetet, majd kattintson a hello **véglegesítési összes** pipa ikon.</span><span class="sxs-lookup"><span data-stu-id="6ea05-183">In VS Code, add a commit message and click hello **Commit All** check icon.</span></span>
   
    ![Git összes véglegesítése](./media/web-sites-create-web-app-using-vscode/20-git-commit.png)
5. <span data-ttu-id="6ea05-185">Miután Git feldolgozása befejeződött, látni fogja, hogy nincsenek-e hello Git ablak felsorolt fájlok **módosítások**.</span><span class="sxs-lookup"><span data-stu-id="6ea05-185">After Git has completed processing, you'll see that there are no files listed in hello Git window under **Changes**.</span></span> 
   
    ![Git nem történtek változások](./media/web-sites-create-web-app-using-vscode/no-changes.png)
6. <span data-ttu-id="6ea05-187">Módosítsa a hátsó toohello, ahol a webalkalmazás hello parancssor mutat, ahol toohello directory parancsablakot.</span><span class="sxs-lookup"><span data-stu-id="6ea05-187">Change back toohello Command Window where hello command prompt points toohello directory where your web app is located.</span></span>
7. <span data-ttu-id="6ea05-188">Frissítések tooyour webalkalmazás tárházat hello korábban kimásolt Git URL (".git" végződő) segítségével távoli hivatkozás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="6ea05-188">Create a remote reference for pushing updates tooyour web app by using hello Git URL (ending in ".git") that you copied earlier.</span></span>
   
        git remote add azure [URL for remote repository]
8. <span data-ttu-id="6ea05-189">Konfigurálhatja a Git toosave a hitelesítő adatok helyben, hogy legyenek, automatikusan hozzáfűzött tooyour leküldéses parancsok által létrehozott Visual STUDIO Code.</span><span class="sxs-lookup"><span data-stu-id="6ea05-189">Configure Git toosave your credentials locally so that they will be automatically appended tooyour push commands generated from VS Code.</span></span>
   
        git config credential.helper store
9. <span data-ttu-id="6ea05-190">A módosítások tooAzure leküldéses hello a következő parancs beírásával.</span><span class="sxs-lookup"><span data-stu-id="6ea05-190">Push your changes tooAzure by entering hello following command.</span></span> <span data-ttu-id="6ea05-191">A kezdeti leküldéses tooAzure után nem tud toodo összes hello leküldéses parancsok VS kódból fogja.</span><span class="sxs-lookup"><span data-stu-id="6ea05-191">After this initial push tooAzure, you will be able toodo all hello push commands from VS Code.</span></span> 
   
        git push -u azure master
   
    <span data-ttu-id="6ea05-192">Korábban létrehozott Azure hello jelszó megadását kéri.</span><span class="sxs-lookup"><span data-stu-id="6ea05-192">You are prompted for hello password you created earlier in Azure.</span></span> <span data-ttu-id="6ea05-193">**Megjegyzés: A jelszó nem lesznek láthatók.**</span><span class="sxs-lookup"><span data-stu-id="6ea05-193">**Note: Your password will not be visible.**</span></span>
   
    <span data-ttu-id="6ea05-194">hello hello fent parancs kimenete egy üzenetet, hogy a telepítés sikeres végződik.</span><span class="sxs-lookup"><span data-stu-id="6ea05-194">hello output from hello above command ends with a message that deployment is successful.</span></span>
   
        remote: Deployment successful.
        toohttps://user@testsite.scm.azurewebsites.net/testsite.git
        [new branch]      master -> master

> [!NOTE]
> <span data-ttu-id="6ea05-195">Ha módosítások tooyour app, közvetlenül a Visual STUDIO Code funkciójával hello beépített Git hello kiválasztásával újbóli **véglegesíti az összes** beállítás követ hello **leküldéses** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="6ea05-195">If you make changes tooyour app, you can republish directly in VS Code using hello built-in Git functionality by selecting hello **Commit All** option followed by hello **Push** option.</span></span> <span data-ttu-id="6ea05-196">Hello található **leküldéses** beállítás elérhető, a legördülő menü következő toohello hello **véglegesíti az összes** és **frissítése** gombokat.</span><span class="sxs-lookup"><span data-stu-id="6ea05-196">You will find hello **Push** option available in hello drop-down menu next toohello **Commit All** and **Refresh** buttons.</span></span>
> 
> 

<span data-ttu-id="6ea05-197">Ha a projekt toocollaborate van szüksége, fontolja meg Between tooAzure küldését tooGitHub kérdez le.</span><span class="sxs-lookup"><span data-stu-id="6ea05-197">If you need toocollaborate on a project, you should consider pushing tooGitHub in between pushing tooAzure.</span></span>

## <a name="run-hello-app-in-azure"></a><span data-ttu-id="6ea05-198">Hello alkalmazás futtatása az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="6ea05-198">Run hello app in Azure</span></span>
<span data-ttu-id="6ea05-199">Most, hogy a webes alkalmazást telepített, akkor most amíg Azure-ban üzemeltetett hello alkalmazás futtatását.</span><span class="sxs-lookup"><span data-stu-id="6ea05-199">Now that you have deployed your web app, let's run hello app while hosted in Azure.</span></span> 

<span data-ttu-id="6ea05-200">Ez kétféleképpen teheti:</span><span class="sxs-lookup"><span data-stu-id="6ea05-200">This can be done in two ways:</span></span>

* <span data-ttu-id="6ea05-201">Nyisson meg egy böngészőt, és az alábbiak szerint adja meg a webalkalmazás hello nevét.</span><span class="sxs-lookup"><span data-stu-id="6ea05-201">Open a browser and enter hello name of your web app as follows.</span></span>   
  
        http://SampleWebAppDemo.azurewebsites.net
* <span data-ttu-id="6ea05-202">A hello Azure portál, hello webalkalmazás a webalkalmazás panelen keresse meg és kattintson a **Tallózás** tooview az alkalmazás</span><span class="sxs-lookup"><span data-stu-id="6ea05-202">In hello Azure Portal, locate hello web app blade for your web app, and click **Browse** tooview your app</span></span> 
* <span data-ttu-id="6ea05-203">az alapértelmezett böngészőben.</span><span class="sxs-lookup"><span data-stu-id="6ea05-203">in your default browser.</span></span>

![Azure-webalkalmazásban](./media/web-sites-create-web-app-using-vscode/21-azurewebapp.png)

## <a name="summary"></a><span data-ttu-id="6ea05-205">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="6ea05-205">Summary</span></span>
<span data-ttu-id="6ea05-206">Ebben az oktatóanyagban, megtudta, hogyan toocreate egy webalkalmazásba a Visual STUDIO Code, és telepítse azt tooAzure.</span><span class="sxs-lookup"><span data-stu-id="6ea05-206">In this tutorial, you learned how toocreate a web app in VS Code and deploy it tooAzure.</span></span> <span data-ttu-id="6ea05-207">Visual STUDIO Code kapcsolatos további információkért lásd: hello cikk [miért Visual Studio Code?](https://code.visualstudio.com/Docs/)</span><span class="sxs-lookup"><span data-stu-id="6ea05-207">For more information about VS Code, see hello article, [Why Visual Studio Code?](https://code.visualstudio.com/Docs/)</span></span> <span data-ttu-id="6ea05-208">További információ az App Service web apps: [webes alkalmazások – áttekintés](app-service-web-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6ea05-208">For information about App Service web apps, see [Web Apps Overview](app-service-web-overview.md).</span></span> 

