---
title: "Csatlakozás Azure IoT - lecke 1 málna Pi (csomópont): (macOS) eszközök beszerzése |} Microsoft Docs"
description: "Töltse le és telepítse a szükséges eszközöket és a szoftver az első mintaalkalmazás pi macOS."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "az IOT-fejlesztési, iot szoftver, internet dolgot szoftver, python mac telepítése, telepítse a git a mac, Futtatás gulp, csomópont js mac telepítése"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: 2ea6d211-c0e8-4ade-ac69-d1c2261d78c4
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 6c2338baa8e88bab4c4be64568220f53178943cd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-the-tools-macos-1010"></a><span data-ttu-id="a5075-104">Eszközök beszerzése (macOS 10.10)</span><span class="sxs-lookup"><span data-stu-id="a5075-104">Get the tools (macOS 10.10)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="a5075-105">Windows 7 vagy újabb</span><span class="sxs-lookup"><span data-stu-id="a5075-105">Windows 7 or later</span></span>](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)
> * [<span data-ttu-id="a5075-106">Ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="a5075-106">Ubuntu 16.04</span></span>](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-ubuntu.md)
> * [<span data-ttu-id="a5075-107">macOS 10.10</span><span class="sxs-lookup"><span data-stu-id="a5075-107">macOS 10.10</span></span>](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-mac.md)

## <a name="what-you-will-do"></a><span data-ttu-id="a5075-108">Mit fog</span><span class="sxs-lookup"><span data-stu-id="a5075-108">What you will do</span></span>
<span data-ttu-id="a5075-109">Töltse le a fejlesztői eszközök és a szoftver a málna Pi 3 első minta alkalmazásához.</span><span class="sxs-lookup"><span data-stu-id="a5075-109">Download the development tools and the software for the first sample application for your Raspberry Pi 3.</span></span> <span data-ttu-id="a5075-110">Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="a5075-110">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="a5075-111">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="a5075-111">What you will learn</span></span>
<span data-ttu-id="a5075-112">Ebből a cikkből megtudhatja:</span><span class="sxs-lookup"><span data-stu-id="a5075-112">In this article, you will learn:</span></span>

* <span data-ttu-id="a5075-113">Hogyan kell telepíteni a Git és Node.js.</span><span class="sxs-lookup"><span data-stu-id="a5075-113">How to install Git and Node.js.</span></span>
  * <span data-ttu-id="a5075-114">[Git](https://git-scm.com) van egy nyílt forráskódú elosztott verziókezelő rendszer.</span><span class="sxs-lookup"><span data-stu-id="a5075-114">[Git](https://git-scm.com) is an open source distributed version control system.</span></span> <span data-ttu-id="a5075-115">Ez a cikk a mintaalkalmazás Git tárolja.</span><span class="sxs-lookup"><span data-stu-id="a5075-115">The sample application for this article is stored on Git.</span></span>
  * <span data-ttu-id="a5075-116">[NODE.js](https://nodejs.org/en/) van a JavaScript futásidejű és gazdag csomag-ökoszisztéma.</span><span class="sxs-lookup"><span data-stu-id="a5075-116">[Node.js](https://nodejs.org/en/) is a JavaScript runtime with a rich package ecosystem.</span></span>
* <span data-ttu-id="a5075-117">Hogyan további Node.js fejlesztői eszközök telepítése az NPM segítségével.</span><span class="sxs-lookup"><span data-stu-id="a5075-117">How to use NPM to install additional Node.js development tools.</span></span>
  * <span data-ttu-id="a5075-118">A Node.js minimálisan szükséges verziója a 4.5-ös LTS.</span><span class="sxs-lookup"><span data-stu-id="a5075-118">The minimum required version of Node.js is 4.5 LTS.</span></span>
  * <span data-ttu-id="a5075-119">[NPM](https://www.npmjs.com) a csomag kezelők, a Node.js egyike.</span><span class="sxs-lookup"><span data-stu-id="a5075-119">[NPM](https://www.npmjs.com) is one of the package managers for Node.js.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="a5075-120">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="a5075-120">What you need</span></span>
<span data-ttu-id="a5075-121">A művelet elvégzéséhez szüksége lesz:</span><span class="sxs-lookup"><span data-stu-id="a5075-121">To complete this operation, you will need:</span></span>

* <span data-ttu-id="a5075-122">A fejlesztői eszközök és a szoftverfrissítések letöltése az internethez.</span><span class="sxs-lookup"><span data-stu-id="a5075-122">An Internet connection to download the development tools and the software.</span></span>
* <span data-ttu-id="a5075-123">MacOS Yosemite (10.10) futtató Mac vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="a5075-123">A Mac that is running macOS Yosemite (10.10) or later.</span></span>

## <a name="install-git-and-nodejs"></a><span data-ttu-id="a5075-124">Telepítse a Git szoftver, Node.js</span><span class="sxs-lookup"><span data-stu-id="a5075-124">Install Git and Node.js</span></span>
<span data-ttu-id="a5075-125">A Git szoftver, Node.js telepítéséhez használja a [Homebrew](http://brew.sh) segédprogramja csomag a következő lépések végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="a5075-125">To install Git and Node.js, use the [Homebrew](http://brew.sh) package management utility by following these steps:</span></span>

1. <span data-ttu-id="a5075-126">Telepítse a Homebrew.</span><span class="sxs-lookup"><span data-stu-id="a5075-126">Install Homebrew.</span></span> <span data-ttu-id="a5075-127">Ha már telepített Homebrew, folytassa a 2.</span><span class="sxs-lookup"><span data-stu-id="a5075-127">If you've already installed Homebrew, go to step 2.</span></span>
   
   1. <span data-ttu-id="a5075-128">Nyomja le az `Cmd + Space` , és írja be `Terminal` egy terminált megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="a5075-128">Press `Cmd + Space` and enter `Terminal` to open a terminal.</span></span>
   2. <span data-ttu-id="a5075-129">Futtassa az alábbi parancsot:</span><span class="sxs-lookup"><span data-stu-id="a5075-129">Run the following command:</span></span>
      
      ```bash
      /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
      ```
2. <span data-ttu-id="a5075-130">Telepítse a Git és a Node.js, a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="a5075-130">Install Git and Node.js by running the following command:</span></span>
   
   ```bash
   brew install node git
   ```

## <a name="install-additional-nodejs-development-tools"></a><span data-ttu-id="a5075-131">További Node.js fejlesztői eszközök telepítése</span><span class="sxs-lookup"><span data-stu-id="a5075-131">Install additional Node.js development tools</span></span>
<span data-ttu-id="a5075-132">Használjon [gulp.js](http://gulpjs.com) Pi a minta-alkalmazás központi telepítésének automatizálásához.</span><span class="sxs-lookup"><span data-stu-id="a5075-132">Use [gulp.js](http://gulpjs.com) to automate the deployment of the sample application to Pi.</span></span> <span data-ttu-id="a5075-133">Használja a [cli-eszköz-felderítési](https://github.com/Azure/device-discovery-cli) az IoT-eszközök hálózati adatainak lekérésére.</span><span class="sxs-lookup"><span data-stu-id="a5075-133">Use the [device-discovery-cli](https://github.com/Azure/device-discovery-cli) to retrieve network information about your IoT devices.</span></span>

<span data-ttu-id="a5075-134">Telepítés `gulp` és `device-discovery-cli` a terminálban a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="a5075-134">Install `gulp` and `device-discovery-cli` by running the following command in the terminal:</span></span>

```bash
npm install -g device-discovery-cli gulp
```

<span data-ttu-id="a5075-135">Ha problémák macOS Node.js és a további fejlesztői eszközök telepítése, lásd: a [hibaelhárítási útmutatója](iot-hub-raspberry-pi-kit-node-troubleshooting.md) gyakori problémák megoldásainak.</span><span class="sxs-lookup"><span data-stu-id="a5075-135">If you experience issues installing Node.js and these additional development tools on macOS, see the [troubleshooting guide](iot-hub-raspberry-pi-kit-node-troubleshooting.md) for solutions to common problems.</span></span>

## <a name="install-visual-studio-code"></a><span data-ttu-id="a5075-136">Visual Studio Code telepítése</span><span class="sxs-lookup"><span data-stu-id="a5075-136">Install Visual Studio Code</span></span>
<span data-ttu-id="a5075-137">[Töltse le](https://code.visualstudio.com/docs/setup/osx) és a Visual Studio Code telepítése.</span><span class="sxs-lookup"><span data-stu-id="a5075-137">[Download](https://code.visualstudio.com/docs/setup/osx) and install Visual Studio Code.</span></span> <span data-ttu-id="a5075-138">A Visual Studio Code a Windows, Linux és macOS egy egyszerűsített, de hatékony forráskód szerkesztőjében.</span><span class="sxs-lookup"><span data-stu-id="a5075-138">Visual Studio Code is a lightweight but powerful source code editor for Windows, Linux, and macOS.</span></span> <span data-ttu-id="a5075-139">A mintakód szerkesztése a szerkesztő használata az oktatóanyag későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="a5075-139">You use this editor later in the tutorial to edit the sample code.</span></span>

## <a name="summary"></a><span data-ttu-id="a5075-140">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="a5075-140">Summary</span></span>
<span data-ttu-id="a5075-141">A szükséges fejlesztői eszközök és az első mintaalkalmazás szoftver telepítése.</span><span class="sxs-lookup"><span data-stu-id="a5075-141">You've installed the required development tools and software for the first sample application.</span></span> <span data-ttu-id="a5075-142">A következő feladat létrehozásához, telepítéséhez és futtassa a mintaalkalmazást a Pi-hoz.</span><span class="sxs-lookup"><span data-stu-id="a5075-142">The next task is to create, deploy, and run the sample application on Pi.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a5075-143">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a5075-143">Next steps</span></span>
[<span data-ttu-id="a5075-144">Létrehozhat és telepíthet a villogási mintaalkalmazás</span><span class="sxs-lookup"><span data-stu-id="a5075-144">Create and deploy the blink sample application</span></span>](iot-hub-raspberry-pi-kit-node-lesson1-deploy-blink-app.md)

