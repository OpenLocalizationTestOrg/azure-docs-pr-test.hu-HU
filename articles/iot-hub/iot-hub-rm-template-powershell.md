---
title: "az Azure IoT-központ sablon (PowerShell) segítségével aaaCreate |} Microsoft Docs"
description: "Hogyan toouse az Azure Resource Manager sablon toocreate az IoT-központ a PowerShell használatával."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 7eade855-c289-4ffb-b5ef-02be8c5f670f
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: e98ff5e898200cd727b9326fb3df393e43b021e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-iot-hub-using-azure-resource-manager-template-powershell"></a><span data-ttu-id="a604c-103">Létrehoz egy IoT-központot, Azure Resource Manager sablonnal (PowerShell)</span><span class="sxs-lookup"><span data-stu-id="a604c-103">Create an IoT hub using Azure Resource Manager template (PowerShell)</span></span>

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

<span data-ttu-id="a604c-104">Használhatja az Azure Resource Manager toocreate és Azure IoT-központok programozott kezelését.</span><span class="sxs-lookup"><span data-stu-id="a604c-104">You can use Azure Resource Manager toocreate and manage Azure IoT hubs programmatically.</span></span> <span data-ttu-id="a604c-105">Az oktatóanyag bemutatja, hogyan toouse az Azure Resource Manager sablon toocreate az IoT-központ a PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="a604c-105">This tutorial shows you how toouse an Azure Resource Manager template toocreate an IoT hub with PowerShell.</span></span>

> [!NOTE]
> <span data-ttu-id="a604c-106">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Azure Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="a604c-106">Azure has two different deployment models for creating and working with resources: [Azure Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="a604c-107">Ez a cikk hello Azure Resource Manager telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="a604c-107">This article covers using hello Azure Resource Manager deployment model.</span></span>

<span data-ttu-id="a604c-108">toocomplete ebben az oktatóanyagban a következő hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="a604c-108">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="a604c-109">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="a604c-109">An active Azure account.</span></span> <br/><span data-ttu-id="a604c-110">Ha nincs fiókja, néhány perc alatt létrehozhat egy [ingyenes fiókot][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="a604c-110">If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.</span></span>
* <span data-ttu-id="a604c-111">[Az Azure PowerShell 1.0] [ lnk-powershell-install] vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="a604c-111">[Azure PowerShell 1.0][lnk-powershell-install] or later.</span></span>

> [!TIP]
> <span data-ttu-id="a604c-112">hello cikk [az Azure PowerShell használata Azure Resource Managerrel] [ lnk-powershell-arm] nyújt további információt toouse PowerShell és az Azure Resource Manager sablonok toocreate Azure erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="a604c-112">hello article [Using Azure PowerShell with Azure Resource Manager][lnk-powershell-arm] provides more information about how toouse PowerShell and Azure Resource Manager templates toocreate Azure resources.</span></span>

## <a name="connect-tooyour-azure-subscription"></a><span data-ttu-id="a604c-113">Csatlakozás Azure-előfizetés tooyour</span><span class="sxs-lookup"><span data-stu-id="a604c-113">Connect tooyour Azure subscription</span></span>

<span data-ttu-id="a604c-114">A PowerShell-parancssort írja be a hello parancs toosign tooyour Azure-előfizetés a következő:</span><span class="sxs-lookup"><span data-stu-id="a604c-114">In a PowerShell command prompt, enter hello following command toosign in tooyour Azure subscription:</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="a604c-115">Ha több Azure-előfizetéssel rendelkezik, birtokában tooall hozzáférhet tooAzure bejelentkezés hello Azure-előfizetéssel társított hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="a604c-115">If you have multiple Azure subscriptions, signing in tooAzure grants you access tooall hello Azure subscriptions associated with your credentials.</span></span> <span data-ttu-id="a604c-116">A következő parancs toolist hello Azure-előfizetések elérhető az Ön toouse hello használata:</span><span class="sxs-lookup"><span data-stu-id="a604c-116">Use hello following command toolist hello Azure subscriptions available for you toouse:</span></span>

```powershell
Get-AzureRMSubscription
```

<span data-ttu-id="a604c-117">A következő parancs tooselect előfizetést, amelyet toouse toorun hello parancsok toocreate az IoT hub hello használata.</span><span class="sxs-lookup"><span data-stu-id="a604c-117">Use hello following command tooselect subscription that you want toouse toorun hello commands toocreate your IoT hub.</span></span> <span data-ttu-id="a604c-118">Hello előfizetés név vagy azonosító hello kimenetből hello előző parancs használható:</span><span class="sxs-lookup"><span data-stu-id="a604c-118">You can use either hello subscription name or ID from hello output of hello previous command:</span></span>

```powershell
Select-AzureRMSubscription `
    -SubscriptionName "{your subscription name}"
```

<span data-ttu-id="a604c-119">A következő parancsok toodiscover, ahol az IoT-központ telepítése és hello jelenleg támogatott API-verziók hello használhatja:</span><span class="sxs-lookup"><span data-stu-id="a604c-119">You can use hello following commands toodiscover where you can deploy an IoT hub and hello currently supported API versions:</span></span>

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Devices).ResourceTypes | Where-Object ResourceTypeName -eq IoTHubs).Locations
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Devices).ResourceTypes | Where-Object ResourceTypeName -eq IoTHubs).ApiVersions
```

<span data-ttu-id="a604c-120">Hozzon létre egy erőforrás csoport toocontain az IoT hub, a következő parancs az IoT-központ hello támogatott helyek egyikén hello segítségével.</span><span class="sxs-lookup"><span data-stu-id="a604c-120">Create a resource group toocontain your IoT hub using hello following command in one of hello supported locations for IoT Hub.</span></span> <span data-ttu-id="a604c-121">Ebben a példában egy nevű erőforráscsoportot hoz létre **MyIoTRG1**:</span><span class="sxs-lookup"><span data-stu-id="a604c-121">This example creates a resource group called **MyIoTRG1**:</span></span>

```powershell
New-AzureRmResourceGroup -Name MyIoTRG1 -Location "East US"
```

## <a name="submit-a-template-toocreate-an-iot-hub"></a><span data-ttu-id="a604c-122">Küldje el a sablon toocreate az IoT-központ</span><span class="sxs-lookup"><span data-stu-id="a604c-122">Submit a template toocreate an IoT hub</span></span>

<span data-ttu-id="a604c-123">A JSON-sablon toocreate az IoT-központ használja az erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="a604c-123">Use a JSON template toocreate an IoT hub in your resource group.</span></span> <span data-ttu-id="a604c-124">Az Azure Resource Manager sablon toomake módosítások tooan meglévő IoT-központ is használható.</span><span class="sxs-lookup"><span data-stu-id="a604c-124">You can also use an Azure Resource Manager template toomake changes tooan existing IoT hub.</span></span>

1. <span data-ttu-id="a604c-125">Használja a szöveg szerkesztő toocreate Azure Resource Manager-sablonok nevű **template.json** a hello erőforrás-definíció toocreate követően egy új, normál IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="a604c-125">Use a text editor toocreate an Azure Resource Manager template called **template.json** with hello following resource definition toocreate a new standard IoT hub.</span></span> <span data-ttu-id="a604c-126">Ebben a példában az IoT-központ hello helyez hello **USA keleti régiója** régió, létrehoz két felhasználói csoportok (**cg1** és **cg2**) hello Event Hub-kompatibilis végpont, és használja hello **2016-02-03** API-verzió.</span><span class="sxs-lookup"><span data-stu-id="a604c-126">This example adds hello IoT Hub in hello **East US** region, creates two consumer groups (**cg1** and **cg2**) on hello Event Hub-compatible endpoint, and uses hello **2016-02-03** API version.</span></span> <span data-ttu-id="a604c-127">Ez a sablon is várja meg toopass az IoT-központnév hello paraméterként nevű **hubName**.</span><span class="sxs-lookup"><span data-stu-id="a604c-127">This template also expects you toopass in hello IoT hub name as a parameter called **hubName**.</span></span> <span data-ttu-id="a604c-128">Hello jelenlegi listája támogatja az IoT Hub-helyeken: [Azure állapot][lnk-status].</span><span class="sxs-lookup"><span data-stu-id="a604c-128">For hello current list of locations that support IoT Hub see [Azure Status][lnk-status].</span></span>

    ```json
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "hubName": {
          "type": "string"
        }
      },
      "resources": [
      {
        "apiVersion": "2016-02-03",
        "type": "Microsoft.Devices/IotHubs",
        "name": "[parameters('hubName')]",
        "location": "East US",
        "sku": {
          "name": "S1",
          "tier": "Standard",
          "capacity": 1
        },
        "properties": {
          "location": "East US"
        }
      },
      {
        "apiVersion": "2016-02-03",
        "type": "Microsoft.Devices/IotHubs/eventhubEndpoints/ConsumerGroups",
        "name": "[concat(parameters('hubName'), '/events/cg1')]",
        "dependsOn": [
          "[concat('Microsoft.Devices/Iothubs/', parameters('hubName'))]"
        ]
      },
      {
        "apiVersion": "2016-02-03",
        "type": "Microsoft.Devices/IotHubs/eventhubEndpoints/ConsumerGroups",
        "name": "[concat(parameters('hubName'), '/events/cg2')]",
        "dependsOn": [
          "[concat('Microsoft.Devices/Iothubs/', parameters('hubName'))]"
        ]
      }
      ],
      "outputs": {
        "hubKeys": {
          "value": "[listKeys(resourceId('Microsoft.Devices/IotHubs', parameters('hubName')), '2016-02-03')]",
          "type": "object"
        }
      }
    }
    ```

2. <span data-ttu-id="a604c-129">Mentse a hello Azure Resource Manager sablon fájlt a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="a604c-129">Save hello Azure Resource Manager template file on your local machine.</span></span> <span data-ttu-id="a604c-130">Ebben a példában feltételezzük, hogy nevű mappába menti **c:\templates**.</span><span class="sxs-lookup"><span data-stu-id="a604c-130">This example assumes you save it in a folder called **c:\templates**.</span></span>

3. <span data-ttu-id="a604c-131">Futtassa a következő parancs toodeploy hello az új IoT hub, amely az IoT hub nevét hello átadott paraméterként.</span><span class="sxs-lookup"><span data-stu-id="a604c-131">Run hello following command toodeploy your new IoT hub, passing hello name of your IoT hub as a parameter.</span></span> <span data-ttu-id="a604c-132">Ebben a példában az IoT-központ hello hello neve nem `abcmyiothub`.</span><span class="sxs-lookup"><span data-stu-id="a604c-132">In this example, hello name of hello IoT hub is `abcmyiothub`.</span></span> <span data-ttu-id="a604c-133">az IoT hub hello nevének globálisan egyedi kell lennie:</span><span class="sxs-lookup"><span data-stu-id="a604c-133">hello name of your IoT hub must be globally unique:</span></span>

    ```powershell
    New-AzureRmResourceGroupDeployment -ResourceGroupName MyIoTRG1 -TemplateFile C:\templates\template.json -hubName abcmyiothub
    ```
  [!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]

4. <span data-ttu-id="a604c-134">hello kimeneti hello kulcsokat hello IoT hub létrehozott jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="a604c-134">hello output displays hello keys for hello IoT hub you created.</span></span>

5. <span data-ttu-id="a604c-135">az alkalmazás hozzáadott tooverify hello új IoT hub, látogasson el hello [Azure-portálon] [ lnk-azure-portal] és tekintse meg az erőforrások listáját.</span><span class="sxs-lookup"><span data-stu-id="a604c-135">tooverify your application added hello new IoT hub, visit hello [Azure portal][lnk-azure-portal] and view your list of resources.</span></span> <span data-ttu-id="a604c-136">Másik megoldásként használhatja a hello **Get-AzureRmResource** PowerShell-parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="a604c-136">Alternatively, use hello **Get-AzureRmResource** PowerShell cmdlet.</span></span>

> [!NOTE]
> <span data-ttu-id="a604c-137">A mintaalkalmazás ad hozzá egy S1 Standard IoT-központot, amelynek kell fizetni.</span><span class="sxs-lookup"><span data-stu-id="a604c-137">This example application adds an S1 Standard IoT Hub for which you are billed.</span></span> <span data-ttu-id="a604c-138">Törölheti a hello IoT-központ keresztül hello [Azure-portálon] [ lnk-azure-portal] vagy hello segítségével **Remove-AzureRmResource** PowerShell-parancsmag, amikor elkészült.</span><span class="sxs-lookup"><span data-stu-id="a604c-138">You can delete hello IoT hub through hello [Azure portal][lnk-azure-portal] or by using hello **Remove-AzureRmResource** PowerShell cmdlet when you are finished.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a604c-139">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a604c-139">Next steps</span></span>

<span data-ttu-id="a604c-140">Most egy IoT-központot, a PowerShell segítségével az Azure Resource Manager-sablon használatával telepített, akkor érdemes lehet további tooexplore:</span><span class="sxs-lookup"><span data-stu-id="a604c-140">Now you have deployed an IoT hub using an Azure Resource Manager template with PowerShell, you may want tooexplore further:</span></span>

* <span data-ttu-id="a604c-141">További információ a hello hello képességeit [IoT-központ erőforrás-szolgáltató REST API][lnk-rest-api].</span><span class="sxs-lookup"><span data-stu-id="a604c-141">Read about hello capabilities of hello [IoT Hub resource provider REST API][lnk-rest-api].</span></span>
* <span data-ttu-id="a604c-142">Olvasási [Azure Resource Manager áttekintése] [ lnk-azure-rm-overview] Azure Resource Manager hello képességeivel kapcsolatos további toolearn.</span><span class="sxs-lookup"><span data-stu-id="a604c-142">Read [Azure Resource Manager overview][lnk-azure-rm-overview] toolearn more about hello capabilities of Azure Resource Manager.</span></span>

<span data-ttu-id="a604c-143">toolearn IoT-központot, fejlesztésével kapcsolatos további hello a következő cikkekben talál:</span><span class="sxs-lookup"><span data-stu-id="a604c-143">toolearn more about developing for IoT Hub, see hello following articles:</span></span>

* <span data-ttu-id="a604c-144">[Bevezetés tooC SDK][lnk-c-sdk]</span><span class="sxs-lookup"><span data-stu-id="a604c-144">[Introduction tooC SDK][lnk-c-sdk]</span></span>
* <span data-ttu-id="a604c-145">[Az Azure IoT SDK-k][lnk-sdks]</span><span class="sxs-lookup"><span data-stu-id="a604c-145">[Azure IoT SDKs][lnk-sdks]</span></span>

<span data-ttu-id="a604c-146">toofurther megismerkedhet az IoT-központ hello képességeit, lásd:</span><span class="sxs-lookup"><span data-stu-id="a604c-146">toofurther explore hello capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="a604c-147">[Egy eszköz szimulálva Azure IoT oldala][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="a604c-147">[Simulating a device with Azure IoT Edge][lnk-iotedge]</span></span>

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-azure-portal]: https://portal.azure.com/
[lnk-status]: https://azure.microsoft.com/status/
[lnk-powershell-install]: /powershell/azure/install-azurerm-ps
[lnk-rest-api]: https://docs.microsoft.com/rest/api/iothub/iothubresource
[lnk-azure-rm-overview]: ../azure-resource-manager/resource-group-overview.md
[lnk-powershell-arm]: ../azure-resource-manager/powershell-azure-resource-manager.md

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
