---
title: "Csatlakozás málna Pi (csomópont) tooAzure IoT - lecke 1: eszközök (Ubuntu) beszerzése |} Microsoft Docs"
description: "Töltse le és Ubuntu hello szükséges eszközök és a szoftverek hello első mintaalkalmazás Pi telepítéséhez."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "az IOT-fejlesztési, iot szoftverek, internet dolgot szoftver, telepítse a git az ubuntu, Futtatás gulp, csomópont js ubuntu telepítése"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: 4d5e45c0-1db9-4662-a039-99ba26333085
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: b4f566fa0d1faf8b2321707145f675e3d87f0bef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-tools-ubuntu-1604"></a><span data-ttu-id="e39c6-104">Hello eszközök (Ubuntu 16.04) beolvasása</span><span class="sxs-lookup"><span data-stu-id="e39c6-104">Get hello tools (Ubuntu 16.04)</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="e39c6-105">Windows 7 vagy újabb</span><span class="sxs-lookup"><span data-stu-id="e39c6-105">Windows 7 or later</span></span>](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)
> * [<span data-ttu-id="e39c6-106">Ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="e39c6-106">Ubuntu 16.04</span></span>](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-ubuntu.md)
> * [<span data-ttu-id="e39c6-107">macOS 10.10</span><span class="sxs-lookup"><span data-stu-id="e39c6-107">macOS 10.10</span></span>](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-mac.md)


## <a name="what-you-will-do"></a><span data-ttu-id="e39c6-108">Mit fog</span><span class="sxs-lookup"><span data-stu-id="e39c6-108">What you will do</span></span>
<span data-ttu-id="e39c6-109">Hello Fejlesztőeszközök és hello első mintaalkalmazás málna Pi 3 hello szoftver letöltése.</span><span class="sxs-lookup"><span data-stu-id="e39c6-109">Download hello development tools and hello software for hello first sample application for Raspberry Pi 3.</span></span> <span data-ttu-id="e39c6-110">Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="e39c6-110">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="e39c6-111">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="e39c6-111">What you will learn</span></span>
<span data-ttu-id="e39c6-112">Ebből a cikkből megtudhatja:</span><span class="sxs-lookup"><span data-stu-id="e39c6-112">In this article, you will learn:</span></span>

* <span data-ttu-id="e39c6-113">Hogyan tooinstall a Git szoftver, Node.js.</span><span class="sxs-lookup"><span data-stu-id="e39c6-113">How tooinstall Git and Node.js.</span></span>
  * <span data-ttu-id="e39c6-114">[Git](https://git-scm.com) van egy nyílt forráskódú elosztott verziókezelő rendszer.</span><span class="sxs-lookup"><span data-stu-id="e39c6-114">[Git](https://git-scm.com) is an open source distributed version control system.</span></span> <span data-ttu-id="e39c6-115">Ez a cikk hello-mintaalkalmazás Git tárolja.</span><span class="sxs-lookup"><span data-stu-id="e39c6-115">hello sample application for this article is stored on Git.</span></span>
  * <span data-ttu-id="e39c6-116">[NODE.js](https://nodejs.org/en/) van a JavaScript futásidejű és gazdag csomag-ökoszisztéma.</span><span class="sxs-lookup"><span data-stu-id="e39c6-116">[Node.js](https://nodejs.org/en/) is a JavaScript runtime with a rich package ecosystem.</span></span>
* <span data-ttu-id="e39c6-117">Hogyan toouse NPM tooinstall további Node.js fejlesztői eszközök.</span><span class="sxs-lookup"><span data-stu-id="e39c6-117">How toouse NPM tooinstall additional Node.js development tools.</span></span>
  * <span data-ttu-id="e39c6-118">hello minimálisan szükséges verziója Node.js 4.5-ös LTS.</span><span class="sxs-lookup"><span data-stu-id="e39c6-118">hello minimum required version of Node.js is 4.5 LTS.</span></span>
  * <span data-ttu-id="e39c6-119">[NPM](https://www.npmjs.com) egyike hello Node.js csomag feletteseit.</span><span class="sxs-lookup"><span data-stu-id="e39c6-119">[NPM](https://www.npmjs.com) is one of hello package managers for Node.js.</span></span>

## <a name="what-do-you-need"></a><span data-ttu-id="e39c6-120">Mire van szüksége</span><span class="sxs-lookup"><span data-stu-id="e39c6-120">What do you need</span></span>
<span data-ttu-id="e39c6-121">toocomplete ennél a műveletnél, szüksége lesz:</span><span class="sxs-lookup"><span data-stu-id="e39c6-121">toocomplete this operation, you will need:</span></span>

* <span data-ttu-id="e39c6-122">Az Internet kapcsolat toodownload hello Fejlesztőeszközök és hello szoftver.</span><span class="sxs-lookup"><span data-stu-id="e39c6-122">An Internet connection toodownload hello development tools and hello software.</span></span>
* <span data-ttu-id="e39c6-123">Ubuntu 16.04 vagy újabb rendszerrel működő számítógép.</span><span class="sxs-lookup"><span data-stu-id="e39c6-123">A computer that is running Ubuntu 16.04 or later.</span></span>

## <a name="install-git-nodejs-and-npm"></a><span data-ttu-id="e39c6-124">Telepítse a Git, Node.js és NPM</span><span class="sxs-lookup"><span data-stu-id="e39c6-124">Install Git, Node.js, and NPM</span></span>
<span data-ttu-id="e39c6-125">Használjon hello billentyűparancsot `Ctrl + Alt + T` tooopen egy terminál és futtatási hello a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="e39c6-125">Use hello keyboard shortcut `Ctrl + Alt + T` tooopen a terminal and run hello following commands:</span></span>

```bash
sudo apt-get update
curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
sudo apt-get install -y nodejs
sudo apt-get install git
```

## <a name="install-additional-nodejs-development-tools"></a><span data-ttu-id="e39c6-126">További Node.js fejlesztői eszközök telepítése</span><span class="sxs-lookup"><span data-stu-id="e39c6-126">Install additional Node.js development tools</span></span>
<span data-ttu-id="e39c6-127">Használhat [gulp.js](http://gulpjs.com) hello minta alkalmazás tooPi tooautomate hello központi telepítését.</span><span class="sxs-lookup"><span data-stu-id="e39c6-127">You use [gulp.js](http://gulpjs.com) tooautomate hello deployment of hello sample application tooPi.</span></span> <span data-ttu-id="e39c6-128">Hello is a [cli-eszköz-felderítési](https://github.com/Azure/device-discovery-cli) tooretrieve hálózati adatokat az IoT-eszközökről.</span><span class="sxs-lookup"><span data-stu-id="e39c6-128">You also use hello [device-discovery-cli](https://github.com/Azure/device-discovery-cli) tooretrieve network information about your IoT devices.</span></span>

<span data-ttu-id="e39c6-129">Telepítés `gulp` és `device-discovery-cli` hello hello terminálban parancs a következő futtatásával:</span><span class="sxs-lookup"><span data-stu-id="e39c6-129">Install `gulp` and `device-discovery-cli` by running hello following command in hello terminal:</span></span>

```bash
sudo npm install -g device-discovery-cli gulp
```

<span data-ttu-id="e39c6-130">Ha problémák Ubuntu Node.js és a további fejlesztői eszközök telepítése, lásd: hello [hibaelhárítási útmutatója](iot-hub-raspberry-pi-kit-node-troubleshooting.md) a megoldások toocommon problémákat.</span><span class="sxs-lookup"><span data-stu-id="e39c6-130">If you experience issues installing Node.js and these additional development tools on Ubuntu, see hello [troubleshooting guide](iot-hub-raspberry-pi-kit-node-troubleshooting.md) for solutions toocommon problems.</span></span>

## <a name="install-visual-studio-code"></a><span data-ttu-id="e39c6-131">Visual Studio Code telepítése</span><span class="sxs-lookup"><span data-stu-id="e39c6-131">Install Visual Studio Code</span></span>
<span data-ttu-id="e39c6-132">[Töltse le](https://code.visualstudio.com/docs/setup/linux) és a Visual Studio Code telepítése.</span><span class="sxs-lookup"><span data-stu-id="e39c6-132">[Download](https://code.visualstudio.com/docs/setup/linux) and install Visual Studio Code.</span></span> <span data-ttu-id="e39c6-133">A Visual Studio Code a Windows, Linux és macOS egy egyszerűsített, de hatékony forráskód szerkesztőjében.</span><span class="sxs-lookup"><span data-stu-id="e39c6-133">Visual Studio Code is a lightweight but powerful source code editor for Windows, Linux, and macOS.</span></span> <span data-ttu-id="e39c6-134">A szerkesztő később hello oktatóanyag tooedit hello mintakód használható.</span><span class="sxs-lookup"><span data-stu-id="e39c6-134">You use this editor later in hello tutorial tooedit hello sample code.</span></span>

## <a name="summary"></a><span data-ttu-id="e39c6-135">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="e39c6-135">Summary</span></span>
<span data-ttu-id="e39c6-136">Szükséges hello fejlesztői eszközök és szoftverek hello első mintaalkalmazás telepítése.</span><span class="sxs-lookup"><span data-stu-id="e39c6-136">You've installed hello required development tools and software for hello first sample application.</span></span> <span data-ttu-id="e39c6-137">hello tovább feladat toocreate, telepítése, és futtassa a hello mintaalkalmazást a Pi.</span><span class="sxs-lookup"><span data-stu-id="e39c6-137">hello next task is toocreate, deploy, and run hello sample application on Pi.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e39c6-138">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e39c6-138">Next steps</span></span>
[<span data-ttu-id="e39c6-139">Hello villogási minta alkalmazás létrehozását és telepítését</span><span class="sxs-lookup"><span data-stu-id="e39c6-139">Create and deploy hello blink sample application</span></span>](iot-hub-raspberry-pi-kit-node-lesson1-deploy-blink-app.md)

