---
title: az Azure Stream Analytics .NET SDK aaaManagement |} Microsoft Docs
description: "Első lépések a Stream Analytics felügyeleti .NET SDK-val. Megtudhatja, hogyan tooset össze, és az analytics-feladatok futtatása. Hozzon létre egy projekt, bemenetek, kimenetek és átalakításához."
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
ms.openlocfilehash: 507c11938bc5bf2249a2e41f6bcc076db8ead3f6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="management-net-sdk-set-up-and-run-analytics-jobs-using-hello-azure-stream-analytics-api-for-net"></a><span data-ttu-id="e4382-106">.NET SDK-kezelés: Beállítása és hello Azure Stream Analytics API használatával a .NET-hez analytics-feladatok futtatása</span><span class="sxs-lookup"><span data-stu-id="e4382-106">Management .NET SDK: Set up and run analytics jobs using hello Azure Stream Analytics API for .NET</span></span>
<span data-ttu-id="e4382-107">Ismerje meg, hogyan tooset fel és futtatási analytics-feladatok hello Stream Analytics API használatával a .NET használatával hello felügyeleti .NET SDK-val.</span><span class="sxs-lookup"><span data-stu-id="e4382-107">Learn how tooset up and run analytics jobs using hello Stream Analytics API for .NET using hello Management .NET SDK.</span></span> <span data-ttu-id="e4382-108">Projekt beállítása, hozzon létre a bemeneti és kimeneti adatforrások, átalakítások, és úgy indítsa és feladatok.</span><span class="sxs-lookup"><span data-stu-id="e4382-108">Set up a project, create input and output sources, transformations, and start and stop jobs.</span></span> <span data-ttu-id="e4382-109">Az analytics-feladatok adatok Blob-tároló vagy az eseményközpont folyamatos átviteléhez.</span><span class="sxs-lookup"><span data-stu-id="e4382-109">For your analytics jobs, you can stream data from Blob storage or from an event hub.</span></span>

<span data-ttu-id="e4382-110">Lásd: hello [felügyeleti referenciadokumentációt hello Stream Analytics API a .NET-keretrendszerhez készült](https://msdn.microsoft.com/library/azure/dn889315.aspx).</span><span class="sxs-lookup"><span data-stu-id="e4382-110">See hello [management reference documentation for hello Stream Analytics API for .NET](https://msdn.microsoft.com/library/azure/dn889315.aspx).</span></span>

<span data-ttu-id="e4382-111">Az Azure Stream Analytics egy teljes körűen felügyelt szolgáltatás biztosítása kis késleltetésű, magas rendelkezésre állású, méretezhető, összetett esemény feldolgozása keresztül adatfolyam hello felhőben.</span><span class="sxs-lookup"><span data-stu-id="e4382-111">Azure Stream Analytics is a fully managed service providing low-latency, highly available, scalable, complex event processing over streaming data in hello cloud.</span></span> <span data-ttu-id="e4382-112">A Stream Analytics lehetővé teszi, hogy az ügyfelek tooset folyamatos átviteli feladatok tooanalyze adatfolyamot fel, és közel valós idejű elemzési toodrive lehetővé teszi.</span><span class="sxs-lookup"><span data-stu-id="e4382-112">Stream Analytics enables customers tooset up streaming jobs tooanalyze data streams, and allows them toodrive near real-time analytics.</span></span>  

> [!NOTE]
> <span data-ttu-id="e4382-113">Ebben a cikkben hello mintakód Azure Stream Analytics Management .NET SDK v2.x verziójával lett frissítve.</span><span class="sxs-lookup"><span data-stu-id="e4382-113">We have updated hello sample code in this article with Azure Stream Analytics Management .NET SDK v2.x version.</span></span> <span data-ttu-id="e4382-114">A hello használó példakódot lagecy (1.x) SDK verzióját használja, ellenőrizze a [hello Management .NET SDK v1.x használja a Stream Analytics](https://docs.microsoft.com/en-us/azure/stream-analytics/stream-analytics-dotnet-management-sdk-v1).</span><span class="sxs-lookup"><span data-stu-id="e4382-114">For sample code using hello uses lagecy (1.x) SDK version, please see [Use hello Management .NET SDK v1.x for Stream Analytics](https://docs.microsoft.com/en-us/azure/stream-analytics/stream-analytics-dotnet-management-sdk-v1).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e4382-115">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="e4382-115">Prerequisites</span></span>
<span data-ttu-id="e4382-116">Ez a cikk elkezdéséhez hello következő kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="e4382-116">Before you begin this article, you must have hello following:</span></span>

* <span data-ttu-id="e4382-117">Telepítse a Visual Studio 2017 vagy 2015.</span><span class="sxs-lookup"><span data-stu-id="e4382-117">Install Visual Studio 2017 or 2015.</span></span>
* <span data-ttu-id="e4382-118">Töltse le és telepítse [Azure .NET SDK](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="e4382-118">Download and install [Azure .NET SDK](https://azure.microsoft.com/downloads/).</span></span>
* <span data-ttu-id="e4382-119">Azure-erőforráscsoport létrehozása az előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="e4382-119">Create an Azure Resource Group in your subscription.</span></span> <span data-ttu-id="e4382-120">hello az alábbiakban látható egy minta Azure PowerShell-parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="e4382-120">hello following is a sample Azure PowerShell script.</span></span> <span data-ttu-id="e4382-121">Azure PowerShell információkért lásd: [telepítse és konfigurálja az Azure Powershellt](/powershell/azure/overview);</span><span class="sxs-lookup"><span data-stu-id="e4382-121">For Azure PowerShell information, see [Install and configure Azure PowerShell](/powershell/azure/overview);</span></span>  

        # Log in tooyour Azure account
        Add-AzureAccount

        # Select hello Azure subscription you want toouse toocreate hello resource group
        Select-AzureSubscription -SubscriptionName <subscription name>

            # If Stream Analytics has not been registered toohello subscription, remove hello remark symbol (#) toorun hello Register-AzureRMProvider cmdlet tooregister hello provider namespace
            #Register-AzureRMProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'

        # Create an Azure resource group
        New-AzureResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>


* <span data-ttu-id="e4382-122">Állítson be egy bemeneti forrás és cél toouse kimeneti.</span><span class="sxs-lookup"><span data-stu-id="e4382-122">Set up an input source and output target toouse.</span></span> <span data-ttu-id="e4382-123">A további tudnivalókat [bemenet hozzáadása](stream-analytics-add-inputs.md) be egy minta bemeneti tooset és [hozzáadása kimenetek](stream-analytics-add-outputs.md) tooset be egy minta kimenet.</span><span class="sxs-lookup"><span data-stu-id="e4382-123">For further instructions see [Add Inputs](stream-analytics-add-inputs.md) tooset up a sample input and [Add Outputs](stream-analytics-add-outputs.md) tooset up a sample output.</span></span>

## <a name="set-up-a-project"></a><span data-ttu-id="e4382-124">Projekt beállítása</span><span class="sxs-lookup"><span data-stu-id="e4382-124">Set up a project</span></span>
<span data-ttu-id="e4382-125">toocreate analytics-feladat hello Stream Analytics API használata a .NET, először állítsa be a projekthez.</span><span class="sxs-lookup"><span data-stu-id="e4382-125">toocreate an analytics job use hello Stream Analytics API for .NET, first set up your project.</span></span>

1. <span data-ttu-id="e4382-126">Hozzon létre egy Visual Studio C# .NET konzolalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="e4382-126">Create a Visual Studio C# .NET console application.</span></span>
2. <span data-ttu-id="e4382-127">Hello Csomagkezelő konzol, a következő futtatási hello parancsokat tooinstall hello NuGet-csomagok.</span><span class="sxs-lookup"><span data-stu-id="e4382-127">In hello Package Manager Console, run hello following commands tooinstall hello NuGet packages.</span></span> <span data-ttu-id="e4382-128">hello először egyik hello Azure Stream Analytics felügyeleti .NET SDK-t.</span><span class="sxs-lookup"><span data-stu-id="e4382-128">hello first one is hello Azure Stream Analytics Management .NET SDK.</span></span> <span data-ttu-id="e4382-129">hello második van az Azure ügyfél-hitelesítéshez.</span><span class="sxs-lookup"><span data-stu-id="e4382-129">hello second one is for Azure client authentication.</span></span>
   
        Install-Package Microsoft.Azure.Management.StreamAnalytics -Version 2.0.0
        Install-Package Microsoft.Rest.ClientRuntime.Azure.Authentication -Version 2.3.1
3. <span data-ttu-id="e4382-130">Adja hozzá a következő hello **appSettings** szakasz toohello App.config fájlban:</span><span class="sxs-lookup"><span data-stu-id="e4382-130">Add hello following **appSettings** section toohello App.config file:</span></span>
   
        <appSettings>
          <add key="ClientId" value="1950a258-227b-4e31-a9cf-717495945fc2" />
          <add key="RedirectUri" value="urn:ietf:wg:oauth:2.0:oob" />
          <add key="SubscriptionId" value="YOUR SUBSCRIPTION ID" />
          <add key="ActiveDirectoryTenantId" value="YOUR TENANT ID" />
        </appSettings>

    <span data-ttu-id="e4382-131">Cserélje le az értékeket **SubscriptionId** és **ActiveDirectoryTenantId** a Azure-előfizetés és a bérlői azonosítók.</span><span class="sxs-lookup"><span data-stu-id="e4382-131">Replace values for **SubscriptionId** and **ActiveDirectoryTenantId** with your Azure subscription and tenant IDs.</span></span> <span data-ttu-id="e4382-132">Ezek az értékek kaphat hello Azure PowerShell-parancsmag a következő futtatásával:</span><span class="sxs-lookup"><span data-stu-id="e4382-132">You can get these values by running hello following Azure PowerShell cmdlet:</span></span>

        Get-AzureAccount

4. <span data-ttu-id="e4382-133">Adja hozzá a következő hivatkozás a .csproj fájlban hello:</span><span class="sxs-lookup"><span data-stu-id="e4382-133">Add hello following reference in your .csproj file:</span></span>

        <Reference Include="System.Configuration" />

5. <span data-ttu-id="e4382-134">Adja hozzá a következő hello **használatával** utasítások toohello forrásfájl (Program.cs) hello projektben:</span><span class="sxs-lookup"><span data-stu-id="e4382-134">Add hello following **using** statements toohello source file (Program.cs) in hello project:</span></span>
   
        using System;
        using System.Collections.Generic;
        using System.Configuration;
        using System.Threading;
        using System.Threading.Tasks;
        
        using Microsoft.Azure.Management.StreamAnalytics;
        using Microsoft.Azure.Management.StreamAnalytics.Models;
        using Microsoft.Rest.Azure.Authentication;
        using Microsoft.Rest;
6. <span data-ttu-id="e4382-135">Adja hozzá a segítő hitelesítési módszert:</span><span class="sxs-lookup"><span data-stu-id="e4382-135">Add an authentication helper method:</span></span>

   ```
   private static async Task<ServiceClientCredentials> GetCredentials()
   {
       var activeDirectoryClientSettings = ActiveDirectoryClientSettings.UsePromptOnly(ConfigurationManager.AppSettings["ClientId"], new Uri("urn:ietf:wg:oauth:2.0:oob"));
       ServiceClientCredentials credentials = await UserTokenProvider.LoginWithPromptAsync(ConfigurationManager.AppSettings["ActiveDirectoryTenantId"], activeDirectoryClientSettings);
       
       return credentials;
    }
   ```

## <a name="create-a-stream-analytics-management-client"></a><span data-ttu-id="e4382-136">Hozzon létre egy Stream Analytics management ügyfél</span><span class="sxs-lookup"><span data-stu-id="e4382-136">Create a Stream Analytics management client</span></span>
<span data-ttu-id="e4382-137">A **StreamAnalyticsManagementClient** objektum lehetővé teszi a toomanage hello feladat és hello feladat összetevők, például bemeneti, kimeneti és átalakítás.</span><span class="sxs-lookup"><span data-stu-id="e4382-137">A **StreamAnalyticsManagementClient** object allows you toomanage hello job and hello job components, such as input, output, and transformation.</span></span>

<span data-ttu-id="e4382-138">Adja hozzá a következő kód toohello elejére hello hello **fő** módszert:</span><span class="sxs-lookup"><span data-stu-id="e4382-138">Add hello following code toohello beginning of hello **Main** method:</span></span>

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

<span data-ttu-id="e4382-139">Hello **resourceGroupName** változó értékét a hello erőforrás hello neve azonos gazdagépcsoport létrehozott vagy hello előfeltételként szükséges lépések kivételezett hello kell lennie.</span><span class="sxs-lookup"><span data-stu-id="e4382-139">hello **resourceGroupName** variable's value should be hello same as hello name of hello resource group you created or picked in hello prerequisite steps.</span></span>

<span data-ttu-id="e4382-140">tooautomate hello credential bemutató szempontja, hogy a feladat létrehozása, tekintse meg a túl[hitelesítéséhez az Azure Resource Manager szolgáltatásnevet](../azure-resource-manager/resource-group-authenticate-service-principal.md).</span><span class="sxs-lookup"><span data-stu-id="e4382-140">tooautomate hello credential presentation aspect of job creation, refer too[Authenticating a service principal with Azure Resource Manager](../azure-resource-manager/resource-group-authenticate-service-principal.md).</span></span>

<span data-ttu-id="e4382-141">hello Ez a cikk fennmaradó részében feltételezik, hogy ez a kód hello hello elején **fő** metódust.</span><span class="sxs-lookup"><span data-stu-id="e4382-141">hello remaining sections of this article assume that this code is at hello beginning of hello **Main** method.</span></span>

## <a name="create-a-stream-analytics-job"></a><span data-ttu-id="e4382-142">Stream Analytics-feladat létrehozása</span><span class="sxs-lookup"><span data-stu-id="e4382-142">Create a Stream Analytics job</span></span>
<span data-ttu-id="e4382-143">hello alábbi kód létrehoz egy Stream Analytics-feladat a megadott erőforráscsoport hello.</span><span class="sxs-lookup"><span data-stu-id="e4382-143">hello following code creates a Stream Analytics job under hello resource group that you have defined.</span></span> <span data-ttu-id="e4382-144">Egy bemeneti, kimeneti és átalakítása toohello feladat később fogja hozzáadni.</span><span class="sxs-lookup"><span data-stu-id="e4382-144">You will add an input, output, and transformation toohello job later.</span></span>

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

## <a name="create-a-stream-analytics-input-source"></a><span data-ttu-id="e4382-145">Hozzon létre egy Stream Analytics bemeneti forrása</span><span class="sxs-lookup"><span data-stu-id="e4382-145">Create a Stream Analytics input source</span></span>
<span data-ttu-id="e4382-146">hello alábbira hoz létre a Stream Analytics bemeneti forrás hello blob bemeneti forrása típusa és a fürt megosztott kötetei szolgáltatás szerializálást.</span><span class="sxs-lookup"><span data-stu-id="e4382-146">hello following code creates a Stream Analytics input source with hello blob input source type and CSV serialization.</span></span> <span data-ttu-id="e4382-147">az event hubs ezen bemeneti forrása, toocreate használja **EventHubStreamInputDataSource** helyett **BlobStreamInputDataSource**.</span><span class="sxs-lookup"><span data-stu-id="e4382-147">toocreate an event hub input source, use **EventHubStreamInputDataSource** instead of **BlobStreamInputDataSource**.</span></span> <span data-ttu-id="e4382-148">Hasonlóképpen testre szabhatja a hello szerializálási típushoz hello bemeneti forrás.</span><span class="sxs-lookup"><span data-stu-id="e4382-148">Similarly, you can customize hello serialization type of hello input source.</span></span>

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

<span data-ttu-id="e4382-149">Beviteli móddal, akár a Blob-tároló vagy egy eseményközpontot, kapcsolt tooa adott feladat.</span><span class="sxs-lookup"><span data-stu-id="e4382-149">Input sources, whether from Blob storage or an event hub, are tied tooa specific job.</span></span> <span data-ttu-id="e4382-150">toouse hello ugyanazon bemeneti forrás a különböző feladatokhoz, kell hello metódust hívja meg ismét, és adjon meg egy másik feladatnévvel.</span><span class="sxs-lookup"><span data-stu-id="e4382-150">toouse hello same input source for different jobs, you must call hello method again and specify a different job name.</span></span>

## <a name="test-a-stream-analytics-input-source"></a><span data-ttu-id="e4382-151">A Stream Analytics bemeneti forrás tesztelése</span><span class="sxs-lookup"><span data-stu-id="e4382-151">Test a Stream Analytics input source</span></span>
<span data-ttu-id="e4382-152">Hello **TestConnection** e hello Stream Analytics-feladat-e képes tooconnect toohello bemeneti forrás, valamint egyéb szempontok adott toohello metódus tesztek bemeneti forrás típusa.</span><span class="sxs-lookup"><span data-stu-id="e4382-152">hello **TestConnection** method tests whether hello Stream Analytics job is able tooconnect toohello input source as well as other aspects specific toohello input source type.</span></span> <span data-ttu-id="e4382-153">Például hello blob bemeneti forrása egy korábbi lépésben létrehozott hello metódus ellenőrzi, hogy hello tárfióknév kulcspár is használt tooconnect toohello tárfiókot kell, valamint ellenőrizze, hogy hello megadott tároló létezik-e.</span><span class="sxs-lookup"><span data-stu-id="e4382-153">For example, in hello blob input source you created in an earlier step, hello method will check that hello Storage account name and key pair can be used tooconnect toohello Storage account as well as check that hello specified container exists.</span></span>

   ```
   // Test hello connection toohello input
   ResourceTestStatus testInputResult = streamAnalyticsManagementClient.Inputs.Test(resourceGroupName, streamingJobName, inputName);
   ```

## <a name="create-a-stream-analytics-output-target"></a><span data-ttu-id="e4382-154">A Stream Analytics kimeneti cél létrehozásához</span><span class="sxs-lookup"><span data-stu-id="e4382-154">Create a Stream Analytics output target</span></span>
<span data-ttu-id="e4382-155">Nagyon hasonló toocreating a Stream Analytics bemeneti forrás egy kimeneti cél létrehozása történik.</span><span class="sxs-lookup"><span data-stu-id="e4382-155">Creating an output target is very similar toocreating a Stream Analytics input source.</span></span> <span data-ttu-id="e4382-156">Beviteli móddal, például a kimeneti célpontjai kapcsolt tooa adott feladat.</span><span class="sxs-lookup"><span data-stu-id="e4382-156">Like input sources, output targets are tied tooa specific job.</span></span> <span data-ttu-id="e4382-157">toouse hello azonos kimeneti célja a különböző feladatokhoz, kell hello metódust hívja meg ismét, és adjon meg egy másik feladatnévvel.</span><span class="sxs-lookup"><span data-stu-id="e4382-157">toouse hello same output target for different jobs, you must call hello method again and specify a different job name.</span></span>

<span data-ttu-id="e4382-158">a következő kód hello hoz létre egy kimeneti célhoz (Azure SQL-adatbázis).</span><span class="sxs-lookup"><span data-stu-id="e4382-158">hello following code creates an output target (Azure SQL database).</span></span> <span data-ttu-id="e4382-159">Testre szabhatja hello kimeneti cél adatok típusát és/vagy szerializálási típushoz.</span><span class="sxs-lookup"><span data-stu-id="e4382-159">You can customize hello output target's data type and/or serialization type.</span></span>

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

## <a name="test-a-stream-analytics-output-target"></a><span data-ttu-id="e4382-160">A Stream Analytics kimeneti cél tesztelése</span><span class="sxs-lookup"><span data-stu-id="e4382-160">Test a Stream Analytics output target</span></span>
<span data-ttu-id="e4382-161">A Stream Analytics kimeneti cél is tartozik hello **TestConnection** kapcsolatok tesztelése a módszert.</span><span class="sxs-lookup"><span data-stu-id="e4382-161">A Stream Analytics output target also has hello **TestConnection** method for testing connections.</span></span>

   ```
   // Test hello connection toohello output
   ResourceTestStatus testOutputResult = streamAnalyticsManagementClient.Outputs.Test(resourceGroupName, streamingJobName, outputName);
   ```

## <a name="create-a-stream-analytics-transformation"></a><span data-ttu-id="e4382-162">Hozzon létre egy Stream Analytics átalakítása</span><span class="sxs-lookup"><span data-stu-id="e4382-162">Create a Stream Analytics transformation</span></span>
<span data-ttu-id="e4382-163">hello alábbira hoz létre a Stream Analytics átalakítás hello lekérdezés "Válasszon * bemeneti" és egy streamelési egységet tooallocate hello Stream Analytics-feladat határozza meg.</span><span class="sxs-lookup"><span data-stu-id="e4382-163">hello following code creates a Stream Analytics transformation with hello query "select * from Input" and specifies tooallocate one streaming unit for hello Stream Analytics job.</span></span> <span data-ttu-id="e4382-164">A streamelési egységek beállítása további információkért lásd: [Scale Azure Stream Analytics-feladatok](stream-analytics-scale-jobs.md).</span><span class="sxs-lookup"><span data-stu-id="e4382-164">For more information on adjusting streaming units, see [Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md).</span></span>

   ```
   // Create a transformation
   Transformation transformation = new Transformation()
   {
       Query = "Select Id, Name from <your input name>", // '<your input name>' should be replaced with hello value you put for hello 'inputName' variable above or in a previous step
       StreamingUnits = 1
   };
   Transformation createTransformationResult = streamAnalyticsManagementClient.Transformations.CreateOrReplace(transformation, resourceGroupName, streamingJobName, transformationName);
   ```

<span data-ttu-id="e4382-165">Bemeneti és kimeneti, például átalakítás egyben a feltételekhez toohello adott Stream Analytics-feladat csoportban hozták létre.</span><span class="sxs-lookup"><span data-stu-id="e4382-165">Like input and output, a transformation is also tied toohello specific Stream Analytics job it was created under.</span></span>

## <a name="start-a-stream-analytics-job"></a><span data-ttu-id="e4382-166">A Stream Analytics-feladat indítása</span><span class="sxs-lookup"><span data-stu-id="e4382-166">Start a Stream Analytics job</span></span>
<span data-ttu-id="e4382-167">Miután létrehozta a Stream Analytics-feladat, és a input(s), kimenete és átalakítása, elindíthatja hello feladat hívó hello **Start** metódust.</span><span class="sxs-lookup"><span data-stu-id="e4382-167">After creating a Stream Analytics job and its input(s), output(s), and transformation, you can start hello job by calling hello **Start** method.</span></span>

<span data-ttu-id="e4382-168">a következő példakód hello Stream Analytics-feladat kezdődik-e egy egyéni kimeneti kezdési idő beállítása tooDecember 12, 2012, 12:12:12 UTC:</span><span class="sxs-lookup"><span data-stu-id="e4382-168">hello following sample code starts a Stream Analytics job with a custom output start time set tooDecember 12, 2012, 12:12:12 UTC:</span></span>

   ```
   // Start a streaming job
   StartStreamingJobParameters startStreamingJobParameters = new StartStreamingJobParameters()
   {
       OutputStartMode = OutputStartMode.CustomTime,
       OutputStartTime = new DateTime(2012, 12, 12, 12, 12, 12, DateTimeKind.Utc)
   };
   streamAnalyticsManagementClient.StreamingJobs.Start(resourceGroupName, streamingJobName, startStreamingJobParameters);
   ```

## <a name="stop-a-stream-analytics-job"></a><span data-ttu-id="e4382-169">A Stream Analytics-feladat leállítása</span><span class="sxs-lookup"><span data-stu-id="e4382-169">Stop a Stream Analytics job</span></span>
<span data-ttu-id="e4382-170">Egy futó Stream Analytics-feladat leállításához hívó hello **leállítása** metódust.</span><span class="sxs-lookup"><span data-stu-id="e4382-170">You can stop a running Stream Analytics job by calling hello **Stop** method.</span></span>

   ```
   // Stop a streaming job
   streamAnalyticsManagementClient.StreamingJobs.Stop(resourceGroupName, streamingJobName);
   ```

## <a name="delete-a-stream-analytics-job"></a><span data-ttu-id="e4382-171">A Stream Analytics-feladat törlése</span><span class="sxs-lookup"><span data-stu-id="e4382-171">Delete a Stream Analytics job</span></span>
<span data-ttu-id="e4382-172">Hello **törlése** metódus hello feladat, valamint az alapul szolgáló alárendelt erőforrások, például az input(s), kimenete és átalakítása hello feladat hello törli.</span><span class="sxs-lookup"><span data-stu-id="e4382-172">hello **Delete** method will delete hello job as well as hello underlying sub-resources, including input(s), output(s), and transformation of hello job.</span></span>

   ```
   // Delete a streaming job
   streamAnalyticsManagementClient.StreamingJobs.Delete(resourceGroupName, streamingJobName);
   ```

## <a name="get-support"></a><span data-ttu-id="e4382-173">Támogatás kérése</span><span class="sxs-lookup"><span data-stu-id="e4382-173">Get support</span></span>
<span data-ttu-id="e4382-174">Ha további segítségre van szüksége, próbálkozzon a [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="e4382-174">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>

## <a name="next-steps"></a><span data-ttu-id="e4382-175">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e4382-175">Next steps</span></span>
<span data-ttu-id="e4382-176">Hogy megismerte a .NET SDK toocreate használatával hello alapjait és analytics-feladatok futtatásához.</span><span class="sxs-lookup"><span data-stu-id="e4382-176">You've learned hello basics of using a .NET SDK toocreate and run analytics jobs.</span></span> <span data-ttu-id="e4382-177">toolearn több, tekintse meg a hello következőt:</span><span class="sxs-lookup"><span data-stu-id="e4382-177">toolearn more, see hello following:</span></span>

* [<span data-ttu-id="e4382-178">A Stream Analytics bemutatása tooAzure</span><span class="sxs-lookup"><span data-stu-id="e4382-178">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* <span data-ttu-id="e4382-179">[Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md) (Bevezetés az Azure Stream Analytics használatába)</span><span class="sxs-lookup"><span data-stu-id="e4382-179">[Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)</span></span>
* <span data-ttu-id="e4382-180">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)</span><span class="sxs-lookup"><span data-stu-id="e4382-180">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md)</span></span>
* <span data-ttu-id="e4382-181">[Az Azure Stream Analytics felügyeleti .NET SDK](https://msdn.microsoft.com/library/azure/dn889315.aspx).</span><span class="sxs-lookup"><span data-stu-id="e4382-181">[Azure Stream Analytics Management .NET SDK](https://msdn.microsoft.com/library/azure/dn889315.aspx).</span></span>
* <span data-ttu-id="e4382-182">[Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referencia az Azure Stream Analytics lekérdezési nyelvhez)</span><span class="sxs-lookup"><span data-stu-id="e4382-182">[Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)</span></span>
* [<span data-ttu-id="e4382-183">Az Azure Stream Analytics felügyeleti REST API referenciája</span><span class="sxs-lookup"><span data-stu-id="e4382-183">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

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
