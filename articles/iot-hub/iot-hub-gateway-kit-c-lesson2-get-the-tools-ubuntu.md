---
title: "SensorTag eszköz & Azure IoT átjáró - rész 2: eszközök (Ubuntu) beszerzése |} Microsoft Docs"
description: "Hello eszközeinek és hello szoftver telepítése a gazdagép Ubuntu rendszert futtató számítógépen, létrehoz egy IoT-központot, és az eszközt regisztrálni kell az IoT-központ hello."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "az IOT-fejlesztési, iot szoftver, iot felhőalapú szolgáltatás, dolgot szoftvert, az azure CLI-t, az eszközök internetes git telepíthető ubuntu, Futtatás gulp, csomópont js ubuntu telepítése"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: 0bac1412-385b-4255-a33f-9d44c35feb3e
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: c9edca91e791ef914b1920178b66eadd12ae0281
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-tools-ubuntu-1604"></a><span data-ttu-id="1ee84-104">Hello eszközök (Ubuntu 16.04) beolvasása</span><span class="sxs-lookup"><span data-stu-id="1ee84-104">Get hello tools (Ubuntu 16.04)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="1ee84-105">Windows 7 vagy újabb</span><span class="sxs-lookup"><span data-stu-id="1ee84-105">Windows 7 or later</span></span>](iot-hub-gateway-kit-c-lesson2-get-the-tools-win32.md)
> * [<span data-ttu-id="1ee84-106">Ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="1ee84-106">Ubuntu 16.04</span></span>](iot-hub-gateway-kit-c-lesson2-get-the-tools-ubuntu.md)
> * [<span data-ttu-id="1ee84-107">macOS 10.10</span><span class="sxs-lookup"><span data-stu-id="1ee84-107">macOS 10.10</span></span>](iot-hub-gateway-kit-c-lesson2-get-the-tools-mac.md)

## <a name="what-you-will-do"></a><span data-ttu-id="1ee84-108">Mit fog</span><span class="sxs-lookup"><span data-stu-id="1ee84-108">What you will do</span></span>

- <span data-ttu-id="1ee84-109">Telepítse a Git, Node.js, Gulp, Python.</span><span class="sxs-lookup"><span data-stu-id="1ee84-109">Install Git, Node.js, Gulp, Python.</span></span>
- <span data-ttu-id="1ee84-110">Hello Azure parancssori felület (CLI) telepítése.</span><span class="sxs-lookup"><span data-stu-id="1ee84-110">Install hello Azure command-line interface (Azure CLI).</span></span> 

<span data-ttu-id="1ee84-111">Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási](iot-hub-gateway-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="1ee84-111">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-gateway-kit-c-troubleshooting.md).</span></span>
## <a name="what-you-will-learn"></a><span data-ttu-id="1ee84-112">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="1ee84-112">What you will learn</span></span>

<span data-ttu-id="1ee84-113">Ez a lecke témák köre:</span><span class="sxs-lookup"><span data-stu-id="1ee84-113">In this lesson, you will learn:</span></span>

- <span data-ttu-id="1ee84-114">Hogyan tooinstall a Git szoftver, Node.js.</span><span class="sxs-lookup"><span data-stu-id="1ee84-114">How tooinstall Git and Node.js.</span></span>
  - <span data-ttu-id="1ee84-115">Git egy nyílt forráskódú elosztott verziókezelő rendszer.</span><span class="sxs-lookup"><span data-stu-id="1ee84-115">Git is an open source distributed version control system.</span></span> <span data-ttu-id="1ee84-116">Ez a lecke hello-mintaalkalmazás Git tárolja.</span><span class="sxs-lookup"><span data-stu-id="1ee84-116">hello sample application for this lesson is stored on Git.</span></span>
  - <span data-ttu-id="1ee84-117">NODE.js a JavaScript futásidejű és gazdag csomag-ökoszisztéma.</span><span class="sxs-lookup"><span data-stu-id="1ee84-117">Node.js is a JavaScript runtime with a rich package ecosystem.</span></span>
- <span data-ttu-id="1ee84-118">Hogyan toouse NPM tooinstall Node.js fejlesztői eszközök.</span><span class="sxs-lookup"><span data-stu-id="1ee84-118">How toouse NPM tooinstall Node.js development tools.</span></span>
  - <span data-ttu-id="1ee84-119">hello minimálisan szükséges verziója Node.js 4.5-ös LTS.</span><span class="sxs-lookup"><span data-stu-id="1ee84-119">hello minimum required version of Node.js is 4.5 LTS.</span></span>
  - <span data-ttu-id="1ee84-120">NPM hello csomag kezelők a Node.js egyike.</span><span class="sxs-lookup"><span data-stu-id="1ee84-120">NPM is one of hello package managers for Node.js.</span></span>
- <span data-ttu-id="1ee84-121">Hogyan tooinstall Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="1ee84-121">How tooinstall Visual Studio Code.</span></span>
  - <span data-ttu-id="1ee84-122">A Visual Studio Code egy többplatformos, a Windows, Linux és macOS egyszerűsített, de hatékony forráskód szerkesztőjében.</span><span class="sxs-lookup"><span data-stu-id="1ee84-122">Visual Studio Code is a cross platform, lightweight but powerful source code editor for Windows, Linux, and macOS.</span></span> <span data-ttu-id="1ee84-123">Nagyszerű hibakeresés, beágyazott Git-vezérlő, szintaxis kiemelését, intelligens kód befejezési, kódtöredékek, és támogatása kód újrabontása is rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="1ee84-123">It has great support for debugging, embedded Git control, syntax highlighting, intelligent code completion, snippets, and code refactoring as well.</span></span>
- <span data-ttu-id="1ee84-124">Hogyan tooinstall hello Azure parancssori felület</span><span class="sxs-lookup"><span data-stu-id="1ee84-124">How tooinstall hello Azure CLI</span></span>
  - <span data-ttu-id="1ee84-125">hello Azure CLI olyan többplatformos parancssori környezetet biztosít az Azure.</span><span class="sxs-lookup"><span data-stu-id="1ee84-125">hello Azure CLI provides a multiplatform command-line experience for Azure.</span></span> <span data-ttu-id="1ee84-126">Közvetlenül egy parancssor tooprovision dolgozhassanak és kezelheti az erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="1ee84-126">You work directly from a command line tooprovision and manage resources.</span></span>
- <span data-ttu-id="1ee84-127">Hogyan toouse hello Azure CLI toocreate egy IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="1ee84-127">How toouse hello Azure CLI toocreate an IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="1ee84-128">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="1ee84-128">What you need</span></span>

- <span data-ttu-id="1ee84-129">Az Internet kapcsolat toodownload hello eszközök és szoftverek.</span><span class="sxs-lookup"><span data-stu-id="1ee84-129">An Internet connection toodownload hello tools and software.</span></span>
- <span data-ttu-id="1ee84-130">Ubuntu 16.04 vagy újabb rendszerrel működő számítógép.</span><span class="sxs-lookup"><span data-stu-id="1ee84-130">A computer that is running Ubuntu 16.04 or later.</span></span>

## <a name="install-git-and-nodejs"></a><span data-ttu-id="1ee84-131">Telepítse a Git szoftver, Node.js</span><span class="sxs-lookup"><span data-stu-id="1ee84-131">Install Git and Node.js</span></span>

<span data-ttu-id="1ee84-132">tooinstall Git és a Node.js, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="1ee84-132">tooinstall Git and Node.js, follow these steps:</span></span>

1. <span data-ttu-id="1ee84-133">Nyomja le az `Ctrl + Alt + T` tooopen egy terminált.</span><span class="sxs-lookup"><span data-stu-id="1ee84-133">Press `Ctrl + Alt + T` tooopen a terminal.</span></span>
2. <span data-ttu-id="1ee84-134">Futtassa a következő parancsok hello:</span><span class="sxs-lookup"><span data-stu-id="1ee84-134">Run hello following commands:</span></span>

   ```bash
   sudo apt-get update
   curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
   sudo apt-get install -y nodejs
   sudo apt-get install git
   ```

## <a name="install-nodejs-development-tools"></a><span data-ttu-id="1ee84-135">Node.js fejlesztői eszközök telepítése</span><span class="sxs-lookup"><span data-stu-id="1ee84-135">Install Node.js development tools</span></span>

<span data-ttu-id="1ee84-136">Használhat [gulp.js](http://gulpjs.com/) tooautomate üzembe helyezési és parancsprogramok végrehajtását.</span><span class="sxs-lookup"><span data-stu-id="1ee84-136">You use [gulp.js](http://gulpjs.com/) tooautomate deployment and execution of scripts.</span></span>

<span data-ttu-id="1ee84-137">tooinstall gulp, futtassa a következő parancs hello terminálban hello:</span><span class="sxs-lookup"><span data-stu-id="1ee84-137">tooinstall gulp, run hello following command in hello terminal:</span></span>

```bash
sudo npm install -g gulp
```

<span data-ttu-id="1ee84-138">Hello telepítési problémákat tapasztal, tekintse meg a hello [hibaelhárítási útmutatója](iot-hub-gateway-kit-c-troubleshooting.md) a megoldások toocommon problémákat.</span><span class="sxs-lookup"><span data-stu-id="1ee84-138">If you experience issues with hello installation, see hello [troubleshooting guide](iot-hub-gateway-kit-c-troubleshooting.md) for solutions toocommon problems.</span></span>

> [!Note]
> <span data-ttu-id="1ee84-139">Csomópont, NPM és Gulp a node.js fejlesztett szükséges toorun automatizálási parancsfájlokat.</span><span class="sxs-lookup"><span data-stu-id="1ee84-139">Node, NPM and Gulp are required toorun automation scripts developed in Node.js.</span></span>

## <a name="install-hello-azure-cli"></a><span data-ttu-id="1ee84-140">Hello Azure parancssori felület telepítése</span><span class="sxs-lookup"><span data-stu-id="1ee84-140">Install hello Azure CLI</span></span>

<span data-ttu-id="1ee84-141">tooinstall hello Azure CLI használata esetén kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="1ee84-141">tooinstall hello Azure CLI, follow these steps:</span></span>

1. <span data-ttu-id="1ee84-142">Futtassa a következő parancsok hello terminálban hello:</span><span class="sxs-lookup"><span data-stu-id="1ee84-142">Run hello following commands in hello terminal:</span></span>

   ```bash
   sudo apt-get update
   sudo apt-get install -y libssl-dev libffi-dev
   sudo apt-get install -y python-dev
   sudo apt-get install -y build-essential
   sudo apt-get install -y python-pip
   sudo pip install --upgrade azure-cli
   sudo pip install --upgrade azure-cli-iot
   ```

   <span data-ttu-id="1ee84-143">hello telepítési 5 percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="1ee84-143">hello installation might take 5 minutes.</span></span>

2. <span data-ttu-id="1ee84-144">Ellenőrizze a hello telepítést hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="1ee84-144">Verify hello installation by running hello following command:</span></span>

   ```bash
   az iot -h
   ```
<span data-ttu-id="1ee84-145">Meg kell jelennie a hello parancskimenet sikeres hello telepítés esetén.</span><span class="sxs-lookup"><span data-stu-id="1ee84-145">You should see hello following output if hello installation is successful.</span></span>
<span data-ttu-id="1ee84-146">![Az Azure parancssori felület telepítésének ellenőrzése](media/iot-hub-gateway-kit-lessons/lesson2/az_iot_help_ubuntu.png)</span><span class="sxs-lookup"><span data-stu-id="1ee84-146">![Verify Azure CLI installation](media/iot-hub-gateway-kit-lessons/lesson2/az_iot_help_ubuntu.png)</span></span>

### <a name="install-visual-studio-code"></a><span data-ttu-id="1ee84-147">Visual Studio Code telepítése</span><span class="sxs-lookup"><span data-stu-id="1ee84-147">Install Visual Studio Code</span></span>

<span data-ttu-id="1ee84-148">Visual Studio Code később használható hello oktatóanyag tooedit konfigurációs fájlok.</span><span class="sxs-lookup"><span data-stu-id="1ee84-148">You use Visual Studio Code later in hello tutorial tooedit configuration files.</span></span>

<span data-ttu-id="1ee84-149">[Töltse le](https://code.visualstudio.com/docs/setup/linux) és a Visual Studio Code telepítése.</span><span class="sxs-lookup"><span data-stu-id="1ee84-149">[Download](https://code.visualstudio.com/docs/setup/linux) and install Visual Studio Code.</span></span>

## <a name="summary"></a><span data-ttu-id="1ee84-150">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="1ee84-150">Summary</span></span>

<span data-ttu-id="1ee84-151">A gazdaszámítógépen telepített összes hello szükséges eszközök és szoftverek.</span><span class="sxs-lookup"><span data-stu-id="1ee84-151">You've installed all hello required tools and software on your host computer.</span></span> <span data-ttu-id="1ee84-152">A következő feladathoz toouse hello Azure CLI toocreate egy IoT-központot, és regisztrálja az eszközt az IoT hub.</span><span class="sxs-lookup"><span data-stu-id="1ee84-152">Your next task is toouse hello Azure CLI toocreate an IoT hub and register your device in your IoT hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1ee84-153">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1ee84-153">Next steps</span></span>
[<span data-ttu-id="1ee84-154">IoT Hub létrehozása és az eszköz regisztrálása</span><span class="sxs-lookup"><span data-stu-id="1ee84-154">Create an IoT hub and register your device</span></span>](iot-hub-gateway-kit-c-lesson2-register-device.md)
