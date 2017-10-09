---
title: "Csatlakozás málna Pi (csomópont) tooAzure IoT - lecke 1: eszközök (macOS) beszerzése |} Microsoft Docs"
description: "Töltse le és hello szükséges eszközök és szoftverek hello első mintaalkalmazás Pi telepítéséhez macOS."
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
ms.openlocfilehash: 382b066cb7ece7ffdeb22b162b725727b22e5fac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-tools-macos-1010"></a><span data-ttu-id="1be9f-104">Hello eszközök (macOS 10.10) beolvasása</span><span class="sxs-lookup"><span data-stu-id="1be9f-104">Get hello tools (macOS 10.10)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="1be9f-105">Windows 7 vagy újabb</span><span class="sxs-lookup"><span data-stu-id="1be9f-105">Windows 7 or later</span></span>](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)
> * [<span data-ttu-id="1be9f-106">Ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="1be9f-106">Ubuntu 16.04</span></span>](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-ubuntu.md)
> * [<span data-ttu-id="1be9f-107">macOS 10.10</span><span class="sxs-lookup"><span data-stu-id="1be9f-107">macOS 10.10</span></span>](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-mac.md)

## <a name="what-you-will-do"></a><span data-ttu-id="1be9f-108">Mit fog</span><span class="sxs-lookup"><span data-stu-id="1be9f-108">What you will do</span></span>
<span data-ttu-id="1be9f-109">Hello Fejlesztőeszközök és hello első mintaalkalmazást a málna Pi 3 hello szoftver letöltése.</span><span class="sxs-lookup"><span data-stu-id="1be9f-109">Download hello development tools and hello software for hello first sample application for your Raspberry Pi 3.</span></span> <span data-ttu-id="1be9f-110">Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="1be9f-110">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="1be9f-111">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="1be9f-111">What you will learn</span></span>
<span data-ttu-id="1be9f-112">Ebből a cikkből megtudhatja:</span><span class="sxs-lookup"><span data-stu-id="1be9f-112">In this article, you will learn:</span></span>

* <span data-ttu-id="1be9f-113">Hogyan tooinstall a Git szoftver, Node.js.</span><span class="sxs-lookup"><span data-stu-id="1be9f-113">How tooinstall Git and Node.js.</span></span>
  * <span data-ttu-id="1be9f-114">[Git](https://git-scm.com) van egy nyílt forráskódú elosztott verziókezelő rendszer.</span><span class="sxs-lookup"><span data-stu-id="1be9f-114">[Git](https://git-scm.com) is an open source distributed version control system.</span></span> <span data-ttu-id="1be9f-115">Ez a cikk hello-mintaalkalmazás Git tárolja.</span><span class="sxs-lookup"><span data-stu-id="1be9f-115">hello sample application for this article is stored on Git.</span></span>
  * <span data-ttu-id="1be9f-116">[NODE.js](https://nodejs.org/en/) van a JavaScript futásidejű és gazdag csomag-ökoszisztéma.</span><span class="sxs-lookup"><span data-stu-id="1be9f-116">[Node.js](https://nodejs.org/en/) is a JavaScript runtime with a rich package ecosystem.</span></span>
* <span data-ttu-id="1be9f-117">Hogyan toouse NPM tooinstall további Node.js fejlesztői eszközök.</span><span class="sxs-lookup"><span data-stu-id="1be9f-117">How toouse NPM tooinstall additional Node.js development tools.</span></span>
  * <span data-ttu-id="1be9f-118">hello minimálisan szükséges verziója Node.js 4.5-ös LTS.</span><span class="sxs-lookup"><span data-stu-id="1be9f-118">hello minimum required version of Node.js is 4.5 LTS.</span></span>
  * <span data-ttu-id="1be9f-119">[NPM](https://www.npmjs.com) egyike hello Node.js csomag feletteseit.</span><span class="sxs-lookup"><span data-stu-id="1be9f-119">[NPM](https://www.npmjs.com) is one of hello package managers for Node.js.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="1be9f-120">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="1be9f-120">What you need</span></span>
<span data-ttu-id="1be9f-121">toocomplete ennél a műveletnél, szüksége lesz:</span><span class="sxs-lookup"><span data-stu-id="1be9f-121">toocomplete this operation, you will need:</span></span>

* <span data-ttu-id="1be9f-122">Az Internet kapcsolat toodownload hello Fejlesztőeszközök és hello szoftver.</span><span class="sxs-lookup"><span data-stu-id="1be9f-122">An Internet connection toodownload hello development tools and hello software.</span></span>
* <span data-ttu-id="1be9f-123">MacOS Yosemite (10.10) futtató Mac vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="1be9f-123">A Mac that is running macOS Yosemite (10.10) or later.</span></span>

## <a name="install-git-and-nodejs"></a><span data-ttu-id="1be9f-124">Telepítse a Git szoftver, Node.js</span><span class="sxs-lookup"><span data-stu-id="1be9f-124">Install Git and Node.js</span></span>
<span data-ttu-id="1be9f-125">tooinstall Git és a Node.js, használja a hello [Homebrew](http://brew.sh) segédprogramja csomag a következő lépések végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="1be9f-125">tooinstall Git and Node.js, use hello [Homebrew](http://brew.sh) package management utility by following these steps:</span></span>

1. <span data-ttu-id="1be9f-126">Telepítse a Homebrew.</span><span class="sxs-lookup"><span data-stu-id="1be9f-126">Install Homebrew.</span></span> <span data-ttu-id="1be9f-127">Ha már telepített Homebrew, nyissa meg toostep 2.</span><span class="sxs-lookup"><span data-stu-id="1be9f-127">If you've already installed Homebrew, go toostep 2.</span></span>
   
   1. <span data-ttu-id="1be9f-128">Nyomja le az `Cmd + Space` , és írja be `Terminal` tooopen egy terminált.</span><span class="sxs-lookup"><span data-stu-id="1be9f-128">Press `Cmd + Space` and enter `Terminal` tooopen a terminal.</span></span>
   2. <span data-ttu-id="1be9f-129">Futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="1be9f-129">Run hello following command:</span></span>
      
      ```bash
      /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
      ```
2. <span data-ttu-id="1be9f-130">Telepítse a Git és Node.js hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="1be9f-130">Install Git and Node.js by running hello following command:</span></span>
   
   ```bash
   brew install node git
   ```

## <a name="install-additional-nodejs-development-tools"></a><span data-ttu-id="1be9f-131">További Node.js fejlesztői eszközök telepítése</span><span class="sxs-lookup"><span data-stu-id="1be9f-131">Install additional Node.js development tools</span></span>
<span data-ttu-id="1be9f-132">Használjon [gulp.js](http://gulpjs.com) hello minta alkalmazás tooPi tooautomate hello központi telepítését.</span><span class="sxs-lookup"><span data-stu-id="1be9f-132">Use [gulp.js](http://gulpjs.com) tooautomate hello deployment of hello sample application tooPi.</span></span> <span data-ttu-id="1be9f-133">Használjon hello [cli-eszköz-felderítési](https://github.com/Azure/device-discovery-cli) tooretrieve hálózati adatokat az IoT-eszközökről.</span><span class="sxs-lookup"><span data-stu-id="1be9f-133">Use hello [device-discovery-cli](https://github.com/Azure/device-discovery-cli) tooretrieve network information about your IoT devices.</span></span>

<span data-ttu-id="1be9f-134">Telepítés `gulp` és `device-discovery-cli` hello hello terminálban parancs a következő futtatásával:</span><span class="sxs-lookup"><span data-stu-id="1be9f-134">Install `gulp` and `device-discovery-cli` by running hello following command in hello terminal:</span></span>

```bash
npm install -g device-discovery-cli gulp
```

<span data-ttu-id="1be9f-135">Ha problémák macOS Node.js és a további fejlesztői eszközök telepítése, lásd: hello [hibaelhárítási útmutatója](iot-hub-raspberry-pi-kit-node-troubleshooting.md) a megoldások toocommon problémákat.</span><span class="sxs-lookup"><span data-stu-id="1be9f-135">If you experience issues installing Node.js and these additional development tools on macOS, see hello [troubleshooting guide](iot-hub-raspberry-pi-kit-node-troubleshooting.md) for solutions toocommon problems.</span></span>

## <a name="install-visual-studio-code"></a><span data-ttu-id="1be9f-136">Visual Studio Code telepítése</span><span class="sxs-lookup"><span data-stu-id="1be9f-136">Install Visual Studio Code</span></span>
<span data-ttu-id="1be9f-137">[Töltse le](https://code.visualstudio.com/docs/setup/osx) és a Visual Studio Code telepítése.</span><span class="sxs-lookup"><span data-stu-id="1be9f-137">[Download](https://code.visualstudio.com/docs/setup/osx) and install Visual Studio Code.</span></span> <span data-ttu-id="1be9f-138">A Visual Studio Code a Windows, Linux és macOS egy egyszerűsített, de hatékony forráskód szerkesztőjében.</span><span class="sxs-lookup"><span data-stu-id="1be9f-138">Visual Studio Code is a lightweight but powerful source code editor for Windows, Linux, and macOS.</span></span> <span data-ttu-id="1be9f-139">A szerkesztő később hello oktatóanyag tooedit hello mintakód használható.</span><span class="sxs-lookup"><span data-stu-id="1be9f-139">You use this editor later in hello tutorial tooedit hello sample code.</span></span>

## <a name="summary"></a><span data-ttu-id="1be9f-140">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="1be9f-140">Summary</span></span>
<span data-ttu-id="1be9f-141">Szükséges hello fejlesztői eszközök és szoftverek hello első mintaalkalmazás telepítése.</span><span class="sxs-lookup"><span data-stu-id="1be9f-141">You've installed hello required development tools and software for hello first sample application.</span></span> <span data-ttu-id="1be9f-142">hello tovább feladat toocreate, telepítése, és futtassa a hello mintaalkalmazást a Pi.</span><span class="sxs-lookup"><span data-stu-id="1be9f-142">hello next task is toocreate, deploy, and run hello sample application on Pi.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1be9f-143">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1be9f-143">Next steps</span></span>
[<span data-ttu-id="1be9f-144">Hello villogási minta alkalmazás létrehozását és telepítését</span><span class="sxs-lookup"><span data-stu-id="1be9f-144">Create and deploy hello blink sample application</span></span>](iot-hub-raspberry-pi-kit-node-lesson1-deploy-blink-app.md)

