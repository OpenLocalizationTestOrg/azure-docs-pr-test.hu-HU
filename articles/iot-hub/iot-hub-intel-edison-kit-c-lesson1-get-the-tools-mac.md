---
title: "Az Azure IoT - lecke 1 Connect Intel Edison (C): eszközök (macOS) beszerzése |} Microsoft Docs"
description: "Töltse le és telepítse a szükséges eszközök és szoftverek Edison első minta alkalmazásához macOS."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "arduino fejlesztőeszközök, iot fejlesztési, iot szoftver, internet dolgot szoftver, telepítse git mac, telepítés csomópont js mac gépen"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-c-get-started
ms.assetid: 3f764f2e-25fa-4dde-9e8d-d278453fccfd
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 27939f731121522f688e606052492bda8ae045fe
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-the-tools-macos-1010"></a><span data-ttu-id="5ec45-104">Eszközök beszerzése (macOS 10.10)</span><span class="sxs-lookup"><span data-stu-id="5ec45-104">Get the tools (macOS 10.10)</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="5ec45-105">[Windows 7 vagy újabb][windows]</span><span class="sxs-lookup"><span data-stu-id="5ec45-105">[Windows 7 or later][windows]</span></span>
> * <span data-ttu-id="5ec45-106">[Ubuntu 16.04][ubuntu]</span><span class="sxs-lookup"><span data-stu-id="5ec45-106">[Ubuntu 16.04][ubuntu]</span></span>
> * <span data-ttu-id="5ec45-107">[macOS 10.10][macos]</span><span class="sxs-lookup"><span data-stu-id="5ec45-107">[macOS 10.10][macos]</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="5ec45-108">Mit fog</span><span class="sxs-lookup"><span data-stu-id="5ec45-108">What you will do</span></span>
<span data-ttu-id="5ec45-109">Töltse le a fejlesztői eszközök és a szoftver az Intel Edison első minta alkalmazásához.</span><span class="sxs-lookup"><span data-stu-id="5ec45-109">Download the development tools and the software for the first sample application for your Intel Edison.</span></span> <span data-ttu-id="5ec45-110">Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="5ec45-110">If you have any problems, look for solutions on the [troubleshooting page][troubleshooting].</span></span>

> [!NOTE]
> <span data-ttu-id="5ec45-111">Bár a programozási nyelv, a fő logikájának C, Node.js eszközök szerepelnek a megszerzett létrehozásához és központi telepítéséhez alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="5ec45-111">Although the programming language of the main logic is C, Node.js tools are used in the lessons to build and deploy sample applications.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="5ec45-112">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="5ec45-112">What you will learn</span></span>
<span data-ttu-id="5ec45-113">Ebből a cikkből megtudhatja:</span><span class="sxs-lookup"><span data-stu-id="5ec45-113">In this article, you will learn:</span></span>

* <span data-ttu-id="5ec45-114">Hogyan kell telepíteni a Git és Node.js.</span><span class="sxs-lookup"><span data-stu-id="5ec45-114">How to install Git and Node.js.</span></span>
  * <span data-ttu-id="5ec45-115">[Git](https://git-scm.com) van egy nyílt forráskódú elosztott verziókezelő rendszer.</span><span class="sxs-lookup"><span data-stu-id="5ec45-115">[Git](https://git-scm.com) is an open source distributed version control system.</span></span> <span data-ttu-id="5ec45-116">Ez a cikk a mintaalkalmazás Git tárolja.</span><span class="sxs-lookup"><span data-stu-id="5ec45-116">The sample application for this article is stored on Git.</span></span>
  * <span data-ttu-id="5ec45-117">[NODE.js](https://nodejs.org/en/) van a JavaScript futásidejű és gazdag csomag-ökoszisztéma.</span><span class="sxs-lookup"><span data-stu-id="5ec45-117">[Node.js](https://nodejs.org/en/) is a JavaScript runtime with a rich package ecosystem.</span></span>
* <span data-ttu-id="5ec45-118">Hogyan további Node.js fejlesztői eszközök telepítése az NPM segítségével.</span><span class="sxs-lookup"><span data-stu-id="5ec45-118">How to use NPM to install additional Node.js development tools.</span></span>
  * <span data-ttu-id="5ec45-119">A Node.js minimálisan szükséges verziója a 4.5-ös LTS.</span><span class="sxs-lookup"><span data-stu-id="5ec45-119">The minimum required version of Node.js is 4.5 LTS.</span></span>
  * <span data-ttu-id="5ec45-120">[NPM](https://www.npmjs.com) a csomag kezelők, a Node.js egyike.</span><span class="sxs-lookup"><span data-stu-id="5ec45-120">[NPM](https://www.npmjs.com) is one of the package managers for Node.js.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="5ec45-121">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="5ec45-121">What you need</span></span>
<span data-ttu-id="5ec45-122">A művelet elvégzéséhez szüksége lesz:</span><span class="sxs-lookup"><span data-stu-id="5ec45-122">To complete this operation, you will need:</span></span>
* <span data-ttu-id="5ec45-123">A fejlesztői eszközök és a szoftverfrissítések letöltése az internethez.</span><span class="sxs-lookup"><span data-stu-id="5ec45-123">An Internet connection to download the development tools and the software.</span></span>
* <span data-ttu-id="5ec45-124">MacOS Yosemite (10.10) futtató Mac vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="5ec45-124">A Mac that is running macOS Yosemite (10.10) or later.</span></span>

## <a name="install-git-and-nodejs"></a><span data-ttu-id="5ec45-125">Telepítse a Git szoftver, Node.js</span><span class="sxs-lookup"><span data-stu-id="5ec45-125">Install Git and Node.js</span></span>
<span data-ttu-id="5ec45-126">A Git szoftver, Node.js telepítéséhez használja a [Homebrew](http://brew.sh) segédprogramja csomag a következő lépések végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="5ec45-126">To install Git and Node.js, use the [Homebrew](http://brew.sh) package management utility by following these steps:</span></span>

1. <span data-ttu-id="5ec45-127">Telepítse a Homebrew.</span><span class="sxs-lookup"><span data-stu-id="5ec45-127">Install Homebrew.</span></span> <span data-ttu-id="5ec45-128">Ha már telepített Homebrew, folytassa a 2.</span><span class="sxs-lookup"><span data-stu-id="5ec45-128">If you've already installed Homebrew, go to step 2.</span></span>

   1. <span data-ttu-id="5ec45-129">Nyomja le az `Cmd + Space` , és írja be `Terminal` egy terminált megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="5ec45-129">Press `Cmd + Space` and enter `Terminal` to open a terminal.</span></span>
   2. <span data-ttu-id="5ec45-130">Futtassa az alábbi parancsot:</span><span class="sxs-lookup"><span data-stu-id="5ec45-130">Run the following command:</span></span>

      ```bash
      /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
      ```
2. <span data-ttu-id="5ec45-131">Telepítse a Git és a Node.js, a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="5ec45-131">Install Git and Node.js by running the following command:</span></span>

   ```bash
   brew install node git
   ```

## <a name="install-additional-nodejs-development-tools"></a><span data-ttu-id="5ec45-132">További Node.js fejlesztői eszközök telepítése</span><span class="sxs-lookup"><span data-stu-id="5ec45-132">Install additional Node.js development tools</span></span>
<span data-ttu-id="5ec45-133">Használjon [gulp.js](http://gulpjs.com) a Edison a minta-alkalmazás központi telepítésének automatizálásához.</span><span class="sxs-lookup"><span data-stu-id="5ec45-133">Use [gulp.js](http://gulpjs.com) to automate the deployment of the sample application to your Edison.</span></span>

<span data-ttu-id="5ec45-134">Telepítse `gulp` a terminálban a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="5ec45-134">Install `gulp` by running the following command in the terminal:</span></span>

```bash
sudo npm install -g gulp
```

<span data-ttu-id="5ec45-135">Ha problémák macOS Node.js és a további fejlesztői eszközök telepítése, lásd: a [hibaelhárítási útmutatója] [ troubleshooting] gyakori problémák megoldásainak.</span><span class="sxs-lookup"><span data-stu-id="5ec45-135">If you experience issues installing Node.js and these additional development tools on macOS, see the [troubleshooting guide][troubleshooting] for solutions to common problems.</span></span>

## <a name="install-visual-studio-code"></a><span data-ttu-id="5ec45-136">Visual Studio Code telepítése</span><span class="sxs-lookup"><span data-stu-id="5ec45-136">Install Visual Studio Code</span></span>
<span data-ttu-id="5ec45-137">[Töltse le](https://code.visualstudio.com/docs/setup/osx) és a Visual Studio Code telepítése.</span><span class="sxs-lookup"><span data-stu-id="5ec45-137">[Download](https://code.visualstudio.com/docs/setup/osx) and install Visual Studio Code.</span></span> <span data-ttu-id="5ec45-138">A Visual Studio Code a Windows, Linux és macOS egy egyszerűsített, de hatékony forráskód szerkesztőjében.</span><span class="sxs-lookup"><span data-stu-id="5ec45-138">Visual Studio Code is a lightweight but powerful source code editor for Windows, Linux, and macOS.</span></span> <span data-ttu-id="5ec45-139">A mintakód szerkesztése a szerkesztő használata az oktatóanyag későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="5ec45-139">You use this editor later in the tutorial to edit the sample code.</span></span>

## <a name="summary"></a><span data-ttu-id="5ec45-140">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="5ec45-140">Summary</span></span>
<span data-ttu-id="5ec45-141">A szükséges fejlesztői eszközök és az első mintaalkalmazás szoftver telepítése.</span><span class="sxs-lookup"><span data-stu-id="5ec45-141">You've installed the required development tools and software for the first sample application.</span></span> <span data-ttu-id="5ec45-142">A következő feladata a létrehozásához, telepítéséhez és a Edison futtassa a mintaalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="5ec45-142">The next task is to create, deploy, and run the sample application on Edison.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5ec45-143">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5ec45-143">Next steps</span></span>
<span data-ttu-id="5ec45-144">[A villogási alkalmazás létrehozását és telepítését][create-and-deploy-the-blink-application]</span><span class="sxs-lookup"><span data-stu-id="5ec45-144">[Create and deploy the blink application][create-and-deploy-the-blink-application]</span></span>
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md
[create-and-deploy-the-blink-application]: iot-hub-intel-edison-kit-c-lesson1-deploy-blink-app.md
[windows]: iot-hub-intel-edison-kit-c-lesson1-get-the-tools-win32.md
[ubuntu]: iot-hub-intel-edison-kit-c-lesson1-get-the-tools-ubuntu.md
[macos]: iot-hub-intel-edison-kit-c-lesson1-get-the-tools-mac.md
