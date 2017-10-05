---
title: "Sablon (PowerShell) segítségével Azure IoT Hub létrehozása |} Microsoft Docs"
description: "Hogyan használható az Azure Resource Manager-sablonok az IoT-központ létrehozása a PowerShell használatával."
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
ms.openlocfilehash: f83fac6cffc9e58582417324a4348ca3b6220f0c
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="create-an-iot-hub-using-azure-resource-manager-template-powershell"></a><span data-ttu-id="a710b-103">Létrehoz egy IoT-központot, Azure Resource Manager sablonnal (PowerShell)</span><span class="sxs-lookup"><span data-stu-id="a710b-103">Create an IoT hub using Azure Resource Manager template (PowerShell)</span></span>

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

<span data-ttu-id="a710b-104">Azure Resource Manager hozhat létre és kezelhet programozott módon Azure IoT-központok.</span><span class="sxs-lookup"><span data-stu-id="a710b-104">You can use Azure Resource Manager to create and manage Azure IoT hubs programmatically.</span></span> <span data-ttu-id="a710b-105">Ez az oktatóanyag bemutatja, hogyan Azure Resource Manager-sablonok segítségével az IoT-központ létrehozása a PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="a710b-105">This tutorial shows you how to use an Azure Resource Manager template to create an IoT hub with PowerShell.</span></span>

> [!NOTE]
> <span data-ttu-id="a710b-106">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Azure Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="a710b-106">Azure has two different deployment models for creating and working with resources: [Azure Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="a710b-107">Ez a cikk az Azure Resource Manager telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="a710b-107">This article covers using the Azure Resource Manager deployment model.</span></span>

<span data-ttu-id="a710b-108">Az oktatóanyag teljesítéséhez a következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="a710b-108">To complete this tutorial, you need the following:</span></span>

* <span data-ttu-id="a710b-109">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="a710b-109">An active Azure account.</span></span> <br/><span data-ttu-id="a710b-110">Ha nincs fiókja, néhány perc alatt létrehozhat egy [ingyenes fiókot][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="a710b-110">If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.</span></span>
* <span data-ttu-id="a710b-111">[Az Azure PowerShell 1.0] [ lnk-powershell-install] vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="a710b-111">[Azure PowerShell 1.0][lnk-powershell-install] or later.</span></span>

> [!TIP]
> <span data-ttu-id="a710b-112">A cikk [az Azure PowerShell használata Azure Resource Managerrel] [ lnk-powershell-arm] PowerShell és az Azure Resource Manager sablonok létrehozása az Azure-erőforrások használatával kapcsolatos további információk.</span><span class="sxs-lookup"><span data-stu-id="a710b-112">The article [Using Azure PowerShell with Azure Resource Manager][lnk-powershell-arm] provides more information about how to use PowerShell and Azure Resource Manager templates to create Azure resources.</span></span>

## <a name="connect-to-your-azure-subscription"></a><span data-ttu-id="a710b-113">Csatlakozás az Azure-előfizetéshez</span><span class="sxs-lookup"><span data-stu-id="a710b-113">Connect to your Azure subscription</span></span>

<span data-ttu-id="a710b-114">Adja meg egy PowerShell-parancssort, jelentkezzen be az Azure-előfizetéshez a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="a710b-114">In a PowerShell command prompt, enter the following command to sign in to your Azure subscription:</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="a710b-115">Ha több Azure-előfizetéssel rendelkezik, a jelentkezik be az Azure ad hozzáférést az összes Azure-előfizetést a hitelesítő adatok társított.</span><span class="sxs-lookup"><span data-stu-id="a710b-115">If you have multiple Azure subscriptions, signing in to Azure grants you access to all the Azure subscriptions associated with your credentials.</span></span> <span data-ttu-id="a710b-116">Használja a következő parancsot a rendelkezésre álló használata Azure-előfizetések listázásához:</span><span class="sxs-lookup"><span data-stu-id="a710b-116">Use the following command to list the Azure subscriptions available for you to use:</span></span>

```powershell
Get-AzureRMSubscription
```

<span data-ttu-id="a710b-117">Az alábbi parancs segítségével válassza ki, hogy az IoT hub létrehozására szolgáló parancsok futtatásához használni kívánt előfizetést.</span><span class="sxs-lookup"><span data-stu-id="a710b-117">Use the following command to select subscription that you want to use to run the commands to create your IoT hub.</span></span> <span data-ttu-id="a710b-118">Az előfizetés neve vagy azonosítója is használhatja, ha az előző parancs kimenetében:</span><span class="sxs-lookup"><span data-stu-id="a710b-118">You can use either the subscription name or ID from the output of the previous command:</span></span>

```powershell
Select-AzureRMSubscription `
    -SubscriptionName "{your subscription name}"
```

<span data-ttu-id="a710b-119">A következő parancsok segítségével felderítése, ahol az IoT-központ és a jelenleg támogatott API-verziók telepítheti:</span><span class="sxs-lookup"><span data-stu-id="a710b-119">You can use the following commands to discover where you can deploy an IoT hub and the currently supported API versions:</span></span>

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Devices).ResourceTypes | Where-Object ResourceTypeName -eq IoTHubs).Locations
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Devices).ResourceTypes | Where-Object ResourceTypeName -eq IoTHubs).ApiVersions
```

<span data-ttu-id="a710b-120">Hozzon létre egy erőforráscsoportot az IoT hub, az alábbi paranccsal egy IoT-központ támogatott helyeket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="a710b-120">Create a resource group to contain your IoT hub using the following command in one of the supported locations for IoT Hub.</span></span> <span data-ttu-id="a710b-121">Ebben a példában egy nevű erőforráscsoportot hoz létre **MyIoTRG1**:</span><span class="sxs-lookup"><span data-stu-id="a710b-121">This example creates a resource group called **MyIoTRG1**:</span></span>

```powershell
New-AzureRmResourceGroup -Name MyIoTRG1 -Location "East US"
```

## <a name="submit-a-template-to-create-an-iot-hub"></a><span data-ttu-id="a710b-122">Az IoT-központ létrehozása sablon küldése</span><span class="sxs-lookup"><span data-stu-id="a710b-122">Submit a template to create an IoT hub</span></span>

<span data-ttu-id="a710b-123">A JSON-sablon használatával létrehoz egy IoT-központot az erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="a710b-123">Use a JSON template to create an IoT hub in your resource group.</span></span> <span data-ttu-id="a710b-124">Az Azure Resource Manager-sablon segítségével módosíthatja egy meglévő IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="a710b-124">You can also use an Azure Resource Manager template to make changes to an existing IoT hub.</span></span>

1. <span data-ttu-id="a710b-125">Egy szövegszerkesztő használatával hozzon létre egy Azure Resource Manager-sablon neve **template.json** egy új, normál IoT-központ létrehozásához a következő erőforrás-definícióval.</span><span class="sxs-lookup"><span data-stu-id="a710b-125">Use a text editor to create an Azure Resource Manager template called **template.json** with the following resource definition to create a new standard IoT hub.</span></span> <span data-ttu-id="a710b-126">Ebben a példában az IoT-központ hozzáadja a **USA keleti régiója** régió, létrehoz két felhasználói csoportok (**cg1** és **cg2**) Event Hub-kompatibilis végpontot, és használja a  **2016-02-03** API-verzió.</span><span class="sxs-lookup"><span data-stu-id="a710b-126">This example adds the IoT Hub in the **East US** region, creates two consumer groups (**cg1** and **cg2**) on the Event Hub-compatible endpoint, and uses the **2016-02-03** API version.</span></span> <span data-ttu-id="a710b-127">Ez a sablon is vár, hogy az IoT-központnév paraméterként adjon át nevű **hubName**.</span><span class="sxs-lookup"><span data-stu-id="a710b-127">This template also expects you to pass in the IoT hub name as a parameter called **hubName**.</span></span> <span data-ttu-id="a710b-128">A helyek, amelyek támogatják az IoT-központ aktuális listáját lásd: [Azure állapot][lnk-status].</span><span class="sxs-lookup"><span data-stu-id="a710b-128">For the current list of locations that support IoT Hub see [Azure Status][lnk-status].</span></span>

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

2. <span data-ttu-id="a710b-129">Mentse az Azure Resource Manager sablon fájlt a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="a710b-129">Save the Azure Resource Manager template file on your local machine.</span></span> <span data-ttu-id="a710b-130">Ebben a példában feltételezzük, hogy nevű mappába menti **c:\templates**.</span><span class="sxs-lookup"><span data-stu-id="a710b-130">This example assumes you save it in a folder called **c:\templates**.</span></span>

3. <span data-ttu-id="a710b-131">A következő paranccsal telepítheti az új IoT hub, amely az IoT hub nevét átadott paraméterként.</span><span class="sxs-lookup"><span data-stu-id="a710b-131">Run the following command to deploy your new IoT hub, passing the name of your IoT hub as a parameter.</span></span> <span data-ttu-id="a710b-132">Ebben a példában az IoT-központ neve nem `abcmyiothub`.</span><span class="sxs-lookup"><span data-stu-id="a710b-132">In this example, the name of the IoT hub is `abcmyiothub`.</span></span> <span data-ttu-id="a710b-133">Az IoT hub nevét globálisan egyedinek kell lennie:</span><span class="sxs-lookup"><span data-stu-id="a710b-133">The name of your IoT hub must be globally unique:</span></span>

    ```powershell
    New-AzureRmResourceGroupDeployment -ResourceGroupName MyIoTRG1 -TemplateFile C:\templates\template.json -hubName abcmyiothub
    ```
  [!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]

4. <span data-ttu-id="a710b-134">A kimenet a kulcsokat az IoT hub létrehozott jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="a710b-134">The output displays the keys for the IoT hub you created.</span></span>

5. <span data-ttu-id="a710b-135">Ellenőrizze, hogy az alkalmazás fel az új IoT hub, látogasson el a [Azure-portálon] [ lnk-azure-portal] és tekintse meg az erőforrások listáját.</span><span class="sxs-lookup"><span data-stu-id="a710b-135">To verify your application added the new IoT hub, visit the [Azure portal][lnk-azure-portal] and view your list of resources.</span></span> <span data-ttu-id="a710b-136">Másik megoldásként használhatja a **Get-AzureRmResource** PowerShell-parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="a710b-136">Alternatively, use the **Get-AzureRmResource** PowerShell cmdlet.</span></span>

> [!NOTE]
> <span data-ttu-id="a710b-137">A mintaalkalmazás ad hozzá egy S1 Standard IoT-központot, amelynek kell fizetni.</span><span class="sxs-lookup"><span data-stu-id="a710b-137">This example application adds an S1 Standard IoT Hub for which you are billed.</span></span> <span data-ttu-id="a710b-138">Az IoT hub használatával törölheti a [Azure-portálon] [ lnk-azure-portal] vagy használatával a **Remove-AzureRmResource** PowerShell-parancsmag, amikor elkészült.</span><span class="sxs-lookup"><span data-stu-id="a710b-138">You can delete the IoT hub through the [Azure portal][lnk-azure-portal] or by using the **Remove-AzureRmResource** PowerShell cmdlet when you are finished.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a710b-139">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a710b-139">Next steps</span></span>

<span data-ttu-id="a710b-140">Most egy IoT-központot, a PowerShell segítségével az Azure Resource Manager-sablon használatával telepített, akkor érdemes lehet további felfedezése:</span><span class="sxs-lookup"><span data-stu-id="a710b-140">Now you have deployed an IoT hub using an Azure Resource Manager template with PowerShell, you may want to explore further:</span></span>

* <span data-ttu-id="a710b-141">Olvassa el a képességeit a [IoT-központ erőforrás-szolgáltató REST API][lnk-rest-api].</span><span class="sxs-lookup"><span data-stu-id="a710b-141">Read about the capabilities of the [IoT Hub resource provider REST API][lnk-rest-api].</span></span>
* <span data-ttu-id="a710b-142">Olvasási [Azure Resource Manager áttekintése] [ lnk-azure-rm-overview] tudhat meg többet az Azure Resource Manager képességeit.</span><span class="sxs-lookup"><span data-stu-id="a710b-142">Read [Azure Resource Manager overview][lnk-azure-rm-overview] to learn more about the capabilities of Azure Resource Manager.</span></span>

<span data-ttu-id="a710b-143">Az IoT-központ fejlesztésével kapcsolatos további tudnivalókért tekintse meg a következő cikkeket:</span><span class="sxs-lookup"><span data-stu-id="a710b-143">To learn more about developing for IoT Hub, see the following articles:</span></span>

* <span data-ttu-id="a710b-144">[C SDK bemutatása][lnk-c-sdk]</span><span class="sxs-lookup"><span data-stu-id="a710b-144">[Introduction to C SDK][lnk-c-sdk]</span></span>
* <span data-ttu-id="a710b-145">[Az Azure IoT SDK-k][lnk-sdks]</span><span class="sxs-lookup"><span data-stu-id="a710b-145">[Azure IoT SDKs][lnk-sdks]</span></span>

<span data-ttu-id="a710b-146">Az IoT-központ képességeit további megismeréséhez lásd:</span><span class="sxs-lookup"><span data-stu-id="a710b-146">To further explore the capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="a710b-147">[Egy eszköz szimulálva Azure IoT oldala][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="a710b-147">[Simulating a device with Azure IoT Edge][lnk-iotedge]</span></span>

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
