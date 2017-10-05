---
title: "Egy helyi Docker-tároló alkalmazások hibakeresés |} Microsoft Docs"
description: "Megtudhatja, hogyan módosíthat egy alkalmazást, amely futtatja a helyi Docker-tároló, a tároló keresztül szerkesztési és frissítési frissítse és adja meg a töréspontokat hibakeresés"
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
ms.openlocfilehash: fcd58736d8915a61683a416fb9bf3892ba7b7bd8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="debugging-apps-in-a-local-docker-container"></a><span data-ttu-id="003d1-103">Alkalmazások hibakeresése a helyi Docker-tárolóban</span><span class="sxs-lookup"><span data-stu-id="003d1-103">Debugging apps in a local Docker container</span></span>
## <a name="overview"></a><span data-ttu-id="003d1-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="003d1-104">Overview</span></span>
<span data-ttu-id="003d1-105">A Visual Studio Tools for Docker a fejlesztésük és az alkalmazást helyileg egy Linux Docker-tároló ellenőrzése egységes módon biztosít.</span><span class="sxs-lookup"><span data-stu-id="003d1-105">The Visual Studio Tools for Docker provides a consistent way to develop in and validate your application locally in a Linux Docker container.</span></span>
<span data-ttu-id="003d1-106">Ne kelljen újraindítja a tárolót minden egyes módosítani egy kódot.</span><span class="sxs-lookup"><span data-stu-id="003d1-106">You don't have to restart the container each time you make a code change.</span></span>
<span data-ttu-id="003d1-107">Ez a cikk bemutatja, hogyan a Szerkesztés és, a frissítés"szolgáltatás segítségével az ASP.NET Core webalkalmazás elindítása a helyi Docker-tároló, a szükséges módosításokat, és ezt követően frissítse a böngészőt, hogy ezeket a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="003d1-107">This article illustrates how to use the "Edit and Refresh" feature to start an ASP.NET Core Web app in a local Docker container, make any necessary changes, and then refresh the browser to see those changes.</span></span>
<span data-ttu-id="003d1-108">Ez a cikk is bemutatja, hogyan állítson be töréspontokat a hibakereséshez.</span><span class="sxs-lookup"><span data-stu-id="003d1-108">This article also shows you how to set breakpoints for debugging.</span></span>

> [!NOTE]
> <span data-ttu-id="003d1-109">Windows-tároló támogatása hamarosan egy későbbi kiadásban</span><span class="sxs-lookup"><span data-stu-id="003d1-109">Windows Container support will be coming in a future release</span></span>
>
>

## <a name="prerequisites"></a><span data-ttu-id="003d1-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="003d1-110">Prerequisites</span></span>
<span data-ttu-id="003d1-111">A következő eszközök telepítése.</span><span class="sxs-lookup"><span data-stu-id="003d1-111">The following tools must be installed.</span></span>

* [<span data-ttu-id="003d1-112">Visual Studio legújabb verziójának</span><span class="sxs-lookup"><span data-stu-id="003d1-112">Latest version of Visual Studio</span></span>](https://www.visualstudio.com/downloads/)
* [<span data-ttu-id="003d1-113">A Microsoft ASP.NET Core 1.0 SDK</span><span class="sxs-lookup"><span data-stu-id="003d1-113">Microsoft ASP.NET Core 1.0 SDK</span></span>](https://go.microsoft.com/fwlink/?LinkID=809122)

<span data-ttu-id="003d1-114">Docker-tároló futtatásához helyileg, szüksége lesz egy helyi docker-ügyfél.</span><span class="sxs-lookup"><span data-stu-id="003d1-114">To run Docker containers locally, you'll need a local docker client.</span></span>
<span data-ttu-id="003d1-115">Használhatja a [Docker eszközkészlet](https://www.docker.com/products/docker-toolbox), ehhez a Hyper-V letiltását, vagy használhatja [Docker for Windows](https://www.docker.com/get-docker), amely Hyper-V használ, és a Windows 10 szükséges.</span><span class="sxs-lookup"><span data-stu-id="003d1-115">You can use the [Docker Toolbox](https://www.docker.com/products/docker-toolbox), which requires Hyper-V to be disabled, or you can use [Docker for Windows](https://www.docker.com/get-docker), which uses Hyper-V, and requires Windows 10.</span></span>

<span data-ttu-id="003d1-116">Docker eszközkészlet használata esetén kell [a Docker-ügyfél konfigurálása](vs-azure-tools-docker-setup.md)</span><span class="sxs-lookup"><span data-stu-id="003d1-116">If using Docker Toolbox, you'll need to [configure the Docker client](vs-azure-tools-docker-setup.md)</span></span>

## <a name="1-create-a-web-app"></a><span data-ttu-id="003d1-117">1. Webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="003d1-117">1. Create a web app</span></span>
[!INCLUDE [create-aspnet5-app](../includes/create-aspnet5-app.md)]

## <a name="2-add-docker-support"></a><span data-ttu-id="003d1-118">2. Adja hozzá a Docker-támogatás</span><span class="sxs-lookup"><span data-stu-id="003d1-118">2. Add Docker support</span></span>
[!INCLUDE [Add docker support](../includes/vs-azure-tools-docker-add-docker-support.md)]

## <a name="3-edit-your-code-and-refresh"></a><span data-ttu-id="003d1-119">3. A kód és a frissítési szerkesztése</span><span class="sxs-lookup"><span data-stu-id="003d1-119">3. Edit your code and refresh</span></span>
<span data-ttu-id="003d1-120">Gyors ismétlésének módosításokat, indítsa el az alkalmazás olyan tárolóban, és folytatni a módosításokat, mint az IIS Express a Megtekintés.</span><span class="sxs-lookup"><span data-stu-id="003d1-120">To quickly iterate changes, you can start your application within a container, and continue to make changes, viewing them as you would with IIS Express.</span></span>

1. <span data-ttu-id="003d1-121">A megoldás konfigurációs beállítása `Debug` nyomja le az ENTER  **&lt;CTRL + F5 >** a docker lemezkép, és futtassa helyileg.</span><span class="sxs-lookup"><span data-stu-id="003d1-121">Set the Solution Configuration to `Debug` and press **&lt;CTRL + F5>** to build your docker image and run it locally.</span></span>

    <span data-ttu-id="003d1-122">Miután a tároló kép készült, és futtatja a Docker-tároló, a Visual Studio elindít az alapértelmezett böngészőben a webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="003d1-122">Once the container image has been built and is running in a Docker container, Visual Studio will launch the Web app in your default browser.</span></span>
    <span data-ttu-id="003d1-123">Ha a Microsoft Edge böngészőt használ, vagy ellenkező esetben a hibák rendelkezik, tekintse meg a [hibaelhárítás](vs-azure-tools-docker-troubleshooting-docker-errors.md) szakasz.</span><span class="sxs-lookup"><span data-stu-id="003d1-123">If you are using the Microsoft Edge browser or otherwise have errors, see [Troubleshooting](vs-azure-tools-docker-troubleshooting-docker-errors.md) section.</span></span>
2. <span data-ttu-id="003d1-124">Ugrás a jogi tudnivalók megjelenítése Névjegy lapot, amely ahol fogjuk a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="003d1-124">Go to the About page, which is where we're going to make our changes.</span></span>
3. <span data-ttu-id="003d1-125">Térjen vissza a Visual Studio, és nyissa meg a `Views\Home\About.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="003d1-125">Return to Visual Studio and open `Views\Home\About.cshtml`.</span></span>
4. <span data-ttu-id="003d1-126">Adja hozzá az alábbi HTML-tartalmakat, a fájl végén, és mentse a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="003d1-126">Add the following HTML content to the end of the file and save the changes.</span></span>

    ```
    <h1>Hello from a Docker Container!</h1>
    ```
5. <span data-ttu-id="003d1-127">A kimeneti ablakban megtekintheti, ha a .NET-build befejeződött, és láthatja, hogy ezek a sorok, lépjen vissza a böngésző, és frissítse a jogi tudnivalók megjelenítése Névjegy lapot.</span><span class="sxs-lookup"><span data-stu-id="003d1-127">Viewing the output window, when the .NET build is completed and you see these lines, switch back to your browser and refresh the About page.</span></span>

   ```
   Now listening on: http://*:80
   Application started. Press Ctrl+C to shut down
   ```
6. <span data-ttu-id="003d1-128">A módosítások léptek érvénybe!</span><span class="sxs-lookup"><span data-stu-id="003d1-128">Your changes have been applied!</span></span>

## <a name="4-debug-with-breakpoints"></a><span data-ttu-id="003d1-129">4. Töréspontokat a hibakereséshez</span><span class="sxs-lookup"><span data-stu-id="003d1-129">4. Debug with breakpoints</span></span>
<span data-ttu-id="003d1-130">Gyakran módosításokat kell további ellenőrzést, a Visual Studio hibakeresési szolgáltatásokat kihasználva.</span><span class="sxs-lookup"><span data-stu-id="003d1-130">Often, changes will need further inspection, leveraging the debugging features of Visual Studio.</span></span>

1. <span data-ttu-id="003d1-131">Térjen vissza a Visual Studio, és nyissa meg a`Controllers\HomeController.cs`</span><span class="sxs-lookup"><span data-stu-id="003d1-131">Return to Visual Studio and open `Controllers\HomeController.cs`</span></span>
2. <span data-ttu-id="003d1-132">Cserélje ki annak tartalmát a About() metódus a következő:</span><span class="sxs-lookup"><span data-stu-id="003d1-132">Replace the contents of the About() method with the following:</span></span>

   ```
   string message = "Your application description page from within a Container";
   ViewData["Message"] = message;
   ````
3. <span data-ttu-id="003d1-133">Állítson be egy töréspontot bal oldalán a `string message`... sor.</span><span class="sxs-lookup"><span data-stu-id="003d1-133">Set a breakpoint to the left of the `string message`... line.</span></span>
4. <span data-ttu-id="003d1-134">Találati  **&lt;F5 >** a hibakeresés.</span><span class="sxs-lookup"><span data-stu-id="003d1-134">Hit **&lt;F5>** to start debugging.</span></span>
5. <span data-ttu-id="003d1-135">Nyissa meg a jogi tudnivalók megjelenítése Névjegy lapot érni a töréspont megjelenését.</span><span class="sxs-lookup"><span data-stu-id="003d1-135">Navigate to the About page to hit your breakpoint.</span></span>
6. <span data-ttu-id="003d1-136">Visual Studio a töréspont megtekintéséhez váltson, és vizsgálja meg az üzenet értékét.</span><span class="sxs-lookup"><span data-stu-id="003d1-136">Switch to Visual Studio to view the breakpoint, and inspect the value of message.</span></span>

   ![][2]

## <a name="summary"></a><span data-ttu-id="003d1-137">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="003d1-137">Summary</span></span>
<span data-ttu-id="003d1-138">A [Docker Visual Studio 2015 eszközök](https://aka.ms/DockerToolsForVS), helyileg, a termelési létrehozásáról a belül egy Docker-tároló fejlődő termelékenységére kaphat.</span><span class="sxs-lookup"><span data-stu-id="003d1-138">With [Visual Studio 2015 Tools for Docker](https://aka.ms/DockerToolsForVS), you can get the productivity of working locally, with the production realism of developing within a Docker container.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="003d1-139">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="003d1-139">Troubleshooting</span></span>
[<span data-ttu-id="003d1-140">Visual Studio Docker fejlesztői hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="003d1-140">Troubleshooting Visual Studio Docker Development</span></span>](vs-azure-tools-docker-troubleshooting-docker-errors.md)

## <a name="more-about-docker-with-visual-studio-windows-and-azure"></a><span data-ttu-id="003d1-141">További információ a Visual Studio, a Windows és az Azure Docker</span><span class="sxs-lookup"><span data-stu-id="003d1-141">More about Docker with Visual Studio, Windows, and Azure</span></span>
* <span data-ttu-id="003d1-142">[Docker Tools for Visual Studio](http://aka.ms/dockertoolsforvs) -tároló a .NET Core kódot fejlesztése</span><span class="sxs-lookup"><span data-stu-id="003d1-142">[Docker Tools for Visual Studio](http://aka.ms/dockertoolsforvs) - Developing your .NET Core code in a container</span></span>
* <span data-ttu-id="003d1-143">[Docker Tools for Visual Studio Team Services](http://aka.ms/dockertoolsforvsts) - Build és a docker-tároló üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="003d1-143">[Docker Tools for Visual Studio Team Services](http://aka.ms/dockertoolsforvsts) - Build and Deploy docker containers</span></span>
* <span data-ttu-id="003d1-144">[Docker Tools for Visual Studio Code](http://aka.ms/dockertoolsforvscode) -nyelv szolgáltatások docker-fájlok, szerkesztéséhez hamarosan további e2e esetén</span><span class="sxs-lookup"><span data-stu-id="003d1-144">[Docker Tools for Visual Studio Code](http://aka.ms/dockertoolsforvscode) - Language services for editing docker files, with more e2e scenarios coming</span></span>
* <span data-ttu-id="003d1-145">[Információ a Windows tároló](http://aka.ms/containers)-Windows Server és a Nano Server információk</span><span class="sxs-lookup"><span data-stu-id="003d1-145">[Windows Container Information](http://aka.ms/containers)- Windows Server and Nano Server information</span></span>
* <span data-ttu-id="003d1-146">[Azure Tárolószolgáltatás](https://azure.microsoft.com/services/container-service/) - [Azure tároló szolgáltatás tartalom](http://aka.ms/AzureContainerService)</span><span class="sxs-lookup"><span data-stu-id="003d1-146">[Azure Container Service](https://azure.microsoft.com/services/container-service/) - [Azure Container Service Content](http://aka.ms/AzureContainerService)</span></span>
* <span data-ttu-id="003d1-147">További Docker használata című részben talál példákat [Docker használata](https://github.com/Microsoft/HealthClinic.biz/wiki/Working-with-Docker) a a [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 Connect [bemutató](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/).</span><span class="sxs-lookup"><span data-stu-id="003d1-147">For more examples of working with Docker, see [Working with Docker](https://github.com/Microsoft/HealthClinic.biz/wiki/Working-with-Docker) from the [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 Connect [demo](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/).</span></span> <span data-ttu-id="003d1-148">A HealthClinic.biz bemutató további gyors útmutatóit lásd: [Azure Developer Tools Quickstarts](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts) (Azure fejlesztői eszközök – gyors útmutatók).</span><span class="sxs-lookup"><span data-stu-id="003d1-148">For more quickstarts from the HealthClinic.biz demo, see [Azure Developer Tools Quickstarts](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).</span></span>

## <a name="various-docker-tools"></a><span data-ttu-id="003d1-149">Különböző Docker-eszközök</span><span class="sxs-lookup"><span data-stu-id="003d1-149">Various Docker tools</span></span>
[<span data-ttu-id="003d1-150">Néhány nagy docker eszközök (Steve Lasker blog)</span><span class="sxs-lookup"><span data-stu-id="003d1-150">Some great docker tools (Steve Lasker's blog)</span></span>](https://blogs.msdn.microsoft.com/stevelasker/2016/03/25/some-great-docker-tools/)

## <a name="good-articles"></a><span data-ttu-id="003d1-151">Jó cikkek</span><span class="sxs-lookup"><span data-stu-id="003d1-151">Good articles</span></span>
[<span data-ttu-id="003d1-152">A NGINX Mikroszolgáltatások bemutatása</span><span class="sxs-lookup"><span data-stu-id="003d1-152">Introduction to Microservices from NGINX</span></span>](https://www.nginx.com/blog/introduction-to-microservices/)

## <a name="presentations"></a><span data-ttu-id="003d1-153">Bemutatók</span><span class="sxs-lookup"><span data-stu-id="003d1-153">Presentations</span></span>
* [<span data-ttu-id="003d1-154">Steve Lasker: A VS élő moszkvai 2016 - Docker e2e</span><span class="sxs-lookup"><span data-stu-id="003d1-154">Steve Lasker: VS Live Las Vegas 2016 - Docker e2e</span></span>](https://github.com/SteveLasker/Presentations/blob/master/VSLive2016/Vegas/)
* [<span data-ttu-id="003d1-155">Bevezetés az ASP.NET Core @ build 2016 – Ha Ön a bemutató</span><span class="sxs-lookup"><span data-stu-id="003d1-155">Introduction to ASP.NET Core @ build 2016 - Where You At Demo</span></span>](https://channel9.msdn.com/Events/Build/2016/B810)
* [<span data-ttu-id="003d1-156">.NET-alkalmazásfejlesztés tárolókban, a Channel 9</span><span class="sxs-lookup"><span data-stu-id="003d1-156">Developing .NET apps in containers, Channel 9</span></span>](https://blogs.msdn.microsoft.com/stevelasker/2016/02/19/developing-asp-net-apps-in-docker-containers/)

[2]: ./media/vs-azure-tools-docker-edit-and-refresh/breakpoint.png
