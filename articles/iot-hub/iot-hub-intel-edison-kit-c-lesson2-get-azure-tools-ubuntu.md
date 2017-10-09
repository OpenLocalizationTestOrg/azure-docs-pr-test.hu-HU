---
title: "Connect Intel Edison (C) tooAzure IoT - lecke 2: Azure-eszközök (Ubuntu) |} Microsoft Docs"
description: "Ubuntu Python és az Azure parancssori felület (CLI) telepíthető."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "az Azure cli, iot felhőalapú szolgáltatás, arduino felhő"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-c-get-started
ms.assetid: 2463cb8e-5758-4d72-af98-62520d41f2f7
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: a7691c13d43aa6dfff24adf2b470728d5266713e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-azure-tools-ubuntu-1604"></a><span data-ttu-id="ebe72-104">Azure-eszközök (Ubuntu 16.04) beolvasása</span><span class="sxs-lookup"><span data-stu-id="ebe72-104">Get Azure tools (Ubuntu 16.04)</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="ebe72-105">[Windows 7 és újabb verziók][windows]</span><span class="sxs-lookup"><span data-stu-id="ebe72-105">[Windows 7 and later][windows]</span></span>
> * <span data-ttu-id="ebe72-106">[Ubuntu 16.04][ubuntu]</span><span class="sxs-lookup"><span data-stu-id="ebe72-106">[Ubuntu 16.04][ubuntu]</span></span>
> * <span data-ttu-id="ebe72-107">[macOS 10.10][macos]</span><span class="sxs-lookup"><span data-stu-id="ebe72-107">[macOS 10.10][macos]</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="ebe72-108">Mit fog</span><span class="sxs-lookup"><span data-stu-id="ebe72-108">What you will do</span></span>
<span data-ttu-id="ebe72-109">Hello Azure parancssori felület (CLI) telepítése.</span><span class="sxs-lookup"><span data-stu-id="ebe72-109">Install hello Azure command-line interface (Azure CLI).</span></span> <span data-ttu-id="ebe72-110">Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="ebe72-110">If you have any problems, look for solutions on hello [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="ebe72-111">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="ebe72-111">What you will learn</span></span>
<span data-ttu-id="ebe72-112">Ebből a cikkből megtudhatja:</span><span class="sxs-lookup"><span data-stu-id="ebe72-112">In this article, you will learn:</span></span>
* <span data-ttu-id="ebe72-113">Hogyan tooinstall hello Azure parancssori felület.</span><span class="sxs-lookup"><span data-stu-id="ebe72-113">How tooinstall hello Azure CLI.</span></span>
* <span data-ttu-id="ebe72-114">Hogyan tooadd hello Azure parancssori felület egy IoT ablaktábla.</span><span class="sxs-lookup"><span data-stu-id="ebe72-114">How tooadd an IoT subgroup of hello Azure CLI.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="ebe72-115">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="ebe72-115">What you need</span></span>
* <span data-ttu-id="ebe72-116">Egy Ubuntu számítógép az internethez.</span><span class="sxs-lookup"><span data-stu-id="ebe72-116">An Ubuntu computer with an Internet connection.</span></span>
* <span data-ttu-id="ebe72-117">Aktív Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="ebe72-117">An active Azure subscription.</span></span> <span data-ttu-id="ebe72-118">Ha nincs fiókja, létrehozhat egy [ingyenes próbafiók](http://azure.microsoft.com/pricing/free-trial/) csak néhány perc múlva.</span><span class="sxs-lookup"><span data-stu-id="ebe72-118">If you don't have an account, you can create a [free trial account](http://azure.microsoft.com/pricing/free-trial/) in just a few minutes.</span></span>

## <a name="install-hello-azure-cli"></a><span data-ttu-id="ebe72-119">Hello Azure parancssori felület telepítése</span><span class="sxs-lookup"><span data-stu-id="ebe72-119">Install hello Azure CLI</span></span>
<span data-ttu-id="ebe72-120">hello Azure CLI olyan többplatformos parancssori környezetet biztosít az Azure, amely lehetővé teszi, a parancssor tooprovision közvetlenül a toowork, és kezelheti az erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="ebe72-120">hello Azure CLI provides a multiplatform command-line experience for Azure, enabling you toowork directly from your command line tooprovision and manage resources.</span></span>

<span data-ttu-id="ebe72-121">tooinstall hello Azure CLI legújabb, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="ebe72-121">tooinstall hello latest Azure CLI, follow these steps:</span></span>

1. <span data-ttu-id="ebe72-122">Futtassa a következő parancsokat egy terminálablakot hello.</span><span class="sxs-lookup"><span data-stu-id="ebe72-122">Run hello following commands in a terminal window.</span></span> <span data-ttu-id="ebe72-123">Öt perc tooinstall hello Azure CLI vehet igénybe.</span><span class="sxs-lookup"><span data-stu-id="ebe72-123">It might take five minutes tooinstall hello Azure CLI.</span></span>

   ```bash
   sudo apt-get update
   sudo apt-get install -y libssl-dev libffi-dev
   sudo apt-get install -y python-dev
   sudo apt-get install -y build-essential
   sudo apt-get install -y python-pip
   sudo pip install --upgrade azure-cli
   sudo pip install --upgrade azure-cli-iot
   ```
2. <span data-ttu-id="ebe72-124">Ellenőrizze a hello telepítést hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="ebe72-124">Verify hello installation by running hello following command:</span></span>

   ```bash
   az iot -h
   ```

<span data-ttu-id="ebe72-125">Meg kell jelennie a hello parancskimenet sikeres hello telepítés esetén.</span><span class="sxs-lookup"><span data-stu-id="ebe72-125">You should see hello following output if hello installation is successful.</span></span>

![Kimeneti sikerességét jelző](media/iot-hub-intel-edison-lessons/lesson2/az_iot_help_ubuntu.png)

## <a name="summary"></a><span data-ttu-id="ebe72-127">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="ebe72-127">Summary</span></span>
<span data-ttu-id="ebe72-128">Hello Azure parancssori felület telepítése.</span><span class="sxs-lookup"><span data-stu-id="ebe72-128">You've installed hello Azure CLI.</span></span> <span data-ttu-id="ebe72-129">A következő feladathoz toocreate a Azure IoT-központ és az eszköz identitása használt hello Azure parancssori felület.</span><span class="sxs-lookup"><span data-stu-id="ebe72-129">Your next task is toocreate your Azure IoT hub and device identity using hello Azure CLI.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ebe72-130">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ebe72-130">Next steps</span></span>
<span data-ttu-id="ebe72-131">[Az IoT hub létrehozni és regisztrálni az Intel Edison][create-your-iot-hub-and-register-intel-edison]</span><span class="sxs-lookup"><span data-stu-id="ebe72-131">[Create your IoT hub and register Intel Edison][create-your-iot-hub-and-register-intel-edison]</span></span>


<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md
[create-your-iot-hub-and-register-intel-edison]: iot-hub-intel-edison-kit-c-lesson2-prepare-azure-iot-hub.md
[windows]: iot-hub-intel-edison-kit-c-lesson2-get-azure-tools-win32.md
[ubuntu]: iot-hub-intel-edison-kit-c-lesson2-get-azure-tools-ubuntu.md
[macos]: iot-hub-intel-edison-kit-c-lesson2-get-azure-tools-mac.md
