---
title: "SensorTag eszköz & Azure IoT átjáró - rész 2: Get tools (Windows) |} Microsoft Docs"
description: "Hello eszközeinek és hello szoftver telepítése a Windows rendszert futtató állomáson, létrehoz egy IoT-központot, és az eszközt regisztrálni kell az IoT-központ hello."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "az IOT-fejlesztési, iot szoftver, iot felhőalapú szolgáltatás, dolgot szoftvert, az azure CLI-t, az internet telepítse a git szoftvert a windows, Futtatás gulp, csomópont js windows telepítése, telepítse a windows npm, python Windows telepítése"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: 18ae6ee4-574a-4d5f-9838-ca2a78165628
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 3b30b60a0115413394992061a88dde4cd442ac19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-tools-windows-7-and-later"></a><span data-ttu-id="241d8-104">Első hello eszközök (Windows 7 és újabb verziók)</span><span class="sxs-lookup"><span data-stu-id="241d8-104">Get hello tools (Windows 7 and later)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="241d8-105">Windows 7 vagy újabb</span><span class="sxs-lookup"><span data-stu-id="241d8-105">Windows 7 or later</span></span>](iot-hub-gateway-kit-c-lesson2-get-the-tools-win32.md)
> * [<span data-ttu-id="241d8-106">Ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="241d8-106">Ubuntu 16.04</span></span>](iot-hub-gateway-kit-c-lesson2-get-the-tools-ubuntu.md)
> * [<span data-ttu-id="241d8-107">macOS 10.10</span><span class="sxs-lookup"><span data-stu-id="241d8-107">macOS 10.10</span></span>](iot-hub-gateway-kit-c-lesson2-get-the-tools-mac.md)

## <a name="what-you-will-do"></a><span data-ttu-id="241d8-108">Mit fog</span><span class="sxs-lookup"><span data-stu-id="241d8-108">What you will do</span></span>

- <span data-ttu-id="241d8-109">Telepítse a Git, Node.js, Gulp, Python.</span><span class="sxs-lookup"><span data-stu-id="241d8-109">Install Git, Node.js, Gulp, Python.</span></span>
- <span data-ttu-id="241d8-110">Hello Azure parancssori felület (CLI) telepítése.</span><span class="sxs-lookup"><span data-stu-id="241d8-110">Install hello Azure command-line interface (Azure CLI).</span></span> 

<span data-ttu-id="241d8-111">Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási](iot-hub-gateway-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="241d8-111">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-gateway-kit-c-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="241d8-112">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="241d8-112">What you will learn</span></span>

<span data-ttu-id="241d8-113">Ez a lecke témák köre:</span><span class="sxs-lookup"><span data-stu-id="241d8-113">In this lesson, you will learn:</span></span>

- <span data-ttu-id="241d8-114">Hogyan tooinstall [Git](https://git-scm.com/) és [Node.js](https://nodejs.org/en/).</span><span class="sxs-lookup"><span data-stu-id="241d8-114">How tooinstall [Git](https://git-scm.com/) and [Node.js](https://nodejs.org/en/).</span></span>
  - <span data-ttu-id="241d8-115">Git egy nyílt forráskódú elosztott verziókezelő rendszer.</span><span class="sxs-lookup"><span data-stu-id="241d8-115">Git is an open source distributed version control system.</span></span> <span data-ttu-id="241d8-116">Ez a lecke hello-mintaalkalmazás Git tárolja.</span><span class="sxs-lookup"><span data-stu-id="241d8-116">hello sample application for this lesson is stored on Git.</span></span>
  - <span data-ttu-id="241d8-117">NODE.js a JavaScript futásidejű és gazdag csomag-ökoszisztéma.</span><span class="sxs-lookup"><span data-stu-id="241d8-117">Node.js is a JavaScript runtime with a rich package ecosystem.</span></span>
- <span data-ttu-id="241d8-118">Hogyan toouse [NPM](https://www.npmjs.com/) tooinstall Node.js fejlesztői eszközök.</span><span class="sxs-lookup"><span data-stu-id="241d8-118">How toouse [NPM](https://www.npmjs.com/) tooinstall Node.js development tools.</span></span>
  - <span data-ttu-id="241d8-119">hello minimálisan szükséges verziója Node.js 4.5-ös LTS.</span><span class="sxs-lookup"><span data-stu-id="241d8-119">hello minimum required version of Node.js is 4.5 LTS.</span></span>
  - <span data-ttu-id="241d8-120">NPM hello csomag kezelők a Node.js egyike.</span><span class="sxs-lookup"><span data-stu-id="241d8-120">NPM is one of hello package managers for Node.js.</span></span>
- <span data-ttu-id="241d8-121">Hogyan tooinstall Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="241d8-121">How tooinstall Visual Studio Code.</span></span>
  - <span data-ttu-id="241d8-122">A Visual Studio Code egy többplatformos, a Windows, Linux és macOS egyszerűsített, de hatékony forráskód szerkesztőjében.</span><span class="sxs-lookup"><span data-stu-id="241d8-122">Visual Studio Code is a cross platform, lightweight but powerful source code editor for Windows, Linux, and macOS.</span></span> <span data-ttu-id="241d8-123">Nagyszerű hibakeresés, beágyazott Git-vezérlő, szintaxis kiemelését, intelligens kód befejezési, kódtöredékek, és támogatása kód újrabontása is rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="241d8-123">It has great support for debugging, embedded Git control, syntax highlighting, intelligent code completion, snippets, and code refactoring as well.</span></span>
- <span data-ttu-id="241d8-124">Hogyan tooinstall Python.</span><span class="sxs-lookup"><span data-stu-id="241d8-124">How tooinstall Python.</span></span>
  - <span data-ttu-id="241d8-125">Python széles körben használt magas szintű, általános célú, értelmezett és dinamikus programozási nyelv.</span><span class="sxs-lookup"><span data-stu-id="241d8-125">Python is a widely used high-level, general-purpose, interpreted and dynamic programming language.</span></span>
- <span data-ttu-id="241d8-126">Hogyan tooinstall hello Azure parancssori felület.</span><span class="sxs-lookup"><span data-stu-id="241d8-126">How tooinstall hello Azure CLI.</span></span>
  - <span data-ttu-id="241d8-127">hello Azure CLI olyan többplatformos parancssori környezetet biztosít az Azure.</span><span class="sxs-lookup"><span data-stu-id="241d8-127">hello Azure CLI provides a multiplatform command-line experience for Azure.</span></span> <span data-ttu-id="241d8-128">Közvetlenül egy parancssor tooprovision dolgozhassanak és kezelheti az erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="241d8-128">You work directly from a command line tooprovision and manage resources.</span></span>
- <span data-ttu-id="241d8-129">Hogyan toouse hello Azure CLI toocreate egy IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="241d8-129">How toouse hello Azure CLI toocreate an IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="241d8-130">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="241d8-130">What you need</span></span>

- <span data-ttu-id="241d8-131">Az Internet kapcsolat toodownload hello eszközök és szoftverek.</span><span class="sxs-lookup"><span data-stu-id="241d8-131">An Internet connection toodownload hello tools and software.</span></span>
- <span data-ttu-id="241d8-132">A Windows rendszerű számítógépeken.</span><span class="sxs-lookup"><span data-stu-id="241d8-132">A Windows computer.</span></span>

## <a name="install-git-and-nodejs"></a><span data-ttu-id="241d8-133">Telepítse a Git szoftver, Node.js</span><span class="sxs-lookup"><span data-stu-id="241d8-133">Install Git and Node.js</span></span>

<span data-ttu-id="241d8-134">A következő hivatkozások toodownload hello kattintson, és telepítse a Git és a Node.js-es lts verzió Windows.</span><span class="sxs-lookup"><span data-stu-id="241d8-134">Click hello following links toodownload and install Git and Node.js LTS for Windows.</span></span>

- [<span data-ttu-id="241d8-135">Git letöltése a Windows rendszerhez</span><span class="sxs-lookup"><span data-stu-id="241d8-135">Get Git for Windows</span></span>](https://git-scm.com/download/win/)
- [<span data-ttu-id="241d8-136">Node.js-es lts verzió letöltése a Windows rendszerhez</span><span class="sxs-lookup"><span data-stu-id="241d8-136">Get Node.js LTS for Windows</span></span>](https://nodejs.org/en/)

## <a name="install-nodejs-development-tools"></a><span data-ttu-id="241d8-137">Node.js fejlesztői eszközök telepítése</span><span class="sxs-lookup"><span data-stu-id="241d8-137">Install Node.js development tools</span></span>

<span data-ttu-id="241d8-138">Használhat [gulp.js](http://gulpjs.com/) tooautomate üzembe helyezési és parancsprogramok végrehajtását.</span><span class="sxs-lookup"><span data-stu-id="241d8-138">You use [gulp.js](http://gulpjs.com/) tooautomate deployment and execution of scripts.</span></span>

<span data-ttu-id="241d8-139">Nyomja le az ENTER `Windows + R`, típus `cmd` nyomja le az ENTER `Enter` tooopen egy parancssori ablakot, és majd a futtatási hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="241d8-139">Press `Windows + R`, type `cmd` and press `Enter` tooopen a Command Prompt window, and then run hello following command:</span></span>

```cmd
npm install -g gulp
```

<span data-ttu-id="241d8-140">Hello telepítési problémákat tapasztal, tekintse meg a hello [hibaelhárítási útmutatója](iot-hub-gateway-kit-c-troubleshooting.md) a megoldások toocommon problémákat.</span><span class="sxs-lookup"><span data-stu-id="241d8-140">If you experience issues with hello installation, see hello [troubleshooting guide](iot-hub-gateway-kit-c-troubleshooting.md) for solutions toocommon problems.</span></span>

> [!Note]
> <span data-ttu-id="241d8-141">Csomópont, NPM és Gulp a node.js fejlesztett szükséges toorun automatizálási parancsfájlokat.</span><span class="sxs-lookup"><span data-stu-id="241d8-141">Node, NPM and Gulp are required toorun automation scripts developed in Node.js.</span></span>

## <a name="install-python"></a><span data-ttu-id="241d8-142">Python telepítése</span><span class="sxs-lookup"><span data-stu-id="241d8-142">Install Python</span></span>

<span data-ttu-id="241d8-143">Python 2.7, 3.4-es vagy 3.5-ös közül választhat.</span><span class="sxs-lookup"><span data-stu-id="241d8-143">You can choose from Python 2.7, 3.4 or 3.5.</span></span> <span data-ttu-id="241d8-144">Ebben az oktatóanyagban a Python 2.7 használjuk.</span><span class="sxs-lookup"><span data-stu-id="241d8-144">In this tutorial, we use Python 2.7.</span></span> <span data-ttu-id="241d8-145">Ha már telepítette a python, nyissa meg a következő szakasz toohello.</span><span class="sxs-lookup"><span data-stu-id="241d8-145">If you've already installed python, go toohello next section.</span></span>

[<span data-ttu-id="241d8-146">Python letöltése a Windows rendszerhez</span><span class="sxs-lookup"><span data-stu-id="241d8-146">Get Python for Windows</span></span>](https://www.python.org/downloads/)

<span data-ttu-id="241d8-147">Szükség tooadd hello elérési útját, amelyben Python.exe és pip.exe is telepített toohello rendszer hello mappák `PATH` környezeti változó.</span><span class="sxs-lookup"><span data-stu-id="241d8-147">You also need tooadd hello path of hello folders where Python.exe and pip.exe are installed toohello system `PATH` environment variable.</span></span> <span data-ttu-id="241d8-148">Alapértelmezés szerint a python.exe telepítve van a `C:\Python27` és pip.exe telepítve van-e `C:\Python27\Scripts`.</span><span class="sxs-lookup"><span data-stu-id="241d8-148">By default, python.exe is installed in `C:\Python27` and pip.exe is installed in `C:\Python27\Scripts`.</span></span>

## <a name="install-hello-azure-cli"></a><span data-ttu-id="241d8-149">Hello Azure parancssori felület telepítése</span><span class="sxs-lookup"><span data-stu-id="241d8-149">Install hello Azure CLI</span></span>

<span data-ttu-id="241d8-150">tooinstall hello Azure CLI használata esetén kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="241d8-150">tooinstall hello Azure CLI, follow these steps:</span></span>

1. <span data-ttu-id="241d8-151">Nyisson meg egy parancssori ablakot rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="241d8-151">Open a Command Prompt window as an administrator.</span></span>

2. <span data-ttu-id="241d8-152">Hello Azure parancssori felület telepítése hello a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="241d8-152">Install hello Azure CLI by running hello following commands:</span></span>

   ```cmd
   pip install --upgrade azure-cli
   pip install --upgrade azure-cli-iot
   ```

   <span data-ttu-id="241d8-153">hello telepítési 5 percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="241d8-153">hello installation might take 5 minutes.</span></span>

3. <span data-ttu-id="241d8-154">Ellenőrizze a hello telepítést hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="241d8-154">Verify hello installation by running hello following command:</span></span>

   ```cmd
   az iot -h
   ```

   <span data-ttu-id="241d8-155">Meg kell jelennie a hello parancskimenet sikeres hello telepítés esetén.</span><span class="sxs-lookup"><span data-stu-id="241d8-155">You should see hello following output if hello installation is successful.</span></span>

   ![Az Azure parancssori felület telepítésének ellenőrzése](media/iot-hub-gateway-kit-lessons/lesson2/az_iot_help_win.png)

## <a name="install-visual-studio-code"></a><span data-ttu-id="241d8-157">Visual Studio Code telepítése</span><span class="sxs-lookup"><span data-stu-id="241d8-157">Install Visual Studio Code</span></span>

<span data-ttu-id="241d8-158">Visual Studio Code később használható hello oktatóanyag tooedit konfigurációs fájlok.</span><span class="sxs-lookup"><span data-stu-id="241d8-158">You use Visual Studio Code later in hello tutorial tooedit configuration files.</span></span>

<span data-ttu-id="241d8-159">[Töltse le](https://code.visualstudio.com/docs/setup/windows) és a Visual Studio Code telepítése.</span><span class="sxs-lookup"><span data-stu-id="241d8-159">[Download](https://code.visualstudio.com/docs/setup/windows) and install Visual Studio Code.</span></span>

## <a name="summary"></a><span data-ttu-id="241d8-160">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="241d8-160">Summary</span></span>

<span data-ttu-id="241d8-161">A gazdaszámítógépen telepített összes hello szükséges eszközök és szoftverek.</span><span class="sxs-lookup"><span data-stu-id="241d8-161">You've installed all hello required tools and software on your host computer.</span></span> <span data-ttu-id="241d8-162">A következő feladathoz toouse hello Azure CLI toocreate egy IoT-központot, és regisztrálja az eszközt az IoT hub.</span><span class="sxs-lookup"><span data-stu-id="241d8-162">Your next task is toouse hello Azure CLI toocreate an IoT hub and register your device in your IoT hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="241d8-163">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="241d8-163">Next steps</span></span>
[<span data-ttu-id="241d8-164">IoT Hub létrehozása és az eszköz regisztrálása</span><span class="sxs-lookup"><span data-stu-id="241d8-164">Create an IoT hub and register your device</span></span>](iot-hub-gateway-kit-c-lesson2-register-device.md)
