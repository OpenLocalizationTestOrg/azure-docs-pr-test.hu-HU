---
title: "Az Azure IoT - lecke 2 Connect Intel Edison (C): (Windows) Azure-eszközök |} Microsoft Docs"
description: "Telepítse a Python és az Azure parancssori felület (CLI) Windows 7 és újabb verziók."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "az Azure cli, iot felhőalapú szolgáltatás, arduino felhő"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-c-get-started
ms.assetid: 1035760e-cdd1-4d99-8003-067e98b29762
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 90ef113ae84ccc8cb3cbdbe8c245e138320a93c6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-azure-tools-windows-7-and-later"></a><span data-ttu-id="3bd98-104">Első Azure-eszközök (Windows 7 és újabb verziók)</span><span class="sxs-lookup"><span data-stu-id="3bd98-104">Get Azure tools (Windows 7 and later)</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="3bd98-105">[Windows 7 és újabb verziók][windows]</span><span class="sxs-lookup"><span data-stu-id="3bd98-105">[Windows 7 and later][windows]</span></span>
> * <span data-ttu-id="3bd98-106">[Ubuntu 16.04][ubuntu]</span><span class="sxs-lookup"><span data-stu-id="3bd98-106">[Ubuntu 16.04][ubuntu]</span></span>
> * <span data-ttu-id="3bd98-107">[macOS 10.10][macos]</span><span class="sxs-lookup"><span data-stu-id="3bd98-107">[macOS 10.10][macos]</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="3bd98-108">Mit fog</span><span class="sxs-lookup"><span data-stu-id="3bd98-108">What you will do</span></span>
<span data-ttu-id="3bd98-109">Telepítse a Python és az Azure parancssori felület (CLI).</span><span class="sxs-lookup"><span data-stu-id="3bd98-109">Install Python and the Azure command-line interface (Azure CLI).</span></span> <span data-ttu-id="3bd98-110">Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="3bd98-110">If you have any problems, look for solutions on the [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="3bd98-111">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="3bd98-111">What you will learn</span></span>
<span data-ttu-id="3bd98-112">Ebből a cikkből megtudhatja:</span><span class="sxs-lookup"><span data-stu-id="3bd98-112">In this article, you will learn:</span></span>
* <span data-ttu-id="3bd98-113">Hogyan kell telepíteni a Python.</span><span class="sxs-lookup"><span data-stu-id="3bd98-113">How to install Python.</span></span>
* <span data-ttu-id="3bd98-114">Tudnivalók az Azure parancssori felület telepítése.</span><span class="sxs-lookup"><span data-stu-id="3bd98-114">How to install the Azure CLI.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="3bd98-115">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="3bd98-115">What you need</span></span>
* <span data-ttu-id="3bd98-116">Egy internetkapcsolattal rendelkező Windows-számítógép.</span><span class="sxs-lookup"><span data-stu-id="3bd98-116">A Windows computer with an Internet connection.</span></span>
* <span data-ttu-id="3bd98-117">Aktív Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="3bd98-117">An active Azure subscription.</span></span> <span data-ttu-id="3bd98-118">Ha az Azure-fiók nem rendelkezik, hozzon létre egy [ingyenes Azure próba-fiókot](http://azure.microsoft.com/pricing/free-trial/) csak néhány perc múlva.</span><span class="sxs-lookup"><span data-stu-id="3bd98-118">If you don't have an Azure account, create a [free Azure trial account](http://azure.microsoft.com/pricing/free-trial/) in just a few minutes.</span></span>

## <a name="install-python"></a><span data-ttu-id="3bd98-119">Python telepítése</span><span class="sxs-lookup"><span data-stu-id="3bd98-119">Install Python</span></span>
<span data-ttu-id="3bd98-120">[Telepítse a Python](https://www.python.org/downloads/) a Windows-számítógépen.</span><span class="sxs-lookup"><span data-stu-id="3bd98-120">[Install Python](https://www.python.org/downloads/) on your Windows computer.</span></span> <span data-ttu-id="3bd98-121">Python 2.7, 3.4-es vagy 3.5-ös verzióját is telepítheti.</span><span class="sxs-lookup"><span data-stu-id="3bd98-121">You can install Python 2.7, 3.4 or 3.5.</span></span> <span data-ttu-id="3bd98-122">Ez az oktatóanyag a Python 2.7 alapul.</span><span class="sxs-lookup"><span data-stu-id="3bd98-122">This tutorial is based on Python 2.7.</span></span> <span data-ttu-id="3bd98-123">Ha már telepítette a Python, folytassa a következő szakasszal, és az Azure parancssori felület telepítése.</span><span class="sxs-lookup"><span data-stu-id="3bd98-123">If you've already installed Python, go to the next section and install the Azure CLI.</span></span>

<span data-ttu-id="3bd98-124">Ahol python.exe és pip.exe vannak telepítve a rendszer a mappák elérési útját adja hozzá kell `PATH` környezeti változó.</span><span class="sxs-lookup"><span data-stu-id="3bd98-124">You also need to add the path of the folders where python.exe and pip.exe are installed to the system `PATH` environment variable.</span></span> <span data-ttu-id="3bd98-125">Alapértelmezés szerint a python.exe telepítve van a `C:\Python27` és pip.exe telepítve van-e `C:\Python27\Scripts`.</span><span class="sxs-lookup"><span data-stu-id="3bd98-125">By default, python.exe is installed in `C:\Python27` and pip.exe is installed in `C:\Python27\Scripts`.</span></span>

## <a name="install-the-azure-cli"></a><span data-ttu-id="3bd98-126">Telepítse az Azure CLI-t</span><span class="sxs-lookup"><span data-stu-id="3bd98-126">Install the Azure CLI</span></span>
<span data-ttu-id="3bd98-127">Az Azure parancssori felület olyan többplatformos parancssori környezetet biztosít az Azure.</span><span class="sxs-lookup"><span data-stu-id="3bd98-127">The Azure CLI provides a multiplatform command-line experience for Azure.</span></span> <span data-ttu-id="3bd98-128">Közvetlenül a parancssorból kiépítését működik, és kezelheti az erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="3bd98-128">You work directly from your command line to provision and manage resources.</span></span>

<span data-ttu-id="3bd98-129">Az Azure parancssori felület telepítéséhez kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="3bd98-129">To install the Azure CLI, follow these steps:</span></span>

1. <span data-ttu-id="3bd98-130">Nyisson meg egy parancssori ablakot rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="3bd98-130">Open a Command Prompt window as an administrator.</span></span>
2. <span data-ttu-id="3bd98-131">Futtassa az alábbi parancsot:</span><span class="sxs-lookup"><span data-stu-id="3bd98-131">Run the following commands:</span></span>

   ```cmd
   pip install --upgrade azure-cli
   pip install --upgrade azure-cli-iot
   ```
3. <span data-ttu-id="3bd98-132">Ellenőrizze a telepítést a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="3bd98-132">Verify the installation by running the following command:</span></span>

   ```cmd
   az iot -h
   ```

<span data-ttu-id="3bd98-133">Ha a telepítés sikeres megjelenik a következő kimenetet.</span><span class="sxs-lookup"><span data-stu-id="3bd98-133">You see the following output if the installation is successful.</span></span>

![Kimeneti sikerességét jelző](media/iot-hub-intel-edison-lessons/lesson2/az_iot_help_win.png)

## <a name="summary"></a><span data-ttu-id="3bd98-135">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="3bd98-135">Summary</span></span>
<span data-ttu-id="3bd98-136">Az Azure parancssori felület telepítése.</span><span class="sxs-lookup"><span data-stu-id="3bd98-136">You've installed the Azure CLI.</span></span> <span data-ttu-id="3bd98-137">A következő feladathoz az Azure IoT hub- és eszközidentitások létrehozása az Azure parancssori felület használatával.</span><span class="sxs-lookup"><span data-stu-id="3bd98-137">Your next task to create your Azure IoT hub and device identity by using the Azure CLI.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3bd98-138">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3bd98-138">Next steps</span></span>
<span data-ttu-id="3bd98-139">[Az IoT hub létrehozni és regisztrálni az Intel Edison][create-your-iot-hub-and-register-intel-edison]</span><span class="sxs-lookup"><span data-stu-id="3bd98-139">[Create your IoT hub and register Intel Edison][create-your-iot-hub-and-register-intel-edison]</span></span>
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md
[create-your-iot-hub-and-register-intel-edison]: iot-hub-intel-edison-kit-c-lesson2-prepare-azure-iot-hub.md
[windows]: iot-hub-intel-edison-kit-c-lesson2-get-azure-tools-win32.md
[ubuntu]: iot-hub-intel-edison-kit-c-lesson2-get-azure-tools-ubuntu.md
[macos]: iot-hub-intel-edison-kit-c-lesson2-get-azure-tools-mac.md
