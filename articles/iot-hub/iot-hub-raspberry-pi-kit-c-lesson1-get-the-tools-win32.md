---
title: 'Connect Raspberry pi (C) tooAzure IoT - lecke 1: Get tools (Windows) |} Microsoft Docs'
description: "Töltse le és hello szükséges eszközök és szoftverek hello első mintaalkalmazás Pi telepítéséhez Windows 7 és újabb verziók."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "az IOT-fejlesztési, iot szoftverek, internet dolgot szoftver, telepítse a git szoftvert a windows, csomópont js windows telepítése, npm telepítése windows"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started
ms.assetid: bd765ddd-65b7-4241-a391-dc77cb3af1c0
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 70ae6d15f9d6af116ff065a79a30d99afc67bffd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-tools-windows-7-or-later"></a><span data-ttu-id="a3b10-104">Első hello eszközök (Windows 7 vagy újabb)</span><span class="sxs-lookup"><span data-stu-id="a3b10-104">Get hello tools (Windows 7 or later)</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="a3b10-105">Windows 7 vagy újabb</span><span class="sxs-lookup"><span data-stu-id="a3b10-105">Windows 7 or later</span></span>](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-win32.md)
> * [<span data-ttu-id="a3b10-106">Ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="a3b10-106">Ubuntu 16.04</span></span>](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-ubuntu.md)
> * [<span data-ttu-id="a3b10-107">macOS 10.10</span><span class="sxs-lookup"><span data-stu-id="a3b10-107">macOS 10.10</span></span>](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-mac.md)

## <a name="what-you-will-do"></a><span data-ttu-id="a3b10-108">Mit fog</span><span class="sxs-lookup"><span data-stu-id="a3b10-108">What you will do</span></span>
<span data-ttu-id="a3b10-109">Hello Fejlesztőeszközök és hello első mintaalkalmazás málna Pi 3 hello szoftver letöltése.</span><span class="sxs-lookup"><span data-stu-id="a3b10-109">Download hello development tools and hello software for hello first sample application for Raspberry Pi 3.</span></span> <span data-ttu-id="a3b10-110">Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="a3b10-110">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span></span>

> [!NOTE]
> <span data-ttu-id="a3b10-111">Bár programozási nyelv hello fő logikájának hello C, Node.js eszközök szerepelnek hello során tapasztalatokat toodiscover eszközök, és hozza létre, és minta alkalmazások központi telepítése.</span><span class="sxs-lookup"><span data-stu-id="a3b10-111">Although hello programming language of hello main logic is C, Node.js tools are used in hello lessons toodiscover devices, and build and deploy sample applications.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="a3b10-112">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="a3b10-112">What you will learn</span></span>
<span data-ttu-id="a3b10-113">Ebből a cikkből megtudhatja:</span><span class="sxs-lookup"><span data-stu-id="a3b10-113">In this article, you will learn:</span></span>

* <span data-ttu-id="a3b10-114">Hogyan tooinstall a Git szoftver, Node.js.</span><span class="sxs-lookup"><span data-stu-id="a3b10-114">How tooinstall Git and Node.js.</span></span>
  * <span data-ttu-id="a3b10-115">[Git](https://git-scm.com) van egy nyílt forráskódú elosztott verziókezelő rendszer.</span><span class="sxs-lookup"><span data-stu-id="a3b10-115">[Git](https://git-scm.com) is an open source distributed version control system.</span></span> <span data-ttu-id="a3b10-116">Ez a cikk hello-mintaalkalmazás Git tárolja.</span><span class="sxs-lookup"><span data-stu-id="a3b10-116">hello sample application for this article is stored on Git.</span></span>
  * <span data-ttu-id="a3b10-117">[NODE.js](https://nodejs.org/en/) van a JavaScript futásidejű és gazdag csomag-ökoszisztéma.</span><span class="sxs-lookup"><span data-stu-id="a3b10-117">[Node.js](https://nodejs.org/en/) is a JavaScript runtime with a rich package ecosystem.</span></span>
* <span data-ttu-id="a3b10-118">Hogyan toouse NPM tooinstall további Node.js fejlesztői eszközök.</span><span class="sxs-lookup"><span data-stu-id="a3b10-118">How toouse NPM tooinstall additional Node.js development tools.</span></span>
  * <span data-ttu-id="a3b10-119">Node.js hello minimális verziójára vonatkozó követelményt a 4.5-ös LTS.</span><span class="sxs-lookup"><span data-stu-id="a3b10-119">hello minimum version requirement of Node.js is 4.5 LTS.</span></span>
  * <span data-ttu-id="a3b10-120">[NPM](https://www.npmjs.com) egyike hello Node.js csomag feletteseit.</span><span class="sxs-lookup"><span data-stu-id="a3b10-120">[NPM](https://www.npmjs.com) is one of hello package managers for Node.js.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="a3b10-121">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="a3b10-121">What you need</span></span>

<span data-ttu-id="a3b10-122">toocomplete ennél a műveletnél, szüksége lesz:</span><span class="sxs-lookup"><span data-stu-id="a3b10-122">toocomplete this operation, you will need:</span></span>

* <span data-ttu-id="a3b10-123">Az Internet kapcsolat toodownload hello Fejlesztőeszközök és hello szoftver.</span><span class="sxs-lookup"><span data-stu-id="a3b10-123">An Internet connection toodownload hello development tools and hello software.</span></span>
* <span data-ttu-id="a3b10-124">Windows rendszerű számítógép.</span><span class="sxs-lookup"><span data-stu-id="a3b10-124">A computer that is running Windows.</span></span>

## <a name="install-git-and-nodejs"></a><span data-ttu-id="a3b10-125">Telepítse a Git szoftver, Node.js</span><span class="sxs-lookup"><span data-stu-id="a3b10-125">Install Git and Node.js</span></span>

<span data-ttu-id="a3b10-126">Alább toodownload hello hivatkozásaira kattint, és telepítse a Git és a Node.js-es lts verzió a Windows.</span><span class="sxs-lookup"><span data-stu-id="a3b10-126">Click hello links below toodownload and install Git and Node.js LTS for Windows.</span></span>

* [<span data-ttu-id="a3b10-127">Git letöltése a Windows rendszerhez</span><span class="sxs-lookup"><span data-stu-id="a3b10-127">Get Git for Windows</span></span>](https://git-scm.com/download/win/)
* [<span data-ttu-id="a3b10-128">Node.js-es lts verzió letöltése a Windows rendszerhez</span><span class="sxs-lookup"><span data-stu-id="a3b10-128">Get Node.js LTS for Windows</span></span>](https://nodejs.org/en/)

## <a name="install-additional-nodejs-development-tools"></a><span data-ttu-id="a3b10-129">További Node.js fejlesztői eszközök telepítése</span><span class="sxs-lookup"><span data-stu-id="a3b10-129">Install additional Node.js development tools</span></span>

<span data-ttu-id="a3b10-130">Használjon [gulp.js](http://gulpjs.com) hello minta alkalmazás tooPi tooautomate hello központi telepítését.</span><span class="sxs-lookup"><span data-stu-id="a3b10-130">Use [gulp.js](http://gulpjs.com) tooautomate hello deployment of hello sample application tooPi.</span></span> <span data-ttu-id="a3b10-131">Használjon hello [cli-eszköz-felderítési](https://github.com/Azure/device-discovery-cli) tooretrieve hálózati adatokat az IoT-eszközökről.</span><span class="sxs-lookup"><span data-stu-id="a3b10-131">Use hello [device-discovery-cli](https://github.com/Azure/device-discovery-cli) tooretrieve network information about your IoT devices.</span></span>

<span data-ttu-id="a3b10-132">Nyisson meg egy parancssort rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="a3b10-132">Start a command prompt as an administrator.</span></span> <span data-ttu-id="a3b10-133">Telepítés `gulp` és `device-discovery-cli` hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="a3b10-133">Install `gulp` and `device-discovery-cli` by running hello following command:</span></span>

```bash
npm install -g device-discovery-cli gulp
```

<span data-ttu-id="a3b10-134">Ha problémák, Node.js és a további Node.js fejlesztői eszközök telepítése a számítógépre, lásd: hello [hibaelhárítási útmutatója](iot-hub-raspberry-pi-kit-c-troubleshooting.md) a megoldások toocommon problémákat.</span><span class="sxs-lookup"><span data-stu-id="a3b10-134">If you experience issues installing Node.js and these additional Node.js development tools on your computer, see hello [troubleshooting guide](iot-hub-raspberry-pi-kit-c-troubleshooting.md) for solutions toocommon problems.</span></span>

## <a name="install-visual-studio-code"></a><span data-ttu-id="a3b10-135">Visual Studio Code telepítése</span><span class="sxs-lookup"><span data-stu-id="a3b10-135">Install Visual Studio Code</span></span>

<span data-ttu-id="a3b10-136">[Töltse le](https://code.visualstudio.com/docs/setup/windows) és a Visual Studio Code telepítése.</span><span class="sxs-lookup"><span data-stu-id="a3b10-136">[Download](https://code.visualstudio.com/docs/setup/windows) and install Visual Studio Code.</span></span> <span data-ttu-id="a3b10-137">A Visual Studio Code a Windows, Linux és macOS egy egyszerűsített, de hatékony forráskód szerkesztőjében.</span><span class="sxs-lookup"><span data-stu-id="a3b10-137">Visual Studio Code is a lightweight but powerful source code editor for Windows, Linux, and macOS.</span></span> <span data-ttu-id="a3b10-138">A szerkesztő később hello oktatóanyag tooedit hello mintakód használható.</span><span class="sxs-lookup"><span data-stu-id="a3b10-138">You use this editor later in hello tutorial tooedit hello sample code.</span></span>

## <a name="summary"></a><span data-ttu-id="a3b10-139">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="a3b10-139">Summary</span></span>

<span data-ttu-id="a3b10-140">Szükséges hello fejlesztői eszközök és szoftverek hello első mintaalkalmazás telepítése.</span><span class="sxs-lookup"><span data-stu-id="a3b10-140">You've installed hello required development tools and software for hello first sample application.</span></span> <span data-ttu-id="a3b10-141">hello tovább feladat toocreate, telepítése, és futtassa a hello mintaalkalmazást a Pi.</span><span class="sxs-lookup"><span data-stu-id="a3b10-141">hello next task is toocreate, deploy, and run hello sample application on Pi.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a3b10-142">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a3b10-142">Next steps</span></span>

[<span data-ttu-id="a3b10-143">Hello villogási alkalmazás létrehozását és telepítését</span><span class="sxs-lookup"><span data-stu-id="a3b10-143">Create and deploy hello blink application</span></span>](iot-hub-raspberry-pi-kit-c-lesson1-deploy-blink-app.md)
