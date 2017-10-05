---
title: "Az Azure IoT - lecke 2 Connect Intel Edison (C): Azure-eszközök (Ubuntu) |} Microsoft Docs"
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
ms.openlocfilehash: 897ab63af85a1f830ed49084ce7c3e74c84a4cc9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-azure-tools-ubuntu-1604"></a><span data-ttu-id="8328a-104">Azure-eszközök (Ubuntu 16.04) beolvasása</span><span class="sxs-lookup"><span data-stu-id="8328a-104">Get Azure tools (Ubuntu 16.04)</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="8328a-105">[Windows 7 és újabb verziók][windows]</span><span class="sxs-lookup"><span data-stu-id="8328a-105">[Windows 7 and later][windows]</span></span>
> * <span data-ttu-id="8328a-106">[Ubuntu 16.04][ubuntu]</span><span class="sxs-lookup"><span data-stu-id="8328a-106">[Ubuntu 16.04][ubuntu]</span></span>
> * <span data-ttu-id="8328a-107">[macOS 10.10][macos]</span><span class="sxs-lookup"><span data-stu-id="8328a-107">[macOS 10.10][macos]</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="8328a-108">Mit fog</span><span class="sxs-lookup"><span data-stu-id="8328a-108">What you will do</span></span>
<span data-ttu-id="8328a-109">Telepítse az Azure parancssori felület (CLI).</span><span class="sxs-lookup"><span data-stu-id="8328a-109">Install the Azure command-line interface (Azure CLI).</span></span> <span data-ttu-id="8328a-110">Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="8328a-110">If you have any problems, look for solutions on the [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="8328a-111">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="8328a-111">What you will learn</span></span>
<span data-ttu-id="8328a-112">Ebből a cikkből megtudhatja:</span><span class="sxs-lookup"><span data-stu-id="8328a-112">In this article, you will learn:</span></span>
* <span data-ttu-id="8328a-113">Tudnivalók az Azure parancssori felület telepítése.</span><span class="sxs-lookup"><span data-stu-id="8328a-113">How to install the Azure CLI.</span></span>
* <span data-ttu-id="8328a-114">Hogyan lehet az Azure parancssori felület egy IoT részcsoport hozzáadni.</span><span class="sxs-lookup"><span data-stu-id="8328a-114">How to add an IoT subgroup of the Azure CLI.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="8328a-115">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="8328a-115">What you need</span></span>
* <span data-ttu-id="8328a-116">Egy Ubuntu számítógép az internethez.</span><span class="sxs-lookup"><span data-stu-id="8328a-116">An Ubuntu computer with an Internet connection.</span></span>
* <span data-ttu-id="8328a-117">Aktív Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="8328a-117">An active Azure subscription.</span></span> <span data-ttu-id="8328a-118">Ha nincs fiókja, létrehozhat egy [ingyenes próbafiók](http://azure.microsoft.com/pricing/free-trial/) csak néhány perc múlva.</span><span class="sxs-lookup"><span data-stu-id="8328a-118">If you don't have an account, you can create a [free trial account](http://azure.microsoft.com/pricing/free-trial/) in just a few minutes.</span></span>

## <a name="install-the-azure-cli"></a><span data-ttu-id="8328a-119">Telepítse az Azure CLI-t</span><span class="sxs-lookup"><span data-stu-id="8328a-119">Install the Azure CLI</span></span>
<span data-ttu-id="8328a-120">Az Azure parancssori felület olyan többplatformos parancssori környezetet biztosít az Azure-ra, így közvetlenül a parancssorból kiépítését működik, és kezelheti az erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="8328a-120">The Azure CLI provides a multiplatform command-line experience for Azure, enabling you to work directly from your command line to provision and manage resources.</span></span>

<span data-ttu-id="8328a-121">Az Azure CLI legújabb telepítéséhez kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="8328a-121">To install the latest Azure CLI, follow these steps:</span></span>

1. <span data-ttu-id="8328a-122">Futtassa a következő parancsokat egy terminálablakot.</span><span class="sxs-lookup"><span data-stu-id="8328a-122">Run the following commands in a terminal window.</span></span> <span data-ttu-id="8328a-123">Az Azure parancssori felület telepítése öt percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="8328a-123">It might take five minutes to install the Azure CLI.</span></span>

   ```bash
   sudo apt-get update
   sudo apt-get install -y libssl-dev libffi-dev
   sudo apt-get install -y python-dev
   sudo apt-get install -y build-essential
   sudo apt-get install -y python-pip
   sudo pip install --upgrade azure-cli
   sudo pip install --upgrade azure-cli-iot
   ```
2. <span data-ttu-id="8328a-124">Ellenőrizze a telepítést a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="8328a-124">Verify the installation by running the following command:</span></span>

   ```bash
   az iot -h
   ```

<span data-ttu-id="8328a-125">Ha a telepítés sikerült a következő kimenetet kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="8328a-125">You should see the following output if the installation is successful.</span></span>

![Kimeneti sikerességét jelző](media/iot-hub-intel-edison-lessons/lesson2/az_iot_help_ubuntu.png)

## <a name="summary"></a><span data-ttu-id="8328a-127">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="8328a-127">Summary</span></span>
<span data-ttu-id="8328a-128">Az Azure parancssori felület telepítése.</span><span class="sxs-lookup"><span data-stu-id="8328a-128">You've installed the Azure CLI.</span></span> <span data-ttu-id="8328a-129">A következő feladathoz, ha az Azure IoT hub- és eszközidentitások az Azure parancssori felület használatával.</span><span class="sxs-lookup"><span data-stu-id="8328a-129">Your next task is to create your Azure IoT hub and device identity using the Azure CLI.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8328a-130">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8328a-130">Next steps</span></span>
<span data-ttu-id="8328a-131">[Az IoT hub létrehozni és regisztrálni az Intel Edison][create-your-iot-hub-and-register-intel-edison]</span><span class="sxs-lookup"><span data-stu-id="8328a-131">[Create your IoT hub and register Intel Edison][create-your-iot-hub-and-register-intel-edison]</span></span>


<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md
[create-your-iot-hub-and-register-intel-edison]: iot-hub-intel-edison-kit-c-lesson2-prepare-azure-iot-hub.md
[windows]: iot-hub-intel-edison-kit-c-lesson2-get-azure-tools-win32.md
[ubuntu]: iot-hub-intel-edison-kit-c-lesson2-get-azure-tools-ubuntu.md
[macos]: iot-hub-intel-edison-kit-c-lesson2-get-azure-tools-mac.md
