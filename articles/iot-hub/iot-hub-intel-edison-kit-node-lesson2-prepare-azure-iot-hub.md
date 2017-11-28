---
title: "Csatlakozás Intel Edison (csomópont) tooAzure IoT - lecke 2: eszköz regisztrálása |} Microsoft Docs"
description: "Hozzon létre egy erőforráscsoportot, Azure IoT hub létrehozása és regisztrálása Edison hello Azure IoT hub hello Azure parancssori felület használatával."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: 
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-node-get-started
ms.assetid: c1465cc2-d0d9-4326-967c-64d95b61dd54
ms.service: iot-hub
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: a70cd8d6a6d620a2cf6226397e061af6cf81e0af
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-iot-hub-and-register-intel-edison"></a><span data-ttu-id="4aa96-103">Az IoT hub létrehozni és regisztrálni az Intel Edison</span><span class="sxs-lookup"><span data-stu-id="4aa96-103">Create your IoT hub and register Intel Edison</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="4aa96-104">Mit fog</span><span class="sxs-lookup"><span data-stu-id="4aa96-104">What you will do</span></span>
* <span data-ttu-id="4aa96-105">Hozzon létre egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="4aa96-105">Create a resource group.</span></span>
* <span data-ttu-id="4aa96-106">Az Azure IoT hub létrehozása hello erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="4aa96-106">Create your Azure IoT hub in hello resource group.</span></span>
* <span data-ttu-id="4aa96-107">Intel Edison toohello Azure IoT hub hozzáadása hello Azure parancssori felület (CLI) használatával.</span><span class="sxs-lookup"><span data-stu-id="4aa96-107">Add Intel Edison toohello Azure IoT hub by using hello Azure command-line interface (Azure CLI).</span></span>

<span data-ttu-id="4aa96-108">Hello Azure CLI tooadd Edison tooyour IoT-központ használatakor hello szolgáltatás kulcsot hoz létre egy az Edison tooauthenticate hello szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="4aa96-108">When you use hello Azure CLI tooadd Edison tooyour IoT hub, hello service generates a key for Edison tooauthenticate with hello service.</span></span> <span data-ttu-id="4aa96-109">Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="4aa96-109">If you have any problems, look for solutions on hello [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="4aa96-110">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="4aa96-110">What you will learn</span></span>
<span data-ttu-id="4aa96-111">Ebből a cikkből megtudhatja:</span><span class="sxs-lookup"><span data-stu-id="4aa96-111">In this article, you will learn:</span></span>
* <span data-ttu-id="4aa96-112">Hogyan toouse hello Azure CLI toocreate egy IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="4aa96-112">How toouse hello Azure CLI toocreate an IoT hub.</span></span>
* <span data-ttu-id="4aa96-113">Hogyan toocreate az IoT hub eszköz identitása Edison számára.</span><span class="sxs-lookup"><span data-stu-id="4aa96-113">How toocreate a device identity for Edison in your IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="4aa96-114">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="4aa96-114">What you need</span></span>
* <span data-ttu-id="4aa96-115">Egy Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="4aa96-115">An Azure account.</span></span> <span data-ttu-id="4aa96-116">Ha az Azure-fiók nem rendelkezik, hozzon létre egy [ingyenes Azure próba-fiókot](http://azure.microsoft.com/pricing/free-trial/) csak néhány perc múlva.</span><span class="sxs-lookup"><span data-stu-id="4aa96-116">If you don't have an Azure account, create a [free Azure trial account](http://azure.microsoft.com/pricing/free-trial/) in just a few minutes.</span></span>
* <span data-ttu-id="4aa96-117">Az Azure parancssori felület telepítve hello kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="4aa96-117">You should have hello Azure CLI installed.</span></span>

## <a name="create-your-iot-hub"></a><span data-ttu-id="4aa96-118">Az IoT hub létrehozása</span><span class="sxs-lookup"><span data-stu-id="4aa96-118">Create your IoT hub</span></span>
<span data-ttu-id="4aa96-119">Azure IoT-központ segítségével csatlakozzon, figyeléséhez és több millió az IoT-eszközök kezelése.</span><span class="sxs-lookup"><span data-stu-id="4aa96-119">Azure IoT Hub helps you connect, monitor, and manage millions of IoT assets.</span></span> <span data-ttu-id="4aa96-120">toocreate az IoT hub, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="4aa96-120">toocreate your IoT hub, follow these steps:</span></span>

1. <span data-ttu-id="4aa96-121">Jelentkezzen be tooyour Azure-fiók hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="4aa96-121">Sign in tooyour Azure account by running hello following command:</span></span>

   ```bash
   az login
   ```

   <span data-ttu-id="4aa96-122">Az elérhető előfizetések sikeres bejelentkezés után vannak felsorolva.</span><span class="sxs-lookup"><span data-stu-id="4aa96-122">All your available subscriptions are listed after a successful sign-in.</span></span>

2. <span data-ttu-id="4aa96-123">Állítsa be a hello alapértelmezett előfizetést, amelyet az toouse hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="4aa96-123">Set hello default subscription that you want toouse by running hello following command:</span></span>

   ```bash
   az account set --subscription {subscription id or name}
   ```

   <span data-ttu-id="4aa96-124">`subscription ID or name`Itt található: hello hello kimenete `az login` vagy hello `az account list` parancsot.</span><span class="sxs-lookup"><span data-stu-id="4aa96-124">`subscription ID or name` can be found in hello output of hello `az login` or hello `az account list` command.</span></span>

3. <span data-ttu-id="4aa96-125">Hello szolgáltató regisztrálása hello a következő parancs futtatásával.</span><span class="sxs-lookup"><span data-stu-id="4aa96-125">Register hello provider by running hello following command.</span></span> <span data-ttu-id="4aa96-126">Erőforrás-szolgáltató a szolgáltatások, hogy biztosít erőforrásokat az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="4aa96-126">Resource providers are services that provide resources for your application.</span></span> <span data-ttu-id="4aa96-127">Hello szolgáltató ajánlatok hello Azure-erőforrás telepítése előtt regisztrálnia kell az hello szolgáltató.</span><span class="sxs-lookup"><span data-stu-id="4aa96-127">You must register hello provider before you can deploy hello Azure resource that hello provider offers.</span></span>

   ```bash
   az provider register -n "Microsoft.Devices"
   ```
4. <span data-ttu-id="4aa96-128">Hozzon létre egy erőforráscsoportot nevű iot-minta hello USA nyugati régiója régióban hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="4aa96-128">Create a resource group named iot-sample in hello West US region by running hello following command:</span></span>

   ```bash
   az group create --name iot-sample --location westus
   ```

   <span data-ttu-id="4aa96-129">`westus`az hello hely, az erőforráscsoport létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="4aa96-129">`westus` is hello location you create your resource group.</span></span> <span data-ttu-id="4aa96-130">Ha máshová szeretné toouse, futtathatja `az account list-locations -o table` toosee összes hello Azure támogatja helyét.</span><span class="sxs-lookup"><span data-stu-id="4aa96-130">If you want toouse another location, you can run `az account list-locations -o table` toosee all hello locations Azure supports.</span></span>

5. <span data-ttu-id="4aa96-131">IoT hub létrehozása hello iot-minta erőforráscsoportban hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="4aa96-131">Create an IoT hub in hello iot-sample resource group by running hello following command:</span></span>

   ```bash
   az iot hub create --name {my hub name} --resource-group iot-sample
   ```

<span data-ttu-id="4aa96-132">Alapértelmezés szerint hello létrehoz egy IoT-központot hello ingyenes tarifacsomag.</span><span class="sxs-lookup"><span data-stu-id="4aa96-132">By default, hello tool creates an IoT Hub in hello Free pricing tier.</span></span> <span data-ttu-id="4aa96-133">További információkért lásd: [Azure IoT Hub árképzési](https://azure.microsoft.com/pricing/details/iot-hub/).</span><span class="sxs-lookup"><span data-stu-id="4aa96-133">For more infomation, see [Azure IoT Hub pricing](https://azure.microsoft.com/pricing/details/iot-hub/).</span></span>

> [!NOTE] 
> <span data-ttu-id="4aa96-134">az IoT hub hello nevének globálisan egyedi kell lennie.</span><span class="sxs-lookup"><span data-stu-id="4aa96-134">hello name of your IoT hub must be globally unique.</span></span>
> <span data-ttu-id="4aa96-135">Az Azure-előfizetéshez tartozó Azure IoT Hub kiadása csak egy F1 hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="4aa96-135">You can create only one F1 edition of Azure IoT Hub under your Azure subscription.</span></span>


## <a name="register-edison-in-your-iot-hub"></a><span data-ttu-id="4aa96-136">Az IoT hub Edison regisztrálása</span><span class="sxs-lookup"><span data-stu-id="4aa96-136">Register Edison in your IoT hub</span></span>
<span data-ttu-id="4aa96-137">Minden eszköz, amely üzeneteket küld üzenetek tooyour IoT-központot, illetve üzeneteket fogad az IoT hub regisztrálni kell egy egyedi azonosítót.</span><span class="sxs-lookup"><span data-stu-id="4aa96-137">Each device that sends messages tooyour IoT hub and receives messages from your IoT hub must be registered with a unique ID.</span></span>

<span data-ttu-id="4aa96-138">Az IoT hub Edison regisztrálása fut a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="4aa96-138">Register Edison in your IoT hub by running following command:</span></span>

```bash
az iot device create --device-id myinteledison --hub-name {my hub name}
```

## <a name="summary"></a><span data-ttu-id="4aa96-139">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="4aa96-139">Summary</span></span>
<span data-ttu-id="4aa96-140">Az IoT-központ elkészítette és Edison regisztrálva az IoT hub eszköz megadásával.</span><span class="sxs-lookup"><span data-stu-id="4aa96-140">You've created an IoT hub and registered Edison with a device identity in your IoT hub.</span></span> <span data-ttu-id="4aa96-141">Most készen áll a toolearn hogyan a toosend Edison tooyour IoT-központ érkező üzenetek.</span><span class="sxs-lookup"><span data-stu-id="4aa96-141">You're ready toolearn how toosend messages from Edison tooyour IoT hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4aa96-142">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4aa96-142">Next steps</span></span>
<span data-ttu-id="4aa96-143">[Hozzon létre egy Azure függvény alkalmazás és az Azure Storage fiók tooprocess és a tároló IoT-központ üzenetek][process-and-store-iot-hub-messages].</span><span class="sxs-lookup"><span data-stu-id="4aa96-143">[Create an Azure function app and an Azure Storage account tooprocess and store IoT hub messages][process-and-store-iot-hub-messages].</span></span>


<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[process-and-store-iot-hub-messages]: iot-hub-intel-edison-kit-node-lesson3-deploy-resource-manager-template.md