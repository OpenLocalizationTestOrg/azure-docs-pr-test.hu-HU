---
title: "Csatlakozás Arduino tooAzure IoT - lecke 2: eszköz regisztrálása |} Microsoft Docs"
description: "Hozzon létre egy erőforráscsoportot, Azure IoT hub létrehozása és regisztrálása Adafruit lágyított M0 Wi-Fi hello Azure IoT hub hello Azure parancssori felület használatával."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Csatlakozás arduino toocloud, az azure iot hub, internet dolgot felhőalapú azure iot hub eszköz, arduino felhő létrehozása"
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
ms.openlocfilehash: ca362f9c143dd3a98bf47a66b63a9725a0ffc2d8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-iot-hub-and-register-your-adafruit-feather-m0-wifi-arduino-board"></a><span data-ttu-id="290aa-104">Az IoT hub létrehozása és regisztrálása a Adafruit lágyított M0 Wi-Fi Arduino board</span><span class="sxs-lookup"><span data-stu-id="290aa-104">Create your IoT hub and register your Adafruit Feather M0 WiFi Arduino board</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="290aa-105">Mit fog</span><span class="sxs-lookup"><span data-stu-id="290aa-105">What you will do</span></span>
* <span data-ttu-id="290aa-106">Hozzon létre egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="290aa-106">Create a resource group.</span></span>
* <span data-ttu-id="290aa-107">Az Azure IoT hub létrehozása hello erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="290aa-107">Create your Azure IoT hub in hello resource group.</span></span>
* <span data-ttu-id="290aa-108">Adja hozzá az Arduino board toohello Azure IoT hub hello Azure parancssori felület (CLI) használatával.</span><span class="sxs-lookup"><span data-stu-id="290aa-108">Add your Arduino board toohello Azure IoT hub by using hello Azure command-line interface (Azure CLI).</span></span>

<span data-ttu-id="290aa-109">Hello Azure CLI tooadd használatakor az Arduino Bizottság tooyour IoT hub hello szolgáltatás kulcsot hoz létre egy az a Arduino board tooauthenticate hello szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="290aa-109">When you use hello Azure CLI tooadd your Arduino board tooyour IoT hub, hello service generates a key for your Arduino board tooauthenticate with hello service.</span></span> <span data-ttu-id="290aa-110">Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási][troubleshoot].</span><span class="sxs-lookup"><span data-stu-id="290aa-110">If you have any problems, look for solutions on hello [troubleshooting page][troubleshoot].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="290aa-111">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="290aa-111">What you will learn</span></span>
<span data-ttu-id="290aa-112">Ebből a cikkből megtudhatja:</span><span class="sxs-lookup"><span data-stu-id="290aa-112">In this article, you will learn:</span></span>
* <span data-ttu-id="290aa-113">Hogyan toouse hello Azure CLI toocreate egy IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="290aa-113">How toouse hello Azure CLI toocreate an IoT hub.</span></span>
* <span data-ttu-id="290aa-114">Hogyan toocreate egy eszköz identitást a Arduino board az IoT hub.</span><span class="sxs-lookup"><span data-stu-id="290aa-114">How toocreate a device identity for your Arduino board in your IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="290aa-115">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="290aa-115">What you need</span></span>
* <span data-ttu-id="290aa-116">Az Azure-fiók</span><span class="sxs-lookup"><span data-stu-id="290aa-116">An Azure account</span></span>
* <span data-ttu-id="290aa-117">Egy számítógép, amelyen hello Azure parancssori felület telepítve</span><span class="sxs-lookup"><span data-stu-id="290aa-117">A computer with hello Azure CLI installed</span></span>

## <a name="create-your-iot-hub"></a><span data-ttu-id="290aa-118">Az IoT hub létrehozása</span><span class="sxs-lookup"><span data-stu-id="290aa-118">Create your IoT hub</span></span>
<span data-ttu-id="290aa-119">Azure IoT-központ segítségével csatlakozzon, figyeléséhez és több millió az IoT-eszközök kezelése.</span><span class="sxs-lookup"><span data-stu-id="290aa-119">Azure IoT Hub helps you connect, monitor, and manage millions of IoT assets.</span></span> <span data-ttu-id="290aa-120">toocreate az IoT hub, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="290aa-120">toocreate your IoT hub, follow these steps:</span></span>

1. <span data-ttu-id="290aa-121">Jelentkezzen be tooyour Azure-fiók hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="290aa-121">Sign in tooyour Azure account by running hello following command:</span></span>

   ```bash
   az login
   ```

   <span data-ttu-id="290aa-122">Az elérhető előfizetések sikeres bejelentkezés után vannak felsorolva.</span><span class="sxs-lookup"><span data-stu-id="290aa-122">All your available subscriptions are listed after a successful sign-in.</span></span>

2. <span data-ttu-id="290aa-123">Állítsa be a hello alapértelmezett előfizetést, amelyet az toouse hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="290aa-123">Set hello default subscription that you want toouse by running hello following command:</span></span>

   ```bash
   az account set --subscription {subscription id or name}
   ```

   <span data-ttu-id="290aa-124">`subscription ID or name`Itt található: hello hello kimenete `az login` vagy hello `az account list` parancsot.</span><span class="sxs-lookup"><span data-stu-id="290aa-124">`subscription ID or name` can be found in hello output of hello `az login` or hello `az account list` command.</span></span>

3. <span data-ttu-id="290aa-125">Hello szolgáltató regisztrálása hello a következő parancs futtatásával.</span><span class="sxs-lookup"><span data-stu-id="290aa-125">Register hello provider by running hello following command.</span></span> <span data-ttu-id="290aa-126">Erőforrás-szolgáltató a szolgáltatások, hogy biztosít erőforrásokat az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="290aa-126">Resource providers are services that provide resources for your application.</span></span> <span data-ttu-id="290aa-127">Hello szolgáltató ajánlatok hello Azure-erőforrás telepítése előtt regisztrálnia kell az hello szolgáltató.</span><span class="sxs-lookup"><span data-stu-id="290aa-127">You must register hello provider before you can deploy hello Azure resource that hello provider offers.</span></span>

   ```bash
   az provider register -n "Microsoft.Devices"
   ```
4. <span data-ttu-id="290aa-128">Hozzon létre egy erőforráscsoportot nevű iot-minta hello USA nyugati régiója régióban hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="290aa-128">Create a resource group named iot-sample in hello West US region by running hello following command:</span></span>

   ```bash
   az group create --name iot-sample --location westus
   ```

   <span data-ttu-id="290aa-129">`westus`az hello hely, az erőforráscsoport létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="290aa-129">`westus` is hello location you create your resource group.</span></span> <span data-ttu-id="290aa-130">Ha máshová szeretné toouse, futtathatja `az account list-locations -o table` toosee összes hello Azure támogatja helyét.</span><span class="sxs-lookup"><span data-stu-id="290aa-130">If you want toouse another location, you can run `az account list-locations -o table` toosee all hello locations Azure supports.</span></span>

5. <span data-ttu-id="290aa-131">IoT hub létrehozása hello iot-minta erőforráscsoportban hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="290aa-131">Create an IoT hub in hello iot-sample resource group by running hello following command:</span></span>

   ```bash
   az iot hub create --name {my hub name} --resource-group iot-sample
   ```

<span data-ttu-id="290aa-132">Alapértelmezés szerint hello létrehoz egy IoT-központot hello ingyenes tarifacsomag.</span><span class="sxs-lookup"><span data-stu-id="290aa-132">By default, hello tool creates an IoT Hub in hello Free pricing tier.</span></span> <span data-ttu-id="290aa-133">További információkért lásd: [Azure IoT Hub árképzési](https://azure.microsoft.com/pricing/details/iot-hub/).</span><span class="sxs-lookup"><span data-stu-id="290aa-133">For more infomation, see [Azure IoT Hub pricing](https://azure.microsoft.com/pricing/details/iot-hub/).</span></span>

> [!NOTE]
> <span data-ttu-id="290aa-134">az IoT hub hello nevének globálisan egyedi kell lennie.</span><span class="sxs-lookup"><span data-stu-id="290aa-134">hello name of your IoT hub must be globally unique.</span></span>
> <span data-ttu-id="290aa-135">Az Azure-előfizetéshez tartozó Azure IoT Hub kiadása csak egy F1 hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="290aa-135">You can create only one F1 edition of Azure IoT Hub under your Azure subscription.</span></span>

## <a name="register-your-arduino-board-in-your-iot-hub"></a><span data-ttu-id="290aa-136">Regisztrálja a Arduino board az IoT hub</span><span class="sxs-lookup"><span data-stu-id="290aa-136">Register your Arduino board in your IoT hub</span></span>
<span data-ttu-id="290aa-137">Minden eszköz, amely üzeneteket küld üzenetek tooyour IoT-központot, illetve üzeneteket fogad az IoT hub regisztrálni kell egy egyedi azonosítót.</span><span class="sxs-lookup"><span data-stu-id="290aa-137">Each device that sends messages tooyour IoT hub and receives messages from your IoT hub must be registered with a unique ID.</span></span>

<span data-ttu-id="290aa-138">Az IoT hub a Arduino board regisztrálása fut a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="290aa-138">Register your Arduino board in your IoT hub by running following command:</span></span>

```bash
az iot device create --device-id mym0wifi --hub-name {my hub name}
```

## <a name="summary"></a><span data-ttu-id="290aa-139">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="290aa-139">Summary</span></span>
<span data-ttu-id="290aa-140">Az IoT-központ elkészítette és a Arduino board regisztrálva az IoT hub eszköz identitással.</span><span class="sxs-lookup"><span data-stu-id="290aa-140">You've created an IoT hub and registered your Arduino board with a device identity in your IoT hub.</span></span> <span data-ttu-id="290aa-141">Most készen áll a toolearn hogyan toosend üzenetek a Arduino board tooyour IoT hubról.</span><span class="sxs-lookup"><span data-stu-id="290aa-141">You're ready toolearn how toosend messages from your Arduino board tooyour IoT hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="290aa-142">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="290aa-142">Next steps</span></span>
<span data-ttu-id="290aa-143">[Hozzon létre egy Azure függvény alkalmazás és az Azure Storage fiók tooprocess és a tároló IoT-központ üzenetek][process-and-store-iot-hub-messages].</span><span class="sxs-lookup"><span data-stu-id="290aa-143">[Create an Azure function app and an Azure Storage account tooprocess and store IoT hub messages][process-and-store-iot-hub-messages].</span></span>


<!-- Images and links -->

[troubleshoot]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md
[process-and-store-iot-hub-messages]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson3-deploy-resource-manager-template.md