---
title: "Connect Raspberry pi (C) tooAzure IoT - lecke 1: eszközök (macOS) beszerzése |} Microsoft Docs"
description: "Töltse le és hello szükséges eszközök és szoftverek hello első mintaalkalmazás Pi telepítéséhez macOS."
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
ms.openlocfilehash: 68ee44945dd69edcdf61ab3da4c80379c0ef955d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-tools-macos-1010"></a><span data-ttu-id="c6900-104">Hello eszközök (macOS 10.10) beolvasása</span><span class="sxs-lookup"><span data-stu-id="c6900-104">Get hello tools (macOS 10.10)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="c6900-105">Windows 7 vagy újabb</span><span class="sxs-lookup"><span data-stu-id="c6900-105">Windows 7 or later</span></span>](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-win32.md)
> * [<span data-ttu-id="c6900-106">Ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="c6900-106">Ubuntu 16.04</span></span>](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-ubuntu.md)
> * [<span data-ttu-id="c6900-107">macOS 10.10</span><span class="sxs-lookup"><span data-stu-id="c6900-107">macOS 10.10</span></span>](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-mac.md)

## <a name="what-you-will-do"></a><span data-ttu-id="c6900-108">Mit fog</span><span class="sxs-lookup"><span data-stu-id="c6900-108">What you will do</span></span>
<span data-ttu-id="c6900-109">Hello Fejlesztőeszközök és hello első mintaalkalmazást a málna Pi 3 hello szoftver letöltése.</span><span class="sxs-lookup"><span data-stu-id="c6900-109">Download hello development tools and hello software for hello first sample application for your Raspberry Pi 3.</span></span> <span data-ttu-id="c6900-110">Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="c6900-110">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span></span>

> [!NOTE]
> <span data-ttu-id="c6900-111">Bár programozási nyelv hello fő logikájának hello C, Node.js eszközök szerepelnek hello során tapasztalatokat toodiscover eszközök, és hozza létre, és minta alkalmazások központi telepítése.</span><span class="sxs-lookup"><span data-stu-id="c6900-111">Although hello programming language of hello main logic is C, Node.js tools are used in hello lessons toodiscover devices, and build and deploy sample applications.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="c6900-112">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="c6900-112">What you will learn</span></span>
<span data-ttu-id="c6900-113">Ebből a cikkből megtudhatja:</span><span class="sxs-lookup"><span data-stu-id="c6900-113">In this article, you will learn:</span></span>

* <span data-ttu-id="c6900-114">Hogyan tooinstall a Git szoftver, Node.js.</span><span class="sxs-lookup"><span data-stu-id="c6900-114">How tooinstall Git and Node.js.</span></span>
  * <span data-ttu-id="c6900-115">[Git](https://git-scm.com) van egy nyílt forráskódú elosztott verziókezelő rendszer.</span><span class="sxs-lookup"><span data-stu-id="c6900-115">[Git](https://git-scm.com) is an open source distributed version control system.</span></span> <span data-ttu-id="c6900-116">Ez a cikk hello-mintaalkalmazás Git tárolja.</span><span class="sxs-lookup"><span data-stu-id="c6900-116">hello sample application for this article is stored on Git.</span></span>
  * <span data-ttu-id="c6900-117">[NODE.js](https://nodejs.org/en/) van a JavaScript futásidejű és gazdag csomag-ökoszisztéma.</span><span class="sxs-lookup"><span data-stu-id="c6900-117">[Node.js](https://nodejs.org/en/) is a JavaScript runtime with a rich package ecosystem.</span></span>
* <span data-ttu-id="c6900-118">Hogyan toouse NPM tooinstall további Node.js fejlesztői eszközök.</span><span class="sxs-lookup"><span data-stu-id="c6900-118">How toouse NPM tooinstall additional Node.js development tools.</span></span>
  * <span data-ttu-id="c6900-119">hello minimálisan szükséges verziója Node.js 4.5-ös LTS.</span><span class="sxs-lookup"><span data-stu-id="c6900-119">hello minimum required version of Node.js is 4.5 LTS.</span></span>
  * <span data-ttu-id="c6900-120">[NPM](https://www.npmjs.com) egyike hello Node.js csomag feletteseit.</span><span class="sxs-lookup"><span data-stu-id="c6900-120">[NPM](https://www.npmjs.com) is one of hello package managers for Node.js.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="c6900-121">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="c6900-121">What you need</span></span>
<span data-ttu-id="c6900-122">toocomplete ennél a műveletnél, szüksége lesz:</span><span class="sxs-lookup"><span data-stu-id="c6900-122">toocomplete this operation, you will need:</span></span>

* <span data-ttu-id="c6900-123">Az Internet kapcsolat toodownload hello Fejlesztőeszközök és hello szoftver.</span><span class="sxs-lookup"><span data-stu-id="c6900-123">An Internet connection toodownload hello development tools and hello software.</span></span>
* <span data-ttu-id="c6900-124">MacOS Yosemite (10.10) futtató Mac vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="c6900-124">A Mac that is running macOS Yosemite (10.10) or later.</span></span>

## <a name="install-git-and-nodejs"></a><span data-ttu-id="c6900-125">Telepítse a Git szoftver, Node.js</span><span class="sxs-lookup"><span data-stu-id="c6900-125">Install Git and Node.js</span></span>
<span data-ttu-id="c6900-126">tooinstall Git és a Node.js, használja a hello [Homebrew](http://brew.sh) segédprogramja csomag a következő lépések végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="c6900-126">tooinstall Git and Node.js, use hello [Homebrew](http://brew.sh) package management utility by following these steps:</span></span>

1. <span data-ttu-id="c6900-127">Telepítse a Homebrew.</span><span class="sxs-lookup"><span data-stu-id="c6900-127">Install Homebrew.</span></span> <span data-ttu-id="c6900-128">Ha már telepített Homebrew, nyissa meg toostep 2.</span><span class="sxs-lookup"><span data-stu-id="c6900-128">If you've already installed Homebrew, go toostep 2.</span></span>
   
   1. <span data-ttu-id="c6900-129">Nyomja le az `Cmd + Space` , és írja be `Terminal` tooopen egy terminált.</span><span class="sxs-lookup"><span data-stu-id="c6900-129">Press `Cmd + Space` and enter `Terminal` tooopen a terminal.</span></span>
   2. <span data-ttu-id="c6900-130">Futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="c6900-130">Run hello following command:</span></span>
      
      ```bash
      /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
      ```
2. <span data-ttu-id="c6900-131">Telepítse a Git és Node.js hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="c6900-131">Install Git and Node.js by running hello following command:</span></span>
   
   ```bash
   brew install node git
   ```

## <a name="install-additional-nodejs-development-tools"></a><span data-ttu-id="c6900-132">További Node.js fejlesztői eszközök telepítése</span><span class="sxs-lookup"><span data-stu-id="c6900-132">Install additional Node.js development tools</span></span>
<span data-ttu-id="c6900-133">Használjon [gulp.js](http://gulpjs.com) hello minta alkalmazás tooyour Pi tooautomate hello központi telepítését.</span><span class="sxs-lookup"><span data-stu-id="c6900-133">Use [gulp.js](http://gulpjs.com) tooautomate hello deployment of hello sample application tooyour Pi.</span></span> <span data-ttu-id="c6900-134">Használjon hello [cli-eszköz-felderítési](https://github.com/Azure/device-discovery-cli) tooretrieve hálózati adatokat az IoT-eszközökről.</span><span class="sxs-lookup"><span data-stu-id="c6900-134">Use hello [device-discovery-cli](https://github.com/Azure/device-discovery-cli) tooretrieve network information about your IoT devices.</span></span>

<span data-ttu-id="c6900-135">Telepítés `gulp` és `device-discovery-cli` hello hello terminálban parancs a következő futtatásával:</span><span class="sxs-lookup"><span data-stu-id="c6900-135">Install `gulp` and `device-discovery-cli` by running hello following command in hello terminal:</span></span>

```bash
sudo npm install -g device-discovery-cli gulp
```

<span data-ttu-id="c6900-136">Ha problémák macOS Node.js és a további fejlesztői eszközök telepítése, lásd: hello [hibaelhárítási útmutatója](iot-hub-raspberry-pi-kit-c-troubleshooting.md) a megoldások toocommon problémákat.</span><span class="sxs-lookup"><span data-stu-id="c6900-136">If you experience issues installing Node.js and these additional development tools on macOS, see hello [troubleshooting guide](iot-hub-raspberry-pi-kit-c-troubleshooting.md) for solutions toocommon problems.</span></span>

## <a name="install-visual-studio-code"></a><span data-ttu-id="c6900-137">Visual Studio Code telepítése</span><span class="sxs-lookup"><span data-stu-id="c6900-137">Install Visual Studio Code</span></span>
<span data-ttu-id="c6900-138">[Töltse le](https://code.visualstudio.com/docs/setup/osx) és a Visual Studio Code telepítése.</span><span class="sxs-lookup"><span data-stu-id="c6900-138">[Download](https://code.visualstudio.com/docs/setup/osx) and install Visual Studio Code.</span></span> <span data-ttu-id="c6900-139">A Visual Studio Code a Windows, Linux és macOS egy egyszerűsített, de hatékony forráskód szerkesztőjében.</span><span class="sxs-lookup"><span data-stu-id="c6900-139">Visual Studio Code is a lightweight but powerful source code editor for Windows, Linux, and macOS.</span></span> <span data-ttu-id="c6900-140">A szerkesztő később hello oktatóanyag tooedit hello mintakód használható.</span><span class="sxs-lookup"><span data-stu-id="c6900-140">You use this editor later in hello tutorial tooedit hello sample code.</span></span>

## <a name="summary"></a><span data-ttu-id="c6900-141">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="c6900-141">Summary</span></span>
<span data-ttu-id="c6900-142">Szükséges hello fejlesztői eszközök és szoftverek hello első mintaalkalmazás telepítése.</span><span class="sxs-lookup"><span data-stu-id="c6900-142">You've installed hello required development tools and software for hello first sample application.</span></span> <span data-ttu-id="c6900-143">hello tovább feladat toocreate, telepítése, és futtassa a hello mintaalkalmazást a Pi.</span><span class="sxs-lookup"><span data-stu-id="c6900-143">hello next task is toocreate, deploy, and run hello sample application on Pi.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c6900-144">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c6900-144">Next steps</span></span>
[<span data-ttu-id="c6900-145">Hello villogási alkalmazás létrehozását és telepítését</span><span class="sxs-lookup"><span data-stu-id="c6900-145">Create and deploy hello blink application</span></span>](iot-hub-raspberry-pi-kit-c-lesson1-deploy-blink-app.md)

