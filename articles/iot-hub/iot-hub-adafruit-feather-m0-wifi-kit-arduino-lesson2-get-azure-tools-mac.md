---
title: "Csatlakozás Arduino tooAzure IoT - lecke 2: Azure-eszközök (macOS) |} Microsoft Docs"
description: "MacOS Python és az Azure parancssori felület (CLI) telepíthető."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "az Azure cli, iot felhőalapú szolgáltatás, arduino felhő"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: 9b719293-01d2-4a2d-9c49-476e67f4816d
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 8f0ec4131e54af5475cd0b4240480c3fda497e14
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-azure-tools-macos-1010"></a><span data-ttu-id="63f77-104">Azure-eszközök (macOS 10.10) beolvasása</span><span class="sxs-lookup"><span data-stu-id="63f77-104">Get Azure tools (macOS 10.10)</span></span>

> [!div class="op_single_selector"]
> * <span data-ttu-id="63f77-105">[Windows 7 vagy újabb][windows]</span><span class="sxs-lookup"><span data-stu-id="63f77-105">[Windows 7 or later][windows]</span></span>
> * <span data-ttu-id="63f77-106">[Ubuntu 16.04][ubuntu]</span><span class="sxs-lookup"><span data-stu-id="63f77-106">[Ubuntu 16.04][ubuntu]</span></span>
> * <span data-ttu-id="63f77-107">[macOS 10.10][macos]</span><span class="sxs-lookup"><span data-stu-id="63f77-107">[macOS 10.10][macos]</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="63f77-108">Mit fog</span><span class="sxs-lookup"><span data-stu-id="63f77-108">What you will do</span></span>

<span data-ttu-id="63f77-109">Hello Azure parancssori felület (CLI) telepítése.</span><span class="sxs-lookup"><span data-stu-id="63f77-109">Install hello Azure command-line interface (Azure CLI).</span></span> <span data-ttu-id="63f77-110">Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md) a Adafruit lágyított M0 Wi-Fi Arduino kártya.</span><span class="sxs-lookup"><span data-stu-id="63f77-110">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md) for your Adafruit Feather M0 WiFi Arduino board.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="63f77-111">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="63f77-111">What you will learn</span></span>
<span data-ttu-id="63f77-112">Ebből a cikkből megtudhatja:</span><span class="sxs-lookup"><span data-stu-id="63f77-112">In this article, you will learn:</span></span>
* <span data-ttu-id="63f77-113">Hogyan tooinstall Azure parancssori felület.</span><span class="sxs-lookup"><span data-stu-id="63f77-113">How tooinstall Azure CLI.</span></span>
* <span data-ttu-id="63f77-114">Hogyan tooadd hello Azure parancssori felület egy IoT ablaktábla.</span><span class="sxs-lookup"><span data-stu-id="63f77-114">How tooadd an IoT subgroup of hello Azure CLI.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="63f77-115">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="63f77-115">What you need</span></span>
* <span data-ttu-id="63f77-116">A Mac, az internetet.</span><span class="sxs-lookup"><span data-stu-id="63f77-116">A Mac with an Internet connection.</span></span>
* <span data-ttu-id="63f77-117">Aktív Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="63f77-117">An active Azure subscription.</span></span> <span data-ttu-id="63f77-118">Ha az Azure-fiók nem rendelkezik, akkor létrehozhat egy [ingyenes Azure próba-fiókot](http://azure.microsoft.com/pricing/free-trial/) csak néhány perc múlva.</span><span class="sxs-lookup"><span data-stu-id="63f77-118">If you don't have an Azure account, you can create a [free Azure trial account](http://azure.microsoft.com/pricing/free-trial/) in just a few minutes.</span></span>

## <a name="install-python"></a><span data-ttu-id="63f77-119">Python telepítése</span><span class="sxs-lookup"><span data-stu-id="63f77-119">Install Python</span></span>
<span data-ttu-id="63f77-120">Bár macOS Python 2.7 hello kezdő verzióról, azt javasoljuk, hogy telepítse a Python Homebrew keresztül.</span><span class="sxs-lookup"><span data-stu-id="63f77-120">Although macOS comes with Python 2.7 out of hello box, we recommend that you install Python through Homebrew.</span></span> <span data-ttu-id="63f77-121">Lásd: [Python telepítése a macOS](http://docs.python-guide.org/en/latest/starting/install/osx/).</span><span class="sxs-lookup"><span data-stu-id="63f77-121">See [Installing Python on macOS](http://docs.python-guide.org/en/latest/starting/install/osx/).</span></span>

<span data-ttu-id="63f77-122">Telepítse a Python és a pip hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="63f77-122">Install Python and pip by running hello following command:</span></span>

```bash
brew install python
```

## <a name="install-hello-azure-cli"></a><span data-ttu-id="63f77-123">Hello Azure parancssori felület telepítése</span><span class="sxs-lookup"><span data-stu-id="63f77-123">Install hello Azure CLI</span></span>
<span data-ttu-id="63f77-124">hello Azure CLI olyan többplatformos parancssori környezetet biztosít az Azure.</span><span class="sxs-lookup"><span data-stu-id="63f77-124">hello Azure CLI provides a multiplatform command-line experience for Azure.</span></span> <span data-ttu-id="63f77-125">Közvetlenül a parancssor tooprovision dolgozhassanak és kezelheti az erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="63f77-125">You work directly from your command line tooprovision and manage resources.</span></span>

<span data-ttu-id="63f77-126">tooinstall hello Azure CLI legújabb, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="63f77-126">tooinstall hello latest Azure CLI, follow these steps:</span></span>

1. <span data-ttu-id="63f77-127">Futtassa a következő parancsokat egy terminálablakot hello.</span><span class="sxs-lookup"><span data-stu-id="63f77-127">Run hello following commands in a terminal window.</span></span> <span data-ttu-id="63f77-128">Öt perc tooinstall hello Azure CLI vehet igénybe.</span><span class="sxs-lookup"><span data-stu-id="63f77-128">It might take five minutes tooinstall hello Azure CLI.</span></span>

   ```bash
   pip install --upgrade azure-cli
   pip install --upgrade azure-cli-iot
   ```
2. <span data-ttu-id="63f77-129">Ellenőrizze a hello telepítést hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="63f77-129">Verify hello installation by running hello following command:</span></span>

   ```bash
   az iot -h
   ```

<span data-ttu-id="63f77-130">Meg kell jelennie a hello parancskimenet sikeres hello telepítés esetén.</span><span class="sxs-lookup"><span data-stu-id="63f77-130">You should see hello following output if hello installation is successful.</span></span>

![Kimeneti sikerességét jelző][output]

## <a name="summary"></a><span data-ttu-id="63f77-132">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="63f77-132">Summary</span></span>
<span data-ttu-id="63f77-133">Hello Azure parancssori felület telepítése.</span><span class="sxs-lookup"><span data-stu-id="63f77-133">You've installed hello Azure CLI.</span></span> <span data-ttu-id="63f77-134">A következő feladathoz toocreate az Azure IoT hub- és eszközidentitások használatával hello Azure parancssori felület.</span><span class="sxs-lookup"><span data-stu-id="63f77-134">Your next task is toocreate your Azure IoT hub and device identity by using hello Azure CLI.</span></span>

## <a name="next-steps"></a><span data-ttu-id="63f77-135">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="63f77-135">Next steps</span></span>
<span data-ttu-id="63f77-136">[Az IoT hub létrehozása és regisztrálása a Arduino tábla][create-your-iot-hub-and-register-your-arduino-board]</span><span class="sxs-lookup"><span data-stu-id="63f77-136">[Create your IoT hub and register your Arduino board][create-your-iot-hub-and-register-your-arduino-board]</span></span>


<!-- Images and links -->

[windows]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-get-azure-tools-win32.md
[ubuntu]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-get-azure-tools-ubuntu.md
[macos]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-get-azure-tools-mac.md
[output]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson2/az_iot_help_osx.png
[create-your-iot-hub-and-register-your-arduino-board]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-prepare-azure-iot-hub.md