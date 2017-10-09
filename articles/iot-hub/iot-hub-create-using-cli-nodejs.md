---
title: "az IoT-központ Azure CLI-vel (azure.js) aaaCreate |} Microsoft Docs"
description: "Hogyan egy Azure IoT hub használatával toocreate hello platformfüggetlen Azure CLI (azure.js)."
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
ms.openlocfilehash: c2a7ea98500b0a0e55a39f4cdfea4605c92add94
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-iot-hub-using-hello-azure-cli"></a><span data-ttu-id="8d687-103">Létrehoz egy IoT-központot hello Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="8d687-103">Create an IoT hub using hello Azure CLI</span></span>

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a><span data-ttu-id="8d687-104">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="8d687-104">Introduction</span></span>

<span data-ttu-id="8d687-105">Használhatja az Azure parancssori felület (azure.js) toocreate és Azure IoT-központok programozott kezelését.</span><span class="sxs-lookup"><span data-stu-id="8d687-105">You can use Azure CLI (azure.js) toocreate and manage Azure IoT hubs programmatically.</span></span> <span data-ttu-id="8d687-106">Ez a cikk bemutatja, hogyan toouse hello Azure CLI (azure.js) toocreate egy IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="8d687-106">This article shows you how toouse hello Azure CLI (azure.js) toocreate an IoT hub.</span></span>

<span data-ttu-id="8d687-107">Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre:</span><span class="sxs-lookup"><span data-stu-id="8d687-107">You can complete hello task using one of hello following CLI versions:</span></span>

* <span data-ttu-id="8d687-108">Az Azure CLI (azure.js) – hello CLI hello klasszikus és resource management üzembe helyezési modellel ebben a cikkben leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="8d687-108">Azure CLI (azure.js) – hello CLI for hello classic and resource management deployment models as described in this article.</span></span>
* <span data-ttu-id="8d687-109">[Az Azure CLI 2.0 (az.py)](iot-hub-create-using-cli.md) -hello következő generációs CLI hello erőforrás management üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="8d687-109">[Azure CLI 2.0 (az.py)](iot-hub-create-using-cli.md) - hello next generation CLI for hello resource management deployment model.</span></span>

<span data-ttu-id="8d687-110">toocomplete ebben az oktatóanyagban a következő hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="8d687-110">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="8d687-111">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="8d687-111">An active Azure account.</span></span> <span data-ttu-id="8d687-112">Ha nincs fiókja, néhány perc alatt létrehozhat egy [ingyenes fiókot][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="8d687-112">If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.</span></span>
* <span data-ttu-id="8d687-113">[Az Azure CLI 0.10.4] [ lnk-CLI-install] vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="8d687-113">[Azure CLI 0.10.4][lnk-CLI-install] or later.</span></span> <span data-ttu-id="8d687-114">Ha már van telepítve az Azure parancssori felület hello, ellenőrzéséhez hello verziószámának hello parancssorban hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="8d687-114">If you already have hello Azure CLI installed, you can validate hello current version at hello command prompt with hello following command:</span></span>

```azurecli
azure --version
```

> [!NOTE]
> <span data-ttu-id="8d687-115">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Azure Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="8d687-115">Azure has two different deployment models for creating and working with resources:  [Azure Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="8d687-116">hello Azure CLI Azure Resource Manager módban kell lennie:</span><span class="sxs-lookup"><span data-stu-id="8d687-116">hello Azure CLI must be in Azure Resource Manager mode:</span></span>
>
> ```azurecli
> azure config mode arm
> ```

## <a name="set-your-azure-account-and-subscription"></a><span data-ttu-id="8d687-117">Az Azure-fiók és -előfizetés beállítása</span><span class="sxs-lookup"><span data-stu-id="8d687-117">Set your Azure account and subscription</span></span>

1. <span data-ttu-id="8d687-118">Hello parancssorba írja be a bejelentkezési hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="8d687-118">At hello command prompt, login by typing hello following command:</span></span>

   ```azurecli
    azure login
   ```

   <span data-ttu-id="8d687-119">Hello javasolt webböngésző és kód tooauthenticate használja.</span><span class="sxs-lookup"><span data-stu-id="8d687-119">Use hello suggested web browser and code tooauthenticate.</span></span>
1. <span data-ttu-id="8d687-120">Ha több Azure-előfizetéssel rendelkezik, csatlakozás tooAzure hozzáférést biztosít tooall hello Azure-előfizetéssel társított hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="8d687-120">If you have multiple Azure subscriptions, connecting tooAzure grants you access tooall hello Azure subscriptions associated with your credentials.</span></span> <span data-ttu-id="8d687-121">Megtekintheti hello Azure-előfizetések, és azok azonosítása, mely még hello alapértelmezés szerint hello parancs használatával:</span><span class="sxs-lookup"><span data-stu-id="8d687-121">You can view hello Azure subscriptions, and identify which one is hello default, using hello command:</span></span>

   ```azurecli
    azure account list
   ```

   <span data-ttu-id="8d687-122">tooset hello előfizetési kontextust kérünk toorun hello többi hello parancsokat használja:</span><span class="sxs-lookup"><span data-stu-id="8d687-122">tooset hello subscription context under which you want toorun hello rest of hello commands use:</span></span>

   ```azurecli
    azure account set <subscription name>
   ```

1. <span data-ttu-id="8d687-123">Ha még nem rendelkezik egy erőforráscsoport, létrehozhat egy nevű **exampleResourceGroup**:</span><span class="sxs-lookup"><span data-stu-id="8d687-123">If you do not have a resource group, you can create one named **exampleResourceGroup**:</span></span>

   ```azurecli
    azure group create -n exampleResourceGroup -l westus
   ```

> [!TIP]
> <span data-ttu-id="8d687-124">hello cikk [használja hello Azure CLI toomanage Azure erőforráscsoport-sablonok és erőforrások] [ lnk-CLI-arm] hogyan toouse hello Azure CLI toomanage Azure további információt nyújt erőforrások.</span><span class="sxs-lookup"><span data-stu-id="8d687-124">hello article [Use hello Azure CLI toomanage Azure resources and resource groups][lnk-CLI-arm] provides more information about how toouse hello Azure CLI toomanage Azure resources.</span></span>

## <a name="create-an-iot-hub"></a><span data-ttu-id="8d687-125">IoT Hub létrehozása</span><span class="sxs-lookup"><span data-stu-id="8d687-125">Create an IoT Hub</span></span>

<span data-ttu-id="8d687-126">Kötelező paraméter:</span><span class="sxs-lookup"><span data-stu-id="8d687-126">Required parameters:</span></span>

```azurecli
azure iothub create -g <resource-group> -n <name> -l <location> -s <sku-name> -u <units>
```

* <span data-ttu-id="8d687-127">**Erőforráscsoport**.</span><span class="sxs-lookup"><span data-stu-id="8d687-127">**resource-group**.</span></span> <span data-ttu-id="8d687-128">hello erőforráscsoport-név.</span><span class="sxs-lookup"><span data-stu-id="8d687-128">hello resource group name.</span></span> <span data-ttu-id="8d687-129">kis-és nagybetűket alfanumerikus, aláhúzásjelet, és kötőjelet tartalmazhat, 1-64 hossza hello formátuma.</span><span class="sxs-lookup"><span data-stu-id="8d687-129">hello format is case insensitive alphanumeric, underscore, and hyphen, 1-64 length.</span></span>
* <span data-ttu-id="8d687-130">**Név**</span><span class="sxs-lookup"><span data-stu-id="8d687-130">**name**.</span></span> <span data-ttu-id="8d687-131">hello IoT hub toobe létrehozott hello neve.</span><span class="sxs-lookup"><span data-stu-id="8d687-131">hello name of hello IoT hub toobe created.</span></span> <span data-ttu-id="8d687-132">kis-és nagybetűket alfanumerikus, aláhúzásjelet, és kötőjelet tartalmazhat, hossza 3 – 50 hello formátuma.</span><span class="sxs-lookup"><span data-stu-id="8d687-132">hello format is case insensitive alphanumeric, underscore, and hyphen, 3-50 length.</span></span>
* <span data-ttu-id="8d687-133">**hely**.</span><span class="sxs-lookup"><span data-stu-id="8d687-133">**location**.</span></span> <span data-ttu-id="8d687-134">hello helye (azure régió/adatközpont) tooprovision hello IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="8d687-134">hello location (azure region/datacenter) tooprovision hello IoT hub.</span></span>
* <span data-ttu-id="8d687-135">**Termékváltozat**.</span><span class="sxs-lookup"><span data-stu-id="8d687-135">**sku-name**.</span></span> <span data-ttu-id="8d687-136">hello sku, egyik hello neve: [F1, S1, S2, S3].</span><span class="sxs-lookup"><span data-stu-id="8d687-136">hello name of hello sku, one of: [F1, S1, S2, S3].</span></span> <span data-ttu-id="8d687-137">Hello legújabb teljes listáját tekintse meg a toohello IoT hub árképzést ismertető oldalra.</span><span class="sxs-lookup"><span data-stu-id="8d687-137">For hello latest full list, refer toohello pricing page for IoT Hub.</span></span>
* <span data-ttu-id="8d687-138">**egységek**.</span><span class="sxs-lookup"><span data-stu-id="8d687-138">**units**.</span></span> <span data-ttu-id="8d687-139">hello kiosztott egységek száma.</span><span class="sxs-lookup"><span data-stu-id="8d687-139">hello number of provisioned units.</span></span> <span data-ttu-id="8d687-140">Tartományon: [1-1] F1: S1, S2 [1-200]: [1 – 10] S3.</span><span class="sxs-lookup"><span data-stu-id="8d687-140">Range: F1 [1-1] : S1, S2 [1-200] : S3 [1-10].</span></span> <span data-ttu-id="8d687-141">IoT Hub-egységek alapuló tooconnect szeretné az összes üzenet számán és hello eszközök száma.</span><span class="sxs-lookup"><span data-stu-id="8d687-141">IoT Hub units are based on your total message count and hello number of devices you want tooconnect.</span></span>

[!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]

<span data-ttu-id="8d687-142">toosee összes hello létrehozásához rendelkezésre álló paramétereket, használhatja a hello súgó parancsot a parancssorba:</span><span class="sxs-lookup"><span data-stu-id="8d687-142">toosee all hello parameters available for creation, you can use hello help command in command prompt:</span></span>

```azurecli
azure iothub create -h
```

<span data-ttu-id="8d687-143">Gyors példa: az IoT-központ nevű toocreate **exampleIoTHubName** hello erőforráscsoportban **exampleResourceGroup**- ben futtassa hello következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="8d687-143">Quick example: toocreate an IoT Hub called **exampleIoTHubName** in hello resource group **exampleResourceGroup**, run hello following command:</span></span>

```azurecli
azure iothub create -g exampleResourceGroup -n exampleIoTHubName -l westus -k s1 -u 1
```

> [!NOTE]
> <span data-ttu-id="8d687-144">Az Azure parancssori felület létrehozza az S1 Standard IoT-központ, amelynek kell fizetni.</span><span class="sxs-lookup"><span data-stu-id="8d687-144">This Azure CLI command creates an S1 Standard IoT Hub for which you are billed.</span></span> <span data-ttu-id="8d687-145">Törölheti a hello IoT-központ **exampleIoTHubName** használatával a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="8d687-145">You can delete hello IoT hub **exampleIoTHubName** using following command:</span></span>
>
> ```azurecli
> azure iothub delete -g exampleResourceGroup -n exampleIoTHubName
> ```

## <a name="next-steps"></a><span data-ttu-id="8d687-146">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8d687-146">Next steps</span></span>

<span data-ttu-id="8d687-147">IoT Hub, fejlesztésével kapcsolatos további toolearn lásd: hello a következő cikket:</span><span class="sxs-lookup"><span data-stu-id="8d687-147">toolearn more about developing for IoT Hub, see hello following article:</span></span>

* <span data-ttu-id="8d687-148">[Az IoT-SDK][lnk-sdks]</span><span class="sxs-lookup"><span data-stu-id="8d687-148">[IoT SDKs][lnk-sdks]</span></span>

<span data-ttu-id="8d687-149">toofurther megismerkedhet az IoT-központ hello képességeit, lásd:</span><span class="sxs-lookup"><span data-stu-id="8d687-149">toofurther explore hello capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="8d687-150">[Az Azure portál toomanage IoT-központ hello használata][lnk-portal]</span><span class="sxs-lookup"><span data-stu-id="8d687-150">[Using hello Azure portal toomanage IoT Hub][lnk-portal]</span></span>

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-azure-portal]: https://portal.azure.com/
[lnk-status]: https://azure.microsoft.com/status/
[lnk-CLI-install]:../cli-install-nodejs.md
[lnk-rest-api]: https://docs.microsoft.com/rest/api/iothub/iothubresource
[lnk-CLI-arm]: ../azure-resource-manager/xplat-cli-azure-resource-manager.md

[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-portal]: iot-hub-create-through-portal.md 
