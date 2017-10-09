---
title: "egy Azure IoT hub használatával aaaCreate hello erőforrás-szolgáltató REST API |} Microsoft Docs"
description: "Hogyan toouse hello erőforrás-szolgáltató REST API toocreate egy IoT-központot."
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
ms.openlocfilehash: 98d240ccce47dec13a255bce28943b40f5354ecf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-iot-hub-using-hello-resource-provider-rest-api-net"></a><span data-ttu-id="d56a9-103">Létrehoz egy IoT-központot hello erőforrás-szolgáltató REST API (.NET)</span><span class="sxs-lookup"><span data-stu-id="d56a9-103">Create an IoT hub using hello resource provider REST API (.NET)</span></span>

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

<span data-ttu-id="d56a9-104">Használhatja a hello [IoT-központ erőforrás-szolgáltató REST API] [ lnk-rest-api] toocreate és Azure IoT-központok programozott kezelését.</span><span class="sxs-lookup"><span data-stu-id="d56a9-104">You can use hello [IoT Hub resource provider REST API][lnk-rest-api] toocreate and manage Azure IoT hubs programmatically.</span></span> <span data-ttu-id="d56a9-105">Az oktatóanyag bemutatja, hogyan toouse hello IoT-központ erőforrás-szolgáltató REST API toocreate egy IoT-központot egy C# programban.</span><span class="sxs-lookup"><span data-stu-id="d56a9-105">This tutorial shows you how toouse hello IoT Hub resource provider REST API toocreate an IoT hub from a C# program.</span></span>

> [!NOTE]
> <span data-ttu-id="d56a9-106">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Azure Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="d56a9-106">Azure has two different deployment models for creating and working with resources:  [Azure Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="d56a9-107">Ez a cikk hello Azure Resource Manager telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="d56a9-107">This article covers using hello Azure Resource Manager deployment model.</span></span>

<span data-ttu-id="d56a9-108">toocomplete ebben az oktatóanyagban a következő hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="d56a9-108">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="d56a9-109">Visual Studio 2015 vagy Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="d56a9-109">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="d56a9-110">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="d56a9-110">An active Azure account.</span></span> <br/><span data-ttu-id="d56a9-111">Ha nincs fiókja, néhány perc alatt létrehozhat egy [ingyenes fiókot][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="d56a9-111">If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.</span></span>
* <span data-ttu-id="d56a9-112">[Az Azure PowerShell 1.0] [ lnk-powershell-install] vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="d56a9-112">[Azure PowerShell 1.0][lnk-powershell-install] or later.</span></span>

[!INCLUDE [iot-hub-prepare-resource-manager](../../includes/iot-hub-prepare-resource-manager.md)]

## <a name="prepare-your-visual-studio-project"></a><span data-ttu-id="d56a9-113">Készítse elő a Visual Studio-projekt</span><span class="sxs-lookup"><span data-stu-id="d56a9-113">Prepare your Visual Studio project</span></span>

1. <span data-ttu-id="d56a9-114">A Visual Studio használatával hello Visual C# klasszikus Windows asztal-projekt létrehozása **Konzolalkalmazás (.NET-keretrendszer)** projektsablon.</span><span class="sxs-lookup"><span data-stu-id="d56a9-114">In Visual Studio, create a Visual C# Windows Classic Desktop project using hello **Console App (.NET Framework)** project template.</span></span> <span data-ttu-id="d56a9-115">Név hello projekt **CreateIoTHubREST**.</span><span class="sxs-lookup"><span data-stu-id="d56a9-115">Name hello project **CreateIoTHubREST**.</span></span>

2. <span data-ttu-id="d56a9-116">A Megoldáskezelőben kattintson a jobb gombbal a projektre, és kattintson a **NuGet-csomagok kezelése**.</span><span class="sxs-lookup"><span data-stu-id="d56a9-116">In Solution Explorer, right-click on your project and then click **Manage NuGet Packages**.</span></span>

3. <span data-ttu-id="d56a9-117">Ellenőrizze a NuGet Package Manager **Include prerelease**, és a hello **Tallózás** lapon keresse meg **Microsoft.Azure.Management.ResourceManager**.</span><span class="sxs-lookup"><span data-stu-id="d56a9-117">In NuGet Package Manager, check **Include prerelease**, and on hello **Browse** page search for **Microsoft.Azure.Management.ResourceManager**.</span></span> <span data-ttu-id="d56a9-118">Jelölje ki azt a hello csomag **telepítése**, a **változások áttekintése** kattintson **OK**, majd kattintson **elfogadom** tooaccept hello licencek.</span><span class="sxs-lookup"><span data-stu-id="d56a9-118">Select hello package, click **Install**, in **Review Changes** click **OK**, then click **I Accept** tooaccept hello licenses.</span></span>

4. <span data-ttu-id="d56a9-119">Keressen rá a NuGet Package Manager **Microsoft.IdentityModel.Clients.ActiveDirectory**.</span><span class="sxs-lookup"><span data-stu-id="d56a9-119">In NuGet Package Manager, search for **Microsoft.IdentityModel.Clients.ActiveDirectory**.</span></span>  <span data-ttu-id="d56a9-120">Kattintson a **telepítése**, a **változások áttekintése** kattintson **OK**, majd kattintson a **elfogadom** tooaccept hello licenc.</span><span class="sxs-lookup"><span data-stu-id="d56a9-120">Click **Install**, in **Review Changes** click **OK**, then click **I Accept** tooaccept hello license.</span></span>

5. <span data-ttu-id="d56a9-121">A productscontract.cs fájlban cserélje le a meglévő hello **használatával** utasítások hello a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="d56a9-121">In Program.cs, replace hello existing **using** statements with hello following code:</span></span>

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

6. <span data-ttu-id="d56a9-122">A program.cs fájlban adja hozzá a statikus változók hello helyőrző értékeket cserélje le a következő hello.</span><span class="sxs-lookup"><span data-stu-id="d56a9-122">In Program.cs, add hello following static variables replacing hello placeholder values.</span></span> <span data-ttu-id="d56a9-123">A feljegyezte **ApplicationId**, **SubscriptionId**, **TenantId**, és **jelszó** az oktatóanyag korábbi.</span><span class="sxs-lookup"><span data-stu-id="d56a9-123">You made a note of **ApplicationId**, **SubscriptionId**, **TenantId**, and **Password** earlier in this tutorial.</span></span> <span data-ttu-id="d56a9-124">**Az erőforráscsoport neve** hello név hello erőforráscsoport hello IoT hub létrehozása során használ.</span><span class="sxs-lookup"><span data-stu-id="d56a9-124">**Resource group name** is hello name of hello resource group you use when you create hello IoT hub.</span></span> <span data-ttu-id="d56a9-125">Használhat egy korábban létező vagy egy új erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="d56a9-125">You can use a pre-existing or a new resource group.</span></span> <span data-ttu-id="d56a9-126">**IoT Hub name** hello hoz létre, mint például az IoT-központ hello neve **MyIoTHub**.</span><span class="sxs-lookup"><span data-stu-id="d56a9-126">**IoT Hub name** is hello name of hello IoT Hub you create, such as **MyIoTHub**.</span></span> <span data-ttu-id="d56a9-127">az IoT hub hello nevének globálisan egyedi kell lennie.</span><span class="sxs-lookup"><span data-stu-id="d56a9-127">hello name of your IoT hub must be globally unique.</span></span> <span data-ttu-id="d56a9-128">**A központi telepítés neve** alhálózatnév hello központi telepítéséhez például a **Deployment_01**.</span><span class="sxs-lookup"><span data-stu-id="d56a9-128">**Deployment name** is a name for hello deployment, such as **Deployment_01**.</span></span>

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

## <a name="use-hello-resource-provider-rest-api-toocreate-an-iot-hub"></a><span data-ttu-id="d56a9-129">Hello erőforrás-szolgáltató REST API-toocreate az IoT-központ használata</span><span class="sxs-lookup"><span data-stu-id="d56a9-129">Use hello resource provider REST API toocreate an IoT hub</span></span>

<span data-ttu-id="d56a9-130">Használjon hello [IoT-központ erőforrás-szolgáltató REST API] [ lnk-rest-api] toocreate az IoT-központ az erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="d56a9-130">Use hello [IoT Hub resource provider REST API][lnk-rest-api] toocreate an IoT hub in your resource group.</span></span> <span data-ttu-id="d56a9-131">Hello erőforrás szolgáltató REST API toomake módosítások tooan meglévő IoT-központ is használható.</span><span class="sxs-lookup"><span data-stu-id="d56a9-131">You can also use hello resource provider REST API toomake changes tooan existing IoT hub.</span></span>

1. <span data-ttu-id="d56a9-132">Adja hozzá a következő metódus tooProgram.cs hello:</span><span class="sxs-lookup"><span data-stu-id="d56a9-132">Add hello following method tooProgram.cs:</span></span>

    ```csharp
    static void CreateIoTHub(string token)
    {

    }
    ```

2. <span data-ttu-id="d56a9-133">Adja hozzá a következő kód toohello hello **CreateIoTHub** metódust.</span><span class="sxs-lookup"><span data-stu-id="d56a9-133">Add hello following code toohello **CreateIoTHub** method.</span></span> <span data-ttu-id="d56a9-134">Ez a kód létrehoz egy **HttpClient** hello hitelesítésére szolgáló jogkivonat hello fejlécekben objektum:</span><span class="sxs-lookup"><span data-stu-id="d56a9-134">This code creates an **HttpClient** object with hello authentication token in hello headers:</span></span>

    ```csharp
    HttpClient client = new HttpClient();
    client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", token);
    ```

3. <span data-ttu-id="d56a9-135">Adja hozzá a következő kód toohello hello **CreateIoTHub** metódust.</span><span class="sxs-lookup"><span data-stu-id="d56a9-135">Add hello following code toohello **CreateIoTHub** method.</span></span> <span data-ttu-id="d56a9-136">Ez a kód hello IoT hub toocreate ismerteti, és létrehoz egy JSON-megjelenítés.</span><span class="sxs-lookup"><span data-stu-id="d56a9-136">This code describes hello IoT hub toocreate and generates a JSON representation.</span></span> <span data-ttu-id="d56a9-137">Hello jelenlegi listája támogatja az IoT Hub-helyeken: [Azure állapot][lnk-status]:</span><span class="sxs-lookup"><span data-stu-id="d56a9-137">For hello current list of locations that support IoT Hub see [Azure Status][lnk-status]:</span></span>

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

4. <span data-ttu-id="d56a9-138">Adja hozzá a következő kód toohello hello **CreateIoTHub** metódust.</span><span class="sxs-lookup"><span data-stu-id="d56a9-138">Add hello following code toohello **CreateIoTHub** method.</span></span> <span data-ttu-id="d56a9-139">Ez a kód elküldi a hello REST kérelem tooAzure.</span><span class="sxs-lookup"><span data-stu-id="d56a9-139">This code submits hello REST request tooAzure.</span></span> <span data-ttu-id="d56a9-140">hello kódot, majd ellenőrzi hello választ, és lekéri a használhatja hello szolgáltatástelepítési feladat állapotának toomonitor hello hello URL-címe:</span><span class="sxs-lookup"><span data-stu-id="d56a9-140">hello code then checks hello response and retrieves hello URL you can use toomonitor hello state of hello deployment task:</span></span>

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

5. <span data-ttu-id="d56a9-141">Adja hozzá a következő kód toohello vége hello hello **CreateIoTHub** metódust.</span><span class="sxs-lookup"><span data-stu-id="d56a9-141">Add hello following code toohello end of hello **CreateIoTHub** method.</span></span> <span data-ttu-id="d56a9-142">Ezt a kódot használja hello **asyncStatusUri** az előző lépésben toowait hello hello telepítési toocomplete beolvasni címe:</span><span class="sxs-lookup"><span data-stu-id="d56a9-142">This code uses hello **asyncStatusUri** address retrieved in hello previous step toowait for hello deployment toocomplete:</span></span>

    ```csharp
    string body;
    do
    {
      Thread.Sleep(10000);
      HttpResponseMessage deploymentstatus = client.GetAsync(asyncStatusUri).Result;
      body = deploymentstatus.Content.ReadAsStringAsync().Result;
    } while (body == "{\"status\":\"Running\"}");
    ```

6. <span data-ttu-id="d56a9-143">Adja hozzá a következő kód toohello vége hello hello **CreateIoTHub** metódust.</span><span class="sxs-lookup"><span data-stu-id="d56a9-143">Add hello following code toohello end of hello **CreateIoTHub** method.</span></span> <span data-ttu-id="d56a9-144">Ez a kód lekéri hello IoT-központ létre, és kiírja őket toohello konzol hello kulcsait:</span><span class="sxs-lookup"><span data-stu-id="d56a9-144">This code retrieves hello keys of hello IoT hub you created and prints them toohello console:</span></span>

    ```csharp
    var listKeysUri = string.Format("https://management.azure.com/subscriptions/{0}/resourceGroups/{1}/providers/Microsoft.Devices/IotHubs/{2}/IoTHubKeys/listkeys?api-version=2016-02-03", subscriptionId, rgName, iotHubName);
    var keysresults = client.PostAsync(listKeysUri, null).Result;

    Console.WriteLine("Keys: {0}", keysresults.Content.ReadAsStringAsync().Result);
    ```

## <a name="complete-and-run-hello-application"></a><span data-ttu-id="d56a9-145">Teljes és futtatási hello alkalmazás</span><span class="sxs-lookup"><span data-stu-id="d56a9-145">Complete and run hello application</span></span>

<span data-ttu-id="d56a9-146">Most befejezheti hello alkalmazás hívó hello **CreateIoTHub** metódus előtt most felépíti és futtatja azt.</span><span class="sxs-lookup"><span data-stu-id="d56a9-146">You can now complete hello application by calling hello **CreateIoTHub** method before you build and run it.</span></span>

1. <span data-ttu-id="d56a9-147">Adja hozzá a következő kód toohello vége hello hello **fő** módszert:</span><span class="sxs-lookup"><span data-stu-id="d56a9-147">Add hello following code toohello end of hello **Main** method:</span></span>

    ```csharp
    CreateIoTHub(token.AccessToken);
    Console.ReadLine();
    ```

2. <span data-ttu-id="d56a9-148">Kattintson a **Build** , majd **megoldás**.</span><span class="sxs-lookup"><span data-stu-id="d56a9-148">Click **Build** and then **Build Solution**.</span></span> <span data-ttu-id="d56a9-149">Javíthatja az esetleges hibákat.</span><span class="sxs-lookup"><span data-stu-id="d56a9-149">Correct any errors.</span></span>

3. <span data-ttu-id="d56a9-150">Kattintson a **Debug** , majd **Start Debugging** toorun hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="d56a9-150">Click **Debug** and then **Start Debugging** toorun hello application.</span></span> <span data-ttu-id="d56a9-151">Hello telepítési toorun több percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="d56a9-151">It may take several minutes for hello deployment toorun.</span></span>

4. <span data-ttu-id="d56a9-152">az alkalmazás által hozzáadott tooverify hello új IoT hub, látogasson el hello [Azure-portálon] [ lnk-azure-portal] és tekintse meg az erőforrások listáját.</span><span class="sxs-lookup"><span data-stu-id="d56a9-152">tooverify that your application added hello new IoT hub, visit hello [Azure portal][lnk-azure-portal] and view your list of resources.</span></span> <span data-ttu-id="d56a9-153">Másik megoldásként használhatja a hello **Get-AzureRmResource** PowerShell-parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="d56a9-153">Alternatively, use hello **Get-AzureRmResource** PowerShell cmdlet.</span></span>

> [!NOTE]
> <span data-ttu-id="d56a9-154">A mintaalkalmazás ad hozzá egy S1 Standard IoT-központot, amelynek kell fizetni.</span><span class="sxs-lookup"><span data-stu-id="d56a9-154">This example application adds an S1 Standard IoT Hub for which you are billed.</span></span> <span data-ttu-id="d56a9-155">Ha elkészült, törölheti a hello IoT-központ keresztül hello [Azure-portálon] [ lnk-azure-portal] vagy hello segítségével **Remove-AzureRmResource** PowerShell-parancsmag, amikor elkészült.</span><span class="sxs-lookup"><span data-stu-id="d56a9-155">When you are finished, you can delete hello IoT hub through hello [Azure portal][lnk-azure-portal] or by using hello **Remove-AzureRmResource** PowerShell cmdlet when you are finished.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d56a9-156">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d56a9-156">Next steps</span></span>
<span data-ttu-id="d56a9-157">Most már telepítette az IoT-központ hello erőforrás-szolgáltató REST API-t használ, érdemes lehet további tooexplore:</span><span class="sxs-lookup"><span data-stu-id="d56a9-157">Now you have deployed an IoT hub using hello resource provider REST API, you may want tooexplore further:</span></span>

* <span data-ttu-id="d56a9-158">További információ a hello hello képességeit [IoT-központ erőforrás-szolgáltató REST API][lnk-rest-api].</span><span class="sxs-lookup"><span data-stu-id="d56a9-158">Read about hello capabilities of hello [IoT Hub resource provider REST API][lnk-rest-api].</span></span>
* <span data-ttu-id="d56a9-159">Olvasási [Azure Resource Manager áttekintése] [ lnk-azure-rm-overview] Azure Resource Manager hello képességeivel kapcsolatos további toolearn.</span><span class="sxs-lookup"><span data-stu-id="d56a9-159">Read [Azure Resource Manager overview][lnk-azure-rm-overview] toolearn more about hello capabilities of Azure Resource Manager.</span></span>

<span data-ttu-id="d56a9-160">toolearn IoT-központot, fejlesztésével kapcsolatos további hello a következő cikkekben talál:</span><span class="sxs-lookup"><span data-stu-id="d56a9-160">toolearn more about developing for IoT Hub, see hello following articles:</span></span>

* <span data-ttu-id="d56a9-161">[Bevezetés tooC SDK][lnk-c-sdk]</span><span class="sxs-lookup"><span data-stu-id="d56a9-161">[Introduction tooC SDK][lnk-c-sdk]</span></span>
* <span data-ttu-id="d56a9-162">[Az Azure IoT SDK-k][lnk-sdks]</span><span class="sxs-lookup"><span data-stu-id="d56a9-162">[Azure IoT SDKs][lnk-sdks]</span></span>

<span data-ttu-id="d56a9-163">toofurther megismerkedhet az IoT-központ hello képességeit, lásd:</span><span class="sxs-lookup"><span data-stu-id="d56a9-163">toofurther explore hello capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="d56a9-164">[Egy eszköz szimulálva Azure IoT oldala][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="d56a9-164">[Simulating a device with Azure IoT Edge][lnk-iotedge]</span></span>

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
