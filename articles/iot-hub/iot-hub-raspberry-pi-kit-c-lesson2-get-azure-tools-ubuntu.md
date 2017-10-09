---
title: "Connect Raspberry pi (C) tooAzure IoT - lecke 2: Azure-eszközök (Ubuntu) |} Microsoft Docs"
description: "Ubuntu Python és az Azure parancssori felület (CLI) telepíthető."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "az IOT felhőalapú szolgáltatás, az azure parancssori felület"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started
ms.assetid: a03512f2-fabe-40c5-8505-b93eef8e5bec
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 1af4848f9fe0508e362c15b36eec8a35aea9ac5b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-azure-tools-ubuntu-1604"></a><span data-ttu-id="6c6e5-104">Azure-eszközök (Ubuntu 16.04) beolvasása</span><span class="sxs-lookup"><span data-stu-id="6c6e5-104">Get Azure tools (Ubuntu 16.04)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="6c6e5-105">Windows 7 és újabb verziók</span><span class="sxs-lookup"><span data-stu-id="6c6e5-105">Windows 7 and later</span></span>](iot-hub-raspberry-pi-kit-c-lesson2-get-azure-tools-win32.md)
> * [<span data-ttu-id="6c6e5-106">Ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="6c6e5-106">Ubuntu 16.04</span></span>](iot-hub-raspberry-pi-kit-c-lesson2-get-azure-tools-ubuntu.md)
> * [<span data-ttu-id="6c6e5-107">macOS 10.10</span><span class="sxs-lookup"><span data-stu-id="6c6e5-107">macOS 10.10</span></span>](iot-hub-raspberry-pi-kit-c-lesson2-get-azure-tools-mac.md)

## <a name="what-you-will-do"></a><span data-ttu-id="6c6e5-108">Mit fog</span><span class="sxs-lookup"><span data-stu-id="6c6e5-108">What you will do</span></span>
<span data-ttu-id="6c6e5-109">Hello Azure parancssori felület (CLI) telepítése.</span><span class="sxs-lookup"><span data-stu-id="6c6e5-109">Install hello Azure command-line interface (Azure CLI).</span></span> <span data-ttu-id="6c6e5-110">Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="6c6e5-110">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="6c6e5-111">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="6c6e5-111">What you will learn</span></span>
<span data-ttu-id="6c6e5-112">Ebből a cikkből megtudhatja:</span><span class="sxs-lookup"><span data-stu-id="6c6e5-112">In this article, you will learn:</span></span>
* <span data-ttu-id="6c6e5-113">Hogyan tooinstall hello Azure parancssori felület.</span><span class="sxs-lookup"><span data-stu-id="6c6e5-113">How tooinstall hello Azure CLI.</span></span>
* <span data-ttu-id="6c6e5-114">Hogyan tooadd hello Azure parancssori felület egy IoT ablaktábla.</span><span class="sxs-lookup"><span data-stu-id="6c6e5-114">How tooadd an IoT subgroup of hello Azure CLI.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="6c6e5-115">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="6c6e5-115">What you need</span></span>
* <span data-ttu-id="6c6e5-116">Egy Ubuntu számítógép az internethez.</span><span class="sxs-lookup"><span data-stu-id="6c6e5-116">An Ubuntu computer with an Internet connection.</span></span>
* <span data-ttu-id="6c6e5-117">Aktív Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="6c6e5-117">An active Azure subscription.</span></span> <span data-ttu-id="6c6e5-118">Ha nincs fiókja, létrehozhat egy [ingyenes próbafiók](http://azure.microsoft.com/pricing/free-trial/) csak néhány perc múlva.</span><span class="sxs-lookup"><span data-stu-id="6c6e5-118">If you don't have an account, you can create a [free trial account](http://azure.microsoft.com/pricing/free-trial/) in just a few minutes.</span></span>

## <a name="install-hello-azure-cli"></a><span data-ttu-id="6c6e5-119">Hello Azure parancssori felület telepítése</span><span class="sxs-lookup"><span data-stu-id="6c6e5-119">Install hello Azure CLI</span></span>
<span data-ttu-id="6c6e5-120">hello Azure CLI olyan többplatformos parancssori környezetet biztosít az Azure, amely lehetővé teszi, a parancssor tooprovision közvetlenül a toowork, és kezelheti az erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="6c6e5-120">hello Azure CLI provides a multiplatform command-line experience for Azure, enabling you toowork directly from your command line tooprovision and manage resources.</span></span>

<span data-ttu-id="6c6e5-121">tooinstall hello Azure CLI legújabb, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="6c6e5-121">tooinstall hello latest Azure CLI, follow these steps:</span></span>

1. <span data-ttu-id="6c6e5-122">Futtassa a következő parancsokat egy terminálablakot hello.</span><span class="sxs-lookup"><span data-stu-id="6c6e5-122">Run hello following commands in a terminal window.</span></span> <span data-ttu-id="6c6e5-123">Öt perc tooinstall hello Azure CLI vehet igénybe.</span><span class="sxs-lookup"><span data-stu-id="6c6e5-123">It might take five minutes tooinstall hello Azure CLI.</span></span>

   ```bash
   sudo apt-get update
   sudo apt-get install -y libssl-dev libffi-dev
   sudo apt-get install -y python-dev
   sudo apt-get install -y build-essential
   sudo apt-get install -y python-pip
   sudo pip install --upgrade azure-cli
   sudo pip install --upgrade azure-cli-iot
   ```
2. <span data-ttu-id="6c6e5-124">Ellenőrizze a hello telepítést hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="6c6e5-124">Verify hello installation by running hello following command:</span></span>

   ```bash
   az iot -h
   ```

<span data-ttu-id="6c6e5-125">Meg kell jelennie a hello parancskimenet sikeres hello telepítés esetén.</span><span class="sxs-lookup"><span data-stu-id="6c6e5-125">You should see hello following output if hello installation is successful.</span></span>

![Kimeneti sikerességét jelző](media/iot-hub-raspberry-pi-lessons/lesson2/az_iot_help_ubuntu.png)

## <a name="summary"></a><span data-ttu-id="6c6e5-127">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="6c6e5-127">Summary</span></span>
<span data-ttu-id="6c6e5-128">Hello Azure parancssori felület telepítése.</span><span class="sxs-lookup"><span data-stu-id="6c6e5-128">You've installed hello Azure CLI.</span></span> <span data-ttu-id="6c6e5-129">A következő feladathoz toocreate a Azure IoT-központ és az eszköz identitása használt hello Azure parancssori felület.</span><span class="sxs-lookup"><span data-stu-id="6c6e5-129">Your next task is toocreate your Azure IoT hub and device identity using hello Azure CLI.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6c6e5-130">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6c6e5-130">Next steps</span></span>
[<span data-ttu-id="6c6e5-131">Az IoT hub létrehozni és regisztrálni az málna Pi 3</span><span class="sxs-lookup"><span data-stu-id="6c6e5-131">Create your IoT hub and register Raspberry Pi 3</span></span>](iot-hub-raspberry-pi-kit-c-lesson2-prepare-azure-iot-hub.md)

