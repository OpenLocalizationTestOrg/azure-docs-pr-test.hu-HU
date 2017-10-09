---
title: "aaaCreate adatok folyamatok Azure .NET SDK használatával |} Microsoft Docs"
description: "Ismerje meg, hogyan tooprogrammatically létrehozásához, figyeléséhez és felügyeletéhez Azure adat-előállítók Data Factory SDK használatával."
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
ms.openlocfilehash: 190b5f99edbb3c27e1e8efb8990b9e601b22458f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-monitor-and-manage-azure-data-factories-using-azure-data-factory-net-sdk"></a><span data-ttu-id="19048-103">Hozzon létre, felügyelete és kezelése az Azure Data Factory .NET SDK használatával Azure adat-előállítók</span><span class="sxs-lookup"><span data-stu-id="19048-103">Create, monitor, and manage Azure data factories using Azure Data Factory .NET SDK</span></span>
## <a name="overview"></a><span data-ttu-id="19048-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="19048-104">Overview</span></span>
<span data-ttu-id="19048-105">Hozzon létre, figyelése és kezelése az Azure adat-előállítók programozott módon a Data Factory .NET SDK használatával.</span><span class="sxs-lookup"><span data-stu-id="19048-105">You can create, monitor, and manage Azure data factories programmatically using Data Factory .NET SDK.</span></span> <span data-ttu-id="19048-106">A cikkben egy forgatókönyv, amelyeket követve toocreate minta .NET-Konzolalkalmazás, amely létrehoz és egy adat-előállító figyeli.</span><span class="sxs-lookup"><span data-stu-id="19048-106">This article contains a walkthrough that you can follow toocreate a sample .NET console application that creates and monitors a data factory.</span></span> 

> [!NOTE]
> <span data-ttu-id="19048-107">Ez a cikk nem foglalkozik az összes hello Data Factory .NET API.</span><span class="sxs-lookup"><span data-stu-id="19048-107">This article does not cover all hello Data Factory .NET API.</span></span> <span data-ttu-id="19048-108">Lásd: [Data Factory .NET API-referencia](/dotnet/api/index?view=azuremgmtdatafactories-4.12.1) .NET API-t a Data Factory átfogó dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="19048-108">See [Data Factory .NET API Reference](/dotnet/api/index?view=azuremgmtdatafactories-4.12.1) for comprehensive documentation on .NET API for Data Factory.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="19048-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="19048-109">Prerequisites</span></span>
* <span data-ttu-id="19048-110">Visual Studio 2012, 2013 vagy 2015</span><span class="sxs-lookup"><span data-stu-id="19048-110">Visual Studio 2012 or 2013 or 2015</span></span>
* <span data-ttu-id="19048-111">Töltse le és telepítse [Azure .NET SDK](http://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="19048-111">Download and install [Azure .NET SDK](http://azure.microsoft.com/downloads/).</span></span>
* <span data-ttu-id="19048-112">Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="19048-112">Azure PowerShell.</span></span> <span data-ttu-id="19048-113">Kövesse az utasításokat [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview) tooinstall Azure PowerShell, a számítógépen a következő cikket.</span><span class="sxs-lookup"><span data-stu-id="19048-113">Follow instructions in [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) article tooinstall Azure PowerShell on your computer.</span></span> <span data-ttu-id="19048-114">Használhatja az Azure PowerShell toocreate egy Azure Active Directory-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="19048-114">You use Azure PowerShell toocreate an Azure Active Directory application.</span></span>

### <a name="create-an-application-in-azure-active-directory"></a><span data-ttu-id="19048-115">Alkalmazás létrehozása az Azure Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="19048-115">Create an application in Azure Active Directory</span></span>
<span data-ttu-id="19048-116">Hozzon létre egy Azure Active Directory-alkalmazás, hello alkalmazás szolgáltatásnevet létrehozni, és rendelje hozzá toohello **Data Factory közreműködői** szerepkör.</span><span class="sxs-lookup"><span data-stu-id="19048-116">Create an Azure Active Directory application, create a service principal for hello application, and assign it toohello **Data Factory Contributor** role.</span></span>

1. <span data-ttu-id="19048-117">Indítsa el a **PowerShellt**.</span><span class="sxs-lookup"><span data-stu-id="19048-117">Launch **PowerShell**.</span></span>
2. <span data-ttu-id="19048-118">Futtassa a következő parancs hello, és írja be a hello felhasználónevet és jelszót toosign használhatja a toohello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="19048-118">Run hello following command and enter hello user name and password that you use toosign in toohello Azure portal.</span></span>

    ```PowerShell
    Login-AzureRmAccount
    ```
3. <span data-ttu-id="19048-119">Futtassa a következő parancs tooview hello minden hello előfizetés ehhez a fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="19048-119">Run hello following command tooview all hello subscriptions for this account.</span></span>

    ```PowerShell
    Get-AzureRmSubscription
    ```
4. <span data-ttu-id="19048-120">Futtassa a következő parancs tooselect hello előfizetést, amelyet a toowork hello.</span><span class="sxs-lookup"><span data-stu-id="19048-120">Run hello following command tooselect hello subscription that you want toowork with.</span></span> <span data-ttu-id="19048-121">Cserélje le  **&lt;NameOfAzureSubscription** &gt; hello nevet, az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="19048-121">Replace **&lt;NameOfAzureSubscription**&gt; with hello name of your Azure subscription.</span></span>

    ```PowerShell
    Get-AzureRmSubscription -SubscriptionName <NameOfAzureSubscription> | Set-AzureRmContext
    ```

   > [!IMPORTANT]
   > <span data-ttu-id="19048-122">Jegyezze fel **SubscriptionId** és **TenantId** a parancs hello kimenetből.</span><span class="sxs-lookup"><span data-stu-id="19048-122">Note down **SubscriptionId** and **TenantId** from hello output of this command.</span></span>

5. <span data-ttu-id="19048-123">Hozzon létre egy Azure erőforráscsoport nevű **ADFTutorialResourceGroup** a következő parancsot a hello PowerShell hello futtatásával.</span><span class="sxs-lookup"><span data-stu-id="19048-123">Create an Azure resource group named **ADFTutorialResourceGroup** by running hello following command in hello PowerShell.</span></span>

    ```PowerShell
    New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"
    ```

    <span data-ttu-id="19048-124">Ha hello erőforráscsoportban már létezik, akkor adja meg, hogy tooupdate (Y), vagy legyen (n).</span><span class="sxs-lookup"><span data-stu-id="19048-124">If hello resource group already exists, you specify whether tooupdate it (Y) or keep it as (N).</span></span>

    <span data-ttu-id="19048-125">Ha egy másik erőforráscsoportban található használja, ebben az oktatóanyagban kell toouse hello ADFTutorialResourceGroup helyett az erőforráscsoport nevét.</span><span class="sxs-lookup"><span data-stu-id="19048-125">If you use a different resource group, you need toouse hello name of your resource group in place of ADFTutorialResourceGroup in this tutorial.</span></span>
6. <span data-ttu-id="19048-126">Hozzon létre egy Azure Active Directory-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="19048-126">Create an Azure Active Directory application.</span></span>

    ```PowerShell
    $azureAdApplication = New-AzureRmADApplication -DisplayName "ADFDotNetWalkthroughApp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.adfdotnetwalkthroughapp.org/example" -Password "Pass@word1"
    ```

    <span data-ttu-id="19048-127">Ha hiba történt a következő hello, adjon meg egy másik URL-címet, majd futtassa újra hello parancsot.</span><span class="sxs-lookup"><span data-stu-id="19048-127">If you get hello following error, specify a different URL and run hello command again.</span></span>
    
    ```PowerShell
    Another object with hello same value for property identifierUris already exists.
    ```
7. <span data-ttu-id="19048-128">Hello AD szolgáltatásnevet létrehozni.</span><span class="sxs-lookup"><span data-stu-id="19048-128">Create hello AD service principal.</span></span>

    ```PowerShell
    New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId
    ```
8. <span data-ttu-id="19048-129">Adja hozzá a szolgáltatás egyszerű toohello **Data Factory közreműködői** szerepkör.</span><span class="sxs-lookup"><span data-stu-id="19048-129">Add service principal toohello **Data Factory Contributor** role.</span></span>

    ```PowerShell
    New-AzureRmRoleAssignment -RoleDefinitionName "Data Factory Contributor" -ServicePrincipalName $azureAdApplication.ApplicationId.Guid
    ```
9. <span data-ttu-id="19048-130">Első hello azonosítót.</span><span class="sxs-lookup"><span data-stu-id="19048-130">Get hello application ID.</span></span>

    ```PowerShell
    $azureAdApplication 
    ```
    <span data-ttu-id="19048-131">Jegyezze fel hello Alkalmazásazonosítót (applicationID) hello kimenetből.</span><span class="sxs-lookup"><span data-stu-id="19048-131">Note down hello application ID (applicationID) from hello output.</span></span>

<span data-ttu-id="19048-132">A fenti lépések elvégzésével beszereztük az alábbi négy értéket:</span><span class="sxs-lookup"><span data-stu-id="19048-132">You should have following four values from these steps:</span></span>

* <span data-ttu-id="19048-133">Bérlőazonosító</span><span class="sxs-lookup"><span data-stu-id="19048-133">Tenant ID</span></span>
* <span data-ttu-id="19048-134">Előfizetés azonosítója</span><span class="sxs-lookup"><span data-stu-id="19048-134">Subscription ID</span></span>
* <span data-ttu-id="19048-135">Alkalmazásazonosító</span><span class="sxs-lookup"><span data-stu-id="19048-135">Application ID</span></span>
* <span data-ttu-id="19048-136">Jelszó (hello első parancsban megadott)</span><span class="sxs-lookup"><span data-stu-id="19048-136">Password (specified in hello first command)</span></span>

## <a name="walkthrough"></a><span data-ttu-id="19048-137">Útmutatás</span><span class="sxs-lookup"><span data-stu-id="19048-137">Walkthrough</span></span>
<span data-ttu-id="19048-138">Hello forgatókönyv akkor hozzon létre egy adat-előállító egy folyamatot, amely tartalmazza a másolási tevékenység.</span><span class="sxs-lookup"><span data-stu-id="19048-138">In hello walkthrough, you create a data factory with a pipeline that contains a copy activity.</span></span> <span data-ttu-id="19048-139">hello másolási tevékenység adatainak másolása az Azure blob storage tooanother mappában a hello mappából azonos blob-tároló.</span><span class="sxs-lookup"><span data-stu-id="19048-139">hello copy activity copies data from a folder in your Azure blob storage tooanother folder in hello same blob storage.</span></span> 

<span data-ttu-id="19048-140">hello másolási tevékenység hello adatmozgás Azure Data Factory hajt végre.</span><span class="sxs-lookup"><span data-stu-id="19048-140">hello Copy Activity performs hello data movement in Azure Data Factory.</span></span> <span data-ttu-id="19048-141">hello tevékenység egy globálisan elérhető szolgáltatás közötti biztonságos, megbízható és skálázható módon különböző adattárolókhoz másolhatja van-e kapcsolva.</span><span class="sxs-lookup"><span data-stu-id="19048-141">hello activity is powered by a globally available service that can copy data between various data stores in a secure, reliable, and scalable way.</span></span> <span data-ttu-id="19048-142">Lásd: [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) szóló cikkben olvashat hello másolási tevékenység.</span><span class="sxs-lookup"><span data-stu-id="19048-142">See [Data Movement Activities](data-factory-data-movement-activities.md) article for details about hello Copy Activity.</span></span>

1. <span data-ttu-id="19048-143">Hozzon létre egy a C# nyelvet használó .NET-konzolalkalmazást a Visual Studio 2012/2013/2015 segítségével.</span><span class="sxs-lookup"><span data-stu-id="19048-143">Using Visual Studio 2012/2013/2015, create a C# .NET console application.</span></span>
   1. <span data-ttu-id="19048-144">Indítsa el a **Visual Studio** 2012/2013/2015-at.</span><span class="sxs-lookup"><span data-stu-id="19048-144">Launch **Visual Studio** 2012/2013/2015.</span></span>
   2. <span data-ttu-id="19048-145">Kattintson a **fájl**, pont túl**új**, és kattintson a **projekt**.</span><span class="sxs-lookup"><span data-stu-id="19048-145">Click **File**, point too**New**, and click **Project**.</span></span>
   3. <span data-ttu-id="19048-146">Bontsa ki a **Sablonok** lehetőséget, és válassza a **Visual C#** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="19048-146">Expand **Templates**, and select **Visual C#**.</span></span> <span data-ttu-id="19048-147">Ebben az útmutatóban a C#-t fogjuk használni, de bármelyik más .NET-nyelv is alkalmazható.</span><span class="sxs-lookup"><span data-stu-id="19048-147">In this walkthrough, you use C#, but you can use any .NET language.</span></span>
   4. <span data-ttu-id="19048-148">Válassza ki **Konzolalkalmazás** jobb hello projekttípusok hello listája.</span><span class="sxs-lookup"><span data-stu-id="19048-148">Select **Console Application** from hello list of project types on hello right.</span></span>
   5. <span data-ttu-id="19048-149">Adja meg **DataFactoryAPITestApp** a hello nevét.</span><span class="sxs-lookup"><span data-stu-id="19048-149">Enter **DataFactoryAPITestApp** for hello Name.</span></span>
   6. <span data-ttu-id="19048-150">Válassza ki **C:\ADFGetStarted** hello hely számára.</span><span class="sxs-lookup"><span data-stu-id="19048-150">Select **C:\ADFGetStarted** for hello Location.</span></span>
   7. <span data-ttu-id="19048-151">Kattintson a **OK** toocreate hello projekt.</span><span class="sxs-lookup"><span data-stu-id="19048-151">Click **OK** toocreate hello project.</span></span>
2. <span data-ttu-id="19048-152">Kattintson a **eszközök**, pont túl**NuGet-Csomagkezelő**, és kattintson a **Csomagkezelő konzol**.</span><span class="sxs-lookup"><span data-stu-id="19048-152">Click **Tools**, point too**NuGet Package Manager**, and click **Package Manager Console**.</span></span>
3. <span data-ttu-id="19048-153">A hello **Csomagkezelő konzol**, hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="19048-153">In hello **Package Manager Console**, do hello following steps:</span></span>
   1. <span data-ttu-id="19048-154">Futtassa a következő parancs tooinstall adat-előállító csomag hello:`Install-Package Microsoft.Azure.Management.DataFactories`</span><span class="sxs-lookup"><span data-stu-id="19048-154">Run hello following command tooinstall Data Factory package: `Install-Package Microsoft.Azure.Management.DataFactories`</span></span>
   2. <span data-ttu-id="19048-155">Futtassa a következő parancs tooinstall Azure Active Directory csomagot (Active Directory API-t használja a hello kódban) hello:`Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.19.208020213`</span><span class="sxs-lookup"><span data-stu-id="19048-155">Run hello following command tooinstall Azure Active Directory package (you use Active Directory API in hello code): `Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.19.208020213`</span></span>
4. <span data-ttu-id="19048-156">Cserélje le a hello tartalmát **App.config** hello a projekt hello tartalom a következő fájlt:</span><span class="sxs-lookup"><span data-stu-id="19048-156">Replace hello contents of **App.config** file in hello project with hello following content:</span></span> 
    
    ```xml
    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
        <appSettings>
            <add key="ActiveDirectoryEndpoint" value="https://login.microsoftonline.com/" />
            <add key="ResourceManagerEndpoint" value="https://management.azure.com/" />
            <add key="WindowsManagementUri" value="https://management.core.windows.net/" />

            <add key="ApplicationId" value="your application ID" />
            <add key="Password" value="Password you used while creating hello AAD application" />
            <add key="SubscriptionId" value= "Subscription ID" />
            <add key="ActiveDirectoryTenantId" value="Tenant ID" />
        </appSettings>
    </configuration>
    ```
5. <span data-ttu-id="19048-157">Hello App.Config fájlban frissítse az értékeket  **&lt;Alkalmazásazonosító&gt;**,  **&lt;jelszó&gt;**,  **&lt; Előfizetés-azonosító&gt;**, és  **&lt;bérlői azonosító&gt;**  saját értékekkel.</span><span class="sxs-lookup"><span data-stu-id="19048-157">In hello App.Config file, update values for **&lt;Application ID&gt;**, **&lt;Password&gt;**, **&lt;Subscription ID&gt;**, and **&lt;tenant ID&gt;** with your own values.</span></span>
6. <span data-ttu-id="19048-158">Adja hozzá a következő hello **használatával** utasítások toohello **Program.cs** fájl hello projektben.</span><span class="sxs-lookup"><span data-stu-id="19048-158">Add hello following **using** statements toohello **Program.cs** file in hello project.</span></span>

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
6. <span data-ttu-id="19048-159">Adja hozzá a következő kódot, amely létrehoz egy új hello **DataPipelineManagementClient** osztály toohello **fő** metódust.</span><span class="sxs-lookup"><span data-stu-id="19048-159">Add hello following code that creates an instance of **DataPipelineManagementClient** class toohello **Main** method.</span></span> <span data-ttu-id="19048-160">Egy adat-előállító, csatolt szolgáltatásként, bemeneti és kimeneti adatkészletek és egy folyamatot ezen objektum toocreate használhatja.</span><span class="sxs-lookup"><span data-stu-id="19048-160">You use this object toocreate a data factory, a linked service, input and output datasets, and a pipeline.</span></span> <span data-ttu-id="19048-161">Az objektum toomonitor szeletek a DataSet adatkészlet futtatás közben is használ.</span><span class="sxs-lookup"><span data-stu-id="19048-161">You also use this object toomonitor slices of a dataset at runtime.</span></span>

    ```csharp
    // create data factory management client

    //IMPORTANT: specify hello name of Azure resource group here
    string resourceGroupName = "ADFTutorialResourceGroup";

    //IMPORTANT: hello name of hello data factory must be globally unique.
    // Therefore, update this value. For example:APITutorialFactory05122017
    string dataFactoryName = "APITutorialFactory";

    TokenCloudCredentials aadTokenCredentials = new TokenCloudCredentials(
            ConfigurationManager.AppSettings["SubscriptionId"],
            GetAuthorizationHeader().Result);

    Uri resourceManagerUri = new Uri(ConfigurationManager.AppSettings["ResourceManagerEndpoint"]);

    DataFactoryManagementClient client = new DataFactoryManagementClient(aadTokenCredentials, resourceManagerUri);
    ```

   > [!IMPORTANT]
   > <span data-ttu-id="19048-162">Cserélje le a hello értékének **resourceGroupName** hello nevet, az Azure-erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="19048-162">Replace hello value of **resourceGroupName** with hello name of your Azure resource group.</span></span> <span data-ttu-id="19048-163">Létrehozhat egy olyan erőforráscsoport, hello segítségével [New-használják](/powershell/module/azurerm.resources/new-azurermresourcegroup) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="19048-163">You can create a resource group using hello [New-AzureResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) cmdlet.</span></span>
   >
   > <span data-ttu-id="19048-164">Frissítés hello data factory (dataFactoryName) toobe egyedi neve.</span><span class="sxs-lookup"><span data-stu-id="19048-164">Update name of hello data factory (dataFactoryName) toobe unique.</span></span> <span data-ttu-id="19048-165">Hello adat-előállító nevét globálisan egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="19048-165">Name of hello data factory must be globally unique.</span></span> <span data-ttu-id="19048-166">A Data Factory-összetevők elnevezési szabályait a [Data Factory - Naming Rules](data-factory-naming-rules.md) (Data Factory – Elnevezési szabályok) című témakörben találhatja.</span><span class="sxs-lookup"><span data-stu-id="19048-166">See [Data Factory - Naming Rules](data-factory-naming-rules.md) topic for naming rules for Data Factory artifacts.</span></span>
7. <span data-ttu-id="19048-167">Adja hozzá a kódot, amely létrehozza a következő hello egy **adat-előállító** toohello **fő** metódust.</span><span class="sxs-lookup"><span data-stu-id="19048-167">Add hello following code that creates a **data factory** toohello **Main** method.</span></span>

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
8. <span data-ttu-id="19048-168">Adja hozzá a kódot, amely létrehozza a következő hello egy **Azure Storage társított szolgáltatásnak** toohello **fő** metódust.</span><span class="sxs-lookup"><span data-stu-id="19048-168">Add hello following code that creates an **Azure Storage linked service** toohello **Main** method.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="19048-169">A **storageaccountname** és az **accountkey** értékek helyére írja be Azure Storage-tárfiókja nevét, illetve kulcsát.</span><span class="sxs-lookup"><span data-stu-id="19048-169">Replace **storageaccountname** and **accountkey** with name and key of your Azure Storage account.</span></span>

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
9. <span data-ttu-id="19048-170">Adja hozzá a kódot, amely létrehozza a következő hello **bemeneti és kimeneti adatkészletek** toohello **fő** metódust.</span><span class="sxs-lookup"><span data-stu-id="19048-170">Add hello following code that creates **input and output datasets** toohello **Main** method.</span></span>

    <span data-ttu-id="19048-171">Hello **FolderPath** hello bemeneti blob túl van-e állítva a**adftutorial /** ahol **adftutorial** hello név hello tároló a blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="19048-171">hello **FolderPath** for hello input blob is set too**adftutorial/** where **adftutorial** is hello name of hello container in your blob storage.</span></span> <span data-ttu-id="19048-172">Ha ez a tároló nem létezik az Azure blob Storage tárolóban, a tároló létrehozása ezen a néven: **adftutorial** , és töltse fel a szöveges fájl toohello tárolót.</span><span class="sxs-lookup"><span data-stu-id="19048-172">If this container does not exist in your Azure blob storage, create a container with this name: **adftutorial** and upload a text file toohello container.</span></span>

    <span data-ttu-id="19048-173">hello FolderPath hello a kimeneti blob értékre van állítva: **adftutorial/apifactoryoutput / {szelet}** ahol **szelet** dinamikusan számított alapján hello érték **SliceStart**(Indítás ideje minden szelet.)</span><span class="sxs-lookup"><span data-stu-id="19048-173">hello FolderPath for hello output blob is set to: **adftutorial/apifactoryoutput/{Slice}** where **Slice** is dynamically calculated based on hello value of **SliceStart** (start date-time of each slice.)</span></span>

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
10. <span data-ttu-id="19048-174">Adja hozzá a következő hello-kód **hoz létre, és aktiválja az adatcsatorna** toohello **fő** metódust.</span><span class="sxs-lookup"><span data-stu-id="19048-174">Add hello following code that **creates and activates a pipeline** toohello **Main** method.</span></span> <span data-ttu-id="19048-175">Az adatcsatorna **CopyActivity** paraméterrel meghatározott másolási tevékenysége a **BlobSource** értékét használja forrásként, és a **BlobSink** értékét fogadóként.</span><span class="sxs-lookup"><span data-stu-id="19048-175">This pipeline has a **CopyActivity** that takes **BlobSource** as a source and **BlobSink** as a sink.</span></span>

    <span data-ttu-id="19048-176">hello másolási tevékenység hello adatmozgás Azure Data Factory hajt végre.</span><span class="sxs-lookup"><span data-stu-id="19048-176">hello Copy Activity performs hello data movement in Azure Data Factory.</span></span> <span data-ttu-id="19048-177">hello tevékenység egy globálisan elérhető szolgáltatás közötti biztonságos, megbízható és skálázható módon különböző adattárolókhoz másolhatja van-e kapcsolva.</span><span class="sxs-lookup"><span data-stu-id="19048-177">hello activity is powered by a globally available service that can copy data between various data stores in a secure, reliable, and scalable way.</span></span> <span data-ttu-id="19048-178">Lásd: [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) szóló cikkben olvashat hello másolási tevékenység.</span><span class="sxs-lookup"><span data-stu-id="19048-178">See [Data Movement Activities](data-factory-data-movement-activities.md) article for details about hello Copy Activity.</span></span>

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
    
                // Initial value for pipeline's active period. With this, you won't need tooset slice status
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
12. <span data-ttu-id="19048-179">Adja hozzá a következő kód toohello hello **fő** metódus tooget hello állapotát egy adatszelet hello a kimeneti adatkészlet.</span><span class="sxs-lookup"><span data-stu-id="19048-179">Add hello following code toohello **Main** method tooget hello status of a data slice of hello output dataset.</span></span> <span data-ttu-id="19048-180">Ez a minta a várt egyetlen szelet van.</span><span class="sxs-lookup"><span data-stu-id="19048-180">There is only one slice expected in this sample.</span></span>

    ```csharp
    // Pulling status within a timeout threshold
    DateTime start = DateTime.Now;
    bool done = false;
    
    while (DateTime.Now - start < TimeSpan.FromMinutes(5) && !done)
    {
        Console.WriteLine("Pulling hello slice status");
        // wait before hello next status check
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
13. <span data-ttu-id="19048-181">**(választható)**  Hozzáadás hello következő kódot a adatok szelet toohello futtatása tooget részleteit **fő** metódust.</span><span class="sxs-lookup"><span data-stu-id="19048-181">**(optional)** Add hello following code tooget run details for a data slice toohello **Main** method.</span></span>

    ```csharp
    Console.WriteLine("Getting run details of a data slice");
    
    // give it a few minutes for hello output slice toobe ready
    Console.WriteLine("\nGive it a few minutes for hello output slice toobe ready and press any key.");
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
    
    Console.WriteLine("\nPress any key tooexit.");
    Console.ReadKey();
    ```
14. <span data-ttu-id="19048-182">Adja hozzá a következő hello segítő módszerének hello **fő** metódus toohello **Program** osztály.</span><span class="sxs-lookup"><span data-stu-id="19048-182">Add hello following helper method used by hello **Main** method toohello **Program** class.</span></span> <span data-ttu-id="19048-183">Ez a módszer egy párbeszédpanelt, amely, amely lehetővé teszi, hogy biztosítható a POP **felhasználónév** és **jelszó** használt toolog tooAzure portálon.</span><span class="sxs-lookup"><span data-stu-id="19048-183">This method pops a dialog box that that lets you provide **user name** and **password** that you use toolog in tooAzure portal.</span></span>

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

        throw new InvalidOperationException("Failed tooacquire token");
    }
    ```

15. <span data-ttu-id="19048-184">A Solution Explorer hello, bontsa ki a hello projekt: **DataFactoryAPITestApp**, kattintson a jobb gombbal **hivatkozások**, és kattintson a **hivatkozás hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="19048-184">In hello Solution Explorer, expand hello project: **DataFactoryAPITestApp**, right-click **References**, and click **Add Reference**.</span></span> <span data-ttu-id="19048-185">Jelölje be a jelölőnégyzetet `System.Configuration` szerelvény, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="19048-185">Select check box for `System.Configuration` assembly and click **OK**.</span></span>
15. <span data-ttu-id="19048-186">Hello Konzolalkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="19048-186">Build hello console application.</span></span> <span data-ttu-id="19048-187">Kattintson a **Build** hello menüre, majd a **megoldás fordítása**.</span><span class="sxs-lookup"><span data-stu-id="19048-187">Click **Build** on hello menu and click **Build Solution**.</span></span>
16. <span data-ttu-id="19048-188">Győződjön meg arról, hogy van legalább egy fájlt hello adftutorial tárolóban az Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="19048-188">Confirm that there is at least one file in hello adftutorial container in your Azure blob storage.</span></span> <span data-ttu-id="19048-189">Ha nem, hozzon létre Emp.txt fájlt a Jegyzettömbben a következő hello tartalom, és töltse fel az toohello adftutorial tároló.</span><span class="sxs-lookup"><span data-stu-id="19048-189">If not, create Emp.txt file in Notepad with hello following content and upload it toohello adftutorial container.</span></span>

    ```
    John, Doe
    Jane, Doe
    ```
17. <span data-ttu-id="19048-190">Futtassa a hello minta kattintva **Debug** -> **Start Debugging** hello menüben.</span><span class="sxs-lookup"><span data-stu-id="19048-190">Run hello sample by clicking **Debug** -> **Start Debugging** on hello menu.</span></span> <span data-ttu-id="19048-191">Amikor megjelenik a hello **első futtatása egy adatszelet részleteit**, várjon néhány percet, és nyomja le az **ENTER**.</span><span class="sxs-lookup"><span data-stu-id="19048-191">When you see hello **Getting run details of a data slice**, wait for a few minutes, and press **ENTER**.</span></span>
18. <span data-ttu-id="19048-192">Használja az Azure portál tooverify hello adott hello adat-előállító **APITutorialFactory** jön létre a következő összetevők hello:</span><span class="sxs-lookup"><span data-stu-id="19048-192">Use hello Azure portal tooverify that hello data factory **APITutorialFactory** is created with hello following artifacts:</span></span>
    * <span data-ttu-id="19048-193">Társított szolgáltatás: **AzureStorageLinkedService**</span><span class="sxs-lookup"><span data-stu-id="19048-193">Linked service: **AzureStorageLinkedService**</span></span>
    * <span data-ttu-id="19048-194">Adatkészlet: **DatasetBlobSource** és **DatasetBlobDestination**.</span><span class="sxs-lookup"><span data-stu-id="19048-194">Dataset: **DatasetBlobSource** and **DatasetBlobDestination**.</span></span>
    * <span data-ttu-id="19048-195">Adatcsatorna: **PipelineBlobSample**</span><span class="sxs-lookup"><span data-stu-id="19048-195">Pipeline: **PipelineBlobSample**</span></span>
19. <span data-ttu-id="19048-196">Győződjön meg arról, hogy a kimeneti fájl létrejött-e hello **apifactoryoutput** hello mappájában **adftutorial** tároló.</span><span class="sxs-lookup"><span data-stu-id="19048-196">Verify that an output file is created in hello **apifactoryoutput** folder in hello **adftutorial** container.</span></span>

## <a name="get-a-list-of-failed-data-slices"></a><span data-ttu-id="19048-197">Hibás adatszeletek listájának lekérdezése</span><span class="sxs-lookup"><span data-stu-id="19048-197">Get a list of failed data slices</span></span> 

```csharp
// Parse hello resource path
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

## <a name="next-steps"></a><span data-ttu-id="19048-198">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="19048-198">Next steps</span></span>
<span data-ttu-id="19048-199">Tekintse meg a következő példa egy folyamatot, amely másol adatokat az Azure blob storage tooan Azure SQL-adatbázis .NET SDK használatával létrehozásához hello:</span><span class="sxs-lookup"><span data-stu-id="19048-199">See hello following example for creating a pipeline using .NET SDK that copies data from an Azure blob storage tooan Azure SQL database:</span></span> 

- [<span data-ttu-id="19048-200">A folyamat toocopy Blob Storage tooSQL adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="19048-200">Create a pipeline toocopy data from Blob Storage tooSQL Database</span></span>](data-factory-copy-activity-tutorial-using-dotnet-api.md)
