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
# <a name="create-an-iot-hub-using-azure-resource-manager-template-net"></a>Létrehoz egy IoT-központot, Azure Resource Manager sablonnal (.NET)

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

Használhatja az Azure Resource Manager toocreate és Azure IoT-központok programozott kezelését. Az oktatóanyag bemutatja, hogyan toouse az Azure Resource Manager sablon toocreate egy IoT-központot egy C# programban.

> [!NOTE]
> Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Azure Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md).  Ez a cikk hello Azure Resource Manager telepítési modell használatát bemutatja.

toocomplete ebben az oktatóanyagban a következő hello szüksége:

* Visual Studio 2015 vagy Visual Studio 2017.
* Aktív Azure-fiók. <br/>Ha nincs fiókja, néhány perc alatt létrehozhat egy [ingyenes fiókot][lnk-free-trial].
* Egy [Azure Storage-fiók] [ lnk-storage-account] az Azure Resource Manager sablon fájlok tárolására.
* [Az Azure PowerShell 1.0] [ lnk-powershell-install] vagy újabb.

[!INCLUDE [iot-hub-prepare-resource-manager](../../includes/iot-hub-prepare-resource-manager.md)]

## <a name="prepare-your-visual-studio-project"></a>Készítse elő a Visual Studio-projekt

1. A Visual Studio használatával hello Visual C# klasszikus Windows asztal-projekt létrehozása **Konzolalkalmazás (.NET-keretrendszer)** projektsablon. Név hello projekt **CreateIoTHub**.

2. A Megoldáskezelőben kattintson a jobb gombbal a projektre, és kattintson a **NuGet-csomagok kezelése**.

3. Ellenőrizze a NuGet Package Manager **Include prerelease**, és a hello **Tallózás** lapon keresse meg **Microsoft.Azure.Management.ResourceManager**. Jelölje ki azt a hello csomag **telepítése**, a **változások áttekintése** kattintson **OK**, majd kattintson **elfogadom** tooaccept hello licencek.

4. Keressen rá a NuGet Package Manager **Microsoft.IdentityModel.Clients.ActiveDirectory**.  Kattintson a **telepítése**, a **változások áttekintése** kattintson **OK**, majd kattintson a **elfogadom** tooaccept hello licenc.

5. A productscontract.cs fájlban cserélje le a meglévő hello **használatával** utasítások hello a következő kódot:

    ```csharp
    using System;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.ResourceManager.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.Rest;
    ```

6. A program.cs fájlban adja hozzá a statikus változók hello helyőrző értékeket cserélje le a következő hello. A feljegyezte **ApplicationId**, **SubscriptionId**, **TenantId**, és **jelszó** az oktatóanyag korábbi. **Az Azure Storage-fiók neve** van hello hello Azure Storage-fiók neve, az Azure Resource Manager sablon fájlokat tárolja. **Az erőforráscsoport neve** hello név hello erőforráscsoport hello IoT hub létrehozása során használ. hello neve lehet egy már meglévő vagy új erőforráscsoportot. **A központi telepítés neve** alhálózatnév hello központi telepítéséhez például a **Deployment_01**.

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

## <a name="submit-a-template-toocreate-an-iot-hub"></a>Küldje el a sablon toocreate az IoT-központ

A JSON sablonnal és paraméterfájlokkal fájl toocreate az IoT-központ használja az erőforráscsoportban. Az Azure Resource Manager sablon toomake módosítások tooan meglévő IoT-központ is használható.

1. A Megoldáskezelőben kattintson a jobb gombbal a projektre, kattintson a **Hozzáadás**, és kattintson a **új elem**. Adja hozzá a JSON-fájl neve **template.json** tooyour projekt.

2. egy szabványos IoT hub toohello tooadd **USA keleti régiója** területet, replace hello tartalmát **template.json** az erőforrás-definíció következő hello. Hello aktuális IoT Hub-t támogató régiók listáját lásd: [Azure állapot][lnk-status]:

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

3. A Megoldáskezelőben kattintson a jobb gombbal a projektre, kattintson a **Hozzáadás**, és kattintson a **új elem**. Adja hozzá a JSON-fájl neve **parameters.json** tooyour projekt.

4. Cserélje le a hello tartalmát **parameters.json** a következő például hello új IoT hub nevét megadó paraméter információ hello **{saját monogramjával} mynewiothub**. hello IoT-központnév egyedinek kell lenniük, a név vagy initials tartalmaznia kell:

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

5. A **Server Explorer**, csatlakozás tooyour Azure-előfizetés és az Azure Storage fiók nevű tárolókat hozhat létre **sablonok**. A hello **tulajdonságok** panelen, a set hello **nyilvános olvasási hozzáférés** hello engedélyeinek **sablonok** tároló túl**Blob**.

6. A **Server Explorer**, kattintson a jobb gombbal a hello **sablonok** tárolót, majd kattintson **nézet Blob tároló**. Kattintson a hello **Blob feltöltése** gombra, válassza ki a hello két fájlt, **parameters.json** és **templates.json**, és kattintson a **nyissa meg** tooupload hello JSON-fájlokat toohello **sablonok** tároló. hello hello blobok hello JSON-adatokat tartalmazó URL-címei a következők:

    ```csharp
    https://{Your storage account name}.blob.core.windows.net/templates/parameters.json
    https://{Your storage account name}.blob.core.windows.net/templates/template.json
    ```
7. Adja hozzá a következő metódus tooProgram.cs hello:

    ```csharp
    static void CreateIoTHub(ResourceManagementClient client)
    {

    }
    ```

8. Adja hozzá a következő kód toohello hello **CreateIoTHub** metódus toosubmit hello sablonnal és paraméterfájlokkal fájlok toohello Azure Resource Manager:

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

9. Adja hozzá a következő kód toohello hello **CreateIoTHub** módszer, amelynek hello állapotát és hello kulcsok hello új IoT hub jeleníti meg:

    ```csharp
    string state = createResponse.Properties.ProvisioningState;
    Console.WriteLine("Deployment state: {0}", state);

    if (state != "Succeeded")
    {
      Console.WriteLine("Failed toocreate iothub");
    }
    Console.WriteLine(createResponse.Properties.Outputs);
    ```

## <a name="complete-and-run-hello-application"></a>Teljes és futtatási hello alkalmazás

Most befejezheti hello alkalmazás hívó hello **CreateIoTHub** metódus előtt most felépíti és futtatja azt.

1. Adja hozzá a következő kód toohello vége hello hello **fő** módszert:

    ```csharp
    CreateIoTHub(client);
    Console.ReadLine();
    ```

2. Kattintson a **Build** , majd **megoldás**. Javíthatja az esetleges hibákat.

3. Kattintson a **Debug** , majd **Start Debugging** toorun hello alkalmazás. Hello telepítési toorun több percig is eltarthat.

4. az alkalmazás hozzáadott tooverify hello új IoT hub, látogasson el hello [Azure-portálon] [ lnk-azure-portal] és tekintse meg az erőforrások listáját. Másik megoldásként használhatja a hello **Get-AzureRmResource** PowerShell-parancsmagot.

> [!NOTE]
> A mintaalkalmazás ad hozzá egy S1 Standard IoT-központot, amelynek kell fizetni. Törölheti a hello IoT-központ keresztül hello [Azure-portálon] [ lnk-azure-portal] vagy hello segítségével **Remove-AzureRmResource** PowerShell-parancsmag, amikor elkészült.

## <a name="next-steps"></a>Következő lépések
Most egy IoT-központot egy C# programban egy Azure Resource Manager sablon használatával telepített, akkor érdemes lehet további tooexplore:

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
[lnk-storage-account]:../storage/common/storage-create-storage-account.md

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
