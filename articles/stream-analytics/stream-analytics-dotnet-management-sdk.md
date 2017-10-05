---
title: "Az Azure Stream Analytics-kezelés .NET SDK |} Microsoft Docs"
description: "Első lépések a Stream Analytics felügyeleti .NET SDK-val. Megtudhatja, hogyan állítson be és az analytics-feladatok futtatásához. Hozzon létre egy projekt, bemenetek, kimenetek és átalakításához."
keywords: .NET SDK-val analytics API
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 5e93de87-0c6f-4f4b-be98-08d63f832897
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/06/2017
ms.author: jeffstok
ms.openlocfilehash: f9aa812e6e82cc0f72d0cd1fe63058e53f794775
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="management-net-sdk-set-up-and-run-analytics-jobs-using-the-azure-stream-analytics-api-for-net"></a><span data-ttu-id="c44ca-106">.NET SDK-kezelés: Beállítása és az Azure Stream Analytics API használatával a .NET-hez analytics-feladatok futtatása</span><span class="sxs-lookup"><span data-stu-id="c44ca-106">Management .NET SDK: Set up and run analytics jobs using the Azure Stream Analytics API for .NET</span></span>
<span data-ttu-id="c44ca-107">Megtudhatja, hogyan állítson be és használja a Stream Analytics API Management .NET SDK használatával .NET analytics-feladatok futtatása.</span><span class="sxs-lookup"><span data-stu-id="c44ca-107">Learn how to set up and run analytics jobs using the Stream Analytics API for .NET using the Management .NET SDK.</span></span> <span data-ttu-id="c44ca-108">Projekt beállítása, hozzon létre a bemeneti és kimeneti adatforrások, átalakítások, és úgy indítsa és feladatok.</span><span class="sxs-lookup"><span data-stu-id="c44ca-108">Set up a project, create input and output sources, transformations, and start and stop jobs.</span></span> <span data-ttu-id="c44ca-109">Az analytics-feladatok adatok Blob-tároló vagy az eseményközpont folyamatos átviteléhez.</span><span class="sxs-lookup"><span data-stu-id="c44ca-109">For your analytics jobs, you can stream data from Blob storage or from an event hub.</span></span>

<span data-ttu-id="c44ca-110">Tekintse meg a [felügyeleti referenciadokumentációt tartalmaz a Stream Analytics API .NET](https://msdn.microsoft.com/library/azure/dn889315.aspx).</span><span class="sxs-lookup"><span data-stu-id="c44ca-110">See the [management reference documentation for the Stream Analytics API for .NET](https://msdn.microsoft.com/library/azure/dn889315.aspx).</span></span>

<span data-ttu-id="c44ca-111">Az Azure Stream Analytics egy olyan teljes körűen felügyelt szolgáltatás biztosít alacsony késésű, magas rendelkezésre állású, méretezhető, összetett Eseményfeldolgozási keresztül a streamelési adatok a felhőben.</span><span class="sxs-lookup"><span data-stu-id="c44ca-111">Azure Stream Analytics is a fully managed service providing low-latency, highly available, scalable, complex event processing over streaming data in the cloud.</span></span> <span data-ttu-id="c44ca-112">A Stream Analytics lehetővé teszi az ügyfelek a folyamatos átviteli feladatok beállítása az adatfolyamokat elemzése, és lehetővé teszi a közel valós idejű elemzési meghajtó.</span><span class="sxs-lookup"><span data-stu-id="c44ca-112">Stream Analytics enables customers to set up streaming jobs to analyze data streams, and allows them to drive near real-time analytics.</span></span>  

> [!NOTE]
> <span data-ttu-id="c44ca-113">Ebben a cikkben a példakód Azure Stream Analytics Management .NET SDK v2.x verziójával lett frissítve.</span><span class="sxs-lookup"><span data-stu-id="c44ca-113">We have updated the sample code in this article with Azure Stream Analytics Management .NET SDK v2.x version.</span></span> <span data-ttu-id="c44ca-114">A által használt lagecy (1.x) SDK-verzió használó példakódot, talál [a Management .NET SDK v1.x használja a Stream Analytics](https://docs.microsoft.com/en-us/azure/stream-analytics/stream-analytics-dotnet-management-sdk-v1).</span><span class="sxs-lookup"><span data-stu-id="c44ca-114">For sample code using the uses lagecy (1.x) SDK version, please see [Use the Management .NET SDK v1.x for Stream Analytics](https://docs.microsoft.com/en-us/azure/stream-analytics/stream-analytics-dotnet-management-sdk-v1).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c44ca-115">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c44ca-115">Prerequisites</span></span>
<span data-ttu-id="c44ca-116">A cikk elkezdéséhez az alábbiakkal kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="c44ca-116">Before you begin this article, you must have the following:</span></span>

* <span data-ttu-id="c44ca-117">Telepítse a Visual Studio 2017 vagy 2015.</span><span class="sxs-lookup"><span data-stu-id="c44ca-117">Install Visual Studio 2017 or 2015.</span></span>
* <span data-ttu-id="c44ca-118">Töltse le és telepítse [Azure .NET SDK](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="c44ca-118">Download and install [Azure .NET SDK](https://azure.microsoft.com/downloads/).</span></span>
* <span data-ttu-id="c44ca-119">Azure-erőforráscsoport létrehozása az előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="c44ca-119">Create an Azure Resource Group in your subscription.</span></span> <span data-ttu-id="c44ca-120">Egy Azure PowerShell-parancsfájlpélda a következő:</span><span class="sxs-lookup"><span data-stu-id="c44ca-120">The following is a sample Azure PowerShell script.</span></span> <span data-ttu-id="c44ca-121">Azure PowerShell információkért lásd: [telepítse és konfigurálja az Azure Powershellt](/powershell/azure/overview);</span><span class="sxs-lookup"><span data-stu-id="c44ca-121">For Azure PowerShell information, see [Install and configure Azure PowerShell](/powershell/azure/overview);</span></span>  

        # Log in to your Azure account
        Add-AzureAccount

        # Select the Azure subscription you want to use to create the resource group
        Select-AzureSubscription -SubscriptionName <subscription name>

            # If Stream Analytics has not been registered to the subscription, remove the remark symbol (#) to run the Register-AzureRMProvider cmdlet to register the provider namespace
            #Register-AzureRMProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'

        # Create an Azure resource group
        New-AzureResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>


* <span data-ttu-id="c44ca-122">Beállítása egy bemeneti forrás- és kimeneti használatára.</span><span class="sxs-lookup"><span data-stu-id="c44ca-122">Set up an input source and output target to use.</span></span> <span data-ttu-id="c44ca-123">A további tudnivalókat [bemenet hozzáadása](stream-analytics-add-inputs.md) állíthat be egy minta bemeneti és [hozzáadása kimenetek](stream-analytics-add-outputs.md) állíthat be egy minta kimenet.</span><span class="sxs-lookup"><span data-stu-id="c44ca-123">For further instructions see [Add Inputs](stream-analytics-add-inputs.md) to set up a sample input and [Add Outputs](stream-analytics-add-outputs.md) to set up a sample output.</span></span>

## <a name="set-up-a-project"></a><span data-ttu-id="c44ca-124">Projekt beállítása</span><span class="sxs-lookup"><span data-stu-id="c44ca-124">Set up a project</span></span>
<span data-ttu-id="c44ca-125">Az analytics-feladat használja a Stream Analytics API a .NET-hez, először állítsa be a projekthez.</span><span class="sxs-lookup"><span data-stu-id="c44ca-125">To create an analytics job use the Stream Analytics API for .NET, first set up your project.</span></span>

1. <span data-ttu-id="c44ca-126">Hozzon létre egy Visual Studio C# .NET konzolalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="c44ca-126">Create a Visual Studio C# .NET console application.</span></span>
2. <span data-ttu-id="c44ca-127">A Package Manager-konzolon, a következő parancsokat a NuGet-csomagok.</span><span class="sxs-lookup"><span data-stu-id="c44ca-127">In the Package Manager Console, run the following commands to install the NuGet packages.</span></span> <span data-ttu-id="c44ca-128">Az első címtárra az Azure Stream Analytics felügyeleti .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="c44ca-128">The first one is the Azure Stream Analytics Management .NET SDK.</span></span> <span data-ttu-id="c44ca-129">A második érték van az Azure ügyfél-hitelesítéshez.</span><span class="sxs-lookup"><span data-stu-id="c44ca-129">The second one is for Azure client authentication.</span></span>
   
        Install-Package Microsoft.Azure.Management.StreamAnalytics -Version 2.0.0
        Install-Package Microsoft.Rest.ClientRuntime.Azure.Authentication -Version 2.3.1
3. <span data-ttu-id="c44ca-130">Adja hozzá a következő **appSettings** szakaszt az App.config fájlban:</span><span class="sxs-lookup"><span data-stu-id="c44ca-130">Add the following **appSettings** section to the App.config file:</span></span>
   
        <appSettings>
          <add key="ClientId" value="1950a258-227b-4e31-a9cf-717495945fc2" />
          <add key="RedirectUri" value="urn:ietf:wg:oauth:2.0:oob" />
          <add key="SubscriptionId" value="YOUR SUBSCRIPTION ID" />
          <add key="ActiveDirectoryTenantId" value="YOUR TENANT ID" />
        </appSettings>

    <span data-ttu-id="c44ca-131">Cserélje le az értékeket **SubscriptionId** és **ActiveDirectoryTenantId** a Azure-előfizetés és a bérlői azonosítók.</span><span class="sxs-lookup"><span data-stu-id="c44ca-131">Replace values for **SubscriptionId** and **ActiveDirectoryTenantId** with your Azure subscription and tenant IDs.</span></span> <span data-ttu-id="c44ca-132">Ezeket az értékeket a következő Azure PowerShell-parancsmag futtatásával kérhető le:</span><span class="sxs-lookup"><span data-stu-id="c44ca-132">You can get these values by running the following Azure PowerShell cmdlet:</span></span>

        Get-AzureAccount

4. <span data-ttu-id="c44ca-133">Adja hozzá a .csproj fájlban a következő hivatkozást:</span><span class="sxs-lookup"><span data-stu-id="c44ca-133">Add the following reference in your .csproj file:</span></span>

        <Reference Include="System.Configuration" />

5. <span data-ttu-id="c44ca-134">Adja hozzá a következő **használatával** utasítást, hogy a projekt forrásfájl (Program.cs):</span><span class="sxs-lookup"><span data-stu-id="c44ca-134">Add the following **using** statements to the source file (Program.cs) in the project:</span></span>
   
        using System;
        using System.Collections.Generic;
        using System.Configuration;
        using System.Threading;
        using System.Threading.Tasks;
        
        using Microsoft.Azure.Management.StreamAnalytics;
        using Microsoft.Azure.Management.StreamAnalytics.Models;
        using Microsoft.Rest.Azure.Authentication;
        using Microsoft.Rest;
6. <span data-ttu-id="c44ca-135">Adja hozzá a segítő hitelesítési módszert:</span><span class="sxs-lookup"><span data-stu-id="c44ca-135">Add an authentication helper method:</span></span>

   ```
   private static async Task<ServiceClientCredentials> GetCredentials()
   {
       var activeDirectoryClientSettings = ActiveDirectoryClientSettings.UsePromptOnly(ConfigurationManager.AppSettings["ClientId"], new Uri("urn:ietf:wg:oauth:2.0:oob"));
       ServiceClientCredentials credentials = await UserTokenProvider.LoginWithPromptAsync(ConfigurationManager.AppSettings["ActiveDirectoryTenantId"], activeDirectoryClientSettings);
       
       return credentials;
    }
   ```

## <a name="create-a-stream-analytics-management-client"></a><span data-ttu-id="c44ca-136">Hozzon létre egy Stream Analytics management ügyfél</span><span class="sxs-lookup"><span data-stu-id="c44ca-136">Create a Stream Analytics management client</span></span>
<span data-ttu-id="c44ca-137">A **StreamAnalyticsManagementClient** objektum lehetővé teszi, hogy a feladat, és a feladat összetevők, például a bemeneti, kimeneti és átalakítása kezelheti.</span><span class="sxs-lookup"><span data-stu-id="c44ca-137">A **StreamAnalyticsManagementClient** object allows you to manage the job and the job components, such as input, output, and transformation.</span></span>

<span data-ttu-id="c44ca-138">Adja hozzá a következő kódot elejéhez a **fő** módszert:</span><span class="sxs-lookup"><span data-stu-id="c44ca-138">Add the following code to the beginning of the **Main** method:</span></span>

   ```
    string resourceGroupName = "<YOUR AZURE RESOURCE GROUP NAME>";
    string streamingJobName = "<YOUR STREAMING JOB NAME>";
    string inputName = "<YOUR JOB INPUT NAME>";
    string transformationName = "<YOUR JOB TRANSFORMATION NAME>";
    string outputName = "<YOUR JOB OUTPUT NAME>";
    
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
    
    // Get credentials
    ServiceClientCredentials credentials = GetCredentials().Result;
    
    // Create Stream Analytics management client
    StreamAnalyticsManagementClient streamAnalyticsManagementClient = new StreamAnalyticsManagementClient(credentials)
    {
        SubscriptionId = ConfigurationManager.AppSettings["SubscriptionId"]
    };
   ```

<span data-ttu-id="c44ca-139">A **resourceGroupName** változó értékét meg kell egyeznie az erőforráscsoport létrehozása, vagy az előfeltételként szükséges lépések kivételezett nevével.</span><span class="sxs-lookup"><span data-stu-id="c44ca-139">The **resourceGroupName** variable's value should be the same as the name of the resource group you created or picked in the prerequisite steps.</span></span>

<span data-ttu-id="c44ca-140">Feladat létrehozása a hitelesítő adatok megjelenítési aspektusa automatizálását, tekintse meg [hitelesítéséhez az Azure Resource Manager szolgáltatásnevet](../azure-resource-manager/resource-group-authenticate-service-principal.md).</span><span class="sxs-lookup"><span data-stu-id="c44ca-140">To automate the credential presentation aspect of job creation, refer to [Authenticating a service principal with Azure Resource Manager](../azure-resource-manager/resource-group-authenticate-service-principal.md).</span></span>

<span data-ttu-id="c44ca-141">Ez a cikk fennmaradó részében feltételezik, hogy ez a kód elején a **fő** metódust.</span><span class="sxs-lookup"><span data-stu-id="c44ca-141">The remaining sections of this article assume that this code is at the beginning of the **Main** method.</span></span>

## <a name="create-a-stream-analytics-job"></a><span data-ttu-id="c44ca-142">Stream Analytics-feladat létrehozása</span><span class="sxs-lookup"><span data-stu-id="c44ca-142">Create a Stream Analytics job</span></span>
<span data-ttu-id="c44ca-143">A következő kódot a Stream Analytics-feladat a megadott erőforráscsoport alapján hoz létre.</span><span class="sxs-lookup"><span data-stu-id="c44ca-143">The following code creates a Stream Analytics job under the resource group that you have defined.</span></span> <span data-ttu-id="c44ca-144">Hozzáadandó egy bemeneti, kimeneti és átalakítása a feladatot később.</span><span class="sxs-lookup"><span data-stu-id="c44ca-144">You will add an input, output, and transformation to the job later.</span></span>

   ```
   // Create a streaming job
   StreamingJob streamingJob = new StreamingJob()
   {
       Tags = new Dictionary<string, string>()
       {
           { "Origin", ".NET SDK" },
           { "ReasonCreated", "Getting started tutorial" }
       },
       Location = "West US",
       EventsOutOfOrderPolicy = EventsOutOfOrderPolicy.Drop,
       EventsOutOfOrderMaxDelayInSeconds = 5,
       EventsLateArrivalMaxDelayInSeconds = 16,
       OutputErrorPolicy = OutputErrorPolicy.Drop,
       DataLocale = "en-US",
       CompatibilityLevel = CompatibilityLevel.OneFullStopZero,
       Sku = new Sku()
       {
           Name = SkuName.Standard
       }
   };
   StreamingJob createStreamingJobResult = streamAnalyticsManagementClient.StreamingJobs.CreateOrReplace(streamingJob, resourceGroupName, streamingJobName);
   ```

## <a name="create-a-stream-analytics-input-source"></a><span data-ttu-id="c44ca-145">Hozzon létre egy Stream Analytics bemeneti forrása</span><span class="sxs-lookup"><span data-stu-id="c44ca-145">Create a Stream Analytics input source</span></span>
<span data-ttu-id="c44ca-146">A következő kódot a Stream Analytics bemeneti forrás a blob bemeneti forrása típusa és a fürt megosztott kötetei szolgáltatás szerializálási hoz létre.</span><span class="sxs-lookup"><span data-stu-id="c44ca-146">The following code creates a Stream Analytics input source with the blob input source type and CSV serialization.</span></span> <span data-ttu-id="c44ca-147">Az event hubs ezen bemeneti forrása hozhat létre **EventHubStreamInputDataSource** helyett **BlobStreamInputDataSource**.</span><span class="sxs-lookup"><span data-stu-id="c44ca-147">To create an event hub input source, use **EventHubStreamInputDataSource** instead of **BlobStreamInputDataSource**.</span></span> <span data-ttu-id="c44ca-148">Hasonlóképpen testre szabhatja a bemeneti forrás a szerializálási típushoz.</span><span class="sxs-lookup"><span data-stu-id="c44ca-148">Similarly, you can customize the serialization type of the input source.</span></span>

   ```
   // Create an input
   StorageAccount storageAccount = new StorageAccount()
   {
       AccountName = "<YOUR STORAGE ACCOUNT NAME>",
       AccountKey = "<YOUR STORAGE ACCOUNT KEY>"
   };
   Input input = new Input()
   {
       Properties = new StreamInputProperties()
       {
           Serialization = new CsvSerialization()
           {
               FieldDelimiter = ",",
               Encoding = Encoding.UTF8
           },
           Datasource = new BlobStreamInputDataSource()
           {
               StorageAccounts = new[] { storageAccount },
               Container = "<YOUR STORAGE BLOB CONTAINER>",
               PathPattern = "{date}/{time}",
               DateFormat = "yyyy/MM/dd",
               TimeFormat = "HH",
               SourcePartitionCount = 16
           }
       }
   };
   Input createInputResult = streamAnalyticsManagementClient.Inputs.CreateOrReplace(input, resourceGroupName, streamingJobName, inputName);
   ```

<span data-ttu-id="c44ca-149">Beviteli móddal, akár a Blob-tároló vagy egy eseményközpontba kötődnek, egy adott feladat.</span><span class="sxs-lookup"><span data-stu-id="c44ca-149">Input sources, whether from Blob storage or an event hub, are tied to a specific job.</span></span> <span data-ttu-id="c44ca-150">Használja az ugyanazon bemeneti forrás a különböző feladatok, hívja meg ismét a metódust, és adjon meg egy másik feladatnévvel.</span><span class="sxs-lookup"><span data-stu-id="c44ca-150">To use the same input source for different jobs, you must call the method again and specify a different job name.</span></span>

## <a name="test-a-stream-analytics-input-source"></a><span data-ttu-id="c44ca-151">A Stream Analytics bemeneti forrás tesztelése</span><span class="sxs-lookup"><span data-stu-id="c44ca-151">Test a Stream Analytics input source</span></span>
<span data-ttu-id="c44ca-152">A **TestConnection** metódus teszteli, hogy a Stream Analytics-feladat csatlakozni tudjanak a bemeneti forrás a bemeneti forrás típusa különleges, valamint egyéb szempontokat.</span><span class="sxs-lookup"><span data-stu-id="c44ca-152">The **TestConnection** method tests whether the Stream Analytics job is able to connect to the input source as well as other aspects specific to the input source type.</span></span> <span data-ttu-id="c44ca-153">Például a blobbemeneti forrás egy korábbi lépésben létrehozott, a a módszerrel ellenőrzi, hogy a Tárfiók nevét és a kulcspár használatával lehet csatlakozni a tárfiókhoz, valamint a ellenőrizze, hogy létezik-e a megadott tároló.</span><span class="sxs-lookup"><span data-stu-id="c44ca-153">For example, in the blob input source you created in an earlier step, the method will check that the Storage account name and key pair can be used to connect to the Storage account as well as check that the specified container exists.</span></span>

   ```
   // Test the connection to the input
   ResourceTestStatus testInputResult = streamAnalyticsManagementClient.Inputs.Test(resourceGroupName, streamingJobName, inputName);
   ```

## <a name="create-a-stream-analytics-output-target"></a><span data-ttu-id="c44ca-154">A Stream Analytics kimeneti cél létrehozásához</span><span class="sxs-lookup"><span data-stu-id="c44ca-154">Create a Stream Analytics output target</span></span>
<span data-ttu-id="c44ca-155">Egy kimeneti cél létrehozása nagyon hasonlít a Stream Analytics bemeneti forrás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="c44ca-155">Creating an output target is very similar to creating a Stream Analytics input source.</span></span> <span data-ttu-id="c44ca-156">Beviteli móddal, például a kimeneti célokat egy adott feladat vannak társítva.</span><span class="sxs-lookup"><span data-stu-id="c44ca-156">Like input sources, output targets are tied to a specific job.</span></span> <span data-ttu-id="c44ca-157">Kimeneti egyazon célobjektum használ a különböző feladatokhoz, hívja meg ismét a metódust, és adjon meg egy másik feladatnévvel.</span><span class="sxs-lookup"><span data-stu-id="c44ca-157">To use the same output target for different jobs, you must call the method again and specify a different job name.</span></span>

<span data-ttu-id="c44ca-158">Az alábbi kód létrehoz egy kimeneti célhoz (Azure SQL-adatbázis).</span><span class="sxs-lookup"><span data-stu-id="c44ca-158">The following code creates an output target (Azure SQL database).</span></span> <span data-ttu-id="c44ca-159">Testre szabhatja a kimeneti adatok céltípus és/vagy szerializálási típushoz.</span><span class="sxs-lookup"><span data-stu-id="c44ca-159">You can customize the output target's data type and/or serialization type.</span></span>

   ```
   // Create an output
   Output output = new Output()
   {
       Datasource = new AzureSqlDatabaseOutputDataSource()
       {
           Server = "<YOUR DATABASE SERVER NAME>",
           Database = "<YOUR DATABASE NAME>",
           User = "<YOUR DATABASE LOGIN>",
           Password = "<YOUR DATABASE LOGIN PASSWORD>",
           Table = "<YOUR DATABASE TABLE NAME>"
       }
   };
   Output createOutputResult = streamAnalyticsManagementClient.Outputs.CreateOrReplace(output, resourceGroupName, streamingJobName, outputName);
   ```

## <a name="test-a-stream-analytics-output-target"></a><span data-ttu-id="c44ca-160">A Stream Analytics kimeneti cél tesztelése</span><span class="sxs-lookup"><span data-stu-id="c44ca-160">Test a Stream Analytics output target</span></span>
<span data-ttu-id="c44ca-161">A Stream Analytics kimeneti cél is tartozik. a **TestConnection** kapcsolatok tesztelése a módszert.</span><span class="sxs-lookup"><span data-stu-id="c44ca-161">A Stream Analytics output target also has the **TestConnection** method for testing connections.</span></span>

   ```
   // Test the connection to the output
   ResourceTestStatus testOutputResult = streamAnalyticsManagementClient.Outputs.Test(resourceGroupName, streamingJobName, outputName);
   ```

## <a name="create-a-stream-analytics-transformation"></a><span data-ttu-id="c44ca-162">Hozzon létre egy Stream Analytics átalakítása</span><span class="sxs-lookup"><span data-stu-id="c44ca-162">Create a Stream Analytics transformation</span></span>
<span data-ttu-id="c44ca-163">A következő kódot a Stream Analytics átalakítás hoz létre a lekérdezés "Válasszon * bemeneti" és egy streamelési egységet a Stream Analytics-feladat lefoglalni határozza meg.</span><span class="sxs-lookup"><span data-stu-id="c44ca-163">The following code creates a Stream Analytics transformation with the query "select * from Input" and specifies to allocate one streaming unit for the Stream Analytics job.</span></span> <span data-ttu-id="c44ca-164">A streamelési egységek beállítása további információkért lásd: [Scale Azure Stream Analytics-feladatok](stream-analytics-scale-jobs.md).</span><span class="sxs-lookup"><span data-stu-id="c44ca-164">For more information on adjusting streaming units, see [Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md).</span></span>

   ```
   // Create a transformation
   Transformation transformation = new Transformation()
   {
       Query = "Select Id, Name from <your input name>", // '<your input name>' should be replaced with the value you put for the 'inputName' variable above or in a previous step
       StreamingUnits = 1
   };
   Transformation createTransformationResult = streamAnalyticsManagementClient.Transformations.CreateOrReplace(transformation, resourceGroupName, streamingJobName, transformationName);
   ```

<span data-ttu-id="c44ca-165">Például a bemeneti és kimeneti átalakítás is van kötve az adott Stream Analytics-feladat csoportban hozták létre.</span><span class="sxs-lookup"><span data-stu-id="c44ca-165">Like input and output, a transformation is also tied to the specific Stream Analytics job it was created under.</span></span>

## <a name="start-a-stream-analytics-job"></a><span data-ttu-id="c44ca-166">A Stream Analytics-feladat indítása</span><span class="sxs-lookup"><span data-stu-id="c44ca-166">Start a Stream Analytics job</span></span>
<span data-ttu-id="c44ca-167">Miután létrehozta a Stream Analytics-feladat, és a input(s), kimenete és átalakítása, megkezdheti a feladat meghívása a **Start** metódust.</span><span class="sxs-lookup"><span data-stu-id="c44ca-167">After creating a Stream Analytics job and its input(s), output(s), and transformation, you can start the job by calling the **Start** method.</span></span>

<span data-ttu-id="c44ca-168">A következő példa egy egyéni kimeneti kezdési idejű Stream Analytics-feladat beállítása a 2012. December 12., 12:12:12 kód indítása (UTC):</span><span class="sxs-lookup"><span data-stu-id="c44ca-168">The following sample code starts a Stream Analytics job with a custom output start time set to December 12, 2012, 12:12:12 UTC:</span></span>

   ```
   // Start a streaming job
   StartStreamingJobParameters startStreamingJobParameters = new StartStreamingJobParameters()
   {
       OutputStartMode = OutputStartMode.CustomTime,
       OutputStartTime = new DateTime(2012, 12, 12, 12, 12, 12, DateTimeKind.Utc)
   };
   streamAnalyticsManagementClient.StreamingJobs.Start(resourceGroupName, streamingJobName, startStreamingJobParameters);
   ```

## <a name="stop-a-stream-analytics-job"></a><span data-ttu-id="c44ca-169">A Stream Analytics-feladat leállítása</span><span class="sxs-lookup"><span data-stu-id="c44ca-169">Stop a Stream Analytics job</span></span>
<span data-ttu-id="c44ca-170">Egy futó Stream Analytics-feladat meghívásával leállíthatja a **leállítása** metódust.</span><span class="sxs-lookup"><span data-stu-id="c44ca-170">You can stop a running Stream Analytics job by calling the **Stop** method.</span></span>

   ```
   // Stop a streaming job
   streamAnalyticsManagementClient.StreamingJobs.Stop(resourceGroupName, streamingJobName);
   ```

## <a name="delete-a-stream-analytics-job"></a><span data-ttu-id="c44ca-171">A Stream Analytics-feladat törlése</span><span class="sxs-lookup"><span data-stu-id="c44ca-171">Delete a Stream Analytics job</span></span>
<span data-ttu-id="c44ca-172">A **törlése** metódus törli a feladatot, valamint az alapul szolgáló alárendelt erőforrások, például az input(s), kimenete és átalakítása a feladat.</span><span class="sxs-lookup"><span data-stu-id="c44ca-172">The **Delete** method will delete the job as well as the underlying sub-resources, including input(s), output(s), and transformation of the job.</span></span>

   ```
   // Delete a streaming job
   streamAnalyticsManagementClient.StreamingJobs.Delete(resourceGroupName, streamingJobName);
   ```

## <a name="get-support"></a><span data-ttu-id="c44ca-173">Támogatás kérése</span><span class="sxs-lookup"><span data-stu-id="c44ca-173">Get support</span></span>
<span data-ttu-id="c44ca-174">Ha további segítségre van szüksége, próbálkozzon a [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="c44ca-174">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>

## <a name="next-steps"></a><span data-ttu-id="c44ca-175">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c44ca-175">Next steps</span></span>
<span data-ttu-id="c44ca-176">Hogy megismerte a .NET SDK használatával hozhat létre és futtathat analytics-feladatok alapjait.</span><span class="sxs-lookup"><span data-stu-id="c44ca-176">You've learned the basics of using a .NET SDK to create and run analytics jobs.</span></span> <span data-ttu-id="c44ca-177">További információ:</span><span class="sxs-lookup"><span data-stu-id="c44ca-177">To learn more, see the following:</span></span>

* [<span data-ttu-id="c44ca-178">Az Azure Stream Analytics bemutatása</span><span class="sxs-lookup"><span data-stu-id="c44ca-178">Introduction to Azure Stream Analytics</span></span>](stream-analytics-introduction.md)
* <span data-ttu-id="c44ca-179">[Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md) (Bevezetés az Azure Stream Analytics használatába)</span><span class="sxs-lookup"><span data-stu-id="c44ca-179">[Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)</span></span>
* <span data-ttu-id="c44ca-180">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)</span><span class="sxs-lookup"><span data-stu-id="c44ca-180">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md)</span></span>
* <span data-ttu-id="c44ca-181">[Az Azure Stream Analytics felügyeleti .NET SDK](https://msdn.microsoft.com/library/azure/dn889315.aspx).</span><span class="sxs-lookup"><span data-stu-id="c44ca-181">[Azure Stream Analytics Management .NET SDK](https://msdn.microsoft.com/library/azure/dn889315.aspx).</span></span>
* <span data-ttu-id="c44ca-182">[Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referencia az Azure Stream Analytics lekérdezési nyelvhez)</span><span class="sxs-lookup"><span data-stu-id="c44ca-182">[Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)</span></span>
* [<span data-ttu-id="c44ca-183">Az Azure Stream Analytics felügyeleti REST API referenciája</span><span class="sxs-lookup"><span data-stu-id="c44ca-183">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Image references-->
[5]: ./media/markdown-template-for-new-articles/octocats.png
[6]: ./media/markdown-template-for-new-articles/pretty49.png
[7]: ./media/markdown-template-for-new-articles/channel-9.png


<!--Link references-->
[azure.blob.storage]: http://azure.microsoft.com/documentation/services/storage/
[azure.blob.storage.use]: http://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/

[azure.event.hubs]: http://azure.microsoft.com/services/event-hubs/
[azure.event.hubs.developer.guide]: http://msdn.microsoft.com/library/azure/dn789972.aspx

[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.forum]: http://go.microsoft.com/fwlink/?LinkId=512151

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.developer.guide]: stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
