---
title: "Csatlakozás málna Pi (csomópont) tooAzure IoT - lecke 2: Get tools (Windows) |} Microsoft Docs"
description: "Telepítse a Python és hello Azure parancssori felület (CLI) Windows 7 és újabb verziók."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "az IOT felhőalapú szolgáltatás, az azure parancssori felület"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: acfa13e3-6d2c-4e68-9a77-1cbc2cf3f9c1
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 25b50214322137ea32861fd1131c474e2fc7ebb7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-azure-tools-windows-7-and-later"></a><span data-ttu-id="f2997-104">Első Azure-eszközök (Windows 7 és újabb verziók)</span><span class="sxs-lookup"><span data-stu-id="f2997-104">Get Azure tools (Windows 7 and later)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="f2997-105">Windows 7 és újabb verziók</span><span class="sxs-lookup"><span data-stu-id="f2997-105">Windows 7 and later</span></span>](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-win32.md)
> * [<span data-ttu-id="f2997-106">Ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="f2997-106">Ubuntu 16.04</span></span>](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-ubuntu.md)
> * [<span data-ttu-id="f2997-107">macOS 10.10</span><span class="sxs-lookup"><span data-stu-id="f2997-107">macOS 10.10</span></span>](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-mac.md)

## <a name="what-you-will-do"></a><span data-ttu-id="f2997-108">Mit fog</span><span class="sxs-lookup"><span data-stu-id="f2997-108">What you will do</span></span>
<span data-ttu-id="f2997-109">Telepítse a Python és hello Azure parancssori felület (CLI).</span><span class="sxs-lookup"><span data-stu-id="f2997-109">Install Python and hello Azure command-line interface (Azure CLI).</span></span> <span data-ttu-id="f2997-110">Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="f2997-110">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="f2997-111">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="f2997-111">What you will learn</span></span>
<span data-ttu-id="f2997-112">Ebből a cikkből megtudhatja:</span><span class="sxs-lookup"><span data-stu-id="f2997-112">In this article, you will learn:</span></span>
* <span data-ttu-id="f2997-113">Hogyan tooinstall Python.</span><span class="sxs-lookup"><span data-stu-id="f2997-113">How tooinstall Python.</span></span>
* <span data-ttu-id="f2997-114">Hogyan tooinstall hello Azure parancssori felület.</span><span class="sxs-lookup"><span data-stu-id="f2997-114">How tooinstall hello Azure CLI.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="f2997-115">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="f2997-115">What you need</span></span>
* <span data-ttu-id="f2997-116">Egy internetkapcsolattal rendelkező Windows-számítógép.</span><span class="sxs-lookup"><span data-stu-id="f2997-116">A Windows computer with an Internet connection.</span></span>
* <span data-ttu-id="f2997-117">Aktív Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="f2997-117">An active Azure subscription.</span></span> <span data-ttu-id="f2997-118">Ha az Azure-fiók nem rendelkezik, hozzon létre egy [ingyenes Azure próba-fiókot](http://azure.microsoft.com/pricing/free-trial/) csak néhány perc múlva.</span><span class="sxs-lookup"><span data-stu-id="f2997-118">If you don't have an Azure account, create a [free Azure trial account](http://azure.microsoft.com/pricing/free-trial/) in just a few minutes.</span></span>

## <a name="install-python"></a><span data-ttu-id="f2997-119">Python telepítése</span><span class="sxs-lookup"><span data-stu-id="f2997-119">Install Python</span></span>
<span data-ttu-id="f2997-120">[Telepítse a Python](https://www.python.org/downloads/) a Windows-számítógépen.</span><span class="sxs-lookup"><span data-stu-id="f2997-120">[Install Python](https://www.python.org/downloads/) on your Windows computer.</span></span> <span data-ttu-id="f2997-121">Python 2.7, 3.4-es vagy 3.5-ös verzióját is telepítheti.</span><span class="sxs-lookup"><span data-stu-id="f2997-121">You can install Python 2.7, 3.4 or 3.5.</span></span> <span data-ttu-id="f2997-122">Ez az oktatóanyag a Python 2.7 alapul.</span><span class="sxs-lookup"><span data-stu-id="f2997-122">This tutorial is based on Python 2.7.</span></span> <span data-ttu-id="f2997-123">Ha már telepítette a Python, nyissa meg a következő szakaszban toohello és hello Azure parancssori felület telepítése.</span><span class="sxs-lookup"><span data-stu-id="f2997-123">If you've already installed Python, go toohello next section and install hello Azure CLI.</span></span>

<span data-ttu-id="f2997-124">Szükség tooadd hello elérési útját, amelyben python.exe és pip.exe is telepített toohello rendszer hello mappák `PATH` környezeti változó.</span><span class="sxs-lookup"><span data-stu-id="f2997-124">You also need tooadd hello path of hello folders where python.exe and pip.exe are installed toohello system `PATH` environment variable.</span></span> <span data-ttu-id="f2997-125">Alapértelmezés szerint a python.exe telepítve van a `C:\Python27` és pip.exe telepítve van-e `C:\Python27\Scripts`.</span><span class="sxs-lookup"><span data-stu-id="f2997-125">By default, python.exe is installed in `C:\Python27` and pip.exe is installed in `C:\Python27\Scripts`.</span></span>

## <a name="install-hello-azure-cli"></a><span data-ttu-id="f2997-126">Hello Azure parancssori felület telepítése</span><span class="sxs-lookup"><span data-stu-id="f2997-126">Install hello Azure CLI</span></span>
<span data-ttu-id="f2997-127">hello Azure CLI olyan többplatformos parancssori környezetet biztosít az Azure.</span><span class="sxs-lookup"><span data-stu-id="f2997-127">hello Azure CLI provides a multiplatform command-line experience for Azure.</span></span> <span data-ttu-id="f2997-128">Közvetlenül a parancssori tooprovision dolgozhassanak és kezelheti az erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="f2997-128">You work directly from your command-line tooprovision and manage resources.</span></span>

<span data-ttu-id="f2997-129">tooinstall hello Azure CLI használata esetén kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="f2997-129">tooinstall hello Azure CLI, follow these steps:</span></span>

1. <span data-ttu-id="f2997-130">Nyisson meg egy parancssori ablakot rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="f2997-130">Open a Command Prompt window as an administrator.</span></span>
2. <span data-ttu-id="f2997-131">Futtassa a következő parancsok hello:</span><span class="sxs-lookup"><span data-stu-id="f2997-131">Run hello following commands:</span></span>

   ```bash
   pip install --upgrade azure-cli
   pip install --upgrade azure-cli-iot
   ```
3. <span data-ttu-id="f2997-132">Ellenőrizze a hello telepítést hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="f2997-132">Verify hello installation by running hello following command:</span></span>

   ```bash
   az iot -h
   ```

<span data-ttu-id="f2997-133">Megjelenik a hello parancskimenet sikeres hello telepítés esetén.</span><span class="sxs-lookup"><span data-stu-id="f2997-133">You see hello following output if hello installation is successful.</span></span>

![Kimeneti sikerességét jelző](media/iot-hub-raspberry-pi-lessons/lesson2/az_iot_help_win.png)

## <a name="summary"></a><span data-ttu-id="f2997-135">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="f2997-135">Summary</span></span>
<span data-ttu-id="f2997-136">Hello Azure parancssori felület telepítése.</span><span class="sxs-lookup"><span data-stu-id="f2997-136">You've installed hello Azure CLI.</span></span> <span data-ttu-id="f2997-137">A Tovább gombra a feladatütemezés toocreate az Azure IoT hub- és eszközidentitások hello Azure parancssori felület használatával.</span><span class="sxs-lookup"><span data-stu-id="f2997-137">Your next task toocreate your Azure IoT hub and device identity by using hello Azure CLI.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f2997-138">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f2997-138">Next steps</span></span>
[<span data-ttu-id="f2997-139">Az IoT hub létrehozni és regisztrálni az málna Pi 3</span><span class="sxs-lookup"><span data-stu-id="f2997-139">Create your IoT hub and register Raspberry Pi 3</span></span>](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md)

