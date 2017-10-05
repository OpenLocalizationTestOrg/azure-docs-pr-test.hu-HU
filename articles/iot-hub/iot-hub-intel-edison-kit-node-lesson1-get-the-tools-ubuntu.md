---
title: "Csatlakozás Azure IoT - lecke 1 Intel Edison (csomópont): (Ubuntu) eszközök beszerzése |} Microsoft Docs"
description: "Töltse le és telepítse a szükséges eszközök és szoftverek Edison első minta alkalmazásához Ubuntu."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "arduino fejlesztői eszközök, az iot-fejlesztési, iot szoftver, internet dolgot szoftver, az ubuntu, telepítés csomópont js ubuntu telepítés git"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-node-get-started
ms.assetid: 9ab5b161-7ec5-41a6-9c5f-4456e4882752
ms.service: iot-hub
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 74c5f06c2b12d140814bfb75125d60b83addf70c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-the-tools-ubuntu-1604"></a><span data-ttu-id="e836d-104">Eszközök beszerzése (Ubuntu 16.04)</span><span class="sxs-lookup"><span data-stu-id="e836d-104">Get the tools (Ubuntu 16.04)</span></span>

> [!div class="op_single_selector"]
> * <span data-ttu-id="e836d-105">[Windows 7 vagy újabb][windows]</span><span class="sxs-lookup"><span data-stu-id="e836d-105">[Windows 7 or later][windows]</span></span>
> * <span data-ttu-id="e836d-106">[Ubuntu 16.04][ubuntu]</span><span class="sxs-lookup"><span data-stu-id="e836d-106">[Ubuntu 16.04][ubuntu]</span></span>
> * <span data-ttu-id="e836d-107">[macOS 10.10][macos]</span><span class="sxs-lookup"><span data-stu-id="e836d-107">[macOS 10.10][macos]</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="e836d-108">Mit fog</span><span class="sxs-lookup"><span data-stu-id="e836d-108">What you will do</span></span>
<span data-ttu-id="e836d-109">Töltse le a fejlesztői eszközök és a szoftver az Intel Edison első minta alkalmazásához.</span><span class="sxs-lookup"><span data-stu-id="e836d-109">Download the development tools and the software for the first sample application for your Intel Edison.</span></span> <span data-ttu-id="e836d-110">Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="e836d-110">If you have any problems, look for solutions on the [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="e836d-111">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="e836d-111">What you will learn</span></span>
<span data-ttu-id="e836d-112">Ebből a cikkből megtudhatja:</span><span class="sxs-lookup"><span data-stu-id="e836d-112">In this article, you will learn:</span></span>

* <span data-ttu-id="e836d-113">A Git szoftver, Node.js telepítése</span><span class="sxs-lookup"><span data-stu-id="e836d-113">How to install Git and Node.js</span></span>
  * <span data-ttu-id="e836d-114">[Git](https://git-scm.com) van egy nyílt forráskódú elosztott verziókezelő rendszer.</span><span class="sxs-lookup"><span data-stu-id="e836d-114">[Git](https://git-scm.com) is an open source distributed version control system.</span></span> <span data-ttu-id="e836d-115">Ez a cikk a mintaalkalmazás Git tárolja.</span><span class="sxs-lookup"><span data-stu-id="e836d-115">The sample application for this article is stored on Git.</span></span>
  * <span data-ttu-id="e836d-116">[NODE.js](https://nodejs.org/en/) van a JavaScript futásidejű és gazdag csomag-ökoszisztéma.</span><span class="sxs-lookup"><span data-stu-id="e836d-116">[Node.js](https://nodejs.org/en/) is a JavaScript runtime with a rich package ecosystem.</span></span>
* <span data-ttu-id="e836d-117">Hogyan további Node.js fejlesztői eszközök telepítése az NPM segítségével.</span><span class="sxs-lookup"><span data-stu-id="e836d-117">How to use NPM to install additional Node.js development tools.</span></span>
  * <span data-ttu-id="e836d-118">A Node.js minimálisan szükséges verziója a 4.5-ös LTS.</span><span class="sxs-lookup"><span data-stu-id="e836d-118">The minimum required version of Node.js is 4.5 LTS.</span></span>
  * <span data-ttu-id="e836d-119">[NPM](https://www.npmjs.com) a csomag kezelők, a Node.js egyike.</span><span class="sxs-lookup"><span data-stu-id="e836d-119">[NPM](https://www.npmjs.com) is one of the package managers for Node.js.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="e836d-120">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="e836d-120">What you need</span></span>
<span data-ttu-id="e836d-121">A művelet elvégzéséhez szüksége lesz:</span><span class="sxs-lookup"><span data-stu-id="e836d-121">To complete this operation, you will need:</span></span>
* <span data-ttu-id="e836d-122">A fejlesztői eszközök és a szoftverfrissítések letöltése az internethez.</span><span class="sxs-lookup"><span data-stu-id="e836d-122">An Internet connection to download the development tools and the software.</span></span>
* <span data-ttu-id="e836d-123">Ubuntu 16.04 vagy újabb rendszerrel működő számítógép.</span><span class="sxs-lookup"><span data-stu-id="e836d-123">A computer that is running Ubuntu 16.04 or later.</span></span>

## <a name="install-git-nodejs-and-npm"></a><span data-ttu-id="e836d-124">Telepítse a Git, Node.js és NPM</span><span class="sxs-lookup"><span data-stu-id="e836d-124">Install Git, Node.js, and NPM</span></span>
<span data-ttu-id="e836d-125">Használja a billentyűparancsot `Ctrl + Alt + T` nyisson meg egy terminált, és futtassa a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="e836d-125">Use the keyboard shortcut `Ctrl + Alt + T` to open a terminal and run the following commands:</span></span>

```bash
sudo apt-get update
curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
sudo apt-get install -y nodejs
sudo apt-get install git
```

## <a name="install-additional-nodejs-development-tools"></a><span data-ttu-id="e836d-126">További Node.js fejlesztői eszközök telepítése</span><span class="sxs-lookup"><span data-stu-id="e836d-126">Install additional Node.js development tools</span></span>
<span data-ttu-id="e836d-127">Használjon [gulp.js](http://gulpjs.com) Edison a minta-alkalmazás központi telepítésének automatizálásához.</span><span class="sxs-lookup"><span data-stu-id="e836d-127">Use [gulp.js](http://gulpjs.com) to automate the deployment of the sample application to Edison.</span></span>

<span data-ttu-id="e836d-128">Telepítse `gulp` a terminálban a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="e836d-128">Install `gulp` by running the following command in the terminal:</span></span>

```bash
sudo npm install -g gulp
```

<span data-ttu-id="e836d-129">Ha problémák Ubuntu Node.js és a további fejlesztői eszközök telepítése, lásd: a [hibaelhárítási útmutatója] [ troubleshooting] gyakori problémák megoldásainak.</span><span class="sxs-lookup"><span data-stu-id="e836d-129">If you experience issues installing Node.js and these additional development tools on Ubuntu, see the [troubleshooting guide][troubleshooting] for solutions to common problems.</span></span>

## <a name="install-visual-studio-code"></a><span data-ttu-id="e836d-130">Visual Studio Code telepítése</span><span class="sxs-lookup"><span data-stu-id="e836d-130">Install Visual Studio Code</span></span>
<span data-ttu-id="e836d-131">[Töltse le](https://code.visualstudio.com/docs/setup/linux) és a Visual Studio Code telepítése.</span><span class="sxs-lookup"><span data-stu-id="e836d-131">[Download](https://code.visualstudio.com/docs/setup/linux) and install Visual Studio Code.</span></span> <span data-ttu-id="e836d-132">A Visual Studio Code a Windows, Linux és macOS egy egyszerűsített, de hatékony forráskód szerkesztőjében.</span><span class="sxs-lookup"><span data-stu-id="e836d-132">Visual Studio Code is a lightweight but powerful source code editor for Windows, Linux, and macOS.</span></span> <span data-ttu-id="e836d-133">A mintakód szerkesztése a szerkesztő használata az oktatóanyag későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="e836d-133">You use this editor later in the tutorial to edit the sample code.</span></span>

## <a name="summary"></a><span data-ttu-id="e836d-134">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="e836d-134">Summary</span></span>
<span data-ttu-id="e836d-135">A szükséges fejlesztői eszközök és az első mintaalkalmazás szoftver telepítése.</span><span class="sxs-lookup"><span data-stu-id="e836d-135">You've installed the required development tools and software for the first sample application.</span></span> <span data-ttu-id="e836d-136">A következő feladata a létrehozásához, telepítéséhez és a Edison futtassa a mintaalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="e836d-136">The next task is to create, deploy, and run the sample application on Edison.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e836d-137">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e836d-137">Next steps</span></span>
<span data-ttu-id="e836d-138">[Létrehozhat és telepíthet a villogási mintaalkalmazás][create-and-deploy-the-blink-application]</span><span class="sxs-lookup"><span data-stu-id="e836d-138">[Create and deploy the blink sample application][create-and-deploy-the-blink-application]</span></span>
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[create-and-deploy-the-blink-application]: iot-hub-intel-edison-kit-node-lesson1-deploy-blink-app.md
[windows]: iot-hub-intel-edison-kit-node-lesson1-get-the-tools-win32.md
[ubuntu]: iot-hub-intel-edison-kit-node-lesson1-get-the-tools-ubuntu.md
[macos]: iot-hub-intel-edison-kit-node-lesson1-get-the-tools-mac.md
