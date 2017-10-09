---
title: "Csatlakozás Arduino tooAzure IoT - lecke 1: eszközök (macOS) beszerzése |} Microsoft Docs"
description: "Töltse le és hello szükséges eszközök és szoftverek hello első mintaalkalmazás Adafruit lágyított M0 Wi-Fi telepítéséhez macOS."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "arduino fejlesztőeszközök, iot fejlesztési, iot szoftver, internet dolgot szoftver, telepítse git mac, telepítés csomópont js mac gépen"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: 0262f3dd-0259-4eb0-962f-9fb534f8359e
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 5fd306fecd7259fb8f1e99d76282a1e464c4d4f6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-tools-macos-1010"></a><span data-ttu-id="79dee-104">Hello eszközök (macOS 10.10) beolvasása</span><span class="sxs-lookup"><span data-stu-id="79dee-104">Get hello tools (macOS 10.10)</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="79dee-105">[Windows 7 vagy újabb][windows]</span><span class="sxs-lookup"><span data-stu-id="79dee-105">[Windows 7 or later][windows]</span></span>
> * <span data-ttu-id="79dee-106">[Ubuntu 16.04][ubuntu]</span><span class="sxs-lookup"><span data-stu-id="79dee-106">[Ubuntu 16.04][ubuntu]</span></span>
> * <span data-ttu-id="79dee-107">[macOS 10.10][macos]</span><span class="sxs-lookup"><span data-stu-id="79dee-107">[macOS 10.10][macos]</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="79dee-108">Mit fog</span><span class="sxs-lookup"><span data-stu-id="79dee-108">What you will do</span></span>

<span data-ttu-id="79dee-109">Hello Fejlesztőeszközök és hello szoftver hello a Adafruit lágyított M0 Wi-Fi Arduino kártya első mintaalkalmazás letöltése.</span><span class="sxs-lookup"><span data-stu-id="79dee-109">Download hello development tools and hello software for hello first sample application for your Adafruit Feather M0 WiFi Arduino board.</span></span> 

<span data-ttu-id="79dee-110">Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="79dee-110">If you have any problems, look for solutions on hello [troubleshooting page][troubleshooting].</span></span>

> [!NOTE]
> <span data-ttu-id="79dee-111">Bár programozási nyelv hello fő logikájának hello Arduino, Node.js eszközök hello során tapasztalatokat toobuild használt alkalmazások és központi telepítésekor minta.</span><span class="sxs-lookup"><span data-stu-id="79dee-111">Although hello programming language of hello main logic is Arduino, Node.js tools are used in hello lessons toobuild and deploy sample applications.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="79dee-112">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="79dee-112">What you will learn</span></span>
<span data-ttu-id="79dee-113">Ebből a cikkből megtudhatja:</span><span class="sxs-lookup"><span data-stu-id="79dee-113">In this article, you will learn:</span></span>

* <span data-ttu-id="79dee-114">Hogyan tooinstall a Git szoftver, Node.js.</span><span class="sxs-lookup"><span data-stu-id="79dee-114">How tooinstall Git and Node.js.</span></span>
  * <span data-ttu-id="79dee-115">[Git](https://git-scm.com) van egy nyílt forráskódú elosztott verziókezelő rendszer.</span><span class="sxs-lookup"><span data-stu-id="79dee-115">[Git](https://git-scm.com) is an open source distributed version control system.</span></span> <span data-ttu-id="79dee-116">Ez a cikk hello-mintaalkalmazás Git tárolja.</span><span class="sxs-lookup"><span data-stu-id="79dee-116">hello sample application for this article is stored on Git.</span></span>
  * <span data-ttu-id="79dee-117">[NODE.js](https://nodejs.org/en/) van a JavaScript futásidejű és gazdag csomag-ökoszisztéma.</span><span class="sxs-lookup"><span data-stu-id="79dee-117">[Node.js](https://nodejs.org/en/) is a JavaScript runtime with a rich package ecosystem.</span></span>
* <span data-ttu-id="79dee-118">Hogyan toouse NPM tooinstall további Node.js fejlesztői eszközök.</span><span class="sxs-lookup"><span data-stu-id="79dee-118">How toouse NPM tooinstall additional Node.js development tools.</span></span>
  * <span data-ttu-id="79dee-119">hello minimálisan szükséges verziója Node.js 4.5-ös LTS.</span><span class="sxs-lookup"><span data-stu-id="79dee-119">hello minimum required version of Node.js is 4.5 LTS.</span></span>
  * <span data-ttu-id="79dee-120">[NPM](https://www.npmjs.com) egyike hello Node.js csomag feletteseit.</span><span class="sxs-lookup"><span data-stu-id="79dee-120">[NPM](https://www.npmjs.com) is one of hello package managers for Node.js.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="79dee-121">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="79dee-121">What you need</span></span>
<span data-ttu-id="79dee-122">toocomplete ennél a műveletnél, szüksége lesz:</span><span class="sxs-lookup"><span data-stu-id="79dee-122">toocomplete this operation, you will need:</span></span>
* <span data-ttu-id="79dee-123">Az Internet kapcsolat toodownload hello Fejlesztőeszközök és hello szoftver.</span><span class="sxs-lookup"><span data-stu-id="79dee-123">An Internet connection toodownload hello development tools and hello software.</span></span>
* <span data-ttu-id="79dee-124">MacOS Yosemite (10.10) futtató Mac vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="79dee-124">A Mac that is running macOS Yosemite (10.10) or later.</span></span>

## <a name="install-git-and-nodejs"></a><span data-ttu-id="79dee-125">Telepítse a Git szoftver, Node.js</span><span class="sxs-lookup"><span data-stu-id="79dee-125">Install Git and Node.js</span></span>
<span data-ttu-id="79dee-126">tooinstall Git és a Node.js, használja a hello [Homebrew](http://brew.sh) segédprogramja csomag a következő lépések végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="79dee-126">tooinstall Git and Node.js, use hello [Homebrew](http://brew.sh) package management utility by following these steps:</span></span>

1. <span data-ttu-id="79dee-127">Telepítse a Homebrew.</span><span class="sxs-lookup"><span data-stu-id="79dee-127">Install Homebrew.</span></span> <span data-ttu-id="79dee-128">Ha már telepített Homebrew, nyissa meg toostep 2.</span><span class="sxs-lookup"><span data-stu-id="79dee-128">If you've already installed Homebrew, go toostep 2.</span></span>

   1. <span data-ttu-id="79dee-129">Nyomja le az `Cmd + Space` , és írja be `Terminal` tooopen egy terminált.</span><span class="sxs-lookup"><span data-stu-id="79dee-129">Press `Cmd + Space` and enter `Terminal` tooopen a terminal.</span></span>
   2. <span data-ttu-id="79dee-130">Futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="79dee-130">Run hello following command:</span></span>

      ```bash
      /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
      ```
2. <span data-ttu-id="79dee-131">Telepítse a Git és Node.js hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="79dee-131">Install Git and Node.js by running hello following command:</span></span>

   ```bash
   brew install node git
   ```

## <a name="install-additional-nodejs-development-tools"></a><span data-ttu-id="79dee-132">További Node.js fejlesztői eszközök telepítése</span><span class="sxs-lookup"><span data-stu-id="79dee-132">Install additional Node.js development tools</span></span>
<span data-ttu-id="79dee-133">Használjon [gulp.js](http://gulpjs.com) hello minta alkalmazás tooyour Arduino board tooautomate hello központi telepítését.</span><span class="sxs-lookup"><span data-stu-id="79dee-133">Use [gulp.js](http://gulpjs.com) tooautomate hello deployment of hello sample application tooyour Arduino board.</span></span>

<span data-ttu-id="79dee-134">Telepítés `gulp`, `device-discovery-cli` hello hello terminálban parancs a következő futtatásával:</span><span class="sxs-lookup"><span data-stu-id="79dee-134">Install `gulp`, `device-discovery-cli` by running hello following command in hello terminal:</span></span>

```bash
sudo npm install -g gulp device-discovery-cli
```

<span data-ttu-id="79dee-135">Ha problémák macOS Node.js és a további fejlesztői eszközök telepítése, lásd: hello [hibaelhárítási útmutatója] [ troubleshooting] a megoldások toocommon problémákat.</span><span class="sxs-lookup"><span data-stu-id="79dee-135">If you experience issues installing Node.js and these additional development tools on macOS, see hello [troubleshooting guide][troubleshooting] for solutions toocommon problems.</span></span>

## <a name="install-visual-studio-code"></a><span data-ttu-id="79dee-136">Visual Studio Code telepítése</span><span class="sxs-lookup"><span data-stu-id="79dee-136">Install Visual Studio Code</span></span>
<span data-ttu-id="79dee-137">[Töltse le](https://code.visualstudio.com/docs/setup/osx) és a Visual Studio Code telepítése.</span><span class="sxs-lookup"><span data-stu-id="79dee-137">[Download](https://code.visualstudio.com/docs/setup/osx) and install Visual Studio Code.</span></span> <span data-ttu-id="79dee-138">A Visual Studio Code a Windows, Linux és macOS egy egyszerűsített, de hatékony forráskód szerkesztőjében.</span><span class="sxs-lookup"><span data-stu-id="79dee-138">Visual Studio Code is a lightweight but powerful source code editor for Windows, Linux, and macOS.</span></span> <span data-ttu-id="79dee-139">A szerkesztő később hello oktatóanyag tooedit hello mintakód használható.</span><span class="sxs-lookup"><span data-stu-id="79dee-139">You use this editor later in hello tutorial tooedit hello sample code.</span></span>

## <a name="summary"></a><span data-ttu-id="79dee-140">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="79dee-140">Summary</span></span>
<span data-ttu-id="79dee-141">Szükséges hello fejlesztői eszközök és szoftverek hello első mintaalkalmazás telepítése.</span><span class="sxs-lookup"><span data-stu-id="79dee-141">You've installed hello required development tools and software for hello first sample application.</span></span> <span data-ttu-id="79dee-142">hello tovább feladat toocreate, telepítheti, és futtassa a hello mintaalkalmazást a Arduino táblán.</span><span class="sxs-lookup"><span data-stu-id="79dee-142">hello next task is toocreate, deploy, and run hello sample application on your Arduino board.</span></span>

## <a name="next-steps"></a><span data-ttu-id="79dee-143">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="79dee-143">Next steps</span></span>
<span data-ttu-id="79dee-144">[Hello villogási alkalmazás létrehozását és telepítését][create-and-deploy-the-blink-application]</span><span class="sxs-lookup"><span data-stu-id="79dee-144">[Create and deploy hello blink application][create-and-deploy-the-blink-application]</span></span>
<!-- Images and links -->

[windows]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-get-the-tools-win32.md
[ubuntu]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-get-the-tools-ubuntu.md
[macos]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-get-the-tools-mac.md
[troubleshooting]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md
[create-and-deploy-the-blink-application]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-deploy-blink-app.md