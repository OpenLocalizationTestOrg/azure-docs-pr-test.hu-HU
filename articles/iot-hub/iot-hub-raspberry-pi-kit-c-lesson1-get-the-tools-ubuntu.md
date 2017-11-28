---
title: "Az Azure IoT - lecke 1 Connect Raspberry pi (C): eszközök (Ubuntu) beszerzése |} Microsoft Docs"
description: "Töltse le és telepítse a szükséges eszközöket és a szoftver az első mintaalkalmazás pi Ubuntu."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "az IOT-fejlesztési, iot szoftverek, internet dolgot szoftver, telepítse a git az ubuntu, Futtatás gulp, csomópont js ubuntu telepítése"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started
ms.assetid: 32cfea00-c254-4cef-8f6f-bbf807eca6b6
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 28ebba82e90d91470518cd830c96e6da39d8b9b4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-the-tools-ubuntu-1604"></a><span data-ttu-id="e8ade-104">Eszközök beszerzése (Ubuntu 16.04)</span><span class="sxs-lookup"><span data-stu-id="e8ade-104">Get the tools (Ubuntu 16.04)</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="e8ade-105">Windows 7 vagy újabb</span><span class="sxs-lookup"><span data-stu-id="e8ade-105">Windows 7 or later</span></span>](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-win32.md)
> * [<span data-ttu-id="e8ade-106">Ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="e8ade-106">Ubuntu 16.04</span></span>](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-ubuntu.md)
> * [<span data-ttu-id="e8ade-107">macOS 10.10</span><span class="sxs-lookup"><span data-stu-id="e8ade-107">macOS 10.10</span></span>](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-mac.md)

## <a name="what-you-will-do"></a><span data-ttu-id="e8ade-108">Mit fog</span><span class="sxs-lookup"><span data-stu-id="e8ade-108">What you will do</span></span>
<span data-ttu-id="e8ade-109">Töltse le a fejlesztői eszközök és a szoftver a málna Pi 3 első minta alkalmazásához.</span><span class="sxs-lookup"><span data-stu-id="e8ade-109">Download the development tools and the software for the first sample application for your Raspberry Pi 3.</span></span> <span data-ttu-id="e8ade-110">Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="e8ade-110">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span></span>

> [!NOTE]
> <span data-ttu-id="e8ade-111">Bár a programozási nyelv, a fő logikájának C, Node.js eszközök használata a megszerzett az eszközök, és felépítéséhez és minta alkalmazások központi telepítése.</span><span class="sxs-lookup"><span data-stu-id="e8ade-111">Although the programming language of the main logic is C, Node.js tools are used in the lessons to discover devices, and build and deploy sample applications.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="e8ade-112">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="e8ade-112">What you will learn</span></span>
<span data-ttu-id="e8ade-113">Ebből a cikkből megtudhatja:</span><span class="sxs-lookup"><span data-stu-id="e8ade-113">In this article, you will learn:</span></span>

* <span data-ttu-id="e8ade-114">A Git szoftver, Node.js telepítése</span><span class="sxs-lookup"><span data-stu-id="e8ade-114">How to install Git and Node.js</span></span>
  * <span data-ttu-id="e8ade-115">[Git](https://git-scm.com) van egy nyílt forráskódú elosztott verziókezelő rendszer.</span><span class="sxs-lookup"><span data-stu-id="e8ade-115">[Git](https://git-scm.com) is an open source distributed version control system.</span></span> <span data-ttu-id="e8ade-116">Ez a cikk a mintaalkalmazás Git tárolja.</span><span class="sxs-lookup"><span data-stu-id="e8ade-116">The sample application for this article is stored on Git.</span></span>
  * <span data-ttu-id="e8ade-117">[NODE.js](https://nodejs.org/en/) van a JavaScript futásidejű és gazdag csomag-ökoszisztéma.</span><span class="sxs-lookup"><span data-stu-id="e8ade-117">[Node.js](https://nodejs.org/en/) is a JavaScript runtime with a rich package ecosystem.</span></span>
* <span data-ttu-id="e8ade-118">Hogyan további Node.js fejlesztői eszközök telepítése az NPM segítségével.</span><span class="sxs-lookup"><span data-stu-id="e8ade-118">How to use NPM to install additional Node.js development tools.</span></span>
  * <span data-ttu-id="e8ade-119">A Node.js minimálisan szükséges verziója a 4.5-ös LTS.</span><span class="sxs-lookup"><span data-stu-id="e8ade-119">The minimum required version of Node.js is 4.5 LTS.</span></span>
  * <span data-ttu-id="e8ade-120">[NPM](https://www.npmjs.com) a csomag kezelők, a Node.js egyike.</span><span class="sxs-lookup"><span data-stu-id="e8ade-120">[NPM](https://www.npmjs.com) is one of the package managers for Node.js.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="e8ade-121">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="e8ade-121">What you need</span></span>
<span data-ttu-id="e8ade-122">A művelet elvégzéséhez szüksége lesz:</span><span class="sxs-lookup"><span data-stu-id="e8ade-122">To complete this operation, you will need:</span></span>

* <span data-ttu-id="e8ade-123">A fejlesztői eszközök és a szoftverfrissítések letöltése az internethez.</span><span class="sxs-lookup"><span data-stu-id="e8ade-123">An Internet connection to download the development tools and the software.</span></span>
* <span data-ttu-id="e8ade-124">Ubuntu 16.04 vagy újabb rendszerrel működő számítógép.</span><span class="sxs-lookup"><span data-stu-id="e8ade-124">A computer that is running Ubuntu 16.04 or later.</span></span>

## <a name="install-git-nodejs-and-npm"></a><span data-ttu-id="e8ade-125">Telepítse a Git, Node.js és NPM</span><span class="sxs-lookup"><span data-stu-id="e8ade-125">Install Git, Node.js, and NPM</span></span>
<span data-ttu-id="e8ade-126">Használja a billentyűparancsot `Ctrl + Alt + T` nyisson meg egy terminált, és futtassa a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="e8ade-126">Use the keyboard shortcut `Ctrl + Alt + T` to open a terminal and run the following commands:</span></span>

```bash
sudo apt-get update
curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
sudo apt-get install -y nodejs
sudo apt-get install git
```

## <a name="install-additional-nodejs-development-tools"></a><span data-ttu-id="e8ade-127">További Node.js fejlesztői eszközök telepítése</span><span class="sxs-lookup"><span data-stu-id="e8ade-127">Install additional Node.js development tools</span></span>
<span data-ttu-id="e8ade-128">Használjon [gulp.js](http://gulpjs.com) Pi a minta-alkalmazás központi telepítésének automatizálásához.</span><span class="sxs-lookup"><span data-stu-id="e8ade-128">Use [gulp.js](http://gulpjs.com) to automate the deployment of the sample application to Pi.</span></span> <span data-ttu-id="e8ade-129">Használja a [cli-eszköz-felderítési](https://github.com/Azure/device-discovery-cli) az IoT-eszközök hálózati adatainak lekérésére.</span><span class="sxs-lookup"><span data-stu-id="e8ade-129">Use the [device-discovery-cli](https://github.com/Azure/device-discovery-cli) to retrieve network information about your IoT devices.</span></span>

<span data-ttu-id="e8ade-130">Telepítés `gulp` és `device-discovery-cli` a terminálban a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="e8ade-130">Install `gulp` and `device-discovery-cli` by running the following command in the terminal:</span></span>

```bash
sudo npm install -g device-discovery-cli gulp
```

<span data-ttu-id="e8ade-131">Ha problémák Ubuntu Node.js és a további fejlesztői eszközök telepítése, lásd: a [hibaelhárítási útmutatója](iot-hub-raspberry-pi-kit-c-troubleshooting.md) gyakori problémák megoldásainak.</span><span class="sxs-lookup"><span data-stu-id="e8ade-131">If you experience issues installing Node.js and these additional development tools on Ubuntu, see the [troubleshooting guide](iot-hub-raspberry-pi-kit-c-troubleshooting.md) for solutions to common problems.</span></span>

## <a name="install-visual-studio-code"></a><span data-ttu-id="e8ade-132">Visual Studio Code telepítése</span><span class="sxs-lookup"><span data-stu-id="e8ade-132">Install Visual Studio Code</span></span>
<span data-ttu-id="e8ade-133">[Töltse le](https://code.visualstudio.com/docs/setup/linux) és a Visual Studio Code telepítése.</span><span class="sxs-lookup"><span data-stu-id="e8ade-133">[Download](https://code.visualstudio.com/docs/setup/linux) and install Visual Studio Code.</span></span> <span data-ttu-id="e8ade-134">A Visual Studio Code a Windows, Linux és macOS egy egyszerűsített, de hatékony forráskód szerkesztőjében.</span><span class="sxs-lookup"><span data-stu-id="e8ade-134">Visual Studio Code is a lightweight but powerful source code editor for Windows, Linux, and macOS.</span></span> <span data-ttu-id="e8ade-135">A mintakód szerkesztése a szerkesztő használata az oktatóanyag későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="e8ade-135">You use this editor later in the tutorial to edit the sample code.</span></span>

## <a name="summary"></a><span data-ttu-id="e8ade-136">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="e8ade-136">Summary</span></span>
<span data-ttu-id="e8ade-137">A szükséges fejlesztői eszközök és az első mintaalkalmazás szoftver telepítése.</span><span class="sxs-lookup"><span data-stu-id="e8ade-137">You've installed the required development tools and software for the first sample application.</span></span> <span data-ttu-id="e8ade-138">A következő feladat létrehozásához, telepítéséhez és futtassa a mintaalkalmazást a Pi-hoz.</span><span class="sxs-lookup"><span data-stu-id="e8ade-138">The next task is to create, deploy, and run the sample application on Pi.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e8ade-139">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e8ade-139">Next steps</span></span>
[<span data-ttu-id="e8ade-140">A villogóalkalmazás elkészítése és üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="e8ade-140">Create and deploy the blink application</span></span>](iot-hub-raspberry-pi-kit-c-lesson1-deploy-blink-app.md)

