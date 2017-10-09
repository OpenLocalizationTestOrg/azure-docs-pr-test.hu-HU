---
title: "SensorTag eszköz & Azure IoT átjáró - rész 2: eszköz regisztrálása |} Microsoft Docs"
description: 
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "az Azure iot hub dolgot felhőalapú azure iot-központ az internet létrehozása eszköz, a ti sensortag, a ti bla"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: 2c18f5ae-e39a-48ae-a9fe-04bb595740a0
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 5d2322268aa18f52f60c2833778323773ac4eec3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-azure-iot-hub-and-register-your-device"></a><span data-ttu-id="36b0c-103">Az Azure IoT hub létrehozása, és regisztrálja az eszközt</span><span class="sxs-lookup"><span data-stu-id="36b0c-103">Create your Azure IoT hub and register your device</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="36b0c-104">Mit fog</span><span class="sxs-lookup"><span data-stu-id="36b0c-104">What you will do</span></span>

- <span data-ttu-id="36b0c-105">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="36b0c-105">Create a resource group</span></span>
- <span data-ttu-id="36b0c-106">Az első IoT hub létrehozása</span><span class="sxs-lookup"><span data-stu-id="36b0c-106">Create your first IoT hub</span></span>
- <span data-ttu-id="36b0c-107">Regisztrálja az eszközt az IoT hub hello Azure parancssori felület használatával.</span><span class="sxs-lookup"><span data-stu-id="36b0c-107">Register your device in your IoT hub by using hello Azure CLI.</span></span> 

<span data-ttu-id="36b0c-108">Amikor regisztrál az eszközt az IoT hub, hello Azure IoT-központ szolgáltatás kulcsot hoz létre egy az az eszköz toouse tooauthenticate hello szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="36b0c-108">When you register your device in your IoT hub, hello Azure IoT Hub service generates a key for your device toouse tooauthenticate with hello service.</span></span> 

<span data-ttu-id="36b0c-109">Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási](iot-hub-gateway-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="36b0c-109">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-gateway-kit-c-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="36b0c-110">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="36b0c-110">What you will learn</span></span>

<span data-ttu-id="36b0c-111">Ez a lecke témák köre:</span><span class="sxs-lookup"><span data-stu-id="36b0c-111">In this lesson, you will learn:</span></span>

- <span data-ttu-id="36b0c-112">Hogyan toouse hello Azure CLI toocreate egy IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="36b0c-112">How toouse hello Azure CLI toocreate an IoT hub.</span></span>
- <span data-ttu-id="36b0c-113">Hogyan tooregister egy eszközt az IoT-központ.</span><span class="sxs-lookup"><span data-stu-id="36b0c-113">How tooregister a device in an IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="36b0c-114">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="36b0c-114">What you need</span></span>

- <span data-ttu-id="36b0c-115">Aktív Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="36b0c-115">An active Azure subscription.</span></span> <span data-ttu-id="36b0c-116">Ha az Azure-fiók nem rendelkezik, akkor létrehozhat egy [ingyenes Azure próba-fiókot](http://azure.microsoft.com/pricing/free-trial/) csak néhány perc múlva.</span><span class="sxs-lookup"><span data-stu-id="36b0c-116">If you don't have an Azure account, you can create a [free Azure trial account](http://azure.microsoft.com/pricing/free-trial/) in just a few minutes.</span></span>
- <span data-ttu-id="36b0c-117">Az Azure parancssori felület telepítve hello kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="36b0c-117">You should have hello Azure CLI installed.</span></span>

## <a name="create-an-iot-hub"></a><span data-ttu-id="36b0c-118">IoT Hub létrehozása</span><span class="sxs-lookup"><span data-stu-id="36b0c-118">Create an IoT hub</span></span>

<span data-ttu-id="36b0c-119">az IoT-központ toocreate kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="36b0c-119">toocreate an IoT hub, follow these steps:</span></span>

1. <span data-ttu-id="36b0c-120">Jelentkezzen be tooyour Azure-fiók hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="36b0c-120">Sign in tooyour Azure account by running hello following command:</span></span>

   ```bash
   az login
   ```

   <span data-ttu-id="36b0c-121">Sikeres bejelentkezés után megjelenik az elérhető előfizetések.</span><span class="sxs-lookup"><span data-stu-id="36b0c-121">All your available subscriptions will be listed after a successful sign-in.</span></span>

2. <span data-ttu-id="36b0c-122">Hello alapértelmezett beállítása Azure-előfizetést, amelyet az toouse hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="36b0c-122">Set hello default Azure subscription that you want toouse by running hello following command:</span></span>

   ```bash
   az account set --subscription {subscription id or name}
   ```

   <span data-ttu-id="36b0c-123">`subscription ID or name`Itt található: hello hello kimenete `az login` vagy hello `az account list` parancsot.</span><span class="sxs-lookup"><span data-stu-id="36b0c-123">`subscription ID or name` can be found in hello output of hello `az login` or hello `az account list` command.</span></span>

3. <span data-ttu-id="36b0c-124">Hello szolgáltató regisztrálása hello a következő parancs futtatásával.</span><span class="sxs-lookup"><span data-stu-id="36b0c-124">Register hello provider by running hello following command.</span></span> <span data-ttu-id="36b0c-125">Erőforrás-szolgáltató a szolgáltatások, hogy biztosít erőforrásokat az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="36b0c-125">Resource providers are services that provide resources for your application.</span></span> <span data-ttu-id="36b0c-126">Hello szolgáltató ajánlatok hello Azure-erőforrás telepítése előtt regisztrálnia kell az hello szolgáltató.</span><span class="sxs-lookup"><span data-stu-id="36b0c-126">You must register hello provider before you can deploy hello Azure resource that hello provider offers.</span></span>

   ```bash
   az provider register -n "Microsoft.Devices"
   ```

4. <span data-ttu-id="36b0c-127">Hozzon létre egy erőforráscsoportot nevű `iot-gateway` hello USA nyugati régiója régióban hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="36b0c-127">Create a resource group named `iot-gateway` in hello West US region by running hello following command:</span></span>

   ```bash
   az group create --name iot-gateway --location westus
   ```
   
   <span data-ttu-id="36b0c-128">`westus`az hello hely, az erőforráscsoport létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="36b0c-128">`westus` is hello location you create your resource group.</span></span> <span data-ttu-id="36b0c-129">Ha máshová szeretné toouse, futtathatja `az account list-locations -o table` toosee összes hello Azure támogatja helyét.</span><span class="sxs-lookup"><span data-stu-id="36b0c-129">If you want toouse another location, you can run `az account list-locations -o table` toosee all hello locations Azure supports.</span></span>

5. <span data-ttu-id="36b0c-130">Hozzon létre egy IoT-központot hello `iot-gateway` erőforráscsoport hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="36b0c-130">Create an IoT hub in hello `iot-gateway` resource group by running hello following command:</span></span>

   ```bash
   az iot hub create --name {my hub name} --resource-group iot-gateway
   ```

<span data-ttu-id="36b0c-131">Alapértelmezés szerint hello létrehoz egy IoT-központot hello ingyenes tarifacsomag.</span><span class="sxs-lookup"><span data-stu-id="36b0c-131">By default, hello tool creates an IoT Hub in hello Free pricing tier.</span></span> <span data-ttu-id="36b0c-132">További információkért lásd: [Azure IoT Hub árképzési](https://azure.microsoft.com/pricing/details/iot-hub/).</span><span class="sxs-lookup"><span data-stu-id="36b0c-132">For more infomation, see [Azure IoT Hub pricing](https://azure.microsoft.com/pricing/details/iot-hub/).</span></span>

> [!NOTE]
> <span data-ttu-id="36b0c-133">az IoT hub hello nevének globálisan egyedi kell lennie.</span><span class="sxs-lookup"><span data-stu-id="36b0c-133">hello name of your IoT hub must be globally unique.</span></span> <span data-ttu-id="36b0c-134">Az Azure-előfizetéshez tartozó Azure Iot Hub kiadása csak egy F1 hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="36b0c-134">You can create only one F1 edition of Azure Iot Hub under your Azure subscription.</span></span>

## <a name="register-your-device-in-your-iot-hub"></a><span data-ttu-id="36b0c-135">Regisztrálja az eszközt az IoT hub</span><span class="sxs-lookup"><span data-stu-id="36b0c-135">Register your device in your IoT hub</span></span>

<span data-ttu-id="36b0c-136">Minden eszköz, amely üzeneteket küld üzenetek tooyour IoT-központot, illetve üzeneteket fogad az IoT hub regisztrálni kell egy egyedi azonosítót.</span><span class="sxs-lookup"><span data-stu-id="36b0c-136">Each device that sends messages tooyour IoT hub and receives messages from your IoT hub must be registered with a unique ID.</span></span>
<span data-ttu-id="36b0c-137">Az eszköz regisztrálása az IoT hub fut a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="36b0c-137">Register your device in your IoT hub by running following command:</span></span>

```bash
az iot device create --device-id mydevice --hub-name {my hub name} --resource-group iot-gateway
```

## <a name="summary"></a><span data-ttu-id="36b0c-138">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="36b0c-138">Summary</span></span>

<span data-ttu-id="36b0c-139">Az IoT-központ elkészítette és a logikai eszköz regisztrálva az IoT hub eszköz megadásával.</span><span class="sxs-lookup"><span data-stu-id="36b0c-139">You've created an IoT hub and registered your logical device with a device identity in your IoT hub.</span></span> <span data-ttu-id="36b0c-140">Most készen áll a toolearn hogyan tooconfigure és a fizikai eszköz tooyour IoT-központ a hello egy átjáró alkalmazás toosend mintaadatok futtatható felhőalapú.</span><span class="sxs-lookup"><span data-stu-id="36b0c-140">You're ready toolearn how tooconfigure and run a gateway sample application toosend data from your physical device tooyour IoT hub in hello cloud.</span></span>

## <a name="next-steps"></a><span data-ttu-id="36b0c-141">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="36b0c-141">Next steps</span></span>
[<span data-ttu-id="36b0c-142">Konfigurálja és egy táblázat mintaalkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="36b0c-142">Configure and run a BLE sample app</span></span>](iot-hub-gateway-kit-c-lesson3-configure-ble-app.md)