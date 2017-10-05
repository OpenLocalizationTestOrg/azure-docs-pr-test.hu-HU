---
title: "Az Azure IoT - lecke 2 Connect Raspberry pi (C): eszköz regisztrálása |} Microsoft Docs"
description: "Hozzon létre egy erőforráscsoportot, Azure IoT hub létrehozása és az Azure IoT hub Pi regisztrálása az Azure parancssori felület használatával."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Raspberry pi felhő pi felhő csatlakozás"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started
ms.assetid: 4bcfb071-b3ae-48cc-8ea5-7e7434732287
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: d7bfd8f6ae8d15dfe09f06a40a4ab415ff2e0a7c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-your-iot-hub-and-register-raspberry-pi-3"></a><span data-ttu-id="c1d74-104">Az IoT hub létrehozni és regisztrálni az málna Pi 3</span><span class="sxs-lookup"><span data-stu-id="c1d74-104">Create your IoT hub and register Raspberry Pi 3</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="c1d74-105">Mit fog</span><span class="sxs-lookup"><span data-stu-id="c1d74-105">What you will do</span></span>
* <span data-ttu-id="c1d74-106">Hozzon létre egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="c1d74-106">Create a resource group.</span></span>
* <span data-ttu-id="c1d74-107">Az Azure IoT hub létrehozása az erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="c1d74-107">Create your Azure IoT hub in the resource group.</span></span>
* <span data-ttu-id="c1d74-108">Az Azure IoT hub málna Pi 3 hozzáadása az Azure parancssori felület (CLI) használatával.</span><span class="sxs-lookup"><span data-stu-id="c1d74-108">Add Raspberry Pi 3 to the Azure IoT hub by using the Azure command-line interface (Azure CLI).</span></span>

<span data-ttu-id="c1d74-109">Ha az Azure parancssori felület használatával Pi hozzáadása az IoT hub, a szolgáltatás kulcsot hoz létre egy az Pi a service szolgáltatással való hitelesítésre.</span><span class="sxs-lookup"><span data-stu-id="c1d74-109">When you use the Azure CLI to add Pi to your IoT hub, the service generates a key for Pi to authenticate with the service.</span></span> <span data-ttu-id="c1d74-110">Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="c1d74-110">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="c1d74-111">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="c1d74-111">What you will learn</span></span>
<span data-ttu-id="c1d74-112">Ebből a cikkből megtudhatja:</span><span class="sxs-lookup"><span data-stu-id="c1d74-112">In this article, you will learn:</span></span>
* <span data-ttu-id="c1d74-113">Hogyan lehet az Azure CLI segítségével létrehoz egy IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="c1d74-113">How to use the Azure CLI to create an IoT hub.</span></span>
* <span data-ttu-id="c1d74-114">Hogyan hozhat létre egy eszközidentitás Pi az IoT hub.</span><span class="sxs-lookup"><span data-stu-id="c1d74-114">How to create a device identity for Pi in your IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="c1d74-115">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="c1d74-115">What you need</span></span>
* <span data-ttu-id="c1d74-116">Az Azure-fiók</span><span class="sxs-lookup"><span data-stu-id="c1d74-116">An Azure account</span></span>
* <span data-ttu-id="c1d74-117">A Mac vagy a Windows rendszerű számítógépeken a telepített Azure parancssori felülettel</span><span class="sxs-lookup"><span data-stu-id="c1d74-117">A Mac or a Windows computer with the Azure CLI installed</span></span>

## <a name="create-your-iot-hub"></a><span data-ttu-id="c1d74-118">Az IoT hub létrehozása</span><span class="sxs-lookup"><span data-stu-id="c1d74-118">Create your IoT hub</span></span>
<span data-ttu-id="c1d74-119">Azure IoT-központ segítségével csatlakozzon, figyeléséhez és több millió az IoT-eszközök kezelése.</span><span class="sxs-lookup"><span data-stu-id="c1d74-119">Azure IoT Hub helps you connect, monitor, and manage millions of IoT assets.</span></span> <span data-ttu-id="c1d74-120">Az IoT hub létrehozásához kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="c1d74-120">To create your IoT hub, follow these steps:</span></span>

1. <span data-ttu-id="c1d74-121">Jelentkezzen be az Azure-fiókjával a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="c1d74-121">Sign in to your Azure account by running the following command:</span></span>

   ```bash
   az login
   ```

   <span data-ttu-id="c1d74-122">Az elérhető előfizetések sikeres bejelentkezés után vannak felsorolva.</span><span class="sxs-lookup"><span data-stu-id="c1d74-122">All your available subscriptions are listed after a successful sign-in.</span></span>

2. <span data-ttu-id="c1d74-123">Állítsa be az alapértelmezett előfizetést szeretné használni a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="c1d74-123">Set the default subscription that you want to use by running the following command:</span></span>

   ```bash
   az account set --subscription {subscription id or name}
   ```

   <span data-ttu-id="c1d74-124">`subscription ID or name`Itt található: a kimenetét a `az login` vagy a `az account list` parancsot.</span><span class="sxs-lookup"><span data-stu-id="c1d74-124">`subscription ID or name` can be found in the output of the `az login` or the `az account list` command.</span></span>

3. <span data-ttu-id="c1d74-125">Regisztrálja a szolgáltatót a következő parancs futtatásával.</span><span class="sxs-lookup"><span data-stu-id="c1d74-125">Register the provider by running the following command.</span></span> <span data-ttu-id="c1d74-126">Erőforrás-szolgáltató a szolgáltatások, hogy biztosít erőforrásokat az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="c1d74-126">Resource providers are services that provide resources for your application.</span></span> <span data-ttu-id="c1d74-127">Az Azure-erőforrás, a szolgáltató által telepítése előtt regisztrálnia kell a szolgáltatót.</span><span class="sxs-lookup"><span data-stu-id="c1d74-127">You must register the provider before you can deploy the Azure resource that the provider offers.</span></span>

   ```bash
   az provider register -n "Microsoft.Devices"
   ```
4. <span data-ttu-id="c1d74-128">Hozzon létre egy erőforráscsoportot az USA nyugati régiója régióban iot-minta a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="c1d74-128">Create a resource group named iot-sample in the West US region by running the following command:</span></span>

   ```bash
   az group create --name iot-sample --location westus
   ```

   <span data-ttu-id="c1d74-129">`westus`az a hely, az erőforráscsoport létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="c1d74-129">`westus` is the location you create your resource group.</span></span> <span data-ttu-id="c1d74-130">Ha egy másik hely használni kívánt, futtathatja `az account list-locations -o table` megtekintéséhez az összes hely Azure támogatja.</span><span class="sxs-lookup"><span data-stu-id="c1d74-130">If you want to use another location, you can run `az account list-locations -o table` to see all the locations Azure supports.</span></span>
 
5. <span data-ttu-id="c1d74-131">Az iot-minta erőforráscsoportban IoT hub létrehozása a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="c1d74-131">Create an IoT hub in the iot-sample resource group by running the following command:</span></span>

   ```bash
   az iot hub create --name {my hub name} --resource-group iot-sample
   ```

   <span data-ttu-id="c1d74-132">Alapértelmezés szerint a létrehoz egy IoT-központ az ingyenes tarifacsomag.</span><span class="sxs-lookup"><span data-stu-id="c1d74-132">By default, the tool creates an IoT Hub in the Free pricing tier.</span></span> <span data-ttu-id="c1d74-133">További információkért lásd: [Azure IoT Hub árképzési](https://azure.microsoft.com/pricing/details/iot-hub/).</span><span class="sxs-lookup"><span data-stu-id="c1d74-133">For more infomation, see [Azure IoT Hub pricing](https://azure.microsoft.com/pricing/details/iot-hub/).</span></span>

> [!NOTE]
> <span data-ttu-id="c1d74-134">Az IoT hub nevét globálisan egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="c1d74-134">The name of your IoT hub must be globally unique.</span></span> <span data-ttu-id="c1d74-135">Az Azure-előfizetéshez tartozó Azure IoT Hub kiadása csak egy F1 hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="c1d74-135">You can create only one F1 edition of Azure IoT Hub under your Azure subscription.</span></span>

## <a name="register-pi-in-your-iot-hub"></a><span data-ttu-id="c1d74-136">Az IoT hub Pi regisztrálása</span><span class="sxs-lookup"><span data-stu-id="c1d74-136">Register Pi in your IoT hub</span></span>
<span data-ttu-id="c1d74-137">Minden eszköz, amely üzeneteket küld az IoT hub, valamint üzeneteket fogad az IoT hub regisztrálni kell egy egyedi azonosítót.</span><span class="sxs-lookup"><span data-stu-id="c1d74-137">Each device that sends messages to your IoT hub and receives messages from your IoT hub must be registered with a unique ID.</span></span>

<span data-ttu-id="c1d74-138">A központ Pi regisztrálása fut a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="c1d74-138">Register Pi in your hub by running following command:</span></span>

```bash
az iot device create --device-id myraspberrypi --hub {my hub name} --resource-group iot-sample
```

## <a name="summary"></a><span data-ttu-id="c1d74-139">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="c1d74-139">Summary</span></span>
<span data-ttu-id="c1d74-140">Az IoT-központ elkészítette és Pi regisztrálva az IoT hub eszköz megadásával.</span><span class="sxs-lookup"><span data-stu-id="c1d74-140">You've created an IoT hub and registered Pi with a device identity in your IoT hub.</span></span> <span data-ttu-id="c1d74-141">Készen áll arra, hogyan Pi üzenetek küldése az IoT hub.</span><span class="sxs-lookup"><span data-stu-id="c1d74-141">You're ready to learn how to send messages from Pi to your IoT hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c1d74-142">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c1d74-142">Next steps</span></span>
<span data-ttu-id="c1d74-143">[Hozzon létre egy Azure függvény alkalmazást és feldolgozni, és az IoT hub üzenetek tárolásához Azure Storage-fiók](iot-hub-raspberry-pi-kit-c-lesson3-deploy-resource-manager-template.md).</span><span class="sxs-lookup"><span data-stu-id="c1d74-143">[Create an Azure function app and an Azure Storage account to process and store IoT hub messages](iot-hub-raspberry-pi-kit-c-lesson3-deploy-resource-manager-template.md).</span></span>

