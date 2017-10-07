---
title: "Csatlakozás málna Pi (csomópont) tooAzure IoT - lecke 2: eszközök (Ubuntu) beszerzése |} Microsoft Docs"
description: "Python telepítse, és az Azure parancssori felület (CLI) hello az Ubuntu."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "az IOT felhőalapú szolgáltatás, az azure parancssori felület"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: 2f98923a-5274-4220-87d4-77ac8beb4d0f
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 0adf91bef41f502e1333fdcc75cfb2fe912364c9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-azure-tools-ubuntu-1604"></a><span data-ttu-id="71095-104">Azure-eszközök (Ubuntu 16.04) beolvasása</span><span class="sxs-lookup"><span data-stu-id="71095-104">Get Azure tools (Ubuntu 16.04)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="71095-105">Windows 7 és újabb verziók</span><span class="sxs-lookup"><span data-stu-id="71095-105">Windows 7 and later</span></span>](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-win32.md)
> * [<span data-ttu-id="71095-106">Ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="71095-106">Ubuntu 16.04</span></span>](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-ubuntu.md)
> * [<span data-ttu-id="71095-107">macOS 10.10</span><span class="sxs-lookup"><span data-stu-id="71095-107">macOS 10.10</span></span>](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-mac.md)

## <a name="what-you-will-do"></a><span data-ttu-id="71095-108">Mit fog</span><span class="sxs-lookup"><span data-stu-id="71095-108">What you will do</span></span>
<span data-ttu-id="71095-109">Hello Azure parancssori felület (CLI) telepítése.</span><span class="sxs-lookup"><span data-stu-id="71095-109">Install hello Azure command-line interface (Azure CLI).</span></span> <span data-ttu-id="71095-110">Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="71095-110">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="71095-111">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="71095-111">What you will learn</span></span>
<span data-ttu-id="71095-112">Ebből a cikkből megtudhatja:</span><span class="sxs-lookup"><span data-stu-id="71095-112">In this article, you will learn:</span></span>
* <span data-ttu-id="71095-113">Hogyan tooinstall hello Azure parancssori felület.</span><span class="sxs-lookup"><span data-stu-id="71095-113">How tooinstall hello Azure CLI.</span></span>
* <span data-ttu-id="71095-114">Hogyan tooadd hello Azure parancssori felület egy IoT ablaktábla.</span><span class="sxs-lookup"><span data-stu-id="71095-114">How tooadd an IoT subgroup of hello Azure CLI.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="71095-115">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="71095-115">What you need</span></span>
* <span data-ttu-id="71095-116">Egy Ubuntu számítógép az internethez.</span><span class="sxs-lookup"><span data-stu-id="71095-116">An Ubuntu computer with an Internet connection.</span></span>
* <span data-ttu-id="71095-117">Aktív Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="71095-117">An active Azure subscription.</span></span> <span data-ttu-id="71095-118">Ha nincs fiókja, létrehozhat egy [ingyenes próbafiók](http://azure.microsoft.com/pricing/free-trial/) csak néhány perc múlva.</span><span class="sxs-lookup"><span data-stu-id="71095-118">If you don't have an account, you can create a [free trial account](http://azure.microsoft.com/pricing/free-trial/) in just a few minutes.</span></span>

## <a name="install-hello-azure-cli"></a><span data-ttu-id="71095-119">Hello Azure parancssori felület telepítése</span><span class="sxs-lookup"><span data-stu-id="71095-119">Install hello Azure CLI</span></span>
<span data-ttu-id="71095-120">hello Azure CLI olyan többplatformos parancssori környezetet biztosít az Azure-ra, így lehetővé teszi a parancssori tooprovision közvetlenül a toowork, és kezelheti az erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="71095-120">hello Azure CLI provides a multiplatform command-line experience for Azure, enabling you toowork directly from your command-line tooprovision and manage resources.</span></span>

<span data-ttu-id="71095-121">tooinstall hello Azure CLI legújabb, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="71095-121">tooinstall hello latest Azure CLI, follow these steps:</span></span>

1. <span data-ttu-id="71095-122">Futtassa a következő parancsokat egy terminálablakot hello.</span><span class="sxs-lookup"><span data-stu-id="71095-122">Run hello following commands in a terminal window.</span></span> <span data-ttu-id="71095-123">Öt perc tooinstall hello Azure CLI vehet igénybe.</span><span class="sxs-lookup"><span data-stu-id="71095-123">It might take five minutes tooinstall hello Azure CLI.</span></span>

   ```bash
   sudo apt-get update
   sudo apt-get install -y libssl-dev libffi-dev
   sudo apt-get install -y python-dev
   sudo apt-get install -y build-essential
   sudo apt-get install -y python-pip
   sudo pip install --upgrade azure-cli
   sudo pip install --upgrade azure-cli-iot
   ```
2. <span data-ttu-id="71095-124">Ellenőrizze a hello telepítést hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="71095-124">Verify hello installation by running hello following command:</span></span>

   ```bash
   az iot -h
   ```

<span data-ttu-id="71095-125">Meg kell jelennie a hello parancskimenet sikeres hello telepítés esetén.</span><span class="sxs-lookup"><span data-stu-id="71095-125">You should see hello following output if hello installation is successful.</span></span>

![Kimeneti sikerességét jelző](media/iot-hub-raspberry-pi-lessons/lesson2/az_iot_help_ubuntu.png)

## <a name="summary"></a><span data-ttu-id="71095-127">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="71095-127">Summary</span></span>
<span data-ttu-id="71095-128">Hello Azure parancssori felület telepítése.</span><span class="sxs-lookup"><span data-stu-id="71095-128">You've installed hello Azure CLI.</span></span> <span data-ttu-id="71095-129">A következő feladathoz toocreate a Azure IoT-központ és az eszköz identitása használt hello Azure parancssori felület.</span><span class="sxs-lookup"><span data-stu-id="71095-129">Your next task is toocreate your Azure IoT hub and device identity using hello Azure CLI.</span></span>

## <a name="next-steps"></a><span data-ttu-id="71095-130">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="71095-130">Next steps</span></span>
[<span data-ttu-id="71095-131">Az IoT hub létrehozni és regisztrálni az málna Pi 3</span><span class="sxs-lookup"><span data-stu-id="71095-131">Create your IoT hub and register Raspberry Pi 3</span></span>](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md)

