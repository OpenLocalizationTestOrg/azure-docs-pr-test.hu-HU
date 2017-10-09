---
title: "Connect Intel Edison (C) tooAzure IoT - lecke 1: eszközök (Ubuntu) beszerzése |} Microsoft Docs"
description: "Töltse le és Ubuntu hello szükséges eszközök és a szoftverek hello első mintaalkalmazás Edison telepítéséhez."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "arduino fejlesztői eszközök, az iot-fejlesztési, iot szoftver, internet dolgot szoftver, az ubuntu, telepítés csomópont js ubuntu telepítés git"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-c-get-started
ms.assetid: 4c7b8e04-b892-459b-8b03-85bcaff2465c
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 1ebd599def4e8bb33d891517cc76bc2fcdc3c35a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-tools-ubuntu-1604"></a><span data-ttu-id="28dab-104">Hello eszközök (Ubuntu 16.04) beolvasása</span><span class="sxs-lookup"><span data-stu-id="28dab-104">Get hello tools (Ubuntu 16.04)</span></span>

> [!div class="op_single_selector"]
> * <span data-ttu-id="28dab-105">[Windows 7 vagy újabb][windows]</span><span class="sxs-lookup"><span data-stu-id="28dab-105">[Windows 7 or later][windows]</span></span>
> * <span data-ttu-id="28dab-106">[Ubuntu 16.04][ubuntu]</span><span class="sxs-lookup"><span data-stu-id="28dab-106">[Ubuntu 16.04][ubuntu]</span></span>
> * <span data-ttu-id="28dab-107">[macOS 10.10][macos]</span><span class="sxs-lookup"><span data-stu-id="28dab-107">[macOS 10.10][macos]</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="28dab-108">Mit fog</span><span class="sxs-lookup"><span data-stu-id="28dab-108">What you will do</span></span>
<span data-ttu-id="28dab-109">Hello Fejlesztőeszközök és hello első mintaalkalmazást az Intel Edison hello szoftver letöltése.</span><span class="sxs-lookup"><span data-stu-id="28dab-109">Download hello development tools and hello software for hello first sample application for your Intel Edison.</span></span> <span data-ttu-id="28dab-110">Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="28dab-110">If you have any problems, look for solutions on hello [troubleshooting page][troubleshooting].</span></span>

> [!NOTE]
> <span data-ttu-id="28dab-111">Bár programozási nyelv hello fő logikájának hello C, Node.js eszközök hello során tapasztalatokat toobuild használt alkalmazások és központi telepítésekor minta.</span><span class="sxs-lookup"><span data-stu-id="28dab-111">Although hello programming language of hello main logic is C, Node.js tools are used in hello lessons toobuild and deploy sample applications.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="28dab-112">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="28dab-112">What you will learn</span></span>
<span data-ttu-id="28dab-113">Ebből a cikkből megtudhatja:</span><span class="sxs-lookup"><span data-stu-id="28dab-113">In this article, you will learn:</span></span>

* <span data-ttu-id="28dab-114">Hogyan tooinstall a Git szoftver, Node.js</span><span class="sxs-lookup"><span data-stu-id="28dab-114">How tooinstall Git and Node.js</span></span>
  * <span data-ttu-id="28dab-115">[Git](https://git-scm.com) van egy nyílt forráskódú elosztott verziókezelő rendszer.</span><span class="sxs-lookup"><span data-stu-id="28dab-115">[Git](https://git-scm.com) is an open source distributed version control system.</span></span> <span data-ttu-id="28dab-116">Ez a cikk hello-mintaalkalmazás Git tárolja.</span><span class="sxs-lookup"><span data-stu-id="28dab-116">hello sample application for this article is stored on Git.</span></span>
  * <span data-ttu-id="28dab-117">[NODE.js](https://nodejs.org/en/) van a JavaScript futásidejű és gazdag csomag-ökoszisztéma.</span><span class="sxs-lookup"><span data-stu-id="28dab-117">[Node.js](https://nodejs.org/en/) is a JavaScript runtime with a rich package ecosystem.</span></span>
* <span data-ttu-id="28dab-118">Hogyan toouse NPM tooinstall további Node.js fejlesztői eszközök.</span><span class="sxs-lookup"><span data-stu-id="28dab-118">How toouse NPM tooinstall additional Node.js development tools.</span></span>
  * <span data-ttu-id="28dab-119">hello minimálisan szükséges verziója Node.js 4.5-ös LTS.</span><span class="sxs-lookup"><span data-stu-id="28dab-119">hello minimum required version of Node.js is 4.5 LTS.</span></span>
  * <span data-ttu-id="28dab-120">[NPM](https://www.npmjs.com) egyike hello Node.js csomag feletteseit.</span><span class="sxs-lookup"><span data-stu-id="28dab-120">[NPM](https://www.npmjs.com) is one of hello package managers for Node.js.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="28dab-121">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="28dab-121">What you need</span></span>
<span data-ttu-id="28dab-122">toocomplete ennél a műveletnél, szüksége lesz:</span><span class="sxs-lookup"><span data-stu-id="28dab-122">toocomplete this operation, you will need:</span></span>
* <span data-ttu-id="28dab-123">Az Internet kapcsolat toodownload hello Fejlesztőeszközök és hello szoftver.</span><span class="sxs-lookup"><span data-stu-id="28dab-123">An Internet connection toodownload hello development tools and hello software.</span></span>
* <span data-ttu-id="28dab-124">Ubuntu 16.04 vagy újabb rendszerrel működő számítógép.</span><span class="sxs-lookup"><span data-stu-id="28dab-124">A computer that is running Ubuntu 16.04 or later.</span></span>

## <a name="install-git-nodejs-and-npm"></a><span data-ttu-id="28dab-125">Telepítse a Git, Node.js és NPM</span><span class="sxs-lookup"><span data-stu-id="28dab-125">Install Git, Node.js, and NPM</span></span>
<span data-ttu-id="28dab-126">Használjon hello billentyűparancsot `Ctrl + Alt + T` tooopen egy terminál és futtatási hello a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="28dab-126">Use hello keyboard shortcut `Ctrl + Alt + T` tooopen a terminal and run hello following commands:</span></span>

```bash
sudo apt-get update
curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
sudo apt-get install -y nodejs
sudo apt-get install git
```

## <a name="install-additional-nodejs-development-tools"></a><span data-ttu-id="28dab-127">További Node.js fejlesztői eszközök telepítése</span><span class="sxs-lookup"><span data-stu-id="28dab-127">Install additional Node.js development tools</span></span>
<span data-ttu-id="28dab-128">Használjon [gulp.js](http://gulpjs.com) hello minta alkalmazás tooEdison tooautomate hello központi telepítését.</span><span class="sxs-lookup"><span data-stu-id="28dab-128">Use [gulp.js](http://gulpjs.com) tooautomate hello deployment of hello sample application tooEdison.</span></span>

<span data-ttu-id="28dab-129">Telepítés `gulp` hello hello terminálban parancs a következő futtatásával:</span><span class="sxs-lookup"><span data-stu-id="28dab-129">Install `gulp` by running hello following command in hello terminal:</span></span>

```bash
sudo npm install -g gulp
```

<span data-ttu-id="28dab-130">Ha problémák Ubuntu Node.js és a további fejlesztői eszközök telepítése, lásd: hello [hibaelhárítási útmutatója] [ troubleshooting] a megoldások toocommon problémákat.</span><span class="sxs-lookup"><span data-stu-id="28dab-130">If you experience issues installing Node.js and these additional development tools on Ubuntu, see hello [troubleshooting guide][troubleshooting] for solutions toocommon problems.</span></span>

## <a name="install-visual-studio-code"></a><span data-ttu-id="28dab-131">Visual Studio Code telepítése</span><span class="sxs-lookup"><span data-stu-id="28dab-131">Install Visual Studio Code</span></span>
<span data-ttu-id="28dab-132">[Töltse le](https://code.visualstudio.com/docs/setup/linux) és a Visual Studio Code telepítése.</span><span class="sxs-lookup"><span data-stu-id="28dab-132">[Download](https://code.visualstudio.com/docs/setup/linux) and install Visual Studio Code.</span></span> <span data-ttu-id="28dab-133">A Visual Studio Code a Windows, Linux és macOS egy egyszerűsített, de hatékony forráskód szerkesztőjében.</span><span class="sxs-lookup"><span data-stu-id="28dab-133">Visual Studio Code is a lightweight but powerful source code editor for Windows, Linux, and macOS.</span></span> <span data-ttu-id="28dab-134">A szerkesztő később hello oktatóanyag tooedit hello mintakód használható.</span><span class="sxs-lookup"><span data-stu-id="28dab-134">You use this editor later in hello tutorial tooedit hello sample code.</span></span>

## <a name="summary"></a><span data-ttu-id="28dab-135">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="28dab-135">Summary</span></span>
<span data-ttu-id="28dab-136">Szükséges hello fejlesztői eszközök és szoftverek hello első mintaalkalmazás telepítése.</span><span class="sxs-lookup"><span data-stu-id="28dab-136">You've installed hello required development tools and software for hello first sample application.</span></span> <span data-ttu-id="28dab-137">hello tovább feladat toocreate, telepítése, és a Edison hello mintaalkalmazás futtatása.</span><span class="sxs-lookup"><span data-stu-id="28dab-137">hello next task is toocreate, deploy, and run hello sample application on Edison.</span></span>

## <a name="next-steps"></a><span data-ttu-id="28dab-138">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="28dab-138">Next steps</span></span>
<span data-ttu-id="28dab-139">[Hello villogási minta alkalmazás létrehozását és telepítését][create-and-deploy-the-blink-application]</span><span class="sxs-lookup"><span data-stu-id="28dab-139">[Create and deploy hello blink sample application][create-and-deploy-the-blink-application]</span></span>

<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md
[create-and-deploy-the-blink-application]: iot-hub-intel-edison-kit-c-lesson1-deploy-blink-app.md
[windows]: iot-hub-intel-edison-kit-c-lesson1-get-the-tools-win32.md
[ubuntu]: iot-hub-intel-edison-kit-c-lesson1-get-the-tools-ubuntu.md
[macos]: iot-hub-intel-edison-kit-c-lesson1-get-the-tools-mac.md
