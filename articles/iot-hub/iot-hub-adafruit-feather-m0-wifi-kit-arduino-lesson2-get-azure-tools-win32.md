---
title: "Csatlakozás Arduino tooAzure IoT - lecke 2: (Windows) Azure-eszközök |} Microsoft Docs"
description: "Telepítse a Python és hello Azure parancssori felület (CLI) Windows 7 és újabb verziók."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "az Azure cli, iot felhőalapú szolgáltatás, arduino felhő"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: 70dfff14-4be1-468d-9919-e40f4bead308
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: f9b891224ff3974d9ce5382eb983470d5d41bcc4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-azure-tools-windows-7-and-later"></a><span data-ttu-id="2eca7-104">Első Azure-eszközök (Windows 7 és újabb verziók)</span><span class="sxs-lookup"><span data-stu-id="2eca7-104">Get Azure tools (Windows 7 and later)</span></span>

> [!div class="op_single_selector"]
> * <span data-ttu-id="2eca7-105">[Windows 7 vagy újabb][windows]</span><span class="sxs-lookup"><span data-stu-id="2eca7-105">[Windows 7 or later][windows]</span></span>
> * <span data-ttu-id="2eca7-106">[Ubuntu 16.04][ubuntu]</span><span class="sxs-lookup"><span data-stu-id="2eca7-106">[Ubuntu 16.04][ubuntu]</span></span>
> * <span data-ttu-id="2eca7-107">[macOS 10.10][macos]</span><span class="sxs-lookup"><span data-stu-id="2eca7-107">[macOS 10.10][macos]</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="2eca7-108">Mit fog</span><span class="sxs-lookup"><span data-stu-id="2eca7-108">What you will do</span></span>

<span data-ttu-id="2eca7-109">Telepítse a Python és hello Azure parancssori felület (CLI).</span><span class="sxs-lookup"><span data-stu-id="2eca7-109">Install Python and hello Azure command-line interface (Azure CLI).</span></span> <span data-ttu-id="2eca7-110">Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md) a Adafruit lágyított M0 Wi-Fi Arduino kártya.</span><span class="sxs-lookup"><span data-stu-id="2eca7-110">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md) for your Adafruit Feather M0 WiFi Arduino board.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="2eca7-111">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="2eca7-111">What you will learn</span></span>
<span data-ttu-id="2eca7-112">Ebből a cikkből megtudhatja:</span><span class="sxs-lookup"><span data-stu-id="2eca7-112">In this article, you will learn:</span></span>
* <span data-ttu-id="2eca7-113">Hogyan tooinstall Python.</span><span class="sxs-lookup"><span data-stu-id="2eca7-113">How tooinstall Python.</span></span>
* <span data-ttu-id="2eca7-114">Hogyan tooinstall hello Azure parancssori felület.</span><span class="sxs-lookup"><span data-stu-id="2eca7-114">How tooinstall hello Azure CLI.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="2eca7-115">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="2eca7-115">What you need</span></span>
* <span data-ttu-id="2eca7-116">Egy internetkapcsolattal rendelkező Windows-számítógép.</span><span class="sxs-lookup"><span data-stu-id="2eca7-116">A Windows computer with an Internet connection.</span></span>
* <span data-ttu-id="2eca7-117">Aktív Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="2eca7-117">An active Azure subscription.</span></span> <span data-ttu-id="2eca7-118">Ha az Azure-fiók nem rendelkezik, hozzon létre egy [ingyenes Azure próba-fiókot](http://azure.microsoft.com/pricing/free-trial/) csak néhány perc múlva.</span><span class="sxs-lookup"><span data-stu-id="2eca7-118">If you don't have an Azure account, create a [free Azure trial account](http://azure.microsoft.com/pricing/free-trial/) in just a few minutes.</span></span>

## <a name="install-python"></a><span data-ttu-id="2eca7-119">Python telepítése</span><span class="sxs-lookup"><span data-stu-id="2eca7-119">Install Python</span></span>
<span data-ttu-id="2eca7-120">[Telepítse a Python](https://www.python.org/downloads/) a Windows-számítógépen.</span><span class="sxs-lookup"><span data-stu-id="2eca7-120">[Install Python](https://www.python.org/downloads/) on your Windows computer.</span></span> <span data-ttu-id="2eca7-121">Python 2.7, 3.4-es vagy 3.5-ös verzióját is telepítheti.</span><span class="sxs-lookup"><span data-stu-id="2eca7-121">You can install Python 2.7, 3.4 or 3.5.</span></span> <span data-ttu-id="2eca7-122">Ez az oktatóanyag a Python 2.7 alapul.</span><span class="sxs-lookup"><span data-stu-id="2eca7-122">This tutorial is based on Python 2.7.</span></span> <span data-ttu-id="2eca7-123">Ha már telepítette a Python, nyissa meg a következő szakaszban toohello és hello Azure parancssori felület telepítése.</span><span class="sxs-lookup"><span data-stu-id="2eca7-123">If you've already installed Python, go toohello next section and install hello Azure CLI.</span></span>

<span data-ttu-id="2eca7-124">Szükség tooadd hello elérési útját, amelyben python.exe és pip.exe is telepített toohello rendszer hello mappák `PATH` környezeti változó.</span><span class="sxs-lookup"><span data-stu-id="2eca7-124">You also need tooadd hello path of hello folders where python.exe and pip.exe are installed toohello system `PATH` environment variable.</span></span> <span data-ttu-id="2eca7-125">Alapértelmezés szerint a python.exe telepítve van a `C:\Python27` és pip.exe telepítve van-e `C:\Python27\Scripts`.</span><span class="sxs-lookup"><span data-stu-id="2eca7-125">By default, python.exe is installed in `C:\Python27` and pip.exe is installed in `C:\Python27\Scripts`.</span></span>

## <a name="install-hello-azure-cli"></a><span data-ttu-id="2eca7-126">Hello Azure parancssori felület telepítése</span><span class="sxs-lookup"><span data-stu-id="2eca7-126">Install hello Azure CLI</span></span>
<span data-ttu-id="2eca7-127">hello Azure CLI olyan többplatformos parancssori környezetet biztosít az Azure.</span><span class="sxs-lookup"><span data-stu-id="2eca7-127">hello Azure CLI provides a multiplatform command-line experience for Azure.</span></span> <span data-ttu-id="2eca7-128">Közvetlenül a parancssor tooprovision dolgozhassanak és kezelheti az erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="2eca7-128">You work directly from your command line tooprovision and manage resources.</span></span>

<span data-ttu-id="2eca7-129">tooinstall hello Azure CLI használata esetén kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="2eca7-129">tooinstall hello Azure CLI, follow these steps:</span></span>

1. <span data-ttu-id="2eca7-130">Nyisson meg egy parancssori ablakot rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="2eca7-130">Open a Command Prompt window as an administrator.</span></span>
2. <span data-ttu-id="2eca7-131">Futtassa a következő parancsok hello:</span><span class="sxs-lookup"><span data-stu-id="2eca7-131">Run hello following commands:</span></span>

   ```bash
   pip install --upgrade azure-cli
   pip install --upgrade azure-cli-iot
   ```
3. <span data-ttu-id="2eca7-132">Ellenőrizze a hello telepítést hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="2eca7-132">Verify hello installation by running hello following command:</span></span>

   ```bash
   az iot -h
   ```

<span data-ttu-id="2eca7-133">Megjelenik a hello parancskimenet sikeres hello telepítés esetén.</span><span class="sxs-lookup"><span data-stu-id="2eca7-133">You see hello following output if hello installation is successful.</span></span>

![Kimeneti sikerességét jelző][output]

## <a name="summary"></a><span data-ttu-id="2eca7-135">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="2eca7-135">Summary</span></span>
<span data-ttu-id="2eca7-136">Hello Azure parancssori felület telepítése.</span><span class="sxs-lookup"><span data-stu-id="2eca7-136">You've installed hello Azure CLI.</span></span> <span data-ttu-id="2eca7-137">A Tovább gombra a feladatütemezés toocreate az Azure IoT hub- és eszközidentitások hello Azure parancssori felület használatával.</span><span class="sxs-lookup"><span data-stu-id="2eca7-137">Your next task toocreate your Azure IoT hub and device identity by using hello Azure CLI.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2eca7-138">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2eca7-138">Next steps</span></span>
<span data-ttu-id="2eca7-139">[Az IoT hub létrehozása és regisztrálása a Arduino tábla][create-your-iot-hub-and-register-your-arduino-board]</span><span class="sxs-lookup"><span data-stu-id="2eca7-139">[Create your IoT hub and register your Arduino board][create-your-iot-hub-and-register-your-arduino-board]</span></span>


<!-- Images and links -->

[windows]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-get-azure-tools-win32.md
[ubuntu]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-get-azure-tools-ubuntu.md
[macos]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-get-azure-tools-mac.md
[output]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson2/az_iot_help_win.png
[create-your-iot-hub-and-register-your-arduino-board]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-prepare-azure-iot-hub.md