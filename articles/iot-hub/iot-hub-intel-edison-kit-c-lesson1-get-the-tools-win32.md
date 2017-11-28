---
title: 'Connect Intel Edison (C) tooAzure IoT - lecke 1: Get tools (Windows) |} Microsoft Docs'
description: "Töltse le és hello szükséges eszközök és szoftverek hello első mintaalkalmazás Edison telepítéséhez Windows 7 és újabb verziók."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "arduino fejlesztőeszközök, iot fejlesztési, iot szoftver, internet dolgot szoftver, telepítse git csomópont js windows telepítése a windows rendszeren"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-c-get-started
ms.assetid: 7d29a358-544d-4657-a504-5ed9b79c2925
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 64d8684ffcb858845de02276a11cf2b2e5c701a3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-tools-windows-7-or-later"></a><span data-ttu-id="e23de-104">Első hello eszközök (Windows 7 vagy újabb)</span><span class="sxs-lookup"><span data-stu-id="e23de-104">Get hello tools (Windows 7 or later)</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="e23de-105">[Windows 7 vagy újabb][windows]</span><span class="sxs-lookup"><span data-stu-id="e23de-105">[Windows 7 or later][windows]</span></span>
> * <span data-ttu-id="e23de-106">[Ubuntu 16.04][ubuntu]</span><span class="sxs-lookup"><span data-stu-id="e23de-106">[Ubuntu 16.04][ubuntu]</span></span>
> * <span data-ttu-id="e23de-107">[macOS 10.10][macos]</span><span class="sxs-lookup"><span data-stu-id="e23de-107">[macOS 10.10][macos]</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="e23de-108">Mit fog</span><span class="sxs-lookup"><span data-stu-id="e23de-108">What you will do</span></span>
<span data-ttu-id="e23de-109">Hello Fejlesztőeszközök és hello első mintaalkalmazás Intel Edison hello szoftver letöltése.</span><span class="sxs-lookup"><span data-stu-id="e23de-109">Download hello development tools and hello software for hello first sample application for Intel Edison.</span></span> <span data-ttu-id="e23de-110">Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="e23de-110">If you have any problems, look for solutions on hello [troubleshooting page][troubleshooting].</span></span>

> [!NOTE]
> <span data-ttu-id="e23de-111">Bár programozási nyelv hello fő logikájának hello C, Node.js eszközök hello során tapasztalatokat toobuild használt alkalmazások és központi telepítésekor minta.</span><span class="sxs-lookup"><span data-stu-id="e23de-111">Although hello programming language of hello main logic is C, Node.js tools are used in hello lessons toobuild and deploy sample applications.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="e23de-112">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="e23de-112">What you will learn</span></span>
<span data-ttu-id="e23de-113">Ebből a cikkből megtudhatja:</span><span class="sxs-lookup"><span data-stu-id="e23de-113">In this article, you will learn:</span></span>

* <span data-ttu-id="e23de-114">Hogyan tooinstall a Git szoftver, Node.js.</span><span class="sxs-lookup"><span data-stu-id="e23de-114">How tooinstall Git and Node.js.</span></span>
  * <span data-ttu-id="e23de-115">[Git](https://git-scm.com) van egy nyílt forráskódú elosztott verziókezelő rendszer.</span><span class="sxs-lookup"><span data-stu-id="e23de-115">[Git](https://git-scm.com) is an open source distributed version control system.</span></span> <span data-ttu-id="e23de-116">Ez a cikk hello-mintaalkalmazás Git tárolja.</span><span class="sxs-lookup"><span data-stu-id="e23de-116">hello sample application for this article is stored on Git.</span></span>
  * <span data-ttu-id="e23de-117">[NODE.js](https://nodejs.org/en/) van a JavaScript futásidejű és gazdag csomag-ökoszisztéma.</span><span class="sxs-lookup"><span data-stu-id="e23de-117">[Node.js](https://nodejs.org/en/) is a JavaScript runtime with a rich package ecosystem.</span></span>
* <span data-ttu-id="e23de-118">Hogyan toouse NPM tooinstall további Node.js fejlesztői eszközök.</span><span class="sxs-lookup"><span data-stu-id="e23de-118">How toouse NPM tooinstall additional Node.js development tools.</span></span>
  * <span data-ttu-id="e23de-119">Node.js hello minimális verziójára vonatkozó követelményt a 4.5-ös LTS.</span><span class="sxs-lookup"><span data-stu-id="e23de-119">hello minimum version requirement of Node.js is 4.5 LTS.</span></span>
  * <span data-ttu-id="e23de-120">[NPM](https://www.npmjs.com) egyike hello Node.js csomag feletteseit.</span><span class="sxs-lookup"><span data-stu-id="e23de-120">[NPM](https://www.npmjs.com) is one of hello package managers for Node.js.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="e23de-121">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="e23de-121">What you need</span></span>

<span data-ttu-id="e23de-122">toocomplete ennél a műveletnél, szüksége lesz:</span><span class="sxs-lookup"><span data-stu-id="e23de-122">toocomplete this operation, you will need:</span></span>

* <span data-ttu-id="e23de-123">Az Internet kapcsolat toodownload hello Fejlesztőeszközök és hello szoftver.</span><span class="sxs-lookup"><span data-stu-id="e23de-123">An Internet connection toodownload hello development tools and hello software.</span></span>
* <span data-ttu-id="e23de-124">Windows rendszerű számítógép.</span><span class="sxs-lookup"><span data-stu-id="e23de-124">A computer that is running Windows.</span></span>

## <a name="install-git-and-nodejs"></a><span data-ttu-id="e23de-125">Telepítse a Git szoftver, Node.js</span><span class="sxs-lookup"><span data-stu-id="e23de-125">Install Git and Node.js</span></span>

<span data-ttu-id="e23de-126">Alább toodownload hello hivatkozásaira kattint, és telepítse a Git és a Node.js-es lts verzió a Windows.</span><span class="sxs-lookup"><span data-stu-id="e23de-126">Click hello links below toodownload and install Git and Node.js LTS for Windows.</span></span>

* [<span data-ttu-id="e23de-127">Git letöltése a Windows rendszerhez</span><span class="sxs-lookup"><span data-stu-id="e23de-127">Get Git for Windows</span></span>](https://git-scm.com/download/win/)
* [<span data-ttu-id="e23de-128">Node.js-es lts verzió letöltése a Windows rendszerhez</span><span class="sxs-lookup"><span data-stu-id="e23de-128">Get Node.js LTS for Windows</span></span>](https://nodejs.org/en/)

## <a name="install-additional-nodejs-development-tools"></a><span data-ttu-id="e23de-129">További Node.js fejlesztői eszközök telepítése</span><span class="sxs-lookup"><span data-stu-id="e23de-129">Install additional Node.js development tools</span></span>

<span data-ttu-id="e23de-130">Használjon [gulp.js](http://gulpjs.com) hello minta alkalmazás tooEdison tooautomate hello központi telepítését.</span><span class="sxs-lookup"><span data-stu-id="e23de-130">Use [gulp.js](http://gulpjs.com) tooautomate hello deployment of hello sample application tooEdison.</span></span>

<span data-ttu-id="e23de-131">Nyisson meg egy parancssort rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="e23de-131">Start a command prompt as an administrator.</span></span> <span data-ttu-id="e23de-132">Telepítés `gulp` hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="e23de-132">Install `gulp` by running hello following command:</span></span>

```cmd
npm install -g gulp
```

<span data-ttu-id="e23de-133">Ha problémák, Node.js és a további Node.js fejlesztői eszközök telepítése a számítógépre, lásd: hello [hibaelhárítási útmutatója] [ troubleshooting] a megoldások toocommon problémákat.</span><span class="sxs-lookup"><span data-stu-id="e23de-133">If you experience issues installing Node.js and these additional Node.js development tools on your computer, see hello [troubleshooting guide][troubleshooting] for solutions toocommon problems.</span></span>

## <a name="install-visual-studio-code"></a><span data-ttu-id="e23de-134">Visual Studio Code telepítése</span><span class="sxs-lookup"><span data-stu-id="e23de-134">Install Visual Studio Code</span></span>

<span data-ttu-id="e23de-135">[Töltse le](https://code.visualstudio.com/docs/setup/windows) és a Visual Studio Code telepítése.</span><span class="sxs-lookup"><span data-stu-id="e23de-135">[Download](https://code.visualstudio.com/docs/setup/windows) and install Visual Studio Code.</span></span> <span data-ttu-id="e23de-136">A Visual Studio Code a Windows, Linux és macOS egy egyszerűsített, de hatékony forráskód szerkesztőjében.</span><span class="sxs-lookup"><span data-stu-id="e23de-136">Visual Studio Code is a lightweight but powerful source code editor for Windows, Linux, and macOS.</span></span> <span data-ttu-id="e23de-137">A szerkesztő később hello oktatóanyag tooedit hello mintakód használható.</span><span class="sxs-lookup"><span data-stu-id="e23de-137">You use this editor later in hello tutorial tooedit hello sample code.</span></span>

## <a name="summary"></a><span data-ttu-id="e23de-138">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="e23de-138">Summary</span></span>

<span data-ttu-id="e23de-139">Szükséges hello fejlesztői eszközök és szoftverek hello első mintaalkalmazás telepítése.</span><span class="sxs-lookup"><span data-stu-id="e23de-139">You've installed hello required development tools and software for hello first sample application.</span></span> <span data-ttu-id="e23de-140">hello tovább feladat toocreate, telepítése, és a Edison hello mintaalkalmazás futtatása.</span><span class="sxs-lookup"><span data-stu-id="e23de-140">hello next task is toocreate, deploy, and run hello sample application on Edison.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e23de-141">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e23de-141">Next steps</span></span>

<span data-ttu-id="e23de-142">[Hello villogási alkalmazás létrehozását és telepítését][create-and-deploy-the-blink-application]</span><span class="sxs-lookup"><span data-stu-id="e23de-142">[Create and deploy hello blink application][create-and-deploy-the-blink-application]</span></span>

<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md
[create-and-deploy-the-blink-application]: iot-hub-intel-edison-kit-c-lesson1-deploy-blink-app.md
[windows]: iot-hub-intel-edison-kit-c-lesson1-get-the-tools-win32.md
[ubuntu]: iot-hub-intel-edison-kit-c-lesson1-get-the-tools-ubuntu.md
[macos]: iot-hub-intel-edison-kit-c-lesson1-get-the-tools-mac.md
