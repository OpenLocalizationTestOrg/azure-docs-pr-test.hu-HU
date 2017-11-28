---
title: "Csatlakozás Azure IoT - lecke 2 Arduino: eszköz regisztrálása |} Microsoft Docs"
description: "Hozzon létre egy erőforráscsoportot, Azure IoT hub létrehozása és az Azure IoT hub Adafruit lágyított M0 Wi-Fi regisztrálása az Azure parancssori felület használatával."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "eszköz, arduino felhő arduino csatlakozik a felhő, az azure iot hub, dolgot felhőalapú azure iot-központ az eszközök internetes létrehozása"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: 5edc690b-7a1d-4ebc-b011-ff27bfffe6e8
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: c5ad5e900671c7cedd3cdad2c2aa345315de223b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-your-iot-hub-and-register-your-adafruit-feather-m0-wifi-arduino-board"></a><span data-ttu-id="a8852-104">Az IoT hub létrehozása és regisztrálása a Adafruit lágyított M0 Wi-Fi Arduino board</span><span class="sxs-lookup"><span data-stu-id="a8852-104">Create your IoT hub and register your Adafruit Feather M0 WiFi Arduino board</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="a8852-105">Mit fog</span><span class="sxs-lookup"><span data-stu-id="a8852-105">What you will do</span></span>
* <span data-ttu-id="a8852-106">Hozzon létre egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="a8852-106">Create a resource group.</span></span>
* <span data-ttu-id="a8852-107">Az Azure IoT hub létrehozása az erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="a8852-107">Create your Azure IoT hub in the resource group.</span></span>
* <span data-ttu-id="a8852-108">A Arduino board az Azure parancssori felület (CLI) használatával adja hozzá az Azure IoT hub.</span><span class="sxs-lookup"><span data-stu-id="a8852-108">Add your Arduino board to the Azure IoT hub by using the Azure command-line interface (Azure CLI).</span></span>

<span data-ttu-id="a8852-109">Ha az Azure parancssori felület használatával adja hozzá a Arduino board az IoT hub, a szolgáltatás kulcsot hoz létre egy az a Arduino üzenőfalon, a szolgáltatás szolgáltatással való hitelesítésre.</span><span class="sxs-lookup"><span data-stu-id="a8852-109">When you use the Azure CLI to add your Arduino board to your IoT hub, the service generates a key for your Arduino board to authenticate with the service.</span></span> <span data-ttu-id="a8852-110">Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási][troubleshoot].</span><span class="sxs-lookup"><span data-stu-id="a8852-110">If you have any problems, look for solutions on the [troubleshooting page][troubleshoot].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="a8852-111">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="a8852-111">What you will learn</span></span>
<span data-ttu-id="a8852-112">Ebből a cikkből megtudhatja:</span><span class="sxs-lookup"><span data-stu-id="a8852-112">In this article, you will learn:</span></span>
* <span data-ttu-id="a8852-113">Hogyan lehet az Azure CLI segítségével létrehoz egy IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="a8852-113">How to use the Azure CLI to create an IoT hub.</span></span>
* <span data-ttu-id="a8852-114">Hogyan hozhat létre egy eszközidentitás a Arduino kártya az IoT hub.</span><span class="sxs-lookup"><span data-stu-id="a8852-114">How to create a device identity for your Arduino board in your IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="a8852-115">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="a8852-115">What you need</span></span>
* <span data-ttu-id="a8852-116">Az Azure-fiók</span><span class="sxs-lookup"><span data-stu-id="a8852-116">An Azure account</span></span>
* <span data-ttu-id="a8852-117">Az Azure parancssori felület telepítve rendelkező számítógép</span><span class="sxs-lookup"><span data-stu-id="a8852-117">A computer with the Azure CLI installed</span></span>

## <a name="create-your-iot-hub"></a><span data-ttu-id="a8852-118">Az IoT hub létrehozása</span><span class="sxs-lookup"><span data-stu-id="a8852-118">Create your IoT hub</span></span>
<span data-ttu-id="a8852-119">Azure IoT-központ segítségével csatlakozzon, figyeléséhez és több millió az IoT-eszközök kezelése.</span><span class="sxs-lookup"><span data-stu-id="a8852-119">Azure IoT Hub helps you connect, monitor, and manage millions of IoT assets.</span></span> <span data-ttu-id="a8852-120">Az IoT hub létrehozásához kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="a8852-120">To create your IoT hub, follow these steps:</span></span>

1. <span data-ttu-id="a8852-121">Jelentkezzen be az Azure-fiókjával a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="a8852-121">Sign in to your Azure account by running the following command:</span></span>

   ```bash
   az login
   ```

   <span data-ttu-id="a8852-122">Az elérhető előfizetések sikeres bejelentkezés után vannak felsorolva.</span><span class="sxs-lookup"><span data-stu-id="a8852-122">All your available subscriptions are listed after a successful sign-in.</span></span>

2. <span data-ttu-id="a8852-123">Állítsa be az alapértelmezett előfizetést szeretné használni a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="a8852-123">Set the default subscription that you want to use by running the following command:</span></span>

   ```bash
   az account set --subscription {subscription id or name}
   ```

   <span data-ttu-id="a8852-124">`subscription ID or name`Itt található: a kimenetét a `az login` vagy a `az account list` parancsot.</span><span class="sxs-lookup"><span data-stu-id="a8852-124">`subscription ID or name` can be found in the output of the `az login` or the `az account list` command.</span></span>

3. <span data-ttu-id="a8852-125">Regisztrálja a szolgáltatót a következő parancs futtatásával.</span><span class="sxs-lookup"><span data-stu-id="a8852-125">Register the provider by running the following command.</span></span> <span data-ttu-id="a8852-126">Erőforrás-szolgáltató a szolgáltatások, hogy biztosít erőforrásokat az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="a8852-126">Resource providers are services that provide resources for your application.</span></span> <span data-ttu-id="a8852-127">Az Azure-erőforrás, a szolgáltató által telepítése előtt regisztrálnia kell a szolgáltatót.</span><span class="sxs-lookup"><span data-stu-id="a8852-127">You must register the provider before you can deploy the Azure resource that the provider offers.</span></span>

   ```bash
   az provider register -n "Microsoft.Devices"
   ```
4. <span data-ttu-id="a8852-128">Hozzon létre egy erőforráscsoportot az USA nyugati régiója régióban iot-minta a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="a8852-128">Create a resource group named iot-sample in the West US region by running the following command:</span></span>

   ```bash
   az group create --name iot-sample --location westus
   ```

   <span data-ttu-id="a8852-129">`westus`az a hely, az erőforráscsoport létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="a8852-129">`westus` is the location you create your resource group.</span></span> <span data-ttu-id="a8852-130">Ha egy másik hely használni kívánt, futtathatja `az account list-locations -o table` megtekintéséhez az összes hely Azure támogatja.</span><span class="sxs-lookup"><span data-stu-id="a8852-130">If you want to use another location, you can run `az account list-locations -o table` to see all the locations Azure supports.</span></span>

5. <span data-ttu-id="a8852-131">Az iot-minta erőforráscsoportban IoT hub létrehozása a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="a8852-131">Create an IoT hub in the iot-sample resource group by running the following command:</span></span>

   ```bash
   az iot hub create --name {my hub name} --resource-group iot-sample
   ```

<span data-ttu-id="a8852-132">Alapértelmezés szerint a létrehoz egy IoT-központ az ingyenes tarifacsomag.</span><span class="sxs-lookup"><span data-stu-id="a8852-132">By default, the tool creates an IoT Hub in the Free pricing tier.</span></span> <span data-ttu-id="a8852-133">További információkért lásd: [Azure IoT Hub árképzési](https://azure.microsoft.com/pricing/details/iot-hub/).</span><span class="sxs-lookup"><span data-stu-id="a8852-133">For more infomation, see [Azure IoT Hub pricing](https://azure.microsoft.com/pricing/details/iot-hub/).</span></span>

> [!NOTE]
> <span data-ttu-id="a8852-134">Az IoT hub nevét globálisan egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="a8852-134">The name of your IoT hub must be globally unique.</span></span>
> <span data-ttu-id="a8852-135">Az Azure-előfizetéshez tartozó Azure IoT Hub kiadása csak egy F1 hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="a8852-135">You can create only one F1 edition of Azure IoT Hub under your Azure subscription.</span></span>

## <a name="register-your-arduino-board-in-your-iot-hub"></a><span data-ttu-id="a8852-136">Regisztrálja a Arduino board az IoT hub</span><span class="sxs-lookup"><span data-stu-id="a8852-136">Register your Arduino board in your IoT hub</span></span>
<span data-ttu-id="a8852-137">Minden eszköz, amely üzeneteket küld az IoT hub, valamint üzeneteket fogad az IoT hub regisztrálni kell egy egyedi azonosítót.</span><span class="sxs-lookup"><span data-stu-id="a8852-137">Each device that sends messages to your IoT hub and receives messages from your IoT hub must be registered with a unique ID.</span></span>

<span data-ttu-id="a8852-138">Az IoT hub a Arduino board regisztrálása fut a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="a8852-138">Register your Arduino board in your IoT hub by running following command:</span></span>

```bash
az iot device create --device-id mym0wifi --hub-name {my hub name}
```

## <a name="summary"></a><span data-ttu-id="a8852-139">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="a8852-139">Summary</span></span>
<span data-ttu-id="a8852-140">Az IoT-központ elkészítette és a Arduino board regisztrálva az IoT hub eszköz identitással.</span><span class="sxs-lookup"><span data-stu-id="a8852-140">You've created an IoT hub and registered your Arduino board with a device identity in your IoT hub.</span></span> <span data-ttu-id="a8852-141">Készen áll a Arduino board üzenetek küldése az IoT hub módjáról.</span><span class="sxs-lookup"><span data-stu-id="a8852-141">You're ready to learn how to send messages from your Arduino board to your IoT hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a8852-142">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a8852-142">Next steps</span></span>
<span data-ttu-id="a8852-143">[Hozzon létre egy Azure függvény alkalmazást és feldolgozni, és az IoT hub üzenetek tárolásához Azure Storage-fiók][process-and-store-iot-hub-messages].</span><span class="sxs-lookup"><span data-stu-id="a8852-143">[Create an Azure function app and an Azure Storage account to process and store IoT hub messages][process-and-store-iot-hub-messages].</span></span>


<!-- Images and links -->

[troubleshoot]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md
[process-and-store-iot-hub-messages]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson3-deploy-resource-manager-template.md