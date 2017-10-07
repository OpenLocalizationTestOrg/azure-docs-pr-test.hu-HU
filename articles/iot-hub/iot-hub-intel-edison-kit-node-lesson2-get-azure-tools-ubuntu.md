---
title: "Csatlakozás Intel Edison (csomópont) tooAzure IoT - lecke 2: Azure-eszközök (Ubuntu) |} Microsoft Docs"
description: "Ubuntu Python és az Azure parancssori felület (CLI) telepíthető."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "az Azure cli, iot felhőalapú szolgáltatás, arduino felhő"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-node-get-started
ms.assetid: 6dcb34bf-54a3-4af0-ba89-95d5cfafceff
ms.service: iot-hub
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: ca2996b779a4d3cde833c5f2824d19ec46241bae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-azure-tools-ubuntu-1604"></a><span data-ttu-id="09c4e-104">Azure-eszközök (Ubuntu 16.04) beolvasása</span><span class="sxs-lookup"><span data-stu-id="09c4e-104">Get Azure tools (Ubuntu 16.04)</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="09c4e-105">[Windows 7 és újabb verziók][windows]</span><span class="sxs-lookup"><span data-stu-id="09c4e-105">[Windows 7 and later][windows]</span></span>
> * <span data-ttu-id="09c4e-106">[Ubuntu 16.04][ubuntu]</span><span class="sxs-lookup"><span data-stu-id="09c4e-106">[Ubuntu 16.04][ubuntu]</span></span>
> * <span data-ttu-id="09c4e-107">[macOS 10.10][macos]</span><span class="sxs-lookup"><span data-stu-id="09c4e-107">[macOS 10.10][macos]</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="09c4e-108">Mit fog</span><span class="sxs-lookup"><span data-stu-id="09c4e-108">What you will do</span></span>
<span data-ttu-id="09c4e-109">Hello Azure parancssori felület (CLI) telepítése.</span><span class="sxs-lookup"><span data-stu-id="09c4e-109">Install hello Azure command-line interface (Azure CLI).</span></span> <span data-ttu-id="09c4e-110">Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="09c4e-110">If you have any problems, look for solutions on hello [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="09c4e-111">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="09c4e-111">What you will learn</span></span>
<span data-ttu-id="09c4e-112">Ebből a cikkből megtudhatja:</span><span class="sxs-lookup"><span data-stu-id="09c4e-112">In this article, you will learn:</span></span>
* <span data-ttu-id="09c4e-113">Hogyan tooinstall hello Azure parancssori felület.</span><span class="sxs-lookup"><span data-stu-id="09c4e-113">How tooinstall hello Azure CLI.</span></span>
* <span data-ttu-id="09c4e-114">Hogyan tooadd hello Azure parancssori felület egy IoT ablaktábla.</span><span class="sxs-lookup"><span data-stu-id="09c4e-114">How tooadd an IoT subgroup of hello Azure CLI.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="09c4e-115">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="09c4e-115">What you need</span></span>
* <span data-ttu-id="09c4e-116">Egy Ubuntu számítógép az internethez.</span><span class="sxs-lookup"><span data-stu-id="09c4e-116">An Ubuntu computer with an Internet connection.</span></span>
* <span data-ttu-id="09c4e-117">Aktív Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="09c4e-117">An active Azure subscription.</span></span> <span data-ttu-id="09c4e-118">Ha nincs fiókja, létrehozhat egy [ingyenes próbafiók](http://azure.microsoft.com/pricing/free-trial/) csak néhány perc múlva.</span><span class="sxs-lookup"><span data-stu-id="09c4e-118">If you don't have an account, you can create a [free trial account](http://azure.microsoft.com/pricing/free-trial/) in just a few minutes.</span></span>

## <a name="install-hello-azure-cli"></a><span data-ttu-id="09c4e-119">Hello Azure parancssori felület telepítése</span><span class="sxs-lookup"><span data-stu-id="09c4e-119">Install hello Azure CLI</span></span>
<span data-ttu-id="09c4e-120">hello Azure CLI olyan többplatformos parancssori környezetet biztosít az Azure, amely lehetővé teszi, a parancssor tooprovision közvetlenül a toowork, és kezelheti az erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="09c4e-120">hello Azure CLI provides a multiplatform command-line experience for Azure, enabling you toowork directly from your command line tooprovision and manage resources.</span></span>

<span data-ttu-id="09c4e-121">tooinstall hello Azure CLI legújabb, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="09c4e-121">tooinstall hello latest Azure CLI, follow these steps:</span></span>

1. <span data-ttu-id="09c4e-122">Futtassa a következő parancsokat egy terminálablakot hello.</span><span class="sxs-lookup"><span data-stu-id="09c4e-122">Run hello following commands in a terminal window.</span></span> <span data-ttu-id="09c4e-123">Öt perc tooinstall hello Azure CLI vehet igénybe.</span><span class="sxs-lookup"><span data-stu-id="09c4e-123">It might take five minutes tooinstall hello Azure CLI.</span></span>

   ```bash
   sudo apt-get update
   sudo apt-get install -y libssl-dev libffi-dev
   sudo apt-get install -y python-dev
   sudo apt-get install -y build-essential
   sudo apt-get install -y python-pip
   sudo pip install --upgrade azure-cli
   sudo pip install --upgrade azure-cli-iot
   ```
2. <span data-ttu-id="09c4e-124">Ellenőrizze a hello telepítést hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="09c4e-124">Verify hello installation by running hello following command:</span></span>

   ```bash
   az iot -h
   ```

<span data-ttu-id="09c4e-125">Meg kell jelennie a hello parancskimenet sikeres hello telepítés esetén.</span><span class="sxs-lookup"><span data-stu-id="09c4e-125">You should see hello following output if hello installation is successful.</span></span>

![Kimeneti sikerességét jelző](media/iot-hub-intel-edison-lessons/lesson2/az_iot_help_ubuntu.png)

## <a name="summary"></a><span data-ttu-id="09c4e-127">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="09c4e-127">Summary</span></span>
<span data-ttu-id="09c4e-128">Hello Azure parancssori felület telepítése.</span><span class="sxs-lookup"><span data-stu-id="09c4e-128">You've installed hello Azure CLI.</span></span> <span data-ttu-id="09c4e-129">A következő feladathoz toocreate a Azure IoT-központ és az eszköz identitása használt hello Azure parancssori felület.</span><span class="sxs-lookup"><span data-stu-id="09c4e-129">Your next task is toocreate your Azure IoT hub and device identity using hello Azure CLI.</span></span>

## <a name="next-steps"></a><span data-ttu-id="09c4e-130">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="09c4e-130">Next steps</span></span>
<span data-ttu-id="09c4e-131">[Az IoT hub létrehozni és regisztrálni az Intel Edison][create-your-iot-hub-and-register-intel-edison]</span><span class="sxs-lookup"><span data-stu-id="09c4e-131">[Create your IoT hub and register Intel Edison][create-your-iot-hub-and-register-intel-edison]</span></span>


<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[create-your-iot-hub-and-register-intel-edison]: iot-hub-intel-edison-kit-node-lesson2-prepare-azure-iot-hub.md
[windows]: iot-hub-intel-edison-kit-node-lesson2-get-azure-tools-win32.md
[ubuntu]: iot-hub-intel-edison-kit-node-lesson2-get-azure-tools-ubuntu.md
[macos]: iot-hub-intel-edison-kit-node-lesson2-get-azure-tools-mac.md
