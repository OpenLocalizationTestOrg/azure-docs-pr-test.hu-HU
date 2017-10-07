---
title: "egy helyi Docker-tároló aaaDebugging alkalmazások |} Microsoft Docs"
description: "Megtudhatja, hogyan toomodify egy alkalmazást, amely futtatja a helyi Docker-tároló hello tároló szerkesztési és frissítési keresztül frissítse, és állítsa a hibakeresés töréspontok"
services: azure-container-service
documentationcenter: na
author: mlearned
manager: douge
editor: 
ms.assetid: 480e3062-aae7-48ef-9701-e4f9ea041382
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 07/22/2016
ms.author: mlearned
ms.openlocfilehash: ff64e62fbb93901a29b5496bd5e17d2c4ea5ca99
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="debugging-apps-in-a-local-docker-container"></a><span data-ttu-id="ff5ae-103">Alkalmazások hibakeresése a helyi Docker-tárolóban</span><span class="sxs-lookup"><span data-stu-id="ff5ae-103">Debugging apps in a local Docker container</span></span>
## <a name="overview"></a><span data-ttu-id="ff5ae-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="ff5ae-104">Overview</span></span>
<span data-ttu-id="ff5ae-105">Visual Studio eszközök hello Docker egy egységes módon toodevelop a biztosít, és az alkalmazást helyileg egy Linux Docker-tároló ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="ff5ae-105">hello Visual Studio Tools for Docker provides a consistent way toodevelop in and validate your application locally in a Linux Docker container.</span></span>
<span data-ttu-id="ff5ae-106">Toorestart hello tároló nincs minden egyes módosítani egy kódot.</span><span class="sxs-lookup"><span data-stu-id="ff5ae-106">You don't have toorestart hello container each time you make a code change.</span></span>
<span data-ttu-id="ff5ae-107">Ez a cikk bemutatja, hogyan toouse hello "Szerkesztése és a frissítés" funkció toostart az ASP.NET Core webalkalmazás egy helyi Docker-tároló, a szükséges módosításokat, és ezt követően frissítse hello böngésző toosee ezeket a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="ff5ae-107">This article illustrates how toouse hello "Edit and Refresh" feature toostart an ASP.NET Core Web app in a local Docker container, make any necessary changes, and then refresh hello browser toosee those changes.</span></span>
<span data-ttu-id="ff5ae-108">Ez a cikk azt is bemutatja, hogyan tooset töréspontokat a hibakereséshez.</span><span class="sxs-lookup"><span data-stu-id="ff5ae-108">This article also shows you how tooset breakpoints for debugging.</span></span>

> [!NOTE]
> <span data-ttu-id="ff5ae-109">Windows-tároló támogatása hamarosan egy későbbi kiadásban</span><span class="sxs-lookup"><span data-stu-id="ff5ae-109">Windows Container support will be coming in a future release</span></span>
>
>

## <a name="prerequisites"></a><span data-ttu-id="ff5ae-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="ff5ae-110">Prerequisites</span></span>
<span data-ttu-id="ff5ae-111">a következő eszközök hello telepítve kell lennie.</span><span class="sxs-lookup"><span data-stu-id="ff5ae-111">hello following tools must be installed.</span></span>

* [<span data-ttu-id="ff5ae-112">Visual Studio legújabb verziójának</span><span class="sxs-lookup"><span data-stu-id="ff5ae-112">Latest version of Visual Studio</span></span>](https://www.visualstudio.com/downloads/)
* [<span data-ttu-id="ff5ae-113">A Microsoft ASP.NET Core 1.0 SDK</span><span class="sxs-lookup"><span data-stu-id="ff5ae-113">Microsoft ASP.NET Core 1.0 SDK</span></span>](https://go.microsoft.com/fwlink/?LinkID=809122)

<span data-ttu-id="ff5ae-114">Docker toorun tárolók helyileg, szüksége lesz egy helyi docker-ügyfél.</span><span class="sxs-lookup"><span data-stu-id="ff5ae-114">toorun Docker containers locally, you'll need a local docker client.</span></span>
<span data-ttu-id="ff5ae-115">Hello használhatja [Docker eszközkészlet](https://www.docker.com/products/docker-toolbox), ami megköveteli, hogy a Hyper-V toobe le van tiltva, vagy használhatja [Docker a Windows](https://www.docker.com/get-docker), amely Hyper-V használ, és a Windows 10 szükséges.</span><span class="sxs-lookup"><span data-stu-id="ff5ae-115">You can use hello [Docker Toolbox](https://www.docker.com/products/docker-toolbox), which requires Hyper-V toobe disabled, or you can use [Docker for Windows](https://www.docker.com/get-docker), which uses Hyper-V, and requires Windows 10.</span></span>

<span data-ttu-id="ff5ae-116">Ha használja a Docker eszközkészlet, szüksége lesz túl[hello Docker-ügyfél konfigurálása](vs-azure-tools-docker-setup.md)</span><span class="sxs-lookup"><span data-stu-id="ff5ae-116">If using Docker Toolbox, you'll need too[configure hello Docker client](vs-azure-tools-docker-setup.md)</span></span>

## <a name="1-create-a-web-app"></a><span data-ttu-id="ff5ae-117">1. Webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="ff5ae-117">1. Create a web app</span></span>
[!INCLUDE [create-aspnet5-app](../includes/create-aspnet5-app.md)]

## <a name="2-add-docker-support"></a><span data-ttu-id="ff5ae-118">2. Adja hozzá a Docker-támogatás</span><span class="sxs-lookup"><span data-stu-id="ff5ae-118">2. Add Docker support</span></span>
[!INCLUDE [Add docker support](../includes/vs-azure-tools-docker-add-docker-support.md)]

## <a name="3-edit-your-code-and-refresh"></a><span data-ttu-id="ff5ae-119">3. A kód és a frissítési szerkesztése</span><span class="sxs-lookup"><span data-stu-id="ff5ae-119">3. Edit your code and refresh</span></span>
<span data-ttu-id="ff5ae-120">tooquickly többször módosításokat, indítsa el az alkalmazás olyan tárolóban, és továbbra is toomake módosításokat, mint az IIS Express a Megtekintés.</span><span class="sxs-lookup"><span data-stu-id="ff5ae-120">tooquickly iterate changes, you can start your application within a container, and continue toomake changes, viewing them as you would with IIS Express.</span></span>

1. <span data-ttu-id="ff5ae-121">Hello megoldás konfigurációs beállítása túl`Debug` nyomja le az ENTER  **&lt;CTRL + F5 >** toobuild a docker rendszerképet, futtassa helyileg.</span><span class="sxs-lookup"><span data-stu-id="ff5ae-121">Set hello Solution Configuration too`Debug` and press **&lt;CTRL + F5>** toobuild your docker image and run it locally.</span></span>

    <span data-ttu-id="ff5ae-122">Miután hello tároló kép készült, és futtatja a Docker-tároló, a Visual Studio elindít hello webalkalmazást az alapértelmezett böngészőben.</span><span class="sxs-lookup"><span data-stu-id="ff5ae-122">Once hello container image has been built and is running in a Docker container, Visual Studio will launch hello Web app in your default browser.</span></span>
    <span data-ttu-id="ff5ae-123">Ha hello Microsoft Edge böngészőt használ, vagy ellenkező esetben a hibák rendelkezik, tekintse meg a [hibaelhárítás](vs-azure-tools-docker-troubleshooting-docker-errors.md) szakasz.</span><span class="sxs-lookup"><span data-stu-id="ff5ae-123">If you are using hello Microsoft Edge browser or otherwise have errors, see [Troubleshooting](vs-azure-tools-docker-troubleshooting-docker-errors.md) section.</span></span>
2. <span data-ttu-id="ff5ae-124">Nyissa meg toohello oldalról, amely ahol programot fogjuk toomake szükséges módosításokat.</span><span class="sxs-lookup"><span data-stu-id="ff5ae-124">Go toohello About page, which is where we're going toomake our changes.</span></span>
3. <span data-ttu-id="ff5ae-125">Térjen vissza a tooVisual Studio, és nyissa meg a `Views\Home\About.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="ff5ae-125">Return tooVisual Studio and open `Views\Home\About.cshtml`.</span></span>
4. <span data-ttu-id="ff5ae-126">Adja hozzá a következő hello fájl végéhez HTML tartalom toohello hello és hello módosítások mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="ff5ae-126">Add hello following HTML content toohello end of hello file and save hello changes.</span></span>

    ```
    <h1>Hello from a Docker Container!</h1>
    ```
5. <span data-ttu-id="ff5ae-127">Hello kimeneti ablakában jelenik meg, amikor hello .NET build befejeződött, és ezek a sorok látja, váltson vissza tooyour böngészőt, és frissítse a hello oldalról.</span><span class="sxs-lookup"><span data-stu-id="ff5ae-127">Viewing hello output window, when hello .NET build is completed and you see these lines, switch back tooyour browser and refresh hello About page.</span></span>

   ```
   Now listening on: http://*:80
   Application started. Press Ctrl+C tooshut down
   ```
6. <span data-ttu-id="ff5ae-128">A módosítások léptek érvénybe!</span><span class="sxs-lookup"><span data-stu-id="ff5ae-128">Your changes have been applied!</span></span>

## <a name="4-debug-with-breakpoints"></a><span data-ttu-id="ff5ae-129">4. Töréspontokat a hibakereséshez</span><span class="sxs-lookup"><span data-stu-id="ff5ae-129">4. Debug with breakpoints</span></span>
<span data-ttu-id="ff5ae-130">Gyakran módosításokat kell további ellenőrzési funkciók a Visual Studio hibakeresési hello kihasználva.</span><span class="sxs-lookup"><span data-stu-id="ff5ae-130">Often, changes will need further inspection, leveraging hello debugging features of Visual Studio.</span></span>

1. <span data-ttu-id="ff5ae-131">Térjen vissza a tooVisual Studio, és nyissa meg a`Controllers\HomeController.cs`</span><span class="sxs-lookup"><span data-stu-id="ff5ae-131">Return tooVisual Studio and open `Controllers\HomeController.cs`</span></span>
2. <span data-ttu-id="ff5ae-132">Cserélje le a hello About() metódus hello tartalmát hello alábbira:</span><span class="sxs-lookup"><span data-stu-id="ff5ae-132">Replace hello contents of hello About() method with hello following:</span></span>

   ```
   string message = "Your application description page from within a Container";
   ViewData["Message"] = message;
   ````
3. <span data-ttu-id="ff5ae-133">A töréspont toohello hello a bal oldali set `string message`... sor.</span><span class="sxs-lookup"><span data-stu-id="ff5ae-133">Set a breakpoint toohello left of hello `string message`... line.</span></span>
4. <span data-ttu-id="ff5ae-134">Találati  **&lt;F5 >** toostart hibakeresést.</span><span class="sxs-lookup"><span data-stu-id="ff5ae-134">Hit **&lt;F5>** toostart debugging.</span></span>
5. <span data-ttu-id="ff5ae-135">Keresse meg a lap toohit kapcsolatos toohello a töréspont megjelenését.</span><span class="sxs-lookup"><span data-stu-id="ff5ae-135">Navigate toohello About page toohit your breakpoint.</span></span>
6. <span data-ttu-id="ff5ae-136">TooVisual Studio tooview hello töréspont váltson, és vizsgálja meg az üzenet hello értékét.</span><span class="sxs-lookup"><span data-stu-id="ff5ae-136">Switch tooVisual Studio tooview hello breakpoint, and inspect hello value of message.</span></span>

   ![][2]

## <a name="summary"></a><span data-ttu-id="ff5ae-137">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="ff5ae-137">Summary</span></span>
<span data-ttu-id="ff5ae-138">A [Docker Visual Studio 2015 eszközök](https://aka.ms/DockerToolsForVS), helyben, hello éles létrehozásáról a belül egy Docker-tároló fejlődő hello hatékonyságára kaphat.</span><span class="sxs-lookup"><span data-stu-id="ff5ae-138">With [Visual Studio 2015 Tools for Docker](https://aka.ms/DockerToolsForVS), you can get hello productivity of working locally, with hello production realism of developing within a Docker container.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="ff5ae-139">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="ff5ae-139">Troubleshooting</span></span>
[<span data-ttu-id="ff5ae-140">Visual Studio Docker fejlesztői hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="ff5ae-140">Troubleshooting Visual Studio Docker Development</span></span>](vs-azure-tools-docker-troubleshooting-docker-errors.md)

## <a name="more-about-docker-with-visual-studio-windows-and-azure"></a><span data-ttu-id="ff5ae-141">További információ a Visual Studio, a Windows és az Azure Docker</span><span class="sxs-lookup"><span data-stu-id="ff5ae-141">More about Docker with Visual Studio, Windows, and Azure</span></span>
* <span data-ttu-id="ff5ae-142">[Docker Tools for Visual Studio](http://aka.ms/dockertoolsforvs) -tároló a .NET Core kódot fejlesztése</span><span class="sxs-lookup"><span data-stu-id="ff5ae-142">[Docker Tools for Visual Studio](http://aka.ms/dockertoolsforvs) - Developing your .NET Core code in a container</span></span>
* <span data-ttu-id="ff5ae-143">[Docker Tools for Visual Studio Team Services](http://aka.ms/dockertoolsforvsts) - Build és a docker-tároló üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="ff5ae-143">[Docker Tools for Visual Studio Team Services](http://aka.ms/dockertoolsforvsts) - Build and Deploy docker containers</span></span>
* <span data-ttu-id="ff5ae-144">[Docker Tools for Visual Studio Code](http://aka.ms/dockertoolsforvscode) -nyelv szolgáltatások docker-fájlok, szerkesztéséhez hamarosan további e2e esetén</span><span class="sxs-lookup"><span data-stu-id="ff5ae-144">[Docker Tools for Visual Studio Code](http://aka.ms/dockertoolsforvscode) - Language services for editing docker files, with more e2e scenarios coming</span></span>
* <span data-ttu-id="ff5ae-145">[Információ a Windows tároló](http://aka.ms/containers)-Windows Server és a Nano Server információk</span><span class="sxs-lookup"><span data-stu-id="ff5ae-145">[Windows Container Information](http://aka.ms/containers)- Windows Server and Nano Server information</span></span>
* <span data-ttu-id="ff5ae-146">[Azure Tárolószolgáltatás](https://azure.microsoft.com/services/container-service/) - [Azure tároló szolgáltatás tartalom](http://aka.ms/AzureContainerService)</span><span class="sxs-lookup"><span data-stu-id="ff5ae-146">[Azure Container Service](https://azure.microsoft.com/services/container-service/) - [Azure Container Service Content](http://aka.ms/AzureContainerService)</span></span>
* <span data-ttu-id="ff5ae-147">További Docker használata című részben talál példákat [Docker használata](https://github.com/Microsoft/HealthClinic.biz/wiki/Working-with-Docker) a hello [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 Connect [bemutató](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/).</span><span class="sxs-lookup"><span data-stu-id="ff5ae-147">For more examples of working with Docker, see [Working with Docker](https://github.com/Microsoft/HealthClinic.biz/wiki/Working-with-Docker) from hello [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 Connect [demo](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/).</span></span> <span data-ttu-id="ff5ae-148">Tekintse meg a hello HealthClinic.biz bemutató további quickstarts [Azure fejlesztői eszközök Quickstarts](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).</span><span class="sxs-lookup"><span data-stu-id="ff5ae-148">For more quickstarts from hello HealthClinic.biz demo, see [Azure Developer Tools Quickstarts](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).</span></span>

## <a name="various-docker-tools"></a><span data-ttu-id="ff5ae-149">Különböző Docker-eszközök</span><span class="sxs-lookup"><span data-stu-id="ff5ae-149">Various Docker tools</span></span>
[<span data-ttu-id="ff5ae-150">Néhány nagy docker eszközök (Steve Lasker blog)</span><span class="sxs-lookup"><span data-stu-id="ff5ae-150">Some great docker tools (Steve Lasker's blog)</span></span>](https://blogs.msdn.microsoft.com/stevelasker/2016/03/25/some-great-docker-tools/)

## <a name="good-articles"></a><span data-ttu-id="ff5ae-151">Jó cikkek</span><span class="sxs-lookup"><span data-stu-id="ff5ae-151">Good articles</span></span>
[<span data-ttu-id="ff5ae-152">A NGINX bemutatása tooMicroservices</span><span class="sxs-lookup"><span data-stu-id="ff5ae-152">Introduction tooMicroservices from NGINX</span></span>](https://www.nginx.com/blog/introduction-to-microservices/)

## <a name="presentations"></a><span data-ttu-id="ff5ae-153">Bemutatók</span><span class="sxs-lookup"><span data-stu-id="ff5ae-153">Presentations</span></span>
* [<span data-ttu-id="ff5ae-154">Steve Lasker: A VS élő moszkvai 2016 - Docker e2e</span><span class="sxs-lookup"><span data-stu-id="ff5ae-154">Steve Lasker: VS Live Las Vegas 2016 - Docker e2e</span></span>](https://github.com/SteveLasker/Presentations/blob/master/VSLive2016/Vegas/)
* [<span data-ttu-id="ff5ae-155">Bevezetés tooASP.NET Core @ build 2016 – Ha Ön a bemutató</span><span class="sxs-lookup"><span data-stu-id="ff5ae-155">Introduction tooASP.NET Core @ build 2016 - Where You At Demo</span></span>](https://channel9.msdn.com/Events/Build/2016/B810)
* [<span data-ttu-id="ff5ae-156">.NET-alkalmazásfejlesztés tárolókban, a Channel 9</span><span class="sxs-lookup"><span data-stu-id="ff5ae-156">Developing .NET apps in containers, Channel 9</span></span>](https://blogs.msdn.microsoft.com/stevelasker/2016/02/19/developing-asp-net-apps-in-docker-containers/)

[2]: ./media/vs-azure-tools-docker-edit-and-refresh/breakpoint.png
