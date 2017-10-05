---
title: "Csatlakozás Azure IoT - lecke 1 málna Pi (csomópont): a Get tools (Windows) |} Microsoft Docs"
description: "Töltse le és telepítse a szükséges eszközök és szoftverek pi első mintaalkalmazás Windows 7 és újabb verziók."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "az IOT-fejlesztési, iot szoftver, dolgot szoftver, internet git telepítése windows, Futtatás gulp, csomópont js windows telepítése, npm telepítése windows, python telepítése windows"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: b3d88e17-97cc-4f23-85fd-a688fc228eb8
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 24c58e006bbef9bbc1fcd626a0f8f6bcac063f05
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-the-tools-windows-7-or-later"></a><span data-ttu-id="f4b72-104">Szerezze be az eszközöket (Windows 7 vagy újabb)</span><span class="sxs-lookup"><span data-stu-id="f4b72-104">Get the tools (Windows 7 or later)</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="f4b72-105">Windows 7 vagy újabb</span><span class="sxs-lookup"><span data-stu-id="f4b72-105">Windows 7 or later</span></span>](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)
> * [<span data-ttu-id="f4b72-106">Ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="f4b72-106">Ubuntu 16.04</span></span>](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-ubuntu.md)
> * [<span data-ttu-id="f4b72-107">macOS 10.10</span><span class="sxs-lookup"><span data-stu-id="f4b72-107">macOS 10.10</span></span>](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-mac.md)


## <a name="what-you-will-do"></a><span data-ttu-id="f4b72-108">Mit fog</span><span class="sxs-lookup"><span data-stu-id="f4b72-108">What you will do</span></span>
<span data-ttu-id="f4b72-109">Töltse le a fejlesztői eszközök és a szoftver málna Pi 3 első minta alkalmazásához.</span><span class="sxs-lookup"><span data-stu-id="f4b72-109">Download the development tools and the software for the first sample application for Raspberry Pi 3.</span></span> <span data-ttu-id="f4b72-110">Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="f4b72-110">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="f4b72-111">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="f4b72-111">What you will learn</span></span>
<span data-ttu-id="f4b72-112">Ebből a cikkből megtudhatja:</span><span class="sxs-lookup"><span data-stu-id="f4b72-112">In this article, you will learn:</span></span>

* <span data-ttu-id="f4b72-113">Hogyan kell telepíteni a Git és Node.js.</span><span class="sxs-lookup"><span data-stu-id="f4b72-113">How to install Git and Node.js.</span></span>
  * <span data-ttu-id="f4b72-114">[Git](https://git-scm.com) van egy nyílt forráskódú elosztott verziókezelő rendszer.</span><span class="sxs-lookup"><span data-stu-id="f4b72-114">[Git](https://git-scm.com) is an open source distributed version control system.</span></span> <span data-ttu-id="f4b72-115">Ez a cikk a mintaalkalmazás Git tárolja.</span><span class="sxs-lookup"><span data-stu-id="f4b72-115">The sample application for this article is stored on Git.</span></span>
  * <span data-ttu-id="f4b72-116">[NODE.js](https://nodejs.org/en/) van a JavaScript futásidejű és gazdag csomag-ökoszisztéma.</span><span class="sxs-lookup"><span data-stu-id="f4b72-116">[Node.js](https://nodejs.org/en/) is a JavaScript runtime with a rich package ecosystem.</span></span>
* <span data-ttu-id="f4b72-117">Hogyan további Node.js fejlesztői eszközök telepítése az NPM segítségével.</span><span class="sxs-lookup"><span data-stu-id="f4b72-117">How to use NPM to install additional Node.js development tools.</span></span>
  * <span data-ttu-id="f4b72-118">A Node.js minimális verziójára vonatkozó követelményt a 4.5-ös LTS.</span><span class="sxs-lookup"><span data-stu-id="f4b72-118">The minimum version requirement of Node.js is 4.5 LTS.</span></span>
  * <span data-ttu-id="f4b72-119">[NPM](https://www.npmjs.com) a csomag kezelők, a Node.js egyike.</span><span class="sxs-lookup"><span data-stu-id="f4b72-119">[NPM](https://www.npmjs.com) is one of the package managers for Node.js.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="f4b72-120">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="f4b72-120">What you need</span></span>
<span data-ttu-id="f4b72-121">A művelet elvégzéséhez szüksége lesz:</span><span class="sxs-lookup"><span data-stu-id="f4b72-121">To complete this operation, you will need:</span></span>

* <span data-ttu-id="f4b72-122">A fejlesztői eszközök és a szoftverfrissítések letöltése az internethez.</span><span class="sxs-lookup"><span data-stu-id="f4b72-122">An Internet connection to download the development tools and the software.</span></span>
* <span data-ttu-id="f4b72-123">Windows rendszerű számítógép.</span><span class="sxs-lookup"><span data-stu-id="f4b72-123">A computer that is running Windows.</span></span>

## <a name="install-git-and-nodejs"></a><span data-ttu-id="f4b72-124">Telepítse a Git szoftver, Node.js</span><span class="sxs-lookup"><span data-stu-id="f4b72-124">Install Git and Node.js</span></span>
<span data-ttu-id="f4b72-125">Kattintson az alábbi hivatkozásokra kattintva töltse le és telepítse a Git szoftver, a Windows Node.js LTS.</span><span class="sxs-lookup"><span data-stu-id="f4b72-125">Click the following links to download and install Git and Node.js LTS for Windows.</span></span>

* [<span data-ttu-id="f4b72-126">Git letöltése a Windows rendszerhez</span><span class="sxs-lookup"><span data-stu-id="f4b72-126">Get Git for Windows</span></span>](https://git-scm.com/download/win/)
* [<span data-ttu-id="f4b72-127">Node.js-es lts verzió letöltése a Windows rendszerhez</span><span class="sxs-lookup"><span data-stu-id="f4b72-127">Get Node.js LTS for Windows</span></span>](https://nodejs.org/en/)

## <a name="install-additional-nodejs-development-tools"></a><span data-ttu-id="f4b72-128">További Node.js fejlesztői eszközök telepítése</span><span class="sxs-lookup"><span data-stu-id="f4b72-128">Install additional Node.js development tools</span></span>
<span data-ttu-id="f4b72-129">Használjon [gulp.js](http://gulpjs.com) Pi a minta-alkalmazás központi telepítésének automatizálásához.</span><span class="sxs-lookup"><span data-stu-id="f4b72-129">Use [gulp.js](http://gulpjs.com) to automate the deployment of the sample application to Pi.</span></span> <span data-ttu-id="f4b72-130">Is használhatja a [cli-eszköz-felderítési](https://github.com/Azure/device-discovery-cli) az IoT-eszközök hálózati adatainak lekérésére.</span><span class="sxs-lookup"><span data-stu-id="f4b72-130">You also use the [device-discovery-cli](https://github.com/Azure/device-discovery-cli) to retrieve network information about your IoT devices.</span></span>

<span data-ttu-id="f4b72-131">Nyisson meg egy parancssort rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="f4b72-131">Start a command prompt as an administrator.</span></span> <span data-ttu-id="f4b72-132">Telepítés `gulp` és `device-discovery-cli` a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="f4b72-132">Install `gulp` and `device-discovery-cli` by running the following command:</span></span>

```bash
npm install -g device-discovery-cli gulp
```

<span data-ttu-id="f4b72-133">Ha problémák, Node.js és a további Node.js fejlesztői eszközök telepítése a számítógépre, tekintse meg a [hibaelhárítási útmutatója](iot-hub-raspberry-pi-kit-node-troubleshooting.md) gyakori problémák megoldásainak.</span><span class="sxs-lookup"><span data-stu-id="f4b72-133">If you experience issues installing Node.js and these additional Node.js development tools on your computer, see the [troubleshooting guide](iot-hub-raspberry-pi-kit-node-troubleshooting.md) for solutions to common problems.</span></span>

## <a name="install-visual-studio-code"></a><span data-ttu-id="f4b72-134">Visual Studio Code telepítése</span><span class="sxs-lookup"><span data-stu-id="f4b72-134">Install Visual Studio Code</span></span>
<span data-ttu-id="f4b72-135">[Töltse le](https://code.visualstudio.com/docs/setup/windows) és a Visual Studio Code telepítése.</span><span class="sxs-lookup"><span data-stu-id="f4b72-135">[Download](https://code.visualstudio.com/docs/setup/windows) and install Visual Studio Code.</span></span> <span data-ttu-id="f4b72-136">A Visual Studio Code a Windows, Linux és macOS egy egyszerűsített, de hatékony forráskód szerkesztőjében.</span><span class="sxs-lookup"><span data-stu-id="f4b72-136">Visual Studio Code is a lightweight but powerful source code editor for Windows, Linux, and macOS.</span></span> <span data-ttu-id="f4b72-137">A mintakód szerkesztése a szerkesztő használata az oktatóanyag későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="f4b72-137">You use this editor later in the tutorial to edit the sample code.</span></span>

## <a name="summary"></a><span data-ttu-id="f4b72-138">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="f4b72-138">Summary</span></span>
<span data-ttu-id="f4b72-139">A szükséges fejlesztői eszközök és az első mintaalkalmazás szoftver telepítése.</span><span class="sxs-lookup"><span data-stu-id="f4b72-139">You've installed the required development tools and software for the first sample application.</span></span> <span data-ttu-id="f4b72-140">A következő feladat létrehozásához, telepítéséhez és futtassa a mintaalkalmazást a Pi-hoz.</span><span class="sxs-lookup"><span data-stu-id="f4b72-140">The next task is to create, deploy, and run the sample application on Pi.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f4b72-141">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f4b72-141">Next steps</span></span>
[<span data-ttu-id="f4b72-142">Létrehozhat és telepíthet a villogási mintaalkalmazás</span><span class="sxs-lookup"><span data-stu-id="f4b72-142">Create and deploy the blink sample application</span></span>](iot-hub-raspberry-pi-kit-node-lesson1-deploy-blink-app.md)

