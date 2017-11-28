---
title: "Adatok folyamatok létrehozása az Azure .NET SDK használatával |} Microsoft Docs"
description: "Megtudhatja, hogyan programozott módon létrehozása, felügyelete és kezelése az Azure adat-előállítók Data Factory SDK használatával."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: b0a357be-3040-4789-831e-0d0a32a0bda5
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: 9d9dac75321c5d4e079f49320d9b7c6f56e48754
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="create-monitor-and-manage-azure-data-factories-using-azure-data-factory-net-sdk"></a><span data-ttu-id="70179-103">Hozzon létre, felügyelete és kezelése az Azure Data Factory .NET SDK használatával Azure adat-előállítók</span><span class="sxs-lookup"><span data-stu-id="70179-103">Create, monitor, and manage Azure data factories using Azure Data Factory .NET SDK</span></span>
## <a name="overview"></a><span data-ttu-id="70179-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="70179-104">Overview</span></span>
<span data-ttu-id="70179-105">Hozzon létre, figyelése és kezelése az Azure adat-előállítók programozott módon a Data Factory .NET SDK használatával.</span><span class="sxs-lookup"><span data-stu-id="70179-105">You can create, monitor, and manage Azure data factories programmatically using Data Factory .NET SDK.</span></span> <span data-ttu-id="70179-106">A cikkben egy forgatókönyv, amelyeket követve hozzon létre egy minta .NET konzolalkalmazást, amely létrehoz és egy adat-előállító figyeli.</span><span class="sxs-lookup"><span data-stu-id="70179-106">This article contains a walkthrough that you can follow to create a sample .NET console application that creates and monitors a data factory.</span></span> 

> [!NOTE]
> <span data-ttu-id="70179-107">Ez a cikk nem tárgyalja a Data Factory teljes .NET API-ját.</span><span class="sxs-lookup"><span data-stu-id="70179-107">This article does not cover all the Data Factory .NET API.</span></span> <span data-ttu-id="70179-108">Lásd: [Data Factory .NET API-referencia](/dotnet/api/index?view=azuremgmtdatafactories-4.12.1) .NET API-t a Data Factory átfogó dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="70179-108">See [Data Factory .NET API Reference](/dotnet/api/index?view=azuremgmtdatafactories-4.12.1) for comprehensive documentation on .NET API for Data Factory.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="70179-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="70179-109">Prerequisites</span></span>
* <span data-ttu-id="70179-110">Visual Studio 2012, 2013 vagy 2015</span><span class="sxs-lookup"><span data-stu-id="70179-110">Visual Studio 2012 or 2013 or 2015</span></span>
* <span data-ttu-id="70179-111">Töltse le és telepítse [Azure .NET SDK](http://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="70179-111">Download and install [Azure .NET SDK](http://azure.microsoft.com/downloads/).</span></span>
* <span data-ttu-id="70179-112">Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="70179-112">Azure PowerShell.</span></span> <span data-ttu-id="70179-113">Kövesse a [How to install and configure Azure PowerShell](/powershell/azure/overview) (Az Azure PowerShell telepítése és konfigurálása) cikkben foglalt utasításokat az Azure PowerShell telepítéséhez a számítógépre.</span><span class="sxs-lookup"><span data-stu-id="70179-113">Follow instructions in [How to install and configure Azure PowerShell](/powershell/azure/overview) article to install Azure PowerShell on your computer.</span></span> <span data-ttu-id="70179-114">Az Azure PowerShellt egy Azure Active Directory-alkalmazás létrehozására fogjuk használni.</span><span class="sxs-lookup"><span data-stu-id="70179-114">You use Azure PowerShell to create an Azure Active Directory application.</span></span>

### <a name="create-an-application-in-azure-active-directory"></a><span data-ttu-id="70179-115">Alkalmazás létrehozása az Azure Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="70179-115">Create an application in Azure Active Directory</span></span>
<span data-ttu-id="70179-116">Hozzon létre egy Azure Active Directory-alkalmazást, hozza létre az alkalmazás szolgáltatásnevét, és rendelje hozzá a **Data Factory közreműködője** szerepkört.</span><span class="sxs-lookup"><span data-stu-id="70179-116">Create an Azure Active Directory application, create a service principal for the application, and assign it to the **Data Factory Contributor** role.</span></span>

1. <span data-ttu-id="70179-117">Indítsa el a **PowerShellt**.</span><span class="sxs-lookup"><span data-stu-id="70179-117">Launch **PowerShell**.</span></span>
2. <span data-ttu-id="70179-118">Futtassa a következő parancsot, és adja meg az Azure Portalra való bejelentkezéshez használt felhasználónevet és jelszót.</span><span class="sxs-lookup"><span data-stu-id="70179-118">Run the following command and enter the user name and password that you use to sign in to the Azure portal.</span></span>

    ```PowerShell
    Login-AzureRmAccount
    ```
3. <span data-ttu-id="70179-119">Futtassa a következő parancsot a fiókhoz tartozó előfizetések megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="70179-119">Run the following command to view all the subscriptions for this account.</span></span>

    ```PowerShell
    Get-AzureRmSubscription
    ```
4. <span data-ttu-id="70179-120">Futtassa a következő parancsot a használni kívánt előfizetés kiválasztásához.</span><span class="sxs-lookup"><span data-stu-id="70179-120">Run the following command to select the subscription that you want to work with.</span></span> <span data-ttu-id="70179-121">A **&lt;NameOfAzureSubscription**&gt; helyére írja be Azure-előfizetése nevét.</span><span class="sxs-lookup"><span data-stu-id="70179-121">Replace **&lt;NameOfAzureSubscription**&gt; with the name of your Azure subscription.</span></span>

    ```PowerShell
    Get-AzureRmSubscription -SubscriptionName <NameOfAzureSubscription> | Set-AzureRmContext
    ```

   > [!IMPORTANT]
   > <span data-ttu-id="70179-122">Írja le a parancs kimenetében szereplő **SubscriptionId** és **TenantId** paraméter értékét.</span><span class="sxs-lookup"><span data-stu-id="70179-122">Note down **SubscriptionId** and **TenantId** from the output of this command.</span></span>

5. <span data-ttu-id="70179-123">Hozzon létre egy **ADFTutorialResourceGroup** nevű Azure-erőforráscsoportot. Ehhez futtassa a következő parancsot a PowerShellben.</span><span class="sxs-lookup"><span data-stu-id="70179-123">Create an Azure resource group named **ADFTutorialResourceGroup** by running the following command in the PowerShell.</span></span>

    ```PowerShell
    New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"
    ```

    <span data-ttu-id="70179-124">Ha az erőforráscsoport már létezik, adja meg, hogy frissítse azt a rendszer (Y), vagy hagyja változatlanul (N).</span><span class="sxs-lookup"><span data-stu-id="70179-124">If the resource group already exists, you specify whether to update it (Y) or keep it as (N).</span></span>

    <span data-ttu-id="70179-125">Ha másik erőforráscsoportot használ, használja ehelyett saját erőforráscsoportja nevét, amikor az oktatóanyag az ADFTutorialResourceGroup csoportra utal.</span><span class="sxs-lookup"><span data-stu-id="70179-125">If you use a different resource group, you need to use the name of your resource group in place of ADFTutorialResourceGroup in this tutorial.</span></span>
6. <span data-ttu-id="70179-126">Hozzon létre egy Azure Active Directory-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="70179-126">Create an Azure Active Directory application.</span></span>

    ```PowerShell
    $azureAdApplication = New-AzureRmADApplication -DisplayName "ADFDotNetWalkthroughApp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.adfdotnetwalkthroughapp.org/example" -Password "Pass@word1"
    ```

    <span data-ttu-id="70179-127">Ha az alábbi hiba jelenik meg, adjon meg egy másik URL-címet, és futtassa ismét a parancsot.</span><span class="sxs-lookup"><span data-stu-id="70179-127">If you get the following error, specify a different URL and run the command again.</span></span>
    
    ```PowerShell
    Another object with the same value for property identifierUris already exists.
    ```
7. <span data-ttu-id="70179-128">Hozza létre az AD-szolgáltatásnevet.</span><span class="sxs-lookup"><span data-stu-id="70179-128">Create the AD service principal.</span></span>

    ```PowerShell
    New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId
    ```
8. <span data-ttu-id="70179-129">Adja hozzá a szolgáltatásnevet a **Data Factory közreműködője** szerepkörhöz.</span><span class="sxs-lookup"><span data-stu-id="70179-129">Add service principal to the **Data Factory Contributor** role.</span></span>

    ```PowerShell
    New-AzureRmRoleAssignment -RoleDefinitionName "Data Factory Contributor" -ServicePrincipalName $azureAdApplication.ApplicationId.Guid
    ```
9. <span data-ttu-id="70179-130">Szerezze be az alkalmazásazonosítót.</span><span class="sxs-lookup"><span data-stu-id="70179-130">Get the application ID.</span></span>

    ```PowerShell
    $azureAdApplication 
    ```
    <span data-ttu-id="70179-131">Írja le az alkalmazásazonosítót (a parancs kimenetében szereplő applicationID paraméter értéke).</span><span class="sxs-lookup"><span data-stu-id="70179-131">Note down the application ID (applicationID) from the output.</span></span>

<span data-ttu-id="70179-132">A fenti lépések elvégzésével beszereztük az alábbi négy értéket:</span><span class="sxs-lookup"><span data-stu-id="70179-132">You should have following four values from these steps:</span></span>

* <span data-ttu-id="70179-133">Bérlőazonosító</span><span class="sxs-lookup"><span data-stu-id="70179-133">Tenant ID</span></span>
* <span data-ttu-id="70179-134">Előfizetés azonosítója</span><span class="sxs-lookup"><span data-stu-id="70179-134">Subscription ID</span></span>
* <span data-ttu-id="70179-135">Alkalmazásazonosító</span><span class="sxs-lookup"><span data-stu-id="70179-135">Application ID</span></span>
* <span data-ttu-id="70179-136">Jelszó (az első parancsnál adtuk meg)</span><span class="sxs-lookup"><span data-stu-id="70179-136">Password (specified in the first command)</span></span>

## <a name="walkthrough"></a><span data-ttu-id="70179-137">Útmutatás</span><span class="sxs-lookup"><span data-stu-id="70179-137">Walkthrough</span></span>
<span data-ttu-id="70179-138">A forgatókönyv hoz létre egy adat-előállító egy folyamatot, amely tartalmazza a másolási tevékenység.</span><span class="sxs-lookup"><span data-stu-id="70179-138">In the walkthrough, you create a data factory with a pipeline that contains a copy activity.</span></span> <span data-ttu-id="70179-139">A másolási tevékenység során másol adatokat az Azure blob storage a mappa ugyanazon a blob Storage tárolóban egy másik mappába.</span><span class="sxs-lookup"><span data-stu-id="70179-139">The copy activity copies data from a folder in your Azure blob storage to another folder in the same blob storage.</span></span> 

<span data-ttu-id="70179-140">A másolási tevékenység végzi az adatok továbbítását az Azure Data Factoryban.</span><span class="sxs-lookup"><span data-stu-id="70179-140">The Copy Activity performs the data movement in Azure Data Factory.</span></span> <span data-ttu-id="70179-141">A tevékenységet egy globálisan elérhető szolgáltatás működteti, amely biztonságos, megbízható és skálázható módon másolja az adatokat a különböző adattárak között.</span><span class="sxs-lookup"><span data-stu-id="70179-141">The activity is powered by a globally available service that can copy data between various data stores in a secure, reliable, and scalable way.</span></span> <span data-ttu-id="70179-142">A Másolás tevékenységgel kapcsolatos részletekért tekintse meg a [Data Movement Activities](data-factory-data-movement-activities.md) (Adattovábbítási tevékenységek) című cikket.</span><span class="sxs-lookup"><span data-stu-id="70179-142">See [Data Movement Activities](data-factory-data-movement-activities.md) article for details about the Copy Activity.</span></span>

1. <span data-ttu-id="70179-143">Hozzon létre egy a C# nyelvet használó .NET-konzolalkalmazást a Visual Studio 2012/2013/2015 segítségével.</span><span class="sxs-lookup"><span data-stu-id="70179-143">Using Visual Studio 2012/2013/2015, create a C# .NET console application.</span></span>
   1. <span data-ttu-id="70179-144">Indítsa el a **Visual Studio** 2012/2013/2015-at.</span><span class="sxs-lookup"><span data-stu-id="70179-144">Launch **Visual Studio** 2012/2013/2015.</span></span>
   2. <span data-ttu-id="70179-145">Kattintson a **File** (Fájl) menüre, mutasson a **New** (Új) elemre, és kattintson a **Project** (Projekt) lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="70179-145">Click **File**, point to **New**, and click **Project**.</span></span>
   3. <span data-ttu-id="70179-146">Bontsa ki a **Sablonok** lehetőséget, és válassza a **Visual C#** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="70179-146">Expand **Templates**, and select **Visual C#**.</span></span> <span data-ttu-id="70179-147">Ebben az útmutatóban a C#-t fogjuk használni, de bármelyik más .NET-nyelv is alkalmazható.</span><span class="sxs-lookup"><span data-stu-id="70179-147">In this walkthrough, you use C#, but you can use any .NET language.</span></span>
   4. <span data-ttu-id="70179-148">A projekttípusok jobb oldalon megjelenő listájában válassza a **Konzolalkalmazás** elemet.</span><span class="sxs-lookup"><span data-stu-id="70179-148">Select **Console Application** from the list of project types on the right.</span></span>
   5. <span data-ttu-id="70179-149">A Név mezőbe írja be: **DataFactoryAPITestApp**.</span><span class="sxs-lookup"><span data-stu-id="70179-149">Enter **DataFactoryAPITestApp** for the Name.</span></span>
   6. <span data-ttu-id="70179-150">A Hely mezőbe írja be: **C:\ADFGetStarted**.</span><span class="sxs-lookup"><span data-stu-id="70179-150">Select **C:\ADFGetStarted** for the Location.</span></span>
   7. <span data-ttu-id="70179-151">A projekt létrehozásához kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="70179-151">Click **OK** to create the project.</span></span>
2. <span data-ttu-id="70179-152">Kattintson az **Eszközök** elemre, mutasson a **NuGet Package Manager** (NuGet-csomagkezelő) lehetőségre, majd kattintson a **Package Manager Console** (Csomagkezelő konzol) elemre.</span><span class="sxs-lookup"><span data-stu-id="70179-152">Click **Tools**, point to **NuGet Package Manager**, and click **Package Manager Console**.</span></span>
3. <span data-ttu-id="70179-153">A **Csomagkezelő konzolban** hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="70179-153">In the **Package Manager Console**, do the following steps:</span></span>
   1. <span data-ttu-id="70179-154">A Data Factory-csomag telepítéséhez futtassa az alábbi parancsot: `Install-Package Microsoft.Azure.Management.DataFactories`</span><span class="sxs-lookup"><span data-stu-id="70179-154">Run the following command to install Data Factory package: `Install-Package Microsoft.Azure.Management.DataFactories`</span></span>
   2. <span data-ttu-id="70179-155">Futtassa a következő parancsot az Azure Active Directory-csomag telepítéséhez (használja az Active Directory API-t a kódban): `Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.19.208020213`</span><span class="sxs-lookup"><span data-stu-id="70179-155">Run the following command to install Azure Active Directory package (you use Active Directory API in the code): `Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.19.208020213`</span></span>
4. <span data-ttu-id="70179-156">Cserélje le a tartalmát **App.config** fájlt a projektben a következő tartalommal:</span><span class="sxs-lookup"><span data-stu-id="70179-156">Replace the contents of **App.config** file in the project with the following content:</span></span> 
    
    ```xml
    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
        <appSettings>
            <add key="ActiveDirectoryEndpoint" value="https://login.microsoftonline.com/" />
            <add key="ResourceManagerEndpoint" value="https://management.azure.com/" />
            <add key="WindowsManagementUri" value="https://management.core.windows.net/" />

            <add key="ApplicationId" value="your application ID" />
            <add key="Password" value="Password you used while creating the AAD application" />
            <add key="SubscriptionId" value= "Subscription ID" />
            <add key="ActiveDirectoryTenantId" value="Tenant ID" />
        </appSettings>
    </configuration>
    ```
5. <span data-ttu-id="70179-157">Az App.Config fájlban frissítse az értékeket  **&lt;Alkalmazásazonosító&gt;**,  **&lt;jelszó&gt;**,  **&lt;előfizetés Azonosító&gt;**, és  **&lt;bérlői azonosító&gt;**  saját értékekkel.</span><span class="sxs-lookup"><span data-stu-id="70179-157">In the App.Config file, update values for **&lt;Application ID&gt;**, **&lt;Password&gt;**, **&lt;Subscription ID&gt;**, and **&lt;tenant ID&gt;** with your own values.</span></span>
6. <span data-ttu-id="70179-158">Adja hozzá a következő **használatával** utasítást, hogy a **Program.cs** fájlt a projektben.</span><span class="sxs-lookup"><span data-stu-id="70179-158">Add the following **using** statements to the **Program.cs** file in the project.</span></span>

    ```csharp
    using System.Configuration;
    using System.Collections.ObjectModel;
    using System.Threading;
    using System.Threading.Tasks;

    using Microsoft.Azure;
    using Microsoft.Azure.Management.DataFactories;
    using Microsoft.Azure.Management.DataFactories.Models;
    using Microsoft.Azure.Management.DataFactories.Common.Models;

    using Microsoft.IdentityModel.Clients.ActiveDirectory;

    ```
6. <span data-ttu-id="70179-159">Adja hozzá az alábbi kódot, amellyel létrehoz egy példányt a **DataPipelineManagementClient** osztályból a **Main** metódusban.</span><span class="sxs-lookup"><span data-stu-id="70179-159">Add the following code that creates an instance of **DataPipelineManagementClient** class to the **Main** method.</span></span> <span data-ttu-id="70179-160">Ezzel az objektummal adat-előállítót, társított szolgáltatást, bemeneti és kimeneti adatkészleteket és egy adatcsatornát is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="70179-160">You use this object to create a data factory, a linked service, input and output datasets, and a pipeline.</span></span> <span data-ttu-id="70179-161">Ezenfelül ez az objektum használható az adatkészlet szeleteinek figyelése a futtatókörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="70179-161">You also use this object to monitor slices of a dataset at runtime.</span></span>

    ```csharp
    // create data factory management client

    //IMPORTANT: specify the name of Azure resource group here
    string resourceGroupName = "ADFTutorialResourceGroup";

    //IMPORTANT: the name of the data factory must be globally unique.
    // Therefore, update this value. For example:APITutorialFactory05122017
    string dataFactoryName = "APITutorialFactory";

    TokenCloudCredentials aadTokenCredentials = new TokenCloudCredentials(
            ConfigurationManager.AppSettings["SubscriptionId"],
            GetAuthorizationHeader().Result);

    Uri resourceManagerUri = new Uri(ConfigurationManager.AppSettings["ResourceManagerEndpoint"]);

    DataFactoryManagementClient client = new DataFactoryManagementClient(aadTokenCredentials, resourceManagerUri);
    ```

   > [!IMPORTANT]
   > <span data-ttu-id="70179-162">A **resourceGroupName** érték helyére írja be saját Azure-erőforráscsoportja nevét.</span><span class="sxs-lookup"><span data-stu-id="70179-162">Replace the value of **resourceGroupName** with the name of your Azure resource group.</span></span> <span data-ttu-id="70179-163">Egy erőforrás csoport használatával hozhat létre a [New-használják](/powershell/module/azurerm.resources/new-azurermresourcegroup) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="70179-163">You can create a resource group using the [New-AzureResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) cmdlet.</span></span>
   >
   > <span data-ttu-id="70179-164">Módosítsa úgy az adat-előállító nevét (dataFactoryName), hogy az egyedi legyen.</span><span class="sxs-lookup"><span data-stu-id="70179-164">Update name of the data factory (dataFactoryName) to be unique.</span></span> <span data-ttu-id="70179-165">Az adat-előállító nevének globálisan egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="70179-165">Name of the data factory must be globally unique.</span></span> <span data-ttu-id="70179-166">A Data Factory-összetevők elnevezési szabályait a [Data Factory - Naming Rules](data-factory-naming-rules.md) (Data Factory – Elnevezési szabályok) című témakörben találhatja.</span><span class="sxs-lookup"><span data-stu-id="70179-166">See [Data Factory - Naming Rules](data-factory-naming-rules.md) topic for naming rules for Data Factory artifacts.</span></span>
7. <span data-ttu-id="70179-167">Adja hozzá az alábbi kódot, amely létrehozza az **adat-előállítót** a **Main** metódusban.</span><span class="sxs-lookup"><span data-stu-id="70179-167">Add the following code that creates a **data factory** to the **Main** method.</span></span>

    ```csharp
    // create a data factory
    Console.WriteLine("Creating a data factory");
    client.DataFactories.CreateOrUpdate(resourceGroupName,
        new DataFactoryCreateOrUpdateParameters()
        {
            DataFactory = new DataFactory()
            {
                Name = dataFactoryName,
                Location = "westus",
                Properties = new DataFactoryProperties()
            }
        }
    );
    ```
8. <span data-ttu-id="70179-168">Adja hozzá az alábbi kódot, amely létrehozza az **Azure Storage-társított szolgáltatást** a **Main** metódusban.</span><span class="sxs-lookup"><span data-stu-id="70179-168">Add the following code that creates an **Azure Storage linked service** to the **Main** method.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="70179-169">A **storageaccountname** és az **accountkey** értékek helyére írja be Azure Storage-tárfiókja nevét, illetve kulcsát.</span><span class="sxs-lookup"><span data-stu-id="70179-169">Replace **storageaccountname** and **accountkey** with name and key of your Azure Storage account.</span></span>

    ```csharp
    // create a linked service for input data store: Azure Storage
    Console.WriteLine("Creating Azure Storage linked service");
    client.LinkedServices.CreateOrUpdate(resourceGroupName, dataFactoryName,
        new LinkedServiceCreateOrUpdateParameters()
        {
            LinkedService = new LinkedService()
            {
                Name = "AzureStorageLinkedService",
                Properties = new LinkedServiceProperties
                (
                    new AzureStorageLinkedService("DefaultEndpointsProtocol=https;AccountName=<storageaccountname>;AccountKey=<accountkey>")
                )
            }
        }
    );
    ```
9. <span data-ttu-id="70179-170">Adja hozzá az alábbi kódot, amely létrehozza a **bemeneti és kimeneti adatkészleteket** a **Main** metódusban.</span><span class="sxs-lookup"><span data-stu-id="70179-170">Add the following code that creates **input and output datasets** to the **Main** method.</span></span>

    <span data-ttu-id="70179-171">A **FolderPath** a bemeneti blob értékre van állítva a **adftutorial /** ahol **adftutorial** neve, a blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="70179-171">The **FolderPath** for the input blob is set to **adftutorial/** where **adftutorial** is the name of the container in your blob storage.</span></span> <span data-ttu-id="70179-172">Ha ez a tároló nem létezik az Azure blob Storage tárolóban, a tároló létrehozása ezen a néven: **adftutorial** és a szöveges fájl feltöltése a tárolóba.</span><span class="sxs-lookup"><span data-stu-id="70179-172">If this container does not exist in your Azure blob storage, create a container with this name: **adftutorial** and upload a text file to the container.</span></span>

    <span data-ttu-id="70179-173">A kimeneti BLOB FolderPath értékre van állítva: **adftutorial/apifactoryoutput / {szelet}** ahol **szelet** értékének dinamikusan kiszámítását **SliceStart** () minden szelet dátum idejű Start.)</span><span class="sxs-lookup"><span data-stu-id="70179-173">The FolderPath for the output blob is set to: **adftutorial/apifactoryoutput/{Slice}** where **Slice** is dynamically calculated based on the value of **SliceStart** (start date-time of each slice.)</span></span>

    ```csharp
    // create input and output datasets
    Console.WriteLine("Creating input and output datasets");
    string Dataset_Source = "DatasetBlobSource";
    string Dataset_Destination = "DatasetBlobDestination";
    
    client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,
    new DatasetCreateOrUpdateParameters()
    {
        Dataset = new Dataset()
        {
            Name = Dataset_Source,
            Properties = new DatasetProperties()
            {
                LinkedServiceName = "AzureStorageLinkedService",
                TypeProperties = new AzureBlobDataset()
                {
                    FolderPath = "adftutorial/",
                    FileName = "emp.txt"
                },
                External = true,
                Availability = new Availability()
                {
                    Frequency = SchedulePeriod.Hour,
                    Interval = 1,
                },
    
                Policy = new Policy()
                {
                    Validation = new ValidationPolicy()
                    {
                        MinimumRows = 1
                    }
                }
            }
        }
    });
    
    client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,
    new DatasetCreateOrUpdateParameters()
    {
        Dataset = new Dataset()
        {
            Name = Dataset_Destination,
            Properties = new DatasetProperties()
            {
    
                LinkedServiceName = "AzureStorageLinkedService",
                TypeProperties = new AzureBlobDataset()
                {
                    FolderPath = "adftutorial/apifactoryoutput/{Slice}",
                    PartitionedBy = new Collection<Partition>()
                    {
                        new Partition()
                        {
                            Name = "Slice",
                            Value = new DateTimePartitionValue()
                            {
                                Date = "SliceStart",
                                Format = "yyyyMMdd-HH"
                            }
                        }
                    }
                },
    
                Availability = new Availability()
                {
                    Frequency = SchedulePeriod.Hour,
                    Interval = 1,
                },
            }
        }
    });
    ```
10. <span data-ttu-id="70179-174">Adja hozzá az alábbi kódot, amely **létrehozza és aktiválja az adatcsatornát** a **Main** metódusban.</span><span class="sxs-lookup"><span data-stu-id="70179-174">Add the following code that **creates and activates a pipeline** to the **Main** method.</span></span> <span data-ttu-id="70179-175">Az adatcsatorna **CopyActivity** paraméterrel meghatározott másolási tevékenysége a **BlobSource** értékét használja forrásként, és a **BlobSink** értékét fogadóként.</span><span class="sxs-lookup"><span data-stu-id="70179-175">This pipeline has a **CopyActivity** that takes **BlobSource** as a source and **BlobSink** as a sink.</span></span>

    <span data-ttu-id="70179-176">A másolási tevékenység végzi az adatok továbbítását az Azure Data Factoryban.</span><span class="sxs-lookup"><span data-stu-id="70179-176">The Copy Activity performs the data movement in Azure Data Factory.</span></span> <span data-ttu-id="70179-177">A tevékenységet egy globálisan elérhető szolgáltatás működteti, amely biztonságos, megbízható és skálázható módon másolja az adatokat a különböző adattárak között.</span><span class="sxs-lookup"><span data-stu-id="70179-177">The activity is powered by a globally available service that can copy data between various data stores in a secure, reliable, and scalable way.</span></span> <span data-ttu-id="70179-178">A Másolás tevékenységgel kapcsolatos részletekért tekintse meg a [Data Movement Activities](data-factory-data-movement-activities.md) (Adattovábbítási tevékenységek) című cikket.</span><span class="sxs-lookup"><span data-stu-id="70179-178">See [Data Movement Activities](data-factory-data-movement-activities.md) article for details about the Copy Activity.</span></span>

    ```csharp
    // create a pipeline
    Console.WriteLine("Creating a pipeline");
    DateTime PipelineActivePeriodStartTime = new DateTime(2014, 8, 9, 0, 0, 0, 0, DateTimeKind.Utc);
    DateTime PipelineActivePeriodEndTime = PipelineActivePeriodStartTime.AddMinutes(60);
    string PipelineName = "PipelineBlobSample";
    
    client.Pipelines.CreateOrUpdate(resourceGroupName, dataFactoryName,
    new PipelineCreateOrUpdateParameters()
    {
        Pipeline = new Pipeline()
        {
            Name = PipelineName,
            Properties = new PipelineProperties()
            {
                Description = "Demo Pipeline for data transfer between blobs",
    
                // Initial value for pipeline's active period. With this, you won't need to set slice status
                Start = PipelineActivePeriodStartTime,
                End = PipelineActivePeriodEndTime,
    
                Activities = new List<Activity>()
                {
                    new Activity()
                    {
                        Name = "BlobToBlob",
                        Inputs = new List<ActivityInput>()
                        {
                            new ActivityInput()
                {
                                Name = Dataset_Source
                            }
                        },
                        Outputs = new List<ActivityOutput>()
                        {
                            new ActivityOutput()
                            {
                                Name = Dataset_Destination
                            }
                        },
                        TypeProperties = new CopyActivity()
                        {
                            Source = new BlobSource(),
                            Sink = new BlobSink()
                            {
                                WriteBatchSize = 10000,
                                WriteBatchTimeout = TimeSpan.FromMinutes(10)
                            }
                        }
                    }
    
                },
            }
        }
    });
    ```
12. <span data-ttu-id="70179-179">A kimeneti adatkészlet adatszeletének állapotának lekéréséhez adja hozzá az alábbi kódot a **Main** metódushoz.</span><span class="sxs-lookup"><span data-stu-id="70179-179">Add the following code to the **Main** method to get the status of a data slice of the output dataset.</span></span> <span data-ttu-id="70179-180">Ez a minta a várt egyetlen szelet van.</span><span class="sxs-lookup"><span data-stu-id="70179-180">There is only one slice expected in this sample.</span></span>

    ```csharp
    // Pulling status within a timeout threshold
    DateTime start = DateTime.Now;
    bool done = false;
    
    while (DateTime.Now - start < TimeSpan.FromMinutes(5) && !done)
    {
        Console.WriteLine("Pulling the slice status");
        // wait before the next status check
        Thread.Sleep(1000 * 12);
    
        var datalistResponse = client.DataSlices.List(resourceGroupName, dataFactoryName, Dataset_Destination,
            new DataSliceListParameters()
            {
                DataSliceRangeStartTime = PipelineActivePeriodStartTime.ConvertToISO8601DateTimeString(),
                DataSliceRangeEndTime = PipelineActivePeriodEndTime.ConvertToISO8601DateTimeString()
            });
    
        foreach (DataSlice slice in datalistResponse.DataSlices)
        {
            if (slice.State == DataSliceState.Failed || slice.State == DataSliceState.Ready)
            {
                Console.WriteLine("Slice execution is done with status: {0}", slice.State);
                done = true;
                break;
            }
            else
            {
                Console.WriteLine("Slice status is: {0}", slice.State);
            }
        }
    }
    ```
13. <span data-ttu-id="70179-181">**(választható)**  Futtatásához lekérni egy adatszelet részleteit az alábbi kódot a **fő** metódust.</span><span class="sxs-lookup"><span data-stu-id="70179-181">**(optional)** Add the following code to get run details for a data slice to the **Main** method.</span></span>

    ```csharp
    Console.WriteLine("Getting run details of a data slice");
    
    // give it a few minutes for the output slice to be ready
    Console.WriteLine("\nGive it a few minutes for the output slice to be ready and press any key.");
    Console.ReadKey();
    
    var datasliceRunListResponse = client.DataSliceRuns.List(
        resourceGroupName,
        dataFactoryName,
        Dataset_Destination,
        new DataSliceRunListParameters()
        {
            DataSliceStartTime = PipelineActivePeriodStartTime.ConvertToISO8601DateTimeString()
        });
    
    foreach (DataSliceRun run in datasliceRunListResponse.DataSliceRuns)
    {
        Console.WriteLine("Status: \t\t{0}", run.Status);
        Console.WriteLine("DataSliceStart: \t{0}", run.DataSliceStart);
        Console.WriteLine("DataSliceEnd: \t\t{0}", run.DataSliceEnd);
        Console.WriteLine("ActivityId: \t\t{0}", run.ActivityName);
        Console.WriteLine("ProcessingStartTime: \t{0}", run.ProcessingStartTime);
        Console.WriteLine("ProcessingEndTime: \t{0}", run.ProcessingEndTime);
        Console.WriteLine("ErrorMessage: \t{0}", run.ErrorMessage);
    }
    
    Console.WriteLine("\nPress any key to exit.");
    Console.ReadKey();
    ```
14. <span data-ttu-id="70179-182">Adja hozzá a **Main** metódushoz szükséges alábbi segédmetódust a **Program** osztályhoz.</span><span class="sxs-lookup"><span data-stu-id="70179-182">Add the following helper method used by the **Main** method to the **Program** class.</span></span> <span data-ttu-id="70179-183">Ez a módszer egy párbeszédpanelt, amely, amely lehetővé teszi, hogy biztosítható a POP **felhasználónév** és **jelszó** , amelyekkel jelentkezzen be Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="70179-183">This method pops a dialog box that that lets you provide **user name** and **password** that you use to log in to Azure portal.</span></span>

    ```csharp
    public static async Task<string> GetAuthorizationHeader()
    {
        AuthenticationContext context = new AuthenticationContext(ConfigurationManager.AppSettings["ActiveDirectoryEndpoint"] + ConfigurationManager.AppSettings["ActiveDirectoryTenantId"]);
        ClientCredential credential = new ClientCredential(
            ConfigurationManager.AppSettings["ApplicationId"],
            ConfigurationManager.AppSettings["Password"]);
        AuthenticationResult result = await context.AcquireTokenAsync(
            resource: ConfigurationManager.AppSettings["WindowsManagementUri"],
            clientCredential: credential);

        if (result != null)
            return result.AccessToken;

        throw new InvalidOperationException("Failed to acquire token");
    }
    ```

15. <span data-ttu-id="70179-184">A Megoldáskezelőben bontsa ki a projekt: **DataFactoryAPITestApp**, kattintson a jobb gombbal **hivatkozások**, és kattintson a **hivatkozás hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="70179-184">In the Solution Explorer, expand the project: **DataFactoryAPITestApp**, right-click **References**, and click **Add Reference**.</span></span> <span data-ttu-id="70179-185">Jelölje be a jelölőnégyzetet `System.Configuration` szerelvény, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="70179-185">Select check box for `System.Configuration` assembly and click **OK**.</span></span>
15. <span data-ttu-id="70179-186">Hozza létre a konzolalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="70179-186">Build the console application.</span></span> <span data-ttu-id="70179-187">Kattintson a menü **Fordítás** elemére, majd a **Megoldás fordítása** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="70179-187">Click **Build** on the menu and click **Build Solution**.</span></span>
16. <span data-ttu-id="70179-188">Győződjön meg arról, hogy van legalább egy fájlt a adftutorial tárolóban az Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="70179-188">Confirm that there is at least one file in the adftutorial container in your Azure blob storage.</span></span> <span data-ttu-id="70179-189">Ha nem, hozzon létre Emp.txt fájlt a Jegyzettömbben az alábbi tartalommal, és töltse fel a adftutorial tároló.</span><span class="sxs-lookup"><span data-stu-id="70179-189">If not, create Emp.txt file in Notepad with the following content and upload it to the adftutorial container.</span></span>

    ```
    John, Doe
    Jane, Doe
    ```
17. <span data-ttu-id="70179-190">Futtassa le a mintát, ehhez kattintson a menü **Hibakeresés** -> **Hibakeresés indítása** elemére.</span><span class="sxs-lookup"><span data-stu-id="70179-190">Run the sample by clicking **Debug** -> **Start Debugging** on the menu.</span></span> <span data-ttu-id="70179-191">Ha megjelenik a **Getting run details of a data slice** (Adatszelet futtatási adatainak lekérése) felirat, várjon néhány percet, majd nyomja le az **ENTER** billentyűt.</span><span class="sxs-lookup"><span data-stu-id="70179-191">When you see the **Getting run details of a data slice**, wait for a few minutes, and press **ENTER**.</span></span>
18. <span data-ttu-id="70179-192">Az Azure Portalon ellenőrizze, hogy az **APITutorialFactory** nevű adat-előállító létrejött-e az alábbi összetevőkkel:</span><span class="sxs-lookup"><span data-stu-id="70179-192">Use the Azure portal to verify that the data factory **APITutorialFactory** is created with the following artifacts:</span></span>
    * <span data-ttu-id="70179-193">Társított szolgáltatás: **AzureStorageLinkedService**</span><span class="sxs-lookup"><span data-stu-id="70179-193">Linked service: **AzureStorageLinkedService**</span></span>
    * <span data-ttu-id="70179-194">Adatkészlet: **DatasetBlobSource** és **DatasetBlobDestination**.</span><span class="sxs-lookup"><span data-stu-id="70179-194">Dataset: **DatasetBlobSource** and **DatasetBlobDestination**.</span></span>
    * <span data-ttu-id="70179-195">Adatcsatorna: **PipelineBlobSample**</span><span class="sxs-lookup"><span data-stu-id="70179-195">Pipeline: **PipelineBlobSample**</span></span>
19. <span data-ttu-id="70179-196">Győződjön meg arról, hogy a kimeneti fájl jön létre a **apifactoryoutput** mappájában a **adftutorial** tároló.</span><span class="sxs-lookup"><span data-stu-id="70179-196">Verify that an output file is created in the **apifactoryoutput** folder in the **adftutorial** container.</span></span>

## <a name="get-a-list-of-failed-data-slices"></a><span data-ttu-id="70179-197">Hibás adatszeletek listájának lekérdezése</span><span class="sxs-lookup"><span data-stu-id="70179-197">Get a list of failed data slices</span></span> 

```csharp
// Parse the resource path
var ResourceGroupName = "ADFTutorialResourceGroup";
var DataFactoryName = "DataFactoryAPITestApp";

var parameters = new ActivityWindowsByDataFactoryListParameters(ResourceGroupName, DataFactoryName);
parameters.WindowState = "Failed";
var response = dataFactoryManagementClient.ActivityWindows.List(parameters);
do
{
    foreach (var activityWindow in response.ActivityWindowListResponseValue.ActivityWindows)
    {
        var row = string.Join(
            "\t",
            activityWindow.WindowStart.ToString(),
            activityWindow.WindowEnd.ToString(),
            activityWindow.RunStart.ToString(),
            activityWindow.RunEnd.ToString(),
            activityWindow.DataFactoryName,
            activityWindow.PipelineName,
            activityWindow.ActivityName,
            string.Join(",", activityWindow.OutputDatasets));
        Console.WriteLine(row);
    }

    if (response.NextLink != null)
    {
        response = dataFactoryManagementClient.ActivityWindows.ListNext(response.NextLink, parameters);
    }
    else
    {
        response = null;
    }
}
while (response != null);
```

## <a name="next-steps"></a><span data-ttu-id="70179-198">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="70179-198">Next steps</span></span>
<span data-ttu-id="70179-199">A következő példa egy folyamatot, amely másol adatokat az Azure blob storage egy Azure SQL Database .NET SDK használatával létrehozásához lásd:</span><span class="sxs-lookup"><span data-stu-id="70179-199">See the following example for creating a pipeline using .NET SDK that copies data from an Azure blob storage to an Azure SQL database:</span></span> 

- [<span data-ttu-id="70179-200">Hozzon létre egy folyamatot, a Blob Storage az SQL Database-adatok másolása</span><span class="sxs-lookup"><span data-stu-id="70179-200">Create a pipeline to copy data from Blob Storage to SQL Database</span></span>](data-factory-copy-activity-tutorial-using-dotnet-api.md)
