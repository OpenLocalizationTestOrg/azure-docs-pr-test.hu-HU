---
title: "Az Azure IoT - lecke 2 Connect Raspberry pi (C): Azure-eszközök (Ubuntu) |} Microsoft Docs"
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
ms.openlocfilehash: be1506edf0e63190dbb85a3adb7897bd1cc84d38
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-azure-tools-ubuntu-1604"></a><span data-ttu-id="cc67f-104">Azure-eszközök (Ubuntu 16.04) beolvasása</span><span class="sxs-lookup"><span data-stu-id="cc67f-104">Get Azure tools (Ubuntu 16.04)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="cc67f-105">Windows 7 és újabb verziók</span><span class="sxs-lookup"><span data-stu-id="cc67f-105">Windows 7 and later</span></span>](iot-hub-raspberry-pi-kit-c-lesson2-get-azure-tools-win32.md)
> * [<span data-ttu-id="cc67f-106">Ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="cc67f-106">Ubuntu 16.04</span></span>](iot-hub-raspberry-pi-kit-c-lesson2-get-azure-tools-ubuntu.md)
> * [<span data-ttu-id="cc67f-107">macOS 10.10</span><span class="sxs-lookup"><span data-stu-id="cc67f-107">macOS 10.10</span></span>](iot-hub-raspberry-pi-kit-c-lesson2-get-azure-tools-mac.md)

## <a name="what-you-will-do"></a><span data-ttu-id="cc67f-108">Mit fog</span><span class="sxs-lookup"><span data-stu-id="cc67f-108">What you will do</span></span>
<span data-ttu-id="cc67f-109">Telepítse az Azure parancssori felület (CLI).</span><span class="sxs-lookup"><span data-stu-id="cc67f-109">Install the Azure command-line interface (Azure CLI).</span></span> <span data-ttu-id="cc67f-110">Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="cc67f-110">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="cc67f-111">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="cc67f-111">What you will learn</span></span>
<span data-ttu-id="cc67f-112">Ebből a cikkből megtudhatja:</span><span class="sxs-lookup"><span data-stu-id="cc67f-112">In this article, you will learn:</span></span>
* <span data-ttu-id="cc67f-113">Tudnivalók az Azure parancssori felület telepítése.</span><span class="sxs-lookup"><span data-stu-id="cc67f-113">How to install the Azure CLI.</span></span>
* <span data-ttu-id="cc67f-114">Hogyan lehet az Azure parancssori felület egy IoT részcsoport hozzáadni.</span><span class="sxs-lookup"><span data-stu-id="cc67f-114">How to add an IoT subgroup of the Azure CLI.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="cc67f-115">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="cc67f-115">What you need</span></span>
* <span data-ttu-id="cc67f-116">Egy Ubuntu számítógép az internethez.</span><span class="sxs-lookup"><span data-stu-id="cc67f-116">An Ubuntu computer with an Internet connection.</span></span>
* <span data-ttu-id="cc67f-117">Aktív Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="cc67f-117">An active Azure subscription.</span></span> <span data-ttu-id="cc67f-118">Ha nincs fiókja, létrehozhat egy [ingyenes próbafiók](http://azure.microsoft.com/pricing/free-trial/) csak néhány perc múlva.</span><span class="sxs-lookup"><span data-stu-id="cc67f-118">If you don't have an account, you can create a [free trial account](http://azure.microsoft.com/pricing/free-trial/) in just a few minutes.</span></span>

## <a name="install-the-azure-cli"></a><span data-ttu-id="cc67f-119">Telepítse az Azure CLI-t</span><span class="sxs-lookup"><span data-stu-id="cc67f-119">Install the Azure CLI</span></span>
<span data-ttu-id="cc67f-120">Az Azure parancssori felület olyan többplatformos parancssori környezetet biztosít az Azure-ra, így közvetlenül a parancssorból kiépítését működik, és kezelheti az erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="cc67f-120">The Azure CLI provides a multiplatform command-line experience for Azure, enabling you to work directly from your command line to provision and manage resources.</span></span>

<span data-ttu-id="cc67f-121">Az Azure CLI legújabb telepítéséhez kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="cc67f-121">To install the latest Azure CLI, follow these steps:</span></span>

1. <span data-ttu-id="cc67f-122">Futtassa a következő parancsokat egy terminálablakot.</span><span class="sxs-lookup"><span data-stu-id="cc67f-122">Run the following commands in a terminal window.</span></span> <span data-ttu-id="cc67f-123">Az Azure parancssori felület telepítése öt percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="cc67f-123">It might take five minutes to install the Azure CLI.</span></span>

   ```bash
   sudo apt-get update
   sudo apt-get install -y libssl-dev libffi-dev
   sudo apt-get install -y python-dev
   sudo apt-get install -y build-essential
   sudo apt-get install -y python-pip
   sudo pip install --upgrade azure-cli
   sudo pip install --upgrade azure-cli-iot
   ```
2. <span data-ttu-id="cc67f-124">Ellenőrizze a telepítést a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="cc67f-124">Verify the installation by running the following command:</span></span>

   ```bash
   az iot -h
   ```

<span data-ttu-id="cc67f-125">Ha a telepítés sikerült a következő kimenetet kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="cc67f-125">You should see the following output if the installation is successful.</span></span>

![Kimeneti sikerességét jelző](media/iot-hub-raspberry-pi-lessons/lesson2/az_iot_help_ubuntu.png)

## <a name="summary"></a><span data-ttu-id="cc67f-127">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="cc67f-127">Summary</span></span>
<span data-ttu-id="cc67f-128">Az Azure parancssori felület telepítése.</span><span class="sxs-lookup"><span data-stu-id="cc67f-128">You've installed the Azure CLI.</span></span> <span data-ttu-id="cc67f-129">A következő feladathoz, ha az Azure IoT hub- és eszközidentitások az Azure parancssori felület használatával.</span><span class="sxs-lookup"><span data-stu-id="cc67f-129">Your next task is to create your Azure IoT hub and device identity using the Azure CLI.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cc67f-130">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cc67f-130">Next steps</span></span>
[<span data-ttu-id="cc67f-131">Az IoT hub létrehozni és regisztrálni az málna Pi 3</span><span class="sxs-lookup"><span data-stu-id="cc67f-131">Create your IoT hub and register Raspberry Pi 3</span></span>](iot-hub-raspberry-pi-kit-c-lesson2-prepare-azure-iot-hub.md)

