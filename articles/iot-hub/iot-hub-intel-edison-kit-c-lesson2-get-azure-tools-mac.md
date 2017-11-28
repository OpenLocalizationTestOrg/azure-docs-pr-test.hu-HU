---
title: "Connect Intel Edison (C) tooAzure IoT - lecke 2: Azure-eszközök (macOS) |} Microsoft Docs"
description: "MacOS Python és az Azure parancssori felület (CLI) telepíthető."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "az Azure cli, iot felhőalapú szolgáltatás, arduino felhő"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-c-get-started
ms.assetid: d561680f-69cc-427a-820d-24f710ba05a8
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 0dfc9ff90e879d5fd03040016ac71a9fe4f4a744
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-azure-tools-macos-1010"></a><span data-ttu-id="3dbee-104">Azure-eszközök (macOS 10.10) beolvasása</span><span class="sxs-lookup"><span data-stu-id="3dbee-104">Get Azure tools (macOS 10.10)</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="3dbee-105">[Windows 7 és újabb verziók][windows]</span><span class="sxs-lookup"><span data-stu-id="3dbee-105">[Windows 7 and later][windows]</span></span>
> * <span data-ttu-id="3dbee-106">[Ubuntu 16.04][ubuntu]</span><span class="sxs-lookup"><span data-stu-id="3dbee-106">[Ubuntu 16.04][ubuntu]</span></span>
> * <span data-ttu-id="3dbee-107">[macOS 10.10][macos]</span><span class="sxs-lookup"><span data-stu-id="3dbee-107">[macOS 10.10][macos]</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="3dbee-108">Mit fog</span><span class="sxs-lookup"><span data-stu-id="3dbee-108">What you will do</span></span>
<span data-ttu-id="3dbee-109">Hello Azure parancssori felület (CLI) telepítése.</span><span class="sxs-lookup"><span data-stu-id="3dbee-109">Install hello Azure command-line interface (Azure CLI).</span></span> <span data-ttu-id="3dbee-110">Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="3dbee-110">If you have any problems, look for solutions on hello [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="3dbee-111">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="3dbee-111">What you will learn</span></span>
<span data-ttu-id="3dbee-112">Ebből a cikkből megtudhatja:</span><span class="sxs-lookup"><span data-stu-id="3dbee-112">In this article, you will learn:</span></span>
* <span data-ttu-id="3dbee-113">Hogyan tooinstall Azure parancssori felület.</span><span class="sxs-lookup"><span data-stu-id="3dbee-113">How tooinstall Azure CLI.</span></span>
* <span data-ttu-id="3dbee-114">Hogyan tooadd hello Azure parancssori felület egy IoT ablaktábla.</span><span class="sxs-lookup"><span data-stu-id="3dbee-114">How tooadd an IoT subgroup of hello Azure CLI.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="3dbee-115">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="3dbee-115">What you need</span></span>
* <span data-ttu-id="3dbee-116">A Mac, az internetet.</span><span class="sxs-lookup"><span data-stu-id="3dbee-116">A Mac with an Internet connection.</span></span>
* <span data-ttu-id="3dbee-117">Aktív Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="3dbee-117">An active Azure subscription.</span></span> <span data-ttu-id="3dbee-118">Ha az Azure-fiók nem rendelkezik, akkor létrehozhat egy [ingyenes Azure próba-fiókot](http://azure.microsoft.com/pricing/free-trial/) csak néhány perc múlva.</span><span class="sxs-lookup"><span data-stu-id="3dbee-118">If you don't have an Azure account, you can create a [free Azure trial account](http://azure.microsoft.com/pricing/free-trial/) in just a few minutes.</span></span>

## <a name="install-python"></a><span data-ttu-id="3dbee-119">Python telepítése</span><span class="sxs-lookup"><span data-stu-id="3dbee-119">Install Python</span></span>
<span data-ttu-id="3dbee-120">Bár macOS Python 2.7 hello kezdő verzióról, azt javasoljuk, hogy telepítse a Python Homebrew keresztül.</span><span class="sxs-lookup"><span data-stu-id="3dbee-120">Although macOS comes with Python 2.7 out of hello box, we recommend that you install Python through Homebrew.</span></span> <span data-ttu-id="3dbee-121">Lásd: [Python telepítése a macOS](http://docs.python-guide.org/en/latest/starting/install/osx/).</span><span class="sxs-lookup"><span data-stu-id="3dbee-121">See [Installing Python on macOS](http://docs.python-guide.org/en/latest/starting/install/osx/).</span></span>

<span data-ttu-id="3dbee-122">Telepítse a Python és a pip hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="3dbee-122">Install Python and pip by running hello following command:</span></span>

```bash
brew install python
```

## <a name="install-hello-azure-cli"></a><span data-ttu-id="3dbee-123">Hello Azure parancssori felület telepítése</span><span class="sxs-lookup"><span data-stu-id="3dbee-123">Install hello Azure CLI</span></span>
<span data-ttu-id="3dbee-124">hello Azure CLI olyan többplatformos parancssori környezetet biztosít az Azure.</span><span class="sxs-lookup"><span data-stu-id="3dbee-124">hello Azure CLI provides a multiplatform command-line experience for Azure.</span></span> <span data-ttu-id="3dbee-125">Közvetlenül a parancssor tooprovision dolgozhassanak és kezelheti az erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="3dbee-125">You work directly from your command line tooprovision and manage resources.</span></span> 

<span data-ttu-id="3dbee-126">tooinstall hello Azure CLI legújabb, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="3dbee-126">tooinstall hello latest Azure CLI, follow these steps:</span></span>

1. <span data-ttu-id="3dbee-127">Futtassa a következő parancsokat egy terminálablakot hello.</span><span class="sxs-lookup"><span data-stu-id="3dbee-127">Run hello following commands in a terminal window.</span></span> <span data-ttu-id="3dbee-128">Öt perc tooinstall hello Azure CLI vehet igénybe.</span><span class="sxs-lookup"><span data-stu-id="3dbee-128">It might take five minutes tooinstall hello Azure CLI.</span></span>

   ```bash
   pip install --upgrade azure-cli
   pip install --upgrade azure-cli-iot
   ```
2. <span data-ttu-id="3dbee-129">Ellenőrizze a hello telepítést hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="3dbee-129">Verify hello installation by running hello following command:</span></span>

   ```bash
   az iot -h
   ```

<span data-ttu-id="3dbee-130">Meg kell jelennie a hello parancskimenet sikeres hello telepítés esetén.</span><span class="sxs-lookup"><span data-stu-id="3dbee-130">You should see hello following output if hello installation is successful.</span></span>

![Kimeneti sikerességét jelző](media/iot-hub-intel-edison-lessons/lesson2/az_iot_help_osx.png)

## <a name="summary"></a><span data-ttu-id="3dbee-132">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="3dbee-132">Summary</span></span>
<span data-ttu-id="3dbee-133">Hello Azure parancssori felület telepítése.</span><span class="sxs-lookup"><span data-stu-id="3dbee-133">You've installed hello Azure CLI.</span></span> <span data-ttu-id="3dbee-134">A következő feladathoz toocreate az Azure IoT hub- és eszközidentitások használatával hello Azure parancssori felület.</span><span class="sxs-lookup"><span data-stu-id="3dbee-134">Your next task is toocreate your Azure IoT hub and device identity by using hello Azure CLI.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3dbee-135">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3dbee-135">Next steps</span></span>
<span data-ttu-id="3dbee-136">[Az IoT hub létrehozni és regisztrálni az Intel Edison][create-your-iot-hub-and-register-intel-edison]</span><span class="sxs-lookup"><span data-stu-id="3dbee-136">[Create your IoT hub and register Intel Edison][create-your-iot-hub-and-register-intel-edison]</span></span>
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md
[create-your-iot-hub-and-register-intel-edison]: iot-hub-intel-edison-kit-c-lesson2-prepare-azure-iot-hub.md
[windows]: iot-hub-intel-edison-kit-c-lesson2-get-azure-tools-win32.md
[ubuntu]: iot-hub-intel-edison-kit-c-lesson2-get-azure-tools-ubuntu.md
[macos]: iot-hub-intel-edison-kit-c-lesson2-get-azure-tools-mac.md
