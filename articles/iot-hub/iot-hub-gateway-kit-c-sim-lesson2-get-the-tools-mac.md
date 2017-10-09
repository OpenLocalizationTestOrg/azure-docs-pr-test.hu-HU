---
title: "A szimulált eszköz & Azure IoT átjáró - lecke 2: eszközök (macOS) beszerzése |} Microsoft Docs"
description: "A Mac számítógépen telepíti, létrehoz egy IoT-központot, és az eszközt regisztrálni kell az IoT-központ hello."
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
ms.openlocfilehash: 391d60f3cbb209698cae53098efed360ac0f5fad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-tools-macos"></a><span data-ttu-id="4c6e1-104">Hello eszközök (macOS) beolvasása</span><span class="sxs-lookup"><span data-stu-id="4c6e1-104">Get hello tools (macOS)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="4c6e1-105">Windows 7 vagy újabb</span><span class="sxs-lookup"><span data-stu-id="4c6e1-105">Windows 7 or later</span></span>](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-win32.md)
> * [<span data-ttu-id="4c6e1-106">Ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="4c6e1-106">Ubuntu 16.04</span></span>](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-ubuntu.md)
> * [<span data-ttu-id="4c6e1-107">macOS 10.10</span><span class="sxs-lookup"><span data-stu-id="4c6e1-107">macOS 10.10</span></span>](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-mac.md)

## <a name="what-you-will-do"></a><span data-ttu-id="4c6e1-108">Mit fog</span><span class="sxs-lookup"><span data-stu-id="4c6e1-108">What you will do</span></span>

- <span data-ttu-id="4c6e1-109">Telepítse a Git, Node.js, Gulp, Python.</span><span class="sxs-lookup"><span data-stu-id="4c6e1-109">Install Git, Node.js, Gulp, Python.</span></span>
- <span data-ttu-id="4c6e1-110">Hello Azure parancssori felület (CLI) telepítése.</span><span class="sxs-lookup"><span data-stu-id="4c6e1-110">Install hello Azure command-line interface (Azure CLI).</span></span> 

<span data-ttu-id="4c6e1-111">Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási](iot-hub-gateway-kit-c-sim-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="4c6e1-111">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-gateway-kit-c-sim-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="4c6e1-112">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="4c6e1-112">What you will learn</span></span>

<span data-ttu-id="4c6e1-113">Ez a lecke témák köre:</span><span class="sxs-lookup"><span data-stu-id="4c6e1-113">In this lesson, you will learn:</span></span>

- <span data-ttu-id="4c6e1-114">Hogyan tooinstall [Git](https://git-scm.com/) és [Node.js](https://nodejs.org/en/).</span><span class="sxs-lookup"><span data-stu-id="4c6e1-114">How tooinstall [Git](https://git-scm.com/) and [Node.js](https://nodejs.org/en/).</span></span>
  - <span data-ttu-id="4c6e1-115">Git egy nyílt forráskódú elosztott verziókezelő rendszer.</span><span class="sxs-lookup"><span data-stu-id="4c6e1-115">Git is an open source distributed version control system.</span></span> <span data-ttu-id="4c6e1-116">Ez a lecke hello-mintaalkalmazás Git tárolja.</span><span class="sxs-lookup"><span data-stu-id="4c6e1-116">hello sample application for this lesson is stored on Git.</span></span>
  - <span data-ttu-id="4c6e1-117">NODE.js a JavaScript futásidejű és gazdag csomag-ökoszisztéma.</span><span class="sxs-lookup"><span data-stu-id="4c6e1-117">Node.js is a JavaScript runtime with a rich package ecosystem.</span></span>
- <span data-ttu-id="4c6e1-118">Hogyan toouse [NPM](https://www.npmjs.com/) tooinstall Node.js fejlesztői eszközök.</span><span class="sxs-lookup"><span data-stu-id="4c6e1-118">How toouse [NPM](https://www.npmjs.com/) tooinstall Node.js development tools.</span></span>
  - <span data-ttu-id="4c6e1-119">hello minimálisan szükséges verziója Node.js 4.5-ös LTS.</span><span class="sxs-lookup"><span data-stu-id="4c6e1-119">hello minimum required version of Node.js is 4.5 LTS.</span></span>
  - <span data-ttu-id="4c6e1-120">NPM hello csomag kezelők a Node.js egyike.</span><span class="sxs-lookup"><span data-stu-id="4c6e1-120">NPM is one of hello package managers for Node.js.</span></span>
- <span data-ttu-id="4c6e1-121">Hogyan tooinstall Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="4c6e1-121">How tooinstall Visual Studio Code.</span></span>
  - <span data-ttu-id="4c6e1-122">A Visual Studio Code egy többplatformos, a Windows, Linux és macOS egyszerűsített, de hatékony forráskód szerkesztőjében.</span><span class="sxs-lookup"><span data-stu-id="4c6e1-122">Visual Studio Code is a cross platform, lightweight but powerful source code editor for Windows, Linux, and macOS.</span></span> <span data-ttu-id="4c6e1-123">Nagyszerű hibakeresés, beágyazott Git-vezérlő, szintaxis kiemelését, intelligens kód befejezési, kódtöredékek, és támogatása kód újrabontása is rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="4c6e1-123">It has great support for debugging, embedded Git control, syntax highlighting, intelligent code completion, snippets, and code refactoring as well.</span></span>
- <span data-ttu-id="4c6e1-124">Hogyan tooinstall Python.</span><span class="sxs-lookup"><span data-stu-id="4c6e1-124">How tooinstall Python.</span></span>
  - <span data-ttu-id="4c6e1-125">Python széles körben használt magas szintű, általános célú, értelmezett és dinamikus programozási nyelv.</span><span class="sxs-lookup"><span data-stu-id="4c6e1-125">Python is a widely used high-level, general-purpose, interpreted and dynamic programming language.</span></span>
- <span data-ttu-id="4c6e1-126">Hogyan tooinstall hello Azure parancssori felület.</span><span class="sxs-lookup"><span data-stu-id="4c6e1-126">How tooinstall hello Azure CLI.</span></span>
  - <span data-ttu-id="4c6e1-127">hello Azure CLI olyan többplatformos parancssori környezetet biztosít az Azure.</span><span class="sxs-lookup"><span data-stu-id="4c6e1-127">hello Azure CLI provides a multiplatform command-line experience for Azure.</span></span> <span data-ttu-id="4c6e1-128">Közvetlenül egy parancssor tooprovision dolgozhassanak és kezelheti az erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="4c6e1-128">You work directly from a command line tooprovision and manage resources.</span></span>
- <span data-ttu-id="4c6e1-129">Hogyan toouse hello Azure CLI toocreate egy IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="4c6e1-129">How toouse hello Azure CLI toocreate an IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="4c6e1-130">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="4c6e1-130">What you need</span></span>

- <span data-ttu-id="4c6e1-131">Az Internet kapcsolat toodownload hello eszközök és szoftverek.</span><span class="sxs-lookup"><span data-stu-id="4c6e1-131">An Internet connection toodownload hello tools and software.</span></span>
- <span data-ttu-id="4c6e1-132">Az operációs rendszer X Yosemite (10.10) vagy újabb rendszert futtató Mac számítógép.</span><span class="sxs-lookup"><span data-stu-id="4c6e1-132">A Mac computer that’s running OS X Yosemite (10.10) or later.</span></span>

## <a name="install-git-and-nodejs"></a><span data-ttu-id="4c6e1-133">Telepítse a Git szoftver, Node.js</span><span class="sxs-lookup"><span data-stu-id="4c6e1-133">Install Git and Node.js</span></span>

<span data-ttu-id="4c6e1-134">tooinstall Git és a Node.js, segédprogrammal hello Homebrew csomag felügyeleti ezeket a lépéseket követve:</span><span class="sxs-lookup"><span data-stu-id="4c6e1-134">tooinstall Git and Node.js, use hello Homebrew package management utility by following these steps:</span></span>

1. <span data-ttu-id="4c6e1-135">[Töltse le](http://brew.sh/) és Homebrew telepítse.</span><span class="sxs-lookup"><span data-stu-id="4c6e1-135">[Download](http://brew.sh/) and install Homebrew.</span></span> <span data-ttu-id="4c6e1-136">Ha már telepített Homebrew, nyissa meg toostep 2.</span><span class="sxs-lookup"><span data-stu-id="4c6e1-136">If you’ve already installed Homebrew, go toostep 2.</span></span>
   1. <span data-ttu-id="4c6e1-137">Nyomja le az `Cmd + Space` , és írja be `Terminal` tooopen egy terminált.</span><span class="sxs-lookup"><span data-stu-id="4c6e1-137">Press `Cmd + Space` and enter `Terminal` tooopen a terminal.</span></span>
   2. <span data-ttu-id="4c6e1-138">Futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="4c6e1-138">Run hello following command:</span></span>

      ```bash
      /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
      ```

2. <span data-ttu-id="4c6e1-139">Telepítse a Git és Node.js hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="4c6e1-139">Install Git and Node.js by running hello following command:</span></span>

    ```bash
    brew install node git
    ```

## <a name="install-nodejs-development-tools"></a><span data-ttu-id="4c6e1-140">Node.js fejlesztői eszközök telepítése</span><span class="sxs-lookup"><span data-stu-id="4c6e1-140">Install Node.js development tools</span></span>

<span data-ttu-id="4c6e1-141">Használhat [gulp.js](http://gulpjs.com/) tooautomate üzembe helyezési és parancsprogramok végrehajtását.</span><span class="sxs-lookup"><span data-stu-id="4c6e1-141">You use [gulp.js](http://gulpjs.com/) tooautomate deployment and execution of scripts.</span></span>

<span data-ttu-id="4c6e1-142">tooinstall gulp, futtassa a következő parancs hello terminálban hello:</span><span class="sxs-lookup"><span data-stu-id="4c6e1-142">tooinstall gulp, run hello following command in hello terminal:</span></span>

```bash
npm install -g gulp
```

<span data-ttu-id="4c6e1-143">Hello telepítési problémákat tapasztal, tekintse meg a hello [hibaelhárítási útmutatója](iot-hub-gateway-kit-c-sim-troubleshooting.md) a megoldások toocommon problémákat.</span><span class="sxs-lookup"><span data-stu-id="4c6e1-143">If you experience issues with hello installation, see hello [troubleshooting guide](iot-hub-gateway-kit-c-sim-troubleshooting.md) for solutions toocommon problems.</span></span>

> [!Note]
> <span data-ttu-id="4c6e1-144">Csomópont, NPM és Gulp a node.js fejlesztett szükséges toorun automatizálási parancsfájlokat.</span><span class="sxs-lookup"><span data-stu-id="4c6e1-144">Node, NPM and Gulp are required toorun automation scripts developed in Node.js.</span></span>

## <a name="install-python"></a><span data-ttu-id="4c6e1-145">Python telepítése</span><span class="sxs-lookup"><span data-stu-id="4c6e1-145">Install Python</span></span>

<span data-ttu-id="4c6e1-146">Bár Mac OS X Python 2.7, azt javasoljuk, hogy telepítse a Python Homebrew keresztül.</span><span class="sxs-lookup"><span data-stu-id="4c6e1-146">Although Mac OS X comes with Python 2.7, we recommend that you install Python through Homebrew.</span></span> <span data-ttu-id="4c6e1-147">Lásd: [Python telepítése Mac OS x](http://docs.python-guide.org/en/latest/starting/install/osx/).</span><span class="sxs-lookup"><span data-stu-id="4c6e1-147">See [Installing Python on Mac OS X](http://docs.python-guide.org/en/latest/starting/install/osx/).</span></span>

<span data-ttu-id="4c6e1-148">Telepítse a Python és a pip hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="4c6e1-148">Install Python and pip by running hello following command:</span></span>

```bash
brew install python
```

## <a name="install-hello-azure-cli"></a><span data-ttu-id="4c6e1-149">Hello Azure parancssori felület telepítése</span><span class="sxs-lookup"><span data-stu-id="4c6e1-149">Install hello Azure CLI</span></span>

<span data-ttu-id="4c6e1-150">tooinstall hello Azure CLI használata esetén kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="4c6e1-150">tooinstall hello Azure CLI, follow these steps:</span></span>

1. <span data-ttu-id="4c6e1-151">Futtassa a következő parancsok hello terminálban hello:</span><span class="sxs-lookup"><span data-stu-id="4c6e1-151">Run hello following commands in hello terminal:</span></span>
   ```bash
   pip install --upgrade azure-cli
   pip install --upgrade azure-cli-iot
   ```
   <span data-ttu-id="4c6e1-152">hello telepítési 5 percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="4c6e1-152">hello installation might take 5 minutes.</span></span>

2. <span data-ttu-id="4c6e1-153">Ellenőrizze a hello telepítést hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="4c6e1-153">Verify hello installation by running hello following command:</span></span>
   ```bash
   az iot -h
   ```
   <span data-ttu-id="4c6e1-154">Meg kell jelennie a hello parancskimenet sikeres hello telepítés esetén.</span><span class="sxs-lookup"><span data-stu-id="4c6e1-154">You should see hello following output if hello installation is successful.</span></span>

   ![Az Azure parancssori felület telepítésének ellenőrzése](media/iot-hub-gateway-kit-lessons/lesson2/az_iot_help_osx.png)

## <a name="install-visual-studio-code"></a><span data-ttu-id="4c6e1-156">Visual Studio Code telepítése</span><span class="sxs-lookup"><span data-stu-id="4c6e1-156">Install Visual Studio Code</span></span>

<span data-ttu-id="4c6e1-157">Visual Studio Code később használható hello oktatóanyag tooedit konfigurációs fájlok.</span><span class="sxs-lookup"><span data-stu-id="4c6e1-157">You use Visual Studio Code later in hello tutorial tooedit configuration files.</span></span>

<span data-ttu-id="4c6e1-158">[Töltse le](https://code.visualstudio.com/docs/setup/osx) és a Visual Studio Code telepítése.</span><span class="sxs-lookup"><span data-stu-id="4c6e1-158">[Download](https://code.visualstudio.com/docs/setup/osx) and install Visual Studio Code.</span></span>

## <a name="summary"></a><span data-ttu-id="4c6e1-159">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="4c6e1-159">Summary</span></span>

<span data-ttu-id="4c6e1-160">A Mac számítógépen telepített összes hello szükséges eszközök és szoftverek.</span><span class="sxs-lookup"><span data-stu-id="4c6e1-160">You’ve installed all hello required tools and software on your Mac computer.</span></span> <span data-ttu-id="4c6e1-161">A következő feladathoz toouse hello Azure CLI toocreate egy IoT-központot, és regisztrálja az eszközt az IoT hub.</span><span class="sxs-lookup"><span data-stu-id="4c6e1-161">Your next task is toouse hello Azure CLI toocreate an IoT hub and register your device in your IoT hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4c6e1-162">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4c6e1-162">Next steps</span></span>
[<span data-ttu-id="4c6e1-163">Az IoT-központ létrehozni és regisztrálni az eszközt</span><span class="sxs-lookup"><span data-stu-id="4c6e1-163">Create an IoT hub and register Device</span></span>](iot-hub-gateway-kit-c-sim-lesson2-register-device.md)
