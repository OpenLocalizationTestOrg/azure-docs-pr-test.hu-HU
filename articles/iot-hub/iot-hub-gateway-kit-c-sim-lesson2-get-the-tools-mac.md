---
title: "A szimulált eszköz & Azure IoT átjáró - lecke 2: eszközök (macOS) beszerzése |} Microsoft Docs"
description: "A Mac számítógépen telepíti, létrehoz egy IoT-központot, és regisztrálja az eszközt az IoT-központ az."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "az IOT-fejlesztési, iot szoftver, iot felhőalapú szolgáltatás, internet dolgot szoftvert, az azure cli, install python mac, telepítse a git mac, gulp futtatja, a telepítés csomópont js mac gépen"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: 42f9d186-e20c-4ef9-98cc-71d39e058b06
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: d86332816130de7a6951a74ceb215c8ce476d5f8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-the-tools-macos"></a><span data-ttu-id="904df-104">Szerezze be az eszközöket (macOS)</span><span class="sxs-lookup"><span data-stu-id="904df-104">Get the tools (macOS)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="904df-105">Windows 7 vagy újabb</span><span class="sxs-lookup"><span data-stu-id="904df-105">Windows 7 or later</span></span>](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-win32.md)
> * [<span data-ttu-id="904df-106">Ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="904df-106">Ubuntu 16.04</span></span>](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-ubuntu.md)
> * [<span data-ttu-id="904df-107">macOS 10.10</span><span class="sxs-lookup"><span data-stu-id="904df-107">macOS 10.10</span></span>](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-mac.md)

## <a name="what-you-will-do"></a><span data-ttu-id="904df-108">Mit fog</span><span class="sxs-lookup"><span data-stu-id="904df-108">What you will do</span></span>

- <span data-ttu-id="904df-109">Telepítse a Git, Node.js, Gulp, Python.</span><span class="sxs-lookup"><span data-stu-id="904df-109">Install Git, Node.js, Gulp, Python.</span></span>
- <span data-ttu-id="904df-110">Telepítse az Azure parancssori felület (CLI).</span><span class="sxs-lookup"><span data-stu-id="904df-110">Install the Azure command-line interface (Azure CLI).</span></span> 

<span data-ttu-id="904df-111">Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási](iot-hub-gateway-kit-c-sim-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="904df-111">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-gateway-kit-c-sim-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="904df-112">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="904df-112">What you will learn</span></span>

<span data-ttu-id="904df-113">Ez a lecke témák köre:</span><span class="sxs-lookup"><span data-stu-id="904df-113">In this lesson, you will learn:</span></span>

- <span data-ttu-id="904df-114">Telepítése [Git](https://git-scm.com/) és [Node.js](https://nodejs.org/en/).</span><span class="sxs-lookup"><span data-stu-id="904df-114">How to install [Git](https://git-scm.com/) and [Node.js](https://nodejs.org/en/).</span></span>
  - <span data-ttu-id="904df-115">Git egy nyílt forráskódú elosztott verziókezelő rendszer.</span><span class="sxs-lookup"><span data-stu-id="904df-115">Git is an open source distributed version control system.</span></span> <span data-ttu-id="904df-116">Ez a lecke a mintaalkalmazás Git tárolja.</span><span class="sxs-lookup"><span data-stu-id="904df-116">The sample application for this lesson is stored on Git.</span></span>
  - <span data-ttu-id="904df-117">NODE.js a JavaScript futásidejű és gazdag csomag-ökoszisztéma.</span><span class="sxs-lookup"><span data-stu-id="904df-117">Node.js is a JavaScript runtime with a rich package ecosystem.</span></span>
- <span data-ttu-id="904df-118">Használata [NPM](https://www.npmjs.com/) Node.js fejlesztői eszközök telepítése.</span><span class="sxs-lookup"><span data-stu-id="904df-118">How to use [NPM](https://www.npmjs.com/) to install Node.js development tools.</span></span>
  - <span data-ttu-id="904df-119">A Node.js minimálisan szükséges verziója a 4.5-ös LTS.</span><span class="sxs-lookup"><span data-stu-id="904df-119">The minimum required version of Node.js is 4.5 LTS.</span></span>
  - <span data-ttu-id="904df-120">NPM egyike a Node.js csomag feletteseit.</span><span class="sxs-lookup"><span data-stu-id="904df-120">NPM is one of the package managers for Node.js.</span></span>
- <span data-ttu-id="904df-121">Hogyan kell telepíteni a Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="904df-121">How to install Visual Studio Code.</span></span>
  - <span data-ttu-id="904df-122">A Visual Studio Code egy többplatformos, a Windows, Linux és macOS egyszerűsített, de hatékony forráskód szerkesztőjében.</span><span class="sxs-lookup"><span data-stu-id="904df-122">Visual Studio Code is a cross platform, lightweight but powerful source code editor for Windows, Linux, and macOS.</span></span> <span data-ttu-id="904df-123">Nagyszerű hibakeresés, beágyazott Git-vezérlő, szintaxis kiemelését, intelligens kód befejezési, kódtöredékek, és támogatása kód újrabontása is rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="904df-123">It has great support for debugging, embedded Git control, syntax highlighting, intelligent code completion, snippets, and code refactoring as well.</span></span>
- <span data-ttu-id="904df-124">Hogyan kell telepíteni a Python.</span><span class="sxs-lookup"><span data-stu-id="904df-124">How to install Python.</span></span>
  - <span data-ttu-id="904df-125">Python széles körben használt magas szintű, általános célú, értelmezett és dinamikus programozási nyelv.</span><span class="sxs-lookup"><span data-stu-id="904df-125">Python is a widely used high-level, general-purpose, interpreted and dynamic programming language.</span></span>
- <span data-ttu-id="904df-126">Tudnivalók az Azure parancssori felület telepítése.</span><span class="sxs-lookup"><span data-stu-id="904df-126">How to install the Azure CLI.</span></span>
  - <span data-ttu-id="904df-127">Az Azure parancssori felület olyan többplatformos parancssori környezetet biztosít az Azure.</span><span class="sxs-lookup"><span data-stu-id="904df-127">The Azure CLI provides a multiplatform command-line experience for Azure.</span></span> <span data-ttu-id="904df-128">Közvetlenül a parancssorból kiépítését működik, és kezelheti az erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="904df-128">You work directly from a command line to provision and manage resources.</span></span>
- <span data-ttu-id="904df-129">Hogyan lehet az Azure CLI segítségével létrehoz egy IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="904df-129">How to use the Azure CLI to create an IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="904df-130">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="904df-130">What you need</span></span>

- <span data-ttu-id="904df-131">Az eszközök és szoftverek letöltése az internethez.</span><span class="sxs-lookup"><span data-stu-id="904df-131">An Internet connection to download the tools and software.</span></span>
- <span data-ttu-id="904df-132">Az operációs rendszer X Yosemite (10.10) vagy újabb rendszert futtató Mac számítógép.</span><span class="sxs-lookup"><span data-stu-id="904df-132">A Mac computer that’s running OS X Yosemite (10.10) or later.</span></span>

## <a name="install-git-and-nodejs"></a><span data-ttu-id="904df-133">Telepítse a Git szoftver, Node.js</span><span class="sxs-lookup"><span data-stu-id="904df-133">Install Git and Node.js</span></span>

<span data-ttu-id="904df-134">Telepítse a Git szoftver, Node.js, a segédprogrammal Homebrew csomag felügyeleti ezeket a lépéseket követve:</span><span class="sxs-lookup"><span data-stu-id="904df-134">To install Git and Node.js, use the Homebrew package management utility by following these steps:</span></span>

1. <span data-ttu-id="904df-135">[Töltse le](http://brew.sh/) és Homebrew telepítse.</span><span class="sxs-lookup"><span data-stu-id="904df-135">[Download](http://brew.sh/) and install Homebrew.</span></span> <span data-ttu-id="904df-136">Ha már telepített Homebrew, folytassa a 2.</span><span class="sxs-lookup"><span data-stu-id="904df-136">If you’ve already installed Homebrew, go to step 2.</span></span>
   1. <span data-ttu-id="904df-137">Nyomja le az `Cmd + Space` , és írja be `Terminal` egy terminált megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="904df-137">Press `Cmd + Space` and enter `Terminal` to open a terminal.</span></span>
   2. <span data-ttu-id="904df-138">Futtassa az alábbi parancsot:</span><span class="sxs-lookup"><span data-stu-id="904df-138">Run the following command:</span></span>

      ```bash
      /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
      ```

2. <span data-ttu-id="904df-139">Telepítse a Git és a Node.js, a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="904df-139">Install Git and Node.js by running the following command:</span></span>

    ```bash
    brew install node git
    ```

## <a name="install-nodejs-development-tools"></a><span data-ttu-id="904df-140">Node.js fejlesztői eszközök telepítése</span><span class="sxs-lookup"><span data-stu-id="904df-140">Install Node.js development tools</span></span>

<span data-ttu-id="904df-141">Használhat [gulp.js](http://gulpjs.com/) automatikus központi telepítési és a parancsprogramok végrehajtását.</span><span class="sxs-lookup"><span data-stu-id="904df-141">You use [gulp.js](http://gulpjs.com/) to automate deployment and execution of scripts.</span></span>

<span data-ttu-id="904df-142">Gulp telepítéséhez futtassa a terminálban a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="904df-142">To install gulp, run the following command in the terminal:</span></span>

```bash
npm install -g gulp
```

<span data-ttu-id="904df-143">Ha a telepítési problémákat tapasztal, tekintse meg a [hibaelhárítási útmutatója](iot-hub-gateway-kit-c-sim-troubleshooting.md) gyakori problémák megoldásainak.</span><span class="sxs-lookup"><span data-stu-id="904df-143">If you experience issues with the installation, see the [troubleshooting guide](iot-hub-gateway-kit-c-sim-troubleshooting.md) for solutions to common problems.</span></span>

> [!Note]
> <span data-ttu-id="904df-144">Csomópont, NPM és Gulp Node.js létre automatizálási parancsfájlokat futtatásához szükséges.</span><span class="sxs-lookup"><span data-stu-id="904df-144">Node, NPM and Gulp are required to run automation scripts developed in Node.js.</span></span>

## <a name="install-python"></a><span data-ttu-id="904df-145">Python telepítése</span><span class="sxs-lookup"><span data-stu-id="904df-145">Install Python</span></span>

<span data-ttu-id="904df-146">Bár Mac OS X Python 2.7, azt javasoljuk, hogy telepítse a Python Homebrew keresztül.</span><span class="sxs-lookup"><span data-stu-id="904df-146">Although Mac OS X comes with Python 2.7, we recommend that you install Python through Homebrew.</span></span> <span data-ttu-id="904df-147">Lásd: [Python telepítése Mac OS x](http://docs.python-guide.org/en/latest/starting/install/osx/).</span><span class="sxs-lookup"><span data-stu-id="904df-147">See [Installing Python on Mac OS X](http://docs.python-guide.org/en/latest/starting/install/osx/).</span></span>

<span data-ttu-id="904df-148">Telepítse a Python és a pip a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="904df-148">Install Python and pip by running the following command:</span></span>

```bash
brew install python
```

## <a name="install-the-azure-cli"></a><span data-ttu-id="904df-149">Telepítse az Azure CLI-t</span><span class="sxs-lookup"><span data-stu-id="904df-149">Install the Azure CLI</span></span>

<span data-ttu-id="904df-150">Az Azure parancssori felület telepítéséhez kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="904df-150">To install the Azure CLI, follow these steps:</span></span>

1. <span data-ttu-id="904df-151">A következő parancsokat a terminálban:</span><span class="sxs-lookup"><span data-stu-id="904df-151">Run the following commands in the terminal:</span></span>
   ```bash
   pip install --upgrade azure-cli
   pip install --upgrade azure-cli-iot
   ```
   <span data-ttu-id="904df-152">A telepítés 5 percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="904df-152">The installation might take 5 minutes.</span></span>

2. <span data-ttu-id="904df-153">Ellenőrizze a telepítést a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="904df-153">Verify the installation by running the following command:</span></span>
   ```bash
   az iot -h
   ```
   <span data-ttu-id="904df-154">Ha a telepítés sikerült a következő kimenetet kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="904df-154">You should see the following output if the installation is successful.</span></span>

   ![Az Azure parancssori felület telepítésének ellenőrzése](media/iot-hub-gateway-kit-lessons/lesson2/az_iot_help_osx.png)

## <a name="install-visual-studio-code"></a><span data-ttu-id="904df-156">Visual Studio Code telepítése</span><span class="sxs-lookup"><span data-stu-id="904df-156">Install Visual Studio Code</span></span>

<span data-ttu-id="904df-157">Visual Studio Code az oktatóanyag későbbi részében, konfigurációs fájlok szerkesztésére használhatja.</span><span class="sxs-lookup"><span data-stu-id="904df-157">You use Visual Studio Code later in the tutorial to edit configuration files.</span></span>

<span data-ttu-id="904df-158">[Töltse le](https://code.visualstudio.com/docs/setup/osx) és a Visual Studio Code telepítése.</span><span class="sxs-lookup"><span data-stu-id="904df-158">[Download](https://code.visualstudio.com/docs/setup/osx) and install Visual Studio Code.</span></span>

## <a name="summary"></a><span data-ttu-id="904df-159">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="904df-159">Summary</span></span>

<span data-ttu-id="904df-160">Telepítette a szükséges eszközök és a szoftver a Mac számítógépen.</span><span class="sxs-lookup"><span data-stu-id="904df-160">You’ve installed all the required tools and software on your Mac computer.</span></span> <span data-ttu-id="904df-161">A következő feladathoz az Azure parancssori felület használatával létrehoz egy IoT-központot, és regisztrálja az eszközt az IoT hub.</span><span class="sxs-lookup"><span data-stu-id="904df-161">Your next task is to use the Azure CLI to create an IoT hub and register your device in your IoT hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="904df-162">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="904df-162">Next steps</span></span>
[<span data-ttu-id="904df-163">Az IoT-központ létrehozni és regisztrálni az eszközt</span><span class="sxs-lookup"><span data-stu-id="904df-163">Create an IoT hub and register Device</span></span>](iot-hub-gateway-kit-c-sim-lesson2-register-device.md)
