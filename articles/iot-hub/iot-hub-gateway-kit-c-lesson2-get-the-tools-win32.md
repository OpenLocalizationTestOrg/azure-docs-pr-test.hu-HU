---
title: "SensorTag eszköz & Azure IoT átjáró - rész 2: Get tools (Windows) |} Microsoft Docs"
description: "Az eszközök és a szoftvert telepíteni a a Windows rendszert futtató számítógépen, létrehoz egy IoT-központot, és regisztrálja az eszközt az IoT-központ az."
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
ms.openlocfilehash: 0d8ba03df63d0b8657a9e275fc636e806c66b683
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-the-tools-windows-7-and-later"></a><span data-ttu-id="714a6-104">Szerezze be az eszközöket (Windows 7 és újabb verziók)</span><span class="sxs-lookup"><span data-stu-id="714a6-104">Get the tools (Windows 7 and later)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="714a6-105">Windows 7 vagy újabb</span><span class="sxs-lookup"><span data-stu-id="714a6-105">Windows 7 or later</span></span>](iot-hub-gateway-kit-c-lesson2-get-the-tools-win32.md)
> * [<span data-ttu-id="714a6-106">Ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="714a6-106">Ubuntu 16.04</span></span>](iot-hub-gateway-kit-c-lesson2-get-the-tools-ubuntu.md)
> * [<span data-ttu-id="714a6-107">macOS 10.10</span><span class="sxs-lookup"><span data-stu-id="714a6-107">macOS 10.10</span></span>](iot-hub-gateway-kit-c-lesson2-get-the-tools-mac.md)

## <a name="what-you-will-do"></a><span data-ttu-id="714a6-108">Mit fog</span><span class="sxs-lookup"><span data-stu-id="714a6-108">What you will do</span></span>

- <span data-ttu-id="714a6-109">Telepítse a Git, Node.js, Gulp, Python.</span><span class="sxs-lookup"><span data-stu-id="714a6-109">Install Git, Node.js, Gulp, Python.</span></span>
- <span data-ttu-id="714a6-110">Telepítse az Azure parancssori felület (CLI).</span><span class="sxs-lookup"><span data-stu-id="714a6-110">Install the Azure command-line interface (Azure CLI).</span></span> 

<span data-ttu-id="714a6-111">Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási](iot-hub-gateway-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="714a6-111">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-gateway-kit-c-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="714a6-112">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="714a6-112">What you will learn</span></span>

<span data-ttu-id="714a6-113">Ez a lecke témák köre:</span><span class="sxs-lookup"><span data-stu-id="714a6-113">In this lesson, you will learn:</span></span>

- <span data-ttu-id="714a6-114">Telepítése [Git](https://git-scm.com/) és [Node.js](https://nodejs.org/en/).</span><span class="sxs-lookup"><span data-stu-id="714a6-114">How to install [Git](https://git-scm.com/) and [Node.js](https://nodejs.org/en/).</span></span>
  - <span data-ttu-id="714a6-115">Git egy nyílt forráskódú elosztott verziókezelő rendszer.</span><span class="sxs-lookup"><span data-stu-id="714a6-115">Git is an open source distributed version control system.</span></span> <span data-ttu-id="714a6-116">Ez a lecke a mintaalkalmazás Git tárolja.</span><span class="sxs-lookup"><span data-stu-id="714a6-116">The sample application for this lesson is stored on Git.</span></span>
  - <span data-ttu-id="714a6-117">NODE.js a JavaScript futásidejű és gazdag csomag-ökoszisztéma.</span><span class="sxs-lookup"><span data-stu-id="714a6-117">Node.js is a JavaScript runtime with a rich package ecosystem.</span></span>
- <span data-ttu-id="714a6-118">Használata [NPM](https://www.npmjs.com/) Node.js fejlesztői eszközök telepítése.</span><span class="sxs-lookup"><span data-stu-id="714a6-118">How to use [NPM](https://www.npmjs.com/) to install Node.js development tools.</span></span>
  - <span data-ttu-id="714a6-119">A Node.js minimálisan szükséges verziója a 4.5-ös LTS.</span><span class="sxs-lookup"><span data-stu-id="714a6-119">The minimum required version of Node.js is 4.5 LTS.</span></span>
  - <span data-ttu-id="714a6-120">NPM egyike a Node.js csomag feletteseit.</span><span class="sxs-lookup"><span data-stu-id="714a6-120">NPM is one of the package managers for Node.js.</span></span>
- <span data-ttu-id="714a6-121">Hogyan kell telepíteni a Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="714a6-121">How to install Visual Studio Code.</span></span>
  - <span data-ttu-id="714a6-122">A Visual Studio Code egy többplatformos, a Windows, Linux és macOS egyszerűsített, de hatékony forráskód szerkesztőjében.</span><span class="sxs-lookup"><span data-stu-id="714a6-122">Visual Studio Code is a cross platform, lightweight but powerful source code editor for Windows, Linux, and macOS.</span></span> <span data-ttu-id="714a6-123">Nagyszerű hibakeresés, beágyazott Git-vezérlő, szintaxis kiemelését, intelligens kód befejezési, kódtöredékek, és támogatása kód újrabontása is rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="714a6-123">It has great support for debugging, embedded Git control, syntax highlighting, intelligent code completion, snippets, and code refactoring as well.</span></span>
- <span data-ttu-id="714a6-124">Hogyan kell telepíteni a Python.</span><span class="sxs-lookup"><span data-stu-id="714a6-124">How to install Python.</span></span>
  - <span data-ttu-id="714a6-125">Python széles körben használt magas szintű, általános célú, értelmezett és dinamikus programozási nyelv.</span><span class="sxs-lookup"><span data-stu-id="714a6-125">Python is a widely used high-level, general-purpose, interpreted and dynamic programming language.</span></span>
- <span data-ttu-id="714a6-126">Tudnivalók az Azure parancssori felület telepítése.</span><span class="sxs-lookup"><span data-stu-id="714a6-126">How to install the Azure CLI.</span></span>
  - <span data-ttu-id="714a6-127">Az Azure parancssori felület olyan többplatformos parancssori környezetet biztosít az Azure.</span><span class="sxs-lookup"><span data-stu-id="714a6-127">The Azure CLI provides a multiplatform command-line experience for Azure.</span></span> <span data-ttu-id="714a6-128">Közvetlenül a parancssorból kiépítését működik, és kezelheti az erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="714a6-128">You work directly from a command line to provision and manage resources.</span></span>
- <span data-ttu-id="714a6-129">Hogyan lehet az Azure CLI segítségével létrehoz egy IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="714a6-129">How to use the Azure CLI to create an IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="714a6-130">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="714a6-130">What you need</span></span>

- <span data-ttu-id="714a6-131">Az eszközök és szoftverek letöltése az internethez.</span><span class="sxs-lookup"><span data-stu-id="714a6-131">An Internet connection to download the tools and software.</span></span>
- <span data-ttu-id="714a6-132">A Windows rendszerű számítógépeken.</span><span class="sxs-lookup"><span data-stu-id="714a6-132">A Windows computer.</span></span>

## <a name="install-git-and-nodejs"></a><span data-ttu-id="714a6-133">Telepítse a Git szoftver, Node.js</span><span class="sxs-lookup"><span data-stu-id="714a6-133">Install Git and Node.js</span></span>

<span data-ttu-id="714a6-134">Kattintson az alábbi hivatkozásokra kattintva töltse le és telepítse a Git szoftver, a Windows Node.js LTS.</span><span class="sxs-lookup"><span data-stu-id="714a6-134">Click the following links to download and install Git and Node.js LTS for Windows.</span></span>

- [<span data-ttu-id="714a6-135">Git letöltése a Windows rendszerhez</span><span class="sxs-lookup"><span data-stu-id="714a6-135">Get Git for Windows</span></span>](https://git-scm.com/download/win/)
- [<span data-ttu-id="714a6-136">Node.js-es lts verzió letöltése a Windows rendszerhez</span><span class="sxs-lookup"><span data-stu-id="714a6-136">Get Node.js LTS for Windows</span></span>](https://nodejs.org/en/)

## <a name="install-nodejs-development-tools"></a><span data-ttu-id="714a6-137">Node.js fejlesztői eszközök telepítése</span><span class="sxs-lookup"><span data-stu-id="714a6-137">Install Node.js development tools</span></span>

<span data-ttu-id="714a6-138">Használhat [gulp.js](http://gulpjs.com/) automatikus központi telepítési és a parancsprogramok végrehajtását.</span><span class="sxs-lookup"><span data-stu-id="714a6-138">You use [gulp.js](http://gulpjs.com/) to automate deployment and execution of scripts.</span></span>

<span data-ttu-id="714a6-139">Nyomja le az ENTER `Windows + R`, típus `cmd` nyomja le az ENTER `Enter` nyisson meg egy parancssori ablakot, és futtassa a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="714a6-139">Press `Windows + R`, type `cmd` and press `Enter` to open a Command Prompt window, and then run the following command:</span></span>

```cmd
npm install -g gulp
```

<span data-ttu-id="714a6-140">Ha a telepítési problémákat tapasztal, tekintse meg a [hibaelhárítási útmutatója](iot-hub-gateway-kit-c-troubleshooting.md) gyakori problémák megoldásainak.</span><span class="sxs-lookup"><span data-stu-id="714a6-140">If you experience issues with the installation, see the [troubleshooting guide](iot-hub-gateway-kit-c-troubleshooting.md) for solutions to common problems.</span></span>

> [!Note]
> <span data-ttu-id="714a6-141">Csomópont, NPM és Gulp Node.js létre automatizálási parancsfájlokat futtatásához szükséges.</span><span class="sxs-lookup"><span data-stu-id="714a6-141">Node, NPM and Gulp are required to run automation scripts developed in Node.js.</span></span>

## <a name="install-python"></a><span data-ttu-id="714a6-142">Python telepítése</span><span class="sxs-lookup"><span data-stu-id="714a6-142">Install Python</span></span>

<span data-ttu-id="714a6-143">Python 2.7, 3.4-es vagy 3.5-ös közül választhat.</span><span class="sxs-lookup"><span data-stu-id="714a6-143">You can choose from Python 2.7, 3.4 or 3.5.</span></span> <span data-ttu-id="714a6-144">Ebben az oktatóanyagban a Python 2.7 használjuk.</span><span class="sxs-lookup"><span data-stu-id="714a6-144">In this tutorial, we use Python 2.7.</span></span> <span data-ttu-id="714a6-145">Ha már telepítette a python, folytassa a következő szakasszal.</span><span class="sxs-lookup"><span data-stu-id="714a6-145">If you've already installed python, go to the next section.</span></span>

[<span data-ttu-id="714a6-146">Python letöltése a Windows rendszerhez</span><span class="sxs-lookup"><span data-stu-id="714a6-146">Get Python for Windows</span></span>](https://www.python.org/downloads/)

<span data-ttu-id="714a6-147">Ahol Python.exe és pip.exe vannak telepítve a rendszer a mappák elérési útját adja hozzá kell `PATH` környezeti változó.</span><span class="sxs-lookup"><span data-stu-id="714a6-147">You also need to add the path of the folders where Python.exe and pip.exe are installed to the system `PATH` environment variable.</span></span> <span data-ttu-id="714a6-148">Alapértelmezés szerint a python.exe telepítve van a `C:\Python27` és pip.exe telepítve van-e `C:\Python27\Scripts`.</span><span class="sxs-lookup"><span data-stu-id="714a6-148">By default, python.exe is installed in `C:\Python27` and pip.exe is installed in `C:\Python27\Scripts`.</span></span>

## <a name="install-the-azure-cli"></a><span data-ttu-id="714a6-149">Telepítse az Azure CLI-t</span><span class="sxs-lookup"><span data-stu-id="714a6-149">Install the Azure CLI</span></span>

<span data-ttu-id="714a6-150">Az Azure parancssori felület telepítéséhez kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="714a6-150">To install the Azure CLI, follow these steps:</span></span>

1. <span data-ttu-id="714a6-151">Nyisson meg egy parancssori ablakot rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="714a6-151">Open a Command Prompt window as an administrator.</span></span>

2. <span data-ttu-id="714a6-152">Az Azure parancssori felület telepítése a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="714a6-152">Install the Azure CLI by running the following commands:</span></span>

   ```cmd
   pip install --upgrade azure-cli
   pip install --upgrade azure-cli-iot
   ```

   <span data-ttu-id="714a6-153">A telepítés 5 percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="714a6-153">The installation might take 5 minutes.</span></span>

3. <span data-ttu-id="714a6-154">Ellenőrizze a telepítést a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="714a6-154">Verify the installation by running the following command:</span></span>

   ```cmd
   az iot -h
   ```

   <span data-ttu-id="714a6-155">Ha a telepítés sikerült a következő kimenetet kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="714a6-155">You should see the following output if the installation is successful.</span></span>

   ![Az Azure parancssori felület telepítésének ellenőrzése](media/iot-hub-gateway-kit-lessons/lesson2/az_iot_help_win.png)

## <a name="install-visual-studio-code"></a><span data-ttu-id="714a6-157">Visual Studio Code telepítése</span><span class="sxs-lookup"><span data-stu-id="714a6-157">Install Visual Studio Code</span></span>

<span data-ttu-id="714a6-158">Visual Studio Code az oktatóanyag későbbi részében, konfigurációs fájlok szerkesztésére használhatja.</span><span class="sxs-lookup"><span data-stu-id="714a6-158">You use Visual Studio Code later in the tutorial to edit configuration files.</span></span>

<span data-ttu-id="714a6-159">[Töltse le](https://code.visualstudio.com/docs/setup/windows) és a Visual Studio Code telepítése.</span><span class="sxs-lookup"><span data-stu-id="714a6-159">[Download](https://code.visualstudio.com/docs/setup/windows) and install Visual Studio Code.</span></span>

## <a name="summary"></a><span data-ttu-id="714a6-160">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="714a6-160">Summary</span></span>

<span data-ttu-id="714a6-161">Telepítette a szükséges eszközök és a szoftver a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="714a6-161">You've installed all the required tools and software on your host computer.</span></span> <span data-ttu-id="714a6-162">A következő feladathoz az Azure parancssori felület használatával létrehoz egy IoT-központot, és regisztrálja az eszközt az IoT hub.</span><span class="sxs-lookup"><span data-stu-id="714a6-162">Your next task is to use the Azure CLI to create an IoT hub and register your device in your IoT hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="714a6-163">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="714a6-163">Next steps</span></span>
[<span data-ttu-id="714a6-164">IoT Hub létrehozása és az eszköz regisztrálása</span><span class="sxs-lookup"><span data-stu-id="714a6-164">Create an IoT hub and register your device</span></span>](iot-hub-gateway-kit-c-lesson2-register-device.md)
