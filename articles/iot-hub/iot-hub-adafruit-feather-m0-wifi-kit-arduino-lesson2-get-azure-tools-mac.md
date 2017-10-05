---
title: "Csatlakozás Azure IoT - lecke 2 Arduino: Azure-eszközök (macOS) |} Microsoft Docs"
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
ms.openlocfilehash: 037f8e3dba542773185fde43a345d5fd487b2d84
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-azure-tools-macos-1010"></a><span data-ttu-id="e8f7b-104">Azure-eszközök (macOS 10.10) beolvasása</span><span class="sxs-lookup"><span data-stu-id="e8f7b-104">Get Azure tools (macOS 10.10)</span></span>

> [!div class="op_single_selector"]
> * <span data-ttu-id="e8f7b-105">[Windows 7 vagy újabb][windows]</span><span class="sxs-lookup"><span data-stu-id="e8f7b-105">[Windows 7 or later][windows]</span></span>
> * <span data-ttu-id="e8f7b-106">[Ubuntu 16.04][ubuntu]</span><span class="sxs-lookup"><span data-stu-id="e8f7b-106">[Ubuntu 16.04][ubuntu]</span></span>
> * <span data-ttu-id="e8f7b-107">[macOS 10.10][macos]</span><span class="sxs-lookup"><span data-stu-id="e8f7b-107">[macOS 10.10][macos]</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="e8f7b-108">Mit fog</span><span class="sxs-lookup"><span data-stu-id="e8f7b-108">What you will do</span></span>

<span data-ttu-id="e8f7b-109">Telepítse az Azure parancssori felület (CLI).</span><span class="sxs-lookup"><span data-stu-id="e8f7b-109">Install the Azure command-line interface (Azure CLI).</span></span> <span data-ttu-id="e8f7b-110">Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md) a Adafruit lágyított M0 Wi-Fi Arduino kártya.</span><span class="sxs-lookup"><span data-stu-id="e8f7b-110">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md) for your Adafruit Feather M0 WiFi Arduino board.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="e8f7b-111">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="e8f7b-111">What you will learn</span></span>
<span data-ttu-id="e8f7b-112">Ebből a cikkből megtudhatja:</span><span class="sxs-lookup"><span data-stu-id="e8f7b-112">In this article, you will learn:</span></span>
* <span data-ttu-id="e8f7b-113">Tudnivalók az Azure parancssori felület telepítése.</span><span class="sxs-lookup"><span data-stu-id="e8f7b-113">How to install Azure CLI.</span></span>
* <span data-ttu-id="e8f7b-114">Hogyan lehet az Azure parancssori felület egy IoT részcsoport hozzáadni.</span><span class="sxs-lookup"><span data-stu-id="e8f7b-114">How to add an IoT subgroup of the Azure CLI.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="e8f7b-115">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="e8f7b-115">What you need</span></span>
* <span data-ttu-id="e8f7b-116">A Mac, az internetet.</span><span class="sxs-lookup"><span data-stu-id="e8f7b-116">A Mac with an Internet connection.</span></span>
* <span data-ttu-id="e8f7b-117">Aktív Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="e8f7b-117">An active Azure subscription.</span></span> <span data-ttu-id="e8f7b-118">Ha az Azure-fiók nem rendelkezik, akkor létrehozhat egy [ingyenes Azure próba-fiókot](http://azure.microsoft.com/pricing/free-trial/) csak néhány perc múlva.</span><span class="sxs-lookup"><span data-stu-id="e8f7b-118">If you don't have an Azure account, you can create a [free Azure trial account](http://azure.microsoft.com/pricing/free-trial/) in just a few minutes.</span></span>

## <a name="install-python"></a><span data-ttu-id="e8f7b-119">Python telepítése</span><span class="sxs-lookup"><span data-stu-id="e8f7b-119">Install Python</span></span>
<span data-ttu-id="e8f7b-120">Bár macOS Python 2.7 kívül a mezőbe, azt javasoljuk, hogy telepítse a Python Homebrew keresztül.</span><span class="sxs-lookup"><span data-stu-id="e8f7b-120">Although macOS comes with Python 2.7 out of the box, we recommend that you install Python through Homebrew.</span></span> <span data-ttu-id="e8f7b-121">Lásd: [Python telepítése a macOS](http://docs.python-guide.org/en/latest/starting/install/osx/).</span><span class="sxs-lookup"><span data-stu-id="e8f7b-121">See [Installing Python on macOS](http://docs.python-guide.org/en/latest/starting/install/osx/).</span></span>

<span data-ttu-id="e8f7b-122">Telepítse a Python és a pip a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="e8f7b-122">Install Python and pip by running the following command:</span></span>

```bash
brew install python
```

## <a name="install-the-azure-cli"></a><span data-ttu-id="e8f7b-123">Telepítse az Azure CLI-t</span><span class="sxs-lookup"><span data-stu-id="e8f7b-123">Install the Azure CLI</span></span>
<span data-ttu-id="e8f7b-124">Az Azure parancssori felület olyan többplatformos parancssori környezetet biztosít az Azure.</span><span class="sxs-lookup"><span data-stu-id="e8f7b-124">The Azure CLI provides a multiplatform command-line experience for Azure.</span></span> <span data-ttu-id="e8f7b-125">Közvetlenül a parancssorból kiépítését működik, és kezelheti az erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="e8f7b-125">You work directly from your command line to provision and manage resources.</span></span>

<span data-ttu-id="e8f7b-126">Az Azure CLI legújabb telepítéséhez kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="e8f7b-126">To install the latest Azure CLI, follow these steps:</span></span>

1. <span data-ttu-id="e8f7b-127">Futtassa a következő parancsokat egy terminálablakot.</span><span class="sxs-lookup"><span data-stu-id="e8f7b-127">Run the following commands in a terminal window.</span></span> <span data-ttu-id="e8f7b-128">Az Azure parancssori felület telepítése öt percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="e8f7b-128">It might take five minutes to install the Azure CLI.</span></span>

   ```bash
   pip install --upgrade azure-cli
   pip install --upgrade azure-cli-iot
   ```
2. <span data-ttu-id="e8f7b-129">Ellenőrizze a telepítést a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="e8f7b-129">Verify the installation by running the following command:</span></span>

   ```bash
   az iot -h
   ```

<span data-ttu-id="e8f7b-130">Ha a telepítés sikerült a következő kimenetet kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="e8f7b-130">You should see the following output if the installation is successful.</span></span>

![Kimeneti sikerességét jelző][output]

## <a name="summary"></a><span data-ttu-id="e8f7b-132">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="e8f7b-132">Summary</span></span>
<span data-ttu-id="e8f7b-133">Az Azure parancssori felület telepítése.</span><span class="sxs-lookup"><span data-stu-id="e8f7b-133">You've installed the Azure CLI.</span></span> <span data-ttu-id="e8f7b-134">A következő feladathoz, ha az Azure IoT hub- és eszközidentitások az Azure parancssori felület használatával.</span><span class="sxs-lookup"><span data-stu-id="e8f7b-134">Your next task is to create your Azure IoT hub and device identity by using the Azure CLI.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e8f7b-135">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e8f7b-135">Next steps</span></span>
<span data-ttu-id="e8f7b-136">[Az IoT hub létrehozása és regisztrálása a Arduino tábla][create-your-iot-hub-and-register-your-arduino-board]</span><span class="sxs-lookup"><span data-stu-id="e8f7b-136">[Create your IoT hub and register your Arduino board][create-your-iot-hub-and-register-your-arduino-board]</span></span>


<!-- Images and links -->

[windows]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-get-azure-tools-win32.md
[ubuntu]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-get-azure-tools-ubuntu.md
[macos]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-get-azure-tools-mac.md
[output]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson2/az_iot_help_osx.png
[create-your-iot-hub-and-register-your-arduino-board]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-prepare-azure-iot-hub.md