---
title: "Csatlakozás málna Pi (csomópont) tooAzure IoT - lecke 2: eszközök (Ubuntu) beszerzése |} Microsoft Docs"
description: "Telepítse a Python, és az Azure parancssori felület (CLI) hello macOS a."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "az IOT felhőalapú szolgáltatás, az azure parancssori felület"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: 1814b703-2d81-45db-aff0-eb338c97f120
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 3f35fae53a360a758bc8f61ed2063f7e2f2dc067
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-azure-tools-macos-1010"></a><span data-ttu-id="0b938-104">Azure-eszközök (macOS 10.10) beolvasása</span><span class="sxs-lookup"><span data-stu-id="0b938-104">Get Azure tools (macOS 10.10)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="0b938-105">Windows 7 és újabb verziók</span><span class="sxs-lookup"><span data-stu-id="0b938-105">Windows 7 and later</span></span>](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-win32.md)
> * [<span data-ttu-id="0b938-106">Ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="0b938-106">Ubuntu 16.04</span></span>](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-ubuntu.md)
> * [<span data-ttu-id="0b938-107">macOS 10.10</span><span class="sxs-lookup"><span data-stu-id="0b938-107">macOS 10.10</span></span>](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-mac.md)

## <a name="what-you-will-do"></a><span data-ttu-id="0b938-108">Mit fog</span><span class="sxs-lookup"><span data-stu-id="0b938-108">What you will do</span></span>
<span data-ttu-id="0b938-109">Hello Azure parancssori felület (CLI) telepítése.</span><span class="sxs-lookup"><span data-stu-id="0b938-109">Install hello Azure command-line interface (Azure CLI).</span></span> <span data-ttu-id="0b938-110">Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="0b938-110">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="0b938-111">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="0b938-111">What you will learn</span></span>
<span data-ttu-id="0b938-112">Ebből a cikkből megtudhatja:</span><span class="sxs-lookup"><span data-stu-id="0b938-112">In this article, you will learn:</span></span>
* <span data-ttu-id="0b938-113">Hogyan tooinstall Azure parancssori felület.</span><span class="sxs-lookup"><span data-stu-id="0b938-113">How tooinstall Azure CLI.</span></span>
* <span data-ttu-id="0b938-114">Hogyan tooadd hello Azure parancssori felület egy IoT ablaktábla.</span><span class="sxs-lookup"><span data-stu-id="0b938-114">How tooadd an IoT subgroup of hello Azure CLI.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="0b938-115">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="0b938-115">What you need</span></span>
* <span data-ttu-id="0b938-116">A Mac, az internetet.</span><span class="sxs-lookup"><span data-stu-id="0b938-116">A Mac with an Internet connection.</span></span>
* <span data-ttu-id="0b938-117">Aktív Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="0b938-117">An active Azure subscription.</span></span> <span data-ttu-id="0b938-118">Ha az Azure-fiók nem rendelkezik, akkor létrehozhat egy [ingyenes Azure próba-fiókot](http://azure.microsoft.com/pricing/free-trial/) csak néhány perc múlva.</span><span class="sxs-lookup"><span data-stu-id="0b938-118">If you don't have an Azure account, you can create a [free Azure trial account](http://azure.microsoft.com/pricing/free-trial/) in just a few minutes.</span></span>

## <a name="install-python"></a><span data-ttu-id="0b938-119">Python telepítése</span><span class="sxs-lookup"><span data-stu-id="0b938-119">Install Python</span></span>
<span data-ttu-id="0b938-120">Bár macOS Python 2.7 hello kezdő verzióról, azt javasoljuk, hogy telepítse a Python Homebrew keresztül.</span><span class="sxs-lookup"><span data-stu-id="0b938-120">Although macOS comes with Python 2.7 out of hello box, we recommend that you install Python through Homebrew.</span></span> <span data-ttu-id="0b938-121">Lásd: [Python telepítése a macOS](http://docs.python-guide.org/en/latest/starting/install/osx/).</span><span class="sxs-lookup"><span data-stu-id="0b938-121">See [Installing Python on macOS](http://docs.python-guide.org/en/latest/starting/install/osx/).</span></span>

<span data-ttu-id="0b938-122">Telepítse a Python és a pip hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="0b938-122">Install Python and pip by running hello following command:</span></span>

```bash
brew install python
```

## <a name="install-hello-azure-cli"></a><span data-ttu-id="0b938-123">Hello Azure parancssori felület telepítése</span><span class="sxs-lookup"><span data-stu-id="0b938-123">Install hello Azure CLI</span></span>
<span data-ttu-id="0b938-124">hello Azure CLI olyan többplatformos parancssori környezetet biztosít az Azure.</span><span class="sxs-lookup"><span data-stu-id="0b938-124">hello Azure CLI provides a multiplatform command-line experience for Azure.</span></span> <span data-ttu-id="0b938-125">Közvetlenül a parancssori tooprovision dolgozhassanak és kezelheti az erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="0b938-125">You work directly from your command-line tooprovision and manage resources.</span></span> 

<span data-ttu-id="0b938-126">tooinstall hello Azure CLI legújabb, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="0b938-126">tooinstall hello latest Azure CLI, follow these steps:</span></span>

1. <span data-ttu-id="0b938-127">Futtassa a következő parancsokat egy terminálablakot hello.</span><span class="sxs-lookup"><span data-stu-id="0b938-127">Run hello following commands in a terminal window.</span></span> <span data-ttu-id="0b938-128">Öt perc tooinstall hello Azure CLI vehet igénybe.</span><span class="sxs-lookup"><span data-stu-id="0b938-128">It might take five minutes tooinstall hello Azure CLI.</span></span>

   ```bash
   pip install --upgrade azure-cli
   pip install --upgrade azure-cli-iot
   ```
2. <span data-ttu-id="0b938-129">Ellenőrizze a hello telepítést hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="0b938-129">Verify hello installation by running hello following command:</span></span>

   ```bash
   az iot -h
   ```

<span data-ttu-id="0b938-130">Meg kell jelennie a hello parancskimenet sikeres hello telepítés esetén.</span><span class="sxs-lookup"><span data-stu-id="0b938-130">You should see hello following output if hello installation is successful.</span></span>

![Kimeneti sikerességét jelző](media/iot-hub-raspberry-pi-lessons/lesson2/az_iot_help_osx.png)

## <a name="summary"></a><span data-ttu-id="0b938-132">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="0b938-132">Summary</span></span>
<span data-ttu-id="0b938-133">Hello Azure parancssori felület telepítése.</span><span class="sxs-lookup"><span data-stu-id="0b938-133">You've installed hello Azure CLI.</span></span> <span data-ttu-id="0b938-134">A következő feladathoz toocreate az Azure IoT hub- és eszközidentitások használatával hello Azure parancssori felület.</span><span class="sxs-lookup"><span data-stu-id="0b938-134">Your next task is toocreate your Azure IoT hub and device identity by using hello Azure CLI.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0b938-135">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0b938-135">Next steps</span></span>
[<span data-ttu-id="0b938-136">Az IoT hub létrehozni és regisztrálni az málna Pi 3</span><span class="sxs-lookup"><span data-stu-id="0b938-136">Create your IoT hub and register Raspberry Pi 3</span></span>](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md)

