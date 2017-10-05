---
title: 'Az Azure IoT - lecke 1 Connect Intel Edison (C): Get tools (Windows) |} Microsoft Docs'
description: "Töltse le és telepítse a szükséges eszközök és szoftverek Edison első minta alkalmazásához Windows 7 és újabb verziók."
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
ms.openlocfilehash: f9d614d17f262b81a75d6128cbc5898dc18ab906
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-the-tools-windows-7-or-later"></a><span data-ttu-id="c9b5b-104">Szerezze be az eszközöket (Windows 7 vagy újabb)</span><span class="sxs-lookup"><span data-stu-id="c9b5b-104">Get the tools (Windows 7 or later)</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="c9b5b-105">[Windows 7 vagy újabb][windows]</span><span class="sxs-lookup"><span data-stu-id="c9b5b-105">[Windows 7 or later][windows]</span></span>
> * <span data-ttu-id="c9b5b-106">[Ubuntu 16.04][ubuntu]</span><span class="sxs-lookup"><span data-stu-id="c9b5b-106">[Ubuntu 16.04][ubuntu]</span></span>
> * <span data-ttu-id="c9b5b-107">[macOS 10.10][macos]</span><span class="sxs-lookup"><span data-stu-id="c9b5b-107">[macOS 10.10][macos]</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="c9b5b-108">Mit fog</span><span class="sxs-lookup"><span data-stu-id="c9b5b-108">What you will do</span></span>
<span data-ttu-id="c9b5b-109">A fejlesztői eszközök és a szoftver az Intel Edison első mintaalkalmazás letöltése.</span><span class="sxs-lookup"><span data-stu-id="c9b5b-109">Download the development tools and the software for the first sample application for Intel Edison.</span></span> <span data-ttu-id="c9b5b-110">Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="c9b5b-110">If you have any problems, look for solutions on the [troubleshooting page][troubleshooting].</span></span>

> [!NOTE]
> <span data-ttu-id="c9b5b-111">Bár a programozási nyelv, a fő logikájának C, Node.js eszközök szerepelnek a megszerzett létrehozásához és központi telepítéséhez alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="c9b5b-111">Although the programming language of the main logic is C, Node.js tools are used in the lessons to build and deploy sample applications.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="c9b5b-112">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="c9b5b-112">What you will learn</span></span>
<span data-ttu-id="c9b5b-113">Ebből a cikkből megtudhatja:</span><span class="sxs-lookup"><span data-stu-id="c9b5b-113">In this article, you will learn:</span></span>

* <span data-ttu-id="c9b5b-114">Hogyan kell telepíteni a Git és Node.js.</span><span class="sxs-lookup"><span data-stu-id="c9b5b-114">How to install Git and Node.js.</span></span>
  * <span data-ttu-id="c9b5b-115">[Git](https://git-scm.com) van egy nyílt forráskódú elosztott verziókezelő rendszer.</span><span class="sxs-lookup"><span data-stu-id="c9b5b-115">[Git](https://git-scm.com) is an open source distributed version control system.</span></span> <span data-ttu-id="c9b5b-116">Ez a cikk a mintaalkalmazás Git tárolja.</span><span class="sxs-lookup"><span data-stu-id="c9b5b-116">The sample application for this article is stored on Git.</span></span>
  * <span data-ttu-id="c9b5b-117">[NODE.js](https://nodejs.org/en/) van a JavaScript futásidejű és gazdag csomag-ökoszisztéma.</span><span class="sxs-lookup"><span data-stu-id="c9b5b-117">[Node.js](https://nodejs.org/en/) is a JavaScript runtime with a rich package ecosystem.</span></span>
* <span data-ttu-id="c9b5b-118">Hogyan további Node.js fejlesztői eszközök telepítése az NPM segítségével.</span><span class="sxs-lookup"><span data-stu-id="c9b5b-118">How to use NPM to install additional Node.js development tools.</span></span>
  * <span data-ttu-id="c9b5b-119">A Node.js minimális verziójára vonatkozó követelményt a 4.5-ös LTS.</span><span class="sxs-lookup"><span data-stu-id="c9b5b-119">The minimum version requirement of Node.js is 4.5 LTS.</span></span>
  * <span data-ttu-id="c9b5b-120">[NPM](https://www.npmjs.com) a csomag kezelők, a Node.js egyike.</span><span class="sxs-lookup"><span data-stu-id="c9b5b-120">[NPM](https://www.npmjs.com) is one of the package managers for Node.js.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="c9b5b-121">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="c9b5b-121">What you need</span></span>

<span data-ttu-id="c9b5b-122">A művelet elvégzéséhez szüksége lesz:</span><span class="sxs-lookup"><span data-stu-id="c9b5b-122">To complete this operation, you will need:</span></span>

* <span data-ttu-id="c9b5b-123">A fejlesztői eszközök és a szoftverfrissítések letöltése az internethez.</span><span class="sxs-lookup"><span data-stu-id="c9b5b-123">An Internet connection to download the development tools and the software.</span></span>
* <span data-ttu-id="c9b5b-124">Windows rendszerű számítógép.</span><span class="sxs-lookup"><span data-stu-id="c9b5b-124">A computer that is running Windows.</span></span>

## <a name="install-git-and-nodejs"></a><span data-ttu-id="c9b5b-125">Telepítse a Git szoftver, Node.js</span><span class="sxs-lookup"><span data-stu-id="c9b5b-125">Install Git and Node.js</span></span>

<span data-ttu-id="c9b5b-126">Töltse le és telepítse a Git szoftver, a Windows Node.js LTS az alábbi hivatkozásokra kattintva.</span><span class="sxs-lookup"><span data-stu-id="c9b5b-126">Click the links below to download and install Git and Node.js LTS for Windows.</span></span>

* [<span data-ttu-id="c9b5b-127">Git letöltése a Windows rendszerhez</span><span class="sxs-lookup"><span data-stu-id="c9b5b-127">Get Git for Windows</span></span>](https://git-scm.com/download/win/)
* [<span data-ttu-id="c9b5b-128">Node.js-es lts verzió letöltése a Windows rendszerhez</span><span class="sxs-lookup"><span data-stu-id="c9b5b-128">Get Node.js LTS for Windows</span></span>](https://nodejs.org/en/)

## <a name="install-additional-nodejs-development-tools"></a><span data-ttu-id="c9b5b-129">További Node.js fejlesztői eszközök telepítése</span><span class="sxs-lookup"><span data-stu-id="c9b5b-129">Install additional Node.js development tools</span></span>

<span data-ttu-id="c9b5b-130">Használjon [gulp.js](http://gulpjs.com) Edison a minta-alkalmazás központi telepítésének automatizálásához.</span><span class="sxs-lookup"><span data-stu-id="c9b5b-130">Use [gulp.js](http://gulpjs.com) to automate the deployment of the sample application to Edison.</span></span>

<span data-ttu-id="c9b5b-131">Nyisson meg egy parancssort rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="c9b5b-131">Start a command prompt as an administrator.</span></span> <span data-ttu-id="c9b5b-132">Telepítse `gulp` a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="c9b5b-132">Install `gulp` by running the following command:</span></span>

```cmd
npm install -g gulp
```

<span data-ttu-id="c9b5b-133">Ha problémák, Node.js és a további Node.js fejlesztői eszközök telepítése a számítógépre, tekintse meg a [hibaelhárítási útmutatója] [ troubleshooting] gyakori problémák megoldásainak.</span><span class="sxs-lookup"><span data-stu-id="c9b5b-133">If you experience issues installing Node.js and these additional Node.js development tools on your computer, see the [troubleshooting guide][troubleshooting] for solutions to common problems.</span></span>

## <a name="install-visual-studio-code"></a><span data-ttu-id="c9b5b-134">Visual Studio Code telepítése</span><span class="sxs-lookup"><span data-stu-id="c9b5b-134">Install Visual Studio Code</span></span>

<span data-ttu-id="c9b5b-135">[Töltse le](https://code.visualstudio.com/docs/setup/windows) és a Visual Studio Code telepítése.</span><span class="sxs-lookup"><span data-stu-id="c9b5b-135">[Download](https://code.visualstudio.com/docs/setup/windows) and install Visual Studio Code.</span></span> <span data-ttu-id="c9b5b-136">A Visual Studio Code a Windows, Linux és macOS egy egyszerűsített, de hatékony forráskód szerkesztőjében.</span><span class="sxs-lookup"><span data-stu-id="c9b5b-136">Visual Studio Code is a lightweight but powerful source code editor for Windows, Linux, and macOS.</span></span> <span data-ttu-id="c9b5b-137">A mintakód szerkesztése a szerkesztő használata az oktatóanyag későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="c9b5b-137">You use this editor later in the tutorial to edit the sample code.</span></span>

## <a name="summary"></a><span data-ttu-id="c9b5b-138">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="c9b5b-138">Summary</span></span>

<span data-ttu-id="c9b5b-139">A szükséges fejlesztői eszközök és az első mintaalkalmazás szoftver telepítése.</span><span class="sxs-lookup"><span data-stu-id="c9b5b-139">You've installed the required development tools and software for the first sample application.</span></span> <span data-ttu-id="c9b5b-140">A következő feladata a létrehozásához, telepítéséhez és a Edison futtassa a mintaalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="c9b5b-140">The next task is to create, deploy, and run the sample application on Edison.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c9b5b-141">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c9b5b-141">Next steps</span></span>

<span data-ttu-id="c9b5b-142">[A villogási alkalmazás létrehozását és telepítését][create-and-deploy-the-blink-application]</span><span class="sxs-lookup"><span data-stu-id="c9b5b-142">[Create and deploy the blink application][create-and-deploy-the-blink-application]</span></span>

<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md
[create-and-deploy-the-blink-application]: iot-hub-intel-edison-kit-c-lesson1-deploy-blink-app.md
[windows]: iot-hub-intel-edison-kit-c-lesson1-get-the-tools-win32.md
[ubuntu]: iot-hub-intel-edison-kit-c-lesson1-get-the-tools-ubuntu.md
[macos]: iot-hub-intel-edison-kit-c-lesson1-get-the-tools-mac.md
