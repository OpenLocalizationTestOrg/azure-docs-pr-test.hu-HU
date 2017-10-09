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
# <a name="create-an-iot-hub-using-hello-resource-provider-rest-api-net"></a>Létrehoz egy IoT-központot hello erőforrás-szolgáltató REST API (.NET)

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

Használhatja a hello [IoT-központ erőforrás-szolgáltató REST API] [ lnk-rest-api] toocreate és Azure IoT-központok programozott kezelését. Az oktatóanyag bemutatja, hogyan toouse hello IoT-központ erőforrás-szolgáltató REST API toocreate egy IoT-központot egy C# programban.

> [!NOTE]
> Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Azure Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md).  Ez a cikk hello Azure Resource Manager telepítési modell használatát bemutatja.

toocomplete ebben az oktatóanyagban a következő hello szüksége:

* Visual Studio 2015 vagy Visual Studio 2017.
* Aktív Azure-fiók. <br/>Ha nincs fiókja, néhány perc alatt létrehozhat egy [ingyenes fiókot][lnk-free-trial].
* [Az Azure PowerShell 1.0] [ lnk-powershell-install] vagy újabb.

[!INCLUDE [iot-hub-prepare-resource-manager](../../includes/iot-hub-prepare-resource-manager.md)]

## <a name="prepare-your-visual-studio-project"></a>Készítse elő a Visual Studio-projekt

1. A Visual Studio használatával hello Visual C# klasszikus Windows asztal-projekt létrehozása **Konzolalkalmazás (.NET-keretrendszer)** projektsablon. Név hello projekt **CreateIoTHubREST**.

2. A Megoldáskezelőben kattintson a jobb gombbal a projektre, és kattintson a **NuGet-csomagok kezelése**.

3. Ellenőrizze a NuGet Package Manager **Include prerelease**, és a hello **Tallózás** lapon keresse meg **Microsoft.Azure.Management.ResourceManager**. Jelölje ki azt a hello csomag **telepítése**, a **változások áttekintése** kattintson **OK**, majd kattintson **elfogadom** tooaccept hello licencek.

4. Keressen rá a NuGet Package Manager **Microsoft.IdentityModel.Clients.ActiveDirectory**.  Kattintson a **telepítése**, a **változások áttekintése** kattintson **OK**, majd kattintson a **elfogadom** tooaccept hello licenc.

5. A productscontract.cs fájlban cserélje le a meglévő hello **használatával** utasítások hello a következő kódot:

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

6. A program.cs fájlban adja hozzá a statikus változók hello helyőrző értékeket cserélje le a következő hello. A feljegyezte **ApplicationId**, **SubscriptionId**, **TenantId**, és **jelszó** az oktatóanyag korábbi. **Az erőforráscsoport neve** hello név hello erőforráscsoport hello IoT hub létrehozása során használ. Használhat egy korábban létező vagy egy új erőforráscsoportot. **IoT Hub name** hello hoz létre, mint például az IoT-központ hello neve **MyIoTHub**. az IoT hub hello nevének globálisan egyedi kell lennie. **A központi telepítés neve** alhálózatnév hello központi telepítéséhez például a **Deployment_01**.

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

## <a name="use-hello-resource-provider-rest-api-toocreate-an-iot-hub"></a>Hello erőforrás-szolgáltató REST API-toocreate az IoT-központ használata

Használjon hello [IoT-központ erőforrás-szolgáltató REST API] [ lnk-rest-api] toocreate az IoT-központ az erőforráscsoportban. Hello erőforrás szolgáltató REST API toomake módosítások tooan meglévő IoT-központ is használható.

1. Adja hozzá a következő metódus tooProgram.cs hello:

    ```csharp
    static void CreateIoTHub(string token)
    {

    }
    ```

2. Adja hozzá a következő kód toohello hello **CreateIoTHub** metódust. Ez a kód létrehoz egy **HttpClient** hello hitelesítésére szolgáló jogkivonat hello fejlécekben objektum:

    ```csharp
    HttpClient client = new HttpClient();
    client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", token);
    ```

3. Adja hozzá a következő kód toohello hello **CreateIoTHub** metódust. Ez a kód hello IoT hub toocreate ismerteti, és létrehoz egy JSON-megjelenítés. Hello jelenlegi listája támogatja az IoT Hub-helyeken: [Azure állapot][lnk-status]:

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

4. Adja hozzá a következő kód toohello hello **CreateIoTHub** metódust. Ez a kód elküldi a hello REST kérelem tooAzure. hello kódot, majd ellenőrzi hello választ, és lekéri a használhatja hello szolgáltatástelepítési feladat állapotának toomonitor hello hello URL-címe:

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

5. Adja hozzá a következő kód toohello vége hello hello **CreateIoTHub** metódust. Ezt a kódot használja hello **asyncStatusUri** az előző lépésben toowait hello hello telepítési toocomplete beolvasni címe:

    ```csharp
    string body;
    do
    {
      Thread.Sleep(10000);
      HttpResponseMessage deploymentstatus = client.GetAsync(asyncStatusUri).Result;
      body = deploymentstatus.Content.ReadAsStringAsync().Result;
    } while (body == "{\"status\":\"Running\"}");
    ```

6. Adja hozzá a következő kód toohello vége hello hello **CreateIoTHub** metódust. Ez a kód lekéri hello IoT-központ létre, és kiírja őket toohello konzol hello kulcsait:

    ```csharp
    var listKeysUri = string.Format("https://management.azure.com/subscriptions/{0}/resourceGroups/{1}/providers/Microsoft.Devices/IotHubs/{2}/IoTHubKeys/listkeys?api-version=2016-02-03", subscriptionId, rgName, iotHubName);
    var keysresults = client.PostAsync(listKeysUri, null).Result;

    Console.WriteLine("Keys: {0}", keysresults.Content.ReadAsStringAsync().Result);
    ```

## <a name="complete-and-run-hello-application"></a>Teljes és futtatási hello alkalmazás

Most befejezheti hello alkalmazás hívó hello **CreateIoTHub** metódus előtt most felépíti és futtatja azt.

1. Adja hozzá a következő kód toohello vége hello hello **fő** módszert:

    ```csharp
    CreateIoTHub(token.AccessToken);
    Console.ReadLine();
    ```

2. Kattintson a **Build** , majd **megoldás**. Javíthatja az esetleges hibákat.

3. Kattintson a **Debug** , majd **Start Debugging** toorun hello alkalmazás. Hello telepítési toorun több percig is eltarthat.

4. az alkalmazás által hozzáadott tooverify hello új IoT hub, látogasson el hello [Azure-portálon] [ lnk-azure-portal] és tekintse meg az erőforrások listáját. Másik megoldásként használhatja a hello **Get-AzureRmResource** PowerShell-parancsmagot.

> [!NOTE]
> A mintaalkalmazás ad hozzá egy S1 Standard IoT-központot, amelynek kell fizetni. Ha elkészült, törölheti a hello IoT-központ keresztül hello [Azure-portálon] [ lnk-azure-portal] vagy hello segítségével **Remove-AzureRmResource** PowerShell-parancsmag, amikor elkészült.

## <a name="next-steps"></a>Következő lépések
Most már telepítette az IoT-központ hello erőforrás-szolgáltató REST API-t használ, érdemes lehet további tooexplore:

* További információ a hello hello képességeit [IoT-központ erőforrás-szolgáltató REST API][lnk-rest-api].
* Olvasási [Azure Resource Manager áttekintése] [ lnk-azure-rm-overview] Azure Resource Manager hello képességeivel kapcsolatos további toolearn.

toolearn IoT-központot, fejlesztésével kapcsolatos további hello a következő cikkekben talál:

* [Bevezetés tooC SDK][lnk-c-sdk]
* [Az Azure IoT SDK-k][lnk-sdks]

toofurther megismerkedhet az IoT-központ hello képességeit, lásd:

* [Egy eszköz szimulálva Azure IoT oldala][lnk-iotedge]

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
