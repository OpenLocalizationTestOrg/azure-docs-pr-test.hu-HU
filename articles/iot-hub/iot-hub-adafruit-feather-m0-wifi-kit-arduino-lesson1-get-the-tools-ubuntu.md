---
title: "Csatlakozás Azure IoT - lecke 1 Arduino: (Ubuntu) eszközök beszerzése |} Microsoft Docs"
description: "Töltse le és telepítse a szükséges eszközök és szoftverek Adafruit lágyított M0 Wi-Fi első minta alkalmazásához Ubuntu."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "arduino fejlesztői eszközök, az iot-fejlesztési, iot szoftver, internet dolgot szoftver, az ubuntu, telepítés csomópont js ubuntu telepítés git"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: 7572f191-420d-41f0-923b-7ea86c0bfa73
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 90b1c12659c33517142e2048d8f5f629f6d6b4c9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-the-tools-ubuntu-1604"></a><span data-ttu-id="25dfa-104">Eszközök beszerzése (Ubuntu 16.04)</span><span class="sxs-lookup"><span data-stu-id="25dfa-104">Get the tools (Ubuntu 16.04)</span></span>

> [!div class="op_single_selector"]
> * <span data-ttu-id="25dfa-105">[Windows 7 vagy újabb][windows]</span><span class="sxs-lookup"><span data-stu-id="25dfa-105">[Windows 7 or later][windows]</span></span>
> * <span data-ttu-id="25dfa-106">[Ubuntu 16.04][ubuntu]</span><span class="sxs-lookup"><span data-stu-id="25dfa-106">[Ubuntu 16.04][ubuntu]</span></span>
> * <span data-ttu-id="25dfa-107">[macOS 10.10][macos]</span><span class="sxs-lookup"><span data-stu-id="25dfa-107">[macOS 10.10][macos]</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="25dfa-108">Mit fog</span><span class="sxs-lookup"><span data-stu-id="25dfa-108">What you will do</span></span>

<span data-ttu-id="25dfa-109">A fejlesztői eszközök és a szoftver a Adafruit lágyított M0 Wi-Fi Arduino kártya első mintaalkalmazás letöltése.</span><span class="sxs-lookup"><span data-stu-id="25dfa-109">Download the development tools and the software for the first sample application for your Adafruit Feather M0 WiFi Arduino board.</span></span> 

<span data-ttu-id="25dfa-110">Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="25dfa-110">If you have any problems, look for solutions on the [troubleshooting page][troubleshooting].</span></span>

> [!NOTE]
> <span data-ttu-id="25dfa-111">Bár a programozási nyelv, a fő logikájának Arduino, Node.js eszközök szerepelnek a megszerzett létrehozásához és központi telepítéséhez alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="25dfa-111">Although the programming language of the main logic is Arduino, Node.js tools are used in the lessons to build and deploy sample applications.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="25dfa-112">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="25dfa-112">What you will learn</span></span>
<span data-ttu-id="25dfa-113">Ebből a cikkből megtudhatja:</span><span class="sxs-lookup"><span data-stu-id="25dfa-113">In this article, you will learn:</span></span>

* <span data-ttu-id="25dfa-114">A Git szoftver, Node.js telepítése</span><span class="sxs-lookup"><span data-stu-id="25dfa-114">How to install Git and Node.js</span></span>
  * <span data-ttu-id="25dfa-115">[Git](https://git-scm.com) van egy nyílt forráskódú elosztott verziókezelő rendszer.</span><span class="sxs-lookup"><span data-stu-id="25dfa-115">[Git](https://git-scm.com) is an open source distributed version control system.</span></span> <span data-ttu-id="25dfa-116">Ez a cikk a mintaalkalmazás Git tárolja.</span><span class="sxs-lookup"><span data-stu-id="25dfa-116">The sample application for this article is stored on Git.</span></span>
  * <span data-ttu-id="25dfa-117">[NODE.js](https://nodejs.org/en/) van a JavaScript futásidejű és gazdag csomag-ökoszisztéma.</span><span class="sxs-lookup"><span data-stu-id="25dfa-117">[Node.js](https://nodejs.org/en/) is a JavaScript runtime with a rich package ecosystem.</span></span>
* <span data-ttu-id="25dfa-118">Hogyan további Node.js fejlesztői eszközök telepítése az NPM segítségével.</span><span class="sxs-lookup"><span data-stu-id="25dfa-118">How to use NPM to install additional Node.js development tools.</span></span>
  * <span data-ttu-id="25dfa-119">A Node.js minimálisan szükséges verziója a 4.5-ös LTS.</span><span class="sxs-lookup"><span data-stu-id="25dfa-119">The minimum required version of Node.js is 4.5 LTS.</span></span>
  * <span data-ttu-id="25dfa-120">[NPM](https://www.npmjs.com) a csomag kezelők, a Node.js egyike.</span><span class="sxs-lookup"><span data-stu-id="25dfa-120">[NPM](https://www.npmjs.com) is one of the package managers for Node.js.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="25dfa-121">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="25dfa-121">What you need</span></span>
<span data-ttu-id="25dfa-122">A művelet elvégzéséhez szüksége lesz:</span><span class="sxs-lookup"><span data-stu-id="25dfa-122">To complete this operation, you will need:</span></span>
* <span data-ttu-id="25dfa-123">A fejlesztői eszközök és a szoftverfrissítések letöltése az internethez.</span><span class="sxs-lookup"><span data-stu-id="25dfa-123">An Internet connection to download the development tools and the software.</span></span>
* <span data-ttu-id="25dfa-124">Ubuntu 16.04 vagy újabb rendszerrel működő számítógép.</span><span class="sxs-lookup"><span data-stu-id="25dfa-124">A computer that is running Ubuntu 16.04 or later.</span></span>

## <a name="install-git-nodejs-and-npm"></a><span data-ttu-id="25dfa-125">Telepítse a Git, Node.js és NPM</span><span class="sxs-lookup"><span data-stu-id="25dfa-125">Install Git, Node.js, and NPM</span></span>
<span data-ttu-id="25dfa-126">Használja a billentyűparancsot `Ctrl + Alt + T` nyisson meg egy terminált, és futtassa a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="25dfa-126">Use the keyboard shortcut `Ctrl + Alt + T` to open a terminal and run the following commands:</span></span>

```bash
sudo apt-get update
curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
sudo apt-get install -y nodejs
sudo apt-get install git
```

## <a name="install-additional-nodejs-development-tools"></a><span data-ttu-id="25dfa-127">További Node.js fejlesztői eszközök telepítése</span><span class="sxs-lookup"><span data-stu-id="25dfa-127">Install additional Node.js development tools</span></span>
<span data-ttu-id="25dfa-128">Használjon [gulp.js](http://gulpjs.com) a Arduino táblán a minta-alkalmazás központi telepítésének automatizálásához.</span><span class="sxs-lookup"><span data-stu-id="25dfa-128">Use [gulp.js](http://gulpjs.com) to automate the deployment of the sample application to your Arduino board.</span></span>

<span data-ttu-id="25dfa-129">Telepítés `gulp`, `device-discovery-cli` a terminálban a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="25dfa-129">Install `gulp`, `device-discovery-cli` by running the following command in the terminal:</span></span>

```bash
sudo npm install -g gulp device-discovery-cli
```

<span data-ttu-id="25dfa-130">Ha problémák Ubuntu Node.js és a további fejlesztői eszközök telepítése, lásd: a [hibaelhárítási útmutatója] [ troubleshooting] gyakori problémák megoldásainak.</span><span class="sxs-lookup"><span data-stu-id="25dfa-130">If you experience issues installing Node.js and these additional development tools on Ubuntu, see the [troubleshooting guide][troubleshooting] for solutions to common problems.</span></span>

## <a name="install-visual-studio-code"></a><span data-ttu-id="25dfa-131">Visual Studio Code telepítése</span><span class="sxs-lookup"><span data-stu-id="25dfa-131">Install Visual Studio Code</span></span>
<span data-ttu-id="25dfa-132">[Töltse le](https://code.visualstudio.com/docs/setup/linux) és a Visual Studio Code telepítése.</span><span class="sxs-lookup"><span data-stu-id="25dfa-132">[Download](https://code.visualstudio.com/docs/setup/linux) and install Visual Studio Code.</span></span> <span data-ttu-id="25dfa-133">A Visual Studio Code a Windows, Linux és macOS egy egyszerűsített, de hatékony forráskód szerkesztőjében.</span><span class="sxs-lookup"><span data-stu-id="25dfa-133">Visual Studio Code is a lightweight but powerful source code editor for Windows, Linux, and macOS.</span></span> <span data-ttu-id="25dfa-134">A mintakód szerkesztése a szerkesztő használata az oktatóanyag későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="25dfa-134">You use this editor later in the tutorial to edit the sample code.</span></span>

## <a name="summary"></a><span data-ttu-id="25dfa-135">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="25dfa-135">Summary</span></span>
<span data-ttu-id="25dfa-136">A szükséges fejlesztői eszközök és az első mintaalkalmazás szoftver telepítése.</span><span class="sxs-lookup"><span data-stu-id="25dfa-136">You've installed the required development tools and software for the first sample application.</span></span> <span data-ttu-id="25dfa-137">A következő feladata a létrehozásához, telepítéséhez és futtassa a mintaalkalmazást a Arduino táblán.</span><span class="sxs-lookup"><span data-stu-id="25dfa-137">The next task is to create, deploy, and run the sample application on your Arduino board.</span></span>

## <a name="next-steps"></a><span data-ttu-id="25dfa-138">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="25dfa-138">Next steps</span></span>
<span data-ttu-id="25dfa-139">[Létrehozhat és telepíthet a villogási mintaalkalmazás][create-and-deploy-the-blink-sample-application]</span><span class="sxs-lookup"><span data-stu-id="25dfa-139">[Create and deploy the blink sample application][create-and-deploy-the-blink-sample-application]</span></span>

<!-- Images and links -->

[windows]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-get-the-tools-win32.md
[ubuntu]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-get-the-tools-ubuntu.md
[macos]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-get-the-tools-mac.md
[troubleshooting]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md
[create-and-deploy-the-blink-sample-application]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-deploy-blink-app.md