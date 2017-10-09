---
title: "Intel Edison (csomópont) tooAzure IoT - lecke 1 Kapcsolódás: eszközök (macOS) beszerzése |} Microsoft Docs"
description: "Töltse le és telepítse hello szükséges eszközök és szoftverek hello első mintaalkalmazás Edison macOS."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "arduino fejlesztőeszközök, iot fejlesztési, iot szoftver, internet dolgot szoftver, telepítse git mac, telepítés csomópont js mac gépen"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-node-get-started
ms.assetid: fb6742be-2825-4524-89f7-8ccb7e7f1de1
ms.service: iot-hub
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: f32c0ea3c69eb2f912171fd694ae883d2586c72e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-tools-macos-1010"></a><span data-ttu-id="6c555-104">Hello eszközök (macOS 10.10) beolvasása</span><span class="sxs-lookup"><span data-stu-id="6c555-104">Get hello tools (macOS 10.10)</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="6c555-105">[Windows 7 vagy újabb][windows]</span><span class="sxs-lookup"><span data-stu-id="6c555-105">[Windows 7 or later][windows]</span></span>
> * <span data-ttu-id="6c555-106">[Ubuntu 16.04][ubuntu]</span><span class="sxs-lookup"><span data-stu-id="6c555-106">[Ubuntu 16.04][ubuntu]</span></span>
> * <span data-ttu-id="6c555-107">[macOS 10.10][macos]</span><span class="sxs-lookup"><span data-stu-id="6c555-107">[macOS 10.10][macos]</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="6c555-108">Mit fog</span><span class="sxs-lookup"><span data-stu-id="6c555-108">What you will do</span></span>
<span data-ttu-id="6c555-109">Hello Fejlesztőeszközök és hello első mintaalkalmazást az Intel Edison hello szoftver letöltése.</span><span class="sxs-lookup"><span data-stu-id="6c555-109">Download hello development tools and hello software for hello first sample application for your Intel Edison.</span></span> <span data-ttu-id="6c555-110">Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="6c555-110">If you have any problems, look for solutions on hello [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="6c555-111">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="6c555-111">What you will learn</span></span>
<span data-ttu-id="6c555-112">Ebből a cikkből megtudhatja:</span><span class="sxs-lookup"><span data-stu-id="6c555-112">In this article, you will learn:</span></span>

* <span data-ttu-id="6c555-113">Hogyan tooinstall a Git szoftver, Node.js.</span><span class="sxs-lookup"><span data-stu-id="6c555-113">How tooinstall Git and Node.js.</span></span>
  * <span data-ttu-id="6c555-114">[Git](https://git-scm.com) van egy nyílt forráskódú elosztott verziókezelő rendszer.</span><span class="sxs-lookup"><span data-stu-id="6c555-114">[Git](https://git-scm.com) is an open source distributed version control system.</span></span> <span data-ttu-id="6c555-115">Ez a cikk hello-mintaalkalmazás Git tárolja.</span><span class="sxs-lookup"><span data-stu-id="6c555-115">hello sample application for this article is stored on Git.</span></span>
  * <span data-ttu-id="6c555-116">[NODE.js](https://nodejs.org/en/) van a JavaScript futásidejű és gazdag csomag-ökoszisztéma.</span><span class="sxs-lookup"><span data-stu-id="6c555-116">[Node.js](https://nodejs.org/en/) is a JavaScript runtime with a rich package ecosystem.</span></span>
* <span data-ttu-id="6c555-117">Hogyan toouse NPM tooinstall további Node.js fejlesztői eszközök.</span><span class="sxs-lookup"><span data-stu-id="6c555-117">How toouse NPM tooinstall additional Node.js development tools.</span></span>
  * <span data-ttu-id="6c555-118">hello minimálisan szükséges verziója Node.js 4.5-ös LTS.</span><span class="sxs-lookup"><span data-stu-id="6c555-118">hello minimum required version of Node.js is 4.5 LTS.</span></span>
  * <span data-ttu-id="6c555-119">[NPM](https://www.npmjs.com) egyike hello Node.js csomag feletteseit.</span><span class="sxs-lookup"><span data-stu-id="6c555-119">[NPM](https://www.npmjs.com) is one of hello package managers for Node.js.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="6c555-120">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="6c555-120">What you need</span></span>
<span data-ttu-id="6c555-121">toocomplete ennél a műveletnél, szüksége lesz:</span><span class="sxs-lookup"><span data-stu-id="6c555-121">toocomplete this operation, you will need:</span></span>
* <span data-ttu-id="6c555-122">Az Internet kapcsolat toodownload hello Fejlesztőeszközök és hello szoftver.</span><span class="sxs-lookup"><span data-stu-id="6c555-122">An Internet connection toodownload hello development tools and hello software.</span></span>
* <span data-ttu-id="6c555-123">MacOS Yosemite (10.10) futtató Mac vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="6c555-123">A Mac that is running macOS Yosemite (10.10) or later.</span></span>

## <a name="install-git-and-nodejs"></a><span data-ttu-id="6c555-124">Telepítse a Git szoftver, Node.js</span><span class="sxs-lookup"><span data-stu-id="6c555-124">Install Git and Node.js</span></span>
<span data-ttu-id="6c555-125">tooinstall Git és a Node.js, használja a hello [Homebrew](http://brew.sh) segédprogramja csomag a következő lépések végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="6c555-125">tooinstall Git and Node.js, use hello [Homebrew](http://brew.sh) package management utility by following these steps:</span></span>

1. <span data-ttu-id="6c555-126">Telepítse a Homebrew.</span><span class="sxs-lookup"><span data-stu-id="6c555-126">Install Homebrew.</span></span> <span data-ttu-id="6c555-127">Ha már telepített Homebrew, nyissa meg toostep 2.</span><span class="sxs-lookup"><span data-stu-id="6c555-127">If you've already installed Homebrew, go toostep 2.</span></span>

   1. <span data-ttu-id="6c555-128">Nyomja le az `Cmd + Space` , és írja be `Terminal` tooopen egy terminált.</span><span class="sxs-lookup"><span data-stu-id="6c555-128">Press `Cmd + Space` and enter `Terminal` tooopen a terminal.</span></span>
   2. <span data-ttu-id="6c555-129">Futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="6c555-129">Run hello following command:</span></span>

      ```bash
      /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
      ```
2. <span data-ttu-id="6c555-130">Telepítse a Git és Node.js hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="6c555-130">Install Git and Node.js by running hello following command:</span></span>

   ```bash
   brew install node git
   ```

## <a name="install-additional-nodejs-development-tools"></a><span data-ttu-id="6c555-131">További Node.js fejlesztői eszközök telepítése</span><span class="sxs-lookup"><span data-stu-id="6c555-131">Install additional Node.js development tools</span></span>
<span data-ttu-id="6c555-132">Használjon [gulp.js](http://gulpjs.com) hello minta alkalmazás tooyour Edison tooautomate hello központi telepítését.</span><span class="sxs-lookup"><span data-stu-id="6c555-132">Use [gulp.js](http://gulpjs.com) tooautomate hello deployment of hello sample application tooyour Edison.</span></span>

<span data-ttu-id="6c555-133">Telepítés `gulp` hello hello terminálban parancs a következő futtatásával:</span><span class="sxs-lookup"><span data-stu-id="6c555-133">Install `gulp` by running hello following command in hello terminal:</span></span>

```bash
sudo npm install -g gulp
```

<span data-ttu-id="6c555-134">Ha problémák macOS Node.js és a további fejlesztői eszközök telepítése, lásd: hello [hibaelhárítási útmutatója] [ troubleshooting] a megoldások toocommon problémákat.</span><span class="sxs-lookup"><span data-stu-id="6c555-134">If you experience issues installing Node.js and these additional development tools on macOS, see hello [troubleshooting guide][troubleshooting] for solutions toocommon problems.</span></span>

## <a name="install-visual-studio-code"></a><span data-ttu-id="6c555-135">Visual Studio Code telepítése</span><span class="sxs-lookup"><span data-stu-id="6c555-135">Install Visual Studio Code</span></span>
<span data-ttu-id="6c555-136">[Töltse le](https://code.visualstudio.com/docs/setup/osx) és a Visual Studio Code telepítése.</span><span class="sxs-lookup"><span data-stu-id="6c555-136">[Download](https://code.visualstudio.com/docs/setup/osx) and install Visual Studio Code.</span></span> <span data-ttu-id="6c555-137">A Visual Studio Code a Windows, Linux és macOS egy egyszerűsített, de hatékony forráskód szerkesztőjében.</span><span class="sxs-lookup"><span data-stu-id="6c555-137">Visual Studio Code is a lightweight but powerful source code editor for Windows, Linux, and macOS.</span></span> <span data-ttu-id="6c555-138">A szerkesztő később hello oktatóanyag tooedit hello mintakód használható.</span><span class="sxs-lookup"><span data-stu-id="6c555-138">You use this editor later in hello tutorial tooedit hello sample code.</span></span>

## <a name="summary"></a><span data-ttu-id="6c555-139">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="6c555-139">Summary</span></span>
<span data-ttu-id="6c555-140">Szükséges hello fejlesztői eszközök és szoftverek hello első mintaalkalmazás telepítése.</span><span class="sxs-lookup"><span data-stu-id="6c555-140">You've installed hello required development tools and software for hello first sample application.</span></span> <span data-ttu-id="6c555-141">hello tovább feladat toocreate, telepítése, és a Edison hello mintaalkalmazás futtatása.</span><span class="sxs-lookup"><span data-stu-id="6c555-141">hello next task is toocreate, deploy, and run hello sample application on Edison.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6c555-142">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6c555-142">Next steps</span></span>
<span data-ttu-id="6c555-143">[Hello villogási alkalmazás létrehozását és telepítését][create-and-deploy-the-blink-application]</span><span class="sxs-lookup"><span data-stu-id="6c555-143">[Create and deploy hello blink application][create-and-deploy-the-blink-application]</span></span>
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[create-and-deploy-the-blink-application]: iot-hub-intel-edison-kit-node-lesson1-deploy-blink-app.md
[windows]: iot-hub-intel-edison-kit-node-lesson1-get-the-tools-win32.md
[ubuntu]: iot-hub-intel-edison-kit-node-lesson1-get-the-tools-ubuntu.md
[macos]: iot-hub-intel-edison-kit-node-lesson1-get-the-tools-mac.md
