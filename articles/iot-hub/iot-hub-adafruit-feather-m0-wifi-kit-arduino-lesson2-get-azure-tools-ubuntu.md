---
title: "Csatlakozás Azure IoT - lecke 2 Arduino: Azure-eszközök (Ubuntu) |} Microsoft Docs"
description: "Ubuntu Python és az Azure parancssori felület (CLI) telepíthető."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "az Azure cli, iot felhőalapú szolgáltatás, arduino felhő"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: bb5cb602-7292-4772-ac90-c0b52ebc8340
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: a2f83e59a37abc3f44e770b22ac089b88481a6a4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-azure-tools-ubuntu-1604"></a><span data-ttu-id="71f26-104">Azure-eszközök (Ubuntu 16.04) beolvasása</span><span class="sxs-lookup"><span data-stu-id="71f26-104">Get Azure tools (Ubuntu 16.04)</span></span>

> [!div class="op_single_selector"]
> * <span data-ttu-id="71f26-105">[Windows 7 vagy újabb][windows]</span><span class="sxs-lookup"><span data-stu-id="71f26-105">[Windows 7 or later][windows]</span></span>
> * <span data-ttu-id="71f26-106">[Ubuntu 16.04][ubuntu]</span><span class="sxs-lookup"><span data-stu-id="71f26-106">[Ubuntu 16.04][ubuntu]</span></span>
> * <span data-ttu-id="71f26-107">[macOS 10.10][macos]</span><span class="sxs-lookup"><span data-stu-id="71f26-107">[macOS 10.10][macos]</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="71f26-108">Mit fog</span><span class="sxs-lookup"><span data-stu-id="71f26-108">What you will do</span></span>

<span data-ttu-id="71f26-109">Telepítse az Azure parancssori felület (CLI).</span><span class="sxs-lookup"><span data-stu-id="71f26-109">Install the Azure command-line interface (Azure CLI).</span></span> <span data-ttu-id="71f26-110">Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md) a Adafruit lágyított M0 Wi-Fi Arduino kártya.</span><span class="sxs-lookup"><span data-stu-id="71f26-110">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md) for your Adafruit Feather M0 WiFi Arduino board.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="71f26-111">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="71f26-111">What you will learn</span></span>
<span data-ttu-id="71f26-112">Ebből a cikkből megtudhatja:</span><span class="sxs-lookup"><span data-stu-id="71f26-112">In this article, you will learn:</span></span>
* <span data-ttu-id="71f26-113">Tudnivalók az Azure parancssori felület telepítése.</span><span class="sxs-lookup"><span data-stu-id="71f26-113">How to install the Azure CLI.</span></span>
* <span data-ttu-id="71f26-114">Hogyan lehet az Azure parancssori felület egy IoT részcsoport hozzáadni.</span><span class="sxs-lookup"><span data-stu-id="71f26-114">How to add an IoT subgroup of the Azure CLI.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="71f26-115">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="71f26-115">What you need</span></span>
* <span data-ttu-id="71f26-116">Egy Ubuntu számítógép az internethez.</span><span class="sxs-lookup"><span data-stu-id="71f26-116">An Ubuntu computer with an Internet connection.</span></span>
* <span data-ttu-id="71f26-117">Aktív Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="71f26-117">An active Azure subscription.</span></span> <span data-ttu-id="71f26-118">Ha nincs fiókja, létrehozhat egy [ingyenes próbafiók](http://azure.microsoft.com/pricing/free-trial/) csak néhány perc múlva.</span><span class="sxs-lookup"><span data-stu-id="71f26-118">If you don't have an account, you can create a [free trial account](http://azure.microsoft.com/pricing/free-trial/) in just a few minutes.</span></span>

## <a name="install-the-azure-cli"></a><span data-ttu-id="71f26-119">Telepítse az Azure CLI-t</span><span class="sxs-lookup"><span data-stu-id="71f26-119">Install the Azure CLI</span></span>
<span data-ttu-id="71f26-120">Az Azure parancssori felület olyan többplatformos parancssori környezetet biztosít az Azure-ra, így közvetlenül a parancssorból kiépítését működik, és kezelheti az erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="71f26-120">The Azure CLI provides a multiplatform command-line experience for Azure, enabling you to work directly from your command line to provision and manage resources.</span></span>

<span data-ttu-id="71f26-121">Az Azure CLI legújabb telepítéséhez kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="71f26-121">To install the latest Azure CLI, follow these steps:</span></span>

1. <span data-ttu-id="71f26-122">Futtassa a következő parancsokat egy terminálablakot.</span><span class="sxs-lookup"><span data-stu-id="71f26-122">Run the following commands in a terminal window.</span></span> <span data-ttu-id="71f26-123">Az Azure parancssori felület telepítése öt percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="71f26-123">It might take five minutes to install the Azure CLI.</span></span>

   ```bash
   sudo apt-get update
   sudo apt-get install -y libssl-dev libffi-dev
   sudo apt-get install -y python-dev
   sudo apt-get install -y build-essential
   sudo apt-get install -y python-pip
   sudo pip install --upgrade azure-cli
   sudo pip install --upgrade azure-cli-iot
   ```
2. <span data-ttu-id="71f26-124">Ellenőrizze a telepítést a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="71f26-124">Verify the installation by running the following command:</span></span>

   ```bash
   az iot -h
   ```

<span data-ttu-id="71f26-125">Ha a telepítés sikerült a következő kimenetet kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="71f26-125">You should see the following output if the installation is successful.</span></span>

![Kimeneti sikerességét jelző][output]

## <a name="summary"></a><span data-ttu-id="71f26-127">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="71f26-127">Summary</span></span>
<span data-ttu-id="71f26-128">Az Azure parancssori felület telepítése.</span><span class="sxs-lookup"><span data-stu-id="71f26-128">You've installed the Azure CLI.</span></span> <span data-ttu-id="71f26-129">A következő feladathoz, ha az Azure IoT hub- és eszközidentitások az Azure parancssori felület használatával.</span><span class="sxs-lookup"><span data-stu-id="71f26-129">Your next task is to create your Azure IoT hub and device identity using the Azure CLI.</span></span>

## <a name="next-steps"></a><span data-ttu-id="71f26-130">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="71f26-130">Next steps</span></span>
<span data-ttu-id="71f26-131">[Az IoT hub létrehozása és regisztrálása a Arduino tábla][create-your-iot-hub-and-register-your-arduino-board]</span><span class="sxs-lookup"><span data-stu-id="71f26-131">[Create your IoT hub and register your Arduino board][create-your-iot-hub-and-register-your-arduino-board]</span></span>
<!-- Images and links -->

[windows]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-get-azure-tools-win32.md
[ubuntu]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-get-azure-tools-ubuntu.md
[macos]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-get-azure-tools-mac.md
[output]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson2/az_iot_help_ubuntu.png
[create-your-iot-hub-and-register-your-arduino-board]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-prepare-azure-iot-hub.md