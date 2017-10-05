---
title: "Az Azure IoT - lecke 1 Connect Raspberry pi (C): eszközök (macOS) beszerzése |} Microsoft Docs"
description: "Töltse le és telepítse a szükséges eszközöket és a szoftver az első mintaalkalmazás pi macOS."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "az IOT-fejlesztési, iot szoftver, internet dolgot szoftver, a mac, futtassa a telepítési csomópont js mac gulp telepítés git"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started
ms.assetid: fc6bd2c8-a847-4bf5-818f-6f7f9a6835ee
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 64db77040ef3482f213bd622320fdb5df243fa93
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-the-tools-macos-1010"></a><span data-ttu-id="137f9-104">Eszközök beszerzése (macOS 10.10)</span><span class="sxs-lookup"><span data-stu-id="137f9-104">Get the tools (macOS 10.10)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="137f9-105">Windows 7 vagy újabb</span><span class="sxs-lookup"><span data-stu-id="137f9-105">Windows 7 or later</span></span>](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-win32.md)
> * [<span data-ttu-id="137f9-106">Ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="137f9-106">Ubuntu 16.04</span></span>](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-ubuntu.md)
> * [<span data-ttu-id="137f9-107">macOS 10.10</span><span class="sxs-lookup"><span data-stu-id="137f9-107">macOS 10.10</span></span>](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-mac.md)

## <a name="what-you-will-do"></a><span data-ttu-id="137f9-108">Mit fog</span><span class="sxs-lookup"><span data-stu-id="137f9-108">What you will do</span></span>
<span data-ttu-id="137f9-109">Töltse le a fejlesztői eszközök és a szoftver a málna Pi 3 első minta alkalmazásához.</span><span class="sxs-lookup"><span data-stu-id="137f9-109">Download the development tools and the software for the first sample application for your Raspberry Pi 3.</span></span> <span data-ttu-id="137f9-110">Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="137f9-110">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span></span>

> [!NOTE]
> <span data-ttu-id="137f9-111">Bár a programozási nyelv, a fő logikájának C, Node.js eszközök használata a megszerzett az eszközök, és felépítéséhez és minta alkalmazások központi telepítése.</span><span class="sxs-lookup"><span data-stu-id="137f9-111">Although the programming language of the main logic is C, Node.js tools are used in the lessons to discover devices, and build and deploy sample applications.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="137f9-112">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="137f9-112">What you will learn</span></span>
<span data-ttu-id="137f9-113">Ebből a cikkből megtudhatja:</span><span class="sxs-lookup"><span data-stu-id="137f9-113">In this article, you will learn:</span></span>

* <span data-ttu-id="137f9-114">Hogyan kell telepíteni a Git és Node.js.</span><span class="sxs-lookup"><span data-stu-id="137f9-114">How to install Git and Node.js.</span></span>
  * <span data-ttu-id="137f9-115">[Git](https://git-scm.com) van egy nyílt forráskódú elosztott verziókezelő rendszer.</span><span class="sxs-lookup"><span data-stu-id="137f9-115">[Git](https://git-scm.com) is an open source distributed version control system.</span></span> <span data-ttu-id="137f9-116">Ez a cikk a mintaalkalmazás Git tárolja.</span><span class="sxs-lookup"><span data-stu-id="137f9-116">The sample application for this article is stored on Git.</span></span>
  * <span data-ttu-id="137f9-117">[NODE.js](https://nodejs.org/en/) van a JavaScript futásidejű és gazdag csomag-ökoszisztéma.</span><span class="sxs-lookup"><span data-stu-id="137f9-117">[Node.js](https://nodejs.org/en/) is a JavaScript runtime with a rich package ecosystem.</span></span>
* <span data-ttu-id="137f9-118">Hogyan további Node.js fejlesztői eszközök telepítése az NPM segítségével.</span><span class="sxs-lookup"><span data-stu-id="137f9-118">How to use NPM to install additional Node.js development tools.</span></span>
  * <span data-ttu-id="137f9-119">A Node.js minimálisan szükséges verziója a 4.5-ös LTS.</span><span class="sxs-lookup"><span data-stu-id="137f9-119">The minimum required version of Node.js is 4.5 LTS.</span></span>
  * <span data-ttu-id="137f9-120">[NPM](https://www.npmjs.com) a csomag kezelők, a Node.js egyike.</span><span class="sxs-lookup"><span data-stu-id="137f9-120">[NPM](https://www.npmjs.com) is one of the package managers for Node.js.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="137f9-121">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="137f9-121">What you need</span></span>
<span data-ttu-id="137f9-122">A művelet elvégzéséhez szüksége lesz:</span><span class="sxs-lookup"><span data-stu-id="137f9-122">To complete this operation, you will need:</span></span>

* <span data-ttu-id="137f9-123">A fejlesztői eszközök és a szoftverfrissítések letöltése az internethez.</span><span class="sxs-lookup"><span data-stu-id="137f9-123">An Internet connection to download the development tools and the software.</span></span>
* <span data-ttu-id="137f9-124">MacOS Yosemite (10.10) futtató Mac vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="137f9-124">A Mac that is running macOS Yosemite (10.10) or later.</span></span>

## <a name="install-git-and-nodejs"></a><span data-ttu-id="137f9-125">Telepítse a Git szoftver, Node.js</span><span class="sxs-lookup"><span data-stu-id="137f9-125">Install Git and Node.js</span></span>
<span data-ttu-id="137f9-126">A Git szoftver, Node.js telepítéséhez használja a [Homebrew](http://brew.sh) segédprogramja csomag a következő lépések végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="137f9-126">To install Git and Node.js, use the [Homebrew](http://brew.sh) package management utility by following these steps:</span></span>

1. <span data-ttu-id="137f9-127">Telepítse a Homebrew.</span><span class="sxs-lookup"><span data-stu-id="137f9-127">Install Homebrew.</span></span> <span data-ttu-id="137f9-128">Ha már telepített Homebrew, folytassa a 2.</span><span class="sxs-lookup"><span data-stu-id="137f9-128">If you've already installed Homebrew, go to step 2.</span></span>
   
   1. <span data-ttu-id="137f9-129">Nyomja le az `Cmd + Space` , és írja be `Terminal` egy terminált megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="137f9-129">Press `Cmd + Space` and enter `Terminal` to open a terminal.</span></span>
   2. <span data-ttu-id="137f9-130">Futtassa az alábbi parancsot:</span><span class="sxs-lookup"><span data-stu-id="137f9-130">Run the following command:</span></span>
      
      ```bash
      /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
      ```
2. <span data-ttu-id="137f9-131">Telepítse a Git és a Node.js, a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="137f9-131">Install Git and Node.js by running the following command:</span></span>
   
   ```bash
   brew install node git
   ```

## <a name="install-additional-nodejs-development-tools"></a><span data-ttu-id="137f9-132">További Node.js fejlesztői eszközök telepítése</span><span class="sxs-lookup"><span data-stu-id="137f9-132">Install additional Node.js development tools</span></span>
<span data-ttu-id="137f9-133">Használjon [gulp.js](http://gulpjs.com) a mintaalkalmazást a Pi való telepítésének automatizálásához.</span><span class="sxs-lookup"><span data-stu-id="137f9-133">Use [gulp.js](http://gulpjs.com) to automate the deployment of the sample application to your Pi.</span></span> <span data-ttu-id="137f9-134">Használja a [cli-eszköz-felderítési](https://github.com/Azure/device-discovery-cli) az IoT-eszközök hálózati adatainak lekérésére.</span><span class="sxs-lookup"><span data-stu-id="137f9-134">Use the [device-discovery-cli](https://github.com/Azure/device-discovery-cli) to retrieve network information about your IoT devices.</span></span>

<span data-ttu-id="137f9-135">Telepítés `gulp` és `device-discovery-cli` a terminálban a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="137f9-135">Install `gulp` and `device-discovery-cli` by running the following command in the terminal:</span></span>

```bash
sudo npm install -g device-discovery-cli gulp
```

<span data-ttu-id="137f9-136">Ha problémák macOS Node.js és a további fejlesztői eszközök telepítése, lásd: a [hibaelhárítási útmutatója](iot-hub-raspberry-pi-kit-c-troubleshooting.md) gyakori problémák megoldásainak.</span><span class="sxs-lookup"><span data-stu-id="137f9-136">If you experience issues installing Node.js and these additional development tools on macOS, see the [troubleshooting guide](iot-hub-raspberry-pi-kit-c-troubleshooting.md) for solutions to common problems.</span></span>

## <a name="install-visual-studio-code"></a><span data-ttu-id="137f9-137">Visual Studio Code telepítése</span><span class="sxs-lookup"><span data-stu-id="137f9-137">Install Visual Studio Code</span></span>
<span data-ttu-id="137f9-138">[Töltse le](https://code.visualstudio.com/docs/setup/osx) és a Visual Studio Code telepítése.</span><span class="sxs-lookup"><span data-stu-id="137f9-138">[Download](https://code.visualstudio.com/docs/setup/osx) and install Visual Studio Code.</span></span> <span data-ttu-id="137f9-139">A Visual Studio Code a Windows, Linux és macOS egy egyszerűsített, de hatékony forráskód szerkesztőjében.</span><span class="sxs-lookup"><span data-stu-id="137f9-139">Visual Studio Code is a lightweight but powerful source code editor for Windows, Linux, and macOS.</span></span> <span data-ttu-id="137f9-140">A mintakód szerkesztése a szerkesztő használata az oktatóanyag későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="137f9-140">You use this editor later in the tutorial to edit the sample code.</span></span>

## <a name="summary"></a><span data-ttu-id="137f9-141">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="137f9-141">Summary</span></span>
<span data-ttu-id="137f9-142">A szükséges fejlesztői eszközök és az első mintaalkalmazás szoftver telepítése.</span><span class="sxs-lookup"><span data-stu-id="137f9-142">You've installed the required development tools and software for the first sample application.</span></span> <span data-ttu-id="137f9-143">A következő feladat létrehozásához, telepítéséhez és futtassa a mintaalkalmazást a Pi-hoz.</span><span class="sxs-lookup"><span data-stu-id="137f9-143">The next task is to create, deploy, and run the sample application on Pi.</span></span>

## <a name="next-steps"></a><span data-ttu-id="137f9-144">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="137f9-144">Next steps</span></span>
[<span data-ttu-id="137f9-145">A villogóalkalmazás elkészítése és üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="137f9-145">Create and deploy the blink application</span></span>](iot-hub-raspberry-pi-kit-c-lesson1-deploy-blink-app.md)

