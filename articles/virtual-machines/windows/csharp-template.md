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
# <a name="deploy-an-azure-virtual-machine-using-c-and-a-resource-manager-template"></a><span data-ttu-id="d65e6-103">Egy Azure virtuális gépen a C# és a Resource Manager-sablon telepítése</span><span class="sxs-lookup"><span data-stu-id="d65e6-103">Deploy an Azure Virtual Machine using C# and a Resource Manager template</span></span>
<span data-ttu-id="d65e6-104">Ez a cikk bemutatja, hogyan toodeploy egy Azure Resource Manager-sablon használatával C#.</span><span class="sxs-lookup"><span data-stu-id="d65e6-104">This article shows you how toodeploy an Azure Resource Manager template using C#.</span></span> <span data-ttu-id="d65e6-105">az Ön által létrehozott hello sablon telepíti a Windows Server rendszert futtató új virtuális hálózatot egyetlen alhálózattal egyetlen virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="d65e6-105">hello template that you create deploys a single virtual machine running Windows Server in a new virtual network with a single subnet.</span></span>

<span data-ttu-id="d65e6-106">Virtuálisgép-erőforrás hello részletes ismertetését lásd: [Azure Resource Manager-sablonokban a virtuális gépek](template-description.md).</span><span class="sxs-lookup"><span data-stu-id="d65e6-106">For a detailed description of hello virtual machine resource, see [Virtual machines in an Azure Resource Manager template](template-description.md).</span></span> <span data-ttu-id="d65e6-107">A sablon erőforrásainak hello kapcsolatos további információkért lásd: [Azure Resource Manager sablonokhoz](../../azure-resource-manager/resource-manager-template-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="d65e6-107">For more information about all hello resources in a template, see [Azure Resource Manager template walkthrough](../../azure-resource-manager/resource-manager-template-walkthrough.md).</span></span>

<span data-ttu-id="d65e6-108">Körülbelül 10 percig toodo szükséges lépéseket.</span><span class="sxs-lookup"><span data-stu-id="d65e6-108">It takes about 10 minutes toodo these steps.</span></span>

## <a name="create-a-visual-studio-project"></a><span data-ttu-id="d65e6-109">Visual Studio-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="d65e6-109">Create a Visual Studio project</span></span>

<span data-ttu-id="d65e6-110">Ebben a lépésben biztosíthatja, hogy telepítve van a Visual Studio és a konzol használt alkalmazás toodeploy hello sablont hoz létre.</span><span class="sxs-lookup"><span data-stu-id="d65e6-110">In this step, you make sure that Visual Studio is installed and you create a console application used toodeploy hello template.</span></span>

1. <span data-ttu-id="d65e6-111">Ha még nem tette meg, telepítse [Visual Studio](https://docs.microsoft.com/visualstudio/install/install-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="d65e6-111">If you haven't already, install [Visual Studio](https://docs.microsoft.com/visualstudio/install/install-visual-studio).</span></span> <span data-ttu-id="d65e6-112">Válassza ki **.NET asztali fejlesztési** hello munkaterhelések lap, és kattintson a **telepítése**.</span><span class="sxs-lookup"><span data-stu-id="d65e6-112">Select **.NET desktop development** on hello Workloads page, and then click **Install**.</span></span> <span data-ttu-id="d65e6-113">Az összefoglaló hello, láthatja, hogy **.NET-keretrendszer 4-4.6 Fejlesztőeszközök** automatikusan ki van jelölve meg.</span><span class="sxs-lookup"><span data-stu-id="d65e6-113">In hello summary, you can see that **.NET Framework 4 - 4.6 development tools** is automatically selected for you.</span></span> <span data-ttu-id="d65e6-114">Ha már telepítette a Visual Studio, a Visual Studio indítója hello segítségével hello .NET munkaterhelés is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="d65e6-114">If you have already installed Visual Studio, you can add hello .NET workload using hello Visual Studio Launcher.</span></span>
2. <span data-ttu-id="d65e6-115">A Visual Studióban kattintson **fájl** > **új** > **projekt**.</span><span class="sxs-lookup"><span data-stu-id="d65e6-115">In Visual Studio, click **File** > **New** > **Project**.</span></span>
3. <span data-ttu-id="d65e6-116">A **sablonok** > **Visual C#**, jelölje be **Konzolalkalmazás (.NET-keretrendszer)**, adja meg *myDotnetProject* hello neveként hello projekt, hello projekt, jelölje be hello helyét, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="d65e6-116">In **Templates** > **Visual C#**, select **Console App (.NET Framework)**, enter *myDotnetProject* for hello name of hello project, select hello location of hello project, and then click **OK**.</span></span>

## <a name="install-hello-packages"></a><span data-ttu-id="d65e6-117">Hello csomagok telepítése</span><span class="sxs-lookup"><span data-stu-id="d65e6-117">Install hello packages</span></span>

<span data-ttu-id="d65e6-118">NuGet-csomagok olyan hello legegyszerűbb módja tooinstall hello könyvtárak kell toofinish ezeket a lépéseket.</span><span class="sxs-lookup"><span data-stu-id="d65e6-118">NuGet packages are hello easiest way tooinstall hello libraries that you need toofinish these steps.</span></span> <span data-ttu-id="d65e6-119">tooget hello szalagtárak, amelyekre szüksége van a Visual Studio, hajtsa végre ezeket a lépéseket:</span><span class="sxs-lookup"><span data-stu-id="d65e6-119">tooget hello libraries that you need in Visual Studio, do these steps:</span></span>

1. <span data-ttu-id="d65e6-120">Kattintson a **eszközök** > **Nuget-Csomagkezelő**, és kattintson a **Csomagkezelő konzol**.</span><span class="sxs-lookup"><span data-stu-id="d65e6-120">Click **Tools** > **Nuget Package Manager**, and then click **Package Manager Console**.</span></span>
2. <span data-ttu-id="d65e6-121">Ezek a parancsok hello konzol írja be:</span><span class="sxs-lookup"><span data-stu-id="d65e6-121">Type these commands in hello console:</span></span>

    ```
    Install-Package Microsoft.Azure.Management.Fluent
    Install-Package WindowsAzure.Storage
    ```

## <a name="create-hello-files"></a><span data-ttu-id="d65e6-122">Hello-fájlok létrehozása</span><span class="sxs-lookup"><span data-stu-id="d65e6-122">Create hello files</span></span>

<span data-ttu-id="d65e6-123">Ebben a lépésben létrehoz egy sablonfájlban hello erőforrások műveletein és paraméterfájl, amely megadja az paraméter értékek toohello sablont.</span><span class="sxs-lookup"><span data-stu-id="d65e6-123">In this step, you create a template file that deploys hello resources and a parameters file that supplies parameter values toohello template.</span></span> <span data-ttu-id="d65e6-124">Is létrehozhat egy engedélyezési fájlt, amely használt tooperform Azure Resource Manager műveletek.</span><span class="sxs-lookup"><span data-stu-id="d65e6-124">You also create an authorization file that is used tooperform Azure Resource Manager operations.</span></span>

### <a name="create-hello-template-file"></a><span data-ttu-id="d65e6-125">Hello sablon fájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="d65e6-125">Create hello template file</span></span>

1. <span data-ttu-id="d65e6-126">A Megoldáskezelőben kattintson a jobb gombbal *myDotnetProject* > **Hozzáadás** > **új elem**, majd válassza ki **szövegfájl** a *Visual C# elemek*.</span><span class="sxs-lookup"><span data-stu-id="d65e6-126">In Solution Explorer, right-click *myDotnetProject* > **Add** > **New Item**, and then select **Text File** in *Visual C# Items*.</span></span> <span data-ttu-id="d65e6-127">Nevű hello fájl *CreateVMTemplate.json*, és kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="d65e6-127">Name hello file *CreateVMTemplate.json*, and then click **Add**.</span></span>
2. <span data-ttu-id="d65e6-128">A kód JSON-toohello létrehozott fájl hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="d65e6-128">Add this JSON code toohello file that you created:</span></span>

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

3. <span data-ttu-id="d65e6-129">Hello CreateVMTemplate.json fájl mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="d65e6-129">Save hello CreateVMTemplate.json file.</span></span>

### <a name="create-hello-parameters-file"></a><span data-ttu-id="d65e6-130">Hello paraméterek fájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="d65e6-130">Create hello parameters file</span></span>

<span data-ttu-id="d65e6-131">hello erőforrás paraméterek hello sablonban definiált toospecify értékeit, létrehozhat paraméterfájl, amely hello értékeket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="d65e6-131">toospecify values for hello resource parameters that are defined in hello template, you create a parameters file that contains hello values.</span></span>

1. <span data-ttu-id="d65e6-132">A Megoldáskezelőben kattintson a jobb gombbal *myDotnetProject* > **Hozzáadás** > **új elem**, majd válassza ki **szövegfájl** a *Visual C# elemek*.</span><span class="sxs-lookup"><span data-stu-id="d65e6-132">In Solution Explorer, right-click *myDotnetProject* > **Add** > **New Item**, and then select **Text File** in *Visual C# Items*.</span></span> <span data-ttu-id="d65e6-133">Nevű hello fájl *Parameters.json*, és kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="d65e6-133">Name hello file *Parameters.json*, and then click **Add**.</span></span>
2. <span data-ttu-id="d65e6-134">A kód JSON-toohello létrehozott fájl hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="d65e6-134">Add this JSON code toohello file that you created:</span></span>

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

4. <span data-ttu-id="d65e6-135">Hello Parameters.json fájl mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="d65e6-135">Save hello Parameters.json file.</span></span>

### <a name="create-hello-authorization-file"></a><span data-ttu-id="d65e6-136">Hello engedélyezési fájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="d65e6-136">Create hello authorization file</span></span>

<span data-ttu-id="d65e6-137">A sablon telepítése előtt győződjön meg arról, hogy rendelkezik-e hozzáférési tooan [Active Directory szolgáltatás egyszerű](../../resource-group-authenticate-service-principal.md).</span><span class="sxs-lookup"><span data-stu-id="d65e6-137">Before you can deploy a template, make sure that you have access tooan [Active Directory service principal](../../resource-group-authenticate-service-principal.md).</span></span> <span data-ttu-id="d65e6-138">Az egyszerű hello szolgáltatást az erőforrás-kezelő kérelmek tooAzure jogkivonat szerez be.</span><span class="sxs-lookup"><span data-stu-id="d65e6-138">From hello service principal, you acquire a token for authenticating requests tooAzure Resource Manager.</span></span> <span data-ttu-id="d65e6-139">Akkor is rögzíteni kell a hello Alkalmazásazonosító, hello hitelesítési kulcs és hello Bérlőazonosító hello engedélyezési fájl szükséges.</span><span class="sxs-lookup"><span data-stu-id="d65e6-139">You should also record hello application ID, hello authentication key, and hello tenant ID that you need in hello authorization file.</span></span>

1. <span data-ttu-id="d65e6-140">A Megoldáskezelőben kattintson a jobb gombbal *myDotnetProject* > **Hozzáadás** > **új elem**, majd válassza ki **szövegfájl** a *Visual C# elemek*.</span><span class="sxs-lookup"><span data-stu-id="d65e6-140">In Solution Explorer, right-click *myDotnetProject* > **Add** > **New Item**, and then select **Text File** in *Visual C# Items*.</span></span> <span data-ttu-id="d65e6-141">Nevű hello fájl *azureauth.properties*, és kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="d65e6-141">Name hello file *azureauth.properties*, and then click **Add**.</span></span>
2. <span data-ttu-id="d65e6-142">Adja hozzá a engedélyezési tulajdonságok:</span><span class="sxs-lookup"><span data-stu-id="d65e6-142">Add these authorization properties:</span></span>

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

    <span data-ttu-id="d65e6-143">Cserélje le  **&lt;előfizetés-azonosító&gt;**  az előfizetés-azonosítóval rendelkező  **&lt;alkalmazásazonosító&gt;**  a hello Active Directory-alkalmazás azonosítóval  **&lt;hitelesítési kulcs&gt;**  hello alkalmazás kulccsal, és  **&lt;bérlőazonosító&gt;**  hello bérlővel azonosítója.</span><span class="sxs-lookup"><span data-stu-id="d65e6-143">Replace **&lt;subscription-id&gt;** with your subscription identifier, **&lt;application-id&gt;** with hello Active Directory application identifier, **&lt;authentication-key&gt;** with hello application key, and **&lt;tenant-id&gt;** with hello tenant identifier.</span></span>

3. <span data-ttu-id="d65e6-144">Hello azureauth.properties fájl mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="d65e6-144">Save hello azureauth.properties file.</span></span>
4. <span data-ttu-id="d65e6-145">A Windows hello teljes elérési útja tooauthorization létrehozott fájl AZURE_AUTH_LOCATION nevű változó beállításához, például a következő PowerShell-parancs használható hello:</span><span class="sxs-lookup"><span data-stu-id="d65e6-145">Set an environment variable in Windows named AZURE_AUTH_LOCATION with hello full path tooauthorization file that you created, for example hello following PowerShell command can be used:</span></span>

    ```
    [Environment]::SetEnvironmentVariable("AZURE_AUTH_LOCATION", "C:\Visual Studio 2017\Projects\myDotnetProject\myDotnetProject\azureauth.properties", "User")
    ```
    
## <a name="create-hello-management-client"></a><span data-ttu-id="d65e6-146">Hello felügyeleti ügyfél létrehozása</span><span class="sxs-lookup"><span data-stu-id="d65e6-146">Create hello management client</span></span>

1. <span data-ttu-id="d65e6-147">Nyissa meg a Program.cs fájl hello hello projekthez létrehozott, és adja hozzá ezek az utasítások toohello meglévő utasítások segítségével a hello fájl felső:</span><span class="sxs-lookup"><span data-stu-id="d65e6-147">Open hello Program.cs file for hello project that you created, and then add these using statements toohello existing statements at top of hello file:</span></span>

    ```
    using Microsoft.Azure.Management.Compute.Fluent;
    using Microsoft.Azure.Management.Compute.Fluent.Models;
    using Microsoft.Azure.Management.Fluent;
    using Microsoft.Azure.Management.ResourceManager.Fluent;
    using Microsoft.Azure.Management.ResourceManager.Fluent.Core;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```

2. <span data-ttu-id="d65e6-148">toocreate hello felügyeleti ügyfél, a kód toohello fő metódus hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="d65e6-148">toocreate hello management client, add this code toohello Main method:</span></span>

    ```
    var credentials = SdkContext.AzureCredentialsFactory
        .FromFile(Environment.GetEnvironmentVariable("AZURE_AUTH_LOCATION"));

    var azure = Azure
        .Configure()
        .WithLogLevel(HttpLoggingDelegatingHandler.Level.Basic)
        .Authenticate(credentials)
        .WithDefaultSubscription();
    ```

## <a name="create-a-resource-group"></a><span data-ttu-id="d65e6-149">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="d65e6-149">Create a resource group</span></span>

<span data-ttu-id="d65e6-150">hello alkalmazás toospecify értékek kód toohello fő metódus hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="d65e6-150">toospecify values for hello application, add code toohello Main method:</span></span>

```
var groupName = "myResourceGroup";
var location = Region.USWest;

var resourceGroup = azure.ResourceGroups.Define(groupName)
    .WithRegion(location)
    .Create();
```

## <a name="create-a-storage-account"></a><span data-ttu-id="d65e6-151">Create a storage account</span><span class="sxs-lookup"><span data-stu-id="d65e6-151">Create a storage account</span></span>

<span data-ttu-id="d65e6-152">egy Azure storage-fiókot a hello sablon és a paraméterek vannak telepítve.</span><span class="sxs-lookup"><span data-stu-id="d65e6-152">hello template and parameters are deployed from a storage account in Azure.</span></span> <span data-ttu-id="d65e6-153">Ebben a lépésben hello fiók létrehozásához, és hello fájlok feltöltése.</span><span class="sxs-lookup"><span data-stu-id="d65e6-153">In this step, you create hello account and upload hello files.</span></span> 

<span data-ttu-id="d65e6-154">toocreate hello fiókot, vegye fel a kód toohello fő metódus:</span><span class="sxs-lookup"><span data-stu-id="d65e6-154">toocreate hello account, add this code toohello Main method:</span></span>

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

## <a name="deploy-hello-template"></a><span data-ttu-id="d65e6-155">Hello sablon üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="d65e6-155">Deploy hello template</span></span>

<span data-ttu-id="d65e6-156">Hello sablon és a paraméterek tárfiókból hello létrehozott központi telepítése.</span><span class="sxs-lookup"><span data-stu-id="d65e6-156">Deploy hello template and parameters from hello storage account that was created.</span></span> 

<span data-ttu-id="d65e6-157">toodeploy hello sablon, a kód toohello fő metódus hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="d65e6-157">toodeploy hello template, add this code toohello Main method:</span></span>

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

## <a name="delete-hello-resources"></a><span data-ttu-id="d65e6-158">Hello erőforrások törlése</span><span class="sxs-lookup"><span data-stu-id="d65e6-158">Delete hello resources</span></span>

<span data-ttu-id="d65e6-159">Mivel az Azure-ban használt erőforrásokhoz van szó, még mindig célszerű toodelete erőforrásokat, amelyek már nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="d65e6-159">Because you are charged for resources used in Azure, it is always good practice toodelete resources that are no longer needed.</span></span> <span data-ttu-id="d65e6-160">Nincs szükség a toodelete az egyes erőforrások elkülönítve egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="d65e6-160">You don’t need toodelete each resource separately from a resource group.</span></span> <span data-ttu-id="d65e6-161">Hello erőforrás csoport törlése és a hozzá tartozó összes erőforrásnak automatikusan törlődnek.</span><span class="sxs-lookup"><span data-stu-id="d65e6-161">Delete hello resource group and all its resources are automatically deleted.</span></span> 

<span data-ttu-id="d65e6-162">toodelete hello erőforrás csoportjában adja hozzá a kódot toohello fő metódus:</span><span class="sxs-lookup"><span data-stu-id="d65e6-162">toodelete hello resource group, add this code toohello Main method:</span></span>

```
azure.ResourceGroups.DeleteByName(groupName);
```

## <a name="run-hello-application"></a><span data-ttu-id="d65e6-163">Hello alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="d65e6-163">Run hello application</span></span>

<span data-ttu-id="d65e6-164">Akkor kell a konzol alkalmazás toorun teljesen a start toofinish körülbelül öt percet is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="d65e6-164">It should take about five minutes for this console application toorun completely from start toofinish.</span></span> 

1. <span data-ttu-id="d65e6-165">toorun hello konzolalkalmazást, kattintson a **Start**.</span><span class="sxs-lookup"><span data-stu-id="d65e6-165">toorun hello console application, click **Start**.</span></span>

2. <span data-ttu-id="d65e6-166">Ahhoz, hogy nyomja le az ENTER **Enter** toostart törlése erőforrásokat, készíthet néhány perc múlva hello erőforrások tooverify hello létrehozását a hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="d65e6-166">Before you press **Enter** toostart deleting resources, you could take a few minutes tooverify hello creation of hello resources in hello Azure portal.</span></span> <span data-ttu-id="d65e6-167">Kattintson a hello telepítési toosee telepítésére vonatkozó állapotadatok hello.</span><span class="sxs-lookup"><span data-stu-id="d65e6-167">Click hello deployment status toosee information about hello deployment.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d65e6-168">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d65e6-168">Next steps</span></span>
* <span data-ttu-id="d65e6-169">Ha problémák merültek fel hello központi telepítés, a következő lépésben lesz toolook: [hibaelhárítás általános az Azure-telepítés az Azure Resource Manager](../../resource-manager-common-deployment-errors.md).</span><span class="sxs-lookup"><span data-stu-id="d65e6-169">If there were issues with hello deployment, a next step would be toolook at [Troubleshoot common Azure deployment errors with Azure Resource Manager](../../resource-manager-common-deployment-errors.md).</span></span>
* <span data-ttu-id="d65e6-170">Megtudhatja, hogyan toodeploy egy virtuális gép és annak támogató forrásokat megtekintésével [központi telepítése egy Azure virtuális gép használata a C#](csharp.md).</span><span class="sxs-lookup"><span data-stu-id="d65e6-170">Learn how toodeploy a virtual machine and its supporting resources by reviewing [Deploy an Azure Virtual Machine Using C#](csharp.md).</span></span>
