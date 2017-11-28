---
title: "a Stream Analytics aaaProgrammatically figyelő feladatok |} Microsoft Docs"
description: "Ismerje meg, hogyan figyelheti az tooprogrammatically a REST API-k, az Azure SDK-t vagy a PowerShell segítségével létrehozott Stream Analytics-feladatok."
keywords: ".NET-figyelő, feladat figyelője alkalmazás figyelése"
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 2ec02cc9-4ca5-4a25-ae60-c44be9ad4835
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/20/2017
ms.author: jeffstok
ms.openlocfilehash: 44a9c29c2161ee81ea76ece4646a8691bf5d5b48
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="programmatically-create-a-stream-analytics-job-monitor"></a><span data-ttu-id="748cf-104">A Stream Analytics-feladat figyelő létrehozása programmal.</span><span class="sxs-lookup"><span data-stu-id="748cf-104">Programmatically create a Stream Analytics job monitor</span></span>

<span data-ttu-id="748cf-105">Ez a cikk bemutatja, hogyan tooenable figyelési Stream Analytics-feladat.</span><span class="sxs-lookup"><span data-stu-id="748cf-105">This article demonstrates how tooenable monitoring for a Stream Analytics job.</span></span> <span data-ttu-id="748cf-106">REST API-k, az Azure SDK-t vagy a PowerShell létrehozott Stream Analytics-feladatok nincs figyelés alapértelmezés szerint engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="748cf-106">Stream Analytics jobs that are created via REST APIs, Azure SDK, or PowerShell do not have monitoring enabled by default.</span></span> <span data-ttu-id="748cf-107">Manuálisan engedélyezheti azt az Azure-portálon hello toohello feladat figyelő lap megnyitásával és gombra kattintva hello engedélyezése vagy hello cikkben leírt lépéseket követve automatizálhatja is a folyamatot.</span><span class="sxs-lookup"><span data-stu-id="748cf-107">You can manually enable it in hello Azure portal by going toohello job’s Monitor page and clicking hello Enable button or you can automate this process by following hello steps in this article.</span></span> <span data-ttu-id="748cf-108">figyelési adatok hello hello Azure-portál a Stream Analytics-feladat a hello metrikák területén fog megjelenni.</span><span class="sxs-lookup"><span data-stu-id="748cf-108">hello monitoring data will show up in hello Metrics area of hello Azure portal for your Stream Analytics job.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="748cf-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="748cf-109">Prerequisites</span></span>

<span data-ttu-id="748cf-110">Ez a folyamat megkezdése előtt hello következő kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="748cf-110">Before you begin this process, you must have hello following:</span></span>

* <span data-ttu-id="748cf-111">A Visual Studio 2017 vagy 2015</span><span class="sxs-lookup"><span data-stu-id="748cf-111">Visual Studio 2017 or 2015</span></span>
* <span data-ttu-id="748cf-112">[Az Azure .NET SDK](https://azure.microsoft.com/downloads/) letöltése és telepítése</span><span class="sxs-lookup"><span data-stu-id="748cf-112">[Azure .NET SDK](https://azure.microsoft.com/downloads/) downloaded and installed</span></span>
* <span data-ttu-id="748cf-113">Egy meglévő Stream Analytics-feladat, amelyet a toohave figyelő engedélyezve</span><span class="sxs-lookup"><span data-stu-id="748cf-113">An existing Stream Analytics job that needs toohave monitoring enabled</span></span>

## <a name="create-a-project"></a><span data-ttu-id="748cf-114">A projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="748cf-114">Create a project</span></span>

1. <span data-ttu-id="748cf-115">Hozzon létre egy Visual Studio C# .NET konzolalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="748cf-115">Create a Visual Studio C# .NET console application.</span></span>
2. <span data-ttu-id="748cf-116">Hello Csomagkezelő konzol, a következő futtatási hello parancsokat tooinstall hello NuGet-csomagok.</span><span class="sxs-lookup"><span data-stu-id="748cf-116">In hello Package Manager Console, run hello following commands tooinstall hello NuGet packages.</span></span> <span data-ttu-id="748cf-117">hello először egyik hello Azure Stream Analytics felügyeleti .NET SDK-t.</span><span class="sxs-lookup"><span data-stu-id="748cf-117">hello first one is hello Azure Stream Analytics Management .NET SDK.</span></span> <span data-ttu-id="748cf-118">hello második hello Azure figyelő SDK használandó tooenable figyelését.</span><span class="sxs-lookup"><span data-stu-id="748cf-118">hello second one is hello Azure Monitor SDK that will be used tooenable monitoring.</span></span> <span data-ttu-id="748cf-119">hello utolsó egyik hello Azure Active Directory-ügyfél-hitelesítéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="748cf-119">hello last one is hello Azure Active Directory client that will be used for authentication.</span></span>
   
   ```
   Install-Package Microsoft.Azure.Management.StreamAnalytics
   Install-Package Microsoft.Azure.Insights -Pre
   Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
   ```
3. <span data-ttu-id="748cf-120">Adja hozzá a következő appSettings szakaszt toohello App.config fájl hello.</span><span class="sxs-lookup"><span data-stu-id="748cf-120">Add hello following appSettings section toohello App.config file.</span></span>
   
   ```
   <appSettings>
     <!--CSM Prod related values-->
     <add key="ResourceGroupName" value="RESOURCE GROUP NAME" />
     <add key="JobName" value="YOUR JOB NAME" />
     <add key="StorageAccountName" value="YOUR STORAGE ACCOUNT"/>
     <add key="ActiveDirectoryEndpoint" value="https://login.microsoftonline.com/" />
     <add key="ResourceManagerEndpoint" value="https://management.azure.com/" />
     <add key="WindowsManagementUri" value="https://management.core.windows.net/" />
     <add key="AsaClientId" value="1950a258-227b-4e31-a9cf-717495945fc2" />
     <add key="RedirectUri" value="urn:ietf:wg:oauth:2.0:oob" />
     <add key="SubscriptionId" value="YOUR AZURE SUBSCRIPTION ID" />
     <add key="ActiveDirectoryTenantId" value="YOUR TENANT ID" />
   </appSettings>
   ```
   <span data-ttu-id="748cf-121">Cserélje le az értékeket *SubscriptionId* és *ActiveDirectoryTenantId* a Azure-előfizetés és a bérlői azonosítók.</span><span class="sxs-lookup"><span data-stu-id="748cf-121">Replace values for *SubscriptionId* and *ActiveDirectoryTenantId* with your Azure subscription and tenant IDs.</span></span> <span data-ttu-id="748cf-122">Hello a következő PowerShell-parancsmag futtatásával kaphatunk ezekkel az értékekkel:</span><span class="sxs-lookup"><span data-stu-id="748cf-122">You can get these values by running hello following PowerShell cmdlet:</span></span>
   
   ```
   Get-AzureAccount
   ```
4. <span data-ttu-id="748cf-123">Adja hozzá a következő hello segítségével utasítások toohello forrásfájl (Program.cs) hello projektben.</span><span class="sxs-lookup"><span data-stu-id="748cf-123">Add hello following using statements toohello source file (Program.cs) in hello project.</span></span>
   
   ```
     using System;
     using System.Configuration;
     using System.Threading;
     using Microsoft.Azure;
     using Microsoft.Azure.Management.Insights;
     using Microsoft.Azure.Management.Insights.Models;
     using Microsoft.Azure.Management.StreamAnalytics;
     using Microsoft.Azure.Management.StreamAnalytics.Models;
     using Microsoft.IdentityModel.Clients.ActiveDirectory;
   ```
5. <span data-ttu-id="748cf-124">Adja hozzá a segítő hitelesítési módszert.</span><span class="sxs-lookup"><span data-stu-id="748cf-124">Add an authentication helper method.</span></span>
   
     <span data-ttu-id="748cf-125">nyilvános, statikus karakterlánc GetAuthorizationHeader()</span><span class="sxs-lookup"><span data-stu-id="748cf-125">public static string GetAuthorizationHeader()</span></span>
   
         {
             AuthenticationResult result = null;
             var thread = new Thread(() =>
             {
                 try
                 {
                     var context = new AuthenticationContext(
                         ConfigurationManager.AppSettings["ActiveDirectoryEndpoint"] +
                         ConfigurationManager.AppSettings["ActiveDirectoryTenantId"]);
   
                     result = context.AcquireToken(
                         resource: ConfigurationManager.AppSettings["WindowsManagementUri"],
                         clientId: ConfigurationManager.AppSettings["AsaClientId"],
                         redirectUri: new Uri(ConfigurationManager.AppSettings["RedirectUri"]),
                         promptBehavior: PromptBehavior.Always);
                 }
                 catch (Exception threadEx)
                 {
                     Console.WriteLine(threadEx.Message);
                 }
             });
   
             thread.SetApartmentState(ApartmentState.STA);
             thread.Name = "AcquireTokenThread";
             thread.Start();
             thread.Join();
   
             if (result != null)
             {
                 return result.AccessToken;
             }
   
             throw new InvalidOperationException("Failed tooacquire token");
     <span data-ttu-id="748cf-126">}</span><span class="sxs-lookup"><span data-stu-id="748cf-126">}</span></span>

## <a name="create-management-clients"></a><span data-ttu-id="748cf-127">Hozzon létre felügyeleti ügyfelek</span><span class="sxs-lookup"><span data-stu-id="748cf-127">Create management clients</span></span>

<span data-ttu-id="748cf-128">hello alábbira beállításához hello szükséges változók és a felügyeleti ügyfeleket.</span><span class="sxs-lookup"><span data-stu-id="748cf-128">hello following code will set up hello necessary variables and management clients.</span></span>

    string resourceGroupName = "<YOUR AZURE RESOURCE GROUP NAME>";
    string streamAnalyticsJobName = "<YOUR STREAM ANALYTICS JOB NAME>";

    // Get authentication token
    TokenCloudCredentials aadTokenCredentials =
        new TokenCloudCredentials(
            ConfigurationManager.AppSettings["SubscriptionId"],
            GetAuthorizationHeader());

    Uri resourceManagerUri = new
    Uri(ConfigurationManager.AppSettings["ResourceManagerEndpoint"]);

    // Create Stream Analytics and Insights management client
    StreamAnalyticsManagementClient streamAnalyticsClient = new
    StreamAnalyticsManagementClient(aadTokenCredentials, resourceManagerUri);
    InsightsManagementClient insightsClient = new
    InsightsManagementClient(aadTokenCredentials, resourceManagerUri);

## <a name="enable-monitoring-for-an-existing-stream-analytics-job"></a><span data-ttu-id="748cf-129">Egy meglévő Stream Analytics-feladat figyelésének engedélyezése</span><span class="sxs-lookup"><span data-stu-id="748cf-129">Enable monitoring for an existing Stream Analytics job</span></span>

<span data-ttu-id="748cf-130">hello következő kódot a figyelő lehetővé teszi, hogy egy **meglévő** Stream Analytics-feladat.</span><span class="sxs-lookup"><span data-stu-id="748cf-130">hello following code enables monitoring for an **existing** Stream Analytics job.</span></span> <span data-ttu-id="748cf-131">hello hello kód első része egy GET kérelmet hello Stream Analytics szolgáltatás tooretrieve információ hello adott Stream Analytics-feladatot hajt végre.</span><span class="sxs-lookup"><span data-stu-id="748cf-131">hello first part of hello code performs a GET request against hello Stream Analytics service tooretrieve information about hello particular Stream Analytics job.</span></span> <span data-ttu-id="748cf-132">Hello használ *azonosító* hello Put metódust a hello paramétereként (beolvasva a hello GET kérés) tulajdonság második felében hello kód, amely elküld egy PUT-kérelmet toohello Insights szolgáltatás tooenable a Stream Analytics hello megfigyelése a feladat.</span><span class="sxs-lookup"><span data-stu-id="748cf-132">It uses hello *Id* property (retrieved from hello GET request) as a parameter for hello Put method in hello second half of hello code, which sends a PUT request toohello Insights service tooenable monitoring for hello Stream Analytics job.</span></span>

>[!WARNING]
><span data-ttu-id="748cf-133">Ha már engedélyezte különböző Stream Analytics-feladat, hello Azure-portálon vagy programozottan keresztül az alábbi kód, hello figyelési **azt javasoljuk, hogy megadja a hello használt ugyanazon tárfiók neve, korábban már engedélyezte a figyelést.**</span><span class="sxs-lookup"><span data-stu-id="748cf-133">If you have previously enabled monitoring for a different Stream Analytics job, either through hello Azure portal or programmatically via hello below code, **we recommend that you provide hello same storage account name that you used when you previously enabled monitoring.**</span></span>
> 
> <span data-ttu-id="748cf-134">hello tárfiók csatolt toohello régióban létrehozott a Stream Analytics-feladat, nem kifejezetten maga toohello feladat kerül.</span><span class="sxs-lookup"><span data-stu-id="748cf-134">hello storage account is linked toohello region that you created your Stream Analytics job in, not specifically toohello job itself.</span></span>
> 
> <span data-ttu-id="748cf-135">Az összes Stream Analytics feladatok (és minden más Azure-erőforrások) ugyanabban a régióban ugyanazt a tárolási fiók toostore figyelési adatok.</span><span class="sxs-lookup"><span data-stu-id="748cf-135">All Stream Analytics jobs (and all other Azure resources) in that same region share this storage account toostore monitoring data.</span></span> <span data-ttu-id="748cf-136">Egy másik tárolási fiókot ad meg, ha nem kívánt hatásai megakadályozhatja a hello a más Stream Analytics-feladatok vagy más Azure-erőforrások figyelése.</span><span class="sxs-lookup"><span data-stu-id="748cf-136">If you provide a different storage account, it might cause unintended side effects in hello monitoring of your other Stream Analytics jobs or other Azure resources.</span></span>
> 
> <span data-ttu-id="748cf-137">hello tárfióknév tooreplace használt `<YOUR STORAGE ACCOUNT NAME>` hello a következő kódot kell egy tárfiókot, amely hello hello Stream Analytics-feladat, amely engedélyezi a figyelést tárolóként ugyanazt az előfizetést.</span><span class="sxs-lookup"><span data-stu-id="748cf-137">hello storage account name that you use tooreplace `<YOUR STORAGE ACCOUNT NAME>` in hello following code should be a storage account that is in hello same subscription as hello Stream Analytics job that you are enabling monitoring for.</span></span>
> 
> 

    // Get an existing Stream Analytics job
    JobGetParameters jobGetParameters = new JobGetParameters()
    {
        PropertiesToExpand = "inputs,transformation,outputs"
    };
    JobGetResponse jobGetResponse = streamAnalyticsClient.StreamingJobs.Get(resourceGroupName, streamAnalyticsJobName, jobGetParameters);

    // Enable monitoring
    ServiceDiagnosticSettingsPutParameters insightPutParameters = new ServiceDiagnosticSettingsPutParameters()
    {
            Properties = new ServiceDiagnosticSettings()
            {
                StorageAccountName = "<YOUR STORAGE ACCOUNT NAME>"
            }
    };
    insightsClient.ServiceDiagnosticSettingsOperations.Put(jobGetResponse.Job.Id, insightPutParameters);



## <a name="get-support"></a><span data-ttu-id="748cf-138">Támogatás kérése</span><span class="sxs-lookup"><span data-stu-id="748cf-138">Get support</span></span>

<span data-ttu-id="748cf-139">Ha további segítségre van szüksége, próbálkozzon a [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="748cf-139">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>

## <a name="next-steps"></a><span data-ttu-id="748cf-140">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="748cf-140">Next steps</span></span>

* [<span data-ttu-id="748cf-141">A Stream Analytics bemutatása tooAzure</span><span class="sxs-lookup"><span data-stu-id="748cf-141">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* <span data-ttu-id="748cf-142">[Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md) (Bevezetés az Azure Stream Analytics használatába)</span><span class="sxs-lookup"><span data-stu-id="748cf-142">[Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)</span></span>
* <span data-ttu-id="748cf-143">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)</span><span class="sxs-lookup"><span data-stu-id="748cf-143">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md)</span></span>
* <span data-ttu-id="748cf-144">[Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referencia az Azure Stream Analytics lekérdezési nyelvhez)</span><span class="sxs-lookup"><span data-stu-id="748cf-144">[Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)</span></span>
* [<span data-ttu-id="748cf-145">Az Azure Stream Analytics felügyeleti REST API referenciája</span><span class="sxs-lookup"><span data-stu-id="748cf-145">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

