---
title: "SensorTag eszköz & Azure IoT átjáró - rész 2: eszközök (Ubuntu) beszerzése |} Microsoft Docs"
description: "Az eszközök és a szoftver telepítése a gazdagép Ubuntu rendszert futtató számítógépen, létrehoz egy IoT-központot, és regisztrálja az eszközt az IoT-központ az."
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
ms.openlocfilehash: 234b60e1f8eaff52ce07f54d4d12de2421cc1a52
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-the-tools-ubuntu-1604"></a><span data-ttu-id="35fcc-104">Eszközök beszerzése (Ubuntu 16.04)</span><span class="sxs-lookup"><span data-stu-id="35fcc-104">Get the tools (Ubuntu 16.04)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="35fcc-105">Windows 7 vagy újabb</span><span class="sxs-lookup"><span data-stu-id="35fcc-105">Windows 7 or later</span></span>](iot-hub-gateway-kit-c-lesson2-get-the-tools-win32.md)
> * [<span data-ttu-id="35fcc-106">Ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="35fcc-106">Ubuntu 16.04</span></span>](iot-hub-gateway-kit-c-lesson2-get-the-tools-ubuntu.md)
> * [<span data-ttu-id="35fcc-107">macOS 10.10</span><span class="sxs-lookup"><span data-stu-id="35fcc-107">macOS 10.10</span></span>](iot-hub-gateway-kit-c-lesson2-get-the-tools-mac.md)

## <a name="what-you-will-do"></a><span data-ttu-id="35fcc-108">Mit fog</span><span class="sxs-lookup"><span data-stu-id="35fcc-108">What you will do</span></span>

- <span data-ttu-id="35fcc-109">Telepítse a Git, Node.js, Gulp, Python.</span><span class="sxs-lookup"><span data-stu-id="35fcc-109">Install Git, Node.js, Gulp, Python.</span></span>
- <span data-ttu-id="35fcc-110">Telepítse az Azure parancssori felület (CLI).</span><span class="sxs-lookup"><span data-stu-id="35fcc-110">Install the Azure command-line interface (Azure CLI).</span></span> 

<span data-ttu-id="35fcc-111">Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási](iot-hub-gateway-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="35fcc-111">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-gateway-kit-c-troubleshooting.md).</span></span>
## <a name="what-you-will-learn"></a><span data-ttu-id="35fcc-112">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="35fcc-112">What you will learn</span></span>

<span data-ttu-id="35fcc-113">Ez a lecke témák köre:</span><span class="sxs-lookup"><span data-stu-id="35fcc-113">In this lesson, you will learn:</span></span>

- <span data-ttu-id="35fcc-114">Hogyan kell telepíteni a Git és Node.js.</span><span class="sxs-lookup"><span data-stu-id="35fcc-114">How to install Git and Node.js.</span></span>
  - <span data-ttu-id="35fcc-115">Git egy nyílt forráskódú elosztott verziókezelő rendszer.</span><span class="sxs-lookup"><span data-stu-id="35fcc-115">Git is an open source distributed version control system.</span></span> <span data-ttu-id="35fcc-116">Ez a lecke a mintaalkalmazás Git tárolja.</span><span class="sxs-lookup"><span data-stu-id="35fcc-116">The sample application for this lesson is stored on Git.</span></span>
  - <span data-ttu-id="35fcc-117">NODE.js a JavaScript futásidejű és gazdag csomag-ökoszisztéma.</span><span class="sxs-lookup"><span data-stu-id="35fcc-117">Node.js is a JavaScript runtime with a rich package ecosystem.</span></span>
- <span data-ttu-id="35fcc-118">Hogyan Node.js fejlesztői eszközök telepítése az NPM segítségével.</span><span class="sxs-lookup"><span data-stu-id="35fcc-118">How to use NPM to install Node.js development tools.</span></span>
  - <span data-ttu-id="35fcc-119">A Node.js minimálisan szükséges verziója a 4.5-ös LTS.</span><span class="sxs-lookup"><span data-stu-id="35fcc-119">The minimum required version of Node.js is 4.5 LTS.</span></span>
  - <span data-ttu-id="35fcc-120">NPM egyike a Node.js csomag feletteseit.</span><span class="sxs-lookup"><span data-stu-id="35fcc-120">NPM is one of the package managers for Node.js.</span></span>
- <span data-ttu-id="35fcc-121">Hogyan kell telepíteni a Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="35fcc-121">How to install Visual Studio Code.</span></span>
  - <span data-ttu-id="35fcc-122">A Visual Studio Code egy többplatformos, a Windows, Linux és macOS egyszerűsített, de hatékony forráskód szerkesztőjében.</span><span class="sxs-lookup"><span data-stu-id="35fcc-122">Visual Studio Code is a cross platform, lightweight but powerful source code editor for Windows, Linux, and macOS.</span></span> <span data-ttu-id="35fcc-123">Nagyszerű hibakeresés, beágyazott Git-vezérlő, szintaxis kiemelését, intelligens kód befejezési, kódtöredékek, és támogatása kód újrabontása is rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="35fcc-123">It has great support for debugging, embedded Git control, syntax highlighting, intelligent code completion, snippets, and code refactoring as well.</span></span>
- <span data-ttu-id="35fcc-124">Az Azure parancssori felület telepítése</span><span class="sxs-lookup"><span data-stu-id="35fcc-124">How to install the Azure CLI</span></span>
  - <span data-ttu-id="35fcc-125">Az Azure parancssori felület olyan többplatformos parancssori környezetet biztosít az Azure.</span><span class="sxs-lookup"><span data-stu-id="35fcc-125">The Azure CLI provides a multiplatform command-line experience for Azure.</span></span> <span data-ttu-id="35fcc-126">Közvetlenül a parancssorból kiépítését működik, és kezelheti az erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="35fcc-126">You work directly from a command line to provision and manage resources.</span></span>
- <span data-ttu-id="35fcc-127">Hogyan lehet az Azure CLI segítségével létrehoz egy IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="35fcc-127">How to use the Azure CLI to create an IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="35fcc-128">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="35fcc-128">What you need</span></span>

- <span data-ttu-id="35fcc-129">Az eszközök és szoftverek letöltése az internethez.</span><span class="sxs-lookup"><span data-stu-id="35fcc-129">An Internet connection to download the tools and software.</span></span>
- <span data-ttu-id="35fcc-130">Ubuntu 16.04 vagy újabb rendszerrel működő számítógép.</span><span class="sxs-lookup"><span data-stu-id="35fcc-130">A computer that is running Ubuntu 16.04 or later.</span></span>

## <a name="install-git-and-nodejs"></a><span data-ttu-id="35fcc-131">Telepítse a Git szoftver, Node.js</span><span class="sxs-lookup"><span data-stu-id="35fcc-131">Install Git and Node.js</span></span>

<span data-ttu-id="35fcc-132">Telepítse a Git szoftver, Node.js, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="35fcc-132">To install Git and Node.js, follow these steps:</span></span>

1. <span data-ttu-id="35fcc-133">Nyomja le az `Ctrl + Alt + T` egy terminált megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="35fcc-133">Press `Ctrl + Alt + T` to open a terminal.</span></span>
2. <span data-ttu-id="35fcc-134">Futtassa az alábbi parancsot:</span><span class="sxs-lookup"><span data-stu-id="35fcc-134">Run the following commands:</span></span>

   ```bash
   sudo apt-get update
   curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
   sudo apt-get install -y nodejs
   sudo apt-get install git
   ```

## <a name="install-nodejs-development-tools"></a><span data-ttu-id="35fcc-135">Node.js fejlesztői eszközök telepítése</span><span class="sxs-lookup"><span data-stu-id="35fcc-135">Install Node.js development tools</span></span>

<span data-ttu-id="35fcc-136">Használhat [gulp.js](http://gulpjs.com/) automatikus központi telepítési és a parancsprogramok végrehajtását.</span><span class="sxs-lookup"><span data-stu-id="35fcc-136">You use [gulp.js](http://gulpjs.com/) to automate deployment and execution of scripts.</span></span>

<span data-ttu-id="35fcc-137">Gulp telepítéséhez futtassa a terminálban a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="35fcc-137">To install gulp, run the following command in the terminal:</span></span>

```bash
sudo npm install -g gulp
```

<span data-ttu-id="35fcc-138">Ha a telepítési problémákat tapasztal, tekintse meg a [hibaelhárítási útmutatója](iot-hub-gateway-kit-c-troubleshooting.md) gyakori problémák megoldásainak.</span><span class="sxs-lookup"><span data-stu-id="35fcc-138">If you experience issues with the installation, see the [troubleshooting guide](iot-hub-gateway-kit-c-troubleshooting.md) for solutions to common problems.</span></span>

> [!Note]
> <span data-ttu-id="35fcc-139">Csomópont, NPM és Gulp Node.js létre automatizálási parancsfájlokat futtatásához szükséges.</span><span class="sxs-lookup"><span data-stu-id="35fcc-139">Node, NPM and Gulp are required to run automation scripts developed in Node.js.</span></span>

## <a name="install-the-azure-cli"></a><span data-ttu-id="35fcc-140">Telepítse az Azure CLI-t</span><span class="sxs-lookup"><span data-stu-id="35fcc-140">Install the Azure CLI</span></span>

<span data-ttu-id="35fcc-141">Az Azure parancssori felület telepítéséhez kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="35fcc-141">To install the Azure CLI, follow these steps:</span></span>

1. <span data-ttu-id="35fcc-142">A következő parancsokat a terminálban:</span><span class="sxs-lookup"><span data-stu-id="35fcc-142">Run the following commands in the terminal:</span></span>

   ```bash
   sudo apt-get update
   sudo apt-get install -y libssl-dev libffi-dev
   sudo apt-get install -y python-dev
   sudo apt-get install -y build-essential
   sudo apt-get install -y python-pip
   sudo pip install --upgrade azure-cli
   sudo pip install --upgrade azure-cli-iot
   ```

   <span data-ttu-id="35fcc-143">A telepítés 5 percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="35fcc-143">The installation might take 5 minutes.</span></span>

2. <span data-ttu-id="35fcc-144">Ellenőrizze a telepítést a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="35fcc-144">Verify the installation by running the following command:</span></span>

   ```bash
   az iot -h
   ```
<span data-ttu-id="35fcc-145">Ha a telepítés sikerült a következő kimenetet kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="35fcc-145">You should see the following output if the installation is successful.</span></span>
<span data-ttu-id="35fcc-146">![Az Azure parancssori felület telepítésének ellenőrzése](media/iot-hub-gateway-kit-lessons/lesson2/az_iot_help_ubuntu.png)</span><span class="sxs-lookup"><span data-stu-id="35fcc-146">![Verify Azure CLI installation](media/iot-hub-gateway-kit-lessons/lesson2/az_iot_help_ubuntu.png)</span></span>

### <a name="install-visual-studio-code"></a><span data-ttu-id="35fcc-147">Visual Studio Code telepítése</span><span class="sxs-lookup"><span data-stu-id="35fcc-147">Install Visual Studio Code</span></span>

<span data-ttu-id="35fcc-148">Visual Studio Code az oktatóanyag későbbi részében, konfigurációs fájlok szerkesztésére használhatja.</span><span class="sxs-lookup"><span data-stu-id="35fcc-148">You use Visual Studio Code later in the tutorial to edit configuration files.</span></span>

<span data-ttu-id="35fcc-149">[Töltse le](https://code.visualstudio.com/docs/setup/linux) és a Visual Studio Code telepítése.</span><span class="sxs-lookup"><span data-stu-id="35fcc-149">[Download](https://code.visualstudio.com/docs/setup/linux) and install Visual Studio Code.</span></span>

## <a name="summary"></a><span data-ttu-id="35fcc-150">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="35fcc-150">Summary</span></span>

<span data-ttu-id="35fcc-151">Telepítette a szükséges eszközök és a szoftver a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="35fcc-151">You've installed all the required tools and software on your host computer.</span></span> <span data-ttu-id="35fcc-152">A következő feladathoz az Azure parancssori felület használatával létrehoz egy IoT-központot, és regisztrálja az eszközt az IoT hub.</span><span class="sxs-lookup"><span data-stu-id="35fcc-152">Your next task is to use the Azure CLI to create an IoT hub and register your device in your IoT hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="35fcc-153">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="35fcc-153">Next steps</span></span>
[<span data-ttu-id="35fcc-154">IoT Hub létrehozása és az eszköz regisztrálása</span><span class="sxs-lookup"><span data-stu-id="35fcc-154">Create an IoT hub and register your device</span></span>](iot-hub-gateway-kit-c-lesson2-register-device.md)
