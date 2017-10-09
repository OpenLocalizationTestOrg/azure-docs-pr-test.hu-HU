---
title: a Web App Linux .NET Core aaaUse |} Microsoft Docs
description: "Használja a .NET Core Linux Web App alkalmazásban."
keywords: "az Azure app service, webalkalmazás, dotnet, mag, linux, oss"
services: app-service
documentationCenter: 
authors: michimune, rachelappel
manager: erikre
editor: 
ms.assetid: c02959e6-7220-496a-a417-9b2147638e2e
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: aelnably;wesmc;mikono;rachelap
ms.openlocfilehash: 9b7fb7185dff2c99ed88e7937d455177504937b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-net-core-in-an-azure-web-app-on-linux"></a><span data-ttu-id="298cc-104">.NET Core használja az Azure-webalkalmazás Linux rendszeren</span><span class="sxs-lookup"><span data-stu-id="298cc-104">Use .NET Core in an Azure web app on Linux</span></span> #

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

<span data-ttu-id="298cc-105">[Webalkalmazás](https://docs.microsoft.com/azure/app-service-web/app-service-linux-intro) Linux biztosít egy jól skálázható, önálló javítási üzemeltetési webszolgáltatás hello Linux operációs rendszert használ.</span><span class="sxs-lookup"><span data-stu-id="298cc-105">[Web App](https://docs.microsoft.com/azure/app-service-web/app-service-linux-intro) on Linux provides a highly scalable, self-patching web hosting service using hello Linux operating system.</span></span> <span data-ttu-id="298cc-106">Ez az oktatóanyag bemutató részletes útmutatást tartalmaz hogyan toocreate egy [.NET Core](https://docs.microsoft.com/aspnet/core/) alkalmazást az Azure-webalkalmazásban Linux rendszeren.</span><span class="sxs-lookup"><span data-stu-id="298cc-106">This tutorial contains step-by-step instructions showing how toocreate a [.NET Core](https://docs.microsoft.com/aspnet/core/) app on Azure web app on Linux.</span></span> 

![Webappok Linuxon][10]

<span data-ttu-id="298cc-108">A lépésekkel hello Mac, a Windows vagy Linux rendszerű gépek használatának alatt.</span><span class="sxs-lookup"><span data-stu-id="298cc-108">You can follow hello steps below using a Mac, Windows, or Linux machine.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="298cc-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="298cc-109">Prerequisites</span></span> ##

<span data-ttu-id="298cc-110">toocomplete Ez az oktatóanyag:</span><span class="sxs-lookup"><span data-stu-id="298cc-110">toocomplete this tutorial:</span></span> 

* <span data-ttu-id="298cc-111">Telepítse a hello [.NET Core SDK](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="298cc-111">Install hello [.NET Core SDK](https://www.microsoft.com/net/download/core).</span></span>
* <span data-ttu-id="298cc-112">Telepítés [Git](https://git-scm.com/downloads).</span><span class="sxs-lookup"><span data-stu-id="298cc-112">Install [Git](https://git-scm.com/downloads).</span></span>

[!INCLUDE [Free trial note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-local-net-core-application"></a><span data-ttu-id="298cc-113">Helyi .NET Core-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="298cc-113">Create a local .NET Core application</span></span> ##

<span data-ttu-id="298cc-114">Egy új terminál-munkamenet elindításához.</span><span class="sxs-lookup"><span data-stu-id="298cc-114">Start a new terminal session.</span></span> <span data-ttu-id="298cc-115">Hozzon létre egy könyvtárat nevű `hellodotnetcore`, és módosítsa a hello aktuális directory tooit.</span><span class="sxs-lookup"><span data-stu-id="298cc-115">Create a directory named `hellodotnetcore`, and change hello current directory tooit.</span></span> <span data-ttu-id="298cc-116">Írja be a következő hello:</span><span class="sxs-lookup"><span data-stu-id="298cc-116">Then type hello following:</span></span> 

```
dotnet new web
``` 

  <span data-ttu-id="298cc-117">Ezzel a paranccsal létrejön három fájlok (*hellodotnetcore.csproj*, *Program.cs*, és *Startup.cs*) és egy üres mappa (*wwwroot /*) hello aktuális könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="298cc-117">This command creates three files (*hellodotnetcore.csproj*, *Program.cs*, and *Startup.cs*) and one empty folder (*wwwroot/*) under hello current directory.</span></span> <span data-ttu-id="298cc-118">a tartalom hello `.csproj` fájl hello hasonlóan kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="298cc-118">hello content of `.csproj` file should look like hello following:</span></span> 

```xml
  <!-- Empty lines are omitted. -->

  <Project Sdk="Microsoft.NET.Sdk.Web">
        <PropertyGroup>
        <TargetFramework>netcoreapp1.1</TargetFramework>
        </PropertyGroup>
        <ItemGroup>
        <Folder Include="wwwroot\" />
        </ItemGroup>
        <ItemGroup>
        <PackageReference Include="Microsoft.AspNetCore" Version="1.1.2" />
        </ItemGroup>
  </Project>
```

<span data-ttu-id="298cc-119">Mivel ez az alkalmazás egy webes alkalmazás, egy hivatkozás tooan ASP.NET Core csomag volt automatikusan hozzáadott toohello *hellodotnetcore.csproj* fájlt.</span><span class="sxs-lookup"><span data-stu-id="298cc-119">Since this app is a web application, a reference tooan ASP.NET Core package was automatically added toohello *hellodotnetcore.csproj* file.</span></span> <span data-ttu-id="298cc-120">hello csomag verziószáma hello toohello keretrendszer választása szerint van beállítva.</span><span class="sxs-lookup"><span data-stu-id="298cc-120">hello version number of hello package is set according toohello chosen framework.</span></span> <span data-ttu-id="298cc-121">Ebben a példában az ASP.NET Core 1.1.2 verzió hivatkozik, mert .NET Core 1.1 használatos.</span><span class="sxs-lookup"><span data-stu-id="298cc-121">This example is referencing ASP.NET Core version 1.1.2 because .NET Core 1.1 is used.</span></span>

## <a name="build-and-test-hello-application-locally"></a><span data-ttu-id="298cc-122">És hello alkalmazás helyi teszteléséhez</span><span class="sxs-lookup"><span data-stu-id="298cc-122">Build and test hello application locally</span></span> ##

<span data-ttu-id="298cc-123">Létrehozhatja és a .NET Core-alkalmazás futtatása az hello `dotnet restore` parancsot, majd hello `dotnet run` parancsban, ahogy az itt látható:</span><span class="sxs-lookup"><span data-stu-id="298cc-123">You can build and run your .NET Core app with hello `dotnet restore` command followed by hello `dotnet run` command, as shown here:</span></span>

```
dotnet restore
dotnet run
```


<span data-ttu-id="298cc-124">Hello alkalmazás indulásakor hello alkalmazás figyeli-e a port tooincoming kérelmek utaló üzenetet jelenít meg.</span><span class="sxs-lookup"><span data-stu-id="298cc-124">When hello application starts, it displays a message indicating hello app is listening tooincoming requests at a port.</span></span> 

```bash
Hosting environment: Production
Content root path: C:\hellodotnetcore
Now listening on: http://localhost:5000
Application started. Press Ctrl+C tooshut down.
```

<span data-ttu-id="298cc-125">Tallózással túl kipróbálja`http://localhost:5000/` a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="298cc-125">Test it by browsing too`http://localhost:5000/` with your browser.</span></span> <span data-ttu-id="298cc-126">Ha minden remekül működik, megjelenik a "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="298cc-126">If everything works fine, you see "Hello World!"</span></span> <span data-ttu-id="298cc-127">hello eredményt szövegként.</span><span class="sxs-lookup"><span data-stu-id="298cc-127">as hello result text.</span></span>

![A böngésző tesztelése][7]

## <a name="create-a-net-core-app-in-hello-azure-portal"></a><span data-ttu-id="298cc-129">A .NET Core-alkalmazás létrehozása az Azure Portal hello</span><span class="sxs-lookup"><span data-stu-id="298cc-129">Create a .NET Core app in hello Azure Portal</span></span> ##

<span data-ttu-id="298cc-130">Először egy üres webalkalmazás toocreate van szüksége.</span><span class="sxs-lookup"><span data-stu-id="298cc-130">First you need toocreate an empty web app.</span></span> <span data-ttu-id="298cc-131">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/) és hozzon létre egy új [Linux webalkalmazás](https://portal.azure.com/#create/Microsoft.AppSvcLinux).</span><span class="sxs-lookup"><span data-stu-id="298cc-131">Log in toohello [Azure portal](https://portal.azure.com/) and create a new [Web App on Linux](https://portal.azure.com/#create/Microsoft.AppSvcLinux).</span></span>

![A webes alkalmazás létrehozása][1]

<span data-ttu-id="298cc-133">Ha hello **létrehozása** lap megnyitásakor, a webalkalmazás részleteit tartalmazzák:</span><span class="sxs-lookup"><span data-stu-id="298cc-133">When hello **Create** page opens, provide details about your web app:</span></span>

![A .NET Core runtime verem kiválasztása][2]

<span data-ttu-id="298cc-135">Használjon hello következő táblázatban egy útmutató toofill hello ki, **létrehozása** lapon, majd válassza ki **OK** és **létrehozása** toocreate hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="298cc-135">Use hello following table as a guide toofill out hello **Create** page, then select **OK** and **Create** toocreate hello app.</span></span>

| <span data-ttu-id="298cc-136">Beállítás</span><span class="sxs-lookup"><span data-stu-id="298cc-136">Setting</span></span>      | <span data-ttu-id="298cc-137">Ajánlott érték</span><span class="sxs-lookup"><span data-stu-id="298cc-137">Suggested value</span></span>  | <span data-ttu-id="298cc-138">Leírás</span><span class="sxs-lookup"><span data-stu-id="298cc-138">Description</span></span>                                        |
| ------------ | ---------------- | -------------------------------------------------- |
| <span data-ttu-id="298cc-139">Alkalmazás neve</span><span class="sxs-lookup"><span data-stu-id="298cc-139">App name</span></span> | <span data-ttu-id="298cc-140">hellodotnetcore</span><span class="sxs-lookup"><span data-stu-id="298cc-140">hellodotnetcore</span></span>  | <span data-ttu-id="298cc-141">az alkalmazás hello neve.</span><span class="sxs-lookup"><span data-stu-id="298cc-141">hello name of your app.</span></span> <span data-ttu-id="298cc-142">Ez a név nem egyedi.</span><span class="sxs-lookup"><span data-stu-id="298cc-142">This name must be unique.</span></span> |
| <span data-ttu-id="298cc-143">Előfizetés</span><span class="sxs-lookup"><span data-stu-id="298cc-143">Subscription</span></span> | <span data-ttu-id="298cc-144">Válasszon egy meglévő előfizetéshez</span><span class="sxs-lookup"><span data-stu-id="298cc-144">Choose an existing subscription</span></span> | <span data-ttu-id="298cc-145">hello Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="298cc-145">hello Azure subscription.</span></span> |
| <span data-ttu-id="298cc-146">Erőforráscsoport</span><span class="sxs-lookup"><span data-stu-id="298cc-146">Resource Group</span></span> | <span data-ttu-id="298cc-147">myResourceGroup</span><span class="sxs-lookup"><span data-stu-id="298cc-147">myResourceGroup</span></span> |  <span data-ttu-id="298cc-148">hello Azure-erőforrás csoport toocontain hello webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="298cc-148">hello Azure resource group toocontain hello web app.</span></span> |
| <span data-ttu-id="298cc-149">App Service-csomag</span><span class="sxs-lookup"><span data-stu-id="298cc-149">App Service Plan</span></span> | <span data-ttu-id="298cc-150">Meglévő App Service-csomag neve</span><span class="sxs-lookup"><span data-stu-id="298cc-150">Existing App Service Plan name</span></span> |  <span data-ttu-id="298cc-151">hello App Service-csomag.</span><span class="sxs-lookup"><span data-stu-id="298cc-151">hello App Service plan.</span></span>  |
| <span data-ttu-id="298cc-152">Tároló konfigurálása</span><span class="sxs-lookup"><span data-stu-id="298cc-152">Configure Container</span></span> | <span data-ttu-id="298cc-153">A .NET core 1.1</span><span class="sxs-lookup"><span data-stu-id="298cc-153">.NET Core 1.1</span></span> | <span data-ttu-id="298cc-154">a webalkalmazás tároló típusú hello: beépített, Docker vagy titkos beállításjegyzék.</span><span class="sxs-lookup"><span data-stu-id="298cc-154">hello type of container for this web app: Built-in, Docker, or Private registry.</span></span> |
| <span data-ttu-id="298cc-155">Képek forrása</span><span class="sxs-lookup"><span data-stu-id="298cc-155">Image source</span></span>  | <span data-ttu-id="298cc-156">Beépített</span><span class="sxs-lookup"><span data-stu-id="298cc-156">Built-in</span></span>  |  <span data-ttu-id="298cc-157">hello kép hello forrását.</span><span class="sxs-lookup"><span data-stu-id="298cc-157">hello source of hello image.</span></span> |
| <span data-ttu-id="298cc-158">Futásidejű verem</span><span class="sxs-lookup"><span data-stu-id="298cc-158">Runtime Stack</span></span>  | <span data-ttu-id="298cc-159">A .NET core 1.1</span><span class="sxs-lookup"><span data-stu-id="298cc-159">.NET Core 1.1</span></span>  | <span data-ttu-id="298cc-160">hello futásidejű verem és verziót.</span><span class="sxs-lookup"><span data-stu-id="298cc-160">hello runtime stack and version.</span></span>  |

## <a name="deploy-your-application-via-git"></a><span data-ttu-id="298cc-161">A Git segítségével alkalmazás központi telepítése</span><span class="sxs-lookup"><span data-stu-id="298cc-161">Deploy your application via Git</span></span> ##

<span data-ttu-id="298cc-162">Git toodeploy hello .NET Core alkalmazás tooAzure App Service Web App használata Linux rendszeren.</span><span class="sxs-lookup"><span data-stu-id="298cc-162">Use Git toodeploy hello .NET Core application tooAzure App Service Web App on Linux.</span></span>

<span data-ttu-id="298cc-163">hello új Azure-webalkalmazás már konfigurált Git-telepítés.</span><span class="sxs-lookup"><span data-stu-id="298cc-163">hello new Azure web app already has Git deployment configured.</span></span> <span data-ttu-id="298cc-164">Navigáljon a következő URL-címet a webalkalmazás nevének behelyezése után toohello található hello Git telepítési URL-cím:</span><span class="sxs-lookup"><span data-stu-id="298cc-164">You will find hello Git deployment URL by navigating toohello following URL after inserting your web app name:</span></span>

```https://{your web app name}.scm.azurewebsites.net/api/scm/info```

<span data-ttu-id="298cc-165">hello Git URL-cím van a webes alkalmazás neve alapján a következő hello:</span><span class="sxs-lookup"><span data-stu-id="298cc-165">hello Git URL has hello following form based on your web app name:</span></span>

```https://{your web app name}.scm.azurewebsites.net/{your web app name}.git```

<span data-ttu-id="298cc-166">Futtassa a következő parancsok toodeploy hello helyi alkalmazás tooyour Azure-webalkalmazásban hello:</span><span class="sxs-lookup"><span data-stu-id="298cc-166">Run hello following commands toodeploy hello local application tooyour Azure web app:</span></span> 
 
```bash
git init
git remote add azure <Git deployment URL from above>
git add *.csproj *.cs
git commit -m "Initial deployment commit"
git push azure master
```

<span data-ttu-id="298cc-167">Nem kell toopush található összes fájl *bin /* vagy *obj /* könyvtárak, mert az alkalmazás hello felhő van építve amikor hello alkalmazás forrásfájljait tároló tooAzure elküldte azokat.</span><span class="sxs-lookup"><span data-stu-id="298cc-167">You don't need toopush any files under *bin/* or *obj/* directories because your application is built in hello cloud when hello application's source files are pushed tooAzure.</span></span> <span data-ttu-id="298cc-168">Hello felépítési folyamat befejezése után bináris fájlokat másol hello alkalmazás könyvtár: */otthoni/hely/wwwroot/*.</span><span class="sxs-lookup"><span data-stu-id="298cc-168">After hello build process is complete, binary files are copied into hello application's directory at */home/site/wwwroot/*.</span></span>

<span data-ttu-id="298cc-169">Győződjön meg arról, hogy a hello távoli üzembe helyezési műveleteket sikeres jelentést.</span><span class="sxs-lookup"><span data-stu-id="298cc-169">Confirm that hello remote deployment operations report success.</span></span> <span data-ttu-id="298cc-170">Leküldéses műveletek előfordulhat, hogy óta csomag feloldási időigényes és hello felhőben futtatható folyamat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="298cc-170">Push operations may take a while since package resolution and build process run in hello cloud.</span></span> <span data-ttu-id="298cc-171">Több állapotüzenetek, beleértve azokat, amely meghatározza, hogy a fájl másolása jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="298cc-171">You will see several status messages, including ones stating that files have been copied.</span></span> <span data-ttu-id="298cc-172">hello kimeneti alábbihoz hasonló toohello következő:</span><span class="sxs-lookup"><span data-stu-id="298cc-172">hello output should look similar toohello following:</span></span>

```bash
/* some output has been removed for brevity */
remote: Copying file: 'System.Net.Websockets.dll' 
remote: Copying file: 'System.Runtime.CompilerServices.Unsafe.dll' 
remote: Copying file: 'System.Runtime.Serialization.Primitives.dll' 
remote: Copying file: 'System.Text.Encodings.Web.dll' 
remote: Copying file: 'hellodotnetcore.deps.json' 
remote: Copying file: 'hellodotnetcore.dll' 
remote: Omitting next output lines...
remote: Finished successfully.
remote: Running post deployment commands...
remote: Deployment successful.
toohttps://hellodotnetcore.scm.azurewebsites.net/
 * [new branch]           master -> master

```

<span data-ttu-id="298cc-173">Hello központi telepítés befejezése után indítsa újra a webalkalmazása hello telepítési tootake hatást.</span><span class="sxs-lookup"><span data-stu-id="298cc-173">Once hello deployment has completed, restart your web app for hello deployment tootake effect.</span></span> <span data-ttu-id="298cc-174">toodo, nyissa meg az Azure portál toohello, és keresse meg a toohello **áttekintése** a webes alkalmazás lapján.</span><span class="sxs-lookup"><span data-stu-id="298cc-174">toodo this, go toohello Azure portal and navigate toohello **Overview** page of your web app.</span></span> <span data-ttu-id="298cc-175">Jelölje be hello **indítsa újra a** hello lap gombjára.</span><span class="sxs-lookup"><span data-stu-id="298cc-175">Select hello **Restart** button in hello page.</span></span> <span data-ttu-id="298cc-176">Amikor megjelenik egy előugró ablak, válassza ki a **Igen** tooconfirm.</span><span class="sxs-lookup"><span data-stu-id="298cc-176">When a popup window shows up, select **Yes** tooconfirm.</span></span> <span data-ttu-id="298cc-177">A webalkalmazás, majd keresse meg, ahogy az itt látható:</span><span class="sxs-lookup"><span data-stu-id="298cc-177">You can then browse your web app, as shown here:</span></span>

![Keresse meg a .NET Core alkalmazás telepítve tooAzure Linux App Service][10]

[!INCLUDE [Clean-up section](../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a><span data-ttu-id="298cc-179">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="298cc-179">Next steps</span></span>
* [<span data-ttu-id="298cc-180">Az Azure App Service webalkalmazásba Linux – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="298cc-180">Azure App Service Web App on Linux FAQ</span></span>](./app-service-linux-faq.md)

[1]: ./media/app-service-linux-using-dotnetcore/top-level-create.png
[2]: ./media/app-service-linux-using-dotnetcore/dotnet-new-webapp.png
[7]: ./media/app-service-linux-using-dotnetcore/dotnet-browse-local.png
[10]: ./media/app-service-linux-using-dotnetcore/dotnet-browse-azure.png
