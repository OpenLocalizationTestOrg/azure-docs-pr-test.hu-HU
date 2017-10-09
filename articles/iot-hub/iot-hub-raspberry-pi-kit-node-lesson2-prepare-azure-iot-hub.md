---
featureFlags: usabilla
title: "Csatlakozás málna Pi (csomópont) tooAzure IoT - lecke 2: eszköz regisztrálása |} Microsoft Docs"
description: "Hozzon létre egy erőforráscsoportot, hozzon létre egy Azure IoT-központot, és Pi regisztrálni kell az IoT-központ identitásjegyzékhez hello Azure parancssori felület használatával."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "Raspberry pi felhő pi felhő csatlakozás"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: 736215b6-e7e4-46f9-af30-0ded9ffa5204
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 97533298d52d1187c49a4c35ddda922d6e45c87d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-iot-hub-and-register-raspberry-pi-3"></a><span data-ttu-id="6a5bf-104">Az IoT hub létrehozni és regisztrálni az málna Pi 3</span><span class="sxs-lookup"><span data-stu-id="6a5bf-104">Create your IoT hub and register Raspberry Pi 3</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="6a5bf-105">Mit fog</span><span class="sxs-lookup"><span data-stu-id="6a5bf-105">What you will do</span></span>
* <span data-ttu-id="6a5bf-106">Hozzon létre egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="6a5bf-106">Create a resource group.</span></span>
* <span data-ttu-id="6a5bf-107">Az Azure IoT hub létrehozása hello erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="6a5bf-107">Create your Azure IoT hub in hello resource group.</span></span>
* <span data-ttu-id="6a5bf-108">Adja hozzá a málna Pi 3 toohello Azure IoT hub hello Azure parancssori felület (CLI) használatával.</span><span class="sxs-lookup"><span data-stu-id="6a5bf-108">Add Raspberry Pi 3 toohello Azure IoT hub by using hello Azure command-line interface (Azure CLI).</span></span>

<span data-ttu-id="6a5bf-109">Az Azure parancssori felület tooadd Pi tooyour IoT-központ használatakor hello szolgáltatás kulcsot hoz létre egy az Pi tooauthenticate hello szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="6a5bf-109">When you use Azure CLI tooadd Pi tooyour IoT hub, hello service generates a key for Pi tooauthenticate with hello service.</span></span> <span data-ttu-id="6a5bf-110">Ha bármilyen problémába ütközik, a keresési hello megoldások [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="6a5bf-110">If you have any problems, seek solutions on hello [troubleshooting page](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="6a5bf-111">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="6a5bf-111">What you will learn</span></span>
<span data-ttu-id="6a5bf-112">Ebből a cikkből megtudhatja:</span><span class="sxs-lookup"><span data-stu-id="6a5bf-112">In this article, you will learn:</span></span>
* <span data-ttu-id="6a5bf-113">Hogyan toouse Azure CLI toocreate az IoT-központ</span><span class="sxs-lookup"><span data-stu-id="6a5bf-113">How toouse Azure CLI toocreate an IoT hub</span></span>
* <span data-ttu-id="6a5bf-114">Hogyan toocreate az IoT hub eszköz identitása pi</span><span class="sxs-lookup"><span data-stu-id="6a5bf-114">How toocreate a device identity for Pi in your IoT hub</span></span>

## <a name="what-you-need"></a><span data-ttu-id="6a5bf-115">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="6a5bf-115">What you need</span></span>
* <span data-ttu-id="6a5bf-116">Az Azure-fiók</span><span class="sxs-lookup"><span data-stu-id="6a5bf-116">An Azure account</span></span>
* <span data-ttu-id="6a5bf-117">Mac vagy Windows-számítógép hello Azure parancssori felület telepítése</span><span class="sxs-lookup"><span data-stu-id="6a5bf-117">A Mac or a Windows computer with hello Azure CLI installed</span></span>

## <a name="create-your-iot-hub"></a><span data-ttu-id="6a5bf-118">Az IoT hub létrehozása</span><span class="sxs-lookup"><span data-stu-id="6a5bf-118">Create your IoT hub</span></span>
<span data-ttu-id="6a5bf-119">Azure IoT-központ segítségével csatlakozzon, figyeléséhez és több millió az IoT-eszközök kezelése.</span><span class="sxs-lookup"><span data-stu-id="6a5bf-119">Azure IoT Hub helps you connect, monitor, and manage millions of IoT assets.</span></span> <span data-ttu-id="6a5bf-120">toocreate az IoT hub, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="6a5bf-120">toocreate your IoT hub, follow these steps:</span></span>

1. <span data-ttu-id="6a5bf-121">Jelentkezzen be tooyour Azure-fiók hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="6a5bf-121">Sign in tooyour Azure account by running hello following command:</span></span>

   ```bash
   az login
   ```

   <span data-ttu-id="6a5bf-122">Az elérhető előfizetések sikeres bejelentkezés után vannak felsorolva.</span><span class="sxs-lookup"><span data-stu-id="6a5bf-122">All your available subscriptions are listed after a successful sign-in.</span></span>

2. <span data-ttu-id="6a5bf-123">Állítsa be a hello alapértelmezett előfizetést, amelyet az toouse hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="6a5bf-123">Set hello default subscription that you want toouse by running hello following command:</span></span>

   ```bash
   az account set --subscription {subscription id or name}
   ```

   <span data-ttu-id="6a5bf-124">`subscription ID or name`Itt található: hello hello kimenete `az login` vagy hello `az account list` parancsot.</span><span class="sxs-lookup"><span data-stu-id="6a5bf-124">`subscription ID or name` can be found in hello output of hello `az login` or hello `az account list` command.</span></span>

3. <span data-ttu-id="6a5bf-125">Hello szolgáltató regisztrálása hello a következő parancs futtatásával.</span><span class="sxs-lookup"><span data-stu-id="6a5bf-125">Register hello provider by running hello following command.</span></span> <span data-ttu-id="6a5bf-126">Erőforrás-szolgáltató a szolgáltatások, hogy biztosít erőforrásokat az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="6a5bf-126">Resource providers are services that provide resources for your application.</span></span> <span data-ttu-id="6a5bf-127">Hello szolgáltató ajánlatok hello Azure-erőforrás telepítése előtt regisztrálnia kell az hello szolgáltató.</span><span class="sxs-lookup"><span data-stu-id="6a5bf-127">You must register hello provider before you can deploy hello Azure resource that hello provider offers.</span></span>

   ```bash
   az provider register -n "Microsoft.Devices"
   ```
4. <span data-ttu-id="6a5bf-128">Hozzon létre egy erőforráscsoportot nevű iot-minta hello USA nyugati régiója régióban hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="6a5bf-128">Create a resource group named iot-sample in hello West US region by running hello following command:</span></span>

   ```bash
   az group create --name iot-sample --location westus
   ```

   <span data-ttu-id="6a5bf-129">`westus`az hello hely, az erőforráscsoport létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="6a5bf-129">`westus` is hello location you create your resource group.</span></span> <span data-ttu-id="6a5bf-130">Ha máshová szeretné toouse, futtathatja `az account list-locations -o table` toosee összes hello Azure támogatja helyét.</span><span class="sxs-lookup"><span data-stu-id="6a5bf-130">If you want toouse another location, you can run `az account list-locations -o table` toosee all hello locations Azure supports.</span></span>
 
5. <span data-ttu-id="6a5bf-131">IoT hub létrehozása hello iot-minta erőforráscsoportban hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="6a5bf-131">Create an IoT hub in hello iot-sample resource group by running hello following command:</span></span>

   ```bash
   az iot hub create --name {my hub name} --resource-group iot-sample
   ```

   <span data-ttu-id="6a5bf-132">Alapértelmezés szerint hello létrehoz egy IoT-központot hello ingyenes tarifacsomag.</span><span class="sxs-lookup"><span data-stu-id="6a5bf-132">By default, hello tool creates an IoT Hub in hello Free pricing tier.</span></span> <span data-ttu-id="6a5bf-133">További információkért lásd: [Azure IoT Hub árképzési](https://azure.microsoft.com/pricing/details/iot-hub/).</span><span class="sxs-lookup"><span data-stu-id="6a5bf-133">For more infomation, see [Azure IoT Hub pricing](https://azure.microsoft.com/pricing/details/iot-hub/).</span></span>

> [!NOTE] 
> <span data-ttu-id="6a5bf-134">az IoT hub hello nevének globálisan egyedi kell lennie.</span><span class="sxs-lookup"><span data-stu-id="6a5bf-134">hello name of your IoT hub must be globally unique.</span></span> <span data-ttu-id="6a5bf-135">Az Azure-előfizetéshez tartozó Azure IoT Hub kiadása csak egy F1 hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="6a5bf-135">You can create only one F1 edition of Azure IoT Hub under your Azure subscription.</span></span>

## <a name="register-pi-in-your-iot-hub"></a><span data-ttu-id="6a5bf-136">Az IoT hub Pi regisztrálása</span><span class="sxs-lookup"><span data-stu-id="6a5bf-136">Register Pi in your IoT hub</span></span>
<span data-ttu-id="6a5bf-137">Minden eszköz, amely üzeneteket küld üzenetek tooyour IoT-központot, illetve üzeneteket fogad az IoT hub regisztrálni kell egy egyedi azonosítót.</span><span class="sxs-lookup"><span data-stu-id="6a5bf-137">Each device that sends messages tooyour IoT hub and receives messages from your IoT hub must be registered with a unique ID.</span></span> <span data-ttu-id="6a5bf-138">Először a Azure CLI tooregister a telepítővel, eszközhitelesítési X.509 önaláírt tanúsítvány létrehozása.</span><span class="sxs-lookup"><span data-stu-id="6a5bf-138">You will use Azure CLI tooregister your Pi and create a self-signed X.509 certificate for device authentication.</span></span>

<span data-ttu-id="6a5bf-139">Futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="6a5bf-139">Run hello following command:</span></span>

```bash
# For Windows command prompt
az iot device create --device-id myraspberrypi --hub-name {my hub name} --x509 --output-dir %USERPROFILE%\.iot-hub-getting-started
 
# For macOS or Ubuntu
az iot device create --device-id myraspberrypi --hub-name {my hub name} --x509 --output-dir ~/.iot-hub-getting-started
```

## <a name="summary"></a><span data-ttu-id="6a5bf-140">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="6a5bf-140">Summary</span></span>
<span data-ttu-id="6a5bf-141">Az IoT-központ elkészítette és Pi regisztrálva az IoT hub eszköz megadásával.</span><span class="sxs-lookup"><span data-stu-id="6a5bf-141">You've created an IoT hub and registered Pi with a device identity in your IoT hub.</span></span> <span data-ttu-id="6a5bf-142">Most készen áll a toolearn hogyan toosend a Pi tooyour IoT-központ érkező üzenetek.</span><span class="sxs-lookup"><span data-stu-id="6a5bf-142">You're ready toolearn how toosend messages from Pi tooyour IoT hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6a5bf-143">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6a5bf-143">Next steps</span></span>
[<span data-ttu-id="6a5bf-144">Hozzon létre egy Azure függvény alkalmazást és egy Azure storage-fiók tooprocess és az IoT hub üzenetek tárolásához</span><span class="sxs-lookup"><span data-stu-id="6a5bf-144">Create an Azure function app and an Azure storage account tooprocess and store IoT hub messages</span></span>](iot-hub-raspberry-pi-kit-node-lesson3-deploy-resource-manager-template.md)

