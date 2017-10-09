---
title: "Oktatóanyag: Másolási tevékenységgel ellátott adatcsatorna létrehozása a .NET API használatával | Microsoft Docs"
description: "Ez az oktatóanyag bemutatja, hogyan hozhat létre másolási tevékenységgel rendelkező Azure Data Factory-adatcsatornákat a .NET API használatával."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 58fc4007-b46d-4c8e-a279-cb9e479b3e2b
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: 52f72da54cdd80691e09d7453bf6730454c4089e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-create-a-pipeline-with-copy-activity-using-net-api"></a><span data-ttu-id="84d00-103">Oktatóanyag: Másolási tevékenységgel ellátott adatcsatorna létrehozása a .NET API használatával</span><span class="sxs-lookup"><span data-stu-id="84d00-103">Tutorial: Create a pipeline with Copy Activity using .NET API</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="84d00-104">Áttekintés és előfeltételek</span><span class="sxs-lookup"><span data-stu-id="84d00-104">Overview and prerequisites</span></span>](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [<span data-ttu-id="84d00-105">Másolás varázsló</span><span class="sxs-lookup"><span data-stu-id="84d00-105">Copy Wizard</span></span>](data-factory-copy-data-wizard-tutorial.md)
> * [<span data-ttu-id="84d00-106">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="84d00-106">Azure portal</span></span>](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [<span data-ttu-id="84d00-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="84d00-107">Visual Studio</span></span>](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [<span data-ttu-id="84d00-108">PowerShell</span><span class="sxs-lookup"><span data-stu-id="84d00-108">PowerShell</span></span>](data-factory-copy-activity-tutorial-using-powershell.md)
> * [<span data-ttu-id="84d00-109">Azure Resource Manager-sablon</span><span class="sxs-lookup"><span data-stu-id="84d00-109">Azure Resource Manager template</span></span>](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [<span data-ttu-id="84d00-110">REST API</span><span class="sxs-lookup"><span data-stu-id="84d00-110">REST API</span></span>](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [<span data-ttu-id="84d00-111">.NET API</span><span class="sxs-lookup"><span data-stu-id="84d00-111">.NET API</span></span>](data-factory-copy-activity-tutorial-using-dotnet-api.md)

<span data-ttu-id="84d00-112">Ebből a cikkből megismerheti, hogyan toouse [.NET API](https://portal.azure.com) toocreate egy adat-előállítót, és egy folyamatot, amely másol adatokat az Azure blob storage tooan Azure SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="84d00-112">In this article, you learn how toouse [.NET API](https://portal.azure.com) toocreate a data factory with a pipeline that copies data from an Azure blob storage tooan Azure SQL database.</span></span> <span data-ttu-id="84d00-113">Ha új tooAzure adat-előállítót, olvassa végig hello [Data Factory bemutatása tooAzure](data-factory-introduction.md) cikk Ez az oktatóanyag elvégzése előtt.</span><span class="sxs-lookup"><span data-stu-id="84d00-113">If you are new tooAzure Data Factory, read through hello [Introduction tooAzure Data Factory](data-factory-introduction.md) article before doing this tutorial.</span></span>   

<span data-ttu-id="84d00-114">Az oktatóanyag segítségével egyetlen tevékenységgel (másolási tevékenységgel) rendelkező folyamatot hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="84d00-114">In this tutorial, you create a pipeline with one activity in it: Copy Activity.</span></span> <span data-ttu-id="84d00-115">hello másolási tevékenység során a támogatott adatokat tároló tooa támogatott fogadó adatokat tároló másol adatokat.</span><span class="sxs-lookup"><span data-stu-id="84d00-115">hello copy activity copies data from a supported data store tooa supported sink data store.</span></span> <span data-ttu-id="84d00-116">A forrásként és fogadóként támogatott adattárak listájáért lásd: [támogatott adattárak](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="84d00-116">For a list of data stores supported as sources and sinks, see [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span> <span data-ttu-id="84d00-117">hello tevékenység egy globálisan elérhető szolgáltatás közötti biztonságos, megbízható és skálázható módon különböző adattárolókhoz másolhatja van-e kapcsolva.</span><span class="sxs-lookup"><span data-stu-id="84d00-117">hello activity is powered by a globally available service that can copy data between various data stores in a secure, reliable, and scalable way.</span></span> <span data-ttu-id="84d00-118">További információ a másolási tevékenység hello: [adatok mozgása tevékenységek](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="84d00-118">For more information about hello Copy Activity, see [Data Movement Activities](data-factory-data-movement-activities.md).</span></span>

<span data-ttu-id="84d00-119">Egy folyamathoz több tevékenység is tartozhat.</span><span class="sxs-lookup"><span data-stu-id="84d00-119">A pipeline can have more than one activity.</span></span> <span data-ttu-id="84d00-120">És láncolt telepítését (egymás után futtatni egy tevékenységet) két tevékenység által hello bemeneti hello az adatkészlet többi tevékenység hello kimeneti adatkészlet egy tevékenység beállítását.</span><span class="sxs-lookup"><span data-stu-id="84d00-120">And, you can chain two activities (run one activity after another) by setting hello output dataset of one activity as hello input dataset of hello other activity.</span></span> <span data-ttu-id="84d00-121">További információért lásd: [egy folyamaton belüli több tevékenység](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span><span class="sxs-lookup"><span data-stu-id="84d00-121">For more information, see [multiple activities in a pipeline](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span></span> 

> [!NOTE] 
> <span data-ttu-id="84d00-122">A Data Factory .NET API teljes dokumentációját a [Data Factory szolgáltatással kapcsolatos .NET API-referencia](/dotnet/api/index?view=azuremgmtdatafactories-4.12.1) tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="84d00-122">For complete documentation on .NET API for Data Factory, see [Data Factory .NET API Reference](/dotnet/api/index?view=azuremgmtdatafactories-4.12.1).</span></span>
> 
> <span data-ttu-id="84d00-123">hello adatok feldolgozási sor az oktatóanyag a forrás adatokat tároló tooa cél adatokat tároló másol adatokat.</span><span class="sxs-lookup"><span data-stu-id="84d00-123">hello data pipeline in this tutorial copies data from a source data store tooa destination data store.</span></span> <span data-ttu-id="84d00-124">Hogyan oktatóanyagért tootransform adatok Azure Data Factory használatával, lásd: [oktatóanyag: a folyamat tootransform adatokat Hadoop-fürt létrehozása](data-factory-build-your-first-pipeline.md).</span><span class="sxs-lookup"><span data-stu-id="84d00-124">For a tutorial on how tootransform data using Azure Data Factory, see [Tutorial: Build a pipeline tootransform data using Hadoop cluster](data-factory-build-your-first-pipeline.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="84d00-125">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="84d00-125">Prerequisites</span></span>
* <span data-ttu-id="84d00-126">Lépkedjen végig [oktatóanyag áttekintése és a szükséges előfeltételek](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) tooget hello oktatóanyag és teljes hello áttekintését **előfeltétel** lépéseket.</span><span class="sxs-lookup"><span data-stu-id="84d00-126">Go through [Tutorial Overview and Pre-requisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) tooget an overview of hello tutorial and complete hello **prerequisite** steps.</span></span>
* <span data-ttu-id="84d00-127">Visual Studio 2012, 2013 vagy 2015</span><span class="sxs-lookup"><span data-stu-id="84d00-127">Visual Studio 2012 or 2013 or 2015</span></span>
* <span data-ttu-id="84d00-128">Az [Azure .NET SDK](http://azure.microsoft.com/downloads/) letöltése és telepítése.</span><span class="sxs-lookup"><span data-stu-id="84d00-128">Download and install [Azure .NET SDK](http://azure.microsoft.com/downloads/)</span></span>
* <span data-ttu-id="84d00-129">Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="84d00-129">Azure PowerShell.</span></span> <span data-ttu-id="84d00-130">Kövesse az utasításokat [hogyan tooinstall és konfigurálja az Azure Powershellt](../powershell-install-configure.md) tooinstall Azure PowerShell, a számítógépen a következő cikket.</span><span class="sxs-lookup"><span data-stu-id="84d00-130">Follow instructions in [How tooinstall and configure Azure PowerShell](../powershell-install-configure.md) article tooinstall Azure PowerShell on your computer.</span></span> <span data-ttu-id="84d00-131">Használhatja az Azure PowerShell toocreate egy Azure Active Directory-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="84d00-131">You use Azure PowerShell toocreate an Azure Active Directory application.</span></span>

### <a name="create-an-application-in-azure-active-directory"></a><span data-ttu-id="84d00-132">Alkalmazás létrehozása az Azure Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="84d00-132">Create an application in Azure Active Directory</span></span>
<span data-ttu-id="84d00-133">Hozzon létre egy Azure Active Directory-alkalmazás, hello alkalmazás szolgáltatásnevet létrehozni, és rendelje hozzá toohello **Data Factory közreműködői** szerepkör.</span><span class="sxs-lookup"><span data-stu-id="84d00-133">Create an Azure Active Directory application, create a service principal for hello application, and assign it toohello **Data Factory Contributor** role.</span></span>

1. <span data-ttu-id="84d00-134">Indítsa el a **PowerShellt**.</span><span class="sxs-lookup"><span data-stu-id="84d00-134">Launch **PowerShell**.</span></span>
2. <span data-ttu-id="84d00-135">Futtassa a következő parancs hello, és írja be a hello felhasználónevet és jelszót toosign használhatja a toohello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="84d00-135">Run hello following command and enter hello user name and password that you use toosign in toohello Azure portal.</span></span>

    ```PowerShell
    Login-AzureRmAccount
    ```
3. <span data-ttu-id="84d00-136">Futtassa a következő parancs tooview hello minden hello előfizetés ehhez a fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="84d00-136">Run hello following command tooview all hello subscriptions for this account.</span></span>

    ```PowerShell
    Get-AzureRmSubscription
    ```
4. <span data-ttu-id="84d00-137">Futtassa a következő parancs tooselect hello előfizetést, amelyet a toowork hello.</span><span class="sxs-lookup"><span data-stu-id="84d00-137">Run hello following command tooselect hello subscription that you want toowork with.</span></span> <span data-ttu-id="84d00-138">Cserélje le  **&lt;NameOfAzureSubscription** &gt; hello nevet, az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="84d00-138">Replace **&lt;NameOfAzureSubscription**&gt; with hello name of your Azure subscription.</span></span>

    ```PowerShell
    Get-AzureRmSubscription -SubscriptionName <NameOfAzureSubscription> | Set-AzureRmContext
    ```

   > [!IMPORTANT]
   > <span data-ttu-id="84d00-139">Jegyezze fel **SubscriptionId** és **TenantId** a parancs hello kimenetből.</span><span class="sxs-lookup"><span data-stu-id="84d00-139">Note down **SubscriptionId** and **TenantId** from hello output of this command.</span></span>

5. <span data-ttu-id="84d00-140">Hozzon létre egy Azure erőforráscsoport nevű **ADFTutorialResourceGroup** a következő parancsot a hello PowerShell hello futtatásával.</span><span class="sxs-lookup"><span data-stu-id="84d00-140">Create an Azure resource group named **ADFTutorialResourceGroup** by running hello following command in hello PowerShell.</span></span>

    ```PowerShell
    New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"
    ```

    <span data-ttu-id="84d00-141">Ha hello erőforráscsoportban már létezik, akkor adja meg, hogy tooupdate (Y), vagy legyen (n).</span><span class="sxs-lookup"><span data-stu-id="84d00-141">If hello resource group already exists, you specify whether tooupdate it (Y) or keep it as (N).</span></span>

    <span data-ttu-id="84d00-142">Ha egy másik erőforráscsoportban található használja, ebben az oktatóanyagban kell toouse hello ADFTutorialResourceGroup helyett az erőforráscsoport nevét.</span><span class="sxs-lookup"><span data-stu-id="84d00-142">If you use a different resource group, you need toouse hello name of your resource group in place of ADFTutorialResourceGroup in this tutorial.</span></span>
6. <span data-ttu-id="84d00-143">Hozzon létre egy Azure Active Directory-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="84d00-143">Create an Azure Active Directory application.</span></span>

    ```PowerShell
    $azureAdApplication = New-AzureRmADApplication -DisplayName "ADFCopyTutotiralApp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.adfcopytutorialapp.org/example" -Password "Pass@word1"
    ```

    <span data-ttu-id="84d00-144">Ha hiba történt a következő hello, adjon meg egy másik URL-címet, majd futtassa újra hello parancsot.</span><span class="sxs-lookup"><span data-stu-id="84d00-144">If you get hello following error, specify a different URL and run hello command again.</span></span>
    
    ```PowerShell
    Another object with hello same value for property identifierUris already exists.
    ```
7. <span data-ttu-id="84d00-145">Hello AD szolgáltatásnevet létrehozni.</span><span class="sxs-lookup"><span data-stu-id="84d00-145">Create hello AD service principal.</span></span>

    ```PowerShell
    New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId
    ```
8. <span data-ttu-id="84d00-146">Adja hozzá a szolgáltatás egyszerű toohello **Data Factory közreműködői** szerepkör.</span><span class="sxs-lookup"><span data-stu-id="84d00-146">Add service principal toohello **Data Factory Contributor** role.</span></span>

    ```PowerShell
    New-AzureRmRoleAssignment -RoleDefinitionName "Data Factory Contributor" -ServicePrincipalName $azureAdApplication.ApplicationId.Guid
    ```
9. <span data-ttu-id="84d00-147">Első hello azonosítót.</span><span class="sxs-lookup"><span data-stu-id="84d00-147">Get hello application ID.</span></span>

    ```PowerShell
    $azureAdApplication 
    ```
    <span data-ttu-id="84d00-148">Jegyezze fel hello Alkalmazásazonosítót (applicationID) hello kimenetből.</span><span class="sxs-lookup"><span data-stu-id="84d00-148">Note down hello application ID (applicationID) from hello output.</span></span>

<span data-ttu-id="84d00-149">A fenti lépések elvégzésével beszereztük az alábbi négy értéket:</span><span class="sxs-lookup"><span data-stu-id="84d00-149">You should have following four values from these steps:</span></span>

* <span data-ttu-id="84d00-150">Bérlőazonosító</span><span class="sxs-lookup"><span data-stu-id="84d00-150">Tenant ID</span></span>
* <span data-ttu-id="84d00-151">Előfizetés azonosítója</span><span class="sxs-lookup"><span data-stu-id="84d00-151">Subscription ID</span></span>
* <span data-ttu-id="84d00-152">Alkalmazásazonosító</span><span class="sxs-lookup"><span data-stu-id="84d00-152">Application ID</span></span>
* <span data-ttu-id="84d00-153">Jelszó (hello első parancsban megadott)</span><span class="sxs-lookup"><span data-stu-id="84d00-153">Password (specified in hello first command)</span></span>

## <a name="walkthrough"></a><span data-ttu-id="84d00-154">Útmutatás</span><span class="sxs-lookup"><span data-stu-id="84d00-154">Walkthrough</span></span>
1. <span data-ttu-id="84d00-155">Hozzon létre egy a C# nyelvet használó .NET-konzolalkalmazást a Visual Studio 2012/2013/2015 segítségével.</span><span class="sxs-lookup"><span data-stu-id="84d00-155">Using Visual Studio 2012/2013/2015, create a C# .NET console application.</span></span>
   1. <span data-ttu-id="84d00-156">Indítsa el a **Visual Studio** 2012/2013/2015-at.</span><span class="sxs-lookup"><span data-stu-id="84d00-156">Launch **Visual Studio** 2012/2013/2015.</span></span>
   2. <span data-ttu-id="84d00-157">Kattintson a **fájl**, pont túl**új**, és kattintson a **projekt**.</span><span class="sxs-lookup"><span data-stu-id="84d00-157">Click **File**, point too**New**, and click **Project**.</span></span>
   3. <span data-ttu-id="84d00-158">Bontsa ki a **Sablonok** lehetőséget, és válassza a **Visual C#** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="84d00-158">Expand **Templates**, and select **Visual C#**.</span></span> <span data-ttu-id="84d00-159">Ebben az útmutatóban a C#-t fogjuk használni, de bármelyik más .NET-nyelv is alkalmazható.</span><span class="sxs-lookup"><span data-stu-id="84d00-159">In this walkthrough, you use C#, but you can use any .NET language.</span></span>
   4. <span data-ttu-id="84d00-160">Válassza ki **Konzolalkalmazás** jobb hello projekttípusok hello listája.</span><span class="sxs-lookup"><span data-stu-id="84d00-160">Select **Console Application** from hello list of project types on hello right.</span></span>
   5. <span data-ttu-id="84d00-161">Adja meg **DataFactoryAPITestApp** a hello nevét.</span><span class="sxs-lookup"><span data-stu-id="84d00-161">Enter **DataFactoryAPITestApp** for hello Name.</span></span>
   6. <span data-ttu-id="84d00-162">Válassza ki **C:\ADFGetStarted** hello hely számára.</span><span class="sxs-lookup"><span data-stu-id="84d00-162">Select **C:\ADFGetStarted** for hello Location.</span></span>
   7. <span data-ttu-id="84d00-163">Kattintson a **OK** toocreate hello projekt.</span><span class="sxs-lookup"><span data-stu-id="84d00-163">Click **OK** toocreate hello project.</span></span>
2. <span data-ttu-id="84d00-164">Kattintson a **eszközök**, pont túl**NuGet-Csomagkezelő**, és kattintson a **Csomagkezelő konzol**.</span><span class="sxs-lookup"><span data-stu-id="84d00-164">Click **Tools**, point too**NuGet Package Manager**, and click **Package Manager Console**.</span></span>
3. <span data-ttu-id="84d00-165">A hello **Csomagkezelő konzol**, hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="84d00-165">In hello **Package Manager Console**, do hello following steps:</span></span>
   1. <span data-ttu-id="84d00-166">Futtassa a következő parancs tooinstall adat-előállító csomag hello:`Install-Package Microsoft.Azure.Management.DataFactories`</span><span class="sxs-lookup"><span data-stu-id="84d00-166">Run hello following command tooinstall Data Factory package: `Install-Package Microsoft.Azure.Management.DataFactories`</span></span>
   2. <span data-ttu-id="84d00-167">Futtassa a következő parancs tooinstall Azure Active Directory csomagot (Active Directory API-t használja a hello kódban) hello:`Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.19.208020213`</span><span class="sxs-lookup"><span data-stu-id="84d00-167">Run hello following command tooinstall Azure Active Directory package (you use Active Directory API in hello code): `Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.19.208020213`</span></span>
4. <span data-ttu-id="84d00-168">Adja hozzá a következő hello **appSetttings** szakasz toohello **App.config** fájlt.</span><span class="sxs-lookup"><span data-stu-id="84d00-168">Add hello following **appSetttings** section toohello **App.config** file.</span></span> <span data-ttu-id="84d00-169">Ezek a beállítások használhatók hello segítő módszerrel: **GetAuthorizationHeader**.</span><span class="sxs-lookup"><span data-stu-id="84d00-169">These settings are used by hello helper method: **GetAuthorizationHeader**.</span></span>

    <span data-ttu-id="84d00-170">Az **&lt;Application ID&gt;**, **&lt;Password&gt;**, **&lt;Subscription ID&gt;** és **&lt;tenant ID&gt;** értékek helyére írja be saját értékeit.</span><span class="sxs-lookup"><span data-stu-id="84d00-170">Replace values for **&lt;Application ID&gt;**, **&lt;Password&gt;**, **&lt;Subscription ID&gt;**, and **&lt;tenant ID&gt;** with your own values.</span></span>

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

5. <span data-ttu-id="84d00-171">Adja hozzá a következő hello **használatával** utasítások toohello forrásfájl (Program.cs) hello projektben.</span><span class="sxs-lookup"><span data-stu-id="84d00-171">Add hello following **using** statements toohello source file (Program.cs) in hello project.</span></span>

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

6. <span data-ttu-id="84d00-172">Adja hozzá a következő kódot, amely létrehoz egy új hello **DataPipelineManagementClient** osztály toohello **fő** metódust.</span><span class="sxs-lookup"><span data-stu-id="84d00-172">Add hello following code that creates an instance of **DataPipelineManagementClient** class toohello **Main** method.</span></span> <span data-ttu-id="84d00-173">Egy adat-előállító, csatolt szolgáltatásként, bemeneti és kimeneti adatkészletek és egy folyamatot ezen objektum toocreate használhatja.</span><span class="sxs-lookup"><span data-stu-id="84d00-173">You use this object toocreate a data factory, a linked service, input and output datasets, and a pipeline.</span></span> <span data-ttu-id="84d00-174">Az objektum toomonitor szeletek a DataSet adatkészlet futtatás közben is használ.</span><span class="sxs-lookup"><span data-stu-id="84d00-174">You also use this object toomonitor slices of a dataset at runtime.</span></span>

    ```csharp
    // create data factory management client
    string resourceGroupName = "ADFTutorialResourceGroup";
    string dataFactoryName = "APITutorialFactory";

    TokenCloudCredentials aadTokenCredentials = new TokenCloudCredentials(
            ConfigurationManager.AppSettings["SubscriptionId"],
            GetAuthorizationHeader().Result);

    Uri resourceManagerUri = new Uri(ConfigurationManager.AppSettings["ResourceManagerEndpoint"]);

    DataFactoryManagementClient client = new DataFactoryManagementClient(aadTokenCredentials, resourceManagerUri);
    ```

   > [!IMPORTANT]
   > <span data-ttu-id="84d00-175">Cserélje le a hello értékének **resourceGroupName** hello nevet, az Azure-erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="84d00-175">Replace hello value of **resourceGroupName** with hello name of your Azure resource group.</span></span>
   >
   > <span data-ttu-id="84d00-176">Frissítés hello data factory (dataFactoryName) toobe egyedi neve.</span><span class="sxs-lookup"><span data-stu-id="84d00-176">Update name of hello data factory (dataFactoryName) toobe unique.</span></span> <span data-ttu-id="84d00-177">Hello adat-előállító nevét globálisan egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="84d00-177">Name of hello data factory must be globally unique.</span></span> <span data-ttu-id="84d00-178">A Data Factory-összetevők elnevezési szabályait a [Data Factory - Naming Rules](data-factory-naming-rules.md) (Data Factory – Elnevezési szabályok) című témakörben találhatja.</span><span class="sxs-lookup"><span data-stu-id="84d00-178">See [Data Factory - Naming Rules](data-factory-naming-rules.md) topic for naming rules for Data Factory artifacts.</span></span>

7. <span data-ttu-id="84d00-179">Adja hozzá a kódot, amely létrehozza a következő hello egy **adat-előállító** toohello **fő** metódust.</span><span class="sxs-lookup"><span data-stu-id="84d00-179">Add hello following code that creates a **data factory** toohello **Main** method.</span></span>

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

    <span data-ttu-id="84d00-180">A data factory egy vagy több folyamattal rendelkezhet.</span><span class="sxs-lookup"><span data-stu-id="84d00-180">A data factory can have one or more pipelines.</span></span> <span data-ttu-id="84d00-181">A folyamaton belül egy vagy több tevékenység lehet.</span><span class="sxs-lookup"><span data-stu-id="84d00-181">A pipeline can have one or more activities in it.</span></span> <span data-ttu-id="84d00-182">Adja meg például a másolási tevékenység toocopy forrás tooa cél adattárat és adatait egy HDInsight Hive tevékenység toorun a Hive parancsfájl tootransform adatok tooproduct kimeneti adatokat.</span><span class="sxs-lookup"><span data-stu-id="84d00-182">For example, a Copy Activity toocopy data from a source tooa destination data store and a HDInsight Hive activity toorun a Hive script tootransform input data tooproduct output data.</span></span> <span data-ttu-id="84d00-183">Kezdjük, az ebben a lépésben hello adat-előállító létrehozása.</span><span class="sxs-lookup"><span data-stu-id="84d00-183">Let's start with creating hello data factory in this step.</span></span>
8. <span data-ttu-id="84d00-184">Adja hozzá a kódot, amely létrehozza a következő hello egy **Azure Storage társított szolgáltatásnak** toohello **fő** metódust.</span><span class="sxs-lookup"><span data-stu-id="84d00-184">Add hello following code that creates an **Azure Storage linked service** toohello **Main** method.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="84d00-185">A **storageaccountname** és az **accountkey** értékek helyére írja be Azure Storage-tárfiókja nevét, illetve kulcsát.</span><span class="sxs-lookup"><span data-stu-id="84d00-185">Replace **storageaccountname** and **accountkey** with name and key of your Azure Storage account.</span></span>

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

    <span data-ttu-id="84d00-186">A data factory toolink adatait tárolja, és számítási szolgáltatások toohello adat-előállító létrehozása társított szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="84d00-186">You create linked services in a data factory toolink your data stores and compute services toohello data factory.</span></span> <span data-ttu-id="84d00-187">Ebben az oktatóanyagban nem használunk számítási szolgáltatásokat (például Azure HDInsight vagy Azure Data Lake Analytics).</span><span class="sxs-lookup"><span data-stu-id="84d00-187">In this tutorial, you don't use any compute service such as Azure HDInsight or Azure Data Lake Analytics.</span></span> <span data-ttu-id="84d00-188">Csak kétféle típusú adattárat használunk: Azure Storage (forrás) és Azure SQL Database (cél).</span><span class="sxs-lookup"><span data-stu-id="84d00-188">You use two data stores of type Azure Storage (source) and Azure SQL Database (destination).</span></span> 

    <span data-ttu-id="84d00-189">Ezért két társított szolgáltatást fog létrehozni AzureStorageLinkedService és AzureSqlLinkedService néven (típus: AzureStorage és AzureSqlDatabase).</span><span class="sxs-lookup"><span data-stu-id="84d00-189">Therefore, you create two linked services named AzureStorageLinkedService and AzureSqlLinkedService of types: AzureStorage and AzureSqlDatabase.</span></span>  

    <span data-ttu-id="84d00-190">hello AzureStorageLinkedService művelet ugyan összeköti a az Azure storage-fiók toohello adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="84d00-190">hello AzureStorageLinkedService links your Azure storage account toohello data factory.</span></span> <span data-ttu-id="84d00-191">Ez a tárfiók egy hello akkor létrejött a tároló és hello adatok részeként feltöltött [Előfeltételek](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="84d00-191">This storage account is hello one in which you created a container and uploaded hello data as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>
9. <span data-ttu-id="84d00-192">Adja hozzá a kódot, amely létrehozza a következő hello egy **Azure SQL társított szolgáltatásnak** toohello **fő** metódust.</span><span class="sxs-lookup"><span data-stu-id="84d00-192">Add hello following code that creates an **Azure SQL linked service** toohello **Main** method.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="84d00-193">A **servername**, **databasename**, **username** és **password** paraméterek értékét cserélje le az Azure SQL-kiszolgáló, az adatbázis és a felhasználói fiók nevére, valamint a felhasználói fiók jelszavára.</span><span class="sxs-lookup"><span data-stu-id="84d00-193">Replace **servername**, **databasename**, **username**, and **password** with names of your Azure SQL server, database, user, and password.</span></span>

    ```csharp
    // create a linked service for output data store: Azure SQL Database
    Console.WriteLine("Creating Azure SQL Database linked service");
    client.LinkedServices.CreateOrUpdate(resourceGroupName, dataFactoryName,
        new LinkedServiceCreateOrUpdateParameters()
        {
            LinkedService = new LinkedService()
            {
                Name = "AzureSqlLinkedService",
                Properties = new LinkedServiceProperties
                (
                    new AzureSqlDatabaseLinkedService("Data Source=tcp:<servername>.database.windows.net,1433;Initial Catalog=<databasename>;User ID=<username>;Password=<password>;Integrated Security=False;Encrypt=True;Connect Timeout=30")
                )
            }
        }
    );
    ```

    <span data-ttu-id="84d00-194">AzureSqlLinkedService az Azure SQL adatbázis toohello adat-előállító hivatkozásokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="84d00-194">AzureSqlLinkedService links your Azure SQL database toohello data factory.</span></span> <span data-ttu-id="84d00-195">hello blobtárolóból másolt hello adatok az adatbázis tárolja.</span><span class="sxs-lookup"><span data-stu-id="84d00-195">hello data that is copied from hello blob storage is stored in this database.</span></span> <span data-ttu-id="84d00-196">Hello üres tábla az adatbázisban részeként létrehozott [Előfeltételek](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="84d00-196">You created hello emp table in this database as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>
10. <span data-ttu-id="84d00-197">Adja hozzá a kódot, amely létrehozza a következő hello **bemeneti és kimeneti adatkészletek** toohello **fő** metódust.</span><span class="sxs-lookup"><span data-stu-id="84d00-197">Add hello following code that creates **input and output datasets** toohello **Main** method.</span></span>

    ```csharp
    // create input and output datasets
    Console.WriteLine("Creating input and output datasets");
    string Dataset_Source = "InputDataset";
    string Dataset_Destination = "OutputDataset";

    Console.WriteLine("Creating input dataset of type: Azure Blob");
    client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,

    new DatasetCreateOrUpdateParameters()
    {
        Dataset = new Dataset()
        {
            Name = Dataset_Source,
            Properties = new DatasetProperties()
            {
                Structure = new List<DataElement>()
                {
                    new DataElement() { Name = "FirstName", Type = "String" },
                    new DataElement() { Name = "LastName", Type = "String" }
                },
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

    Console.WriteLine("Creating output dataset of type: Azure SQL");
    client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,
        new DatasetCreateOrUpdateParameters()
        {
            Dataset = new Dataset()
            {
                Name = Dataset_Destination,
                Properties = new DatasetProperties()
                {
                    Structure = new List<DataElement>()
                    {
                        new DataElement() { Name = "FirstName", Type = "String" },
                        new DataElement() { Name = "LastName", Type = "String" }
                    },
                    LinkedServiceName = "AzureSqlLinkedService",
                    TypeProperties = new AzureSqlTableDataset()
                    {
                        TableName = "emp"
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
    
    <span data-ttu-id="84d00-198">Hello előző lépésben létrehozott összekapcsolt szolgáltatások toolink az Azure Storage-fiók és az Azure SQL adatbázis tooyour adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="84d00-198">In hello previous step, you created linked services toolink your Azure Storage account and Azure SQL database tooyour data factory.</span></span> <span data-ttu-id="84d00-199">Ebben a lépésben InputDataset és OutputDataset, amelyek megfelelnek a bemeneti és kimeneti adatok AzureStorageLinkedService és AzureSqlLinkedService által hivatkozott hello adattárolókhoz tárolt nevű két adatkészletet határozza meg.</span><span class="sxs-lookup"><span data-stu-id="84d00-199">In this step, you define two datasets named InputDataset and OutputDataset that represent input and output data that is stored in hello data stores referred by AzureStorageLinkedService and AzureSqlLinkedService respectively.</span></span>

    <span data-ttu-id="84d00-200">hello Azure tárolás társított szolgáltatásának határozza meg a Data Factory szolgáltatásnak a futási idő tooconnect tooyour Azure storage-fiók használó hello kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="84d00-200">hello Azure storage linked service specifies hello connection string that Data Factory service uses at run time tooconnect tooyour Azure storage account.</span></span> <span data-ttu-id="84d00-201">És hello bemeneti blob-adathalmazra (InputDataset) hello tároló és hello bemeneti adatokat tartalmazó hello mappát adja meg.</span><span class="sxs-lookup"><span data-stu-id="84d00-201">And, hello input blob dataset (InputDataset) specifies hello container and hello folder that contains hello input data.</span></span>  

    <span data-ttu-id="84d00-202">Hello csatolt Azure SQL Database szolgáltatáshoz hasonlóan hello kapcsolati karakterláncot használó Data Factory szolgáltatásnak a futási idő tooconnect tooyour Azure SQL adatbázis határozza meg.</span><span class="sxs-lookup"><span data-stu-id="84d00-202">Similarly, hello Azure SQL Database linked service specifies hello connection string that Data Factory service uses at run time tooconnect tooyour Azure SQL database.</span></span> <span data-ttu-id="84d00-203">És hello kimeneti SQL táblázat dataset (OututDataset) hello adatbázis toowhich hello hello blobtárolóból az adatok másolásakor hello tábla határozza meg.</span><span class="sxs-lookup"><span data-stu-id="84d00-203">And, hello output SQL table dataset (OututDataset) specifies hello table in hello database toowhich hello data from hello blob storage is copied.</span></span>

    <span data-ttu-id="84d00-204">Ebben a lépésben hoz létre, mely tooa fájlját (emp.txt) InputDataset nevű adatkészlete hello gyökérmappájában lévő mappának a blob-tároló (adftutorial) az Azure Storage hello AzureStorageLinkedService kapcsolódó szolgáltatás által képviselt hello.</span><span class="sxs-lookup"><span data-stu-id="84d00-204">In this step, you create a dataset named InputDataset that points tooa blob file (emp.txt) in hello root folder of a blob container (adftutorial) in hello Azure Storage represented by hello AzureStorageLinkedService linked service.</span></span> <span data-ttu-id="84d00-205">Ha nem hello fájlnév értéket (vagy hagyja ki), a hello bemeneti mappában található összes BLOB adatait is másolt toohello cél.</span><span class="sxs-lookup"><span data-stu-id="84d00-205">If you don't specify a value for hello fileName (or skip it), data from all blobs in hello input folder are copied toohello destination.</span></span> <span data-ttu-id="84d00-206">Ebben az oktatóanyagban hello fájlnév értéket kell megadni.</span><span class="sxs-lookup"><span data-stu-id="84d00-206">In this tutorial, you specify a value for hello fileName.</span></span>    

    <span data-ttu-id="84d00-207">Ebben a lépésben egy kimeneti adatkészletet hoz létre **OutputDataset** néven.</span><span class="sxs-lookup"><span data-stu-id="84d00-207">In this step, you create an output dataset named **OutputDataset**.</span></span> <span data-ttu-id="84d00-208">Ez az adatkészlet mutat tooa SQL táblázat hello Azure SQL Database által képviselt **AzureSqlLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="84d00-208">This dataset points tooa SQL table in hello Azure SQL database represented by **AzureSqlLinkedService**.</span></span>
11. <span data-ttu-id="84d00-209">Adja hozzá a következő hello-kód **hoz létre, és aktiválja az adatcsatorna** toohello **fő** metódust.</span><span class="sxs-lookup"><span data-stu-id="84d00-209">Add hello following code that **creates and activates a pipeline** toohello **Main** method.</span></span> <span data-ttu-id="84d00-210">Ebben a lépésben létrehoz egy **másolási tevékenységgel** rendelkező folyamatot, amely bemenetként az **InputDataset**, kimenetként pedig az **OutputDataset** adatkészletet használja.</span><span class="sxs-lookup"><span data-stu-id="84d00-210">In this step, you create a pipeline with a **copy activity** that uses **InputDataset** as an input and **OutputDataset** as an output.</span></span>

    ```csharp
    // create a pipeline
    Console.WriteLine("Creating a pipeline");
    DateTime PipelineActivePeriodStartTime = new DateTime(2017, 5, 11, 0, 0, 0, 0, DateTimeKind.Utc);
    DateTime PipelineActivePeriodEndTime = new DateTime(2017, 5, 12, 0, 0, 0, 0, DateTimeKind.Utc);
    string PipelineName = "ADFTutorialPipeline";

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
                            Name = "BlobToAzureSql",
                            Inputs = new List<ActivityInput>()
                            {
                                new ActivityInput() {
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
                    }
                }
            }
        });
    ```

    <span data-ttu-id="84d00-211">Vegye figyelembe a következő pontok hello:</span><span class="sxs-lookup"><span data-stu-id="84d00-211">Note hello following points:</span></span>
   
    - <span data-ttu-id="84d00-212">Hello tevékenységek szakaszban csak egy tevékenység nincs amelynek **típus** értéke túl**másolási**.</span><span class="sxs-lookup"><span data-stu-id="84d00-212">In hello activities section, there is only one activity whose **type** is set too**Copy**.</span></span> <span data-ttu-id="84d00-213">Hello másolási tevékenység kapcsolatos további információkért lásd: [adatok mozgása tevékenységek](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="84d00-213">For more information about hello copy activity, see [data movement activities](data-factory-data-movement-activities.md).</span></span> <span data-ttu-id="84d00-214">A Data Factory megoldásaiban használhatja az [adatátalakítási tevékenységeket](data-factory-data-transformation-activities.md) is.</span><span class="sxs-lookup"><span data-stu-id="84d00-214">In Data Factory solutions, you can also use [data transformation activities](data-factory-data-transformation-activities.md).</span></span>
    - <span data-ttu-id="84d00-215">Adjon meg a hello tevékenység értéke túl**InputDataset** és hello tevékenység túl van-e állítva a kimeneti**OutputDataset**.</span><span class="sxs-lookup"><span data-stu-id="84d00-215">Input for hello activity is set too**InputDataset** and output for hello activity is set too**OutputDataset**.</span></span> 
    - <span data-ttu-id="84d00-216">A hello **typeProperties** szakaszban **BlobSource** hello forrás típusaként van megadva, és **SqlSink** hello a fogadó típusa van megadva.</span><span class="sxs-lookup"><span data-stu-id="84d00-216">In hello **typeProperties** section, **BlobSource** is specified as hello source type and **SqlSink** is specified as hello sink type.</span></span> <span data-ttu-id="84d00-217">Források és mosdók hello másolási tevékenység által támogatott adattárolókhoz teljes listáját lásd: [adattárolókhoz támogatott](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="84d00-217">For a complete list of data stores supported by hello copy activity as sources and sinks, see [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span> <span data-ttu-id="84d00-218">Hogyan toouse a támogatott adatokat tárolót, mint a forrás/fogadó toolearn hivatkozásra hello hello táblában.</span><span class="sxs-lookup"><span data-stu-id="84d00-218">toolearn how toouse a specific supported data store as a source/sink, click hello link in hello table.</span></span>  
   
    <span data-ttu-id="84d00-219">Kimeneti adatkészlet jelenleg milyen meghajtók hello ütemezést.</span><span class="sxs-lookup"><span data-stu-id="84d00-219">Currently, output dataset is what drives hello schedule.</span></span> <span data-ttu-id="84d00-220">Ebben az oktatóanyagban a kimeneti adatkészlet óránként konfigurált tooproduce szelet.</span><span class="sxs-lookup"><span data-stu-id="84d00-220">In this tutorial, output dataset is configured tooproduce a slice once an hour.</span></span> <span data-ttu-id="84d00-221">hello folyamat rendelkezik egy kezdési és befejezési időpontja, amelyek egy nap telhet el, amely 24 óra.</span><span class="sxs-lookup"><span data-stu-id="84d00-221">hello pipeline has a start time and end time that are one day apart, which is 24 hours.</span></span> <span data-ttu-id="84d00-222">Ezért a kimeneti adatkészlet 24 szeletek előállított hello folyamat.</span><span class="sxs-lookup"><span data-stu-id="84d00-222">Therefore, 24 slices of output dataset are produced by hello pipeline.</span></span>
12. <span data-ttu-id="84d00-223">Adja hozzá a következő kód toohello hello **fő** metódus tooget hello állapotát egy adatszelet hello a kimeneti adatkészlet.</span><span class="sxs-lookup"><span data-stu-id="84d00-223">Add hello following code toohello **Main** method tooget hello status of a data slice of hello output dataset.</span></span> <span data-ttu-id="84d00-224">Ebben a példában csupán egy szeletet fogunk kapni.</span><span class="sxs-lookup"><span data-stu-id="84d00-224">There is only slice expected in this sample.</span></span>

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

13. <span data-ttu-id="84d00-225">Adja hozzá a következő kód futtatása tooget adatok számára egy adatok szelet toohello hello **fő** metódust.</span><span class="sxs-lookup"><span data-stu-id="84d00-225">Add hello following code tooget run details for a data slice toohello **Main** method.</span></span>

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
            }
        );

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

14. <span data-ttu-id="84d00-226">Adja hozzá a következő hello segítő módszerének hello **fő** metódus toohello **Program** osztály.</span><span class="sxs-lookup"><span data-stu-id="84d00-226">Add hello following helper method used by hello **Main** method toohello **Program** class.</span></span>

    > [!NOTE] 
    > <span data-ttu-id="84d00-227">Másolja és illessze be a kódját a következő hello, győződjön meg arról, hogy hello másolt kód jelenleg hello azonos szinten, hello fő metódus.</span><span class="sxs-lookup"><span data-stu-id="84d00-227">When you copy and paste hello following code, make sure that hello copied code is at hello same level as hello Main method.</span></span>

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

15. <span data-ttu-id="84d00-228">A Megoldáskezelőben hello, bontsa ki a hello project (DataFactoryAPITestApp), kattintson a jobb gombbal **hivatkozások**, és kattintson a **hivatkozás hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="84d00-228">In hello Solution Explorer, expand hello project (DataFactoryAPITestApp), right-click **References**, and click **Add Reference**.</span></span> <span data-ttu-id="84d00-229">Jelölje be a **System.Configuration** szerelvényhez tartozó jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="84d00-229">Select check box for **System.Configuration** assembly.</span></span> <span data-ttu-id="84d00-230">Kattintson **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="84d00-230">and click **OK**.</span></span>
16. <span data-ttu-id="84d00-231">Hello Konzolalkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="84d00-231">Build hello console application.</span></span> <span data-ttu-id="84d00-232">Kattintson a **Build** hello menüre, majd a **megoldás fordítása**.</span><span class="sxs-lookup"><span data-stu-id="84d00-232">Click **Build** on hello menu and click **Build Solution**.</span></span>
17. <span data-ttu-id="84d00-233">Győződjön meg arról, hogy nincs-e legalább egy fájl hello **adftutorial** az Azure blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="84d00-233">Confirm that there is at least one file in hello **adftutorial** container in your Azure blob storage.</span></span> <span data-ttu-id="84d00-234">Ha nem, hozzon létre **Emp.txt** hello tartalom a következő fájlt a Jegyzettömbben, és töltse fel toohello adftutorial tároló.</span><span class="sxs-lookup"><span data-stu-id="84d00-234">If not, create **Emp.txt** file in Notepad with hello following content and upload it toohello adftutorial container.</span></span>

    ```
    John, Doe
    Jane, Doe
    ```
18. <span data-ttu-id="84d00-235">Futtassa a hello minta kattintva **Debug** -> **Start Debugging** hello menüben.</span><span class="sxs-lookup"><span data-stu-id="84d00-235">Run hello sample by clicking **Debug** -> **Start Debugging** on hello menu.</span></span> <span data-ttu-id="84d00-236">Amikor megjelenik a hello **első futtatása egy adatszelet részleteit**, várjon néhány percet, és nyomja le az **ENTER**.</span><span class="sxs-lookup"><span data-stu-id="84d00-236">When you see hello **Getting run details of a data slice**, wait for a few minutes, and press **ENTER**.</span></span>
19. <span data-ttu-id="84d00-237">Használja az Azure portál tooverify hello adott hello adat-előállító **APITutorialFactory** jön létre a következő összetevők hello:</span><span class="sxs-lookup"><span data-stu-id="84d00-237">Use hello Azure portal tooverify that hello data factory **APITutorialFactory** is created with hello following artifacts:</span></span>
   * <span data-ttu-id="84d00-238">Társított szolgáltatás: **LinkedService_AzureStorage**</span><span class="sxs-lookup"><span data-stu-id="84d00-238">Linked service: **LinkedService_AzureStorage**</span></span>
   * <span data-ttu-id="84d00-239">Adatkészlet: **InputDataset** és **OutputDataset**.</span><span class="sxs-lookup"><span data-stu-id="84d00-239">Dataset: **InputDataset** and **OutputDataset**.</span></span>
   * <span data-ttu-id="84d00-240">Adatcsatorna: **PipelineBlobSample**</span><span class="sxs-lookup"><span data-stu-id="84d00-240">Pipeline: **PipelineBlobSample**</span></span>
20. <span data-ttu-id="84d00-241">Győződjön meg arról, hogy a két hello alkalmazott rekordok hello létrejönnek **üres** hello táblázat megadott Azure SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="84d00-241">Verify that hello two employee records are created in hello **emp** table in hello specified Azure SQL database.</span></span>

## <a name="next-steps"></a><span data-ttu-id="84d00-242">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="84d00-242">Next steps</span></span>
<span data-ttu-id="84d00-243">A Data Factory .NET API teljes dokumentációját a [Data Factory szolgáltatással kapcsolatos .NET API-referencia](/dotnet/api/index?view=azuremgmtdatafactories-4.12.1) tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="84d00-243">For complete documentation on .NET API for Data Factory, see [Data Factory .NET API Reference](/dotnet/api/index?view=azuremgmtdatafactories-4.12.1).</span></span>

<span data-ttu-id="84d00-244">Ez az oktatóanyag egy olyan másolási műveletet mutatott be, amelynek a forrásadattára egy Azure Blob Storage-tár, a céladattára pedig egy Azure SQL-adatbázis volt.</span><span class="sxs-lookup"><span data-stu-id="84d00-244">In this tutorial, you used Azure blob storage as a source data store and an Azure SQL database as a destination data store in a copy operation.</span></span> <span data-ttu-id="84d00-245">hello következő táblázat felsorolja az adatforrások és a célhelyek hello másolási tevékenység által támogatott adattárolókhoz:</span><span class="sxs-lookup"><span data-stu-id="84d00-245">hello following table provides a list of data stores supported as sources and destinations by hello copy activity:</span></span> 

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

<span data-ttu-id="84d00-246">Hogyan toocopy adatok belőle egy adatok tárolására, kapcsolatos toolearn hivatkozásra hello hello adattároló hello tábla.</span><span class="sxs-lookup"><span data-stu-id="84d00-246">toolearn about how toocopy data to/from a data store, click hello link for hello data store in hello table.</span></span>

