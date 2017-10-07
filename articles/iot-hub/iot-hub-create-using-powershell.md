---
title: "egy Azure IoT-központot egy PowerShell-parancsmaggal aaaCreate |} Microsoft Docs"
description: "Hogyan toouse egy PowerShell-parancsmag toocreate egy IoT-központot."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: 005cd8d48eb39d2e8b1bfb9ef84330d4aae4658f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-iot-hub-using-hello-new-azurermiothub-cmdlet"></a><span data-ttu-id="717f6-103">Új-AzureRmIotHub hello parancsmaggal IoT hub létrehozása</span><span class="sxs-lookup"><span data-stu-id="717f6-103">Create an IoT hub using hello New-AzureRmIotHub cmdlet</span></span>

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a><span data-ttu-id="717f6-104">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="717f6-104">Introduction</span></span>

<span data-ttu-id="717f6-105">Használhatja az Azure PowerShell-parancsmagok toocreate és Azure IoT-központok kezelése.</span><span class="sxs-lookup"><span data-stu-id="717f6-105">You can use Azure PowerShell cmdlets toocreate and manage Azure IoT hubs.</span></span> <span data-ttu-id="717f6-106">Az oktatóanyag bemutatja, hogyan toocreate az IoT-központ a PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="717f6-106">This tutorial shows you how toocreate an IoT hub with PowerShell.</span></span>

> [!NOTE]
> <span data-ttu-id="717f6-107">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Azure Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="717f6-107">Azure has two different deployment models for creating and working with resources: [Azure Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="717f6-108">Ez a cikk hello Azure Resource Manager telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="717f6-108">This article covers using hello Azure Resource Manager deployment model.</span></span>

<span data-ttu-id="717f6-109">toocomplete ebben az oktatóanyagban a következő hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="717f6-109">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="717f6-110">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="717f6-110">An active Azure account.</span></span> <br/><span data-ttu-id="717f6-111">Ha nincs fiókja, néhány perc alatt létrehozhat egy [ingyenes fiókot][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="717f6-111">If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.</span></span>
* <span data-ttu-id="717f6-112">[Az Azure PowerShell-parancsmagok][lnk-powershell-install].</span><span class="sxs-lookup"><span data-stu-id="717f6-112">[Azure PowerShell cmdlets][lnk-powershell-install].</span></span>

## <a name="connect-tooyour-azure-subscription"></a><span data-ttu-id="717f6-113">Csatlakozás Azure-előfizetés tooyour</span><span class="sxs-lookup"><span data-stu-id="717f6-113">Connect tooyour Azure subscription</span></span>
<span data-ttu-id="717f6-114">A PowerShell-parancssort írja be a hello parancs toosign tooyour Azure-előfizetés a következő:</span><span class="sxs-lookup"><span data-stu-id="717f6-114">In a PowerShell command prompt, enter hello following command toosign in tooyour Azure subscription:</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="717f6-115">Ha több Azure-előfizetéssel rendelkezik, birtokában tooall hozzáférhet tooAzure bejelentkezés hello Azure-előfizetéssel társított hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="717f6-115">If you have multiple Azure subscriptions, signing in tooAzure grants you access tooall hello Azure subscriptions associated with your credentials.</span></span> <span data-ttu-id="717f6-116">A következő parancs toolist hello Azure-előfizetések elérhető az Ön toouse hello használata:</span><span class="sxs-lookup"><span data-stu-id="717f6-116">Use hello following command toolist hello Azure subscriptions available for you toouse:</span></span>

```powershell
Get-AzureRMSubscription
```

<span data-ttu-id="717f6-117">A következő parancs tooselect előfizetést, amelyet toouse toorun hello parancsok toocreate az IoT hub hello használata.</span><span class="sxs-lookup"><span data-stu-id="717f6-117">Use hello following command tooselect subscription that you want toouse toorun hello commands toocreate your IoT hub.</span></span> <span data-ttu-id="717f6-118">Hello előfizetés név vagy azonosító hello kimenetből hello előző parancs használható:</span><span class="sxs-lookup"><span data-stu-id="717f6-118">You can use either hello subscription name or ID from hello output of hello previous command:</span></span>

```powershell
Select-AzureRMSubscription `
    -SubscriptionName "{your subscription name}"
```

## <a name="create-resource-group"></a><span data-ttu-id="717f6-119">Erőforráscsoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="717f6-119">Create resource group</span></span>

<span data-ttu-id="717f6-120">Egy erőforrás csoport toodeploy az IoT-központ van szüksége.</span><span class="sxs-lookup"><span data-stu-id="717f6-120">You need a resource group toodeploy an IoT hub.</span></span> <span data-ttu-id="717f6-121">Használjon egy meglévő erőforráscsoportot, vagy hozzon létre egy újat.</span><span class="sxs-lookup"><span data-stu-id="717f6-121">You can use an existing resource group or create a new one.</span></span>

<span data-ttu-id="717f6-122">Használhatja az alábbi parancs toodiscover hello helyek telepíthető az IoT-központ hello:</span><span class="sxs-lookup"><span data-stu-id="717f6-122">You can use hello following command toodiscover hello locations where you can deploy an IoT hub:</span></span>

```powershell
((Get-AzureRmResourceProvider `
  -ProviderNamespace Microsoft.Devices).ResourceTypes `
  | Where-Object ResourceTypeName -eq IoTHubs).Locations
```

<span data-ttu-id="717f6-123">az IoT hub az IoT-központ, a következő parancs használata hello hello támogatott helyek egyikén erőforráscsoport toocreate.</span><span class="sxs-lookup"><span data-stu-id="717f6-123">toocreate a resource group for your IoT hub in one of hello supported locations for IoT Hub, use hello following command.</span></span> <span data-ttu-id="717f6-124">Ebben a példában egy nevű erőforráscsoportot hoz létre **MyIoTRG1** a hello **USA keleti régiója** régió:</span><span class="sxs-lookup"><span data-stu-id="717f6-124">This example creates a resource group called **MyIoTRG1** in hello **East US** region:</span></span>

```powershell
New-AzureRmResourceGroup -Name MyIoTRG1 -Location "East US"
```

## <a name="create-an-iot-hub"></a><span data-ttu-id="717f6-125">IoT Hub létrehozása</span><span class="sxs-lookup"><span data-stu-id="717f6-125">Create an IoT hub</span></span>

<span data-ttu-id="717f6-126">az IoT-központ hello előző lépésben, a következő parancs használata hello létrehozott hello erőforráscsoportban toocreate.</span><span class="sxs-lookup"><span data-stu-id="717f6-126">toocreate an IoT hub in hello resource group you created in hello previous step, use hello following command.</span></span> <span data-ttu-id="717f6-127">Ez a példa létrehoz egy **S1** nevű hub **MyTestIoTHub** a hello **USA keleti régiója** régió:</span><span class="sxs-lookup"><span data-stu-id="717f6-127">This example creates an **S1** hub called **MyTestIoTHub** in hello **East US** region:</span></span>

```powershell
New-AzureRmIotHub `
    -ResourceGroupName MyIoTRG1 `
    -Name MyTestIoTHub `
    -SkuName S1 -Units 1 `
    -Location "East US"
```

<span data-ttu-id="717f6-128">az IoT-központ hello hello nevének egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="717f6-128">hello name of hello IoT hub must be unique.</span></span>

[!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]


<span data-ttu-id="717f6-129">Minden hello IoT-központok az előfizetéshez a következő parancs hello segítségével jeleníthetők meg:</span><span class="sxs-lookup"><span data-stu-id="717f6-129">You can list all hello IoT hubs in your subscription using hello following command:</span></span>

```powershell
Get-AzureRmIotHub
```

<span data-ttu-id="717f6-130">hello előző példa ad hozzá egy S1 Standard IoT-központot, amelynek kell fizetni.</span><span class="sxs-lookup"><span data-stu-id="717f6-130">hello previous example adds an S1 Standard IoT Hub for which you are billed.</span></span> <span data-ttu-id="717f6-131">Törölheti a hello IoT-központ hello a következő parancs használatával:</span><span class="sxs-lookup"><span data-stu-id="717f6-131">You can delete hello IoT hub using hello following command:</span></span>

```powershell
Remove-AzureRmIotHub `
    -ResourceGroupName MyIoTRG1 `
    -Name MyTestIoTHub
```

<span data-ttu-id="717f6-132">Eltávolíthatja az egy erőforráscsoport, és minden hello erőforrásokat tartalmaz, a következő parancs hello használata:</span><span class="sxs-lookup"><span data-stu-id="717f6-132">Alternatively, you can remove a resource group and all hello resources it contains using hello following command:</span></span>

```powershell
Remove-AzureRmResourceGroup -Name MyIoTRG1
```

## <a name="next-steps"></a><span data-ttu-id="717f6-133">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="717f6-133">Next steps</span></span>

<span data-ttu-id="717f6-134">Most egy IoT-központot egy PowerShell-parancsmag használatával telepített, akkor érdemes lehet további tooexplore:</span><span class="sxs-lookup"><span data-stu-id="717f6-134">Now you have deployed an IoT hub using a PowerShell cmdlet, you may want tooexplore further:</span></span>

* <span data-ttu-id="717f6-135">Fedezze fel más [PowerShell-parancsmagok használata az IoT hub][lnk-iothub-cmdlets].</span><span class="sxs-lookup"><span data-stu-id="717f6-135">Discover other [PowerShell cmdlets for working with your IoT hub][lnk-iothub-cmdlets].</span></span>
* <span data-ttu-id="717f6-136">További információ a hello hello képességeit [IoT-központ erőforrás-szolgáltató REST API][lnk-rest-api].</span><span class="sxs-lookup"><span data-stu-id="717f6-136">Read about hello capabilities of hello [IoT Hub resource provider REST API][lnk-rest-api].</span></span>

<span data-ttu-id="717f6-137">toolearn IoT-központot, fejlesztésével kapcsolatos további hello a következő cikkekben talál:</span><span class="sxs-lookup"><span data-stu-id="717f6-137">toolearn more about developing for IoT Hub, see hello following articles:</span></span>

* <span data-ttu-id="717f6-138">[Bevezetés tooC SDK][lnk-c-sdk]</span><span class="sxs-lookup"><span data-stu-id="717f6-138">[Introduction tooC SDK][lnk-c-sdk]</span></span>
* <span data-ttu-id="717f6-139">[Az Azure IoT SDK-k][lnk-sdks]</span><span class="sxs-lookup"><span data-stu-id="717f6-139">[Azure IoT SDKs][lnk-sdks]</span></span>

<span data-ttu-id="717f6-140">toofurther megismerkedhet az IoT-központ hello képességeit, lásd:</span><span class="sxs-lookup"><span data-stu-id="717f6-140">toofurther explore hello capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="717f6-141">[Egy eszköz szimulálva IoT oldala][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="717f6-141">[Simulating a device with IoT Edge][lnk-iotedge]</span></span>

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-powershell-install]: https://docs.microsoft.com/powershell/azure/install-azurerm-ps
[lnk-iothub-cmdlets]: https://docs.microsoft.com/powershell/module/azurerm.iothub/
[lnk-rest-api]: https://docs.microsoft.com/rest/api/iothub/iothubresource

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
