---
title: "Hozzon létre egy Azure IoT Hub sablonnal (.NET) |} Microsoft Docs"
description: "Hogyan használható az Azure Resource Manager-sablonok az IoT-központ létrehozása a C# programban."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: a447b40c-c728-487e-875d-db554db5adc3
ms.service: iot-hub
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: 0f197a28e0c51b06d0b47a03c29fe1fde0c6b78d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="create-an-iot-hub-using-azure-resource-manager-template-net"></a><span data-ttu-id="03f0b-103">Létrehoz egy IoT-központot, Azure Resource Manager sablonnal (.NET)</span><span class="sxs-lookup"><span data-stu-id="03f0b-103">Create an IoT hub using Azure Resource Manager template (.NET)</span></span>

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

<span data-ttu-id="03f0b-104">Azure Resource Manager hozhat létre és kezelhet programozott módon Azure IoT-központok.</span><span class="sxs-lookup"><span data-stu-id="03f0b-104">You can use Azure Resource Manager to create and manage Azure IoT hubs programmatically.</span></span> <span data-ttu-id="03f0b-105">Az oktatóanyag bemutatja, hogyan használható az Azure Resource Manager-sablon IoT hub létrehozása a C# programban.</span><span class="sxs-lookup"><span data-stu-id="03f0b-105">This tutorial shows you how to use an Azure Resource Manager template to create an IoT hub from a C# program.</span></span>

> [!NOTE]
> <span data-ttu-id="03f0b-106">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Azure Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="03f0b-106">Azure has two different deployment models for creating and working with resources:  [Azure Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="03f0b-107">Ez a cikk az Azure Resource Manager telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="03f0b-107">This article covers using the Azure Resource Manager deployment model.</span></span>

<span data-ttu-id="03f0b-108">Az oktatóanyag teljesítéséhez a következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="03f0b-108">To complete this tutorial, you need the following:</span></span>

* <span data-ttu-id="03f0b-109">Visual Studio 2015 vagy Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="03f0b-109">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="03f0b-110">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="03f0b-110">An active Azure account.</span></span> <br/><span data-ttu-id="03f0b-111">Ha nincs fiókja, néhány perc alatt létrehozhat egy [ingyenes fiókot][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="03f0b-111">If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.</span></span>
* <span data-ttu-id="03f0b-112">Egy [Azure Storage-fiók] [ lnk-storage-account] az Azure Resource Manager sablon fájlok tárolására.</span><span class="sxs-lookup"><span data-stu-id="03f0b-112">An [Azure Storage account][lnk-storage-account] where you can store your Azure Resource Manager template files.</span></span>
* <span data-ttu-id="03f0b-113">[Az Azure PowerShell 1.0] [ lnk-powershell-install] vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="03f0b-113">[Azure PowerShell 1.0][lnk-powershell-install] or later.</span></span>

[!INCLUDE [iot-hub-prepare-resource-manager](../../includes/iot-hub-prepare-resource-manager.md)]

## <a name="prepare-your-visual-studio-project"></a><span data-ttu-id="03f0b-114">Készítse elő a Visual Studio-projekt</span><span class="sxs-lookup"><span data-stu-id="03f0b-114">Prepare your Visual Studio project</span></span>

1. <span data-ttu-id="03f0b-115">A Visual Studio project using Visual C# klasszikus Windows asztal létrehozása a **Konzolalkalmazás (.NET-keretrendszer)** projektsablon.</span><span class="sxs-lookup"><span data-stu-id="03f0b-115">In Visual Studio, create a Visual C# Windows Classic Desktop project using the **Console App (.NET Framework)** project template.</span></span> <span data-ttu-id="03f0b-116">Nevet a projektnek **CreateIoTHub**.</span><span class="sxs-lookup"><span data-stu-id="03f0b-116">Name the project **CreateIoTHub**.</span></span>

2. <span data-ttu-id="03f0b-117">A Megoldáskezelőben kattintson a jobb gombbal a projektre, és kattintson a **NuGet-csomagok kezelése**.</span><span class="sxs-lookup"><span data-stu-id="03f0b-117">In Solution Explorer, right-click on your project and then click **Manage NuGet Packages**.</span></span>

3. <span data-ttu-id="03f0b-118">Ellenőrizze a NuGet Package Manager **Include prerelease**, és a a **Tallózás** lapon keresse meg **Microsoft.Azure.Management.ResourceManager**.</span><span class="sxs-lookup"><span data-stu-id="03f0b-118">In NuGet Package Manager, check **Include prerelease**, and on the **Browse** page search for **Microsoft.Azure.Management.ResourceManager**.</span></span> <span data-ttu-id="03f0b-119">Válassza ki a csomagot, kattintson a **telepítése**, a **változások áttekintése** kattintson **OK**, majd kattintson a **elfogadom** fogadására a licencek.</span><span class="sxs-lookup"><span data-stu-id="03f0b-119">Select the package, click **Install**, in **Review Changes** click **OK**, then click **I Accept** to accept the licenses.</span></span>

4. <span data-ttu-id="03f0b-120">Keressen rá a NuGet Package Manager **Microsoft.IdentityModel.Clients.ActiveDirectory**.</span><span class="sxs-lookup"><span data-stu-id="03f0b-120">In NuGet Package Manager, search for **Microsoft.IdentityModel.Clients.ActiveDirectory**.</span></span>  <span data-ttu-id="03f0b-121">Kattintson a **telepítése**, a **változások áttekintése** kattintson **OK**, majd kattintson a **elfogadom** elfogadja a licencfeltételeket.</span><span class="sxs-lookup"><span data-stu-id="03f0b-121">Click **Install**, in **Review Changes** click **OK**, then click **I Accept** to accept the license.</span></span>

5. <span data-ttu-id="03f0b-122">A productscontract.cs fájlban cserélje le a meglévő **használatával** utasítások a következő kóddal:</span><span class="sxs-lookup"><span data-stu-id="03f0b-122">In Program.cs, replace the existing **using** statements with the following code:</span></span>

    ```csharp
    using System;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.ResourceManager.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.Rest;
    ```

6. <span data-ttu-id="03f0b-123">A program.cs fájlban adja hozzá a következő statikus változó a helyőrző értékeket.</span><span class="sxs-lookup"><span data-stu-id="03f0b-123">In Program.cs, add the following static variables replacing the placeholder values.</span></span> <span data-ttu-id="03f0b-124">A feljegyezte **ApplicationId**, **SubscriptionId**, **TenantId**, és **jelszó** az oktatóanyag korábbi.</span><span class="sxs-lookup"><span data-stu-id="03f0b-124">You made a note of **ApplicationId**, **SubscriptionId**, **TenantId**, and **Password** earlier in this tutorial.</span></span> <span data-ttu-id="03f0b-125">**Az Azure Storage-fiók neve** van az Azure Storage-fiók nevét, az Azure Resource Manager sablon fájlokat tárolja.</span><span class="sxs-lookup"><span data-stu-id="03f0b-125">**Your Azure Storage account name** is the name of the Azure Storage account where you store your Azure Resource Manager template files.</span></span> <span data-ttu-id="03f0b-126">**Az erőforráscsoport neve** az IoT hub létrehozása során használ az erőforráscsoport neve.</span><span class="sxs-lookup"><span data-stu-id="03f0b-126">**Resource group name** is the name of the resource group you use when you create the IoT hub.</span></span> <span data-ttu-id="03f0b-127">A név lehet egy már meglévő vagy új erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="03f0b-127">The name can be a pre-existing or new resource group.</span></span> <span data-ttu-id="03f0b-128">**A központi telepítés neve** van, mint a központi telepítés nevét **Deployment_01**.</span><span class="sxs-lookup"><span data-stu-id="03f0b-128">**Deployment name** is a name for the deployment, such as **Deployment_01**.</span></span>

    ```csharp
    static string applicationId = "{Your ApplicationId}";
    static string subscriptionId = "{Your SubscriptionId}";
    static string tenantId = "{Your TenantId}";
    static string password = "{Your application Password}";
    static string storageAddress = "https://{Your storage account name}.blob.core.windows.net";
    static string rgName = "{Resource group name}";
    static string deploymentName = "{Deployment name}";
    ```

[!INCLUDE [iot-hub-get-access-token](../../includes/iot-hub-get-access-token.md)]

## <a name="submit-a-template-to-create-an-iot-hub"></a><span data-ttu-id="03f0b-129">Az IoT-központ létrehozása sablon küldése</span><span class="sxs-lookup"><span data-stu-id="03f0b-129">Submit a template to create an IoT hub</span></span>

<span data-ttu-id="03f0b-130">A JSON-sablonnal és paraméterfájlokkal fájl segítségével létrehoz egy IoT-központot az erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="03f0b-130">Use a JSON template and parameter file to create an IoT hub in your resource group.</span></span> <span data-ttu-id="03f0b-131">Az Azure Resource Manager-sablon segítségével módosíthatja egy meglévő IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="03f0b-131">You can also use an Azure Resource Manager template to make changes to an existing IoT hub.</span></span>

1. <span data-ttu-id="03f0b-132">A Megoldáskezelőben kattintson a jobb gombbal a projektre, kattintson a **Hozzáadás**, és kattintson a **új elem**.</span><span class="sxs-lookup"><span data-stu-id="03f0b-132">In Solution Explorer, right-click on your project, click **Add**, and then click **New Item**.</span></span> <span data-ttu-id="03f0b-133">Adja hozzá a JSON-fájl neve **template.json** a projekthez.</span><span class="sxs-lookup"><span data-stu-id="03f0b-133">Add a JSON file called **template.json** to your project.</span></span>

2. <span data-ttu-id="03f0b-134">A szabványos IoT-központ hozzáadása a **USA keleti régiója** régió, cserélje ki a tartalmát **template.json** a következő erőforrás-definícióval.</span><span class="sxs-lookup"><span data-stu-id="03f0b-134">To add a standard IoT hub to the **East US** region, replace the contents of **template.json** with the following resource definition.</span></span> <span data-ttu-id="03f0b-135">Az aktuális IoT Hub-t támogató régiók listáját lásd: [Azure állapot][lnk-status]:</span><span class="sxs-lookup"><span data-stu-id="03f0b-135">For the current list of regions that support IoT Hub see [Azure Status][lnk-status]:</span></span>

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

3. <span data-ttu-id="03f0b-136">A Megoldáskezelőben kattintson a jobb gombbal a projektre, kattintson a **Hozzáadás**, és kattintson a **új elem**.</span><span class="sxs-lookup"><span data-stu-id="03f0b-136">In Solution Explorer, right-click on your project, click **Add**, and then click **New Item**.</span></span> <span data-ttu-id="03f0b-137">Adja hozzá a JSON-fájl neve **parameters.json** a projekthez.</span><span class="sxs-lookup"><span data-stu-id="03f0b-137">Add a JSON file called **parameters.json** to your project.</span></span>

4. <span data-ttu-id="03f0b-138">Cserélje le a tartalmát **parameters.json** a következő paraméter információkat, például az új IoT hub nevét megadó **{saját monogramjával} mynewiothub**.</span><span class="sxs-lookup"><span data-stu-id="03f0b-138">Replace the contents of **parameters.json** with the following parameter information that sets a name for the new IoT hub such as **{your initials}mynewiothub**.</span></span> <span data-ttu-id="03f0b-139">Az IoT-központnév globálisan egyedinek kell lennie, a név vagy initials tartalmaznia kell:</span><span class="sxs-lookup"><span data-stu-id="03f0b-139">The IoT hub name must be globally unique so it should include your name or initials:</span></span>

    ```json
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "hubName": { "value": "mynewiothub" }
      }
    }
    ```
  [!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]

5. <span data-ttu-id="03f0b-140">A **Server Explorer**, az Azure-előfizetéshez csatlakozni és az Azure Storage-fiókban létrehozni egy tárolót, úgynevezett **sablonok**.</span><span class="sxs-lookup"><span data-stu-id="03f0b-140">In **Server Explorer**, connect to your Azure subscription, and in your Azure Storage account create a container called **templates**.</span></span> <span data-ttu-id="03f0b-141">Az a **tulajdonságok** panelen, állítsa be a **nyilvános olvasási hozzáférés** engedélyeit a **sablonok** tárolót, hogy **Blob**.</span><span class="sxs-lookup"><span data-stu-id="03f0b-141">In the **Properties** panel, set the **Public Read Access** permissions for the **templates** container to **Blob**.</span></span>

6. <span data-ttu-id="03f0b-142">A **Server Explorer**, kattintson a jobb gombbal a **sablonok** tárolót, majd kattintson **nézet Blob tároló**.</span><span class="sxs-lookup"><span data-stu-id="03f0b-142">In **Server Explorer**, right-click on the **templates** container and then click **View Blob Container**.</span></span> <span data-ttu-id="03f0b-143">Kattintson a **Blob feltöltése** gombra, válassza ki a két fájl **parameters.json** és **templates.json**, és kattintson a **nyissa meg** a JSON-fájlok feltöltése a **sablonok** tároló.</span><span class="sxs-lookup"><span data-stu-id="03f0b-143">Click the **Upload Blob** button, select the two files, **parameters.json** and **templates.json**, and then click **Open** to upload the JSON files to the **templates** container.</span></span> <span data-ttu-id="03f0b-144">A blobok JSON-adatokat tartalmazó URL-címei a következők:</span><span class="sxs-lookup"><span data-stu-id="03f0b-144">The URLs of the blobs containing the JSON data are:</span></span>

    ```csharp
    https://{Your storage account name}.blob.core.windows.net/templates/parameters.json
    https://{Your storage account name}.blob.core.windows.net/templates/template.json
    ```
7. <span data-ttu-id="03f0b-145">Adja hozzá a következő metódust a program.cs fájlt:</span><span class="sxs-lookup"><span data-stu-id="03f0b-145">Add the following method to Program.cs:</span></span>

    ```csharp
    static void CreateIoTHub(ResourceManagementClient client)
    {

    }
    ```

8. <span data-ttu-id="03f0b-146">Adja hozzá a következő kódot a **CreateIoTHub** elküldeni a sablonnal és paraméterfájlokkal fájlok az Azure erőforrás-kezelő metódus:</span><span class="sxs-lookup"><span data-stu-id="03f0b-146">Add the following code to the **CreateIoTHub** method to submit the template and parameter files to the Azure Resource Manager:</span></span>

    ```csharp
    var createResponse = client.Deployments.CreateOrUpdate(
        rgName,
        deploymentName,
        new Deployment()
        {
          Properties = new DeploymentProperties
          {
            Mode = DeploymentMode.Incremental,
            TemplateLink = new TemplateLink
            {
              Uri = storageAddress + "/templates/template.json"
            },
            ParametersLink = new ParametersLink
            {
              Uri = storageAddress + "/templates/parameters.json"
            }
          }
        });
    ```

9. <span data-ttu-id="03f0b-147">Adja hozzá a következő kódot a **CreateIoTHub** módszer, amelynek állapota és a kulcsokat az új IoT hub jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="03f0b-147">Add the following code to the **CreateIoTHub** method that displays the status and the keys for the new IoT hub:</span></span>

    ```csharp
    string state = createResponse.Properties.ProvisioningState;
    Console.WriteLine("Deployment state: {0}", state);

    if (state != "Succeeded")
    {
      Console.WriteLine("Failed to create iothub");
    }
    Console.WriteLine(createResponse.Properties.Outputs);
    ```

## <a name="complete-and-run-the-application"></a><span data-ttu-id="03f0b-148">Végezze el, és futtassa az alkalmazást</span><span class="sxs-lookup"><span data-stu-id="03f0b-148">Complete and run the application</span></span>

<span data-ttu-id="03f0b-149">Meghívásával hajthatja végre az alkalmazás most már a **CreateIoTHub** metódus előtt most felépíti és futtatja azt.</span><span class="sxs-lookup"><span data-stu-id="03f0b-149">You can now complete the application by calling the **CreateIoTHub** method before you build and run it.</span></span>

1. <span data-ttu-id="03f0b-150">Adja hozzá a következő kódot végén a **fő** módszert:</span><span class="sxs-lookup"><span data-stu-id="03f0b-150">Add the following code to the end of the **Main** method:</span></span>

    ```csharp
    CreateIoTHub(client);
    Console.ReadLine();
    ```

2. <span data-ttu-id="03f0b-151">Kattintson a **Build** , majd **megoldás**.</span><span class="sxs-lookup"><span data-stu-id="03f0b-151">Click **Build** and then **Build Solution**.</span></span> <span data-ttu-id="03f0b-152">Javíthatja az esetleges hibákat.</span><span class="sxs-lookup"><span data-stu-id="03f0b-152">Correct any errors.</span></span>

3. <span data-ttu-id="03f0b-153">Kattintson a **Debug** , majd **Start Debugging** az alkalmazás futtatásához.</span><span class="sxs-lookup"><span data-stu-id="03f0b-153">Click **Debug** and then **Start Debugging** to run the application.</span></span> <span data-ttu-id="03f0b-154">A központi telepítés futtatása több percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="03f0b-154">It may take several minutes for the deployment to run.</span></span>

4. <span data-ttu-id="03f0b-155">Ellenőrizze, hogy az alkalmazás fel az új IoT hub, látogasson el a [Azure-portálon] [ lnk-azure-portal] és tekintse meg az erőforrások listáját.</span><span class="sxs-lookup"><span data-stu-id="03f0b-155">To verify your application added the new IoT hub, visit the [Azure portal][lnk-azure-portal] and view your list of resources.</span></span> <span data-ttu-id="03f0b-156">Másik megoldásként használhatja a **Get-AzureRmResource** PowerShell-parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="03f0b-156">Alternatively, use the **Get-AzureRmResource** PowerShell cmdlet.</span></span>

> [!NOTE]
> <span data-ttu-id="03f0b-157">A mintaalkalmazás ad hozzá egy S1 Standard IoT-központot, amelynek kell fizetni.</span><span class="sxs-lookup"><span data-stu-id="03f0b-157">This example application adds an S1 Standard IoT Hub for which you are billed.</span></span> <span data-ttu-id="03f0b-158">Az IoT hub használatával törölheti a [Azure-portálon] [ lnk-azure-portal] vagy használatával a **Remove-AzureRmResource** PowerShell-parancsmag, amikor elkészült.</span><span class="sxs-lookup"><span data-stu-id="03f0b-158">You can delete the IoT hub through the [Azure portal][lnk-azure-portal] or by using the **Remove-AzureRmResource** PowerShell cmdlet when you are finished.</span></span>

## <a name="next-steps"></a><span data-ttu-id="03f0b-159">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="03f0b-159">Next steps</span></span>
<span data-ttu-id="03f0b-160">Most egy IoT-központot egy C# programban egy Azure Resource Manager sablon használatával telepített, akkor érdemes lehet további felfedezése:</span><span class="sxs-lookup"><span data-stu-id="03f0b-160">Now you have deployed an IoT hub using an Azure Resource Manager template with a C# program, you may want to explore further:</span></span>

* <span data-ttu-id="03f0b-161">Olvassa el a képességeit a [IoT-központ erőforrás-szolgáltató REST API][lnk-rest-api].</span><span class="sxs-lookup"><span data-stu-id="03f0b-161">Read about the capabilities of the [IoT Hub resource provider REST API][lnk-rest-api].</span></span>
* <span data-ttu-id="03f0b-162">Olvasási [Azure Resource Manager áttekintése] [ lnk-azure-rm-overview] tudhat meg többet az Azure Resource Manager képességeit.</span><span class="sxs-lookup"><span data-stu-id="03f0b-162">Read [Azure Resource Manager overview][lnk-azure-rm-overview] to learn more about the capabilities of Azure Resource Manager.</span></span>

<span data-ttu-id="03f0b-163">Az IoT-központ fejlesztésével kapcsolatos további tudnivalókért tekintse meg a következő cikkeket:</span><span class="sxs-lookup"><span data-stu-id="03f0b-163">To learn more about developing for IoT Hub, see the following articles:</span></span>

* <span data-ttu-id="03f0b-164">[C SDK bemutatása][lnk-c-sdk]</span><span class="sxs-lookup"><span data-stu-id="03f0b-164">[Introduction to C SDK][lnk-c-sdk]</span></span>
* <span data-ttu-id="03f0b-165">[Az Azure IoT SDK-k][lnk-sdks]</span><span class="sxs-lookup"><span data-stu-id="03f0b-165">[Azure IoT SDKs][lnk-sdks]</span></span>

<span data-ttu-id="03f0b-166">Az IoT-központ képességeit további megismeréséhez lásd:</span><span class="sxs-lookup"><span data-stu-id="03f0b-166">To further explore the capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="03f0b-167">[Egy eszköz szimulálva Azure IoT oldala][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="03f0b-167">[Simulating a device with Azure IoT Edge][lnk-iotedge]</span></span>

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-azure-portal]: https://portal.azure.com/
[lnk-status]: https://azure.microsoft.com/status/
[lnk-powershell-install]: https://docs.microsoft.com/powershell/azure/install-azurerm-ps
[lnk-rest-api]: https://docs.microsoft.com/rest/api/iothub/iothubresource
[lnk-azure-rm-overview]: ../azure-resource-manager/resource-group-overview.md
[lnk-storage-account]:../storage/common/storage-create-storage-account.md

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
