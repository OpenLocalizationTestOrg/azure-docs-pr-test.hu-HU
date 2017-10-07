---
title: "Csatlakozás málna Pi (csomópont) tooAzure IoT - lecke 1: Get tools (Windows) |} Microsoft Docs"
description: "Töltse le és hello szükséges eszközök és szoftverek hello első mintaalkalmazás Pi telepítéséhez Windows 7 és újabb verziók."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "az IOT-fejlesztési, iot szoftver, dolgot szoftver, internet git telepítése windows, Futtatás gulp, csomópont js windows telepítése, npm telepítése windows, python telepítése windows"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: b3d88e17-97cc-4f23-85fd-a688fc228eb8
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: ea7f77cc79c70c8c7952b63462b926471fcf71cf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-tools-windows-7-or-later"></a><span data-ttu-id="7558f-104">Első hello eszközök (Windows 7 vagy újabb)</span><span class="sxs-lookup"><span data-stu-id="7558f-104">Get hello tools (Windows 7 or later)</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="7558f-105">Windows 7 vagy újabb</span><span class="sxs-lookup"><span data-stu-id="7558f-105">Windows 7 or later</span></span>](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)
> * [<span data-ttu-id="7558f-106">Ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="7558f-106">Ubuntu 16.04</span></span>](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-ubuntu.md)
> * [<span data-ttu-id="7558f-107">macOS 10.10</span><span class="sxs-lookup"><span data-stu-id="7558f-107">macOS 10.10</span></span>](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-mac.md)


## <a name="what-you-will-do"></a><span data-ttu-id="7558f-108">Mit fog</span><span class="sxs-lookup"><span data-stu-id="7558f-108">What you will do</span></span>
<span data-ttu-id="7558f-109">Hello Fejlesztőeszközök és hello első mintaalkalmazás málna Pi 3 hello szoftver letöltése.</span><span class="sxs-lookup"><span data-stu-id="7558f-109">Download hello development tools and hello software for hello first sample application for Raspberry Pi 3.</span></span> <span data-ttu-id="7558f-110">Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="7558f-110">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="7558f-111">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="7558f-111">What you will learn</span></span>
<span data-ttu-id="7558f-112">Ebből a cikkből megtudhatja:</span><span class="sxs-lookup"><span data-stu-id="7558f-112">In this article, you will learn:</span></span>

* <span data-ttu-id="7558f-113">Hogyan tooinstall a Git szoftver, Node.js.</span><span class="sxs-lookup"><span data-stu-id="7558f-113">How tooinstall Git and Node.js.</span></span>
  * <span data-ttu-id="7558f-114">[Git](https://git-scm.com) van egy nyílt forráskódú elosztott verziókezelő rendszer.</span><span class="sxs-lookup"><span data-stu-id="7558f-114">[Git](https://git-scm.com) is an open source distributed version control system.</span></span> <span data-ttu-id="7558f-115">Ez a cikk hello-mintaalkalmazás Git tárolja.</span><span class="sxs-lookup"><span data-stu-id="7558f-115">hello sample application for this article is stored on Git.</span></span>
  * <span data-ttu-id="7558f-116">[NODE.js](https://nodejs.org/en/) van a JavaScript futásidejű és gazdag csomag-ökoszisztéma.</span><span class="sxs-lookup"><span data-stu-id="7558f-116">[Node.js](https://nodejs.org/en/) is a JavaScript runtime with a rich package ecosystem.</span></span>
* <span data-ttu-id="7558f-117">Hogyan toouse NPM tooinstall további Node.js fejlesztői eszközök.</span><span class="sxs-lookup"><span data-stu-id="7558f-117">How toouse NPM tooinstall additional Node.js development tools.</span></span>
  * <span data-ttu-id="7558f-118">Node.js hello minimális verziójára vonatkozó követelményt a 4.5-ös LTS.</span><span class="sxs-lookup"><span data-stu-id="7558f-118">hello minimum version requirement of Node.js is 4.5 LTS.</span></span>
  * <span data-ttu-id="7558f-119">[NPM](https://www.npmjs.com) egyike hello Node.js csomag feletteseit.</span><span class="sxs-lookup"><span data-stu-id="7558f-119">[NPM](https://www.npmjs.com) is one of hello package managers for Node.js.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="7558f-120">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="7558f-120">What you need</span></span>
<span data-ttu-id="7558f-121">toocomplete ennél a műveletnél, szüksége lesz:</span><span class="sxs-lookup"><span data-stu-id="7558f-121">toocomplete this operation, you will need:</span></span>

* <span data-ttu-id="7558f-122">Az Internet kapcsolat toodownload hello Fejlesztőeszközök és hello szoftver.</span><span class="sxs-lookup"><span data-stu-id="7558f-122">An Internet connection toodownload hello development tools and hello software.</span></span>
* <span data-ttu-id="7558f-123">Windows rendszerű számítógép.</span><span class="sxs-lookup"><span data-stu-id="7558f-123">A computer that is running Windows.</span></span>

## <a name="install-git-and-nodejs"></a><span data-ttu-id="7558f-124">Telepítse a Git szoftver, Node.js</span><span class="sxs-lookup"><span data-stu-id="7558f-124">Install Git and Node.js</span></span>
<span data-ttu-id="7558f-125">A következő hivatkozások toodownload hello kattintson, és telepítse a Git és a Node.js-es lts verzió Windows.</span><span class="sxs-lookup"><span data-stu-id="7558f-125">Click hello following links toodownload and install Git and Node.js LTS for Windows.</span></span>

* [<span data-ttu-id="7558f-126">Git letöltése a Windows rendszerhez</span><span class="sxs-lookup"><span data-stu-id="7558f-126">Get Git for Windows</span></span>](https://git-scm.com/download/win/)
* [<span data-ttu-id="7558f-127">Node.js-es lts verzió letöltése a Windows rendszerhez</span><span class="sxs-lookup"><span data-stu-id="7558f-127">Get Node.js LTS for Windows</span></span>](https://nodejs.org/en/)

## <a name="install-additional-nodejs-development-tools"></a><span data-ttu-id="7558f-128">További Node.js fejlesztői eszközök telepítése</span><span class="sxs-lookup"><span data-stu-id="7558f-128">Install additional Node.js development tools</span></span>
<span data-ttu-id="7558f-129">Használjon [gulp.js](http://gulpjs.com) hello minta alkalmazás tooPi tooautomate hello központi telepítését.</span><span class="sxs-lookup"><span data-stu-id="7558f-129">Use [gulp.js](http://gulpjs.com) tooautomate hello deployment of hello sample application tooPi.</span></span> <span data-ttu-id="7558f-130">Hello is a [cli-eszköz-felderítési](https://github.com/Azure/device-discovery-cli) tooretrieve hálózati adatokat az IoT-eszközökről.</span><span class="sxs-lookup"><span data-stu-id="7558f-130">You also use hello [device-discovery-cli](https://github.com/Azure/device-discovery-cli) tooretrieve network information about your IoT devices.</span></span>

<span data-ttu-id="7558f-131">Nyisson meg egy parancssort rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="7558f-131">Start a command prompt as an administrator.</span></span> <span data-ttu-id="7558f-132">Telepítés `gulp` és `device-discovery-cli` hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="7558f-132">Install `gulp` and `device-discovery-cli` by running hello following command:</span></span>

```bash
npm install -g device-discovery-cli gulp
```

<span data-ttu-id="7558f-133">Ha problémák, Node.js és a további Node.js fejlesztői eszközök telepítése a számítógépre, lásd: hello [hibaelhárítási útmutatója](iot-hub-raspberry-pi-kit-node-troubleshooting.md) a megoldások toocommon problémákat.</span><span class="sxs-lookup"><span data-stu-id="7558f-133">If you experience issues installing Node.js and these additional Node.js development tools on your computer, see hello [troubleshooting guide](iot-hub-raspberry-pi-kit-node-troubleshooting.md) for solutions toocommon problems.</span></span>

## <a name="install-visual-studio-code"></a><span data-ttu-id="7558f-134">Visual Studio Code telepítése</span><span class="sxs-lookup"><span data-stu-id="7558f-134">Install Visual Studio Code</span></span>
<span data-ttu-id="7558f-135">[Töltse le](https://code.visualstudio.com/docs/setup/windows) és a Visual Studio Code telepítése.</span><span class="sxs-lookup"><span data-stu-id="7558f-135">[Download](https://code.visualstudio.com/docs/setup/windows) and install Visual Studio Code.</span></span> <span data-ttu-id="7558f-136">A Visual Studio Code a Windows, Linux és macOS egy egyszerűsített, de hatékony forráskód szerkesztőjében.</span><span class="sxs-lookup"><span data-stu-id="7558f-136">Visual Studio Code is a lightweight but powerful source code editor for Windows, Linux, and macOS.</span></span> <span data-ttu-id="7558f-137">A szerkesztő később hello oktatóanyag tooedit hello mintakód használható.</span><span class="sxs-lookup"><span data-stu-id="7558f-137">You use this editor later in hello tutorial tooedit hello sample code.</span></span>

## <a name="summary"></a><span data-ttu-id="7558f-138">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="7558f-138">Summary</span></span>
<span data-ttu-id="7558f-139">Szükséges hello fejlesztői eszközök és szoftverek hello első mintaalkalmazás telepítése.</span><span class="sxs-lookup"><span data-stu-id="7558f-139">You've installed hello required development tools and software for hello first sample application.</span></span> <span data-ttu-id="7558f-140">hello tovább feladat toocreate, telepítése, és futtassa a hello mintaalkalmazást a Pi.</span><span class="sxs-lookup"><span data-stu-id="7558f-140">hello next task is toocreate, deploy, and run hello sample application on Pi.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7558f-141">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7558f-141">Next steps</span></span>
[<span data-ttu-id="7558f-142">Hello villogási minta alkalmazás létrehozását és telepítését</span><span class="sxs-lookup"><span data-stu-id="7558f-142">Create and deploy hello blink sample application</span></span>](iot-hub-raspberry-pi-kit-node-lesson1-deploy-blink-app.md)

