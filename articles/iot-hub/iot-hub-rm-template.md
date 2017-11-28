---
title: "az Azure IoT-központ (.NET) sablon használatával aaaCreate |} Microsoft Docs"
description: "Hogyan toouse az Azure Resource Manager sablon toocreate egy IoT-központot egy C# programban."
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
ms.openlocfilehash: 6140deff3553701f994502fd4a60178f874e27cf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-iot-hub-using-azure-resource-manager-template-net"></a><span data-ttu-id="d49c5-103">Létrehoz egy IoT-központot, Azure Resource Manager sablonnal (.NET)</span><span class="sxs-lookup"><span data-stu-id="d49c5-103">Create an IoT hub using Azure Resource Manager template (.NET)</span></span>

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

<span data-ttu-id="d49c5-104">Használhatja az Azure Resource Manager toocreate és Azure IoT-központok programozott kezelését.</span><span class="sxs-lookup"><span data-stu-id="d49c5-104">You can use Azure Resource Manager toocreate and manage Azure IoT hubs programmatically.</span></span> <span data-ttu-id="d49c5-105">Az oktatóanyag bemutatja, hogyan toouse az Azure Resource Manager sablon toocreate egy IoT-központot egy C# programban.</span><span class="sxs-lookup"><span data-stu-id="d49c5-105">This tutorial shows you how toouse an Azure Resource Manager template toocreate an IoT hub from a C# program.</span></span>

> [!NOTE]
> <span data-ttu-id="d49c5-106">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Azure Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="d49c5-106">Azure has two different deployment models for creating and working with resources:  [Azure Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="d49c5-107">Ez a cikk hello Azure Resource Manager telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="d49c5-107">This article covers using hello Azure Resource Manager deployment model.</span></span>

<span data-ttu-id="d49c5-108">toocomplete ebben az oktatóanyagban a következő hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="d49c5-108">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="d49c5-109">Visual Studio 2015 vagy Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="d49c5-109">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="d49c5-110">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="d49c5-110">An active Azure account.</span></span> <br/><span data-ttu-id="d49c5-111">Ha nincs fiókja, néhány perc alatt létrehozhat egy [ingyenes fiókot][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="d49c5-111">If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.</span></span>
* <span data-ttu-id="d49c5-112">Egy [Azure Storage-fiók] [ lnk-storage-account] az Azure Resource Manager sablon fájlok tárolására.</span><span class="sxs-lookup"><span data-stu-id="d49c5-112">An [Azure Storage account][lnk-storage-account] where you can store your Azure Resource Manager template files.</span></span>
* <span data-ttu-id="d49c5-113">[Az Azure PowerShell 1.0] [ lnk-powershell-install] vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="d49c5-113">[Azure PowerShell 1.0][lnk-powershell-install] or later.</span></span>

[!INCLUDE [iot-hub-prepare-resource-manager](../../includes/iot-hub-prepare-resource-manager.md)]

## <a name="prepare-your-visual-studio-project"></a><span data-ttu-id="d49c5-114">Készítse elő a Visual Studio-projekt</span><span class="sxs-lookup"><span data-stu-id="d49c5-114">Prepare your Visual Studio project</span></span>

1. <span data-ttu-id="d49c5-115">A Visual Studio használatával hello Visual C# klasszikus Windows asztal-projekt létrehozása **Konzolalkalmazás (.NET-keretrendszer)** projektsablon.</span><span class="sxs-lookup"><span data-stu-id="d49c5-115">In Visual Studio, create a Visual C# Windows Classic Desktop project using hello **Console App (.NET Framework)** project template.</span></span> <span data-ttu-id="d49c5-116">Név hello projekt **CreateIoTHub**.</span><span class="sxs-lookup"><span data-stu-id="d49c5-116">Name hello project **CreateIoTHub**.</span></span>

2. <span data-ttu-id="d49c5-117">A Megoldáskezelőben kattintson a jobb gombbal a projektre, és kattintson a **NuGet-csomagok kezelése**.</span><span class="sxs-lookup"><span data-stu-id="d49c5-117">In Solution Explorer, right-click on your project and then click **Manage NuGet Packages**.</span></span>

3. <span data-ttu-id="d49c5-118">Ellenőrizze a NuGet Package Manager **Include prerelease**, és a hello **Tallózás** lapon keresse meg **Microsoft.Azure.Management.ResourceManager**.</span><span class="sxs-lookup"><span data-stu-id="d49c5-118">In NuGet Package Manager, check **Include prerelease**, and on hello **Browse** page search for **Microsoft.Azure.Management.ResourceManager**.</span></span> <span data-ttu-id="d49c5-119">Jelölje ki azt a hello csomag **telepítése**, a **változások áttekintése** kattintson **OK**, majd kattintson **elfogadom** tooaccept hello licencek.</span><span class="sxs-lookup"><span data-stu-id="d49c5-119">Select hello package, click **Install**, in **Review Changes** click **OK**, then click **I Accept** tooaccept hello licenses.</span></span>

4. <span data-ttu-id="d49c5-120">Keressen rá a NuGet Package Manager **Microsoft.IdentityModel.Clients.ActiveDirectory**.</span><span class="sxs-lookup"><span data-stu-id="d49c5-120">In NuGet Package Manager, search for **Microsoft.IdentityModel.Clients.ActiveDirectory**.</span></span>  <span data-ttu-id="d49c5-121">Kattintson a **telepítése**, a **változások áttekintése** kattintson **OK**, majd kattintson a **elfogadom** tooaccept hello licenc.</span><span class="sxs-lookup"><span data-stu-id="d49c5-121">Click **Install**, in **Review Changes** click **OK**, then click **I Accept** tooaccept hello license.</span></span>

5. <span data-ttu-id="d49c5-122">A productscontract.cs fájlban cserélje le a meglévő hello **használatával** utasítások hello a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="d49c5-122">In Program.cs, replace hello existing **using** statements with hello following code:</span></span>

    ```csharp
    using System;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.ResourceManager.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.Rest;
    ```

6. <span data-ttu-id="d49c5-123">A program.cs fájlban adja hozzá a statikus változók hello helyőrző értékeket cserélje le a következő hello.</span><span class="sxs-lookup"><span data-stu-id="d49c5-123">In Program.cs, add hello following static variables replacing hello placeholder values.</span></span> <span data-ttu-id="d49c5-124">A feljegyezte **ApplicationId**, **SubscriptionId**, **TenantId**, és **jelszó** az oktatóanyag korábbi.</span><span class="sxs-lookup"><span data-stu-id="d49c5-124">You made a note of **ApplicationId**, **SubscriptionId**, **TenantId**, and **Password** earlier in this tutorial.</span></span> <span data-ttu-id="d49c5-125">**Az Azure Storage-fiók neve** van hello hello Azure Storage-fiók neve, az Azure Resource Manager sablon fájlokat tárolja.</span><span class="sxs-lookup"><span data-stu-id="d49c5-125">**Your Azure Storage account name** is hello name of hello Azure Storage account where you store your Azure Resource Manager template files.</span></span> <span data-ttu-id="d49c5-126">**Az erőforráscsoport neve** hello név hello erőforráscsoport hello IoT hub létrehozása során használ.</span><span class="sxs-lookup"><span data-stu-id="d49c5-126">**Resource group name** is hello name of hello resource group you use when you create hello IoT hub.</span></span> <span data-ttu-id="d49c5-127">hello neve lehet egy már meglévő vagy új erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="d49c5-127">hello name can be a pre-existing or new resource group.</span></span> <span data-ttu-id="d49c5-128">**A központi telepítés neve** alhálózatnév hello központi telepítéséhez például a **Deployment_01**.</span><span class="sxs-lookup"><span data-stu-id="d49c5-128">**Deployment name** is a name for hello deployment, such as **Deployment_01**.</span></span>

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

## <a name="submit-a-template-toocreate-an-iot-hub"></a><span data-ttu-id="d49c5-129">Küldje el a sablon toocreate az IoT-központ</span><span class="sxs-lookup"><span data-stu-id="d49c5-129">Submit a template toocreate an IoT hub</span></span>

<span data-ttu-id="d49c5-130">A JSON sablonnal és paraméterfájlokkal fájl toocreate az IoT-központ használja az erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="d49c5-130">Use a JSON template and parameter file toocreate an IoT hub in your resource group.</span></span> <span data-ttu-id="d49c5-131">Az Azure Resource Manager sablon toomake módosítások tooan meglévő IoT-központ is használható.</span><span class="sxs-lookup"><span data-stu-id="d49c5-131">You can also use an Azure Resource Manager template toomake changes tooan existing IoT hub.</span></span>

1. <span data-ttu-id="d49c5-132">A Megoldáskezelőben kattintson a jobb gombbal a projektre, kattintson a **Hozzáadás**, és kattintson a **új elem**.</span><span class="sxs-lookup"><span data-stu-id="d49c5-132">In Solution Explorer, right-click on your project, click **Add**, and then click **New Item**.</span></span> <span data-ttu-id="d49c5-133">Adja hozzá a JSON-fájl neve **template.json** tooyour projekt.</span><span class="sxs-lookup"><span data-stu-id="d49c5-133">Add a JSON file called **template.json** tooyour project.</span></span>

2. <span data-ttu-id="d49c5-134">egy szabványos IoT hub toohello tooadd **USA keleti régiója** területet, replace hello tartalmát **template.json** az erőforrás-definíció következő hello.</span><span class="sxs-lookup"><span data-stu-id="d49c5-134">tooadd a standard IoT hub toohello **East US** region, replace hello contents of **template.json** with hello following resource definition.</span></span> <span data-ttu-id="d49c5-135">Hello aktuális IoT Hub-t támogató régiók listáját lásd: [Azure állapot][lnk-status]:</span><span class="sxs-lookup"><span data-stu-id="d49c5-135">For hello current list of regions that support IoT Hub see [Azure Status][lnk-status]:</span></span>

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

3. <span data-ttu-id="d49c5-136">A Megoldáskezelőben kattintson a jobb gombbal a projektre, kattintson a **Hozzáadás**, és kattintson a **új elem**.</span><span class="sxs-lookup"><span data-stu-id="d49c5-136">In Solution Explorer, right-click on your project, click **Add**, and then click **New Item**.</span></span> <span data-ttu-id="d49c5-137">Adja hozzá a JSON-fájl neve **parameters.json** tooyour projekt.</span><span class="sxs-lookup"><span data-stu-id="d49c5-137">Add a JSON file called **parameters.json** tooyour project.</span></span>

4. <span data-ttu-id="d49c5-138">Cserélje le a hello tartalmát **parameters.json** a következő például hello új IoT hub nevét megadó paraméter információ hello **{saját monogramjával} mynewiothub**.</span><span class="sxs-lookup"><span data-stu-id="d49c5-138">Replace hello contents of **parameters.json** with hello following parameter information that sets a name for hello new IoT hub such as **{your initials}mynewiothub**.</span></span> <span data-ttu-id="d49c5-139">hello IoT-központnév egyedinek kell lenniük, a név vagy initials tartalmaznia kell:</span><span class="sxs-lookup"><span data-stu-id="d49c5-139">hello IoT hub name must be globally unique so it should include your name or initials:</span></span>

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

5. <span data-ttu-id="d49c5-140">A **Server Explorer**, csatlakozás tooyour Azure-előfizetés és az Azure Storage fiók nevű tárolókat hozhat létre **sablonok**.</span><span class="sxs-lookup"><span data-stu-id="d49c5-140">In **Server Explorer**, connect tooyour Azure subscription, and in your Azure Storage account create a container called **templates**.</span></span> <span data-ttu-id="d49c5-141">A hello **tulajdonságok** panelen, a set hello **nyilvános olvasási hozzáférés** hello engedélyeinek **sablonok** tároló túl**Blob**.</span><span class="sxs-lookup"><span data-stu-id="d49c5-141">In hello **Properties** panel, set hello **Public Read Access** permissions for hello **templates** container too**Blob**.</span></span>

6. <span data-ttu-id="d49c5-142">A **Server Explorer**, kattintson a jobb gombbal a hello **sablonok** tárolót, majd kattintson **nézet Blob tároló**.</span><span class="sxs-lookup"><span data-stu-id="d49c5-142">In **Server Explorer**, right-click on hello **templates** container and then click **View Blob Container**.</span></span> <span data-ttu-id="d49c5-143">Kattintson a hello **Blob feltöltése** gombra, válassza ki a hello két fájlt, **parameters.json** és **templates.json**, és kattintson a **nyissa meg** tooupload hello JSON-fájlokat toohello **sablonok** tároló.</span><span class="sxs-lookup"><span data-stu-id="d49c5-143">Click hello **Upload Blob** button, select hello two files, **parameters.json** and **templates.json**, and then click **Open** tooupload hello JSON files toohello **templates** container.</span></span> <span data-ttu-id="d49c5-144">hello hello blobok hello JSON-adatokat tartalmazó URL-címei a következők:</span><span class="sxs-lookup"><span data-stu-id="d49c5-144">hello URLs of hello blobs containing hello JSON data are:</span></span>

    ```csharp
    https://{Your storage account name}.blob.core.windows.net/templates/parameters.json
    https://{Your storage account name}.blob.core.windows.net/templates/template.json
    ```
7. <span data-ttu-id="d49c5-145">Adja hozzá a következő metódus tooProgram.cs hello:</span><span class="sxs-lookup"><span data-stu-id="d49c5-145">Add hello following method tooProgram.cs:</span></span>

    ```csharp
    static void CreateIoTHub(ResourceManagementClient client)
    {

    }
    ```

8. <span data-ttu-id="d49c5-146">Adja hozzá a következő kód toohello hello **CreateIoTHub** metódus toosubmit hello sablonnal és paraméterfájlokkal fájlok toohello Azure Resource Manager:</span><span class="sxs-lookup"><span data-stu-id="d49c5-146">Add hello following code toohello **CreateIoTHub** method toosubmit hello template and parameter files toohello Azure Resource Manager:</span></span>

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

9. <span data-ttu-id="d49c5-147">Adja hozzá a következő kód toohello hello **CreateIoTHub** módszer, amelynek hello állapotát és hello kulcsok hello új IoT hub jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="d49c5-147">Add hello following code toohello **CreateIoTHub** method that displays hello status and hello keys for hello new IoT hub:</span></span>

    ```csharp
    string state = createResponse.Properties.ProvisioningState;
    Console.WriteLine("Deployment state: {0}", state);

    if (state != "Succeeded")
    {
      Console.WriteLine("Failed toocreate iothub");
    }
    Console.WriteLine(createResponse.Properties.Outputs);
    ```

## <a name="complete-and-run-hello-application"></a><span data-ttu-id="d49c5-148">Teljes és futtatási hello alkalmazás</span><span class="sxs-lookup"><span data-stu-id="d49c5-148">Complete and run hello application</span></span>

<span data-ttu-id="d49c5-149">Most befejezheti hello alkalmazás hívó hello **CreateIoTHub** metódus előtt most felépíti és futtatja azt.</span><span class="sxs-lookup"><span data-stu-id="d49c5-149">You can now complete hello application by calling hello **CreateIoTHub** method before you build and run it.</span></span>

1. <span data-ttu-id="d49c5-150">Adja hozzá a következő kód toohello vége hello hello **fő** módszert:</span><span class="sxs-lookup"><span data-stu-id="d49c5-150">Add hello following code toohello end of hello **Main** method:</span></span>

    ```csharp
    CreateIoTHub(client);
    Console.ReadLine();
    ```

2. <span data-ttu-id="d49c5-151">Kattintson a **Build** , majd **megoldás**.</span><span class="sxs-lookup"><span data-stu-id="d49c5-151">Click **Build** and then **Build Solution**.</span></span> <span data-ttu-id="d49c5-152">Javíthatja az esetleges hibákat.</span><span class="sxs-lookup"><span data-stu-id="d49c5-152">Correct any errors.</span></span>

3. <span data-ttu-id="d49c5-153">Kattintson a **Debug** , majd **Start Debugging** toorun hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="d49c5-153">Click **Debug** and then **Start Debugging** toorun hello application.</span></span> <span data-ttu-id="d49c5-154">Hello telepítési toorun több percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="d49c5-154">It may take several minutes for hello deployment toorun.</span></span>

4. <span data-ttu-id="d49c5-155">az alkalmazás hozzáadott tooverify hello új IoT hub, látogasson el hello [Azure-portálon] [ lnk-azure-portal] és tekintse meg az erőforrások listáját.</span><span class="sxs-lookup"><span data-stu-id="d49c5-155">tooverify your application added hello new IoT hub, visit hello [Azure portal][lnk-azure-portal] and view your list of resources.</span></span> <span data-ttu-id="d49c5-156">Másik megoldásként használhatja a hello **Get-AzureRmResource** PowerShell-parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="d49c5-156">Alternatively, use hello **Get-AzureRmResource** PowerShell cmdlet.</span></span>

> [!NOTE]
> <span data-ttu-id="d49c5-157">A mintaalkalmazás ad hozzá egy S1 Standard IoT-központot, amelynek kell fizetni.</span><span class="sxs-lookup"><span data-stu-id="d49c5-157">This example application adds an S1 Standard IoT Hub for which you are billed.</span></span> <span data-ttu-id="d49c5-158">Törölheti a hello IoT-központ keresztül hello [Azure-portálon] [ lnk-azure-portal] vagy hello segítségével **Remove-AzureRmResource** PowerShell-parancsmag, amikor elkészült.</span><span class="sxs-lookup"><span data-stu-id="d49c5-158">You can delete hello IoT hub through hello [Azure portal][lnk-azure-portal] or by using hello **Remove-AzureRmResource** PowerShell cmdlet when you are finished.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d49c5-159">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d49c5-159">Next steps</span></span>
<span data-ttu-id="d49c5-160">Most egy IoT-központot egy C# programban egy Azure Resource Manager sablon használatával telepített, akkor érdemes lehet további tooexplore:</span><span class="sxs-lookup"><span data-stu-id="d49c5-160">Now you have deployed an IoT hub using an Azure Resource Manager template with a C# program, you may want tooexplore further:</span></span>

* <span data-ttu-id="d49c5-161">További információ a hello hello képességeit [IoT-központ erőforrás-szolgáltató REST API][lnk-rest-api].</span><span class="sxs-lookup"><span data-stu-id="d49c5-161">Read about hello capabilities of hello [IoT Hub resource provider REST API][lnk-rest-api].</span></span>
* <span data-ttu-id="d49c5-162">Olvasási [Azure Resource Manager áttekintése] [ lnk-azure-rm-overview] Azure Resource Manager hello képességeivel kapcsolatos további toolearn.</span><span class="sxs-lookup"><span data-stu-id="d49c5-162">Read [Azure Resource Manager overview][lnk-azure-rm-overview] toolearn more about hello capabilities of Azure Resource Manager.</span></span>

<span data-ttu-id="d49c5-163">toolearn IoT-központot, fejlesztésével kapcsolatos további hello a következő cikkekben talál:</span><span class="sxs-lookup"><span data-stu-id="d49c5-163">toolearn more about developing for IoT Hub, see hello following articles:</span></span>

* <span data-ttu-id="d49c5-164">[Bevezetés tooC SDK][lnk-c-sdk]</span><span class="sxs-lookup"><span data-stu-id="d49c5-164">[Introduction tooC SDK][lnk-c-sdk]</span></span>
* <span data-ttu-id="d49c5-165">[Az Azure IoT SDK-k][lnk-sdks]</span><span class="sxs-lookup"><span data-stu-id="d49c5-165">[Azure IoT SDKs][lnk-sdks]</span></span>

<span data-ttu-id="d49c5-166">toofurther megismerkedhet az IoT-központ hello képességeit, lásd:</span><span class="sxs-lookup"><span data-stu-id="d49c5-166">toofurther explore hello capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="d49c5-167">[Egy eszköz szimulálva Azure IoT oldala][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="d49c5-167">[Simulating a device with Azure IoT Edge][lnk-iotedge]</span></span>

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
