---
title: "Az Azure IoT - lecke 2 Connect Raspberry pi (C): (Windows) Azure-eszközök |} Microsoft Docs"
description: "Telepítse a Python és az Azure parancssori felület (CLI) Windows 7 és újabb verziók."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "az IOT felhőalapú szolgáltatás, az azure parancssori felület"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started
ms.assetid: a3c083b5-0d76-46af-bc77-2ad7d8aadc1e
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: aa96000cb676c088a90f2b3d45c159913185a2e1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-azure-tools-windows-7-and-later"></a><span data-ttu-id="40ccc-104">Első Azure-eszközök (Windows 7 és újabb verziók)</span><span class="sxs-lookup"><span data-stu-id="40ccc-104">Get Azure tools (Windows 7 and later)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="40ccc-105">Windows 7 és újabb verziók</span><span class="sxs-lookup"><span data-stu-id="40ccc-105">Windows 7 and later</span></span>](iot-hub-raspberry-pi-kit-c-lesson2-get-azure-tools-win32.md)
> * [<span data-ttu-id="40ccc-106">Ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="40ccc-106">Ubuntu 16.04</span></span>](iot-hub-raspberry-pi-kit-c-lesson2-get-azure-tools-ubuntu.md)
> * [<span data-ttu-id="40ccc-107">macOS 10.10</span><span class="sxs-lookup"><span data-stu-id="40ccc-107">macOS 10.10</span></span>](iot-hub-raspberry-pi-kit-c-lesson2-get-azure-tools-mac.md)

## <a name="what-you-will-do"></a><span data-ttu-id="40ccc-108">Mit fog</span><span class="sxs-lookup"><span data-stu-id="40ccc-108">What you will do</span></span>
<span data-ttu-id="40ccc-109">Telepítse a Python és az Azure parancssori felület (CLI).</span><span class="sxs-lookup"><span data-stu-id="40ccc-109">Install Python and the Azure command-line interface (Azure CLI).</span></span> <span data-ttu-id="40ccc-110">Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="40ccc-110">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="40ccc-111">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="40ccc-111">What you will learn</span></span>
<span data-ttu-id="40ccc-112">Ebből a cikkből megtudhatja:</span><span class="sxs-lookup"><span data-stu-id="40ccc-112">In this article, you will learn:</span></span>
* <span data-ttu-id="40ccc-113">Hogyan kell telepíteni a Python.</span><span class="sxs-lookup"><span data-stu-id="40ccc-113">How to install Python.</span></span>
* <span data-ttu-id="40ccc-114">Tudnivalók az Azure parancssori felület telepítése.</span><span class="sxs-lookup"><span data-stu-id="40ccc-114">How to install the Azure CLI.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="40ccc-115">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="40ccc-115">What you need</span></span>
* <span data-ttu-id="40ccc-116">Egy internetkapcsolattal rendelkező Windows-számítógép.</span><span class="sxs-lookup"><span data-stu-id="40ccc-116">A Windows computer with an Internet connection.</span></span>
* <span data-ttu-id="40ccc-117">Aktív Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="40ccc-117">An active Azure subscription.</span></span> <span data-ttu-id="40ccc-118">Ha az Azure-fiók nem rendelkezik, hozzon létre egy [ingyenes Azure próba-fiókot](http://azure.microsoft.com/pricing/free-trial/) csak néhány perc múlva.</span><span class="sxs-lookup"><span data-stu-id="40ccc-118">If you don't have an Azure account, create a [free Azure trial account](http://azure.microsoft.com/pricing/free-trial/) in just a few minutes.</span></span>

## <a name="install-python"></a><span data-ttu-id="40ccc-119">Python telepítése</span><span class="sxs-lookup"><span data-stu-id="40ccc-119">Install Python</span></span>
<span data-ttu-id="40ccc-120">[Telepítse a Python](https://www.python.org/downloads/) a Windows-számítógépen.</span><span class="sxs-lookup"><span data-stu-id="40ccc-120">[Install Python](https://www.python.org/downloads/) on your Windows computer.</span></span> <span data-ttu-id="40ccc-121">Python 2.7, 3.4-es vagy 3.5-ös verzióját is telepítheti.</span><span class="sxs-lookup"><span data-stu-id="40ccc-121">You can install Python 2.7, 3.4 or 3.5.</span></span> <span data-ttu-id="40ccc-122">Ez az oktatóanyag a Python 2.7 alapul.</span><span class="sxs-lookup"><span data-stu-id="40ccc-122">This tutorial is based on Python 2.7.</span></span> <span data-ttu-id="40ccc-123">Ha már telepítette a Python, folytassa a következő szakasszal, és az Azure parancssori felület telepítése.</span><span class="sxs-lookup"><span data-stu-id="40ccc-123">If you've already installed Python, go to the next section and install the Azure CLI.</span></span>

<span data-ttu-id="40ccc-124">Ahol python.exe és pip.exe vannak telepítve a rendszer a mappák elérési útját adja hozzá kell `PATH` környezeti változó.</span><span class="sxs-lookup"><span data-stu-id="40ccc-124">You also need to add the path of the folders where python.exe and pip.exe are installed to the system `PATH` environment variable.</span></span> <span data-ttu-id="40ccc-125">Alapértelmezés szerint a python.exe telepítve van a `C:\Python27` és pip.exe telepítve van-e `C:\Python27\Scripts`.</span><span class="sxs-lookup"><span data-stu-id="40ccc-125">By default, python.exe is installed in `C:\Python27` and pip.exe is installed in `C:\Python27\Scripts`.</span></span>

## <a name="install-the-azure-cli"></a><span data-ttu-id="40ccc-126">Telepítse az Azure CLI-t</span><span class="sxs-lookup"><span data-stu-id="40ccc-126">Install the Azure CLI</span></span>
<span data-ttu-id="40ccc-127">Az Azure parancssori felület olyan többplatformos parancssori környezetet biztosít az Azure.</span><span class="sxs-lookup"><span data-stu-id="40ccc-127">The Azure CLI provides a multiplatform command-line experience for Azure.</span></span> <span data-ttu-id="40ccc-128">Közvetlenül a parancssorból kiépítését működik, és kezelheti az erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="40ccc-128">You work directly from your command line to provision and manage resources.</span></span>

<span data-ttu-id="40ccc-129">Az Azure parancssori felület telepítéséhez kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="40ccc-129">To install the Azure CLI, follow these steps:</span></span>

1. <span data-ttu-id="40ccc-130">Nyisson meg egy parancssori ablakot rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="40ccc-130">Open a Command Prompt window as an administrator.</span></span>
2. <span data-ttu-id="40ccc-131">Futtassa az alábbi parancsot:</span><span class="sxs-lookup"><span data-stu-id="40ccc-131">Run the following commands:</span></span>

   ```bash
   pip install --upgrade azure-cli
   pip install --upgrade azure-cli-iot
   ```
3. <span data-ttu-id="40ccc-132">Ellenőrizze a telepítést a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="40ccc-132">Verify the installation by running the following command:</span></span>

   ```bash
   az iot -h
   ```

<span data-ttu-id="40ccc-133">Ha a telepítés sikeres megjelenik a következő kimenetet.</span><span class="sxs-lookup"><span data-stu-id="40ccc-133">You see the following output if the installation is successful.</span></span>

![Kimeneti sikerességét jelző](media/iot-hub-raspberry-pi-lessons/lesson2/az_iot_help_win.png)

## <a name="summary"></a><span data-ttu-id="40ccc-135">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="40ccc-135">Summary</span></span>
<span data-ttu-id="40ccc-136">Az Azure parancssori felület telepítése.</span><span class="sxs-lookup"><span data-stu-id="40ccc-136">You've installed the Azure CLI.</span></span> <span data-ttu-id="40ccc-137">A következő feladathoz az Azure IoT hub- és eszközidentitások létrehozása az Azure parancssori felület használatával.</span><span class="sxs-lookup"><span data-stu-id="40ccc-137">Your next task to create your Azure IoT hub and device identity by using the Azure CLI.</span></span>

## <a name="next-steps"></a><span data-ttu-id="40ccc-138">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="40ccc-138">Next steps</span></span>
[<span data-ttu-id="40ccc-139">Az IoT hub létrehozni és regisztrálni az málna Pi 3</span><span class="sxs-lookup"><span data-stu-id="40ccc-139">Create your IoT hub and register Raspberry Pi 3</span></span>](iot-hub-raspberry-pi-kit-c-lesson2-prepare-azure-iot-hub.md)

