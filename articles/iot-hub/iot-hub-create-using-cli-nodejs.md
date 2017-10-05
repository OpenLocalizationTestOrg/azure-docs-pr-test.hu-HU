---
title: "Létrehoz egy IoT-központot Azure CLI-vel (azure.js) |} Microsoft Docs"
description: "Tudnivalók az Azure IoT-központ a platformok közötti Azure parancssori felület (azure.js) használatával."
services: iot-hub
documentationcenter: .net
author: BeatriceOltean
manager: timlt
editor: 
ms.assetid: 46a17831-650c-41d9-b228-445c5bb423d3
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/04/2017
ms.author: boltean
ms.openlocfilehash: 5e37c6c5e8625ce446ab203f19f9a8b2f1cd5a46
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="create-an-iot-hub-using-the-azure-cli"></a><span data-ttu-id="5bce0-103">Létrehoz egy IoT-központot, az Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="5bce0-103">Create an IoT hub using the Azure CLI</span></span>

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a><span data-ttu-id="5bce0-104">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="5bce0-104">Introduction</span></span>

<span data-ttu-id="5bce0-105">Azure parancssori felület (azure.js) létrehozását és kezelését a Azure IoT-központok programozott módon használhatja.</span><span class="sxs-lookup"><span data-stu-id="5bce0-105">You can use Azure CLI (azure.js) to create and manage Azure IoT hubs programmatically.</span></span> <span data-ttu-id="5bce0-106">Ez a cikk bemutatja, hogyan használható az Azure parancssori felület (azure.js) létrehoz egy IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="5bce0-106">This article shows you how to use the Azure CLI (azure.js) to create an IoT hub.</span></span>

<span data-ttu-id="5bce0-107">A következő CLI-verziók egyikével elvégezheti a feladatot:</span><span class="sxs-lookup"><span data-stu-id="5bce0-107">You can complete the task using one of the following CLI versions:</span></span>

* <span data-ttu-id="5bce0-108">Az Azure CLI (azure.js) – a parancssori felületen a klasszikus és resource management üzembe helyezési modellel ebben a cikkben leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="5bce0-108">Azure CLI (azure.js) – the CLI for the classic and resource management deployment models as described in this article.</span></span>
* <span data-ttu-id="5bce0-109">[Az Azure CLI 2.0 (az.py)](iot-hub-create-using-cli.md) - CLI következő generációs erőforrás felügyeleti telepítési modell.</span><span class="sxs-lookup"><span data-stu-id="5bce0-109">[Azure CLI 2.0 (az.py)](iot-hub-create-using-cli.md) - the next generation CLI for the resource management deployment model.</span></span>

<span data-ttu-id="5bce0-110">Az oktatóanyag teljesítéséhez a következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="5bce0-110">To complete this tutorial, you need the following:</span></span>

* <span data-ttu-id="5bce0-111">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="5bce0-111">An active Azure account.</span></span> <span data-ttu-id="5bce0-112">Ha nincs fiókja, néhány perc alatt létrehozhat egy [ingyenes fiókot][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="5bce0-112">If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.</span></span>
* <span data-ttu-id="5bce0-113">[Az Azure CLI 0.10.4] [ lnk-CLI-install] vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="5bce0-113">[Azure CLI 0.10.4][lnk-CLI-install] or later.</span></span> <span data-ttu-id="5bce0-114">Ha már rendelkezik az Azure parancssori felület telepítve, az aktuális verzió parancsot a parancssorba a következő paranccsal ellenőrizheti:</span><span class="sxs-lookup"><span data-stu-id="5bce0-114">If you already have the Azure CLI installed, you can validate the current version at the command prompt with the following command:</span></span>

```azurecli
azure --version
```

> [!NOTE]
> <span data-ttu-id="5bce0-115">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Azure Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="5bce0-115">Azure has two different deployment models for creating and working with resources:  [Azure Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="5bce0-116">Az Azure CLI Azure Resource Manager módban kell lennie:</span><span class="sxs-lookup"><span data-stu-id="5bce0-116">The Azure CLI must be in Azure Resource Manager mode:</span></span>
>
> ```azurecli
> azure config mode arm
> ```

## <a name="set-your-azure-account-and-subscription"></a><span data-ttu-id="5bce0-117">Az Azure-fiók és -előfizetés beállítása</span><span class="sxs-lookup"><span data-stu-id="5bce0-117">Set your Azure account and subscription</span></span>

1. <span data-ttu-id="5bce0-118">A parancssorba a következő parancs beírásával bejelentkezési azonosító:</span><span class="sxs-lookup"><span data-stu-id="5bce0-118">At the command prompt, login by typing the following command:</span></span>

   ```azurecli
    azure login
   ```

   <span data-ttu-id="5bce0-119">A javasolt webböngésző és a kód segítségével hitelesíteni.</span><span class="sxs-lookup"><span data-stu-id="5bce0-119">Use the suggested web browser and code to authenticate.</span></span>
1. <span data-ttu-id="5bce0-120">Ha több Azure-előfizetéssel rendelkezik, a csatlakoztatása az Azure ad hozzáférést az összes Azure-előfizetést a hitelesítő adatok társított.</span><span class="sxs-lookup"><span data-stu-id="5bce0-120">If you have multiple Azure subscriptions, connecting to Azure grants you access to all the Azure subscriptions associated with your credentials.</span></span> <span data-ttu-id="5bce0-121">Az Azure-előfizetések megtekintése és részre, ahol meghatározhatja azt, amelyiket a alapértelmezés szerint a parancs segítségével:</span><span class="sxs-lookup"><span data-stu-id="5bce0-121">You can view the Azure subscriptions, and identify which one is the default, using the command:</span></span>

   ```azurecli
    azure account list
   ```

   <span data-ttu-id="5bce0-122">Az előfizetési kontextust, amely alatt a parancsok használata a többi futtatni kívánt beállítása:</span><span class="sxs-lookup"><span data-stu-id="5bce0-122">To set the subscription context under which you want to run the rest of the commands use:</span></span>

   ```azurecli
    azure account set <subscription name>
   ```

1. <span data-ttu-id="5bce0-123">Ha még nem rendelkezik egy erőforráscsoport, létrehozhat egy nevű **exampleResourceGroup**:</span><span class="sxs-lookup"><span data-stu-id="5bce0-123">If you do not have a resource group, you can create one named **exampleResourceGroup**:</span></span>

   ```azurecli
    azure group create -n exampleResourceGroup -l westus
   ```

> [!TIP]
> <span data-ttu-id="5bce0-124">A cikk [Azure-erőforrások és csoportok kezelése az Azure parancssori felület használatával] [ lnk-CLI-arm] Azure-erőforrások kezelése az Azure parancssori felület használatával kapcsolatos további információk.</span><span class="sxs-lookup"><span data-stu-id="5bce0-124">The article [Use the Azure CLI to manage Azure resources and resource groups][lnk-CLI-arm] provides more information about how to use the Azure CLI to manage Azure resources.</span></span>

## <a name="create-an-iot-hub"></a><span data-ttu-id="5bce0-125">IoT Hub létrehozása</span><span class="sxs-lookup"><span data-stu-id="5bce0-125">Create an IoT Hub</span></span>

<span data-ttu-id="5bce0-126">Kötelező paraméter:</span><span class="sxs-lookup"><span data-stu-id="5bce0-126">Required parameters:</span></span>

```azurecli
azure iothub create -g <resource-group> -n <name> -l <location> -s <sku-name> -u <units>
```

* <span data-ttu-id="5bce0-127">**Erőforráscsoport**.</span><span class="sxs-lookup"><span data-stu-id="5bce0-127">**resource-group**.</span></span> <span data-ttu-id="5bce0-128">Az erőforráscsoport neve.</span><span class="sxs-lookup"><span data-stu-id="5bce0-128">The resource group name.</span></span> <span data-ttu-id="5bce0-129">A kis-és nagybetűket alfanumerikus, aláhúzásjelet, és kötőjelet tartalmazhat, 1-64 hossza érvénytelen.</span><span class="sxs-lookup"><span data-stu-id="5bce0-129">The format is case insensitive alphanumeric, underscore, and hyphen, 1-64 length.</span></span>
* <span data-ttu-id="5bce0-130">**Név**</span><span class="sxs-lookup"><span data-stu-id="5bce0-130">**name**.</span></span> <span data-ttu-id="5bce0-131">Az IoT hub létrehozni neve.</span><span class="sxs-lookup"><span data-stu-id="5bce0-131">The name of the IoT hub to be created.</span></span> <span data-ttu-id="5bce0-132">A kis-és nagybetűket alfanumerikus, aláhúzásjelet, és kötőjelet tartalmazhat, 3 – 50 hossza érvénytelen.</span><span class="sxs-lookup"><span data-stu-id="5bce0-132">The format is case insensitive alphanumeric, underscore, and hyphen, 3-50 length.</span></span>
* <span data-ttu-id="5bce0-133">**hely**.</span><span class="sxs-lookup"><span data-stu-id="5bce0-133">**location**.</span></span> <span data-ttu-id="5bce0-134">A hely (azure régió/adatközpont) az IoT-központ telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="5bce0-134">The location (azure region/datacenter) to provision the IoT hub.</span></span>
* <span data-ttu-id="5bce0-135">**Termékváltozat**.</span><span class="sxs-lookup"><span data-stu-id="5bce0-135">**sku-name**.</span></span> <span data-ttu-id="5bce0-136">A termékváltozat, egyik neve: [F1, S1, S2, S3].</span><span class="sxs-lookup"><span data-stu-id="5bce0-136">The name of the sku, one of: [F1, S1, S2, S3].</span></span> <span data-ttu-id="5bce0-137">A legújabb teljes listája tekintse meg a tarifákat tartalmazó oldalt az IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="5bce0-137">For the latest full list, refer to the pricing page for IoT Hub.</span></span>
* <span data-ttu-id="5bce0-138">**egységek**.</span><span class="sxs-lookup"><span data-stu-id="5bce0-138">**units**.</span></span> <span data-ttu-id="5bce0-139">A kiépített egységek száma.</span><span class="sxs-lookup"><span data-stu-id="5bce0-139">The number of provisioned units.</span></span> <span data-ttu-id="5bce0-140">Tartományon: [1-1] F1: S1, S2 [1-200]: [1 – 10] S3.</span><span class="sxs-lookup"><span data-stu-id="5bce0-140">Range: F1 [1-1] : S1, S2 [1-200] : S3 [1-10].</span></span> <span data-ttu-id="5bce0-141">IoT Hub-egységek az összes üzenet számán és a, amelyhez csatlakozni eszközök számán alapulnak.</span><span class="sxs-lookup"><span data-stu-id="5bce0-141">IoT Hub units are based on your total message count and the number of devices you want to connect.</span></span>

[!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]

<span data-ttu-id="5bce0-142">Tekintse meg az összes paramétereinek létrehozása, használhatja a help paranccsal parancssorban:</span><span class="sxs-lookup"><span data-stu-id="5bce0-142">To see all the parameters available for creation, you can use the help command in command prompt:</span></span>

```azurecli
azure iothub create -h
```

<span data-ttu-id="5bce0-143">Gyors példa: az IoT-központ létrehozásához nevű **exampleIoTHubName** erőforráscsoportban **exampleResourceGroup**, a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="5bce0-143">Quick example: To create an IoT Hub called **exampleIoTHubName** in the resource group **exampleResourceGroup**, run the following command:</span></span>

```azurecli
azure iothub create -g exampleResourceGroup -n exampleIoTHubName -l westus -k s1 -u 1
```

> [!NOTE]
> <span data-ttu-id="5bce0-144">Az Azure parancssori felület létrehozza az S1 Standard IoT-központ, amelynek kell fizetni.</span><span class="sxs-lookup"><span data-stu-id="5bce0-144">This Azure CLI command creates an S1 Standard IoT Hub for which you are billed.</span></span> <span data-ttu-id="5bce0-145">Az IoT hub törlése **exampleIoTHubName** használatával a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="5bce0-145">You can delete the IoT hub **exampleIoTHubName** using following command:</span></span>
>
> ```azurecli
> azure iothub delete -g exampleResourceGroup -n exampleIoTHubName
> ```

## <a name="next-steps"></a><span data-ttu-id="5bce0-146">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5bce0-146">Next steps</span></span>

<span data-ttu-id="5bce0-147">Az IoT-központ fejlesztésével kapcsolatos további tudnivalókért tekintse meg a következő cikket:</span><span class="sxs-lookup"><span data-stu-id="5bce0-147">To learn more about developing for IoT Hub, see the following article:</span></span>

* <span data-ttu-id="5bce0-148">[Az IoT-SDK][lnk-sdks]</span><span class="sxs-lookup"><span data-stu-id="5bce0-148">[IoT SDKs][lnk-sdks]</span></span>

<span data-ttu-id="5bce0-149">Az IoT-központ képességeit további megismeréséhez lásd:</span><span class="sxs-lookup"><span data-stu-id="5bce0-149">To further explore the capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="5bce0-150">[Az IoT-központ kezelése az Azure portál használatával][lnk-portal]</span><span class="sxs-lookup"><span data-stu-id="5bce0-150">[Using the Azure portal to manage IoT Hub][lnk-portal]</span></span>

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-azure-portal]: https://portal.azure.com/
[lnk-status]: https://azure.microsoft.com/status/
[lnk-CLI-install]:../cli-install-nodejs.md
[lnk-rest-api]: https://docs.microsoft.com/rest/api/iothub/iothubresource
[lnk-CLI-arm]: ../azure-resource-manager/xplat-cli-azure-resource-manager.md

[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-portal]: iot-hub-create-through-portal.md 
