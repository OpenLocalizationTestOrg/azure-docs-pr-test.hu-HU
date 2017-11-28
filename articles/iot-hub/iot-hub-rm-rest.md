---
title: "Az erőforrás-szolgáltató REST API-t használó Azure IoT hub létrehozása |} Microsoft Docs"
description: "Hogyan lehet az erőforrás-szolgáltató REST API segítségével létrehoz egy IoT-központot."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 52814ee5-bc10-4abe-9eb2-f8973096c2d8
ms.service: iot-hub
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: e443259507aacbefca141be4c9c1688ab19bf6ec
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="create-an-iot-hub-using-the-resource-provider-rest-api-net"></a><span data-ttu-id="3394b-103">Létrehoz egy IoT-központot, az erőforrás-szolgáltató REST API (.NET) használata</span><span class="sxs-lookup"><span data-stu-id="3394b-103">Create an IoT hub using the resource provider REST API (.NET)</span></span>

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

<span data-ttu-id="3394b-104">Használhatja a [IoT-központ erőforrás-szolgáltató REST API] [ lnk-rest-api] létrehozásához és kezeléséhez programozott módon Azure IoT-központok.</span><span class="sxs-lookup"><span data-stu-id="3394b-104">You can use the [IoT Hub resource provider REST API][lnk-rest-api] to create and manage Azure IoT hubs programmatically.</span></span> <span data-ttu-id="3394b-105">Ez az oktatóanyag bemutatja, hogyan IoT hub létrehozása a C# programban az IoT-központ erőforrás-szolgáltató REST API használatával.</span><span class="sxs-lookup"><span data-stu-id="3394b-105">This tutorial shows you how to use the IoT Hub resource provider REST API to create an IoT hub from a C# program.</span></span>

> [!NOTE]
> <span data-ttu-id="3394b-106">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Azure Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="3394b-106">Azure has two different deployment models for creating and working with resources:  [Azure Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="3394b-107">Ez a cikk az Azure Resource Manager telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="3394b-107">This article covers using the Azure Resource Manager deployment model.</span></span>

<span data-ttu-id="3394b-108">Az oktatóanyag teljesítéséhez a következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="3394b-108">To complete this tutorial, you need the following:</span></span>

* <span data-ttu-id="3394b-109">Visual Studio 2015 vagy Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="3394b-109">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="3394b-110">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="3394b-110">An active Azure account.</span></span> <br/><span data-ttu-id="3394b-111">Ha nincs fiókja, néhány perc alatt létrehozhat egy [ingyenes fiókot][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="3394b-111">If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.</span></span>
* <span data-ttu-id="3394b-112">[Az Azure PowerShell 1.0] [ lnk-powershell-install] vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="3394b-112">[Azure PowerShell 1.0][lnk-powershell-install] or later.</span></span>

[!INCLUDE [iot-hub-prepare-resource-manager](../../includes/iot-hub-prepare-resource-manager.md)]

## <a name="prepare-your-visual-studio-project"></a><span data-ttu-id="3394b-113">Készítse elő a Visual Studio-projekt</span><span class="sxs-lookup"><span data-stu-id="3394b-113">Prepare your Visual Studio project</span></span>

1. <span data-ttu-id="3394b-114">A Visual Studio project using Visual C# klasszikus Windows asztal létrehozása a **Konzolalkalmazás (.NET-keretrendszer)** projektsablon.</span><span class="sxs-lookup"><span data-stu-id="3394b-114">In Visual Studio, create a Visual C# Windows Classic Desktop project using the **Console App (.NET Framework)** project template.</span></span> <span data-ttu-id="3394b-115">Nevet a projektnek **CreateIoTHubREST**.</span><span class="sxs-lookup"><span data-stu-id="3394b-115">Name the project **CreateIoTHubREST**.</span></span>

2. <span data-ttu-id="3394b-116">A Megoldáskezelőben kattintson a jobb gombbal a projektre, és kattintson a **NuGet-csomagok kezelése**.</span><span class="sxs-lookup"><span data-stu-id="3394b-116">In Solution Explorer, right-click on your project and then click **Manage NuGet Packages**.</span></span>

3. <span data-ttu-id="3394b-117">Ellenőrizze a NuGet Package Manager **Include prerelease**, és a a **Tallózás** lapon keresse meg **Microsoft.Azure.Management.ResourceManager**.</span><span class="sxs-lookup"><span data-stu-id="3394b-117">In NuGet Package Manager, check **Include prerelease**, and on the **Browse** page search for **Microsoft.Azure.Management.ResourceManager**.</span></span> <span data-ttu-id="3394b-118">Válassza ki a csomagot, kattintson a **telepítése**, a **változások áttekintése** kattintson **OK**, majd kattintson a **elfogadom** fogadására a licencek.</span><span class="sxs-lookup"><span data-stu-id="3394b-118">Select the package, click **Install**, in **Review Changes** click **OK**, then click **I Accept** to accept the licenses.</span></span>

4. <span data-ttu-id="3394b-119">Keressen rá a NuGet Package Manager **Microsoft.IdentityModel.Clients.ActiveDirectory**.</span><span class="sxs-lookup"><span data-stu-id="3394b-119">In NuGet Package Manager, search for **Microsoft.IdentityModel.Clients.ActiveDirectory**.</span></span>  <span data-ttu-id="3394b-120">Kattintson a **telepítése**, a **változások áttekintése** kattintson **OK**, majd kattintson a **elfogadom** elfogadja a licencfeltételeket.</span><span class="sxs-lookup"><span data-stu-id="3394b-120">Click **Install**, in **Review Changes** click **OK**, then click **I Accept** to accept the license.</span></span>

5. <span data-ttu-id="3394b-121">A productscontract.cs fájlban cserélje le a meglévő **használatával** utasítások a következő kóddal:</span><span class="sxs-lookup"><span data-stu-id="3394b-121">In Program.cs, replace the existing **using** statements with the following code:</span></span>

    ```csharp
    using System;
    using System.Net.Http;
    using System.Net.Http.Headers;
    using System.Text;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.ResourceManager.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Newtonsoft.Json;
    using Microsoft.Rest;
    using System.Linq;
    using System.Threading;
    ```

6. <span data-ttu-id="3394b-122">A program.cs fájlban adja hozzá a következő statikus változó a helyőrző értékeket.</span><span class="sxs-lookup"><span data-stu-id="3394b-122">In Program.cs, add the following static variables replacing the placeholder values.</span></span> <span data-ttu-id="3394b-123">A feljegyezte **ApplicationId**, **SubscriptionId**, **TenantId**, és **jelszó** az oktatóanyag korábbi.</span><span class="sxs-lookup"><span data-stu-id="3394b-123">You made a note of **ApplicationId**, **SubscriptionId**, **TenantId**, and **Password** earlier in this tutorial.</span></span> <span data-ttu-id="3394b-124">**Az erőforráscsoport neve** az IoT hub létrehozása során használ az erőforráscsoport neve.</span><span class="sxs-lookup"><span data-stu-id="3394b-124">**Resource group name** is the name of the resource group you use when you create the IoT hub.</span></span> <span data-ttu-id="3394b-125">Használhat egy korábban létező vagy egy új erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="3394b-125">You can use a pre-existing or a new resource group.</span></span> <span data-ttu-id="3394b-126">**IoT Hub name** hoz létre, mint például az IoT-központ neve **MyIoTHub**.</span><span class="sxs-lookup"><span data-stu-id="3394b-126">**IoT Hub name** is the name of the IoT Hub you create, such as **MyIoTHub**.</span></span> <span data-ttu-id="3394b-127">Az IoT hub nevét globálisan egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="3394b-127">The name of your IoT hub must be globally unique.</span></span> <span data-ttu-id="3394b-128">**A központi telepítés neve** van, mint a központi telepítés nevét **Deployment_01**.</span><span class="sxs-lookup"><span data-stu-id="3394b-128">**Deployment name** is a name for the deployment, such as **Deployment_01**.</span></span>

    ```csharp
    static string applicationId = "{Your ApplicationId}";
    static string subscriptionId = "{Your SubscriptionId}";
    static string tenantId = "{Your TenantId}";
    static string password = "{Your application Password}";

    static string rgName = "{Resource group name}";
    static string iotHubName = "{IoT Hub name including your initials}";
    ```
[!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]

[!INCLUDE [iot-hub-get-access-token](../../includes/iot-hub-get-access-token.md)]

## <a name="use-the-resource-provider-rest-api-to-create-an-iot-hub"></a><span data-ttu-id="3394b-129">Az erőforrás-szolgáltató REST API használatával létrehoz egy IoT-központot</span><span class="sxs-lookup"><span data-stu-id="3394b-129">Use the resource provider REST API to create an IoT hub</span></span>

<span data-ttu-id="3394b-130">Használja a [IoT-központ erőforrás-szolgáltató REST API] [ lnk-rest-api] az IoT-központ létrehozásához az erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="3394b-130">Use the [IoT Hub resource provider REST API][lnk-rest-api] to create an IoT hub in your resource group.</span></span> <span data-ttu-id="3394b-131">Az erőforrás-szolgáltató REST API segítségével módosíthatja egy meglévő IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="3394b-131">You can also use the resource provider REST API to make changes to an existing IoT hub.</span></span>

1. <span data-ttu-id="3394b-132">Adja hozzá a következő metódust a program.cs fájlt:</span><span class="sxs-lookup"><span data-stu-id="3394b-132">Add the following method to Program.cs:</span></span>

    ```csharp
    static void CreateIoTHub(string token)
    {

    }
    ```

2. <span data-ttu-id="3394b-133">Adja hozzá a következő kódot a **CreateIoTHub** metódust.</span><span class="sxs-lookup"><span data-stu-id="3394b-133">Add the following code to the **CreateIoTHub** method.</span></span> <span data-ttu-id="3394b-134">Ez a kód létrehoz egy **HttpClient** a hitelesítési jogkivonat a fejlécekben objektum:</span><span class="sxs-lookup"><span data-stu-id="3394b-134">This code creates an **HttpClient** object with the authentication token in the headers:</span></span>

    ```csharp
    HttpClient client = new HttpClient();
    client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", token);
    ```

3. <span data-ttu-id="3394b-135">Adja hozzá a következő kódot a **CreateIoTHub** metódust.</span><span class="sxs-lookup"><span data-stu-id="3394b-135">Add the following code to the **CreateIoTHub** method.</span></span> <span data-ttu-id="3394b-136">Ez a kód ismerteti az IoT hub létrehozása, és létrehoz egy JSON-megjelenítés.</span><span class="sxs-lookup"><span data-stu-id="3394b-136">This code describes the IoT hub to create and generates a JSON representation.</span></span> <span data-ttu-id="3394b-137">A helyek, amelyek támogatják az IoT-központ aktuális listáját lásd: [Azure állapot][lnk-status]:</span><span class="sxs-lookup"><span data-stu-id="3394b-137">For the current list of locations that support IoT Hub see [Azure Status][lnk-status]:</span></span>

    ```csharp
    var description = new
    {
      name = iotHubName,
      location = "East US",
      sku = new
      {
        name = "S1",
        tier = "Standard",
        capacity = 1
      }
    };

    var json = JsonConvert.SerializeObject(description, Formatting.Indented);
    ```

4. <span data-ttu-id="3394b-138">Adja hozzá a következő kódot a **CreateIoTHub** metódust.</span><span class="sxs-lookup"><span data-stu-id="3394b-138">Add the following code to the **CreateIoTHub** method.</span></span> <span data-ttu-id="3394b-139">Ez a kód elküldi a REST-kérelmet az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="3394b-139">This code submits the REST request to Azure.</span></span> <span data-ttu-id="3394b-140">A kód majd ellenőrzi a választ, és lekéri az URL-cím segítségével a központi telepítés a feladat állapotának figyelése:</span><span class="sxs-lookup"><span data-stu-id="3394b-140">The code then checks the response and retrieves the URL you can use to monitor the state of the deployment task:</span></span>

    ```csharp
    var content = new StringContent(JsonConvert.SerializeObject(description), Encoding.UTF8, "application/json");
    var requestUri = string.Format("https://management.azure.com/subscriptions/{0}/resourcegroups/{1}/providers/Microsoft.devices/IotHubs/{2}?api-version=2016-02-03", subscriptionId, rgName, iotHubName);
    var result = client.PutAsync(requestUri, content).Result;

    if (!result.IsSuccessStatusCode)
    {
      Console.WriteLine("Failed {0}", result.Content.ReadAsStringAsync().Result);
      return;
    }

    var asyncStatusUri = result.Headers.GetValues("Azure-AsyncOperation").First();
    ```

5. <span data-ttu-id="3394b-141">Adja hozzá a következő kódot végén a **CreateIoTHub** metódust.</span><span class="sxs-lookup"><span data-stu-id="3394b-141">Add the following code to the end of the **CreateIoTHub** method.</span></span> <span data-ttu-id="3394b-142">Ezt a kódot használja a **asyncStatusUri** történő várakozás a telepítés befejezéséhez az előző lépésben beolvasott cím:</span><span class="sxs-lookup"><span data-stu-id="3394b-142">This code uses the **asyncStatusUri** address retrieved in the previous step to wait for the deployment to complete:</span></span>

    ```csharp
    string body;
    do
    {
      Thread.Sleep(10000);
      HttpResponseMessage deploymentstatus = client.GetAsync(asyncStatusUri).Result;
      body = deploymentstatus.Content.ReadAsStringAsync().Result;
    } while (body == "{\"status\":\"Running\"}");
    ```

6. <span data-ttu-id="3394b-143">Adja hozzá a következő kódot végén a **CreateIoTHub** metódust.</span><span class="sxs-lookup"><span data-stu-id="3394b-143">Add the following code to the end of the **CreateIoTHub** method.</span></span> <span data-ttu-id="3394b-144">Ez a kód lekéri az IoT hub létrehozása és a konzolablakba írja őket a konzol kulcsának:</span><span class="sxs-lookup"><span data-stu-id="3394b-144">This code retrieves the keys of the IoT hub you created and prints them to the console:</span></span>

    ```csharp
    var listKeysUri = string.Format("https://management.azure.com/subscriptions/{0}/resourceGroups/{1}/providers/Microsoft.Devices/IotHubs/{2}/IoTHubKeys/listkeys?api-version=2016-02-03", subscriptionId, rgName, iotHubName);
    var keysresults = client.PostAsync(listKeysUri, null).Result;

    Console.WriteLine("Keys: {0}", keysresults.Content.ReadAsStringAsync().Result);
    ```

## <a name="complete-and-run-the-application"></a><span data-ttu-id="3394b-145">Végezze el, és futtassa az alkalmazást</span><span class="sxs-lookup"><span data-stu-id="3394b-145">Complete and run the application</span></span>

<span data-ttu-id="3394b-146">Meghívásával hajthatja végre az alkalmazás most már a **CreateIoTHub** metódus előtt most felépíti és futtatja azt.</span><span class="sxs-lookup"><span data-stu-id="3394b-146">You can now complete the application by calling the **CreateIoTHub** method before you build and run it.</span></span>

1. <span data-ttu-id="3394b-147">Adja hozzá a következő kódot végén a **fő** módszert:</span><span class="sxs-lookup"><span data-stu-id="3394b-147">Add the following code to the end of the **Main** method:</span></span>

    ```csharp
    CreateIoTHub(token.AccessToken);
    Console.ReadLine();
    ```

2. <span data-ttu-id="3394b-148">Kattintson a **Build** , majd **megoldás**.</span><span class="sxs-lookup"><span data-stu-id="3394b-148">Click **Build** and then **Build Solution**.</span></span> <span data-ttu-id="3394b-149">Javíthatja az esetleges hibákat.</span><span class="sxs-lookup"><span data-stu-id="3394b-149">Correct any errors.</span></span>

3. <span data-ttu-id="3394b-150">Kattintson a **Debug** , majd **Start Debugging** az alkalmazás futtatásához.</span><span class="sxs-lookup"><span data-stu-id="3394b-150">Click **Debug** and then **Start Debugging** to run the application.</span></span> <span data-ttu-id="3394b-151">A központi telepítés futtatása több percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="3394b-151">It may take several minutes for the deployment to run.</span></span>

4. <span data-ttu-id="3394b-152">Győződjön meg arról, hogy az alkalmazás fel van véve az új IoT hub, látogasson el a [Azure-portálon] [ lnk-azure-portal] és tekintse meg az erőforrások listáját.</span><span class="sxs-lookup"><span data-stu-id="3394b-152">To verify that your application added the new IoT hub, visit the [Azure portal][lnk-azure-portal] and view your list of resources.</span></span> <span data-ttu-id="3394b-153">Másik megoldásként használhatja a **Get-AzureRmResource** PowerShell-parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="3394b-153">Alternatively, use the **Get-AzureRmResource** PowerShell cmdlet.</span></span>

> [!NOTE]
> <span data-ttu-id="3394b-154">A mintaalkalmazás ad hozzá egy S1 Standard IoT-központot, amelynek kell fizetni.</span><span class="sxs-lookup"><span data-stu-id="3394b-154">This example application adds an S1 Standard IoT Hub for which you are billed.</span></span> <span data-ttu-id="3394b-155">Ha elkészült, az IoT hub használatával törölheti a [Azure-portálon] [ lnk-azure-portal] vagy használatával a **Remove-AzureRmResource** PowerShell-parancsmag, amikor elkészült.</span><span class="sxs-lookup"><span data-stu-id="3394b-155">When you are finished, you can delete the IoT hub through the [Azure portal][lnk-azure-portal] or by using the **Remove-AzureRmResource** PowerShell cmdlet when you are finished.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3394b-156">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3394b-156">Next steps</span></span>
<span data-ttu-id="3394b-157">Most telepítette az IoT-központ az erőforrás-szolgáltató REST API-t használ, érdemes lehet további felfedezése:</span><span class="sxs-lookup"><span data-stu-id="3394b-157">Now you have deployed an IoT hub using the resource provider REST API, you may want to explore further:</span></span>

* <span data-ttu-id="3394b-158">Olvassa el a képességeit a [IoT-központ erőforrás-szolgáltató REST API][lnk-rest-api].</span><span class="sxs-lookup"><span data-stu-id="3394b-158">Read about the capabilities of the [IoT Hub resource provider REST API][lnk-rest-api].</span></span>
* <span data-ttu-id="3394b-159">Olvasási [Azure Resource Manager áttekintése] [ lnk-azure-rm-overview] tudhat meg többet az Azure Resource Manager képességeit.</span><span class="sxs-lookup"><span data-stu-id="3394b-159">Read [Azure Resource Manager overview][lnk-azure-rm-overview] to learn more about the capabilities of Azure Resource Manager.</span></span>

<span data-ttu-id="3394b-160">Az IoT-központ fejlesztésével kapcsolatos további tudnivalókért tekintse meg a következő cikkeket:</span><span class="sxs-lookup"><span data-stu-id="3394b-160">To learn more about developing for IoT Hub, see the following articles:</span></span>

* <span data-ttu-id="3394b-161">[C SDK bemutatása][lnk-c-sdk]</span><span class="sxs-lookup"><span data-stu-id="3394b-161">[Introduction to C SDK][lnk-c-sdk]</span></span>
* <span data-ttu-id="3394b-162">[Az Azure IoT SDK-k][lnk-sdks]</span><span class="sxs-lookup"><span data-stu-id="3394b-162">[Azure IoT SDKs][lnk-sdks]</span></span>

<span data-ttu-id="3394b-163">Az IoT-központ képességeit további megismeréséhez lásd:</span><span class="sxs-lookup"><span data-stu-id="3394b-163">To further explore the capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="3394b-164">[Egy eszköz szimulálva Azure IoT oldala][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="3394b-164">[Simulating a device with Azure IoT Edge][lnk-iotedge]</span></span>

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-azure-portal]: https://portal.azure.com/
[lnk-status]: https://azure.microsoft.com/status/
[lnk-powershell-install]: https://docs.microsoft.com/powershell/azure/install-azurerm-ps
[lnk-rest-api]: https://docs.microsoft.com/rest/api/iothub/iothubresource
[lnk-azure-rm-overview]: ../azure-resource-manager/resource-group-overview.md

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
