---
title: "a virtuális gépet a C# és a Resource Manager-sablon aaaDeploy |} Microsoft Docs"
description: "Ismerje meg, toohow toouse C# és a Resource Manager sablon toodeploy egy Azure virtuális Gépen."
services: virtual-machines-windows
documentationcenter: 
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: bfba66e8-c923-4df2-900a-0c2643b81240
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: davidmu
ms.openlocfilehash: 91d470228cfeed72336b488ffef4dfbb25bc3a40
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-an-azure-virtual-machine-using-c-and-a-resource-manager-template"></a>Egy Azure virtuális gépen a C# és a Resource Manager-sablon telepítése
Ez a cikk bemutatja, hogyan toodeploy egy Azure Resource Manager-sablon használatával C#. az Ön által létrehozott hello sablon telepíti a Windows Server rendszert futtató új virtuális hálózatot egyetlen alhálózattal egyetlen virtuális gép.

Virtuálisgép-erőforrás hello részletes ismertetését lásd: [Azure Resource Manager-sablonokban a virtuális gépek](template-description.md). A sablon erőforrásainak hello kapcsolatos további információkért lásd: [Azure Resource Manager sablonokhoz](../../azure-resource-manager/resource-manager-template-walkthrough.md).

Körülbelül 10 percig toodo szükséges lépéseket.

## <a name="create-a-visual-studio-project"></a>Visual Studio-projekt létrehozása

Ebben a lépésben biztosíthatja, hogy telepítve van a Visual Studio és a konzol használt alkalmazás toodeploy hello sablont hoz létre.

1. Ha még nem tette meg, telepítse [Visual Studio](https://docs.microsoft.com/visualstudio/install/install-visual-studio). Válassza ki **.NET asztali fejlesztési** hello munkaterhelések lap, és kattintson a **telepítése**. Az összefoglaló hello, láthatja, hogy **.NET-keretrendszer 4-4.6 Fejlesztőeszközök** automatikusan ki van jelölve meg. Ha már telepítette a Visual Studio, a Visual Studio indítója hello segítségével hello .NET munkaterhelés is hozzáadhat.
2. A Visual Studióban kattintson **fájl** > **új** > **projekt**.
3. A **sablonok** > **Visual C#**, jelölje be **Konzolalkalmazás (.NET-keretrendszer)**, adja meg *myDotnetProject* hello neveként hello projekt, hello projekt, jelölje be hello helyét, és kattintson a **OK**.

## <a name="install-hello-packages"></a>Hello csomagok telepítése

NuGet-csomagok olyan hello legegyszerűbb módja tooinstall hello könyvtárak kell toofinish ezeket a lépéseket. tooget hello szalagtárak, amelyekre szüksége van a Visual Studio, hajtsa végre ezeket a lépéseket:

1. Kattintson a **eszközök** > **Nuget-Csomagkezelő**, és kattintson a **Csomagkezelő konzol**.
2. Ezek a parancsok hello konzol írja be:

    ```
    Install-Package Microsoft.Azure.Management.Fluent
    Install-Package WindowsAzure.Storage
    ```

## <a name="create-hello-files"></a>Hello-fájlok létrehozása

Ebben a lépésben létrehoz egy sablonfájlban hello erőforrások műveletein és paraméterfájl, amely megadja az paraméter értékek toohello sablont. Is létrehozhat egy engedélyezési fájlt, amely használt tooperform Azure Resource Manager műveletek.

### <a name="create-hello-template-file"></a>Hello sablon fájl létrehozása

1. A Megoldáskezelőben kattintson a jobb gombbal *myDotnetProject* > **Hozzáadás** > **új elem**, majd válassza ki **szövegfájl** a *Visual C# elemek*. Nevű hello fájl *CreateVMTemplate.json*, és kattintson a **Hozzáadás**.
2. A kód JSON-toohello létrehozott fájl hozzáadása:

    ```json
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "adminUsername": { "type": "string" },
        "adminPassword": { "type": "securestring" }
      },
      "variables": {
        "vnetID": "[resourceId('Microsoft.Network/virtualNetworks','myVNet')]", 
        "subnetRef": "[concat(variables('vnetID'),'/subnets/mySubnet')]", 
      },
      "resources": [
        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Network/publicIPAddresses",
          "name": "myPublicIPAddress",
          "location": "[resourceGroup().location]",
          "properties": {
            "publicIPAllocationMethod": "Dynamic",
            "dnsSettings": {
              "domainNameLabel": "myresourcegroupdns1"
            }
          }
        },
        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Network/virtualNetworks",
          "name": "myVNet",
          "location": "[resourceGroup().location]",
          "properties": {
            "addressSpace": { "addressPrefixes": [ "10.0.0.0/16" ] },
            "subnets": [
              {
                "name": "mySubnet",
                "properties": { "addressPrefix": "10.0.0.0/24" }
              }
            ]
          }
        },
        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Network/networkInterfaces",
          "name": "myNic",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[resourceId('Microsoft.Network/publicIPAddresses/', 'myPublicIPAddress')]",
            "[resourceId('Microsoft.Network/virtualNetworks/', 'myVNet')]"
          ],
          "properties": {
            "ipConfigurations": [
              {
                "name": "ipconfig1",
                "properties": {
                  "privateIPAllocationMethod": "Dynamic",
                  "publicIPAddress": { "id": "[resourceId('Microsoft.Network/publicIPAddresses','myPublicIPAddress')]" },
                  "subnet": { "id": "[variables('subnetRef')]" }
                }
              }
            ]
          }
        },
        {
          "apiVersion": "2016-04-30-preview",
          "type": "Microsoft.Compute/virtualMachines",
          "name": "myVM",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[resourceId('Microsoft.Network/networkInterfaces/', 'myNic')]"
          ],
          "properties": {
            "hardwareProfile": { "vmSize": "Standard_DS1" },
            "osProfile": {
              "computerName": "myVM",
              "adminUsername": "[parameters('adminUsername')]",
              "adminPassword": "[parameters('adminPassword')]"
            },
            "storageProfile": {
              "imageReference": {
                "publisher": "MicrosoftWindowsServer",
                "offer": "WindowsServer",
                "sku": "2012-R2-Datacenter",
                "version": "latest"
              },
              "osDisk": {
                "name": "myManagedOSDisk",
                "caching": "ReadWrite",
                "createOption": "FromImage"
              }
            },
            "networkProfile": {
              "networkInterfaces": [
                {
                  "id": "[resourceId('Microsoft.Network/networkInterfaces','myNic')]"
                }
              ]
            }
          }
        }
      ]
    }
    ```

3. Hello CreateVMTemplate.json fájl mentéséhez.

### <a name="create-hello-parameters-file"></a>Hello paraméterek fájl létrehozása

hello erőforrás paraméterek hello sablonban definiált toospecify értékeit, létrehozhat paraméterfájl, amely hello értékeket tartalmaz.

1. A Megoldáskezelőben kattintson a jobb gombbal *myDotnetProject* > **Hozzáadás** > **új elem**, majd válassza ki **szövegfájl** a *Visual C# elemek*. Nevű hello fájl *Parameters.json*, és kattintson a **Hozzáadás**.
2. A kód JSON-toohello létrehozott fájl hozzáadása:

    ```json
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "adminUserName": { "value": "azureuser" },
        "adminPassword": { "value": "Azure12345678" }
      }
    }
    ```

4. Hello Parameters.json fájl mentéséhez.

### <a name="create-hello-authorization-file"></a>Hello engedélyezési fájl létrehozása

A sablon telepítése előtt győződjön meg arról, hogy rendelkezik-e hozzáférési tooan [Active Directory szolgáltatás egyszerű](../../resource-group-authenticate-service-principal.md). Az egyszerű hello szolgáltatást az erőforrás-kezelő kérelmek tooAzure jogkivonat szerez be. Akkor is rögzíteni kell a hello Alkalmazásazonosító, hello hitelesítési kulcs és hello Bérlőazonosító hello engedélyezési fájl szükséges.

1. A Megoldáskezelőben kattintson a jobb gombbal *myDotnetProject* > **Hozzáadás** > **új elem**, majd válassza ki **szövegfájl** a *Visual C# elemek*. Nevű hello fájl *azureauth.properties*, és kattintson a **Hozzáadás**.
2. Adja hozzá a engedélyezési tulajdonságok:

    ```
    subscription=<subscription-id>
    client=<application-id>
    key=<authentication-key>
    tenant=<tenant-id>
    managementURI=https://management.core.windows.net/
    baseURL=https://management.azure.com/
    authURL=https://login.windows.net/
    graphURL=https://graph.windows.net/
    ```

    Cserélje le  **&lt;előfizetés-azonosító&gt;**  az előfizetés-azonosítóval rendelkező  **&lt;alkalmazásazonosító&gt;**  a hello Active Directory-alkalmazás azonosítóval  **&lt;hitelesítési kulcs&gt;**  hello alkalmazás kulccsal, és  **&lt;bérlőazonosító&gt;**  hello bérlővel azonosítója.

3. Hello azureauth.properties fájl mentéséhez.
4. A Windows hello teljes elérési útja tooauthorization létrehozott fájl AZURE_AUTH_LOCATION nevű változó beállításához, például a következő PowerShell-parancs használható hello:

    ```
    [Environment]::SetEnvironmentVariable("AZURE_AUTH_LOCATION", "C:\Visual Studio 2017\Projects\myDotnetProject\myDotnetProject\azureauth.properties", "User")
    ```
    
## <a name="create-hello-management-client"></a>Hello felügyeleti ügyfél létrehozása

1. Nyissa meg a Program.cs fájl hello hello projekthez létrehozott, és adja hozzá ezek az utasítások toohello meglévő utasítások segítségével a hello fájl felső:

    ```
    using Microsoft.Azure.Management.Compute.Fluent;
    using Microsoft.Azure.Management.Compute.Fluent.Models;
    using Microsoft.Azure.Management.Fluent;
    using Microsoft.Azure.Management.ResourceManager.Fluent;
    using Microsoft.Azure.Management.ResourceManager.Fluent.Core;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```

2. toocreate hello felügyeleti ügyfél, a kód toohello fő metódus hozzáadása:

    ```
    var credentials = SdkContext.AzureCredentialsFactory
        .FromFile(Environment.GetEnvironmentVariable("AZURE_AUTH_LOCATION"));

    var azure = Azure
        .Configure()
        .WithLogLevel(HttpLoggingDelegatingHandler.Level.Basic)
        .Authenticate(credentials)
        .WithDefaultSubscription();
    ```

## <a name="create-a-resource-group"></a>Hozzon létre egy erőforráscsoportot

hello alkalmazás toospecify értékek kód toohello fő metódus hozzáadása:

```
var groupName = "myResourceGroup";
var location = Region.USWest;

var resourceGroup = azure.ResourceGroups.Define(groupName)
    .WithRegion(location)
    .Create();
```

## <a name="create-a-storage-account"></a>Create a storage account

egy Azure storage-fiókot a hello sablon és a paraméterek vannak telepítve. Ebben a lépésben hello fiók létrehozásához, és hello fájlok feltöltése. 

toocreate hello fiókot, vegye fel a kód toohello fő metódus:

```
string storageAccountName = SdkContext.RandomResourceName("st", 10);

Console.WriteLine("Creating storage account...");
var storage = azure.StorageAccounts.Define(storageAccountName)
    .WithRegion(Region.USWest)
    .WithExistingResourceGroup(resourceGroup)
    .Create();

var storageKeys = storage.GetKeys();
string storageConnectionString = "DefaultEndpointsProtocol=https;"
    + "AccountName=" + storage.Name
    + ";AccountKey=" + storageKeys[0].Value
    + ";EndpointSuffix=core.windows.net";

var account = CloudStorageAccount.Parse(storageConnectionString);
var serviceClient = account.CreateCloudBlobClient();

Console.WriteLine("Creating container...");
var container = serviceClient.GetContainerReference("templates");
container.CreateIfNotExistsAsync().Wait();
var containerPermissions = new BlobContainerPermissions()
    { PublicAccess = BlobContainerPublicAccessType.Container };
container.SetPermissionsAsync(containerPermissions).Wait();

Console.WriteLine("Uploading template file...");
var templateblob = container.GetBlockBlobReference("CreateVMTemplate.json");
templateblob.UploadFromFile("..\\..\\CreateVMTemplate.json");

Console.WriteLine("Uploading parameters file...");
var paramblob = container.GetBlockBlobReference("Parameters.json");
paramblob.UploadFromFile("..\\..\\Parameters.json");
```

## <a name="deploy-hello-template"></a>Hello sablon üzembe helyezése

Hello sablon és a paraméterek tárfiókból hello létrehozott központi telepítése. 

toodeploy hello sablon, a kód toohello fő metódus hozzáadása:

```
var templatePath = "https://" + storageAccountName + ".blob.core.windows.net/templates/CreateVMTemplate.json";
var paramPath = "https://" + storageAccountName + ".blob.core.windows.net/templates/Parameters.json";
var deployment = azure.Deployments.Define("myDeployment")
    .WithExistingResourceGroup(groupName)
    .WithTemplateLink(templatePath, "1.0.0.0")
    .WithParametersLink(paramPath, "1.0.0.0")
    .WithMode(Microsoft.Azure.Management.ResourceManager.Fluent.Models.DeploymentMode.Incremental)
    .Create();
Console.WriteLine("Press enter toodelete hello resource group...");
Console.ReadLine();
```

## <a name="delete-hello-resources"></a>Hello erőforrások törlése

Mivel az Azure-ban használt erőforrásokhoz van szó, még mindig célszerű toodelete erőforrásokat, amelyek már nem szükséges. Nincs szükség a toodelete az egyes erőforrások elkülönítve egy erőforráscsoportot. Hello erőforrás csoport törlése és a hozzá tartozó összes erőforrásnak automatikusan törlődnek. 

toodelete hello erőforrás csoportjában adja hozzá a kódot toohello fő metódus:

```
azure.ResourceGroups.DeleteByName(groupName);
```

## <a name="run-hello-application"></a>Hello alkalmazás futtatása

Akkor kell a konzol alkalmazás toorun teljesen a start toofinish körülbelül öt percet is igénybe vehet. 

1. toorun hello konzolalkalmazást, kattintson a **Start**.

2. Ahhoz, hogy nyomja le az ENTER **Enter** toostart törlése erőforrásokat, készíthet néhány perc múlva hello erőforrások tooverify hello létrehozását a hello Azure-portálon. Kattintson a hello telepítési toosee telepítésére vonatkozó állapotadatok hello.

## <a name="next-steps"></a>Következő lépések
* Ha problémák merültek fel hello központi telepítés, a következő lépésben lesz toolook: [hibaelhárítás általános az Azure-telepítés az Azure Resource Manager](../../resource-manager-common-deployment-errors.md).
* Megtudhatja, hogyan toodeploy egy virtuális gép és annak támogató forrásokat megtekintésével [központi telepítése egy Azure virtuális gép használata a C#](csharp.md).
