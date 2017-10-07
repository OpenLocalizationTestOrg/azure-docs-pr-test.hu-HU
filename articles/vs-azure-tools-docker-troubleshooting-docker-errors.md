---
title: "aaaTroubleshooting Docker ügyfél hibákat a Windows Visual Studio használatával |} Microsoft Docs"
description: "Visual Studio toocreate használata során felmerülő problémák megoldásához, és központi telepítése a web apps tooDocker Windows rendszeren Visual Studio használatával."
services: azure-container-service
documentationcenter: na
author: mlearned
manager: douge
editor: 
ms.assetid: 346f70b9-7b52-4688-a8e8-8f53869618d3
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 06/08/2016
ms.author: mlearned
ms.openlocfilehash: 7421ae8e044d58fc412d748fb870da4c9b2fdb3b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-visual-studio-docker-development"></a><span data-ttu-id="2f0e4-103">A Visual Studio Docker fejlesztési hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="2f0e4-103">Troubleshoot Visual Studio Docker development</span></span>

<span data-ttu-id="2f0e4-104">Ha a Visual Studio eszközök a Docker Preview dolgozik, néhány problémákat tapasztalhat a hello preview hello jellege miatt.</span><span class="sxs-lookup"><span data-stu-id="2f0e4-104">When you're working with Visual Studio Tools for Docker Preview, you may encounter some problems because of hello nature of hello preview.</span></span>
<span data-ttu-id="2f0e4-105">Az alábbiakban néhány gyakori problémák és megoldások.</span><span class="sxs-lookup"><span data-stu-id="2f0e4-105">Following are some common issues and resolutions.</span></span>  

## <a name="visual-studio-2017-rc"></a><span data-ttu-id="2f0e4-106">A Visual Studio 2017 RC</span><span class="sxs-lookup"><span data-stu-id="2f0e4-106">Visual Studio 2017 RC</span></span>

### <a name="linux-containers"></a><span data-ttu-id="2f0e4-107">**Linux-tárolók**</span><span class="sxs-lookup"><span data-stu-id="2f0e4-107">**Linux containers**</span></span>

####  <a name="build-errors-occur-when-debugging-a-net-core-web-or-console-application"></a><span data-ttu-id="2f0e4-108">Build hiba történik, amikor a .NET Core web vagy konzol alkalmazásban</span><span class="sxs-lookup"><span data-stu-id="2f0e4-108">Build errors occur when debugging a .NET Core web or console application</span></span>  

<span data-ttu-id="2f0e4-109">Ennek oka lehet a Docker a Windows hello projektet tartalmazó hello meghajtó megosztása kapcsolódó toonot.</span><span class="sxs-lookup"><span data-stu-id="2f0e4-109">This could be related toonot sharing hello drive where hello project resides with Docker for Windows.</span></span>  <span data-ttu-id="2f0e4-110">Hasonló hello hiba jelenhet meg:</span><span class="sxs-lookup"><span data-stu-id="2f0e4-110">You may receive an error like hello following:</span></span>

```
hello "PrepareForLaunch" task failed unexpectedly.
Microsoft.DotNet.Docker.CommandLineClientException: Creating network "webapplication13628050196_default" with hello default driver
Building webapplication1
Creating webapplication13628050196_webapplication1_1
ERROR: for webapplication1  Cannot create container for service webapplication1: C: drive is not shared. Please share it in Docker for Windows Settings
```
<span data-ttu-id="2f0e4-111">tooresolve probléma:</span><span class="sxs-lookup"><span data-stu-id="2f0e4-111">tooresolve this issue:</span></span>

1. <span data-ttu-id="2f0e4-112">Kattintson a jobb gombbal **Docker for Windows** a hello értesítési területén, majd válassza ki **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="2f0e4-112">Right-click **Docker for Windows** in hello notification area, and then select **Settings**.</span></span>  
2. <span data-ttu-id="2f0e4-113">Válassza ki **megosztott meghajtók** és hello meghajtó, ahol hello projekt található.</span><span class="sxs-lookup"><span data-stu-id="2f0e4-113">Select **Shared Drives** and share hello drive where hello project resides.</span></span>

### <a name="windows-containers"></a><span data-ttu-id="2f0e4-114">**Windows-tárolók**</span><span class="sxs-lookup"><span data-stu-id="2f0e4-114">**Windows containers**</span></span>

<span data-ttu-id="2f0e4-115">hello következő problémák adott toodebugging .NET-keretrendszer webes és a konzol Windows-tárolókban lévő alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="2f0e4-115">hello following issues are specific toodebugging .NET Framework web and console applications in Windows containers.</span></span>

#### <a name="prerequisites"></a><span data-ttu-id="2f0e4-116">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="2f0e4-116">Prerequisites</span></span>

1. <span data-ttu-id="2f0e4-117">A Visual Studio 2017 RC (vagy újabb) a .NET Core hello és Docker Preview munkaterhelést kell telepíteni.</span><span class="sxs-lookup"><span data-stu-id="2f0e4-117">Visual Studio 2017 RC (or later) with hello .NET Core and Docker Preview workload must be installed.</span></span>
2. <span data-ttu-id="2f0e4-118">Windows 10 évforduló frissítése a legújabb Windows frissítéshez javításokkal.</span><span class="sxs-lookup"><span data-stu-id="2f0e4-118">Windows 10 Anniversary Update with that latest Windows Update patches.</span></span> <span data-ttu-id="2f0e4-119">Kifejezetten [KB3194798](https://support.microsoft.com/en-us/help/3194798/cumulative-update-for-windows-10-version-1607-and-windows-server-2016-october-11,-2016) kell telepíteni.</span><span class="sxs-lookup"><span data-stu-id="2f0e4-119">Specifically [KB3194798](https://support.microsoft.com/en-us/help/3194798/cumulative-update-for-windows-10-version-1607-and-windows-server-2016-october-11,-2016) must be installed.</span></span> 
3. <span data-ttu-id="2f0e4-120">[A Windows docker](https://docs.docker.com/docker-for-windows/) (build 1.13.0 vagy újabb) kell telepíteni.</span><span class="sxs-lookup"><span data-stu-id="2f0e4-120">[Docker for Windows](https://docs.docker.com/docker-for-windows/) (build 1.13.0 or later) must be installed.</span></span>
4. <span data-ttu-id="2f0e4-121">**Váltás tooWindows tárolók** ki kell jelölni.</span><span class="sxs-lookup"><span data-stu-id="2f0e4-121">**Switch tooWindows containers** must be selected.</span></span> <span data-ttu-id="2f0e4-122">Hello értesítési területen kattintson a **Docker for Windows**, majd válassza ki **tooWindows tárolók kapcsoló**.</span><span class="sxs-lookup"><span data-stu-id="2f0e4-122">In hello notification area, click **Docker for Windows**, and then select **Switch tooWindows containers**.</span></span> <span data-ttu-id="2f0e4-123">Hello számítógép újraindítását követően győződjön meg arról, hogy ez a beállítás megőrzi.</span><span class="sxs-lookup"><span data-stu-id="2f0e4-123">After hello machine restarts, ensure that this setting is retained.</span></span>

#### <a name="console-output-does-not-appear-in-visual-studios-output-window-while-debugging-a-console-application"></a><span data-ttu-id="2f0e4-124">Konzol kimeneti nem jelenik meg a Visual Studio kimeneti ablakában a Konzolalkalmazás-hibakeresés során</span><span class="sxs-lookup"><span data-stu-id="2f0e4-124">Console output does not appear in Visual Studio's output window while debugging a console application</span></span>

<span data-ttu-id="2f0e4-125">Ez az egy ismert probléma az hello Visual Studio hibakereső (msvsmon.exe), amely jelenleg nem az ebben a forgatókönyvben.</span><span class="sxs-lookup"><span data-stu-id="2f0e4-125">This is a known issue with hello Visual Studio debugger (msvsmon.exe), which is currently not designed for this scenario.</span></span> <span data-ttu-id="2f0e4-126">Ez a forgatókönyv támogatása egy későbbi kiadásban szereplő.</span><span class="sxs-lookup"><span data-stu-id="2f0e4-126">Support for this scenario might be included in a future release.</span></span> <span data-ttu-id="2f0e4-127">toosee kimenetét hello konzolalkalmazást a Visual Studio, használja a **Docker: Start projekt**, egyenértékű túl**indítása nélkül hibakeresés**.</span><span class="sxs-lookup"><span data-stu-id="2f0e4-127">toosee output from hello console application in Visual Studio, use **Docker: Start Project**, which is equivalent too**Start without Debugging**.</span></span>

#### <a name="debugging-web-applications-with-hello-release-configuration-fails-with-403-forbidden-error"></a><span data-ttu-id="2f0e4-128">Hello hibakeresési alkalmazások kiadási (403-as) tiltja hiba konfigurációs meghiúsul</span><span class="sxs-lookup"><span data-stu-id="2f0e4-128">Debugging web applications with hello release configuration fails with (403) Forbidden error</span></span>

<span data-ttu-id="2f0e4-129">toowork a probléma megoldásához nyissa meg a web.release.config hello megoldásban és megjegyzéssé, vagy törölje az alábbi hello:</span><span class="sxs-lookup"><span data-stu-id="2f0e4-129">toowork around this issue, open web.release.config in hello solution and comment out or delete hello following lines:</span></span>

```
<compilation xdt:Transform="RemoveAttributes(debug)" />
```

## <a name="visual-studio-2015"></a><span data-ttu-id="2f0e4-130">Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="2f0e4-130">Visual Studio 2015</span></span>

### <a name="linux-containers"></a><span data-ttu-id="2f0e4-131">**Linux-tárolók**</span><span class="sxs-lookup"><span data-stu-id="2f0e4-131">**Linux containers**</span></span>

#### <a name="unable-toovalidate-volume-mapping"></a><span data-ttu-id="2f0e4-132">Nem lehet toovalidate kötet leképezése</span><span class="sxs-lookup"><span data-stu-id="2f0e4-132">Unable toovalidate volume mapping</span></span>
<span data-ttu-id="2f0e4-133">Kötet lesz szükség tooshare hello forráskódját és a hello tároló hello app mappa az alkalmazás a bináris fájljait.</span><span class="sxs-lookup"><span data-stu-id="2f0e4-133">Volume mapping is required tooshare hello source code and binaries of your application with hello app folder in hello container.</span></span>  <span data-ttu-id="2f0e4-134">Adott kötet hozzárendelések docker-compose.dev.debug.yml és a docker-compose.dev.release.yml belül találhatók.</span><span class="sxs-lookup"><span data-stu-id="2f0e4-134">Specific volume mappings are contained within docker-compose.dev.debug.yml and docker-compose.dev.release.yml.</span></span> <span data-ttu-id="2f0e4-135">Fájlok módosulnak, a gazdagépen, mert a hello tárolók hasonló gyökérmappa-szerkezetében lévő változtatásoknak megfelelően.</span><span class="sxs-lookup"><span data-stu-id="2f0e4-135">As files are changed on your host machine, hello containers reflect these changes in a similar folder structure.</span></span>

<span data-ttu-id="2f0e4-136">tooenable kötet hozzárendelése:</span><span class="sxs-lookup"><span data-stu-id="2f0e4-136">tooenable volume mapping:</span></span>

1. <span data-ttu-id="2f0e4-137">Kattintson a **Moby** hello értesítési területről, és válassza ki a **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="2f0e4-137">Click **Moby** in hello notification area and select **Settings**.</span></span>
2. <span data-ttu-id="2f0e4-138">Válassza ki **megosztott meghajtók**.</span><span class="sxs-lookup"><span data-stu-id="2f0e4-138">Select **Shared Drives**.</span></span>
3. <span data-ttu-id="2f0e4-139">Válassza ki a projekt és hello meghajtó, ahol a % USERPROFILE % található hello meghajtón.</span><span class="sxs-lookup"><span data-stu-id="2f0e4-139">Select hello drive that hosts your project and hello drive where %USERPROFILE% resides.</span></span>
4. <span data-ttu-id="2f0e4-140">Kattintson az **Alkalmaz** gombra.</span><span class="sxs-lookup"><span data-stu-id="2f0e4-140">Click **Apply**.</span></span>

<span data-ttu-id="2f0e4-141">tootest kötet leképezési működik, ha építse újra, és válassza ki az F5 billentyűt a Visual Studio, egy vagy több meghajtó van megosztva, vagy után futtassa a következő kód egy parancs parancssori futtatásával hello.</span><span class="sxs-lookup"><span data-stu-id="2f0e4-141">tootest if volume mapping is functioning, Rebuild and select F5 from within Visual Studio after one or more drives have been shared, or run hello following code from a command prompt.</span></span>

> [!NOTE]
> <span data-ttu-id="2f0e4-142">Ez a példa feltételezi, hogy a felhasználók mappában található a C meghajtó, és hogy azt meg lett osztva.</span><span class="sxs-lookup"><span data-stu-id="2f0e4-142">This example assumes that your Users folder is located on drive C and that it has been shared.</span></span>
> <span data-ttu-id="2f0e4-143">Szükség esetén, ha egy másik meghajtót megosztott módosításához.</span><span class="sxs-lookup"><span data-stu-id="2f0e4-143">Revise as necessary if you have shared a different drive.</span></span>

```
docker run -it -v /c/Users/Public:/wormhole busybox
```

<span data-ttu-id="2f0e4-144">Futtassa a következő kód hello Linux tárolóban hello.</span><span class="sxs-lookup"><span data-stu-id="2f0e4-144">Run hello following code in hello Linux container.</span></span>

```
/ # ls
```

<span data-ttu-id="2f0e4-145">Meg kell jelennie egy könyvtárlistát hello felhasználók vagy nyilvános mappából.</span><span class="sxs-lookup"><span data-stu-id="2f0e4-145">You should see a directory listing from hello Users/Public folder.</span></span> <span data-ttu-id="2f0e4-146">Ha nem jelenik meg, és a /c/Users/Public mappa nem üres, kötet leképezési nincs megfelelően konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="2f0e4-146">If no files are displayed and your /c/Users/Public folder isn't empty, volume mapping is not configured properly.</span></span>

```
bin       etc       proc      sys       usr       wormhole
dev       home      root      tmp       var
```

<span data-ttu-id="2f0e4-147">Módosítsa toohello féreglyuk directory toosee hello hello `/c/Users/Public` könyvtár:</span><span class="sxs-lookup"><span data-stu-id="2f0e4-147">Change toohello wormhole directory toosee hello contents of hello `/c/Users/Public` directory:</span></span>

```
/ # cd wormhole/
/wormhole # ls
AccountPictures  Downloads        Music            Videos
Desktop          Host             NuGet.Config     desktop.ini
Documents        Libraries        Pictures
/wormhole #
```

> [!NOTE]
> <span data-ttu-id="2f0e4-148">Linux virtuális gépek dolgozik, hello tároló fájlrendszer esetén kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="2f0e4-148">When you're working with Linux VMs, hello container file system is case-sensitive.</span></span>

## <a name="build-prepareforbuild-task-failed-unexpectedly"></a><span data-ttu-id="2f0e4-149">Build: "PrepareForBuild" feladat váratlanul leállt</span><span class="sxs-lookup"><span data-stu-id="2f0e4-149">Build: "PrepareForBuild" task failed unexpectedly</span></span>

<span data-ttu-id="2f0e4-150">Microsoft.DotNet.Docker.CommandLine.ClientException: Hiba történt, miközben a rendszer tooconnect.</span><span class="sxs-lookup"><span data-stu-id="2f0e4-150">Microsoft.DotNet.Docker.CommandLine.ClientException: An error occurred trying tooconnect.</span></span>

<span data-ttu-id="2f0e4-151">Ellenőrizze, hogy adott hello alapértelmezett Docker gazdagépen fut-e.</span><span class="sxs-lookup"><span data-stu-id="2f0e4-151">Verify that hello default Docker host is running.</span></span> <span data-ttu-id="2f0e4-152">Nyisson meg egy parancssort, és hajtsa végre:</span><span class="sxs-lookup"><span data-stu-id="2f0e4-152">Open a command prompt and execute:</span></span>

```
docker info
```

<span data-ttu-id="2f0e4-153">Ha ezt a hibát ad vissza, majd próbálja meg toostart hello **Docker for Windows** egy asztali alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="2f0e4-153">If this returns an error, then attempt toostart hello **Docker for Windows** desktop app.</span></span> <span data-ttu-id="2f0e4-154">Ha hello egy asztali alkalmazás fut, majd **Moby** hello értesítési területen látható kell lennie.</span><span class="sxs-lookup"><span data-stu-id="2f0e4-154">If hello desktop app is running, then **Moby** should be visible in hello notification area.</span></span> <span data-ttu-id="2f0e4-155">Kattintson a jobb gombbal **Moby** , és nyissa meg **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="2f0e4-155">Right-click **Moby** and open **Settings**.</span></span> <span data-ttu-id="2f0e4-156">Kattintson a **alaphelyzetbe**, majd indítsa újra a Docker.</span><span class="sxs-lookup"><span data-stu-id="2f0e4-156">Click **Reset**, and then restart Docker.</span></span>

## <a name="an-error-dialog-occurs-when-attempting-tooadd-docker-support-or-debug-f5-an-aspnet-core-application-in-a-container"></a><span data-ttu-id="2f0e4-157">Hiba-párbeszédpanel tooadd Docker támogatási tett kísérlet során következik be, vagy a hibakeresési (F5), az ASP.NET Core alkalmazás tároló</span><span class="sxs-lookup"><span data-stu-id="2f0e4-157">An error dialog occurs when attempting tooadd Docker Support or debug (F5) an ASP.NET Core application in a container</span></span>

<span data-ttu-id="2f0e4-158">Eltávolítása és bővítmények telepítése után a Visual Studio felügyelt bővítési keretrendszer (MEF) gyorsítótár hello is megsérülhet.</span><span class="sxs-lookup"><span data-stu-id="2f0e4-158">After uninstalling and installing extensions, hello Managed Extensibility Framework (MEF) cache in Visual Studio can become corrupted.</span></span> <span data-ttu-id="2f0e4-159">Ha ez történik, különböző hibaüzenetek okozhat, ha Ön most Docker-támogatás hozzáadása és/vagy toorun kísérlet, illetve az ASP.NET Core alkalmazás (F5) hibakeresését.</span><span class="sxs-lookup"><span data-stu-id="2f0e4-159">When this occurs, it can cause various error messages when you're adding Docker Support and/or attempting toorun or debug (F5) your ASP.NET Core application.</span></span> <span data-ttu-id="2f0e4-160">Ideiglenes megoldásként használja a következő lépéseket toodelete és újragenerálása hello MEF gyorsítótár hello.</span><span class="sxs-lookup"><span data-stu-id="2f0e4-160">As a temporary workaround, use hello following steps toodelete and regenerate hello MEF cache.</span></span>

1. <span data-ttu-id="2f0e4-161">Zárja be a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2f0e4-161">Close all instances of Visual Studio.</span></span>
1. <span data-ttu-id="2f0e4-162">Nyissa meg a % USERPROFILE%\AppData\Local\Microsoft\VisualStudio\14.0\.</span><span class="sxs-lookup"><span data-stu-id="2f0e4-162">Open %USERPROFILE%\AppData\Local\Microsoft\VisualStudio\14.0\.</span></span>
1. <span data-ttu-id="2f0e4-163">A következő mappák hello törlése:</span><span class="sxs-lookup"><span data-stu-id="2f0e4-163">Delete hello following folders:</span></span>
     ```
       ComponentModelCache
       Extensions
       MEFCacheBackup
    ```
1. <span data-ttu-id="2f0e4-164">Nyissa meg a Visual Studiót.</span><span class="sxs-lookup"><span data-stu-id="2f0e4-164">Open Visual Studio.</span></span>
1. <span data-ttu-id="2f0e4-165">Próbálja megismételni az hello forgatókönyv.</span><span class="sxs-lookup"><span data-stu-id="2f0e4-165">Attempt hello scenario again.</span></span>
