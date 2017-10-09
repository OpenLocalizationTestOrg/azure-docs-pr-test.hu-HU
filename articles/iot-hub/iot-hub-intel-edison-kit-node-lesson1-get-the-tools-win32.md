---
title: "Intel Edison (csomópont) tooAzure IoT - lecke 1 Kapcsolódás: Get tools (Windows) |} Microsoft Docs"
description: "Töltse le és hello szükséges eszközök és szoftverek hello első mintaalkalmazás Edison telepítéséhez Windows 7 és újabb verziók."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "arduino fejlesztőeszközök, iot fejlesztési, iot szoftver, internet dolgot szoftver, telepítse git csomópont js windows telepítése a windows rendszeren"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-node-get-started
ms.assetid: 4164b5a1-5a42-4d8a-9ff6-441e79fcc936
ms.service: iot-hub
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 933cc585d1b8b0236d76452f5c449ae9f2f3987b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-tools-windows-7-or-later"></a><span data-ttu-id="75623-104">Első hello eszközök (Windows 7 vagy újabb)</span><span class="sxs-lookup"><span data-stu-id="75623-104">Get hello tools (Windows 7 or later)</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="75623-105">[Windows 7 vagy újabb][windows]</span><span class="sxs-lookup"><span data-stu-id="75623-105">[Windows 7 or later][windows]</span></span>
> * <span data-ttu-id="75623-106">[Ubuntu 16.04][ubuntu]</span><span class="sxs-lookup"><span data-stu-id="75623-106">[Ubuntu 16.04][ubuntu]</span></span>
> * <span data-ttu-id="75623-107">[macOS 10.10][macos]</span><span class="sxs-lookup"><span data-stu-id="75623-107">[macOS 10.10][macos]</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="75623-108">Mit fog</span><span class="sxs-lookup"><span data-stu-id="75623-108">What you will do</span></span>
<span data-ttu-id="75623-109">Hello Fejlesztőeszközök és hello első mintaalkalmazás Intel Edison hello szoftver letöltése.</span><span class="sxs-lookup"><span data-stu-id="75623-109">Download hello development tools and hello software for hello first sample application for Intel Edison.</span></span> <span data-ttu-id="75623-110">Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="75623-110">If you have any problems, look for solutions on hello [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="75623-111">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="75623-111">What you will learn</span></span>
<span data-ttu-id="75623-112">Ebből a cikkből megtudhatja:</span><span class="sxs-lookup"><span data-stu-id="75623-112">In this article, you will learn:</span></span>

* <span data-ttu-id="75623-113">Hogyan tooinstall a Git szoftver, Node.js.</span><span class="sxs-lookup"><span data-stu-id="75623-113">How tooinstall Git and Node.js.</span></span>
  * <span data-ttu-id="75623-114">[Git](https://git-scm.com) van egy nyílt forráskódú elosztott verziókezelő rendszer.</span><span class="sxs-lookup"><span data-stu-id="75623-114">[Git](https://git-scm.com) is an open source distributed version control system.</span></span> <span data-ttu-id="75623-115">Ez a cikk hello-mintaalkalmazás Git tárolja.</span><span class="sxs-lookup"><span data-stu-id="75623-115">hello sample application for this article is stored on Git.</span></span>
  * <span data-ttu-id="75623-116">[NODE.js](https://nodejs.org/en/) van a JavaScript futásidejű és gazdag csomag-ökoszisztéma.</span><span class="sxs-lookup"><span data-stu-id="75623-116">[Node.js](https://nodejs.org/en/) is a JavaScript runtime with a rich package ecosystem.</span></span>
* <span data-ttu-id="75623-117">Hogyan toouse NPM tooinstall további Node.js fejlesztői eszközök.</span><span class="sxs-lookup"><span data-stu-id="75623-117">How toouse NPM tooinstall additional Node.js development tools.</span></span>
  * <span data-ttu-id="75623-118">Node.js hello minimális verziójára vonatkozó követelményt a 4.5-ös LTS.</span><span class="sxs-lookup"><span data-stu-id="75623-118">hello minimum version requirement of Node.js is 4.5 LTS.</span></span>
  * <span data-ttu-id="75623-119">[NPM](https://www.npmjs.com) egyike hello Node.js csomag feletteseit.</span><span class="sxs-lookup"><span data-stu-id="75623-119">[NPM](https://www.npmjs.com) is one of hello package managers for Node.js.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="75623-120">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="75623-120">What you need</span></span>

<span data-ttu-id="75623-121">toocomplete ennél a műveletnél, szüksége lesz:</span><span class="sxs-lookup"><span data-stu-id="75623-121">toocomplete this operation, you will need:</span></span>

* <span data-ttu-id="75623-122">Az Internet kapcsolat toodownload hello Fejlesztőeszközök és hello szoftver.</span><span class="sxs-lookup"><span data-stu-id="75623-122">An Internet connection toodownload hello development tools and hello software.</span></span>
* <span data-ttu-id="75623-123">Windows rendszerű számítógép.</span><span class="sxs-lookup"><span data-stu-id="75623-123">A computer that is running Windows.</span></span>

## <a name="install-git-and-nodejs"></a><span data-ttu-id="75623-124">Telepítse a Git szoftver, Node.js</span><span class="sxs-lookup"><span data-stu-id="75623-124">Install Git and Node.js</span></span>

<span data-ttu-id="75623-125">Alább toodownload hello hivatkozásaira kattint, és telepítse a Git és a Node.js-es lts verzió a Windows.</span><span class="sxs-lookup"><span data-stu-id="75623-125">Click hello links below toodownload and install Git and Node.js LTS for Windows.</span></span>

* [<span data-ttu-id="75623-126">Git letöltése a Windows rendszerhez</span><span class="sxs-lookup"><span data-stu-id="75623-126">Get Git for Windows</span></span>](https://git-scm.com/download/win/)
* [<span data-ttu-id="75623-127">Node.js-es lts verzió letöltése a Windows rendszerhez</span><span class="sxs-lookup"><span data-stu-id="75623-127">Get Node.js LTS for Windows</span></span>](https://nodejs.org/en/)

## <a name="install-additional-nodejs-development-tools"></a><span data-ttu-id="75623-128">További Node.js fejlesztői eszközök telepítése</span><span class="sxs-lookup"><span data-stu-id="75623-128">Install additional Node.js development tools</span></span>

<span data-ttu-id="75623-129">Használjon [gulp.js](http://gulpjs.com) hello minta alkalmazás tooEdison tooautomate hello központi telepítését.</span><span class="sxs-lookup"><span data-stu-id="75623-129">Use [gulp.js](http://gulpjs.com) tooautomate hello deployment of hello sample application tooEdison.</span></span>

<span data-ttu-id="75623-130">Nyisson meg egy parancssort rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="75623-130">Start a command prompt as an administrator.</span></span> <span data-ttu-id="75623-131">Telepítés `gulp` hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="75623-131">Install `gulp` by running hello following command:</span></span>

```cmd
npm install -g gulp
```

<span data-ttu-id="75623-132">Ha problémák, Node.js és a további Node.js fejlesztői eszközök telepítése a számítógépre, lásd: hello [hibaelhárítási útmutatója] [ troubleshooting] a megoldások toocommon problémákat.</span><span class="sxs-lookup"><span data-stu-id="75623-132">If you experience issues installing Node.js and these additional Node.js development tools on your computer, see hello [troubleshooting guide][troubleshooting] for solutions toocommon problems.</span></span>

## <a name="install-visual-studio-code"></a><span data-ttu-id="75623-133">Visual Studio Code telepítése</span><span class="sxs-lookup"><span data-stu-id="75623-133">Install Visual Studio Code</span></span>

<span data-ttu-id="75623-134">[Töltse le](https://code.visualstudio.com/docs/setup/windows) és a Visual Studio Code telepítése.</span><span class="sxs-lookup"><span data-stu-id="75623-134">[Download](https://code.visualstudio.com/docs/setup/windows) and install Visual Studio Code.</span></span> <span data-ttu-id="75623-135">A Visual Studio Code a Windows, Linux és macOS egy egyszerűsített, de hatékony forráskód szerkesztőjében.</span><span class="sxs-lookup"><span data-stu-id="75623-135">Visual Studio Code is a lightweight but powerful source code editor for Windows, Linux, and macOS.</span></span> <span data-ttu-id="75623-136">A szerkesztő később hello oktatóanyag tooedit hello mintakód használható.</span><span class="sxs-lookup"><span data-stu-id="75623-136">You use this editor later in hello tutorial tooedit hello sample code.</span></span>

## <a name="summary"></a><span data-ttu-id="75623-137">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="75623-137">Summary</span></span>

<span data-ttu-id="75623-138">Szükséges hello fejlesztői eszközök és szoftverek hello első mintaalkalmazás telepítése.</span><span class="sxs-lookup"><span data-stu-id="75623-138">You've installed hello required development tools and software for hello first sample application.</span></span> <span data-ttu-id="75623-139">hello tovább feladat toocreate, telepítése, és a Edison hello mintaalkalmazás futtatása.</span><span class="sxs-lookup"><span data-stu-id="75623-139">hello next task is toocreate, deploy, and run hello sample application on Edison.</span></span>

## <a name="next-steps"></a><span data-ttu-id="75623-140">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="75623-140">Next steps</span></span>

<span data-ttu-id="75623-141">[Hello villogási alkalmazás létrehozását és telepítését][create-and-deploy-the-blink-application]</span><span class="sxs-lookup"><span data-stu-id="75623-141">[Create and deploy hello blink application][create-and-deploy-the-blink-application]</span></span>
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[create-and-deploy-the-blink-application]: iot-hub-intel-edison-kit-node-lesson1-deploy-blink-app.md
[windows]: iot-hub-intel-edison-kit-node-lesson1-get-the-tools-win32.md
[ubuntu]: iot-hub-intel-edison-kit-node-lesson1-get-the-tools-ubuntu.md
[macos]: iot-hub-intel-edison-kit-node-lesson1-get-the-tools-mac.md
