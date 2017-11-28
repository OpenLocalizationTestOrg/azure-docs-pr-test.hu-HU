---
title: "Connect Raspberry pi (C) tooAzure IoT - lecke 2: eszköz regisztrálása |} Microsoft Docs"
description: "Hozzon létre egy erőforráscsoportot, Azure IoT hub létrehozása és regisztrálása Pi hello Azure IoT hub hello Azure parancssori felület használatával."
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
ms.openlocfilehash: 473658c5a8e1e0d4cfced0efafbad2640a1e0696
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-iot-hub-and-register-raspberry-pi-3"></a><span data-ttu-id="19faa-104">Az IoT hub létrehozni és regisztrálni az málna Pi 3</span><span class="sxs-lookup"><span data-stu-id="19faa-104">Create your IoT hub and register Raspberry Pi 3</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="19faa-105">Mit fog</span><span class="sxs-lookup"><span data-stu-id="19faa-105">What you will do</span></span>
* <span data-ttu-id="19faa-106">Hozzon létre egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="19faa-106">Create a resource group.</span></span>
* <span data-ttu-id="19faa-107">Az Azure IoT hub létrehozása hello erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="19faa-107">Create your Azure IoT hub in hello resource group.</span></span>
* <span data-ttu-id="19faa-108">Adja hozzá a málna Pi 3 toohello Azure IoT hub hello Azure parancssori felület (CLI) használatával.</span><span class="sxs-lookup"><span data-stu-id="19faa-108">Add Raspberry Pi 3 toohello Azure IoT hub by using hello Azure command-line interface (Azure CLI).</span></span>

<span data-ttu-id="19faa-109">Hello Azure CLI tooadd Pi tooyour IoT-központ használatakor hello szolgáltatás kulcsot hoz létre egy az Pi tooauthenticate hello szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="19faa-109">When you use hello Azure CLI tooadd Pi tooyour IoT hub, hello service generates a key for Pi tooauthenticate with hello service.</span></span> <span data-ttu-id="19faa-110">Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="19faa-110">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="19faa-111">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="19faa-111">What you will learn</span></span>
<span data-ttu-id="19faa-112">Ebből a cikkből megtudhatja:</span><span class="sxs-lookup"><span data-stu-id="19faa-112">In this article, you will learn:</span></span>
* <span data-ttu-id="19faa-113">Hogyan toouse hello Azure CLI toocreate egy IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="19faa-113">How toouse hello Azure CLI toocreate an IoT hub.</span></span>
* <span data-ttu-id="19faa-114">Hogyan toocreate az IoT hub eszköz identitása pi.</span><span class="sxs-lookup"><span data-stu-id="19faa-114">How toocreate a device identity for Pi in your IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="19faa-115">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="19faa-115">What you need</span></span>
* <span data-ttu-id="19faa-116">Az Azure-fiók</span><span class="sxs-lookup"><span data-stu-id="19faa-116">An Azure account</span></span>
* <span data-ttu-id="19faa-117">Mac vagy Windows-számítógép hello Azure parancssori felület telepítése</span><span class="sxs-lookup"><span data-stu-id="19faa-117">A Mac or a Windows computer with hello Azure CLI installed</span></span>

## <a name="create-your-iot-hub"></a><span data-ttu-id="19faa-118">Az IoT hub létrehozása</span><span class="sxs-lookup"><span data-stu-id="19faa-118">Create your IoT hub</span></span>
<span data-ttu-id="19faa-119">Azure IoT-központ segítségével csatlakozzon, figyeléséhez és több millió az IoT-eszközök kezelése.</span><span class="sxs-lookup"><span data-stu-id="19faa-119">Azure IoT Hub helps you connect, monitor, and manage millions of IoT assets.</span></span> <span data-ttu-id="19faa-120">toocreate az IoT hub, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="19faa-120">toocreate your IoT hub, follow these steps:</span></span>

1. <span data-ttu-id="19faa-121">Jelentkezzen be tooyour Azure-fiók hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="19faa-121">Sign in tooyour Azure account by running hello following command:</span></span>

   ```bash
   az login
   ```

   <span data-ttu-id="19faa-122">Az elérhető előfizetések sikeres bejelentkezés után vannak felsorolva.</span><span class="sxs-lookup"><span data-stu-id="19faa-122">All your available subscriptions are listed after a successful sign-in.</span></span>

2. <span data-ttu-id="19faa-123">Állítsa be a hello alapértelmezett előfizetést, amelyet az toouse hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="19faa-123">Set hello default subscription that you want toouse by running hello following command:</span></span>

   ```bash
   az account set --subscription {subscription id or name}
   ```

   <span data-ttu-id="19faa-124">`subscription ID or name`Itt található: hello hello kimenete `az login` vagy hello `az account list` parancsot.</span><span class="sxs-lookup"><span data-stu-id="19faa-124">`subscription ID or name` can be found in hello output of hello `az login` or hello `az account list` command.</span></span>

3. <span data-ttu-id="19faa-125">Hello szolgáltató regisztrálása hello a következő parancs futtatásával.</span><span class="sxs-lookup"><span data-stu-id="19faa-125">Register hello provider by running hello following command.</span></span> <span data-ttu-id="19faa-126">Erőforrás-szolgáltató a szolgáltatások, hogy biztosít erőforrásokat az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="19faa-126">Resource providers are services that provide resources for your application.</span></span> <span data-ttu-id="19faa-127">Hello szolgáltató ajánlatok hello Azure-erőforrás telepítése előtt regisztrálnia kell az hello szolgáltató.</span><span class="sxs-lookup"><span data-stu-id="19faa-127">You must register hello provider before you can deploy hello Azure resource that hello provider offers.</span></span>

   ```bash
   az provider register -n "Microsoft.Devices"
   ```
4. <span data-ttu-id="19faa-128">Hozzon létre egy erőforráscsoportot nevű iot-minta hello USA nyugati régiója régióban hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="19faa-128">Create a resource group named iot-sample in hello West US region by running hello following command:</span></span>

   ```bash
   az group create --name iot-sample --location westus
   ```

   <span data-ttu-id="19faa-129">`westus`az hello hely, az erőforráscsoport létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="19faa-129">`westus` is hello location you create your resource group.</span></span> <span data-ttu-id="19faa-130">Ha máshová szeretné toouse, futtathatja `az account list-locations -o table` toosee összes hello Azure támogatja helyét.</span><span class="sxs-lookup"><span data-stu-id="19faa-130">If you want toouse another location, you can run `az account list-locations -o table` toosee all hello locations Azure supports.</span></span>
 
5. <span data-ttu-id="19faa-131">IoT hub létrehozása hello iot-minta erőforráscsoportban hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="19faa-131">Create an IoT hub in hello iot-sample resource group by running hello following command:</span></span>

   ```bash
   az iot hub create --name {my hub name} --resource-group iot-sample
   ```

   <span data-ttu-id="19faa-132">Alapértelmezés szerint hello létrehoz egy IoT-központot hello ingyenes tarifacsomag.</span><span class="sxs-lookup"><span data-stu-id="19faa-132">By default, hello tool creates an IoT Hub in hello Free pricing tier.</span></span> <span data-ttu-id="19faa-133">További információkért lásd: [Azure IoT Hub árképzési](https://azure.microsoft.com/pricing/details/iot-hub/).</span><span class="sxs-lookup"><span data-stu-id="19faa-133">For more infomation, see [Azure IoT Hub pricing](https://azure.microsoft.com/pricing/details/iot-hub/).</span></span>

> [!NOTE]
> <span data-ttu-id="19faa-134">az IoT hub hello nevének globálisan egyedi kell lennie.</span><span class="sxs-lookup"><span data-stu-id="19faa-134">hello name of your IoT hub must be globally unique.</span></span> <span data-ttu-id="19faa-135">Az Azure-előfizetéshez tartozó Azure IoT Hub kiadása csak egy F1 hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="19faa-135">You can create only one F1 edition of Azure IoT Hub under your Azure subscription.</span></span>

## <a name="register-pi-in-your-iot-hub"></a><span data-ttu-id="19faa-136">Az IoT hub Pi regisztrálása</span><span class="sxs-lookup"><span data-stu-id="19faa-136">Register Pi in your IoT hub</span></span>
<span data-ttu-id="19faa-137">Minden eszköz, amely üzeneteket küld üzenetek tooyour IoT-központot, illetve üzeneteket fogad az IoT hub regisztrálni kell egy egyedi azonosítót.</span><span class="sxs-lookup"><span data-stu-id="19faa-137">Each device that sends messages tooyour IoT hub and receives messages from your IoT hub must be registered with a unique ID.</span></span>

<span data-ttu-id="19faa-138">A központ Pi regisztrálása fut a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="19faa-138">Register Pi in your hub by running following command:</span></span>

```bash
az iot device create --device-id myraspberrypi --hub {my hub name} --resource-group iot-sample
```

## <a name="summary"></a><span data-ttu-id="19faa-139">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="19faa-139">Summary</span></span>
<span data-ttu-id="19faa-140">Az IoT-központ elkészítette és Pi regisztrálva az IoT hub eszköz megadásával.</span><span class="sxs-lookup"><span data-stu-id="19faa-140">You've created an IoT hub and registered Pi with a device identity in your IoT hub.</span></span> <span data-ttu-id="19faa-141">Most készen áll a toolearn hogyan toosend a Pi tooyour IoT-központ érkező üzenetek.</span><span class="sxs-lookup"><span data-stu-id="19faa-141">You're ready toolearn how toosend messages from Pi tooyour IoT hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="19faa-142">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="19faa-142">Next steps</span></span>
<span data-ttu-id="19faa-143">[Hozzon létre egy Azure függvény alkalmazás és az Azure Storage fiók tooprocess és a tároló IoT-központ üzenetek](iot-hub-raspberry-pi-kit-c-lesson3-deploy-resource-manager-template.md).</span><span class="sxs-lookup"><span data-stu-id="19faa-143">[Create an Azure function app and an Azure Storage account tooprocess and store IoT hub messages](iot-hub-raspberry-pi-kit-c-lesson3-deploy-resource-manager-template.md).</span></span>

